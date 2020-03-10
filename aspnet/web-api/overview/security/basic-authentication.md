---
uid: web-api/overview/security/basic-authentication
title: Autenticação básica no ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: Descreve o uso da autenticação básica no ASP.NET Web API.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1470bd4b5abd5199b9a5105973b053812d643351
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555723"
---
# <a name="basic-authentication-in-aspnet-web-api"></a>Autenticação básica no ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

A autenticação básica é definida em [RFC 2617, autenticação http: autenticação de acesso básica e Digest](http://www.ietf.org/rfc/rfc2617.txt).

Desvantagens

- As credenciais do usuário são enviadas na solicitação.
- As credenciais são enviadas como texto sem formatação.
- As credenciais são enviadas com cada solicitação.
- Não há como fazer logoff, exceto encerrando a sessão do navegador.
- Vulnerável a falsificação de solicitação entre sites (CSRF); requer medidas CSRF.

Vantagens

- Padrão da Internet.
- Compatível com todos os principais navegadores.
- Protocolo relativamente simples.

A autenticação básica funciona da seguinte maneira:

1. Se uma solicitação exigir autenticação, o servidor retornará 401 (não autorizado). A resposta inclui um cabeçalho WWW-Authenticate, indicando que o servidor dá suporte à autenticação básica.
2. O cliente envia outra solicitação, com as credenciais do cliente no cabeçalho de autorização. As credenciais são formatadas como a cadeia de caracteres "nome: senha", codificada em base64. As credenciais não são criptografadas.

A autenticação básica é executada no contexto de um "Realm". O servidor inclui o nome do Realm no cabeçalho WWW-Authenticate. As credenciais do usuário são válidas dentro desse realm. O escopo exato de um realm é definido pelo servidor. Por exemplo, você pode definir vários territórios para particionar recursos.

![](basic-authentication/_static/image1.png)

Como as credenciais são enviadas não criptografadas, a autenticação básica só é segura por HTTPS. Consulte [trabalhando com SSL na API Web](working-with-ssl-in-web-api.md).

A autenticação básica também é vulnerável a ataques CSRF. Depois que o usuário insere as credenciais, o navegador as envia automaticamente em solicitações subsequentes para o mesmo domínio, durante a sessão. Isso inclui solicitações AJAX. Consulte [impedindo ataques CSRF (solicitação entre sites forjada)](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Autenticação básica com o IIS

O IIS dá suporte à autenticação básica, mas há uma limitação: o usuário é autenticado em relação às suas credenciais do Windows. Isso significa que o usuário deve ter uma conta no domínio do servidor. Para um site voltado ao público, normalmente você deseja se autenticar em um provedor de associação do ASP.NET.

Para habilitar a autenticação básica usando o IIS, defina o modo de autenticação como "Windows" no Web. config do seu projeto do ASP.NET:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

Nesse modo, o IIS usa credenciais do Windows para autenticar. Além disso, você deve habilitar a autenticação básica no IIS. No Gerenciador do IIS, vá para o modo de exibição de recursos, selecione autenticação e habilite a autenticação básica.

![](basic-authentication/_static/image2.png)

Em seu projeto de API Web, adicione o atributo `[Authorize]` para qualquer ação de controlador que precise de autenticação.

Um cliente se autentica sozinho definindo o cabeçalho de autorização na solicitação. Os clientes de navegador executam essa etapa automaticamente. Os clientes que não são navegadores precisarão definir o cabeçalho.

## <a name="basic-authentication-with-custom-membership"></a>Autenticação básica com associação personalizada

Conforme mencionado, a autenticação básica interna do IIS usa as credenciais do Windows. Isso significa que você precisa criar contas para seus usuários no servidor de hospedagem. Mas, para um aplicativo de Internet, as contas de usuário são normalmente armazenadas em um banco de dados externo.

O código a seguir apresenta como um módulo HTTP que executa a autenticação básica. Você pode facilmente conectar um provedor de associação do ASP.NET substituindo o método `CheckPassword`, que é um método fictício neste exemplo.

Na API Web 2, você deve considerar escrever um [filtro de autenticação](authentication-filters.md) ou um [middleware OWIN](../../../aspnet/overview/owin-and-katana/index.md), em vez de um módulo http.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Para habilitar o módulo HTTP, adicione o seguinte ao seu arquivo Web. config na seção **System. WebServer** :

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Substitua "YourAssemblyName" pelo nome do assembly (sem incluir a extensão "dll").

Você deve desabilitar outros esquemas de autenticação, como formulários ou autenticação do Windows.
