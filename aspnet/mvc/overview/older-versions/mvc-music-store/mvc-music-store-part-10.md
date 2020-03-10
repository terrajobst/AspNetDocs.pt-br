---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Parte 10: atualizações finais de navegação e design de site, conclusão | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 10 aborda as atualizações finais para navegação e S...
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: f701d1fbabc3e1a97c3750d00e96bf8dba1105cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539364"
---
# <a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="19776-104">Parte 10: atualizações finais de navegação e design de site, conclusão</span><span class="sxs-lookup"><span data-stu-id="19776-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>

<span data-ttu-id="19776-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="19776-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="19776-106">A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.</span><span class="sxs-lookup"><span data-stu-id="19776-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="19776-107">A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="19776-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="19776-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="19776-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="19776-109">A parte 10 aborda as atualizações finais de navegação e design de site, conclusão.</span><span class="sxs-lookup"><span data-stu-id="19776-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>

<span data-ttu-id="19776-110">Concluímos toda a funcionalidade principal para nosso site, mas ainda temos alguns recursos para adicionar à navegação do site, ao home page e à página de navegação da loja.</span><span class="sxs-lookup"><span data-stu-id="19776-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="19776-111">Criando a exibição parcial do resumo do carrinho de compras</span><span class="sxs-lookup"><span data-stu-id="19776-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="19776-112">Queremos expor o número de itens no carrinho de compras do usuário em todo o site.</span><span class="sxs-lookup"><span data-stu-id="19776-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="19776-113">Podemos implementar isso facilmente criando uma exibição parcial que é adicionada ao nosso site. master.</span><span class="sxs-lookup"><span data-stu-id="19776-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="19776-114">Como mostrado anteriormente, o controlador ShoppingCart inclui um método de ação CartSummary que retorna uma exibição parcial:</span><span class="sxs-lookup"><span data-stu-id="19776-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="19776-115">Para criar a exibição parcial de CartSummary, clique com o botão direito do mouse na pasta views/ShoppingCart e selecione Add View.</span><span class="sxs-lookup"><span data-stu-id="19776-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="19776-116">Nomeie a exibição CartSummary e marque a caixa de seleção "criar uma exibição parcial", conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="19776-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="19776-117">A exibição parcial CartSummary é realmente simples – é apenas um link para a exibição do índice ShoppingCart que mostra o número de itens no carrinho.</span><span class="sxs-lookup"><span data-stu-id="19776-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="19776-118">O código completo para CartSummary. cshtml é o seguinte:</span><span class="sxs-lookup"><span data-stu-id="19776-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="19776-119">Podemos incluir uma exibição parcial em qualquer página no site, incluindo o site mestre, usando o método html. RenderAction.</span><span class="sxs-lookup"><span data-stu-id="19776-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="19776-120">Renderingaction exige que especifiquemos o nome da ação ("CartSummary") e o nome do controlador ("ShoppingCart") como mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="19776-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="19776-121">Antes de adicioná-lo ao layout do site, também criaremos o menu de gênero para que possamos fazer todas as nossas atualizações de site. Master ao mesmo tempo.</span><span class="sxs-lookup"><span data-stu-id="19776-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="19776-122">Criando a exibição parcial do menu de gênero</span><span class="sxs-lookup"><span data-stu-id="19776-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="19776-123">Podemos tornar muito mais fácil para nossos usuários navegar pela loja adicionando um menu de gênero que lista todos os gêneros disponíveis em nossa loja.</span><span class="sxs-lookup"><span data-stu-id="19776-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="19776-124">Seguiremos as mesmas etapas também criam uma exibição parcial GenreMenu e, em seguida, podemos adicioná-las ao mestre do site.</span><span class="sxs-lookup"><span data-stu-id="19776-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="19776-125">Primeiro, adicione a seguinte ação do controlador GenreMenu ao StoreController:</span><span class="sxs-lookup"><span data-stu-id="19776-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="19776-126">Essa ação retorna uma lista de gêneros que serão exibidos pela exibição parcial, que criaremos a seguir.</span><span class="sxs-lookup"><span data-stu-id="19776-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="19776-127">*Observação: adicionamos o atributo [ChildActionOnly] a essa ação do controlador, que indica que só queremos que essa ação seja usada em uma exibição parcial. Esse atributo impedirá que a ação do controlador seja executada navegando até/Store/GenreMenu. Isso não é necessário para exibições parciais, mas é uma boa prática, pois queremos garantir que nossas ações do controlador sejam usadas como pretendida. Também estamos retornando PartialView em vez de View, que permite ao mecanismo de exibição saber que ele não deve usar o layout dessa exibição, pois está sendo incluído em outras exibições.*</span><span class="sxs-lookup"><span data-stu-id="19776-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="19776-128">Clique com o botão direito do mouse na ação do controlador GenreMenu e crie uma exibição parcial chamada GenreMenu, que é fortemente tipada usando a classe de dados de exibição de gênero, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="19776-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="19776-129">Atualize o código de exibição para a exibição parcial de GenreMenu para exibir os itens usando uma lista não ordenada da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="19776-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="19776-130">Atualizando layout do site para exibir nossas exibições parciais</span><span class="sxs-lookup"><span data-stu-id="19776-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="19776-131">Podemos adicionar nossas exibições parciais ao layout do site (/Views/Shared/\_layout. cshtml) chamando HTML. RenderAction ().</span><span class="sxs-lookup"><span data-stu-id="19776-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="19776-132">Vamos adicioná-los no, bem como algumas marcações adicionais para exibi-los, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="19776-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="19776-133">Agora, quando executarmos o aplicativo, veremos o gênero na área de navegação à esquerda e o resumo do carrinho na parte superior.</span><span class="sxs-lookup"><span data-stu-id="19776-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="19776-134">Atualizar para a página de navegação da loja</span><span class="sxs-lookup"><span data-stu-id="19776-134">Update to the Store Browse page</span></span>

<span data-ttu-id="19776-135">A página de navegação da loja é funcional, mas não parece muito boa.</span><span class="sxs-lookup"><span data-stu-id="19776-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="19776-136">Podemos atualizar a página para mostrar os álbuns em um layout melhor atualizando o código de exibição (encontrado em/Views/Store/Browse.cshtml) da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="19776-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="19776-137">Aqui estamos fazendo uso de URL. Action em vez de HTML. ActionLink para que possamos aplicar formatação especial ao link para incluir a arte-final do álbum.</span><span class="sxs-lookup"><span data-stu-id="19776-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="19776-138">*Observação: estamos exibindo uma capa de álbum genérico para esses álbuns. Essas informações são armazenadas no banco de dados e são editáveis por meio do Gerenciador da loja. Você é bem-vindo a adicionar seu próprio trabalho artístico.*</span><span class="sxs-lookup"><span data-stu-id="19776-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="19776-139">Agora, quando navegarmos até um gênero, veremos os álbuns mostrados em uma grade com a arte-final do álbum.</span><span class="sxs-lookup"><span data-stu-id="19776-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="19776-140">Atualizando a Home Page para mostrar os principais álbuns de vendas</span><span class="sxs-lookup"><span data-stu-id="19776-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="19776-141">Queremos apresentar nossos principais álbuns de vendas na home page para aumentar as vendas.</span><span class="sxs-lookup"><span data-stu-id="19776-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="19776-142">Faremos algumas atualizações em nosso HomeController para lidar com isso e também adicionaremos alguns elementos gráficos adicionais.</span><span class="sxs-lookup"><span data-stu-id="19776-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="19776-143">Primeiro, adicionaremos uma propriedade de navegação à nossa classe de álbum para que o EntityFramework saiba que está associado.</span><span class="sxs-lookup"><span data-stu-id="19776-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="19776-144">As últimas linhas da nossa classe de **álbum** agora devem ser assim:</span><span class="sxs-lookup"><span data-stu-id="19776-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="19776-145">*Observação: isso exigirá a adição de uma instrução using para trazer o namespace System. Collections. Generic.*</span><span class="sxs-lookup"><span data-stu-id="19776-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="19776-146">Primeiro, adicionaremos um campo storeDB e o MvcMusicStore. Models usando instruções, como em nossos outros controladores.</span><span class="sxs-lookup"><span data-stu-id="19776-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="19776-147">Em seguida, adicionaremos o seguinte método ao HomeController, que consulta nosso banco de dados para localizar os principais álbuns de vendas de acordo com OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="19776-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="19776-148">Esse é um método particular, pois não queremos disponibilizá-lo como uma ação de controlador.</span><span class="sxs-lookup"><span data-stu-id="19776-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="19776-149">Estamos incluindo isso no HomeController para simplificar, mas você é incentivado a mover sua lógica de negócios para classes de serviço separadas, conforme apropriado.</span><span class="sxs-lookup"><span data-stu-id="19776-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="19776-150">Com isso em vigor, podemos atualizar a ação do controlador de índice para consultar os 5 principais álbuns de vendas e retorná-los à exibição.</span><span class="sxs-lookup"><span data-stu-id="19776-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="19776-151">O código completo para o HomeController atualizado é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="19776-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="19776-152">Por fim, precisaremos atualizar nossa exibição de índice doméstico para que ela possa exibir uma lista de álbuns atualizando o tipo de modelo e adicionando a lista de álbuns à parte inferior.</span><span class="sxs-lookup"><span data-stu-id="19776-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="19776-153">Levaremos essa oportunidade para adicionar também um título e uma seção de promoção à página.</span><span class="sxs-lookup"><span data-stu-id="19776-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="19776-154">Agora, quando executarmos o aplicativo, veremos os home page atualizados com os principais álbuns de vendas e nossa mensagem promocional.</span><span class="sxs-lookup"><span data-stu-id="19776-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="19776-155">Conclusão</span><span class="sxs-lookup"><span data-stu-id="19776-155">Conclusion</span></span>

<span data-ttu-id="19776-156">Vimos que o ASP.NET MVC facilita a criação de um site sofisticado com acesso ao banco de dados, associação, AJAX, etc.</span><span class="sxs-lookup"><span data-stu-id="19776-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="19776-157">muito rapidamente.</span><span class="sxs-lookup"><span data-stu-id="19776-157">pretty quickly.</span></span> <span data-ttu-id="19776-158">Espero que este tutorial tenha lhe dado as ferramentas necessárias para começar a criar seus próprios aplicativos MVC do ASP.NET!</span><span class="sxs-lookup"><span data-stu-id="19776-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="19776-159">Anterior</span><span class="sxs-lookup"><span data-stu-id="19776-159">Previous</span></span>](mvc-music-store-part-9.md)
