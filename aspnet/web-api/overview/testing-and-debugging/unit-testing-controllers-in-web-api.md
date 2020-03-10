---
uid: web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
title: Controladores de teste de unidade no ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Este tópico descreve algumas técnicas específicas para controladores de teste de unidade na API Web 2. Antes de ler este tópico, talvez você queira ler a unidade do tutorial...
ms.author: riande
ms.date: 06/11/2014
ms.assetid: 43a6cce7-a3ef-42aa-ad06-90d36d49f098
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-controllers-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: cdb1700537021e276669de1a9e0330a62659746c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554988"
---
# <a name="unit-testing-controllers-in-aspnet-web-api-2"></a>Controladores de teste de unidade no ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

> Este tópico descreve algumas técnicas específicas para controladores de teste de unidade na API Web 2. Antes de ler este tópico, talvez você queira ler o tutorial [teste de unidade ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), que mostra como adicionar um projeto de teste de unidade à sua solução.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - API Web 2
> - [MOQ](https://github.com/Moq) 4.5.30

> [!NOTE]
> Usei MOQ, mas a mesma ideia se aplica a qualquer estrutura fictícia. O MOQ 4.5.30 (e posterior) dá suporte ao Visual Studio 2017, Roslyn e .NET 4,5 e versões posteriores.

Um padrão comum em testes de unidade é &quot;o Arrange-Act-Assert&quot;:

- Organizar: configure todos os pré-requisitos para que o teste seja executado.
- Act: execute o teste.
- Assert: Verifique se o teste foi bem-sucedido.

Na etapa organizar, você geralmente usará objetos fictícios ou de stub. Isso minimiza o número de dependências, de modo que o teste se concentra em testar uma coisa.

Aqui estão algumas coisas que você deve testar unidade em seus controladores de API Web:

- A ação retorna o tipo correto de resposta.
- Parâmetros inválidos retornam a resposta de erro correta.
- A ação chama o método correto no repositório ou na camada de serviço.
- Se a resposta incluir um modelo de domínio, verifique o tipo de modelo.

Essas são algumas das coisas gerais a serem testadas, mas as especificações dependem da implementação do controlador. Em particular, faz uma grande diferença se as ações do controlador retornarem **HttpResponseMessage** ou **IHttpActionResult**. Para obter mais informações sobre esses tipos de resultado, consulte [resultados da ação na API da Web 2](../getting-started-with-aspnet-web-api/action-results.md).

## <a name="testing-actions-that-return-httpresponsemessage"></a>Ações de teste que retornam HttpResponseMessage

Aqui está um exemplo de um controlador cujas ações retornam **HttpResponseMessage**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample1.cs)]

Observe que o controlador usa injeção de dependência para injetar um `IProductRepository`. Isso torna o controlador mais testado, pois você pode injetar um repositório fictício. O teste de unidade a seguir verifica se o método `Get` grava um `Product` no corpo da resposta. Suponha que `repository` seja uma simulação `IProductRepository`.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample2.cs)]

É importante definir a **solicitação** e a **configuração** no controlador. Caso contrário, o teste falhará com um **ArgumentNullException** ou **InvalidOperationException**.

## <a name="testing-link-generation"></a>Testando a geração de link

O método `Post` chama **UrlHelper. link** para criar links na resposta. Isso requer um pouco mais de configuração no teste de unidade:

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample3.cs)]

A classe **UrlHelper** precisa da URL de solicitação e dos dados de rota, portanto, o teste precisa definir valores para eles. Outra opção é imitação ou **UrlHelper**de stub. Com essa abordagem, você substitui o valor padrão de [ApiController. URL](https://msdn.microsoft.com/library/system.web.http.apicontroller.url.aspx) por uma simulação ou versão de stub que retorna um valor fixo.

Vamos reescrever o teste usando a estrutura [MOQ](https://github.com/Moq) . Instale o `Moq` pacote NuGet no projeto de teste.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample4.cs)]

Nesta versão, você não precisa configurar nenhum dado de rota, pois o **UrlHelper** de simulação retorna uma cadeia de caracteres constante.

## <a name="testing-actions-that-return-ihttpactionresult"></a>Ações de teste que retornam IHttpActionResult

Na API da Web 2, uma ação do controlador pode retornar **IHttpActionResult**, que é análogo ao **ACTIONRESULT** no ASP.NET MVC. A interface **IHttpActionResult** define um padrão de comando para criar respostas http. Em vez de criar a resposta diretamente, o controlador retorna um **IHttpActionResult**. Posteriormente, o pipeline invoca o **IHttpActionResult** para criar a resposta. Essa abordagem facilita a gravação de testes de unidade, pois você pode ignorar muitas das configurações necessárias para o **HttpResponseMessage**.

Aqui está um controlador de exemplo cujas ações retornam **IHttpActionResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample5.cs)]

Este exemplo mostra alguns padrões comuns usando **IHttpActionResult**. Vamos ver como testar as unidades.

### <a name="action-returns-200-ok-with-a-response-body"></a>A ação retorna 200 (OK) com um corpo de resposta

O método `Get` chama `Ok(product)` se o produto for encontrado. No teste de unidade, verifique se o tipo de retorno é **OkNegotiatedContentResult** e se o produto retornado tem a ID correta.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample6.cs)]

Observe que o teste de unidade não executa o resultado da ação. Você pode assumir que o resultado da ação cria a resposta HTTP corretamente. (É por isso que a estrutura da API Web tem seus próprios testes de unidade!)

### <a name="action-returns-404-not-found"></a>A ação retorna 404 (não encontrado)

O método `Get` chama `NotFound()` se o produto não for encontrado. Para esse caso, o teste de unidade apenas verifica se o tipo de retorno é **NotFoundResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample7.cs)]

### <a name="action-returns-200-ok-with-no-response-body"></a>A ação retorna 200 (OK) sem corpo de resposta

O método `Delete` chama `Ok()` para retornar uma resposta HTTP 200 vazia. Como no exemplo anterior, o teste de unidade verifica o tipo de retorno, neste caso, **OkResult**.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample8.cs)]

### <a name="action-returns-201-created-with-a-location-header"></a>A ação retorna 201 (criado) com um cabeçalho de local

O método `Post` chama `CreatedAtRoute` para retornar uma resposta HTTP 201 com um URI no cabeçalho de local. No teste de unidade, verifique se a ação define os valores de roteamento corretos.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample9.cs)]

### <a name="action-returns-another-2xx-with-a-response-body"></a>A ação retorna outro 2xx com um corpo de resposta

O método `Put` chama `Content` para retornar uma resposta HTTP 202 (aceito) com um corpo de resposta. Esse caso é semelhante a retornar 200 (OK), mas o teste de unidade também deve verificar o código de status.

[!code-csharp[Main](unit-testing-controllers-in-web-api/samples/sample10.cs)]

## <a name="additional-resources"></a>Recursos adicionais

- [Simulação de Entity Framework quando o teste de unidade ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)
- [Gravando testes para um serviço de ASP.NET Web API](https://blogs.msdn.com/b/youssefm/archive/2013/01/28/writing-tests-for-an-asp-net-webapi-service.aspx) (postagem de blog de Youssef Moussaoui).
- [Depurando ASP.NET Web API com o depurador de rota](https://blogs.msdn.com/b/webdev/archive/2013/04/04/debugging-asp-net-web-api-with-route-debugger.aspx)
