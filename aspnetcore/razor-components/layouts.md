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
# <a name="razor-components-layouts"></a>Layouts de componentes do Razor

Por [Rainer Stropek](https://www.timecockpit.com)

Os aplicativos normalmente contêm mais de uma página. Elementos de layout, como menus, mensagens de direitos autorais e logotipos, devem estar presentes em todas as páginas. Copiando o código desses elementos de layout para todas as páginas de um aplicativo não é uma solução eficiente. Essa duplicação é difícil de manter e provavelmente leva ao conteúdo inconsistente ao longo do tempo. *Layouts* resolver esse problema.

Tecnicamente, um layout é apenas outro componente. Um layout é definido em um modelo Razor ou em C# de código e pode conter outros recursos comuns de componentes, injeção de dependência e vinculação de dados. Dois aspectos adicionais ativar uma *componente* em um *layout*:

* O componente de layout deve herdar de `BlazorLayoutComponent`. `BlazorLayoutComponent` define um `Body` propriedade que contém o conteúdo a ser renderizado dentro do layout.
* O componente de layout usa o `Body` propriedade para especificar onde o conteúdo do corpo deve ser renderizado usando a sintaxe do Razor `@Body`. Durante o processamento, `@Body` é substituída pelo conteúdo do layout.

O exemplo de código a seguir mostra o modelo do Razor de um componente de layout. Observe o uso de `BlazorLayoutComponent` e `@Body`:

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

## <a name="use-a-layout-in-a-component"></a>Usar um layout em um componente

Use a diretiva do Razor `@layout` para aplicar um layout para um componente. O compilador converte essa diretiva em um `LayoutAttribute`, que é aplicado à classe de componente.

O exemplo de código a seguir demonstra o conceito. O conteúdo desse componente é inserido na *MasterLayout* na posição do `@Body`:

```csharp
@layout MasterLayout

@page "/master-data"

<h2>Master Data Management</h2>
...
```

## <a name="centralized-layout-selection"></a>Seleção de layout centralizado

Todas as pastas de um um aplicativo pode, opcionalmente, conter um arquivo de modelo chamado *viewimports. cshtml*. O compilador incluirá as diretivas especificadas no arquivo de importações de exibição em todos os modelos do Razor na mesma pasta e recursivamente em todas as suas subpastas. Portanto, uma *viewimports. cshtml* arquivo contendo `@layout MainLayout` garante que todos os componentes em uma pasta, use o *MainLayout* layout. Não é necessário adicionar repetidamente `@layout` a todos os  *\*. cshtml* arquivos.

Observe que o modelo padrão usa a *viewimports. cshtml* mecanismo para seleção de layout. Um aplicativo recém-criado contém o *viewimports. cshtml* do arquivo na *páginas* pasta.

## <a name="nested-layouts"></a>Layouts aninhados

Aplicativos podem consistir em layouts aninhados. Um componente pode fazer referência a um layout que por sua vez faz referência a outro layout. Por exemplo, os layouts de aninhamento podem ser usados para refletir uma estrutura de vários níveis de menu.

Os exemplos de código a seguir mostram como usar layouts aninhados. O *CustomersComponent.cshtml* arquivo é o componente para exibir. Observe que o componente o referencia o layout `MasterDataLayout`.

*CustomersComponent.cshtml*:

```csharp
@layout MasterDataLayout

@page "/master-data/customers"

<h1>Customer Maintenance</h1>
...
```

O *MasterDataLayout.cshtml* arquivo fornece o `MasterDataLayout`. O layout faz referência a outro layout, `MainLayout`, onde vai a ser inserido.

*MasterDataLayout.cshtml*:

```csharp
@layout MainLayout
@inherits BlazorLayoutComponent

<nav>
    <!-- Menu structure of master data module -->
    ...
</nav>

@Body
```

Por fim, `MainLayout` contém os elementos de layout de nível superior, como o cabeçalho, rodapé e menu principal.

*MainLayout.cshtml*:

```csharp
@inherits BlazorLayoutComponent

<header>...</header>
<nav>...</nav>

@Body
```
