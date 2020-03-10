---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Exibir detalhes do item | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 3f48c5ad73ceb9a4873fbbb621b518553a498966
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557319"
---
# <a name="display-item-details"></a>Exibir detalhes do item

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://github.com/MikeWasson/BookService)

Nesta seção, você adicionará a capacidade de exibir detalhes de cada livro. No app. js, adicione ao seguinte código ao modelo de exibição:

[!code-javascript[Main](part-8/samples/sample1.js)]

Em views/home/index. cshtml, adicione um elemento de vinculação de dados ao link detalhes:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Isso associa o manipulador de cliques para o &lt;um elemento&gt; à função `getBookDetail` no modelo de exibição.

No mesmo arquivo, substitua a seguinte marcação:

[!code-html[Main](part-8/samples/sample3.html)]

por este:

[!code-html[Main](part-8/samples/sample4.html)]

Essa marcação cria uma tabela que está associada a dados para as propriedades do `detail` observável no modelo de exibição.

A sintaxe de &quot; "&lt;!--ko--&gt;permite incluir uma associação de Knockout fora de um elemento DOM. Nesse caso, a associação de `if` faz com que esta seção de marcação seja exibida somente quando `details` não é nulo.

[!code-html[Main](part-8/samples/sample5.html)]

Agora, se você executar o aplicativo e clicar em uma das &quot;detalhes&quot; links, o aplicativo exibirá os detalhes do livro.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Anterior](part-7.md)
> [Próximo](part-9.md)
