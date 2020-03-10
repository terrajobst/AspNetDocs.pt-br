---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Permitindo apenas determinados caracteres em uma caixa de texto (VB) | Microsoft Docs
author: wenz
description: Os controles de validação ASP.NET podem garantir que apenas determinados caracteres sejam permitidos na entrada do usuário. No entanto, isso ainda não impede que os usuários digitem inválidos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: 895708ebecc30c5f35e6ecd0349604bb777cbd93
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613508"
---
# <a name="allowing-only-certain-characters-in-a-text-box-vb"></a>Permitir somente determinados caracteres em uma caixa de texto (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> Os controles de validação ASP.NET podem garantir que apenas determinados caracteres sejam permitidos na entrada do usuário. No entanto, isso ainda não impede que os usuários digitem caracteres inválidos e tentem enviar o formulário.

## <a name="overview"></a>Visão geral

Os controles de validação ASP.NET podem garantir que apenas determinados caracteres sejam permitidos na entrada do usuário. No entanto, isso ainda não impede que os usuários digitem caracteres inválidos e tentem enviar o formulário.

## <a name="steps"></a>Etapas

O kit de ferramentas de controle AJAX ASP.NET contém o controle de `FilteredTextBox` que estende uma caixa de texto. Depois de ativado, apenas um determinado conjunto de caracteres pode ser inserido no campo.

Para que isso funcione, primeiro precisamos do `ScriptManager` AJAX ASP.NET que carrega as bibliotecas JavaScript que também são usadas pelo ASP.NET AJAX Control Toolkit:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

Em seguida, precisamos de uma caixa de texto:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

Por fim, o controle de `FilteredTextBoxExtender` cuida da restrição dos caracteres que o usuário tem permissão para digitar. Primeiro, defina o atributo `TargetControlID` como o `ID` do controle de `TextBox`. Em seguida, escolha um dos valores de `FilterType` disponíveis:

- `Custom` padrão; Você precisa fornecer uma lista de caracteres válidos
- `LowercaseLetters` apenas letras minúsculas
- somente `Numbers` dígitos
- `UppercaseLetters` apenas letras maiúsculas

Se o `Custom FilterType` for usado, a propriedade `ValidChars` deverá ser definida e fornecerá uma lista de caracteres que podem ser digitados. A propósito: se você tentar colar o texto na caixa de texto, todos os caracteres inválidos serão removidos.

Aqui está a marcação para o controle de `FilteredTextBoxExtender` que permite apenas dígitos (algo que também teria sido possível com `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

Execute a página e tente inserir uma letra se o JavaScript estiver habilitado, ele não funcionará; no entanto, os dígitos aparecem na página. No entanto, observe que a proteção que o `FilteredTextBox` fornece não faz a prova de marcadores: se o JavaScript estiver habilitado, todos os dados poderão ser inseridos na caixa de texto, portanto, você precisará usar a validação adicional, ou seja, ASP. Controles de validação de rede.

[![somente dígitos podem ser inseridos](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

Somente os dígitos podem ser inseridos ([clique para exibir a imagem em tamanho normal](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](allowing-only-certain-characters-in-a-text-box-cs.md)
