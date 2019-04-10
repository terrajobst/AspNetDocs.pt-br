---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Herança de tipo complexo no OData v4 com a API Web ASP.NET | Microsoft Docs
author: microsoft
description: Acordo com a especificação do OData v4, um tipo complexo pode herdar de outro tipo complexo. (Um tipo complexo é um tipo estruturado sem uma chave.) API da Web...
ms.author: riande
ms.date: 09/16/2014
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 76db6325b8528af5b82ca3ea4e34284ca470ff6e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59378583"
---
# <a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="a9476-104">Herança de tipo complexo no OData v4 com a API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a9476-104">Complex Type Inheritance in OData v4 with ASP.NET Web API</span></span>

<span data-ttu-id="a9476-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a9476-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a9476-106">Acordo com o OData v4 [especificação](http://www.odata.org/documentation/odata-version-4-0/), um tipo complexo pode herdar de outro tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="a9476-106">According to the OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), a complex type can inherit from another complex type.</span></span> <span data-ttu-id="a9476-107">(Um *complexos* é um tipo estruturado sem uma chave.) Web API OData 5.3 oferece suporte a herança de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="a9476-107">(A *complex* type is a structured type without a key.) Web API OData 5.3 supports complex type inheritance.</span></span>
> 
> <span data-ttu-id="a9476-108">Este tópico mostra como criar um modelo de dados de entidade (EDM) com tipos complexos de herança.</span><span class="sxs-lookup"><span data-stu-id="a9476-108">This topic shows how to build an entity data model (EDM) with complex inheritance types.</span></span> <span data-ttu-id="a9476-109">Para o código-fonte completo, consulte [exemplo de herança de tipo complexos do OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="a9476-109">For the complete source code, see [OData Complex Type Inheritance Sample](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a9476-110">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="a9476-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a9476-111">Web API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="a9476-111">Web API OData 5.3</span></span>
> - <span data-ttu-id="a9476-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="a9476-112">OData v4</span></span>


## <a name="model-hierarchy"></a><span data-ttu-id="a9476-113">Hierarquia de modelo</span><span class="sxs-lookup"><span data-stu-id="a9476-113">Model Hierarchy</span></span>

<span data-ttu-id="a9476-114">Para ilustrar a herança de tipo complexo, vamos usar a seguinte hierarquia de classe.</span><span class="sxs-lookup"><span data-stu-id="a9476-114">To illustrate complex type inheritance, we'll use the following class hierarchy.</span></span>

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape` <span data-ttu-id="a9476-115">é um tipo complexo de abstrato.</span><span class="sxs-lookup"><span data-stu-id="a9476-115">is an abstract complex type.</span></span> `Rectangle`<span data-ttu-id="a9476-116">, `Triangle`, e `Circle` são tipos complexos derivados `Shape`, e `RoundRectangle` deriva `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="a9476-116">, `Triangle`, and `Circle` are complex types derived from `Shape`, and `RoundRectangle` derives from `Rectangle`.</span></span> `Window` <span data-ttu-id="a9476-117">é um tipo de entidade e contém um `Shape` instância.</span><span class="sxs-lookup"><span data-stu-id="a9476-117">is an entity type and contains a `Shape` instance.</span></span>

<span data-ttu-id="a9476-118">Aqui estão as classes CLR que definem esses tipos.</span><span class="sxs-lookup"><span data-stu-id="a9476-118">Here are the CLR classes that define these types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a><span data-ttu-id="a9476-119">Criar o modelo EDM</span><span class="sxs-lookup"><span data-stu-id="a9476-119">Build the EDM Model</span></span>

<span data-ttu-id="a9476-120">Para criar o EDM, você pode usar **ODataConventionModelBuilder**, qual infere as relações de herança dos tipos de CLR.</span><span class="sxs-lookup"><span data-stu-id="a9476-120">To create the EDM, you can use **ODataConventionModelBuilder**, which infers the inheritance relationships from the CLR types.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="a9476-121">Você também pode compilar o EDM explicitamente, usando **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="a9476-121">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span> <span data-ttu-id="a9476-122">Isso leva mais código, mas oferece mais controle sobre o EDM.</span><span class="sxs-lookup"><span data-stu-id="a9476-122">This takes more code, but gives you more control over the EDM.</span></span>

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="a9476-123">Estes dois exemplos criam o mesmo esquema EDM.</span><span class="sxs-lookup"><span data-stu-id="a9476-123">These two examples create the same EDM schema.</span></span>

## <a name="metadata-document"></a><span data-ttu-id="a9476-124">Documento de metadados</span><span class="sxs-lookup"><span data-stu-id="a9476-124">Metadata Document</span></span>

<span data-ttu-id="a9476-125">Aqui está o documento de metadados OData, que mostra a herança de tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="a9476-125">Here is the OData metadata document, showing complex type inheritance.</span></span>

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

<span data-ttu-id="a9476-126">Do documento de metadados, você pode ver que:</span><span class="sxs-lookup"><span data-stu-id="a9476-126">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="a9476-127">O `Shape` tipo complexo é abstrato.</span><span class="sxs-lookup"><span data-stu-id="a9476-127">The `Shape` complex type is abstract.</span></span>
- <span data-ttu-id="a9476-128">O `Rectangle`, `Triangle`, e `Circle` tipo complexo tem o tipo base `Shape`.</span><span class="sxs-lookup"><span data-stu-id="a9476-128">The `Rectangle`, `Triangle`, and `Circle` complex type have the base type `Shape`.</span></span>
- <span data-ttu-id="a9476-129">O `RoundRectangle` tipo tem o tipo base `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="a9476-129">The `RoundRectangle` type has the base type `Rectangle`.</span></span>

## <a name="casting-complex-types"></a><span data-ttu-id="a9476-130">Conversão de tipos complexos</span><span class="sxs-lookup"><span data-stu-id="a9476-130">Casting Complex Types</span></span>

<span data-ttu-id="a9476-131">Agora há suporte para a conversão em tipos complexos.</span><span class="sxs-lookup"><span data-stu-id="a9476-131">Casting on complex types is now supported.</span></span> <span data-ttu-id="a9476-132">Por exemplo, a consulta a seguir converte um `Shape` para um `Rectangle`.</span><span class="sxs-lookup"><span data-stu-id="a9476-132">For example, the following query casts a `Shape` to a `Rectangle`.</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

<span data-ttu-id="a9476-133">Aqui está a carga de resposta:</span><span class="sxs-lookup"><span data-stu-id="a9476-133">Here's the response payload:</span></span>

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
