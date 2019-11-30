---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Recuperando e exibindo dados com associação de modelo e formulários da Web | Microsoft Docs
author: Rick-Anderson
description: Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms. A associação de modelo torna a interação de dados mais direta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 81cca22cb4752d071d2a68986ae9ac2bed737594
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633181"
---
# <a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="269f7-104">Recuperando e exibindo dados com associação de modelo e formulários da Web</span><span class="sxs-lookup"><span data-stu-id="269f7-104">Retrieving and displaying data with model binding and web forms</span></span>

> <span data-ttu-id="269f7-105">Esta série de tutoriais demonstra os aspectos básicos do uso de associação de modelo com um projeto ASP.NET Web Forms.</span><span class="sxs-lookup"><span data-stu-id="269f7-105">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="269f7-106">A associação de modelo torna a interação de dados mais direta do que lidar com objetos de fonte de dados (como ObjectDataSource ou SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="269f7-106">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="269f7-107">Esta série começa com material introdutório e passa para conceitos mais avançados em Tutoriais posteriores.</span><span class="sxs-lookup"><span data-stu-id="269f7-107">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="269f7-108">O padrão de associação de modelo funciona com qualquer tecnologia de acesso a dados.</span><span class="sxs-lookup"><span data-stu-id="269f7-108">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="269f7-109">Neste tutorial, você usará Entity Framework, mas poderá usar a tecnologia de acesso a dados mais familiar para você.</span><span class="sxs-lookup"><span data-stu-id="269f7-109">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="269f7-110">De um controle de servidor associado a dados, como um controle GridView, ListView, DetailsView ou FormView, você especifica os nomes dos métodos a serem usados para selecionar, atualizar, excluir e criar dados.</span><span class="sxs-lookup"><span data-stu-id="269f7-110">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="269f7-111">Neste tutorial, você especificará um valor para SelectMethod.</span><span class="sxs-lookup"><span data-stu-id="269f7-111">In this tutorial, you will specify a value for the SelectMethod.</span></span> 
> 
> <span data-ttu-id="269f7-112">Dentro desse método, você fornece a lógica para recuperar os dados.</span><span class="sxs-lookup"><span data-stu-id="269f7-112">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="269f7-113">No próximo tutorial, você definirá valores para UpdateMethod, DeleteMethod e InsertMethod.</span><span class="sxs-lookup"><span data-stu-id="269f7-113">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
>
> <span data-ttu-id="269f7-114">Você pode [baixar](https://go.microsoft.com/fwlink/?LinkId=286116) o projeto completo no C# ou Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="269f7-114">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or Visual Basic.</span></span> <span data-ttu-id="269f7-115">O código que pode ser baixado funciona com o Visual Studio 2012 e posterior.</span><span class="sxs-lookup"><span data-stu-id="269f7-115">The downloadable code works with Visual Studio 2012 and later.</span></span> <span data-ttu-id="269f7-116">Ele usa o modelo do Visual Studio 2012, que é ligeiramente diferente do modelo do Visual Studio 2017 mostrado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="269f7-116">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2017 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="269f7-117">No tutorial, você executa o aplicativo no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="269f7-117">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="269f7-118">Você também pode implantar o aplicativo em um provedor de hospedagem e disponibilizá-lo pela Internet.</span><span class="sxs-lookup"><span data-stu-id="269f7-118">You can also deploy the application to a hosting provider and make it available over the internet.</span></span> <span data-ttu-id="269f7-119">A Microsoft oferece hospedagem na Web gratuita para até 10 sites em um</span><span class="sxs-lookup"><span data-stu-id="269f7-119">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
> <span data-ttu-id="269f7-120">[conta de avaliação gratuita do Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="269f7-120">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="269f7-121">Para obter informações sobre como implantar um projeto Web do Visual Studio em aplicativos Web do Azure App Service, consulte a [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) Series.</span><span class="sxs-lookup"><span data-stu-id="269f7-121">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="269f7-122">Esse tutorial também mostra como usar Migrações do Entity Framework Code First para implantar o banco de dados do SQL Server no banco de dados SQL do Azure.</span><span class="sxs-lookup"><span data-stu-id="269f7-122">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="269f7-123">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="269f7-123">Software versions used in the tutorial</span></span>
> 
> - <span data-ttu-id="269f7-124">Microsoft Visual Studio 2017 ou Microsoft Visual Studio Community 2017</span><span class="sxs-lookup"><span data-stu-id="269f7-124">Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017</span></span>
>   
> <span data-ttu-id="269f7-125">Este tutorial também funciona com o Visual Studio 2012 e Visual Studio 2013, mas há algumas diferenças na interface do usuário e no modelo de projeto.</span><span class="sxs-lookup"><span data-stu-id="269f7-125">This tutorial also works with Visual Studio 2012 and Visual Studio 2013, but there are some differences in the user interface and project template.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="269f7-126">O que você criará</span><span class="sxs-lookup"><span data-stu-id="269f7-126">What you'll build</span></span>

<span data-ttu-id="269f7-127">Neste tutorial, você vai:</span><span class="sxs-lookup"><span data-stu-id="269f7-127">In this tutorial, you'll:</span></span>

* <span data-ttu-id="269f7-128">Crie objetos de dados que reflitam uma universidade com alunos registrados em cursos</span><span class="sxs-lookup"><span data-stu-id="269f7-128">Build data objects that reflect a university with students enrolled in courses</span></span>
* <span data-ttu-id="269f7-129">Criar tabelas de banco de dados a partir dos objetos</span><span class="sxs-lookup"><span data-stu-id="269f7-129">Build database tables from the objects</span></span>
* <span data-ttu-id="269f7-130">Popule o banco de dados com dado de teste</span><span class="sxs-lookup"><span data-stu-id="269f7-130">Populate the database with test data</span></span>
* <span data-ttu-id="269f7-131">Exibir dados em um formulário da Web</span><span class="sxs-lookup"><span data-stu-id="269f7-131">Display data in a web form</span></span>

## <a name="create-the-project"></a><span data-ttu-id="269f7-132">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="269f7-132">Create the project</span></span>

1. <span data-ttu-id="269f7-133">No Visual Studio 2017, crie um projeto de **aplicativo Web ASP.net (.NET Framework)** chamado **ContosoUniversityModelBinding**.</span><span class="sxs-lookup"><span data-stu-id="269f7-133">In Visual Studio 2017, create a **ASP.NET Web Application (.NET Framework)** project called **ContosoUniversityModelBinding**.</span></span>

   ![Criar projeto](retrieving-data/_static/image19.png)

2. <span data-ttu-id="269f7-135">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="269f7-135">Select **OK**.</span></span> <span data-ttu-id="269f7-136">A caixa de diálogo para selecionar um modelo é exibida.</span><span class="sxs-lookup"><span data-stu-id="269f7-136">The dialog box to select a template appears.</span></span>

   ![selecionar formulários da Web](retrieving-data/_static/image3.png)

3. <span data-ttu-id="269f7-138">Selecione o modelo de **Web Forms** .</span><span class="sxs-lookup"><span data-stu-id="269f7-138">Select the **Web Forms** template.</span></span> 

4. <span data-ttu-id="269f7-139">Se necessário, altere a autenticação para **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="269f7-139">If necessary, change the authentication to **Individual User Accounts**.</span></span> 

5. <span data-ttu-id="269f7-140">Selecione **OK** para criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="269f7-140">Select **OK** to create the project.</span></span>

## <a name="modify-site-appearance"></a><span data-ttu-id="269f7-141">Modificar a aparência do site</span><span class="sxs-lookup"><span data-stu-id="269f7-141">Modify site appearance</span></span>

   <span data-ttu-id="269f7-142">Faça algumas alterações para personalizar a aparência do site.</span><span class="sxs-lookup"><span data-stu-id="269f7-142">Make a few changes to customize site appearance.</span></span> 
   
   1. <span data-ttu-id="269f7-143">Abra o arquivo site. master.</span><span class="sxs-lookup"><span data-stu-id="269f7-143">Open the Site.Master file.</span></span>
   
   2. <span data-ttu-id="269f7-144">Altere o título para exibir a **Contoso University** e não **meu aplicativo ASP.net**.</span><span class="sxs-lookup"><span data-stu-id="269f7-144">Change the title to display **Contoso University** and not **My ASP.NET Application**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. <span data-ttu-id="269f7-145">Altere o texto do cabeçalho do **nome do aplicativo** para **Contoso University**.</span><span class="sxs-lookup"><span data-stu-id="269f7-145">Change the header text from **Application name** to **Contoso University**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. <span data-ttu-id="269f7-146">Altere os links de cabeçalho de navegação para os sites apropriados.</span><span class="sxs-lookup"><span data-stu-id="269f7-146">Change the navigation header links to site appropriate ones.</span></span> 
   
      <span data-ttu-id="269f7-147">Remova os links para **sobre** e **contate** e, em vez disso, vincule a uma página de **alunos** que você criará.</span><span class="sxs-lookup"><span data-stu-id="269f7-147">Remove the links for **About** and **Contact** and, instead, link to a **Students** page, which you will create.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. <span data-ttu-id="269f7-148">Salve site. master.</span><span class="sxs-lookup"><span data-stu-id="269f7-148">Save Site.Master.</span></span>

## <a name="add-a-web-form-to-display-student-data"></a><span data-ttu-id="269f7-149">Adicionar um formulário da Web para exibir dados do aluno</span><span class="sxs-lookup"><span data-stu-id="269f7-149">Add a web form to display student data</span></span>

   1. <span data-ttu-id="269f7-150">Em **Gerenciador de soluções**, clique com o botão direito do mouse em seu projeto, selecione **Adicionar** e **novo item**.</span><span class="sxs-lookup"><span data-stu-id="269f7-150">In **Solution Explorer**, right-click your project, select **Add** and then **New Item**.</span></span> 
   
   2. <span data-ttu-id="269f7-151">Na caixa de diálogo **Adicionar novo item** , selecione o modelo **formulário da Web com página mestra** e nomeie-o como **Student. aspx**.</span><span class="sxs-lookup"><span data-stu-id="269f7-151">In the **Add New Item** dialog box, select the **Web Form with Master Page** template and name it **Students.aspx**.</span></span>

      ![criar página](retrieving-data/_static/image5.png)

   3. <span data-ttu-id="269f7-153">Selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="269f7-153">Select **Add**.</span></span>
   
   4. <span data-ttu-id="269f7-154">Para a página mestra do formulário da Web, selecione **site. Master**.</span><span class="sxs-lookup"><span data-stu-id="269f7-154">For the web form's master page, select **Site.Master**.</span></span>
   
   5. <span data-ttu-id="269f7-155">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="269f7-155">Select **OK**.</span></span>

## <a name="add-the-data-model"></a><span data-ttu-id="269f7-156">Adicionar o modelo de dados</span><span class="sxs-lookup"><span data-stu-id="269f7-156">Add the data model</span></span>

<span data-ttu-id="269f7-157">Na pasta **modelos** , adicione uma classe chamada **UniversityModels.cs**.</span><span class="sxs-lookup"><span data-stu-id="269f7-157">In the **Models** folder, add a class named **UniversityModels.cs**.</span></span>

   1. <span data-ttu-id="269f7-158">Clique com o botão direito do mouse em **modelos**, selecione **Adicionar**e **novo item**.</span><span class="sxs-lookup"><span data-stu-id="269f7-158">Right-click **Models**, select **Add**, and then **New Item**.</span></span> <span data-ttu-id="269f7-159">A caixa de diálogo **Adicionar Novo Item** é exibida.</span><span class="sxs-lookup"><span data-stu-id="269f7-159">The **Add New Item** dialog box appears.</span></span>

   2. <span data-ttu-id="269f7-160">No menu de navegação à esquerda, selecione **código**e **classe**.</span><span class="sxs-lookup"><span data-stu-id="269f7-160">From the left navigation menu, select **Code**, then **Class**.</span></span>

      ![criar classe de modelo](retrieving-data/_static/image20.png)

   3. <span data-ttu-id="269f7-162">Nomeie a classe **UniversityModels.cs** e selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="269f7-162">Name the class **UniversityModels.cs** and select **Add**.</span></span>

      <span data-ttu-id="269f7-163">Nesse arquivo, defina as classes `SchoolContext`, `Student`, `Enrollment`e `Course` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="269f7-163">In this file, define the `SchoolContext`, `Student`, `Enrollment`, and `Course` classes as follows:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      <span data-ttu-id="269f7-164">A classe de `SchoolContext` deriva de `DbContext`, que gerencia a conexão de banco de dados e as alterações nos mesmos.</span><span class="sxs-lookup"><span data-stu-id="269f7-164">The `SchoolContext` class derives from `DbContext`, which manages the database connection and changes in the data.</span></span>

      <span data-ttu-id="269f7-165">Na classe `Student`, observe os atributos aplicados às propriedades `FirstName`, `LastName`e `Year`.</span><span class="sxs-lookup"><span data-stu-id="269f7-165">In the `Student` class, notice the attributes applied to the `FirstName`, `LastName`, and `Year` properties.</span></span> <span data-ttu-id="269f7-166">Este tutorial usa esses atributos para validação de dados.</span><span class="sxs-lookup"><span data-stu-id="269f7-166">This tutorial uses these attributes for data validation.</span></span> <span data-ttu-id="269f7-167">Para simplificar o código, somente essas propriedades são marcadas com atributos de validação de dados.</span><span class="sxs-lookup"><span data-stu-id="269f7-167">To simplify the code, only these properties are marked with data-validation attributes.</span></span> <span data-ttu-id="269f7-168">Em um projeto real, você aplicaria atributos de validação a todas as propriedades que precisam de validação.</span><span class="sxs-lookup"><span data-stu-id="269f7-168">In a real project, you would apply validation attributes to all properties needing validation.</span></span>

   4. <span data-ttu-id="269f7-169">Salve UniversityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="269f7-169">Save UniversityModels.cs.</span></span>

## <a name="set-up-the-database-based-on-classes"></a><span data-ttu-id="269f7-170">Configurar o banco de dados com base em classes</span><span class="sxs-lookup"><span data-stu-id="269f7-170">Set up the database based on classes</span></span>

<span data-ttu-id="269f7-171">Este tutorial usa [migrações do Code First](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) para criar objetos e tabelas de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="269f7-171">This tutorial uses [Code First Migrations](https://docs.microsoft.com/ef/ef6/modeling/code-first/migrations/) to create objects and database tables.</span></span> <span data-ttu-id="269f7-172">Essas tabelas armazenam informações sobre os alunos e seus cursos.</span><span class="sxs-lookup"><span data-stu-id="269f7-172">These tables store information about the students and their courses.</span></span>

   1. <span data-ttu-id="269f7-173">Selecione **ferramentas** > **Gerenciador de pacotes NuGet** > **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="269f7-173">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

   2. <span data-ttu-id="269f7-174">No **console do Gerenciador de pacotes**, execute este comando:</span><span class="sxs-lookup"><span data-stu-id="269f7-174">In **Package Manager Console**, run this command:</span></span>  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      <span data-ttu-id="269f7-175">Se o comando for concluído com êxito, será exibida uma mensagem informando que as migrações foram habilitadas.</span><span class="sxs-lookup"><span data-stu-id="269f7-175">If the command completes successfully, a message stating migrations have been enabled appears.</span></span>

      ![Habilitar migrações](retrieving-data/_static/image8.png)

      <span data-ttu-id="269f7-177">Observe que um arquivo chamado *Configuration.cs* foi criado.</span><span class="sxs-lookup"><span data-stu-id="269f7-177">Notice that a file named *Configuration.cs* has been created.</span></span> <span data-ttu-id="269f7-178">A classe `Configuration` tem um método `Seed`, que pode preencher previamente as tabelas de banco de dados com dado de teste.</span><span class="sxs-lookup"><span data-stu-id="269f7-178">The `Configuration` class has a `Seed` method, which can pre-populate the database tables with test data.</span></span>

## <a name="pre-populate-the-database"></a><span data-ttu-id="269f7-179">Preencher previamente o banco de dados</span><span class="sxs-lookup"><span data-stu-id="269f7-179">Pre-populate the database</span></span>

   1. <span data-ttu-id="269f7-180">Abra Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="269f7-180">Open Configuration.cs.</span></span>
   
   2. <span data-ttu-id="269f7-181">Adicione o seguinte código ao método de `Seed` .</span><span class="sxs-lookup"><span data-stu-id="269f7-181">Add the following code to the `Seed` method.</span></span> <span data-ttu-id="269f7-182">Além disso, adicione uma instrução `using` para o namespace `ContosoUniversityModelBinding. Models`.</span><span class="sxs-lookup"><span data-stu-id="269f7-182">Also, add a `using` statement for the `ContosoUniversityModelBinding. Models` namespace.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. <span data-ttu-id="269f7-183">Salve Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="269f7-183">Save Configuration.cs.</span></span>

   4. <span data-ttu-id="269f7-184">No console do Gerenciador de pacotes, execute o comando **Adicionar-migração inicial**.</span><span class="sxs-lookup"><span data-stu-id="269f7-184">In the Package Manager Console, run the command **add-migration initial**.</span></span>

   5. <span data-ttu-id="269f7-185">Execute o comando **Update-Database**.</span><span class="sxs-lookup"><span data-stu-id="269f7-185">Run the command **update-database**.</span></span>

      <span data-ttu-id="269f7-186">Se você receber uma exceção ao executar esse comando, os valores `StudentID` e `CourseID` poderão ser diferentes dos valores do método `Seed`.</span><span class="sxs-lookup"><span data-stu-id="269f7-186">If you receive an exception when running this command, the `StudentID` and `CourseID` values might be different from the `Seed` method values.</span></span> <span data-ttu-id="269f7-187">Abra essas tabelas de banco de dados e encontre os valores existentes para `StudentID` e `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="269f7-187">Open those database tables and find existing values for `StudentID` and `CourseID`.</span></span> <span data-ttu-id="269f7-188">Adicione esses valores ao código para propagar a tabela `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="269f7-188">Add those values to the code for seeding the `Enrollments` table.</span></span>

## <a name="add-a-gridview-control"></a><span data-ttu-id="269f7-189">Adicionar um controle GridView</span><span class="sxs-lookup"><span data-stu-id="269f7-189">Add a GridView control</span></span>

<span data-ttu-id="269f7-190">Com os dados populados do banco, agora você está pronto para recuperar esses dados e exibi-los.</span><span class="sxs-lookup"><span data-stu-id="269f7-190">With populated database data, you're now ready to retrieve that data and display it.</span></span> 

1. <span data-ttu-id="269f7-191">Abra o students. aspx.</span><span class="sxs-lookup"><span data-stu-id="269f7-191">Open Students.aspx.</span></span>

2. <span data-ttu-id="269f7-192">Localize o espaço reservado `MainContent`.</span><span class="sxs-lookup"><span data-stu-id="269f7-192">Locate the `MainContent` placeholder.</span></span> <span data-ttu-id="269f7-193">Dentro desse espaço reservado, adicione um controle **GridView** que inclua esse código.</span><span class="sxs-lookup"><span data-stu-id="269f7-193">Within that placeholder, add a **GridView** control that includes this code.</span></span>

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   <span data-ttu-id="269f7-194">Itens a serem observados:</span><span class="sxs-lookup"><span data-stu-id="269f7-194">Things to note:</span></span>
   * <span data-ttu-id="269f7-195">Observe o valor definido para a propriedade `SelectMethod` no elemento GridView.</span><span class="sxs-lookup"><span data-stu-id="269f7-195">Notice the value set for the `SelectMethod` property in the GridView element.</span></span> <span data-ttu-id="269f7-196">Esse valor especifica o método usado para recuperar dados GridView, que você cria na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="269f7-196">This value specifies the method used to retrieve GridView data, which you create in the next step.</span></span> 
   
   * <span data-ttu-id="269f7-197">A propriedade `ItemType` é definida como a classe `Student` criada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="269f7-197">The `ItemType` property is set to the `Student` class created earlier.</span></span> <span data-ttu-id="269f7-198">Essa configuração permite que você referencie Propriedades de classe na marcação.</span><span class="sxs-lookup"><span data-stu-id="269f7-198">This setting allows you to reference class properties in the markup.</span></span> <span data-ttu-id="269f7-199">Por exemplo, a classe `Student` tem uma coleção chamada `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="269f7-199">For example, the `Student` class has a collection named `Enrollments`.</span></span> <span data-ttu-id="269f7-200">Você pode usar `Item.Enrollments` para recuperar essa coleção e, em seguida, usar a [sintaxe LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) para recuperar a soma dos créditos registrados de cada aluno.</span><span class="sxs-lookup"><span data-stu-id="269f7-200">You can use `Item.Enrollments` to retrieve that collection and then use [LINQ syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) to retrieve each student's enrolled credits sum.</span></span>
   
3. <span data-ttu-id="269f7-201">Salve students. aspx.</span><span class="sxs-lookup"><span data-stu-id="269f7-201">Save Students.aspx.</span></span>

## <a name="add-code-to-retrieve-data"></a><span data-ttu-id="269f7-202">Adicionar código para recuperar dados</span><span class="sxs-lookup"><span data-stu-id="269f7-202">Add code to retrieve data</span></span>

   <span data-ttu-id="269f7-203">No arquivo code-behind dos alunos. aspx, adicione o método especificado para o valor `SelectMethod`.</span><span class="sxs-lookup"><span data-stu-id="269f7-203">In the Students.aspx code-behind file, add the method specified for the `SelectMethod` value.</span></span> 
   
   1. <span data-ttu-id="269f7-204">Abra Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="269f7-204">Open Students.aspx.cs.</span></span>
   
   2. <span data-ttu-id="269f7-205">Adicione `using` instruções para os namespaces de `ContosoUniversityModelBinding. Models` e `System.Data.Entity`.</span><span class="sxs-lookup"><span data-stu-id="269f7-205">Add `using` statements for the `ContosoUniversityModelBinding. Models` and `System.Data.Entity` namespaces.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. <span data-ttu-id="269f7-206">Adicione o método especificado para `SelectMethod`:</span><span class="sxs-lookup"><span data-stu-id="269f7-206">Add the method you specified for `SelectMethod`:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      <span data-ttu-id="269f7-207">A cláusula `Include` melhora o desempenho da consulta, mas não é necessária.</span><span class="sxs-lookup"><span data-stu-id="269f7-207">The `Include` clause improves query performance but isn't required.</span></span> <span data-ttu-id="269f7-208">Sem a cláusula `Include`, os dados são recuperados usando o [*carregamento lento*](https://en.wikipedia.org/wiki/Lazy_loading), o que envolve o envio de uma consulta separada para o banco de dados cada vez que forem recuperados.</span><span class="sxs-lookup"><span data-stu-id="269f7-208">Without the `Include` clause, the data is retrieved using [*lazy loading*](https://en.wikipedia.org/wiki/Lazy_loading), which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="269f7-209">Com a cláusula `Include`, os dados são recuperados usando o *carregamento adiantado*, o que significa que uma única consulta de banco de dados recupera todos os dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="269f7-209">With the `Include` clause, data is retrieved using *eager loading*, which means a single database query retrieves all related data.</span></span> <span data-ttu-id="269f7-210">Se os dados relacionados não forem usados, o carregamento adiantado é menos eficiente, pois mais dados são recuperados.</span><span class="sxs-lookup"><span data-stu-id="269f7-210">If related data isn't used, eager loading is less efficient because more data is retrieved.</span></span> <span data-ttu-id="269f7-211">No entanto, nesse caso, o carregamento adiantado oferece o melhor desempenho porque os dados relacionados são exibidos para cada registro.</span><span class="sxs-lookup"><span data-stu-id="269f7-211">However, in this case, eager loading gives you the best performance because the related data is displayed for each record.</span></span>

      <span data-ttu-id="269f7-212">Para obter mais informações sobre considerações de desempenho ao carregar dados relacionados, consulte a seção **carregamento lento, adiantado e explícito de dados relacionados** no artigo [lendo dados relacionados com o Entity Framework em um aplicativo MVC ASP.net](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) .</span><span class="sxs-lookup"><span data-stu-id="269f7-212">For more information about performance considerations when loading related data, see the **Lazy, Eager, and Explicit Loading of Related Data** section in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) article.</span></span>

      <span data-ttu-id="269f7-213">Por padrão, os dados são classificados pelos valores da propriedade marcada como a chave.</span><span class="sxs-lookup"><span data-stu-id="269f7-213">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="269f7-214">Você pode adicionar uma cláusula `OrderBy` para especificar um valor de classificação diferente.</span><span class="sxs-lookup"><span data-stu-id="269f7-214">You can add an `OrderBy` clause to specify a different sort value.</span></span> <span data-ttu-id="269f7-215">Neste exemplo, a propriedade de `StudentID` padrão é usada para classificação.</span><span class="sxs-lookup"><span data-stu-id="269f7-215">In this example, the default `StudentID` property is used for sorting.</span></span> <span data-ttu-id="269f7-216">No artigo [classificando, paginando e Filtrando dados](sorting-paging-and-filtering-data.md) , o usuário está habilitado para selecionar uma coluna para classificação.</span><span class="sxs-lookup"><span data-stu-id="269f7-216">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) article, the user is enabled to select a column for sorting.</span></span>
 
   4. <span data-ttu-id="269f7-217">Salve Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="269f7-217">Save Students.aspx.cs.</span></span>

## <a name="run-your-application"></a><span data-ttu-id="269f7-218">Executar seu aplicativo</span><span class="sxs-lookup"><span data-stu-id="269f7-218">Run your application</span></span> 

<span data-ttu-id="269f7-219">Execute seu aplicativo Web (**F5**) e navegue até a página **alunos** , que exibe o seguinte:</span><span class="sxs-lookup"><span data-stu-id="269f7-219">Run your web application (**F5**) and navigate to the **Students** page, which displays the following:</span></span>

   ![Mostrar dados](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="269f7-221">Geração automática de métodos de associação de modelo</span><span class="sxs-lookup"><span data-stu-id="269f7-221">Automatic generation of model binding methods</span></span>

<span data-ttu-id="269f7-222">Ao trabalhar nesta série de tutoriais, você pode simplesmente copiar o código do tutorial para o seu projeto.</span><span class="sxs-lookup"><span data-stu-id="269f7-222">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="269f7-223">No entanto, uma desvantagem dessa abordagem é que você pode não estar ciente do recurso fornecido pelo Visual Studio para gerar automaticamente o código para métodos de associação de modelo.</span><span class="sxs-lookup"><span data-stu-id="269f7-223">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="269f7-224">Ao trabalhar em seus próprios projetos, a geração automática de código pode poupar tempo e ajudá-lo a ter uma noção de como implementar uma operação.</span><span class="sxs-lookup"><span data-stu-id="269f7-224">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="269f7-225">Esta seção descreve o recurso de geração de código automático.</span><span class="sxs-lookup"><span data-stu-id="269f7-225">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="269f7-226">Esta seção é informativa apenas e não contém nenhum código que você precise implementar em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="269f7-226">This section is only informational and does not contain any code you need to implement in your project.</span></span> 

<span data-ttu-id="269f7-227">Ao definir um valor para as propriedades `SelectMethod`, `UpdateMethod`, `InsertMethod`ou `DeleteMethod` no código de marcação, você pode selecionar a opção **criar novo método** .</span><span class="sxs-lookup"><span data-stu-id="269f7-227">When setting a value for the `SelectMethod`, `UpdateMethod`, `InsertMethod`, or `DeleteMethod` properties in the markup code, you can select the **Create New Method** option.</span></span>

![criar um método](retrieving-data/_static/image18.png)

<span data-ttu-id="269f7-229">O Visual Studio não apenas cria um método no code-behind com a assinatura apropriada, mas também gera código de implementação para executar a operação.</span><span class="sxs-lookup"><span data-stu-id="269f7-229">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to perform the operation.</span></span> <span data-ttu-id="269f7-230">Se você definir primeiro a propriedade `ItemType` antes de usar o recurso de geração de código automático, o código gerado usará esse tipo para as operações.</span><span class="sxs-lookup"><span data-stu-id="269f7-230">If you first set the `ItemType` property before using the automatic code generation feature, the generated code uses that type for the operations.</span></span> <span data-ttu-id="269f7-231">Por exemplo, ao definir a propriedade `UpdateMethod`, o código a seguir é gerado automaticamente:</span><span class="sxs-lookup"><span data-stu-id="269f7-231">For example, when setting the `UpdateMethod` property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="269f7-232">Novamente, esse código não precisa ser adicionado ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="269f7-232">Again, this code doesn't need to be added to your project.</span></span> <span data-ttu-id="269f7-233">No próximo tutorial, você implementará métodos para atualizar, excluir e adicionar novos dados.</span><span class="sxs-lookup"><span data-stu-id="269f7-233">In the next tutorial, you'll implement methods for updating, deleting, and adding new data.</span></span>

## <a name="summary"></a><span data-ttu-id="269f7-234">Resumo</span><span class="sxs-lookup"><span data-stu-id="269f7-234">Summary</span></span>

<span data-ttu-id="269f7-235">Neste tutorial, você criou classes de modelo de dados e gerou um banco de dado dessas classes.</span><span class="sxs-lookup"><span data-stu-id="269f7-235">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="269f7-236">Você preencheu as tabelas de banco de dados com test data.</span><span class="sxs-lookup"><span data-stu-id="269f7-236">You filled the database tables with test data.</span></span> <span data-ttu-id="269f7-237">Você usou a associação de modelo para recuperar dados do banco e, em seguida, exibiu os dados em um GridView.</span><span class="sxs-lookup"><span data-stu-id="269f7-237">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="269f7-238">No próximo [tutorial](updating-deleting-and-creating-data.md) desta série, você habilitará a atualização, a exclusão e a criação de dados.</span><span class="sxs-lookup"><span data-stu-id="269f7-238">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you'll enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="269f7-239">Avançar</span><span class="sxs-lookup"><span data-stu-id="269f7-239">Next</span></span>](updating-deleting-and-creating-data.md)
