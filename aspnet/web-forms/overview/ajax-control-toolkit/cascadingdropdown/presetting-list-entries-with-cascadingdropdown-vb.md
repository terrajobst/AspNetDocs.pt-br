---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Predefinindo entradas da lista com CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 58d675993777f9dcbe0ce1890a60046c91ee8907
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599528"
---
# <a name="presetting-list-entries-with-cascadingdropdown-vb"></a>Predefinição de entradas de lista com CascadingDropDown (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)

> O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em outra DropDownList. Com um pouco de código, é possível que um elemento de lista seja preselecionado quando os dados tiverem sido carregados dinamicamente.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O controle CascadingDropDown no AJAX Control Toolkit estende um controle DropDownList para que as alterações em uma DropDownList carreguem valores associados em outra DropDownList. (Por exemplo, uma lista fornece uma lista de Estados dos EUA e a próxima lista é então preenchida com cidades principais nesse estado.) Com um pouco de código, é possível que um elemento de lista seja preselecionado quando os dados tiverem sido carregados dinamicamente.

## <a name="steps"></a>Etapas

Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado em qualquer lugar na página (mas dentro do elemento `<form>`):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

Em seguida, um controle DropDownList é necessário:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

Para essa lista, um extensor CascadingDropDown é adicionado, fornecendo a URL do serviço Web e informações do método:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

Em seguida, o extensor CascadingDropDown chama assincronamente um serviço Web com a seguinte assinatura de método:

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

O método retorna uma matriz do tipo valor CascadingDropDown. O construtor do tipo espera primeiro a legenda da entrada da lista e, em seguida, o valor (HTML `value` atributo). Se o terceiro argumento for definido como true, o elemento List será selecionado automaticamente no navegador.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

Carregar a página no navegador preencherá a lista suspensa com três fornecedores, a segunda sendo selecionada.

[![a lista é preenchida e preselecionada automaticamente](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)

A lista é preenchida e preselecionada automaticamente ([clique para exibir a imagem em tamanho normal](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-cascadingdropdown-with-a-database-vb.md)
> [Próximo](using-auto-postback-with-cascadingdropdown-vb.md)
