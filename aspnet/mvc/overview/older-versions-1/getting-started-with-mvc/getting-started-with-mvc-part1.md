---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Introdução ao MVC do ASP.NET | Microsoft Docs
author: shanselman
description: Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Crie um aplicativo Web simples que lê e grava de um banco de dados.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: f8f0014776ba1313119e8c39c63a216b0fc864e7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581581"
---
# <a name="intro-to-aspnet-mvc"></a><span data-ttu-id="25454-104">Introdução ao ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="25454-104">Intro to ASP.NET MVC</span></span>

<span data-ttu-id="25454-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="25454-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> > [!NOTE]
> > <span data-ttu-id="25454-106">Uma versão atualizada se este tutorial estiver disponível [aqui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span><span class="sxs-lookup"><span data-stu-id="25454-106">An updated version if this tutorial is available [here](../../getting-started/introduction/getting-started.md) using [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).</span></span> <span data-ttu-id="25454-107">O novo tutorial usa o ASP.NET MVC 5, que fornece vários aprimoramentos sobre este tutorial.</span><span class="sxs-lookup"><span data-stu-id="25454-107">The new tutorial uses ASP.NET MVC 5, which provides many improvements over this tutorial.</span></span>
>
>
> <span data-ttu-id="25454-108">Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="25454-108">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="25454-109">Você criará um aplicativo Web simples que lê e grava de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="25454-109">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="25454-110">Visite o [ASP.NET MVC Learning Center](../../../index.md) para encontrar outros tutoriais e exemplos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="25454-110">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>

<span data-ttu-id="25454-111">Vamos criar nosso primeiro aplicativo Web ASP.NET MVC usando o [Visual Web developer 2010 Express](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="25454-111">Let's make our first ASP.NET MVC Web Application using [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="25454-112">Vamos fazer um pequeno aplicativo de lista de filmes que nos permitirá criar e listar filmes.</span><span class="sxs-lookup"><span data-stu-id="25454-112">We'll make a little Movie list application that will let us create and list movies.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="25454-113">O que você vai construir</span><span class="sxs-lookup"><span data-stu-id="25454-113">What You'll Build</span></span>

<span data-ttu-id="25454-114">Aqui estão duas capturas de tela do aplicativo que você criará.</span><span class="sxs-lookup"><span data-stu-id="25454-114">Here are two screenshots of the application you'll build.</span></span> <span data-ttu-id="25454-115">Você terá uma tabela simples de filmes com várias colunas.</span><span class="sxs-lookup"><span data-stu-id="25454-115">You will have a simple table of movies with various columns.</span></span>

<span data-ttu-id="25454-116">[Lista de filmes ![-Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="25454-116">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)</span></span>

<span data-ttu-id="25454-117">E você terá um formulário criar para que possamos adicionar filmes à lista.</span><span class="sxs-lookup"><span data-stu-id="25454-117">And you'll have a Create Form so we can add movies to the list.</span></span>

<span data-ttu-id="25454-118">[![criar um filme-Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="25454-118">[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)</span></span>

## <a name="skills-youll-learn"></a><span data-ttu-id="25454-119">Qualificações que você aprenderá</span><span class="sxs-lookup"><span data-stu-id="25454-119">Skills You'll Learn</span></span>

<span data-ttu-id="25454-120">Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="25454-120">This tutorial will teach you the basics of building an ASP.NET MVC Web Application using Visual Studio.</span></span> <span data-ttu-id="25454-121">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="25454-121">You'll learn:</span></span>

- <span data-ttu-id="25454-122">Como criar um novo projeto MVC do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="25454-122">How to create a new ASP.NET MVC Project</span></span>
- <span data-ttu-id="25454-123">Como criar um novo banco de dados com SQL Server</span><span class="sxs-lookup"><span data-stu-id="25454-123">How to create a new Database with SQL Server</span></span>
- <span data-ttu-id="25454-124">Como criar controladores e exibições do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="25454-124">How to create ASP.NET MVC Controllers and Views</span></span>
- <span data-ttu-id="25454-125">Como recuperar e exibir dados</span><span class="sxs-lookup"><span data-stu-id="25454-125">How to retrieve and display data</span></span>
- <span data-ttu-id="25454-126">Como editar dados e habilitar a validação de dados</span><span class="sxs-lookup"><span data-stu-id="25454-126">How to edit data and enable data validation</span></span>
- <span data-ttu-id="25454-127">Como atualizar o esquema de banco de dados</span><span class="sxs-lookup"><span data-stu-id="25454-127">How to update the database schema</span></span>

## <a name="get-started"></a><span data-ttu-id="25454-128">Introdução</span><span class="sxs-lookup"><span data-stu-id="25454-128">Get Started</span></span>

<span data-ttu-id="25454-129">Comece executando o Visual Web Developer 2010 Express (vou chamá-lo de "VWD" de agora em diante) e selecione novo projeto na tela inicial.</span><span class="sxs-lookup"><span data-stu-id="25454-129">Start by running Visual Web Developer 2010 Express (I'll call it "VWD" from now on) and select New Project from the Start Screen.</span></span>

<span data-ttu-id="25454-130">O Visual Web Developer é um IDE ou um ambiente de desenvolvedor integrado.</span><span class="sxs-lookup"><span data-stu-id="25454-130">Visual Web Developer is an IDE, or Integrated Developer Environment.</span></span> <span data-ttu-id="25454-131">Assim como você usa o Microsoft Word para escrever documentos, você usará um IDE para criar aplicativos.</span><span class="sxs-lookup"><span data-stu-id="25454-131">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="25454-132">Há uma barra de ferramentas na parte superior mostrando várias opções disponíveis para você, bem como o menu que você também pode ter usado para selecionar arquivo | Novo projeto.</span><span class="sxs-lookup"><span data-stu-id="25454-132">There's a toolbar along the top showing various options available to you, as well as the menu you could also have used to Select File | New project.</span></span>

<span data-ttu-id="25454-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="25454-133">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)</span></span>

## <a name="creating-your-first-application"></a><span data-ttu-id="25454-134">Criando seu primeiro aplicativo</span><span class="sxs-lookup"><span data-stu-id="25454-134">Creating Your First Application</span></span>

<span data-ttu-id="25454-135">Você pode criar aplicativos usando Visual Basic ou Visual C#.</span><span class="sxs-lookup"><span data-stu-id="25454-135">You can create applications using Visual Basic or Visual C#.</span></span> <span data-ttu-id="25454-136">Por enquanto, selecione Visual C# à esquerda e, em seguida, selecione "aplicativo Web ASP.NET MVC 2".</span><span class="sxs-lookup"><span data-stu-id="25454-136">For now, Select Visual C# on the left, then pick "ASP.NET MVC 2 Web Application."</span></span> <span data-ttu-id="25454-137">Nomeie o seu projeto como "filmes" e clique em OK.</span><span class="sxs-lookup"><span data-stu-id="25454-137">Name your project "Movies" and click OK.</span></span>

<span data-ttu-id="25454-138">[![novo projeto](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="25454-138">[![New Project](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)</span></span>

<span data-ttu-id="25454-139">No lado direito está o Gerenciador de Soluções mostrar todos os arquivos e pastas em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="25454-139">On the right-hand side is the Solution Explorer showing all the files and folders in your application.</span></span> <span data-ttu-id="25454-140">A grande janela no meio é onde você edita seu código e passa a maior parte do tempo.</span><span class="sxs-lookup"><span data-stu-id="25454-140">The big window in the middle is where you edit your code and spend most of your time.</span></span> <span data-ttu-id="25454-141">O Visual Studio usou um modelo padrão para o projeto MVC ASP.NET que você acabou de criar, portanto, você tem um aplicativo funcional agora sem fazer nada!</span><span class="sxs-lookup"><span data-stu-id="25454-141">Visual Studio used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="25454-142">Este é um simples "Olá, Mundo!</span><span class="sxs-lookup"><span data-stu-id="25454-142">This is a simple "Hello World!</span></span> <span data-ttu-id="25454-143">projeto, e é um bom ponto de partida para o nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="25454-143">project, and it is a good place to start for our application.</span></span>

<span data-ttu-id="25454-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="25454-144">[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)</span></span>

<span data-ttu-id="25454-145">Selecione o botão "reproduzir" na barra de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="25454-145">Select the "play" button to the toolbar.</span></span>

![Iniciar Depuração](getting-started-with-mvc-part1/_static/image11.png)

<span data-ttu-id="25454-147">É uma seta verde apontando para a direita que compilará seu programa e iniciará seu aplicativo em um navegador da Web.</span><span class="sxs-lookup"><span data-stu-id="25454-147">It's a green arrow pointing to the right that will compile your program and start your application in a web browser.</span></span>

<span data-ttu-id="25454-148">*Observação: em vez disso, você pode pressionar F5 no teclado ou selecionar Debug-&gt;iniciar a depuração no menu "depurar".*</span><span class="sxs-lookup"><span data-stu-id="25454-148">*NOTE: You can instead press F5 on your keyboard, or select Debug-&gt;Start Debugging from the "Debug" menu.*</span></span>

<span data-ttu-id="25454-149">Isso fará com que o Visual Web Developer inicie um servidor Web de desenvolvimento e execute nosso aplicativo Web (não há nenhuma configuração ou etapa manual necessária para habilitá-lo).</span><span class="sxs-lookup"><span data-stu-id="25454-149">This will cause Visual Web Developer to start a development web-server and run our web application (there are no configuration or manual steps required to enable this).</span></span> <span data-ttu-id="25454-150">Em seguida, ele iniciará um navegador e o configurará para procurar a página inicial do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="25454-150">It will then launch a browser and configure it to browse the application's home-page.</span></span> <span data-ttu-id="25454-151">Observe abaixo que a barra de endereços do navegador diz "localhost", e não algo como example.com.</span><span class="sxs-lookup"><span data-stu-id="25454-151">Notice below that the address bar of the browser says "localhost", and not something like example.com.</span></span> <span data-ttu-id="25454-152">Isso ocorre porque o localhost sempre aponta para seu próprio computador local – que, nesse caso, está executando o aplicativo que acabamos de criar.</span><span class="sxs-lookup"><span data-stu-id="25454-152">That's because localhost always points to your own local computer - which in this case is running the application we just built.</span></span>

<span data-ttu-id="25454-153">[Página inicial do ![](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="25454-153">[![Home Page](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)</span></span>

<span data-ttu-id="25454-154">Esse modelo padrão fornece duas páginas para você visitar e uma página de logon básica.</span><span class="sxs-lookup"><span data-stu-id="25454-154">Out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="25454-155">Vamos alterar como esse aplicativo funciona e aprender um pouco sobre o ASP.NET MVC no processo.</span><span class="sxs-lookup"><span data-stu-id="25454-155">Let's change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="25454-156">Feche o navegador e altere algum código.</span><span class="sxs-lookup"><span data-stu-id="25454-156">Close your browser and lets change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="25454-157">Próximo</span><span class="sxs-lookup"><span data-stu-id="25454-157">Next</span></span>](getting-started-with-mvc-part2.md)
