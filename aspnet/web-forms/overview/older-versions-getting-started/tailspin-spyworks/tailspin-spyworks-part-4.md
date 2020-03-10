---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: listando produtos | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 4 aborda a listagem de produtos com o GridView contr...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7af1b8afa2ecc8df9846f2edd2091b26b93a811c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566979"
---
# <a name="part-4-listing-products"></a><span data-ttu-id="4b5b4-104">Parte 4: listando produtos</span><span class="sxs-lookup"><span data-stu-id="4b5b4-104">Part 4: Listing Products</span></span>

<span data-ttu-id="4b5b4-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="4b5b4-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="4b5b4-106">A Tailspin Spyworks demonstra como é extremamente simples criar aplicativos poderosos e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="4b5b4-107">Ele mostra como usar os ótimos novos recursos do ASP.NET 4 para criar uma loja online, incluindo compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="4b5b4-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="4b5b4-109">A parte 4 aborda a listagem de produtos com o controle GridView.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-109">Part 4 covers listing products with the GridView control.</span></span>

## <a id="_Toc260221670"></a><span data-ttu-id="4b5b4-110">Listando produtos com o controle GridView</span><span class="sxs-lookup"><span data-stu-id="4b5b4-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="4b5b4-111">Vamos começar a implementar nossa página ProductList. aspx "clicando com o botão direito do mouse" em nossa solução e selecionando "Add" e "New Item".</span><span class="sxs-lookup"><span data-stu-id="4b5b4-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="4b5b4-112">Escolha "formulário da Web usando a página mestra" e insira um nome de página de ProductList. aspx ".</span><span class="sxs-lookup"><span data-stu-id="4b5b4-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="4b5b4-113">Clique em "Adicionar".</span><span class="sxs-lookup"><span data-stu-id="4b5b4-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="4b5b4-114">Em seguida, escolha a pasta "estilos" onde colocamos a página site. Master e a selecionamos na janela "conteúdo da pasta".</span><span class="sxs-lookup"><span data-stu-id="4b5b4-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="4b5b4-115">Clique em "OK" para criar a página.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="4b5b4-116">Nosso banco de dados é populado com a data do produto, como mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="4b5b4-117">Depois que nossa página for criada, usaremos novamente uma fonte de dados de entidade para acessar os dados do produto, mas, nesta instância, precisamos selecionar as entidades do produto e precisamos restringir os itens que são retornados apenas para aqueles da categoria selecionada.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="4b5b4-118">Para fazer isso, vamos dizer à EntityDataSource para gerar automaticamente a cláusula WHERE e especificaremos o WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="4b5b4-119">Você se lembrará de que quando criamos os itens de menu em nosso "menu de categoria do produto", criamos dinamicamente o link adicionando o CategoryID à QueryString para cada link.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CategoryID to the QueryString for each link.</span></span> <span data-ttu-id="4b5b4-120">Informaremos a fonte de dados de entidade para derivar o parâmetro WHERE do parâmetro QueryString.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="4b5b4-121">Em seguida, configuraremos o controle ListView para exibir uma lista de produtos.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="4b5b4-122">Para criar uma experiência de compra ideal, compactaremos vários recursos concisos em cada produto individual exibido em nosso ListVew.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="4b5b4-123">O nome do produto será um link para a exibição de detalhes do produto.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="4b5b4-124">O preço do produto será exibido.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="4b5b4-125">Uma imagem do produto será exibida e selecionaremos dinamicamente a imagem de um diretório de imagens de catálogo em nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="4b5b4-126">Incluiremos um link para adicionar imediatamente o produto específico ao carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="4b5b4-127">Aqui está a marcação para nossa instância de controle ListView.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="4b5b4-128">Estamos criando dinamicamente vários links para cada produto exibido.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="4b5b4-129">Além disso, antes de testarmos a própria página nova, precisamos criar a estrutura de diretório para as imagens do catálogo de produtos da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="4b5b4-130">Depois que nossas imagens de produto estiverem acessíveis, podemos testar nossa página de lista de produtos.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="4b5b4-131">Na home page do site, clique em um dos links da lista de categorias.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="4b5b4-132">Agora precisamos implementar a página ProductDetails. aspx e a funcionalidade AddToCart.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-132">Now we need to implement the ProductDetails.aspx page and the AddToCart functionality.</span></span>

<span data-ttu-id="4b5b4-133">Use o arquivo-&gt;novo para criar um nome de página ProductDetails. aspx usando a página mestra do site como fizemos anteriormente.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="4b5b4-134">Usaremos novamente um controle EntityDataSource para acessar o registro específico do produto no banco de dados e usaremos um controle FormView ASP.NET para exibir os dados do produto da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="4b5b4-135">Não se preocupe se a formatação parecer um pouco engraçado para você.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="4b5b4-136">A marcação acima deixa espaço no layout de exibição para alguns recursos que implementaremos mais tarde.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="4b5b4-137">O carrinho de compras representará a lógica mais complexa em nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="4b5b4-138">Para começar, use File-&gt;New para criar uma página chamada MyShoppingCart. aspx.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="4b5b4-139">Observe que não estamos escolhendo o nome ShoppingCart. aspx.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="4b5b4-140">Nosso banco de dados contém uma tabela chamada "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="4b5b4-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="4b5b4-141">Quando geramos um Modelo de Dados de Entidade uma classe foi criada para cada tabela no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="4b5b4-142">Portanto, o Modelo de Dados de Entidade gerou uma classe de entidade chamada "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="4b5b4-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="4b5b4-143">Poderíamos editar o modelo para que possamos usar esse nome para a implementação do carrinho de compras ou estendê-lo para nossas necessidades, mas, em vez disso, optaremos por simplesmente selecionar um nome que evitará o conflito.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply select a name that will avoid the conflict.</span></span>

<span data-ttu-id="4b5b4-144">Também vale a pena observar que iremos criar um carrinho de compras simples e inserir a lógica do carrinho de compras com a exibição do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="4b5b4-145">Também poderemos optar por implementar nosso carrinho de compras em uma camada comercial completamente separada.</span><span class="sxs-lookup"><span data-stu-id="4b5b4-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4b5b4-146">[Anterior](tailspin-spyworks-part-3.md)
> [Próximo](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="4b5b4-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
