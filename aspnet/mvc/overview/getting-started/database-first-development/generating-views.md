---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Tutorial: gerar exibições para o EF Database First com o aplicativo MVC ASP.NET'
description: Este tutorial se concentra no uso do ASP.NET scaffolding para gerar os controladores e as exibições.
author: Rick-Anderson
ms.author: riande
ms.date: 01/28/2019
ms.topic: tutorial
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: e71e13e22d8a72e1699cfc70d4d93af603edba5b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616203"
---
# <a name="tutorial-generate-views-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="428d3-103">Tutorial: gerar exibições para o EF Database First com o aplicativo MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="428d3-103">Tutorial: Generate views for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="428d3-104">Usando MVC, Entity Framework e ASP.NET scaffolding, você pode criar um aplicativo Web que fornece uma interface para um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="428d3-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="428d3-105">Esta série de tutoriais mostra como gerar automaticamente o código que permite que os usuários exibam, editem, criem e excluam dados que residem em uma tabela.</span><span class="sxs-lookup"><span data-stu-id="428d3-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="428d3-106">O código gerado corresponde às colunas na tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="428d3-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="428d3-107">Este tutorial se concentra no uso do ASP.NET scaffolding para gerar os controladores e as exibições.</span><span class="sxs-lookup"><span data-stu-id="428d3-107">This tutorial focuses on using ASP.NET Scaffolding to generate the controllers and views.</span></span>

<span data-ttu-id="428d3-108">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="428d3-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="428d3-109">Adicionar Scaffold</span><span class="sxs-lookup"><span data-stu-id="428d3-109">Add scaffold</span></span>
> * <span data-ttu-id="428d3-110">Adicionar links a novas exibições</span><span class="sxs-lookup"><span data-stu-id="428d3-110">Add links to new views</span></span>
> * <span data-ttu-id="428d3-111">Exibir exibições de aluno</span><span class="sxs-lookup"><span data-stu-id="428d3-111">Display student views</span></span>
> * <span data-ttu-id="428d3-112">Exibir exibições de registro</span><span class="sxs-lookup"><span data-stu-id="428d3-112">Display enrollment views</span></span>

## <a name="prerequisite"></a><span data-ttu-id="428d3-113">Pré-requisito</span><span class="sxs-lookup"><span data-stu-id="428d3-113">Prerequisite</span></span>

* [<span data-ttu-id="428d3-114">Criar o aplicativo Web e modelos de dados</span><span class="sxs-lookup"><span data-stu-id="428d3-114">Create the web application and data models</span></span>](creating-the-web-application.md)

## <a name="add-scaffold"></a><span data-ttu-id="428d3-115">Adicionar Scaffold</span><span class="sxs-lookup"><span data-stu-id="428d3-115">Add scaffold</span></span>

<span data-ttu-id="428d3-116">Você está pronto para gerar código que fornecerá operações de dados padrão para as classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="428d3-116">You are ready to generate code that will provide standard data operations for the model classes.</span></span> <span data-ttu-id="428d3-117">Você adiciona o código adicionando um item Scaffold.</span><span class="sxs-lookup"><span data-stu-id="428d3-117">You add the code by adding a scaffold item.</span></span> <span data-ttu-id="428d3-118">Há muitas opções para o tipo de scaffolding que você pode adicionar; Neste tutorial, o scaffold incluirá um controlador e exibições que correspondem aos modelos de estudante e de registro que você criou na seção anterior.</span><span class="sxs-lookup"><span data-stu-id="428d3-118">There are many options for the type of scaffolding you can add; in this tutorial, the scaffold will include a controller and views that correspond to the Student and Enrollment models you created in the previous section.</span></span>

<span data-ttu-id="428d3-119">Para manter a consistência em seu projeto, você adicionará o novo controlador à pasta **controladores** existentes.</span><span class="sxs-lookup"><span data-stu-id="428d3-119">To maintain consistency in your project, you will add the new controller to the existing **Controllers** folder.</span></span> <span data-ttu-id="428d3-120">Clique com o botão direito do mouse na pasta **controladores** e selecione **Adicionar** > **novo item com Scaffold**.</span><span class="sxs-lookup"><span data-stu-id="428d3-120">Right-click the **Controllers** folder, and select **Add** > **New Scaffolded Item**.</span></span>

<span data-ttu-id="428d3-121">Selecione o **controlador MVC 5 com exibições, usando Entity Framework** opção.</span><span class="sxs-lookup"><span data-stu-id="428d3-121">Select the **MVC 5 Controller with views, using Entity Framework** option.</span></span> <span data-ttu-id="428d3-122">Esta opção gerará o controlador e as exibições para atualizar, excluir, criar e exibir os dados em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="428d3-122">This option will generate the controller and views for updating, deleting, creating and displaying the data in your model.</span></span>

![Adicionar controlador MVC](generating-views/_static/image2.png)

<span data-ttu-id="428d3-124">Selecione **aluno (ContosoSite. Models)** para a classe de modelo e selecione o **ContosoUniversityDataEntities (ContosoSite. Models)** para a classe de contexto.</span><span class="sxs-lookup"><span data-stu-id="428d3-124">Select **Student (ContosoSite.Models)** for the model class and select the **ContosoUniversityDataEntities (ContosoSite.Models)** for the context class.</span></span> <span data-ttu-id="428d3-125">Mantenha o nome do controlador como **StudentsController**.</span><span class="sxs-lookup"><span data-stu-id="428d3-125">Keep the controller name as **StudentsController**.</span></span>

<span data-ttu-id="428d3-126">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="428d3-126">Click **Add**.</span></span>

<span data-ttu-id="428d3-127">Se você receber um erro, talvez seja porque você não criou o projeto na seção anterior.</span><span class="sxs-lookup"><span data-stu-id="428d3-127">If you receive an error, it may be because you did not build the project in the previous section.</span></span> <span data-ttu-id="428d3-128">Nesse caso, tente Compilar o projeto e, em seguida, adicione o item com Scaffold novamente.</span><span class="sxs-lookup"><span data-stu-id="428d3-128">If so, try building the project, and then add the scaffolded item again.</span></span>

<span data-ttu-id="428d3-129">Depois que o processo de geração de código for concluído, você verá um novo controlador e exibições nos **controladores** e **exibições** do seu projeto > pastas dos **alunos** .</span><span class="sxs-lookup"><span data-stu-id="428d3-129">After the code generation process is complete, you will see a new controller and views in your project's **Controllers** and **Views** > **Students** folders.</span></span>

<span data-ttu-id="428d3-130">Execute as mesmas etapas novamente, mas adicione um Scaffold para a classe de **registro** .</span><span class="sxs-lookup"><span data-stu-id="428d3-130">Perform the same steps again, but add a scaffold for the **Enrollment** class.</span></span> <span data-ttu-id="428d3-131">Quando terminar, você terá um arquivo **EnrollmentsController.cs** e uma pasta em **exibições** nomeadas como **registros** com as exibições criar, excluir, detalhes, editar e indexar.</span><span class="sxs-lookup"><span data-stu-id="428d3-131">When finished, you have an **EnrollmentsController.cs** file, and a folder under **Views** named **Enrollments** with the Create, Delete, Details, Edit and Index views.</span></span>

## <a name="add-links-to-new-views"></a><span data-ttu-id="428d3-132">Adicionar links a novas exibições</span><span class="sxs-lookup"><span data-stu-id="428d3-132">Add links to new views</span></span>

<span data-ttu-id="428d3-133">Para facilitar a navegação para suas novas exibições, você pode adicionar alguns hiperlinks às exibições de índice para estudantes e registros.</span><span class="sxs-lookup"><span data-stu-id="428d3-133">To make it easier for you to navigate to your new views, you can add a couple of hyperlinks to the Index views for students and enrollments.</span></span> <span data-ttu-id="428d3-134">Abra o arquivo em **exibições** > **página inicial** > *index. cshtml*, que é o home page do seu site.</span><span class="sxs-lookup"><span data-stu-id="428d3-134">Open the file at **Views** > **Home** > *Index.cshtml*, which is the home page for your site.</span></span> <span data-ttu-id="428d3-135">Adicione o seguinte código abaixo do Jumbotron.</span><span class="sxs-lookup"><span data-stu-id="428d3-135">Add the following code below the jumbotron.</span></span>

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

<span data-ttu-id="428d3-136">Para o método ActionLink, o primeiro parâmetro é o texto a ser exibido no link.</span><span class="sxs-lookup"><span data-stu-id="428d3-136">For the ActionLink method, the first parameter is the text to display in the link.</span></span> <span data-ttu-id="428d3-137">O segundo parâmetro é a ação e o terceiro parâmetro é o nome do controlador.</span><span class="sxs-lookup"><span data-stu-id="428d3-137">The second parameter is the action and the third parameter is the name of the controller.</span></span> <span data-ttu-id="428d3-138">Por exemplo, o primeiro link aponta para a ação de índice em StudentsController.</span><span class="sxs-lookup"><span data-stu-id="428d3-138">For example, the first link points to the Index action in StudentsController.</span></span> <span data-ttu-id="428d3-139">O hiperlink real é construído com base nesses valores.</span><span class="sxs-lookup"><span data-stu-id="428d3-139">The actual hyperlink is constructed from these values.</span></span> <span data-ttu-id="428d3-140">O primeiro link leva os usuários para o arquivo **index. cshtml** dentro da pasta **views/Students** .</span><span class="sxs-lookup"><span data-stu-id="428d3-140">The first link ultimately takes users to the **Index.cshtml** file within the **Views/Students** folder.</span></span>

## <a name="display-student-views"></a><span data-ttu-id="428d3-141">Exibir exibições de aluno</span><span class="sxs-lookup"><span data-stu-id="428d3-141">Display student views</span></span>

<span data-ttu-id="428d3-142">Você verificará se o código adicionado ao seu projeto exibe corretamente uma lista de alunos e permite que os usuários editem, criem ou excluam os registros de aluno no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="428d3-142">You will verify that the code added to your project correctly displays a list of the students, and enables users to edit, create, or delete the student records in the database.</span></span>

<span data-ttu-id="428d3-143">Clique com o botão direito do mouse no arquivo **views** > **Home** > *index. cshtml* e selecione **Exibir no navegador**.</span><span class="sxs-lookup"><span data-stu-id="428d3-143">Right-click the **Views** > **Home** > *Index.cshtml* file, and select **View in Browser**.</span></span> <span data-ttu-id="428d3-144">No home page do aplicativo, selecione **lista de alunos**.</span><span class="sxs-lookup"><span data-stu-id="428d3-144">On the application home page, select **List of students**.</span></span>

![](generating-views/_static/image6.png)

<span data-ttu-id="428d3-145">Na página **índice** , observe a lista de alunos e links para modificar esses dados.</span><span class="sxs-lookup"><span data-stu-id="428d3-145">On the **Index** page, notice the list of the students and links to modify this data.</span></span> <span data-ttu-id="428d3-146">Selecione o link **criar novo** e forneça alguns valores para um novo aluno.</span><span class="sxs-lookup"><span data-stu-id="428d3-146">Select the **Create New** link and provide some values for a new student.</span></span> <span data-ttu-id="428d3-147">Clique em **criar**e observe que o novo aluno é adicionado à sua lista.</span><span class="sxs-lookup"><span data-stu-id="428d3-147">Click **Create**, and notice the new student is added to your list.</span></span>

<span data-ttu-id="428d3-148">De volta à página **índice** , selecione o link **Editar** e altere alguns dos valores de um aluno.</span><span class="sxs-lookup"><span data-stu-id="428d3-148">Back on the **Index** page, select the **Edit** link, and change some of the values for a student.</span></span> <span data-ttu-id="428d3-149">Clique em **salvar**e observe que o registro do aluno foi alterado.</span><span class="sxs-lookup"><span data-stu-id="428d3-149">Click **Save**, and notice the student record has been changed.</span></span>

<span data-ttu-id="428d3-150">Por fim, selecione o link **excluir** e confirme que você deseja excluir o registro clicando no botão **excluir** .</span><span class="sxs-lookup"><span data-stu-id="428d3-150">Finally, select the **Delete** link and confirm that you want to delete the record by clicking the **Delete** button.</span></span>

<span data-ttu-id="428d3-151">Sem escrever nenhum código, você adicionou exibições que executam operações comuns nos dados na tabela Student.</span><span class="sxs-lookup"><span data-stu-id="428d3-151">Without writing any code, you have added views that perform common operations on the data in the Student table.</span></span>

<span data-ttu-id="428d3-152">Talvez você tenha notado que o rótulo de texto de um campo é baseado na Propriedade do banco de dados (como **LastName**), que não é necessariamente o que você deseja exibir na página da Web.</span><span class="sxs-lookup"><span data-stu-id="428d3-152">You may have noticed that the text label for a field is based on the database property (such as **LastName**) which is not necessarily what you want to display on the web page.</span></span> <span data-ttu-id="428d3-153">Por exemplo, você pode preferir que o rótulo seja **sobrenome**.</span><span class="sxs-lookup"><span data-stu-id="428d3-153">For example, you may prefer the label to be **Last Name**.</span></span> <span data-ttu-id="428d3-154">Você corrigirá esse problema de exibição posteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="428d3-154">You will fix this display issue later in the tutorial.</span></span>

## <a name="display-enrollment-views"></a><span data-ttu-id="428d3-155">Exibir exibições de registro</span><span class="sxs-lookup"><span data-stu-id="428d3-155">Display enrollment views</span></span>

<span data-ttu-id="428d3-156">Seu banco de dados inclui uma relação um-para-muitos entre as tabelas Student e de registro e uma relação um-para-muitos entre as tabelas curso e registro.</span><span class="sxs-lookup"><span data-stu-id="428d3-156">Your database includes a one-to-many relationship between the Student and Enrollment tables, and a one-to-many relationship between the Course and Enrollment tables.</span></span> <span data-ttu-id="428d3-157">As exibições do registro manipulam corretamente essas relações.</span><span class="sxs-lookup"><span data-stu-id="428d3-157">The views for Enrollment correctly handle these relationships.</span></span> <span data-ttu-id="428d3-158">Navegue até o home page do seu site e selecione o link **lista de registros** e, em seguida, o link **criar novo** .</span><span class="sxs-lookup"><span data-stu-id="428d3-158">Navigate to the home page for your site and select the **List of enrollments** link and then the **Create New** link.</span></span>

<span data-ttu-id="428d3-159">A exibição exibe um formulário para criar um novo registro de registro.</span><span class="sxs-lookup"><span data-stu-id="428d3-159">The view displays a form for creating a new enrollment record.</span></span> <span data-ttu-id="428d3-160">Em particular, observe que o formulário contém uma lista suspensa **cursoid** e uma lista suspensa **StudentId** .</span><span class="sxs-lookup"><span data-stu-id="428d3-160">In particular, notice that the form contains a **CourseID** drop-down list and a **StudentID** drop-down list.</span></span> <span data-ttu-id="428d3-161">Ambos são preenchidos com valores das tabelas relacionadas.</span><span class="sxs-lookup"><span data-stu-id="428d3-161">Both are populated with values from the related tables.</span></span>

<span data-ttu-id="428d3-162">Além disso, a validação dos valores fornecidos é aplicada automaticamente com base no tipo de dados do campo.</span><span class="sxs-lookup"><span data-stu-id="428d3-162">Furthermore, validation of the provided values is automatically applied based on the data type of the field.</span></span> <span data-ttu-id="428d3-163">A **classificação** requer um número, portanto, uma mensagem de erro será exibida se você tentar fornecer um valor incompatível: *a classificação do campo deve ser um número.*</span><span class="sxs-lookup"><span data-stu-id="428d3-163">**Grade** requires a number, so an error message is displayed if you try to provide an incompatible value: *The field Grade must be a number.*</span></span>

<span data-ttu-id="428d3-164">Você verificou que as exibições geradas automaticamente permitem que os usuários trabalhem com os dados no banco de dado.</span><span class="sxs-lookup"><span data-stu-id="428d3-164">You have verified that the automatically-generated views enable users to work with the data in the database.</span></span> <span data-ttu-id="428d3-165">No próximo tutorial desta série, você atualizará o banco de dados e fará as alterações correspondentes no aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="428d3-165">In the next tutorial in this series, you will update the database and make the corresponding changes in the web application.</span></span>

## <a name="next-steps"></a><span data-ttu-id="428d3-166">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="428d3-166">Next steps</span></span>

<span data-ttu-id="428d3-167">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="428d3-167">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="428d3-168">Scaffold adicionados</span><span class="sxs-lookup"><span data-stu-id="428d3-168">Added scaffold</span></span>
> * <span data-ttu-id="428d3-169">Links adicionados a novas exibições</span><span class="sxs-lookup"><span data-stu-id="428d3-169">Added links to new views</span></span>
> * <span data-ttu-id="428d3-170">Exibições de aluno exibidas</span><span class="sxs-lookup"><span data-stu-id="428d3-170">Displayed student views</span></span>
> * <span data-ttu-id="428d3-171">Exibições de registro exibidas</span><span class="sxs-lookup"><span data-stu-id="428d3-171">Displayed enrollment views</span></span>

<span data-ttu-id="428d3-172">Avance para o próximo tutorial para saber como alterar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="428d3-172">Advance to the next tutorial to learn how to change the database.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="428d3-173">Alterar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="428d3-173">Change the database</span></span>](changing-the-database.md)