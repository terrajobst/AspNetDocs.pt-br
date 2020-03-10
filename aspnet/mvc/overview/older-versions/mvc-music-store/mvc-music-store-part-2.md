---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: controladores | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 2 abrange controladores.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9dc2226f4951d4bed122df37d35bbb94730a00ad
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559874"
---
# <a name="part-2-controllers"></a><span data-ttu-id="6e0cd-104">Parte 2: controladores</span><span class="sxs-lookup"><span data-stu-id="6e0cd-104">Part 2: Controllers</span></span>

<span data-ttu-id="6e0cd-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="6e0cd-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="6e0cd-106">A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="6e0cd-107">A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="6e0cd-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="6e0cd-109">A parte 2 abrange controladores.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-109">Part 2 covers Controllers.</span></span>

<span data-ttu-id="6e0cd-110">Com estruturas da Web tradicionais, as URLs de entrada são normalmente mapeadas para arquivos em disco.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="6e0cd-111">Por exemplo: uma solicitação para uma URL como "/Products.aspx" ou "/Products.php" pode ser processada por um arquivo "Products. aspx" ou "Products. php".</span><span class="sxs-lookup"><span data-stu-id="6e0cd-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="6e0cd-112">As estruturas MVC baseadas na Web mapeiam URLs para código de servidor de forma ligeiramente diferente.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="6e0cd-113">Em vez de mapear URLs de entrada para arquivos, eles mapeiam URLs para métodos em classes.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="6e0cd-114">Essas classes são chamadas de "controladores" e são responsáveis pelo processamento de solicitações HTTP de entrada, tratamento de entrada do usuário, recuperação e salvamento de dados e determinação da resposta a ser enviada de volta ao cliente (exibir HTML, baixar um arquivo, redirecionar para outro URL, etc.).</span><span class="sxs-lookup"><span data-stu-id="6e0cd-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="6e0cd-115">Adicionando um HomeController</span><span class="sxs-lookup"><span data-stu-id="6e0cd-115">Adding a HomeController</span></span>

<span data-ttu-id="6e0cd-116">Vamos começar nosso aplicativo MVC Music Store adicionando uma classe Controller que tratará URLs para a Home Page de nosso site.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="6e0cd-117">Seguiremos as convenções de nomenclatura padrão do ASP.NET MVC e as chamarei de HomeController.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="6e0cd-118">Clique com o botão direito do mouse na pasta "controladores" dentro da Gerenciador de Soluções e selecione "Adicionar" e, em seguida, o "controlador..." linha</span><span class="sxs-lookup"><span data-stu-id="6e0cd-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…" command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="6e0cd-119">Isso abrirá a caixa de diálogo "Adicionar controlador".</span><span class="sxs-lookup"><span data-stu-id="6e0cd-119">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="6e0cd-120">Nomeie o controlador como "HomeController" e pressione o botão Adicionar.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-120">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="6e0cd-121">Isso criará um novo arquivo, HomeController.cs, com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="6e0cd-121">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="6e0cd-122">Para iniciar o mais simples possível, vamos substituir o método de índice por um método simples que simplesmente retorna uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-122">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="6e0cd-123">Faremos duas alterações:</span><span class="sxs-lookup"><span data-stu-id="6e0cd-123">We'll make two changes:</span></span>

- <span data-ttu-id="6e0cd-124">Alterar o método para retornar uma cadeia de caracteres em vez de um ActionResult</span><span class="sxs-lookup"><span data-stu-id="6e0cd-124">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="6e0cd-125">Altere a instrução Return para retornar "Olá de casa"</span><span class="sxs-lookup"><span data-stu-id="6e0cd-125">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="6e0cd-126">O método agora deve ser assim:</span><span class="sxs-lookup"><span data-stu-id="6e0cd-126">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="6e0cd-127">Executando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="6e0cd-127">Running the Application</span></span>

<span data-ttu-id="6e0cd-128">Agora, vamos executar o site.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-128">Now let's run the site.</span></span> <span data-ttu-id="6e0cd-129">Podemos iniciar nosso servidor Web e experimentar o site usando uma das seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="6e0cd-129">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="6e0cd-130">Escolha o item de menu Depurar ⇨ iniciar depuração</span><span class="sxs-lookup"><span data-stu-id="6e0cd-130">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="6e0cd-131">Clique no botão de seta verde na barra de ferramentas ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="6e0cd-131">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="6e0cd-132">Use o atalho de teclado, F5.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-132">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="6e0cd-133">O uso de qualquer uma das etapas acima compilará nosso projeto e, em seguida, fará com que o ASP.NET Development Server que é integrado ao Visual Web Developer seja iniciado.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-133">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="6e0cd-134">Uma notificação será exibida no canto inferior da tela para indicar que a ASP.NET Development Server foi iniciada e mostrará o número da porta em que ela está sendo executada.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-134">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="6e0cd-135">O Visual Web Developer abrirá automaticamente uma janela do navegador cuja URL aponta para nosso servidor Web.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-135">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="6e0cd-136">Isso nos permitirá testar rapidamente nosso aplicativo Web:</span><span class="sxs-lookup"><span data-stu-id="6e0cd-136">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="6e0cd-137">Ok, isso foi bem rápido – criamos um novo site, adicionamos uma função de três linhas e temos texto em um navegador.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-137">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="6e0cd-138">Não tem ciência de Rocket, mas é um começo.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-138">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="6e0cd-139">*Observação: o Visual Web Developer inclui o ASP.NET Development Server, que executará seu site em um número "porta" livre aleatório. Na captura de tela acima, o site está em execução em `http://localhost:26641/`, portanto, ele está usando a porta 26641. O número da porta será diferente. Quando falamos sobre a URL como/Store/Browse neste tutorial, isso vai depois do número da porta. Supondo que um número de porta 26641, navegar até/Store/Browse significará navegar até `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="6e0cd-139">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="6e0cd-140">Adicionando um StoreController</span><span class="sxs-lookup"><span data-stu-id="6e0cd-140">Adding a StoreController</span></span>

<span data-ttu-id="6e0cd-141">Adicionamos um HomeController simples que implementa a home page do nosso site.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-141">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="6e0cd-142">Agora, vamos adicionar outro controlador que usaremos para implementar a funcionalidade de navegação de nossa loja de música.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-142">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="6e0cd-143">Nosso controlador de loja dará suporte a três cenários:</span><span class="sxs-lookup"><span data-stu-id="6e0cd-143">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="6e0cd-144">Uma página de listagem dos gêneros musicais em nossa loja de música</span><span class="sxs-lookup"><span data-stu-id="6e0cd-144">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="6e0cd-145">Uma página de navegação que lista todos os álbuns de música em um gênero específico</span><span class="sxs-lookup"><span data-stu-id="6e0cd-145">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="6e0cd-146">Uma página de detalhes que mostra informações sobre um álbum de música específico</span><span class="sxs-lookup"><span data-stu-id="6e0cd-146">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="6e0cd-147">Vamos começar adicionando uma nova classe StoreController.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-147">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="6e0cd-148">Se você ainda não fez isso, interrompa a execução do aplicativo fechando o navegador ou selecionando o item de menu Depurar ⇨ parar depuração.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-148">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="6e0cd-149">Agora, adicione um novo StoreController.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-149">Now add a new StoreController.</span></span> <span data-ttu-id="6e0cd-150">Assim como fizemos com HomeController, faremos isso clicando com o botão direito do mouse na pasta "controladores" dentro da Gerenciador de Soluções e escolhendo o item de menu do controlador Add-&gt;</span><span class="sxs-lookup"><span data-stu-id="6e0cd-150">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="6e0cd-151">Nosso novo StoreController já tem um método "index".</span><span class="sxs-lookup"><span data-stu-id="6e0cd-151">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="6e0cd-152">Usaremos esse método de "índice" para implementar nossa página de listagem que lista todos os gêneros em nossa loja de música.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-152">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="6e0cd-153">Também adicionaremos dois métodos adicionais para implementar os dois outros cenários que queremos que nosso StoreController manipule: Browse e details.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-153">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="6e0cd-154">Esses métodos (índice, navegação e detalhes) em nosso controlador são chamados de "ações do controlador" e, como você já viu com o método de ação HomeController. Index (), seu trabalho é responder às solicitações de URL e (geralmente falando) determinar qual conteúdo deve ser enviado de volta para o navegador ou usuário que invocou a URL.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-154">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="6e0cd-155">Vamos iniciar nossa implementação de StoreController alterando o método index () para retornar a cadeia de caracteres "Hello from Store. Index ()" e adicionaremos métodos semelhantes para Browse () e Details ():</span><span class="sxs-lookup"><span data-stu-id="6e0cd-155">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="6e0cd-156">Execute o projeto novamente e procure as seguintes URLs:</span><span class="sxs-lookup"><span data-stu-id="6e0cd-156">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="6e0cd-157">/Store</span><span class="sxs-lookup"><span data-stu-id="6e0cd-157">/Store</span></span>
- <span data-ttu-id="6e0cd-158">/Store/Browse</span><span class="sxs-lookup"><span data-stu-id="6e0cd-158">/Store/Browse</span></span>
- <span data-ttu-id="6e0cd-159">/Store/Details</span><span class="sxs-lookup"><span data-stu-id="6e0cd-159">/Store/Details</span></span>

<span data-ttu-id="6e0cd-160">O acesso a essas URLs invocará os métodos de ação em nosso controlador e retornará respostas de cadeia de caracteres:</span><span class="sxs-lookup"><span data-stu-id="6e0cd-160">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="6e0cd-161">Isso é ótimo, mas essas são apenas cadeias de caracteres constantes.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-161">That's great, but these are just constant strings.</span></span> <span data-ttu-id="6e0cd-162">Vamos torná-las dinâmicas, portanto, elas recebem informações da URL e as exibem na saída da página.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-162">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="6e0cd-163">Primeiro, alteraremos o método de ação Browse para recuperar um valor de QueryString da URL.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-163">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="6e0cd-164">Podemos fazer isso adicionando um parâmetro "gênero" ao nosso método de ação.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-164">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="6e0cd-165">Quando fizermos isso, o ASP.NET MVC passará automaticamente qualquer parâmetro de postagem de formulário ou QueryString chamado "gênero" para nosso método de ação quando for invocado.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-165">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="6e0cd-166">*Observação: estamos usando o método de utilitário HttpUtility. HtmlEncode para limpar a entrada do usuário. Isso impede que os usuários insiram JavaScript em nossa exibição com um link como/Store/Browse? Gênero =&lt;script&gt;janela. Location = 'http://hackersite.com'&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="6e0cd-166">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="6e0cd-167">Agora, vamos navegar até/Store/Browse? Gênero = disco</span><span class="sxs-lookup"><span data-stu-id="6e0cd-167">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="6e0cd-168">Em seguida, vamos alterar a ação Details para ler e exibir um parâmetro de entrada chamado ID.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-168">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="6e0cd-169">Ao contrário de nosso método anterior, não inseriremos o valor de ID como um parâmetro QueryString.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-169">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="6e0cd-170">Em vez disso, vamos inseri-lo diretamente na própria URL.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-170">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="6e0cd-171">Por exemplo:/Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-171">For example: /Store/Details/5.</span></span>

<span data-ttu-id="6e0cd-172">O ASP.NET MVC nos permite fazer isso facilmente sem precisar configurar nada.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-172">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="6e0cd-173">A Convenção de roteamento padrão do ASP.NET MVC é tratar o segmento de uma URL após o nome do método de ação como um parâmetro chamado "ID".</span><span class="sxs-lookup"><span data-stu-id="6e0cd-173">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="6e0cd-174">Se o método de ação tiver um parâmetro chamado ID, o ASP.NET MVC passará automaticamente o segmento de URL para você como um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="6e0cd-174">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="6e0cd-175">Execute o aplicativo e navegue até/Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="6e0cd-175">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="6e0cd-176">Vamos recapitular o que fizemos até agora:</span><span class="sxs-lookup"><span data-stu-id="6e0cd-176">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="6e0cd-177">Criamos um novo projeto MVC do ASP.NET no Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="6e0cd-177">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="6e0cd-178">Discutimos a estrutura básica de pastas de um aplicativo MVC ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6e0cd-178">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="6e0cd-179">Aprendemos como executar nosso site usando o ASP.NET Development Server</span><span class="sxs-lookup"><span data-stu-id="6e0cd-179">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="6e0cd-180">Criamos duas classes de controlador: um HomeController e um StoreController</span><span class="sxs-lookup"><span data-stu-id="6e0cd-180">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="6e0cd-181">Adicionamos métodos de ação a nossos controladores que respondem a solicitações de URL e retornam texto para o navegador</span><span class="sxs-lookup"><span data-stu-id="6e0cd-181">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6e0cd-182">[Anterior](mvc-music-store-part-1.md)
> [Próximo](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="6e0cd-182">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
