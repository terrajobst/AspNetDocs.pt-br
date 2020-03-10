---
uid: whitepapers/aspnet-web-deployment-content-map
title: Implantação da Web do ASP.NET-recursos recomendados | Microsoft Docs
author: rick-anderson
description: Este tópico fornece links para recursos de documentação sobre como implantar (publicar) aplicativos Web ASP.NET no IIS usando o Visual Studio 2010, Visual Web de...
ms.author: riande
ms.date: 03/14/2014
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 96873c8f2b0ad2415f371aceb651400c801a3338
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636986"
---
# <a name="aspnet-web-deployment---recommended-resources"></a>Implantação da Web do ASP.NET – Recursos recomendados

> Este tópico fornece links para recursos de documentação sobre como implantar (publicar) aplicativos Web ASP.NET no IIS usando o Visual Studio 2010, o Visual Web Developer 2010 e versões posteriores.
> 
> Se você conhece uma ótima postagem no blog, thread [StackOverflow](http://stackoverflow.com) ou qualquer outro link que seria útil, [envie um email](mailto:aspnetue@microsoft.com?subject=Deployment%20Content%20Map) com o link.
> 
> > [!NOTE] 
> > 
> > Muitos desses recursos descrevem os recursos de implantação que estão disponíveis apenas se você instalar uma versão recente da [atualização de publicação na Web do Visual Studio](https://go.microsoft.com/fwlink/?LinkID=208120). Alguns dos recursos estão disponíveis apenas no Visual Studio 2012 ou Visual Studio 2013.

Este tópico contém as seguintes seções:

- [Noções básicas sobre opções de implantação para projetos Web](#understanding)
- [Localizando provedores de hospedagem para um aplicativo ASP.NET](#findinghosting)
- [Implantando um aplicativo Web do Visual Studio](#fromvs)
- [Implantando um aplicativo Web Criando e instalando um pacote de implantação da Web](#package)
- [Implantando um aplicativo Web usando um processo de CI (integração contínua)](#ci)
- [Usando transformações de Web. config para alterar as configurações no arquivo Web. config de destino ou no arquivo app. config durante a implantação](#transforms)
- [Usando parâmetros de Implantação da Web para alterar as configurações no aplicativo Web de destino durante a implantação](#webdeployparms)
- [Verificando se um aplicativo está off-line durante a implantação](#appoffline)
- [Implantando um banco de dados ou alterações em um banco de dados como parte da implantação do aplicativo Web](#databasewithweb)
- [Implantando um banco de dados separadamente da implantação de aplicativo Web](#databaseseparate)
- [Implantando um aplicativo Web que usa serviços de aplicativo ASP.NET, como associação e criação de perfil](#aspnetmembership)
- [Pré-compilando para implantação](#precompiling)
- [Implantando um aplicativo Web de intranet](#intranet)
- [Automatizando tarefas comuns de implantação que não são automatizadas](#automating)
- [Configurando servidores Web para que os desenvolvedores possam implantar aplicativos Web para eles usando Implantação da Web](#configuringservers)
- [Configurando servidores para um provedor de hospedagem](#hostingprovider)
- [Solucionando problemas de implantação](#troubleshooting)
- [Obtendo ajuda com uma pergunta de implantação específica](#gettinghelp)
- [Recursos adicionais](#additional)

<a id="understanding"></a>

## <a name="understanding-deployment-options-for-web-projects"></a>Noções básicas sobre opções de implantação para projetos Web

- [Visão geral da implantação da Web para Visual Studio e ASP.net](https://msdn.microsoft.com/library/dd394698.aspx) (MSDN).
- [Como implantar um site do Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Explica opções e links para recursos de implantação de projetos da Web em sites do Windows Azure, incluindo [entrega contínua](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (automatizada do [controle do código-fonte](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)), bem como usando o Visual Studio.
- [Aprimoramentos na publicação na Web do Visual Studio 2012](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (vídeo de Scott Hanselman).
- [Postagem de visão geral para implantação na Web no VS 2010](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (blog de Vishal Joshi). Uma postagem mais antiga do blog, mas alguns dos recursos do Visual Studio 2010 que ele vincula tem informações que ainda são relevantes para o Visual Studio 2012.

<a id="findinghosting"></a>

## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Localizando provedores de hospedagem para um aplicativo ASP.NET

- [Hospedagem ASP.NET](https://asp.net/hosting)

<a id="fromvs"></a>

## <a name="deploying-a-web-application-from-visual-studio"></a>Implantando um aplicativo Web do Visual Studio

- [Como implantar um site do Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Explica opções e fornece links para recursos para implantar projetos da Web em sites do Windows Azure. Inclui uma seção sobre a implantação do Visual Studio.
- [Implantação da Web do ASP.NET usando o Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). a série de tutoriais de 12 partes, mostra como implantar aplicativos Web com bancos de dados SQL Server. Para a implantação de banco de dados usa o provedor dbDacFx e o Migrações do Entity Framework Code First. Também inclui informações sobre [transformações de arquivo Web. config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), [implantação de arquivos individuais](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), [implantação de linha de comando](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md)e [como personalizar o pipeline de publicação da Web do Visual Studio editando arquivos. pubxml](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). Aplica-se a todos os projetos da Web do ASP.NET, incluindo Web Forms, MVC e API da Web.)
- [Como implantar um projeto Web usando a publicação com um clique no Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (informações de referência para o assistente de publicação da Web do Visual Studio.)
- [Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Esta é uma versão anterior do **ASP.NET Web Deployment usando o Visual Studio** listado na parte superior desta seção. Principalmente útil agora para obter informações sobre como implantar bancos de dados do SQL Server Compact e como migrar do SQL Server Compact para uma edição completa do SQL Server.
- [Aplicativo .net de várias camadas usando tabelas de armazenamento, filas e blobs](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure site). a série de tutoriais de 5 partes, mostra como criar um projeto do MVC e implantá-lo em um serviço de nuvem do Windows Azure.

<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Implantando um aplicativo Web Criando e instalando um pacote de implantação da Web

- [Como: criar um pacote de implantação da Web no Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx) (MSDN).
- [Como instalar um pacote de implantação usando o arquivo Deploy. cmd criado pelo Visual Studio](https://msdn.microsoft.com/library/ff356104.aspx) (MSDN).
- [Usando um pacote implantação da Web para implantar no IIS na caixa dev e em um host](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) de terceiros (blog de sayed Hashimi). Como usar o Gerenciador do IIS para instalar um pacote de implantação no IIS no computador local e em uma empresa de hospedagem que dá suporte ao Gerenciador do IIS para administração remota.
- [Criando um pacote de implantação da Web do Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (site da IIS.net). Inclui instruções para a criação e a instalação do pacote de linha de comando.
- [Pacote depois de publicar em qualquer lugar](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (blog de sayed Hashimi). Apresenta um pacote NuGet que automatiza o processo de transformar o arquivo Web. config para vários ambientes de destino, para que você possa implantar um pacote em vários servidores. Consulte também o [vídeo PackageWeb](https://www.youtube.com/watch?v=-LvUJFI8CzM) por sayed Hashimi.

Consulte também a seção a seguir.

<a id="ci"></a>

## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Implantando um aplicativo Web usando um processo de CI (integração contínua)

- [Integração contínua e entrega contínua (criando aplicativos de nuvem do mundo real com o Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Capítulo de livro eletrônico que introduz a integração contínua e a entrega contínua.
- [Como implantar um site do Windows Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Explica opções e links para recursos para implantar projetos da Web em sites do Windows Azure. Inclui uma seção sobre como automatizar a implantação do controle do código-fonte.
- [Implantação de aplicativos Web em cenários empresariais](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). a série de tutoriais de 40 partes mostra como automatizar a implantação em um processo de CI usando o Visual Studio 2010 e Team Foundation Server 2010.
- [Dentro do Microsoft Build Engine: usando o MSBuild e o Team Foundation Build, por sayed Hashimi e William Bartholomew](http://msbuildbook.com). Este é um livro, não um recurso da Web, mas é um guia essencial para aprender a configurar o MSBuild para cenários de integração contínua.
- [Pacote de extensão do MSBuild](https://github.com/mikefourie/MSBuildExtensionPack). Inclui tarefas de implantação.
- [Guia de personalização do Team Foundation Build](https://aka.ms/vsarsolutions). A documentação do ALM Rangers na configuração de Team Foundation Server aborda a implantação da Web e inclui tutoriais e vídeos.
- [Transformações XML SlowCheetah de um servidor de CI](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (blog de sayed Hashimi). Explica como usar o SlowCheetah, um suplemento do Visual Studio para transformar o app. config e outros arquivos XML.

Consulte também [verificando se um aplicativo está off-line durante a implantação](aspnet-web-deployment-content-map.md#appoffline) mais adiante nesta página.

<a id="transforms"></a>

## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Usando transformações de Web. config para alterar as configurações no arquivo Web. config de destino ou no arquivo app. config durante a implantação

- [Transformações de arquivo Web. config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Sintaxe de transformação Web. config para implantação de projeto Web usando o Visual Studio](https://msdn.microsoft.com/library/dd465326.aspx) (MSDN).
- [Ferramentas da web 2012,2-transformações de Web. config](https://www.youtube.com/watch?v=HdPK8mxpKEI) (vídeo do YouTube por sayed Hashimi). Mostra como configurar e Visualizar transformações de Web. config.
- [Como fazer desabilitar a transformação do Web. config?](https://msdn.microsoft.com/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [Quando devo usar parâmetros de Implantação da Web em vez de transformações de Web. config?](https://msdn.microsoft.com/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [Xdt (transformação de documento XML) lançada no CodePlex.com](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (blog de ferramentas e desenvolvimento Web .net). Anuncia a disponibilidade do código-fonte para o mecanismo de transformação do arquivo Web. config e lista algumas ferramentas que o utilizam.
- [Sites do Windows Azure: como as cadeias de caracteres do aplicativo e as cadeias de conexão funcionam](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (Microsoft Azure blog). Uma alternativa para as transformações de Web. config se o ambiente de destino é de sites do Windows Azure e você deseja transformar `appSettings` ou `connectionStrings`.

<a id="webdeployparms"></a>

## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>Usando parâmetros de Implantação da Web para alterar as configurações no aplicativo Web de destino durante a implantação

- [Como: usar parâmetros de implantação da Web em um pacote de implantação da Web](https://msdn.microsoft.com/library/ff398068.aspx) (MSDN).
- [MSDeploy: como atualizar as configurações do aplicativo na publicação com base no perfil de publicação](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (blog de sayed Hashimi). Mostra como integrar parâmetros de implantação da Web em perfis de publicação do Visual Studio.
- [Implantação da Web parametrização](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (site da IIS.net).
- [Implantação da Web parametrização em ação](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (blog de Vishal Joshi).
- [Implantação da Web parametrização versus a transformação Web. config](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (blog de Vishal Joshi).
- [Sites do Windows Azure: como as cadeias de caracteres do aplicativo e as cadeias de conexão funcionam](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (Microsoft Azure blog). Uma alternativa para os parâmetros de implantação da Web se o seu ambiente de destino é o Windows Azure Web sites e você deseja parametrizar `appSettings` ou `connectionStrings`.

<a id="appoffline"></a>

## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Verificando se um aplicativo está off-line durante a implantação

- [Implantação da Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Consulte a seção **colocar o aplicativo offline durante a implantação.**
- [Colocando um aplicativo offline antes da publicação](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (IIS.net site). Explica um recurso interno do Implantação da Web 3,0 que automatiza o tratamento de um aplicativo\_arquivo. htm offline. Este recurso não funciona com o aplicativo personalizado\_arquivos. htm offline.
- [Como colocar seu aplicativo Web offline durante a publicação](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (blog de sayed Hashimi). Como automatizar o processo de usar um aplicativo personalizado\_arquivo offline. htm.
- [Atualizações de publicação na Web para o aplicativo offline e usechecksum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (blog de desenvolvimento na Web da Microsoft). Outra opção para automatizar o uso do aplicativo\_offline. htm.
- [Implantação da Web 3,5 RTW](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (site IIS.net). O novo recurso no Implantação da Web 3,5 para o aplicativo personalizado\_arquivos. htm offline.

<a id="databasewithweb"></a>

## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Implantando um banco de dados ou alterações em um banco de dados como parte da implantação do aplicativo Web

- [Configurando a implantação de banco de dados no Visual Studio](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment) (MSDN). Visão geral das opções para implantar um banco de dados com um projeto Web.
- [Implantação da Web do ASP.NET usando o Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). série de tutoriais de 12 partes, mostra a implantação de banco de dados usando o provedor dbDacFx e o Migrações do Entity Framework Code First.
- [Como implantar um projeto Web usando a publicação com um clique no Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (MSDN).
- [Implante um aplicativo Secure ASP.NET MVC 5 com associação, OAuth e banco de dados SQL em um site do Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Um tutorial longo que cria e implanta um aplicativo que usa um único banco de dados de SQL Server para associação e os de aplicativos.
- [Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). a série de tutoriais de 12 partes, mostra como implantar SQL Server Compact bancos de dados e como migrar do SQL Server Compact para uma edição completa do SQL Server.

Consulte também implantando um aplicativo Web Criando e instalando um pacote de implantação da Web e implantando um aplicativo Web usando um processo de CI (integração contínua) anteriormente nesta página.

<a id="databaseseparate"></a>

## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Implantando um banco de dados separadamente da implantação de aplicativo Web

- [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686(v=vs.103).aspx) (MSDN).
- [Incluindo dados em um projeto de banco de dado SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (blog da equipe do SQL Server Data Tools). Como implantar o esquema e os dados ao implantar um banco de dado.
- [Como implantar um banco de dados no Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (Microsoft Azure site)
- [Migrando bancos de dados para o Windows Azure SQL Database (anteriormente chamado de SQL Azure)](https://msdn.microsoft.com/library/windowsazure/ee730904.aspx) (MSDN).
- [Migrando um banco de dados para SQL Azure usando o SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (blog da equipe do SQL Server Data Tools).
- [Migrando aplicativos centrados em dados para o Windows Azure](https://msdn.microsoft.com/library/jj156154.aspx) (MSDN).
- [Migração de bancos de dados SQL Server para o banco de dados SQL do Windows Azure](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).

<a id="aspnetmembership"></a>

## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Implantando um aplicativo Web que usa serviços de aplicativo ASP.NET, como associação e criação de perfil

- [Implante um aplicativo Secure ASP.NET MVC 5 com associação, OAuth e banco de dados SQL em um site do Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Um tutorial longo que cria e implanta um aplicativo que usa um único banco de dados de SQL Server para associação e os de aplicativos.
- [ASP.net Identity](https://asp.net/identity/). Recursos para ASP.NET Identity.
- [Implantação da Web do ASP.NET usando o Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). a série de tutoriais de 12 partes, mostra como implantar um banco de dados de associação do ASP.NET.
- [Configuração de um site que usa serviços de aplicativos](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). Para projetos de sites da Web, mas também é relevante para projetos de aplicativos Web.
- [Usuários e funções no site de produção](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). Para projetos de sites da Web, mas também é relevante para projetos de aplicativos Web.

<a id="precompiling"></a>

## <a name="precompiling-for-deployment"></a>Pré-compilando para implantação

- [Visão geral da pré-compilação do projeto de aplicativo Web do ASP.net](https://msdn.microsoft.com/library/aa983464.aspx) (MSDN).
- [Guia pacote/publicar Web, propriedades do projeto](https://msdn.microsoft.com/library/dd410108.aspx) (MSDN).
- [Caixa de diálogo Configurações avançadas de pré-compilação](https://msdn.microsoft.com/library/hh475319.aspx) (MSDN).

<a id="intranet"></a>

## <a name="deploying-an-intranet-web-application"></a>Implantando um aplicativo Web de intranet

- [Use a opção de autenticação organizacional local (ADFS) com ASP.net em Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (blog de Vittorio Bertocci.).
- [Como criar um site de intranet usando o ASP.NET MVC](https://msdn.microsoft.com/library/gg703322(VS.98).aspx) (MSDN). A versão mais antiga do Write-in do Visual Studio 2010 não reflete alterações importantes em modelos de projetos de intranet introduzidos no Visual Studio 2013.

<a id="automating"></a>

## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Automatizando tarefas comuns de implantação que não são automatizadas

- [Implantação da Web do ASP.NET usando o Visual Studio: Implantando arquivos extras](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Definindo permissões de pasta na publicação da Web](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (blog de sayed Hashimi).
- [Como estender o arquivo de destinos para incluir configurações de registro para um pacote de projeto Web](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (blog de ferramentas de desenvolvimento da Web).
- [Estendendo a transformação XML (Web. config)](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (blog de sayed Hashimi). Mostra como criar transformações XDT personalizadas.
- O [provedor personalizado de MSDeploy (ferramenta de implantação da Web) Take 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (blog de sayed Hashimi). Mostra como criar um Implantação da Web provedor personalizado.
- [Como empacotar e implantar componentes com](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (blog de ferramentas de desenvolvimento da Web).
- [Como empacotar assemblies .net](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (blog de ferramentas de desenvolvimento da Web). Como implantar assemblies no GAC.
- [Gerar script de tudo-inicialize sua VM do Windows Azure para seu servidor Web com IIS, implantação da Web e outras coisas](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (blog de Tugberk Ugurlu).

<a id="configuringservers"></a>

## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Configurando servidores Web para que os desenvolvedores possam implantar aplicativos Web para eles usando Implantação da Web

- [Instalação e configuração de implantação da Web para implantações de administrador e não administrador](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (site IIS.net).

<a id="hostingprovider"></a>

## <a name="configuring-servers-for-a-hosting-provider"></a>Configurando servidores para um provedor de hospedagem

- [Guia de implantação de hospedagem do Microsoft ASP.NET 4](https://go.microsoft.com/fwlink/?LinkId=191365) (centro de download da Microsoft).
- [Gerar um arquivo XML de perfil](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (site IIS.net).

<a id="troubleshooting"></a>

## <a name="troubleshooting-deployment-problems"></a>Solucionando problemas de implantação

- [Solução de problemas de sites do Windows Azure no Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (Microsoft Azure site).
- [Implantação da Web do ASP.NET usando o Visual Studio: solução de problemas](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Solução de problemas comuns com o implantação da Web](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Implantação da Web códigos de erro](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (site IIS.net).
- [Perguntas frequentes sobre implantação da Web para Visual Studio e ASP.net](https://msdn.microsoft.com/library/ee942158.aspx) (MSDN).
- [Principais diferenças entre o IIS e o ASP.NET Development Server](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Diferenças de configuração comuns entre desenvolvimento e produção](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Hospedagem de aplicativos ASP.net em confiança média](http://www.4guysfromrolla.com/articles/100307-1.aspx) (4 pessoas do site do Rolla).

<a id="gettinghelp"></a>

## <a name="getting-help-with-a-specific-deployment-question"></a>Obtendo ajuda com uma pergunta de implantação específica

- [Fórum de configuração e implantação do ASP.net](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).

<a id="additional"></a>

## <a name="additional-resources"></a>Recursos adicionais

Esta seção fornece links para recursos adicionais que são úteis para aprender mais sobre como usar o Visual Studio e as ferramentas de implantação do IIS.

Os Blogs a seguir geralmente contêm informações sobre a implantação da Web do Visual Studio:

- [Blog de ferramentas de desenvolvimento para a Web no Microsoft](https://blogs.msdn.com/b/webdevtools/).
- [Blog de sayed Hashimi](http://www.sedodream.com/).

Os recursos a seguir fornecem documentação sobre Implantação da Web, a estrutura do IIS que o Visual Studio usa para executar tarefas de implantação de projeto de aplicativo Web. Você pode fazer perguntas sobre Implantação da Web no [Fórum da ferramenta de implantação da Web](https://go.microsoft.com/fwlink/?LinkId=149411) no site da IIS.net.

- [Introdução ao implantação da Web](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Instalando e Configurando implantação da Web](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [Scripts do PowerShell para automatizar implantação da Web configuração](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Ferramenta de Implantação de Web](https://go.microsoft.com/fwlink/?LinkId=151481). Nó de Sumário de nível superior para obter Implantação da Web documentação no site do TechNet. Inclui informações de referência úteis, mas a maioria das páginas do TechNet não foi atualizada há anos.
- [Namespace Microsoft. Web. Deployment](https://go.microsoft.com/fwlink/?LinkId=148630). A documentação da API não foi atualizada desde a versão 1,0.
- [Blog da equipe de implantação na Web da Microsoft](https://blogs.iis.net/msdeploy/default.aspx).
- [Guia publicar no site da IIS.net](https://www.iis.net/learn/publish).
