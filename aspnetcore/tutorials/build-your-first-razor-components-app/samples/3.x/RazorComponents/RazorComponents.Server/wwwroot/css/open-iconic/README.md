---
ms.openlocfilehash: 3965884c8e0d7116f5d8a42e6aefb0dc5be94f1e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57064843"
---
<a name="open-iconic-v111httpuseiconiccomopen"></a>[<span data-ttu-id="cd93a-101">Open Iconic v1.1.1</span><span class="sxs-lookup"><span data-stu-id="cd93a-101">Open Iconic v1.1.1</span></span>](http://useiconic.com/open)
===========

### <a name="open-iconic-is-the-open-source-sibling-of-iconichttpuseiconiccom-it-is-a-hyper-legible-collection-of-223-icons-with-a-tiny-footprintmdashready-to-use-with-bootstrap-and-foundation-view-the-collectionhttpuseiconiccomopenicons"></a><span data-ttu-id="cd93a-102">O Open Iconic é o irmão de software livre do [Iconic](http://useiconic.com).</span><span class="sxs-lookup"><span data-stu-id="cd93a-102">Open Iconic is the open source sibling of [Iconic](http://useiconic.com).</span></span> <span data-ttu-id="cd93a-103">Trata-se de uma coleção hiperlegível de 223 ícones com um volume muito pequeno &mdash;pronto para uso com o Bootstrap e o Foundation.</span><span class="sxs-lookup"><span data-stu-id="cd93a-103">It is a hyper-legible collection of 223 icons with a tiny footprint&mdash;ready to use with Bootstrap and Foundation.</span></span> [<span data-ttu-id="cd93a-104">Exibir a coleção</span><span class="sxs-lookup"><span data-stu-id="cd93a-104">View the collection</span></span>](http://useiconic.com/open#icons)



## <a name="whats-in-open-iconic"></a><span data-ttu-id="cd93a-105">O que é Open Iconic?</span><span class="sxs-lookup"><span data-stu-id="cd93a-105">What's in Open Iconic?</span></span>

* <span data-ttu-id="cd93a-106">223 ícones projetados para serem legíveis até 8 pixels</span><span class="sxs-lookup"><span data-stu-id="cd93a-106">223 icons designed to be legible down to 8 pixels</span></span>
* <span data-ttu-id="cd93a-107">Arquivos SVG superleves: 61,8 para o conjunto inteiro</span><span class="sxs-lookup"><span data-stu-id="cd93a-107">Super-light SVG files - 61.8 for the entire set</span></span> 
* <span data-ttu-id="cd93a-108">SVG Sprite&mdash;a substituição moderna para fontes de ícone</span><span class="sxs-lookup"><span data-stu-id="cd93a-108">SVG sprite&mdash;the modern replacement for icon fonts</span></span>
* <span data-ttu-id="cd93a-109">Formatos de fonte da Web (EOT, OTF, SVG, TTF, WOFF), PNG e WebP</span><span class="sxs-lookup"><span data-stu-id="cd93a-109">Webfont (EOT, OTF, SVG, TTF, WOFF), PNG and WebP formats</span></span>
* <span data-ttu-id="cd93a-110">Folhas de estilos de fonte da Web (incluindo versões para Bootstrap e Foundation) em formatos CSS, LESS, SCSS e Stylus</span><span class="sxs-lookup"><span data-stu-id="cd93a-110">Webfont stylesheets (including versions for Bootstrap and Foundation) in CSS, LESS, SCSS and Stylus formats</span></span>
* <span data-ttu-id="cd93a-111">Imagens de varredura PNG e WebP em 8, 16, 24, 32, 48 e 64 pixels.</span><span class="sxs-lookup"><span data-stu-id="cd93a-111">PNG and WebP raster images in 8px, 16px, 24px, 32px, 48px and 64px.</span></span>


## <a name="getting-started"></a><span data-ttu-id="cd93a-112">Guia de Introdução</span><span class="sxs-lookup"><span data-stu-id="cd93a-112">Getting Started</span></span>

#### <a name="for-code-samples-and-everything-else-you-need-to-get-started-with-open-iconic-check-out-our-iconshttpuseiconiccomopenicons-and-referencehttpuseiconiccomopenreference-sections"></a><span data-ttu-id="cd93a-113">Para obter exemplos de código e tudo o que é preciso para começar a usar o Open Iconic, confira nossas seções [Ícones](http://useiconic.com/open#icons) e [Referência](http://useiconic.com/open#reference).</span><span class="sxs-lookup"><span data-stu-id="cd93a-113">For code samples and everything else you need to get started with Open Iconic, check out our [Icons](http://useiconic.com/open#icons) and [Reference](http://useiconic.com/open#reference) sections.</span></span>

### <a name="general-usage"></a><span data-ttu-id="cd93a-114">Uso geral</span><span class="sxs-lookup"><span data-stu-id="cd93a-114">General Usage</span></span>

#### <a name="using-open-iconics-svgs"></a><span data-ttu-id="cd93a-115">Usando SVGs do Open Iconic</span><span class="sxs-lookup"><span data-stu-id="cd93a-115">Using Open Iconic's SVGs</span></span>

<span data-ttu-id="cd93a-116">Gostamos de SVGs e acreditamos que eles são a maneira ideal de exibir ícones na Web.</span><span class="sxs-lookup"><span data-stu-id="cd93a-116">We like SVGs and we think they're the way to display icons on the web.</span></span> <span data-ttu-id="cd93a-117">Uma vez que o Open Iconic representa apenas SVGs básicos, sugerimos exibi-los como qualquer outra imagem (lembre-se do atributo `alt`).</span><span class="sxs-lookup"><span data-stu-id="cd93a-117">Since Open Iconic are just basic SVGs, we suggest you display them like you would any other image (don't forget the `alt` attribute).</span></span>

```
<img src="/open-iconic/svg/icon-name.svg" alt="icon name">
```

#### <a name="using-open-iconics-svg-sprite"></a><span data-ttu-id="cd93a-118">Usando SVG Sprite do Open Iconic</span><span class="sxs-lookup"><span data-stu-id="cd93a-118">Using Open Iconic's SVG Sprite</span></span>

<span data-ttu-id="cd93a-119">O Open Iconic também é apresentado em um SVG Sprite, que permite exibir todos os ícones no conjunto com uma única solicitação.</span><span class="sxs-lookup"><span data-stu-id="cd93a-119">Open Iconic also comes in a SVG sprite which allows you to display all the icons in the set with a single request.</span></span> <span data-ttu-id="cd93a-120">É como uma fonte de ícone, sem ser um hack.</span><span class="sxs-lookup"><span data-stu-id="cd93a-120">It's like an icon font, without being a hack.</span></span>

<span data-ttu-id="cd93a-121">Adicionar um ícone usando um SVG Sprite é um pouco diferente do que você está acostumado, mas ainda assim, uma tarefa bem fácil.</span><span class="sxs-lookup"><span data-stu-id="cd93a-121">Adding an icon from an SVG sprite is a little different than what you're used to, but it's still a piece of cake.</span></span> <span data-ttu-id="cd93a-122">*Dica: para tornar seus ícones facilmente aptos à estilização, sugerimos a adição de uma classe geral à marca* `<svg>` *e um nome de classe exclusivo para cada ícone diferente na marca* `<use>` *.*</span><span class="sxs-lookup"><span data-stu-id="cd93a-122">*Tip: To make your icons easily style able, we suggest adding a general class to the* `<svg>` *tag and a unique class name for each different icon in the* `<use>` *tag.*</span></span>  

```
<svg class="icon">
  <use xlink:href="open-iconic.svg#account-login" class="icon-account-login"></use>
</svg>
```

<span data-ttu-id="cd93a-123">O dimensionamento de ícones precisa apenas de CSS básico.</span><span class="sxs-lookup"><span data-stu-id="cd93a-123">Sizing icons only needs basic CSS.</span></span> <span data-ttu-id="cd93a-124">Todos os ícones estão em um formato quadrado, portanto, basta definir a marca `<svg>` com dimensões iguais de largura e altura.</span><span class="sxs-lookup"><span data-stu-id="cd93a-124">All the icons are in a square format, so just set the `<svg>` tag with equal width and height dimensions.</span></span>

```
.icon {
  width: 16px;
  height: 16px;
}
```

<span data-ttu-id="cd93a-125">Colorir ícones é ainda mais fácil.</span><span class="sxs-lookup"><span data-stu-id="cd93a-125">Coloring icons is even easier.</span></span> <span data-ttu-id="cd93a-126">Tudo o que você precisa fazer é definir a regra `fill` na marca `<use>`.</span><span class="sxs-lookup"><span data-stu-id="cd93a-126">All you need to do is set the `fill` rule on the `<use>` tag.</span></span>

```
.icon-account-login {
  fill: #f00;
}
```

<span data-ttu-id="cd93a-127">Para saber mais sobre SVG Sprites, leia o [Guia de Chris Coyier](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span><span class="sxs-lookup"><span data-stu-id="cd93a-127">To learn more about SVG Sprites, read [Chris Coyier's guide](http://css-tricks.com/svg-sprites-use-better-icon-fonts/).</span></span>

#### <a name="using-open-iconics-icon-font"></a><span data-ttu-id="cd93a-128">Usando a Fonte de Ícone do Open Iconic...</span><span class="sxs-lookup"><span data-stu-id="cd93a-128">Using Open Iconic's Icon Font...</span></span>


##### <a name="with-bootstrap"></a><span data-ttu-id="cd93a-129">... com o Bootstrap</span><span class="sxs-lookup"><span data-stu-id="cd93a-129">…with Bootstrap</span></span>

<span data-ttu-id="cd93a-130">Você pode encontrar nossas folhas de estilo do Bootstrap em `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="cd93a-130">You can find our Bootstrap stylesheets in `font/css/open-iconic-bootstrap.{css, less, scss, styl}`</span></span>


```
<link href="/open-iconic/font/css/open-iconic-bootstrap.css" rel="stylesheet">
```


```
<span class="oi oi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="with-foundation"></a><span data-ttu-id="cd93a-131">… com o Foundation</span><span class="sxs-lookup"><span data-stu-id="cd93a-131">…with Foundation</span></span>

<span data-ttu-id="cd93a-132">Você pode encontrar nossas folhas de estilo do Foundation em `font/css/open-iconic-foundation.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="cd93a-132">You can find our Foundation stylesheets in `font/css/open-iconic-foundation.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic-foundation.css" rel="stylesheet">
```


```
<span class="fi-icon-name" title="icon name" aria-hidden="true"></span>
```

##### <a name="on-its-own"></a><span data-ttu-id="cd93a-133">... por conta própria</span><span class="sxs-lookup"><span data-stu-id="cd93a-133">…on its own</span></span>

<span data-ttu-id="cd93a-134">Você pode encontrar nossas folhas de estilo padrão em `font/css/open-iconic.{css, less, scss, styl}`</span><span class="sxs-lookup"><span data-stu-id="cd93a-134">You can find our default stylesheets in `font/css/open-iconic.{css, less, scss, styl}`</span></span>

```
<link href="/open-iconic/font/css/open-iconic.css" rel="stylesheet">
```

```
<span class="oi" data-glyph="icon-name" title="icon name" aria-hidden="true"></span>
```


## <a name="license"></a><span data-ttu-id="cd93a-135">Licença</span><span class="sxs-lookup"><span data-stu-id="cd93a-135">License</span></span>

### <a name="icons"></a><span data-ttu-id="cd93a-136">Ícones</span><span class="sxs-lookup"><span data-stu-id="cd93a-136">Icons</span></span>

<span data-ttu-id="cd93a-137">Todo o código (inclusive a marcação SVG) está sob a [Licença MIT](http://opensource.org/licenses/MIT).</span><span class="sxs-lookup"><span data-stu-id="cd93a-137">All code (including SVG markup) is under the [MIT License](http://opensource.org/licenses/MIT).</span></span>

### <a name="fonts"></a><span data-ttu-id="cd93a-138">Fontes</span><span class="sxs-lookup"><span data-stu-id="cd93a-138">Fonts</span></span>

<span data-ttu-id="cd93a-139">Todas as fontes estão sob a [Licença SIL](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span><span class="sxs-lookup"><span data-stu-id="cd93a-139">All fonts are under the [SIL Licensed](http://scripts.sil.org/cms/scripts/page.php?item_id=OFL_web).</span></span>
