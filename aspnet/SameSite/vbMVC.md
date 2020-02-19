---
title: Exemplo de cookie SameSite para ASP.NET 4.7.2 VB MVC
author: blowdart
description: Exemplo de cookie SameSite para ASP.NET 4.7.2 VB MVC
ms.author: riande
ms.date: 2/15/2019
uid: samesite/vbMVC
ms.openlocfilehash: f6effce6075f94fb58ce10ec08bf010fab8b4b56
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77458465"
---
# <a name="samesite-cookie-sample-for-aspnet-472-vb-mvc"></a><span data-ttu-id="8c3bc-103">Exemplo de cookie SameSite para ASP.NET 4.7.2 VB MVC</span><span class="sxs-lookup"><span data-stu-id="8c3bc-103">SameSite cookie sample for ASP.NET 4.7.2 VB MVC</span></span>

<span data-ttu-id="8c3bc-104">.NET Framework 4,7 tem suporte interno para o atributo [SameSite](https://www.owasp.org/index.php/SameSite) , mas está de acordo com o padrão original.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-104">.NET Framework 4.7 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it adheres to the original standard.</span></span>
<span data-ttu-id="8c3bc-105">O comportamento de patches alterou o significado de `SameSite.None` para emitir o atributo com um valor de `None`, em vez de não emitir o valor.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-105">The patched behavior changed the meaning of `SameSite.None` to emit the attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="8c3bc-106">Se você quiser não emitir o valor, poderá definir a propriedade `SameSite` em um cookie como-1.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="8c3bc-107">Gravando o atributo SameSite</span><span class="sxs-lookup"><span data-stu-id="8c3bc-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="8c3bc-108">Veja a seguir um exemplo de como escrever um atributo SameSite em um cookie;</span><span class="sxs-lookup"><span data-stu-id="8c3bc-108">Following is an example of how to write a SameSite attribute on a cookie;</span></span>

```vb
' Create the cookie
Dim sameSiteCookie As New HttpCookie("sameSiteSample")

' Set a value for the cookie
sameSiteCookie.Value = "sample"

' Set the secure flag, which Chrome's changes will require for SameSite none.
' Note this will also require you to be running on HTTPS
sameSiteCookie.Secure = True

' Set the cookie to HTTP only which is good practice unless you really do need
' to access it client side in scripts.
sameSiteCookie.HttpOnly = True

' Expire the cookie in 1 minute
sameSiteCookie.Expires = Date.Now.AddMinutes(1)

' Add the SameSite attribute, this will emit the attribute with a value of none.
' To Not emit the attribute at all set the SameSite property to -1.
sameSiteCookie.SameSite = SameSiteMode.None

' Add the cookie to the response cookie collection
Response.Cookies.Add(sameSiteCookie)
```

[!INCLUDE[](~/includes/MTcomments.md)]

<span data-ttu-id="8c3bc-109">O atributo sameSite padrão para o estado de sessão é definido no parâmetro ' cookieSameSite ' das configurações de sessão no `web.config`</span><span class="sxs-lookup"><span data-stu-id="8c3bc-109">The default sameSite attribute for session state is set in the 'cookieSameSite' parameter of the session settings in `web.config`</span></span>

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

## <a name="mvc-authentication"></a><span data-ttu-id="8c3bc-110">Autenticação do MVC</span><span class="sxs-lookup"><span data-stu-id="8c3bc-110">MVC Authentication</span></span>

<span data-ttu-id="8c3bc-111">A autenticação baseada em cookie do OWIN MVC usa um Gerenciador de cookies para habilitar a alteração de atributos de cookie.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-111">OWIN MVC cookie based authentication uses a cookie manager to enable the changing of cookie attributes.</span></span> <span data-ttu-id="8c3bc-112">O [SameSiteCookieManager. vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieManager.vb) é uma implementação de tal classe que você pode copiar em seus próprios projetos.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-112">The [SameSiteCookieManager.vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieManager.vb) is an implementation of such a class which you can copy into your own projects.</span></span> 

<span data-ttu-id="8c3bc-113">Você deve garantir que seus componentes Microsoft. Owin sejam atualizados para a versão 4.1.0 ou superior.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-113">You must ensure your Microsoft.Owin components are all upgraded to version 4.1.0 or greater.</span></span> <span data-ttu-id="8c3bc-114">Verifique seu arquivo de `packages.config` para garantir que todos os números de versão correspondam, por exemplo.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-114">Check your `packages.config` file to ensure all the version numbers match, for example.</span></span>

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

<span data-ttu-id="8c3bc-115">Os componentes de autenticação devem ser configurados para usar o Cookiemanager em sua classe de inicialização;</span><span class="sxs-lookup"><span data-stu-id="8c3bc-115">The authentication components must be configured to use the CookieManager in your startup class;</span></span>

```vb
Public Sub Configuration(app As IAppBuilder)
    app.UseCookieAuthentication(New CookieAuthenticationOptions() With {
        .CookieSameSite = SameSiteMode.None,
        .CookieHttpOnly = True,
        .CookieSecure = CookieSecureOption.Always,
        .CookieManager = New SameSiteCookieManager(New SystemWebCookieManager())
    })
End Sub
```

<span data-ttu-id="8c3bc-116">Um Gerenciador de cookies deve ser definido em *cada* componente que dá suporte a ele, incluindo CookieAuthentication e OpenIdConnectAuthentication.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-116">A cookie manager must be set on *each* component that supports it, this includes CookieAuthentication and OpenIdConnectAuthentication.</span></span>

<span data-ttu-id="8c3bc-117">O SystemWebCookieManager é usado para evitar [problemas conhecidos](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) com a integração de cookie de resposta.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-117">The SystemWebCookieManager is used to avoid [known issues](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) with response cookie integration.</span></span>

### <a name="running-the-sample"></a><span data-ttu-id="8c3bc-118">Executando o exemplo</span><span class="sxs-lookup"><span data-stu-id="8c3bc-118">Running the sample</span></span>

<span data-ttu-id="8c3bc-119">Se você executar o projeto de exemplo, carregue o depurador do navegador na página inicial e use-o para exibir a coleção de cookies para o site.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-119">If you run the sample project please load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span>
<span data-ttu-id="8c3bc-120">Para fazer isso no Edge e no Chrome, pressione `F12` selecione a guia `Application` e clique na URL do site na opção `Cookies` na seção `Storage`.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-120">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![Lista de cookies do depurador do navegador](sample/img/BrowserDebugger.png)

<span data-ttu-id="8c3bc-122">Você pode ver na imagem acima que o cookie criado pelo exemplo ao clicar no botão "Create SameSite cookie" tem um valor de atributo SameSite de `Lax`, correspondendo ao valor definido no código de [exemplo](#sampleCode).</span><span class="sxs-lookup"><span data-stu-id="8c3bc-122">You can see from the image above that the cookie created by the sample when you click the "Create SameSite Cookie" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="8c3bc-123">Interceptando cookies que você não controla</span><span class="sxs-lookup"><span data-stu-id="8c3bc-123">Intercepting cookies you do not control</span></span>

<span data-ttu-id="8c3bc-124">O .NET 4.5.2 introduziu um novo evento para interceptar a gravação de cabeçalhos, `Response.AddOnSendingHeaders`.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-124">.NET 4.5.2 introduced a new event for intercepting the writing of headers, `Response.AddOnSendingHeaders`.</span></span> <span data-ttu-id="8c3bc-125">Isso pode ser usado para interceptar cookies antes que eles sejam retornados para o computador cliente.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-125">This can be used to intercept cookies before they are returned to the client machine.</span></span> <span data-ttu-id="8c3bc-126">No exemplo, conectamos o evento a um método estático que verifica se o navegador dá suporte a novas alterações de sameSite e, caso contrário, altera os cookies para não emitir o atributo se o novo valor de `None` tiver sido definido.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-126">In the sample we wire up the event to a static method which checks whether the browser supports the new sameSite changes, and if not, changes the cookies to not emit the attribute if the new `None` value has been set.</span></span>

<span data-ttu-id="8c3bc-127">Consulte [global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/Global.asax.vb) para obter um exemplo de como conectar o evento e [SameSiteCookieRewriter. vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieRewriter.vb) para obter um exemplo de como tratar o evento e ajustar o cookie `sameSite` atributo que você pode copiar em seu próprio código.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-127">See [global.asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/Global.asax.vb) for an example of hooking up the event and [SameSiteCookieRewriter.vb](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicMVC5/SameSiteCookieRewriter.vb) for an example of handling the event and adjusting the cookie `sameSite` attribute which you can copy into your own code.</span></span>

```vb
Sub FilterSameSiteNoneForIncompatibleUserAgents(ByVal sender As Object)
    Dim application As HttpApplication = TryCast(sender, HttpApplication)

    If application IsNot Nothing Then
        Dim userAgent = application.Context.Request.UserAgent

        If SameSite.DisallowsSameSiteNone(userAgent) Then
            application.Response.AddOnSendingHeaders(
                Function(context)
                    Dim cookies = context.Response.Cookies

                    For i = 0 To cookies.Count - 1
                        Dim cookie = cookies(i)

                        If cookie.SameSite = SameSiteMode.None Then
                            cookie.SameSite = CType((-1), SameSiteMode)
                        End If
                    Next
                End Function)
        End If
    End If
End Sub
```

<span data-ttu-id="8c3bc-128">Você pode alterar o comportamento de Cookie nomeado específico praticamente da mesma forma; o exemplo a seguir ajusta o cookie de autenticação padrão de `Lax` para `None` em navegadores que dão suporte ao valor de `None` ou remove o atributo sameSite em navegadores que não dão suporte a `None`.</span><span class="sxs-lookup"><span data-stu-id="8c3bc-128">You can change specific named cookie behavior in much the same way; the sample below adjust the default authentication cookie from `Lax` to `None` on browsers which support the `None` value, or removes the sameSite attribute on browsers which do not support `None`.</span></span>

```vb
Public Shared Sub AdjustSpecificCookieSettings()
    HttpContext.Current.Response.AddOnSendingHeaders(Function(context)
            Dim cookies = context.Response.Cookies

            For i = 0 To cookies.Count - 1
            Dim cookie = cookies(i)

            If String.Equals(".ASPXAUTH", cookie.Name, StringComparison.Ordinal) Then

                If SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent) Then
                    cookie.SameSite = -1
                Else
                    cookie.SameSite = SameSiteMode.None
                End If

                cookie.Secure = True
            End If
            Next
        End Function)
End Sub
```

## <a name="more-information"></a><span data-ttu-id="8c3bc-129">Mais informações</span><span class="sxs-lookup"><span data-stu-id="8c3bc-129">More Information</span></span>
 
[<span data-ttu-id="8c3bc-130">Atualizações do Chrome</span><span class="sxs-lookup"><span data-stu-id="8c3bc-130">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)

[<span data-ttu-id="8c3bc-131">Documentação do OWIN SameSite</span><span class="sxs-lookup"><span data-stu-id="8c3bc-131">OWIN SameSite Documentation</span></span>](/aspnet/samesite/owin-samesite)

[<span data-ttu-id="8c3bc-132">Documentação do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8c3bc-132">ASP.NET Documentation</span></span>](/aspnet/samesite/system-web-samesite)

[<span data-ttu-id="8c3bc-133">Patches do .NET SameSite</span><span class="sxs-lookup"><span data-stu-id="8c3bc-133">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)