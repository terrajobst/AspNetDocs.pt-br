---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animação em resposta à interação do usuárioC#() | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações podem Star...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: d04fa680d0cd4f7fb54521ac6fbb47a2cf9a83cf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614369"
---
# <a name="animating-in-response-to-user-interaction-c"></a>Animação em resposta à interação do usuário (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações podem ser iniciadas automaticamente ou podem ser disparadas pela interação do usuário, por exemplo, clicando com o mouse.

## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações podem ser iniciadas automaticamente ou podem ser disparadas pela interação do usuário, por exemplo, clicando com o mouse.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

A animação será aplicada a um painel de texto semelhante a este:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e a `runat="server"`obrigatório:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

Dentro do nó de `<Animations>`, há cinco maneiras de iniciar a animação por meio da interação do usuário (o elemento ausente é `<OnLoad>` que é executado quando a página inteira tiver sido totalmente carregada):

- `<OnClick>` (clique com o botão do mouse no controle)
- `<OnHoverOut>` (o mouse deixa o controle)
- `<OnHoverOver>` (o mouse passa sobre um controle, parando a animação de `<OnHoverOut>`)
- `<OnMouseOut>` (o mouse deixa um controle)
- `<OnMouseOver>` (o mouse passa sobre um controle, não parando a animação de `<OnMouseOut>`)

Nesse cenário, `<OnClick>` é usado. Quando o usuário clica no painel, ele é redimensionado e desaparece ao mesmo tempo.

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]

[![um clique do mouse inicia a animação](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)

Um clique do mouse inicia a animação ([clique para exibir a imagem em tamanho normal](animating-in-response-to-user-interaction-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](picking-one-animation-out-of-a-list-cs.md)
> [Próximo](disabling-actions-during-animation-cs.md)
