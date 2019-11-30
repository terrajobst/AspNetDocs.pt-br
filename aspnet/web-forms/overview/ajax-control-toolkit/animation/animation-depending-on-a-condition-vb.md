---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
title: Animação, dependendo de uma condição (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Se uma animação é...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1b87d8d6-b3f7-4126-b51c-d41442fbf947
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-vb
msc.type: authoredcontent
ms.openlocfilehash: 583ebdbf109beb74b9a425020477183067bbb79a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606996"
---
# <a name="animation-depending-on-a-condition-vb"></a>Animação dependendo de uma condição (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Se uma animação é executada ou não também pode depender de uma condição na forma de algum código JavaScript.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Se uma animação é executada ou não também pode depender de uma condição na forma de algum código JavaScript.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample1.aspx)]

A animação será aplicada a um painel de texto semelhante a este:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample2.aspx)]

Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:

[!code-css[Main](animation-depending-on-a-condition-vb/samples/sample3.css)]

Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e o obrigatório `runat="server":`

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample4.aspx)]

Dentro do nó `<Animations>`, use `<OnLoad>` para executar as animações depois que a página tiver sido totalmente carregada. Em vez de uma das animações regulares, o elemento `<Condition>` entra em cena. O código JavaScript fornecido como o valor do atributo `ConditionScript` é executado em tempo de execução. Se for avaliado como true, a animação será executada, caso contrário, não. A marcação a seguir fornece duas animações, cada uma delas sendo executada em 50% dos casos após Random. Como pode haver apenas uma animação dentro de `<OnLoad>`, as duas animações `<Condition>` são unidas usando o elemento `<Sequence>`:

[!code-aspx[Main](animation-depending-on-a-condition-vb/samples/sample5.aspx)]

Observe que o sinal de menor que (`<`) no atributo `ConditionScript` deve ser escape (). Quando você executa esse script, nenhuma execução de animação é executada ou uma das duas faz, ou ambas, as duas.

[![o painel está esmaecido sem redimensionamento, a segunda animação é executada, o primeiro não](animation-depending-on-a-condition-vb/_static/image2.png)](animation-depending-on-a-condition-vb/_static/image1.png)

O painel está esmaecido sem redimensionamento, portanto a segunda animação é executada, a primeira não foi ([clique para exibir a imagem em tamanho normal](animation-depending-on-a-condition-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](executing-several-animations-after-each-other-vb.md)
> [Próximo](picking-one-animation-out-of-a-list-vb.md)
