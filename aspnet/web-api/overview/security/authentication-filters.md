---
uid: web-api/overview/security/authentication-filters
title: Filtros de autenticação no ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Um filtro de autenticação é um componente que autentica uma solicitação HTTP. A API Web 2 e MVC 5 oferecem suporte a filtros de autenticação, mas diferem ligeiramente...
ms.author: riande
ms.date: 09/25/2014
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 2ef9e62a6c634237e920b6d7aba2127b835f959d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555884"
---
# <a name="authentication-filters-in-aspnet-web-api-2"></a>Filtros de autenticação no ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

> Um filtro de autenticação é um componente que autentica uma solicitação HTTP. A API Web 2 e MVC 5 dão suporte a filtros de autenticação, mas diferem ligeiramente, principalmente nas convenções de nomenclatura da interface de filtro. Este tópico descreve os filtros de autenticação da API Web.

Os filtros de autenticação permitem definir um esquema de autenticação para controladores ou ações individuais. Dessa forma, seu aplicativo pode dar suporte a mecanismos de autenticação diferentes para diferentes recursos HTTP.

Neste artigo, mostrarei o código do exemplo de [autenticação básica](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BasicAuthentication) em [https://github.com/aspnet/samples](https://github.com/aspnet/samples). O exemplo mostra um filtro de autenticação que implementa o esquema de autenticação de acesso básico HTTP (RFC 2617). O filtro é implementado em uma classe chamada `IdentityBasicAuthenticationAttribute`. Não mostrarei todo o código do exemplo, apenas as partes que ilustram como escrever um filtro de autenticação.

## <a name="setting-an-authentication-filter"></a>Configurando um filtro de autenticação

Assim como outros filtros, os filtros de autenticação podem ser aplicados por controlador, por ação ou globalmente a todos os controladores de API da Web.

Para aplicar um filtro de autenticação a um controlador, decore a classe Controller com o atributo Filter. O código a seguir define o filtro de `[IdentityBasicAuthentication]` em uma classe de controlador, que habilita a autenticação básica para todas as ações do controlador.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Para aplicar o filtro a uma ação, decore a ação com o filtro. O código a seguir define o filtro de `[IdentityBasicAuthentication]` no método `Post` do controlador.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Para aplicar o filtro a todos os controladores de API da Web, adicione-o a **GlobalConfiguration. Filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implementando um filtro de autenticação de API Web

Na API Web, os filtros de autenticação implementam a interface [System. Web. http. Filters. IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) . Eles também devem herdar de **System. Attribute**, a fim de serem aplicados como atributos.

A interface **IAuthenticationFilter** tem dois métodos:

- O **AuthenticateAsync** autentica a solicitação validando as credenciais na solicitação, se presente.
- O **ChallengeAsync** adiciona um desafio de autenticação à resposta http, se necessário.

Esses métodos correspondem ao fluxo de autenticação definido em [rfc 2612](http://tools.ietf.org/html/rfc2616) e [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. O cliente envia credenciais no cabeçalho de autorização. Isso normalmente acontece depois que o cliente recebe uma resposta 401 (não autorizada) do servidor. No entanto, um cliente pode enviar credenciais com qualquer solicitação, não apenas depois de obter um 401.
2. Se o servidor não aceitar as credenciais, ele retornará uma resposta 401 (não autorizada). A resposta inclui um cabeçalho WWW-Authenticate que contém um ou mais desafios. Cada desafio especifica um esquema de autenticação reconhecido pelo servidor.

O servidor também pode retornar 401 de uma solicitação anônima. Na verdade, é normalmente assim que o processo de autenticação é iniciado:

1. O cliente envia uma solicitação anônima.
2. O servidor retorna 401.
3. Os clientes reenviam a solicitação com credenciais.

Esse fluxo inclui as etapas de *autenticação* e *autorização* .

- A autenticação comprova a identidade do cliente.
- A autorização determina se o cliente pode acessar um recurso específico.

Na API Web, os filtros de autenticação manipulam a autenticação, mas não a autorização. A autorização deve ser feita por um filtro de autorização ou dentro da ação do controlador.

Este é o fluxo no pipeline da API Web 2:

1. Antes de invocar uma ação, a API da Web cria uma lista dos filtros de autenticação para essa ação. Isso inclui filtros com escopo de ação, escopo do controlador e escopo global.
2. A API Web chama **AuthenticateAsync** em cada filtro na lista. Cada filtro pode validar credenciais na solicitação. Se qualquer filtro validar as credenciais com êxito, o filtro criará uma **IPrincipal** e a anexará à solicitação. Um filtro também pode disparar um erro neste momento. Nesse caso, o restante do pipeline não é executado.
3. Supondo que não haja erro, a solicitação flui pelo resto do pipeline.
4. Por fim, a API Web chama o método **ChallengeAsync** de cada filtro de autenticação. Os filtros usam esse método para adicionar um desafio à resposta, se necessário. Normalmente (mas nem sempre) que aconteceria em resposta a um erro 401.

Os diagramas a seguir mostram dois casos possíveis. Na primeira, o filtro de autenticação autentica a solicitação com êxito, um filtro de autorização autoriza a solicitação e a ação do controlador retorna 200 (OK).

![](authentication-filters/_static/image1.png)

No segundo exemplo, o filtro de autenticação autentica a solicitação, mas o filtro de autorização retorna 401 (não autorizado). Nesse caso, a ação do controlador não é invocada. O filtro de autenticação adiciona um cabeçalho WWW-Authenticate à resposta.

![](authentication-filters/_static/image2.png)

Outras combinações são possíveis&mdash;por exemplo, se a ação do controlador permitir solicitações anônimas, você poderá ter um filtro de autenticação, mas nenhuma autorização.

## <a name="implementing-the-authenticateasync-method"></a>Implementando o método AuthenticateAsync

O método **AuthenticateAsync** tenta autenticar a solicitação. Veja a assinatura do método:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

O método **AuthenticateAsync** deve seguir um destes procedimentos:

1. Nada (no-op).
2. Crie um **IPrincipal** e defina-o na solicitação.
3. Defina um resultado de erro.

Opção (1) significa que a solicitação não tinha nenhuma credencial que o filtro entenda. Opção (2) significa que o filtro autenticou a solicitação com êxito. Opção (3) significa que a solicitação tinha credenciais inválidas (como a senha incorreta), que dispara uma resposta de erro.

Aqui está uma estrutura geral para implementar **AuthenticateAsync**.

1. Procure as credenciais na solicitação.
2. Se não houver nenhuma credencial, não faça nada e retorne (no-op).
3. Se houver credenciais, mas o filtro não reconhecer o esquema de autenticação, não faça nada e retorne (no-op). Outro filtro no pipeline pode entender o esquema.
4. Se houver credenciais que o filtro entenda, tente autenticá-las.
5. Se as credenciais forem ruins, retorne 401 definindo `context.ErrorResult`.
6. Se as credenciais forem válidas, crie um **IPrincipal** e defina `context.Principal`.

O código a seguir mostra o método **AuthenticateAsync** do exemplo de [autenticação básica](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BasicAuthentication) . Os comentários indicam cada etapa. O código mostra vários tipos de erro: um cabeçalho de autorização sem credenciais, credenciais malformadas e nome de usuário/senha incorretos.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Configurando um resultado de erro

Se as credenciais forem inválidas, o filtro deverá definir `context.ErrorResult` como um **IHttpActionResult** que cria uma resposta de erro. Para obter mais informações sobre **IHttpActionResult**, consulte [resultados da ação na API da Web 2](../getting-started-with-aspnet-web-api/action-results.md).

O exemplo de autenticação básica inclui uma classe `AuthenticationFailureResult` que é adequada para essa finalidade.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementando ChallengeAsync

A finalidade do método **ChallengeAsync** é adicionar desafios de autenticação à resposta, se necessário. Veja a assinatura do método:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

O método é chamado em cada filtro de autenticação no pipeline de solicitação.

É importante entender que **ChallengeAsync** é chamado *antes* da criação da resposta http e, possivelmente, antes da execução da ação do controlador. Quando **ChallengeAsync** é chamado, `context.Result` contém um **IHttpActionResult**, que é usado posteriormente para criar a resposta http. Assim, quando **ChallengeAsync** for chamado, você ainda não saberá nada sobre a resposta http. O método **ChallengeAsync** deve substituir o valor original de `context.Result` por um novo **IHttpActionResult**. Esse **IHttpActionResult** deve encapsular o `context.Result`original.

![](authentication-filters/_static/image3.png)

Chamarei o **IHttpActionResult** original do *resultado interno*e o novo **IHttpActionResult** o *resultado externo*. O resultado externo deve fazer o seguinte:

1. Invoque o resultado interno para criar a resposta HTTP.
2. Examine a resposta.
3. Adicione um desafio de autenticação à resposta, se necessário.

O exemplo a seguir é obtido do exemplo de autenticação básica. Ele define um **IHttpActionResult** para o resultado externo.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

A propriedade `InnerResult` mantém o **IHttpActionResult**interno. A propriedade `Challenge` representa um cabeçalho www-Authentication. Observe que o **ExecuteAsync** primeiro chama `InnerResult.ExecuteAsync` para criar a resposta http e, em seguida, adiciona o desafio, se necessário.

Verifique o código de resposta antes de adicionar o desafio. A maioria dos esquemas de autenticação só adicionará um desafio se a resposta for 401, como mostrado aqui. No entanto, alguns esquemas de autenticação adicionam um desafio a uma resposta de êxito. Por exemplo, consulte [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Considerando a classe `AddChallengeOnUnauthorizedResult`, o código real em **ChallengeAsync** é simples. Basta criar o resultado e anexá-lo a `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Observação: o exemplo de autenticação básica abstrai essa lógica um pouco, colocando-a em um método de extensão.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Combinando filtros de autenticação com autenticação no nível do host

"Autenticação no nível do host" é a autenticação executada pelo host (como o IIS), antes que a solicitação atinja a estrutura da API da Web.

Geralmente, talvez você queira habilitar a autenticação em nível de host para o restante do seu aplicativo, mas desabilitá-lo para seus controladores de API Web. Por exemplo, um cenário típico é habilitar a autenticação de formulários no nível do host, mas usar a autenticação baseada em token para a API da Web.

Para desabilitar a autenticação em nível de host dentro do pipeline da API Web, chame `config.SuppressHostPrincipal()` em sua configuração. Isso faz com que a API da Web remova a **IPrincipal** de qualquer solicitação que entra no pipeline da API Web. Efetivamente, ele &quot;cancelar a autenticação&quot; solicitação.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Recursos adicionais

[Filtros de segurança ASP.NET Web API](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)
