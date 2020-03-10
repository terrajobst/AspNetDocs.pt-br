---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Usando DynamicPopulate com um controle de usuário e JavaScriptC#() | Microsoft Docs
author: wenz
description: O controle DynamicPopulate no kit de ferramentas de controle AJAX ASP.NET chama um serviço Web (ou método de página) e preenche o valor resultante em um controle de destino em t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: a0e6d04a5f62ab558aceb8302d94d3bf2dc8a39f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613704"
---
# <a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a><span data-ttu-id="4849d-103">Uso de DynamicPopulate com um controle de usuário e o JavaScript (C#)</span><span class="sxs-lookup"><span data-stu-id="4849d-103">Using DynamicPopulate with a User Control And JavaScript (C#)</span></span>

<span data-ttu-id="4849d-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4849d-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4849d-105">[Baixar código](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4849d-105">[Download Code](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span></span>

> <span data-ttu-id="4849d-106">O controle DynamicPopulate no kit de ferramentas de controle AJAX ASP.NET chama um serviço Web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página.</span><span class="sxs-lookup"><span data-stu-id="4849d-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="4849d-107">Também é possível disparar a população usando código JavaScript do lado do cliente personalizado.</span><span class="sxs-lookup"><span data-stu-id="4849d-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="4849d-108">No entanto, é preciso tomar cuidado especial quando o extensor reside em um controle de usuário.</span><span class="sxs-lookup"><span data-stu-id="4849d-108">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="overview"></a><span data-ttu-id="4849d-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="4849d-109">Overview</span></span>

<span data-ttu-id="4849d-110">O controle de `DynamicPopulate` no ASP.NET AJAX Control Toolkit chama um serviço Web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página.</span><span class="sxs-lookup"><span data-stu-id="4849d-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="4849d-111">Também é possível disparar a população usando código JavaScript do lado do cliente personalizado.</span><span class="sxs-lookup"><span data-stu-id="4849d-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="4849d-112">No entanto, é preciso tomar cuidado especial quando o extensor reside em um controle de usuário.</span><span class="sxs-lookup"><span data-stu-id="4849d-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="4849d-113">Etapas</span><span class="sxs-lookup"><span data-stu-id="4849d-113">Steps</span></span>

<span data-ttu-id="4849d-114">Em primeiro lugar, você precisa de um serviço Web ASP.NET que implementa o método a ser chamado pelo controle de `DynamicPopulateExtender`.</span><span class="sxs-lookup"><span data-stu-id="4849d-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="4849d-115">O serviço Web implementa o método `getDate()` que espera um argumento do tipo cadeia de caracteres, chamado `contextKey`, pois o controle de `DynamicPopulate` envia uma parte das informações de contexto com cada chamada de serviço da Web.</span><span class="sxs-lookup"><span data-stu-id="4849d-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="4849d-116">Este é o código (`DynamicPopulate.cs.asmx`de arquivo) que recupera a data atual em um dos três formatos:</span><span class="sxs-lookup"><span data-stu-id="4849d-116">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="4849d-117">Na próxima etapa, crie um novo controle de usuário (`.ascx` arquivo), indicado pela seguinte declaração em sua primeira linha:</span><span class="sxs-lookup"><span data-stu-id="4849d-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="4849d-118">Um &lt;`label`elemento &gt; será usado para exibir os dados provenientes do servidor.</span><span class="sxs-lookup"><span data-stu-id="4849d-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

<span data-ttu-id="4849d-119">Também no arquivo de controle de usuário, usaremos três botões de opção, cada um representando um dos três formatos de data possíveis com suporte do serviço Web.</span><span class="sxs-lookup"><span data-stu-id="4849d-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="4849d-120">Quando o usuário clicar em um dos botões de opção, o navegador executará o código JavaScript, que tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="4849d-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

<span data-ttu-id="4849d-121">Esse código acessa o `DynamicPopulateExtender` (não se preocupe com a ID estranha ainda, isso será abordado posteriormente) e acionará a população dinâmica com os dados.</span><span class="sxs-lookup"><span data-stu-id="4849d-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="4849d-122">No contexto do botão de opção atual, `this.value` se refere a seu valor que é `format1`, `format2` ou `format3` exatamente o que o método da Web espera.</span><span class="sxs-lookup"><span data-stu-id="4849d-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="4849d-123">A única coisa ausente no controle de usuário ainda é o controle de `DynamicPopulateExtender` que vincula os botões de opção ao serviço Web.</span><span class="sxs-lookup"><span data-stu-id="4849d-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="4849d-124">Novamente, você pode observar a ID estranha usada no controle: `mcd1$myDate` em vez de `myDate`.</span><span class="sxs-lookup"><span data-stu-id="4849d-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="4849d-125">Anteriormente, o código JavaScript usava `mcd1_dpe1` para acessar o `DynamicPopulateExtender` em vez de `dpe1`. Essa estratégia de nomenclatura é um requisito especial ao usar `DynamicPopulateExtender` dentro de um controle de usuário.</span><span class="sxs-lookup"><span data-stu-id="4849d-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="4849d-126">Além disso, você precisa inserir o controle de usuário de uma maneira específica para fazer tudo funcionar.</span><span class="sxs-lookup"><span data-stu-id="4849d-126">Furthermore, you have to embed the user control in a specific way to make it all work.</span></span> <span data-ttu-id="4849d-127">Crie uma nova página ASP.NET e registre um prefixo de marca para o controle de usuário que você acabou de implementar:</span><span class="sxs-lookup"><span data-stu-id="4849d-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

<span data-ttu-id="4849d-128">Em seguida, inclua o controle `ScriptManager` AJAX ASP.NET na nova página:</span><span class="sxs-lookup"><span data-stu-id="4849d-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

<span data-ttu-id="4849d-129">Por fim, adicione o controle de usuário à página.</span><span class="sxs-lookup"><span data-stu-id="4849d-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="4849d-130">Você só precisa definir seu atributo `ID` (e `runat="server"`, é claro), mas também precisa defini-lo como um nome específico: `mcd1` uma vez que esse é o prefixo usado no controle de usuário para acessá-lo usando JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4849d-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

<span data-ttu-id="4849d-131">E isso é tudo!</span><span class="sxs-lookup"><span data-stu-id="4849d-131">And that's it!</span></span> <span data-ttu-id="4849d-132">A página se comporta conforme o esperado: um usuário clica em um dos botões de opção, o controle no kit de ferramentas chama o serviço Web e exibe a data atual no formato desejado.</span><span class="sxs-lookup"><span data-stu-id="4849d-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>

<span data-ttu-id="4849d-133">[![os botões de opção residem em um controle de usuário](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4849d-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="4849d-134">Os botões de opção residem em um controle de usuário ([clique para exibir a imagem em tamanho normal](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4849d-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4849d-135">[Anterior](dynamically-populating-a-control-using-javascript-code-cs.md)
> [Próximo](dynamically-populating-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4849d-135">[Previous](dynamically-populating-a-control-using-javascript-code-cs.md)
[Next](dynamically-populating-a-control-vb.md)</span></span>
