---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipulando Propriedades DropShadow do código do clienteC#() | Microsoft Docs
author: wenz
description: Personalizando a interface de edição do DataList
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 790f0d881e43518600968d6c175d4eaa53d0e5f9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574084"
---
# <a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="a1f27-103">Manipular propriedades de DropShadow através de código de cliente (C#)</span><span class="sxs-lookup"><span data-stu-id="a1f27-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>

<span data-ttu-id="a1f27-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a1f27-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a1f27-105">[Baixar código](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a1f27-105">[Download Code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="a1f27-106">O controle DropShadow no AJAX Control Toolkit estende um painel com uma sombra.</span><span class="sxs-lookup"><span data-stu-id="a1f27-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="a1f27-107">As propriedades desse extensor também podem ser alteradas usando código JavaScript do cliente.</span><span class="sxs-lookup"><span data-stu-id="a1f27-107">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="a1f27-108">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="a1f27-108">Overview</span></span>

<span data-ttu-id="a1f27-109">O controle DropShadow no AJAX Control Toolkit estende um painel com uma sombra.</span><span class="sxs-lookup"><span data-stu-id="a1f27-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="a1f27-110">As propriedades desse extensor também podem ser alteradas usando código JavaScript do cliente.</span><span class="sxs-lookup"><span data-stu-id="a1f27-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="a1f27-111">Etapas</span><span class="sxs-lookup"><span data-stu-id="a1f27-111">Steps</span></span>

<span data-ttu-id="a1f27-112">O código começa com um painel contendo algumas linhas de texto:</span><span class="sxs-lookup"><span data-stu-id="a1f27-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="a1f27-113">A classe CSS associada dá ao painel uma ótima cor de plano de fundo:</span><span class="sxs-lookup"><span data-stu-id="a1f27-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="a1f27-114">O `DropShadowExtender` é adicionado para estender o painel com um efeito de sombra drop, a opacidade é definida como 50%:</span><span class="sxs-lookup"><span data-stu-id="a1f27-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="a1f27-115">Em seguida, o controle de `ScriptManager` AJAX ASP.NET permite que o kit de ferramentas de controle funcione:</span><span class="sxs-lookup"><span data-stu-id="a1f27-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="a1f27-116">Outro painel contém dois links JavaScript para definir a opacidade da sombra: o link menos diminui a opacidade da sombra, e o link mais aumenta.</span><span class="sxs-lookup"><span data-stu-id="a1f27-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="a1f27-117">A função JavaScript `changeOpacity()` deve primeiro localizar o controle de `DropShadowExtender` na página.</span><span class="sxs-lookup"><span data-stu-id="a1f27-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="a1f27-118">O ASP.NET AJAX define o método `$find()` para exatamente essa tarefa.</span><span class="sxs-lookup"><span data-stu-id="a1f27-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="a1f27-119">Em seguida, o método `get_Opacity()` recupera a opacidade atual, o método `set_Opacity()` a define.</span><span class="sxs-lookup"><span data-stu-id="a1f27-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="a1f27-120">Em seguida, o código JavaScript coloca o valor de opacidade atual no elemento `<label>`:</span><span class="sxs-lookup"><span data-stu-id="a1f27-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]

<span data-ttu-id="a1f27-121">[![a opacidade é alterada no lado do cliente](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a1f27-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="a1f27-122">A opacidade é alterada no lado do cliente ([clique para exibir a imagem em tamanho normal](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a1f27-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a1f27-123">[Anterior](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Próximo](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a1f27-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
