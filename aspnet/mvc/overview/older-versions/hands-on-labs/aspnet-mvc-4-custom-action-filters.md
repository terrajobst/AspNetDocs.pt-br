---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: Filtros de ação personalizada do ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: O ASP.NET MVC fornece filtros de ação para executar a lógica de filtragem antes ou depois de um método de ação ser chamado. Os filtros de ação são atributos personalizados tha...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: eaeb32180f79fabf557cbc38ff067eb26b47fea7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579691"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="8b5a4-104">Filtros de ação personalizada do ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="8b5a4-104">ASP.NET MVC 4 Custom Action Filters</span></span>

<span data-ttu-id="8b5a4-105">por [equipe de acampamentos da Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="8b5a4-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="8b5a4-106">Baixe o kit de treinamento do Web acampamentos</span><span class="sxs-lookup"><span data-stu-id="8b5a4-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="8b5a4-107">O ASP.NET MVC fornece filtros de ação para executar a lógica de filtragem antes ou depois de um método de ação ser chamado.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-107">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="8b5a4-108">Os filtros de ação são atributos personalizados que fornecem meios declarativos para adicionar o comportamento de ação prévia e ação aos métodos de ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-108">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>

<span data-ttu-id="8b5a4-109">Neste laboratório prático, você criará um atributo de filtro de ação personalizado na solução MvcMusicStore para capturar as solicitações do controlador e registrar a atividade de um site em uma tabela de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-109">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="8b5a4-110">Você poderá adicionar o filtro de log por injeção a qualquer controlador ou ação.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-110">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="8b5a4-111">Por fim, você verá o modo de exibição de log que mostra a lista de visitantes.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-111">Finally, you will see the log view that shows the list of visitors.</span></span>

<span data-ttu-id="8b5a4-112">Este laboratório prático pressupõe que você tenha conhecimento básico do **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-112">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="8b5a4-113">Se você não usou o **ASP.NET MVC** antes, recomendamos que você vá para o laboratório prático do **ASP.NET MVC 4 Fundamentals** .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-113">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="8b5a4-114">Todos os códigos de exemplo e trechos de código estão incluídos no Web acampamentos Training Kit, disponível em [Microsoft-Web/WebCampTrainingKit releases](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="8b5a4-115">O projeto específico deste laboratório está disponível em [filtros de ação personalizada do ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-115">The project specific to this lab is available at [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="8b5a4-116">Objetivos</span><span class="sxs-lookup"><span data-stu-id="8b5a4-116">Objectives</span></span>

<span data-ttu-id="8b5a4-117">Neste laboratório prático, você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="8b5a4-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="8b5a4-118">Criar um atributo de filtro de ação personalizado para estender os recursos de filtragem</span><span class="sxs-lookup"><span data-stu-id="8b5a4-118">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="8b5a4-119">Aplicar um atributo de filtro personalizado por injeção a um nível específico</span><span class="sxs-lookup"><span data-stu-id="8b5a4-119">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="8b5a4-120">Registrar um filtro de ação personalizada globalmente</span><span class="sxs-lookup"><span data-stu-id="8b5a4-120">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="8b5a4-121">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="8b5a4-121">Prerequisites</span></span>

<span data-ttu-id="8b5a4-122">Você deve ter os seguintes itens para concluir este laboratório:</span><span class="sxs-lookup"><span data-stu-id="8b5a4-122">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="8b5a4-123">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [o apêndice a](#AppendixA) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="8b5a4-124">Instalação</span><span class="sxs-lookup"><span data-stu-id="8b5a4-124">Setup</span></span>

<span data-ttu-id="8b5a4-125">**Instalando trechos de código**</span><span class="sxs-lookup"><span data-stu-id="8b5a4-125">**Installing Code Snippets**</span></span>

<span data-ttu-id="8b5a4-126">Para sua conveniência, grande parte do código que você gerenciará ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-126">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="8b5a4-127">Para instalar os trechos de código, execute o arquivo **.\Source\Setup\CodeSnippets.VSI** .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-127">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="8b5a4-128">Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot;[Apêndice C: usando trechos de código](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-128">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="8b5a4-129">Exercícios</span><span class="sxs-lookup"><span data-stu-id="8b5a4-129">Exercises</span></span>

<span data-ttu-id="8b5a4-130">Este laboratório prático é composto pelos seguintes exercícios:</span><span class="sxs-lookup"><span data-stu-id="8b5a4-130">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="8b5a4-131">Exercício 1: ações de log</span><span class="sxs-lookup"><span data-stu-id="8b5a4-131">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="8b5a4-132">Exercício 2: gerenciando vários filtros de ação</span><span class="sxs-lookup"><span data-stu-id="8b5a4-132">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="8b5a4-133">Tempo estimado para concluir este laboratório: **30 minutos**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-133">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="8b5a4-134">Cada exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir os exercícios.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-134">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="8b5a4-135">Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com os exercícios.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-135">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="8b5a4-136">Exercício 1: ações de log</span><span class="sxs-lookup"><span data-stu-id="8b5a4-136">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="8b5a4-137">Neste exercício, você aprenderá a criar um filtro de log de ação personalizado usando os provedores de filtros do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-137">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="8b5a4-138">Para essa finalidade, você aplicará um filtro de log ao site MusicStore, que registrará todas as atividades nos controladores selecionados.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-138">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="8b5a4-139">O filtro estenderá **ActionFilterAttributeClass** e substituirá o método **OnActionExecuting** para capturar cada solicitação e, em seguida, executará as ações de log.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-139">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="8b5a4-140">As informações de contexto sobre solicitações HTTP, executando métodos, resultados e parâmetros serão fornecidas pela classe ASP.NET MVC ActionExecutingContext **.**</span><span class="sxs-lookup"><span data-stu-id="8b5a4-140">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="8b5a4-141">O ASP.NET MVC 4 também tem provedores de filtros padrão que você pode usar sem criar um filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-141">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="8b5a4-142">O ASP.NET MVC 4 fornece os seguintes tipos de filtros:</span><span class="sxs-lookup"><span data-stu-id="8b5a4-142">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="8b5a4-143">Filtro de **autorização** , que toma decisões de segurança sobre a execução de um método de ação, como executar a autenticação ou validar as propriedades da solicitação.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-143">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="8b5a4-144">Filtro de **ação** , que encapsula a execução do método de ação.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-144">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="8b5a4-145">Esse filtro pode executar processamento adicional, como fornecer dados extras ao método de ação, inspecionar o valor de retorno ou cancelar a execução do método de ação</span><span class="sxs-lookup"><span data-stu-id="8b5a4-145">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="8b5a4-146">Filtro de **resultado** , que encapsula a execução do objeto ActionResult.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-146">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="8b5a4-147">Esse filtro pode executar processamento adicional do resultado, como modificar a resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-147">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="8b5a4-148">Filtro de **exceção** , que é executado se houver uma exceção sem tratamento gerada em algum lugar no método de ação, começando com os filtros de autorização e terminando com a execução do resultado.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-148">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="8b5a4-149">Os filtros de exceção podem ser usados para tarefas como registro em log ou exibição de uma página de erro.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-149">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="8b5a4-150">Para obter mais informações sobre os provedores de filtros, visite este link do MSDN: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-150">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>

<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="8b5a4-151">Sobre o recurso de log de aplicativo da loja de música MVC</span><span class="sxs-lookup"><span data-stu-id="8b5a4-151">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="8b5a4-152">Esta solução de repositório de música tem uma nova tabela de modelo de dados para log de site, **ActionLog**, com os seguintes campos: nome do controlador que recebeu uma solicitação, chamada ação, IP do cliente e carimbo de data/hora.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-152">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="8b5a4-153">![Modelo de dados. Tabela ActionLog.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Modelo de dados. Tabela ActionLog.")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-153">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="8b5a4-154">*Modelo de dados-tabela ActionLog*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-154">*Data model - ActionLog table*</span></span>

<span data-ttu-id="8b5a4-155">A solução fornece uma exibição MVC do ASP.NET para o log de ações que pode ser encontrada em **MvcMusicStores/views/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="8b5a4-155">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="8b5a4-156">![Exibição do log de ações](aspnet-mvc-4-custom-action-filters/_static/image2.png "Exibição do log de ações")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-156">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="8b5a4-157">*Exibição do log de ações*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-157">*Action Log view*</span></span>

<span data-ttu-id="8b5a4-158">Com essa estrutura determinada, todo o trabalho será focado na solicitação de interrupção do controlador e na execução do registro em log usando a filtragem personalizada.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-158">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="8b5a4-159">Tarefa 1-Criando um filtro personalizado para capturar a solicitação de um controlador</span><span class="sxs-lookup"><span data-stu-id="8b5a4-159">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="8b5a4-160">Nesta tarefa, você criará uma classe de atributo de filtro personalizado que conterá a lógica de log.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-160">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="8b5a4-161">Para essa finalidade, você estenderá a classe ASP.NET MVC **ActionFilterAttribute** e implementará a interface **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-161">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="8b5a4-162">O **ActionFilterAttribute** é a classe base para todos os filtros de atributo.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-162">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="8b5a4-163">Ele fornece os seguintes métodos para executar uma lógica específica após e antes da execução da ação do controlador:</span><span class="sxs-lookup"><span data-stu-id="8b5a4-163">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="8b5a4-164">**OnActionExecuting**(ActionExecutingContext filterContext): logo antes do método de ação ser chamado.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-164">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="8b5a4-165">**OnActionExecuted**(ActionExecutedContext filterContext): depois que o método de ação é chamado e antes do resultado ser executado (antes de exibir o processamento).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-165">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="8b5a4-166">**OnResultExecuting**(ResultExecutingContext filterContext): logo antes de o resultado ser executado (antes de exibir o processamento).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-166">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="8b5a4-167">**OnResultExecuted**(ResultExecutedContext filterContext): depois que o resultado é executado (depois que a exibição é renderizada).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-167">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="8b5a4-168">Ao substituir qualquer um desses métodos em uma classe derivada, você pode executar seu próprio código de filtragem.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-168">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>

1. <span data-ttu-id="8b5a4-169">Abra a solução **inicial** localizada na pasta **\Source\Ex01-LoggingActions\Begin** .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-169">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

   1. <span data-ttu-id="8b5a4-170">Você precisará baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-170">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="8b5a4-171">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-171">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="8b5a4-172">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-172">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="8b5a4-173">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-173">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="8b5a4-174">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-174">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8b5a4-175">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-175">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8b5a4-176">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-176">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="8b5a4-177">Para obter mais informações, consulte este artigo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-177">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="8b5a4-178">Adicione uma nova C# classe à pasta **filtros** e nomeie-a *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-178">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="8b5a4-179">Essa pasta irá armazenar todos os filtros personalizados.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-179">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="8b5a4-180">Abra **CustomActionFilter.cs** e adicione uma referência aos namespaces **System. Web. Mvc** e **MvcMusicStore. Models** :</span><span class="sxs-lookup"><span data-stu-id="8b5a4-180">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="8b5a4-181">(Trecho de código – *ASP.net de ação personalizada do MVC 4-EX1-CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="8b5a4-181">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="8b5a4-182">Herde a classe **CustomActionFilter** de **ActionFilterAttribute** e, em seguida, torne a classe **CustomActionFilter** implementar a interface **IActionFilter** .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-182">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="8b5a4-183">Faça com que a classe **CustomActionFilter** substitua o método **OnActionExecuting** e adicione a lógica necessária para registrar a execução do filtro.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-183">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="8b5a4-184">Para fazer isso, adicione o seguinte código realçado na classe **CustomActionFilter** .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-184">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="8b5a4-185">(Trecho de código – *ASP.net de ação personalizada do MVC 4-EX1-LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="8b5a4-185">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="8b5a4-186">O método **OnActionExecuting** está usando **Entity Framework** para adicionar um novo registro ActionLog.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-186">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="8b5a4-187">Ele cria e preenche uma nova instância de entidade com as informações de contexto de **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-187">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="8b5a4-188">Você pode ler mais sobre a classe **ControllerContext** no [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-188">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="8b5a4-189">Tarefa 2-injetando um interceptador de código na classe do controlador de armazenamento</span><span class="sxs-lookup"><span data-stu-id="8b5a4-189">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="8b5a4-190">Nesta tarefa, você adicionará o filtro personalizado injetando-o a todas as classes de controlador e ações de controlador que serão registradas em log.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-190">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="8b5a4-191">Para fins deste exercício, a classe do controlador de loja terá um log.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-191">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="8b5a4-192">O método **OnActionExecuting** do filtro personalizado **ActionLogFilterAttribute** é executado quando um elemento injetado é chamado.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-192">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="8b5a4-193">Também é possível interceptar um método de controlador específico.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-193">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="8b5a4-194">Abra o **StoreController** em **MvcMusicStore\Controllers** e adicione uma referência ao namespace de **filtros** :</span><span class="sxs-lookup"><span data-stu-id="8b5a4-194">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="8b5a4-195">Insira o filtro personalizado **CustomActionFilter** na classe **StoreController** adicionando o atributo **[CustomActionFilter]** antes da declaração de classe.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-195">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > <span data-ttu-id="8b5a4-196">Quando um filtro é injetado em uma classe de controlador, todas as suas ações também são injetadas.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-196">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="8b5a4-197">Se você quiser aplicar o filtro somente para um conjunto de ações, precisará inserir **[CustomActionFilter]** em cada um deles:</span><span class="sxs-lookup"><span data-stu-id="8b5a4-197">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="8b5a4-198">Tarefa 3-executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="8b5a4-198">Task 3 - Running the Application</span></span>

<span data-ttu-id="8b5a4-199">Nesta tarefa, você testará se o filtro de log está funcionando.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-199">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="8b5a4-200">Você iniciará o aplicativo e visitará a loja e, em seguida, verificará as atividades registradas.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-200">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="8b5a4-201">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-201">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="8b5a4-202">Navegue até **/ActionLog** para ver o estado inicial do modo de exibição de log:</span><span class="sxs-lookup"><span data-stu-id="8b5a4-202">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="8b5a4-203">![Status do controlador de log antes da atividade da página](aspnet-mvc-4-custom-action-filters/_static/image3.png "Status do controlador de log antes da atividade da página")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-203">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="8b5a4-204">*Status do controlador de log antes da atividade da página*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-204">*Log tracker status before page activity*</span></span>

   > [!NOTE]
   > <span data-ttu-id="8b5a4-205">Por padrão, ele sempre mostrará um item que é gerado ao recuperar os gêneros existentes para o menu.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-205">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
   > 
   > <span data-ttu-id="8b5a4-206">Para fins de simplicidade, estamos limpando a tabela **ActionLog** cada vez que o aplicativo é executado, portanto, ele mostrará apenas os logs de cada verificação de tarefa específica.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-206">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
   > 
   > <span data-ttu-id="8b5a4-207">Talvez seja necessário remover o código a seguir da **sessão\_** método de início (na classe **global. asax** ), para salvar um log histórico de todas as ações executadas no controlador da loja.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-207">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="8b5a4-208">Clique em um dos **gêneros** no menu e execute algumas ações, como navegar por um álbum disponível.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-208">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="8b5a4-209">Navegue até **/ActionLog** e, se o log estiver vazio, pressione **F5** para atualizar a página.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-209">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="8b5a4-210">Verifique se suas visitas foram rastreadas:</span><span class="sxs-lookup"><span data-stu-id="8b5a4-210">Check that your visits were tracked:</span></span>

    <span data-ttu-id="8b5a4-211">![Log de ação com atividade registrada](aspnet-mvc-4-custom-action-filters/_static/image4.png "Log de ação com atividade registrada")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-211">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="8b5a4-212">*Log de ação com atividade registrada*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-212">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="8b5a4-213">Exercício 2: gerenciando vários filtros de ação</span><span class="sxs-lookup"><span data-stu-id="8b5a4-213">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="8b5a4-214">Neste exercício, você adicionará um segundo filtro de ação personalizada à classe StoreController e definirá a ordem específica na qual os dois filtros serão executados.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-214">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="8b5a4-215">Em seguida, você atualizará o código para registrar o filtro globalmente.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-215">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="8b5a4-216">Há diferentes opções a serem levadas em conta ao definir a ordem de execução dos filtros.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-216">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="8b5a4-217">Por exemplo, a propriedade Order e o escopo dos filtros:</span><span class="sxs-lookup"><span data-stu-id="8b5a4-217">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="8b5a4-218">Você pode definir um **escopo** para cada um dos filtros, por exemplo, você pode fazer o escopo de todos os filtros de ação a serem executados dentro do **escopo do controlador**e todos os filtros de autorização para serem executados no **escopo global**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-218">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="8b5a4-219">Os escopos têm uma ordem de execução definida.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-219">The scopes have a defined execution order.</span></span>

<span data-ttu-id="8b5a4-220">Além disso, cada filtro de ação tem uma propriedade Order, que é usada para determinar a ordem de execução no escopo do filtro.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-220">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="8b5a4-221">Para obter mais informações sobre a ordem de execução de filtros de ação personalizada, visite este artigo do MSDN: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-221">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="8b5a4-222">Tarefa 1: Criando um novo filtro de ação personalizado</span><span class="sxs-lookup"><span data-stu-id="8b5a4-222">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="8b5a4-223">Nesta tarefa, você criará um novo filtro de ação personalizado para injetar na classe StoreController, aprendendo a gerenciar a ordem de execução dos filtros.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-223">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="8b5a4-224">Abra a solução **inicial** localizada na pasta **\Source\Ex02-ManagingMultipleActionFilters\Begin** .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-224">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="8b5a4-225">Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-225">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="8b5a4-226">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-226">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="8b5a4-227">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-227">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="8b5a4-228">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-228">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="8b5a4-229">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-229">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="8b5a4-230">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-230">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="8b5a4-231">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-231">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="8b5a4-232">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-232">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="8b5a4-233">Para obter mais informações, consulte este artigo: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-233">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="8b5a4-234">Adicione uma nova C# classe à pasta **filtros** e nomeie-a *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-234">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="8b5a4-235">Abra **MyNewCustomActionFilter.cs** e adicione uma referência a **System. Web. Mvc** e ao namespace **MvcMusicStore. Models** :</span><span class="sxs-lookup"><span data-stu-id="8b5a4-235">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="8b5a4-236">(Trecho de código – *ASP.net de ação personalizada do MVC 4-EX2-MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="8b5a4-236">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="8b5a4-237">Substitua a declaração de classe padrão pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-237">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="8b5a4-238">(Trecho de código – *ASP.net de ação personalizada do MVC 4-EX2-MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="8b5a4-238">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="8b5a4-239">Esse filtro de ação personalizada é quase igual ao que você criou no exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-239">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="8b5a4-240">A principal diferença é que ele tem o *&quot;registrado pelo atributo&quot;* atualizado com esse novo nome de classe para identificar qual filtro registrou o log.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-240">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify which filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="8b5a4-241">Tarefa 2: injetando um novo interceptador de código na classe StoreController</span><span class="sxs-lookup"><span data-stu-id="8b5a4-241">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="8b5a4-242">Nesta tarefa, você adicionará um novo filtro personalizado à classe StoreController e executará a solução para verificar como os dois filtros funcionam juntos.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-242">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="8b5a4-243">Abra a classe **StoreController** localizada em **MvcMusicStore\Controllers** e insira o novo filtro personalizado **MyNewCustomActionFilter** na classe **StoreController** , como é mostrado no código a seguir.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-243">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="8b5a4-244">Agora, execute o aplicativo para ver como esses dois filtros de ação personalizados funcionam.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-244">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="8b5a4-245">Para fazer isso, pressione **F5** e aguarde até que o aplicativo seja iniciado.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-245">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="8b5a4-246">Navegue até **/ActionLog** para ver o estado inicial da exibição de log.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-246">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="8b5a4-247">![Status do controlador de log antes da atividade da página](aspnet-mvc-4-custom-action-filters/_static/image5.png "Status do controlador de log antes da atividade da página")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-247">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="8b5a4-248">*Status do controlador de log antes da atividade da página*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-248">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="8b5a4-249">Clique em um dos **gêneros** no menu e execute algumas ações, como navegar por um álbum disponível.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-249">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="8b5a4-250">Verifique esta hora; suas visitas foram rastreadas duas vezes: uma vez para cada um dos filtros de ação personalizada que você adicionou na classe **StorageController** .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-250">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="8b5a4-251">![Log de ação com atividade registrada](aspnet-mvc-4-custom-action-filters/_static/image6.png "Log de ação com atividade registrada")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-251">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="8b5a4-252">*Log de ação com atividade registrada*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-252">*Action log with activity logged*</span></span>
6. <span data-ttu-id="8b5a4-253">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-253">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="8b5a4-254">Tarefa 3: Gerenciando a ordenação de filtro</span><span class="sxs-lookup"><span data-stu-id="8b5a4-254">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="8b5a4-255">Nesta tarefa, você aprenderá a gerenciar a ordem de execução dos filtros usando a propriedade Order.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-255">In this task, you will learn how to manage the filters' execution order by using the Order property.</span></span>

1. <span data-ttu-id="8b5a4-256">Abra a classe **StoreController** localizada em **MvcMusicStore\Controllers** e especifique a propriedade **Order** em ambos os filtros, como mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-256">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="8b5a4-257">Agora, verifique como os filtros são executados, dependendo do valor da propriedade Order.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-257">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="8b5a4-258">Você verá que o filtro com o menor valor de pedido (**CustomActionFilter**) é o primeiro que é executado.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-258">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="8b5a4-259">Pressione **F5** e aguarde até que o aplicativo seja iniciado.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-259">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="8b5a4-260">Navegue até **/ActionLog** para ver o estado inicial da exibição de log.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-260">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="8b5a4-261">![Status do controlador de log antes da atividade da página](aspnet-mvc-4-custom-action-filters/_static/image7.png "Status do controlador de log antes da atividade da página")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-261">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="8b5a4-262">*Status do controlador de log antes da atividade da página*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-262">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="8b5a4-263">Clique em um dos **gêneros** no menu e execute algumas ações, como navegar por um álbum disponível.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-263">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="8b5a4-264">Verifique se, desta vez, suas visitas foram rastreadas ordenadas pelo valor de ordem dos filtros: **CustomActionFilter** logs ' primeiro.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-264">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="8b5a4-265">![Log de ação com atividade registrada](aspnet-mvc-4-custom-action-filters/_static/image8.png "Log de ação com atividade registrada")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-265">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="8b5a4-266">*Log de ação com atividade registrada*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-266">*Action log with activity logged*</span></span>
6. <span data-ttu-id="8b5a4-267">Agora, você atualizará o valor da ordem dos filtros e verificará como a ordem de registro em log é alterada.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-267">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="8b5a4-268">Na classe **StoreController** , atualize o valor de ordem dos filtros, como mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-268">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="8b5a4-269">Execute o aplicativo novamente pressionando **F5**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-269">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="8b5a4-270">Clique em um dos **gêneros** no menu e execute algumas ações, como navegar por um álbum disponível.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-270">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="8b5a4-271">Verifique se, desta vez, os logs criados pelo filtro **MyNewCustomActionFilter** aparecem primeiro.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-271">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="8b5a4-272">![Log de ação com atividade registrada](aspnet-mvc-4-custom-action-filters/_static/image9.png "Log de ação com atividade registrada")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-272">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="8b5a4-273">*Log de ação com atividade registrada*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-273">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="8b5a4-274">Tarefa 4: registrando filtros globalmente</span><span class="sxs-lookup"><span data-stu-id="8b5a4-274">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="8b5a4-275">Nesta tarefa, você atualizará a solução para registrar o novo filtro (**MyNewCustomActionFilter**) como um filtro global.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-275">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="8b5a4-276">Ao fazer isso, ele será disparado por todas as ações executadas no aplicativo e não apenas nos StoreControllers como na tarefa anterior.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-276">By doing this, it will be triggered by all the actions performed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="8b5a4-277">Na classe **StoreController** , remova o atributo **[MyNewCustomActionFilter]** e a propriedade Order de **[CustomActionFilter]** .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-277">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="8b5a4-278">Sua tela deverá ter a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="8b5a4-278">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="8b5a4-279">Abra o arquivo **global. asax** e localize o **aplicativo\_método iniciar** .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-279">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="8b5a4-280">Observe que sempre que o aplicativo é iniciado, ele está registrando os filtros globais chamando o método **RegisterGlobalFilters** na classe **FilterConfig** .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-280">Notice that each time the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="8b5a4-281">![Registrando filtros globais no global. asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registrando filtros globais no global. asax")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-281">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="8b5a4-282">*Registrando filtros globais no global. asax*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-282">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="8b5a4-283">Abra o arquivo **FilterConfig.cs** no **aplicativo\_pasta inicial** .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-283">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="8b5a4-284">Adicione uma referência ao usando System. Web. Mvc; usando MvcMusicStore. Filters; namespace.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-284">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="8b5a4-285">Atualize o método **RegisterGlobalFilters** adicionando seu filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-285">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="8b5a4-286">Para fazer isso, adicione o código realçado:</span><span class="sxs-lookup"><span data-stu-id="8b5a4-286">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="8b5a4-287">Execute o aplicativo pressionando **F5**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-287">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="8b5a4-288">Clique em um dos **gêneros** no menu e execute algumas ações, como navegar por um álbum disponível.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-288">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="8b5a4-289">Verifique se agora **[MyNewCustomActionFilter]** está sendo injetado em HomeController e ActionLogController também.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-289">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="8b5a4-290">![Log de ação com atividade registrada](aspnet-mvc-4-custom-action-filters/_static/image11.png "Log de ação com atividade registrada")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-290">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="8b5a4-291">*Log de ação com atividade global registrada*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-291">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="8b5a4-292">Além disso, você pode implantar esse aplicativo em sites do Windows Azure seguindo [o apêndice B: publicando um aplicativo ASP.NET MVC 4 usando implantação da Web](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-292">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="8b5a4-293">Resumo</span><span class="sxs-lookup"><span data-stu-id="8b5a4-293">Summary</span></span>

<span data-ttu-id="8b5a4-294">Ao concluir este laboratório prático, você aprendeu a estender um filtro de ação para executar ações personalizadas.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-294">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="8b5a4-295">Você também aprendeu como injetar qualquer filtro em seus controladores de página.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-295">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="8b5a4-296">Os seguintes conceitos foram usados:</span><span class="sxs-lookup"><span data-stu-id="8b5a4-296">The following concepts were used:</span></span>

- <span data-ttu-id="8b5a4-297">Como criar filtros de ação personalizados com a classe ASP.NET MVC ActionFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="8b5a4-297">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="8b5a4-298">Como injetar filtros em controladores MVC do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8b5a4-298">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="8b5a4-299">Como gerenciar a ordenação de filtro usando a propriedade Order</span><span class="sxs-lookup"><span data-stu-id="8b5a4-299">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="8b5a4-300">Como registrar filtros globalmente</span><span class="sxs-lookup"><span data-stu-id="8b5a4-300">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="8b5a4-301">Apêndice A: Instalando o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="8b5a4-301">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="8b5a4-302">Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou outra versão &quot;Express&quot; usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-302">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="8b5a4-303">As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-303">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="8b5a4-304">Ir para [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-304">Go to [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="8b5a4-305">Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e pesquisar o produto &quot;<em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-305">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="8b5a4-306">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-306">Click on **Install Now**.</span></span> <span data-ttu-id="8b5a4-307">Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-307">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="8b5a4-308">Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-308">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="8b5a4-309">![Instalar Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-309">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="8b5a4-310">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-310">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="8b5a4-311">Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-311">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitando os termos de licença](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="8b5a4-313">*Aceitando os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-313">*Accepting the license terms*</span></span>
5. <span data-ttu-id="8b5a4-314">Aguarde até que o processo de download e instalação seja concluído.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-314">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="8b5a4-316">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-316">*Installation progress*</span></span>
6. <span data-ttu-id="8b5a4-317">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-317">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="8b5a4-319">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-319">*Installation completed*</span></span>
7. <span data-ttu-id="8b5a4-320">Clique em **sair** para fechar Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-320">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="8b5a4-321">Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot;**vs Express**&quot;e, em seguida, clique no bloco **vs Express para Web** .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-321">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Bloco VS Express para Web](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="8b5a4-323">*Bloco VS Express para Web*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-323">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="8b5a4-324">Apêndice B: publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web</span><span class="sxs-lookup"><span data-stu-id="8b5a4-324">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="8b5a4-325">Este apêndice mostrará como criar um novo site da Web do Windows Azure Portal de Gerenciamento e publicar o aplicativo obtido seguindo o laboratório, aproveitando o recurso de publicação de Implantação da Web fornecido pelo Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-325">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="8b5a4-326">Tarefa 1-Criando um novo site no portal do Windows Azure</span><span class="sxs-lookup"><span data-stu-id="8b5a4-326">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="8b5a4-327">Acesse o [portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-327">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8b5a4-328">Com o Windows Azure, você pode hospedar 10 sites da Web de ASP.NET gratuitamente e, em seguida, dimensionar conforme o tráfego cresce.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-328">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="8b5a4-329">Você pode se inscrever [aqui](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-329">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="8b5a4-330">![Fazer logon no Windows portal do Azure](aspnet-mvc-4-custom-action-filters/_static/image17.png "Fazer logon no Windows portal do Azure")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-330">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="8b5a4-331">*Fazer logon no Windows Azure Portal de Gerenciamento*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-331">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="8b5a4-332">Clique em **novo** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-332">Click **New** on the command bar.</span></span>

    <span data-ttu-id="8b5a4-333">![Criando um novo site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Criando um novo site")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-333">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="8b5a4-334">*Criando um novo site*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-334">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="8b5a4-335">Clique em **computação** | **site**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-335">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="8b5a4-336">Em seguida, selecione a opção **criação rápida** .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-336">Then select **Quick Create** option.</span></span> <span data-ttu-id="8b5a4-337">Forneça uma URL disponível para o novo site e clique em **criar site**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-337">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8b5a4-338">Um site do Windows Azure é o host de um aplicativo Web em execução na nuvem que você pode controlar e gerenciar.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-338">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="8b5a4-339">A opção criação rápida permite que você implante um aplicativo Web concluído no site do Windows Azure de fora do Portal.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-339">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="8b5a4-340">Ele não inclui etapas para configurar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-340">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="8b5a4-341">![Criando um novo site usando a criação rápida](aspnet-mvc-4-custom-action-filters/_static/image19.png "Criando um novo site usando a criação rápida")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-341">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="8b5a4-342">*Criando um novo site usando a criação rápida*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-342">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="8b5a4-343">Aguarde até que o novo **site** seja criado.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-343">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="8b5a4-344">Depois que o site for criado, clique no link sob a coluna **URL** .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-344">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="8b5a4-345">Verifique se o novo site está funcionando.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-345">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="8b5a4-346">![Navegando até o novo site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Navegando até o novo site")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-346">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="8b5a4-347">*Navegando até o novo site*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-347">*Browsing to the new web site*</span></span>

    <span data-ttu-id="8b5a4-348">![Site em execução](aspnet-mvc-4-custom-action-filters/_static/image21.png "Site em execução")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-348">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="8b5a4-349">*Site em execução*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-349">*Web site running*</span></span>
6. <span data-ttu-id="8b5a4-350">Volte para o portal e clique no nome do site na coluna **nome** para exibir as páginas de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-350">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="8b5a4-351">![Abrindo as páginas de gerenciamento de site](aspnet-mvc-4-custom-action-filters/_static/image22.png "Abrindo as páginas de gerenciamento de site")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-351">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="8b5a4-352">*Abrindo as páginas de gerenciamento de site*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-352">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="8b5a4-353">Na página **painel** , na seção **visão rápida** , clique no link **baixar perfil de publicação** .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-353">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8b5a4-354">O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo Web em um site do Windows Azure para cada método de publicação habilitado.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-354">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="8b5a4-355">O perfil de publicação contém as URLs, as credenciais de usuário e as cadeias de conexão de banco de dados necessárias para conectar-se e autenticar cada um dos pontos de extremidade para os quais um método de publicação é habilitado.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-355">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="8b5a4-356">**O Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** e **Microsoft Visual Studio 2012** dão suporte à leitura de perfis de publicação para automatizar a configuração desses programas para a publicação de aplicativos Web em sites do Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="8b5a4-357">![Baixando o perfil de publicação do site](aspnet-mvc-4-custom-action-filters/_static/image23.png "Baixando o perfil de publicação do site")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-357">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="8b5a4-358">*Baixando o perfil de publicação do site*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-358">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="8b5a4-359">Baixe o arquivo de perfil de publicação em um local conhecido.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-359">Download the publish profile file to a known location.</span></span> <span data-ttu-id="8b5a4-360">Neste exercício, você verá como usar esse arquivo para publicar um aplicativo Web em um site do Windows Azure a partir do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-360">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="8b5a4-361">![Salvando o arquivo de perfil de publicação](aspnet-mvc-4-custom-action-filters/_static/image24.png "Salvando o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-361">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="8b5a4-362">*Salvando o arquivo de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-362">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="8b5a4-363">Tarefa 2-Configurando o servidor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="8b5a4-363">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="8b5a4-364">Se seu aplicativo utiliza bancos de dados SQL Server, você precisará criar um servidor de banco de dados SQL.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-364">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="8b5a4-365">Se você quiser implantar um aplicativo simples que não usa SQL Server você pode ignorar essa tarefa.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-365">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="8b5a4-366">Você precisará de um servidor do banco de dados SQL para armazenar o banco de dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-366">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="8b5a4-367">Você pode exibir os servidores do banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em bancos de dados **sql** | **servidores** | **painel do servidor**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-367">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="8b5a4-368">Se você não tiver um servidor criado, poderá criar um usando o botão **Adicionar** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-368">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="8b5a4-369">Anote o nome do **servidor e a URL, o nome de logon e a senha do administrador**, pois você irá usá-los nas próximas tarefas.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-369">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="8b5a4-370">Não crie o banco de dados ainda, pois ele será criado em um estágio posterior.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-370">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="8b5a4-371">![Painel do servidor do banco de dados SQL](aspnet-mvc-4-custom-action-filters/_static/image25.png "Painel do servidor do banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-371">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="8b5a4-372">*Painel do servidor do banco de dados SQL*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-372">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="8b5a4-373">Na próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, será necessário incluir o endereço IP local na lista de **endereços IP permitidos**do servidor.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-373">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="8b5a4-374">Para fazer isso, clique em **Configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o nas caixas de texto endereço IP **inicial** e **endereço IP final** e clique no botão ![adicionar-cliente-IP-endereço-OK-botão](aspnet-mvc-4-custom-action-filters/_static/image26.png).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-374">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![Adicionando endereço IP do cliente](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="8b5a4-376">*Adicionando endereço IP do cliente*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-376">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="8b5a4-377">Depois que o **endereço IP do cliente** for adicionado à lista endereços IP permitidos, clique em **salvar** para confirmar as alterações.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-377">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar alterações](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="8b5a4-379">*Confirmar alterações*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-379">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="8b5a4-380">Tarefa 3-publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web</span><span class="sxs-lookup"><span data-stu-id="8b5a4-380">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="8b5a4-381">Volte para a solução ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-381">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="8b5a4-382">Na **Gerenciador de soluções**, clique com o botão direito do mouse no projeto de site e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-382">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="8b5a4-383">![Publicando o aplicativo](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publicando o aplicativo")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-383">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="8b5a4-384">*Publicando o site*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-384">*Publishing the web site*</span></span>
2. <span data-ttu-id="8b5a4-385">Importe o perfil de publicação salvo na primeira tarefa.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-385">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="8b5a4-386">![Importando o perfil de publicação](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importando o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-386">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="8b5a4-387">*Importando perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-387">*Importing publish profile*</span></span>
3. <span data-ttu-id="8b5a4-388">Clique em **validar conexão**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-388">Click **Validate Connection**.</span></span> <span data-ttu-id="8b5a4-389">Depois que a validação for concluída, clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-389">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="8b5a4-390">A validação será concluída quando você vir uma marca de seleção verde ao lado do botão Validar conexão.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-390">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="8b5a4-391">![Validando conexão](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validando conexão")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-391">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="8b5a4-392">*Validando conexão*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-392">*Validating connection*</span></span>
4. <span data-ttu-id="8b5a4-393">Na página **configurações** , na seção **bancos** de dados, clique no botão ao lado da caixa de texto da sua conexão de banco (ou seja, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-393">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="8b5a4-394">![Configuração de implantação da Web](aspnet-mvc-4-custom-action-filters/_static/image32.png "Configuração de implantação da Web")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-394">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="8b5a4-395">*Configuração de implantação da Web*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-395">*Web deploy configuration*</span></span>
5. <span data-ttu-id="8b5a4-396">Configure a conexão de banco de dados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="8b5a4-396">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="8b5a4-397">No **nome do servidor** , digite a URL do servidor do banco de dados SQL usando o prefixo *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="8b5a4-397">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="8b5a4-398">Em **nome de usuário** , digite o nome de logon do administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-398">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="8b5a4-399">Em **senha** , digite a senha de logon do administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-399">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="8b5a4-400">Digite um novo nome de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-400">Type a new database name.</span></span>

     <span data-ttu-id="8b5a4-401">![Configurando a cadeia de conexão de destino](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configurando a cadeia de conexão de destino")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-401">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="8b5a4-402">*Configurando a cadeia de conexão de destino*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-402">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="8b5a4-403">Em seguida, clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-403">Then click **OK**.</span></span> <span data-ttu-id="8b5a4-404">Quando for solicitado a criar o banco de dados, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-404">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="8b5a4-405">![Criando o banco de dados](aspnet-mvc-4-custom-action-filters/_static/image34.png "Criando a cadeia de caracteres do banco de dados")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-405">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="8b5a4-406">*Criando o banco de dados*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-406">*Creating the database*</span></span>
7. <span data-ttu-id="8b5a4-407">A cadeia de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto conexão padrão.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-407">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="8b5a4-408">Em seguida, clique em **Próximo**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-408">Then click **Next**.</span></span>

    <span data-ttu-id="8b5a4-409">![Cadeia de conexão apontando para o banco de dados SQL](aspnet-mvc-4-custom-action-filters/_static/image35.png "Cadeia de conexão apontando para o banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-409">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="8b5a4-410">*Cadeia de conexão apontando para o banco de dados SQL*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-410">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="8b5a4-411">Na página **Visualização** , clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-411">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="8b5a4-412">![Publicando o aplicativo Web](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publicando o aplicativo Web")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-412">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="8b5a4-413">*Publicando o aplicativo Web*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-413">*Publishing the web application*</span></span>
9. <span data-ttu-id="8b5a4-414">Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-414">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="8b5a4-415">Apêndice C: usando trechos de código</span><span class="sxs-lookup"><span data-stu-id="8b5a4-415">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="8b5a4-416">Com trechos de código, você tem todo o código de que precisa ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-416">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="8b5a4-417">O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-417">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="8b5a4-418">![Usando trechos de código do Visual Studio para inserir código em seu projeto](aspnet-mvc-4-custom-action-filters/_static/image37.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-418">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="8b5a4-419">*Usando trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-419">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="8b5a4-420">***Para adicionar um trecho de código usando o tecladoC# (somente)***</span><span class="sxs-lookup"><span data-stu-id="8b5a4-420">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="8b5a4-421">Coloque o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-421">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="8b5a4-422">Comece digitando o nome do trecho de código (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-422">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="8b5a4-423">Observe como o IntelliSense exibe nomes de trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-423">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="8b5a4-424">Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).</span><span class="sxs-lookup"><span data-stu-id="8b5a4-424">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="8b5a4-425">Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-425">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="8b5a4-426">![Comece a digitar o nome do trecho](aspnet-mvc-4-custom-action-filters/_static/image38.png "Comece a digitar o nome do trecho")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-426">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="8b5a4-427">*Comece a digitar o nome do trecho*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-427">*Start typing the snippet name*</span></span>

<span data-ttu-id="8b5a4-428">![Pressione Tab para selecionar o trecho realçado](aspnet-mvc-4-custom-action-filters/_static/image39.png "Pressione Tab para selecionar o trecho realçado")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-428">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="8b5a4-429">*Pressione Tab para selecionar o trecho realçado*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-429">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="8b5a4-430">![Pressione Tab novamente e o trecho será expandido](aspnet-mvc-4-custom-action-filters/_static/image40.png "Pressione Tab novamente e o trecho será expandido")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-430">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="8b5a4-431">*Pressione Tab novamente e o trecho será expandido*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-431">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="8b5a4-432">***Para adicionar um trecho de código usando o mouseC#(, Visual Basic e XML)*** uma.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-432">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="8b5a4-433">Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-433">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="8b5a4-434">Selecione **Inserir trecho** seguido por **meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-434">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="8b5a4-435">Selecione o trecho relevante na lista clicando nele.</span><span class="sxs-lookup"><span data-stu-id="8b5a4-435">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="8b5a4-436">![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](aspnet-mvc-4-custom-action-filters/_static/image41.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-436">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="8b5a4-437">*Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-437">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="8b5a4-438">![Selecione o trecho relevante na lista clicando nele](aspnet-mvc-4-custom-action-filters/_static/image42.png "Selecione o trecho relevante na lista clicando nele")</span><span class="sxs-lookup"><span data-stu-id="8b5a4-438">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="8b5a4-439">*Selecione o trecho relevante na lista clicando nele*</span><span class="sxs-lookup"><span data-stu-id="8b5a4-439">*Pick the relevant snippet from the list, by clicking on it*</span></span>
