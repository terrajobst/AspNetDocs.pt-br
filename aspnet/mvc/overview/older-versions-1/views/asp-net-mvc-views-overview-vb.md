---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
title: Visão geral das exibições do ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: O que é uma exibição do ASP.NET MVC e como ela difere de uma página HTML? Neste tutorial, Stephen Walther apresenta exibições e demonstra como você não pode...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: c28ba88d-3a93-47f5-a306-049bd766714d
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: f02728ed248f29b09d654e509977ed43889cbb83
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541303"
---
# <a name="aspnet-mvc-views-overview-vb"></a><span data-ttu-id="7e603-104">Visão geral de exibições do ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="7e603-104">ASP.NET MVC Views Overview (VB)</span></span>

<span data-ttu-id="7e603-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="7e603-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="7e603-106">O que é uma exibição do ASP.NET MVC e como ela difere de uma página HTML?</span><span class="sxs-lookup"><span data-stu-id="7e603-106">What is an ASP.NET MVC View and how does it differ from a HTML page?</span></span> <span data-ttu-id="7e603-107">Neste tutorial, Stephen Walther apresenta exibições e demonstra como você pode tirar proveito da exibição de dados e auxiliares HTML em uma exibição.</span><span class="sxs-lookup"><span data-stu-id="7e603-107">In this tutorial, Stephen Walther introduces you to Views and demonstrates how you can take advantage of View Data and HTML Helpers within a View.</span></span>

<span data-ttu-id="7e603-108">A finalidade deste tutorial é fornecer uma breve introdução às exibições do ASP.NET MVC, exibir dados e auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="7e603-108">The purpose of this tutorial is to provide you with a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="7e603-109">Ao final deste tutorial, você deve entender como criar novos modos de exibição, passar dados de um controlador para uma visualização e usar auxiliares HTML para gerar conteúdo em uma exibição.</span><span class="sxs-lookup"><span data-stu-id="7e603-109">By the end of this tutorial, you should understand how to create new views, pass data from a controller to a view, and use HTML Helpers to generate content in a view.</span></span>

## <a name="understanding-views"></a><span data-ttu-id="7e603-110">Compreendendo as exibições</span><span class="sxs-lookup"><span data-stu-id="7e603-110">Understanding Views</span></span>

<span data-ttu-id="7e603-111">Ao contrário das páginas ASP.NET ou Active Server, o ASP.NET MVC não inclui nada que corresponda diretamente a uma página.</span><span class="sxs-lookup"><span data-stu-id="7e603-111">Unlike ASP.NET or Active Server Pages, ASP.NET MVC does not include anything that directly corresponds to a page.</span></span> <span data-ttu-id="7e603-112">Em um aplicativo MVC ASP.NET, não há uma página em disco que corresponda ao caminho na URL que você digitar na barra de endereços do seu navegador.</span><span class="sxs-lookup"><span data-stu-id="7e603-112">In an ASP.NET MVC application, there is not a page on disk that corresponds to the path in the URL that you type into the address bar of your browser.</span></span> <span data-ttu-id="7e603-113">A coisa mais próxima de uma página em um aplicativo MVC ASP.NET é algo chamado de *exibição*.</span><span class="sxs-lookup"><span data-stu-id="7e603-113">The closest thing to a page in an ASP.NET MVC application is something called a *view*.</span></span>

<span data-ttu-id="7e603-114">Em um aplicativo MVC do ASP.NET, as solicitações de entrada do navegador são mapeadas para ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="7e603-114">In an ASP.NET MVC application, incoming browser requests are mapped to controller actions.</span></span> <span data-ttu-id="7e603-115">Uma ação do controlador pode retornar uma exibição.</span><span class="sxs-lookup"><span data-stu-id="7e603-115">A controller action might return a view.</span></span> <span data-ttu-id="7e603-116">No entanto, uma ação do controlador pode executar algum outro tipo de ação, como redirecionar você para outra ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="7e603-116">However, a controller action might perform some other type of action such as redirecting you to another controller action.</span></span>

<span data-ttu-id="7e603-117">A listagem 1 contém um controlador simples chamado HomeController.</span><span class="sxs-lookup"><span data-stu-id="7e603-117">Listing 1 contains a simple controller named the HomeController.</span></span> <span data-ttu-id="7e603-118">O HomeController expõe duas ações de controlador denominadas index () e Details ().</span><span class="sxs-lookup"><span data-stu-id="7e603-118">The HomeController exposes two controller actions named Index() and Details().</span></span>

<span data-ttu-id="7e603-119">**Listagem 1-HomeController. vb**</span><span class="sxs-lookup"><span data-stu-id="7e603-119">**Listing 1 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample1.vb)]

<span data-ttu-id="7e603-120">Você pode invocar a primeira ação, a ação de índice (), digitando a seguinte URL na barra de endereços do navegador:</span><span class="sxs-lookup"><span data-stu-id="7e603-120">You can invoke the first action, the Index() action, by typing the following URL into your browser address bar:</span></span>

<span data-ttu-id="7e603-121">/Home/Index</span><span class="sxs-lookup"><span data-stu-id="7e603-121">/Home/Index</span></span>

<span data-ttu-id="7e603-122">Você pode invocar a segunda ação, a ação detalhes (), digitando esse endereço em seu navegador:</span><span class="sxs-lookup"><span data-stu-id="7e603-122">You can invoke the second action, the Details() action, by typing this address into your browser:</span></span>

<span data-ttu-id="7e603-123">/Home/Details</span><span class="sxs-lookup"><span data-stu-id="7e603-123">/Home/Details</span></span>

<span data-ttu-id="7e603-124">A ação index () retorna uma exibição.</span><span class="sxs-lookup"><span data-stu-id="7e603-124">The Index() action returns a view.</span></span> <span data-ttu-id="7e603-125">A maioria das ações que você criar retornará exibições.</span><span class="sxs-lookup"><span data-stu-id="7e603-125">Most actions that you create will return views.</span></span> <span data-ttu-id="7e603-126">No entanto, uma ação pode retornar outros tipos de resultados de ação.</span><span class="sxs-lookup"><span data-stu-id="7e603-126">However, an action can return other types of action results.</span></span> <span data-ttu-id="7e603-127">Por exemplo, a ação Details () retorna um RedirectToActionResult que redireciona a solicitação de entrada para a ação index ().</span><span class="sxs-lookup"><span data-stu-id="7e603-127">For example, the Details() action returns a RedirectToActionResult that redirects incoming request to the Index() action.</span></span>

<span data-ttu-id="7e603-128">A ação index () contém a seguinte linha única de código:</span><span class="sxs-lookup"><span data-stu-id="7e603-128">The Index() action contains the following single line of code:</span></span>

<span data-ttu-id="7e603-129">Exibir ()</span><span class="sxs-lookup"><span data-stu-id="7e603-129">View()</span></span>

<span data-ttu-id="7e603-130">Essa linha de código retorna uma exibição que deve estar localizada no seguinte caminho no seu servidor Web:</span><span class="sxs-lookup"><span data-stu-id="7e603-130">This line of code returns a view that must be located at the following path on your web server:</span></span>

<span data-ttu-id="7e603-131">\Views\Home\Index.aspx</span><span class="sxs-lookup"><span data-stu-id="7e603-131">\Views\Home\Index.aspx</span></span>

<span data-ttu-id="7e603-132">O caminho para a exibição é inferido a partir do nome do controlador e do nome da ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="7e603-132">The path to the view is inferred from the name of the controller and the name of the controller action.</span></span>

<span data-ttu-id="7e603-133">Se preferir, você pode ser explícito sobre a exibição.</span><span class="sxs-lookup"><span data-stu-id="7e603-133">If you prefer, you can be explicit about the view.</span></span> <span data-ttu-id="7e603-134">A linha de código a seguir retorna uma exibição chamada Fred:</span><span class="sxs-lookup"><span data-stu-id="7e603-134">The following line of code returns a view named Fred :</span></span>

<span data-ttu-id="7e603-135">Exibir (Fred)</span><span class="sxs-lookup"><span data-stu-id="7e603-135">View( Fred )</span></span>

<span data-ttu-id="7e603-136">Quando essa linha de código é executada, uma exibição é retornada do seguinte caminho:</span><span class="sxs-lookup"><span data-stu-id="7e603-136">When this line of code is executed, a view is returned from the following path:</span></span>

<span data-ttu-id="7e603-137">\Views\Home\Fred.aspx</span><span class="sxs-lookup"><span data-stu-id="7e603-137">\Views\Home\Fred.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="7e603-138">Se você planeja criar testes de unidade para seu aplicativo ASP.NET MVC, é uma boa ideia ser explícita sobre nomes de exibição.</span><span class="sxs-lookup"><span data-stu-id="7e603-138">If you plan to create unit tests for your ASP.NET MVC application then it is a good idea to be explicit about view names.</span></span> <span data-ttu-id="7e603-139">Dessa forma, você pode criar um teste de unidade para verificar se a exibição esperada foi retornada por uma ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="7e603-139">That way, you can create a unit test to verify that the expected view was returned by a controller action.</span></span>

## <a name="adding-content-to-a-view"></a><span data-ttu-id="7e603-140">Adicionando conteúdo a uma exibição</span><span class="sxs-lookup"><span data-stu-id="7e603-140">Adding Content to a View</span></span>

<span data-ttu-id="7e603-141">Uma exibição é um documento HTML padrão (X) que pode conter scripts.</span><span class="sxs-lookup"><span data-stu-id="7e603-141">A view is a standard (X)HTML document that can contain scripts.</span></span> <span data-ttu-id="7e603-142">Você usa scripts para adicionar conteúdo dinâmico a uma exibição.</span><span class="sxs-lookup"><span data-stu-id="7e603-142">You use scripts to add dynamic content to a view.</span></span>

<span data-ttu-id="7e603-143">Por exemplo, a exibição na Listagem 2 exibe a data e a hora atuais.</span><span class="sxs-lookup"><span data-stu-id="7e603-143">For example, the view in Listing 2 displays the current date and time.</span></span>

<span data-ttu-id="7e603-144">**Listagem 2-\Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="7e603-144">**Listing 2 - \Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample2.aspx)]

<span data-ttu-id="7e603-145">Observe que o corpo da página HTML na Listagem 2 contém o script a seguir:</span><span class="sxs-lookup"><span data-stu-id="7e603-145">Notice that the body of the HTML page in Listing 2 contains the following script:</span></span>

<span data-ttu-id="7e603-146">&lt;% Response. Write (DateTime. Now)%&gt;</span><span class="sxs-lookup"><span data-stu-id="7e603-146">&lt;% Response.Write(DateTime.Now)%&gt;</span></span>

<span data-ttu-id="7e603-147">Você usa os delimitadores de script &lt;% e%&gt; para marcar o início e o fim de um script.</span><span class="sxs-lookup"><span data-stu-id="7e603-147">You use the script delimiters &lt;% and %&gt; to mark the beginning and end of a script.</span></span> <span data-ttu-id="7e603-148">Esse script é escrito em Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="7e603-148">This script is written in Visual basic.</span></span> <span data-ttu-id="7e603-149">Ele exibe a data e a hora atuais chamando o método Response. Write () para renderizar o conteúdo para o navegador.</span><span class="sxs-lookup"><span data-stu-id="7e603-149">It displays the current date and time by calling the Response.Write() method to render content to the browser.</span></span> <span data-ttu-id="7e603-150">Os delimitadores de script &lt;% e%&gt; podem ser usados para executar uma ou mais instruções.</span><span class="sxs-lookup"><span data-stu-id="7e603-150">The script delimiters &lt;% and %&gt; can be used to execute one or more statements.</span></span>

<span data-ttu-id="7e603-151">Como você chama Response. Write () com frequência, a Microsoft fornece um atalho para chamar o método Response. Write ().</span><span class="sxs-lookup"><span data-stu-id="7e603-151">Since you call Response.Write() so often, Microsoft provides you with a shortcut for calling the Response.Write() method.</span></span> <span data-ttu-id="7e603-152">A exibição na Listagem 3 usa os delimitadores &lt;% = e%&gt; como um atalho para chamar Response. Write ().</span><span class="sxs-lookup"><span data-stu-id="7e603-152">The view in Listing 3 uses the delimiters &lt;%= and %&gt; as a shortcut for calling Response.Write().</span></span>

<span data-ttu-id="7e603-153">**Listagem 3-Views\Home\Index2.aspx**</span><span class="sxs-lookup"><span data-stu-id="7e603-153">**Listing 3 - Views\Home\Index2.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample3.aspx)]

<span data-ttu-id="7e603-154">Você pode usar qualquer linguagem .NET para gerar conteúdo dinâmico em uma exibição.</span><span class="sxs-lookup"><span data-stu-id="7e603-154">You can use any .NET language to generate dynamic content in a view.</span></span> <span data-ttu-id="7e603-155">Normalmente, você usa o Visual Basic .NET ou C# para escrever seus controladores e exibições.</span><span class="sxs-lookup"><span data-stu-id="7e603-155">Normally, you�ll use either Visual Basic .NET or C# to write your controllers and views.</span></span>

## <a name="using-html-helpers-to-generate-view-content"></a><span data-ttu-id="7e603-156">Usando auxiliares HTML para gerar conteúdo de exibição</span><span class="sxs-lookup"><span data-stu-id="7e603-156">Using HTML Helpers to Generate View Content</span></span>

<span data-ttu-id="7e603-157">Para facilitar a adição de conteúdo a uma exibição, você pode aproveitar algo chamado de *auxiliar HTML*.</span><span class="sxs-lookup"><span data-stu-id="7e603-157">To make it easier to add content to a view, you can take advantage of something called an *HTML Helper*.</span></span> <span data-ttu-id="7e603-158">Um auxiliar HTML, normalmente, é um método que gera uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="7e603-158">An HTML Helper, typically, is a method that generates a string.</span></span> <span data-ttu-id="7e603-159">Você pode usar os auxiliares HTML para gerar elementos HTML padrão, como caixas de Text, links, listas suspensas e caixas de listagem.</span><span class="sxs-lookup"><span data-stu-id="7e603-159">You can use HTML Helpers to generate standard HTML elements such as textboxes, links, dropdown lists, and list boxes.</span></span>

<span data-ttu-id="7e603-160">Por exemplo, a exibição na Listagem 4 aproveita os três auxiliares HTML--o BeginForm (), a caixa de texto () e a senha () auxiliares – para gerar um formulário de logon (consulte a Figura 1).</span><span class="sxs-lookup"><span data-stu-id="7e603-160">For example, the view in Listing 4 takes advantage of three HTML Helpers -- the BeginForm(), the TextBox() and Password() helpers -- to generate a Login form (see Figure 1).</span></span>

<span data-ttu-id="7e603-161">**Listagem 4--\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="7e603-161">**Listing 4 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample4.aspx)]

<span data-ttu-id="7e603-162">[![caixa de diálogo novo projeto](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7e603-162">[![The New Project dialog box](asp-net-mvc-views-overview-vb/_static/image1.jpg)](asp-net-mvc-views-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="7e603-163">**Figura 01**: um formulário de logon padrão ([clique para exibir a imagem em tamanho normal](asp-net-mvc-views-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="7e603-163">**Figure 01**: A standard Login form ([Click to view full-size image](asp-net-mvc-views-overview-vb/_static/image2.png))</span></span>

<span data-ttu-id="7e603-164">Todos os métodos de auxiliares HTML são chamados na propriedade HTML da exibição.</span><span class="sxs-lookup"><span data-stu-id="7e603-164">All of the HTML Helpers methods are called on the Html property of the view.</span></span> <span data-ttu-id="7e603-165">Por exemplo, você renderiza uma caixa de texto chamando o método html. TextBox ().</span><span class="sxs-lookup"><span data-stu-id="7e603-165">For example, you render a TextBox by calling the Html.TextBox() method.</span></span>

<span data-ttu-id="7e603-166">Observe que você usa os delimitadores de script &lt;% = e%&gt; ao chamar os auxiliares HTML. TextBox () e HTML. Password ().</span><span class="sxs-lookup"><span data-stu-id="7e603-166">Notice that you use the script delimiters &lt;%= and %&gt; when calling both the Html.TextBox() and Html.Password() helpers.</span></span> <span data-ttu-id="7e603-167">Esses auxiliares simplesmente retornam uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="7e603-167">These helpers simply return a string.</span></span> <span data-ttu-id="7e603-168">Você precisa chamar Response. Write () para renderizar a cadeia de caracteres para o navegador.</span><span class="sxs-lookup"><span data-stu-id="7e603-168">You need to call Response.Write() in order to render the string to the browser.</span></span>

<span data-ttu-id="7e603-169">O uso de métodos auxiliares HTML é opcional.</span><span class="sxs-lookup"><span data-stu-id="7e603-169">Using HTML Helper methods is optional.</span></span> <span data-ttu-id="7e603-170">Eles facilitam sua vida reduzindo a quantidade de HTML e script que você precisa escrever.</span><span class="sxs-lookup"><span data-stu-id="7e603-170">They make your life easier by reducing the amount of HTML and script that you need to write.</span></span> <span data-ttu-id="7e603-171">A exibição na listagem 5 renderiza exatamente a mesma forma que a exibição na Listagem 4 sem usar auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="7e603-171">The view in Listing 5 renders the exact same form as the view in Listing 4 without using HTML Helpers.</span></span>

<span data-ttu-id="7e603-172">**Listagem 5--\Views\Home\Login.aspx**</span><span class="sxs-lookup"><span data-stu-id="7e603-172">**Listing 5 -- \Views\Home\Login.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample5.aspx)]

<span data-ttu-id="7e603-173">Você também tem a opção de criar seus próprios auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="7e603-173">You also have the option of creating your own HTML Helpers.</span></span> <span data-ttu-id="7e603-174">Por exemplo, você pode criar um método auxiliar GridView () que exibe um conjunto de registros de banco de dados em uma tabela HTML automaticamente.</span><span class="sxs-lookup"><span data-stu-id="7e603-174">For example, you can create a GridView() helper method that displays a set of database records in an HTML table automatically.</span></span> <span data-ttu-id="7e603-175">Exploramos este tópico no tutorial **criando auxiliares HTML personalizados**.</span><span class="sxs-lookup"><span data-stu-id="7e603-175">We explore this topic in the tutorial **Creating Custom HTML Helpers**.</span></span>

## <a name="using-view-data-to-pass-data-to-a-view"></a><span data-ttu-id="7e603-176">Usando dados de exibição para passar dados para uma exibição</span><span class="sxs-lookup"><span data-stu-id="7e603-176">Using View Data to Pass Data to a View</span></span>

<span data-ttu-id="7e603-177">Você usa exibir dados para passar dados de um controlador para um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="7e603-177">You use view data to pass data from a controller to a view.</span></span> <span data-ttu-id="7e603-178">Considere exibir dados como um pacote enviado por email.</span><span class="sxs-lookup"><span data-stu-id="7e603-178">Think of view data like a package that you send through the mail.</span></span> <span data-ttu-id="7e603-179">Todos os dados passados de um controlador para uma exibição devem ser enviados usando este pacote.</span><span class="sxs-lookup"><span data-stu-id="7e603-179">All data passed from a controller to a view must be sent using this package.</span></span> <span data-ttu-id="7e603-180">Por exemplo, o controlador na Listagem 6 adiciona uma mensagem para exibir dados.</span><span class="sxs-lookup"><span data-stu-id="7e603-180">For example, the controller in Listing 6 adds a message to view data.</span></span>

<span data-ttu-id="7e603-181">**Listagem 6-ProductController. vb**</span><span class="sxs-lookup"><span data-stu-id="7e603-181">**Listing 6 - ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-views-overview-vb/samples/sample6.vb)]

<span data-ttu-id="7e603-182">A propriedade ViewData do controlador representa uma coleção de pares de nome e valor.</span><span class="sxs-lookup"><span data-stu-id="7e603-182">The controller ViewData property represents a collection of name and value pairs.</span></span> <span data-ttu-id="7e603-183">Na Listagem 6, o método index () adiciona um item à exibição da coleção de dados denominada Message com o valor Olá, Mundo!.</span><span class="sxs-lookup"><span data-stu-id="7e603-183">In Listing 6, the Index() method adds an item to the view data collection named message with the value Hello World!.</span></span> <span data-ttu-id="7e603-184">Quando o modo de exibição é retornado pelo método index (), os dados de exibição são passados para a exibição automaticamente.</span><span class="sxs-lookup"><span data-stu-id="7e603-184">When the view is returned by the Index() method, the view data is passed to the view automatically.</span></span>

<span data-ttu-id="7e603-185">A exibição na Listagem 7 recupera a mensagem do modo de exibição dados e renderiza a mensagem para o navegador.</span><span class="sxs-lookup"><span data-stu-id="7e603-185">The view in Listing 7 retrieves the message from the view data and renders the message to the browser.</span></span>

<span data-ttu-id="7e603-186">**Listagem 7--\Views\Product\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="7e603-186">**Listing 7 -- \Views\Product\Index.aspx**</span></span>

[!code-aspx[Main](asp-net-mvc-views-overview-vb/samples/sample7.aspx)]

<span data-ttu-id="7e603-187">Observe que a exibição aproveita o método auxiliar HTML HTML. Encode () ao renderizar a mensagem.</span><span class="sxs-lookup"><span data-stu-id="7e603-187">Notice that the view takes advantage of the Html.Encode() HTML Helper method when rendering the message.</span></span> <span data-ttu-id="7e603-188">O auxiliar HTML HTML. Encode () codifica caracteres especiais, como &lt; e &gt; em caracteres que são seguros para serem exibidos em uma página da Web.</span><span class="sxs-lookup"><span data-stu-id="7e603-188">The Html.Encode() HTML Helper encodes special characters such as &lt; and &gt; into characters that are safe to display in a web page.</span></span> <span data-ttu-id="7e603-189">Sempre que você renderiza o conteúdo que um usuário envia para um site, você deve codificar o conteúdo para evitar ataques de injeção de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7e603-189">Whenever you render content that a user submits to a website, you should encode the content to prevent JavaScript injection attacks.</span></span>

<span data-ttu-id="7e603-190">(Como criamos a mensagem por conta própria no ProductController, não precisamos realmente codificar a mensagem.</span><span class="sxs-lookup"><span data-stu-id="7e603-190">(Because we created the message ourselves in the ProductController, we don�t really need to encode the message.</span></span> <span data-ttu-id="7e603-191">No entanto, é um bom hábito sempre chamar o método html. Encode () ao exibir o conteúdo recuperado dos dados de exibição em uma exibição.)</span><span class="sxs-lookup"><span data-stu-id="7e603-191">However, it is a good habit to always call the Html.Encode() method when displaying content retrieved from view data within a view.)</span></span>

<span data-ttu-id="7e603-192">Na Listagem 7, tiramos proveito da exibição de dados para passar uma mensagem simples de cadeia de caracteres de um controlador para um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="7e603-192">In Listing 7, we took advantage of view data to pass a simple string message from a controller to a view.</span></span> <span data-ttu-id="7e603-193">Você também pode usar o modo de exibição de dados para passar outros tipos de dados, como uma coleção de registros de banco de dado, de um controlador para um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="7e603-193">You also can use view data to pass other types of data, such as a collection of database records, from a controller to a view.</span></span> <span data-ttu-id="7e603-194">Por exemplo, se você quiser exibir o conteúdo da tabela de banco de dados Products em uma exibição, você passaria a coleção de registros de banco de dados em View Data.</span><span class="sxs-lookup"><span data-stu-id="7e603-194">For example, if you want to display the contents of the Products database table in a view, then you would pass the collection of database records in view data.</span></span>

<span data-ttu-id="7e603-195">Você também tem a opção de passar dados de exibição com rigidez de tipos de um controlador para um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="7e603-195">You also have the option of passing strongly typed view data from a controller to a view.</span></span> <span data-ttu-id="7e603-196">Exploraremos este tópico no tutorial **entendendo dados e exibições de exibição com rigidez de tipos**.</span><span class="sxs-lookup"><span data-stu-id="7e603-196">We explore this topic in the tutorial **Understanding Strongly Typed View Data and Views**.</span></span>

## <a name="summary"></a><span data-ttu-id="7e603-197">Resumo</span><span class="sxs-lookup"><span data-stu-id="7e603-197">Summary</span></span>

<span data-ttu-id="7e603-198">Este tutorial forneceu uma breve introdução às exibições do ASP.NET MVC, exibir dados e auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="7e603-198">This tutorial provided a brief introduction to ASP.NET MVC views, view data, and HTML Helpers.</span></span> <span data-ttu-id="7e603-199">Na primeira seção, você aprendeu a adicionar novas exibições ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="7e603-199">In the first section, you learned how to add new views to your project.</span></span> <span data-ttu-id="7e603-200">Você aprendeu que deve adicionar uma exibição à pasta direita para chamá-la de um controlador específico.</span><span class="sxs-lookup"><span data-stu-id="7e603-200">You learned that you must add a view to the right folder in order to call it from a particular controller.</span></span> <span data-ttu-id="7e603-201">Em seguida, discutimos o tópico de auxiliares HTML.</span><span class="sxs-lookup"><span data-stu-id="7e603-201">Next, we discussed the topic of HTML Helpers.</span></span> <span data-ttu-id="7e603-202">Você aprendeu como os auxiliares HTML permitem que você gere facilmente conteúdo HTML padrão.</span><span class="sxs-lookup"><span data-stu-id="7e603-202">You learned how HTML Helpers enable you to easily generate standard HTML content.</span></span> <span data-ttu-id="7e603-203">Por fim, você aprendeu a aproveitar os dados de exibição para passar dados de um controlador para um modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="7e603-203">Finally, you learned how to take advantage of view data to pass data from a controller to a view.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7e603-204">[Anterior](passing-data-to-view-master-pages-cs.md)
> [Próximo](creating-custom-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7e603-204">[Previous](passing-data-to-view-master-pages-cs.md)
[Next](creating-custom-html-helpers-vb.md)</span></span>
