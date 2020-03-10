---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Criar o modo de exibição (IU) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 62c4523c2c6fb399cfbc3716309a1379996d601c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557298"
---
# <a name="create-the-view-ui"></a><span data-ttu-id="858b9-102">Criar a exibição (interface do usuário)</span><span class="sxs-lookup"><span data-stu-id="858b9-102">Create the View (UI)</span></span>

<span data-ttu-id="858b9-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="858b9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="858b9-104">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="858b9-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="858b9-105">Nesta seção, você começará a definir o HTML para o aplicativo e adicionará a vinculação de dados entre o HTML e o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="858b9-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="858b9-106">Abra o arquivo views/home/index. cshtml.</span><span class="sxs-lookup"><span data-stu-id="858b9-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="858b9-107">Substitua todo o conteúdo desse arquivo pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="858b9-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="858b9-108">A maioria dos elementos de `div` está lá para o estilo de [inicialização](http://getbootstrap.com/) .</span><span class="sxs-lookup"><span data-stu-id="858b9-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="858b9-109">Os elementos importantes são aqueles com `data-bind` atributos.</span><span class="sxs-lookup"><span data-stu-id="858b9-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="858b9-110">Esse atributo vincula o HTML ao modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="858b9-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="858b9-111">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="858b9-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="858b9-112">Neste exemplo, a associação de &quot; de `text`de &quot;faz com que o elemento `<p>` mostre o valor da propriedade `error` do modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="858b9-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="858b9-113">Lembre-se de que `error` foi declarado como um `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="858b9-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="858b9-114">Sempre que um novo valor é atribuído a `error`, o Knockout atualiza o texto no elemento `<p>`.</span><span class="sxs-lookup"><span data-stu-id="858b9-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="858b9-115">A associação de `foreach` informa ao Knockout para executar um loop pelo conteúdo da matriz `books`.</span><span class="sxs-lookup"><span data-stu-id="858b9-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="858b9-116">Para cada item na matriz, o Knockout cria um novo elemento &lt;li&gt;.</span><span class="sxs-lookup"><span data-stu-id="858b9-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="858b9-117">Associações dentro do contexto do `foreach` se referem a propriedades no item da matriz.</span><span class="sxs-lookup"><span data-stu-id="858b9-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="858b9-118">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="858b9-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="858b9-119">Aqui, a associação de `text` lê a propriedade autor de cada livro.</span><span class="sxs-lookup"><span data-stu-id="858b9-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="858b9-120">Se você executar o aplicativo agora, ele deverá ter a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="858b9-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="858b9-121">A lista de livros é carregada de forma assíncrona, após o carregamento da página.</span><span class="sxs-lookup"><span data-stu-id="858b9-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="858b9-122">No momento, o &quot;detalhes&quot; links não são funcionais.</span><span class="sxs-lookup"><span data-stu-id="858b9-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="858b9-123">Vamos adicionar essa funcionalidade na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="858b9-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="858b9-124">[Anterior](part-6.md)
> [Próximo](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="858b9-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
