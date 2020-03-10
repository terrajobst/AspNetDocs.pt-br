---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Manipulando postbacks de um ModalPopup (C#) | Microsoft Docs
author: wenz
description: O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente. Deve-se tomar cuidado especial quando um PDV...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 20073d156b4bd5ce67a47d2511b28594b70ce260
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554057"
---
# <a name="handling-postbacks-from-a-modalpopup-c"></a>Tratamento de postbacks de um ModalPopup (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente. Deve-se tomar cuidado especial quando um postback é criado de dentro do pop-up.

## <a name="overview"></a>Visão geral

O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente. Deve-se tomar cuidado especial quando um postback é criado de dentro do pop-up.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado em qualquer lugar na página (mas dentro do elemento `<form>`):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

Em seguida, adicione um painel que serve como o popup modal. Lá, o usuário pode inserir um nome e um endereço de email. Um botão é usado para fechar o pop-up e salvar as informações. Observe que o atributo `OnClick` é definido de forma que um postback ocorra quando esse botão for clicado:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

A página em si consiste em dois rótulos para exatamente as mesmas informações: nome e endereço de email. Um botão é usado para disparar o popup modal:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

Para que o pop-up seja exibido, adicione o controle de `ModalPopupExtender`. Defina o atributo `PopupControlID` como a ID do painel e `TargetControlID` à ID do botão:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

Agora, sempre que o botão de `Save` dentro do popup modal for clicado, o método de `SaveData()` do lado do servidor será executado. Lá, você pode salvar os dados inseridos em um armazenamento de dados. Para simplificar, os novos dados são apenas de saída no rótulo:

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

Além disso, os controles de caixa de texto dentro do popup modal devem ser preenchidos com o nome e o email atuais. No entanto, isso só é necessário quando nenhum postback ocorre. Se houver um postback, o recurso ViewState ASP.NET preencherá automaticamente as caixas de Textcom os valores apropriados.

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]

[![o popup modal causa um postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

O popup modal causa um postback ([clique para exibir a imagem em tamanho normal](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-modalpopup-with-a-repeater-control-cs.md)
> [Próximo](positioning-a-modalpopup-cs.md)
