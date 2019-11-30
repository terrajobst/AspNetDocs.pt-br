---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Parte 5: criando uma interface do usuário dinâmica com knockout. js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: bbdeba756de7986cfeb92aa10a57f4f101382f99
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600003"
---
# <a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="766fc-102">Parte 5: criando uma interface do usuário dinâmica com knockout. js</span><span class="sxs-lookup"><span data-stu-id="766fc-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="766fc-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="766fc-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="766fc-104">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="766fc-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="766fc-105">Criação uma interface do usuário dinâmica com Knockout.js</span><span class="sxs-lookup"><span data-stu-id="766fc-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="766fc-106">Nesta seção, usaremos knockout. js para adicionar funcionalidade à exibição de administrador.</span><span class="sxs-lookup"><span data-stu-id="766fc-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="766fc-107">[Knockout. js](http://knockoutjs.com/) é uma biblioteca JavaScript que facilita a ligação de controles HTML a dados.</span><span class="sxs-lookup"><span data-stu-id="766fc-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="766fc-108">O knockout. js usa o padrão Model-View-ViewModel (MVVM).</span><span class="sxs-lookup"><span data-stu-id="766fc-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="766fc-109">O *modelo* é a representação do lado do servidor dos dados no domínio de negócios (em nosso caso, produtos e pedidos).</span><span class="sxs-lookup"><span data-stu-id="766fc-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="766fc-110">A *exibição* é a camada de apresentação (HTML).</span><span class="sxs-lookup"><span data-stu-id="766fc-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="766fc-111">O *View-Model* é um objeto JavaScript que contém os dados do modelo.</span><span class="sxs-lookup"><span data-stu-id="766fc-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="766fc-112">O View-Model é uma abstração de código da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="766fc-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="766fc-113">Ele não tem conhecimento da representação HTML.</span><span class="sxs-lookup"><span data-stu-id="766fc-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="766fc-114">Em vez disso, ele representa recursos abstratos da exibição, como "uma lista de itens".</span><span class="sxs-lookup"><span data-stu-id="766fc-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="766fc-115">A exibição é associada a dados ao modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="766fc-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="766fc-116">As atualizações do modelo de exibição são refletidas automaticamente na exibição.</span><span class="sxs-lookup"><span data-stu-id="766fc-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="766fc-117">O View-Model também obtém eventos da exibição, como cliques de botão, e executa operações no modelo, como criar um pedido.</span><span class="sxs-lookup"><span data-stu-id="766fc-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="766fc-118">Primeiro, definiremos o View-Model.</span><span class="sxs-lookup"><span data-stu-id="766fc-118">First we'll define the view-model.</span></span> <span data-ttu-id="766fc-119">Depois disso, Vincularemos a marcação HTML ao modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="766fc-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="766fc-120">Adicione a seguinte seção do Razor a admin. cshtml:</span><span class="sxs-lookup"><span data-stu-id="766fc-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="766fc-121">Você pode adicionar essa seção em qualquer lugar do arquivo.</span><span class="sxs-lookup"><span data-stu-id="766fc-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="766fc-122">Quando a exibição é renderizada, a seção é exibida na parte inferior da página HTML, logo antes do fechamento &lt;marca de&gt;/Body.</span><span class="sxs-lookup"><span data-stu-id="766fc-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="766fc-123">Todo o script desta página será inserido na marca de script indicada pelo comentário:</span><span class="sxs-lookup"><span data-stu-id="766fc-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="766fc-124">Primeiro, defina uma classe de modelo de exibição:</span><span class="sxs-lookup"><span data-stu-id="766fc-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="766fc-125">**ko. observableArray** é um tipo especial de objeto no knockout, chamado de *observável*.</span><span class="sxs-lookup"><span data-stu-id="766fc-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="766fc-126">Na [documentação do knockout. js](http://knockoutjs.com/documentation/observables.html): um observável é um "objeto JavaScript que pode notificar os assinantes sobre alterações".</span><span class="sxs-lookup"><span data-stu-id="766fc-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="766fc-127">Quando o conteúdo de uma alteração observável, a exibição é atualizada automaticamente para corresponder.</span><span class="sxs-lookup"><span data-stu-id="766fc-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="766fc-128">Para popular a matriz de `products`, faça uma solicitação AJAX para a API da Web.</span><span class="sxs-lookup"><span data-stu-id="766fc-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="766fc-129">Lembre-se de que armazenamos o URI de base para a API na bolsa de exibição (consulte a [parte 4](using-web-api-with-entity-framework-part-4.md) do tutorial).</span><span class="sxs-lookup"><span data-stu-id="766fc-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="766fc-130">Em seguida, adicione funções ao modelo de exibição para criar, atualizar e excluir produtos.</span><span class="sxs-lookup"><span data-stu-id="766fc-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="766fc-131">Essas funções enviam chamadas AJAX para a API da Web e usam os resultados para atualizar o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="766fc-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="766fc-132">Agora, a parte mais importante: quando o DOM for carregado, chame a função **ko. applyBindings** e passe uma nova instância do `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="766fc-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="766fc-133">O método **ko. applyBindings** ativa o Knockout e conecta o modelo de exibição à exibição.</span><span class="sxs-lookup"><span data-stu-id="766fc-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="766fc-134">Agora que temos um View-Model, podemos criar as associações.</span><span class="sxs-lookup"><span data-stu-id="766fc-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="766fc-135">No knockout. js, você faz isso adicionando `data-bind` atributos a elementos HTML.</span><span class="sxs-lookup"><span data-stu-id="766fc-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="766fc-136">Por exemplo, para associar uma lista HTML a uma matriz, use a associação de `foreach`:</span><span class="sxs-lookup"><span data-stu-id="766fc-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="766fc-137">A associação de `foreach` itera através da matriz e cria elementos filho para cada objeto na matriz.</span><span class="sxs-lookup"><span data-stu-id="766fc-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="766fc-138">Associações nos elementos filho podem se referir a propriedades nos objetos de matriz.</span><span class="sxs-lookup"><span data-stu-id="766fc-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="766fc-139">Adicione as seguintes associações à lista "atualização-produtos":</span><span class="sxs-lookup"><span data-stu-id="766fc-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="766fc-140">O elemento `<li>` ocorre dentro do escopo da Associação **foreach** .</span><span class="sxs-lookup"><span data-stu-id="766fc-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="766fc-141">Isso significa que o Knockout renderizará o elemento uma vez para cada produto na matriz de `products`.</span><span class="sxs-lookup"><span data-stu-id="766fc-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="766fc-142">Todas as associações dentro do elemento `<li>` referem-se à instância do produto.</span><span class="sxs-lookup"><span data-stu-id="766fc-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="766fc-143">Por exemplo, `$data.Name` refere-se à propriedade `Name` no produto.</span><span class="sxs-lookup"><span data-stu-id="766fc-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="766fc-144">Para definir os valores das entradas de texto, use a associação de `value`.</span><span class="sxs-lookup"><span data-stu-id="766fc-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="766fc-145">Os botões são associados a funções no modo de exibição de modelo, usando a associação de `click`.</span><span class="sxs-lookup"><span data-stu-id="766fc-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="766fc-146">A instância do produto é passada como um parâmetro para cada função.</span><span class="sxs-lookup"><span data-stu-id="766fc-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="766fc-147">Para obter mais informações, a [documentação do knockout. js](http://knockoutjs.com/documentation/observables.html) tem uma boa descrição das várias associações.</span><span class="sxs-lookup"><span data-stu-id="766fc-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="766fc-148">Em seguida, adicione uma associação para o evento **Enviar** no formulário Adicionar produto:</span><span class="sxs-lookup"><span data-stu-id="766fc-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="766fc-149">Essa ligação chama a função `create` no View-Model para criar um novo produto.</span><span class="sxs-lookup"><span data-stu-id="766fc-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="766fc-150">Este é o código completo para o modo de exibição de administrador:</span><span class="sxs-lookup"><span data-stu-id="766fc-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="766fc-151">Execute o aplicativo, faça logon com a conta de administrador e clique no link "admin".</span><span class="sxs-lookup"><span data-stu-id="766fc-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="766fc-152">Você deve ver a lista de produtos e ser capaz de criar, atualizar ou excluir produtos.</span><span class="sxs-lookup"><span data-stu-id="766fc-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="766fc-153">[Anterior](using-web-api-with-entity-framework-part-4.md)
> [Próximo](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="766fc-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
