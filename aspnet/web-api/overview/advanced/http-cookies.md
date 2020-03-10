---
uid: web-api/overview/advanced/http-cookies
title: Cookies HTTP no ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Descreve como enviar e receber cookies HTTP na API Web para ASP.NET 4. x.
ms.author: riande
ms.date: 09/17/2012
ms.custom: seoapril2019
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 8ca26ff6776daa13bc4f8b06c2eba61afcfefba2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557683"
---
# <a name="http-cookies-in-aspnet-web-api"></a>Cookies HTTP no ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

Este tópico descreve como enviar e receber cookies HTTP na API da Web.

## <a name="background-on-http-cookies"></a>Plano de fundo em cookies HTTP

Esta seção fornece uma breve visão geral de como os cookies são implementados no nível de HTTP. Para obter detalhes, consulte [RFC 6265](http://tools.ietf.org/html/rfc6265).

Um cookie é um dado que um servidor envia na resposta HTTP. O cliente (opcionalmente) armazena o cookie e o retorna em solicitações subsequentes. Isso permite que o cliente e o servidor compartilhem o estado. Para definir um cookie, o servidor inclui um cabeçalho Set-cookie na resposta. O formato de um cookie é um par de nome-valor, com atributos opcionais. Por exemplo:

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

Aqui está um exemplo com atributos:

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

Para retornar um cookie ao servidor, o cliente inclui um cabeçalho de cookie em solicitações posteriores.

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

Uma resposta HTTP pode incluir vários cabeçalhos Set-cookie.

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

O cliente retorna vários cookies usando um único cabeçalho de cookie.

[!code-console[Main](http-cookies/samples/sample5.cmd)]

O escopo e a duração de um cookie são controlados pelos seguintes atributos no cabeçalho Set-Cookie:

- **Domínio**: informa ao cliente qual domínio deve receber o cookie. Por exemplo, se o domínio for "example.com", o cliente retornará o cookie para cada subdomínio de example.com. Se não for especificado, o domínio será o servidor de origem.
- **Caminho**: restringe o cookie para o caminho especificado dentro do domínio. Se não for especificado, o caminho do URI de solicitação será usado.
- **Expira**em: define uma data de validade para o cookie. O cliente exclui o cookie quando ele expira.
- **Max-age**: define a idade máxima para o cookie. O cliente exclui o cookie quando atinge a idade máxima.

Se `Expires` e `Max-Age` forem definidos, `Max-Age` terá precedência. Se nenhum for definido, o cliente excluirá o cookie quando a sessão atual terminar. (O significado exato de "sessão" é determinado pelo agente do usuário.)

No entanto, lembre-se de que os clientes podem ignorar cookies. Por exemplo, um usuário pode desabilitar cookies por motivos de privacidade. Os clientes podem excluir cookies antes de expirarem ou limitar o número de cookies armazenados. Por motivos de privacidade, os clientes geralmente rejeitam cookies "terceiros", em que o domínio não corresponde ao servidor de origem. Em suma, o servidor não deve confiar na obtenção dos cookies que ele define.

## <a name="cookies-in-web-api"></a>Cookies na API da Web

Para adicionar um cookie a uma resposta HTTP, crie uma instância **CookieHeaderValue** que representa o cookie. Em seguida, chame o método de extensão **Addcookies** , que é definido no **sistema .net. http. Classe HttpResponseHeadersExtensions** , para adicionar o cookie.

Por exemplo, o código a seguir adiciona um cookie dentro de uma ação do controlador:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

Observe que **Addcookies** usa uma matriz de instâncias de **CookieHeaderValue** .

Para extrair os cookies de uma solicitação do cliente, chame o método **getCookies** :

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

Um **CookieHeaderValue** contém uma coleção de instâncias de **cookiestate** . Cada **cookiestate** representa um cookie. Use o método indexador para obter um **cookiestate** por nome, como mostrado.

## <a name="structured-cookie-data"></a>Dados de cookie estruturados

Muitos navegadores limitam quantos cookies armazenarão&#8212;o número total e o número por domínio. Portanto, pode ser útil colocar dados estruturados em um único cookie, em vez de definir vários cookies.

> [!NOTE]
> A RFC 6265 não define a estrutura de dados do cookie.

Usando a classe **CookieHeaderValue** , você pode passar uma lista de pares de nome-valor para os dados do cookie. Esses pares de nome-valor são codificados como dados de formulário codificados em URL no cabeçalho Set-Cookie:

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

O código anterior produz o seguinte cabeçalho Set-Cookie:

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

A classe **cookiestate** fornece um método de indexador para ler os subvalors de um cookie na mensagem de solicitação:

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>Exemplo: definir e recuperar cookies em um manipulador de mensagens

Os exemplos anteriores mostraram como usar cookies de dentro de um controlador de API da Web. Outra opção é usar [manipuladores de mensagens](http-message-handlers.md). Os manipuladores de mensagens são invocados anteriormente no pipeline do que os controladores. Um manipulador de mensagens pode ler cookies da solicitação antes que a solicitação atinja o controlador ou adicionar cookies à resposta depois que o controlador gerar a resposta.

![](http-cookies/_static/image2.png)

O código a seguir mostra um manipulador de mensagens para a criação de IDs de sessão. A ID da sessão é armazenada em um cookie. O manipulador verifica a solicitação para o cookie de sessão. Se a solicitação não incluir o cookie, o manipulador gerará uma nova ID de sessão. Em ambos os casos, o manipulador armazena a ID de sessão no recipiente de propriedades **HttpRequestMessage. Properties** . Ele também adiciona o cookie de sessão à resposta HTTP.

Essa implementação não valida se a ID da sessão do cliente foi realmente emitida pelo servidor. Não o use como uma forma de autenticação! O ponto do exemplo é mostrar o gerenciamento de cookies HTTP.

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

Um controlador pode obter a ID de sessão do recipiente de propriedades **HttpRequestMessage. Properties** .

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
