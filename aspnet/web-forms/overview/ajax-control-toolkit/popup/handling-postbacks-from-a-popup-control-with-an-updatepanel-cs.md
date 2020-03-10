---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Manipulando postbacks de um controle popup com um UpdatePanel (C#) | Microsoft Docs
author: wenz
description: O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado. Deve-se tomar cuidado especial...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b9e58d68b3d6c5d01ceaba6c01653e9574b541b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612927"
---
# <a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a><span data-ttu-id="df8f0-104">Tratamento de postbacks de um controle pop-up com um UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="df8f0-104">Handling Postbacks from A Popup Control With an UpdatePanel (C#)</span></span>

<span data-ttu-id="df8f0-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="df8f0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="df8f0-106">[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="df8f0-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span></span>

> <span data-ttu-id="df8f0-107">O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado.</span><span class="sxs-lookup"><span data-stu-id="df8f0-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="df8f0-108">Deve-se tomar cuidado especial quando um postback ocorre dentro de um pop-up.</span><span class="sxs-lookup"><span data-stu-id="df8f0-108">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="overview"></a><span data-ttu-id="df8f0-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="df8f0-109">Overview</span></span>

<span data-ttu-id="df8f0-110">O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado.</span><span class="sxs-lookup"><span data-stu-id="df8f0-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="df8f0-111">Deve-se tomar cuidado especial quando um postback ocorre dentro de um pop-up.</span><span class="sxs-lookup"><span data-stu-id="df8f0-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="df8f0-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="df8f0-112">Steps</span></span>

<span data-ttu-id="df8f0-113">Ao usar um `PopupControl` com um postback, um `UpdatePanel` pode impedir a atualização da página causada pelo postback.</span><span class="sxs-lookup"><span data-stu-id="df8f0-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="df8f0-114">A marcação a seguir define alguns elementos importantes:</span><span class="sxs-lookup"><span data-stu-id="df8f0-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="df8f0-115">Um controle de `ScriptManager` para que o kit de ferramentas de controle AJAX ASP.NET funcione</span><span class="sxs-lookup"><span data-stu-id="df8f0-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="df8f0-116">Dois controles `TextBox` que irão disparar um pop-up</span><span class="sxs-lookup"><span data-stu-id="df8f0-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="df8f0-117">Um controle `Panel` que servirá como o pop-up</span><span class="sxs-lookup"><span data-stu-id="df8f0-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="df8f0-118">Dentro do painel, um controle de `Calendar` é inserido dentro de um controle de `UpdatePanel`</span><span class="sxs-lookup"><span data-stu-id="df8f0-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="df8f0-119">Dois controles `PopupControlExtender` que atribuem o painel às caixas de texto</span><span class="sxs-lookup"><span data-stu-id="df8f0-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="df8f0-120">Observe que o atributo `OnSelectionChanged` do controle `Calendar` está definido.</span><span class="sxs-lookup"><span data-stu-id="df8f0-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="df8f0-121">Assim, quando o usuário seleciona uma data dentro do calendário, ocorre um postback e o método do servidor `c1_SelectionChanged()` é executado.</span><span class="sxs-lookup"><span data-stu-id="df8f0-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="df8f0-122">Dentro desse método, a data atual deve ser recuperada e gravada novamente na caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="df8f0-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="df8f0-123">A sintaxe para isso é a seguinte: primeiro de todos, um objeto proxy para o `PopupControlExtender` na página deve ser gerado.</span><span class="sxs-lookup"><span data-stu-id="df8f0-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="df8f0-124">O ASP.NET AJAX Control Toolkit oferece o método `GetProxyForCurrentPopup()`.</span><span class="sxs-lookup"><span data-stu-id="df8f0-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="df8f0-125">O objeto que esse método retorna dá suporte ao método `Commit()` que envia um valor de volta para o controle que disparou o Popup (não o controle que disparou a chamada de método!).</span><span class="sxs-lookup"><span data-stu-id="df8f0-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="df8f0-126">O código a seguir fornece a data selecionada como o argumento para o método `Commit()`, fazendo com que o código grave a data selecionada de volta na caixa de texto:</span><span class="sxs-lookup"><span data-stu-id="df8f0-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="df8f0-127">Agora, sempre que você clicar em uma data do calendário, a data selecionada aparecerá na caixa de texto associada, criando um controle de seletor de data que pode ser encontrado atualmente em muitos sites.</span><span class="sxs-lookup"><span data-stu-id="df8f0-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>

<span data-ttu-id="df8f0-128">[![o calendário aparece quando o usuário clica na caixa de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="df8f0-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="df8f0-129">O calendário é exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="df8f0-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span></span>

<span data-ttu-id="df8f0-130">[![clicar em uma data a coloca na caixa de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="df8f0-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="df8f0-131">Clicar em uma data a coloca na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="df8f0-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="df8f0-132">[Anterior](using-multiple-popup-controls-cs.md)
> [Próximo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span><span class="sxs-lookup"><span data-stu-id="df8f0-132">[Previous](using-multiple-popup-controls-cs.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span></span>
