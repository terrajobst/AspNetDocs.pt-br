---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Escolhendo uma animação fora de uma lista (C#) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A estrutura também permitirá...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 2bc203d694792716bbf4f7d7b8d152c589bf99b1
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575103"
---
# <a name="picking-one-animation-out-of-a-list-c"></a><span data-ttu-id="611d8-104">Escolher uma animação em uma lista (C#)</span><span class="sxs-lookup"><span data-stu-id="611d8-104">Picking One Animation Out Of a List (C#)</span></span>

<span data-ttu-id="611d8-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="611d8-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="611d8-106">[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="611d8-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span></span>

> <span data-ttu-id="611d8-107">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="611d8-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="611d8-108">A estrutura também permite que o programador escolha uma animação fora de uma lista de animações, dependendo da avaliação de algum código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="611d8-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="611d8-109">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="611d8-109">Overview</span></span>

<span data-ttu-id="611d8-110">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="611d8-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="611d8-111">A estrutura também permite que o programador escolha uma animação fora de uma lista de animações, dependendo da avaliação de algum código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="611d8-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="611d8-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="611d8-112">Steps</span></span>

<span data-ttu-id="611d8-113">Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:</span><span class="sxs-lookup"><span data-stu-id="611d8-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

<span data-ttu-id="611d8-114">A animação será aplicada a um painel de texto semelhante a este:</span><span class="sxs-lookup"><span data-stu-id="611d8-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

<span data-ttu-id="611d8-115">Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:</span><span class="sxs-lookup"><span data-stu-id="611d8-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

<span data-ttu-id="611d8-116">Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e o obrigatório `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="611d8-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

<span data-ttu-id="611d8-117">Dentro do nó `<Animations>`, use `<OnLoad>` para executar as animações depois que a página tiver sido totalmente carregada.</span><span class="sxs-lookup"><span data-stu-id="611d8-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="611d8-118">Em vez de uma das animações regulares, o elemento `<Case>` entra em cena.</span><span class="sxs-lookup"><span data-stu-id="611d8-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="611d8-119">O valor de seu atributo SelectScript é avaliado; o valor de retorno deve ser numérico.</span><span class="sxs-lookup"><span data-stu-id="611d8-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="611d8-120">Dependendo desse número, uma das subanimaçãos dentro de &lt;caso&gt; é executada.</span><span class="sxs-lookup"><span data-stu-id="611d8-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="611d8-121">Por exemplo, se SelectScript for avaliada como 2, o kit de ferramentas de controle executará a terceira animação dentro &lt;caso&gt; (a contagem começa em 0).</span><span class="sxs-lookup"><span data-stu-id="611d8-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="611d8-122">A marcação a seguir define três subanimaçãos: redimensionando a largura, redimensionando a altura e desbotando. O código JavaScript (`Math.floor(3 * Math.random())`), em seguida, escolhe um número entre 0 e 2, para que uma das três animações seja executada:</span><span class="sxs-lookup"><span data-stu-id="611d8-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]

<span data-ttu-id="611d8-123">[![uma das três animações possíveis: o painel fica mais largo](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="611d8-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span></span>

<span data-ttu-id="611d8-124">Uma das três animações possíveis: o painel fica mais largo ([clique para exibir a imagem em tamanho normal](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="611d8-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="611d8-125">[Anterior](animation-depending-on-a-condition-cs.md)
> [Próximo](animating-in-response-to-user-interaction-cs.md)</span><span class="sxs-lookup"><span data-stu-id="611d8-125">[Previous](animation-depending-on-a-condition-cs.md)
[Next](animating-in-response-to-user-interaction-cs.md)</span></span>
