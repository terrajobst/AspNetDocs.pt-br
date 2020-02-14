---
title: Trabalhar com cookies SameSite no ASP.NET
author: rick-anderson
description: Saiba como usar o para SameSite cookies no ASP.NET
ms.author: riande
ms.date: 1/22/2019
uid: samesite/system-web-samesite
ms.openlocfilehash: c262e300361f33621e8bd126a34b251c23f56e1a
ms.sourcegitcommit: 6bd0d7581ec36dc32cb85d0d5fc0e51068dd4423
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77234756"
---
# <a name="work-with-samesite-cookies-in-aspnet"></a>Trabalhar com cookies SameSite no ASP.NET

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

SameSite é um padrão de rascunho de [IETF](https://ietf.org/about/) projetado para fornecer alguma proteção contra ataques CSRF (solicitação entre sites forjada). Originalmente rascunho em [2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07), o rascunho padrão foi atualizado em [2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00). O padrão atualizado não é compatível com versões anteriores com o padrão anterior, com as diferenças mais perceptíveis a seguir:

* Cookies sem cabeçalho SameSite são tratados como `SameSite=Lax` por padrão.
* `SameSite=None` deve ser usado para permitir o uso de cookies entre sites.
* Os cookies que afirmam `SameSite=None` também devem ser marcados como `Secure`.
* O valor SameSite = None não é permitido pelo [padrão 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07) e faz com que algumas implementações tratem cookies como SameSite = Strict. Consulte [suporte a navegadores mais antigos](#sob) neste documento.

A configuração `SameSite=Lax` funciona para a maioria dos cookies de aplicativo. Algumas formas de autenticação, como o [OpenID Connect](https://openid.net/connect/) (OIDC) e o [WS-Federation,](https://auth0.com/docs/protocols/ws-fed) assumem o padrão de redirecionamentos baseados em post. Os redirecionamentos baseados em POST disparam as proteções do navegador SameSite, portanto, o SameSite está desabilitado para esses componentes. A maioria dos logons [OAuth](https://oauth.net/) não são afetados devido a diferenças na forma como os fluxos de solicitação.

Os aplicativos que usam `iframe` podem ter problemas com `SameSite=Lax` ou `SameSite=Strict` cookies, pois os iframes são tratados como cenários entre sites.

Cada componente ASP.NET que emite cookies precisa decidir se SameSite é apropriado.

Consulte [problemas conhecidos](#known) para problemas com aplicativos após a instalação das atualizações do 2019 .net SameSite.

## <a name="using-samesite-in-aspnet-472-and-48"></a>Usando SameSite no ASP.NET 4.7.2 e 4,8

O .NET 4.7.2 e 4,8 suporta o [padrão de rascunho 2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00) para SameSite desde o lançamento das atualizações em dezembro de 2019. Os desenvolvedores podem controlar o valor do cabeçalho SameSite usando a [Propriedade HttpCookie. SameSite](/dotnet/api/system.web.httpcookie.samesite#System_Web_HttpCookie_SameSite)de forma programática. Definir a propriedade `SameSite` como `Strict`, `Lax`ou `None` resulta na gravação desses valores na rede com o cookie. Defini-lo como igual a `(SameSiteMode)(-1)` indica que nenhum cabeçalho SameSite deve ser incluído na rede com o cookie. A [Propriedade HttpCookie. Secure](/dotnet/api/system.web.httpcookie.secure), ou ' RequireSSL ' nos arquivos de configuração, pode ser usada para marcar o cookie como `Secure` ou não.

Novas instâncias de `HttpCookie` serão padronizadas para `SameSite=(SameSiteMode)(-1)` e `Secure=false`. Esses padrões podem ser substituídos na seção de configuração `system.web/httpCookies`, em que a cadeia de caracteres `"Unspecified"` é uma sintaxe amigável somente para configuração para `(SameSiteMode)(-1)`:

```xml
<configuration>
 <system.web>
  <httpCookies sameSite="[Strict|Lax|None|Unspecified]" requireSSL="[true|false]" />
 <system.web>
<configuration>
```

O ASP.Net também emite quatro cookies específicos para estes recursos: autenticação anônima, autenticação de formulários, estado de sessão e gerenciamento de função. As instâncias desses cookies obtidas no tempo de execução podem ser manipuladas usando as propriedades `SameSite` e `Secure` assim como qualquer outra instância de HttpCookie. No entanto, devido ao surgimento colcha do padrão SameSite, as opções de configuração para esses quatro recursos cookies são inconsistentes. As seções e os atributos de configuração relevantes, com padrões, são mostrados abaixo. Se não houver nenhum atributo relacionado `SameSite` ou `Secure` para um recurso, o recurso será reproduzido nos padrões configurados na seção `system.web/httpCookies` discutida acima.

```xml
<configuration>
 <system.web>
  <anonymousIdentification cookieRequireSSL="false" /> <!-- No config attribute for SameSite -->
  <authentication>
   <forms cookieSameSite="Lax" requireSSL="false" />
  </authentication>
  <sessionState cookieSameSite="Lax" /> <!-- No config attribute for Secure -->
  <roleManager cookieRequireSSL="false" /> <!-- No config attribute for SameSite -->
 <system.web>
<configuration>
```  

**Observação**: "não especificado" está disponível somente para `system.web/httpCookies@sameSite` no momento. Esperamos adicionar sintaxe semelhante aos atributos cookieSameSite mostrados anteriormente em atualizações futuras. A configuração `(SameSiteMode)(-1)` no código ainda funciona em instâncias desses cookies. *

## <a name="history-and-changes"></a>Histórico e alterações

O suporte a SameSite foi implementado pela primeira vez no .NET 4.7.2 usando o [padrão de rascunho 2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1).

As atualizações de 19 de novembro de 2019 para o Windows atualizaram o .NET 4.7.2 + do padrão 2016 para o padrão 2019. Atualizações adicionais estão em breve para outras versões do Windows. Para obter mais informações, consulte <xref:samesite/kbs-samesite>.

 O rascunho 2019 da especificação de SameSite:

* **Não** é compatível com versões anteriores com o rascunho 2016. Para obter mais informações, consulte [dando suporte a navegadores mais antigos](#sob) neste documento.
* Especifica que os cookies são tratados como `SameSite=Lax` por padrão.
* Especifica cookies que declaram explicitamente `SameSite=None` para habilitar a entrega entre sites também devem ser marcados como `Secure`.
* O é suportado por patches emitidos conforme descrito em KB listados acima.
* Está agendado para ser habilitado pelo [Chrome](https://chromestatus.com/feature/5088147346030592) por padrão em [fevereiro de 2020](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html). Os navegadores começaram a passar para esse padrão em 2019.

<a name="known"><a/>

## <a name="known-issues"></a>Problemas conhecidos

Como as especificações de rascunho 2016 e 2019 não são compatíveis, a atualização de novembro de 2019 do .NET Framework introduz algumas alterações que podem estar sendo interrompidas.

* Os cookies de estado de sessão e de autenticação de formulários agora são gravados na rede como `Lax` em vez de não especificados.
  * Embora a maioria dos aplicativos trabalhe com `SameSite=Lax` cookies, os aplicativos que fazem o POST entre sites ou aplicativos que usam `iframe` podem descobrir que seus Estados de sessão ou cookies de autorização de formulários não estão sendo usados conforme o esperado. Para corrigir isso, altere o valor `cookieSameSite` na seção de configuração apropriada, conforme discutido anteriormente.
* Os HttpCookies que definem explicitamente `SameSite=None` no código ou na configuração agora têm esse valor gravado com o cookie, enquanto ele foi omitido anteriormente. Isso pode causar problemas com navegadores mais antigos que dão suporte apenas ao padrão de rascunho 2016.
  * Ao direcionar os navegadores que dão suporte ao padrão de rascunho 2019 com cookies `SameSite=None`, lembre-se também de marcá-los `Secure` ou eles podem não ser reconhecidos.
  * Para reverter para o comportamento 2016 de não gravar `SameSite=None`, use a configuração de aplicativo `aspnet:SupressSameSiteNone=true`. Observe que isso será aplicado a todos os HttpCookies no aplicativo.

### <a name="azure-app-servicesamesite-cookie-handling"></a>Serviço de Azure App — tratamento de cookies SameSite

Consulte [serviço de Azure app – tratamento de cookies SameSite e .NET Framework patch do 4.7.2](https://azure.microsoft.com/updates/app-service-samesite-cookie-update/) para obter informações sobre como Azure app serviço está configurando comportamentos de SameSite em aplicativos .NET 4.7.2.

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

* [Próximas alterações de cookie SameSite em ASP.NET e ASP.NET Core](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/)
* [Blog do Chromium: desenvolvedores: Prepare-se para o New SameSite = None; Configurações de cookie seguro](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [SameSite cookies explicados](https://web.dev/samesite-cookies-explained/)
