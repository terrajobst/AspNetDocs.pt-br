---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Usando postbacks com reordenalist (VB) | Microsoft Docs
author: wenz
description: O controle reorganizelist no AJAX Control Toolkit fornece uma lista que pode ser reordenada pelo usuário por meio de arrastar e soltar. Sempre que a lista é reordenada, um po...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5d6075e40df2c32df6c0d801243eff98fa7790b2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611362"
---
# <a name="using-postbacks-with-reorderlist-vb"></a>Uso de postbacks com ReorderList (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)

> O controle reorganizelist no AJAX Control Toolkit fornece uma lista que pode ser reordenada pelo usuário por meio de arrastar e soltar. Sempre que a lista for reordenada, um postback deverá informar o servidor da alteração.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O controle de `ReorderList` no AJAX Control Toolkit fornece uma lista que pode ser reordenada pelo usuário por meio de arrastar e soltar. Sempre que a lista for reordenada, um postback deverá informar o servidor da alteração.

## <a name="steps"></a>Etapas

Há várias fontes de dados possíveis para o controle de `ReorderList`. Uma é usar um controle de `XmlDataSource`:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

Para associar esse XML a um controle de `ReorderList` e habilitar postbacks, os seguintes atributos devem ser definidos:

- `DataSourceID`: a ID da fonte de dados
- `SortOrderField`: a propriedade a ser classificada por
- `AllowReorder`: se deseja permitir que o usuário reordene os elementos da lista
- `PostBackOnReorder`: se deseja criar um postback sempre que a lista é reorganizada

Aqui está a marcação apropriada para o controle:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

Dentro do controle de `ReorderList`, dados específicos da fonte de dados podem ser associados usando o método `Eval()`:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

Em uma posição arbitrária na página, um rótulo manterá as informações quando a última reordenação ocorrer:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

Este rótulo é preenchido com texto no código do lado do servidor, tratando o postback:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

Por fim, para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado na página:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]

[![cada reordenação dispara um postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

Cada reordenação dispara um postback ([clique para exibir a imagem em tamanho normal](using-postbacks-with-reorderlist-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](drag-and-drop-via-reorderlist-cs.md)
> [Próximo](drag-and-drop-via-reorderlist-vb.md)
