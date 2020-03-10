---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animação, dependendo de uma condição (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Se uma animação é...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 583ebdbf109beb74b9a425020477183067bbb79a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614341"
---
# <a name="animation-depending-on-a-condition-vb"></a><span data-ttu-id="07f48-104">Animação dependendo de uma condição (VB)</span><span class="sxs-lookup"><span data-stu-id="07f48-104">Animation Depending On a Condition (VB)</span></span>

<span data-ttu-id="07f48-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="07f48-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="07f48-106">[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="07f48-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)</span></span>

> <span data-ttu-id="07f48-107">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="07f48-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="07f48-108">Se uma animação é executada ou não também pode depender de uma condição na forma de algum código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="07f48-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="07f48-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="07f48-109">Overview</span></span>

<span data-ttu-id="07f48-110">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="07f48-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="07f48-111">Se uma animação é executada ou não também pode depender de uma condição na forma de algum código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="07f48-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="07f48-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="07f48-112">Steps</span></span>

<span data-ttu-id="07f48-113">Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:</span><span class="sxs-lookup"><span data-stu-id="07f48-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

<span data-ttu-id="07f48-114">A animação será aplicada a um painel de texto semelhante a este:</span><span class="sxs-lookup"><span data-stu-id="07f48-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

<span data-ttu-id="07f48-115">Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:</span><span class="sxs-lookup"><span data-stu-id="07f48-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

<span data-ttu-id="07f48-116">Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e o obrigatório `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="07f48-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

<span data-ttu-id="07f48-117">Dentro do nó `<Animations>`, use `<OnLoad>` para executar as animações depois que a página tiver sido totalmente carregada.</span><span class="sxs-lookup"><span data-stu-id="07f48-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="07f48-118">Em vez de uma das animações regulares, o elemento `<Condition>` entra em cena.</span><span class="sxs-lookup"><span data-stu-id="07f48-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="07f48-119">O código JavaScript fornecido como o valor do atributo `ConditionScript` é executado em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="07f48-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="07f48-120">Se for avaliado como true, a animação será executada, caso contrário, não.</span><span class="sxs-lookup"><span data-stu-id="07f48-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="07f48-121">A marcação a seguir fornece duas animações, cada uma delas sendo executada em 50% dos casos após Random.</span><span class="sxs-lookup"><span data-stu-id="07f48-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="07f48-122">Como pode haver apenas uma animação dentro de `<OnLoad>`, as duas animações `<Condition>` são unidas usando o elemento `<Sequence>`:</span><span class="sxs-lookup"><span data-stu-id="07f48-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

<span data-ttu-id="07f48-123">Observe que o sinal de menor que (`<`) no atributo `ConditionScript` deve ser escape ().</span><span class="sxs-lookup"><span data-stu-id="07f48-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="07f48-124">Quando você executa esse script, nenhuma execução de animação é executada ou uma das duas faz, ou ambas, as duas.</span><span class="sxs-lookup"><span data-stu-id="07f48-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>

<span data-ttu-id="07f48-125">[![o painel está esmaecido sem redimensionamento, a segunda animação é executada, o primeiro não](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="07f48-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)</span></span>

<span data-ttu-id="07f48-126">O painel está esmaecido sem redimensionamento, portanto a segunda animação é executada, a primeira não foi ([clique para exibir a imagem em tamanho normal](animation-depending-on-a-condition-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="07f48-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="07f48-127">[Anterior](executing-several-animations-after-each-other-vb.md)
> [Próximo](picking-one-animation-out-of-a-list-vb.md)</span><span class="sxs-lookup"><span data-stu-id="07f48-127">[Previous](executing-several-animations-after-each-other-vb.md)
[Next](picking-one-animation-out-of-a-list-vb.md)</span></span>
