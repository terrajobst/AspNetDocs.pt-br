---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Chamando um serviço OData de um cliente .NET (C#) | Microsoft Docs
author: MikeWasson
description: Este tutorial mostra como chamar um serviço OData de um C# aplicativo cliente. Versões de software usadas no tutorial Visual Studio 2013 (funciona com Visual S...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 6a289fcb843634eeeefef1e0767e04e0be8b6973
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600371"
---
# <a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="2c370-104">Chamar um serviço OData em um cliente .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="2c370-104">Calling an OData Service From a .NET Client (C#)</span></span>

<span data-ttu-id="2c370-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2c370-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2c370-106">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="2c370-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="2c370-107">Este tutorial mostra como chamar um serviço OData de um C# aplicativo cliente.</span><span class="sxs-lookup"><span data-stu-id="2c370-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2c370-108">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="2c370-108">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="2c370-109">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (funciona com o Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="2c370-109">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="2c370-110">[WCF Data Services Client Library](https://msdn.microsoft.com/library/cc668772.aspx) (Biblioteca de clientes do WCF Data Services)</span><span class="sxs-lookup"><span data-stu-id="2c370-110">[WCF Data Services Client Library](https://msdn.microsoft.com/library/cc668772.aspx)</span></span>
> - <span data-ttu-id="2c370-111">API Web 2.</span><span class="sxs-lookup"><span data-stu-id="2c370-111">Web API 2.</span></span> <span data-ttu-id="2c370-112">(O exemplo de serviço OData é criado usando a API da Web 2, mas o aplicativo cliente não depende da API da Web.)</span><span class="sxs-lookup"><span data-stu-id="2c370-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>

<span data-ttu-id="2c370-113">Neste tutorial, examinarei a criação de um aplicativo cliente que chama um serviço OData.</span><span class="sxs-lookup"><span data-stu-id="2c370-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="2c370-114">O serviço OData expõe as seguintes entidades:</span><span class="sxs-lookup"><span data-stu-id="2c370-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="2c370-115">Os artigos a seguir descrevem como implementar o serviço OData na API da Web.</span><span class="sxs-lookup"><span data-stu-id="2c370-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="2c370-116">(No entanto, você não precisa lê-los para entender este tutorial.)</span><span class="sxs-lookup"><span data-stu-id="2c370-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="2c370-117">Criando um ponto de extremidade OData na API Web 2</span><span class="sxs-lookup"><span data-stu-id="2c370-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="2c370-118">Relações de entidade OData na API Web 2</span><span class="sxs-lookup"><span data-stu-id="2c370-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="2c370-119">Ações de OData na API Web 2</span><span class="sxs-lookup"><span data-stu-id="2c370-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="2c370-120">Gerar o proxy de serviço</span><span class="sxs-lookup"><span data-stu-id="2c370-120">Generate the Service Proxy</span></span>

<span data-ttu-id="2c370-121">A primeira etapa é gerar um proxy de serviço.</span><span class="sxs-lookup"><span data-stu-id="2c370-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="2c370-122">O proxy de serviço é uma classe .NET que define métodos para acessar o serviço OData.</span><span class="sxs-lookup"><span data-stu-id="2c370-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="2c370-123">O proxy traduz as chamadas de método em solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="2c370-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="2c370-124">Comece abrindo o projeto de serviço OData no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2c370-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="2c370-125">Pressione CTRL + F5 para executar o serviço localmente no IIS Express.</span><span class="sxs-lookup"><span data-stu-id="2c370-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="2c370-126">Observe o endereço local, incluindo o número da porta que o Visual Studio atribui.</span><span class="sxs-lookup"><span data-stu-id="2c370-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="2c370-127">Você precisará desse endereço ao criar o proxy.</span><span class="sxs-lookup"><span data-stu-id="2c370-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="2c370-128">Em seguida, abra outra instância do Visual Studio e crie um projeto de aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="2c370-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="2c370-129">O aplicativo de console será nosso aplicativo cliente OData.</span><span class="sxs-lookup"><span data-stu-id="2c370-129">The console application will be our OData client application.</span></span> <span data-ttu-id="2c370-130">(Você também pode adicionar o projeto à mesma solução que o serviço.)</span><span class="sxs-lookup"><span data-stu-id="2c370-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="2c370-131">As etapas restantes referem-se ao projeto de console.</span><span class="sxs-lookup"><span data-stu-id="2c370-131">The remaining steps refer the console project.</span></span>

<span data-ttu-id="2c370-132">Em Gerenciador de Soluções, clique com o botão direito do mouse em **referências** e selecione **Adicionar referência de serviço**.</span><span class="sxs-lookup"><span data-stu-id="2c370-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="2c370-133">Na caixa de diálogo **Adicionar referência de serviço** , digite o endereço do serviço OData:</span><span class="sxs-lookup"><span data-stu-id="2c370-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="2c370-134">em que *Port* é o número da porta.</span><span class="sxs-lookup"><span data-stu-id="2c370-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="2c370-135">Para **namespace**, digite "ProductService".</span><span class="sxs-lookup"><span data-stu-id="2c370-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="2c370-136">Essa opção define o namespace da classe proxy.</span><span class="sxs-lookup"><span data-stu-id="2c370-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="2c370-137">Clique em **ir**.</span><span class="sxs-lookup"><span data-stu-id="2c370-137">Click **Go**.</span></span> <span data-ttu-id="2c370-138">O Visual Studio lê o documento de metadados OData para descobrir as entidades no serviço.</span><span class="sxs-lookup"><span data-stu-id="2c370-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="2c370-139">Clique em **OK** para adicionar a classe de proxy ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="2c370-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="2c370-140">Criar uma instância da classe de proxy de serviço</span><span class="sxs-lookup"><span data-stu-id="2c370-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="2c370-141">Dentro de seu método `Main`, crie uma nova instância da classe proxy, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="2c370-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="2c370-142">Novamente, use o número da porta real em que o serviço está em execução.</span><span class="sxs-lookup"><span data-stu-id="2c370-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="2c370-143">Ao implantar seu serviço, você usará o URI do serviço ativo.</span><span class="sxs-lookup"><span data-stu-id="2c370-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="2c370-144">Você não precisa atualizar o proxy.</span><span class="sxs-lookup"><span data-stu-id="2c370-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="2c370-145">O código a seguir adiciona um manipulador de eventos que imprime os URIs de solicitação na janela do console.</span><span class="sxs-lookup"><span data-stu-id="2c370-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="2c370-146">Essa etapa não é necessária, mas é interessante ver os URIs de cada consulta.</span><span class="sxs-lookup"><span data-stu-id="2c370-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="2c370-147">Consultar o serviço</span><span class="sxs-lookup"><span data-stu-id="2c370-147">Query the Service</span></span>

<span data-ttu-id="2c370-148">O código a seguir obtém a lista de produtos do serviço OData.</span><span class="sxs-lookup"><span data-stu-id="2c370-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="2c370-149">Observe que você não precisa escrever nenhum código para enviar a solicitação HTTP ou analisar a resposta.</span><span class="sxs-lookup"><span data-stu-id="2c370-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="2c370-150">A classe proxy faz isso automaticamente quando você enumera a coleção de `Container.Products` no loop **foreach** .</span><span class="sxs-lookup"><span data-stu-id="2c370-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="2c370-151">Quando você executa o aplicativo, a saída deve ser semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="2c370-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="2c370-152">Para obter uma entidade por ID, use uma cláusula `where`.</span><span class="sxs-lookup"><span data-stu-id="2c370-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="2c370-153">Para o restante deste tópico, não mostrarei toda a função de `Main`, apenas o código necessário para chamar o serviço.</span><span class="sxs-lookup"><span data-stu-id="2c370-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="2c370-154">Aplicar opções de consulta</span><span class="sxs-lookup"><span data-stu-id="2c370-154">Apply Query Options</span></span>

<span data-ttu-id="2c370-155">O OData define [Opções de consulta](../supporting-odata-query-options.md) que podem ser usadas para filtrar, classificar, paginar dados e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="2c370-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="2c370-156">No proxy de serviço, você pode aplicar essas opções usando várias expressões LINQ.</span><span class="sxs-lookup"><span data-stu-id="2c370-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="2c370-157">Nesta seção, mostrarei exemplos breves.</span><span class="sxs-lookup"><span data-stu-id="2c370-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="2c370-158">Para obter mais detalhes, consulte o tópico [Considerações sobre LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) no msdn.</span><span class="sxs-lookup"><span data-stu-id="2c370-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="2c370-159">Filtragem ($filter)</span><span class="sxs-lookup"><span data-stu-id="2c370-159">Filtering ($filter)</span></span>

<span data-ttu-id="2c370-160">Para filtrar, use uma cláusula `where`.</span><span class="sxs-lookup"><span data-stu-id="2c370-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="2c370-161">O exemplo a seguir filtra por categoria de produto.</span><span class="sxs-lookup"><span data-stu-id="2c370-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="2c370-162">Esse código corresponde à consulta OData a seguir.</span><span class="sxs-lookup"><span data-stu-id="2c370-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="2c370-163">Observe que o proxy converte a cláusula `where` em uma expressão de `$filter` OData.</span><span class="sxs-lookup"><span data-stu-id="2c370-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="2c370-164">Classificação ($orderby)</span><span class="sxs-lookup"><span data-stu-id="2c370-164">Sorting ($orderby)</span></span>

<span data-ttu-id="2c370-165">Para classificar, use uma cláusula `orderby`.</span><span class="sxs-lookup"><span data-stu-id="2c370-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="2c370-166">O exemplo a seguir classifica por preço, do mais alto para o mais baixo.</span><span class="sxs-lookup"><span data-stu-id="2c370-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="2c370-167">Aqui está a solicitação de OData correspondente.</span><span class="sxs-lookup"><span data-stu-id="2c370-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="2c370-168">Paginação do lado do cliente ($skip e $top)</span><span class="sxs-lookup"><span data-stu-id="2c370-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="2c370-169">Para conjuntos de entidades grandes, o cliente pode querer limitar o número de resultados.</span><span class="sxs-lookup"><span data-stu-id="2c370-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="2c370-170">Por exemplo, um cliente pode mostrar 10 entradas por vez.</span><span class="sxs-lookup"><span data-stu-id="2c370-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="2c370-171">Isso é chamado *de paginação do lado do cliente*.</span><span class="sxs-lookup"><span data-stu-id="2c370-171">This is called *client-side paging*.</span></span> <span data-ttu-id="2c370-172">(Também há uma [paginação do servidor](../supporting-odata-query-options.md#server-paging), onde o servidor limita o número de resultados.) Para executar a paginação do lado do cliente, use os métodos **Skip** e **Take** do LINQ.</span><span class="sxs-lookup"><span data-stu-id="2c370-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="2c370-173">O exemplo a seguir ignora os primeiros 40 resultados e usa os próximos 10.</span><span class="sxs-lookup"><span data-stu-id="2c370-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="2c370-174">Aqui está a solicitação de OData correspondente:</span><span class="sxs-lookup"><span data-stu-id="2c370-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="2c370-175">Select ($select) e Expand ($expand)</span><span class="sxs-lookup"><span data-stu-id="2c370-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="2c370-176">Para incluir entidades relacionadas, use o método `DataServiceQuery<t>.Expand`.</span><span class="sxs-lookup"><span data-stu-id="2c370-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="2c370-177">Por exemplo, para incluir o `Supplier` para cada `Product`:</span><span class="sxs-lookup"><span data-stu-id="2c370-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="2c370-178">Aqui está a solicitação de OData correspondente:</span><span class="sxs-lookup"><span data-stu-id="2c370-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="2c370-179">Para alterar a forma da resposta, use a cláusula **Select** do LINQ.</span><span class="sxs-lookup"><span data-stu-id="2c370-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="2c370-180">O exemplo a seguir obtém apenas o nome de cada produto, sem outras propriedades.</span><span class="sxs-lookup"><span data-stu-id="2c370-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="2c370-181">Aqui está a solicitação de OData correspondente:</span><span class="sxs-lookup"><span data-stu-id="2c370-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="2c370-182">Uma cláusula SELECT pode incluir entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="2c370-182">A select clause can include related entities.</span></span> <span data-ttu-id="2c370-183">Nesse caso, não chame **expandir**; o proxy inclui automaticamente a expansão nesse caso.</span><span class="sxs-lookup"><span data-stu-id="2c370-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="2c370-184">O exemplo a seguir obtém o nome e o fornecedor de cada produto.</span><span class="sxs-lookup"><span data-stu-id="2c370-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="2c370-185">Aqui está a solicitação de OData correspondente.</span><span class="sxs-lookup"><span data-stu-id="2c370-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="2c370-186">Observe que ele inclui a opção **$Expand** .</span><span class="sxs-lookup"><span data-stu-id="2c370-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="2c370-187">Para obter mais informações sobre $select e $expand, consulte [usando $Select, $Expand e $value na API Web 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="2c370-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="2c370-188">Adicionar uma nova entidade</span><span class="sxs-lookup"><span data-stu-id="2c370-188">Add a New Entity</span></span>

<span data-ttu-id="2c370-189">Para adicionar uma nova entidade a um conjunto de entidades, chame `AddToEntitySet`, em que *EntitySet* é o nome do conjunto de entidades.</span><span class="sxs-lookup"><span data-stu-id="2c370-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="2c370-190">Por exemplo, `AddToProducts` adiciona um novo `Product` ao conjunto de entidades de `Products`.</span><span class="sxs-lookup"><span data-stu-id="2c370-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="2c370-191">Quando você gera o proxy, WCF Data Services cria automaticamente esses métodos **AddTo** fortemente tipados.</span><span class="sxs-lookup"><span data-stu-id="2c370-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="2c370-192">Para adicionar um link entre duas entidades, use os métodos **AddLink** e **SetLink** .</span><span class="sxs-lookup"><span data-stu-id="2c370-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="2c370-193">O código a seguir adiciona um novo fornecedor e um novo produto e, em seguida, cria links entre eles.</span><span class="sxs-lookup"><span data-stu-id="2c370-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="2c370-194">Use **AddLink** quando a propriedade de navegação for uma coleção.</span><span class="sxs-lookup"><span data-stu-id="2c370-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="2c370-195">Neste exemplo, estamos adicionando um produto à coleção de `Products` no fornecedor.</span><span class="sxs-lookup"><span data-stu-id="2c370-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="2c370-196">Use **SetLink** quando a propriedade de navegação for uma única entidade.</span><span class="sxs-lookup"><span data-stu-id="2c370-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="2c370-197">Neste exemplo, estamos definindo a propriedade `Supplier` no produto.</span><span class="sxs-lookup"><span data-stu-id="2c370-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="2c370-198">Atualização/patch</span><span class="sxs-lookup"><span data-stu-id="2c370-198">Update / Patch</span></span>

<span data-ttu-id="2c370-199">Para atualizar uma entidade, chame o método **UpdateObject** .</span><span class="sxs-lookup"><span data-stu-id="2c370-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="2c370-200">A atualização é executada quando você chama **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="2c370-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="2c370-201">Por padrão, o WCF envia uma solicitação de MESCLAgem HTTP.</span><span class="sxs-lookup"><span data-stu-id="2c370-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="2c370-202">A opção **PatchOnUpdate** informa ao WCF para enviar um patch http em vez disso.</span><span class="sxs-lookup"><span data-stu-id="2c370-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="2c370-203">Por que PATCH versus MESCLAgem?</span><span class="sxs-lookup"><span data-stu-id="2c370-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="2c370-204">A especificação HTTP 1,1 original ([RCF 2616](http://tools.ietf.org/html/rfc2616)) não definiu nenhum método http com a semântica "atualização parcial".</span><span class="sxs-lookup"><span data-stu-id="2c370-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="2c370-205">Para dar suporte a atualizações parciais, a especificação OData definiu o método MERGE.</span><span class="sxs-lookup"><span data-stu-id="2c370-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="2c370-206">Em 2010, a [RFC 5789](http://tools.ietf.org/html/rfc5789) definiu o método de patch para atualizações parciais.</span><span class="sxs-lookup"><span data-stu-id="2c370-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="2c370-207">Você pode ler parte do histórico nesta [postagem de blog](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) no blog WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="2c370-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="2c370-208">Hoje, o PATCH é preferencial em relação à MESCLAgem.</span><span class="sxs-lookup"><span data-stu-id="2c370-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="2c370-209">O controlador OData criado pela API Web scaffolding dá suporte a ambos os métodos.</span><span class="sxs-lookup"><span data-stu-id="2c370-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>

<span data-ttu-id="2c370-210">Se você quiser substituir toda a entidade (colocar semântica), especifique a opção **ReplaceOnUpdate** .</span><span class="sxs-lookup"><span data-stu-id="2c370-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="2c370-211">Isso faz com que o WCF envie uma solicitação HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="2c370-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="2c370-212">Excluir uma entidade</span><span class="sxs-lookup"><span data-stu-id="2c370-212">Delete an Entity</span></span>

<span data-ttu-id="2c370-213">Para excluir uma entidade, chame **ExcluirObjeto**.</span><span class="sxs-lookup"><span data-stu-id="2c370-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="2c370-214">Invocar uma ação do OData</span><span class="sxs-lookup"><span data-stu-id="2c370-214">Invoke an OData Action</span></span>

<span data-ttu-id="2c370-215">No OData, as [ações](odata-actions.md) são uma maneira de adicionar comportamentos do lado do servidor que não são facilmente definidos como operações CRUD em entidades.</span><span class="sxs-lookup"><span data-stu-id="2c370-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="2c370-216">Embora o documento de metadados OData Descreva as ações, a classe proxy não cria nenhum método fortemente tipado para elas.</span><span class="sxs-lookup"><span data-stu-id="2c370-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="2c370-217">Você ainda pode invocar uma ação OData usando o método genérico **Execute** .</span><span class="sxs-lookup"><span data-stu-id="2c370-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="2c370-218">No entanto, você precisará saber os tipos de dados dos parâmetros e o valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="2c370-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="2c370-219">Por exemplo, a ação `RateProduct` usa o parâmetro chamado "rating" do tipo `Int32` e retorna um `double`.</span><span class="sxs-lookup"><span data-stu-id="2c370-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="2c370-220">O código a seguir mostra como invocar essa ação.</span><span class="sxs-lookup"><span data-stu-id="2c370-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="2c370-221">Para obter mais informações, consulte[chamando operações e ações de serviço](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="2c370-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="2c370-222">Uma opção é estender a classe de **contêiner** para fornecer um método fortemente tipado que invoque a ação:</span><span class="sxs-lookup"><span data-stu-id="2c370-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
