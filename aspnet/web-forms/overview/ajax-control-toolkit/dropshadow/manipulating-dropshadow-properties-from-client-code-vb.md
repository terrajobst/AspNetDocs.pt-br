---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Manipulando Propriedades DropShadow do código de cliente (VB) | Microsoft Docs
author: wenz
description: O controle DropShadow no AJAX Control Toolkit estende um painel com uma sombra. As propriedades desse extensor também podem ser alteradas usando constroem do cliente...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: a39adb9c06819f6f828add7d762effad430b8570
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613795"
---
# <a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="f2aa8-104">Manipular propriedades de DropShadow através de código de cliente (VB)</span><span class="sxs-lookup"><span data-stu-id="f2aa8-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>

<span data-ttu-id="f2aa8-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f2aa8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f2aa8-106">[Baixar código](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f2aa8-106">[Download Code](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="f2aa8-107">O controle DropShadow no AJAX Control Toolkit estende um painel com uma sombra.</span><span class="sxs-lookup"><span data-stu-id="f2aa8-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="f2aa8-108">As propriedades desse extensor também podem ser alteradas usando código JavaScript do cliente.</span><span class="sxs-lookup"><span data-stu-id="f2aa8-108">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="f2aa8-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="f2aa8-109">Overview</span></span>

<span data-ttu-id="f2aa8-110">O controle DropShadow no AJAX Control Toolkit estende um painel com uma sombra.</span><span class="sxs-lookup"><span data-stu-id="f2aa8-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="f2aa8-111">As propriedades desse extensor também podem ser alteradas usando código JavaScript do cliente.</span><span class="sxs-lookup"><span data-stu-id="f2aa8-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="f2aa8-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="f2aa8-112">Steps</span></span>

<span data-ttu-id="f2aa8-113">O código começa com um painel contendo algumas linhas de texto:</span><span class="sxs-lookup"><span data-stu-id="f2aa8-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="f2aa8-114">A classe CSS associada dá ao painel uma ótima cor de plano de fundo:</span><span class="sxs-lookup"><span data-stu-id="f2aa8-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="f2aa8-115">O `DropShadowExtender` é adicionado para estender o painel com um efeito de sombra drop, a opacidade é definida como 50%:</span><span class="sxs-lookup"><span data-stu-id="f2aa8-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="f2aa8-116">Em seguida, o controle de `ScriptManager` AJAX ASP.NET permite que o kit de ferramentas de controle funcione:</span><span class="sxs-lookup"><span data-stu-id="f2aa8-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="f2aa8-117">Outro painel contém dois links JavaScript para definir a opacidade da sombra: o link menos diminui a opacidade da sombra, e o link mais aumenta.</span><span class="sxs-lookup"><span data-stu-id="f2aa8-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="f2aa8-118">A função JavaScript `changeOpacity()` deve primeiro localizar o controle de `DropShadowExtender` na página.</span><span class="sxs-lookup"><span data-stu-id="f2aa8-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="f2aa8-119">O ASP.NET AJAX define o método `$find()` para exatamente essa tarefa.</span><span class="sxs-lookup"><span data-stu-id="f2aa8-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="f2aa8-120">Em seguida, o método `get_Opacity()` recupera a opacidade atual, o método `set_Opacity()` a define.</span><span class="sxs-lookup"><span data-stu-id="f2aa8-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="f2aa8-121">Em seguida, o código JavaScript coloca o valor de opacidade atual no elemento `<label>`:</span><span class="sxs-lookup"><span data-stu-id="f2aa8-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]

<span data-ttu-id="f2aa8-122">[![a opacidade é alterada no lado do cliente](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f2aa8-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="f2aa8-123">A opacidade é alterada no lado do cliente ([clique para exibir a imagem em tamanho normal](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f2aa8-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f2aa8-124">Anterior</span><span class="sxs-lookup"><span data-stu-id="f2aa8-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
