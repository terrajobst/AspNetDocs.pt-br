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
# <a name="razor-components-routing"></a><span data-ttu-id="5a45d-103">Componentes de roteamento do Razor</span><span class="sxs-lookup"><span data-stu-id="5a45d-103">Razor Components routing</span></span>

<span data-ttu-id="5a45d-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5a45d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5a45d-105">Saiba como rotear solicitações em aplicativos e sobre o componente NavLink.</span><span class="sxs-lookup"><span data-stu-id="5a45d-105">Learn how to route requests in apps and about the NavLink component.</span></span>

<span data-ttu-id="5a45d-106">[Exibir ou baixar um código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([como baixar](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="5a45d-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="5a45d-107">Veja o tópico [Introdução](xref:razor-components/get-started) para conhecer os pré-requisitos.</span><span class="sxs-lookup"><span data-stu-id="5a45d-107">See the [Get started](xref:razor-components/get-started) topic for prerequisites.</span></span>

## <a name="route-templates"></a><span data-ttu-id="5a45d-108">Modelos de rota</span><span class="sxs-lookup"><span data-stu-id="5a45d-108">Route templates</span></span>

<span data-ttu-id="5a45d-109">O `<Router>` habilita o componente de roteamento e um modelo de rota é fornecido para cada componente acessível.</span><span class="sxs-lookup"><span data-stu-id="5a45d-109">The `<Router>` component enables routing, and a route template is provided to each accessible component.</span></span> <span data-ttu-id="5a45d-110">O `<Router>` componente aparece na *App.cshtml* arquivo:</span><span class="sxs-lookup"><span data-stu-id="5a45d-110">The `<Router>` component appears in the *App.cshtml* file:</span></span>

```cshtml
<Router AppAssembly=typeof(Program).Assembly />
```

<span data-ttu-id="5a45d-111">Quando um  *\*. cshtml* do arquivo com um `@page` diretiva é compilada, a classe gerada recebe um [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) especificando o modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="5a45d-111">When a *\*.cshtml* file with an `@page` directive is compiled, the generated class is given a [RouteAttribute](/dotnet/api/microsoft.aspnetcore.mvc.routeattribute) specifying the route template.</span></span> <span data-ttu-id="5a45d-112">Em tempo de execução, o roteador procura por classes de componente com um `RouteAttribute` e renderiza a qualquer componente tem um modelo de rota que corresponde à URL solicitada.</span><span class="sxs-lookup"><span data-stu-id="5a45d-112">At runtime, the router looks for component classes with a `RouteAttribute` and renders whichever component has a route template that matches the requested URL.</span></span>

<span data-ttu-id="5a45d-113">Vários modelos de rota podem ser aplicados a um componente.</span><span class="sxs-lookup"><span data-stu-id="5a45d-113">Multiple route templates can be applied to a component.</span></span> <span data-ttu-id="5a45d-114">No [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), o componente a seguir responde a solicitações para `/BlazorRoute` e `/DifferentBlazorRoute`.</span><span class="sxs-lookup"><span data-stu-id="5a45d-114">In the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/), the following component responds to requests for `/BlazorRoute` and `/DifferentBlazorRoute`.</span></span>

<span data-ttu-id="5a45d-115">*Pages/BlazorRoute.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5a45d-115">*Pages/BlazorRoute.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/BlazorRoute.cshtml?start=1&end=4)]

<span data-ttu-id="5a45d-116">`<Router>` dá suporte à configuração de um componente de fallback para a renderização quando uma rota solicitada não é resolvido.</span><span class="sxs-lookup"><span data-stu-id="5a45d-116">`<Router>` supports setting a fallback component for rendering when a requested route isn't resolved.</span></span> <span data-ttu-id="5a45d-117">Habilitar esse cenário consentir, definindo o `FallbackComponent` parâmetro para o tipo da classe de componente de fallback.</span><span class="sxs-lookup"><span data-stu-id="5a45d-117">Enable this opt-in scenario by setting the `FallbackComponent` parameter to the type of the fallback component class.</span></span>

<span data-ttu-id="5a45d-118">O exemplo a seguir define um componente definido no *Pages/MyFallbackRazorComponent.cshtml* como o componente de fallback para um `<Router>`:</span><span class="sxs-lookup"><span data-stu-id="5a45d-118">The following example sets a component defined in *Pages/MyFallbackRazorComponent.cshtml* as the fallback component for a `<Router>`:</span></span>

```cshtml
<Router ... FallbackComponent="typeof(Pages.MyFallbackRazorComponent)" />
```

> [!IMPORTANT]
> <span data-ttu-id="5a45d-119">Para gerar rotas corretamente, o aplicativo deve incluir um `<base>` marcar no seu *wwwroot/index.html* arquivo com o caminho base do aplicativo especificado no `href` atributo (`<base href="/" />`).</span><span class="sxs-lookup"><span data-stu-id="5a45d-119">To generate routes properly, the app must include a `<base>` tag in its *wwwroot/index.html* file with the app base path specified in the `href` attribute (`<base href="/" />`).</span></span> <span data-ttu-id="5a45d-120">Para obter mais informações, consulte [Host e implantar: Caminho base do aplicativo](xref:host-and-deploy/razor-components/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="5a45d-120">For more information, see [Host and deploy: App base path](xref:host-and-deploy/razor-components/index#app-base-path).</span></span>

## <a name="route-parameters"></a><span data-ttu-id="5a45d-121">Parâmetros de rota</span><span class="sxs-lookup"><span data-stu-id="5a45d-121">Route parameters</span></span>

<span data-ttu-id="5a45d-122">O roteador usa parâmetros de rota para preencher os parâmetros correspondentes do componente com o mesmo nome (diferencia maiusculas de minúsculas).</span><span class="sxs-lookup"><span data-stu-id="5a45d-122">The router uses route parameters to populate the corresponding component parameters with the same name (case insensitive).</span></span>

<span data-ttu-id="5a45d-123">*Pages/RouteParameter.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="5a45d-123">*Pages/RouteParameter.cshtml*:</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Pages/RouteParameter.cshtml?start=1&end=8)]

<span data-ttu-id="5a45d-124">Parâmetros opcionais não têm suporte ainda, portanto, dois `@page` diretivas são aplicadas no exemplo acima.</span><span class="sxs-lookup"><span data-stu-id="5a45d-124">Optional parameters aren't supported yet, so two `@page` directives are applied in the example above.</span></span> <span data-ttu-id="5a45d-125">A primeira permite a navegação para o componente sem um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="5a45d-125">The first permits navigation to the component without a parameter.</span></span> <span data-ttu-id="5a45d-126">A segunda `@page` diretiva leva a `{text}` parâmetro de rota e atribui o valor para o `Text` propriedade.</span><span class="sxs-lookup"><span data-stu-id="5a45d-126">The second `@page` directive takes the `{text}` route parameter and assigns the value to the `Text` property.</span></span>

## <a name="route-constraints"></a><span data-ttu-id="5a45d-127">Restrições de rota</span><span class="sxs-lookup"><span data-stu-id="5a45d-127">Route constraints</span></span>

<span data-ttu-id="5a45d-128">Uma restrição de rota impõe o tipo correspondente em um segmento de rota para um componente.</span><span class="sxs-lookup"><span data-stu-id="5a45d-128">A route constraint enforces type matching on a route segment to a component.</span></span>

<span data-ttu-id="5a45d-129">No exemplo a seguir, a rota para o componente de usuários corresponde apenas se:</span><span class="sxs-lookup"><span data-stu-id="5a45d-129">In the following example, the route to the Users component only matches if:</span></span>

* <span data-ttu-id="5a45d-130">Um `Id` segmento de rota está presente na URL de solicitação.</span><span class="sxs-lookup"><span data-stu-id="5a45d-130">An `Id` route segment is present on the request URL.</span></span>
* <span data-ttu-id="5a45d-131">O `Id` segmento é um inteiro (`int`).</span><span class="sxs-lookup"><span data-stu-id="5a45d-131">The `Id` segment is an integer (`int`).</span></span>

```cshtml
@page "/Users/{Id:int}"

<h1>The user Id is @Id!</h1>

@functions {
    [Parameter]
    private int Id { get; set; }
}
```

<span data-ttu-id="5a45d-132">As restrições da rota mostradas na tabela a seguir estão disponíveis para uso.</span><span class="sxs-lookup"><span data-stu-id="5a45d-132">The route constraints shown in the following table are available for use.</span></span> <span data-ttu-id="5a45d-133">Para as restrições da rota que coincidirem com a cultura invariável, consulte o aviso abaixo da tabela para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="5a45d-133">For the route constraints that match with the invariant culture, see the warning below the table for more information.</span></span>

| <span data-ttu-id="5a45d-134">Restrição</span><span class="sxs-lookup"><span data-stu-id="5a45d-134">Constraint</span></span> | <span data-ttu-id="5a45d-135">Exemplo</span><span class="sxs-lookup"><span data-stu-id="5a45d-135">Example</span></span>           | <span data-ttu-id="5a45d-136">Correspondências de exemplo</span><span class="sxs-lookup"><span data-stu-id="5a45d-136">Example Matches</span></span>                                                                  | <span data-ttu-id="5a45d-137">Constante</span><span class="sxs-lookup"><span data-stu-id="5a45d-137">Invariant</span></span><br><span data-ttu-id="5a45d-138">cultura</span><span class="sxs-lookup"><span data-stu-id="5a45d-138">culture</span></span><br><span data-ttu-id="5a45d-139">correspondência</span><span class="sxs-lookup"><span data-stu-id="5a45d-139">matching</span></span> |
| ---------- | ----------------- | -------------------------------------------------------------------------------- | :------------------------------: |
| `bool`     | `{active:bool}`   | <span data-ttu-id="5a45d-140">`true`, `FALSE`</span><span class="sxs-lookup"><span data-stu-id="5a45d-140">`true`, `FALSE`</span></span>                                                                  | <span data-ttu-id="5a45d-141">Não</span><span class="sxs-lookup"><span data-stu-id="5a45d-141">No</span></span>                               |
| `datetime` | `{dob:datetime}`  | <span data-ttu-id="5a45d-142">`2016-12-31`, `2016-12-31 7:32pm`</span><span class="sxs-lookup"><span data-stu-id="5a45d-142">`2016-12-31`, `2016-12-31 7:32pm`</span></span>                                                | <span data-ttu-id="5a45d-143">Sim</span><span class="sxs-lookup"><span data-stu-id="5a45d-143">Yes</span></span>                              |
| `decimal`  | `{price:decimal}` | <span data-ttu-id="5a45d-144">`49.99`, `-1,000.01`</span><span class="sxs-lookup"><span data-stu-id="5a45d-144">`49.99`, `-1,000.01`</span></span>                                                             | <span data-ttu-id="5a45d-145">Sim</span><span class="sxs-lookup"><span data-stu-id="5a45d-145">Yes</span></span>                              |
| `double`   | `{weight:double}` | <span data-ttu-id="5a45d-146">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="5a45d-146">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="5a45d-147">Sim</span><span class="sxs-lookup"><span data-stu-id="5a45d-147">Yes</span></span>                              |
| `float`    | `{weight:float}`  | <span data-ttu-id="5a45d-148">`1.234`, `-1,001.01e8`</span><span class="sxs-lookup"><span data-stu-id="5a45d-148">`1.234`, `-1,001.01e8`</span></span>                                                           | <span data-ttu-id="5a45d-149">Sim</span><span class="sxs-lookup"><span data-stu-id="5a45d-149">Yes</span></span>                              |
| `guid`     | `{id:guid}`       | <span data-ttu-id="5a45d-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span><span class="sxs-lookup"><span data-stu-id="5a45d-150">`CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}`</span></span> | <span data-ttu-id="5a45d-151">Não</span><span class="sxs-lookup"><span data-stu-id="5a45d-151">No</span></span>                               |
| `int`      | `{id:int}`        | <span data-ttu-id="5a45d-152">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="5a45d-152">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="5a45d-153">Sim</span><span class="sxs-lookup"><span data-stu-id="5a45d-153">Yes</span></span>                              |
| `long`     | `{ticks:long}`    | <span data-ttu-id="5a45d-154">`123456789`, `-123456789`</span><span class="sxs-lookup"><span data-stu-id="5a45d-154">`123456789`, `-123456789`</span></span>                                                        | <span data-ttu-id="5a45d-155">Sim</span><span class="sxs-lookup"><span data-stu-id="5a45d-155">Yes</span></span>                              |

> [!WARNING]
> <span data-ttu-id="5a45d-156">As restrições de rota que verificam a URL e são convertidas em um tipo CLR (como `int` ou `DateTime`) sempre usam a cultura invariável.</span><span class="sxs-lookup"><span data-stu-id="5a45d-156">Route constraints that verify the URL and are converted to a CLR type (such as `int` or `DateTime`) always use the invariant culture.</span></span> <span data-ttu-id="5a45d-157">Essas restrições consideram que a URL não é localizável.</span><span class="sxs-lookup"><span data-stu-id="5a45d-157">These constraints assume that the URL is non-localizable.</span></span>

## <a name="navlink-component"></a><span data-ttu-id="5a45d-158">Componente NavLink</span><span class="sxs-lookup"><span data-stu-id="5a45d-158">NavLink component</span></span>

<span data-ttu-id="5a45d-159">Usar um componente NavLink no lugar de HTML  **\<um >** elementos durante a criação de links de navegação.</span><span class="sxs-lookup"><span data-stu-id="5a45d-159">Use a NavLink component in place of HTML **\<a>** elements when creating navigation links.</span></span> <span data-ttu-id="5a45d-160">Um componente NavLink se comporta como uma  **\<um >** elemento, exceto que ele é alternado um `active` classe CSS com base na possibilidade seu `href` corresponde à URL atual.</span><span class="sxs-lookup"><span data-stu-id="5a45d-160">A NavLink component behaves like an **\<a>** element, except it toggles an `active` CSS class based on whether its `href` matches the current URL.</span></span> <span data-ttu-id="5a45d-161">O `active` classe ajuda um usuário a entender qual página é a página ativa entre os links de navegação exibidos.</span><span class="sxs-lookup"><span data-stu-id="5a45d-161">The `active` class helps a user understand which page is the active page among the navigation links displayed.</span></span>

<span data-ttu-id="5a45d-162">O componente NavMenu na [aplicativo de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) cria um [Bootstrap](https://getbootstrap.com/docs/) barra de navegação que demonstra como usar componentes NavLink.</span><span class="sxs-lookup"><span data-stu-id="5a45d-162">The NavMenu component in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-components/common/samples/) creates a [Bootstrap](https://getbootstrap.com/docs/) nav bar that demonstrates how to use NavLink components.</span></span> <span data-ttu-id="5a45d-163">A marcação a seguir mostra as duas primeiras NavLinks na *Shared/NavMenu.cshtml* arquivo.</span><span class="sxs-lookup"><span data-stu-id="5a45d-163">The following markup shows the first two NavLinks in the *Shared/NavMenu.cshtml* file.</span></span>

[!code-cshtml[](common/samples/3.x/BlazorSample/Shared/NavMenu.cshtml?start=13&end=24&highlight=4-6,9-11)]

<span data-ttu-id="5a45d-164">Há dois `NavLinkMatch` opções:</span><span class="sxs-lookup"><span data-stu-id="5a45d-164">There are two `NavLinkMatch` options:</span></span>

* <span data-ttu-id="5a45d-165">`NavLinkMatch.All` &ndash; Especifica que o NavLink deve estar ativo quando ele corresponder a toda a URL atual.</span><span class="sxs-lookup"><span data-stu-id="5a45d-165">`NavLinkMatch.All` &ndash; Specifies that the NavLink should be active when it matches the entire current URL.</span></span>
* <span data-ttu-id="5a45d-166">`NavLinkMatch.Prefix` &ndash; Especifica que o NavLink deve estar ativo quando há correspondência com qualquer prefixo da URL atual.</span><span class="sxs-lookup"><span data-stu-id="5a45d-166">`NavLinkMatch.Prefix` &ndash; Specifies that the NavLink should be active when it matches any prefix of the current URL.</span></span>

<span data-ttu-id="5a45d-167">No exemplo anterior, NavLink Home (`href=""`) corresponde a todas as URLs e sempre recebe o `active` classe CSS.</span><span class="sxs-lookup"><span data-stu-id="5a45d-167">In the preceding example, the Home NavLink (`href=""`) matches all URLs and always receives the `active` CSS class.</span></span> <span data-ttu-id="5a45d-168">O segundo NavLink recebe apenas a `active` classe quando o usuário visitar o componente BlazorRoute (`href="BlazorRoute"`).</span><span class="sxs-lookup"><span data-stu-id="5a45d-168">The second NavLink only receives the `active` class when the user visits the BlazorRoute component (`href="BlazorRoute"`).</span></span>
