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
# <a name="samesite-cookie-sample-for-aspnet-472-vb-webforms"></a>Exemplo de cookie SameSite para WebForms do ASP.NET 4.7.2 VB
.NET Framework 4,7 tem suporte interno para o atributo [SameSite](https://www.owasp.org/index.php/SameSite) , mas está de acordo com o padrão original.
O comportamento de patches alterou o significado de `SameSite.None` para emitir o atributo com um valor de `None`, em vez de não emitir o valor. Se você quiser não emitir o valor, poderá definir a propriedade `SameSite` em um cookie como-1.

## <a name="sampleCode"></a>Gravando o atributo SameSite

Veja a seguir um exemplo de como escrever um atributo SameSite em um cookie;

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

O atributo sameSite padrão para um cookie de autenticação de formulários é definido no parâmetro `cookieSameSite` das configurações de autenticação de formulários no `web.config` 

```xml
<system.web>
  <authentication mode="Forms">
    <forms name=".ASPXAUTH" loginUrl="~/" cookieSameSite="None" requireSSL="true">
    </forms>
  </authentication>
</system.web>
```

O atributo sameSite padrão para o estado de sessão também é definido no parâmetro ' cookieSameSite ' das configurações de sessão no `web.config`

```xml
<system.web>
  <sessionState cookieSameSite="None">     
  </sessionState>
</system.web>
```

A atualização de novembro de 2019 para .NET alterou as configurações padrão de autenticação de formulários e sessão para `lax` como é a configuração mais compatível. no entanto, se você inserir páginas em IFrames, talvez seja necessário reverter essa configuração para nenhuma e, em seguida, adicionar o código de [intercepção](#interception) mostrado abaixo para ajustar o comportamento de `none` dependendo da funcionalidade do navegador.

### <a name="running-the-sample"></a>Executando o exemplo

Se você executar o projeto de exemplo, carregue o depurador do navegador na página inicial e use-o para exibir a coleção de cookies para o site.
Para fazer isso no Edge e no Chrome, pressione `F12` selecione a guia `Application` e clique na URL do site na opção `Cookies` na seção `Storage`.

![Lista de cookies do depurador do navegador](sample/img/BrowserDebugger.png)

Você pode ver na imagem acima que o cookie criado pelo exemplo ao clicar no botão "criar cookies" tem um valor de atributo SameSite de `Lax`, correspondendo ao valor definido no código de [exemplo](#sampleCode).

## <a name="interception"></a>Interceptando cookies que você não controla

O .NET 4.5.2 introduziu um novo evento para interceptar a gravação de cabeçalhos, `Response.AddOnSendingHeaders`. Isso pode ser usado para interceptar cookies antes que eles sejam retornados para o computador cliente. No exemplo, conectamos o evento a um método estático que verifica se o navegador dá suporte a novas alterações de sameSite e, caso contrário, altera os cookies para não emitir o atributo se o novo valor de `None` tiver sido definido.

Consulte [global. asax](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/Global.asax.vb) para obter um exemplo de como conectar o evento e [SameSiteCookieRewriter.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNet472VisualBasicWebForms/SameSiteCookieRewriter.vb) para obter um exemplo de como manipular o evento e ajustar o atributo `sameSite` cookie.


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

Você pode alterar o comportamento de Cookie nomeado específico praticamente da mesma forma; o exemplo a seguir ajusta o cookie de autenticação padrão de `Lax` para `None` em navegadores que dão suporte ao valor de `None` ou remove o atributo sameSite em navegadores que não dão suporte a `None`.

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

## <a name="more-information"></a>Mais informações

[Atualizações do Chrome](https://www.chromium.org/updates/same-site)

[Documentação do ASP.NET](/aspnet/samesite/system-web-samesite)

[Patches do .NET SameSite](/aspnet/samesite/kbs-samesite)