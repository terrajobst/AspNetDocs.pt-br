---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: Animar um controle UpdatePanel (c#) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 36d1166e1bd2b4c97b3cf3dd0bc3a7e8010a9443
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59393685"
---
# <a name="animating-an-updatepanel-control-c"></a><span data-ttu-id="76f41-104">Animar um controle UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="76f41-104">Animating an UpdatePanel Control (C#)</span></span>

<span data-ttu-id="76f41-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="76f41-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="76f41-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="76f41-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)</span></span>

> <span data-ttu-id="76f41-107">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="76f41-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="76f41-108">Para o conteúdo de um UpdatePanel, um extensor especial existe que depende muito do framework de animação: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="76f41-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="76f41-109">Este tutorial mostra como configurar uma animação desse tipo para um UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="76f41-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>


## <a name="overview"></a><span data-ttu-id="76f41-110">Visão geral</span><span class="sxs-lookup"><span data-stu-id="76f41-110">Overview</span></span>

<span data-ttu-id="76f41-111">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="76f41-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="76f41-112">Para o conteúdo de um `UpdatePanel`, um extensor especial existe que depende muito do framework de animação: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="76f41-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="76f41-113">Este tutorial mostra como configurar uma animação desse tipo para um `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="76f41-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="76f41-114">Etapas</span><span class="sxs-lookup"><span data-stu-id="76f41-114">Steps</span></span>

<span data-ttu-id="76f41-115">Como de costume, a primeira etapa é incluir o `ScriptManager` na página de modo que a biblioteca do AJAX ASP.NET é carregada e o Kit de ferramentas de controle pode ser usado:</span><span class="sxs-lookup"><span data-stu-id="76f41-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

<span data-ttu-id="76f41-116">A animação neste cenário será aplicada a um ASP.NET `Wizard` controle de web que reside em um `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="76f41-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="76f41-117">Três etapas (arbitrárias) fornecem opções suficiente para disparar postbacks:</span><span class="sxs-lookup"><span data-stu-id="76f41-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

<span data-ttu-id="76f41-118">A marcação necessária para o `UpdatePanelAnimationExtender` controle é bastante semelhante à marcação usada para o `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="76f41-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="76f41-119">No `TargetControlID` atributo fornecemos a `ID` da `UpdatePanel` animar; dentro a `UpdatePanelAnimationExtender` controle, o `<Animations>` elemento contém a marcação XML para as animações.</span><span class="sxs-lookup"><span data-stu-id="76f41-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="76f41-120">No entanto, há uma diferença: A quantidade de eventos e manipuladores de eventos é limitada em comparação com `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="76f41-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="76f41-121">Para `UpdatePanels`, apenas duas delas existir:</span><span class="sxs-lookup"><span data-stu-id="76f41-121">For `UpdatePanels`, only two of them exist:</span></span>

- `<OnUpdated>` <span data-ttu-id="76f41-122">Quando o UpdatePanel foi atualizado</span><span class="sxs-lookup"><span data-stu-id="76f41-122">when the UpdatePanel has been updated</span></span>
- `<OnUpdating>` <span data-ttu-id="76f41-123">Quando o UpdatePanel inicia a atualização</span><span class="sxs-lookup"><span data-stu-id="76f41-123">when the UpdatePanel starts updating</span></span>

<span data-ttu-id="76f41-124">Nesse cenário, o novo conteúdo do `UpdatePanel` (após o postback) deverá aplicar fade-in.</span><span class="sxs-lookup"><span data-stu-id="76f41-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="76f41-125">Isso é a marcação necessária para fazer isso:</span><span class="sxs-lookup"><span data-stu-id="76f41-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

<span data-ttu-id="76f41-126">Agora, sempre que um postback ocorre dentro do UpdatePanel, o novo conteúdo do painel aplicar fade-in sem problemas.</span><span class="sxs-lookup"><span data-stu-id="76f41-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>


[![T<span data-ttu-id="76f41-127">próxima etapa do assistente he está desaparecendo]</span><span class="sxs-lookup"><span data-stu-id="76f41-127">he next wizard step is fading in]</span></span>(animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)

<span data-ttu-id="76f41-128">A próxima etapa do assistente está desaparecendo ([clique para exibir a imagem em tamanho normal](animating-an-updatepanel-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="76f41-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="76f41-129">[Anterior](changing-an-animation-using-client-side-code-cs.md)
> [Próximo](dynamically-controlling-updatepanel-animations-cs.md)</span><span class="sxs-lookup"><span data-stu-id="76f41-129">[Previous](changing-an-animation-using-client-side-code-cs.md)
[Next](dynamically-controlling-updatepanel-animations-cs.md)</span></span>
