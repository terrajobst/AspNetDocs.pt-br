---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: Visão geral de roteamento doC#ASP.NET MVC () | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther mostra como o ASP.NET MVC Framework mapeia solicitações de navegador para ações do controlador.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 5e1155ca676e7a25b5bfc63e251c6387a010eb34
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544138"
---
# <a name="aspnet-mvc-routing-overview-c"></a><span data-ttu-id="2cdf2-103">Visão geral sobre roteamento do ASP.NET MVC (C#)</span><span class="sxs-lookup"><span data-stu-id="2cdf2-103">ASP.NET MVC Routing Overview (C#)</span></span>

<span data-ttu-id="2cdf2-104">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="2cdf2-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="2cdf2-105">Neste tutorial, Stephen Walther mostra como o ASP.NET MVC Framework mapeia solicitações de navegador para ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>

<span data-ttu-id="2cdf2-106">Neste tutorial, você será apresentado a um recurso importante de cada aplicativo MVC ASP.NET chamado *roteamento ASP.net*.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="2cdf2-107">O módulo de roteamento ASP.NET é responsável por mapear solicitações de navegador de entrada para ações específicas do controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="2cdf2-108">Ao final deste tutorial, você entenderá como a tabela de rotas padrão mapeia solicitações para ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="2cdf2-109">Usando a tabela de rotas padrão</span><span class="sxs-lookup"><span data-stu-id="2cdf2-109">Using the Default Route Table</span></span>

<span data-ttu-id="2cdf2-110">Quando você cria um novo aplicativo MVC do ASP.NET, o aplicativo já está configurado para usar o roteamento do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="2cdf2-111">O roteamento do ASP.NET é configurado em dois locais.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="2cdf2-112">Primeiro, o roteamento ASP.NET está habilitado no arquivo de configuração da Web do seu aplicativo (arquivo Web. config).</span><span class="sxs-lookup"><span data-stu-id="2cdf2-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="2cdf2-113">Há quatro seções no arquivo de configuração que são relevantes para o roteamento: a seção System. Web. httpModules, a seção System. Web. httpHandlers, a seção System. WebServer. modules e a seção System. WebServer. Handlers.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="2cdf2-114">Tenha cuidado para não excluir essas seções porque, sem essas seções, o roteamento não funcionará mais.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="2cdf2-115">Segundo, e o mais importante, uma tabela de rotas é criada no arquivo global. asax do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="2cdf2-116">O arquivo global. asax é um arquivo especial que contém manipuladores de eventos para eventos de ciclo de vida do aplicativo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="2cdf2-117">A tabela de rotas é criada durante o evento de início do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="2cdf2-118">O arquivo na Listagem 1 contém o arquivo global. asax padrão para um aplicativo MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="2cdf2-119">**Listagem 1-Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="2cdf2-119">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

<span data-ttu-id="2cdf2-120">Quando um aplicativo MVC é iniciado pela primeira vez, o aplicativo\_método Start () é chamado.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="2cdf2-121">Esse método, por sua vez, chama o método RegisterRoutes ().</span><span class="sxs-lookup"><span data-stu-id="2cdf2-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="2cdf2-122">O método RegisterRoutes () cria a tabela de rotas.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="2cdf2-123">A tabela de rotas padrão contém uma única rota (denominada padrão).</span><span class="sxs-lookup"><span data-stu-id="2cdf2-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="2cdf2-124">A rota padrão mapeia o primeiro segmento de uma URL para um nome de controlador, o segundo segmento de uma URL para uma ação de controlador e o terceiro segmento para um parâmetro chamado **ID**.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="2cdf2-125">Imagine que você insira a seguinte URL na barra de endereços do navegador da Web:</span><span class="sxs-lookup"><span data-stu-id="2cdf2-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="2cdf2-126">/Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="2cdf2-126">/Home/Index/3</span></span>

<span data-ttu-id="2cdf2-127">A rota padrão mapeia essa URL para os seguintes parâmetros:</span><span class="sxs-lookup"><span data-stu-id="2cdf2-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="2cdf2-128">controlador = página inicial</span><span class="sxs-lookup"><span data-stu-id="2cdf2-128">controller = Home</span></span>

- <span data-ttu-id="2cdf2-129">ação = índice</span><span class="sxs-lookup"><span data-stu-id="2cdf2-129">action = Index</span></span>

- <span data-ttu-id="2cdf2-130">id = 3</span><span class="sxs-lookup"><span data-stu-id="2cdf2-130">id = 3</span></span>

<span data-ttu-id="2cdf2-131">Quando você solicita a URL/Home/Index/3, o código a seguir é executado:</span><span class="sxs-lookup"><span data-stu-id="2cdf2-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="2cdf2-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="2cdf2-132">HomeController.Index(3)</span></span>

<span data-ttu-id="2cdf2-133">A rota padrão inclui padrões para todos os três parâmetros.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="2cdf2-134">Se você não fornecer um controlador, o parâmetro do controlador padrão será o valor **página inicial**.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="2cdf2-135">Se você não fornecer uma ação, o parâmetro de ação assume como padrão o **índice**de valor.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="2cdf2-136">Por fim, se você não fornecer uma ID, o parâmetro de ID assume como padrão uma cadeia de caracteres vazia.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="2cdf2-137">Vejamos alguns exemplos de como a rota padrão mapeia URLs para ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="2cdf2-138">Imagine que você insira a seguinte URL na barra de endereços do navegador:</span><span class="sxs-lookup"><span data-stu-id="2cdf2-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="2cdf2-139">/Home</span><span class="sxs-lookup"><span data-stu-id="2cdf2-139">/Home</span></span>

<span data-ttu-id="2cdf2-140">Devido aos padrões de parâmetro de rota padrão, a inserção dessa URL fará com que o método index () da classe HomeController na Listagem 2 seja chamado.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="2cdf2-141">**Listagem 2-HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="2cdf2-141">**Listing 2 - HomeController.cs**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

<span data-ttu-id="2cdf2-142">Na Listagem 2, a classe HomeController inclui um método chamado index () que aceita um único parâmetro chamado ID. A URL/Home faz com que o método index () seja chamado com uma cadeia de caracteres vazia como o valor do parâmetro ID.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with an empty string as the value of the Id parameter.</span></span>

<span data-ttu-id="2cdf2-143">Devido à forma como o MVC Framework invoca ações do controlador, a URL/Home também corresponde ao método index () da classe HomeController na Listagem 3.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="2cdf2-144">**Listagem 3-HomeController.cs (ação de índice sem parâmetro)**</span><span class="sxs-lookup"><span data-stu-id="2cdf2-144">**Listing 3 - HomeController.cs (Index action with no parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

<span data-ttu-id="2cdf2-145">O método index () na Listagem 3 não aceita nenhum parâmetro.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="2cdf2-146">A URL/Home fará com que esse método de índice () seja chamado.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="2cdf2-147">A URL/Home/Index/3 também invoca esse método (a ID é ignorada).</span><span class="sxs-lookup"><span data-stu-id="2cdf2-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="2cdf2-148">A URL/Home também corresponde ao método index () da classe HomeController na Listagem 4.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="2cdf2-149">**Listagem 4-HomeController.cs (ação de índice com parâmetro anulável)**</span><span class="sxs-lookup"><span data-stu-id="2cdf2-149">**Listing 4 - HomeController.cs (Index action with nullable parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

<span data-ttu-id="2cdf2-150">Na Listagem 4, o método index () tem um parâmetro Integer.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="2cdf2-151">Como o parâmetro é um parâmetro anulável (pode ter o valor NULL), o índice () pode ser chamado sem gerar um erro.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-151">Because the parameter is a nullable parameter (can have the value Null), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="2cdf2-152">Finalmente, invocar o método index () na listagem 5 com a URL/Home causa uma exceção, uma vez que o parâmetro ID *não é* um parâmetro anulável.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="2cdf2-153">Se você tentar invocar o método index (), você obterá o erro exibido na Figura 1.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="2cdf2-154">**Listagem 5-HomeController.cs (ação de índice com parâmetro de ID)**</span><span class="sxs-lookup"><span data-stu-id="2cdf2-154">**Listing 5 - HomeController.cs (Index action with Id parameter)**</span></span>

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]

<span data-ttu-id="2cdf2-155">[![invocando uma ação do controlador que espera um valor de parâmetro](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2cdf2-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)</span></span>

<span data-ttu-id="2cdf2-156">**Figura 01**: invocando uma ação do controlador que espera um valor de parâmetro ([clique para exibir a imagem em tamanho normal](asp-net-mvc-routing-overview-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="2cdf2-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-cs/_static/image2.png))</span></span>

<span data-ttu-id="2cdf2-157">A URL/Home/Index/3, por outro lado, funciona bem com a ação de controlador de índice na listagem 5.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="2cdf2-158">A solicitação/Home/Index/3 faz com que o método index () seja chamado com um parâmetro de ID que tenha o valor 3.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="2cdf2-159">Resumo</span><span class="sxs-lookup"><span data-stu-id="2cdf2-159">Summary</span></span>

<span data-ttu-id="2cdf2-160">O objetivo deste tutorial era fornecer uma breve introdução ao roteamento ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="2cdf2-161">Examinamos a tabela de rotas padrão que você obtém com um novo aplicativo MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="2cdf2-162">Você aprendeu como a rota padrão mapeia URLs para ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="2cdf2-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2cdf2-163">Próximo</span><span class="sxs-lookup"><span data-stu-id="2cdf2-163">Next</span></span>](understanding-action-filters-cs.md)
