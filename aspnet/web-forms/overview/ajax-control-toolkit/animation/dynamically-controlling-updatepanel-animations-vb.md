---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Controlando dinamicamente animações UpdatePanel (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 97a52bd75fdf63ad62282afd9df772f0a9e4f931
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536151"
---
# <a name="dynamically-controlling-updatepanel-animations-vb"></a><span data-ttu-id="c942d-104">Controlar dinamicamente animações UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="c942d-104">Dynamically Controlling UpdatePanel Animations (VB)</span></span>

<span data-ttu-id="c942d-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c942d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c942d-106">[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c942d-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span></span>

> <span data-ttu-id="c942d-107">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="c942d-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c942d-108">Para o conteúdo de um UpdatePanel, existe um extensor especial que depende muito da estrutura de animação: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="c942d-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="c942d-109">Ele também pode trabalhar junto com gatilhos UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="c942d-109">It can also work together with UpdatePanel triggers.</span></span>

## <a name="overview"></a><span data-ttu-id="c942d-110">Visão geral</span><span class="sxs-lookup"><span data-stu-id="c942d-110">Overview</span></span>

<span data-ttu-id="c942d-111">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="c942d-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c942d-112">Para o conteúdo de um `UpdatePanel`, existe um extensor especial que depende muito da estrutura de animação: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="c942d-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="c942d-113">Ele também pode trabalhar junto com `UpdatePanel` gatilhos.</span><span class="sxs-lookup"><span data-stu-id="c942d-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="c942d-114">Etapas</span><span class="sxs-lookup"><span data-stu-id="c942d-114">Steps</span></span>

<span data-ttu-id="c942d-115">A primeira etapa é normalmente para incluir o `ScriptManager` na página para que a biblioteca ASP.NET AJAX seja carregada e o kit de ferramentas de controle possa ser usado:</span><span class="sxs-lookup"><span data-stu-id="c942d-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

<span data-ttu-id="c942d-116">A animação neste cenário será aplicada a uma exibição da hora atual.</span><span class="sxs-lookup"><span data-stu-id="c942d-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="c942d-117">Essas informações podem ser gravadas em um rótulo usando o método `Page_Load()` ou (para simplificar) o seguinte código embutido é usado:</span><span class="sxs-lookup"><span data-stu-id="c942d-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

<span data-ttu-id="c942d-118">Além disso, um botão para disparar a atualização da hora é criado:</span><span class="sxs-lookup"><span data-stu-id="c942d-118">Also, a button to trigger updating the time is created:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

<span data-ttu-id="c942d-119">Em seguida, esse código é colocado na seção `<ContentTemplate>` de um elemento `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="c942d-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="c942d-120">O atributo de `UpdateMode` do painel deve ser definido como `"Conditional"`, já que somente gatilhos podem atualizar o conteúdo do painel.</span><span class="sxs-lookup"><span data-stu-id="c942d-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="c942d-121">Na seção `<Triggers>` do `UpdatePanel`, um gatilho de postback assíncrono é criado e vinculado ao evento `Click` do botão.</span><span class="sxs-lookup"><span data-stu-id="c942d-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="c942d-122">Portanto, se o usuário clicar no botão, a `UpdatePanel` será atualizada.</span><span class="sxs-lookup"><span data-stu-id="c942d-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="c942d-123">Aqui está a marcação para o controle de `UpdatePanel`:</span><span class="sxs-lookup"><span data-stu-id="c942d-123">Here is the markup for the `UpdatePanel` control:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

<span data-ttu-id="c942d-124">Por fim, o `UpdatePanelAnimationExtender` deve ser configurado: defina o atributo `TargetControlID` para a ID do painel e defina uma animação dentro do extensor.</span><span class="sxs-lookup"><span data-stu-id="c942d-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="c942d-125">O esmaecimento faz sentido, o que cria uma boa ênfase visual no tempo atualizado.</span><span class="sxs-lookup"><span data-stu-id="c942d-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="c942d-126">A marcação do extensor pode ter a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="c942d-126">Your extender markup may then look like this:</span></span>

[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

<span data-ttu-id="c942d-127">Execute o arquivo no navegador.</span><span class="sxs-lookup"><span data-stu-id="c942d-127">Run the file in the browser.</span></span> <span data-ttu-id="c942d-128">Sempre que você clicar no botão, a hora atual será mostrada no painel, sempre esmaecida pela duração de um segundo.</span><span class="sxs-lookup"><span data-stu-id="c942d-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>

<span data-ttu-id="c942d-129">[![a hora atual está esmaecida](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c942d-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span></span>

<span data-ttu-id="c942d-130">A hora atual está esmaecida ([clique para exibir a imagem em tamanho normal](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c942d-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c942d-131">Anterior</span><span class="sxs-lookup"><span data-stu-id="c942d-131">Previous</span></span>](animating-an-updatepanel-control-vb.md)
