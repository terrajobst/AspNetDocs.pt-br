---
title: Layouts de componentes do Razor
author: guardrex
description: Saiba como criar componentes reutilizáveis de layout para aplicativos Blazor e componentes do Razor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/layouts
ms.openlocfilehash: 23d8f441c0b3bbde7a73717f6257013831617ec0
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57039033"
---
# <a name="razor-components-layouts"></a><span data-ttu-id="6f03a-103">Layouts de componentes do Razor</span><span class="sxs-lookup"><span data-stu-id="6f03a-103">Razor Components layouts</span></span>

<span data-ttu-id="6f03a-104">Por [Rainer Stropek](https://www.timecockpit.com)</span><span class="sxs-lookup"><span data-stu-id="6f03a-104">By [Rainer Stropek](https://www.timecockpit.com)</span></span>

<span data-ttu-id="6f03a-105">Os aplicativos normalmente contêm mais de uma página.</span><span class="sxs-lookup"><span data-stu-id="6f03a-105">Apps typically contain more than one page.</span></span> <span data-ttu-id="6f03a-106">Elementos de layout, como menus, mensagens de direitos autorais e logotipos, devem estar presentes em todas as páginas.</span><span class="sxs-lookup"><span data-stu-id="6f03a-106">Layout elements, such as menus, copyright messages, and logos, must be present on all pages.</span></span> <span data-ttu-id="6f03a-107">Copiando o código desses elementos de layout para todas as páginas de um aplicativo não é uma solução eficiente.</span><span class="sxs-lookup"><span data-stu-id="6f03a-107">Copying the code of these layout elements into all of the pages of an app isn't an efficient solution.</span></span> <span data-ttu-id="6f03a-108">Essa duplicação é difícil de manter e provavelmente leva ao conteúdo inconsistente ao longo do tempo.</span><span class="sxs-lookup"><span data-stu-id="6f03a-108">Such duplication is hard to maintain and probably leads to inconsistent content over time.</span></span> <span data-ttu-id="6f03a-109">*Layouts* resolver esse problema.</span><span class="sxs-lookup"><span data-stu-id="6f03a-109">*Layouts* solve this problem.</span></span>

<span data-ttu-id="6f03a-110">Tecnicamente, um layout é apenas outro componente.</span><span class="sxs-lookup"><span data-stu-id="6f03a-110">Technically, a layout is just another component.</span></span> <span data-ttu-id="6f03a-111">Um layout é definido em um modelo Razor ou em C# de código e pode conter outros recursos comuns de componentes, injeção de dependência e vinculação de dados.</span><span class="sxs-lookup"><span data-stu-id="6f03a-111">A layout is defined in a Razor template or in C# code and can contain data binding, dependency injection, and other ordinary features of components.</span></span> <span data-ttu-id="6f03a-112">Dois aspectos adicionais ativar uma *componente* em um *layout*:</span><span class="sxs-lookup"><span data-stu-id="6f03a-112">Two additional aspects turn a *component* into a *layout*:</span></span>

* <span data-ttu-id="6f03a-113">O componente de layout deve herdar de `BlazorLayoutComponent`.</span><span class="sxs-lookup"><span data-stu-id="6f03a-113">The layout component must inherit from `BlazorLayoutComponent`.</span></span> <span data-ttu-id="6f03a-114">`BlazorLayoutComponent` define um `Body` propriedade que contém o conteúdo a ser renderizado dentro do layout.</span><span class="sxs-lookup"><span data-stu-id="6f03a-114">`BlazorLayoutComponent` defines a `Body` property that contains the content to be rendered inside the layout.</span></span>
* <span data-ttu-id="6f03a-115">O componente de layout usa o `Body` propriedade para especificar onde o conteúdo do corpo deve ser renderizado usando a sintaxe do Razor `@Body`.</span><span class="sxs-lookup"><span data-stu-id="6f03a-115">The layout component uses the `Body` property to specify where the body content should be rendered using the Razor syntax `@Body`.</span></span> <span data-ttu-id="6f03a-116">Durante o processamento, `@Body` é substituída pelo conteúdo do layout.</span><span class="sxs-lookup"><span data-stu-id="6f03a-116">During rendering, `@Body` is replaced by the content of the layout.</span></span>

<span data-ttu-id="6f03a-117">O exemplo de código a seguir mostra o modelo do Razor de um componente de layout.</span><span class="sxs-lookup"><span data-stu-id="6f03a-117">The following code sample shows the Razor template of a layout component.</span></span> <span data-ttu-id="6f03a-118">Observe o uso de `BlazorLayoutComponent` e `@Body`:</span><span class="sxs-lookup"><span data-stu-id="6f03a-118">Note the use of `BlazorLayoutComponent` and `@Body`:</span></span>

```csharp
@inherits BlazorLayoutComponent

<header>
    <h1>ERP Master 3000</h1>
</header>

<nav>
    <a href="master-data">Master Data Management</a>
    <a href="invoicing">Invoicing</a>
    <a href="accounting">Accounting</a>
</nav>

@Body

<footer>
    &copy; by @CopyrightMessage
</footer>

@functions {
    public string CopyrightMessage { get; set; }
    ...
}
```

## <a name="use-a-layout-in-a-component"></a><span data-ttu-id="6f03a-119">Usar um layout em um componente</span><span class="sxs-lookup"><span data-stu-id="6f03a-119">Use a layout in a component</span></span>

<span data-ttu-id="6f03a-120">Use a diretiva do Razor `@layout` para aplicar um layout para um componente.</span><span class="sxs-lookup"><span data-stu-id="6f03a-120">Use the Razor directive `@layout` to apply a layout to a component.</span></span> <span data-ttu-id="6f03a-121">O compilador converte essa diretiva em um `LayoutAttribute`, que é aplicado à classe de componente.</span><span class="sxs-lookup"><span data-stu-id="6f03a-121">The compiler converts this directive into a `LayoutAttribute`, which is applied to the component class.</span></span>

<span data-ttu-id="6f03a-122">O exemplo de código a seguir demonstra o conceito.</span><span class="sxs-lookup"><span data-stu-id="6f03a-122">The following code sample demonstrates the concept.</span></span> <span data-ttu-id="6f03a-123">O conteúdo desse componente é inserido na *MasterLayout* na posição do `@Body`:</span><span class="sxs-lookup"><span data-stu-id="6f03a-123">The content of this component is inserted into the *MasterLayout* at the position of `@Body`:</span></span>

```csharp
@layout MasterLayout

@page "/master-data"

<h2>Master Data Management</h2>
...
```

## <a name="centralized-layout-selection"></a><span data-ttu-id="6f03a-124">Seleção de layout centralizado</span><span class="sxs-lookup"><span data-stu-id="6f03a-124">Centralized layout selection</span></span>

<span data-ttu-id="6f03a-125">Todas as pastas de um um aplicativo pode, opcionalmente, conter um arquivo de modelo chamado *viewimports. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6f03a-125">Every folder of a an app can optionally contain a template file named *_ViewImports.cshtml*.</span></span> <span data-ttu-id="6f03a-126">O compilador incluirá as diretivas especificadas no arquivo de importações de exibição em todos os modelos do Razor na mesma pasta e recursivamente em todas as suas subpastas.</span><span class="sxs-lookup"><span data-stu-id="6f03a-126">The compiler includes the directives specified in the view imports file in all of the Razor templates in the same folder and recursively in all of its subfolders.</span></span> <span data-ttu-id="6f03a-127">Portanto, uma *viewimports. cshtml* arquivo contendo `@layout MainLayout` garante que todos os componentes em uma pasta, use o *MainLayout* layout.</span><span class="sxs-lookup"><span data-stu-id="6f03a-127">Therefore, a *_ViewImports.cshtml* file containing `@layout MainLayout` ensures that all of the components in a folder use the *MainLayout* layout.</span></span> <span data-ttu-id="6f03a-128">Não é necessário adicionar repetidamente `@layout` a todos os  *\*. cshtml* arquivos.</span><span class="sxs-lookup"><span data-stu-id="6f03a-128">There's no need to repeatedly add `@layout` to all of the *\*.cshtml* files.</span></span>

<span data-ttu-id="6f03a-129">Observe que o modelo padrão usa a *viewimports. cshtml* mecanismo para seleção de layout.</span><span class="sxs-lookup"><span data-stu-id="6f03a-129">Note that the default template uses the *_ViewImports.cshtml* mechanism for layout selection.</span></span> <span data-ttu-id="6f03a-130">Um aplicativo recém-criado contém o *viewimports. cshtml* do arquivo na *páginas* pasta.</span><span class="sxs-lookup"><span data-stu-id="6f03a-130">A newly created app contains the *_ViewImports.cshtml* file in the *Pages* folder.</span></span>

## <a name="nested-layouts"></a><span data-ttu-id="6f03a-131">Layouts aninhados</span><span class="sxs-lookup"><span data-stu-id="6f03a-131">Nested layouts</span></span>

<span data-ttu-id="6f03a-132">Aplicativos podem consistir em layouts aninhados.</span><span class="sxs-lookup"><span data-stu-id="6f03a-132">Apps can consist of nested layouts.</span></span> <span data-ttu-id="6f03a-133">Um componente pode fazer referência a um layout que por sua vez faz referência a outro layout.</span><span class="sxs-lookup"><span data-stu-id="6f03a-133">A component can reference a layout which in turn references another layout.</span></span> <span data-ttu-id="6f03a-134">Por exemplo, os layouts de aninhamento podem ser usados para refletir uma estrutura de vários níveis de menu.</span><span class="sxs-lookup"><span data-stu-id="6f03a-134">For example, nesting layouts can be used to reflect a multi-level menu structure.</span></span>

<span data-ttu-id="6f03a-135">Os exemplos de código a seguir mostram como usar layouts aninhados.</span><span class="sxs-lookup"><span data-stu-id="6f03a-135">The following code samples show how to use nested layouts.</span></span> <span data-ttu-id="6f03a-136">O *CustomersComponent.cshtml* arquivo é o componente para exibir.</span><span class="sxs-lookup"><span data-stu-id="6f03a-136">The *CustomersComponent.cshtml* file is the component to display.</span></span> <span data-ttu-id="6f03a-137">Observe que o componente o referencia o layout `MasterDataLayout`.</span><span class="sxs-lookup"><span data-stu-id="6f03a-137">Note that the component references the layout `MasterDataLayout`.</span></span>

<span data-ttu-id="6f03a-138">*CustomersComponent.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6f03a-138">*CustomersComponent.cshtml*:</span></span>

```csharp
@layout MasterDataLayout

@page "/master-data/customers"

<h1>Customer Maintenance</h1>
...
```

<span data-ttu-id="6f03a-139">O *MasterDataLayout.cshtml* arquivo fornece o `MasterDataLayout`.</span><span class="sxs-lookup"><span data-stu-id="6f03a-139">The *MasterDataLayout.cshtml* file provides the `MasterDataLayout`.</span></span> <span data-ttu-id="6f03a-140">O layout faz referência a outro layout, `MainLayout`, onde vai a ser inserido.</span><span class="sxs-lookup"><span data-stu-id="6f03a-140">The layout references another layout, `MainLayout`, where it's going to be embedded.</span></span>

<span data-ttu-id="6f03a-141">*MasterDataLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6f03a-141">*MasterDataLayout.cshtml*:</span></span>

```csharp
@layout MainLayout
@inherits BlazorLayoutComponent

<nav>
    <!-- Menu structure of master data module -->
    ...
</nav>

@Body
```

<span data-ttu-id="6f03a-142">Por fim, `MainLayout` contém os elementos de layout de nível superior, como o cabeçalho, rodapé e menu principal.</span><span class="sxs-lookup"><span data-stu-id="6f03a-142">Finally, `MainLayout` contains the top-level layout elements, such as the header, footer, and main menu.</span></span>

<span data-ttu-id="6f03a-143">*MainLayout.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6f03a-143">*MainLayout.cshtml*:</span></span>

```csharp
@inherits BlazorLayoutComponent

<header>...</header>
<nav>...</nav>

@Body
```
