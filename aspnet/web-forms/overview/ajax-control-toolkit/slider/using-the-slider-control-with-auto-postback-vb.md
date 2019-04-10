---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Usando o controle deslizante com Postback automático (VB) | Microsoft Docs
author: wenz
description: O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse. É possível fazer a lançar automaticamente slider...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: 702cdd898e261f6a5793fa04069b69398745d576
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59415577"
---
# <a name="using-the-slider-control-with-auto-postback-vb"></a><span data-ttu-id="a3b02-104">Usando o controle deslizante com Postback automático (VB)</span><span class="sxs-lookup"><span data-stu-id="a3b02-104">Using the Slider Control With Auto-Postback (VB)</span></span>

<span data-ttu-id="a3b02-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a3b02-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a3b02-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a3b02-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span></span>

> <span data-ttu-id="a3b02-107">O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse.</span><span class="sxs-lookup"><span data-stu-id="a3b02-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="a3b02-108">É possível fazer o controle deslizante autopostback uma vez, seu valor for alterado.</span><span class="sxs-lookup"><span data-stu-id="a3b02-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="a3b02-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="a3b02-109">Overview</span></span>

<span data-ttu-id="a3b02-110">O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse.</span><span class="sxs-lookup"><span data-stu-id="a3b02-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="a3b02-111">É possível fazer o controle deslizante autopostback uma vez, seu valor for alterado.</span><span class="sxs-lookup"><span data-stu-id="a3b02-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="a3b02-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="a3b02-112">Steps</span></span>

<span data-ttu-id="a3b02-113">Para fazer o controle deslizante com postback automaticamente após uma alteração, ambas as caixas de texto precisam do atributo `AutoPostBack="true"`: Caixa de texto que se tornará o controle deslizante em si e a caixa de texto que contém a posição do controle deslizante.</span><span class="sxs-lookup"><span data-stu-id="a3b02-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="a3b02-114">Aqui está a marcação necessária para fazer isso:</span><span class="sxs-lookup"><span data-stu-id="a3b02-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

<span data-ttu-id="a3b02-115">O `SliderExtender` controle do ASP.NET AJAX Control Toolkit atribui a funcionalidade de controle deslizante para duas caixas de texto:</span><span class="sxs-lookup"><span data-stu-id="a3b02-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

<span data-ttu-id="a3b02-116">Um elemento de rótulo adicional será usado mais tarde para informar o usuário de um postback:</span><span class="sxs-lookup"><span data-stu-id="a3b02-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

<span data-ttu-id="a3b02-117">Por fim, o `ScriptManager` controle do ASP.NET AJAX carrega o JavaScript necessário para o Kit de ferramentas trabalhar:</span><span class="sxs-lookup"><span data-stu-id="a3b02-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

<span data-ttu-id="a3b02-118">Agora o controle deslizante está postando novamente; no lado do servidor, esse evento pode ser detectado e tratado:</span><span class="sxs-lookup"><span data-stu-id="a3b02-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


[![M<span data-ttu-id="a3b02-119">o controle deslizante de ão dispara um postback]</span><span class="sxs-lookup"><span data-stu-id="a3b02-119">oving the slider triggers a postback]</span></span>(using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

<span data-ttu-id="a3b02-120">Movendo o controle deslizante dispara um postback ([clique para exibir a imagem em tamanho normal](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a3b02-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span></span>


[![A<span data-ttu-id="a3b02-121">fterwards, a data desta alteração é gravada no rótulo]</span><span class="sxs-lookup"><span data-stu-id="a3b02-121">fterwards, the date of this change is written in the label]</span></span>(using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

<span data-ttu-id="a3b02-122">Depois disso, a data desta alteração é gravada no rótulo ([clique para exibir a imagem em tamanho normal](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a3b02-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a3b02-123">[Anterior](databinding-the-slider-control-cs.md)
> [Próximo](databinding-the-slider-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a3b02-123">[Previous](databinding-the-slider-control-cs.md)
[Next](databinding-the-slider-control-vb.md)</span></span>
