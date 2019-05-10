---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Ajuste do índice Z de um DropShadow (VB) | Microsoft Docs
author: wenz
description: O controle de DropShadow no AJAX Control Toolkit, um painel com uma sombra. No entanto esse sombra às vezes, está em conflito com outros controles, para insta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: f56087b1e94653d2a6a06f915191db6ec5e358a2
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65116958"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="2c9bc-104">Ajuste do índice Z de um DropShadow (VB)</span><span class="sxs-lookup"><span data-stu-id="2c9bc-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>

<span data-ttu-id="2c9bc-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2c9bc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2c9bc-106">[Baixar o código](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2c9bc-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="2c9bc-107">O controle de DropShadow no AJAX Control Toolkit, um painel com uma sombra.</span><span class="sxs-lookup"><span data-stu-id="2c9bc-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="2c9bc-108">No entanto esse sombra às vezes, está em conflito com outros controles, por exemplo, o controle de Menu do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2c9bc-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="2c9bc-109">Quando uma entrada de menu é exibido, ele aparece por trás da sombra.</span><span class="sxs-lookup"><span data-stu-id="2c9bc-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="overview"></a><span data-ttu-id="2c9bc-110">Visão geral</span><span class="sxs-lookup"><span data-stu-id="2c9bc-110">Overview</span></span>

<span data-ttu-id="2c9bc-111">O controle de DropShadow no AJAX Control Toolkit, um painel com uma sombra.</span><span class="sxs-lookup"><span data-stu-id="2c9bc-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="2c9bc-112">No entanto esse sombra às vezes, está em conflito com outros controles, por exemplo, o controle de Menu do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2c9bc-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="2c9bc-113">Quando uma entrada de menu é exibido, ele aparece por trás da sombra.</span><span class="sxs-lookup"><span data-stu-id="2c9bc-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="2c9bc-114">Etapas</span><span class="sxs-lookup"><span data-stu-id="2c9bc-114">Steps</span></span>

<span data-ttu-id="2c9bc-115">O código começa com o próprio painel, que contém o texto suficiente para que o painel contém texto suficiente para o efeito para ser visível:</span><span class="sxs-lookup"><span data-stu-id="2c9bc-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="2c9bc-116">Outro painel é colocado diretamente antes do `panelShadow` painel.</span><span class="sxs-lookup"><span data-stu-id="2c9bc-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="2c9bc-117">Ele contém um menu com a orientação horizontal para que as entradas de menu aparecem ao longo (ou melhor: sob) o `dropShadow` painel):</span><span class="sxs-lookup"><span data-stu-id="2c9bc-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="2c9bc-118">Em seguida, o `DropShadowExtender` é adicionado ao estender o `panelShadow` painel com um efeito de sombra:</span><span class="sxs-lookup"><span data-stu-id="2c9bc-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="2c9bc-119">Por fim, o ASP.NET AJAX `ScriptManager` controle permite que o Kit de ferramentas trabalhar:</span><span class="sxs-lookup"><span data-stu-id="2c9bc-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="2c9bc-120">Quando você executa esse script, as entradas de menu aparecem sob o painel.</span><span class="sxs-lookup"><span data-stu-id="2c9bc-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="2c9bc-121">No entanto, o menu usa a classe CSS `panel` em que você só precisa definir duas coisas para fazer com que elementos são exibidos na frente de outro painel:</span><span class="sxs-lookup"><span data-stu-id="2c9bc-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="2c9bc-122">Posicionamento relativo</span><span class="sxs-lookup"><span data-stu-id="2c9bc-122">Relative positioning</span></span>
- <span data-ttu-id="2c9bc-123">Um índice z positivo</span><span class="sxs-lookup"><span data-stu-id="2c9bc-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="2c9bc-124">Em seguida, o `DropShadowExtender` controle não está em conflito mais com o controle de Menu.</span><span class="sxs-lookup"><span data-stu-id="2c9bc-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>

<span data-ttu-id="2c9bc-125">[![Antes: A entrada de menu não é visível](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2c9bc-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="2c9bc-126">Antes: A entrada de menu não é visível ([clique para exibir a imagem em tamanho normal](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2c9bc-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>

<span data-ttu-id="2c9bc-127">[![Depois de: A entrada de menu é exibido](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="2c9bc-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="2c9bc-128">Depois: A entrada de menu é exibido ([clique para exibir a imagem em tamanho normal](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="2c9bc-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2c9bc-129">[Anterior](manipulating-dropshadow-properties-from-client-code-cs.md)
> [Próximo](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2c9bc-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
