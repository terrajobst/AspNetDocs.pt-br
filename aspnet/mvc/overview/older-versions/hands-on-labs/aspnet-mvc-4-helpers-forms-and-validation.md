---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
title: Auxiliares, formulários e validação do ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Nos modelos do ASP.NET MVC 4 e no laboratório prático de acesso a dados, você está carregando e exibindo dados do banco de dado. Neste laboratório prático, você adicionará ao...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 187ee9cd-bc70-479b-bfed-f568b8da96eb
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-helpers-forms-and-validation
msc.type: authoredcontent
ms.openlocfilehash: 0e2605a4188eaf814f6ab0ebfeaabed4457bcfa3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539574"
---
# <a name="aspnet-mvc-4-helpers-forms-and-validation"></a><span data-ttu-id="9576a-104">Validação, formulários e auxiliares do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="9576a-104">ASP.NET MVC 4 Helpers, Forms and Validation</span></span>

<span data-ttu-id="9576a-105">por [equipe de acampamentos da Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="9576a-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="9576a-106">Baixe o kit de treinamento do Web acampamentos</span><span class="sxs-lookup"><span data-stu-id="9576a-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="9576a-107">Nos **modelos do ASP.NET MVC 4 e** no laboratório prático de acesso a dados, você está carregando e exibindo dados do banco de dado.</span><span class="sxs-lookup"><span data-stu-id="9576a-107">In **ASP.NET MVC 4 Models and Data Access** Hands-on Lab, you have been loading and displaying data from the database.</span></span> <span data-ttu-id="9576a-108">Neste laboratório prático, você adicionará ao aplicativo de loja de **música** a capacidade de editar esses dados.</span><span class="sxs-lookup"><span data-stu-id="9576a-108">In this Hands-on Lab, you will add to the **Music Store** application the ability to edit that data.</span></span>

<span data-ttu-id="9576a-109">Com essa meta em mente, primeiro você criará o controlador que dará suporte às ações CRUD (criar, ler, atualizar e excluir) dos álbuns.</span><span class="sxs-lookup"><span data-stu-id="9576a-109">With that goal in mind, you will first create the controller that will support the Create, Read, Update and Delete (CRUD) actions of albums.</span></span> <span data-ttu-id="9576a-110">Você vai gerar um modelo de exibição de índice aproveitando o recurso scaffolding do ASP.NET MVC para exibir as propriedades de álbuns em uma tabela HTML.</span><span class="sxs-lookup"><span data-stu-id="9576a-110">You will generate an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span> <span data-ttu-id="9576a-111">Para aprimorar essa exibição, você adicionará um auxiliar HTML personalizado que truncará descrições longas.</span><span class="sxs-lookup"><span data-stu-id="9576a-111">To enhance that view, you will add a custom HTML helper that will truncate long descriptions.</span></span>

<span data-ttu-id="9576a-112">Posteriormente, você adicionará as exibições editar e criar que permitirão que você altere os álbuns no banco de dados, com a ajuda de elementos de formulário como listas suspensas.</span><span class="sxs-lookup"><span data-stu-id="9576a-112">Afterwards, you will add the Edit and Create Views that will let you alter the albums in the database, with the help of form elements like dropdowns.</span></span>

<span data-ttu-id="9576a-113">Por fim, você permitirá que os usuários excluam um álbum e também os impedirão de inserir dados errados Validando sua entrada.</span><span class="sxs-lookup"><span data-stu-id="9576a-113">Lastly, you will let users delete an album and also you will prevent them from entering wrong data by validating their input.</span></span>

<span data-ttu-id="9576a-114">Este laboratório prático pressupõe que você tenha conhecimento básico do **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="9576a-114">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="9576a-115">Se você não usou o **ASP.NET MVC** antes, recomendamos que você vá para o laboratório prático do **ASP.NET MVC Fundamentals** .</span><span class="sxs-lookup"><span data-stu-id="9576a-115">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC Fundamentals** Hands-on Lab.</span></span>

<span data-ttu-id="9576a-116">Este laboratório orienta você pelos aprimoramentos e novos recursos descritos anteriormente, aplicando pequenas alterações a um aplicativo Web de exemplo fornecido na pasta de origem.</span><span class="sxs-lookup"><span data-stu-id="9576a-116">This lab walks you through the enhancements and new features previously described by applying minor changes to a sample Web application provided in the Source folder.</span></span>

> [!NOTE]
> <span data-ttu-id="9576a-117">Todos os códigos de exemplo e trechos de código estão incluídos no Web acampamentos Training Kit, disponível nas [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="9576a-117">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="9576a-118">O projeto específico deste laboratório está disponível em [auxiliares, formulários e validação do ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span><span class="sxs-lookup"><span data-stu-id="9576a-118">The project specific to this lab is available at [ASP.NET MVC 4 Helpers, Forms and Validation](https://github.com/Microsoft-Web/HOL-MVC4HelpersFormsAndValidation).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="9576a-119">Objetivos</span><span class="sxs-lookup"><span data-stu-id="9576a-119">Objectives</span></span>

<span data-ttu-id="9576a-120">Neste laboratório prático, você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="9576a-120">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="9576a-121">Criar um controlador para dar suporte a operações CRUD</span><span class="sxs-lookup"><span data-stu-id="9576a-121">Create a controller to support CRUD operations</span></span>
- <span data-ttu-id="9576a-122">Gerar uma exibição de índice para exibir propriedades de entidade em uma tabela HTML</span><span class="sxs-lookup"><span data-stu-id="9576a-122">Generate an Index View to display entity properties in an HTML table</span></span>
- <span data-ttu-id="9576a-123">Adicionar um auxiliar HTML personalizado</span><span class="sxs-lookup"><span data-stu-id="9576a-123">Add a custom HTML helper</span></span>
- <span data-ttu-id="9576a-124">Criar e personalizar um modo de exibição de edição</span><span class="sxs-lookup"><span data-stu-id="9576a-124">Create and customize an Edit View</span></span>
- <span data-ttu-id="9576a-125">Diferenciar os métodos de ação que reagem às chamadas HTTP-GET ou HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="9576a-125">Differentiate between action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="9576a-126">Adicionar e personalizar um modo de exibição de criação</span><span class="sxs-lookup"><span data-stu-id="9576a-126">Add and customize a Create View</span></span>
- <span data-ttu-id="9576a-127">Manipular a exclusão de uma entidade</span><span class="sxs-lookup"><span data-stu-id="9576a-127">Handle the deletion of an entity</span></span>
- <span data-ttu-id="9576a-128">Valide a entrada do usuário</span><span class="sxs-lookup"><span data-stu-id="9576a-128">Validate user input</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="9576a-129">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="9576a-129">Prerequisites</span></span>

<span data-ttu-id="9576a-130">Você deve ter os seguintes itens para concluir este laboratório:</span><span class="sxs-lookup"><span data-stu-id="9576a-130">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="9576a-131">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [o apêndice a](#AppendixA) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="9576a-131">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="9576a-132">Instalação</span><span class="sxs-lookup"><span data-stu-id="9576a-132">Setup</span></span>

<span data-ttu-id="9576a-133">**Instalando trechos de código**</span><span class="sxs-lookup"><span data-stu-id="9576a-133">**Installing Code Snippets**</span></span>

<span data-ttu-id="9576a-134">Para sua conveniência, grande parte do código que você gerenciará ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9576a-134">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="9576a-135">Para instalar os trechos de código, execute o arquivo **.\Source\Setup\CodeSnippets.VSI** .</span><span class="sxs-lookup"><span data-stu-id="9576a-135">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="9576a-136">Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot;[Apêndice B: usando trechos de código](#AppendixB)&quot;.</span><span class="sxs-lookup"><span data-stu-id="9576a-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix B: Using Code Snippets](#AppendixB)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="9576a-137">Exercícios</span><span class="sxs-lookup"><span data-stu-id="9576a-137">Exercises</span></span>

<span data-ttu-id="9576a-138">Os seguintes exercícios compõem este laboratório prático:</span><span class="sxs-lookup"><span data-stu-id="9576a-138">The following exercises make up this Hands-On Lab:</span></span>

1. [<span data-ttu-id="9576a-139">Criando o controlador do Store Manager e sua exibição de índice</span><span class="sxs-lookup"><span data-stu-id="9576a-139">Creating the Store Manager controller and its Index view</span></span>](#Exercise1)
2. [<span data-ttu-id="9576a-140">Adicionando um auxiliar HTML</span><span class="sxs-lookup"><span data-stu-id="9576a-140">Adding an HTML Helper</span></span>](#Exercise2)
3. [<span data-ttu-id="9576a-141">Criando o modo de exibição de edição</span><span class="sxs-lookup"><span data-stu-id="9576a-141">Creating the Edit View</span></span>](#Exercise3)
4. [<span data-ttu-id="9576a-142">Adicionando um modo de exibição de criação</span><span class="sxs-lookup"><span data-stu-id="9576a-142">Adding a Create View</span></span>](#Exercise4)
5. [<span data-ttu-id="9576a-143">Tratamento da exclusão</span><span class="sxs-lookup"><span data-stu-id="9576a-143">Handling Deletion</span></span>](#Exercise5)
6. [<span data-ttu-id="9576a-144">Adicionando uma Validação</span><span class="sxs-lookup"><span data-stu-id="9576a-144">Adding Validation</span></span>](#Exercise6)
7. [<span data-ttu-id="9576a-145">Usando o jQuery discreto no lado do cliente</span><span class="sxs-lookup"><span data-stu-id="9576a-145">Using Unobtrusive jQuery at Client Side</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="9576a-146">Cada exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir os exercícios.</span><span class="sxs-lookup"><span data-stu-id="9576a-146">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="9576a-147">Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com os exercícios.</span><span class="sxs-lookup"><span data-stu-id="9576a-147">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="9576a-148">Tempo estimado para concluir este laboratório: **60 minutos**</span><span class="sxs-lookup"><span data-stu-id="9576a-148">Estimated time to complete this lab: **60 minutes**</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_the_Store_Manager_controller_and_its_Index_view"></a>
### <a name="exercise-1-creating-the-store-manager-controller-and-its-index-view"></a><span data-ttu-id="9576a-149">Exercício 1: criando o controlador do Store Manager e sua exibição de índice</span><span class="sxs-lookup"><span data-stu-id="9576a-149">Exercise 1: Creating the Store Manager controller and its Index view</span></span>

<span data-ttu-id="9576a-150">Neste exercício, você aprenderá a criar um novo controlador para dar suporte a operações CRUD, personalizar seu método de ação de índice para retornar uma lista de álbuns do banco de dados e, finalmente, gerar um modelo de exibição de índice aproveitando o scaffolding do ASP.NET MVC recurso para exibir as propriedades de álbuns em uma tabela HTML.</span><span class="sxs-lookup"><span data-stu-id="9576a-150">In this exercise, you will learn how to create a new controller to support CRUD operations, customize its Index action method to return a list of albums from the database and finally generating an Index View template taking advantage of ASP.NET MVC's scaffolding feature to display the albums' properties in an HTML table.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_StoreManagerController"></a>
#### <a name="task-1---creating-the-storemanagercontroller"></a><span data-ttu-id="9576a-151">Tarefa 1-criando o StoreManagerController</span><span class="sxs-lookup"><span data-stu-id="9576a-151">Task 1 - Creating the StoreManagerController</span></span>

<span data-ttu-id="9576a-152">Nesta tarefa, você criará um novo controlador chamado **StoreManagerController** para dar suporte a operações CRUD.</span><span class="sxs-lookup"><span data-stu-id="9576a-152">In this task, you will create a new controller called **StoreManagerController** to support CRUD operations.</span></span>

1. <span data-ttu-id="9576a-153">Abra a solução **inicial** localizada na **origem/EX1-CreatingTheStoreManagerController/início/** pasta.</span><span class="sxs-lookup"><span data-stu-id="9576a-153">Open the **Begin** solution located at **Source/Ex1-CreatingTheStoreManagerController/Begin/** folder.</span></span>

   1. <span data-ttu-id="9576a-154">Será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="9576a-154">You will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9576a-155">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9576a-155">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9576a-156">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="9576a-156">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9576a-157">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="9576a-157">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9576a-158">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="9576a-158">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9576a-159">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="9576a-159">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9576a-160">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="9576a-160">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="9576a-161">Adicione um novo controlador.</span><span class="sxs-lookup"><span data-stu-id="9576a-161">Add a new controller.</span></span> <span data-ttu-id="9576a-162">Para fazer isso, clique com o botão direito do mouse na pasta **controladores** dentro da Gerenciador de soluções, selecione **Adicionar** e, em seguida, o comando **controlador** .</span><span class="sxs-lookup"><span data-stu-id="9576a-162">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="9576a-163">Altere o **nome** do controlador para **StoreManagerController** e verifique se a opção **controlador MVC com ações de leitura/gravação vazias** está selecionada.</span><span class="sxs-lookup"><span data-stu-id="9576a-163">Change the **Controller** **Name** to **StoreManagerController** and make sure the option **MVC controller with empty read/write actions** is selected.</span></span> <span data-ttu-id="9576a-164">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="9576a-164">Click **Add**.</span></span>

    <span data-ttu-id="9576a-165">![Caixa de diálogo Adicionar controlador](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Caixa de diálogo Adicionar controlador")</span><span class="sxs-lookup"><span data-stu-id="9576a-165">![Add controller dialog](aspnet-mvc-4-helpers-forms-and-validation/_static/image1.png "Add controller dialog")</span></span>

    <span data-ttu-id="9576a-166">*Caixa de diálogo Adicionar controlador*</span><span class="sxs-lookup"><span data-stu-id="9576a-166">*Add Controller Dialog*</span></span>

    <span data-ttu-id="9576a-167">Uma nova classe de controlador é gerada.</span><span class="sxs-lookup"><span data-stu-id="9576a-167">A new Controller class is generated.</span></span> <span data-ttu-id="9576a-168">Como você indicou para adicionar ações de leitura/gravação, métodos de stub para eles, ações CRUD comuns são criadas com comentários TODO preenchidos, solicitando que incluam a lógica específica do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9576a-168">Since you indicated to add actions for read/write, stub methods for those, common CRUD actions are created with TODO comments filled in, prompting to include the application specific logic.</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Customizing_the_StoreManager_Index"></a>
#### <a name="task-2---customizing-the-storemanager-index"></a><span data-ttu-id="9576a-169">Tarefa 2-Personalizando o índice Storemanager</span><span class="sxs-lookup"><span data-stu-id="9576a-169">Task 2 - Customizing the StoreManager Index</span></span>

<span data-ttu-id="9576a-170">Nesta tarefa, você personalizará o método de ação de índice Storemanager para retornar uma exibição com a lista de álbuns do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9576a-170">In this task, you will customize the StoreManager Index action method to return a View with the list of albums from the database.</span></span>

1. <span data-ttu-id="9576a-171">Na classe StoreManagerController, adicione as seguintes diretivas *using* .</span><span class="sxs-lookup"><span data-stu-id="9576a-171">In the StoreManagerController class, add the following *using* directives.</span></span>

    <span data-ttu-id="9576a-172">(Trecho de código- *auxiliares e formulários do ASP.NET MVC 4 e validação-EX1 usando MvcMusicStore*)</span><span class="sxs-lookup"><span data-stu-id="9576a-172">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 using MvcMusicStore*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample1.cs)]
2. <span data-ttu-id="9576a-173">Adicione um campo ao **StoreManagerController** para manter uma instância de **MusicStoreEntities.**</span><span class="sxs-lookup"><span data-stu-id="9576a-173">Add a field to the **StoreManagerController** to hold an instance of **MusicStoreEntities.**</span></span>

    <span data-ttu-id="9576a-174">(Trecho de código- *auxiliares ASP.NET MVC 4 e Forms e Validation-EX1 MusicStoreEntities*)</span><span class="sxs-lookup"><span data-stu-id="9576a-174">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 MusicStoreEntities*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample2.cs)]
3. <span data-ttu-id="9576a-175">Implemente a ação de índice StoreManagerController para retornar uma exibição com a lista de álbuns.</span><span class="sxs-lookup"><span data-stu-id="9576a-175">Implement the StoreManagerController Index action to return a View with the list of albums.</span></span>

    <span data-ttu-id="9576a-176">A lógica de ação do controlador será muito semelhante à ação de índice do StoreController gravada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="9576a-176">The Controller action logic will be very similar to the StoreController's Index action written earlier.</span></span> <span data-ttu-id="9576a-177">Use o LINQ para recuperar todos os álbuns, incluindo informações de gênero e artista para exibição.</span><span class="sxs-lookup"><span data-stu-id="9576a-177">Use LINQ to retrieve all albums, including Genre and Artist information for display.</span></span>

    <span data-ttu-id="9576a-178">(Trecho de código- *ASP.net e auxiliares MVC 4 e Forms e Validation-EX1 StoreManagerController index*)</span><span class="sxs-lookup"><span data-stu-id="9576a-178">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex1 StoreManagerController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample3.cs)]

<a id="Ex1Task3"></a>

<a id="strongTask_3_-_Creating_the_Index_Viewstrong"></a>
#### <a name="task-3---creating-the-index-view"></a><span data-ttu-id="9576a-179">Tarefa 3-criando a exibição de índice</span><span class="sxs-lookup"><span data-stu-id="9576a-179">Task 3 - Creating the Index View</span></span>

<span data-ttu-id="9576a-180">Nesta tarefa, você criará o modelo de exibição de índice para exibir a lista de álbuns retornados pelo controlador **storemanager** .</span><span class="sxs-lookup"><span data-stu-id="9576a-180">In this task, you will create the Index View template to display the list of albums returned by the **StoreManager** Controller.</span></span>

1. <span data-ttu-id="9576a-181">Antes de criar o novo modelo de exibição, você deve compilar o projeto para que a **caixa de diálogo Adicionar exibição** saiba sobre a classe de **álbum** a ser usada.</span><span class="sxs-lookup"><span data-stu-id="9576a-181">Before creating the new View template, you should build the project so that the **Add View Dialog** knows about the **Album** class to use.</span></span> <span data-ttu-id="9576a-182">Selecionar **compilação | Crie MvcMusicStore** para compilar o projeto.</span><span class="sxs-lookup"><span data-stu-id="9576a-182">Select **Build | Build MvcMusicStore** to build the project.</span></span>
2. <span data-ttu-id="9576a-183">Clique com o botão direito do mouse dentro do método de ação de **índice** e selecione **Adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="9576a-183">Right-click inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="9576a-184">Isso abrirá a caixa de diálogo **Adicionar exibição** .</span><span class="sxs-lookup"><span data-stu-id="9576a-184">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="9576a-185">![Adicionar modo de exibição](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Adicionar modo de exibição")</span><span class="sxs-lookup"><span data-stu-id="9576a-185">![Add view](aspnet-mvc-4-helpers-forms-and-validation/_static/image2.png "Add view")</span></span>

    <span data-ttu-id="9576a-186">*Adicionando uma exibição de dentro do método de índice*</span><span class="sxs-lookup"><span data-stu-id="9576a-186">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="9576a-187">Na caixa de diálogo Adicionar exibição, verifique se o nome da exibição é **índice**.</span><span class="sxs-lookup"><span data-stu-id="9576a-187">In the Add View dialog, verify that the View Name is **Index**.</span></span> <span data-ttu-id="9576a-188">Selecione a opção **criar uma exibição fortemente tipada** e selecione **álbum (MvcMusicStore. Models)** na lista suspensa **classe de modelo** .</span><span class="sxs-lookup"><span data-stu-id="9576a-188">Select the **Create a strongly-typed view** option, and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="9576a-189">Selecione **list na lista** suspensa **modelo de Scaffold** .</span><span class="sxs-lookup"><span data-stu-id="9576a-189">Select **List** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="9576a-190">Deixe o **mecanismo de exibição** para o **Razor** e os outros campos com seu valor padrão e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="9576a-190">Leave the **View engine** to **Razor** and the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="9576a-191">![Adicionando uma exibição de índice](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adicionando uma exibição de índice")</span><span class="sxs-lookup"><span data-stu-id="9576a-191">![Adding an index view](aspnet-mvc-4-helpers-forms-and-validation/_static/image3.png "Adding an index view")</span></span>

    <span data-ttu-id="9576a-192">*Adicionando uma exibição de índice*</span><span class="sxs-lookup"><span data-stu-id="9576a-192">*Adding an Index View*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Customizing_the_scaffold_of_the_Index_View"></a>
#### <a name="task-4---customizing-the-scaffold-of-the-index-view"></a><span data-ttu-id="9576a-193">Tarefa 4-Personalizando o scaffold da exibição do índice</span><span class="sxs-lookup"><span data-stu-id="9576a-193">Task 4 - Customizing the scaffold of the Index View</span></span>

<span data-ttu-id="9576a-194">Nesta tarefa, você ajustará o modelo de exibição simples criado com o recurso ASP.NET MVC scaffolding para que ele exiba os campos desejados.</span><span class="sxs-lookup"><span data-stu-id="9576a-194">In this task, you will adjust the simple View template created with ASP.NET MVC scaffolding feature to have it display the fields you want.</span></span>

> [!NOTE]
> <span data-ttu-id="9576a-195">O suporte do **scaffolding** no ASP.NET MVC gera um modelo de exibição simples que lista todos os campos no modelo de álbum.</span><span class="sxs-lookup"><span data-stu-id="9576a-195">The **scaffolding** support within ASP.NET MVC generates a simple View template which lists all fields in the Album model.</span></span> <span data-ttu-id="9576a-196">O **scaffolding** fornece uma maneira rápida de começar a usar uma exibição fortemente tipada: em vez de ter que escrever o modelo de exibição manualmente, o scaffolding gera rapidamente um modelo padrão e, em seguida, você pode modificar o código gerado.</span><span class="sxs-lookup"><span data-stu-id="9576a-196">**Scaffolding** provides a quick way to get started on a strongly typed view: rather than having to write the View template manually, scaffolding quickly generates a default template and then you can modify the generated code.</span></span>

1. <span data-ttu-id="9576a-197">Examine o código criado.</span><span class="sxs-lookup"><span data-stu-id="9576a-197">Review the code created.</span></span> <span data-ttu-id="9576a-198">A lista de campos gerada fará parte da seguinte tabela HTML que o **scaffolding** está usando para exibir dados tabulares.</span><span class="sxs-lookup"><span data-stu-id="9576a-198">The generated list of fields will be part of the following HTML table that **Scaffolding** is using for displaying tabular data.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample4.cshtml)]
2. <span data-ttu-id="9576a-199">Substitua a **tabela&lt;&gt;** código pelo código a seguir para exibir apenas os campos **gênero**, **artista**, **título do álbum**e **preço** .</span><span class="sxs-lookup"><span data-stu-id="9576a-199">Replace the **&lt;table&gt;** code with the following code to display only the **Genre**, **Artist**, **Album Title**, and **Price** fields.</span></span> <span data-ttu-id="9576a-200">Isso exclui as colunas de URL do **álbumid** e da **arte do álbum** .</span><span class="sxs-lookup"><span data-stu-id="9576a-200">This deletes the **AlbumId** and **Album Art URL** columns.</span></span> <span data-ttu-id="9576a-201">Além disso, ele altera as colunas Gêneroid e Artistaid para exibir suas propriedades de classe vinculadas de **Artist.Name** e **Genre.Name**e remove o link **Details** .</span><span class="sxs-lookup"><span data-stu-id="9576a-201">Also, it changes GenreId and ArtistId columns to display their linked class properties of **Artist.Name** and **Genre.Name**, and removes the **Details** link.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample5.cshtml)]
3. <span data-ttu-id="9576a-202">Altere as descrições a seguir.</span><span class="sxs-lookup"><span data-stu-id="9576a-202">Change the following descriptions.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample6.cshtml)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="9576a-203">Tarefa 5-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="9576a-203">Task 5 - Running the Application</span></span>

<span data-ttu-id="9576a-204">Nesta tarefa, você testará que o modelo de exibição de **índice** do **storemanager** exibe uma lista de álbuns de acordo com o design das etapas anteriores.</span><span class="sxs-lookup"><span data-stu-id="9576a-204">In this task, you will test that the **StoreManager** **Index** View template displays a list of albums according to the design of the previous steps.</span></span>

1. <span data-ttu-id="9576a-205">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9576a-205">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9576a-206">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="9576a-206">The project starts in the Home page.</span></span> <span data-ttu-id="9576a-207">Altere a URL para **/StoreManager** para verificar se uma lista de álbuns é exibida, mostrando seu **título**, **artista** e **gênero**.</span><span class="sxs-lookup"><span data-stu-id="9576a-207">Change the URL to **/StoreManager** to verify that a list of albums is displayed, showing their **Title**, **Artist** and **Genre**.</span></span>

    <span data-ttu-id="9576a-208">![Navegando na lista de álbuns](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Navegando na lista de álbuns")</span><span class="sxs-lookup"><span data-stu-id="9576a-208">![Browsing the list of albums](aspnet-mvc-4-helpers-forms-and-validation/_static/image4.png "Browsing the list of albums")</span></span>

    <span data-ttu-id="9576a-209">*Navegando na lista de álbuns*</span><span class="sxs-lookup"><span data-stu-id="9576a-209">*Browsing the list of albums*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Adding_an_HTML_Helper"></a>
### <a name="exercise-2-adding-an-html-helper"></a><span data-ttu-id="9576a-210">Exercício 2: adicionando um auxiliar HTML</span><span class="sxs-lookup"><span data-stu-id="9576a-210">Exercise 2: Adding an HTML Helper</span></span>

<span data-ttu-id="9576a-211">A página de índice do Storemanager tem um possível problema: as propriedades de título e de nome do artista podem ser longas o suficiente para lançar a formatação da tabela.</span><span class="sxs-lookup"><span data-stu-id="9576a-211">The StoreManager Index page has one potential issue: Title and Artist Name properties can both be long enough to throw off the table formatting.</span></span> <span data-ttu-id="9576a-212">Neste exercício, você aprenderá a adicionar um auxiliar HTML personalizado para truncar esse texto.</span><span class="sxs-lookup"><span data-stu-id="9576a-212">In this exercise you will learn how to add a custom HTML helper to truncate that text.</span></span>

<span data-ttu-id="9576a-213">Na figura a seguir, você pode ver como o formato é modificado devido ao comprimento do texto quando você usa um pequeno tamanho de navegador.</span><span class="sxs-lookup"><span data-stu-id="9576a-213">In the following figure, you can see how the format is modified because of the length of the text when you use a small browser size.</span></span>

<span data-ttu-id="9576a-214">![Navegando na lista de álbuns com texto não truncado](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Navegando na lista de álbuns com texto não truncado")</span><span class="sxs-lookup"><span data-stu-id="9576a-214">![Browsing the list of Albums with not truncated text](aspnet-mvc-4-helpers-forms-and-validation/_static/image5.png "Browsing the list of Albums with not truncated text")</span></span>

<span data-ttu-id="9576a-215">*Navegando na lista de álbuns com texto não truncado*</span><span class="sxs-lookup"><span data-stu-id="9576a-215">*Browsing the list of Albums with not truncated text*</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Extending_the_HTML_Helper"></a>
#### <a name="task-1---extending-the-html-helper"></a><span data-ttu-id="9576a-216">Tarefa 1-estendendo o auxiliar HTML</span><span class="sxs-lookup"><span data-stu-id="9576a-216">Task 1 - Extending the HTML Helper</span></span>

<span data-ttu-id="9576a-217">Nesta tarefa, você adicionará um novo método **truncado** para o objeto **HTML** exposto nas exibições do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9576a-217">In this task, you will add a new method **Truncate** to the **HTML** object exposed within ASP.NET MVC Views.</span></span> <span data-ttu-id="9576a-218">Para fazer isso, você vai implementar um **método de extensão** para a classe **System. Web. Mvc. HtmlHelper** interna fornecida pelo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9576a-218">To do this, you will implement an **extension method** to the built-in **System.Web.Mvc.HtmlHelper** class provided by ASP.NET MVC.</span></span>

> [!NOTE]
> <span data-ttu-id="9576a-219">Para ler mais sobre os **métodos de extensão**, visite este artigo do MSDN.</span><span class="sxs-lookup"><span data-stu-id="9576a-219">To read more about **Extension Methods**, please visit this msdn article.</span></span> <span data-ttu-id="9576a-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span><span class="sxs-lookup"><span data-stu-id="9576a-220">[https://msdn.microsoft.com/library/bb383977.aspx](https://msdn.microsoft.com/library/bb383977.aspx).</span></span>

1. <span data-ttu-id="9576a-221">Abra a solução **inicial** localizada na **origem/EX2-AddingAnHTMLHelper/início/** pasta.</span><span class="sxs-lookup"><span data-stu-id="9576a-221">Open the **Begin** solution located at **Source/Ex2-AddingAnHTMLHelper/Begin/** folder.</span></span> <span data-ttu-id="9576a-222">Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="9576a-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="9576a-223">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="9576a-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9576a-224">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9576a-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9576a-225">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="9576a-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9576a-226">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="9576a-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9576a-227">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="9576a-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9576a-228">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="9576a-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9576a-229">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="9576a-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="9576a-230">Abra a exibição de índice do Storemanager.</span><span class="sxs-lookup"><span data-stu-id="9576a-230">Open StoreManager's Index View.</span></span> <span data-ttu-id="9576a-231">Para fazer isso, no Gerenciador de Soluções expanda a pasta **exibições** , em seguida, o **storemanager** e abra o arquivo **index. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="9576a-231">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
3. <span data-ttu-id="9576a-232">Adicione o seguinte código abaixo da diretiva <strong>@model</strong> para definir o método auxiliar <strong>TRUNCATE</strong> .</span><span class="sxs-lookup"><span data-stu-id="9576a-232">Add the following code below the <strong>@model</strong> directive to define the <strong>Truncate</strong> helper method.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample7.cshtml)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Truncating_Text_in_the_Page"></a>
#### <a name="task-2---truncating-text-in-the-page"></a><span data-ttu-id="9576a-233">Tarefa 2 – truncando texto na página</span><span class="sxs-lookup"><span data-stu-id="9576a-233">Task 2 - Truncating Text in the Page</span></span>

<span data-ttu-id="9576a-234">Nesta tarefa, você usará o método **TRUNCATE** para truncar o texto no modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="9576a-234">In this task, you will use the **Truncate** method to truncate the text in the View template.</span></span>

1. <span data-ttu-id="9576a-235">Abra a exibição de índice do Storemanager.</span><span class="sxs-lookup"><span data-stu-id="9576a-235">Open StoreManager's Index View.</span></span> <span data-ttu-id="9576a-236">Para fazer isso, no Gerenciador de Soluções expanda a pasta **exibições** , em seguida, o **storemanager** e abra o arquivo **index. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="9576a-236">To do this, in the Solution Explorer expand the **Views** folder, then the **StoreManager** and open the **Index.cshtml** file.</span></span>
2. <span data-ttu-id="9576a-237">Substitua as linhas que mostram o **nome do artista** e o **título**do álbum.</span><span class="sxs-lookup"><span data-stu-id="9576a-237">Replace the lines that show the **Artist Name** and Album's **Title**.</span></span> <span data-ttu-id="9576a-238">Para fazer isso, substitua as linhas a seguir.</span><span class="sxs-lookup"><span data-stu-id="9576a-238">To do this, replace the following lines.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample8.cshtml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="9576a-239">Tarefa 3-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="9576a-239">Task 3 - Running the Application</span></span>

<span data-ttu-id="9576a-240">Nesta tarefa, você testará que o modelo de exibição de **índice** do **storemanager** trunca o título e o nome do artista do álbum.</span><span class="sxs-lookup"><span data-stu-id="9576a-240">In this task, you will test that the **StoreManager** **Index** View template truncates the Album's Title and Artist Name.</span></span>

1. <span data-ttu-id="9576a-241">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9576a-241">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9576a-242">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="9576a-242">The project starts in the Home page.</span></span> <span data-ttu-id="9576a-243">Altere a URL para **/StoreManager** para verificar se textos longos na coluna **título** e **artista** estão truncados.</span><span class="sxs-lookup"><span data-stu-id="9576a-243">Change the URL to **/StoreManager** to verify that long texts in the **Title** and **Artist** column are truncated.</span></span>

    <span data-ttu-id="9576a-244">![Nomes de títulos e artistas truncados](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Nomes de títulos e artistas truncados")</span><span class="sxs-lookup"><span data-stu-id="9576a-244">![Truncated titles and artists names](aspnet-mvc-4-helpers-forms-and-validation/_static/image6.png "Truncated titles and artists names")</span></span>

    <span data-ttu-id="9576a-245">*Títulos truncados e nomes de artistas*</span><span class="sxs-lookup"><span data-stu-id="9576a-245">*Truncated Titles and Artist Names*</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Creating_the_Edit_View"></a>
### <a name="exercise-3-creating-the-edit-view"></a><span data-ttu-id="9576a-246">Exercício 3: criando o modo de exibição de edição</span><span class="sxs-lookup"><span data-stu-id="9576a-246">Exercise 3: Creating the Edit View</span></span>

<span data-ttu-id="9576a-247">Neste exercício, você aprenderá a criar um formulário para permitir que os gerentes de loja editem um álbum.</span><span class="sxs-lookup"><span data-stu-id="9576a-247">In this exercise, you will learn how to create a form to allow store managers to edit an Album.</span></span> <span data-ttu-id="9576a-248">Eles procurarão a URL **/StoreManager/Edit/ID** (**ID** sendo a ID exclusiva do álbum a ser editada), fazendo assim uma chamada HTTP-Get para o servidor.</span><span class="sxs-lookup"><span data-stu-id="9576a-248">They will browse the **/StoreManager/Edit/id** URL (**id** being the unique id of the album to edit), thus making an HTTP-GET call to the server.</span></span>

<span data-ttu-id="9576a-249">O método de ação de edição do controlador recuperará o álbum apropriado do banco de dados, criará um objeto **StoreManagerViewModel** para encapsulá-lo (junto com uma lista de artistas e gêneros) e, em seguida, passá-lo para um modelo de exibição para renderizar a página HTML de volta para o usuário.</span><span class="sxs-lookup"><span data-stu-id="9576a-249">The Controller Edit action method will retrieve the appropriate Album from the database, create a **StoreManagerViewModel** object to encapsulate it (along with a list of Artists and Genres), and then pass it off to a View template to render the HTML page back to the user.</span></span> <span data-ttu-id="9576a-250">Esta página conterá um **&lt;formulário&gt;** elemento com caixas de Texte listas suspensas para editar as propriedades do álbum.</span><span class="sxs-lookup"><span data-stu-id="9576a-250">This page will contain a **&lt;form&gt;** element with textboxes and dropdowns for editing the Album properties.</span></span>

<span data-ttu-id="9576a-251">Depois que o usuário atualiza os valores de formulário do álbum e clica no botão **salvar** , as alterações são enviadas por meio de uma chamada http-post de volta para **/StoreManager/Edit/ID**. Embora a URL permaneça a mesma da última chamada, o ASP.NET MVC identifica que, desta vez é um HTTP-POST e, portanto, executa um método de ação de edição diferente (um decorado com **[HttpPost]** ).</span><span class="sxs-lookup"><span data-stu-id="9576a-251">Once the user updates the Album form values and clicks the **Save** button, the changes are submitted via an HTTP-POST call back to **/StoreManager/Edit/id**. Although the URL remains the same as in the last call, ASP.NET MVC identifies that this time it is an HTTP-POST and therefore executes a different Edit action method (one decorated with **[HttpPost]**).</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Edit_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-edit-action-method"></a><span data-ttu-id="9576a-252">Tarefa 1-implementando o método de ação de edição HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="9576a-252">Task 1 - Implementing the HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="9576a-253">Nesta tarefa, você implementará a versão HTTP-GET do método editar ação para recuperar o álbum apropriado do banco de dados, bem como uma lista de todos os gêneros e artistas.</span><span class="sxs-lookup"><span data-stu-id="9576a-253">In this task, you will implement the HTTP-GET version of the Edit action method to retrieve the appropriate Album from the database, as well as a list of all Genres and Artists.</span></span> <span data-ttu-id="9576a-254">Ele empacotará esses dados no objeto **StoreManagerViewModel** definido na última etapa, que será então passada para um modelo de exibição para renderizar a resposta.</span><span class="sxs-lookup"><span data-stu-id="9576a-254">It will package this data up into the **StoreManagerViewModel** object defined in the last step, which will then be passed to a View template to render the response with.</span></span>

1. <span data-ttu-id="9576a-255">Abra a solução **inicial** localizada na **origem/EX3-CreatingTheEditView/início/** pasta.</span><span class="sxs-lookup"><span data-stu-id="9576a-255">Open the **Begin** solution located at **Source/Ex3-CreatingTheEditView/Begin/** folder.</span></span> <span data-ttu-id="9576a-256">Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="9576a-256">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="9576a-257">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="9576a-257">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9576a-258">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9576a-258">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9576a-259">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="9576a-259">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9576a-260">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="9576a-260">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9576a-261">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="9576a-261">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9576a-262">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="9576a-262">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9576a-263">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="9576a-263">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="9576a-264">Abra a classe **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="9576a-264">Open the **StoreManagerController** class.</span></span> <span data-ttu-id="9576a-265">Para fazer isso, expanda a pasta **controladores** e clique duas vezes em **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="9576a-265">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="9576a-266">Substitua o método de ação de **edição http-Get** pelo código a seguir para recuperar o **álbum** apropriado, bem como as listas de **gêneros** e **artistas** .</span><span class="sxs-lookup"><span data-stu-id="9576a-266">Replace the **HTTP-GET Edit** action method with the following code to retrieve the appropriate **Album** as well as the **Genres** and **Artists** lists.</span></span>

    <span data-ttu-id="9576a-267">(Trecho de código- *ASP.net e auxiliares MVC 4 e formulários e validação – EX3 STOREMANAGERCONTROLLER http – obter ação de edição*)</span><span class="sxs-lookup"><span data-stu-id="9576a-267">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-GET Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="9576a-268">Você está usando a lista de **seleção** de **System. Web. Mvc** para artistas e gêneros em vez de **System. Collections. genérica** List.</span><span class="sxs-lookup"><span data-stu-id="9576a-268">You are using **System.Web.Mvc** **SelectList** for Artists and Genres instead of the **System.Collections.Generic** List.</span></span>
    > 
    > <span data-ttu-id="9576a-269">A lista de **seleção** é uma maneira mais limpa de preencher listas suspensas HTML e gerenciar coisas como a seleção atual.</span><span class="sxs-lookup"><span data-stu-id="9576a-269">**SelectList** is a cleaner way to populate HTML dropdowns and manage things like current selection.</span></span> <span data-ttu-id="9576a-270">Instanciar e posteriormente configurar esses objetos ViewModel na ação do controlador tornará o limpador do cenário Editar formulário.</span><span class="sxs-lookup"><span data-stu-id="9576a-270">Instantiating and later setting up these ViewModel objects in the controller action will make the Edit form scenario cleaner.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Creating_the_Edit_View"></a>
#### <a name="task-2---creating-the-edit-view"></a><span data-ttu-id="9576a-271">Tarefa 2-Criando o modo de exibição de edição</span><span class="sxs-lookup"><span data-stu-id="9576a-271">Task 2 - Creating the Edit View</span></span>

<span data-ttu-id="9576a-272">Nesta tarefa, você criará um modelo de exibição de edição que exibirá posteriormente as propriedades do álbum.</span><span class="sxs-lookup"><span data-stu-id="9576a-272">In this task, you will create an Edit View template that will later display the album properties.</span></span>

1. <span data-ttu-id="9576a-273">Crie o modo de exibição de edição.</span><span class="sxs-lookup"><span data-stu-id="9576a-273">Create the Edit View.</span></span> <span data-ttu-id="9576a-274">Para fazer isso, clique com o botão direito do mouse dentro do método **Editar** ação e selecione **Adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="9576a-274">To do this, right-click inside the **Edit** action method and select **Add View**.</span></span>
2. <span data-ttu-id="9576a-275">Na caixa de diálogo Adicionar exibição, verifique se o nome da exibição é **Editar**.</span><span class="sxs-lookup"><span data-stu-id="9576a-275">In the Add View dialog, verify that the View Name is **Edit**.</span></span> <span data-ttu-id="9576a-276">Marque a caixa de seleção **criar uma exibição fortemente tipada** e selecione **álbum (MvcMusicStore. Models)** na lista suspensa **Exibir classe de dados** .</span><span class="sxs-lookup"><span data-stu-id="9576a-276">Check the **Create a strongly-typed view** checkbox and select **Album (MvcMusicStore.Models)** from the **View data class** drop-down.</span></span> <span data-ttu-id="9576a-277">Selecione **Editar** na lista suspensa **modelo de Scaffold** .</span><span class="sxs-lookup"><span data-stu-id="9576a-277">Select **Edit** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="9576a-278">Deixe os outros campos com o valor padrão e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="9576a-278">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="9576a-279">![Adicionando uma exibição de edição](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adicionando uma exibição de edição")</span><span class="sxs-lookup"><span data-stu-id="9576a-279">![Adding an Edit view](aspnet-mvc-4-helpers-forms-and-validation/_static/image7.png "Adding an Edit view")</span></span>

    <span data-ttu-id="9576a-280">*Adicionando uma exibição de edição*</span><span class="sxs-lookup"><span data-stu-id="9576a-280">*Adding an Edit view*</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="9576a-281">Tarefa 3-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="9576a-281">Task 3 - Running the Application</span></span>

<span data-ttu-id="9576a-282">Nesta tarefa, você testará que a página de exibição de **edição** do **storemanager** exibe os valores de propriedades para o álbum passado como parâmetro.</span><span class="sxs-lookup"><span data-stu-id="9576a-282">In this task, you will test that the **StoreManager** **Edit** View page displays the properties' values for the album passed as parameter.</span></span>

1. <span data-ttu-id="9576a-283">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9576a-283">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9576a-284">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="9576a-284">The project starts in the Home page.</span></span> <span data-ttu-id="9576a-285">Altere a URL para **/StoreManager/Edit/1** para verificar se os valores das propriedades para o álbum passado são exibidos.</span><span class="sxs-lookup"><span data-stu-id="9576a-285">Change the URL to **/StoreManager/Edit/1** to verify that the properties' values for the album passed are displayed.</span></span>

    <span data-ttu-id="9576a-286">![Modo de exibição de edição do álbum de navegação](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Modo de exibição de edição do álbum de navegação")</span><span class="sxs-lookup"><span data-stu-id="9576a-286">![Browsing Album's Edit View](aspnet-mvc-4-helpers-forms-and-validation/_static/image8.png "Browsing Album's Edit View")</span></span>

    <span data-ttu-id="9576a-287">*Modo de exibição de edição do álbum de navegação*</span><span class="sxs-lookup"><span data-stu-id="9576a-287">*Browsing Album's Edit view*</span></span>

<a id="Ex3Task4"></a>

<a id="Task_4_-_Implementing_drop-downs_on_the_Album_Editor_Template"></a>
#### <a name="task-4---implementing-drop-downs-on-the-album-editor-template"></a><span data-ttu-id="9576a-288">Tarefa 4 – implementando os menus suspensos no modelo do editor de álbuns</span><span class="sxs-lookup"><span data-stu-id="9576a-288">Task 4 - Implementing drop-downs on the Album Editor Template</span></span>

<span data-ttu-id="9576a-289">Nesta tarefa, você adicionará os menus suspensos ao modelo de exibição criado na última tarefa, para que o usuário possa selecionar em uma lista de artistas e gêneros.</span><span class="sxs-lookup"><span data-stu-id="9576a-289">In this task, you will add drop-downs to the View template created in the last task, so that the user can select from a list of Artists and Genres.</span></span>

1. <span data-ttu-id="9576a-290">Substitua todo o código do fieldset do **álbum** pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="9576a-290">Replace all the **Album** fieldset code with the following:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample10.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="9576a-291">Um auxiliar **HTML. DropDownList** foi adicionado para os menus suspensos de renderização para escolher artistas e gêneros.</span><span class="sxs-lookup"><span data-stu-id="9576a-291">An **Html.DropDownList** helper has been added to render drop-downs for choosing Artists and Genres.</span></span> <span data-ttu-id="9576a-292">Os parâmetros passados para **HTML. DropDownList** são:</span><span class="sxs-lookup"><span data-stu-id="9576a-292">The parameters passed to **Html.DropDownList** are:</span></span>
    > 
    > 1. <span data-ttu-id="9576a-293">O nome do campo de formulário ( **&quot;artistaid&quot;** ).</span><span class="sxs-lookup"><span data-stu-id="9576a-293">The name of the form field (**&quot;ArtistId&quot;**).</span></span>
    > 2. <span data-ttu-id="9576a-294">A lista de valores de **Select** para o menu suspenso.</span><span class="sxs-lookup"><span data-stu-id="9576a-294">The **SelectList** of values for the drop-down.</span></span>

<a id="Ex3Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="9576a-295">Tarefa 5-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="9576a-295">Task 5 - Running the Application</span></span>

<span data-ttu-id="9576a-296">Nesta tarefa, você testará que a página de exibição de **edição** do **storemanager** exibe menus suspensos em vez dos campos de texto ID do artista e gênero.</span><span class="sxs-lookup"><span data-stu-id="9576a-296">In this task, you will test that the **StoreManager** **Edit** View page displays drop-downs instead of Artist and Genre ID text fields.</span></span>

1. <span data-ttu-id="9576a-297">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9576a-297">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9576a-298">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="9576a-298">The project starts in the Home page.</span></span> <span data-ttu-id="9576a-299">Altere a URL para **/StoreManager/Edit/1** para verificar se ela exibe os menus suspensos em vez dos campos de texto ID do artista e gênero.</span><span class="sxs-lookup"><span data-stu-id="9576a-299">Change the URL to **/StoreManager/Edit/1** to verify that it displays drop-downs instead of Artist and Genre ID text fields.</span></span>

    <span data-ttu-id="9576a-300">![Navegando na exibição de edição do álbum com os menus suspensos](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Navegando na exibição de edição do álbum com os menus suspensos")</span><span class="sxs-lookup"><span data-stu-id="9576a-300">![Browsing Album's Edit View with drop downs](aspnet-mvc-4-helpers-forms-and-validation/_static/image9.png "Browsing Album's Edit View with drop downs")</span></span>

    <span data-ttu-id="9576a-301">*Pesquisando o modo de exibição de álbum, desta vez com listas suspensas*</span><span class="sxs-lookup"><span data-stu-id="9576a-301">*Browsing Album's Edit view, this time with dropdowns*</span></span>

<a id="Ex3Task6"></a>

<a id="Task_6_-_Implementing_the_HTTP-POST_Edit_action_method"></a>
#### <a name="task-6---implementing-the-http-post-edit-action-method"></a><span data-ttu-id="9576a-302">Tarefa 6-implementando o método de ação HTTP-POST Edit</span><span class="sxs-lookup"><span data-stu-id="9576a-302">Task 6 - Implementing the HTTP-POST Edit action method</span></span>

<span data-ttu-id="9576a-303">Agora que o modo de exibição de edição é exibido conforme o esperado, você precisa implementar o método de ação HTTP-POST Edit para salvar as alterações feitas no álbum.</span><span class="sxs-lookup"><span data-stu-id="9576a-303">Now that the Edit View displays as expected, you need to implement the HTTP-POST Edit Action method to save the changes made to the Album.</span></span>

1. <span data-ttu-id="9576a-304">Feche o navegador, se necessário, para retornar à janela do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9576a-304">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="9576a-305">Abra o **StoreManagerController** na pasta **controladores** .</span><span class="sxs-lookup"><span data-stu-id="9576a-305">Open **StoreManagerController** from the **Controllers** folder.</span></span>
2. <span data-ttu-id="9576a-306">Substitua o código do método de ação **http-Post Edit** pelo seguinte (Observe que o método que deve ser substituído é a versão sobrecarregada que recebe dois parâmetros):</span><span class="sxs-lookup"><span data-stu-id="9576a-306">Replace **HTTP-POST Edit** action method code with the following (note that the method that must be replaced is overloaded version that receives two parameters):</span></span>

    <span data-ttu-id="9576a-307">(Trecho de código- *ASP.net e auxiliares MVC 4 e Forms e Validation – EX3 STOREMANAGERCONTROLLER http-Post Edit Action*)</span><span class="sxs-lookup"><span data-stu-id="9576a-307">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex3 StoreManagerController HTTP-POST Edit action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="9576a-308">Esse método será executado quando o usuário clicar no botão **salvar** da exibição e executará um http-post dos valores do formulário de volta para o servidor para mantê-los no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9576a-308">This method will be executed when the user clicks the **Save** button of the View and performs an HTTP-POST of the form values back to the server to persist them in the database.</span></span> <span data-ttu-id="9576a-309">O decorador **[HttpPost]** indica que o método deve ser usado para os cenários http-post.</span><span class="sxs-lookup"><span data-stu-id="9576a-309">The decorator **[HttpPost]** indicates that the method should be used for those HTTP-POST scenarios.</span></span> <span data-ttu-id="9576a-310">O método usa um objeto de **álbum** .</span><span class="sxs-lookup"><span data-stu-id="9576a-310">The method takes an **Album** object.</span></span> <span data-ttu-id="9576a-311">O ASP.NET MVC criará automaticamente o objeto de álbum do formulário &lt;Postado&gt; valores.</span><span class="sxs-lookup"><span data-stu-id="9576a-311">ASP.NET MVC will automatically create the Album object from the posted &lt;form&gt; values.</span></span>
    > 
    > <span data-ttu-id="9576a-312">O método executará estas etapas:</span><span class="sxs-lookup"><span data-stu-id="9576a-312">The method will perform these steps:</span></span>
    > 
    > 1. <span data-ttu-id="9576a-313">Se o modelo for válido:</span><span class="sxs-lookup"><span data-stu-id="9576a-313">If model is valid:</span></span>
    > 
    >     1. <span data-ttu-id="9576a-314">Atualize a entrada do álbum no contexto para marcá-la como um objeto modificado.</span><span class="sxs-lookup"><span data-stu-id="9576a-314">Update the album entry in the context to mark it as a modified object.</span></span>
    >     2. <span data-ttu-id="9576a-315">Salve as alterações e redirecione-as para a exibição de índice.</span><span class="sxs-lookup"><span data-stu-id="9576a-315">Save the changes and redirect to the index view.</span></span>
    > 2. <span data-ttu-id="9576a-316">Se o modelo não for válido, ele preencherá ViewBag com o **gêneroid** e a **artistaid**, retornará a exibição com o objeto de álbum recebido para permitir que o usuário execute qualquer atualização necessária.</span><span class="sxs-lookup"><span data-stu-id="9576a-316">If the model is not valid, it will populate the ViewBag with the **GenreId** and **ArtistId**, then it will return the view with the received Album object to allow the user perform any required update.</span></span>

<a id="Ex3Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="9576a-317">Tarefa 7-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="9576a-317">Task 7 - Running the Application</span></span>

<span data-ttu-id="9576a-318">Nesta tarefa, você testará que a página de exibição de **edição do storemanager** , na verdade, salva os dados de álbum atualizados no banco de dado.</span><span class="sxs-lookup"><span data-stu-id="9576a-318">In this task, you will test that the **StoreManager Edit** View page actually saves the updated Album data in the database.</span></span>

1. <span data-ttu-id="9576a-319">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9576a-319">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9576a-320">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="9576a-320">The project starts in the Home page.</span></span> <span data-ttu-id="9576a-321">Altere a URL para **/StoreManager/Edit/1**.</span><span class="sxs-lookup"><span data-stu-id="9576a-321">Change the URL to **/StoreManager/Edit/1**.</span></span> <span data-ttu-id="9576a-322">Altere o título do álbum para **carregar** e clique em **salvar**.</span><span class="sxs-lookup"><span data-stu-id="9576a-322">Change the Album title to **Load** and click on **Save**.</span></span> <span data-ttu-id="9576a-323">Verifique se o título do álbum realmente mudou na lista de álbuns.</span><span class="sxs-lookup"><span data-stu-id="9576a-323">Verify that album's title actually changed in the list of albums.</span></span>

    <span data-ttu-id="9576a-324">![Atualizando um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Atualizando um álbum")</span><span class="sxs-lookup"><span data-stu-id="9576a-324">![Updating an album](aspnet-mvc-4-helpers-forms-and-validation/_static/image10.png "Updating an album")</span></span>

    <span data-ttu-id="9576a-325">*Atualizando um álbum*</span><span class="sxs-lookup"><span data-stu-id="9576a-325">*Updating an Album*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Adding_a_Create_View"></a>
### <a name="exercise-4-adding-a-create-view"></a><span data-ttu-id="9576a-326">Exercício 4: adicionando um modo de exibição de criação</span><span class="sxs-lookup"><span data-stu-id="9576a-326">Exercise 4: Adding a Create View</span></span>

<span data-ttu-id="9576a-327">Agora que o **StoreManagerController** dá suporte à capacidade de **edição** , neste exercício, você aprenderá a adicionar um modelo Create View para permitir que os gerentes de armazenamento adicionem novos álbuns ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9576a-327">Now that the **StoreManagerController** supports the **Edit** ability, in this exercise you will learn how to add a Create View template to let store managers add new Albums to the application.</span></span>

<span data-ttu-id="9576a-328">Como você fez com a funcionalidade de edição, implementará o cenário de criação usando dois métodos separados dentro da classe **StoreManagerController** :</span><span class="sxs-lookup"><span data-stu-id="9576a-328">Like you did with the Edit functionality, you will implement the Create scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="9576a-329">Um método de ação exibirá um formulário vazio quando os gerentes de loja visitarem primeiro a URL **/StoreManager/Create** .</span><span class="sxs-lookup"><span data-stu-id="9576a-329">One action method will display an empty form when store managers first visit the **/StoreManager/Create** URL.</span></span>
2. <span data-ttu-id="9576a-330">Um segundo método de ação manipulará o cenário em que o Gerenciador de loja clica no botão **salvar** dentro do formulário e envia os valores de volta para a URL **/STOREMANAGER/Create** como um http-post.</span><span class="sxs-lookup"><span data-stu-id="9576a-330">A second action method will handle the scenario where the store manager clicks the **Save** button within the form and submits the values back to the **/StoreManager/Create** URL as an HTTP-POST.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Create_action_method"></a>
#### <a name="task-1---implementing-the-http-get-create-action-method"></a><span data-ttu-id="9576a-331">Tarefa 1-implementando o método de ação de criação HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="9576a-331">Task 1 - Implementing the HTTP-GET Create action method</span></span>

<span data-ttu-id="9576a-332">Nesta tarefa, você implementará a versão HTTP-GET do método criar ação para recuperar uma lista de todos os gêneros e artistas, empacotando esses dados em um objeto **StoreManagerViewModel** , que será então passado para um modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="9576a-332">In this task, you will implement the HTTP-GET version of the Create action method to retrieve a list of all Genres and Artists, package this data up into a **StoreManagerViewModel** object, which will then be passed to a View template.</span></span>

1. <span data-ttu-id="9576a-333">Abra a solução **inicial** localizada na **origem/Ex4-AddingACreateView/início/** pasta.</span><span class="sxs-lookup"><span data-stu-id="9576a-333">Open the **Begin** solution located at **Source/Ex4-AddingACreateView/Begin/** folder.</span></span> <span data-ttu-id="9576a-334">Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="9576a-334">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="9576a-335">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="9576a-335">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9576a-336">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9576a-336">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9576a-337">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="9576a-337">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9576a-338">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="9576a-338">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9576a-339">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="9576a-339">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9576a-340">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="9576a-340">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9576a-341">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="9576a-341">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="9576a-342">Abra a classe **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="9576a-342">Open **StoreManagerController** class.</span></span> <span data-ttu-id="9576a-343">Para fazer isso, expanda a pasta **controladores** e clique duas vezes em **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="9576a-343">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="9576a-344">Substitua o código de método de ação **Create** pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="9576a-344">Replace the **Create** action method code with the following:</span></span>

    <span data-ttu-id="9576a-345">(Trecho de código- *auxiliares do ASP.NET MVC 4 e formulários e validação-Ex4 STOREMANAGERCONTROLLER http-Get criar ação*)</span><span class="sxs-lookup"><span data-stu-id="9576a-345">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP-GET Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample12.cs)]

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_the_Create_View"></a>
#### <a name="task-2---adding-the-create-view"></a><span data-ttu-id="9576a-346">Tarefa 2-adicionando o modo de exibição de criação</span><span class="sxs-lookup"><span data-stu-id="9576a-346">Task 2 - Adding the Create View</span></span>

<span data-ttu-id="9576a-347">Nesta tarefa, você adicionará o modelo criar exibição que exibirá um novo formulário de álbum (vazio).</span><span class="sxs-lookup"><span data-stu-id="9576a-347">In this task, you will add the Create View template that will display a new (empty) Album form.</span></span>

1. <span data-ttu-id="9576a-348">Clique com o botão direito do mouse dentro do método **criar** ação e selecione **Adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="9576a-348">Right-click inside the **Create** action method and select **Add View**.</span></span> <span data-ttu-id="9576a-349">Isso abrirá a caixa de diálogo Adicionar exibição.</span><span class="sxs-lookup"><span data-stu-id="9576a-349">This will bring up the Add View dialog.</span></span>
2. <span data-ttu-id="9576a-350">Na caixa de diálogo Adicionar exibição, verifique se o nome da exibição é **criar**.</span><span class="sxs-lookup"><span data-stu-id="9576a-350">In the Add View dialog, verify that the View Name is **Create**.</span></span> <span data-ttu-id="9576a-351">Selecione a opção **criar uma exibição fortemente tipada** e selecione **álbum (MvcMusicStore. Models)** na lista suspensa **classe de modelo** e **criar** na lista suspensa modelo de **Scaffold** .</span><span class="sxs-lookup"><span data-stu-id="9576a-351">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down and **Create** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="9576a-352">Deixe os outros campos com o valor padrão e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="9576a-352">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="9576a-353">![Adicionando um modo de exibição de criação](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-Create-View. png")</span><span class="sxs-lookup"><span data-stu-id="9576a-353">![Adding a create view](aspnet-mvc-4-helpers-forms-and-validation/_static/image11.png "adding-a-create-view.png")</span></span>

    <span data-ttu-id="9576a-354">*Adicionando o modo de exibição de criação*</span><span class="sxs-lookup"><span data-stu-id="9576a-354">*Adding the Create View*</span></span>
3. <span data-ttu-id="9576a-355">Atualize os campos **gêneroid** e **artistaid** para usar uma lista suspensa, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="9576a-355">Update the **GenreId** and **ArtistId** fields to use a drop-down list as shown below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample13.cshtml)]

<a id="Ex4Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="9576a-356">Tarefa 3-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="9576a-356">Task 3 - Running the Application</span></span>

<span data-ttu-id="9576a-357">Nesta tarefa, você testará que a página **criar** exibição do **storemanager** exibe um formulário de álbum vazio.</span><span class="sxs-lookup"><span data-stu-id="9576a-357">In this task, you will test that the **StoreManager** **Create** View page displays an empty Album form.</span></span>

1. <span data-ttu-id="9576a-358">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9576a-358">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9576a-359">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="9576a-359">The project starts in the Home page.</span></span> <span data-ttu-id="9576a-360">Altere a URL para **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="9576a-360">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="9576a-361">Verifique se um formulário vazio é exibido para preencher as novas propriedades do álbum.</span><span class="sxs-lookup"><span data-stu-id="9576a-361">Verify that an empty form is displayed for filling the new Album properties.</span></span>

    <span data-ttu-id="9576a-362">![Criar modo de exibição com um formulário vazio](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Criar modo de exibição com um formulário vazio")</span><span class="sxs-lookup"><span data-stu-id="9576a-362">![Create View with an empty form](aspnet-mvc-4-helpers-forms-and-validation/_static/image12.png "Create View with an empty form")</span></span>

    <span data-ttu-id="9576a-363">*Criar modo de exibição com um formulário vazio*</span><span class="sxs-lookup"><span data-stu-id="9576a-363">*Create View with an empty form*</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_-_Implementing_the_HTTP-POST_Create_Action_Method"></a>
#### <a name="task-4---implementing-the-http-post-create-action-method"></a><span data-ttu-id="9576a-364">Tarefa 4-implementando o método de ação HTTP-POST Create</span><span class="sxs-lookup"><span data-stu-id="9576a-364">Task 4 - Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="9576a-365">Nesta tarefa, você implementará a versão HTTP-POST do método criar ação que será invocado quando um usuário clicar no botão **salvar** .</span><span class="sxs-lookup"><span data-stu-id="9576a-365">In this task, you will implement the HTTP-POST version of the Create action method that will be invoked when a user clicks the **Save** button.</span></span> <span data-ttu-id="9576a-366">O método deve salvar o novo álbum no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9576a-366">The method should save the new album in the database.</span></span>

1. <span data-ttu-id="9576a-367">Feche o navegador, se necessário, para retornar à janela do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9576a-367">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="9576a-368">Abra a classe **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="9576a-368">Open **StoreManagerController** class.</span></span> <span data-ttu-id="9576a-369">Para fazer isso, expanda a pasta **controladores** e clique duas vezes em **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="9576a-369">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="9576a-370">Substitua o código do método de ação **http-post Create** pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="9576a-370">Replace **HTTP-POST Create** action method code with the following:</span></span>

    <span data-ttu-id="9576a-371">(Trecho de código- *ASP.net e auxiliares MVC 4 e formulários e validação – Ex4 STOREMANAGERCONTROLLER http-post criar ação*)</span><span class="sxs-lookup"><span data-stu-id="9576a-371">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex4 StoreManagerController HTTP- POST Create action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample14.cs)]

    > [!NOTE]
    > <span data-ttu-id="9576a-372">A ação criar é bastante semelhante ao método de ação editar anterior, mas em vez de definir o objeto como modificado, ele está sendo adicionado ao contexto.</span><span class="sxs-lookup"><span data-stu-id="9576a-372">The Create action is pretty similar to the previous Edit action method but instead of setting the object as modified, it is being added to the context.</span></span>

<a id="Ex4Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="9576a-373">Tarefa 5-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="9576a-373">Task 5 - Running the Application</span></span>

<span data-ttu-id="9576a-374">Nesta tarefa, você testará que a página **criar exibição do storemanager** permite criar um novo álbum e, em seguida, redireciona para a exibição do índice storemanager.</span><span class="sxs-lookup"><span data-stu-id="9576a-374">In this task, you will test that the **StoreManager Create** View page lets you create a new Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="9576a-375">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9576a-375">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9576a-376">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="9576a-376">The project starts in the Home page.</span></span> <span data-ttu-id="9576a-377">Altere a URL para **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="9576a-377">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="9576a-378">Preencha todos os campos de formulário com dados para um novo álbum, como aquele na figura a seguir:</span><span class="sxs-lookup"><span data-stu-id="9576a-378">Fill all the form fields with data for a new Album, like the one in the following figure:</span></span>

    <span data-ttu-id="9576a-379">![Criando um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Criando um álbum")</span><span class="sxs-lookup"><span data-stu-id="9576a-379">![Creating an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image13.png "Creating an Album")</span></span>

    <span data-ttu-id="9576a-380">*Criando um álbum*</span><span class="sxs-lookup"><span data-stu-id="9576a-380">*Creating an Album*</span></span>
3. <span data-ttu-id="9576a-381">Verifique se você foi redirecionado para a exibição de índice do Storemanager que inclui o novo álbum recém-criado.</span><span class="sxs-lookup"><span data-stu-id="9576a-381">Verify that you get redirected to the StoreManager Index View that includes the new Album just created.</span></span>

    <span data-ttu-id="9576a-382">![Novo álbum criado](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "Novo álbum criado")</span><span class="sxs-lookup"><span data-stu-id="9576a-382">![New Album Created](aspnet-mvc-4-helpers-forms-and-validation/_static/image14.png "New Album Created")</span></span>

    <span data-ttu-id="9576a-383">*Novo álbum criado*</span><span class="sxs-lookup"><span data-stu-id="9576a-383">*New Album Created*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Handling_Deletion"></a>
### <a name="exercise-5-handling-deletion"></a><span data-ttu-id="9576a-384">Exercício 5: manipulando a exclusão</span><span class="sxs-lookup"><span data-stu-id="9576a-384">Exercise 5: Handling Deletion</span></span>

<span data-ttu-id="9576a-385">A capacidade de excluir álbuns ainda não foi implementada.</span><span class="sxs-lookup"><span data-stu-id="9576a-385">The ability to delete albums is not yet implemented.</span></span> <span data-ttu-id="9576a-386">É isso que se trata deste exercício.</span><span class="sxs-lookup"><span data-stu-id="9576a-386">This is what this exercise will be about.</span></span> <span data-ttu-id="9576a-387">Assim como antes, você implementará o cenário de exclusão usando dois métodos separados dentro da classe **StoreManagerController** :</span><span class="sxs-lookup"><span data-stu-id="9576a-387">Like before, you will implement the Delete scenario using two separate methods within the **StoreManagerController** class:</span></span>

1. <span data-ttu-id="9576a-388">Um método de ação exibirá um formulário de confirmação</span><span class="sxs-lookup"><span data-stu-id="9576a-388">One action method will display a confirmation form</span></span>
2. <span data-ttu-id="9576a-389">Um segundo método de ação manipulará o envio do formulário</span><span class="sxs-lookup"><span data-stu-id="9576a-389">A second action method will handle the form submission</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Implementing_the_HTTP-GET_Delete_Action_Method"></a>
#### <a name="task-1---implementing-the-http-get-delete-action-method"></a><span data-ttu-id="9576a-390">Tarefa 1-implementando o método de ação de exclusão HTTP-GET</span><span class="sxs-lookup"><span data-stu-id="9576a-390">Task 1 - Implementing the HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="9576a-391">Nesta tarefa, você implementará a versão HTTP-GET do método Excluir ação para recuperar as informações do álbum.</span><span class="sxs-lookup"><span data-stu-id="9576a-391">In this task, you will implement the HTTP-GET version of the Delete action method to retrieve the album's information.</span></span>

1. <span data-ttu-id="9576a-392">Abra a solução **inicial** localizada na **origem/Ex5-HandlingDeletion/início/** pasta.</span><span class="sxs-lookup"><span data-stu-id="9576a-392">Open the **Begin** solution located at **Source/Ex5-HandlingDeletion/Begin/** folder.</span></span> <span data-ttu-id="9576a-393">Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="9576a-393">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="9576a-394">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="9576a-394">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9576a-395">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9576a-395">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9576a-396">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="9576a-396">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9576a-397">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="9576a-397">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9576a-398">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="9576a-398">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9576a-399">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="9576a-399">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9576a-400">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="9576a-400">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="9576a-401">Abra a classe **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="9576a-401">Open **StoreManagerController** class.</span></span> <span data-ttu-id="9576a-402">Para fazer isso, expanda a pasta **controladores** e clique duas vezes em **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="9576a-402">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
3. <span data-ttu-id="9576a-403">A ação Excluir controlador é exatamente igual à ação do controlador de detalhes da loja anterior: consulta o objeto de **álbum** do banco de dados usando a **ID** fornecida na URL e retorna a **exibição**apropriada.</span><span class="sxs-lookup"><span data-stu-id="9576a-403">The Delete controller action is exactly the same as the previous Store Details controller action: it queries the **album** object from the database using the **id** provided in the URL and returns the appropriate **View**.</span></span> <span data-ttu-id="9576a-404">Para fazer isso, substitua o código do método de ação de **exclusão** http-Get pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="9576a-404">To do this, replace the HTTP-GET **Delete** action method code with the following:</span></span>

    <span data-ttu-id="9576a-405">(Trecho de código- *auxiliares do ASP.NET MVC 4 e formulários e validação-Ex5 tratamento de exclusão http-obter ação de exclusão*)</span><span class="sxs-lookup"><span data-stu-id="9576a-405">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-GET Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample15.cs)]
4. <span data-ttu-id="9576a-406">Clique com o botão direito do mouse dentro do método de ação **excluir** e selecione **Adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="9576a-406">Right-click inside the **Delete** action method and select **Add View**.</span></span> <span data-ttu-id="9576a-407">Isso abrirá a caixa de diálogo Adicionar exibição.</span><span class="sxs-lookup"><span data-stu-id="9576a-407">This will bring up the Add View dialog.</span></span>
5. <span data-ttu-id="9576a-408">Na caixa de diálogo Adicionar exibição, verifique se o nome da exibição é **excluir**.</span><span class="sxs-lookup"><span data-stu-id="9576a-408">In the Add View dialog, verify that the View name is **Delete**.</span></span> <span data-ttu-id="9576a-409">Selecione a opção **criar uma exibição fortemente tipada** e selecione **álbum (MvcMusicStore. Models)** na lista suspensa **classe de modelo** .</span><span class="sxs-lookup"><span data-stu-id="9576a-409">Select the **Create a strongly-typed view** option and select **Album (MvcMusicStore.Models)** from the **Model class** drop-down.</span></span> <span data-ttu-id="9576a-410">Selecione **excluir** na lista suspensa **modelo de Scaffold** .</span><span class="sxs-lookup"><span data-stu-id="9576a-410">Select **Delete** from the **Scaffold template** drop-down.</span></span> <span data-ttu-id="9576a-411">Deixe os outros campos com o valor padrão e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="9576a-411">Leave the other fields with their default value and then click **Add**.</span></span>

    <span data-ttu-id="9576a-412">![Adicionando uma exibição de exclusão](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adicionando uma exibição de exclusão")</span><span class="sxs-lookup"><span data-stu-id="9576a-412">![Adding a Delete View](aspnet-mvc-4-helpers-forms-and-validation/_static/image15.png "Adding a Delete View")</span></span>

    <span data-ttu-id="9576a-413">*Adicionando uma exibição de exclusão*</span><span class="sxs-lookup"><span data-stu-id="9576a-413">*Adding a Delete View*</span></span>
6. <span data-ttu-id="9576a-414">O modelo de exclusão mostra todos os campos do modelo.</span><span class="sxs-lookup"><span data-stu-id="9576a-414">The Delete template shows all the fields from the model.</span></span> <span data-ttu-id="9576a-415">Você mostrará apenas o título do álbum.</span><span class="sxs-lookup"><span data-stu-id="9576a-415">You will show only the album's title.</span></span> <span data-ttu-id="9576a-416">Para fazer isso, substitua o conteúdo da exibição pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="9576a-416">To do this, replace the content of the view with the following code:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample16.cshtml)]

<a id="Ex05Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="9576a-417">Tarefa 2-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="9576a-417">Task 2 - Running the Application</span></span>

<span data-ttu-id="9576a-418">Nesta tarefa, você testará que a página de exibição **excluir** do **storemanager** exibe um formulário de exclusão de confirmação.</span><span class="sxs-lookup"><span data-stu-id="9576a-418">In this task, you will test that the **StoreManager** **Delete** View page displays a confirmation deletion form.</span></span>

1. <span data-ttu-id="9576a-419">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9576a-419">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9576a-420">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="9576a-420">The project starts in the Home page.</span></span> <span data-ttu-id="9576a-421">Altere a URL para **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="9576a-421">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="9576a-422">Selecione um álbum para excluir clicando em **excluir** e verifique se o novo modo de exibição foi carregado.</span><span class="sxs-lookup"><span data-stu-id="9576a-422">Select one album to delete by clicking **Delete** and verify that the new view is uploaded.</span></span>

    <span data-ttu-id="9576a-423">![Excluindo um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Excluindo um álbum")</span><span class="sxs-lookup"><span data-stu-id="9576a-423">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image16.png "Deleting an Album")</span></span>

    <span data-ttu-id="9576a-424">*Excluindo um álbum*</span><span class="sxs-lookup"><span data-stu-id="9576a-424">*Deleting an Album*</span></span>

<a id="Ex05Task3"></a>

<a id="Task_3-_Implementing_the_HTTP-POST_Delete_Action_Method"></a>
#### <a name="task-3--implementing-the-http-post-delete-action-method"></a><span data-ttu-id="9576a-425">Tarefa 3-implementando o método de ação HTTP-POST Delete</span><span class="sxs-lookup"><span data-stu-id="9576a-425">Task 3- Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="9576a-426">Nesta tarefa, você implementará a versão HTTP-POST do método Excluir ação que será invocado quando um usuário clicar no botão **excluir** .</span><span class="sxs-lookup"><span data-stu-id="9576a-426">In this task, you will implement the HTTP-POST version of the Delete action method that will be invoked when a user clicks the **Delete** button.</span></span> <span data-ttu-id="9576a-427">O método deve excluir o álbum no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9576a-427">The method should delete the album in the database.</span></span>

1. <span data-ttu-id="9576a-428">Feche o navegador, se necessário, para retornar à janela do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9576a-428">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="9576a-429">Abra a classe **StoreManagerController** .</span><span class="sxs-lookup"><span data-stu-id="9576a-429">Open **StoreManagerController** class.</span></span> <span data-ttu-id="9576a-430">Para fazer isso, expanda a pasta **controladores** e clique duas vezes em **StoreManagerController.cs**.</span><span class="sxs-lookup"><span data-stu-id="9576a-430">To do this, expand the **Controllers** folder and double-click **StoreManagerController.cs**.</span></span>
2. <span data-ttu-id="9576a-431">Substitua o código do método de ação **http-post Delete** pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="9576a-431">Replace **HTTP-POST Delete** action method code with the following:</span></span>

    <span data-ttu-id="9576a-432">(Trecho de código- *ASP.net e auxiliares MVC 4 e formulários e validação-Ex5 tratamento exclusão http-post excluir ação*)</span><span class="sxs-lookup"><span data-stu-id="9576a-432">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex5 Handling Deletion HTTP-POST Delete action*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample17.cs)]

<a id="Ex5Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="9576a-433">Tarefa 4-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="9576a-433">Task 4 - Running the Application</span></span>

<span data-ttu-id="9576a-434">Nesta tarefa, você testará que a página de exibição **excluir do storemanager** permite excluir um álbum e, em seguida, redireciona para a exibição de índice storemanager.</span><span class="sxs-lookup"><span data-stu-id="9576a-434">In this task, you will test that the **StoreManager Delete** View page lets you delete an Album and then redirects to the StoreManager Index View.</span></span>

1. <span data-ttu-id="9576a-435">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9576a-435">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9576a-436">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="9576a-436">The project starts in the Home page.</span></span> <span data-ttu-id="9576a-437">Altere a URL para **/StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="9576a-437">Change the URL to **/StoreManager**.</span></span> <span data-ttu-id="9576a-438">Selecione um álbum para excluir clicando em **excluir.**</span><span class="sxs-lookup"><span data-stu-id="9576a-438">Select one album to delete by clicking **Delete.**</span></span> <span data-ttu-id="9576a-439">Confirme a exclusão clicando no botão **excluir** :</span><span class="sxs-lookup"><span data-stu-id="9576a-439">Confirm the deletion by clicking **Delete** button:</span></span>

    <span data-ttu-id="9576a-440">![Excluindo um álbum](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Excluindo um álbum")</span><span class="sxs-lookup"><span data-stu-id="9576a-440">![Deleting an Album](aspnet-mvc-4-helpers-forms-and-validation/_static/image17.png "Deleting an Album")</span></span>

    <span data-ttu-id="9576a-441">*Excluindo um álbum*</span><span class="sxs-lookup"><span data-stu-id="9576a-441">*Deleting an Album*</span></span>
3. <span data-ttu-id="9576a-442">Verifique se o álbum foi excluído, pois ele não aparece na página de **índice** .</span><span class="sxs-lookup"><span data-stu-id="9576a-442">Verify that the album was deleted since it does not appear in the **Index** page.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Adding_Validation"></a>
### <a name="exercise-6-adding-validation"></a><span data-ttu-id="9576a-443">Exercício 6: adicionando validação</span><span class="sxs-lookup"><span data-stu-id="9576a-443">Exercise 6: Adding Validation</span></span>

<span data-ttu-id="9576a-444">Atualmente, os formulários de criação e edição que você tem em vigor não executam nenhum tipo de validação.</span><span class="sxs-lookup"><span data-stu-id="9576a-444">Currently, the Create and Edit forms you have in place do not perform any kind of validation.</span></span> <span data-ttu-id="9576a-445">Se o usuário deixar um campo obrigatório em branco ou digitar letras no campo preço, o primeiro erro será obtido do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9576a-445">If the user leaves a required field blank or type letters in the price field, the first error you will get will be from the database.</span></span>

<span data-ttu-id="9576a-446">Você pode adicionar validação ao aplicativo Adicionando anotações de dados à sua classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="9576a-446">You can add validation to the application by adding Data Annotations to your model class.</span></span> <span data-ttu-id="9576a-447">As anotações de dados permitem descrever as regras que você deseja aplicar às propriedades do modelo e o ASP.NET MVC cuidará da imposição e exibição da mensagem apropriada aos usuários.</span><span class="sxs-lookup"><span data-stu-id="9576a-447">Data Annotations allow describing the rules you want applied to your model properties, and ASP.NET MVC will take care of enforcing and displaying appropriate message to users.</span></span>

<a id="Ex06Task1"></a>

<a id="Task_1_-_Adding_Data_Annotations"></a>
#### <a name="task-1---adding-data-annotations"></a><span data-ttu-id="9576a-448">Tarefa 1-Adicionando anotações de dados</span><span class="sxs-lookup"><span data-stu-id="9576a-448">Task 1 - Adding Data Annotations</span></span>

<span data-ttu-id="9576a-449">Nesta tarefa, você adicionará anotações de dados ao modelo de álbum que fará com que a página criar e editar exiba mensagens de validação quando apropriado.</span><span class="sxs-lookup"><span data-stu-id="9576a-449">In this task, you will add Data Annotations to the Album Model that will make the Create and Edit page display validation messages when appropriate.</span></span>

<span data-ttu-id="9576a-450">Para uma classe de modelo simples, a adição de uma anotação de dados é tratada apenas com a adição de uma instrução **using** para **System. ComponentModel. DataAnnotation**e, em seguida, a colocação de um atributo **[Required]** nas propriedades apropriadas.</span><span class="sxs-lookup"><span data-stu-id="9576a-450">For a simple Model class, adding a Data Annotation is just handled by adding a **using** statement for **System.ComponentModel.DataAnnotation**, then placing a **[Required]** attribute on the appropriate properties.</span></span> <span data-ttu-id="9576a-451">O exemplo a seguir tornaria a propriedade **Name** um campo obrigatório na exibição.</span><span class="sxs-lookup"><span data-stu-id="9576a-451">The following example would make the **Name** property a required field in the View.</span></span>

[!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample18.cs)]

<span data-ttu-id="9576a-452">Isso é um pouco mais complexo em casos como esse aplicativo onde o Modelo de Dados de Entidade é gerado.</span><span class="sxs-lookup"><span data-stu-id="9576a-452">This is a little more complex in cases like this application where the Entity Data Model is generated.</span></span> <span data-ttu-id="9576a-453">Se você adicionou anotações de dados diretamente às classes de modelo, elas serão substituídas se você atualizar o modelo do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9576a-453">If you added Data Annotations directly to the model classes, they would be overwritten if you update the model from the database.</span></span> <span data-ttu-id="9576a-454">Em vez disso, você pode fazer uso de classes parciais de metadados que existirão para manter as anotações e estão associadas às classes de modelo usando o atributo **[MetadataType]** .</span><span class="sxs-lookup"><span data-stu-id="9576a-454">Instead, you can make use of metadata partial classes which will exist to hold the annotations and are associated with the model classes using the **[MetadataType]** attribute.</span></span>

1. <span data-ttu-id="9576a-455">Abra a solução **inicial** localizada na **origem/Ex6-AddingValidation/início/** pasta.</span><span class="sxs-lookup"><span data-stu-id="9576a-455">Open the **Begin** solution located at **Source/Ex6-AddingValidation/Begin/** folder.</span></span> <span data-ttu-id="9576a-456">Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="9576a-456">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="9576a-457">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="9576a-457">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9576a-458">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9576a-458">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9576a-459">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="9576a-459">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9576a-460">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="9576a-460">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9576a-461">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="9576a-461">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9576a-462">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="9576a-462">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9576a-463">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="9576a-463">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="9576a-464">Abra o **Album.cs** na pasta **modelos** .</span><span class="sxs-lookup"><span data-stu-id="9576a-464">Open the **Album.cs** from the **Models** folder.</span></span>
3. <span data-ttu-id="9576a-465">Substitua o conteúdo do **Album.cs** pelo código realçado, para que ele seja semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="9576a-465">Replace **Album.cs** content with the highlighted code, so that it looks like the following:</span></span>

    > [!NOTE]
    > <span data-ttu-id="9576a-466">A linha **[DisplayFormat (ConvertEmptyStringToNull = false)]** indica que as cadeias de caracteres vazias do modelo não serão convertidas em NULL quando o campo de dados for atualizado na fonte de dados.</span><span class="sxs-lookup"><span data-stu-id="9576a-466">The line **[DisplayFormat(ConvertEmptyStringToNull=false)]** indicates that empty strings from the model won't be converted to null when the data field is updated in the data source.</span></span> <span data-ttu-id="9576a-467">Essa configuração evitará uma exceção quando o Entity Framework atribuir valores nulos ao modelo antes de a anotação de dados validar os campos.</span><span class="sxs-lookup"><span data-stu-id="9576a-467">This setting will avoid an exception when the Entity Framework assigns null values to the model before Data Annotation validates the fields.</span></span>

    <span data-ttu-id="9576a-468">(Trecho de código- *ASP.net e auxiliares MVC 4 e formulários e validação – classe parcial de metadados de álbum Ex6*)</span><span class="sxs-lookup"><span data-stu-id="9576a-468">(Code Snippet - *ASP.NET MVC 4 Helpers and Forms and Validation - Ex6 Album metadata partial class*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="9576a-469">Essa classe parcial do **álbum** tem um atributo **MetadataType** que aponta para a classe **AlbumMetaData** para as anotações de dados.</span><span class="sxs-lookup"><span data-stu-id="9576a-469">This **Album** partial class has a **MetadataType** attribute which points to the **AlbumMetaData** class for the Data Annotations.</span></span> <span data-ttu-id="9576a-470">Estes são alguns dos atributos de anotação de dados que você está usando para anotar o modelo de álbum:</span><span class="sxs-lookup"><span data-stu-id="9576a-470">These are some of the Data Annotation attributes you are using to annotate the Album model:</span></span>
    > 
    > - <span data-ttu-id="9576a-471">Obrigatório-indica que a propriedade é um campo obrigatório</span><span class="sxs-lookup"><span data-stu-id="9576a-471">Required - Indicates that the property is a required field</span></span>
    > - <span data-ttu-id="9576a-472">DisplayName-define o texto a ser usado em campos de formulário e mensagens de validação</span><span class="sxs-lookup"><span data-stu-id="9576a-472">DisplayName - Defines the text to be used on form fields and validation messages</span></span>
    > - <span data-ttu-id="9576a-473">DisplayFormat-especifica como os campos de dados são exibidos e formatados.</span><span class="sxs-lookup"><span data-stu-id="9576a-473">DisplayFormat - Specifies how data fields are displayed and formatted.</span></span>
    > - <span data-ttu-id="9576a-474">StringLength-define um comprimento máximo para um campo de cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="9576a-474">StringLength - Defines a maximum length for a string field</span></span>
    > - <span data-ttu-id="9576a-475">Range-fornece um valor máximo e mínimo para um campo numérico</span><span class="sxs-lookup"><span data-stu-id="9576a-475">Range - Gives a maximum and minimum value for a numeric field</span></span>
    > - <span data-ttu-id="9576a-476">ScaffoldColumn-permite ocultar campos de formulários do editor</span><span class="sxs-lookup"><span data-stu-id="9576a-476">ScaffoldColumn - Allows hiding fields from editor forms</span></span>

<a id="Ex06Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="9576a-477">Tarefa 2-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="9576a-477">Task 2 - Running the Application</span></span>

<span data-ttu-id="9576a-478">Nesta tarefa, você testará que a página criar e editar páginas validam os campos, usando os nomes de exibição escolhidos na última tarefa.</span><span class="sxs-lookup"><span data-stu-id="9576a-478">In this task, you will test that the Create and Edit pages validate fields, using the display names chosen in the last task.</span></span>

1. <span data-ttu-id="9576a-479">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9576a-479">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9576a-480">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="9576a-480">The project starts in the Home page.</span></span> <span data-ttu-id="9576a-481">Altere a URL para **/StoreManager/Create**.</span><span class="sxs-lookup"><span data-stu-id="9576a-481">Change the URL to **/StoreManager/Create**.</span></span> <span data-ttu-id="9576a-482">Verifique se os nomes de exibição correspondem àqueles na classe Partial (como a **URL de arte do álbum** em vez de **AlbumArtUrl**)</span><span class="sxs-lookup"><span data-stu-id="9576a-482">Verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**)</span></span>
3. <span data-ttu-id="9576a-483">Clique em **criar**, sem preencher o formulário.</span><span class="sxs-lookup"><span data-stu-id="9576a-483">Click **Create**, without filling the form.</span></span> <span data-ttu-id="9576a-484">Verifique se você obtém as mensagens de validação correspondentes.</span><span class="sxs-lookup"><span data-stu-id="9576a-484">Verify that you get the corresponding validation messages.</span></span>

    <span data-ttu-id="9576a-485">![Campos validados na página criar](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Campos validados na página criar")</span><span class="sxs-lookup"><span data-stu-id="9576a-485">![Validated fields in the Create page](aspnet-mvc-4-helpers-forms-and-validation/_static/image18.png "Validated fields in the Create page")</span></span>

    <span data-ttu-id="9576a-486">*Campos validados na página criar*</span><span class="sxs-lookup"><span data-stu-id="9576a-486">*Validated fields in the Create page*</span></span>
4. <span data-ttu-id="9576a-487">Você pode verificar se o mesmo ocorre com a página **Editar** .</span><span class="sxs-lookup"><span data-stu-id="9576a-487">You can verify that the same occurs with the **Edit** page.</span></span> <span data-ttu-id="9576a-488">Altere a URL para **/StoreManager/Edit/1** e verifique se os nomes de exibição correspondem àqueles na classe Partial (como a **URL de arte do álbum** em vez de **AlbumArtUrl**).</span><span class="sxs-lookup"><span data-stu-id="9576a-488">Change the URL to **/StoreManager/Edit/1** and verify that the display names match the ones in the partial class (like **Album Art URL** instead of **AlbumArtUrl**).</span></span> <span data-ttu-id="9576a-489">Esvazie os campos **título** e **Preço** e clique em **salvar**.</span><span class="sxs-lookup"><span data-stu-id="9576a-489">Empty the **Title** and **Price** fields and click **Save**.</span></span> <span data-ttu-id="9576a-490">Verifique se você obtém as mensagens de validação correspondentes.</span><span class="sxs-lookup"><span data-stu-id="9576a-490">Verify that you get the corresponding validation messages.</span></span>

    ![Campos validados na página Editar](aspnet-mvc-4-helpers-forms-and-validation/_static/image19.png)

    <span data-ttu-id="9576a-492">*Campos validados na página Editar*</span><span class="sxs-lookup"><span data-stu-id="9576a-492">*Validated fields in the Edit page*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_Using_Unobtrusive_jQuery_at_Client_Side"></a>
### <a name="exercise-7-using-unobtrusive-jquery-at-client-side"></a><span data-ttu-id="9576a-493">Exercício 7: usando o jQuery discreto no lado do cliente</span><span class="sxs-lookup"><span data-stu-id="9576a-493">Exercise 7: Using Unobtrusive jQuery at Client Side</span></span>

<span data-ttu-id="9576a-494">Neste exercício, você aprenderá a habilitar a validação de jQuery do MVC 4 discreta no lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="9576a-494">In this exercise, you will learn how to enable MVC 4 Unobtrusive jQuery validation at client side.</span></span>

> [!NOTE]
> <span data-ttu-id="9576a-495">O jQuery não intrusivo usa JavaScript de prefixo de AJAX de dados para invocar métodos de ação no servidor em vez de emitir scripts de cliente embutidos de forma invasiva.</span><span class="sxs-lookup"><span data-stu-id="9576a-495">The Unobtrusive jQuery uses data-ajax prefix JavaScript to invoke action methods on the server rather than intrusively emitting inline client scripts.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_-_Running_the_Application_before_Enabling_Unobtrusive_jQuery"></a>
#### <a name="task-1---running-the-application-before-enabling-unobtrusive-jquery"></a><span data-ttu-id="9576a-496">Tarefa 1-executando o aplicativo antes de habilitar o jQuery discreto</span><span class="sxs-lookup"><span data-stu-id="9576a-496">Task 1 - Running the Application before Enabling Unobtrusive jQuery</span></span>

<span data-ttu-id="9576a-497">Nesta tarefa, você executará o aplicativo antes de incluir o jQuery para comparar os dois modelos de validação.</span><span class="sxs-lookup"><span data-stu-id="9576a-497">In this task, you will run the application before including jQuery in order to compare both validation models.</span></span>

1. <span data-ttu-id="9576a-498">Abra a solução **inicial** localizada na **origem/Ex7-UnobtrusivejQueryValidation/início/** pasta.</span><span class="sxs-lookup"><span data-stu-id="9576a-498">Open the **Begin** solution located at **Source/Ex7-UnobtrusivejQueryValidation/Begin/** folder.</span></span> <span data-ttu-id="9576a-499">Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="9576a-499">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="9576a-500">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="9576a-500">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9576a-501">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="9576a-501">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9576a-502">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="9576a-502">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9576a-503">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="9576a-503">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9576a-504">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="9576a-504">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9576a-505">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="9576a-505">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9576a-506">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="9576a-506">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="9576a-507">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9576a-507">Press **F5** to run the application.</span></span>
3. <span data-ttu-id="9576a-508">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="9576a-508">The project starts in the Home page.</span></span> <span data-ttu-id="9576a-509">Procure **/StoreManager/Create** e clique em **criar** sem preencher o formulário para verificar se você obtém as mensagens de validação:</span><span class="sxs-lookup"><span data-stu-id="9576a-509">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="9576a-510">![Validação do cliente desabilitada](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Validação do cliente desabilitada")</span><span class="sxs-lookup"><span data-stu-id="9576a-510">![Client validation disabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image20.png "Client validation disabled")</span></span>

    <span data-ttu-id="9576a-511">*Validação do cliente desabilitada*</span><span class="sxs-lookup"><span data-stu-id="9576a-511">*Client validation disabled*</span></span>
4. <span data-ttu-id="9576a-512">No navegador, abra o código-fonte HTML:</span><span class="sxs-lookup"><span data-stu-id="9576a-512">In the browser, open the HTML source code:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample20.html)]

<a id="Ex7Task2"></a>

<a id="Task_2_-_Enabling_Unobtrusive_Client_Validation"></a>
#### <a name="task-2---enabling-unobtrusive-client-validation"></a><span data-ttu-id="9576a-513">Tarefa 2-habilitando a validação de cliente discreta</span><span class="sxs-lookup"><span data-stu-id="9576a-513">Task 2 - Enabling Unobtrusive Client Validation</span></span>

<span data-ttu-id="9576a-514">Nesta tarefa, você habilitará a **validação de cliente** do jQuery discreta do arquivo **Web. config** , que é definido por padrão como false em todos os novos projetos do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="9576a-514">In this task, you will enable jQuery **unobtrusive client validation** from **Web.config** file, which is by default set to false in all new ASP.NET MVC 4 projects.</span></span> <span data-ttu-id="9576a-515">Além disso, você adicionará as referências de scripts necessárias para tornar o jQuery não invasivo o trabalho de validação do cliente.</span><span class="sxs-lookup"><span data-stu-id="9576a-515">Additionally, you will add the necessary scripts references to make jQuery Unobtrusive Client Validation work.</span></span>

1. <span data-ttu-id="9576a-516">Abra o arquivo **Web. config** na raiz do projeto e verifique se os valores das chaves **ClientValidationEnabled** e **UnobtrusiveJavaScriptEnabled** estão definidos como **true**.</span><span class="sxs-lookup"><span data-stu-id="9576a-516">Open **Web.Config** file at project root, and make sure that the **ClientValidationEnabled** and **UnobtrusiveJavaScriptEnabled** keys values are set to **true**.</span></span>

    [!code-xml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample21.xml)]

    > [!NOTE]
    > <span data-ttu-id="9576a-517">Você também pode habilitar a validação do cliente pelo código em Global.asax.cs para obter os mesmos resultados:</span><span class="sxs-lookup"><span data-stu-id="9576a-517">You can also enable client validation by code at Global.asax.cs to get the same results:</span></span>
    > 
    > <span data-ttu-id="9576a-518">**HtmlHelper. ClientValidationEnabled = true;**</span><span class="sxs-lookup"><span data-stu-id="9576a-518">**HtmlHelper.ClientValidationEnabled = true;**</span></span>
    > 
    > <span data-ttu-id="9576a-519">Além disso, você pode atribuir o atributo ClientValidationEnabled a qualquer controlador para ter um comportamento personalizado.</span><span class="sxs-lookup"><span data-stu-id="9576a-519">Additionally, you can assign ClientValidationEnabled attribute into any controller to have a custom behavior.</span></span>
2. <span data-ttu-id="9576a-520">Abra **Create. cshtml** em **Views\StoreManager**.</span><span class="sxs-lookup"><span data-stu-id="9576a-520">Open **Create.cshtml** at **Views\StoreManager**.</span></span>
3. <span data-ttu-id="9576a-521">Verifique se os arquivos de script a seguir, **jQuery. Validate** e **jQuery. Validate. nondiscretos**, são referenciados na exibição por meio do pacote de&quot; &quot; **~/Bundles/jqueryval** .</span><span class="sxs-lookup"><span data-stu-id="9576a-521">Make sure the following script files, **jquery.validate** and **jquery.validate.unobtrusive**, are referenced in the view through the &quot;**~/bundles/jqueryval**&quot; bundle.</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample22.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="9576a-522">Todas essas bibliotecas jQuery estão incluídas nos novos projetos do MVC 4.</span><span class="sxs-lookup"><span data-stu-id="9576a-522">All these jQuery libraries are included in MVC 4 new projects.</span></span> <span data-ttu-id="9576a-523">Você pode encontrar mais bibliotecas na pasta **/scripts** do projeto.</span><span class="sxs-lookup"><span data-stu-id="9576a-523">You can find more libraries in the **/Scripts** folder of you project.</span></span>
    > 
    > <span data-ttu-id="9576a-524">Para fazer com que essas bibliotecas de validação funcionem, você precisa adicionar uma referência à biblioteca do jQuery Framework.</span><span class="sxs-lookup"><span data-stu-id="9576a-524">In order to make this validation libraries work, you need to add a reference to the jQuery framework library.</span></span> <span data-ttu-id="9576a-525">Como essa referência já foi adicionada no arquivo **layout. cshtml de\_** , você não precisa adicioná-la neste modo de exibição específico.</span><span class="sxs-lookup"><span data-stu-id="9576a-525">Since this reference is already added in the **\_Layout.cshtml** file, you do not need to add it in this particular view.</span></span>

<a id="Ex7Task3"></a>

<a id="Task_3_-_Running_the_Application_Using_Unobtrusive_jQuery_Validation"></a>
#### <a name="task-3---running-the-application-using-unobtrusive-jquery-validation"></a><span data-ttu-id="9576a-526">Tarefa 3 – executando o aplicativo usando a validação não invasiva do jQuery</span><span class="sxs-lookup"><span data-stu-id="9576a-526">Task 3 - Running the Application Using Unobtrusive jQuery Validation</span></span>

<span data-ttu-id="9576a-527">Nesta tarefa, você testará que o modelo de exibição de criação do **storemanager** executa a validação do lado do cliente usando bibliotecas jQuery quando o usuário cria um novo álbum.</span><span class="sxs-lookup"><span data-stu-id="9576a-527">In this task, you will test that the **StoreManager** create view template performs client side validation using jQuery libraries when the user creates a new album.</span></span>

1. <span data-ttu-id="9576a-528">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9576a-528">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="9576a-529">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="9576a-529">The project starts in the Home page.</span></span> <span data-ttu-id="9576a-530">Procure **/StoreManager/Create** e clique em **criar** sem preencher o formulário para verificar se você obtém as mensagens de validação:</span><span class="sxs-lookup"><span data-stu-id="9576a-530">Browse **/StoreManager/Create** and click **Create** without filling the form to verify that you get validation messages:</span></span>

    <span data-ttu-id="9576a-531">![Validação de cliente com jQuery habilitada](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Validação de cliente com jQuery habilitada")</span><span class="sxs-lookup"><span data-stu-id="9576a-531">![Client validation with jQuery enabled](aspnet-mvc-4-helpers-forms-and-validation/_static/image21.png "Client validation with jQuery enabled")</span></span>

    <span data-ttu-id="9576a-532">*Validação de cliente com jQuery habilitada*</span><span class="sxs-lookup"><span data-stu-id="9576a-532">*Client validation with jQuery enabled*</span></span>
3. <span data-ttu-id="9576a-533">No navegador, abra o código-fonte para criar modo de exibição:</span><span class="sxs-lookup"><span data-stu-id="9576a-533">In the browser, open the source code for Create view:</span></span>

    [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample23.html)]

   > [!NOTE]
   > <span data-ttu-id="9576a-534">Para cada regra de validação de cliente, o jQuery discreto adiciona um atributo com data-Val-*rulename*=&quot;*mensagem*&quot;.</span><span class="sxs-lookup"><span data-stu-id="9576a-534">For each client validation rule, Unobtrusive jQuery adds an attribute with data-val-*rulename*=&quot;*message*&quot;.</span></span> <span data-ttu-id="9576a-535">Abaixo está uma lista de marcas que o jQuery discreto insere no campo de entrada HTML para executar a validação do cliente:</span><span class="sxs-lookup"><span data-stu-id="9576a-535">Below is a list of tags that Unobtrusive jQuery inserts into the html input field to perform client validation:</span></span>
   > 
   > - <span data-ttu-id="9576a-536">Valor de dados</span><span class="sxs-lookup"><span data-stu-id="9576a-536">Data-val</span></span>
   > - <span data-ttu-id="9576a-537">Data-val-número</span><span class="sxs-lookup"><span data-stu-id="9576a-537">Data-val-number</span></span>
   > - <span data-ttu-id="9576a-538">Data-Val-intervalo</span><span class="sxs-lookup"><span data-stu-id="9576a-538">Data-val-range</span></span>
   > - <span data-ttu-id="9576a-539">Data-Val-Range-min/Data-Val-Range-Max</span><span class="sxs-lookup"><span data-stu-id="9576a-539">Data-val-range-min / Data-val-range-max</span></span>
   > - <span data-ttu-id="9576a-540">Data-Val-obrigatório</span><span class="sxs-lookup"><span data-stu-id="9576a-540">Data-val-required</span></span>
   > - <span data-ttu-id="9576a-541">Data-Val-Length</span><span class="sxs-lookup"><span data-stu-id="9576a-541">Data-val-length</span></span>
   > - <span data-ttu-id="9576a-542">Data-Val-Length-Max/data-Val-Length-min</span><span class="sxs-lookup"><span data-stu-id="9576a-542">Data-val-length-max / Data-val-length-min</span></span>
   > 
   > <span data-ttu-id="9576a-543">Todos os valores de dados são preenchidos com a **anotação de dados**de modelo.</span><span class="sxs-lookup"><span data-stu-id="9576a-543">All the data values are filled with model **Data Annotation**.</span></span> <span data-ttu-id="9576a-544">Em seguida, toda a lógica que funciona no lado do servidor pode ser executada no lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="9576a-544">Then, all the logic that works at server side can be run at client side.</span></span> <span data-ttu-id="9576a-545">Por exemplo, o atributo de preço tem a seguinte anotação de dados no modelo:</span><span class="sxs-lookup"><span data-stu-id="9576a-545">For example, Price attribute has the following data annotation in the model:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample24.cs)]
   > 
   > <span data-ttu-id="9576a-546">Depois de usar o jQuery discreto, o código gerado é:</span><span class="sxs-lookup"><span data-stu-id="9576a-546">After using Unobtrusive jQuery, the generated code is:</span></span>
   > 
   > [!code-html[Main](aspnet-mvc-4-helpers-forms-and-validation/samples/sample25.html)]

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="9576a-547">Resumo</span><span class="sxs-lookup"><span data-stu-id="9576a-547">Summary</span></span>

<span data-ttu-id="9576a-548">Ao concluir este laboratório prático, você aprendeu a permitir que os usuários alterem os dados armazenados no banco de dados com o uso do seguinte:</span><span class="sxs-lookup"><span data-stu-id="9576a-548">By completing this Hands-On Lab you have learned how to enable users to change the data stored in the database with the use of the following:</span></span>

- <span data-ttu-id="9576a-549">Ações do controlador, como índice, criar, editar, excluir</span><span class="sxs-lookup"><span data-stu-id="9576a-549">Controller actions like Index, Create, Edit, Delete</span></span>
- <span data-ttu-id="9576a-550">Recurso scaffolding do ASP.NET MVC para exibir propriedades em uma tabela HTML</span><span class="sxs-lookup"><span data-stu-id="9576a-550">ASP.NET MVC's scaffolding feature for displaying properties in an HTML table</span></span>
- <span data-ttu-id="9576a-551">Auxiliares HTML personalizados para melhorar a experiência do usuário</span><span class="sxs-lookup"><span data-stu-id="9576a-551">Custom HTML helpers to improve user experience</span></span>
- <span data-ttu-id="9576a-552">Métodos de ação que reagem às chamadas HTTP-GET ou HTTP-POST</span><span class="sxs-lookup"><span data-stu-id="9576a-552">Action methods that react to either HTTP-GET or HTTP-POST calls</span></span>
- <span data-ttu-id="9576a-553">Um modelo de editor compartilhado para modelos de exibição semelhantes, como criar e editar</span><span class="sxs-lookup"><span data-stu-id="9576a-553">A shared editor template for similar View templates like Create and Edit</span></span>
- <span data-ttu-id="9576a-554">Elementos de formulário como menus suspensos</span><span class="sxs-lookup"><span data-stu-id="9576a-554">Form elements like drop-downs</span></span>
- <span data-ttu-id="9576a-555">Anotações de dados para validação de modelo</span><span class="sxs-lookup"><span data-stu-id="9576a-555">Data annotations for Model validation</span></span>
- <span data-ttu-id="9576a-556">Validação do lado do cliente usando a biblioteca discreta do jQuery</span><span class="sxs-lookup"><span data-stu-id="9576a-556">Client Side Validation using jQuery Unobtrusive library</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="9576a-557">Apêndice A: Instalando o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="9576a-557">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="9576a-558">Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou outra versão &quot;Express&quot; usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="9576a-558">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="9576a-559">As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="9576a-559">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="9576a-560">Vá para [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="9576a-560">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="9576a-561">Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e pesquisar o produto &quot;<em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="9576a-561">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="9576a-562">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="9576a-562">Click on **Install Now**.</span></span> <span data-ttu-id="9576a-563">Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="9576a-563">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="9576a-564">Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="9576a-564">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="9576a-565">![Instalar Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="9576a-565">![Install Visual Studio Express](aspnet-mvc-4-helpers-forms-and-validation/_static/image22.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="9576a-566">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="9576a-566">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="9576a-567">Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="9576a-567">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitando os termos de licença](aspnet-mvc-4-helpers-forms-and-validation/_static/image23.png)

    <span data-ttu-id="9576a-569">*Aceitando os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="9576a-569">*Accepting the license terms*</span></span>
5. <span data-ttu-id="9576a-570">Aguarde até que o processo de download e instalação seja concluído.</span><span class="sxs-lookup"><span data-stu-id="9576a-570">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](aspnet-mvc-4-helpers-forms-and-validation/_static/image24.png)

    <span data-ttu-id="9576a-572">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="9576a-572">*Installation progress*</span></span>
6. <span data-ttu-id="9576a-573">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="9576a-573">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](aspnet-mvc-4-helpers-forms-and-validation/_static/image25.png)

    <span data-ttu-id="9576a-575">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="9576a-575">*Installation completed*</span></span>
7. <span data-ttu-id="9576a-576">Clique em **sair** para fechar Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="9576a-576">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="9576a-577">Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot;**vs Express**&quot;e, em seguida, clique no bloco **vs Express para Web** .</span><span class="sxs-lookup"><span data-stu-id="9576a-577">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Bloco VS Express para Web](aspnet-mvc-4-helpers-forms-and-validation/_static/image26.png)

    <span data-ttu-id="9576a-579">*Bloco VS Express para Web*</span><span class="sxs-lookup"><span data-stu-id="9576a-579">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a><span data-ttu-id="9576a-580">Apêndice B: usando trechos de código</span><span class="sxs-lookup"><span data-stu-id="9576a-580">Appendix B: Using Code Snippets</span></span>

<span data-ttu-id="9576a-581">Com trechos de código, você tem todo o código de que precisa ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="9576a-581">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="9576a-582">O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="9576a-582">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="9576a-583">![Usando trechos de código do Visual Studio para inserir código em seu projeto](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="9576a-583">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-helpers-forms-and-validation/_static/image27.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="9576a-584">*Usando trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="9576a-584">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="9576a-585">***Para adicionar um trecho de código usando o tecladoC# (somente)***</span><span class="sxs-lookup"><span data-stu-id="9576a-585">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="9576a-586">Coloque o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="9576a-586">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="9576a-587">Comece digitando o nome do trecho de código (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="9576a-587">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="9576a-588">Observe como o IntelliSense exibe nomes de trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="9576a-588">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="9576a-589">Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).</span><span class="sxs-lookup"><span data-stu-id="9576a-589">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="9576a-590">Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="9576a-590">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="9576a-591">![Comece a digitar o nome do trecho](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Comece a digitar o nome do trecho")</span><span class="sxs-lookup"><span data-stu-id="9576a-591">![Start typing the snippet name](aspnet-mvc-4-helpers-forms-and-validation/_static/image28.png "Start typing the snippet name")</span></span>

<span data-ttu-id="9576a-592">*Comece a digitar o nome do trecho*</span><span class="sxs-lookup"><span data-stu-id="9576a-592">*Start typing the snippet name*</span></span>

<span data-ttu-id="9576a-593">![Pressione Tab para selecionar o trecho realçado](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Pressione Tab para selecionar o trecho realçado")</span><span class="sxs-lookup"><span data-stu-id="9576a-593">![Press Tab to select the highlighted snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image29.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="9576a-594">*Pressione Tab para selecionar o trecho realçado*</span><span class="sxs-lookup"><span data-stu-id="9576a-594">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="9576a-595">![Pressione Tab novamente e o trecho será expandido](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Pressione Tab novamente e o trecho será expandido")</span><span class="sxs-lookup"><span data-stu-id="9576a-595">![Press Tab again and the snippet will expand](aspnet-mvc-4-helpers-forms-and-validation/_static/image30.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="9576a-596">*Pressione Tab novamente e o trecho será expandido*</span><span class="sxs-lookup"><span data-stu-id="9576a-596">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="9576a-597">***Para adicionar um trecho de código usando o mouseC#(, Visual Basic e XML)*** uma.</span><span class="sxs-lookup"><span data-stu-id="9576a-597">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="9576a-598">Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="9576a-598">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="9576a-599">Selecione **Inserir trecho** seguido por **meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="9576a-599">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="9576a-600">Selecione o trecho relevante na lista clicando nele.</span><span class="sxs-lookup"><span data-stu-id="9576a-600">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="9576a-601">![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")</span><span class="sxs-lookup"><span data-stu-id="9576a-601">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-helpers-forms-and-validation/_static/image31.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="9576a-602">*Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*</span><span class="sxs-lookup"><span data-stu-id="9576a-602">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="9576a-603">![Selecione o trecho relevante na lista clicando nele](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Selecione o trecho relevante na lista clicando nele")</span><span class="sxs-lookup"><span data-stu-id="9576a-603">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-helpers-forms-and-validation/_static/image32.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="9576a-604">*Selecione o trecho relevante na lista clicando nele*</span><span class="sxs-lookup"><span data-stu-id="9576a-604">*Pick the relevant snippet from the list, by clicking on it*</span></span>
