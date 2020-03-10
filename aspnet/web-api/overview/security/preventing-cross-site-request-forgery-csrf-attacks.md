---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Impedindo ataques CSRF (solicitação intersite forjada) no ASP.NET MVC
author: MikeWasson
description: Descreve o ataque CSRF (solicitação entre sites forjada) e como implementar medidas CSRF no ASP.NET Web MVC.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: 5fb0f8bcc9e587ba4fbbf2b857d3bf7adcaafb94
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555114"
---
# <a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-mvc-application"></a>Impedindo ataques CSRF (solicitação entre sites forjada) no aplicativo ASP.NET MVC

por [Mike Wasson](https://github.com/MikeWasson)

A CSRF (solicitação intersite forjada) é um ataque em que um site mal-intencionado envia uma solicitação para um site vulnerável em que o usuário está conectado no momento

Aqui está um exemplo de um ataque de CSRF:

1. Um usuário faz logon em `www.example.com` usando a autenticação de formulários.
2. O servidor autentica o usuário. A resposta do servidor inclui um cookie de autenticação.
3. Sem fazer logoff, o usuário visita um site mal-intencionado. Este site mal-intencionado contém o seguinte formulário HTML: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Observe que a ação do formulário é postagem no site vulnerável, não no site mal-intencionado. Esta é a parte "entre sites" do CSRF.
4. O usuário clica no botão enviar. O navegador inclui o cookie de autenticação com a solicitação.
5. A solicitação é executada no servidor com o contexto de autenticação do usuário e pode fazer tudo o que um usuário autenticado tem permissão para fazer.

Embora este exemplo exija que o usuário clique no botão formulário, a página mal-intencionada poderia simplesmente executar um script que envia o formulário automaticamente. Além disso, o uso do SSL não impede um ataque CSRF, porque o site mal-intencionado pode enviar uma solicitação "https://".

Normalmente, os ataques de CSRF são possíveis em sites que usam cookies para autenticação, pois os navegadores enviam todos os cookies relevantes para o site de destino. No entanto, os ataques CSRF não estão limitados à exploração de cookies. Por exemplo, a autenticação básica e resumida também são vulneráveis. Depois que um usuário faz logon com a autenticação básica ou resumida. o navegador envia automaticamente as credenciais até que a sessão termine.

## <a name="anti-forgery-tokens"></a>Tokens antifalsificação

Para ajudar a evitar ataques de CSRF, o ASP.NET MVC usa tokens antifalsificação, também chamados de *tokens de verificação de solicitação*.

1. O cliente solicita uma página HTML que contém um formulário.
2. O servidor inclui dois tokens na resposta. Um token é enviado como um cookie. O outro é colocado em um campo de formulário oculto. Os tokens são gerados aleatoriamente para que um adversário não possa adivinhar os valores.
3. Quando o cliente envia o formulário, ele deve enviar os dois tokens de volta ao servidor. O cliente envia o token de cookie como um cookie e envia o token de formulário dentro dos dados do formulário. (Um cliente de navegador faz isso automaticamente quando o usuário envia o formulário.)
4. Se uma solicitação não incluir ambos os tokens, o servidor não permitirá a solicitação.

Aqui está um exemplo de um formulário HTML com um token de formulário oculto:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

Os tokens antifalsificação funcionam porque a página mal-intencionada não pode ler os tokens do usuário, devido às políticas de mesma origem. ([As políticas de mesma origem](http://www.w3.org/Security/wiki/Same_Origin_Policy) impedem que os documentos hospedados em dois sites diferentes acessem o conteúdo de cada um. Portanto, no exemplo anterior, a página mal-intencionada pode enviar solicitações para example.com, mas não pode ler a resposta.)

Para evitar ataques de CSRF, use tokens antifalsificação com qualquer protocolo de autenticação em que o navegador envie silenciosamente as credenciais depois que o usuário fizer logon. Isso inclui protocolos de autenticação baseados em cookies, como a autenticação de formulários, bem como protocolos como a autenticação básica e resumida.

Você deve exigir tokens antifalsificação para quaisquer métodos não seguros (POST, PUT e DELETE). Além disso, certifique-se de que os métodos seguros (GET, HEAD) não tenham nenhum efeito colateral. Além disso, se você habilitar o suporte entre domínios, como CORS ou JSONP, até mesmo métodos seguros como GET são potencialmente vulneráveis a ataques de CSRF, permitindo que o invasor leia dados potencialmente confidenciais.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Tokens antifalsificação no ASP.NET MVC

Para adicionar os tokens antifalsificação a uma página Razor, use o método auxiliar **HtmlHelper. AntiForgeryToken** :

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Esse método adiciona o campo de formulário oculto e também define o token do cookie.

## <a name="anti-csrf-and-ajax"></a>Anti-CSRF e AJAX

O token de formulário pode ser um problema para solicitações AJAX, porque uma solicitação AJAX pode enviar dados JSON, não dados de formulário HTML. Uma solução é enviar os tokens em um cabeçalho HTTP personalizado. O código a seguir usa a sintaxe Razor para gerar os tokens e adiciona os tokens a uma solicitação AJAX. Os tokens são gerados no servidor chamando **antifalsificação. Gettokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Ao processar a solicitação, extraia os tokens do cabeçalho da solicitação. Em seguida, chame o método **antifalsificação. Validate** para validar os tokens. O método **Validate** gera uma exceção se os tokens não forem válidos.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
