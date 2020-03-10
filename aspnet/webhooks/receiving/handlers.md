---
uid: webhooks/receiving/handlers
title: Manipuladores de WebHooks do ASP.NET | Microsoft Docs
author: rick-anderson
description: Como lidar com solicitações em WebHooks ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: a55b0d20-9c90-4bd3-a471-20da6f569f0c
ms.openlocfilehash: 01c9a283d105c4a0973ff88c8de646c5f49a34db
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637868"
---
# <a name="aspnet-webhooks-handlers"></a><span data-ttu-id="18d1b-103">Manipuladores de WebHooks do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="18d1b-103">ASP.NET WebHooks handlers</span></span>

<span data-ttu-id="18d1b-104">Após a validação das solicitações de webhook por um receptor de webhook, ela estará pronta para ser processada pelo código do usuário.</span><span class="sxs-lookup"><span data-stu-id="18d1b-104">Once WebHooks requests has been validated by a WebHook receiver, it is ready to be processed by user code.</span></span> <span data-ttu-id="18d1b-105">É aí que os *manipuladores* entram.</span><span class="sxs-lookup"><span data-stu-id="18d1b-105">This is where *handlers* come in.</span></span> <span data-ttu-id="18d1b-106">Os manipuladores derivam da interface [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) , mas normalmente usam a classe [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) em vez de derivar diretamente da interface.</span><span class="sxs-lookup"><span data-stu-id="18d1b-106">Handlers derive from the [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) interface but typically uses the [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) class instead of deriving directly from the interface.</span></span>

<span data-ttu-id="18d1b-107">Uma solicitação de webhook pode ser processada por um ou mais manipuladores.</span><span class="sxs-lookup"><span data-stu-id="18d1b-107">A WebHook request can be processed by one or more handlers.</span></span> <span data-ttu-id="18d1b-108">Os manipuladores são chamados na ordem com base em sua respectiva propriedade de *ordem* indo do mais baixo para o mais alto, em que a ordem é um inteiro simples (sugerido para estar entre 1 e 100):</span><span class="sxs-lookup"><span data-stu-id="18d1b-108">Handlers are called in order based on their respective *Order* property going from lowest to highest where Order is a simple integer (suggested to be between 1 and 100):</span></span>

![Diagrama de propriedades de ordem do manipulador de webhook](_static/Handlers.png)

<span data-ttu-id="18d1b-110">Um manipulador pode, opcionalmente, definir a propriedade de *resposta* no WebHookHandlerContext, que levará o processamento a parar e a resposta a ser enviada de volta como a resposta http para o webhook.</span><span class="sxs-lookup"><span data-stu-id="18d1b-110">A handler can optionally set the *Response* property on the WebHookHandlerContext which will lead the processing to stop and the response to be sent back as the HTTP response to the WebHook.</span></span> <span data-ttu-id="18d1b-111">No caso acima, o manipulador C não será chamado porque ele tem uma ordem superior à B e B define a resposta.</span><span class="sxs-lookup"><span data-stu-id="18d1b-111">In the case above, Handler C won't get called because it has a higher order than B and B sets the response.</span></span>

<span data-ttu-id="18d1b-112">A definição da resposta normalmente é relevante apenas para WebHooks em que a resposta pode transportar informações para a API de origem.</span><span class="sxs-lookup"><span data-stu-id="18d1b-112">Setting the response is typically only relevant for WebHooks where the response can carry information back to the originating API.</span></span> <span data-ttu-id="18d1b-113">Isso é, por exemplo, o caso com WebHooks de margem de atraso onde a resposta é postada para o canal no qual o webhook veio.</span><span class="sxs-lookup"><span data-stu-id="18d1b-113">This is for example the case with Slack WebHooks where the response is posted back to the channel where the WebHook came from.</span></span> <span data-ttu-id="18d1b-114">Os manipuladores podem definir a propriedade Receiver se quiserem apenas receber WebHooks desse receptor específico.</span><span class="sxs-lookup"><span data-stu-id="18d1b-114">Handlers can set the Receiver property if they only want to receive WebHooks from that particular receiver.</span></span> <span data-ttu-id="18d1b-115">Se eles não definirem o receptor, eles serão chamados para todos eles.</span><span class="sxs-lookup"><span data-stu-id="18d1b-115">If they don't set the receiver they are called for all of them.</span></span>

<span data-ttu-id="18d1b-116">Um outro uso comum de uma resposta é usar uma resposta *410* , para indicar que o webhook não está mais ativo e que nenhuma solicitação adicional deve ser enviada.</span><span class="sxs-lookup"><span data-stu-id="18d1b-116">One other common use of a response is to use a *410 Gone* response to indicate that the WebHook no longer is active and no further requests should be submitted.</span></span>

<span data-ttu-id="18d1b-117">Por padrão, um manipulador será chamado por todos os receptores de webhook.</span><span class="sxs-lookup"><span data-stu-id="18d1b-117">By default a handler will be called by all WebHook receivers.</span></span> <span data-ttu-id="18d1b-118">No entanto, se a propriedade *Receiver* for definida como o nome de um manipulador, esse manipulador só receberá solicitações de webhook desse receptor.</span><span class="sxs-lookup"><span data-stu-id="18d1b-118">However, if the *Receiver* property is set to the name of a handler then that handler will only receive WebHook requests from that receiver.</span></span>

## <a name="processing-a-webhook"></a><span data-ttu-id="18d1b-119">Processando um webhook</span><span class="sxs-lookup"><span data-stu-id="18d1b-119">Processing a WebHook</span></span>

<span data-ttu-id="18d1b-120">Quando um manipulador é chamado, ele obtém um [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) que contém informações sobre a solicitação de webhook.</span><span class="sxs-lookup"><span data-stu-id="18d1b-120">When a handler is called, it gets a [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) containing information about the WebHook request.</span></span> <span data-ttu-id="18d1b-121">Os dados, normalmente o corpo da solicitação HTTP, estão disponíveis na propriedade *Data* .</span><span class="sxs-lookup"><span data-stu-id="18d1b-121">The data, typically the HTTP request body, is available from the *Data* property.</span></span>

<span data-ttu-id="18d1b-122">O tipo dos dados normalmente são dados de formulário JSON ou HTML, mas é possível convertê-los em um tipo mais específico, se desejado.</span><span class="sxs-lookup"><span data-stu-id="18d1b-122">The type of the data is typically JSON or HTML form data, but it is possible to cast to a more specific type if desired.</span></span> <span data-ttu-id="18d1b-123">Por exemplo, os WebHooks personalizados gerados por WebHooks ASP.NET podem ser convertidos no tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="18d1b-123">For example, the custom WebHooks generated by ASP.NET WebHooks can be cast to the type [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) as follows:</span></span>

```csharp
public class MyWebHookHandler : WebHookHandler
{
    public MyWebHookHandler()
    {
        this.Receiver = "custom";
    }

    public override Task ExecuteAsync(string generator, WebHookHandlerContext context)
    {
        CustomNotifications notifications = context.GetDataOrDefault<CustomNotifications>();
        foreach (var notification in notifications.Notifications)
        {
            ...
        }
        return Task.FromResult(true);
    }
}
```

  ## <a name="queued-processing"></a><span data-ttu-id="18d1b-124">Processamento em fila</span><span class="sxs-lookup"><span data-stu-id="18d1b-124">Queued Processing</span></span>

<span data-ttu-id="18d1b-125">A maioria dos remetentes de webhook reenviará um webhook se uma resposta não for gerada dentro de alguns segundos.</span><span class="sxs-lookup"><span data-stu-id="18d1b-125">Most WebHook senders will resend a WebHook if a response is not generated within a handful of seconds.</span></span> <span data-ttu-id="18d1b-126">Isso significa que o manipulador deve concluir o processamento dentro desse período de tempo para que ele não seja chamado novamente.</span><span class="sxs-lookup"><span data-stu-id="18d1b-126">This means that your handler must complete the processing within that time frame in order not for it to be called again.</span></span>

<span data-ttu-id="18d1b-127">Se o processamento levar mais tempo ou for melhor manipulado separadamente, o [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) poderá ser usado para enviar a solicitação de webhook para uma fila, por exemplo, [fila de armazenamento do Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span><span class="sxs-lookup"><span data-stu-id="18d1b-127">If the processing takes longer, or is better handled separately then the [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) can be used to submit the WebHook request to a queue, for example [Azure Storage Queue](https://msdn.microsoft.com/library/azure/dd179353.aspx).</span></span>

<span data-ttu-id="18d1b-128">Um contorno de uma implementação de [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) é fornecido aqui:</span><span class="sxs-lookup"><span data-stu-id="18d1b-128">An outline of a [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) implementation is provided here:</span></span>

```csharp
public class QueueHandler : WebHookQueueHandler
{
    public override Task EnqueueAsync(WebHookQueueContext context)
    {

        // Enqueue WebHookQueueContext to your queuing system of choice

        return Task.FromResult(true);
    }
}
```
