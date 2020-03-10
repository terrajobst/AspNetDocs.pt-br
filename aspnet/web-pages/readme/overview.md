---
uid: web-pages/readme/overview
title: Leiame do WebMatrix | Microsoft Docs
author: rick-anderson
description: Leiame do WebMatrix and Páginas da Web do ASP.NET (Razor) 1,0 versão
ms.author: riande
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: fac53e935860a90d8f2aa96699d56d66ade3a40f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563465"
---
# <a name="webmatrix-readme"></a><span data-ttu-id="55f6f-103">Leiame do WebMatrix</span><span class="sxs-lookup"><span data-stu-id="55f6f-103">WebMatrix Readme</span></span>

<span data-ttu-id="55f6f-104">13 de janeiro de 2011</span><span class="sxs-lookup"><span data-stu-id="55f6f-104">13 January 2011</span></span>

## <a name="contents"></a><span data-ttu-id="55f6f-105">Conteúdo</span><span class="sxs-lookup"><span data-stu-id="55f6f-105">Contents</span></span>

> [!NOTE]
> <span data-ttu-id="55f6f-106">Este Leiame se aplica à versão 1,0 do WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="55f6f-106">This readme applies to the 1.0 release of WebMatrix.</span></span>

- [<span data-ttu-id="55f6f-107">Visão geral</span><span class="sxs-lookup"><span data-stu-id="55f6f-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="55f6f-108">Instalação</span><span class="sxs-lookup"><span data-stu-id="55f6f-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="55f6f-109">Como publicar aplicativos</span><span class="sxs-lookup"><span data-stu-id="55f6f-109">How to Publish Applications</span></span>](#InstructionsForPublishingApplications)
- [<span data-ttu-id="55f6f-110">Alterações e problemas</span><span class="sxs-lookup"><span data-stu-id="55f6f-110">Changes and Issues</span></span>](#ChangesAndIssues)

    - [<span data-ttu-id="55f6f-111">Instalação do WebMatrix 1,0</span><span class="sxs-lookup"><span data-stu-id="55f6f-111">WebMatrix 1.0 Installation</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="55f6f-112">Páginas da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="55f6f-112">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="55f6f-113">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="55f6f-113">WebMatrix</span></span>](#Known_Issues_WebMatrix)
    - [<span data-ttu-id="55f6f-114">IIS Express</span><span class="sxs-lookup"><span data-stu-id="55f6f-114">IIS Express</span></span>](#Known_Issues_IISExpress)
    - [<span data-ttu-id="55f6f-115">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="55f6f-115">SQL Server Compact</span></span>](#Known_Issues_SQLServerCompact)
    - [<span data-ttu-id="55f6f-116">Instalando aplicativos</span><span class="sxs-lookup"><span data-stu-id="55f6f-116">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="55f6f-117">Publicando aplicativos</span><span class="sxs-lookup"><span data-stu-id="55f6f-117">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
- [<span data-ttu-id="55f6f-118">Para obter mais informações</span><span class="sxs-lookup"><span data-stu-id="55f6f-118">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="55f6f-119">Visão geral</span><span class="sxs-lookup"><span data-stu-id="55f6f-119">Overview</span></span>

> <span data-ttu-id="55f6f-120">O Microsoft WebMatrix 1,0 é uma pilha de desenvolvimento na Web gratuita que é instalada em minutos.</span><span class="sxs-lookup"><span data-stu-id="55f6f-120">Microsoft WebMatrix 1.0 is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="55f6f-121">Ele integra um servidor Web com estruturas de banco de dados e programação para criar uma experiência única e integrada.</span><span class="sxs-lookup"><span data-stu-id="55f6f-121">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="55f6f-122">Você pode usar o WebMatrix para simplificar a maneira como você codifica, testa e publica seu próprio site ASP.NET ou PHP, ou pode usar o WebMatrix para iniciar um novo site usando aplicativos de software livre populares, como o DotNetNuke, o Umbraco, o WordPress ou o Joomla.</span><span class="sxs-lookup"><span data-stu-id="55f6f-122">You can use WebMatrix to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="55f6f-123">O WebMatrix usa o mesmo ambiente avançado de servidor Web, mecanismo de banco de dados e estruturas que executará seu site na Internet, o que torna a transição de desenvolvimento para produção tranqüila e contínua.</span><span class="sxs-lookup"><span data-stu-id="55f6f-123">WebMatrix uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>

<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="55f6f-124">Instalação</span><span class="sxs-lookup"><span data-stu-id="55f6f-124">Installation</span></span>

> <span data-ttu-id="55f6f-125">Para instalar o WebMatrix 1,0, você deve primeiro instalar o [Microsoft Web Platform Installer 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="55f6f-125">To install WebMatrix 1.0, you must first install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="55f6f-126">Depois de instalar o Web Platform Installer, você pode usá-lo para instalar o WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="55f6f-126">After you've installed the Web Platform Installer, you can use it to install WebMatrix.</span></span>
> 
> <span data-ttu-id="55f6f-127">Se você tiver problemas durante a instalação, consulte [Solucionando problemas com Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="55f6f-127">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>

<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a><span data-ttu-id="55f6f-128">Como publicar aplicativos</span><span class="sxs-lookup"><span data-stu-id="55f6f-128">How to Publish Applications</span></span>

> <span data-ttu-id="55f6f-129">Consulte [as instruções passo a passo para publicar aplicativos](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="55f6f-129">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>

<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a><span data-ttu-id="55f6f-130">Alterações e problemas</span><span class="sxs-lookup"><span data-stu-id="55f6f-130">Changes and Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a><span data-ttu-id="55f6f-131">Problemas de instalação do WebMatrix 1,0</span><span class="sxs-lookup"><span data-stu-id="55f6f-131">WebMatrix 1.0 Installation Issues</span></span>

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="55f6f-132">Problema: o WebMatrix 1,0 está disponível apenas em plataformas que dão suporte ao Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="55f6f-132">Issue: WebMatrix 1.0 is available only on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="55f6f-133">O .NET Framework versão 4 é necessário para o WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="55f6f-133">The .NET Framework version 4 is required for WebMatrix.</span></span> <span data-ttu-id="55f6f-134">Em determinados casos, o instalador do WebMatrix 1,0 permitirá que você tente instalar o em uma plataforma que não faça parte do conjunto de configuração com suporte.</span><span class="sxs-lookup"><span data-stu-id="55f6f-134">In certain cases, the WebMatrix 1.0 installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="55f6f-135">Em particular, o Windows Vista sem a atualização do SP1 permitirá que você comece a instalação do WebMatrix, mas o componente .NET Framework 4 falhará e bloqueará a instalação.</span><span class="sxs-lookup"><span data-stu-id="55f6f-135">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="55f6f-136">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-136">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-137">Instale o em uma plataforma com suporte, que inclui:</span><span class="sxs-lookup"><span data-stu-id="55f6f-137">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="55f6f-138">Windows 7</span><span class="sxs-lookup"><span data-stu-id="55f6f-138">Windows 7</span></span>
> - <span data-ttu-id="55f6f-139">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="55f6f-139">Windows Server 2008</span></span>
> - <span data-ttu-id="55f6f-140">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="55f6f-140">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="55f6f-141">Windows Vista SP1 ou posterior</span><span class="sxs-lookup"><span data-stu-id="55f6f-141">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="55f6f-142">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="55f6f-142">Windows XP SP3</span></span>
> - <span data-ttu-id="55f6f-143">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="55f6f-143">Windows Server 2003 SP2</span></span>

#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="55f6f-144">Problema: não é possível instalar o WebMatrix 1,0 se o Microsoft Visual Studio 2008 for instalado sem Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="55f6f-144">Issue: Cannot install WebMatrix 1.0 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="55f6f-145">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-145">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-146">Instale o [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) no centro de download da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="55f6f-146">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>

#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="55f6f-147">Problema: alguns assemblies para o SQL Server Compact 4,0 não estão instalados no GAC</span><span class="sxs-lookup"><span data-stu-id="55f6f-147">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="55f6f-148">Os assemblies gerenciados para SQL Server Compact 4,0 não são colocados no GAC (cache de assembly global) quando você instala o SQL Server Compact 4,0 em um computador de 64 bits e o computador tem apenas o perfil de cliente do .NET Framework 3,5 SP1 instalado.</span><span class="sxs-lookup"><span data-stu-id="55f6f-148">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="55f6f-149">Os assemblies gerenciados que não estão instalados no GAC são:</span><span class="sxs-lookup"><span data-stu-id="55f6f-149">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="55f6f-150">*System. Data. SqlServerCe. dll* (provedor ADO.net)</span><span class="sxs-lookup"><span data-stu-id="55f6f-150">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="55f6f-151">*System. Data. SqlServerCe. Entity. dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="55f6f-151">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="55f6f-152">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-152">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-153">Desinstale o SQL Server Compact 4,0.</span><span class="sxs-lookup"><span data-stu-id="55f6f-153">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="55f6f-154">Baixe e instale a versão completa do .NET Framework 3,5 SP1 no seguinte local:</span><span class="sxs-lookup"><span data-stu-id="55f6f-154">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="55f6f-155">Microsoft .NET Framework 3,5 Service Pack 1 (pacote completo)</span><span class="sxs-lookup"><span data-stu-id="55f6f-155">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="55f6f-156">Em seguida, reinstale o SQL Server Compact 4,0.</span><span class="sxs-lookup"><span data-stu-id="55f6f-156">Then reinstall SQL Server Compact 4.0.</span></span>

#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="55f6f-157">Problema: não é possível desinstalar SQL Server Compact usando a linha de comando</span><span class="sxs-lookup"><span data-stu-id="55f6f-157">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="55f6f-158">A desinstalação do SQL Server Compact usando opções de linha de comando não funciona nesta versão.</span><span class="sxs-lookup"><span data-stu-id="55f6f-158">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="55f6f-159">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-159">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-160">Use *programas e recursos* no painel de controle do Windows para desinstalar o Microsoft SQL Server Compact 4,0.</span><span class="sxs-lookup"><span data-stu-id="55f6f-160">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>

<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="55f6f-161">Páginas da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="55f6f-161">ASP.NET Web Pages</span></span>

<span data-ttu-id="55f6f-162">Esta seção do documento descreve os novos recursos, as alterações e os problemas conhecidos com a versão 1,0 do Páginas da Web do ASP.NET com sintaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="55f6f-162">This section of the document describes new features, changes, and known issues with the 1.0 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="55f6f-163">Novos recursos</span><span class="sxs-lookup"><span data-stu-id="55f6f-163">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="55f6f-164">Alterações</span><span class="sxs-lookup"><span data-stu-id="55f6f-164">Changes</span></span>](#Changes)
- [<span data-ttu-id="55f6f-165">Problemas</span><span class="sxs-lookup"><span data-stu-id="55f6f-165">Issues</span></span>](#Issues)

#### <a id="NewFeatures"></a><span data-ttu-id="55f6f-166">Novos recursos</span><span class="sxs-lookup"><span data-stu-id="55f6f-166">New Features</span></span>

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a><span data-ttu-id="55f6f-167">Novo: definição de configuração adicionada para desabilitar o Gerenciador de pacotes</span><span class="sxs-lookup"><span data-stu-id="55f6f-167">New: Configuration setting added to disable the package manager</span></span>

> <span data-ttu-id="55f6f-168">Uma nova chave de `asp:AdminManagerEnabled` está disponível para o elemento `<appSettings>` no arquivo *Web. config* , o que permite desabilitar completamente o Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="55f6f-168">A new `asp:AdminManagerEnabled` key is available for the `<appSettings>` element in the *web.config* file, which lets you completely disable the package manager.</span></span> <span data-ttu-id="55f6f-169">O valor padrão para esse elemento é true, o que significa que, se ele não estiver incluído no arquivo *Web. config* , o Gerenciador de pacotes será habilitado.</span><span class="sxs-lookup"><span data-stu-id="55f6f-169">The default value for this element is true, meaning that if it is not included in the *web.config* file, the package manager is enabled.</span></span> <span data-ttu-id="55f6f-170">Para desabilitar o Gerenciador de pacotes, adicione o seguinte elemento ao arquivo *Web. config* na raiz do site:</span><span class="sxs-lookup"><span data-stu-id="55f6f-170">To disable the package manager, add the following element to the *web.config* file in the root of the website:</span></span>
> 
> [!code-xml[Main](overview/samples/sample1.xml)]

#### <a id="Changes"></a><span data-ttu-id="55f6f-171">For</span><span class="sxs-lookup"><span data-stu-id="55f6f-171">Changes</span></span>

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a><span data-ttu-id="55f6f-172">Alteração: a chave "webpages: AdminFolderVirtualPath" foi renomeada para "ASP: AdminFolderVirtualPath"</span><span class="sxs-lookup"><span data-stu-id="55f6f-172">Change: "webPages:AdminFolderVirtualPath" key renamed to "asp:AdminFolderVirtualPath"</span></span>

> <span data-ttu-id="55f6f-173">A chave de `webPages:AdminFolderVirtualPath` que pode ser adicionada ao arquivo *Web. config* para especificar o local do Gerenciador de pacotes foi renomeada para usar o namespace `asp:` em vez do namespace `webPages`.</span><span class="sxs-lookup"><span data-stu-id="55f6f-173">The `webPages:AdminFolderVirtualPath` key that can be added to the *web.config* file to specify the location of the package manager has been renamed to use the `asp:` namespace instead of the `webPages` namespace.</span></span> <span data-ttu-id="55f6f-174">Se você tiver usado esse elemento, deverá renomeá-lo no arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="55f6f-174">If you have used this element, you must rename it in the configuration file.</span></span>

#### <a id="Issues"></a>  <span data-ttu-id="55f6f-175">Problemas Conhecidos</span><span class="sxs-lookup"><span data-stu-id="55f6f-175">Known Issues</span></span>

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a><span data-ttu-id="55f6f-176">Problema: senhas para usuários da associação não são mais reconhecidas</span><span class="sxs-lookup"><span data-stu-id="55f6f-176">Issue: Passwords for membership users no longer recognized</span></span>

> <span data-ttu-id="55f6f-177">O algoritmo para criar e armazenar senhas de associação (logon) foi alterado para ser mais seguro.</span><span class="sxs-lookup"><span data-stu-id="55f6f-177">The algorithm for creating and storing membership (login) passwords has been changed to be more secure.</span></span> <span data-ttu-id="55f6f-178">Como resultado, as senhas armazenadas para Membros (usuários) criadas em versões beta do ASP.NET Razor não serão reconhecidas.</span><span class="sxs-lookup"><span data-stu-id="55f6f-178">As a result, the passwords stored for members (users) created in Beta versions of ASP.NET Razor will not be recognized.</span></span> 
> 
> <span data-ttu-id="55f6f-179">**Solução alternativa** Se o site ainda não tiver sido colocado em produção, remova os registros de usuário do banco de dados de associação.</span><span class="sxs-lookup"><span data-stu-id="55f6f-179">**Workaround** If the site has not yet been put into production, remove the user records from the membership database.</span></span> <span data-ttu-id="55f6f-180">Se o banco de dados estiver ativo, gere programaticamente as senhas existentes no banco de dados de associação.</span><span class="sxs-lookup"><span data-stu-id="55f6f-180">If database is live, programmatically regenerate existing passwords in the membership database.</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="55f6f-181">Problema: comportamento inesperado ao usar uma tabela de usuário personalizada para associação</span><span class="sxs-lookup"><span data-stu-id="55f6f-181">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="55f6f-182">Para inicializar o provedor de associação para um site do Razor do ASP.NET, chame o método `WebSecurity.InitializeDatabaseConnection`.</span><span class="sxs-lookup"><span data-stu-id="55f6f-182">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="55f6f-183">(No WebMatrix, o modelo de site inicial inclui uma chamada para esse método no arquivo *\_AppStart. cshtml* ). Se o parâmetro `autoCreateTables` desse método for definido como true (por padrão, ele será definido como true no modelo de site inicial) e, se um nome de tabela não reconhecido for passado para o método (o segundo parâmetro), o método não gerará um erro.</span><span class="sxs-lookup"><span data-stu-id="55f6f-183">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="55f6f-184">Em vez disso, ele cria automaticamente a tabela.</span><span class="sxs-lookup"><span data-stu-id="55f6f-184">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="55f6f-185">Isso pode ser um problema se você pretender usar uma tabela de usuário personalizada para associação, mas passar o nome de tabela incorreto para o método `WebSecurity.InitializeDatabaseConnection`.</span><span class="sxs-lookup"><span data-stu-id="55f6f-185">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="55f6f-186">Como o método não gera, por padrão, um erro se a tabela especificada não existir e, em vez disso, criar uma nova tabela, o aplicativo poderá parecer estar funcionando.</span><span class="sxs-lookup"><span data-stu-id="55f6f-186">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="55f6f-187">No entanto, o código do aplicativo que depende da sua tabela de usuário personalizada (e em campos) pode eventualmente falhar com erros inesperados.</span><span class="sxs-lookup"><span data-stu-id="55f6f-187">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="55f6f-188">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-188">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-189">Verifique se o nome passado no método `InitializeDatabaseConnection` corresponde à tabela de perfil de usuário no banco de dados de associação ou certifique-se de que o parâmetro `autoCreateTables` está definido como false.</span><span class="sxs-lookup"><span data-stu-id="55f6f-189">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>

#### <a name="issue-error-message-the-admin-module-requires-access-to-app_data"></a><span data-ttu-id="55f6f-190">Problema: mensagem de erro "o módulo de administrador requer acesso a ~/app dados de\_"</span><span class="sxs-lookup"><span data-stu-id="55f6f-190">Issue: Error message "The Admin Module requires access to ~/App\_Data"</span></span>

> <span data-ttu-id="55f6f-191">Em algumas circunstâncias, tentar criar usuários ou, de outra forma, trabalhar com o sistema de associação ASP.NET pode fazer com que a página exiba o erro *o módulo de administração requer acesso a ~/App\_dados*.</span><span class="sxs-lookup"><span data-stu-id="55f6f-191">Under some circumstances, trying to create users or otherwise work with the ASP.NET membership system can cause the page to display the error *The Admin Module requires access to ~/App\_Data*.</span></span> <span data-ttu-id="55f6f-192">Isso ocorre se a conta em que o IIS ou o IIS Express está sendo executado não tem permissões para criar e gravar na pasta de *dados do aplicativo\_* na raiz do site.</span><span class="sxs-lookup"><span data-stu-id="55f6f-192">This occurs if the account that IIS or IIS Express is running under does not have permissions to create and write to the *App\_Data* folder under the website root.</span></span> 
> 
> <span data-ttu-id="55f6f-193">**Solução alternativa** Crie manualmente uma pasta de *dados de\_de aplicativo* para o site.</span><span class="sxs-lookup"><span data-stu-id="55f6f-193">**Workaround** Manually create an *App\_Data* folder for the website.</span></span> <span data-ttu-id="55f6f-194">Em seguida, verifique se a conta do Windows em que o aplicativo é executado (normalmente serviço de rede) tem permissões de leitura/gravação para pastas raiz do aplicativo e para subpastas como dados de\_de aplicativo.</span><span class="sxs-lookup"><span data-stu-id="55f6f-194">Then make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as App\_Data.</span></span> <span data-ttu-id="55f6f-195">Informações mais detalhadas estão disponíveis nos problemas do artigo da base de conhecimento [com SQL Server Express instanciação de usuário e projetos de aplicativo Web ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="55f6f-195">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="55f6f-196">Problema: erro "falha ao gerar uma instância de usuário do SQL Server"</span><span class="sxs-lookup"><span data-stu-id="55f6f-196">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="55f6f-197">Se um aplicativo Web do WebMatrix usar SQL Server Express e estiver executando o IIS 7,5 no Windows 7 ou no Windows Server 2008 R2, você poderá ver um erro que indica que SQL Server não é possível recuperar o caminho do aplicativo local do usuário em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="55f6f-197">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="55f6f-198">**Solução alternativa** Verifique se a conta do Windows em que o aplicativo é executado (normalmente serviço de rede) tem permissões de leitura/gravação para pastas raiz do aplicativo e para subpastas como *dados de\_de aplicativo*.</span><span class="sxs-lookup"><span data-stu-id="55f6f-198">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="55f6f-199">Informações mais detalhadas estão disponíveis nos problemas do artigo da base de conhecimento [com SQL Server Express instanciação de usuário e projetos de aplicativo Web ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="55f6f-199">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>

#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a><span data-ttu-id="55f6f-200">Problema: os arquivos que contêm recursos do Gerenciador de pacotes ou senhas do Gerenciador de pacotes podem ser servveis no IIS 6,0 e versões anteriores</span><span class="sxs-lookup"><span data-stu-id="55f6f-200">Issue: Files that contains package-manager resources or package-manager passwords are servable under IIS 6.0 and earlier</span></span>

> <span data-ttu-id="55f6f-201">Se você implantar um aplicativo Páginas da Web do ASP.NET (Razor) que foi criado usando a versão RC2, e se o aplicativo contiver um arquivo *password. txt* ou *PackageSources. txt* em */app\_data/admin*, o IIS 6,0 servirá o arquivo se solicitado, expondo potencialmente as senhas para a instância do Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="55f6f-201">If you deploy an ASP.NET Web Pages (Razor) application that was built using the RC2 release, and if the application contains a *password.txt* or *packagesources.txt* file under */App\_Data/admin*, IIS 6.0 will serve the file if requested, potentially exposing the passwords for your package manager instance.</span></span> 
> 
> <span data-ttu-id="55f6f-202">**Solução alternativa** Renomeie o arquivo *password. txt* ou *PackageSources. txt* para *password. config* ou *PackageSources. config*. Por padrão, o IIS 6,0 não atende aos arquivos que têm a extensão *. config* .</span><span class="sxs-lookup"><span data-stu-id="55f6f-202">**Workaround** Rename the *password.txt* or *packagesources.txt* file to *password.config* or *packagesources.config*. By default, IIS 6.0 does not serve files that have the *.config* extension.</span></span> <span data-ttu-id="55f6f-203">(No IIS 7, nenhum arquivo na pasta de *dados de\_de aplicativo* é servido, portanto, não é necessário renomear os arquivos.)</span><span class="sxs-lookup"><span data-stu-id="55f6f-203">(In IIS 7, no files in the *App\_Data* folder are served, so you do not need to rename the files.)</span></span>

#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a><span data-ttu-id="55f6f-204">Problema: a desinstalação de pacotes instalados usando a versão beta 3 não remove completamente os componentes do pacote</span><span class="sxs-lookup"><span data-stu-id="55f6f-204">Issue: Uninstalling packages installed using the Beta 3 release does not completely remove package components</span></span>

> <span data-ttu-id="55f6f-205">Se você instalou um pacote usando o Gerenciador de pacotes na versão beta 3 e, em seguida, tentar desinstalá-lo usando a versão atual, o pacote não será completamente desinstalado.</span><span class="sxs-lookup"><span data-stu-id="55f6f-205">If you installed a package using the package manager in the Beta 3 release and then try to uninstall it using the current release, the package is not completely uninstalled.</span></span> <span data-ttu-id="55f6f-206">Usar o botão **desinstalar** do Gerenciador de pacotes remove alguns componentes, mas deixa o código da biblioteca do pacote e não atualiza o arquivo *Package. config* .</span><span class="sxs-lookup"><span data-stu-id="55f6f-206">Using the package manager's **Uninstall** button removes some components, but leaves the package's library code and does not update the *package.config* file.</span></span>
> 
> <span data-ttu-id="55f6f-207">**Solução alternativa** </span><span class="sxs-lookup"><span data-stu-id="55f6f-207">**Workaround** </span></span>  
> <span data-ttu-id="55f6f-208">Execute estas etapas:</span><span class="sxs-lookup"><span data-stu-id="55f6f-208">Perform these steps:</span></span>  
> 1. <span data-ttu-id="55f6f-209">Exclua o *aplicativo\_pasta Data\packages*</span><span class="sxs-lookup"><span data-stu-id="55f6f-209">Delete the *App\_Data\packages* folder.</span></span> <span data-ttu-id="55f6f-210">Isso remove todos os pacotes.</span><span class="sxs-lookup"><span data-stu-id="55f6f-210">This removes all packages.</span></span>   
> 2. <span data-ttu-id="55f6f-211">Exclua o arquivo *Packages. config* na raiz do site.</span><span class="sxs-lookup"><span data-stu-id="55f6f-211">Delete the *packages.config* file in the root of the website.</span></span>

#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a><span data-ttu-id="55f6f-212">Problema: no Visual Studio, invocar o Gerenciador de pacotes baseado na Web coloca o aplicativo offline</span><span class="sxs-lookup"><span data-stu-id="55f6f-212">Issue: In Visual Studio, invoking the web-based package manager takes the application offline</span></span>

> <span data-ttu-id="55f6f-213">Se você estiver trabalhando no Visual Studio (não no WebMatrix) e usar a funcionalidade de *Administração de\_* para iniciar o Gerenciador de pacotes, o Visual Studio coloca o aplicativo offline e posta o *aplicativo\_offline. htm* na raiz do site, o que interrompe a sua capacidade de usar o Gerenciador de pacotes.</span><span class="sxs-lookup"><span data-stu-id="55f6f-213">If you are working in Visual Studio (not WebMatrix) and use the *\_admin* functionality to start the package manager, Visual Studio takes the application offline and posts the *app\_offline.htm* into the website root, which disrupts your ability to use the package manager.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="55f6f-214">Embora você normalmente Veja esse comportamento ao usar a interface do Gerenciador de pacotes baseada na Web, o mesmo comportamento ocorrerá se você adicionar, remover ou modificar todos os arquivos na pasta de *dados do\_de aplicativos* .</span><span class="sxs-lookup"><span data-stu-id="55f6f-214">Although you would most typically see this behavior when using the web-based package manager interface, the same behavior occurs if you add, remove, or modify any files in the *App\_Data* folder.</span></span>
> 
> <span data-ttu-id="55f6f-215">**Solução alternativa** </span><span class="sxs-lookup"><span data-stu-id="55f6f-215">**Workaround** </span></span>  
> <span data-ttu-id="55f6f-216">Para trabalhar com pacotes no Visual Studio, use a extensão NuGet em vez do Gerenciador de pacotes baseado na Web.</span><span class="sxs-lookup"><span data-stu-id="55f6f-216">To work with packages in Visual Studio, use the NuGet extension instead of the web-based package manager.</span></span> <span data-ttu-id="55f6f-217">Para obter informações, consulte a [documentação do NuGet](https://docs.microsoft.com/nuget/).</span><span class="sxs-lookup"><span data-stu-id="55f6f-217">For information, see the [NuGet documentation](https://docs.microsoft.com/nuget/).</span></span> <span data-ttu-id="55f6f-218">Se você estiver trabalhando com outros arquivos na pasta de *dados do\_de aplicativos* , considere manter os arquivos em outro lugar para evitar esse problema.</span><span class="sxs-lookup"><span data-stu-id="55f6f-218">If you are working with other files in the *App\_Data* folder, consider keeping the files elsewhere to avoid this issue.</span></span> <span data-ttu-id="55f6f-219">Se isso não for prático, exclua o arquivo *offline. htm do aplicativo\_* manualmente ou aguarde até que o site volte a ficar online automaticamente (por padrão, após 30 segundos).</span><span class="sxs-lookup"><span data-stu-id="55f6f-219">If that's not practical, delete the *app\_offline.htm* file manually or wait until the site comes back online automatically (by default, after 30 seconds).</span></span>

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="55f6f-220">Problema: Visual Studio IntelliSense e modelos de projeto disponíveis somente no ASP.NET MVC versão 3</span><span class="sxs-lookup"><span data-stu-id="55f6f-220">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="55f6f-221">A instalação do Páginas da Web do ASP.NET não também instala ferramentas para o Visual Studio, como IntelliSense e modelos de projeto para Páginas da Web do ASP.NET aplicativos.</span><span class="sxs-lookup"><span data-stu-id="55f6f-221">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="55f6f-222">**Solução alternativa** Para usar o IntelliSense e modelos de projeto para Páginas da Web do ASP.NET aplicativos no Visual Studio, instale o ASP.NET MVC 3 RC por meio do Web Platform Installer ou do [instalador](https://go.microsoft.com/fwlink/?LinkID=191797)autônomo.</span><span class="sxs-lookup"><span data-stu-id="55f6f-222">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="55f6f-223">Problema: lendo feeds ou outros dados externos por meio de um servidor proxy</span><span class="sxs-lookup"><span data-stu-id="55f6f-223">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="55f6f-224">Se o servidor que está executando o site estiver protegido por um servidor proxy, talvez seja necessário configurar as informações de proxy no arquivo *Web. config* para poder ler as informações provenientes de fora do site.</span><span class="sxs-lookup"><span data-stu-id="55f6f-224">If the server running the site is behind a proxy server, you might need to configure proxy information in the *web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="55f6f-225">Por exemplo, se você usar o auxiliar de `ReCaptcha`, o auxiliar se comunicará com o serviço reCAPTCHA, mas poderá ser bloqueado pelo servidor proxy.</span><span class="sxs-lookup"><span data-stu-id="55f6f-225">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="55f6f-226">Da mesma forma, os feeds usados no Páginas da Web do ASP.NET, como o feed usado pelo Gerenciador de pacotes, podem exigir a configuração de proxy.</span><span class="sxs-lookup"><span data-stu-id="55f6f-226">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="55f6f-227">Se você tiver problemas ao trabalhar com um serviço externo ou trabalhar com o feed de pacote, coloque os seguintes elementos no arquivo *Web. config* da raiz do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="55f6f-227">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *web.config* file:</span></span>
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> <span data-ttu-id="55f6f-228">Para obter mais informações sobre como configurar um servidor proxy, consulte [&lt;elemento&gt; proxy (configurações de rede)](https://msdn.microsoft.com/library/sa91de1e.aspx) no site do MSDN.</span><span class="sxs-lookup"><span data-stu-id="55f6f-228">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="55f6f-229">Problema: a desinstalação do .NET Framework versão 4 desabilita Páginas da Web do ASP.NET com a sintaxe do Razor</span><span class="sxs-lookup"><span data-stu-id="55f6f-229">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="55f6f-230">Se você desinstalar o .NET Framework versão 4 e, em seguida, reinstalá-lo, Páginas da Web do ASP.NET com sintaxe Razor será desabilitado.</span><span class="sxs-lookup"><span data-stu-id="55f6f-230">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="55f6f-231">As páginas com a extensão *. cshtml* não são executadas corretamente.</span><span class="sxs-lookup"><span data-stu-id="55f6f-231">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="55f6f-232">Páginas da Web do ASP.NET registra um assembly no arquivo *Web. config da* raiz do computador e a remoção do .NET Framework remove esse arquivo.</span><span class="sxs-lookup"><span data-stu-id="55f6f-232">ASP.NET Web Pages registers an assembly in the machine root *web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="55f6f-233">Reinstalar o .NET Framework instala uma nova versão do arquivo de configuração, mas não adiciona a referência para o assembly Páginas da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="55f6f-233">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="55f6f-234">**Solução alternativa** Depois de reinstalar o .NET Framework, reinstale o Páginas da Web do ASP.NET com sintaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="55f6f-234">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="55f6f-235">Isso adiciona o seguinte elemento ao arquivo *Web. config* na raiz do computador, que normalmente está no seguinte local:</span><span class="sxs-lookup"><span data-stu-id="55f6f-235">This adds the following element to the *web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]

#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="55f6f-236">Problema: URLs sem extensão não localizam arquivos. cshtml/. vbhtml no IIS 7 ou IIS 7,5</span><span class="sxs-lookup"><span data-stu-id="55f6f-236">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="55f6f-237">No IIS 7 ou IIS 7,5, as solicitações com uma URL como esta não são capazes de localizar páginas que tenham a extensão *. cshtml* ou *. vbhtml* :</span><span class="sxs-lookup"><span data-stu-id="55f6f-237">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="55f6f-238">O problema ocorre porque a regravação de URL não está habilitada por padrão para o IIS 7 ou o IIS 7,5.</span><span class="sxs-lookup"><span data-stu-id="55f6f-238">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="55f6f-239">O cenário likeliest é que você não vê o problema ao testar localmente usando IIS Express, mas você o experimenta quando implanta seu site em um site de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="55f6f-239">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="55f6f-240">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-240">**Workaround**</span></span>
> 
> - <span data-ttu-id="55f6f-241">Se você tiver controle sobre o computador servidor, no computador servidor, instale a atualização descrita em [uma atualização disponível que permite que determinados manipuladores do iis 7,0 ou iis 7,5 manipulem solicitações cujas URLs não terminam com um ponto](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="55f6f-241">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="55f6f-242">Se você não tiver controle sobre o computador servidor (por exemplo, você está implantando em um site de hospedagem), adicione o seguinte ao arquivo *Web. config* do site:</span><span class="sxs-lookup"><span data-stu-id="55f6f-242">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *web.config* file:</span></span> 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="55f6f-243">Problema: Implantando um aplicativo em um computador que não tem SQL Server Compact instalado</span><span class="sxs-lookup"><span data-stu-id="55f6f-243">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="55f6f-244">Os aplicativos que incluem bancos de dados do SQL Server Compact podem ser executados em um computador onde o SQL Server Compact não está instalado.</span><span class="sxs-lookup"><span data-stu-id="55f6f-244">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="55f6f-245">O Microsoft WebMatrix 1,0 copia automaticamente esses binários para você e executa as transformações de arquivo *Web. config* apropriadas.</span><span class="sxs-lookup"><span data-stu-id="55f6f-245">Microsoft WebMatrix 1.0 automatically copies these binaries for you and performs the appropriate *web.config* file transforms.</span></span>
> 
> <span data-ttu-id="55f6f-246">**Solução alternativa** Se você precisar copiar esses arquivos e fazer com que o arquivo *Web. config* seja alterado manualmente, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="55f6f-246">**Workaround** If you need to copy these files and make the *web.config* file changes manually, do the following:</span></span>
> 
> 1. <span data-ttu-id="55f6f-247">Copie os assemblies do mecanismo de banco de dados para a pasta *bin* (e subpastas) do aplicativo no computador de destino:</span><span class="sxs-lookup"><span data-stu-id="55f6f-247">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span>  
> 
>    - <span data-ttu-id="55f6f-248">Copie *c:\Arquivos de programas\microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span><span class="sxs-lookup"><span data-stu-id="55f6f-248">Copy *C:\Program Files\Microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* </span></span>  
>      <span data-ttu-id="55f6f-249">**para** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="55f6f-249">**to** *\Bin*</span></span>
>    - <span data-ttu-id="55f6f-250">Copie *c:\Arquivos de programas\microsoft SQL Server Compact Edition\v4.0\Private\x86\\* **para** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="55f6f-250">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\* **to** *\Bin\x86*</span></span>
>    - <span data-ttu-id="55f6f-251">Copie *c:\Arquivos de programas\microsoft SQL Server Compact Edition\v4.0\Private\amd64\\* \* **para** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="55f6f-251">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 
> 2. <span data-ttu-id="55f6f-252">Na pasta raiz do site, crie ou abra um arquivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="55f6f-252">In the root folder of the website, create or open a *web.config* file.</span></span> <span data-ttu-id="55f6f-253">(No WebMatrix 1,0, esse tipo de arquivo estará disponível se você clicar em **tudo** na caixa de diálogo **escolher um tipo de arquivo** .)</span><span class="sxs-lookup"><span data-stu-id="55f6f-253">(In WebMatrix 1.0, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="55f6f-254">Adicione o seguinte elemento como um filho do elemento `<configuration>` (não dentro do elemento `<system.web>`):</span><span class="sxs-lookup"><span data-stu-id="55f6f-254">Add the following element as a child of the `<configuration>` element (not inside the `<system.web>` element):</span></span>
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="55f6f-255">Problema: os auxiliares "banco de dados" e "WebGrid" não funcionam em confiança média em Visual Basic</span><span class="sxs-lookup"><span data-stu-id="55f6f-255">Issue: "Database" and "WebGrid" helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="55f6f-256">Se você estiver usando Visual Basic (criando arquivos *. vbhtml* ), os auxiliares `Database` e `WebGrid` não funcionarão se o aplicativo for definido para usar a confiança média.</span><span class="sxs-lookup"><span data-stu-id="55f6f-256">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="55f6f-257">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-257">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-258">Se você usar o Visual Studio 2010, poderá resolver esse problema instalando a versão do Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="55f6f-258">If you use Visual Studio 2010, you can resolve this problem by installing the Service Pack 1 release.</span></span> <span data-ttu-id="55f6f-259">Até que a versão final da versão SP1 esteja disponível, você pode baixar a versão beta do SP1 na página [Microsoft Visual Studio 2010 Service Pack 1 beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) no centro de download da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="55f6f-259">Until the final version of the SP1 release is available, you can download the Beta version of SP1 from the [Microsoft Visual Studio 2010 Service Pack 1 Beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) page on the Microsoft Download Center.</span></span>   
>   
> <span data-ttu-id="55f6f-260">Se isso não for prático ou se você não usar o Visual Studio 2010, poderá definir temporariamente o aplicativo para usar a confiança total.</span><span class="sxs-lookup"><span data-stu-id="55f6f-260">If this is not practical, or if you do not use Visual Studio 2010, you can temporarily set the application to use Full Trust.</span></span>

#### <a name="issue-applicationpart-resources-are-externally-accessible"></a><span data-ttu-id="55f6f-261">Problema: os recursos "ApplicationPart" são externamente acessíveis</span><span class="sxs-lookup"><span data-stu-id="55f6f-261">Issue: "ApplicationPart" resources are externally accessible</span></span>

> <span data-ttu-id="55f6f-262">Se um assembly contiver objetos que derivam da classe `ApplicationPart`, os recursos do assembly serão expostos pela classe `ResourceRouteHandler`.</span><span class="sxs-lookup"><span data-stu-id="55f6f-262">If an assembly contains objects that derives from the `ApplicationPart` class, that assembly's resources are exposed by the `ResourceRouteHandler` class.</span></span> <span data-ttu-id="55f6f-263">Por exemplo, considere a seguinte URL:</span><span class="sxs-lookup"><span data-stu-id="55f6f-263">For example, consider the following URL:</span></span>  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> <span data-ttu-id="55f6f-264">Essa solicitação baixa todas as cadeias de caracteres de recurso no assembly *System. Web. webpages. Administration. dll* .</span><span class="sxs-lookup"><span data-stu-id="55f6f-264">This request downloads all of the resource strings in the *System.Web.WebPages.Administration.dll* assembly.</span></span> <span data-ttu-id="55f6f-265">Todos os recursos incorporados (mesmo aqueles que não devem ser servidos como conteúdo estático) são baixados.</span><span class="sxs-lookup"><span data-stu-id="55f6f-265">All of the embedded resources (even those that are not intended to be served as static content) are downloaded.</span></span> <span data-ttu-id="55f6f-266">Se os recursos inseridos contiverem informações confidenciais, isso poderá representar um risco à segurança.</span><span class="sxs-lookup"><span data-stu-id="55f6f-266">If the embedded resources contain sensitive information, this can represent a security risk.</span></span> 
> 
> <span data-ttu-id="55f6f-267">**Solução alternativa** </span><span class="sxs-lookup"><span data-stu-id="55f6f-267">**Workaround** </span></span>  
> <span data-ttu-id="55f6f-268">Se você criar um objeto **ApplicationPart** , certifique-se de que os recursos inseridos associados ao assembly do objeto **ApplicationPart** não contenham informações confidenciais.</span><span class="sxs-lookup"><span data-stu-id="55f6f-268">If you create an **ApplicationPart** object, make sure that the embedded resources associated with that **ApplicationPart** object's assembly do not contain sensitive information.</span></span>

<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a><span data-ttu-id="55f6f-269">WebMatrix</span><span class="sxs-lookup"><span data-stu-id="55f6f-269">WebMatrix</span></span>

> [!NOTE]
> <span data-ttu-id="55f6f-270">Para obter informações sobre problemas de instalação do WebMatrix, consulte [problemas de instalação do WebMatrix](#Known_Issues_Installation) anteriormente neste documento.</span><span class="sxs-lookup"><span data-stu-id="55f6f-270">For information about installation issues for WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

<span data-ttu-id="55f6f-271">Esta seção do documento descreve problemas conhecidos do ambiente de desenvolvimento do WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="55f6f-271">This section of the document describes known issues for the WebMatrix development environment.</span></span>

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a><span data-ttu-id="55f6f-272">Problema: alterações no nome de usuário ou senha de uma cadeia de conexão de banco de dados em um arquivo Web. config não são refletidas no espaço de trabalho bancos</span><span class="sxs-lookup"><span data-stu-id="55f6f-272">Issue: Changes in the username or password of a database connection string in a web.config file are not reflected in the Databases workspace</span></span>

> <span data-ttu-id="55f6f-273">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-273">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="55f6f-274">No arquivo *Web. config* , altere o nome do banco de dados na cadeia de conexão (por exemplo, adicione "1" a ele).</span><span class="sxs-lookup"><span data-stu-id="55f6f-274">In the *web.config* file, change the database name in the connection string (for example, add "1" to it).</span></span>
> 2. <span data-ttu-id="55f6f-275">Salve o arquivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="55f6f-275">Save the *web.config* file.</span></span>
> 3. <span data-ttu-id="55f6f-276">Clique em **bancos de dados** e em atualizar.</span><span class="sxs-lookup"><span data-stu-id="55f6f-276">Click **Databases** and refresh.</span></span>
> 4. <span data-ttu-id="55f6f-277">Altere o nome do banco de dados na cadeia de conexão no arquivo *Web. config* de volta para o nome do banco de dados original.</span><span class="sxs-lookup"><span data-stu-id="55f6f-277">Change the database name in the connection string in the *web.config* file back to the original database name.</span></span>
> 5. <span data-ttu-id="55f6f-278">Salve o arquivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="55f6f-278">Save the *web.config* file.</span></span>
> 6. <span data-ttu-id="55f6f-279">Clique em **bancos de dados** e em atualizar.</span><span class="sxs-lookup"><span data-stu-id="55f6f-279">Click **Databases** and refresh.</span></span>

#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a><span data-ttu-id="55f6f-280">Problema: pastas criadas pelo WebMatrix não podem ser excluídas</span><span class="sxs-lookup"><span data-stu-id="55f6f-280">Issue: Folders created by WebMatrix cannot be deleted</span></span>

> <span data-ttu-id="55f6f-281">Se o WebMatrix estiver sendo executado usando permissões elevadas (ou seja, você iniciou o WebMatrix usando a opção **Executar como administrador** no Windows), as pastas criadas pelo WebMatrix não poderão ser excluídas usando o Windows Explorer.</span><span class="sxs-lookup"><span data-stu-id="55f6f-281">If WebMatrix is running using elevated permissions (that is, you started WebMatrix using the **Run as Administrator** option in Windows), folders that are created by WebMatrix cannot be deleted using Windows Explorer.</span></span>
> 
> <span data-ttu-id="55f6f-282">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-282">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-283">Execute o Windows Explorer usando permissões elevadas.</span><span class="sxs-lookup"><span data-stu-id="55f6f-283">Run Windows Explorer using elevated permissions.</span></span> <span data-ttu-id="55f6f-284">Siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="55f6f-284">Follow these steps:</span></span>  
> 
> 1. <span data-ttu-id="55f6f-285">No Windows, clique em **Iniciar**.</span><span class="sxs-lookup"><span data-stu-id="55f6f-285">In Windows, click **Start**.</span></span>
> 2. <span data-ttu-id="55f6f-286">Insira "Windows Explorer" e clique com o botão direito do mouse na entrada do **Windows Explorer**.</span><span class="sxs-lookup"><span data-stu-id="55f6f-286">Enter "Windows Explorer" and right-click the entry for **Windows Explorer**.</span></span>
> 3. <span data-ttu-id="55f6f-287">Clique em **Executar como administrador**.</span><span class="sxs-lookup"><span data-stu-id="55f6f-287">Click **Run as Administrator**.</span></span> <span data-ttu-id="55f6f-288">Em seguida, você pode excluir as pastas.</span><span class="sxs-lookup"><span data-stu-id="55f6f-288">You can then delete the folders.</span></span>

#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="55f6f-289">Problema: o WebMatrix 1,0 não pode executar determinadas tarefas que exigem elevação</span><span class="sxs-lookup"><span data-stu-id="55f6f-289">Issue: WebMatrix 1.0 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="55f6f-290">O WebMatrix 1,0 não pode executar determinadas tarefas que exigem elevação, como a instalação de componentes adicionais nas seguintes situações:</span><span class="sxs-lookup"><span data-stu-id="55f6f-290">WebMatrix 1.0 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="55f6f-291">No Windows Vista ou no Windows 7, você está conectado com uma conta que não tem privilégios administrativos e o UAC (controle de conta de usuário) está desabilitado.</span><span class="sxs-lookup"><span data-stu-id="55f6f-291">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="55f6f-292">Você está usando o Microsoft Windows XP ou o Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="55f6f-292">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="55f6f-293">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-293">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-294">A maioria das tarefas no WebMatrix 1,0 não exigem permissão administrativa.</span><span class="sxs-lookup"><span data-stu-id="55f6f-294">Most tasks in WebMatrix 1.0 do not require administrative permission.</span></span> <span data-ttu-id="55f6f-295">Para aqueles que fazem isso, você pode executar a operação como administrador ou seguir estas etapas:</span><span class="sxs-lookup"><span data-stu-id="55f6f-295">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="55f6f-296">No Windows Vista ou no Windows 7, habilite o UAC.</span><span class="sxs-lookup"><span data-stu-id="55f6f-296">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="55f6f-297">No Windows XP, adicione o usuário ao grupo de segurança Administradores.</span><span class="sxs-lookup"><span data-stu-id="55f6f-297">On Windows XP, add the user to the Administrators security group.</span></span>

#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="55f6f-298">Problema: "site da Galeria da Web" está desabilitado</span><span class="sxs-lookup"><span data-stu-id="55f6f-298">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="55f6f-299">A opção **site da Galeria da Web** será desabilitada se o Web Platform Installer 3,0 não estiver instalado.</span><span class="sxs-lookup"><span data-stu-id="55f6f-299">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="55f6f-300">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-300">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-301">Instale o [Microsoft Web Platform Installer 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="55f6f-301">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>

#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="55f6f-302">Problema: o Google Chrome não está disponível como uma opção de execução</span><span class="sxs-lookup"><span data-stu-id="55f6f-302">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="55f6f-303">O Google Chrome não é exibido na lista de navegadores em **executar** na guia **início** .</span><span class="sxs-lookup"><span data-stu-id="55f6f-303">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="55f6f-304">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-304">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-305">Algumas versões do Google Chrome não se registram corretamente com o recurso de programas padrão no Windows.</span><span class="sxs-lookup"><span data-stu-id="55f6f-305">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="55f6f-306">Como alternativa, inicie o Google Chrome, clique no menu *Personalizar e controlar Google Chrome* , clique em *Opções*e, em seguida, clique em *tornar o Google Chrome meu navegador padrão*.</span><span class="sxs-lookup"><span data-stu-id="55f6f-306">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>

#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="55f6f-307">Problema: a caixa de diálogo "chave estrangeira" não permite inserir uma chave primária</span><span class="sxs-lookup"><span data-stu-id="55f6f-307">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="55f6f-308">A caixa de diálogo **chave estrangeira** não permite que você insira o nome da chave primária da tabela de chave primária.</span><span class="sxs-lookup"><span data-stu-id="55f6f-308">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="55f6f-309">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-309">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-310">Isso é intencional.</span><span class="sxs-lookup"><span data-stu-id="55f6f-310">This is intentional.</span></span> <span data-ttu-id="55f6f-311">Você não precisa inserir o nome da chave primária da tabela de chave primária.</span><span class="sxs-lookup"><span data-stu-id="55f6f-311">You do not need to enter the name of the primary key from the primary key table.</span></span>

#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a><span data-ttu-id="55f6f-312">Problema: o IntelliSense não está disponível no WebMatrix para sintaxe Razor C#, ou Visual Basic</span><span class="sxs-lookup"><span data-stu-id="55f6f-312">Issue: IntelliSense is not available in WebMatrix for Razor syntax, C#, or Visual Basic</span></span>

> <span data-ttu-id="55f6f-313">O IntelliSense tem suporte no WebMatrix para HTML e CSS.</span><span class="sxs-lookup"><span data-stu-id="55f6f-313">IntelliSense is supported in WebMatrix for HTML and CSS.</span></span> <span data-ttu-id="55f6f-314">No entanto, ele não está disponível para outros idiomas.</span><span class="sxs-lookup"><span data-stu-id="55f6f-314">However, it is not available for other languages.</span></span> 
> 
> <span data-ttu-id="55f6f-315">**Solução alternativa** </span><span class="sxs-lookup"><span data-stu-id="55f6f-315">**Workaround** </span></span>  
> <span data-ttu-id="55f6f-316">Nenhum.</span><span class="sxs-lookup"><span data-stu-id="55f6f-316">None.</span></span>

#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a><span data-ttu-id="55f6f-317">Problema: o IntelliSense para HTML e CSS sugere elementos que não são contextualmente apropriados</span><span class="sxs-lookup"><span data-stu-id="55f6f-317">Issue: IntelliSense for HTML and CSS suggests elements that are not contextually appropriate</span></span>

> <span data-ttu-id="55f6f-318">O IntelliSense para marcação no WebMatrix dá suporte a HTML usando o [esquema de transição XHTML 1,0](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) e o CSS usando o [esquema CSS 2,1](http://www.w3.org/TR/CSS2/).</span><span class="sxs-lookup"><span data-stu-id="55f6f-318">IntelliSense for markup in WebMatrix supports HTML using the [XHTML 1.0 Transitional schema](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) and CSS using the [CSS 2.1 schema](http://www.w3.org/TR/CSS2/).</span></span> <span data-ttu-id="55f6f-319">Como o IntelliSense é baseado nesses esquemas específicos, determinadas marcas, atributos ou propriedades podem ser sugeridas que não são apropriadas para a definição de página ou estilo atual.</span><span class="sxs-lookup"><span data-stu-id="55f6f-319">Because IntelliSense is based on these specific schemas, certain tags, attributes, or properties might be suggested that are not appropriate for the current page or style definition.</span></span> <span data-ttu-id="55f6f-320">Para HTML, ele também pode levar a sugestões inesperadas no conteúdo que podem ser interpretadas como XHTML malformado (por exemplo, quando as marcas não são fechadas).</span><span class="sxs-lookup"><span data-stu-id="55f6f-320">For HTML, it can also lead to unexpected suggestions in content that might be interpreted as malformed XHTML (for example, when tags are not closed).</span></span> <span data-ttu-id="55f6f-321">Esse problema pode ser mais perceptível se o ponto de inserção estiver dentro de uma marca incompleta; Nesse caso, o IntelliSense pode sugerir novas marcas de abertura ou oferecer outras sugestões incorretas.</span><span class="sxs-lookup"><span data-stu-id="55f6f-321">This issue might be more noticeable if the insertion point is inside an incomplete tag; in that case, IntelliSense might suggest new opening tags or offer other incorrect suggestions.</span></span> 
> 
> <span data-ttu-id="55f6f-322">**Solução alternativa** </span><span class="sxs-lookup"><span data-stu-id="55f6f-322">**Workaround** </span></span>  
> <span data-ttu-id="55f6f-323">Para HTML, verifique se você está trabalhando em uma página XHTML completa e bem formada.</span><span class="sxs-lookup"><span data-stu-id="55f6f-323">For HTML, make sure that you are working within a well-formed, complete XHTML page.</span></span> <span data-ttu-id="55f6f-324">Para o CSS, não há nenhuma solução alternativa.</span><span class="sxs-lookup"><span data-stu-id="55f6f-324">For CSS, there is no workaround.</span></span>

#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a><span data-ttu-id="55f6f-325">Problema: o IntelliSense não é invocado enquanto você digita</span><span class="sxs-lookup"><span data-stu-id="55f6f-325">Issue: IntelliSense is not invoked while you type</span></span>

> <span data-ttu-id="55f6f-326">Às vezes, o IntelliSense pode não ser invocado, pois HTML ou CSS está sendo inserido no editor.</span><span class="sxs-lookup"><span data-stu-id="55f6f-326">At times, IntelliSense might not be invoked as HTML or CSS is being entered in the editor.</span></span> <span data-ttu-id="55f6f-327">Em particular, isso pode acontecer quando o ponto de inserção está diretamente ao lado de outro elemento ou no final de um arquivo.</span><span class="sxs-lookup"><span data-stu-id="55f6f-327">In particular, this might happen when the insertion point is directly next to another element or at the end of a file.</span></span> 
> 
> <span data-ttu-id="55f6f-328">**Solução alternativa** </span><span class="sxs-lookup"><span data-stu-id="55f6f-328">**Workaround** </span></span>  
> <span data-ttu-id="55f6f-329">Verifique se há espaço em branco ao lado do ponto de inserção e se o ponto de inserção não está no final de um arquivo.</span><span class="sxs-lookup"><span data-stu-id="55f6f-329">Make sure that there is whitespace around the insertion point and that the insertion point is not at the end of a file.</span></span> <span data-ttu-id="55f6f-330">Você também pode invocar o IntelliSense manualmente pressionando CTRL + espaço.</span><span class="sxs-lookup"><span data-stu-id="55f6f-330">You can also invoke IntelliSense manually by pressing Ctrl+Space.</span></span>

#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a><span data-ttu-id="55f6f-331">Problema: nenhuma interface do usuário está disponível para desabilitar o IntelliSense</span><span class="sxs-lookup"><span data-stu-id="55f6f-331">Issue: No UI is available for disabling IntelliSense</span></span>

> <span data-ttu-id="55f6f-332">O WebMatrix 1,0 não fornece nenhuma interface do usuário nem gesto para desabilitar o IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="55f6f-332">WebMatrix 1.0 provides no UI or gesture for disabling IntelliSense.</span></span> 
> 
> <span data-ttu-id="55f6f-333">**Solução alternativa** </span><span class="sxs-lookup"><span data-stu-id="55f6f-333">**Workaround** </span></span>  
> <span data-ttu-id="55f6f-334">Inicie o WebMatrix usando o comando a seguir, que inclui uma opção que desabilita o IntelliSense:</span><span class="sxs-lookup"><span data-stu-id="55f6f-334">Start WebMatrix using the following command, which includes a switch that disables IntelliSense:</span></span>  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`

<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a><span data-ttu-id="55f6f-335">IIS Express</span><span class="sxs-lookup"><span data-stu-id="55f6f-335">IIS Express</span></span>

<span data-ttu-id="55f6f-336">IIS Express tem seu próprio arquivo Leiame, que está disponível na seguinte URL:</span><span class="sxs-lookup"><span data-stu-id="55f6f-336">IIS Express has its own readme file, which is available at the following URL:</span></span>

[<span data-ttu-id="55f6f-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0x409</span><span class="sxs-lookup"><span data-stu-id="55f6f-337">https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409</span></span>](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a><span data-ttu-id="55f6f-338">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="55f6f-338">SQL Server Compact</span></span>

<span data-ttu-id="55f6f-339">SQL Server Compact tem seu próprio arquivo Leiame, que está disponível na seguinte URL:</span><span class="sxs-lookup"><span data-stu-id="55f6f-339">SQL Server Compact has its own readme file, which is available at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

<span data-ttu-id="55f6f-340">Para obter informações sobre problemas que envolvem a instalação do SQL Server Compact como parte do WebMatrix, consulte [problemas de instalação do WebMatrix](#Known_Issues_Installation) anteriormente neste documento.</span><span class="sxs-lookup"><span data-stu-id="55f6f-340">For information about issues that involve installing SQL Server Compact as part of WebMatrix, see [WebMatrix Installation Issues](#Known_Issues_Installation) earlier in this document.</span></span>

### <a id="Known_Issues_Installing_Applications"></a><span data-ttu-id="55f6f-341">Instalando aplicativos</span><span class="sxs-lookup"><span data-stu-id="55f6f-341">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="55f6f-342">Problema: a instalação de um aplicativo pode levar muito tempo se a pasta meus documentos do usuário for redirecionada para um compartilhamento de rede</span><span class="sxs-lookup"><span data-stu-id="55f6f-342">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="55f6f-343">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-343">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-344">Nenhum.</span><span class="sxs-lookup"><span data-stu-id="55f6f-344">None.</span></span> <span data-ttu-id="55f6f-345">O aplicativo pode levar algum tempo para ser instalado, mas será instalado corretamente.</span><span class="sxs-lookup"><span data-stu-id="55f6f-345">The application might take a while to install, but will install correctly.</span></span>

### <a id="Known_Issues_Publishing_Applications"></a><span data-ttu-id="55f6f-346">Publicando aplicativos</span><span class="sxs-lookup"><span data-stu-id="55f6f-346">Publishing Applications</span></span>

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a><span data-ttu-id="55f6f-347">Problema: erro "não é possível adquirir as permissões necessárias" ao publicar um banco de dados do SQL Compact</span><span class="sxs-lookup"><span data-stu-id="55f6f-347">Issue: "Required permissions cannot be acquired" error when publishing a SQL Compact Database</span></span>

> <span data-ttu-id="55f6f-348">O WebMatrix não dá suporte total à implantação de binários de suporte para SQL Server Compact a um servidor que esteja executando o .NET Framework versão 3,5 com uma configuração de confiança média.</span><span class="sxs-lookup"><span data-stu-id="55f6f-348">WebMatrix does not fully support deploying supporting binaries for SQL Server Compact to a server that is running .NET Framework version 3.5 with a medium trust configuration.</span></span>
> 
> <span data-ttu-id="55f6f-349">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-349">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-350">A solução alternativa preferida é instalar o .NET Framework 4 no servidor.</span><span class="sxs-lookup"><span data-stu-id="55f6f-350">The preferred workaround is to install the .NET Framework 4 on the server.</span></span> <span data-ttu-id="55f6f-351">Como alternativa, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="55f6f-351">Alternatively, do the following:</span></span>
> 
> 1. <span data-ttu-id="55f6f-352">Adicione os seguintes elementos à seção `SecurityClasses` no arquivo *Web\_MediumTrust. config* :</span><span class="sxs-lookup"><span data-stu-id="55f6f-352">Add the following elements to the `SecurityClasses` section in *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. <span data-ttu-id="55f6f-353">Crie um novo conjunto de permissões no arquivo *Web\_MediumTrust. config* com as seguintes permissões necessárias:</span><span class="sxs-lookup"><span data-stu-id="55f6f-353">Create a new permission set in the *Web\_MediumTrust.config* file with the following required permissions:</span></span>
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. <span data-ttu-id="55f6f-354">Aplique o conjunto de permissões a SQL Server Compact colocando os seguintes elementos no arquivo *Web\_MediumTrust. config* :</span><span class="sxs-lookup"><span data-stu-id="55f6f-354">Apply the permission set to SQL Server Compact by putting the following elements in the *Web\_MediumTrust.config* file:</span></span>
> 
>     [!code-html[Main](overview/samples/sample8.html)]

#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a><span data-ttu-id="55f6f-355">Problema: os aplicativos Web da galeria e do PhpBB exibem um erro "Serviço indisponível" após a publicação</span><span class="sxs-lookup"><span data-stu-id="55f6f-355">Issue: Gallery and PhpBB web applications display a "Service is unavailable" error after publishing</span></span>

> <span data-ttu-id="55f6f-356">Em algumas circunstâncias, a publicação de um aplicativo causa um erro "Serviço indisponível".</span><span class="sxs-lookup"><span data-stu-id="55f6f-356">Under some circumstances, publishing an application causes a "service is unavailable" error.</span></span>
> 
> <span data-ttu-id="55f6f-357">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-357">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-358">No WebMatrix, adicione uma barra invertida (\) ao final do nome do servidor na janela de **configurações de publicação** e, em seguida, publique o aplicativo novamente.</span><span class="sxs-lookup"><span data-stu-id="55f6f-358">In WebMatrix, add a backslash (\) to the end of the server name in the **Publish Settings** window and then publish the application again.</span></span>

#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a><span data-ttu-id="55f6f-359">Problema: layout e links do site Moodle são quebrados após a publicação</span><span class="sxs-lookup"><span data-stu-id="55f6f-359">Issue: Moodle website layout and links are broken after publishing</span></span>

> <span data-ttu-id="55f6f-360">Depois que você publicar um aplicativo Moodle, o aplicativo não funcionará corretamente.</span><span class="sxs-lookup"><span data-stu-id="55f6f-360">After you publish a Moodle application, the application does not work correctly.</span></span>
> 
> <span data-ttu-id="55f6f-361">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-361">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-362">No WebMatrix, adicione uma barra (/) ao final do campo **nome do site** na janela **configurações de publicação** e, em seguida, publique o aplicativo novamente.</span><span class="sxs-lookup"><span data-stu-id="55f6f-362">In WebMatrix, add a slash (/) to the end of the **Site Name** field in the **Publish Settings** window and then publish the application again.</span></span>

#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a><span data-ttu-id="55f6f-363">Problema: a publicação de nopCommerce falha com um erro de banco de dados</span><span class="sxs-lookup"><span data-stu-id="55f6f-363">Issue: Publishing nopCommerce fails with a database error</span></span>

> <span data-ttu-id="55f6f-364">A publicação de nopCommerce falha e relata um erro de banco de dados como "inserir na tabela de log de\_de Nop falhou".</span><span class="sxs-lookup"><span data-stu-id="55f6f-364">Publishing nopCommerce fails and reports a database error like "Insert into the nop\_log table failed."</span></span>
> 
> <span data-ttu-id="55f6f-365">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-365">**Workaround**</span></span>  
> 
> 1. <span data-ttu-id="55f6f-366">No WebMatrix, clique em **executar** para iniciar o Nopcommerce localmente.</span><span class="sxs-lookup"><span data-stu-id="55f6f-366">In WebMatrix, click **Run** to launch nopCommerce locally.</span></span>
> 2. <span data-ttu-id="55f6f-367">Faça logon na página de administração.</span><span class="sxs-lookup"><span data-stu-id="55f6f-367">Log into the administration page.</span></span>
> 3. <span data-ttu-id="55f6f-368">Clique no menu **sistema** .</span><span class="sxs-lookup"><span data-stu-id="55f6f-368">Click the **System** menu.</span></span>
> 4. <span data-ttu-id="55f6f-369">Clique na opção **log** .</span><span class="sxs-lookup"><span data-stu-id="55f6f-369">Click the **Log** option.</span></span>
> 5. <span data-ttu-id="55f6f-370">Clique no botão **Limpar Log**.</span><span class="sxs-lookup"><span data-stu-id="55f6f-370">Click the **Clear Log** button.</span></span>
> 6. <span data-ttu-id="55f6f-371">Publique nopCommerce novamente.</span><span class="sxs-lookup"><span data-stu-id="55f6f-371">Publish nopCommerce again.</span></span>

#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a><span data-ttu-id="55f6f-372">Problema: o Silverstripe CMS exibe um erro "HTTP 500 PHP FCGI" ao baixar um site publicado</span><span class="sxs-lookup"><span data-stu-id="55f6f-372">Issue: Silverstripe CMS displays a "HTTP 500 PHP FCGI Error" when you download a published site</span></span>

> <span data-ttu-id="55f6f-373">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-373">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-374">Depois de clicar em **baixar site publicado**, pule `silverstripe-cache/manifest_main` na **visualização de publicação**.</span><span class="sxs-lookup"><span data-stu-id="55f6f-374">After you click **Download published site**, skip `silverstripe-cache/manifest_main` in **Publish Preview**.</span></span> <span data-ttu-id="55f6f-375">Esse arquivo é usado para fins de cache e é específico para cada computador.</span><span class="sxs-lookup"><span data-stu-id="55f6f-375">This file is used for caching purposes and is specific to each computer.</span></span>

#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a><span data-ttu-id="55f6f-376">Problema: o subtexto exibe "erro de servidor no aplicativo"/"quando você baixa um site publicado</span><span class="sxs-lookup"><span data-stu-id="55f6f-376">Issue: Subtext displays "Server Error in '/' Application" when you download a published site</span></span>

> <span data-ttu-id="55f6f-377">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-377">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-378">Abra o arquivo *Web. config* do site e substitua a ID de usuário e a senha na cadeia de conexão do banco de dados pelo SQL Server credenciais de administrador (as credenciais "SA").</span><span class="sxs-lookup"><span data-stu-id="55f6f-378">Open the site's *web.config* file and replace the user ID and password in the database connection string with the SQL Server administrator credentials (the "sa" credentials).</span></span>
> 
> <span data-ttu-id="55f6f-379">Como alternativa, siga estas etapas para dar à conta de usuário com a qual você está conectado `db_owner` permissões:</span><span class="sxs-lookup"><span data-stu-id="55f6f-379">Alternatively, follow these steps in order to give the user account you are logged in with `db_owner` permissions:</span></span>
> 
> 1. <span data-ttu-id="55f6f-380">Instale SQL Server Management Studio usando o Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="55f6f-380">Install SQL Server Management Studio using the Web Platform Installer.</span></span>
> 2. <span data-ttu-id="55f6f-381">Conecte-se à instância de SQL Server Express local (por padrão, `.\SQLEXPRESS`).</span><span class="sxs-lookup"><span data-stu-id="55f6f-381">Connect to the local SQL Server Express instance (by default, `.\SQLEXPRESS`).</span></span>
> 3. <span data-ttu-id="55f6f-382">Clique em **bancos de dados** &gt; *[LocalSubtextDatabase]* &gt; **segurança** &gt; **usuários** &gt; *[localSubtextUser*] (o padrão é `subtextuser`], clique com o botão direito do mouse e clique em **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="55f6f-382">Click **Databases** &gt; *[localSubtextDatabase]* &gt; **Security** &gt; **Users** &gt; *[localSubtextUser*] (default is `subtextuser`], right-click, and click **Properties**.</span></span>
> 4. <span data-ttu-id="55f6f-383">Selecione **db\_proprietário** na seção Associação de função.</span><span class="sxs-lookup"><span data-stu-id="55f6f-383">Select **db\_owner** in the role membership section.</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="55f6f-384">Problema: o site pode não funcionar após a publicação se o campo "URL de destino" não for prefixado com http://ou https://</span><span class="sxs-lookup"><span data-stu-id="55f6f-384">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="55f6f-385">Na caixa de diálogo **configurações de publicação** , se a URL de destino não começar com `http://` ou `https://`, o site poderá não funcionar após a implantação.</span><span class="sxs-lookup"><span data-stu-id="55f6f-385">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="55f6f-386">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-386">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-387">Certifique-se de que, antes de publicar um site, a URL de destino na caixa de diálogo **configurações de publicação** comece com `http://` ou `https://`.</span><span class="sxs-lookup"><span data-stu-id="55f6f-387">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>

#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="55f6f-388">Problema: a publicação de um banco de dados MySQL falha com o erro "falha ao publicar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="55f6f-388">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="55f6f-389">Isso pode acontecer se o banco de dados remoto não puder executar o script. "</span><span class="sxs-lookup"><span data-stu-id="55f6f-389">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="55f6f-390">O erro pode ocorrer por vários motivos.</span><span class="sxs-lookup"><span data-stu-id="55f6f-390">The error can occur for a number of reasons.</span></span> <span data-ttu-id="55f6f-391">Um motivo para você ver esse erro é se o script de banco de dados contiver um caractere de aspas simples (') e o conjunto de caracteres padrão do banco de dados MySQL de destino não for UTF-8.</span><span class="sxs-lookup"><span data-stu-id="55f6f-391">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="55f6f-392">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-392">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-393">Defina o conjunto de caracteres padrão para o banco de dados MySQL remoto como UTF-8.</span><span class="sxs-lookup"><span data-stu-id="55f6f-393">Set the default character set for the remote MySQL database to UTF-8.</span></span>

#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a><span data-ttu-id="55f6f-394">Problema: alguns links não são visíveis no DotNetNuke após a publicação ou o download do site</span><span class="sxs-lookup"><span data-stu-id="55f6f-394">Issue: Some links are not visible in DotNetNuke after publishing or downloading the site</span></span>

> <span data-ttu-id="55f6f-395">Se você publicar ou baixar um site do DotNetNuke, talvez seja necessário limpar o cache para obter os novos links a serem exibidos no site.</span><span class="sxs-lookup"><span data-stu-id="55f6f-395">If you publish or download a DotNetNuke site, you might need to clear the cache to get the new links to appear on the site.</span></span>
> 
> <span data-ttu-id="55f6f-396">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-396">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="55f6f-397">Faça logon como "host".</span><span class="sxs-lookup"><span data-stu-id="55f6f-397">Log in as "Host".</span></span>
> 2. <span data-ttu-id="55f6f-398">Vá para o menu host e selecione **configurações do host**.</span><span class="sxs-lookup"><span data-stu-id="55f6f-398">Go to the host menu and select **Host Settings**.</span></span>
> 3. <span data-ttu-id="55f6f-399">Role para baixo e, em **Configurações avançadas**, expanda **configurações de desempenho**.</span><span class="sxs-lookup"><span data-stu-id="55f6f-399">Scroll down and under **Advanced Settings**, expand **Performance Settings**.</span></span>
> 4. <span data-ttu-id="55f6f-400">Clique no link **Limpar cache** para páginas.</span><span class="sxs-lookup"><span data-stu-id="55f6f-400">Click the **Clear Cache** link for pages.</span></span>
> 5. <span data-ttu-id="55f6f-401">Vá para a parte inferior da página e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="55f6f-401">Go to the bottom of the page and restart the application.</span></span>

#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a><span data-ttu-id="55f6f-402">Problema: alguns links em AtomSite são quebrados após o download de um site publicado</span><span class="sxs-lookup"><span data-stu-id="55f6f-402">Issue: Some links in AtomSite are broken after you download a published site</span></span>

> <span data-ttu-id="55f6f-403">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-403">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-404">No arquivo *Service. config* , no arquivo *users. config* e em todos os arquivos *. xml* , substitua a cadeia de caracteres da URL (por exemplo, `http://myhost.com/atomsite`) pelo local um (por exemplo, `http://localhost:1239`).</span><span class="sxs-lookup"><span data-stu-id="55f6f-404">In the *service.config* file, *users.config* file, and all *.xml* files, replace the URL string (for example, `http://myhost.com/atomsite`) with the local one (for example, `http://localhost:1239`).</span></span>

#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a><span data-ttu-id="55f6f-405">Problema: aplicativos baseados em MySQL como o WordPress falham ao publicar e relatar um erro de banco de dados</span><span class="sxs-lookup"><span data-stu-id="55f6f-405">Issue: MySQL-based applications like WordPress fail to publish and report a database error</span></span>

> <span data-ttu-id="55f6f-406">Por padrão, o WebMatrix instala o MySQL com o conjunto de caracteres UTF-8.</span><span class="sxs-lookup"><span data-stu-id="55f6f-406">By default, WebMatrix installs MySQL with the UTF-8 character set.</span></span> <span data-ttu-id="55f6f-407">Se você instalar o MySQL por conta própria e o conjunto de caracteres não for UTF-8 (por exemplo, é Latino1), o processo de publicação para bancos de dados poderá falhar.</span><span class="sxs-lookup"><span data-stu-id="55f6f-407">If you install MySQL on your own, and the character set is not UTF-8 (for example, it is Latin1), the publish process for databases might fail.</span></span>
> 
> <span data-ttu-id="55f6f-408">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-408">**Workaround**</span></span>
> 
> 1. <span data-ttu-id="55f6f-409">Altere o conjunto de caracteres para MySQL para UTF-8.</span><span class="sxs-lookup"><span data-stu-id="55f6f-409">Change the character set for MySQL to UTF-8.</span></span> <span data-ttu-id="55f6f-410">(Para obter detalhes, consulte [conjunto de caracteres do servidor e agrupamento](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) no site do MySQL.)</span><span class="sxs-lookup"><span data-stu-id="55f6f-410">(For details, see [Server Character Set and Collation](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) on the MySQL website.)</span></span>
> 2. <span data-ttu-id="55f6f-411">Reinstale o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="55f6f-411">Reinstall the application.</span></span>
> 3. <span data-ttu-id="55f6f-412">Republique o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="55f6f-412">Republish the application.</span></span>

#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a><span data-ttu-id="55f6f-413">Problema: "baixar site publicado" falha para aplicativos que têm a instalação baseada em navegador</span><span class="sxs-lookup"><span data-stu-id="55f6f-413">Issue: "Download published site" fails for applications that have browser-based setup</span></span>

> <span data-ttu-id="55f6f-414">Alguns aplicativos (por exemplo, Kentico CMS) exigem que você os inicie no navegador para executar a instalação após a instalação, como a criação de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="55f6f-414">Some applications (for example, Kentico CMS) require you to launch them in the browser in order to perform post-installation setup such as creating a database.</span></span> <span data-ttu-id="55f6f-415">Se você publicar um aplicativo como este sem concluir a instalação baseada em navegador, a tentativa de baixar o mesmo site de um servidor remoto falhará.</span><span class="sxs-lookup"><span data-stu-id="55f6f-415">If you publish an application like this without completing the browser-based setup, attempting to download the same site from a remote server will fail.</span></span>
> 
> <span data-ttu-id="55f6f-416">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-416">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-417">Conclua a instalação baseada em navegador antes de publicar o site.</span><span class="sxs-lookup"><span data-stu-id="55f6f-417">Finish browser-based setup before publishing the site.</span></span>

#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a><span data-ttu-id="55f6f-418">Problema: "baixar site publicado" falha com um erro de banco de dados para o DotNetNuke e o Kooboo CMS</span><span class="sxs-lookup"><span data-stu-id="55f6f-418">Issue: "Download published site" fails with a database error for DotNetNuke and Kooboo CMS</span></span>

> <span data-ttu-id="55f6f-419">Se você tentar baixar um aplicativo de um servidor e tiver credenciais de administrador na cadeia de conexão do banco de dados na caixa de diálogo **configurações de publicação** , poderá ver o seguinte erro no log de publicação:</span><span class="sxs-lookup"><span data-stu-id="55f6f-419">If you try to download an application from a server and you have administrator credentials in the database connection string in the **Publish Settings** dialog, you might see the following error in the publish log:</span></span>
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> <span data-ttu-id="55f6f-420">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="55f6f-420">**Workaround**</span></span>  
> <span data-ttu-id="55f6f-421">Se for prático, Republique o site (ou publique-o) usando credenciais de não administrador para o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="55f6f-421">If practical, republish the site (or have it published) using non-administrator credentials for the database.</span></span>

<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="55f6f-422">Para obter mais informações</span><span class="sxs-lookup"><span data-stu-id="55f6f-422">For More Information</span></span>

<span data-ttu-id="55f6f-423">Para obter mais informações sobre o WebMatrix 1,0, consulte os seguintes sites:</span><span class="sxs-lookup"><span data-stu-id="55f6f-423">For more information about WebMatrix 1.0, see the following websites:</span></span>

- [<span data-ttu-id="55f6f-424">IIS.net</span><span class="sxs-lookup"><span data-stu-id="55f6f-424">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="55f6f-425">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="55f6f-425">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="55f6f-426">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="55f6f-426">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

<span data-ttu-id="55f6f-427">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="55f6f-427">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="55f6f-428">Todos os direitos reservados.</span><span class="sxs-lookup"><span data-stu-id="55f6f-428">All Rights Reserved.</span></span> <span data-ttu-id="55f6f-429">[Termos de uso](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="55f6f-429">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
