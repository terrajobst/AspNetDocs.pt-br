---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Implantando um aplicativo Web ASP.NET com o SQL Server Compact usando o Visual Studio ou o Visual Web Developer: Implantando uma atualização de banco de dados SQL Server-11 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0894c0ac24737e66b6960ef3d48aa17f78c6aa1d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528122"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="f48f4-103">Implantando um aplicativo Web ASP.NET com o SQL Server Compact usando o Visual Studio ou o Visual Web Developer: Implantando uma atualização de banco de dados SQL Server-11 de 12</span><span class="sxs-lookup"><span data-stu-id="f48f4-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>

<span data-ttu-id="f48f4-104">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f48f4-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="f48f4-105">Baixar o projeto inicial</span><span class="sxs-lookup"><span data-stu-id="f48f4-105">Download Starter Project</span></span>](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="f48f4-106">Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Studio 2012 RC ou o Visual Studio Express 2012 RC para Web.</span><span class="sxs-lookup"><span data-stu-id="f48f4-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="f48f4-107">Você também pode usar o Visual Studio 2010 se instalar a atualização de publicação na Web.</span><span class="sxs-lookup"><span data-stu-id="f48f4-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="f48f4-108">Para obter uma introdução à série, consulte [o primeiro tutorial da série](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="f48f4-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="f48f4-109">Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar SQL Server edições diferentes de SQL Server Compact e mostra como implantar em sites do Windows Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f48f4-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="f48f4-110">Visão geral</span><span class="sxs-lookup"><span data-stu-id="f48f4-110">Overview</span></span>

<span data-ttu-id="f48f4-111">Este tutorial mostra como implantar uma atualização de banco de dados em um banco de dados SQL Server completo.</span><span class="sxs-lookup"><span data-stu-id="f48f4-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="f48f4-112">Como Migrações do Code First faz todo o trabalho de atualizar o banco de dados, o processo é quase idêntico ao que você fez por SQL Server Compact no tutorial [implantando uma atualização de banco de dados](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) .</span><span class="sxs-lookup"><span data-stu-id="f48f4-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="f48f4-113">Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="f48f4-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="f48f4-114">Adicionando uma nova coluna a uma tabela</span><span class="sxs-lookup"><span data-stu-id="f48f4-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="f48f4-115">Nesta seção do tutorial, você fará uma alteração no banco de dados e as alterações de código correspondentes e os testará no Visual Studio na preparação para implantá-los nos ambientes de teste e produção.</span><span class="sxs-lookup"><span data-stu-id="f48f4-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="f48f4-116">A alteração envolve a adição de uma coluna `OfficeHours` à entidade `Instructor` e a exibição das novas informações na página da Web dos **instrutores** .</span><span class="sxs-lookup"><span data-stu-id="f48f4-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="f48f4-117">No projeto ContosoUniversity. DAL, abra *Instructor.cs* e adicione a seguinte propriedade entre as propriedades `HireDate` e `Courses`:</span><span class="sxs-lookup"><span data-stu-id="f48f4-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="f48f4-118">Atualize a classe do inicializador para que ela propaga a nova coluna com dados de teste.</span><span class="sxs-lookup"><span data-stu-id="f48f4-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="f48f4-119">Abra *Migrations\Configuration.cs* e substitua o bloco de código que começa `var instructors = new List<Instructor>` com o seguinte bloco de código que inclui a nova coluna:</span><span class="sxs-lookup"><span data-stu-id="f48f4-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="f48f4-120">No projeto ContosoUniversity, abra *instrutores. aspx* e adicione um novo campo de modelo para o horário comercial logo antes da marca de `</Columns>` de fechamento no primeiro controle `GridView`:</span><span class="sxs-lookup"><span data-stu-id="f48f4-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="f48f4-121">Compile a solução.</span><span class="sxs-lookup"><span data-stu-id="f48f4-121">Build the solution.</span></span>

<span data-ttu-id="f48f4-122">Abra a janela do **console do Gerenciador de pacotes** e selecione CONTOSOUNIVERSITY. Dal como o **projeto padrão**.</span><span class="sxs-lookup"><span data-stu-id="f48f4-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="f48f4-123">Digite os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="f48f4-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="f48f4-124">Execute o aplicativo e selecione a página **instrutores** .</span><span class="sxs-lookup"><span data-stu-id="f48f4-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="f48f4-125">A página leva um pouco mais tempo do que o normal para ser carregada, porque a Entity Framework recria o banco de dados e propaga-o com dado de teste.</span><span class="sxs-lookup"><span data-stu-id="f48f4-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="f48f4-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f48f4-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="f48f4-127">Implantando a atualização do banco de dados no ambiente de teste</span><span class="sxs-lookup"><span data-stu-id="f48f4-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="f48f4-128">Quando você usa Migrações do Code First, o método para implantar uma alteração de banco de dados em SQL Server é o mesmo que para o SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="f48f4-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="f48f4-129">No entanto, você precisa alterar o perfil de publicação de teste porque ele ainda está configurado para migrar de SQL Server Compact para SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f48f4-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="f48f4-130">A primeira etapa é remover as transformações de cadeia de conexão que você criou no tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="f48f4-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="f48f4-131">Elas não são mais necessárias porque você especificará as transformações de cadeia de conexão no perfil de publicação, como fez antes de configurar a guia **pacote/publicar SQL** para migração para o SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f48f4-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="f48f4-132">Abra o arquivo *Web. Test. config* e remova o elemento `connectionStrings`.</span><span class="sxs-lookup"><span data-stu-id="f48f4-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="f48f4-133">A única transformação restante no arquivo *Web. Test. config* é para o valor `Environment` no elemento `appSettings`.</span><span class="sxs-lookup"><span data-stu-id="f48f4-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="f48f4-134">Agora você pode atualizar o perfil de publicação e publicar no ambiente de teste.</span><span class="sxs-lookup"><span data-stu-id="f48f4-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="f48f4-135">Abra o assistente **publicar Web** e, em seguida, alterne para a guia **perfil** .</span><span class="sxs-lookup"><span data-stu-id="f48f4-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="f48f4-136">Selecione o perfil de publicação de **teste** .</span><span class="sxs-lookup"><span data-stu-id="f48f4-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="f48f4-137">Selecione a guia **Configurações**.</span><span class="sxs-lookup"><span data-stu-id="f48f4-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="f48f4-138">Clique em **habilitar novos aprimoramentos de publicação de banco de dados**.</span><span class="sxs-lookup"><span data-stu-id="f48f4-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="f48f4-139">Na caixa Cadeia de conexão para **SchoolContext**, insira o mesmo valor que você usou no arquivo de transformação *Web. Test. config* no tutorial anterior:</span><span class="sxs-lookup"><span data-stu-id="f48f4-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="f48f4-140">Selecione **executar migrações do Code First (é executado no início do aplicativo)** .</span><span class="sxs-lookup"><span data-stu-id="f48f4-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="f48f4-141">(Em sua versão do Visual Studio, a caixa de seleção pode ser rotulada como **aplicar migrações do Code First**.)</span><span class="sxs-lookup"><span data-stu-id="f48f4-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="f48f4-142">Na caixa Cadeia de conexão para **DefaultConnection**, insira o mesmo valor que você usou no arquivo de transformação *Web. Test. config* no tutorial anterior:</span><span class="sxs-lookup"><span data-stu-id="f48f4-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="f48f4-143">Deixe o **banco de dados de atualização** limpo.</span><span class="sxs-lookup"><span data-stu-id="f48f4-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="f48f4-144">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="f48f4-144">Click **Publish**.</span></span>

<span data-ttu-id="f48f4-145">O Visual Studio implanta as alterações de código no ambiente de teste e abre o navegador para a Contoso University home page.</span><span class="sxs-lookup"><span data-stu-id="f48f4-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="f48f4-146">Selecione a página instrutores.</span><span class="sxs-lookup"><span data-stu-id="f48f4-146">Select the Instructors page.</span></span>

<span data-ttu-id="f48f4-147">Quando o aplicativo executa essa página, ele tenta acessar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f48f4-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="f48f4-148">Migrações do Code First verifica se o banco de dados está atualizado e descobre que a migração mais recente ainda não foi aplicada.</span><span class="sxs-lookup"><span data-stu-id="f48f4-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="f48f4-149">Migrações do Code First aplica a migração mais recente, executa o método `Seed` e, em seguida, a página é executada normalmente.</span><span class="sxs-lookup"><span data-stu-id="f48f4-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="f48f4-150">Você vê a nova coluna de horários do Office com os dados propagados.</span><span class="sxs-lookup"><span data-stu-id="f48f4-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="f48f4-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f48f4-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="f48f4-152">Implantando a atualização do banco de dados no ambiente de produção</span><span class="sxs-lookup"><span data-stu-id="f48f4-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="f48f4-153">Também é necessário alterar o perfil de publicação para o ambiente de produção.</span><span class="sxs-lookup"><span data-stu-id="f48f4-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="f48f4-154">Nesse caso, você removerá o perfil existente e criará um novo importando um arquivo. publishsettings atualizado.</span><span class="sxs-lookup"><span data-stu-id="f48f4-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="f48f4-155">O arquivo atualizado incluirá a cadeia de conexão para o banco de dados SQL Server em Cytanium.</span><span class="sxs-lookup"><span data-stu-id="f48f4-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="f48f4-156">Como vimos quando você implantou o no ambiente de teste, você não precisa mais de transformações de cadeia de conexão no arquivo de transformação *Web. Production. config* .</span><span class="sxs-lookup"><span data-stu-id="f48f4-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="f48f4-157">Abra esse arquivo e remova o elemento `connectionStrings`.</span><span class="sxs-lookup"><span data-stu-id="f48f4-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="f48f4-158">As transformações restantes são para o valor `Environment` no elemento `appSettings` e o elemento `location` que restringe o acesso aos relatórios de erros do ELMAH.</span><span class="sxs-lookup"><span data-stu-id="f48f4-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="f48f4-159">Antes de criar um novo perfil de publicação para produção, baixe um arquivo. publishsettings atualizado da mesma maneira que você fez anteriormente no tutorial [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) .</span><span class="sxs-lookup"><span data-stu-id="f48f4-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="f48f4-160">(No painel de controle Cytanium, clique em **sites**e, em seguida, clique no site da **contosouniversity.com** .</span><span class="sxs-lookup"><span data-stu-id="f48f4-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="f48f4-161">Selecione a guia **publicação na Web** e clique em **baixar perfil de publicação para este site**.) O motivo pelo qual você está fazendo isso é pegar a cadeia de conexão do banco de dados no arquivo. publishsettings.</span><span class="sxs-lookup"><span data-stu-id="f48f4-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="f48f4-162">A cadeia de conexão não estava disponível na primeira vez que você baixou o arquivo, pois você ainda estava usando SQL Server Compact e não havia criado o banco de dados SQL Server em Cytanium ainda.</span><span class="sxs-lookup"><span data-stu-id="f48f4-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="f48f4-163">Agora você pode atualizar o perfil de publicação e publicar no ambiente de produção.</span><span class="sxs-lookup"><span data-stu-id="f48f4-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="f48f4-164">Abra o assistente **publicar Web** e, em seguida, alterne para a guia **perfil** .</span><span class="sxs-lookup"><span data-stu-id="f48f4-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="f48f4-165">Clique em **gerenciar perfis**e, em seguida, exclua o perfil de produção.</span><span class="sxs-lookup"><span data-stu-id="f48f4-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="f48f4-166">Feche o assistente **publicar Web** para salvar essa alteração.</span><span class="sxs-lookup"><span data-stu-id="f48f4-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="f48f4-167">Abra o assistente **publicar Web** novamente e clique em **importar**.</span><span class="sxs-lookup"><span data-stu-id="f48f4-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="f48f4-168">Na guia **conexão** , altere a **URL de destino** para o valor apropriado se você estiver usando uma URL temporária.</span><span class="sxs-lookup"><span data-stu-id="f48f4-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="f48f4-169">Clique em **Próximo**.</span><span class="sxs-lookup"><span data-stu-id="f48f4-169">Click **Next**.</span></span>

<span data-ttu-id="f48f4-170">Na guia **configurações** , clique em **habilitar novos aprimoramentos de publicação de banco de dados**.</span><span class="sxs-lookup"><span data-stu-id="f48f4-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="f48f4-171">Na lista suspensa cadeia de conexão para **SchoolContext**, selecione a cadeia de conexão Cytanium.</span><span class="sxs-lookup"><span data-stu-id="f48f4-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="f48f4-173">Selecione **executar Code First migrações (é executado no início do aplicativo)** .</span><span class="sxs-lookup"><span data-stu-id="f48f4-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="f48f4-174">Na lista suspensa cadeia de conexão para **DefaultConnection**, selecione a cadeia de conexão Cytanium.</span><span class="sxs-lookup"><span data-stu-id="f48f4-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="f48f4-175">Selecione a guia **perfil** , clique em **gerenciar perfis**e renomeie o perfil de "contosouniversity.com-implantação da Web" como "produção".</span><span class="sxs-lookup"><span data-stu-id="f48f4-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="f48f4-176">Feche o perfil de publicação para salvar a alteração e, em seguida, abra-a novamente.</span><span class="sxs-lookup"><span data-stu-id="f48f4-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="f48f4-177">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="f48f4-177">Click **Publish**.</span></span> <span data-ttu-id="f48f4-178">(Para um site de produção real, copie o *aplicativo\_offline. htm* para produção e coloque-o em sua pasta de projeto antes de publicar e, em seguida, remova-o quando a implantação for concluída.)</span><span class="sxs-lookup"><span data-stu-id="f48f4-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="f48f4-179">O Visual Studio implanta as alterações de código no ambiente de teste e abre o navegador para a Contoso University home page.</span><span class="sxs-lookup"><span data-stu-id="f48f4-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="f48f4-180">Selecione a página instrutores.</span><span class="sxs-lookup"><span data-stu-id="f48f4-180">Select the Instructors page.</span></span>

<span data-ttu-id="f48f4-181">Migrações do Code First atualiza o banco de dados da mesma maneira que fazia no ambiente de teste.</span><span class="sxs-lookup"><span data-stu-id="f48f4-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="f48f4-182">Você vê a nova coluna de horários do Office com os dados propagados.</span><span class="sxs-lookup"><span data-stu-id="f48f4-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="f48f4-184">Agora você implantou com êxito uma atualização de aplicativo que incluiu uma alteração de banco de dados, usando um banco de dados SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f48f4-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="f48f4-185">Mais informações</span><span class="sxs-lookup"><span data-stu-id="f48f4-185">More Information</span></span>

<span data-ttu-id="f48f4-186">Isso conclui esta série de tutoriais sobre como implantar um aplicativo Web ASP.NET em um provedor de Hospedagem de terceiros.</span><span class="sxs-lookup"><span data-stu-id="f48f4-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="f48f4-187">Para obter mais informações sobre qualquer um dos tópicos abordados nesses tutoriais, consulte o [mapa de conteúdo de implantação do ASP.net](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) no site do MSDN.</span><span class="sxs-lookup"><span data-stu-id="f48f4-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="f48f4-188">Confirmações</span><span class="sxs-lookup"><span data-stu-id="f48f4-188">Acknowledgements</span></span>

<span data-ttu-id="f48f4-189">Gostaria de agradecer às seguintes pessoas que fizeram contribuições significativas para o conteúdo desta série de tutoriais:</span><span class="sxs-lookup"><span data-stu-id="f48f4-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="f48f4-190">Alberto Poblacion, MVP &amp; MCT, Espanha</span><span class="sxs-lookup"><span data-stu-id="f48f4-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="f48f4-191">Jarod Ferguson, MVP de desenvolvimento de plataforma de dados, Estados Unidos</span><span class="sxs-lookup"><span data-stu-id="f48f4-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="f48f4-192">Adverso do Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="f48f4-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="f48f4-193">Kristina Olson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="f48f4-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="f48f4-194">Mike Papa, Microsoft</span><span class="sxs-lookup"><span data-stu-id="f48f4-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="f48f4-195">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="f48f4-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="f48f4-196">Raffaele Rialdi, Itália</span><span class="sxs-lookup"><span data-stu-id="f48f4-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="f48f4-197">Rick Anderson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="f48f4-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="f48f4-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="f48f4-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="f48f4-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="f48f4-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="f48f4-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="f48f4-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="f48f4-201">Srđan Božović, Sérvia</span><span class="sxs-lookup"><span data-stu-id="f48f4-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="f48f4-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="f48f4-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f48f4-203">[Anterior](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="f48f4-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>
