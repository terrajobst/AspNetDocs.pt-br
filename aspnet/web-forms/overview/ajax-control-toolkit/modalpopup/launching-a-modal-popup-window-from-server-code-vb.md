---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Iniciando uma janela pop-up modal do código do servidor (VB) | Microsoft Docs
author: wenz
description: O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente. No entanto, alguns cenários exigem que t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 1368a78d35ac6461bbc2e852e468f42eef2c0d2c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606583"
---
# <a name="launching-a-modal-popup-window-from-server-code-vb"></a>Inicialização de uma janela ModalPopup por meio de código do servidor (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente. No entanto, alguns cenários exigem que a abertura do popup modal seja disparada no lado do servidor.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente. No entanto, alguns cenários exigem que a abertura do popup modal seja disparada no lado do servidor.

## <a name="steps"></a>Etapas

Em primeiro lugar, um controle da Web de botão ASP.NET é necessário para demonstrar como o controle ModalPopup funciona. Adicione esse botão dentro do &lt;formulário&gt; elemento em uma nova página:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

Em seguida, você precisa da marcação para o Popup que deseja criar. Defina-o como um controle de `<asp:Panel>` e certifique-se de que ele inclui um controle de botão. O controle ModalPopup oferece a funcionalidade para fazer esse botão fechar o pop-up; caso contrário, não há uma maneira fácil de deixá-lo desaparecer.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

Em seguida, adicione o controle ModalPopup do kit de ferramentas do ASP.NET AJAX à página. Defina as propriedades para o botão que carrega o controle, o botão que o torna desaparecedo e a ID do Popup real.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

Como com todas as páginas da Web baseadas no ASP.NET AJAX; o Gerenciador de scripts é necessário para carregar as bibliotecas JavaScript necessárias para os diferentes navegadores de destino:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

Execute o exemplo no navegador. Quando você clica no botão, o popup modal é exibido. Para obter o mesmo efeito usando o código do servidor, um novo botão é necessário:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

Como você pode ver, um clique no botão gera um postback e executa o método `ServerButton_Click()` no servidor. Nesse método, uma função JavaScript chamada `launchModal()` é executada para ser exata, a função JavaScript será executada assim que a página for carregada:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

O trabalho de `launchModal()` é exibir o ModalPopup. A função `launchModal()` é executada quando a página HTML completa é carregada. Nesse momento, no entanto, a estrutura do ASP.NET AJAX ainda não foi totalmente carregada. Portanto, a função `launchModal()` apenas define uma variável que o controle ModalPopup deve ser mostrado posteriormente:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

O `pageLoad()` função JavaScript é uma função especial que é executada quando o ASP.NET AJAX tiver sido totalmente carregado. Portanto, adicionamos o código a essa função para mostrar o controle ModalPopup, mas somente se `launchModal()` tiver sido chamado antes:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

A função `$find()` está procurando um elemento nomeado na página e espera a ID do lado do servidor como um parâmetro. Portanto, `$find("mpe")` retorna a representação de cliente do controle ModalPopup; seu método `show()` permite que o pop-up apareça.

[![popup modal aparece quando um dos botões é clicado](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

O popup modal aparece quando um dos botões é clicado ([clique para exibir a imagem em tamanho normal](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](positioning-a-modalpopup-cs.md)
> [Próximo](using-modalpopup-with-a-repeater-control-vb.md)
