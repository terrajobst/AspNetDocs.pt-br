---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Colocando aplicativos Web offline com o Implantação da Web | Microsoft Docs
author: jrjlee
description: Este tópico descreve como colocar um aplicativo Web offline pela duração de uma implantação automatizada usando o Serviços de Informações da Internet (IIS) Web alerta...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: ba60664a0c3daa0650cd7e7cfc4ab9da08df3440
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526064"
---
# <a name="taking-web-applications-offline-with-web-deploy"></a>Colocar aplicativos Web em offline com a Implantação da Web

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como colocar um aplicativo Web offline pela duração de uma implantação automatizada usando a ferramenta de implantação da Web do Serviços de Informações da Internet (IIS) (Implantação da Web). Os usuários que navegam para o aplicativo Web são redirecionados para um *aplicativo\_arquivo offline. htm* até que a implantação seja concluída.

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no coração desses tutoriais baseia-se na abordagem de arquivo de projeto dividido descrita em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), no qual o processo de compilação é&#x2014;controlado por dois arquivos de projeto, um contendo instruções de Build que se aplicam a todos os ambientes de destino e um contendo configurações específicas de ambiente e de implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto independente do ambiente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Em muitos cenários, você desejará colocar um aplicativo Web offline enquanto fizer alterações em componentes relacionados, como bancos de dados ou serviços Web. Normalmente, no IIS e no ASP.NET, você realiza isso colocando um arquivo chamado *App\_offline. htm* na pasta raiz do site do IIS ou do aplicativo Web. O *aplicativo\_offline. htm* é um arquivo HTML padrão e geralmente conterá uma mensagem simples avisando o usuário de que o site está temporariamente indisponível devido à manutenção. Embora o *aplicativo\_o arquivo offline. htm* exista na pasta raiz do site, o IIS redirecionará automaticamente todas as solicitações para o arquivo. Quando terminar de fazer atualizações, você removerá o arquivo *\_offline. htm do aplicativo* e o site continuará atendendo as solicitações como de costume.

Ao usar o Implantação da Web para executar implantações automatizadas ou de uma única etapa em um ambiente de destino, convém incorporar a adição e remoção do arquivo *. htm do aplicativo\_offline* em seu processo de implantação. Para fazer isso, você precisará concluir estas tarefas de alto nível:

- No arquivo de projeto do Microsoft Build Engine (MSBuild) que você usa para controlar o processo de implantação, crie um destino do MSBuild que copia um arquivo do *aplicativo\_offline. htm* para o servidor de destino antes de qualquer tarefa de implantação começar.
- Adicione outro destino do MSBuild que remova o arquivo *\_offline. htm do aplicativo* do servidor de destino quando todas as tarefas de implantação forem concluídas.
- No projeto de aplicativo Web, crie um arquivo *. WPP. targets* que garante que um *aplicativo\_arquivo offline. htm* seja adicionado ao pacote de implantação quando implantação da Web for invocado.

Este tópico mostrará como executar esses procedimentos. As tarefas e orientações neste tópico pressupõem que você já criou uma solução que contém pelo menos um projeto de aplicativo Web e que usa um arquivo de projeto personalizado para controlar o processo de implantação, conforme descrito em [implantação da Web na empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Como alternativa, você pode usar a solução de exemplo [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) para seguir os exemplos no tópico.

## <a name="adding-an-app_offline-file-to-a-web-application-project"></a>Adicionando um aplicativo\_arquivo offline a um projeto de aplicativo Web

A primeira tarefa que você precisa concluir é adicionar um *aplicativo\_arquivo offline* ao seu projeto de aplicativo Web:

- Para impedir que o arquivo interfira no processo de desenvolvimento (você não quer que seu aplicativo fique permanentemente offline), você deve chamá-lo de outra forma que não seja o *aplicativo\_offline. htm*. Por exemplo, você pode nomear o *aplicativo de arquivo\_offline-template. htm*.
- Para impedir que o arquivo seja implantado no estado em que se encontra, você deve definir a ação de Build como **None**.

**Para adicionar um aplicativo\_arquivo offline a um projeto de aplicativo Web**

1. Abra sua solução no Visual Studio 2010.
2. Na janela **Gerenciador de soluções** , clique com o botão direito do mouse no projeto de aplicativo Web, aponte para **Adicionar**e clique em **novo item**.
3. Na caixa de diálogo **Adicionar novo item** , selecione **página HTML**.
4. Na caixa **nome** , digite **app\_offline-template. htm**e clique em **Adicionar**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Adicione um HTML simples para informar os usuários de que o aplicativo está indisponível e, em seguida, salve o arquivo. Não inclua marcas do lado do servidor (por exemplo, quaisquer marcas que tenham o prefixo "ASP:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. Na janela **Gerenciador de soluções** , clique com o botão direito do mouse no novo arquivo e clique em **Propriedades**.
7. Na janela **Propriedades** , na linha **ação de compilação** , selecione **nenhum**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-app_offline-file"></a>Implantando e excluindo um aplicativo\_arquivo offline

A próxima etapa é modificar a lógica de implantação para copiar o arquivo para o servidor de destino no início do processo de implantação e removê-lo no final.

> [!NOTE]
> O próximo procedimento pressupõe que você esteja usando um arquivo de projeto MSBuild personalizado para controlar seu processo de implantação, conforme descrito em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Se estiver implantando o diretamente do Visual Studio, você precisará usar uma abordagem diferente. Sayed Ibrahim Hashimi descreve uma abordagem desse tipo de [como colocar seu aplicativo Web offline durante a publicação](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).

Para implantar um *aplicativo\_arquivo offline* em um site do IIS de destino, você precisa invocar o MSDeploy. exe usando o [provedor de implantação da Web **contentPath** ](https://technet.microsoft.com/library/dd569034(WS.10).aspx). O provedor **contentPath** dá suporte a caminhos de diretórios físicos e a caminhos de aplicativos ou sites do IIS, o que o torna a opção ideal para sincronizar um arquivo entre uma pasta de projeto do Visual Studio e um aplicativo Web do IIS. Para implantar o arquivo, o comando MSDeploy deve ser semelhante ao seguinte:

[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]

Para remover o arquivo do site de destino no final do processo de implantação, o comando MSDeploy deve ser semelhante ao seguinte:

[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]

Para automatizar esses comandos como parte de um processo de compilação e implantação, você precisa integrá-los em seu arquivo de projeto do MSBuild personalizado. O procedimento a seguir descreve como fazer isso.

**Para implantar e excluir um aplicativo\_arquivo offline**

1. No Visual Studio 2010, abra o arquivo de projeto do MSBuild que controla seu processo de implantação. Na solução de exemplo [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , esse é o arquivo *Publish. proj* .
2. No elemento do **projeto** raiz, crie um novo elemento **PropertyGroup** para armazenar variáveis para o *aplicativo\_implantação offline* :

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. A propriedade **SourceRoot** é definida em outro lugar no arquivo *Publish. proj* . Indica o local da pasta raiz para o conteúdo de origem relativo ao caminho&#x2014;atual em outras palavras, em relação ao local do arquivo *Publish. proj* .
4. O provedor **contentPath** não aceitará caminhos de arquivo relativos, portanto, você precisa obter um caminho absoluto para o arquivo de origem antes de implantá-lo. Você pode usar a tarefa [ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx) para fazer isso.
5. Adicione um novo elemento de **destino** chamado **GetAppOfflineAbsolutePath**. Nesse destino, use a tarefa **ConvertToAbsolutePath** para obter um caminho absoluto para o *aplicativo\_arquivo de modelo offline* na pasta do projeto.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Esse destino usa o caminho relativo para o *aplicativo\_arquivo de modelo offline* na pasta do projeto e salva-o em uma nova propriedade como um caminho de arquivo absoluto. O atributo **BeforeTargets** especifica que você deseja que esse destino seja executado antes do destino **DeployAppOffline** , que você criará na próxima etapa.
7. Adicione um novo destino chamado **DeployAppOffline**. Dentro desse destino, invoque o comando MSDeploy. exe que implanta seu *aplicativo\_arquivo offline* no servidor Web de destino.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. Neste exemplo, a propriedade **ContactManagerIisPath** está definida em outro lugar no arquivo de projeto. Esse é simplesmente um caminho de aplicativo do IIS, no formato *[nome do site do IIS]/[nome do aplicativo]* . A inclusão de uma condição no destino permite que os usuários alternem o *aplicativo\_implantação offline* seja ativada ou desativada alterando um valor de propriedade ou fornecendo um parâmetro de linha de comando.
9. Adicione um novo destino chamado **DeleteAppOffline**. Dentro desse destino, invoque o comando MSDeploy. exe que remove seu *aplicativo\_arquivo offline* do servidor Web de destino.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. A tarefa final é invocar esses novos destinos em pontos apropriados durante a execução do arquivo de projeto. Você pode fazer isso de várias maneiras. Por exemplo, no arquivo *Publish. proj* , a propriedade **FullPublishDependsOn** especifica uma lista de destinos que devem ser executados na ordem em que o destino padrão **FullPublish** é invocado.
11. Modifique o arquivo de projeto do MSBuild para invocar os destinos **DeployAppOffline** e **DeleteAppOffline** em pontos apropriados no processo de publicação.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Quando você executa o arquivo de projeto do MSBuild personalizado, o *aplicativo\_arquivo offline* será implantado no servidor imediatamente após uma compilação bem-sucedida. Ele será excluído do servidor depois que todas as tarefas de implantação forem concluídas.

## <a name="adding-an-app_offline-file-to-deployment-packages"></a>Adicionando um aplicativo\_arquivo offline a pacotes de implantação

Dependendo de como você configura sua implantação, qualquer conteúdo existente no aplicativo&#x2014;Web IIS de destino, como o *aplicativo\_offline. htm* ,&#x2014;poderá ser excluído automaticamente quando você implantar um pacote da Web no destino. Para garantir que o *aplicativo\_o arquivo offline. htm* permaneça em vigor durante a implantação, você precisa incluir o arquivo no próprio pacote de implantação da Web, além de implantar o arquivo diretamente no início do processo de implantação.

- Se você seguiu as tarefas anteriores neste tópico, você terá adicionado o arquivo *\_offline. htm* ao seu projeto de aplicativo Web em um nome de arquivo diferente (usamos o *aplicativo\_offline-template. htm*) e você terá definido a ação de compilação como **nenhum**. Essas alterações são necessárias para impedir que o arquivo interfira no desenvolvimento e na depuração. Como resultado, você precisa personalizar o processo de empacotamento para garantir que o *aplicativo\_arquivo offline. htm* seja incluído no pacote de implantação da Web.

O WPP (pipeline de publicação na Web) usa uma lista de itens chamada **FilesForPackagingFromProject** para criar uma lista de arquivos que devem ser incluídos no pacote de implantação da Web. Você pode personalizar o conteúdo de seus pacotes da Web adicionando seus próprios itens à lista. Para fazer isso, você precisa concluir estas etapas de alto nível:

1. Crie um arquivo de projeto personalizado chamado *[Project Name]. WPP. targets* na mesma pasta que o seu arquivo de projeto.

    > [!NOTE]
    > O arquivo *. WPP. targets* precisa ir para a mesma pasta do seu arquivo&#x2014;de projeto de aplicativo Web, por exemplo, *ContactManager. Mvc. csproj*&#x2014;, em vez de na mesma pasta que os arquivos de projeto personalizados que você usa para controlar o processo de compilação e implantação.
2. No arquivo *. WPP. targets* , crie um novo destino do MSBuild que seja executado *antes* do destino **CopyAllFilesToSingleFolderForPackage** . Esse é o destino do WPP que cria uma lista de itens a serem incluídos no pacote.
3. No novo destino, crie um elemento do **rowgroup** .
4. No elemento **rowgroup** , adicione um item **FilesForPackagingFromProject** e especifique o arquivo *\_offline. htm do aplicativo* .

O arquivo *. WPP. targets* deve ser semelhante ao seguinte:

[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]

Estes são os principais pontos de observação neste exemplo:

- O atributo **BeforeTargets** insere esse destino no WPP especificando que ele deve ser executado imediatamente antes do destino **CopyAllFilesToSingleFolderForPackage** .
- O item **FilesForPackagingFromProject** usa o valor de metadados **DestinationRelativePath** para renomear o arquivo do *aplicativo\_offline-template. htm* para o *aplicativo\_offline. htm* , pois ele é adicionado à lista.

O procedimento a seguir mostra como adicionar esse arquivo *. WPP. targets* a um projeto de aplicativo Web.

**Para adicionar um arquivo. WPP. targets a um pacote de implantação da Web**

1. Abra sua solução no Visual Studio 2010.
2. Na janela **Gerenciador de soluções** , clique com o botão direito do mouse no nó do projeto de aplicativo Web (por exemplo, **ContactManager. Mvc**), aponte para **Adicionar**e clique em **novo item**.
3. Na caixa de diálogo **Adicionar novo item** , selecione o modelo de **arquivo XML** .
4. Na caixa **nome** , digite *[nome do projeto] * * *. WPP. targets** (por exemplo, **ContactManager. Mvc. WPP. targets**) e clique em **Adicionar**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Se você adicionar um novo item ao nó raiz de um projeto, o arquivo será criado na mesma pasta que o arquivo de projeto. Você pode verificar isso abrindo a pasta no Windows Explorer.
5. No arquivo, adicione a marcação MSBuild descrita anteriormente.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Salve e feche o arquivo *[nome do projeto]. WPP. targets* .

Na próxima vez que você criar e empacotar seu projeto de aplicativo Web, o WPP detectará automaticamente o arquivo *. WPP. targets* . O *aplicativo\_arquivo offline-template. htm* será incluído no pacote de implantação da Web resultante como o *aplicativo\_offline. htm*.

> [!NOTE]
> Se a sua implantação falhar, o *aplicativo\_o arquivo offline. htm* permanecerá em vigor e seu aplicativo permanecerá offline. Normalmente, esse é o comportamento desejado. Para colocar seu aplicativo online novamente, você pode excluir o *aplicativo\_arquivo offline. htm* do seu servidor Web. Como alternativa, se você corrigir erros e executar uma implantação bem-sucedida, o arquivo *. htm do aplicativo\_offline* será removido.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como colocar um aplicativo Web offline durante a implantação, publicando um *aplicativo\_arquivo offline. htm* no servidor de destino no início do processo de implantação e removendo-o no final. Ele também abordou como incluir um *aplicativo\_arquivo offline. htm* em um pacote de implantação da Web.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre o processo de empacotamento e implantação, consulte [criando e empacotando projetos de aplicativos Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [configurando parâmetros para implantação de pacote da Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)e [implantando pacotes da Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Se você publicar seus aplicativos Web diretamente do Visual Studio, em vez de usar a abordagem de arquivo de projeto do MSBuild personalizada descrita nestes tutoriais, você precisará usar uma abordagem um pouco diferente para colocar o aplicativo offline durante a publicação Process.

> [!div class="step-by-step"]
> [Anterior](excluding-files-and-folders-from-deployment.md)
> [Próximo](running-windows-powershell-scripts-from-msbuild-project-files.md)
