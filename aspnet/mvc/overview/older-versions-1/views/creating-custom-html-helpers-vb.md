---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Criando auxiliares HTML personalizados (VB) | Microsoft Docs
author: microsoft
description: O objetivo deste tutorial é demonstrar como você pode criar auxiliares HTML personalizados que podem ser usados em suas exibições do MVC. Aproveitando o auxiliar HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: aaeadde258a2855343a5bfb1e5ee76000e04f6bd
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74593866"
---
# <a name="creating-custom-html-helpers-vb"></a><span data-ttu-id="45b1d-104">Criação de auxiliares de HTML personalizados (VB)</span><span class="sxs-lookup"><span data-stu-id="45b1d-104">Creating Custom HTML Helpers (VB)</span></span>

<span data-ttu-id="45b1d-105">pela [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="45b1d-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="45b1d-106">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="45b1d-106">Download PDF</span></span>](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> <span data-ttu-id="45b1d-107">O objetivo deste tutorial é demonstrar como você pode criar auxiliares HTML personalizados que podem ser usados em suas exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="45b1d-107">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="45b1d-108">Aproveitando os auxiliares HTML, você pode reduzir a quantidade de digitação entediante de marcas HTML que você deve executar para criar uma página HTML padrão.</span><span class="sxs-lookup"><span data-stu-id="45b1d-108">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="45b1d-109">O objetivo deste tutorial é demonstrar como você pode criar auxiliares HTML personalizados que podem ser usados em suas exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="45b1d-109">The goal of this tutorial is to demonstrate how you can create custom HTML Helpers that you can use within your MVC views.</span></span> <span data-ttu-id="45b1d-110">Aproveitando os auxiliares HTML, você pode reduzir a quantidade de digitação entediante de marcas HTML que você deve executar para criar uma página HTML padrão.</span><span class="sxs-lookup"><span data-stu-id="45b1d-110">By taking advantage of HTML Helpers, you can reduce the amount of tedious typing of HTML tags that you must perform to create a standard HTML page.</span></span>

<span data-ttu-id="45b1d-111">Na primeira parte deste tutorial, descrevo alguns dos auxiliares HTML existentes incluídos na estrutura MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="45b1d-111">In the first part of this tutorial, I describe some of the existing HTML Helpers included with the ASP.NET MVC framework.</span></span> <span data-ttu-id="45b1d-112">Em seguida, descrevo dois métodos de criação de auxiliares HTML personalizados: Eu explico como criar auxiliares HTML personalizados criando um método compartilhado e criando um método de extensão.</span><span class="sxs-lookup"><span data-stu-id="45b1d-112">Next, I describe two methods of creating custom HTML Helpers: I explain how to create custom HTML Helpers by creating a shared method and by creating an extension method.</span></span>

## <a name="understanding-html-helpers"></a><span data-ttu-id="45b1d-113">Noções básicas sobre auxiliares HTML</span><span class="sxs-lookup"><span data-stu-id="45b1d-113">Understanding HTML Helpers</span></span>

<span data-ttu-id="45b1d-114">Um auxiliar HTML é apenas um método que retorna uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="45b1d-114">An HTML Helper is just a method that returns a string.</span></span> <span data-ttu-id="45b1d-115">A cadeia de caracteres pode representar qualquer tipo de conteúdo desejado.</span><span class="sxs-lookup"><span data-stu-id="45b1d-115">The string can represent any type of content that you want.</span></span> <span data-ttu-id="45b1d-116">Por exemplo, você pode usar os auxiliares HTML para renderizar marcas HTML padrão, como HTML `<input>` e marcas de `<img>`.</span><span class="sxs-lookup"><span data-stu-id="45b1d-116">For example, you can use HTML Helpers to render standard HTML tags like HTML `<input>` and `<img>` tags.</span></span> <span data-ttu-id="45b1d-117">Você também pode usar os auxiliares HTML para renderizar um conteúdo mais complexo, como uma faixa de guias ou uma tabela HTML de dados de Database.</span><span class="sxs-lookup"><span data-stu-id="45b1d-117">You also can use HTML Helpers to render more complex content such as a tab strip or an HTML table of database data.</span></span>

<span data-ttu-id="45b1d-118">O ASP.NET MVC Framework inclui o seguinte conjunto de auxiliares HTML padrão (esta não é uma lista completa):</span><span class="sxs-lookup"><span data-stu-id="45b1d-118">The ASP.NET MVC framework includes the following set of standard HTML Helpers (this is not a complete list):</span></span>

- <span data-ttu-id="45b1d-119">HTML. ActionLink ()</span><span class="sxs-lookup"><span data-stu-id="45b1d-119">Html.ActionLink()</span></span>
- <span data-ttu-id="45b1d-120">HTML. BeginForm ()</span><span class="sxs-lookup"><span data-stu-id="45b1d-120">Html.BeginForm()</span></span>
- <span data-ttu-id="45b1d-121">HTML. CheckBox ()</span><span class="sxs-lookup"><span data-stu-id="45b1d-121">Html.CheckBox()</span></span>
- <span data-ttu-id="45b1d-122">HTML. DropDownList ()</span><span class="sxs-lookup"><span data-stu-id="45b1d-122">Html.DropDownList()</span></span>
- <span data-ttu-id="45b1d-123">HTML. EndForm ()</span><span class="sxs-lookup"><span data-stu-id="45b1d-123">Html.EndForm()</span></span>
- <span data-ttu-id="45b1d-124">HTML. Hidden ()</span><span class="sxs-lookup"><span data-stu-id="45b1d-124">Html.Hidden()</span></span>
- <span data-ttu-id="45b1d-125">HTML. ListBox ()</span><span class="sxs-lookup"><span data-stu-id="45b1d-125">Html.ListBox()</span></span>
- <span data-ttu-id="45b1d-126">HTML. Password ()</span><span class="sxs-lookup"><span data-stu-id="45b1d-126">Html.Password()</span></span>
- <span data-ttu-id="45b1d-127">HTML. RadioButton ()</span><span class="sxs-lookup"><span data-stu-id="45b1d-127">Html.RadioButton()</span></span>
- <span data-ttu-id="45b1d-128">HTML. TextArea ()</span><span class="sxs-lookup"><span data-stu-id="45b1d-128">Html.TextArea()</span></span>
- <span data-ttu-id="45b1d-129">HTML. TextBox ()</span><span class="sxs-lookup"><span data-stu-id="45b1d-129">Html.TextBox()</span></span>

<span data-ttu-id="45b1d-130">Por exemplo, considere o formulário na Listagem 1.</span><span class="sxs-lookup"><span data-stu-id="45b1d-130">For example, consider the form in Listing 1.</span></span> <span data-ttu-id="45b1d-131">Esse formulário é renderizado com a ajuda de dois dos auxiliares HTML padrão (consulte a Figura 1).</span><span class="sxs-lookup"><span data-stu-id="45b1d-131">This form is rendered with the help of two of the standard HTML Helpers (see Figure 1).</span></span> <span data-ttu-id="45b1d-132">Esse formulário usa os métodos auxiliares `Html.BeginForm()` e `Html.TextBox()`.</span><span class="sxs-lookup"><span data-stu-id="45b1d-132">This form uses the `Html.BeginForm()` and `Html.TextBox()` Helper methods.</span></span>

<span data-ttu-id="45b1d-133">[![página renderizada com auxiliares HTML](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="45b1d-133">[![Page rendered with HTML Helpers](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="45b1d-134">**Figura 01**: página renderizada com auxiliares HTML ([clique para exibir a imagem em tamanho normal](creating-custom-html-helpers-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="45b1d-134">**Figure 01**: Page rendered with HTML Helpers ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image3.png))</span></span>

<span data-ttu-id="45b1d-135">**Listagem 1 – `Views\Home\Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="45b1d-135">**Listing 1 – `Views\Home\Index.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

<span data-ttu-id="45b1d-136">O método auxiliar `Html.BeginForm()` é usado para criar as marcas de `<form>` HTML de abertura e fechamento.</span><span class="sxs-lookup"><span data-stu-id="45b1d-136">The `Html.BeginForm()` Helper method is used to create the opening and closing HTML `<form>` tags.</span></span> <span data-ttu-id="45b1d-137">Observe que o método `Html.BeginForm()` é chamado dentro de uma instrução using.</span><span class="sxs-lookup"><span data-stu-id="45b1d-137">Notice that the `Html.BeginForm()` method is called within a using statement.</span></span> <span data-ttu-id="45b1d-138">A instrução using garante que a marca de `<form>` seja fechada no final do bloco Using.</span><span class="sxs-lookup"><span data-stu-id="45b1d-138">The using statement ensures that the `<form>` tag gets closed at the end of the using block.</span></span>

<span data-ttu-id="45b1d-139">Se preferir, em vez de criar um bloco Using, você pode chamar o método auxiliar HTML. EndForm () para fechar a marca de `<form>`.</span><span class="sxs-lookup"><span data-stu-id="45b1d-139">If you prefer, instead of creating a using block, you can call the Html.EndForm() Helper method to close the `<form>` tag.</span></span> <span data-ttu-id="45b1d-140">Use qualquer abordagem para criar uma marca de `<form>` de abertura e fechamento que pareça mais intuitiva para você.</span><span class="sxs-lookup"><span data-stu-id="45b1d-140">Use whichever approach to creating an opening and closing `<form>` tag that seems most intuitive to you.</span></span>

<span data-ttu-id="45b1d-141">Os métodos auxiliares `Html.TextBox()` são usados na Listagem 1 para processar marcas HTML `<input>`.</span><span class="sxs-lookup"><span data-stu-id="45b1d-141">The `Html.TextBox()` Helper methods are used in Listing 1 to render HTML `<input>` tags.</span></span> <span data-ttu-id="45b1d-142">Se você selecionar Exibir origem em seu navegador, verá a fonte HTML na Listagem 2.</span><span class="sxs-lookup"><span data-stu-id="45b1d-142">If you select view source in your browser then you see the HTML source in Listing 2.</span></span> <span data-ttu-id="45b1d-143">Observe que a origem contém marcas HTML padrão.</span><span class="sxs-lookup"><span data-stu-id="45b1d-143">Notice that the source contains standard HTML tags.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="45b1d-144">Observe que o auxiliar `Html.TextBox()`-HTML é renderizado com `<%= %>` marcas em vez de `<% %>` marcas.</span><span class="sxs-lookup"><span data-stu-id="45b1d-144">notice that the `Html.TextBox()`-HTML Helper is rendered with `<%= %>` tags instead of `<% %>` tags.</span></span> <span data-ttu-id="45b1d-145">Se você não incluir o sinal de igual, nada será renderizado para o navegador.</span><span class="sxs-lookup"><span data-stu-id="45b1d-145">If you don't include the equal sign, then nothing gets rendered to the browser.</span></span>

<span data-ttu-id="45b1d-146">O ASP.NET MVC Framework contém um pequeno conjunto de auxiliares.</span><span class="sxs-lookup"><span data-stu-id="45b1d-146">The ASP.NET MVC framework contains a small set of helpers.</span></span> <span data-ttu-id="45b1d-147">Provavelmente, será necessário estender a estrutura MVC com auxiliares HTML personalizados.</span><span class="sxs-lookup"><span data-stu-id="45b1d-147">Most likely, you will need to extend the MVC framework with custom HTML Helpers.</span></span> <span data-ttu-id="45b1d-148">No restante deste tutorial, você aprenderá dois métodos de criação de auxiliares HTML personalizados.</span><span class="sxs-lookup"><span data-stu-id="45b1d-148">In the remainder of this tutorial, you learn two methods of creating custom HTML Helpers.</span></span>

<span data-ttu-id="45b1d-149">**Listagem 2 – `Index.aspx Source`**</span><span class="sxs-lookup"><span data-stu-id="45b1d-149">**Listing 2 – `Index.aspx Source`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a><span data-ttu-id="45b1d-150">Criando auxiliares HTML com métodos compartilhados</span><span class="sxs-lookup"><span data-stu-id="45b1d-150">Creating HTML Helpers with Shared Methods</span></span>

<span data-ttu-id="45b1d-151">A maneira mais fácil de criar um novo auxiliar HTML é criar um método compartilhado que retorne uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="45b1d-151">The easiest way to create a new HTML Helper is to create a shared method that returns a string.</span></span> <span data-ttu-id="45b1d-152">Imagine, por exemplo, que você decida criar um novo auxiliar HTML que renderiza uma marcação HTML `<label>`.</span><span class="sxs-lookup"><span data-stu-id="45b1d-152">Imagine, for example, that you decide to create a new HTML Helper that renders an HTML `<label>` tag.</span></span> <span data-ttu-id="45b1d-153">Você pode usar a classe na Listagem 2 para renderizar um `<label>`.</span><span class="sxs-lookup"><span data-stu-id="45b1d-153">You can use the class in Listing 2 to render a `<label>`.</span></span>

<span data-ttu-id="45b1d-154">**Listagem 2 – `Helpers\LabelHelper.vb`**</span><span class="sxs-lookup"><span data-stu-id="45b1d-154">**Listing 2 – `Helpers\LabelHelper.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

<span data-ttu-id="45b1d-155">Não há nada de especial sobre a classe na Listagem 2.</span><span class="sxs-lookup"><span data-stu-id="45b1d-155">There is nothing special about the class in Listing 2.</span></span> <span data-ttu-id="45b1d-156">O método `Label()` simplesmente retorna uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="45b1d-156">The `Label()` method simply returns a string.</span></span>

<span data-ttu-id="45b1d-157">A exibição de índice modificada na Listagem 3 usa o `LabelHelper` para processar marcas HTML `<label>`.</span><span class="sxs-lookup"><span data-stu-id="45b1d-157">The modified Index view in Listing 3 uses the `LabelHelper` to render HTML `<label>` tags.</span></span> <span data-ttu-id="45b1d-158">Observe que a exibição inclui uma diretiva `<%@ imports %>` que importa o namespace Application1. Helpers.</span><span class="sxs-lookup"><span data-stu-id="45b1d-158">Notice that the view includes an `<%@ imports %>` directive that imports the Application1.Helpers namespace.</span></span>

<span data-ttu-id="45b1d-159">**Listagem 2 – `Views\Home\Index2.aspx`**</span><span class="sxs-lookup"><span data-stu-id="45b1d-159">**Listing 2 – `Views\Home\Index2.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a><span data-ttu-id="45b1d-160">Criando auxiliares HTML com métodos de extensão</span><span class="sxs-lookup"><span data-stu-id="45b1d-160">Creating HTML Helpers with Extension Methods</span></span>

<span data-ttu-id="45b1d-161">Se você quiser criar auxiliares HTML que funcionam exatamente como os auxiliares HTML padrão incluídos no ASP.NET MVC Framework, você precisará criar métodos de extensão.</span><span class="sxs-lookup"><span data-stu-id="45b1d-161">If you want to create HTML Helpers that work just like the standard HTML Helpers included in the ASP.NET MVC framework then you need to create extension methods.</span></span> <span data-ttu-id="45b1d-162">Os métodos de extensão permitem adicionar novos métodos a uma classe existente.</span><span class="sxs-lookup"><span data-stu-id="45b1d-162">Extension methods enable you to add new methods to an existing class.</span></span> <span data-ttu-id="45b1d-163">Ao criar um método auxiliar HTML, você adiciona novos métodos à classe `HtmlHelper` representada pela propriedade HTML de uma exibição.</span><span class="sxs-lookup"><span data-stu-id="45b1d-163">When creating an HTML Helper method, you add new methods to the `HtmlHelper` class represented by a view's Html property.</span></span>

<span data-ttu-id="45b1d-164">O módulo Visual Basic na Listagem 3 adiciona um método de extensão chamado `Label()` à classe `HtmlHelper`.</span><span class="sxs-lookup"><span data-stu-id="45b1d-164">The Visual Basic module in Listing 3 adds an extension method named `Label()` to the `HtmlHelper` class.</span></span> <span data-ttu-id="45b1d-165">Há algumas coisas que você deve observar sobre esse módulo.</span><span class="sxs-lookup"><span data-stu-id="45b1d-165">There are a couple of things that you should notice about this module.</span></span> <span data-ttu-id="45b1d-166">Primeiro, observe que o módulo está decorado com o atributo `<Extension()>`.</span><span class="sxs-lookup"><span data-stu-id="45b1d-166">First, notice that the module is decorated with the `<Extension()>` attribute.</span></span> <span data-ttu-id="45b1d-167">Para usar esse atributo, você deve importar o namespace `System.Runtime.CompilerServices`</span><span class="sxs-lookup"><span data-stu-id="45b1d-167">In order to use this attribute, you must import the `System.Runtime.CompilerServices` namespace</span></span>

<span data-ttu-id="45b1d-168">Em segundo lugar, observe que o primeiro parâmetro do método `Label()` representa a classe `HtmlHelper`.</span><span class="sxs-lookup"><span data-stu-id="45b1d-168">Second, notice that the first parameter of the `Label()` method represents the `HtmlHelper` class.</span></span> <span data-ttu-id="45b1d-169">O primeiro parâmetro de um método de extensão indica a classe que o método de extensão estende.</span><span class="sxs-lookup"><span data-stu-id="45b1d-169">The first parameter of an extension method indicates the class that the extension method extends.</span></span>

<span data-ttu-id="45b1d-170">**Listagem 3 – `Helpers\LabelExtensions.vb`**</span><span class="sxs-lookup"><span data-stu-id="45b1d-170">**Listing 3 – `Helpers\LabelExtensions.vb`**</span></span>

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

<span data-ttu-id="45b1d-171">Depois de criar um método de extensão e criar seu aplicativo com êxito, o método de extensão aparecerá no Visual Studio IntelliSense como todos os outros métodos de uma classe (consulte a Figura 2).</span><span class="sxs-lookup"><span data-stu-id="45b1d-171">After you create an extension method, and build your application successfully, the extension method appears in Visual Studio Intellisense like all of the other methods of a class (see Figure 2).</span></span> <span data-ttu-id="45b1d-172">A única diferença é que os métodos de extensão aparecem com um símbolo especial ao lado deles (um ícone de seta para baixo).</span><span class="sxs-lookup"><span data-stu-id="45b1d-172">The only difference is that extension methods appear with a special symbol next to them (an icon of a downward arrow).</span></span>

<span data-ttu-id="45b1d-173">[![usando o método de extensão HTML. Label ()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="45b1d-173">[![Using the Html.Label() extension method](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)</span></span>

<span data-ttu-id="45b1d-174">**Figura 02**: usando o método de extensão HTML. Label () ([clique para exibir a imagem em tamanho normal](creating-custom-html-helpers-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="45b1d-174">**Figure 02**: Using the Html.Label() extension method  ([Click to view full-size image](creating-custom-html-helpers-vb/_static/image6.png))</span></span>

<span data-ttu-id="45b1d-175">A exibição de índice modificada na Listagem 4 usa o método de extensão HTML. Label () para renderizar todas as suas &lt;rótulo&gt; marcas.</span><span class="sxs-lookup"><span data-stu-id="45b1d-175">The modified Index view in Listing 4 uses the Html.Label() extension method to render all of its &lt;label&gt; tags.</span></span>

<span data-ttu-id="45b1d-176">**Listagem 4 – `Views\Home\Index3.aspx`**</span><span class="sxs-lookup"><span data-stu-id="45b1d-176">**Listing 4 – `Views\Home\Index3.aspx`**</span></span>

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a><span data-ttu-id="45b1d-177">Resumo</span><span class="sxs-lookup"><span data-stu-id="45b1d-177">Summary</span></span>

<span data-ttu-id="45b1d-178">Neste tutorial, você aprendeu dois métodos de criação de auxiliares HTML personalizados.</span><span class="sxs-lookup"><span data-stu-id="45b1d-178">In this tutorial, you learned two methods of creating custom HTML Helpers.</span></span> <span data-ttu-id="45b1d-179">Primeiro, você aprendeu a criar um auxiliar HTML personalizado `Label()` criando um método compartilhado que retorna uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="45b1d-179">First, you learned how to create a custom `Label()` HTML Helper by creating a shared method that returns a string.</span></span> <span data-ttu-id="45b1d-180">Em seguida, você aprendeu a criar um método auxiliar HTML `Label()` personalizado criando um método de extensão na classe `HtmlHelper`.</span><span class="sxs-lookup"><span data-stu-id="45b1d-180">Next, you learned how to create a custom `Label()` HTML Helper method by creating an extension method on the `HtmlHelper` class.</span></span>

<span data-ttu-id="45b1d-181">Neste tutorial, eu me deparava com a criação de um método auxiliar HTML extremamente simples.</span><span class="sxs-lookup"><span data-stu-id="45b1d-181">In this tutorial, I focused on building an extremely simple HTML Helper method.</span></span> <span data-ttu-id="45b1d-182">Perceba que um auxiliar HTML pode ser tão complicado quanto você desejar.</span><span class="sxs-lookup"><span data-stu-id="45b1d-182">Realize that an HTML Helper can be as complicated as you want.</span></span> <span data-ttu-id="45b1d-183">Você pode criar auxiliares HTML que processam conteúdo rico, como modos de exibição de árvore, menus ou tabelas de dados de banco.</span><span class="sxs-lookup"><span data-stu-id="45b1d-183">You can build HTML Helpers that render rich content such as tree views, menus, or tables of database data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="45b1d-184">[Anterior](asp-net-mvc-views-overview-vb.md)
> [Próximo](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span><span class="sxs-lookup"><span data-stu-id="45b1d-184">[Previous](asp-net-mvc-views-overview-vb.md)
[Next](using-the-tagbuilder-class-to-build-html-helpers-vb.md)</span></span>
