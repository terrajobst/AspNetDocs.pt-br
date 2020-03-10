---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Populando dinamicamente um controle usando códigoC#JavaScript () | Microsoft Docs
author: wenz
description: O controle DynamicPopulate no kit de ferramentas de controle AJAX ASP.NET chama um serviço Web (ou método de página) e preenche o valor resultante em um controle de destino em t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 24dc358427dec3ffcba16d00041c9a2db657e7e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535738"
---
# <a name="dynamically-populating-a-control-using-javascript-code-c"></a>Preenchimento dinâmico de um controle com código de JavaScript (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> O controle DynamicPopulate no kit de ferramentas de controle AJAX ASP.NET chama um serviço Web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Também é possível disparar a população usando código JavaScript do lado do cliente personalizado.

## <a name="overview"></a>Visão geral

O controle de `DynamicPopulate` no ASP.NET AJAX Control Toolkit chama um serviço Web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Também é possível disparar a população usando código JavaScript do lado do cliente personalizado.

## <a name="steps"></a>Etapas

Em primeiro lugar, você precisa de um serviço Web ASP.NET que implementa o método a ser chamado pelo controle de `DynamicPopulateExtender`. O serviço Web implementa o método `getDate()` que espera um argumento do tipo cadeia de caracteres, chamado `contextKey`, pois o controle de `DynamicPopulate` envia uma parte das informações de contexto com cada chamada de serviço da Web. Este é o código (`DynamicPopulate.cs.asmx`de arquivo) que recupera a data atual em um dos três formatos:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

Na próxima etapa, crie um novo site do ASP.NET e comece com o controle do ScriptManager do ASP.NET AJAX:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

Em seguida, adicione um controle rótulo (por exemplo, usando o controle HTML de mesmo nome ou o `<asp:Label />` controle da Web), que mostrará posteriormente o resultado da chamada do serviço Web.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

Em seguida, inclua um controle de `DynamicPopulateExtender` e forneça informações de serviço Web, controle de destino, mas não o nome do controle que dispara a população que isso será feito posteriormente, usando JavaScript personalizado!

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

Agora, para a parte JavaScript. A função `$find()`, definida pela biblioteca ASP.NET AJAX, retorna uma referência a objetos do lado do servidor do ASP.NET AJAX Control Toolkit, como `DynamicPopulateExtender`. No arquivo atual, `$find("dpe")` retorna uma referência ao controle de um `DynamicPopulateExtender` na página. Ele expõe um método chamado `populate()` que dispara o processo de população dinâmica. O método `populate()` requer um argumento: a chave de contexto que servirá como argumento para o método Web `getDate()`. Por exemplo, `$find("dpe").populate("format1")` preencheria o rótulo com a data atual no formato mês-dia-ano.

Para tornar o exemplo um pouco mais flexível, o usuário pode agora escolher entre vários formatos de data. Para cada um deles, um botão de opção é exibido. Quando o usuário clica em um botão de opção, o código JavaScript preenche dinamicamente o rótulo com o formato de data selecionado. Estes são os botões de opção:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Observe que, dentro do contexto de um botão de opção, a expressão JavaScript `this.value` se refere ao valor do botão atual, que é exatamente a mesma informação com a qual o método `getDate()` pode trabalhar.

[![um clique no botão recupera a data do servidor, no formato especificado](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Um clique no botão recupera a data do servidor, no formato especificado ([clique para exibir a imagem em tamanho normal](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](dynamically-populating-a-control-cs.md)
> [Próximo](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
