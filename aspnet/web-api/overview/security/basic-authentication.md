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
# <a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="ad6a8-103">Autenticação básica no ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="ad6a8-103">Basic Authentication in ASP.NET Web API</span></span>

<span data-ttu-id="ad6a8-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="ad6a8-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="ad6a8-105">A autenticação básica é definida em [RFC 2617, autenticação http: autenticação de acesso básica e Digest](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="ad6a8-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="ad6a8-106">Desvantagens</span><span class="sxs-lookup"><span data-stu-id="ad6a8-106">Disadvantages</span></span>

- <span data-ttu-id="ad6a8-107">As credenciais do usuário são enviadas na solicitação.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="ad6a8-108">As credenciais são enviadas como texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="ad6a8-109">As credenciais são enviadas com cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="ad6a8-110">Não há como fazer logoff, exceto encerrando a sessão do navegador.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="ad6a8-111">Vulnerável a falsificação de solicitação entre sites (CSRF); requer medidas CSRF.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="ad6a8-112">Vantagens</span><span class="sxs-lookup"><span data-stu-id="ad6a8-112">Advantages</span></span>

- <span data-ttu-id="ad6a8-113">Padrão da Internet.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-113">Internet standard.</span></span>
- <span data-ttu-id="ad6a8-114">Compatível com todos os principais navegadores.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="ad6a8-115">Protocolo relativamente simples.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-115">Relatively simple protocol.</span></span>

<span data-ttu-id="ad6a8-116">A autenticação básica funciona da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="ad6a8-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="ad6a8-117">Se uma solicitação exigir autenticação, o servidor retornará 401 (não autorizado).</span><span class="sxs-lookup"><span data-stu-id="ad6a8-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="ad6a8-118">A resposta inclui um cabeçalho WWW-Authenticate, indicando que o servidor dá suporte à autenticação básica.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="ad6a8-119">O cliente envia outra solicitação, com as credenciais do cliente no cabeçalho de autorização.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="ad6a8-120">As credenciais são formatadas como a cadeia de caracteres "nome: senha", codificada em base64.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="ad6a8-121">As credenciais não são criptografadas.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="ad6a8-122">A autenticação básica é executada no contexto de um "Realm".</span><span class="sxs-lookup"><span data-stu-id="ad6a8-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="ad6a8-123">O servidor inclui o nome do Realm no cabeçalho WWW-Authenticate.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="ad6a8-124">As credenciais do usuário são válidas dentro desse realm.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="ad6a8-125">O escopo exato de um realm é definido pelo servidor.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="ad6a8-126">Por exemplo, você pode definir vários territórios para particionar recursos.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="ad6a8-127">Como as credenciais são enviadas não criptografadas, a autenticação básica só é segura por HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="ad6a8-128">Consulte [trabalhando com SSL na API Web](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="ad6a8-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="ad6a8-129">A autenticação básica também é vulnerável a ataques CSRF.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="ad6a8-130">Depois que o usuário insere as credenciais, o navegador as envia automaticamente em solicitações subsequentes para o mesmo domínio, durante a sessão.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="ad6a8-131">Isso inclui solicitações AJAX.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-131">This includes AJAX requests.</span></span> <span data-ttu-id="ad6a8-132">Consulte [impedindo ataques CSRF (solicitação entre sites forjada)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="ad6a8-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="ad6a8-133">Autenticação básica com o IIS</span><span class="sxs-lookup"><span data-stu-id="ad6a8-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="ad6a8-134">O IIS dá suporte à autenticação básica, mas há uma limitação: o usuário é autenticado em relação às suas credenciais do Windows.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="ad6a8-135">Isso significa que o usuário deve ter uma conta no domínio do servidor.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="ad6a8-136">Para um site voltado ao público, normalmente você deseja se autenticar em um provedor de associação do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="ad6a8-137">Para habilitar a autenticação básica usando o IIS, defina o modo de autenticação como "Windows" no Web. config do seu projeto do ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="ad6a8-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="ad6a8-138">Nesse modo, o IIS usa credenciais do Windows para autenticar.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="ad6a8-139">Além disso, você deve habilitar a autenticação básica no IIS.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="ad6a8-140">No Gerenciador do IIS, vá para o modo de exibição de recursos, selecione autenticação e habilite a autenticação básica.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="ad6a8-141">Em seu projeto de API Web, adicione o atributo `[Authorize]` para qualquer ação de controlador que precise de autenticação.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="ad6a8-142">Um cliente se autentica sozinho definindo o cabeçalho de autorização na solicitação.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="ad6a8-143">Os clientes de navegador executam essa etapa automaticamente.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="ad6a8-144">Os clientes que não são navegadores precisarão definir o cabeçalho.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="ad6a8-145">Autenticação básica com associação personalizada</span><span class="sxs-lookup"><span data-stu-id="ad6a8-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="ad6a8-146">Conforme mencionado, a autenticação básica interna do IIS usa as credenciais do Windows.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="ad6a8-147">Isso significa que você precisa criar contas para seus usuários no servidor de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="ad6a8-148">Mas, para um aplicativo de Internet, as contas de usuário são normalmente armazenadas em um banco de dados externo.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="ad6a8-149">O código a seguir apresenta como um módulo HTTP que executa a autenticação básica.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="ad6a8-150">Você pode facilmente conectar um provedor de associação do ASP.NET substituindo o método `CheckPassword`, que é um método fictício neste exemplo.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="ad6a8-151">Na API Web 2, você deve considerar escrever um [filtro de autenticação](authentication-filters.md) ou um [middleware OWIN](../../../aspnet/overview/owin-and-katana/index.md), em vez de um módulo http.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="ad6a8-152">Para habilitar o módulo HTTP, adicione o seguinte ao seu arquivo Web. config na seção **System. WebServer** :</span><span class="sxs-lookup"><span data-stu-id="ad6a8-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="ad6a8-153">Substitua "YourAssemblyName" pelo nome do assembly (sem incluir a extensão "dll").</span><span class="sxs-lookup"><span data-stu-id="ad6a8-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="ad6a8-154">Você deve desabilitar outros esquemas de autenticação, como formulários ou autenticação do Windows.</span><span class="sxs-lookup"><span data-stu-id="ad6a8-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
