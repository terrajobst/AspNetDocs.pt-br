---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: 'Cenário: Configurando um ambiente de produção para implantação da Web | Microsoft Docs'
author: jrjlee
description: Este tópico descreve um cenário de implantação da Web típico para um ambiente de produção e explica as tarefas que você precisa concluir para configurar um semelhante...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2d76e715cdbf6ec484fa0ff98b3b3d1d8dfd3961
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78635418"
---
# <a name="scenario-configuring-a-production-environment-for-web-deployment"></a>Cenário: Configurando um ambiente de produção para implantação na Web

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve um cenário de implantação da Web típico para um ambiente de produção e explica as tarefas que você precisa concluir para configurar um ambiente semelhante.

O ambiente de produção é o destino final de um aplicativo Web ou de um site. Nesse ponto, seu aplicativo passou por testes, foi implantado em um ambiente de preparo e está pronto para "entrar em funcionamento". As características de um ambiente de produção podem variar muito de acordo com a natureza e a finalidade do seu conteúdo da Web, o tamanho da sua organização, o público-alvo e muitos outros fatores. Em um cenário de escala empresarial, o ambiente de produção pode ter estas características:

- O ambiente consiste em vários servidores Web com balanceamento de carga e um ou mais servidores de banco de dados, geralmente com clustering de failover e espelhamento de banco de dados.
- Se o ambiente for voltado para a Internet, provavelmente será separado da sua rede interna. Ele pode estar em uma sub-rede diferente em uma rede de perímetro, podendo estar em um domínio diferente e pode estar em uma infraestrutura de rede totalmente diferente.
- Os desenvolvedores e as contas de processo do servidor de compilação são altamente improvável de ter privilégios de administrador nos servidores de produção.
- As alterações nos aplicativos são implantadas com menos frequência do que as implantações de teste ou de preparo.

> [!NOTE]
> A expansão de uma implantação de banco de dados em vários servidores está além do escopo deste tutorial. Para obter mais informações sobre essa área, consulte [manuais online do SQL Server](https://technet.microsoft.com/library/ms130214.aspx).

Por exemplo, em nosso [cenário de tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), um servidor do Team Build inclui definições de compilação que permitem aos usuários criar a solução Contact Manager e implantá-la em um ambiente de preparo em uma única etapa. Quando o aplicativo está pronto para ser implantado para produção, devido às restrições impostas pelos requisitos de segurança e pela infraestrutura de rede, o administrador do ambiente de produção deve copiar manualmente o pacote da Web em um servidor Web de produção e importar por meio do Gerenciador do Serviços de Informações da Internet (IIS).

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a>Visão geral da solução

Nesse cenário, você pode deduzir esses fatos de uma análise dos requisitos de implantação:

- Devido a restrições de segurança e à configuração de rede, você não pode configurar o ambiente de produção para dar suporte à implantação de um clique ou automatizada. A implantação offline é a única abordagem viável nesse cenário.
- O ambiente de produção inclui vários servidores Web, para que você possa usar o WFF (Web farm Framework) para criar um farm de servidores. Usando essa abordagem, o administrador só precisa importar o aplicativo para um servidor Web (o servidor primário) e o WFF replicará a implantação em todos os outros servidores Web no ambiente de produção.

Esses tópicos fornecem todas as informações de que você precisa para concluir essas tarefas:

- [Crie um farm de servidores com a estrutura do Web farm](configuring-a-database-server-for-web-deploy-publishing.md). Este tópico descreve como criar e configurar um farm de servidores usando o WFF, para que os produtos e componentes da plataforma Web, definições de configuração e sites e aplicativos sejam replicados em vários servidores Web com balanceamento de carga.
- [Configurar um servidor Web para publicação de implantação da Web (implantação offline)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Este tópico descreve como criar um servidor Web que permite que os administradores importem e implantem pacotes da Web manualmente, começando em uma compilação limpa do Windows Server 2008 R2.
- [Configure um servidor de banco de dados para implantação da Web publicação](configuring-a-database-server-for-web-deploy-publishing.md). Este tópico descreve como configurar um servidor de banco de dados para dar suporte ao acesso e à implantação remotos, a partir de uma instalação padrão do SQL Server 2008 R2.

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre como configurar um ambiente típico de teste do desenvolvedor, consulte [cenário: Configurando um ambiente de teste para implantação da Web](scenario-configuring-a-test-environment-for-web-deployment.md). Para obter orientação sobre como configurar um ambiente de preparo típico, consulte [cenário: Configurando um ambiente de preparo para implantação da Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](scenario-configuring-a-staging-environment-for-web-deployment.md)
> [Próximo](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
