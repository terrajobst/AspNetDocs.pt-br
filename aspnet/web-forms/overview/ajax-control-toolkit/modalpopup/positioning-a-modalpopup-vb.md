---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Posicionando um ModalPopup (VB) | Microsoft Docs
author: wenz
description: O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente. No entanto, o controle não oferece um...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: fb79a08a339588ed8adc4b4236911819ea9286b4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613193"
---
# <a name="positioning-a-modalpopup-vb"></a><span data-ttu-id="c2ba3-104">Posicionamento de um ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="c2ba3-104">Positioning a ModalPopup (VB)</span></span>

<span data-ttu-id="c2ba3-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c2ba3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c2ba3-106">[Baixar código](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c2ba3-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span></span>

> <span data-ttu-id="c2ba3-107">O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="c2ba3-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="c2ba3-108">No entanto, o controle não oferece uma funcionalidade interna para posicionar o pop-up.</span><span class="sxs-lookup"><span data-stu-id="c2ba3-108">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="overview"></a><span data-ttu-id="c2ba3-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="c2ba3-109">Overview</span></span>

<span data-ttu-id="c2ba3-110">O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="c2ba3-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="c2ba3-111">No entanto, o controle não oferece uma funcionalidade interna para posicionar o pop-up.</span><span class="sxs-lookup"><span data-stu-id="c2ba3-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="c2ba3-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="c2ba3-112">Steps</span></span>

<span data-ttu-id="c2ba3-113">Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="c2ba3-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="c2ba3-114">o controle deve ser colocado em qualquer lugar na página (mas dentro do elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="c2ba3-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="c2ba3-115">Em seguida, adicione um painel que serve como o popup modal.</span><span class="sxs-lookup"><span data-stu-id="c2ba3-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="c2ba3-116">Um botão é usado para fechar o pop-up:</span><span class="sxs-lookup"><span data-stu-id="c2ba3-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="c2ba3-117">Sempre que o Popup for mostrado, ele deverá ser posicionado em um determinado local na página.</span><span class="sxs-lookup"><span data-stu-id="c2ba3-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="c2ba3-118">Para essa tarefa, uma função JavaScript do lado do cliente é criada.</span><span class="sxs-lookup"><span data-stu-id="c2ba3-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="c2ba3-119">Ele primeiro tenta acessar o painel.</span><span class="sxs-lookup"><span data-stu-id="c2ba3-119">It first tries to access the panel.</span></span> <span data-ttu-id="c2ba3-120">Se for bem sucedido, a posição do painel será definida usando CSS e JavaScript (altere a posição do pop-up em irá).</span><span class="sxs-lookup"><span data-stu-id="c2ba3-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="c2ba3-121">No entanto, o controle de `ModalPopupExtender` também tenta posicionar o pop-up.</span><span class="sxs-lookup"><span data-stu-id="c2ba3-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="c2ba3-122">Portanto, o código JavaScript posiciona repetidamente o Popup, a cada décimo de segundo.</span><span class="sxs-lookup"><span data-stu-id="c2ba3-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

<span data-ttu-id="c2ba3-123">Como você pode ver, o valor de retorno do `setTimeout()` método JavaScript é salvo em uma variável global.</span><span class="sxs-lookup"><span data-stu-id="c2ba3-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="c2ba3-124">Isso permite interromper o posicionamento repetido do Popup sob demanda, usando o método `clearTimeout()`:</span><span class="sxs-lookup"><span data-stu-id="c2ba3-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

<span data-ttu-id="c2ba3-125">Agora, tudo o que resta fazer é fazer com que o navegador chame essas funções sempre que apropriado.</span><span class="sxs-lookup"><span data-stu-id="c2ba3-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="c2ba3-126">A função JavaScript `movePanel()` deve ser chamada quando o botão é clicado que dispara o painel:</span><span class="sxs-lookup"><span data-stu-id="c2ba3-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

<span data-ttu-id="c2ba3-127">E a função `stopMoving()` entra em ação quando o popup é fechado, isso pode ser disparado no controle de `ModalPopupExtender`:</span><span class="sxs-lookup"><span data-stu-id="c2ba3-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]

<span data-ttu-id="c2ba3-128">[![popup modal aparece na posição designada](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c2ba3-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="c2ba3-129">O popup modal aparece na posição designada ([clique para exibir a imagem em tamanho normal](positioning-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c2ba3-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c2ba3-130">Anterior</span><span class="sxs-lookup"><span data-stu-id="c2ba3-130">Previous</span></span>](handling-postbacks-from-a-modalpopup-vb.md)
