---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Adicionando dinamicamente um painel de acordeão (VB) | Microsoft Docs
author: wenz
description: O controle de acordeão no AJAX Control Toolkit fornece vários painéis e permite que o usuário exiba um deles de cada vez. Os painéis geralmente são declarados com w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: be48db5ea3de4af46b0f864cc9e73d2f518294a4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74607196"
---
# <a name="dynamically-adding-an-accordion-pane-vb"></a>Adicionando dinamicamente um painel de acordeão (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> O controle de acordeão no AJAX Control Toolkit fornece vários painéis e permite que o usuário exiba um deles de cada vez. Os painéis geralmente são declarados dentro da própria página, mas o código do servidor pode ser usado para obter o mesmo resultado.

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

O controle de acordeão no AJAX Control Toolkit fornece vários painéis e permite que o usuário exiba um deles de cada vez. Os painéis geralmente são declarados dentro da própria página, mas o código do servidor pode ser usado para obter o mesmo resultado.

## <a name="steps"></a>Etapas

O controle de acordeão expõe todas as propriedades importantes para o código do lado do servidor. Entre outras coisas, a propriedade `Panes` concede acesso à coleção de painéis que compõem o acordeão. Cada painel é do tipo `AccordionPane`. Portanto, é trivial criar esse painel:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

A propriedade `HeaderContainer` de `AccordionPane` fornece acesso aos controles ASP.NET dentro da seção de cabeçalho do painel; a propriedade `ContentContainer` de `AccordionPane` faz o mesmo para a seção de conteúdo do painel. Isso permite que o código ASP.NET adicione conteúdo aos painéis:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Por fim, os painéis devem ser adicionados à coleção de `Panes` do acordeão:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

Aqui está um código completo do servidor que adiciona dois painéis a um controle de acordeão:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

O único elemento ausente é o próprio acordeão, o que depende da presença do controle de `ScriptManager` ASP.NET:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Para concluir o exemplo, as duas classes CSS referenciadas no controle acordeão fornecem informações de estilo para o navegador:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]

[![os dados no acordeão foram adicionados dinamicamente pelo código do lado do servidor](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Os dados no acordeão foram adicionados dinamicamente pelo código do lado do servidor ([clique para exibir a imagem em tamanho normal](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](databinding-to-an-accordion-vb.md)
