---
title: Exemplo de cookie SameSite para WebForms do ASP.NET 4.7.2 VB
author: blowdart
description: Exemplo de cookie SameSite para WebForms do ASP.NET 4.7.2 VB
ms.author: riande
ms.date: 2/15/2019
uid: samesite/vbWF
ms.openlocfilehash: 8979edecc5acf7dac81b9f53d31af00389f4727c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544992"
---
# <a name="samesite-cookie-sample-for-aspnet-472-vb-webforms"></a><span data-ttu-id="caf18-103">Exemplo de cookie SameSite para WebForms do ASP.NET 4.7.2 VB</span><span class="sxs-lookup"><span data-stu-id="caf18-103">SameSite cookie sample for ASP.NET 4.7.2 VB WebForms</span></span>
<span data-ttu-id="caf18-104">.NET Framework 4,7 tem suporte interno para o atributo [SameSite](https://www.owasp.org/index.php/SameSite) , mas está de acordo com o padrão original.</span><span class="sxs-lookup"><span data-stu-id="caf18-104">.NET Framework 4.7 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it adheres to the original standard.</span></span>
<span data-ttu-id="caf18-105">O comportamento de patches alterou o significado de `SameSite.None` para emitir o atributo com um valor de `None`, em vez de não emitir o valor.</span><span class="sxs-lookup"><span data-stu-id="caf18-105">The patched behavior changed the meaning of `SameSite.None` to emit the attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="caf18-106">Se você quiser não emitir o valor, poderá definir a propriedade `SameSite` em um cookie como-1.</span><span class="sxs-lookup"><span data-stu-id="caf18-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="caf18-107">Gravando o atributo SameSite</span><span class="sxs-lookup"><span data-stu-id="caf18-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="caf18-108">Veja a seguir um exemplo de como escrever um atributo SameSite em um cookie;</span><span class="sxs-lookup"><span data-stu-id="caf18-108">Following is an example of how to write a SameSite attribute on a cookie;</span></span>

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

<span data-ttu-id="caf18-109">O atributo sameSite padrão para um cookie de autenticação de formulários é definido no parâmetro `cookieSameSite` das configurações de autenticação de formulários no `web.config`</span><span class="sxs-lookup"><span data-stu-id="caf18-109">The default sameSite attribute for a forms authentication cookie is set in the `cookieSameSite` parameter of the forms authentication settings in `web.config`</span></span> 

```xml
<system.web>
  <authentication mode="Forms">
    <forms name=".ASPXAUTH" loginUrl="~/" cookieSameSite="None" requireSSL="true">
    </forms>
  </authentication>
</system.web>
```

<span data-ttu-id="caf18-110">O atributo sameSite padrão para o estado de sessão também é definido no parâmetro ' cookieSameSite ' das configurações de sessão no `web.config`</span><span class="sxs-lookup"><span data-stu-id="caf18-110">The default sameSite attribute for session state is also set in the 'cookieSameSite' parameter of the session settings in `web.config`</span></span>

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

<span data-ttu-id="caf18-111">A atualização de novembro de 2019 para .NET alterou as configurações padrão de autenticação de formulários e sessão para `lax` como é a configuração mais compatível. no entanto, se você inserir páginas em IFrames, talvez seja necessário reverter essa configuração para nenhuma e, em seguida, adicionar o código de [intercepção](#interception) mostrado abaixo para ajustar o comportamento de `none` dependendo da funcionalidade do navegador.</span><span class="sxs-lookup"><span data-stu-id="caf18-111">The November 2019 update to .NET changed the default settings for Forms Authentication and Session to `lax` as is the most compatible setting, however if you embed pages into iframes you may need to revert this setting to None, and then add the [interception](#interception) code shown below to adjust the `none` behavior depending on browser capability.</span></span>

### <a name="running-the-sample"></a><span data-ttu-id="caf18-112">Executando o exemplo</span><span class="sxs-lookup"><span data-stu-id="caf18-112">Running the sample</span></span>

<span data-ttu-id="caf18-113">Se você executar o projeto de exemplo, carregue o depurador do navegador na página inicial e use-o para exibir a coleção de cookies para o site.</span><span class="sxs-lookup"><span data-stu-id="caf18-113">If you run the sample project  load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span>
<span data-ttu-id="caf18-114">Para fazer isso no Edge e no Chrome, pressione `F12` selecione a guia `Application` e clique na URL do site na opção `Cookies` na seção `Storage`.</span><span class="sxs-lookup"><span data-stu-id="caf18-114">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![Lista de cookies do depurador do navegador](sample/img/BrowserDebugger.png)

<span data-ttu-id="caf18-116">Você pode ver na imagem acima que o cookie criado pelo exemplo ao clicar no botão "criar cookies" tem um valor de atributo SameSite de `Lax`, correspondendo ao valor definido no código de [exemplo](#sampleCode).</span><span class="sxs-lookup"><span data-stu-id="caf18-116">You can see from the image above that the cookie created by the sample when you click the "Create Cookies" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="caf18-117">Interceptando cookies que você não controla</span><span class="sxs-lookup"><span data-stu-id="caf18-117">Intercepting cookies you do not control</span></span>

<span data-ttu-id="caf18-118">O .NET 4.5.2 introduziu um novo evento para interceptar a gravação de cabeçalhos, `Response.AddOnSendingHeaders`.</span><span class="sxs-lookup"><span data-stu-id="caf18-118">.NET 4.5.2 introduced a new event for intercepting the writing of headers, `Response.AddOnSendingHeaders`.</span></span> <span data-ttu-id="caf18-119">Isso pode ser usado para interceptar cookies antes que eles sejam retornados para o computador cliente.</span><span class="sxs-lookup"><span data-stu-id="caf18-119">This can be used to intercept cookies before they are returned to the client machine.</span></span> <span data-ttu-id="caf18-120">No exemplo, conectamos o evento a um método estático que verifica se o navegador dá suporte a novas alterações de sameSite e, caso contrário, altera os cookies para não emitir o atributo se o novo valor de `None` tiver sido definido.</span><span class="sxs-lookup"><span data-stu-id="caf18-120">In the sample we wire up the event to a static method which checks whether the browser supports the new sameSite changes, and if not, changes the cookies to not emit the attribute if the new `None` value has been set.</span></span>

<span data-ttu-id="caf18-121">Consulte [global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/Global.asax.vb) para obter um exemplo de como conectar o evento e [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/SameSiteCookieRewriter.vb) para obter um exemplo de como manipular o evento e ajustar o atributo `sameSite` cookie.</span><span class="sxs-lookup"><span data-stu-id="caf18-121">See [global.asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/Global.asax.vb) for an example of hooking up the event and [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/SameSiteCookieRewriter.vb) for an example of handling the event and adjusting the cookie `sameSite` attribute.</span></span>


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

<span data-ttu-id="caf18-122">Você pode alterar o comportamento de Cookie nomeado específico praticamente da mesma forma; o exemplo a seguir ajusta o cookie de autenticação padrão de `Lax` para `None` em navegadores que dão suporte ao valor de `None` ou remove o atributo sameSite em navegadores que não dão suporte a `None`.</span><span class="sxs-lookup"><span data-stu-id="caf18-122">You can change specific named cookie behavior in much the same way; the sample below adjust the default authentication cookie from `Lax` to `None` on browsers which support the `None` value, or removes the sameSite attribute on browsers which do not support `None`.</span></span>

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

## <a name="more-information"></a><span data-ttu-id="caf18-123">Mais informações</span><span class="sxs-lookup"><span data-stu-id="caf18-123">More Information</span></span>

[<span data-ttu-id="caf18-124">Atualizações do Chrome</span><span class="sxs-lookup"><span data-stu-id="caf18-124">Chrome Updates</span></span>](https://www.chromium.org/updates/same-site)

[<span data-ttu-id="caf18-125">Documentação do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="caf18-125">ASP.NET Documentation</span></span>](/aspnet/samesite/system-web-samesite)

[<span data-ttu-id="caf18-126">Patches do .NET SameSite</span><span class="sxs-lookup"><span data-stu-id="caf18-126">.NET SameSite Patches</span></span>](/aspnet/samesite/kbs-samesite)