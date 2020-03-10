---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: Usando TextBoxWatermark com controles de validação (VB) | Microsoft Docs
author: wenz
description: O controle TextBoxWatermark no AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa. Quando um usuário clica na caixa, ele...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 141cae26c9e52be510e2a5a8f816cbac977dcf3d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627039"
---
# <a name="using-textboxwatermark-with-validation-controls-vb"></a>Uso de TextBoxWatermark com controles de validação (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)

> O controle TextBoxWatermark no AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa. Quando um usuário clica na caixa, ele é esvaziado. Se o usuário deixar a caixa sem inserir o texto, o texto preenchedo reaparecerá. Isso pode colidir com controles de validação ASP.NET na mesma página, mas esses problemas podem ser superados.

## <a name="overview"></a>Visão geral

O controle de `TextBoxWatermark` no AJAX Control Toolkit estende uma caixa de texto para que um texto seja exibido dentro da caixa. Quando um usuário clica na caixa, ele é esvaziado. Se o usuário deixar a caixa sem inserir o texto, o texto preenchedo reaparecerá. Isso pode colidir com controles de validação ASP.NET na mesma página, mas esses problemas podem ser superados.

## <a name="steps"></a>Etapas

A configuração básica do exemplo é a seguinte: um controle de `TextBox` é marca d' água usando um controle de `TextBoxWatermarkExtender`. Um botão dispara um postback e posteriormente será usado para disparar os controles de validação na página. Além disso, um controle de `ScriptManager` é necessário para inicializar o ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

Agora, adicione um controle de `RequiredFieldValidator` que verifica se há texto no campo quando o formulário é enviado. A propriedade `InitialValue` do validador deve ser definida com o mesmo valor usado no controle de `TextBoxWatermarkExtender`: quando o formulário é enviado, o valor de uma caixa de texto inalterada é o valor de marca d' água dentro dela:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

No entanto, há um problema com essa abordagem: se o cliente desabilitar o JavaScript, o campo de texto não será preenchido com o texto de marca d' água, portanto, o `RequiredFieldValidator` não disparará uma mensagem de erro. Portanto, um segundo controle de `RequiredFieldValidator` é necessário, que verifica se há uma caixa de texto vazia (omitindo o atributo `InitialValue`).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

Como ambos os validadores usam `Display`=`"Dynamic"`, o usuário final não pode distinguir da aparência visual quais dos dois validadores foram acionados; em vez disso, parece que havia apenas um deles.

Por fim, adicione um código do lado do servidor para gerar o texto no campo se nenhum validador emitiu uma mensagem de erro:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]

[![o validador reclama que não há texto no campo](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)

O validador reclama que não há texto no campo ([clique para exibir a imagem em tamanho normal](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-textboxwatermark-in-a-formview-vb.md)
