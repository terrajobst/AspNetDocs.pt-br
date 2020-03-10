---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Tutorial: alterar o banco de dados para o EF Database First com o aplicativo MVC ASP.NET'
description: Este tutorial se concentra em fazer uma atualização para a estrutura do banco de dados e propagar essa alteração em todo o aplicativo Web.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 52cad1120908cf0d4f85770f8e2690f9415c5f56
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616259"
---
# <a name="tutorial-change-the-database-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="e3fcb-103">Tutorial: alterar o banco de dados para o EF Database First com o aplicativo MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e3fcb-103">Tutorial: Change the database for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="e3fcb-104">Usando MVC, Entity Framework e ASP.NET scaffolding, você pode criar um aplicativo Web que fornece uma interface para um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="e3fcb-105">Esta série de tutoriais mostra como gerar automaticamente o código que permite que os usuários exibam, editem, criem e excluam dados que residem em uma tabela.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="e3fcb-106">O código gerado corresponde às colunas na tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="e3fcb-107">Este tutorial se concentra em fazer uma atualização para a estrutura do banco de dados e propagar essa alteração em todo o aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-107">This tutorial focuses on making an update to the database structure and propagating that change throughout the web application.</span></span>

<span data-ttu-id="e3fcb-108">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="e3fcb-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e3fcb-109">Adicionar uma coluna</span><span class="sxs-lookup"><span data-stu-id="e3fcb-109">Add a column</span></span>
> * <span data-ttu-id="e3fcb-110">Adicionar a propriedade às exibições</span><span class="sxs-lookup"><span data-stu-id="e3fcb-110">Add the property to the views</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e3fcb-111">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="e3fcb-111">Prerequisites</span></span>

* [<span data-ttu-id="e3fcb-112">Gerando exibições</span><span class="sxs-lookup"><span data-stu-id="e3fcb-112">Generating views</span></span>](generating-views.md)

## <a name="add-a-column"></a><span data-ttu-id="e3fcb-113">Adicionar uma coluna</span><span class="sxs-lookup"><span data-stu-id="e3fcb-113">Add a column</span></span>

<span data-ttu-id="e3fcb-114">Se você atualizar a estrutura de uma tabela em seu banco de dados, precisará garantir que sua alteração seja propagada para o modelo de dados, exibições e controlador.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-114">If you update the structure of a table in your database, you need to ensure that your change is propagated to the data model, views, and controller.</span></span>

<span data-ttu-id="e3fcb-115">Para este tutorial, você adicionará uma nova coluna à tabela Student para registrar o nome do meio do aluno.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-115">For this tutorial, you will add a new column to the Student table to record the middle name of the student.</span></span> <span data-ttu-id="e3fcb-116">Para adicionar essa coluna, abra o projeto de banco de dados e abra o arquivo Student. Sql.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-116">To add this column, open the database project, and open the Student.sql file.</span></span> <span data-ttu-id="e3fcb-117">Por meio do designer ou do código T-SQL, adicione uma coluna chamada **MiddleName** que seja um nvarchar (50) e permita valores nulos.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-117">Through either the designer or the T-SQL code, add a column named **MiddleName** that is an NVARCHAR(50) and allows NULL values.</span></span>

<span data-ttu-id="e3fcb-118">Implante essa alteração em seu banco de dados local iniciando seu projeto de banco de dados (ou F5).</span><span class="sxs-lookup"><span data-stu-id="e3fcb-118">Deploy this change to your local database by starting your database project (or F5).</span></span> <span data-ttu-id="e3fcb-119">O novo campo é adicionado à tabela.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-119">The new field is added to the table.</span></span> <span data-ttu-id="e3fcb-120">Se você não o vir no Pesquisador de Objetos do SQL Server, clique no botão Atualizar no painel.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-120">If you do not see it in the SQL Server Object Explorer, click the Refresh button in the pane.</span></span>

![Mostrar nova coluna](changing-the-database/_static/image2.png)

<span data-ttu-id="e3fcb-122">A nova coluna existe na tabela de banco de dados, mas ela não existe atualmente na classe de modelo de dado.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-122">The new column exists in the database table, but it does not currently exist in the data model class.</span></span> <span data-ttu-id="e3fcb-123">Você deve atualizar o modelo para incluir a nova coluna.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-123">You must update the model to include your new column.</span></span> <span data-ttu-id="e3fcb-124">Na pasta **modelos** , abra o arquivo **ContosoModel. edmx** para exibir o diagrama de modelo.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-124">In the **Models** folder, open the **ContosoModel.edmx** file to display the model diagram.</span></span> <span data-ttu-id="e3fcb-125">Observe que o modelo de aluno não contém a propriedade MiddleName.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-125">Notice that the Student model does not contain the MiddleName property.</span></span> <span data-ttu-id="e3fcb-126">Clique com o botão direito do mouse em qualquer lugar na superfície de design e selecione **atualizar modelo do banco de dados**.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-126">Right-click anywhere on the design surface, and select **Update Model from Database**.</span></span>

<span data-ttu-id="e3fcb-127">No assistente de atualização, selecione a guia **Atualizar** e, em seguida, selecione **tabelas** > **dbo** > **aluno**.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-127">In the Update Wizard, select the **Refresh** tab and then select **Tables** > **dbo** > **Student**.</span></span> <span data-ttu-id="e3fcb-128">Clique em **Concluir**.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-128">Click **Finish**.</span></span>

<span data-ttu-id="e3fcb-129">Depois que o processo de atualização for concluído, o diagrama de banco de dados incluirá a nova propriedade **MiddleName** .</span><span class="sxs-lookup"><span data-stu-id="e3fcb-129">After the update process is finished, the database diagram includes the new **MiddleName** property.</span></span> <span data-ttu-id="e3fcb-130">Salve o arquivo **ContosoModel. edmx** .</span><span class="sxs-lookup"><span data-stu-id="e3fcb-130">Save the **ContosoModel.edmx** file.</span></span> <span data-ttu-id="e3fcb-131">Você deve salvar esse arquivo para que a nova propriedade seja propagada para a classe **Student.cs** .</span><span class="sxs-lookup"><span data-stu-id="e3fcb-131">You must save this file for the new property to be propagated to the **Student.cs** class.</span></span> <span data-ttu-id="e3fcb-132">Agora você atualizou o banco de dados e o modelo.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-132">You have now updated the database and the model.</span></span>

<span data-ttu-id="e3fcb-133">Compile a solução.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-133">Build the solution.</span></span>

## <a name="add-the-property-to-the-views"></a><span data-ttu-id="e3fcb-134">Adicionar a propriedade às exibições</span><span class="sxs-lookup"><span data-stu-id="e3fcb-134">Add the property to the views</span></span>

<span data-ttu-id="e3fcb-135">Infelizmente, as exibições ainda não contêm a nova propriedade.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-135">Unfortunately, the views still do not contain the new property.</span></span> <span data-ttu-id="e3fcb-136">Para atualizar as exibições, você tem duas opções: você pode regenerar as exibições novamente adicionando scaffolding para a classe Student, ou você pode adicionar manualmente a nova propriedade a suas exibições existentes.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-136">To update the views you have two options - you can either re-generate the views by once again adding scaffolding for the Student class, or you can manually add the new property to your existing views.</span></span> <span data-ttu-id="e3fcb-137">Neste tutorial, você adicionará o scaffolding novamente, pois você não fez nenhuma alteração personalizada nas exibições geradas automaticamente.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-137">In this tutorial, you will add the scaffolding again because you have not made any customized changes to the automatically-generated views.</span></span> <span data-ttu-id="e3fcb-138">Você pode considerar adicionar a propriedade manualmente quando tiver feito alterações nas exibições e não quiser perder essas alterações.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-138">You might consider manually adding the property when you have made changes to the views and do not want to lose those changes.</span></span>

<span data-ttu-id="e3fcb-139">Para garantir que as exibições sejam recriadas, exclua a pasta **estudantes** em **exibições**e exclua o **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-139">To ensure the views are re-created, delete the **Students** folder under **Views**, and delete the **StudentsController**.</span></span> <span data-ttu-id="e3fcb-140">Em seguida, clique com o botão direito do mouse na pasta **controladores** e adicione scaffolding para o modelo de **aluno** .</span><span class="sxs-lookup"><span data-stu-id="e3fcb-140">Then, right-click the **Controllers** folder and add scaffolding for the **Student** model.</span></span> <span data-ttu-id="e3fcb-141">Novamente, nomeie o controlador **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-141">Again, name the controller **StudentsController**.</span></span> <span data-ttu-id="e3fcb-142">Selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-142">Select **Add**.</span></span>

<span data-ttu-id="e3fcb-143">Compile a solução novamente.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-143">Build the solution again.</span></span> <span data-ttu-id="e3fcb-144">As exibições agora contêm a propriedade MiddleName.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-144">The views now contain the MiddleName property.</span></span>

![Mostrar nome do meio](changing-the-database/_static/image5.png)

## <a name="next-steps"></a><span data-ttu-id="e3fcb-146">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="e3fcb-146">Next steps</span></span>

<span data-ttu-id="e3fcb-147">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="e3fcb-147">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e3fcb-148">Adicionou uma coluna</span><span class="sxs-lookup"><span data-stu-id="e3fcb-148">Added a column</span></span>
> * <span data-ttu-id="e3fcb-149">Adicionada a propriedade às exibições</span><span class="sxs-lookup"><span data-stu-id="e3fcb-149">Added the property to the views</span></span>

<span data-ttu-id="e3fcb-150">Avance para o próximo tutorial para aprender a personalizar o modo de exibição para mostrar detalhes sobre um registro de aluno.</span><span class="sxs-lookup"><span data-stu-id="e3fcb-150">Advance to the next tutorial to learn how to customize the view for showing details about a student record.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="e3fcb-151">Personalizar uma exibição</span><span class="sxs-lookup"><span data-stu-id="e3fcb-151">Customize a view</span></span>](customizing-a-view.md)