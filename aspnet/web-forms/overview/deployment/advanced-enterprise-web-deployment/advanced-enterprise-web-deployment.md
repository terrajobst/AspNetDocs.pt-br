---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
title: Implantação da Web empresarial avançada | Microsoft Docs
author: jrjlee
description: Este tutorial mostrará como executar várias tarefas que são necessárias ou desejáveis em muitos cenários de implantação corporativa. Para uma tradução em italiano...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 7dcaba80-f2ec-4db3-ad98-daadc3afdb49
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/advanced-enterprise-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: bf4c7d021763017592483df35cb717295c4924aa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587601"
---
# <a name="advanced-enterprise-web-deployment"></a>Implantação da Web corporativa avançada

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial mostrará como executar várias tarefas que são necessárias ou desejáveis em muitos cenários de implantação corporativa.
> 
> Para uma tradução italiana desses tutoriais, visite [http://www.lucamorelli.it](http://www.lucamorelli.it).

Isso faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da solução&#x2014; [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no coração desses tutoriais é baseado na abordagem de arquivo de projeto dividido descrita na [compreensão do processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md), no qual o processo de compilação é controlado por dois&#x2014;arquivos de projeto, um contendo instruções de Build que se aplicam a todos os ambientes de destino e um contendo configurações específicas de ambiente e de implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto independente do ambiente para formar um conjunto completo de instruções de compilação.

## <a name="scenario-overview"></a>Visão geral do cenário

O cenário de alto nível para esses tutoriais é descrito em [Enterprise Web Deployment: visão geral do cenário](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Recomendamos que você examine este tópico antes de começar neste tutorial.

## <a name="how-to-use-this-tutorial"></a>Como usar este tutorial

- Cada um dos tópicos deste tutorial é independente e aborda um determinado desafio ou problema que ocorre em cenários de implantação corporativa. Você não precisa trabalhar com esses tópicos em nenhuma ordem específica. No entanto, este tutorial aborda algumas tarefas avançadas. Dessa forma, você deve se familiarizar com os conceitos e técnicas que o tutorial [da implantação da Web no Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) aborda para aproveitar ao máximo o benefício desse conteúdo.
- Este tutorial inclui estes tópicos:
- [Executando uma implantação "What If"](performing-a-what-if-deployment.md). Em muitos cenários, você desejará determinar o impacto de uma implantação proposta em um ambiente de destino ou qualquer conteúdo existente antes de realmente fazer qualquer alteração. Este tópico descreve como você pode executar uma implantação "e se" para gerar arquivos de log e scripts de atualização de banco de dados como se você tivesse implantado conteúdo em um ambiente de destino, sem realmente fazer nenhuma alteração. A análise desses recursos pode ajudá-lo a identificar possíveis problemas antes de uma implantação ao vivo.
- [Personalizando implantações de banco de dados para vários ambientes](customizing-database-deployments-for-multiple-environments.md). Ao implantar um projeto de banco de dados em vários destinos, muitas vezes você desejará personalizar as propriedades de implantação para cada ambiente de destino. Por exemplo, em ambientes de teste, você normalmente recriaria o banco de dados em todas as implantações, enquanto em ambientes de preparo ou de produção você seria muito mais provável de fazer atualizações incrementais para preservar seus dados. Este tópico descreve como você pode incorporar essas alterações de propriedade em sua lógica de implantação criando um arquivo de configuração de implantação específica do ambiente (. sqldeployment) para cada ambiente de destino.
- Implantação de associações de função de banco de dados para ambientes de teste. Ao recriar um banco de dados em cada&#x2014;implantação, por exemplo, como parte de uma compilação de CI (integração contínua) e implantá-&#x2014;lo em um ambiente de teste, normalmente você precisará configurar associações de função de banco de dados a cada vez. Por exemplo, você geralmente precisará conceder permissões para a identidade do pool de aplicativos associada ao seu aplicativo Web. Este tópico descreve como você pode automatizar esse processo adicionando um script SQL pós-implantação à sua lógica de implantação.
- [Implantação de bancos de dados de associação em ambientes corporativos](deploying-membership-databases-to-enterprise-environments.md). Os bancos de dados de associação do ASP.NET têm várias características que podem complicar o processo de implantação. Por exemplo, uma implantação somente de esquema deixará o banco de dados em um estado não operacional. Na maioria dos cenários, é preferível criar um banco de dados de associação diretamente em cada ambiente de destino. No entanto, se você precisar implantar um banco de dados de associação, este tópico descreve algumas das abordagens que podem ser usadas para atender aos desafios inerentes.
- [Excluindo arquivos e pastas da implantação](excluding-files-and-folders-from-deployment.md). Em alguns cenários, você desejará personalizar o conteúdo do pacote da Web para ambientes de destino específicos. Por exemplo, talvez você queira incluir versões completas de bibliotecas JavaScript ao implantar em um ambiente de teste, para dar suporte à depuração do lado do cliente, mas use versões reduzidos das bibliotecas ao implantar em um ambiente de preparo ou de produção. Este tópico descreve como você pode excluir arquivos e pastas específicas do processo de criação de pacote.
- [Colocando aplicativos Web offline com implantação da Web](taking-web-applications-offline-with-web-deploy.md). Ao implantar soluções em um ambiente de preparo ou de produção, muitas vezes você desejará colocar seus aplicativos Web offline durante o processo de implantação. Este tópico descreve como você pode adicionar um arquivo de *aplicativo\_offline. htm* ao seu aplicativo Web no início do processo de implantação e removê-lo no final. Embora o *aplicativo\_o arquivo offline. htm* esteja em vigor, todos os usuários que navegam para o aplicativo Web são automaticamente redirecionados para o *aplicativo\_arquivo offline. htm* .
- [Executando scripts do Windows PowerShell do MSBuild](running-windows-powershell-scripts-from-msbuild-project-files.md). Muitos cenários de implantação exigem ações de pós-implantação mais complexas, como adicionar fontes de evento personalizadas ao registro ou configurar a replicação entre SQL Server instâncias. Essas ações geralmente são realizadas por meio de scripts do Windows PowerShell. Este tópico descreve como executar scripts do Windows PowerShell de um arquivo de projeto Microsoft Build Engine (MSBuild) como parte do processo de compilação e implantação.
- [Solução de problemas do processo de empacotamento](troubleshooting-the-packaging-process.md). O WPP (pipeline de publicação na Web) define uma propriedade do MSBuild chamada **EnablePackageProcessLoggingAndAssert** que você pode usar para gerar informações detalhadas sobre o processo de empacotamento para projetos de aplicativos Web. Este tópico descreve o que a propriedade faz e como usá-la.

## <a name="key-technologies"></a>Principais tecnologias

Este tutorial se concentra em como usar esses produtos e tecnologias para dar suporte à compilação automatizada e à implantação na Web:

- Visual Studio 2010 e Team Foundation Server (TFS) 2010
- MSBuild e TFS Team Build
- Serviços de Informações da Internet (IIS) 7,5
- Ferramenta de implantação da Web do IIS (Implantação da Web) 2,1
- O utilitário de implantação de banco de dados VSDBCMD. exe

## <a name="other-tutorials-in-this-series"></a>Outros tutoriais desta série

Isso faz parte de uma série de cinco tutoriais sobre a implantação na Web em escala empresarial. Estes são outros tutoriais da série:

- [Implantação de aplicativos Web em cenários empresariais](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este conteúdo introdutório fornece o plano de fundo contextual para a série de tutoriais. Ele descreve o cenário de tutorial e ilustra como as tarefas e as orientações descritas em toda a série se encaixam em um processo de ALM (gerenciamento de ciclo de vida de aplicativo) mais amplo.
- [Implantação da Web na empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial fornece uma introdução conceitual aos arquivos de projeto do MSBuild, ao WPP, ao Implantação da Web e a outras tecnologias relacionadas. Ele explica como você pode usar essas ferramentas juntas para gerenciar processos de implantação complexos.
- [Configurando ambientes de servidor para implantação na Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial descreve como configurar servidores Windows para oferecer suporte a vários cenários de implantação, incluindo a implantação remota de pacotes da Web usando o serviço Web Deployment Agent (o agente remoto) ou o manipulador de Implantação da Web e a implantação de banco de dados remoto. Ele fornece orientação sobre como escolher o método de implantação apropriado para seu próprio ambiente e descreve como usar o WFF (Web farm Framework) para replicar aplicativos Web implantados em todos os servidores Web em um farm de servidores.
- [Configurando Team Foundation Server para implantação da Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial descreve como configurar o TFS para dar suporte a vários cenários de implantação, incluindo a implantação automatizada como parte de um processo de CI e as implantações disparadas manualmente de compilações específicas.

> [!div class="step-by-step"]
> [Próximo](performing-a-what-if-deployment.md)
