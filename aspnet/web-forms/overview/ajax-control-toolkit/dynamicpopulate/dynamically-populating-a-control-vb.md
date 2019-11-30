---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Populando dinamicamente um controle (VB) | Microsoft Docs
author: wenz
description: O controle DynamicPopulate no kit de ferramentas de controle AJAX ASP.NET chama um serviço Web (ou método de página) e preenche o valor resultante em um controle de destino em t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d11320f1f89bb69afe5f62751574079716124da0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599386"
---
# <a name="dynamically-populating-a-control-vb"></a>Preenchimento dinâmico de um controle (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> O controle DynamicPopulate no kit de ferramentas de controle AJAX ASP.NET chama um serviço Web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O controle de `DynamicPopulate` no ASP.NET AJAX Control Toolkit chama um serviço Web (ou método de página) e preenche o valor resultante em um controle de destino na página, sem uma atualização de página. Este tutorial mostra como configurar isso.

## <a name="steps"></a>Etapas

Em primeiro lugar, você precisa de um serviço Web ASP.NET que implementa o método a ser chamado por `DynamicPopulate`. A classe de serviço Web requer o atributo `ScriptService` que é definido no `Microsoft.Web.Script.Services`; caso contrário, o ASP.NET AJAX não poderá criar o proxy JavaScript do lado do cliente para o serviço Web que, por sua vez, é exigido pelo `DynamicPopulate`.

O método Web deve esperar um argumento do tipo cadeia de caracteres, chamado `contextKey`, uma vez que o controle de `DynamicPopulate` envia uma parte das informações de contexto com cada chamada de serviço da Web. O serviço Web a seguir retorna a data atual em um formato representado pelo argumento `contextKey`:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

O serviço Web é salvo como `DynamicPopulate.vb.asmx`. Como alternativa, você pode implementar o método `getDate()` como um método de página na página ASP.NET real com o controle `DynamicPopulate`.

Na próxima etapa, crie um novo arquivo ASP.NET. Como sempre, a primeira etapa é incluir o `ScriptManager` na página atual para carregar a biblioteca do ASP.NET AJAX e fazer com que o kit de ferramentas de controle funcione:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

Em seguida, adicione um controle rótulo (por exemplo, usando o controle HTML de mesmo nome ou o &lt;`asp:Label` /&gt; controle da Web) que mostrará posteriormente o resultado da chamada do serviço Web.

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

Um botão HTML (como um controle HTML, já que não precisamos de um postback para o servidor), será usado para disparar a população dinâmica:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

Finalmente, precisamos do controle de `DynamicPopulateExtender` para conectar as coisas. Os seguintes atributos serão definidos (além dos óbvios, `ID` e `runat`=`"server"`):

- `TargetControlID` onde colocar o resultado da chamada de serviço Web
- `ServicePath` caminho para o serviço Web (Omita se você quiser usar um método de página)
- `ServiceMethod` nome do método da Web ou do método de página
- `ContextKey` informações de contexto a serem enviadas para o serviço Web
- `PopulateTriggerControlID` elemento que dispara a chamada de serviço Web
- `ClearContentsDuringUpdate` se o elemento de destino deve ser esvaziado durante a chamada do serviço Web

Como você pode ver, o controle requer algumas informações, mas colocar tudo em vigor é muito simples. Aqui está a marcação para o controle de `DynamicPopulateExtender` no cenário atual:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

Execute a página ASP.NET no navegador e clique no botão; Você receberá a data atual no formato mês-dia-ano.

[![um clique no botão recupera a data do servidor](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

Um clique no botão recupera a data do servidor ([clique para exibir a imagem em tamanho normal](dynamically-populating-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [Próximo](dynamically-populating-a-control-using-javascript-code-vb.md)
