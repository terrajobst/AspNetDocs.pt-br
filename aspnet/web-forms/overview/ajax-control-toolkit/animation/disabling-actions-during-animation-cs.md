---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Desabilitando ações durante a animaçãoC#() | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele também dá suporte a ação...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: e91205ad2f9e6ee1fdd869ceb7587c3a82754772
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599720"
---
# <a name="disabling-actions-during-animation-c"></a>Desabilitar ações durante a animação (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele também dá suporte a ações, como cliques do mouse. No entanto, quando um clique do mouse inicia uma animação, é desejável desabilitar cliques do mouse durante a animação.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele também dá suporte a ações, como cliques do mouse. No entanto, quando um clique do mouse inicia uma animação, é desejável desabilitar cliques do mouse durante a animação.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

A animação será aplicada a um botão HTML como este:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

Observe que um controle HTML é usado em vez de um controle da Web, já que não queremos que o botão crie um postback; Ele deve simplesmente iniciar a animação do lado do cliente para nós.

Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e a `runat="server"`obrigatório:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

Dentro do nó `<Animations>`, `<OnClick>` é o elemento certo para manipular o clique do mouse. No entanto, o botão também poderia ser clicado durante a animação. O elemento `<EnableAction>` pode cuidar disso. A configuração `Enabled="false"` desabilita o botão como parte da animação. Como estamos usando várias animações individuais (desativando o botão e as animações reais), o elemento `<Parallel>` é necessário para unir as animações individuais em uma. Aqui está a marcação completa para `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

Também seria possível reabilitar o botão após a animação, usando o seguinte elemento XML no final da lista:

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

No entanto, no cenário fornecido, isso seria inútil, pois o botão desaparece e não fica visível no final da animação.

[![o botão é desabilitado assim que a animação é executada](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

O botão é desabilitado assim que a animação é executada ([clique para exibir a imagem em tamanho normal](disabling-actions-during-animation-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animating-in-response-to-user-interaction-cs.md)
> [Próximo](triggering-an-animation-in-another-control-cs.md)
