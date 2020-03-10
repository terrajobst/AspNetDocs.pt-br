---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Contenção no OData v4 usando a API Web 2,2 | Microsoft Docs
author: rick-anderson
description: Tradicionalmente, uma entidade só poderia ser acessada se fosse encapsulada dentro de um conjunto de entidades. Mas o OData v4 fornece duas opções adicionais, singleton e con...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 50050e40c4c42bf6d769d077c27864ee6417d4db
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525119"
---
# <a name="containment-in-odata-v4-using-web-api-22"></a><span data-ttu-id="23c47-104">Contenção no OData v4 usando a API Web 2,2</span><span class="sxs-lookup"><span data-stu-id="23c47-104">Containment in OData v4 Using Web API 2.2</span></span>

<span data-ttu-id="23c47-105">por JinFu Tan</span><span class="sxs-lookup"><span data-stu-id="23c47-105">by Jinfu Tan</span></span>

> <span data-ttu-id="23c47-106">Tradicionalmente, uma entidade só poderia ser acessada se fosse encapsulada dentro de um conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="23c47-106">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="23c47-107">Mas o OData v4 fornece duas opções adicionais, singleton e confinamento, que são compatíveis com o WebAPI 2,2.</span><span class="sxs-lookup"><span data-stu-id="23c47-107">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>

<span data-ttu-id="23c47-108">Este tópico mostra como definir uma contenção em um ponto de extremidade OData no WebApi 2,2.</span><span class="sxs-lookup"><span data-stu-id="23c47-108">This topic shows how to define a containment in an OData endpoint in WebApi 2.2.</span></span> <span data-ttu-id="23c47-109">Para obter mais informações sobre contenção, consulte [a contenção é proveniente do OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span><span class="sxs-lookup"><span data-stu-id="23c47-109">For more information about containment, see [Containment is coming with OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx).</span></span> <span data-ttu-id="23c47-110">Para criar um ponto de extremidade do OData v4 na API Web, consulte [criar um ponto de extremidade do OData v4 usando ASP.NET Web API 2,2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="23c47-110">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span>

<span data-ttu-id="23c47-111">Primeiro, criaremos um modelo de domínio de confinamento no serviço OData, usando este modelo de dados:</span><span class="sxs-lookup"><span data-stu-id="23c47-111">First, we'll create a containment domain model in the OData service, using this data model:</span></span>

![Modelo de dados](odata-containment-in-web-api-22/_static/image1.png)

<span data-ttu-id="23c47-113">Uma conta contém muitos PaymentInstruments (PI), mas não definimos um conjunto de entidades para um PI.</span><span class="sxs-lookup"><span data-stu-id="23c47-113">An account contains many PaymentInstruments (PI), but we don't define an entity set for a PI.</span></span> <span data-ttu-id="23c47-114">Em vez disso, o PIs só pode ser acessado por meio de uma conta.</span><span class="sxs-lookup"><span data-stu-id="23c47-114">Instead, the PIs can only be accessed through an Account.</span></span>

<span data-ttu-id="23c47-115">Você pode baixar a solução usada neste tópico do [codeplex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span><span class="sxs-lookup"><span data-stu-id="23c47-115">You can download the solution used in this topic from [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).</span></span>

## <a name="defining-the-data-model"></a><span data-ttu-id="23c47-116">Definindo o modelo de dados</span><span class="sxs-lookup"><span data-stu-id="23c47-116">Defining the data model</span></span>

1. <span data-ttu-id="23c47-117">Defina os tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="23c47-117">Define the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    <span data-ttu-id="23c47-118">O atributo `Contained` é usado para propriedades de navegação de confinamento.</span><span class="sxs-lookup"><span data-stu-id="23c47-118">The `Contained` attribute is used for containment navigation properties.</span></span>
2. <span data-ttu-id="23c47-119">Gere o modelo EDM com base nos tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="23c47-119">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="23c47-120">O `ODataConventionModelBuilder` tratará da criação do modelo EDM se o atributo `Contained` for adicionado à propriedade de navegação correspondente.</span><span class="sxs-lookup"><span data-stu-id="23c47-120">The `ODataConventionModelBuilder` will handle building the EDM model if the `Contained` attribute is added to the corresponding navigation property.</span></span> <span data-ttu-id="23c47-121">Se a propriedade for um tipo de coleção, uma função `GetCount(string NameContains)` também será criada.</span><span class="sxs-lookup"><span data-stu-id="23c47-121">If the property is a collection type, a `GetCount(string NameContains)` function will also be created.</span></span>

    <span data-ttu-id="23c47-122">Os metadados gerados serão semelhantes ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="23c47-122">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    <span data-ttu-id="23c47-123">O atributo `ContainsTarget` indica que a propriedade de navegação é uma contenção.</span><span class="sxs-lookup"><span data-stu-id="23c47-123">The `ContainsTarget` attribute indicates that the navigation property is a containment.</span></span>

## <a name="define-the-containing-entity-set-controller"></a><span data-ttu-id="23c47-124">Definir o controlador de conjunto de entidades de contenção</span><span class="sxs-lookup"><span data-stu-id="23c47-124">Define the containing entity set controller</span></span>

<span data-ttu-id="23c47-125">Entidades contidas não têm seu próprio controlador; a ação é definida no controlador de conjunto de entidades que a contém.</span><span class="sxs-lookup"><span data-stu-id="23c47-125">Contained entities don't have their own controller; the action is defined in the containing entity set controller.</span></span> <span data-ttu-id="23c47-126">Neste exemplo, há um AccountsController, mas nenhum PaymentInstrumentsController.</span><span class="sxs-lookup"><span data-stu-id="23c47-126">In this sample, there is an AccountsController, but no PaymentInstrumentsController.</span></span>

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

<span data-ttu-id="23c47-127">Se o caminho OData for de 4 ou mais segmentos, somente o roteamento de atributos funcionará, como `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` no controlador acima.</span><span class="sxs-lookup"><span data-stu-id="23c47-127">If the OData path is 4 or more segments, only attribute routing works, such as `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` in the above controller.</span></span> <span data-ttu-id="23c47-128">Caso contrário, o atributo e o roteamento convencional funcionarão: por exemplo, `GetPayInPIs(int key)` corresponde a `GET ~/Accounts(1)/PayinPIs`.</span><span class="sxs-lookup"><span data-stu-id="23c47-128">Otherwise, both attribute and conventional routing works: for instance, `GetPayInPIs(int key)` matches `GET ~/Accounts(1)/PayinPIs`.</span></span>

<span data-ttu-id="23c47-129">*Agradecemos ao Leo Hu pelo conteúdo original deste artigo.*</span><span class="sxs-lookup"><span data-stu-id="23c47-129">*Thanks to Leo Hu for the original content of this article.*</span></span>
