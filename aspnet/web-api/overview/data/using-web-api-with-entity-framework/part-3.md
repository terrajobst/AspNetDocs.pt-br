---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Usar Migrações do Code First para propagar o banco de dados | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 257bd06848adb949330856cc71eeb3d685e9d036
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557452"
---
# <a name="use-code-first-migrations-to-seed-the-database"></a><span data-ttu-id="3b03d-102">Usar Migrações do Code First para propagar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="3b03d-102">Use Code First Migrations to Seed the Database</span></span>

<span data-ttu-id="3b03d-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3b03d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3b03d-104">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="3b03d-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="3b03d-105">Nesta seção, você usará o [migrações do Code First](https://msdn.microsoft.com/data/jj591621) no EF para propagar o banco de dados com dado de teste.</span><span class="sxs-lookup"><span data-stu-id="3b03d-105">In this section, you will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) in EF to seed the database with test data.</span></span>

<span data-ttu-id="3b03d-106">No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="3b03d-106">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="3b03d-107">Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="3b03d-107">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-3/samples/sample1.cmd)]

<span data-ttu-id="3b03d-108">Esse comando adiciona uma pasta chamada migrações ao seu projeto, além de um arquivo de código chamado Configuration.cs na pasta Migrations.</span><span class="sxs-lookup"><span data-stu-id="3b03d-108">This command adds a folder named Migrations to your project, plus a code file named Configuration.cs in the Migrations folder.</span></span>

![](part-3/_static/image1.png)

<span data-ttu-id="3b03d-109">Abra o arquivo Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="3b03d-109">Open the Configuration.cs file.</span></span> <span data-ttu-id="3b03d-110">Adicione a seguinte instrução **using** .</span><span class="sxs-lookup"><span data-stu-id="3b03d-110">Add the following **using** statement.</span></span>

[!code-csharp[Main](part-3/samples/sample2.cs)]

<span data-ttu-id="3b03d-111">Em seguida, adicione o seguinte código ao método **Configuration. semente** :</span><span class="sxs-lookup"><span data-stu-id="3b03d-111">Then add the following code to the **Configuration.Seed** method:</span></span>

[!code-csharp[Main](part-3/samples/sample3.cs)]

<span data-ttu-id="3b03d-112">Na janela do console do Gerenciador de pacotes, digite os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="3b03d-112">In the Package Manager Console window, type the following commands:</span></span>

[!code-console[Main](part-3/samples/sample4.cmd)]

<span data-ttu-id="3b03d-113">O primeiro comando gera o código que cria o banco de dados e o segundo comando executa esse código.</span><span class="sxs-lookup"><span data-stu-id="3b03d-113">The first command generates code that creates the database, and the second command executes that code.</span></span> <span data-ttu-id="3b03d-114">O banco de dados é criado localmente, usando o [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span><span class="sxs-lookup"><span data-stu-id="3b03d-114">The database is created locally, using [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).</span></span>

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a><span data-ttu-id="3b03d-115">Explorar a API (opcional)</span><span class="sxs-lookup"><span data-stu-id="3b03d-115">Explore the API (Optional)</span></span>

<span data-ttu-id="3b03d-116">Pressione F5 para executar o aplicativo em modo de depuração.</span><span class="sxs-lookup"><span data-stu-id="3b03d-116">Press F5 to run the application in debug mode.</span></span> <span data-ttu-id="3b03d-117">O Visual Studio inicia IIS Express e executa seu aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="3b03d-117">Visual Studio starts IIS Express and runs your web app.</span></span> <span data-ttu-id="3b03d-118">Em seguida, o Visual Studio inicia um navegador e abre a home page do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3b03d-118">Visual Studio then launches a browser and opens the app's home page.</span></span>

<span data-ttu-id="3b03d-119">Quando o Visual Studio executa um projeto Web, ele atribui um número de porta.</span><span class="sxs-lookup"><span data-stu-id="3b03d-119">When Visual Studio runs a web project, it assigns a port number.</span></span> <span data-ttu-id="3b03d-120">Na imagem abaixo, o número da porta é 50524.</span><span class="sxs-lookup"><span data-stu-id="3b03d-120">In the image below, the port number is 50524.</span></span> <span data-ttu-id="3b03d-121">Ao executar o aplicativo, você verá um número de porta diferente.</span><span class="sxs-lookup"><span data-stu-id="3b03d-121">When you run the application, you'll see a different port number.</span></span>

![](part-3/_static/image3.png)

<span data-ttu-id="3b03d-122">O home page é implementado usando o MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3b03d-122">The home page is implemented using ASP.NET MVC.</span></span> <span data-ttu-id="3b03d-123">Na parte superior da página, há um link que diz "API".</span><span class="sxs-lookup"><span data-stu-id="3b03d-123">At the top of the page, there is a link that says "API".</span></span> <span data-ttu-id="3b03d-124">Esse link leva você a uma página de ajuda gerada automaticamente para a API Web.</span><span class="sxs-lookup"><span data-stu-id="3b03d-124">This link brings you to an auto-generated help page for the web API.</span></span> <span data-ttu-id="3b03d-125">(Para saber como essa página de ajuda é gerada e como você pode adicionar sua própria documentação à página, consulte [criando páginas de ajuda para ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) Você pode clicar nos links da página de ajuda para ver detalhes sobre a API, incluindo o formato de solicitação e resposta.</span><span class="sxs-lookup"><span data-stu-id="3b03d-125">(To learn how this help page is generated, and how you can add your own documentation to the page, see [Creating Help Pages for ASP.NET Web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md).) You can click on the help page links to see details about the API, including the request and response format.</span></span>

![](part-3/_static/image4.png)

<span data-ttu-id="3b03d-126">A API habilita as operações CRUD no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3b03d-126">The API enables CRUD operations on the database.</span></span> <span data-ttu-id="3b03d-127">O seguinte resume a API.</span><span class="sxs-lookup"><span data-stu-id="3b03d-127">The following summarizes the API.</span></span>

| <span data-ttu-id="3b03d-128">Autores</span><span class="sxs-lookup"><span data-stu-id="3b03d-128">Authors</span></span> |  |
| --- | -- |
| <span data-ttu-id="3b03d-129">GET api/authors</span><span class="sxs-lookup"><span data-stu-id="3b03d-129">GET api/authors</span></span> | <span data-ttu-id="3b03d-130">Obter todos os autores.</span><span class="sxs-lookup"><span data-stu-id="3b03d-130">Get all authors.</span></span> |
| <span data-ttu-id="3b03d-131">GET api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="3b03d-131">GET api/authors/{id}</span></span> | <span data-ttu-id="3b03d-132">Obtenha um autor por ID.</span><span class="sxs-lookup"><span data-stu-id="3b03d-132">Get an author by ID.</span></span> |
| <span data-ttu-id="3b03d-133">POST /api/authors</span><span class="sxs-lookup"><span data-stu-id="3b03d-133">POST /api/authors</span></span> | <span data-ttu-id="3b03d-134">Crie um novo autor.</span><span class="sxs-lookup"><span data-stu-id="3b03d-134">Create a new author.</span></span> |
| <span data-ttu-id="3b03d-135">PUT /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="3b03d-135">PUT /api/authors/{id}</span></span> | <span data-ttu-id="3b03d-136">Atualizar um autor existente.</span><span class="sxs-lookup"><span data-stu-id="3b03d-136">Update an existing author.</span></span> |
| <span data-ttu-id="3b03d-137">DELETE /api/authors/{id}</span><span class="sxs-lookup"><span data-stu-id="3b03d-137">DELETE /api/authors/{id}</span></span> | <span data-ttu-id="3b03d-138">Excluir um autor.</span><span class="sxs-lookup"><span data-stu-id="3b03d-138">Delete an author.</span></span> |

| <span data-ttu-id="3b03d-139">Manuais</span><span class="sxs-lookup"><span data-stu-id="3b03d-139">Books</span></span> |  |
| --- | -- |
| <span data-ttu-id="3b03d-140">GET /api/books</span><span class="sxs-lookup"><span data-stu-id="3b03d-140">GET /api/books</span></span> | <span data-ttu-id="3b03d-141">Obter todos os livros.</span><span class="sxs-lookup"><span data-stu-id="3b03d-141">Get all books.</span></span> |
| <span data-ttu-id="3b03d-142">GET /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="3b03d-142">GET /api/books/{id}</span></span> | <span data-ttu-id="3b03d-143">Obter um livro por ID.</span><span class="sxs-lookup"><span data-stu-id="3b03d-143">Get a book by ID.</span></span> |
| <span data-ttu-id="3b03d-144">POST /api/books</span><span class="sxs-lookup"><span data-stu-id="3b03d-144">POST /api/books</span></span> | <span data-ttu-id="3b03d-145">Crie um novo livro.</span><span class="sxs-lookup"><span data-stu-id="3b03d-145">Create a new book.</span></span> |
| <span data-ttu-id="3b03d-146">PUT /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="3b03d-146">PUT /api/books/{id}</span></span> | <span data-ttu-id="3b03d-147">Atualizar um livro existente.</span><span class="sxs-lookup"><span data-stu-id="3b03d-147">Update an existing book.</span></span> |
| <span data-ttu-id="3b03d-148">DELETE /api/books/{id}</span><span class="sxs-lookup"><span data-stu-id="3b03d-148">DELETE /api/books/{id}</span></span> | <span data-ttu-id="3b03d-149">Excluir um livro.</span><span class="sxs-lookup"><span data-stu-id="3b03d-149">Delete a book.</span></span> |

## <a name="view-the-database-optional"></a><span data-ttu-id="3b03d-150">Exibir o banco de dados (opcional)</span><span class="sxs-lookup"><span data-stu-id="3b03d-150">View the Database (Optional)</span></span>

<span data-ttu-id="3b03d-151">Quando você executou o comando Update-Database, o EF criou o banco de dados e chamou o método `Seed`.</span><span class="sxs-lookup"><span data-stu-id="3b03d-151">When you ran the Update-Database command, EF created the database and called the `Seed` method.</span></span> <span data-ttu-id="3b03d-152">Quando você executa o aplicativo localmente, o EF usa o [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span><span class="sxs-lookup"><span data-stu-id="3b03d-152">When you run the application locally, EF uses [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx).</span></span> <span data-ttu-id="3b03d-153">Você pode exibir o banco de dados no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3b03d-153">You can view the database in Visual Studio.</span></span> <span data-ttu-id="3b03d-154">No menu **Visualizar**, selecione **Pesquisador de objetos do SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="3b03d-154">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

![](part-3/_static/image5.png)

<span data-ttu-id="3b03d-155">No diálogo **conectar ao servidor** , na caixa de edição **nome do servidor** , digite "(LocalDB) \v11.0".</span><span class="sxs-lookup"><span data-stu-id="3b03d-155">In the **Connect to Server** dialog, in the **Server Name** edit box, type "(localdb)\v11.0".</span></span> <span data-ttu-id="3b03d-156">Deixe a opção de **autenticação** como "autenticação do Windows".</span><span class="sxs-lookup"><span data-stu-id="3b03d-156">Leave the **Authentication** option as "Windows Authentication".</span></span> <span data-ttu-id="3b03d-157">Clique em **Conectar**.</span><span class="sxs-lookup"><span data-stu-id="3b03d-157">Click **Connect**.</span></span>

![](part-3/_static/image6.png)

<span data-ttu-id="3b03d-158">O Visual Studio se conecta ao LocalDB e mostra os bancos de dados existentes na janela Pesquisador de Objetos do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3b03d-158">Visual Studio connects to LocalDB and shows your existing databases in the SQL Server Object Explorer window.</span></span> <span data-ttu-id="3b03d-159">Você pode expandir os nós para ver as tabelas que o EF criou.</span><span class="sxs-lookup"><span data-stu-id="3b03d-159">You can expand the nodes to see the tables that EF created.</span></span>

![](part-3/_static/image7.png)

<span data-ttu-id="3b03d-160">Para exibir os dados, clique com o botão direito do mouse em uma tabela e selecione **exibir dados**.</span><span class="sxs-lookup"><span data-stu-id="3b03d-160">To view the data, right-click a table and select **View Data**.</span></span>

![](part-3/_static/image8.png)

<span data-ttu-id="3b03d-161">A captura de tela a seguir mostra os resultados da tabela Books.</span><span class="sxs-lookup"><span data-stu-id="3b03d-161">The following screenshot shows the results for the Books table.</span></span> <span data-ttu-id="3b03d-162">Observe que o EF populava o banco de dados com o semente data e a tabela contém a chave estrangeira para a tabela authors.</span><span class="sxs-lookup"><span data-stu-id="3b03d-162">Notice that EF populated the database with the seed data, and the table contains the foreign key to the Authors table.</span></span>

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> <span data-ttu-id="3b03d-163">[Anterior](part-2.md)
> [Próximo](part-4.md)</span><span class="sxs-lookup"><span data-stu-id="3b03d-163">[Previous](part-2.md)
[Next](part-4.md)</span></span>
