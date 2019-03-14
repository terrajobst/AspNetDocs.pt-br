---
ms.openlocfilehash: 610b16b181422978dc143b95f28658bbdf177fdd
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57028273"
---
# <a name="jquery"></a>jQuery

> O jQuery é uma biblioteca JavaScript rápida, pequena e rica em recursos.

Para obter informações sobre como começar a usar o jQuery, confira [Documentação do jQuery](http://api.jquery.com/).
Para os problemas e arquivos de origem, acesse o [repositório jQuery](https://github.com/jquery/jquery).

Na caso de uma atualização, confira a [postagem no blog para 3.3.1](https://blog.jquery.com/2017/03/20/jquery-3.3.1-now-available/). Isso inclui diferenças notáveis ​​da versão anterior e um log de alterações mais legível.

## <a name="including-jquery"></a>Inclusão do jQuery

Abaixo estão algumas das maneiras mais comuns de incluir o jQuery.

### <a name="browser"></a>Navegador

#### <a name="script-tag"></a>Tag de script

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
```

#### <a name="babel"></a>Babel

[Babel](http://babeljs.io/) é um compilador JavaScript de próxima geração. Um dos recursos é a capacidade de usar os módulos ES6/ES2015 agora, embora os navegadores ainda não ofereçam suporte a esse recurso nativamente.

```js
import $ from "jquery";
```

#### <a name="browserifywebpack"></a>Browserify/Webpack

Há várias maneiras de usar o [Browserify](http://browserify.org/) e [Webpack](https://webpack.github.io/). Para obter mais informações sobre o uso dessas ferramentas, consulte a documentação do projeto correspondente. No script, a inclusão do jQuery geralmente ficará assim...

```js
var $ = require("jquery");
```

#### <a name="amd-asynchronous-module-definition"></a>AMD (Definição de Módulo Assíncrono)

AMD é um formato de módulo compilado para o navegador. Para obter mais informações, recomendamos a [documentação de require.js'](http://requirejs.org/docs/whyamd.html).

```js
define(["jquery"], function($) {

});
```

### <a name="node"></a>Nó

Para incluir o jQuery no [Node](nodejs.org), primeiro instale com npm.

```sh
npm install jquery
```

Para o jQuery funcionar no Node, é necessária uma janela com um documento. Como não existem janelas nativamente no Node, uma pode ser simulada por ferramentas como [jsdom](https://github.com/tmpvar/jsdom). Isso pode ser útil para fins de teste.

```js
require("jsdom").env("", function(err, window) {
    if (err) {
        console.error(err);
        return;
    }

    var $ = require("jquery")(window);
});
```
