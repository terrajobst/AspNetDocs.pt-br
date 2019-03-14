---
ms.openlocfilehash: 610b16b181422978dc143b95f28658bbdf177fdd
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57028273"
---
# <a name="jquery"></a><span data-ttu-id="1f9d2-101">jQuery</span><span class="sxs-lookup"><span data-stu-id="1f9d2-101">jQuery</span></span>

> <span data-ttu-id="1f9d2-102">O jQuery é uma biblioteca JavaScript rápida, pequena e rica em recursos.</span><span class="sxs-lookup"><span data-stu-id="1f9d2-102">jQuery is a fast, small, and feature-rich JavaScript library.</span></span>

<span data-ttu-id="1f9d2-103">Para obter informações sobre como começar a usar o jQuery, confira [Documentação do jQuery](http://api.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="1f9d2-103">For information on how to get started and how to use jQuery, please see [jQuery's documentation](http://api.jquery.com/).</span></span>
<span data-ttu-id="1f9d2-104">Para os problemas e arquivos de origem, acesse o [repositório jQuery](https://github.com/jquery/jquery).</span><span class="sxs-lookup"><span data-stu-id="1f9d2-104">For source files and issues, please visit the [jQuery repo](https://github.com/jquery/jquery).</span></span>

<span data-ttu-id="1f9d2-105">Na caso de uma atualização, confira a [postagem no blog para 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/).</span><span class="sxs-lookup"><span data-stu-id="1f9d2-105">If upgrading, please see the [blog post for 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/).</span></span> <span data-ttu-id="1f9d2-106">Isso inclui diferenças notáveis ​​da versão anterior e um log de alterações mais legível.</span><span class="sxs-lookup"><span data-stu-id="1f9d2-106">This includes notable differences from the previous version and a more readable changelog.</span></span>

## <a name="including-jquery"></a><span data-ttu-id="1f9d2-107">Inclusão do jQuery</span><span class="sxs-lookup"><span data-stu-id="1f9d2-107">Including jQuery</span></span>

<span data-ttu-id="1f9d2-108">Abaixo estão algumas das maneiras mais comuns de incluir o jQuery.</span><span class="sxs-lookup"><span data-stu-id="1f9d2-108">Below are some of the most common ways to include jQuery.</span></span>

### <a name="browser"></a><span data-ttu-id="1f9d2-109">Navegador</span><span class="sxs-lookup"><span data-stu-id="1f9d2-109">Browser</span></span>

#### <a name="script-tag"></a><span data-ttu-id="1f9d2-110">Tag de script</span><span class="sxs-lookup"><span data-stu-id="1f9d2-110">Script tag</span></span>

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
```

#### <a name="babel"></a><span data-ttu-id="1f9d2-111">Babel</span><span class="sxs-lookup"><span data-stu-id="1f9d2-111">Babel</span></span>

<span data-ttu-id="1f9d2-112">[Babel](http://babeljs.io/) é um compilador JavaScript de próxima geração.</span><span class="sxs-lookup"><span data-stu-id="1f9d2-112">[Babel](http://babeljs.io/) is a next generation JavaScript compiler.</span></span> <span data-ttu-id="1f9d2-113">Um dos recursos é a capacidade de usar os módulos ES6/ES2015 agora, embora os navegadores ainda não ofereçam suporte a esse recurso nativamente.</span><span class="sxs-lookup"><span data-stu-id="1f9d2-113">One of the features is the ability to use ES6/ES2015 modules now, even though browsers do not yet support this feature natively.</span></span>

```js
import $ from "jquery";
```

#### <a name="browserifywebpack"></a><span data-ttu-id="1f9d2-114">Browserify/Webpack</span><span class="sxs-lookup"><span data-stu-id="1f9d2-114">Browserify/Webpack</span></span>

<span data-ttu-id="1f9d2-115">Há várias maneiras de usar o [Browserify](http://browserify.org/) e [Webpack](https://webpack.github.io/).</span><span class="sxs-lookup"><span data-stu-id="1f9d2-115">There are several ways to use [Browserify](http://browserify.org/) and [Webpack](https://webpack.github.io/).</span></span> <span data-ttu-id="1f9d2-116">Para obter mais informações sobre o uso dessas ferramentas, consulte a documentação do projeto correspondente.</span><span class="sxs-lookup"><span data-stu-id="1f9d2-116">For more information on using these tools, please refer to the corresponding project's documention.</span></span> <span data-ttu-id="1f9d2-117">No script, a inclusão do jQuery geralmente ficará assim...</span><span class="sxs-lookup"><span data-stu-id="1f9d2-117">In the script, including jQuery will usually look like this...</span></span>

```js
var $ = require("jquery");
```

#### <a name="amd-asynchronous-module-definition"></a><span data-ttu-id="1f9d2-118">AMD (Definição de Módulo Assíncrono)</span><span class="sxs-lookup"><span data-stu-id="1f9d2-118">AMD (Asynchronous Module Definition)</span></span>

<span data-ttu-id="1f9d2-119">AMD é um formato de módulo compilado para o navegador.</span><span class="sxs-lookup"><span data-stu-id="1f9d2-119">AMD is a module format built for the browser.</span></span> <span data-ttu-id="1f9d2-120">Para obter mais informações, recomendamos a [documentação de require.js'](http://requirejs.org/docs/whyamd.html).</span><span class="sxs-lookup"><span data-stu-id="1f9d2-120">For more information, we recommend [require.js' documentation](http://requirejs.org/docs/whyamd.html).</span></span>

```js
define(["jquery"], function($) {

});
```

### <a name="node"></a><span data-ttu-id="1f9d2-121">Nó</span><span class="sxs-lookup"><span data-stu-id="1f9d2-121">Node</span></span>

<span data-ttu-id="1f9d2-122">Para incluir o jQuery no [Node](nodejs.org), primeiro instale com npm.</span><span class="sxs-lookup"><span data-stu-id="1f9d2-122">To include jQuery in [Node](nodejs.org), first install with npm.</span></span>

```sh
npm install jquery
```

<span data-ttu-id="1f9d2-123">Para o jQuery funcionar no Node, é necessária uma janela com um documento.</span><span class="sxs-lookup"><span data-stu-id="1f9d2-123">For jQuery to work in Node, a window with a document is required.</span></span> <span data-ttu-id="1f9d2-124">Como não existem janelas nativamente no Node, uma pode ser simulada por ferramentas como [jsdom](https://github.com/tmpvar/jsdom).</span><span class="sxs-lookup"><span data-stu-id="1f9d2-124">Since no such window exists natively in Node, one can be mocked by tools such as [jsdom](https://github.com/tmpvar/jsdom).</span></span> <span data-ttu-id="1f9d2-125">Isso pode ser útil para fins de teste.</span><span class="sxs-lookup"><span data-stu-id="1f9d2-125">This can be useful for testing purposes.</span></span>

```js
require("jsdom").env("", function(err, window) {
    if (err) {
        console.error(err);
        return;
    }

    var $ = require("jquery")(window);
});
```
