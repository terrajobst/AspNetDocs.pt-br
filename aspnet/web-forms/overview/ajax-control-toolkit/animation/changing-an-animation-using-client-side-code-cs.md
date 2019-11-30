---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Alterando uma animação usando o código do ladoC#do cliente () | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação também pode...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 84fc2d6646b89cfabb2193cdfca59462d6d7ef16
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606970"
---
# <a name="changing-an-animation-using-client-side-code-c"></a>Alterar uma animação usando o código do lado do cliente (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação também pode ser alterada usando código JavaScript do lado do cliente personalizado.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação também pode ser alterada usando código JavaScript do lado do cliente personalizado.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

A animação será aplicada a um painel de texto semelhante a este:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

A animação real é iniciada por um botão HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e a `runat="server"`obrigatório:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Observe que não há `<Animations>` nó dentro do controle de `AnimationExtender`. O código JavaScript personalizado é usado para fornecer as animações a serem usadas com o controle.

Assim como acontece com a API de servidor do `AnimationExtender`, não há uma maneira fácil de atribuir uma animação ao extensor ainda. No entanto, o extensor expõe vários métodos para ler e gravar animações registradas com os vários eventos (`OnClick`, `OnLoad`e assim por diante). Estes são alguns exemplos:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

O formato do valor de retorno das funções de `get_*()` e o formato do argumento para as funções de `set_*()` é uma cadeia de caracteres JSON, fornecendo uma representação de objeto do que seria a marcação XML. Atualmente, não há como passar um objeto no, mas é possível ler um objeto de uma determinada animação (`get_OnXXXBehavior()` métodos).

Aqui está uma cadeia de caracteres JSON (sem as aspas delimitadoras e formatada facilmente) que representa uma animação disparada pelo botão, mas animando o painel redimensionando-o e esmaecido ao mesmo tempo:

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

O código JavaScript a seguir atribui esse script JSON à animação `OnClick` do extensor atual e o executa:

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]

[![a animação é executada imediatamente, sem um clique do mouse (e com muito pouca marcação)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

A animação é executada imediatamente, sem um clique do mouse (e com uma pequena marcação) ([clique para exibir a imagem em tamanho normal](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](executing-animations-using-client-side-code-cs.md)
> [Próximo](animating-an-updatepanel-control-cs.md)
