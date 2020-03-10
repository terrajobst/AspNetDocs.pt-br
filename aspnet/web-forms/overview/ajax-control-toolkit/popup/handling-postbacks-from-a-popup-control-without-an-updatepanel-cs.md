---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Manipulando postbacks de um controle Popup sem um UpdatePanel (C#) | Microsoft Docs
author: wenz
description: O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado. Quando um postback ocorre no Su...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 9c4c59bb9dbd3e2ba2b3b81ecf76271f21673bce
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612738"
---
# <a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a><span data-ttu-id="7fdf1-104">Tratamento de postbacks de um controle pop-up sem um UpdatePanel (C#)</span><span class="sxs-lookup"><span data-stu-id="7fdf1-104">Handling Postbacks from A Popup Control Without an UpdatePanel (C#)</span></span>

<span data-ttu-id="7fdf1-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7fdf1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7fdf1-106">[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7fdf1-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span></span>

> <span data-ttu-id="7fdf1-107">O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado.</span><span class="sxs-lookup"><span data-stu-id="7fdf1-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="7fdf1-108">Quando ocorre um postback nesse painel e há vários painéis na página, é difícil determinar qual painel foi clicado.</span><span class="sxs-lookup"><span data-stu-id="7fdf1-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="overview"></a><span data-ttu-id="7fdf1-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="7fdf1-109">Overview</span></span>

<span data-ttu-id="7fdf1-110">O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado.</span><span class="sxs-lookup"><span data-stu-id="7fdf1-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="7fdf1-111">Quando ocorre um postback nesse painel e há vários painéis na página, é difícil determinar qual painel foi clicado.</span><span class="sxs-lookup"><span data-stu-id="7fdf1-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="7fdf1-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="7fdf1-112">Steps</span></span>

<span data-ttu-id="7fdf1-113">Ao usar um `PopupControl` com um postback, mas sem ter uma `UpdatePanel` na página, o kit de ferramentas de controle não oferece uma maneira de determinar qual elemento de cliente disparou o Popup que, por sua vez, causou o postback.</span><span class="sxs-lookup"><span data-stu-id="7fdf1-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="7fdf1-114">No entanto, um pequeno truque fornece uma solução alternativa para esse cenário.</span><span class="sxs-lookup"><span data-stu-id="7fdf1-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="7fdf1-115">Em primeiro lugar, aqui está a configuração básica: duas caixas de texto que disparam o mesmo pop-up, um calendário.</span><span class="sxs-lookup"><span data-stu-id="7fdf1-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="7fdf1-116">Dois `PopupControlExtenders` trazer caixas de texto e pop-up juntos.</span><span class="sxs-lookup"><span data-stu-id="7fdf1-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="7fdf1-117">A ideia básica é adicionar um campo de formulário oculto no elemento &lt;`form`&gt; que contém a caixa de texto que abriu o Popup:</span><span class="sxs-lookup"><span data-stu-id="7fdf1-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="7fdf1-118">Quando a página é carregada, o código JavaScript adiciona um manipulador de eventos a ambas as caixas de texto: sempre que o usuário clica em uma caixa de texto, seu nome é gravado no campo de formulário oculto:</span><span class="sxs-lookup"><span data-stu-id="7fdf1-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

<span data-ttu-id="7fdf1-119">No código do servidor, o valor do campo oculto deve ser lido.</span><span class="sxs-lookup"><span data-stu-id="7fdf1-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="7fdf1-120">Como os campos de formulário ocultos são triviais para manipular, é necessária uma abordagem de lista de permissões para validar o valor oculto.</span><span class="sxs-lookup"><span data-stu-id="7fdf1-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="7fdf1-121">Depois que a caixa de texto correta tiver sido identificada, a data do calendário será gravada nela.</span><span class="sxs-lookup"><span data-stu-id="7fdf1-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]

<span data-ttu-id="7fdf1-122">[![o calendário aparece quando o usuário clica na caixa de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7fdf1-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="7fdf1-123">O calendário é exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7fdf1-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span></span>

<span data-ttu-id="7fdf1-124">[![clicar em uma data a coloca na caixa de texto](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="7fdf1-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="7fdf1-125">Clicar em uma data a coloca na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="7fdf1-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7fdf1-126">[Anterior](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [Próximo](using-multiple-popup-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7fdf1-126">[Previous](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Next](using-multiple-popup-controls-vb.md)</span></span>
