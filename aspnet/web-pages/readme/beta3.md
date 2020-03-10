---
uid: web-pages/readme/beta3
title: Leiame da versão beta 3 do Web Matrix e do Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: rick-anderson
description: Leiame do Web Matrix e de Páginas da Web do ASP.NET (Razor) versão beta 3
ms.author: riande
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: dc1d9237c04a7fcdbf4db6ccc8c36d255f6de003
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628894"
---
# <a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a><span data-ttu-id="d75bb-103">Leiame do Web Matrix e de Páginas da Web do ASP.NET (Razor) versão beta 3</span><span class="sxs-lookup"><span data-stu-id="d75bb-103">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

> <span data-ttu-id="d75bb-104">Leiame do Web Matrix e de Páginas da Web do ASP.NET (Razor) versão beta 3</span><span class="sxs-lookup"><span data-stu-id="d75bb-104">Web Matrix and ASP.NET Web Pages (Razor) Beta 3 Release Readme</span></span>

<span data-ttu-id="d75bb-105">9 de novembro de 2010</span><span class="sxs-lookup"><span data-stu-id="d75bb-105">9 November 2010</span></span>

## <a name="contents"></a><span data-ttu-id="d75bb-106">Conteúdo</span><span class="sxs-lookup"><span data-stu-id="d75bb-106">Contents</span></span>

- [<span data-ttu-id="d75bb-107">Visão geral</span><span class="sxs-lookup"><span data-stu-id="d75bb-107">Overview</span></span>](#Overview)
- [<span data-ttu-id="d75bb-108">Instalação</span><span class="sxs-lookup"><span data-stu-id="d75bb-108">Installation</span></span>](#Installation_Notes)
- [<span data-ttu-id="d75bb-109">Novos recursos, alterações e problemas conhecidos na versão beta 3</span><span class="sxs-lookup"><span data-stu-id="d75bb-109">New Features, Changes, and Known Issues in the Beta 3 release</span></span>](#Known_Issues)

    - [<span data-ttu-id="d75bb-110">Problemas de instalação do WebMatrix</span><span class="sxs-lookup"><span data-stu-id="d75bb-110">WebMatrix Installation Issues</span></span>](#Known_Issues_Installation)
    - [<span data-ttu-id="d75bb-111">Páginas da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d75bb-111">ASP.NET Web Pages</span></span>](#Known_Issues_ASPNET)
    - [<span data-ttu-id="d75bb-112">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="d75bb-112">SQL Server Compact</span></span>](#Known_Issues_SQL_Server_Compact)
    - [<span data-ttu-id="d75bb-113">Instalando aplicativos</span><span class="sxs-lookup"><span data-stu-id="d75bb-113">Installing Applications</span></span>](#Known_Issues_Installing_Applications)
    - [<span data-ttu-id="d75bb-114">Publicando aplicativos</span><span class="sxs-lookup"><span data-stu-id="d75bb-114">Publishing Applications</span></span>](#Known_Issues_Publishing_Applications)
    - [<span data-ttu-id="d75bb-115">Outros problemas</span><span class="sxs-lookup"><span data-stu-id="d75bb-115">Other Issues</span></span>](#Known_Issues_Other_Issues)
- [<span data-ttu-id="d75bb-116">Para obter mais informações</span><span class="sxs-lookup"><span data-stu-id="d75bb-116">For More Information</span></span>](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a><span data-ttu-id="d75bb-117">Visão geral</span><span class="sxs-lookup"><span data-stu-id="d75bb-117">Overview</span></span>

> <span data-ttu-id="d75bb-118">O Microsoft WebMatrix beta é uma pilha de desenvolvimento na Web gratuita que é instalada em minutos.</span><span class="sxs-lookup"><span data-stu-id="d75bb-118">Microsoft WebMatrix Beta is a free web development stack that installs in minutes.</span></span> <span data-ttu-id="d75bb-119">Ele integra um servidor Web com estruturas de banco de dados e programação para criar uma experiência única e integrada.</span><span class="sxs-lookup"><span data-stu-id="d75bb-119">It integrates a web server with database and programming frameworks to create a single, integrated experience.</span></span> <span data-ttu-id="d75bb-120">Você pode usar o WebMatrix beta para simplificar a maneira como você codifica, testa e publica seu próprio site ASP.NET ou PHP ou pode usar o WebMatrix beta para iniciar um novo site usando aplicativos de software livre populares, como o DotNetNuke, o Umbraco, o WordPress ou o Joomla.</span><span class="sxs-lookup"><span data-stu-id="d75bb-120">You can use WebMatrix Beta to streamline the way you code, test, and publish your own ASP.NET or PHP website, or you can use WebMatrix Beta to start a new website using popular open-source apps like DotNetNuke, Umbraco, WordPress, or Joomla.</span></span> <span data-ttu-id="d75bb-121">O WebMatrix beta usa o mesmo ambiente avançado de servidor Web, mecanismo de banco de dados e estruturas que executará seu site na Internet, o que torna a transição do desenvolvimento para a produção tranqüila e contínua.</span><span class="sxs-lookup"><span data-stu-id="d75bb-121">WebMatrix Beta uses the same powerful web server, database engine, and frameworks environment that will run your website on the internet, which makes the transition from development to production smooth and seamless.</span></span>

<a id="Installation_Notes"></a>

## <a name="installation"></a><span data-ttu-id="d75bb-122">Instalação</span><span class="sxs-lookup"><span data-stu-id="d75bb-122">Installation</span></span>

> <span data-ttu-id="d75bb-123">Para instalar o WebMatrix Beta 3, você usa [Microsoft Web Platform Installer 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="d75bb-123">To install WebMatrix Beta 3, you use [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span> <span data-ttu-id="d75bb-124">Depois de instalar o Web Platform Installer, você poderá usá-lo para instalar o WebMatrix Beta 3.</span><span class="sxs-lookup"><span data-stu-id="d75bb-124">After you've installed the Web Platform Installer, you can use it to install WebMatrix Beta 3.</span></span>
> 
> <span data-ttu-id="d75bb-125">Se você tiver problemas durante a instalação, consulte [Solucionando problemas com Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span><span class="sxs-lookup"><span data-stu-id="d75bb-125">If you have problems during installation, refer to [Troubleshooting Problems with Microsoft Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=196212).</span></span>

<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a><span data-ttu-id="d75bb-126">Instruções para publicação de aplicativos</span><span class="sxs-lookup"><span data-stu-id="d75bb-126">Instructions for Publishing Applications</span></span>

> <span data-ttu-id="d75bb-127">Consulte [as instruções passo a passo para publicar aplicativos](https://go.microsoft.com/fwlink/?LinkID=196149)</span><span class="sxs-lookup"><span data-stu-id="d75bb-127">See [Step-by-Step Instructions for Publishing Applications](https://go.microsoft.com/fwlink/?LinkID=196149)</span></span>

<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a><span data-ttu-id="d75bb-128">Novos recursos, alterações, problemas de andKnown</span><span class="sxs-lookup"><span data-stu-id="d75bb-128">New Features, Changes, andKnown Issues</span></span>

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a><span data-ttu-id="d75bb-129">Instalação do WebMatrix Beta 3</span><span class="sxs-lookup"><span data-stu-id="d75bb-129">WebMatrix Beta 3 Installation</span></span>

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a><span data-ttu-id="d75bb-130">Problema: o WebMatrix Beta 3 só está disponível em plataformas que dão suporte ao Microsoft .NET Framework 4</span><span class="sxs-lookup"><span data-stu-id="d75bb-130">Issue: WebMatrix Beta 3 is only available on platforms that support Microsoft .NET Framework 4</span></span>

> <span data-ttu-id="d75bb-131">O .NET Framework versão 4 é necessário para o WebMatrix beta.</span><span class="sxs-lookup"><span data-stu-id="d75bb-131">The .NET Framework version 4 is required for WebMatrix Beta.</span></span> <span data-ttu-id="d75bb-132">Em determinados casos, o instalador do WebMatrix beta permitirá que você tente instalar o em uma plataforma que não faça parte do conjunto de configuração com suporte.</span><span class="sxs-lookup"><span data-stu-id="d75bb-132">In certain cases, the WebMatrix Beta installer will let you try to install on a platform that is not part of the supported configuration set.</span></span> <span data-ttu-id="d75bb-133">Em particular, o Windows Vista sem a atualização do SP1 permitirá que você comece a instalação do WebMatrix beta, mas o componente .NET Framework 4 falhará e bloqueará sua instalação.</span><span class="sxs-lookup"><span data-stu-id="d75bb-133">In particular, Windows Vista without the SP1 update will let you begin the installation of WebMatrix Beta, but the .NET Framework 4 component will fail and block your installation.</span></span>
> 
> <span data-ttu-id="d75bb-134">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-134">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-135">Instale o em uma plataforma com suporte, que inclui:</span><span class="sxs-lookup"><span data-stu-id="d75bb-135">Install on a supported platform, which includes:</span></span>
> 
> - <span data-ttu-id="d75bb-136">Windows 7</span><span class="sxs-lookup"><span data-stu-id="d75bb-136">Windows 7</span></span>
> - <span data-ttu-id="d75bb-137">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="d75bb-137">Windows Server 2008</span></span>
> - <span data-ttu-id="d75bb-138">Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="d75bb-138">Windows Server 2008 R2</span></span>
> - <span data-ttu-id="d75bb-139">Windows Vista SP1 ou posterior</span><span class="sxs-lookup"><span data-stu-id="d75bb-139">Windows Vista SP1 or later</span></span>
> - <span data-ttu-id="d75bb-140">Windows XP SP3</span><span class="sxs-lookup"><span data-stu-id="d75bb-140">Windows XP SP3</span></span>
> - <span data-ttu-id="d75bb-141">Windows Server 2003 SP2</span><span class="sxs-lookup"><span data-stu-id="d75bb-141">Windows Server 2003 SP2</span></span>

#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a><span data-ttu-id="d75bb-142">Problema: não é possível instalar o WebMatrix Beta 3 se o Microsoft Visual Studio 2008 for instalado sem Microsoft Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="d75bb-142">Issue: Cannot install WebMatrix Beta 3 if Microsoft Visual Studio 2008 is installed without Microsoft Visual Studio 2008 SP1</span></span>

> <span data-ttu-id="d75bb-143">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-143">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-144">Instale o [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) no centro de download da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="d75bb-144">Install [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) from the Microsoft Download Center.</span></span>

#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a><span data-ttu-id="d75bb-145">Problema: alguns assemblies para o SQL Server Compact 4,0 não estão instalados no GAC</span><span class="sxs-lookup"><span data-stu-id="d75bb-145">Issue: Some assemblies for SQL Server Compact 4.0 are not installed in the GAC</span></span>

> <span data-ttu-id="d75bb-146">Os assemblies gerenciados para SQL Server Compact 4,0 não são colocados no GAC (cache de assembly global) quando você instala o SQL Server Compact 4,0 em um computador de 64 bits e o computador tem apenas o perfil de cliente do .NET Framework 3,5 SP1 instalado.</span><span class="sxs-lookup"><span data-stu-id="d75bb-146">The managed assemblies for SQL Server Compact 4.0 are not placed in the global assembly cache (GAC) when you install SQL Server Compact 4.0 on a 64-bit computer and the computer has only the .NET Framework 3.5 SP1 Client Profile installed.</span></span> <span data-ttu-id="d75bb-147">Os assemblies gerenciados que não estão instalados no GAC são:</span><span class="sxs-lookup"><span data-stu-id="d75bb-147">The managed assemblies that are not installed in the GAC are:</span></span>
> 
> - <span data-ttu-id="d75bb-148">*System. Data. SqlServerCe. dll* (provedor ADO.net)</span><span class="sxs-lookup"><span data-stu-id="d75bb-148">*System.Data.SqlServerCe.dll* (ADO.NET provider)</span></span>
> - <span data-ttu-id="d75bb-149">*System. Data. SqlServerCe. Entity. dll* (ADO.NET Entity Framework)</span><span class="sxs-lookup"><span data-stu-id="d75bb-149">*System.Data.SqlServerCe.Entity.dll* (ADO.NET Entity Framework )</span></span>
> 
> <span data-ttu-id="d75bb-150">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-150">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-151">Desinstale o SQL Server Compact 4,0.</span><span class="sxs-lookup"><span data-stu-id="d75bb-151">Uninstall SQL Server Compact 4.0.</span></span> <span data-ttu-id="d75bb-152">Baixe e instale a versão completa do .NET Framework 3,5 SP1 no seguinte local:</span><span class="sxs-lookup"><span data-stu-id="d75bb-152">Download and install the full version of .NET Framework 3.5 SP1 from the following location:</span></span>  
>   
> [<span data-ttu-id="d75bb-153">Microsoft .NET Framework 3,5 Service Pack 1 (pacote completo)</span><span class="sxs-lookup"><span data-stu-id="d75bb-153">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> <span data-ttu-id="d75bb-154">Em seguida, reinstale o SQL Server Compact 4,0.</span><span class="sxs-lookup"><span data-stu-id="d75bb-154">Then reinstall SQL Server Compact 4.0.</span></span>

#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a><span data-ttu-id="d75bb-155">Problema: não é possível desinstalar SQL Server Compact usando a linha de comando</span><span class="sxs-lookup"><span data-stu-id="d75bb-155">Issue: Cannot uninstall SQL Server Compact using the command line</span></span>

> <span data-ttu-id="d75bb-156">A desinstalação do SQL Server Compact usando opções de linha de comando não funciona nesta versão.</span><span class="sxs-lookup"><span data-stu-id="d75bb-156">Uninstallation of SQL Server Compact using command-line options does not work in this release.</span></span>
> 
> <span data-ttu-id="d75bb-157">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-157">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-158">Use *programas e recursos* no painel de controle do Windows para desinstalar o Microsoft SQL Server Compact 4,0.</span><span class="sxs-lookup"><span data-stu-id="d75bb-158">Use *Programs and Features* in the Windows Control Panel to uninstall Microsoft SQL Server Compact 4.0.</span></span>

<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a><span data-ttu-id="d75bb-159">Páginas da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d75bb-159">ASP.NET Web Pages</span></span>

<span data-ttu-id="d75bb-160">Esta seção do documento descreve novos recursos, alterações e problemas conhecidos com a versão beta 3 do Páginas da Web do ASP.NET com sintaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="d75bb-160">This section of the document describes new features, changes, and known issues with the Beta 3 release of ASP.NET Web Pages with Razor syntax.</span></span>

- [<span data-ttu-id="d75bb-161">Novos recursos</span><span class="sxs-lookup"><span data-stu-id="d75bb-161">New features</span></span>](#NewFeatures)
- [<span data-ttu-id="d75bb-162">Alterações</span><span class="sxs-lookup"><span data-stu-id="d75bb-162">Changes</span></span>](#Changes)
- [<span data-ttu-id="d75bb-163">Problemas</span><span class="sxs-lookup"><span data-stu-id="d75bb-163">Issues</span></span>](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="d75bb-164">Novos recursos na versão beta 3 para Páginas da Web do ASP.NET com a sintaxe do Razor</span><span class="sxs-lookup"><span data-stu-id="d75bb-164">New Features in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a><span data-ttu-id="d75bb-165">Novo: o método "HTML. Raw" renderiza a marcação não codificada</span><span class="sxs-lookup"><span data-stu-id="d75bb-165">New: "Html.Raw" method renders unencoded markup</span></span>

> <span data-ttu-id="d75bb-166">O novo método `Html.Raw` permite que você processe a marcação HTML como marcação em vez de renderizar a saída codificada.</span><span class="sxs-lookup"><span data-stu-id="d75bb-166">The new `Html.Raw` method lets you render HTML markup as markup instead of rendering encoded output.</span></span> <span data-ttu-id="d75bb-167">(Por padrão, o ASP.NET Razor codifica cadeias de caracteres antes de renderizá-las.) A sintaxe é:</span><span class="sxs-lookup"><span data-stu-id="d75bb-167">(By default, ASP.NET Razor encodes strings before rendering them.) The syntax is:</span></span>
> 
> `Html.Raw(value)`
> 
> <span data-ttu-id="d75bb-168">O exemplo a seguir mostra como usar `Html.Raw`:</span><span class="sxs-lookup"><span data-stu-id="d75bb-168">The following example shows how to use `Html.Raw`:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]

<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="d75bb-169">Alterações na versão beta 3 para Páginas da Web do ASP.NET com a sintaxe do Razor</span><span class="sxs-lookup"><span data-stu-id="d75bb-169">Changes in Beta 3 for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="change-hrefattribute-method-removed"></a><span data-ttu-id="d75bb-170">Alteração: método "Hrefattribute" removido</span><span class="sxs-lookup"><span data-stu-id="d75bb-170">Change: "HrefAttribute" method removed</span></span>

> <span data-ttu-id="d75bb-171">O método `HrefAttribute` da classe `WebPage` foi removido.</span><span class="sxs-lookup"><span data-stu-id="d75bb-171">The `HrefAttribute` method of the `WebPage` class has been removed.</span></span> <span data-ttu-id="d75bb-172">Esse auxiliar foi usado para codificar caracteres não seguros em URLs.</span><span class="sxs-lookup"><span data-stu-id="d75bb-172">This helper was used to encode unsafe characters in URLs.</span></span> <span data-ttu-id="d75bb-173">Ele não é mais necessário porque o ASP.NET Razor codifica automaticamente as cadeias de caracteres.</span><span class="sxs-lookup"><span data-stu-id="d75bb-173">It is no longer required because ASP.NET Razor automatically encodes strings.</span></span> <span data-ttu-id="d75bb-174">(Use o novo método `Html.Raw` para renderizar cadeias de caracteres não codificadas.)</span><span class="sxs-lookup"><span data-stu-id="d75bb-174">(Use the new `Html.Raw` method to render unencoded strings.)</span></span>

#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a><span data-ttu-id="d75bb-175">Alteração: sintaxe para auxiliares "@helper" declarativos alterada</span><span class="sxs-lookup"><span data-stu-id="d75bb-175">Change: Syntax for declarative "@helper" helpers changed</span></span>

> <span data-ttu-id="d75bb-176">Na versão beta 3, o ASP.NET altera como ele analisa auxiliares criados usando a sintaxe `@helper`.</span><span class="sxs-lookup"><span data-stu-id="d75bb-176">In the Beta 3 release, ASP.NET changes how it parses helpers that are created using the `@helper` syntax.</span></span> <span data-ttu-id="d75bb-177">Em essência, a sintaxe de `@helper` agora é analisada como um bloco de código em vez de como um bloco de marcação que pode incluir código.</span><span class="sxs-lookup"><span data-stu-id="d75bb-177">In essence, the `@helper` syntax is now parsed as a code block instead of as a block of markup that can include code.</span></span> <span data-ttu-id="d75bb-178">Portanto, o código dentro do auxiliar não precisa ser colocado em blocos de `@{ }`.</span><span class="sxs-lookup"><span data-stu-id="d75bb-178">Therefore, code inside the helper does not need to be enclosed in `@{ }` blocks.</span></span> <span data-ttu-id="d75bb-179">Por outro lado, a marcação dentro do auxiliar deve ser incluída explicitamente em elementos HTML ou em marcas de `<text></text>` Razor do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d75bb-179">Conversely, markup inside the helper has to be explicitly included in HTML elements or in ASP.NET Razor `<text></text>` tags.</span></span>
> 
> <span data-ttu-id="d75bb-180">Por exemplo, a sintaxe de `@helper` a seguir funciona na versão beta 3:</span><span class="sxs-lookup"><span data-stu-id="d75bb-180">For example, the following `@helper` syntax works in the Beta 3 release:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> <span data-ttu-id="d75bb-181">Na versão beta 3, esse auxiliar deve ser alterado para ser semelhante ao exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="d75bb-181">In the Beta 3 release, this helper must be changed to look like the following example:</span></span>
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> <span data-ttu-id="d75bb-182">Observe que o `@{ }` caracteres em volta do código inicial no auxiliar não é mais usado.</span><span class="sxs-lookup"><span data-stu-id="d75bb-182">Notice that the `@{ }` characters around the initial code in the helper is no longer used.</span></span> <span data-ttu-id="d75bb-183">Isso ocorre porque o conteúdo dos auxiliares é tratado como um bloco de código por padrão.</span><span class="sxs-lookup"><span data-stu-id="d75bb-183">This is because the contents of the helpers are treated as a code block by default.</span></span> <span data-ttu-id="d75bb-184">O auxiliar renderiza a marcação, que começa com a marca de `<a>` de abertura.</span><span class="sxs-lookup"><span data-stu-id="d75bb-184">The helper renders markup, which starts with the opening `<a>` tag.</span></span> <span data-ttu-id="d75bb-185">Se o auxiliar precisar renderizar texto sem formatação ou marcas que não incluam uma marca de fechamento (por exemplo, marcas de `<meta>`), o conteúdo a ser renderizado deverá estar em marcas `<text></text>`.</span><span class="sxs-lookup"><span data-stu-id="d75bb-185">If the helper must render plain text or tags that do not include a closing tag (for example, `<meta>` tags), the content to be rendered must be in `<text></text>` tags.</span></span>

#### <a name="change-webpagecontexthttpcontext-removed"></a><span data-ttu-id="d75bb-186">Alteração: "WebPageContext. HttpContext" removido</span><span class="sxs-lookup"><span data-stu-id="d75bb-186">Change: "WebPageContext.HttpContext" removed</span></span>

> <span data-ttu-id="d75bb-187">A propriedade `WebPageContext.HttpContext` foi removida.</span><span class="sxs-lookup"><span data-stu-id="d75bb-187">The `WebPageContext.HttpContext` property has been removed.</span></span> <span data-ttu-id="d75bb-188">Use `HttpContext.Current` em vez disso.</span><span class="sxs-lookup"><span data-stu-id="d75bb-188">Use `HttpContext.Current` instead.</span></span> <span data-ttu-id="d75bb-189">(A propriedade `WebPageContext.HttpContext` simplesmente encapsulava isso.)</span><span class="sxs-lookup"><span data-stu-id="d75bb-189">(The `WebPageContext.HttpContext` property simply wrapped this.)</span></span>

#### <a name="change-facebook-helper-moved-to-new-package"></a><span data-ttu-id="d75bb-190">Alteração: o auxiliar "Facebook" foi movido para o novo pacote</span><span class="sxs-lookup"><span data-stu-id="d75bb-190">Change: "Facebook" helper moved to new package</span></span>

> <span data-ttu-id="d75bb-191">O auxiliar de `Facebook` foi movido para a biblioteca do *Facebook. Helper* , que inclui o auxiliar de `Facebook` e a funcionalidade adicional.</span><span class="sxs-lookup"><span data-stu-id="d75bb-191">The `Facebook` helper has been moved to the *Facebook.Helper* library, which includes the `Facebook` helper and additional functionality.</span></span> <span data-ttu-id="d75bb-192">Você deve instalar essa biblioteca como um pacote separado, conforme descrito em "Instalando auxiliares com o Gerenciador de pacotes" no tutorial [introdução com páginas ASP.net](https://go.microsoft.com/fwlink/?LinkId=202889).</span><span class="sxs-lookup"><span data-stu-id="d75bb-192">You must install this library as a separate package, as described in "Installing Helpers with Package Manager" in the tutorial [Getting Started with ASP.NET Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>

#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a><span data-ttu-id="d75bb-193">Alteração: Associação, função e tipos de segurança são movidos para o novo assembly</span><span class="sxs-lookup"><span data-stu-id="d75bb-193">Change: Membership, Role, and Security types moves to new assembly</span></span>

> <span data-ttu-id="d75bb-194">Os seguintes tipos foram movidos para o assembly `WebMatrix.WebData`:</span><span class="sxs-lookup"><span data-stu-id="d75bb-194">The following types were moved to the `WebMatrix.WebData` assembly:</span></span>
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`

#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a><span data-ttu-id="d75bb-195">Alteração: a classe "TagBuilder" foi movida para o assembly System. Web. webpages. dll</span><span class="sxs-lookup"><span data-stu-id="d75bb-195">Change: "TagBuilder" class moved to System.Web.WebPages.dll assembly</span></span>

> <span data-ttu-id="d75bb-196">A classe `TagBuilde` r foi movida para o assembly System. Web. webpages. dll.</span><span class="sxs-lookup"><span data-stu-id="d75bb-196">The `TagBuilde` r class has been moved to the System.Web.WebPages.dll assembly.</span></span> <span data-ttu-id="d75bb-197">Anteriormente, isso estava em um assembly que era parte do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="d75bb-197">Previously, this was in an assembly that was part of ASP.NET MVC.</span></span> <span data-ttu-id="d75bb-198">Essa alteração significa que você não precisa instalar o ASP.NET MVC para usar a classe `TagBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d75bb-198">This change means that you do not have to install ASP.NET MVC in order to use the `TagBuilder` class.</span></span>
> 
> <span data-ttu-id="d75bb-199">No entanto, a classe ainda está no namespace `System.Web.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="d75bb-199">However, the class is still in the `System.Web.Mvc` namespace.</span></span> <span data-ttu-id="d75bb-200">Para usar a classe `TagBuilder` (por exemplo, em um auxiliar personalizado do Razor do ASP.NET), você deve referenciar o namespace (por exemplo, adicionando `@using System.Web.Mvc` ao seu código).</span><span class="sxs-lookup"><span data-stu-id="d75bb-200">In order to use the `TagBuilder` class (for example, in a custom ASP.NET Razor helper), you must reference the namespace (for example, by adding `@using System.Web.Mvc` to your code).</span></span>

#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a><span data-ttu-id="d75bb-201">Alteração: sintaxe de validação de solicitação alterada; Classe de "validação" removida</span><span class="sxs-lookup"><span data-stu-id="d75bb-201">Change: Request validation syntax changed; "Validation" class removed</span></span>

> <span data-ttu-id="d75bb-202">Na versão beta 3, para desabilitar a validação de um campo individual ou de um conjunto de campos, você pode chamar o método `Validation.Exclude`, passando o nome ou os nomes dos campos a serem excluídos da validação.</span><span class="sxs-lookup"><span data-stu-id="d75bb-202">In the Beta 3 release, to disable validation for an individual field or set of fields, you can call the `Validation.Exclude` method, passing in the name or names of the fields to exclude from validation.</span></span> <span data-ttu-id="d75bb-203">Uma nova sintaxe está disponível na versão beta 3 para ignorar a validação.</span><span class="sxs-lookup"><span data-stu-id="d75bb-203">A new syntax is available in the Beta 3 release for bypassing validation.</span></span> <span data-ttu-id="d75bb-204">O método `Validation` usado na versão beta 3 foi removido.</span><span class="sxs-lookup"><span data-stu-id="d75bb-204">The `Validation` method used in Beta 3 has been removed.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="d75bb-205">Se você não desabilitar a validação de solicitação, se os usuários tentarem carregar a marcação HTML (por exemplo, usando um editor de Rich Text em uma página), o site relatará um erro como *um valor de solicitação potencialmente perigoso. formulário foi detectado no cliente* e a entrada do usuário não será aceita.</span><span class="sxs-lookup"><span data-stu-id="d75bb-205">If you do not disable request validation, if users try to upload HTML markup (for example, by using a rich text editor on a page), the website will report an error like *A potentially dangerous Request.Form value was detected from the client* and the user input is not accepted.</span></span> <span data-ttu-id="d75bb-206">Se você desabilitar a validação de solicitação, deverá verificar manualmente a entrada do usuário para certificar-se de que ela não contém marcação potencialmente perigosa ou script usando algo como a [biblioteca de scripts do Microsoft anti-cross site v 4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span><span class="sxs-lookup"><span data-stu-id="d75bb-206">If you disable request validation, you must manually check user input to make sure that it does not contain potentially dangerous markup or script using something like the [Microsoft Anti-Cross Site Scripting Library V4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).</span></span>
> 
> 
> <span data-ttu-id="d75bb-207">Para desabilitar a validação automática de solicitação, chame o método `Request.Unvalidated`, passando o nome do campo ou outro objeto post para o qual você deseja ignorar a validação de solicitação.</span><span class="sxs-lookup"><span data-stu-id="d75bb-207">To disable automatic request validation, call the `Request.Unvalidated` method, passing it the name of the field or other post object that you want to bypass request validation for.</span></span> <span data-ttu-id="d75bb-208">Você pode usar esse método para ignorar a validação de todos os itens nas coleções `Form`, `QueryString`, `Cookies`e `ServerVariables`.</span><span class="sxs-lookup"><span data-stu-id="d75bb-208">You can use this method to bypass validation for any items in the `Form`, `QueryString`, `Cookies`, and `ServerVariables` collections.</span></span> <span data-ttu-id="d75bb-209">Os exemplos a seguir mostram como usar o método `Unvalidated`:</span><span class="sxs-lookup"><span data-stu-id="d75bb-209">The following examples show how to use the `Unvalidated` method:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]

<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="d75bb-210">Problemas conhecidos para Páginas da Web do ASP.NET com a sintaxe do Razor</span><span class="sxs-lookup"><span data-stu-id="d75bb-210">Known Issues for ASP.NET Web Pages with Razor Syntax</span></span>

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a><span data-ttu-id="d75bb-211">Problema: comportamento inesperado ao usar uma tabela de usuário personalizada para associação</span><span class="sxs-lookup"><span data-stu-id="d75bb-211">Issue: Unexpected behavior when using a custom user table for membership</span></span>

> <span data-ttu-id="d75bb-212">Para inicializar o provedor de associação para um site do Razor do ASP.NET, chame o método `WebSecurity.InitializeDatabaseConnection`.</span><span class="sxs-lookup"><span data-stu-id="d75bb-212">To initialize the membership provider for an ASP.NET Razor website, you call the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="d75bb-213">(No WebMatrix, o modelo de site inicial inclui uma chamada para esse método no arquivo *\_AppStart. cshtml* ). Se o parâmetro `autoCreateTables` desse método for definido como true (por padrão, ele será definido como true no modelo de site inicial) e, se um nome de tabela não reconhecido for passado para o método (o segundo parâmetro), o método não gerará um erro.</span><span class="sxs-lookup"><span data-stu-id="d75bb-213">(In WebMatrix, the Starter Site template includes a call to this method in the *\_AppStart.cshtml* file.) If the `autoCreateTables` parameter of this method is set to true (by default, it is set to true in the Starter Site template), and if an unrecognized table name is passed to the method (the second parameter), the method does not throw an error.</span></span> <span data-ttu-id="d75bb-214">Em vez disso, ele cria automaticamente a tabela.</span><span class="sxs-lookup"><span data-stu-id="d75bb-214">Instead, it automatically creates the table.</span></span>
> 
> <span data-ttu-id="d75bb-215">Isso pode ser um problema se você pretender usar uma tabela de usuário personalizada para associação, mas passar o nome de tabela incorreto para o método `WebSecurity.InitializeDatabaseConnection`.</span><span class="sxs-lookup"><span data-stu-id="d75bb-215">This can be a problem if you intend to use a custom user table for membership but pass the wrong table name to the `WebSecurity.InitializeDatabaseConnection` method.</span></span> <span data-ttu-id="d75bb-216">Como o método não gera, por padrão, um erro se a tabela especificada não existir e, em vez disso, criar uma nova tabela, o aplicativo poderá parecer estar funcionando.</span><span class="sxs-lookup"><span data-stu-id="d75bb-216">Because the method does not by default raise an error if the table you specify does not exist, and because it instead creates a new table, the application can appear to be working.</span></span> <span data-ttu-id="d75bb-217">No entanto, o código do aplicativo que depende da sua tabela de usuário personalizada (e em campos) pode eventualmente falhar com erros inesperados.</span><span class="sxs-lookup"><span data-stu-id="d75bb-217">However, application code that relies on your custom user table (and on fields in it) can eventually fail with unexpected errors.</span></span>
> 
> <span data-ttu-id="d75bb-218">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-218">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-219">Verifique se o nome passado no método `InitializeDatabaseConnection` corresponde à tabela de perfil de usuário no banco de dados de associação ou certifique-se de que o parâmetro `autoCreateTables` está definido como false.</span><span class="sxs-lookup"><span data-stu-id="d75bb-219">Make sure that the name passed in the `InitializeDatabaseConnection` method matches the user profile table in the membership database, or make sure that the `autoCreateTables` parameter is set to false.</span></span>

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a><span data-ttu-id="d75bb-220">Problema: erro "falha ao gerar uma instância de usuário do SQL Server"</span><span class="sxs-lookup"><span data-stu-id="d75bb-220">Issue: "Failed to generate a user instance of SQL Server" error</span></span>

> <span data-ttu-id="d75bb-221">Se um aplicativo Web do WebMatrix usar SQL Server Express e estiver executando o IIS 7,5 no Windows 7 ou no Windows Server 2008 R2, você poderá ver um erro que indica que SQL Server não é possível recuperar o caminho do aplicativo local do usuário em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="d75bb-221">If a WebMatrix Web application uses SQL Server Express and is running IIS 7.5 on Windows 7 or Windows Server 2008 R2, you might see an error that indicates that SQL Server cannot retrieve the user's local application path at run time.</span></span>
> 
> <span data-ttu-id="d75bb-222">**Solução alternativa** Verifique se a conta do Windows em que o aplicativo é executado (normalmente serviço de rede) tem permissões de leitura/gravação para pastas raiz do aplicativo e para subpastas como *dados de\_de aplicativo*.</span><span class="sxs-lookup"><span data-stu-id="d75bb-222">**Workaround** Make sure that the Windows account that the application runs under (typically NETWORK SERVICE) has read/write permissions for root folders of the application and for subfolders such as *App\_Data*.</span></span> <span data-ttu-id="d75bb-223">Informações mais detalhadas estão disponíveis nos problemas do artigo da base de conhecimento [com SQL Server Express instanciação de usuário e projetos de aplicativo Web ASP.net](https://support.microsoft.com/kb/2002980).</span><span class="sxs-lookup"><span data-stu-id="d75bb-223">More detailed information is available in the KnowledgeBase article [Problems with SQL Server Express user instancing and ASP.net Web Application Projects](https://support.microsoft.com/kb/2002980).</span></span>

#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a><span data-ttu-id="d75bb-224">Problema: no Visual Studio, os namespaces para assemblies personalizados (DLLs) não são importados automaticamente</span><span class="sxs-lookup"><span data-stu-id="d75bb-224">Issue: In Visual Studio, namespaces for custom assemblies (DLLs) are not imported automatically</span></span>

> <span data-ttu-id="d75bb-225">Se você usar assemblies personalizados em um projeto no Visual Studio, os namespaces declarados nesses assemblies não serão importados automaticamente em tempo de design.</span><span class="sxs-lookup"><span data-stu-id="d75bb-225">If you use custom assemblies in a project in Visual Studio, the namespaces declared in those assemblies are not automatically imported at design time.</span></span> <span data-ttu-id="d75bb-226">Como resultado, as referências a tipos personalizados podem não ser reconhecidas em tempo de design e são marcadas como não reconhecidas no Visual Studio (usando um "rabisco").</span><span class="sxs-lookup"><span data-stu-id="d75bb-226">As a result, references to custom types might not be recognized at design time and are marked as not recognized in Visual Studio (using a "squiggle").</span></span> <span data-ttu-id="d75bb-227">Esse problema ocorre apenas em tempo de design no Visual Studio; o aplicativo em si é executado corretamente.</span><span class="sxs-lookup"><span data-stu-id="d75bb-227">This problem occurs only at design time in Visual Studio; the application itself runs properly.</span></span>
> 
> <span data-ttu-id="d75bb-228">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-228">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-229">Inclua uma instrução `using` (`imports` em Visual Basic) que faça referência às entidades que não são reconhecidas no tempo de design.</span><span class="sxs-lookup"><span data-stu-id="d75bb-229">Include a `using` statement (`imports` in Visual Basic) that references the entities that are not recognized at design time.</span></span>

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a><span data-ttu-id="d75bb-230">Problema: Visual Studio IntelliSense e modelos de projeto disponíveis somente no ASP.NET MVC versão 3</span><span class="sxs-lookup"><span data-stu-id="d75bb-230">Issue: Visual Studio IntelliSense and project templates available only in ASP.NET MVC version 3</span></span>

> <span data-ttu-id="d75bb-231">A instalação do Páginas da Web do ASP.NET não também instala ferramentas para o Visual Studio, como IntelliSense e modelos de projeto para Páginas da Web do ASP.NET aplicativos.</span><span class="sxs-lookup"><span data-stu-id="d75bb-231">Installing ASP.NET Web Pages does not also install tools for Visual Studio such as IntelliSense and project templates for ASP.NET Web Pages applications.</span></span>
> 
> <span data-ttu-id="d75bb-232">**Solução alternativa** Para usar o IntelliSense e modelos de projeto para Páginas da Web do ASP.NET aplicativos no Visual Studio, instale o ASP.NET MVC 3 RC por meio do Web Platform Installer ou do [instalador](https://go.microsoft.com/fwlink/?LinkID=191797)autônomo.</span><span class="sxs-lookup"><span data-stu-id="d75bb-232">**Workaround** To use IntelliSense and project templates for ASP.NET Web Pages applications in Visual Studio, install ASP.NET MVC 3 RC either through the Web Platform Installer or the [stand-alone installer](https://go.microsoft.com/fwlink/?LinkID=191797).</span></span>

#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a><span data-ttu-id="d75bb-233">Problema: erro "&lt;auxiliar&gt; classe não pode ser encontrado"</span><span class="sxs-lookup"><span data-stu-id="d75bb-233">Issue: "&lt;helper&gt; class cannot be found" error</span></span>

> <span data-ttu-id="d75bb-234">Depois de atualizar para a versão beta 3, você poderá ver um erro de que uma classe auxiliar (por exemplo, a classe `Facebook`) não pode ser encontrada.</span><span class="sxs-lookup"><span data-stu-id="d75bb-234">After you upgrade to Beta 3, you might see an error that a helper class (for example, the `Facebook` class) cannot not be found.</span></span> <span data-ttu-id="d75bb-235">A partir da versão beta 2 e continuando na versão beta 3, os auxiliares foram movidos para os pacotes que você deve instalar explicitamente.</span><span class="sxs-lookup"><span data-stu-id="d75bb-235">Starting in Beta 2 and continuing in Beta 3, helpers have been moved to packages that you must explicitly install.</span></span> <span data-ttu-id="d75bb-236">Os sites existentes não são atualizados para incluir esses pacotes; Isso inclui sites nas pastas da Web de *\Meus Documents\IISExpress* ou *\Meus documentos\Minhas sites* .</span><span class="sxs-lookup"><span data-stu-id="d75bb-236">Existing sites are not upgraded to include these packages; this includes sites in the *\My Documents\IISExpress* or *\My Documents\My Web Sites* folders.</span></span> <span data-ttu-id="d75bb-237">Em particular, você verá esse erro se usar o site padrão em *meus sites* (WebSite1), que inclui uma referência ao auxiliar de `Twitter`.</span><span class="sxs-lookup"><span data-stu-id="d75bb-237">In particular, you will see this error if you use the default site in *My Sites* (WebSite1), which includes a reference to the `Twitter` helper.</span></span>
> 
> <span data-ttu-id="d75bb-238">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-238">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-239">Comente as chamadas para os auxiliares no site, execute a página *\_admin* e instale o pacote ou os pacotes que incluem os auxiliares que você deseja usar.</span><span class="sxs-lookup"><span data-stu-id="d75bb-239">Comment out calls to any helpers in the site, run the *\_Admin* page, and install the package or packages that include the helpers that you want to use.</span></span> <span data-ttu-id="d75bb-240">Depois de instalar o pacote, você pode remover os comentários das linhas que fazem referência aos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="d75bb-240">After you've installed the package, you can uncomment the lines that reference helpers.</span></span>

#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a><span data-ttu-id="d75bb-241">Problema: a implantação de assemblies do Razor do beta 3 ASP.NET na pasta bin pode não funcionar em sites de hospedagem</span><span class="sxs-lookup"><span data-stu-id="d75bb-241">Issue: Deploying Beta 3 ASP.NET Razor assemblies to the Bin folder might not work on hosting sites</span></span>

> <span data-ttu-id="d75bb-242">Se você implantar um site Páginas da Web do ASP.NET em um site de hospedagem e, se implantar os assemblies do ASP.NET Razor Beta 3 na pasta *bin* do site, poderá encontrar erros, incluindo o seguinte:</span><span class="sxs-lookup"><span data-stu-id="d75bb-242">If you deploy an ASP.NET Web Pages website to a hosting site, and if you deploy the ASP.NET Razor Beta 3 assemblies to the site's *Bin* folder, you might experience errors, including the following:</span></span>
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> <span data-ttu-id="d75bb-243">Isso pode acontecer se o provedor de hospedagem tiver instalado os assemblies Páginas da Web do ASP.NET beta 1 no cache de aplicativo global (GAC) do servidor.</span><span class="sxs-lookup"><span data-stu-id="d75bb-243">This can happen if the hosting provider has installed the ASP.NET Web Pages Beta 1 assemblies into the server's global application cache (GAC).</span></span> <span data-ttu-id="d75bb-244">Os assemblies no GAC obtêm precedência sobre assemblies instalados localmente na pasta *bin* .</span><span class="sxs-lookup"><span data-stu-id="d75bb-244">Assemblies in the GAC get precedence over assemblies installed locally in the *Bin* folder.</span></span>
> 
> <span data-ttu-id="d75bb-245">**Solução alternativa** Entre em contato com seu provedor de hospedagem para confirmar se os erros que você está vendo estão devido a um conflito entre as versões do provedor dos assemblies e seus.</span><span class="sxs-lookup"><span data-stu-id="d75bb-245">**Workaround** Contact your hosting provider to confirm that the errors you are seeing are due to a conflict between the provider's versions of the assemblies and yours.</span></span> <span data-ttu-id="d75bb-246">Nesse caso, solicite que o provedor de hospedagem atualize os assemblies no GAC do servidor.</span><span class="sxs-lookup"><span data-stu-id="d75bb-246">If so, request that the hosting provider update the assemblies in the server's GAC.</span></span>

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a><span data-ttu-id="d75bb-247">Problema: lendo feeds ou outros dados externos por meio de um servidor proxy</span><span class="sxs-lookup"><span data-stu-id="d75bb-247">Issue: Reading feeds or other external data via a proxy server</span></span>

> <span data-ttu-id="d75bb-248">Se o servidor que está executando o site estiver protegido por um servidor proxy, talvez seja necessário configurar as informações de proxy no arquivo *Web. config* para poder ler as informações provenientes de fora do site.</span><span class="sxs-lookup"><span data-stu-id="d75bb-248">If the server running the site is behind a proxy server, you might need to configure proxy information in the *Web.config* file in order to be able to read information that comes from outside your site.</span></span> <span data-ttu-id="d75bb-249">Por exemplo, se você usar o auxiliar de `ReCaptcha`, o auxiliar se comunicará com o serviço reCAPTCHA, mas poderá ser bloqueado pelo servidor proxy.</span><span class="sxs-lookup"><span data-stu-id="d75bb-249">For example, if you use the `ReCaptcha` helper, the helper communicates with the reCAPTCHA service, but might be blocked by your proxy server.</span></span> <span data-ttu-id="d75bb-250">Da mesma forma, os feeds usados no Páginas da Web do ASP.NET, como o feed usado pelo Gerenciador de pacotes, podem exigir a configuração de proxy.</span><span class="sxs-lookup"><span data-stu-id="d75bb-250">Similarly, feeds that are used in ASP.NET Web Pages, such as the feed used by the package manager, might require proxy configuration.</span></span>
> 
> <span data-ttu-id="d75bb-251">Se você tiver problemas ao trabalhar com um serviço externo ou trabalhar com o feed de pacote, coloque os seguintes elementos no arquivo *Web. config* da raiz do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="d75bb-251">If you experience problems in working with an external service or working with the package feed, put the following elements into your application's root *Web.config* file:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> <span data-ttu-id="d75bb-252">Para obter mais informações sobre como configurar um servidor proxy, consulte [&lt;elemento&gt; proxy (configurações de rede)](https://msdn.microsoft.com/library/sa91de1e.aspx) no site do MSDN.</span><span class="sxs-lookup"><span data-stu-id="d75bb-252">For more information about configuring a proxy server, see [&lt;proxy&gt; Element (Network Settings)](https://msdn.microsoft.com/library/sa91de1e.aspx) on the MSDN Web site.</span></span>

#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a><span data-ttu-id="d75bb-253">Problema: erro "Microsoft. Web. Infrastructure. dll não pode ser carregado"</span><span class="sxs-lookup"><span data-stu-id="d75bb-253">Issue: "Microsoft.Web.Infrastructure.dll cannot be loaded" error</span></span>

> <span data-ttu-id="d75bb-254">Se você tiver instalado anteriormente a versão beta 1 do Páginas da Web do ASP.NET com sintaxe Razor e, em seguida, instalar a versão beta 3, todos os assemblies apropriados serão instalados no GAC, exceto *Microsoft. Web. Infrastructure. dll*.</span><span class="sxs-lookup"><span data-stu-id="d75bb-254">If you previously installed the Beta 1 version of ASP.NET Web Pages with Razor syntax and then install the Beta 3 version, all appropriate assemblies are installed in the GAC except *Microsoft.Web.Infrastructure.dll*.</span></span> <span data-ttu-id="d75bb-255">Como consequência, ao executar ASP.NET páginas Razor, você verá um erro que indica que *Microsoft. Web. Infrastructure. dll* não pôde ser carregado.</span><span class="sxs-lookup"><span data-stu-id="d75bb-255">As a consequence, when you run ASP.NET Razor pages, you see an error that indicates that *Microsoft.Web.Infrastructure.dll* could not be loaded.</span></span>
> 
> <span data-ttu-id="d75bb-256">Esse problema não ocorrerá se você carregou a versão beta 3 em um computador limpo.</span><span class="sxs-lookup"><span data-stu-id="d75bb-256">This issue does not occur if you loaded the Beta 3 release on a clean computer.</span></span>
> 
> <span data-ttu-id="d75bb-257">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-257">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-258">No painel de controle, desinstale o Páginas da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d75bb-258">In Control Panel, uninstall ASP.NET Web Pages.</span></span> <span data-ttu-id="d75bb-259">Em seguida, reinstale a versão beta 3.</span><span class="sxs-lookup"><span data-stu-id="d75bb-259">Then reinstall the Beta 3 release.</span></span>

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a><span data-ttu-id="d75bb-260">Problema: a desinstalação do .NET Framework versão 4 desabilita Páginas da Web do ASP.NET com a sintaxe do Razor</span><span class="sxs-lookup"><span data-stu-id="d75bb-260">Issue: Uninstalling the .NET Framework version 4 disables ASP.NET Web Pages with Razor Syntax</span></span>

> <span data-ttu-id="d75bb-261">Se você desinstalar o .NET Framework versão 4 e, em seguida, reinstalá-lo, Páginas da Web do ASP.NET com sintaxe Razor será desabilitado.</span><span class="sxs-lookup"><span data-stu-id="d75bb-261">If you uninstall the .NET Framework version 4 and then reinstall it, ASP.NET Web Pages with Razor syntax is disabled.</span></span> <span data-ttu-id="d75bb-262">As páginas com a extensão *. cshtml* não são executadas corretamente.</span><span class="sxs-lookup"><span data-stu-id="d75bb-262">Pages with the *.cshtml* extension do not run correctly.</span></span> <span data-ttu-id="d75bb-263">Páginas da Web do ASP.NET registra um assembly no arquivo *Web. config da* raiz do computador e a remoção do .NET Framework remove esse arquivo.</span><span class="sxs-lookup"><span data-stu-id="d75bb-263">ASP.NET Web Pages registers an assembly in the machine root *Web.config* file, and removing the .NET Framework removes that file.</span></span> <span data-ttu-id="d75bb-264">Reinstalar o .NET Framework instala uma nova versão do arquivo de configuração, mas não adiciona a referência para o assembly Páginas da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d75bb-264">Reinstalling the .NET Framework installs a new version of the configuration file, but does not add the reference for the ASP.NET Web Pages assembly.</span></span>
> 
> <span data-ttu-id="d75bb-265">**Solução alternativa** Depois de reinstalar o .NET Framework, reinstale o Páginas da Web do ASP.NET com sintaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="d75bb-265">**Workaround** After reinstalling the .NET Framework, reinstall ASP.NET Web Pages with Razor syntax.</span></span> <span data-ttu-id="d75bb-266">Isso adiciona o seguinte elemento ao arquivo *Web. config* na raiz do computador, que normalmente está no seguinte local:</span><span class="sxs-lookup"><span data-stu-id="d75bb-266">This adds the following element to the *Web.config* file in the machine root, which is typically in the following location:</span></span>  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]

#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a><span data-ttu-id="d75bb-267">Problema: os aplicativos implantados anteriormente com assemblies ASP.NET na pasta bin apresentam erros</span><span class="sxs-lookup"><span data-stu-id="d75bb-267">Issue: Applications previously deployed with ASP.NET assemblies in the Bin folder experience errors</span></span>

> <span data-ttu-id="d75bb-268">Durante a implantação, as cópias dos assemblies de Páginas da Web do ASP.NET (por exemplo, *Microsoft. webpages. dll*) para a pasta *bin* do site no servidor.</span><span class="sxs-lookup"><span data-stu-id="d75bb-268">During deployment, copies of the ASP.NET Web Pages assemblies (for example, *Microsoft.WebPages.dll*) to the *Bin* folder of the website on the server.</span></span> <span data-ttu-id="d75bb-269">(Isso pode ter ocorrido automaticamente durante a implantação ou porque o desenvolvedor copiou explicitamente os assemblies.) No entanto, quando a versão beta 3 estiver instalada, ocorrerão erros, como erros que determinados tipos não podem ser encontrados.</span><span class="sxs-lookup"><span data-stu-id="d75bb-269">(This might have happened automatically during deployment or because the developer explicitly copied the assemblies.) However, when the Beta 3 release is installed, errors occurs, such as errors that certain types cannot be found.</span></span> <span data-ttu-id="d75bb-270">Isso ocorre porque vários tipos de Páginas da Web do ASP.NET foram movidos para namespaces diferentes para a versão beta 3.</span><span class="sxs-lookup"><span data-stu-id="d75bb-270">This occurs because a number of ASP.NET Web Pages types were moved into different namespaces for the Beta 3 release.</span></span>
> 
> <span data-ttu-id="d75bb-271">**Solução alternativa** </span><span class="sxs-lookup"><span data-stu-id="d75bb-271">**Workaround** </span></span>  
> <span data-ttu-id="d75bb-272">Limpe a pasta *bin* do aplicativo implantado, copie os novos assemblies para a pasta (ou reimplante o aplicativo) e reinicie o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d75bb-272">Clear the *Bin* folder of the deployed application, copy the new assemblies to the folder (or redeploy the application), and then restart the application.</span></span>

#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a><span data-ttu-id="d75bb-273">Problema: URLs sem extensão não localizam arquivos. cshtml/. vbhtml no IIS 7 ou IIS 7,5</span><span class="sxs-lookup"><span data-stu-id="d75bb-273">Issue: Extensionless URLs do not find .cshtml/.vbhtml files on IIS 7 or IIS 7.5</span></span>

> <span data-ttu-id="d75bb-274">No IIS 7 ou IIS 7,5, as solicitações com uma URL como esta não são capazes de localizar páginas que tenham a extensão *. cshtml* ou *. vbhtml* :</span><span class="sxs-lookup"><span data-stu-id="d75bb-274">On IIS 7 or IIS 7.5, requests with a URL like the following are not able to find pages that have the *.cshtml* or *.vbhtml* extension:</span></span>  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> <span data-ttu-id="d75bb-275">O problema ocorre porque a regravação de URL não está habilitada por padrão para o IIS 7 ou o IIS 7,5.</span><span class="sxs-lookup"><span data-stu-id="d75bb-275">The issue arises because URL rewriting is not enabled by default for IIS 7 or IIS 7.5.</span></span> <span data-ttu-id="d75bb-276">O cenário likeliest é que você não vê o problema ao testar localmente usando IIS Express, mas você o experimenta quando implanta seu site em um site de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="d75bb-276">The likeliest scenario is that you do not see the problem when testing locally using IIS Express, but you experience it when you deploy your website to a hosting website.</span></span>
> 
> <span data-ttu-id="d75bb-277">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-277">**Workaround**</span></span>
> 
> - <span data-ttu-id="d75bb-278">Se você tiver controle sobre o computador servidor, no computador servidor, instale a atualização descrita em [uma atualização disponível que permite que determinados manipuladores do iis 7,0 ou iis 7,5 manipulem solicitações cujas URLs não terminam com um ponto](https://support.microsoft.com/kb/980368).</span><span class="sxs-lookup"><span data-stu-id="d75bb-278">If you have control over the server computer, on the server computer install the update that is described in [A update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).</span></span>
> - <span data-ttu-id="d75bb-279">Se você não tiver controle sobre o computador servidor (por exemplo, você está implantando em um site de hospedagem), adicione o seguinte ao arquivo *Web. config* do site:</span><span class="sxs-lookup"><span data-stu-id="d75bb-279">If you do not have control over the server computer (for example, you are deploying to a hosting website), add the following to the website's *Web.config* file:</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]

#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a><span data-ttu-id="d75bb-280">Problema: usando o projeto de aplicativo Web ou o ASP.NET MVC e as páginas da Web do ASP.NET no mesmo aplicativo</span><span class="sxs-lookup"><span data-stu-id="d75bb-280">Issue: Using Web Application Project or ASP.NET MVC and ASP.NET Web pages in the same application</span></span>

> <span data-ttu-id="d75bb-281">Se você estiver usando Páginas da Web do ASP.NET em um projeto de aplicativo Web ou em um aplicativo ASP.NET MVC, poderá ver um erro de que *WebPageHttpApplication* não pode ser encontrado.</span><span class="sxs-lookup"><span data-stu-id="d75bb-281">If you were using ASP.NET Web Pages in a Web Application project or ASP.NET MVC application, you might see an error that *WebPageHttpApplication* cannot be found.</span></span>
> 
> <span data-ttu-id="d75bb-282">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-282">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-283">Se você receber esse erro, altere a classe base da qual o aplicativo deriva.</span><span class="sxs-lookup"><span data-stu-id="d75bb-283">If you get this error, change the base class from which the application derives.</span></span> <span data-ttu-id="d75bb-284">No arquivo *global. asax* , altere a seguinte linha:</span><span class="sxs-lookup"><span data-stu-id="d75bb-284">In the *Global.asax* file, change the following line:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> <span data-ttu-id="d75bb-285">Para isso:</span><span class="sxs-lookup"><span data-stu-id="d75bb-285">To this:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> <span data-ttu-id="d75bb-286">Isso, em vigor, reverte uma alteração que foi introduzida para a versão beta 1 do Páginas da Web do ASP.NET com sintaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="d75bb-286">This in effect reverses a change that was introduced for the Beta 1 release of ASP.NET Web Pages with Razor syntax.</span></span>

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a><span data-ttu-id="d75bb-287">Problema: Implantando um aplicativo em um computador que não tem SQL Server Compact instalado</span><span class="sxs-lookup"><span data-stu-id="d75bb-287">Issue: Deploying an application to a computer that does not have SQL Server Compact installed</span></span>

> <span data-ttu-id="d75bb-288">Os aplicativos que incluem bancos de dados do SQL Server Compact podem ser executados em um computador onde o SQL Server Compact não está instalado.</span><span class="sxs-lookup"><span data-stu-id="d75bb-288">Applications that include SQL Server Compact databases can run on a computer where SQL Server Compact is not installed.</span></span> <span data-ttu-id="d75bb-289">O Microsoft WebMatrix Beta 3 copia automaticamente esses binários para você e executa as transformações de arquivo *Web. config* apropriadas.</span><span class="sxs-lookup"><span data-stu-id="d75bb-289">Microsoft WebMatrix Beta 3 automatically copies these binaries for you and performs the appropriate *Web.config* file transforms.</span></span>
> 
> <span data-ttu-id="d75bb-290">**Solução alternativa** Se você precisar copiar esses arquivos e fazer com que o arquivo *Web. config* seja alterado manualmente, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="d75bb-290">**Workaround** If you need to copy these files and make the *Web.config* file changes manually, do the following :</span></span>
> 
> 1. <span data-ttu-id="d75bb-291">Copie os assemblies do mecanismo de banco de dados para a pasta *bin* (e subpastas) do aplicativo no computador de destino:</span><span class="sxs-lookup"><span data-stu-id="d75bb-291">Copy the database engine assemblies to the *Bin* folder (and subfolders) of the application on the target computer:</span></span> 
> 
>     - <span data-ttu-id="d75bb-292">Copiar *c:\Arquivos de programas\microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **para** *\Bin*</span><span class="sxs-lookup"><span data-stu-id="d75bb-292">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **to** *\Bin*</span></span>
>     - <span data-ttu-id="d75bb-293">Copie *c:\Arquivos de programas\microsoft SQL Server Compact Edition\v4.0\Private\x86\\* \* **para** *\Bin\x86*</span><span class="sxs-lookup"><span data-stu-id="d75bb-293">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\x86\\*\* **to** *\Bin\x86*</span></span>
>     - <span data-ttu-id="d75bb-294">Copie *c:\Arquivos de programas\microsoft SQL Server Compact Edition\v4.0\Private\amd64\\* \* **para** *\Bin\amd64*</span><span class="sxs-lookup"><span data-stu-id="d75bb-294">Copy *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private\amd64\\*\* **to** *\Bin\amd64*</span></span>
> 2. <span data-ttu-id="d75bb-295">Na pasta raiz do site, crie ou abra um arquivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="d75bb-295">In the root folder of the website, create or open a *Web.config* file.</span></span> <span data-ttu-id="d75bb-296">(No WebMatrix Beta 3, esse tipo de arquivo estará disponível se você clicar em **tudo** na caixa de diálogo **escolher um tipo de arquivo** .)</span><span class="sxs-lookup"><span data-stu-id="d75bb-296">(In WebMatrix Beta 3, this file type is available if you click **All** in the **Choose a File Type** dialog box.)</span></span>
> 3. <span data-ttu-id="d75bb-297">Adicione o seguinte elemento como um filho do elemento **&lt;configuration&gt;** (não dentro do elemento **&lt;system. Web&gt;** ):</span><span class="sxs-lookup"><span data-stu-id="d75bb-297">Add the following element as a child of the **&lt;configuration&gt;** element (not inside the **&lt;system.web&gt;** element):</span></span>
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a><span data-ttu-id="d75bb-298">Problema: os auxiliares de banco de dados e WebGrid não funcionam em confiança média no Visual Basic</span><span class="sxs-lookup"><span data-stu-id="d75bb-298">Issue: Database and WebGrid helpers do not work in Medium Trust in Visual Basic</span></span>

> <span data-ttu-id="d75bb-299">Se você estiver usando Visual Basic (criando arquivos *. vbhtml* ), os auxiliares `Database` e `WebGrid` não funcionarão se o aplicativo for definido para usar a confiança média.</span><span class="sxs-lookup"><span data-stu-id="d75bb-299">If you are using Visual Basic (creating *.vbhtml* files), the `Database` and `WebGrid` helpers will not work if the application is set to use Medium Trust.</span></span>
> 
> <span data-ttu-id="d75bb-300">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-300">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-301">Defina temporariamente o aplicativo para usar confiança total.</span><span class="sxs-lookup"><span data-stu-id="d75bb-301">Temporarily set the application to use Full Trust.</span></span>

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a><span data-ttu-id="d75bb-302">SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="d75bb-302">SQL Server Compact</span></span>

#### <a name="issue-encrypt-property-is-not-recognized"></a><span data-ttu-id="d75bb-303">Problema: a propriedade "Encrypt" não é reconhecida</span><span class="sxs-lookup"><span data-stu-id="d75bb-303">Issue: "Encrypt" property is not recognized</span></span>

> <span data-ttu-id="d75bb-304">SQL Server Compact 4,0 não reconhece a propriedade `Encrypt` da classe `SqlCeConnection`.</span><span class="sxs-lookup"><span data-stu-id="d75bb-304">SQL Server Compact 4.0 does not recognize the `Encrypt` property of the `SqlCeConnection` class.</span></span> <span data-ttu-id="d75bb-305">Você não deve usar essa propriedade para criptografar arquivos de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d75bb-305">You should not use this property to encrypt database files.</span></span> <span data-ttu-id="d75bb-306">A propriedade `Encrypt` foi preterida na versão SQL Server Compact 3,5 e foi retida apenas para compatibilidade com versões anteriores.</span><span class="sxs-lookup"><span data-stu-id="d75bb-306">The `Encrypt` property was deprecated in SQL Server Compact 3.5 release and was retained only for backward compatibility.</span></span> 
> 
> <span data-ttu-id="d75bb-307">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-307">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-308">Use a propriedade `Encryption Mode` da classe `SqlCeConnection` para criptografar arquivos de banco de dados SQL Server Compact 4,0.</span><span class="sxs-lookup"><span data-stu-id="d75bb-308">Use the `Encryption Mode` property of the `SqlCeConnection` class to encrypt SQL Server Compact 4.0 database files.</span></span> <span data-ttu-id="d75bb-309">O exemplo a seguir mostra como criar um banco de dados criptografado SQL Server Compact 4,0 usando a propriedade `Encryption Mode`:</span><span class="sxs-lookup"><span data-stu-id="d75bb-309">The following example shows how to create an encrypted SQL Server Compact 4.0 database using the `Encryption Mode` property:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> <span data-ttu-id="d75bb-310">Para alterar o modo de criptografia de um banco de dados SQL Server Compact 4,0 existente, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="d75bb-310">To change the encryption mode of an existing SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> <span data-ttu-id="d75bb-311">Para criptografar um banco de dados do SQL Server Compact 4,0 não criptografado, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="d75bb-311">To encrypt an unencrypted SQL Server Compact 4.0 database, do the following:</span></span>
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]

#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a><span data-ttu-id="d75bb-312">Problema: as bibliotecas C++ de tempo de execução do Microsoft Visual 2008 são necessárias</span><span class="sxs-lookup"><span data-stu-id="d75bb-312">Issue: Microsoft Visual C++ 2008 runtime libraries are required</span></span>

> <span data-ttu-id="d75bb-313">As DLLs nativas do SQL Server Compact 4,0 precisam das bibliotecas C++ de tempo de execução do Microsoft Visual 2008 (x86, IA64 e x64), Service Pack 1.</span><span class="sxs-lookup"><span data-stu-id="d75bb-313">The native DLLs of SQL Server Compact 4.0 need the Microsoft Visual C++ 2008 Runtime Libraries (x86, IA64, and x64), Service Pack 1.</span></span>
> 
> <span data-ttu-id="d75bb-314">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-314">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-315">Instale o .NET Framework 3,5 SP1.</span><span class="sxs-lookup"><span data-stu-id="d75bb-315">Install the .NET Framework 3.5 SP1.</span></span> <span data-ttu-id="d75bb-316">Isso também instala as bibliotecas C++ de tempo de execução do Visual 2008 SP1.</span><span class="sxs-lookup"><span data-stu-id="d75bb-316">This also installs the Visual C++ 2008 Runtime Libraries SP1.</span></span> <span data-ttu-id="d75bb-317">Você pode baixar as bibliotecas do seguinte local:</span><span class="sxs-lookup"><span data-stu-id="d75bb-317">You can download the libraries from the following location:</span></span>   
>   
> [<span data-ttu-id="d75bb-318">Atualização de C++ segurança da ATL do pacote redistribuível do Microsoft Visual 2008 Service Pack 1</span><span class="sxs-lookup"><span data-stu-id="d75bb-318">Microsoft Visual C++ 2008 Service Pack 1 Redistributable Package ATL Security Update</span></span>](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> <span data-ttu-id="d75bb-319">Observe que a instalação do .NET Framework 2,0, 3,0 ou 4 *não* instala as bibliotecas de C++ tempo de execução do Visual 2008 SP1.</span><span class="sxs-lookup"><span data-stu-id="d75bb-319">Note that installing the .NET Framework 2.0, 3.0, or 4 does *not* install the Visual C++ 2008 Runtime Libraries SP1.</span></span>

#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a><span data-ttu-id="d75bb-320">Problema: se o SQL Server Compact for instalado antes da instalação do .NET Framework no computador, o nome invariável do provedor não será registrado no arquivo Machine. config do .NET Framework</span><span class="sxs-lookup"><span data-stu-id="d75bb-320">Issue: If SQL Server Compact is installed prior to installing .NET Framework on the computer, its provider invariant name is not registered in the .NET Framework machine.config file</span></span>

> <span data-ttu-id="d75bb-321">SQL Server Compact pode ser instalado em um computador que não tenha o .NET Framework instalado porque SQL Server Compact requer o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d75bb-321">SQL Server Compact can be installed on a machine that does not have .NET Framework installed because SQL Server Compact does require the .NET framework.</span></span> <span data-ttu-id="d75bb-322">Se nenhuma .NET Framework versão 3,5 nem 4 estiver instalada antes de instalar o SQL Server Compact, a instalação do SQL Server Compact não registrará seu nome invariável do provedor no arquivo *Machine. config* .</span><span class="sxs-lookup"><span data-stu-id="d75bb-322">If neither .NET Framework version 3.5 nor 4 is installed before you install SQL Server Compact, the SQL Server Compact Setup does not register its provider invariant name in the *machine.config* file.</span></span> <span data-ttu-id="d75bb-323">Qualquer aplicativo que dependa da entrada SQL Server Compact no arquivo *Machine. config* falhará.</span><span class="sxs-lookup"><span data-stu-id="d75bb-323">Any application that relies on the SQL Server Compact entry in the *machine.config* file will fail.</span></span> <span data-ttu-id="d75bb-324">A entrada de registro de nome invariável em *Machine. config* é semelhante ao exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="d75bb-324">The invariant name registration entry in *machine.config* looks like the following example:</span></span>
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> <span data-ttu-id="d75bb-325">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-325">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-326">Desinstale SQL Server Compact 4,0 CTP1.</span><span class="sxs-lookup"><span data-stu-id="d75bb-326">Uninstall SQL Server Compact 4.0 CTP1.</span></span> <span data-ttu-id="d75bb-327">Baixe e instale as versões completas do .NET Framework do seguinte local:</span><span class="sxs-lookup"><span data-stu-id="d75bb-327">Download and install the full versions of the .NET Framework from the following location:</span></span>
> 
> [<span data-ttu-id="d75bb-328">Microsoft .NET Framework 3,5 Service Pack 1 (pacote completo)</span><span class="sxs-lookup"><span data-stu-id="d75bb-328">Microsoft .NET Framework 3.5 Service pack 1 (Full Package)</span></span>](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [<span data-ttu-id="d75bb-329">Versão do Microsoft .NET Framework 4,0 (pacote completo)</span><span class="sxs-lookup"><span data-stu-id="d75bb-329">Microsoft .NET Framework 4.0 Release (Full Package)</span></span>](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> <span data-ttu-id="d75bb-330">Em seguida, reinstale [SQL Server Compact 4,0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span><span class="sxs-lookup"><span data-stu-id="d75bb-330">Then reinstall [SQL Server Compact 4.0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).</span></span>

<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a><span data-ttu-id="d75bb-331">Instalando aplicativos</span><span class="sxs-lookup"><span data-stu-id="d75bb-331">Installing Applications</span></span>

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a><span data-ttu-id="d75bb-332">Problema: a instalação de um aplicativo pode levar muito tempo se a pasta meus documentos do usuário for redirecionada para um compartilhamento de rede</span><span class="sxs-lookup"><span data-stu-id="d75bb-332">Issue: Installing an application can take a long time if the user's My Documents folder is redirected to a network share</span></span>

> <span data-ttu-id="d75bb-333">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-333">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-334">Nenhum.</span><span class="sxs-lookup"><span data-stu-id="d75bb-334">None.</span></span> <span data-ttu-id="d75bb-335">O aplicativo pode levar algum tempo para ser instalado, mas será instalado corretamente.</span><span class="sxs-lookup"><span data-stu-id="d75bb-335">The application might take a while to install, but will install correctly.</span></span>

<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a><span data-ttu-id="d75bb-336">Publicando aplicativos</span><span class="sxs-lookup"><span data-stu-id="d75bb-336">Publishing Applications</span></span>

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a><span data-ttu-id="d75bb-337">Problema: o site pode não funcionar após a publicação se o campo "URL de destino" não for prefixado com http://ou https://</span><span class="sxs-lookup"><span data-stu-id="d75bb-337">Issue: Site might not work after publishing if the "Destination URL" field is not prefixed with http:// or https://</span></span>

> <span data-ttu-id="d75bb-338">Na caixa de diálogo **configurações de publicação** , se a URL de destino não começar com `http://` ou `https://`, o site poderá não funcionar após a implantação.</span><span class="sxs-lookup"><span data-stu-id="d75bb-338">In the **Publishing Settings** dialog box, if the destination URL does not begin with `http://` or `https://`, the site might not work after deployment.</span></span>
> 
> <span data-ttu-id="d75bb-339">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-339">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-340">Certifique-se de que, antes de publicar um site, a URL de destino na caixa de diálogo **configurações de publicação** comece com `http://` ou `https://`.</span><span class="sxs-lookup"><span data-stu-id="d75bb-340">Make sure that before you publish a site, the destination URL in the **Publish Settings** dialog box starts with `http://` or `https://`.</span></span>

#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a><span data-ttu-id="d75bb-341">Problema: a publicação de um banco de dados MySQL falha com o erro "falha ao publicar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="d75bb-341">Issue: Publishing a MySQL database fails with the error "Failed to publish the database.</span></span> <span data-ttu-id="d75bb-342">Isso pode acontecer se o banco de dados remoto não puder executar o script. "</span><span class="sxs-lookup"><span data-stu-id="d75bb-342">This can happen if the remote database cannot run the script."</span></span>

> <span data-ttu-id="d75bb-343">O erro pode ocorrer por vários motivos.</span><span class="sxs-lookup"><span data-stu-id="d75bb-343">The error can occur for a number of reasons.</span></span> <span data-ttu-id="d75bb-344">Um motivo para você ver esse erro é se o script de banco de dados contiver um caractere de aspas simples (') e o conjunto de caracteres padrão do banco de dados MySQL de destino não for UTF-8.</span><span class="sxs-lookup"><span data-stu-id="d75bb-344">One reason you can see this error is if the database script contains a single quotation character (') and the destination MySQL database's default character set is not to UTF-8.</span></span>
> 
> <span data-ttu-id="d75bb-345">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-345">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-346">Defina o conjunto de caracteres padrão para o banco de dados MySQL remoto como UTF-8.</span><span class="sxs-lookup"><span data-stu-id="d75bb-346">Set the default character set for the remote MySQL database to UTF-8.</span></span>

<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a><span data-ttu-id="d75bb-347">Outros problemas</span><span class="sxs-lookup"><span data-stu-id="d75bb-347">Other Issues</span></span>

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a><span data-ttu-id="d75bb-348">Problema: a pesquisa/filtro não funciona em relatórios para Group by: tipo de problema</span><span class="sxs-lookup"><span data-stu-id="d75bb-348">Issue: Search/Filter does not work in Reports for Group By: Issue Type</span></span>

> <span data-ttu-id="d75bb-349">Ao executar um relatório para um site, se você inserir texto na caixa *Filtrar por URL* e clicar em *Pesquisar*, nada acontecerá.</span><span class="sxs-lookup"><span data-stu-id="d75bb-349">When you run a report for a site, if you enter text in the *Filter by URL* box and click *Search*, nothing happens.</span></span> <span data-ttu-id="d75bb-350">Isso ocorre porque esse controle não é funcional enquanto o estado *Group by* do relatório é definido como *tipo de emissão*, que é o padrão.</span><span class="sxs-lookup"><span data-stu-id="d75bb-350">This is because this control is not functional while the *Group By* state of the report is set to *Issue Type*, which is the default.</span></span>
> 
> <span data-ttu-id="d75bb-351">**Solução alternativa** Na guia *Agrupar por* da faixa de, clique em *URL* para agrupar as entradas por sua URL de origem.</span><span class="sxs-lookup"><span data-stu-id="d75bb-351">**Workaround** In the *Group By* tab of ribbon, click *URL* to group the entries by their source URL.</span></span> <span data-ttu-id="d75bb-352">A caixa de texto e o botão para filtrar as entradas são funcionais enquanto estão nesse estado.</span><span class="sxs-lookup"><span data-stu-id="d75bb-352">The text box and button to filter the entries are functional while in this state.</span></span>

#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a><span data-ttu-id="d75bb-353">Problema: falha na execução de aplicativos WCF com o IIS Express</span><span class="sxs-lookup"><span data-stu-id="d75bb-353">Issue: WCF applications fail to run with IIS Express</span></span>

> <span data-ttu-id="d75bb-354">A navegação para um aplicativo WCF resulta em um erro como o seguinte:</span><span class="sxs-lookup"><span data-stu-id="d75bb-354">Browsing to a WCF application results in an error like the following one:</span></span>
> 
> <span data-ttu-id="d75bb-355">*Não foi possível carregar o arquivo ou o assembly ' Microsoft. Web. Administration, versão = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' ou uma de suas dependências. O sistema não pode localizar o arquivo especificado.*</span><span class="sxs-lookup"><span data-stu-id="d75bb-355">*Could not load file or assembly 'Microsoft.Web.Administration, Version=7.0.0.0, Culture=neutral,PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.*</span></span>
> 
> <span data-ttu-id="d75bb-356">Isso ocorre porque IIS Express versão beta não dá suporte ao WCF por padrão.</span><span class="sxs-lookup"><span data-stu-id="d75bb-356">This occurs because IIS Express Beta release doesn't support WCF by default.</span></span>
> 
> <span data-ttu-id="d75bb-357">**Solução alternativa** Use qualquer uma das seguintes soluções alternativas (a solução alternativa #2 requer o Microsoft Windows Vista ou superior):</span><span class="sxs-lookup"><span data-stu-id="d75bb-357">**Workaround** Use any one of the following workarounds (workaround #2 requires Microsoft Windows Vista or higher):</span></span>
> 
> 
> 1. <span data-ttu-id="d75bb-358">Copie os assemblies *Microsoft. Web. dll* e *Microsoft. Web. Administration. dll* do local de instalação do WebMatrix para o diretório *bin* do aplicativo WCF.</span><span class="sxs-lookup"><span data-stu-id="d75bb-358">Copy the *Microsoft.Web.dll* and *Microsoft.Web.Administration.dll* assemblies from the WebMatrix installation location to the *bin* directory of the WCF application.</span></span> <span data-ttu-id="d75bb-359">Por padrão, o WebMatrix é instalado na subpasta *Microsoft WebMatrix* na pasta *arquivos de programas* do sistema.</span><span class="sxs-lookup"><span data-stu-id="d75bb-359">By default, WebMatrix is installed in the *Microsoft WebMatrix* subfolder under the system's *Program Files* folder.</span></span>
> 2. <span data-ttu-id="d75bb-360">No Microsoft Windows Vista ou superior, crie um symlink para os assemblies no diretório *bin* usando os comandos a seguir.</span><span class="sxs-lookup"><span data-stu-id="d75bb-360">On Microsoft Windows Vista or higher, create a symlink to the assemblies in the *bin* directory using the following commands.</span></span> <span data-ttu-id="d75bb-361">(Essa abordagem tem a vantagem de que ele não cria uma cópia dos assemblies.)</span><span class="sxs-lookup"><span data-stu-id="d75bb-361">(This approach has the advantage that it does not create a copy of the assemblies.)</span></span>
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. <span data-ttu-id="d75bb-362">Instale os dois assemblies no GAC.</span><span class="sxs-lookup"><span data-stu-id="d75bb-362">Install the two assemblies in the GAC.</span></span> <span data-ttu-id="d75bb-363">Em um prompt com privilégios elevados, execute os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="d75bb-363">From an elevated prompt, run the following commands:</span></span>
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]

#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a><span data-ttu-id="d75bb-364">Problema: o WebMatrix Beta 3 não pode executar determinadas tarefas que exigem elevação</span><span class="sxs-lookup"><span data-stu-id="d75bb-364">Issue: WebMatrix Beta 3 is unable to perform certain tasks that require elevation</span></span>

> <span data-ttu-id="d75bb-365">O WebMatrix Beta 3 não pode executar determinadas tarefas que exigem elevação, como a instalação de componentes adicionais nas seguintes situações:</span><span class="sxs-lookup"><span data-stu-id="d75bb-365">WebMatrix Beta 3 is unable to perform certain tasks that require elevation, such as installing additional components in the following situations:</span></span>
> 
> - <span data-ttu-id="d75bb-366">No Windows Vista ou no Windows 7, você está conectado com uma conta que não tem privilégios administrativos e o UAC (controle de conta de usuário) está desabilitado.</span><span class="sxs-lookup"><span data-stu-id="d75bb-366">On Windows Vista or Windows 7, you are logged in with an account that does not have administrative privileges and User Account Control (UAC) is disabled.</span></span>
> - <span data-ttu-id="d75bb-367">Você está usando o Microsoft Windows XP ou o Microsoft Windows Server 2003.</span><span class="sxs-lookup"><span data-stu-id="d75bb-367">You are using Microsoft Windows XP or Microsoft Windows Server 2003.</span></span>
> 
> <span data-ttu-id="d75bb-368">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-368">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-369">A maioria das tarefas no WebMatrix Beta 3 não requer permissão administrativa.</span><span class="sxs-lookup"><span data-stu-id="d75bb-369">Most tasks in WebMatrix Beta 3 do not require administrative permission.</span></span> <span data-ttu-id="d75bb-370">Para aqueles que fazem isso, você pode executar a operação como administrador ou seguir estas etapas:</span><span class="sxs-lookup"><span data-stu-id="d75bb-370">For those that do, you can perform the operation as an administrator, or follow these steps:</span></span>
> 
> - <span data-ttu-id="d75bb-371">No Windows Vista ou no Windows 7, habilite o UAC.</span><span class="sxs-lookup"><span data-stu-id="d75bb-371">On Windows Vista or Windows 7, enable UAC.</span></span>
> - <span data-ttu-id="d75bb-372">No Windows XP, adicione o usuário ao grupo de segurança Administradores.</span><span class="sxs-lookup"><span data-stu-id="d75bb-372">On Windows XP, add the user to the Administrators security group.</span></span>

#### <a name="issue-site-from-web-gallery-is-disabled"></a><span data-ttu-id="d75bb-373">Problema: "site da Galeria da Web" está desabilitado</span><span class="sxs-lookup"><span data-stu-id="d75bb-373">Issue: "Site from Web Gallery" is disabled</span></span>

> <span data-ttu-id="d75bb-374">A opção **site da Galeria da Web** será desabilitada se o Web Platform Installer 3,0 não estiver instalado.</span><span class="sxs-lookup"><span data-stu-id="d75bb-374">The **Site from Web Gallery** option is disabled if the Web Platform Installer 3.0 is not installed.</span></span>
> 
> <span data-ttu-id="d75bb-375">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-375">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-376">Instale o [Microsoft Web Platform Installer 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).</span><span class="sxs-lookup"><span data-stu-id="d75bb-376">Install the [Microsoft Web Platform Installer 3.0](https://go.microsoft.com/fwlink/?LinkID=194638).</span></span>

#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a><span data-ttu-id="d75bb-377">Problema: no Windows Server 2003, o IIS Express não é iniciado para um usuário não administrativo</span><span class="sxs-lookup"><span data-stu-id="d75bb-377">Issue: On Windows Server 2003, IIS Express does not start for a non-administrative user</span></span>

> <span data-ttu-id="d75bb-378">No Windows Server 2003, quando você inicia uma página ou inicia IIS Express, IIS Express não é iniciado.</span><span class="sxs-lookup"><span data-stu-id="d75bb-378">On Windows Server 2003, when you launch a page or start IIS Express, IIS Express does not start.</span></span> <span data-ttu-id="d75bb-379">Para páginas da Web, é exibido um erro que indica que o aplicativo foi iniciado por um usuário não administrativo.</span><span class="sxs-lookup"><span data-stu-id="d75bb-379">For Web pages, an error is displayed that indicates that the application has been started by a non-administrative user.</span></span>
> 
> <span data-ttu-id="d75bb-380">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-380">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-381">Inicie o WebMatrix Beta 3 como um usuário administrativo.</span><span class="sxs-lookup"><span data-stu-id="d75bb-381">Start WebMatrix Beta 3 as an administrative user.</span></span> <span data-ttu-id="d75bb-382">Para obter mais detalhes, consulte o seguinte artigo da base de conhecimento:</span><span class="sxs-lookup"><span data-stu-id="d75bb-382">For more details, see the following KnowledgeBase article:</span></span>  
>   
> [<span data-ttu-id="d75bb-383">Um aplicativo que é iniciado por um usuário não administrativo não pode escutar o tráfego HTTP do computador no qual o aplicativo está sendo executado no Windows Vista, no Windows Server 2003 ou no Windows XP.</span><span class="sxs-lookup"><span data-stu-id="d75bb-383">An application that is started by a non-administrative user cannot listen to the HTTP traffic of the computer on which the application is running in Windows Vista, Windows Server 2003, or Windows XP.</span></span>](https://support.microsoft.com/kb/939786)

#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a><span data-ttu-id="d75bb-384">Problema: o Google Chrome não está disponível como uma opção de execução</span><span class="sxs-lookup"><span data-stu-id="d75bb-384">Issue: Google Chrome is not available as a Run option</span></span>

> <span data-ttu-id="d75bb-385">O Google Chrome não é exibido na lista de navegadores em **executar** na guia **início** .</span><span class="sxs-lookup"><span data-stu-id="d75bb-385">Google Chrome is not displayed in the list of browsers under **Run** on the **Home** tab.</span></span>
> 
> <span data-ttu-id="d75bb-386">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-386">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-387">Algumas versões do Google Chrome não se registram corretamente com o recurso de programas padrão no Windows.</span><span class="sxs-lookup"><span data-stu-id="d75bb-387">Some versions of Google Chrome do not register themselves correctly with the Default Programs feature in Windows.</span></span> <span data-ttu-id="d75bb-388">Como alternativa, inicie o Google Chrome, clique no menu *Personalizar e controlar Google Chrome* , clique em *Opções*e, em seguida, clique em *tornar o Google Chrome meu navegador padrão*.</span><span class="sxs-lookup"><span data-stu-id="d75bb-388">As a workaround, start Google Chrome, click the *Customize and control Google Chrome* menu, click *Options*, and then click *Make Google Chrome my default browser*.</span></span>

#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a><span data-ttu-id="d75bb-389">Problema: a caixa de diálogo "chave estrangeira" não permite inserir uma chave primária</span><span class="sxs-lookup"><span data-stu-id="d75bb-389">Issue: The "Foreign Key" dialog box doesn't allow entering a primary key</span></span>

> <span data-ttu-id="d75bb-390">A caixa de diálogo **chave estrangeira** não permite que você insira o nome da chave primária da tabela de chave primária.</span><span class="sxs-lookup"><span data-stu-id="d75bb-390">The **Foreign Key** dialog box does not allow you to enter the primary key name from the primary key table.</span></span>
> 
> <span data-ttu-id="d75bb-391">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-391">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-392">Isso é intencional.</span><span class="sxs-lookup"><span data-stu-id="d75bb-392">This is intentional.</span></span> <span data-ttu-id="d75bb-393">Você não precisa inserir o nome da chave primária da tabela de chave primária.</span><span class="sxs-lookup"><span data-stu-id="d75bb-393">You do not need to enter the name of the primary key from the primary key table.</span></span>

#### <a name="issue-the-relationships-button-is-disabled"></a><span data-ttu-id="d75bb-394">Problema: o botão "relações" está desabilitado</span><span class="sxs-lookup"><span data-stu-id="d75bb-394">Issue: The "Relationships" button is disabled</span></span>

> <span data-ttu-id="d75bb-395">O botão **relações** na guia **tabela** do espaço de trabalho **bancos de dados** está desabilitado para bancos de dados SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="d75bb-395">The **Relationships** button under the **Table** tab in the **Databases** workspace is disabled for SQL Server Compact databases.</span></span>
> 
> <span data-ttu-id="d75bb-396">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-396">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-397">Nenhum.</span><span class="sxs-lookup"><span data-stu-id="d75bb-397">None.</span></span> <span data-ttu-id="d75bb-398">SQL Server Compact não oferece suporte a relações entre tabelas.</span><span class="sxs-lookup"><span data-stu-id="d75bb-398">SQL Server Compact does not support relationships between tables.</span></span>

#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a><span data-ttu-id="d75bb-399">Problema: as consultas SQL parametrizadas geram exceções</span><span class="sxs-lookup"><span data-stu-id="d75bb-399">Issue: Parameterized SQL queries throw exceptions</span></span>

> <span data-ttu-id="d75bb-400">No SQL Server Compact 4,0, se você não especificar um tipo de dados como `SqlDbType` ou `DbType` para parâmetros em consultas parametrizadas, uma exceção será lançada quando a consulta for executada.</span><span class="sxs-lookup"><span data-stu-id="d75bb-400">In SQL Server Compact 4.0, if you do not specify a data type such as `SqlDbType` or `DbType` for parameters in parameterized queries, an exception is thrown when the query runs.</span></span>
> 
> <span data-ttu-id="d75bb-401">**Solução alternativa**</span><span class="sxs-lookup"><span data-stu-id="d75bb-401">**Workaround**</span></span>  
> <span data-ttu-id="d75bb-402">Defina explicitamente o tipo de dados para parâmetros como `SqlDbType` ou `DbType`.</span><span class="sxs-lookup"><span data-stu-id="d75bb-402">Explicitly set the data type for parameters such as `SqlDbType` or `DbType`.</span></span> <span data-ttu-id="d75bb-403">Isso é crítico no caso de tipos de dados de BLOB (`image` e `ntext`).</span><span class="sxs-lookup"><span data-stu-id="d75bb-403">This is critical in the case of BLOB data types (`image` and `ntext`).</span></span> <span data-ttu-id="d75bb-404">Use um código semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="d75bb-404">Use code like the following:</span></span>
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]

<a id="More_Info"></a>

## <a name="for-more-information"></a><span data-ttu-id="d75bb-405">Para obter mais informações</span><span class="sxs-lookup"><span data-stu-id="d75bb-405">For More Information</span></span>

<span data-ttu-id="d75bb-406">Para obter mais informações sobre o WebMatrix Beta 3, consulte os seguintes sites:</span><span class="sxs-lookup"><span data-stu-id="d75bb-406">For more information about WebMatrix Beta 3, see the following websites:</span></span>

- [<span data-ttu-id="d75bb-407">IIS.net</span><span class="sxs-lookup"><span data-stu-id="d75bb-407">IIS.net</span></span>](http://iis.net/)
- [<span data-ttu-id="d75bb-408">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="d75bb-408">ASP.NET</span></span>](https://asp.net/webmatrix)
- [<span data-ttu-id="d75bb-409">Microsoft.com/web</span><span class="sxs-lookup"><span data-stu-id="d75bb-409">Microsoft.com/web</span></span>](https://www.microsoft.com/web)

---

<span data-ttu-id="d75bb-410">© 2010 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="d75bb-410">© 2010 Microsoft Corporation.</span></span> <span data-ttu-id="d75bb-411">Todos os direitos reservados.</span><span class="sxs-lookup"><span data-stu-id="d75bb-411">All Rights Reserved.</span></span> <span data-ttu-id="d75bb-412">[Termos de uso](https://msdn.microsoft.cos/cc300389.aspx).</span><span class="sxs-lookup"><span data-stu-id="d75bb-412">[Terms of Use](https://msdn.microsoft.cos/cc300389.aspx).</span></span>
