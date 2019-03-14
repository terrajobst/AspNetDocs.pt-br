---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57054213"
---
<a name="open-iconic-v111httpuseiconiccomopen"></a>[Open Iconic v1.1.1](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconichttpuseiconiccom-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collectionhttpuseiconiccomopenicons"></a>O Open Iconic é o irmão de software livre do [Iconic](http://useiconic.com). Trata-se de uma coleção hiperlegível de 223 ícones com um volume muito pequeno &mdash;pronto para uso com o Bootstrap e o Foundation. [Exibir a coleção](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a>O que é Open Iconic?

* 223 ícones projetados para serem legíveis até 8 pixels
* Arquivos SVG superleves: 61,8 para o conjunto inteiro 
* SVG Sprite&mdash;a substituição moderna para fontes de ícone
* Formatos de fonte da Web (EOT, OTF, SVG, TTF, WOFF), PNG e WebP
* Folhas de estilos de fonte da Web (incluindo versões para Bootstrap e Foundation) em formatos CSS, LESS, SCSS e Stylus
* Imagens de varredura PNG e WebP em 8, 16, 24, 32, 48 e 64 pixels.


## <a name="getting-started"></a>Guia de Introdução

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-iconshttpuseiconiccomopenicons-and-referencehttpuseiconiccomopenreference-sections"></a>Para obter exemplos de código e tudo o que é preciso para começar a usar o Open Iconic, confira nossas seções [Ícones](http://useiconic.com/open#icons) e [Referência](http://useiconic.com/open#reference).

### <a name="general-usage"></a>Uso geral

#### <a name="using-open-iconics-svgs"></a>Usando SVGs do Open Iconic

Gostamos de SVGs e acreditamos que eles são a maneira ideal de exibir ícones na Web. Uma vez que o Open Iconic representa apenas SVGs básicos, sugerimos exibi-los como qualquer outra imagem (lembre-se do atributo `alt`).

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a>Usando SVG Sprite do Open Iconic

O Open Iconic também é apresentado em um SVG Sprite, que permite exibir todos os ícones no conjunto com uma única solicitação. É como uma fonte de ícone, sem ser um hack.

Adicionar um ícone usando um SVG Sprite é um pouco diferente do que você está acostumado, mas ainda assim, uma tarefa bem fácil. *Dica: para tornar seus ícones facilmente aptos à estilização, sugerimos a adição de uma classe geral à marca* `<svg>` *e um nome de classe exclusivo para cada ícone diferente na marca* `<use>` *.*  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

O dimensionamento de ícones precisa apenas de CSS básico. Todos os ícones estão em um formato quadrado, portanto, basta definir a marca `<svg>` com dimensões iguais de largura e altura.

```
.icon {
  width: 16px;
  height: 16px;
}
```

Colorir ícones é ainda mais fácil. Tudo o que você precisa fazer é definir a regra `fill` na marca `<use>`.

```
.icon-account-login {
  fill: #f00;
}
```

Para saber mais sobre SVG Sprites, leia o [Guia de Chris Coyier](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).

#### <a name="using-open-iconics-icon-font"></a>Usando a Fonte de Ícone do Open Iconic...


##### <a name="with-bootstrap"></a>... com o Bootstrap

Você pode encontrar nossas folhas de estilo do Bootstrap em `font/css/open-iconic-bootstrap.{css, less, scss, styl}`


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a>… com o Foundation

Você pode encontrar nossas folhas de estilo do Foundation em `font/css/open-iconic-foundation.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a>... por conta própria

Você pode encontrar nossas folhas de estilo padrão em `font/css/open-iconic.{css, less, scss, styl}`

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a>Licença

### <a name="icons"></a>Ícones

Todo o código (inclusive a marcação SVG) está sob a [Licença MIT](http://opensource.org/licenses/MIT).

### <a name="fonts"></a>Fontes

Todas as fontes estão sob a [Licença SIL](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).
