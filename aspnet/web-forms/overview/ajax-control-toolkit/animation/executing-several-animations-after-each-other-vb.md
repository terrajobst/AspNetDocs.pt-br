---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Executando várias animações depois umas às outras (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele permite executar o servidor...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 5034fe905299d4559b713dd841cb166886179c39
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598129"
---
# <a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="b843d-104">Executar várias animações, uma após a outra (VB)</span><span class="sxs-lookup"><span data-stu-id="b843d-104">Executing Several Animations after Each Other (VB)</span></span>

<span data-ttu-id="b843d-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b843d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b843d-106">[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="b843d-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="b843d-107">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="b843d-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b843d-108">Ele permite executar várias animações uma após a outra.</span><span class="sxs-lookup"><span data-stu-id="b843d-108">It allows to run several animations one after the other.</span></span>

## <a name="overview"></a><span data-ttu-id="b843d-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="b843d-109">Overview</span></span>

<span data-ttu-id="b843d-110">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="b843d-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b843d-111">Ele permite executar várias animações uma após a outra.</span><span class="sxs-lookup"><span data-stu-id="b843d-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="b843d-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="b843d-112">Steps</span></span>

<span data-ttu-id="b843d-113">Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:</span><span class="sxs-lookup"><span data-stu-id="b843d-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="b843d-114">A animação será aplicada a um painel de texto semelhante a este:</span><span class="sxs-lookup"><span data-stu-id="b843d-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="b843d-115">Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:</span><span class="sxs-lookup"><span data-stu-id="b843d-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="b843d-116">Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e o obrigatório `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="b843d-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="b843d-117">Dentro do nó `<Animations>`, use `<OnLoad>` para executar as animações depois que a página tiver sido totalmente carregada.</span><span class="sxs-lookup"><span data-stu-id="b843d-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="b843d-118">Em geral, `<OnLoad>` aceita apenas uma animação.</span><span class="sxs-lookup"><span data-stu-id="b843d-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="b843d-119">A estrutura de animação permite unir várias animações em uma usando o elemento `<Sequence>`.</span><span class="sxs-lookup"><span data-stu-id="b843d-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="b843d-120">Todas as animações dentro de `<Sequence>` são executadas uma após a outra.</span><span class="sxs-lookup"><span data-stu-id="b843d-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="b843d-121">Aqui está a marcação possível para o controle de `AnimationExtender`, primeiro tornando o painel mais largo e, em seguida, diminuindo sua altura:</span><span class="sxs-lookup"><span data-stu-id="b843d-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="b843d-122">Quando você executa esse script, o painel primeiro fica mais largo e depois menor.</span><span class="sxs-lookup"><span data-stu-id="b843d-122">When you run this script, the panel first gets wider and then smaller.</span></span>

<span data-ttu-id="b843d-123">[![primeira a largura é aumentada](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b843d-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="b843d-124">Primeiro, a largura é aumentada ([clique para exibir a imagem em tamanho normal](executing-several-animations-after-each-other-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b843d-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>

<span data-ttu-id="b843d-125">[![, a altura é diminuída](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="b843d-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="b843d-126">Em seguida, a altura é diminuída ([clique para exibir a imagem em tamanho normal](executing-several-animations-after-each-other-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b843d-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b843d-127">[Anterior](executing-several-animations-at-the-same-time-vb.md)
> [Próximo](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b843d-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
