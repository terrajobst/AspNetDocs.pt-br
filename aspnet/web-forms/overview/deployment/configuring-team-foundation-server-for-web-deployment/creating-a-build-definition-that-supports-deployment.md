---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Criando uma definição de compilação que dá suporte à implantação | Microsoft Docs
author: jrjlee
description: Se você quiser executar qualquer tipo de compilação no Team Foundation Server (TFS) 2010, você precisará criar uma definição de compilação dentro de seu projeto de equipe. Este tópico des...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: e11c91a824446572aaf0b3bc6954b9b8ffb4eaff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78603995"
---
# <a name="creating-a-build-definition-that-supports-deployment"></a>Criação de uma definição de build compatível com a implantação

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Se você quiser executar qualquer tipo de compilação no Team Foundation Server (TFS) 2010, você precisará criar uma definição de compilação dentro de seu projeto de equipe. Este tópico descreve como criar uma nova definição de compilação no TFS e como controlar a implantação da Web como parte do processo de compilação no Team Build.

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no coração desses tutoriais é baseado na abordagem do arquivo de projeto dividido descrito em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), no qual o processo de compilação e implantação é controlado&#x2014;por dois arquivos de projeto, um contendo instruções de Build que se aplicam a todos os ambientes de destino e um contendo configurações específicas de ambiente e de implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto independente do ambiente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Uma definição de compilação é o mecanismo que controla como e quando as compilações ocorrem para projetos de equipe no TFS. Cada definição de compilação especifica:

- As coisas que você deseja criar, como arquivos de solução do Visual Studio ou arquivos de projeto do Microsoft Build Engine (MSBuild) personalizados.
- Os critérios que determinam quando uma compilação deve ocorrer, como gatilhos manuais, CI (integração contínua) ou check-ins Restritos.
- O local para o qual o Team Build deve enviar saídas de compilação, incluindo artefatos de implantação como pacotes da Web e scripts de banco de dados.
- A quantidade de tempo que cada compilação deve ser mantida.
- Vários outros parâmetros do processo de compilação.

> [!NOTE]
> Para obter mais informações sobre definições de compilação, consulte [definir o processo de compilação](https://msdn.microsoft.com/library/ms181715.aspx).

Este tópico mostrará como criar uma definição de compilação que usa CI, para que uma compilação seja disparada quando um desenvolvedor fizer o check-in de um novo conteúdo. Se a compilação for realizada com sucesso, o serviço de compilação executará um arquivo de projeto personalizado para implantar a solução em um ambiente de teste.

Quando você dispara uma compilação, essas ações precisam acontecer:

- Primeiro, o Team Build deve criar a solução. Como parte desse processo, o Team Build invocará o WPP (pipeline de publicação na Web) para gerar pacotes de implantação da Web para cada um dos projetos de aplicativo Web na solução. O Team Build também executará qualquer teste de unidade associado à solução.
- Se o Build da solução falhar, o Team Build não deverá executar nenhuma ação adicional. As falhas de teste de unidade devem ser tratadas como uma falha de compilação.
- Se a compilação da solução for realizada com sucesso, o Team Build deverá executar o arquivo de projeto personalizado que controla a implantação da solução. Como parte desse processo, o Team Build invocará a Implantação da Web (ferramenta de implantação da Web) do Serviços de Informações da Internet (IIS) para instalar os aplicativos Web empacotados nos servidores Web de destino e invocará o utilitário VSDBCMD. exe para executar a criação do banco de dados scripts nos servidores de banco de dados de destino.

Isso ilustra o processo:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

A solução de exemplo [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) inclui um arquivo de projeto MSBuild personalizado, *Publish. proj*, que pode ser executado do MSBuild ou do Team Build. Conforme descrito em [noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md), esse arquivo de projeto define a lógica que implanta seus pacotes da Web e bancos de dados em um ambiente de destino. O arquivo inclui lógica que omite o processo de criação e empacotamento se estiver em execução no Team Build, deixando apenas as tarefas de implantação a serem executadas. Isso ocorre porque, ao automatizar a implantação dessa forma, você normalmente desejará garantir que a solução seja compilada com êxito e passe quaisquer testes de unidade antes que o processo de implantação seja iniciado.

A próxima seção explica como implementar esse processo criando uma nova definição de compilação.

> [!NOTE]
> Esse procedimento&#x2014;em que um único processo automatizado cria, testa e implanta uma solução&#x2014;é provavelmente mais adequado para a implantação em ambientes de teste. Para ambientes de preparo e produção, é muito mais provável que você queira implantar o conteúdo de uma compilação anterior que você já verificou e validou em um ambiente de teste. Essa abordagem é descrita no próximo tópico, [implantando uma compilação específica](deploying-a-specific-build.md).

### <a name="who-performs-this-procedure"></a>Quem executa este procedimento?

Normalmente, um administrador do TFS executa esse procedimento. Em alguns casos, um líder de equipe de desenvolvedores pode assumir a responsabilidade pela coleção de projetos de equipe no TFS. Para criar uma nova definição de compilação, você precisa ser um membro do grupo de **Administradores de compilação da coleção do projeto** para a coleção de projetos de equipe que contém sua solução.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Criar uma definição de compilação para CI e implantação

O procedimento a seguir descreve como criar uma definição de compilação que o CI dispara. Se a compilação for realizada com sucesso, a solução será implantada usando a lógica em um arquivo de projeto do MSBuild personalizado.

**Para criar uma definição de compilação para CI e implantação**

1. No Visual Studio 2010, na janela **Team Explorer** , expanda o nó do projeto de equipe, clique com o botão direito do mouse em **Builds**e clique em **nova definição de compilação**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Na guia **geral** , dê um nome à definição de compilação (por exemplo, **DeployToTest**) e uma descrição opcional.
3. Na guia **gatilho** , selecione os critérios nos quais você deseja disparar uma nova compilação. Por exemplo, se você quiser criar a solução e implantar no ambiente de teste sempre que um desenvolvedor fizer o check-in de um novo código, selecione **integração contínua**.
4. Na guia **padrões de compilação** , na caixa **copiar saída da compilação para a seguinte pasta de destino** , digite o caminho UNC (Convenção de nomenclatura universal) de sua pasta de destino (por exemplo, **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Esse local de destino armazena várias compilações, dependendo da política de retenção que você configurar. Quando você deseja publicar os artefatos de implantação de uma compilação específica em um ambiente de preparo ou de produção, é aqui que você os encontrará.
5. Na guia **processo** , na lista suspensa de **arquivos do processo de compilação** , deixe **DefaultTemplate. XAML** selecionado. Este é um dos modelos de processo de compilação padrão que são adicionados a todos os novos projetos de equipe.
6. Na tabela **parâmetros do processo de compilação** , clique na linha **itens a serem criados** e, em seguida, clique no botão de **reticências** .

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. Na caixa de diálogo **itens a serem criados** , clique em **Adicionar**.
8. Navegue até o local do arquivo de solução e clique em **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. Na caixa de diálogo **itens a serem criados** , clique em **Adicionar**.
10. Na lista suspensa **itens do tipo** , selecione **arquivos de projeto do MSBuild**.
11. Navegue até o local do arquivo de projeto personalizado com o qual você controla o processo de implantação, selecione o arquivo e clique em **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. A caixa de diálogo **itens a serem criados** agora deve mostrar dois itens. Clique em **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. Na guia **processo** , na tabela **parâmetros do processo de compilação** , expanda a seção **avançado** .
14. Na linha de **argumentos do MSBuild** , adicione quaisquer argumentos de linha de comando do MSBuild que os itens a serem compilados precisem. No cenário de solução do Gerenciador de contatos, esses argumentos são necessários:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. Neste exemplo:

    1. Os argumentos **DeployOnBuild = true** e **DeployTarget = Package** são necessários quando você cria a solução Contact Manager. Isso instrui o MSBuild a criar pacotes de implantação na Web após a criação de cada projeto de aplicativo Web, conforme descrito em [criando e empacotando projetos de aplicativos Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. O argumento **TargetEnvPropsFile** é necessário quando você cria o arquivo *Publish. proj* . Essa propriedade indica o local do arquivo de configuração específico do ambiente, conforme descrito em [noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. Na guia **política de retenção** , configure quantas compilações de cada tipo você deseja manter conforme necessário.
17. Clique em **Save** (Salvar).

## <a name="queue-a-build"></a>Enfileirar uma compilação

Neste ponto, você criou pelo menos uma nova definição de compilação. O processo de compilação que você definiu agora será executado de acordo com os gatilhos especificados na definição de compilação.

Se você tiver configurado sua definição de compilação para usar CI, você pode testar sua definição de compilação de duas maneiras:

- Faça check-in de algum conteúdo para o projeto de equipe para disparar uma compilação automática.
- Enfileirar uma compilação manualmente.

**Para enfileirar uma compilação manualmente**

1. Na janela **Team Explorer** , clique com o botão direito do mouse na definição da compilação e clique em **enfileirar nova compilação**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. Na caixa de diálogo **Build de fila** , examine as propriedades de compilação e, em seguida, clique em **fila**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Para examinar o progresso e o resultado de uma compilação&#x2014;, independentemente de ela ter sido disparada&#x2014;manualmente ou automaticamente, clique duas vezes na definição da compilação na janela **Team Explorer** . Isso abrirá uma guia **Build Explorer** .

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

A partir daqui, você pode solucionar problemas de compilações com falha. Se você clicar duas vezes em uma compilação individual, poderá exibir informações de resumo e clicar em arquivos de log detalhados.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Você pode usar essas informações para solucionar problemas de compilações com falha e solucionar qualquer problema antes de tentar outra compilação.

> [!NOTE]
> Compilações que executam a lógica de implantação provavelmente falharão até que você tenha concedido ao servidor de compilação todas as permissões necessárias no ambiente de destino. Para obter mais informações, consulte [Configurando permissões para implantação do Team Build](configuring-permissions-for-team-build-deployment.md).

## <a name="monitor-the-build-process"></a>Monitorar o processo de compilação

O TFS fornece uma ampla gama de funcionalidades para ajudá-lo a monitorar o processo de compilação. Por exemplo, o TFS pode enviar um email ou exibir alertas na área de notificação da barra de tarefas quando uma compilação for concluída. Para obter mais informações, consulte [executar e monitorar compilações](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como criar uma definição de compilação no TFS. A definição de compilação é configurada para CI, portanto, o processo de compilação é executado sempre que um desenvolvedor faz o check-in do conteúdo para o projeto de equipe. A definição de compilação executa um arquivo de projeto do MSBuild personalizado para implantar pacotes da Web e scripts de banco de dados em um ambiente de servidor de destino.

Para que uma implantação automatizada tenha sucesso como parte de um processo de compilação, você precisará conceder as permissões apropriadas para a conta de serviço de compilação nos servidores Web de destino e no servidor de banco de dados de destino. O tópico final deste tutorial, [Configurando permissões para a implantação do Team Build](configuring-permissions-for-team-build-deployment.md), descreve como identificar e configurar as permissões necessárias para a implantação automatizada de um servidor do Team Build.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como criar definições de compilação, consulte [criar uma definição de compilação básica](https://msdn.microsoft.com/library/ms181716.aspx) e [definir o processo de compilação](https://msdn.microsoft.com/library/ms181715.aspx). Para obter mais diretrizes sobre as compilações de enfileiramento, consulte enfileirar [uma compilação](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Anterior](configuring-a-tfs-build-server-for-web-deployment.md)
> [Próximo](deploying-a-specific-build.md)
