---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Manipulando postbacks de um controle popup com um UpdatePanel (C#) | Microsoft Docs
author: wenz
description: O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado. Deve-se tomar cuidado especial...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b9e58d68b3d6c5d01ceaba6c01653e9574b541b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78612927"
---
# <a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a>Tratamento de postbacks de um controle pop-up com um UpdatePanel (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)

> O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado. Deve-se tomar cuidado especial quando um postback ocorre dentro de um pop-up.

## <a name="overview"></a>Visão geral

O extensor PopupControl no AJAX Control Toolkit oferece uma maneira fácil de disparar um popup quando qualquer outro controle é ativado. Deve-se tomar cuidado especial quando um postback ocorre dentro de um pop-up.

## <a name="steps"></a>Etapas

Ao usar um `PopupControl` com um postback, um `UpdatePanel` pode impedir a atualização da página causada pelo postback. A marcação a seguir define alguns elementos importantes:

- Um controle de `ScriptManager` para que o kit de ferramentas de controle AJAX ASP.NET funcione
- Dois controles `TextBox` que irão disparar um pop-up
- Um controle `Panel` que servirá como o pop-up
- Dentro do painel, um controle de `Calendar` é inserido dentro de um controle de `UpdatePanel`
- Dois controles `PopupControlExtender` que atribuem o painel às caixas de texto

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

Observe que o atributo `OnSelectionChanged` do controle `Calendar` está definido. Assim, quando o usuário seleciona uma data dentro do calendário, ocorre um postback e o método do servidor `c1_SelectionChanged()` é executado. Dentro desse método, a data atual deve ser recuperada e gravada novamente na caixa de texto.

A sintaxe para isso é a seguinte: primeiro de todos, um objeto proxy para o `PopupControlExtender` na página deve ser gerado. O ASP.NET AJAX Control Toolkit oferece o método `GetProxyForCurrentPopup()`. O objeto que esse método retorna dá suporte ao método `Commit()` que envia um valor de volta para o controle que disparou o Popup (não o controle que disparou a chamada de método!). O código a seguir fornece a data selecionada como o argumento para o método `Commit()`, fazendo com que o código grave a data selecionada de volta na caixa de texto:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

Agora, sempre que você clicar em uma data do calendário, a data selecionada aparecerá na caixa de texto associada, criando um controle de seletor de data que pode ser encontrado atualmente em muitos sites.

[![o calendário aparece quando o usuário clica na caixa de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)

O calendário é exibido quando o usuário clica na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))

[![clicar em uma data a coloca na caixa de texto](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)

Clicar em uma data a coloca na caixa de texto ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Anterior](using-multiple-popup-controls-cs.md)
> [Próximo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
