---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Adicionando dinamicamente um painel de acordeãoC#() | Microsoft Docs
author: wenz
description: O controle de acordeão no AJAX Control Toolkit fornece vários painéis e permite que o usuário exiba um deles de cada vez. Os painéis geralmente são declarados com w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 2834f56bd77c412923f4a8f382e670727f70eae4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614460"
---
# <a name="dynamically-adding-an-accordion-pane-c"></a><span data-ttu-id="b7da8-104">Adicionando dinamicamente um painel de acordeãoC#()</span><span class="sxs-lookup"><span data-stu-id="b7da8-104">Dynamically Adding An Accordion Pane (C#)</span></span>

<span data-ttu-id="b7da8-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b7da8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b7da8-106">[Baixar código](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b7da8-106">[Download Code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span></span>

> <span data-ttu-id="b7da8-107">O controle de acordeão no AJAX Control Toolkit fornece vários painéis e permite que o usuário exiba um deles de cada vez.</span><span class="sxs-lookup"><span data-stu-id="b7da8-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="b7da8-108">Os painéis geralmente são declarados dentro da própria página, mas o código do servidor pode ser usado para obter o mesmo resultado.</span><span class="sxs-lookup"><span data-stu-id="b7da8-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="overview"></a><span data-ttu-id="b7da8-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="b7da8-109">Overview</span></span>

<span data-ttu-id="b7da8-110">O controle de acordeão no AJAX Control Toolkit fornece vários painéis e permite que o usuário exiba um deles de cada vez.</span><span class="sxs-lookup"><span data-stu-id="b7da8-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="b7da8-111">Os painéis geralmente são declarados dentro da própria página, mas o código do servidor pode ser usado para obter o mesmo resultado.</span><span class="sxs-lookup"><span data-stu-id="b7da8-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="b7da8-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="b7da8-112">Steps</span></span>

<span data-ttu-id="b7da8-113">O controle de acordeão expõe todas as propriedades importantes para o código do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="b7da8-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="b7da8-114">Entre outras coisas, a propriedade `Panes` concede acesso à coleção de painéis que compõem o acordeão.</span><span class="sxs-lookup"><span data-stu-id="b7da8-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="b7da8-115">Cada painel é do tipo `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="b7da8-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="b7da8-116">Portanto, é trivial criar esse painel:</span><span class="sxs-lookup"><span data-stu-id="b7da8-116">It is therefore trivial to create such a pane:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

<span data-ttu-id="b7da8-117">A propriedade `HeaderContainer` de `AccordionPane` fornece acesso aos controles ASP.NET dentro da seção de cabeçalho do painel; a propriedade `ContentContainer` de `AccordionPane` faz o mesmo para a seção de conteúdo do painel.</span><span class="sxs-lookup"><span data-stu-id="b7da8-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="b7da8-118">Isso permite que o código ASP.NET adicione conteúdo aos painéis:</span><span class="sxs-lookup"><span data-stu-id="b7da8-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

<span data-ttu-id="b7da8-119">Por fim, os painéis devem ser adicionados à coleção de `Panes` do acordeão:</span><span class="sxs-lookup"><span data-stu-id="b7da8-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

<span data-ttu-id="b7da8-120">Aqui está um código completo do servidor que adiciona dois painéis a um controle de acordeão:</span><span class="sxs-lookup"><span data-stu-id="b7da8-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

<span data-ttu-id="b7da8-121">O único elemento ausente é o próprio acordeão, o que depende da presença do controle de `ScriptManager` ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="b7da8-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

<span data-ttu-id="b7da8-122">Para concluir o exemplo, as duas classes CSS referenciadas no controle acordeão fornecem informações de estilo para o navegador:</span><span class="sxs-lookup"><span data-stu-id="b7da8-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]

<span data-ttu-id="b7da8-123">[![os dados no acordeão foram adicionados dinamicamente pelo código do lado do servidor](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b7da8-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span></span>

<span data-ttu-id="b7da8-124">Os dados no acordeão foram adicionados dinamicamente pelo código do lado do servidor ([clique para exibir a imagem em tamanho normal](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b7da8-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b7da8-125">[Anterior](databinding-to-an-accordion-cs.md)
> [Próximo](databinding-to-an-accordion-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b7da8-125">[Previous](databinding-to-an-accordion-cs.md)
[Next](databinding-to-an-accordion-vb.md)</span></span>
