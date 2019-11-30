---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Vinculando o controle deslizante (C#) | Microsoft Docs
author: wenz
description: O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse. É possível associar o Positio atual...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ef547573f17f3265ad72717d3d3bbc622fd6894e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598602"
---
# <a name="databinding-the-slider-control-c"></a>Associação de Dados do controle deslizante (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)

> O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse. É possível associar a posição atual do controle deslizante a outro controle ASP.NET.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O controle deslizante no AJAX Control Toolkit fornece um controle deslizante gráfico que pode ser controlado usando o mouse. É possível associar a posição atual do controle deslizante a outro controle ASP.NET.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado em qualquer lugar na página (mas dentro do elemento `<form>`):

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

Em seguida, adicione dois controles de `TextBox` à página. Uma será transformada em um controle deslizante gráfico e a outra terá a posição do controle deslizante.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

A próxima etapa já é a etapa final. O controle de `SliderExtender` do ASP.NET AJAX Control Toolkit faz um controle deslizante da primeira caixa de texto e atualiza automaticamente a segunda caixa de texto quando a posição do controle deslizante é alterada. Para que isso funcione, o atributo `TargetControlID` do `SliderExtender`deve ser definido como a ID da primeira caixa de texto; o atributo `BoundControlID` deve ser definido como a ID da segunda caixa de texto.

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

Como você pode ver no navegador, a vinculação de dados funciona em ambas as direções: inserir um novo valor na caixa de texto atualiza a posição do controle deslizante. Se você fizer a segunda caixa de texto somente leitura, poderá adicionar uma proteção fraca ao campo de texto para que seja mais difícil para o usuário atualizar manualmente o valor ali.

[o controle deslizante de ![e a caixa de texto estão em sincronia](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)

O controle deslizante e a caixa de texto estão em sincronia ([clique para exibir a imagem em tamanho normal](databinding-the-slider-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-the-slider-control-with-auto-postback-cs.md)
> [Próximo](using-the-slider-control-with-auto-postback-vb.md)
