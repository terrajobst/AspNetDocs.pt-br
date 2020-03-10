---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Desabilitando ações durante a animação (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele também dá suporte a ação...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 4924d4f70099255b930d53f6a72e810be7a47485
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536235"
---
# <a name="disabling-actions-during-animation-vb"></a>Desabilitar ações durante a animação (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele também dá suporte a ações, como cliques do mouse. No entanto, quando um clique do mouse inicia uma animação, é desejável desabilitar cliques do mouse durante a animação.

## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele também dá suporte a ações, como cliques do mouse. No entanto, quando um clique do mouse inicia uma animação, é desejável desabilitar cliques do mouse durante a animação.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

A animação será aplicada a um botão HTML como este:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

Observe que um controle HTML é usado em vez de um controle da Web, já que não queremos que o botão crie um postback; Ele deve simplesmente iniciar a animação do lado do cliente para nós.

Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e a `runat="server"`obrigatório:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

Dentro do nó `<Animations>`, `<OnClick>` é o elemento certo para manipular o clique do mouse. No entanto, o botão também poderia ser clicado durante a animação. O elemento `<EnableAction>` pode cuidar disso. A configuração `Enabled="false"` desabilita o botão como parte da animação. Como estamos usando várias animações individuais (desativando o botão e as animações reais), o elemento `<Parallel>` é necessário para unir as animações individuais em uma. Aqui está a marcação completa para `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

Também seria possível reabilitar o botão após a animação, usando o seguinte elemento XML no final da lista:

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

No entanto, no cenário fornecido, isso seria inútil, pois o botão desaparece e não fica visível no final da animação.

[![o botão é desabilitado assim que a animação é executada](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

O botão é desabilitado assim que a animação é executada ([clique para exibir a imagem em tamanho normal](disabling-actions-during-animation-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](animating-in-response-to-user-interaction-vb.md)
> [Próximo](triggering-an-animation-in-another-control-vb.md)
