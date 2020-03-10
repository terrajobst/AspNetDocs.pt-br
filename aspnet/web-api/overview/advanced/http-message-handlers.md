---
uid: web-api/overview/advanced/http-message-handlers
title: Manipuladores de mensagens HTTP em ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Uma visão geral dos manipuladores de mensagens HTTP em ASP.NET Web API para ASP.NET 4. x
ms.author: riande
ms.date: 02/13/2012
ms.custom: seoapril2019
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: a8e6f1da8df4802e1acf7779a2fc75bfe8ab876f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622580"
---
# <a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="9f13e-103">Manipuladores de mensagens HTTP no ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="9f13e-103">HTTP Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="9f13e-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9f13e-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9f13e-105">Um *manipulador de mensagens* é uma classe que recebe uma solicitação HTTP e retorna uma resposta http.</span><span class="sxs-lookup"><span data-stu-id="9f13e-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="9f13e-106">Os manipuladores de mensagens derivam da classe abstrata **HttpMessageHandler** .</span><span class="sxs-lookup"><span data-stu-id="9f13e-106">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="9f13e-107">Normalmente, uma série de manipuladores de mensagens é encadeada.</span><span class="sxs-lookup"><span data-stu-id="9f13e-107">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="9f13e-108">O primeiro manipulador recebe uma solicitação HTTP, executa algum processamento e fornece a solicitação para o próximo manipulador.</span><span class="sxs-lookup"><span data-stu-id="9f13e-108">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="9f13e-109">Em algum momento, a resposta é criada e retorna o backup da cadeia.</span><span class="sxs-lookup"><span data-stu-id="9f13e-109">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="9f13e-110">Esse padrão é chamado de manipulador de *delegação* .</span><span class="sxs-lookup"><span data-stu-id="9f13e-110">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="9f13e-111">Manipuladores de mensagens do lado do servidor</span><span class="sxs-lookup"><span data-stu-id="9f13e-111">Server-Side Message Handlers</span></span>

<span data-ttu-id="9f13e-112">No lado do servidor, o pipeline da API Web usa alguns manipuladores de mensagens internos:</span><span class="sxs-lookup"><span data-stu-id="9f13e-112">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="9f13e-113">**HttpServer** Obtém a solicitação do host.</span><span class="sxs-lookup"><span data-stu-id="9f13e-113">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="9f13e-114">**HttpRoutingDispatcher** despacha a solicitação com base na rota.</span><span class="sxs-lookup"><span data-stu-id="9f13e-114">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="9f13e-115">**HttpControllerDispatcher** envia a solicitação para um controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="9f13e-115">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="9f13e-116">Você pode adicionar manipuladores personalizados ao pipeline.</span><span class="sxs-lookup"><span data-stu-id="9f13e-116">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="9f13e-117">Os manipuladores de mensagens são bons para preocupações abrangentes que operam no nível de mensagens HTTP (em vez de ações do controlador).</span><span class="sxs-lookup"><span data-stu-id="9f13e-117">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="9f13e-118">Por exemplo, um manipulador de mensagens pode:</span><span class="sxs-lookup"><span data-stu-id="9f13e-118">For example, a message handler might:</span></span>

- <span data-ttu-id="9f13e-119">Ler ou modificar cabeçalhos de solicitação.</span><span class="sxs-lookup"><span data-stu-id="9f13e-119">Read or modify request headers.</span></span>
- <span data-ttu-id="9f13e-120">Adicione um cabeçalho de resposta às respostas.</span><span class="sxs-lookup"><span data-stu-id="9f13e-120">Add a response header to responses.</span></span>
- <span data-ttu-id="9f13e-121">Valide as solicitações antes que elas atinjam o controlador.</span><span class="sxs-lookup"><span data-stu-id="9f13e-121">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="9f13e-122">Este diagrama mostra dois manipuladores personalizados inseridos no pipeline:</span><span class="sxs-lookup"><span data-stu-id="9f13e-122">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="9f13e-123">No lado do cliente, o HttpClient também usa manipuladores de mensagens.</span><span class="sxs-lookup"><span data-stu-id="9f13e-123">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="9f13e-124">Para obter mais informações, consulte [manipuladores de mensagens HttpClient](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="9f13e-124">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>

## <a name="custom-message-handlers"></a><span data-ttu-id="9f13e-125">Manipuladores de mensagens personalizadas</span><span class="sxs-lookup"><span data-stu-id="9f13e-125">Custom Message Handlers</span></span>

<span data-ttu-id="9f13e-126">Para gravar um manipulador de mensagens personalizado, derive de **System .net. http. DelegatingHandler** e substitua o método **SendAsync** .</span><span class="sxs-lookup"><span data-stu-id="9f13e-126">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="9f13e-127">Esse método tem a seguinte assinatura:</span><span class="sxs-lookup"><span data-stu-id="9f13e-127">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="9f13e-128">O método usa um **HttpRequestMessage** como entrada e retorna de forma assíncrona um **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="9f13e-128">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="9f13e-129">Uma implementação típica faz o seguinte:</span><span class="sxs-lookup"><span data-stu-id="9f13e-129">A typical implementation does the following:</span></span>

1. <span data-ttu-id="9f13e-130">Processar a mensagem de solicitação.</span><span class="sxs-lookup"><span data-stu-id="9f13e-130">Process the request message.</span></span>
2. <span data-ttu-id="9f13e-131">Chame `base.SendAsync` para enviar a solicitação para o manipulador interno.</span><span class="sxs-lookup"><span data-stu-id="9f13e-131">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="9f13e-132">O manipulador interno retorna uma mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="9f13e-132">The inner handler returns a response message.</span></span> <span data-ttu-id="9f13e-133">(Essa etapa é assíncrona.)</span><span class="sxs-lookup"><span data-stu-id="9f13e-133">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="9f13e-134">Processar a resposta e retorná-la ao chamador.</span><span class="sxs-lookup"><span data-stu-id="9f13e-134">Process the response and return it to the caller.</span></span>

<span data-ttu-id="9f13e-135">Aqui está um exemplo trivial:</span><span class="sxs-lookup"><span data-stu-id="9f13e-135">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="9f13e-136">A chamada a `base.SendAsync` é assíncrona.</span><span class="sxs-lookup"><span data-stu-id="9f13e-136">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="9f13e-137">Se o manipulador funcionar após essa chamada, use a palavra-chave **Await** , conforme mostrado.</span><span class="sxs-lookup"><span data-stu-id="9f13e-137">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>

<span data-ttu-id="9f13e-138">Um manipulador de delegação também pode ignorar o manipulador interno e criar a resposta diretamente:</span><span class="sxs-lookup"><span data-stu-id="9f13e-138">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="9f13e-139">Se um manipulador de delegação criar a resposta sem chamar `base.SendAsync`, a solicitação ignorará o restante do pipeline.</span><span class="sxs-lookup"><span data-stu-id="9f13e-139">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="9f13e-140">Isso pode ser útil para um manipulador que valida a solicitação (criando uma resposta de erro).</span><span class="sxs-lookup"><span data-stu-id="9f13e-140">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="9f13e-141">Adicionando um manipulador ao pipeline</span><span class="sxs-lookup"><span data-stu-id="9f13e-141">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="9f13e-142">Para adicionar um manipulador de mensagens no lado do servidor, adicione o manipulador à coleção **HttpConfiguration. MessageHandlers** .</span><span class="sxs-lookup"><span data-stu-id="9f13e-142">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="9f13e-143">Se você usou o modelo "aplicativo Web ASP.NET MVC 4" para criar o projeto, poderá fazer isso dentro da classe **WebApiConfig** :</span><span class="sxs-lookup"><span data-stu-id="9f13e-143">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="9f13e-144">Os manipuladores de mensagens são chamados na mesma ordem em que aparecem na coleção **MessageHandlers** .</span><span class="sxs-lookup"><span data-stu-id="9f13e-144">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="9f13e-145">Como eles são aninhados, a mensagem de resposta viaja na outra direção.</span><span class="sxs-lookup"><span data-stu-id="9f13e-145">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="9f13e-146">Ou seja, o último manipulador é o primeiro a obter a mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="9f13e-146">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="9f13e-147">Observe que você não precisa definir os manipuladores internos; a estrutura da API Web conecta automaticamente os manipuladores de mensagens.</span><span class="sxs-lookup"><span data-stu-id="9f13e-147">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="9f13e-148">Se você estiver [hospedando internamente](../older-versions/self-host-a-web-api.md), crie uma instância da classe **HttpSelfHostConfiguration** e adicione os manipuladores à coleção **MessageHandlers** .</span><span class="sxs-lookup"><span data-stu-id="9f13e-148">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="9f13e-149">Agora, vejamos alguns exemplos de manipuladores de mensagens personalizadas.</span><span class="sxs-lookup"><span data-stu-id="9f13e-149">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="9f13e-150">Exemplo: X-HTTP-Method-override</span><span class="sxs-lookup"><span data-stu-id="9f13e-150">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="9f13e-151">X-HTTP-Method-override é um cabeçalho HTTP não padrão.</span><span class="sxs-lookup"><span data-stu-id="9f13e-151">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="9f13e-152">Ele foi projetado para clientes que não podem enviar determinados tipos de solicitação HTTP, como PUT ou DELETE.</span><span class="sxs-lookup"><span data-stu-id="9f13e-152">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="9f13e-153">Em vez disso, o cliente envia uma solicitação POST e define o cabeçalho X-HTTP-Method-override para o método desejado.</span><span class="sxs-lookup"><span data-stu-id="9f13e-153">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="9f13e-154">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="9f13e-154">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="9f13e-155">Aqui está um manipulador de mensagens que adiciona suporte para X-HTTP-Method-override:</span><span class="sxs-lookup"><span data-stu-id="9f13e-155">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="9f13e-156">No método **SendAsync** , o manipulador verifica se a mensagem de solicitação é uma solicitação post e se contém o cabeçalho X-http-Method-override.</span><span class="sxs-lookup"><span data-stu-id="9f13e-156">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="9f13e-157">Nesse caso, ele valida o valor do cabeçalho e, em seguida, modifica o método de solicitação.</span><span class="sxs-lookup"><span data-stu-id="9f13e-157">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="9f13e-158">Por fim, o manipulador chama `base.SendAsync` para passar a mensagem para o próximo manipulador.</span><span class="sxs-lookup"><span data-stu-id="9f13e-158">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="9f13e-159">Quando a solicitação atingir a classe **HttpControllerDispatcher** , **HttpControllerDispatcher** roteará a solicitação com base no método de solicitação atualizado.</span><span class="sxs-lookup"><span data-stu-id="9f13e-159">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="9f13e-160">Exemplo: adicionando um cabeçalho de resposta personalizado</span><span class="sxs-lookup"><span data-stu-id="9f13e-160">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="9f13e-161">Aqui está um manipulador de mensagens que adiciona um cabeçalho personalizado a cada mensagem de resposta:</span><span class="sxs-lookup"><span data-stu-id="9f13e-161">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="9f13e-162">Primeiro, o manipulador chama `base.SendAsync` para passar a solicitação para o manipulador de mensagens internas.</span><span class="sxs-lookup"><span data-stu-id="9f13e-162">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="9f13e-163">O manipulador interno retorna uma mensagem de resposta, mas faz isso de forma assíncrona usando um objeto **Task&lt;t&gt;** .</span><span class="sxs-lookup"><span data-stu-id="9f13e-163">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="9f13e-164">A mensagem de resposta não estará disponível até que `base.SendAsync` seja concluída de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="9f13e-164">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="9f13e-165">Este exemplo usa a palavra-chave **Await** para executar o trabalho de forma assíncrona após a conclusão da `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="9f13e-165">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="9f13e-166">Se você estiver direcionando .NET Framework 4,0, use a **tarefa**&lt;t&gt; **. Método ContinueWith** :</span><span class="sxs-lookup"><span data-stu-id="9f13e-166">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="9f13e-167">Exemplo: verificando uma chave de API</span><span class="sxs-lookup"><span data-stu-id="9f13e-167">Example: Checking for an API Key</span></span>

<span data-ttu-id="9f13e-168">Alguns serviços Web exigem que os clientes incluam uma chave de API em sua solicitação.</span><span class="sxs-lookup"><span data-stu-id="9f13e-168">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="9f13e-169">O exemplo a seguir mostra como um manipulador de mensagens pode verificar solicitações para uma chave de API válida:</span><span class="sxs-lookup"><span data-stu-id="9f13e-169">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="9f13e-170">Esse manipulador procura a chave de API na cadeia de caracteres de consulta do URI.</span><span class="sxs-lookup"><span data-stu-id="9f13e-170">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="9f13e-171">(Para este exemplo, presumimos que a chave é uma cadeia de caracteres estática.</span><span class="sxs-lookup"><span data-stu-id="9f13e-171">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="9f13e-172">Uma implementação real provavelmente usaria uma validação mais complexa.) Se a cadeia de caracteres de consulta contiver a chave, o manipulador passará a solicitação para o manipulador interno.</span><span class="sxs-lookup"><span data-stu-id="9f13e-172">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="9f13e-173">Se a solicitação não tiver uma chave válida, o manipulador criará uma mensagem de resposta com o status 403, proibido.</span><span class="sxs-lookup"><span data-stu-id="9f13e-173">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="9f13e-174">Nesse caso, o manipulador não chama `base.SendAsync`, portanto, o manipulador interno nunca recebe a solicitação, nem o controlador.</span><span class="sxs-lookup"><span data-stu-id="9f13e-174">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="9f13e-175">Portanto, o controlador pode assumir que todas as solicitações de entrada têm uma chave de API válida.</span><span class="sxs-lookup"><span data-stu-id="9f13e-175">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="9f13e-176">Se a chave de API se aplicar somente a determinadas ações do controlador, considere usar um filtro de ação em vez de um manipulador de mensagens.</span><span class="sxs-lookup"><span data-stu-id="9f13e-176">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="9f13e-177">Filtros de ação são executados após o roteamento do URI ser executado.</span><span class="sxs-lookup"><span data-stu-id="9f13e-177">Action filters run after URI routing is performed.</span></span>

## <a name="per-route-message-handlers"></a><span data-ttu-id="9f13e-178">Manipuladores de mensagens por rota</span><span class="sxs-lookup"><span data-stu-id="9f13e-178">Per-Route Message Handlers</span></span>

<span data-ttu-id="9f13e-179">Os manipuladores na coleção **HttpConfiguration. MessageHandlers** se aplicam globalmente.</span><span class="sxs-lookup"><span data-stu-id="9f13e-179">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="9f13e-180">Como alternativa, você pode adicionar um manipulador de mensagens a uma rota específica ao definir a rota:</span><span class="sxs-lookup"><span data-stu-id="9f13e-180">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="9f13e-181">Neste exemplo, se o URI de solicitação corresponder a "Route2", a solicitação será expedida para `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="9f13e-181">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="9f13e-182">O diagrama a seguir mostra o pipeline para essas duas rotas:</span><span class="sxs-lookup"><span data-stu-id="9f13e-182">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="9f13e-183">Observe que `MessageHandler2` substitui o **HttpControllerDispatcher**padrão.</span><span class="sxs-lookup"><span data-stu-id="9f13e-183">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="9f13e-184">Neste exemplo, `MessageHandler2` cria a resposta e as solicitações que correspondem a "Route2" nunca vão para um controlador.</span><span class="sxs-lookup"><span data-stu-id="9f13e-184">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="9f13e-185">Isso permite que você substitua todo o mecanismo do controlador da API Web pelo seu próprio ponto de extremidade personalizado.</span><span class="sxs-lookup"><span data-stu-id="9f13e-185">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="9f13e-186">Como alternativa, um manipulador de mensagens por rota pode delegar para **HttpControllerDispatcher**, que, em seguida, é expedido para um controlador.</span><span class="sxs-lookup"><span data-stu-id="9f13e-186">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="9f13e-187">O código a seguir mostra como configurar essa rota:</span><span class="sxs-lookup"><span data-stu-id="9f13e-187">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
