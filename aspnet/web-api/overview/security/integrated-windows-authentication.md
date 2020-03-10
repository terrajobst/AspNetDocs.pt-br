---
uid: web-api/overview/security/integrated-windows-authentication
title: Autenticação integrada do Windows | Microsoft Docs
author: MikeWasson
description: Descreve o uso da autenticação integrada do Windows no ASP.NET Web API.
ms.author: riande
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: e4f31f191f3c0fabff308ea5dadb0f1d9ce7d448
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621740"
---
# <a name="integrated-windows-authentication"></a>Autenticação Integrada do Windows

por [Mike Wasson](https://github.com/MikeWasson)

A autenticação integrada do Windows permite que os usuários façam logon com suas credenciais do Windows, usando Kerberos ou NTLM. O cliente envia credenciais no cabeçalho de autorização. A autenticação do Windows é mais adequada para um ambiente de intranet. Para obter mais informações, veja [Autenticação do Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Vantagens | Desvantagens |
| --- | --- |
| -Interno ao IIS. -Não envia as credenciais do usuário na solicitação. -Se o computador cliente pertencer ao domínio (por exemplo, aplicativo de intranet), o usuário não precisará inserir as credenciais. | -Não é recomendado para aplicativos da Internet. -Requer suporte a Kerberos ou NTLM no cliente. -O cliente deve estar no domínio Active Directory. |

> [!NOTE]
> Se seu aplicativo estiver hospedado no Azure e você tiver um domínio de Active Directory local, considere a Federação de seu anúncio local com Azure Active Directory. Dessa forma, os usuários podem fazer logon com suas credenciais locais, mas a autenticação é executada pelo Azure AD. Para obter mais informações, consulte [autenticação do Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).

Para criar um aplicativo que usa a autenticação integrada do Windows, selecione o modelo "aplicativo de intranet" no assistente de projeto MVC 4. Este modelo de projeto coloca a seguinte configuração no arquivo Web. config:

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

No lado do cliente, a autenticação integrada do Windows funciona com qualquer navegador que dê suporte ao esquema de autenticação [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) , que inclui a maioria dos principais navegadores. Para aplicativos cliente .NET, a classe **HttpClient** dá suporte à autenticação do Windows:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

A autenticação do Windows é vulnerável a ataques CSRF (solicitação entre sites forjada). Consulte [impedindo ataques CSRF (solicitação entre sites forjada)](preventing-cross-site-request-forgery-csrf-attacks.md).
