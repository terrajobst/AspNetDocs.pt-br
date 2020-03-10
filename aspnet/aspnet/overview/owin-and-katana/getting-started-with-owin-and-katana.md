---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Introdução com OWIN e Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 4dfd7b8ebb2bb48d7ef800fd522b79a7b4a045c2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584668"
---
# <a name="getting-started-with-owin-and-katana"></a><span data-ttu-id="b4dc5-102">Introdução a OWIN e Katana</span><span class="sxs-lookup"><span data-stu-id="b4dc5-102">Getting Started with OWIN and Katana</span></span>

<span data-ttu-id="b4dc5-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b4dc5-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="b4dc5-104">O [Open Web interface for .net (OWIN)](http://owin.org/) define uma abstração entre servidores Web e aplicativos Web do .net.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-104">[Open Web Interface for .NET (OWIN)](http://owin.org/) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="b4dc5-105">Ao desacoplar o servidor Web do aplicativo, o OWIN facilita a criação de middleware para desenvolvimento para a Web .NET.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-105">By decoupling the web server from the application, OWIN makes it easier to create middleware for .NET web development.</span></span> <span data-ttu-id="b4dc5-106">Além disso, o OWIN facilita a porta de aplicativos Web para outros&#8212;hosts, por exemplo, hospedagem interna em um serviço do Windows ou outro processo.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-106">Also, OWIN makes it easier to port web applications to other hosts&#8212;for example, self-hosting in a Windows service or other process.</span></span>

<span data-ttu-id="b4dc5-107">OWIN é uma especificação de propriedade da Comunidade, não uma implementação.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-107">OWIN is a community-owned specification, not an implementation.</span></span> <span data-ttu-id="b4dc5-108">O projeto Katana é um conjunto de componentes de OWIN de software livre desenvolvidos pela Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-108">The Katana project is a set of open-source OWIN components developed by Microsoft.</span></span> <span data-ttu-id="b4dc5-109">Para obter uma visão geral do OWIN e do katana, consulte [uma visão geral do Project Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="b4dc5-109">For a general overview of both OWIN and Katana, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="b4dc5-110">Neste artigo, vou ir direto para o código para começar.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-110">In this article, I will jump right into code to get started.</span></span>

<span data-ttu-id="b4dc5-111">Este tutorial usa [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), mas você também pode usar o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-111">This tutorial uses [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), but you can also use Visual Studio 2012.</span></span> <span data-ttu-id="b4dc5-112">Algumas das etapas são diferentes no Visual Studio 2012, que eu denotarei abaixo.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-112">A few of the steps are different in Visual Studio 2012, which I note below.</span></span>

## <a name="host-owin-in-iis"></a><span data-ttu-id="b4dc5-113">Host OWIN no IIS</span><span class="sxs-lookup"><span data-stu-id="b4dc5-113">Host OWIN in IIS</span></span>

<span data-ttu-id="b4dc5-114">Nesta seção, vamos hospedar OWIN no IIS.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-114">In this section, we'll host OWIN in IIS.</span></span> <span data-ttu-id="b4dc5-115">Essa opção oferece a flexibilidade e a capacidade de composição de um pipeline OWIN junto com o conjunto de recursos maduro do IIS.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-115">This option gives you the flexibility and composability of an OWIN pipeline together with the mature feature set of IIS.</span></span> <span data-ttu-id="b4dc5-116">Usando essa opção, o aplicativo OWIN é executado no pipeline de solicitação ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-116">Using this option, the OWIN application runs in the ASP.NET request pipeline.</span></span>

<span data-ttu-id="b4dc5-117">Primeiro, crie um novo projeto de aplicativo Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-117">First, create a new ASP.NET Web Application project.</span></span> <span data-ttu-id="b4dc5-118">(No Visual Studio 2012, use o tipo de projeto de aplicativo Web ASP.NET vazio.)</span><span class="sxs-lookup"><span data-stu-id="b4dc5-118">(In Visual Studio 2012, use the ASP.NET Empty Web Application project type.)</span></span>

![](getting-started-with-owin-and-katana/_static/image1.png)

<span data-ttu-id="b4dc5-119">Na caixa de diálogo **novo projeto ASP.net** , selecione o modelo **vazio** .</span><span class="sxs-lookup"><span data-stu-id="b4dc5-119">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span>

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a><span data-ttu-id="b4dc5-120">Adicionar pacotes do NuGet</span><span class="sxs-lookup"><span data-stu-id="b4dc5-120">Add NuGet Packages</span></span>

<span data-ttu-id="b4dc5-121">Em seguida, adicione os pacotes NuGet necessários.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-121">Next, add the required NuGet packages.</span></span> <span data-ttu-id="b4dc5-122">No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-122">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="b4dc5-123">Na janela do console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="b4dc5-123">In the Package Manager Console window, type the following command:</span></span>

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a><span data-ttu-id="b4dc5-124">Adicionar uma classe de inicialização</span><span class="sxs-lookup"><span data-stu-id="b4dc5-124">Add a Startup Class</span></span>

<span data-ttu-id="b4dc5-125">Em seguida, adicione uma classe de inicialização OWIN.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-125">Next, add an OWIN startup class.</span></span> <span data-ttu-id="b4dc5-126">Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto e selecione **Adicionar**e, em seguida, selecione **novo item**.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-126">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span> <span data-ttu-id="b4dc5-127">Na caixa de diálogo **Adicionar novo item** , selecione **classe de inicialização Owin**.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-127">In the **Add New Item** dialog, select **Owin Startup class**.</span></span> <span data-ttu-id="b4dc5-128">Para obter mais informações sobre como configurar a classe de inicialização, consulte [detecção de classe de inicialização OWIN](owin-startup-class-detection.md).</span><span class="sxs-lookup"><span data-stu-id="b4dc5-128">For more info on configuring the startup class, see [OWIN Startup Class Detection](owin-startup-class-detection.md).</span></span>

![](getting-started-with-owin-and-katana/_static/image4.png)

<span data-ttu-id="b4dc5-129">Adicione o seguinte código ao método `Startup1.Configuration`:</span><span class="sxs-lookup"><span data-stu-id="b4dc5-129">Add the following code to the `Startup1.Configuration` method:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

<span data-ttu-id="b4dc5-130">Esse código adiciona uma parte simples do middleware ao pipeline OWIN, implementado como uma função que recebe uma instância **Microsoft. OWIN. IOwinContext** .</span><span class="sxs-lookup"><span data-stu-id="b4dc5-130">This code adds a simple piece of middleware to the OWIN pipeline, implemented as a function that receives a **Microsoft.Owin.IOwinContext** instance.</span></span> <span data-ttu-id="b4dc5-131">Quando o servidor recebe uma solicitação HTTP, o pipeline OWIN invoca o middleware.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-131">When the server receives an HTTP request, the OWIN pipeline invokes the middleware.</span></span> <span data-ttu-id="b4dc5-132">O middleware define o tipo de conteúdo para a resposta e grava o corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-132">The middleware sets the content type for the response and writes the response body.</span></span>

> [!NOTE]
> <span data-ttu-id="b4dc5-133">O modelo de classe de inicialização OWIN está disponível em Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-133">The OWIN Startup class template is available in Visual Studio 2013.</span></span> <span data-ttu-id="b4dc5-134">Se você estiver usando o Visual Studio 2012, basta adicionar uma nova classe vazia denominada `Startup1`e colar o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="b4dc5-134">If you are using Visual Studio 2012, just add a new empty class named `Startup1`, and paste in the following code:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a><span data-ttu-id="b4dc5-135">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="b4dc5-135">Run the Application</span></span>

<span data-ttu-id="b4dc5-136">Pressione F5 para iniciar a depuração.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-136">Press F5 to begin debugging.</span></span> <span data-ttu-id="b4dc5-137">O Visual Studio abrirá uma janela do navegador para `http://localhost:*port*/`.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-137">Visual Studio will open a browser window to `http://localhost:*port*/`.</span></span> <span data-ttu-id="b4dc5-138">A página deve ser parecida com a seguinte:</span><span class="sxs-lookup"><span data-stu-id="b4dc5-138">The page should look like the following:</span></span>

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a><span data-ttu-id="b4dc5-139">OWIN de hospedagem interna em um aplicativo de console</span><span class="sxs-lookup"><span data-stu-id="b4dc5-139">Self-Host OWIN in a Console Application</span></span>

<span data-ttu-id="b4dc5-140">É fácil converter esse aplicativo da hospedagem do IIS para hospedagem interna em um processo personalizado.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-140">It's easy to convert this application from IIS hosting to self-hosting in a custom process.</span></span> <span data-ttu-id="b4dc5-141">Com a hospedagem do IIS, o IIS atua como o servidor HTTP e como o processo que hospeda o serviço.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-141">With IIS hosting, IIS acts as both the HTTP server and as the process that hosts the service.</span></span> <span data-ttu-id="b4dc5-142">Com a hospedagem interna, seu aplicativo cria o processo e usa a classe **HttpListener** como o servidor http.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-142">With self-hosting, your application creates the process and uses the **HttpListener** class as the HTTP server.</span></span>

<span data-ttu-id="b4dc5-143">No Visual Studio, crie um novo aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-143">In Visual Studio, create a new console application.</span></span> <span data-ttu-id="b4dc5-144">Na janela do console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="b4dc5-144">In the Package Manager Console window, type the following command:</span></span>

`Install-Package Microsoft.Owin.SelfHost -Pre`

<span data-ttu-id="b4dc5-145">Adicione uma classe de `Startup1` da parte 1 deste tutorial ao projeto.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-145">Add a `Startup1` class from part 1 of this tutorial to the project.</span></span> <span data-ttu-id="b4dc5-146">Você não precisa modificar essa classe.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-146">You don't need to modify this class.</span></span>

<span data-ttu-id="b4dc5-147">Implemente o método de `Main` do aplicativo da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-147">Implement the application's `Main` method as follows.</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

<span data-ttu-id="b4dc5-148">Quando você executa o aplicativo de console, o servidor começa a escutar `http://localhost:9000`.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-148">When you run the console application, the server starts listening to `http://localhost:9000`.</span></span> <span data-ttu-id="b4dc5-149">Se você navegar até esse endereço em um navegador da Web, verá a página "Olá, mundo".</span><span class="sxs-lookup"><span data-stu-id="b4dc5-149">If you navigate to this address in a web browser, you will see the "Hello world" page.</span></span>

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a><span data-ttu-id="b4dc5-150">Adicionar diagnóstico de OWIN</span><span class="sxs-lookup"><span data-stu-id="b4dc5-150">Add OWIN Diagnostics</span></span>

<span data-ttu-id="b4dc5-151">O pacote Microsoft. Owin. Diagnostics contém middleware que captura exceções sem tratamento e exibe uma página HTML com detalhes do erro.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-151">The Microsoft.Owin.Diagnostics package contains middleware that catches unhandled exceptions and displays an HTML page with error details.</span></span> <span data-ttu-id="b4dc5-152">Essa página funciona de forma muito parecida com a página de erro ASP.NET que, às vezes, é chamada de "[tela amarela de morte](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span><span class="sxs-lookup"><span data-stu-id="b4dc5-152">This page functions much like the ASP.NET error page that is sometimes called the "[yellow screen of death](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD).</span></span> <span data-ttu-id="b4dc5-153">Assim como o YSOD, a página de erro Katana é útil durante o desenvolvimento, mas é uma boa prática desabilitá-la no modo de produção.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-153">Like the YSOD, the Katana error page is useful during development, but it's a good practice to disable it in production mode.</span></span>

<span data-ttu-id="b4dc5-154">Para instalar o pacote de diagnóstico em seu projeto, digite o seguinte comando na janela do console do Gerenciador de pacotes:</span><span class="sxs-lookup"><span data-stu-id="b4dc5-154">To install the Diagnostics package in your project, type the following command in the Package Manager Console window:</span></span>

`install-package Microsoft.Owin.Diagnostics –Pre`

<span data-ttu-id="b4dc5-155">Altere o código em seu método `Startup1.Configuration` da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="b4dc5-155">Change the code in your `Startup1.Configuration` method as follows:</span></span>

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

<span data-ttu-id="b4dc5-156">Agora use CTRL + F5 para executar o aplicativo sem depuração, de modo que o Visual Studio não irá interromper a exceção.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-156">Now use CTRL+F5 to run the application without debugging, so that Visual Studio will not break on the exception.</span></span> <span data-ttu-id="b4dc5-157">O aplicativo se comporta da mesma forma que antes, até que você navegue até `http://localhost/fail`, ponto em que o aplicativo gera a exceção.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-157">The application behaves the same as before, until you navigate to `http://localhost/fail`, at which point the application throws the exception.</span></span> <span data-ttu-id="b4dc5-158">O middleware da página de erro irá capturar a exceção e exibir uma página HTML com informações sobre o erro.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-158">The error page middleware will catch the exception and display an HTML page with information about the error.</span></span> <span data-ttu-id="b4dc5-159">Você pode clicar nas guias para ver as variáveis de ambiente pilha, Cadeia de caracteres de consulta, cookies, cabeçalho de solicitação e OWIN.</span><span class="sxs-lookup"><span data-stu-id="b4dc5-159">You can click the tabs to see the stack, query string, cookies, request header, and OWIN environment variables.</span></span>

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a><span data-ttu-id="b4dc5-160">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="b4dc5-160">Next Steps</span></span>

- [<span data-ttu-id="b4dc5-161">Detecção de classe de inicialização OWIN</span><span class="sxs-lookup"><span data-stu-id="b4dc5-161">OWIN Startup Class Detection</span></span>](owin-startup-class-detection.md)
- [<span data-ttu-id="b4dc5-162">Usar o OWIN para hospedar automaticamente ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="b4dc5-162">Use OWIN to Self-Host ASP.NET Web API</span></span>](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [<span data-ttu-id="b4dc5-163">Usar o OWIN para o Signalr de autohost</span><span class="sxs-lookup"><span data-stu-id="b4dc5-163">Use OWIN to Self-Host SignalR</span></span>](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
