---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Introdução ao ASP.NET MVC 3 (C#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: e71275c93558c0b6ca087a145786e8c846b69721
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457514"
---
# <a name="intro-to-aspnet-mvc-3-c"></a><span data-ttu-id="5b939-103">Introdução ao ASP.NET MVC 3 (C#)</span><span class="sxs-lookup"><span data-stu-id="5b939-103">Intro to ASP.NET MVC 3 (C#)</span></span>

<span data-ttu-id="5b939-104">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5b939-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> > [!NOTE]
> > <span data-ttu-id="5b939-105">Uma versão atualizada deste tutorial está disponível [aqui](../../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="5b939-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="5b939-106">É mais seguro, muito mais simples de seguir e demonstra mais recursos.</span><span class="sxs-lookup"><span data-stu-id="5b939-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="5b939-107">Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5b939-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="5b939-108">Antes de começar, verifique se você instalou os pré-requisitos listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="5b939-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="5b939-109">Você pode instalar todos eles clicando no seguinte link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="5b939-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="5b939-110">Como alternativa, você pode instalar os pré-requisitos individualmente usando os seguintes links:</span><span class="sxs-lookup"><span data-stu-id="5b939-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="5b939-111">Pré-requisitos do Visual Studio Web Developer Express SP1</span><span class="sxs-lookup"><span data-stu-id="5b939-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="5b939-112">Atualização de ferramentas do ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="5b939-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="5b939-113">[SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(suporte + ferramentas de tempo de execução)</span><span class="sxs-lookup"><span data-stu-id="5b939-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="5b939-114">Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no seguinte link: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span><span class="sxs-lookup"><span data-stu-id="5b939-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="5b939-115">Um projeto do Visual Web Developer com C# código-fonte está disponível para acompanhar este tópico.</span><span class="sxs-lookup"><span data-stu-id="5b939-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="5b939-116">[Baixe a C# versão](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span><span class="sxs-lookup"><span data-stu-id="5b939-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="5b939-117">Se preferir Visual Basic, alterne para a [versão Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="5b939-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="5b939-118">O que você vai construir</span><span class="sxs-lookup"><span data-stu-id="5b939-118">What You'll Build</span></span>

<span data-ttu-id="5b939-119">Você implementará um aplicativo simples de listagem de filmes que dá suporte à criação, edição e listagem de filmes de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5b939-119">You'll implement a simple movie-listing application that supports creating, editing, and listing movies from a database.</span></span> <span data-ttu-id="5b939-120">Abaixo estão duas capturas de tela do aplicativo que você criará.</span><span class="sxs-lookup"><span data-stu-id="5b939-120">Below are two screenshots of the application you'll build.</span></span> <span data-ttu-id="5b939-121">Ele inclui uma página que exibe uma lista de filmes de um banco de dados:</span><span class="sxs-lookup"><span data-stu-id="5b939-121">It includes a page that displays a list of movies from a database:</span></span>

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

<span data-ttu-id="5b939-123">O aplicativo também permite que você adicione, edite e exclua filmes, bem como veja detalhes sobre aqueles individuais.</span><span class="sxs-lookup"><span data-stu-id="5b939-123">The application also lets you add, edit, and delete movies, as well as see details about individual ones.</span></span> <span data-ttu-id="5b939-124">Todos os cenários de entrada de dados incluem validação para garantir que os dados armazenados no banco de dado estejam corretos.</span><span class="sxs-lookup"><span data-stu-id="5b939-124">All data-entry scenarios include validation to ensure that the data stored in the database is correct.</span></span>

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="5b939-125">Qualificações que você aprenderá</span><span class="sxs-lookup"><span data-stu-id="5b939-125">Skills You'll Learn</span></span>

<span data-ttu-id="5b939-126">Eis o que você vai aprender:</span><span class="sxs-lookup"><span data-stu-id="5b939-126">Here's what you'll learn:</span></span>

- <span data-ttu-id="5b939-127">Como criar um novo projeto MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5b939-127">How to create a new ASP.NET MVC project.</span></span>
- <span data-ttu-id="5b939-128">Como criar controladores e exibições do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5b939-128">How to create ASP.NET MVC controllers and views.</span></span>
- <span data-ttu-id="5b939-129">Como criar um novo banco de dados usando o Entity Framework Code First paradigma.</span><span class="sxs-lookup"><span data-stu-id="5b939-129">How to create a new database using the Entity Framework Code First paradigm.</span></span>
- <span data-ttu-id="5b939-130">Como recuperar e exibir dados.</span><span class="sxs-lookup"><span data-stu-id="5b939-130">How to retrieve and display data.</span></span>
- <span data-ttu-id="5b939-131">Como editar dados e habilitar a validação de dados.</span><span class="sxs-lookup"><span data-stu-id="5b939-131">How to edit data and enable data validation.</span></span>

## <a name="getting-started"></a><span data-ttu-id="5b939-132">Introdução</span><span class="sxs-lookup"><span data-stu-id="5b939-132">Getting Started</span></span>

<span data-ttu-id="5b939-133">Comece executando o Visual Web Developer 2010 Express ("Visual Web Developer" para curto) e selecione **novo projeto** na página **inicial** .</span><span class="sxs-lookup"><span data-stu-id="5b939-133">Start by running Visual Web Developer 2010 Express ("Visual Web Developer" for short) and select **New Project** from the **Start** page.</span></span>

<span data-ttu-id="5b939-134">O Visual Web Developer é um IDE ou ambiente de desenvolvimento integrado.</span><span class="sxs-lookup"><span data-stu-id="5b939-134">Visual Web Developer is an IDE, or integrated development environment.</span></span> <span data-ttu-id="5b939-135">Assim como você usa o Microsoft Word para escrever documentos, você usará um IDE para criar aplicativos.</span><span class="sxs-lookup"><span data-stu-id="5b939-135">Just like you use Microsoft Word to write documents, you'll use an IDE to create applications.</span></span> <span data-ttu-id="5b939-136">No Visual Web Developer há uma barra de ferramentas ao longo da parte superior mostrando várias opções disponíveis para você.</span><span class="sxs-lookup"><span data-stu-id="5b939-136">In Visual Web Developer there's a toolbar along the top showing various options available to you.</span></span> <span data-ttu-id="5b939-137">Há também um menu que fornece outra maneira de executar tarefas no IDE.</span><span class="sxs-lookup"><span data-stu-id="5b939-137">There's also a menu that provides another way to perform tasks in the IDE.</span></span> <span data-ttu-id="5b939-138">(Por exemplo, em vez de selecionar **novo projeto** na página **inicial** , você pode usar o menu e selecionar **arquivo** &gt; **novo projeto**.)</span><span class="sxs-lookup"><span data-stu-id="5b939-138">(For example, instead of selecting **New Project** from the **Start** page, you can use the menu and select **File** &gt; **New Project**.)</span></span>

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a><span data-ttu-id="5b939-139">Criando seu primeiro aplicativo</span><span class="sxs-lookup"><span data-stu-id="5b939-139">Creating Your First Application</span></span>

<span data-ttu-id="5b939-140">Você pode criar aplicativos usando o Visual Basic ou o C# Visual como a linguagem de programação.</span><span class="sxs-lookup"><span data-stu-id="5b939-140">You can create applications using either Visual Basic or Visual C# as the programming language.</span></span> <span data-ttu-id="5b939-141">Selecione Visual C# à esquerda e, em seguida, selecione **aplicativo Web ASP.NET MVC 3**.</span><span class="sxs-lookup"><span data-stu-id="5b939-141">Select Visual C# on the left and then select **ASP.NET MVC 3 Web Application**.</span></span> <span data-ttu-id="5b939-142">Nomeie seu projeto como "MvcMovie" e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="5b939-142">Name your project "MvcMovie" and then click **OK**.</span></span> <span data-ttu-id="5b939-143">(Se preferir Visual Basic, alterne para a [versão Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.)</span><span class="sxs-lookup"><span data-stu-id="5b939-143">(If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.)</span></span>

![](intro-to-aspnet-mvc-3/_static/image5.png)

<span data-ttu-id="5b939-144">Na caixa de diálogo **novo projeto do ASP.NET MVC 3** , selecione **aplicativo de Internet**.</span><span class="sxs-lookup"><span data-stu-id="5b939-144">In the **New ASP.NET MVC 3 Project** dialog box, select **Internet Application**.</span></span> <span data-ttu-id="5b939-145">Marque a seleção **usar marcação HTML5** e deixe o **Razor** como o mecanismo de exibição padrão.</span><span class="sxs-lookup"><span data-stu-id="5b939-145">Check **Use HTML5 markup** and leave **Razor** as the default view engine.</span></span>

![](intro-to-aspnet-mvc-3/_static/image6.png)

<span data-ttu-id="5b939-146">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="5b939-146">Click **OK**.</span></span> <span data-ttu-id="5b939-147">O Visual Web Developer usou um modelo padrão para o projeto ASP.NET MVC que você acabou de criar, portanto, você tem um aplicativo funcional agora sem fazer nada!</span><span class="sxs-lookup"><span data-stu-id="5b939-147">Visual Web Developer used a default template for the ASP.NET MVC project you just created, so you have a working application right now without doing anything!</span></span> <span data-ttu-id="5b939-148">Este é um "Olá, Mundo!" simples</span><span class="sxs-lookup"><span data-stu-id="5b939-148">This is a simple "Hello World!"</span></span> <span data-ttu-id="5b939-149">projeto, e é um bom lugar para iniciar seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5b939-149">project, and it's a good place to start your application.</span></span>

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

<span data-ttu-id="5b939-150">No menu **Depuração**, selecione **Iniciar Depuração**.</span><span class="sxs-lookup"><span data-stu-id="5b939-150">From the **Debug** menu, select **Start Debugging**.</span></span>

![](intro-to-aspnet-mvc-3/_static/image9.png)

<span data-ttu-id="5b939-151">Observe que o atalho de teclado para iniciar a depuração é F5.</span><span class="sxs-lookup"><span data-stu-id="5b939-151">Notice that the keyboard shortcut to start debugging is F5.</span></span>

<span data-ttu-id="5b939-152">F5 faz com que o Visual Web Developer inicie um servidor Web de desenvolvimento e execute seu aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="5b939-152">F5 causes Visual Web Developer to start a development web server and run your web application.</span></span> <span data-ttu-id="5b939-153">Em seguida, o Visual Web Developer inicia um navegador e abre a home page do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5b939-153">Visual Web Developer then launches a browser and opens the application's home page.</span></span> <span data-ttu-id="5b939-154">Observe que a barra de endereços do navegador indica `localhost` e não algo como `example.com`.</span><span class="sxs-lookup"><span data-stu-id="5b939-154">Notice that the address bar of the browser says `localhost` and not something like `example.com`.</span></span> <span data-ttu-id="5b939-155">Isso ocorre porque `localhost` sempre aponta para seu próprio computador local, que nesse caso está executando o aplicativo que você acabou de criar.</span><span class="sxs-lookup"><span data-stu-id="5b939-155">That's because `localhost` always points to your own local computer, which in this case is running the application you just built.</span></span> <span data-ttu-id="5b939-156">Quando o Visual Web Developer executa um projeto Web, uma porta aleatória é usada para o servidor Web.</span><span class="sxs-lookup"><span data-stu-id="5b939-156">When Visual Web Developer runs a web project, a random port is used for the web server.</span></span> <span data-ttu-id="5b939-157">Na imagem abaixo, o número da porta aleatória é 43246.</span><span class="sxs-lookup"><span data-stu-id="5b939-157">In the image below, the random port number is 43246.</span></span> <span data-ttu-id="5b939-158">Ao executar o aplicativo, você provavelmente verá um número de porta diferente.</span><span class="sxs-lookup"><span data-stu-id="5b939-158">When you run the application, you'll probably see a different port number.</span></span>

![](intro-to-aspnet-mvc-3/_static/image10.png)

<span data-ttu-id="5b939-159">Pronto para uso, esse modelo padrão fornece duas páginas para você visitar e uma página de logon básica.</span><span class="sxs-lookup"><span data-stu-id="5b939-159">Right out of the box this default template gives you two pages to visit and a basic login page.</span></span> <span data-ttu-id="5b939-160">A próxima etapa é alterar a forma como esse aplicativo funciona e aprender um pouco sobre o ASP.NET MVC no processo.</span><span class="sxs-lookup"><span data-stu-id="5b939-160">The next step is to change how this application works and learn a little bit about ASP.NET MVC in the process.</span></span> <span data-ttu-id="5b939-161">Feche o navegador e vamos alterar algum código.</span><span class="sxs-lookup"><span data-stu-id="5b939-161">Close your browser and let's change some code.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5b939-162">Próximo</span><span class="sxs-lookup"><span data-stu-id="5b939-162">Next</span></span>](adding-a-controller.md)
