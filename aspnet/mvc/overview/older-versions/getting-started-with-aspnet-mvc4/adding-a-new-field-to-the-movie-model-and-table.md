---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Adicionando um novo campo ao modelo de filme e à tabela | Microsoft Docs
author: Rick-Anderson
description: 'Observação: uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstrar...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d966b95163f64b20a17d2327a12c5d6c44a4a66b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615083"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table"></a><span data-ttu-id="f5f13-104">Adicionar um novo campo ao modelo de filme e à tabela</span><span class="sxs-lookup"><span data-stu-id="f5f13-104">Adding a New Field to the Movie Model and Table</span></span>

<span data-ttu-id="f5f13-105">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f5f13-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> > [!NOTE]
> > <span data-ttu-id="f5f13-106">Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="f5f13-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="f5f13-107">É mais seguro, muito mais simples de seguir e demonstra mais recursos.</span><span class="sxs-lookup"><span data-stu-id="f5f13-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>

<span data-ttu-id="f5f13-108">Nesta seção, você usará Migrações do Entity Framework Code First para migrar algumas alterações nas classes de modelo para que a alteração seja aplicada ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f5f13-108">In this section you'll use Entity Framework Code First Migrations to migrate some changes to the model classes so the change is applied to the database.</span></span>

<span data-ttu-id="f5f13-109">Por padrão, quando você usa Entity Framework Code First para criar automaticamente um banco de dados, como fazia anteriormente neste tutorial, Code First adiciona uma tabela ao banco de dados para ajudar a controlar se o esquema do banco de dados está em sincronia com as classes de modelo das quais ele foi gerado.</span><span class="sxs-lookup"><span data-stu-id="f5f13-109">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="f5f13-110">Se eles não estiverem em sincronia, o Entity Framework gerará um erro.</span><span class="sxs-lookup"><span data-stu-id="f5f13-110">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="f5f13-111">Isso facilita o rastreamento de problemas em tempo de desenvolvimento que, de outra forma, você pode localizar (por erros obscuros) em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="f5f13-111">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span>

## <a name="setting-up-code-first-migrations-for-model-changes"></a><span data-ttu-id="f5f13-112">Configurando Migrações do Code First para alterações de modelo</span><span class="sxs-lookup"><span data-stu-id="f5f13-112">Setting up Code First Migrations for Model Changes</span></span>

<span data-ttu-id="f5f13-113">Se você estiver usando o Visual Studio 2012, clique duas vezes no arquivo *Movies. MDF* de Gerenciador de soluções para abrir a ferramenta de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f5f13-113">If you are using Visual Studio 2012, double click the *Movies.mdf* file from Solution Explorer to open the database tool.</span></span> <span data-ttu-id="f5f13-114">Visual Studio Express para Web mostrará Gerenciador de Banco de Dados, o Visual Studio 2012 mostrará Gerenciador de Servidores.</span><span class="sxs-lookup"><span data-stu-id="f5f13-114">Visual Studio Express for Web will show Database Explorer, Visual Studio 2012 will show Server Explorer.</span></span> <span data-ttu-id="f5f13-115">Se você estiver usando o Visual Studio 2010, use Pesquisador de Objetos do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f5f13-115">If you are using Visual Studio 2010, use SQL Server Object Explorer.</span></span>

<span data-ttu-id="f5f13-116">Na ferramenta de banco de dados (Gerenciador de Banco de Dados, Gerenciador de Servidores ou Pesquisador de Objetos do SQL Server), clique com o botão direito do mouse em `MovieDBContext` e selecione **excluir** para remover o banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="f5f13-116">In the database tool (Database Explorer, Server Explorer or SQL Server Object Explorer), right click on `MovieDBContext` and select **Delete** to drop the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

<span data-ttu-id="f5f13-117">Navegue de volta para Gerenciador de Soluções.</span><span class="sxs-lookup"><span data-stu-id="f5f13-117">Navigate back to Solution Explorer.</span></span> <span data-ttu-id="f5f13-118">Clique com o botão direito do mouse no arquivo *Movies. MDF* e selecione **excluir** para remover o banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="f5f13-118">Right click on the *Movies.mdf* file and select **Delete** to remove the movies database.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

<span data-ttu-id="f5f13-119">Compile o aplicativo para verificar se não existem erros.</span><span class="sxs-lookup"><span data-stu-id="f5f13-119">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="f5f13-120">No menu **Ferramentas**, clique em **Gerenciador de Pacotes NuGet** e, em seguida, em **Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="f5f13-120">From the **Tools** menu, click **NuGet Package Manager** and then **Package Manager Console**.</span></span>

![Adicionar Man de pacote](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

<span data-ttu-id="f5f13-122">Na janela do **console do Gerenciador de pacotes** , na `PM>` prompt, digite "Enable-Migrations-ContextTypeName MvcMovie. Models. MovieDBContext".</span><span class="sxs-lookup"><span data-stu-id="f5f13-122">In the **Package Manager Console** window at the `PM>` prompt enter "Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext".</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

<span data-ttu-id="f5f13-123">O comando **Enable-Migrations** (mostrado acima) cria um arquivo *Configuration.cs* em uma nova pasta *migrações* .</span><span class="sxs-lookup"><span data-stu-id="f5f13-123">The **Enable-Migrations** command (shown above) creates a *Configuration.cs* file in a new *Migrations* folder.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

<span data-ttu-id="f5f13-124">O Visual Studio abre o arquivo *Configuration.cs* .</span><span class="sxs-lookup"><span data-stu-id="f5f13-124">Visual Studio opens the *Configuration.cs* file.</span></span> <span data-ttu-id="f5f13-125">Substitua o método `Seed` no arquivo *Configuration.cs* pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="f5f13-125">Replace the `Seed` method in the *Configuration.cs* file with the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

<span data-ttu-id="f5f13-126">Clique com o botão direito do mouse na linha vermelha ondulada em `Movie` e selecione **resolver** e, em seguida, **usando** **MvcMovie. Models;**</span><span class="sxs-lookup"><span data-stu-id="f5f13-126">Right click on the red squiggly line under `Movie` and select **Resolve** then **using** **MvcMovie.Models;**</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

<span data-ttu-id="f5f13-127">Isso adiciona a seguinte instrução Using:</span><span class="sxs-lookup"><span data-stu-id="f5f13-127">Doing so adds the following using statement:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="f5f13-128">Migrações do Code First chama o método `Seed` após cada migração (ou seja, chamando **Update-Database** no console do Gerenciador de pacotes) e esse método atualiza as linhas que já foram inseridas ou as insere se elas ainda não existirem.</span><span class="sxs-lookup"><span data-stu-id="f5f13-128">Code First Migrations calls the `Seed` method after every migration (that is, calling **update-database** in the Package Manager Console), and this method updates rows that have already been inserted, or inserts them if they don't exist yet.</span></span>

<span data-ttu-id="f5f13-129">**Pressione Ctrl-Shift-B para compilar o projeto.** (As etapas a seguir falharão se seu não for criado neste ponto.)</span><span class="sxs-lookup"><span data-stu-id="f5f13-129">**Press CTRL-SHIFT-B to build the project.**(The following steps will fail if your don't build at this point.)</span></span>

<span data-ttu-id="f5f13-130">A próxima etapa é criar uma classe de `DbMigration` para a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="f5f13-130">The next step is to create a `DbMigration` class for the initial migration.</span></span> <span data-ttu-id="f5f13-131">Essa migração para criar um novo banco de dados, é por isso que você excluiu o arquivo *Movie. MDF* em uma etapa anterior.</span><span class="sxs-lookup"><span data-stu-id="f5f13-131">This migration to creates a new database, that's why you deleted the *movie.mdf* file in a previous step.</span></span>

<span data-ttu-id="f5f13-132">Na janela do **console do Gerenciador de pacotes** , insira o comando "Add-Migration Initial" para criar a migração inicial.</span><span class="sxs-lookup"><span data-stu-id="f5f13-132">In the **Package Manager Console** window, enter the command "add-migration Initial" to create the initial migration.</span></span> <span data-ttu-id="f5f13-133">O nome "Initial" é arbitrário e é usado para nomear o arquivo de migração criado.</span><span class="sxs-lookup"><span data-stu-id="f5f13-133">The name "Initial" is arbitrary and is used to name the migration file created.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

<span data-ttu-id="f5f13-134">Migrações do Code First cria outro arquivo de classe na pasta *migrações* (com o nome *{DateStamp}\_Initial.cs* ) e essa classe contém um código que cria o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f5f13-134">Code First Migrations creates another class file in the *Migrations* folder (with the name *{DateStamp}\_Initial.cs* ), and this class contains code that creates the database schema.</span></span> <span data-ttu-id="f5f13-135">O nome de arquivo de migração é previamente corrigido com um carimbo de data/hora para ajudar com a ordenação.</span><span class="sxs-lookup"><span data-stu-id="f5f13-135">The migration filename is pre-fixed with a timestamp to help with ordering.</span></span> <span data-ttu-id="f5f13-136">Examine o arquivo *{dateStamp}\_Initial.cs* , que contém as instruções para criar a tabela de filmes para o BD de filme.</span><span class="sxs-lookup"><span data-stu-id="f5f13-136">Examine the *{DateStamp}\_Initial.cs* file, it contains the instructions to create the Movies table for the Movie DB.</span></span> <span data-ttu-id="f5f13-137">Quando você atualizar o banco de dados nas instruções abaixo, este arquivo *{dateStamp}\_Initial.cs* será executado e criará o esquema do BD.</span><span class="sxs-lookup"><span data-stu-id="f5f13-137">When you update the database in the instructions below, this *{DateStamp}\_Initial.cs* file will run and create the DB schema.</span></span> <span data-ttu-id="f5f13-138">Em seguida, o método **semente** será executado para popular o DB com dados de teste.</span><span class="sxs-lookup"><span data-stu-id="f5f13-138">Then the **Seed** method will run to populate the DB with test data.</span></span>

<span data-ttu-id="f5f13-139">No **console do Gerenciador de pacotes**, digite o comando "Update-Database" para criar o banco de dados e executar o método **semente** .</span><span class="sxs-lookup"><span data-stu-id="f5f13-139">In the **Package Manager Console**, enter the command "update-database" to create the database and run the **Seed** method.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

<span data-ttu-id="f5f13-140">Se você receber um erro que indica que uma tabela já existe e não pode ser criada, é provável que você tenha executado o aplicativo depois de ter excluído o banco de dados e antes de executar `update-database`.</span><span class="sxs-lookup"><span data-stu-id="f5f13-140">If you get an error that indicates a table already exists and can't be created, it is probably because you ran the application after you deleted the database and before you executed `update-database`.</span></span> <span data-ttu-id="f5f13-141">Nesse caso, exclua o arquivo *Movies. MDF* novamente e repita o comando `update-database`.</span><span class="sxs-lookup"><span data-stu-id="f5f13-141">In that case, delete the *Movies.mdf* file again and retry the `update-database` command.</span></span> <span data-ttu-id="f5f13-142">Se você ainda receber um erro, exclua a pasta de migrações e o conteúdo, em seguida, inicie com as instruções na parte superior desta página (isto é, exclua o arquivo *Movies. MDF* e continue para Enable-Migrations).</span><span class="sxs-lookup"><span data-stu-id="f5f13-142">If you still get an error, delete the migrations folder and contents then start with the instructions at the top of this page (that is delete the *Movies.mdf* file then proceed to Enable-Migrations).</span></span>

<span data-ttu-id="f5f13-143">Execute o aplicativo e navegue até a URL */Movies* .</span><span class="sxs-lookup"><span data-stu-id="f5f13-143">Run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="f5f13-144">Os dados de semente são exibidos.</span><span class="sxs-lookup"><span data-stu-id="f5f13-144">The seed data is displayed.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="f5f13-145">Adicionando uma propriedade de classificação ao modelo de filme</span><span class="sxs-lookup"><span data-stu-id="f5f13-145">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="f5f13-146">Comece adicionando uma nova propriedade `Rating` à classe `Movie` existente.</span><span class="sxs-lookup"><span data-stu-id="f5f13-146">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="f5f13-147">Abra o arquivo *Models\Movie.cs* e adicione a propriedade `Rating` como esta:</span><span class="sxs-lookup"><span data-stu-id="f5f13-147">Open the *Models\Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

<span data-ttu-id="f5f13-148">A classe completa `Movie` agora é semelhante ao seguinte código:</span><span class="sxs-lookup"><span data-stu-id="f5f13-148">The complete `Movie` class now looks like the following code:</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

<span data-ttu-id="f5f13-149">Crie o aplicativo usando o comando de menu **criar** &gt;**Compilar filme** ou pressionando Ctrl-Shift-B.</span><span class="sxs-lookup"><span data-stu-id="f5f13-149">Build the application using the **Build** &gt;**Build Movie** menu command or by pressing CTRL-SHIFT-B.</span></span>

<span data-ttu-id="f5f13-150">Agora que você atualizou a classe `Model`, também precisa atualizar os modelos de exibição *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* para exibir a nova propriedade `Rating` no modo de exibição de navegador.</span><span class="sxs-lookup"><span data-stu-id="f5f13-150">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.cshtml* and *\Views\Movies\Create.cshtml* view templates in order to display the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="f5f13-151">Abra o arquivo<em>\Views\Movies\Index.cshtml</em> e adicione um título de coluna `<th>Rating</th>` logo após a coluna <strong>Price</strong> .</span><span class="sxs-lookup"><span data-stu-id="f5f13-151">Open the<em>\Views\Movies\Index.cshtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="f5f13-152">Em seguida, adicione uma coluna `<td>` próximo ao final do modelo para renderizar o valor de `@item.Rating`.</span><span class="sxs-lookup"><span data-stu-id="f5f13-152">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="f5f13-153">Abaixo está a aparência do modelo de exibição <em>index. cshtml</em> atualizado:</span><span class="sxs-lookup"><span data-stu-id="f5f13-153">Below is what the updated <em>Index.cshtml</em> view template looks like:</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

<span data-ttu-id="f5f13-154">Em seguida, abra o arquivo *\Views\Movies\Create.cshtml* e adicione a marcação a seguir próximo ao final do formulário.</span><span class="sxs-lookup"><span data-stu-id="f5f13-154">Next, open the *\Views\Movies\Create.cshtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="f5f13-155">Isso renderiza uma caixa de texto para que você possa especificar uma classificação quando um novo filme é criado.</span><span class="sxs-lookup"><span data-stu-id="f5f13-155">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

<span data-ttu-id="f5f13-156">Agora você atualizou o código do aplicativo para dar suporte à nova propriedade `Rating`.</span><span class="sxs-lookup"><span data-stu-id="f5f13-156">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="f5f13-157">Agora, execute o aplicativo e navegue até a URL */Movies* .</span><span class="sxs-lookup"><span data-stu-id="f5f13-157">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="f5f13-158">No entanto, ao fazer isso, você verá um dos seguintes erros:</span><span class="sxs-lookup"><span data-stu-id="f5f13-158">When you do this, though, you'll see one of the following errors:</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

<span data-ttu-id="f5f13-159">Você está vendo esse erro porque a classe do modelo de `Movie` atualizado no aplicativo agora é diferente do esquema da tabela `Movie` do banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="f5f13-159">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="f5f13-160">(Não há nenhuma coluna `Rating` na tabela de banco de dados.)</span><span class="sxs-lookup"><span data-stu-id="f5f13-160">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="f5f13-161">Existem algumas abordagens para resolver o erro:</span><span class="sxs-lookup"><span data-stu-id="f5f13-161">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="f5f13-162">Faça com que o Entity Framework remova automaticamente e recrie o banco de dados com base no novo esquema de classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="f5f13-162">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="f5f13-163">Essa abordagem é muito conveniente ao fazer o desenvolvimento ativo em um banco de dados de teste; Ele permite que você evolua rapidamente o modelo e o esquema de banco de dados juntos.</span><span class="sxs-lookup"><span data-stu-id="f5f13-163">This approach is very convenient when doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="f5f13-164">No entanto, a desvantagem é que você perde os dados existentes no banco de dados – portanto, *não* convém usar essa abordagem em um banco de dados de produção!</span><span class="sxs-lookup"><span data-stu-id="f5f13-164">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span> <span data-ttu-id="f5f13-165">O uso de um inicializador para propagar automaticamente um banco de dados com dado de teste é geralmente uma maneira produtiva de desenvolver um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f5f13-165">Using an initializer to automatically seed a database with test data is often a productive way to develope an application.</span></span> <span data-ttu-id="f5f13-166">Para obter mais informações sobre inicializadores de banco de dados Entity Framework, consulte o [tutorial ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)de Tom Dykstra.</span><span class="sxs-lookup"><span data-stu-id="f5f13-166">For more information on Entity Framework database initializers, see Tom Dykstra's [ASP.NET MVC/Entity Framework tutorial](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>
2. <span data-ttu-id="f5f13-167">Modifique explicitamente o esquema do banco de dados existente para que ele corresponda às classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="f5f13-167">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="f5f13-168">A vantagem dessa abordagem é que você mantém os dados.</span><span class="sxs-lookup"><span data-stu-id="f5f13-168">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="f5f13-169">Faça essa alteração manualmente ou criando um script de alteração de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f5f13-169">You can make this change either manually or by creating a database change script.</span></span>
3. <span data-ttu-id="f5f13-170">Use as Migrações do Code First para atualizar o esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f5f13-170">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="f5f13-171">Para este tutorial, usaremos as Migrações do Code First.</span><span class="sxs-lookup"><span data-stu-id="f5f13-171">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="f5f13-172">Atualize o método semente para que ele forneça um valor para a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="f5f13-172">Update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="f5f13-173">Abra o arquivo Migrations\Configuration.cs e adicione um campo de classificação a cada objeto de filme.</span><span class="sxs-lookup"><span data-stu-id="f5f13-173">Open Migrations\Configuration.cs file and add a Rating field to each Movie object.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

<span data-ttu-id="f5f13-174">Compile a solução e, em seguida, abra a janela do **console do Gerenciador de pacotes** e digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="f5f13-174">Build the solution, and then open the **Package Manager Console** window and enter the following command:</span></span>

`add-migration AddRatingMig`

<span data-ttu-id="f5f13-175">O comando `add-migration` informa à estrutura de migração para examinar o modelo de filme atual com o esquema atual do BD do filme e criar o código necessário para migrar o BD para o novo modelo.</span><span class="sxs-lookup"><span data-stu-id="f5f13-175">The `add-migration` command tells the migration framework to examine the current movie model with the current movie DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="f5f13-176">O AddRatingMig é arbitrário e é usado para nomear o arquivo de migração.</span><span class="sxs-lookup"><span data-stu-id="f5f13-176">The AddRatingMig is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="f5f13-177">É útil usar um nome significativo para a etapa de migração.</span><span class="sxs-lookup"><span data-stu-id="f5f13-177">It's helpful to use a meaningful name for the migration step.</span></span>

<span data-ttu-id="f5f13-178">Quando esse comando for concluído, o Visual Studio abrirá o arquivo de classe que define a nova classe derivada `DbMigration` e, no método `Up`, você poderá ver o código que cria a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="f5f13-178">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` derived class, and in the `Up` method you can see the code that creates the new column.</span></span>

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

<span data-ttu-id="f5f13-179">Compile a solução e, em seguida, insira o comando "Update-Database" na janela do **console do Gerenciador de pacotes** .</span><span class="sxs-lookup"><span data-stu-id="f5f13-179">Build the solution, and then enter the "update-database" command in the **Package Manager Console** window.</span></span>

<span data-ttu-id="f5f13-180">A imagem a seguir mostra a saída na janela do **console do Gerenciador de pacotes** (o carimbo de data AddRatingMig de pendência será diferente.)</span><span class="sxs-lookup"><span data-stu-id="f5f13-180">The following image shows the output in the **Package Manager Console** window (The date stamp prepending AddRatingMig will be different.)</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

<span data-ttu-id="f5f13-181">Execute novamente o aplicativo e navegue até a URL do/Movies.</span><span class="sxs-lookup"><span data-stu-id="f5f13-181">Re-run the application and navigate to the /Movies URL.</span></span> <span data-ttu-id="f5f13-182">Você pode ver o novo campo de classificação.</span><span class="sxs-lookup"><span data-stu-id="f5f13-182">You can see the new Rating field.</span></span>

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

<span data-ttu-id="f5f13-183">Clique no link **criar novo** para adicionar um novo filme.</span><span class="sxs-lookup"><span data-stu-id="f5f13-183">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="f5f13-184">Observe que você pode adicionar uma classificação.</span><span class="sxs-lookup"><span data-stu-id="f5f13-184">Note that you can add a rating.</span></span>

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

<span data-ttu-id="f5f13-186">Clique em **Criar**.</span><span class="sxs-lookup"><span data-stu-id="f5f13-186">Click **Create**.</span></span> <span data-ttu-id="f5f13-187">O novo filme, incluindo a classificação, agora aparece na listagem de filmes:</span><span class="sxs-lookup"><span data-stu-id="f5f13-187">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

<span data-ttu-id="f5f13-189">Você também deve adicionar o campo `Rating` aos modelos de exibição Editar, detalhes e SearchIndex.</span><span class="sxs-lookup"><span data-stu-id="f5f13-189">You should also add the `Rating` field to the Edit, Details and SearchIndex view templates.</span></span>

<span data-ttu-id="f5f13-190">Você pode inserir o comando "Update-Database" na janela do **console do Gerenciador de pacotes** novamente e nenhuma alteração será feita, pois o esquema corresponde ao modelo.</span><span class="sxs-lookup"><span data-stu-id="f5f13-190">You could enter the "update-database" command in the **Package Manager Console** window again and no changes would be made, because the schema matches the model.</span></span>

<span data-ttu-id="f5f13-191">Nesta seção, você viu como é possível modificar objetos de modelo e manter o banco de dados em sincronia com as alterações.</span><span class="sxs-lookup"><span data-stu-id="f5f13-191">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="f5f13-192">Você também aprendeu uma maneira de preencher um banco de dados recém-criado com exemplos de dado, de modo que você possa experimentar cenários.</span><span class="sxs-lookup"><span data-stu-id="f5f13-192">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="f5f13-193">Em seguida, vamos dar uma olhada em como você pode adicionar uma lógica de validação mais rica às classes de modelo e permitir que algumas regras de negócio sejam impostas.</span><span class="sxs-lookup"><span data-stu-id="f5f13-193">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f5f13-194">[Anterior](examining-the-edit-methods-and-edit-view.md)
> [Próximo](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="f5f13-194">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
