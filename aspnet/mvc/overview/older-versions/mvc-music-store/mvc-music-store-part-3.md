---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Parte 3: exibições e ViewModels | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 3 aborda exibições e ViewModels.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3fcfc816cde22c697a78bab2c9ea7ace1bf68501
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559804"
---
# <a name="part-3-views-and-viewmodels"></a><span data-ttu-id="cde37-104">Parte 3: exibições e ViewModels</span><span class="sxs-lookup"><span data-stu-id="cde37-104">Part 3: Views and ViewModels</span></span>

<span data-ttu-id="cde37-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="cde37-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="cde37-106">A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.</span><span class="sxs-lookup"><span data-stu-id="cde37-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="cde37-107">A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="cde37-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="cde37-108">Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cde37-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="cde37-109">A parte 3 aborda exibições e ViewModels.</span><span class="sxs-lookup"><span data-stu-id="cde37-109">Part 3 covers Views and ViewModels.</span></span>

<span data-ttu-id="cde37-110">Até agora, acabamos retornando cadeias de caracteres de ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="cde37-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="cde37-111">Essa é uma boa maneira de ter uma ideia de como os controladores funcionam, mas não é como você desejaria criar um aplicativo Web real.</span><span class="sxs-lookup"><span data-stu-id="cde37-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="cde37-112">Vamos querer uma maneira melhor de gerar o HTML de volta para os navegadores visitando nosso site – um em que podemos usar arquivos de modelo para personalizar mais facilmente o envio de conteúdo HTML.</span><span class="sxs-lookup"><span data-stu-id="cde37-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="cde37-113">Isso é exatamente o que as exibições fazem.</span><span class="sxs-lookup"><span data-stu-id="cde37-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="cde37-114">Adicionando um modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="cde37-114">Adding a View template</span></span>

<span data-ttu-id="cde37-115">Para usar um View-template, vamos alterar o método de índice HomeController para retornar um ActionResult e fazer com que ele retorne View (), como abaixo:</span><span class="sxs-lookup"><span data-stu-id="cde37-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="cde37-116">A alteração acima indica que, em vez de retornar uma cadeia de caracteres, queremos usar uma "exibição" para gerar um resultado.</span><span class="sxs-lookup"><span data-stu-id="cde37-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="cde37-117">Agora vamos adicionar um modelo de exibição apropriado ao nosso projeto.</span><span class="sxs-lookup"><span data-stu-id="cde37-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="cde37-118">Para fazer isso, vamos posicionar o cursor de texto dentro do método de ação de índice, clicar com o botão direito do mouse e selecionar "Add View".</span><span class="sxs-lookup"><span data-stu-id="cde37-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="cde37-119">Isso abrirá a caixa de diálogo Adicionar exibição:</span><span class="sxs-lookup"><span data-stu-id="cde37-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="cde37-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="cde37-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="cde37-121">A caixa de diálogo "Adicionar exibição" nos permite gerar com rapidez e facilidade arquivos de modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="cde37-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="cde37-122">Por padrão, a caixa de diálogo "Adicionar exibição" popula previamente o nome do modelo de exibição a ser criado para que ele corresponda ao método de ação que o usará.</span><span class="sxs-lookup"><span data-stu-id="cde37-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="cde37-123">Como usamos o menu de contexto "Add View" dentro do método de ação index () de nosso HomeController, a caixa de diálogo "Add View" acima tem "index" como o nome de exibição preenchido previamente por padrão.</span><span class="sxs-lookup"><span data-stu-id="cde37-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="cde37-124">Não precisamos alterar nenhuma das opções nessa caixa de diálogo, portanto, clique no botão Adicionar.</span><span class="sxs-lookup"><span data-stu-id="cde37-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="cde37-125">Quando clicamos no botão Adicionar, o Visual Web Developer criará um novo modelo de exibição index. cshtml para nós no diretório \Views\Home, criando a pasta se ela ainda não existir.</span><span class="sxs-lookup"><span data-stu-id="cde37-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="cde37-126">O nome e o local da pasta do arquivo "index. cshtml" são importantes e seguem as convenções de nomenclatura ASP.NET MVC padrão.</span><span class="sxs-lookup"><span data-stu-id="cde37-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="cde37-127">O nome do diretório, \Views\Home, corresponde ao controlador, que é chamado de HomeController.</span><span class="sxs-lookup"><span data-stu-id="cde37-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="cde37-128">O nome do modelo de exibição, índice, corresponde ao método de ação do controlador que exibirá a exibição.</span><span class="sxs-lookup"><span data-stu-id="cde37-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="cde37-129">O ASP.NET MVC nos permite evitar ter que especificar explicitamente o nome ou o local de um modelo de exibição quando usamos essa Convenção de nomenclatura para retornar uma exibição.</span><span class="sxs-lookup"><span data-stu-id="cde37-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="cde37-130">Por padrão, ele renderizará o modelo de exibição \Views\Home\Index.cshtml quando escrevermos código como abaixo em nosso HomeController:</span><span class="sxs-lookup"><span data-stu-id="cde37-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="cde37-131">O Visual Web Developer criou e abriu o modelo de exibição "index. cshtml" depois que clicamos no botão "Adicionar" na caixa de diálogo "Adicionar exibição".</span><span class="sxs-lookup"><span data-stu-id="cde37-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="cde37-132">O conteúdo de index. cshtml é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="cde37-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="cde37-133">Essa exibição está usando o sintaxe Razor, que é mais conciso do que o mecanismo de exibição Web Forms usado em ASP.NET Web Forms e versões anteriores do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="cde37-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="cde37-134">O mecanismo de exibição de Web Forms ainda está disponível no ASP.NET MVC 3, mas muitos desenvolvedores acham que o mecanismo de exibição do Razor se adapta muito bem ao desenvolvimento do MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cde37-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="cde37-135">As três primeiras linhas definem o título da página usando ViewBag. Title.</span><span class="sxs-lookup"><span data-stu-id="cde37-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="cde37-136">Vamos examinar como isso funciona em mais detalhes em breve, mas primeiro vamos atualizar o texto do cabeçalho do texto e exibir a página.</span><span class="sxs-lookup"><span data-stu-id="cde37-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="cde37-137">Atualize a marca &lt;H2&gt; para dizer "esta é a Home Page", conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="cde37-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="cde37-138">A execução do aplicativo mostra que nosso novo texto está visível na home page.</span><span class="sxs-lookup"><span data-stu-id="cde37-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="cde37-139">Usando um layout para elementos de site comuns</span><span class="sxs-lookup"><span data-stu-id="cde37-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="cde37-140">A maioria dos sites tem conteúdo que é compartilhado entre muitas páginas: navegação, rodapés, imagens de logotipo, referências de folha de estilos, etc. O mecanismo de exibição do Razor facilita o gerenciamento usando uma página chamada \_layout. cshtml, que foi criada automaticamente para nós dentro da pasta/Views/Shared.</span><span class="sxs-lookup"><span data-stu-id="cde37-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="cde37-141">Clique duas vezes nessa pasta para exibir o conteúdo, que é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="cde37-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="cde37-142">O conteúdo de nossos modos de exibição individuais será exibido pelo comando @RenderBody() e qualquer conteúdo comum que desejarmos aparecer fora dele poderá ser adicionado à marcação \_layout. cshtml.</span><span class="sxs-lookup"><span data-stu-id="cde37-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="cde37-143">Vamos querer que nossa loja de música MVC tenha um cabeçalho comum com links para nossa home page e área de armazenamento em todas as páginas do site, então vamos adicioná-la ao modelo diretamente acima dessa instrução @RenderBody().</span><span class="sxs-lookup"><span data-stu-id="cde37-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="cde37-144">Atualizando a folha de estilos</span><span class="sxs-lookup"><span data-stu-id="cde37-144">Updating the StyleSheet</span></span>

<span data-ttu-id="cde37-145">O modelo de projeto vazio inclui um arquivo CSS muito simplificado que inclui apenas estilos usados para exibir mensagens de validação.</span><span class="sxs-lookup"><span data-stu-id="cde37-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="cde37-146">Nosso designer forneceu algumas CSS e imagens adicionais para definir a aparência para nosso site, portanto, vamos adicioná-las agora.</span><span class="sxs-lookup"><span data-stu-id="cde37-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="cde37-147">O arquivo CSS atualizado e as imagens são incluídos no diretório de conteúdo do MvcMusicStore-Assets. zip que está disponível em [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="cde37-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="cde37-148">Vamos selecionar ambos no Windows Explorer e soltá-los na pasta de conteúdo da nossa solução no Visual Web Developer, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="cde37-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="cde37-149">Você será solicitado a confirmar se deseja substituir o arquivo site. css existente.</span><span class="sxs-lookup"><span data-stu-id="cde37-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="cde37-150">Clique em Sim.</span><span class="sxs-lookup"><span data-stu-id="cde37-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="cde37-151">A pasta de conteúdo do seu aplicativo agora será exibida da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="cde37-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="cde37-152">Agora, vamos executar o aplicativo e ver como nossas alterações são examinadas na Home Page.</span><span class="sxs-lookup"><span data-stu-id="cde37-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="cde37-153">Vamos examinar o que mudou: o método de ação de índice do HomeController encontrou e exibiu o modelo \Views\Home\Index.cshtmlView, embora nosso código tenha chamado "Return View ()", porque nosso modelo de exibição seguiu a Convenção de nomenclatura padrão.</span><span class="sxs-lookup"><span data-stu-id="cde37-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="cde37-154">A Home Page está exibindo uma mensagem de boas-vindas simples que é definida no modelo de exibição \Views\Home\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="cde37-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="cde37-155">A Home Page está usando nosso modelo \_layout. cshtml e, portanto, a mensagem de boas-vindas está contida no layout HTML do site padrão.</span><span class="sxs-lookup"><span data-stu-id="cde37-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="cde37-156">Usando um modelo para passar informações para nossa exibição</span><span class="sxs-lookup"><span data-stu-id="cde37-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="cde37-157">Um modelo de exibição que apenas exibe HTML embutido em código não fará um site muito interessante.</span><span class="sxs-lookup"><span data-stu-id="cde37-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="cde37-158">Para criar um site dinâmico, vamos querer passar informações de nossas ações do controlador para nossos modelos de exibição.</span><span class="sxs-lookup"><span data-stu-id="cde37-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="cde37-159">No padrão Model-View-Controller, o termo Model refere-se a objetos que representam os dados no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cde37-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="cde37-160">Geralmente, objetos de modelo correspondem a tabelas em seu banco de dados, mas não precisam.</span><span class="sxs-lookup"><span data-stu-id="cde37-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="cde37-161">Os métodos de ação do controlador que retornam um ActionResult podem passar um objeto de modelo para a exibição.</span><span class="sxs-lookup"><span data-stu-id="cde37-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="cde37-162">Isso permite que um controlador empacote corretamente todas as informações necessárias para gerar uma resposta e, em seguida, passe essas informações para um modelo de exibição a ser usado para gerar a resposta HTML apropriada.</span><span class="sxs-lookup"><span data-stu-id="cde37-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="cde37-163">Isso é mais fácil de entender ao vê-lo em ação, então vamos começar.</span><span class="sxs-lookup"><span data-stu-id="cde37-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="cde37-164">Primeiro, criaremos algumas classes de modelo para representar gêneros e álbuns em nossa loja.</span><span class="sxs-lookup"><span data-stu-id="cde37-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="cde37-165">Vamos começar criando uma classe de gênero.</span><span class="sxs-lookup"><span data-stu-id="cde37-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="cde37-166">Clique com o botão direito do mouse na pasta "modelos" em seu projeto, escolha a opção "Adicionar classe" e nomeie o arquivo como "Genre.cs".</span><span class="sxs-lookup"><span data-stu-id="cde37-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="cde37-167">Em seguida, adicione uma propriedade de nome de cadeia de caracteres pública à classe que foi criada:</span><span class="sxs-lookup"><span data-stu-id="cde37-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="cde37-168">*Observação: caso você esteja imaginando, a notação {Get; Set;} está usando o recurso C#de propriedades implementadas automaticamente. Isso nos dá os benefícios de uma propriedade sem exigir que declaremos um campo de apoio.*</span><span class="sxs-lookup"><span data-stu-id="cde37-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="cde37-169">Em seguida, siga as mesmas etapas para criar uma classe de álbum (chamada Album.cs) que tem um título e uma propriedade de Gênero:</span><span class="sxs-lookup"><span data-stu-id="cde37-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="cde37-170">Agora, podemos modificar o StoreController para usar modos de exibição que exibem informações dinâmicas de nosso modelo.</span><span class="sxs-lookup"><span data-stu-id="cde37-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="cde37-171">Se-para fins de demonstração no momento, chamamos nossos álbuns com base na ID da solicitação, poderíamos exibir essas informações como na exibição abaixo.</span><span class="sxs-lookup"><span data-stu-id="cde37-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="cde37-172">Vamos começar alterando a ação armazenar detalhes para que ela mostre as informações de um único álbum.</span><span class="sxs-lookup"><span data-stu-id="cde37-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="cde37-173">Adicione uma instrução "using" à parte superior da classe **StoreControllers** para incluir o namespace MvcMusicStore. Models, portanto, não precisamos digitar MvcMusicStore. Models. Album toda vez que quisermos usar a classe Album.</span><span class="sxs-lookup"><span data-stu-id="cde37-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="cde37-174">A seção "Usings" dessa classe agora deve aparecer como abaixo.</span><span class="sxs-lookup"><span data-stu-id="cde37-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="cde37-175">Em seguida, atualizaremos a ação do controlador de detalhes para que ela retorne um ActionResult em vez de uma cadeia de caracteres, como fizemos com o método de índice de HomeController.</span><span class="sxs-lookup"><span data-stu-id="cde37-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="cde37-176">Agora, podemos modificar a lógica para retornar um objeto de álbum para a exibição.</span><span class="sxs-lookup"><span data-stu-id="cde37-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="cde37-177">Posteriormente neste tutorial, recuperaremos os dados de um banco de dado – mas, no momento, usaremos "dados fictícios" para começar.</span><span class="sxs-lookup"><span data-stu-id="cde37-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="cde37-178">*Observação: se você não estiver familiarizado com C#o, poderá pressupor que o uso de var significa que nossa variável de álbum é de associação tardia. Isso não está correto – o C# compilador está usando a inferência de tipos com base no que estamos atribuindo à variável para determinar se o álbum é do tipo álbum e compilando a variável de álbum local como um tipo de álbum, portanto, obtemos a verificação de tempo de compilação e o suporte ao editor de código do Visual Studio.*</span><span class="sxs-lookup"><span data-stu-id="cde37-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="cde37-179">Agora, vamos criar um modelo de exibição que usa nosso álbum para gerar uma resposta HTML.</span><span class="sxs-lookup"><span data-stu-id="cde37-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="cde37-180">Antes de fazermos isso, precisamos criar o projeto para que a caixa de diálogo Adicionar exibição saiba sobre nossa classe de álbum recém-criada.</span><span class="sxs-lookup"><span data-stu-id="cde37-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="cde37-181">Você pode criar o projeto selecionando o item de menu Debug ⇨ Build MvcMusicStore (para crédito extra, você pode usar o atalho Ctrl-Shift-B para criar o projeto).</span><span class="sxs-lookup"><span data-stu-id="cde37-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="cde37-182">Agora que configuramos nossas classes de suporte, estamos prontos para criar nosso modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="cde37-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="cde37-183">Clique com o botão direito do mouse no método Details e selecione "Add View..." no menu de contexto.</span><span class="sxs-lookup"><span data-stu-id="cde37-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="cde37-184">Vamos criar um novo modelo de exibição como fizemos antes com o HomeController.</span><span class="sxs-lookup"><span data-stu-id="cde37-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="cde37-185">Como estamos criando a partir do StoreController, ele será gerado por padrão em um arquivo \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="cde37-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="cde37-186">Ao contrário de antes, vamos marcar a caixa de seleção de exibição "criar um fortemente tipado".</span><span class="sxs-lookup"><span data-stu-id="cde37-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="cde37-187">Em seguida, vamos selecionar nossa classe "Album" na lista suspensa "View Data-Class".</span><span class="sxs-lookup"><span data-stu-id="cde37-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="cde37-188">Isso fará com que a caixa de diálogo "Adicionar exibição" Crie um modelo de exibição que espera que um objeto de álbum será passado para ele para uso.</span><span class="sxs-lookup"><span data-stu-id="cde37-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="cde37-189">Quando clicamos no botão "Adicionar", nosso modelo de exibição \Views\Store\Details.cshtml será criado, contendo o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="cde37-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="cde37-190">Observe a primeira linha, que indica que essa exibição é fortemente tipada para nossa classe de álbum.</span><span class="sxs-lookup"><span data-stu-id="cde37-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="cde37-191">O mecanismo de exibição do Razor entende que passou um objeto de álbum, portanto, podemos acessar facilmente as propriedades do modelo e até mesmo ter o benefício do IntelliSense no editor do Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="cde37-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="cde37-192">Atualize a marca &lt;H2&gt; para que ela exiba a propriedade Title do álbum, modificando essa linha para que ela apareça da seguinte maneira.</span><span class="sxs-lookup"><span data-stu-id="cde37-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="cde37-193">Observe que o IntelliSense é disparado quando você insere o período após a palavra-chave @Model, mostrando as propriedades e os métodos aos quais a classe de álbum dá suporte.</span><span class="sxs-lookup"><span data-stu-id="cde37-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="cde37-194">Agora, vamos executar novamente o nosso projeto e visitar a URL do/Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="cde37-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="cde37-195">Veremos os detalhes de um álbum como abaixo.</span><span class="sxs-lookup"><span data-stu-id="cde37-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="cde37-196">Agora, vamos fazer uma atualização semelhante para o método de ação da loja de procura.</span><span class="sxs-lookup"><span data-stu-id="cde37-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="cde37-197">Atualize o método para que ele retorne um ActionResult e modifique a lógica do método para que ele crie um novo objeto de gênero e o retorne ao modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="cde37-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="cde37-198">Clique com o botão direito do mouse no método procurar e selecione "Adicionar exibição..." no menu de contexto, adicione uma exibição fortemente tipada adicione um tipo fortemente tipado à classe gênero.</span><span class="sxs-lookup"><span data-stu-id="cde37-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="cde37-199">Atualize o elemento &lt;H2&gt; no código de exibição (em/Views/Store/Browse.cshtml) para exibir as informações de gênero.</span><span class="sxs-lookup"><span data-stu-id="cde37-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="cde37-200">Agora, vamos executar novamente o nosso projeto e navegar até o/Store/Browse? Gênero = URL de disco.</span><span class="sxs-lookup"><span data-stu-id="cde37-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="cde37-201">Veremos a página de navegação exibida como abaixo.</span><span class="sxs-lookup"><span data-stu-id="cde37-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="cde37-202">Por fim, vamos fazer uma atualização um pouco mais complexa para o método de ação de **índice de loja** e exibir para exibir uma lista de todos os gêneros em nossa loja.</span><span class="sxs-lookup"><span data-stu-id="cde37-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="cde37-203">Faremos isso usando uma lista de gêneros como nosso objeto de modelo, em vez de apenas um único gênero.</span><span class="sxs-lookup"><span data-stu-id="cde37-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="cde37-204">Clique com o botão direito do mouse no método de ação armazenar índice e selecione Adicionar exibição como antes, selecione gênero como a classe modelo e pressione o botão Adicionar.</span><span class="sxs-lookup"><span data-stu-id="cde37-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="cde37-205">Primeiro, alteraremos a declaração de @model para indicar que a exibição estará esperando vários objetos gênero em vez de apenas um.</span><span class="sxs-lookup"><span data-stu-id="cde37-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="cde37-206">Altere a primeira linha de/Store/Index.cshtml para ler da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="cde37-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="cde37-207">Isso informa ao mecanismo de exibição do Razor que ele estará trabalhando com um objeto de modelo que pode conter vários objetos gênero.</span><span class="sxs-lookup"><span data-stu-id="cde37-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="cde37-208">Estamos usando um gênero IEnumerable&lt;&gt; em vez de uma lista&lt;gênero&gt; porque é mais genérica, permitindo alterar nosso tipo de modelo posteriormente para qualquer tipo de objeto que dê suporte à interface IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="cde37-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="cde37-209">Em seguida, vamos executar um loop nos objetos gênero no modelo, conforme mostrado no código de exibição concluído abaixo.</span><span class="sxs-lookup"><span data-stu-id="cde37-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="cde37-210">Observe que temos suporte total ao IntelliSense enquanto inserimos esse código, para que, quando digitarmos "@Model".</span><span class="sxs-lookup"><span data-stu-id="cde37-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="cde37-211">vemos todos os métodos e propriedades com suporte de um IEnumerable do tipo gênero.</span><span class="sxs-lookup"><span data-stu-id="cde37-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="cde37-212">Em nosso loop "foreach", o Visual Web Developer sabe que cada item é do tipo gênero, portanto, vemos o IntelliSense para cada tipo de gênero.</span><span class="sxs-lookup"><span data-stu-id="cde37-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSense for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="cde37-213">Em seguida, o recurso scaffolding examinou o objeto gênero e determinou que cada um terá uma propriedade Name, portanto, ele faz o loop e os grava. Ele também gera links de edição, detalhes e exclusão para cada item individual.</span><span class="sxs-lookup"><span data-stu-id="cde37-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="cde37-214">Aproveitaremos isso mais tarde em nosso Store Manager, mas, por ora, gostaríamos de ter uma lista simples em vez disso.</span><span class="sxs-lookup"><span data-stu-id="cde37-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="cde37-215">Quando executamos o aplicativo e navegamos até/Store, vemos que a contagem e a lista de gêneros são exibidas.</span><span class="sxs-lookup"><span data-stu-id="cde37-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="cde37-216">Adicionando links entre páginas</span><span class="sxs-lookup"><span data-stu-id="cde37-216">Adding Links between pages</span></span>

<span data-ttu-id="cde37-217">Nossa URL/Store que lista os gêneros atualmente lista os nomes de gênero simplesmente como texto sem formatação.</span><span class="sxs-lookup"><span data-stu-id="cde37-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="cde37-218">Vamos alterar isso para que, em vez de texto sem formatação, tenhamos o link nomes de gênero para a URL/Store/Browse apropriada, de forma que clicar em um gênero musical como "disco" navegará para a URL/Store/Browse? gênero = disco.</span><span class="sxs-lookup"><span data-stu-id="cde37-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="cde37-219">Poderíamos atualizar nosso modelo de exibição \Views\Store\Index.cshtml para gerar esses links usando um código como abaixo **(não digite isso em – vamos melhorar isso)** :</span><span class="sxs-lookup"><span data-stu-id="cde37-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="cde37-220">Isso funciona, mas poderia causar problemas mais tarde, pois depende de uma cadeia de caracteres codificada.</span><span class="sxs-lookup"><span data-stu-id="cde37-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="cde37-221">Por exemplo, se quiséssemos renomear o controlador, precisaremos Pesquisar nosso código procurando links que precisam ser atualizados.</span><span class="sxs-lookup"><span data-stu-id="cde37-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="cde37-222">Uma abordagem alternativa que podemos usar é aproveitar um método auxiliar HTML.</span><span class="sxs-lookup"><span data-stu-id="cde37-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="cde37-223">O ASP.NET MVC inclui métodos auxiliares HTML que estão disponíveis em nosso código de modelo de exibição para executar uma variedade de tarefas comuns, assim como essa.</span><span class="sxs-lookup"><span data-stu-id="cde37-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="cde37-224">O método auxiliar HTML. ActionLink () é especialmente útil e facilita a criação de HTML &lt;um&gt; links e cuida de detalhes incômodos, como garantir que os caminhos de URL sejam codificados corretamente em URL.</span><span class="sxs-lookup"><span data-stu-id="cde37-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="cde37-225">HTML. ActionLink () tem várias sobrecargas diferentes para permitir a especificação de quantas informações forem necessárias para seus links.</span><span class="sxs-lookup"><span data-stu-id="cde37-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="cde37-226">No caso mais simples, você fornecerá apenas o texto do link e o método de ação para ir quando o hiperlink for clicado no cliente.</span><span class="sxs-lookup"><span data-stu-id="cde37-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="cde37-227">Por exemplo, podemos vincular ao método "/Store/" index () na página de detalhes do repositório com o texto do link "go to the Store index" usando a seguinte chamada:</span><span class="sxs-lookup"><span data-stu-id="cde37-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="cde37-228">*Observação: nesse caso, não precisamos especificar o nome do controlador porque estamos apenas vinculando a outra ação dentro do mesmo controlador que está renderizando a exibição atual.*</span><span class="sxs-lookup"><span data-stu-id="cde37-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="cde37-229">Os links para a página de navegação precisarão passar um parâmetro, mas, portanto, usaremos outra sobrecarga do método html. ActionLink que usa três parâmetros:</span><span class="sxs-lookup"><span data-stu-id="cde37-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="cde37-230">Texto do link, que exibirá o nome do gênero</span><span class="sxs-lookup"><span data-stu-id="cde37-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="cde37-231">Nome da ação do controlador (procurar)</span><span class="sxs-lookup"><span data-stu-id="cde37-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="cde37-232">Rotear valores de parâmetro, especificando o nome (gênero) e o valor (nome do gênero)</span><span class="sxs-lookup"><span data-stu-id="cde37-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="cde37-233">Colocando tudo isso, veja como escreveremos esses links para a exibição do índice de loja:</span><span class="sxs-lookup"><span data-stu-id="cde37-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="cde37-234">Agora, quando executamos nosso projeto novamente e acessamos a URL/Store/, veremos uma lista de gêneros.</span><span class="sxs-lookup"><span data-stu-id="cde37-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="cde37-235">Cada gênero é um hiperlink – quando clicado, ele nos levará à nossa URL/Store/Browse? gênero = *[gênero]* .</span><span class="sxs-lookup"><span data-stu-id="cde37-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="cde37-236">O HTML da lista de gêneros é semelhante a este:</span><span class="sxs-lookup"><span data-stu-id="cde37-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]

> [!div class="step-by-step"]
> <span data-ttu-id="cde37-237">[Anterior](mvc-music-store-part-2.md)
> [Próximo](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="cde37-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
