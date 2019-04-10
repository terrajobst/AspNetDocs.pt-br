---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Ajuste do índice Z de um DropShadow (c#) | Microsoft Docs
author: wenz
description: O controle de DropShadow no AJAX Control Toolkit, um painel com uma sombra. No entanto esse sombra às vezes, está em conflito com outros controles, para insta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: cc9407ba15474f58437817c9536d6040e0ea2e84
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59381427"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-c"></a>Ajuste do índice Z de um DropShadow (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar o código](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> O controle de DropShadow no AJAX Control Toolkit, um painel com uma sombra. No entanto esse sombra às vezes, está em conflito com outros controles, por exemplo, o controle de Menu do ASP.NET. Quando uma entrada de menu é exibido, ele aparece por trás da sombra.


## <a name="overview"></a>Visão geral

O controle de DropShadow no AJAX Control Toolkit, um painel com uma sombra. No entanto esse sombra às vezes, está em conflito com outros controles, por exemplo, o controle de Menu do ASP.NET. Quando uma entrada de menu é exibido, ele aparece por trás da sombra.

## <a name="steps"></a>Etapas

O código começa com o próprio painel, que contém o texto suficiente para que o painel contém texto suficiente para o efeito para ser visível:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

Outro painel é colocado diretamente antes do `panelShadow` painel. Ele contém um menu com a orientação horizontal para que as entradas de menu aparecem ao longo (ou melhor: sob) o `dropShadow` painel):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

Em seguida, o `DropShadowExtender` é adicionado ao estender o `panelShadow` painel com um efeito de sombra:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

Por fim, o ASP.NET AJAX `ScriptManager` controle permite que o Kit de ferramentas trabalhar:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

Quando você executa esse script, as entradas de menu aparecem sob o painel. No entanto, o menu usa a classe CSS `panel` em que você só precisa definir duas coisas para fazer com que elementos são exibidos na frente de outro painel:

- Posicionamento relativo
- Um índice z positivo

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

Em seguida, o `DropShadowExtender` controle não está em conflito mais com o controle de Menu.


[![Bantes: A entrada de menu não é visível](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

Antes: A entrada de menu não é visível ([clique para exibir a imagem em tamanho normal](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))


[![Atermina após: A entrada de menu aparece](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

Depois: A entrada de menu é exibido ([clique para exibir a imagem em tamanho normal](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Avançar](manipulating-dropshadow-properties-from-client-code-cs.md)
