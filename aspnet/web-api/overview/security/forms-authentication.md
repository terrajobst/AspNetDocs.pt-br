---
uid: web-api/overview/security/forms-authentication
title: Autenticação de formulários em ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: Descreve o uso da autenticação de formulários no ASP.NET Web API.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 147bfab76e48497f35a72b28cd935f40ec4193bf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598591"
---
# <a name="forms-authentication-in-aspnet-web-api"></a>Autenticação de formulários no ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

A autenticação de formulários usa um formulário HTML para enviar as credenciais do usuário para o servidor. Ele não é um padrão da Internet. A autenticação de formulários é apropriada somente para APIs Web que são chamadas de um aplicativo Web, para que o usuário possa interagir com o formulário HTML.

| Vantagens | Desvantagens |
| --- | --- |
| – Fácil de implementar: incorporada em ASP.NET. -Usa o provedor de associação do ASP.NET, que facilita o gerenciamento de contas de usuário. | -Não é um mecanismo de autenticação HTTP padrão; usa cookies HTTP em vez do cabeçalho de autorização padrão. -Requer um cliente de navegador. -As credenciais são enviadas como texto sem formatação. -Vulnerável a falsificação de solicitação entre sites (CSRF); requer medidas CSRF. -Difícil de usar de clientes não navegadores. O logon requer um navegador. -As credenciais do usuário são enviadas na solicitação. -Alguns usuários desabilitam cookies. |

Resumidamente, a autenticação de formulários no ASP.NET funciona da seguinte maneira:

1. O cliente solicita um recurso que requer autenticação.
2. Se o usuário não for autenticado, o servidor retornará HTTP 302 (encontrado) e redirecionará para uma página de logon.
3. O usuário insere as credenciais e envia o formulário.
4. O servidor retorna outro HTTP 302 que redireciona de volta para o URI original. Essa resposta inclui um cookie de autenticação.
5. O cliente solicita o recurso novamente. A solicitação inclui o cookie de autenticação, portanto, o servidor concede a solicitação.

![](forms-authentication/_static/image1.png)

Para obter mais informações, consulte [uma visão geral da autenticação de formulários.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Usando a autenticação de formulários com a API Web

Para criar um aplicativo que usa a autenticação de formulários, selecione o modelo "aplicativo de Internet" no assistente de projeto MVC 4. Este modelo cria controladores MVC para gerenciamento de conta. Você também pode usar o modelo "aplicativo de página única", disponível na atualização do ASP.NET Outono 2012.

Em seus controladores de API Web, você pode restringir o acesso usando o atributo `[Authorize]`, conforme descrito em [usando o atributo [autorizar]](authentication-and-authorization-in-aspnet-web-api.md#auth3).

Formulários-a autenticação usa um cookie de sessão para autenticar solicitações. Os navegadores enviam automaticamente todos os cookies relevantes para o site de destino. Esse recurso torna a autenticação de formulários potencialmente vulnerável a ataques CSRF (solicitação intersite forjada), consulte [prevenção de ataques de CSRF (solicitação entre sites forjada)](preventing-cross-site-request-forgery-csrf-attacks.md).

A autenticação de formulários não criptografa as credenciais do usuário. Portanto, a autenticação de formulários não é segura, a menos que seja usada com SSL. Consulte [trabalhando com SSL na API Web](working-with-ssl-in-web-api.md).
