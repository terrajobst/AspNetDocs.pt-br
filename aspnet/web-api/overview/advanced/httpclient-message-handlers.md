---
uid: web-api/overview/advanced/httpclient-message-handlers
title: Manipuladores de mensagens HttpClient no ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Criar manipuladores de mensagens personalizados para ASP.NET Web API no ASP.NET 4. x
ms.author: riande
ms.date: 10/01/2012
ms.custom: seoapril2019
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 265bd9b2f48ed7d1e955f3c4947d10fd589b3e17
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557641"
---
# <a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="8abc2-103">Manipuladores de mensagens HttpClient no ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="8abc2-103">HttpClient Message Handlers in ASP.NET Web API</span></span>

<span data-ttu-id="8abc2-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8abc2-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8abc2-105">Um *manipulador de mensagens* é uma classe que recebe uma solicitação HTTP e retorna uma resposta http.</span><span class="sxs-lookup"><span data-stu-id="8abc2-105">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="8abc2-106">Normalmente, uma série de manipuladores de mensagens é encadeada.</span><span class="sxs-lookup"><span data-stu-id="8abc2-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="8abc2-107">O primeiro manipulador recebe uma solicitação HTTP, executa algum processamento e fornece a solicitação para o próximo manipulador.</span><span class="sxs-lookup"><span data-stu-id="8abc2-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="8abc2-108">Em algum momento, a resposta é criada e retorna o backup da cadeia.</span><span class="sxs-lookup"><span data-stu-id="8abc2-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="8abc2-109">Esse padrão é chamado de manipulador de *delegação* .</span><span class="sxs-lookup"><span data-stu-id="8abc2-109">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="8abc2-110">No lado do cliente, a classe **HttpClient** usa um manipulador de mensagens para processar solicitações.</span><span class="sxs-lookup"><span data-stu-id="8abc2-110">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="8abc2-111">O manipulador padrão é **HttpClientHandler**, que envia a solicitação pela rede e obtém a resposta do servidor.</span><span class="sxs-lookup"><span data-stu-id="8abc2-111">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="8abc2-112">Você pode inserir manipuladores de mensagens personalizados no pipeline do cliente:</span><span class="sxs-lookup"><span data-stu-id="8abc2-112">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="8abc2-113">ASP.NET Web API também usa manipuladores de mensagens no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="8abc2-113">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="8abc2-114">Para obter mais informações, consulte [manipuladores de mensagens http](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="8abc2-114">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>

## <a name="custom-message-handlers"></a><span data-ttu-id="8abc2-115">Manipuladores de mensagens personalizadas</span><span class="sxs-lookup"><span data-stu-id="8abc2-115">Custom Message Handlers</span></span>

<span data-ttu-id="8abc2-116">Para gravar um manipulador de mensagens personalizado, derive de **System .net. http. DelegatingHandler** e substitua o método **SendAsync** .</span><span class="sxs-lookup"><span data-stu-id="8abc2-116">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="8abc2-117">Veja a assinatura do método:</span><span class="sxs-lookup"><span data-stu-id="8abc2-117">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="8abc2-118">O método usa um **HttpRequestMessage** como entrada e retorna de forma assíncrona um **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="8abc2-118">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="8abc2-119">Uma implementação típica faz o seguinte:</span><span class="sxs-lookup"><span data-stu-id="8abc2-119">A typical implementation does the following:</span></span>

1. <span data-ttu-id="8abc2-120">Processar a mensagem de solicitação.</span><span class="sxs-lookup"><span data-stu-id="8abc2-120">Process the request message.</span></span>
2. <span data-ttu-id="8abc2-121">Chame `base.SendAsync` para enviar a solicitação para o manipulador interno.</span><span class="sxs-lookup"><span data-stu-id="8abc2-121">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="8abc2-122">O manipulador interno retorna uma mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="8abc2-122">The inner handler returns a response message.</span></span> <span data-ttu-id="8abc2-123">(Essa etapa é assíncrona.)</span><span class="sxs-lookup"><span data-stu-id="8abc2-123">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="8abc2-124">Processar a resposta e retorná-la ao chamador.</span><span class="sxs-lookup"><span data-stu-id="8abc2-124">Process the response and return it to the caller.</span></span>

<span data-ttu-id="8abc2-125">O exemplo a seguir mostra um manipulador de mensagens que adiciona um cabeçalho personalizado à solicitação de saída:</span><span class="sxs-lookup"><span data-stu-id="8abc2-125">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="8abc2-126">A chamada a `base.SendAsync` é assíncrona.</span><span class="sxs-lookup"><span data-stu-id="8abc2-126">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="8abc2-127">Se o manipulador executar qualquer trabalho após essa chamada, use a palavra-chave **Await** para retomar a execução após a conclusão do método.</span><span class="sxs-lookup"><span data-stu-id="8abc2-127">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="8abc2-128">O exemplo a seguir mostra um manipulador que registra códigos de erro.</span><span class="sxs-lookup"><span data-stu-id="8abc2-128">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="8abc2-129">O registro em log não é muito interessante, mas o exemplo mostra como obter a resposta dentro do manipulador.</span><span class="sxs-lookup"><span data-stu-id="8abc2-129">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="8abc2-130">Adicionando manipuladores de mensagens ao pipeline do cliente</span><span class="sxs-lookup"><span data-stu-id="8abc2-130">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="8abc2-131">Para adicionar manipuladores personalizados a **HttpClient**, use o método **HttpClientFactory. Create** :</span><span class="sxs-lookup"><span data-stu-id="8abc2-131">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="8abc2-132">Os manipuladores de mensagens são chamados na ordem em que são passados para o método **Create** .</span><span class="sxs-lookup"><span data-stu-id="8abc2-132">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="8abc2-133">Como os manipuladores são aninhados, a mensagem de resposta viaja na outra direção.</span><span class="sxs-lookup"><span data-stu-id="8abc2-133">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="8abc2-134">Ou seja, o último manipulador é o primeiro a obter a mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="8abc2-134">That is, the last handler is the first to get the response message.</span></span>
