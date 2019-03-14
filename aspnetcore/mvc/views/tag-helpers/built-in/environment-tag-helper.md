---
title: Auxiliar de Marca de Ambiente no ASP.NET Core
author: pkellner
description: Definição de Auxiliar de Marca de Ambiente do ASP.NET Core, incluindo todas as propriedades
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
uid: mvc/views/tag-helpers/builtin-th/environment-tag-helper
ms.openlocfilehash: 379f58ed37329f047d53adf1dcfdfd2ad6a6ca4e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57028623"
---
# <a name="environment-tag-helper-in-aspnet-core"></a><span data-ttu-id="274b3-103">Auxiliar de Marca de Ambiente no ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="274b3-103">Environment Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="274b3-104">Por [Peter Kellner](http://peterkellner.net), [Hisham Bin Ateya](https://twitter.com/hishambinateya) e [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="274b3-104">By [Peter Kellner](http://peterkellner.net), [Hisham Bin Ateya](https://twitter.com/hishambinateya), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="274b3-105">O Auxiliar de Marca de Ambiente renderiza condicionalmente seu conteúdo contido com base no [ambiente de hospedagem](xref:fundamentals/environments) atual.</span><span class="sxs-lookup"><span data-stu-id="274b3-105">The Environment Tag Helper conditionally renders its enclosed content based on the current [hosting environment](xref:fundamentals/environments).</span></span> <span data-ttu-id="274b3-106">Atributo único do Auxiliar de Marca de Ambiente, `names`, é uma lista separada por vírgulas de nomes de ambiente.</span><span class="sxs-lookup"><span data-stu-id="274b3-106">The Environment Tag Helper's single attribute, `names`, is a comma-separated list of environment names.</span></span> <span data-ttu-id="274b3-107">Se nenhum dos nomes de ambiente fornecido corresponder ao ambiente atual, o conteúdo contido será renderizado.</span><span class="sxs-lookup"><span data-stu-id="274b3-107">If any of the provided environment names match the current environment, the enclosed content is rendered.</span></span>

<span data-ttu-id="274b3-108">Para obter uma visão geral dos Auxiliares de Marca, confira <xref:mvc/views/tag-helpers/intro>.</span><span class="sxs-lookup"><span data-stu-id="274b3-108">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

## <a name="environment-tag-helper-attributes"></a><span data-ttu-id="274b3-109">Atributos do Auxiliar de Marca de Ambiente</span><span class="sxs-lookup"><span data-stu-id="274b3-109">Environment Tag Helper Attributes</span></span>

### <a name="names"></a><span data-ttu-id="274b3-110">nomes</span><span class="sxs-lookup"><span data-stu-id="274b3-110">names</span></span>

<span data-ttu-id="274b3-111">`names` aceita um único nome de ambiente de hospedagem ou uma lista separada por vírgula de nomes de ambiente de hospedagem que disparam a renderização do conteúdo contido.</span><span class="sxs-lookup"><span data-stu-id="274b3-111">`names` accepts a single hosting environment name or a comma-separated list of hosting environment names that trigger the rendering of the enclosed content.</span></span>

<span data-ttu-id="274b3-112">Valores de ambiente são comparados com o valor retornado por [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="274b3-112">Environment values are compared to the current value returned by [IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*).</span></span> <span data-ttu-id="274b3-113">A comparação ignora o uso de maiúsculas.</span><span class="sxs-lookup"><span data-stu-id="274b3-113">The comparison ignores case.</span></span>

<span data-ttu-id="274b3-114">O exemplo a seguir usa um Auxiliar de Marca de Ambiente.</span><span class="sxs-lookup"><span data-stu-id="274b3-114">The following example uses an Environment Tag Helper.</span></span> <span data-ttu-id="274b3-115">O conteúdo será renderizado se o ambiente de hospedagem for De Preparo ou de Produção:</span><span class="sxs-lookup"><span data-stu-id="274b3-115">The content is rendered if the hosting environment is Staging or Production:</span></span>

```cshtml
<environment names="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

::: moniker range=">= aspnetcore-2.0"

## <a name="include-and-exclude-attributes"></a><span data-ttu-id="274b3-116">incluir e excluir atributos</span><span class="sxs-lookup"><span data-stu-id="274b3-116">include and exclude attributes</span></span>

<span data-ttu-id="274b3-117">Os atributos `include` & `exclude` controlam a renderização do conteúdo contido com base nos nomes de ambiente de hospedagem incluídos ou excluídos.</span><span class="sxs-lookup"><span data-stu-id="274b3-117">`include` & `exclude` attributes control rendering the enclosed content based on the included or excluded hosting environment names.</span></span>

### <a name="include"></a><span data-ttu-id="274b3-118">include</span><span class="sxs-lookup"><span data-stu-id="274b3-118">include</span></span>

<span data-ttu-id="274b3-119">A propriedade `include` exibe um comportamento semelhante para o atributo `names`.</span><span class="sxs-lookup"><span data-stu-id="274b3-119">The `include` property exhibits similar behavior to the `names` attribute.</span></span> <span data-ttu-id="274b3-120">Um ambiente listado no valor do atributo `include` deve corresponder ao ambiente de hospedagem do aplicativo ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) para renderizar o conteúdo da marcação `<environment>`.</span><span class="sxs-lookup"><span data-stu-id="274b3-120">An environment listed in the `include` attribute value must match the app's hosting environment ([IHostingEnvironment.EnvironmentName](xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName*)) to render the content of the `<environment>` tag.</span></span>

```cshtml
<environment include="Staging,Production">
    <strong>HostingEnvironment.EnvironmentName is Staging or Production</strong>
</environment>
```

### <a name="exclude"></a><span data-ttu-id="274b3-121">exclude</span><span class="sxs-lookup"><span data-stu-id="274b3-121">exclude</span></span>

<span data-ttu-id="274b3-122">Em contraste com o atributo `include`, o conteúdo da marcação `<environment>` é processado quando o ambiente de hospedagem não corresponde a um ambiente listado no valor do atributo `exclude`.</span><span class="sxs-lookup"><span data-stu-id="274b3-122">In contrast to the `include` attribute, the content of the `<environment>` tag is rendered when the hosting environment doesn't match an environment listed in the `exclude` attribute value.</span></span>

```cshtml
<environment exclude="Development">
    <strong>HostingEnvironment.EnvironmentName is not Development</strong>
</environment>
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="274b3-123">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="274b3-123">Additional resources</span></span>

* <xref:fundamentals/environments>
