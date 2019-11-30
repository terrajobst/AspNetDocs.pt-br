---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Posicionando um ModalPopup (VB) | Microsoft Docs
author: wenz
description: O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente. No entanto, o controle não oferece um...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: fb79a08a339588ed8adc4b4236911819ea9286b4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598981"
---
# <a name="positioning-a-modalpopup-vb"></a>Posicionamento de um ModalPopup (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)

> O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente. No entanto, o controle não oferece uma funcionalidade interna para posicionar o pop-up.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente. No entanto, o controle não oferece uma funcionalidade interna para posicionar o pop-up.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o `ScriptManager`. o controle deve ser colocado em qualquer lugar na página (mas dentro do elemento `<form>`):

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

Em seguida, adicione um painel que serve como o popup modal. Um botão é usado para fechar o pop-up:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

Sempre que o Popup for mostrado, ele deverá ser posicionado em um determinado local na página. Para essa tarefa, uma função JavaScript do lado do cliente é criada. Ele primeiro tenta acessar o painel. Se for bem sucedido, a posição do painel será definida usando CSS e JavaScript (altere a posição do pop-up em irá). No entanto, o controle de `ModalPopupExtender` também tenta posicionar o pop-up. Portanto, o código JavaScript posiciona repetidamente o Popup, a cada décimo de segundo.

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

Como você pode ver, o valor de retorno do `setTimeout()` método JavaScript é salvo em uma variável global. Isso permite interromper o posicionamento repetido do Popup sob demanda, usando o método `clearTimeout()`:

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

Agora, tudo o que resta fazer é fazer com que o navegador chame essas funções sempre que apropriado. A função JavaScript `movePanel()` deve ser chamada quando o botão é clicado que dispara o painel:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

E a função `stopMoving()` entra em ação quando o popup é fechado, isso pode ser disparado no controle de `ModalPopupExtender`:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]

[![popup modal aparece na posição designada](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)

O popup modal aparece na posição designada ([clique para exibir a imagem em tamanho normal](positioning-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](handling-postbacks-from-a-modalpopup-vb.md)
