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
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74573929"
---
# <a name="allowing-only-certain-characters-in-a-text-box-vb"></a><span data-ttu-id="63307-104">Permitir somente determinados caracteres em uma caixa de texto (VB)</span><span class="sxs-lookup"><span data-stu-id="63307-104">Allowing Only Certain Characters in a Text Box (VB)</span></span>

<span data-ttu-id="63307-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="63307-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="63307-106">[Baixar código](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="63307-106">[Download Code](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)</span></span>

> <span data-ttu-id="63307-107">Os controles de validação ASP.NET podem garantir que apenas determinados caracteres sejam permitidos na entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="63307-107">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="63307-108">No entanto, isso ainda não impede que os usuários digitem caracteres inválidos e tentem enviar o formulário.</span><span class="sxs-lookup"><span data-stu-id="63307-108">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="overview"></a><span data-ttu-id="63307-109">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="63307-109">Overview</span></span>

<span data-ttu-id="63307-110">Os controles de validação ASP.NET podem garantir que apenas determinados caracteres sejam permitidos na entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="63307-110">ASP.NET validation controls can ensure that only certain characters are allowed in user input.</span></span> <span data-ttu-id="63307-111">No entanto, isso ainda não impede que os usuários digitem caracteres inválidos e tentem enviar o formulário.</span><span class="sxs-lookup"><span data-stu-id="63307-111">However this still does not prevent users from typing invalid characters and trying to submit the form.</span></span>

## <a name="steps"></a><span data-ttu-id="63307-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="63307-112">Steps</span></span>

<span data-ttu-id="63307-113">O kit de ferramentas de controle AJAX ASP.NET contém o controle de `FilteredTextBox` que estende uma caixa de texto.</span><span class="sxs-lookup"><span data-stu-id="63307-113">The ASP.NET AJAX Control Toolkit contains the `FilteredTextBox` control which extends a text box.</span></span> <span data-ttu-id="63307-114">Depois de ativado, apenas um determinado conjunto de caracteres pode ser inserido no campo.</span><span class="sxs-lookup"><span data-stu-id="63307-114">Once activated, only a certain set of characters may be entered into the field.</span></span>

<span data-ttu-id="63307-115">Para que isso funcione, primeiro precisamos do `ScriptManager` AJAX ASP.NET que carrega as bibliotecas JavaScript que também são usadas pelo ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="63307-115">For this to work, we first need as usual the ASP.NET AJAX `ScriptManager` which loads the JavaScript libraries which are also used by the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

<span data-ttu-id="63307-116">Em seguida, precisamos de uma caixa de texto:</span><span class="sxs-lookup"><span data-stu-id="63307-116">Then, we need a text box:</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

<span data-ttu-id="63307-117">Por fim, o controle de `FilteredTextBoxExtender` cuida da restrição dos caracteres que o usuário tem permissão para digitar.</span><span class="sxs-lookup"><span data-stu-id="63307-117">Finally, the `FilteredTextBoxExtender` control takes care of restricting the characters the user is allowed to type.</span></span> <span data-ttu-id="63307-118">Primeiro, defina o atributo `TargetControlID` como o `ID` do controle de `TextBox`.</span><span class="sxs-lookup"><span data-stu-id="63307-118">First, set the `TargetControlID` attribute to the `ID` of the `TextBox` control.</span></span> <span data-ttu-id="63307-119">Em seguida, escolha um dos valores de `FilterType` disponíveis:</span><span class="sxs-lookup"><span data-stu-id="63307-119">Then, choose one of the available `FilterType` values:</span></span>

- <span data-ttu-id="63307-120">`Custom` padrão; Você precisa fornecer uma lista de caracteres válidos</span><span class="sxs-lookup"><span data-stu-id="63307-120">`Custom` default; you have to provide a list of valid chars</span></span>
- <span data-ttu-id="63307-121">`LowercaseLetters` apenas letras minúsculas</span><span class="sxs-lookup"><span data-stu-id="63307-121">`LowercaseLetters` lowercase letters only</span></span>
- <span data-ttu-id="63307-122">somente `Numbers` dígitos</span><span class="sxs-lookup"><span data-stu-id="63307-122">`Numbers` digits only</span></span>
- <span data-ttu-id="63307-123">`UppercaseLetters` apenas letras maiúsculas</span><span class="sxs-lookup"><span data-stu-id="63307-123">`UppercaseLetters` uppercase letters only</span></span>

<span data-ttu-id="63307-124">Se o `Custom FilterType` for usado, a propriedade `ValidChars` deverá ser definida e fornecerá uma lista de caracteres que podem ser digitados.</span><span class="sxs-lookup"><span data-stu-id="63307-124">If the `Custom FilterType` is used, the `ValidChars` property must be set and provide a list of characters that may be typed.</span></span> <span data-ttu-id="63307-125">A propósito: se você tentar colar o texto na caixa de texto, todos os caracteres inválidos serão removidos.</span><span class="sxs-lookup"><span data-stu-id="63307-125">By the way: if you try to paste text into the text box, all invalid chars are removed.</span></span>

<span data-ttu-id="63307-126">Aqui está a marcação para o controle de `FilteredTextBoxExtender` que permite apenas dígitos (algo que também teria sido possível com `FilterType="Numbers"`):</span><span class="sxs-lookup"><span data-stu-id="63307-126">Here is the markup for the `FilteredTextBoxExtender` control that only allows digits (something that would also have been possible with `FilterType="Numbers"`):</span></span>

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

<span data-ttu-id="63307-127">Execute a página e tente inserir uma letra se o JavaScript estiver habilitado, ele não funcionará; no entanto, os dígitos aparecem na página.</span><span class="sxs-lookup"><span data-stu-id="63307-127">Run the page and try to enter a letter if JavaScript is enabled, it will not work; digits however appear on the page.</span></span> <span data-ttu-id="63307-128">No entanto, observe que a proteção que o `FilteredTextBox` fornece não faz a prova de marcadores: se o JavaScript estiver habilitado, todos os dados poderão ser inseridos na caixa de texto, portanto, você precisará usar a validação adicional, ou seja, ASP. Controles de validação de rede.</span><span class="sxs-lookup"><span data-stu-id="63307-128">However note that the protection `FilteredTextBox` provides is not bullet-proof: If JavaScript is enabled, any data may be entered in the text box, so you have to use additional validation means, i.e. ASP.NET's validation controls.</span></span>

<span data-ttu-id="63307-129">[![somente dígitos podem ser inseridos](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="63307-129">[![Only digits may be entered](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)</span></span>

<span data-ttu-id="63307-130">Somente os dígitos podem ser inseridos ([clique para exibir a imagem em tamanho normal](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="63307-130">Only digits may be entered ([Click to view full-size image](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="63307-131">Anterior</span><span class="sxs-lookup"><span data-stu-id="63307-131">Previous</span></span>](allowing-only-certain-characters-in-a-text-box-cs.md)
