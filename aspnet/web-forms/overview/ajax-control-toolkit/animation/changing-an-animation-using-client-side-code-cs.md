---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Alterando uma animação usando o código do ladoC#do cliente () | Microsoft Docs
author: wenz
description: O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle. A animação também pode...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 84fc2d6646b89cfabb2193cdfca59462d6d7ef16
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598213"
---
# <a name="changing-an-animation-using-client-side-code-c"></a><span data-ttu-id="86b57-104">Alterar uma animação usando o código do lado do cliente (C#)</span><span class="sxs-lookup"><span data-stu-id="86b57-104">Changing an Animation Using Client-Side Code (C#)</span></span>

<span data-ttu-id="86b57-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="86b57-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="86b57-106">[Baixar código](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="86b57-106">[Download Code](https://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) or [Download PDF](https://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span></span>

> <span data-ttu-id="86b57-107">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="86b57-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="86b57-108">A animação também pode ser alterada usando código JavaScript do lado do cliente personalizado.</span><span class="sxs-lookup"><span data-stu-id="86b57-108">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="overview"></a><span data-ttu-id="86b57-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="86b57-109">Overview</span></span>

<span data-ttu-id="86b57-110">O controle de animação no ASP.NET AJAX Control Toolkit não é apenas um controle, mas uma estrutura inteira para adicionar animações a um controle.</span><span class="sxs-lookup"><span data-stu-id="86b57-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="86b57-111">A animação também pode ser alterada usando código JavaScript do lado do cliente personalizado.</span><span class="sxs-lookup"><span data-stu-id="86b57-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="86b57-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="86b57-112">Steps</span></span>

<span data-ttu-id="86b57-113">Em primeiro lugar, inclua o `ScriptManager` na página; em seguida, a biblioteca do ASP.NET AJAX é carregada, possibilitando o uso do kit de ferramentas de controle:</span><span class="sxs-lookup"><span data-stu-id="86b57-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="86b57-114">A animação será aplicada a um painel de texto semelhante a este:</span><span class="sxs-lookup"><span data-stu-id="86b57-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="86b57-115">Na classe CSS associada para o painel, defina uma cor de plano de fundo boa e também defina uma largura fixa para o painel:</span><span class="sxs-lookup"><span data-stu-id="86b57-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="86b57-116">A animação real é iniciada por um botão HTML:</span><span class="sxs-lookup"><span data-stu-id="86b57-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="86b57-117">Em seguida, adicione o `AnimationExtender` à página, fornecendo um `ID`, o atributo `TargetControlID` e a `runat="server"`obrigatório:</span><span class="sxs-lookup"><span data-stu-id="86b57-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

<span data-ttu-id="86b57-118">Observe que não há `<Animations>` nó dentro do controle de `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="86b57-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="86b57-119">O código JavaScript personalizado é usado para fornecer as animações a serem usadas com o controle.</span><span class="sxs-lookup"><span data-stu-id="86b57-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="86b57-120">Assim como acontece com a API de servidor do `AnimationExtender`, não há uma maneira fácil de atribuir uma animação ao extensor ainda.</span><span class="sxs-lookup"><span data-stu-id="86b57-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="86b57-121">No entanto, o extensor expõe vários métodos para ler e gravar animações registradas com os vários eventos (`OnClick`, `OnLoad`e assim por diante).</span><span class="sxs-lookup"><span data-stu-id="86b57-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="86b57-122">Estes são alguns exemplos:</span><span class="sxs-lookup"><span data-stu-id="86b57-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="86b57-123">O formato do valor de retorno das funções de `get_*()` e o formato do argumento para as funções de `set_*()` é uma cadeia de caracteres JSON, fornecendo uma representação de objeto do que seria a marcação XML.</span><span class="sxs-lookup"><span data-stu-id="86b57-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="86b57-124">Atualmente, não há como passar um objeto no, mas é possível ler um objeto de uma determinada animação (`get_OnXXXBehavior()` métodos).</span><span class="sxs-lookup"><span data-stu-id="86b57-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="86b57-125">Aqui está uma cadeia de caracteres JSON (sem as aspas delimitadoras e formatada facilmente) que representa uma animação disparada pelo botão, mas animando o painel redimensionando-o e esmaecido ao mesmo tempo:</span><span class="sxs-lookup"><span data-stu-id="86b57-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

<span data-ttu-id="86b57-126">O código JavaScript a seguir atribui esse script JSON à animação `OnClick` do extensor atual e o executa:</span><span class="sxs-lookup"><span data-stu-id="86b57-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]

<span data-ttu-id="86b57-127">[![a animação é executada imediatamente, sem um clique do mouse (e com muito pouca marcação)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="86b57-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="86b57-128">A animação é executada imediatamente, sem um clique do mouse (e com uma pequena marcação) ([clique para exibir a imagem em tamanho normal](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="86b57-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="86b57-129">[Anterior](executing-animations-using-client-side-code-cs.md)
> [Próximo](animating-an-updatepanel-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="86b57-129">[Previous](executing-animations-using-client-side-code-cs.md)
[Next](animating-an-updatepanel-control-cs.md)</span></span>
