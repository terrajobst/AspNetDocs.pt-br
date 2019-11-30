---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Implantação da Web do ASP.NET usando o Visual Studio: Implantando uma atualização de banco de dados | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros, por usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 805eb84c24764cf921291f89054435601dbac48e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636827"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="26e1b-103">Implantação da Web do ASP.NET usando o Visual Studio: Implantando uma atualização de banco de dados</span><span class="sxs-lookup"><span data-stu-id="26e1b-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>

<span data-ttu-id="26e1b-104">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="26e1b-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="26e1b-105">Baixar o projeto inicial</span><span class="sxs-lookup"><span data-stu-id="26e1b-105">Download Starter Project</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="26e1b-106">Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros usando o Visual Studio 2012 ou o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="26e1b-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="26e1b-107">Para obter informações sobre a série, consulte [o primeiro tutorial da série](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="26e1b-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="26e1b-108">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="26e1b-108">Overview</span></span>

<span data-ttu-id="26e1b-109">Neste tutorial, você faz uma alteração de banco de dados e alterações de código relacionadas, testa as alterações no Visual Studio e, em seguida, implanta a atualização nos ambientes de teste, de preparo e de produção.</span><span class="sxs-lookup"><span data-stu-id="26e1b-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="26e1b-110">O tutorial mostra primeiro como atualizar um banco de dados gerenciado pelo Migrações do Code First e, depois, mostra como atualizar um banco de dados usando o provedor dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="26e1b-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="26e1b-111">Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="26e1b-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="26e1b-112">Implantar uma atualização de banco de dados usando Migrações do Code First</span><span class="sxs-lookup"><span data-stu-id="26e1b-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="26e1b-113">Nesta seção, você adiciona uma coluna data de nascimento à classe base `Person` para as entidades `Student` e `Instructor`.</span><span class="sxs-lookup"><span data-stu-id="26e1b-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="26e1b-114">Em seguida, você atualiza a página que exibe dados do instrutor para que ele exiba a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="26e1b-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="26e1b-115">Por fim, você implanta as alterações para teste, preparo e produção.</span><span class="sxs-lookup"><span data-stu-id="26e1b-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="26e1b-116">Adicionar uma coluna a uma tabela no banco de dados do aplicativo</span><span class="sxs-lookup"><span data-stu-id="26e1b-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="26e1b-117">No projeto *ContosoUniversity. Dal* , abra *Person.cs* e adicione a seguinte propriedade no final da classe `Person` (deve haver duas chaves de fechamento após a ti):</span><span class="sxs-lookup"><span data-stu-id="26e1b-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="26e1b-118">Em seguida, atualize o método `Seed` para que ele forneça um valor para a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="26e1b-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="26e1b-119">Abra *Migrations\Configuration.cs* e substitua o bloco de código que começa `var instructors = new List<Instructor>` com o seguinte bloco de código que inclui informações de data de nascimento:</span><span class="sxs-lookup"><span data-stu-id="26e1b-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="26e1b-120">Compile a solução e, em seguida, abra a janela do **console do Gerenciador de pacotes** .</span><span class="sxs-lookup"><span data-stu-id="26e1b-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="26e1b-121">Verifique se ContosoUniversity. DAL ainda está selecionado como o **projeto padrão**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="26e1b-122">Na janela do **console do Gerenciador de pacotes** , selecione **ContosoUniversity. Dal** como o **projeto padrão**e, em seguida, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="26e1b-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="26e1b-123">Quando esse comando for concluído, o Visual Studio abrirá o arquivo de classe que define a nova classe `DbMigration` e, no método `Up`, você poderá ver o código que cria a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="26e1b-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMigration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="26e1b-124">O método `Up` cria a coluna quando você está implementando a alteração, e o método `Down` exclui a coluna ao reverter a alteração.</span><span class="sxs-lookup"><span data-stu-id="26e1b-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="26e1b-126">Compile a solução e, em seguida, digite o seguinte comando na janela do **console do Gerenciador de pacotes** (verifique se o projeto CONTOSOUNIVERSITY. Dal ainda está selecionado):</span><span class="sxs-lookup"><span data-stu-id="26e1b-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="26e1b-127">O Entity Framework executa o método `Up` e, em seguida, executa o método `Seed`.</span><span class="sxs-lookup"><span data-stu-id="26e1b-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="26e1b-128">Exibir a nova coluna na página de instrutores</span><span class="sxs-lookup"><span data-stu-id="26e1b-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="26e1b-129">No projeto ContosoUniversity, abra *instrutores. aspx* e adicione um novo campo de modelo para exibir a data de nascimento.</span><span class="sxs-lookup"><span data-stu-id="26e1b-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="26e1b-130">Adicione-o entre os itens para a data de contratação e a atribuição do Office:</span><span class="sxs-lookup"><span data-stu-id="26e1b-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="26e1b-131">(Se o recuo de código ficar fora de sincronia, você poderá pressionar CTRL-K e, em seguida, CTRL-D para reformatar o arquivo automaticamente.)</span><span class="sxs-lookup"><span data-stu-id="26e1b-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="26e1b-132">Execute o aplicativo e clique no link **instrutores** .</span><span class="sxs-lookup"><span data-stu-id="26e1b-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="26e1b-133">Quando a página for carregada, você verá que ela tem o novo campo data de nascimento.</span><span class="sxs-lookup"><span data-stu-id="26e1b-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Página de instrutores com DataDeNascimento](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="26e1b-135">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="26e1b-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="26e1b-136">Implantar a atualização do banco de dados</span><span class="sxs-lookup"><span data-stu-id="26e1b-136">Deploy the database update</span></span>

1. <span data-ttu-id="26e1b-137">Em **Gerenciador de soluções** selecione o projeto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="26e1b-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="26e1b-138">Na barra de ferramentas de **publicação do clique do Web One** , clique no perfil de publicação de **teste** e, em seguida, clique em **publicar Web**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="26e1b-139">(Se a barra de ferramentas estiver desabilitada, selecione o projeto ContosoUniversity em **Gerenciador de soluções**.)</span><span class="sxs-lookup"><span data-stu-id="26e1b-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="26e1b-140">O Visual Studio implanta o aplicativo atualizado e o navegador é aberto na home page.</span><span class="sxs-lookup"><span data-stu-id="26e1b-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="26e1b-141">Execute a página **instrutores** para verificar se a atualização foi implantada com êxito.</span><span class="sxs-lookup"><span data-stu-id="26e1b-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="26e1b-142">Quando o aplicativo tenta acessar o banco de dados para essa página, Code First atualiza o esquema de banco de dados e executa o método `Seed`.</span><span class="sxs-lookup"><span data-stu-id="26e1b-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="26e1b-143">Quando a página for exibida, você verá a coluna **data de nascimento** esperada com datas.</span><span class="sxs-lookup"><span data-stu-id="26e1b-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="26e1b-144">Na barra de ferramentas de **publicação de um clique da Web** , clique no perfil de publicação de **preparo** e, em seguida, clique em **publicar Web**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="26e1b-145">Execute a página **instrutores** em preparo para verificar se a atualização foi implantada com êxito.</span><span class="sxs-lookup"><span data-stu-id="26e1b-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="26e1b-146">Na barra de ferramentas de **publicação do clique do Web One** , clique no perfil de publicação de **produção** e clique em **publicar Web**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="26e1b-147">Execute a página **instrutores** em produção para verificar se a atualização foi implantada com êxito.</span><span class="sxs-lookup"><span data-stu-id="26e1b-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="26e1b-148">Para uma atualização de aplicativo de produção real que inclui uma alteração de banco de dados, normalmente, você também colocaria o aplicativo offline durante a implantação usando o *aplicativo\_offline. htm*, como vimos no tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="26e1b-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="26e1b-149">Implantar uma atualização de banco de dados usando o provedor dbDacFx</span><span class="sxs-lookup"><span data-stu-id="26e1b-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="26e1b-150">Nesta seção, você adiciona uma coluna de *comentários* à tabela de *usuário* no banco de dados de associação e cria uma página que permite exibir e editar comentários para cada usuário.</span><span class="sxs-lookup"><span data-stu-id="26e1b-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="26e1b-151">Em seguida, você implanta as alterações para teste, preparo e produção.</span><span class="sxs-lookup"><span data-stu-id="26e1b-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="26e1b-152">Adicionar uma coluna a uma tabela no banco de dados de associação</span><span class="sxs-lookup"><span data-stu-id="26e1b-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="26e1b-153">No Visual Studio, abra **pesquisador de objetos do SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="26e1b-154">Expanda **(LocalDB) \v11.0**, expanda **bancos de dados**, expanda **ASPNET-ContosoUniversity** (não **ASPNET-ContosoUniversity-prod**) e expanda **tabelas**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="26e1b-155">Se você não vir **(LocalDB) \v11.0** no nó **SQL Server** , clique com o botão direito do mouse no nó **SQL Server** e clique em **Adicionar SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="26e1b-156">Na caixa de diálogo **conectar ao servidor** , insira *(LocalDB) \v11.0* como o **nome do servidor**e, em seguida, clique em **conectar**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="26e1b-157">Se você não vir **ASPNET-ContosoUniversity**, execute o projeto e faça logon usando as credenciais de *administrador* (a senha é *devpwd*) e, em seguida, atualize a janela de **pesquisador de objetos do SQL Server** .</span><span class="sxs-lookup"><span data-stu-id="26e1b-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="26e1b-158">Clique com o botão direito do mouse na tabela **usuários** e clique em **Designer de exibição**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Designer de exibição do SSOX](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="26e1b-160">No designer, adicione uma coluna *comentários* e torne-a *nvarchar (128)* e Nullable e, em seguida, clique em **Atualizar**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Adicionando coluna de comentários](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="26e1b-162">Na caixa de **atualizações do banco de dados de visualização** , clique em **Atualizar banco de dados**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Visualizar atualizações de banco de dados](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="26e1b-164">Criar uma página para exibir e editar a nova coluna</span><span class="sxs-lookup"><span data-stu-id="26e1b-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="26e1b-165">Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta **conta** no projeto ContosoUniversity, clique em **Adicionar**e em **novo item**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="26e1b-166">Crie um novo **formulário da Web usando a página mestra** e nomeie-o como *UserInfo. aspx*.</span><span class="sxs-lookup"><span data-stu-id="26e1b-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="26e1b-167">Aceite o arquivo *site. Master* padrão como a página mestra.</span><span class="sxs-lookup"><span data-stu-id="26e1b-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="26e1b-168">Copie a marcação a seguir no elemento `MainContent` `Content` (o último dos três elementos `Content`):</span><span class="sxs-lookup"><span data-stu-id="26e1b-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="26e1b-169">Clique com o botão direito do mouse na página *UserInfo. aspx* e clique em **Exibir no navegador**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="26e1b-170">Faça logon com suas credenciais de usuário *administrador* (senha é *devpwd*) e adicione alguns comentários a um usuário para verificar se a página funciona corretamente.</span><span class="sxs-lookup"><span data-stu-id="26e1b-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Página UserInfo](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="26e1b-172">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="26e1b-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="26e1b-173">Implantar a atualização do banco de dados</span><span class="sxs-lookup"><span data-stu-id="26e1b-173">Deploy the database update</span></span>

<span data-ttu-id="26e1b-174">Para implantar usando o provedor dbDacFx, basta selecionar a opção **Atualizar banco de dados** no perfil de publicação.</span><span class="sxs-lookup"><span data-stu-id="26e1b-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="26e1b-175">No entanto, para a implantação inicial quando você usou essa opção, também configurou alguns scripts SQL adicionais a serem executados: eles ainda estão no perfil e você terá que impedi-los de executar novamente.</span><span class="sxs-lookup"><span data-stu-id="26e1b-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="26e1b-176">Abra o assistente **publicar na Web** clicando com o botão direito do mouse no projeto ContosoUniversity e clicando em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="26e1b-177">Selecione o perfil de **teste** .</span><span class="sxs-lookup"><span data-stu-id="26e1b-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="26e1b-178">Clique na guia **configurações** .</span><span class="sxs-lookup"><span data-stu-id="26e1b-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="26e1b-179">Em **DefaultConnection**, selecione **Atualizar banco de dados**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="26e1b-180">Desabilite os scripts adicionais que você configurou para execução na implantação inicial:</span><span class="sxs-lookup"><span data-stu-id="26e1b-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="26e1b-181">Clique em **Configurar atualizações de banco de dados**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="26e1b-182">Na caixa de diálogo **Configurar atualizações de banco de dados** , desmarque as caixas de seleção ao lado de *Grant. SQL* e *ASPNET-data-dev. SQL*.</span><span class="sxs-lookup"><span data-stu-id="26e1b-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="26e1b-183">Clique em **Fechar**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-183">Click **Close**.</span></span>
6. <span data-ttu-id="26e1b-184">Clique na guia **Visualizar** .</span><span class="sxs-lookup"><span data-stu-id="26e1b-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="26e1b-185">Em **bancos** de dados e à direita de **DefaultConnection**, clique no link **Preview Database** .</span><span class="sxs-lookup"><span data-stu-id="26e1b-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Visualização do banco de dados](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="26e1b-187">A janela de visualização mostra o script que será executado no banco de dados de destino para fazer com que o esquema de banco de dados corresponda ao esquema do banco de dados de origem.</span><span class="sxs-lookup"><span data-stu-id="26e1b-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="26e1b-188">O script inclui um comando ALTER TABLE que adiciona a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="26e1b-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="26e1b-189">Feche a caixa de diálogo **visualização do banco de dados** e clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="26e1b-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="26e1b-190">O Visual Studio implanta o aplicativo atualizado e o navegador é aberto na home page.</span><span class="sxs-lookup"><span data-stu-id="26e1b-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="26e1b-191">Execute a página UserInfo (adicione *Account/UserInfo. aspx* à URL Home Page) para verificar se a atualização foi implantada com êxito.</span><span class="sxs-lookup"><span data-stu-id="26e1b-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="26e1b-192">Você precisará fazer logon digitando *admin* e *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="26e1b-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="26e1b-193">Os dados nas tabelas não são implantados por padrão e você não configurou um script de implantação de dados para executar, portanto, não encontrará o comentário que você adicionou no desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="26e1b-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="26e1b-194">Você pode adicionar um novo comentário agora em preparo para verificar se a alteração foi implantada no banco de dados e a página funciona corretamente.</span><span class="sxs-lookup"><span data-stu-id="26e1b-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="26e1b-195">Siga o mesmo procedimento para implantar o preparo e a produção.</span><span class="sxs-lookup"><span data-stu-id="26e1b-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="26e1b-196">Não se esqueça de desabilitar os scripts extras.</span><span class="sxs-lookup"><span data-stu-id="26e1b-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="26e1b-197">A única diferença em comparação com o perfil de teste é que você desabilitará apenas um script nos perfis de preparo e produção, pois eles foram configurados para executar somente *ASPNET-prod-Data. SQL*.</span><span class="sxs-lookup"><span data-stu-id="26e1b-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="26e1b-198">As credenciais para preparo e produção são admin e prodpwd.</span><span class="sxs-lookup"><span data-stu-id="26e1b-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="26e1b-199">Para uma atualização de aplicativo de produção real que inclui uma alteração de banco de dados, normalmente, você também colocaria o aplicativo offline durante a implantação carregando o *aplicativo\_offline. htm* antes de publicá-lo e excluí-lo posteriormente, como vimos no [tutorial anterior](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="26e1b-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="26e1b-200">Resumo</span><span class="sxs-lookup"><span data-stu-id="26e1b-200">Summary</span></span>

<span data-ttu-id="26e1b-201">Agora você implantou uma atualização de aplicativo que incluiu uma alteração de banco de dados usando Migrações do Code First e o provedor dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="26e1b-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Página de instrutores com DataDeNascimento](deploying-a-database-update/_static/image8.png)

![Página UserInfo](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="26e1b-204">O próximo tutorial mostra como executar implantações usando a linha de comando.</span><span class="sxs-lookup"><span data-stu-id="26e1b-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="26e1b-205">[Anterior](deploying-a-code-update.md)
> [Próximo](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="26e1b-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
