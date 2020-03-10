---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Parte 1: visão geral e arquivo-> novo projeto | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 1 abrange visão geral e arquivo-> novo projeto.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 48428ff4ab5888253ed93ac41e79006eec823ad2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559979"
---
# <a name="part-1-overview-and-file-new-project"></a><span data-ttu-id="a6611-104">Parte 1: visão geral e arquivo-> novo projeto</span><span class="sxs-lookup"><span data-stu-id="a6611-104">Part 1: Overview and File->New Project</span></span>

<span data-ttu-id="a6611-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="a6611-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="a6611-106">A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.</span><span class="sxs-lookup"><span data-stu-id="a6611-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="a6611-107">A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="a6611-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="a6611-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a6611-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="a6611-109">A parte 1 abrange visão geral e arquivo-&gt;novo projeto.</span><span class="sxs-lookup"><span data-stu-id="a6611-109">Part 1 covers Overview and File-&gt;New Project.</span></span>

## <a name="overview"></a><span data-ttu-id="a6611-110">Visão geral</span><span class="sxs-lookup"><span data-stu-id="a6611-110">Overview</span></span>

<span data-ttu-id="a6611-111">A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Web Developer para desenvolvimento para a Web.</span><span class="sxs-lookup"><span data-stu-id="a6611-111">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Web Developer for web development.</span></span> <span data-ttu-id="a6611-112">Começaremos lentamente, então a experiência de desenvolvimento para a Web de nível principiante está ok.</span><span class="sxs-lookup"><span data-stu-id="a6611-112">We'll be starting slowly, so beginner level web development experience is okay.</span></span>

<span data-ttu-id="a6611-113">O aplicativo que iremos criar é uma loja de música simples.</span><span class="sxs-lookup"><span data-stu-id="a6611-113">The application we'll be building is a simple music store.</span></span> <span data-ttu-id="a6611-114">Há três partes principais para o aplicativo: compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="a6611-114">There are three main parts to the application: shopping, checkout, and administration.</span></span>

![](mvc-music-store-part-1/_static/image1.jpg)

<span data-ttu-id="a6611-115">Os visitantes podem procurar álbuns por gênero:</span><span class="sxs-lookup"><span data-stu-id="a6611-115">Visitors can browse Albums by Genre:</span></span>

![](mvc-music-store-part-1/_static/image2.jpg)

<span data-ttu-id="a6611-116">Eles podem exibir um único álbum e adicioná-lo ao seu carrinho:</span><span class="sxs-lookup"><span data-stu-id="a6611-116">They can view a single album and add it to their cart:</span></span>

![](mvc-music-store-part-1/_static/image3.jpg)

<span data-ttu-id="a6611-117">Eles podem revisar seus carrinhos, removendo os itens que não desejam mais:</span><span class="sxs-lookup"><span data-stu-id="a6611-117">They can review their cart, removing any items they no longer want:</span></span>

![](mvc-music-store-part-1/_static/image4.jpg)

<span data-ttu-id="a6611-118">Prosseguir com o check-out solicitará que eles façam logon ou registrem-se para uma conta de usuário.</span><span class="sxs-lookup"><span data-stu-id="a6611-118">Proceeding to Checkout will prompt them to login or register for a user account.</span></span>

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

<span data-ttu-id="a6611-119">Depois de criar uma conta, eles podem concluir o pedido preenchendo as informações de envio e pagamento.</span><span class="sxs-lookup"><span data-stu-id="a6611-119">After creating an account, they can complete the order by filling out shipping and payment information.</span></span> <span data-ttu-id="a6611-120">Para simplificar as coisas, estamos executando uma promoção incrível: tudo está livre se inserir o código de promoção "gratuito"!</span><span class="sxs-lookup"><span data-stu-id="a6611-120">To keep things simple, we're running an amazing promotion: everything's free if they enter promotion code "FREE"!</span></span>

![](mvc-music-store-part-1/_static/image5.jpg)

<span data-ttu-id="a6611-121">Após a ordenação, eles veem uma tela de confirmação simples:</span><span class="sxs-lookup"><span data-stu-id="a6611-121">After ordering, they see a simple confirmation screen:</span></span>

![](mvc-music-store-part-1/_static/image6.jpg)

<span data-ttu-id="a6611-122">Além das páginas voltadas para o cliente, também criaremos uma seção de administrador que mostra uma lista de álbuns dos quais os administradores podem criar, editar e excluir álbuns:</span><span class="sxs-lookup"><span data-stu-id="a6611-122">In addition to customer-facing pages, we'll also build an administrator section that shows a list of albums from which Administrators can Create, Edit, and Delete albums:</span></span>

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a><span data-ttu-id="a6611-123">1. arquivo-&gt; novo projeto</span><span class="sxs-lookup"><span data-stu-id="a6611-123">1. File -&gt; New Project</span></span>

### <a name="installing-the-software"></a><span data-ttu-id="a6611-124">Instalando o software</span><span class="sxs-lookup"><span data-stu-id="a6611-124">Installing the software</span></span>

<span data-ttu-id="a6611-125">Este tutorial começará criando um novo projeto do ASP.NET MVC 3 usando o Visual Web Developer 2010 Express gratuito (que é gratuito) e, em seguida, adicionaremos os recursos de forma incremental para criar um aplicativo funcional completo.</span><span class="sxs-lookup"><span data-stu-id="a6611-125">This tutorial will begin by creating a new ASP.NET MVC 3 project using the free Visual Web Developer 2010 Express (which is free), and then we'll incrementally add features to create a complete functioning application.</span></span> <span data-ttu-id="a6611-126">Ao longo do caminho, abordaremos o acesso ao banco de dados, os cenários de lançamento de formulário, a validação, a utilização de páginas mestras para layout de página consistente, usando AJAX para atualizações de página e validação, logon de usuário e muito mais.</span><span class="sxs-lookup"><span data-stu-id="a6611-126">Along the way, we'll cover database access, form posting scenarios, data validation, using master pages for consistent page layout, using AJAX for page updates and validation, user login, and more.</span></span>

<span data-ttu-id="a6611-127">Você pode acompanhar o passo a passo ou pode baixar o aplicativo concluído do [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="a6611-127">You can follow along step by step, or you can download the completed application from [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="a6611-128">Você pode usar o Visual Studio 2010 SP1 ou o Visual Web Developer 2010 Express SP1 (uma versão gratuita do Visual Studio 2010) para compilar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a6611-128">You can use either Visual Studio 2010 SP1 or Visual Web Developer 2010 Express SP1 (a free version of Visual Studio 2010) to build the application.</span></span> <span data-ttu-id="a6611-129">Vamos usar o SQL Server Compact (também gratuito) para hospedar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="a6611-129">We'll be using the SQL Server Compact (also free) to host the database.</span></span> <span data-ttu-id="a6611-130">Antes de começar, verifique se você instalou os pré-requisitos listados abaixo.</span><span class="sxs-lookup"><span data-stu-id="a6611-130">Before you start, make sure you've installed the prerequisites listed below.</span></span>

- <span data-ttu-id="a6611-131">[Pré-requisitos do Visual Studio Web Developer Express SP1]</span><span class="sxs-lookup"><span data-stu-id="a6611-131">[Visual Studio Web Developer Express SP1 prerequisites]</span></span>
- <span data-ttu-id="a6611-132">[Atualização de ferramentas do MVC 3 do ASP.NET]</span><span class="sxs-lookup"><span data-stu-id="a6611-132">[ASP.NET MVC 3 Tools Update]</span></span>
- <span data-ttu-id="a6611-133">[SQL Server Compact 4,0]-incluindo suporte para tempo de execução e ferramentas</span><span class="sxs-lookup"><span data-stu-id="a6611-133">[SQL Server Compact 4.0] - including both runtime and tools support</span></span>

### <a name="creating-a-new-aspnet-mvc-3-project"></a><span data-ttu-id="a6611-134">Criando um novo projeto do ASP.NET MVC 3</span><span class="sxs-lookup"><span data-stu-id="a6611-134">Creating a new ASP.NET MVC 3 project</span></span>

<span data-ttu-id="a6611-135">Vamos começar selecionando "novo projeto" no menu arquivo no Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="a6611-135">We'll start by selecting "New Project" from the File menu in Visual Web Developer.</span></span> <span data-ttu-id="a6611-136">Isso abre a caixa de diálogo novo projeto.</span><span class="sxs-lookup"><span data-stu-id="a6611-136">This brings up the New Project dialog.</span></span>

![](mvc-music-store-part-1/_static/image5.png)

<span data-ttu-id="a6611-137">Selecionaremos o grupo modelos C# da Web do Visual&gt; à esquerda e, em seguida, escolheremos o modelo "aplicativo Web ASP.NET MVC 3" na coluna central.</span><span class="sxs-lookup"><span data-stu-id="a6611-137">We'll select the Visual C# -&gt; Web Templates group on the left, then choose the "ASP.NET MVC 3 Web Application" template in the center column.</span></span> <span data-ttu-id="a6611-138">Nomeie o projeto MvcMusicStore e pressione o botão OK.</span><span class="sxs-lookup"><span data-stu-id="a6611-138">Name your project MvcMusicStore and press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image8.jpg)

<span data-ttu-id="a6611-139">Isso exibirá uma caixa de diálogo secundária, que nos permite fazer algumas configurações específicas do MVC para o nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="a6611-139">This will display a secondary dialog which allows us to make some MVC specific settings for our project.</span></span> <span data-ttu-id="a6611-140">Selecione o seguinte:</span><span class="sxs-lookup"><span data-stu-id="a6611-140">Select the following:</span></span>

<span data-ttu-id="a6611-141">Modelo de projeto-selecione vazio</span><span class="sxs-lookup"><span data-stu-id="a6611-141">Project Template - select Empty</span></span>

<span data-ttu-id="a6611-142">Exibir mecanismo-selecione Razor</span><span class="sxs-lookup"><span data-stu-id="a6611-142">View Engine - select Razor</span></span>

<span data-ttu-id="a6611-143">Usar marcação semântica HTML5-verificada</span><span class="sxs-lookup"><span data-stu-id="a6611-143">Use HTML5 semantic markup - checked</span></span>

<span data-ttu-id="a6611-144">Verifique se as configurações são as mostradas abaixo e pressione o botão OK.</span><span class="sxs-lookup"><span data-stu-id="a6611-144">Verify that your settings are as shown below, then press the OK button.</span></span>

![](mvc-music-store-part-1/_static/image9.jpg)

<span data-ttu-id="a6611-145">Isso criará nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="a6611-145">This will create our project.</span></span> <span data-ttu-id="a6611-146">Vamos dar uma olhada nas pastas que foram adicionadas ao nosso aplicativo na Gerenciador de Soluções no lado direito.</span><span class="sxs-lookup"><span data-stu-id="a6611-146">Let's take a look at the folders that have been added to our application in the Solution Explorer on the right side.</span></span>

![](mvc-music-store-part-1/_static/image10.jpg)

<span data-ttu-id="a6611-147">O modelo MVC 3 vazio não está completamente vazio – ele adiciona uma estrutura de pastas básica:</span><span class="sxs-lookup"><span data-stu-id="a6611-147">The Empty MVC 3 template isn't completely empty – it adds a basic folder structure:</span></span>

![](mvc-music-store-part-1/_static/image6.png)

<span data-ttu-id="a6611-148">O ASP.NET MVC usa algumas convenções de nomenclatura básicas para nomes de pastas:</span><span class="sxs-lookup"><span data-stu-id="a6611-148">ASP.NET MVC makes use of some basic naming conventions for folder names:</span></span>

| <span data-ttu-id="a6611-149">**Pasta**</span><span class="sxs-lookup"><span data-stu-id="a6611-149">**Folder**</span></span> | <span data-ttu-id="a6611-150">**Finalidade**</span><span class="sxs-lookup"><span data-stu-id="a6611-150">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="a6611-151">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="a6611-151">**/Controllers**</span></span> | <span data-ttu-id="a6611-152">Os controladores respondem à entrada do navegador, decidem o que fazer com ele e retornam a resposta ao usuário.</span><span class="sxs-lookup"><span data-stu-id="a6611-152">Controllers respond to input from the browser, decide what to do with it, and return response to the user.</span></span> |
| <span data-ttu-id="a6611-153">**/Views**</span><span class="sxs-lookup"><span data-stu-id="a6611-153">**/Views**</span></span> | <span data-ttu-id="a6611-154">Os modos de exibição mantêm nossos modelos de interface do usuário</span><span class="sxs-lookup"><span data-stu-id="a6611-154">Views hold our UI templates</span></span> |
| <span data-ttu-id="a6611-155">**/Models**</span><span class="sxs-lookup"><span data-stu-id="a6611-155">**/Models**</span></span> | <span data-ttu-id="a6611-156">Modelos mantêm e manipulam dados</span><span class="sxs-lookup"><span data-stu-id="a6611-156">Models hold and manipulate data</span></span> |
| <span data-ttu-id="a6611-157">**/Content**</span><span class="sxs-lookup"><span data-stu-id="a6611-157">**/Content**</span></span> | <span data-ttu-id="a6611-158">Essa pasta contém nossas imagens, CSS e qualquer outro conteúdo estático</span><span class="sxs-lookup"><span data-stu-id="a6611-158">This folder holds our images, CSS, and any other static content</span></span> |
| <span data-ttu-id="a6611-159">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="a6611-159">**/Scripts**</span></span> | <span data-ttu-id="a6611-160">Essa pasta contém nossos arquivos JavaScript</span><span class="sxs-lookup"><span data-stu-id="a6611-160">This folder holds our JavaScript files</span></span> |

<span data-ttu-id="a6611-161">Essas pastas são incluídas mesmo em um aplicativo ASP.NET MVC vazio, pois a estrutura MVC do ASP.NET, por padrão, usa uma abordagem "Convenção sobre configuração" e faz algumas suposições padrão com base nas convenções de nomenclatura de pastas.</span><span class="sxs-lookup"><span data-stu-id="a6611-161">These folders are included even in an Empty ASP.NET MVC application because the ASP.NET MVC framework by default uses a "convention over configuration" approach and makes some default assumptions based on folder naming conventions.</span></span> <span data-ttu-id="a6611-162">Por exemplo, controladores procuram exibições na pasta views por padrão sem a necessidade de especificar explicitamente isso em seu código.</span><span class="sxs-lookup"><span data-stu-id="a6611-162">For instance, controllers look for views in the Views folder by default without you having to explicitly specify this in your code.</span></span> <span data-ttu-id="a6611-163">Aprimorar as convenções padrão reduz a quantidade de código que você precisa escrever e também pode facilitar para outros desenvolvedores a compreensão do seu projeto.</span><span class="sxs-lookup"><span data-stu-id="a6611-163">Sticking with the default conventions reduces the amount of code you need to write, and can also make it easier for other developers to understand your project.</span></span> <span data-ttu-id="a6611-164">Explicaremos essas convenções mais à medida que criarmos nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="a6611-164">We'll explain these conventions more as we build our application.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a6611-165">Próximo</span><span class="sxs-lookup"><span data-stu-id="a6611-165">Next</span></span>](mvc-music-store-part-2.md)
