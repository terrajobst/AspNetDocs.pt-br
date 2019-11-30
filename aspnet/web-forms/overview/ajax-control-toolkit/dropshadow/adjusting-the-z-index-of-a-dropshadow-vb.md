---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Ajustando o índice Z de um DropShadow (VB) | Microsoft Docs
author: wenz
description: O controle DropShadow no AJAX Control Toolkit estende um painel com uma sombra. No entanto, essa sombra às vezes entra em conflito com outros controles, para insta...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: 10495a9590ce1f25e9e3fa218ac5144268f50711
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574170"
---
# <a name="adjusting-the-z-index-of-a-dropshadow-vb"></a>Ajuste do índice Z de um DropShadow (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)

> O controle DropShadow no AJAX Control Toolkit estende um painel com uma sombra. No entanto, essa sombra às vezes entra em conflito com outros controles, por exemplo, o controle de menu ASP.NET. Quando uma entrada de menu é exibida, ela aparece atrás da sombra.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O controle DropShadow no AJAX Control Toolkit estende um painel com uma sombra. No entanto, essa sombra às vezes entra em conflito com outros controles, por exemplo, o controle de menu ASP.NET. Quando uma entrada de menu é exibida, ela aparece atrás da sombra.

## <a name="steps"></a>Etapas

O código começa com o próprio painel, contendo texto suficiente para que o painel contenha texto suficiente para que o efeito fique visível:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

Outro painel é colocado diretamente antes do painel de `panelShadow`. Ele contém um menu com orientação horizontal para que as entradas de menu apareçam sobre (ou em vez disso: abaixo) do painel de `dropShadow`):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

Em seguida, a `DropShadowExtender` é adicionada para estender o painel de `panelShadow` com um efeito de sombra.

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

Por fim, o controle de `ScriptManager` AJAX ASP.NET permite que o kit de ferramentas de controle funcione:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

Quando você executa esse script, as entradas de menu aparecem embaixo do painel. No entanto, o menu usa a classe CSS `panel` onde você só precisa definir duas coisas para fazer os elementos aparecerem na frente do outro painel:

- Posicionamento relativo
- Um índice z positivo

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

Em seguida, o controle de `DropShadowExtender` não é mais um conflito com o controle de menu.

[![antes: a entrada do menu não está visível](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)

Antes: a entrada do menu não está visível ([clique para exibir a imagem em tamanho normal](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))

[![após: a entrada de menu é exibida](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)

Após: a entrada de menu é exibida ([clique para exibir a imagem em tamanho normal](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](manipulating-dropshadow-properties-from-client-code-cs.md)
> [Próximo](manipulating-dropshadow-properties-from-client-code-vb.md)
