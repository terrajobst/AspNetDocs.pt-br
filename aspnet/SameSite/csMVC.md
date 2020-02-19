---
title: Exemplo de cookie SameSite para o C# ASP.NET 4.7.2 MVC
author: blowdart
description: Exemplo de cookie SameSite para o C# ASP.NET 4.7.2 MVC
ms.author: riande
ms.date: 2/15/2019
uid: samesite/csMVC
ms.openlocfilehash: dcbd0bee009669fb747d74e6ccef07fbae70a236
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458451"
---
# <a name="samesite-cookie-sample-for-aspnet-472-c-mvc"></a><span data-ttu-id="49922-103">Exemplo de cookie SameSite para o C# ASP.NET 4.7.2 MVC</span><span class="sxs-lookup"><span data-stu-id="49922-103">SameSite cookie sample for ASP.NET 4.7.2 C# MVC</span></span>

<span data-ttu-id="49922-104">.NET Framework 4,7 tem suporte interno para o atributo [SameSite](https://www.owasp.org/index.php/SameSite) , mas está de acordo com o padrão original.</span><span class="sxs-lookup"><span data-stu-id="49922-104">.NET Framework 4.7 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it adheres to the original standard.</span></span>
<span data-ttu-id="49922-105">O comportamento de patches alterou o significado de `SameSite.None` para emitir o atributo com um valor de `None`, em vez de não emitir o valor.</span><span class="sxs-lookup"><span data-stu-id="49922-105">The patched behavior changed the meaning of `SameSite.None` to emit the attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="49922-106">Se você quiser não emitir o valor, poderá definir a propriedade `SameSite` em um cookie como-1.</span><span class="sxs-lookup"><span data-stu-id="49922-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="49922-107">Gravando o atributo SameSite</span><span class="sxs-lookup"><span data-stu-id="49922-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="49922-108">Veja a seguir um exemplo de como escrever um atributo SameSite em um cookie;</span><span class="sxs-lookup"><span data-stu-id="49922-108">Following is an example of how to write a SameSite attribute on a cookie;</span></span>

```c#
// Create the cookie
HttpCookie sameSiteCookie = new HttpCookie("SameSiteSample");

// Set a value for the cookieSite none.
// Note this will also require you to be running on HTTPS
sameSiteCookie.Value = "sample";

// Set the secure flag, which Chrome's changes will require for Same
sameSiteCookie.Secure = true;

// Set the cookie to HTTP only which is good practice unless you really do need
// to access it client side in scripts.
sameSiteCookie.HttpOnly = true;

// Add the SameSite attribute, this will emit the attribute with a value of none.
// To not emit the attribute at all set the SameSite property to -1.
sameSiteCookie.SameSite = SameSiteMode.None;

// Add the cookie to the response cookie collection
Response.Cookies.Add(sameSiteCookie);
```

[!INCLUDE[](~/includes/MTcomments.md)]

<span data-ttu-id="49922-109">O atributo sameSite padrão para o estado de sessão é definido no parâmetro ' cookieSameSite ' das configurações de sessão no `web.config`</span><span class="sxs-lookup"><span data-stu-id="49922-109">The default sameSite attribute for session state is set in the 'cookieSameSite' parameter of the session settings in `web.config`</span></span>

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

## <a name="mvc-authentication"></a><span data-ttu-id="49922-110">Autenticação do MVC</span><span class="sxs-lookup"><span data-stu-id="49922-110">MVC Authentication</span></span>

<span data-ttu-id="49922-111">A autenticação baseada em cookie do OWIN MVC usa um Gerenciador de cookies para habilitar a alteração de atributos de cookie.</span><span class="sxs-lookup"><span data-stu-id="49922-111">OWIN MVC cookie based authentication uses a cookie manager to enable the changing of cookie attributes.</span></span> <span data-ttu-id="49922-112">O [SameSiteCookieManager.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieManager.cs) é uma implementação de tal classe que você pode copiar em seus próprios projetos.</span><span class="sxs-lookup"><span data-stu-id="49922-112">The [SameSiteCookieManager.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieManager.cs) is an implementation of such a class which you can copy into your own projects.</span></span> 

<span data-ttu-id="49922-113">Você deve garantir que seus componentes Microsoft. Owin sejam atualizados para a versão 4.1.0 ou superior.</span><span class="sxs-lookup"><span data-stu-id="49922-113">You must ensure your Microsoft.Owin components are all upgraded to version 4.1.0 or greater.</span></span> <span data-ttu-id="49922-114">Verifique seu arquivo de `packages.config` para garantir que todos os números de versão correspondam, por exemplo.</span><span class="sxs-lookup"><span data-stu-id="49922-114">Check your `packages.config` file to ensure all the version numbers match, for example.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <!-- other packages -->
  <package id="Microsoft.Owin.Host.SystemWeb" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Owin.Security" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Owin.Security.Cookies" version="4.1.0" targetFramework="net472" />
  <package id="Microsoft.Web.Infrastructure" version="1.0.0.0" targetFramework="net472" />
  <package id="Owin" version="1.0" targetFramework="net472" />
</packages>
```

<span data-ttu-id="49922-115">Os componentes de autenticação devem ser configurados para usar o Cookiemanager em sua classe de inicialização;</span><span class="sxs-lookup"><span data-stu-id="49922-115">The authentication components must then be configured to use the CookieManager in your startup class;</span></span>

```c#
public void Configuration(IAppBuilder app)
{
    app.UseCookieAuthentication(new CookieAuthenticationOptions
    {
        CookieSameSite = SameSiteMode.None,
        CookieHttpOnly = true,
        CookieSecure = CookieSecureOption.Always,
        CookieManager = new SameSiteCookieManager(new SystemWebCookieManager())
    });
}
```

<span data-ttu-id="49922-116">Um Gerenciador de cookies deve ser definido em *cada* componente que dá suporte a ele, incluindo CookieAuthentication e OpenIdConnectAuthentication.</span><span class="sxs-lookup"><span data-stu-id="49922-116">A cookie manager must be set on *each* component that supports it, this includes CookieAuthentication and OpenIdConnectAuthentication.</span></span>

<span data-ttu-id="49922-117">O SystemWebCookieManager é usado para evitar [problemas conhecidos](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) com a integração de cookie de resposta.</span><span class="sxs-lookup"><span data-stu-id="49922-117">The SystemWebCookieManager is used to avoid [known issues](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) with response cookie integration.</span></span>

### <a name="running-the-sample"></a><span data-ttu-id="49922-118">Executando o exemplo</span><span class="sxs-lookup"><span data-stu-id="49922-118">Running the sample</span></span>

<span data-ttu-id="49922-119">Se você executar o projeto de exemplo, carregue o depurador do navegador na página inicial e use-o para exibir a coleção de cookies para o site.</span><span class="sxs-lookup"><span data-stu-id="49922-119">If you run the sample project  load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span>
<span data-ttu-id="49922-120">Para fazer isso no Edge e no Chrome, pressione `F12` selecione a guia `Application` e clique na URL do site na opção `Cookies` na seção `Storage`.</span><span class="sxs-lookup"><span data-stu-id="49922-120">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![Lista de cookies do depurador do navegador](sample/img/BrowserDebugger.png)

<span data-ttu-id="49922-122">Você pode ver na imagem acima que o cookie criado pelo exemplo ao clicar no botão "criar cookies" tem um valor de atributo SameSite de `Lax`, correspondendo ao valor definido no código de [exemplo](#sampleCode).</span><span class="sxs-lookup"><span data-stu-id="49922-122">You can see from the image above that the cookie created by the sample when you click the "Create Cookies" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="49922-123">Interceptando cookies que você não controla</span><span class="sxs-lookup"><span data-stu-id="49922-123">Intercepting cookies you do not control</span></span>

<span data-ttu-id="49922-124">O .NET 4.5.2 introduziu um novo evento para interceptar a gravação de cabeçalhos, `Response.AddOnSendingHeaders`.</span><span class="sxs-lookup"><span data-stu-id="49922-124">.NET 4.5.2 introduced a new event for intercepting the writing of headers, `Response.AddOnSendingHeaders`.</span></span> <span data-ttu-id="49922-125">Isso pode ser usado para interceptar cookies antes que eles sejam retornados para o computador cliente.</span><span class="sxs-lookup"><span data-stu-id="49922-125">This can be used to intercept cookies before they are returned to the client machine.</span></span> <span data-ttu-id="49922-126">No exemplo, conectamos o evento a um método estático que verifica se o navegador dá suporte a novas alterações de sameSite e, caso contrário, altera os cookies para não emitir o atributo se o novo valor de `None` tiver sido definido.</span><span class="sxs-lookup"><span data-stu-id="49922-126">In the sample we wire up the event to a static method which checks whether the browser supports the new sameSite changes, and if not, changes the cookies to not emit the attribute if the new `None` value has been set.</span></span>

<span data-ttu-id="49922-127">Consulte [global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/Global.asax.cs) para obter um exemplo de como conectar o evento e [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieRewriter.cs) para obter um exemplo de como manipular o evento e ajustar o cookie `sameSite` atributo que você pode copiar em seu próprio código.</span><span class="sxs-lookup"><span data-stu-id="49922-127">See [global.asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/Global.asax.cs) for an example of hooking up the event and [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieRewriter.cs) for an example of handling the event and adjusting the cookie `sameSite` attribute which you can copy into your own code.</span></span>

```c#
public static void FilterSameSiteNoneForIncompatibleUserAgents(object sender)
{
    HttpApplication application = sender as HttpApplication;
    if (application != null)
    {
        var userAgent = application.Context.Request.UserAgent;
        if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
        {
            HttpContext.Current.Response.AddOnSendingHeaders(context =>
            {
                var cookies = context.Response.Cookies;
                for (var i = 0; i < cookies.Count; i++)
                {
                    var cookie = cookies[i];
                    if (cookie.SameSite == SameSiteMode.None)
                    {
                        cookie.SameSite = (SameSiteMode)(-1); // Unspecified
                    }
                }
            });
        }
    }
}
```

<span data-ttu-id="49922-128">Você pode alterar o comportamento de Cookie nomeado específico praticamente da mesma forma; o exemplo a seguir ajusta o cookie de autenticação padrão de `Lax` para `None` em navegadores que dão suporte ao valor de `None` ou remove o atributo sameSite em navegadores que não dão suporte a `None`.</span><span class="sxs-lookup"><span data-stu-id="49922-128">You can change specific named cookie behavior in much the same way; the sample below adjust the default authentication cookie from `Lax` to `None` on browsers which support the `None` value, or removes the sameSite attribute on browsers which do not support `None`.</span></span>

```c#
public static void AdjustSpecificCookieSettings()
{
    HttpContext.Current.Response.AddOnSendingHeaders(context =>
    {
        var cookies = context.Response.Cookies;
        for (var i = 0; i < cookies.Count; i++)
        {
            var cookie = cookies[i]; 
            // Forms auth: ".ASPXAUTH"
            // Session: "ASP.NET_SessionId"
            if (string.Equals(".ASPXAUTH", cookie.Name, StringComparison.Ordinal))
            { 
                if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
                {
                    cookie.SameSite = -1;
                }
                else
                {
                    cookie.SameSite = SameSiteMode.None;
                }
                cookie.Secure = true;
            }
        }
    });
}
```

### <a name="more-information"></a><span data-ttu-id="49922-129">Mais informações</span><span class="sxs-lookup"><span data-stu-id="49922-129">More Information</span></span>
 
[<span data-ttu-id="49922-130">Atualizações do Chrome</span><span class="sxs-lookup"><span data-stu-id="49922-130">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)

[<span data-ttu-id="49922-131">Documentação do OWIN SameSite</span><span class="sxs-lookup"><span data-stu-id="49922-131">OWIN SameSite Documentation</span></span>](/aspnet/samesite/owin-samesite)

[<span data-ttu-id="49922-132">Documentação do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="49922-132">ASP.NET Documentation</span></span>](/aspnet/samesite/system-web-samesite)

[<span data-ttu-id="49922-133">Patches do .NET SameSite</span><span class="sxs-lookup"><span data-stu-id="49922-133">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)