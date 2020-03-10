---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Implantando pacotes da Web | Microsoft Docs
author: jrjlee
description: Este tópico descreve como você pode publicar pacotes de implantação da Web em um servidor remoto usando a ferramenta de implantação da Web do Serviços de Informações da Internet (IIS) (Web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 91b99e6e250342851aea6860164b6f6af54818d1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575946"
---
# <a name="deploying-web-packages"></a>Implantação de pacotes da Web

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como você pode publicar pacotes de implantação da Web em um servidor remoto usando a Implantação da Web (ferramenta de implantação da Web) do Serviços de Informações da Internet (IIS) 2,0.
> 
> Há duas maneiras principais em que você pode implantar um pacote da Web em um servidor remoto:
> 
> - Você pode usar o utilitário de linha de comando MSDeploy. exe diretamente.
> - Você pode executar o arquivo *[Project Name]. Deploy. cmd* que o processo de compilação gera.
> 
> O resultado final é o mesmo, independentemente da abordagem usada. Essencialmente, todo o arquivo *. Deploy. cmd* é executar o MSDeploy. exe com alguns valores predeterminados, para que você não precise fornecer tantas informações para implantar o pacote. Isso simplifica o processo de implantação. Por outro lado, o uso do MSDeploy. exe diretamente proporciona muito mais flexibilidade em relação à forma como o pacote é implantado.
> 
> A abordagem que você usar dependerá de uma variedade de fatores, incluindo a quantidade de controle de que você precisa sobre o processo de implantação e se você está direcionando o Implantação da Web serviço de agente remoto ou o manipulador de Implantação da Web. Este tópico explica como usar cada abordagem e identifica quando cada abordagem é apropriada.
> 
> As tarefas e as orientações neste tópico pressupõem que:
> 
> - Você criou e empacotau seu aplicativo Web, conforme descrito em [criando e empacotando projetos de aplicativos Web](building-and-packaging-web-application-projects.md).
> - Você modificou o arquivo *Parameters. xml* para fornecer os valores de parâmetro corretos para seu ambiente de destino, conforme descrito em [configurando parâmetros para implantação de pacote da Web](configuring-parameters-for-web-package-deployment.md).

Executar o arquivo [*Project Name*] *. Deploy. cmd* é a maneira mais simples de implantar um pacote da Web. Em particular, o uso do arquivo *. Deploy. cmd* oferece essas vantagens em relação ao uso do MSDeploy. exe diretamente:

- Você não precisa especificar o local do pacote&#x2014;de implantação da Web. o arquivo *. Deploy. cmd* já sabe onde ele está.
- Você não precisa especificar o local do arquivo&#x2014;Parameters *. xml* . o arquivo *. Deploy. cmd* já sabe onde ele está.
- Você não precisa especificar provedores&#x2014;de MSDeploy de origem e de destino. o arquivo *. Deploy. cmd* já sabe quais valores usar.
- Você não precisa especificar as configurações&#x2014;de operação de MSDeploy. o arquivo *. Deploy. cmd* adiciona os valores normalmente necessários ao comando MSDeploy. exe automaticamente.

Antes de usar o arquivo *. Deploy. cmd* para implantar um pacote da Web, você deve garantir que:

- O arquivo *. Deploy. cmd* , o [*nome do projeto*]. O arquivo *Parameters. xml* e o pacote da Web ([*nome do projeto*]. *zip*) estão na mesma pasta.
- Implantação da Web (MSDeploy. exe) é instalado no computador que executa o arquivo *. Deploy. cmd* .

O arquivo *. Deploy. cmd* dá suporte a várias opções de linha de comando. Quando você executa o arquivo em um prompt de comando, essa é a sintaxe básica:

[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]

Você deve especificar um sinalizador **/t** ou um sinalizador **/y** para indicar se deseja executar uma execução de avaliação ou uma implantação ao vivo, respectivamente (não use ambos os sinalizadores no mesmo comando). Esta tabela explica a finalidade de cada um desses sinalizadores.

| Sinalizador | DESCRIÇÃO |
| --- | --- |
| **/T** | Chama MSDeploy. exe com o sinalizador **– WhatIf** , que indica uma execução de avaliação. Em vez de implantar o pacote, ele cria um relatório do que aconteceria se você implantou o pacote. |
| **/Y** | Chama MSDeploy. exe sem o sinalizador **– WhatIf** . Isso implanta o pacote no computador local ou no servidor de destino especificado. |
| **Opção** | Especifica o nome do servidor de destino ou a URL do serviço. Para obter mais informações sobre os valores que você pode fornecer aqui, consulte a seção **considerações de ponto de extremidade** neste tópico. Se você omitir o sinalizador **/m** , o pacote será implantado no computador local. |
| **SRDF** | Especifica o tipo de autenticação que o MSDeploy. exe deve usar para executar a implantação. Os valores possíveis são **NTLM** e **básico**. Se você omitir o sinalizador **/a** , o tipo de autenticação padrão será **NTLM** para implantação no serviço de agente remoto implantação da Web e em **básico** para implantação no manipulador de implantação da Web. |
| **/U** | Especifica um nome de usuário. Isso se aplicará somente se você estiver usando a autenticação básica. |
| **/P** | Especifica a senha. Isso se aplicará somente se você estiver usando a autenticação básica. |
| **/L** | Indica que o pacote deve ser implantado na instância de IIS Express local. |
| **/G** | Especifica que o pacote é implantado usando a [configuração do provedor tempAgent](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Se você omitir o sinalizador **/g** , o valor padrão será **false**. |

> [!NOTE]
> Toda vez que o processo de compilação cria um pacote da Web, ele também cria um arquivo chamado *[Project Name]. Deploy-README. txt* que explica essas opções de implantação.

Além desses sinalizadores, você pode especificar Implantação da Web configurações de operação como parâmetros adicionais *. Deploy. cmd* . As configurações adicionais que você especificar serão simplesmente passadas para o comando MSDeploy. exe subjacente. Para obter mais informações sobre essas configurações, consulte [implantação da Web configurações de operação](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Suponha que você queira implantar o projeto de aplicativo Web ContactManager. Mvc em um ambiente de teste executando o arquivo *. Deploy. cmd* . Seu ambiente de teste está configurado para usar o Implantação da Web serviço de agente remoto, conforme descrito em [configurar um servidor Web para implantação da Web publicação (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Para implantar o aplicativo Web, você precisa concluir as próximas etapas.

**Para implantar um aplicativo Web usando o arquivo. Deploy. cmd**

1. Compile e empacote o projeto de aplicativo Web, conforme descrito em [criando e empacotando projetos de aplicativos Web](building-and-packaging-web-application-projects.md).
2. Modifique o arquivo *ContactManager. Mvc. Parameters. xml* para conter os valores de parâmetro corretos para seu ambiente de teste, conforme descrito em [configurando parâmetros para implantação de pacote da Web](configuring-parameters-for-web-package-deployment.md).
3. Abra uma janela de prompt de comando e navegue até o local do arquivo *ContactName. Mvc. Deploy. cmd* .
4. Digite este comando e pressione ENTER:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

Neste exemplo:

- O sinalizador **/y** indica que você deseja realmente implantar o pacote, em vez de fazer uma execução de avaliação.
- O sinalizador **/m** indica que você deseja implantar o pacote no servidor chamado TESTWEB1. A partir desse valor, o MSDeploy. exe tentará implantar o pacote no serviço de agente remoto Implantação da Web em http://TESTWEB1/MSDeployAgentService.
- O sinalizador **/a** indica que você deseja usar a autenticação NTLM. Assim, você não precisa especificar um nome de usuário e uma senha.

Para ilustrar como o uso do arquivo *. Deploy. cmd* simplifica o processo de implantação, dê uma olhada no comando MSDeploy. exe que é gerado e executado quando você executa *ContactManager. Mvc. Deploy. cmd* usando as opções mostradas acima.

[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]

Para obter mais informações sobre como usar o arquivo *. Deploy. cmd* para implantar um pacote da Web, consulte [como: instalar um pacote de implantação usando o arquivo Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>Usando o MSDeploy. exe

Embora o uso do arquivo *. Deploy. cmd* geralmente simplifique o processo de implantação, há algumas situações em que é preferível usar o MSDeploy. exe diretamente. Por exemplo:

- Se você quiser implantar o manipulador de Implantação da Web como um usuário não administrador, não poderá usar o arquivo *. Deploy. cmd* . Isso ocorre devido a um bug no Implantação da Web 2,0, conforme descrito em **Considerações sobre o ponto de extremidade**.
- Se você quiser alternar manualmente entre diferentes arquivos *Parameters. xml* em locais diferentes, talvez prefira usar o MSDeploy. exe diretamente.
- Se você quiser substituir vários argumentos de linha de comando MSDeploy. exe, talvez prefira usar o MSDeploy. exe diretamente.

Ao usar o MSDeploy. exe, você precisa fornecer três informações importantes:

- Um parâmetro **– Source** que indica de onde seus dados vêm.
- Um parâmetro **– dest** que indica onde seus dados vão.
- Um parâmetro **– Verb** que indica a [operação](https://technet.microsoft.com/library/dd568989(WS.10).aspx) que você deseja executar.

O MSDeploy. exe se baseia em [provedores de implantação da Web](https://technet.microsoft.com/library/dd569040(WS.10).aspx) para processar dados de origem e de destino. O Implantação da Web inclui muitos provedores que representam a variedade de aplicativos e fontes de dados com&#x2014;os quais ele pode trabalhar, por exemplo, há provedores para bancos de SQL Server, servidores Web do IIS, certificados, assemblies GAC (cache de assembly global), vários arquivos de configuração diferentes e muitos outros tipos de dados. O parâmetro **– Source** e o parâmetro **– dest** devem especificar um provedor, no formato **– origem**: [*ProviderName*] = [*local*]. Quando você estiver implantando um pacote da Web em um site do IIS, deverá usar estes valores:

- O provedor **– Source** é sempre [pacote](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Por exemplo:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- O provedor **– dest** é sempre [automático](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Por exemplo:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- O **verbo –** é sempre **sincronizado**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Além disso, você precisará especificar várias outras [configurações específicas do provedor](https://technet.microsoft.com/library/dd569001(WS.10).aspx) e configurações gerais de [operação](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Por exemplo, suponha que você queira implantar o aplicativo Web ContactManager. Mvc em um ambiente de preparo. A implantação destinará-se ao manipulador de Implantação da Web e deverá usar a autenticação básica. Para implantar o aplicativo Web, você precisa concluir as próximas etapas.

**Para implantar um aplicativo Web usando o MSDeploy. exe**

1. Compile e empacote o projeto de aplicativo Web, conforme descrito em [criando e empacotando projetos de aplicativos Web](building-and-packaging-web-application-projects.md).
2. Modifique o arquivo *ContactManager. Mvc. Parameters. xml* para conter os valores de parâmetro corretos para seu ambiente de preparo, conforme descrito em [configurando parâmetros para implantação de pacote da Web](configuring-parameters-for-web-package-deployment.md).
3. Abra uma janela de prompt de comando e navegue até o local de MSDeploy. exe. Isso normalmente é em%PROGRAMFILES%\IIS\Microsoft Implantação da Web V2\msdeploy.exe.
4. Digite este comando e pressione Enter (desconsidere as quebras de linha):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

Neste exemplo:

- O parâmetro **– Source** especifica o provedor de **pacote** e indica o local do pacote da Web.
- O parâmetro **– dest** especifica o provedor **automático** . A configuração **ComputerName** fornece a URL de serviço do manipulador de implantação da Web no servidor de destino. A configuração **AuthType** indica que você deseja usar a autenticação básica e, como tal, você precisa fornecer um **nome de usuário** e uma **senha**. Por fim, a configuração **includeAcls = "false"** indica que você não deseja copiar as ACLs (listas de controle de acesso) dos arquivos em seu aplicativo Web de origem para o servidor de destino.
- O argumento **– verbo: Sync** indica que você deseja replicar o conteúdo de origem no servidor de destino.
- Os argumentos **– disableLink** indicam que você não deseja replicar pools de aplicativos, configuração de diretório virtual ou certificados de protocolo SSL (SSL) no servidor de destino. Para obter mais informações, consulte [implantação da Web link Extensions](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- O parâmetro **– setparamfile** fornece o local do arquivo *Parameters. xml* .
- A opção **– allowUntrusted** indica que implantação da Web deve aceitar certificados SSL que não foram emitidos por uma autoridade de certificação confiável. Se você estiver implantando no manipulador de Implantação da Web e tiver usado um certificado autoassinado para proteger a URL do serviço, você precisará incluir essa opção.

## <a name="automating-web-package-deployment"></a>Automatizando a implantação de pacotes da Web

Em muitos cenários empresariais, você desejará implantar seus pacotes da Web como parte de uma implantação única ou automatizada maior. Independentemente de você optar por implantar seus pacotes da Web executando o arquivo *. Deploy. cmd* ou usando o MSDeploy. exe diretamente, você pode parametrizar seus comandos e chamá-los de um destino em um arquivo de projeto Microsoft Build Engine (MSBuild).

Na solução de exemplo Contact Manager, dê uma olhada no destino **PublishWebPackages** no arquivo *Publish. proj* . Esse destino é executado uma vez para cada arquivo *. Deploy. cmd* identificado por uma lista de itens chamada **PublishPackages**. O destino usa propriedades e metadados de item para criar um conjunto completo de valores de argumento para cada arquivo *. Deploy. cmd* e, em seguida, usa a tarefa **exec** para executar o comando.

[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]

> [!NOTE]
> Para obter uma visão geral mais ampla do modelo de arquivo do projeto na solução de exemplo e uma introdução aos arquivos de projeto personalizados em geral, consulte [noções básicas sobre o arquivo de projeto](understanding-the-project-file.md) e [noções básicas sobre o processo de compilação](understanding-the-build-process.md).

## <a name="endpoint-considerations"></a>Considerações de ponto de extremidade

Independentemente de você implantar seu pacote da Web executando o arquivo *. Deploy. cmd* ou usando o MSDeploy. exe diretamente, você precisa especificar um nome de computador ou um ponto de extremidade de serviço para sua implantação.

Se o servidor Web de destino estiver configurado para implantação usando o Implantação da Web serviço de agente remoto, especifique a URL do serviço de destino como seu destino.

[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]

Como alternativa, você pode especificar o nome do servidor sozinho como seu destino e Implantação da Web inferirá a URL do serviço de agente remoto.

[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]

Se o servidor Web de destino estiver configurado para implantação usando o manipulador de Implantação da Web, você precisará especificar o endereço do ponto de extremidade do serviço de gerenciamento da Web do IIS (WMSvc) como seu destino. Por padrão, isso assume o formato:

[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]

Você pode direcionar qualquer um desses pontos de extremidade usando o arquivo *. Deploy. cmd* ou o MSDeploy. exe diretamente. No entanto, se você quiser implantar no manipulador de Implantação da Web como um usuário não administrador, conforme descrito em [configurar um servidor Web para implantação da Web publicação (manipulador de implantação da Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), você precisará adicionar uma cadeia de caracteres de consulta ao endereço do ponto de extremidade de serviço.

[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]

Isso ocorre porque o usuário não administrador não tem acesso no nível de servidor ao IIS; Ele só tem acesso a um site do IIS específico. No momento da gravação, devido a um bug no WPP (pipeline de publicação na Web), não é possível executar o arquivo *. Deploy. cmd* usando um endereço de ponto de extremidade que inclui uma cadeia de caracteres de consulta. Nesse cenário, você precisa implantar o pacote da Web usando o MSDeploy. exe diretamente.

> [!NOTE]
> Para obter mais informações sobre o Implantação da Web serviço de agente remoto e o manipulador de Implantação da Web, consulte [escolhendo a abordagem certa para a implantação da Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Para obter orientação sobre como configurar seus arquivos de projeto específicos do ambiente para implantar nesses pontos de extremidade, consulte [Configurar Propriedades de implantação para um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="authentication-considerations"></a>Considerações de autenticação

Independentemente de você implantar seu pacote da Web executando o arquivo *. Deploy. cmd* ou usando o MSDeploy. exe diretamente, você precisa especificar um tipo de autenticação. Implantação da Web aceita dois valores possíveis: **NTLM** ou **básico**. Se você especificar a autenticação básica, também precisará fornecer um nome de usuário e uma senha. Há vários fatores que você precisa saber ao selecionar um tipo de autenticação:

- Se estiver implantando no serviço de agente remoto Implantação da Web, você deverá usar a autenticação NTLM. O serviço de agente remoto não aceita credenciais de autenticação básicas.
- Se você estiver implantando no manipulador de Implantação da Web, poderá usar a autenticação NTLM ou básica. A configuração padrão é autenticação básica. Embora a autenticação básica dependa de nomes de usuário e senhas sendo transmitidas em texto sem formatação, suas credenciais são protegidas, pois o manipulador de Implantação da Web sempre usa criptografia SSL.
- Se o seu pacote da Web incluir um banco de dados e o servidor Web e o servidor de banco de dados forem computadores separados, você não poderá implantar o banco de dados usando a autenticação NTLM devido à [limitação de "salto duplo" do NTLM](https://go.microsoft.com/?linkid=9805120). Você precisa usar credenciais de SQL Server na sua cadeia de conexão de implantação ou fornecer credenciais de autenticação básica para Implantação da Web. Esse problema é descrito mais detalhadamente na [implantação de bancos de dados de associação em ambientes corporativos](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como você pode implantar um pacote da Web executando o arquivo *. Deploy. cmd* ou usando o MSDeploy. exe diretamente. Ele explicou quando cada abordagem pode ser apropriada e descreveu como você pode parametrizar e executar um comando de implantação como parte de um processo de compilação automatizado ou de uma única etapa maior.

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre como criar e Parametrizar um pacote de implantação da Web, consulte [criando e empacotando projetos de aplicativos Web](building-and-packaging-web-application-projects.md) e [configurando parâmetros para implantação de pacote da Web](configuring-parameters-for-web-package-deployment.md). Para obter orientação sobre como criar e implantar pacotes da Web de uma instância do Team Foundation Server (TFS), consulte [Configurando o Team Foundation Server para implantação automatizada da Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Para obter informações sobre como personalizar e solucionar problemas do processo de implantação, consulte [excluindo arquivos e pastas da implantação](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Anterior](configuring-parameters-for-web-package-deployment.md)
> [Próximo](deploying-database-projects.md)
