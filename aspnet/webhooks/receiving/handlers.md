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
# <a name="aspnet-webhooks-handlers"></a>Manipuladores de WebHooks do ASP.NET

Após a validação das solicitações de webhook por um receptor de webhook, ela estará pronta para ser processada pelo código do usuário. É aí que os *manipuladores* entram. Os manipuladores derivam da interface [IWebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) , mas normalmente usam a classe [WebHookHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandler.cs) em vez de derivar diretamente da interface.

Uma solicitação de webhook pode ser processada por um ou mais manipuladores. Os manipuladores são chamados na ordem com base em sua respectiva propriedade de *ordem* indo do mais baixo para o mais alto, em que a ordem é um inteiro simples (sugerido para estar entre 1 e 100):

![Diagrama de propriedades de ordem do manipulador de webhook](_static/Handlers.png)

Um manipulador pode, opcionalmente, definir a propriedade de *resposta* no WebHookHandlerContext, que levará o processamento a parar e a resposta a ser enviada de volta como a resposta http para o webhook. No caso acima, o manipulador C não será chamado porque ele tem uma ordem superior à B e B define a resposta.

A definição da resposta normalmente é relevante apenas para WebHooks em que a resposta pode transportar informações para a API de origem. Isso é, por exemplo, o caso com WebHooks de margem de atraso onde a resposta é postada para o canal no qual o webhook veio. Os manipuladores podem definir a propriedade Receiver se quiserem apenas receber WebHooks desse receptor específico. Se eles não definirem o receptor, eles serão chamados para todos eles.

Um outro uso comum de uma resposta é usar uma resposta *410* , para indicar que o webhook não está mais ativo e que nenhuma solicitação adicional deve ser enviada.

Por padrão, um manipulador será chamado por todos os receptores de webhook. No entanto, se a propriedade *Receiver* for definida como o nome de um manipulador, esse manipulador só receberá solicitações de webhook desse receptor.

## <a name="processing-a-webhook"></a>Processando um webhook

Quando um manipulador é chamado, ele obtém um [WebHookHandlerContext](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookHandlerContext.cs) que contém informações sobre a solicitação de webhook. Os dados, normalmente o corpo da solicitação HTTP, estão disponíveis na propriedade *Data* .

O tipo dos dados normalmente são dados de formulário JSON ou HTML, mas é possível convertê-los em um tipo mais específico, se desejado. Por exemplo, os WebHooks personalizados gerados por WebHooks ASP.NET podem ser convertidos no tipo [CustomNotifications](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers.Custom/WebHooks/CustomNotifications.cs) da seguinte maneira:

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

  ## <a name="queued-processing"></a>Processamento em fila

A maioria dos remetentes de webhook reenviará um webhook se uma resposta não for gerada dentro de alguns segundos. Isso significa que o manipulador deve concluir o processamento dentro desse período de tempo para que ele não seja chamado novamente.

Se o processamento levar mais tempo ou for melhor manipulado separadamente, o [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) poderá ser usado para enviar a solicitação de webhook para uma fila, por exemplo, [fila de armazenamento do Azure](https://msdn.microsoft.com/library/azure/dd179353.aspx).

Um contorno de uma implementação de [WebHookQueueHandler](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/WebHookQueueHandler.cs) é fornecido aqui:

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
