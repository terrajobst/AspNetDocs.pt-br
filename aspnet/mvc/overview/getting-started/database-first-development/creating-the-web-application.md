---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: 'Tutorial: criar o aplicativo Web e modelos de dados para o EF Database First com o ASP.NET MVC'
description: Este tutorial se concentra na criação do aplicativo Web e na geração de modelos de dados com base em suas tabelas de banco.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: 30fd42be5677df6fa6ee0630914098c30d21385b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616266"
---
# <a name="tutorial-create-the-web-application-and-data-models-for-ef-database-first-with-aspnet-mvc"></a><span data-ttu-id="48764-103">Tutorial: criar o aplicativo Web e modelos de dados para o EF Database First com o ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="48764-103">Tutorial: Create the Web Application and Data Models for EF Database First with ASP.NET MVC</span></span>

 <span data-ttu-id="48764-104">Usando MVC, Entity Framework e ASP.NET scaffolding, você pode criar um aplicativo Web que fornece uma interface para um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="48764-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="48764-105">Esta série de tutoriais mostra como gerar automaticamente o código que permite que os usuários exibam, editem, criem e excluam dados que residem em uma tabela.</span><span class="sxs-lookup"><span data-stu-id="48764-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="48764-106">O código gerado corresponde às colunas na tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="48764-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="48764-107">Este tutorial se concentra na criação do aplicativo Web e na geração de modelos de dados com base em suas tabelas de banco.</span><span class="sxs-lookup"><span data-stu-id="48764-107">This tutorial focuses on creating the web application, and generating the data models based on your database tables.</span></span>

<span data-ttu-id="48764-108">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="48764-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="48764-109">Criar um aplicativo Web ASP .NET</span><span class="sxs-lookup"><span data-stu-id="48764-109">Create an ASP.NET web app</span></span>
> * <span data-ttu-id="48764-110">Gerar os modelos</span><span class="sxs-lookup"><span data-stu-id="48764-110">Generate the models</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48764-111">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="48764-111">Prerequisites</span></span>

* [<span data-ttu-id="48764-112">Introdução ao Entity Framework 6 Database First usando o MVC 5</span><span class="sxs-lookup"><span data-stu-id="48764-112">Getting started with Entity Framework 6 Database First using MVC 5</span></span>](setting-up-database.md)

## <a name="create-an-aspnet-web-app"></a><span data-ttu-id="48764-113">Criar um aplicativo Web ASP .NET</span><span class="sxs-lookup"><span data-stu-id="48764-113">Create an ASP.NET web app</span></span>

<span data-ttu-id="48764-114">Em uma nova solução ou na mesma solução que o projeto de banco de dados, crie um novo projeto no Visual Studio e selecione o modelo de **aplicativo Web ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="48764-114">In either a new solution or the same solution as the database project, create a new project in Visual Studio and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="48764-115">Nomeie o projeto **ContosoSite**.</span><span class="sxs-lookup"><span data-stu-id="48764-115">Name the project **ContosoSite**.</span></span>

![criar projeto](creating-the-web-application/_static/image1.png)

<span data-ttu-id="48764-117">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="48764-117">Click **OK**.</span></span>

<span data-ttu-id="48764-118">Na janela novo projeto ASP.NET, selecione o modelo **MVC** .</span><span class="sxs-lookup"><span data-stu-id="48764-118">In the New ASP.NET Project window, select the **MVC** template.</span></span> <span data-ttu-id="48764-119">Você pode limpar o **host na** opção de nuvem por enquanto, pois você implantará o aplicativo na nuvem mais tarde.</span><span class="sxs-lookup"><span data-stu-id="48764-119">You can clear the **Host in the cloud** option for now because you will deploy the application to the cloud later.</span></span> <span data-ttu-id="48764-120">Clique em **OK** para criar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="48764-120">Click **OK** to create the application.</span></span>

<span data-ttu-id="48764-121">O projeto é criado com os arquivos e pastas padrão.</span><span class="sxs-lookup"><span data-stu-id="48764-121">The project is created with the default files and folders.</span></span>

<span data-ttu-id="48764-122">Neste tutorial, você usará o Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="48764-122">In this tutorial, you will use Entity Framework 6.</span></span> <span data-ttu-id="48764-123">Você pode verificar novamente a versão do Entity Framework em seu projeto por meio da janela gerenciar pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="48764-123">You can double-check the version of Entity Framework in your project through the Manage NuGet Packages window.</span></span> <span data-ttu-id="48764-124">Se necessário, atualize sua versão do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="48764-124">If necessary, update your version of Entity Framework.</span></span>

![Mostrar versão](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a><span data-ttu-id="48764-126">Gerar os modelos</span><span class="sxs-lookup"><span data-stu-id="48764-126">Generate the models</span></span>

<span data-ttu-id="48764-127">Agora, você criará Entity Framework modelos a partir das tabelas de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="48764-127">You will now create Entity Framework models from the database tables.</span></span> <span data-ttu-id="48764-128">Esses modelos são classes que você usará para trabalhar com os dados.</span><span class="sxs-lookup"><span data-stu-id="48764-128">These models are classes that you will use to work with the data.</span></span> <span data-ttu-id="48764-129">Cada modelo espelha uma tabela no banco de dados e contém propriedades que correspondem às colunas na tabela.</span><span class="sxs-lookup"><span data-stu-id="48764-129">Each model mirrors a table in the database and contains properties that correspond to the columns in the table.</span></span>

<span data-ttu-id="48764-130">Clique com o botão direito do mouse na pasta **modelos** e selecione **Adicionar** e **novo item**.</span><span class="sxs-lookup"><span data-stu-id="48764-130">Right-click the **Models** folder, and select **Add** and **New Item**.</span></span>

<span data-ttu-id="48764-131">Na janela Adicionar novo item, selecione **dados** no painel esquerdo e **ADO.NET modelo de dados de entidade** das opções no painel central.</span><span class="sxs-lookup"><span data-stu-id="48764-131">In the Add New Item window, select **Data** in the left pane and **ADO.NET Entity Data Model** from the options in the center pane.</span></span> <span data-ttu-id="48764-132">Nomeie o novo arquivo de modelo **ContosoModel**.</span><span class="sxs-lookup"><span data-stu-id="48764-132">Name the new model file **ContosoModel**.</span></span>

<span data-ttu-id="48764-133">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="48764-133">Click **Add**.</span></span>

<span data-ttu-id="48764-134">No assistente de Modelo de Dados de Entidade, selecione **EF designer do banco de dados**.</span><span class="sxs-lookup"><span data-stu-id="48764-134">In the Entity Data Model Wizard, select **EF Designer from database**.</span></span>

<span data-ttu-id="48764-135">Clique em **Próximo**.</span><span class="sxs-lookup"><span data-stu-id="48764-135">Click **Next**.</span></span>

<span data-ttu-id="48764-136">Se você tiver conexões de banco de dados definidas em seu ambiente de desenvolvimento, poderá ver uma dessas conexões previamente selecionada.</span><span class="sxs-lookup"><span data-stu-id="48764-136">If you have database connections defined within your development environment, you may see one of these connections pre-selected.</span></span> <span data-ttu-id="48764-137">No entanto, você deseja criar uma nova conexão com o banco de dados criado na primeira parte deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="48764-137">However, you want to create a new connection to the database you created in the first part of this tutorial.</span></span> <span data-ttu-id="48764-138">Clique no botão **nova conexão** .</span><span class="sxs-lookup"><span data-stu-id="48764-138">Click the **New Connection** button.</span></span>

<span data-ttu-id="48764-139">No janela Propriedades de conexão, forneça o nome do servidor local onde o banco de dados foi criado (neste caso, **\ProjectsV13**).</span><span class="sxs-lookup"><span data-stu-id="48764-139">In the Connection Properties window, provide the name of the local server where your database was created (in this case **(localdb)\ProjectsV13**).</span></span> <span data-ttu-id="48764-140">Depois de fornecer o nome do servidor, selecione o ContosoUniversityData dos bancos de dados disponíveis.</span><span class="sxs-lookup"><span data-stu-id="48764-140">After providing the server name, select the ContosoUniversityData from the available databases.</span></span>

![definir propriedades de conexão](creating-the-web-application/_static/image8.png)

<span data-ttu-id="48764-142">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="48764-142">Click **OK**.</span></span>

<span data-ttu-id="48764-143">As propriedades de conexão corretas agora são exibidas.</span><span class="sxs-lookup"><span data-stu-id="48764-143">The correct connection properties are now displayed.</span></span> <span data-ttu-id="48764-144">Você pode usar o nome padrão para conexão no arquivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="48764-144">You can use the default name for connection in the Web.Config file.</span></span>

<span data-ttu-id="48764-145">Clique em **Próximo**.</span><span class="sxs-lookup"><span data-stu-id="48764-145">Click **Next**.</span></span>

<span data-ttu-id="48764-146">Selecione a versão mais recente do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="48764-146">Select the latest version of Entity Framework.</span></span>

<span data-ttu-id="48764-147">Clique em **Próximo**.</span><span class="sxs-lookup"><span data-stu-id="48764-147">Click **Next**.</span></span>

<span data-ttu-id="48764-148">Selecione **tabelas** para gerar modelos para todas as três tabelas.</span><span class="sxs-lookup"><span data-stu-id="48764-148">Select **Tables** to generate models for all three tables.</span></span>

<span data-ttu-id="48764-149">Clique em **Concluir**.</span><span class="sxs-lookup"><span data-stu-id="48764-149">Click **Finish**.</span></span>

<span data-ttu-id="48764-150">Se você receber um aviso de segurança, selecione **OK** para continuar a execução do modelo.</span><span class="sxs-lookup"><span data-stu-id="48764-150">If you receive a security warning, select **OK** to continue running the template.</span></span>

<span data-ttu-id="48764-151">Os modelos são gerados a partir das tabelas de banco de dados e é exibido um diagrama que mostra as propriedades e relações entre as tabelas.</span><span class="sxs-lookup"><span data-stu-id="48764-151">The models are generated from the database tables, and a diagram is displayed that shows the properties and relationships between the tables.</span></span>

![diagrama de modelo](creating-the-web-application/_static/image11.png)

<span data-ttu-id="48764-153">A pasta modelos agora inclui muitos arquivos novos relacionados aos modelos que foram gerados a partir do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="48764-153">The Models folder now includes many new files related to the models that were generated from the database.</span></span>

<span data-ttu-id="48764-154">O arquivo **ContosoModel.Context.cs** contém uma classe que deriva da classe **DbContext** e fornece uma propriedade para cada classe de modelo que corresponde a uma tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="48764-154">The **ContosoModel.Context.cs** file contains a class that derives from the **DbContext** class, and provides a property for each model class that corresponds to a database table.</span></span> <span data-ttu-id="48764-155">Os arquivos **Course.cs**, **Enrollment.cs**e **Student.cs** contêm as classes de modelo que representam as tabelas de bancos de dados.</span><span class="sxs-lookup"><span data-stu-id="48764-155">The **Course.cs**, **Enrollment.cs**, and **Student.cs** files contain the model classes that represent the databases tables.</span></span> <span data-ttu-id="48764-156">Você usará a classe de contexto e as classes de modelo ao trabalhar com scaffolding.</span><span class="sxs-lookup"><span data-stu-id="48764-156">You will use both the context class and the model classes when working with scaffolding.</span></span>

<span data-ttu-id="48764-157">Antes de prosseguir com este tutorial, compile o projeto.</span><span class="sxs-lookup"><span data-stu-id="48764-157">Before proceeding with this tutorial, build the project.</span></span> <span data-ttu-id="48764-158">Na próxima seção, você gerará código com base nos modelos de dados, mas essa seção não funcionará se o projeto não tiver sido criado.</span><span class="sxs-lookup"><span data-stu-id="48764-158">In the next section, you will generate code based on the data models, but that section will not work if the project has not been built.</span></span>

## <a name="next-steps"></a><span data-ttu-id="48764-159">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="48764-159">Next steps</span></span>

<span data-ttu-id="48764-160">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="48764-160">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="48764-161">Criou um aplicativo Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="48764-161">Created an ASP.NET web app</span></span>
> * <span data-ttu-id="48764-162">Os modelos foram gerados</span><span class="sxs-lookup"><span data-stu-id="48764-162">Generated the models</span></span>

<span data-ttu-id="48764-163">Avance para o próximo tutorial para aprender a criar o código de geração com base nos modelos de dados.</span><span class="sxs-lookup"><span data-stu-id="48764-163">Advance to the next tutorial to learn how to create generate code based on the data models.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="48764-164">Gerando exibições</span><span class="sxs-lookup"><span data-stu-id="48764-164">Generating views</span></span>](generating-views.md)
