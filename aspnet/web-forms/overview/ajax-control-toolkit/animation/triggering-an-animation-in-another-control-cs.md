---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Disparando uma animação em outro controleC#() | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Geralmente, iniciando um...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e0a1f8986047da04db6fde8e54b6fd0ac6ee2603
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599631"
---
# <a name="triggering-an-animation-in-another-control-c"></a><span data-ttu-id="7b377-104">Disparar uma animação em outro controle (C#)</span><span class="sxs-lookup"><span data-stu-id="7b377-104">Triggering an Animation in another Control (C#)</span></span>

<span data-ttu-id="7b377-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7b377-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7b377-106">[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7b377-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span></span>

> <span data-ttu-id="7b377-107">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="7b377-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7b377-108">Geralmente, iniciar uma animação é disparado pela interação do usuário com o mesmo controle.</span><span class="sxs-lookup"><span data-stu-id="7b377-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="7b377-109">No entanto, também é possível interagir com um controle e, em seguida, animações de outro controle.</span><span class="sxs-lookup"><span data-stu-id="7b377-109">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="overview"></a><span data-ttu-id="7b377-110">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="7b377-110">Overview</span></span>

<span data-ttu-id="7b377-111">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="7b377-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7b377-112">Geralmente, iniciar uma animação é disparado pela interação do usuário com o mesmo controle.</span><span class="sxs-lookup"><span data-stu-id="7b377-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="7b377-113">No entanto, também é possível interagir com um controle e, em seguida, animações de outro controle.</span><span class="sxs-lookup"><span data-stu-id="7b377-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="7b377-114">Etapas</span><span class="sxs-lookup"><span data-stu-id="7b377-114">Steps</span></span>

<span data-ttu-id="7b377-115">Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:</span><span class="sxs-lookup"><span data-stu-id="7b377-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

<span data-ttu-id="7b377-116">A animação será aplicada a um painel de texto semelhante a este:</span><span class="sxs-lookup"><span data-stu-id="7b377-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

<span data-ttu-id="7b377-117">Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:</span><span class="sxs-lookup"><span data-stu-id="7b377-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

<span data-ttu-id="7b377-118">Para começar a animar o painel, é usado um botão HTML.</span><span class="sxs-lookup"><span data-stu-id="7b377-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="7b377-119">Observe que `<input type="button" />` é Favoured sobre `<asp:Button />`, já que não queremos um postback quando o usuário clica nesse botão.</span><span class="sxs-lookup"><span data-stu-id="7b377-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

<span data-ttu-id="7b377-120">Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e a `runat="server"`obrigatório.</span><span class="sxs-lookup"><span data-stu-id="7b377-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="7b377-121">É importante definir `TargetControlID` como a ID do botão (o elemento que dispara a animação), não para a ID do painel (o elemento que está sendo animado)</span><span class="sxs-lookup"><span data-stu-id="7b377-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

<span data-ttu-id="7b377-122">No nó `<Animations>`, coloque as animações como de costume.</span><span class="sxs-lookup"><span data-stu-id="7b377-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="7b377-123">Para fazer com que eles alterem o painel, não o botão, defina o atributo `AnimationTarget` para cada elemento de animação dentro de `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="7b377-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="7b377-124">O valor de `AnimationTarget` é a ID do painel, é claro.</span><span class="sxs-lookup"><span data-stu-id="7b377-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="7b377-125">Dessa forma, as animações acontecem com o painel, não com o botão de disparo.</span><span class="sxs-lookup"><span data-stu-id="7b377-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="7b377-126">Aqui está a marcação `AnimationExtender` para este cenário:</span><span class="sxs-lookup"><span data-stu-id="7b377-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

<span data-ttu-id="7b377-127">Observe a ordem especial na qual as animações individuais aparecem.</span><span class="sxs-lookup"><span data-stu-id="7b377-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="7b377-128">Em primeiro lugar, o botão é desativado quando a animação é executada.</span><span class="sxs-lookup"><span data-stu-id="7b377-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="7b377-129">Como não há nenhum atributo `AnimationTarget` no elemento `<EnableAction>`, essa animação é aplicada ao controle de origem: o botão.</span><span class="sxs-lookup"><span data-stu-id="7b377-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="7b377-130">As próximas duas etapas de animação devem ser executadas em paralelo (`<Parallel>` elemento).</span><span class="sxs-lookup"><span data-stu-id="7b377-130">The next two animation steps shall be carried out in parallel (`<Parallel>` element).</span></span> <span data-ttu-id="7b377-131">Ambos têm seus atributos de `AnimationTarget` definidos como `"Panel1"`, animando o painel, não o botão.</span><span class="sxs-lookup"><span data-stu-id="7b377-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>

<span data-ttu-id="7b377-132">[![um clique do mouse no botão inicia a animação do painel](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7b377-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="7b377-133">Um clique do mouse no botão inicia a animação do painel ([clique para exibir a imagem em tamanho normal](triggering-an-animation-in-another-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7b377-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7b377-134">[Anterior](disabling-actions-during-animation-cs.md)
> [Próximo](modifying-animations-from-the-server-side-cs.md)</span><span class="sxs-lookup"><span data-stu-id="7b377-134">[Previous](disabling-actions-during-animation-cs.md)
[Next](modifying-animations-from-the-server-side-cs.md)</span></span>
