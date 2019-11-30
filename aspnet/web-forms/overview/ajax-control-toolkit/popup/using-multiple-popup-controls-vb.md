---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Usando vários controles pop-up (VB) | Microsoft Docs
author: wenz
description: O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado. Também é possível usar m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: e1f4ff64e9fdf48ea63b75c97acd53a64b5ab5ce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611593"
---
# <a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="fc10c-104">Uso de vários controles pop-up (VB)</span><span class="sxs-lookup"><span data-stu-id="fc10c-104">Using Multiple Popup Controls (VB)</span></span>

<span data-ttu-id="fc10c-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fc10c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fc10c-106">[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="fc10c-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="fc10c-107">O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado.</span><span class="sxs-lookup"><span data-stu-id="fc10c-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="fc10c-108">Também é possível usar mais de um controle popup em uma página.</span><span class="sxs-lookup"><span data-stu-id="fc10c-108">It is also possible to use more than one popup control on one page.</span></span>

## <a name="overview"></a><span data-ttu-id="fc10c-109">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="fc10c-109">Overview</span></span>

<span data-ttu-id="fc10c-110">O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado.</span><span class="sxs-lookup"><span data-stu-id="fc10c-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="fc10c-111">Também é possível usar mais de um controle popup em uma página.</span><span class="sxs-lookup"><span data-stu-id="fc10c-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="fc10c-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="fc10c-112">Steps</span></span>

<span data-ttu-id="fc10c-113">Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado em qualquer lugar na página (mas dentro do elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="fc10c-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="fc10c-114">Em seguida, adicione um painel que serve como o Popup.</span><span class="sxs-lookup"><span data-stu-id="fc10c-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="fc10c-115">No cenário atual, o painel contém um controle de `Calendar`.</span><span class="sxs-lookup"><span data-stu-id="fc10c-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="fc10c-116">Para evitar as atualizações de página causadas pelos postbacks do calendário, o painel é colocado dentro de um controle de `UpdatePanel`:</span><span class="sxs-lookup"><span data-stu-id="fc10c-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="fc10c-117">A página também contém duas caixas de texto.</span><span class="sxs-lookup"><span data-stu-id="fc10c-117">The page also contains two text boxes.</span></span> <span data-ttu-id="fc10c-118">Para cada caixa de texto, o pop-up calendário deve aparecer assim que a caixa de texto for ativada.</span><span class="sxs-lookup"><span data-stu-id="fc10c-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="fc10c-119">Agora, estenda cada uma das duas caixas de texto com uma `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="fc10c-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="fc10c-120">O atributo `TargetControlID` fornece a ID do controle vinculado ao extensor.</span><span class="sxs-lookup"><span data-stu-id="fc10c-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="fc10c-121">O atributo `PopupControlID` contém a ID do painel pop-up.</span><span class="sxs-lookup"><span data-stu-id="fc10c-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="fc10c-122">Nesse caso, os extensores mostram o mesmo painel, mas painéis diferentes também são possíveis.</span><span class="sxs-lookup"><span data-stu-id="fc10c-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="fc10c-123">Agora, sempre que você clicar em um campo de texto, um calendário será exibido abaixo do campo, permitindo que você selecione uma data.</span><span class="sxs-lookup"><span data-stu-id="fc10c-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="fc10c-124">(Obter a data selecionada para trás nas caixas de texto será abordado em um tutorial diferente.)</span><span class="sxs-lookup"><span data-stu-id="fc10c-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>

<span data-ttu-id="fc10c-125">[![o calendário aparece quando o usuário clica na caixa de texto](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fc10c-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="fc10c-126">O calendário é exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fc10c-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fc10c-127">[Anterior](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Próximo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="fc10c-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
