---
title: Trabalhar com cookies SameSite no ASP.NET
author: rick-anderson
description: Saiba como usar o para SameSite cookies no ASP.NET
ms.author: riande
ms.date: 12/03/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: 47a3d7576edb0e818c39b32fbbcb98475248e18e
ms.sourcegitcommit: 7b1e1784213dd4c301635f9e181764f3e2f94162
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74993067"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a>Trabalhar com cookies SameSite no ASP.NET

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

SameSite é um rascunho de [IETF](https://ietf.org/about/) projetado para fornecer uma proteção contra ataques de CSRF (solicitação entre sites forjados). O [rascunho do SameSite 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00):

* Trata cookies como `SameSite=Lax` por padrão.
* Os cookies de Estados que declaram explicitamente `SameSite=None` para habilitar a entrega entre sites devem ser marcados como `Secure`.

`Lax` funciona para a maioria dos cookies de aplicativo. Algumas formas de autenticação, como o [OpenID Connect](https://openid.net/connect/) (OIDC) e o [WS-Federation,](https://auth0.com/docs/protocols/ws-fed) assumem o padrão de redirecionamentos baseados em post. Os redirecionamentos baseados em POST disparam as proteções do navegador SameSite, portanto, o SameSite está desabilitado para esses componentes. A maioria dos logons [OAuth](https://oauth.net/) não são afetados devido a diferenças na forma como os fluxos de solicitação.

O parâmetro `None` causa problemas de compatibilidade com clientes que implementaram o padrão anterior de [rascunho 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) (por exemplo, IOS 12). Consulte [suporte a navegadores mais antigos](#sob) neste documento.

Cada componente ASP.NET Core que emite cookies precisa decidir se SameSite é apropriado.

## <a name="api-usage-with-samesite"></a>Uso da API com SameSite

Consulte a [Propriedade HttpCookie. SameSite](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite)

## <a name="history-and-changes"></a>Histórico e alterações

O suporte a SameSite foi implementado pela primeira vez no .NET 4.7.2 usando o [padrão de rascunho 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

As atualizações de 19 de novembro de 2019 para o Windows atualizaram o .NET 4.7.2 + do padrão 2016 para o padrão 2019. Atualizações adicionais estão em breve para outras versões do Windows. Para obter mais informações, consulte <xref:samesite/kbs-samesite>.

 O rascunho 2019 da especificação de SameSite:

* **Não** é compatível com versões anteriores com o rascunho 2016. Para obter mais informações, consulte [dando suporte a navegadores mais antigos](#sob) neste documento.
* Especifica que os cookies são tratados como `SameSite=Lax` por padrão.
* Especifica cookies que declaram explicitamente `SameSite=None` para habilitar a entrega entre sites devem ser marcados como `Secure`. `None` é uma nova entrada a ser recusada.
* O é suportado por patches emitidos conforme descrito em KB listados acima.
* Está agendado para ser habilitado pelo [Chrome](https://chromestatus.com/feature/5088147346030592) por padrão em [fevereiro de 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html). Os navegadores começaram a passar para esse padrão em 2019.

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>Suporte a navegadores mais antigos

O padrão de 2016 SameSite exigiu que valores desconhecidos devem ser tratados como valores `SameSite=Strict`. Os aplicativos acessados de navegadores mais antigos que dão suporte ao padrão de 2016 SameSite podem falhar quando obtêm uma propriedade SameSite com um valor de `None`. Os aplicativos Web devem implementar a detecção do navegador se pretenderem oferecer suporte a navegadores mais antigos. O ASP.NET não implementa a detecção de navegador, pois os valores dos agentes de usuário são altamente voláteis e mudam com frequência. O código a seguir pode ser chamado no site de chamada <xref:HTTP.HttpCookie>:

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet)]

No exemplo anterior, `MyUserAgentDetectionLib.DisallowsSameSiteNone` é uma biblioteca fornecida pelo usuário que detecta se o agente do usuário não dá suporte a `None`SameSite. O código a seguir mostra um exemplo de método de `DisallowsSameSiteNone`:

> [!WARNING]
> O código a seguir é apenas para demonstração:
> * Ele não deve ser considerado concluído.
> * Ele não é mantido ou tem suporte.

[!code-csharp[](sample/SameSiteCheck.cs?name=snippet2)]

## <a name="test-apps-for-samesite-problems"></a>Testar aplicativos para problemas de SameSite

Aplicativos que interagem com sites remotos, como por meio de logon de terceiros, precisam:

* Teste a interação em vários navegadores.
* Aplique a [detecção e mitigação do navegador](#sob) discutidas neste documento.

Teste os aplicativos Web usando uma versão do cliente que pode aceitar o novo comportamento de SameSite. O Chrome, o Firefox e o Chromium Edge têm novos sinalizadores de recurso de aceitação que podem ser usados para teste. Depois que seu aplicativo aplicar os patches do SameSite, teste-o com versões mais antigas do cliente, especialmente o Safari. Para obter mais informações, consulte [dando suporte a navegadores mais antigos](#sob) neste documento.

### <a name="test-with-chrome"></a>Teste com o Chrome

O Chrome 78 + fornece resultados enganosos porque tem uma mitigação temporária em vigor. A mitigação do Chrome 78 + temporária permite cookies com menos de dois minutos. O Chrome 76 ou 77 com os sinalizadores de teste apropriados habilitados fornece resultados mais precisos. Para testar o novo comportamento de SameSite, alterne `chrome://flags/#same-site-by-default-cookies` para **habilitado**. Versões mais antigas do Chrome (75 e inferior) são relatadas para falha com a nova configuração de `None`. Consulte [suporte a navegadores mais antigos](#sob) neste documento.

O Google não disponibiliza versões mais antigas do Chrome. Siga as instruções em [baixar o Chromium](https://www.chromium.org/getting-involved/download-chromium) para testar versões anteriores do Chrome. **Não** Baixe o Chrome de links fornecidos pela pesquisa de versões mais antigas do Chrome.

* [Chromium 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chromium 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

### <a name="test-with-safari"></a>Teste com o Safari

O Safari 12 implementou estritamente o rascunho anterior e falha quando o novo valor de `None` está em um cookie. `None` é evitada por meio do código de detecção de navegador que [dá suporte a navegadores mais antigos](#sob) neste documento. Teste OS logons de estilo do sistema operacional baseado no Safari 12, Safari 13 e WebKit usando MSAL, ADAL ou qualquer biblioteca que você esteja usando. O problema depende da versão subjacente do sistema operacional. OSX Mojave (10,14) e iOS 12 são conhecidos por ter problemas de compatibilidade com o novo comportamento de SameSite. Atualizar o sistema operacional para OSX Catalina (10,15) ou iOS 13 corrige o problema. No momento, o Safari não tem um sinalizador de aceitação para testar o novo comportamento de especificação.

### <a name="test-with-firefox"></a>Testar com o Firefox

O suporte do Firefox para o novo padrão pode ser testado na versão 68 +, optando na página de `about:config` com o sinalizador de recurso `network.cookie.sameSite.laxByDefault`. Não há relatórios de problemas de compatibilidade com versões mais antigas do Firefox.

### <a name="test-with-edge-browser"></a>Testar com o navegador Edge

O Edge dá suporte ao antigo SameSite padrão. A versão 44 do Edge não tem nenhum problema de compatibilidade conhecido com o novo padrão.

### <a name="test-with-edge-chromium"></a>Testar com borda (Chromium)

Os sinalizadores SameSite são definidos na página `edge://flags/#same-site-by-default-cookies`. Nenhum problema de compatibilidade foi descoberto com o Edge Chromium.

### <a name="test-with-electron"></a>Testar com o

As versões do at-SS são versões mais antigas do Chromium. Por exemplo, a versão do at-SS usada pelas equipes é Chromium 66, que exibe o comportamento mais antigo. Você deve executar seu próprio teste de compatibilidade com a versão do uso de digital que seu produto usa. Consulte [suporte a navegadores mais antigos](#sob) na seção a seguir.

## <a name="additional-resources"></a>Recursos adicionais

* [Blog do Chromium: desenvolvedores: Prepare-se para o New SameSite = None; Configurações de cookie seguro](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [SameSite cookies explicados](https://web.dev/samesite-cookies-explained/)
