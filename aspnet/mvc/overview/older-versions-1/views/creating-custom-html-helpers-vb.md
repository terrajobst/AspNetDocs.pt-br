---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Criando auxiliares HTML personalizados (VB) | Microsoft Docs
author: microsoft
description: O objetivo deste tutorial é demonstrar como você pode criar auxiliares HTML personalizados que podem ser usados em suas exibições do MVC. Aproveitando o auxiliar HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: aaeadde258a2855343a5bfb1e5ee76000e04f6bd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600271"
---
# <a name="creating-custom-html-helpers-vb"></a>Criação de auxiliares de HTML personalizados (VB)

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> O objetivo deste tutorial é demonstrar como você pode criar auxiliares HTML personalizados que podem ser usados em suas exibições do MVC. Aproveitando os auxiliares HTML, você pode reduzir a quantidade de digitação entediante de marcas HTML que você deve executar para criar uma página HTML padrão.

O objetivo deste tutorial é demonstrar como você pode criar auxiliares HTML personalizados que podem ser usados em suas exibições do MVC. Aproveitando os auxiliares HTML, você pode reduzir a quantidade de digitação entediante de marcas HTML que você deve executar para criar uma página HTML padrão.

Na primeira parte deste tutorial, descrevo alguns dos auxiliares HTML existentes incluídos na estrutura MVC do ASP.NET. Em seguida, descrevo dois métodos de criação de auxiliares HTML personalizados: Eu explico como criar auxiliares HTML personalizados criando um método compartilhado e criando um método de extensão.

## <a name="understanding-html-helpers"></a>Noções básicas sobre auxiliares HTML

Um auxiliar HTML é apenas um método que retorna uma cadeia de caracteres. A cadeia de caracteres pode representar qualquer tipo de conteúdo desejado. Por exemplo, você pode usar os auxiliares HTML para renderizar marcas HTML padrão, como HTML `<input>` e marcas de `<img>`. Você também pode usar os auxiliares HTML para renderizar um conteúdo mais complexo, como uma faixa de guias ou uma tabela HTML de dados de Database.

O ASP.NET MVC Framework inclui o seguinte conjunto de auxiliares HTML padrão (esta não é uma lista completa):

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

Por exemplo, considere o formulário na Listagem 1. Esse formulário é renderizado com a ajuda de dois dos auxiliares HTML padrão (consulte a Figura 1). Esse formulário usa os métodos auxiliares `Html.BeginForm()` e `Html.TextBox()`.

[![página renderizada com auxiliares HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)

**Figura 01**: página renderizada com auxiliares HTML ([clique para exibir a imagem em tamanho normal](creating-custom-html-helpers-vb/_static/image3.png))

**Listagem 1 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

O método auxiliar `Html.BeginForm()` é usado para criar as marcas de `<form>` HTML de abertura e fechamento. Observe que o método `Html.BeginForm()` é chamado dentro de uma instrução using. A instrução using garante que a marca de `<form>` seja fechada no final do bloco Using.

Se preferir, em vez de criar um bloco Using, você pode chamar o método auxiliar HTML. EndForm () para fechar a marca de `<form>`. Use qualquer abordagem para criar uma marca de `<form>` de abertura e fechamento que pareça mais intuitiva para você.

Os métodos auxiliares `Html.TextBox()` são usados na Listagem 1 para processar marcas HTML `<input>`. Se você selecionar Exibir origem em seu navegador, verá a fonte HTML na Listagem 2. Observe que a origem contém marcas HTML padrão.

> [!IMPORTANT]
> Observe que o auxiliar `Html.TextBox()`-HTML é renderizado com `<%= %>` marcas em vez de `<% %>` marcas. Se você não incluir o sinal de igual, nada será renderizado para o navegador.

O ASP.NET MVC Framework contém um pequeno conjunto de auxiliares. Provavelmente, será necessário estender a estrutura MVC com auxiliares HTML personalizados. No restante deste tutorial, você aprenderá dois métodos de criação de auxiliares HTML personalizados.

**Listagem 2 – `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a>Criando auxiliares HTML com métodos compartilhados

A maneira mais fácil de criar um novo auxiliar HTML é criar um método compartilhado que retorne uma cadeia de caracteres. Imagine, por exemplo, que você decida criar um novo auxiliar HTML que renderiza uma marcação HTML `<label>`. Você pode usar a classe na Listagem 2 para renderizar um `<label>`.

**Listagem 2 – `Helpers\LabelHelper.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

Não há nada de especial sobre a classe na Listagem 2. O método `Label()` simplesmente retorna uma cadeia de caracteres.

A exibição de índice modificada na Listagem 3 usa o `LabelHelper` para processar marcas HTML `<label>`. Observe que a exibição inclui uma diretiva `<%@ imports %>` que importa o namespace Application1. Helpers.

**Listagem 2 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Criando auxiliares HTML com métodos de extensão

Se você quiser criar auxiliares HTML que funcionam exatamente como os auxiliares HTML padrão incluídos no ASP.NET MVC Framework, você precisará criar métodos de extensão. Os métodos de extensão permitem adicionar novos métodos a uma classe existente. Ao criar um método auxiliar HTML, você adiciona novos métodos à classe `HtmlHelper` representada pela propriedade HTML de uma exibição.

O módulo Visual Basic na Listagem 3 adiciona um método de extensão chamado `Label()` à classe `HtmlHelper`. Há algumas coisas que você deve observar sobre esse módulo. Primeiro, observe que o módulo está decorado com o atributo `<Extension()>`. Para usar esse atributo, você deve importar o namespace `System.Runtime.CompilerServices`

Em segundo lugar, observe que o primeiro parâmetro do método `Label()` representa a classe `HtmlHelper`. O primeiro parâmetro de um método de extensão indica a classe que o método de extensão estende.

**Listagem 3 – `Helpers\LabelExtensions.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

Depois de criar um método de extensão e criar seu aplicativo com êxito, o método de extensão aparecerá no Visual Studio IntelliSense como todos os outros métodos de uma classe (consulte a Figura 2). A única diferença é que os métodos de extensão aparecem com um símbolo especial ao lado deles (um ícone de seta para baixo).

[![usando o método de extensão HTML. Label ()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)

**Figura 02**: usando o método de extensão HTML. Label () ([clique para exibir a imagem em tamanho normal](creating-custom-html-helpers-vb/_static/image6.png))

A exibição de índice modificada na Listagem 4 usa o método de extensão HTML. Label () para renderizar todas as suas &lt;rótulo&gt; marcas.

**Listagem 4 – `Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu dois métodos de criação de auxiliares HTML personalizados. Primeiro, você aprendeu a criar um auxiliar HTML personalizado `Label()` criando um método compartilhado que retorna uma cadeia de caracteres. Em seguida, você aprendeu a criar um método auxiliar HTML `Label()` personalizado criando um método de extensão na classe `HtmlHelper`.

Neste tutorial, eu me deparava com a criação de um método auxiliar HTML extremamente simples. Perceba que um auxiliar HTML pode ser tão complicado quanto você desejar. Você pode criar auxiliares HTML que processam conteúdo rico, como modos de exibição de árvore, menus ou tabelas de dados de banco.

> [!div class="step-by-step"]
> [Anterior](asp-net-mvc-views-overview-vb.md)
> [Próximo](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
