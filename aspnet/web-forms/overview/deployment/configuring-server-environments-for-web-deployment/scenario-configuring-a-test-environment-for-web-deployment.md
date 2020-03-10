---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
title: 'Cenário: Configurando um ambiente de teste para implantação da Web | Microsoft Docs'
author: jrjlee
description: Este tópico descreve um cenário típico de implantação da Web para ambientes de desenvolvimento ou de teste e explica as tarefas que você precisa concluir para configurar um si...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 44a22ac7-1fc7-4174-b946-c6129fb6a19b
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: d580e550f2461837f0e8a4e477273348b49cb53e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637812"
---
# <a name="scenario-configuring-a-test-environment-for-web-deployment"></a>Cenário: Configurando um ambiente de teste para implantação da Web

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve um cenário típico de implantação da Web para ambientes de desenvolvimento ou de teste e explica as tarefas que você precisa concluir para configurar um ambiente semelhante.

Quando os desenvolvedores trabalham em aplicativos Web, muitas vezes são concedidos acesso a um ambiente de servidor que eles podem usar para testar alterações em seus aplicativos em uma configuração realista. Esse tipo de ambiente de desenvolvimento ou teste normalmente tem estas características:

- O ambiente consiste em um único servidor Web e um único servidor de banco de dados.
- Os desenvolvedores geralmente têm privilégios de administrador nos servidores, para permitir que eles configurem o ambiente para os requisitos de seus aplicativos.
- As alterações nos aplicativos são implantadas com frequência, portanto, o ambiente precisa dar suporte à implantação única ou automatizada.

Por exemplo, em nosso [cenário de tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Matt hink é um desenvolvedor da Fabrikam, Inc. Matt está trabalhando na solução Contact Manager e, regularmente, precisa implantar alterações em um ambiente de teste. Matt é um administrador no servidor Web de teste e no servidor de banco de dados de teste. Inicialmente, Matt precisa ser capaz de implantar a solução diretamente no ambiente de teste.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image1.png)

À medida que o trabalho progride e mais desenvolvedores ingressam na equipe, a solução Contact Manager é configurada para CI (integração contínua) no Team Foundation Server (TFS). Sempre que um desenvolvedor fizer o check-in de conteúdo, o Team Build deverá criar a solução, executar qualquer teste de unidade e implantar automaticamente a solução no ambiente de teste.

![](scenario-configuring-a-test-environment-for-web-deployment/_static/image2.png)

## <a name="solution-overview"></a>Visão geral da solução

O ambiente de teste precisa dar suporte a uma implantação única ou automatizada de um computador remoto, para que você tenha uma opção de duas abordagens principais. Você pode:

- Configure o servidor Web de teste para dar suporte à implantação usando o serviço de Deployment Agent da Web (o "agente remoto").
- Configure o servidor Web de teste para dar suporte à implantação usando o manipulador de Implantação da Web.

> [!NOTE]
> Você também pode usar [implantação da Web sob demanda](https://technet.microsoft.com/library/ee517345(WS.10).aspx) (o "agente Temp"). Isso é semelhante à abordagem do agente remoto em termos de requisitos e restrições.

Nesse caso, os desenvolvedores têm privilégios de administrador nos servidores de destino, e o ambiente de teste não está sujeito a restrições de segurança estritas; portanto, a escolha lógica é configurar o servidor Web de teste para dar suporte à implantação usando o agente remoto. Isso é menos complexo e requer menos configuração inicial do que a abordagem do manipulador de Implantação da Web. Você também precisará configurar o servidor de banco de dados para dar suporte ao acesso e à implantação remotos.

Esses tópicos fornecem todas as informações de que você precisa para concluir essas tarefas:

- [Configurar um servidor Web para publicação de implantação da Web (agente remoto)](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Este tópico descreve como criar um servidor Web que dá suporte à publicação de Implantação da Web, usando a abordagem do agente remoto, a partir de uma compilação limpa do Windows Server 2008 R2.
- [Configure um servidor de banco de dados para implantação da Web publicação](configuring-a-database-server-for-web-deploy-publishing.md). Este tópico descreve como configurar um servidor de banco de dados para dar suporte ao acesso e à implantação remotos, a partir de uma instalação padrão do SQL Server 2008 R2.

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre como configurar um ambiente de preparo típico, consulte [cenário: Configurando um ambiente de preparo para implantação da Web](scenario-configuring-a-staging-environment-for-web-deployment.md). Para obter orientação sobre como configurar um ambiente de produção típico, consulte [cenário: Configurando um ambiente de produção para implantação da Web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](choosing-the-right-approach-to-web-deployment.md)
> [Próximo](scenario-configuring-a-staging-environment-for-web-deployment.md)
