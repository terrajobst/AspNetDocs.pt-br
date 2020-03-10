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
# <a name="integrated-windows-authentication"></a><span data-ttu-id="e15fc-103">Autenticação Integrada do Windows</span><span class="sxs-lookup"><span data-stu-id="e15fc-103">Integrated Windows Authentication</span></span>

<span data-ttu-id="e15fc-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e15fc-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e15fc-105">A autenticação integrada do Windows permite que os usuários façam logon com suas credenciais do Windows, usando Kerberos ou NTLM.</span><span class="sxs-lookup"><span data-stu-id="e15fc-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="e15fc-106">O cliente envia credenciais no cabeçalho de autorização.</span><span class="sxs-lookup"><span data-stu-id="e15fc-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="e15fc-107">A autenticação do Windows é mais adequada para um ambiente de intranet.</span><span class="sxs-lookup"><span data-stu-id="e15fc-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="e15fc-108">Para obter mais informações, veja [Autenticação do Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="e15fc-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="e15fc-109">Vantagens</span><span class="sxs-lookup"><span data-stu-id="e15fc-109">Advantages</span></span> | <span data-ttu-id="e15fc-110">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="e15fc-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="e15fc-111">-Interno ao IIS.</span><span class="sxs-lookup"><span data-stu-id="e15fc-111">- Built into IIS.</span></span> <span data-ttu-id="e15fc-112">-Não envia as credenciais do usuário na solicitação.</span><span class="sxs-lookup"><span data-stu-id="e15fc-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="e15fc-113">-Se o computador cliente pertencer ao domínio (por exemplo, aplicativo de intranet), o usuário não precisará inserir as credenciais.</span><span class="sxs-lookup"><span data-stu-id="e15fc-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="e15fc-114">-Não é recomendado para aplicativos da Internet.</span><span class="sxs-lookup"><span data-stu-id="e15fc-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="e15fc-115">-Requer suporte a Kerberos ou NTLM no cliente.</span><span class="sxs-lookup"><span data-stu-id="e15fc-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="e15fc-116">-O cliente deve estar no domínio Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e15fc-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="e15fc-117">Se seu aplicativo estiver hospedado no Azure e você tiver um domínio de Active Directory local, considere a Federação de seu anúncio local com Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e15fc-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="e15fc-118">Dessa forma, os usuários podem fazer logon com suas credenciais locais, mas a autenticação é executada pelo Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e15fc-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="e15fc-119">Para obter mais informações, consulte [autenticação do Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="e15fc-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>

<span data-ttu-id="e15fc-120">Para criar um aplicativo que usa a autenticação integrada do Windows, selecione o modelo "aplicativo de intranet" no assistente de projeto MVC 4.</span><span class="sxs-lookup"><span data-stu-id="e15fc-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="e15fc-121">Este modelo de projeto coloca a seguinte configuração no arquivo Web. config:</span><span class="sxs-lookup"><span data-stu-id="e15fc-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="e15fc-122">No lado do cliente, a autenticação integrada do Windows funciona com qualquer navegador que dê suporte ao esquema de autenticação [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) , que inclui a maioria dos principais navegadores.</span><span class="sxs-lookup"><span data-stu-id="e15fc-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="e15fc-123">Para aplicativos cliente .NET, a classe **HttpClient** dá suporte à autenticação do Windows:</span><span class="sxs-lookup"><span data-stu-id="e15fc-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="e15fc-124">A autenticação do Windows é vulnerável a ataques CSRF (solicitação entre sites forjada).</span><span class="sxs-lookup"><span data-stu-id="e15fc-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="e15fc-125">Consulte [impedindo ataques CSRF (solicitação entre sites forjada)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="e15fc-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
