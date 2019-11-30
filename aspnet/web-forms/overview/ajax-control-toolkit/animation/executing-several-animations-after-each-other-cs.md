---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Executando várias animações depois umas às outrasC#() | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele permite executar o servidor...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: e378ac038796dbd44e89d099532b9e186dcf673f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575441"
---
# <a name="executing-several-animations-after-each-other-c"></a>Executar várias animações, uma após a outra (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele permite executar várias animações uma após a outra.

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele permite executar várias animações uma após a outra.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

A animação será aplicada a um painel de texto semelhante a este:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e o obrigatório `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

Dentro do nó `<Animations>`, use `<OnLoad>` para executar as animações depois que a página tiver sido totalmente carregada. Em geral, `<OnLoad>` aceita apenas uma animação. A estrutura de animação permite unir várias animações em uma usando o elemento `<Sequence>`. Todas as animações dentro de `<Sequence>` são executadas uma após a outra. Aqui está a marcação possível para o controle de `AnimationExtender`, primeiro tornando o painel mais largo e, em seguida, diminuindo sua altura:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

Quando você executa esse script, o painel primeiro fica mais largo e depois menor.

[![primeira a largura é aumentada](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)

Primeiro, a largura é aumentada ([clique para exibir a imagem em tamanho normal](executing-several-animations-after-each-other-cs/_static/image3.png))

[![, a altura é diminuída](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)

Em seguida, a altura é diminuída ([clique para exibir a imagem em tamanho normal](executing-several-animations-after-each-other-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](executing-several-animations-at-the-same-time-cs.md)
> [Próximo](animation-depending-on-a-condition-cs.md)
