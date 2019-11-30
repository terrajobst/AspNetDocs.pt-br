---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Ajustando o índice Z de um DropShadow (C#) | Microsoft Docs
author: wenz
description: O controle DropShadow no AJAX Control Toolkit estende um painel com uma sombra. No entanto, essa sombra às vezes entra em conflito com outros controles, para insta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 12bc7f0430f1f30ff964cd9547ee1e9b0aa7423c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574290"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-c"></a><span data-ttu-id="d863b-104">Ajuste do índice Z de um DropShadow (C#)</span><span class="sxs-lookup"><span data-stu-id="d863b-104">Adjusting the Z-Index of a DropShadow (C#)</span></span>

<span data-ttu-id="d863b-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d863b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d863b-106">[Baixar código](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d863b-106">[Download Code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span></span>

> <span data-ttu-id="d863b-107">O controle DropShadow no AJAX Control Toolkit estende um painel com uma sombra.</span><span class="sxs-lookup"><span data-stu-id="d863b-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="d863b-108">No entanto, essa sombra às vezes entra em conflito com outros controles, por exemplo, o controle de menu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d863b-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="d863b-109">Quando uma entrada de menu é exibida, ela aparece atrás da sombra.</span><span class="sxs-lookup"><span data-stu-id="d863b-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="overview"></a><span data-ttu-id="d863b-110">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="d863b-110">Overview</span></span>

<span data-ttu-id="d863b-111">O controle DropShadow no AJAX Control Toolkit estende um painel com uma sombra.</span><span class="sxs-lookup"><span data-stu-id="d863b-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="d863b-112">No entanto, essa sombra às vezes entra em conflito com outros controles, por exemplo, o controle de menu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d863b-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="d863b-113">Quando uma entrada de menu é exibida, ela aparece atrás da sombra.</span><span class="sxs-lookup"><span data-stu-id="d863b-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="d863b-114">Etapas</span><span class="sxs-lookup"><span data-stu-id="d863b-114">Steps</span></span>

<span data-ttu-id="d863b-115">O código começa com o próprio painel, contendo texto suficiente para que o painel contenha texto suficiente para que o efeito fique visível:</span><span class="sxs-lookup"><span data-stu-id="d863b-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

<span data-ttu-id="d863b-116">Outro painel é colocado diretamente antes do painel de `panelShadow`.</span><span class="sxs-lookup"><span data-stu-id="d863b-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="d863b-117">Ele contém um menu com orientação horizontal para que as entradas de menu apareçam sobre (ou em vez disso: abaixo) do painel de `dropShadow`):</span><span class="sxs-lookup"><span data-stu-id="d863b-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

<span data-ttu-id="d863b-118">Em seguida, a `DropShadowExtender` é adicionada para estender o painel de `panelShadow` com um efeito de sombra.</span><span class="sxs-lookup"><span data-stu-id="d863b-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

<span data-ttu-id="d863b-119">Por fim, o controle de `ScriptManager` AJAX ASP.NET permite que o kit de ferramentas de controle funcione:</span><span class="sxs-lookup"><span data-stu-id="d863b-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

<span data-ttu-id="d863b-120">Quando você executa esse script, as entradas de menu aparecem embaixo do painel.</span><span class="sxs-lookup"><span data-stu-id="d863b-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="d863b-121">No entanto, o menu usa a classe CSS `panel` onde você só precisa definir duas coisas para fazer os elementos aparecerem na frente do outro painel:</span><span class="sxs-lookup"><span data-stu-id="d863b-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="d863b-122">Posicionamento relativo</span><span class="sxs-lookup"><span data-stu-id="d863b-122">Relative positioning</span></span>
- <span data-ttu-id="d863b-123">Um índice z positivo</span><span class="sxs-lookup"><span data-stu-id="d863b-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

<span data-ttu-id="d863b-124">Em seguida, o controle de `DropShadowExtender` não é mais um conflito com o controle de menu.</span><span class="sxs-lookup"><span data-stu-id="d863b-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>

<span data-ttu-id="d863b-125">[![antes: a entrada do menu não está visível](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d863b-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span></span>

<span data-ttu-id="d863b-126">Antes: a entrada do menu não está visível ([clique para exibir a imagem em tamanho normal](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d863b-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span></span>

<span data-ttu-id="d863b-127">[![após: a entrada de menu é exibida](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="d863b-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span></span>

<span data-ttu-id="d863b-128">Após: a entrada de menu é exibida ([clique para exibir a imagem em tamanho normal](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d863b-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d863b-129">Avançar</span><span class="sxs-lookup"><span data-stu-id="d863b-129">Next</span></span>](manipulating-dropshadow-properties-from-client-code-cs.md)
