---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Criando caixas de seleção mutuamente exclusivas (VB) | Microsoft Docs
author: wenz
description: 'Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção geralmente são usados. Há uma desvantagem, porém: quando um botão de opção em um grupo é selecionado,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: f33936dd4d71f6bbf08f02966eefe44c8c152eba
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606488"
---
# <a name="creating-mutually-exclusive-checkboxes-vb"></a><span data-ttu-id="43ccb-104">Criação de caixas de seleção mutuamente exclusivas (VB)</span><span class="sxs-lookup"><span data-stu-id="43ccb-104">Creating Mutually Exclusive Checkboxes (VB)</span></span>

<span data-ttu-id="43ccb-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="43ccb-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="43ccb-106">[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="43ccb-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) or [Download PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)</span></span>

> <span data-ttu-id="43ccb-107">Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção geralmente são usados.</span><span class="sxs-lookup"><span data-stu-id="43ccb-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="43ccb-108">Há uma desvantagem, porém: quando um botão de opção em um grupo é selecionado, não é possível desmarcar todos os botões de opção.</span><span class="sxs-lookup"><span data-stu-id="43ccb-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="43ccb-109">As caixas de seleção podem ser desmarcadas a qualquer momento, no entanto, não são mutuamente exclusivas.</span><span class="sxs-lookup"><span data-stu-id="43ccb-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="43ccb-110">Este tutorial fornece o melhor das duas abordagens: caixas de seleção que são mutuamente exclusivas.</span><span class="sxs-lookup"><span data-stu-id="43ccb-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="overview"></a><span data-ttu-id="43ccb-111">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="43ccb-111">Overview</span></span>

<span data-ttu-id="43ccb-112">Quando apenas um de um conjunto de opções pode ser selecionado, os botões de opção geralmente são usados.</span><span class="sxs-lookup"><span data-stu-id="43ccb-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="43ccb-113">Há uma desvantagem, porém: quando um botão de opção em um grupo é selecionado, não é possível desmarcar todos os botões de opção.</span><span class="sxs-lookup"><span data-stu-id="43ccb-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="43ccb-114">As caixas de seleção podem ser desmarcadas a qualquer momento, no entanto, não são mutuamente exclusivas.</span><span class="sxs-lookup"><span data-stu-id="43ccb-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="43ccb-115">Este tutorial fornece o melhor das duas abordagens: caixas de seleção que são mutuamente exclusivas.</span><span class="sxs-lookup"><span data-stu-id="43ccb-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="43ccb-116">Etapas</span><span class="sxs-lookup"><span data-stu-id="43ccb-116">Steps</span></span>

<span data-ttu-id="43ccb-117">O ASP.NET AJAX Control Toolkit contém o extensor MutuallyExclusiveCheckBox.</span><span class="sxs-lookup"><span data-stu-id="43ccb-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="43ccb-118">Isso permite que os programadores atribuam qualquer caixa de seleção a um nome de grupo (`Key` atributo).</span><span class="sxs-lookup"><span data-stu-id="43ccb-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="43ccb-119">Em todas as caixas de seleção dentro do mesmo grupo, apenas uma pode ser selecionada ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="43ccb-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="43ccb-120">Vamos começar com a colocação de duas caixas de seleção em uma nova página do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="43ccb-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="43ccb-121">Pode haver mais, mas duas delas são suficientes para demonstrar o princípio:</span><span class="sxs-lookup"><span data-stu-id="43ccb-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

<span data-ttu-id="43ccb-122">Para ambas as caixas de seleção, um controle MutuallyExclusiveCheckBoxExtender deve ser colocado na página.</span><span class="sxs-lookup"><span data-stu-id="43ccb-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="43ccb-123">Os dois atributos de chave precisam ter o mesmo valor, assim como os atributos de valor dos elementos do botão de opção HTML devem ser idênticos para indicar o grupo ao qual pertencem.</span><span class="sxs-lookup"><span data-stu-id="43ccb-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="43ccb-124">A propriedade TargetControlID do extensor aponta para a ID da caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="43ccb-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

<span data-ttu-id="43ccb-125">Por fim, inclua o `ScriptManager` AJAX ASP.NET, que é exigido por todos os elementos do ASP.NET AJAX Control Toolkit:</span><span class="sxs-lookup"><span data-stu-id="43ccb-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

<span data-ttu-id="43ccb-126">Salvar e executar a página: você pode marcar e desmarcar ambas as caixas de seleção, no entanto, em nenhum momento, ambas as caixas de seleção podem ser marcadas.</span><span class="sxs-lookup"><span data-stu-id="43ccb-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>

<span data-ttu-id="43ccb-127">[![apenas uma caixa de seleção pode ser marcada por vez](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="43ccb-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)</span></span>

<span data-ttu-id="43ccb-128">Somente uma caixa de seleção pode ser marcada de cada vez ([clique para exibir a imagem em tamanho normal](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="43ccb-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="43ccb-129">Anterior</span><span class="sxs-lookup"><span data-stu-id="43ccb-129">Previous</span></span>](creating-mutually-exclusive-checkboxes-cs.md)
