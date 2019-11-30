---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Escolhendo uma animação fora de uma lista (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A estrutura também permitirá...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 694416532b558291ff6ab57a442058b53d6167e0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606730"
---
# <a name="picking-one-animation-out-of-a-list-vb"></a>Escolher uma animação em uma lista (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A estrutura também permite que o programador escolha uma animação fora de uma lista de animações, dependendo da avaliação de algum código JavaScript.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A estrutura também permite que o programador escolha uma animação fora de uma lista de animações, dependendo da avaliação de algum código JavaScript.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

A animação será aplicada a um painel de texto semelhante a este:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e o obrigatório `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

Dentro do nó `<Animations>`, use `<OnLoad>` para executar as animações depois que a página tiver sido totalmente carregada. Em vez de uma das animações regulares, o elemento `<Case>` entra em cena. O valor de seu atributo SelectScript é avaliado; o valor de retorno deve ser numérico. Dependendo desse número, uma das subanimaçãos dentro de &lt;caso&gt; é executada. Por exemplo, se SelectScript for avaliada como 2, o kit de ferramentas de controle executará a terceira animação dentro &lt;caso&gt; (a contagem começa em 0).

A marcação a seguir define três subanimaçãos: redimensionando a largura, redimensionando a altura e desbotando. O código JavaScript (`Math.floor(3 * Math.random())`), em seguida, escolhe um número entre 0 e 2, para que uma das três animações seja executada:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]

[![uma das três animações possíveis: o painel fica mais largo](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)

Uma das três animações possíveis: o painel fica mais largo ([clique para exibir a imagem em tamanho normal](picking-one-animation-out-of-a-list-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animation-depending-on-a-condition-vb.md)
> [Próximo](animating-in-response-to-user-interaction-vb.md)
