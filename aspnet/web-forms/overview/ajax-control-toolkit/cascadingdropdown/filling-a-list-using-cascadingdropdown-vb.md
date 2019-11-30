---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Preenchendo uma lista usando CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 8dd9ef8a4bdf705ba4451b7fd240e4de8618221c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599589"
---
# <a name="filling-a-list-using-cascadingdropdown-vb"></a><span data-ttu-id="8475b-103">Preencher uma lista usando o CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="8475b-103">Filling a List Using CascadingDropDown (VB)</span></span>

<span data-ttu-id="8475b-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8475b-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8475b-105">[Baixar código](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8475b-105">[Download Code](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span></span>

> <span data-ttu-id="8475b-106">O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em outra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="8475b-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="8475b-107">(Por exemplo, uma lista fornece uma lista de Estados dos EUA e a próxima lista é então preenchida com cidades principais nesse estado.) O primeiro desafio a ser resolvido é realmente preencher uma lista suspensa usando esse controle.</span><span class="sxs-lookup"><span data-stu-id="8475b-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="overview"></a><span data-ttu-id="8475b-108">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="8475b-108">Overview</span></span>

<span data-ttu-id="8475b-109">O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em outra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="8475b-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="8475b-110">(Por exemplo, uma lista fornece uma lista de Estados dos EUA e a próxima lista é então preenchida com cidades principais nesse estado.) O primeiro desafio a ser resolvido é realmente preencher uma lista suspensa usando esse controle.</span><span class="sxs-lookup"><span data-stu-id="8475b-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="8475b-111">Etapas</span><span class="sxs-lookup"><span data-stu-id="8475b-111">Steps</span></span>

<span data-ttu-id="8475b-112">Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado em qualquer lugar na página (mas dentro do elemento `<form>`):</span><span class="sxs-lookup"><span data-stu-id="8475b-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="8475b-113">Em seguida, um controle DropDownList é necessário:</span><span class="sxs-lookup"><span data-stu-id="8475b-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="8475b-114">Para essa lista, um extensor CascadingDropDown é adicionado.</span><span class="sxs-lookup"><span data-stu-id="8475b-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="8475b-115">Ele enviará uma solicitação assíncrona para um serviço Web que retornará uma lista de entradas a serem exibidas na lista.</span><span class="sxs-lookup"><span data-stu-id="8475b-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="8475b-116">Para que isso funcione, os seguintes atributos CascadingDropDown precisam ser definidos:</span><span class="sxs-lookup"><span data-stu-id="8475b-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="8475b-117">`ServicePath`: URL de um serviço Web que fornece as entradas da lista</span><span class="sxs-lookup"><span data-stu-id="8475b-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="8475b-118">`ServiceMethod`: método Web fornecendo as entradas da lista</span><span class="sxs-lookup"><span data-stu-id="8475b-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="8475b-119">`TargetControlID`: ID da lista suspensa</span><span class="sxs-lookup"><span data-stu-id="8475b-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="8475b-120">`Category`: informações de categoria que são enviadas para o método Web quando chamado</span><span class="sxs-lookup"><span data-stu-id="8475b-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="8475b-121">`PromptText`: texto exibido ao carregar dados da lista de forma assíncrona do servidor</span><span class="sxs-lookup"><span data-stu-id="8475b-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="8475b-122">Aqui está a marcação para o elemento `CascadingDropDown`.</span><span class="sxs-lookup"><span data-stu-id="8475b-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="8475b-123">A única diferença entre C# o e o VB é o nome do serviço Web associado:</span><span class="sxs-lookup"><span data-stu-id="8475b-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="8475b-124">O código JavaScript proveniente do `CascadingDropDown` Extender chama um método de serviço Web com a seguinte assinatura:</span><span class="sxs-lookup"><span data-stu-id="8475b-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="8475b-125">Portanto, o aspecto importante é que o método precisa retornar uma matriz do tipo `CascadingDropDownNameValue` (definida pelo ASP.NET AJAX Control Toolkit).</span><span class="sxs-lookup"><span data-stu-id="8475b-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="8475b-126">No Construtor `CascadingDropDownNameValue`, primeiro o texto da entrada da lista e, em seguida, seu valor deve ser fornecido, assim como `<option value="VALUE">NAME</option>` faria no HTML.</span><span class="sxs-lookup"><span data-stu-id="8475b-126">In the `CascadingDropDownNameValue` constructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="8475b-127">Aqui estão alguns dados de exemplo:</span><span class="sxs-lookup"><span data-stu-id="8475b-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="8475b-128">Carregar a página no navegador irá disparar a lista a ser preenchida com três fornecedores.</span><span class="sxs-lookup"><span data-stu-id="8475b-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>

<span data-ttu-id="8475b-129">[![a lista é preenchida automaticamente](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8475b-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="8475b-130">A lista é preenchida automaticamente ([clique para exibir a imagem em tamanho normal](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8475b-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8475b-131">[Anterior](using-auto-postback-with-cascadingdropdown-cs.md)
> [Próximo](using-cascadingdropdown-with-a-database-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8475b-131">[Previous](using-auto-postback-with-cascadingdropdown-cs.md)
[Next](using-cascadingdropdown-with-a-database-vb.md)</span></span>
