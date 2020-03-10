---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Simulação de Entity Framework quando o teste de unidade ASP.NET Web API 2 | Microsoft Docs
author: Rick-Anderson
description: Este guia e aplicativo demonstram como criar testes de unidade para seu aplicativo Web API 2 que usa o Entity Framework. Ele mostra como modificar o...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 258450107ee7443c4efd43a3b8e4851249745227
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555086"
---
# <a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="e7206-104">Simulação de Entity Framework quando o teste de unidade ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="e7206-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>

<span data-ttu-id="e7206-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e7206-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="e7206-106">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="e7206-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="e7206-107">Este guia e aplicativo demonstram como criar testes de unidade para seu aplicativo Web API 2 que usa o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e7206-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="e7206-108">Ele mostra como modificar o controlador com Scaffold para habilitar a passagem de um objeto de contexto para teste e como criar objetos de teste que funcionam com Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e7206-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
>
> <span data-ttu-id="e7206-109">Para obter uma introdução ao teste de unidade com ASP.NET Web API, consulte [testes de unidade com ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e7206-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
>
> <span data-ttu-id="e7206-110">Este tutorial pressupõe que você esteja familiarizado com os conceitos básicos do ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="e7206-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="e7206-111">Para obter um tutorial introdutório, consulte [introdução com ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e7206-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e7206-112">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="e7206-112">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="e7206-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="e7206-113">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="e7206-114">API Web 2</span><span class="sxs-lookup"><span data-stu-id="e7206-114">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="e7206-115">Neste tópico</span><span class="sxs-lookup"><span data-stu-id="e7206-115">In this topic</span></span>

<span data-ttu-id="e7206-116">Este tópico contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="e7206-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="e7206-117">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="e7206-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="e7206-118">Código de download</span><span class="sxs-lookup"><span data-stu-id="e7206-118">Download code</span></span>](#download)
- [<span data-ttu-id="e7206-119">Criar aplicativo com projeto de teste de unidade</span><span class="sxs-lookup"><span data-stu-id="e7206-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="e7206-120">Criar a classe de modelo</span><span class="sxs-lookup"><span data-stu-id="e7206-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="e7206-121">Adicionar controlador</span><span class="sxs-lookup"><span data-stu-id="e7206-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="e7206-122">Adicionar injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="e7206-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="e7206-123">Instalar pacotes NuGet no projeto de teste</span><span class="sxs-lookup"><span data-stu-id="e7206-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="e7206-124">Criar contexto de teste</span><span class="sxs-lookup"><span data-stu-id="e7206-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="e7206-125">Criar testes</span><span class="sxs-lookup"><span data-stu-id="e7206-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="e7206-126">Executar testes</span><span class="sxs-lookup"><span data-stu-id="e7206-126">Run tests</span></span>](#runtests)

<span data-ttu-id="e7206-127">Se você já tiver concluído as etapas em [testes de unidade com ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), poderá pular para a seção [Adicionar o controlador](#controller).</span><span class="sxs-lookup"><span data-stu-id="e7206-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="e7206-128">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="e7206-128">Prerequisites</span></span>

<span data-ttu-id="e7206-129">Visual Studio 2017 Community, Professional ou Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="e7206-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="e7206-130">Código de download</span><span class="sxs-lookup"><span data-stu-id="e7206-130">Download code</span></span>

<span data-ttu-id="e7206-131">Baixe o [projeto concluído](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="e7206-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="e7206-132">O projeto que pode ser baixado inclui o código de teste de unidade para este tópico e para o tópico [teste de unidade ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="e7206-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="e7206-133">Criar aplicativo com projeto de teste de unidade</span><span class="sxs-lookup"><span data-stu-id="e7206-133">Create application with unit test project</span></span>

<span data-ttu-id="e7206-134">Você pode criar um projeto de teste de unidade ao criar seu aplicativo ou adicionar um projeto de teste de unidade a um aplicativo existente.</span><span class="sxs-lookup"><span data-stu-id="e7206-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="e7206-135">Este tutorial mostra como criar um projeto de teste de unidade ao criar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e7206-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="e7206-136">Crie um novo aplicativo Web ASP.NET chamado **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="e7206-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="e7206-137">Nas janelas do novo projeto ASP.NET, selecione o modelo **vazio** e adicione as pastas e as referências principais para a API da Web.</span><span class="sxs-lookup"><span data-stu-id="e7206-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="e7206-138">Selecione a opção **Adicionar testes de unidade** .</span><span class="sxs-lookup"><span data-stu-id="e7206-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="e7206-139">O projeto de teste de unidade é chamado automaticamente de **StoreApp. Tests**.</span><span class="sxs-lookup"><span data-stu-id="e7206-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="e7206-140">Você pode manter esse nome.</span><span class="sxs-lookup"><span data-stu-id="e7206-140">You can keep this name.</span></span>

![Criar projeto de teste de unidade](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="e7206-142">Depois de criar o aplicativo, você verá que ele contém dois projetos- **StoreApp** e **StoreApp. Tests**.</span><span class="sxs-lookup"><span data-stu-id="e7206-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="e7206-143">Criar a classe de modelo</span><span class="sxs-lookup"><span data-stu-id="e7206-143">Create the model class</span></span>

<span data-ttu-id="e7206-144">Em seu projeto StoreApp, adicione um arquivo de classe à pasta **modelos** chamada **Product.cs**.</span><span class="sxs-lookup"><span data-stu-id="e7206-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="e7206-145">Substitua o conteúdo do arquivo pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e7206-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="e7206-146">Compile a solução.</span><span class="sxs-lookup"><span data-stu-id="e7206-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="e7206-147">Adicionar controlador</span><span class="sxs-lookup"><span data-stu-id="e7206-147">Add the controller</span></span>

<span data-ttu-id="e7206-148">Clique com o botão direito do mouse na pasta controladores e selecione **Adicionar** e **novo item com Scaffold**.</span><span class="sxs-lookup"><span data-stu-id="e7206-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="e7206-149">Selecione controlador Web API 2 com ações, usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e7206-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![Adicionar novo controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="e7206-151">Defina os seguintes valores:</span><span class="sxs-lookup"><span data-stu-id="e7206-151">Set the following values:</span></span>

- <span data-ttu-id="e7206-152">Nome do controlador: **ProductController**</span><span class="sxs-lookup"><span data-stu-id="e7206-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="e7206-153">Classe de modelo: **produto**</span><span class="sxs-lookup"><span data-stu-id="e7206-153">Model class: **Product**</span></span>
- <span data-ttu-id="e7206-154">Classe de contexto de dados: [botão Selecionar **novo contexto de dados** que preenche os valores exibidos abaixo]</span><span class="sxs-lookup"><span data-stu-id="e7206-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![especificar controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="e7206-156">Clique em **Adicionar** para criar o controlador com o código gerado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="e7206-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="e7206-157">O código inclui métodos para criar, recuperar, atualizar e excluir instâncias da classe Product.</span><span class="sxs-lookup"><span data-stu-id="e7206-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="e7206-158">O código a seguir mostra o método para adicionar um produto.</span><span class="sxs-lookup"><span data-stu-id="e7206-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="e7206-159">Observe que o método retorna uma instância de **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="e7206-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="e7206-160">O IHttpActionResult é um dos novos recursos da API Web 2 e simplifica o desenvolvimento de testes de unidade.</span><span class="sxs-lookup"><span data-stu-id="e7206-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="e7206-161">Na próxima seção, você personalizará o código gerado para facilitar a passagem de objetos de teste para o controlador.</span><span class="sxs-lookup"><span data-stu-id="e7206-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="e7206-162">Adicionar injeção de dependência</span><span class="sxs-lookup"><span data-stu-id="e7206-162">Add dependency injection</span></span>

<span data-ttu-id="e7206-163">Atualmente, a classe ProductController é embutida em código para usar uma instância da classe StoreAppContext.</span><span class="sxs-lookup"><span data-stu-id="e7206-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="e7206-164">Você usará um padrão chamado injeção de dependência para modificar seu aplicativo e remover essa dependência embutida em código.</span><span class="sxs-lookup"><span data-stu-id="e7206-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="e7206-165">Ao dividir essa dependência, você pode passar um objeto fictício durante o teste.</span><span class="sxs-lookup"><span data-stu-id="e7206-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="e7206-166">Clique com o botão direito do mouse na pasta **modelos** e adicione uma nova interface chamada **IStoreAppContext**.</span><span class="sxs-lookup"><span data-stu-id="e7206-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="e7206-167">Substitua o código pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e7206-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="e7206-168">Abra o arquivo StoreAppContext.cs e faça as seguintes alterações realçadas.</span><span class="sxs-lookup"><span data-stu-id="e7206-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="e7206-169">As alterações importantes a serem observadas são:</span><span class="sxs-lookup"><span data-stu-id="e7206-169">The important changes to note are:</span></span>

- <span data-ttu-id="e7206-170">Classe StoreAppContext implementa a interface IStoreAppContext</span><span class="sxs-lookup"><span data-stu-id="e7206-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="e7206-171">O método MarkAsModified é implementado</span><span class="sxs-lookup"><span data-stu-id="e7206-171">MarkAsModified method is implemented</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="e7206-172">Abra o arquivo ProductController.cs.</span><span class="sxs-lookup"><span data-stu-id="e7206-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="e7206-173">Altere o código existente para corresponder ao código realçado.</span><span class="sxs-lookup"><span data-stu-id="e7206-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="e7206-174">Essas alterações quebram a dependência em StoreAppContext e permitem que outras classes passem em um objeto diferente para a classe de contexto.</span><span class="sxs-lookup"><span data-stu-id="e7206-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="e7206-175">Essa alteração permitirá que você passe um contexto de teste durante os testes de unidade.</span><span class="sxs-lookup"><span data-stu-id="e7206-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="e7206-176">Há mais uma alteração que você deve fazer em ProductController.</span><span class="sxs-lookup"><span data-stu-id="e7206-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="e7206-177">No método **PutProduct** , substitua a linha que define o estado da entidade como modificado com uma chamada para o método MarkAsModified.</span><span class="sxs-lookup"><span data-stu-id="e7206-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="e7206-178">Compile a solução.</span><span class="sxs-lookup"><span data-stu-id="e7206-178">Build the solution.</span></span>

<span data-ttu-id="e7206-179">Agora você está pronto para configurar o projeto de teste.</span><span class="sxs-lookup"><span data-stu-id="e7206-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="e7206-180">Instalar pacotes NuGet no projeto de teste</span><span class="sxs-lookup"><span data-stu-id="e7206-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="e7206-181">Quando você usa o modelo vazio para criar um aplicativo, o projeto de teste de unidade (StoreApp. Tests) não inclui nenhum pacote NuGet instalado.</span><span class="sxs-lookup"><span data-stu-id="e7206-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="e7206-182">Outros modelos, como o modelo de API Web, incluem alguns pacotes NuGet no projeto de teste de unidade.</span><span class="sxs-lookup"><span data-stu-id="e7206-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="e7206-183">Para este tutorial, você deve incluir o pacote de Entity Framework e o pacote de Microsoft ASP.NET Web API 2 core para o projeto de teste.</span><span class="sxs-lookup"><span data-stu-id="e7206-183">For this tutorial, you must include the Entity Framework package and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="e7206-184">Clique com o botão direito do mouse no projeto StoreApp. Tests e selecione **gerenciar pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="e7206-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="e7206-185">Você deve selecionar o projeto StoreApp. Tests para adicionar os pacotes a esse projeto.</span><span class="sxs-lookup"><span data-stu-id="e7206-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![gerenciar pacotes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="e7206-187">Nos pacotes online, localize e instale o pacote do EntityFramework (versão 6,0 ou posterior).</span><span class="sxs-lookup"><span data-stu-id="e7206-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="e7206-188">Se parecer que o pacote do EntityFramework já está instalado, você pode ter selecionado o projeto StoreApp em vez do projeto StoreApp. Tests.</span><span class="sxs-lookup"><span data-stu-id="e7206-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the StoreApp.Tests project.</span></span>

![Adicionar Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="e7206-190">Localize e instale Microsoft ASP.NET pacote de núcleo da API Web 2.</span><span class="sxs-lookup"><span data-stu-id="e7206-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![instalar o pacote de núcleo da API Web](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="e7206-192">Feche a janela gerenciar pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="e7206-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="e7206-193">Criar contexto de teste</span><span class="sxs-lookup"><span data-stu-id="e7206-193">Create test context</span></span>

<span data-ttu-id="e7206-194">Adicione uma classe chamada **TestDbSet** ao projeto de teste.</span><span class="sxs-lookup"><span data-stu-id="e7206-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="e7206-195">Essa classe serve como a classe base para o conjunto de dados de teste.</span><span class="sxs-lookup"><span data-stu-id="e7206-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="e7206-196">Substitua o código pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e7206-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="e7206-197">Adicione uma classe chamada **TestProductDbSet** ao projeto de teste que contém o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e7206-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="e7206-198">Adicione uma classe chamada **TestStoreAppContext** e substitua o código existente pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e7206-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="e7206-199">Criar testes</span><span class="sxs-lookup"><span data-stu-id="e7206-199">Create tests</span></span>

<span data-ttu-id="e7206-200">Por padrão, o projeto de teste inclui um arquivo de teste vazio chamado **UnitTest1.cs**.</span><span class="sxs-lookup"><span data-stu-id="e7206-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="e7206-201">Esse arquivo mostra os atributos que você usa para criar métodos de teste.</span><span class="sxs-lookup"><span data-stu-id="e7206-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="e7206-202">Para este tutorial, você pode excluir este arquivo porque você adicionará uma nova classe de teste.</span><span class="sxs-lookup"><span data-stu-id="e7206-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="e7206-203">Adicione uma classe chamada **TestProductController** ao projeto de teste.</span><span class="sxs-lookup"><span data-stu-id="e7206-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="e7206-204">Substitua o código pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e7206-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="e7206-205">Executar testes</span><span class="sxs-lookup"><span data-stu-id="e7206-205">Run tests</span></span>

<span data-ttu-id="e7206-206">Agora você está pronto para executar os testes.</span><span class="sxs-lookup"><span data-stu-id="e7206-206">You are now ready to run the tests.</span></span> <span data-ttu-id="e7206-207">Todos os métodos marcados com o atributo **TestMethod** serão testados.</span><span class="sxs-lookup"><span data-stu-id="e7206-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="e7206-208">No item de menu de **teste** , execute os testes.</span><span class="sxs-lookup"><span data-stu-id="e7206-208">From the **Test** menu item, run the tests.</span></span>

![executar testes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="e7206-210">Abra a janela **Gerenciador de testes** e observe os resultados dos testes.</span><span class="sxs-lookup"><span data-stu-id="e7206-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![resultados de testes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
