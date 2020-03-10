---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Parte 3: Criando um controlador de administração | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: f39be7a84e85db93487d246e9f8cb59c401fe5ce
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556045"
---
# <a name="part-3-creating-an-admin-controller"></a><span data-ttu-id="f55ca-102">Parte 3: Criando um controlador de administração</span><span class="sxs-lookup"><span data-stu-id="f55ca-102">Part 3: Creating an Admin Controller</span></span>

<span data-ttu-id="f55ca-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f55ca-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="f55ca-104">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="f55ca-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a><span data-ttu-id="f55ca-105">Adicionar um controlador de administrador</span><span class="sxs-lookup"><span data-stu-id="f55ca-105">Add an Admin Controller</span></span>

<span data-ttu-id="f55ca-106">Nesta seção, adicionaremos um controlador de API Web que oferece suporte a operações CRUD (criar, ler, atualizar e excluir) em produtos.</span><span class="sxs-lookup"><span data-stu-id="f55ca-106">In this section, we'll add a Web API controller that supports CRUD (create, read, update, and delete) operations on products.</span></span> <span data-ttu-id="f55ca-107">O controlador usará Entity Framework para se comunicar com a camada de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f55ca-107">The controller will use Entity Framework to communicate with the database layer.</span></span> <span data-ttu-id="f55ca-108">Somente os administradores poderão usar esse controlador.</span><span class="sxs-lookup"><span data-stu-id="f55ca-108">Only administrators will be able to use this controller.</span></span> <span data-ttu-id="f55ca-109">Os clientes acessarão os produtos por meio de outro controlador.</span><span class="sxs-lookup"><span data-stu-id="f55ca-109">Customers will access the products through another controller.</span></span>

<span data-ttu-id="f55ca-110">Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta controladores.</span><span class="sxs-lookup"><span data-stu-id="f55ca-110">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="f55ca-111">Selecione **Adicionar** e, em seguida, **controlador**.</span><span class="sxs-lookup"><span data-stu-id="f55ca-111">Select **Add** and then **Controller**.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

<span data-ttu-id="f55ca-112">Na caixa de diálogo **Adicionar controlador** , nomeie o controlador `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="f55ca-112">In the **Add Controller** dialog, name the controller `AdminController`.</span></span> <span data-ttu-id="f55ca-113">Em **modelo**, selecione &quot;controlador de API com ações de leitura/gravação, usando Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="f55ca-113">Under **Template**, select &quot;API controller with read/write actions, using Entity Framework&quot;.</span></span> <span data-ttu-id="f55ca-114">Em **classe de modelo**, selecione "produto (ProductStore. Models)".</span><span class="sxs-lookup"><span data-stu-id="f55ca-114">Under **Model class**, select "Product (ProductStore.Models)".</span></span> <span data-ttu-id="f55ca-115">Em **contexto de dados**, selecione "&lt;novo contexto de dados&gt;".</span><span class="sxs-lookup"><span data-stu-id="f55ca-115">Under **Data Context**, select "&lt;New Data Context&gt;".</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="f55ca-116">Se a lista suspensa **classe de modelo** não mostrar classes de modelo, verifique se você compilou o projeto.</span><span class="sxs-lookup"><span data-stu-id="f55ca-116">If the **Model class** drop-down does not show any model classes, make sure you compiled the project.</span></span> <span data-ttu-id="f55ca-117">Entity Framework usa reflexão, portanto, precisa do assembly compilado.</span><span class="sxs-lookup"><span data-stu-id="f55ca-117">Entity Framework uses reflection, so it needs the compiled assembly.</span></span>

<span data-ttu-id="f55ca-118">Se você selecionar "&lt;novo contexto de dados&gt;" abrirá a caixa de diálogo **novo contexto de dados** .</span><span class="sxs-lookup"><span data-stu-id="f55ca-118">Selecting "&lt;New Data Context&gt;" will open the **New Data Context** dialog.</span></span> <span data-ttu-id="f55ca-119">Nomeie o `ProductStore.Models.OrdersContext`de contexto de dados.</span><span class="sxs-lookup"><span data-stu-id="f55ca-119">Name the data context `ProductStore.Models.OrdersContext`.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

<span data-ttu-id="f55ca-120">Clique em **OK** para ignorar a caixa de diálogo **novo contexto de dados** .</span><span class="sxs-lookup"><span data-stu-id="f55ca-120">Click **OK** to dismiss the **New Data Context** dialog.</span></span> <span data-ttu-id="f55ca-121">Na caixa de diálogo **Adicionar controlador** , clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="f55ca-121">In the **Add Controller** dialog, click **Add**.</span></span>

<span data-ttu-id="f55ca-122">Veja o que foi adicionado ao projeto:</span><span class="sxs-lookup"><span data-stu-id="f55ca-122">Here's what got added to the project:</span></span>

- <span data-ttu-id="f55ca-123">Uma classe chamada `OrdersContext` que deriva de **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="f55ca-123">A class named `OrdersContext` that derives from **DbContext**.</span></span> <span data-ttu-id="f55ca-124">Essa classe fornece a cola entre os modelos POCO e o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f55ca-124">This class provides the glue between the POCO models and the database.</span></span>
- <span data-ttu-id="f55ca-125">Um controlador de API da Web chamado `AdminController`.</span><span class="sxs-lookup"><span data-stu-id="f55ca-125">A Web API controller named `AdminController`.</span></span> <span data-ttu-id="f55ca-126">Esse controlador oferece suporte a operações CRUD em instâncias de `Product`.</span><span class="sxs-lookup"><span data-stu-id="f55ca-126">This controller supports CRUD operations on `Product` instances.</span></span> <span data-ttu-id="f55ca-127">Ele usa a classe `OrdersContext` para se comunicar com Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f55ca-127">It uses the `OrdersContext` class to communicate with Entity Framework.</span></span>
- <span data-ttu-id="f55ca-128">Uma nova cadeia de conexão de banco de dados no arquivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="f55ca-128">A new database connection string in the Web.config file.</span></span>

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

<span data-ttu-id="f55ca-129">Abra o arquivo OrdersContext.cs.</span><span class="sxs-lookup"><span data-stu-id="f55ca-129">Open the OrdersContext.cs file.</span></span> <span data-ttu-id="f55ca-130">Observe que o construtor Especifica o nome da cadeia de conexão do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f55ca-130">Notice that the constructor specifies the name of the database connection string.</span></span> <span data-ttu-id="f55ca-131">Esse nome se refere à cadeia de conexão que foi adicionada ao Web. config.</span><span class="sxs-lookup"><span data-stu-id="f55ca-131">This name refers to the connection string that was added to Web.config.</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

<span data-ttu-id="f55ca-132">Adicione as seguintes propriedades à classe `OrdersContext`:</span><span class="sxs-lookup"><span data-stu-id="f55ca-132">Add the following properties to the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

<span data-ttu-id="f55ca-133">Um **DbSet** representa um conjunto de entidades que podem ser consultadas.</span><span class="sxs-lookup"><span data-stu-id="f55ca-133">A **DbSet** represents a set of entities that can be queried.</span></span> <span data-ttu-id="f55ca-134">Aqui está a listagem completa para a classe `OrdersContext`:</span><span class="sxs-lookup"><span data-stu-id="f55ca-134">Here is the complete listing for the `OrdersContext` class:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

<span data-ttu-id="f55ca-135">A classe `AdminController` define cinco métodos que implementam a funcionalidade CRUD básica.</span><span class="sxs-lookup"><span data-stu-id="f55ca-135">The `AdminController` class defines five methods that implement basic CRUD functionality.</span></span> <span data-ttu-id="f55ca-136">Cada método corresponde a um URI que o cliente pode invocar:</span><span class="sxs-lookup"><span data-stu-id="f55ca-136">Each method corresponds to a URI that the client can invoke:</span></span>

| <span data-ttu-id="f55ca-137">Método de controlador</span><span class="sxs-lookup"><span data-stu-id="f55ca-137">Controller Method</span></span> | <span data-ttu-id="f55ca-138">DESCRIÇÃO</span><span class="sxs-lookup"><span data-stu-id="f55ca-138">Description</span></span> | <span data-ttu-id="f55ca-139">URI</span><span class="sxs-lookup"><span data-stu-id="f55ca-139">URI</span></span> | <span data-ttu-id="f55ca-140">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="f55ca-140">HTTP Method</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f55ca-141">GetProducts</span><span class="sxs-lookup"><span data-stu-id="f55ca-141">GetProducts</span></span> | <span data-ttu-id="f55ca-142">Obtém todos os produtos.</span><span class="sxs-lookup"><span data-stu-id="f55ca-142">Gets all products.</span></span> | <span data-ttu-id="f55ca-143">API/produtos</span><span class="sxs-lookup"><span data-stu-id="f55ca-143">api/products</span></span> | <span data-ttu-id="f55ca-144">GET</span><span class="sxs-lookup"><span data-stu-id="f55ca-144">GET</span></span> |
| <span data-ttu-id="f55ca-145">Getproduct</span><span class="sxs-lookup"><span data-stu-id="f55ca-145">GetProduct</span></span> | <span data-ttu-id="f55ca-146">Localiza um produto por ID.</span><span class="sxs-lookup"><span data-stu-id="f55ca-146">Finds a product by ID.</span></span> | <span data-ttu-id="f55ca-147">API/produtos/*ID*</span><span class="sxs-lookup"><span data-stu-id="f55ca-147">api/products/*id*</span></span> | <span data-ttu-id="f55ca-148">GET</span><span class="sxs-lookup"><span data-stu-id="f55ca-148">GET</span></span> |
| <span data-ttu-id="f55ca-149">PutProduct</span><span class="sxs-lookup"><span data-stu-id="f55ca-149">PutProduct</span></span> | <span data-ttu-id="f55ca-150">Atualiza um produto.</span><span class="sxs-lookup"><span data-stu-id="f55ca-150">Updates a product.</span></span> | <span data-ttu-id="f55ca-151">API/produtos/*ID*</span><span class="sxs-lookup"><span data-stu-id="f55ca-151">api/products/*id*</span></span> | <span data-ttu-id="f55ca-152">PUT</span><span class="sxs-lookup"><span data-stu-id="f55ca-152">PUT</span></span> |
| <span data-ttu-id="f55ca-153">Produto</span><span class="sxs-lookup"><span data-stu-id="f55ca-153">PostProduct</span></span> | <span data-ttu-id="f55ca-154">Cria um produto.</span><span class="sxs-lookup"><span data-stu-id="f55ca-154">Creates a new product.</span></span> | <span data-ttu-id="f55ca-155">API/produtos</span><span class="sxs-lookup"><span data-stu-id="f55ca-155">api/products</span></span> | <span data-ttu-id="f55ca-156">POST</span><span class="sxs-lookup"><span data-stu-id="f55ca-156">POST</span></span> |
| <span data-ttu-id="f55ca-157">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="f55ca-157">DeleteProduct</span></span> | <span data-ttu-id="f55ca-158">Exclui um produto.</span><span class="sxs-lookup"><span data-stu-id="f55ca-158">Deletes a product.</span></span> | <span data-ttu-id="f55ca-159">API/produtos/*ID*</span><span class="sxs-lookup"><span data-stu-id="f55ca-159">api/products/*id*</span></span> | <span data-ttu-id="f55ca-160">Delete (excluir)</span><span class="sxs-lookup"><span data-stu-id="f55ca-160">DELETE</span></span> |

<span data-ttu-id="f55ca-161">Cada método chama `OrdersContext` para consultar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f55ca-161">Each method calls into `OrdersContext` to query the database.</span></span> <span data-ttu-id="f55ca-162">Os métodos que modificam a coleção (PUT, POST e DELETE) chamam `db.SaveChanges` para persistir as alterações no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f55ca-162">The methods that modify the collection (PUT, POST, and DELETE) call `db.SaveChanges` to persist the changes to the database.</span></span> <span data-ttu-id="f55ca-163">Os controladores são criados por solicitação HTTP e, em seguida, descartados, portanto, é necessário manter as alterações antes que um método seja retornado.</span><span class="sxs-lookup"><span data-stu-id="f55ca-163">Controllers are created per HTTP request and then disposed, so it is necessary to persist changes before a method returns.</span></span>

## <a name="add-a-database-initializer"></a><span data-ttu-id="f55ca-164">Adicionar um inicializador de banco de dados</span><span class="sxs-lookup"><span data-stu-id="f55ca-164">Add a Database Initializer</span></span>

<span data-ttu-id="f55ca-165">Entity Framework tem um bom recurso que permite que você preencha o banco de dados na inicialização e recrie o banco de dados automaticamente sempre que os modelos forem alterados.</span><span class="sxs-lookup"><span data-stu-id="f55ca-165">Entity Framework has a nice feature that lets you populate the database on startup, and automatically recreate the database whenever the models change.</span></span> <span data-ttu-id="f55ca-166">Esse recurso é útil durante o desenvolvimento, porque você sempre tem alguns dados de teste, mesmo se você alterar os modelos.</span><span class="sxs-lookup"><span data-stu-id="f55ca-166">This feature is useful during development, because you always have some test data, even if you change the models.</span></span>

<span data-ttu-id="f55ca-167">Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta modelos e crie uma nova classe chamada `OrdersContextInitializer`.</span><span class="sxs-lookup"><span data-stu-id="f55ca-167">In Solution Explorer, right-click the Models folder and create a new class named `OrdersContextInitializer`.</span></span> <span data-ttu-id="f55ca-168">Cole na seguinte implementação:</span><span class="sxs-lookup"><span data-stu-id="f55ca-168">Paste in the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

<span data-ttu-id="f55ca-169">Ao herdar da classe **DropCreateDatabaseIfModelChanges** , estamos dizendo Entity Framework para descartar o banco de dados sempre que modificarmos as classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="f55ca-169">By inheriting from the **DropCreateDatabaseIfModelChanges** class, we are telling Entity Framework to drop the database whenever we modify the model classes.</span></span> <span data-ttu-id="f55ca-170">Quando Entity Framework cria (ou recria) o banco de dados, ele chama o método **semente** para popular as tabelas.</span><span class="sxs-lookup"><span data-stu-id="f55ca-170">When Entity Framework creates (or recreates) the database, it calls the **Seed** method to populate the tables.</span></span> <span data-ttu-id="f55ca-171">Usamos o método **semente** para adicionar alguns produtos de exemplo mais um exemplo de ordem.</span><span class="sxs-lookup"><span data-stu-id="f55ca-171">We use the **Seed** method to add some example products plus an example order.</span></span>

<span data-ttu-id="f55ca-172">Esse recurso é ótimo para teste, mas não use a classe **DropCreateDatabaseIfModelChanges** em produção, porque você poderá perder seus dados se alguém alterar uma classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="f55ca-172">This feature is great for testing, but don't use the **DropCreateDatabaseIfModelChanges** class in production,, because you could lose your data if someone changes a model class.</span></span>

<span data-ttu-id="f55ca-173">Em seguida, abra global. asax e adicione o seguinte código ao **aplicativo\_** método de início:</span><span class="sxs-lookup"><span data-stu-id="f55ca-173">Next, open Global.asax and add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a><span data-ttu-id="f55ca-174">Enviar uma solicitação para o controlador</span><span class="sxs-lookup"><span data-stu-id="f55ca-174">Send a Request to the Controller</span></span>

<span data-ttu-id="f55ca-175">Neste ponto, não escrevemos nenhum código de cliente, mas você pode invocar a API Web usando um navegador da Web ou uma ferramenta de depuração de HTTP, como o [Fiddler](http://www.fiddler2.com/fiddler2/).</span><span class="sxs-lookup"><span data-stu-id="f55ca-175">At this point, we haven't written any client code, but you can invoke the web API using a web browser or an HTTP debugging tool such as [Fiddler](http://www.fiddler2.com/fiddler2/).</span></span> <span data-ttu-id="f55ca-176">No Visual Studio, pressione F5 para iniciar a depuração.</span><span class="sxs-lookup"><span data-stu-id="f55ca-176">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="f55ca-177">Seu navegador da Web será aberto para `http://localhost:*portnum*/`, em que *PortNum* é um número de porta.</span><span class="sxs-lookup"><span data-stu-id="f55ca-177">Your web browser will open to `http://localhost:*portnum*/`, where *portnum* is some port number.</span></span>

<span data-ttu-id="f55ca-178">Enviar uma solicitação HTTP para "`http://localhost:*portnum*/api/admin`.</span><span class="sxs-lookup"><span data-stu-id="f55ca-178">Send an HTTP request to "`http://localhost:*portnum*/api/admin`.</span></span> <span data-ttu-id="f55ca-179">A primeira solicitação pode ser lenta para ser concluída, pois Entity Framework precisa criar e propagar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f55ca-179">The first request may be slow to complete, because Entity Framework needs to create and seed the database.</span></span> <span data-ttu-id="f55ca-180">A resposta deve ter algo semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="f55ca-180">The response should something similar to the following:</span></span>

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> <span data-ttu-id="f55ca-181">[Anterior](using-web-api-with-entity-framework-part-2.md)
> [Próximo](using-web-api-with-entity-framework-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="f55ca-181">[Previous](using-web-api-with-entity-framework-part-2.md)
[Next](using-web-api-with-entity-framework-part-4.md)</span></span>
