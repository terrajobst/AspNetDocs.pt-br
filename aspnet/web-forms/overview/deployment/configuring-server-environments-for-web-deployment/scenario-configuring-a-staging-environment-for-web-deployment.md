---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
title: 'Cenário: Configurando um ambiente de preparo para implantação da Web | Microsoft Docs'
author: jrjlee
description: Este tópico descreve um cenário de implantação da Web típico para um ambiente de preparo e explica as tarefas que precisam ser concluídas para configurar uma variável parecida...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 5a8e49b7-5317-4125-b107-7e2466b47bb3
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: eaa61ca850817f8dd98955b59e94be93389bf256
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637840"
---
# <a name="scenario-configuring-a-staging-environment-for-web-deployment"></a>Cenário: Configurando um ambiente de preparo para implantação na Web

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve um cenário de implantação da Web típico para um ambiente de preparo e explica as tarefas que você precisa concluir para configurar um ambiente semelhante.

Muitas organizações usam ambientes de preparo para visualizar atualizações em aplicativos Web ou sites. Isso dá às pessoas da organização a oportunidade de explorar e examinar novas funcionalidades ou conteúdo antes que o site "fique ativo" ou, em outras palavras, é implantado em um ambiente de produção. O ambiente de preparo foi projetado para replicar o ambiente de produção o mais próximo possível, a fim de fornecer uma visualização realista. Esse tipo de ambiente de preparo normalmente tem estas características:

- O ambiente consiste em vários servidores Web com balanceamento de carga e um ou mais servidores de banco de dados, geralmente com clustering de failover e espelhamento de banco de dados.
- Os aplicativos podem ser implantados manualmente por uma equipe de desenvolvimento ou automaticamente por um servidor de compilação de equipe.
- É improvável que os usuários ou contas de processo que implantam aplicativos tenham privilégios de administrador nos servidores de preparo.
- As alterações nos aplicativos são implantadas com frequência, portanto, o ambiente precisa dar suporte à implantação única ou automatizada.

> [!NOTE]
> A expansão de uma implantação de banco de dados em vários servidores está além do escopo deste tutorial. Para obter mais informações sobre essa área, consulte [manuais online do SQL Server](https://technet.microsoft.com/library/ms130214.aspx).

Por exemplo, em nosso [cenário de tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), Team Foundation Server (TFS) gerencia a solução do Gerenciador de contatos. O administrador do TFS, Rob Fábio, criou uma definição de compilação que permite aos desenvolvedores disparar uma implantação para o ambiente de preparo conforme necessário.

![](scenario-configuring-a-staging-environment-for-web-deployment/_static/image1.png)

Observe que, na maioria dos casos, você não desejará necessariamente implantar a compilação mais recente no ambiente de preparo. Em vez disso, é muito mais provável que você queira implantar uma compilação específica que já passou pela validação e verificação no ambiente de teste.

## <a name="solution-overview"></a>Visão geral da solução

Nesse cenário, você pode deduzir esses fatos de uma análise dos requisitos de implantação:

- A conta de usuário ou processo que executa a implantação não terá privilégios de administrador nos servidores de preparo, de modo que os servidores Web de preparo devem oferecer suporte à implantação não-administrador. Dessa forma, você precisará configurar os servidores Web de preparo para usar o manipulador de Implantação da Web em vez do agente remoto.
- O ambiente de preparo inclui vários servidores Web, mas precisa dar suporte à implantação de um clique ou automatizada, portanto, você precisará usar o WFF (Web farm Framework) para criar um farm de servidores. Usando essa abordagem, você pode implantar um aplicativo em um servidor Web (o servidor primário) e o WFF replicará a implantação em todos os outros servidores Web no ambiente de preparo.
- A conta de usuário ou processo que executa a implantação deve ter permissões para criar bancos de dados. Dessa forma, você precisará adicionar a conta à função de servidor **dbcreator** no servidor de banco de dados, além de configurar o servidor de banco de dados para dar suporte ao acesso e à implantação remotos.

Esses tópicos fornecem todas as informações de que você precisa para concluir essas tarefas:

- [Crie um farm de servidores com a estrutura do Web farm](creating-a-server-farm-with-the-web-farm-framework.md). Este tópico descreve como criar e configurar um farm de servidores usando o WFF, para que os produtos e componentes da plataforma Web, definições de configuração e sites e aplicativos sejam replicados em vários servidores Web com balanceamento de carga.
- [Configure um servidor Web para publicação de implantação da Web (manipulador de implantação da Web)](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Este tópico descreve como criar um servidor Web que dá suporte à publicação de Implantação da Web, usando a abordagem do agente remoto, a partir de uma compilação limpa do Windows Server 2008 R2.
- [Configure um servidor de banco de dados para implantação da Web publicação](configuring-a-database-server-for-web-deploy-publishing.md). Este tópico descreve como configurar um servidor de banco de dados para dar suporte ao acesso e à implantação remotos, a partir de uma instalação padrão do SQL Server 2008 R2.

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre como configurar um ambiente típico de teste do desenvolvedor, consulte [cenário: Configurando um ambiente de teste para implantação da Web](scenario-configuring-a-test-environment-for-web-deployment.md). Para obter orientação sobre como configurar um ambiente de produção típico, consulte [cenário: Configurando um ambiente de produção para implantação da Web](scenario-configuring-a-production-environment-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](scenario-configuring-a-test-environment-for-web-deployment.md)
> [Próximo](scenario-configuring-a-production-environment-for-web-deployment.md)
