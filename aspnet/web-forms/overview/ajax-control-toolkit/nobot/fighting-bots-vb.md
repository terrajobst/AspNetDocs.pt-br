---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: Combatendo bots (VB) | Microsoft Docs
author: wenz
description: Weblogs automáticos de bots Plaster e outros sites com spam, enviando formulários de comentários sem nenhuma interação do usuário. O controle NoBot no ASP.NET AJAX con...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: a8ca71b96cb84c97b1a60ae6a3d1a129cd1b0b10
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627389"
---
# <a name="fighting-bots-vb"></a><span data-ttu-id="56425-104">Bots de combate (VB)</span><span class="sxs-lookup"><span data-stu-id="56425-104">Fighting Bots (VB)</span></span>

<span data-ttu-id="56425-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="56425-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="56425-106">[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="56425-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span></span>

> <span data-ttu-id="56425-107">Weblogs automáticos de bots Plaster e outros sites com spam, enviando formulários de comentários sem nenhuma interação do usuário.</span><span class="sxs-lookup"><span data-stu-id="56425-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="56425-108">O controle NoBot no ASP.NET AJAX Control Toolkit pode ajudar a combater esses bots.</span><span class="sxs-lookup"><span data-stu-id="56425-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="overview"></a><span data-ttu-id="56425-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="56425-109">Overview</span></span>

<span data-ttu-id="56425-110">Weblogs automáticos de bots Plaster e outros sites com spam, enviando formulários de comentários sem nenhuma interação do usuário.</span><span class="sxs-lookup"><span data-stu-id="56425-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="56425-111">O controle NoBot no ASP.NET AJAX Control Toolkit pode ajudar a combater esses bots.</span><span class="sxs-lookup"><span data-stu-id="56425-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="56425-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="56425-112">Steps</span></span>

<span data-ttu-id="56425-113">Uma abordagem comum para derrotar os bots é usar o teste de Turing público CAPTCHAs completamente automatizado para informar os computadores e os seres humanos.</span><span class="sxs-lookup"><span data-stu-id="56425-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="56425-114">Um teste do Turing era originalmente um teste em que alguém precisava decidir se um parceiro de comunicação é humano ou um computador.</span><span class="sxs-lookup"><span data-stu-id="56425-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="56425-115">Na Web, um CAPTCHA geralmente consiste em uma imagem com algumas letras distorcidas nela.</span><span class="sxs-lookup"><span data-stu-id="56425-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="56425-116">A ideia é que apenas um humano possa ler as letras da imagem, enquanto os algoritmos de OCR falharão.</span><span class="sxs-lookup"><span data-stu-id="56425-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="56425-117">Há várias vantagens e desvantagens nessa abordagem, mas uma discussão disso está além do escopo deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="56425-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="56425-118">No entanto, há um controle no ASP.NET AJAX Control Toolkit que fornece uma abordagem semelhante: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="56425-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="56425-119">É mais fácil superar do que uma CAPTCHA, mas é muito fácil de usar e se torna extremamente bem em sites como Blogs, em que é considerado um sucesso se a maioria das tentativas de spam é derrotada, o que o controle de `NoBot` pode fazer.</span><span class="sxs-lookup"><span data-stu-id="56425-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="56425-120">`NoBot` intercepta o postback do formulário da Web ASP.NET atual se pelo menos uma dessas condições for atendida:</span><span class="sxs-lookup"><span data-stu-id="56425-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="56425-121">O navegador não resolve um quebra-cabeça de JavaScript (por exemplo, quando o JavaScript é desativado)</span><span class="sxs-lookup"><span data-stu-id="56425-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="56425-122">O usuário enviou o formulário para rápido</span><span class="sxs-lookup"><span data-stu-id="56425-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="56425-123">O endereço IP do cliente enviou o formulário com muita frequência em um determinado período de tempo.</span><span class="sxs-lookup"><span data-stu-id="56425-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="56425-124">Para verificar essas condições, o controle de `NoBot` requer esses atributos (todos eles opcionais):</span><span class="sxs-lookup"><span data-stu-id="56425-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="56425-125">`ResponseMinimumDelaySeconds` quantidade mínima de segundos entre postbacks</span><span class="sxs-lookup"><span data-stu-id="56425-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="56425-126">`CutoffWindowSeconds` duração do intervalo de tempo no qual os postbacks de um IP são medidas</span><span class="sxs-lookup"><span data-stu-id="56425-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="56425-127">`CutoffMaximumInstances` quantidade máxima de segundos por intervalo de tempo</span><span class="sxs-lookup"><span data-stu-id="56425-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="56425-128">A marcação a seguir exige que pelo menos dois segundos decorram entre postbacks e que haja apenas cinco postbacks ou menos dentro de um intervalo de 30 segundos:</span><span class="sxs-lookup"><span data-stu-id="56425-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

<span data-ttu-id="56425-129">Em seguida, como sempre, certifique-se de incluir o `ScriptManager` na página para que a biblioteca do ASP.NET AJAX seja carregada e o kit de ferramentas de controle possa ser usado:</span><span class="sxs-lookup"><span data-stu-id="56425-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

<span data-ttu-id="56425-130">Como a maioria das verificações `NoBot` está ocorrendo no lado do servidor, você precisa verificar o resultado dessas validações.</span><span class="sxs-lookup"><span data-stu-id="56425-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="56425-131">Isso pode ser feito chamando o método `IsValid()` do `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="56425-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="56425-132">Ele tem um argumento (como `out` parâmetro/`ByRef` parâmetro) que é do tipo `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="56425-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="56425-133">Sua representação de cadeia de caracteres contém o motivo pelo qual a verificação falha e `Valid` de outra forma.</span><span class="sxs-lookup"><span data-stu-id="56425-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="56425-134">O código a seguir gera uma mensagem de acordo com o resultado de `NoBot`:</span><span class="sxs-lookup"><span data-stu-id="56425-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

<span data-ttu-id="56425-135">Por fim, você precisa de um formulário para enviar e um elemento de rótulo para gerar a mensagem e pronto!</span><span class="sxs-lookup"><span data-stu-id="56425-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

<span data-ttu-id="56425-136">Quando você executar esse script e desativar o JavaScript ou enviar o formulário dentro dos dois primeiros segundos ou enviar o formulário sete vezes em trinta segundos, receberá uma mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="56425-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="56425-137">No entanto, use esse controle de forma inteligente, pois apenas cerca de 90-95% dos usuários têm o JavaScript ativado, portanto, 5-10% dos usuários falharão no teste `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="56425-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>

<span data-ttu-id="56425-138">[![essa mensagem de erro pode ter sido causada por um bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="56425-138">[![This error message could have been caused by a bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span></span>

<span data-ttu-id="56425-139">Essa mensagem de erro pode ter sido causada por um bot ([clique para exibir a imagem em tamanho normal](fighting-bots-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="56425-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="56425-140">Anterior</span><span class="sxs-lookup"><span data-stu-id="56425-140">Previous</span></span>](fighting-bots-cs.md)
