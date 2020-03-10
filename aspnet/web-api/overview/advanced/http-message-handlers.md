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
# <a name="http-message-handlers-in-aspnet-web-api"></a>Manipuladores de mensagens HTTP no ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

Um *manipulador de mensagens* é uma classe que recebe uma solicitação HTTP e retorna uma resposta http. Os manipuladores de mensagens derivam da classe abstrata **HttpMessageHandler** .

Normalmente, uma série de manipuladores de mensagens é encadeada. O primeiro manipulador recebe uma solicitação HTTP, executa algum processamento e fornece a solicitação para o próximo manipulador. Em algum momento, a resposta é criada e retorna o backup da cadeia. Esse padrão é chamado de manipulador de *delegação* .

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Manipuladores de mensagens do lado do servidor

No lado do servidor, o pipeline da API Web usa alguns manipuladores de mensagens internos:

- **HttpServer** Obtém a solicitação do host.
- **HttpRoutingDispatcher** despacha a solicitação com base na rota.
- **HttpControllerDispatcher** envia a solicitação para um controlador de API da Web.

Você pode adicionar manipuladores personalizados ao pipeline. Os manipuladores de mensagens são bons para preocupações abrangentes que operam no nível de mensagens HTTP (em vez de ações do controlador). Por exemplo, um manipulador de mensagens pode:

- Ler ou modificar cabeçalhos de solicitação.
- Adicione um cabeçalho de resposta às respostas.
- Valide as solicitações antes que elas atinjam o controlador.

Este diagrama mostra dois manipuladores personalizados inseridos no pipeline:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> No lado do cliente, o HttpClient também usa manipuladores de mensagens. Para obter mais informações, consulte [manipuladores de mensagens HttpClient](httpclient-message-handlers.md).

## <a name="custom-message-handlers"></a>Manipuladores de mensagens personalizadas

Para gravar um manipulador de mensagens personalizado, derive de **System .net. http. DelegatingHandler** e substitua o método **SendAsync** . Esse método tem a seguinte assinatura:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

O método usa um **HttpRequestMessage** como entrada e retorna de forma assíncrona um **HttpResponseMessage**. Uma implementação típica faz o seguinte:

1. Processar a mensagem de solicitação.
2. Chame `base.SendAsync` para enviar a solicitação para o manipulador interno.
3. O manipulador interno retorna uma mensagem de resposta. (Essa etapa é assíncrona.)
4. Processar a resposta e retorná-la ao chamador.

Aqui está um exemplo trivial:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> A chamada a `base.SendAsync` é assíncrona. Se o manipulador funcionar após essa chamada, use a palavra-chave **Await** , conforme mostrado.

Um manipulador de delegação também pode ignorar o manipulador interno e criar a resposta diretamente:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Se um manipulador de delegação criar a resposta sem chamar `base.SendAsync`, a solicitação ignorará o restante do pipeline. Isso pode ser útil para um manipulador que valida a solicitação (criando uma resposta de erro).

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Adicionando um manipulador ao pipeline

Para adicionar um manipulador de mensagens no lado do servidor, adicione o manipulador à coleção **HttpConfiguration. MessageHandlers** . Se você usou o modelo "aplicativo Web ASP.NET MVC 4" para criar o projeto, poderá fazer isso dentro da classe **WebApiConfig** :

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

Os manipuladores de mensagens são chamados na mesma ordem em que aparecem na coleção **MessageHandlers** . Como eles são aninhados, a mensagem de resposta viaja na outra direção. Ou seja, o último manipulador é o primeiro a obter a mensagem de resposta.

Observe que você não precisa definir os manipuladores internos; a estrutura da API Web conecta automaticamente os manipuladores de mensagens.

Se você estiver [hospedando internamente](../older-versions/self-host-a-web-api.md), crie uma instância da classe **HttpSelfHostConfiguration** e adicione os manipuladores à coleção **MessageHandlers** .

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Agora, vejamos alguns exemplos de manipuladores de mensagens personalizadas.

## <a name="example-x-http-method-override"></a>Exemplo: X-HTTP-Method-override

X-HTTP-Method-override é um cabeçalho HTTP não padrão. Ele foi projetado para clientes que não podem enviar determinados tipos de solicitação HTTP, como PUT ou DELETE. Em vez disso, o cliente envia uma solicitação POST e define o cabeçalho X-HTTP-Method-override para o método desejado. Por exemplo:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Aqui está um manipulador de mensagens que adiciona suporte para X-HTTP-Method-override:

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

No método **SendAsync** , o manipulador verifica se a mensagem de solicitação é uma solicitação post e se contém o cabeçalho X-http-Method-override. Nesse caso, ele valida o valor do cabeçalho e, em seguida, modifica o método de solicitação. Por fim, o manipulador chama `base.SendAsync` para passar a mensagem para o próximo manipulador.

Quando a solicitação atingir a classe **HttpControllerDispatcher** , **HttpControllerDispatcher** roteará a solicitação com base no método de solicitação atualizado.

## <a name="example-adding-a-custom-response-header"></a>Exemplo: adicionando um cabeçalho de resposta personalizado

Aqui está um manipulador de mensagens que adiciona um cabeçalho personalizado a cada mensagem de resposta:

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

Primeiro, o manipulador chama `base.SendAsync` para passar a solicitação para o manipulador de mensagens internas. O manipulador interno retorna uma mensagem de resposta, mas faz isso de forma assíncrona usando um objeto **Task&lt;t&gt;** . A mensagem de resposta não estará disponível até que `base.SendAsync` seja concluída de forma assíncrona.

Este exemplo usa a palavra-chave **Await** para executar o trabalho de forma assíncrona após a conclusão da `SendAsync`. Se você estiver direcionando .NET Framework 4,0, use a **tarefa**&lt;t&gt; **. Método ContinueWith** :

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Exemplo: verificando uma chave de API

Alguns serviços Web exigem que os clientes incluam uma chave de API em sua solicitação. O exemplo a seguir mostra como um manipulador de mensagens pode verificar solicitações para uma chave de API válida:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Esse manipulador procura a chave de API na cadeia de caracteres de consulta do URI. (Para este exemplo, presumimos que a chave é uma cadeia de caracteres estática. Uma implementação real provavelmente usaria uma validação mais complexa.) Se a cadeia de caracteres de consulta contiver a chave, o manipulador passará a solicitação para o manipulador interno.

Se a solicitação não tiver uma chave válida, o manipulador criará uma mensagem de resposta com o status 403, proibido. Nesse caso, o manipulador não chama `base.SendAsync`, portanto, o manipulador interno nunca recebe a solicitação, nem o controlador. Portanto, o controlador pode assumir que todas as solicitações de entrada têm uma chave de API válida.

> [!NOTE]
> Se a chave de API se aplicar somente a determinadas ações do controlador, considere usar um filtro de ação em vez de um manipulador de mensagens. Filtros de ação são executados após o roteamento do URI ser executado.

## <a name="per-route-message-handlers"></a>Manipuladores de mensagens por rota

Os manipuladores na coleção **HttpConfiguration. MessageHandlers** se aplicam globalmente.

Como alternativa, você pode adicionar um manipulador de mensagens a uma rota específica ao definir a rota:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

Neste exemplo, se o URI de solicitação corresponder a "Route2", a solicitação será expedida para `MessageHandler2`. O diagrama a seguir mostra o pipeline para essas duas rotas:

![](http-message-handlers/_static/image4.png)

Observe que `MessageHandler2` substitui o **HttpControllerDispatcher**padrão. Neste exemplo, `MessageHandler2` cria a resposta e as solicitações que correspondem a "Route2" nunca vão para um controlador. Isso permite que você substitua todo o mecanismo do controlador da API Web pelo seu próprio ponto de extremidade personalizado.

Como alternativa, um manipulador de mensagens por rota pode delegar para **HttpControllerDispatcher**, que, em seguida, é expedido para um controlador.

![](http-message-handlers/_static/image5.png)

O código a seguir mostra como configurar essa rota:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
