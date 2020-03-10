---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Noções básicas sobre o processo de execução do ASP.NET MVC | Microsoft Docs
author: microsoft
description: Saiba como o ASP.NET MVC Framework processa uma solicitação de navegador passo a passo.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 28940947253e0af43886cf1231f8aaf4615526cc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541513"
---
# <a name="understanding-the-aspnet-mvc-execution-process"></a><span data-ttu-id="bb05b-103">Noções básicas sobre o processo de execução do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="bb05b-103">Understanding the ASP.NET MVC Execution Process</span></span>

<span data-ttu-id="bb05b-104">pela [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="bb05b-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="bb05b-105">Saiba como o ASP.NET MVC Framework processa uma solicitação de navegador passo a passo.</span><span class="sxs-lookup"><span data-stu-id="bb05b-105">Learn how the ASP.NET MVC framework processes a browser request step-by-step.</span></span>

<span data-ttu-id="bb05b-106">As solicitações para um aplicativo Web baseado em ASP.NET MVC primeiro passam pelo objeto **UrlRoutingModule** , que é um módulo http.</span><span class="sxs-lookup"><span data-stu-id="bb05b-106">Requests to an ASP.NET MVC-based Web application first pass through the **UrlRoutingModule** object, which is an HTTP module.</span></span> <span data-ttu-id="bb05b-107">Este módulo analisa a solicitação e executa a seleção de rota.</span><span class="sxs-lookup"><span data-stu-id="bb05b-107">This module parses the request and performs route selection.</span></span> <span data-ttu-id="bb05b-108">O objeto **UrlRoutingModule** seleciona o primeiro objeto de rota que corresponde à solicitação atual.</span><span class="sxs-lookup"><span data-stu-id="bb05b-108">The **UrlRoutingModule** object selects the first route object that matches the current request.</span></span> <span data-ttu-id="bb05b-109">(Um objeto de rota é uma classe que implementa **RouteBase**e normalmente é uma instância da classe **Route** .) Se nenhuma rota corresponder, o objeto **UrlRoutingModule** não fará nada e permitirá que a solicitação volte para o processamento de solicitações ASP.net ou IIS regular.</span><span class="sxs-lookup"><span data-stu-id="bb05b-109">(A route object is a class that implements **RouteBase**, and is typically an instance of the **Route** class.) If no routes match, the **UrlRoutingModule** object does nothing and lets the request fall back to the regular ASP.NET or IIS request processing.</span></span>

<span data-ttu-id="bb05b-110">A partir do objeto de **rota** selecionado, o objeto **UrlRoutingModule** Obtém o objeto **IRouteHandler** associado ao objeto de **rota** .</span><span class="sxs-lookup"><span data-stu-id="bb05b-110">From the selected **Route** object, the **UrlRoutingModule** object obtains the **IRouteHandler** object that is associated with the **Route** object.</span></span> <span data-ttu-id="bb05b-111">Normalmente, em um aplicativo MVC, essa será uma instância de **MvcRouteHandler**.</span><span class="sxs-lookup"><span data-stu-id="bb05b-111">Typically, in an MVC application, this will be an instance of **MvcRouteHandler**.</span></span> <span data-ttu-id="bb05b-112">A instância **IRouteHandler** cria um objeto **IHttpHandler** e o passa para o objeto **IHttpContext** .</span><span class="sxs-lookup"><span data-stu-id="bb05b-112">The **IRouteHandler** instance creates an **IHttpHandler** object and passes it the **IHttpContext** object.</span></span> <span data-ttu-id="bb05b-113">Por padrão, a instância **IHttpHandler** para MVC é o objeto **MvcHandler** .</span><span class="sxs-lookup"><span data-stu-id="bb05b-113">By default, the **IHttpHandler** instance for MVC is the **MvcHandler** object.</span></span> <span data-ttu-id="bb05b-114">Em seguida, o objeto **MvcHandler** seleciona o controlador que, por fim, manipulará a solicitação.</span><span class="sxs-lookup"><span data-stu-id="bb05b-114">The **MvcHandler** object then selects the controller that will ultimately handle the request.</span></span>

> [!NOTE]
> <span data-ttu-id="bb05b-115">Quando um aplicativo Web ASP.NET MVC é executado no IIS 7,0, nenhuma extensão de nome de arquivo é necessária para projetos do MVC.</span><span class="sxs-lookup"><span data-stu-id="bb05b-115">When an ASP.NET MVC Web application runs in IIS 7.0, no file name extension is required for MVC projects.</span></span> <span data-ttu-id="bb05b-116">No entanto, no IIS 6,0, o manipulador requer que você mapeie a extensão de nome de arquivo. Mvc para a DLL ISAPI ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bb05b-116">However, in IIS 6.0, the handler requires that you map the .mvc file name extension to the ASP.NET ISAPI DLL.</span></span>

<span data-ttu-id="bb05b-117">O módulo e o manipulador são os pontos de entrada para a estrutura MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bb05b-117">The module and handler are the entry points to the ASP.NET MVC framework.</span></span> <span data-ttu-id="bb05b-118">Eles executam as seguintes ações:</span><span class="sxs-lookup"><span data-stu-id="bb05b-118">They perform the following actions:</span></span>

- <span data-ttu-id="bb05b-119">Selecione o controlador apropriado em um aplicativo Web MVC.</span><span class="sxs-lookup"><span data-stu-id="bb05b-119">Select the appropriate controller in an MVC Web application.</span></span>
- <span data-ttu-id="bb05b-120">Obtenha uma instância de controlador específica.</span><span class="sxs-lookup"><span data-stu-id="bb05b-120">Obtain a specific controller instance.</span></span>
- <span data-ttu-id="bb05b-121">Chame o método **Execute** do controlador.</span><span class="sxs-lookup"><span data-stu-id="bb05b-121">Call the controller's **Execute** method.</span></span>

<span data-ttu-id="bb05b-122">O seguinte lista os estágios de execução de um projeto Web MVC:</span><span class="sxs-lookup"><span data-stu-id="bb05b-122">The following lists the stages of execution for an MVC Web project:</span></span>

- <span data-ttu-id="bb05b-123">Receber a primeira solicitação para o aplicativo</span><span class="sxs-lookup"><span data-stu-id="bb05b-123">Receive first request for the application</span></span> 

    - <span data-ttu-id="bb05b-124">No arquivo global. asax, os objetos de **rota** são adicionados ao objeto **RouteTable** .</span><span class="sxs-lookup"><span data-stu-id="bb05b-124">In the Global.asax file, **Route** objects are added to the **RouteTable** object.</span></span>
- <span data-ttu-id="bb05b-125">Executar roteamento</span><span class="sxs-lookup"><span data-stu-id="bb05b-125">Perform routing</span></span> 

    - <span data-ttu-id="bb05b-126">O **módulo UrlRoutingModule** usa o primeiro objeto **de rota** correspondente na coleção **RouteTable** para criar o objeto **RouteData** , que ele usa para criar um objeto **RequestContext** (**IHttpContext**).</span><span class="sxs-lookup"><span data-stu-id="bb05b-126">The **UrlRoutingModule** module uses the first matching **Route** object in the **RouteTable** collection to create the **RouteData** object, which it then uses to create a **RequestContext** (**IHttpContext**) object.</span></span>
- <span data-ttu-id="bb05b-127">Criar manipulador de solicitação MVC</span><span class="sxs-lookup"><span data-stu-id="bb05b-127">Create MVC request handler</span></span> 

    - <span data-ttu-id="bb05b-128">O objeto **MvcRouteHandler** cria uma instância da classe **MvcHandler** e a passa para a instância de **RequestContext** .</span><span class="sxs-lookup"><span data-stu-id="bb05b-128">The **MvcRouteHandler** object creates an instance of the **MvcHandler** class and passes it the **RequestContext** instance.</span></span>
- <span data-ttu-id="bb05b-129">Criar controlador</span><span class="sxs-lookup"><span data-stu-id="bb05b-129">Create controller</span></span> 

    - <span data-ttu-id="bb05b-130">O objeto **MvcHandler** usa a instância **RequestContext** para identificar o objeto **IControllerFactory** (normalmente uma instância da classe **DefaultControllerFactory** ) para criar a instância do controlador com o.</span><span class="sxs-lookup"><span data-stu-id="bb05b-130">The **MvcHandler** object uses the **RequestContext** instance to identify the **IControllerFactory** object (typically an instance of the **DefaultControllerFactory** class) to create the controller instance with.</span></span>
- <span data-ttu-id="bb05b-131">Executar controlador-a instância **MvcHandler** chama o método **Execute** do controlador.</span><span class="sxs-lookup"><span data-stu-id="bb05b-131">Execute controller - The **MvcHandler** instance calls the controller s **Execute** method.</span></span> |
- <span data-ttu-id="bb05b-132">Ação de invocação</span><span class="sxs-lookup"><span data-stu-id="bb05b-132">Invoke action</span></span> 

    - <span data-ttu-id="bb05b-133">A maioria dos controladores herdam da classe base do **controlador** .</span><span class="sxs-lookup"><span data-stu-id="bb05b-133">Most controllers inherit from the **Controller** base class.</span></span> <span data-ttu-id="bb05b-134">Para controladores que fazem isso, o objeto **ControllerActionInvoker** associado ao controlador determina qual método de ação da classe de controlador chamar e, em seguida, chama esse método.</span><span class="sxs-lookup"><span data-stu-id="bb05b-134">For controllers that do so, the **ControllerActionInvoker** object that is associated with the controller determines which action method of the controller class to call, and then calls that method.</span></span>
- <span data-ttu-id="bb05b-135">Executar resultado</span><span class="sxs-lookup"><span data-stu-id="bb05b-135">Execute result</span></span> 

    - <span data-ttu-id="bb05b-136">Um método de ação típico pode receber entrada do usuário, preparar os dados de resposta apropriados e, em seguida, executar o resultado retornando um tipo de resultado.</span><span class="sxs-lookup"><span data-stu-id="bb05b-136">A typical action method might receive user input, prepare the appropriate response data, and then execute the result by returning a result type.</span></span> <span data-ttu-id="bb05b-137">Os tipos de resultados internos que podem ser executados incluem o seguinte: **ViewResult** (que renderiza uma exibição e é o tipo de resultado usado com mais frequência), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**e **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="bb05b-137">The built-in result types that can be executed include the following: **ViewResult** (which renders a view and is the most-often used result type), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, and **EmptyResult**.</span></span>
