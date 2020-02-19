---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Modos de exibição dinâmicos versus Exibições com rigidez de tipos | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 3e81c6381b1e280e3b74cb7eb6ea6e6c3224e655
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455631"
---
# <a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="51a3b-103">Modos de exibição dinâmicos versus</span><span class="sxs-lookup"><span data-stu-id="51a3b-103">Dynamic v.</span></span> <span data-ttu-id="51a3b-104">Exibições fortemente tipadas</span><span class="sxs-lookup"><span data-stu-id="51a3b-104">Strongly Typed Views</span></span>

<span data-ttu-id="51a3b-105">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="51a3b-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="51a3b-106">Há três maneiras de passar informações de um controlador para uma exibição no ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="51a3b-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="51a3b-107">Como um objeto de modelo fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="51a3b-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="51a3b-108">Como um tipo dinâmico (usando @model dinâmico)</span><span class="sxs-lookup"><span data-stu-id="51a3b-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="51a3b-109">Usando ViewBag</span><span class="sxs-lookup"><span data-stu-id="51a3b-109">Using the ViewBag</span></span>

<span data-ttu-id="51a3b-110">Escrevi um aplicativo de blog mais simples MVC 3 para comparar e contrastar exibições dinâmicas e com rigidez de tipos.</span><span class="sxs-lookup"><span data-stu-id="51a3b-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="51a3b-111">O controlador começa com uma lista simples de Blogs:</span><span class="sxs-lookup"><span data-stu-id="51a3b-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="51a3b-112">Clique com o botão direito do mouse no método IndexNotStonglyTyped () e adicione uma exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="51a3b-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="51a3b-113">[![8475. NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="51a3b-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="51a3b-114">Verifique se a caixa de **exibição criar um tipo forte** não está marcada.</span><span class="sxs-lookup"><span data-stu-id="51a3b-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="51a3b-115">A exibição resultante não contém muito:</span><span class="sxs-lookup"><span data-stu-id="51a3b-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="51a3b-116">Como estamos usando uma exibição dinâmica e não com rigidez de tipos, o IntelliSense não nos ajuda.</span><span class="sxs-lookup"><span data-stu-id="51a3b-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="51a3b-117">O código completo é mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="51a3b-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="51a3b-118">[![6646. NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="51a3b-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="51a3b-119">Agora, adicionaremos uma exibição com rigidez de tipos.</span><span class="sxs-lookup"><span data-stu-id="51a3b-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="51a3b-120">Adicione o seguinte código ao controlador:</span><span class="sxs-lookup"><span data-stu-id="51a3b-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]

<span data-ttu-id="51a3b-121">Observe que é exatamente a mesma exibição de retorno (topBlogs); Chame como a exibição não fortemente tipada.</span><span class="sxs-lookup"><span data-stu-id="51a3b-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="51a3b-122">Clique com o botão direito do mouse dentro de *StonglyTypedIndex ()* e selecione **Adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="51a3b-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="51a3b-123">Desta vez, selecione a classe de modelo de **blog** e selecione **lista** como o modelo Scaffold.</span><span class="sxs-lookup"><span data-stu-id="51a3b-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="51a3b-124">[![5658. StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="51a3b-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="51a3b-125">Dentro do novo modelo de exibição, obtemos suporte ao IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="51a3b-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="51a3b-126">[![7002. IntelliSense [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="51a3b-126">[![7002.IntelliSense[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="51a3b-127">O projeto c# pode ser baixado [aqui](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="51a3b-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
