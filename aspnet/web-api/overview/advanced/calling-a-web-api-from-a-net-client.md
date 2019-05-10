---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Chamar uma API Web de um cliente .NET (C#)-ASP.NET 4.x
author: MikeWasson
description: Este tutorial mostra como chamar uma API da web de um aplicativo do .NET 4.x.
ms.author: riande
ms.date: 11/24/2017
ms.custom: seoapril2019
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: ca3b9424f30f48c7b8c71b850ffeca64244b123b
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65112839"
---
# <a name="call-a-web-api-from-a-net-client-c"></a><span data-ttu-id="d78e7-103">Chamar uma API Web de um cliente .NET (c#)</span><span class="sxs-lookup"><span data-stu-id="d78e7-103">Call a Web API From a .NET Client (C#)</span></span>

<span data-ttu-id="d78e7-104">por [Mike Wasson](https://github.com/MikeWasson) e [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d78e7-104">by [Mike Wasson](https://github.com/MikeWasson) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d78e7-105">[Baixe o projeto concluído](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span><span class="sxs-lookup"><span data-stu-id="d78e7-105">[Download Completed Project](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample).</span></span> <span data-ttu-id="d78e7-106">[Instruções de download](/aspnet/core/tutorials/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d78e7-106">[Download instructions](/aspnet/core/tutorials/#how-to-download-a-sample).</span></span> 

<span data-ttu-id="d78e7-107">Este tutorial mostra como chamar uma API da web de um aplicativo .NET, usando [HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span><span class="sxs-lookup"><span data-stu-id="d78e7-107">This tutorial shows how to call a web API from a .NET application, using [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)</span></span>

<span data-ttu-id="d78e7-108">Neste tutorial, um aplicativo cliente é escrito que consome a API da web seguinte:</span><span class="sxs-lookup"><span data-stu-id="d78e7-108">In this tutorial, a client app is written that consumes the following web API:</span></span>

| <span data-ttu-id="d78e7-109">Ação</span><span class="sxs-lookup"><span data-stu-id="d78e7-109">Action</span></span> | <span data-ttu-id="d78e7-110">Método HTTP</span><span class="sxs-lookup"><span data-stu-id="d78e7-110">HTTP method</span></span> | <span data-ttu-id="d78e7-111">URI relativo</span><span class="sxs-lookup"><span data-stu-id="d78e7-111">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="d78e7-112">Obter um produto por ID</span><span class="sxs-lookup"><span data-stu-id="d78e7-112">Get a product by ID</span></span> | <span data-ttu-id="d78e7-113">OBTER</span><span class="sxs-lookup"><span data-stu-id="d78e7-113">GET</span></span> | <span data-ttu-id="d78e7-114">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="d78e7-114">/api/products/*id*</span></span> |
| <span data-ttu-id="d78e7-115">Criar um novo produto</span><span class="sxs-lookup"><span data-stu-id="d78e7-115">Create a new product</span></span> | <span data-ttu-id="d78e7-116">POSTAR</span><span class="sxs-lookup"><span data-stu-id="d78e7-116">POST</span></span> | <span data-ttu-id="d78e7-117">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="d78e7-117">/api/products</span></span> |
| <span data-ttu-id="d78e7-118">Atualizar um produto</span><span class="sxs-lookup"><span data-stu-id="d78e7-118">Update a product</span></span> | <span data-ttu-id="d78e7-119">PUT</span><span class="sxs-lookup"><span data-stu-id="d78e7-119">PUT</span></span> | <span data-ttu-id="d78e7-120">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="d78e7-120">/api/products/*id*</span></span> |
| <span data-ttu-id="d78e7-121">Excluir um produto</span><span class="sxs-lookup"><span data-stu-id="d78e7-121">Delete a product</span></span> | <span data-ttu-id="d78e7-122">DELETE</span><span class="sxs-lookup"><span data-stu-id="d78e7-122">DELETE</span></span> | <span data-ttu-id="d78e7-123">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="d78e7-123">/api/products/*id*</span></span> |

<span data-ttu-id="d78e7-124">Para saber como implementar essa API com a API Web ASP.NET, consulte [criando uma API da Web que dá suporte a operações de CRUD](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span><span class="sxs-lookup"><span data-stu-id="d78e7-124">To learn how to implement this API with ASP.NET Web API, see [Creating a Web API that Supports CRUD Operations](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).</span></span>

<span data-ttu-id="d78e7-125">Para simplificar, o aplicativo de cliente neste tutorial é um aplicativo de console do Windows.</span><span class="sxs-lookup"><span data-stu-id="d78e7-125">For simplicity, the client application in this tutorial is a Windows console application.</span></span> <span data-ttu-id="d78e7-126">**HttpClient** também há suporte para aplicativos do Windows Phone e Windows Store.</span><span class="sxs-lookup"><span data-stu-id="d78e7-126">**HttpClient** is also supported for Windows Phone and Windows Store apps.</span></span> <span data-ttu-id="d78e7-127">Para obter mais informações, consulte [escrevendo código de cliente de API da Web para várias plataformas usando bibliotecas portáteis](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span><span class="sxs-lookup"><span data-stu-id="d78e7-127">For more information, see [Writing Web API Client Code for Multiple Platforms Using Portable Libraries](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)</span></span>

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a><span data-ttu-id="d78e7-128">Criar o aplicativo de Console</span><span class="sxs-lookup"><span data-stu-id="d78e7-128">Create the Console Application</span></span>

<span data-ttu-id="d78e7-129">No Visual Studio, crie um novo aplicativo de console do Windows chamado **HttpClientSample** e cole no código a seguir:</span><span class="sxs-lookup"><span data-stu-id="d78e7-129">In Visual Studio, create a new Windows console app named **HttpClientSample** and paste in the following code:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

<span data-ttu-id="d78e7-130">O código anterior é o aplicativo de cliente completa.</span><span class="sxs-lookup"><span data-stu-id="d78e7-130">The preceding code is the complete client app.</span></span>

<span data-ttu-id="d78e7-131">`RunAsync` execuções e bloqueia até que ela seja concluída.</span><span class="sxs-lookup"><span data-stu-id="d78e7-131">`RunAsync` runs and blocks until it completes.</span></span> <span data-ttu-id="d78e7-132">A maioria dos **HttpClient** métodos são assíncronos, porque elas executam e/s de rede.</span><span class="sxs-lookup"><span data-stu-id="d78e7-132">Most **HttpClient** methods are async, because they perform network I/O.</span></span> <span data-ttu-id="d78e7-133">Todas as tarefas de async terminar dentro `RunAsync`.</span><span class="sxs-lookup"><span data-stu-id="d78e7-133">All of the async tasks are done inside `RunAsync`.</span></span> <span data-ttu-id="d78e7-134">Normalmente um aplicativo não bloqueia o thread principal, mas este aplicativo não permite que qualquer interação.</span><span class="sxs-lookup"><span data-stu-id="d78e7-134">Normally an app doesn't block the main thread, but this app doesn't allow any interaction.</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a><span data-ttu-id="d78e7-135">Instalar as bibliotecas de cliente da API da Web</span><span class="sxs-lookup"><span data-stu-id="d78e7-135">Install the Web API Client Libraries</span></span>

<span data-ttu-id="d78e7-136">Use o Gerenciador de pacotes de NuGet para instalar o pacote de bibliotecas de cliente de API da Web.</span><span class="sxs-lookup"><span data-stu-id="d78e7-136">Use NuGet Package Manager to install the Web API Client Libraries package.</span></span>

<span data-ttu-id="d78e7-137">No menu **Ferramentas**, selecione **Gerenciador de pacotes NuGet** > **Console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="d78e7-137">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span> <span data-ttu-id="d78e7-138">No pacote Manager Console (PMC), digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="d78e7-138">In the Package Manager Console (PMC), type the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.Client`

<span data-ttu-id="d78e7-139">O comando anterior adiciona os seguintes pacotes NuGet ao projeto:</span><span class="sxs-lookup"><span data-stu-id="d78e7-139">The preceding command adds the following NuGet packages to the project:</span></span>

* <span data-ttu-id="d78e7-140">Microsoft.AspNet.WebApi.Client</span><span class="sxs-lookup"><span data-stu-id="d78e7-140">Microsoft.AspNet.WebApi.Client</span></span>
* <span data-ttu-id="d78e7-141">Newtonsoft.Json</span><span class="sxs-lookup"><span data-stu-id="d78e7-141">Newtonsoft.Json</span></span>

<span data-ttu-id="d78e7-142">Json.NET é uma estrutura popular de JSON de alto desempenho para .NET.</span><span class="sxs-lookup"><span data-stu-id="d78e7-142">Json.NET is a popular high-performance JSON framework for .NET.</span></span>

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a><span data-ttu-id="d78e7-143">Adicionar uma classe de modelo</span><span class="sxs-lookup"><span data-stu-id="d78e7-143">Add a Model Class</span></span>

<span data-ttu-id="d78e7-144">Examine a classe `Product`:</span><span class="sxs-lookup"><span data-stu-id="d78e7-144">Examine the `Product` class:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

<span data-ttu-id="d78e7-145">Essa classe corresponde ao modelo de dados usado pela API da web.</span><span class="sxs-lookup"><span data-stu-id="d78e7-145">This class matches the data model used by the web API.</span></span> <span data-ttu-id="d78e7-146">Um aplicativo pode usar **HttpClient** para ler um `Product` instância de uma resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="d78e7-146">An app can use **HttpClient** to read a `Product` instance from an HTTP response.</span></span> <span data-ttu-id="d78e7-147">O aplicativo não precisa escrever nenhum código de desserialização.</span><span class="sxs-lookup"><span data-stu-id="d78e7-147">The app doesn't have to write any deserialization code.</span></span>

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a><span data-ttu-id="d78e7-148">Criar e inicializar o HttpClient</span><span class="sxs-lookup"><span data-stu-id="d78e7-148">Create and Initialize HttpClient</span></span>

<span data-ttu-id="d78e7-149">Examinar estático **HttpClient** propriedade:</span><span class="sxs-lookup"><span data-stu-id="d78e7-149">Examine the static **HttpClient** property:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

<span data-ttu-id="d78e7-150">**HttpClient** é se destina a ser instanciado uma vez e reutilizado durante a vida útil de um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="d78e7-150">**HttpClient** is intended to be instantiated once and reused throughout the life of an application.</span></span> <span data-ttu-id="d78e7-151">As condições a seguir podem resultar em **SocketException** erros:</span><span class="sxs-lookup"><span data-stu-id="d78e7-151">The following conditions can result in **SocketException** errors:</span></span>

* <span data-ttu-id="d78e7-152">Criando um novo **HttpClient** instância por solicitação.</span><span class="sxs-lookup"><span data-stu-id="d78e7-152">Creating a new **HttpClient** instance per request.</span></span>
* <span data-ttu-id="d78e7-153">Servidor sob carga pesada.</span><span class="sxs-lookup"><span data-stu-id="d78e7-153">Server under heavy load.</span></span>

<span data-ttu-id="d78e7-154">Criando um novo **HttpClient** instância por solicitação pode esgotar os soquetes disponíveis.</span><span class="sxs-lookup"><span data-stu-id="d78e7-154">Creating a new **HttpClient** instance per request can exhaust the available sockets.</span></span>

<span data-ttu-id="d78e7-155">O código a seguir inicializa o **HttpClient** instância:</span><span class="sxs-lookup"><span data-stu-id="d78e7-155">The following code initializes the **HttpClient** instance:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

<span data-ttu-id="d78e7-156">O código anterior:</span><span class="sxs-lookup"><span data-stu-id="d78e7-156">The preceding code:</span></span>

* <span data-ttu-id="d78e7-157">Define o URI de base para solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="d78e7-157">Sets the base URI for HTTP requests.</span></span> <span data-ttu-id="d78e7-158">Altere o número da porta para a porta usada no aplicativo do servidor.</span><span class="sxs-lookup"><span data-stu-id="d78e7-158">Change the port number to the port used in the server app.</span></span> <span data-ttu-id="d78e7-159">O aplicativo não funcionará a menos que a porta para o aplicativo de servidor é usada.</span><span class="sxs-lookup"><span data-stu-id="d78e7-159">The app won't work unless port for the server app is used.</span></span>
* <span data-ttu-id="d78e7-160">Define o cabeçalho Accept, como "application/json".</span><span class="sxs-lookup"><span data-stu-id="d78e7-160">Sets the Accept header to "application/json".</span></span> <span data-ttu-id="d78e7-161">Definir esse cabeçalho informa ao servidor para enviar dados em formato JSON.</span><span class="sxs-lookup"><span data-stu-id="d78e7-161">Setting this header tells the server to send data in JSON format.</span></span>

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a><span data-ttu-id="d78e7-162">Envie uma solicitação GET para recuperar um recurso</span><span class="sxs-lookup"><span data-stu-id="d78e7-162">Send a GET request to retrieve a resource</span></span>

<span data-ttu-id="d78e7-163">O código a seguir envia uma solicitação GET para um produto:</span><span class="sxs-lookup"><span data-stu-id="d78e7-163">The following code sends a GET request for a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

<span data-ttu-id="d78e7-164">O **GetAsync** método envia a solicitação HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="d78e7-164">The **GetAsync** method sends the HTTP GET request.</span></span> <span data-ttu-id="d78e7-165">Quando o método for concluído, ele retorna um **HttpResponseMessage** que contém a resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="d78e7-165">When the method completes, it returns an **HttpResponseMessage** that contains the HTTP response.</span></span> <span data-ttu-id="d78e7-166">Se o código de status na resposta é um código de êxito, o corpo da resposta contém a representação JSON de um produto.</span><span class="sxs-lookup"><span data-stu-id="d78e7-166">If the status code in the response is a success code, the response body contains the JSON representation of a product.</span></span> <span data-ttu-id="d78e7-167">Chame **ReadAsAsync** desserializar o conteúdo JSON para um `Product` instância.</span><span class="sxs-lookup"><span data-stu-id="d78e7-167">Call **ReadAsAsync** to deserialize the JSON payload to a `Product` instance.</span></span> <span data-ttu-id="d78e7-168">O **ReadAsAsync** método é assíncrono, como o corpo da resposta pode ser arbitrariamente grande.</span><span class="sxs-lookup"><span data-stu-id="d78e7-168">The **ReadAsAsync** method is asynchronous because the response body can be arbitrarily large.</span></span>

<span data-ttu-id="d78e7-169">**HttpClient** não lança uma exceção quando a resposta HTTP contém um código de erro.</span><span class="sxs-lookup"><span data-stu-id="d78e7-169">**HttpClient** does not throw an exception when the HTTP response contains an error code.</span></span> <span data-ttu-id="d78e7-170">Em vez disso, o **IsSuccessStatusCode** é de propriedade **falso** se o status é um código de erro.</span><span class="sxs-lookup"><span data-stu-id="d78e7-170">Instead, the **IsSuccessStatusCode** property is **false** if the status is an error code.</span></span> <span data-ttu-id="d78e7-171">Se você preferir tratar os códigos de erro HTTP como exceções, chame [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) no objeto de resposta.</span><span class="sxs-lookup"><span data-stu-id="d78e7-171">If you prefer to treat HTTP error codes as exceptions, call [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) on the response object.</span></span> <span data-ttu-id="d78e7-172">`EnsureSuccessStatusCode` gera uma exceção se o código de status fica fora do intervalo de 200&ndash;299.</span><span class="sxs-lookup"><span data-stu-id="d78e7-172">`EnsureSuccessStatusCode` throws an exception if the status code falls outside the range 200&ndash;299.</span></span> <span data-ttu-id="d78e7-173">Observe que **HttpClient** pode gerar exceções por outros motivos &mdash; por exemplo, se a solicitação expira.</span><span class="sxs-lookup"><span data-stu-id="d78e7-173">Note that **HttpClient** can throw exceptions for other reasons &mdash; for example, if the request times out.</span></span>

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a><span data-ttu-id="d78e7-174">Formatadores de tipo de mídia a ser desserializado</span><span class="sxs-lookup"><span data-stu-id="d78e7-174">Media-Type Formatters to Deserialize</span></span>

<span data-ttu-id="d78e7-175">Quando **ReadAsAsync** é chamado sem parâmetros, ele usa um conjunto padrão de *formatadores de mídia* para ler o corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="d78e7-175">When **ReadAsAsync** is called with no parameters, it uses a default set of *media formatters* to read the response body.</span></span> <span data-ttu-id="d78e7-176">Os formatadores padrão dão suporte a JSON, XML e dados codificados de url do formulário.</span><span class="sxs-lookup"><span data-stu-id="d78e7-176">The default formatters support JSON, XML, and Form-url-encoded data.</span></span>

<span data-ttu-id="d78e7-177">Em vez de usar os formatadores de padrão, você pode fornecer uma lista de formatadores para o **ReadAsAsync** método.</span><span class="sxs-lookup"><span data-stu-id="d78e7-177">Instead of using the default formatters, you can provide a list of formatters to the **ReadAsAsync** method.</span></span>  <span data-ttu-id="d78e7-178">Usando uma lista de formatadores é útil se você tiver um formatador personalizado do tipo de mídia:</span><span class="sxs-lookup"><span data-stu-id="d78e7-178">Using a list of formatters is useful if you have a custom media-type formatter:</span></span>

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

<span data-ttu-id="d78e7-179">Para obter mais informações, consulte [formatadores de mídia na API Web ASP.NET 2](../formats-and-model-binding/media-formatters.md)</span><span class="sxs-lookup"><span data-stu-id="d78e7-179">For more information, see [Media Formatters in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)</span></span>

## <a name="sending-a-post-request-to-create-a-resource"></a><span data-ttu-id="d78e7-180">Enviar uma solicitação POST para criar um recurso</span><span class="sxs-lookup"><span data-stu-id="d78e7-180">Sending a POST Request to Create a Resource</span></span>

<span data-ttu-id="d78e7-181">O código a seguir envia uma solicitação POST que contém um `Product` instância no formato JSON:</span><span class="sxs-lookup"><span data-stu-id="d78e7-181">The following code sends a POST request that contains a `Product` instance in JSON format:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

<span data-ttu-id="d78e7-182">O **PostAsJsonAsync** método:</span><span class="sxs-lookup"><span data-stu-id="d78e7-182">The **PostAsJsonAsync** method:</span></span>

* <span data-ttu-id="d78e7-183">Serializa um objeto em JSON.</span><span class="sxs-lookup"><span data-stu-id="d78e7-183">Serializes an object to JSON.</span></span>
* <span data-ttu-id="d78e7-184">Envia o conteúdo JSON em uma solicitação POST.</span><span class="sxs-lookup"><span data-stu-id="d78e7-184">Sends the JSON payload in a POST request.</span></span>

<span data-ttu-id="d78e7-185">Se a solicitação for bem-sucedida:</span><span class="sxs-lookup"><span data-stu-id="d78e7-185">If the request succeeds:</span></span>

* <span data-ttu-id="d78e7-186">Ele deverá retornar uma resposta de 201 (criado).</span><span class="sxs-lookup"><span data-stu-id="d78e7-186">It should return a 201 (Created) response.</span></span>
* <span data-ttu-id="d78e7-187">A resposta deve incluir a URL, os recursos criados no cabeçalho Location.</span><span class="sxs-lookup"><span data-stu-id="d78e7-187">The response should include the URL of the created resources in the Location header.</span></span>

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a><span data-ttu-id="d78e7-188">Enviar uma solicitação PUT para atualizar um recurso</span><span class="sxs-lookup"><span data-stu-id="d78e7-188">Sending a PUT Request to Update a Resource</span></span>

<span data-ttu-id="d78e7-189">O código a seguir envia uma solicitação PUT para atualizar um produto:</span><span class="sxs-lookup"><span data-stu-id="d78e7-189">The following code sends a PUT request to update a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

<span data-ttu-id="d78e7-190">O **PutAsJsonAsync** método funciona como **PostAsJsonAsync**, exceto que ele envia uma solicitação PUT, em vez de POSTAGEM.</span><span class="sxs-lookup"><span data-stu-id="d78e7-190">The **PutAsJsonAsync** method works like **PostAsJsonAsync**, except that it sends a PUT request instead of POST.</span></span>

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a><span data-ttu-id="d78e7-191">Enviar uma solicitação DELETE para excluir um recurso</span><span class="sxs-lookup"><span data-stu-id="d78e7-191">Sending a DELETE Request to Delete a Resource</span></span>

<span data-ttu-id="d78e7-192">O código a seguir envia uma solicitação DELETE para excluir um produto:</span><span class="sxs-lookup"><span data-stu-id="d78e7-192">The following code sends a DELETE request to delete a product:</span></span>

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

<span data-ttu-id="d78e7-193">Como GET, uma solicitação de exclusão não tem um corpo de solicitação.</span><span class="sxs-lookup"><span data-stu-id="d78e7-193">Like GET, a DELETE request does not have a request body.</span></span> <span data-ttu-id="d78e7-194">Você não precisa especificar o formato JSON ou XML com a exclusão.</span><span class="sxs-lookup"><span data-stu-id="d78e7-194">You don't need to specify JSON or XML format with DELETE.</span></span>

## <a name="test-the-sample"></a><span data-ttu-id="d78e7-195">O exemplo de teste</span><span class="sxs-lookup"><span data-stu-id="d78e7-195">Test the sample</span></span>

<span data-ttu-id="d78e7-196">Para testar o aplicativo cliente:</span><span class="sxs-lookup"><span data-stu-id="d78e7-196">To test the client app:</span></span>

1. <span data-ttu-id="d78e7-197">[Baixar](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) e executar o aplicativo de servidor.</span><span class="sxs-lookup"><span data-stu-id="d78e7-197">[Download](https://github.com/aspnet/AspNetDocs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) and run the server app.</span></span> <span data-ttu-id="d78e7-198">[Instruções de download](/aspnet/core/#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d78e7-198">[Download instructions](/aspnet/core/#how-to-download-a-sample).</span></span> <span data-ttu-id="d78e7-199">Verifique se que o aplicativo de servidor está funcionando.</span><span class="sxs-lookup"><span data-stu-id="d78e7-199">Verify the server app is working.</span></span> <span data-ttu-id="d78e7-200">Por exemplo, `http://localhost:64195/api/products` deve retornar uma lista de produtos.</span><span class="sxs-lookup"><span data-stu-id="d78e7-200">For example, `http://localhost:64195/api/products` should return a list of products.</span></span>
2. <span data-ttu-id="d78e7-201">Defina o URI de base para solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="d78e7-201">Set the base URI for HTTP requests.</span></span> <span data-ttu-id="d78e7-202">Altere o número da porta para a porta usada no aplicativo do servidor.</span><span class="sxs-lookup"><span data-stu-id="d78e7-202">Change the port number to the port used in the server app.</span></span>
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. <span data-ttu-id="d78e7-203">Execute o aplicativo cliente.</span><span class="sxs-lookup"><span data-stu-id="d78e7-203">Run the client app.</span></span> <span data-ttu-id="d78e7-204">A saída a seguir será produzida:</span><span class="sxs-lookup"><span data-stu-id="d78e7-204">The following output is produced:</span></span>

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
