---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalando um auxiliar em um site Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este artigo descreve como instalar um auxiliar em um site Páginas da Web do ASP.NET (Razor). Um auxiliar é um componente reutilizável que inclui código e marcação para por...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 41e33c04a53a6ad257c3937cdadcec767e9217c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638582"
---
# <a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="6de7e-104">Instalando um auxiliar em um site Páginas da Web do ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="6de7e-104">Installing a Helper in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="6de7e-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6de7e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6de7e-106">Este artigo descreve como instalar um auxiliar em um site Páginas da Web do ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="6de7e-106">This article describes how to install a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="6de7e-107">Um *auxiliar* é um componente reutilizável que inclui código e marcação para executar uma tarefa que pode ser entediante ou complexa.</span><span class="sxs-lookup"><span data-stu-id="6de7e-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="6de7e-108">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="6de7e-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="6de7e-109">Como instalar um auxiliar em um site criado usando o WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="6de7e-109">How to install a helper in a website created using WebMatrix 3.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6de7e-110">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="6de7e-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6de7e-111">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="6de7e-111">WebMatrix 3</span></span>

## <a name="overview-of-helpers"></a><span data-ttu-id="6de7e-112">Visão geral dos auxiliares</span><span class="sxs-lookup"><span data-stu-id="6de7e-112">Overview of Helpers</span></span>

<span data-ttu-id="6de7e-113">Algumas tarefas que as pessoas geralmente desejam fazer em páginas da Web exigem muito código ou exigem conhecimento extra.</span><span class="sxs-lookup"><span data-stu-id="6de7e-113">Some tasks that people often want to do on web pages require a lot of code or require extra knowledge.</span></span> <span data-ttu-id="6de7e-114">Os exemplos incluem a exibição de um gráfico para dados; colocando um botão "seguir" do Twitter em uma página; enviando emails do seu site; corte ou redimensionamento de imagens; usando o PayPal para seu site.</span><span class="sxs-lookup"><span data-stu-id="6de7e-114">Examples include displaying a chart for data; putting a Twitter "Follow" button on a page; sending email from your website; cropping or resizing images; using PayPal for your site.</span></span> <span data-ttu-id="6de7e-115">Para facilitar esse tipo de coisas, Páginas da Web do ASP.NET permite que você use *auxiliares*.</span><span class="sxs-lookup"><span data-stu-id="6de7e-115">To make it easy to do these kinds of things, ASP.NET Web Pages lets you use *helpers*.</span></span> <span data-ttu-id="6de7e-116">Os auxiliares são componentes que você instala para um site e que permitem executar tarefas típicas usando apenas uma linha ou duas de código do Razor.</span><span class="sxs-lookup"><span data-stu-id="6de7e-116">Helpers are components that you install for a site and that let you perform typical tasks by using just a line or two of Razor code.</span></span>

<span data-ttu-id="6de7e-117">Páginas da Web do ASP.NET tem alguns auxiliares internos.</span><span class="sxs-lookup"><span data-stu-id="6de7e-117">ASP.NET Web Pages has a few helpers built in.</span></span> <span data-ttu-id="6de7e-118">No entanto, muitos auxiliares estão disponíveis em pacotes (suplementos) que são fornecidos usando o Gerenciador de pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="6de7e-118">However, many helpers are available in packages (add-ins) that are provided using the NuGet package manager.</span></span> <span data-ttu-id="6de7e-119">O NuGet permite selecionar um pacote a ser instalado e cuida de todos os detalhes da instalação.</span><span class="sxs-lookup"><span data-stu-id="6de7e-119">NuGet lets you select a package to install and then it takes care of all the details of the installation.</span></span>

## <a name="installing-a-helper-in-webmatrix-3"></a><span data-ttu-id="6de7e-120">Instalando um auxiliar no WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="6de7e-120">Installing a Helper in WebMatrix 3</span></span>

1. <span data-ttu-id="6de7e-121">No WebMatrix 3, clique no botão **NuGet** .</span><span class="sxs-lookup"><span data-stu-id="6de7e-121">In WebMatrix 3, click the **NuGet** button.</span></span>

    ![Caixa de diálogo Galeria do NuGet no WebMatrix](installing-helpers/_static/image1.png)
2. <span data-ttu-id="6de7e-123">Isso inicia o Gerenciador de pacotes NuGet e exibe os pacotes disponíveis.</span><span class="sxs-lookup"><span data-stu-id="6de7e-123">This launches the NuGet package manager and displays available packages.</span></span> <span data-ttu-id="6de7e-124">Na caixa de pesquisa, insira uma palavra-chave para o auxiliar que você deseja instalar.</span><span class="sxs-lookup"><span data-stu-id="6de7e-124">In the search box, enter a keyword for the helper you want to install.</span></span>

    ![Caixa de diálogo Galeria do NuGet no WebMatrix](installing-helpers/_static/image2.png)
3. <span data-ttu-id="6de7e-126">Selecione o pacote e clique em **instalar**.</span><span class="sxs-lookup"><span data-stu-id="6de7e-126">Select the package and then click **Install**.</span></span> <span data-ttu-id="6de7e-127">Clique em **Sim** quando for perguntado se você deseja instalar o pacote e indicar que aceita os termos.</span><span class="sxs-lookup"><span data-stu-id="6de7e-127">Click **Yes** when asked if you want to install the package and indicate that you accept the terms.</span></span>

     <span data-ttu-id="6de7e-128">Se esta for a primeira vez que você instalou um auxiliar, o NuGet criará pastas no seu site para o código que compõe o auxiliar.</span><span class="sxs-lookup"><span data-stu-id="6de7e-128">If this is the first time you've installed a helper, NuGet creates folders in your website for the code that makes up the helper.</span></span>
4. <span data-ttu-id="6de7e-129">Para desinstalar um auxiliar, clique no botão **Galeria** , clique na guia **instalado** e selecione o pacote que deseja desinstalar.</span><span class="sxs-lookup"><span data-stu-id="6de7e-129">To uninstall a helper, click the **Gallery** button, click the **Installed** tab, and pick the package you want to uninstall.</span></span>

## <a name="installing-the-twitter-helper"></a><span data-ttu-id="6de7e-130">Instalando o auxiliar do Twitter</span><span class="sxs-lookup"><span data-stu-id="6de7e-130">Installing the Twitter helper</span></span>

<span data-ttu-id="6de7e-131">A versão mais recente da API do Twitter não é compatível com o auxiliar do Twitter que você instala por meio do NuGet.</span><span class="sxs-lookup"><span data-stu-id="6de7e-131">The latest version of the Twitter API is not compatible with the Twitter helper you install through NuGet.</span></span> <span data-ttu-id="6de7e-132">Em vez disso, consulte o tópico [auxiliar do Twitter com WebMatrix](twitter-helper.md) para obter informações sobre como configurar o auxiliar do Twitter em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="6de7e-132">Instead, see the [Twitter Helper with WebMatrix](twitter-helper.md) topic for information about how to set up the Twitter helper in your project.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="6de7e-133">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="6de7e-133">Additional Resources</span></span>

[<span data-ttu-id="6de7e-134">Introdução ao Páginas da Web do ASP.NET 2-noções básicas de programação</span><span class="sxs-lookup"><span data-stu-id="6de7e-134">Introducing ASP.NET Web Pages 2 - Programming Basics</span></span>](../getting-started/introducing-razor-syntax-c.md)

[<span data-ttu-id="6de7e-135">Auxiliar do Twitter com WebMatrix</span><span class="sxs-lookup"><span data-stu-id="6de7e-135">Twitter Helper with WebMatrix</span></span>](twitter-helper.md)
