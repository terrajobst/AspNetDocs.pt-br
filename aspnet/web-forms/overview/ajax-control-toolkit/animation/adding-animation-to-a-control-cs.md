---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Adicionando animação a um controle (c#) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Este tutorial mostra como...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: aac6e97ae5d3d777c3644515628d2669076a88c4
ms.sourcegitcommit: 289e051cc8a90e8f7127e239fda73047bde4de12
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58421915"
---
<a name="adding-animation-to-a-control-c"></a><span data-ttu-id="09301-104">Adição de animação a um controle (C#)</span><span class="sxs-lookup"><span data-stu-id="09301-104">Adding Animation to a Control (C#)</span></span>
====================
<span data-ttu-id="09301-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="09301-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="09301-106">[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="09301-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span></span>

> <span data-ttu-id="09301-107">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="09301-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="09301-108">Este tutorial mostra como configurar uma animação desse tipo.</span><span class="sxs-lookup"><span data-stu-id="09301-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="09301-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="09301-109">Overview</span></span>

<span data-ttu-id="09301-110">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="09301-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="09301-111">Este tutorial mostra como configurar uma animação desse tipo.</span><span class="sxs-lookup"><span data-stu-id="09301-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="09301-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="09301-112">Steps</span></span>

<span data-ttu-id="09301-113">Como de costume, a primeira etapa é incluir o `ScriptManager` na página de modo que a biblioteca do AJAX ASP.NET é carregada e o Kit de ferramentas de controle pode ser usado:</span><span class="sxs-lookup"><span data-stu-id="09301-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="09301-114">A animação neste cenário será aplicada a um painel de texto que tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="09301-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="09301-115">A classe CSS associada para o painel define uma cor de fundo e uma largura:</span><span class="sxs-lookup"><span data-stu-id="09301-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

<span data-ttu-id="09301-116">Em seguida, é necessário o `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="09301-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="09301-117">Depois de fornecer um `ID` e o usual `runat="server"`, o `TargetControlID` atributo deve ser definido para o controle para animar em nosso caso, o painel:</span><span class="sxs-lookup"><span data-stu-id="09301-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="09301-118">A animação completa é aplicada declarativamente, usando uma sintaxe XML, infelizmente atualmente não têm suportada completo pelo IntelliSense do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="09301-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="09301-119">O nó raiz é `<Animations>;` dentro desse nó, vários eventos são permitidos que determinam quando as animações take(s) local:</span><span class="sxs-lookup"><span data-stu-id="09301-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="09301-120">`OnClick` (clique do mouse)</span><span class="sxs-lookup"><span data-stu-id="09301-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="09301-121">`OnHoverOut` (quando o mouse deixa um controle)</span><span class="sxs-lookup"><span data-stu-id="09301-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="09301-122">`OnHoverOver` (quando o mouse passa sobre um controle, interrompendo o `OnHoverOut` animação)</span><span class="sxs-lookup"><span data-stu-id="09301-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="09301-123">`OnLoad` (quando a página foi carregada)</span><span class="sxs-lookup"><span data-stu-id="09301-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="09301-124">`OnMouseOut` (quando o mouse deixa um controle)</span><span class="sxs-lookup"><span data-stu-id="09301-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="09301-125">`OnMouseOver` (quando o mouse passa sobre um controle, não parar a `OnMouseOut` animação)</span><span class="sxs-lookup"><span data-stu-id="09301-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="09301-126">A estrutura vem com um conjunto de animações, cada um representado por seu próprio elemento XML.</span><span class="sxs-lookup"><span data-stu-id="09301-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="09301-127">Aqui está uma seleção:</span><span class="sxs-lookup"><span data-stu-id="09301-127">Here is a selection:</span></span>

- <span data-ttu-id="09301-128">`<Color>` (uma cor de alteração)</span><span class="sxs-lookup"><span data-stu-id="09301-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="09301-129">`<FadeIn>` (fade in)</span><span class="sxs-lookup"><span data-stu-id="09301-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="09301-130">`<FadeOut>` (esmaecimento)</span><span class="sxs-lookup"><span data-stu-id="09301-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="09301-131">`<Property>` (propriedade de um controle de alteração)</span><span class="sxs-lookup"><span data-stu-id="09301-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="09301-132">`<Pulse>` (pulsating)</span><span class="sxs-lookup"><span data-stu-id="09301-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="09301-133">`<Resize>` (o tamanho de alteração)</span><span class="sxs-lookup"><span data-stu-id="09301-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="09301-134">`<Scale>` (proporcionalmente o tamanho de alteração)</span><span class="sxs-lookup"><span data-stu-id="09301-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="09301-135">Neste exemplo, o painel deve desaparecer. A animação deverão utilizar 1,5 segundos (`Duration` atributo), exibindo a 24 quadros (etapas de animação) por segundo (`Fps` atributo).</span><span class="sxs-lookup"><span data-stu-id="09301-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attribute).</span></span> <span data-ttu-id="09301-136">Aqui está a marcação completa para o `AnimationExtender` controle:</span><span class="sxs-lookup"><span data-stu-id="09301-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="09301-137">Quando você executa esse script, o painel é exibido e fade out em um e meio segundos.</span><span class="sxs-lookup"><span data-stu-id="09301-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="09301-138">[![O painel está desaparecendo](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="09301-138">[![The panel is fading out](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="09301-139">O painel está desaparecendo ([clique para exibir a imagem em tamanho normal](adding-animation-to-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="09301-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="09301-140">Avançar</span><span class="sxs-lookup"><span data-stu-id="09301-140">Next</span></span>](executing-several-animations-at-the-same-time-cs.md)
