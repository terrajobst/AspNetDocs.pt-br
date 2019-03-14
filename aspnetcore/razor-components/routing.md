---
title: Componentes de roteamento do Razor
author: guardrex
description: Saiba como rotear solicitações em aplicativos e sobre o componente NavLink.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/01/2019
uid: razor-components/routing
ms.openlocfilehash: 5c648ba1bb3846f5baa515e808a98a3b33f81438
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57031603"
---
# <a name="razor-components-routing"></a>Componentes de roteamento do Razor

Por [Luke Latham](https://github.com/guardrex)

Saiba como rotear solicitações em aplicativos e sobre o componente NavLink.

[Exibir ou baixar um código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([como baixar](xref:index#how-to-download-a-sample)). Veja o tópico [Introdução](xref:razor-components/get-started) para conhecer os pré-requisitos.

## <a name="route-templates"></a>Modelos de rota

O `<Router>` habilita o componente de roteamento e um modelo de rota é fornecido para cada componente acessível. O `<Router>` componente aparece na *App.cshtml* arquivo:

```cshtml
<Router AppAssembly=typeof(Program).Assembly />
```

Quando um  *\*. cshtml* do arquivo com um `@page` diretiva é compilada, a classe gerada recebe um [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) especificando o modelo de rota. Em tempo de execução, o roteador procura por classes de componente com um `RouteAttribute` e renderiza a qualquer componente tem um modelo de rota que corresponde à URL solicitada.

Vários modelos de rota podem ser aplicados a um componente. No [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), o componente a seguir responde a solicitações para `/BlazorRoute` e `/DifferentBlazorRoute`.

*Pages/BlazorRoute.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

`<Router>` dá suporte à configuração de um componente de fallback para a renderização quando uma rota solicitada não é resolvido. Habilitar esse cenário consentir, definindo o `FallbackComponent` parâmetro para o tipo da classe de componente de fallback.

O exemplo a seguir define um componente definido no *Pages/MyFallbackRazorComponent.cshtml* como o componente de fallback para um `<Router>`:

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> Para gerar rotas corretamente, o aplicativo deve incluir um `<base>` marcar no seu *wwwroot/index.html* arquivo com o caminho base do aplicativo especificado no `href` atributo (`<base href="/" />`). Para obter mais informações, consulte [Host e implantar: Caminho base do aplicativo](xref:host-and-deploy/razor-components/index#app-base-path).

## <a name="route-parameters"></a>Parâmetros de rota

O roteador usa parâmetros de rota para preencher os parâmetros correspondentes do componente com o mesmo nome (diferencia maiusculas de minúsculas).

*Pages/RouteParameter.cshtml*:

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=8)]

Parâmetros opcionais não têm suporte ainda, portanto, dois `@page` diretivas são aplicadas no exemplo acima. A primeira permite a navegação para o componente sem um parâmetro. A segunda `@page` diretiva leva a `{text}` parâmetro de rota e atribui o valor para o `Text` propriedade.

## <a name="route-constraints"></a>Restrições de rota

Uma restrição de rota impõe o tipo correspondente em um segmento de rota para um componente.

No exemplo a seguir, a rota para o componente de usuários corresponde apenas se:

* Um `Id` segmento de rota está presente na URL de solicitação.
* O `Id` segmento é um inteiro (`int`).

```cshtml
@page "/Users/{Id:int}"

<h1>The user Id is @Id!</h1>

@functions {
    [Parameter]
    private int Id { get; set; }
}
```

As restrições da rota mostradas na tabela a seguir estão disponíveis para uso. Para as restrições da rota que coincidirem com a cultura invariável, consulte o aviso abaixo da tabela para obter mais informações.

| Restrição | Exemplo           | Correspondências de exemplo                                                                  | Constante<br>cultura<br>correspondência |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | `true`, `FALSE`                                                                  | Não                               |
| `datetime` | `{dob:datetime}`  | `2016-12-31`, `2016-12-31 7:32pm`                                                | Sim                              |
| `decimal`  | `{price:decimal}` | `49.99`, `-1,000.01`                                                             | Sim                              |
| `double`   | `{weight:double}` | `1.234`, `-1,001.01e8`                                                           | Sim                              |
| `float`    | `{weight:float}`  | `1.234`, `-1,001.01e8`                                                           | Sim                              |
| `guid`     | `{id:guid}`       | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Não                               |
| `int`      | `{id:int}`        | `123456789`, `-123456789`                                                        | Sim                              |
| `long`     | `{ticks:long}`    | `123456789`, `-123456789`                                                        | Sim                              |

> [!WARNING]
> As restrições de rota que verificam a URL e são convertidas em um tipo CLR (como `int` ou `DateTime`) sempre usam a cultura invariável. Essas restrições consideram que a URL não é localizável.

## <a name="navlink-component"></a>Componente NavLink

Usar um componente NavLink no lugar de HTML  **\<um >** elementos durante a criação de links de navegação. Um componente NavLink se comporta como uma  **\<um >** elemento, exceto que ele é alternado um `active` classe CSS com base na possibilidade seu `href` corresponde à URL atual. O `active` classe ajuda um usuário a entender qual página é a página ativa entre os links de navegação exibidos.

O componente NavMenu na [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) cria um [Bootstrap](https://getbootstrap.com/docs/) barra de navegação que demonstra como usar componentes NavLink. A marcação a seguir mostra as duas primeiras NavLinks na *Shared/NavMenu.cshtml* arquivo.

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?start=13&end=24&highlight=4-6,9-11)]

Há dois `NavLinkMatch` opções:

* `NavLinkMatch.All` &ndash; Especifica que o NavLink deve estar ativo quando ele corresponder a toda a URL atual.
* `NavLinkMatch.Prefix` &ndash; Especifica que o NavLink deve estar ativo quando há correspondência com qualquer prefixo da URL atual.

No exemplo anterior, NavLink Home (`href=""`) corresponde a todas as URLs e sempre recebe o `active` classe CSS. O segundo NavLink recebe apenas a `active` classe quando o usuário visitar o componente BlazorRoute (`href="BlazorRoute"`).
