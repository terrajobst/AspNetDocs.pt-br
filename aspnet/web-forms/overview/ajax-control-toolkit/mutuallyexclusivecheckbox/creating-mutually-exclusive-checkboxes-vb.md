---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Criando caixas de seleção mutuamente exclusivas (VB) | Microsoft Docs
author: wenz
description: 'Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção geralmente são usados. Há uma desvantagem, porém: quando um botão de opção em um grupo é selecionado,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: f33936dd4d71f6bbf08f02966eefe44c8c152eba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554008"
---
# <a name="creating-mutually-exclusive-checkboxes-vb"></a>Criação de caixas de seleção mutuamente exclusivas (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)

> Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção geralmente são usados. Há uma desvantagem, porém: quando um botão de opção em um grupo é selecionado, não é possível desmarcar todos os botões de opção. As caixas de seleção podem ser desmarcadas a qualquer momento, no entanto, não são mutuamente exclusivas. Este tutorial fornece o melhor das duas abordagens: caixas de seleção que são mutuamente exclusivas.

## <a name="overview"></a>Visão geral

Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção geralmente são usados. Há uma desvantagem, porém: quando um botão de opção em um grupo é selecionado, não é possível desmarcar todos os botões de opção. As caixas de seleção podem ser desmarcadas a qualquer momento, no entanto, não são mutuamente exclusivas. Este tutorial fornece o melhor das duas abordagens: caixas de seleção que são mutuamente exclusivas.

## <a name="steps"></a>Etapas

O ASP.NET AJAX Control Toolkit contém o extensor MutuallyExclusiveCheckBox. Isso permite que os programadores atribuam qualquer caixa de seleção a um nome de grupo (`Key` atributo). Em todas as caixas de seleção dentro do mesmo grupo, apenas uma pode ser selecionada ao mesmo tempo.

Vamos começar com a colocação de duas caixas de seleção em uma nova página do ASP.NET. Pode haver mais, mas duas delas são suficientes para demonstrar o princípio:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

Para ambas as caixas de seleção, um controle MutuallyExclusiveCheckBoxExtender deve ser colocado na página. Os dois atributos de chave precisam ter o mesmo valor, assim como os atributos de valor dos elementos do botão de opção HTML devem ser idênticos para indicar o grupo ao qual pertencem. A propriedade TargetControlID do extensor aponta para a ID da caixa de seleção.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

Por fim, inclua o `ScriptManager` AJAX ASP.NET, que é exigido por todos os elementos do ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

Salvar e executar a página: você pode marcar e desmarcar ambas as caixas de seleção, no entanto, em nenhum momento, ambas as caixas de seleção podem ser marcadas.

[![apenas uma caixa de seleção pode ser marcada por vez](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

Somente uma caixa de seleção pode ser marcada de cada vez ([clique para exibir a imagem em tamanho normal](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](creating-mutually-exclusive-checkboxes-cs.md)
