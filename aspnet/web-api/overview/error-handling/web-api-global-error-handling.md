---
uid: web-api/overview/error-handling/web-api-global-error-handling
title: Tratamento de erro global no ASP.NET Web API 2-ASP.NET 4. x
author: davidmatson
description: Uma visão geral do tratamento de erros global no ASP.NET Web API 2 para ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: bffd7863-f63b-4b23-a13c-372b5492e9fb
msc.legacyurl: /web-api/overview/error-handling/web-api-global-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 94f2d6d31d0b37f9bb0077e6258c70a2dfb1918d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557277"
---
# <a name="global-error-handling-in-aspnet-web-api-2"></a>Tratamento de erro global no ASP.NET Web API 2

por [David Matson](https://github.com/davidmatson), [Rick Anderson](https://twitter.com/RickAndMSFT)

Este tópico fornece uma visão geral do tratamento de erros global no ASP.NET Web API 2 para ASP.NET 4. x. Hoje, não há uma maneira fácil na API Web registrar ou manipular erros globalmente. Algumas exceções sem tratamento podem ser processadas por meio de [filtros de exceção](exception-handling.md), mas há vários casos que os filtros de exceção não podem manipular. Por exemplo:

1. Exceções geradas por construtores de controlador.
2. Exceções geradas por manipuladores de mensagens.
3. Exceções geradas durante o roteamento.
4. Exceções geradas durante a serialização do conteúdo da resposta.

Queremos fornecer uma maneira simples e consistente de registrar em log e manipular (sempre que possível) essas exceções. 

Há dois casos principais para lidar com exceções, o caso em que é possível enviar uma resposta de erro e o caso em que tudo o que podemos fazer é registrar a exceção. Um exemplo para o último caso é quando uma exceção é lançada no meio do conteúdo da resposta de streaming; Nesse caso, é tarde demais para enviar uma nova mensagem de resposta, uma vez que o código de status, os cabeçalhos e o conteúdo parcial já passaram pela rede, então simplesmente anulamos a conexão. Embora a exceção não possa ser tratada para produzir uma nova mensagem de resposta, ainda damos suporte ao registro em log da exceção. Nos casos em que podemos detectar um erro, podemos retornar uma resposta de erro apropriada, conforme mostrado a seguir:

[!code-csharp[Main](web-api-global-error-handling/samples/sample1.cs?highlight=6)]

### <a name="existing-options"></a>Opções existentes

Além dos [filtros de exceção](exception-handling.md), os [manipuladores de mensagens](../advanced/http-message-handlers.md) podem ser usados hoje para observar todas as respostas de nível 500, mas agir nessas respostas é difícil, pois elas não têm contexto sobre o erro original. Os manipuladores de mensagens também têm algumas das mesmas limitações que os filtros de exceção em relação aos casos que eles podem manipular. Embora a API da Web tenha uma infraestrutura de rastreamento que capture condições de erro, a infraestrutura de rastreamento é para fins de diagnóstico e não é projetada ou adequada para execução em ambientes de produção. A manipulação de exceção global e o registro em log devem ser serviços que podem ser executados durante a produção e conectados a soluções de monitoramento existentes (por exemplo, [ELMAH](https://code.google.com/p/elmah/) ).

### <a name="solution-overview"></a>Visão geral da solução

 Fornecemos dois novos serviços substituíveis pelo usuário, [IExceptionLogger](../releases/whats-new-in-aspnet-web-api-21.md) e IExceptionHandler, para registrar e manipular exceções sem tratamento. Os serviços são muito semelhantes, com duas diferenças principais:

1. Damos suporte ao registro de vários agentes de exceção, mas apenas um único manipulador de exceção.
2. Os agentes de exceção sempre são chamados, mesmo que estejamos prestes a anular a conexão. Os manipuladores de exceção só são chamados quando ainda podemos escolher qual mensagem de resposta enviar.

Ambos os serviços fornecem acesso a um contexto de exceção que contém informações relevantes do ponto em que a exceção foi detectada, especialmente o [HttpRequestMessage](https://msdn.microsoft.com/library/system.net.http.httprequestmessage(v=vs.110).aspx), o [HttpRequestContext](https://msdn.microsoft.com/library/system.web.http.controllers.httprequestcontext(v=vs.118).aspx), a exceção gerada e a origem da exceção (detalhes abaixo).

### <a name="design-principles"></a>Princípios de design

1. **Sem alterações significativas** Como essa funcionalidade está sendo adicionada em uma versão secundária, uma restrição importante que afeta a solução é que não há nenhuma alteração significativa, seja para o tipo de contratos ou para o comportamento. Essa restrição descartava alguma limpeza que gostaríamos de ter feito em termos de blocos catch existentes, transformando exceções em 500 respostas. Essa limpeza adicional é algo que pode ser considerado para uma versão principal subsequente. Se isso for importante para você, vote nela em [ASP.NET Web API usuário Voice](http://aspnet.uservoice.com/forums/147201-asp-net-web-api/suggestions/5451321-add-flag-to-enable-iexceptionlogger-and-iexception).
2. **Manter a consistência com os constructos da API Web** O pipeline de filtro da API Web é uma ótima maneira de lidar com as preocupações abrangentes com a flexibilidade de aplicar a lógica em um escopo global ou específico do controlador de ação. Filtros, incluindo filtros de exceção, sempre têm contextos de ação e de controlador, mesmo quando registrados no escopo global. Esse contrato faz sentido para filtros, mas significa que os filtros de exceção, até mesmo com escopo global, não são uma boa opção para alguns casos de tratamento de exceção, como exceções de manipuladores de mensagens, onde não existe nenhum contexto de ação ou controlador. Se quisermos usar o escopo flexível proporcionado por filtros para manipulação de exceção, ainda precisamos de filtros de exceção. Mas se precisamos tratar a exceção fora de um contexto do controlador, também precisamos de uma construção separada para tratamento de erro global completo (algo sem o contexto do controlador e restrições de contexto de ação).

### <a name="when-to-use"></a>Quando usar

- Os agentes de exceção são a solução para ver toda exceção sem tratamento detectada pela API Web.
- Manipuladores de exceção são a solução para personalizar todas as respostas possíveis para exceções sem tratamento detectadas pela API da Web.
- Os filtros de exceção são a solução mais fácil para processar as exceções sem tratamento do subconjunto relacionadas a uma ação ou um controlador específico.

### <a name="service-details"></a>Detalhes do Serviço

 As interfaces do agente de log de exceções e do serviço são métodos assíncronos simples que levam aos respectivos contextos: 

[!code-csharp[Main](web-api-global-error-handling/samples/sample2.cs)]

 Também fornecemos classes base para ambas as interfaces. Substituir os métodos básicos (Sync ou Async) é tudo o que é necessário para registrar ou manipular nos tempos recomendados. Para registro em log, a classe base `ExceptionLogger` garantirá que o método de log principal seja chamado apenas uma vez para cada exceção (mesmo que posteriormente se propague mais adiante na pilha de chamadas e seja capturado novamente). A classe base `ExceptionHandler` chamará o método de tratamento de núcleo somente para exceções na parte superior da pilha de chamadas, ignorando blocos de captura aninhados herdados. (As versões simplificadas dessas classes base estão no apêndice abaixo.) Ambos `IExceptionLogger` e `IExceptionHandler` receber informações sobre a exceção por meio de um `ExceptionContext`.

[!code-csharp[Main](web-api-global-error-handling/samples/sample3.cs)]

Quando a estrutura chama um agente de log de exceção ou um manipulador de exceção, ela sempre fornecerá um `Exception` e um `Request`. Exceto para teste de unidade, ele também fornecerá sempre um `RequestContext`. Ele raramente fornecerá um `ControllerContext` e `ActionContext` (somente ao chamar do bloco catch para filtros de exceção). Muito raramente fornecerá um `Response`(somente em determinados casos do IIS, no meio de tentar gravar a resposta). Observe que, como algumas dessas propriedades podem ser `null` cabe ao consumidor verificar `null` antes de acessar os membros da classe de exceção.`CatchBlock` é uma cadeia de caracteres que indica qual bloco catch viu a exceção. As cadeias de caracteres de bloco catch são as seguintes:

- HttpServer (método SendAsync)
- HttpControllerDispatcher (método SendAsync)
- HttpBatchHandler (método SendAsync)
- IExceptionFilter (processamento de ApiController do pipeline de filtro de exceção no ExecuteAsync)
- Host OWIN:

    - HttpMessageHandlerAdapter. BufferResponseContentAsync (para saída de buffer)
    - HttpMessageHandlerAdapter. CopyResponseContentAsync (para saída de streaming)
- Host da Web:

    - HttpControllerHandler. WriteBufferedResponseContentAsync (para saída de buffer)
    - HttpControllerHandler. WriteStreamedResponseContentAsync (para saída de streaming)
    - HttpControllerHandler. WriteErrorResponseContentAsync (para falhas na recuperação de erro sob o modo de saída em buffer)

A lista de cadeias de caracteres de bloco catch também está disponível por meio de propriedades somente leitura. (A cadeia de caracteres do bloco catch principal está no ExceptionCatchBlocks estático; o restante aparece em uma classe estática cada para OWIN e host Web).`IsTopLevelCatchBlock` é útil para seguir o padrão recomendado de tratamento de exceções somente na parte superior da pilha de chamadas. Em vez de transformar as exceções em 500 respostas em qualquer lugar em que um bloco catch aninhado ocorra, um manipulador de exceção poderá permitir que as exceções se propaguem até que elas estejam prestes a ser vistas pelo host.

Além do `ExceptionContext`, um agente de log obtém mais uma informação por meio do `ExceptionLoggerContext`completo:

[!code-csharp[Main](web-api-global-error-handling/samples/sample4.cs)]

A segunda propriedade, `CanBeHandled`, permite que um agente de log identifique uma exceção que não pode ser tratada. Quando a conexão está prestes a ser anulada e nenhuma nova mensagem de resposta pode ser enviada, os agentes de log serão chamados, mas o manipulador ***não*** será chamado, e os agentes de log poderão identificar esse cenário a partir dessa propriedade.

Em adicionais ao `ExceptionContext`, um manipulador obtém mais uma propriedade que pode ser definida na `ExceptionHandlerContext` completa para manipular a exceção:

[!code-csharp[Main](web-api-global-error-handling/samples/sample5.cs)]

Um manipulador de exceção indica que ele tratou uma exceção definindo a propriedade `Result` como um resultado de ação (por exemplo, um [ExceptionResult](https://msdn.microsoft.com/library/system.web.http.results.exceptionresult(v=vs.118).aspx), [InternalServerErrorResult](https://msdn.microsoft.com/library/system.web.http.results.internalservererrorresult(v=vs.118).aspx), [StatusCodeResult](https://msdn.microsoft.com/library/system.web.http.results.statuscoderesult(v=vs.118).aspx)ou um resultado personalizado). Se a propriedade `Result` for nula, a exceção será sem tratamento e a exceção original será relançada.

Para exceções na parte superior da pilha de chamadas, pegamos uma etapa adicional para garantir que a resposta seja apropriada para chamadores de API. Se a exceção for propagada para o host, o chamador veria a tela amarela de morte ou outra resposta fornecida pelo host, que normalmente é HTML e geralmente não é uma resposta de erro de API apropriada. Nesses casos, o resultado começa não nulo e somente se um manipulador de exceção personalizado o definir explicitamente de volta para `null` (sem tratamento) a exceção será propagada para o host. A definição de `Result` para `null` nesses casos pode ser útil para dois cenários:

1. API web hospedada OWIN com middleware de manipulação de exceção personalizada registrado antes/fora da API Web.
2. Depuração local por meio de um navegador, onde a tela amarela de morte é, na verdade, uma resposta útil para uma exceção sem tratamento.

Para ambos os agentes de exceção e manipuladores de exceção, não fazemos nada para recuperar se o agente ou o próprio manipulador gera uma exceção. (Além de permitir que a exceção seja propagada, deixe os comentários na parte inferior desta página se você tiver uma abordagem melhor.) O contrato para agentes e manipuladores de exceções é que eles não devem permitir que as exceções se propaguem até seus chamadores; caso contrário, a exceção será apenas propagada, muitas vezes, até o host, resultando em um erro HTML (como o ASP. Tela amarela da rede) sendo enviada de volta ao cliente (que geralmente não é a opção preferida para chamadores de API que esperam JSON ou XML).

## <a name="examples"></a>Exemplos

### <a name="tracing-exception-logger"></a>Rastreando o agente de exceção

O agente de exceção abaixo envia dados de exceção para fontes de rastreamento configuradas (incluindo a janela de saída de depuração no Visual Studio).

[!code-csharp[Main](web-api-global-error-handling/samples/sample6.cs)]

### <a name="custom-error-message-exception-handler"></a>Manipulador de exceção de mensagem de erro personalizada

O seguinte abaixo produz uma resposta de erro personalizada para clientes, incluindo um endereço de email para contatar o suporte.

[!code-csharp[Main](web-api-global-error-handling/samples/sample7.cs)]

## <a name="registering-exception-filters"></a>Registrando filtros de exceção

Se você usar o modelo de projeto "aplicativo Web ASP.NET MVC 4" para criar seu projeto, coloque o código de configuração da API Web dentro da classe `WebApiConfig`, na pasta *app/_Start* :

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

## <a name="appendix-base-class-details"></a>Apêndice: detalhes da classe base

[!code-csharp[Main](web-api-global-error-handling/samples/sample8.cs)]
