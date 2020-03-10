---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Parte 8: carrinho de compras com atualizações do AJAX | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 8 aborda o carrinho de compras com as atualizações do Ajax.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 89897ad41b217764cbd17317d4bf5d6a5c5d488f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539252"
---
# <a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="88e3f-104">Parte 8: carrinho de compras com atualizações do AJAX</span><span class="sxs-lookup"><span data-stu-id="88e3f-104">Part 8: Shopping Cart with Ajax Updates</span></span>

<span data-ttu-id="88e3f-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="88e3f-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="88e3f-106">A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.</span><span class="sxs-lookup"><span data-stu-id="88e3f-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="88e3f-107">A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="88e3f-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="88e3f-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="88e3f-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="88e3f-109">A parte 8 aborda o carrinho de compras com as atualizações do Ajax.</span><span class="sxs-lookup"><span data-stu-id="88e3f-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>

<span data-ttu-id="88e3f-110">Vamos permitir que os usuários coloquem álbuns em seu carrinho sem se registrar, mas eles precisarão se registrar como convidados para concluir o check-out.</span><span class="sxs-lookup"><span data-stu-id="88e3f-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="88e3f-111">O processo de compra e de check-out será separado em dois controladores: um controlador ShoppingCart que permite adicionar itens anonimamente a um carrinho e um controlador de check-out que manipula o processo de check-out.</span><span class="sxs-lookup"><span data-stu-id="88e3f-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="88e3f-112">Vamos começar com o carrinho de compras nesta seção e, em seguida, criar o processo de check-out na seção a seguir.</span><span class="sxs-lookup"><span data-stu-id="88e3f-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="88e3f-113">Adicionando as classes de modelo de carrinho, ordem e OrderDetail</span><span class="sxs-lookup"><span data-stu-id="88e3f-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="88e3f-114">Nosso carrinho de compras e os processos de check-out farão uso de algumas novas classes.</span><span class="sxs-lookup"><span data-stu-id="88e3f-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="88e3f-115">Clique com o botão direito do mouse na pasta modelos e adicione uma classe de carrinho (Cart.cs) com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="88e3f-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="88e3f-116">Essa classe é muito parecida com as outras que usamos até agora, com exceção do atributo [key] para a propriedade recordId.</span><span class="sxs-lookup"><span data-stu-id="88e3f-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="88e3f-117">Nossos itens de carrinho terão um identificador de cadeia de caracteres chamado Cartid para permitir compras anônimas, mas a tabela inclui uma chave primária de inteiro chamada recordId.</span><span class="sxs-lookup"><span data-stu-id="88e3f-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="88e3f-118">Por convenção, Entity Framework código primeiro espera que a chave primária para uma tabela denominada cart seja a Cartid ou ID, mas podemos substituir facilmente isso por meio de anotações ou código, se desejarmos.</span><span class="sxs-lookup"><span data-stu-id="88e3f-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="88e3f-119">Este é um exemplo de como podemos usar as convenções simples em Entity Framework Code-First quando elas se adequarem a nós, mas não estamos restritos por elas quando não estiverem.</span><span class="sxs-lookup"><span data-stu-id="88e3f-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="88e3f-120">Em seguida, adicione uma classe Order (Order.cs) com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="88e3f-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="88e3f-121">Essa classe acompanha as informações de resumo e entrega de um pedido.</span><span class="sxs-lookup"><span data-stu-id="88e3f-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="88e3f-122">**Ele ainda não será compilado**, pois tem uma propriedade de navegação OrderDetails que depende de uma classe que ainda não criamos.</span><span class="sxs-lookup"><span data-stu-id="88e3f-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="88e3f-123">Vamos corrigir isso agora adicionando uma classe chamada OrderDetail.cs, adicionando o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="88e3f-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="88e3f-124">Vamos fazer uma última atualização para nossa classe MusicStoreEntities para incluir DbSets que expõem essas novas classes de modelo, incluindo também uma&gt;de&lt;artista.</span><span class="sxs-lookup"><span data-stu-id="88e3f-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="88e3f-125">A classe MusicStoreEntities atualizada aparece como abaixo.</span><span class="sxs-lookup"><span data-stu-id="88e3f-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="88e3f-126">Gerenciando a lógica de negócios do carrinho de compras</span><span class="sxs-lookup"><span data-stu-id="88e3f-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="88e3f-127">Em seguida, criaremos a classe ShoppingCart na pasta modelos.</span><span class="sxs-lookup"><span data-stu-id="88e3f-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="88e3f-128">O modelo ShoppingCart manipula o acesso a dados para a tabela de carrinhos.</span><span class="sxs-lookup"><span data-stu-id="88e3f-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="88e3f-129">Além disso, ele tratará a lógica de negócios para a adição e remoção de itens do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="88e3f-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="88e3f-130">Como não queremos exigir que os usuários se inscrevam para uma conta apenas para adicionar itens ao carrinho de compras, atribuíremos aos usuários um identificador exclusivo temporário (usando um GUID ou um identificador global exclusivo) quando acessarem o carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="88e3f-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="88e3f-131">Armazenaremos essa ID usando a classe de sessão ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="88e3f-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="88e3f-132">*Observação: a sessão ASP.NET é um local conveniente para armazenar informações específicas do usuário, que expirarão depois que saírem do site. Embora o uso indevido do estado da sessão possa causar implicações de desempenho em sites maiores, nosso uso leve funcionará bem para fins de demonstração.*</span><span class="sxs-lookup"><span data-stu-id="88e3f-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="88e3f-133">A classe ShoppingCart expõe os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="88e3f-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="88e3f-134">**AddToCart** usa um álbum como um parâmetro e o adiciona ao carrinho do usuário.</span><span class="sxs-lookup"><span data-stu-id="88e3f-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="88e3f-135">Como a tabela de carrinhos rastreia a quantidade de cada álbum, ela inclui a lógica para criar uma nova linha, se necessário, ou apenas incrementar a quantidade se o usuário já tiver solicitado uma cópia do álbum.</span><span class="sxs-lookup"><span data-stu-id="88e3f-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="88e3f-136">**RemoveFromCart** usa uma ID de álbum e a remove do carrinho do usuário.</span><span class="sxs-lookup"><span data-stu-id="88e3f-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="88e3f-137">Se o usuário tiver apenas uma cópia do álbum em seu carrinho, a linha será removida.</span><span class="sxs-lookup"><span data-stu-id="88e3f-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="88e3f-138">**EmptyCart** remove todos os itens do carrinho de compras de um usuário.</span><span class="sxs-lookup"><span data-stu-id="88e3f-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="88e3f-139">**GetCartItems** recupera uma lista de CartItems para exibição ou processamento.</span><span class="sxs-lookup"><span data-stu-id="88e3f-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="88e3f-140">**GetCount** recupera um número total de álbuns que um usuário tem em seu carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="88e3f-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="88e3f-141">**GetTotal** calcula o custo total de todos os itens no carrinho.</span><span class="sxs-lookup"><span data-stu-id="88e3f-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="88e3f-142">**CreateOrder** converte o carrinho de compras em um pedido durante a fase de check-out.</span><span class="sxs-lookup"><span data-stu-id="88e3f-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="88e3f-143">**Getcart** é um método estático que permite que nossos controladores obtenham um objeto de carrinho.</span><span class="sxs-lookup"><span data-stu-id="88e3f-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="88e3f-144">Ele usa o método **Getcarrinhoid** para lidar com a leitura de cartid a partir da sessão do usuário.</span><span class="sxs-lookup"><span data-stu-id="88e3f-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="88e3f-145">O método getcarrinhoid requer o HttpContextBase para que ele possa ler o Carrinhoid do usuário a partir da sessão do usuário.</span><span class="sxs-lookup"><span data-stu-id="88e3f-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="88e3f-146">Aqui está a **classe ShoppingCart**completa:</span><span class="sxs-lookup"><span data-stu-id="88e3f-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="88e3f-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="88e3f-147">ViewModels</span></span>

<span data-ttu-id="88e3f-148">Nosso controlador de carrinho de compras precisará comunicar algumas informações complexas para suas exibições que não são mapeadas corretamente para nossos objetos de modelo.</span><span class="sxs-lookup"><span data-stu-id="88e3f-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="88e3f-149">Não queremos modificar nossos modelos para se adequar às nossas exibições; Classes de modelo devem representar nosso domínio, não a interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="88e3f-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="88e3f-150">Uma solução seria passar as informações para nossas exibições usando a classe ViewBag, como fizemos com as informações suspensas do Store Manager, mas passar muitas informações por meio de ViewBag é difícil de gerenciar.</span><span class="sxs-lookup"><span data-stu-id="88e3f-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="88e3f-151">Uma solução para isso é usar o padrão *ViewModel* .</span><span class="sxs-lookup"><span data-stu-id="88e3f-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="88e3f-152">Ao usar esse padrão, criamos classes fortemente tipadas que são otimizadas para nossos cenários de exibição específicos e que expõem propriedades para os valores dinâmicos/conteúdo exigidos por nossos modelos de exibição.</span><span class="sxs-lookup"><span data-stu-id="88e3f-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="88e3f-153">Nossas classes de controlador podem então popular e passar essas classes otimizadas para exibição para nosso modelo de exibição a ser usado.</span><span class="sxs-lookup"><span data-stu-id="88e3f-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="88e3f-154">Isso habilita a segurança de tipo, a verificação de tempo de compilação e o IntelliSense do editor em modelos de exibição.</span><span class="sxs-lookup"><span data-stu-id="88e3f-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="88e3f-155">Criaremos dois modelos de exibição para uso em nosso controlador de carrinho de compras: o ShoppingCartViewModel manterá o conteúdo do carrinho de compras do usuário e o ShoppingCartRemoveViewModel será usado para exibir informações de confirmação quando um usuário remover algo do carrinho.</span><span class="sxs-lookup"><span data-stu-id="88e3f-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="88e3f-156">Vamos criar uma nova pasta ViewModels na raiz do nosso projeto para manter as coisas organizadas.</span><span class="sxs-lookup"><span data-stu-id="88e3f-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="88e3f-157">Clique com o botão direito do mouse no projeto, selecione Adicionar/nova pasta.</span><span class="sxs-lookup"><span data-stu-id="88e3f-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="88e3f-158">Nomeie a pasta ViewModels.</span><span class="sxs-lookup"><span data-stu-id="88e3f-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="88e3f-159">Em seguida, adicione a classe ShoppingCartViewModel na pasta ViewModels.</span><span class="sxs-lookup"><span data-stu-id="88e3f-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="88e3f-160">Ele tem duas propriedades: uma lista de itens do carrinho e um valor decimal para manter o preço total de todos os itens no carrinho.</span><span class="sxs-lookup"><span data-stu-id="88e3f-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="88e3f-161">Agora, adicione o ShoppingCartRemoveViewModel à pasta ViewModels, com as quatro propriedades a seguir.</span><span class="sxs-lookup"><span data-stu-id="88e3f-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="88e3f-162">O controlador do carrinho de compras</span><span class="sxs-lookup"><span data-stu-id="88e3f-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="88e3f-163">O controlador do carrinho de compras tem três objetivos principais: adicionar itens a um carrinho, remover itens do carrinho e exibir itens no carrinho.</span><span class="sxs-lookup"><span data-stu-id="88e3f-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="88e3f-164">Ele fará uso das três classes que acabamos de criar: ShoppingCartViewModel, ShoppingCartRemoveViewModel e ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="88e3f-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="88e3f-165">Como no StoreController e no StoreManagerController, adicionaremos um campo para manter uma instância de MusicStoreEntities.</span><span class="sxs-lookup"><span data-stu-id="88e3f-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="88e3f-166">Adicione um novo controlador de carrinho de compras ao projeto usando o modelo de controlador vazio.</span><span class="sxs-lookup"><span data-stu-id="88e3f-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="88e3f-167">Aqui está o controlador ShoppingCart completo.</span><span class="sxs-lookup"><span data-stu-id="88e3f-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="88e3f-168">As ações de índice e adicionar controlador devem parecer muito familiares.</span><span class="sxs-lookup"><span data-stu-id="88e3f-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="88e3f-169">As ações do controlador remove e CartSummary tratam de dois casos especiais, que abordaremos na seção a seguir.</span><span class="sxs-lookup"><span data-stu-id="88e3f-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="88e3f-170">Atualizações do AJAX com o jQuery</span><span class="sxs-lookup"><span data-stu-id="88e3f-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="88e3f-171">Em seguida, criaremos uma página de índice do carrinho de compras que é fortemente tipada para o ShoppingCartViewModel e usa o modelo de exibição de lista usando o mesmo método de antes.</span><span class="sxs-lookup"><span data-stu-id="88e3f-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="88e3f-172">No entanto, em vez de usar um HTML. ActionLink para remover itens do carrinho, usaremos o jQuery para "vincular" o evento Click de todos os links nesta exibição que têm a classe HTML RemoveLink.</span><span class="sxs-lookup"><span data-stu-id="88e3f-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="88e3f-173">Em vez de lançar o formulário, esse manipulador de eventos de clique simplesmente fará um retorno de chamada AJAX para nossa ação do controlador RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="88e3f-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="88e3f-174">O RemoveFromCart retorna um resultado serializado JSON, que nosso retorno de chamada do jQuery, em seguida, analisa e executa quatro atualizações rápidas na página usando o jQuery:</span><span class="sxs-lookup"><span data-stu-id="88e3f-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="88e3f-175">Remove o álbum excluído da lista</span><span class="sxs-lookup"><span data-stu-id="88e3f-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="88e3f-176">Atualiza a contagem de carrinhos no cabeçalho</span><span class="sxs-lookup"><span data-stu-id="88e3f-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="88e3f-177">Exibe uma mensagem de atualização para o usuário</span><span class="sxs-lookup"><span data-stu-id="88e3f-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="88e3f-178">Atualiza o preço total do carrinho</span><span class="sxs-lookup"><span data-stu-id="88e3f-178">Updates the cart total price</span></span>

<span data-ttu-id="88e3f-179">Como o cenário de remoção está sendo manipulado por um retorno de chamada AJAX dentro da exibição de índice, não precisamos de uma exibição adicional para a ação RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="88e3f-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="88e3f-180">Aqui está o código completo para a exibição/ShoppingCart/Index:</span><span class="sxs-lookup"><span data-stu-id="88e3f-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="88e3f-181">Para testar isso, precisamos ser capazes de adicionar itens ao nosso carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="88e3f-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="88e3f-182">Atualizaremos nossa exibição de **detalhes da loja** para incluir um botão "adicionar ao carrinho".</span><span class="sxs-lookup"><span data-stu-id="88e3f-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="88e3f-183">Enquanto estamos com ele, podemos incluir algumas das informações adicionais do álbum que adicionamos desde a última atualização deste modo de exibição: gênero, artista, preço e arte do álbum.</span><span class="sxs-lookup"><span data-stu-id="88e3f-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="88e3f-184">O código de exibição de detalhes do repositório atualizado aparece como mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="88e3f-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="88e3f-185">Agora, podemos clicar na loja e testar adicionar e remover álbuns de e para nosso carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="88e3f-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="88e3f-186">Execute o aplicativo e navegue até o índice da loja.</span><span class="sxs-lookup"><span data-stu-id="88e3f-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="88e3f-187">Em seguida, clique em um gênero para exibir uma lista de álbuns.</span><span class="sxs-lookup"><span data-stu-id="88e3f-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="88e3f-188">Clicar em um título de álbum agora mostra nosso modo de exibição de detalhes do álbum atualizado, incluindo o botão "adicionar ao carrinho".</span><span class="sxs-lookup"><span data-stu-id="88e3f-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="88e3f-189">Clicar no botão "adicionar ao carrinho" mostra nossa exibição de índice do carrinho de compras com a lista de resumo do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="88e3f-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="88e3f-190">Depois de carregar seu carrinho de compras, você pode clicar no link remover do carrinho para ver a atualização do AJAX para seu carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="88e3f-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="88e3f-191">Criamos um carrinho de compras funcional que permite que usuários não registrados adicionem itens ao seu carrinho.</span><span class="sxs-lookup"><span data-stu-id="88e3f-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="88e3f-192">Na seção a seguir, vamos permitir que eles se registrem e concluam o processo de check-out.</span><span class="sxs-lookup"><span data-stu-id="88e3f-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="88e3f-193">[Anterior](mvc-music-store-part-7.md)
> [Próximo](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="88e3f-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
