---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Parte 7: criando a página principal | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: fe4074c701159a137be3644d65ca844f160c2399
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598675"
---
# <a name="part-7-creating-the-main-page"></a><span data-ttu-id="99cc9-102">Parte 7: criando a página principal</span><span class="sxs-lookup"><span data-stu-id="99cc9-102">Part 7: Creating the Main Page</span></span>

<span data-ttu-id="99cc9-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="99cc9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="99cc9-104">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="99cc9-104">Download Completed Project</span></span>](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="99cc9-105">Criação da página principal</span><span class="sxs-lookup"><span data-stu-id="99cc9-105">Creating the Main Page</span></span>

<span data-ttu-id="99cc9-106">Nesta seção, você criará a página principal do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="99cc9-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="99cc9-107">Essa página será mais complexa do que a página de administração, portanto, vamos abordá-la em várias etapas.</span><span class="sxs-lookup"><span data-stu-id="99cc9-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="99cc9-108">Ao longo do caminho, você verá algumas técnicas mais avançadas de Knockout. js.</span><span class="sxs-lookup"><span data-stu-id="99cc9-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="99cc9-109">Este é o layout básico da página:</span><span class="sxs-lookup"><span data-stu-id="99cc9-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="99cc9-110">"Produtos" contém uma matriz de produtos.</span><span class="sxs-lookup"><span data-stu-id="99cc9-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="99cc9-111">"Carrinho" contém uma matriz de produtos com quantidades.</span><span class="sxs-lookup"><span data-stu-id="99cc9-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="99cc9-112">Clicar em "adicionar ao carrinho" atualizará o carrinho.</span><span class="sxs-lookup"><span data-stu-id="99cc9-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="99cc9-113">"Orders" mantém uma matriz de IDs de pedido.</span><span class="sxs-lookup"><span data-stu-id="99cc9-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="99cc9-114">"Detalhes" contém um detalhe do pedido, que é uma matriz de itens (produtos com quantidades)</span><span class="sxs-lookup"><span data-stu-id="99cc9-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="99cc9-115">Vamos começar definindo um layout básico em HTML, sem vinculação de dados ou script.</span><span class="sxs-lookup"><span data-stu-id="99cc9-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="99cc9-116">Abra o arquivo views/home/index. cshtml e substitua todo o conteúdo pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="99cc9-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="99cc9-117">Em seguida, adicione uma seção scripts e crie um modelo de exibição vazio:</span><span class="sxs-lookup"><span data-stu-id="99cc9-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="99cc9-118">Com base no design esboçado anteriormente, nosso modelo de exibição precisa de observáveis para produtos, carrinho, pedidos e detalhes.</span><span class="sxs-lookup"><span data-stu-id="99cc9-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="99cc9-119">Adicione as seguintes variáveis ao objeto `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="99cc9-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="99cc9-120">Os usuários podem adicionar itens da lista de produtos no carrinho e remover itens do carrinho.</span><span class="sxs-lookup"><span data-stu-id="99cc9-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="99cc9-121">Para encapsular essas funções, criaremos outra classe View-Model que representa um produto.</span><span class="sxs-lookup"><span data-stu-id="99cc9-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="99cc9-122">Adicione o seguinte código a `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="99cc9-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="99cc9-123">A classe `ProductViewModel` contém duas funções que são usadas para mover o produto de e para o carrinho: `addItemToCart` adiciona uma unidade do produto ao carrinho e `removeAllFromCart` remove todas as quantidades do produto.</span><span class="sxs-lookup"><span data-stu-id="99cc9-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="99cc9-124">Os usuários podem selecionar um pedido existente e obter os detalhes do pedido.</span><span class="sxs-lookup"><span data-stu-id="99cc9-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="99cc9-125">Encapsularemos essa funcionalidade em outro View-Model:</span><span class="sxs-lookup"><span data-stu-id="99cc9-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="99cc9-126">O `OrderDetailsViewModel` é inicializado com um pedido e busca os detalhes do pedido enviando uma solicitação AJAX ao servidor.</span><span class="sxs-lookup"><span data-stu-id="99cc9-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="99cc9-127">Além disso, observe a propriedade `total` no `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="99cc9-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="99cc9-128">Essa propriedade é um tipo especial de observável chamado de [observed calculado](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="99cc9-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="99cc9-129">Como o nome indica, um observed calculado permite que você vincule dados a um valor&#8212;calculado nesse caso, o custo total do pedido.</span><span class="sxs-lookup"><span data-stu-id="99cc9-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="99cc9-130">Em seguida, adicione essas funções a `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="99cc9-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="99cc9-131">`resetCart` remove todos os itens do carrinho.</span><span class="sxs-lookup"><span data-stu-id="99cc9-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="99cc9-132">`getDetails` Obtém os detalhes de um pedido (enviando um novo `OrderDetailsViewModel` para a lista de `details`).</span><span class="sxs-lookup"><span data-stu-id="99cc9-132">`getDetails` gets the details for an order (by pushing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="99cc9-133">`createOrder` cria um novo pedido e esvazia o carrinho.</span><span class="sxs-lookup"><span data-stu-id="99cc9-133">`createOrder` creates a new order and empties the cart.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="99cc9-134">Por fim, inicialize o modelo de exibição fazendo solicitações AJAX para os produtos e pedidos:</span><span class="sxs-lookup"><span data-stu-id="99cc9-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="99cc9-135">OK, isso é muito código, mas nós o criamos passo a passo, então espero que o design esteja claro.</span><span class="sxs-lookup"><span data-stu-id="99cc9-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="99cc9-136">Agora, podemos adicionar algumas associações knockout. js ao HTML.</span><span class="sxs-lookup"><span data-stu-id="99cc9-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="99cc9-137">**Produtos**</span><span class="sxs-lookup"><span data-stu-id="99cc9-137">**Products**</span></span>

<span data-ttu-id="99cc9-138">Aqui estão as associações para a lista de produtos:</span><span class="sxs-lookup"><span data-stu-id="99cc9-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="99cc9-139">Isso itera na matriz de produtos e exibe o nome e o preço.</span><span class="sxs-lookup"><span data-stu-id="99cc9-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="99cc9-140">O botão "adicionar à ordem" só fica visível quando o usuário está conectado.</span><span class="sxs-lookup"><span data-stu-id="99cc9-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="99cc9-141">O botão "adicionar à ordem" chama `addItemToCart` na instância de `ProductViewModel` do produto.</span><span class="sxs-lookup"><span data-stu-id="99cc9-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="99cc9-142">Isso demonstra um bom recurso de Knockout. js: quando um View-Model contém outros modos de exibição-modelos, você pode aplicar as associações ao modelo interno.</span><span class="sxs-lookup"><span data-stu-id="99cc9-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="99cc9-143">Neste exemplo, as associações dentro do `foreach` são aplicadas a cada uma das instâncias de `ProductViewModel`.</span><span class="sxs-lookup"><span data-stu-id="99cc9-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="99cc9-144">Essa abordagem é muito mais limpa do que colocar toda a funcionalidade em um único modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="99cc9-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="99cc9-145">**Carrinho**</span><span class="sxs-lookup"><span data-stu-id="99cc9-145">**Cart**</span></span>

<span data-ttu-id="99cc9-146">Aqui estão as associações para o carrinho:</span><span class="sxs-lookup"><span data-stu-id="99cc9-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="99cc9-147">Isso itera na matriz de carrinhos e exibe o nome, o preço e a quantidade.</span><span class="sxs-lookup"><span data-stu-id="99cc9-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="99cc9-148">Observe que o link "remover" e o botão "criar ordem" estão associados às funções de modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="99cc9-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="99cc9-149">**Solicitar**</span><span class="sxs-lookup"><span data-stu-id="99cc9-149">**Orders**</span></span>

<span data-ttu-id="99cc9-150">Aqui estão as associações para a lista Orders:</span><span class="sxs-lookup"><span data-stu-id="99cc9-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="99cc9-151">Isso itera sobre os pedidos e mostra a ID do pedido.</span><span class="sxs-lookup"><span data-stu-id="99cc9-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="99cc9-152">O evento de clique no link está associado à função `getDetails`.</span><span class="sxs-lookup"><span data-stu-id="99cc9-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="99cc9-153">**Detalhes do pedido**</span><span class="sxs-lookup"><span data-stu-id="99cc9-153">**Order Details**</span></span>

<span data-ttu-id="99cc9-154">Aqui estão as associações para os detalhes do pedido:</span><span class="sxs-lookup"><span data-stu-id="99cc9-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="99cc9-155">Isso itera sobre os itens na ordem e exibe o produto, o preço e a quantidade.</span><span class="sxs-lookup"><span data-stu-id="99cc9-155">This iterates over the items in the order and displays the product, price, and quantity.</span></span> <span data-ttu-id="99cc9-156">A div ao redor só será visível se a matriz de detalhes contiver um ou mais itens.</span><span class="sxs-lookup"><span data-stu-id="99cc9-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="99cc9-157">Conclusão</span><span class="sxs-lookup"><span data-stu-id="99cc9-157">Conclusion</span></span>

<span data-ttu-id="99cc9-158">Neste tutorial, você criou um aplicativo que usa Entity Framework para se comunicar com o banco de dados e ASP.NET Web API para fornecer uma interface voltada para o público sobre a camada de dado.</span><span class="sxs-lookup"><span data-stu-id="99cc9-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="99cc9-159">Usamos o ASP.NET MVC 4 para renderizar as páginas HTML e knockout. js Plus jQuery para fornecer interações dinâmicas sem recargas de página.</span><span class="sxs-lookup"><span data-stu-id="99cc9-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="99cc9-160">Recursos adicionais:</span><span class="sxs-lookup"><span data-stu-id="99cc9-160">Additional resources:</span></span>

- [<span data-ttu-id="99cc9-161">Mapa de conteúdo de acesso a dados do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="99cc9-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="99cc9-162">Centro de desenvolvedores Entity Framework</span><span class="sxs-lookup"><span data-stu-id="99cc9-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [<span data-ttu-id="99cc9-163">Anterior</span><span class="sxs-lookup"><span data-stu-id="99cc9-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
