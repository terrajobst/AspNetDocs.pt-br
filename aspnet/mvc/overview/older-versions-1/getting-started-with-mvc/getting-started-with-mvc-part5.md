---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Acessando os dados do modelo de um controlador | Microsoft Docs
author: shanselman
description: Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Crie um aplicativo Web simples que lê e grava de um banco de dados.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 207ed880977d794d81efdc1ea458d17a68d501d8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543746"
---
# <a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="fc679-104">Acessar dados do modelo por meio de um controlador</span><span class="sxs-lookup"><span data-stu-id="fc679-104">Accessing your Model's Data from a Controller</span></span>

<span data-ttu-id="fc679-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="fc679-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="fc679-106">Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fc679-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="fc679-107">Você criará um aplicativo Web simples que lê e grava de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fc679-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="fc679-108">Visite o [ASP.NET MVC Learning Center](../../../index.md) para encontrar outros tutoriais e exemplos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="fc679-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="fc679-109">Nesta seção, vamos criar uma nova classe MoviesController e escrever um código que recupere nossos dados de filme e o exiba de volta para o navegador usando um modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="fc679-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="fc679-110">Clique com o botão direito do mouse na pasta controladores e faça um novo MoviesController.</span><span class="sxs-lookup"><span data-stu-id="fc679-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="fc679-111">[![adicionar controlador](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fc679-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="fc679-112">Isso criará um novo arquivo "MoviesController.cs" sob nossa pasta \Controllers em nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="fc679-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="fc679-113">Vamos atualizar o MovieController para recuperar a lista de filmes do nosso banco de dados populado recentemente.</span><span class="sxs-lookup"><span data-stu-id="fc679-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="fc679-114">Estamos executando uma consulta LINQ para que recuperemos apenas filmes lançados após o verão de 1984.</span><span class="sxs-lookup"><span data-stu-id="fc679-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="fc679-115">Precisaremos de um modelo de exibição para renderizar essa lista de filmes de volta, então clique com o botão direito do mouse no método e selecione Adicionar exibição para criá-lo.</span><span class="sxs-lookup"><span data-stu-id="fc679-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="fc679-116">Na caixa de diálogo Adicionar exibição, vamos indicar que estamos passando uma lista&lt;Movies. Models. Movie&gt; para nosso modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="fc679-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="fc679-117">Ao contrário das horas anteriores, usamos a caixa de diálogo Adicionar exibição e optamos por criar um modelo "vazio", desta vez indicaremos que queremos que o Visual Studio "Scaffold" automaticamente um modelo de exibição para nós com algum conteúdo padrão.</span><span class="sxs-lookup"><span data-stu-id="fc679-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="fc679-118">Faremos isso selecionando o item "listar" no menu suspenso "exibir conteúdo".</span><span class="sxs-lookup"><span data-stu-id="fc679-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="fc679-119">Lembre-se de que, quando você tiver criado uma nova classe, precisará compilar seu aplicativo para que ele apareça na caixa de diálogo Adicionar exibição.</span><span class="sxs-lookup"><span data-stu-id="fc679-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Adicionar modo de exibição](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="fc679-121">Clique em Adicionar e o sistema gerará automaticamente o código para um modo de exibição para nós que exibe nossa lista de filmes.</span><span class="sxs-lookup"><span data-stu-id="fc679-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="fc679-122">Esse é um bom momento para alterar o cabeçalho do &lt;H2&gt; para algo como "minha lista de filmes" como fizemos anteriormente com o Olá, Mundo exibição.</span><span class="sxs-lookup"><span data-stu-id="fc679-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="fc679-123">[Filmes de ![-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="fc679-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="fc679-124">Execute o aplicativo e visite/Movies na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="fc679-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="fc679-125">Agora, recuperamos os dados do Database usando uma consulta básica dentro do controlador e retornamos os dados a uma exibição que conhece os filmes.</span><span class="sxs-lookup"><span data-stu-id="fc679-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="fc679-126">Essa exibição, em seguida, gira na lista de filmes e cria uma tabela de dados para nós.</span><span class="sxs-lookup"><span data-stu-id="fc679-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="fc679-127">[Lista de filmes ![-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="fc679-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="fc679-128">Não implementaremos a funcionalidade de edição, detalhes e exclusão com este aplicativo. portanto, não precisamos dos links padrão que o modelo Scaffold criou para nós.</span><span class="sxs-lookup"><span data-stu-id="fc679-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="fc679-129">Abra o arquivo/Movies/Index.aspx e remova-o.</span><span class="sxs-lookup"><span data-stu-id="fc679-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="fc679-130">Aqui está o código-fonte para o que nosso modelo de exibição atualizado deve ter uma aparência assim que fizermos essas alterações:</span><span class="sxs-lookup"><span data-stu-id="fc679-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="fc679-131">Ele está criando links que não precisaremos, portanto, vamos excluí-los para este exemplo.</span><span class="sxs-lookup"><span data-stu-id="fc679-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="fc679-132">No entanto, vamos manter nosso novo link Create, como é o próximo!</span><span class="sxs-lookup"><span data-stu-id="fc679-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="fc679-133">Veja a aparência de nosso aplicativo com essa coluna removida.</span><span class="sxs-lookup"><span data-stu-id="fc679-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="fc679-134">[Lista de filmes ![-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="fc679-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="fc679-135">Agora temos uma lista simples de nossos dados de filme.</span><span class="sxs-lookup"><span data-stu-id="fc679-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="fc679-136">No entanto, se clicarmos no link "criar novo", obteremos um erro, pois ele não é conectado!</span><span class="sxs-lookup"><span data-stu-id="fc679-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="fc679-137">Vamos implementar um método Create Action e permitir que um usuário insira novos filmes em nosso banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fc679-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fc679-138">[Anterior](getting-started-with-mvc-part4.md)
> [Próximo](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="fc679-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
