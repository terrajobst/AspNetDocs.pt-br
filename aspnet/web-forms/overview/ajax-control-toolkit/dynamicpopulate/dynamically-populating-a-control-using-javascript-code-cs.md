---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Populando dinamicamente um controle usando códigoC#JavaScript () | Microsoft Docs
author: wenz
description: O controle DynamicPopulate no kit de ferramentas de controle AJAX ASP.NET chama um serviço Web (ou método de página) e preenche o valor resultante em um controle de destino em t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 24dc358427dec3ffcba16d00041c9a2db657e7e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535738"
---
# <a name="dynamically-populating-a-control-using-javascript-code-c"></a><span data-ttu-id="57a41-103">Preenchimento dinâmico de um controle com código de JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="57a41-103">Dynamically Populating a Control Using JavaScript Code (C#)</span></span>

<span data-ttu-id="57a41-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="57a41-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="57a41-105">[Baixar código](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="57a41-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)</span></span>

> <span data-ttu-id="57a41-106">O controle DynamicPopulate no kit de ferramentas de controle AJAX ASP.NET chama um serviço Web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página.</span><span class="sxs-lookup"><span data-stu-id="57a41-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="57a41-107">Também é possível disparar a população usando código JavaScript do lado do cliente personalizado.</span><span class="sxs-lookup"><span data-stu-id="57a41-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="57a41-108">Visão geral</span><span class="sxs-lookup"><span data-stu-id="57a41-108">Overview</span></span>

<span data-ttu-id="57a41-109">O controle de `DynamicPopulate` no ASP.NET AJAX Control Toolkit chama um serviço Web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página.</span><span class="sxs-lookup"><span data-stu-id="57a41-109">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="57a41-110">Também é possível disparar a população usando código JavaScript do lado do cliente personalizado.</span><span class="sxs-lookup"><span data-stu-id="57a41-110">It is also possible to trigger the population using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="57a41-111">Etapas</span><span class="sxs-lookup"><span data-stu-id="57a41-111">Steps</span></span>

<span data-ttu-id="57a41-112">Em primeiro lugar, você precisa de um serviço Web ASP.NET que implementa o método a ser chamado pelo controle de `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="57a41-112">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="57a41-113">O serviço Web implementa o método `getDate()` que espera um argumento do tipo cadeia de caracteres, chamado `contextKey`, pois o controle de `DynamicPopulate` envia uma parte das informações de contexto com cada chamada de serviço da Web.</span><span class="sxs-lookup"><span data-stu-id="57a41-113">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="57a41-114">Este é o código (`DynamicPopulate.cs.asmx`de arquivo) que recupera a data atual em um dos três formatos:</span><span class="sxs-lookup"><span data-stu-id="57a41-114">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

<span data-ttu-id="57a41-115">Na próxima etapa, crie um novo site do ASP.NET e comece com o controle do ScriptManager do ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="57a41-115">In the next step, create a new ASP.NET site and start with the ASP.NET AJAX ScriptManager control:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

<span data-ttu-id="57a41-116">Em seguida, adicione um controle rótulo (por exemplo, usando o controle HTML de mesmo nome ou o `<asp:Label />` controle da Web), que mostrará posteriormente o resultado da chamada do serviço Web.</span><span class="sxs-lookup"><span data-stu-id="57a41-116">Then, add a label control (for instance using the HTML control of the same name, or the `<asp:Label />` web control) which will later show the result of the web service call.</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

<span data-ttu-id="57a41-117">Em seguida, inclua um controle de `DynamicPopulateExtender` e forneça informações de serviço Web, controle de destino, mas não o nome do controle que dispara a população que isso será feito posteriormente, usando JavaScript personalizado!</span><span class="sxs-lookup"><span data-stu-id="57a41-117">Next, include a `DynamicPopulateExtender` control and provide web service information, target control, but not the name of the control which triggers the population this will be done later on, using custom JavaScript!</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

<span data-ttu-id="57a41-118">Agora, para a parte JavaScript.</span><span class="sxs-lookup"><span data-stu-id="57a41-118">Now to the JavaScript part.</span></span> <span data-ttu-id="57a41-119">A função `$find()`, definida pela biblioteca ASP.NET AJAX, retorna uma referência a objetos do lado do servidor do ASP.NET AJAX Control Toolkit, como `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="57a41-119">The `$find()` function, defined by the ASP.NET AJAX library, returns a reference to server-side objects of the ASP.NET AJAX Control Toolkit such as `DynamicPopulateExtender`.</span></span> <span data-ttu-id="57a41-120">No arquivo atual, `$find("dpe")` retorna uma referência ao controle de um `DynamicPopulateExtender` na página.</span><span class="sxs-lookup"><span data-stu-id="57a41-120">In the current file, `$find("dpe")` returns a reference to the one `DynamicPopulateExtender` control in the page.</span></span> <span data-ttu-id="57a41-121">Ele expõe um método chamado `populate()` que dispara o processo de população dinâmica.</span><span class="sxs-lookup"><span data-stu-id="57a41-121">It exposes a method called `populate()` which triggers the dynamic population process.</span></span> <span data-ttu-id="57a41-122">O método `populate()` requer um argumento: a chave de contexto que servirá como argumento para o método Web `getDate()`.</span><span class="sxs-lookup"><span data-stu-id="57a41-122">The `populate()` method requires one argument: the context key which will serve as argument to the `getDate()` web method.</span></span> <span data-ttu-id="57a41-123">Por exemplo, `$find("dpe").populate("format1")` preencheria o rótulo com a data atual no formato mês-dia-ano.</span><span class="sxs-lookup"><span data-stu-id="57a41-123">So for instance, `$find("dpe").populate("format1")` would populate the label with the current date in month-day-year format.</span></span>

<span data-ttu-id="57a41-124">Para tornar o exemplo um pouco mais flexível, o usuário pode agora escolher entre vários formatos de data.</span><span class="sxs-lookup"><span data-stu-id="57a41-124">In order to make the sample a bit more flexible, the user may now choose between several date formats.</span></span> <span data-ttu-id="57a41-125">Para cada um deles, um botão de opção é exibido.</span><span class="sxs-lookup"><span data-stu-id="57a41-125">For each one of them, a radio button is displayed.</span></span> <span data-ttu-id="57a41-126">Quando o usuário clica em um botão de opção, o código JavaScript preenche dinamicamente o rótulo com o formato de data selecionado.</span><span class="sxs-lookup"><span data-stu-id="57a41-126">Once the user clicks on a radio button, JavaScript code dynamically populates the label with the selected date format.</span></span> <span data-ttu-id="57a41-127">Estes são os botões de opção:</span><span class="sxs-lookup"><span data-stu-id="57a41-127">Here are those radio buttons:</span></span>

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

<span data-ttu-id="57a41-128">Observe que, dentro do contexto de um botão de opção, a expressão JavaScript `this.value` se refere ao valor do botão atual, que é exatamente a mesma informação com a qual o método `getDate()` pode trabalhar.</span><span class="sxs-lookup"><span data-stu-id="57a41-128">Note that within the context of a radio button, the JavaScript expression `this.value` refers to the value of the current button, which happens to be exactly the same information the `getDate()` method can work with.</span></span>

<span data-ttu-id="57a41-129">[![um clique no botão recupera a data do servidor, no formato especificado](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="57a41-129">[![A click on the button retrieves the date from the server, in the format specified](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="57a41-130">Um clique no botão recupera a data do servidor, no formato especificado ([clique para exibir a imagem em tamanho normal](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="57a41-130">A click on the button retrieves the date from the server, in the format specified ([Click to view full-size image](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="57a41-131">[Anterior](dynamically-populating-a-control-cs.md)
> [Próximo](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span><span class="sxs-lookup"><span data-stu-id="57a41-131">[Previous](dynamically-populating-a-control-cs.md)
[Next](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)</span></span>
