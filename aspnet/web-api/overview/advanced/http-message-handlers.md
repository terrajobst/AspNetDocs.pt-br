---
uid: web-api/overview/advanced/http-message-handlers
title: Manipuladores de mensagens de HTTP na API Web ASP.NET - ASP.NET 4.x
author: MikeWasson
description: Uma visão geral de manipuladores de mensagens HTTP na API Web do ASP.NET para ASP.NET 4. x
ms.author: riande
ms.date: 02/13/2012
ms.custom: seoapril2019
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 308d2e3dd21917e7656f7ffe889dc965d9275d74
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59392099"
---
# <a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="de22c-103">Manipuladores de mensagens de HTTP na API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="de22c-103">HTTP Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="de22c-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="de22c-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="de22c-105">Um *manipulador de mensagens* é uma classe que recebe uma solicitação HTTP e retorna uma resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="de22c-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="de22c-106">Manipuladores de mensagens derivam abstrata **HttpMessageHandler** classe.</span><span class="sxs-lookup"><span data-stu-id="de22c-106">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="de22c-107">Normalmente, uma série de manipuladores de mensagens são encadeados juntos.</span><span class="sxs-lookup"><span data-stu-id="de22c-107">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="de22c-108">O primeiro manipulador recebe uma solicitação HTTP, executa algum processamento e fornece a solicitação para o próximo manipulador.</span><span class="sxs-lookup"><span data-stu-id="de22c-108">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="de22c-109">Em algum momento, a resposta é criada e retorna a cadeia.</span><span class="sxs-lookup"><span data-stu-id="de22c-109">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="de22c-110">Esse padrão é chamado de um *delegando* manipulador.</span><span class="sxs-lookup"><span data-stu-id="de22c-110">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="de22c-111">Manipuladores de mensagens do lado do servidor</span><span class="sxs-lookup"><span data-stu-id="de22c-111">Server-Side Message Handlers</span></span>

<span data-ttu-id="de22c-112">No lado do servidor, o pipeline da API Web usa alguns manipuladores de mensagem interna:</span><span class="sxs-lookup"><span data-stu-id="de22c-112">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="de22c-113">**Servidor HTTP** obtém a solicitação do host.</span><span class="sxs-lookup"><span data-stu-id="de22c-113">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="de22c-114">**HttpRoutingDispatcher** envia a solicitação com base na rota.</span><span class="sxs-lookup"><span data-stu-id="de22c-114">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="de22c-115">**HttpControllerDispatcher** envia a solicitação para um controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="de22c-115">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="de22c-116">Você pode adicionar manipuladores personalizados para o pipeline.</span><span class="sxs-lookup"><span data-stu-id="de22c-116">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="de22c-117">Manipuladores de mensagens são bons para interesses transversais que operam no nível de HTTP mensagens (em vez de ações do controlador).</span><span class="sxs-lookup"><span data-stu-id="de22c-117">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="de22c-118">Por exemplo, um manipulador de mensagens pode:</span><span class="sxs-lookup"><span data-stu-id="de22c-118">For example, a message handler might:</span></span>

- <span data-ttu-id="de22c-119">Ler ou modificar os cabeçalhos de solicitação.</span><span class="sxs-lookup"><span data-stu-id="de22c-119">Read or modify request headers.</span></span>
- <span data-ttu-id="de22c-120">Adicione um cabeçalho de resposta para as respostas.</span><span class="sxs-lookup"><span data-stu-id="de22c-120">Add a response header to responses.</span></span>
- <span data-ttu-id="de22c-121">Valide as solicitações antes que elas atinjam o controlador.</span><span class="sxs-lookup"><span data-stu-id="de22c-121">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="de22c-122">Este diagrama mostra dois manipuladores personalizados inseridos no pipeline:</span><span class="sxs-lookup"><span data-stu-id="de22c-122">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="de22c-123">No lado do cliente, o HttpClient também utiliza manipuladores de mensagens.</span><span class="sxs-lookup"><span data-stu-id="de22c-123">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="de22c-124">Para obter mais informações, consulte [manipuladores de mensagens de HttpClient](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="de22c-124">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="de22c-125">Manipuladores de mensagens personalizado</span><span class="sxs-lookup"><span data-stu-id="de22c-125">Custom Message Handlers</span></span>

<span data-ttu-id="de22c-126">Para escrever um manipulador de mensagens personalizadas, derivam **System.Net.Http.DelegatingHandler** e substitua o **SendAsync** método.</span><span class="sxs-lookup"><span data-stu-id="de22c-126">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="de22c-127">Esse método tem a seguinte assinatura:</span><span class="sxs-lookup"><span data-stu-id="de22c-127">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="de22c-128">O método utiliza um **HttpRequestMessage** como entrada e retorna de maneira assíncrona uma **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="de22c-128">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="de22c-129">Uma implementação típica faz o seguinte:</span><span class="sxs-lookup"><span data-stu-id="de22c-129">A typical implementation does the following:</span></span>

1. <span data-ttu-id="de22c-130">Processe a mensagem de solicitação.</span><span class="sxs-lookup"><span data-stu-id="de22c-130">Process the request message.</span></span>
2. <span data-ttu-id="de22c-131">Chamar `base.SendAsync` para enviar a solicitação para o manipulador interno.</span><span class="sxs-lookup"><span data-stu-id="de22c-131">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="de22c-132">O manipulador interno retorna uma mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="de22c-132">The inner handler returns a response message.</span></span> <span data-ttu-id="de22c-133">(Essa etapa é assíncrona).</span><span class="sxs-lookup"><span data-stu-id="de22c-133">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="de22c-134">Processar a resposta e retorná-lo ao chamador.</span><span class="sxs-lookup"><span data-stu-id="de22c-134">Process the response and return it to the caller.</span></span>

<span data-ttu-id="de22c-135">Aqui está um exemplo trivial:</span><span class="sxs-lookup"><span data-stu-id="de22c-135">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="de22c-136">A chamada para `base.SendAsync` é assíncrona.</span><span class="sxs-lookup"><span data-stu-id="de22c-136">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="de22c-137">Se o manipulador faz qualquer trabalho após esta chamada, use o **await** palavra-chave, conforme mostrado.</span><span class="sxs-lookup"><span data-stu-id="de22c-137">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>


<span data-ttu-id="de22c-138">Um manipulador de delegação pode também ignorar o manipulador interno e criar diretamente a resposta:</span><span class="sxs-lookup"><span data-stu-id="de22c-138">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="de22c-139">Se a delegação de um manipulador cria a resposta sem chamar `base.SendAsync`, a solicitação irá ignorar o restante do pipeline.</span><span class="sxs-lookup"><span data-stu-id="de22c-139">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="de22c-140">Isso pode ser útil para um manipulador que valida a solicitação (Criando uma resposta de erro).</span><span class="sxs-lookup"><span data-stu-id="de22c-140">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="de22c-141">Adicionando um manipulador para o Pipeline</span><span class="sxs-lookup"><span data-stu-id="de22c-141">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="de22c-142">Para adicionar um manipulador de mensagens no lado do servidor, adicione o manipulador para o **HttpConfiguration.MessageHandlers** coleção.</span><span class="sxs-lookup"><span data-stu-id="de22c-142">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="de22c-143">Se você usou o modelo "Aplicativo de Web do ASP.NET MVC 4" para criar o projeto, você pode fazer isso dentro de **WebApiConfig** classe:</span><span class="sxs-lookup"><span data-stu-id="de22c-143">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="de22c-144">Manipuladores de mensagens são chamados na mesma ordem em que aparecem no **MessageHandlers** coleção.</span><span class="sxs-lookup"><span data-stu-id="de22c-144">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="de22c-145">Porque eles estão aninhados, a mensagem de resposta viaja na outra direção.</span><span class="sxs-lookup"><span data-stu-id="de22c-145">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="de22c-146">Ou seja, o último manipulador é o primeiro para obter a mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="de22c-146">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="de22c-147">Observe que você não precisa definir os manipuladores internos; a estrutura da API Web se conecta automaticamente os manipuladores de mensagem.</span><span class="sxs-lookup"><span data-stu-id="de22c-147">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="de22c-148">Se você estiver [auto-hospedagem](../older-versions/self-host-a-web-api.md), crie uma instância das **HttpSelfHostConfiguration** de classe e adicione os manipuladores para o **MessageHandlers** coleção.</span><span class="sxs-lookup"><span data-stu-id="de22c-148">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="de22c-149">Agora vamos examinar alguns exemplos de manipuladores de mensagens personalizadas.</span><span class="sxs-lookup"><span data-stu-id="de22c-149">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="de22c-150">Exemplo: X-HTTP-Method-Override</span><span class="sxs-lookup"><span data-stu-id="de22c-150">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="de22c-151">X-HTTP-Method-Override é um cabeçalho HTTP não padrão.</span><span class="sxs-lookup"><span data-stu-id="de22c-151">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="de22c-152">Ele é projetado para clientes que não é possível enviar determinados tipos de solicitação HTTP, como PUT ou DELETE.</span><span class="sxs-lookup"><span data-stu-id="de22c-152">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="de22c-153">Em vez disso, o cliente envia uma solicitação POST e define o cabeçalho X-HTTP-Method-Override para o método desejado.</span><span class="sxs-lookup"><span data-stu-id="de22c-153">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="de22c-154">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="de22c-154">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="de22c-155">Aqui está um manipulador de mensagens que adiciona suporte para X-HTTP-Method-Override:</span><span class="sxs-lookup"><span data-stu-id="de22c-155">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="de22c-156">No **SendAsync** método, o manipulador verifica se a mensagem de solicitação é uma solicitação POST e se ele contém o cabeçalho X-HTTP-Method-Override.</span><span class="sxs-lookup"><span data-stu-id="de22c-156">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="de22c-157">Nesse caso, ele valida o valor do cabeçalho e, em seguida, modifica o método de solicitação.</span><span class="sxs-lookup"><span data-stu-id="de22c-157">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="de22c-158">Por fim, chama o manipulador `base.SendAsync` para passar a mensagem para o próximo manipulador.</span><span class="sxs-lookup"><span data-stu-id="de22c-158">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="de22c-159">Quando a solicitação atinja o **HttpControllerDispatcher** classe, **HttpControllerDispatcher** irá rotear a solicitação com base no método de solicitação atualizado.</span><span class="sxs-lookup"><span data-stu-id="de22c-159">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="de22c-160">Exemplo: Adicionando um cabeçalho de resposta personalizada</span><span class="sxs-lookup"><span data-stu-id="de22c-160">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="de22c-161">Aqui está um manipulador de mensagens que adiciona um cabeçalho personalizado para cada mensagem de resposta:</span><span class="sxs-lookup"><span data-stu-id="de22c-161">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="de22c-162">Primeiro, o manipulador chama `base.SendAsync` para passar a solicitação para o manipulador de mensagens internas.</span><span class="sxs-lookup"><span data-stu-id="de22c-162">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="de22c-163">O manipulador interno retorna uma mensagem de resposta, mas ele faz isso de forma assíncrona usando um **tarefa&lt;T&gt;**  objeto.</span><span class="sxs-lookup"><span data-stu-id="de22c-163">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="de22c-164">A mensagem de resposta não está disponível até `base.SendAsync` for concluída de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="de22c-164">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="de22c-165">Este exemplo usa o **await** palavra-chave para executar o trabalho assincronamente após `SendAsync` é concluída.</span><span class="sxs-lookup"><span data-stu-id="de22c-165">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="de22c-166">Se você estiver direcionando o .NET Framework 4.0, use o **tarefa**&lt;T&gt;**. ContinueWith** método:</span><span class="sxs-lookup"><span data-stu-id="de22c-166">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="de22c-167">Exemplo: Procurando uma chave de API</span><span class="sxs-lookup"><span data-stu-id="de22c-167">Example: Checking for an API Key</span></span>

<span data-ttu-id="de22c-168">Alguns serviços da web exigem que os clientes incluir uma chave de API na sua solicitação.</span><span class="sxs-lookup"><span data-stu-id="de22c-168">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="de22c-169">O exemplo a seguir mostra como um manipulador de mensagens pode verificar as solicitações para uma chave de API válida:</span><span class="sxs-lookup"><span data-stu-id="de22c-169">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="de22c-170">Esse manipulador procura a chave de API na cadeia de caracteres de consulta do URI.</span><span class="sxs-lookup"><span data-stu-id="de22c-170">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="de22c-171">(Neste exemplo, vamos supor que a chave é uma cadeia de caracteres estática.</span><span class="sxs-lookup"><span data-stu-id="de22c-171">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="de22c-172">Uma implementação real provavelmente usaria uma validação mais complexa.) Se a cadeia de caracteres de consulta contém a chave, o manipulador passa a solicitação para o manipulador interno.</span><span class="sxs-lookup"><span data-stu-id="de22c-172">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="de22c-173">Se a solicitação não tiver uma chave válida, o manipulador cria uma mensagem de resposta com status 403, proibido.</span><span class="sxs-lookup"><span data-stu-id="de22c-173">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="de22c-174">Nesse caso, o manipulador não chama `base.SendAsync`, portanto, o manipulador interno nunca recebe a solicitação, nem o controlador.</span><span class="sxs-lookup"><span data-stu-id="de22c-174">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="de22c-175">Portanto, o controlador pode assumir que todas as solicitações de entrada tem uma chave de API válida.</span><span class="sxs-lookup"><span data-stu-id="de22c-175">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="de22c-176">Se a chave de API se aplica apenas a determinadas ações do controlador, considere usar um filtro de ação em vez de um manipulador de mensagens.</span><span class="sxs-lookup"><span data-stu-id="de22c-176">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="de22c-177">Filtros de ação executado após o roteamento do URI é executado.</span><span class="sxs-lookup"><span data-stu-id="de22c-177">Action filters run after URI routing is performed.</span></span>


## <a name="per-route-message-handlers"></a><span data-ttu-id="de22c-178">Manipuladores de mensagens por rota</span><span class="sxs-lookup"><span data-stu-id="de22c-178">Per-Route Message Handlers</span></span>

<span data-ttu-id="de22c-179">Manipuladores de **HttpConfiguration.MessageHandlers** coleção se aplicam globalmente.</span><span class="sxs-lookup"><span data-stu-id="de22c-179">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="de22c-180">Como alternativa, você pode adicionar um manipulador de mensagens para uma rota específica quando você define a rota:</span><span class="sxs-lookup"><span data-stu-id="de22c-180">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="de22c-181">Neste exemplo, se o URI da solicitação corresponde a "Route2", a solicitação será enviada para `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="de22c-181">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="de22c-182">O diagrama a seguir mostra o pipeline para essas duas rotas:</span><span class="sxs-lookup"><span data-stu-id="de22c-182">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="de22c-183">Observe que `MessageHandler2` substitui o padrão **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="de22c-183">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="de22c-184">Neste exemplo, `MessageHandler2` cria a resposta e as solicitações que correspondem a "Route2" nunca ir para um controlador.</span><span class="sxs-lookup"><span data-stu-id="de22c-184">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="de22c-185">Isso lhe permite substituir o mecanismo de controlador de API da Web inteiro com seu próprio ponto de extremidade personalizado.</span><span class="sxs-lookup"><span data-stu-id="de22c-185">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="de22c-186">Como alternativa, um manipulador de mensagens por rota pode delegar a **HttpControllerDispatcher**, que então expede para um controlador.</span><span class="sxs-lookup"><span data-stu-id="de22c-186">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="de22c-187">O código a seguir mostra como configurar essa rota:</span><span class="sxs-lookup"><span data-stu-id="de22c-187">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
