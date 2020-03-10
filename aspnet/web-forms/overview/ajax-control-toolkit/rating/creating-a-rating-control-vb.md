---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Criando um controle de classificação (VB) | Microsoft Docs
author: wenz
description: Muitos sites, de comércio eletrônico a sites da Comunidade, oferecem aos usuários a taxa de artigos ou itens. Isso geralmente requer algum esforço de codificação, mas temos o...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 08e245edfe73db4e3896db51151e5d7a0fa9697c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612192"
---
# <a name="creating-a-rating-control-vb"></a>Criação de um controle Classificação (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)

> Muitos sites, de comércio eletrônico a sites da Comunidade, oferecem aos usuários a taxa de artigos ou itens. Isso geralmente requer algum esforço de codificação, mas temos o kit de ferramentas de controle para nossa disposição.

## <a name="overview"></a>Visão geral

Muitos sites, de comércio eletrônico a sites da Comunidade, oferecem aos usuários a taxa de artigos ou itens. Isso geralmente requer algum esforço de codificação, mas temos o kit de ferramentas de controle para nossa disposição.

## <a name="steps"></a>Etapas

Em primeiro lugar, você precisa de (pelo menos) dois tipos de imagens: uma para um item de avaliação preenchido e outra para um item de classificação vazio. Um item de classificação geralmente é uma estrela ou um Smiley. Para esse cenário, você encontra três arquivos, Smiley. png e Empty. png e Smiley-done. png como parte do download do código-fonte para este tutorial.

Em seguida, crie um novo arquivo ASP.NET e comece com a adição de um controle de `ScriptManager` a ele:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

Em seguida, adicione o controle de `Rating` do kit de ferramentas de controle AJAX ASP.NET. Os atributos a seguir precisam ser definidos para este exemplo:

- `CurrentRating` a classificação inicial a ser usada
- `MaxRating` a classificação máxima
- `EmptyStarCssClass` a classe CSS a ser usada quando um item de classificação (estrela) estiver vazio
- `FilledStarCssClass` a classe CSS a ser usada quando um item de classificação (estrela) é preenchido
- `StarCssClass` a classe CSS a ser usada para uma estatística visível
- `WaitingStarCssClass` a classe CSS a ser usada enquanto uma classificação por estrelas é enviada de volta ao servidor

E aqui está a marcação que cria um controle de classificação com cinco itens (sorriso) dos quais nenhum é preenchido inicialmente:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

As três classes CSS referenciadas agora precisam mostrar os arquivos de imagem apropriados, o que é fácil de fazer usando CSS:

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

Certifique-se de fornecer a largura e a altura das três imagens, caso contrário, a exibição pode parecer um pouco bagunçada.

Por fim, o resultado da classificação deve ser exibido ao usuário (ou, pelo menos, salvo em um banco de dados). Portanto, adicione um rótulo para a saída de uma mensagem de texto e um botão enviar para postar o formulário de classificação no servidor:

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

No código do servidor, acesse o controle de classificação por meio de seu `ID` e, em seguida, acesse sua propriedade `CurrentRating`, que é o número dos itens de classificação selecionados, em nosso exemplo um valor entre 0 e 5.

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

Salve a página e carregue-a no navegador. Quando você passa o mouse sobre os itens de classificação (inicialmente vazios), ocorre um efeito de JavaScript: a classificação muda. Quando você clica no conjunto de estrelas, a classificação atual é mantida. Por fim, quando você envia o formulário, o código do lado do servidor gera a classificação selecionada.

[![criar um sistema de classificação com código mínimo](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)

Criando um sistema de classificação com código mínimo ([clique para exibir a imagem em tamanho normal](creating-a-rating-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](creating-a-rating-control-cs.md)
