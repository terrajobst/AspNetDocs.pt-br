---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Executando animações usando o código do lado do cliente (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A execução da animação...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7ef36900d20d8d07c3c6f3b63ce96568a377a0ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598157"
---
# <a name="executing-animations-using-client-side-code-vb"></a>Executar animações usando o código do lado do cliente (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)

> O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A execução da animação também pode ser disparada usando código JavaScript personalizado do lado do cliente.

## <a name="overview"></a>Visão geral

O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A execução da animação também pode ser disparada usando código JavaScript personalizado do lado do cliente.

## <a name="steps"></a>Etapas

Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

A animação será aplicada a um painel de texto semelhante a este:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e a `runat="server"`obrigatório:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

Dentro do nó `<Animations>`, use `<OnClick>` para executar as animações quando o usuário clicar no painel. Adicione duas animações a serem executadas em paralelo:

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

Para fins de demonstração, essa animação (e qualquer outra animação criada usando o Control Toolkit) é executada usando código JavaScript, depois que a página é executada. Primeiro, precisamos de acesso ao controle de `AnimationExtender`. A biblioteca do ASP.NET AJAX fornece a função `$find()` para esta tarefa:

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

O controle de `AnimationExtender` expõe uma API avançada, incluindo métodos com nomes idênticos aos manipuladores de eventos usados na marcação XML: `OnClick()`, `OnLoad()`e assim por diante. Por exemplo, uma chamada do método `OnClick()` executa a animação dentro do elemento `<OnClick>` do controle de `AnimationExtender`:

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

Aqui está o código JavaScript completo do lado do cliente que emula o clique no painel depois que a página tiver sido totalmente carregada Observe que o nome da função `pageLoad()` é usado pelo AJAX ASP.NET depois que a página e todas as bibliotecas JavaScript incluídas tiverem sido carregadas.

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]

[![a animação é executada imediatamente, sem um clique do mouse](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

A animação é executada imediatamente, sem um clique do mouse ([clique para exibir a imagem em tamanho normal](executing-animations-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](modifying-animations-from-the-server-side-vb.md)
> [Próximo](changing-an-animation-using-client-side-code-vb.md)
