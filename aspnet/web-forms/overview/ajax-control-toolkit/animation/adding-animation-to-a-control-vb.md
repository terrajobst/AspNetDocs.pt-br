---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Adicionando animação a um controle (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Este tutorial mostra como...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: efaee9c1665d795dc1a889b9ac9f25dd1c08f4e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598297"
---
# <a name="adding-animation-to-a-control-vb"></a>Adição de animação a um controle (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Este tutorial mostra como configurar essa animação.

## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Este tutorial mostra como configurar essa animação.

## <a name="steps"></a>Etapas

A primeira etapa é normalmente para incluir o `ScriptManager` na página para que a biblioteca ASP.NET AJAX seja carregada e o kit de ferramentas de controle possa ser usado:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

A animação neste cenário será aplicada a um painel de texto semelhante a este:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

A classe CSS associada para o painel define uma cor de plano de fundo e uma largura:

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

Em seguida, precisamos do `AnimationExtender`. Depois de fornecer um `ID` e o `runat="server"`usual, o atributo `TargetControlID` deve ser definido como o controle para animar em nosso caso, o painel:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

A animação inteira é aplicada declarativamente, usando uma sintaxe XML, infelizmente, atualmente, não tem suporte total do IntelliSense do Visual Studio. O nó raiz está `<Animations>;` dentro desse nó, vários eventos são permitidos, que determinam quando as animações levam (m) lugar:

- `OnClick` (clique do mouse)
- `OnHoverOut` (quando o mouse sai de um controle)
- `OnHoverOver` (quando o mouse passa sobre um controle, parando a animação de `OnHoverOut`)
- `OnLoad` (quando a página tiver sido carregada)
- `OnMouseOut` (quando o mouse sai de um controle)
- `OnMouseOver` (quando o mouse passa sobre um controle, não parando a animação de `OnMouseOut`)

A estrutura vem com um conjunto de animações, cada uma representada por seu próprio elemento XML. Aqui está uma seleção:

- `<Color>` (alterando uma cor)
- `<FadeIn>` (esmaecimento)
- `<FadeOut>` (esmaecimento)
- `<Property>` (alterando a propriedade de um controle)
- `<Pulse>` (Pulsating)
- `<Resize>` (alterando o tamanho)
- `<Scale>` (alterando o tamanho proporcionalmente)

Neste exemplo, o painel deve desaparecer. A animação deve levar de 10 a 1,5 segundos (`Duration` atributo), exibindo 24 quadros (etapas de animação) por segundo (atributo`Fps`). Aqui está a marcação completa para o controle de `AnimationExtender`:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

Quando você executa esse script, o painel é exibido e desaparece em um e meio segundo.

[![o painel está esmaecido](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

O painel está esmaecido ([clique para exibir a imagem em tamanho normal](adding-animation-to-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](dynamically-controlling-updatepanel-animations-cs.md)
> [Próximo](executing-several-animations-at-the-same-time-vb.md)
