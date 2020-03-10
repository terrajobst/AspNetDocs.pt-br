---
uid: web-api/overview/error-handling/exception-handling
title: Manipulação de exceção em ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.custom: seoapril2019
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: dbdbab6aefec840e2fec9e9cd33f3d124093750e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622314"
---
# <a name="exception-handling-in-aspnet-web-api"></a>Manipulação de exceção no ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

Este artigo descreve o tratamento de erros e exceções no ASP.NET Web API.

- [HttpResponseexception](#httpresponserexception)
- [Filtros de exceção](#exception_filters)
- [Registrando filtros de exceção](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

O que acontece se um controlador da API da Web gerar uma exceção não capturada? Por padrão, a maioria das exceções são convertidas em uma resposta HTTP com o código de status 500, erro interno do servidor.

O tipo **httpresponseexception** é um caso especial. Essa exceção retorna qualquer código de status HTTP que você especificar no construtor de exceção. Por exemplo, o método a seguir retorna 404, não encontrado, se o parâmetro *ID* não for válido.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Para obter mais controle sobre a resposta, você também pode construir a mensagem de resposta inteira e incluí-la com o **httpresponseexception:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Filtros de exceção

Você pode personalizar como a API da Web lida com exceções escrevendo um *filtro de exceção*. Um filtro de exceção é executado quando um método de controlador gera qualquer exceção sem tratamento que *não* seja uma exceção **httpresponseexception** . O tipo **httpresponseexception** é um caso especial, pois ele foi projetado especificamente para retornar uma resposta http.

Os filtros de exceção implementam a interface **System. Web. http. Filters. IExceptionFilter** . A maneira mais simples de escrever um filtro de exceção é derivar da classe **System. Web. http. Filters. ExceptionFilterAttribute** e substituir o método **OnException** .

> [!NOTE]
> Filtros de exceção no ASP.NET Web API são semelhantes aos do ASP.NET MVC. No entanto, eles são declarados em um namespace e uma função separados separadamente. Em particular, a classe **HandleErrorAttribute** usada no MVC não trata as exceções geradas pelos controladores de API da Web.

Aqui está um filtro que converte exceções **NotImplementedException** no código de status HTTP 501, não implementado:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

A propriedade **Response** do objeto **HttpActionExecutedContext** contém a mensagem de resposta http que será enviada ao cliente.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Registrando filtros de exceção

Há várias maneiras de registrar um filtro de exceção da API Web:

- por ação
- pelo controlador
- globalmente

Para aplicar o filtro em uma ação específica, adicione o filtro como um atributo à ação:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Para aplicar o filtro a todas as ações em um controlador, adicione o filtro como um atributo à classe do controlador:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Para aplicar o filtro globalmente a todos os controladores de API da Web, adicione uma instância do filtro à coleção **GlobalConfiguration. Configuration. Filters** . Os filtros de exceção nesta coleção aplicam-se a qualquer ação do controlador da API Web.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Se você usar o modelo de projeto "aplicativo Web ASP.NET MVC 4" para criar seu projeto, coloque o código de configuração da API Web dentro da classe `WebApiConfig`, localizada na pasta de início do aplicativo\_:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

O objeto **HttpError** fornece uma maneira consistente de retornar informações de erro no corpo da resposta. O exemplo a seguir mostra como retornar o código de status HTTP 404 (não encontrado) com um **HttpError** no corpo da resposta.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** é um método de extensão definido na classe **System .net. http. HttpRequestMessageExtensions** . Internamente, o **CreateErrorResponse** cria uma instância de **HttpError** e, em seguida, cria um **HttpResponseMessage** que contém o **HttpError**.

Neste exemplo, se o método for bem-sucedido, ele retornará o produto na resposta HTTP. Mas se o produto solicitado não for encontrado, a resposta HTTP conterá um **HttpError** no corpo da solicitação. A resposta pode ser semelhante ao seguinte:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Observe que o **HttpError** foi SERIALIZADO para JSON neste exemplo. Uma vantagem de usar o **HttpError** é que ele passa pela mesma [negociação de conteúdo](../formats-and-model-binding/content-negotiation.md) e processo de serialização que qualquer outro modelo fortemente tipado.

### <a name="httperror-and-model-validation"></a>HttpError e validação de modelo

Para a validação do modelo, você pode passar o estado do modelo para **CreateErrorResponse**, para incluir os erros de validação na resposta:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

Este exemplo pode retornar a seguinte resposta:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Para obter mais informações sobre a validação de modelo, consulte [validação de modelo em ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Usando HttpError com HttpResponseexception

Os exemplos anteriores retornam uma mensagem **HttpResponseMessage** da ação do controlador, mas você também pode usar **httpresponseexception** para retornar um **HttpError**. Isso permite que você retorne um modelo fortemente tipado no caso de êxito normal, enquanto ainda retorna **HttpError** se houver um erro:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
