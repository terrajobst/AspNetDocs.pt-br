---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Manipulando Propriedades DropShadow do código de cliente (VB) | Microsoft Docs
author: wenz
description: O controle DropShadow no AJAX Control Toolkit estende um painel com uma sombra. As propriedades desse extensor também podem ser alteradas usando constroem do cliente...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: a39adb9c06819f6f828add7d762effad430b8570
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613795"
---
# <a name="manipulating-dropshadow-properties-from-client-code-vb"></a>Manipular propriedades de DropShadow através de código de cliente (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)

> O controle DropShadow no AJAX Control Toolkit estende um painel com uma sombra. As propriedades desse extensor também podem ser alteradas usando código JavaScript do cliente.

## <a name="overview"></a>Visão geral

O controle DropShadow no AJAX Control Toolkit estende um painel com uma sombra. As propriedades desse extensor também podem ser alteradas usando código JavaScript do cliente.

## <a name="steps"></a>Etapas

O código começa com um painel contendo algumas linhas de texto:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

A classe CSS associada dá ao painel uma ótima cor de plano de fundo:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

O `DropShadowExtender` é adicionado para estender o painel com um efeito de sombra drop, a opacidade é definida como 50%:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

Em seguida, o controle de `ScriptManager` AJAX ASP.NET permite que o kit de ferramentas de controle funcione:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

Outro painel contém dois links JavaScript para definir a opacidade da sombra: o link menos diminui a opacidade da sombra, e o link mais aumenta.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

A função JavaScript `changeOpacity()` deve primeiro localizar o controle de `DropShadowExtender` na página. O ASP.NET AJAX define o método `$find()` para exatamente essa tarefa. Em seguida, o método `get_Opacity()` recupera a opacidade atual, o método `set_Opacity()` a define. Em seguida, o código JavaScript coloca o valor de opacidade atual no elemento `<label>`:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]

[![a opacidade é alterada no lado do cliente](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)

A opacidade é alterada no lado do cliente ([clique para exibir a imagem em tamanho normal](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](adjusting-the-z-index-of-a-dropshadow-vb.md)
