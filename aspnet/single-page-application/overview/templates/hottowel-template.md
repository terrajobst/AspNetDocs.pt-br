---
uid: single-page-application/overview/templates/hottowel-template
title: Modelo de toalha quente | Microsoft Docs
author: madskristensen
description: Modelo HotTowel
ms.author: riande
ms.date: 02/09/2013
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: eeab69e75546791978bb09d7823d95caf9dca1a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578480"
---
# <a name="hot-towel-template"></a><span data-ttu-id="c3fc7-103">Modelo Hot Towel</span><span class="sxs-lookup"><span data-stu-id="c3fc7-103">Hot Towel template</span></span>

<span data-ttu-id="c3fc7-104">por [Mads Kristensen](https://github.com/madskristensen)</span><span class="sxs-lookup"><span data-stu-id="c3fc7-104">by [Mads Kristensen](https://github.com/madskristensen)</span></span>

> <span data-ttu-id="c3fc7-105">O modelo do Hot toalha MVC é escrito por John Papa</span><span class="sxs-lookup"><span data-stu-id="c3fc7-105">The Hot Towel MVC Template is written by John Papa</span></span>
> 
> <span data-ttu-id="c3fc7-106">Escolha qual versão baixar:</span><span class="sxs-lookup"><span data-stu-id="c3fc7-106">Choose which version to download:</span></span>
> 
> [<span data-ttu-id="c3fc7-107">Modelo do Hot toalha MVC para Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="c3fc7-107">Hot Towel MVC Template for Visual Studio 2012</span></span>](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [<span data-ttu-id="c3fc7-108">Modelo do Hot toalha MVC para Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c3fc7-108">Hot Towel MVC Template for Visual Studio 2013</span></span>](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> <span data-ttu-id="c3fc7-109">Toalha quente: porque você não deseja ir para o SPA sem um!</span><span class="sxs-lookup"><span data-stu-id="c3fc7-109">Hot Towel: Because you don't want to go to the SPA without one!</span></span>

<span data-ttu-id="c3fc7-110">Deseja criar um SPA, mas não pode decidir onde começar?</span><span class="sxs-lookup"><span data-stu-id="c3fc7-110">Want to build a SPA but can't decide where to start?</span></span> <span data-ttu-id="c3fc7-111">Use a toalha quente e, em segundos, você terá um SPA e todas as ferramentas de que precisa para se basear!</span><span class="sxs-lookup"><span data-stu-id="c3fc7-111">Use Hot Towel and in seconds you'll have a SPA and all the tools you need to build on it!</span></span>

<span data-ttu-id="c3fc7-112">A toalha quente cria um ótimo ponto de partida para a criação de um aplicativo de página única (SPA) com ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-112">Hot Towel creates a great starting point for building a Single Page Application (SPA) with ASP.NET.</span></span> <span data-ttu-id="c3fc7-113">Você fornece uma estrutura modular para seu código, exibição de navegação, vinculação de dados, gerenciamento de dados avançado e estilo simples, mas elegante.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-113">Out of the box you it provides a modular structure for your code, view navigation, data binding, rich data management and simple but elegant styling.</span></span> <span data-ttu-id="c3fc7-114">A toalha quente fornece tudo o que você precisa para criar um SPA, para que você possa se concentrar em seu aplicativo, não no encanamento.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-114">Hot Towel provides everything you need to build a SPA, so you can focus on your app, not the plumbing.</span></span>

> <span data-ttu-id="c3fc7-115">Saiba mais sobre como criar um SPA nos [vídeos, tutoriais e cursos da Pluralsight do John Papa](http://johnpapa.net/spa?vsix).</span><span class="sxs-lookup"><span data-stu-id="c3fc7-115">Learn more about building a SPA from [John Papa's videos, tutorials and Pluralsight courses](http://johnpapa.net/spa?vsix).</span></span>

## <a name="application-structure"></a><span data-ttu-id="c3fc7-116">Estrutura de Aplicativos</span><span class="sxs-lookup"><span data-stu-id="c3fc7-116">Application Structure</span></span>

<span data-ttu-id="c3fc7-117">O SPA quente fornece uma pasta de aplicativo que contém os arquivos JavaScript e HTML que definem seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-117">Hot Towel SPA provides an App folder which contains the JavaScript and HTML files that define your application.</span></span>

<span data-ttu-id="c3fc7-118">Dentro da pasta do aplicativo:</span><span class="sxs-lookup"><span data-stu-id="c3fc7-118">Inside the App folder:</span></span>

- <span data-ttu-id="c3fc7-119">Durandal</span><span class="sxs-lookup"><span data-stu-id="c3fc7-119">durandal</span></span>
- <span data-ttu-id="c3fc7-120">services</span><span class="sxs-lookup"><span data-stu-id="c3fc7-120">services</span></span>
- <span data-ttu-id="c3fc7-121">ViewModels</span><span class="sxs-lookup"><span data-stu-id="c3fc7-121">viewmodels</span></span>
- <span data-ttu-id="c3fc7-122">Modos de exibição</span><span class="sxs-lookup"><span data-stu-id="c3fc7-122">views</span></span>

<span data-ttu-id="c3fc7-123">A pasta do aplicativo contém uma coleção de módulos.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-123">The App folder contains a collection of modules.</span></span> <span data-ttu-id="c3fc7-124">Esses módulos encapsulam a funcionalidade e declaram dependências em outros módulos.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-124">These modules encapsulate functionality and declare dependencies on other modules.</span></span> <span data-ttu-id="c3fc7-125">A pasta views contém o HTML para seu aplicativo e a pasta ViewModels contém a lógica de apresentação para as exibições (um padrão MVVM comum).</span><span class="sxs-lookup"><span data-stu-id="c3fc7-125">The views folder contains the HTML for your application and the viewmodels folder contains the presentation logic for the views (a common MVVM pattern).</span></span> <span data-ttu-id="c3fc7-126">A pasta serviços é ideal para hospedar qualquer serviço comum que seu aplicativo possa precisar, como a recuperação de dados HTTP ou a interação de armazenamento local.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-126">The services folder is ideal for housing any common services that your application may need such as HTTP data retrieval or local storage interaction.</span></span> <span data-ttu-id="c3fc7-127">É comum que vários ViewModels usem novamente o código dos módulos de serviço.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-127">It is common for multiple viewmodels to re-use code from the service modules.</span></span>

## <a name="aspnet-mvc-server-side-application-structure"></a><span data-ttu-id="c3fc7-128">Estrutura de aplicativo do lado do servidor ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c3fc7-128">ASP.NET MVC Server Side Application Structure</span></span>

<span data-ttu-id="c3fc7-129">A toalha quente se baseia na estrutura MVC familiar e poderosa do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-129">Hot Towel builds on the familiar and powerful ASP.NET MVC structure.</span></span>

- <span data-ttu-id="c3fc7-130">Início do\_do aplicativo</span><span class="sxs-lookup"><span data-stu-id="c3fc7-130">App\_Start</span></span>
- <span data-ttu-id="c3fc7-131">Conteúdo</span><span class="sxs-lookup"><span data-stu-id="c3fc7-131">Content</span></span>
- <span data-ttu-id="c3fc7-132">Controladores</span><span class="sxs-lookup"><span data-stu-id="c3fc7-132">Controllers</span></span>
- <span data-ttu-id="c3fc7-133">Modelos</span><span class="sxs-lookup"><span data-stu-id="c3fc7-133">Models</span></span>
- <span data-ttu-id="c3fc7-134">Scripts</span><span class="sxs-lookup"><span data-stu-id="c3fc7-134">Scripts</span></span>
- <span data-ttu-id="c3fc7-135">Exibições</span><span class="sxs-lookup"><span data-stu-id="c3fc7-135">Views</span></span>

## <a name="featured-libraries"></a><span data-ttu-id="c3fc7-136">Bibliotecas em destaque</span><span class="sxs-lookup"><span data-stu-id="c3fc7-136">Featured Libraries</span></span>

- <span data-ttu-id="c3fc7-137">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="c3fc7-137">ASP.NET MVC</span></span>
- <span data-ttu-id="c3fc7-138">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="c3fc7-138">ASP.NET Web API</span></span>
- <span data-ttu-id="c3fc7-139">Otimização da Web do ASP.NET-agrupamento e minificação</span><span class="sxs-lookup"><span data-stu-id="c3fc7-139">ASP.NET Web Optimization - bundling and minification</span></span>
- <span data-ttu-id="c3fc7-140">[Breeze. js](http://Breezejs.com) -gerenciamento de dados avançado</span><span class="sxs-lookup"><span data-stu-id="c3fc7-140">[Breeze.js](http://Breezejs.com) - rich data management</span></span>
- <span data-ttu-id="c3fc7-141">[Durandal. js](http://Durandaljs.com) -composição de navegação e exibição</span><span class="sxs-lookup"><span data-stu-id="c3fc7-141">[Durandal.js](http://Durandaljs.com) - navigation and View composition</span></span>
- <span data-ttu-id="c3fc7-142">[Knockout. js](http://Knockoutjs.com) -associações de dados</span><span class="sxs-lookup"><span data-stu-id="c3fc7-142">[Knockout.js](http://Knockoutjs.com) - data bindings</span></span>
- <span data-ttu-id="c3fc7-143">[Requires. js](http://requirejs.org) -modularidade com AMD e otimização</span><span class="sxs-lookup"><span data-stu-id="c3fc7-143">[Require.js](http://requirejs.org) - Modularity with AMD and optimization</span></span>
- <span data-ttu-id="c3fc7-144">[Torradeira. js](http://jpapa.me/c7toastr) -mensagens pop-up</span><span class="sxs-lookup"><span data-stu-id="c3fc7-144">[Toastr.js](http://jpapa.me/c7toastr) - pop-up messages</span></span>
- <span data-ttu-id="c3fc7-145">[Bootstrap do Twitter](https://twitter.github.com/bootstrap/) – estilo de CSS robusto</span><span class="sxs-lookup"><span data-stu-id="c3fc7-145">[Twitter Bootstrap](https://twitter.github.com/bootstrap/) - robust CSS styling</span></span>

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a><span data-ttu-id="c3fc7-146">Instalando por meio do modelo de SPA do Visual Studio 2012 quente</span><span class="sxs-lookup"><span data-stu-id="c3fc7-146">Installing via the Visual Studio 2012 Hot Towel SPA Template</span></span>

<span data-ttu-id="c3fc7-147">A toalha quente pode ser instalada como um modelo do Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-147">Hot Towel can be installed as a Visual Studio 2012 template.</span></span> <span data-ttu-id="c3fc7-148">Basta clicar `File` | `New Project` e escolher `ASP.NET MVC 4 Web Application`.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-148">Just click `File` | `New Project` and choose `ASP.NET MVC 4 Web Application`.</span></span> <span data-ttu-id="c3fc7-149">Em seguida, selecione o modelo "aplicativo de página única de toalha quente" e execute!</span><span class="sxs-lookup"><span data-stu-id="c3fc7-149">Then select the 'Hot Towel Single Page Application" template and run!</span></span>

## <a name="installing-via-the-nuget-package"></a><span data-ttu-id="c3fc7-150">Instalando por meio do pacote NuGet</span><span class="sxs-lookup"><span data-stu-id="c3fc7-150">Installing via the NuGet Package</span></span>

<span data-ttu-id="c3fc7-151">A toalha quente também é um pacote NuGet que aumenta um projeto do MVC ASP.NET vazio existente.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-151">Hot Towel is also a NuGet package that augments an existing empty ASP.NET MVC project.</span></span> <span data-ttu-id="c3fc7-152">Basta instalar usando o NuGet e, em seguida, executar!</span><span class="sxs-lookup"><span data-stu-id="c3fc7-152">Just install using Nuget and then run!</span></span>

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a><span data-ttu-id="c3fc7-153">Como faço para criar na toalha quente?</span><span class="sxs-lookup"><span data-stu-id="c3fc7-153">How Do I Build On Hot Towel?</span></span>

<span data-ttu-id="c3fc7-154">Basta começar a adicionar código!</span><span class="sxs-lookup"><span data-stu-id="c3fc7-154">Simply start adding code!</span></span>

1. <span data-ttu-id="c3fc7-155">Adicione seu próprio código do lado do servidor, preferencialmente Entity Framework e WebAPI (que realmente brilha com o Breeze. js)</span><span class="sxs-lookup"><span data-stu-id="c3fc7-155">Add your own server-side code, preferably Entity Framework and WebAPI (which really shine with Breeze.js)</span></span>
2. <span data-ttu-id="c3fc7-156">Adicionar exibições à pasta `App/views`</span><span class="sxs-lookup"><span data-stu-id="c3fc7-156">Add views to the `App/views` folder</span></span>
3. <span data-ttu-id="c3fc7-157">Adicionar ViewModels à pasta `App/viewmodels`</span><span class="sxs-lookup"><span data-stu-id="c3fc7-157">Add viewmodels to the `App/viewmodels` folder</span></span>
4. <span data-ttu-id="c3fc7-158">Adicionar associações de dados HTML e Knockout às suas novas exibições</span><span class="sxs-lookup"><span data-stu-id="c3fc7-158">Add HTML and Knockout data bindings to your new views</span></span>
5. <span data-ttu-id="c3fc7-159">Atualizar as rotas de navegação no `shell.js`</span><span class="sxs-lookup"><span data-stu-id="c3fc7-159">Update the navigation routes in `shell.js`</span></span>

## <a name="walkthrough-of-the-htmljavascript"></a><span data-ttu-id="c3fc7-160">Walkthrough. HTML/JavaScript</span><span class="sxs-lookup"><span data-stu-id="c3fc7-160">Walkthrough of the HTML/JavaScript</span></span>

### <a name="viewshottowelindexcshtml"></a><span data-ttu-id="c3fc7-161">Views/HotTowel/index.cshtml</span><span class="sxs-lookup"><span data-stu-id="c3fc7-161">Views/HotTowel/index.cshtml</span></span>

<span data-ttu-id="c3fc7-162">index. cshtml é a rota inicial e a exibição para o aplicativo MVC.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-162">index.cshtml is the starting route and view for the MVC application.</span></span> <span data-ttu-id="c3fc7-163">Ele contém todas as marcas meta padrão, Links CSS e Referências JavaScript que você esperaria.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-163">It contains all the standard meta tags, css links, and JavaScript references you would expect.</span></span> <span data-ttu-id="c3fc7-164">O corpo contém um único `<div>` que é onde todo o conteúdo (suas exibições) será colocado quando forem solicitados.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-164">The body contains a single `<div>` which is where all of the content (your views) will be placed when they are requested.</span></span> <span data-ttu-id="c3fc7-165">O `@Scripts.Render` usa o require. js para executar o ponto de entrada para o código do aplicativo, que está contido no arquivo de `main.js`.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-165">The `@Scripts.Render` uses Require.js to run the entrance point for the application's code, which is contained in the `main.js` file.</span></span> <span data-ttu-id="c3fc7-166">Uma tela inicial é fornecida para demonstrar como criar uma tela inicial enquanto seu aplicativo é carregado.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-166">A splash screen is provided to demonstrate how to create a splash screen while your app loads.</span></span>

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a><span data-ttu-id="c3fc7-167">App/Main. js</span><span class="sxs-lookup"><span data-stu-id="c3fc7-167">App/main.js</span></span>

<span data-ttu-id="c3fc7-168">O arquivo de `main.js` contém o código que será executado assim que seu aplicativo for carregado.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-168">The `main.js` file contains the code that will run as soon as your app is loaded.</span></span> <span data-ttu-id="c3fc7-169">É aqui que você deseja definir suas rotas de navegação, definir suas exibições de inicialização e executar qualquer configuração/inicialização, como estender os dados do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-169">This is where you want to define your navigation routes, set your start up views, and perform any setup/bootstrapping such as priming your application's data.</span></span>

<span data-ttu-id="c3fc7-170">O arquivo `main.js` define vários módulos do Durandal para ajudar a iniciar o início do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-170">The `main.js` file defines several of durandal's modules to help the application kick start.</span></span> <span data-ttu-id="c3fc7-171">A instrução define ajuda a resolver as dependências dos módulos para que fiquem disponíveis para a função.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-171">The define statement helps resolve the modules dependencies so they are available for the function.</span></span> <span data-ttu-id="c3fc7-172">Primeiro, as mensagens de depuração são habilitadas, que enviam mensagens sobre quais eventos o aplicativo está executando na janela do console.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-172">First the debugging messages are enabled, which send messages about what events the application is performing to the console window.</span></span> <span data-ttu-id="c3fc7-173">O código app. Start diz ao Durandal Framework para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-173">The app.start code tells durandal framework to start the application.</span></span> <span data-ttu-id="c3fc7-174">As convenções são definidas para que Durandal saiba que todos os modos de exibição e ViewModels estão contidos nas mesmas pastas nomeadas, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-174">The conventions are set so that durandal knows all views and viewmodels are contained in the same named folders, respectively.</span></span> <span data-ttu-id="c3fc7-175">Por fim, o `app.setRoot` aciona o `shell` usando uma animação de `entrance` predefinida.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-175">Finally, the `app.setRoot` kicks loads the `shell` using a predefined `entrance` animation.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a><span data-ttu-id="c3fc7-176">Exibições</span><span class="sxs-lookup"><span data-stu-id="c3fc7-176">Views</span></span>

<span data-ttu-id="c3fc7-177">As exibições são encontradas na pasta `App/views`.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-177">Views are found in the `App/views` folder.</span></span>

### <a name="shellhtml"></a><span data-ttu-id="c3fc7-178">shell.html</span><span class="sxs-lookup"><span data-stu-id="c3fc7-178">shell.html</span></span>

<span data-ttu-id="c3fc7-179">O `shell.html` contém o layout mestre para seu HTML.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-179">The `shell.html` contains the master layout for your HTML.</span></span> <span data-ttu-id="c3fc7-180">Todas as outras exibições serão compostas em algum lugar do seu `shell` exibição.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-180">All of your other views will be composed somewhere in side of your `shell` view.</span></span> <span data-ttu-id="c3fc7-181">A toalha quente fornece uma `shell` com três regiões: um cabeçalho, uma área de conteúdo e um rodapé.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-181">Hot Towel provides a `shell` with three such regions: a header, a content area, and a footer.</span></span> <span data-ttu-id="c3fc7-182">Cada uma dessas regiões é carregada com o conteúdo do formulário outras exibições quando solicitado.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-182">Each of these regions is loaded with contents form other views when requested.</span></span>

<span data-ttu-id="c3fc7-183">As associações de `compose` para o cabeçalho e o rodapé são embutidas em código na toalha quente para apontar para as exibições de `nav` e `footer`, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-183">The `compose` bindings for the header and footer are hard coded in Hot Towel to point to the `nav` and `footer` views, respectively.</span></span> <span data-ttu-id="c3fc7-184">A associação de composição para a seção `#content` está associada ao item ativo do módulo `router`.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-184">The compose binding for the section `#content` is bound to the `router` module's active item.</span></span> <span data-ttu-id="c3fc7-185">Em outras palavras, quando você clica em um link de navegação, o modo de exibição correspondente é carregado nessa área.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-185">In other words, when you click a navigation link it's corresponding view is loaded in this area.</span></span>

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a><span data-ttu-id="c3fc7-186">nav.html</span><span class="sxs-lookup"><span data-stu-id="c3fc7-186">nav.html</span></span>

<span data-ttu-id="c3fc7-187">O `nav.html` contém os links de navegação para o SPA.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-187">The `nav.html` contains the navigation links for the SPA.</span></span> <span data-ttu-id="c3fc7-188">É aí que a estrutura de menu pode ser colocada, por exemplo.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-188">This is where the menu structure can be placed, for example.</span></span> <span data-ttu-id="c3fc7-189">Geralmente, isso é associado a dados (usando o Knockout) ao módulo `router` para exibir a navegação que você definiu na `shell.js`.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-189">Often this is data bound (using Knockout) to the `router` module to display the navigation you defined in the `shell.js`.</span></span> <span data-ttu-id="c3fc7-190">O Knockout procura os atributos de vinculação de dados e associa-os à `shell` ViewModel para exibir as rotas de navegação e mostrar um ProgressBar (usando a inicialização do Twitter) se o módulo `router` estiver no meio da navegação de uma exibição para outra (consulte `router.isNavigating`).</span><span class="sxs-lookup"><span data-stu-id="c3fc7-190">Knockout looks for the data-bind attributes and binds those to the `shell` viewmodel to display the navigation routes and to show a progressbar (using Twitter Bootstrap) if the `router` module is in the middle of navigating from one view to another (see `router.isNavigating`).</span></span>

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a><span data-ttu-id="c3fc7-191">Home. html e details. html</span><span class="sxs-lookup"><span data-stu-id="c3fc7-191">home.html and details.html</span></span>

<span data-ttu-id="c3fc7-192">Essas exibições contêm HTML para exibições personalizadas.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-192">These views contain HTML for custom views.</span></span> <span data-ttu-id="c3fc7-193">Quando o link de `home` no menu do `nav` exibição é clicado, o `home` exibição será colocado na área de conteúdo da exibição de `shell`.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-193">When the `home` link in the `nav` view's menu is clicked, the `home` view will be placed in the content area of the `shell` view.</span></span> <span data-ttu-id="c3fc7-194">Essas exibições podem ser aumentadas ou substituídas por suas próprias exibições personalizadas.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-194">These views can be augmented or replaced with your own custom views.</span></span>

### <a name="footerhtml"></a><span data-ttu-id="c3fc7-195">footer.html</span><span class="sxs-lookup"><span data-stu-id="c3fc7-195">footer.html</span></span>

<span data-ttu-id="c3fc7-196">O `footer.html` contém HTML que aparece no rodapé, na parte inferior da exibição `shell`.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-196">The `footer.html` contains HTML that appears in the footer, at the bottom of the `shell` view.</span></span>

## <a name="viewmodels"></a><span data-ttu-id="c3fc7-197">ViewModels</span><span class="sxs-lookup"><span data-stu-id="c3fc7-197">ViewModels</span></span>

<span data-ttu-id="c3fc7-198">ViewModels são encontrados na pasta `App/viewmodels`.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-198">ViewModels are found in the `App/viewmodels` folder.</span></span>

### <a name="shelljs"></a><span data-ttu-id="c3fc7-199">Shell. js</span><span class="sxs-lookup"><span data-stu-id="c3fc7-199">shell.js</span></span>

<span data-ttu-id="c3fc7-200">O `shell` ViewModel contém propriedades e funções que estão associadas à exibição `shell`.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-200">The `shell` viewmodel contains properties and functions that are bound to the `shell` view.</span></span> <span data-ttu-id="c3fc7-201">Geralmente, é aqui que as associações de navegação de menu são encontradas (consulte a lógica de `router.mapNav`).</span><span class="sxs-lookup"><span data-stu-id="c3fc7-201">Often this is where the menu navigation bindings are found (see the `router.mapNav` logic).</span></span>

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a><span data-ttu-id="c3fc7-202">Home. js e details. js</span><span class="sxs-lookup"><span data-stu-id="c3fc7-202">home.js and details.js</span></span>

<span data-ttu-id="c3fc7-203">Esses ViewModels contêm as propriedades e funções que estão associadas à exibição de `home`.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-203">These viewmodels contain the properties and functions that are bound to the `home` view.</span></span> <span data-ttu-id="c3fc7-204">Ele também contém a lógica de apresentação da exibição e é a cola entre os dados e a exibição.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-204">it also contains the presentation logic for the view, and is the glue between the data and the view.</span></span>

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a><span data-ttu-id="c3fc7-205">Serviços</span><span class="sxs-lookup"><span data-stu-id="c3fc7-205">Services</span></span>

<span data-ttu-id="c3fc7-206">Os serviços são encontrados na pasta app/Services.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-206">Services are found in the App/services folder.</span></span> <span data-ttu-id="c3fc7-207">O ideal é que seus serviços futuros, como um módulo DataService, que sejam responsáveis por obter e postar dados remotos, possam ser colocados.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-207">Ideally your future services such as a dataservice module, that is responsible for getting and posting remote data, could be placed.</span></span>

### <a name="loggerjs"></a><span data-ttu-id="c3fc7-208">logger.js</span><span class="sxs-lookup"><span data-stu-id="c3fc7-208">logger.js</span></span>

<span data-ttu-id="c3fc7-209">A toalha quente fornece um módulo `logger` na pasta serviços.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-209">Hot Towel provides a `logger` module in the services folder.</span></span> <span data-ttu-id="c3fc7-210">O módulo `logger` é ideal para registrar mensagens no console e para o usuário em exibir notificações.</span><span class="sxs-lookup"><span data-stu-id="c3fc7-210">The `logger` module is ideal for logging messages to the console and to the user in pop up toasts.</span></span>
