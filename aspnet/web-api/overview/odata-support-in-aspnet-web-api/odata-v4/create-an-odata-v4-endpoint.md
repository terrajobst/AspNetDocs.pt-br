---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Criar um ponto de extremidade do OData v4 usando ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: O Protocolo Open Data (OData) é um protocolo de acesso a dados para a Web. O OData fornece uma maneira uniforme de consultar e manipular conjuntos de dados por meio de operações CRUD...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 81d134cbd3231b9a0d5537ccbd1bbfe6419254af
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598731"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a><span data-ttu-id="cbce7-104">Criar um ponto de extremidade do OData v4 usando ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="cbce7-104">Create an OData v4 Endpoint Using ASP.NET Web API</span></span> 

> <span data-ttu-id="cbce7-105">O Protocolo Open Data (OData) é um protocolo de acesso a dados para a Web.</span><span class="sxs-lookup"><span data-stu-id="cbce7-105">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="cbce7-106">O OData fornece uma maneira uniforme de consultar e manipular conjuntos de dados por meio de operações CRUD (criar, ler, atualizar e excluir).</span><span class="sxs-lookup"><span data-stu-id="cbce7-106">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
>
> <span data-ttu-id="cbce7-107">O ASP.NET Web API dá suporte a V3 e v4 do protocolo.</span><span class="sxs-lookup"><span data-stu-id="cbce7-107">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="cbce7-108">Você pode até mesmo ter um ponto de extremidade v4 que é executado lado a lado com um ponto de extremidade v3.</span><span class="sxs-lookup"><span data-stu-id="cbce7-108">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
>
> <span data-ttu-id="cbce7-109">Este tutorial mostra como criar um ponto de extremidade do OData v4 que oferece suporte a operações CRUD.</span><span class="sxs-lookup"><span data-stu-id="cbce7-109">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="cbce7-110">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="cbce7-110">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="cbce7-111">API Web 5,2</span><span class="sxs-lookup"><span data-stu-id="cbce7-111">Web API 5.2</span></span>
> - <span data-ttu-id="cbce7-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="cbce7-112">OData v4</span></span>
> - <span data-ttu-id="cbce7-113">Visual Studio 2017 (Baixe o Visual Studio 2017 [aqui](https://visualstudio.microsoft.com/downloads/))</span><span class="sxs-lookup"><span data-stu-id="cbce7-113">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/))</span></span>
> - <span data-ttu-id="cbce7-114">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="cbce7-114">Entity Framework 6</span></span>
> - <span data-ttu-id="cbce7-115">.NET 4.7.2</span><span class="sxs-lookup"><span data-stu-id="cbce7-115">.NET 4.7.2</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="cbce7-116">Versões do tutorial</span><span class="sxs-lookup"><span data-stu-id="cbce7-116">Tutorial versions</span></span>
>
> <span data-ttu-id="cbce7-117">Para o OData versão 3, consulte [criando um ponto de extremidade OData v3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="cbce7-117">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="cbce7-118">Criar o projeto do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cbce7-118">Create the Visual Studio Project</span></span>

<span data-ttu-id="cbce7-119">No Visual Studio, no menu **arquivo** , selecione **novo** **projeto**de &gt;.</span><span class="sxs-lookup"><span data-stu-id="cbce7-119">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="cbce7-120">Expanda **instalado** &gt;  **C# Visual** &gt; **Web**e selecione o modelo **aplicativo Web do ASP.net (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="cbce7-120">Expand **Installed** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application (.NET Framework)** template.</span></span> <span data-ttu-id="cbce7-121">Nomeie o projeto &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="cbce7-121">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

<span data-ttu-id="cbce7-122">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="cbce7-122">Select **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

<span data-ttu-id="cbce7-123">Selecione o modelo **Vazio**.</span><span class="sxs-lookup"><span data-stu-id="cbce7-123">Select the **Empty** template.</span></span> <span data-ttu-id="cbce7-124">Em **Adicionar pastas e referências principais para:** , selecione **API Web**.</span><span class="sxs-lookup"><span data-stu-id="cbce7-124">Under **Add folders and core references for:**, select **Web API**.</span></span> <span data-ttu-id="cbce7-125">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="cbce7-125">Select **OK**.</span></span>

## <a name="install-the-odata-packages"></a><span data-ttu-id="cbce7-126">Instalar os pacotes OData</span><span class="sxs-lookup"><span data-stu-id="cbce7-126">Install the OData packages</span></span>

<span data-ttu-id="cbce7-127">No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet** &gt; **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="cbce7-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="cbce7-128">Na janela do console do Gerenciador de pacotes, digite:</span><span class="sxs-lookup"><span data-stu-id="cbce7-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="cbce7-129">Esse comando instala os pacotes do NuGet do OData mais recentes.</span><span class="sxs-lookup"><span data-stu-id="cbce7-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="cbce7-130">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="cbce7-130">Add a model class</span></span>

<span data-ttu-id="cbce7-131">Um *modelo* é um objeto que representa uma entidade de dados em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cbce7-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="cbce7-132">No Gerenciador de Soluções, clique com o botão direito do mouse na pasta Modelos.</span><span class="sxs-lookup"><span data-stu-id="cbce7-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="cbce7-133">No menu de contexto, selecione **adicionar** &gt; **classe**.</span><span class="sxs-lookup"><span data-stu-id="cbce7-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="cbce7-134">Por convenção, as classes de modelo são colocadas na pasta modelos, mas você não precisa seguir essa convenção em seus próprios projetos.</span><span class="sxs-lookup"><span data-stu-id="cbce7-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>

<span data-ttu-id="cbce7-135">Nome da classe `Product`.</span><span class="sxs-lookup"><span data-stu-id="cbce7-135">Name the class `Product`.</span></span> <span data-ttu-id="cbce7-136">No arquivo Product.cs, substitua o código clichê pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="cbce7-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="cbce7-137">A propriedade `Id` é a chave de entidade.</span><span class="sxs-lookup"><span data-stu-id="cbce7-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="cbce7-138">Os clientes podem consultar entidades por chave.</span><span class="sxs-lookup"><span data-stu-id="cbce7-138">Clients can query entities by key.</span></span> <span data-ttu-id="cbce7-139">Por exemplo, para obter o produto com a ID 5, o URI é `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="cbce7-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="cbce7-140">A propriedade `Id` também será a chave primária no banco de dados back-end.</span><span class="sxs-lookup"><span data-stu-id="cbce7-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="cbce7-141">Habilitar Entity Framework</span><span class="sxs-lookup"><span data-stu-id="cbce7-141">Enable Entity Framework</span></span>

<span data-ttu-id="cbce7-142">Para este tutorial, usaremos Entity Framework (EF) Code First para criar o banco de dados back-end.</span><span class="sxs-lookup"><span data-stu-id="cbce7-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="cbce7-143">O OData da API Web não requer o EF.</span><span class="sxs-lookup"><span data-stu-id="cbce7-143">Web API OData does not require EF.</span></span> <span data-ttu-id="cbce7-144">Use qualquer camada de acesso a dados que possa converter entidades de banco de dado em modelos.</span><span class="sxs-lookup"><span data-stu-id="cbce7-144">Use any data-access layer that can translate database entities into models.</span></span>

<span data-ttu-id="cbce7-145">Primeiro, instale o pacote NuGet para o EF.</span><span class="sxs-lookup"><span data-stu-id="cbce7-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="cbce7-146">No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet** &gt; **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="cbce7-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="cbce7-147">Na janela do console do Gerenciador de pacotes, digite:</span><span class="sxs-lookup"><span data-stu-id="cbce7-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="cbce7-148">Abra o arquivo Web. config e adicione a seção a seguir dentro do elemento de **configuração** , após o elemento **configSections** .</span><span class="sxs-lookup"><span data-stu-id="cbce7-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="cbce7-149">Essa configuração adiciona uma cadeia de conexão para um banco de dados LocalDB.</span><span class="sxs-lookup"><span data-stu-id="cbce7-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="cbce7-150">Esse banco de dados será usado quando você executar o aplicativo localmente.</span><span class="sxs-lookup"><span data-stu-id="cbce7-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="cbce7-151">Em seguida, adicione uma classe chamada `ProductsContext` à pasta modelos:</span><span class="sxs-lookup"><span data-stu-id="cbce7-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="cbce7-152">No construtor, `"name=ProductsContext"` fornece o nome da cadeia de conexão.</span><span class="sxs-lookup"><span data-stu-id="cbce7-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="cbce7-153">Configurar o ponto de extremidade OData</span><span class="sxs-lookup"><span data-stu-id="cbce7-153">Configure the OData endpoint</span></span>

<span data-ttu-id="cbce7-154">Abra o aplicativo de arquivo\_Start/WebApiConfig. cs.</span><span class="sxs-lookup"><span data-stu-id="cbce7-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="cbce7-155">Adicione as seguintes instruções **using** :</span><span class="sxs-lookup"><span data-stu-id="cbce7-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="cbce7-156">Em seguida, adicione o seguinte código ao método **Register** :</span><span class="sxs-lookup"><span data-stu-id="cbce7-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="cbce7-157">Esse código faz duas coisas:</span><span class="sxs-lookup"><span data-stu-id="cbce7-157">This code does two things:</span></span>

- <span data-ttu-id="cbce7-158">Cria um Modelo de Dados de Entidade (EDM).</span><span class="sxs-lookup"><span data-stu-id="cbce7-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="cbce7-159">Adiciona uma rota.</span><span class="sxs-lookup"><span data-stu-id="cbce7-159">Adds a route.</span></span>

<span data-ttu-id="cbce7-160">Um EDM é um modelo abstrato dos dados.</span><span class="sxs-lookup"><span data-stu-id="cbce7-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="cbce7-161">O EDM é usado para criar o documento de metadados de serviço.</span><span class="sxs-lookup"><span data-stu-id="cbce7-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="cbce7-162">A classe **ODataConventionModelBuilder** cria um EDM usando as convenções de nomenclatura padrão.</span><span class="sxs-lookup"><span data-stu-id="cbce7-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="cbce7-163">Essa abordagem requer o mínimo de código.</span><span class="sxs-lookup"><span data-stu-id="cbce7-163">This approach requires the least code.</span></span> <span data-ttu-id="cbce7-164">Se você quiser mais controle sobre o EDM, poderá usar a classe **ODataModelBuilder** para criar o EDM adicionando Propriedades, chaves e propriedades de navegação explicitamente.</span><span class="sxs-lookup"><span data-stu-id="cbce7-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="cbce7-165">Uma *rota* informa à API da Web como rotear solicitações HTTP para o ponto de extremidade.</span><span class="sxs-lookup"><span data-stu-id="cbce7-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="cbce7-166">Para criar uma rota v4 do OData, chame o método de extensão **MapODataServiceRoute** .</span><span class="sxs-lookup"><span data-stu-id="cbce7-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="cbce7-167">Se seu aplicativo tiver vários pontos de extremidade OData, crie uma rota separada para cada um.</span><span class="sxs-lookup"><span data-stu-id="cbce7-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="cbce7-168">Dê a cada rota um nome e prefixo de rota exclusivos.</span><span class="sxs-lookup"><span data-stu-id="cbce7-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="cbce7-169">Adicionar o controlador OData</span><span class="sxs-lookup"><span data-stu-id="cbce7-169">Add the OData controller</span></span>

<span data-ttu-id="cbce7-170">Um *controlador* é uma classe que MANIPULA solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="cbce7-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="cbce7-171">Você cria um controlador separado para cada conjunto de entidades em seu serviço OData.</span><span class="sxs-lookup"><span data-stu-id="cbce7-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="cbce7-172">Neste tutorial, você criará um controlador para a entidade de `Product`.</span><span class="sxs-lookup"><span data-stu-id="cbce7-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="cbce7-173">Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta controladores e selecione **adicionar** &gt; **classe**.</span><span class="sxs-lookup"><span data-stu-id="cbce7-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="cbce7-174">Nome da classe `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="cbce7-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="cbce7-175">A versão deste tutorial para OData v3 usa o **Add Controller** scaffolding.</span><span class="sxs-lookup"><span data-stu-id="cbce7-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="cbce7-176">Atualmente, não há nenhum scaffolding para o OData v4.</span><span class="sxs-lookup"><span data-stu-id="cbce7-176">Currently, there is no scaffolding for OData v4.</span></span>

<span data-ttu-id="cbce7-177">Substitua o código clichê em ProductsController.cs pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="cbce7-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="cbce7-178">O controlador usa a classe `ProductsContext` para acessar o banco de dados usando o EF.</span><span class="sxs-lookup"><span data-stu-id="cbce7-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="cbce7-179">Observe que o controlador substitui o método **Dispose** para descartar o **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="cbce7-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="cbce7-180">Este é o ponto de partida para o controlador.</span><span class="sxs-lookup"><span data-stu-id="cbce7-180">This is the starting point for the controller.</span></span> <span data-ttu-id="cbce7-181">Em seguida, adicionaremos métodos para todas as operações CRUD.</span><span class="sxs-lookup"><span data-stu-id="cbce7-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="query-the-entity-set"></a><span data-ttu-id="cbce7-182">Consultar o conjunto de entidades</span><span class="sxs-lookup"><span data-stu-id="cbce7-182">Query the entity set</span></span>

<span data-ttu-id="cbce7-183">Adicione os métodos a seguir para `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="cbce7-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="cbce7-184">A versão sem parâmetros do método `Get` retorna a coleção de produtos inteira.</span><span class="sxs-lookup"><span data-stu-id="cbce7-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="cbce7-185">O método `Get` com um parâmetro de *chave* pesquisa um produto por sua chave (nesse caso, a propriedade `Id`).</span><span class="sxs-lookup"><span data-stu-id="cbce7-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="cbce7-186">O atributo **[EnableQuery]** permite que os clientes modifiquem a consulta usando opções de consulta, como $filter, $sort e $Page.</span><span class="sxs-lookup"><span data-stu-id="cbce7-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="cbce7-187">Para obter mais informações, consulte [Opções de consulta do OData de suporte](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="cbce7-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="add-an-entity-to-the-entity-set"></a><span data-ttu-id="cbce7-188">Adicionar uma entidade ao conjunto de entidades</span><span class="sxs-lookup"><span data-stu-id="cbce7-188">Add an entity to the entity set</span></span>

<span data-ttu-id="cbce7-189">Para permitir que os clientes adicionem um novo produto ao banco de dados, adicione o método a seguir para `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="cbce7-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a><span data-ttu-id="cbce7-190">Atualizar uma entidade</span><span class="sxs-lookup"><span data-stu-id="cbce7-190">Update an entity</span></span>

<span data-ttu-id="cbce7-191">O OData dá suporte a duas semânticas diferentes para atualizar uma entidade, um PATCH e um PUT.</span><span class="sxs-lookup"><span data-stu-id="cbce7-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="cbce7-192">O PATCH executa uma atualização parcial.</span><span class="sxs-lookup"><span data-stu-id="cbce7-192">PATCH performs a partial update.</span></span> <span data-ttu-id="cbce7-193">O cliente especifica apenas as propriedades a serem atualizadas.</span><span class="sxs-lookup"><span data-stu-id="cbce7-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="cbce7-194">PUT substitui a entidade inteira.</span><span class="sxs-lookup"><span data-stu-id="cbce7-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="cbce7-195">A desvantagem de PUT é que o cliente deve enviar valores para todas as propriedades na entidade, incluindo valores que não estão sendo alterados.</span><span class="sxs-lookup"><span data-stu-id="cbce7-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="cbce7-196">A [especificação do OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) declara que o patch é preferencial.</span><span class="sxs-lookup"><span data-stu-id="cbce7-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="cbce7-197">Em qualquer caso, aqui está o código para os métodos PATCH e PUT:</span><span class="sxs-lookup"><span data-stu-id="cbce7-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="cbce7-198">No caso do PATCH, o controlador usa o tipo de **&gt;de&lt;de Delta** para controlar as alterações.</span><span class="sxs-lookup"><span data-stu-id="cbce7-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="delete-an-entity"></a><span data-ttu-id="cbce7-199">Excluir uma entidade</span><span class="sxs-lookup"><span data-stu-id="cbce7-199">Delete an entity</span></span>

<span data-ttu-id="cbce7-200">Para permitir que os clientes excluam um produto do banco de dados, adicione o seguinte método a `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="cbce7-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
