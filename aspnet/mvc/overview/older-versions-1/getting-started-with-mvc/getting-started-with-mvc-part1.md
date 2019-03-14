---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introdução ao ASP.NET MVC | Microsoft Docs
author: shanselman
description: Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC. Crie um aplicativo web simples que lê e grava de um banco de dados.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 2d9c1dd0dd3c9f892b42b0f29ac3361a7f2b638c
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037253"
---
<a name="intro-to-aspnet-mvc"></a><span data-ttu-id="e98d3-104">Introdução ao ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e98d3-104">Intro to ASP.NET MVC</span></span>
====================
<span data-ttu-id="e98d3-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="e98d3-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="e98d3-106">Uma versão atualizada, se este tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span><span class="sxs-lookup"><span data-stu-id="e98d3-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span></span> <span data-ttu-id="e98d3-107">O novo tutorial usa o ASP.NET MVC 5, que fornece muitos aprimoramentos ao longo deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="e98d3-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
>
>
> <span data-ttu-id="e98d3-108">Este é um tutorial para iniciantes que apresenta os conceitos básicos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e98d3-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="e98d3-109">Você criará um aplicativo web simples que lê e grava de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e98d3-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="e98d3-110">Visite o [Central de informações do ASP.NET MVC](../../../index.md) para localizar outros ASP.NET MVC, tutoriais e exemplos.</span><span class="sxs-lookup"><span data-stu-id="e98d3-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="e98d3-111">Vamos criar nosso primeiro aplicativo Web ASP.NET MVC usando [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="e98d3-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="e98d3-112">Vamos fazer um pequeno aplicativo de lista de filmes que vamos criar e lista de filmes.</span><span class="sxs-lookup"><span data-stu-id="e98d3-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="e98d3-113">O que você vai criar</span><span class="sxs-lookup"><span data-stu-id="e98d3-113">What You'll Build</span></span>

<span data-ttu-id="e98d3-114">Aqui estão as duas capturas de tela do aplicativo que você criará.</span><span class="sxs-lookup"><span data-stu-id="e98d3-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="e98d3-115">Você terá uma tabela simples de filmes com várias colunas.</span><span class="sxs-lookup"><span data-stu-id="e98d3-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="e98d3-116">[![Lista de filmes - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e98d3-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="e98d3-117">E você terá um formulário de criação para poder adicionar à lista de filmes.</span><span class="sxs-lookup"><span data-stu-id="e98d3-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="e98d3-118">[![Criar um filme - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="e98d3-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="e98d3-119">Habilidades que você aprenderá</span><span class="sxs-lookup"><span data-stu-id="e98d3-119">Skills You'll Learn</span></span>

<span data-ttu-id="e98d3-120">Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web do ASP.NET MVC usando o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e98d3-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="e98d3-121">Você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="e98d3-121">You'll learn:</span></span>

- <span data-ttu-id="e98d3-122">Como criar um novo projeto ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="e98d3-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="e98d3-123">Como criar um novo banco de dados com o SQL Server</span><span class="sxs-lookup"><span data-stu-id="e98d3-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="e98d3-124">Como criar exibições e controladores de MVC do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e98d3-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="e98d3-125">Como recuperar e exibir dados</span><span class="sxs-lookup"><span data-stu-id="e98d3-125">How to retrieve and display data</span></span>
- <span data-ttu-id="e98d3-126">Como editar dados e habilitar a validação de dados</span><span class="sxs-lookup"><span data-stu-id="e98d3-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="e98d3-127">Como atualizar o esquema de banco de dados</span><span class="sxs-lookup"><span data-stu-id="e98d3-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="e98d3-128">Introdução</span><span class="sxs-lookup"><span data-stu-id="e98d3-128">Get Started</span></span>

<span data-ttu-id="e98d3-129">Comece executando o Visual Web Developer 2010 Express (chamarei de "VWD" de agora em diante) e selecione Novo projeto na tela Iniciar.</span><span class="sxs-lookup"><span data-stu-id="e98d3-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="e98d3-130">O Visual Web Developer é um IDE ou ambiente de desenvolvimento integrado.</span><span class="sxs-lookup"><span data-stu-id="e98d3-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="e98d3-131">Assim como usar o Microsoft Word para gravar documentos, você usará um IDE para criar aplicativos.</span><span class="sxs-lookup"><span data-stu-id="e98d3-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="e98d3-132">Há uma barra de ferramentas na parte superior mostrando várias opções disponíveis para você, bem como no menu que você também poderia ter usado para selecionar o arquivo | Novo projeto.</span><span class="sxs-lookup"><span data-stu-id="e98d3-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="e98d3-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="e98d3-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="e98d3-134">Criando seu primeiro aplicativo</span><span class="sxs-lookup"><span data-stu-id="e98d3-134">Creating Your First Application</span></span>

<span data-ttu-id="e98d3-135">Você pode criar aplicativos usando o Visual Basic ou Visual c#.</span><span class="sxs-lookup"><span data-stu-id="e98d3-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="e98d3-136">Por enquanto, selecione Visual c# à esquerda, em seguida, escolher "Aplicativo de Web do ASP.NET MVC 2".</span><span class="sxs-lookup"><span data-stu-id="e98d3-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="e98d3-137">Nomeie o projeto "Filmes" e clique em Okey.</span><span class="sxs-lookup"><span data-stu-id="e98d3-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="e98d3-138">[![Novo projeto](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="e98d3-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="e98d3-139">No lado direito é o Gerenciador de soluções mostrando todos os arquivos e pastas em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e98d3-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="e98d3-140">A janela grande no meio é onde você pode editar seu código e passa a maior parte do seu tempo.</span><span class="sxs-lookup"><span data-stu-id="e98d3-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="e98d3-141">Visual Studio usou um modelo padrão para o projeto do ASP.NET MVC que você acabou de criar, portanto, você tem agora um aplicativo de trabalho sem fazer nada!</span><span class="sxs-lookup"><span data-stu-id="e98d3-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="e98d3-142">Isso é um simples "Hello World!</span><span class="sxs-lookup"><span data-stu-id="e98d3-142">This is a simple "Hello World!</span></span> <span data-ttu-id="e98d3-143">projeto e é um bom lugar para começar nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e98d3-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="e98d3-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="e98d3-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="e98d3-145">Selecione o botão "reproduzir" na barra de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="e98d3-145">Select the "play" button to the toolbar.</span></span>

![Iniciar a depuração](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="e98d3-147">É uma seta verde apontando para a direita que será compilado seu programa e iniciar seu aplicativo em um navegador da web.</span><span class="sxs-lookup"><span data-stu-id="e98d3-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="e98d3-148">*OBSERVAÇÃO: Em vez disso, você pode pressionar F5 no teclado ou selecione Debug -&gt;iniciar depuração no menu "Debug".*</span><span class="sxs-lookup"><span data-stu-id="e98d3-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="e98d3-149">Isso fará com que o Visual Web Developer iniciar um servidor web de desenvolvimento e executar nosso aplicativo web (não há nenhuma configuração ou etapas manuais necessárias para habilitar isso).</span><span class="sxs-lookup"><span data-stu-id="e98d3-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="e98d3-150">Em seguida, ele iniciará um navegador e configurá-lo para procurar a home page do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e98d3-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="e98d3-151">Abaixo, observe que a barra de endereços do navegador diz "localhost" e não algo como exemplo.com.</span><span class="sxs-lookup"><span data-stu-id="e98d3-151">Notice below that the address bar of the browser says "localhost", and not something like example.com.</span></span> <span data-ttu-id="e98d3-152">Isso ocorre porque o localhost sempre aponta para o seu próprio computador local – que nesse caso está executando o aplicativo que acabou de criar.</span><span class="sxs-lookup"><span data-stu-id="e98d3-152">That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="e98d3-153">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="e98d3-153">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="e98d3-154">Fora da caixa desse modelo padrão fornece a você duas páginas para visitar e uma página de logon básica.</span><span class="sxs-lookup"><span data-stu-id="e98d3-154">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="e98d3-155">Vamos alterar como este aplicativo funciona e aprender um pouco sobre o ASP.NET MVC no processo.</span><span class="sxs-lookup"><span data-stu-id="e98d3-155">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="e98d3-156">Feche seu navegador e permite alterar o código.</span><span class="sxs-lookup"><span data-stu-id="e98d3-156">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e98d3-157">Avançar</span><span class="sxs-lookup"><span data-stu-id="e98d3-157">Next</span></span>](getting-started-with-mvc-part2.md)
