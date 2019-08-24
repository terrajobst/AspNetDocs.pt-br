---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Resultados da ação na API Web 2-ASP.NET 4. x
author: MikeWasson
description: Descreve como ASP.NET Web API converte o valor de retorno de uma ação do controlador em uma mensagem de resposta HTTP no ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 1eaaf8e87168096683212fa66d3ddf415ad6b22b
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000714"
---
# <a name="action-results-in-web-api-2"></a>Resultados da ação na API Web 2

Este tópico descreve como ASP.NET Web API converte o valor de retorno de uma ação do controlador em uma mensagem de resposta HTTP.

Uma ação do controlador da API Web pode retornar qualquer um dos seguintes:

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. Algum outro tipo

Dependendo de quais delas é retornado, a API da Web usa um mecanismo diferente para criar a resposta HTTP.

| Tipo de retorno | Como a API da Web cria a resposta |
| --- | --- |
| void | Retornar 204 vazio (sem conteúdo) |
| **HttpResponseMessage** | Converter diretamente em uma mensagem de resposta HTTP. |
| **IHttpActionResult** | Chame **ExecuteAsync** para criar um **HttpResponseMessage**e, em seguida, converta em uma mensagem de resposta http. |
| Outro tipo | Gravar o valor de retorno serializado no corpo da resposta; retornar 200 (OK). |

O restante deste tópico descreve cada opção mais detalhadamente.

## <a name="void"></a>void

Se o tipo de retorno `void`for, a API Web simplesmente retornará uma resposta http vazia com o código de status 204 (sem conteúdo).

Controlador de exemplo:

[!code-csharp[Main](action-results/samples/sample1.cs)]

Resposta HTTP:

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

Se a ação retornar um [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), a API da Web converterá o valor de retorno diretamente em uma mensagem de resposta http, usando as propriedades do objeto **HttpResponseMessage** para popular a resposta.

Essa opção lhe dá muito controle sobre a mensagem de resposta. Por exemplo, a ação do controlador a seguir define o cabeçalho Cache-Control.

[!code-csharp[Main](action-results/samples/sample3.cs)]

Responde

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

Se você passar um modelo de domínio para o método CreateResponse, a API da Web usará um formatador de [mídia](../formats-and-model-binding/media-formatters.md) para gravar o modelo serializado no corpo da resposta.

[!code-csharp[Main](action-results/samples/sample5.cs)]

A API Web usa o cabeçalho Accept na solicitação para escolher o formatador. Para obter mais informações, consulte [negociação de conteúdo](../formats-and-model-binding/content-negotiation.md).

## <a name="ihttpactionresult"></a>IHttpActionResult

A interface **IHttpActionResult** foi introduzida na API Web 2. Essencialmente, ele define uma fábrica **HttpResponseMessage** . Aqui estão algumas vantagens de usar a interface **IHttpActionResult** :

- Simplifica o [teste de unidade](../testing-and-debugging/unit-testing-controllers-in-web-api.md) de seus controladores.
- Move a lógica comum para criar respostas HTTP em classes separadas.
- Torna a intenção da ação do controlador mais clara, ocultando os detalhes de baixo nível da construção da resposta.

**IHttpActionResult** contém um único método, **ExecuteAsync**, que cria de forma assíncrona uma instância de **HttpResponseMessage** .

[!code-csharp[Main](action-results/samples/sample6.cs)]

Se uma ação do controlador retornar um **IHttpActionResult**, a API Web chamará o método **ExecuteAsync** para criar um **HttpResponseMessage**. Em seguida, ele converte o **HttpResponseMessage** em uma mensagem de resposta http.

Aqui está uma implementação simples de **IHttpActionResult** que cria uma resposta de texto sem formatação:

[!code-csharp[Main](action-results/samples/sample7.cs)]

Exemplo de ação do controlador:

[!code-csharp[Main](action-results/samples/sample8.cs)]

Responde

[!code-console[Main](action-results/samples/sample9.cmd)]

Com mais frequência, você usa as implementações **IHttpActionResult** definidas no namespace **[System. Web. http. Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** . A classe **ApiController** define os métodos auxiliares que retornam esses resultados de ação internos.

No exemplo a seguir, se a solicitação não corresponder a uma ID de produto existente, o controlador chamará [ApiController. não encontrado](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) para criar uma resposta 404 (não encontrada). Caso contrário, o controlador chama [ApiController. ok](https://msdn.microsoft.com/library/dn314591.aspx), que cria uma resposta de 200 (OK) que contém o produto.

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>Outros tipos de retorno

Para todos os outros tipos de retorno, a API da Web usa um formatador de [mídia](../formats-and-model-binding/media-formatters.md) para serializar o valor de retorno. A API da Web grava o valor serializado no corpo da resposta. O código de status de resposta é 200 (OK).

[!code-csharp[Main](action-results/samples/sample11.cs)]

Uma desvantagem dessa abordagem é que você não pode retornar diretamente um código de erro, como 404. No entanto, você pode gerar um httpresponseexception para códigos de erro. Para obter mais informações, consulte [tratamento de exceção em ASP.NET Web API](../error-handling/exception-handling.md).

A API Web usa o cabeçalho Accept na solicitação para escolher o formatador. Para obter mais informações, consulte [negociação de conteúdo](../formats-and-model-binding/content-negotiation.md).

Exemplo de solicitação

[!code-console[Main](action-results/samples/sample12.cmd)]

Resposta de exemplo

[!code-console[Main](action-results/samples/sample13.cmd)]
