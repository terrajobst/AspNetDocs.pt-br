---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
title: Executando várias animações ao mesmo tempo (C#) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele permite executar o servidor...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 219149e1-3ee9-4b79-8fe4-7433f6b7d15b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-cs
msc.type: authoredcontent
ms.openlocfilehash: fe71feccbcbc4ee8e9cdc09d6220de6a53dd2d2b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598115"
---
# <a name="executing-several-animations-at-the-same-time-c"></a>Executando várias animações ao mesmo tempo (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2CS.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele permite executar várias animações de maneira paralela.

## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele permite executar várias animações de maneira paralela.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample1.aspx)]

A animação será aplicada a um painel de texto semelhante a este:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample2.aspx)]

Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:

[!code-css[Main](executing-several-animations-at-the-same-time-cs/samples/sample3.css)]

Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e a `runat="server"`obrigatório:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample4.aspx)]

Dentro do nó `<Animations>`, use `<OnLoad>` para executar as animações depois que a página tiver sido totalmente carregada. Em geral, `<OnLoad>` aceita apenas uma animação. A estrutura de animação permite unir várias animações em uma usando o elemento `<Parallel>`. Todas as animações dentro de `<Parallel>` são executadas ao mesmo tempo.

Veja uma possível marcação para o controle de `AnimationExtender`, desbotando e redimensionando o painel ao mesmo tempo:

[!code-aspx[Main](executing-several-animations-at-the-same-time-cs/samples/sample5.aspx)]

E de fato: quando você executa esse script, o painel é exibido e, em seguida, redimensiona (mais do que percorrida sua largura e metade da altura) e esmaece ao mesmo tempo.

[![o painel está esmaecido e redimensionando (incluindo seu conteúdo, graças ao mecanismo de renderização do navegador)](executing-several-animations-at-the-same-time-cs/_static/image2.png)](executing-several-animations-at-the-same-time-cs/_static/image1.png)

O painel está esmaecido e redimensionando (incluindo seu conteúdo, graças ao mecanismo de renderização do navegador) ([clique para exibir a imagem em tamanho normal](executing-several-animations-at-the-same-time-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](adding-animation-to-a-control-cs.md)
> [Próximo](executing-several-animations-after-each-other-cs.md)
