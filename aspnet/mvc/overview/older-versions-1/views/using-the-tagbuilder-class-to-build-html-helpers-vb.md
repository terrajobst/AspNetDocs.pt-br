---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: Usando a classe TagBuilder para criar auxiliares HTML (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther apresenta uma classe de utilitário útil no ASP.NET MVC Framework chamada The TagBuilder Class. Você pode usar a classe TagBuilder para facilmente...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 3b0aa9816209cc326d3dea4b8dfb1b13cf697fcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78599998"
---
# <a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a>Usando a classe TagBuilder para criar auxiliares HTML (VB)

por [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther apresenta uma classe de utilitário útil no ASP.NET MVC Framework chamada The TagBuilder Class. Você pode usar a classe TagBuilder para criar facilmente marcas HTML.

A estrutura MVC do ASP.NET inclui uma classe de utilitário útil denominada classe TagBuilder que você pode usar ao criar auxiliares HTML. A classe TagBuilder, como o nome da classe sugere, permite que você crie facilmente marcas HTML. Neste breve tutorial, você receberá uma visão geral da classe TagBuilder e aprenderá a usar essa classe ao criar um auxiliar HTML simples que processa as marcas HTML &lt;img&gt;.

## <a name="overview-of-the-tagbuilder-class"></a>Visão geral da classe TagBuilder

A classe TagBuilder está contida no namespace System. Web. Mvc. Ele tem cinco métodos:

- AddCssClass () – permite que você adicione um novo atributo *Class = ""* a uma marca.
- Gerarid () – permite que você adicione um atributo de ID a uma marca. Esse método substitui automaticamente os períodos na ID (por padrão, os períodos são substituídos por sublinhados)
- MergeAttribute () – permite que você adicione atributos a uma marca. Há várias sobrecargas desse método.
- Setinnertext () – permite que você defina o texto interno da marca. O texto interno é codificado automaticamente em HTML.
- ToString () – permite que você processe a marca. Você pode especificar se deseja criar uma marca normal, uma marca de início, uma marca de fim ou uma marca de fechamento automático.

A classe TagBuilder tem quatro propriedades importantes:

- Atributos – representa todos os atributos da marca.
- IdAttributeDotReplacement – representa o caractere usado pelo método generateId () para substituir períodos (o padrão é um sublinhado).
- InnerHTML – representa o conteúdo interno da marca. A atribuição de uma cadeia de caracteres a essa propriedade *não* codifica a cadeia de caracteres em HTML.
- TagName – representa o nome da marca.

Esses métodos e propriedades fornecem a você todos os métodos e propriedades básicos de que você precisa para criar uma marca HTML. Você não precisa realmente usar a classe TagBuilder. Em vez disso, você poderia usar uma classe StringBuilder. No entanto, a classe TagBuilder torna a sua vida um pouco mais fácil.

## <a name="creating-an-image-html-helper"></a>Criando um auxiliar HTML de imagem

Ao criar uma instância da classe TagBuilder, você passa o nome da marca que deseja criar para o Construtor TagBuilder. Em seguida, você pode chamar métodos como os métodos AddCssClass e MergeAttribute () para modificar os atributos da marca. Finalmente, você chama o método ToString () para renderizar a marca.

Por exemplo, a listagem 1 contém um auxiliar HTML de imagem. O auxiliar de imagem é implementado internamente com um TagBuilder que representa uma marcação HTML &lt;img&gt;.

**Listagem 1 – Helpers\ImageHelper.vb**

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

O módulo na Listagem 1 contém dois métodos sobrecarregados chamados Image (). Ao chamar o método Image (), você pode passar um objeto que representa um conjunto de atributos HTML ou não.

Observe como o método TagBuilder. MergeAttribute () é usado para adicionar atributos individuais, como o atributo src ao TagBuilder. Observe, além disso, como o método TagBuilder. Mergeattributes () é usado para adicionar uma coleção de atributos ao TagBuilder. O método Mergeattributes () aceita um dicionário&lt;cadeia de caracteres, objeto&gt; parâmetro. A classe RouteValueDictionary é usada para converter o objeto que representa a coleção de atributos em um dicionário&lt;cadeia de caracteres,&gt;de objeto.

Depois de criar o auxiliar de imagem, você pode usar o auxiliar em suas exibições do ASP.NET MVC, assim como qualquer um dos outros auxiliares HTML padrão. A exibição na Listagem 2 usa o auxiliar de imagem para exibir a mesma imagem de um Xbox duas vezes (consulte a Figura 1). O auxiliar Image () é chamado com e sem uma coleção de atributos HTML.

**Listagem 2 – Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]

[![caixa de diálogo novo projeto](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)

**Figura 01**: usando o auxiliar de imagem ([clique para exibir a imagem em tamanho normal](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))

Observe que você deve importar o namespace associado ao auxiliar de imagem na parte superior da exibição index. aspx. O auxiliar é importado com a seguinte diretiva:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

Em um aplicativo Visual Basic, o namespace padrão é o mesmo que o nome do aplicativo.

> [!div class="step-by-step"]
> [Anterior](creating-custom-html-helpers-vb.md)
> [Próximo](creating-page-layouts-with-view-master-pages-vb.md)
