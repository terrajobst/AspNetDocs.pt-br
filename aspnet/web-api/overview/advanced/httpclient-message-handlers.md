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
# <a name="httpclient-message-handlers-in-aspnet-web-api"></a>Manipuladores de mensagens HttpClient no ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

Um *manipulador de mensagens* é uma classe que recebe uma solicitação HTTP e retorna uma resposta http.

Normalmente, uma série de manipuladores de mensagens é encadeada. O primeiro manipulador recebe uma solicitação HTTP, executa algum processamento e fornece a solicitação para o próximo manipulador. Em algum momento, a resposta é criada e retorna o backup da cadeia. Esse padrão é chamado de manipulador de *delegação* .

![](httpclient-message-handlers/_static/image1.png)

No lado do cliente, a classe **HttpClient** usa um manipulador de mensagens para processar solicitações. O manipulador padrão é **HttpClientHandler**, que envia a solicitação pela rede e obtém a resposta do servidor. Você pode inserir manipuladores de mensagens personalizados no pipeline do cliente:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> ASP.NET Web API também usa manipuladores de mensagens no lado do servidor. Para obter mais informações, consulte [manipuladores de mensagens http](http-message-handlers.md).

## <a name="custom-message-handlers"></a>Manipuladores de mensagens personalizadas

Para gravar um manipulador de mensagens personalizado, derive de **System .net. http. DelegatingHandler** e substitua o método **SendAsync** . Veja a assinatura do método:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

O método usa um **HttpRequestMessage** como entrada e retorna de forma assíncrona um **HttpResponseMessage**. Uma implementação típica faz o seguinte:

1. Processar a mensagem de solicitação.
2. Chame `base.SendAsync` para enviar a solicitação para o manipulador interno.
3. O manipulador interno retorna uma mensagem de resposta. (Essa etapa é assíncrona.)
4. Processar a resposta e retorná-la ao chamador.

O exemplo a seguir mostra um manipulador de mensagens que adiciona um cabeçalho personalizado à solicitação de saída:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

A chamada a `base.SendAsync` é assíncrona. Se o manipulador executar qualquer trabalho após essa chamada, use a palavra-chave **Await** para retomar a execução após a conclusão do método. O exemplo a seguir mostra um manipulador que registra códigos de erro. O registro em log não é muito interessante, mas o exemplo mostra como obter a resposta dentro do manipulador.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Adicionando manipuladores de mensagens ao pipeline do cliente

Para adicionar manipuladores personalizados a **HttpClient**, use o método **HttpClientFactory. Create** :

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Os manipuladores de mensagens são chamados na ordem em que são passados para o método **Create** . Como os manipuladores são aninhados, a mensagem de resposta viaja na outra direção. Ou seja, o último manipulador é o primeiro a obter a mensagem de resposta.
