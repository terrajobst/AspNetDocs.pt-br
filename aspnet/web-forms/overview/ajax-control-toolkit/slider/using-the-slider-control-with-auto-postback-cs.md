---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Usando o controle deslizante com postback automático (C#) | Microsoft Docs
author: wenz
description: O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse. É possível fazer com que o controle deslizante autolance...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: 785d62108667fddac42994344cde265e82aca8f4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598379"
---
# <a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="d120d-104">Usando o controle deslizante com postback automático (C#)</span><span class="sxs-lookup"><span data-stu-id="d120d-104">Using the Slider Control With Auto-Postback (C#)</span></span>

<span data-ttu-id="d120d-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d120d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d120d-106">[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d120d-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="d120d-107">O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse.</span><span class="sxs-lookup"><span data-stu-id="d120d-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="d120d-108">É possível fazer o controle deslizante AutoPostBack quando seu valor é alterado.</span><span class="sxs-lookup"><span data-stu-id="d120d-108">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="overview"></a><span data-ttu-id="d120d-109">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="d120d-109">Overview</span></span>

<span data-ttu-id="d120d-110">O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse.</span><span class="sxs-lookup"><span data-stu-id="d120d-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="d120d-111">É possível fazer o controle deslizante AutoPostBack quando seu valor é alterado.</span><span class="sxs-lookup"><span data-stu-id="d120d-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="d120d-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="d120d-112">Steps</span></span>

<span data-ttu-id="d120d-113">Para fazer com que o controle deslizante seja automaticamente postback após uma alteração, ambas as caixas de texto precisam do atributo `AutoPostBack="true"`: a caixa de texto que se tornará o controle deslizante e a caixa de texto que contém a posição do controle deslizante.</span><span class="sxs-lookup"><span data-stu-id="d120d-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="d120d-114">Aqui está a marcação necessária para isso:</span><span class="sxs-lookup"><span data-stu-id="d120d-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="d120d-115">O controle de `SliderExtender` do kit de ferramentas de controle AJAX ASP.NET atribui a funcionalidade Slider às duas caixas de texto:</span><span class="sxs-lookup"><span data-stu-id="d120d-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="d120d-116">Um elemento rótulo adicional posteriormente será usado para informar o usuário de um postback:</span><span class="sxs-lookup"><span data-stu-id="d120d-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="d120d-117">Por fim, o `ScriptManager` controle do ASP.NET AJAX carrega o JavaScript necessário para que o kit de ferramentas de controle funcione:</span><span class="sxs-lookup"><span data-stu-id="d120d-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="d120d-118">Agora o controle deslizante está fazendo o lançamento; no lado do servidor, esse evento pode ser capturado e Tratado:</span><span class="sxs-lookup"><span data-stu-id="d120d-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]

<span data-ttu-id="d120d-119">[![mover o controle deslizante dispara um postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d120d-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="d120d-120">Mover o controle deslizante dispara um postback ([clique para exibir a imagem em tamanho normal](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d120d-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>

<span data-ttu-id="d120d-121">[![depois, a data dessa alteração será gravada no rótulo](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="d120d-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="d120d-122">Posteriormente, a data dessa alteração será gravada no rótulo ([clique para exibir a imagem em tamanho normal](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="d120d-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="d120d-123">Avançar</span><span class="sxs-lookup"><span data-stu-id="d120d-123">Next</span></span>](databinding-the-slider-control-cs.md)
