---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Usando postback automático com CascadingDropDown (C#) | Microsoft Docs
author: wenz
description: O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 8bccd716814e7de544798010cecbc148ec50b5cd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574518"
---
# <a name="using-auto-postback-with-cascadingdropdown-c"></a>Uso de postback automático com CascadingDropDown (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)

> O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em outra DropDownList. No entanto, ao usar o controle CascadingDropDown, o ASP. O recurso AutoPostBack do controle DropDownList da NET não funciona, pois o carregamento de dados de forma assíncrona na lista gera um postback (desnecessário) em si. Com algum código JavaScript, esse efeito pode ser evitado.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em outra DropDownList. (Por exemplo, uma lista fornece uma lista de Estados dos EUA e a próxima lista é então preenchida com cidades principais nesse estado.) No entanto, ao usar o controle CascadingDropDown, o ASP. O recurso AutoPostBack do controle DropDownList da NET não funciona, pois o carregamento de dados de forma assíncrona na lista gera um postback (desnecessário) em si. Com algum código JavaScript, esse efeito pode ser evitado.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado em qualquer lugar na página (mas dentro do elemento &lt;`form`&gt;):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

Em seguida, um controle DropDownList é necessário:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

Para essa lista, um extensor CascadingDropDown é adicionado, fornecendo a URL do serviço Web e informações do método:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

Em seguida, o extensor CascadingDropDown chama assincronamente um serviço Web com a seguinte assinatura de método:

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

O método retorna uma matriz do tipo valor CascadingDropDown. O construtor do tipo espera primeiro a legenda da entrada da lista e, em seguida, o valor (HTML `value` atributo).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

Carregar a página no navegador preencherá a lista suspensa com três fornecedores, a segunda sendo selecionada. Além disso, ASP.NET define o `__doPostBack()` método JavaScript. Depois que a página for carregada, essa chamada JavaScript será adicionada à lista suspensa, mas somente se houver elementos nela. Se não houver nenhum elemento na lista, o kit de ferramentas de controle está carregando-os no momento, portanto, o código JavaScript usa um tempo limite e tenta novamente em um meio segundo.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

Dessa forma, um postback só é executado quando há elementos na lista e o usuário seleciona uma entrada.

[![selecionar um elemento de lista causa um postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)

A seleção de um elemento de lista causa um postback ([clique para exibir a imagem em tamanho normal](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](presetting-list-entries-with-cascadingdropdown-cs.md)
> [Próximo](filling-a-list-using-cascadingdropdown-vb.md)
