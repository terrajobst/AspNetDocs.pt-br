---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Preenchendo uma lista usando o CascadingDropDown (c#) | Microsoft Docs
author: wenz
description: O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associado valores em anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: a9a3bf12b721c8f5eec21f3090142e40e74b0b9c
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59395635"
---
# <a name="filling-a-list-using-cascadingdropdown-c"></a><span data-ttu-id="a6fd6-103">Preencher uma lista usando o CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="a6fd6-103">Filling a List Using CascadingDropDown (C#)</span></span>

<span data-ttu-id="a6fd6-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a6fd6-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a6fd6-105">[Baixar o código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a6fd6-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span></span>

> <span data-ttu-id="a6fd6-106">O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associadas a valores em outra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="a6fd6-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="a6fd6-107">(Por exemplo, uma lista fornece uma lista de estados dos EUA e a lista seguinte, em seguida, é preenchida com principais cidades nesse estado.) O primeiro desafio para resolver é, na verdade, preencher uma lista suspensa usando esse controle.</span><span class="sxs-lookup"><span data-stu-id="a6fd6-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="a6fd6-108">Visão geral</span><span class="sxs-lookup"><span data-stu-id="a6fd6-108">Overview</span></span>

<span data-ttu-id="a6fd6-109">O controle CascadingDropDown do AJAX Control Toolkit estende um controle DropDownList, de modo que as alterações em uma carga de DropDownList associadas a valores em outra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="a6fd6-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="a6fd6-110">(Por exemplo, uma lista fornece uma lista de estados dos EUA e a lista seguinte, em seguida, é preenchida com principais cidades nesse estado.) O primeiro desafio para resolver é, na verdade, preencher uma lista suspensa usando esse controle.</span><span class="sxs-lookup"><span data-stu-id="a6fd6-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="a6fd6-111">Etapas</span><span class="sxs-lookup"><span data-stu-id="a6fd6-111">Steps</span></span>

<span data-ttu-id="a6fd6-112">Para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada em qualquer lugar na página (mas dentro de `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="a6fd6-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="a6fd6-113">Em seguida, um controle DropDownList é necessário:</span><span class="sxs-lookup"><span data-stu-id="a6fd6-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="a6fd6-114">Para obter essa lista, um extensor de CascadingDropDown é adicionado.</span><span class="sxs-lookup"><span data-stu-id="a6fd6-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="a6fd6-115">Ele envia uma solicitação assíncrona para um serviço web que, em seguida, retornará uma lista de entradas a ser exibido na lista.</span><span class="sxs-lookup"><span data-stu-id="a6fd6-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="a6fd6-116">Para que isso funcione, os seguintes atributos de CascadingDropDown precisam ser definidas:</span><span class="sxs-lookup"><span data-stu-id="a6fd6-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="a6fd6-117">`ServicePath`: URL de um serviço web fornecendo as entradas da lista</span><span class="sxs-lookup"><span data-stu-id="a6fd6-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="a6fd6-118">`ServiceMethod`: Método Web fornecendo as entradas da lista</span><span class="sxs-lookup"><span data-stu-id="a6fd6-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="a6fd6-119">`TargetControlID`: ID da lista suspensa</span><span class="sxs-lookup"><span data-stu-id="a6fd6-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="a6fd6-120">`Category`: Informações de categoria que são enviadas para o método da web quando chamado</span><span class="sxs-lookup"><span data-stu-id="a6fd6-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="a6fd6-121">`PromptText`: Texto exibido quando assincronamente Carregando dados da lista do servidor</span><span class="sxs-lookup"><span data-stu-id="a6fd6-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="a6fd6-122">Aqui está a marcação para o `CascadingDropDown` elemento.</span><span class="sxs-lookup"><span data-stu-id="a6fd6-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="a6fd6-123">A única diferença entre c# e VB é o nome do serviço web associado:</span><span class="sxs-lookup"><span data-stu-id="a6fd6-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="a6fd6-124">O código JavaScript provenientes de `CascadingDropDown` extensor chama um método de serviço web com a seguinte assinatura:</span><span class="sxs-lookup"><span data-stu-id="a6fd6-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="a6fd6-125">Portanto, o aspecto importante é que o método deve retornar uma matriz do tipo `CascadingDropDownNameValue` (definido pelo ASP.NET AJAX Control Toolkit).</span><span class="sxs-lookup"><span data-stu-id="a6fd6-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="a6fd6-126">No `CascadingDropDownNameValue` construtor, primeiro texto da entrada de lista e, em seguida, seu valor devem ser fornecidas, assim como `<option value="VALUE">NAME</option>` faria em HTML.</span><span class="sxs-lookup"><span data-stu-id="a6fd6-126">In the `CascadingDropDownNameValue` constructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="a6fd6-127">Aqui estão alguns dados de exemplo:</span><span class="sxs-lookup"><span data-stu-id="a6fd6-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="a6fd6-128">Carregamento da página no navegador disparará a lista a ser preenchido com três fornecedores.</span><span class="sxs-lookup"><span data-stu-id="a6fd6-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="a6fd6-129">[![A lista é preenchida automaticamente](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a6fd6-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="a6fd6-130">A lista é preenchida automaticamente ([clique para exibir a imagem em tamanho normal](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a6fd6-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a6fd6-131">Avançar</span><span class="sxs-lookup"><span data-stu-id="a6fd6-131">Next</span></span>](using-cascadingdropdown-with-a-database-cs.md)
