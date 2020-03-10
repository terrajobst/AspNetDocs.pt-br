---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Parte 5: editar formulários e Templating | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 5 aborda os formulários de edição e modelagem.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: 20b99cbe57b5dfa623205838a5929733a6c2d70d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559545"
---
# <a name="part-5-edit-forms-and-templating"></a><span data-ttu-id="64b27-104">Parte 5: editar formulários e modelos</span><span class="sxs-lookup"><span data-stu-id="64b27-104">Part 5: Edit Forms and Templating</span></span>

<span data-ttu-id="64b27-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="64b27-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="64b27-106">A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.</span><span class="sxs-lookup"><span data-stu-id="64b27-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="64b27-107">A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="64b27-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="64b27-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="64b27-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="64b27-109">A parte 5 aborda os formulários de edição e modelagem.</span><span class="sxs-lookup"><span data-stu-id="64b27-109">Part 5 covers Edit Forms and Templating.</span></span>

<span data-ttu-id="64b27-110">No capítulo anterior, estávamos carregando dados de nosso banco de dado e exibindo-os.</span><span class="sxs-lookup"><span data-stu-id="64b27-110">In the past chapter, we were loading data from our database and displaying it.</span></span> <span data-ttu-id="64b27-111">Neste capítulo, também Habilitaremos a edição dos dados.</span><span class="sxs-lookup"><span data-stu-id="64b27-111">In this chapter, we'll also enable editing the data.</span></span>

## <a name="creating-the-storemanagercontroller"></a><span data-ttu-id="64b27-112">Criando o StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="64b27-112">Creating the StoreManagerController</span></span>

<span data-ttu-id="64b27-113">Vamos começar criando um novo controlador chamado **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="64b27-113">We'll begin by creating a new controller called **StoreManagerController**.</span></span> <span data-ttu-id="64b27-114">Para esse controlador, aproveitaremos os recursos do scaffolding disponíveis na atualização das ferramentas do ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="64b27-114">For this controller, we will be taking advantage of the Scaffolding features available in the ASP.NET MVC 3 Tools Update.</span></span> <span data-ttu-id="64b27-115">Defina as opções para a caixa de diálogo Adicionar controlador, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="64b27-115">Set the options for the Add Controller dialog as shown below.</span></span>

![](mvc-music-store-part-5/_static/image1.png)

<span data-ttu-id="64b27-116">Ao clicar no botão Adicionar, você verá que o mecanismo ASP.NET MVC 3 scaffolding faz uma boa quantidade de trabalho para você:</span><span class="sxs-lookup"><span data-stu-id="64b27-116">When you click the Add button, you'll see that the ASP.NET MVC 3 scaffolding mechanism does a good amount of work for you:</span></span>

- <span data-ttu-id="64b27-117">Ele cria o novo StoreManagerController com uma variável de Entity Framework local</span><span class="sxs-lookup"><span data-stu-id="64b27-117">It creates the new StoreManagerController with a local Entity Framework variable</span></span>
- <span data-ttu-id="64b27-118">Ele adiciona uma pasta Storemanager à pasta views do projeto</span><span class="sxs-lookup"><span data-stu-id="64b27-118">It adds a StoreManager folder to the project's Views folder</span></span>
- <span data-ttu-id="64b27-119">Ele adiciona Create. cshtml, Delete. cshtml, details. cshtml, Edit. cshtml e index. cshtml View, fortemente tipado à classe Album</span><span class="sxs-lookup"><span data-stu-id="64b27-119">It adds Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml, and Index.cshtml view, strongly typed to the Album class</span></span>

![](mvc-music-store-part-5/_static/image2.png)

<span data-ttu-id="64b27-120">A nova classe do controlador Storemanager inclui ações de controlador CRUD (criar, ler, atualizar, excluir) que sabem como trabalhar com a classe de modelo de álbum e usar nosso contexto de Entity Framework para acesso ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="64b27-120">The new StoreManager controller class includes CRUD (create, read, update, delete) controller actions which know how to work with the Album model class and use our Entity Framework context for database access.</span></span>

## <a name="modifying-a-scaffolded-view"></a><span data-ttu-id="64b27-121">Modificando uma exibição do com Scaffold</span><span class="sxs-lookup"><span data-stu-id="64b27-121">Modifying a Scaffolded View</span></span>

<span data-ttu-id="64b27-122">É importante lembrar que, embora esse código tenha sido gerado para nós, ele é o código MVC padrão do ASP.NET, assim como escrevemos durante todo este tutorial.</span><span class="sxs-lookup"><span data-stu-id="64b27-122">It's important to remember that, while this code was generated for us, it's standard ASP.NET MVC code, just like we've been writing throughout this tutorial.</span></span> <span data-ttu-id="64b27-123">Ele se destina a poupar o tempo que você gastaria escrevendo código de controlador clichê e criando exibições com rigidez de tipos manualmente, mas esse não é o tipo de código gerado que você pode ter visto precedido com avisos de dire em comentários sobre como você não deve alterar o auto-completar.</span><span class="sxs-lookup"><span data-stu-id="64b27-123">It's intended to save you the time you'd spend on writing boilerplate controller code and creating the strongly typed views manually, but this isn't the kind of generated code you may have seen prefaced with dire warnings in comments about how you mustn't change the code.</span></span> <span data-ttu-id="64b27-124">Esse é o seu código e você deve alterá-lo.</span><span class="sxs-lookup"><span data-stu-id="64b27-124">This is your code, and you're expected to change it.</span></span>

<span data-ttu-id="64b27-125">Portanto, vamos começar com uma edição rápida para a exibição do índice do Storemanager (/Views/StoreManager/Index.cshtml).</span><span class="sxs-lookup"><span data-stu-id="64b27-125">So, let's start with a quick edit to the StoreManager Index view (/Views/StoreManager/Index.cshtml).</span></span> <span data-ttu-id="64b27-126">Essa exibição exibirá uma tabela que lista os álbuns em nossa loja com editar/detalhes/excluir links e inclui as propriedades públicas do álbum.</span><span class="sxs-lookup"><span data-stu-id="64b27-126">This view will display a table which lists the Albums in our store with Edit / Details / Delete links, and includes the Album's public properties.</span></span> <span data-ttu-id="64b27-127">Removeremos o campo AlbumArtUrl, pois ele não é muito útil nessa exibição.</span><span class="sxs-lookup"><span data-stu-id="64b27-127">We'll remove the AlbumArtUrl field, as it's not very useful in this display.</span></span> <span data-ttu-id="64b27-128">Na tabela &lt;&gt; seção do código de exibição, remova os elementos &lt;th&gt; e &lt;TD&gt; ao redor das referências AlbumArtUrl, conforme indicado pelas linhas realçadas abaixo:</span><span class="sxs-lookup"><span data-stu-id="64b27-128">In &lt;table&gt; section of the view code, remove the &lt;th&gt; and &lt;td&gt; elements surrounding AlbumArtUrl references, as indicated by the highlighted lines below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

<span data-ttu-id="64b27-129">O código de exibição modificado será exibido da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="64b27-129">The modified view code will appear as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a><span data-ttu-id="64b27-130">Uma primeira olhada no Store Manager</span><span class="sxs-lookup"><span data-stu-id="64b27-130">A first look at the Store Manager</span></span>

<span data-ttu-id="64b27-131">Agora execute o aplicativo e navegue até/StoreManager/.</span><span class="sxs-lookup"><span data-stu-id="64b27-131">Now run the application and browse to /StoreManager/.</span></span> <span data-ttu-id="64b27-132">Isso exibe o índice do Store Manager que acabamos de modificar, mostrando uma lista dos álbuns no repositório com links para editar, detalhar e excluir.</span><span class="sxs-lookup"><span data-stu-id="64b27-132">This displays the Store Manager Index we just modified, showing a list of the albums in the store with links to Edit, Details, and Delete.</span></span>

![](mvc-music-store-part-5/_static/image3.png)

<span data-ttu-id="64b27-133">Clicar no link Editar exibe um formulário de edição com campos para o álbum, incluindo os menus suspensos para gênero e artista.</span><span class="sxs-lookup"><span data-stu-id="64b27-133">Clicking the Edit link displays an edit form with fields for the Album, including dropdowns for Genre and Artist.</span></span>

![](mvc-music-store-part-5/_static/image4.png)

<span data-ttu-id="64b27-134">Clique no link "voltar à lista" na parte inferior e, em seguida, clique no link detalhes de um álbum.</span><span class="sxs-lookup"><span data-stu-id="64b27-134">Click the "Back to List" link at the bottom, then click on the Details link for an Album.</span></span> <span data-ttu-id="64b27-135">Isso exibe as informações detalhadas de um álbum individual.</span><span class="sxs-lookup"><span data-stu-id="64b27-135">This displays the detail information for an individual Album.</span></span>

![](mvc-music-store-part-5/_static/image5.png)

<span data-ttu-id="64b27-136">Novamente, clique no link voltar à lista e, em seguida, clique em um link de exclusão.</span><span class="sxs-lookup"><span data-stu-id="64b27-136">Again, click the Back to List link, then click on a Delete link.</span></span> <span data-ttu-id="64b27-137">Isso exibe uma caixa de diálogo de confirmação, mostrando os detalhes do álbum e perguntando se temos certeza de que desejamos excluí-lo.</span><span class="sxs-lookup"><span data-stu-id="64b27-137">This displays a confirmation dialog, showing the album details and asking if we're sure we want to delete it.</span></span>

![](mvc-music-store-part-5/_static/image6.png)

<span data-ttu-id="64b27-138">Clicar no botão excluir na parte inferior excluirá o álbum e o retornará para a página de índice, que mostra o álbum excluído.</span><span class="sxs-lookup"><span data-stu-id="64b27-138">Clicking the Delete button at the bottom will delete the album and return you to the Index page, which shows the album deleted.</span></span>

<span data-ttu-id="64b27-139">Não fazemos isso com o Store Manager, mas temos o controlador de trabalho e o código de exibição para as operações CRUD começarem.</span><span class="sxs-lookup"><span data-stu-id="64b27-139">We're not done with the Store Manager, but we have working controller and view code for the CRUD operations to start from.</span></span>

## <a name="looking-at-the-store-manager-controller-code"></a><span data-ttu-id="64b27-140">Examinando o código do controlador do Store Manager</span><span class="sxs-lookup"><span data-stu-id="64b27-140">Looking at the Store Manager Controller code</span></span>

<span data-ttu-id="64b27-141">O controlador do Store Manager contém uma boa quantidade de código.</span><span class="sxs-lookup"><span data-stu-id="64b27-141">The Store Manager Controller contains a good amount of code.</span></span> <span data-ttu-id="64b27-142">Vamos percorrer isso de cima para baixo.</span><span class="sxs-lookup"><span data-stu-id="64b27-142">Let's go through this from top to bottom.</span></span> <span data-ttu-id="64b27-143">O controlador inclui alguns namespaces padrão para um controlador MVC, bem como uma referência ao nosso namespace de modelos.</span><span class="sxs-lookup"><span data-stu-id="64b27-143">The controller includes some standard namespaces for an MVC controller, as well as a reference to our Models namespace.</span></span> <span data-ttu-id="64b27-144">O controlador tem uma instância privada de MusicStoreEntities, usada por cada uma das ações do controlador para acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="64b27-144">The controller has a private instance of MusicStoreEntities, used by each of the controller actions for data access.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a><span data-ttu-id="64b27-145">Ações de índice e detalhes do Store Manager</span><span class="sxs-lookup"><span data-stu-id="64b27-145">Store Manager Index and Details actions</span></span>

<span data-ttu-id="64b27-146">A exibição de índice recupera uma lista de álbuns, incluindo as informações de gênero e artista referenciadas de cada álbum, como vimos anteriormente ao trabalhar no método de procura da loja.</span><span class="sxs-lookup"><span data-stu-id="64b27-146">The index view retrieves a list of Albums, including each album's referenced Genre and Artist information, as we previously saw when working on the Store Browse method.</span></span> <span data-ttu-id="64b27-147">A exibição de índice está seguindo as referências aos objetos vinculados para que ele possa exibir o nome do gênero e o nome do artista de cada álbum, para que o controlador esteja sendo eficiente e consultando essas informações na solicitação original.</span><span class="sxs-lookup"><span data-stu-id="64b27-147">The Index view is following the references to the linked objects so that it can display each album's Genre name and Artist name, so the controller is being efficient and querying for this information in the original request.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

<span data-ttu-id="64b27-148">A ação do controlador de detalhes do controlador Storemanager funciona exatamente da mesma forma que a ação de detalhes do controlador da loja que escrevemos anteriormente. ele consulta o álbum por ID usando o método Find () e, em seguida, retorna-o para a exibição.</span><span class="sxs-lookup"><span data-stu-id="64b27-148">The StoreManager Controller's Details controller action works exactly the same as the Store Controller Details action we wrote previously - it queries for the Album by ID using the Find() method, then returns it to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a><span data-ttu-id="64b27-149">Os métodos de ação Create</span><span class="sxs-lookup"><span data-stu-id="64b27-149">The Create Action Methods</span></span>

<span data-ttu-id="64b27-150">Os métodos de ação Create são um pouco diferentes daqueles que vimos até agora, pois manipulam a entrada de formulário.</span><span class="sxs-lookup"><span data-stu-id="64b27-150">The Create action methods are a little different from ones we've seen so far, because they handle form input.</span></span> <span data-ttu-id="64b27-151">Quando um usuário visita pela primeira vez/StoreManager/Create/, ele mostrará um formulário vazio.</span><span class="sxs-lookup"><span data-stu-id="64b27-151">When a user first visits /StoreManager/Create/ they will be shown an empty form.</span></span> <span data-ttu-id="64b27-152">Essa página HTML conterá um &lt;formulário&gt; elemento que contém os elementos de entrada DropDown e TextBox, em que eles podem inserir os detalhes do álbum.</span><span class="sxs-lookup"><span data-stu-id="64b27-152">This HTML page will contain a &lt;form&gt; element that contains dropdown and textbox input elements where they can enter the album's details.</span></span>

<span data-ttu-id="64b27-153">Depois que o usuário preenche os valores de formulário do álbum, eles podem pressionar o botão "salvar" para enviar essas alterações de volta ao nosso aplicativo para salvar no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="64b27-153">After the user fills in the Album form values, they can press the "Save" button to submit these changes back to our application to save within the database.</span></span> <span data-ttu-id="64b27-154">Quando o usuário pressionar o botão "salvar", o formulário de &lt;&gt; executará um HTTP-POST de volta para a URL/StoreManager/Create/e enviará os valores&gt; formulário de &lt;como parte do HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="64b27-154">When the user presses the "save" button the &lt;form&gt; will perform an HTTP-POST back to the /StoreManager/Create/ URL and submit the &lt;form&gt; values as part of the HTTP-POST.</span></span>

<span data-ttu-id="64b27-155">O ASP.NET MVC nos permite dividir facilmente a lógica desses dois cenários de invocação de URL, permitindo que implementem dois métodos de ação "Create" separados em nossa classe StoreManagerController – um para tratar o HTTP inicial GET para a URL/StoreManager/Create/e o outro para manipular o HTTP-POST das alterações enviadas.</span><span class="sxs-lookup"><span data-stu-id="64b27-155">ASP.NET MVC allows us to easily split up the logic of these two URL invocation scenarios by enabling us to implement two separate "Create" action methods within our StoreManagerController class – one to handle the initial HTTP-GET browse to the /StoreManager/Create/ URL, and the other to handle the HTTP-POST of the submitted changes.</span></span>

### <a name="passing-information-to-a-view-using-viewbag"></a><span data-ttu-id="64b27-156">Passando informações para uma exibição usando ViewBag</span><span class="sxs-lookup"><span data-stu-id="64b27-156">Passing information to a View using ViewBag</span></span>

<span data-ttu-id="64b27-157">Usamos a ViewBag anteriormente neste tutorial, mas não falamos muito sobre isso.</span><span class="sxs-lookup"><span data-stu-id="64b27-157">We've used the ViewBag earlier in this tutorial, but haven't talked much about it.</span></span> <span data-ttu-id="64b27-158">ViewBag nos permite passar informações para a exibição sem usar um objeto de modelo fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="64b27-158">The ViewBag allows us to pass information to the view without using a strongly typed model object.</span></span> <span data-ttu-id="64b27-159">Nesse caso, nossa ação editar controlador HTTP-GET precisa passar uma lista de gêneros e artistas para o formulário para preencher os menus suspensos e a maneira mais simples de fazer isso é retorná-los como itens ViewBag.</span><span class="sxs-lookup"><span data-stu-id="64b27-159">In this case, our Edit HTTP-GET controller action needs to pass both a list of Genres and Artists to the form to populate the dropdowns, and the simplest way to do that is to return them as ViewBag items.</span></span>

<span data-ttu-id="64b27-160">ViewBag é um objeto dinâmico, o que significa que você pode digitar ViewBag. foo ou ViewBag. YourNameHere sem escrever código para definir essas propriedades.</span><span class="sxs-lookup"><span data-stu-id="64b27-160">The ViewBag is a dynamic object, meaning that you can type ViewBag.Foo or ViewBag.YourNameHere without writing code to define those properties.</span></span> <span data-ttu-id="64b27-161">Nesse caso, o código do controlador usa ViewBag. Gêneroid e ViewBag. Artistaid para que os valores suspensos enviados com o formulário sejam Gêneroid e Artistaid, que são as propriedades do álbum que serão configuradas.</span><span class="sxs-lookup"><span data-stu-id="64b27-161">In this case, the controller code uses ViewBag.GenreId and ViewBag.ArtistId so that the dropdown values submitted with the form will be GenreId and ArtistId, which are the Album properties they will be setting.</span></span>

<span data-ttu-id="64b27-162">Esses valores suspensos são retornados para o formulário usando o objeto SelectList, que é criado apenas para essa finalidade.</span><span class="sxs-lookup"><span data-stu-id="64b27-162">These dropdown values are returned to the form using the SelectList object, which is built just for that purpose.</span></span> <span data-ttu-id="64b27-163">Isso é feito usando um código como este:</span><span class="sxs-lookup"><span data-stu-id="64b27-163">This is done using code like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

<span data-ttu-id="64b27-164">Como você pode ver no código do método de ação, três parâmetros estão sendo usados para criar esse objeto:</span><span class="sxs-lookup"><span data-stu-id="64b27-164">As you can see from the action method code, three parameters are being used to create this object:</span></span>

- <span data-ttu-id="64b27-165">A lista de itens que o menu suspenso exibirá.</span><span class="sxs-lookup"><span data-stu-id="64b27-165">The list of items the dropdown will be displaying.</span></span> <span data-ttu-id="64b27-166">Observe que isso não é apenas uma cadeia de caracteres – estamos passando uma lista de gêneros.</span><span class="sxs-lookup"><span data-stu-id="64b27-166">Note that this isn't just a string - we're passing a list of Genres.</span></span>
- <span data-ttu-id="64b27-167">O próximo parâmetro que está sendo passado para a SelectList é o valor selecionado.</span><span class="sxs-lookup"><span data-stu-id="64b27-167">The next parameter being passed to the SelectList is the Selected Value.</span></span> <span data-ttu-id="64b27-168">Esta é a maneira como a lista de seleção sabe como selecionar previamente um item na lista.</span><span class="sxs-lookup"><span data-stu-id="64b27-168">This how the SelectList knows how to pre-select an item in the list.</span></span> <span data-ttu-id="64b27-169">Isso será mais fácil de entender quando observarmos o formulário de edição, que é bastante semelhante.</span><span class="sxs-lookup"><span data-stu-id="64b27-169">This will be easier to understand when we look at the Edit form, which is pretty similar.</span></span>
- <span data-ttu-id="64b27-170">O parâmetro final é a propriedade a ser exibida.</span><span class="sxs-lookup"><span data-stu-id="64b27-170">The final parameter is the property to be displayed.</span></span> <span data-ttu-id="64b27-171">Nesse caso, isso indica que a propriedade Genre.Name é o que será exibido para o usuário.</span><span class="sxs-lookup"><span data-stu-id="64b27-171">In this case, this is indicating that the Genre.Name property is what will be shown to the user.</span></span>

<span data-ttu-id="64b27-172">Com isso em mente, a ação de criação de HTTP-GET é bem simples-duas SelectLists são adicionadas a ViewBag e nenhum objeto de modelo é passado para o formulário (já que ainda não foi criado).</span><span class="sxs-lookup"><span data-stu-id="64b27-172">With that in mind, then, the HTTP-GET Create action is pretty simple - two SelectLists are added to the ViewBag, and no model object is passed to the form (since it hasn't been created yet).</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a><span data-ttu-id="64b27-173">Auxiliares HTML para exibir os menus suspensos na exibição criar</span><span class="sxs-lookup"><span data-stu-id="64b27-173">HTML Helpers to display the Drop Downs in the Create View</span></span>

<span data-ttu-id="64b27-174">Como falamos sobre como os valores de lista suspensa são passados para a exibição, vamos dar uma olhada rápida na exibição para ver como esses valores são exibidos.</span><span class="sxs-lookup"><span data-stu-id="64b27-174">Since we've talked about how the drop down values are passed to the view, let's take a quick look at the view to see how those values are displayed.</span></span> <span data-ttu-id="64b27-175">No código de exibição (/Views/StoreManager/Create.cshtml), você verá que a seguinte chamada é feita para exibir a lista suspensa gênero.</span><span class="sxs-lookup"><span data-stu-id="64b27-175">In the view code (/Views/StoreManager/Create.cshtml), you'll see the following call is made to display the Genre drop down.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

<span data-ttu-id="64b27-176">Isso é conhecido como um auxiliar HTML – um método utilitário que executa uma tarefa de exibição comum.</span><span class="sxs-lookup"><span data-stu-id="64b27-176">This is known as an HTML Helper - a utility method which performs a common view task.</span></span> <span data-ttu-id="64b27-177">Os auxiliares HTML são muito úteis para manter nosso código de exibição conciso e legível.</span><span class="sxs-lookup"><span data-stu-id="64b27-177">HTML Helpers are very useful in keeping our view code concise and readable.</span></span> <span data-ttu-id="64b27-178">O auxiliar HTML. DropDownList é fornecido pelo ASP.NET MVC, mas como veremos posteriormente, é possível criar nossos próprios auxiliares para o código de exibição que vamos reutilizar em nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="64b27-178">The Html.DropDownList helper is provided by ASP.NET MVC, but as we'll see later it's possible to create our own helpers for view code we'll reuse in our application.</span></span>

<span data-ttu-id="64b27-179">A chamada de HTML. DropDownList só precisa ser formada por duas coisas – onde obter a lista para exibição e qual valor (se houver) deve ser pré-selecionado.</span><span class="sxs-lookup"><span data-stu-id="64b27-179">The Html.DropDownList call just needs to be told two things - where to get the list to display, and what value (if any) should be pre-selected.</span></span> <span data-ttu-id="64b27-180">O primeiro parâmetro, Gêneroid, informa ao DropDownList para procurar um valor chamado Gêneroid no modelo ou ViewBag.</span><span class="sxs-lookup"><span data-stu-id="64b27-180">The first parameter, GenreId, tells the DropDownList to look for a value named GenreId in either the model or ViewBag.</span></span> <span data-ttu-id="64b27-181">O segundo parâmetro é usado para indicar o valor a ser mostrado como selecionado inicialmente na lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="64b27-181">The second parameter is used to indicate the value to show as initially selected in the drop down list.</span></span> <span data-ttu-id="64b27-182">Como esse formulário é um formulário de criação, não há nenhum valor a ser selecionado e String. Empty é passado.</span><span class="sxs-lookup"><span data-stu-id="64b27-182">Since this form is a Create form, there's no value to be preselected and String.Empty is passed.</span></span>

### <a name="handling-the-posted-form-values"></a><span data-ttu-id="64b27-183">Manipulando os valores de formulário postados</span><span class="sxs-lookup"><span data-stu-id="64b27-183">Handling the Posted Form values</span></span>

<span data-ttu-id="64b27-184">Como discutimos anteriormente, há dois métodos de ação associados a cada formulário.</span><span class="sxs-lookup"><span data-stu-id="64b27-184">As we discussed before, there are two action methods associated with each form.</span></span> <span data-ttu-id="64b27-185">O primeiro manipula a solicitação HTTP-GET e exibe o formulário.</span><span class="sxs-lookup"><span data-stu-id="64b27-185">The first handles the HTTP-GET request and displays the form.</span></span> <span data-ttu-id="64b27-186">O segundo manipula a solicitação HTTP-POST, que contém os valores de formulário enviados.</span><span class="sxs-lookup"><span data-stu-id="64b27-186">The second handles the HTTP-POST request, which contains the submitted form values.</span></span> <span data-ttu-id="64b27-187">Observe que a ação do controlador tem um atributo [HttpPost], que informa ao ASP.NET MVC que ele só deve responder às solicitações HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="64b27-187">Notice that controller action has an [HttpPost] attribute, which tells ASP.NET MVC that it should only respond to HTTP-POST requests.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

<span data-ttu-id="64b27-188">Esta ação tem quatro responsabilidades:</span><span class="sxs-lookup"><span data-stu-id="64b27-188">This action has four responsibilities:</span></span>

- 1. <span data-ttu-id="64b27-189">Ler os valores de formulário</span><span class="sxs-lookup"><span data-stu-id="64b27-189">Read the form values</span></span>
- 2. <span data-ttu-id="64b27-190">Verificar se os valores de formulário passam por qualquer regra de validação</span><span class="sxs-lookup"><span data-stu-id="64b27-190">Check if the form values pass any validation rules</span></span>
- 3. <span data-ttu-id="64b27-191">Se o envio do formulário for válido, salve os dados e exiba a lista atualizada</span><span class="sxs-lookup"><span data-stu-id="64b27-191">If the form submission is valid, save the data and display the updated list</span></span>
- 4. <span data-ttu-id="64b27-192">Se o envio do formulário não for válido, exiba novamente o formulário com erros de validação</span><span class="sxs-lookup"><span data-stu-id="64b27-192">If the form submission is not valid, redisplay the form with validation errors</span></span>

#### <a name="reading-form-values-with-model-binding"></a><span data-ttu-id="64b27-193">Lendo valores de formulário com associação de modelo</span><span class="sxs-lookup"><span data-stu-id="64b27-193">Reading Form Values with Model Binding</span></span>

<span data-ttu-id="64b27-194">A ação do controlador está processando um envio de formulário que inclui valores para Gêneroid e Artistaid (da lista suspensa) e valores de caixa de texto para título, preço e AlbumArtUrl.</span><span class="sxs-lookup"><span data-stu-id="64b27-194">The controller action is processing a form submission that includes values for GenreId and ArtistId (from the drop down list) and textbox values for Title, Price, and AlbumArtUrl.</span></span> <span data-ttu-id="64b27-195">Embora seja possível acessar diretamente os valores de formulário, uma abordagem melhor é usar os recursos de associação de modelo criados no ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="64b27-195">While it's possible to directly access form values, a better approach is to use the Model Binding capabilities built into ASP.NET MVC.</span></span> <span data-ttu-id="64b27-196">Quando uma ação do controlador usa um tipo de modelo como parâmetro, o ASP.NET MVC tentará preencher um objeto desse tipo usando entradas de formulário (bem como valores de Route e QueryString).</span><span class="sxs-lookup"><span data-stu-id="64b27-196">When a controller action takes a model type as a parameter, ASP.NET MVC will attempt to populate an object of that type using form inputs (as well as route and querystring values).</span></span> <span data-ttu-id="64b27-197">Ele faz isso procurando valores cujos nomes correspondem às propriedades do objeto de modelo, por exemplo, ao definir o valor de Gêneroid do novo objeto de álbum, ele procura uma entrada com o nome Gêneroid.</span><span class="sxs-lookup"><span data-stu-id="64b27-197">It does this by looking for values whose names match properties of the model object, e.g. when setting the new Album object's GenreId value, it looks for an input with the name GenreId.</span></span> <span data-ttu-id="64b27-198">Quando você cria modos de exibição usando os métodos padrão no ASP.NET MVC, os formulários sempre serão renderizados usando nomes de propriedade como nomes de campo de entrada, de modo que os nomes dos campos só corresponderão.</span><span class="sxs-lookup"><span data-stu-id="64b27-198">When you create views using the standard methods in ASP.NET MVC, the forms will always be rendered using property names as input field names, so this the field names will just match up.</span></span>

#### <a name="validating-the-model"></a><span data-ttu-id="64b27-199">Validando o modelo</span><span class="sxs-lookup"><span data-stu-id="64b27-199">Validating the Model</span></span>

<span data-ttu-id="64b27-200">O modelo é validado com uma chamada simples para ModelState. IsValid.</span><span class="sxs-lookup"><span data-stu-id="64b27-200">The model is validated with a simple call to ModelState.IsValid.</span></span> <span data-ttu-id="64b27-201">Ainda não adicionamos nenhuma regra de validação à nossa classe de álbum – faremos isso em breve. agora, essa verificação não tem muito a ver.</span><span class="sxs-lookup"><span data-stu-id="64b27-201">We haven't added any validation rules to our Album class yet - we'll do that in a bit - so right now this check doesn't have much to do.</span></span> <span data-ttu-id="64b27-202">O que é importante é que essa verificação de ModelStat. IsValid se adaptará às regras de validação que colocamos em nosso modelo, assim as alterações futuras nas regras de validação não exigirão nenhuma atualização para o código de ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="64b27-202">What's important is that this ModelStat.IsValid check will adapt to the validation rules we put on our model, so future changes to validation rules won't require any updates to the controller action code.</span></span>

#### <a name="saving-the-submitted-values"></a><span data-ttu-id="64b27-203">Salvando os valores enviados</span><span class="sxs-lookup"><span data-stu-id="64b27-203">Saving the submitted values</span></span>

<span data-ttu-id="64b27-204">Se o envio do formulário passar na validação, será hora de salvar os valores no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="64b27-204">If the form submission passes validation, it's time to save the values to the database.</span></span> <span data-ttu-id="64b27-205">Com Entity Framework, isso apenas requer a adição do modelo à coleção de álbuns e a chamada a SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="64b27-205">With Entity Framework, that just requires adding the model to the Albums collection and calling SaveChanges.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

<span data-ttu-id="64b27-206">Entity Framework gera os comandos SQL apropriados para manter o valor.</span><span class="sxs-lookup"><span data-stu-id="64b27-206">Entity Framework generates the appropriate SQL commands to persist the value.</span></span> <span data-ttu-id="64b27-207">Depois de salvar os dados, redirecionamos de volta para a lista de álbuns para que possamos ver nossa atualização.</span><span class="sxs-lookup"><span data-stu-id="64b27-207">After saving the data, we redirect back to the list of Albums so we can see our update.</span></span> <span data-ttu-id="64b27-208">Isso é feito retornando RedirectToAction com o nome da ação do controlador que desejamos exibir.</span><span class="sxs-lookup"><span data-stu-id="64b27-208">This is done by returning RedirectToAction with the name of the controller action we want displayed.</span></span> <span data-ttu-id="64b27-209">Nesse caso, esse é o método de índice.</span><span class="sxs-lookup"><span data-stu-id="64b27-209">In this case, that's the Index method.</span></span>

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a><span data-ttu-id="64b27-210">Exibindo envios de formulário inválidos com erros de validação</span><span class="sxs-lookup"><span data-stu-id="64b27-210">Displaying invalid form submissions with Validation Errors</span></span>

<span data-ttu-id="64b27-211">No caso de entrada de formulário inválida, os valores suspensos são adicionados ao ViewBag (como no caso de HTTP-GET) e os valores do modelo associado são passados de volta para a exibição para exibição.</span><span class="sxs-lookup"><span data-stu-id="64b27-211">In the case of invalid form input, the dropdown values are added to the ViewBag (as in the HTTP-GET case) and the bound model values are passed back to the view for display.</span></span> <span data-ttu-id="64b27-212">Os erros de validação são exibidos automaticamente usando o @Html.ValidationMessageFor HTML Helper.</span><span class="sxs-lookup"><span data-stu-id="64b27-212">Validation errors are automatically displayed using the @Html.ValidationMessageFor HTML Helper.</span></span>

#### <a name="testing-the-create-form"></a><span data-ttu-id="64b27-213">Testando o formulário de criação</span><span class="sxs-lookup"><span data-stu-id="64b27-213">Testing the Create Form</span></span>

<span data-ttu-id="64b27-214">Para testar isso, execute o aplicativo e navegue até/StoreManager/Create/-isso lhe mostrará o formulário em branco que foi retornado pelo método Create HTTP-GET de StoreController.</span><span class="sxs-lookup"><span data-stu-id="64b27-214">To test this out, run the application and browse to /StoreManager/Create/ - this will show you the blank form which was returned by the StoreController Create HTTP-GET method.</span></span>

<span data-ttu-id="64b27-215">Preencha alguns valores e clique no botão criar para enviar o formulário.</span><span class="sxs-lookup"><span data-stu-id="64b27-215">Fill in some values and click the Create button to submit the form.</span></span>

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a><span data-ttu-id="64b27-216">Manipulando edições</span><span class="sxs-lookup"><span data-stu-id="64b27-216">Handling Edits</span></span>

<span data-ttu-id="64b27-217">O par editar ação (HTTP-GET e HTTP-POST) são muito semelhantes aos métodos de ação de criação que acabamos de examinar.</span><span class="sxs-lookup"><span data-stu-id="64b27-217">The Edit action pair (HTTP-GET and HTTP-POST) are very similar to the Create action methods we just looked at.</span></span> <span data-ttu-id="64b27-218">Como o cenário de edição envolve trabalhar com um álbum existente, o método Edit HTTP-GET carrega o álbum com base no parâmetro "ID", passado por meio da rota.</span><span class="sxs-lookup"><span data-stu-id="64b27-218">Since the edit scenario involves working with an existing album, the Edit HTTP-GET method loads the Album based on the "id" parameter, passed in via the route.</span></span> <span data-ttu-id="64b27-219">Esse código para recuperar um álbum por albumid é o mesmo que vimos anteriormente na ação do controlador de detalhes.</span><span class="sxs-lookup"><span data-stu-id="64b27-219">This code for retrieving an album by AlbumId is the same as we've previously looked at in the Details controller action.</span></span> <span data-ttu-id="64b27-220">Assim como acontece com o método Create/HTTP-GET, os valores suspensos são retornados por meio de ViewBag.</span><span class="sxs-lookup"><span data-stu-id="64b27-220">As with the Create / HTTP-GET method, the drop down values are returned via the ViewBag.</span></span> <span data-ttu-id="64b27-221">Isso nos permite retornar um álbum como nosso objeto de modelo para a exibição (que é fortemente tipada para a classe de álbum) ao passar dados adicionais (por exemplo, uma lista de gêneros) por meio de ViewBag.</span><span class="sxs-lookup"><span data-stu-id="64b27-221">This allows us to return an Album as our model object to the view (which is strongly typed to the Album class) while passing additional data (e.g. a list of Genres) via the ViewBag.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

<span data-ttu-id="64b27-222">A ação editar HTTP-POST é muito semelhante à ação criar HTTP-POST.</span><span class="sxs-lookup"><span data-stu-id="64b27-222">The Edit HTTP-POST action is very similar to the Create HTTP-POST action.</span></span> <span data-ttu-id="64b27-223">A única diferença é que, em vez de adicionar um novo álbum ao BD. Coleção de álbuns, estamos encontrando a instância atual do álbum usando o BD. Entrada (álbum) e definir seu estado como modificado.</span><span class="sxs-lookup"><span data-stu-id="64b27-223">The only difference is that instead of adding a new album to the db.Albums collection, we're finding the current instance of the Album using db.Entry(album) and setting its state to Modified.</span></span> <span data-ttu-id="64b27-224">Isso informa Entity Framework que estamos modificando um álbum existente em vez de criar um novo.</span><span class="sxs-lookup"><span data-stu-id="64b27-224">This tells Entity Framework that we are modifying an existing album as opposed to creating a new one.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

<span data-ttu-id="64b27-225">Podemos testar isso executando o aplicativo e navegando até/StoreManger/e clicando no link editar para um álbum.</span><span class="sxs-lookup"><span data-stu-id="64b27-225">We can test this out by running the application and browsing to /StoreManger/, then clicking the Edit link for an album.</span></span>

![](mvc-music-store-part-5/_static/image9.png)

<span data-ttu-id="64b27-226">Isso exibe o formulário de edição mostrado pelo método editar HTTP-GET.</span><span class="sxs-lookup"><span data-stu-id="64b27-226">This displays the Edit form shown by the Edit HTTP-GET method.</span></span> <span data-ttu-id="64b27-227">Preencha alguns valores e clique no botão salvar.</span><span class="sxs-lookup"><span data-stu-id="64b27-227">Fill in some values and click the Save button.</span></span>

![](mvc-music-store-part-5/_static/image10.png)

<span data-ttu-id="64b27-228">Isso posta o formulário, salva os valores e nos retorna à lista de álbuns, mostrando que os valores foram atualizados.</span><span class="sxs-lookup"><span data-stu-id="64b27-228">This posts the form, saves the values, and returns us to the Album list, showing that the values were updated.</span></span>

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a><span data-ttu-id="64b27-229">Tratamento da exclusão</span><span class="sxs-lookup"><span data-stu-id="64b27-229">Handling Deletion</span></span>

<span data-ttu-id="64b27-230">A exclusão segue o mesmo padrão que editar e criar, usando uma ação do controlador para exibir o formulário de confirmação e outra ação do controlador para lidar com o envio do formulário.</span><span class="sxs-lookup"><span data-stu-id="64b27-230">Deletion follows the same pattern as Edit and Create, using one controller action to display the confirmation form, and another controller action to handle the form submission.</span></span>

<span data-ttu-id="64b27-231">A ação HTTP-obter controlador de exclusão é exatamente a mesma que nossa ação anterior do controlador de detalhes do Store Manager.</span><span class="sxs-lookup"><span data-stu-id="64b27-231">The HTTP-GET Delete controller action is exactly the same as our previous Store Manager Details controller action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

<span data-ttu-id="64b27-232">Exibimos um formulário fortemente tipado para um tipo de álbum, usando o modelo excluir conteúdo de exibição.</span><span class="sxs-lookup"><span data-stu-id="64b27-232">We display a form that's strongly typed to an Album type, using the Delete view content template.</span></span>

![](mvc-music-store-part-5/_static/image12.png)

<span data-ttu-id="64b27-233">O modelo de exclusão mostra todos os campos para o modelo, mas podemos simplificar isso um pouco.</span><span class="sxs-lookup"><span data-stu-id="64b27-233">The Delete template shows all the fields for the model, but we can simplify that down quite a bit.</span></span> <span data-ttu-id="64b27-234">Altere o código de exibição em/Views/StoreManager/Delete.cshtml para o seguinte.</span><span class="sxs-lookup"><span data-stu-id="64b27-234">Change the view code in /Views/StoreManager/Delete.cshtml to the following.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

<span data-ttu-id="64b27-235">Isso exibe uma confirmação de exclusão simplificada.</span><span class="sxs-lookup"><span data-stu-id="64b27-235">This displays a simplified Delete confirmation.</span></span>

![](mvc-music-store-part-5/_static/image13.png)

<span data-ttu-id="64b27-236">Clicar no botão excluir faz com que o formulário seja Postado de volta para o servidor, que executa a ação DeleteConfirmed.</span><span class="sxs-lookup"><span data-stu-id="64b27-236">Clicking the Delete button causes the form to be posted back to the server, which executes the DeleteConfirmed action.</span></span>

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

<span data-ttu-id="64b27-237">Nossa ação do controlador HTTP-POST Delete executa as seguintes ações:</span><span class="sxs-lookup"><span data-stu-id="64b27-237">Our HTTP-POST Delete Controller Action takes the following actions:</span></span>

- 1. <span data-ttu-id="64b27-238">Carrega o álbum por ID</span><span class="sxs-lookup"><span data-stu-id="64b27-238">Loads the Album by ID</span></span>
- 2. <span data-ttu-id="64b27-239">Exclui o álbum e salva as alterações</span><span class="sxs-lookup"><span data-stu-id="64b27-239">Deletes it the album and save changes</span></span>
- 3. <span data-ttu-id="64b27-240">Redireciona para o índice, mostrando que o álbum foi removido da lista</span><span class="sxs-lookup"><span data-stu-id="64b27-240">Redirects to the Index, showing that the Album was removed from the list</span></span>

<span data-ttu-id="64b27-241">Para testar isso, execute o aplicativo e navegue até/StoreManager.</span><span class="sxs-lookup"><span data-stu-id="64b27-241">To test this, run the application and browse to /StoreManager.</span></span> <span data-ttu-id="64b27-242">Selecione um álbum na lista e clique no link excluir.</span><span class="sxs-lookup"><span data-stu-id="64b27-242">Select an album from the list and click the Delete link.</span></span>

![](mvc-music-store-part-5/_static/image14.png)

<span data-ttu-id="64b27-243">Isso exibe nossa tela de confirmação de exclusão.</span><span class="sxs-lookup"><span data-stu-id="64b27-243">This displays our Delete confirmation screen.</span></span>

![](mvc-music-store-part-5/_static/image15.png)

<span data-ttu-id="64b27-244">Clicar no botão excluir remove o álbum e nos retorna à página de índice do Store Manager, que mostra que o álbum foi excluído.</span><span class="sxs-lookup"><span data-stu-id="64b27-244">Clicking the Delete button removes the album and returns us to the Store Manager Index page, which shows that the album has been deleted.</span></span>

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a><span data-ttu-id="64b27-245">Usando um auxiliar HTML personalizado para truncar o texto</span><span class="sxs-lookup"><span data-stu-id="64b27-245">Using a custom HTML Helper to truncate text</span></span>

<span data-ttu-id="64b27-246">Temos um possível problema com nossa página de índice do Store Manager.</span><span class="sxs-lookup"><span data-stu-id="64b27-246">We've got one potential issue with our Store Manager Index page.</span></span> <span data-ttu-id="64b27-247">As propriedades título do nosso álbum e nome do artista podem ser longas o suficiente para que possam lançar nossa formatação de tabela.</span><span class="sxs-lookup"><span data-stu-id="64b27-247">Our Album Title and Artist Name properties can both be long enough that they could throw off our table formatting.</span></span> <span data-ttu-id="64b27-248">Vamos criar um auxiliar HTML personalizado para nos permitir truncar facilmente essas e outras propriedades em nossas exibições.</span><span class="sxs-lookup"><span data-stu-id="64b27-248">We'll create a custom HTML Helper to allow us to easily truncate these and other properties in our Views.</span></span>

![](mvc-music-store-part-5/_static/image17.png)

<span data-ttu-id="64b27-249">A sintaxe @helper do Razor tornou muito fácil criar suas próprias funções auxiliares para uso em suas exibições.</span><span class="sxs-lookup"><span data-stu-id="64b27-249">Razor's @helper syntax has made it pretty easy to create your own helper functions for use in your views.</span></span> <span data-ttu-id="64b27-250">Abra a exibição/Views/StoreManager/Index.cshtml e adicione o código a seguir diretamente após a linha de @model.</span><span class="sxs-lookup"><span data-stu-id="64b27-250">Open the /Views/StoreManager/Index.cshtml view and add the following code directly after the @model line.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

<span data-ttu-id="64b27-251">Esse método auxiliar usa uma cadeia de caracteres e um comprimento máximo para permitir.</span><span class="sxs-lookup"><span data-stu-id="64b27-251">This helper method takes a string and a maximum length to allow.</span></span> <span data-ttu-id="64b27-252">Se o texto fornecido for menor que o comprimento especificado, o auxiliar o produzirá como está.</span><span class="sxs-lookup"><span data-stu-id="64b27-252">If the text supplied is shorter than the length specified, the helper outputs it as-is.</span></span> <span data-ttu-id="64b27-253">Se for maior, ele truncará o texto e renderizará "..." para o restante.</span><span class="sxs-lookup"><span data-stu-id="64b27-253">If it is longer, then it truncates the text and renders "…" for the remainder.</span></span>

<span data-ttu-id="64b27-254">Agora, podemos usar nosso auxiliar TRUNCATE para garantir que as propriedades título do álbum e nome do artista tenham menos de 25 caracteres.</span><span class="sxs-lookup"><span data-stu-id="64b27-254">Now we can use our Truncate helper to ensure that both the Album Title and Artist Name properties are less than 25 characters.</span></span> <span data-ttu-id="64b27-255">O código de exibição completo usando nosso novo auxiliar TRUNCATE aparece abaixo.</span><span class="sxs-lookup"><span data-stu-id="64b27-255">The complete view code using our new Truncate helper appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

<span data-ttu-id="64b27-256">Agora, quando navegamos pela URL do/StoreManager/, os álbuns e os títulos são mantidos abaixo de nossos comprimentos máximos.</span><span class="sxs-lookup"><span data-stu-id="64b27-256">Now when we browse the /StoreManager/ URL, the albums and titles are kept below our maximum lengths.</span></span>

![](mvc-music-store-part-5/_static/image18.png)

<span data-ttu-id="64b27-257">Observação: isso mostra o caso simples de criar e usar um auxiliar em uma exibição.</span><span class="sxs-lookup"><span data-stu-id="64b27-257">Note: This shows the simple case of creating and using a helper in one view.</span></span> <span data-ttu-id="64b27-258">Para saber mais sobre como criar auxiliares que podem ser usados em todo o site, consulte minha postagem no blog: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span><span class="sxs-lookup"><span data-stu-id="64b27-258">To learn more about creating helpers that you can use throughout your site, see my blog post: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="64b27-259">[Anterior](mvc-music-store-part-4.md)
> [Próximo](mvc-music-store-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="64b27-259">[Previous](mvc-music-store-part-4.md)
[Next](mvc-music-store-part-6.md)</span></span>
