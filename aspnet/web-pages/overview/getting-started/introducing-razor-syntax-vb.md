---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Introdução à programação da Web do ASP.NET usando a sintaxe do Razor (Visual Basic) | Microsoft Docs
author: Rick-Anderson
description: Este apêndice fornece uma visão geral da programação com páginas da Web do ASP.NET em Visual Basic, usando o sintaxe Razor.
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 2be57655b8c9b76b94e1d9a7ae5fbee27545a0a9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526589"
---
# <a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a><span data-ttu-id="4f18c-103">Introdução à programação da Web do ASP.NET usando a sintaxe do Razor (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="4f18c-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic)</span></span>

<span data-ttu-id="4f18c-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4f18c-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4f18c-105">Este artigo fornece uma visão geral da programação com Páginas da Web do ASP.NET usando o sintaxe Razor e Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="4f18c-105">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax and Visual Basic.</span></span> <span data-ttu-id="4f18c-106">ASP.NET é a tecnologia da Microsoft para executar páginas da Web dinâmicas em servidores Web.</span><span class="sxs-lookup"><span data-stu-id="4f18c-106">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span>
> 
> <span data-ttu-id="4f18c-107">**O que você aprenderá**:</span><span class="sxs-lookup"><span data-stu-id="4f18c-107">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="4f18c-108">As 8 principais dicas de programação para introdução ao Páginas da Web do ASP.NET de programação usando sintaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="4f18c-108">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="4f18c-109">Conceitos básicos de programação que você precisará.</span><span class="sxs-lookup"><span data-stu-id="4f18c-109">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="4f18c-110">O que é o código do servidor ASP.NET e a sintaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="4f18c-110">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="4f18c-111">Versões de software</span><span class="sxs-lookup"><span data-stu-id="4f18c-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="4f18c-112">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="4f18c-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="4f18c-113">Este tutorial também funciona com o Páginas da Web do ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="4f18c-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>

<span data-ttu-id="4f18c-114">A maioria dos exemplos de uso de Páginas da Web do ASP.NET C#com sintaxe Razor uso.</span><span class="sxs-lookup"><span data-stu-id="4f18c-114">Most examples of using ASP.NET Web Pages with Razor syntax use C#.</span></span> <span data-ttu-id="4f18c-115">Mas o sintaxe Razor também dá suporte a Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="4f18c-115">But the Razor syntax also supports Visual Basic.</span></span> <span data-ttu-id="4f18c-116">Para programar uma página da Web do ASP.NET no Visual Basic, você cria uma página da Web com uma extensão de nome de arquivo *. vbhtml* e, em seguida, adiciona Visual Basic código.</span><span class="sxs-lookup"><span data-stu-id="4f18c-116">To program an ASP.NET web page in Visual Basic, you create a web page with a *.vbhtml* filename extension, and then add Visual Basic code.</span></span> <span data-ttu-id="4f18c-117">Este artigo fornece uma visão geral de como trabalhar com a linguagem de Visual Basic e a sintaxe para criar páginas da Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4f18c-117">This article gives you an overview of working with the Visual Basic language and syntax to create ASP.NET Webpages.</span></span>

> [!NOTE]
> <span data-ttu-id="4f18c-118">Os modelos de site padrão do Microsoft WebMatrix (**padaria**, **Galeria de fotos**e **site de início**, etc.) estão disponíveis no C# e Visual Basic versões.</span><span class="sxs-lookup"><span data-stu-id="4f18c-118">The default website templates for Microsoft WebMatrix (**Bakery**, **Photo Gallery**, and **Starter Site**, etc.) are available in C# and Visual Basic versions.</span></span> <span data-ttu-id="4f18c-119">Você pode instalar os modelos de Visual Basic pelo como pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="4f18c-119">You can install the Visual Basic templates by as NuGet packages.</span></span> <span data-ttu-id="4f18c-120">Os modelos de site são instalados na pasta raiz do seu site em uma pasta chamada *Microsoft Templates*.</span><span class="sxs-lookup"><span data-stu-id="4f18c-120">Website templates are installed in the root folder of your site in a folder named *Microsoft Templates*.</span></span>

## <a name="the-top-8-programming-tips"></a><span data-ttu-id="4f18c-121">As 8 principais dicas de programação</span><span class="sxs-lookup"><span data-stu-id="4f18c-121">The Top 8 Programming Tips</span></span>

<span data-ttu-id="4f18c-122">Esta seção lista algumas dicas que você precisa saber ao começar a escrever o código do servidor ASP.NET usando o sintaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="4f18c-122">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="4f18c-123">1. você adiciona código a uma página usando o caractere @</span><span class="sxs-lookup"><span data-stu-id="4f18c-123">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="4f18c-124">O caractere `@` inicia expressões embutidas, blocos de instrução única e blocos de várias instruções:</span><span class="sxs-lookup"><span data-stu-id="4f18c-124">The `@` character starts inline expressions, single-statement blocks, and multi-statement blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

<span data-ttu-id="4f18c-125">O resultado exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="4f18c-125">The result displayed in a browser:</span></span>

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="4f18c-127">**Codificação HTML**</span><span class="sxs-lookup"><span data-stu-id="4f18c-127">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="4f18c-128">Quando você exibe o conteúdo em uma página usando o caractere `@`, como nos exemplos anteriores, o ASP.NET HTML codifica a saída.</span><span class="sxs-lookup"><span data-stu-id="4f18c-128">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="4f18c-129">Isso substitui os caracteres HTML reservados (como `<` e `>` e `&`) por códigos que permitem que os caracteres sejam exibidos como caracteres em uma página da Web, em vez de serem interpretados como marcas HTML ou entidades.</span><span class="sxs-lookup"><span data-stu-id="4f18c-129">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="4f18c-130">Sem codificação HTML, a saída do código do servidor pode não ser exibida corretamente e pode expor uma página a riscos de segurança.</span><span class="sxs-lookup"><span data-stu-id="4f18c-130">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="4f18c-131">Se sua meta é gerar uma marcação HTML que renderiza marcas como marcação (por exemplo `<p></p>` para um parágrafo ou `<em></em>` para enfatizar o texto), consulte a seção [combinando texto, marcação e código em blocos de código](#BM_CombiningTextMarkupAndCode) mais adiante neste artigo.</span><span class="sxs-lookup"><span data-stu-id="4f18c-131">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="4f18c-132">Você pode ler mais sobre codificação HTML em [trabalhando com formulários HTML em páginas da Web do ASP.net sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span><span class="sxs-lookup"><span data-stu-id="4f18c-132">You can read more about HTML encoding in [Working with HTML Forms in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>

### <a name="2-you-enclose-code-blocks-with-codeend-code"></a><span data-ttu-id="4f18c-133">2. você coloca blocos de código com código... Código final</span><span class="sxs-lookup"><span data-stu-id="4f18c-133">2. You enclose code blocks with Code...End Code</span></span>

<span data-ttu-id="4f18c-134">Um bloco de código inclui uma ou mais instruções de código e é incluído com as palavras-chave `Code` e `End Code`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-134">A code block includes one or more code statements and is enclosed with the keywords `Code` and `End Code`.</span></span> <span data-ttu-id="4f18c-135">Coloque a palavra-chave `Code` de abertura imediatamente após &#8212; o caractere de `@` não pode haver espaço em branco entre elas.</span><span class="sxs-lookup"><span data-stu-id="4f18c-135">Place the opening `Code` keyword immediately after the `@` character &#8212; there can't be whitespace between them.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

<span data-ttu-id="4f18c-136">O resultado exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="4f18c-136">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a><span data-ttu-id="4f18c-138">3. dentro de um bloco, você encerra cada instrução de código com uma quebra de linha</span><span class="sxs-lookup"><span data-stu-id="4f18c-138">3. Inside a block, you end each code statement with a line break</span></span>

<span data-ttu-id="4f18c-139">Em um bloco de código Visual Basic, cada instrução termina com uma quebra de linha.</span><span class="sxs-lookup"><span data-stu-id="4f18c-139">In a Visual Basic code block, each statement ends with a line break.</span></span> <span data-ttu-id="4f18c-140">(Posteriormente, no artigo, você verá uma maneira de encapsular uma instrução de código longo em várias linhas, se necessário.)</span><span class="sxs-lookup"><span data-stu-id="4f18c-140">(Later in the article you'll see a way to wrap a long code statement into multiple lines if needed.)</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="4f18c-141">4. você usa variáveis para armazenar valores</span><span class="sxs-lookup"><span data-stu-id="4f18c-141">4. You use variables to store values</span></span>

<span data-ttu-id="4f18c-142">Você pode armazenar valores em uma *variável*, incluindo cadeias de caracteres, números e datas, etc. Você cria uma nova variável usando a palavra-chave `Dim`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-142">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `Dim` keyword.</span></span> <span data-ttu-id="4f18c-143">Você pode inserir valores de variáveis diretamente em uma página usando `@`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-143">You can insert variable values directly in a page using `@`.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

<span data-ttu-id="4f18c-144">O resultado exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="4f18c-144">The result displayed in a browser:</span></span>

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="4f18c-146">5. você coloca valores de cadeia de caracteres literais entre aspas duplas</span><span class="sxs-lookup"><span data-stu-id="4f18c-146">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="4f18c-147">Uma *String* é uma sequência de caracteres que são tratados como texto.</span><span class="sxs-lookup"><span data-stu-id="4f18c-147">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="4f18c-148">Para especificar uma cadeia de caracteres, coloque-a entre aspas duplas:</span><span class="sxs-lookup"><span data-stu-id="4f18c-148">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

<span data-ttu-id="4f18c-149">Para inserir aspas duplas em um valor de cadeia de caracteres, insira dois caracteres de aspas duplas.</span><span class="sxs-lookup"><span data-stu-id="4f18c-149">To embed double quotation marks within a string value, insert two double quotation mark characters.</span></span> <span data-ttu-id="4f18c-150">Se você quiser que o caractere de aspas duplas apareça uma vez na saída da página, insira-o como `""` dentro da cadeia de caracteres entre aspas e, se desejar que ele apareça duas vezes, insira-o como `""""` dentro da cadeia de caracteres entre aspas.</span><span class="sxs-lookup"><span data-stu-id="4f18c-150">If you want the double quotation character to appear once in the page output, enter it as `""` within the quoted string, and if you want it to appear twice, enter it as `""""` within the quoted string.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

<span data-ttu-id="4f18c-151">O resultado exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="4f18c-151">The result displayed in a browser:</span></span>

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a><span data-ttu-id="4f18c-153">6. o código de Visual Basic não diferencia maiúsculas de minúsculas</span><span class="sxs-lookup"><span data-stu-id="4f18c-153">6. Visual Basic code is not case sensitive</span></span>

<span data-ttu-id="4f18c-154">O idioma da Visual Basic não diferencia maiúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="4f18c-154">The Visual Basic language is not case sensitive.</span></span> <span data-ttu-id="4f18c-155">As palavras-chave de programação (como `Dim`, `If`e `True`) e nomes de variáveis (como `myString`ou `subTotal`) podem ser escritas em qualquer caso.</span><span class="sxs-lookup"><span data-stu-id="4f18c-155">Programming keywords (like `Dim`, `If`, and `True`) and variable names (like `myString`, or `subTotal`) can be written in any case.</span></span>

<span data-ttu-id="4f18c-156">As linhas de código a seguir atribuem um valor à variável `lastname` usando um nome em minúsculas e, em seguida, geram o valor da variável para a página usando um nome em maiúsculas.</span><span class="sxs-lookup"><span data-stu-id="4f18c-156">The following lines of code assign a value to the variable `lastname` using a lowercase name, and then output the variable value to the page using an uppercase name.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

<span data-ttu-id="4f18c-157">O resultado exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="4f18c-157">The result displayed in a browser:</span></span>

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a><span data-ttu-id="4f18c-159">7. a maior parte da codificação envolve trabalhar com objetos</span><span class="sxs-lookup"><span data-stu-id="4f18c-159">7. Much of your coding involves working with objects</span></span>

<span data-ttu-id="4f18c-160">Um objeto representa algo que você pode programar com &#8212; uma página, uma caixa de texto, um arquivo, uma imagem, uma solicitação da Web, uma mensagem de email, um registro de cliente (linha de banco de dados), etc. Os objetos têm propriedades que descrevem suas &#8212; características um objeto de caixa de texto tem uma propriedade `Text`, um objeto de solicitação tem uma propriedade `Url`, uma mensagem de email tem uma propriedade `From` e um objeto Customer tem uma propriedade `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-160">An object represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics &#8212; a text box object has a `Text` property, a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="4f18c-161">Os objetos também têm métodos que são os verbos &quot;&quot; podem ser executados.</span><span class="sxs-lookup"><span data-stu-id="4f18c-161">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="4f18c-162">Os exemplos incluem o método de `Save` de um objeto de arquivo, o método `Rotate` de um objeto de imagem e um método de `Send` do objeto de email.</span><span class="sxs-lookup"><span data-stu-id="4f18c-162">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="4f18c-163">Com freqüência, você trabalhará com o objeto `Request`, que fornece informações como os valores dos campos de formulário na página (caixas de texto, etc.), que tipo de navegador fez a solicitação, a URL da página, a identidade do usuário, etc. Este exemplo mostra como acessar as propriedades do objeto `Request` e como chamar o método `MapPath` do objeto `Request`, que fornece o caminho absoluto da página no servidor:</span><span class="sxs-lookup"><span data-stu-id="4f18c-163">You'll often work with the `Request` object, which gives you information like the values of form fields on the page (text boxes, etc.), what type of browser made the request, the URL of the page, the user identity, etc. This example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

<span data-ttu-id="4f18c-164">O resultado exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="4f18c-164">The result displayed in a browser:</span></span>

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="4f18c-166">8. você pode escrever código que toma decisões</span><span class="sxs-lookup"><span data-stu-id="4f18c-166">8. You can write code that makes decisions</span></span>

<span data-ttu-id="4f18c-167">Um recurso importante das páginas da Web dinâmicas é que você pode determinar o que fazer com base nas condições.</span><span class="sxs-lookup"><span data-stu-id="4f18c-167">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="4f18c-168">A maneira mais comum de fazer isso é com a instrução de `If` (e a instrução `Else` opcional).</span><span class="sxs-lookup"><span data-stu-id="4f18c-168">The most common way to do this is with the `If` statement (and optional `Else` statement).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

<span data-ttu-id="4f18c-169">A instrução `If IsPost` é uma maneira abreviada de escrever `If IsPost = True`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-169">The statement `If IsPost` is a shorthand way of writing `If IsPost = True`.</span></span> <span data-ttu-id="4f18c-170">Juntamente com `If` instruções, há várias maneiras de testar condições, repetir blocos de código e assim por diante, que são descritos posteriormente neste artigo.</span><span class="sxs-lookup"><span data-stu-id="4f18c-170">Along with `If` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="4f18c-171">O resultado exibido em um navegador (depois de clicar em **Enviar**):</span><span class="sxs-lookup"><span data-stu-id="4f18c-171">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> <span data-ttu-id="4f18c-173">**Métodos GET e POST de HTTP e a propriedade IsPost**</span><span class="sxs-lookup"><span data-stu-id="4f18c-173">**HTTP GET and POST Methods and the IsPost Property**</span></span>
> 
> <span data-ttu-id="4f18c-174">O protocolo usado para páginas da Web (HTTP) oferece suporte a um número muito limitado de métodos (&quot;verbos&quot;) que são usados para fazer solicitações ao servidor.</span><span class="sxs-lookup"><span data-stu-id="4f18c-174">The protocol used for web pages (HTTP) supports a very limited number of methods (&quot;verbs&quot;) that are used to make requests to the server.</span></span> <span data-ttu-id="4f18c-175">Os dois mais comuns são GET, que é usado para ler uma página e POST, que é usado para enviar uma página.</span><span class="sxs-lookup"><span data-stu-id="4f18c-175">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="4f18c-176">Em geral, na primeira vez que um usuário solicita uma página, a página é solicitada usando GET.</span><span class="sxs-lookup"><span data-stu-id="4f18c-176">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="4f18c-177">Se o usuário preenche um formulário e clica em **Enviar**, o navegador faz uma solicitação post para o servidor.</span><span class="sxs-lookup"><span data-stu-id="4f18c-177">If the user fills in a form and then clicks **Submit**, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="4f18c-178">Na programação da Web, geralmente é útil saber se uma página está sendo solicitada como um GET ou como uma POSTAgem para que você saiba como processar a página.</span><span class="sxs-lookup"><span data-stu-id="4f18c-178">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="4f18c-179">No Páginas da Web do ASP.NET, você pode usar a propriedade `IsPost` para ver se uma solicitação é GET ou POST.</span><span class="sxs-lookup"><span data-stu-id="4f18c-179">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="4f18c-180">Se a solicitação for uma POSTAgem, a propriedade `IsPost` retornará true e você poderá fazer coisas como ler os valores das caixas de texto em um formulário.</span><span class="sxs-lookup"><span data-stu-id="4f18c-180">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="4f18c-181">Muitos exemplos que você verá mostram como processar a página de forma diferente, dependendo do valor de `IsPost`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-181">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>

## <a name="a-simple-code-example"></a><span data-ttu-id="4f18c-182">Um exemplo de código simples</span><span class="sxs-lookup"><span data-stu-id="4f18c-182">A Simple Code Example</span></span>

<span data-ttu-id="4f18c-183">Este procedimento mostra como criar uma página que ilustra as técnicas básicas de programação.</span><span class="sxs-lookup"><span data-stu-id="4f18c-183">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="4f18c-184">No exemplo, você cria uma página que permite aos usuários inserir dois números e, em seguida, adiciona-os e exibe o resultado.</span><span class="sxs-lookup"><span data-stu-id="4f18c-184">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="4f18c-185">No editor, crie um novo arquivo e nomeie-o como *Addnumerates. vbhtml*.</span><span class="sxs-lookup"><span data-stu-id="4f18c-185">In your editor, create a new file and name it *AddNumbers.vbhtml*.</span></span>
2. <span data-ttu-id="4f18c-186">Copie o código e a marcação a seguir para a página, substituindo qualquer coisa que já esteja na página.</span><span class="sxs-lookup"><span data-stu-id="4f18c-186">Copy the following code and markup into the page, replacing anything already in the page.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    <span data-ttu-id="4f18c-187">Aqui estão algumas coisas que você deve observar:</span><span class="sxs-lookup"><span data-stu-id="4f18c-187">Here are some things for you to note:</span></span>

    - <span data-ttu-id="4f18c-188">O caractere `@` inicia o primeiro bloco de código na página e precede a variável `totalMessage` inserida perto da parte inferior.</span><span class="sxs-lookup"><span data-stu-id="4f18c-188">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable embedded near the bottom.</span></span>
    - <span data-ttu-id="4f18c-189">O bloco na parte superior da página está entre `Code...End Code`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-189">The block at the top of the page is enclosed in `Code...End Code`.</span></span>
    - <span data-ttu-id="4f18c-190">As variáveis `total`, `num1`, `num2`e `totalMessage` armazenam vários números e uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="4f18c-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="4f18c-191">O valor da cadeia de caracteres literal atribuído à variável `totalMessage` está entre aspas duplas.</span><span class="sxs-lookup"><span data-stu-id="4f18c-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="4f18c-192">Como o código de Visual Basic não diferencia maiúsculas de minúsculas, quando a variável de `totalMessage` é usada perto da parte inferior da página, seu nome precisa apenas corresponder à grafia da declaração de variável na parte superior da página.</span><span class="sxs-lookup"><span data-stu-id="4f18c-192">Because Visual Basic code is not case sensitive, when the `totalMessage` variable is used near the bottom of the page, its name only needs to match the spelling of the variable declaration at the top of the page.</span></span> <span data-ttu-id="4f18c-193">A capitalização não importa.</span><span class="sxs-lookup"><span data-stu-id="4f18c-193">The casing doesn't matter.</span></span>
    - <span data-ttu-id="4f18c-194">A expressão `num1.AsInt()` + `num2.AsInt()` mostra como trabalhar com objetos e métodos.</span><span class="sxs-lookup"><span data-stu-id="4f18c-194">The expression `num1.AsInt()` + `num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="4f18c-195">O método `AsInt` em cada variável converte a cadeia de caracteres inserida por um usuário em um número inteiro (um inteiro) que pode ser adicionado.</span><span class="sxs-lookup"><span data-stu-id="4f18c-195">The `AsInt` method on each variable converts the string entered by a user to a whole number (an integer) that can be added.</span></span>
    - <span data-ttu-id="4f18c-196">A marca de `<form>` inclui um atributo de `method="post"`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-196">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="4f18c-197">Isso especifica que quando o usuário clicar em **Adicionar**, a página será enviada ao servidor usando o método http post.</span><span class="sxs-lookup"><span data-stu-id="4f18c-197">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="4f18c-198">Quando a página é enviada, o código `If IsPost` é avaliado como true e o código condicional é executado, exibindo o resultado da adição dos números.</span><span class="sxs-lookup"><span data-stu-id="4f18c-198">When the page is submitted, the code `If IsPost` evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="4f18c-199">Salve a página e execute-a em um navegador.</span><span class="sxs-lookup"><span data-stu-id="4f18c-199">Save the page and run it in a browser.</span></span> <span data-ttu-id="4f18c-200">(Verifique se a página está selecionada no espaço de trabalho **arquivos** antes de executá-la.) Insira dois números inteiros e, em seguida, clique no botão **Adicionar** .</span><span class="sxs-lookup"><span data-stu-id="4f18c-200">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span>

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a><span data-ttu-id="4f18c-202">Linguagem e sintaxe de Visual Basic</span><span class="sxs-lookup"><span data-stu-id="4f18c-202">Visual Basic Language and Syntax</span></span>

<span data-ttu-id="4f18c-203">Anteriormente, você viu um exemplo básico de como criar uma página da Web ASP.NET e como você pode adicionar código de servidor à marcação HTML.</span><span class="sxs-lookup"><span data-stu-id="4f18c-203">Earlier you saw a basic example of how to create an ASP.NET web page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="4f18c-204">Aqui, você aprenderá as noções básicas do uso de Visual Basic para escrever código de servidor &#8212; ASP.NET usando o sintaxe Razor, ou seja, as regras de linguagem de programação.</span><span class="sxs-lookup"><span data-stu-id="4f18c-204">Here you'll learn the basics of using Visual Basic to write ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="4f18c-205">Se você tiver experiência com programação (especialmente se tiver usado C, C++, C#, Visual Basic ou JavaScript), grande parte do que você lê aqui será familiar.</span><span class="sxs-lookup"><span data-stu-id="4f18c-205">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="4f18c-206">Você provavelmente precisará se familiarizar apenas com o modo como o código do WebMatrix é adicionado à marcação em arquivos *. vbhtml* .</span><span class="sxs-lookup"><span data-stu-id="4f18c-206">You'll probably need to familiarize yourself only with how WebMatrix code is added to markup in *.vbhtml* files.</span></span>

### <a id="BM_CombiningTextMarkupAndCode"></a><span data-ttu-id="4f18c-207">Combinando texto, marcação e código em blocos de código</span><span class="sxs-lookup"><span data-stu-id="4f18c-207">Combining text, markup, and code in code blocks</span></span>

<span data-ttu-id="4f18c-208">Em blocos de código de servidor, muitas vezes você desejará produzir texto e marcação para a página.</span><span class="sxs-lookup"><span data-stu-id="4f18c-208">In server code blocks, you'll often want to output text and markup to the page.</span></span> <span data-ttu-id="4f18c-209">Se um bloco de código de servidor contiver texto que não seja código e que, em vez disso, deve ser renderizado como está, ASP.NET precisará ser capaz de distinguir esse texto do código.</span><span class="sxs-lookup"><span data-stu-id="4f18c-209">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="4f18c-210">Há várias maneiras de fazer isso.</span><span class="sxs-lookup"><span data-stu-id="4f18c-210">There are several ways to do this.</span></span>

- <span data-ttu-id="4f18c-211">Coloque o texto em um elemento de bloco HTML como `<p></p>` ou `<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="4f18c-211">Enclose the text in an HTML block element like `<p></p>` or `<em></em>`:</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    <span data-ttu-id="4f18c-212">O elemento HTML pode incluir texto, elementos HTML adicionais e expressões de código de servidor.</span><span class="sxs-lookup"><span data-stu-id="4f18c-212">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="4f18c-213">Quando ASP.NET vê a marca HTML de abertura (por exemplo, `<p>`), ela processa tudo o elemento e seu conteúdo como está no navegador (e resolve as expressões de código de servidor).</span><span class="sxs-lookup"><span data-stu-id="4f18c-213">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything the element and its content as is to the browser (and resolves the server-code expressions).</span></span>

- <span data-ttu-id="4f18c-214">Use o operador `@:` ou o elemento `<text>`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-214">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="4f18c-215">O `@:` gera uma única linha de conteúdo que contém texto sem formatação ou marcas HTML não correspondentes; o elemento `<text>` inclui várias linhas na saída.</span><span class="sxs-lookup"><span data-stu-id="4f18c-215">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="4f18c-216">Essas opções são úteis quando você não deseja renderizar um elemento HTML como parte da saída.</span><span class="sxs-lookup"><span data-stu-id="4f18c-216">These options are useful when you don't want to render an HTML element as part of the output.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    <span data-ttu-id="4f18c-217">O exemplo a seguir repete o exemplo anterior, mas usa um único par de marcas `<text>` para colocar o texto a ser renderizado.</span><span class="sxs-lookup"><span data-stu-id="4f18c-217">The following example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    <span data-ttu-id="4f18c-218">No exemplo a seguir, as marcas `<text>` e `</text>` incluem três linhas, todas as quais têm um texto não contido e marcas HTML não correspondentes (`<br />`), juntamente com o código do servidor e as marcas HTML correspondentes.</span><span class="sxs-lookup"><span data-stu-id="4f18c-218">In the following example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="4f18c-219">Novamente, você também pode preceder cada linha individualmente com o operador de `@:`; de qualquer forma funciona.</span><span class="sxs-lookup"><span data-stu-id="4f18c-219">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > <span data-ttu-id="4f18c-220">Quando você faz a saída de texto conforme mostrado &#8212; nesta seção usando um elemento HTML, o operador `@:` ou o elemento &#8212; `<text>` ASP.net não codifica a saída em HTML.</span><span class="sxs-lookup"><span data-stu-id="4f18c-220">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="4f18c-221">(Como observado anteriormente, o ASP.NET codifica a saída de expressões de código do servidor e os blocos de código do servidor que são precedidos por `@`, exceto nos casos especiais indicados nesta seção.)</span><span class="sxs-lookup"><span data-stu-id="4f18c-221">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="4f18c-222">Espaço em branco</span><span class="sxs-lookup"><span data-stu-id="4f18c-222">Whitespace</span></span>

<span data-ttu-id="4f18c-223">Espaços extras em uma instrução (e fora de um literal de cadeia de caracteres) não afetam a instrução:</span><span class="sxs-lookup"><span data-stu-id="4f18c-223">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a><span data-ttu-id="4f18c-224">Dividindo instruções longas em várias linhas</span><span class="sxs-lookup"><span data-stu-id="4f18c-224">Breaking long statements into multiple lines</span></span>

<span data-ttu-id="4f18c-225">Você pode dividir uma instrução de código longo em várias linhas usando o caractere de sublinhado `_` (que, em Visual Basic, é chamado de *caractere de continuação*) após cada linha de código.</span><span class="sxs-lookup"><span data-stu-id="4f18c-225">You can break a long code statement into multiple lines by using the underscore character `_` (which in Visual Basic is called the *continuation character*) after each line of code.</span></span> <span data-ttu-id="4f18c-226">Para quebrar uma instrução na próxima linha, no final da linha, adicione um espaço e, em seguida, o caractere de continuação.</span><span class="sxs-lookup"><span data-stu-id="4f18c-226">To break a statement onto the next line, at the end of the line add a space and then the continuation character.</span></span> <span data-ttu-id="4f18c-227">Continue a instrução na próxima linha.</span><span class="sxs-lookup"><span data-stu-id="4f18c-227">Continue the statement on the next line.</span></span> <span data-ttu-id="4f18c-228">Você pode encapsular instruções em quantas linhas forem necessárias para melhorar a legibilidade.</span><span class="sxs-lookup"><span data-stu-id="4f18c-228">You can wrap statements onto as many lines as you need to improve readability.</span></span> <span data-ttu-id="4f18c-229">As seguintes instruções são iguais:</span><span class="sxs-lookup"><span data-stu-id="4f18c-229">The following statements are the same:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

<span data-ttu-id="4f18c-230">No entanto, não é possível encapsular uma linha no meio de um literal de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="4f18c-230">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="4f18c-231">O exemplo a seguir não funciona:</span><span class="sxs-lookup"><span data-stu-id="4f18c-231">The following example doesn't work:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

<span data-ttu-id="4f18c-232">Para combinar uma cadeia de caracteres longa que envolve várias linhas como o código acima, você precisaria usar o *operador de concatenação* (`&`), que você verá mais adiante neste artigo.</span><span class="sxs-lookup"><span data-stu-id="4f18c-232">To combine a long string that wraps to multiple lines like the above code, you would need to use the *concatenation operator* (`&`), which you'll see later in this article.</span></span>

### <a name="code-comments"></a><span data-ttu-id="4f18c-233">Comentários de código</span><span class="sxs-lookup"><span data-stu-id="4f18c-233">Code comments</span></span>

<span data-ttu-id="4f18c-234">Os comentários permitem que você deixe anotações para si mesmo ou para outras pessoas.</span><span class="sxs-lookup"><span data-stu-id="4f18c-234">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="4f18c-235">Sintaxe Razor comentários são prefixados com `@*` e terminam com `*@`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-235">Razor syntax comments are prefixed with `@*` and end with `*@`.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

<span data-ttu-id="4f18c-236">Nos blocos de código, você pode usar os comentários de sintaxe Razor ou pode usar o caractere de comentário de Visual Basic comum, que é uma aspa simples (`'`) prefixada para cada linha.</span><span class="sxs-lookup"><span data-stu-id="4f18c-236">Within code blocks you can use the Razor syntax comments, or you can use ordinary Visual Basic comment character, which is a single quote (`'`) prefixed to each line.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a><span data-ttu-id="4f18c-237">variáveis</span><span class="sxs-lookup"><span data-stu-id="4f18c-237">Variables</span></span>

<span data-ttu-id="4f18c-238">Uma variável é um objeto nomeado que você usa para armazenar dados.</span><span class="sxs-lookup"><span data-stu-id="4f18c-238">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="4f18c-239">Você pode nomear variáveis como qualquer coisa, mas o nome deve começar com um caractere alfabético e não pode conter espaços em branco ou caracteres reservados.</span><span class="sxs-lookup"><span data-stu-id="4f18c-239">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span> <span data-ttu-id="4f18c-240">Em Visual Basic, como vimos anteriormente, o caso das letras em um nome de variável não importa.</span><span class="sxs-lookup"><span data-stu-id="4f18c-240">In Visual Basic, as you saw earlier, the case of the letters in a variable name doesn't matter.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="4f18c-241">Variáveis e tipos de dados</span><span class="sxs-lookup"><span data-stu-id="4f18c-241">Variables and data types</span></span>

<span data-ttu-id="4f18c-242">Uma variável pode ter um tipo de dados específico, que indica que tipo de dados é armazenado na variável.</span><span class="sxs-lookup"><span data-stu-id="4f18c-242">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="4f18c-243">Você pode ter variáveis de cadeia de caracteres que armazenam valores de cadeia de caracteres (como &quot;Olá, mundo&quot;), variáveis de inteiro que armazenam valores de número inteiro (como 3 ou 79) e variáveis de data que armazenam valores de data em uma variedade de formatos (como 4/12/2012 ou 2009 de março).</span><span class="sxs-lookup"><span data-stu-id="4f18c-243">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="4f18c-244">E há muitos outros tipos de dados que você pode usar.</span><span class="sxs-lookup"><span data-stu-id="4f18c-244">And there are many other data types you can use.</span></span>

<span data-ttu-id="4f18c-245">No entanto, você não precisa especificar um tipo para uma variável.</span><span class="sxs-lookup"><span data-stu-id="4f18c-245">However, you don't have to specify a type for a variable.</span></span> <span data-ttu-id="4f18c-246">Na maioria dos casos, ASP.NET pode descobrir o tipo com base em como os dados na variável estão sendo usados.</span><span class="sxs-lookup"><span data-stu-id="4f18c-246">In most cases ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="4f18c-247">(Ocasionalmente, você deve especificar um tipo; você verá exemplos em que isso é verdadeiro.)</span><span class="sxs-lookup"><span data-stu-id="4f18c-247">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="4f18c-248">Para declarar uma variável sem especificar um tipo, use `Dim` mais o nome da variável (por exemplo, `Dim myVar`).</span><span class="sxs-lookup"><span data-stu-id="4f18c-248">To declare a variable without specifying a type, use `Dim` plus the variable name (for instance, `Dim myVar`).</span></span> <span data-ttu-id="4f18c-249">Para declarar uma variável com um tipo, use `Dim` mais o nome da variável, seguido por `As` e, em seguida, o nome do tipo (por exemplo, `Dim myVar As String`).</span><span class="sxs-lookup"><span data-stu-id="4f18c-249">To declare a variable with a type, use `Dim` plus the variable name, followed by `As` and then the type name (for instance, `Dim myVar As String`).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

<span data-ttu-id="4f18c-250">O exemplo a seguir mostra algumas expressões embutidas que usam as variáveis em uma página da Web.</span><span class="sxs-lookup"><span data-stu-id="4f18c-250">The following example shows some inline expressions that use the variables in a web page.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

<span data-ttu-id="4f18c-251">O resultado exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="4f18c-251">The result displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="4f18c-253">Convertendo e testando tipos de dados</span><span class="sxs-lookup"><span data-stu-id="4f18c-253">Converting and testing data types</span></span>

<span data-ttu-id="4f18c-254">Embora ASP.NET normalmente possa determinar um tipo de dados automaticamente, às vezes ele não pode.</span><span class="sxs-lookup"><span data-stu-id="4f18c-254">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="4f18c-255">Portanto, talvez seja necessário ajudar a ASP.NET com a execução de uma conversão explícita.</span><span class="sxs-lookup"><span data-stu-id="4f18c-255">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="4f18c-256">Mesmo que você não precise converter tipos, às vezes é útil testar para ver de que tipo de dados você pode estar trabalhando.</span><span class="sxs-lookup"><span data-stu-id="4f18c-256">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="4f18c-257">O caso mais comum é que você precisa converter uma cadeia de caracteres em outro tipo, como em um inteiro ou data.</span><span class="sxs-lookup"><span data-stu-id="4f18c-257">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="4f18c-258">O exemplo a seguir mostra um caso típico em que você deve converter uma cadeia de caracteres em um número.</span><span class="sxs-lookup"><span data-stu-id="4f18c-258">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

<span data-ttu-id="4f18c-259">Como regra, a entrada do usuário chega a você como cadeias de caracteres.</span><span class="sxs-lookup"><span data-stu-id="4f18c-259">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="4f18c-260">Mesmo que você tenha solicitado ao usuário que insira um número e, mesmo que tenha inserido um dígito, quando a entrada do usuário é enviada e você a lê no código, os dados estão no formato de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="4f18c-260">Even if you've prompted the user to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="4f18c-261">Portanto, você deve converter a cadeia de caracteres em um número.</span><span class="sxs-lookup"><span data-stu-id="4f18c-261">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="4f18c-262">No exemplo, se você tentar executar aritmética nos valores sem convertê-los, os seguintes resultados de erro, porque ASP.NET não pode adicionar duas cadeias de caracteres:</span><span class="sxs-lookup"><span data-stu-id="4f18c-262">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

`Cannot implicitly convert type 'string' to 'int'.`

<span data-ttu-id="4f18c-263">Para converter os valores em inteiros, você chama o método `AsInt`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-263">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="4f18c-264">Se a conversão for bem-sucedida, você poderá adicionar os números.</span><span class="sxs-lookup"><span data-stu-id="4f18c-264">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="4f18c-265">A tabela a seguir lista alguns métodos comuns de conversão e teste para variáveis.</span><span class="sxs-lookup"><span data-stu-id="4f18c-265">The following table lists some common conversion and test methods for variables.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="4f18c-266"><strong>Método</strong></span><span class="sxs-lookup"><span data-stu-id="4f18c-266"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-267"><strong>Descrição</strong></span><span class="sxs-lookup"><span data-stu-id="4f18c-267"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-268"><strong>Exemplo</strong></span><span class="sxs-lookup"><span data-stu-id="4f18c-268"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-269">Converte uma cadeia de caracteres que representa um número inteiro (como &quot;593&quot;) em um número inteiro.</span><span class="sxs-lookup"><span data-stu-id="4f18c-269">Converts a string that represents a whole number (like &quot;593&quot;) to an integer.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-270">Converte uma cadeia de caracteres como &quot;true&quot; ou &quot;false&quot; em um tipo Boolean.</span><span class="sxs-lookup"><span data-stu-id="4f18c-270">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-271">Converte uma cadeia de caracteres que tem um valor decimal como &quot;1,3&quot; ou &quot;7,439&quot; para um número de ponto flutuante.</span><span class="sxs-lookup"><span data-stu-id="4f18c-271">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-272">Converte uma cadeia de caracteres que tem um valor decimal como &quot;1,3&quot; ou &quot;7,439&quot; em um número decimal.</span><span class="sxs-lookup"><span data-stu-id="4f18c-272">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="4f18c-273">(Em ASP.NET, um número decimal é mais preciso do que um número de ponto flutuante.)</span><span class="sxs-lookup"><span data-stu-id="4f18c-273">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-274">Converte uma cadeia de caracteres que representa um valor de data e hora para o tipo de `DateTime` ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4f18c-274">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-275">Converte qualquer outro tipo de dados em uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="4f18c-275">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a><span data-ttu-id="4f18c-276">Operadores</span><span class="sxs-lookup"><span data-stu-id="4f18c-276">Operators</span></span>

<span data-ttu-id="4f18c-277">Um operador é uma palavra-chave ou um caractere que informa ASP.NET que tipo de comando executar em uma expressão.</span><span class="sxs-lookup"><span data-stu-id="4f18c-277">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="4f18c-278">O Visual Basic dá suporte a muitos operadores, mas você só precisa reconhecer alguns para começar a desenvolver páginas da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4f18c-278">Visual Basic supports many operators, but you only need to recognize a few to get started developing ASP.NET web pages.</span></span> <span data-ttu-id="4f18c-279">A tabela a seguir resume os operadores mais comuns.</span><span class="sxs-lookup"><span data-stu-id="4f18c-279">The following table summarizes the most common operators.</span></span>

:::row:::
    :::column:::
        <span data-ttu-id="4f18c-280"><strong>Operador</strong></span><span class="sxs-lookup"><span data-stu-id="4f18c-280"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-281"><strong>Descrição</strong></span><span class="sxs-lookup"><span data-stu-id="4f18c-281"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-282"><strong>Exemplos</strong></span><span class="sxs-lookup"><span data-stu-id="4f18c-282"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-283">Operadores matemáticos usados em expressões numéricas.</span><span class="sxs-lookup"><span data-stu-id="4f18c-283">Math operators used in numerical expressions.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-284">Atribuição e igualdade.</span><span class="sxs-lookup"><span data-stu-id="4f18c-284">Assignment and equality.</span></span> <span data-ttu-id="4f18c-285">Dependendo do contexto, o atribui o valor no lado direito de uma instrução ao objeto no lado esquerdo ou verifica os valores de igualdade.</span><span class="sxs-lookup"><span data-stu-id="4f18c-285">Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-286">Desigualdade.</span><span class="sxs-lookup"><span data-stu-id="4f18c-286">Inequality.</span></span> <span data-ttu-id="4f18c-287">Retorna `True` se os valores não forem iguais.</span><span class="sxs-lookup"><span data-stu-id="4f18c-287">Returns `True` if the values are not equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-288">Menor que, maior que, menor ou igual a ou maior que ou igual a.</span><span class="sxs-lookup"><span data-stu-id="4f18c-288">Less than, greater than, less than or equal, and greater than or equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-289">Concatenação, que é usada para unir cadeias de caracteres.</span><span class="sxs-lookup"><span data-stu-id="4f18c-289">Concatenation, which is used to join strings.</span></span>
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-290">Os operadores de incremento e decremento, que adicionam e subtraim 1 (respectivamente) de uma variável.</span><span class="sxs-lookup"><span data-stu-id="4f18c-290">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-291">Final.</span><span class="sxs-lookup"><span data-stu-id="4f18c-291">Dot.</span></span> <span data-ttu-id="4f18c-292">Usado para distinguir objetos e suas propriedades e métodos.</span><span class="sxs-lookup"><span data-stu-id="4f18c-292">Used to distinguish objects and their properties and methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-293">Parênteses.</span><span class="sxs-lookup"><span data-stu-id="4f18c-293">Parentheses.</span></span> <span data-ttu-id="4f18c-294">Usado para agrupar expressões, para passar parâmetros para métodos e para acessar membros de matrizes e coleções.</span><span class="sxs-lookup"><span data-stu-id="4f18c-294">Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.</span></span>
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-295">Válido.</span><span class="sxs-lookup"><span data-stu-id="4f18c-295">Not.</span></span> <span data-ttu-id="4f18c-296">Reverte um valor verdadeiro para false e vice-versa.</span><span class="sxs-lookup"><span data-stu-id="4f18c-296">Reverses a true value to false and vice versa.</span></span> <span data-ttu-id="4f18c-297">Normalmente usado como uma maneira abreviada de testar `False` (ou seja, para não `True`).</span><span class="sxs-lookup"><span data-stu-id="4f18c-297">Typically used as a shorthand way to test for `False` (that is, for not `True`).</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::

---

:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        <span data-ttu-id="4f18c-298">AND lógico e OR, que são usados para vincular condições juntas.</span><span class="sxs-lookup"><span data-stu-id="4f18c-298">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="4f18c-299">Trabalhando com caminhos de arquivos e pastas no código</span><span class="sxs-lookup"><span data-stu-id="4f18c-299">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="4f18c-300">Com freqüência, você trabalhará com caminhos de arquivos e pastas em seu código.</span><span class="sxs-lookup"><span data-stu-id="4f18c-300">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="4f18c-301">Aqui está um exemplo de estrutura de pasta física para um site como pode aparecer em seu computador de desenvolvimento:</span><span class="sxs-lookup"><span data-stu-id="4f18c-301">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="4f18c-302">Aqui estão alguns detalhes essenciais sobre URLs e caminhos:</span><span class="sxs-lookup"><span data-stu-id="4f18c-302">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="4f18c-303">Uma URL começa com um nome de domínio (`http://www.example.com`) ou um nome de servidor (`http://localhost`, `http://mycomputer`).</span><span class="sxs-lookup"><span data-stu-id="4f18c-303">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="4f18c-304">Uma URL corresponde a um caminho físico em um computador host.</span><span class="sxs-lookup"><span data-stu-id="4f18c-304">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="4f18c-305">Por exemplo, `http://myserver` pode corresponder à pasta *C:\websites\mywebsite* no servidor.</span><span class="sxs-lookup"><span data-stu-id="4f18c-305">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="4f18c-306">Um caminho virtual é abreviado para representar caminhos no código sem precisar especificar o caminho completo.</span><span class="sxs-lookup"><span data-stu-id="4f18c-306">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="4f18c-307">Ele inclui a parte de uma URL que segue o nome do domínio ou do servidor.</span><span class="sxs-lookup"><span data-stu-id="4f18c-307">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="4f18c-308">Ao usar caminhos virtuais, você pode mover seu código para um domínio ou servidor diferente sem precisar atualizar os caminhos.</span><span class="sxs-lookup"><span data-stu-id="4f18c-308">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="4f18c-309">Aqui está um exemplo para ajudá-lo a entender as diferenças:</span><span class="sxs-lookup"><span data-stu-id="4f18c-309">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="4f18c-310">URL completa</span><span class="sxs-lookup"><span data-stu-id="4f18c-310">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="4f18c-311">Nome do servidor</span><span class="sxs-lookup"><span data-stu-id="4f18c-311">Server name</span></span> | <span data-ttu-id="4f18c-312">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="4f18c-312">*mycompanyserver*</span></span> |
| <span data-ttu-id="4f18c-313">Caminho virtual</span><span class="sxs-lookup"><span data-stu-id="4f18c-313">Virtual path</span></span> | <span data-ttu-id="4f18c-314">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="4f18c-314">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="4f18c-315">Caminho físico</span><span class="sxs-lookup"><span data-stu-id="4f18c-315">Physical path</span></span> | <span data-ttu-id="4f18c-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="4f18c-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="4f18c-317">A raiz virtual é/, assim como a raiz da sua unidade C: é \.</span><span class="sxs-lookup"><span data-stu-id="4f18c-317">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="4f18c-318">(Os caminhos de pastas virtuais sempre usam barras "/".) O caminho virtual de uma pasta não precisa ter o mesmo nome que a pasta física; pode ser um alias.</span><span class="sxs-lookup"><span data-stu-id="4f18c-318">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="4f18c-319">(Em servidores de produção, o caminho virtual raramente corresponde a um caminho físico exato.)</span><span class="sxs-lookup"><span data-stu-id="4f18c-319">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="4f18c-320">Quando você trabalha com arquivos e pastas no código, às vezes você precisa referenciar o caminho físico e, às vezes, um caminho virtual, dependendo de quais objetos você está trabalhando.</span><span class="sxs-lookup"><span data-stu-id="4f18c-320">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="4f18c-321">O ASP.NET fornece essas ferramentas para trabalhar com caminhos de arquivos e pastas no código: o método `Server.MapPath` e o operador `~` e o método `Href`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-321">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="4f18c-322">Convertendo caminhos virtuais em físicos: o método Server. MapPath</span><span class="sxs-lookup"><span data-stu-id="4f18c-322">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="4f18c-323">O método `Server.MapPath` converte um caminho virtual (como */Default.cshtml*) em um caminho físico absoluto (como *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="4f18c-323">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="4f18c-324">Você usa esse método sempre que precisar de um caminho físico completo.</span><span class="sxs-lookup"><span data-stu-id="4f18c-324">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="4f18c-325">Um exemplo típico é quando você está lendo ou gravando um arquivo de texto ou de imagem no servidor Web.</span><span class="sxs-lookup"><span data-stu-id="4f18c-325">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="4f18c-326">Normalmente, você não sabe o caminho físico absoluto do seu site no servidor de um site de hospedagem, portanto, esse método pode converter o caminho que você conhece — o caminho virtual — para o caminho correspondente no servidor para você.</span><span class="sxs-lookup"><span data-stu-id="4f18c-326">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="4f18c-327">Você passa o caminho virtual para um arquivo ou pasta para o método e retorna o caminho físico:</span><span class="sxs-lookup"><span data-stu-id="4f18c-327">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="4f18c-328">Referenciando a raiz virtual: o operador ~ e o método href</span><span class="sxs-lookup"><span data-stu-id="4f18c-328">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="4f18c-329">Em um arquivo *. cshtml* ou *. vbhtml* , você pode fazer referência ao caminho raiz virtual usando o operador de `~`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-329">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="4f18c-330">Isso é muito útil porque você pode mover páginas em um site e quaisquer links que eles contêm para outras páginas não serão quebrados.</span><span class="sxs-lookup"><span data-stu-id="4f18c-330">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="4f18c-331">Também é útil caso você sempre mova seu site para um local diferente.</span><span class="sxs-lookup"><span data-stu-id="4f18c-331">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="4f18c-332">Estes são alguns exemplos:</span><span class="sxs-lookup"><span data-stu-id="4f18c-332">Here are some examples:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

<span data-ttu-id="4f18c-333">Se o site for `http://myserver/myapp`, aqui está como o ASP.NET tratará esses caminhos quando a página for executada:</span><span class="sxs-lookup"><span data-stu-id="4f18c-333">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="4f18c-334">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="4f18c-334">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="4f18c-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="4f18c-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="4f18c-336">(Você realmente não verá esses caminhos como os valores da variável, mas ASP.NET tratará os caminhos como se fosse isso.)</span><span class="sxs-lookup"><span data-stu-id="4f18c-336">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="4f18c-337">Você pode usar o operador de `~` no código do servidor (como acima) e na marcação, desta forma:</span><span class="sxs-lookup"><span data-stu-id="4f18c-337">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

<span data-ttu-id="4f18c-338">Na marcação, você usa o operador `~` para criar caminhos para recursos como arquivos de imagem, outras páginas da Web e arquivos CSS.</span><span class="sxs-lookup"><span data-stu-id="4f18c-338">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="4f18c-339">Quando a página é executada, o ASP.NET examina a página (código e marcação) e resolve todas as referências de `~` para o caminho apropriado.</span><span class="sxs-lookup"><span data-stu-id="4f18c-339">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="4f18c-340">Lógica condicional e loops</span><span class="sxs-lookup"><span data-stu-id="4f18c-340">Conditional Logic and Loops</span></span>

<span data-ttu-id="4f18c-341">O código do servidor ASP.NET permite executar tarefas com base em condições e escrever código que repete instruções em um número específico de vezes, o código que executa um loop).</span><span class="sxs-lookup"><span data-stu-id="4f18c-341">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="4f18c-342">Condições de teste</span><span class="sxs-lookup"><span data-stu-id="4f18c-342">Testing conditions</span></span>

<span data-ttu-id="4f18c-343">Para testar uma condição simples, use a instrução `If...Then`, que retorna `True` ou `False` com base em um teste que você especificar:</span><span class="sxs-lookup"><span data-stu-id="4f18c-343">To test a simple condition you use the `If...Then` statement, which returns `True` or `False` based on a test you specify:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

<span data-ttu-id="4f18c-344">A palavra-chave `If` inicia um bloco.</span><span class="sxs-lookup"><span data-stu-id="4f18c-344">The `If` keyword starts a block.</span></span> <span data-ttu-id="4f18c-345">O teste real (condição) segue a palavra-chave `If` e retorna true ou false.</span><span class="sxs-lookup"><span data-stu-id="4f18c-345">The actual test (condition) follows the `If` keyword and returns true or false.</span></span> <span data-ttu-id="4f18c-346">A instrução `If` termina com `Then`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-346">The `If` statement ends with `Then`.</span></span> <span data-ttu-id="4f18c-347">As instruções que serão executadas se o teste for true serão delimitadas por `If` e `End If`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-347">The statements that will run if the test is true are enclosed by `If` and `End If`.</span></span> <span data-ttu-id="4f18c-348">Uma instrução `If` pode incluir um bloco `Else` que especifica as instruções a serem executadas se a condição for falsa:</span><span class="sxs-lookup"><span data-stu-id="4f18c-348">An `If` statement can include an `Else` block that specifies statements to run if the condition is false:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

<span data-ttu-id="4f18c-349">Se uma instrução `If` iniciar um bloco de código, você não precisará usar as instruções `Code...End Code` normais para incluir os blocos.</span><span class="sxs-lookup"><span data-stu-id="4f18c-349">If an `If` statement starts a code block, you don't have to use the normal `Code...End Code` statements to include the blocks.</span></span> <span data-ttu-id="4f18c-350">Você pode apenas adicionar `@` ao bloco e ele funcionará.</span><span class="sxs-lookup"><span data-stu-id="4f18c-350">You can just add `@` to the block, and it will work.</span></span> <span data-ttu-id="4f18c-351">Essa abordagem funciona com `If`, bem como outras palavras-chave de programação de Visual Basic que são seguidas por blocos de código, incluindo `For`, `For Each`, `Do While`, etc.</span><span class="sxs-lookup"><span data-stu-id="4f18c-351">This approach works with `If` as well as other Visual Basic programming keywords that are followed by code blocks, including `For`, `For Each`, `Do While`, etc.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

<span data-ttu-id="4f18c-352">Você pode adicionar várias condições usando um ou mais blocos de `ElseIf`:</span><span class="sxs-lookup"><span data-stu-id="4f18c-352">You can add multiple conditions using one or more `ElseIf` blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

<span data-ttu-id="4f18c-353">Neste exemplo, se a primeira condição no bloco de `If` não for verdadeira, a condição de `ElseIf` será verificada.</span><span class="sxs-lookup"><span data-stu-id="4f18c-353">In this example, if the first condition in the `If` block is not true, the `ElseIf` condition is checked.</span></span> <span data-ttu-id="4f18c-354">Se essa condição for atendida, as instruções no bloco de `ElseIf` serão executadas.</span><span class="sxs-lookup"><span data-stu-id="4f18c-354">If that condition is met, the statements in the `ElseIf` block are executed.</span></span> <span data-ttu-id="4f18c-355">Se nenhuma das condições for atendida, as instruções no bloco de `Else` serão executadas.</span><span class="sxs-lookup"><span data-stu-id="4f18c-355">If none of the conditions are met, the statements in the `Else` block are executed.</span></span> <span data-ttu-id="4f18c-356">Você pode adicionar qualquer número de blocos de `ElseIf` e, em seguida, fechar com um bloco de `Else` como a condição de &quot;todo o resto&quot;.</span><span class="sxs-lookup"><span data-stu-id="4f18c-356">You can add any number of `ElseIf` blocks, and then close with an `Else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="4f18c-357">Para testar um grande número de condições, use um bloco de `Select Case`:</span><span class="sxs-lookup"><span data-stu-id="4f18c-357">To test a large number of conditions, use a `Select Case` block:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

<span data-ttu-id="4f18c-358">O valor a ser testado está entre parênteses (no exemplo, a variável de dia da semana).</span><span class="sxs-lookup"><span data-stu-id="4f18c-358">The value to test is in parentheses (in the example, the weekday variable).</span></span> <span data-ttu-id="4f18c-359">Cada teste individual usa uma instrução `Case` que lista um valor.</span><span class="sxs-lookup"><span data-stu-id="4f18c-359">Each individual test uses a `Case` statement that lists a value.</span></span> <span data-ttu-id="4f18c-360">Se o valor de uma instrução `Case` corresponder ao valor de teste, o código nesse bloco de `Case` será executado.</span><span class="sxs-lookup"><span data-stu-id="4f18c-360">If the value of a `Case` statement matches the test value, the code in that `Case` block is executed.</span></span>

<span data-ttu-id="4f18c-361">O resultado dos dois últimos blocos condicionais exibidos em um navegador:</span><span class="sxs-lookup"><span data-stu-id="4f18c-361">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="4f18c-363">Código de loop</span><span class="sxs-lookup"><span data-stu-id="4f18c-363">Looping code</span></span>

<span data-ttu-id="4f18c-364">Muitas vezes, você precisa executar as mesmas instruções repetidamente.</span><span class="sxs-lookup"><span data-stu-id="4f18c-364">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="4f18c-365">Faça isso fazendo um loop.</span><span class="sxs-lookup"><span data-stu-id="4f18c-365">You do this by looping.</span></span> <span data-ttu-id="4f18c-366">Por exemplo, você geralmente executa as mesmas instruções para cada item em uma coleção de dados.</span><span class="sxs-lookup"><span data-stu-id="4f18c-366">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="4f18c-367">Se você souber exatamente quantas vezes deseja executar um loop, você pode usar um loop `For`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-367">If you know exactly how many times you want to loop, you can use a `For` loop.</span></span> <span data-ttu-id="4f18c-368">Esse tipo de loop é especialmente útil para contar ou contar para baixo:</span><span class="sxs-lookup"><span data-stu-id="4f18c-368">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

<span data-ttu-id="4f18c-369">O loop começa com a palavra-chave `For`, seguida por três elementos:</span><span class="sxs-lookup"><span data-stu-id="4f18c-369">The loop begins with the `For` keyword, followed by three elements:</span></span>

- <span data-ttu-id="4f18c-370">Imediatamente após a instrução `For`, você declara uma variável de contador (você não precisa usar `Dim`) e, em seguida, indica o intervalo, como em `i = 10 to 20`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-370">Immediately after the `For` statement, you declare a counter variable (you don't have to use `Dim`) and then indicate the range, as in `i = 10 to 20`.</span></span> <span data-ttu-id="4f18c-371">Isso significa que a variável `i` começará a contar em 10 e continuará até atingir 20 (inclusivo).</span><span class="sxs-lookup"><span data-stu-id="4f18c-371">This means the variable `i` will start counting at 10 and continue until it reaches 20 (inclusive).</span></span>
- <span data-ttu-id="4f18c-372">Entre as instruções `For` e `Next` é o conteúdo do bloco.</span><span class="sxs-lookup"><span data-stu-id="4f18c-372">Between the `For` and `Next` statements is the content of the block.</span></span> <span data-ttu-id="4f18c-373">Isso pode conter uma ou mais instruções de código que são executadas com cada loop.</span><span class="sxs-lookup"><span data-stu-id="4f18c-373">This can contain one or more code statements that execute with each loop.</span></span>
- <span data-ttu-id="4f18c-374">A instrução `Next i` encerra o loop.</span><span class="sxs-lookup"><span data-stu-id="4f18c-374">The `Next i` statement ends the loop.</span></span> <span data-ttu-id="4f18c-375">Ele incrementa o contador e inicia a próxima iteração do loop.</span><span class="sxs-lookup"><span data-stu-id="4f18c-375">It increments the counter and starts the next iteration of the loop.</span></span>

<span data-ttu-id="4f18c-376">A linha de código entre as linhas `For` e `Next` contém o código que é executado para cada iteração do loop.</span><span class="sxs-lookup"><span data-stu-id="4f18c-376">The line of code between the `For` and `Next` lines contains the code that runs for each iteration of the loop.</span></span> <span data-ttu-id="4f18c-377">A marcação cria um novo parágrafo (`<p>` elemento) a cada vez e adiciona uma linha à saída, exibindo o valor de i (o contador).</span><span class="sxs-lookup"><span data-stu-id="4f18c-377">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of i (the counter).</span></span> <span data-ttu-id="4f18c-378">Quando você executa essa página, o exemplo cria 11 linhas exibindo a saída, com o texto em cada linha indicando o número do item.</span><span class="sxs-lookup"><span data-stu-id="4f18c-378">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

<span data-ttu-id="4f18c-380">Se você estiver trabalhando com uma coleção ou matriz, geralmente usará um loop `For Each`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-380">If you're working with a collection or array, you often use a `For Each` loop.</span></span> <span data-ttu-id="4f18c-381">Uma coleção é um grupo de objetos semelhantes, e o loop de `For Each` permite que você execute uma tarefa em cada item da coleção.</span><span class="sxs-lookup"><span data-stu-id="4f18c-381">A collection is a group of similar objects, and the `For Each` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="4f18c-382">Esse tipo de loop é conveniente para coleções, porque, ao contrário de um loop de `For`, você não precisa incrementar o contador ou definir um limite.</span><span class="sxs-lookup"><span data-stu-id="4f18c-382">This type of loop is convenient for collections, because unlike a `For` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="4f18c-383">Em vez disso, o código de loop de `For Each` simplesmente prossegue pela coleção até que seja concluído.</span><span class="sxs-lookup"><span data-stu-id="4f18c-383">Instead, the `For Each` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="4f18c-384">Este exemplo retorna os itens na coleção de `Request.ServerVariables` (que contém informações sobre seu servidor Web).</span><span class="sxs-lookup"><span data-stu-id="4f18c-384">This example returns the items in the `Request.ServerVariables` collection (which contains information about your web server).</span></span> <span data-ttu-id="4f18c-385">Ele usa um loop `For Each` para exibir o nome de cada item criando um novo elemento `<li>` em uma lista com marcadores HTML.</span><span class="sxs-lookup"><span data-stu-id="4f18c-385">It uses a `For Each` loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

<span data-ttu-id="4f18c-386">A palavra-chave `For Each` é seguida por uma variável que representa um único item na coleção (no exemplo, `myItem`), seguido pela palavra-chave `In`, seguida pela coleção na qual você deseja executar o loop.</span><span class="sxs-lookup"><span data-stu-id="4f18c-386">The `For Each` keyword is followed by a variable that represents a single item in the collection (in the example, `myItem`), followed by the `In` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="4f18c-387">No corpo do loop de `For Each`, você pode acessar o item atual usando a variável que você declarou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="4f18c-387">In the body of the `For Each` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

<span data-ttu-id="4f18c-389">Para criar um loop de finalidade mais geral, use a instrução `Do While`:</span><span class="sxs-lookup"><span data-stu-id="4f18c-389">To create a more general-purpose loop, use the `Do While` statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

<span data-ttu-id="4f18c-390">Esse loop começa com a palavra-chave `Do While`, seguida por uma condição, seguida pelo bloco a ser repetido.</span><span class="sxs-lookup"><span data-stu-id="4f18c-390">This loop begins with the `Do While` keyword, followed by a condition, followed by the block to repeat.</span></span> <span data-ttu-id="4f18c-391">Os loops normalmente incrementam (adicionam) ou decrementam (subtraim de) uma variável ou objeto usado para contagem.</span><span class="sxs-lookup"><span data-stu-id="4f18c-391">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="4f18c-392">No exemplo, o operador de `+=` adiciona 1 ao valor de uma variável toda vez que o loop é executado.</span><span class="sxs-lookup"><span data-stu-id="4f18c-392">In the example, the `+=` operator adds 1 to the value of a variable each time the loop runs.</span></span> <span data-ttu-id="4f18c-393">(Para diminuir uma variável em um loop que conta inativo, você usaria o operador decremento `-=`.)</span><span class="sxs-lookup"><span data-stu-id="4f18c-393">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`.)</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="4f18c-394">Objetos e coleções</span><span class="sxs-lookup"><span data-stu-id="4f18c-394">Objects and Collections</span></span>

<span data-ttu-id="4f18c-395">Quase tudo em um site ASP.NET é um objeto, incluindo a própria página da Web.</span><span class="sxs-lookup"><span data-stu-id="4f18c-395">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="4f18c-396">Esta seção aborda alguns objetos importantes com os quais você trabalhará com frequência em seu código.</span><span class="sxs-lookup"><span data-stu-id="4f18c-396">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="4f18c-397">Objetos de página</span><span class="sxs-lookup"><span data-stu-id="4f18c-397">Page objects</span></span>

<span data-ttu-id="4f18c-398">O objeto mais básico em ASP.NET é a página.</span><span class="sxs-lookup"><span data-stu-id="4f18c-398">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="4f18c-399">Você pode acessar as propriedades do objeto Page diretamente sem nenhum objeto qualificado.</span><span class="sxs-lookup"><span data-stu-id="4f18c-399">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="4f18c-400">O código a seguir obtém o caminho do arquivo da página, usando o objeto `Request` da página:</span><span class="sxs-lookup"><span data-stu-id="4f18c-400">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

<span data-ttu-id="4f18c-401">Você pode usar as propriedades do objeto `Page` para obter muitas informações, como:</span><span class="sxs-lookup"><span data-stu-id="4f18c-401">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="4f18c-402">`Request`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-402">`Request`.</span></span> <span data-ttu-id="4f18c-403">Como você já viu, trata-se de uma coleção de informações sobre a solicitação atual, incluindo que tipo de navegador fez a solicitação, a URL da página, a identidade do usuário, etc.</span><span class="sxs-lookup"><span data-stu-id="4f18c-403">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="4f18c-404">`Response`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-404">`Response`.</span></span> <span data-ttu-id="4f18c-405">Esta é uma coleção de informações sobre a resposta (página) que será enviada ao navegador quando o código do servidor terminar a execução.</span><span class="sxs-lookup"><span data-stu-id="4f18c-405">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="4f18c-406">Por exemplo, você pode usar essa propriedade para gravar informações na resposta.</span><span class="sxs-lookup"><span data-stu-id="4f18c-406">For example, you can use this property to write information into the response.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="4f18c-407">Objetos de coleção (matrizes e dicionários)</span><span class="sxs-lookup"><span data-stu-id="4f18c-407">Collection objects (arrays and dictionaries)</span></span>

<span data-ttu-id="4f18c-408">Uma coleção é um grupo de objetos do mesmo tipo, como uma coleção de objetos `Customer` de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f18c-408">A collection is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="4f18c-409">ASP.NET contém muitas coleções internas, como a coleção de `Request.Files`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-409">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="4f18c-410">Você geralmente trabalhará com dados em coleções.</span><span class="sxs-lookup"><span data-stu-id="4f18c-410">You'll often work with data in collections.</span></span> <span data-ttu-id="4f18c-411">Dois tipos de coleção comuns são a *matriz* e o *dicionário*.</span><span class="sxs-lookup"><span data-stu-id="4f18c-411">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="4f18c-412">Uma matriz é útil quando você deseja armazenar uma coleção de itens semelhantes, mas não quer criar uma variável separada para conter cada item:</span><span class="sxs-lookup"><span data-stu-id="4f18c-412">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

<span data-ttu-id="4f18c-413">Com matrizes, você declara um tipo de dados específico, como `String`, `Integer`ou `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-413">With arrays, you declare a specific data type, such as `String`, `Integer`, or `DateTime`.</span></span> <span data-ttu-id="4f18c-414">Para indicar que a variável pode conter uma matriz, você adiciona parênteses ao nome da variável na declaração (como `Dim myVar() As String`).</span><span class="sxs-lookup"><span data-stu-id="4f18c-414">To indicate that the variable can contain an array, you add parentheses to the variable name in the declaration (such as `Dim myVar() As String`).</span></span> <span data-ttu-id="4f18c-415">Você pode acessar itens em uma matriz usando sua posição (índice) ou usando a instrução `For Each`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-415">You can access items in an array using their position (index) or by using the `For Each` statement.</span></span> <span data-ttu-id="4f18c-416">Os índices de matriz são baseados &#8212; em zero ou seja, o primeiro item está na posição 0, o segundo item está na posição 1 e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="4f18c-416">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

<span data-ttu-id="4f18c-417">Você pode determinar o número de itens em uma matriz obtendo sua propriedade `Length`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-417">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="4f18c-418">Para obter a posição de um item específico na matriz (ou seja, para pesquisar a matriz), use o método `Array.IndexOf`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-418">To get the position of a specific item in the array (that is, to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="4f18c-419">Você também pode fazer coisas como reverter o conteúdo de uma matriz (o método `Array.Reverse`) ou classificar o conteúdo (o método `Array.Sort`).</span><span class="sxs-lookup"><span data-stu-id="4f18c-419">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="4f18c-420">A saída do código da matriz de cadeia de caracteres exibido em um navegador:</span><span class="sxs-lookup"><span data-stu-id="4f18c-420">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

<span data-ttu-id="4f18c-422">Um dicionário é uma coleção de pares de chave/valor, em que você fornece a chave (ou nome) para definir ou recuperar o valor correspondente:</span><span class="sxs-lookup"><span data-stu-id="4f18c-422">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

<span data-ttu-id="4f18c-423">Para criar um dicionário, use a palavra-chave `New` para indicar que você está criando um novo objeto de `Dictionary`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-423">To create a dictionary, you use the `New` keyword to indicate that you're creating a new `Dictionary` object.</span></span> <span data-ttu-id="4f18c-424">Você pode atribuir um dicionário a uma variável usando a palavra-chave `Dim`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-424">You can assign a dictionary to a variable using the `Dim` keyword.</span></span> <span data-ttu-id="4f18c-425">Você indica os tipos de dados dos itens no dicionário usando parênteses (`( )`).</span><span class="sxs-lookup"><span data-stu-id="4f18c-425">You indicate the data types of the items in the dictionary using parentheses ( `( )` ).</span></span> <span data-ttu-id="4f18c-426">No final da declaração, você deve adicionar outro par de parênteses, pois esse é, na verdade, um método que cria um novo dicionário.</span><span class="sxs-lookup"><span data-stu-id="4f18c-426">At the end of the declaration, you must add another pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="4f18c-427">Para adicionar itens ao dicionário, você pode chamar o método `Add` da variável Dictionary (`myScores` nesse caso) e, em seguida, especificar uma chave e um valor.</span><span class="sxs-lookup"><span data-stu-id="4f18c-427">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="4f18c-428">Como alternativa, você pode usar parênteses para indicar a chave e fazer uma atribuição simples, como no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="4f18c-428">Alternatively, you can use parentheses to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

<span data-ttu-id="4f18c-429">Para obter um valor do dicionário, especifique a chave entre parênteses:</span><span class="sxs-lookup"><span data-stu-id="4f18c-429">To get a value from the dictionary, you specify the key in parentheses:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="4f18c-430">Chamando métodos com parâmetros</span><span class="sxs-lookup"><span data-stu-id="4f18c-430">Calling Methods with Parameters</span></span>

<span data-ttu-id="4f18c-431">Como vimos anteriormente neste artigo, os objetos com os quais você programau têm métodos.</span><span class="sxs-lookup"><span data-stu-id="4f18c-431">As you saw earlier in this article, the objects that you program with have methods.</span></span> <span data-ttu-id="4f18c-432">Por exemplo, um objeto `Database` pode ter um método `Database.Connect`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-432">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="4f18c-433">Muitos métodos também têm um ou mais parâmetros.</span><span class="sxs-lookup"><span data-stu-id="4f18c-433">Many methods also have one or more parameters.</span></span> <span data-ttu-id="4f18c-434">Um *parâmetro* é um valor que você passa para um método para permitir que o método conclua sua tarefa.</span><span class="sxs-lookup"><span data-stu-id="4f18c-434">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="4f18c-435">Por exemplo, examine uma declaração para o método `Request.MapPath`, que usa três parâmetros:</span><span class="sxs-lookup"><span data-stu-id="4f18c-435">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

<span data-ttu-id="4f18c-436">Esse método retorna o caminho físico no servidor que corresponde a um caminho virtual especificado.</span><span class="sxs-lookup"><span data-stu-id="4f18c-436">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="4f18c-437">Os três parâmetros para o método são `virtualPath`, `baseVirtualDir`e `allowCrossAppMapping`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-437">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="4f18c-438">(Observe que na declaração, os parâmetros são listados com os tipos de dados dos dados que eles aceitarão.) Ao chamar esse método, você deve fornecer valores para todos os três parâmetros.</span><span class="sxs-lookup"><span data-stu-id="4f18c-438">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="4f18c-439">Quando você estiver usando Visual Basic com a sintaxe Razor, terá duas opções para passar parâmetros para um método: *parâmetros posicionais* ou *parâmetros nomeados*.</span><span class="sxs-lookup"><span data-stu-id="4f18c-439">When you're using Visual Basic with the Razor syntax, you have two options for passing parameters to a method: *positional parameters* or *named parameters*.</span></span> <span data-ttu-id="4f18c-440">Para chamar um método usando parâmetros posicionais, você passa os parâmetros em uma ordem estrita que é especificada na declaração do método.</span><span class="sxs-lookup"><span data-stu-id="4f18c-440">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="4f18c-441">(Normalmente, você sabe esse pedido lendo a documentação do método.) Você deve seguir a ordem e não pode ignorar nenhum dos parâmetros &#8212; , se necessário, você passa uma cadeia de caracteres vazia (`""`) ou NULL para um parâmetro posicional para o qual você não tem um valor.</span><span class="sxs-lookup"><span data-stu-id="4f18c-441">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or null for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="4f18c-442">O exemplo a seguir pressupõe que você tenha uma pasta chamada *scripts* em seu site.</span><span class="sxs-lookup"><span data-stu-id="4f18c-442">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="4f18c-443">O código chama o método `Request.MapPath` e passa valores para os três parâmetros na ordem correta.</span><span class="sxs-lookup"><span data-stu-id="4f18c-443">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="4f18c-444">Em seguida, ele exibe o caminho mapeado resultante.</span><span class="sxs-lookup"><span data-stu-id="4f18c-444">It then displays the resulting mapped path.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

<span data-ttu-id="4f18c-445">Quando há muitos parâmetros para um método, você pode manter o seu código mais limpo e mais legível usando parâmetros nomeados.</span><span class="sxs-lookup"><span data-stu-id="4f18c-445">When there are many parameters for a method, you can keep your code cleaner and more readable by using named parameters.</span></span> <span data-ttu-id="4f18c-446">Para chamar um método usando parâmetros nomeados, especifique o nome do parâmetro seguido por `:=` e, em seguida, forneça o valor.</span><span class="sxs-lookup"><span data-stu-id="4f18c-446">To call a method using named parameters, specify the parameter name followed by `:=` and then provide the value.</span></span> <span data-ttu-id="4f18c-447">Uma vantagem dos parâmetros nomeados é que você pode adicioná-los em qualquer ordem desejada.</span><span class="sxs-lookup"><span data-stu-id="4f18c-447">An advantage of named parameters is that you can add them in any order you want.</span></span> <span data-ttu-id="4f18c-448">(Uma desvantagem é que a chamada de método não é compactada.)</span><span class="sxs-lookup"><span data-stu-id="4f18c-448">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="4f18c-449">O exemplo a seguir chama o mesmo método mostrado acima, mas usa parâmetros nomeados para fornecer os valores:</span><span class="sxs-lookup"><span data-stu-id="4f18c-449">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

<span data-ttu-id="4f18c-450">Como você pode ver, os parâmetros são passados em uma ordem diferente.</span><span class="sxs-lookup"><span data-stu-id="4f18c-450">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="4f18c-451">No entanto, se você executar o exemplo anterior e este exemplo, eles retornarão o mesmo valor.</span><span class="sxs-lookup"><span data-stu-id="4f18c-451">However, if you run the previous example and this example, they'll return the same value.</span></span>

## <a name="handling-errors"></a><span data-ttu-id="4f18c-452">Tratando erros</span><span class="sxs-lookup"><span data-stu-id="4f18c-452">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="4f18c-453">Instruções Try-Catch</span><span class="sxs-lookup"><span data-stu-id="4f18c-453">Try-Catch statements</span></span>

<span data-ttu-id="4f18c-454">Muitas vezes, você terá instruções em seu código que podem falhar por motivos fora do seu controle.</span><span class="sxs-lookup"><span data-stu-id="4f18c-454">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="4f18c-455">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="4f18c-455">For example:</span></span>

- <span data-ttu-id="4f18c-456">Se o seu código tentar abrir, criar, ler ou gravar um arquivo, todos os tipos de erros poderão ocorrer.</span><span class="sxs-lookup"><span data-stu-id="4f18c-456">If your code tries to open, create, read, or write a file, all sorts of errors might occur.</span></span> <span data-ttu-id="4f18c-457">O arquivo desejado talvez não exista, ele pode estar bloqueado, o código pode não ter permissões e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="4f18c-457">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="4f18c-458">Da mesma forma, se o seu código tentar atualizar os registros em um banco de dados, poderá haver problemas de permissões, a conexão com o banco de dados pode ser descartada, o que pode ser salvo, e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="4f18c-458">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="4f18c-459">Em termos de programação, essas situações são chamadas de *exceções*.</span><span class="sxs-lookup"><span data-stu-id="4f18c-459">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="4f18c-460">Se o código encontrar uma exceção, ele gerará (gera) uma mensagem de erro que é, no melhor, irritante para os usuários.</span><span class="sxs-lookup"><span data-stu-id="4f18c-460">If your code encounters an exception, it generates (throws) an error message that is, at best, annoying to users.</span></span>

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

<span data-ttu-id="4f18c-462">Em situações em que o código pode encontrar exceções e, para evitar mensagens de erro desse tipo, você pode usar instruções de `Try/Catch`.</span><span class="sxs-lookup"><span data-stu-id="4f18c-462">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `Try/Catch` statements.</span></span> <span data-ttu-id="4f18c-463">Na instrução `Try`, você executa o código que está verificando.</span><span class="sxs-lookup"><span data-stu-id="4f18c-463">In the `Try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="4f18c-464">Em uma ou mais instruções `Catch`, você pode procurar erros específicos (tipos específicos de exceções) que podem ter ocorrido.</span><span class="sxs-lookup"><span data-stu-id="4f18c-464">In one or more `Catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="4f18c-465">Você pode incluir tantas `Catch` instruções quantas forem necessárias para procurar erros que você está prevendo.</span><span class="sxs-lookup"><span data-stu-id="4f18c-465">You can include as many `Catch` statements as you need to look for errors that you're anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="4f18c-466">Recomendamos que você evite usar o método `Response.Redirect` em instruções `Try/Catch`, pois isso pode causar uma exceção em sua página.</span><span class="sxs-lookup"><span data-stu-id="4f18c-466">We recommend that you avoid using the `Response.Redirect` method in `Try/Catch` statements, because it can cause an exception in your page.</span></span>

<span data-ttu-id="4f18c-467">O exemplo a seguir mostra uma página que cria um arquivo de texto na primeira solicitação e, em seguida, exibe um botão que permite ao usuário abrir o arquivo.</span><span class="sxs-lookup"><span data-stu-id="4f18c-467">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="4f18c-468">O exemplo deliberadamente usa um nome de arquivo inadequado para que cause uma exceção.</span><span class="sxs-lookup"><span data-stu-id="4f18c-468">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="4f18c-469">O código inclui `Catch` instruções para duas possíveis exceções: `FileNotFoundException`, que ocorrerá se o nome do arquivo for insatisfatório, e `DirectoryNotFoundException`, que ocorrerá se ASP.NET não conseguir localizar a pasta.</span><span class="sxs-lookup"><span data-stu-id="4f18c-469">The code includes `Catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="4f18c-470">(Você pode remover o comentário de uma instrução no exemplo para ver como ela é executada quando tudo funciona corretamente.)</span><span class="sxs-lookup"><span data-stu-id="4f18c-470">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="4f18c-471">Se o código não tratar a exceção, você verá uma página de erro como a captura de tela anterior.</span><span class="sxs-lookup"><span data-stu-id="4f18c-471">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="4f18c-472">No entanto, a seção `Try/Catch` ajuda a impedir que o usuário veja esses tipos de erros.</span><span class="sxs-lookup"><span data-stu-id="4f18c-472">However, the `Try/Catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a><span data-ttu-id="4f18c-473">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="4f18c-473">Additional Resources</span></span>

### <a name="reference-documentation"></a><span data-ttu-id="4f18c-474">Documentação de referência</span><span class="sxs-lookup"><span data-stu-id="4f18c-474">Reference Documentation</span></span>

- [<span data-ttu-id="4f18c-475">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4f18c-475">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)
- [<span data-ttu-id="4f18c-476">Linguagem Visual Basic</span><span class="sxs-lookup"><span data-stu-id="4f18c-476">Visual Basic Language</span></span>](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
