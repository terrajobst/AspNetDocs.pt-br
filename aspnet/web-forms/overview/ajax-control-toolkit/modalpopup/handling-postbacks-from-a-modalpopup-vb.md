---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Manipulando postbacks de um ModalPopup (VB) | Microsoft Docs
author: wenz
description: O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente. Deve-se tomar cuidado especial quando um PDV...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: df0b71b3e336a0d230869623473bdac24b3dd07b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621523"
---
# <a name="handling-postbacks-from-a-modalpopup-vb"></a><span data-ttu-id="62c03-104">Tratamento de postbacks de um ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="62c03-104">Handling Postbacks from a ModalPopup (VB)</span></span>

<span data-ttu-id="62c03-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="62c03-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="62c03-106">[Baixar código](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="62c03-106">[Download Code](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span></span>

> <span data-ttu-id="62c03-107">O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="62c03-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="62c03-108">Deve-se tomar cuidado especial quando um postback é criado de dentro do pop-up.</span><span class="sxs-lookup"><span data-stu-id="62c03-108">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="overview"></a><span data-ttu-id="62c03-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="62c03-109">Overview</span></span>

<span data-ttu-id="62c03-110">O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="62c03-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="62c03-111">Deve-se tomar cuidado especial quando um postback é criado de dentro do pop-up.</span><span class="sxs-lookup"><span data-stu-id="62c03-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="62c03-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="62c03-112">Steps</span></span>

<span data-ttu-id="62c03-113">Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado em qualquer lugar na página (mas dentro do elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="62c03-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="62c03-114">Em seguida, adicione um painel que serve como o popup modal.</span><span class="sxs-lookup"><span data-stu-id="62c03-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="62c03-115">Lá, o usuário pode inserir um nome e um endereço de email.</span><span class="sxs-lookup"><span data-stu-id="62c03-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="62c03-116">Um botão é usado para fechar o pop-up e salvar as informações.</span><span class="sxs-lookup"><span data-stu-id="62c03-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="62c03-117">Observe que o atributo `OnClick` é definido de forma que um postback ocorra quando esse botão for clicado:</span><span class="sxs-lookup"><span data-stu-id="62c03-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="62c03-118">A página em si consiste em dois rótulos para exatamente as mesmas informações: nome e endereço de email.</span><span class="sxs-lookup"><span data-stu-id="62c03-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="62c03-119">Um botão é usado para disparar o popup modal:</span><span class="sxs-lookup"><span data-stu-id="62c03-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

<span data-ttu-id="62c03-120">Para que o pop-up seja exibido, adicione o controle de `ModalPopupExtender`.</span><span class="sxs-lookup"><span data-stu-id="62c03-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="62c03-121">Defina o atributo `PopupControlID` como a ID do painel e `TargetControlID` à ID do botão:</span><span class="sxs-lookup"><span data-stu-id="62c03-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

<span data-ttu-id="62c03-122">Agora, sempre que o botão de `Save` dentro do popup modal for clicado, o método de `SaveData()` do lado do servidor será executado.</span><span class="sxs-lookup"><span data-stu-id="62c03-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="62c03-123">Lá, você pode salvar os dados inseridos em um armazenamento de dados.</span><span class="sxs-lookup"><span data-stu-id="62c03-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="62c03-124">Para simplificar, os novos dados são apenas de saída no rótulo:</span><span class="sxs-lookup"><span data-stu-id="62c03-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

<span data-ttu-id="62c03-125">Além disso, os controles de caixa de texto dentro do popup modal devem ser preenchidos com o nome e o email atuais.</span><span class="sxs-lookup"><span data-stu-id="62c03-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="62c03-126">No entanto, isso só é necessário quando nenhum postback ocorre.</span><span class="sxs-lookup"><span data-stu-id="62c03-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="62c03-127">Se houver um postback, o recurso ViewState ASP.NET preencherá automaticamente as caixas de Textcom os valores apropriados.</span><span class="sxs-lookup"><span data-stu-id="62c03-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]

<span data-ttu-id="62c03-128">[![o popup modal causa um postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="62c03-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="62c03-129">O popup modal causa um postback ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="62c03-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="62c03-130">[Anterior](using-modalpopup-with-a-repeater-control-vb.md)
> [Próximo](positioning-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="62c03-130">[Previous](using-modalpopup-with-a-repeater-control-vb.md)
[Next](positioning-a-modalpopup-vb.md)</span></span>
