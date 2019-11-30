---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Adicionando dinamicamente um painel de acordeão (VB) | Microsoft Docs
author: wenz
description: O controle de acordeão no AJAX Control Toolkit fornece vários painéis e permite que o usuário exiba um deles de cada vez. Os painéis geralmente são declarados com w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: be48db5ea3de4af46b0f864cc9e73d2f518294a4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607196"
---
# <a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="f4ceb-104">Adicionando dinamicamente um painel de acordeão (VB)</span><span class="sxs-lookup"><span data-stu-id="f4ceb-104">Dynamically Adding An Accordion Pane (VB)</span></span>

<span data-ttu-id="f4ceb-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f4ceb-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f4ceb-106">[Baixar código](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f4ceb-106">[Download Code](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="f4ceb-107">O controle de acordeão no AJAX Control Toolkit fornece vários painéis e permite que o usuário exiba um deles de cada vez.</span><span class="sxs-lookup"><span data-stu-id="f4ceb-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="f4ceb-108">Os painéis geralmente são declarados dentro da própria página, mas o código do servidor pode ser usado para obter o mesmo resultado.</span><span class="sxs-lookup"><span data-stu-id="f4ceb-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="overview"></a><span data-ttu-id="f4ceb-109">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="f4ceb-109">Overview</span></span>

<span data-ttu-id="f4ceb-110">O controle de acordeão no AJAX Control Toolkit fornece vários painéis e permite que o usuário exiba um deles de cada vez.</span><span class="sxs-lookup"><span data-stu-id="f4ceb-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="f4ceb-111">Os painéis geralmente são declarados dentro da própria página, mas o código do servidor pode ser usado para obter o mesmo resultado.</span><span class="sxs-lookup"><span data-stu-id="f4ceb-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="f4ceb-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="f4ceb-112">Steps</span></span>

<span data-ttu-id="f4ceb-113">O controle de acordeão expõe todas as propriedades importantes para o código do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="f4ceb-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="f4ceb-114">Entre outras coisas, a propriedade `Panes` concede acesso à coleção de painéis que compõem o acordeão.</span><span class="sxs-lookup"><span data-stu-id="f4ceb-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="f4ceb-115">Cada painel é do tipo `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="f4ceb-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="f4ceb-116">Portanto, é trivial criar esse painel:</span><span class="sxs-lookup"><span data-stu-id="f4ceb-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="f4ceb-117">A propriedade `HeaderContainer` de `AccordionPane` fornece acesso aos controles ASP.NET dentro da seção de cabeçalho do painel; a propriedade `ContentContainer` de `AccordionPane` faz o mesmo para a seção de conteúdo do painel.</span><span class="sxs-lookup"><span data-stu-id="f4ceb-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="f4ceb-118">Isso permite que o código ASP.NET adicione conteúdo aos painéis:</span><span class="sxs-lookup"><span data-stu-id="f4ceb-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="f4ceb-119">Por fim, os painéis devem ser adicionados à coleção de `Panes` do acordeão:</span><span class="sxs-lookup"><span data-stu-id="f4ceb-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="f4ceb-120">Aqui está um código completo do servidor que adiciona dois painéis a um controle de acordeão:</span><span class="sxs-lookup"><span data-stu-id="f4ceb-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="f4ceb-121">O único elemento ausente é o próprio acordeão, o que depende da presença do controle de `ScriptManager` ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="f4ceb-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="f4ceb-122">Para concluir o exemplo, as duas classes CSS referenciadas no controle acordeão fornecem informações de estilo para o navegador:</span><span class="sxs-lookup"><span data-stu-id="f4ceb-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]

<span data-ttu-id="f4ceb-123">[![os dados no acordeão foram adicionados dinamicamente pelo código do lado do servidor](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f4ceb-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="f4ceb-124">Os dados no acordeão foram adicionados dinamicamente pelo código do lado do servidor ([clique para exibir a imagem em tamanho normal](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f4ceb-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f4ceb-125">Anterior</span><span class="sxs-lookup"><span data-stu-id="f4ceb-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
