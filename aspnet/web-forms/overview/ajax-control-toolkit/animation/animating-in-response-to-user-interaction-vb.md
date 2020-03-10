---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animação em resposta à interação do usuário (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações podem Star...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 629d79505cebd49c2f05333bfbf78166f80fc6cb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614397"
---
# <a name="animating-in-response-to-user-interaction-vb"></a>Animação em resposta à interação do usuário (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações podem ser iniciadas automaticamente ou podem ser disparadas pela interação do usuário, por exemplo, clicando com o mouse.

## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações podem ser iniciadas automaticamente ou podem ser disparadas pela interação do usuário, por exemplo, clicando com o mouse.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

A animação será aplicada a um painel de texto semelhante a este:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e a `runat="server"`obrigatório:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

Dentro do nó de `<Animations>`, há cinco maneiras de iniciar a animação por meio da interação do usuário (o elemento ausente é `<OnLoad>` que é executado quando a página inteira tiver sido totalmente carregada):

- `<OnClick>` (clique com o botão do mouse no controle)
- `<OnHoverOut>` (o mouse deixa o controle)
- `<OnHoverOver>` (o mouse passa sobre um controle, parando a animação de `<OnHoverOut>`)
- `<OnMouseOut>` (o mouse deixa um controle)
- `<OnMouseOver>` (o mouse passa sobre um controle, não parando a animação de `<OnMouseOut>`)

Nesse cenário, `<OnClick>` é usado. Quando o usuário clica no painel, ele é redimensionado e desaparece ao mesmo tempo.

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]

[![um clique do mouse inicia a animação](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)

Um clique do mouse inicia a animação ([clique para exibir a imagem em tamanho normal](animating-in-response-to-user-interaction-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](picking-one-animation-out-of-a-list-vb.md)
> [Próximo](disabling-actions-during-animation-vb.md)
