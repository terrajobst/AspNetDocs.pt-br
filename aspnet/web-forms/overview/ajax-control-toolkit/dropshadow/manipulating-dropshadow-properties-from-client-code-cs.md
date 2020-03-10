---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Manipulando Propriedades DropShadow do código do clienteC#() | Microsoft Docs
author: wenz
description: Personalizando a interface de edição do DataList
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 790f0d881e43518600968d6c175d4eaa53d0e5f9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613816"
---
# <a name="manipulating-dropshadow-properties-from-client-code-c"></a>Manipular propriedades de DropShadow através de código de cliente (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> O controle DropShadow no AJAX Control Toolkit estende um painel com uma sombra. As propriedades desse extensor também podem ser alteradas usando código JavaScript do cliente.

## <a name="overview"></a>Visão geral

O controle DropShadow no AJAX Control Toolkit estende um painel com uma sombra. As propriedades desse extensor também podem ser alteradas usando código JavaScript do cliente.

## <a name="steps"></a>Etapas

O código começa com um painel contendo algumas linhas de texto:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

A classe CSS associada dá ao painel uma ótima cor de plano de fundo:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

O `DropShadowExtender` é adicionado para estender o painel com um efeito de sombra drop, a opacidade é definida como 50%:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

Em seguida, o controle de `ScriptManager` AJAX ASP.NET permite que o kit de ferramentas de controle funcione:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

Outro painel contém dois links JavaScript para definir a opacidade da sombra: o link menos diminui a opacidade da sombra, e o link mais aumenta.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

A função JavaScript `changeOpacity()` deve primeiro localizar o controle de `DropShadowExtender` na página. O ASP.NET AJAX define o método `$find()` para exatamente essa tarefa. Em seguida, o método `get_Opacity()` recupera a opacidade atual, o método `set_Opacity()` a define. Em seguida, o código JavaScript coloca o valor de opacidade atual no elemento `<label>`:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]

[![a opacidade é alterada no lado do cliente](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

A opacidade é alterada no lado do cliente ([clique para exibir a imagem em tamanho normal](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Próximo](adjusting-the-z-index-of-a-dropshadow-vb.md)
