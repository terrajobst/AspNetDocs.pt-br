---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Recolhendo e expandindo um painel do JavaScriptC#() | Microsoft Docs
author: wenz
description: O controle CollapsiblePanel no kit de ferramentas de controle do ASP.NET AJAX estende um painel e fornece a capacidade de recolher seu conteúdo e expandi-lo...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: bed14d82394d28336493bec10e31ddb4d411a192
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599455"
---
# <a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>Recolher e expandir um painel de JavaScript (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> O controle CollapsiblePanel no kit de ferramentas de controle do ASP.NET AJAX estende um painel e fornece a capacidade de recolher seu conteúdo e expandi-lo novamente. Essas duas ações também podem ser disparadas a partir do código JavaScript personalizado.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O controle CollapsiblePanel no kit de ferramentas de controle do ASP.NET AJAX estende um painel e fornece a capacidade de recolher seu conteúdo e expandi-lo novamente. Essas duas ações também podem ser disparadas a partir do código JavaScript personalizado.

## <a name="steps"></a>Etapas

Em primeiro lugar, crie uma nova página ASP.NET e inclua o `ScriptManager` dentro do elemento um `<form>`. Isso carrega a biblioteca do ASP.NET AJAX que é exigida pelo kit de ferramentas de controle:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

Em seguida, crie um painel com texto para que o efeito recolher/expandir possa ser visto:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

Como você pode ver, o painel faz referência a uma classe CSS que é mostrada aqui (e basicamente define uma cor de plano de fundo e a largura do painel):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

O controle `CollapsiblePanelExtender` requer o atributo `TargetControlID` para que o kit de ferramentas saiba qual painel deve ser recolhido ou expandido após a solicitação:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

Infelizmente, o extensor atualmente não expõe uma API específica para recolher ou expandir o painel, mas alguns métodos não documentados farão isso. Em primeiro lugar, adicione três botões HTML à página, o que irá disparar o JavaScript do lado do cliente para recolher ou expandir o conteúdo do painel:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

No código JavaScript do lado do cliente (iniciado com `<script type="text/javascript">`), o método `$find()` precisa ser usado para acessar o `CollapsiblePanelExtender`. `$find("cpe")` retornará uma referência a ele. A partir daí, métodos específicos resolverão a tarefa em questão.

O método para abrir (expandir) o painel é chamado de `_doOpen()`; o código a seguir implementa a função `doOpen()` chamada quando o primeiro botão é clicado:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

Para fechar ou recolher o painel, o método `_doClose()` precisa ser executado. Assim, quando o usuário clica no segundo botão, o seguinte código JavaScript é chamado:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

O terceiro botão alterna o estado do painel: de recolhido para expandido, e vice-versa. O `CollapsiblePanelExtender` expõe o método `toggle()` que faz exatamente isso: reverte o estado do painel. No entanto, há também outra abordagem (que é usada internamente pelo método `toggle()`): o método `get_Collapsed()` da `CollapsiblePanelExtender()` nos informa se o painel está recolhido ou não. Dependendo do valor de retorno dessa função, o painel será expandido (método`_doOpen()`) ou o método recolhido (`_doClose()`):

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]

[![o terceiro botão altera o estado do painel: de recolhido para expandido e para trás](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

O terceiro botão altera o estado do painel: de recolhido para expandido e de volta ([clique para exibir a imagem em tamanho normal](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Avançar](collapsing-and-expanding-a-panel-from-javascript-vb.md)
