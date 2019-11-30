---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Vinculando o controle deslizante (C#) | Microsoft Docs
author: wenz
description: O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse. É possível associar o Positio atual...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ef547573f17f3265ad72717d3d3bbc622fd6894e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598602"
---
# <a name="databinding-the-slider-control-c"></a><span data-ttu-id="61cdf-104">Associação de Dados do controle deslizante (C#)</span><span class="sxs-lookup"><span data-stu-id="61cdf-104">Databinding the Slider Control (C#)</span></span>

<span data-ttu-id="61cdf-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="61cdf-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="61cdf-106">[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="61cdf-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="61cdf-107">O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse.</span><span class="sxs-lookup"><span data-stu-id="61cdf-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="61cdf-108">É possível associar a posição atual do controle deslizante a outro controle ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="61cdf-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="overview"></a><span data-ttu-id="61cdf-109">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="61cdf-109">Overview</span></span>

<span data-ttu-id="61cdf-110">O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse.</span><span class="sxs-lookup"><span data-stu-id="61cdf-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="61cdf-111">É possível associar a posição atual do controle deslizante a outro controle ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="61cdf-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="61cdf-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="61cdf-112">Steps</span></span>

<span data-ttu-id="61cdf-113">Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado em qualquer lugar na página (mas dentro do elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="61cdf-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="61cdf-114">Em seguida, adicione dois controles de `TextBox` à página.</span><span class="sxs-lookup"><span data-stu-id="61cdf-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="61cdf-115">Uma será transformada em um controle deslizante gráfico e a outra terá a posição do controle deslizante.</span><span class="sxs-lookup"><span data-stu-id="61cdf-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="61cdf-116">A próxima etapa já é a etapa final.</span><span class="sxs-lookup"><span data-stu-id="61cdf-116">The next step is already the final step.</span></span> <span data-ttu-id="61cdf-117">O controle de `SliderExtender` do ASP.NET AJAX Control Toolkit faz um controle deslizante da primeira caixa de texto e atualiza automaticamente a segunda caixa de texto quando a posição do controle deslizante é alterada.</span><span class="sxs-lookup"><span data-stu-id="61cdf-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="61cdf-118">Para que isso funcione, o atributo `TargetControlID` do `SliderExtender`deve ser definido como a ID da primeira caixa de texto; o atributo `BoundControlID` deve ser definido como a ID da segunda caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="61cdf-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="61cdf-119">Como você pode ver no navegador, a vinculação de dados funciona em ambas as direções: inserir um novo valor na caixa de texto atualiza a posição do controle deslizante.</span><span class="sxs-lookup"><span data-stu-id="61cdf-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="61cdf-120">Se você fizer a segunda caixa de texto somente leitura, poderá adicionar uma proteção fraca ao campo de texto para que seja mais difícil para o usuário atualizar manualmente o valor ali.</span><span class="sxs-lookup"><span data-stu-id="61cdf-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>

<span data-ttu-id="61cdf-121">[o controle deslizante de ![e a caixa de texto estão em sincronia](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="61cdf-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="61cdf-122">O controle deslizante e a caixa de texto estão em sincronia ([clique para exibir a imagem em tamanho normal](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="61cdf-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="61cdf-123">[Anterior](using-the-slider-control-with-auto-postback-cs.md)
> [Próximo](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="61cdf-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
