---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
title: Configurando ambientes de servidor para implantação da Web | Microsoft Docs
author: jrjlee
description: Este tutorial mostrará como configurar ambientes de servidor para dar suporte à implantação de um único clique ou automatizada do site e publicação em vários Scen diferentes...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 0bf0959b-4ca8-45de-bd13-b15347543b5a
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 073161ce4faa3b7ba6749d7dfbaa5309eeca4f74
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634466"
---
# <a name="configuring-server-environments-for-web-deployment"></a>Configuração de ambientes de servidor para a Implantação da Web

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial mostrará como configurar ambientes de servidor para dar suporte à implantação de um único clique ou automatizada do site e publicação em vários cenários diferentes. O tutorial inclui tópicos para orientá-lo na conclusão de várias tarefas, como a configuração de um servidor Web para dar suporte a abordagens específicas para a implantação e configuração de um farm de servidores WFF (estrutura de Web farm), junto com visões gerais baseadas em cenários que fornecem Diretrizes de ponta a ponta de nível superior.
> 
> O tutorial usa o cenário de implantação Fabrikam, Inc. descrito em [Enterprise Web Deployment: visão geral do cenário](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) como um ponto de referência para exemplos e infraestrutura de rede.
> 
> Para uma tradução italiana desses tutoriais, visite [http://www.lucamorelli.it](http://www.lucamorelli.it).

Este tutorial inclui estes tópicos:

- [Escolha da abordagem correta para a Implantação da Web](choosing-the-right-approach-to-web-deployment.md)
- [Cenário: configuração de um ambiente de teste para a Implantação da Web](scenario-configuring-a-test-environment-for-web-deployment.md)
- [Cenário: configuração de um ambiente de preparo para a Implantação da Web](scenario-configuring-a-staging-environment-for-web-deployment.md)
- [Cenário: configuração de um ambiente de produção para a Implantação da Web](scenario-configuring-a-production-environment-for-web-deployment.md)
- [Configuração de um servidor Web para publicação de Implantação da Web (agente remoto)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
- [Configuração de um servidor Web para publicação de Implantação da Web (manipulador de Implantação da Web)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
- [Configuração de um servidor Web para publicação de Implantação da Web (implantação offline)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
- [Configuração de um servidor de banco de dados para publicação de Implantação da Web](configuring-a-database-server-for-web-deploy-publishing.md)
- [Criação de um farm de servidores com o Web Farm Framework](creating-a-server-farm-with-the-web-farm-framework.md)
- [Configuração de propriedades de implantação para um ambiente de destino](configuring-deployment-properties-for-a-target-environment.md)

O primeiro tópico, [escolhendo a abordagem certa para a implantação da Web](choosing-the-right-approach-to-web-deployment.md), descreve as principais abordagens que você pode usar para publicar aplicativos Web usando a ferramenta de implantação da web do serviços de informações da Internet (IIS) (Implantação da Web) 2,0. Ele também identifica os cenários que são mapeados para cada abordagem. A partir daqui, cada tópico de cenário fornece uma visão geral de alto nível das tarefas que você precisa concluir e identifica os tópicos que você precisará trabalhar para ajudá-lo a concluir essas tarefas.

Se você estiver usando a abordagem de arquivo de projeto dividido descrita em [noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md) para criar e implantar sua solução, o tópico final, [Configurando propriedades de implantação para um ambiente de destino](configuring-deployment-properties-for-a-target-environment.md), descreve como configurar arquivos de projeto específicos do ambiente para implantação em ambientes de destino diferentes.

## <a name="key-technologies"></a>Principais tecnologias

Este tutorial se concentra em como usar esses produtos e tecnologias para oferecer suporte à implantação da Web:

- IIS 7,5
- Implantação da Web 2. x
- WFF 2.x
- Serviço de gerenciamento da Web do IIS (WMSvc)

O tutorial também aborda o uso do Windows Server 2008 R2, SQL Server 2008 R2, ASP.NET 4,0 e ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Outros tutoriais desta série

Isso faz parte de uma série de cinco tutoriais sobre a implantação na Web em escala empresarial. Estes são os outros tutoriais da série:

- [Implantação de aplicativos Web em cenários empresariais](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este conteúdo introdutório fornece o plano de fundo contextual para a série de tutoriais. Ele descreve o cenário de tutorial e ilustra como as tarefas e as orientações descritas em toda a série se encaixam em um processo de ALM (gerenciamento de ciclo de vida de aplicativo) mais amplo.
- [Implantação da Web na empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial fornece uma introdução conceitual aos arquivos de projeto do Microsoft Build Engine (MSBuild), ao pipeline de publicação na Web, Implantação da Web e outras tecnologias relacionadas. Ele explica como você pode usar essas ferramentas juntas para gerenciar processos de implantação complexos.
- [Configurando Team Foundation Server para implantação da Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial descreve como configurar o Team Foundation Server (TFS) para dar suporte a vários cenários de implantação, incluindo a implantação automatizada como parte de um processo de CI (integração contínua) e as implantações disparadas manualmente de compilações específicas.
- [Implantação da Web empresarial avançada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial descreve como realizar várias tarefas de implantação mais avançadas, como personalizar implantações de banco de dados para vários ambientes, excluindo arquivos e pastas da implantação e colocando aplicativos Web offline durante o processo de implantação .

> [!div class="step-by-step"]
> [Próximo](choosing-the-right-approach-to-web-deployment.md)
