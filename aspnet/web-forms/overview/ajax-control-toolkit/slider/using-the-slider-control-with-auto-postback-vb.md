---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Usando o controle deslizante com postback automático (VB) | Microsoft Docs
author: wenz
description: O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse. É possível fazer com que o controle deslizante autolance...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: e7a3286bcf7ca844f5dcfa4848c15e0bd4767c0f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553560"
---
# <a name="using-the-slider-control-with-auto-postback-vb"></a>Usando o controle deslizante com postback automático (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse. É possível fazer o controle deslizante AutoPostBack quando seu valor é alterado.

## <a name="overview"></a>Visão geral

O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse. É possível fazer o controle deslizante AutoPostBack quando seu valor é alterado.

## <a name="steps"></a>Etapas

Para fazer com que o controle deslizante seja automaticamente postback após uma alteração, ambas as caixas de texto precisam do atributo `AutoPostBack="true"`: a caixa de texto que se tornará o controle deslizante e a caixa de texto que contém a posição do controle deslizante. Aqui está a marcação necessária para isso:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

O controle de `SliderExtender` do kit de ferramentas de controle AJAX ASP.NET atribui a funcionalidade Slider às duas caixas de texto:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

Um elemento rótulo adicional posteriormente será usado para informar o usuário de um postback:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

Por fim, o `ScriptManager` controle do ASP.NET AJAX carrega o JavaScript necessário para que o kit de ferramentas de controle funcione:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

Agora o controle deslizante está fazendo o lançamento; no lado do servidor, esse evento pode ser capturado e Tratado:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]

[![mover o controle deslizante dispara um postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

Mover o controle deslizante dispara um postback ([clique para exibir a imagem em tamanho normal](using-the-slider-control-with-auto-postback-vb/_static/image3.png))

[![depois, a data dessa alteração será gravada no rótulo](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

Posteriormente, a data dessa alteração será gravada no rótulo ([clique para exibir a imagem em tamanho normal](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](databinding-the-slider-control-cs.md)
> [Próximo](databinding-the-slider-control-vb.md)
