---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
title: Implantação da Web na empresa | Microsoft Docs
author: jrjlee
description: Este tutorial descreve como atender a vários dos desafios que você encontrará ao gerenciar a implantação de aplicativos Web de escala empresarial para o desenvolvedor...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b8283698-7b82-42a8-8d83-3aeb18ca7fcc
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/web-deployment-in-the-enterprise
msc.type: authoredcontent
ms.openlocfilehash: bc7bb676a71af4ec3451aa3adf3c03ce3b5200d5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628166"
---
# <a name="web-deployment-in-the-enterprise"></a>Implantação da Web na empresa

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial descreve como atender a vários dos desafios que você encontrará ao gerenciar a implantação de aplicativos Web de escala empresarial para ambientes de desenvolvimento, teste, preparo e produção. O tutorial inclui uma solução de referência junto com uma combinação de conteúdo conceitual e orientado a tarefas para orientá-lo em várias tarefas e procedimentos comuns.
> 
> Para uma tradução italiana desses tutoriais, visite [http://www.lucamorelli.it](http://www.lucamorelli.it).

## <a name="enterprise-deployment-challenges"></a>Desafios de implantação corporativa

As organizações geralmente encontram esses desafios quando buscam gerenciar a implantação de soluções complexas de escala empresarial:

- Você precisa ser capaz de implantar projetos em vários ambientes, como ambientes de desenvolvimento ou de teste, plataformas de preparo e servidores de produção. A solução precisa ser implantada com definições de configuração diferentes para cada ambiente.
- Você precisa implantar vários projetos dependentes simultaneamente como parte de um processo de compilação e implantação automatizado ou de etapa única.
- Você precisa ser capaz de impulsionar a implantação de um processo automatizado. Por exemplo, você deseja usar um processo de CI (integração contínua) para implantar aplicativos Web em um ambiente de teste quando for feito o check-in de um novo código.
- Você precisa ser capaz de controlar o processo de implantação e definir variáveis de implantação de fora do Visual Studio, já que os desenvolvedores provavelmente têm as definições de configuração corretas ou as credenciais necessárias para cada ambiente de destino.
- Você precisa implantar projetos de banco de dados baseados em esquema e preservar os existentes em implantações subsequentes.
- Você precisa implantar bancos de dados de associação em uma base ad hoc sem implantar a conta de usuário. Talvez você também precise atualizar o esquema dos bancos de dados de associação implantados sem perder os existentes.
- Você precisa excluir determinados arquivos ou pastas ao implantar conteúdo em vários ambientes de destino.

## <a name="overview-of-approach"></a>Visão geral da abordagem

Este tutorial, junto com os outros tutoriais desta série, usa essa abordagem de alto nível para atender aos desafios descritos acima.

- **Use arquivos de projeto do Microsoft Build Engine (MSBuild) personalizados para controlar o processo geral de compilação e implantação.**
- Isso permite que você crie e implante cada projeto na solução como parte de uma única operação programável por script.
- As configurações específicas do ambiente são configuradas usando arquivos de projeto simples específicos do ambiente. Ao contrário da abordagem centrada no Visual Studio do uso de configurações de solução e de perfis de publicação para configurar implantações para ambientes diferentes, essa abordagem permite que você configure e gerencie o processo de implantação de fora do Visual Studio. Isso significa que os desenvolvedores não precisam de conhecimento avançado de cadeias de conexão, pontos de extremidade de serviço, credenciais de servidor e outras variáveis de implantação para ambientes de destino.
- Os arquivos de projeto personalizados podem ser invocados pelo Team Build como parte de um fluxo de trabalho Team Foundation Server (TFS). Isso permite que você configure a implantação automatizada para cenários de CI.

**Use a Implantação da Web (ferramenta de implantação da Web) do Serviços de Informações da Internet (IIS) para empacotar e implantar projetos de aplicativos Web.**

- O Implantação da Web fornece uma estrutura que permite empacotar e implantar o conteúdo do aplicativo Web em um servidor Web do IIS de destino, juntamente com dependências, definições de configuração, configurações de segurança e quaisquer outros requisitos.
- Você pode controlar todo o processo de empacotamento e implantação de dentro de seus arquivos de projeto do MSBuild personalizados. Você também pode manipular as definições de configuração que acompanham seu pacote de implantação da Web, como cadeias de conexão, pontos de extremidade de serviço e detalhes de destino do IIS.
- Implantação da Web, junto com o pipeline de publicação na Web, oferece muitos pontos de extensibilidade que permitem personalizar suas implantações. Por exemplo, é fácil excluir arquivos e pastas indesejados de seus pacotes de implantação da Web.

**Use o utilitário VSDBCMD. exe para implantar e atualizar esquemas de banco de dados.**

- O VSDBCMD permite que você implante bancos de dados a partir de um arquivo de esquema (. dbschema), que é gerado quando você cria um projeto de banco de dados do Visual Studio. Por outro lado, a funcionalidade de implantação de banco de dados incluída no Implantação da Web é mais adequada para a implantação de dados existentes de uma instância de SQL Server local.
- Ao contrário da funcionalidade do Visual Studio para implantar projetos de banco de dados, o VSDBCMD permite implantar atualizações diferenciais em um banco de dados de destino existente. Isso permite preservar qualquer dado existente enquanto você atualiza o esquema de banco de dados.
- Você pode executar comandos VSDBCMD de dentro de seus arquivos de projeto do MSBuild personalizados.

## <a name="content-map"></a>Mapa de conteúdo

Este tutorial inclui tópicos que se enquadram em quatro áreas principais.

Estes tópicos apresentam a solução&#x2014;de referência da solução&#x2014;Contact Manager e descrevem como baixá-la e configurá-la em seu computador local:

- [A solução Gerenciador de Contatos](the-contact-manager-solution.md)
- [Configuração da solução Gerenciador de Contatos](setting-up-the-contact-manager-solution.md)

Estes tópicos introduzem arquivos de projeto do MSBuild, descrevem como você pode criar e usar arquivos de projeto personalizados e percorrer o processo de implantação para a solução Contact Manager:

- [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md)
- [Noções básicas sobre o processo de build](understanding-the-build-process.md)

Esses tópicos descrevem a implantação de aplicativos Web, incluindo como funciona o processo de criação e empacotamento, como o processo de compilação se integra ao pipeline de publicação na Web, como modificar parâmetros de implantação e como implantar pacotes da Web no destino sistemas

- [Criação e a empacotamento de projetos de aplicativos Web](building-and-packaging-web-application-projects.md)
- [Configuração de parâmetros para a implantação de pacote da Web](configuring-parameters-for-web-package-deployment.md)
- [Implantando pacotes da Web](deploying-web-packages.md)

- A [implantação de projetos de banco de dados](deploying-database-projects.md) descreve as diferentes técnicas que você pode usar para implantar projetos de banco de dados do Visual Studio, juntamente com as vantagens e desvantagens de cada abordagem. [Criar e executar um arquivo de comando de implantação](creating-and-running-a-deployment-command-file.md) descreve como criar um arquivo de comando simples que encapsula sua lógica de implantação e permite que você implante soluções complexas como um processo de etapa única.
- Por fim, a [instalação manual de pacotes da Web](manually-installing-web-packages.md) conclui o tutorial mostrando a importação de pacotes da Web para o IIS.

## <a name="key-technologies"></a>Principais tecnologias

Os tópicos neste tutorial usam principalmente essas tecnologias para gerenciar a compilação e a implantação:

- Visual Studio 2010
- MSBuild
- IIS 7,5
- Web Deploy 2.0
- O utilitário de implantação de banco de dados VSDBCMD. exe

## <a name="other-tutorials-in-this-series"></a>Outros tutoriais desta série

Isso faz parte de uma série de cinco tutoriais sobre a implantação na Web em escala empresarial. Estes são os outros tutoriais da série:

- [Implantação de aplicativos Web em cenários empresariais](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este conteúdo introdutório fornece o plano de fundo contextual para a série de tutoriais. Ele descreve o cenário de tutorial e ilustra como as tarefas e as orientações descritas em toda a série se encaixam em um processo de ALM (gerenciamento de ciclo de vida de aplicativo) mais amplo.
- [Configurando ambientes de servidor para implantação na Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial descreve como configurar servidores Windows para oferecer suporte a vários cenários de implantação, incluindo a implantação remota de pacotes da Web usando o serviço Web Deployment Agent (o agente remoto) ou o manipulador de Implantação da Web e a implantação de banco de dados remoto. Ele fornece orientação sobre como escolher o método de implantação apropriado para seu próprio ambiente e descreve como usar o WFF (Web farm Framework) para replicar aplicativos Web implantados em todos os servidores Web em um farm de servidores.
- [Configurando Team Foundation Server para implantação da Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial descreve como configurar o TFS para dar suporte a vários cenários de implantação, incluindo a implantação automatizada como parte de um processo de CI e as implantações disparadas manualmente de compilações específicas.
- [Implantação da Web empresarial avançada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial descreve como realizar várias tarefas de implantação mais avançadas, como personalizar implantações de banco de dados para vários ambientes, excluindo arquivos e pastas da implantação e colocando aplicativos Web offline durante o processo de implantação .

> [!div class="step-by-step"]
> [Próximo](the-contact-manager-solution.md)
