---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Alterando uma animação usando o código do lado do cliente (VB) | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação também pode...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: cce0a5a901f71edd40eada59ac7eeba93222e2b3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606915"
---
# <a name="changing-an-animation-using-client-side-code-vb"></a><span data-ttu-id="2106a-104">Alterar uma animação usando o código do lado do cliente (VB)</span><span class="sxs-lookup"><span data-stu-id="2106a-104">Changing an Animation Using Client-Side Code (VB)</span></span>

<span data-ttu-id="2106a-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2106a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2106a-106">[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2106a-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span></span>

> <span data-ttu-id="2106a-107">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="2106a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2106a-108">A animação também pode ser alterada usando código JavaScript do lado do cliente personalizado.</span><span class="sxs-lookup"><span data-stu-id="2106a-108">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="2106a-109">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2106a-109">Overview</span></span>

<span data-ttu-id="2106a-110">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="2106a-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2106a-111">A animação também pode ser alterada usando código JavaScript do lado do cliente personalizado.</span><span class="sxs-lookup"><span data-stu-id="2106a-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="2106a-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="2106a-112">Steps</span></span>

<span data-ttu-id="2106a-113">Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:</span><span class="sxs-lookup"><span data-stu-id="2106a-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="2106a-114">A animação será aplicada a um painel de texto semelhante a este:</span><span class="sxs-lookup"><span data-stu-id="2106a-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="2106a-115">Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:</span><span class="sxs-lookup"><span data-stu-id="2106a-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="2106a-116">A animação real é iniciada por um botão HTML:</span><span class="sxs-lookup"><span data-stu-id="2106a-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="2106a-117">Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e a `runat="server"`obrigatório:</span><span class="sxs-lookup"><span data-stu-id="2106a-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

<span data-ttu-id="2106a-118">Observe que não há `<Animations>` nó dentro do controle de `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="2106a-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="2106a-119">O código JavaScript personalizado é usado para fornecer as animações a serem usadas com o controle.</span><span class="sxs-lookup"><span data-stu-id="2106a-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="2106a-120">Assim como acontece com a API de servidor do `AnimationExtender`, não há uma maneira fácil de atribuir uma animação ao extensor ainda.</span><span class="sxs-lookup"><span data-stu-id="2106a-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="2106a-121">No entanto, o extensor expõe vários métodos para ler e gravar animações registradas com os vários eventos (`OnClick`, `OnLoad`e assim por diante).</span><span class="sxs-lookup"><span data-stu-id="2106a-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="2106a-122">Estes são alguns exemplos:</span><span class="sxs-lookup"><span data-stu-id="2106a-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="2106a-123">O formato do valor de retorno das funções de `get_*()` e o formato do argumento para as funções de `set_*()` é uma cadeia de caracteres JSON, fornecendo uma representação de objeto do que seria a marcação XML.</span><span class="sxs-lookup"><span data-stu-id="2106a-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="2106a-124">Atualmente, não há como passar um objeto no, mas é possível ler um objeto de uma determinada animação (`get_OnXXXBehavior()` métodos).</span><span class="sxs-lookup"><span data-stu-id="2106a-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="2106a-125">Aqui está uma cadeia de caracteres JSON (sem as aspas delimitadoras e formatada facilmente) que representa uma animação disparada pelo botão, mas animando o painel redimensionando-o e esmaecido ao mesmo tempo:</span><span class="sxs-lookup"><span data-stu-id="2106a-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

<span data-ttu-id="2106a-126">O código JavaScript a seguir atribui esse script JSON à animação `OnClick` do extensor atual e o executa:</span><span class="sxs-lookup"><span data-stu-id="2106a-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]

<span data-ttu-id="2106a-127">[![a animação é executada imediatamente, sem um clique do mouse (e com muito pouca marcação)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2106a-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="2106a-128">A animação é executada imediatamente, sem um clique do mouse (e com uma pequena marcação) ([clique para exibir a imagem em tamanho normal](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2106a-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2106a-129">[Anterior](executing-animations-using-client-side-code-vb.md)
> [Próximo](animating-an-updatepanel-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2106a-129">[Previous](executing-animations-using-client-side-code-vb.md)
[Next](animating-an-updatepanel-control-vb.md)</span></span>
