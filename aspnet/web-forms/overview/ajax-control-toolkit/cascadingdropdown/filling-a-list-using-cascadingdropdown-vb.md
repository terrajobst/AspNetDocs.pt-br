---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Preenchendo uma lista usando CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 8dd9ef8a4bdf705ba4451b7fd240e4de8618221c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536004"
---
# <a name="filling-a-list-using-cascadingdropdown-vb"></a>Preencher uma lista usando o CascadingDropDown (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)

> O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em outra DropDownList. (Por exemplo, uma lista fornece uma lista de Estados dos EUA e a próxima lista é então preenchida com cidades principais nesse estado.) O primeiro desafio a ser resolvido é realmente preencher uma lista suspensa usando esse controle.

## <a name="overview"></a>Visão geral

O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em outra DropDownList. (Por exemplo, uma lista fornece uma lista de Estados dos EUA e a próxima lista é então preenchida com cidades principais nesse estado.) O primeiro desafio a ser resolvido é realmente preencher uma lista suspensa usando esse controle.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado em qualquer lugar na página (mas dentro do elemento `<form>`):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

Em seguida, um controle DropDownList é necessário:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

Para essa lista, um extensor CascadingDropDown é adicionado. Ele enviará uma solicitação assíncrona para um serviço Web que retornará uma lista de entradas a serem exibidas na lista. Para que isso funcione, os seguintes atributos CascadingDropDown precisam ser definidos:

- `ServicePath`: URL de um serviço Web que fornece as entradas da lista
- `ServiceMethod`: método Web fornecendo as entradas da lista
- `TargetControlID`: ID da lista suspensa
- `Category`: informações de categoria que são enviadas para o método Web quando chamado
- `PromptText`: texto exibido ao carregar dados da lista de forma assíncrona do servidor

Aqui está a marcação para o elemento `CascadingDropDown`. A única diferença entre C# o e o VB é o nome do serviço Web associado:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

O código JavaScript proveniente do `CascadingDropDown` Extender chama um método de serviço Web com a seguinte assinatura:

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

Portanto, o aspecto importante é que o método precisa retornar uma matriz do tipo `CascadingDropDownNameValue` (definida pelo ASP.NET AJAX Control Toolkit). No Construtor `CascadingDropDownNameValue`, primeiro o texto da entrada da lista e, em seguida, seu valor deve ser fornecido, assim como `<option value="VALUE">NAME</option>` faria no HTML. Aqui estão alguns dados de exemplo:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

Carregar a página no navegador irá disparar a lista a ser preenchida com três fornecedores.

[![a lista é preenchida automaticamente](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)

A lista é preenchida automaticamente ([clique para exibir a imagem em tamanho normal](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-auto-postback-with-cascadingdropdown-cs.md)
> [Próximo](using-cascadingdropdown-with-a-database-vb.md)
