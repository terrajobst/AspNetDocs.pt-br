---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: Animando um controle UpdatePanelC#() | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 8875d750d57c5f4e362bdf461826130a881ab1d4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536375"
---
# <a name="animating-an-updatepanel-control-c"></a>Animar um controle UpdatePanel (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um UpdatePanel, existe um extensor especial que depende muito da estrutura de animação: UpdatePanelAnimation. Este tutorial mostra como configurar essa animação para um UpdatePanel.

## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Para o conteúdo de um `UpdatePanel`, existe um extensor especial que depende muito da estrutura de animação: `UpdatePanelAnimation`. Este tutorial mostra como configurar essa animação para um `UpdatePanel`.

## <a name="steps"></a>Etapas

A primeira etapa é normalmente para incluir o `ScriptManager` na página para que a biblioteca ASP.NET AJAX seja carregada e o kit de ferramentas de controle possa ser usado:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

A animação neste cenário será aplicada a um ASP.NET `Wizard` controle da Web que reside em um `UpdatePanel`. Três etapas (arbitrárias) fornecem opções suficientes para disparar postbacks:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

A marcação necessária para o controle de `UpdatePanelAnimationExtender` é bem semelhante à marcação usada para o `AnimationExtender`. No atributo `TargetControlID`, fornecemos a `ID` do `UpdatePanel` para animar; dentro do controle `UpdatePanelAnimationExtender`, o elemento `<Animations>` mantém a marcação XML para as animações. No entanto, há uma diferença: a quantidade de eventos e manipuladores de eventos é limitada em comparação com `AnimationExtender`. Por `UpdatePanels`, somente duas delas existem:

- `<OnUpdated>` quando o UpdatePanel foi atualizado
- `<OnUpdating>` quando o UpdatePanel inicia a atualização

Nesse cenário, o novo conteúdo do `UpdatePanel` (após o postback) deve desaparecer. Essa é a marcação necessária para isso:

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

Agora, sempre que um postback ocorrer no UpdatePanel, o novo conteúdo do painel esmaecerá sem problemas.

[![a próxima etapa do assistente está esmaecida](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)

A próxima etapa do assistente está esmaecida ([clique para exibir a imagem em tamanho normal](animating-an-updatepanel-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](changing-an-animation-using-client-side-code-cs.md)
> [Próximo](dynamically-controlling-updatepanel-animations-cs.md)
