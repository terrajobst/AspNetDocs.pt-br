---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: Usando o extensor de controleC#ColorPicker () | Microsoft Docs
author: microsoft
description: ColorPicker é um extensor AJAX ASP.NET que fornece funcionalidade de separação de cores do lado do cliente com a interface do usuário em um controle Popup. Ele pode ser anexado a qualquer ASP.NET...
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: ac510ab353878038c1c7a103bfbf6d32fb1b2686
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614040"
---
# <a name="using-the-colorpicker-control-extender-c"></a>Usando o extensor de controleC#ColorPicker ()

pela [Microsoft](https://github.com/microsoft)

> ColorPicker é um extensor AJAX ASP.NET que fornece funcionalidade de separação de cores do lado do cliente com a interface do usuário em um controle Popup. Ele pode ser anexado a qualquer controle de caixa de texto ASP.NET. Fosse.

O objetivo deste tutorial é explicar como você pode usar o extensor do controle ColorPicker do AJAX Control Toolkit. O extensor de controle ColorPicker exibe uma caixa de diálogo pop-up que permite que você selecione uma cor. O ColorPicker é útil sempre que você deseja fornecer uma interface do usuário intuitiva para que um usuário escolha uma cor.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Estendendo um controle TextBox com o extensor de controle ColorPicker

Imagine, por exemplo, que você deseja criar um site que permita que os visitantes criem cartões de visita personalizados. Os visitantes podem inserir o texto de um cartão de visita e escolher a cor. A página ASP.NET na Listagem 1 contém dois controles TextBox chamados txtCardText e txtCardColor. Quando você envia o formulário, os valores selecionados são exibidos (veja a Figura 1).

[![formulário simples para criar um cartão de visita](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)

**Figura 01**: formulário simples para criar um cartão de visita ([clique para exibir a imagem em tamanho normal](using-the-colorpicker-control-extender-cs/_static/image2.png))

**Listagem 1-createcard. aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

O formulário na Listagem 1 funciona, mas não fornece uma excelente experiência do usuário. O usuário precisa digitar uma cor na caixa de texto. Se o usuário quiser uma cor especializada, por exemplo, apenas o tom certo de Pea Green, o usuário deverá descobrir o código de cor HTML sem qualquer ajuda.

Você pode usar o extensor de controle ColorPicker para criar uma melhor experiência do usuário. O ColorPicker exibe uma caixa de diálogo de cores quando você move o foco para um controle TextBox (veja a Figura 2).

[![o extensor de controle ColorPicker](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)

**Figura 02**: o extensor de controle ColorPicker ([clique para exibir a imagem em tamanho normal](using-the-colorpicker-control-extender-cs/_static/image4.png))

Você precisa concluir duas etapas para usar o extensor de controle ColorPicker com o formulário na Listagem 1:

1. Adicionar um controle ScriptManager à página
2. Adicionar o extensor de controle ColorPicker à página

Antes de poder usar o ColorPicker, você deve adicionar um ScriptManager à sua página. Um bom local para adicionar o ScriptManager está logo abaixo do formulário de &lt;de abertura do lado do servidor&gt; marca. Você pode arrastar o ScriptManager para a página da caixa de ferramentas (o ScriptManager está localizado na guia extensões AJAX). Como alternativa, você pode digitar a seguinte marca no modo de exibição de origem abaixo da marca de formulário de abertura do lado do servidor:

&lt;ASP: ScriptManager ID = "ScriptManager1" runat = "Server"/&gt;

A maneira mais fácil de adicionar o extensor de controle ColorPicker à página está no modo Design. Se você passar o mouse sobre a caixa de texto txtCardColor, uma opção Smart Task será exibida, permitindo que você adicione um extensor (consulte a Figura 3). Se você escolher essa opção, o assistente de extensor será exibido (consulte a Figura 4).

[![adicionar um extensor](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)

**Figura 03**: adicionando um extensor ([clique para exibir a imagem em tamanho normal](using-the-colorpicker-control-extender-cs/_static/image6.png))

[![selecionar um extensor de controle com o assistente do extensor](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)

**Figura 04**: selecionando um extensor de controle com o assistente do extensor ([clique para exibir a imagem em tamanho normal](using-the-colorpicker-control-extender-cs/_static/image8.png))

Você pode escolher o extensor de ColorPicker para estender a caixa de texto txtCardColor com o extensor de ColorPicker. Clique em OK para fechar o diálogo.

Depois de fazer essas alterações, a origem da página é semelhante à listagem 2.

Listagem 2-createcard. aspx (com ColorPicker)

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

Observe que agora a página contém um controle ColorPickerExtender que aparece diretamente abaixo do controle TextBox txtCardColor. O controle ColorPickerExtender estende o controle txtCardColor para que ele exiba uma caixa de diálogo Seletor de cores.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Usando um botão para iniciar a caixa de diálogo do seletor de cores

O extensor de ColorPicker dá suporte às seguintes propriedades:

- PopupButtonId-a ID de um botão na página que faz com que a caixa de diálogo do seletor de cores apareça.
- PopupPosition-a posição, relativa ao controle de destino, da caixa de diálogo Seletor de cores. Os valores possíveis são absoluta, Center, BottomLeft, BottomRight, TopLeft, canto superior, Right e Left (o padrão é BottomLeft).
- SampleControlId-a ID de um controle que exibe a cor selecionada.
- SelectedColor-a cor inicial selecionada pelo ColorPicker.

Você pode usar essas propriedades para personalizar como a caixa de diálogo Seletor de cores é exibida e como a cor selecionada é exibida. A página na Listagem 3 ilustra como você pode usar várias dessas propriedades.

**Listagem 3-CreateCardButton. aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

A página na Listagem 3 inclui um botão Selecionar cor (veja a Figura 5). Quando você clica nesse botão, a caixa de diálogo Seletor de cores é exibida acima da caixa de texto. Se você selecionar uma cor da caixa de diálogo, a cor selecionada aparecerá como a cor do plano de fundo do controle de rótulo lblSample.

A propriedade ColorPicker PopupButtonID é usada para associar o botão de cor de seleção ao extensor de ColorPicker. Quando você fornece um valor para a propriedade PopupButtonID, a caixa de diálogo Seletor de cores não é mais exibida quando o controle de destino tem foco. Você deve clicar no botão para exibir a caixa de diálogo.

A propriedade SampleControlID é usada para associar um controle que exibe a cor selecionada com o ColorPicker. O ColorPicker altera a cor do plano de fundo desse controle para a cor atualmente selecionada.

[![exibir a caixa de diálogo Seletor de cores com um botão](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)

**Figura 05**: exibindo a caixa de diálogo Seletor de cores com um botão ([clique para exibir a imagem em tamanho normal](using-the-colorpicker-control-extender-cs/_static/image10.png))

## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu a usar o extensor de controle ColorPicker para exibir uma caixa de diálogo Seletor de cores pop-up. Primeiro, examinamos como você pode exibir a caixa de diálogo quando o foco é movido para um controle TextBox. Em seguida, você aprendeu a criar um botão que exibe a caixa de diálogo Seletor de cores quando o botão é clicado.

> [!div class="step-by-step"]
> [Próximo](using-the-colorpicker-control-extender-vb.md)
