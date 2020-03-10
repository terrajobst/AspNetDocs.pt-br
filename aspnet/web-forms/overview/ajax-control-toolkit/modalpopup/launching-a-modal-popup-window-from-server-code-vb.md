---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Iniciando uma janela pop-up modal do código do servidor (VB) | Microsoft Docs
author: wenz
description: O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente. No entanto, alguns cenários exigem que t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 1368a78d35ac6461bbc2e852e468f42eef2c0d2c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613270"
---
# <a name="launching-a-modal-popup-window-from-server-code-vb"></a><span data-ttu-id="f164b-104">Inicialização de uma janela ModalPopup por meio de código do servidor (VB)</span><span class="sxs-lookup"><span data-stu-id="f164b-104">Launching a Modal Popup Window from Server Code (VB)</span></span>

<span data-ttu-id="f164b-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f164b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f164b-106">[Baixar código](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f164b-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span></span>

> <span data-ttu-id="f164b-107">O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="f164b-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="f164b-108">No entanto, alguns cenários exigem que a abertura do popup modal seja disparada no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="f164b-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="overview"></a><span data-ttu-id="f164b-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="f164b-109">Overview</span></span>

<span data-ttu-id="f164b-110">O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="f164b-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="f164b-111">No entanto, alguns cenários exigem que a abertura do popup modal seja disparada no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="f164b-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="f164b-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="f164b-112">Steps</span></span>

<span data-ttu-id="f164b-113">Em primeiro lugar, um controle da Web de botão ASP.NET é necessário para demonstrar como o controle ModalPopup funciona.</span><span class="sxs-lookup"><span data-stu-id="f164b-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="f164b-114">Adicione esse botão dentro do &lt;formulário&gt; elemento em uma nova página:</span><span class="sxs-lookup"><span data-stu-id="f164b-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

<span data-ttu-id="f164b-115">Em seguida, você precisa da marcação para o Popup que deseja criar.</span><span class="sxs-lookup"><span data-stu-id="f164b-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="f164b-116">Defina-o como um controle de `<asp:Panel>` e certifique-se de que ele inclui um controle de botão.</span><span class="sxs-lookup"><span data-stu-id="f164b-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="f164b-117">O controle ModalPopup oferece a funcionalidade para fazer esse botão fechar o pop-up; caso contrário, não há uma maneira fácil de deixá-lo desaparecer.</span><span class="sxs-lookup"><span data-stu-id="f164b-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

<span data-ttu-id="f164b-118">Em seguida, adicione o controle ModalPopup do kit de ferramentas do ASP.NET AJAX à página.</span><span class="sxs-lookup"><span data-stu-id="f164b-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="f164b-119">Defina as propriedades para o botão que carrega o controle, o botão que o torna desaparecedo e a ID do Popup real.</span><span class="sxs-lookup"><span data-stu-id="f164b-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

<span data-ttu-id="f164b-120">Como com todas as páginas da Web baseadas no ASP.NET AJAX; o Gerenciador de scripts é necessário para carregar as bibliotecas JavaScript necessárias para os diferentes navegadores de destino:</span><span class="sxs-lookup"><span data-stu-id="f164b-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

<span data-ttu-id="f164b-121">Execute o exemplo no navegador.</span><span class="sxs-lookup"><span data-stu-id="f164b-121">Run the example in the browser.</span></span> <span data-ttu-id="f164b-122">Quando você clica no botão, o popup modal é exibido.</span><span class="sxs-lookup"><span data-stu-id="f164b-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="f164b-123">Para obter o mesmo efeito usando o código do servidor, um novo botão é necessário:</span><span class="sxs-lookup"><span data-stu-id="f164b-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

<span data-ttu-id="f164b-124">Como você pode ver, um clique no botão gera um postback e executa o método `ServerButton_Click()` no servidor.</span><span class="sxs-lookup"><span data-stu-id="f164b-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="f164b-125">Nesse método, uma função JavaScript chamada `launchModal()` é executada para ser exata, a função JavaScript será executada assim que a página for carregada:</span><span class="sxs-lookup"><span data-stu-id="f164b-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

<span data-ttu-id="f164b-126">O trabalho de `launchModal()` é exibir o ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="f164b-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="f164b-127">A função `launchModal()` é executada quando a página HTML completa é carregada.</span><span class="sxs-lookup"><span data-stu-id="f164b-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="f164b-128">Nesse momento, no entanto, a estrutura do ASP.NET AJAX ainda não foi totalmente carregada.</span><span class="sxs-lookup"><span data-stu-id="f164b-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="f164b-129">Portanto, a função `launchModal()` apenas define uma variável que o controle ModalPopup deve ser mostrado posteriormente:</span><span class="sxs-lookup"><span data-stu-id="f164b-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

<span data-ttu-id="f164b-130">O `pageLoad()` função JavaScript é uma função especial que é executada quando o ASP.NET AJAX tiver sido totalmente carregado.</span><span class="sxs-lookup"><span data-stu-id="f164b-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="f164b-131">Portanto, adicionamos o código a essa função para mostrar o controle ModalPopup, mas somente se `launchModal()` tiver sido chamado antes:</span><span class="sxs-lookup"><span data-stu-id="f164b-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

<span data-ttu-id="f164b-132">A função `$find()` está procurando um elemento nomeado na página e espera a ID do lado do servidor como um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="f164b-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="f164b-133">Portanto, `$find("mpe")` retorna a representação de cliente do controle ModalPopup; seu método `show()` permite que o pop-up apareça.</span><span class="sxs-lookup"><span data-stu-id="f164b-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>

<span data-ttu-id="f164b-134">[![popup modal aparece quando um dos botões é clicado](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f164b-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="f164b-135">O popup modal aparece quando um dos botões é clicado ([clique para exibir a imagem em tamanho normal](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f164b-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f164b-136">[Anterior](positioning-a-modalpopup-cs.md)
> [Próximo](using-modalpopup-with-a-repeater-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f164b-136">[Previous](positioning-a-modalpopup-cs.md)
[Next](using-modalpopup-with-a-repeater-control-vb.md)</span></span>
