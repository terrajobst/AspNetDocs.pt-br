---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Conceitos básicos do ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Este laboratório prático baseia-se na loja de música MVC (Model View Controller), um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MV...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 95e9b9f55b2080c0ed01dc34e3a32f9f1c905644
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598815"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="b256e-103">Conceitos básicos do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="b256e-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="b256e-104">por [equipe de acampamentos da Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="b256e-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="b256e-105">Baixe o kit de treinamento do Web acampamentos</span><span class="sxs-lookup"><span data-stu-id="b256e-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="b256e-106">Este laboratório prático baseia-se na loja de música MVC (Model View Controller), um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b256e-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="b256e-107">Em todo o laboratório, você aprenderá a simplicidade, mas também a poder de usar essas tecnologias juntas.</span><span class="sxs-lookup"><span data-stu-id="b256e-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="b256e-108">Você começará com um aplicativo simples e o criará até ter um aplicativo Web ASP.NET MVC 4 totalmente funcional.</span><span class="sxs-lookup"><span data-stu-id="b256e-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="b256e-109">Este laboratório funciona com o ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b256e-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="b256e-110">Se você quiser explorar a versão do ASP.NET MVC 3 do aplicativo tutorial, poderá encontrá-la no [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="b256e-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="b256e-111">Este laboratório prático pressupõe que o desenvolvedor tenha experiência em tecnologias de desenvolvimento para a Web, como HTML e JavaScript.</span><span class="sxs-lookup"><span data-stu-id="b256e-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="b256e-112">Todos os códigos de exemplo e trechos de código estão incluídos no Web acampamentos Training Kit, disponível nas [versões Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="b256e-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="b256e-113">O projeto específico deste laboratório está disponível nos [conceitos básicos do ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="b256e-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="b256e-114">O aplicativo da loja de música</span><span class="sxs-lookup"><span data-stu-id="b256e-114">The Music Store application</span></span>

<span data-ttu-id="b256e-115">O aplicativo Web da loja de música que será criado em todo este laboratório é composto por três partes principais: compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="b256e-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="b256e-116">Os visitantes poderão procurar álbuns por gênero, adicionar álbuns ao seu carrinho, revisar sua seleção e, por fim, continuar a fazer o check-out e concluir o pedido.</span><span class="sxs-lookup"><span data-stu-id="b256e-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="b256e-117">Além disso, os administradores da loja poderão gerenciar os álbuns disponíveis, bem como suas propriedades principais.</span><span class="sxs-lookup"><span data-stu-id="b256e-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="b256e-118">![Telas da loja de música](aspnet-mvc-4-fundamentals/_static/image1.png "Telas da loja de música")</span><span class="sxs-lookup"><span data-stu-id="b256e-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="b256e-119">*Telas da loja de música*</span><span class="sxs-lookup"><span data-stu-id="b256e-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="b256e-120">ASP.NET MVC 4 Essentials</span><span class="sxs-lookup"><span data-stu-id="b256e-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="b256e-121">O aplicativo da loja de música será criado usando o **MVC (Model View Controller)** , um padrão de arquitetura que separa um aplicativo em três componentes principais:</span><span class="sxs-lookup"><span data-stu-id="b256e-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="b256e-122">**Modelos**: objetos de modelo são as partes do aplicativo que implementam a lógica de domínio.</span><span class="sxs-lookup"><span data-stu-id="b256e-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="b256e-123">Geralmente, os objetos de modelo também recuperam e armazenam o estado do modelo em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b256e-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="b256e-124">**Modos de exibição:** As exibições são os componentes que exibem a interface do usuário do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b256e-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="b256e-125">Normalmente, esta IU é criada a partir dos dados do modelo.</span><span class="sxs-lookup"><span data-stu-id="b256e-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="b256e-126">Um exemplo seria o modo de exibição de edição de álbuns que exibe caixas de texto e uma lista suspensa com base no estado atual de um objeto de álbum.</span><span class="sxs-lookup"><span data-stu-id="b256e-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="b256e-127">**Controladores:** Os controladores são os componentes que manipulam a interação do usuário, manipulam o modelo e, por fim, selecionam uma exibição para renderizar a interface de usuário.</span><span class="sxs-lookup"><span data-stu-id="b256e-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="b256e-128">Em um aplicativo MVC, a exibição só mostra informações; o controlador manipula e responde à entrada e à interação do usuário.</span><span class="sxs-lookup"><span data-stu-id="b256e-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="b256e-129">O padrão MVC ajuda a criar aplicativos que separam os diferentes aspectos do aplicativo (lógica de entrada, lógica de negócios e lógica de interface do usuário), fornecendo, ao mesmo tempo, um acoplamento flexível entre esses elementos.</span><span class="sxs-lookup"><span data-stu-id="b256e-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="b256e-130">Essa separação ajuda a gerenciar a complexidade quando você cria um aplicativo, pois permite que você se concentre em um aspecto da implementação de cada vez.</span><span class="sxs-lookup"><span data-stu-id="b256e-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="b256e-131">Além disso, o padrão MVC facilita o teste de aplicativos, também incentivando o uso do TDD (desenvolvimento controlado por testes) para a criação de aplicativos.</span><span class="sxs-lookup"><span data-stu-id="b256e-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="b256e-132">O **ASP.NET MVC** Framework fornece uma alternativa ao padrão de Web Forms ASP.net para criar aplicativos Web baseados em ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b256e-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="b256e-133">O **ASP.NET MVC** Framework é uma estrutura de apresentação leve e altamente testada que (assim como os aplicativos baseados em formulários da Web) é integrada aos recursos existentes do ASP.net, como páginas mestras e autenticação baseada em associação para que você obtenha todo o poder do .NET Framework principal.</span><span class="sxs-lookup"><span data-stu-id="b256e-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="b256e-134">Isso será útil se você já estiver familiarizado com ASP.NET Web Forms porque todas as bibliotecas que você já usa também estão disponíveis no ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b256e-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="b256e-135">Além disso, o acoplamento flexível entre os três componentes principais de um aplicativo MVC também promove o desenvolvimento paralelo.</span><span class="sxs-lookup"><span data-stu-id="b256e-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="b256e-136">Por exemplo, um desenvolvedor pode trabalhar na exibição, um segundo desenvolvedor pode trabalhar na lógica do controlador e um terceiro desenvolvedor pode se concentrar na lógica de negócios no modelo.</span><span class="sxs-lookup"><span data-stu-id="b256e-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="b256e-137">Objetivos</span><span class="sxs-lookup"><span data-stu-id="b256e-137">Objectives</span></span>

<span data-ttu-id="b256e-138">Neste laboratório prático, você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="b256e-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="b256e-139">Criar um aplicativo MVC ASP.NET do zero com base no tutorial do aplicativo de loja de música</span><span class="sxs-lookup"><span data-stu-id="b256e-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="b256e-140">Adicionar controladores para lidar com URLs na home page do site e para procurar sua funcionalidade principal</span><span class="sxs-lookup"><span data-stu-id="b256e-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="b256e-141">Adicionar uma exibição para personalizar o conteúdo exibido junto com seu estilo</span><span class="sxs-lookup"><span data-stu-id="b256e-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="b256e-142">Adicionar classes de modelo para conter e gerenciar dados e lógica de domínio</span><span class="sxs-lookup"><span data-stu-id="b256e-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="b256e-143">Use o padrão de modelo de exibição para passar informações de ações do controlador para os modelos de exibição</span><span class="sxs-lookup"><span data-stu-id="b256e-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="b256e-144">Explore o novo modelo do ASP.NET MVC 4 para aplicativos de Internet</span><span class="sxs-lookup"><span data-stu-id="b256e-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="b256e-145">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="b256e-145">Prerequisites</span></span>

<span data-ttu-id="b256e-146">Você deve ter os seguintes itens para concluir este laboratório:</span><span class="sxs-lookup"><span data-stu-id="b256e-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="b256e-147">[Visual Studio 2012 Express para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (leia [o apêndice a](#AppendixA) para obter instruções sobre como instalá-lo)</span><span class="sxs-lookup"><span data-stu-id="b256e-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="b256e-148">Instalação</span><span class="sxs-lookup"><span data-stu-id="b256e-148">Setup</span></span>

<span data-ttu-id="b256e-149">**Instalando trechos de código**</span><span class="sxs-lookup"><span data-stu-id="b256e-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="b256e-150">Para sua conveniência, grande parte do código que você gerenciará ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b256e-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="b256e-151">Para instalar os trechos de código, execute o arquivo **.\Source\Setup\CodeSnippets.VSI** .</span><span class="sxs-lookup"><span data-stu-id="b256e-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="b256e-152">Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot;[Apêndice C: usando trechos de código](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="b256e-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="b256e-153">Exercícios</span><span class="sxs-lookup"><span data-stu-id="b256e-153">Exercises</span></span>

<span data-ttu-id="b256e-154">Este laboratório prático é composto pelos seguintes exercícios:</span><span class="sxs-lookup"><span data-stu-id="b256e-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="b256e-155">Exercício 1: criando o projeto de aplicativo Web MusicStore ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b256e-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="b256e-156">Exercício 2: Criando um controlador</span><span class="sxs-lookup"><span data-stu-id="b256e-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="b256e-157">Exercício 3: passando parâmetros para um controlador</span><span class="sxs-lookup"><span data-stu-id="b256e-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="b256e-158">Exercício 4: criando uma exibição</span><span class="sxs-lookup"><span data-stu-id="b256e-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="b256e-159">Exercício 5: Criando um modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="b256e-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="b256e-160">Exercício 6: usando parâmetros na exibição</span><span class="sxs-lookup"><span data-stu-id="b256e-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="b256e-161">Exercício 7: um colo sobre o novo modelo do MVC 4 do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b256e-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="b256e-162">Cada exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir os exercícios.</span><span class="sxs-lookup"><span data-stu-id="b256e-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="b256e-163">Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com os exercícios.</span><span class="sxs-lookup"><span data-stu-id="b256e-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="b256e-164">Tempo estimado para concluir este laboratório: **60 minutos**.</span><span class="sxs-lookup"><span data-stu-id="b256e-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="b256e-165">Exercício 1: criando o projeto de aplicativo Web MusicStore ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b256e-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="b256e-166">Neste exercício, você aprenderá a criar um aplicativo MVC ASP.NET no Visual Studio 2012 Express para Web, bem como em sua organização de pastas principais.</span><span class="sxs-lookup"><span data-stu-id="b256e-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="b256e-167">Além disso, você aprenderá como adicionar um novo controlador e fazer com que ele exiba uma cadeia de caracteres simples no home page do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b256e-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="b256e-168">Tarefa 1-criando o projeto de aplicativo Web do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b256e-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="b256e-169">Nesta tarefa, você criará um projeto de aplicativo ASP.NET MVC vazio usando o modelo do Visual Studio MVC.</span><span class="sxs-lookup"><span data-stu-id="b256e-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="b256e-170">Iniciar **vs Express para Web**.</span><span class="sxs-lookup"><span data-stu-id="b256e-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="b256e-171">No menu **Arquivo**, clique em **Novo Projeto**.</span><span class="sxs-lookup"><span data-stu-id="b256e-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="b256e-172">Na caixa de diálogo **novo projeto** , selecione o tipo de projeto de **aplicativo Web ASP.NET MVC 4** , localizado em **Visual C#, lista de** modelos **da Web** .</span><span class="sxs-lookup"><span data-stu-id="b256e-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="b256e-173">Altere o **nome** para *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="b256e-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="b256e-174">Defina o local da solução dentro de uma nova pasta **inicial** na pasta de origem deste exercício, por exemplo **[Your-Chol-Path] \Source\Ex01-CreatingMusicStoreProject\Begin**.</span><span class="sxs-lookup"><span data-stu-id="b256e-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="b256e-175">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="b256e-175">Click **OK**.</span></span>

    <span data-ttu-id="b256e-176">![Caixa de diálogo Criar novo projeto](aspnet-mvc-4-fundamentals/_static/image2.png "Caixa de diálogo Criar novo projeto")</span><span class="sxs-lookup"><span data-stu-id="b256e-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="b256e-177">*Caixa de diálogo Criar novo projeto*</span><span class="sxs-lookup"><span data-stu-id="b256e-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="b256e-178">Na caixa de diálogo **novo projeto do ASP.NET MVC 4** , selecione o modelo **básico** e verifique se o **mecanismo de exibição** selecionado é **Razor**.</span><span class="sxs-lookup"><span data-stu-id="b256e-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="b256e-179">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="b256e-179">Click **OK**.</span></span>

    <span data-ttu-id="b256e-180">![Caixa de diálogo novo projeto do ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "Caixa de diálogo novo projeto do ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="b256e-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="b256e-181">*Caixa de diálogo novo projeto do ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="b256e-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="b256e-182">Tarefa 2-explorando a estrutura da solução</span><span class="sxs-lookup"><span data-stu-id="b256e-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="b256e-183">O ASP.NET MVC Framework inclui um modelo de projeto do Visual Studio que ajuda a criar aplicativos Web que dão suporte ao padrão MVC.</span><span class="sxs-lookup"><span data-stu-id="b256e-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="b256e-184">Este modelo cria um novo aplicativo Web ASP.NET MVC com as pastas necessárias, os modelos de item e as entradas de arquivo de configuração.</span><span class="sxs-lookup"><span data-stu-id="b256e-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="b256e-185">Nesta tarefa, você examinará a estrutura da solução para entender os elementos envolvidos e suas relações.</span><span class="sxs-lookup"><span data-stu-id="b256e-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="b256e-186">As seguintes pastas estão incluídas em todos os aplicativos ASP.NET MVC, pois a estrutura MVC do ASP.NET, por padrão, usa uma Convenção de &quot;sobre a abordagem de&quot; de configuração e faz algumas suposições padrão com base nas convenções de nomenclatura de pastas.</span><span class="sxs-lookup"><span data-stu-id="b256e-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="b256e-187">Depois que o projeto for criado, examine a estrutura de pastas que foi criada na Gerenciador de Soluções no lado direito:</span><span class="sxs-lookup"><span data-stu-id="b256e-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="b256e-188">![Estrutura de pastas MVC do ASP.NET no Gerenciador de Soluções](aspnet-mvc-4-fundamentals/_static/image4.png "Estrutura de pastas MVC do ASP.NET no Gerenciador de Soluções")</span><span class="sxs-lookup"><span data-stu-id="b256e-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="b256e-189">*Estrutura de pastas MVC do ASP.NET no Gerenciador de Soluções*</span><span class="sxs-lookup"><span data-stu-id="b256e-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="b256e-190">**Controladores**.</span><span class="sxs-lookup"><span data-stu-id="b256e-190">**Controllers**.</span></span> <span data-ttu-id="b256e-191">Essa pasta conterá as classes do controlador.</span><span class="sxs-lookup"><span data-stu-id="b256e-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="b256e-192">Em um aplicativo baseado em MVC, os controladores são responsáveis por lidar com a interação do usuário final, manipulando o modelo e, por fim, escolhendo uma exibição para renderizar a interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="b256e-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="b256e-193">A MVC Framework exige que os nomes de todos os controladores terminem com &quot;controlador&quot;-por exemplo, HomeController, LoginController ou ProductController.</span><span class="sxs-lookup"><span data-stu-id="b256e-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="b256e-194">**Modelos**.</span><span class="sxs-lookup"><span data-stu-id="b256e-194">**Models**.</span></span> <span data-ttu-id="b256e-195">Essa pasta é fornecida para classes que representam o modelo de aplicativo para o aplicativo Web MVC.</span><span class="sxs-lookup"><span data-stu-id="b256e-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="b256e-196">Isso geralmente inclui um código que define objetos e a lógica para interagir com o armazenamento de dados.</span><span class="sxs-lookup"><span data-stu-id="b256e-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="b256e-197">Normalmente, os objetos de modelo reais estarão em bibliotecas de classes separadas.</span><span class="sxs-lookup"><span data-stu-id="b256e-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="b256e-198">No entanto, ao criar um novo aplicativo, você pode incluir classes e, em seguida, movê-las para bibliotecas de classes separadas em um ponto posterior no ciclo de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="b256e-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="b256e-199">**Exibições**.</span><span class="sxs-lookup"><span data-stu-id="b256e-199">**Views**.</span></span> <span data-ttu-id="b256e-200">Essa pasta é o local recomendado para exibições, os componentes responsáveis por exibir a interface do usuário do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b256e-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="b256e-201">Os modos de exibição usam arquivos. aspx,. ascx,. cshtml e. Master, além de quaisquer outros arquivos relacionados às exibições de renderização.</span><span class="sxs-lookup"><span data-stu-id="b256e-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="b256e-202">A pasta views contém uma pasta para cada controlador; a pasta é nomeada com o prefixo do nome do controlador.</span><span class="sxs-lookup"><span data-stu-id="b256e-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="b256e-203">Por exemplo, se você tiver um controlador chamado **HomeController**, a pasta views conterá uma pasta chamada Home.</span><span class="sxs-lookup"><span data-stu-id="b256e-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="b256e-204">Por padrão, quando o ASP.NET MVC Framework carrega uma exibição, ele procura um arquivo. aspx com o nome de exibição solicitado na pasta Views\controllerName (**views [ControllerName] [Action]. aspx**) ou (**views [ControllerName] [Action]. cshtml**) para exibições do Razor.</span><span class="sxs-lookup"><span data-stu-id="b256e-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b256e-205">Além das pastas listadas anteriormente, um aplicativo Web MVC usa o arquivo **global. asax** para definir padrões de roteamento de URL global e usa o arquivo **Web. config** para configurar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b256e-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="b256e-206">Tarefa 3-adicionando um HomeController</span><span class="sxs-lookup"><span data-stu-id="b256e-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="b256e-207">Em aplicativos ASP.NET que não usam a MVC Framework, a interação do usuário é organizada em volta das páginas e, ao gerar e manipular eventos a partir dessas páginas.</span><span class="sxs-lookup"><span data-stu-id="b256e-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="b256e-208">Por outro lado, a interação do usuário com aplicativos ASP.NET MVC é organizada em relação a controladores e seus métodos de ação.</span><span class="sxs-lookup"><span data-stu-id="b256e-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="b256e-209">Por outro lado, o ASP.NET MVC Framework mapeia URLs para classes que são conhecidas como controladores.</span><span class="sxs-lookup"><span data-stu-id="b256e-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="b256e-210">Os controladores processam solicitações de entrada, lidam com a entrada e as interações do usuário, executam a lógica do aplicativo apropriada e determinam a resposta a ser enviada de volta ao cliente (exibir HTML, baixar um arquivo, redirecionar para uma URL diferente, etc.).</span><span class="sxs-lookup"><span data-stu-id="b256e-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="b256e-211">No caso da exibição de HTML, uma classe de controlador normalmente chama um componente de exibição separado para gerar a marcação HTML para a solicitação.</span><span class="sxs-lookup"><span data-stu-id="b256e-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="b256e-212">Em um aplicativo MVC, a exibição só mostra informações; o controlador manipula e responde à entrada e à interação do usuário.</span><span class="sxs-lookup"><span data-stu-id="b256e-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="b256e-213">Nesta tarefa, você adicionará uma classe de controlador que tratará URLs para a home page do site da loja de música.</span><span class="sxs-lookup"><span data-stu-id="b256e-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="b256e-214">Clique com o botão direito do mouse na pasta **controladores** dentro da Gerenciador de soluções, selecione **Adicionar** e, em seguida, comando do **controlador** :</span><span class="sxs-lookup"><span data-stu-id="b256e-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="b256e-215">![Adicionar um comando de controlador](aspnet-mvc-4-fundamentals/_static/image5.png "Adicionar um comando de controlador")</span><span class="sxs-lookup"><span data-stu-id="b256e-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="b256e-216">*Comando adicionar controlador*</span><span class="sxs-lookup"><span data-stu-id="b256e-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="b256e-217">A caixa de diálogo **Adicionar controlador** é exibida.</span><span class="sxs-lookup"><span data-stu-id="b256e-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="b256e-218">Nomeie o controlador *HomeController* e pressione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b256e-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="b256e-219">![Caixa de diálogo Adicionar controlador](aspnet-mvc-4-fundamentals/_static/image6.png "Caixa de diálogo Adicionar controlador")</span><span class="sxs-lookup"><span data-stu-id="b256e-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="b256e-220">*Caixa de diálogo Adicionar controlador*</span><span class="sxs-lookup"><span data-stu-id="b256e-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="b256e-221">O arquivo **HomeController.cs** é criado na pasta **controladores** .</span><span class="sxs-lookup"><span data-stu-id="b256e-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="b256e-222">Para que o **HomeController** retorne uma cadeia de caracteres em sua ação de índice, substitua o método de **índice** pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="b256e-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="b256e-223">(Trecho de código- *ASP.NET MVC 4 Fundamentals-EX1 HomeController index*)</span><span class="sxs-lookup"><span data-stu-id="b256e-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="b256e-224">Tarefa 4-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="b256e-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="b256e-225">Nesta tarefa, você experimentará o aplicativo em um navegador da Web.</span><span class="sxs-lookup"><span data-stu-id="b256e-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="b256e-226">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b256e-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="b256e-227">O projeto é compilado e o servidor Web do IIS local é iniciado.</span><span class="sxs-lookup"><span data-stu-id="b256e-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="b256e-228">O servidor Web local do IIS abrirá automaticamente um navegador da Web apontando para a URL do servidor Web.</span><span class="sxs-lookup"><span data-stu-id="b256e-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="b256e-229">![Aplicativo em execução em um navegador da Web](aspnet-mvc-4-fundamentals/_static/image7.png "Aplicativo em execução em um navegador da Web")</span><span class="sxs-lookup"><span data-stu-id="b256e-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="b256e-230">*Aplicativo em execução em um navegador da Web*</span><span class="sxs-lookup"><span data-stu-id="b256e-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="b256e-231">O servidor Web local do IIS executará o site em um número de porta livre aleatório.</span><span class="sxs-lookup"><span data-stu-id="b256e-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="b256e-232">Na figura acima, o site está em execução em `http://localhost:50103/`, portanto, ele está usando a porta 50103.</span><span class="sxs-lookup"><span data-stu-id="b256e-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="b256e-233">O número da porta pode variar.</span><span class="sxs-lookup"><span data-stu-id="b256e-233">Your port number may vary.</span></span>
2. <span data-ttu-id="b256e-234">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="b256e-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="b256e-235">Exercício 2: Criando um controlador</span><span class="sxs-lookup"><span data-stu-id="b256e-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="b256e-236">Neste exercício, você aprenderá a atualizar o controlador para implementar a funcionalidade simples do aplicativo de loja de música.</span><span class="sxs-lookup"><span data-stu-id="b256e-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="b256e-237">Esse controlador definirá métodos de ação para lidar com cada uma das seguintes solicitações específicas:</span><span class="sxs-lookup"><span data-stu-id="b256e-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="b256e-238">Uma página de listagem dos gêneros musicais na loja de música</span><span class="sxs-lookup"><span data-stu-id="b256e-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="b256e-239">Uma página de navegação que lista todos os álbuns de música para um gênero específico</span><span class="sxs-lookup"><span data-stu-id="b256e-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="b256e-240">Uma página de detalhes que mostra informações sobre um álbum de música específico</span><span class="sxs-lookup"><span data-stu-id="b256e-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="b256e-241">Para o escopo deste exercício, essas ações simplesmente retornarão uma cadeia de caracteres agora.</span><span class="sxs-lookup"><span data-stu-id="b256e-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="b256e-242">Tarefa 1-adicionando uma nova classe StoreController</span><span class="sxs-lookup"><span data-stu-id="b256e-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="b256e-243">Nesta tarefa, você adicionará um novo controlador.</span><span class="sxs-lookup"><span data-stu-id="b256e-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="b256e-244">Se ainda não estiver aberto, inicie o **VS Express para Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="b256e-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="b256e-245">No menu **arquivo** , escolha **Abrir projeto**.</span><span class="sxs-lookup"><span data-stu-id="b256e-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="b256e-246">Na caixa de diálogo Abrir projeto, navegue até **Source\Ex02-CreatingAController\Begin**, selecione **Iniciar. sln** e clique em **abrir**.</span><span class="sxs-lookup"><span data-stu-id="b256e-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="b256e-247">Como alternativa, você pode continuar com a solução obtida depois de concluir o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="b256e-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="b256e-248">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="b256e-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b256e-249">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b256e-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b256e-250">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="b256e-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b256e-251">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="b256e-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b256e-252">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="b256e-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b256e-253">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="b256e-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b256e-254">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="b256e-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="b256e-255">Adicione o novo controlador.</span><span class="sxs-lookup"><span data-stu-id="b256e-255">Add the new controller.</span></span> <span data-ttu-id="b256e-256">Para fazer isso, clique com o botão direito do mouse na pasta **controladores** dentro da Gerenciador de soluções, selecione **Adicionar** e, em seguida, o comando **controlador** .</span><span class="sxs-lookup"><span data-stu-id="b256e-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="b256e-257">Altere o **nome do controlador** para *StoreController*e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b256e-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="b256e-258">![Caixa de diálogo Adicionar controlador](aspnet-mvc-4-fundamentals/_static/image8.png "Caixa de diálogo Adicionar controlador")</span><span class="sxs-lookup"><span data-stu-id="b256e-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="b256e-259">*Caixa de diálogo Adicionar controlador*</span><span class="sxs-lookup"><span data-stu-id="b256e-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="b256e-260">Tarefa 2-modificando as ações do StoreController</span><span class="sxs-lookup"><span data-stu-id="b256e-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="b256e-261">Nesta tarefa, você modificará os métodos do controlador que são chamados de **ações**.</span><span class="sxs-lookup"><span data-stu-id="b256e-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="b256e-262">As ações são responsáveis pelo tratamento de solicitações de URL e pela determinação do conteúdo que deve ser enviado de volta ao navegador ou ao usuário que invocou a URL.</span><span class="sxs-lookup"><span data-stu-id="b256e-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="b256e-263">A classe **StoreController** já tem um método **index** .</span><span class="sxs-lookup"><span data-stu-id="b256e-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="b256e-264">Você o usará posteriormente neste laboratório para implementar a página que lista todos os gêneros da loja de música.</span><span class="sxs-lookup"><span data-stu-id="b256e-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="b256e-265">Por enquanto, basta substituir o método de **índice** pelo código a seguir que retorna uma cadeia de caracteres &quot;Olá de Store. Index ()&quot;:</span><span class="sxs-lookup"><span data-stu-id="b256e-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="b256e-266">(Trecho de código- *ASP.NET MVC 4 Fundamentals-EX2 StoreController index*)</span><span class="sxs-lookup"><span data-stu-id="b256e-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="b256e-267">Adicione os métodos **Browse** e **Details** .</span><span class="sxs-lookup"><span data-stu-id="b256e-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="b256e-268">Para fazer isso, adicione o seguinte código ao **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="b256e-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="b256e-269">(Trecho de código- *ASP.NET MVC 4 Fundamentals-EX2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="b256e-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="b256e-270">Tarefa 3-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="b256e-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="b256e-271">Nesta tarefa, você experimentará o aplicativo em um navegador da Web.</span><span class="sxs-lookup"><span data-stu-id="b256e-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="b256e-272">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b256e-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b256e-273">O projeto é iniciado na **Home** Page.</span><span class="sxs-lookup"><span data-stu-id="b256e-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="b256e-274">Altere a URL para verificar a implementação de cada ação.</span><span class="sxs-lookup"><span data-stu-id="b256e-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="b256e-275">**/Store**.</span><span class="sxs-lookup"><span data-stu-id="b256e-275">**/Store**.</span></span> <span data-ttu-id="b256e-276">Você verá **&quot;Olá do Store. Index ()&quot;** .</span><span class="sxs-lookup"><span data-stu-id="b256e-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="b256e-277">**/Store/Browse**.</span><span class="sxs-lookup"><span data-stu-id="b256e-277">**/Store/Browse**.</span></span> <span data-ttu-id="b256e-278">Você verá **&quot;Olá de Store. Browse ()&quot;** .</span><span class="sxs-lookup"><span data-stu-id="b256e-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="b256e-279">**/Store/Details**.</span><span class="sxs-lookup"><span data-stu-id="b256e-279">**/Store/Details**.</span></span> <span data-ttu-id="b256e-280">Você verá **&quot;Olá da Store. Details ()&quot;** .</span><span class="sxs-lookup"><span data-stu-id="b256e-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="b256e-281">![Navegando em StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Navegando em StoreBrowse")</span><span class="sxs-lookup"><span data-stu-id="b256e-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="b256e-282">*Navegando em/Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="b256e-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="b256e-283">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="b256e-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="b256e-284">Exercício 3: passando parâmetros para um controlador</span><span class="sxs-lookup"><span data-stu-id="b256e-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="b256e-285">Até agora, você está retornando cadeias de caracteres constantes dos controladores.</span><span class="sxs-lookup"><span data-stu-id="b256e-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="b256e-286">Neste exercício, você aprenderá a passar parâmetros para um controlador usando a URL e a QueryString e, em seguida, fazer com que as ações do método respondam com o texto para o navegador.</span><span class="sxs-lookup"><span data-stu-id="b256e-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="b256e-287">Tarefa 1-adicionando o parâmetro de gênero a StoreController</span><span class="sxs-lookup"><span data-stu-id="b256e-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="b256e-288">Nesta tarefa, você usará a **QueryString** para enviar parâmetros para o método de ação **procurar** no **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="b256e-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="b256e-289">Se ainda não estiver aberto, inicie o **vs Express para Web**.</span><span class="sxs-lookup"><span data-stu-id="b256e-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="b256e-290">No menu **arquivo** , escolha **Abrir projeto**.</span><span class="sxs-lookup"><span data-stu-id="b256e-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="b256e-291">Na caixa de diálogo Abrir projeto, navegue até **Source\Ex03-PassingParametersToAController\Begin**, selecione **Iniciar. sln** e clique em **abrir**.</span><span class="sxs-lookup"><span data-stu-id="b256e-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="b256e-292">Como alternativa, você pode continuar com a solução obtida depois de concluir o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="b256e-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="b256e-293">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="b256e-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b256e-294">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b256e-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b256e-295">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="b256e-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b256e-296">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="b256e-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b256e-297">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="b256e-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b256e-298">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="b256e-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b256e-299">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="b256e-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="b256e-300">Abra a classe **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="b256e-300">Open **StoreController** class.</span></span> <span data-ttu-id="b256e-301">Para fazer isso, na **Gerenciador de soluções**, expanda a pasta **controladores** e clique duas vezes em **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="b256e-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="b256e-302">Altere o método **procurar** , adicionando um parâmetro de cadeia de caracteres para solicitar um gênero específico.</span><span class="sxs-lookup"><span data-stu-id="b256e-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="b256e-303">O ASP.NET MVC passará automaticamente qualquer parâmetro QueryString ou formulário post chamado **gênero** para esse método de ação quando invocado.</span><span class="sxs-lookup"><span data-stu-id="b256e-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="b256e-304">Para fazer isso, substitua o método **Browse** pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="b256e-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="b256e-305">(Trecho de código- *ASP.NET MVC 4 Fundamentals-EX3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="b256e-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="b256e-306">Você está usando o método do utilitário **HttpUtility. HtmlEncode** para impedir que os usuários insiram JavaScript na exibição com um link como **/Store/Browse? Gênero =&lt;script&gt;janela. Location = '[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;** .</span><span class="sxs-lookup"><span data-stu-id="b256e-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="b256e-307">Para obter mais explicações, visite [Este artigo do MSDN](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="b256e-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="b256e-308">Tarefa 2-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="b256e-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="b256e-309">Nesta tarefa, você experimentará o aplicativo em um navegador da Web e usará o parâmetro **gênero** .</span><span class="sxs-lookup"><span data-stu-id="b256e-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="b256e-310">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b256e-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b256e-311">O projeto é iniciado na **Home** Page.</span><span class="sxs-lookup"><span data-stu-id="b256e-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="b256e-312">Alterar a URL para */Store/Browse? Gênero = disco* para verificar se a ação recebe o parâmetro gênero.</span><span class="sxs-lookup"><span data-stu-id="b256e-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="b256e-313">![Navegando em StoreBrowseGenre = disco](aspnet-mvc-4-fundamentals/_static/image10.png "Navegando em StoreBrowseGenre = disco")</span><span class="sxs-lookup"><span data-stu-id="b256e-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="b256e-314">*Procurando/Store/Browse? Gênero = disco*</span><span class="sxs-lookup"><span data-stu-id="b256e-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="b256e-315">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="b256e-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="b256e-316">Tarefa 3-adicionando um parâmetro de ID inserido na URL</span><span class="sxs-lookup"><span data-stu-id="b256e-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="b256e-317">Nesta tarefa, você usará a **URL** para passar um parâmetro de **ID** para o método de ação de **detalhes** do **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="b256e-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="b256e-318">A Convenção de roteamento padrão do ASP.NET MVC é tratar o segmento de uma URL após o nome do método de ação como um parâmetro chamado **ID**. Se o método de ação tiver um parâmetro chamado ID, o ASP.NET MVC passará automaticamente o segmento de URL para você como um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b256e-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="b256e-319">Na URL **Store/Details/5**, a **ID** será interpretada como **5**.</span><span class="sxs-lookup"><span data-stu-id="b256e-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="b256e-320">Altere o método **Details** do **StoreController**, adicionando um parâmetro **int** chamado **ID**. Para fazer isso, substitua o método **Details** pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="b256e-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="b256e-321">(Trecho de código- *ASP.NET MVC 4 Fundamentals-EX3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="b256e-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="b256e-322">Tarefa 4-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="b256e-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="b256e-323">Nesta tarefa, você experimentará o aplicativo em um navegador da Web e usará o parâmetro **ID** .</span><span class="sxs-lookup"><span data-stu-id="b256e-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="b256e-324">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b256e-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b256e-325">O projeto é iniciado na **Home** Page.</span><span class="sxs-lookup"><span data-stu-id="b256e-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="b256e-326">Altere a URL para */Store/Details/5* para verificar se a ação recebe o parâmetro ID.</span><span class="sxs-lookup"><span data-stu-id="b256e-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="b256e-327">![Navegando em StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Navegando em StoreDetails5")</span><span class="sxs-lookup"><span data-stu-id="b256e-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="b256e-328">*Navegando em/Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="b256e-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="b256e-329">Exercício 4: criando uma exibição</span><span class="sxs-lookup"><span data-stu-id="b256e-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="b256e-330">Até agora, você está retornando cadeias de caracteres de ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="b256e-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="b256e-331">Embora essa seja uma maneira útil de entender como os controladores funcionam, não é assim que seus aplicativos Web reais são criados.</span><span class="sxs-lookup"><span data-stu-id="b256e-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="b256e-332">Modos de exibição são componentes que fornecem uma abordagem melhor para gerar o HTML de volta para o navegador com o uso de arquivos de modelo.</span><span class="sxs-lookup"><span data-stu-id="b256e-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="b256e-333">Neste exercício, você aprenderá a adicionar uma página mestra de layout para configurar um modelo para conteúdo HTML comum, uma folha de estilos para aprimorar a aparência do site e, finalmente, um modelo de exibição para permitir que o HomeController retorne HTML.</span><span class="sxs-lookup"><span data-stu-id="b256e-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-_layoutcshtml"></a><span data-ttu-id="b256e-334">Tarefa 1-modificando o arquivo \_layout. cshtml</span><span class="sxs-lookup"><span data-stu-id="b256e-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="b256e-335">O arquivo **~/Views/Shared/\_layout. cshtml** permite configurar um modelo para que o HTML comum seja usado em todo o site.</span><span class="sxs-lookup"><span data-stu-id="b256e-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="b256e-336">Nesta tarefa, você adicionará uma página mestra de layout com um cabeçalho comum com links para a página inicial e área de armazenamento.</span><span class="sxs-lookup"><span data-stu-id="b256e-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="b256e-337">Se ainda não estiver aberto, inicie o **vs Express para Web**.</span><span class="sxs-lookup"><span data-stu-id="b256e-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="b256e-338">No menu **arquivo** , escolha **Abrir projeto**.</span><span class="sxs-lookup"><span data-stu-id="b256e-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="b256e-339">Na caixa de diálogo Abrir projeto, navegue até **Source\Ex04-CreatingAView\Begin**, selecione **Iniciar. sln** e clique em **abrir**.</span><span class="sxs-lookup"><span data-stu-id="b256e-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="b256e-340">Como alternativa, você pode continuar com a solução obtida depois de concluir o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="b256e-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="b256e-341">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="b256e-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b256e-342">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b256e-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b256e-343">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="b256e-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b256e-344">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="b256e-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b256e-345">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="b256e-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b256e-346">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="b256e-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b256e-347">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="b256e-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="b256e-348">O arquivo <strong>\_layout. cshtml</strong> contém o layout do contêiner HTML para todas as páginas no site.</span><span class="sxs-lookup"><span data-stu-id="b256e-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="b256e-349">Ele inclui o elemento <strong>&lt;html&gt;</strong> para a resposta HTML, bem como os elementos <strong>&gt;Head&lt;</strong> e&lt;<strong>Body</strong>&gt;.</span><span class="sxs-lookup"><span data-stu-id="b256e-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="b256e-350"><strong>@RenderBody()</strong> no corpo de HTML identificam as regiões que exibem modelos que poderão preencher com conteúdo dinâmico.</span><span class="sxs-lookup"><span data-stu-id="b256e-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="b256e-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="b256e-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="b256e-352">Adicione um cabeçalho comum com links para a Home Page e área de armazenamento em todas as páginas do site.</span><span class="sxs-lookup"><span data-stu-id="b256e-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="b256e-353">Para fazer isso, adicione o seguinte código abaixo &lt;instrução&gt; do corpo.</span><span class="sxs-lookup"><span data-stu-id="b256e-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="b256e-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="b256e-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="b256e-355">Inclua um div para renderizar a seção de corpo de cada página.</span><span class="sxs-lookup"><span data-stu-id="b256e-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="b256e-356">Substitua <strong>@RenderBody()</strong> pelo seguinte código realçado:C#()</span><span class="sxs-lookup"><span data-stu-id="b256e-356">Replace <strong>@RenderBody()</strong> with the following highlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="b256e-357">Você sabia?</span><span class="sxs-lookup"><span data-stu-id="b256e-357">Did you know?</span></span> <span data-ttu-id="b256e-358">O Visual Studio 2012 tem trechos que facilitam a adição de código comumente usado em HTML, arquivos de código e muito mais!</span><span class="sxs-lookup"><span data-stu-id="b256e-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="b256e-359">Experimente, digitando **&lt;div&gt;** e pressionando **Tab** duas vezes para inserir uma marca **div** completa.</span><span class="sxs-lookup"><span data-stu-id="b256e-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="b256e-360">Tarefa 2-Adicionando folha de estilos CSS</span><span class="sxs-lookup"><span data-stu-id="b256e-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="b256e-361">O modelo de projeto vazio inclui um arquivo CSS muito simplificado que inclui apenas estilos usados para exibir formulários básicos e mensagens de validação.</span><span class="sxs-lookup"><span data-stu-id="b256e-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="b256e-362">Você usará CSS e imagens adicionais (potencialmente fornecidos por um designer) para aprimorar a aparência do site.</span><span class="sxs-lookup"><span data-stu-id="b256e-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="b256e-363">Nesta tarefa, você adicionará uma folha de estilo CSS para definir os estilos do site.</span><span class="sxs-lookup"><span data-stu-id="b256e-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="b256e-364">O arquivo CSS e as imagens são incluídos na pasta **Source\Assets\Content** deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="b256e-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="b256e-365">Para adicioná-los ao aplicativo, arraste o conteúdo de uma janela do **Windows Explorer** para o **Gerenciador de soluções** no Visual Studio Express para Web, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="b256e-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="b256e-366">![Arrastando conteúdo do estilo](aspnet-mvc-4-fundamentals/_static/image12.png "Arrastando conteúdo do estilo")</span><span class="sxs-lookup"><span data-stu-id="b256e-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="b256e-367">*Arrastando conteúdo do estilo*</span><span class="sxs-lookup"><span data-stu-id="b256e-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="b256e-368">Uma caixa de diálogo de aviso será exibida, solicitando a confirmação para substituir o arquivo **site. css** e algumas imagens existentes.</span><span class="sxs-lookup"><span data-stu-id="b256e-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="b256e-369">Marque **aplicar a todos os itens** e clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="b256e-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="b256e-370">Tarefa 3-adicionando um modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="b256e-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="b256e-371">Nesta tarefa, você adicionará um modelo de exibição para gerar a resposta HTML que usará a página mestra de layout e a CSS adicionada neste exercício.</span><span class="sxs-lookup"><span data-stu-id="b256e-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="b256e-372">Para usar um modelo de exibição ao navegar na home page do site, primeiro será necessário indicar que, em vez de retornar uma cadeia de caracteres, o método de **índice HomeController** retornará uma **exibição**.</span><span class="sxs-lookup"><span data-stu-id="b256e-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="b256e-373">Abra a classe **HomeController** e altere seu método de **índice** para retornar um **ActionResult**e faça com que ele retorne **View ()** .</span><span class="sxs-lookup"><span data-stu-id="b256e-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="b256e-374">(Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex4 HomeController index*)</span><span class="sxs-lookup"><span data-stu-id="b256e-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="b256e-375">Agora, você precisa adicionar um modelo de exibição apropriado.</span><span class="sxs-lookup"><span data-stu-id="b256e-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="b256e-376">Para fazer isso, **clique com o botão direito do mouse** dentro do método de ação de **índice** e selecione **Adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="b256e-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="b256e-377">Isso abrirá a caixa de diálogo **Adicionar exibição** .</span><span class="sxs-lookup"><span data-stu-id="b256e-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="b256e-378">![Adicionando uma exibição de dentro do método de índice](aspnet-mvc-4-fundamentals/_static/image13.png "Adicionando uma exibição de dentro do método de índice")</span><span class="sxs-lookup"><span data-stu-id="b256e-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="b256e-379">*Adicionando uma exibição de dentro do método de índice*</span><span class="sxs-lookup"><span data-stu-id="b256e-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="b256e-380">A caixa de diálogo **Adicionar exibição** será exibida para gerar um arquivo de modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="b256e-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="b256e-381">Por padrão, essa caixa de diálogo popula previamente o nome do modelo de exibição para que ele corresponda ao método de ação que o usará.</span><span class="sxs-lookup"><span data-stu-id="b256e-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="b256e-382">Como você usou o menu de contexto **Adicionar exibição** dentro do método de ação de **índice** dentro de HomeController, a caixa de diálogo **Adicionar exibição** tem índice como o nome de exibição padrão.</span><span class="sxs-lookup"><span data-stu-id="b256e-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="b256e-383">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b256e-383">Click **Add**.</span></span>

    <span data-ttu-id="b256e-384">![Caixa de diálogo Adicionar exibição](aspnet-mvc-4-fundamentals/_static/image14.png "Caixa de diálogo Adicionar exibição")</span><span class="sxs-lookup"><span data-stu-id="b256e-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="b256e-385">*Caixa de diálogo Adicionar exibição*</span><span class="sxs-lookup"><span data-stu-id="b256e-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="b256e-386">O Visual Studio gera um modelo de exibição **index. cshtml** dentro da pasta **views\home** e, em seguida, abre-o.</span><span class="sxs-lookup"><span data-stu-id="b256e-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="b256e-387">![Exibição do índice inicial criada](aspnet-mvc-4-fundamentals/_static/image15.png "Exibição do índice inicial criada")</span><span class="sxs-lookup"><span data-stu-id="b256e-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="b256e-388">*Exibição do índice inicial criada*</span><span class="sxs-lookup"><span data-stu-id="b256e-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="b256e-389">o nome e o local do arquivo **index. cshtml** são relevantes e seguem as convenções de nomenclatura ASP.NET MVC padrão.</span><span class="sxs-lookup"><span data-stu-id="b256e-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="b256e-390">A pasta \Views\**Home*\* corresponde ao nome do controlador (controlador**doméstico** ).</span><span class="sxs-lookup"><span data-stu-id="b256e-390">The folder \Views\**Home*\* matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="b256e-391">O nome do modelo de exibição (**índice**) corresponde ao método de ação do controlador que exibirá a exibição.</span><span class="sxs-lookup"><span data-stu-id="b256e-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="b256e-392">Dessa forma, o ASP.NET MVC evita que você precise especificar explicitamente o nome ou o local de um modelo de exibição ao usar essa Convenção de nomenclatura para retornar uma exibição.</span><span class="sxs-lookup"><span data-stu-id="b256e-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="b256e-393">O modelo de exibição gerado é baseado no modelo **\_layout. cshtml** definido anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b256e-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="b256e-394">Atualize a propriedade ViewBag. title para **página inicial**e altere o conteúdo principal para **esta é a Home Page**, conforme mostrado no código abaixo:</span><span class="sxs-lookup"><span data-stu-id="b256e-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="b256e-395">Selecione o projeto **MvcMusicStore** na Gerenciador de soluções e pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b256e-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="b256e-396">Tarefa 4: verificação</span><span class="sxs-lookup"><span data-stu-id="b256e-396">Task 4: Verification</span></span>

<span data-ttu-id="b256e-397">Para verificar se você executou corretamente todas as etapas no exercício anterior, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="b256e-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="b256e-398">Com o aplicativo aberto em um navegador, você deve observar que:</span><span class="sxs-lookup"><span data-stu-id="b256e-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="b256e-399">O método de ação de índice do HomeController encontrou e exibiu o modelo de exibição **\Views\Home\Index.cshtml** , embora o código tenha chamado de **modo de exibição de retorno ()** , porque o modelo de exibição seguiu a Convenção de nomenclatura padrão.</span><span class="sxs-lookup"><span data-stu-id="b256e-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="b256e-400">A Home Page exibe a mensagem de boas-vindas definida no modelo de exibição **\Views\Home\Index.cshtml** .</span><span class="sxs-lookup"><span data-stu-id="b256e-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="b256e-401">A Home Page está usando o modelo **\_layout. cshtml** e, portanto, a mensagem de boas-vindas está contida no layout HTML do site padrão.</span><span class="sxs-lookup"><span data-stu-id="b256e-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="b256e-402">![Exibição de índice inicial usando o LayoutPage e o estilo definidos](aspnet-mvc-4-fundamentals/_static/image16.png "Exibição de índice inicial usando o LayoutPage e o estilo definidos")</span><span class="sxs-lookup"><span data-stu-id="b256e-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="b256e-403">*Exibição de índice inicial usando o LayoutPage e o estilo definidos*</span><span class="sxs-lookup"><span data-stu-id="b256e-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="b256e-404">Exercício 5: Criando um modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="b256e-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="b256e-405">Até agora, você fez suas exibições exibir HTML codificado, mas, para criar aplicativos Web dinâmicos, o modelo de exibição deve receber informações do controlador.</span><span class="sxs-lookup"><span data-stu-id="b256e-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="b256e-406">Uma técnica comum a ser usada para essa finalidade é o padrão **ViewModel** , que permite que um controlador empacote todas as informações necessárias para gerar a resposta HTML apropriada.</span><span class="sxs-lookup"><span data-stu-id="b256e-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="b256e-407">Neste exercício, você aprenderá a criar uma classe ViewModel e a adicionar as propriedades necessárias: o número de gêneros no repositório e uma lista desses gêneros.</span><span class="sxs-lookup"><span data-stu-id="b256e-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="b256e-408">Você também atualizará o StoreController para usar o ViewModel criado e, por fim, criará um novo modelo de exibição que exibirá as propriedades mencionadas na página.</span><span class="sxs-lookup"><span data-stu-id="b256e-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="b256e-409">Tarefa 1-criando uma classe ViewModel</span><span class="sxs-lookup"><span data-stu-id="b256e-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="b256e-410">Nesta tarefa, você criará uma classe ViewModel que implementará o cenário de listagem de gênero de repositório.</span><span class="sxs-lookup"><span data-stu-id="b256e-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="b256e-411">Se ainda não estiver aberto, inicie o **vs Express para Web**.</span><span class="sxs-lookup"><span data-stu-id="b256e-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="b256e-412">No menu **arquivo** , escolha **Abrir projeto**.</span><span class="sxs-lookup"><span data-stu-id="b256e-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="b256e-413">Na caixa de diálogo Abrir projeto, navegue até **Source\Ex05-CreatingAViewModel\Begin**, selecione **Iniciar. sln** e clique em **abrir**.</span><span class="sxs-lookup"><span data-stu-id="b256e-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="b256e-414">Como alternativa, você pode continuar com a solução obtida depois de concluir o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="b256e-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="b256e-415">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="b256e-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b256e-416">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b256e-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b256e-417">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="b256e-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b256e-418">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="b256e-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b256e-419">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="b256e-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b256e-420">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="b256e-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b256e-421">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="b256e-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="b256e-422">Crie uma pasta **ViewModels** para manter o ViewModel.</span><span class="sxs-lookup"><span data-stu-id="b256e-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="b256e-423">Para fazer isso, clique com o botão direito do mouse no projeto **MvcMusicStore** de nível superior, selecione **Adicionar** e **nova pasta**.</span><span class="sxs-lookup"><span data-stu-id="b256e-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="b256e-424">![Adicionando uma nova pasta](aspnet-mvc-4-fundamentals/_static/image17.png "Adicionando uma nova pasta")</span><span class="sxs-lookup"><span data-stu-id="b256e-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="b256e-425">*Adicionando uma nova pasta*</span><span class="sxs-lookup"><span data-stu-id="b256e-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="b256e-426">Nomeie a pasta *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="b256e-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="b256e-427">![Pasta ViewModels no Gerenciador de Soluções](aspnet-mvc-4-fundamentals/_static/image18.png "Pasta ViewModels no Gerenciador de Soluções")</span><span class="sxs-lookup"><span data-stu-id="b256e-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="b256e-428">*Pasta ViewModels no Gerenciador de Soluções*</span><span class="sxs-lookup"><span data-stu-id="b256e-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="b256e-429">Crie uma classe **ViewModel** .</span><span class="sxs-lookup"><span data-stu-id="b256e-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="b256e-430">Para fazer isso, clique com o botão direito do mouse na pasta **ViewModels** criada recentemente, selecione **Adicionar** e **novo item**.</span><span class="sxs-lookup"><span data-stu-id="b256e-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="b256e-431">Em **código**, escolha o item de **classe** e nomeie o arquivo *StoreIndexViewModel.cs*e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b256e-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="b256e-432">![Adicionando uma nova classe](aspnet-mvc-4-fundamentals/_static/image19.png "Adicionando uma nova classe")</span><span class="sxs-lookup"><span data-stu-id="b256e-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="b256e-433">*Adicionando uma nova classe*</span><span class="sxs-lookup"><span data-stu-id="b256e-433">*Adding a new Class*</span></span>

    <span data-ttu-id="b256e-434">![Criando classe StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "Criando classe StoreIndexViewModel")</span><span class="sxs-lookup"><span data-stu-id="b256e-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="b256e-435">*Criando classe StoreIndexViewModel*</span><span class="sxs-lookup"><span data-stu-id="b256e-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="b256e-436">Tarefa 2-Adicionando propriedades à classe ViewModel</span><span class="sxs-lookup"><span data-stu-id="b256e-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="b256e-437">Há dois parâmetros a serem passados do StoreController para o modelo de exibição a fim de gerar a resposta HTML esperada: o número de gêneros na loja e uma lista desses gêneros.</span><span class="sxs-lookup"><span data-stu-id="b256e-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="b256e-438">Nesta tarefa, você adicionará essas duas propriedades à classe **StoreIndexViewModel** : **NumberOfGenres** (um inteiro) e **gêneros** (uma lista de cadeias de caracteres).</span><span class="sxs-lookup"><span data-stu-id="b256e-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="b256e-439">Adicione as propriedades **NumberOfGenres** e **gêneros** à classe **StoreIndexViewModel** .</span><span class="sxs-lookup"><span data-stu-id="b256e-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="b256e-440">Para fazer isso, adicione as duas linhas a seguir à definição de classe:</span><span class="sxs-lookup"><span data-stu-id="b256e-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="b256e-441">(Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex5 StoreIndexViewModel Propriedades*)</span><span class="sxs-lookup"><span data-stu-id="b256e-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="b256e-442">A notação **{Get; Set;}** faz uso C#do recurso de propriedades implementadas automaticamente.</span><span class="sxs-lookup"><span data-stu-id="b256e-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="b256e-443">Ele fornece os benefícios de uma propriedade sem exigir que declaremos um campo de apoio.</span><span class="sxs-lookup"><span data-stu-id="b256e-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="b256e-444">Tarefa 3-atualizando StoreController para usar o StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="b256e-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="b256e-445">A classe **StoreIndexViewModel** encapsula as informações necessárias para passar do método de **índice** de **StoreController**para um modelo de exibição a fim de gerar uma resposta.</span><span class="sxs-lookup"><span data-stu-id="b256e-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="b256e-446">Nesta tarefa, você atualizará o **StoreController** para usar o **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="b256e-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="b256e-447">Abra a classe **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="b256e-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="b256e-448">![Abrindo classe StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "Abrindo classe StoreController")</span><span class="sxs-lookup"><span data-stu-id="b256e-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="b256e-449">*Abrindo classe StoreController*</span><span class="sxs-lookup"><span data-stu-id="b256e-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="b256e-450">Para usar a classe **StoreIndexViewModel** do **StoreController**, adicione o seguinte namespace na parte superior do código **StoreController** :</span><span class="sxs-lookup"><span data-stu-id="b256e-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="b256e-451">(Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex5 StoreIndexViewModel usando ViewModels*)</span><span class="sxs-lookup"><span data-stu-id="b256e-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="b256e-452">Altere o método de ação de **índice** de **StoreController**para que ele crie e preencha um objeto **StoreIndexViewModel** e, em seguida, o passe para um modelo de exibição para gerar uma resposta HTML com ele.</span><span class="sxs-lookup"><span data-stu-id="b256e-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b256e-453">No laboratório &quot;modelos e acesso a dados do ASP.NET MVC&quot; você escreverá um código que recupere a lista de gêneros da loja de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b256e-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="b256e-454">No código a seguir, você criará uma **lista** de gêneros de dados fictícios que preencherão o **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="b256e-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="b256e-455">Depois de criar e configurar o objeto **StoreIndexViewModel** , ele será passado como um argumento para o método **View** .</span><span class="sxs-lookup"><span data-stu-id="b256e-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="b256e-456">Isso indica que o modelo de exibição usará esse objeto para gerar uma resposta HTML com ele.</span><span class="sxs-lookup"><span data-stu-id="b256e-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="b256e-457">Substitua o método de **índice** pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="b256e-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="b256e-458">(Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex5 StoreController index Method*)</span><span class="sxs-lookup"><span data-stu-id="b256e-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="b256e-459">Se você não estiver familiarizado com C#o, poderá pressupor que o uso de **var** significa que a variável **viewModel** é de associação tardia.</span><span class="sxs-lookup"><span data-stu-id="b256e-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="b256e-460">Isso não está correto-o C# compilador está usando a inferência de tipos com base no que você atribui à variável para determinar que **viewModel** é do tipo **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="b256e-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="b256e-461">Além disso, ao compilar a variável **viewModel** local como um tipo **StoreIndexViewModel** , você obtém a verificação de tempo de compilação e o suporte ao editor de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b256e-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="b256e-462">Tarefa 4-Criando um modelo de exibição que usa StoreIndexViewModel</span><span class="sxs-lookup"><span data-stu-id="b256e-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="b256e-463">Nesta tarefa, você criará um modelo de exibição que usará um objeto StoreIndexViewModel passado do controlador para exibir uma lista de gêneros.</span><span class="sxs-lookup"><span data-stu-id="b256e-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="b256e-464">Antes de criar o novo modelo de exibição, vamos criar o projeto para que a **caixa de diálogo Adicionar exibição** saiba mais sobre a classe **StoreIndexViewModel** .</span><span class="sxs-lookup"><span data-stu-id="b256e-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="b256e-465">Crie o projeto selecionando o item de menu **Build** e, em seguida, **Build MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="b256e-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="b256e-466">![Compilando o projeto](aspnet-mvc-4-fundamentals/_static/image22.png "Compilando o projeto")</span><span class="sxs-lookup"><span data-stu-id="b256e-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="b256e-467">*Compilando o projeto*</span><span class="sxs-lookup"><span data-stu-id="b256e-467">*Building the project*</span></span>
2. <span data-ttu-id="b256e-468">Crie um novo modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="b256e-468">Create a new View template.</span></span> <span data-ttu-id="b256e-469">Para fazer isso, clique com o botão direito do mouse dentro do método **index** e selecione **Add View**.</span><span class="sxs-lookup"><span data-stu-id="b256e-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="b256e-470">![Adicionando uma exibição](aspnet-mvc-4-fundamentals/_static/image23.png "Adicionar uma exibição")</span><span class="sxs-lookup"><span data-stu-id="b256e-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="b256e-471">*Adicionando uma exibição*</span><span class="sxs-lookup"><span data-stu-id="b256e-471">*Adding a View*</span></span>
3. <span data-ttu-id="b256e-472">Como a **caixa de diálogo Adicionar exibição** foi invocada do **StoreController**, ela adicionará o modelo de exibição por padrão em um arquivo **\Views\Store\Index.cshtml** .</span><span class="sxs-lookup"><span data-stu-id="b256e-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="b256e-473">Marque a caixa de seleção **criar uma exibição fortemente tipada** e, em seguida, selecione **StoreIndexViewModel** como a **classe de modelo**.</span><span class="sxs-lookup"><span data-stu-id="b256e-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="b256e-474">Além disso, verifique se o mecanismo de exibição selecionado é **Razor**.</span><span class="sxs-lookup"><span data-stu-id="b256e-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="b256e-475">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b256e-475">Click **Add**.</span></span>

    <span data-ttu-id="b256e-476">![Caixa de diálogo Adicionar exibição](aspnet-mvc-4-fundamentals/_static/image24.png "Caixa de diálogo Adicionar exibição")</span><span class="sxs-lookup"><span data-stu-id="b256e-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="b256e-477">*Caixa de diálogo Adicionar exibição*</span><span class="sxs-lookup"><span data-stu-id="b256e-477">*Add View Dialog*</span></span>

    <span data-ttu-id="b256e-478">O arquivo de modelo de exibição **\Views\Store\Index.cshtml** é criado e aberto.</span><span class="sxs-lookup"><span data-stu-id="b256e-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="b256e-479">Com base nas informações fornecidas para a caixa de diálogo **Adicionar exibição** na última etapa, o modelo de exibição esperará uma instância de **StoreIndexViewModel** como os dados a serem usados para gerar uma resposta em HTML.</span><span class="sxs-lookup"><span data-stu-id="b256e-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="b256e-480">Você observará que o modelo herda um `ViewPage<musicstore.viewmodels.storeindexviewmodel>` no C#.</span><span class="sxs-lookup"><span data-stu-id="b256e-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="b256e-481">Tarefa 5-Atualizando o modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="b256e-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="b256e-482">Nesta tarefa, você atualizará o modelo de exibição criado na última tarefa para recuperar o número de gêneros e seus nomes dentro da página.</span><span class="sxs-lookup"><span data-stu-id="b256e-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="b256e-483">Você usará @ Syntax (geralmente conhecido como &quot;código Nuggets&quot;) para executar código dentro do modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="b256e-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="b256e-484">No arquivo **index. cshtml** , dentro da pasta **Store** , substitua seu código pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="b256e-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. <span data-ttu-id="b256e-485">Faça um loop sobre a lista de gênero em **StoreIndexViewModel** e crie uma lista de **&gt;de&lt;UL** de HTML usando um loop **foreach** .</span><span class="sxs-lookup"><span data-stu-id="b256e-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="b256e-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="b256e-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="b256e-487">Pressione **F5** para executar o aplicativo e procurar **/Store**.</span><span class="sxs-lookup"><span data-stu-id="b256e-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="b256e-488">Você verá a lista de gêneros passados no objeto **StoreIndexViewModel** do **StoreController** para o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="b256e-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="b256e-489">![Exibir exibindo uma lista de gêneros](aspnet-mvc-4-fundamentals/_static/image26.png "Exibir exibindo uma lista de gêneros")</span><span class="sxs-lookup"><span data-stu-id="b256e-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="b256e-490">*Exibir exibindo uma lista de gêneros*</span><span class="sxs-lookup"><span data-stu-id="b256e-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="b256e-491">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="b256e-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="b256e-492">Exercício 6: usando parâmetros na exibição</span><span class="sxs-lookup"><span data-stu-id="b256e-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="b256e-493">No exercício 3, você aprendeu a passar parâmetros para o controlador.</span><span class="sxs-lookup"><span data-stu-id="b256e-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="b256e-494">Neste exercício, você aprenderá a usar esses parâmetros no modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="b256e-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="b256e-495">Para essa finalidade, você será apresentado primeiro a classes de modelo que o ajudarão a gerenciar seus dados e a lógica do domínio.</span><span class="sxs-lookup"><span data-stu-id="b256e-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="b256e-496">Além disso, você aprenderá a criar links para páginas dentro do aplicativo MVC ASP.NET sem se preocupar com a codificação de caminhos de URL.</span><span class="sxs-lookup"><span data-stu-id="b256e-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="b256e-497">Tarefa 1-adicionando classes de modelo</span><span class="sxs-lookup"><span data-stu-id="b256e-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="b256e-498">Ao contrário de ViewModels, que são criados apenas para passar informações do controlador para a exibição, as classes de modelo são criadas para conter e gerenciar dados e lógica de domínio.</span><span class="sxs-lookup"><span data-stu-id="b256e-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="b256e-499">Nesta tarefa, você adicionará duas classes de modelo para representar esses conceitos: **gênero** e **Album**.</span><span class="sxs-lookup"><span data-stu-id="b256e-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="b256e-500">Se ainda não estiver aberto, inicie **vs Express para Web**</span><span class="sxs-lookup"><span data-stu-id="b256e-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="b256e-501">No menu **arquivo** , escolha **Abrir projeto**.</span><span class="sxs-lookup"><span data-stu-id="b256e-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="b256e-502">Na caixa de diálogo Abrir projeto, navegue até **Source\Ex06-UsingParametersInView\Begin**, selecione **Iniciar. sln** e clique em **abrir**.</span><span class="sxs-lookup"><span data-stu-id="b256e-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="b256e-503">Como alternativa, você pode continuar com a solução obtida depois de concluir o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="b256e-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="b256e-504">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="b256e-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="b256e-505">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b256e-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="b256e-506">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="b256e-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="b256e-507">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="b256e-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b256e-508">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="b256e-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="b256e-509">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="b256e-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="b256e-510">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="b256e-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="b256e-511">Adicione uma classe de modelo de **gênero** .</span><span class="sxs-lookup"><span data-stu-id="b256e-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="b256e-512">Para fazer isso, clique com o botão direito do mouse na pasta **modelos** na **Gerenciador de soluções**, selecione **Adicionar** e, em seguida, a opção **novo item** .</span><span class="sxs-lookup"><span data-stu-id="b256e-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="b256e-513">Em **código**, escolha o item de **classe** e nomeie o arquivo *Genre.cs*e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b256e-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="b256e-514">![Adicionando uma classe](aspnet-mvc-4-fundamentals/_static/image27.png "Adicionando uma classe")</span><span class="sxs-lookup"><span data-stu-id="b256e-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="b256e-515">*Adicionando um novo item*</span><span class="sxs-lookup"><span data-stu-id="b256e-515">*Adding a new item*</span></span>

    <span data-ttu-id="b256e-516">![Adicionar classe de modelo de gênero](aspnet-mvc-4-fundamentals/_static/image28.png "Adicionar classe de modelo de gênero")</span><span class="sxs-lookup"><span data-stu-id="b256e-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="b256e-517">*Adicionar classe de modelo de gênero*</span><span class="sxs-lookup"><span data-stu-id="b256e-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="b256e-518">Adicione uma propriedade **Name** à classe gênero.</span><span class="sxs-lookup"><span data-stu-id="b256e-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="b256e-519">Para fazer isso, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="b256e-519">To do this, add the following code:</span></span>

    <span data-ttu-id="b256e-520">(Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex6 gênero*)</span><span class="sxs-lookup"><span data-stu-id="b256e-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="b256e-521">Seguindo o mesmo procedimento que antes, adicione uma classe de **álbum** .</span><span class="sxs-lookup"><span data-stu-id="b256e-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="b256e-522">Para fazer isso, clique com o botão direito do mouse na pasta **modelos** na **Gerenciador de soluções**, selecione **Adicionar** e, em seguida, a opção **novo item** .</span><span class="sxs-lookup"><span data-stu-id="b256e-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="b256e-523">Em **código**, escolha o item de **classe** e nomeie o arquivo *Album.cs*e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b256e-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="b256e-524">Adicione duas propriedades à classe Album: **gênero** e **title**.</span><span class="sxs-lookup"><span data-stu-id="b256e-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="b256e-525">Para fazer isso, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="b256e-525">To do this, add the following code:</span></span>

    <span data-ttu-id="b256e-526">(Trecho de código- *ASP.NET MVC 4 Fundamentals – álbum Ex6*)</span><span class="sxs-lookup"><span data-stu-id="b256e-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="b256e-527">Tarefa 2-Adicionando um StoreBrowseViewModel</span><span class="sxs-lookup"><span data-stu-id="b256e-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="b256e-528">Um **StoreBrowseViewModel** será usado nesta tarefa para mostrar os álbuns que correspondem a um gênero selecionado.</span><span class="sxs-lookup"><span data-stu-id="b256e-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="b256e-529">Nesta tarefa, você criará essa classe e, em seguida, adicionará duas propriedades para manipular o **gênero** e a lista do seu **álbum**.</span><span class="sxs-lookup"><span data-stu-id="b256e-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="b256e-530">Adicione uma classe **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="b256e-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="b256e-531">Para fazer isso, clique com o botão direito do mouse na pasta **ViewModels** na **Gerenciador de soluções**, selecione **Adicionar** e, em seguida, a opção **novo item** .</span><span class="sxs-lookup"><span data-stu-id="b256e-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="b256e-532">Em **código**, escolha o item de **classe** e nomeie o arquivo *StoreBrowseViewModel.cs*e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b256e-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="b256e-533">Adicione uma referência aos modelos na classe **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="b256e-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="b256e-534">Para fazer isso, adicione o seguinte usando o namespace:</span><span class="sxs-lookup"><span data-stu-id="b256e-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="b256e-535">(Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="b256e-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="b256e-536">Adicione duas propriedades à classe **StoreBrowseViewModel** : **gênero** e **álbuns**.</span><span class="sxs-lookup"><span data-stu-id="b256e-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="b256e-537">Para fazer isso, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="b256e-537">To do this, add the following code:</span></span>

    <span data-ttu-id="b256e-538">(Trecho de código- *ASP.NET MVC 4 Fundamentals – Ex6 modelproperties*)</span><span class="sxs-lookup"><span data-stu-id="b256e-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="b256e-539">O que é a **lista&lt;álbum&gt;** ?: essa definição está usando a **lista&lt;t&gt;** tipo, em que **t** restringe o tipo ao qual os elementos dessa **lista** pertencem, neste caso, o **álbum** (ou qualquer um de seus descendentes).</span><span class="sxs-lookup"><span data-stu-id="b256e-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="b256e-540">Essa capacidade de criar classes e métodos que adiam a especificação de um ou mais tipos até que a classe ou o método seja declarado e instanciado pelo código do cliente é C# um recurso do idioma chamado **genéricos**.</span><span class="sxs-lookup"><span data-stu-id="b256e-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="b256e-541">**List&lt;t&gt;** é o equivalente genérico do tipo **ArrayList** e está disponível no namespace **System. Collections. Generic** .</span><span class="sxs-lookup"><span data-stu-id="b256e-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="b256e-542">Um dos benefícios de usar os **genéricos** é que, como o tipo é especificado, você não precisa cuidar das operações de verificação de tipo, como a conversão dos elementos no **álbum** como faria com uma **ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="b256e-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="b256e-543">Tarefa 3-usando o novo ViewModel no StoreController</span><span class="sxs-lookup"><span data-stu-id="b256e-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="b256e-544">Nesta tarefa, você modificará os métodos de ação **procurar** e **detalhes** do **StoreController**para usar o novo **StoreBrowseViewModel**.</span><span class="sxs-lookup"><span data-stu-id="b256e-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="b256e-545">Adicione uma referência à pasta modelos na classe **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="b256e-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="b256e-546">Para fazer isso, expanda a pasta **controladores** no **Gerenciador de soluções** e abra a classe **StoreController** .</span><span class="sxs-lookup"><span data-stu-id="b256e-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="b256e-547">Em seguida, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="b256e-547">Then add the following code:</span></span>

    <span data-ttu-id="b256e-548">(Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="b256e-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="b256e-549">Substitua o método de ação **procurar** para usar a classe **StoreViewBrowseController** .</span><span class="sxs-lookup"><span data-stu-id="b256e-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="b256e-550">Você criará um gênero e dois novos objetos de álbuns com dados fictícios (no próximo laboratório prático, você consumirá dados reais de um banco de dado).</span><span class="sxs-lookup"><span data-stu-id="b256e-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="b256e-551">Para fazer isso, substitua o método **Browse** pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="b256e-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="b256e-552">(Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="b256e-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="b256e-553">Substitua o método de ação de **detalhes** para usar a classe **StoreViewBrowseController** .</span><span class="sxs-lookup"><span data-stu-id="b256e-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="b256e-554">Você criará um novo objeto de **álbum** a ser retornado para a **exibição**.</span><span class="sxs-lookup"><span data-stu-id="b256e-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="b256e-555">Para fazer isso, substitua o método **Details** pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="b256e-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="b256e-556">(Trecho de código- *ASP.NET MVC 4 Fundamentals-Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="b256e-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="b256e-557">Tarefa 4-adicionando um modelo de exibição de navegação</span><span class="sxs-lookup"><span data-stu-id="b256e-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="b256e-558">Nesta tarefa, você adicionará um modo de exibição de **navegação** para mostrar os álbuns encontrados para um gênero específico.</span><span class="sxs-lookup"><span data-stu-id="b256e-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="b256e-559">Antes de criar o novo modelo de exibição, você deve compilar o projeto para que a caixa de diálogo **Adicionar exibição** saiba sobre a classe **ViewModel** a ser usada.</span><span class="sxs-lookup"><span data-stu-id="b256e-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="b256e-560">Crie o projeto selecionando o item de menu **Build** e, em seguida, **Build MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="b256e-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="b256e-561">Adicione um modo de exibição de **navegação** .</span><span class="sxs-lookup"><span data-stu-id="b256e-561">Add a **Browse** View.</span></span> <span data-ttu-id="b256e-562">Para fazer isso, clique com o botão direito do mouse no método de ação **procurar** do **StoreController** e clique em **Adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="b256e-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="b256e-563">Na caixa de diálogo **Adicionar exibição** , verifique se o nome da exibição é **procurar**.</span><span class="sxs-lookup"><span data-stu-id="b256e-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="b256e-564">Marque a caixa de seleção **criar uma exibição fortemente tipada** e selecione **StoreBrowseViewModel** na lista suspensa **classe de modelo** .</span><span class="sxs-lookup"><span data-stu-id="b256e-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="b256e-565">Deixe os outros campos com o valor padrão.</span><span class="sxs-lookup"><span data-stu-id="b256e-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="b256e-566">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b256e-566">Then click **Add**.</span></span>

    <span data-ttu-id="b256e-567">![Adicionando um modo de exibição de navegação](aspnet-mvc-4-fundamentals/_static/image29.png "Adicionando um modo de exibição de navegação")</span><span class="sxs-lookup"><span data-stu-id="b256e-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="b256e-568">*Adicionando um modo de exibição de navegação*</span><span class="sxs-lookup"><span data-stu-id="b256e-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="b256e-569">Modifique o **Browse. cshtml** para exibir as informações do gênero, acessando o objeto **StoreBrowseViewModel** que é passado para o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="b256e-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="b256e-570">Para fazer isso, substitua o conteúdo pelo seguinte: (C#)</span><span class="sxs-lookup"><span data-stu-id="b256e-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="b256e-571">Tarefa 5-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="b256e-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="b256e-572">Nesta tarefa, você testará que o método **Browse** recupera álbuns da ação do método **procurar** .</span><span class="sxs-lookup"><span data-stu-id="b256e-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="b256e-573">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b256e-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b256e-574">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="b256e-574">The project starts in the Home page.</span></span> <span data-ttu-id="b256e-575">Alterar a URL para **/Store/Browse? Gênero = disco** para verificar se a ação retorna dois álbuns.</span><span class="sxs-lookup"><span data-stu-id="b256e-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="b256e-576">![Pesquisar álbuns do disco do repositório](aspnet-mvc-4-fundamentals/_static/image30.png "Pesquisar álbuns do disco do repositório")</span><span class="sxs-lookup"><span data-stu-id="b256e-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="b256e-577">*Pesquisar álbuns do disco do repositório*</span><span class="sxs-lookup"><span data-stu-id="b256e-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="b256e-578">Tarefa 6-exibindo informações sobre um álbum específico</span><span class="sxs-lookup"><span data-stu-id="b256e-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="b256e-579">Nesta tarefa, você implementará a exibição de **Armazenamento/detalhes** para exibir informações sobre um álbum específico.</span><span class="sxs-lookup"><span data-stu-id="b256e-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="b256e-580">Neste laboratório prático, tudo o que você exibirá sobre o álbum já está contido no modelo de **exibição** .</span><span class="sxs-lookup"><span data-stu-id="b256e-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="b256e-581">Portanto, em vez de criar uma classe **StoreDetailsViewModel** , você usará o modelo **StoreBrowseViewModel** atual passando o álbum para ele.</span><span class="sxs-lookup"><span data-stu-id="b256e-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="b256e-582">Feche o navegador, se necessário, para retornar à janela do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b256e-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="b256e-583">Adicione uma nova exibição de **detalhes** para o método de ação **detalhes** do **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="b256e-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="b256e-584">Para fazer isso, clique com o botão direito do mouse no método **Details** na classe **StoreController** e clique em **Adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="b256e-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="b256e-585">Na caixa de diálogo **Adicionar exibição** , verifique se o **nome da exibição** é **detalhes**.</span><span class="sxs-lookup"><span data-stu-id="b256e-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="b256e-586">Marque a caixa de seleção **criar uma exibição fortemente tipada** e selecione **álbum** na lista suspensa **classe de modelo** .</span><span class="sxs-lookup"><span data-stu-id="b256e-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="b256e-587">Deixe os outros campos com o valor padrão.</span><span class="sxs-lookup"><span data-stu-id="b256e-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="b256e-588">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b256e-588">Then click **Add**.</span></span> <span data-ttu-id="b256e-589">Isso criará e abrirá um arquivo **\Views\Store\Details.cshtml** .</span><span class="sxs-lookup"><span data-stu-id="b256e-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="b256e-590">![Adicionando uma exibição de detalhes](aspnet-mvc-4-fundamentals/_static/image31.png "Adicionando uma exibição de detalhes")</span><span class="sxs-lookup"><span data-stu-id="b256e-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="b256e-591">*Adicionando uma exibição de detalhes*</span><span class="sxs-lookup"><span data-stu-id="b256e-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="b256e-592">Modifique o arquivo **Details. cshtml** para exibir as informações do álbum, acessando o objeto de **álbum** que é passado para o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="b256e-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="b256e-593">Para fazer isso, substitua o conteúdo pelo seguinte: (C#)</span><span class="sxs-lookup"><span data-stu-id="b256e-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="b256e-594">Tarefa 7-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="b256e-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="b256e-595">Nesta tarefa, você testará que a exibição de **detalhes** recupera as informações do álbum do método de **ação de detalhes** .</span><span class="sxs-lookup"><span data-stu-id="b256e-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="b256e-596">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b256e-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b256e-597">O projeto é iniciado na **Home** Page.</span><span class="sxs-lookup"><span data-stu-id="b256e-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="b256e-598">Altere a URL para **/Store/Details/5** para verificar as informações do álbum.</span><span class="sxs-lookup"><span data-stu-id="b256e-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="b256e-599">![Detalhando os álbuns de navegação](aspnet-mvc-4-fundamentals/_static/image32.png "Detalhando os álbuns de navegação")</span><span class="sxs-lookup"><span data-stu-id="b256e-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="b256e-600">*Pesquisando detalhes do álbum*</span><span class="sxs-lookup"><span data-stu-id="b256e-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="b256e-601">Tarefa 8-adicionando links entre páginas</span><span class="sxs-lookup"><span data-stu-id="b256e-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="b256e-602">Nesta tarefa, você adicionará um link na exibição da loja para ter um link em cada nome de gênero para a URL **/Store/Browse** apropriada.</span><span class="sxs-lookup"><span data-stu-id="b256e-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="b256e-603">Dessa forma, quando você clicar em um gênero, por exemplo, o **disco**será navegado para **/Store/Browse? gênero =** URL de disco.</span><span class="sxs-lookup"><span data-stu-id="b256e-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="b256e-604">Feche o navegador, se necessário, para retornar à janela do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b256e-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="b256e-605">Atualize a página de **índice** para adicionar um link à página de **navegação** .</span><span class="sxs-lookup"><span data-stu-id="b256e-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="b256e-606">Para fazer isso, no **Gerenciador de soluções** expanda a pasta **exibições** , em seguida, a pasta de **armazenamento** e clique duas vezes na página **index. cshtml** .</span><span class="sxs-lookup"><span data-stu-id="b256e-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="b256e-607">Adicione um link para o modo de exibição de navegação indicando o gênero selecionado.</span><span class="sxs-lookup"><span data-stu-id="b256e-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="b256e-608">Para fazer isso, substitua o seguinte código realçado dentro das marcas de **&gt;&lt;li** : (C#)</span><span class="sxs-lookup"><span data-stu-id="b256e-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="b256e-609">outra abordagem seria vincular diretamente à página, com um código semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="b256e-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="b256e-610">&lt;a href =&quot;/Store/Browse? gênero =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="b256e-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="b256e-611">Embora essa abordagem funcione, ela depende de uma cadeia de caracteres codificada.</span><span class="sxs-lookup"><span data-stu-id="b256e-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="b256e-612">Se posteriormente você renomear o controlador, precisará alterar essa instrução manualmente.</span><span class="sxs-lookup"><span data-stu-id="b256e-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="b256e-613">Uma alternativa melhor é usar um método **auxiliar HTML** .</span><span class="sxs-lookup"><span data-stu-id="b256e-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="b256e-614">O ASP.NET MVC inclui um método auxiliar HTML que está disponível para essas tarefas.</span><span class="sxs-lookup"><span data-stu-id="b256e-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="b256e-615">O método auxiliar **HTML. ActionLink ()** facilita a criação **de html&lt;um&gt;** links, garantindo que os caminhos de URL sejam codificados corretamente em URL.</span><span class="sxs-lookup"><span data-stu-id="b256e-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="b256e-616">HTML. ActionLink tem várias sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="b256e-616">Html.ActionLink has several overloads.</span></span> <span data-ttu-id="b256e-617">Neste exercício, você usará um que usa três parâmetros:</span><span class="sxs-lookup"><span data-stu-id="b256e-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="b256e-618">Texto do link, que exibirá o nome do gênero</span><span class="sxs-lookup"><span data-stu-id="b256e-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="b256e-619">Nome da ação do controlador (**procurar**)</span><span class="sxs-lookup"><span data-stu-id="b256e-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="b256e-620">Rotear valores de parâmetro, especificando o nome (**gênero**) e o valor (**nome do gênero**)</span><span class="sxs-lookup"><span data-stu-id="b256e-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="b256e-621">Tarefa 9-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="b256e-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="b256e-622">Nesta tarefa, você testará que cada gênero é exibido com um link para a URL **/Store/Browse** apropriada.</span><span class="sxs-lookup"><span data-stu-id="b256e-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="b256e-623">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b256e-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b256e-624">O projeto é iniciado na Home Page.</span><span class="sxs-lookup"><span data-stu-id="b256e-624">The project starts in the Home page.</span></span> <span data-ttu-id="b256e-625">Altere a URL para **/Store** para verificar se cada gênero está vinculado à URL **/Store/Browse** apropriada.</span><span class="sxs-lookup"><span data-stu-id="b256e-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="b256e-626">![Navegando em gêneros com links para a página de navegação](aspnet-mvc-4-fundamentals/_static/image33.png "Navegando em gêneros com links para a página de navegação")</span><span class="sxs-lookup"><span data-stu-id="b256e-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="b256e-627">*Navegando em gêneros com links para a página de navegação*</span><span class="sxs-lookup"><span data-stu-id="b256e-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="b256e-628">Tarefa 10-usando a coleção ViewModel dinâmica para passar valores</span><span class="sxs-lookup"><span data-stu-id="b256e-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="b256e-629">Nesta tarefa, você aprenderá um método simples e poderoso para passar valores entre o controlador e a exibição sem fazer nenhuma alteração no modelo.</span><span class="sxs-lookup"><span data-stu-id="b256e-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="b256e-630">O ASP.NET MVC 4 fornece a coleção &quot;ViewModel&quot;, que pode ser atribuída a qualquer valor dinâmico e acessado dentro de controladores e exibições também.</span><span class="sxs-lookup"><span data-stu-id="b256e-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="b256e-631">Agora, você usará a coleção dinâmica ViewBag para passar uma lista de &quot;**gêneros estrelado**&quot; do controlador para a exibição.</span><span class="sxs-lookup"><span data-stu-id="b256e-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="b256e-632">A exibição do índice de repositório será acessada pelo **ViewModel** e exibirá as informações.</span><span class="sxs-lookup"><span data-stu-id="b256e-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="b256e-633">Feche o navegador, se necessário, para retornar à janela do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b256e-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="b256e-634">Abra **StoreController.cs** e modifique o método **index** para criar uma lista de gêneros estrelado na coleção ViewModel:</span><span class="sxs-lookup"><span data-stu-id="b256e-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="b256e-635">Você também pode usar a sintaxe **ViewBag [&quot;estrelado&quot;]** para acessar as propriedades.</span><span class="sxs-lookup"><span data-stu-id="b256e-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="b256e-636">O ícone de estrela **&quot;o&quot;estrelado. png** está incluído na pasta **Source\Assets\Images** deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="b256e-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="b256e-637">Para adicioná-lo ao aplicativo, arraste o conteúdo de uma janela do **Windows Explorer** para o **Gerenciador de soluções** no Visual Web Developer Express, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="b256e-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="b256e-638">![Adicionando imagem em estrela à solução](aspnet-mvc-4-fundamentals/_static/image34.png "Adicionando imagem em estrela à solução")</span><span class="sxs-lookup"><span data-stu-id="b256e-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="b256e-639">*Adicionando imagem em estrela à solução*</span><span class="sxs-lookup"><span data-stu-id="b256e-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="b256e-640">Abra a exibição **Store/index. cshtml** e modifique o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="b256e-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="b256e-641">Você lerá a propriedade &quot;estrelado&quot; na coleção **ViewBag** e perguntará se o nome do gênero atual está na lista.</span><span class="sxs-lookup"><span data-stu-id="b256e-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="b256e-642">Nesse caso, você mostrará um ícone de estrela à direita para o link de gênero.</span><span class="sxs-lookup"><span data-stu-id="b256e-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="b256e-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="b256e-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="b256e-644">Tarefa 11-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="b256e-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="b256e-645">Nesta tarefa, você testará que os gêneros estrelado exibem um ícone de estrela.</span><span class="sxs-lookup"><span data-stu-id="b256e-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="b256e-646">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b256e-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="b256e-647">O projeto é iniciado na **Home** Page.</span><span class="sxs-lookup"><span data-stu-id="b256e-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="b256e-648">Altere a URL para **/Store** para verificar se cada gênero em destaque tem o rótulo de respeito:</span><span class="sxs-lookup"><span data-stu-id="b256e-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="b256e-649">![Navegando em gêneros com elementos estrelado](aspnet-mvc-4-fundamentals/_static/image35.png "Navegando em gêneros com elementos estrelado")</span><span class="sxs-lookup"><span data-stu-id="b256e-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="b256e-650">*Navegando em gêneros com elementos estrelado*</span><span class="sxs-lookup"><span data-stu-id="b256e-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="b256e-651">Exercício 7: um colo sobre o novo modelo do MVC 4 do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b256e-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="b256e-652">Neste exercício, você explorará os aprimoramentos nos modelos de projeto do ASP.NET MVC 4, dando uma olhada nos recursos mais relevantes do novo modelo.</span><span class="sxs-lookup"><span data-stu-id="b256e-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="b256e-653">Tarefa 1: explorando o modelo de aplicativo de Internet do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="b256e-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="b256e-654">Se ainda não estiver aberto, inicie **vs Express para Web**</span><span class="sxs-lookup"><span data-stu-id="b256e-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="b256e-655">Selecione o **arquivo | Novo |** Comando de menu do projeto.</span><span class="sxs-lookup"><span data-stu-id="b256e-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="b256e-656">Na caixa de diálogo **novo projeto** , selecione **o C#Visual | Modelo da Web** na árvore do painel esquerdo e escolha o **aplicativo Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="b256e-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="b256e-657">**Nomeie** o projeto *MusicStore* e atualize o **nome da solução** para *começar*, em seguida, selecione um local (ou deixe o padrão) e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="b256e-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="b256e-658">![Criando um novo projeto do ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "Criando um novo projeto do ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="b256e-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="b256e-659">*Criando um novo projeto do ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="b256e-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="b256e-660">Na caixa de diálogo **novo projeto do ASP.NET MVC 4** , selecione o modelo de projeto de **aplicativo da Internet** e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="b256e-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="b256e-661">Observe que você pode selecionar o Razor ou o ASPX como o mecanismo de exibição.</span><span class="sxs-lookup"><span data-stu-id="b256e-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="b256e-662">![Criando um novo aplicativo de Internet ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image37.png "Criando um novo aplicativo de Internet ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="b256e-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="b256e-663">*Criando um novo aplicativo de Internet ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="b256e-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="b256e-664">Sintaxe Razor foi introduzido no ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="b256e-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="b256e-665">Seu objetivo é minimizar o número de caracteres e os pressionamentos de teclas necessários em um arquivo, permitindo um fluxo de trabalho de codificação rápido e fluido.</span><span class="sxs-lookup"><span data-stu-id="b256e-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="b256e-666">O Razor aproveita as C#habilidades da linguagem/vb (ou outras) existentes e fornece uma sintaxe de marcação de modelo que habilita um fluxo de trabalho de construção HTML incrível.</span><span class="sxs-lookup"><span data-stu-id="b256e-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="b256e-667">Pressione **F5** para executar a solução e ver o modelo renovado.</span><span class="sxs-lookup"><span data-stu-id="b256e-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="b256e-668">Você pode conferir os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="b256e-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="b256e-669">**Modelos de estilo moderno**</span><span class="sxs-lookup"><span data-stu-id="b256e-669">**Modern-style templates**</span></span>

        <span data-ttu-id="b256e-670">Os modelos foram renovados, fornecendo mais estilos de aparência moderna.</span><span class="sxs-lookup"><span data-stu-id="b256e-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="b256e-671">![Modelos reestilizados do ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image38.png "Modelos reestilizados do ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="b256e-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="b256e-672">*Modelos reestilizados do ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="b256e-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="b256e-673">**Renderização adaptável**</span><span class="sxs-lookup"><span data-stu-id="b256e-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="b256e-674">Confira redimensionar a janela do navegador e observe como o layout da página se adapta dinamicamente ao novo tamanho da janela.</span><span class="sxs-lookup"><span data-stu-id="b256e-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="b256e-675">Esses modelos usam a técnica de renderização adaptável para renderizar adequadamente nas plataformas desktop e móvel sem qualquer personalização.</span><span class="sxs-lookup"><span data-stu-id="b256e-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="b256e-676">![Modelo de projeto ASP.NET MVC 4 em diferentes tamanhos de navegador](aspnet-mvc-4-fundamentals/_static/image39.png "Modelo de projeto ASP.NET MVC 4 em diferentes tamanhos de navegador")</span><span class="sxs-lookup"><span data-stu-id="b256e-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="b256e-677">*Modelo de projeto ASP.NET MVC 4 em diferentes tamanhos de navegador*</span><span class="sxs-lookup"><span data-stu-id="b256e-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="b256e-678">Feche o navegador para parar o depurador e retornar ao Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b256e-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="b256e-679">Agora você pode explorar a solução e conferir alguns dos novos recursos introduzidos pelo ASP.NET MVC 4 no modelo de projeto.</span><span class="sxs-lookup"><span data-stu-id="b256e-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="b256e-680">![ASP.NET MVC4-Internet-Application-Project-template](aspnet-mvc-4-fundamentals/_static/image40.png "O modelo de projeto de aplicativo de Internet do ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="b256e-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="b256e-681">*O modelo de projeto de aplicativo de Internet do ASP.NET MVC 4*</span><span class="sxs-lookup"><span data-stu-id="b256e-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="b256e-682">**Marcação HTML5**</span><span class="sxs-lookup"><span data-stu-id="b256e-682">**HTML5 markup**</span></span>

       <span data-ttu-id="b256e-683">Procurar exibições de modelo para descobrir a nova marcação de tema, por exemplo, abra a exibição **about. cshtml** na pasta **base** .</span><span class="sxs-lookup"><span data-stu-id="b256e-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="b256e-684">![Novo modelo, usando marcação Razor e HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "Novo modelo, usando marcação Razor e HTML5")</span><span class="sxs-lookup"><span data-stu-id="b256e-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="b256e-685">*Novo modelo, usando marcação Razor e HTML5*</span><span class="sxs-lookup"><span data-stu-id="b256e-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="b256e-686">**Bibliotecas JavaScript incluídas**</span><span class="sxs-lookup"><span data-stu-id="b256e-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="b256e-687">**jQuery**: o jQuery simplifica a travessia de documentos HTML, manipulação de eventos, animação e interações com AJAX.</span><span class="sxs-lookup"><span data-stu-id="b256e-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="b256e-688">**interface do usuário do jQuery**: essa biblioteca fornece abstrações para interação e animação de baixo nível, efeitos avançados e widgets passíveis, criados sobre a biblioteca JavaScript do jQuery.</span><span class="sxs-lookup"><span data-stu-id="b256e-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="b256e-689">Você pode aprender sobre o jQuery e a interface do usuário do jQuery no [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="b256e-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="b256e-690">**KnockoutJS**: o modelo padrão ASP.NET MVC 4 agora inclui o **KnockoutJS**, uma estrutura MVVM do JavaScript que permite criar aplicativos Web avançados e altamente responsivos usando JavaScript e HTML.</span><span class="sxs-lookup"><span data-stu-id="b256e-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="b256e-691">Como nas bibliotecas de interface do usuário do ASP.NET MVC 3, jQuery e jQuery também estão incluídas no ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b256e-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="b256e-692">Você pode obter mais informações sobre a biblioteca KnockOutJS neste link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="b256e-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="b256e-693">**Modernizr**: essa biblioteca é executada automaticamente, tornando seu site compatível com navegadores mais antigos ao usar as tecnologias HTML5 e CSS3.</span><span class="sxs-lookup"><span data-stu-id="b256e-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="b256e-694">Você pode obter mais informações sobre a biblioteca do Modernizr neste link: [http://www.modernizr.com/](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="b256e-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="b256e-695">**SimpleMembership incluídos na solução**</span><span class="sxs-lookup"><span data-stu-id="b256e-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="b256e-696">O SimpleMembership foi projetado como uma substituição para a função ASP.NET anterior e o sistema do provedor de associação.</span><span class="sxs-lookup"><span data-stu-id="b256e-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="b256e-697">Ele tem muitos recursos novos que tornam mais fácil para o desenvolvedor proteger páginas da Web de maneira mais flexível.</span><span class="sxs-lookup"><span data-stu-id="b256e-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="b256e-698">O modelo da Internet já configurou algumas coisas para integrar o SimpleMembership, por exemplo, o AccountController está preparado para usar o OAuthWebSecurity (para registro de conta OAuth, logon, gerenciamento, etc.) e segurança da Web.</span><span class="sxs-lookup"><span data-stu-id="b256e-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="b256e-699">![SimpleMembership incluídos na solução](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership incluídos na solução")</span><span class="sxs-lookup"><span data-stu-id="b256e-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="b256e-700">*SimpleMembership incluídos na solução*</span><span class="sxs-lookup"><span data-stu-id="b256e-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="b256e-701">Encontre mais informações sobre o [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) no msdn.</span><span class="sxs-lookup"><span data-stu-id="b256e-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="b256e-702">Além disso, você pode implantar esse aplicativo em sites do Windows Azure seguindo [o apêndice B: publicando um aplicativo ASP.NET MVC 4 usando implantação da Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="b256e-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="b256e-703">Resumo</span><span class="sxs-lookup"><span data-stu-id="b256e-703">Summary</span></span>

<span data-ttu-id="b256e-704">Ao concluir este laboratório prático, você aprendeu os conceitos básicos do ASP.NET MVC:</span><span class="sxs-lookup"><span data-stu-id="b256e-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="b256e-705">Os principais elementos de um aplicativo MVC e como eles interagem</span><span class="sxs-lookup"><span data-stu-id="b256e-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="b256e-706">Como criar um aplicativo MVC do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b256e-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="b256e-707">Como adicionar e configurar controladores para lidar com parâmetros passados por meio da URL e da QueryString</span><span class="sxs-lookup"><span data-stu-id="b256e-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="b256e-708">Como adicionar uma página mestra de layout para configurar um modelo para conteúdo HTML comum, uma folha de estilos para aprimorar a aparência e um modelo de exibição para exibir conteúdo HTML</span><span class="sxs-lookup"><span data-stu-id="b256e-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="b256e-709">Como usar o padrão ViewModel para passar Propriedades para o modelo de exibição para exibir informações dinâmicas</span><span class="sxs-lookup"><span data-stu-id="b256e-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="b256e-710">Como usar parâmetros passados para controladores no modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="b256e-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="b256e-711">Como adicionar links a páginas dentro do aplicativo MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b256e-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="b256e-712">Como adicionar e usar propriedades dinâmicas em uma exibição</span><span class="sxs-lookup"><span data-stu-id="b256e-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="b256e-713">Os aprimoramentos nos modelos de projeto do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="b256e-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="b256e-714">Apêndice A: Instalando o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="b256e-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="b256e-715">Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou outra versão &quot;Express&quot; usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="b256e-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="b256e-716">As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="b256e-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="b256e-717">Vá para [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="b256e-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="b256e-718">Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e pesquisar o produto &quot;<em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="b256e-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="b256e-719">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="b256e-719">Click on **Install Now**.</span></span> <span data-ttu-id="b256e-720">Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="b256e-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="b256e-721">Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="b256e-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="b256e-722">![Instalar Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="b256e-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="b256e-723">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="b256e-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="b256e-724">Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="b256e-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitando os termos de licença](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="b256e-726">*Aceitando os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="b256e-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="b256e-727">Aguarde até que o processo de download e instalação seja concluído.</span><span class="sxs-lookup"><span data-stu-id="b256e-727">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="b256e-729">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="b256e-729">*Installation progress*</span></span>
6. <span data-ttu-id="b256e-730">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="b256e-730">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="b256e-732">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="b256e-732">*Installation completed*</span></span>
7. <span data-ttu-id="b256e-733">Clique em **sair** para fechar Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="b256e-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="b256e-734">Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot;**vs Express**&quot;e, em seguida, clique no bloco **vs Express para Web** .</span><span class="sxs-lookup"><span data-stu-id="b256e-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Bloco VS Express para Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="b256e-736">*Bloco VS Express para Web*</span><span class="sxs-lookup"><span data-stu-id="b256e-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="b256e-737">Apêndice B: publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web</span><span class="sxs-lookup"><span data-stu-id="b256e-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="b256e-738">Este apêndice mostrará como criar um novo site da Web do Windows Azure Portal de Gerenciamento e publicar o aplicativo obtido seguindo o laboratório, aproveitando o recurso de publicação de Implantação da Web fornecido pelo Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b256e-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="b256e-739">Tarefa 1-Criando um novo site no portal do Windows Azure</span><span class="sxs-lookup"><span data-stu-id="b256e-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="b256e-740">Acesse o [portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="b256e-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b256e-741">Com o Windows Azure, você pode hospedar 10 sites da Web de ASP.NET gratuitamente e, em seguida, dimensionar conforme o tráfego cresce.</span><span class="sxs-lookup"><span data-stu-id="b256e-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="b256e-742">Você pode se inscrever [aqui](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="b256e-742">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="b256e-743">![Fazer logon no Windows portal do Azure](aspnet-mvc-4-fundamentals/_static/image48.png "Fazer logon no Windows portal do Azure")</span><span class="sxs-lookup"><span data-stu-id="b256e-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="b256e-744">*Fazer logon no Windows Azure Portal de Gerenciamento*</span><span class="sxs-lookup"><span data-stu-id="b256e-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="b256e-745">Clique em **novo** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="b256e-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="b256e-746">![Criando um novo site](aspnet-mvc-4-fundamentals/_static/image49.png "Criando um novo site")</span><span class="sxs-lookup"><span data-stu-id="b256e-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="b256e-747">*Criando um novo site*</span><span class="sxs-lookup"><span data-stu-id="b256e-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="b256e-748">Clique em **computação** | **site**.</span><span class="sxs-lookup"><span data-stu-id="b256e-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="b256e-749">Em seguida, selecione a opção **criação rápida** .</span><span class="sxs-lookup"><span data-stu-id="b256e-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="b256e-750">Forneça uma URL disponível para o novo site e clique em **criar site**.</span><span class="sxs-lookup"><span data-stu-id="b256e-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b256e-751">Um site do Windows Azure é o host de um aplicativo Web em execução na nuvem que você pode controlar e gerenciar.</span><span class="sxs-lookup"><span data-stu-id="b256e-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="b256e-752">A opção criação rápida permite que você implante um aplicativo Web concluído no site do Windows Azure de fora do Portal.</span><span class="sxs-lookup"><span data-stu-id="b256e-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="b256e-753">Ele não inclui etapas para configurar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b256e-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="b256e-754">![Criando um novo site usando a criação rápida](aspnet-mvc-4-fundamentals/_static/image50.png "Criando um novo site usando a criação rápida")</span><span class="sxs-lookup"><span data-stu-id="b256e-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="b256e-755">*Criando um novo site usando a criação rápida*</span><span class="sxs-lookup"><span data-stu-id="b256e-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="b256e-756">Aguarde até que o novo **site** seja criado.</span><span class="sxs-lookup"><span data-stu-id="b256e-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="b256e-757">Depois que o site for criado, clique no link sob a coluna **URL** .</span><span class="sxs-lookup"><span data-stu-id="b256e-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="b256e-758">Verifique se o novo site está funcionando.</span><span class="sxs-lookup"><span data-stu-id="b256e-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="b256e-759">![Navegando até o novo site](aspnet-mvc-4-fundamentals/_static/image51.png "Navegando até o novo site")</span><span class="sxs-lookup"><span data-stu-id="b256e-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="b256e-760">*Navegando até o novo site*</span><span class="sxs-lookup"><span data-stu-id="b256e-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="b256e-761">![Site em execução](aspnet-mvc-4-fundamentals/_static/image52.png "Site em execução")</span><span class="sxs-lookup"><span data-stu-id="b256e-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="b256e-762">*Site em execução*</span><span class="sxs-lookup"><span data-stu-id="b256e-762">*Web site running*</span></span>
6. <span data-ttu-id="b256e-763">Volte para o portal e clique no nome do site na coluna **nome** para exibir as páginas de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="b256e-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="b256e-764">![Abrindo as páginas de gerenciamento de site](aspnet-mvc-4-fundamentals/_static/image53.png "Abrindo as páginas de gerenciamento de site")</span><span class="sxs-lookup"><span data-stu-id="b256e-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="b256e-765">*Abrindo as páginas de gerenciamento de site*</span><span class="sxs-lookup"><span data-stu-id="b256e-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="b256e-766">Na página **painel** , na seção **visão rápida** , clique no link **baixar perfil de publicação** .</span><span class="sxs-lookup"><span data-stu-id="b256e-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b256e-767">O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo Web em um site do Windows Azure para cada método de publicação habilitado.</span><span class="sxs-lookup"><span data-stu-id="b256e-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="b256e-768">O perfil de publicação contém as URLs, as credenciais de usuário e as cadeias de conexão de banco de dados necessárias para conectar-se e autenticar cada um dos pontos de extremidade para os quais um método de publicação é habilitado.</span><span class="sxs-lookup"><span data-stu-id="b256e-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="b256e-769">**O Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** e **Microsoft Visual Studio 2012** dão suporte à leitura de perfis de publicação para automatizar a configuração desses programas para a publicação de aplicativos Web em sites do Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b256e-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="b256e-770">![Baixando o perfil de publicação do site](aspnet-mvc-4-fundamentals/_static/image54.png "Baixando o perfil de publicação do site")</span><span class="sxs-lookup"><span data-stu-id="b256e-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="b256e-771">*Baixando o perfil de publicação do site*</span><span class="sxs-lookup"><span data-stu-id="b256e-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="b256e-772">Baixe o arquivo de perfil de publicação em um local conhecido.</span><span class="sxs-lookup"><span data-stu-id="b256e-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="b256e-773">Neste exercício, você verá como usar esse arquivo para publicar um aplicativo Web em um site do Windows Azure a partir do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b256e-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="b256e-774">![Salvando o arquivo de perfil de publicação](aspnet-mvc-4-fundamentals/_static/image55.png "Salvando o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="b256e-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="b256e-775">*Salvando o arquivo de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="b256e-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="b256e-776">Tarefa 2-Configurando o servidor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="b256e-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="b256e-777">Se seu aplicativo utiliza bancos de dados SQL Server, você precisará criar um servidor de banco de dados SQL.</span><span class="sxs-lookup"><span data-stu-id="b256e-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="b256e-778">Se você quiser implantar um aplicativo simples que não usa SQL Server você pode ignorar essa tarefa.</span><span class="sxs-lookup"><span data-stu-id="b256e-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="b256e-779">Você precisará de um servidor do banco de dados SQL para armazenar o banco de dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b256e-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="b256e-780">Você pode exibir os servidores do banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em bancos de dados **sql** | **servidores** | **painel do servidor**.</span><span class="sxs-lookup"><span data-stu-id="b256e-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="b256e-781">Se você não tiver um servidor criado, poderá criar um usando o botão **Adicionar** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="b256e-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="b256e-782">Anote o nome do **servidor e a URL, o nome de logon e a senha do administrador**, pois você irá usá-los nas próximas tarefas.</span><span class="sxs-lookup"><span data-stu-id="b256e-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="b256e-783">Não crie o banco de dados ainda, pois ele será criado em um estágio posterior.</span><span class="sxs-lookup"><span data-stu-id="b256e-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="b256e-784">![Painel do servidor do banco de dados SQL](aspnet-mvc-4-fundamentals/_static/image56.png "Painel do servidor do banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="b256e-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="b256e-785">*Painel do servidor do banco de dados SQL*</span><span class="sxs-lookup"><span data-stu-id="b256e-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="b256e-786">Na próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, será necessário incluir o endereço IP local na lista de **endereços IP permitidos**do servidor.</span><span class="sxs-lookup"><span data-stu-id="b256e-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="b256e-787">Para fazer isso, clique em **Configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o nas caixas de texto endereço IP **inicial** e **endereço IP final** e clique no botão ![adicionar-cliente-IP-endereço-OK-botão](aspnet-mvc-4-fundamentals/_static/image57.png).</span><span class="sxs-lookup"><span data-stu-id="b256e-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![Adicionando endereço IP do cliente](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="b256e-789">*Adicionando endereço IP do cliente*</span><span class="sxs-lookup"><span data-stu-id="b256e-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="b256e-790">Depois que o **endereço IP do cliente** for adicionado à lista endereços IP permitidos, clique em **salvar** para confirmar as alterações.</span><span class="sxs-lookup"><span data-stu-id="b256e-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar alterações](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="b256e-792">*Confirmar alterações*</span><span class="sxs-lookup"><span data-stu-id="b256e-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="b256e-793">Tarefa 3-publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web</span><span class="sxs-lookup"><span data-stu-id="b256e-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="b256e-794">Volte para a solução ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="b256e-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="b256e-795">Na **Gerenciador de soluções**, clique com o botão direito do mouse no projeto de site e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="b256e-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="b256e-796">![Publicando o aplicativo](aspnet-mvc-4-fundamentals/_static/image60.png "Publicando o aplicativo")</span><span class="sxs-lookup"><span data-stu-id="b256e-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="b256e-797">*Publicando o site*</span><span class="sxs-lookup"><span data-stu-id="b256e-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="b256e-798">Importe o perfil de publicação salvo na primeira tarefa.</span><span class="sxs-lookup"><span data-stu-id="b256e-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="b256e-799">![Importando o perfil de publicação](aspnet-mvc-4-fundamentals/_static/image61.png "Importando o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="b256e-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="b256e-800">*Importando perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="b256e-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="b256e-801">Clique em **validar conexão**.</span><span class="sxs-lookup"><span data-stu-id="b256e-801">Click **Validate Connection**.</span></span> <span data-ttu-id="b256e-802">Depois que a validação for concluída, clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="b256e-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b256e-803">A validação será concluída quando você vir uma marca de seleção verde ao lado do botão Validar conexão.</span><span class="sxs-lookup"><span data-stu-id="b256e-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="b256e-804">![Validando conexão](aspnet-mvc-4-fundamentals/_static/image62.png "Validando conexão")</span><span class="sxs-lookup"><span data-stu-id="b256e-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="b256e-805">*Validando conexão*</span><span class="sxs-lookup"><span data-stu-id="b256e-805">*Validating connection*</span></span>
4. <span data-ttu-id="b256e-806">Na página **configurações** , na seção **bancos** de dados, clique no botão ao lado da caixa de texto da sua conexão de banco (ou seja, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="b256e-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="b256e-807">![Configuração de implantação da Web](aspnet-mvc-4-fundamentals/_static/image63.png "Configuração de implantação da Web")</span><span class="sxs-lookup"><span data-stu-id="b256e-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="b256e-808">*Configuração de implantação da Web*</span><span class="sxs-lookup"><span data-stu-id="b256e-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="b256e-809">Configure a conexão de banco de dados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="b256e-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="b256e-810">No **nome do servidor** , digite a URL do servidor do banco de dados SQL usando o prefixo *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="b256e-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="b256e-811">Em **nome de usuário** , digite o nome de logon do administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="b256e-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="b256e-812">Em **senha** , digite a senha de logon do administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="b256e-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="b256e-813">Digite um novo nome de banco de dados, por exemplo: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="b256e-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="b256e-814">![Configurando a cadeia de conexão de destino](aspnet-mvc-4-fundamentals/_static/image64.png "Configurando a cadeia de conexão de destino")</span><span class="sxs-lookup"><span data-stu-id="b256e-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="b256e-815">*Configurando a cadeia de conexão de destino*</span><span class="sxs-lookup"><span data-stu-id="b256e-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="b256e-816">Em seguida, clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="b256e-816">Then click **OK**.</span></span> <span data-ttu-id="b256e-817">Quando for solicitado a criar o banco de dados, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="b256e-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="b256e-818">![Criando o banco de dados](aspnet-mvc-4-fundamentals/_static/image65.png "Criando a cadeia de caracteres do banco de dados")</span><span class="sxs-lookup"><span data-stu-id="b256e-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="b256e-819">*Criando o banco de dados*</span><span class="sxs-lookup"><span data-stu-id="b256e-819">*Creating the database*</span></span>
7. <span data-ttu-id="b256e-820">A cadeia de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto conexão padrão.</span><span class="sxs-lookup"><span data-stu-id="b256e-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="b256e-821">Em seguida, clique em **Próximo**.</span><span class="sxs-lookup"><span data-stu-id="b256e-821">Then click **Next**.</span></span>

    <span data-ttu-id="b256e-822">![Cadeia de conexão apontando para o banco de dados SQL](aspnet-mvc-4-fundamentals/_static/image66.png "Cadeia de conexão apontando para o banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="b256e-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="b256e-823">*Cadeia de conexão apontando para o banco de dados SQL*</span><span class="sxs-lookup"><span data-stu-id="b256e-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="b256e-824">Na página **Visualização** , clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="b256e-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="b256e-825">![Publicando o aplicativo Web](aspnet-mvc-4-fundamentals/_static/image67.png "Publicando o aplicativo Web")</span><span class="sxs-lookup"><span data-stu-id="b256e-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="b256e-826">*Publicando o aplicativo Web*</span><span class="sxs-lookup"><span data-stu-id="b256e-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="b256e-827">Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.</span><span class="sxs-lookup"><span data-stu-id="b256e-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="b256e-828">![Aplicativo publicado no Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Aplicativo publicado no Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="b256e-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="b256e-829">*Aplicativo publicado no Windows Azure*</span><span class="sxs-lookup"><span data-stu-id="b256e-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="b256e-830">Apêndice C: usando trechos de código</span><span class="sxs-lookup"><span data-stu-id="b256e-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="b256e-831">Com trechos de código, você tem todo o código de que precisa ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="b256e-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="b256e-832">O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="b256e-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="b256e-833">![Usando trechos de código do Visual Studio para inserir código em seu projeto](aspnet-mvc-4-fundamentals/_static/image69.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="b256e-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="b256e-834">*Usando trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="b256e-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="b256e-835">***Para adicionar um trecho de código usando o tecladoC# (somente)***</span><span class="sxs-lookup"><span data-stu-id="b256e-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="b256e-836">Coloque o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="b256e-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="b256e-837">Comece digitando o nome do trecho de código (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="b256e-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="b256e-838">Observe como o IntelliSense exibe nomes de trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="b256e-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="b256e-839">Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).</span><span class="sxs-lookup"><span data-stu-id="b256e-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="b256e-840">Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="b256e-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="b256e-841">![Comece a digitar o nome do trecho](aspnet-mvc-4-fundamentals/_static/image70.png "Comece a digitar o nome do trecho")</span><span class="sxs-lookup"><span data-stu-id="b256e-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="b256e-842">*Comece a digitar o nome do trecho*</span><span class="sxs-lookup"><span data-stu-id="b256e-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="b256e-843">![Pressione Tab para selecionar o trecho realçado](aspnet-mvc-4-fundamentals/_static/image71.png "Pressione Tab para selecionar o trecho realçado")</span><span class="sxs-lookup"><span data-stu-id="b256e-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="b256e-844">*Pressione Tab para selecionar o trecho realçado*</span><span class="sxs-lookup"><span data-stu-id="b256e-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="b256e-845">![Pressione Tab novamente e o trecho será expandido](aspnet-mvc-4-fundamentals/_static/image72.png "Pressione Tab novamente e o trecho será expandido")</span><span class="sxs-lookup"><span data-stu-id="b256e-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="b256e-846">*Pressione Tab novamente e o trecho será expandido*</span><span class="sxs-lookup"><span data-stu-id="b256e-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="b256e-847">***Para adicionar um trecho de código usando o mouseC#(, Visual Basic e XML)*** uma.</span><span class="sxs-lookup"><span data-stu-id="b256e-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="b256e-848">Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="b256e-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="b256e-849">Selecione **Inserir trecho** seguido por **meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="b256e-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="b256e-850">Selecione o trecho relevante na lista clicando nele.</span><span class="sxs-lookup"><span data-stu-id="b256e-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="b256e-851">![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-fundamentals/_static/image73.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")</span><span class="sxs-lookup"><span data-stu-id="b256e-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="b256e-852">*Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*</span><span class="sxs-lookup"><span data-stu-id="b256e-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="b256e-853">![Selecione o trecho relevante na lista clicando nele](aspnet-mvc-4-fundamentals/_static/image74.png "Selecione o trecho relevante na lista clicando nele")</span><span class="sxs-lookup"><span data-stu-id="b256e-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="b256e-854">*Selecione o trecho relevante na lista clicando nele*</span><span class="sxs-lookup"><span data-stu-id="b256e-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
