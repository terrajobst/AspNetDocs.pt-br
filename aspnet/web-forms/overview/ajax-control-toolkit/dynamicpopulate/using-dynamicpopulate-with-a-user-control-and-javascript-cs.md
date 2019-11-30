---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Usando DynamicPopulate com um controle de usuário e JavaScriptC#() | Microsoft Docs
author: wenz
description: O controle DynamicPopulate no kit de ferramentas de controle AJAX ASP.NET chama um serviço Web (ou método de página) e preenche o valor resultante em um controle de destino em t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: a0e6d04a5f62ab558aceb8302d94d3bf2dc8a39f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599185"
---
# <a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a>Uso de DynamicPopulate com um controle de usuário e o JavaScript (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)

> O controle DynamicPopulate no kit de ferramentas de controle AJAX ASP.NET chama um serviço Web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Também é possível disparar a população usando código JavaScript do lado do cliente personalizado. No entanto, é preciso tomar cuidado especial quando o extensor reside em um controle de usuário.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O controle de `DynamicPopulate` no ASP.NET AJAX Control Toolkit chama um serviço Web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Também é possível disparar a população usando código JavaScript do lado do cliente personalizado. No entanto, é preciso tomar cuidado especial quando o extensor reside em um controle de usuário.

## <a name="steps"></a>Etapas

Em primeiro lugar, você precisa de um serviço Web ASP.NET que implementa o método a ser chamado pelo controle de `DynamicPopulateExtender`. O serviço Web implementa o método `getDate()` que espera um argumento do tipo cadeia de caracteres, chamado `contextKey`, pois o controle de `DynamicPopulate` envia uma parte das informações de contexto com cada chamada de serviço da Web. Este é o código (`DynamicPopulate.cs.asmx`de arquivo) que recupera a data atual em um dos três formatos:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

Na próxima etapa, crie um novo controle de usuário (`.ascx` arquivo), indicado pela seguinte declaração em sua primeira linha:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

Um &lt;`label`elemento &gt; será usado para exibir os dados provenientes do servidor.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

Também no arquivo de controle de usuário, usaremos três botões de opção, cada um representando um dos três formatos de data possíveis com suporte do serviço Web. Quando o usuário clicar em um dos botões de opção, o navegador executará o código JavaScript, que tem esta aparência:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

Esse código acessa o `DynamicPopulateExtender` (não se preocupe com a ID estranha ainda, isso será abordado posteriormente) e acionará a população dinâmica com os dados. No contexto do botão de opção atual, `this.value` se refere a seu valor que é `format1`, `format2` ou `format3` exatamente o que o método da Web espera.

A única coisa ausente no controle de usuário ainda é o controle de `DynamicPopulateExtender` que vincula os botões de opção ao serviço Web.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

Novamente, você pode observar a ID estranha usada no controle: `mcd1$myDate` em vez de `myDate`. Anteriormente, o código JavaScript usava `mcd1_dpe1` para acessar o `DynamicPopulateExtender` em vez de `dpe1`. Essa estratégia de nomenclatura é um requisito especial ao usar `DynamicPopulateExtender` dentro de um controle de usuário. Além disso, você precisa inserir o controle de usuário de uma maneira específica para fazer tudo funcionar. Crie uma nova página ASP.NET e registre um prefixo de marca para o controle de usuário que você acabou de implementar:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

Em seguida, inclua o controle `ScriptManager` AJAX ASP.NET na nova página:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

Por fim, adicione o controle de usuário à página. Você só precisa definir seu atributo `ID` (e `runat="server"`, é claro), mas também precisa defini-lo como um nome específico: `mcd1` uma vez que esse é o prefixo usado no controle de usuário para acessá-lo usando JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

E pronto. A página se comporta conforme o esperado: um usuário clica em um dos botões de opção, o controle no kit de ferramentas chama o serviço Web e exibe a data atual no formato desejado.

[![os botões de opção residem em um controle de usuário](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)

Os botões de opção residem em um controle de usuário ([clique para exibir a imagem em tamanho normal](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](dynamically-populating-a-control-using-javascript-code-cs.md)
> [Próximo](dynamically-populating-a-control-vb.md)
