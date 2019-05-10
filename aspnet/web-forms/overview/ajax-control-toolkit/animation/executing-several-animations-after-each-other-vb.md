---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Executando várias animações após a outra (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. Ele permite para executar severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 0e0c9aa2c9ce8b55824c24ef43881e35a0788d28
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65108317"
---
# <a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="4dc48-104">Executar várias animações, uma após a outra (VB)</span><span class="sxs-lookup"><span data-stu-id="4dc48-104">Executing Several Animations after Each Other (VB)</span></span>

<span data-ttu-id="4dc48-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4dc48-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4dc48-106">[Baixar o código](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) ou [baixar PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4dc48-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="4dc48-107">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="4dc48-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4dc48-108">Ele permite para executar várias animações um após o outro.</span><span class="sxs-lookup"><span data-stu-id="4dc48-108">It allows to run several animations one after the other.</span></span>

## <a name="overview"></a><span data-ttu-id="4dc48-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="4dc48-109">Overview</span></span>

<span data-ttu-id="4dc48-110">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="4dc48-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4dc48-111">Ele permite para executar várias animações um após o outro.</span><span class="sxs-lookup"><span data-stu-id="4dc48-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="4dc48-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="4dc48-112">Steps</span></span>

<span data-ttu-id="4dc48-113">Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do AJAX ASP.NET é carregada, tornando possível usar o Kit de ferramentas de controle:</span><span class="sxs-lookup"><span data-stu-id="4dc48-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="4dc48-114">A animação será aplicada a um painel de texto que tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="4dc48-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="4dc48-115">Na classe CSS associado para o painel, definir uma cor de fundo interessante e também definir uma largura fixa para o painel:</span><span class="sxs-lookup"><span data-stu-id="4dc48-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="4dc48-116">Em seguida, adicione a `AnimationExtender` para a página, fornecendo uma `ID`, o `TargetControlID` atributo e o obrigatório `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="4dc48-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="4dc48-117">Dentro de `<Animations>` nó, use `<OnLoad>` para as animações são executadas uma vez que a página foi totalmente carregada.</span><span class="sxs-lookup"><span data-stu-id="4dc48-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="4dc48-118">Em geral, `<OnLoad>` aceita apenas uma animação.</span><span class="sxs-lookup"><span data-stu-id="4dc48-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="4dc48-119">A estrutura de animação permite unir várias animações em um, utilizando o `<Sequence>` elemento.</span><span class="sxs-lookup"><span data-stu-id="4dc48-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="4dc48-120">Todas as animações em `<Sequence>` são executados um após o outro.</span><span class="sxs-lookup"><span data-stu-id="4dc48-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="4dc48-121">Aqui está a uma marcação possíveis para o `AnimationExtender` controle, primeiro fazer o painel mais amplo e, em seguida, diminuir sua altura:</span><span class="sxs-lookup"><span data-stu-id="4dc48-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="4dc48-122">Quando você executa esse script, o painel primeiro obtém mais largos e, em seguida, menores.</span><span class="sxs-lookup"><span data-stu-id="4dc48-122">When you run this script, the panel first gets wider and then smaller.</span></span>

<span data-ttu-id="4dc48-123">[![Pela primeira vez a largura é aumentada](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4dc48-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="4dc48-124">Pela primeira vez a largura é aumentada ([clique para exibir a imagem em tamanho normal](executing-several-animations-after-each-other-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4dc48-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>

<span data-ttu-id="4dc48-125">[![Em seguida, diminui a altura](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="4dc48-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="4dc48-126">Em seguida, diminui a altura ([clique para exibir a imagem em tamanho normal](executing-several-animations-after-each-other-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="4dc48-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4dc48-127">[Anterior](executing-several-animations-at-the-same-time-vb.md)
> [Próximo](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4dc48-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
