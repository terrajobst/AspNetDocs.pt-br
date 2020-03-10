---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Criando um controlador (VB) | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther demonstra como você pode adicionar um controlador a um aplicativo MVC ASP.NET.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: 60636b79ab5fc06ca904dee90ce74f256e046d12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601454"
---
# <a name="creating-a-controller-vb"></a><span data-ttu-id="59be5-103">Criar um controlador (VB)</span><span class="sxs-lookup"><span data-stu-id="59be5-103">Creating a Controller (VB)</span></span>

<span data-ttu-id="59be5-104">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="59be5-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="59be5-105">Neste tutorial, Stephen Walther demonstra como você pode adicionar um controlador a um aplicativo MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="59be5-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>

<span data-ttu-id="59be5-106">O objetivo deste tutorial é explicar como você pode criar novos controladores MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="59be5-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="59be5-107">Você aprende a criar controladores usando a opção de menu Adicionar controlador do Visual Studio e criando um arquivo de classe manualmente.</span><span class="sxs-lookup"><span data-stu-id="59be5-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="59be5-108">Usando a opção de menu Adicionar controlador</span><span class="sxs-lookup"><span data-stu-id="59be5-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="59be5-109">A maneira mais fácil de criar um novo controlador é clicar com o botão direito do mouse na pasta controladores na janela de Gerenciador de Soluções do Visual Studio e selecionar a opção de menu **Adicionar, controlador** (consulte a Figura 1).</span><span class="sxs-lookup"><span data-stu-id="59be5-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="59be5-110">A seleção dessa opção de menu abre a caixa de diálogo **Adicionar controlador** (consulte a Figura 2).</span><span class="sxs-lookup"><span data-stu-id="59be5-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>

<span data-ttu-id="59be5-111">[![caixa de diálogo novo projeto](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="59be5-111">[![The New Project dialog box](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span></span>

<span data-ttu-id="59be5-112">**Figura 01**: adicionando um novo controlador ([clique para exibir a imagem em tamanho normal](creating-a-controller-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="59be5-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-vb/_static/image2.png))</span></span>

<span data-ttu-id="59be5-113">[![caixa de diálogo novo projeto](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="59be5-113">[![The New Project dialog box](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span></span>

<span data-ttu-id="59be5-114">**Figura 02**: a caixa de diálogo Adicionar controlador ([clique para exibir a imagem em tamanho normal](creating-a-controller-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="59be5-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-vb/_static/image4.png))</span></span>

<span data-ttu-id="59be5-115">Observe que a primeira parte do nome do controlador está realçada na caixa de diálogo **Adicionar controlador** .</span><span class="sxs-lookup"><span data-stu-id="59be5-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="59be5-116">Cada nome de controlador deve terminar com o *controlador*de sufixo.</span><span class="sxs-lookup"><span data-stu-id="59be5-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="59be5-117">Por exemplo, você pode criar um controlador chamado *ProductController* , mas não um controlador chamado *Product*.</span><span class="sxs-lookup"><span data-stu-id="59be5-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>

<span data-ttu-id="59be5-118">Se você criar um controlador que não tem o sufixo do *controlador* , não será possível invocar o controlador.</span><span class="sxs-lookup"><span data-stu-id="59be5-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="59be5-119">Não faça isso, desperdiçamos incontáveis horas da minha vida depois de fazer esse erro.</span><span class="sxs-lookup"><span data-stu-id="59be5-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>

<span data-ttu-id="59be5-120">**Listagem 1-Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="59be5-120">**Listing 1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

<span data-ttu-id="59be5-121">Você sempre deve criar controladores na pasta controladores.</span><span class="sxs-lookup"><span data-stu-id="59be5-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="59be5-122">Caso contrário, você violará as convenções do ASP.NET MVC e outros desenvolvedores terão um tempo mais difícil de entender seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="59be5-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="59be5-123">Métodos de ação scaffolding</span><span class="sxs-lookup"><span data-stu-id="59be5-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="59be5-124">Ao criar um controlador, você tem a opção de gerar automaticamente os métodos de ação criar, atualizar e detalhes (veja a Figura 3).</span><span class="sxs-lookup"><span data-stu-id="59be5-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="59be5-125">Se você selecionar essa opção, a classe de controlador na Listagem 2 será gerada.</span><span class="sxs-lookup"><span data-stu-id="59be5-125">If you select this option then the controller class in Listing 2 is generated.</span></span>

<span data-ttu-id="59be5-126">[![criar métodos de ação automaticamente](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="59be5-126">[![Creating action methods automatically](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span></span>

<span data-ttu-id="59be5-127">**Figura 03**: criando métodos de ação automaticamente ([clique para exibir a imagem em tamanho normal](creating-a-controller-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="59be5-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-vb/_static/image6.png))</span></span>

<span data-ttu-id="59be5-128">**Listagem 2-Controllers\CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="59be5-128">**Listing 2 - Controllers\CustomerController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

<span data-ttu-id="59be5-129">Esses métodos gerados são métodos stub.</span><span class="sxs-lookup"><span data-stu-id="59be5-129">These generated methods are stub methods.</span></span> <span data-ttu-id="59be5-130">Você deve adicionar a lógica real para criar, atualizar e mostrar os detalhes de um cliente por conta própria.</span><span class="sxs-lookup"><span data-stu-id="59be5-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="59be5-131">Mas, os métodos de stub fornecem um bom ponto de partida.</span><span class="sxs-lookup"><span data-stu-id="59be5-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="59be5-132">Criando uma classe de controlador</span><span class="sxs-lookup"><span data-stu-id="59be5-132">Creating a Controller Class</span></span>

<span data-ttu-id="59be5-133">O controlador MVC ASP.NET é apenas uma classe.</span><span class="sxs-lookup"><span data-stu-id="59be5-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="59be5-134">Se preferir, você pode ignorar o scaffolding conveniente do Visual Studio Controller e criar uma classe de controlador manualmente.</span><span class="sxs-lookup"><span data-stu-id="59be5-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="59be5-135">Siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="59be5-135">Follow these steps:</span></span>

1. <span data-ttu-id="59be5-136">Clique com o botão direito do mouse na pasta controladores e selecione a opção de menu **Adicionar, novo item** e selecione o modelo de **classe** (consulte a Figura 4).</span><span class="sxs-lookup"><span data-stu-id="59be5-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="59be5-137">Nomeie a nova classe PersonController. vb e clique no botão **Adicionar** .</span><span class="sxs-lookup"><span data-stu-id="59be5-137">Name the new class PersonController.vb and click the **Add** button.</span></span>
3. <span data-ttu-id="59be5-138">Modifique o arquivo de classe resultante para que a classe herde da classe base System. Web. Mvc. Controller (consulte a listagem 3).</span><span class="sxs-lookup"><span data-stu-id="59be5-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>

<span data-ttu-id="59be5-139">[![criar uma nova classe](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="59be5-139">[![Creating a new class](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span></span>

<span data-ttu-id="59be5-140">**Figura 04**: criando uma nova classe ([clique para exibir a imagem em tamanho normal](creating-a-controller-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="59be5-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-vb/_static/image8.png))</span></span>

<span data-ttu-id="59be5-141">**Listagem 3-Controllers\PersonController.vb**</span><span class="sxs-lookup"><span data-stu-id="59be5-141">**Listing 3 - Controllers\PersonController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

<span data-ttu-id="59be5-142">O controlador na Listagem 3 expõe uma ação chamada index () que retorna a cadeia de caracteres "Olá, Mundo!".</span><span class="sxs-lookup"><span data-stu-id="59be5-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="59be5-143">Você pode invocar essa ação do controlador executando seu aplicativo e solicitando uma URL como a seguinte:</span><span class="sxs-lookup"><span data-stu-id="59be5-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="59be5-144">O ASP.NET Development Server usa um número de porta aleatório (por exemplo, 40071).</span><span class="sxs-lookup"><span data-stu-id="59be5-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="59be5-145">Ao inserir uma URL para invocar um controlador, você precisará fornecer o número de porta correto.</span><span class="sxs-lookup"><span data-stu-id="59be5-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="59be5-146">Você pode determinar o número da porta passando o mouse sobre o ícone do ASP.NET Development Server na área de notificação do Windows (inferior direita da tela).</span><span class="sxs-lookup"><span data-stu-id="59be5-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="59be5-147">[Anterior](adding-dynamic-content-to-a-cached-page-vb.md)
> [Próximo](creating-an-action-vb.md)</span><span class="sxs-lookup"><span data-stu-id="59be5-147">[Previous](adding-dynamic-content-to-a-cached-page-vb.md)
[Next](creating-an-action-vb.md)</span></span>
