---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Modificando animações do lado do servidorC#() | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações também podem...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 0594efea9598a6c2461a72f789b5bd5f8ece23da
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575165"
---
# <a name="modifying-animations-from-the-server-side-c"></a>Modificando animações do lado do servidorC#()

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações também podem ser alteradas no lado do servidor

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações também podem ser alteradas no lado do servidor

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

A animação será aplicada a um painel de texto semelhante a este:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

O restante do código é executado no lado do servidor e não usa marcação; em vez disso, ele usa código para criar o controle de `AnimationExtender`:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

No entanto, o kit de ferramentas de controle atualmente não fornece um acesso de API para criar as animações individuais. No entanto, é possível definir a propriedade animações do `AnimationExtender`como uma cadeia de caracteres que contém a marcação XML usada ao atribuir as animações de forma declarativa. Para criar o XML que não deve conter o elemento `<Animations>`, você pode usar o suporte a XML do .NET Framework ou, como no código a seguir, basta fornecer a cadeia de caracteres:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

Por fim, adicione o controle de `AnimationExtender` à página atual, dentro do elemento `<form runat="server">`, certificando-se de que a animação está incluída e é executada:

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]

[![a animação é criada usando o código de C#/vb do lado do servidor](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

A animação é criada usando o código de C#/vb do lado do servidor ([clique para exibir a imagem em tamanho normal](modifying-animations-from-the-server-side-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](triggering-an-animation-in-another-control-cs.md)
> [Próximo](executing-animations-using-client-side-code-cs.md)
