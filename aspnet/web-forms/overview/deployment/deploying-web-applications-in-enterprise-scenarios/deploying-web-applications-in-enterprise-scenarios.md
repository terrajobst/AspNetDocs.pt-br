---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
title: Implantando aplicativos Web em cenários empresariais usando o Visual Studio 2010 | Microsoft Docs
author: jrjlee
description: Este conjunto de tutoriais descreve as ferramentas e técnicas que você pode usar para implantar aplicativos Web em vários cenários empresariais. Ele explica como fazer melhor uso...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: 48cfe378-d62a-48c6-a4db-6be3cead6898
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios
msc.type: authoredcontent
ms.openlocfilehash: eb7b82d16079d4d086af1919d092fb28db60d8b3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574154"
---
# <a name="deploying-web-applications-in-enterprise-scenarios-using-visual-studio-2010"></a>Implantação de aplicativos Web em cenários corporativos usando o Visual Studio 2010

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este conjunto de tutoriais descreve as ferramentas e técnicas que você pode usar para implantar aplicativos Web em vários cenários empresariais. Ele explica como fazer o melhor uso de tecnologias como o Visual Studio 2010, o Microsoft Build Engine (MSBuild), o Serviços de Informações da Internet (IIS) 7,5, a ferramenta de implantação da Web do IIS (Implantação da Web), o WFF (Web farm Framework) e utilitários como o VSDBCMD. exe para Simplifique e gerencie o processo de implantação. Ele inclui visões gerais conceituais e diretrizes orientadas a tarefas que ajudarão você a:
> 
> - Examine e estabeleça os requisitos de implantação para um aplicativo Web de escala empresarial.
> - Configure ambientes de servidor Web de teste, de preparo e de produção para dar suporte à implantação da Web.
> - Configure os processos de CI (integração contínua) do Team Foundation Server (TFS) para dar suporte à implantação automatizada da Web.
> - Implante aplicativos Web de escala empresarial em ambientes de servidor diferentes com requisitos e restrições variados.
> - Implantar alterações em aplicativos Web que estão sendo executados em diferentes ambientes de servidor.
> 
> > [!NOTE]
> > Embora esses tutoriais descrevam o uso do TFS como um servidor de CI, as diretrizes são facilmente adaptadas para qualquer servidor de CI. Você não precisa de um conhecimento detalhado do TFS para entender e aproveitar os tutoriais.
> 
> 
> Para uma tradução italiana desses tutoriais, visite [http://www.lucamorelli.it](http://www.lucamorelli.it).

## <a name="about-the-authors"></a>Sobre os autores

Jason Lee é tecnólogo principal com [conteúdo mestre](http://www.contentmaster.com/) , onde trabalha com produtos e tecnologias da Microsoft, especialmente o SharePoint e o ASP.net, há vários anos. Jason tem um PhD em computação e atualmente credenciais e MCTS certificado. Você pode ler o blog técnico de Jason em [www.jrjlee.com](http://www.jrjlee.com/).

Benjamin curry é tecnólogo principal com [conteúdo mestre](http://www.contentmaster.com/) , que escreveu white papers, documentação do SDK, apresentações do PowerPoint e cursos de treinamento online e ministrados por instrutores durante sua carreira. Um membro original da equipe de documentação do ASP.NET, ele trabalhou com as tecnologias da Web da Microsoft por mais de uma década.

## <a name="target-audience"></a>Público-alvo

Este conjunto de tutoriais é para desenvolvedores de aplicativos Web ASP.NET e arquitetos de soluções que usam o Visual Studio 2010 para criar aplicativos Web de escala empresarial. Para obter o máximo do conteúdo, você deve estar confortável usando o Visual Studio 2010 e ter uma familiaridade básica com o TFS, juntamente com um reconhecimento das tecnologias da plataforma Web da Microsoft como o ASP.NET MVC 3, o Windows Communication Foundation (WCF), o IIS, o SQL E projetos de banco de dados do Visual Studio. No entanto, você não precisa estar familiarizado com as tecnologias e ferramentas de implantação ou precisa saber como configurar os sistemas de CI.

## <a name="requirements"></a>Requisitos

Para seguir as orientações e executar as tarefas que esses tutoriais descrevem, você precisará instalar este software em seu computador de desenvolvimento:

- Visual Studio 2010 Premium ou Ultimate Edition com Service Pack 1
- .NET Framework 4.0
- .NET Framework 3,5 com Service Pack 1
- ASP.NET MVC 3.0
- IIS 7.5 Express
- SQL Server Express 2008 R2

Para executar as etapas de implantação descritas durante essas orientações, você precisará ter acesso aos ambientes de implantação de aplicativo Web de exemplo. Para obter melhores resultados, esses ambientes devem refletir o padrão de implantação empresarial de sua organização. Em seguida, você pode modificar as orientações fornecidas nesta documentação para refletir os ambientes de implantação e os requisitos de sua própria organização.

## <a name="series-contents"></a>Conteúdo da série

Esta seção introdutória consiste em dois tópicos adicionais. Elas foram projetadas para fornecer um contexto mais amplo para os tutoriais a seguir:

- [Enterprise Web Deployment: visão geral do cenário](enterprise-web-deployment-scenario-overview.md). Este tópico descreve o cenário que subfixa cada um dos tutoriais desta série. O cenário se concentra nos requisitos de ALM (gerenciamento do ciclo de vida do aplicativo) de uma empresa fictícia chamada Fabrikam, Inc. à medida que desenvolve um aplicativo Web de escala empresarial.
- [Gerenciamento do ciclo de vida do aplicativo: de desenvolvimento para produção](application-lifecycle-management-from-development-to-production.md). Este tópico fornece uma visão geral de alto nível de ponta a ponta de um processo de implantação. Ele ilustra como a Fabrikam, Inc. move um aplicativo Web ASP.NET de escala empresarial por meio de ambientes de teste, de preparo e de produção como parte de um processo de desenvolvimento contínuo.

A série inclui quatro conjuntos de tutoriais. Cada um concentra-se em diferentes aspectos da implantação da Web:

- [Implantação da Web na empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial fornece uma introdução conceitual aos arquivos de projeto do MSBuild, ao pipeline de publicação na Web, Implantação da Web e outras tecnologias relacionadas. Ele explica como você pode usar essas ferramentas juntas para gerenciar processos de implantação complexos.
- [Configurando ambientes de servidor para implantação na Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial descreve como configurar servidores Windows para oferecer suporte a vários cenários de implantação, incluindo a implantação remota de pacotes da Web usando o serviço de Deployment Agent da Web (o "agente remoto") ou o manipulador de Implantação da Web e a implantação de banco de dados remoto. Ele fornece orientação sobre como escolher o método de implantação apropriado para seu próprio ambiente e descreve como usar o WFF para replicar aplicativos Web implantados em todos os servidores Web em um farm de servidores.
- [Configurando Team Foundation Server para implantação da Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial descreve como configurar o TFS para dar suporte a vários cenários de implantação, incluindo a implantação automatizada como parte de um processo de CI e as implantações disparadas manualmente de compilações específicas.
- [Implantação da Web empresarial avançada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial descreve como realizar várias tarefas de implantação mais avançadas, como personalizar implantações de banco de dados para vários ambientes, excluindo arquivos e pastas da implantação e colocando aplicativos Web offline durante o processo de implantação .

## <a name="where-to-start"></a>Onde começar

Esse conjunto de tutoriais usa uma solução de exemplo com um nível realista de complexidade, junto com um cenário de implantação empresarial fictício, para fornecer uma implementação de referência e dar às tarefas e passo a passos um contexto comum. O próximo tópico, [Enterprise Web Deployment: visão geral do cenário](enterprise-web-deployment-scenario-overview.md), apresenta o cenário e a solução de exemplo. A partir daí, você pode trabalhar com os tutoriais e tópicos que mais atendem às suas necessidades.

> [!div class="step-by-step"]
> [Próximo](enterprise-web-deployment-scenario-overview.md)
