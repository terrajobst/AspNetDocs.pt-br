---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Parte 1: arquivo-> novo projeto | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks. A parte 1 abrange visão geral e arquivo/novo projeto.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: 05a3ace3d8fef9c1f3593f7948e42b4725d70134
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636125"
---
# <a name="part-1-file--new-project"></a><span data-ttu-id="bcd01-104">Parte 1: arquivo-> novo projeto</span><span class="sxs-lookup"><span data-stu-id="bcd01-104">Part 1: File-> New Project</span></span>

<span data-ttu-id="bcd01-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="bcd01-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="bcd01-106">A Tailspin Spyworks demonstra como é extremamente simples criar aplicativos poderosos e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="bcd01-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="bcd01-107">Ele mostra como usar os ótimos novos recursos do ASP.NET 4 para criar uma loja online, incluindo compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="bcd01-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="bcd01-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="bcd01-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="bcd01-109">A parte 1 abrange visão geral e arquivo/novo projeto.</span><span class="sxs-lookup"><span data-stu-id="bcd01-109">Part 1 covers Overview and File/New Project.</span></span>

## <a id="_Toc260221666"></a><span data-ttu-id="bcd01-110">Sobre</span><span class="sxs-lookup"><span data-stu-id="bcd01-110">Overview</span></span>

<span data-ttu-id="bcd01-111">Este tutorial é uma introdução aos WebForms do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bcd01-111">This tutorial is an introduction to ASP.NET WebForms.</span></span> <span data-ttu-id="bcd01-112">Começaremos lentamente, então a experiência de desenvolvimento para a Web de nível principiante está ok.</span><span class="sxs-lookup"><span data-stu-id="bcd01-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="bcd01-113">O aplicativo que iremos criar é um armazenamento online simples.</span><span class="sxs-lookup"><span data-stu-id="bcd01-113">The application we'll be building is a simple on-line store.</span></span>

![](tailspin-spyworks-part-1/_static/image1.jpg)

<span data-ttu-id="bcd01-114">Os visitantes podem procurar produtos por categoria:</span><span class="sxs-lookup"><span data-stu-id="bcd01-114">Visitors can browse Products by Category:</span></span>

![](tailspin-spyworks-part-1/_static/image2.jpg)

<span data-ttu-id="bcd01-115">Eles podem exibir um único produto e adicioná-lo ao seu carrinho:</span><span class="sxs-lookup"><span data-stu-id="bcd01-115">They can view a single product and add it to their cart:</span></span>

![](tailspin-spyworks-part-1/_static/image3.jpg)

<span data-ttu-id="bcd01-116">Eles podem revisar seus carrinhos, removendo os itens que não desejam mais:</span><span class="sxs-lookup"><span data-stu-id="bcd01-116">They can review their cart, removing any items they no longer want:</span></span>

![](tailspin-spyworks-part-1/_static/image4.jpg)

<span data-ttu-id="bcd01-117">Prosseguir para o check-out solicitará a eles</span><span class="sxs-lookup"><span data-stu-id="bcd01-117">Proceeding to Checkout will prompt them to</span></span>

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

<span data-ttu-id="bcd01-118">Após a ordenação, eles veem uma tela de confirmação simples:</span><span class="sxs-lookup"><span data-stu-id="bcd01-118">After ordering, they see a simple confirmation screen:</span></span>

![](tailspin-spyworks-part-1/_static/image7.jpg)

<span data-ttu-id="bcd01-119">Vamos começar criando um novo projeto WebForms do ASP.NET no Visual Studio 2010 e, incrementalmente, adicionaremos recursos para criar um aplicativo funcional completo.</span><span class="sxs-lookup"><span data-stu-id="bcd01-119">We'll begin by creating a new ASP.NET WebForms project in Visual Studio 2010, and we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="bcd01-120">Ao longo do caminho, abordaremos o acesso ao banco de dados, exibições de lista e de grade, páginas de atualização, validação de dados, uso de páginas mestras para layout de página consistente, AJAX, validação, associação de usuário e muito mais.</span><span class="sxs-lookup"><span data-stu-id="bcd01-120">Along the way, we'll cover database access, list and grid views, data update pages, data validation, using master pages for consistent page layout, AJAX, validation, user membership, and more.</span></span>

<span data-ttu-id="bcd01-121">Você pode acompanhar o passo a passo ou pode baixar o aplicativo concluído de [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="bcd01-121">You can follow along step by step, or you can download the completed application from [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)</span></span>

<span data-ttu-id="bcd01-122">Você pode usar o Visual Studio 2010 ou a versão gratuita do Visual Web Developer 2010 do [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span><span class="sxs-lookup"><span data-stu-id="bcd01-122">You can use either Visual Studio 2010 or the free Visual Web Developer 2010 from [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/).</span></span> <span data-ttu-id="bcd01-123">Para compilar o aplicativo, você pode usar SQL Server ou o SQL Server Express gratuito para hospedar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="bcd01-123">To build the application, you can use either SQL Server or the free SQL Server Express to host the database.</span></span>

## <a id="_Toc260221667"></a><span data-ttu-id="bcd01-124">Arquivo/novo projeto</span><span class="sxs-lookup"><span data-stu-id="bcd01-124">File / New Project</span></span>

<span data-ttu-id="bcd01-125">Vamos começar selecionando o novo projeto no menu arquivo no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bcd01-125">We'll start by selecting the New Project from the File menu in Visual Studio.</span></span> <span data-ttu-id="bcd01-126">Isso abre a caixa de diálogo novo projeto.</span><span class="sxs-lookup"><span data-stu-id="bcd01-126">This brings up the New Project dialog.</span></span>

![](tailspin-spyworks-part-1/_static/image8.jpg)

<span data-ttu-id="bcd01-127">Selecionaremos o grupo de C# modelos visuais/Web à esquerda e, em seguida, escolheremos o modelo "aplicativo Web ASP.net" na coluna central.</span><span class="sxs-lookup"><span data-stu-id="bcd01-127">We'll select the Visual C# / Web Templates group on the left, and then choose the "ASP.NET Web Application" template in the center column.</span></span> <span data-ttu-id="bcd01-128">Nomeie o projeto TailspinSpyworks e pressione o botão OK.</span><span class="sxs-lookup"><span data-stu-id="bcd01-128">Name your project TailspinSpyworks and press the OK button.</span></span>

![](tailspin-spyworks-part-1/_static/image9.jpg)

<span data-ttu-id="bcd01-129">Isso criará nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="bcd01-129">This will create our project.</span></span> <span data-ttu-id="bcd01-130">Vamos dar uma olhada nas pastas que estão incluídas em nosso aplicativo na Gerenciador de Soluções no lado direito.</span><span class="sxs-lookup"><span data-stu-id="bcd01-130">Let's take a look at the folders that are included in our application in the Solution Explorer on the right side.</span></span>

![](tailspin-spyworks-part-1/_static/image10.jpg)

<span data-ttu-id="bcd01-131">A solução vazia não está completamente vazia – ela adiciona uma estrutura de pastas básica:</span><span class="sxs-lookup"><span data-stu-id="bcd01-131">The Empty Solution isn't completely empty – it adds a basic folder structure:</span></span>

![](tailspin-spyworks-part-1/_static/image1.png)

<span data-ttu-id="bcd01-132">Observe as convenções implementadas pelo modelo de projeto padrão ASP.NET 4.</span><span class="sxs-lookup"><span data-stu-id="bcd01-132">Note the conventions implemented by the ASP.NET 4 default project template.</span></span>

- <span data-ttu-id="bcd01-133">A pasta "Account" implementa uma interface de usuário básica para ASP. Subsistema de associação da rede.</span><span class="sxs-lookup"><span data-stu-id="bcd01-133">The "Account" folder implements a basic user interface for ASP.NET's membership subsystem.</span></span>
- <span data-ttu-id="bcd01-134">A pasta "scripts" serve como o repositório para arquivos JavaScript do lado do cliente e os arquivos principais do jQuery. js são disponibilizados por padrão.</span><span class="sxs-lookup"><span data-stu-id="bcd01-134">The "Scripts" folder serves as the repository for client side JavaScript files and the core jQuery .js files are made available by default.</span></span>
- <span data-ttu-id="bcd01-135">A pasta "estilos" é usada para organizar nossos elementos visuais de site (folhas de estilo CSS)</span><span class="sxs-lookup"><span data-stu-id="bcd01-135">The "Styles" folder is used to organize our web site visuals (CSS Style Sheets)</span></span>

<span data-ttu-id="bcd01-136">Quando pressionamos F5 para executar nosso aplicativo e renderizamos a página default. aspx, vemos o seguinte.</span><span class="sxs-lookup"><span data-stu-id="bcd01-136">When we press F5 to run our application and render the default.aspx page we see the following.</span></span>

![](tailspin-spyworks-part-1/_static/image11.jpg)

<span data-ttu-id="bcd01-137">Nosso primeiro aprimoramento de aplicativo será substituir o arquivo Style. CSS do modelo WebForms padrão pelas classes CSS e arquivos de imagem associados que processarão o Visual Asthetics que desejamos para nosso aplicativo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="bcd01-137">Our first application enhancement will be to replace the Style.css file from the default WebForms template with the CSS classes and associated image files that will render the visual asthetics that we want for our Tailspin Spyworks application.</span></span>

<span data-ttu-id="bcd01-138">Depois de fazer isso, nossa página default. aspx é renderizada dessa forma.</span><span class="sxs-lookup"><span data-stu-id="bcd01-138">After doing so our default.aspx page renders like this.</span></span>

![](tailspin-spyworks-part-1/_static/image12.jpg)

<span data-ttu-id="bcd01-139">Observe os links de imagem na parte superior direita da página e os itens de menu que foram adicionados à página mestra.</span><span class="sxs-lookup"><span data-stu-id="bcd01-139">Notice the image links at the top right of the page and the menu items that have been added to the master page.</span></span> <span data-ttu-id="bcd01-140">Somente os links "entrar" e "conta" apontam para páginas que existem (geradas pelo modelo padrão) e o restante das páginas que iremos implementar à medida que criamos nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bcd01-140">Only the "Sign In" and "Account" links point to pages that exist (generated by the default template) and the rest of the pages we will implement as we build our application.</span></span>

<span data-ttu-id="bcd01-141">Também vamos realocar a página mestra para o diretório de estilos.</span><span class="sxs-lookup"><span data-stu-id="bcd01-141">We're also going to relocate the Master Page to the Styles directory.</span></span> <span data-ttu-id="bcd01-142">Embora essa seja apenas uma preferência, isso pode tornar as coisas um pouco mais fáceis se decidirmos tornar nosso aplicativo "em pele" no futuro.</span><span class="sxs-lookup"><span data-stu-id="bcd01-142">Though this is only a preference it may make things a little easier if we decide to make our application "skinable" in the future.</span></span>

<span data-ttu-id="bcd01-143">Depois de fazer isso, precisaremos alterar as referências da página mestra em todos os arquivos. aspx gerados pelas páginas WebForms padrão do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bcd01-143">After doing this we'll need to change the master page references in all the .aspx files generated by the default ASP.NET WebForms pages.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bcd01-144">Próximo</span><span class="sxs-lookup"><span data-stu-id="bcd01-144">Next</span></span>](tailspin-spyworks-part-2.md)
