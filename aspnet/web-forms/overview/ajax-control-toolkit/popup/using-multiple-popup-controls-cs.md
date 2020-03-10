---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Usando vários controles pop-C#up () | Microsoft Docs
author: wenz
description: O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado. Também é possível usar m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 8700fe89af591e8b481e853580b0efa0cddbf1bc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612647"
---
# <a name="using-multiple-popup-controls-c"></a>Uso de vários controles pop-up (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)

> O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado. Também é possível usar mais de um controle popup em uma página.

## <a name="overview"></a>Visão geral

O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado. Também é possível usar mais de um controle popup em uma página.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado em qualquer lugar na página (mas dentro do elemento `<form>`):

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

Em seguida, adicione um painel que serve como o Popup. No cenário atual, o painel contém um controle de `Calendar`. Para evitar as atualizações de página causadas pelos postbacks do calendário, o painel é colocado dentro de um controle de `UpdatePanel`:

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

A página também contém duas caixas de texto. Para cada caixa de texto, o pop-up calendário deve aparecer assim que a caixa de texto for ativada.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

Agora, estenda cada uma das duas caixas de texto com uma `PopupControlExtender`. O atributo `TargetControlID` fornece a ID do controle vinculado ao extensor. O atributo `PopupControlID` contém a ID do painel pop-up. Nesse caso, os extensores mostram o mesmo painel, mas painéis diferentes também são possíveis.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

Agora, sempre que você clicar em um campo de texto, um calendário será exibido abaixo do campo, permitindo que você selecione uma data. (Obter a data selecionada para trás nas caixas de texto será abordado em um tutorial diferente.)

[![o calendário aparece quando o usuário clica na caixa de texto](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

O calendário é exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](using-multiple-popup-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Próximo](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
