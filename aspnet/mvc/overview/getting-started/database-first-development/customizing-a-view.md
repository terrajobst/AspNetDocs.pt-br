---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Tutorial: personalizar o modo de exibição para o EF Database First com o aplicativo MVC ASP.NET'
description: Este tutorial se concentra em alterar as exibições geradas automaticamente para aprimorar a apresentação.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583590"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a><span data-ttu-id="ea5c5-103">Tutorial: personalizar o modo de exibição para o EF Database First com o aplicativo MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ea5c5-103">Tutorial: Customize view for EF Database First with ASP.NET MVC app</span></span>

<span data-ttu-id="ea5c5-104">Usando MVC, Entity Framework e ASP.NET scaffolding, você pode criar um aplicativo Web que fornece uma interface para um banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-104">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="ea5c5-105">Esta série de tutoriais mostra como gerar automaticamente o código que permite que os usuários exibam, editem, criem e excluam dados que residem em uma tabela.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-105">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="ea5c5-106">O código gerado corresponde às colunas na tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-106">The generated code corresponds to the columns in the database table.</span></span>

<span data-ttu-id="ea5c5-107">Este tutorial se concentra em alterar as exibições geradas automaticamente para aprimorar a apresentação.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-107">This tutorial focuses on changing the automatically-generated views to enhance the presentation.</span></span>

<span data-ttu-id="ea5c5-108">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="ea5c5-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ea5c5-109">Adicionar cursos à página de detalhes do aluno</span><span class="sxs-lookup"><span data-stu-id="ea5c5-109">Add courses to the student detail page</span></span>
> * <span data-ttu-id="ea5c5-110">Confirmar que os cursos são adicionados à página</span><span class="sxs-lookup"><span data-stu-id="ea5c5-110">Confirm that the courses are added to the page</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ea5c5-111">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="ea5c5-111">Prerequisites</span></span>

* [<span data-ttu-id="ea5c5-112">Alterar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="ea5c5-112">Change the database</span></span>](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a><span data-ttu-id="ea5c5-113">Adicionar cursos aos detalhes do aluno</span><span class="sxs-lookup"><span data-stu-id="ea5c5-113">Add courses to student detail</span></span>

<span data-ttu-id="ea5c5-114">O código gerado fornece um bom ponto de partida para seu aplicativo, mas ele não fornece necessariamente todas as funcionalidades de que você precisa em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-114">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="ea5c5-115">Você pode personalizar o código para atender aos requisitos específicos do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-115">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="ea5c5-116">Atualmente, seu aplicativo não exibe os cursos registrados para o aluno selecionado.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-116">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="ea5c5-117">Nesta seção, você adicionará os cursos registrados para cada aluno à exibição de **detalhes** do aluno.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-117">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="ea5c5-118">Abra **exibições** > **alunos** > *Details. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-118">Open **Views** > **Students** > *Details.cshtml*.</span></span> <span data-ttu-id="ea5c5-119">Abaixo da última &lt;/DL&gt; marca, mas antes de fechar &lt;marca de&gt; de/div, adicione o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-119">Below the last &lt;/dl&gt; tag, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="ea5c5-120">Esse código cria uma tabela que exibe uma linha para cada registro na tabela de registro do aluno selecionado.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-120">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="ea5c5-121">O método de **exibição** renderiza HTML para o objeto (ModelItem) que representa a expressão.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-121">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="ea5c5-122">Você usa o método de exibição (em vez de simplesmente inserir o valor da propriedade no código) para garantir que o valor seja formatado corretamente com base em seu tipo e no modelo para esse tipo.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-122">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="ea5c5-123">Neste exemplo, cada expressão retorna uma única propriedade do registro atual no loop e os valores são tipos primitivos que são renderizados como texto.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-123">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

## <a name="confirm-courses-are-added"></a><span data-ttu-id="ea5c5-124">Confirmar que os cursos foram adicionados</span><span class="sxs-lookup"><span data-stu-id="ea5c5-124">Confirm courses are added</span></span>

<span data-ttu-id="ea5c5-125">Execute a solução.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-125">Run the solution.</span></span> <span data-ttu-id="ea5c5-126">Clique em **lista de alunos** e selecione **detalhes** para um dos alunos.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-126">Click **List of students** and select **Details** for one of the students.</span></span> <span data-ttu-id="ea5c5-127">Você verá que os cursos registrados foram incluídos na exibição.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-127">You will see the enrolled courses have been included in the view.</span></span>

![aluno com registro](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a><span data-ttu-id="ea5c5-129">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="ea5c5-129">Next steps</span></span>
<span data-ttu-id="ea5c5-130">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="ea5c5-130">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ea5c5-131">Cursos adicionados à página de detalhes do aluno</span><span class="sxs-lookup"><span data-stu-id="ea5c5-131">Added courses to the student detail page</span></span>
> * <span data-ttu-id="ea5c5-132">Confirmado que os cursos são adicionados à página</span><span class="sxs-lookup"><span data-stu-id="ea5c5-132">Confirmed that the courses are added to the page</span></span>

<span data-ttu-id="ea5c5-133">Avance para o próximo tutorial para saber como adicionar anotações de dados para especificar requisitos de validação e formatação de exibição.</span><span class="sxs-lookup"><span data-stu-id="ea5c5-133">Advance to the next tutorial to learn how to add data annotations to specify validation requirements and display formatting.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="ea5c5-134">Aprimorar a validação de dados</span><span class="sxs-lookup"><span data-stu-id="ea5c5-134">Enhance data validation</span></span>](enhancing-data-validation.md)
