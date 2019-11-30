---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Usando vários controles pop-up (VB) | Microsoft Docs
author: wenz
description: O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado. Também é possível usar m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: e1f4ff64e9fdf48ea63b75c97acd53a64b5ab5ce
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611593"
---
# <a name="using-multiple-popup-controls-vb"></a>Uso de vários controles pop-up (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado. Também é possível usar mais de um controle popup em uma página.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado. Também é possível usar mais de um controle popup em uma página.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado em qualquer lugar na página (mas dentro do elemento `<form>`):

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

Em seguida, adicione um painel que serve como o Popup. No cenário atual, o painel contém um controle de `Calendar`. Para evitar as atualizações de página causadas pelos postbacks do calendário, o painel é colocado dentro de um controle de `UpdatePanel`:

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

A página também contém duas caixas de texto. Para cada caixa de texto, o pop-up calendário deve aparecer assim que a caixa de texto for ativada.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

Agora, estenda cada uma das duas caixas de texto com uma `PopupControlExtender`. O atributo `TargetControlID` fornece a ID do controle vinculado ao extensor. O atributo `PopupControlID` contém a ID do painel pop-up. Nesse caso, os extensores mostram o mesmo painel, mas painéis diferentes também são possíveis.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

Agora, sempre que você clicar em um campo de texto, um calendário será exibido abaixo do campo, permitindo que você selecione uma data. (Obter a data selecionada para trás nas caixas de texto será abordado em um tutorial diferente.)

[![o calendário aparece quando o usuário clica na caixa de texto](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

O calendário é exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](using-multiple-popup-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Próximo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
