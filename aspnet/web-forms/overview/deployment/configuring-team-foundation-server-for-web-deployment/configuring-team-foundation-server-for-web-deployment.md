---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
title: Configurando Team Foundation Server para implantação da Web | Microsoft Docs
author: jrjlee
description: Este tutorial mostrará como configurar o Team Foundation Server (TFS) 2010 para criar soluções e implantar o conteúdo da Web em vários ambientes de destino. Isso...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: ff55233a-e795-4007-a4fc-861fe1bb590b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 638d696abbc5f05957c0ed2eb7ebb65fce7813ea
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528388"
---
# <a name="configuring-team-foundation-server-for-web-deployment"></a>Configuração do Team Foundation Server para a implantação da Web

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tutorial mostrará como configurar o Team Foundation Server (TFS) 2010 para criar soluções e implantar o conteúdo da Web em vários ambientes de destino. Isso inclui cenários de CI (integração contínua), em que você implanta conteúdo automaticamente toda vez que um desenvolvedor faz uma alteração. Ele também pode incluir cenários de gatilho manual, em que um administrador pode querer disparar a implantação de uma compilação específica em um ambiente de preparo depois que a compilação tiver sido verificada e validada no ambiente de teste. Os tópicos deste tutorial orientarão você durante todo o processo de configuração, incluindo:
> 
> - Como criar um novo projeto de equipe no TFS.
> - Como adicionar conteúdo ao controle do código-fonte.
> - Como configurar um servidor de compilação para dar suporte a CI e implantação.
> - Como criar uma definição de compilação que inclui a lógica de implantação.
> - Como configurar permissões para implantação automatizada.
> 
> Para uma tradução italiana desses tutoriais, visite [http://www.lucamorelli.it](http://www.lucamorelli.it).

Este tutorial pressupõe que você instalou o TFS 2010 e criou uma coleção de projetos de equipe como parte do processo de configuração inicial. O [Guia de instalação do Team Foundation para Visual Studio 2010](https://go.microsoft.com/?linkid=9805132) fornece orientação abrangente sobre essas tarefas.

## <a name="context"></a>Contexto

Este formulário faz parte de uma série de tutoriais com base nos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da solução&#x2014; [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no coração desses tutoriais é baseado na abordagem de arquivo de projeto dividido descrita na [compreensão do processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md), no qual o processo de compilação é controlado por dois&#x2014;arquivos de projeto, um contendo instruções de Build que se aplicam a todos os ambientes de destino e um contendo configurações específicas de ambiente e de implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto independente do ambiente para formar um conjunto completo de instruções de compilação.

## <a name="scenario-overview"></a>Visão geral do cenário

O cenário de alto nível para esses tutoriais é descrito em [Enterprise Web Deployment: visão geral do cenário](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md). Recomendamos que você examine este tópico antes de começar neste tutorial.

## <a name="how-to-use-this-tutorial"></a>Como usar este tutorial

Se esta for a primeira vez que você executou as tarefas descritas neste tutorial ou se desejar seguir os exemplos usando a solução de exemplo, você deve trabalhar nos tópicos do tutorial em ordem. Como alternativa, você pode usar tópicos individuais como diretrizes para tarefas específicas. Este tutorial inclui estes tópicos:

- [Criando um projeto de equipe no TFS](creating-a-team-project-in-tfs.md). Um projeto de equipe é a unidade principal para controle do código-fonte, gerenciamento de processos e compilação no TFS. Você precisa criar um projeto de equipe antes de poder adicionar conteúdo ao controle do código-fonte ou criar definições de compilação.
- [Adicionando conteúdo ao controle do código-fonte](adding-content-to-source-control.md). Depois de criar um projeto de equipe, você pode começar a adicionar conteúdo ao controle do código-fonte. Você precisará adicionar seus projetos e soluções, juntamente com quaisquer dependências externas, antes de começar a configurar as compilações.
- [Configurando um servidor de compilação do TFS para implantação da Web](configuring-a-tfs-build-server-for-web-deployment.md). Se desejar criar o conteúdo do projeto de equipe, você precisará configurar um servidor de compilação. Na maioria dos casos, isso deve estar em um computador separado da instalação do TFS. Para configurar um servidor de compilação, você precisa instalar e configurar o serviço de compilação do TFS, instalar o Visual Studio 2010, criar controladores de compilação e Compilar agentes, instalar quaisquer produtos ou componentes que seu código precisa para compilar com êxito e instalar o Implantação da Web (ferramenta de implantação da Web) do Serviços de Informações da Internet (IIS).
- [Criando uma definição de compilação que dá suporte à implantação](creating-a-build-definition-that-supports-deployment.md). Antes de começar a enfileirar ou disparar compilações no TFS, você precisa criar pelo menos uma definição de compilação para seu projeto de equipe. A definição de compilação define todos os aspectos da compilação, incluindo quais coisas devem ser incluídas na compilação, o que deve disparar a compilação e onde o Team Build deve enviar as saídas da compilação. Você pode configurar uma definição de compilação para executar arquivos de projeto de Microsoft Build Engine (MSBuild) personalizados, o que permite incluir lógica de implantação em suas compilações automatizadas.
- [Implantando uma compilação específica](deploying-a-specific-build.md). Em muitos cenários, você desejará implantar uma compilação específica, em vez da compilação mais recente, em um ambiente de destino. Nesse caso, você pode configurar uma definição de compilação que implanta conteúdo de uma pasta de destino específica.
- [Configurando permissões para implantação do Team Build](configuring-permissions-for-team-build-deployment.md). Se o serviço de compilação for implantar conteúdo como parte de um processo de compilação automatizado, você precisará conceder várias permissões para a conta de serviço de compilação em qualquer servidor Web de destino e servidores de banco de dados.

## <a name="key-technologies"></a>Principais tecnologias

Este tutorial se concentra em como usar esses produtos e tecnologias para dar suporte à compilação automatizada e à implantação na Web:

- Visual Studio Team Foundation Server 2010
- Team Build e MSBuild
- Implantação da Web

O tutorial também aborda o uso do Windows Server 2008 R2, IIS 7,5, SQL Server 2008 R2, ASP.NET 4,0 e ASP.NET MVC 3.

## <a name="other-tutorials-in-this-series"></a>Outros tutoriais desta série

Isso faz parte de uma série de cinco tutoriais sobre a implantação na Web em escala empresarial. Estes são os outros tutoriais da série:

- [Implantação de aplicativos Web em cenários empresariais](../deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). Este conteúdo introdutório fornece o plano de fundo contextual para a série de tutoriais. Ele descreve o cenário de tutorial e ilustra como as tarefas e as orientações descritas em toda a série se encaixam em um processo de ALM (gerenciamento de ciclo de vida de aplicativo) mais amplo.
- [Implantação da Web na empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial fornece uma introdução conceitual aos arquivos de projeto do MSBuild, ao WPP (pipeline de publicação na Web), Implantação da Web e outras tecnologias relacionadas. Ele explica como você pode usar essas ferramentas juntas para gerenciar processos de implantação complexos.
- [Configurando ambientes de servidor para implantação na Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial descreve como configurar servidores Windows para oferecer suporte a vários cenários de implantação, incluindo a implantação remota de pacotes da Web usando o serviço Web Deployment Agent (o agente remoto) ou o manipulador de Implantação da Web e a implantação de banco de dados remoto. Ele fornece orientação sobre como escolher o método de implantação apropriado para seu próprio ambiente e descreve como usar o WFF (Web farm Framework) para replicar aplicativos Web implantados em todos os servidores Web em um farm de servidores.
- [Implantação da Web empresarial avançada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial descreve como realizar várias tarefas de implantação mais avançadas, como personalizar implantações de banco de dados para vários ambientes, excluindo arquivos e pastas da implantação e colocando aplicativos Web offline durante o processo de implantação .

> [!div class="step-by-step"]
> [Próximo](creating-a-team-project-in-tfs.md)
