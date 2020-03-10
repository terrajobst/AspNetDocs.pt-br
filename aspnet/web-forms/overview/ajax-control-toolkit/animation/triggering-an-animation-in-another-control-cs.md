---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Disparando uma animação em outro controleC#() | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Geralmente, iniciando um...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e0a1f8986047da04db6fde8e54b6fd0ac6ee2603
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536074"
---
# <a name="triggering-an-animation-in-another-control-c"></a>Disparar uma animação em outro controle (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Geralmente, iniciar uma animação é disparado pela interação do usuário com o mesmo controle. No entanto, também é possível interagir com um controle e, em seguida, animações de outro controle.

## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Geralmente, iniciar uma animação é disparado pela interação do usuário com o mesmo controle. No entanto, também é possível interagir com um controle e, em seguida, animações de outro controle.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

A animação será aplicada a um painel de texto semelhante a este:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Para começar a animar o painel, é usado um botão HTML. Observe que `<input type="button" />` é Favoured sobre `<asp:Button />`, já que não queremos um postback quando o usuário clica nesse botão.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e a `runat="server"`obrigatório. É importante definir `TargetControlID` como a ID do botão (o elemento que dispara a animação), não para a ID do painel (o elemento que está sendo animado)

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

No nó `<Animations>`, coloque as animações como de costume. Para fazer com que eles alterem o painel, não o botão, defina o atributo `AnimationTarget` para cada elemento de animação dentro de `AnimationExtender`. O valor de `AnimationTarget` é a ID do painel, é claro. Dessa forma, as animações acontecem com o painel, não com o botão de disparo. Aqui está a marcação `AnimationExtender` para este cenário:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Observe a ordem especial na qual as animações individuais aparecem. Em primeiro lugar, o botão é desativado quando a animação é executada. Como não há nenhum atributo `AnimationTarget` no elemento `<EnableAction>`, essa animação é aplicada ao controle de origem: o botão. As próximas duas etapas de animação devem ser executadas em paralelo (`<Parallel>` elemento). Ambos têm seus atributos de `AnimationTarget` definidos como `"Panel1"`, animando o painel, não o botão.

[![um clique do mouse no botão inicia a animação do painel](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Um clique do mouse no botão inicia a animação do painel ([clique para exibir a imagem em tamanho normal](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](disabling-actions-during-animation-cs.md)
> [Próximo](modifying-animations-from-the-server-side-cs.md)
