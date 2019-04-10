---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Criar APIs RESTful com a API Web ASP.NET - ASP.NET 4.x
author: rick-anderson
description: 'Laboratório prático: Usar a API da Web no ASP.NET 4.x para compilar uma API REST simples para um aplicativo do Gerenciador de contatos.'
ms.author: riande
ms.date: 02/18/2013
ms.custom: seoapril2019
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 3ba7f2d186e6f0837a32f69f964cec19fe625953
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59391475"
---
# <a name="build-restful-apis-with-aspnet-web-api"></a><span data-ttu-id="c4be8-103">Criar APIs RESTful com a API da Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c4be8-103">Build RESTful APIs with ASP.NET Web API</span></span>

<span data-ttu-id="c4be8-104">por [Web Camps equipe](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="c4be8-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="c4be8-105">Laboratório prático: Usar a API da Web no ASP.NET 4.x para compilar uma API REST simples para um aplicativo do Gerenciador de contatos.</span><span class="sxs-lookup"><span data-stu-id="c4be8-105">Hands on lab: Use Web API in ASP.NET 4.x to build a simple REST API for a contact manager application.</span></span> <span data-ttu-id="c4be8-106">Você também criará um cliente para consumir a API.</span><span class="sxs-lookup"><span data-stu-id="c4be8-106">You will also build a client to consume the API.</span></span>

<span data-ttu-id="c4be8-107">Nos últimos anos, tornou-se claro que o HTTP não é apenas para servir as páginas HTML.</span><span class="sxs-lookup"><span data-stu-id="c4be8-107">In recent years, it has become clear that HTTP is not just for serving up HTML pages.</span></span> <span data-ttu-id="c4be8-108">Também é uma plataforma poderosa para a criação de APIs da Web, usando um punhado de verbos (GET, POST e assim por diante) além de alguns conceitos simples, como *URIs* e *cabeçalhos*.</span><span class="sxs-lookup"><span data-stu-id="c4be8-108">It is also a powerful platform for building Web APIs, using a handful of verbs (GET, POST, and so forth) plus a few simple concepts such as *URIs* and *headers*.</span></span> <span data-ttu-id="c4be8-109">API Web ASP.NET é um conjunto de componentes que simplificam a programação HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4be8-109">ASP.NET Web API is a set of components that simplify HTTP programming.</span></span> <span data-ttu-id="c4be8-110">Porque ele se baseia no tempo de execução do ASP.NET MVC, API Web manipula automaticamente os detalhes de baixo nível de transporte de HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4be8-110">Because it is built on top of the ASP.NET MVC runtime, Web API automatically handles the low-level transport details of HTTP.</span></span> <span data-ttu-id="c4be8-111">Ao mesmo tempo, a API da Web naturalmente expõe o modelo de programação HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4be8-111">At the same time, Web API naturally exposes the HTTP programming model.</span></span> <span data-ttu-id="c4be8-112">Na verdade, é uma meta de API da Web *não* abstraem a realidade de HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4be8-112">In fact, one goal of Web API is to *not* abstract away the reality of HTTP.</span></span> <span data-ttu-id="c4be8-113">Como resultado, a API Web é flexível e fácil de estender.</span><span class="sxs-lookup"><span data-stu-id="c4be8-113">As a result, Web API is both flexible and easy to extend.</span></span>  <span data-ttu-id="c4be8-114">O estilo de arquitetura REST provou para ser uma maneira eficiente de aproveitar HTTP – embora certamente isso não seja a abordagem só é válida para HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4be8-114">The REST architectural style has proven to be an effective way to leverage HTTP - although it is certainly not the only valid approach to HTTP.</span></span> <span data-ttu-id="c4be8-115">O Gerenciador de contatos irá expor o RESTful para listagem, adicionando e removendo contatos, entre outros.</span><span class="sxs-lookup"><span data-stu-id="c4be8-115">The contact manager will expose the RESTful for listing, adding and removing contacts, among others.</span></span> 

<span data-ttu-id="c4be8-116">Este laboratório requer um entendimento básico de HTTP, REST e pressupõe que você tenha um conhecimento prático básico de HTML, JavaScript e jQuery.</span><span class="sxs-lookup"><span data-stu-id="c4be8-116">This lab requires a basic understanding of HTTP, REST, and assumes you have a basic working knowledge of HTML, JavaScript, and jQuery.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="c4be8-117">O site da Web do ASP.NET tem uma área dedicada a estrutura de API Web ASP.NET no [ https://asp.net/web-api ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="c4be8-117">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [https://asp.net/web-api](https://asp.net/web-api).</span></span> <span data-ttu-id="c4be8-118">Este site continuará fornecer informações mais recentes, exemplos e notícias relacionadas à API da Web, portanto, verifique-lo com frequência se você gostaria de nos aprofundar na arte de criação de APIs Web personalizadas disponíveis para praticamente qualquer estrutura de desenvolvimento ou dispositivo.</span><span class="sxs-lookup"><span data-stu-id="c4be8-118">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>
> > 
> > <span data-ttu-id="c4be8-119">API Web ASP.NET, semelhante ao ASP.NET MVC 4, tem grande flexibilidade em termos de separar a camada de serviço dos controladores, permitindo que você usar várias das estruturas de injeção de dependência disponíveis razoavelmente fácil.</span><span class="sxs-lookup"><span data-stu-id="c4be8-119">ASP.NET Web API, similar to ASP.NET MVC 4, has great flexibility in terms of separating the service layer from the controllers allowing you to use several of the available Dependency Injection frameworks fairly easy.</span></span> <span data-ttu-id="c4be8-120">Há um bom exemplo no MSDN que mostra como usar Ninject para injeção de dependência em um projeto de API Web ASP.NET que você pode baixá-lo partir [aqui](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span><span class="sxs-lookup"><span data-stu-id="c4be8-120">There is a good sample in MSDN that shows how to use Ninject for dependency injection in an ASP.NET Web API project that you can download it from [here](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).</span></span>
> 
> 
> <span data-ttu-id="c4be8-121">Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponível em [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="c4be8-121">All sample code and snippets are included in the Web Camps Training Kit, available at [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).</span></span>


<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="c4be8-122">Objetivos</span><span class="sxs-lookup"><span data-stu-id="c4be8-122">Objectives</span></span>

<span data-ttu-id="c4be8-123">Neste laboratório prático, você aprenderá como:</span><span class="sxs-lookup"><span data-stu-id="c4be8-123">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="c4be8-124">Implementar uma API Web RESTful</span><span class="sxs-lookup"><span data-stu-id="c4be8-124">Implement a RESTful Web API</span></span>
- <span data-ttu-id="c4be8-125">Chamar a API de um cliente HTML</span><span class="sxs-lookup"><span data-stu-id="c4be8-125">Call the API from an HTML client</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="c4be8-126">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="c4be8-126">Prerequisites</span></span>

<span data-ttu-id="c4be8-127">O exemplo a seguir é necessário para concluir este laboratório prático:</span><span class="sxs-lookup"><span data-stu-id="c4be8-127">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="c4be8-128">[Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [Apêndice B](#AppendixB) para obter instruções sobre como instalá-lo).</span><span class="sxs-lookup"><span data-stu-id="c4be8-128">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="c4be8-129">Configuração</span><span class="sxs-lookup"><span data-stu-id="c4be8-129">Setup</span></span>

**<span data-ttu-id="c4be8-130">Instalando os trechos de código</span><span class="sxs-lookup"><span data-stu-id="c4be8-130">Installing Code Snippets</span></span>**

<span data-ttu-id="c4be8-131">Para sua conveniência, grande parte do código que você estiver gerenciando ao longo deste laboratório está disponível como trechos de código do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c4be8-131">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="c4be8-132">Para instalar os trechos de código executados **.\Source\Setup\CodeSnippets.vsi** arquivo.</span><span class="sxs-lookup"><span data-stu-id="c4be8-132">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="c4be8-133">Se você não estiver familiarizado com os trechos de código do Visual Studio e aprender como usá-los, consulte o Apêndice deste documento &quot; [apêndice a: Usar trechos de código](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="c4be8-133">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="c4be8-134">Exercícios</span><span class="sxs-lookup"><span data-stu-id="c4be8-134">Exercises</span></span>

<span data-ttu-id="c4be8-135">Este laboratório prático inclui o exercício a seguir:</span><span class="sxs-lookup"><span data-stu-id="c4be8-135">This hands-on lab includes the following exercise:</span></span>

1. [<span data-ttu-id="c4be8-136">Exercício 1: Criar uma API de Web somente leitura</span><span class="sxs-lookup"><span data-stu-id="c4be8-136">Exercise 1: Create a Read-Only Web API</span></span>](#Exercise1)
2. [<span data-ttu-id="c4be8-137">Exercício 2: Criar uma API da Web de leitura/gravação</span><span class="sxs-lookup"><span data-stu-id="c4be8-137">Exercise 2: Create a Read/Write Web API</span></span>](#Exercise2)
3. [<span data-ttu-id="c4be8-138">Exercício 3: Consumir a API da Web de um cliente HTML</span><span class="sxs-lookup"><span data-stu-id="c4be8-138">Exercise 3: Consume the Web API from an HTML Client</span></span>](#Exercise3)

> [!NOTE]
> <span data-ttu-id="c4be8-139">Cada exercício é acompanhado por um **final** pasta que contém a solução resultante, você deve obter depois de concluir os exercícios.</span><span class="sxs-lookup"><span data-stu-id="c4be8-139">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="c4be8-140">Você pode usar essa solução como um guia se você precisar trabalhar com os exercícios de ajuda adicional.</span><span class="sxs-lookup"><span data-stu-id="c4be8-140">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="c4be8-141">Tempo estimado para concluir este laboratório: **60 minutos**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-141">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a><span data-ttu-id="c4be8-142">Exercício 1: Criar uma API de Web somente leitura</span><span class="sxs-lookup"><span data-stu-id="c4be8-142">Exercise 1: Create a Read-Only Web API</span></span>

<span data-ttu-id="c4be8-143">Neste exercício, você implementará os métodos GET somente leitura para o Gerenciador de contatos.</span><span class="sxs-lookup"><span data-stu-id="c4be8-143">In this exercise, you will implement the read-only GET methods for the contact manager.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a><span data-ttu-id="c4be8-144">Tarefa 1 - criar o projeto de API</span><span class="sxs-lookup"><span data-stu-id="c4be8-144">Task 1 - Creating the API Project</span></span>

<span data-ttu-id="c4be8-145">Nesta tarefa, você usará os novos modelos de projeto de web do ASP.NET para criar um aplicativo de web API da Web.</span><span class="sxs-lookup"><span data-stu-id="c4be8-145">In this task, you will use the new ASP.NET web project templates to create a Web API web application.</span></span>

1. <span data-ttu-id="c4be8-146">Execute **Visual Studio 2012 Express para Web**, para fazer isso, vá para **inicie** e digite **VS Express para Web** , em seguida, pressione **Enter**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-146">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="c4be8-147">Dos **arquivo** menu, selecione **novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-147">From the **File** menu, select **New Project**.</span></span> <span data-ttu-id="c4be8-148">Selecione o **Visual c# | Web** tipo de exibição de árvore de tipo de projeto do projeto, em seguida, selecione o **aplicativo Web do ASP.NET MVC 4** tipo de projeto.</span><span class="sxs-lookup"><span data-stu-id="c4be8-148">Select the **Visual C# | Web** project type from the project type tree view, then select the **ASP.NET MVC 4 Web Application** project type.</span></span> <span data-ttu-id="c4be8-149">Defina o projeto **nome** para *ContactManager* e o **nome da solução** para *começar*, em seguida, clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-149">Set the project's **Name** to *ContactManager* and the **Solution name** to *Begin*, then click **OK**.</span></span>

    <span data-ttu-id="c4be8-150">![Criando um novo projeto de aplicativo do ASP.NET MVC 4.0 da Web](build-restful-apis-with-aspnet-web-api/_static/image1.png "criando um novo projeto de aplicativo do ASP.NET MVC 4.0 da Web")</span><span class="sxs-lookup"><span data-stu-id="c4be8-150">![Creating a new ASP.NET MVC 4.0 Web Application Project](build-restful-apis-with-aspnet-web-api/_static/image1.png "Creating a new ASP.NET MVC 4.0 Web Application Project")</span></span>

    *<span data-ttu-id="c4be8-151">Criando um novo projeto de aplicativo do ASP.NET MVC 4.0 da Web</span><span class="sxs-lookup"><span data-stu-id="c4be8-151">Creating a new ASP.NET MVC 4.0 Web Application Project</span></span>*
3. <span data-ttu-id="c4be8-152">No diálogo de tipo de projeto do ASP.NET MVC 4, selecione a **API Web** tipo de projeto.</span><span class="sxs-lookup"><span data-stu-id="c4be8-152">In the ASP.NET MVC 4 project type dialog, select the **Web API** project type.</span></span> <span data-ttu-id="c4be8-153">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-153">Click **OK**.</span></span>

    <span data-ttu-id="c4be8-154">![Especifica o tipo de projeto de API da Web](build-restful-apis-with-aspnet-web-api/_static/image2.png "especificando o tipo de projeto de API da Web")</span><span class="sxs-lookup"><span data-stu-id="c4be8-154">![Specifying the Web API project type](build-restful-apis-with-aspnet-web-api/_static/image2.png "Specifying the Web API project type")</span></span>

    *<span data-ttu-id="c4be8-155">Especifica o tipo de projeto de API da Web</span><span class="sxs-lookup"><span data-stu-id="c4be8-155">Specifying the Web API project type</span></span>*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a><span data-ttu-id="c4be8-156">Tarefa 2: criar os controladores de API do Gerenciador de contatos</span><span class="sxs-lookup"><span data-stu-id="c4be8-156">Task 2 - Creating the Contact Manager API Controllers</span></span>

<span data-ttu-id="c4be8-157">Nesta tarefa, você criará as classes do controlador no qual os métodos de API residirá.</span><span class="sxs-lookup"><span data-stu-id="c4be8-157">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="c4be8-158">Exclua o arquivo chamado **ValuesController.cs** dentro **controladores** pasta do projeto.</span><span class="sxs-lookup"><span data-stu-id="c4be8-158">Delete the file named **ValuesController.cs** within **Controllers** folder from the project.</span></span>
2. <span data-ttu-id="c4be8-159">Clique com botão direito do **controladores** pasta no projeto e selecione **adicionar | Controlador** no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="c4be8-159">Right-click the **Controllers** folder in the project and select **Add | Controller** from the context menu.</span></span>

    <span data-ttu-id="c4be8-160">![Adicionando um novo controlador ao projeto](build-restful-apis-with-aspnet-web-api/_static/image3.png "adicionando um novo controlador ao projeto")</span><span class="sxs-lookup"><span data-stu-id="c4be8-160">![Adding a new controller to the project](build-restful-apis-with-aspnet-web-api/_static/image3.png "Adding a new controller to the project")</span></span>

    *<span data-ttu-id="c4be8-161">Adicionando um novo controlador ao projeto</span><span class="sxs-lookup"><span data-stu-id="c4be8-161">Adding a new controller to the project</span></span>*
3. <span data-ttu-id="c4be8-162">No **Adicionar controlador** caixa de diálogo que aparece, selecione **controlador API vazio** no menu modelo.</span><span class="sxs-lookup"><span data-stu-id="c4be8-162">In the **Add Controller** dialog that appears, select **Empty API Controller** from the Template menu.</span></span> <span data-ttu-id="c4be8-163">Nomeie a classe do controlador **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-163">Name the controller class **ContactController**.</span></span> <span data-ttu-id="c4be8-164">Em seguida, clique em **adicionar.**</span><span class="sxs-lookup"><span data-stu-id="c4be8-164">Then, click **Add.**</span></span>

    <span data-ttu-id="c4be8-165">![Usando a caixa de diálogo Adicionar controlador para criar um novo controlador de API da Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "usando a caixa de diálogo Adicionar controlador para criar um novo controlador de API da Web")</span><span class="sxs-lookup"><span data-stu-id="c4be8-165">![Using the Add Controller dialog to create a new Web API controller](build-restful-apis-with-aspnet-web-api/_static/image4.png "Using the Add Controller dialog to create a new Web API controller")</span></span>

    *<span data-ttu-id="c4be8-166">Usando a caixa de diálogo Adicionar controlador para criar um novo controlador de API da Web</span><span class="sxs-lookup"><span data-stu-id="c4be8-166">Using the Add Controller dialog to create a new Web API controller</span></span>*
4. <span data-ttu-id="c4be8-167">Adicione o seguinte código para o **ContactController**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-167">Add the following code to the **ContactController**.</span></span>

    <span data-ttu-id="c4be8-168">(Código de trecho de código – *laboratório de API da Web - Ex01 - obter método API*)</span><span class="sxs-lookup"><span data-stu-id="c4be8-168">(Code Snippet - *Web API Lab - Ex01 - Get API Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. <span data-ttu-id="c4be8-169">Pressione **F5** para depurar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c4be8-169">Press **F5** to debug the application.</span></span> <span data-ttu-id="c4be8-170">A home page padrão para um projeto de API da Web deve aparecer.</span><span class="sxs-lookup"><span data-stu-id="c4be8-170">The default home page for a Web API project should appear.</span></span>

    <span data-ttu-id="c4be8-171">![A home page padrão de um aplicativo de API Web ASP.NET](build-restful-apis-with-aspnet-web-api/_static/image5.png "a home page padrão de um aplicativo de API Web ASP.NET")</span><span class="sxs-lookup"><span data-stu-id="c4be8-171">![The default home page of an ASP.NET Web API application](build-restful-apis-with-aspnet-web-api/_static/image5.png "The default home page of an ASP.NET Web API application")</span></span>

    *<span data-ttu-id="c4be8-172">A home page padrão de um aplicativo de API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c4be8-172">The default home page of an ASP.NET Web API application</span></span>*
6. <span data-ttu-id="c4be8-173">Na janela do Internet Explorer, pressione a **F12** tecla para abrir o **ferramentas de desenvolvedor** janela.</span><span class="sxs-lookup"><span data-stu-id="c4be8-173">In the Internet Explorer window, press the **F12** key to open the **Developer Tools** window.</span></span> <span data-ttu-id="c4be8-174">Clique o **rede** guia e, em seguida, clique no **iniciar captura** botão para começar a capturar o tráfego de rede para a janela.</span><span class="sxs-lookup"><span data-stu-id="c4be8-174">Click the **Network** tab, and then click the **Start Capturing** button to begin capturing network traffic into the window.</span></span>

    <span data-ttu-id="c4be8-175">![Abrindo a guia rede e iniciar captura de rede](build-restful-apis-with-aspnet-web-api/_static/image6.png "abrindo a guia rede e iniciar captura de rede")</span><span class="sxs-lookup"><span data-stu-id="c4be8-175">![Opening the network tab and initiating network capture](build-restful-apis-with-aspnet-web-api/_static/image6.png "Opening the network tab and initiating network capture")</span></span>

    *<span data-ttu-id="c4be8-176">Abrindo a guia rede e iniciar a captura de rede</span><span class="sxs-lookup"><span data-stu-id="c4be8-176">Opening the network tab and initiating network capture</span></span>*
7. <span data-ttu-id="c4be8-177">Anexar a URL na barra de endereços do navegador com **/api/contact** e pressione enter.</span><span class="sxs-lookup"><span data-stu-id="c4be8-177">Append the URL in the browser's address bar with **/api/contact** and press enter.</span></span> <span data-ttu-id="c4be8-178">Os detalhes de transmissão aparecerá na janela de captura de rede.</span><span class="sxs-lookup"><span data-stu-id="c4be8-178">The transmission details will appear in the network capture window.</span></span> <span data-ttu-id="c4be8-179">Observe que o tipo MIME da resposta **application/json**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-179">Note that the response's MIME type is **application/json**.</span></span> <span data-ttu-id="c4be8-180">Isso demonstra como o formato de saída padrão é JSON.</span><span class="sxs-lookup"><span data-stu-id="c4be8-180">This demonstrates how the default output format is JSON.</span></span>

    <span data-ttu-id="c4be8-181">![Exibindo a saída da solicitação de API da Web no modo de exibição de rede](build-restful-apis-with-aspnet-web-api/_static/image7.png "exibindo a saída da solicitação de API da Web no modo de exibição de rede")</span><span class="sxs-lookup"><span data-stu-id="c4be8-181">![Viewing the output of the Web API request in the Network view](build-restful-apis-with-aspnet-web-api/_static/image7.png "Viewing the output of the Web API request in the Network view")</span></span>

    *<span data-ttu-id="c4be8-182">Exibindo a saída da solicitação de API da Web no modo de exibição de rede</span><span class="sxs-lookup"><span data-stu-id="c4be8-182">Viewing the output of the Web API request in the Network view</span></span>*

    > [!NOTE]
    > <span data-ttu-id="c4be8-183">Comportamento de padrão do Internet Explorer 10 no momento será perguntar se o usuário gostaria de salvar ou abrir o fluxo resultante da chamada à API Web.</span><span class="sxs-lookup"><span data-stu-id="c4be8-183">Internet Explorer 10's default behavior at this point will be to ask if the user would like to save or open the stream resulting from the Web API call.</span></span> <span data-ttu-id="c4be8-184">A saída será um arquivo de texto que contém o resultado JSON da chamada de URL da API Web.</span><span class="sxs-lookup"><span data-stu-id="c4be8-184">The output will be a text file containing the JSON result of the Web API URL call.</span></span> <span data-ttu-id="c4be8-185">Não cancele a caixa de diálogo para que seja possível assistir o conteúdo da resposta por meio da janela de ferramentas de desenvolvedores.</span><span class="sxs-lookup"><span data-stu-id="c4be8-185">Do not cancel the dialog in order to be able to watch the response's content through Developers Tool window.</span></span>
8. <span data-ttu-id="c4be8-186">Clique o **ir para a exibição detalhada** botão para ver mais detalhes sobre a resposta dessa chamada de API.</span><span class="sxs-lookup"><span data-stu-id="c4be8-186">Click the **Go to detailed view** button to see more details about the response of this API call.</span></span>

    <span data-ttu-id="c4be8-187">![Alternar para a exibição detalhada](build-restful-apis-with-aspnet-web-api/_static/image8.png "alternar para a exibição de detalhes")</span><span class="sxs-lookup"><span data-stu-id="c4be8-187">![Switch to Detailed View](build-restful-apis-with-aspnet-web-api/_static/image8.png "Switch to Details View")</span></span>

    *<span data-ttu-id="c4be8-188">Alternar para a exibição detalhada</span><span class="sxs-lookup"><span data-stu-id="c4be8-188">Switch to Detailed View</span></span>*
9. <span data-ttu-id="c4be8-189">Clique o **corpo da resposta** guia para exibir o texto de resposta JSON real.</span><span class="sxs-lookup"><span data-stu-id="c4be8-189">Click the **Response body** tab to view the actual JSON response text.</span></span>

    <span data-ttu-id="c4be8-190">![Exibir o JSON de saída de texto no monitor de rede](build-restful-apis-with-aspnet-web-api/_static/image9.png "exibir o JSON de saída de texto no monitor de rede")</span><span class="sxs-lookup"><span data-stu-id="c4be8-190">![Viewing the JSON output text in the network monitor](build-restful-apis-with-aspnet-web-api/_static/image9.png "Viewing the JSON output text in the network monitor")</span></span>

    *<span data-ttu-id="c4be8-191">Exibindo o texto de saída JSON no monitor de rede</span><span class="sxs-lookup"><span data-stu-id="c4be8-191">Viewing the JSON output text in the network monitor</span></span>*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a><span data-ttu-id="c4be8-192">Tarefa 3: Criando modelos de contato e aumente o controlador de contato</span><span class="sxs-lookup"><span data-stu-id="c4be8-192">Task 3 - Creating the Contact Models and Augment the Contact Controller</span></span>

<span data-ttu-id="c4be8-193">Nesta tarefa, você criará as classes do controlador no qual os métodos de API residirá.</span><span class="sxs-lookup"><span data-stu-id="c4be8-193">In this task, you will create the controller classes in which API methods will reside.</span></span>

1. <span data-ttu-id="c4be8-194">Clique com botão direito do **modelos** pasta e selecione **adicionar | Classe...**  no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="c4be8-194">Right-click the **Models** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="c4be8-195">![Adicionando um novo modelo para o aplicativo web](build-restful-apis-with-aspnet-web-api/_static/image10.png "adicionando um novo modelo para o aplicativo web")</span><span class="sxs-lookup"><span data-stu-id="c4be8-195">![Adding a new model to the web application](build-restful-apis-with-aspnet-web-api/_static/image10.png "Adding a new model to the web application")</span></span>

    *<span data-ttu-id="c4be8-196">Adicionando um novo modelo para o aplicativo web</span><span class="sxs-lookup"><span data-stu-id="c4be8-196">Adding a new model to the web application</span></span>*
2. <span data-ttu-id="c4be8-197">No **Adicionar Novo Item** caixa de diálogo, nomeie o novo arquivo **Contact.cs** e clique em **adicionar.**</span><span class="sxs-lookup"><span data-stu-id="c4be8-197">In the **Add New Item** dialog, name the new file **Contact.cs** and click **Add.**</span></span>

    <span data-ttu-id="c4be8-198">![Criar o novo arquivo de classe de contato](build-restful-apis-with-aspnet-web-api/_static/image11.png "criar o novo arquivo de classe do contato")</span><span class="sxs-lookup"><span data-stu-id="c4be8-198">![Creating the new Contact class file](build-restful-apis-with-aspnet-web-api/_static/image11.png "Creating the new Contact class file")</span></span>

    *<span data-ttu-id="c4be8-199">Criar o novo arquivo de classe do contato</span><span class="sxs-lookup"><span data-stu-id="c4be8-199">Creating the new Contact class file</span></span>*
3. <span data-ttu-id="c4be8-200">Adicione o seguinte código realçado para o **entre em contato com** classe.</span><span class="sxs-lookup"><span data-stu-id="c4be8-200">Add the following highlighted code to the **Contact** class.</span></span>

    <span data-ttu-id="c4be8-201">(Código de trecho de código – *classe de contato do laboratório de API - Ex01 - Web*)</span><span class="sxs-lookup"><span data-stu-id="c4be8-201">(Code Snippet - *Web API Lab - Ex01 - Contact Class*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. <span data-ttu-id="c4be8-202">No **ContactController** classe, selecione a palavra **cadeia de caracteres** na definição de método da **obter** método e digite a palavra *entre em contato com*.</span><span class="sxs-lookup"><span data-stu-id="c4be8-202">In the **ContactController** class, select the word **string** in method definition of the **Get** method, and type the word *Contact*.</span></span> <span data-ttu-id="c4be8-203">Depois que o word é digitado, um indicador será exibido no início da palavra **entre em contato com**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-203">Once the word is typed in, an indicator will appear at the beginning of the word **Contact**.</span></span> <span data-ttu-id="c4be8-204">O mantenha pressionada a **Ctrl** da chave e pressione a tecla de ponto (.) ou clique no ícone de usando o mouse para abrir a caixa de diálogo de Ajuda no editor de código para preencher automaticamente a **usando** para os modelos de diretiva namespace.</span><span class="sxs-lookup"><span data-stu-id="c4be8-204">Either hold down the **Ctrl** key and press the period (.) key or click the icon using your mouse to open up the assistance dialog in the code editor, to automatically fill in the **using** directive for the Models namespace.</span></span>

    ![Usando a assistência do Intellisense para declarações de namespace](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *<span data-ttu-id="c4be8-206">Usando a assistência do Intellisense para declarações de namespace</span><span class="sxs-lookup"><span data-stu-id="c4be8-206">Using Intellisense assistance for namespace declarations</span></span>*
5. <span data-ttu-id="c4be8-207">Modificar o código para o **obter** , de modo que ele retorna uma matriz de instâncias de modelo de contato.</span><span class="sxs-lookup"><span data-stu-id="c4be8-207">Modify the code for the **Get** method so that it returns an array of Contact model instances.</span></span>

    <span data-ttu-id="c4be8-208">(Código de trecho de código – *laboratório de API da Web - Ex01 - retornando uma lista de contatos*)</span><span class="sxs-lookup"><span data-stu-id="c4be8-208">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. <span data-ttu-id="c4be8-209">Pressione **F5** para depurar o aplicativo web no navegador.</span><span class="sxs-lookup"><span data-stu-id="c4be8-209">Press **F5** to debug the web application in the browser.</span></span> <span data-ttu-id="c4be8-210">Para exibir as alterações feitas à saída de resposta da API, execute as seguintes etapas.</span><span class="sxs-lookup"><span data-stu-id="c4be8-210">To view the changes made to the response output of the API, perform the following steps.</span></span>

   1. <span data-ttu-id="c4be8-211">Depois que o navegador é aberto, pressione **F12** se as ferramentas de desenvolvedor ainda não estiverem abertas.</span><span class="sxs-lookup"><span data-stu-id="c4be8-211">Once the browser opens, press **F12** if the developer tools are not open yet.</span></span>
   2. <span data-ttu-id="c4be8-212">Clique o **rede** guia.</span><span class="sxs-lookup"><span data-stu-id="c4be8-212">Click the **Network** tab.</span></span>
   3. <span data-ttu-id="c4be8-213">Pressione a **iniciar captura** botão.</span><span class="sxs-lookup"><span data-stu-id="c4be8-213">Press the **Start Capturing** button.</span></span>
   4. <span data-ttu-id="c4be8-214">Adicione o sufixo de URL **/api/contact** para a URL na barra de endereços e pressione a **Enter** chave.</span><span class="sxs-lookup"><span data-stu-id="c4be8-214">Add the URL suffix **/api/contact** to the URL in the address bar and press the **Enter** key.</span></span>
   5. <span data-ttu-id="c4be8-215">Pressione a **ir para a exibição detalhada** botão.</span><span class="sxs-lookup"><span data-stu-id="c4be8-215">Press the **Go to detailed view** button.</span></span>
   6. <span data-ttu-id="c4be8-216">Selecione o **corpo da resposta** guia. Você deve ver uma cadeia de caracteres JSON que representa o formato serializado de uma matriz de instâncias de contato.</span><span class="sxs-lookup"><span data-stu-id="c4be8-216">Select the **Response body** tab. You should see a JSON string representing the serialized form of an array of Contact instances.</span></span>

      <span data-ttu-id="c4be8-217">![Saída de uma chamada de método de API da Web complexa serializado de JSON](build-restful-apis-with-aspnet-web-api/_static/image13.png "saída de uma chamada de método de API da Web complexa serializado de JSON")</span><span class="sxs-lookup"><span data-stu-id="c4be8-217">![JSON serialized output of a complex Web API method call](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serialized output of a complex Web API method call")</span></span>

      *<span data-ttu-id="c4be8-218">Saída de uma chamada de método de API da Web complexa serializado de JSON</span><span class="sxs-lookup"><span data-stu-id="c4be8-218">JSON serialized output of a complex Web API method call</span></span>*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a><span data-ttu-id="c4be8-219">Tarefa 4 - funcionalidade de extração em uma camada de serviço</span><span class="sxs-lookup"><span data-stu-id="c4be8-219">Task 4 - Extracting Functionality into a Service Layer</span></span>

<span data-ttu-id="c4be8-220">Essa tarefa demonstrará como extrair a funcionalidade em uma camada de serviço para torná-lo mais fácil para os desenvolvedores a separar sua funcionalidade de serviço da camada de controlador, permitindo assim que a capacidade de reutilização dos serviços que realmente fazem o trabalho.</span><span class="sxs-lookup"><span data-stu-id="c4be8-220">This task will demonstrate how to extract functionality into a Service layer to make it easy for developers to separate their service functionality from the controller layer, thereby allowing reusability of the services that actually do the work.</span></span>

1. <span data-ttu-id="c4be8-221">Crie uma nova pasta na raiz da solução e nomeie- **Services**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-221">Create a new folder in the solution root and name it **Services**.</span></span> <span data-ttu-id="c4be8-222">Para fazer isso, clique com botão direito **ContactManager** projeto, selecione **Add** | **nova pasta**, nomeie-o *serviços*.</span><span class="sxs-lookup"><span data-stu-id="c4be8-222">To do this, right-click **ContactManager** project, select **Add** | **New Folder**, name it *Services*.</span></span>

    <span data-ttu-id="c4be8-223">![Criando pasta de serviços](build-restful-apis-with-aspnet-web-api/_static/image14.png "pasta criando serviços")</span><span class="sxs-lookup"><span data-stu-id="c4be8-223">![Creating Services folder](build-restful-apis-with-aspnet-web-api/_static/image14.png "Creating Services folder")</span></span>

    *<span data-ttu-id="c4be8-224">Criando a pasta de serviços</span><span class="sxs-lookup"><span data-stu-id="c4be8-224">Creating Services folder</span></span>*
2. <span data-ttu-id="c4be8-225">Clique com botão direito do **Services** pasta e selecione **adicionar | Classe...**  no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="c4be8-225">Right-click the **Services** folder and select **Add | Class...** from the context menu.</span></span>

    <span data-ttu-id="c4be8-226">![Adicionando uma nova classe para a pasta de serviços](build-restful-apis-with-aspnet-web-api/_static/image15.png "adicionando uma nova classe para a pasta de serviços")</span><span class="sxs-lookup"><span data-stu-id="c4be8-226">![Adding a new class to the Services folder](build-restful-apis-with-aspnet-web-api/_static/image15.png "Adding a new class to the Services folder")</span></span>

    *<span data-ttu-id="c4be8-227">Adicionando uma nova classe para a pasta de serviços</span><span class="sxs-lookup"><span data-stu-id="c4be8-227">Adding a new class to the Services folder</span></span>*
3. <span data-ttu-id="c4be8-228">Quando o **Adicionar Novo Item** caixa de diálogo aparece, nomeie a nova classe **ContactRepository** e clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-228">When the **Add New Item** dialog appears, name the new class **ContactRepository** and click **Add**.</span></span>

    <span data-ttu-id="c4be8-229">![Criando um arquivo de classe para conter o código para a camada de serviço de repositório de contato](build-restful-apis-with-aspnet-web-api/_static/image16.png "criando um arquivo de classe para conter o código para a camada de serviço de repositório de contato")</span><span class="sxs-lookup"><span data-stu-id="c4be8-229">![Creating a class file to contain the code for the Contact Repository service layer](build-restful-apis-with-aspnet-web-api/_static/image16.png "Creating a class file to contain the code for the Contact Repository service layer")</span></span>

    *<span data-ttu-id="c4be8-230">Criando um arquivo de classe para conter o código para a camada de serviço de repositório de contato</span><span class="sxs-lookup"><span data-stu-id="c4be8-230">Creating a class file to contain the code for the Contact Repository service layer</span></span>*
4. <span data-ttu-id="c4be8-231">Adicionar uma diretiva para o **ContactRepository.cs** arquivo para incluir o namespace de modelos.</span><span class="sxs-lookup"><span data-stu-id="c4be8-231">Add a using directive to the **ContactRepository.cs** file to include the models namespace.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. <span data-ttu-id="c4be8-232">Adicione o seguinte código realçado para o **ContactRepository.cs** arquivo para implementar o método GetAllContacts.</span><span class="sxs-lookup"><span data-stu-id="c4be8-232">Add the following highlighted code to the **ContactRepository.cs** file to implement GetAllContacts method.</span></span>

    <span data-ttu-id="c4be8-233">(Código de trecho de código – *Web repositório contato do laboratório de API - Ex01 -*)</span><span class="sxs-lookup"><span data-stu-id="c4be8-233">(Code Snippet - *Web API Lab - Ex01 - Contact Repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. <span data-ttu-id="c4be8-234">Abra o **ContactController.cs** arquivo caso ele não ainda estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="c4be8-234">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="c4be8-235">Adicione a seguinte instrução using à seção de declaração de namespace do arquivo.</span><span class="sxs-lookup"><span data-stu-id="c4be8-235">Add the following using statement to the namespace declaration section of the file.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. <span data-ttu-id="c4be8-236">Adicione o seguinte código realçado para o **ContactController.cs** classe para adicionar um campo particular para representar a instância do repositório, para que o restante da classe que os membros podem fazer usar da implementação do serviço.</span><span class="sxs-lookup"><span data-stu-id="c4be8-236">Add the following highlighted code to the **ContactController.cs** class to add a private field to represent the instance of the repository, so that the rest of the class members can make use of the service implementation.</span></span>

    <span data-ttu-id="c4be8-237">(Código de trecho de código – *controlador de contato do laboratório de API - Ex01 - Web*)</span><span class="sxs-lookup"><span data-stu-id="c4be8-237">(Code Snippet - *Web API Lab - Ex01 - Contact Controller*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. <span data-ttu-id="c4be8-238">Alterar o **obter** , de modo que ele faz ao uso do serviço de repositório de contato.</span><span class="sxs-lookup"><span data-stu-id="c4be8-238">Change the **Get** method so that it makes use of the contact repository service.</span></span>

    <span data-ttu-id="c4be8-239">(Código de trecho de código – *laboratório de API da Web - Ex01 - retornando uma lista de contatos por meio do repositório*)</span><span class="sxs-lookup"><span data-stu-id="c4be8-239">(Code Snippet - *Web API Lab - Ex01 - Returning a list of contacts via the repository*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. <span data-ttu-id="c4be8-240">Colocar um ponto de interrupção a **ContactController**do **obter** definição de método.</span><span class="sxs-lookup"><span data-stu-id="c4be8-240">Put a breakpoint on the **ContactController**'s **Get** method definition.</span></span>

   <span data-ttu-id="c4be8-241">![Adicionar pontos de interrupção ao controlador de contato](build-restful-apis-with-aspnet-web-api/_static/image17.png "adicionar pontos de interrupção ao controlador de contato")</span><span class="sxs-lookup"><span data-stu-id="c4be8-241">![Adding breakpoints to the contact controller](build-restful-apis-with-aspnet-web-api/_static/image17.png "Adding breakpoints to the contact controller")</span></span>

   *<span data-ttu-id="c4be8-242">Adicionar pontos de interrupção ao controlador de contato</span><span class="sxs-lookup"><span data-stu-id="c4be8-242">Adding breakpoints to the contact controller</span></span>*
11. <span data-ttu-id="c4be8-243">Pressione **F5** para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c4be8-243">Press **F5** to run the application.</span></span>
12. <span data-ttu-id="c4be8-244">Quando o navegador é aberto, pressione **F12** para abrir as ferramentas de desenvolvedor.</span><span class="sxs-lookup"><span data-stu-id="c4be8-244">When the browser opens, press **F12** to open the developer tools.</span></span>
13. <span data-ttu-id="c4be8-245">Clique o **rede** guia.</span><span class="sxs-lookup"><span data-stu-id="c4be8-245">Click the **Network** tab.</span></span>
14. <span data-ttu-id="c4be8-246">Clique o **iniciar captura** botão.</span><span class="sxs-lookup"><span data-stu-id="c4be8-246">Click the **Start Capturing** button.</span></span>
15. <span data-ttu-id="c4be8-247">Anexar a URL na barra de endereços com o sufixo **/api/contact** e pressione **Enter** para carregar o controlador da API.</span><span class="sxs-lookup"><span data-stu-id="c4be8-247">Append the URL in the address bar with the suffix **/api/contact** and press **Enter** to load the API controller.</span></span>
16. <span data-ttu-id="c4be8-248">Visual Studio 2012 deve ser interrompido uma vez **obter** método começa a ser executada.</span><span class="sxs-lookup"><span data-stu-id="c4be8-248">Visual Studio 2012 should break once **Get** method begins execution.</span></span>

   <span data-ttu-id="c4be8-249">![Quebra no método Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "quebra no método Get")</span><span class="sxs-lookup"><span data-stu-id="c4be8-249">![Breaking within the Get method](build-restful-apis-with-aspnet-web-api/_static/image18.png "Breaking within the Get method")</span></span>

   *<span data-ttu-id="c4be8-250">Quebra no método Get</span><span class="sxs-lookup"><span data-stu-id="c4be8-250">Breaking within the Get method</span></span>*
17. <span data-ttu-id="c4be8-251">Pressione **F5** para continuar.</span><span class="sxs-lookup"><span data-stu-id="c4be8-251">Press **F5** to continue.</span></span>
18. <span data-ttu-id="c4be8-252">Volte para o Internet Explorer se ele não ainda estiver em foco.</span><span class="sxs-lookup"><span data-stu-id="c4be8-252">Go back to Internet Explorer if it is not already in focus.</span></span> <span data-ttu-id="c4be8-253">Observe a janela de captura de rede.</span><span class="sxs-lookup"><span data-stu-id="c4be8-253">Note the network capture window.</span></span>

    <span data-ttu-id="c4be8-254">![Modo de exibição no Internet Explorer, mostrando os resultados da chamada à API da Web de rede](build-restful-apis-with-aspnet-web-api/_static/image19.png "exibição no Internet Explorer, mostrando os resultados da chamada à API da Web de rede")</span><span class="sxs-lookup"><span data-stu-id="c4be8-254">![Network view in Internet Explorer showing results of the Web API call](build-restful-apis-with-aspnet-web-api/_static/image19.png "Network view in Internet Explorer showing results of the Web API call")</span></span>

    *<span data-ttu-id="c4be8-255">Exibição de rede no Internet Explorer, mostrando os resultados da chamada à API da Web</span><span class="sxs-lookup"><span data-stu-id="c4be8-255">Network view in Internet Explorer showing results of the Web API call</span></span>*
19. <span data-ttu-id="c4be8-256">Clique o **ir para a exibição detalhada** botão.</span><span class="sxs-lookup"><span data-stu-id="c4be8-256">Click the **Go to detailed view** button.</span></span>
20. <span data-ttu-id="c4be8-257">Clique o **corpo da resposta** guia. Observe a saída JSON da chamada à API e como ele representa dois contatos recuperados pela camada de serviço.</span><span class="sxs-lookup"><span data-stu-id="c4be8-257">Click the **Response body** tab. Note the JSON output of the API call, and how it represents the two contacts retrieved by the service layer.</span></span>

    <span data-ttu-id="c4be8-258">![Exibindo a saída JSON da API Web na janela de ferramentas de desenvolvedor](build-restful-apis-with-aspnet-web-api/_static/image20.png "exibindo a saída JSON da API Web na janela de ferramentas de desenvolvedor")</span><span class="sxs-lookup"><span data-stu-id="c4be8-258">![Viewing the JSON output from the Web API in the developer tools window](build-restful-apis-with-aspnet-web-api/_static/image20.png "Viewing the JSON output from the Web API in the developer tools window")</span></span>

    *<span data-ttu-id="c4be8-259">Exibindo a saída JSON da API Web na janela de ferramentas de desenvolvedor</span><span class="sxs-lookup"><span data-stu-id="c4be8-259">Viewing the JSON output from the Web API in the developer tools window</span></span>*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a><span data-ttu-id="c4be8-260">Exercício 2: Criar uma API da Web de leitura/gravação</span><span class="sxs-lookup"><span data-stu-id="c4be8-260">Exercise 2: Create a Read/Write Web API</span></span>

<span data-ttu-id="c4be8-261">Neste exercício, você implementará o POST e PUT métodos para o Gerenciador de contato para habilitá-lo com os recursos de edição de dados.</span><span class="sxs-lookup"><span data-stu-id="c4be8-261">In this exercise, you will implement POST and PUT methods for the contact manager to enable it with data-editing features.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a><span data-ttu-id="c4be8-262">Tarefa 1 - abrindo o projeto de API da Web</span><span class="sxs-lookup"><span data-stu-id="c4be8-262">Task 1 - Opening the Web API Project</span></span>

<span data-ttu-id="c4be8-263">Nesta tarefa, você preparará aprimorar o projeto de API da Web criado no Exercício 1 para que ele pode aceitar a entrada do usuário.</span><span class="sxs-lookup"><span data-stu-id="c4be8-263">In this task, you will prepare to enhance the Web API project created in Exercise 1 so that it can accept user input.</span></span>

1. <span data-ttu-id="c4be8-264">Execute **Visual Studio 2012 Express para Web**, para fazer isso, vá para **inicie** e digite **VS Express para Web** , em seguida, pressione **Enter**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-264">Run **Visual Studio 2012 Express for Web**, to do this go to **Start** and type **VS Express for Web** then press **Enter**.</span></span>
2. <span data-ttu-id="c4be8-265">Abra o **começar** solução localizado em **origem/Ex02-ReadWriteWebAPI/início/** pasta.</span><span class="sxs-lookup"><span data-stu-id="c4be8-265">Open the **Begin** solution located at **Source/Ex02-ReadWriteWebAPI/Begin/** folder.</span></span> <span data-ttu-id="c4be8-266">Caso contrário, você pode continuar usando o **final** solução obtida ao concluir o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="c4be8-266">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="c4be8-267">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="c4be8-267">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c4be8-268">Para fazer isso, clique o **Project** menu e selecione **Manage NuGet Packages**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-268">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c4be8-269">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="c4be8-269">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c4be8-270">Por fim, compile a solução clicando **construir** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-270">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c4be8-271">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="c4be8-271">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c4be8-272">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="c4be8-272">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c4be8-273">É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="c4be8-273">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="c4be8-274">Abra o **Services/ContactRepository.cs** arquivo.</span><span class="sxs-lookup"><span data-stu-id="c4be8-274">Open the **Services/ContactRepository.cs** file.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a><span data-ttu-id="c4be8-275">Tarefa 2 - adicionando recursos de persistência de dados para a implementação de repositório de contato</span><span class="sxs-lookup"><span data-stu-id="c4be8-275">Task 2 - Adding Data-Persistence Features to the Contact Repository Implementation</span></span>

<span data-ttu-id="c4be8-276">Nesta tarefa, você irá aumentar a classe ContactRepository do projeto de API da Web criado no Exercício 1 para que ele possa persistir e aceitar a entrada do usuário e as novas instâncias de contato.</span><span class="sxs-lookup"><span data-stu-id="c4be8-276">In this task, you will augment the ContactRepository class of the Web API project created in Exercise 1 so that it can persist and accept user input and new Contact instances.</span></span>

1. <span data-ttu-id="c4be8-277">Adicione a seguinte constante para o **ContactRepository** classe para representar o nome do web server cache item nome da chave mais tarde neste exercício.</span><span class="sxs-lookup"><span data-stu-id="c4be8-277">Add the following constant to the **ContactRepository** class to represent the name of the web server cache item key name later in this exercise.</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. <span data-ttu-id="c4be8-278">Adicione um construtor para o **ContactRepository** que contém o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="c4be8-278">Add a constructor to the **ContactRepository** containing the following code.</span></span>

    <span data-ttu-id="c4be8-279">(Código de trecho de código – *Web API laboratório - Ex02 - repositório contato construtor*)</span><span class="sxs-lookup"><span data-stu-id="c4be8-279">(Code Snippet - *Web API Lab - Ex02 - Contact Repository Constructor*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. <span data-ttu-id="c4be8-280">Modificar o código para o **GetAllContacts** método conforme demonstrado a seguir.</span><span class="sxs-lookup"><span data-stu-id="c4be8-280">Modify the code for the **GetAllContacts** method as demonstrated below.</span></span>

    <span data-ttu-id="c4be8-281">(Código de trecho de código – *laboratório de API da Web - Ex02 - obter todos os contatos*)</span><span class="sxs-lookup"><span data-stu-id="c4be8-281">(Code Snippet - *Web API Lab - Ex02 - Get All Contacts*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="c4be8-282">Este exemplo é para fins de demonstração e usará cache do servidor web como um meio de armazenamento, para que os valores serão estar disponível para vários clientes simultaneamente, em vez de usar um mecanismo de armazenamento de sessão ou um tempo de vida de armazenamento de solicitação.</span><span class="sxs-lookup"><span data-stu-id="c4be8-282">This example is for demonstration purposes and will use the web server's cache as a storage medium, so that the values will be available to multiple clients simultaneously, rather than use a Session storage mechanism or a Request storage lifetime.</span></span> <span data-ttu-id="c4be8-283">Um pode usar o Entity Framework, armazenamento XML ou qualquer outra variedade no lugar de cache do servidor web.</span><span class="sxs-lookup"><span data-stu-id="c4be8-283">One could use Entity Framework, XML storage, or any other variety in place of the web server cache.</span></span>
4. <span data-ttu-id="c4be8-284">Implementar um novo método chamado **SaveContact** para o **ContactRepository** classe para fazer o trabalho de salvar um contato.</span><span class="sxs-lookup"><span data-stu-id="c4be8-284">Implement a new method named **SaveContact** to the **ContactRepository** class to do the work of saving a contact.</span></span> <span data-ttu-id="c4be8-285">O **SaveContact** método deve adotar uma única **contato** valor de parâmetro e retornar um booleano indicando o êxito ou falha.</span><span class="sxs-lookup"><span data-stu-id="c4be8-285">The **SaveContact** method should take a single **Contact** parameter and return a Boolean value indicating success or failure.</span></span>

    <span data-ttu-id="c4be8-286">(Código de trecho de código – *Web API laboratório - Ex02 - Implementando o método SaveContact*)</span><span class="sxs-lookup"><span data-stu-id="c4be8-286">(Code Snippet - *Web API Lab - Ex02 - Implementing the SaveContact Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a><span data-ttu-id="c4be8-287">Exercício 3: Consumir a API da Web de um cliente HTML</span><span class="sxs-lookup"><span data-stu-id="c4be8-287">Exercise 3: Consume the Web API from an HTML Client</span></span>

<span data-ttu-id="c4be8-288">Neste exercício, você criará um cliente HTML para chamar a API da Web.</span><span class="sxs-lookup"><span data-stu-id="c4be8-288">In this exercise, you will create an HTML client to call the Web API.</span></span> <span data-ttu-id="c4be8-289">Esse cliente facilitam a troca de dados com a API da Web usando JavaScript e exibirá os resultados em um navegador da web usando marcação HTML.</span><span class="sxs-lookup"><span data-stu-id="c4be8-289">This client will facilitate data exchange with the Web API using JavaScript and will display the results in a web browser using HTML markup.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a><span data-ttu-id="c4be8-290">Tarefa 1 - modificando a exibição de índice para fornecer uma GUI para exibir contatos</span><span class="sxs-lookup"><span data-stu-id="c4be8-290">Task 1 - Modifying the Index View to Provide a GUI for Displaying Contacts</span></span>

<span data-ttu-id="c4be8-291">Nesta tarefa, você modificará a exibição de índice padrão do aplicativo web para dar suporte a necessidade de exibir a lista de contatos existentes em um navegador HTML.</span><span class="sxs-lookup"><span data-stu-id="c4be8-291">In this task, you will modify the default Index view of the web application to support the requirement of displaying the list of existing contacts in an HTML browser.</span></span>

1. <span data-ttu-id="c4be8-292">Abra **Visual Studio 2012 Express para Web** se ele não ainda estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="c4be8-292">Open **Visual Studio 2012 Express for Web** if it is not already open.</span></span>
2. <span data-ttu-id="c4be8-293">Abra o **começar** solução localizado em **origem/Ex03-ConsumingWebAPI/início/** pasta.</span><span class="sxs-lookup"><span data-stu-id="c4be8-293">Open the **Begin** solution located at **Source/Ex03-ConsumingWebAPI/Begin/** folder.</span></span> <span data-ttu-id="c4be8-294">Caso contrário, você pode continuar usando o **final** solução obtida ao concluir o exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="c4be8-294">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="c4be8-295">Se você abriu fornecido **começar** solução, você precisará baixar alguns pacotes do NuGet ausentes antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="c4be8-295">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="c4be8-296">Para fazer isso, clique o **Project** menu e selecione **Manage NuGet Packages**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-296">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="c4be8-297">No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.</span><span class="sxs-lookup"><span data-stu-id="c4be8-297">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="c4be8-298">Por fim, compile a solução clicando **construir** | **compilar solução**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-298">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="c4be8-299">Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto.</span><span class="sxs-lookup"><span data-stu-id="c4be8-299">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="c4be8-300">Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="c4be8-300">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="c4be8-301">É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="c4be8-301">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="c4be8-302">Abra o **index. cshtml** arquivo localizado em **exibições/inicial** pasta.</span><span class="sxs-lookup"><span data-stu-id="c4be8-302">Open the **Index.cshtml** file located at **Views/Home** folder.</span></span>
4. <span data-ttu-id="c4be8-303">Substitua o código HTML dentro do elemento div com id **corpo** para que ele se parece com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="c4be8-303">Replace the HTML code within the div element with id **body** so that it looks like the following code.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. <span data-ttu-id="c4be8-304">Adicione o seguinte código Javascript na parte inferior do arquivo para executar a solicitação HTTP para a API da Web.</span><span class="sxs-lookup"><span data-stu-id="c4be8-304">Add the following Javascript code at the bottom of the file to perform the HTTP request to the Web API.</span></span>

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. <span data-ttu-id="c4be8-305">Abra o **ContactController.cs** arquivo caso ele não ainda estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="c4be8-305">Open the **ContactController.cs** file if it is not already open.</span></span>
7. <span data-ttu-id="c4be8-306">Colocar um ponto de interrupção na **Obtenha** método o **ContactController** classe.</span><span class="sxs-lookup"><span data-stu-id="c4be8-306">Place a breakpoint on the **Get** method of the **ContactController** class.</span></span>

    <span data-ttu-id="c4be8-307">![Colocar um ponto de interrupção no método Get do controlador de API](build-restful-apis-with-aspnet-web-api/_static/image21.png "colocando um ponto de interrupção no método Get do controlador de API")</span><span class="sxs-lookup"><span data-stu-id="c4be8-307">![Placing a breakpoint on the Get method of the API controller](build-restful-apis-with-aspnet-web-api/_static/image21.png "Placing a breakpoint on the Get method of the API controller")</span></span>

    *<span data-ttu-id="c4be8-308">Colocar um ponto de interrupção no método Get do controlador de API</span><span class="sxs-lookup"><span data-stu-id="c4be8-308">Placing a breakpoint on the Get method of the API controller</span></span>*
8. <span data-ttu-id="c4be8-309">Pressione **F5** para executar o projeto.</span><span class="sxs-lookup"><span data-stu-id="c4be8-309">Press **F5** to run the project.</span></span> <span data-ttu-id="c4be8-310">O navegador carregará o documento HTML.</span><span class="sxs-lookup"><span data-stu-id="c4be8-310">The browser will load the HTML document.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c4be8-311">Certifique-se de que você está navegando para a URL raiz do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c4be8-311">Ensure that you are browsing to the root URL of your application.</span></span>
9. <span data-ttu-id="c4be8-312">Depois que a página for carregada e o JavaScript é executado, o ponto de interrupção será atingido e fará uma pausa a execução do código no controlador.</span><span class="sxs-lookup"><span data-stu-id="c4be8-312">Once the page loads and the JavaScript executes, the breakpoint will be hit and the code execution will pause in the controller.</span></span>

    <span data-ttu-id="c4be8-313">![Depuração nas chamadas de API da Web usando o VS Express para Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "depuração nas chamadas de API da Web usando o VS Express para Web")</span><span class="sxs-lookup"><span data-stu-id="c4be8-313">![Debugging into the Web API calls using VS Express for Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "Debugging into the Web API calls using VS Express for Web")</span></span>

    *<span data-ttu-id="c4be8-314">Depuração para a chamada de API da Web usando o Visual Studio 2012 Express para Web</span><span class="sxs-lookup"><span data-stu-id="c4be8-314">Debugging into the Web API call using Visual Studio 2012 Express for Web</span></span>*
10. <span data-ttu-id="c4be8-315">Remover o ponto de interrupção e pressione **F5** ou a barra de ferramentas depuração **continuar** botão para continuar carregando o modo de exibição no navegador.</span><span class="sxs-lookup"><span data-stu-id="c4be8-315">Remove the breakpoint and press **F5** or the debugging toolbar's **Continue** button to continue loading the view in the browser.</span></span> <span data-ttu-id="c4be8-316">Após a conclusão da chamada à API Web você deve ver os contatos retornados da API Web chamar exibidos como itens de lista no navegador.</span><span class="sxs-lookup"><span data-stu-id="c4be8-316">Once the Web API call completes you should see the contacts returned from the Web API call displayed as list items in the browser.</span></span>

    <span data-ttu-id="c4be8-317">![Resultados da chamada à API exibida no navegador, como itens de lista](build-restful-apis-with-aspnet-web-api/_static/image23.png "resultados da chamada à API exibida no navegador, como itens de lista")</span><span class="sxs-lookup"><span data-stu-id="c4be8-317">![Results of the API call displayed in the browser as list items](build-restful-apis-with-aspnet-web-api/_static/image23.png "Results of the API call displayed in the browser as list items")</span></span>

    *<span data-ttu-id="c4be8-318">Resultados da chamada à API exibida no navegador, como itens de lista</span><span class="sxs-lookup"><span data-stu-id="c4be8-318">Results of the API call displayed in the browser as list items</span></span>*
11. <span data-ttu-id="c4be8-319">Pare a depuração.</span><span class="sxs-lookup"><span data-stu-id="c4be8-319">Stop debugging.</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a><span data-ttu-id="c4be8-320">Tarefa 2 – modificar a exibição de índice para fornecer uma interface gráfica para criação de contatos</span><span class="sxs-lookup"><span data-stu-id="c4be8-320">Task 2 - Modifying the Index View to Provide a GUI for Creating Contacts</span></span>

<span data-ttu-id="c4be8-321">Nesta tarefa, você continuará modificar a exibição de índice do aplicativo MVC.</span><span class="sxs-lookup"><span data-stu-id="c4be8-321">In this task, you will continue to modify the Index view of the MVC application.</span></span> <span data-ttu-id="c4be8-322">Um formulário será adicionado à página HTML que irão capturar a entrada do usuário e enviá-lo para a API da Web para criar um novo contato e um novo método de controlador de API da Web será criado para coletar a data da GUI.</span><span class="sxs-lookup"><span data-stu-id="c4be8-322">A form will be added to the HTML page that will capture user input and send it to the Web API to create a new Contact, and a new Web API controller method will be created to collect date from the GUI.</span></span>

1. <span data-ttu-id="c4be8-323">Abra o **ContactController.cs** arquivo.</span><span class="sxs-lookup"><span data-stu-id="c4be8-323">Open the **ContactController.cs** file.</span></span>
2. <span data-ttu-id="c4be8-324">Adicione um novo método à classe do controlador nomeado **Post** conforme mostrado no código a seguir.</span><span class="sxs-lookup"><span data-stu-id="c4be8-324">Add a new method to the controller class named **Post** as shown in the following code.</span></span>

    <span data-ttu-id="c4be8-325">(Código de trecho de código – *laboratório API - Ex03 - Post método Web*)</span><span class="sxs-lookup"><span data-stu-id="c4be8-325">(Code Snippet - *Web API Lab - Ex03 - Post Method*)</span></span>

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. <span data-ttu-id="c4be8-326">Abra o **index. cshtml** arquivo no Visual Studio, caso ele não ainda estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="c4be8-326">Open the **Index.cshtml** file in Visual Studio if it is not already open.</span></span>
4. <span data-ttu-id="c4be8-327">Adicione o código HTML a seguir ao arquivo logo após a lista não ordenada que você adicionou na tarefa anterior.</span><span class="sxs-lookup"><span data-stu-id="c4be8-327">Add the HTML code below to the file just after the unordered list you added in the previous task.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. <span data-ttu-id="c4be8-328">Dentro do elemento de script na parte inferior do documento, adicione o seguinte código realçado para manipular eventos de clique de botão, que postará os dados para a API Web usando uma chamada HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="c4be8-328">Within the script element at the bottom of the document, add the following highlighted code to handle button-click events, which will post the data to the Web API using an HTTP POST call.</span></span>

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. <span data-ttu-id="c4be8-329">Na **ContactController.cs**, coloque um ponto de interrupção na **Post** método.</span><span class="sxs-lookup"><span data-stu-id="c4be8-329">In **ContactController.cs**, place a breakpoint on the **Post** method.</span></span>
7. <span data-ttu-id="c4be8-330">Pressione **F5** para executar o aplicativo no navegador.</span><span class="sxs-lookup"><span data-stu-id="c4be8-330">Press **F5** to run the application in the browser.</span></span>
8. <span data-ttu-id="c4be8-331">Depois que a página é carregada no navegador, digite um novo nome de contato e Id e clique no **salvar** botão.</span><span class="sxs-lookup"><span data-stu-id="c4be8-331">Once the page is loaded in the browser, type in a new contact name and Id and click the **Save** button.</span></span>

    <span data-ttu-id="c4be8-332">![O documento de cliente HTML carregado no navegador](build-restful-apis-with-aspnet-web-api/_static/image24.png "o documento de cliente HTML carregado no navegador")</span><span class="sxs-lookup"><span data-stu-id="c4be8-332">![The client HTML document loaded in the browser](build-restful-apis-with-aspnet-web-api/_static/image24.png "The client HTML document loaded in the browser")</span></span>

    *<span data-ttu-id="c4be8-333">O documento HTML do cliente carregada no navegador</span><span class="sxs-lookup"><span data-stu-id="c4be8-333">The client HTML document loaded in the browser</span></span>*
9. <span data-ttu-id="c4be8-334">Quando a janela do depurador interrompe **Post** método, examine as propriedades da **entre em contato com** parâmetro.</span><span class="sxs-lookup"><span data-stu-id="c4be8-334">When the debugger window breaks in the **Post** method, take a look at the properties of the **contact** parameter.</span></span> <span data-ttu-id="c4be8-335">Os valores devem corresponder os dados inseridos no formulário.</span><span class="sxs-lookup"><span data-stu-id="c4be8-335">The values should match the data you entered in the form.</span></span>

    <span data-ttu-id="c4be8-336">![O objeto de contato que está sendo enviado para a API da Web do cliente](build-restful-apis-with-aspnet-web-api/_static/image25.png "objeto entre em contato com o que está sendo enviado para a API da Web do cliente")</span><span class="sxs-lookup"><span data-stu-id="c4be8-336">![The Contact object being sent to the Web API from the client](build-restful-apis-with-aspnet-web-api/_static/image25.png "The Contact object being sent to the Web API from the client")</span></span>

    *<span data-ttu-id="c4be8-337">O objeto de contato que está sendo enviado para a API da Web do cliente</span><span class="sxs-lookup"><span data-stu-id="c4be8-337">The Contact object being sent to the Web API from the client</span></span>*
10. <span data-ttu-id="c4be8-338">Etapa por meio do método no depurador até que o **resposta** variável foi criada.</span><span class="sxs-lookup"><span data-stu-id="c4be8-338">Step through the method in the debugger until the **response** variable has been created.</span></span> <span data-ttu-id="c4be8-339">Após a inspeção na **Locals** janela no depurador, você verá que todas as propriedades foram definidas.</span><span class="sxs-lookup"><span data-stu-id="c4be8-339">Upon inspection in the **Locals** window in the debugger, you'll see that all the properties have been set.</span></span>

   <span data-ttu-id="c4be8-340">![A resposta após a criação no depurador](build-restful-apis-with-aspnet-web-api/_static/image26.png "a resposta após a criação no depurador")</span><span class="sxs-lookup"><span data-stu-id="c4be8-340">![The response following creation in the debugger](build-restful-apis-with-aspnet-web-api/_static/image26.png "The response following creation in the debugger")</span></span>

   *<span data-ttu-id="c4be8-341">A resposta após a criação no depurador</span><span class="sxs-lookup"><span data-stu-id="c4be8-341">The response following creation in the debugger</span></span>*
11. <span data-ttu-id="c4be8-342">Se você pressionar **F5** ou clique em **continuar** no depurador, a solicitação será concluída.</span><span class="sxs-lookup"><span data-stu-id="c4be8-342">If you press **F5** or click **Continue** in the debugger the request will complete.</span></span> <span data-ttu-id="c4be8-343">Depois que você alternar de volta para o navegador, o novo contato foi adicionado à lista de contatos armazenados pelo **ContactRepository** implementação.</span><span class="sxs-lookup"><span data-stu-id="c4be8-343">Once you switch back to the browser, the new contact has been added to the list of contacts stored by the **ContactRepository** implementation.</span></span>

   <span data-ttu-id="c4be8-344">![O navegador reflete a criação bem-sucedida da nova instância de contato](build-restful-apis-with-aspnet-web-api/_static/image27.png "navegador reflete a criação bem-sucedida da nova instância de contato")</span><span class="sxs-lookup"><span data-stu-id="c4be8-344">![The browser reflects successful creation of the new contact instance](build-restful-apis-with-aspnet-web-api/_static/image27.png "The browser reflects successful creation of the new contact instance")</span></span>

   *<span data-ttu-id="c4be8-345">O navegador reflete a criação bem-sucedida da nova instância de contato</span><span class="sxs-lookup"><span data-stu-id="c4be8-345">The browser reflects successful creation of the new contact instance</span></span>*

> [!NOTE]
> <span data-ttu-id="c4be8-346">Além disso, você pode implantar esse aplicativo do Azure seguinte [apêndice c: Publicando um aplicativo ASP.NET MVC 4 usando a implantação da Web](#AppendixC).</span><span class="sxs-lookup"><span data-stu-id="c4be8-346">Additionally, you can deploy this application to Azure following [Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixC).</span></span>


---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="c4be8-347">Resumo</span><span class="sxs-lookup"><span data-stu-id="c4be8-347">Summary</span></span>

<span data-ttu-id="c4be8-348">Este laboratório tem apresentou a você para a nova estrutura de API Web ASP.NET e a implementação de APIs da Web RESTful, usando a estrutura.</span><span class="sxs-lookup"><span data-stu-id="c4be8-348">This lab has introduced you to the new ASP.NET Web API framework and to the implementation of RESTful Web APIs using the framework.</span></span> <span data-ttu-id="c4be8-349">A partir daqui, você pode criar um novo repositório que facilita a persistência de dados usando qualquer número de mecanismos e conectar esse serviço em vez do simples fornecido como um exemplo neste laboratório.</span><span class="sxs-lookup"><span data-stu-id="c4be8-349">From here, you could create a new repository that facilitates data persistence using any number of mechanisms and wire that service up rather than the simple one provided as an example in this lab.</span></span> <span data-ttu-id="c4be8-350">API da Web dá suporte a um número de recursos adicionais, como habilitar a comunicação de clientes não-HTML escritos em qualquer linguagem que dá suporte a HTTP e JSON ou XML.</span><span class="sxs-lookup"><span data-stu-id="c4be8-350">Web API supports a number of additional features, such as enabling communication from non-HTML clients written in any language that supports HTTP and JSON or XML.</span></span> <span data-ttu-id="c4be8-351">A capacidade de hospedar uma API da Web fora de um aplicativo web típico também é possível, bem como é a capacidade de criar seus próprios formatos de serialização.</span><span class="sxs-lookup"><span data-stu-id="c4be8-351">The ability to host a Web API outside of a typical web application is also possible, as well as is the ability to create your own serialization formats.</span></span>

<span data-ttu-id="c4be8-352">O site da Web do ASP.NET tem uma área dedicada a estrutura de API Web ASP.NET no [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api).</span><span class="sxs-lookup"><span data-stu-id="c4be8-352">The ASP.NET Web site has an area dedicated to the ASP.NET Web API framework at [[https://asp.net/web-api](https://asp.net/web-api)](https://asp.net/web-api).</span></span> <span data-ttu-id="c4be8-353">Este site continuará fornecer informações mais recentes, exemplos e notícias relacionadas à API da Web, portanto, verifique-lo com frequência se você gostaria de nos aprofundar na arte de criação de APIs Web personalizadas disponíveis para praticamente qualquer estrutura de desenvolvimento ou dispositivo.</span><span class="sxs-lookup"><span data-stu-id="c4be8-353">This site will continue to provide late-breaking information, samples, and news related to Web API, so check it frequently if you'd like to delve deeper into the art of creating custom Web APIs available to virtually any device or development framework.</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="c4be8-354">Apêndice a: Usar trechos de código</span><span class="sxs-lookup"><span data-stu-id="c4be8-354">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="c4be8-355">Com trechos de código, você tem todo o código que necessário ao seu alcance.</span><span class="sxs-lookup"><span data-stu-id="c4be8-355">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="c4be8-356">O documento de laboratório lhe dirá exatamente quando você pode usá-los, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="c4be8-356">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="c4be8-357">![Usar trechos de código do Visual Studio para inserir código em seu projeto](build-restful-apis-with-aspnet-web-api/_static/image28.png "trechos de código usando o Visual Studio para inserir código em seu projeto")</span><span class="sxs-lookup"><span data-stu-id="c4be8-357">![Using Visual Studio code snippets to insert code into your project](build-restful-apis-with-aspnet-web-api/_static/image28.png "Using Visual Studio code snippets to insert code into your project")</span></span>

*<span data-ttu-id="c4be8-358">Usar trechos de código do Visual Studio para inserir código em seu projeto</span><span class="sxs-lookup"><span data-stu-id="c4be8-358">Using Visual Studio code snippets to insert code into your project</span></span>*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a><span data-ttu-id="c4be8-359">Para adicionar um trecho de código usando o teclado (somente c#)</span><span class="sxs-lookup"><span data-stu-id="c4be8-359">To add a code snippet using the keyboard (C# only)</span></span>

1. <span data-ttu-id="c4be8-360">Coloque o cursor onde você deseja inserir o código.</span><span class="sxs-lookup"><span data-stu-id="c4be8-360">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="c4be8-361">Comece a digitar o nome do trecho de código (sem espaços ou hifens).</span><span class="sxs-lookup"><span data-stu-id="c4be8-361">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="c4be8-362">Assista como o IntelliSense exibe os nomes dos trechos de código correspondentes.</span><span class="sxs-lookup"><span data-stu-id="c4be8-362">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="c4be8-363">Selecione o trecho de código correto (ou continue digitando até que o nome do trecho toda está selecionado).</span><span class="sxs-lookup"><span data-stu-id="c4be8-363">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="c4be8-364">Pressione a tecla Tab duas vezes para inserir o trecho de código no local do cursor.</span><span class="sxs-lookup"><span data-stu-id="c4be8-364">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

    <span data-ttu-id="c4be8-365">![Comece a digitar o nome do trecho](build-restful-apis-with-aspnet-web-api/_static/image29.png "comece a digitar o nome do trecho de código")</span><span class="sxs-lookup"><span data-stu-id="c4be8-365">![Start typing the snippet name](build-restful-apis-with-aspnet-web-api/_static/image29.png "Start typing the snippet name")</span></span>

    *<span data-ttu-id="c4be8-366">Comece a digitar o nome do trecho de código</span><span class="sxs-lookup"><span data-stu-id="c4be8-366">Start typing the snippet name</span></span>*

    <span data-ttu-id="c4be8-367">![Pressione Tab para selecionar o trecho de código realçado](build-restful-apis-with-aspnet-web-api/_static/image30.png "pressione Tab para selecionar o trecho de código realçado")</span><span class="sxs-lookup"><span data-stu-id="c4be8-367">![Press Tab to select the highlighted snippet](build-restful-apis-with-aspnet-web-api/_static/image30.png "Press Tab to select the highlighted snippet")</span></span>

    *<span data-ttu-id="c4be8-368">Pressione Tab para selecionar o trecho de código realçado</span><span class="sxs-lookup"><span data-stu-id="c4be8-368">Press Tab to select the highlighted snippet</span></span>*

    <span data-ttu-id="c4be8-369">![Pressione Tab novamente e o trecho de código serão expandido](build-restful-apis-with-aspnet-web-api/_static/image31.png "pressione Tab novamente e o trecho de código serão expandido")</span><span class="sxs-lookup"><span data-stu-id="c4be8-369">![Press Tab again and the snippet will expand](build-restful-apis-with-aspnet-web-api/_static/image31.png "Press Tab again and the snippet will expand")</span></span>

    *<span data-ttu-id="c4be8-370">Pressione Tab novamente e o trecho de código serão expandido</span><span class="sxs-lookup"><span data-stu-id="c4be8-370">Press Tab again and the snippet will expand</span></span>*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a><span data-ttu-id="c4be8-371">Para adicionar um trecho de código usando o mouse (c#, Visual Basic e XML)</span><span class="sxs-lookup"><span data-stu-id="c4be8-371">To add a code snippet using the mouse (C#, Visual Basic and XML)</span></span>

1. <span data-ttu-id="c4be8-372">Clique com botão direito no qual você deseja inserir o trecho de código.</span><span class="sxs-lookup"><span data-stu-id="c4be8-372">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="c4be8-373">Selecione **Inserir trecho** seguido **Meus trechos de código**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-373">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="c4be8-374">Selecione o trecho relevante na lista, clicando nele.</span><span class="sxs-lookup"><span data-stu-id="c4be8-374">Pick the relevant snippet from the list, by clicking on it.</span></span>

    <span data-ttu-id="c4be8-375">![Clique com botão direito no qual você deseja inserir o trecho de código e selecione Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "botão direito do mouse em que você deseja inserir o trecho de código e selecione Insert Snippet")</span><span class="sxs-lookup"><span data-stu-id="c4be8-375">![Right-click where you want to insert the code snippet and select Insert Snippet](build-restful-apis-with-aspnet-web-api/_static/image32.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

    *<span data-ttu-id="c4be8-376">Clique com botão direito no qual você deseja inserir o trecho de código e selecione Insert Snippet</span><span class="sxs-lookup"><span data-stu-id="c4be8-376">Right-click where you want to insert the code snippet and select Insert Snippet</span></span>*

    <span data-ttu-id="c4be8-377">![Escolher o trecho relevante na lista, clicando nele](build-restful-apis-with-aspnet-web-api/_static/image33.png "escolher o trecho relevante na lista, clicando nele")</span><span class="sxs-lookup"><span data-stu-id="c4be8-377">![Pick the relevant snippet from the list, by clicking on it](build-restful-apis-with-aspnet-web-api/_static/image33.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

    *<span data-ttu-id="c4be8-378">Escolher o trecho relevante na lista, clicando nele</span><span class="sxs-lookup"><span data-stu-id="c4be8-378">Pick the relevant snippet from the list, by clicking on it</span></span>*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="c4be8-379">Apêndice b: Instalar o Visual Studio Express 2012 para Web</span><span class="sxs-lookup"><span data-stu-id="c4be8-379">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="c4be8-380">Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outra &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-380">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="c4be8-381">As instruções a seguir guia você pelas etapas necessárias para instalar *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.</span><span class="sxs-lookup"><span data-stu-id="c4be8-381">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="c4be8-382">Vá para [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="c4be8-382">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="c4be8-383">Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-lo e pesquisar o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Azure</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="c4be8-383">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="c4be8-384">Clique em **instalar agora**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-384">Click on **Install Now**.</span></span> <span data-ttu-id="c4be8-385">Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.</span><span class="sxs-lookup"><span data-stu-id="c4be8-385">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="c4be8-386">Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.</span><span class="sxs-lookup"><span data-stu-id="c4be8-386">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="c4be8-387">![Instalar o Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "instalar o Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="c4be8-387">![Install Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "Install Visual Studio Express")</span></span>

    *<span data-ttu-id="c4be8-388">Instalar o Visual Studio Express</span><span class="sxs-lookup"><span data-stu-id="c4be8-388">Install Visual Studio Express</span></span>*
4. <span data-ttu-id="c4be8-389">Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.</span><span class="sxs-lookup"><span data-stu-id="c4be8-389">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Aceitar os termos de licença](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *<span data-ttu-id="c4be8-391">Aceitar os termos de licença</span><span class="sxs-lookup"><span data-stu-id="c4be8-391">Accepting the license terms</span></span>*
5. <span data-ttu-id="c4be8-392">Aguarde até que o processo de download e instalação for concluído.</span><span class="sxs-lookup"><span data-stu-id="c4be8-392">Wait until the downloading and installation process completes.</span></span>

    ![Progresso da instalação](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *<span data-ttu-id="c4be8-394">Progresso da instalação</span><span class="sxs-lookup"><span data-stu-id="c4be8-394">Installation progress</span></span>*
6. <span data-ttu-id="c4be8-395">Quando a instalação for concluída, clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-395">When the installation completes, click **Finish**.</span></span>

    ![Instalação concluída](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *<span data-ttu-id="c4be8-397">Instalação concluída</span><span class="sxs-lookup"><span data-stu-id="c4be8-397">Installation completed</span></span>*
7. <span data-ttu-id="c4be8-398">Clique em **Exit** para fechar o Web Platform Installer.</span><span class="sxs-lookup"><span data-stu-id="c4be8-398">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="c4be8-399">Para abrir o Visual Studio Express para Web, vá para o **iniciar** de tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.</span><span class="sxs-lookup"><span data-stu-id="c4be8-399">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express para o bloco da Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *<span data-ttu-id="c4be8-401">VS Express para o bloco da Web</span><span class="sxs-lookup"><span data-stu-id="c4be8-401">VS Express for Web tile</span></span>*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c4be8-402">Apêndice c: Publicando um aplicativo ASP.NET MVC 4 usando a implantação da Web</span><span class="sxs-lookup"><span data-stu-id="c4be8-402">Appendix C: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="c4be8-403">Este apêndice mostram como criar um novo site do Portal do Azure e publicar o aplicativo que você obteve seguindo o ambiente de laboratório, aproveitando o recurso de publicação de implantação da Web fornecido pelo Azure.</span><span class="sxs-lookup"><span data-stu-id="c4be8-403">This appendix will show you how to create a new web site from the Azure Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Azure.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a><span data-ttu-id="c4be8-404">Tarefa 1 - criar um novo Site do Portal do Azure</span><span class="sxs-lookup"><span data-stu-id="c4be8-404">Task 1 - Creating a New Web Site from the Azure Portal</span></span>

1. <span data-ttu-id="c4be8-405">Vá para o [Portal de gerenciamento](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.</span><span class="sxs-lookup"><span data-stu-id="c4be8-405">Go to the [Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c4be8-406">Com o Azure, você pode hospedar 10 Sites da Web ASP.NET gratuitamente e, em seguida, dimensione conforme seu tráfego aumenta.</span><span class="sxs-lookup"><span data-stu-id="c4be8-406">With Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="c4be8-407">Você pode se inscrever [aqui](https://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="c4be8-407">You can sign up [here](https://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="c4be8-408">![Faça logon no portal do Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "faça logon no portal do Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="c4be8-408">![Log on to Windows Azure portal](build-restful-apis-with-aspnet-web-api/_static/image39.png "Log on to Windows Azure portal")</span></span>

    *<span data-ttu-id="c4be8-409">Faça logon no Portal</span><span class="sxs-lookup"><span data-stu-id="c4be8-409">Log on to Portal</span></span>*
2. <span data-ttu-id="c4be8-410">Clique em **New** na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="c4be8-410">Click **New** on the command bar.</span></span>

    <span data-ttu-id="c4be8-411">![Criando um novo Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "criando um novo Site")</span><span class="sxs-lookup"><span data-stu-id="c4be8-411">![Creating a new Web Site](build-restful-apis-with-aspnet-web-api/_static/image40.png "Creating a new Web Site")</span></span>

    *<span data-ttu-id="c4be8-412">Criando um novo Site</span><span class="sxs-lookup"><span data-stu-id="c4be8-412">Creating a new Web Site</span></span>*
3. <span data-ttu-id="c4be8-413">Clique em **Compute** | **Site da Web**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-413">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="c4be8-414">Em seguida, selecione **criação rápida** opção.</span><span class="sxs-lookup"><span data-stu-id="c4be8-414">Then select **Quick Create** option.</span></span> <span data-ttu-id="c4be8-415">Forneça uma URL disponível para o novo site da web e clique em **Criar Site**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-415">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c4be8-416">O Azure é o host para um aplicativo web em execução na nuvem que você pode controlar e gerenciar.</span><span class="sxs-lookup"><span data-stu-id="c4be8-416">Azure is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="c4be8-417">A opção criação rápida permite que você implante um aplicativo web concluído no Azure de fora do portal.</span><span class="sxs-lookup"><span data-stu-id="c4be8-417">The Quick Create option allows you to deploy a completed web application to the Azure from outside the portal.</span></span> <span data-ttu-id="c4be8-418">Ele não inclui as etapas para configurar um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c4be8-418">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="c4be8-419">![Criando um novo Site usando a criação rápida](build-restful-apis-with-aspnet-web-api/_static/image41.png "criando um novo Site usando a criação rápida")</span><span class="sxs-lookup"><span data-stu-id="c4be8-419">![Creating a new Web Site using Quick Create](build-restful-apis-with-aspnet-web-api/_static/image41.png "Creating a new Web Site using Quick Create")</span></span>

    *<span data-ttu-id="c4be8-420">Criando um novo Site usando a criação rápida</span><span class="sxs-lookup"><span data-stu-id="c4be8-420">Creating a new Web Site using Quick Create</span></span>*
4. <span data-ttu-id="c4be8-421">Aguarde até que o novo **Site da Web** é criado.</span><span class="sxs-lookup"><span data-stu-id="c4be8-421">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="c4be8-422">Depois que o Site da Web é criado, clique no link sob a **URL** coluna.</span><span class="sxs-lookup"><span data-stu-id="c4be8-422">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="c4be8-423">Verifique se o novo Site da Web está funcionando.</span><span class="sxs-lookup"><span data-stu-id="c4be8-423">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="c4be8-424">![Navegando até o novo site](build-restful-apis-with-aspnet-web-api/_static/image42.png "navegando até o novo site da web")</span><span class="sxs-lookup"><span data-stu-id="c4be8-424">![Browsing to the new web site](build-restful-apis-with-aspnet-web-api/_static/image42.png "Browsing to the new web site")</span></span>

    *<span data-ttu-id="c4be8-425">Navegando até o novo site da web</span><span class="sxs-lookup"><span data-stu-id="c4be8-425">Browsing to the new web site</span></span>*

    <span data-ttu-id="c4be8-426">![Site da Web em execução](build-restful-apis-with-aspnet-web-api/_static/image43.png "site da Web em execução")</span><span class="sxs-lookup"><span data-stu-id="c4be8-426">![Web site running](build-restful-apis-with-aspnet-web-api/_static/image43.png "Web site running")</span></span>

    *<span data-ttu-id="c4be8-427">Site da Web em execução</span><span class="sxs-lookup"><span data-stu-id="c4be8-427">Web site running</span></span>*
6. <span data-ttu-id="c4be8-428">Volte para o portal e clique no nome do site da web sob o **nome** coluna para exibir as páginas de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="c4be8-428">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="c4be8-429">![Abrir as páginas de gerenciamento do site da web](build-restful-apis-with-aspnet-web-api/_static/image44.png "abrir as páginas de gerenciamento do site da web")</span><span class="sxs-lookup"><span data-stu-id="c4be8-429">![Opening the web site management pages](build-restful-apis-with-aspnet-web-api/_static/image44.png "Opening the web site management pages")</span></span>

    *<span data-ttu-id="c4be8-430">Abrir as páginas de gerenciamento do Site da Web</span><span class="sxs-lookup"><span data-stu-id="c4be8-430">Opening the Web Site management pages</span></span>*
7. <span data-ttu-id="c4be8-431">No **Dashboard** página na **visão geral rápida** seção, clique no **baixar perfil de publicação** link.</span><span class="sxs-lookup"><span data-stu-id="c4be8-431">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c4be8-432">O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo web para um Azure para cada método de publicação habilitado.</span><span class="sxs-lookup"><span data-stu-id="c4be8-432">The *publish profile* contains all of the information required to publish a web application to a Azure for each enabled publication method.</span></span> <span data-ttu-id="c4be8-433">O perfil de publicação contém as URLs, as credenciais do usuário e cadeias de caracteres de banco de dados necessárias para conectar e autenticar em relação a cada um dos pontos de extremidade para o qual um método de publicação está habilitado.</span><span class="sxs-lookup"><span data-stu-id="c4be8-433">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="c4be8-434">**Microsoft WebMatrix 2**, **Microsoft Visual Studio para Web Express** e **Microsoft Visual Studio 2012** oferecem suporte à leitura perfis de publicação para automatizar a configuração desses programas para publicação de aplicativos web do Azure.</span><span class="sxs-lookup"><span data-stu-id="c4be8-434">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Azure.</span></span>

    <span data-ttu-id="c4be8-435">![Baixando o site da web de perfil de publicação](build-restful-apis-with-aspnet-web-api/_static/image45.png "baixando o site da web de perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="c4be8-435">![Downloading the web site publish profile](build-restful-apis-with-aspnet-web-api/_static/image45.png "Downloading the web site publish profile")</span></span>

    *<span data-ttu-id="c4be8-436">Baixando o Site da Web de perfil de publicação</span><span class="sxs-lookup"><span data-stu-id="c4be8-436">Downloading the Web Site publish profile</span></span>*
8. <span data-ttu-id="c4be8-437">Baixe o arquivo de perfil de publicação em um local conhecido.</span><span class="sxs-lookup"><span data-stu-id="c4be8-437">Download the publish profile file to a known location.</span></span> <span data-ttu-id="c4be8-438">Ainda mais neste exercício, você verá como usar esse arquivo para publicar um aplicativo web do Azure do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c4be8-438">Further in this exercise you will see how to use this file to publish a web application to Azure from Visual Studio.</span></span>

    <span data-ttu-id="c4be8-439">![Salvando o arquivo de perfil de publicação](build-restful-apis-with-aspnet-web-api/_static/image46.png "salvar o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="c4be8-439">![Saving the publish profile file](build-restful-apis-with-aspnet-web-api/_static/image46.png "Saving the publish profile")</span></span>

    *<span data-ttu-id="c4be8-440">Salvando o arquivo de perfil de publicação</span><span class="sxs-lookup"><span data-stu-id="c4be8-440">Saving the publish profile file</span></span>*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="c4be8-441">Tarefa 2 – Configurando o servidor de banco de dados</span><span class="sxs-lookup"><span data-stu-id="c4be8-441">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="c4be8-442">Se seu aplicativo faz uso do SQL Server você precisará criar um servidor de banco de dados SQL de bancos de dados.</span><span class="sxs-lookup"><span data-stu-id="c4be8-442">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="c4be8-443">Se você quiser implantar um aplicativo simples que não usa o SQL Server, você pode ignorar esta tarefa.</span><span class="sxs-lookup"><span data-stu-id="c4be8-443">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="c4be8-444">Você precisará de um servidor de banco de dados SQL para armazenar o banco de dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c4be8-444">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="c4be8-445">Você pode exibir os servidores de banco de dados SQL na sua assinatura no portal de gerenciamento do Azure em **bancos de dados Sql** | **servidores** | **painel do servidor**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-445">You can view the SQL Database servers from your subscription in the Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="c4be8-446">Se você não tiver um servidor criado, você pode criar uma usando o **adicionar** botão na barra de comandos.</span><span class="sxs-lookup"><span data-stu-id="c4be8-446">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="c4be8-447">Anote o **nome do servidor e o URL, o nome de logon de administrador e senha**, pois você usará nas próximas tarefas.</span><span class="sxs-lookup"><span data-stu-id="c4be8-447">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="c4be8-448">Não crie o banco de dados ainda, ele será criado em um estágio posterior.</span><span class="sxs-lookup"><span data-stu-id="c4be8-448">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="c4be8-449">![Painel do servidor de banco de dados SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "painel de banco de dados do SQL Server")</span><span class="sxs-lookup"><span data-stu-id="c4be8-449">![SQL Database Server Dashboard](build-restful-apis-with-aspnet-web-api/_static/image47.png "SQL Database Server Dashboard")</span></span>

    *<span data-ttu-id="c4be8-450">Painel de banco de dados do SQL Server</span><span class="sxs-lookup"><span data-stu-id="c4be8-450">SQL Database Server Dashboard</span></span>*
2. <span data-ttu-id="c4be8-451">A próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, você precisa incluir seu endereço IP local na lista do servidor **endereços IP permitidos**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-451">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="c4be8-452">Para fazer isso, clique em **configurar**, selecione o endereço IP do **endereço de IP do cliente atual** e cole-o na **endereço IP inicial** e **oendereçoIPfinal** caixas de texto e clique em de ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) botão.</span><span class="sxs-lookup"><span data-stu-id="c4be8-452">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) button.</span></span>

    ![Adicionar endereço IP do cliente](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *<span data-ttu-id="c4be8-454">Adicionar endereço IP do cliente</span><span class="sxs-lookup"><span data-stu-id="c4be8-454">Adding Client IP Address</span></span>*
3. <span data-ttu-id="c4be8-455">Uma vez a **endereço IP do cliente** é adicionado para endereços IP, clique em **salvar** para confirmar as alterações.</span><span class="sxs-lookup"><span data-stu-id="c4be8-455">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Confirmar alterações](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *<span data-ttu-id="c4be8-457">Confirmar alterações</span><span class="sxs-lookup"><span data-stu-id="c4be8-457">Confirm Changes</span></span>*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="c4be8-458">Tarefa 3 – publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web</span><span class="sxs-lookup"><span data-stu-id="c4be8-458">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="c4be8-459">Volte para a solução do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c4be8-459">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="c4be8-460">No **Gerenciador de soluções**, clique com botão direito no projeto de site da web e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-460">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="c4be8-461">![O aplicativo de publicação](build-restful-apis-with-aspnet-web-api/_static/image51.png "publicando o aplicativo")</span><span class="sxs-lookup"><span data-stu-id="c4be8-461">![Publishing the Application](build-restful-apis-with-aspnet-web-api/_static/image51.png "Publishing the Application")</span></span>

    *<span data-ttu-id="c4be8-462">Publicar o site da web</span><span class="sxs-lookup"><span data-stu-id="c4be8-462">Publishing the web site</span></span>*
2. <span data-ttu-id="c4be8-463">Importe o perfil de publicação que você salvou na primeira tarefa.</span><span class="sxs-lookup"><span data-stu-id="c4be8-463">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="c4be8-464">![Importando o perfil de publicação](build-restful-apis-with-aspnet-web-api/_static/image52.png "importando o perfil de publicação")</span><span class="sxs-lookup"><span data-stu-id="c4be8-464">![Importing the publish profile](build-restful-apis-with-aspnet-web-api/_static/image52.png "Importing the publish profile")</span></span>

    *<span data-ttu-id="c4be8-465">Importando o perfil de publicação</span><span class="sxs-lookup"><span data-stu-id="c4be8-465">Importing publish profile</span></span>*
3. <span data-ttu-id="c4be8-466">Clique em **validar Conexão**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-466">Click **Validate Connection**.</span></span> <span data-ttu-id="c4be8-467">Depois que a validação estiver concluída, clique em **próxima**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-467">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="c4be8-468">A validação é concluída quando você vir uma marca de seleção verde aparecer ao lado do botão Validar Conexão.</span><span class="sxs-lookup"><span data-stu-id="c4be8-468">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="c4be8-469">![Validar conexão](build-restful-apis-with-aspnet-web-api/_static/image53.png "validar conexão")</span><span class="sxs-lookup"><span data-stu-id="c4be8-469">![Validating connection](build-restful-apis-with-aspnet-web-api/_static/image53.png "Validating connection")</span></span>

    *<span data-ttu-id="c4be8-470">Validando a conexão</span><span class="sxs-lookup"><span data-stu-id="c4be8-470">Validating connection</span></span>*
4. <span data-ttu-id="c4be8-471">No **as configurações** página na **bancos de dados** seção, clique no botão ao lado da caixa de texto do sua conexão banco de dados (ou seja, **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="c4be8-471">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="c4be8-472">![Configuração de implantação da Web](build-restful-apis-with-aspnet-web-api/_static/image54.png "configuração de implantação da Web")</span><span class="sxs-lookup"><span data-stu-id="c4be8-472">![Web deploy configuration](build-restful-apis-with-aspnet-web-api/_static/image54.png "Web deploy configuration")</span></span>

    *<span data-ttu-id="c4be8-473">Configuração de implantação da Web</span><span class="sxs-lookup"><span data-stu-id="c4be8-473">Web deploy configuration</span></span>*
5. <span data-ttu-id="c4be8-474">Configure a conexão de banco de dados da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c4be8-474">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="c4be8-475">No **nome do servidor** digite sua URL de servidor de banco de dados SQL usando o *tcp:* prefixo.</span><span class="sxs-lookup"><span data-stu-id="c4be8-475">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="c4be8-476">Na **nome de usuário** digite seu nome de logon de administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="c4be8-476">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="c4be8-477">Na **senha** digite sua senha de logon de administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="c4be8-477">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="c4be8-478">Digite um novo nome de banco de dados, por exemplo: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="c4be8-478">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="c4be8-479">![Configurando a cadeia de caracteres de conexão de destino](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configurando a cadeia de caracteres de conexão de destino")</span><span class="sxs-lookup"><span data-stu-id="c4be8-479">![Configuring destination connection string](build-restful-apis-with-aspnet-web-api/_static/image55.png "Configuring destination connection string")</span></span>

     *<span data-ttu-id="c4be8-480">Configurando a cadeia de caracteres de conexão de destino</span><span class="sxs-lookup"><span data-stu-id="c4be8-480">Configuring destination connection string</span></span>*
6. <span data-ttu-id="c4be8-481">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-481">Then click **OK**.</span></span> <span data-ttu-id="c4be8-482">Quando for solicitado a criar o banco de dados, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-482">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="c4be8-483">![Criando o banco de dados](build-restful-apis-with-aspnet-web-api/_static/image56.png "criando a cadeia de caracteres de banco de dados")</span><span class="sxs-lookup"><span data-stu-id="c4be8-483">![Creating the database](build-restful-apis-with-aspnet-web-api/_static/image56.png "Creating the database string")</span></span>

    *<span data-ttu-id="c4be8-484">Criando o banco de dados</span><span class="sxs-lookup"><span data-stu-id="c4be8-484">Creating the database</span></span>*
7. <span data-ttu-id="c4be8-485">A cadeia de caracteres de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto de Conexão padrão.</span><span class="sxs-lookup"><span data-stu-id="c4be8-485">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="c4be8-486">Clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-486">Then click **Next**.</span></span>

    <span data-ttu-id="c4be8-487">![Cadeia de caracteres de Conexão que aponta para o banco de dados SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "cadeia de caracteres de Conexão que aponta para o banco de dados SQL")</span><span class="sxs-lookup"><span data-stu-id="c4be8-487">![Connection string pointing to SQL Database](build-restful-apis-with-aspnet-web-api/_static/image57.png "Connection string pointing to SQL Database")</span></span>

    *<span data-ttu-id="c4be8-488">Cadeia de caracteres de Conexão que aponta para o banco de dados SQL</span><span class="sxs-lookup"><span data-stu-id="c4be8-488">Connection string pointing to SQL Database</span></span>*
8. <span data-ttu-id="c4be8-489">No **versão prévia** , clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="c4be8-489">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="c4be8-490">![Publicando o aplicativo web](build-restful-apis-with-aspnet-web-api/_static/image58.png "publicando o aplicativo web")</span><span class="sxs-lookup"><span data-stu-id="c4be8-490">![Publishing the web application](build-restful-apis-with-aspnet-web-api/_static/image58.png "Publishing the web application")</span></span>

    *<span data-ttu-id="c4be8-491">Publicar o aplicativo da web</span><span class="sxs-lookup"><span data-stu-id="c4be8-491">Publishing the web application</span></span>*
9. <span data-ttu-id="c4be8-492">Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.</span><span class="sxs-lookup"><span data-stu-id="c4be8-492">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="c4be8-493">![Aplicativo publicado no Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "aplicativo publicado no Windows Azure")</span><span class="sxs-lookup"><span data-stu-id="c4be8-493">![Application published to Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "Application published to Windows Azure")</span></span>

    *<span data-ttu-id="c4be8-494">Aplicativo publicado no Azure</span><span class="sxs-lookup"><span data-stu-id="c4be8-494">Application published to Azure</span></span>*
