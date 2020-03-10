---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Parte 2: criando os modelos de domínio | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 7c5ed1bdb4b390c94907b14e208231f16ad42d96
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621817"
---
# <a name="part-2-creating-the-domain-models"></a><span data-ttu-id="7c63c-102">Parte 2: criando os modelos de domínio</span><span class="sxs-lookup"><span data-stu-id="7c63c-102">Part 2: Creating the Domain Models</span></span>

<span data-ttu-id="7c63c-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7c63c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="7c63c-104">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="7c63c-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a><span data-ttu-id="7c63c-105">Adicionar modelos</span><span class="sxs-lookup"><span data-stu-id="7c63c-105">Add Models</span></span>

<span data-ttu-id="7c63c-106">Há três maneiras de se abordar Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="7c63c-106">There are three ways to approach Entity Framework:</span></span>

- <span data-ttu-id="7c63c-107">Banco de dados-primeiro: você começa com um banco de dados e Entity Framework gera o código.</span><span class="sxs-lookup"><span data-stu-id="7c63c-107">Database-first: You start with a database, and Entity Framework generates the code.</span></span>
- <span data-ttu-id="7c63c-108">Modelo-primeiro: você começa com um modelo visual e Entity Framework gera o banco de dados e o código.</span><span class="sxs-lookup"><span data-stu-id="7c63c-108">Model-first: You start with a visual model, and Entity Framework generates both the database and code.</span></span>
- <span data-ttu-id="7c63c-109">Primeiro código: você começa com o código e Entity Framework gera o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7c63c-109">Code-first: You start with code, and Entity Framework generates the database.</span></span>

<span data-ttu-id="7c63c-110">Estamos usando a abordagem Code-First, portanto, começamos definindo nossos objetos de domínio como POCOs (objetos CLR antigos).</span><span class="sxs-lookup"><span data-stu-id="7c63c-110">We are using the code-first approach, so we start by defining our domain objects as POCOs (plain-old CLR objects).</span></span> <span data-ttu-id="7c63c-111">Com a abordagem de primeiro código, os objetos de domínio não precisam de nenhum código extra para dar suporte à camada de banco de dados, como transações ou persistência.</span><span class="sxs-lookup"><span data-stu-id="7c63c-111">With the code-first approach, domain objects don't need any extra code to support the database layer, such as transactions or persistence.</span></span> <span data-ttu-id="7c63c-112">(Especificamente, eles não precisam herdar da classe [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) .) Você ainda pode usar anotações de dados para controlar como Entity Framework cria o esquema de banco de dado.</span><span class="sxs-lookup"><span data-stu-id="7c63c-112">(Specifically, they do not need to inherit from the [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) class.) You can still use data annotations to control how Entity Framework creates the database schema.</span></span>

<span data-ttu-id="7c63c-113">Como POCOs não carregam nenhuma propriedade extra que descreva o [estado do banco de dados](https://msdn.microsoft.com/library/system.data.entitystate.aspx), eles podem ser facilmente serializados para JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="7c63c-113">Because POCOs do not carry any extra properties that describe [database state](https://msdn.microsoft.com/library/system.data.entitystate.aspx), they can easily be serialized to JSON or XML.</span></span> <span data-ttu-id="7c63c-114">No entanto, isso não significa que você sempre deve expor seus modelos de Entity Framework diretamente aos clientes, como veremos mais adiante no tutorial.</span><span class="sxs-lookup"><span data-stu-id="7c63c-114">However, that does not mean you should always expose your Entity Framework models directly to clients, as we'll see later in the tutorial.</span></span>

<span data-ttu-id="7c63c-115">Criaremos o seguinte POCOs:</span><span class="sxs-lookup"><span data-stu-id="7c63c-115">We will create the following POCOs:</span></span>

- <span data-ttu-id="7c63c-116">Produto</span><span class="sxs-lookup"><span data-stu-id="7c63c-116">Product</span></span>
- <span data-ttu-id="7c63c-117">Order</span><span class="sxs-lookup"><span data-stu-id="7c63c-117">Order</span></span>
- <span data-ttu-id="7c63c-118">OrderDetail</span><span class="sxs-lookup"><span data-stu-id="7c63c-118">OrderDetail</span></span>

<span data-ttu-id="7c63c-119">Para criar cada classe, clique com o botão direito do mouse na pasta modelos em Gerenciador de Soluções.</span><span class="sxs-lookup"><span data-stu-id="7c63c-119">To create each class, right-click the Models folder in Solution Explorer.</span></span> <span data-ttu-id="7c63c-120">No menu de contexto, selecione **Adicionar** e, em seguida, selecione **classe.**</span><span class="sxs-lookup"><span data-stu-id="7c63c-120">From the context menu, select **Add** and then select **Class.**</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

<span data-ttu-id="7c63c-121">Adicione uma classe de `Product` com a seguinte implementação:</span><span class="sxs-lookup"><span data-stu-id="7c63c-121">Add a `Product` class with the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

<span data-ttu-id="7c63c-122">Por convenção, Entity Framework usa a propriedade `Id` como a chave primária e a mapeia para uma coluna de identidade na tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7c63c-122">By convention, Entity Framework uses the `Id` property as the primary key and maps it to an identity column in the database table.</span></span> <span data-ttu-id="7c63c-123">Ao criar uma nova instância de `Product`, você não definirá um valor para `Id`, porque o banco de dados gera o valor.</span><span class="sxs-lookup"><span data-stu-id="7c63c-123">When you create a new `Product` instance, you won't set a value for `Id`, because the database generates the value.</span></span>

<span data-ttu-id="7c63c-124">O atributo **ScaffoldColumn** informa ao ASP.NET MVC para ignorar a propriedade `Id` ao gerar um formulário do editor.</span><span class="sxs-lookup"><span data-stu-id="7c63c-124">The **ScaffoldColumn** attribute tells ASP.NET MVC to skip the `Id` property when generating an editor form.</span></span> <span data-ttu-id="7c63c-125">O atributo **Required** é usado para validar o modelo.</span><span class="sxs-lookup"><span data-stu-id="7c63c-125">The **Required** attribute is used to validate the model.</span></span> <span data-ttu-id="7c63c-126">Ele especifica que a propriedade `Name` deve ser uma cadeia de caracteres não vazia.</span><span class="sxs-lookup"><span data-stu-id="7c63c-126">It specifies that the `Name` property must be a non-empty string.</span></span>

<span data-ttu-id="7c63c-127">Adicione a classe `Order`:</span><span class="sxs-lookup"><span data-stu-id="7c63c-127">Add the `Order` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

<span data-ttu-id="7c63c-128">Adicione a classe `OrderDetail`:</span><span class="sxs-lookup"><span data-stu-id="7c63c-128">Add the `OrderDetail` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a><span data-ttu-id="7c63c-129">Relações de chave estrangeira</span><span class="sxs-lookup"><span data-stu-id="7c63c-129">Foreign Key Relations</span></span>

<span data-ttu-id="7c63c-130">Um pedido contém muitos detalhes do pedido e cada detalhe do pedido se refere a um único produto.</span><span class="sxs-lookup"><span data-stu-id="7c63c-130">An order contains many order details, and each order detail refers to a single product.</span></span> <span data-ttu-id="7c63c-131">Para representar essas relações, a classe `OrderDetail` define as propriedades chamadas `OrderId` e `ProductId`.</span><span class="sxs-lookup"><span data-stu-id="7c63c-131">To represent these relations, the `OrderDetail` class defines properties named `OrderId` and `ProductId`.</span></span> <span data-ttu-id="7c63c-132">Entity Framework inferirá que essas propriedades representam chaves estrangeiras e adicionará restrições Foreign-Key ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7c63c-132">Entity Framework will infer that these properties represent foreign keys, and will add foreign-key constraints to the database.</span></span>

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

<span data-ttu-id="7c63c-133">As classes `Order` e `OrderDetail` também incluem propriedades de "navegação", que contêm referências aos objetos relacionados.</span><span class="sxs-lookup"><span data-stu-id="7c63c-133">The `Order` and `OrderDetail` classes also include "navigation" properties, which contain references to the related objects.</span></span> <span data-ttu-id="7c63c-134">Dado um pedido, você pode navegar para os produtos na ordem seguindo as propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="7c63c-134">Given an order, you can navigate to the products in the order by following the navigation properties.</span></span>

<span data-ttu-id="7c63c-135">Compile o projeto agora.</span><span class="sxs-lookup"><span data-stu-id="7c63c-135">Compile the project now.</span></span> <span data-ttu-id="7c63c-136">Entity Framework usa a reflexão para descobrir as propriedades dos modelos, portanto, ele requer um assembly compilado para criar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7c63c-136">Entity Framework uses reflection to discover the properties of the models, so it requires a compiled assembly to create the database schema.</span></span>

## <a name="configure-the-media-type-formatters"></a><span data-ttu-id="7c63c-137">Configurar os formatadores de tipo de mídia</span><span class="sxs-lookup"><span data-stu-id="7c63c-137">Configure the Media-Type Formatters</span></span>

<span data-ttu-id="7c63c-138">Um [formatador de tipo de mídia](../../formats-and-model-binding/media-formatters.md) é um objeto que serializa seus dados quando a API da Web grava o corpo da resposta http.</span><span class="sxs-lookup"><span data-stu-id="7c63c-138">A [media-type formatter](../../formats-and-model-binding/media-formatters.md) is an object that serializes your data when Web API writes the HTTP response body.</span></span> <span data-ttu-id="7c63c-139">Os formatadores internos dão suporte à saída JSON e XML.</span><span class="sxs-lookup"><span data-stu-id="7c63c-139">The built-in formatters support JSON and XML output.</span></span> <span data-ttu-id="7c63c-140">Por padrão, esses dois formatadores serializam todos os objetos por valor.</span><span class="sxs-lookup"><span data-stu-id="7c63c-140">By default, both of these formatters serialize all objects by value.</span></span>

<span data-ttu-id="7c63c-141">A serialização por valor criará um problema se um gráfico de objetos contiver referências circulares.</span><span class="sxs-lookup"><span data-stu-id="7c63c-141">Serialization by value creates a problem if an object graph contains circular references.</span></span> <span data-ttu-id="7c63c-142">Isso é exatamente o caso com as classes `Order` e `OrderDetail`, porque cada uma tem uma referência para a outra.</span><span class="sxs-lookup"><span data-stu-id="7c63c-142">That's exactly the case with the `Order` and `OrderDetail` classes, because each holds a reference to the other.</span></span> <span data-ttu-id="7c63c-143">O formatador seguirá as referências, gravando cada objeto por valor e acessará os círculos.</span><span class="sxs-lookup"><span data-stu-id="7c63c-143">The formatter will follow the references, writing each object by value, and go in circles.</span></span> <span data-ttu-id="7c63c-144">Portanto, precisamos alterar o comportamento padrão.</span><span class="sxs-lookup"><span data-stu-id="7c63c-144">Therefore, we need to change the default behavior.</span></span>

<span data-ttu-id="7c63c-145">Em Gerenciador de Soluções, expanda a pasta\_iniciar o aplicativo e abra o arquivo chamado WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="7c63c-145">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="7c63c-146">Adicione o código a seguir à classe `WebApiConfig`:</span><span class="sxs-lookup"><span data-stu-id="7c63c-146">Add the following code to the `WebApiConfig` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

<span data-ttu-id="7c63c-147">Esse código define o formatador JSON para preservar referências de objeto e remove totalmente o formatador XML do pipeline.</span><span class="sxs-lookup"><span data-stu-id="7c63c-147">This code sets the JSON formatter to preserve object references, and removes the XML formatter from the pipeline entirely.</span></span> <span data-ttu-id="7c63c-148">(Você pode configurar o formatador XML para preservar referências de objeto, mas é um pouco mais de trabalho, e precisamos apenas de JSON para esse aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7c63c-148">(You can configure the XML formatter to preserve object references, but it's a little more work, and we only need JSON for this application.</span></span> <span data-ttu-id="7c63c-149">Para obter mais informações, consulte [lidando com referências a objetos circulares](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span><span class="sxs-lookup"><span data-stu-id="7c63c-149">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7c63c-150">[Anterior](using-web-api-with-entity-framework-part-1.md)
> [Próximo](using-web-api-with-entity-framework-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="7c63c-150">[Previous](using-web-api-with-entity-framework-part-1.md)
[Next](using-web-api-with-entity-framework-part-3.md)</span></span>
