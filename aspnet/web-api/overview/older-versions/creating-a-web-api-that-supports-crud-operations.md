---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Habilitando operações CRUD no ASP.NET Web API 1-ASP.NET 4. x
author: MikeWasson
description: O tutorial mostra como dar suporte a operações CRUD em um serviço HTTP usando ASP.NET Web API para ASP.NET 4. x.
ms.author: riande
ms.date: 01/28/2012
ms.custom: seoapril2019
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a096fd1c54df33b40115907a5c2517b2e3fec5b8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600331"
---
# <a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="e3e3b-103">Habilitando operações CRUD no ASP.NET Web API 1</span><span class="sxs-lookup"><span data-stu-id="e3e3b-103">Enabling CRUD Operations in ASP.NET Web API 1</span></span>

<span data-ttu-id="e3e3b-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e3e3b-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e3e3b-105">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="e3e3b-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="e3e3b-106">Este tutorial mostra como dar suporte a operações CRUD em um serviço HTTP usando ASP.NET Web API para ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-106">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API for ASP.NET 4.x.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e3e3b-107">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="e3e3b-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e3e3b-108">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="e3e3b-108">Visual Studio 2012</span></span>
> - <span data-ttu-id="e3e3b-109">API Web 1 (também funciona com a API Web 2)</span><span class="sxs-lookup"><span data-stu-id="e3e3b-109">Web API 1 (also works with Web API 2)</span></span>

<span data-ttu-id="e3e3b-110">CRUD significa &quot;criar, ler, atualizar e excluir&quot; que são as quatro operações básicas de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-110">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="e3e3b-111">Muitos serviços HTTP também modelam as operações CRUD por meio de APIs do tipo REST ou REST.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-111">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="e3e3b-112">Neste tutorial, você criará uma API Web muito simples para gerenciar uma lista de produtos.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-112">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="e3e3b-113">Cada produto conterá um nome, preço e categoria (como &quot;Toys&quot; ou &quot;&quot;de hardware), além de uma ID de produto.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-113">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="e3e3b-114">A API de produtos irá expor os métodos a seguir.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-114">The products API will expose following methods.</span></span>

| <span data-ttu-id="e3e3b-115">Action</span><span class="sxs-lookup"><span data-stu-id="e3e3b-115">Action</span></span> | <span data-ttu-id="e3e3b-116">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="e3e3b-116">HTTP method</span></span> | <span data-ttu-id="e3e3b-117">URI relativo</span><span class="sxs-lookup"><span data-stu-id="e3e3b-117">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e3e3b-118">Obter uma lista de todos os produtos</span><span class="sxs-lookup"><span data-stu-id="e3e3b-118">Get a list of all products</span></span> | <span data-ttu-id="e3e3b-119">Obter</span><span class="sxs-lookup"><span data-stu-id="e3e3b-119">GET</span></span> | <span data-ttu-id="e3e3b-120">/api/products</span><span class="sxs-lookup"><span data-stu-id="e3e3b-120">/api/products</span></span> |
| <span data-ttu-id="e3e3b-121">Obter um produto por ID</span><span class="sxs-lookup"><span data-stu-id="e3e3b-121">Get a product by ID</span></span> | <span data-ttu-id="e3e3b-122">Obter</span><span class="sxs-lookup"><span data-stu-id="e3e3b-122">GET</span></span> | <span data-ttu-id="e3e3b-123">*ID* do/API/Products/</span><span class="sxs-lookup"><span data-stu-id="e3e3b-123">/api/products/*id*</span></span> |
| <span data-ttu-id="e3e3b-124">Obter um produto por categoria</span><span class="sxs-lookup"><span data-stu-id="e3e3b-124">Get a product by category</span></span> | <span data-ttu-id="e3e3b-125">Obter</span><span class="sxs-lookup"><span data-stu-id="e3e3b-125">GET</span></span> | <span data-ttu-id="e3e3b-126">/API/Products? categoria =*categoria*</span><span class="sxs-lookup"><span data-stu-id="e3e3b-126">/api/products?category=*category*</span></span> |
| <span data-ttu-id="e3e3b-127">Criar um novo produto</span><span class="sxs-lookup"><span data-stu-id="e3e3b-127">Create a new product</span></span> | <span data-ttu-id="e3e3b-128">Postar</span><span class="sxs-lookup"><span data-stu-id="e3e3b-128">POST</span></span> | <span data-ttu-id="e3e3b-129">/api/products</span><span class="sxs-lookup"><span data-stu-id="e3e3b-129">/api/products</span></span> |
| <span data-ttu-id="e3e3b-130">Atualizar um produto</span><span class="sxs-lookup"><span data-stu-id="e3e3b-130">Update a product</span></span> | <span data-ttu-id="e3e3b-131">PUT</span><span class="sxs-lookup"><span data-stu-id="e3e3b-131">PUT</span></span> | <span data-ttu-id="e3e3b-132">*ID* do/API/Products/</span><span class="sxs-lookup"><span data-stu-id="e3e3b-132">/api/products/*id*</span></span> |
| <span data-ttu-id="e3e3b-133">Excluir um produto</span><span class="sxs-lookup"><span data-stu-id="e3e3b-133">Delete a product</span></span> | <span data-ttu-id="e3e3b-134">DELETE</span><span class="sxs-lookup"><span data-stu-id="e3e3b-134">DELETE</span></span> | <span data-ttu-id="e3e3b-135">*ID* do/API/Products/</span><span class="sxs-lookup"><span data-stu-id="e3e3b-135">/api/products/*id*</span></span> |

<span data-ttu-id="e3e3b-136">Observe que alguns dos URIs incluem a ID do produto no caminho.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-136">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="e3e3b-137">Por exemplo, para obter o produto cuja ID é 28, o cliente envia uma solicitação GET para `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-137">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="e3e3b-138">Recursos</span><span class="sxs-lookup"><span data-stu-id="e3e3b-138">Resources</span></span>

<span data-ttu-id="e3e3b-139">A API Products define URIs para dois tipos de recursos:</span><span class="sxs-lookup"><span data-stu-id="e3e3b-139">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="e3e3b-140">Resource</span><span class="sxs-lookup"><span data-stu-id="e3e3b-140">Resource</span></span> | <span data-ttu-id="e3e3b-141">{1&gt;URI&lt;1}</span><span class="sxs-lookup"><span data-stu-id="e3e3b-141">URI</span></span> |
| --- | --- |
| <span data-ttu-id="e3e3b-142">A lista de todos os produtos.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-142">The list of all the products.</span></span> | <span data-ttu-id="e3e3b-143">/api/products</span><span class="sxs-lookup"><span data-stu-id="e3e3b-143">/api/products</span></span> |
| <span data-ttu-id="e3e3b-144">Um produto individual.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-144">An individual product.</span></span> | <span data-ttu-id="e3e3b-145">*ID* do/API/Products/</span><span class="sxs-lookup"><span data-stu-id="e3e3b-145">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="e3e3b-146">{1&gt;Métodos&lt;1}</span><span class="sxs-lookup"><span data-stu-id="e3e3b-146">Methods</span></span>

<span data-ttu-id="e3e3b-147">Os quatro principais métodos HTTP (GET, PUT, POST e DELETE) podem ser mapeados para operações CRUD da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="e3e3b-147">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="e3e3b-148">GET recupera a representação do recurso em um URI especificado.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-148">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="e3e3b-149">GET não deve ter efeitos colaterais no servidor.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-149">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="e3e3b-150">PUT atualiza um recurso em um URI especificado.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-150">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="e3e3b-151">PUT também pode ser usado para criar um novo recurso em um URI especificado, se o servidor permitir que os clientes especifiquem novos URIs.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-151">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="e3e3b-152">Para este tutorial, a API não dará suporte à criação por meio de PUT.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-152">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="e3e3b-153">POST cria um novo recurso.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-153">POST creates a new resource.</span></span> <span data-ttu-id="e3e3b-154">O servidor atribui o URI para o novo objeto e retorna esse URI como parte da mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-154">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="e3e3b-155">Excluir exclui um recurso em um URI especificado.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-155">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="e3e3b-156">Observação: o método PUT substitui toda a entidade Product.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-156">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="e3e3b-157">Ou seja, espera-se que o cliente envie uma representação completa do produto atualizado.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-157">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="e3e3b-158">Se você quiser dar suporte a atualizações parciais, o método PATCH é preferencial.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-158">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="e3e3b-159">Este tutorial não implementa PATCH.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-159">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="e3e3b-160">Criar um novo projeto de API Web</span><span class="sxs-lookup"><span data-stu-id="e3e3b-160">Create a New Web API Project</span></span>

<span data-ttu-id="e3e3b-161">Comece executando o Visual Studio e selecione **novo projeto** na página **inicial** .</span><span class="sxs-lookup"><span data-stu-id="e3e3b-161">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="e3e3b-162">Ou, no menu **arquivo** , selecione **novo** e **projeto**.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-162">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="e3e3b-163">No painel **modelos** , selecione **modelos instalados** e expanda o **nó C# Visual** .</span><span class="sxs-lookup"><span data-stu-id="e3e3b-163">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="e3e3b-164">Em **Visual C#** , selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-164">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="e3e3b-165">Na lista de modelos de projeto, selecione **aplicativo Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-165">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="e3e3b-166">Nomeie o projeto &quot;ProductStore&quot; e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-166">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="e3e3b-167">Na caixa de diálogo **novo projeto ASP.NET MVC 4** , selecione **API Web** e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-167">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="e3e3b-168">Adicionar um modelo</span><span class="sxs-lookup"><span data-stu-id="e3e3b-168">Adding a Model</span></span>

<span data-ttu-id="e3e3b-169">Um *modelo* é um objeto que representa os dados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-169">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="e3e3b-170">No ASP.NET Web API, você pode usar objetos CLR com rigidez de tipos como modelos e eles serão automaticamente serializados para XML ou JSON para o cliente.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-170">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="e3e3b-171">Para a API ProductStore, nossos dados consistem em produtos, então vamos criar uma nova classe chamada `Product`.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-171">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="e3e3b-172">Se Gerenciador de Soluções ainda não estiver visível, clique no menu **Exibir** e selecione **Gerenciador de soluções**.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-172">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="e3e3b-173">Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta **modelos** .</span><span class="sxs-lookup"><span data-stu-id="e3e3b-173">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="e3e3b-174">No menu de contexto, selecione **Adicionar**e, em seguida, selecione **classe**.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-174">From the context menu, select **Add**, then select **Class**.</span></span> <span data-ttu-id="e3e3b-175">Nomeie a classe &quot;produto&quot;.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-175">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="e3e3b-176">Adicione as propriedades a seguir à classe `Product`.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-176">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="e3e3b-177">Adicionando um repositório</span><span class="sxs-lookup"><span data-stu-id="e3e3b-177">Adding a Repository</span></span>

<span data-ttu-id="e3e3b-178">Precisamos armazenar uma coleção de produtos.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-178">We need to store a collection of products.</span></span> <span data-ttu-id="e3e3b-179">É uma boa ideia separar a coleção de nossa implementação de serviço.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-179">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="e3e3b-180">Dessa forma, podemos alterar o armazenamento de backup sem reescrever a classe de serviço.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-180">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="e3e3b-181">Esse tipo de design é chamado de padrão de *repositório* .</span><span class="sxs-lookup"><span data-stu-id="e3e3b-181">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="e3e3b-182">Comece definindo uma interface genérica para o repositório.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-182">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="e3e3b-183">Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta **modelos** .</span><span class="sxs-lookup"><span data-stu-id="e3e3b-183">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="e3e3b-184">Selecione **Adicionar**e, em seguida, selecione **novo item**.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-184">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="e3e3b-185">No painel **modelos** , selecione **modelos instalados** e expanda o C# nó.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-185">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="e3e3b-186">Em C#, selecione **código**.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-186">Under C#, select **Code**.</span></span> <span data-ttu-id="e3e3b-187">Na lista de modelos de código, selecione **interface**.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-187">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="e3e3b-188">Nomeie a interface &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-188">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="e3e3b-189">Adicione a seguinte implementação:</span><span class="sxs-lookup"><span data-stu-id="e3e3b-189">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="e3e3b-190">Agora, adicione outra classe à pasta modelos, chamada &quot;ProductRepository.&quot; essa classe implementará a interface `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-190">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRepository` interface.</span></span> <span data-ttu-id="e3e3b-191">Adicione a seguinte implementação:</span><span class="sxs-lookup"><span data-stu-id="e3e3b-191">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="e3e3b-192">O repositório mantém a lista na memória local.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-192">The repository keeps the list in local memory.</span></span> <span data-ttu-id="e3e3b-193">Isso é certo para um tutorial, mas em um aplicativo real, você armazenaria os dados externamente, um banco de dado ou em armazenamento em nuvem.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-193">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="e3e3b-194">O padrão de repositório tornará mais fácil alterar a implementação posteriormente.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-194">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="e3e3b-195">Adicionando um controlador de API Web</span><span class="sxs-lookup"><span data-stu-id="e3e3b-195">Adding a Web API Controller</span></span>

<span data-ttu-id="e3e3b-196">Se você trabalhou com o ASP.NET MVC, já está familiarizado com os controladores.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-196">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="e3e3b-197">No ASP.NET Web API, um *controlador* é uma classe que MANIPULA solicitações HTTP do cliente.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-197">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="e3e3b-198">O assistente de novo projeto criou dois controladores para você quando ele criou o projeto.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-198">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="e3e3b-199">Para vê-los, expanda a pasta controladores em Gerenciador de Soluções.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-199">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="e3e3b-200">HomeController é um controlador MVC ASP.NET tradicional.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-200">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="e3e3b-201">Ele é responsável por servir páginas HTML para o site e não está diretamente relacionado à nossa API Web.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-201">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="e3e3b-202">ValuesController é um controlador WebAPI de exemplo.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-202">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="e3e3b-203">Vá em frente e exclua ValuesController clicando com o botão direito do mouse no arquivo em Gerenciador de Soluções e selecionando **excluir.**</span><span class="sxs-lookup"><span data-stu-id="e3e3b-203">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="e3e3b-204">Agora, adicione um novo controlador, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="e3e3b-204">Now add a new controller, as follows:</span></span>

<span data-ttu-id="e3e3b-205">Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta controladores.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-205">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="e3e3b-206">Selecione **Adicionar** e, em seguida, selecione **controlador**.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-206">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="e3e3b-207">No Assistente para **Adicionar controlador** , nomeie o controlador &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-207">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="e3e3b-208">Na lista suspensa **modelo** , selecione **controlador de API vazio**.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-208">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="e3e3b-209">Em seguida, clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-209">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="e3e3b-210">Não é necessário colocar os controladores em uma pasta chamada Controllers.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-210">It is not necessary to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="e3e3b-211">O nome da pasta não é importante; é simplesmente uma maneira conveniente de organizar os arquivos de origem.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-211">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>

<span data-ttu-id="e3e3b-212">O assistente para **Adicionar controlador** criará um arquivo chamado ProductsController.cs na pasta controladores.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-212">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="e3e3b-213">Se esse arquivo ainda não estiver aberto, clique duas vezes no arquivo para abri-lo.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-213">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="e3e3b-214">Adicione a seguinte instrução **using** :</span><span class="sxs-lookup"><span data-stu-id="e3e3b-214">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="e3e3b-215">Adicione um campo que contém uma instância de **IProductRepository** .</span><span class="sxs-lookup"><span data-stu-id="e3e3b-215">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="e3e3b-216">Chamar `new ProductRepository()` no controlador não é o melhor design, pois ele vincula o controlador a uma implementação específica de `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-216">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="e3e3b-217">Para obter uma abordagem melhor, consulte [usando o resolvedor de dependência da API Web](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="e3e3b-217">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>

## <a name="getting-a-resource"></a><span data-ttu-id="e3e3b-218">Obtendo um recurso</span><span class="sxs-lookup"><span data-stu-id="e3e3b-218">Getting a Resource</span></span>

<span data-ttu-id="e3e3b-219">A API ProductStore irá expor várias ações de&quot; de leitura &quot;como métodos GET HTTP.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-219">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="e3e3b-220">Cada ação corresponderá a um método na classe `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-220">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="e3e3b-221">Action</span><span class="sxs-lookup"><span data-stu-id="e3e3b-221">Action</span></span> | <span data-ttu-id="e3e3b-222">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="e3e3b-222">HTTP method</span></span> | <span data-ttu-id="e3e3b-223">URI relativo</span><span class="sxs-lookup"><span data-stu-id="e3e3b-223">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="e3e3b-224">Obter uma lista de todos os produtos</span><span class="sxs-lookup"><span data-stu-id="e3e3b-224">Get a list of all products</span></span> | <span data-ttu-id="e3e3b-225">Obter</span><span class="sxs-lookup"><span data-stu-id="e3e3b-225">GET</span></span> | <span data-ttu-id="e3e3b-226">/api/products</span><span class="sxs-lookup"><span data-stu-id="e3e3b-226">/api/products</span></span> |
| <span data-ttu-id="e3e3b-227">Obter um produto por ID</span><span class="sxs-lookup"><span data-stu-id="e3e3b-227">Get a product by ID</span></span> | <span data-ttu-id="e3e3b-228">Obter</span><span class="sxs-lookup"><span data-stu-id="e3e3b-228">GET</span></span> | <span data-ttu-id="e3e3b-229">*ID* do/API/Products/</span><span class="sxs-lookup"><span data-stu-id="e3e3b-229">/api/products/*id*</span></span> |
| <span data-ttu-id="e3e3b-230">Obter um produto por categoria</span><span class="sxs-lookup"><span data-stu-id="e3e3b-230">Get a product by category</span></span> | <span data-ttu-id="e3e3b-231">Obter</span><span class="sxs-lookup"><span data-stu-id="e3e3b-231">GET</span></span> | <span data-ttu-id="e3e3b-232">/API/Products? categoria =*categoria*</span><span class="sxs-lookup"><span data-stu-id="e3e3b-232">/api/products?category=*category*</span></span> |

<span data-ttu-id="e3e3b-233">Para obter a lista de todos os produtos, adicione este método à classe `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="e3e3b-233">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="e3e3b-234">O nome do método começa com &quot;obter&quot;, portanto, por convenção, ele é mapeado para solicitações GET.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-234">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="e3e3b-235">Além disso, como o método não tem parâmetros, ele é mapeado para um URI que não contém uma *ID de&quot;&quot;* segmento no caminho.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-235">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="e3e3b-236">Para obter um produto por ID, adicione este método à classe `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="e3e3b-236">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="e3e3b-237">Esse nome de método também começa com &quot;obter&quot;, mas o método tem um parâmetro chamado *ID*. Esse parâmetro é mapeado para a ID de &quot;&quot; segmento do caminho do URI.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-237">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="e3e3b-238">A estrutura de ASP.NET Web API converte automaticamente a ID para o tipo de dados correto (**int**) para o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-238">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="e3e3b-239">O método getproduct gera uma exceção do tipo **httpresponseexception** se a *ID* não for válida.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-239">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="e3e3b-240">Essa exceção será convertida pelo Framework em um erro 404 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="e3e3b-240">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="e3e3b-241">Por fim, adicione um método para localizar produtos por categoria:</span><span class="sxs-lookup"><span data-stu-id="e3e3b-241">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="e3e3b-242">Se o URI de solicitação tiver uma cadeia de caracteres de consulta, a API Web tentará corresponder os parâmetros de consulta aos parâmetros no método do controlador.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-242">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="e3e3b-243">Portanto, um URI do formato "API/produtos? Category =*Category*" será mapeado para esse método.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-243">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="e3e3b-244">Criando um recurso</span><span class="sxs-lookup"><span data-stu-id="e3e3b-244">Creating a Resource</span></span>

<span data-ttu-id="e3e3b-245">Em seguida, adicionaremos um método à classe `ProductsController` para criar um novo produto.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-245">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="e3e3b-246">Aqui está uma implementação simples do método:</span><span class="sxs-lookup"><span data-stu-id="e3e3b-246">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="e3e3b-247">Observe duas coisas sobre este método:</span><span class="sxs-lookup"><span data-stu-id="e3e3b-247">Note two things about this method:</span></span>

- <span data-ttu-id="e3e3b-248">O nome do método começa com &quot;post...&quot;.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-248">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="e3e3b-249">Para criar um novo produto, o cliente envia uma solicitação HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-249">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="e3e3b-250">O método usa um parâmetro do tipo Product.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-250">The method takes a parameter of type Product.</span></span> <span data-ttu-id="e3e3b-251">Na API Web, os parâmetros com tipos complexos são desserializados do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-251">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="e3e3b-252">Portanto, esperamos que o cliente envie uma representação serializada de um objeto Product, no formato XML ou JSON.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-252">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="e3e3b-253">Essa implementação funcionará, mas não está completamente completa.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-253">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="e3e3b-254">Idealmente, gostaríamos que a resposta HTTP incluísse o seguinte:</span><span class="sxs-lookup"><span data-stu-id="e3e3b-254">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="e3e3b-255">**Código de resposta:** Por padrão, a estrutura da API da Web define o código de status de resposta como 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="e3e3b-255">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="e3e3b-256">Mas, de acordo com o protocolo HTTP/1.1, quando uma solicitação POST resulta na criação de um recurso, o servidor deve responder com o status 201 (criado).</span><span class="sxs-lookup"><span data-stu-id="e3e3b-256">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="e3e3b-257">**Local:** Quando o servidor cria um recurso, ele deve incluir o URI do novo recurso no cabeçalho de local da resposta.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-257">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="e3e3b-258">ASP.NET Web API torna mais fácil manipular a mensagem de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-258">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="e3e3b-259">Esta é a implementação aprimorada:</span><span class="sxs-lookup"><span data-stu-id="e3e3b-259">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="e3e3b-260">Observe que o tipo de retorno do método agora é **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-260">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="e3e3b-261">Ao retornar um **HttpResponseMessage** em vez de um produto, podemos controlar os detalhes da mensagem de resposta http, incluindo o código de status e o cabeçalho do local.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-261">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="e3e3b-262">O método **CreateResponse** cria um **HttpResponseMessage** e grava automaticamente uma representação serializada do objeto Product no corpo da mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-262">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="e3e3b-263">Este exemplo não valida o `Product`.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-263">This example does not validate the `Product`.</span></span> <span data-ttu-id="e3e3b-264">Para obter informações sobre a validação de modelo, consulte [validação de modelo em ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e3e3b-264">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

## <a name="updating-a-resource"></a><span data-ttu-id="e3e3b-265">Atualizando um recurso</span><span class="sxs-lookup"><span data-stu-id="e3e3b-265">Updating a Resource</span></span>

<span data-ttu-id="e3e3b-266">A atualização de um produto com PUT é simples:</span><span class="sxs-lookup"><span data-stu-id="e3e3b-266">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="e3e3b-267">O nome do método começa com &quot;put...&quot;, portanto, a API Web faz a correspondência para colocar solicitações.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-267">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="e3e3b-268">O método usa dois parâmetros, a ID do produto e o produto atualizado.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-268">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="e3e3b-269">O parâmetro *ID* é extraído do caminho do URI e o parâmetro *Product* é desserializado do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-269">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="e3e3b-270">Por padrão, a estrutura de ASP.NET Web API usa tipos de parâmetro simples da rota e dos tipos complexos do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-270">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="e3e3b-271">Excluindo um recurso</span><span class="sxs-lookup"><span data-stu-id="e3e3b-271">Deleting a Resource</span></span>

<span data-ttu-id="e3e3b-272">Para excluir um recurso, defina um "excluir..." forma.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-272">To delete a resource, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="e3e3b-273">Se uma solicitação de exclusão for realizada com sucesso, ela poderá retornar o status 200 (OK) com um corpo de entidade que descreve o status; status 202 (aceito) se a exclusão ainda estiver pendente; ou o status 204 (sem conteúdo) sem corpo de entidade.</span><span class="sxs-lookup"><span data-stu-id="e3e3b-273">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="e3e3b-274">Nesse caso, o método `DeleteProduct` tem um tipo de retorno `void`, portanto ASP.NET Web API converte automaticamente isso no código de status 204 (sem conteúdo).</span><span class="sxs-lookup"><span data-stu-id="e3e3b-274">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
