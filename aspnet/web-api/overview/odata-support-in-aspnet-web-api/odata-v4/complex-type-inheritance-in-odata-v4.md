---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Herança de tipo complexo no OData V4 com ASP.NET Web API | Microsoft Docs
author: microsoft
description: De acordo com a especificação do OData v4, um tipo complexo pode herdar de outro tipo complexo. (Um tipo complexo é um tipo estruturado sem uma chave.) API da Web...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 3d90216c8e594055f77577eb6d8b1d978ae4c24d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556304"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="1e51b-104">Herança de tipo complexo no OData V4 com ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="1e51b-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="1e51b-105">pela [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1e51b-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="1e51b-106">De acordo com a [especificação](http://www.odata.org/documentation/odata-version-4-0/)do OData v4, um tipo complexo pode herdar de outro tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="1e51b-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="1e51b-107">(Um tipo *complexo* é um tipo estruturado sem uma chave.) A API Web OData 5,3 dá suporte à herança de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="1e51b-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="1e51b-108">Este tópico mostra como criar um EDM (modelo de dados de entidade) com tipos de herança complexos.</span><span class="sxs-lookup"><span data-stu-id="1e51b-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="1e51b-109">Para obter o código-fonte completo, consulte [exemplo de herança de tipo complexo do OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="1e51b-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1e51b-110">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="1e51b-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="1e51b-111">API Web OData 5,3</span><span class="sxs-lookup"><span data-stu-id="1e51b-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="1e51b-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="1e51b-112">OData v4</span></span>

## <a name="model-hierarchy"></a><span data-ttu-id="1e51b-113">Hierarquia de modelo</span><span class="sxs-lookup"><span data-stu-id="1e51b-113">Model Hierarchy</span></span>

<span data-ttu-id="1e51b-114">Para ilustrar a herança de tipo complexo, usaremos a seguinte hierarquia de classe.</span><span class="sxs-lookup"><span data-stu-id="1e51b-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

<span data-ttu-id="1e51b-115">`Shape` é um tipo complexo abstrato.</span><span class="sxs-lookup"><span data-stu-id="1e51b-115">`Shape` is an abstract complex type.</span></span> <span data-ttu-id="1e51b-116">`Rectangle`, `Triangle`e `Circle` são tipos complexos derivados de `Shape`e `RoundRectangle` deriva de `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="1e51b-116">`Rectangle`, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> <span data-ttu-id="1e51b-117">`Window` é um tipo de entidade e contém uma instância de `Shape`.</span><span class="sxs-lookup"><span data-stu-id="1e51b-117">`Window` is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="1e51b-118">Aqui estão as classes CLR que definem esses tipos.</span><span class="sxs-lookup"><span data-stu-id="1e51b-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="1e51b-119">Compilar o modelo EDM</span><span class="sxs-lookup"><span data-stu-id="1e51b-119">Build the EDM Model</span></span>

<span data-ttu-id="1e51b-120">Para criar o EDM, você pode usar **ODataConventionModelBuilder**, que infere as relações de herança dos tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="1e51b-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="1e51b-121">Você também pode criar o EDM explicitamente, usando o **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="1e51b-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="1e51b-122">Isso usa mais código, mas lhe dá mais controle sobre o EDM.</span><span class="sxs-lookup"><span data-stu-id="1e51b-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="1e51b-123">Esses dois exemplos criam o mesmo esquema EDM.</span><span class="sxs-lookup"><span data-stu-id="1e51b-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="1e51b-124">Documento de metadados</span><span class="sxs-lookup"><span data-stu-id="1e51b-124">Metadata Document</span></span>

<span data-ttu-id="1e51b-125">Este é o documento de metadados OData, mostrando herança de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="1e51b-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="1e51b-126">No documento de metadados, você pode ver que:</span><span class="sxs-lookup"><span data-stu-id="1e51b-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="1e51b-127">O tipo complexo `Shape` é abstract.</span><span class="sxs-lookup"><span data-stu-id="1e51b-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="1e51b-128">O `Rectangle`, `Triangle`e `Circle` tipo complexo têm o tipo base `Shape`.</span><span class="sxs-lookup"><span data-stu-id="1e51b-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="1e51b-129">O tipo de `RoundRectangle` tem o tipo base `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="1e51b-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="1e51b-130">Conversão de tipos complexos</span><span class="sxs-lookup"><span data-stu-id="1e51b-130">Casting Complex Types</span></span>

<span data-ttu-id="1e51b-131">Agora há suporte para a conversão em tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="1e51b-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="1e51b-132">Por exemplo, a consulta a seguir converte um `Shape` em um `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="1e51b-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="1e51b-133">Aqui está a carga de resposta:</span><span class="sxs-lookup"><span data-stu-id="1e51b-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
