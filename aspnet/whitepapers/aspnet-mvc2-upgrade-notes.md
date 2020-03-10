---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Atualizando um aplicativo ASP.NET MVC 1,0 para o ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Este documento descreve como atualizar manualmente e com um assistente de um aplicativo ASP.NET MVC 1,0 para o ASP.NET MVC 2. Este documento também está disponível para d...
ms.author: riande
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 27589f1b1c9d5038118e5ff0cc2e7cecae17d5ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637014"
---
# <a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="ddb20-104">Atualização de um aplicativo do ASP.NET MVC 1.0 para o ASP.NET MVC 2</span><span class="sxs-lookup"><span data-stu-id="ddb20-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>

> <span data-ttu-id="ddb20-105">Este documento descreve como atualizar manualmente e com um assistente de um aplicativo ASP.NET MVC 1,0 para o ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="ddb20-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="ddb20-106">Este documento também está disponível para [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="ddb20-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>

## <a name="introduction"></a><span data-ttu-id="ddb20-107">Introdução</span><span class="sxs-lookup"><span data-stu-id="ddb20-107">Introduction</span></span>

<span data-ttu-id="ddb20-108">O ASP.NET MVC 2 pode ser instalado lado a lado com o ASP.NET MVC 1,0 no mesmo servidor.</span><span class="sxs-lookup"><span data-stu-id="ddb20-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="ddb20-109">Isso proporciona aos desenvolvedores de aplicativos flexibilidade na escolha de quando atualizar um aplicativo ASP.NET MVC 1,0 para o ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="ddb20-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="ddb20-110">O Visual Studio 2010 inclui um assistente que atualiza projetos existentes do ASP.NET MVC 1,0 criados com o Visual Studio 2008 para o ASP.NET MVC 2.</span><span class="sxs-lookup"><span data-stu-id="ddb20-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="ddb20-111">O assistente de atualização é iniciado abrindo um projeto ASP.NET MVC 1,0 no Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="ddb20-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="ddb20-112">Assistente de atualização para ASP.NET MVC 1,0 no Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="ddb20-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="ddb20-113">Para atualizar um aplicativo ASP.NET MVC 1,0 para o ASP.NET MVC 2 no Visual Studio 2008 SP1, use o aplicativo MvcAppConverter (sem suporte).</span><span class="sxs-lookup"><span data-stu-id="ddb20-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="ddb20-114">Você pode baixar este aplicativo da seguinte URL:</span><span class="sxs-lookup"><span data-stu-id="ddb20-114">You can download this application from the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="ddb20-115">Atualizando manualmente um projeto do ASP.NET MVC 1,0</span><span class="sxs-lookup"><span data-stu-id="ddb20-115">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="ddb20-116">Para atualizar manualmente um aplicativo existente do ASP.NET MVC 1,0 para a versão 2, siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="ddb20-116">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="ddb20-117">Faça um backup do projeto existente.</span><span class="sxs-lookup"><span data-stu-id="ddb20-117">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="ddb20-118">Em um editor de texto, abra o arquivo de projeto (o arquivo com a extensão de arquivo. csproj ou. vbproj) e localize o elemento ProjectTypeGuid.</span><span class="sxs-lookup"><span data-stu-id="ddb20-118">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="ddb20-119">Como o valor desse elemento, substitua o GUID {603c0e0b-db56-11dc-be95-000d561079b0} por {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span><span class="sxs-lookup"><span data-stu-id="ddb20-119">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="ddb20-120">Quando terminar, o valor desse elemento deverá ser o seguinte:</span><span class="sxs-lookup"><span data-stu-id="ddb20-120">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="ddb20-121">Na pasta raiz do aplicativo Web, edite o arquivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="ddb20-121">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="ddb20-122">Pesquise System. Web. Mvc, Version = 1.0.0.0 e substitua todas as instâncias por System. Web. Mvc, Version = 2.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="ddb20-122">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="ddb20-123">Repita a etapa anterior para o arquivo Web. config localizado na pasta exibições.</span><span class="sxs-lookup"><span data-stu-id="ddb20-123">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="ddb20-124">Abra o projeto usando o Visual Studio e, em **Gerenciador de soluções**, expanda o nó **referências** .</span><span class="sxs-lookup"><span data-stu-id="ddb20-124">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="ddb20-125">Exclua a referência a System. Web. Mvc (que aponta para o assembly da versão 1,0).</span><span class="sxs-lookup"><span data-stu-id="ddb20-125">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="ddb20-126">Adicione uma referência a System. Web. Mvc (v 2.0.0.0).</span><span class="sxs-lookup"><span data-stu-id="ddb20-126">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="ddb20-127">Adicione o seguinte elemento bindingRedirect ao arquivo Web. config na raiz do aplicativo na seção configuração:</span><span class="sxs-lookup"><span data-stu-id="ddb20-127">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="ddb20-128">Crie um novo aplicativo ASP.NET MVC 2 vazio.</span><span class="sxs-lookup"><span data-stu-id="ddb20-128">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="ddb20-129">Copie os arquivos da pasta scripts do novo aplicativo na pasta scripts do aplicativo existente.</span><span class="sxs-lookup"><span data-stu-id="ddb20-129">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="ddb20-130">Atualize o arquivo CSS existente do aplicativos €™ s com as definições de estilo CSS no arquivo site. css.</span><span class="sxs-lookup"><span data-stu-id="ddb20-130">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="ddb20-131">Compile o aplicativo e execute-o.</span><span class="sxs-lookup"><span data-stu-id="ddb20-131">Compile the application and run it.</span></span> <span data-ttu-id="ddb20-132">Se ocorrerem erros, consulte a seção alterações significativas da página [novidades na ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) .</span><span class="sxs-lookup"><span data-stu-id="ddb20-132">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>
