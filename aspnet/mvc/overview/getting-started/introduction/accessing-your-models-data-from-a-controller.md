---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Acessando os dados do modelo de um controlador | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 5d882d765133d32d3acdba9ffb5d43b69119a273
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457226"
---
# <a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="2fba8-102">Acessar dados do seu modelo por meio de um controlador</span><span class="sxs-lookup"><span data-stu-id="2fba8-102">Accessing Your Model's Data from a Controller</span></span>

<span data-ttu-id="2fba8-103">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2fba8-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [Tutorial Note](index.md)]

<span data-ttu-id="2fba8-104">Nesta seção, você criará uma nova classe de `MoviesController` e escreverá o código que recupera os dados do filme e os exibe no navegador usando um modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="2fba8-104">In this section, you'll create a new `MoviesController` class and write code that retrieves the movie data and displays it in the browser using a view template.</span></span>

<span data-ttu-id="2fba8-105">**Compile o aplicativo** antes de prosseguir para a próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="2fba8-105">**Build the application** before going on to the next step.</span></span> <span data-ttu-id="2fba8-106">Se você não compilar o aplicativo, obterá um erro ao adicionar um controlador.</span><span class="sxs-lookup"><span data-stu-id="2fba8-106">If you don't build the application, you'll get an error adding a controller.</span></span>

<span data-ttu-id="2fba8-107">Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta *controladores* e clique em **Adicionar**e em **controlador**.</span><span class="sxs-lookup"><span data-stu-id="2fba8-107">In Solution Explorer, right-click the *Controllers* folder and then click **Add**, then **Controller**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image1.png)

<span data-ttu-id="2fba8-108">Na caixa de diálogo **Adicionar Scaffold** , clique em **controlador MVC 5 com exibições, usando Entity Framework**e, em seguida, clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="2fba8-108">In the **Add Scaffold** dialog box, click **MVC 5 Controller with views, using Entity Framework**, and then click **Add**.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- <span data-ttu-id="2fba8-109">Selecione **filme (MvcMovie. Models)** para a classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="2fba8-109">Select **Movie (MvcMovie.Models)** for the Model class.</span></span>
- <span data-ttu-id="2fba8-110">Selecione **MovieDBContext (MvcMovie. Models)** para a classe de contexto de dados.</span><span class="sxs-lookup"><span data-stu-id="2fba8-110">Select **MovieDBContext (MvcMovie.Models)** for the Data context class.</span></span>
- <span data-ttu-id="2fba8-111">Para o nome do controlador, insira **MoviesController**.</span><span class="sxs-lookup"><span data-stu-id="2fba8-111">For the Controller name enter **MoviesController**.</span></span>

  <span data-ttu-id="2fba8-112">A imagem abaixo mostra a caixa de diálogo concluído.</span><span class="sxs-lookup"><span data-stu-id="2fba8-112">The image below shows the completed dialog.</span></span>  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

<span data-ttu-id="2fba8-113">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="2fba8-113">Click **Add**.</span></span> <span data-ttu-id="2fba8-114">(Se você receber um erro, provavelmente não criou o aplicativo antes de começar a adicionar o controlador.) O Visual Studio cria os seguintes arquivos e pastas:</span><span class="sxs-lookup"><span data-stu-id="2fba8-114">(If you get an error, you probably didn't build the application before starting adding the controller.) Visual Studio creates the following files and folders:</span></span>

- <span data-ttu-id="2fba8-115">*Um arquivo MoviesController.cs* na pasta *Controllers* .</span><span class="sxs-lookup"><span data-stu-id="2fba8-115">*A MoviesController.cs* file in the *Controllers* folder.</span></span>
- <span data-ttu-id="2fba8-116">Uma pasta *Views\Movies* .</span><span class="sxs-lookup"><span data-stu-id="2fba8-116">A *Views\Movies* folder.</span></span>
- <span data-ttu-id="2fba8-117">*Crie. cshtml, Delete. cshtml, details. cshtml, Edit. cshtml*e *index. cshtml* na nova pasta *Views\Movies*</span><span class="sxs-lookup"><span data-stu-id="2fba8-117">*Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, and *Index.cshtml* in the new *Views\Movies* folder.</span></span>

<span data-ttu-id="2fba8-118">O Visual Studio criou automaticamente os métodos de ação [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (criar, ler, atualizar e excluir) para você (a criação automática de métodos e exibições de ação CRUD é conhecida como scaffolding).</span><span class="sxs-lookup"><span data-stu-id="2fba8-118">Visual Studio automatically created the [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views for you (the automatic creation of CRUD action methods and views is known as scaffolding).</span></span> <span data-ttu-id="2fba8-119">Agora você tem um aplicativo Web totalmente funcional que permite criar, listar, editar e excluir entradas de filme.</span><span class="sxs-lookup"><span data-stu-id="2fba8-119">You now have a fully functional web application that lets you create, list, edit, and delete movie entries.</span></span>

<span data-ttu-id="2fba8-120">Execute o aplicativo e clique no link do **filme do MVC** (ou navegue até o controlador `Movies` acrescentando */Movies* à URL na barra de endereços do seu navegador).</span><span class="sxs-lookup"><span data-stu-id="2fba8-120">Run the application and click on the **MVC Movie** link (or browse to the `Movies` controller by appending */Movies* to the URL in the address bar of your browser).</span></span> <span data-ttu-id="2fba8-121">Como o aplicativo depende do roteamento padrão (definido no arquivo de\_do *aplicativo Start\RouteConfig.cs* ), a solicitação do navegador `http://localhost:xxxxx/Movies` é roteada para o método de ação de `Index` padrão do controlador de `Movies`.</span><span class="sxs-lookup"><span data-stu-id="2fba8-121">Because the application is relying on the default routing (defined in the *App\_Start\RouteConfig.cs* file), the browser request `http://localhost:xxxxx/Movies` is routed to the default `Index` action method of the `Movies` controller.</span></span> <span data-ttu-id="2fba8-122">Em outras palavras, a solicitação do navegador `http://localhost:xxxxx/Movies` é efetivamente a mesma que a solicitação do navegador `http://localhost:xxxxx/Movies/Index`.</span><span class="sxs-lookup"><span data-stu-id="2fba8-122">In other words, the browser request `http://localhost:xxxxx/Movies` is effectively the same as the browser request `http://localhost:xxxxx/Movies/Index`.</span></span> <span data-ttu-id="2fba8-123">O resultado é uma lista vazia de filmes, pois você ainda não adicionou nenhum.</span><span class="sxs-lookup"><span data-stu-id="2fba8-123">The result is an empty list of movies, because you haven't added any yet.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a><span data-ttu-id="2fba8-124">Criando um filme</span><span class="sxs-lookup"><span data-stu-id="2fba8-124">Creating a Movie</span></span>

<span data-ttu-id="2fba8-125">Selecione o link **Criar Novo**.</span><span class="sxs-lookup"><span data-stu-id="2fba8-125">Select the **Create New** link.</span></span> <span data-ttu-id="2fba8-126">Insira alguns detalhes sobre um filme e, em seguida, clique no botão **criar** .</span><span class="sxs-lookup"><span data-stu-id="2fba8-126">Enter some details about a movie and then click the **Create** button.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="2fba8-127">Talvez você não consiga inserir pontos decimais ou vírgulas no campo preço.</span><span class="sxs-lookup"><span data-stu-id="2fba8-127">You may not be able to enter decimal points or commas in the Price field.</span></span> <span data-ttu-id="2fba8-128">Para dar suporte à validação do jQuery para localidades não inglesas que usam uma vírgula (&quot;,&quot;) para um ponto decimal, e os formatos de data em inglês dos EUA, você deve incluir *globalizable. js* e seu arquivo *culturas/globalizate. culturas* específico (de [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) e JavaScript para usar `Globalize.parseFloat`.</span><span class="sxs-lookup"><span data-stu-id="2fba8-128">To support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, and non US-English date formats, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="2fba8-129">Mostrarei como fazer isso no próximo tutorial.</span><span class="sxs-lookup"><span data-stu-id="2fba8-129">I'll show how to do this in the next tutorial.</span></span> <span data-ttu-id="2fba8-130">Por enquanto, digite apenas números inteiros como 10.</span><span class="sxs-lookup"><span data-stu-id="2fba8-130">For now, just enter whole numbers like 10.</span></span>

<span data-ttu-id="2fba8-131">Clicar no botão **criar** faz com que o formulário seja Postado no servidor, onde as informações do filme são salvas no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2fba8-131">Clicking the **Create** button causes the form to be posted to the server, where the movie information is saved in the database.</span></span> <span data-ttu-id="2fba8-132">Em seguida, você será redirecionado para a URL */Movies* , onde poderá ver o filme recém-criado na lista.</span><span class="sxs-lookup"><span data-stu-id="2fba8-132">You're then redirected to the */Movies* URL, where you can see the newly created movie in the listing.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image6.png)

<span data-ttu-id="2fba8-133">Crie duas mais entradas de filme adicionais.</span><span class="sxs-lookup"><span data-stu-id="2fba8-133">Create a couple more movie entries.</span></span> <span data-ttu-id="2fba8-134">Experimente os links **Editar**, **Detalhes** e **Excluir**, que estão todos funcionais.</span><span class="sxs-lookup"><span data-stu-id="2fba8-134">Try the **Edit**, **Details**, and **Delete** links, which are all functional.</span></span>

## <a name="examining-the-generated-code"></a><span data-ttu-id="2fba8-135">Examinando o código gerado</span><span class="sxs-lookup"><span data-stu-id="2fba8-135">Examining the Generated Code</span></span>

<span data-ttu-id="2fba8-136">Abra o arquivo *Controllers\MoviesController.cs* e examine o método `Index` gerado.</span><span class="sxs-lookup"><span data-stu-id="2fba8-136">Open the *Controllers\MoviesController.cs* file and examine the generated `Index` method.</span></span> <span data-ttu-id="2fba8-137">Uma parte do controlador de filme com o método `Index` é mostrada abaixo.</span><span class="sxs-lookup"><span data-stu-id="2fba8-137">A portion of the movie controller with the `Index` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

<span data-ttu-id="2fba8-138">Uma solicitação para o controlador de `Movies` retorna todas as entradas na tabela `Movies` e, em seguida, passa os resultados para a exibição `Index`.</span><span class="sxs-lookup"><span data-stu-id="2fba8-138">A request to the `Movies` controller returns all the entries in the `Movies` table and then passes the results to the `Index` view.</span></span> <span data-ttu-id="2fba8-139">A linha a seguir da classe `MoviesController` instancia um contexto de banco de dados de filme, conforme descrito anteriormente.</span><span class="sxs-lookup"><span data-stu-id="2fba8-139">The following line from the `MoviesController` class instantiates a movie database context, as described previously.</span></span> <span data-ttu-id="2fba8-140">Você pode usar o contexto de banco de dados de filme para consultar, editar e excluir filmes.</span><span class="sxs-lookup"><span data-stu-id="2fba8-140">You can use the movie database context to query, edit, and delete movies.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="2fba8-141">Modelos fortemente tipados e a palavra-chave @model</span><span class="sxs-lookup"><span data-stu-id="2fba8-141">Strongly Typed Models and the @model Keyword</span></span>

<span data-ttu-id="2fba8-142">Anteriormente neste tutorial, você viu como um controlador pode passar dados ou objetos para um modelo de exibição usando o objeto `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="2fba8-142">Earlier in this tutorial, you saw how a controller can pass data or objects to a view template using the `ViewBag` object.</span></span> <span data-ttu-id="2fba8-143">O `ViewBag` é um objeto dinâmico que fornece uma maneira conveniente de ligação tardia para passar informações para uma exibição.</span><span class="sxs-lookup"><span data-stu-id="2fba8-143">The `ViewBag` is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="2fba8-144">O MVC também fornece a capacidade de passar objetos *fortemente* tipados para um modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="2fba8-144">MVC also provides the ability to pass *strongly* typed objects to a view template.</span></span> <span data-ttu-id="2fba8-145">Essa abordagem fortemente tipada permite uma melhor verificação de tempo de compilação do seu código e do [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) mais rico no editor do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2fba8-145">This strongly typed approach enables better compile-time checking of your code and richer [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) in the Visual Studio editor.</span></span> <span data-ttu-id="2fba8-146">O mecanismo scaffolding no Visual Studio usava essa abordagem (isto é, passando um modelo *fortemente* tipado) com a classe `MoviesController` e os modelos de exibição ao criar os métodos e exibições.</span><span class="sxs-lookup"><span data-stu-id="2fba8-146">The scaffolding mechanism in Visual Studio used this approach (that is, passing a *strongly* typed model) with the `MoviesController` class and view templates when it created the methods and views.</span></span>

<span data-ttu-id="2fba8-147">No arquivo *Controllers\MoviesController.cs* , examine o método `Details` gerado.</span><span class="sxs-lookup"><span data-stu-id="2fba8-147">In the *Controllers\MoviesController.cs* file examine the generated `Details` method.</span></span> <span data-ttu-id="2fba8-148">O método `Details` é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="2fba8-148">The `Details` method is shown below.</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

<span data-ttu-id="2fba8-149">O parâmetro `id` geralmente é passado como dados de rota, por exemplo `http://localhost:1234/movies/details/1` definirá o controlador para o controlador de filme, a ação a ser `details`da e a `id` como 1.</span><span class="sxs-lookup"><span data-stu-id="2fba8-149">The `id` parameter is generally passed as route data, for example `http://localhost:1234/movies/details/1` will set the controller to the movie controller, the action to `details` and the `id` to 1.</span></span> <span data-ttu-id="2fba8-150">Você também pode passar a ID com uma cadeia de caracteres de consulta da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="2fba8-150">You could also pass in the id with a query string as follows:</span></span>

`http://localhost:1234/movies/details?id=1`

<span data-ttu-id="2fba8-151">Se um `Movie` for encontrado, uma instância do modelo de `Movie` será passada para a exibição `Details`:</span><span class="sxs-lookup"><span data-stu-id="2fba8-151">If a `Movie` is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

<span data-ttu-id="2fba8-152">Examine o conteúdo do arquivo *Views\Movies\Details.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2fba8-152">Examine the contents of the *Views\Movies\Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

<span data-ttu-id="2fba8-153">Ao incluir uma instrução `@model` na parte superior do arquivo de modelo de exibição, você pode especificar o tipo de objeto esperado pela exibição.</span><span class="sxs-lookup"><span data-stu-id="2fba8-153">By including a `@model` statement at the top of the view template file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="2fba8-154">Quando você criou o controlador de filmes, o Visual Studio incluiu automaticamente a seguinte instrução `@model` na parte superior do arquivo *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="2fba8-154">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Details.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

<span data-ttu-id="2fba8-155">Esta diretiva `@model` permite acessar o filme que o controlador passou para a exibição usando um objeto `Model` fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="2fba8-155">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="2fba8-156">Por exemplo, no modelo *Details. cshtml* , o código passa cada campo de filme para os auxiliares de HTML `DisplayNameFor` e [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) com o objeto de `Model` fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="2fba8-156">For example, in the *Details.cshtml* template, the code passes each movie field to the `DisplayNameFor` and [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="2fba8-157">Os métodos `Create` e `Edit` e modelos de exibição também passam um objeto de modelo de filme.</span><span class="sxs-lookup"><span data-stu-id="2fba8-157">The `Create` and `Edit` methods and view templates also pass a movie model object.</span></span>

<span data-ttu-id="2fba8-158">Examine o modelo de exibição *index. cshtml* e o método `Index` no arquivo *MoviesController.cs* .</span><span class="sxs-lookup"><span data-stu-id="2fba8-158">Examine the *Index.cshtml* view template and the `Index` method in the *MoviesController.cs* file.</span></span> <span data-ttu-id="2fba8-159">Observe como o código cria um objeto [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) ao chamar o método auxiliar `View` no método de ação `Index`.</span><span class="sxs-lookup"><span data-stu-id="2fba8-159">Notice how the code creates a [`List`](https://msdn.microsoft.com/library/6sh2ey19.aspx) object when it calls the `View` helper method in the `Index` action method.</span></span> <span data-ttu-id="2fba8-160">Em seguida, o código passa essa `Movies` lista do método de ação `Index` para a exibição:</span><span class="sxs-lookup"><span data-stu-id="2fba8-160">The code then passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

<span data-ttu-id="2fba8-161">Quando você criou o controlador de filme, o Visual Studio inclui automaticamente a seguinte instrução de `@model` na parte superior do arquivo *index. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="2fba8-161">When you created the movie controller, Visual Studio automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

<span data-ttu-id="2fba8-162">Essa diretiva de `@model` permite que você acesse a lista de filmes que o controlador passou para a exibição usando um objeto `Model` que é fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="2fba8-162">This `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="2fba8-163">Por exemplo, no modelo *index. cshtml* , o código percorre os filmes fazendo uma instrução `foreach` sobre o objeto `Model` fortemente tipado:</span><span class="sxs-lookup"><span data-stu-id="2fba8-163">For example, in the *Index.cshtml* template, the code loops through the movies by doing a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

<span data-ttu-id="2fba8-164">Como o objeto `Model` é fortemente tipado (como um objeto `IEnumerable<Movie>`), cada objeto `item` no loop é digitado como `Movie`.</span><span class="sxs-lookup"><span data-stu-id="2fba8-164">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each `item` object in the loop is typed as `Movie`.</span></span> <span data-ttu-id="2fba8-165">Entre outros benefícios, isso significa que você obtém a verificação em tempo de compilação do código e o suporte total ao IntelliSense no editor de códigos:</span><span class="sxs-lookup"><span data-stu-id="2fba8-165">Among other benefits, this means that you get compile-time checking of the code and full IntelliSense support in the code editor:</span></span>

![ModelIntelliSense](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a><span data-ttu-id="2fba8-167">Trabalhando com o SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="2fba8-167">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="2fba8-168">Entity Framework Code First detectou que a cadeia de conexão do banco de dados fornecida apontava para um banco de dados `Movies` que ainda não existia, portanto, Code First criou o banco de dados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="2fba8-168">Entity Framework Code First detected that the database connection string that was provided pointed to a `Movies` database that didn't exist yet, so Code First created the database automatically.</span></span> <span data-ttu-id="2fba8-169">Você pode verificar se ele foi criado examinando a pasta de *dados de\_do aplicativo* .</span><span class="sxs-lookup"><span data-stu-id="2fba8-169">You can verify that it's been created by looking in the *App\_Data* folder.</span></span> <span data-ttu-id="2fba8-170">Se você não vir o arquivo *Movies. MDF* , clique no botão **Mostrar todos os arquivos** na barra de ferramentas **Gerenciador de soluções** , clique no botão **Atualizar** e expanda a pasta *\_dados do aplicativo* .</span><span class="sxs-lookup"><span data-stu-id="2fba8-170">If you don't see the *Movies.mdf* file, click the **Show All Files** button in the **Solution Explorer** toolbar, click the **Refresh** button, and then expand the *App\_Data* folder.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image9.png)

<span data-ttu-id="2fba8-171">Clique duas vezes em *Movies. MDF* para abrir o **Gerenciador de servidores**e, em seguida, expanda a pasta **tabelas** para ver a tabela de filmes.</span><span class="sxs-lookup"><span data-stu-id="2fba8-171">Double-click *Movies.mdf* to open **SERVER EXPLORER**, then expand the **Tables** folder to see the Movies table.</span></span> <span data-ttu-id="2fba8-172">Observe o ícone de chave ao lado de ID.</span><span class="sxs-lookup"><span data-stu-id="2fba8-172">Note the key icon next to ID.</span></span> <span data-ttu-id="2fba8-173">Por padrão, o EF fará com que uma propriedade nomeada ID seja a chave primária.</span><span class="sxs-lookup"><span data-stu-id="2fba8-173">By default, EF will make a property named ID the primary key.</span></span> <span data-ttu-id="2fba8-174">Para obter mais informações sobre o EF e o MVC, consulte o excelente tutorial de Tom Dykstra sobre [MVC e EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="2fba8-174">For more information on EF and MVC, see Tom Dykstra's excellent tutorial on [MVC and EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

<span data-ttu-id="2fba8-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span><span class="sxs-lookup"><span data-stu-id="2fba8-175">![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")</span></span>

<span data-ttu-id="2fba8-176">Clique com o botão direito do mouse na tabela `Movies` e selecione **Mostrar dados da tabela** para ver os dados que você criou.</span><span class="sxs-lookup"><span data-stu-id="2fba8-176">Right-click the `Movies` table and select **Show Table Data** to see the data you created.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

<span data-ttu-id="2fba8-177">Clique com o botão direito do mouse na tabela `Movies` e selecione **Abrir definição de tabela** para ver a estrutura de tabela que Entity Framework Code First criada para você.</span><span class="sxs-lookup"><span data-stu-id="2fba8-177">Right-click the `Movies` table and select **Open Table Definition** to see the table structure that Entity Framework Code First created for you.</span></span>

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

<span data-ttu-id="2fba8-178">Observe como o esquema da tabela de `Movies` é mapeado para a classe `Movie` que você criou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="2fba8-178">Notice how the schema of the `Movies` table maps to the `Movie` class you created earlier.</span></span> <span data-ttu-id="2fba8-179">Entity Framework Code First criado automaticamente esse esquema para você com base em sua classe de `Movie`.</span><span class="sxs-lookup"><span data-stu-id="2fba8-179">Entity Framework Code First automatically created this schema for you based on your `Movie` class.</span></span>

<span data-ttu-id="2fba8-180">Quando tiver terminado, feche a conexão clicando com o botão direito do mouse em *MovieDBContext* e selecionando **fechar conexão**.</span><span class="sxs-lookup"><span data-stu-id="2fba8-180">When you're finished, close the connection by right clicking *MovieDBContext* and selecting **Close Connection**.</span></span> <span data-ttu-id="2fba8-181">(Se você não fechar a conexão, poderá receber um erro na próxima vez que executar o projeto).</span><span class="sxs-lookup"><span data-stu-id="2fba8-181">(If you don't close the connection, you might get an error the next time you run the project).</span></span>

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

<span data-ttu-id="2fba8-182">Agora você tem um banco de dados e páginas para exibir, editar, atualizar e excluir dados.</span><span class="sxs-lookup"><span data-stu-id="2fba8-182">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="2fba8-183">No próximo tutorial, examinaremos o restante do código com Scaffold e adicionaremos um método `SearchIndex` e uma exibição `SearchIndex` que permite pesquisar filmes nesse banco de dados.</span><span class="sxs-lookup"><span data-stu-id="2fba8-183">In the next tutorial, we'll examine the rest of the scaffolded code and add a `SearchIndex` method and a `SearchIndex` view that lets you search for movies in this database.</span></span> <span data-ttu-id="2fba8-184">Para obter mais informações sobre como usar Entity Framework com o MVC, consulte [criando um modelo de dados de Entity Framework para um aplicativo MVC ASP.net](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="2fba8-184">For more information on using Entity Framework with MVC, see [Creating an Entity Framework Data Model for an ASP.NET MVC Application](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2fba8-185">[Anterior](creating-a-connection-string.md)
> [Próximo](examining-the-edit-methods-and-edit-view.md)</span><span class="sxs-lookup"><span data-stu-id="2fba8-185">[Previous](creating-a-connection-string.md)
[Next](examining-the-edit-methods-and-edit-view.md)</span></span>
