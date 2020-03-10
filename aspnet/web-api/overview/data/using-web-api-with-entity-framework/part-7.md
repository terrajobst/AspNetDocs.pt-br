---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Criar o modo de exibição (IU) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 62c4523c2c6fb399cfbc3716309a1379996d601c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557298"
---
# <a name="create-the-view-ui"></a>Criar a exibição (interface do usuário)

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://github.com/MikeWasson/BookService)

Nesta seção, você começará a definir o HTML para o aplicativo e adicionará a vinculação de dados entre o HTML e o modelo de exibição.

Abra o arquivo views/home/index. cshtml. Substitua todo o conteúdo desse arquivo pelo seguinte.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

A maioria dos elementos de `div` está lá para o estilo de [inicialização](http://getbootstrap.com/) . Os elementos importantes são aqueles com `data-bind` atributos. Esse atributo vincula o HTML ao modelo de exibição.

Por exemplo:

[!code-html[Main](part-7/samples/sample2.html)]

Neste exemplo, a associação de &quot; de `text`de &quot;faz com que o elemento `<p>` mostre o valor da propriedade `error` do modelo de exibição. Lembre-se de que `error` foi declarado como um `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Sempre que um novo valor é atribuído a `error`, o Knockout atualiza o texto no elemento `<p>`.

A associação de `foreach` informa ao Knockout para executar um loop pelo conteúdo da matriz `books`. Para cada item na matriz, o Knockout cria um novo elemento &lt;li&gt;. Associações dentro do contexto do `foreach` se referem a propriedades no item da matriz. Por exemplo:

[!code-html[Main](part-7/samples/sample4.html)]

Aqui, a associação de `text` lê a propriedade autor de cada livro.

Se você executar o aplicativo agora, ele deverá ter a seguinte aparência:

![](part-7/_static/image1.png)

A lista de livros é carregada de forma assíncrona, após o carregamento da página. No momento, o &quot;detalhes&quot; links não são funcionais. Vamos adicionar essa funcionalidade na próxima seção.

> [!div class="step-by-step"]
> [Anterior](part-6.md)
> [Próximo](part-8.md)
