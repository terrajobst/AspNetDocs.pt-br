---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Habilitar operações CRUD na API 1 - ASP.NET Web ASP.NET 4. x
author: MikeWasson
description: O tutorial mostra como dar suporte a operações CRUD em um serviço HTTP usando a API Web do ASP.NET para ASP.NET 4. x.
ms.author: riande
ms.date: 01/28/2012
ms.custom: seoapril2019
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 855c3fa35d82173c87d13adb51e10fd13698ade5
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59381342"
---
# <a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="ebcad-103">Habilitar operações CRUD na API 1 da Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ebcad-103">Enabling CRUD Operations in ASP.NET Web API 1</span></span>

<span data-ttu-id="ebcad-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ebcad-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="ebcad-105">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="ebcad-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="ebcad-106">Este tutorial mostra como dar suporte a operações CRUD em um serviço HTTP usando a API Web do ASP.NET para ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="ebcad-106">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API for ASP.NET 4.x.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="ebcad-107">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="ebcad-107">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="ebcad-108">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="ebcad-108">Visual Studio 2012</span></span>
> - <span data-ttu-id="ebcad-109">API Web 1 (também funciona com a API Web 2)</span><span class="sxs-lookup"><span data-stu-id="ebcad-109">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="ebcad-110">Representa o CRUD &quot;criação, leitura, atualização e exclusão,&quot; que são as quatro operações de banco de dados básico.</span><span class="sxs-lookup"><span data-stu-id="ebcad-110">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="ebcad-111">Muitos serviços HTTP do modelo também operações CRUD por meio do REST ou APIs de REST.</span><span class="sxs-lookup"><span data-stu-id="ebcad-111">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="ebcad-112">Neste tutorial, você criará uma API para gerenciar uma lista de produtos de web muito simple.</span><span class="sxs-lookup"><span data-stu-id="ebcad-112">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="ebcad-113">Cada produto contém um nome, preço e categoria (tal como &quot;toys&quot; ou &quot;hardware&quot;), além de uma ID de produto.</span><span class="sxs-lookup"><span data-stu-id="ebcad-113">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="ebcad-114">Os produtos de API irá expor métodos a seguir.</span><span class="sxs-lookup"><span data-stu-id="ebcad-114">The products API will expose following methods.</span></span>

| <span data-ttu-id="ebcad-115">Ação</span><span class="sxs-lookup"><span data-stu-id="ebcad-115">Action</span></span> | <span data-ttu-id="ebcad-116">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="ebcad-116">HTTP method</span></span> | <span data-ttu-id="ebcad-117">URI relativo</span><span class="sxs-lookup"><span data-stu-id="ebcad-117">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ebcad-118">Obter uma lista de todos os produtos</span><span class="sxs-lookup"><span data-stu-id="ebcad-118">Get a list of all products</span></span> | <span data-ttu-id="ebcad-119">OBTER</span><span class="sxs-lookup"><span data-stu-id="ebcad-119">GET</span></span> | <span data-ttu-id="ebcad-120">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="ebcad-120">/api/products</span></span> |
| <span data-ttu-id="ebcad-121">Obter um produto por ID</span><span class="sxs-lookup"><span data-stu-id="ebcad-121">Get a product by ID</span></span> | <span data-ttu-id="ebcad-122">OBTER</span><span class="sxs-lookup"><span data-stu-id="ebcad-122">GET</span></span> | <span data-ttu-id="ebcad-123">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="ebcad-123">/api/products/*id*</span></span> |
| <span data-ttu-id="ebcad-124">Obter um produto por categoria</span><span class="sxs-lookup"><span data-stu-id="ebcad-124">Get a product by category</span></span> | <span data-ttu-id="ebcad-125">OBTER</span><span class="sxs-lookup"><span data-stu-id="ebcad-125">GET</span></span> | <span data-ttu-id="ebcad-126">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="ebcad-126">/api/products?category=*category*</span></span> |
| <span data-ttu-id="ebcad-127">Criar um novo produto</span><span class="sxs-lookup"><span data-stu-id="ebcad-127">Create a new product</span></span> | <span data-ttu-id="ebcad-128">POSTAR</span><span class="sxs-lookup"><span data-stu-id="ebcad-128">POST</span></span> | <span data-ttu-id="ebcad-129">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="ebcad-129">/api/products</span></span> |
| <span data-ttu-id="ebcad-130">Atualizar um produto</span><span class="sxs-lookup"><span data-stu-id="ebcad-130">Update a product</span></span> | <span data-ttu-id="ebcad-131">PUT</span><span class="sxs-lookup"><span data-stu-id="ebcad-131">PUT</span></span> | <span data-ttu-id="ebcad-132">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="ebcad-132">/api/products/*id*</span></span> |
| <span data-ttu-id="ebcad-133">Excluir um produto</span><span class="sxs-lookup"><span data-stu-id="ebcad-133">Delete a product</span></span> | <span data-ttu-id="ebcad-134">DELETE</span><span class="sxs-lookup"><span data-stu-id="ebcad-134">DELETE</span></span> | <span data-ttu-id="ebcad-135">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="ebcad-135">/api/products/*id*</span></span> |

<span data-ttu-id="ebcad-136">Observe que alguns dos URIs incluem a ID do produto no caminho.</span><span class="sxs-lookup"><span data-stu-id="ebcad-136">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="ebcad-137">Por exemplo, para obter o produto cuja ID é 28, o cliente envia uma solicitação GET `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="ebcad-137">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="ebcad-138">Recursos</span><span class="sxs-lookup"><span data-stu-id="ebcad-138">Resources</span></span>

<span data-ttu-id="ebcad-139">Os produtos de API define os URIs para dois tipos de recursos:</span><span class="sxs-lookup"><span data-stu-id="ebcad-139">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="ebcad-140">Recurso</span><span class="sxs-lookup"><span data-stu-id="ebcad-140">Resource</span></span> | <span data-ttu-id="ebcad-141">URI</span><span class="sxs-lookup"><span data-stu-id="ebcad-141">URI</span></span> |
| --- | --- |
| <span data-ttu-id="ebcad-142">A lista de todos os produtos.</span><span class="sxs-lookup"><span data-stu-id="ebcad-142">The list of all the products.</span></span> | <span data-ttu-id="ebcad-143">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="ebcad-143">/api/products</span></span> |
| <span data-ttu-id="ebcad-144">Um produto individual.</span><span class="sxs-lookup"><span data-stu-id="ebcad-144">An individual product.</span></span> | <span data-ttu-id="ebcad-145">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="ebcad-145">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="ebcad-146">Métodos</span><span class="sxs-lookup"><span data-stu-id="ebcad-146">Methods</span></span>

<span data-ttu-id="ebcad-147">Os quatro principais métodos HTTP (GET, PUT, POST e DELETE) podem ser mapeados para operações CRUD da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ebcad-147">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="ebcad-148">GET recupera a representação do recurso em um URI especificado.</span><span class="sxs-lookup"><span data-stu-id="ebcad-148">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="ebcad-149">GET não deve ter nenhum efeito colateral no servidor.</span><span class="sxs-lookup"><span data-stu-id="ebcad-149">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="ebcad-150">PUT atualiza um recurso em um URI especificado.</span><span class="sxs-lookup"><span data-stu-id="ebcad-150">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="ebcad-151">PUT também pode ser usado para criar um novo recurso em um URI especificado, se o servidor permite que os clientes especifiquem os novos URIs.</span><span class="sxs-lookup"><span data-stu-id="ebcad-151">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="ebcad-152">Para este tutorial, a API não dará suporte à criação por meio de PUT.</span><span class="sxs-lookup"><span data-stu-id="ebcad-152">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="ebcad-153">POST cria um novo recurso.</span><span class="sxs-lookup"><span data-stu-id="ebcad-153">POST creates a new resource.</span></span> <span data-ttu-id="ebcad-154">O servidor atribui o URI para o novo objeto e retorna esse URI como parte da mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="ebcad-154">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="ebcad-155">DELETE Exclui um recurso em um URI especificado.</span><span class="sxs-lookup"><span data-stu-id="ebcad-155">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="ebcad-156">Observação: O método PUT substitui a entidade de produtos inteiro.</span><span class="sxs-lookup"><span data-stu-id="ebcad-156">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="ebcad-157">Ou seja, o cliente deve enviar uma representação completa do produto atualizado.</span><span class="sxs-lookup"><span data-stu-id="ebcad-157">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="ebcad-158">Se você quiser dar suporte a atualizações parciais, o método de PATCH é preferencial.</span><span class="sxs-lookup"><span data-stu-id="ebcad-158">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="ebcad-159">Este tutorial não implementa o PATCH.</span><span class="sxs-lookup"><span data-stu-id="ebcad-159">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="ebcad-160">Criar um novo projeto de API da Web</span><span class="sxs-lookup"><span data-stu-id="ebcad-160">Create a New Web API Project</span></span>

<span data-ttu-id="ebcad-161">Comece executando o Visual Studio e selecione **novo projeto** da **iniciar** página.</span><span class="sxs-lookup"><span data-stu-id="ebcad-161">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="ebcad-162">Ou, no menu **Arquivo**, selecione **Novo** e, em seguida, **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="ebcad-162">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="ebcad-163">No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="ebcad-163">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="ebcad-164">Em **Visual C#**, selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="ebcad-164">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="ebcad-165">Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="ebcad-165">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="ebcad-166">Nomeie o projeto &quot;ProductStore&quot; e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="ebcad-166">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="ebcad-167">No **novo projeto ASP.NET MVC 4** caixa de diálogo, selecione **API da Web** e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="ebcad-167">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="ebcad-168">Adicionar um modelo</span><span class="sxs-lookup"><span data-stu-id="ebcad-168">Adding a Model</span></span>

<span data-ttu-id="ebcad-169">Um *modelo (model)* é um objeto que representa os dados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ebcad-169">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="ebcad-170">Na API Web ASP.NET, você pode usar objetos CLR fortemente tipados como modelos e eles serão automaticamente serializados para JSON ou XML para o cliente.</span><span class="sxs-lookup"><span data-stu-id="ebcad-170">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="ebcad-171">Para a API ProductStore, nossos dados consistem em produtos, portanto, vamos criar uma nova classe chamada `Product`.</span><span class="sxs-lookup"><span data-stu-id="ebcad-171">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="ebcad-172">Se o Gerenciador de Soluções não estiver visível, clique no menu **Exibir** e selecione **Gerenciador de Soluções**.</span><span class="sxs-lookup"><span data-stu-id="ebcad-172">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="ebcad-173">No Gerenciador de soluções, clique com botão direito do **modelos** pasta.</span><span class="sxs-lookup"><span data-stu-id="ebcad-173">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="ebcad-174">No menu de contexto, selecione **Add**, em seguida, selecione **classe**.</span><span class="sxs-lookup"><span data-stu-id="ebcad-174">From the context menu, select **Add**, then select **Class**.</span></span> <span data-ttu-id="ebcad-175">Nomeie a classe &quot;Produt&quot; (produto).</span><span class="sxs-lookup"><span data-stu-id="ebcad-175">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="ebcad-176">Adicione as seguintes propriedades para a classe `Product`.</span><span class="sxs-lookup"><span data-stu-id="ebcad-176">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="ebcad-177">Adicionar um repositório</span><span class="sxs-lookup"><span data-stu-id="ebcad-177">Adding a Repository</span></span>

<span data-ttu-id="ebcad-178">É necessário armazenar uma coleção de produtos.</span><span class="sxs-lookup"><span data-stu-id="ebcad-178">We need to store a collection of products.</span></span> <span data-ttu-id="ebcad-179">É uma boa ideia separar a coleção da nossa implementação de serviço.</span><span class="sxs-lookup"><span data-stu-id="ebcad-179">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="ebcad-180">Dessa forma, podemos alterar o repositório de backup sem reescrever a classe de serviço.</span><span class="sxs-lookup"><span data-stu-id="ebcad-180">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="ebcad-181">Esse tipo de design é chamado de *repositório* padrão.</span><span class="sxs-lookup"><span data-stu-id="ebcad-181">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="ebcad-182">Comece definindo uma interface genérica para o repositório.</span><span class="sxs-lookup"><span data-stu-id="ebcad-182">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="ebcad-183">No Gerenciador de soluções, clique com botão direito do **modelos** pasta.</span><span class="sxs-lookup"><span data-stu-id="ebcad-183">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="ebcad-184">Selecione **Add**, em seguida, selecione **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="ebcad-184">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="ebcad-185">No **modelos** painel, selecione **modelos instalados** e expanda o nó do c#.</span><span class="sxs-lookup"><span data-stu-id="ebcad-185">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="ebcad-186">Em c#, selecione **código**.</span><span class="sxs-lookup"><span data-stu-id="ebcad-186">Under C#, select **Code**.</span></span> <span data-ttu-id="ebcad-187">Na lista de modelos de código, selecione **Interface**.</span><span class="sxs-lookup"><span data-stu-id="ebcad-187">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="ebcad-188">Nomeie a interface &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="ebcad-188">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="ebcad-189">Adicione a seguinte implementação:</span><span class="sxs-lookup"><span data-stu-id="ebcad-189">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="ebcad-190">Agora, adicione outra classe para a pasta de modelos, denominada &quot;ProductRepository.&quot; Essa classe implementará a interface `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="ebcad-190">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRepository` interface.</span></span> <span data-ttu-id="ebcad-191">Adicione a seguinte implementação:</span><span class="sxs-lookup"><span data-stu-id="ebcad-191">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="ebcad-192">O repositório mantém a lista na memória local.</span><span class="sxs-lookup"><span data-stu-id="ebcad-192">The repository keeps the list in local memory.</span></span> <span data-ttu-id="ebcad-193">Isso é Okey para obter um tutorial, mas em um aplicativo real, você poderia armazenar os dados externamente, um banco de dados ou no armazenamento em nuvem.</span><span class="sxs-lookup"><span data-stu-id="ebcad-193">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="ebcad-194">O padrão de repositório tornará mais fácil alterar a implementação mais tarde.</span><span class="sxs-lookup"><span data-stu-id="ebcad-194">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="ebcad-195">Adicionando um controlador de API da Web</span><span class="sxs-lookup"><span data-stu-id="ebcad-195">Adding a Web API Controller</span></span>

<span data-ttu-id="ebcad-196">Se você tiver trabalhado com o ASP.NET MVC, em seguida, você já está familiarizados com os controladores.</span><span class="sxs-lookup"><span data-stu-id="ebcad-196">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="ebcad-197">Na API Web ASP.NET, uma *controlador* é uma classe que manipula as solicitações HTTP do cliente.</span><span class="sxs-lookup"><span data-stu-id="ebcad-197">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="ebcad-198">O Assistente de novo projeto criado dois controladores para você quando ele criou o projeto.</span><span class="sxs-lookup"><span data-stu-id="ebcad-198">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="ebcad-199">Para vê-las, expanda a pasta de controladores no Gerenciador de soluções.</span><span class="sxs-lookup"><span data-stu-id="ebcad-199">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="ebcad-200">HomeController é um controlador de MVC do ASP.NET tradicional.</span><span class="sxs-lookup"><span data-stu-id="ebcad-200">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="ebcad-201">Ele é responsável por fornecer páginas HTML para o site e não está diretamente relacionado à nossa API da web.</span><span class="sxs-lookup"><span data-stu-id="ebcad-201">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="ebcad-202">ValuesController é um controlador de API da Web de exemplo.</span><span class="sxs-lookup"><span data-stu-id="ebcad-202">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="ebcad-203">Vá em frente e exclua ValuesController, clicando duas vezes no arquivo no Gerenciador de soluções e selecionando **excluir.**</span><span class="sxs-lookup"><span data-stu-id="ebcad-203">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="ebcad-204">Agora adicione um novo controlador, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ebcad-204">Now add a new controller, as follows:</span></span>

<span data-ttu-id="ebcad-205">Em **Gerenciador de Soluções**, clique com o botão direito na pasta controllers (controladores). </span><span class="sxs-lookup"><span data-stu-id="ebcad-205">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="ebcad-206">Selecione **Adicionar** e, em seguida, selecione **Controlador**.</span><span class="sxs-lookup"><span data-stu-id="ebcad-206">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="ebcad-207">No **Adicionar controlador** assistente, nomeie o controlador &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="ebcad-207">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="ebcad-208">No **modelo** lista suspensa, selecione **controlador API vazio**.</span><span class="sxs-lookup"><span data-stu-id="ebcad-208">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="ebcad-209">Em seguida, clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="ebcad-209">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="ebcad-210">Não é necessário colocar seus controladores em uma pasta chamada controladores.</span><span class="sxs-lookup"><span data-stu-id="ebcad-210">It is not necessary to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="ebcad-211">O nome da pasta não é importante; é simplesmente uma maneira conveniente de organizar seus arquivos de origem.</span><span class="sxs-lookup"><span data-stu-id="ebcad-211">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="ebcad-212">O **Adicionar controlador** assistente criará um arquivo chamado ProductsController.cs na pasta controladores.</span><span class="sxs-lookup"><span data-stu-id="ebcad-212">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="ebcad-213">Se esse arquivo ainda não estiver aberto, clique duas vezes no arquivo para abri-lo.</span><span class="sxs-lookup"><span data-stu-id="ebcad-213">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="ebcad-214">Adicione o seguinte **usando** instrução:</span><span class="sxs-lookup"><span data-stu-id="ebcad-214">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="ebcad-215">Adicionar um campo que contém um **IProductRepository** instância.</span><span class="sxs-lookup"><span data-stu-id="ebcad-215">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="ebcad-216">Chamando `new ProductRepository()` no controlador não é o melhor design, porque ele liga o controlador para uma implementação específica de `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="ebcad-216">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="ebcad-217">Para uma abordagem melhor, consulte [usando o resolvedor de dependência do Web API](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="ebcad-217">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="ebcad-218">Introdução a um recurso</span><span class="sxs-lookup"><span data-stu-id="ebcad-218">Getting a Resource</span></span>

<span data-ttu-id="ebcad-219">A API ProductStore irá expor várias &quot;ler&quot; ações como métodos HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="ebcad-219">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="ebcad-220">Cada ação corresponderá a um método no `ProductsController` classe.</span><span class="sxs-lookup"><span data-stu-id="ebcad-220">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="ebcad-221">Ação</span><span class="sxs-lookup"><span data-stu-id="ebcad-221">Action</span></span> | <span data-ttu-id="ebcad-222">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="ebcad-222">HTTP method</span></span> | <span data-ttu-id="ebcad-223">URI relativo</span><span class="sxs-lookup"><span data-stu-id="ebcad-223">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ebcad-224">Obter uma lista de todos os produtos</span><span class="sxs-lookup"><span data-stu-id="ebcad-224">Get a list of all products</span></span> | <span data-ttu-id="ebcad-225">OBTER</span><span class="sxs-lookup"><span data-stu-id="ebcad-225">GET</span></span> | <span data-ttu-id="ebcad-226">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="ebcad-226">/api/products</span></span> |
| <span data-ttu-id="ebcad-227">Obter um produto por ID</span><span class="sxs-lookup"><span data-stu-id="ebcad-227">Get a product by ID</span></span> | <span data-ttu-id="ebcad-228">OBTER</span><span class="sxs-lookup"><span data-stu-id="ebcad-228">GET</span></span> | <span data-ttu-id="ebcad-229">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="ebcad-229">/api/products/*id*</span></span> |
| <span data-ttu-id="ebcad-230">Obter um produto por categoria</span><span class="sxs-lookup"><span data-stu-id="ebcad-230">Get a product by category</span></span> | <span data-ttu-id="ebcad-231">OBTER</span><span class="sxs-lookup"><span data-stu-id="ebcad-231">GET</span></span> | <span data-ttu-id="ebcad-232">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="ebcad-232">/api/products?category=*category*</span></span> |

<span data-ttu-id="ebcad-233">Para obter a lista de todos os produtos, adicione este método para o `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="ebcad-233">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="ebcad-234">O nome do método começa com &quot;obter&quot;, portanto, por convenção, ele é mapeado para solicitações GET.</span><span class="sxs-lookup"><span data-stu-id="ebcad-234">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="ebcad-235">Além disso, porque o método não tiver parâmetros, ele é mapeado para um URI que não contém um *&quot;id&quot;* segmento no caminho.</span><span class="sxs-lookup"><span data-stu-id="ebcad-235">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="ebcad-236">Para obter um produto por ID, adicione este método para o `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="ebcad-236">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="ebcad-237">Esse nome de método também começa com &quot;Obtenha&quot;, mas o método tem um parâmetro denominado *id*. Esse parâmetro é mapeado para o &quot;id&quot; segmento do caminho do URI.</span><span class="sxs-lookup"><span data-stu-id="ebcad-237">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="ebcad-238">A estrutura da API Web ASP.NET converte automaticamente a ID para o tipo de dados correto (**int**) para o parâmetro.</span><span class="sxs-lookup"><span data-stu-id="ebcad-238">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="ebcad-239">O método GetProduct lança uma exceção do tipo **HttpResponseException** se *id* não é válido.</span><span class="sxs-lookup"><span data-stu-id="ebcad-239">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="ebcad-240">Essa exceção será convertida pela estrutura em um erro 404 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="ebcad-240">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="ebcad-241">Por fim, adicione um método para localizar produtos por categoria:</span><span class="sxs-lookup"><span data-stu-id="ebcad-241">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="ebcad-242">Se o URI da solicitação tem uma cadeia de caracteres de consulta, API da Web tenta corresponder os parâmetros de consulta para parâmetros do método do controlador.</span><span class="sxs-lookup"><span data-stu-id="ebcad-242">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="ebcad-243">Portanto, um URI do formulário "api/produtos? categoria =*categoria*" será mapeado para esse método.</span><span class="sxs-lookup"><span data-stu-id="ebcad-243">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="ebcad-244">Criar um recurso</span><span class="sxs-lookup"><span data-stu-id="ebcad-244">Creating a Resource</span></span>

<span data-ttu-id="ebcad-245">Em seguida, adicionaremos um método para o `ProductsController` classe para criar um novo produto.</span><span class="sxs-lookup"><span data-stu-id="ebcad-245">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="ebcad-246">Aqui está uma implementação simples do método:</span><span class="sxs-lookup"><span data-stu-id="ebcad-246">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="ebcad-247">Observe duas coisas sobre esse método:</span><span class="sxs-lookup"><span data-stu-id="ebcad-247">Note two things about this method:</span></span>

- <span data-ttu-id="ebcad-248">O nome do método começa com &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="ebcad-248">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="ebcad-249">Para criar um novo produto, o cliente envia uma solicitação HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="ebcad-249">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="ebcad-250">O método utiliza um parâmetro de tipo de produto.</span><span class="sxs-lookup"><span data-stu-id="ebcad-250">The method takes a parameter of type Product.</span></span> <span data-ttu-id="ebcad-251">Na API da Web, os parâmetros com tipos complexos são desserializados do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="ebcad-251">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="ebcad-252">Portanto, esperamos que o cliente envie uma representação serializada de um objeto de produto, em formato XML ou JSON.</span><span class="sxs-lookup"><span data-stu-id="ebcad-252">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="ebcad-253">Essa implementação funcionará, mas não está completa.</span><span class="sxs-lookup"><span data-stu-id="ebcad-253">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="ebcad-254">Idealmente, gostaríamos de resposta HTTP para incluir o seguinte:</span><span class="sxs-lookup"><span data-stu-id="ebcad-254">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="ebcad-255">**Código de resposta:** Por padrão, a estrutura da API Web define o código de status de resposta a 200 (Okey).</span><span class="sxs-lookup"><span data-stu-id="ebcad-255">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="ebcad-256">Mas de acordo com o protocolo HTTP/1.1, quando uma solicitação POST resulta na criação de um recurso, o servidor deverá responder com status 201 (criado).</span><span class="sxs-lookup"><span data-stu-id="ebcad-256">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="ebcad-257">**Local:** Quando o servidor cria um recurso, ele deve incluir o URI do novo recurso no cabeçalho Location da resposta.</span><span class="sxs-lookup"><span data-stu-id="ebcad-257">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="ebcad-258">API Web ASP.NET torna fácil manipular a mensagem de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="ebcad-258">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="ebcad-259">Aqui está a implementação aprimorada:</span><span class="sxs-lookup"><span data-stu-id="ebcad-259">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="ebcad-260">Observe que o tipo de retorno do método agora é **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="ebcad-260">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="ebcad-261">Retornando um **HttpResponseMessage** em vez de um produto, podemos controlar os detalhes da mensagem de resposta HTTP, incluindo o código de status e o cabeçalho de localização.</span><span class="sxs-lookup"><span data-stu-id="ebcad-261">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="ebcad-262">O **CreateResponse** método cria um **HttpResponseMessage** e gravará automaticamente uma representação serializada do objeto Product no corpo da fo a mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="ebcad-262">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="ebcad-263">Este exemplo não valida o `Product`.</span><span class="sxs-lookup"><span data-stu-id="ebcad-263">This example does not validate the `Product`.</span></span> <span data-ttu-id="ebcad-264">Para obter informações sobre a validação de modelo, consulte [validação do modelo na API Web ASP.NET](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ebcad-264">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="ebcad-265">Atualizar um recurso</span><span class="sxs-lookup"><span data-stu-id="ebcad-265">Updating a Resource</span></span>

<span data-ttu-id="ebcad-266">Atualizar um produto com PUT é simples:</span><span class="sxs-lookup"><span data-stu-id="ebcad-266">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="ebcad-267">O nome do método começa com &quot;colocar... &quot;, portanto, a API da Web corresponde a ele para solicitações PUT.</span><span class="sxs-lookup"><span data-stu-id="ebcad-267">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="ebcad-268">O método utiliza dois parâmetros, a ID do produto e o produto atualizado.</span><span class="sxs-lookup"><span data-stu-id="ebcad-268">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="ebcad-269">O *identificação* parâmetro é obtido do caminho do URI e o *produto* parâmetro é desserializado do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="ebcad-269">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="ebcad-270">Por padrão, a estrutura da API Web do ASP.NET usa tipos de parâmetro simples da rota e tipos complexos do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="ebcad-270">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="ebcad-271">Excluir um recurso</span><span class="sxs-lookup"><span data-stu-id="ebcad-271">Deleting a Resource</span></span>

<span data-ttu-id="ebcad-272">Para excluir um recurso, definir um "excluir..." método.</span><span class="sxs-lookup"><span data-stu-id="ebcad-272">To delete a resource, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="ebcad-273">Se uma solicitação de exclusão for bem-sucedida, ela pode retornar o status 200 (Okey) com um corpo de entidade que descreve o status; status 202 (aceito) se a exclusão ainda estiver pendente; ou de status 204 (sem conteúdo) sem o corpo da entidade.</span><span class="sxs-lookup"><span data-stu-id="ebcad-273">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="ebcad-274">Nesse caso, o `DeleteProduct` método tem um `void` tipo de retorno, portanto, a API Web ASP.NET automaticamente converte isso em status 204 (sem conteúdo) de código.</span><span class="sxs-lookup"><span data-stu-id="ebcad-274">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
