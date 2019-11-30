---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
title: Criação de um controle numérico para cima/para baixo com um backC#-end do serviço Web () | Microsoft Docs
author: wenz
description: Em vez de permitir que um usuário digite um valor em uma caixa de seleção, um controle numérico para cima/para baixo (que existe no Windows e em outros sistemas operacionais) poderia provar mais c...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c99bbc72-d4de-41ed-92a4-9a4632368363
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-cs
msc.type: authoredcontent
ms.openlocfilehash: 816a840b9e93b95a049c3a4cb792e9deeab28983
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598897"
---
# <a name="creating-a-numeric-updown-control-with-a-web-service-backend-c"></a>Criação um controle numérico para cima/para baixo com um back-end de serviço Web (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1CS.pdf)

> Em vez de permitir que um usuário digite um valor em uma caixa de seleção, um controle numérico para cima/para baixo (que existe no Windows e em outros sistemas operacionais) pode provar o mais confortável. Por padrão, o controle NumericUpDown sempre aumenta ou diminui um valor em 1, mas um serviço Web comprova mais flexibilidade.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

Em vez de permitir que um usuário digite um valor em uma caixa de seleção, um controle numérico para cima/para baixo (que existe no Windows e em outros sistemas operacionais) pode provar o mais confortável. Por padrão, o controle de `NumericUpDown` sempre aumenta ou diminui um valor em 1, mas um serviço Web comprova mais flexibilidade.

## <a name="steps"></a>Etapas

O ASP.NET AJAX Control Toolkit contém o `NumericUpDown` Extender, que adiciona automaticamente dois botões a uma caixa de texto: um para aumentar seu valor, um para diminuir. No entanto, o controle também dá suporte a uma chamada de serviço Web (ou chamada de método de página). Sempre que o botão para cima ou para baixo é clicado, o código JavaScript se conecta ao servidor Web e executa um método lá. A assinatura do método é a seguinte:

[!code-csharp[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample1.cs)]

O argumento `current` é o valor atual na caixa de texto; o atributo `tag` são dados de contexto adicionais que podem ser definidos como uma propriedade do extensor `NumericUpDown` (mas não é obrigatório).

Para este exemplo, o controle numérico para cima/para baixo só deve permitir valores que sejam potências de dois: 1, 2, 4, 8, 16, 32, 64 e assim por diante. Portanto, o método executado quando o usuário deseja aumentar o valor deve dobrar o valor antigo; o outro método deve dividir o valor em dois. Aqui está o serviço Web completo:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample2.aspx)]

Por fim, crie uma nova página do ASP.NET. Como de costume, você precisa de um controle de `ScriptManager`, um controle de `TextBox` e um controle de `NumericUpDownExtender`. Para o último, você precisa fornecer as informações do serviço Web:

- `ServiceDownMethod` nome do método da Web ou do método de página inoperante
- `ServiceDownPath` caminho para o serviço Web com o método de serviço inoperante; omitir se você estiver usando um método de página
- `ServiceUpMethod` nome do método ou da página da Web up
- `ServiceUpPath` caminho para o serviço Web com o método de serviço up; omitir se você estiver usando um método de página

Aqui está a marcação completa para a página:

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/samples/sample3.aspx)]

Se você executar a página, observe como o valor na caixa de texto sempre é dobrado quando você clica no botão superior e é dividido quando você clica no botão inferior.

[![apenas números que são uma potência de 2 aparecem](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image1.png)

Somente os números que são uma potência de 2 aparecem ([clique para exibir a imagem em tamanho normal](creating-a-numeric-up-down-control-with-a-web-service-backend-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Avançar](creating-a-numeric-up-down-control-with-a-web-service-backend-vb.md)
