---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Modificando animações do lado do servidorC#() | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. As animações também podem...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 0594efea9598a6c2461a72f789b5bd5f8ece23da
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74575165"
---
# <a name="modifying-animations-from-the-server-side-c"></a><span data-ttu-id="c3349-104">Modificando animações do lado do servidorC#()</span><span class="sxs-lookup"><span data-stu-id="c3349-104">Modifying Animations From The Server Side (C#)</span></span>

<span data-ttu-id="c3349-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c3349-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c3349-106">[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c3349-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span></span>

> <span data-ttu-id="c3349-107">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="c3349-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c3349-108">As animações também podem ser alteradas no lado do servidor</span><span class="sxs-lookup"><span data-stu-id="c3349-108">The animations may also be changed on the server-side</span></span>

## <a name="overview"></a><span data-ttu-id="c3349-109">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="c3349-109">Overview</span></span>

<span data-ttu-id="c3349-110">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="c3349-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c3349-111">As animações também podem ser alteradas no lado do servidor</span><span class="sxs-lookup"><span data-stu-id="c3349-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="c3349-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="c3349-112">Steps</span></span>

<span data-ttu-id="c3349-113">Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:</span><span class="sxs-lookup"><span data-stu-id="c3349-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

<span data-ttu-id="c3349-114">A animação será aplicada a um painel de texto semelhante a este:</span><span class="sxs-lookup"><span data-stu-id="c3349-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

<span data-ttu-id="c3349-115">Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:</span><span class="sxs-lookup"><span data-stu-id="c3349-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

<span data-ttu-id="c3349-116">O restante do código é executado no lado do servidor e não usa marcação; em vez disso, ele usa código para criar o controle de `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="c3349-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

<span data-ttu-id="c3349-117">No entanto, o kit de ferramentas de controle atualmente não fornece um acesso de API para criar as animações individuais.</span><span class="sxs-lookup"><span data-stu-id="c3349-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="c3349-118">No entanto, é possível definir a propriedade animações do `AnimationExtender`como uma cadeia de caracteres que contém a marcação XML usada ao atribuir as animações de forma declarativa.</span><span class="sxs-lookup"><span data-stu-id="c3349-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="c3349-119">Para criar o XML que não deve conter o elemento `<Animations>`, você pode usar o suporte a XML do .NET Framework ou, como no código a seguir, basta fornecer a cadeia de caracteres:</span><span class="sxs-lookup"><span data-stu-id="c3349-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

<span data-ttu-id="c3349-120">Por fim, adicione o controle de `AnimationExtender` à página atual, dentro do elemento `<form runat="server">`, certificando-se de que a animação está incluída e é executada:</span><span class="sxs-lookup"><span data-stu-id="c3349-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]

<span data-ttu-id="c3349-121">[![a animação é criada usando o código de C#/vb do lado do servidor](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c3349-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span></span>

<span data-ttu-id="c3349-122">A animação é criada usando o código de C#/vb do lado do servidor ([clique para exibir a imagem em tamanho normal](modifying-animations-from-the-server-side-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c3349-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c3349-123">[Anterior](triggering-an-animation-in-another-control-cs.md)
> [Próximo](executing-animations-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="c3349-123">[Previous](triggering-an-animation-in-another-control-cs.md)
[Next](executing-animations-using-client-side-code-cs.md)</span></span>
