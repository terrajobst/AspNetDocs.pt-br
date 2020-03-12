---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Introdução ao ASP.NET Web API 2 (C#)-ASP.NET 4. x
author: MikeWasson
description: Tutorial com código. Use ASP.NET Web API para criar uma API Web que retorna uma lista de produtos.
ms.author: riande
ms.date: 11/28/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 2717d93f47be9d4a6548731d8deeca312b25f39f
ms.sourcegitcommit: 9e3ca74997a67c18589729d4b7303799905473eb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79084046"
---
# <a name="get-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="2f47b-104">Introdução ao ASP.NET Web API 2 (C#)</span><span class="sxs-lookup"><span data-stu-id="2f47b-104">Get Started with ASP.NET Web API 2 (C#)</span></span>

<span data-ttu-id="2f47b-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2f47b-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2f47b-106">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="2f47b-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="2f47b-107">Neste tutorial, você usará a API Web ASP.NET para criar uma API Web que retorna uma lista de produtos.</span><span class="sxs-lookup"><span data-stu-id="2f47b-107">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

<span data-ttu-id="2f47b-108">HTTP não serve apenas para servir páginas da Web.</span><span class="sxs-lookup"><span data-stu-id="2f47b-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="2f47b-109">O HTTP também é uma plataforma poderosa para a criação de APIs que expõem serviços e dados.</span><span class="sxs-lookup"><span data-stu-id="2f47b-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="2f47b-110">O HTTP é simples, flexível e onipresente.</span><span class="sxs-lookup"><span data-stu-id="2f47b-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="2f47b-111">Quase todas as plataformas que você pode considerar têm uma biblioteca HTTP, portanto, os serviços HTTP podem alcançar uma ampla variedade de clientes, incluindo navegadores, dispositivos móveis e aplicativos de desktops tradicionais.</span><span class="sxs-lookup"><span data-stu-id="2f47b-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>

<span data-ttu-id="2f47b-112">A API Web ASP.NET é uma estrutura para a criação de APIs Web sobre o .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2f47b-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> 

## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2f47b-113">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="2f47b-113">Software versions used in the tutorial</span></span>

- [<span data-ttu-id="2f47b-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2f47b-114">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
- <span data-ttu-id="2f47b-115">API Web 2</span><span class="sxs-lookup"><span data-stu-id="2f47b-115">Web API 2</span></span>

<span data-ttu-id="2f47b-116">Consulte [criar uma API Web com o ASP.NET Core e o Visual Studio para Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) para obter uma versão mais recente deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="2f47b-116">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="2f47b-117">Criar um projeto de API Web</span><span class="sxs-lookup"><span data-stu-id="2f47b-117">Create a Web API Project</span></span>

<span data-ttu-id="2f47b-118">Neste tutorial, você usará a API Web ASP.NET para criar uma API Web que retorna uma lista de produtos.</span><span class="sxs-lookup"><span data-stu-id="2f47b-118">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="2f47b-119">A página da Web front-end usa jQuery para exibir os resultados.</span><span class="sxs-lookup"><span data-stu-id="2f47b-119">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="2f47b-120">Inicie o Visual Studio e selecione **novo projeto** na página **inicial** .</span><span class="sxs-lookup"><span data-stu-id="2f47b-120">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="2f47b-121">Ou, no menu **arquivo** , selecione **novo** e **projeto**.</span><span class="sxs-lookup"><span data-stu-id="2f47b-121">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="2f47b-122">No painel **modelos** , selecione **modelos instalados** e expanda o **nó C# Visual** .</span><span class="sxs-lookup"><span data-stu-id="2f47b-122">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="2f47b-123">Em **Visual C#** , selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="2f47b-123">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="2f47b-124">Na lista de modelos de projeto, selecione **aplicativo Web ASP.net**.</span><span class="sxs-lookup"><span data-stu-id="2f47b-124">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="2f47b-125">Nomeie o projeto "ProductsApp" e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f47b-125">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="2f47b-126">Na caixa de diálogo **novo projeto ASP.net** , selecione o modelo **vazio** .</span><span class="sxs-lookup"><span data-stu-id="2f47b-126">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="2f47b-127">Em &quot;adicionar pastas e referências principais para&quot;, verifique a **API Web**.</span><span class="sxs-lookup"><span data-stu-id="2f47b-127">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="2f47b-128">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="2f47b-128">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="2f47b-129">Você também pode criar um projeto de API Web usando o modelo &quot;API Web&quot;.</span><span class="sxs-lookup"><span data-stu-id="2f47b-129">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="2f47b-130">O modelo de Web API usa o ASP.NET MVC para fornecer páginas de Ajuda da API.</span><span class="sxs-lookup"><span data-stu-id="2f47b-130">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="2f47b-131">Estou usando o modelo vazio para este tutorial porque Mostrar Web API sem MVC.</span><span class="sxs-lookup"><span data-stu-id="2f47b-131">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="2f47b-132">Em geral, você não precisa saber o ASP.NET MVC para usar a Web API.</span><span class="sxs-lookup"><span data-stu-id="2f47b-132">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>

## <a name="adding-a-model"></a><span data-ttu-id="2f47b-133">Adicionar um modelo</span><span class="sxs-lookup"><span data-stu-id="2f47b-133">Adding a Model</span></span>

<span data-ttu-id="2f47b-134">Um *modelo* é um objeto que representa os dados no seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2f47b-134">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="2f47b-135">ASP.NET Web API pode serializar automaticamente seu modelo para outro formato, XML ou JSON, e, em seguida, gravar os dados serializados no corpo da mensagem de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2f47b-135">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="2f47b-136">Como um cliente pode ler o formato de serialização, ele pode desserializar o objeto.</span><span class="sxs-lookup"><span data-stu-id="2f47b-136">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="2f47b-137">A maioria dos clientes pode analisar XML ou JSON.</span><span class="sxs-lookup"><span data-stu-id="2f47b-137">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="2f47b-138">Além disso, o cliente pode indicar qual formato ele deseja definindo o cabeçalho Accept na mensagem de solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="2f47b-138">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="2f47b-139">Vamos começar criando um modelo simples que representa um produto.</span><span class="sxs-lookup"><span data-stu-id="2f47b-139">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="2f47b-140">Se o Gerenciador de Soluções não estiver visível, clique no menu **Exibir** e selecione **Gerenciador de Soluções**.</span><span class="sxs-lookup"><span data-stu-id="2f47b-140">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="2f47b-141">No Gerenciador de Soluções, clique com o botão direito do mouse na pasta Modelos.</span><span class="sxs-lookup"><span data-stu-id="2f47b-141">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="2f47b-142">No menu de contexto, selecione **Adicionar** e, em seguida, selecione **Classe**.</span><span class="sxs-lookup"><span data-stu-id="2f47b-142">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="2f47b-143">Nomeie a classe &quot;produto&quot;.</span><span class="sxs-lookup"><span data-stu-id="2f47b-143">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="2f47b-144">Adicione as propriedades a seguir à classe `Product`.</span><span class="sxs-lookup"><span data-stu-id="2f47b-144">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="2f47b-145">Adicionando um controlador</span><span class="sxs-lookup"><span data-stu-id="2f47b-145">Adding a Controller</span></span>

<span data-ttu-id="2f47b-146">Na API da Web, um *controlador* é um objeto que manipula as solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="2f47b-146">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="2f47b-147">Vamos adicionar um controlador que pode retornar uma lista de produtos ou um único produto especificado por ID.</span><span class="sxs-lookup"><span data-stu-id="2f47b-147">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="2f47b-148">Se você tiver usado o ASP.NET MVC, você já está familiarizados com os controladores.</span><span class="sxs-lookup"><span data-stu-id="2f47b-148">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="2f47b-149">Os controladores de API da Web são semelhantes aos controladores MVC, mas herdam a classe **ApiController** em vez da classe **Controller** .</span><span class="sxs-lookup"><span data-stu-id="2f47b-149">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="2f47b-150">No **Gerenciador de Soluções**, clique com o botão direito do mouse na pasta Controladores.</span><span class="sxs-lookup"><span data-stu-id="2f47b-150">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="2f47b-151">Selecione **Adicionar** e, em seguida, selecione **Controlador**.</span><span class="sxs-lookup"><span data-stu-id="2f47b-151">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="2f47b-152">Na caixa de diálogo **Adicionar Scaffold**, selecione **Controlador da API Web – Vazio**.</span><span class="sxs-lookup"><span data-stu-id="2f47b-152">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="2f47b-153">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="2f47b-153">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="2f47b-154">Na caixa de diálogo **Adicionar controlador** , nomeie o controlador &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="2f47b-154">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="2f47b-155">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="2f47b-155">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="2f47b-156">O scaffolding cria um arquivo chamado ProductsController.cs na pasta controladores.</span><span class="sxs-lookup"><span data-stu-id="2f47b-156">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="2f47b-157">Você não precisa colocar seus controladores em uma pasta chamada Controllers.</span><span class="sxs-lookup"><span data-stu-id="2f47b-157">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="2f47b-158">O nome da pasta é apenas uma maneira conveniente de organizar os arquivos de origem.</span><span class="sxs-lookup"><span data-stu-id="2f47b-158">The folder name is just a convenient way to organize your source files.</span></span>

<span data-ttu-id="2f47b-159">Se esse arquivo não estiver aberto, clique duas vezes no arquivo para abri-lo.</span><span class="sxs-lookup"><span data-stu-id="2f47b-159">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="2f47b-160">Substitua o código deste arquivo pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="2f47b-160">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="2f47b-161">Para manter o exemplo simples, os produtos são armazenados em uma matriz fixa dentro da classe do controlador.</span><span class="sxs-lookup"><span data-stu-id="2f47b-161">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="2f47b-162">É claro que, em um aplicativo real, você consulta um banco de dados ou usar outra fonte de dados externa.</span><span class="sxs-lookup"><span data-stu-id="2f47b-162">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="2f47b-163">O controlador define dois métodos que retornam produtos:</span><span class="sxs-lookup"><span data-stu-id="2f47b-163">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="2f47b-164">O método `GetAllProducts` retorna a lista completa de produtos como um tipo de **&gt;de produto IEnumerable&lt;** .</span><span class="sxs-lookup"><span data-stu-id="2f47b-164">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="2f47b-165">O método `GetProduct` pesquisa um único produto por sua ID.</span><span class="sxs-lookup"><span data-stu-id="2f47b-165">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="2f47b-166">É isso!</span><span class="sxs-lookup"><span data-stu-id="2f47b-166">That's it!</span></span> <span data-ttu-id="2f47b-167">Você está trabalhando em uma API Web.</span><span class="sxs-lookup"><span data-stu-id="2f47b-167">You have a working web API.</span></span> <span data-ttu-id="2f47b-168">Cada método do controlador corresponde a um ou mais URIs:</span><span class="sxs-lookup"><span data-stu-id="2f47b-168">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="2f47b-169">Método de controlador</span><span class="sxs-lookup"><span data-stu-id="2f47b-169">Controller Method</span></span> | <span data-ttu-id="2f47b-170">URI</span><span class="sxs-lookup"><span data-stu-id="2f47b-170">URI</span></span> |
| --- | --- |
| <span data-ttu-id="2f47b-171">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="2f47b-171">GetAllProducts</span></span> | <span data-ttu-id="2f47b-172">/api/products</span><span class="sxs-lookup"><span data-stu-id="2f47b-172">/api/products</span></span> |
| <span data-ttu-id="2f47b-173">Getproduct</span><span class="sxs-lookup"><span data-stu-id="2f47b-173">GetProduct</span></span> | <span data-ttu-id="2f47b-174">*ID* do/API/Products/</span><span class="sxs-lookup"><span data-stu-id="2f47b-174">/api/products/*id*</span></span> |

<span data-ttu-id="2f47b-175">Para o método `GetProduct`, a *ID* no URI é um espaço reservado.</span><span class="sxs-lookup"><span data-stu-id="2f47b-175">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="2f47b-176">Por exemplo, para obter o produto com a ID 5, o URI é `api/products/5`.</span><span class="sxs-lookup"><span data-stu-id="2f47b-176">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="2f47b-177">Para obter mais informações sobre como a API Web roteia solicitações HTTP para métodos do controlador, consulte [Roteamento em ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="2f47b-177">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="2f47b-178">Chamar a API Web com Javascript e jQuery</span><span class="sxs-lookup"><span data-stu-id="2f47b-178">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="2f47b-179">Nesta seção, vamos adicionar uma página HTML que usa AJAX para chamar a API Web.</span><span class="sxs-lookup"><span data-stu-id="2f47b-179">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="2f47b-180">Vamos usar jQuery para fazer chamadas AJAX e também para atualizar a página com os resultados.</span><span class="sxs-lookup"><span data-stu-id="2f47b-180">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="2f47b-181">Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto e selecione **Adicionar**e, em seguida, selecione **novo item**.</span><span class="sxs-lookup"><span data-stu-id="2f47b-181">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="2f47b-182">Na caixa de diálogo **Adicionar novo item** , selecione o nó **Web** em **C#Visual**e, em seguida, selecione o item **página HTML** .</span><span class="sxs-lookup"><span data-stu-id="2f47b-182">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="2f47b-183">Nomeie a página &quot;index. html&quot;.</span><span class="sxs-lookup"><span data-stu-id="2f47b-183">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="2f47b-184">Substitua tudo neste arquivo pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="2f47b-184">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="2f47b-185">Há várias maneiras de obter o jQuery.</span><span class="sxs-lookup"><span data-stu-id="2f47b-185">There are several ways to get jQuery.</span></span> <span data-ttu-id="2f47b-186">Neste exemplo, usei a CDN do [Microsoft Ajax](../../../ajax/cdn/overview.md).</span><span class="sxs-lookup"><span data-stu-id="2f47b-186">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="2f47b-187">Você também pode baixá-lo de [http://jquery.com/](http://jquery.com/)e o modelo de projeto ASP.net "API Web" também inclui o jQuery.</span><span class="sxs-lookup"><span data-stu-id="2f47b-187">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="2f47b-188">Obtendo uma lista de produtos</span><span class="sxs-lookup"><span data-stu-id="2f47b-188">Getting a List of Products</span></span>

<span data-ttu-id="2f47b-189">Para obter uma lista de produtos, envie uma solicitação HTTP GET para &quot;/API/Products&quot;.</span><span class="sxs-lookup"><span data-stu-id="2f47b-189">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="2f47b-190">A função [getJson](http://api.jquery.com/jQuery.getJSON/) do jQuery envia uma solicitação Ajax.</span><span class="sxs-lookup"><span data-stu-id="2f47b-190">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="2f47b-191">A resposta contém a matriz de objetos JSON.</span><span class="sxs-lookup"><span data-stu-id="2f47b-191">The response contains array of JSON objects.</span></span> <span data-ttu-id="2f47b-192">A função `done` especifica um retorno de chamada que é chamado se a solicitação for realizada com sucesso.</span><span class="sxs-lookup"><span data-stu-id="2f47b-192">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="2f47b-193">No retorno de chamada, atualizamos o DOM com as informações de produto.</span><span class="sxs-lookup"><span data-stu-id="2f47b-193">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="2f47b-194">Obtendo um produto por ID</span><span class="sxs-lookup"><span data-stu-id="2f47b-194">Getting a Product By ID</span></span>

<span data-ttu-id="2f47b-195">Para obter um produto por ID, envie uma solicitação HTTP GET para &quot;*ID* /API/Products/&quot;, em que *ID* é a ID do produto.</span><span class="sxs-lookup"><span data-stu-id="2f47b-195">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="2f47b-196">Ainda chamamos `getJSON` para enviar a solicitação AJAX, mas desta vez colocamos a ID no URI de solicitação.</span><span class="sxs-lookup"><span data-stu-id="2f47b-196">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="2f47b-197">A resposta dessa solicitação é uma representação JSON de um único produto.</span><span class="sxs-lookup"><span data-stu-id="2f47b-197">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="2f47b-198">Executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="2f47b-198">Running the Application</span></span>

<span data-ttu-id="2f47b-199">Pressione F5 para iniciar a depuração do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2f47b-199">Press F5 to start debugging the application.</span></span> <span data-ttu-id="2f47b-200">A página da Web deve ser parecida com a seguinte:</span><span class="sxs-lookup"><span data-stu-id="2f47b-200">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="2f47b-201">Para obter um produto por ID, insira a ID e clique em Pesquisar:</span><span class="sxs-lookup"><span data-stu-id="2f47b-201">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="2f47b-202">Se você inserir uma ID inválida, o servidor retornará um erro HTTP:</span><span class="sxs-lookup"><span data-stu-id="2f47b-202">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="2f47b-203">Usando F12 para exibir a solicitação e a resposta HTTP</span><span class="sxs-lookup"><span data-stu-id="2f47b-203">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="2f47b-204">Quando você está trabalhando com um serviço HTTP, pode ser muito útil ver as mensagens de solicitação e resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2f47b-204">When you are working with an HTTP service, it can be very useful to see the HTTP request and response messages.</span></span> <span data-ttu-id="2f47b-205">Você pode fazer isso usando as ferramentas de desenvolvedor F12 no Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="2f47b-205">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="2f47b-206">No Internet Explorer 9, pressione **F12** para abrir as ferramentas.</span><span class="sxs-lookup"><span data-stu-id="2f47b-206">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="2f47b-207">Clique na guia **rede** e pressione **Iniciar captura**.</span><span class="sxs-lookup"><span data-stu-id="2f47b-207">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="2f47b-208">Agora, volte para a página da Web e pressione **F5** para recarregar a página da Web.</span><span class="sxs-lookup"><span data-stu-id="2f47b-208">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="2f47b-209">O Internet Explorer capturará o tráfego HTTP entre o navegador e o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="2f47b-209">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="2f47b-210">A exibição de resumo mostra todo o tráfego de rede de uma página:</span><span class="sxs-lookup"><span data-stu-id="2f47b-210">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="2f47b-211">Localize a entrada para o URI relativo "api/produtos /".</span><span class="sxs-lookup"><span data-stu-id="2f47b-211">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="2f47b-212">Selecione essa entrada e clique em **ir para exibição detalhada**.</span><span class="sxs-lookup"><span data-stu-id="2f47b-212">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="2f47b-213">Na exibição detalhada, há guias para exibir os corpos e cabeçalhos de solicitação e resposta.</span><span class="sxs-lookup"><span data-stu-id="2f47b-213">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="2f47b-214">Por exemplo, se você clicar na guia **cabeçalhos de solicitação** , poderá ver que o cliente solicitou &quot;aplicativo/JSON&quot; no cabeçalho Accept.</span><span class="sxs-lookup"><span data-stu-id="2f47b-214">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="2f47b-215">Se você clicar na guia corpo da resposta, poderá ver como a lista de produtos foi serializada para JSON.</span><span class="sxs-lookup"><span data-stu-id="2f47b-215">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="2f47b-216">Outros navegadores têm funcionalidade semelhante.</span><span class="sxs-lookup"><span data-stu-id="2f47b-216">Other browsers have similar functionality.</span></span> <span data-ttu-id="2f47b-217">Outra ferramenta útil é o [Fiddler](http://www.fiddler2.com/fiddler2/), um proxy de depuração da Web.</span><span class="sxs-lookup"><span data-stu-id="2f47b-217">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="2f47b-218">Você pode usar o Fiddler para exibir o tráfego HTTP e também para compor solicitações HTTP, o que lhe dá controle total sobre os cabeçalhos HTTP na solicitação.</span><span class="sxs-lookup"><span data-stu-id="2f47b-218">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="2f47b-219">Consulte este aplicativo em execução no Azure</span><span class="sxs-lookup"><span data-stu-id="2f47b-219">See this App Running on Azure</span></span>

<span data-ttu-id="2f47b-220">Você gostaria de ver o site concluído em execução como um aplicativo Web ativo?</span><span class="sxs-lookup"><span data-stu-id="2f47b-220">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="2f47b-221">Você pode implantar uma versão completa do aplicativo em sua conta do Azure simplesmente clicando no botão a seguir.</span><span class="sxs-lookup"><span data-stu-id="2f47b-221">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](https://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="2f47b-222">Você precisa de uma conta do Azure para implantar essa solução no Azure.</span><span class="sxs-lookup"><span data-stu-id="2f47b-222">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="2f47b-223">Se você ainda não tiver uma conta, terá as seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="2f47b-223">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="2f47b-224">[Abra uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -você obtém créditos que podem ser usados para experimentar serviços pagos do Azure e, mesmo depois que eles são usados, você pode manter a conta e usar os serviços gratuitos do Azure.</span><span class="sxs-lookup"><span data-stu-id="2f47b-224">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="2f47b-225">[Ativar os benefícios do assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -sua assinatura do MSDN fornece créditos todos os meses que você pode usar para serviços pagos do Azure.</span><span class="sxs-lookup"><span data-stu-id="2f47b-225">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2f47b-226">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="2f47b-226">Next Steps</span></span>

- <span data-ttu-id="2f47b-227">Para obter um exemplo mais completo de um serviço HTTP que dá suporte a ações POST, PUT e DELETE e gravações em um banco de dados, consulte [usando a API da Web 2 com Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span><span class="sxs-lookup"><span data-stu-id="2f47b-227">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="2f47b-228">Para obter mais informações sobre a criação de aplicativos Web fluidos e responsivos sobre um serviço HTTP, consulte [aplicativo de página única do ASP.net](../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="2f47b-228">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="2f47b-229">Para obter informações sobre como implantar um projeto Web do Visual Studio para Azure App serviço, consulte [criar um aplicativo web ASP.net no serviço Azure app](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="2f47b-229">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
