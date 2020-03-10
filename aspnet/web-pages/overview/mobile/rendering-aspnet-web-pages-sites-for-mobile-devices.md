---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Renderizando Páginas da Web do ASP.NET (Razor) sites para dispositivos móveis | Microsoft Docs
author: Rick-Anderson
description: 'Este artigo descreve como criar páginas em um site Páginas da Web do ASP.NET (Razor) que será renderizado adequadamente em dispositivos móveis. O que você aprenderá: como fazer isso...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: c012348d65e48a275cb0e4808fef2a7f31e5fb33
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563563"
---
# <a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a><span data-ttu-id="62139-104">Renderizando Páginas da Web do ASP.NET (Razor) sites para dispositivos móveis</span><span class="sxs-lookup"><span data-stu-id="62139-104">Rendering ASP.NET Web Pages (Razor) Sites for Mobile Devices</span></span>

<span data-ttu-id="62139-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="62139-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="62139-106">Este artigo descreve como criar páginas em um site Páginas da Web do ASP.NET (Razor) que será renderizado adequadamente em dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="62139-106">This article describes how to create pages in an ASP.NET Web Pages (Razor) site that will render appropriately on mobile devices.</span></span>
> 
> <span data-ttu-id="62139-107">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="62139-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="62139-108">Como usar uma Convenção de nomenclatura para especificar que uma página seja projetada especificamente para dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="62139-108">How to use a naming convention to specify that a page is designed specifically for mobile devices.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="62139-109">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="62139-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="62139-110">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="62139-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="62139-111">Este tutorial também funciona com o Páginas da Web do ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="62139-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="62139-112">Páginas da Web do ASP.NET permite que você crie exibições personalizadas para o processamento de conteúdo em dispositivos móveis ou outros.</span><span class="sxs-lookup"><span data-stu-id="62139-112">ASP.NET Web Pages lets you create custom displays for rendering content on mobile or other devices.</span></span>

<span data-ttu-id="62139-113">A maneira mais simples de criar uma página específica do dispositivo em um Páginas da Web do ASP.NET site é usando um padrão de nomeação de arquivo como este: *filename. Mobile. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="62139-113">The simplest way to create device-specific page in an ASP.NET Web Pages site is by using a file-naming pattern like this: *FileName.Mobile.cshtml*.</span></span> <span data-ttu-id="62139-114">Você pode criar duas versões de uma página (por exemplo, uma chamada *MyFile. cshtml* e outra chamada *MyFile. Mobile. cshtml*).</span><span class="sxs-lookup"><span data-stu-id="62139-114">You can create two versions of a page (for example, one named *MyFile.cshtml* and one named *MyFile.Mobile.cshtml*).</span></span> <span data-ttu-id="62139-115">Em tempo de execução, quando um dispositivo móvel solicita *MyFile. cshtml*, ASP.net renderiza o conteúdo de *MyFile. Mobile. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="62139-115">At run time, when a mobile device requests *MyFile.cshtml*, ASP.NET renders the content from *MyFile.Mobile.cshtml*.</span></span> <span data-ttu-id="62139-116">Caso contrário, *MyFile. cshtml* será renderizado.</span><span class="sxs-lookup"><span data-stu-id="62139-116">Otherwise, *MyFile.cshtml* is rendered.</span></span>

<span data-ttu-id="62139-117">O exemplo a seguir mostra como habilitar a renderização móvel adicionando uma página de conteúdo para dispositivos móveis.</span><span class="sxs-lookup"><span data-stu-id="62139-117">The following example shows how to enable mobile rendering by adding a content page for mobile devices.</span></span> <span data-ttu-id="62139-118">*Página1. cshtml* contém conteúdo mais uma barra lateral de navegação.</span><span class="sxs-lookup"><span data-stu-id="62139-118">*Page1.cshtml* contains content plus a navigation sidebar.</span></span> <span data-ttu-id="62139-119">*Página1. Mobile. cshtml* contém o mesmo conteúdo, mas omite a barra lateral.</span><span class="sxs-lookup"><span data-stu-id="62139-119">*Page1.Mobile.cshtml* contains the same content, but omits the sidebar.</span></span>

1. <span data-ttu-id="62139-120">Em um Páginas da Web do ASP.NET site, crie um arquivo chamado *página1. cshtml* e substitua o conteúdo atual pela marcação a seguir.</span><span class="sxs-lookup"><span data-stu-id="62139-120">In an ASP.NET Web Pages site, create a file named *Page1.cshtml* and replace the current content with following markup.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. <span data-ttu-id="62139-121">Crie um arquivo chamado *página1. Mobile. cshtml* e substitua o conteúdo existente pela marcação a seguir.</span><span class="sxs-lookup"><span data-stu-id="62139-121">Create a file named *Page1.Mobile.cshtml* and replace the existing content with the following markup.</span></span> <span data-ttu-id="62139-122">Observe que a versão móvel da página omite a seção de navegação para uma melhor renderização em uma tela menor.</span><span class="sxs-lookup"><span data-stu-id="62139-122">Notice that the mobile version of the page omits the navigation section for better rendering on a smaller screen.</span></span>

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. <span data-ttu-id="62139-123">Execute um navegador da área de trabalho e navegue até *página1. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="62139-123">Run a desktop browser and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="62139-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="62139-124">![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)</span></span>
4. <span data-ttu-id="62139-125">Execute um navegador móvel (ou um emulador de dispositivo móvel) e navegue até *página1. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="62139-125">Run a mobile browser (or a mobile device emulator) and browse to *Page1.cshtml*.</span></span> <span data-ttu-id="62139-126">(Observe que você não inclui o *. Mobile.*</span><span class="sxs-lookup"><span data-stu-id="62139-126">(Notice that you do not include *.mobile.*</span></span> <span data-ttu-id="62139-127">como parte da URL.) Embora a solicitação seja para *página1. cshtml*, ASP.net renderiza *página1. Mobile. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="62139-127">as part of the URL.) Even though the request is to *Page1.cshtml*, ASP.NET renders *Page1.Mobile.cshtml*.</span></span>

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="62139-129">Para testar as páginas móveis, você pode usar um simulador de dispositivo móvel que é executado em um computador desktop.</span><span class="sxs-lookup"><span data-stu-id="62139-129">To test mobile pages, you can use a mobile device simulator that runs on a desktop computer.</span></span> <span data-ttu-id="62139-130">Essa ferramenta permite que você teste páginas da Web da mesma forma que elas examinarão dispositivos móveis (ou seja, normalmente com uma área de exibição muito menor).</span><span class="sxs-lookup"><span data-stu-id="62139-130">This tool lets you test web pages as they would look on mobile devices (that is, typically with a much smaller display area).</span></span> <span data-ttu-id="62139-131">Um exemplo de simulador é o [complemento de seletor de agente do usuário](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) para Mozilla Firefox, que permite emular vários navegadores móveis de uma versão de área de trabalho do Firefox.</span><span class="sxs-lookup"><span data-stu-id="62139-131">One example of a simulator is the [User Agent Switcher add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) for Mozilla Firefox, which lets you emulate various mobile browsers from a desktop version of Firefox.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="62139-132">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="62139-132">Additional Resources</span></span>

<span data-ttu-id="62139-133">[Emulador de Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span><span class="sxs-lookup"><span data-stu-id="62139-133">[Windows Phone Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)</span></span>
