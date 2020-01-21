---
title: Trabalhar com cookies SameSite e com a interface Web aberta para .NET (OWIN)
author: rick-anderson
description: Trabalhar com cookies SameSite e com a interface Web aberta para .NET (OWIN)
ms.author: riande
ms.date: 12/6/2019
uid: owin-samesite
ms.openlocfilehash: ac5ae24eeb9e8e1cc6296667a4bebef72c3eb62c
ms.sourcegitcommit: 7b1e1784213dd4c301635f9e181764f3e2f94162
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74993084"
---
# <a name="samesite-cookies-and-the-open-web-interface-for-net-owin"></a>SameSite cookies e a Open Web interface for .NET (OWIN)

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

`SameSite` é um rascunho de [IETF](https://ietf.org/about/) projetado para fornecer alguma proteção contra ataques CSRF (solicitação entre sites forjada). O [rascunho do SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):

* Trata cookies como `SameSite=Lax` por padrão.
* Os cookies de Estados que declaram explicitamente `SameSite=None` para habilitar a entrega entre sites devem ser marcados como `Secure`.

`Lax` funciona para a maioria dos cookies de aplicativo. Algumas formas de autenticação, como o [OpenID Connect](https://openid.net/connect/) (OIDC) e o [WS-Federation,](https://auth0.com/docs/protocols/ws-fed) assumem o padrão de redirecionamentos baseados em post. Os redirecionamentos baseados em POST disparam as proteções `SameSite` navegador, de modo que `SameSite` está desabilitado para esses componentes. A maioria dos logons [OAuth](https://oauth.net/) não é afetada devido a diferenças na forma como os fluxos de solicitação. Todos os outros componentes **não** definem `SameSite` por padrão e usam o comportamento padrão dos clientes (antigo ou novo).

O parâmetro `None` causa problemas de compatibilidade com clientes que implementaram o padrão anterior de [rascunho 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) (por exemplo, IOS 12). Consulte [suporte a navegadores mais antigos](#sob) neste documento.

Cada componente OWIN que emite cookies precisa decidir se `SameSite` é apropriado.

Para a versão do ASP.NET 4. x deste artigo, consulte <xref:samesite/system-web-samesite>.

## <a name="api-usage-with-samesite"></a>Uso da API com SameSite

`Microsoft.Owin` tem sua própria implementação de `SameSite`:

* Isso não é diretamente dependente do que no `System.Web`.
* `SameSite` funciona em todas as versões que são preparadas pelos pacotes de `Microsoft.Owin`, .NET 4,5 e posterior.
* Somente o componente [SystemWebCookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebCookieManager.cs) interage diretamente com a classe `System.Web` `HttpCookie`.

`SystemWebCookieManager` depende das APIs do .NET 4.7.2 `System.Web` para habilitar o suporte a `SameSite` e os patches para alterar o comportamento.

Os motivos para usar `SystemWebCookieManager` são descritos em problemas de [integração de cookies de resposta de OWIN e System. Web](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues). `SystemWebCookieManager` é recomendado ao executar em `System.Web`.

O código a seguir define `SameSite` para `Lax`:

```csharp
owinContext.Response.Cookies.Append("My Key", "My Value", new CookieOptions()
{
    SameSite = SameSiteMode.Lax
});
```

As APIs a seguir usam `SameSite`:

* [Microsoft. Owin. SameSiteMode](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin/SameSiteMode.cs)
* [Cookieoptions. SameSite](xref:Microsoft.AspNetCore.Http.CookieOptions.SameSite)
* [Classe CookieAuthenticationOptions](/previous-versions/aspnet/dn385599(v%3Dvs.113)) <!-- CookieAuthenticationOptions.CookieSameSite not published -->
* [CookieAuthenticationOptions. CookieSameSite](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.Cookies/CookieAuthenticationOptions.cs#L68-#L72)
* [ICookieManager](/previous-versions/aspnet/dn800238(v%3Dvs.113))
* [SystemWebCookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebCookieManager.cs)
* [SystemWebChunkingCookieManager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Host.SystemWeb/SystemWebChunkingCookieManager.cs)
* [CookieAuthenticationOptions. Cookiemanager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.Cookies/CookieAuthenticationOptions.cs#L143-#AL148)
* [OpenIdConnectAuthenticationOptions. Cookiemanager](https://github.com/aspnet/AspNetKatana/blob/dev/src/Microsoft.Owin.Security.OpenIdConnect/OpenIdConnectAuthenticationOptions.cs#L315-#L318)

## <a name="history-and-changes"></a>Histórico e alterações

[Microsoft. Owin](https://www.nuget.org/packages/Microsoft.Owin/) nunca suportava o [padrão de rascunho`SameSite` 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

O suporte para o [rascunho do SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) só está disponível no `Microsoft.Owin` 4.1.0 e posterior. Não há patches para versões anteriores.

O rascunho 2019 da especificação de `SameSite`:

* **Não** é compatível com versões anteriores com o rascunho 2016. Para obter mais informações, consulte [dando suporte a navegadores mais antigos](#sob) neste documento.
* Especifica que os cookies são tratados como `SameSite=Lax` por padrão.
* Especifica cookies que declaram explicitamente `SameSite=None` para habilitar a entrega entre sites devem ser marcados como `Secure`. `None` é uma nova entrada a ser recusada.
* Está agendado para ser habilitado pelo [Chrome](https://chromestatus.com/feature/5088147346030592) por padrão em [fevereiro de 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html). Os navegadores começaram a passar para esse padrão em 2019.
* O é suportado por patches emitidos conforme descrito em artigos da base de conhecimento. Para obter mais informações, consulte <xref:samesite/kbs-samesite>.

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>Suporte a navegadores mais antigos

O padrão de 2016 `SameSite` obrigatório que valores desconhecidos devem ser tratados como valores de `SameSite=Strict`. Os aplicativos acessados de navegadores mais antigos que dão suporte ao 2016 `SameSite` Standard podem falhar quando obtêm uma propriedade `SameSite` com um valor de `None`. Os aplicativos Web devem implementar a detecção do navegador se pretenderem oferecer suporte a navegadores mais antigos. O ASP.NET não implementa a detecção de navegador, pois os valores dos agentes de usuário são altamente voláteis e mudam com frequência. Um ponto de extensão no [ICookieManager](/previous-versions/aspnet/dn800238(v%3Dvs.113)) permite a conexão da lógica específica do agente do usuário.
<!-- https://docs.microsoft.com/en-us/previous-versions/aspnet/dn800238(v%3Dvs.113) -->

Em `Startup.Configuration`, adicione um código semelhante ao seguinte:

[!code-csharp[](sample/Startup1.cs?name=snippet)]

O código anterior requer o patch do .NET 4.7.2 ou posterior `SameSite`.

O código a seguir mostra um exemplo de implementação de `SameSiteCookieManager`:

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet)]

No exemplo anterior, `DisallowsSameSiteNone` é chamado no método `CheckSameSite`. `DisallowsSameSiteNone` é um método de usuário que detecta se o agente do usuário não dá suporte a `SameSite` `None`:

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet3&highlight=4)]

O código a seguir mostra um exemplo de método de `DisallowsSameSiteNone`:

> [!WARNING]
> O código a seguir é apenas para demonstração:
> * Ele não deve ser considerado concluído.
> * Ele não é mantido ou tem suporte.

[!code-csharp[](sample/SameSiteCookieManager.cs?name=snippet2)]

## <a name="test-apps-for-samesite-problems"></a>Testar aplicativos para problemas de SameSite

Aplicativos que interagem com sites remotos, como por meio de logon de terceiros, precisam:

* Teste a interação em vários navegadores.
* Aplique a [detecção e mitigação do navegador](#sob) discutidas neste documento.

Teste os aplicativos Web usando uma versão do cliente que pode aceitar o novo comportamento de `SameSite`. O Chrome, o Firefox e o Chromium Edge têm novos sinalizadores de recurso de aceitação que podem ser usados para teste. Depois que seu aplicativo aplicar os patches `SameSite`, teste-os com versões mais antigas do cliente, especialmente no Safari. Para obter mais informações, consulte [dando suporte a navegadores mais antigos](#sob) neste documento.

### <a name="test-with-chrome"></a>Teste com o Chrome

O Chrome 78 + fornece resultados enganosos porque tem uma mitigação temporária em vigor. A mitigação do Chrome 78 + temporária permite cookies com menos de dois minutos. O Chrome 76 ou 77 com os sinalizadores de teste apropriados habilitados fornece resultados mais precisos. Para testar o novo comportamento de `SameSite`, alterne `chrome://flags/#same-site-by-default-cookies` para **habilitado**. Versões mais antigas do Chrome (75 e inferior) são relatadas para falha com a nova configuração de `None`. Consulte [suporte a navegadores mais antigos](#sob) neste documento.

O Google não disponibiliza versões mais antigas do Chrome. Siga as instruções em [baixar o Chromium](https://www.chromium.org/getting-involved/download-chromium) para testar versões anteriores do Chrome. **Não** Baixe o Chrome de links fornecidos pela pesquisa de versões mais antigas do Chrome.

* [Chromium 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chromium 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a>Teste com o Safari

O Safari 12 implementou estritamente o rascunho anterior e falha quando o novo valor de `None` está em um cookie. `None` é evitada por meio do código de detecção de navegador que [dá suporte a navegadores mais antigos](#sob) neste documento. Teste OS logons de estilo do sistema operacional baseado no Safari 12, Safari 13 e WebKit usando MSAL, ADAL ou qualquer biblioteca que você esteja usando. O problema depende da versão subjacente do sistema operacional. OSX Mojave (10,14) e iOS 12 são conhecidos por ter problemas de compatibilidade com o novo comportamento de `SameSite`. Atualizar o sistema operacional para OSX Catalina (10,15) ou iOS 13 corrige o problema. No momento, o Safari não tem um sinalizador de aceitação para testar o novo comportamento de especificação.

### <a name="test-with-firefox"></a>Testar com o Firefox

O suporte do Firefox para o novo padrão pode ser testado na versão 68 +, optando na página de `about:config` com o sinalizador de recurso `network.cookie.sameSite.laxByDefault`. Não há relatórios de problemas de compatibilidade com versões mais antigas do Firefox.

### <a name="test-with-edge-browser"></a>Testar com o navegador Microsoft Edge

O Microsoft Edge dá suporte ao antigo `SameSite` padrão. A versão 44 do Microsoft Edge não tem nenhum problema de compatibilidade conhecido com o novo padrão.

### <a name="test-with-edge-chromium"></a>Testar com borda (Chromium)

`SameSite` sinalizadores são definidos na página `edge://flags/#same-site-by-default-cookies`. Nenhum problema de compatibilidade foi descoberto com o Edge Chromium.

### <a name="test-with-electron"></a>Testar com o

As versões do at-SS são versões mais antigas do Chromium. Por exemplo, a versão do at-SS usada pelas equipes é Chromium 66, que exibe o comportamento mais antigo. Você deve executar seu próprio teste de compatibilidade com a versão do uso de digital que seu produto usa. Consulte [suporte a navegadores mais antigos](#sob) na seção a seguir.

## <a name="additional-resources"></a>Recursos adicionais

* [Blog do Chromium: desenvolvedores: Prepare-se para o New SameSite = None; Configurações de cookie seguro](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [SameSite cookies explicados](https://web.dev/samesite-cookies-explained/)
* [Problemas de integração de cookies de resposta OWIN e System. Web](https://github.com/aspnet/AspNetKatana/wiki/System.Web-response-cookie-integration-issues)
