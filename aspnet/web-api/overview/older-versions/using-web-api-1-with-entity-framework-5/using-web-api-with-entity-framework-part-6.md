---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Parte 6: criando controladores de produtos e pedidos | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: e0bf88e3477acbde910cde956042449bc86ce79a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600022"
---
# <a name="part-6-creating-product-and-order-controllers"></a><span data-ttu-id="c10e3-102">Parte 6: criando controladores de produtos e pedidos</span><span class="sxs-lookup"><span data-stu-id="c10e3-102">Part 6: Creating Product and Order Controllers</span></span>

<span data-ttu-id="c10e3-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c10e3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c10e3-104">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="c10e3-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a><span data-ttu-id="c10e3-105">Adicionar um controlador de produtos</span><span class="sxs-lookup"><span data-stu-id="c10e3-105">Add a Products Controller</span></span>

<span data-ttu-id="c10e3-106">O controlador de administração é para usuários que têm privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="c10e3-106">The Admin controller is for users who have administrator privileges.</span></span> <span data-ttu-id="c10e3-107">Os clientes, por outro lado, podem exibir produtos, mas não podem criá-los, atualizá-los ou excluí-los.</span><span class="sxs-lookup"><span data-stu-id="c10e3-107">Customers, on the other hand, can view products but cannot create, update, or delete them.</span></span>

<span data-ttu-id="c10e3-108">Podemos restringir facilmente o acesso aos métodos post, PUT e Delete, deixando os métodos get abertos.</span><span class="sxs-lookup"><span data-stu-id="c10e3-108">We can easily restrict access to the Post, Put, and Delete methods, while leaving the Get methods open.</span></span> <span data-ttu-id="c10e3-109">Mas examine os dados que são retornados para um produto:</span><span class="sxs-lookup"><span data-stu-id="c10e3-109">But look at the data that is returned for a product:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

<span data-ttu-id="c10e3-110">A propriedade `ActualCost` não deve ser visível para os clientes!</span><span class="sxs-lookup"><span data-stu-id="c10e3-110">The `ActualCost` property should not be visible to customers!</span></span> <span data-ttu-id="c10e3-111">A solução é definir um DTO ( *objeto de transferência de dados* ) que inclua um subconjunto de propriedades que deve ser visível aos clientes.</span><span class="sxs-lookup"><span data-stu-id="c10e3-111">The solution is to define a *data transfer object* (DTO) that includes a subset of properties that should be visible to customers.</span></span> <span data-ttu-id="c10e3-112">Usaremos o LINQ para projetar `Product` instâncias para `ProductDTO` instâncias.</span><span class="sxs-lookup"><span data-stu-id="c10e3-112">We will use LINQ to project `Product` instances to `ProductDTO` instances.</span></span>

<span data-ttu-id="c10e3-113">Adicione uma classe chamada `ProductDTO` à pasta modelos.</span><span class="sxs-lookup"><span data-stu-id="c10e3-113">Add a class named `ProductDTO` to the Models folder.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

<span data-ttu-id="c10e3-114">Agora, adicione o controlador.</span><span class="sxs-lookup"><span data-stu-id="c10e3-114">Now add the controller.</span></span> <span data-ttu-id="c10e3-115">Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta controladores.</span><span class="sxs-lookup"><span data-stu-id="c10e3-115">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="c10e3-116">Selecione **Adicionar**e, em seguida, selecione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="c10e3-116">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="c10e3-117">Na caixa de diálogo **Adicionar controlador** , nomeie o controlador &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="c10e3-117">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="c10e3-118">Em **modelo**, selecione **controlador de API vazio**.</span><span class="sxs-lookup"><span data-stu-id="c10e3-118">Under **Template**, select **Empty API controller**.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

<span data-ttu-id="c10e3-119">Substitua tudo no arquivo de origem pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="c10e3-119">Replace everything in the source file with the following code:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

<span data-ttu-id="c10e3-120">O controlador ainda usa o `OrdersContext` para consultar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c10e3-120">The controller still uses the `OrdersContext` to query the database.</span></span> <span data-ttu-id="c10e3-121">Mas, em vez de retornar `Product` instâncias diretamente, chamamos `MapProducts` para projetar em `ProductDTO` instâncias:</span><span class="sxs-lookup"><span data-stu-id="c10e3-121">But instead of returning `Product` instances directly, we call `MapProducts` to project them onto `ProductDTO` instances:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

<span data-ttu-id="c10e3-122">O método `MapProducts` retorna um **IQueryable**, portanto, podemos compor o resultado com outros parâmetros de consulta.</span><span class="sxs-lookup"><span data-stu-id="c10e3-122">The `MapProducts` method returns an **IQueryable**, so we can compose the result with other query parameters.</span></span> <span data-ttu-id="c10e3-123">Você pode ver isso no método `GetProduct`, que adiciona uma cláusula **Where** à consulta:</span><span class="sxs-lookup"><span data-stu-id="c10e3-123">You can see this in the `GetProduct` method, which adds a **where** clause to the query:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a><span data-ttu-id="c10e3-124">Adicionar um controlador de pedidos</span><span class="sxs-lookup"><span data-stu-id="c10e3-124">Add an Orders Controller</span></span>

<span data-ttu-id="c10e3-125">Em seguida, adicione um controlador que permita aos usuários criar e exibir pedidos.</span><span class="sxs-lookup"><span data-stu-id="c10e3-125">Next, add a controller that lets users create and view orders.</span></span>

<span data-ttu-id="c10e3-126">Começaremos com outro DTO.</span><span class="sxs-lookup"><span data-stu-id="c10e3-126">We'll start with another DTO.</span></span> <span data-ttu-id="c10e3-127">Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta modelos e adicione uma classe chamada `OrderDTO` use a seguinte implementação:</span><span class="sxs-lookup"><span data-stu-id="c10e3-127">In Solution Explorer, right-click the Models folder and add a class named `OrderDTO` Use the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

<span data-ttu-id="c10e3-128">Agora, adicione o controlador.</span><span class="sxs-lookup"><span data-stu-id="c10e3-128">Now add the controller.</span></span> <span data-ttu-id="c10e3-129">Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta controladores.</span><span class="sxs-lookup"><span data-stu-id="c10e3-129">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="c10e3-130">Selecione **Adicionar**e, em seguida, selecione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="c10e3-130">Select **Add**, then select **Controller**.</span></span> <span data-ttu-id="c10e3-131">Na caixa de diálogo **Adicionar controlador** , defina as seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="c10e3-131">In the **Add Controller** dialog, set the following options:</span></span>

- <span data-ttu-id="c10e3-132">Em **nome do controlador**, insira "OrdersController".</span><span class="sxs-lookup"><span data-stu-id="c10e3-132">Under **Controller Name**, enter "OrdersController".</span></span>
- <span data-ttu-id="c10e3-133">Em **modelo**, selecione "controlador de API com ações de leitura/gravação, usando Entity Framework".</span><span class="sxs-lookup"><span data-stu-id="c10e3-133">Under **Template**, select "API controller with read/write actions, using Entity Framework".</span></span>
- <span data-ttu-id="c10e3-134">Em **classe de modelo**, selecione &quot;ordem (ProductStore. Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="c10e3-134">Under **Model class**, select &quot;Order (ProductStore.Models)&quot;.</span></span>
- <span data-ttu-id="c10e3-135">Em **classe de contexto de dados**, selecione &quot;OrdersContext (ProductStore. Models)&quot;.</span><span class="sxs-lookup"><span data-stu-id="c10e3-135">Under **Data context class**, select &quot;OrdersContext (ProductStore.Models)&quot;.</span></span>

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

<span data-ttu-id="c10e3-136">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="c10e3-136">Click **Add**.</span></span> <span data-ttu-id="c10e3-137">Isso adiciona um arquivo chamado OrdersController.cs.</span><span class="sxs-lookup"><span data-stu-id="c10e3-137">This adds a file named OrdersController.cs.</span></span> <span data-ttu-id="c10e3-138">Em seguida, precisamos modificar a implementação padrão do controlador.</span><span class="sxs-lookup"><span data-stu-id="c10e3-138">Next, we need to modify the default implementation of the controller.</span></span>

<span data-ttu-id="c10e3-139">Primeiro, exclua os métodos `PutOrder` e `DeleteOrder`.</span><span class="sxs-lookup"><span data-stu-id="c10e3-139">First, delete the `PutOrder` and `DeleteOrder` methods.</span></span> <span data-ttu-id="c10e3-140">Para este exemplo, os clientes não podem modificar ou excluir pedidos existentes.</span><span class="sxs-lookup"><span data-stu-id="c10e3-140">For this sample, customers cannot modify or delete existing orders.</span></span> <span data-ttu-id="c10e3-141">Em um aplicativo real, você precisaria de muita lógica de back-end para lidar com esses casos.</span><span class="sxs-lookup"><span data-stu-id="c10e3-141">In a real application, you would need lots of back-end logic to handle these cases.</span></span> <span data-ttu-id="c10e3-142">(Por exemplo, o pedido já foi enviado?)</span><span class="sxs-lookup"><span data-stu-id="c10e3-142">(For example, was the order already shipped?)</span></span>

<span data-ttu-id="c10e3-143">Altere o método `GetOrders` para retornar apenas os pedidos que pertencem ao usuário:</span><span class="sxs-lookup"><span data-stu-id="c10e3-143">Change the `GetOrders` method to return just the orders that belong to the user:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

<span data-ttu-id="c10e3-144">Altere o método `GetOrder` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c10e3-144">Change the `GetOrder` method as follows:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

<span data-ttu-id="c10e3-145">Aqui estão as alterações que fizemos no método:</span><span class="sxs-lookup"><span data-stu-id="c10e3-145">Here are the changes that we made to the method:</span></span>

- <span data-ttu-id="c10e3-146">O valor de retorno é uma instância de `OrderDTO`, em vez de uma `Order`.</span><span class="sxs-lookup"><span data-stu-id="c10e3-146">The return value is an `OrderDTO` instance, instead of an `Order`.</span></span>
- <span data-ttu-id="c10e3-147">Quando consultamos o banco de dados para o pedido, usamos o método [DbQuery. include](https://msdn.microsoft.com/library/gg696395) para buscar as entidades relacionadas `OrderDetail` e `Product`.</span><span class="sxs-lookup"><span data-stu-id="c10e3-147">When we query the database for the order, we use the [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) method to fetch the related `OrderDetail` and `Product` entities.</span></span>
- <span data-ttu-id="c10e3-148">Nós mesclamos o resultado usando uma projeção.</span><span class="sxs-lookup"><span data-stu-id="c10e3-148">We flatten the result by using a projection.</span></span>

<span data-ttu-id="c10e3-149">A resposta HTTP conterá uma matriz de produtos com quantidades:</span><span class="sxs-lookup"><span data-stu-id="c10e3-149">The HTTP response will contain an array of products with quantities:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

<span data-ttu-id="c10e3-150">Esse formato é mais fácil para os clientes consumirem do que o grafo do objeto original, que contém entidades aninhadas (ordem, detalhes e produtos).</span><span class="sxs-lookup"><span data-stu-id="c10e3-150">This format is easier for clients to consume than the original object graph, which contains nested entities (order, details, and products).</span></span>

<span data-ttu-id="c10e3-151">O último método a ser considerado `PostOrder`.</span><span class="sxs-lookup"><span data-stu-id="c10e3-151">The last method to consider it `PostOrder`.</span></span> <span data-ttu-id="c10e3-152">No momento, esse método usa uma instância de `Order`.</span><span class="sxs-lookup"><span data-stu-id="c10e3-152">Right now, this method takes an `Order` instance.</span></span> <span data-ttu-id="c10e3-153">Mas considere o que acontece se um cliente envia um corpo de solicitação como este:</span><span class="sxs-lookup"><span data-stu-id="c10e3-153">But consider what happens if a client sends a request body like this:</span></span>

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

<span data-ttu-id="c10e3-154">Essa é uma ordem bem estruturada e Entity Framework irá inseri-la em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c10e3-154">This is a well-structured order, and Entity Framework will happily insert it into the database.</span></span> <span data-ttu-id="c10e3-155">Mas ele contém uma entidade de produto que não existia anteriormente.</span><span class="sxs-lookup"><span data-stu-id="c10e3-155">But it contains a Product entity that did not exist previously.</span></span> <span data-ttu-id="c10e3-156">O cliente acabou de criar um novo produto em nosso banco de dados!</span><span class="sxs-lookup"><span data-stu-id="c10e3-156">The client just created a new product in our database!</span></span> <span data-ttu-id="c10e3-157">Isso será uma surpresa para o departamento de preenchimento de pedidos, quando eles veem um pedido de Koala.</span><span class="sxs-lookup"><span data-stu-id="c10e3-157">This will be a surprise to the order fulfillment department, when they see an order for koala bears.</span></span> <span data-ttu-id="c10e3-158">A moral é ter muito cuidado com os dados que você aceita em uma solicitação POST ou PUT.</span><span class="sxs-lookup"><span data-stu-id="c10e3-158">The moral is, be really careful about the data you accept in a POST or PUT request.</span></span>

<span data-ttu-id="c10e3-159">Para evitar esse problema, altere o método `PostOrder` para tomar uma instância `OrderDTO`.</span><span class="sxs-lookup"><span data-stu-id="c10e3-159">To avoid this problem, change the `PostOrder` method to take an `OrderDTO` instance.</span></span> <span data-ttu-id="c10e3-160">Use o `OrderDTO` para criar o `Order`.</span><span class="sxs-lookup"><span data-stu-id="c10e3-160">Use the `OrderDTO` to create the `Order`.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

<span data-ttu-id="c10e3-161">Observe que usamos as propriedades `ProductID` e `Quantity` e ignoramos quaisquer valores que o cliente enviou para o nome ou o preço do produto.</span><span class="sxs-lookup"><span data-stu-id="c10e3-161">Notice that we use the `ProductID` and `Quantity` properties, and we ignore any values that the client sent for either product name or price.</span></span> <span data-ttu-id="c10e3-162">Se a ID do produto não for válida, ela violará a restrição FOREIGN KEY no banco de dados e a inserção falhará, como deveria.</span><span class="sxs-lookup"><span data-stu-id="c10e3-162">If the product ID is not valid, it will violate the foreign key constraint in the database, and the insert will fail, as it should.</span></span>

<span data-ttu-id="c10e3-163">Aqui está o método de `PostOrder` completo:</span><span class="sxs-lookup"><span data-stu-id="c10e3-163">Here is the complete `PostOrder` method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

<span data-ttu-id="c10e3-164">Por fim, adicione o atributo **Authorize** ao controlador:</span><span class="sxs-lookup"><span data-stu-id="c10e3-164">Finally, add the **Authorize** attribute to the controller:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

<span data-ttu-id="c10e3-165">Agora, somente os usuários registrados podem criar ou exibir pedidos.</span><span class="sxs-lookup"><span data-stu-id="c10e3-165">Now only registered users can create or view orders.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c10e3-166">[Anterior](using-web-api-with-entity-framework-part-5.md)
> [Próximo](using-web-api-with-entity-framework-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="c10e3-166">[Previous](using-web-api-with-entity-framework-part-5.md)
[Next](using-web-api-with-entity-framework-part-7.md)</span></span>
