---
title: Exemplo de cookie SameSite para o C# ASP.NET 4.7.2 MVC
author: blowdart
description: Exemplo de cookie SameSite para o C# ASP.NET 4.7.2 MVC
ms.author: riande
ms.date: 2/15/2019
uid: samesite/csMVC
ms.openlocfilehash: dcbd0bee009669fb747d74e6ccef07fbae70a236
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544719"
---
# <a name="samesite-cookie-sample-for-aspnet-472-c-mvc"></a>Exemplo de cookie SameSite para o C# ASP.NET 4.7.2 MVC

.NET Framework 4,7 tem suporte interno para o atributo [SameSite](https://www.owasp.org/index.php/SameSite) , mas está de acordo com o padrão original.
O comportamento de patches alterou o significado de `SameSite.None` para emitir o atributo com um valor de `None`, em vez de não emitir o valor. Se você quiser não emitir o valor, poderá definir a propriedade `SameSite` em um cookie como-1.

## <a name="sampleCode"></a>Gravando o atributo SameSite

Veja a seguir um exemplo de como escrever um atributo SameSite em um cookie;

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

O atributo sameSite padrão para o estado de sessão é definido no parâmetro ' cookieSameSite ' das configurações de sessão no `web.config`

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

## <a name="mvc-authentication"></a>Autenticação do MVC

A autenticação baseada em cookie do OWIN MVC usa um Gerenciador de cookies para habilitar a alteração de atributos de cookie. O [SameSiteCookieManager.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieManager.cs) é uma implementação de tal classe que você pode copiar em seus próprios projetos. 

Você deve garantir que seus componentes Microsoft. Owin sejam atualizados para a versão 4.1.0 ou superior. Verifique seu arquivo de `packages.config` para garantir que todos os números de versão correspondam, por exemplo.

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

Os componentes de autenticação devem ser configurados para usar o Cookiemanager em sua classe de inicialização;

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

Um Gerenciador de cookies deve ser definido em *cada* componente que dá suporte a ele, incluindo CookieAuthentication e OpenIdConnectAuthentication.

O SystemWebCookieManager é usado para evitar [problemas conhecidos](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues) com a integração de cookie de resposta.

### <a name="running-the-sample"></a>Executando o exemplo

Se você executar o projeto de exemplo, carregue o depurador do navegador na página inicial e use-o para exibir a coleção de cookies para o site.
Para fazer isso no Edge e no Chrome, pressione `F12` selecione a guia `Application` e clique na URL do site na opção `Cookies` na seção `Storage`.

![Lista de cookies do depurador do navegador](sample/img/BrowserDebugger.png)

Você pode ver na imagem acima que o cookie criado pelo exemplo ao clicar no botão "criar cookies" tem um valor de atributo SameSite de `Lax`, correspondendo ao valor definido no código de [exemplo](#sampleCode).

## <a name="interception"></a>Interceptando cookies que você não controla

O .NET 4.5.2 introduziu um novo evento para interceptar a gravação de cabeçalhos, `Response.AddOnSendingHeaders`. Isso pode ser usado para interceptar cookies antes que eles sejam retornados para o computador cliente. No exemplo, conectamos o evento a um método estático que verifica se o navegador dá suporte a novas alterações de sameSite e, caso contrário, altera os cookies para não emitir o atributo se o novo valor de `None` tiver sido definido.

Consulte [global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/Global.asax.cs) para obter um exemplo de como conectar o evento e [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472CSharpMVC5/SameSiteCookieRewriter.cs) para obter um exemplo de como manipular o evento e ajustar o cookie `sameSite` atributo que você pode copiar em seu próprio código.

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

Você pode alterar o comportamento de Cookie nomeado específico praticamente da mesma forma; o exemplo a seguir ajusta o cookie de autenticação padrão de `Lax` para `None` em navegadores que dão suporte ao valor de `None` ou remove o atributo sameSite em navegadores que não dão suporte a `None`.

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

### <a name="more-information"></a>Mais informações
 
[Atualizações do Chrome](https://www.chromium.org/updates/same-site)

[Documentação do OWIN SameSite](/aspnet/samesite/owin-samesite)

[Documentação do ASP.NET](/aspnet/samesite/system-web-samesite)

[Patches do .NET SameSite](/aspnet/samesite/kbs-samesite)