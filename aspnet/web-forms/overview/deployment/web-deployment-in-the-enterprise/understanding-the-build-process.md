---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Compreendendo o processo de compilação | Microsoft Docs
author: jrjlee
description: Este tópico fornece uma explicação sobre um processo de compilação e implantação em escala empresarial. A abordagem descrita neste tópico usa o Microsoft Build Engin personalizado...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 802d93f7ca987d018967275bae68b8c56d883a25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641137"
---
# <a name="understanding-the-build-process"></a>Noções básicas sobre o processo de build

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico fornece uma explicação sobre um processo de compilação e implantação em escala empresarial. A abordagem descrita neste tópico usa arquivos de projeto do Microsoft Build Engine (MSBuild) personalizados para fornecer controle refinado sobre cada aspecto do processo. Nos arquivos de projeto, os destinos do MSBuild personalizados são usados para executar utilitários de implantação como a ferramenta de implantação da Web do Serviços de Informações da Internet (IIS) (MSDeploy. exe) e o utilitário de implantação de banco de dados VSDBCMD. exe.
> 
> > [!NOTE]
> > O tópico anterior, [compreendendo o arquivo de projeto](understanding-the-project-file.md), descreveu os principais componentes de um arquivo de projeto do MSBuild e introduziu o conceito de arquivos de projeto divididos para dar suporte à implantação em vários ambientes de destino. Se você ainda não estiver familiarizado com esses conceitos, examine [noções básicas sobre o arquivo de projeto](understanding-the-project-file.md) antes de trabalhar neste tópico.

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no coração desses tutoriais baseia-se na abordagem de arquivo de projeto dividido descrita em [noções básicas sobre o arquivo de projeto](understanding-the-project-file.md), no qual o processo de compilação é&#x2014;controlado por dois arquivos de projeto, um contendo instruções de Build que se aplicam a todos os ambientes de destino e um contendo configurações específicas de ambiente e de implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto independente do ambiente para formar um conjunto completo de instruções de compilação.

## <a name="build-and-deployment-overview"></a>Visão geral de Compilação e Implantação

Na [solução Contact Manager](the-contact-manager-solution.md), três arquivos controlam o processo de compilação e implantação:

- Um *arquivo de projeto universal* (*Publish. proj*). Isso contém instruções de compilação e implantação que não são alteradas entre os ambientes de destino.
- Um *arquivo de projeto específico do ambiente* (*env-dev. proj*). Isso contém as configurações de compilação e implantação específicas para um determinado ambiente de destino. Por exemplo, você pode usar o arquivo *env-dev. proj* para fornecer configurações para um ambiente de desenvolvedor ou de teste e criar um arquivo alternativo chamado *env-Stage. proj* para fornecer configurações para um ambiente de preparo.
- Um *arquivo de comando* (*Publish-dev. cmd*). Isso contém um MSBuild.exe comando que especifica qual projeto arquivos que você deseja executar. Você pode criar um arquivo de comando para cada ambiente de destino, onde cada arquivo contém um comando MSBuild. exe que especifica um arquivo de projeto específico do ambiente diferente. Isso permite que o desenvolvedor implante em ambientes diferentes simplesmente executando o arquivo de comando apropriado.

Na solução de exemplo, você pode encontrar esses três arquivos na pasta publicar solução.

![](understanding-the-build-process/_static/image1.png)

Antes de examinar esses arquivos mais detalhadamente, vamos dar uma olhada em como o processo de compilação geral funciona quando você usa essa abordagem. Em um alto nível, o processo de compilação e implantação tem esta aparência:

![](understanding-the-build-process/_static/image2.png)

A primeira coisa que acontece é que os dois arquivos&#x2014;de projeto que contêm instruções de compilação e implantação universal e um contendo configurações&#x2014;específicas do ambiente são mesclados em um único arquivo de projeto. O MSBuild funciona por meio das instruções no arquivo de projeto. Ele cria cada um dos projetos na solução, usando o arquivo de projeto para cada projeto. Em seguida, ele chama outras ferramentas, como Implantação da Web (MSDeploy. exe) e o utilitário VSDBCMD para implantar seu conteúdo da Web e bancos de dados no ambiente de destino.

Do início ao fim, o processo de compilação e implantação executa estas tarefas:

1. Ele exclui o conteúdo do diretório de saída, em preparação para um Build novo.
2. Ele cria cada projeto na solução:

    1. Para projetos&#x2014;Web neste caso, um aplicativo Web ASP.NET MVC e um serviço&#x2014;Web WCF o processo de compilação cria um pacote de implantação da Web para cada projeto.
    2. Para projetos de banco de dados, o processo de compilação cria um manifesto de implantação (arquivo. DeployManifest) para cada projeto.
3. Ele usa o utilitário VSDBCMD. exe para implantar cada projeto de banco de dados na solução, usando várias propriedades dos arquivos&#x2014;de projeto uma cadeia de conexão de destino&#x2014;e um nome de banco de dados junto com o arquivo. DeployManifest.
4. Ele usa o utilitário MSDeploy. exe para implantar cada projeto da Web na solução, usando várias propriedades dos arquivos de projeto para controlar o processo de implantação.

Você pode usar a solução de exemplo para rastrear esse processo mais detalhadamente.

> [!NOTE]
> Para obter orientação sobre como personalizar os arquivos de projeto específicos do ambiente para seus próprios ambientes de servidor, consulte [Configurar Propriedades de implantação para um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="invoking-the-build-and-deployment-process"></a>Invocando o processo de Compilação e Implantação

Para implantar a solução do Gerenciador de contatos em um ambiente de teste do desenvolvedor, o desenvolvedor executa o arquivo de comando *Publish-dev. cmd* . Isso invoca o MSBuild. exe, especificando *Publish. proj* como o arquivo de projeto a ser executado e *env-dev. proj* como um valor de parâmetro.

[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]

> [!NOTE]
> A opção **/fl** (short for **/FileLogger**) registra a saída da compilação em um arquivo chamado *MSBuild. log* no diretório atual. Para obter mais informações, consulte a [referência de linha de comando do MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

Neste ponto, o MSBuild começa a ser executado, carrega o arquivo *Publish. proj* e começa a processar as instruções dentro dele. A primeira instrução informa ao MSBuild para importar o arquivo de projeto que o parâmetro **TargetEnvPropsFile** especifica.

[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]

O parâmetro **TargetEnvPropsFile** especifica o arquivo *env-dev. proj* , portanto, o MSBuild mescla o conteúdo do arquivo *env-dev. proj* no arquivo *Publish. proj* .

Os próximos elementos que o MSBuild encontra no arquivo de projeto mesclado são grupos de propriedades. As propriedades são processadas na ordem em que aparecem no arquivo. O MSBuild cria um par chave-valor para cada propriedade, fornecendo que todas as condições especificadas sejam atendidas. As propriedades definidas posteriormente no arquivo substituirão todas as propriedades com o mesmo nome definido anteriormente no arquivo. Por exemplo, considere as propriedades **OutputRoot** .

[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]

Quando o MSBuild processa o primeiro elemento **OutputRoot** , fornecer um parâmetro nomeado de forma semelhante não foi fornecido, ele define o valor da propriedade **OutputRoot** como **.. \Publish\Out**. Quando encontrar o segundo elemento **OutputRoot** , se a condição for avaliada como **true**, ela substituirá o valor da propriedade **OutputRoot** pelo valor do parâmetro **OutDir** .

O próximo elemento que o MSBuild encontra é um único grupo de itens, contendo um item chamado **ProjectsToBuild**.

[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]

O MSBuild processa essa instrução criando uma lista de itens chamada **ProjectsToBuild**. Nesse caso, a lista de itens contém um único valor&#x2014;o caminho e o nome do arquivo da solução.

Neste ponto, os elementos restantes são destinos. Os destinos são processados de forma diferente&#x2014;das propriedades e dos itens, essencialmente, os destinos não são processados, a menos que sejam explicitamente especificados pelo usuário ou invocados por outra construção dentro do arquivo de projeto. Lembre-se de que a marca de abertura do **projeto** inclui um atributo **DefaultTargets** .

[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]

Isso instrui o MSBuild a invocar o destino **FullPublish** , se os destinos não forem especificados quando o MSBuild. exe for invocado. O destino **FullPublish** não contém nenhuma tarefa; em vez disso, ele simplesmente especifica uma lista de dependências.

[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]

Essa dependência informa ao MSBuild que, para executar o destino **FullPublish** , ele precisa invocar essa lista de destinos na ordem fornecida:

1. Ele deve invocar o destino **limpo** .
2. Ele deve invocar o destino **BuildProjects** .
3. Ele deve invocar o destino **GatherPackagesForPublishing** .
4. Ele deve invocar o destino **PublishDbPackages** .
5. Ele deve invocar o destino **PublishWebPackages** .

### <a name="the-clean-target"></a>O destino limpo

O destino **limpo** basicamente exclui o diretório de saída e todo o seu conteúdo, como preparação para uma nova compilação.

[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]

Observe que o destino inclui um elemento do **rowgroup** . Ao definir propriedades ou itens dentro de um elemento de **destino** , você está criando Propriedades e itens *dinâmicos* . Em outras palavras, as propriedades ou os itens não são processados até que o destino seja executado. O diretório de saída pode não existir ou conter arquivos até que o processo de compilação comece, portanto, você não pode criar a lista de **\_FilesToDelete** como um item estático; Você precisa aguardar até que a execução esteja em andamento. Dessa forma, você cria a lista como um item dinâmico dentro do destino.

> [!NOTE]
> Nesse caso, como o destino **limpo** é o primeiro a ser executado, não há necessidade real de usar um grupo de itens dinâmicos. No entanto, é uma boa prática usar propriedades dinâmicas e itens nesse tipo de cenário, pois talvez você queira executar destinos em uma ordem diferente em algum ponto.  
> Você também deve ter como objetivo evitar a declaração de itens que nunca serão usados. Se você tiver itens que serão usados apenas por um destino específico, considere colocá-los dentro do destino para remover qualquer sobrecarga desnecessária no processo de compilação.

Além dos itens dinâmicos, o destino **limpo** é bem simples e usa as tarefas **internas,** **delete**e **RemoveDir** para:

1. Envie uma mensagem para o agente de log.
2. Crie uma lista de arquivos a serem excluídos.
3. Exclua os arquivos.
4. Remova o diretório de saída.

### <a name="the-buildprojects-target"></a>O destino BuildProjects

O destino **BuildProjects** basicamente compila todos os projetos na solução de exemplo.

[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]

Esse destino foi descrito em alguns detalhes no tópico anterior, [compreendendo o arquivo de projeto](understanding-the-project-file.md), para ilustrar como as tarefas e os destinos referenciam as propriedades e os itens. Neste ponto, você está principalmente interessado na tarefa do **MSBuild** . Você pode usar essa tarefa para criar vários projetos. A tarefa não cria uma nova instância do MSBuild. exe; Ele usa a instância em execução atual para compilar cada projeto. Os principais pontos de interesse neste exemplo são as propriedades de implantação:

- A propriedade **DeployOnBuild** instrui o MSBuild a executar qualquer instrução de implantação nas configurações do projeto quando a compilação de cada projeto for concluída.
- A propriedade **DeployTarget** identifica o destino que você deseja invocar depois que o projeto é compilado. Nesse caso, o destino do **pacote** cria a saída do projeto em um pacote da Web implantável.

> [!NOTE]
> O destino do **pacote** invoca o WPP (pipeline de publicação na Web), que fornece integração entre o MSBuild e o implantação da Web. Se você quiser dar uma olhada nos destinos internos que o WPP fornece, examine o arquivo *Microsoft. Web. Publishing. targets* na pasta% ProgramFiles (x86)% \ MSBuild\Microsoft\VisualStudio\v10.0\Web

### <a name="the-gatherpackagesforpublishing-target"></a>O destino GatherPackagesForPublishing

Se você estudar o destino **GatherPackagesForPublishing** , observará que ele não contém realmente nenhuma tarefa. Em vez disso, ele contém um único grupo de itens que define três itens dinâmicos.

[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]

Esses itens se referem aos pacotes de implantação que foram criados quando o destino **BuildProjects** foi executado. Não foi possível definir esses itens estaticamente no arquivo de projeto, porque os arquivos aos quais os itens se referem não existem até que o destino **BuildProjects** seja executado. Em vez disso, os itens devem ser definidos dinamicamente dentro de um destino que não é invocado até que o destino **BuildProjects** seja executado.

Os itens não são usados nesse destino&#x2014;. esse destino simplesmente cria os itens e os metadados associados a cada valor de item. Depois que esses elementos são processados, o item **PublishPackages** conterá dois valores, o caminho para o arquivo *ContactManager. Mvc. Deploy. cmd* e o caminho para o arquivo *ContactManager. Service. Deploy. cmd* . Implantação da Web cria esses arquivos como parte do pacote da Web para cada projeto, e esses são os arquivos que você deve invocar no servidor de destino para implantar os pacotes. Se você abrir um desses arquivos, basicamente verá um comando MSDeploy. exe com vários valores de parâmetro específicos da compilação.

O item **DbPublishPackages** conterá um único valor, o caminho para o arquivo *ContactManager. Database. DeployManifest* .

> [!NOTE]
> Um arquivo. DeployManifest é gerado quando você cria um projeto de banco de dados e usa o mesmo esquema de um arquivo de projeto do MSBuild. Ele contém todas as informações necessárias para implantar um banco de dados, incluindo o local do esquema de banco de dados (. dbschema) e detalhes de qualquer script de pré-implantação e pós-implantação. Para obter mais informações, consulte [uma visão geral do compilação e implantação de banco de dados](https://msdn.microsoft.com/library/aa833165.aspx).

Você aprenderá mais sobre como pacotes de implantação e manifestos de implantação de banco de dados são criados e usados na [criação e no empacotamento de projetos de aplicativos Web](building-and-packaging-web-application-projects.md) e na [implantação de projetos de banco de dados](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>O destino PublishDbPackages

Falando brevemente, o destino **PublishDbPackages** invoca o utilitário VSDBCMD para implantar o banco de dados **ContactManager** em um ambiente de destino. Configurar a implantação de banco de dados envolve muitas decisões e nuances, e você aprenderá mais sobre isso na [implantação de projetos de banco de dados](deploying-database-projects.md) e na [personalização de implantações de banco de dados para vários ambientes](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Neste tópico, vamos nos concentrar em como esse destino realmente funciona.

Primeiro, observe que a marca de abertura inclui um atributo **Outputs** .

[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]

Este é um exemplo de *envio em lote de destino*. Em arquivos de projeto MSBuild, o envio em lote é uma técnica para iteração em coleções. O valor do atributo **Outputs** , **"% (DbPublishPackages. Identity)"** , refere-se à propriedade Metadata de **identidade** da lista de itens **DbPublishPackages** . Essa notação, **Outputs =% * * * (lista de @ Metadata)* , é convertida como:

- Divida os itens em **DbPublishPackages** em lotes de itens que contêm o mesmo valor de metadados de **identidade** .
- Execute o destino uma vez por lote.

> [!NOTE]
> **Identity** é um dos [valores de metadados internos](https://msdn.microsoft.com/library/ms164313.aspx) que é atribuído a cada item na criação. Ele se refere ao valor do atributo **include** no elemento&#x2014; **Item** em outras palavras, o caminho e o nome do arquivo do item.

Nesse caso, como nunca deve haver mais de um item com o mesmo caminho e nome de arquivo, estamos basicamente trabalhando com tamanhos de lote de um. O destino é executado uma vez para cada pacote de banco de dados.

Você pode ver uma notação semelhante na propriedade **\_cmd** , que cria um comando VSDBCMD com as opções apropriadas.

[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]

Nesse caso, **% (DbPublishPackages. DatabaseConnectionString)** , **% (DbPublishPackages. TargetDatabase)** e **% (DbPublishPackages. FullPath)** se referem aos valores de metadados da coleção de itens de **DbPublishPackages** . A propriedade **\_cmd** é usada pela tarefa **exec** , que invoca o comando.

[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]

Como resultado dessa notação, a tarefa **exec** criará lotes com base em combinações exclusivas dos valores de metadados **DatabaseConnectionString**, **TargetDatabase**e **FullPath** , e a tarefa será executada uma vez para cada lote. Este é um exemplo de *envio em lote de tarefas*. No entanto, como o envio em lote em nível de destino já dividiu nossa coleção de itens em lotes de item único, a tarefa **exec** será executada uma vez e apenas uma vez para cada iteração do destino. Em outras palavras, essa tarefa invoca o utilitário VSDBCMD uma vez para cada pacote de banco de dados na solução.

> [!NOTE]
> Para obter mais informações sobre o envio em lote de tarefas e destino, consulte [envio em lote](https://msdn.microsoft.com/library/ms171473.aspx)do MSBuild, [metadados de item no lote de destino](https://msdn.microsoft.com/library/ms228229.aspx)e [metadados de item no envio em lote de tarefas](https://msdn.microsoft.com/library/ms171474.aspx).

### <a name="the-publishwebpackages-target"></a>O destino PublishWebPackages

Nesse ponto, você chamou o destino **BuildProjects** , que gera um pacote de implantação da Web para cada projeto na solução de exemplo. O acompanhamento de cada pacote é um arquivo *Deploy. cmd* , que contém os comandos MSDeploy. exe necessários para implantar o pacote no ambiente de destino e um arquivo Parameters *. xml* , que especifica os detalhes necessários do ambiente de destino. Você também chamou o destino **GatherPackagesForPublishing** , que gera uma coleção de itens que contém os arquivos *Deploy. cmd* nos quais você está interessado. Essencialmente, o destino **PublishWebPackages** executa essas funções:

- Ele manipula o arquivo *Parameters. xml* para cada pacote para incluir os detalhes corretos para o ambiente de destino, **usando a tarefa** xmlfazer.
- Ele invoca o arquivo *Deploy. cmd* para cada pacote, usando as opções apropriadas.

Assim como o destino **PublishDbPackages** , o destino **PublishWebPackages** usa o envio em lote de destino para garantir que o destino seja executado uma vez para cada pacote da Web.

[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]

Dentro do destino, a tarefa **exec** é usada para executar o arquivo *Deploy. cmd* para cada pacote da Web.

[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]

Para obter mais informações sobre como configurar a implantação de pacotes da Web, consulte [criando e empacotando projetos de aplicativos Web](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Conclusão

Este tópico forneceu uma explicação detalhada de como os arquivos de projeto divididos são usados para controlar o processo de compilação e implantação do início ao fim da solução de exemplo do Gerenciador de contatos. O uso dessa abordagem permite executar implantações complexas e de escala empresarial em uma única etapa reproduzível, simplesmente executando um arquivo de comando específico do ambiente.

## <a name="further-reading"></a>Leitura adicional

Para obter uma introdução mais detalhada aos arquivos de projeto e ao WPP, consulte [dentro do Microsoft Build Engine: usando o MSBuild e o Team Foundation Build](http://amzn.com/0735645248) de Sayed Ibrahim Hashimi e William BARTHOLOMEW, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Anterior](understanding-the-project-file.md)
> [Próximo](building-and-packaging-web-application-projects.md)
