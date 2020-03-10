---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Usando TextBoxWatermark com controles de validaçãoC#() | Microsoft Docs
author: wenz
description: O controle TextBoxWatermark no AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa. Quando um usuário clica na caixa, ele...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: bc9498b1c5ba2f38b90706c9200ffa813a945fa9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78577836"
---
# <a name="using-textboxwatermark-with-validation-controls-c"></a><span data-ttu-id="8a3ea-104">Uso de TextBoxWatermark com controles de validação (C#)</span><span class="sxs-lookup"><span data-stu-id="8a3ea-104">Using TextBoxWatermark With Validation Controls (C#)</span></span>

<span data-ttu-id="8a3ea-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8a3ea-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8a3ea-106">[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8a3ea-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span></span>

> <span data-ttu-id="8a3ea-107">O controle TextBoxWatermark no AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa.</span><span class="sxs-lookup"><span data-stu-id="8a3ea-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="8a3ea-108">Quando um usuário clica na caixa, ele é esvaziado.</span><span class="sxs-lookup"><span data-stu-id="8a3ea-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="8a3ea-109">Se o usuário deixar a caixa sem inserir o texto, o texto preenchedo reaparecerá.</span><span class="sxs-lookup"><span data-stu-id="8a3ea-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="8a3ea-110">Isso pode colidir com controles de validação ASP.NET na mesma página, mas esses problemas podem ser superados.</span><span class="sxs-lookup"><span data-stu-id="8a3ea-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="overview"></a><span data-ttu-id="8a3ea-111">Visão geral</span><span class="sxs-lookup"><span data-stu-id="8a3ea-111">Overview</span></span>

<span data-ttu-id="8a3ea-112">O controle de `TextBoxWatermark` no AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa.</span><span class="sxs-lookup"><span data-stu-id="8a3ea-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="8a3ea-113">Quando um usuário clica na caixa, ele é esvaziado.</span><span class="sxs-lookup"><span data-stu-id="8a3ea-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="8a3ea-114">Se o usuário deixar a caixa sem inserir o texto, o texto preenchedo reaparecerá.</span><span class="sxs-lookup"><span data-stu-id="8a3ea-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="8a3ea-115">Isso pode colidir com controles de validação ASP.NET na mesma página, mas esses problemas podem ser superados.</span><span class="sxs-lookup"><span data-stu-id="8a3ea-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="8a3ea-116">Etapas</span><span class="sxs-lookup"><span data-stu-id="8a3ea-116">Steps</span></span>

<span data-ttu-id="8a3ea-117">A configuração básica do exemplo é a seguinte: um controle de `TextBox` é marca d' água usando um controle de `TextBoxWatermarkExtender`.</span><span class="sxs-lookup"><span data-stu-id="8a3ea-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="8a3ea-118">Um botão dispara um postback e posteriormente será usado para disparar os controles de validação na página.</span><span class="sxs-lookup"><span data-stu-id="8a3ea-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="8a3ea-119">Além disso, um controle de `ScriptManager` é necessário para inicializar o ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="8a3ea-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="8a3ea-120">Agora, adicione um controle de `RequiredFieldValidator` que verifica se há texto no campo quando o formulário é enviado.</span><span class="sxs-lookup"><span data-stu-id="8a3ea-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="8a3ea-121">A propriedade `InitialValue` do validador deve ser definida com o mesmo valor usado no controle de `TextBoxWatermarkExtender`: quando o formulário é enviado, o valor de uma caixa de texto inalterada é o valor de marca d' água dentro dela:</span><span class="sxs-lookup"><span data-stu-id="8a3ea-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="8a3ea-122">No entanto, há um problema com essa abordagem: se o cliente desabilitar o JavaScript, o campo de texto não será preenchido com o texto de marca d' água, portanto, o `RequiredFieldValidator` não disparará uma mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="8a3ea-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="8a3ea-123">Portanto, um segundo controle de `RequiredFieldValidator` é necessário, que verifica se há uma caixa de texto vazia (omitindo o atributo `InitialValue`).</span><span class="sxs-lookup"><span data-stu-id="8a3ea-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="8a3ea-124">Como ambos os validadores usam `Display`=`"Dynamic"`, o usuário final não pode distinguir da aparência visual quais dos dois validadores foram acionados; em vez disso, parece que havia apenas um deles.</span><span class="sxs-lookup"><span data-stu-id="8a3ea-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="8a3ea-125">Por fim, adicione um código do lado do servidor para gerar o texto no campo se nenhum validador emitiu uma mensagem de erro:</span><span class="sxs-lookup"><span data-stu-id="8a3ea-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="8a3ea-126">[![o validador reclama que não há texto no campo](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8a3ea-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="8a3ea-127">O validador reclama que não há texto no campo ([clique para exibir a imagem em tamanho normal](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8a3ea-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8a3ea-128">[Anterior](using-textboxwatermark-in-a-formview-cs.md)
> [Próximo](using-textboxwatermark-in-a-formview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8a3ea-128">[Previous](using-textboxwatermark-in-a-formview-cs.md)
[Next](using-textboxwatermark-in-a-formview-vb.md)</span></span>
