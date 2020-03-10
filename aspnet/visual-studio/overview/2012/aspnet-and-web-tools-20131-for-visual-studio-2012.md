---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Notas de versão do ASP.NET and Web Tools 2013,1 para Visual Studio 2012 | Microsoft Docs
author: microsoft
description: Este documento descreve a versão do ASP.NET and Web Tools 2013,1 para Visual Studio 2012.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 260af1018064d60d80cbb1002001f28c4daffffd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578438"
---
# <a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="5b265-103">Notas de versão do ASP.NET and Web Tools 2013.1 para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="5b265-103">Release Notes for ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<span data-ttu-id="5b265-104">pela [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="5b265-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="5b265-105">Este documento descreve a versão do ASP.NET and Web Tools 2013,1 para Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="5b265-105">This document describes the release of ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

## <a name="contents"></a><span data-ttu-id="5b265-106">Conteúdo</span><span class="sxs-lookup"><span data-stu-id="5b265-106">Contents</span></span>

- [<span data-ttu-id="5b265-107">Notas de instalação</span><span class="sxs-lookup"><span data-stu-id="5b265-107">Installation Notes</span></span>](#install)
- [<span data-ttu-id="5b265-108">Requisitos de software</span><span class="sxs-lookup"><span data-stu-id="5b265-108">Software Requirements</span></span>](#requirements)
- <span data-ttu-id="5b265-109">Novos recursos no ASP.NET and Web Tools 2013,1 para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="5b265-109">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

    - [<span data-ttu-id="5b265-110">Bootstrap</span><span class="sxs-lookup"><span data-stu-id="5b265-110">Bootstrap</span></span>](#bootstrap)
    - [<span data-ttu-id="5b265-111">Modelos</span><span class="sxs-lookup"><span data-stu-id="5b265-111">Templates</span></span>](#templates)

        - [<span data-ttu-id="5b265-112">Modelo MVC 5 do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5b265-112">ASP.NET MVC 5 template</span></span>](#mvc5template)
        - [<span data-ttu-id="5b265-113">Modelo de ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="5b265-113">ASP.NET Web API 2 template</span></span>](#apitemplate)
        - [<span data-ttu-id="5b265-114">Modelos de item</span><span class="sxs-lookup"><span data-stu-id="5b265-114">Item Templates</span></span>](#itemtemplate)
    - [<span data-ttu-id="5b265-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="5b265-115">Entity Framework 6</span></span>](#ef6)
    - [<span data-ttu-id="5b265-116">ASP.NET scaffolding</span><span class="sxs-lookup"><span data-stu-id="5b265-116">ASP.NET Scaffolding</span></span>](#scaffold)
    - [<span data-ttu-id="5b265-117">Editor Razor</span><span class="sxs-lookup"><span data-stu-id="5b265-117">Razor Editor</span></span>](#razor)
    - [<span data-ttu-id="5b265-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="5b265-118">NuGet 2.7</span></span>](#nuget)
- <span data-ttu-id="5b265-119">Problemas conhecidos e alterações recentes</span><span class="sxs-lookup"><span data-stu-id="5b265-119">Known Issues and Breaking Changes</span></span>

    - [<span data-ttu-id="5b265-120">ASP.NET scaffolding</span><span class="sxs-lookup"><span data-stu-id="5b265-120">ASP.NET Scaffolding</span></span>](#issuescaffolding)

        - [<span data-ttu-id="5b265-121">MVC e API Web scaffolding-HTTP 404, erro não encontrado</span><span class="sxs-lookup"><span data-stu-id="5b265-121">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>](#404issue)
        - [<span data-ttu-id="5b265-122">Visual Studio Express 2012 para Web parar de funcionar depois de adicionar um item com Scaffold</span><span class="sxs-lookup"><span data-stu-id="5b265-122">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>](#expressissue)
    - [<span data-ttu-id="5b265-123">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="5b265-123">ASP.NET Razor 3</span></span>](#issuerazor)

        - [<span data-ttu-id="5b265-124">A exibição do arquivo cshtml com browse com ou F5 causa um erro de servidor</span><span class="sxs-lookup"><span data-stu-id="5b265-124">Viewing cshtml file with Browse With or F5 causes a server error</span></span>](#browseissue)
        - [<span data-ttu-id="5b265-125">Regravação de URL e til (~)</span><span class="sxs-lookup"><span data-stu-id="5b265-125">Url Rewrite and Tilde(~)</span></span>](#rewriteissue)
    - [<span data-ttu-id="5b265-126">Modelos</span><span class="sxs-lookup"><span data-stu-id="5b265-126">Templates</span></span>](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a><span data-ttu-id="5b265-127">Notas de instalação</span><span class="sxs-lookup"><span data-stu-id="5b265-127">Installation Notes</span></span>

<span data-ttu-id="5b265-128">[Instalar](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) o ASP.NET and Web Tools 2013,1 para Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="5b265-128">[Install](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="requirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="5b265-129">Requisitos de software</span><span class="sxs-lookup"><span data-stu-id="5b265-129">Software Requirements</span></span>

<span data-ttu-id="5b265-130">Você deve ter o Visual Studio 2012 ou o Visual Studio Express 2012 para Web.</span><span class="sxs-lookup"><span data-stu-id="5b265-130">You must have either Visual Studio 2012 or Visual Studio Express 2012 for Web.</span></span>

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a><span data-ttu-id="5b265-131">Novos recursos no ASP.NET and Web Tools 2013,1 para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="5b265-131">New Features in ASP.NET and Web Tools 2013.1 for Visual Studio 2012</span></span>

<a id="bootstrap"></a>
### <a name="bootstrap"></a><span data-ttu-id="5b265-132">Inicialização</span><span class="sxs-lookup"><span data-stu-id="5b265-132">Bootstrap</span></span>

<span data-ttu-id="5b265-133">Quando você Scaffold os controladores e exibições MVC 5, a marcação para as exibições usa a [inicialização](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="5b265-133">When you scaffold MVC 5 controllers and views, the markup for the views uses [Bootstrap](http://getbootstrap.com/).</span></span>

<a id="templates"></a>
### <a name="templates"></a><span data-ttu-id="5b265-134">Modelos</span><span class="sxs-lookup"><span data-stu-id="5b265-134">Templates</span></span>

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a><span data-ttu-id="5b265-135">Modelo MVC 5 do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5b265-135">ASP.NET MVC 5 template</span></span>

<span data-ttu-id="5b265-136">Adicionamos um novo modelo MVC 5.</span><span class="sxs-lookup"><span data-stu-id="5b265-136">We added a new MVC 5 template.</span></span> <span data-ttu-id="5b265-137">Ele faz referência aos últimos pacotes NuGet do MVC 5 e você pode usar scaffolding para adicionar controladores e exibições.</span><span class="sxs-lookup"><span data-stu-id="5b265-137">It references the latest MVC 5 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a><span data-ttu-id="5b265-138">Modelo de ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="5b265-138">ASP.NET Web API 2 template</span></span>

<span data-ttu-id="5b265-139">Adicionamos um novo modelo de API Web 2.</span><span class="sxs-lookup"><span data-stu-id="5b265-139">We added a new Web API 2 template.</span></span> <span data-ttu-id="5b265-140">Ele faz referência aos pacotes NuGet mais recentes da API Web 2, e você pode usar scaffolding para adicionar controladores e exibições.</span><span class="sxs-lookup"><span data-stu-id="5b265-140">It references the latest Web API 2 NuGet packages, and you can use scaffolding to add controllers and views.</span></span>

<a id="itemtemplate"></a>
#### <a name="item-templates"></a><span data-ttu-id="5b265-141">Modelos de item</span><span class="sxs-lookup"><span data-stu-id="5b265-141">Item Templates</span></span>

<span data-ttu-id="5b265-142">Adicionamos novos modelos de item para exibições MVC 5, páginas da Web (Razor 3) e controladores da API Web 2.</span><span class="sxs-lookup"><span data-stu-id="5b265-142">We added new item templates for MVC 5 views, Web Pages (Razor 3), and Web API 2 controllers.</span></span> <span data-ttu-id="5b265-143">Eles instalam os pacotes do NuGet relacionados ao projeto ao adicionar novos itens.</span><span class="sxs-lookup"><span data-stu-id="5b265-143">They install the related NuGet packages to the project while adding new items.</span></span>

<a id="ef6"></a>
### <a name="entity-framework-6"></a><span data-ttu-id="5b265-144">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="5b265-144">Entity Framework 6</span></span>

<span data-ttu-id="5b265-145">Quando você Scaffold um MVC ou um controlador de API Web usando Entity Framework, usamos o Framework 6.</span><span class="sxs-lookup"><span data-stu-id="5b265-145">When you scaffold an MVC or Web API controller using Entity Framework, we use Framework 6.</span></span> <span data-ttu-id="5b265-146">Para obter mais informações sobre Entity Framework, consulte o [histórico de versão do Entity Framework](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="5b265-146">For more information about Entity Framework, see the [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<span data-ttu-id="5b265-147">Você também pode baixar e instalar as ferramentas do Entity Framework 6 para Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="5b265-147">You can also download and install the Entity Framework 6 Tools for Visual Studio 2012.</span></span> <span data-ttu-id="5b265-148">Consulte [obter Entity Framework](https://msdn.com/data/ee712906#tooling).</span><span class="sxs-lookup"><span data-stu-id="5b265-148">See the [Get Entity Framework](https://msdn.com/data/ee712906#tooling).</span></span>

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="5b265-149">ASP.NET scaffolding</span><span class="sxs-lookup"><span data-stu-id="5b265-149">ASP.NET Scaffolding</span></span>

<span data-ttu-id="5b265-150">ASP.NET scaffolding é uma estrutura de geração de código para aplicativos ASP.NET Web.</span><span class="sxs-lookup"><span data-stu-id="5b265-150">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="5b265-151">Ele facilita a adição de código clichê ao seu projeto que interage com um modelo de dados.</span><span class="sxs-lookup"><span data-stu-id="5b265-151">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="5b265-152">Nas versões anteriores do Visual Studio, o scaffolding era limitado a projetos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5b265-152">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="5b265-153">Com essa atualização, agora você pode usar scaffolding para qualquer projeto ASP.NET, incluindo Web Forms.</span><span class="sxs-lookup"><span data-stu-id="5b265-153">With this update, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="5b265-154">Essa atualização não oferece suporte à geração de páginas para um projeto Web Forms, mas você ainda pode usar scaffolding com Web Forms adicionando dependências MVC ao projeto.</span><span class="sxs-lookup"><span data-stu-id="5b265-154">This update does not support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="5b265-155">O suporte para gerar páginas para Web Forms será adicionado em uma atualização futura.</span><span class="sxs-lookup"><span data-stu-id="5b265-155">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="5b265-156">Ao usar o scaffolding, garantimos que todas as dependências necessárias sejam instaladas no projeto.</span><span class="sxs-lookup"><span data-stu-id="5b265-156">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="5b265-157">Por exemplo, se você começar com um projeto ASP.NET Web Forms e, em seguida, usar scaffolding para adicionar um controlador de API Web, os pacotes e as referências do NuGet necessários serão adicionados automaticamente ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="5b265-157">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="5b265-158">Para adicionar o MVC scaffolding a um projeto Web Forms, adicione um **novo item com Scaffold** e selecione **dependências do MVC 5** na janela de diálogo.</span><span class="sxs-lookup"><span data-stu-id="5b265-158">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="5b265-159">Há duas opções para o scaffolding MVC; Mínimo e completo.</span><span class="sxs-lookup"><span data-stu-id="5b265-159">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="5b265-160">Se você selecionar mínimo, somente os pacotes e as referências do NuGet para ASP.NET MVC serão adicionados ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="5b265-160">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="5b265-161">Se você selecionar a opção completa, as dependências mínimas serão adicionadas, bem como os arquivos de conteúdo necessários para um projeto MVC.</span><span class="sxs-lookup"><span data-stu-id="5b265-161">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="5b265-162">O suporte para controladores assíncronos scaffolding usa os novos recursos assíncronos do Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="5b265-162">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="5b265-163">Para obter mais informações e tutoriais, consulte [visão geral do ASP.net scaffolding](../2013/aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5b265-163">For more information and tutorials, see [ASP.NET Scaffolding Overview](../2013/aspnet-scaffolding-overview.md).</span></span> <span data-ttu-id="5b265-164">Esses tutoriais mostram scaffolding com Visual Studio 2013, mas também são aplicáveis a ASP.NET and Web Tools 2013,1 para Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="5b265-164">These tutorials show scaffolding with Visual Studio 2013, but they are also applicable to ASP.NET and Web Tools 2013.1 for Visual Studio 2012.</span></span>

<a id="razor"></a>
### <a name="razor-editor"></a><span data-ttu-id="5b265-165">Editor Razor</span><span class="sxs-lookup"><span data-stu-id="5b265-165">Razor Editor</span></span>

<span data-ttu-id="5b265-166">Com essa atualização, o Visual Studio 2012 agora oferece suporte a ferramentas/edição do Razor 3.</span><span class="sxs-lookup"><span data-stu-id="5b265-166">With this update, Visual Studio 2012 now supports Razor 3 tooling/editing.</span></span>

<a id="nuget"></a>
### <a name="nuget-27"></a><span data-ttu-id="5b265-167">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="5b265-167">NuGet 2.7</span></span>

<span data-ttu-id="5b265-168">O NuGet 2,7 inclui um conjunto avançado de novos recursos que são descritos em detalhes nas [notas de versão do NuGet 2,7](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="5b265-168">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="5b265-169">Esta versão do NuGet elimina a necessidade de os usuários permitirem explicitamente que o NuGet restaure pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="5b265-169">This version of NuGet removes the need for users to explicitly allow NuGet to restore missing packages.</span></span> <span data-ttu-id="5b265-170">Ao instalar o NuGet 2,7, os usuários concordam implicitamente em restaurar automaticamente os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="5b265-170">When installing NuGet 2.7, users implicitly consent to automatically restoring missing packages.</span></span> <span data-ttu-id="5b265-171">Os usuários podem explicitamente recusar a restauração do pacote por meio das configurações do NuGet no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b265-171">Users can explicitly opt out of package restoration through the NuGet settings in Visual Studio.</span></span> <span data-ttu-id="5b265-172">Essa alteração simplifica a forma como a restauração de pacotes funciona.</span><span class="sxs-lookup"><span data-stu-id="5b265-172">This change simplifies how package restoration works.</span></span>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="5b265-173">Problemas conhecidos e alterações recentes</span><span class="sxs-lookup"><span data-stu-id="5b265-173">Known Issues and Breaking Changes</span></span>

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a><span data-ttu-id="5b265-174">ASP.NET scaffolding</span><span class="sxs-lookup"><span data-stu-id="5b265-174">ASP.NET Scaffolding</span></span>

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="5b265-175">MVC e API Web scaffolding-HTTP 404, erro não encontrado</span><span class="sxs-lookup"><span data-stu-id="5b265-175">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="5b265-176">Se você encontrar um erro ao adicionar um item com Scaffold a um projeto, é possível que seu projeto seja deixado em um estado inconsistente.</span><span class="sxs-lookup"><span data-stu-id="5b265-176">If you encounter an error when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="5b265-177">Algumas das alterações feitas no scaffolding serão revertidas, mas outras alterações, como os pacotes NuGet instalados, não serão revertidas.</span><span class="sxs-lookup"><span data-stu-id="5b265-177">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="5b265-178">Se as alterações de configuração de roteamento forem revertidas, os usuários receberão um erro HTTP 404 ao navegar para itens com Scaffold.</span><span class="sxs-lookup"><span data-stu-id="5b265-178">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="5b265-179">Para corrigir esse erro para o MVC, adicione um novo item com Scaffold e selecione dependências MVC 5 (mínima ou completa).</span><span class="sxs-lookup"><span data-stu-id="5b265-179">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="5b265-180">Esse processo adicionará todas as alterações necessárias ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="5b265-180">This process will add all of the required changes to your project.</span></span>

<span data-ttu-id="5b265-181">Para corrigir esse erro para a API Web:</span><span class="sxs-lookup"><span data-stu-id="5b265-181">To fix this error for Web API:</span></span>

1. <span data-ttu-id="5b265-182">Adicione a seguinte classe WebApiConfig ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="5b265-182">Add the following WebApiConfig class to your project.</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. <span data-ttu-id="5b265-183">Configure WebApiConfig. Register no aplicativo\_o método Start no global. asax da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="5b265-183">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a><span data-ttu-id="5b265-184">Visual Studio Express 2012 para Web parar de funcionar depois de adicionar um item com Scaffold</span><span class="sxs-lookup"><span data-stu-id="5b265-184">Visual Studio Express 2012 for Web stops working after adding a scaffolded item</span></span>

<span data-ttu-id="5b265-185">Se Visual Studio Express 2012 para a Web parar de funcionar depois de adicionar o item com Scaffold com Entity Framework (como o controlador Web API 2 com ações, usando Entity Framework), é possível que Visual Studio Express falha ao carregar a imagem nativa de um assembly dependente de System. Web. Extensions.</span><span class="sxs-lookup"><span data-stu-id="5b265-185">If Visual Studio Express 2012 for Web stops working after adding scaffolded item with Entity Framework (such as Web API 2 Controller with actions, using Entity Framework), it is possible that Visual Studio Express failed to load the native image of an assembly dependent on System.Web.Extensions.</span></span>

<span data-ttu-id="5b265-186">Para corrigir esse problema, configure Visual Studio Express para trabalhar com a imagem MSIL de System. Web. Extensions:</span><span class="sxs-lookup"><span data-stu-id="5b265-186">To correct this problem, configure Visual Studio Express to work with the MSIL image of System.Web.Extensions:</span></span>

1. <span data-ttu-id="5b265-187">Abra o prompt de comando no modo de administrador.</span><span class="sxs-lookup"><span data-stu-id="5b265-187">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="5b265-188">Vá para%ProgramFiles%\Microsoft Visual Studio 11.0 \ Common7\IDE ou% ProgramFiles (x86)% \ Microsoft Visual Studio 11.0 \ Common7\IDE (para o Windows de 64 bits).</span><span class="sxs-lookup"><span data-stu-id="5b265-188">Go to %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE or %ProgramFiles(x86)%\Microsoft Visual Studio 11.0\Common7\IDE (for 64 bit Windows).</span></span>
3. <span data-ttu-id="5b265-189">Abra VWDExpress. exe. config em um editor de texto.</span><span class="sxs-lookup"><span data-stu-id="5b265-189">Open VWDExpress.exe.config in a text editor.</span></span>
4. <span data-ttu-id="5b265-190">Adicione a seguinte linha sob a &lt;configuração&gt;/elemento&gt; de tempo de execução &lt;:</span><span class="sxs-lookup"><span data-stu-id="5b265-190">Add the following line under the &lt;configuration&gt;/&lt;runtime&gt; element:</span></span>  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. <span data-ttu-id="5b265-191">Reinicie o Visual Studio Express 2012 para Web.</span><span class="sxs-lookup"><span data-stu-id="5b265-191">Restart Visual Studio Express 2012 for Web.</span></span>

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a><span data-ttu-id="5b265-192">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="5b265-192">ASP.NET Razor 3</span></span>

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-with-browse-with-or-f5-causes-a-server-error"></a><span data-ttu-id="5b265-193">A exibição do arquivo cshtml com browse com ou F5 causa um erro de servidor</span><span class="sxs-lookup"><span data-stu-id="5b265-193">Viewing cshtml file with Browse With or F5 causes a server error</span></span>

<span data-ttu-id="5b265-194">Ao criar um projeto MVC 5 no Visual Studio 2012 (ou abrir no Visual Studio 2012 um projeto MVC 5 criado no Visual Studio 2013) e tentar exibir um arquivo cshtml usando procurar ou F5, você receberá um erro informando que o **erro do servidor no aplicativo '/'** é exibido.</span><span class="sxs-lookup"><span data-stu-id="5b265-194">When you create an MVC 5 project in Visual Studio 2012 (or open in Visual Studio 2012 an MVC 5 project that was created in Visual Studio 2013) and attempt to view a cshtml file by using Browse With or F5, you will receive an error that states - **Server Error in '/' Application**.</span></span> <span data-ttu-id="5b265-195">O servidor tenta navegar para `http://localhost:XXXX/Views/../XXXX.cshtml`</span><span class="sxs-lookup"><span data-stu-id="5b265-195">The server attempts to navigate to `http://localhost:XXXX/Views/../XXXX.cshtml`</span></span>

<span data-ttu-id="5b265-196">Para resolver esse problema, altere a configuração **ação de início** em seu projeto para **página específica**.</span><span class="sxs-lookup"><span data-stu-id="5b265-196">To resolve this issue, change the **Start Action** setting in your project to **Specific Page**.</span></span> <span data-ttu-id="5b265-197">Você não precisa fornecer um valor para a página.</span><span class="sxs-lookup"><span data-stu-id="5b265-197">You do not need to provide a value for the page.</span></span>

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

<span data-ttu-id="5b265-198">Depois de fazer essa alteração, selecionar F5 navega para a raiz do seu aplicativo (`http://localhost:XXXX`).</span><span class="sxs-lookup"><span data-stu-id="5b265-198">After making this change, selecting F5 navigates to the root of your application (`http://localhost:XXXX`).</span></span> <span data-ttu-id="5b265-199">Esse comportamento não é o mesmo que o comportamento para projetos MVC 5 no Visual Studio 2013, em que a configuração da **página atual** inicia a página aberta.</span><span class="sxs-lookup"><span data-stu-id="5b265-199">This behavior is not the same as the behavior for MVC 5 projects in Visual Studio 2013, where the **Current Page** setting launches the open page.</span></span>

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a><span data-ttu-id="5b265-200">Regravação de URL e til (~)</span><span class="sxs-lookup"><span data-stu-id="5b265-200">Url Rewrite and Tilde(~)</span></span>

<span data-ttu-id="5b265-201">Após a atualização para ASP.NET Razor 3 ou ASP.NET MVC 5, a notação til (~) poderá não funcionar corretamente se você estiver usando regravações de URL.</span><span class="sxs-lookup"><span data-stu-id="5b265-201">After upgrading to ASP.NET Razor 3 or ASP.NET MVC 5, the tilde(~) notation may no longer work correctly if you are using URL rewrites.</span></span> <span data-ttu-id="5b265-202">A reescrita de URL afeta a notação de til (~) em elementos HTML, como &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;e, como resultado, o til não é mais mapeado para o diretório raiz.</span><span class="sxs-lookup"><span data-stu-id="5b265-202">The URL rewrite affects the tilde(~) notation in HTML elements such as &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;, and as a result the tilde no longer maps to the root directory.</span></span>

<span data-ttu-id="5b265-203">Por exemplo, se você reescrever solicitações de **ASP.net/content** para **ASP.net**, o atributo href em &lt;a href = "~/content/"/&gt; será resolvido para **/content/content/** em vez de **/** .</span><span class="sxs-lookup"><span data-stu-id="5b265-203">For example, if you rewrite requests for **asp.net/content** to **asp.net**, the href attribute in &lt;A href="~/content/"/&gt; resolves to **/content/content/** instead of **/**.</span></span> <span data-ttu-id="5b265-204">Para suprimir essa alteração, você pode definir o contexto de **WasUrlRewritten do IIS\_** como false em cada página da Web ou no **aplicativo\_BeginRequest** em global. asax.</span><span class="sxs-lookup"><span data-stu-id="5b265-204">To suppress this change, you can set the **IIS\_WasUrlRewritten** context to false in each Web Page or in **Application\_BeginRequest** in Global.asax.</span></span>

<a id="templateissue"></a>
### <a name="templates"></a><span data-ttu-id="5b265-205">Modelos</span><span class="sxs-lookup"><span data-stu-id="5b265-205">Templates</span></span>

<span data-ttu-id="5b265-206">Quando você cria projetos do ASP.NET MVC com o Visual Studio 2012 no Windows 8.1 ou no Windows Server 2012 R2, o Visual Studio exibe uma mensagem de erro informando "Configurando Web [url] para ASP.NET 4,5 falhou".</span><span class="sxs-lookup"><span data-stu-id="5b265-206">When you create ASP.NET MVC projects with Visual Studio 2012 on Windows 8.1 or Windows Server 2012 R2, Visual Studio displays an error message that states "Configuring Web [url] for ASP.NET 4.5 failed."</span></span>

![erro de configuração](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

<span data-ttu-id="5b265-208">Você verá esse erro porque o Visual Studio 2012 não habilita o recurso ASP.NET 4,5 quando ele está instalado nessas versões do Windows.</span><span class="sxs-lookup"><span data-stu-id="5b265-208">You see this error because Visual Studio 2012 does not enable the ASP.NET 4.5 feature when it is installed on those releases of Windows.</span></span> <span data-ttu-id="5b265-209">Para habilitar o ASP.NET 4,5, execute as etapas descritas em [Ativar ou desativar recursos do Windows](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span><span class="sxs-lookup"><span data-stu-id="5b265-209">To enable ASP.NET 4.5, perform the steps described in [Turn Windows features on or off](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).</span></span>

![ativar ou desativar os recursos do Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

<span data-ttu-id="5b265-211">Como alternativa, você pode habilitar o ASP.NET 4,5 por meio da linha de comando.</span><span class="sxs-lookup"><span data-stu-id="5b265-211">Alternatively, you can enable ASP.NET 4.5 through the command line.</span></span>

1. <span data-ttu-id="5b265-212">Abra o prompt de comando no modo de administrador.</span><span class="sxs-lookup"><span data-stu-id="5b265-212">Open Command Prompt in the Administrator mode.</span></span>
2. <span data-ttu-id="5b265-213">Execute o comando a seguir para habilitar o ASP.NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="5b265-213">Run the following command to enable ASP.NET 4.5.</span></span>  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
