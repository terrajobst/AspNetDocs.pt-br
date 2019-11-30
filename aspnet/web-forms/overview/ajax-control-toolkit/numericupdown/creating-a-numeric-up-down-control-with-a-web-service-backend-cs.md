---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Criação de um controle numérico para cima/para baixo com um backC#-end do serviço Web () | Microsoft Docs
author: wenz
description: Em vez de permitir que um usuário digite um valor em uma caixa de seleção, um controle numérico para cima/para baixo (que existe no Windows e em outros sistemas operacionais) poderia provar mais c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: 816a840b9e93b95a049c3a4cb792e9deeab28983
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598897"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a><span data-ttu-id="0fbf6-103">Criação um controle numérico para cima/para baixo com um back-end de serviço Web (C#)</span><span class="sxs-lookup"><span data-stu-id="0fbf6-103">Creating a Numeric Up/Down Control with a Web Service Backend (C#)</span></span>

<span data-ttu-id="0fbf6-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0fbf6-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0fbf6-105">[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0fbf6-105">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)</span></span>

> <span data-ttu-id="0fbf6-106">Em vez de permitir que um usuário digite um valor em uma caixa de seleção, um controle numérico para cima/para baixo (que existe no Windows e em outros sistemas operacionais) pode provar o mais confortável.</span><span class="sxs-lookup"><span data-stu-id="0fbf6-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="0fbf6-107">Por padrão, o controle NumericUpDown sempre aumenta ou diminui um valor em 1, mas um serviço Web comprova mais flexibilidade.</span><span class="sxs-lookup"><span data-stu-id="0fbf6-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="overview"></a><span data-ttu-id="0fbf6-108">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="0fbf6-108">Overview</span></span>

<span data-ttu-id="0fbf6-109">Em vez de permitir que um usuário digite um valor em uma caixa de seleção, um controle numérico para cima/para baixo (que existe no Windows e em outros sistemas operacionais) pode provar o mais confortável.</span><span class="sxs-lookup"><span data-stu-id="0fbf6-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="0fbf6-110">Por padrão, o controle de `NumericUpDown` sempre aumenta ou diminui um valor em 1, mas um serviço Web comprova mais flexibilidade.</span><span class="sxs-lookup"><span data-stu-id="0fbf6-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="0fbf6-111">Etapas</span><span class="sxs-lookup"><span data-stu-id="0fbf6-111">Steps</span></span>

<span data-ttu-id="0fbf6-112">O ASP.NET AJAX Control Toolkit contém o `NumericUpDown` Extender, que adiciona automaticamente dois botões a uma caixa de texto: um para aumentar seu valor, um para diminuir.</span><span class="sxs-lookup"><span data-stu-id="0fbf6-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="0fbf6-113">No entanto, o controle também dá suporte a uma chamada de serviço Web (ou chamada de método de página).</span><span class="sxs-lookup"><span data-stu-id="0fbf6-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="0fbf6-114">Sempre que o botão para cima ou para baixo é clicado, o código JavaScript se conecta ao servidor Web e executa um método lá.</span><span class="sxs-lookup"><span data-stu-id="0fbf6-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="0fbf6-115">A assinatura do método é a seguinte:</span><span class="sxs-lookup"><span data-stu-id="0fbf6-115">The method signature is the following one:</span></span>

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

<span data-ttu-id="0fbf6-116">O argumento `current` é o valor atual na caixa de texto; o atributo `tag` são dados de contexto adicionais que podem ser definidos como uma propriedade do extensor `NumericUpDown` (mas não é obrigatório).</span><span class="sxs-lookup"><span data-stu-id="0fbf6-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="0fbf6-117">Para este exemplo, o controle numérico para cima/para baixo só deve permitir valores que sejam potências de dois: 1, 2, 4, 8, 16, 32, 64 e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="0fbf6-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="0fbf6-118">Portanto, o método executado quando o usuário deseja aumentar o valor deve dobrar o valor antigo; o outro método deve dividir o valor em dois.</span><span class="sxs-lookup"><span data-stu-id="0fbf6-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="0fbf6-119">Aqui está o serviço Web completo:</span><span class="sxs-lookup"><span data-stu-id="0fbf6-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

<span data-ttu-id="0fbf6-120">Por fim, crie uma nova página do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0fbf6-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="0fbf6-121">Como de costume, você precisa de um controle de `ScriptManager`, um controle de `TextBox` e um controle de `NumericUpDownExtender`.</span><span class="sxs-lookup"><span data-stu-id="0fbf6-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="0fbf6-122">Para o último, você precisa fornecer as informações do serviço Web:</span><span class="sxs-lookup"><span data-stu-id="0fbf6-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="0fbf6-123">`ServiceDownMethod` nome do método da Web ou do método de página inoperante</span><span class="sxs-lookup"><span data-stu-id="0fbf6-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="0fbf6-124">`ServiceDownPath` caminho para o serviço Web com o método de serviço inoperante; omitir se você estiver usando um método de página</span><span class="sxs-lookup"><span data-stu-id="0fbf6-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="0fbf6-125">`ServiceUpMethod` nome do método ou da página da Web up</span><span class="sxs-lookup"><span data-stu-id="0fbf6-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="0fbf6-126">`ServiceUpPath` caminho para o serviço Web com o método de serviço up; omitir se você estiver usando um método de página</span><span class="sxs-lookup"><span data-stu-id="0fbf6-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="0fbf6-127">Aqui está a marcação completa para a página:</span><span class="sxs-lookup"><span data-stu-id="0fbf6-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

<span data-ttu-id="0fbf6-128">Se você executar a página, observe como o valor na caixa de texto sempre é dobrado quando você clica no botão superior e é dividido quando você clica no botão inferior.</span><span class="sxs-lookup"><span data-stu-id="0fbf6-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>

<span data-ttu-id="0fbf6-129">[![apenas números que são uma potência de 2 aparecem](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0fbf6-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)</span></span>

<span data-ttu-id="0fbf6-130">Somente os números que são uma potência de 2 aparecem ([clique para exibir a imagem em tamanho normal](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0fbf6-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0fbf6-131">Avançar</span><span class="sxs-lookup"><span data-stu-id="0fbf6-131">Next</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
