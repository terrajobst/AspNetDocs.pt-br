---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Criando e usando um auxiliar em um site Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este artigo descreve como criar um auxiliar em um site Páginas da Web do ASP.NET (Razor). Um auxiliar é um componente reutilizável que inclui código e marcação para perf...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 380663951094c9fc7d5f0601e30995fa073a204b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563507"
---
# <a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="65064-104">Criando e usando um auxiliar em um site Páginas da Web do ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="65064-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="65064-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="65064-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="65064-106">Este artigo descreve como criar um auxiliar em um site Páginas da Web do ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="65064-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="65064-107">Um *auxiliar* é um componente reutilizável que inclui código e marcação para executar uma tarefa que pode ser entediante ou complexa.</span><span class="sxs-lookup"><span data-stu-id="65064-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="65064-108">**O que você aprenderá:**</span><span class="sxs-lookup"><span data-stu-id="65064-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="65064-109">Como criar e usar um auxiliar simples.</span><span class="sxs-lookup"><span data-stu-id="65064-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="65064-110">Estes são os recursos do ASP.NET apresentados no artigo:</span><span class="sxs-lookup"><span data-stu-id="65064-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="65064-111">A sintaxe de `@helper`.</span><span class="sxs-lookup"><span data-stu-id="65064-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="65064-112">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="65064-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="65064-113">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="65064-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="65064-114">Este tutorial também funciona com o Páginas da Web do ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="65064-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="overview-of-helpers"></a><span data-ttu-id="65064-115">Visão geral dos auxiliares</span><span class="sxs-lookup"><span data-stu-id="65064-115">Overview of Helpers</span></span>

<span data-ttu-id="65064-116">Se você precisar executar as mesmas tarefas em páginas diferentes em seu site, poderá usar um auxiliar.</span><span class="sxs-lookup"><span data-stu-id="65064-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="65064-117">O Páginas da Web do ASP.NET inclui vários auxiliares, e há muito mais que você pode baixar e instalar.</span><span class="sxs-lookup"><span data-stu-id="65064-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="65064-118">(Uma lista dos auxiliares internos no Páginas da Web do ASP.NET está listada na [referência rápida da API ASP.net](https://go.microsoft.com/fwlink/?LinkId=202907).) Se nenhum dos auxiliares existentes atender às suas necessidades, você poderá criar seu próprio auxiliar.</span><span class="sxs-lookup"><span data-stu-id="65064-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="65064-119">Um auxiliar permite que você use um bloco comum de código em várias páginas.</span><span class="sxs-lookup"><span data-stu-id="65064-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="65064-120">Suponha que, em sua página, você geralmente queira criar um item de nota que seja separado de parágrafos normais.</span><span class="sxs-lookup"><span data-stu-id="65064-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="65064-121">Talvez a observação seja criada como um elemento `<div>` com o estilo de uma caixa com uma borda.</span><span class="sxs-lookup"><span data-stu-id="65064-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="65064-122">Em vez de adicionar essa mesma marcação a uma página toda vez que você quiser exibir uma observação, você pode empacotar a marcação como um auxiliar.</span><span class="sxs-lookup"><span data-stu-id="65064-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="65064-123">Em seguida, você pode inserir a nota com uma única linha de código em qualquer lugar necessário.</span><span class="sxs-lookup"><span data-stu-id="65064-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="65064-124">Usar um auxiliar como esse torna o código em cada uma das páginas mais simples e fácil de ler.</span><span class="sxs-lookup"><span data-stu-id="65064-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="65064-125">Ele também facilita a manutenção de seu site, porque se você precisar alterar a aparência das observações, poderá alterar a marcação em um único lugar.</span><span class="sxs-lookup"><span data-stu-id="65064-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="65064-126">Criando um auxiliar</span><span class="sxs-lookup"><span data-stu-id="65064-126">Creating a Helper</span></span>

<span data-ttu-id="65064-127">Este procedimento mostra como criar o auxiliar que cria a observação, como acabei de descrever.</span><span class="sxs-lookup"><span data-stu-id="65064-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="65064-128">Esse é um exemplo simples, mas o auxiliar personalizado pode incluir qualquer marcação e código ASP.NET necessário.</span><span class="sxs-lookup"><span data-stu-id="65064-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="65064-129">Na pasta raiz do site, crie uma pasta chamada *App\_código*.</span><span class="sxs-lookup"><span data-stu-id="65064-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="65064-130">Esse é um nome de pasta reservado em ASP.NET, onde você pode colocar código para componentes como auxiliares.</span><span class="sxs-lookup"><span data-stu-id="65064-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="65064-131">Na pasta *código de\_de aplicativo* , crie um novo arquivo *. cshtml* e nomeie-o como *myhelprs. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="65064-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="65064-132">Substitua o conteúdo existente pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="65064-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="65064-133">O código usa a sintaxe `@helper` para declarar um novo auxiliar chamado `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="65064-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="65064-134">Esse auxiliar específico permite passar um parâmetro chamado `content` que pode conter uma combinação de texto e marcação.</span><span class="sxs-lookup"><span data-stu-id="65064-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="65064-135">O auxiliar insere a cadeia de caracteres no corpo da nota usando a variável `@content`.</span><span class="sxs-lookup"><span data-stu-id="65064-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="65064-136">Observe que o arquivo é chamado de *Myhelper. cshtml*, mas o auxiliar é chamado de `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="65064-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="65064-137">Você pode colocar vários auxiliares personalizados em um único arquivo.</span><span class="sxs-lookup"><span data-stu-id="65064-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="65064-138">Salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="65064-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="65064-139">Usando o auxiliar em uma página</span><span class="sxs-lookup"><span data-stu-id="65064-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="65064-140">Na pasta raiz, crie um novo arquivo em branco chamado *TestHelper. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="65064-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="65064-141">Adicione o seguinte código ao arquivo:</span><span class="sxs-lookup"><span data-stu-id="65064-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="65064-142">Para chamar o auxiliar que você criou, use `@` seguido pelo nome do arquivo em que o auxiliar é, um ponto e o nome do auxiliar.</span><span class="sxs-lookup"><span data-stu-id="65064-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="65064-143">(Se você tiver várias pastas na pasta de *código do\_de aplicativos* , poderá usar a sintaxe `@FolderName.FileName.HelperName` para chamar o auxiliar em qualquer nível de pasta aninhada).</span><span class="sxs-lookup"><span data-stu-id="65064-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="65064-144">O texto que você adiciona entre aspas dentro dos parênteses é o texto que o auxiliar exibirá como parte da nota na página da Web.</span><span class="sxs-lookup"><span data-stu-id="65064-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="65064-145">Salve a página e execute-a em um navegador.</span><span class="sxs-lookup"><span data-stu-id="65064-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="65064-146">O auxiliar gera o item de observação logo em que você chamou o auxiliar: entre os dois parágrafos.</span><span class="sxs-lookup"><span data-stu-id="65064-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Captura de tela mostrando a página no navegador e como o auxiliar gerou a marcação que coloca uma caixa em volta do texto especificado.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="65064-148">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="65064-148">Additional Resources</span></span>

<span data-ttu-id="65064-149">[Menu horizontal como um auxiliar do Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="65064-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="65064-150">Esta entrada de blog de Mike Papa mostra como criar um menu horizontal como um auxiliar usando marcação, CSS e código.</span><span class="sxs-lookup"><span data-stu-id="65064-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="65064-151">[Aproveitando o HTML5 nos auxiliares páginas da Web do ASP.net para WebMatrix e ASP.net MvC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="65064-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="65064-152">Essa entrada de blog por Sam Abraham mostra um auxiliar que renderiza um elemento `Canvas` HTML5.</span><span class="sxs-lookup"><span data-stu-id="65064-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="65064-153">[A diferença entre @Helpers e @Functions no WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="65064-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="65064-154">Esta entrada de blog de Mike Brind descreve `@helper` sintaxe e sintaxe de `@function` e quando usar cada um.</span><span class="sxs-lookup"><span data-stu-id="65064-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
