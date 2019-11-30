---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Criando caixas de seleção mutuamente exclusivasC#() | Microsoft Docs
author: wenz
description: 'Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção geralmente são usados. Há uma desvantagem, porém: quando um botão de opção em um grupo é selecionado,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: ddc154601752cc856f00dd4f3207952ab7e0e3e0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606494"
---
# <a name="creating-mutually-exclusive-checkboxes-c"></a>Criação de caixas de seleção mutuamente exclusivas (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)

> Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção geralmente são usados. Há uma desvantagem, porém: quando um botão de opção em um grupo é selecionado, não é possível desmarcar todos os botões de opção. As caixas de seleção podem ser desmarcadas a qualquer momento, no entanto, não são mutuamente exclusivas. Este tutorial fornece o melhor das duas abordagens: caixas de seleção que são mutuamente exclusivas.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção geralmente são usados. Há uma desvantagem, porém: quando um botão de opção em um grupo é selecionado, não é possível desmarcar todos os botões de opção. As caixas de seleção podem ser desmarcadas a qualquer momento, no entanto, não são mutuamente exclusivas. Este tutorial fornece o melhor das duas abordagens: caixas de seleção que são mutuamente exclusivas.

## <a name="steps"></a>Etapas

O ASP.NET AJAX Control Toolkit contém o extensor MutuallyExclusiveCheckBox. Isso permite que os programadores atribuam qualquer caixa de seleção a um nome de grupo (`Key` atributo). Em todas as caixas de seleção dentro do mesmo grupo, apenas uma pode ser selecionada ao mesmo tempo.

Vamos começar com a colocação de duas caixas de seleção em uma nova página do ASP.NET. Pode haver mais, mas duas delas são suficientes para demonstrar o princípio:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

Para ambas as caixas de seleção, um controle MutuallyExclusiveCheckBoxExtender deve ser colocado na página. Os dois atributos de chave precisam ter o mesmo valor, assim como os atributos de valor dos elementos do botão de opção HTML devem ser idênticos para indicar o grupo ao qual pertencem. A propriedade TargetControlID do extensor aponta para a ID da caixa de seleção.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

Por fim, inclua o `ScriptManager` AJAX ASP.NET, que é exigido por todos os elementos do ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

Salvar e executar a página: você pode marcar e desmarcar ambas as caixas de seleção, no entanto, em nenhum momento, ambas as caixas de seleção podem ser marcadas.

[![apenas uma caixa de seleção pode ser marcada por vez](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)

Somente uma caixa de seleção pode ser marcada de cada vez ([clique para exibir a imagem em tamanho normal](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Avançar](creating-mutually-exclusive-checkboxes-vb.md)
