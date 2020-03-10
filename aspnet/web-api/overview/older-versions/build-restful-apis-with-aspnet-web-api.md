---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Criar APIs RESTful com ASP.NET Web API-ASP.NET 4. x
author: rick-anderson
description: 'Laboratório prático: Use a API da Web no ASP.NET 4. x para criar uma API REST simples para um aplicativo do Gerenciador de contatos.'
ms.author: riande
ms.date: 02/18/2013
ms.custom: seoapril2019
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 35b115d6b4f84084e78e429bbb4842670e57bba4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621810"
---
# <a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="f2ccf-103">Criar APIs RESTful com ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="f2ccf-103">Build RESTful APIs with ASP.NET Web API</span></span>

<span data-ttu-id="f2ccf-104">por [equipe de acampamentos da Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="f2ccf-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="f2ccf-105">Laboratório prático: Use a API da Web no ASP.NET 4. x para criar uma API REST simples para um aplicativo do Gerenciador de contatos.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-105">Hands on lab: Use Web API in ASP.NET 4.x to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="f2ccf-106">Você também criará um cliente para consumir a API.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-106">You will also build a client to consume the API.</span></span>

<span data-ttu-id="f2ccf-107">Nos últimos anos, ficou claro que HTTP não serve apenas para servir páginas HTML.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-107">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="f2ccf-108">Também é uma plataforma poderosa para a criação de APIs da Web, usando alguns verbos (GET, POST e assim por diante), além de alguns conceitos simples, como *URIs* e *cabeçalhos*.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-108">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="f2ccf-109">ASP.NET Web API é um conjunto de componentes que simplificam a programação HTTP.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-109">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="f2ccf-110">Como ele é criado sobre o tempo de execução MVC do ASP.NET, a API da Web manipula automaticamente os detalhes de transporte de nível inferior do HTTP.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-110">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="f2ccf-111">Ao mesmo tempo, a API Web expõe naturalmente o modelo de programação HTTP.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-111">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="f2ccf-112">Na verdade, uma meta da API Web é *não* abstrair a realidade do http.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-112">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="f2ccf-113">Como resultado, a API da Web é flexível e fácil de estender.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-113">As a result, Web API is both flexible and easy to extend.</span></span>  <span data-ttu-id="f2ccf-114">O estilo de arquitetura REST provou ser uma maneira eficaz de aproveitar HTTP, embora, certamente, não seja a única abordagem válida para HTTP.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-114">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="f2ccf-115">O Gerenciador de contatos irá expor o RESTful para listagem, adição e remoção de contatos, entre outros.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-115">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> 

<span data-ttu-id="f2ccf-116">Este laboratório requer uma compreensão básica do HTTP, do REST e pressupõe que você tenha um conhecimento funcional básico de HTML, JavaScript e jQuery.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-116">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="f2ccf-117">O site da ASP.NET tem uma área dedicada à estrutura de ASP.NET Web API em [https://asp.net/web-api](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="f2ccf-117">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="f2ccf-118">Este site continuará a fornecer informações mais recentes, amostras e notícias relacionadas à API da Web, portanto, verifique com frequência se você gostaria de se aprofundar na arte de criar APIs Web personalizadas disponíveis para praticamente qualquer estrutura de dispositivo ou de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-118">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="f2ccf-119">ASP.NET Web API, semelhante ao ASP.NET MVC 4, tem grande flexibilidade em termos de separar a camada de serviço dos controladores, permitindo que você use várias das estruturas de injeção de dependência disponíveis razoavelmente fáceis.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-119">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="f2ccf-120">Há um bom exemplo no MSDN que mostra como usar o Ninject para injeção de dependência em um projeto ASP.NET Web API que você pode baixá-lo [aqui](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="f2ccf-120">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="f2ccf-121">Todos os códigos de exemplo e trechos de código estão incluídos no kit de treinamento do acampamentos da Web, disponível em [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="f2ccf-121">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="f2ccf-122">Objetivos</span><span class="sxs-lookup"><span data-stu-id="f2ccf-122">Objectives</span></span>

<span data-ttu-id="f2ccf-123">Neste laboratório prático, você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="f2ccf-123">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="f2ccf-124">Implementar uma API Web RESTful</span><span class="sxs-lookup"><span data-stu-id="f2ccf-124">Implement a RESTful Web API</span></span>
- <span data-ttu-id="f2ccf-125">Chamar a API de um cliente HTML</span><span class="sxs-lookup"><span data-stu-id="f2ccf-125">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="f2ccf-126">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="f2ccf-126">Prerequisites</span></span>

<span data-ttu-id="f2ccf-127">O seguinte é necessário para concluir este laboratório prático:</span><span class="sxs-lookup"><span data-stu-id="f2ccf-127">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="f2ccf-128">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia o [Apêndice B](#AppendixB) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="f2ccf-128">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="f2ccf-129">Instalação</span><span class="sxs-lookup"><span data-stu-id="f2ccf-129">Setup</span></span>

<span data-ttu-id="f2ccf-130">**Instalando trechos de código**</span><span class="sxs-lookup"><span data-stu-id="f2ccf-130">**Installing Code Snippets**</span></span>

<span data-ttu-id="f2ccf-131">Para sua conveniência, grande parte do código que você gerenciará ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-131">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="f2ccf-132">Para instalar os trechos de código, execute o arquivo **.\Source\Setup\CodeSnippets.VSI** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-132">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="f2ccf-133">Se você não estiver familiarizado com os trechos de Visual Studio Code e quiser saber como usá-los, consulte o apêndice deste documento &quot;[Apêndice a: usando trechos de código](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-133">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="f2ccf-134">Exercícios</span><span class="sxs-lookup"><span data-stu-id="f2ccf-134">Exercises</span></span>

<span data-ttu-id="f2ccf-135">Este laboratório prático inclui o seguinte exercício:</span><span class="sxs-lookup"><span data-stu-id="f2ccf-135">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="f2ccf-136">Exercício 1: criar uma API Web somente leitura</span><span class="sxs-lookup"><span data-stu-id="f2ccf-136">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="f2ccf-137">Exercício 2: criar uma API da Web de leitura/gravação</span><span class="sxs-lookup"><span data-stu-id="f2ccf-137">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="f2ccf-138">Exercício 3: consumir a API Web de um cliente HTML</span><span class="sxs-lookup"><span data-stu-id="f2ccf-138">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="f2ccf-139">Cada exercício é acompanhado por uma pasta **final** que contém a solução resultante que você deve obter depois de concluir os exercícios.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-139">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="f2ccf-140">Você pode usar essa solução como um guia se precisar de ajuda adicional para trabalhar com os exercícios.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-140">You can use this solution as a guide if you need additional help working through the exercises.</span></span>

<span data-ttu-id="f2ccf-141">Tempo estimado para concluir este laboratório: **60 minutos**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-141">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="f2ccf-142">Exercício 1: criar uma API Web somente leitura</span><span class="sxs-lookup"><span data-stu-id="f2ccf-142">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="f2ccf-143">Neste exercício, você vai implementar os métodos GET somente leitura para o Gerenciador de contatos.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-143">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="f2ccf-144">Tarefa 1-criando o projeto de API</span><span class="sxs-lookup"><span data-stu-id="f2ccf-144">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="f2ccf-145">Nesta tarefa, você usará os novos modelos de projeto Web ASP.NET para criar um aplicativo Web da API Web.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-145">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="f2ccf-146">Execute o **Visual Studio 2012 Express para Web**, para fazer isso, vá para **iniciar** e digite **vs Express para Web** pressione **Enter**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-146">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="f2ccf-147">No menu **arquivo** , selecione **novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-147">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="f2ccf-148">Selecione o **Visual C# |** Tipo de projeto Web no modo de exibição de árvore do tipo de projeto e, em seguida, selecione o tipo de projeto de **aplicativo Web ASP.NET MVC 4** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-148">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="f2ccf-149">Defina o **nome** do projeto como *ContactManager* e o **nome da solução** como *begin*e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-149">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="f2ccf-150">![Criando um novo projeto de aplicativo Web do ASP.NET MVC 4,0](build-restful-apis-with-aspnet-web-api/_static/image1.png "Criando um novo projeto de aplicativo Web do ASP.NET MVC 4,0")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-150">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    <span data-ttu-id="f2ccf-151">*Criando um novo projeto de aplicativo Web do ASP.NET MVC 4,0*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-151">*Creating a new ASP.NET MVC 4.0 Web Application Project*</span></span>
3. <span data-ttu-id="f2ccf-152">Na caixa de diálogo tipo de projeto do ASP.NET MVC 4, selecione o tipo de projeto de **API Web** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-152">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="f2ccf-153">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-153">Click **OK**.</span></span>

    <span data-ttu-id="f2ccf-154">![Especificando o tipo de projeto de API Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "Especificando o tipo de projeto de API Web")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-154">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    <span data-ttu-id="f2ccf-155">*Especificando o tipo de projeto de API Web*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-155">*Specifying the Web API project type*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="f2ccf-156">Tarefa 2-Criando os controladores de API do Gerenciador de contatos</span><span class="sxs-lookup"><span data-stu-id="f2ccf-156">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="f2ccf-157">Nesta tarefa, você criará as classes de controlador nas quais os métodos de API residirão.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-157">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="f2ccf-158">Exclua o arquivo chamado **ValuesController.cs** na pasta **controladores** do projeto.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-158">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="f2ccf-159">Clique com o botão direito do mouse na pasta **controladores** no projeto e selecione **Adicionar | Controlador** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-159">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="f2ccf-160">![Adicionando um novo controlador ao projeto](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adicionando um novo controlador ao projeto")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-160">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    <span data-ttu-id="f2ccf-161">*Adicionando um novo controlador ao projeto*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-161">*Adding a new controller to the project*</span></span>
3. <span data-ttu-id="f2ccf-162">Na caixa de diálogo **Adicionar controlador** exibida, selecione **controlador de API vazio** no menu modelo.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-162">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="f2ccf-163">Nomeie a classe de controlador **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-163">Name the controller class **ContactController**.</span></span> <span data-ttu-id="f2ccf-164">Em seguida, clique em **Adicionar.**</span><span class="sxs-lookup"><span data-stu-id="f2ccf-164">Then, click **Add.**</span></span>

    <span data-ttu-id="f2ccf-165">![Usando a caixa de diálogo Adicionar controlador para criar um novo controlador de API Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "Usando a caixa de diálogo Adicionar controlador para criar um novo controlador de API Web")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-165">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    <span data-ttu-id="f2ccf-166">*Usando a caixa de diálogo Adicionar controlador para criar um novo controlador de API Web*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-166">*Using the Add Controller dialog to create a new Web API controller*</span></span>
4. <span data-ttu-id="f2ccf-167">Adicione o código a seguir ao **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-167">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="f2ccf-168">(Trecho de código- *laboratório de API Web-Ex01-método de API Get*)</span><span class="sxs-lookup"><span data-stu-id="f2ccf-168">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="f2ccf-169">Pressione **F5** para depurar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-169">Press **F5** to debug the application.</span></span> <span data-ttu-id="f2ccf-170">O home page padrão para um projeto de API Web deve aparecer.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-170">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="f2ccf-171">![O home page padrão de um aplicativo ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "O home page padrão de um aplicativo ASP.NET Web API")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-171">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    <span data-ttu-id="f2ccf-172">*O home page padrão de um aplicativo ASP.NET Web API*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-172">*The default home page of an ASP.NET Web API application*</span></span>
6. <span data-ttu-id="f2ccf-173">Na janela do Internet Explorer, pressione a tecla **F12** para abrir a janela **ferramentas para desenvolvedores** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-173">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="f2ccf-174">Clique na guia **rede** e, em seguida, clique no botão **Iniciar captura** para começar a capturar o tráfego de rede na janela.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-174">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="f2ccf-175">![Abrindo a guia rede e iniciando a captura de rede](build-restful-apis-with-aspnet-web-api/_static/image6.png "Abrindo a guia rede e iniciando a captura de rede")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-175">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    <span data-ttu-id="f2ccf-176">*Abrindo a guia rede e iniciando a captura de rede*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-176">*Opening the network tab and initiating network capture*</span></span>
7. <span data-ttu-id="f2ccf-177">Acrescente a URL na barra de endereços do navegador com **/API/Contact** e pressione Enter.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-177">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="f2ccf-178">Os detalhes da transmissão aparecerão na janela captura de rede.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-178">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="f2ccf-179">Observe que o tipo MIME da resposta é **Application/JSON**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-179">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="f2ccf-180">Isso demonstra como o formato de saída padrão é JSON.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-180">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="f2ccf-181">![Exibindo a saída da solicitação da API Web na exibição de rede](build-restful-apis-with-aspnet-web-api/_static/image7.png "Exibindo a saída da solicitação da API Web na exibição de rede")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-181">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    <span data-ttu-id="f2ccf-182">*Exibindo a saída da solicitação da API Web na exibição de rede*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-182">*Viewing the output of the Web API request in the Network view*</span></span>

    > [!NOTE]
    > <span data-ttu-id="f2ccf-183">O comportamento padrão do Internet Explorer 10 neste momento será perguntar se o usuário deseja salvar ou abrir o fluxo resultante da chamada à API Web.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-183">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="f2ccf-184">A saída será um arquivo de texto que contém o resultado JSON da chamada de URL da API Web.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-184">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="f2ccf-185">Não cancele a caixa de diálogo para poder assistir ao conteúdo da resposta por meio da janela de ferramentas de desenvolvedores.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-185">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="f2ccf-186">Clique no botão **ir para exibição detalhada** para ver mais detalhes sobre a resposta dessa chamada à API.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-186">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="f2ccf-187">![Alternar para exibição detalhada](build-restful-apis-with-aspnet-web-api/_static/image8.png "Alternar para exibição de detalhes")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-187">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    <span data-ttu-id="f2ccf-188">*Alternar para exibição detalhada*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-188">*Switch to Detailed View*</span></span>
9. <span data-ttu-id="f2ccf-189">Clique na guia **corpo da resposta** para exibir o texto real da resposta JSON.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-189">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="f2ccf-190">![Exibindo o texto de saída JSON no monitor de rede](build-restful-apis-with-aspnet-web-api/_static/image9.png "Exibindo o texto de saída JSON no monitor de rede")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-190">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    <span data-ttu-id="f2ccf-191">*Exibindo o texto de saída JSON no monitor de rede*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-191">*Viewing the JSON output text in the network monitor*</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="f2ccf-192">Tarefa 3-criando os modelos de contato e aumentando o controlador de contato</span><span class="sxs-lookup"><span data-stu-id="f2ccf-192">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="f2ccf-193">Nesta tarefa, você criará as classes de controlador nas quais os métodos de API residirão.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-193">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="f2ccf-194">Clique com o botão direito do mouse na pasta **modelos** e selecione **Adicionar | Classe...** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-194">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="f2ccf-195">![Adicionando um novo modelo ao aplicativo Web](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adicionando um novo modelo ao aplicativo Web")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-195">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    <span data-ttu-id="f2ccf-196">*Adicionando um novo modelo ao aplicativo Web*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-196">*Adding a new model to the web application*</span></span>
2. <span data-ttu-id="f2ccf-197">Na caixa de diálogo **Adicionar novo item** , nomeie o novo arquivo **Contact.cs** e clique em **Adicionar.**</span><span class="sxs-lookup"><span data-stu-id="f2ccf-197">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="f2ccf-198">![Criando o novo arquivo de classe de contato](build-restful-apis-with-aspnet-web-api/_static/image11.png "Criando o novo arquivo de classe de contato")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-198">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    <span data-ttu-id="f2ccf-199">*Criando o novo arquivo de classe de contato*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-199">*Creating the new Contact class file*</span></span>
3. <span data-ttu-id="f2ccf-200">Adicione o seguinte código realçado à classe **Contact** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-200">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="f2ccf-201">(Trecho de código- *laboratório da API Web-Ex01-classe de contato*)</span><span class="sxs-lookup"><span data-stu-id="f2ccf-201">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="f2ccf-202">Na classe **ContactController** , selecione a palavra **cadeia de caracteres** na definição de método do método **Get** e digite a palavra *contato*.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-202">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="f2ccf-203">Depois que a palavra for digitada, um indicador será exibido no início da palavra **contato**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-203">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="f2ccf-204">Mantenha pressionada a tecla **Ctrl** e pressione a tecla period (.) ou clique no ícone usando o mouse para abrir a caixa de diálogo assistência no editor de códigos, para preencher automaticamente a diretiva **using** para o namespace Models.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-204">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Usando a assistência do IntelliSense para declarações de namespace](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    <span data-ttu-id="f2ccf-206">*Usando a assistência do IntelliSense para declarações de namespace*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-206">*Using Intellisense assistance for namespace declarations*</span></span>
5. <span data-ttu-id="f2ccf-207">Modifique o código para o método **Get** para que ele retorne uma matriz de instâncias de modelo de contato.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-207">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="f2ccf-208">(Trecho de código- *laboratório da API Web-Ex01-retornando uma lista de contatos*)</span><span class="sxs-lookup"><span data-stu-id="f2ccf-208">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="f2ccf-209">Pressione **F5** para depurar o aplicativo Web no navegador.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-209">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="f2ccf-210">Para exibir as alterações feitas na saída de resposta da API, execute as etapas a seguir.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-210">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="f2ccf-211">Depois que o navegador for aberto, pressione **F12** se as ferramentas de desenvolvedor ainda não estiverem abertas.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-211">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="f2ccf-212">Clique na guia **rede** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-212">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="f2ccf-213">Pressione o botão **Iniciar captura** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-213">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="f2ccf-214">Adicione o sufixo de URL **/API/Contact** à URL na barra de endereços e pressione a tecla **Enter** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-214">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="f2ccf-215">Pressione o botão **ir para exibição detalhada** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-215">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="f2ccf-216">Selecione a guia **corpo da resposta** . Você deve ver uma cadeia de caracteres JSON que representa a forma serializada de uma matriz de instâncias de contato.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-216">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="f2ccf-217">![Saída serializada JSON de uma chamada de método de API Web complexa](build-restful-apis-with-aspnet-web-api/_static/image13.png "Saída serializada JSON de uma chamada de método de API Web complexa")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-217">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      <span data-ttu-id="f2ccf-218">*Saída serializada JSON de uma chamada de método de API Web complexa*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-218">*JSON serialized output of a complex Web API method call*</span></span>

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="f2ccf-219">Tarefa 4-extraindo funcionalidade em uma camada de serviço</span><span class="sxs-lookup"><span data-stu-id="f2ccf-219">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="f2ccf-220">Essa tarefa demonstrará como extrair a funcionalidade em uma camada de serviço para facilitar para os desenvolvedores a separação da funcionalidade de serviço da camada do controlador, permitindo, assim, a reutilização dos serviços que realmente fazem o trabalho.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-220">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="f2ccf-221">Crie uma nova pasta na raiz da solução e nomeie-a como **Serviços**de ti.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-221">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="f2ccf-222">Para fazer isso, clique **com** o botão direito do mouse em projectmanager projeto, selecione **Adicionar** | **nova pasta**, nomeie os *Serviços*de ti.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-222">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="f2ccf-223">![Criando pasta de serviços](build-restful-apis-with-aspnet-web-api/_static/image14.png "Criando pasta de serviços")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-223">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    <span data-ttu-id="f2ccf-224">*Criando pasta de serviços*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-224">*Creating Services folder*</span></span>
2. <span data-ttu-id="f2ccf-225">Clique com o botão direito do mouse na pasta **Serviços** e selecione **Adicionar | Classe...** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-225">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="f2ccf-226">![Adicionando uma nova classe à pasta serviços](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adicionando uma nova classe à pasta serviços")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-226">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    <span data-ttu-id="f2ccf-227">*Adicionando uma nova classe à pasta serviços*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-227">*Adding a new class to the Services folder*</span></span>
3. <span data-ttu-id="f2ccf-228">Quando a caixa de diálogo **Adicionar novo item** for exibida, nomeie a nova classe **ContactRepository** e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-228">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="f2ccf-229">![Criando um arquivo de classe para conter o código para a camada de serviço do repositório de contatos](build-restful-apis-with-aspnet-web-api/_static/image16.png "Criando um arquivo de classe para conter o código para a camada de serviço do repositório de contatos")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-229">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    <span data-ttu-id="f2ccf-230">*Criando um arquivo de classe para conter o código para a camada de serviço do repositório de contatos*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-230">*Creating a class file to contain the code for the Contact Repository service layer*</span></span>
4. <span data-ttu-id="f2ccf-231">Adicione uma diretiva using ao arquivo **ContactRepository.cs** para incluir o namespace de modelos.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-231">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="f2ccf-232">Adicione o seguinte código realçado ao arquivo **ContactRepository.cs** para implementar o método GetAllContacts.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-232">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="f2ccf-233">(Trecho de código- *laboratório da API Web-Ex01-repositório de contatos*)</span><span class="sxs-lookup"><span data-stu-id="f2ccf-233">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="f2ccf-234">Abra o arquivo **ContactController.cs** se ele ainda não estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-234">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="f2ccf-235">Adicione a seguinte instrução using à seção de declaração de namespace do arquivo.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-235">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="f2ccf-236">Adicione o seguinte código realçado à classe **ContactController.cs** para adicionar um campo particular para representar a instância do repositório, para que o restante dos membros da classe possa fazer uso da implementação do serviço.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-236">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="f2ccf-237">(Trecho de código- *laboratório da API Web-Ex01-controlador de contato*)</span><span class="sxs-lookup"><span data-stu-id="f2ccf-237">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="f2ccf-238">Altere o método **Get** para que ele faça uso do serviço de repositório de contatos.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-238">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="f2ccf-239">(Trecho de código- *laboratório da API Web-Ex01-retornando uma lista de contatos por meio do repositório*)</span><span class="sxs-lookup"><span data-stu-id="f2ccf-239">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="f2ccf-240">Coloque um ponto de interrupção na definição do método **Get** de **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-240">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="f2ccf-241">![Adicionando pontos de interrupção ao controlador de contato](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adicionando pontos de interrupção ao controlador de contato")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-241">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   <span data-ttu-id="f2ccf-242">*Adicionando pontos de interrupção ao controlador de contato*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-242">*Adding breakpoints to the contact controller*</span></span>
11. <span data-ttu-id="f2ccf-243">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-243">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="f2ccf-244">Quando o navegador for aberto, pressione **F12** para abrir as ferramentas de desenvolvedor.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-244">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="f2ccf-245">Clique na guia **rede** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-245">Click the **Network** tab.</span></span>
14. <span data-ttu-id="f2ccf-246">Clique no botão **Iniciar captura** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-246">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="f2ccf-247">Acrescente a URL na barra de endereços com o sufixo **/API/Contact** e pressione **Enter** para carregar o controlador de API.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-247">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="f2ccf-248">O Visual Studio 2012 deve interromper uma vez que o método **Get** comece a execução.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-248">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="f2ccf-249">![Quebrando dentro do método Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "Quebrando dentro do método Get")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-249">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   <span data-ttu-id="f2ccf-250">*Quebrando dentro do método Get*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-250">*Breaking within the Get method*</span></span>
17. <span data-ttu-id="f2ccf-251">Pressione **F5** para continuar.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-251">Press **F5** to continue.</span></span>
18. <span data-ttu-id="f2ccf-252">Volte para o Internet Explorer se ele ainda não estiver em foco.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-252">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="f2ccf-253">Observe a janela de captura de rede.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-253">Note the network capture window.</span></span>

    <span data-ttu-id="f2ccf-254">![Exibição de rede no Internet Explorer mostrando os resultados da chamada à API Web](build-restful-apis-with-aspnet-web-api/_static/image19.png "Exibição de rede no Internet Explorer mostrando os resultados da chamada à API Web")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-254">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    <span data-ttu-id="f2ccf-255">*Exibição de rede no Internet Explorer mostrando os resultados da chamada à API Web*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-255">*Network view in Internet Explorer showing results of the Web API call*</span></span>
19. <span data-ttu-id="f2ccf-256">Clique no botão **ir para exibição detalhada** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-256">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="f2ccf-257">Clique na guia **corpo da resposta** . Observe a saída JSON da chamada à API e como ela representa os dois contatos recuperados pela camada de serviço.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-257">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="f2ccf-258">![Exibindo a saída JSON da API Web na janela ferramentas de desenvolvedor](build-restful-apis-with-aspnet-web-api/_static/image20.png "Exibindo a saída JSON da API Web na janela ferramentas de desenvolvedor")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-258">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    <span data-ttu-id="f2ccf-259">*Exibindo a saída JSON da API Web na janela ferramentas de desenvolvedor*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-259">*Viewing the JSON output from the Web API in the developer tools window*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="f2ccf-260">Exercício 2: criar uma API da Web de leitura/gravação</span><span class="sxs-lookup"><span data-stu-id="f2ccf-260">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="f2ccf-261">Neste exercício, você implementará os métodos POST e PUT para o Gerenciador de contatos para habilitá-lo com recursos de edição de dados.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-261">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="f2ccf-262">Tarefa 1-abrindo o projeto de API Web</span><span class="sxs-lookup"><span data-stu-id="f2ccf-262">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="f2ccf-263">Nesta tarefa, você irá se preparar para aprimorar o projeto de API Web criado no exercício 1 para que ele possa aceitar a entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-263">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="f2ccf-264">Execute o **Visual Studio 2012 Express para Web**, para fazer isso, vá para **iniciar** e digite **vs Express para Web** pressione **Enter**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-264">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="f2ccf-265">Abra a solução **inicial** localizada na **origem/Ex02-ReadWriteWebAPI/início/** pasta.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-265">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="f2ccf-266">Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-266">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="f2ccf-267">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-267">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f2ccf-268">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-268">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f2ccf-269">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-269">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f2ccf-270">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-270">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f2ccf-271">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-271">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f2ccf-272">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-272">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f2ccf-273">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-273">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="f2ccf-274">Abra o arquivo **Services/ContactRepository. cs** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-274">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="f2ccf-275">Tarefa 2-adicionando recursos de persistência de dados à implementação do repositório de contatos</span><span class="sxs-lookup"><span data-stu-id="f2ccf-275">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="f2ccf-276">Nesta tarefa, você aumentará a classe ContactRepository do projeto de API Web criado no exercício 1 para que ele possa persistir e aceitar as novas instâncias de contato e entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-276">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="f2ccf-277">Adicione a seguinte constante à classe **ContactRepository** para representar o nome do nome da chave do item de cache do servidor Web posteriormente neste exercício.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-277">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="f2ccf-278">Adicione um construtor ao **ContactRepository** que contém o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-278">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="f2ccf-279">(Trecho de código- *Ex02-Lab de API Web-Construtor de repositório de contatos*)</span><span class="sxs-lookup"><span data-stu-id="f2ccf-279">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="f2ccf-280">Modifique o código para o método **GetAllContacts** , conforme demonstrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-280">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="f2ccf-281">(Trecho de código- *laboratório da API Web-Ex02-obter todos os contatos*)</span><span class="sxs-lookup"><span data-stu-id="f2ccf-281">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="f2ccf-282">Este exemplo é para fins de demonstração e usará o cache do servidor Web como meio de armazenamento, de modo que os valores estarão disponíveis para vários clientes simultaneamente, em vez de usar um mecanismo de armazenamento de sessão ou um tempo de vida de armazenamento de solicitação.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-282">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="f2ccf-283">É possível usar Entity Framework, o armazenamento XML ou qualquer outra variedade no lugar do cache do servidor Web.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-283">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="f2ccf-284">Implemente um novo método chamado **SaveContact** para a classe **ContactRepository** para fazer o trabalho de salvar um contato.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-284">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="f2ccf-285">O método **SaveContact** deve usar um único parâmetro de **contato** e retornar um valor booliano indicando êxito ou falha.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-285">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="f2ccf-286">(Trecho de código- *laboratório da API Web-Ex02-implementando o método SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="f2ccf-286">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="f2ccf-287">Exercício 3: consumir a API Web de um cliente HTML</span><span class="sxs-lookup"><span data-stu-id="f2ccf-287">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="f2ccf-288">Neste exercício, você criará um cliente HTML para chamar a API da Web.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-288">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="f2ccf-289">Esse cliente facilitará a troca de dados com a API Web usando JavaScript e exibirá os resultados em um navegador da Web usando marcação HTML.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-289">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="f2ccf-290">Tarefa 1-modificando a exibição de índice para fornecer uma GUI para exibir contatos</span><span class="sxs-lookup"><span data-stu-id="f2ccf-290">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="f2ccf-291">Nesta tarefa, você modificará a exibição de índice padrão do aplicativo Web para dar suporte ao requisito de exibir a lista de contatos existentes em um navegador HTML.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-291">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="f2ccf-292">Abra o **Visual Studio 2012 Express para Web** se ele ainda não estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-292">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="f2ccf-293">Abra a solução **inicial** localizada na **origem/Ex03-ConsumingWebAPI/início/** pasta.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-293">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="f2ccf-294">Caso contrário, você pode continuar usando a solução **final** obtida concluindo o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-294">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="f2ccf-295">Se você tiver aberto a solução **inicial** fornecida, será necessário baixar alguns pacotes NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-295">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="f2ccf-296">Para fazer isso, clique no menu **projeto** e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-296">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="f2ccf-297">Na caixa de diálogo **gerenciar pacotes NuGet** , clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-297">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="f2ccf-298">Por fim, Compile a solução clicando em **build** | **Compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-298">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="f2ccf-299">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-299">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="f2ccf-300">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez em que executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-300">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="f2ccf-301">É por isso que você precisará executar essas etapas depois de abrir uma solução existente deste laboratório.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-301">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="f2ccf-302">Abra o arquivo **index. cshtml** localizado em **exibições/pasta base** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-302">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="f2ccf-303">Substitua o código HTML dentro do elemento div por um **corpo** de ID para que ele se pareça com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-303">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="f2ccf-304">Adicione o seguinte código JavaScript na parte inferior do arquivo para executar a solicitação HTTP para a API da Web.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-304">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="f2ccf-305">Abra o arquivo **ContactController.cs** se ele ainda não estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-305">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="f2ccf-306">Coloque um ponto de interrupção no método **Get** da classe **ContactController** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-306">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="f2ccf-307">![Colocando um ponto de interrupção no método Get do controlador de API](build-restful-apis-with-aspnet-web-api/_static/image21.png "Colocando um ponto de interrupção no método Get do controlador de API")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-307">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    <span data-ttu-id="f2ccf-308">*Colocando um ponto de interrupção no método Get do controlador de API*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-308">*Placing a breakpoint on the Get method of the API controller*</span></span>
8. <span data-ttu-id="f2ccf-309">Pressione **F5** para executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-309">Press **F5** to run the project.</span></span> <span data-ttu-id="f2ccf-310">O navegador carregará o documento HTML.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-310">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f2ccf-311">Certifique-se de que você está navegando para a URL raiz do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-311">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="f2ccf-312">Depois que a página for carregada e o JavaScript for executado, o ponto de interrupção será atingido e a execução do código será pausada no controlador.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-312">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="f2ccf-313">![Depuração nas chamadas à API Web usando VS Express para Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Depuração nas chamadas à API Web usando VS Express para Web")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-313">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    <span data-ttu-id="f2ccf-314">*Depuração na chamada à API Web usando o Visual Studio 2012 Express para Web*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-314">*Debugging into the Web API call using Visual Studio 2012 Express for Web*</span></span>
10. <span data-ttu-id="f2ccf-315">Remova o ponto de interrupção e pressione **F5** ou o botão **continuar** da barra de ferramentas de depuração para continuar carregando o modo de exibição no navegador.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-315">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="f2ccf-316">Depois que a chamada da API Web for concluída, você deverá ver os contatos retornados da chamada da API Web exibida como itens de lista no navegador.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-316">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="f2ccf-317">![Resultados da chamada à API exibida no navegador como itens de lista](build-restful-apis-with-aspnet-web-api/_static/image23.png "Resultados da chamada à API exibida no navegador como itens de lista")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-317">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    <span data-ttu-id="f2ccf-318">*Resultados da chamada à API exibida no navegador como itens de lista*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-318">*Results of the API call displayed in the browser as list items*</span></span>
11. <span data-ttu-id="f2ccf-319">{2&gt;Pare a depuração.&lt;2}</span><span class="sxs-lookup"><span data-stu-id="f2ccf-319">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="f2ccf-320">Tarefa 2-modificando a exibição de índice para fornecer uma GUI para criar contatos</span><span class="sxs-lookup"><span data-stu-id="f2ccf-320">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="f2ccf-321">Nesta tarefa, você continuará a modificar a exibição de índice do aplicativo MVC.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-321">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="f2ccf-322">Um formulário será adicionado à página HTML que capturará a entrada do usuário e o enviará para a API da Web para criar um novo contato, e um novo método do controlador da API Web será criado para coletar a data da GUI.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-322">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="f2ccf-323">Abra o arquivo **ContactController.cs** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-323">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="f2ccf-324">Adicione um novo método à classe de controlador chamada **post** , conforme mostrado no código a seguir.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-324">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="f2ccf-325">(Trecho de código- *laboratório da API Web-método Ex03-post*)</span><span class="sxs-lookup"><span data-stu-id="f2ccf-325">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="f2ccf-326">Abra o arquivo **index. cshtml** no Visual Studio se ele ainda não estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-326">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="f2ccf-327">Adicione o código HTML abaixo ao arquivo logo após a lista não ordenada que você adicionou na tarefa anterior.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-327">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="f2ccf-328">No elemento script na parte inferior do documento, adicione o seguinte código realçado para manipular eventos de clique de botão, que irão postar os dados para a API Web usando uma chamada HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-328">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="f2ccf-329">Em **ContactController.cs**, coloque um ponto de interrupção no método **post** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-329">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="f2ccf-330">Pressione **F5** para executar o aplicativo no navegador.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-330">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="f2ccf-331">Depois que a página for carregada no navegador, digite um novo nome e ID de contato e clique no botão **salvar** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-331">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="f2ccf-332">![O documento HTML do cliente carregado no navegador](build-restful-apis-with-aspnet-web-api/_static/image24.png "O documento HTML do cliente carregado no navegador")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-332">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    <span data-ttu-id="f2ccf-333">*O documento HTML do cliente carregado no navegador*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-333">*The client HTML document loaded in the browser*</span></span>
9. <span data-ttu-id="f2ccf-334">Quando a janela do depurador for interrompida no método **post** , dê uma olhada nas propriedades do parâmetro **Contact** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-334">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="f2ccf-335">Os valores devem corresponder aos dados inseridos no formulário.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-335">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="f2ccf-336">![O objeto de contato que está sendo enviado para a API Web do cliente](build-restful-apis-with-aspnet-web-api/_static/image25.png "O objeto de contato que está sendo enviado para a API Web do cliente")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-336">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    <span data-ttu-id="f2ccf-337">*O objeto de contato que está sendo enviado para a API Web do cliente*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-337">*The Contact object being sent to the Web API from the client*</span></span>
10. <span data-ttu-id="f2ccf-338">Percorra o método no depurador até que a variável de **resposta** tenha sido criada.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-338">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="f2ccf-339">Após a inspeção na janela **locais** no depurador, você verá que todas as propriedades foram definidas.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-339">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="f2ccf-340">![A resposta após a criação no depurador](build-restful-apis-with-aspnet-web-api/_static/image26.png "A resposta após a criação no depurador")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-340">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   <span data-ttu-id="f2ccf-341">*A resposta após a criação no depurador*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-341">*The response following creation in the debugger*</span></span>
11. <span data-ttu-id="f2ccf-342">Se você pressionar **F5** ou clicar em **continuar** no depurador, a solicitação será concluída.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-342">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="f2ccf-343">Depois de voltar para o navegador, o novo contato foi adicionado à lista de contatos armazenados pela implementação do **ContactRepository** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-343">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="f2ccf-344">![O navegador reflete a criação bem-sucedida da nova instância de contato](build-restful-apis-with-aspnet-web-api/_static/image27.png "O navegador reflete a criação bem-sucedida da nova instância de contato")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-344">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   <span data-ttu-id="f2ccf-345">*O navegador reflete a criação bem-sucedida da nova instância de contato*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-345">*The browser reflects successful creation of the new contact instance*</span></span>

> [!NOTE]
> <span data-ttu-id="f2ccf-346">Além disso, você pode implantar esse aplicativo no Azure após o [Apêndice C: publicando um aplicativo ASP.NET MVC 4 usando implantação da Web](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="f2ccf-346">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="f2ccf-347">Resumo</span><span class="sxs-lookup"><span data-stu-id="f2ccf-347">Summary</span></span>

<span data-ttu-id="f2ccf-348">Este laboratório apresentou a você o novo ASP.NET Web API Framework e a implementação de APIs Web RESTful usando a estrutura.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-348">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="f2ccf-349">A partir daqui, você pode criar um novo repositório que facilite a persistência de dados usando qualquer número de mecanismos e conecte esse serviço em vez do simples fornecido como um exemplo neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-349">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="f2ccf-350">A API Web dá suporte a vários recursos adicionais, como a habilitação da comunicação de clientes não HTML escritos em qualquer linguagem compatível com HTTP e JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-350">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="f2ccf-351">A capacidade de hospedar uma API da Web fora de um aplicativo Web típico também é possível, bem como a capacidade de criar seus próprios formatos de serialização.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-351">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="f2ccf-352">O site da ASP.NET tem uma área dedicada à estrutura de ASP.NET Web API em [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="f2ccf-352">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="f2ccf-353">Este site continuará a fornecer informações mais recentes, amostras e notícias relacionadas à API da Web, portanto, verifique com frequência se você gostaria de se aprofundar na arte de criar APIs Web personalizadas disponíveis para praticamente qualquer estrutura de dispositivo ou de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-353">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="f2ccf-354">Apêndice A: usando trechos de código</span><span class="sxs-lookup"><span data-stu-id="f2ccf-354">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="f2ccf-355">Com trechos de código, você tem todo o código de que precisa ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-355">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="f2ccf-356">O documento de laboratório informará exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-356">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="f2ccf-357">![Usando trechos de código do Visual Studio para inserir código em seu projeto](build-restful-apis-with-aspnet-web-api/_static/image28.png "Usando trechos de código do Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-357">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="f2ccf-358">*Usando trechos de código do Visual Studio para inserir código em seu projeto*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-358">*Using Visual Studio code snippets to insert code into your project*</span></span>

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="f2ccf-359">Para adicionar um trecho de código usando o tecladoC# (somente)</span><span class="sxs-lookup"><span data-stu-id="f2ccf-359">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="f2ccf-360">Coloque o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-360">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="f2ccf-361">Comece digitando o nome do trecho de código (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="f2ccf-361">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="f2ccf-362">Observe como o IntelliSense exibe nomes de trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-362">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="f2ccf-363">Selecione o trecho correto (ou continue digitando até que o nome do trecho inteiro seja selecionado).</span><span class="sxs-lookup"><span data-stu-id="f2ccf-363">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="f2ccf-364">Pressione a tecla TAB duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-364">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="f2ccf-365">![Comece a digitar o nome do trecho](build-restful-apis-with-aspnet-web-api/_static/image29.png "Comece a digitar o nome do trecho")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-365">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    <span data-ttu-id="f2ccf-366">*Comece a digitar o nome do trecho*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-366">*Start typing the snippet name*</span></span>

    <span data-ttu-id="f2ccf-367">![Pressione Tab para selecionar o trecho realçado](build-restful-apis-with-aspnet-web-api/_static/image30.png "Pressione Tab para selecionar o trecho realçado")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-367">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    <span data-ttu-id="f2ccf-368">*Pressione Tab para selecionar o trecho realçado*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-368">*Press Tab to select the highlighted snippet*</span></span>

    <span data-ttu-id="f2ccf-369">![Pressione Tab novamente e o trecho será expandido](build-restful-apis-with-aspnet-web-api/_static/image31.png "Pressione Tab novamente e o trecho será expandido")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-369">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    <span data-ttu-id="f2ccf-370">*Pressione Tab novamente e o trecho será expandido*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-370">*Press Tab again and the snippet will expand*</span></span>

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="f2ccf-371">Para adicionar um trecho de código usando o mouseC#(, Visual Basic e XML)</span><span class="sxs-lookup"><span data-stu-id="f2ccf-371">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="f2ccf-372">Clique com o botão direito do mouse no local em que você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-372">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="f2ccf-373">Selecione **Inserir trecho** seguido por **meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-373">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="f2ccf-374">Selecione o trecho relevante na lista clicando nele.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-374">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="f2ccf-375">![Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho](build-restful-apis-with-aspnet-web-api/_static/image32.png "Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-375">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    <span data-ttu-id="f2ccf-376">*Clique com o botão direito do mouse em onde você deseja inserir o trecho de código e selecione Inserir trecho*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-376">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

    <span data-ttu-id="f2ccf-377">![Selecione o trecho relevante na lista clicando nele](build-restful-apis-with-aspnet-web-api/_static/image33.png "Selecione o trecho relevante na lista clicando nele")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-377">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    <span data-ttu-id="f2ccf-378">*Selecione o trecho relevante na lista clicando nele*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-378">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="f2ccf-379">Apêndice B: Instalando o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="f2ccf-379">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="f2ccf-380">Você pode instalar o **Microsoft Visual Studio Express 2012 para Web** ou outra versão &quot;Express&quot; usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-380">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="f2ccf-381">As instruções a seguir orientam você pelas etapas necessárias para instalar o *Visual Studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-381">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="f2ccf-382">Vá para [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="f2ccf-382">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="f2ccf-383">Como alternativa, se você já tiver instalado o Web Platform Installer, poderá abri-lo e pesquisar o produto &quot;<em>Visual Studio Express 2012 para Web com o SDK do Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-383">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="f2ccf-384">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-384">Click on **Install Now**.</span></span> <span data-ttu-id="f2ccf-385">Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-385">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="f2ccf-386">Quando **Web Platform Installer** estiver aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-386">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="f2ccf-387">![Instalar Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Instalar Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-387">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="f2ccf-388">*Instalar Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-388">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="f2ccf-389">Leia todos os termos e licenças de produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-389">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitando os termos de licença](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    <span data-ttu-id="f2ccf-391">*Aceitando os termos de licença*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-391">*Accepting the license terms*</span></span>
5. <span data-ttu-id="f2ccf-392">Aguarde até que o processo de download e instalação seja concluído.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-392">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    <span data-ttu-id="f2ccf-394">*Progresso da instalação*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-394">*Installation progress*</span></span>
6. <span data-ttu-id="f2ccf-395">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-395">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    <span data-ttu-id="f2ccf-397">*Instalação concluída*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-397">*Installation completed*</span></span>
7. <span data-ttu-id="f2ccf-398">Clique em **sair** para fechar Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-398">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="f2ccf-399">Para abrir o Visual Studio Express para Web, vá para a tela **Iniciar** e comece a escrever &quot;**vs Express**&quot;e, em seguida, clique no bloco **vs Express para Web** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-399">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Bloco VS Express para Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    <span data-ttu-id="f2ccf-401">*Bloco VS Express para Web*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-401">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="f2ccf-402">Apêndice C: publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web</span><span class="sxs-lookup"><span data-stu-id="f2ccf-402">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="f2ccf-403">Este apêndice mostrará como criar um novo site no portal do Azure e publicar o aplicativo obtido seguindo o laboratório, aproveitando o recurso de publicação de Implantação da Web fornecido pelo Azure.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-403">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="f2ccf-404">Tarefa 1-Criando um novo site no portal do Azure</span><span class="sxs-lookup"><span data-stu-id="f2ccf-404">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="f2ccf-405">Vá para o [portal de gerenciamento do Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-405">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f2ccf-406">Com o Azure, você pode hospedar 10 sites da Web de ASP.NET gratuitamente e, em seguida, dimensionar conforme o tráfego cresce.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-406">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="f2ccf-407">Você pode se inscrever [aqui](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="f2ccf-407">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="f2ccf-408">![Fazer logon no Windows portal do Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "Fazer logon no Windows portal do Azure")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-408">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="f2ccf-409">*Fazer logon no portal*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-409">*Log on to Portal*</span></span>
2. <span data-ttu-id="f2ccf-410">Clique em **novo** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-410">Click **New** on the command bar.</span></span>

    <span data-ttu-id="f2ccf-411">![Criando um novo site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Criando um novo site")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-411">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="f2ccf-412">*Criando um novo site*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-412">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="f2ccf-413">Clique em **computação** | **site**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-413">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="f2ccf-414">Em seguida, selecione a opção **criação rápida** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-414">Then select **Quick Create** option.</span></span> <span data-ttu-id="f2ccf-415">Forneça uma URL disponível para o novo site e clique em **criar site**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-415">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f2ccf-416">O Azure é o host para um aplicativo Web em execução na nuvem que você pode controlar e gerenciar.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-416">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="f2ccf-417">A opção criação rápida permite que você implante um aplicativo Web completo no Azure de fora do Portal.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-417">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="f2ccf-418">Ele não inclui etapas para configurar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-418">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="f2ccf-419">![Criando um novo site usando a criação rápida](build-restful-apis-with-aspnet-web-api/_static/image41.png "Criando um novo site usando a criação rápida")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-419">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="f2ccf-420">*Criando um novo site usando a criação rápida*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-420">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="f2ccf-421">Aguarde até que o novo **site** seja criado.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-421">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="f2ccf-422">Depois que o site for criado, clique no link sob a coluna **URL** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-422">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="f2ccf-423">Verifique se o novo site está funcionando.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-423">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="f2ccf-424">![Navegando até o novo site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Navegando até o novo site")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-424">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="f2ccf-425">*Navegando até o novo site*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-425">*Browsing to the new web site*</span></span>

    <span data-ttu-id="f2ccf-426">![Site em execução](build-restful-apis-with-aspnet-web-api/_static/image43.png "Site em execução")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-426">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    <span data-ttu-id="f2ccf-427">*Site em execução*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-427">*Web site running*</span></span>
6. <span data-ttu-id="f2ccf-428">Volte para o portal e clique no nome do site na coluna **nome** para exibir as páginas de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-428">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="f2ccf-429">![Abrindo as páginas de gerenciamento de site](build-restful-apis-with-aspnet-web-api/_static/image44.png "Abrindo as páginas de gerenciamento de site")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-429">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="f2ccf-430">*Abrindo as páginas de gerenciamento de site*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-430">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="f2ccf-431">Na página **painel** , na seção **visão rápida** , clique no link **baixar perfil de publicação** .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-431">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f2ccf-432">O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo Web em um Azure para cada método de publicação habilitado.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-432">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="f2ccf-433">O perfil de publicação contém as URLs, as credenciais de usuário e as cadeias de conexão de banco de dados necessárias para conectar-se e autenticar cada um dos pontos de extremidade para os quais um método de publicação é habilitado.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-433">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="f2ccf-434">**O Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** e **Microsoft Visual Studio 2012** dão suporte à leitura de perfis de publicação para automatizar a configuração desses programas para a publicação de aplicativos Web no Azure.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-434">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="f2ccf-435">![Baixando o perfil de publicação do site](build-restful-apis-with-aspnet-web-api/_static/image45.png "Baixando o perfil de publicação do site")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-435">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="f2ccf-436">*Baixando o perfil de publicação do site*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-436">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="f2ccf-437">Baixe o arquivo de perfil de publicação em um local conhecido.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-437">Download the publish profile file to a known location.</span></span> <span data-ttu-id="f2ccf-438">Neste exercício, você verá como usar esse arquivo para publicar um aplicativo Web no Azure por meio do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-438">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="f2ccf-439">![Salvando o arquivo de perfil de publicação](build-restful-apis-with-aspnet-web-api/_static/image46.png "Salvando o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-439">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    <span data-ttu-id="f2ccf-440">*Salvando o arquivo de perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-440">*Saving the publish profile file*</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="f2ccf-441">Tarefa 2-Configurando o servidor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="f2ccf-441">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="f2ccf-442">Se seu aplicativo utiliza bancos de dados SQL Server, você precisará criar um servidor de banco de dados SQL.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-442">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="f2ccf-443">Se você quiser implantar um aplicativo simples que não usa SQL Server você pode ignorar essa tarefa.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-443">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="f2ccf-444">Você precisará de um servidor do banco de dados SQL para armazenar o banco de dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-444">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="f2ccf-445">Você pode exibir os servidores do banco de dados SQL de sua assinatura no portal de gerenciamento do Azure em bancos de dados **sql** | **servidores** | **painel do servidor**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-445">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="f2ccf-446">Se você não tiver um servidor criado, poderá criar um usando o botão **Adicionar** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-446">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="f2ccf-447">Anote o nome do **servidor e a URL, o nome de logon e a senha do administrador**, pois você irá usá-los nas próximas tarefas.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-447">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="f2ccf-448">Não crie o banco de dados ainda, pois ele será criado em um estágio posterior.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-448">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="f2ccf-449">![Painel do servidor do banco de dados SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "Painel do servidor do banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-449">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="f2ccf-450">*Painel do servidor do banco de dados SQL*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-450">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="f2ccf-451">Na próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, será necessário incluir o endereço IP local na lista de **endereços IP permitidos**do servidor.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-451">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="f2ccf-452">Para fazer isso, clique em **Configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o nas caixas de texto endereço IP **inicial** e **endereço IP final** e clique no botão ![adicionar-cliente-IP-endereço-OK-botão](build-restful-apis-with-aspnet-web-api/_static/image48.png).</span><span class="sxs-lookup"><span data-stu-id="f2ccf-452">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Adicionando endereço IP do cliente](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    <span data-ttu-id="f2ccf-454">*Adicionando endereço IP do cliente*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-454">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="f2ccf-455">Depois que o **endereço IP do cliente** for adicionado à lista endereços IP permitidos, clique em **salvar** para confirmar as alterações.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-455">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar alterações](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    <span data-ttu-id="f2ccf-457">*Confirmar alterações*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-457">*Confirm Changes*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="f2ccf-458">Tarefa 3-publicando um aplicativo ASP.NET MVC 4 usando Implantação da Web</span><span class="sxs-lookup"><span data-stu-id="f2ccf-458">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="f2ccf-459">Volte para a solução ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-459">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="f2ccf-460">Na **Gerenciador de soluções**, clique com o botão direito do mouse no projeto de site e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-460">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="f2ccf-461">![Publicando o aplicativo](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publicando o aplicativo")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-461">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    <span data-ttu-id="f2ccf-462">*Publicando o site*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-462">*Publishing the web site*</span></span>
2. <span data-ttu-id="f2ccf-463">Importe o perfil de publicação salvo na primeira tarefa.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-463">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="f2ccf-464">![Importando o perfil de publicação](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importando o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-464">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    <span data-ttu-id="f2ccf-465">*Importando perfil de publicação*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-465">*Importing publish profile*</span></span>
3. <span data-ttu-id="f2ccf-466">Clique em **validar conexão**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-466">Click **Validate Connection**.</span></span> <span data-ttu-id="f2ccf-467">Depois que a validação for concluída, clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-467">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f2ccf-468">A validação será concluída quando você vir uma marca de seleção verde ao lado do botão Validar conexão.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-468">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="f2ccf-469">![Validando conexão](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validando conexão")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-469">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    <span data-ttu-id="f2ccf-470">*Validando conexão*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-470">*Validating connection*</span></span>
4. <span data-ttu-id="f2ccf-471">Na página **configurações** , na seção **bancos** de dados, clique no botão ao lado da caixa de texto da sua conexão de banco (ou seja, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="f2ccf-471">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="f2ccf-472">![Configuração de implantação da Web](build-restful-apis-with-aspnet-web-api/_static/image54.png "Configuração de implantação da Web")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-472">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    <span data-ttu-id="f2ccf-473">*Configuração de implantação da Web*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-473">*Web deploy configuration*</span></span>
5. <span data-ttu-id="f2ccf-474">Configure a conexão de banco de dados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="f2ccf-474">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="f2ccf-475">No **nome do servidor** , digite a URL do servidor do banco de dados SQL usando o prefixo *TCP:* .</span><span class="sxs-lookup"><span data-stu-id="f2ccf-475">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="f2ccf-476">Em **nome de usuário** , digite o nome de logon do administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-476">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="f2ccf-477">Em **senha** , digite a senha de logon do administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-477">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="f2ccf-478">Digite um novo nome de banco de dados, por exemplo: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-478">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="f2ccf-479">![Configurando a cadeia de conexão de destino](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configurando a cadeia de conexão de destino")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-479">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="f2ccf-480">*Configurando a cadeia de conexão de destino*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-480">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="f2ccf-481">Em seguida, clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-481">Then click **OK**.</span></span> <span data-ttu-id="f2ccf-482">Quando for solicitado a criar o banco de dados, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-482">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="f2ccf-483">![Criando o banco de dados](build-restful-apis-with-aspnet-web-api/_static/image56.png "Criando a cadeia de caracteres do banco de dados")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-483">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    <span data-ttu-id="f2ccf-484">*Criando o banco de dados*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-484">*Creating the database*</span></span>
7. <span data-ttu-id="f2ccf-485">A cadeia de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto conexão padrão.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-485">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="f2ccf-486">Em seguida, clique em **Próximo**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-486">Then click **Next**.</span></span>

    <span data-ttu-id="f2ccf-487">![Cadeia de conexão apontando para o banco de dados SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "Cadeia de conexão apontando para o banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-487">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="f2ccf-488">*Cadeia de conexão apontando para o banco de dados SQL*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-488">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="f2ccf-489">Na página **Visualização** , clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-489">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="f2ccf-490">![Publicando o aplicativo Web](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publicando o aplicativo Web")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-490">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    <span data-ttu-id="f2ccf-491">*Publicando o aplicativo Web*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-491">*Publishing the web application*</span></span>
9. <span data-ttu-id="f2ccf-492">Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.</span><span class="sxs-lookup"><span data-stu-id="f2ccf-492">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="f2ccf-493">![Aplicativo publicado no Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Aplicativo publicado no Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="f2ccf-493">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="f2ccf-494">*Aplicativo publicado no Azure*</span><span class="sxs-lookup"><span data-stu-id="f2ccf-494">*Application published to Azure*</span></span>
