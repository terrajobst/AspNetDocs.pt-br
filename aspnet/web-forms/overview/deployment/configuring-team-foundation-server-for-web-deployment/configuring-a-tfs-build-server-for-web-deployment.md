---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Configurando um servidor de compilação do TFS para implantação da Web | Microsoft Docs
author: jrjlee
description: Este tópico descreve como preparar um servidor de compilação Team Foundation Server (TFS) para compilar e implantar suas soluções usando o Team Build e o informativo da Internet...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: b3aaf7234706d149a3c784347528923f662c3511
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78631092"
---
# <a name="configuring-a-tfs-build-server-for-web-deployment"></a>Configuração de um servidor de build do TFS para a implantação da Web

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como preparar um servidor de compilação Team Foundation Server (TFS) para compilar e implantar suas soluções usando o Team Build e a Implantação da Web (ferramenta de implantação da Web) do Serviços de Informações da Internet (IIS).

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no coração desses tutoriais baseia-se na abordagem de arquivo de projeto dividido descrita em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), no qual o processo de compilação é&#x2014;controlado por dois arquivos de projeto, um contendo instruções de Build que se aplicam a todos os ambientes de destino e um contendo configurações específicas de ambiente e de implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto independente do ambiente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Para preparar um servidor de compilação para compilar e implantar suas soluções, você precisará:

- Instalar e configurar o serviço de compilação do TFS.
- Instalar o Visual Studio 2010.
- Instale todos os produtos ou componentes necessários para criar sua solução, como versões do .NET Framework ou MVC ASP.NET.
- Instale o Implantação da Web 2,0 ou posterior.

Este tópico mostrará como executar esses procedimentos ou apontar para outros recursos onde eles existem. As tarefas e as orientações neste tópico pressupõem que:

- Você está começando com uma compilação de servidor limpa que executa o Windows Server 2008 R2 Service Pack 1.
- O servidor está ingressado no domínio com um endereço IP estático.
- Você instalou a camada de aplicativo do TFS em um servidor separado, conforme descrito em [Enterprise Web Deployment: visão geral do cenário](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>Quem executa esses procedimentos?

Na maioria dos casos, um administrador do TFS será responsável por configurar servidores de compilação. Em alguns casos, a equipe de desenvolvedores pode assumir a propriedade de servidores de Build específicos.

## <a name="install-and-configure-the-tfs-build-service"></a>Instalar e configurar o serviço de compilação do TFS

Quando você configura um servidor de compilação, sua primeira tarefa é instalar e configurar o serviço de compilação do TFS. Como parte desse processo, você precisará:

- Instale o serviço de compilação do TFS e configure uma conta de serviço. Qualquer tarefa de compilação, incluindo a implantação, será executada usando a identidade da conta de serviço de compilação.
- Crie um *controlador de compilação* e um ou mais *agentes de compilação*. Cada controlador de compilação gerencia um conjunto de agentes de compilação. Quando você enfileira uma compilação, o controlador de compilação atribui a tarefa de compilação a um agente de compilação disponível. Cada coleção de projetos de equipe no TFS é mapeada para um único controlador de compilação.
- Configure uma pasta de destino para suas saídas de compilação. Este é um compartilhamento de rede. Qualquer saída de compilação, como pacotes de implantação da Web, é enviada para a pasta de destino.

O capítulo [Administração do Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) no MSDN contém todos os recursos necessários para executar essas tarefas:

- Para obter uma visão geral conceitual do Team Foundation Build, incluindo o serviço de compilação, controladores de compilação e agentes de compilação, consulte [noções básicas sobre um sistema de compilação do Team Foundation](https://msdn.microsoft.com/library/dd793166.aspx).
- Para obter informações sobre como instalar e configurar o serviço de compilação, consulte [configurar um computador de compilação](https://msdn.microsoft.com/library/ms181712.aspx).
- Para obter informações sobre como criar controladores de compilação, consulte [criar e trabalhar com um controlador de compilação](https://msdn.microsoft.com/library/ee330987.aspx).
- Para obter informações sobre como criar agentes de compilação, consulte [criar e trabalhar com agentes de compilação](https://msdn.microsoft.com/library/bb399135.aspx).
- Para obter informações sobre como criar e configurar pastas suspensas, consulte [configurar pastas suspensas](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Instalar produtos e componentes necessários

Para habilitar o servidor de compilação para criar suas soluções, você deve instalar todos os produtos, componentes ou assemblies que sua solução requer. Antes de instalar qualquer componente da plataforma da Web, você deve instalar o Visual Studio 2010 (qualquer versão) no servidor de compilação. Isso garante que os arquivos de destino do MSBuild (Microsoft Build Engine principal) e os arquivos de destino do WPP (pipeline de publicação na Web) estejam disponíveis para o serviço de compilação. O instalador do Visual Studio também deve instalar Implantação da Web, que será necessário se você planeja implantar pacotes da Web como parte do processo de compilação.

A melhor maneira de instalar componentes comuns da Web Platform é usar o [Web Platform Installer](https://go.microsoft.com/?linkid=9805118). Isso garante que você esteja instalando a versão mais recente de cada produto e também detecta e instala automaticamente todos os pré-requisitos para cada produto. No caso da solução [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , você deve usar o Web Platform Installer para instalar esses produtos e componentes:

- **.NET Framework 4,0**. Isso é necessário para executar aplicativos que foram criados nesta versão do .NET Framework.
- **Ferramenta de implantação da Web 2,1 ou posterior**. Isso instala Implantação da Web (e seu executável subjacente, MSDeploy. exe) em seu servidor. Como parte desse processo, ele instala e inicia o Web Deployment Agent Service. Esse serviço permite que você implante pacotes da Web de um computador remoto.
- **ASP.NET MVC 3**. Isso instala os assemblies necessários para executar os aplicativos ASP.NET MVC 3.

**Para instalar os produtos e componentes necessários**

1. Instalar o Visual Studio 2010. Quando solicitado a selecionar os recursos a serem instalados, você deve incluir:

    1. Qualquer linguagem de programação que você precise compilar.
    2. Visual Web Developer. Isso garante que os destinos de WPP sejam adicionados ao seu servidor de Build.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Quando a instalação do Visual Studio 2010 for concluída, baixe e instale o [visual studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (se ele ainda não estiver incluído em sua mídia de instalação).

    > [!NOTE]
    > O Visual Studio 2010 Service Pack 1 resolve um bug que pode impedir que o MSBuild localize o executável do MSDeploy.
3. Baixe e inicie o [Web Platform Installer](https://go.microsoft.com/?linkid=9805118).
4. Na parte superior da janela **Web Platform Installer 3,0** , clique em **produtos**.
5. No lado esquerdo da janela, no painel de navegação, clique em **estruturas**.
6. Na linha **Microsoft .NET Framework 4** , se o .NET Framework ainda não estiver instalado, clique em **Adicionar**.

    > [!NOTE]
    > Talvez você já tenha instalado o .NET Framework 4,0 por meio de Windows Update. Se um produto ou componente já estiver instalado, o Web Platform Installer indicará isso substituindo o botão **Adicionar** pelo texto **instalado**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. Na linha **ASP.NET MVC 3 (Visual Studio 2010)** , clique em **Adicionar**.
8. No painel de navegação, clique em **servidor**.
9. Na linha **ferramenta de implantação da Web 2,1** , clique em **Adicionar**.
10. Clique em **Instalar**. O Web Platform Installer mostrará uma lista de produtos&#x2014;em conjunto com as dependências&#x2014;associadas a serem instaladas e solicitará que você aceite os termos de licença.
11. Examine os termos de licença e, se você concordar com os termos, clique em **aceito**.
12. Quando a instalação for concluída, clique em **concluir**e feche a janela **Web Platform Installer 3,0** .

> [!NOTE]
> Se o processo de implantação incluir o uso de ferramentas como VSDBCMD. exe ou SQLCMD. exe, você precisará garantir que elas estejam instaladas em seu servidor de compilação. O VSDBCMD. exe é uma ferramenta do Visual Studio e é normalmente adicionado ao servidor quando você instala o Team Foundation Build. O SQLCMD. exe é uma ferramenta SQL Server. Você pode baixar uma versão autônoma do SQLCMD. exe na página [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) .

## <a name="conclusion"></a>Conclusão

Neste ponto, o servidor de compilação está pronto para começar a criar e implantar seus projetos de aplicativo Web. O próximo tópico, [criando uma definição de compilação que dá suporte à implantação](creating-a-build-definition-that-supports-deployment.md), descreve como criar e configurar uma definição de compilação para controlar quando e como seus projetos são criados e implantados.

## <a name="further-reading"></a>Leitura adicional

Para obter diretrizes mais gerais sobre como trabalhar com o Team Build, consulte [administrando o Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

> [!div class="step-by-step"]
> [Anterior](adding-content-to-source-control.md)
> [Próximo](creating-a-build-definition-that-supports-deployment.md)
