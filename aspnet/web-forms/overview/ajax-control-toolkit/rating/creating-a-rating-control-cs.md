---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: Criando um controle de classificaçãoC#() | Microsoft Docs
author: wenz
description: Muitos sites, de comércio eletrônico a sites da Comunidade, oferecem aos usuários a taxa de artigos ou itens. Isso geralmente requer algum esforço de codificação, mas temos o...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: c0bf793406e378321f54f0460031c526a0b41a02
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611562"
---
# <a name="creating-a-rating-control-c"></a><span data-ttu-id="9ba58-104">Criação de um controle Classificação (C#)</span><span class="sxs-lookup"><span data-stu-id="9ba58-104">Creating a Rating Control (C#)</span></span>

<span data-ttu-id="9ba58-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9ba58-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9ba58-106">[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="9ba58-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span></span>

> <span data-ttu-id="9ba58-107">Muitos sites, de comércio eletrônico a sites da Comunidade, oferecem aos usuários a taxa de artigos ou itens.</span><span class="sxs-lookup"><span data-stu-id="9ba58-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="9ba58-108">Isso geralmente requer algum esforço de codificação, mas temos o kit de ferramentas de controle para nossa disposição.</span><span class="sxs-lookup"><span data-stu-id="9ba58-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="overview"></a><span data-ttu-id="9ba58-109">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="9ba58-109">Overview</span></span>

<span data-ttu-id="9ba58-110">Muitos sites, de comércio eletrônico a sites da Comunidade, oferecem aos usuários a taxa de artigos ou itens.</span><span class="sxs-lookup"><span data-stu-id="9ba58-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="9ba58-111">Isso geralmente requer algum esforço de codificação, mas temos o kit de ferramentas de controle para nossa disposição.</span><span class="sxs-lookup"><span data-stu-id="9ba58-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="9ba58-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="9ba58-112">Steps</span></span>

<span data-ttu-id="9ba58-113">Em primeiro lugar, você precisa de (pelo menos) dois tipos de imagens: uma para um item de avaliação preenchido e outra para um item de classificação vazio.</span><span class="sxs-lookup"><span data-stu-id="9ba58-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="9ba58-114">Um item de classificação geralmente é uma estrela ou um Smiley.</span><span class="sxs-lookup"><span data-stu-id="9ba58-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="9ba58-115">Para esse cenário, você encontra três arquivos, Smiley. png e Empty. png e Smiley-done. png como parte do download do código-fonte para este tutorial.</span><span class="sxs-lookup"><span data-stu-id="9ba58-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="9ba58-116">Em seguida, crie um novo arquivo ASP.NET e comece com a adição de um controle de `ScriptManager` a ele:</span><span class="sxs-lookup"><span data-stu-id="9ba58-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

<span data-ttu-id="9ba58-117">Em seguida, adicione o controle de `Rating` do kit de ferramentas de controle AJAX ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9ba58-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="9ba58-118">Os atributos a seguir precisam ser definidos para este exemplo:</span><span class="sxs-lookup"><span data-stu-id="9ba58-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="9ba58-119">`CurrentRating` a classificação inicial a ser usada</span><span class="sxs-lookup"><span data-stu-id="9ba58-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="9ba58-120">`MaxRating` a classificação máxima</span><span class="sxs-lookup"><span data-stu-id="9ba58-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="9ba58-121">`EmptyStarCssClass` a classe CSS a ser usada quando um item de classificação (estrela) estiver vazio</span><span class="sxs-lookup"><span data-stu-id="9ba58-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="9ba58-122">`FilledStarCssClass` a classe CSS a ser usada quando um item de classificação (estrela) é preenchido</span><span class="sxs-lookup"><span data-stu-id="9ba58-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="9ba58-123">`StarCssClass` a classe CSS a ser usada para uma estatística visível</span><span class="sxs-lookup"><span data-stu-id="9ba58-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="9ba58-124">`WaitingStarCssClass` a classe CSS a ser usada enquanto uma classificação por estrelas é enviada de volta ao servidor</span><span class="sxs-lookup"><span data-stu-id="9ba58-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="9ba58-125">E aqui está a marcação que cria um controle de classificação com cinco itens (sorriso) dos quais nenhum é preenchido inicialmente:</span><span class="sxs-lookup"><span data-stu-id="9ba58-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

<span data-ttu-id="9ba58-126">As três classes CSS referenciadas agora precisam mostrar os arquivos de imagem apropriados, o que é fácil de fazer usando CSS:</span><span class="sxs-lookup"><span data-stu-id="9ba58-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

<span data-ttu-id="9ba58-127">Certifique-se de fornecer a largura e a altura das três imagens, caso contrário, a exibição pode parecer um pouco bagunçada.</span><span class="sxs-lookup"><span data-stu-id="9ba58-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="9ba58-128">Por fim, o resultado da classificação deve ser exibido ao usuário (ou, pelo menos, salvo em um banco de dados).</span><span class="sxs-lookup"><span data-stu-id="9ba58-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="9ba58-129">Portanto, adicione um rótulo para a saída de uma mensagem de texto e um botão enviar para postar o formulário de classificação no servidor:</span><span class="sxs-lookup"><span data-stu-id="9ba58-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

<span data-ttu-id="9ba58-130">No código do servidor, acesse o controle de classificação por meio de seu `ID` e, em seguida, acesse sua propriedade `CurrentRating`, que é o número dos itens de classificação selecionados, em nosso exemplo um valor entre 0 e 5.</span><span class="sxs-lookup"><span data-stu-id="9ba58-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

<span data-ttu-id="9ba58-131">Salve a página e carregue-a no navegador.</span><span class="sxs-lookup"><span data-stu-id="9ba58-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="9ba58-132">Quando você passa o mouse sobre os itens de classificação (inicialmente vazios), ocorre um efeito de JavaScript: a classificação muda.</span><span class="sxs-lookup"><span data-stu-id="9ba58-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="9ba58-133">Quando você clica no conjunto de estrelas, a classificação atual é mantida.</span><span class="sxs-lookup"><span data-stu-id="9ba58-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="9ba58-134">Por fim, quando você envia o formulário, o código do lado do servidor gera a classificação selecionada.</span><span class="sxs-lookup"><span data-stu-id="9ba58-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>

<span data-ttu-id="9ba58-135">[![criar um sistema de classificação com código mínimo](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9ba58-135">[![Creating a rating system with minimal code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="9ba58-136">Criando um sistema de classificação com código mínimo ([clique para exibir a imagem em tamanho normal](creating-a-rating-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9ba58-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="9ba58-137">Avançar</span><span class="sxs-lookup"><span data-stu-id="9ba58-137">Next</span></span>](creating-a-rating-control-vb.md)
