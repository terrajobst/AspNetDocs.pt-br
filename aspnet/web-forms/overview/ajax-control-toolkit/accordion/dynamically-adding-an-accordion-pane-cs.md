---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Adição dinâmica de um painel Accordion (c#) | Microsoft Docs
author: wenz
description: O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite que o usuário exibir um por vez. Painéis são normalmente declaradas w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 7134c95845ec7f22b5216e10b50ab8f81cd24806
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65131248"
---
# <a name="dynamically-adding-an-accordion-pane-c"></a><span data-ttu-id="d28dd-104">Adição dinâmica de um painel Accordion (c#)</span><span class="sxs-lookup"><span data-stu-id="d28dd-104">Dynamically Adding An Accordion Pane (C#)</span></span>

<span data-ttu-id="d28dd-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d28dd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d28dd-106">[Baixar o código](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d28dd-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span></span>

> <span data-ttu-id="d28dd-107">O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite que o usuário exibir um por vez.</span><span class="sxs-lookup"><span data-stu-id="d28dd-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="d28dd-108">Painéis geralmente são declaradas dentro da página em si, mas o código do lado do servidor pode ser usado para alcançar o mesmo resultado.</span><span class="sxs-lookup"><span data-stu-id="d28dd-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="overview"></a><span data-ttu-id="d28dd-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="d28dd-109">Overview</span></span>

<span data-ttu-id="d28dd-110">O controle Accordion no AJAX Control Toolkit fornece vários painéis e permite que o usuário exibir um por vez.</span><span class="sxs-lookup"><span data-stu-id="d28dd-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="d28dd-111">Painéis geralmente são declaradas dentro da página em si, mas o código do lado do servidor pode ser usado para alcançar o mesmo resultado.</span><span class="sxs-lookup"><span data-stu-id="d28dd-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="d28dd-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="d28dd-112">Steps</span></span>

<span data-ttu-id="d28dd-113">O controle Accordion expõe todas as propriedades importantes para o código do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="d28dd-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="d28dd-114">Entre outras coisas, o `Panes` propriedade concede acesso à coleção de painéis que compõem o Accordion.</span><span class="sxs-lookup"><span data-stu-id="d28dd-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="d28dd-115">Cada painel lá é do tipo `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="d28dd-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="d28dd-116">Portanto, é trivial criar tal um painel:</span><span class="sxs-lookup"><span data-stu-id="d28dd-116">It is therefore trivial to create such a pane:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

<span data-ttu-id="d28dd-117">O `HeaderContainer` propriedade de `AccordionPane` fornece acesso aos controles ASP.NET dentro da seção de cabeçalho do painel; a `ContentContainer` propriedade do `AccordionPane` faz o mesmo para a seção de conteúdo do painel.</span><span class="sxs-lookup"><span data-stu-id="d28dd-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="d28dd-118">Isso permite que o código ASP.NET para adicionar conteúdo para os painéis:</span><span class="sxs-lookup"><span data-stu-id="d28dd-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

<span data-ttu-id="d28dd-119">Por fim, os painéis devem ser adicionados para o `Panes` coleção de Acordeão:</span><span class="sxs-lookup"><span data-stu-id="d28dd-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

<span data-ttu-id="d28dd-120">Aqui está um código do lado do servidor completo que adiciona dois painéis a um controle Accordion:</span><span class="sxs-lookup"><span data-stu-id="d28dd-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

<span data-ttu-id="d28dd-121">O único elemento ausente é Accordion em si, que depende da presença do ASP.NET `ScriptManager` controle:</span><span class="sxs-lookup"><span data-stu-id="d28dd-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

<span data-ttu-id="d28dd-122">Para concluir o exemplo, as duas classes CSS referenciadas no controle Accordion fornecem informações de estilo para o navegador:</span><span class="sxs-lookup"><span data-stu-id="d28dd-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]

<span data-ttu-id="d28dd-123">[![Os dados em um acordeão dinamicamente foi adicionados pelo código do lado do servidor](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d28dd-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span></span>

<span data-ttu-id="d28dd-124">Os dados em um acordeão dinamicamente foi adicionados pelo código do lado do servidor ([clique para exibir a imagem em tamanho normal](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d28dd-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d28dd-125">[Anterior](databinding-to-an-accordion-cs.md)
> [Próximo](databinding-to-an-accordion-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d28dd-125">[Previous](databinding-to-an-accordion-cs.md)
[Next](databinding-to-an-accordion-vb.md)</span></span>
