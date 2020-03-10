---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Criar um singleton no OData v4 usando a API Web 2,2 | Microsoft Docs
author: rick-anderson
description: Este tópico mostra como definir um singleton em um ponto de extremidade OData na API Web 2,2.
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 218449c18759b306e425c55f8e7b573d837b4658
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622083"
---
# <a name="create-a-singleton-in-odata-v4-using-web-api-22"></a><span data-ttu-id="32c5d-103">Criar um singleton no OData v4 usando a API Web 2,2</span><span class="sxs-lookup"><span data-stu-id="32c5d-103">Create a Singleton in OData v4 Using Web API 2.2</span></span>

<span data-ttu-id="32c5d-104">por Zoe Luo</span><span class="sxs-lookup"><span data-stu-id="32c5d-104">by Zoe Luo</span></span>

> <span data-ttu-id="32c5d-105">Tradicionalmente, uma entidade só poderia ser acessada se fosse encapsulada dentro de um conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="32c5d-105">Traditionally, an entity could only be accessed if it were encapsulated inside an entity set.</span></span> <span data-ttu-id="32c5d-106">Mas o OData v4 fornece duas opções adicionais, singleton e confinamento, que são compatíveis com o WebAPI 2,2.</span><span class="sxs-lookup"><span data-stu-id="32c5d-106">But OData v4 provides two additional options, Singleton and Containment, both of which WebAPI 2.2 supports.</span></span>

<span data-ttu-id="32c5d-107">Este artigo mostra como definir um singleton em um ponto de extremidade OData na API Web 2,2.</span><span class="sxs-lookup"><span data-stu-id="32c5d-107">This article shows how to define a singleton in an OData endpoint in Web API 2.2.</span></span> <span data-ttu-id="32c5d-108">Para obter informações sobre o que é um singleton e como você pode se beneficiar com o uso dele, consulte [usando um singleton para definir sua entidade especial](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span><span class="sxs-lookup"><span data-stu-id="32c5d-108">For information on what a singleton is and how you can benefit from using it, see [Using a singleton to define your special entity](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx).</span></span> <span data-ttu-id="32c5d-109">Para criar um ponto de extremidade do OData v4 na API Web, consulte [criar um ponto de extremidade do OData v4 usando ASP.NET Web API 2,2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="32c5d-109">To create an OData V4 endpoint in Web API, see [Create an OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).</span></span> 

<span data-ttu-id="32c5d-110">Criaremos um singleton em seu projeto de API Web usando o seguinte modelo de dados:</span><span class="sxs-lookup"><span data-stu-id="32c5d-110">We'll create a singleton in your Web API project using the following data model:</span></span>

![Modelo de dados](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

<span data-ttu-id="32c5d-112">Um singleton chamado `Umbrella` será definido com base no tipo `Company`e um conjunto de entidades chamado `Employees` será definido com base no tipo `Employee`.</span><span class="sxs-lookup"><span data-stu-id="32c5d-112">A singleton named `Umbrella` will be defined based on type `Company`, and an entity set named `Employees` will be defined based on type `Employee`.</span></span>

<span data-ttu-id="32c5d-113">A solução usada neste tutorial pode ser baixada do [codeplex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span><span class="sxs-lookup"><span data-stu-id="32c5d-113">The solution used in this tutorial can be downloaded from [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="32c5d-114">Definir o modelo de dados</span><span class="sxs-lookup"><span data-stu-id="32c5d-114">Define the data model</span></span>

1. <span data-ttu-id="32c5d-115">Defina os tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="32c5d-115">Define the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. <span data-ttu-id="32c5d-116">Gere o modelo EDM com base nos tipos CLR.</span><span class="sxs-lookup"><span data-stu-id="32c5d-116">Generate the EDM model based on the CLR types.</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    <span data-ttu-id="32c5d-117">Aqui, `builder.Singleton<Company>("Umbrella")` diz ao construtor de modelos para criar um singleton nomeado `Umbrella` no modelo EDM.</span><span class="sxs-lookup"><span data-stu-id="32c5d-117">Here, `builder.Singleton<Company>("Umbrella")` tells the model builder to create a singleton named `Umbrella` in the EDM model.</span></span>

    <span data-ttu-id="32c5d-118">Os metadados gerados serão semelhantes ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="32c5d-118">The generated metadata will look like the following:</span></span>

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    <span data-ttu-id="32c5d-119">Dos metadados, podemos ver que a propriedade de navegação `Company` no conjunto de entidades de `Employees` está associada à `Umbrella`singleton.</span><span class="sxs-lookup"><span data-stu-id="32c5d-119">From the metadata we can see that the navigation property `Company` in the `Employees` entity set is bound to the singleton `Umbrella`.</span></span> <span data-ttu-id="32c5d-120">A associação é feita automaticamente pelo `ODataConventionModelBuilder`, já que somente `Umbrella` tem o tipo de `Company`.</span><span class="sxs-lookup"><span data-stu-id="32c5d-120">The binding is done automatically by `ODataConventionModelBuilder`, since only `Umbrella` has the `Company` type.</span></span> <span data-ttu-id="32c5d-121">Se houver qualquer ambiguidade no modelo, você poderá usar `HasSingletonBinding` para associar explicitamente uma propriedade de navegação a um singleton; `HasSingletonBinding` tem o mesmo efeito que usar o atributo `Singleton` na definição de tipo CLR:</span><span class="sxs-lookup"><span data-stu-id="32c5d-121">If there is any ambiguity in the model, you can use `HasSingletonBinding` to explicitly bind a navigation property to a singleton; `HasSingletonBinding` has the same effect as using the `Singleton` attribute in the CLR type definition:</span></span>

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a><span data-ttu-id="32c5d-122">Definir o controlador singleton</span><span class="sxs-lookup"><span data-stu-id="32c5d-122">Define the singleton controller</span></span>

<span data-ttu-id="32c5d-123">Assim como o controlador de EntitySet, o controlador singleton é herdado de `ODataController`e o nome do controlador singleton deve ser `[singletonName]Controller`.</span><span class="sxs-lookup"><span data-stu-id="32c5d-123">Like the EntitySet controller, the singleton controller inherits from `ODataController`, and the singleton controller name should be `[singletonName]Controller`.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

<span data-ttu-id="32c5d-124">Para lidar com diferentes tipos de solicitações, é necessário que as ações sejam predefinidas no controlador.</span><span class="sxs-lookup"><span data-stu-id="32c5d-124">In order to handle different kinds of requests, actions are required to be pre-defined in the controller.</span></span> <span data-ttu-id="32c5d-125">O **Roteamento de atributos** é habilitado por padrão no WebApi 2,2.</span><span class="sxs-lookup"><span data-stu-id="32c5d-125">**Attribute routing** is enabled by default in WebApi 2.2.</span></span> <span data-ttu-id="32c5d-126">Por exemplo, para definir uma ação para lidar com a consulta `Revenue` de `Company` usando o roteamento de atributos, use o seguinte:</span><span class="sxs-lookup"><span data-stu-id="32c5d-126">For example, to define an action to handle querying `Revenue` from `Company` using attribute routing, use the following:</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

<span data-ttu-id="32c5d-127">Se você não estiver disposto a definir atributos para cada ação, basta definir suas ações seguindo as [convenções de roteamento OData](../odata-routing-conventions.md).</span><span class="sxs-lookup"><span data-stu-id="32c5d-127">If you are not willing to define attributes for each action, just define your actions following [OData Routing Conventions](../odata-routing-conventions.md).</span></span> <span data-ttu-id="32c5d-128">Como uma chave não é necessária para consultar um singleton, as ações definidas no controlador singleton são ligeiramente diferentes das ações definidas no controlador de EntitySet.</span><span class="sxs-lookup"><span data-stu-id="32c5d-128">Since a key is not required for querying a singleton, the actions defined in the singleton controller are slightly different from actions defined in the entityset controller.</span></span>

<span data-ttu-id="32c5d-129">Para referência, as assinaturas de método para cada definição de ação no controlador singleton estão listadas abaixo.</span><span class="sxs-lookup"><span data-stu-id="32c5d-129">For reference, method signatures for every action definition in the singleton controller are listed below.</span></span>

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

<span data-ttu-id="32c5d-130">Basicamente, isso é tudo o que você precisa fazer no lado do serviço.</span><span class="sxs-lookup"><span data-stu-id="32c5d-130">Basically, this is all you need to do on the service side.</span></span> <span data-ttu-id="32c5d-131">O [projeto de exemplo](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contém todo o código para a solução e o cliente OData que mostra como usar o singleton.</span><span class="sxs-lookup"><span data-stu-id="32c5d-131">The [sample project](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contains all of the code for the solution and the OData client that shows how to use the singleton.</span></span> <span data-ttu-id="32c5d-132">O cliente é criado seguindo as etapas em [criar um aplicativo cliente do OData v4](create-an-odata-v4-client-app.md).</span><span class="sxs-lookup"><span data-stu-id="32c5d-132">The client is built by following the steps in [Create an OData v4 Client App](create-an-odata-v4-client-app.md).</span></span>

<span data-ttu-id="32c5d-133">.</span><span class="sxs-lookup"><span data-stu-id="32c5d-133">.</span></span> 

<span data-ttu-id="32c5d-134">*Agradecemos ao Leo Hu pelo conteúdo original deste artigo.*</span><span class="sxs-lookup"><span data-stu-id="32c5d-134">*Thanks to Leo Hu for the original content of this article.*</span></span>
