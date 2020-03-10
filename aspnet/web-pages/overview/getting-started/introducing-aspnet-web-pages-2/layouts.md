---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Introdução ao Páginas da Web do ASP.NET-Criando um layout consistente | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como usar layouts para criar uma aparência consistente para as páginas em um site que usa Páginas da Web do ASP.NET. Ele pressupõe que você concluiu o...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 678eb7089e95e3d221d6b2d82034a62aefa75757
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526687"
---
# <a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="77746-104">Introdução ao Páginas da Web do ASP.NET-Criando um layout consistente</span><span class="sxs-lookup"><span data-stu-id="77746-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>

<span data-ttu-id="77746-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="77746-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="77746-106">Este tutorial mostra como usar *layouts* para criar uma aparência consistente para as páginas em um site que usa páginas da Web do ASP.net.</span><span class="sxs-lookup"><span data-stu-id="77746-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="77746-107">Ele pressupõe que você concluiu a série por meio da [exclusão de dados de banco de dado no páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="77746-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="77746-108">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="77746-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="77746-109">O que é uma página de layout.</span><span class="sxs-lookup"><span data-stu-id="77746-109">What a layout page is.</span></span>
> - <span data-ttu-id="77746-110">Como combinar páginas de layout com conteúdo dinâmico.</span><span class="sxs-lookup"><span data-stu-id="77746-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="77746-111">Como passar valores para uma página de layout.</span><span class="sxs-lookup"><span data-stu-id="77746-111">How to pass values to a layout page.</span></span>

## <a name="about-layouts"></a><span data-ttu-id="77746-112">Sobre layouts</span><span class="sxs-lookup"><span data-stu-id="77746-112">About Layouts</span></span>

<span data-ttu-id="77746-113">As páginas que você criou até agora foram todas concluídas, páginas autônomas.</span><span class="sxs-lookup"><span data-stu-id="77746-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="77746-114">Todos eles pertencem ao mesmo site, mas não têm nenhum elemento comum ou uma aparência padrão.</span><span class="sxs-lookup"><span data-stu-id="77746-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="77746-115">A maioria dos sites tem uma aparência e um layout consistentes.</span><span class="sxs-lookup"><span data-stu-id="77746-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="77746-116">Por exemplo, se você for para o site do [Microsoft.com/Web](https://www.microsoft.com/web/) e observar, verá que as páginas são todas seguidas em um layout geral e em um tema Visual:</span><span class="sxs-lookup"><span data-stu-id="77746-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Página do site do Microsoft.com/web mostrando o layout do cabeçalho, da área de navegação, da área de conteúdo e do rodapé](layouts/_static/image1.png)

<span data-ttu-id="77746-118">Uma maneira *ineficiente* de criar esse layout seria definir um cabeçalho, uma barra de navegação e um rodapé separadamente em cada uma de suas páginas.</span><span class="sxs-lookup"><span data-stu-id="77746-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="77746-119">Você estaria duplicando a mesma marcação a cada vez.</span><span class="sxs-lookup"><span data-stu-id="77746-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="77746-120">Se você quisesse alterar algo (por exemplo, atualizar o rodapé), precisaria alterar cada página separadamente.</span><span class="sxs-lookup"><span data-stu-id="77746-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="77746-121">É aí que entram as *páginas de layout* .</span><span class="sxs-lookup"><span data-stu-id="77746-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="77746-122">No Páginas da Web do ASP.NET, você pode definir uma página de layout que fornece um contêiner geral para páginas em seu site.</span><span class="sxs-lookup"><span data-stu-id="77746-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="77746-123">Por exemplo, a página de layout pode conter o cabeçalho, a área de navegação e o rodapé.</span><span class="sxs-lookup"><span data-stu-id="77746-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="77746-124">A página de layout inclui um espaço reservado onde o conteúdo principal vai.</span><span class="sxs-lookup"><span data-stu-id="77746-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="77746-125">Em seguida, você pode definir páginas de conteúdo individuais que contêm a marcação e o código somente para essa página.</span><span class="sxs-lookup"><span data-stu-id="77746-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="77746-126">As páginas de conteúdo não precisam ser páginas HTML completas; Eles nem precisam ter um elemento `<body>`.</span><span class="sxs-lookup"><span data-stu-id="77746-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="77746-127">Eles também têm uma linha de código que diz ao ASP.NET em qual página de layout você deseja exibir o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="77746-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="77746-128">Aqui está uma imagem que mostra aproximadamente como essa relação funciona:</span><span class="sxs-lookup"><span data-stu-id="77746-128">Here's a picture that shows roughly how this relationship works:</span></span>

![Diagrama conceitual que mostra duas páginas de conteúdo e uma página de layout na qual elas se ajustam](layouts/_static/image2.png)

<span data-ttu-id="77746-130">Essa interação é fácil de entender quando você a vê em ação.</span><span class="sxs-lookup"><span data-stu-id="77746-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="77746-131">Neste tutorial, você alterará suas páginas de filmes para usar um layout.</span><span class="sxs-lookup"><span data-stu-id="77746-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="77746-132">Adicionando uma página de layout</span><span class="sxs-lookup"><span data-stu-id="77746-132">Adding a Layout Page</span></span>

<span data-ttu-id="77746-133">Você começará criando uma página de layout que define um layout de página típico com um cabeçalho, um rodapé e uma área para o conteúdo principal.</span><span class="sxs-lookup"><span data-stu-id="77746-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="77746-134">No site do WebPagesMovies, adicione uma página CSHTML denominada *\_layout. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="77746-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="77746-135">O caractere de sublinhado à esquerda (`_`) é significativo.</span><span class="sxs-lookup"><span data-stu-id="77746-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="77746-136">Se o nome de uma página começar com um sublinhado, ASP.NET não enviará diretamente essa página para o navegador.</span><span class="sxs-lookup"><span data-stu-id="77746-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="77746-137">Essa Convenção permite definir páginas que são necessárias para seu site, mas que os usuários não devem ser capazes de solicitar diretamente.</span><span class="sxs-lookup"><span data-stu-id="77746-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="77746-138">Substitua o conteúdo da página pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="77746-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="77746-139">Como você pode ver, essa marcação é apenas HTML que usa elementos `<div>` para definir três seções na página mais um elemento mais `<div>` para manter as três seções.</span><span class="sxs-lookup"><span data-stu-id="77746-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="77746-140">O rodapé contém um pouco de código do Razor: `@DateTime.Now.Year`, que renderizará o ano atual nesse local na página.</span><span class="sxs-lookup"><span data-stu-id="77746-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="77746-141">Observe que há um link para uma folha de estilos chamada *Movies. css*.</span><span class="sxs-lookup"><span data-stu-id="77746-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="77746-142">A folha de estilos é onde os detalhes do layout físico dos elementos serão definidos.</span><span class="sxs-lookup"><span data-stu-id="77746-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="77746-143">Você criará isso daqui a pouco.</span><span class="sxs-lookup"><span data-stu-id="77746-143">You'll create that in a moment.</span></span>

<span data-ttu-id="77746-144">O único recurso incomum neste *\_página layout. cshtml* é a linha de `@Render.Body()`.</span><span class="sxs-lookup"><span data-stu-id="77746-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="77746-145">Esse é o espaço reservado onde o conteúdo será usado quando esse layout for mesclado com outra página.</span><span class="sxs-lookup"><span data-stu-id="77746-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="77746-146">Adicionando um arquivo. css</span><span class="sxs-lookup"><span data-stu-id="77746-146">Adding a .css File</span></span>

<span data-ttu-id="77746-147">A maneira preferida de definir a organização real (ou seja, aparência) dos elementos na página é usar as regras de CSS (folha de estilos em cascata).</span><span class="sxs-lookup"><span data-stu-id="77746-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="77746-148">Portanto, você criará um arquivo *. css* que tem as regras para o novo layout.</span><span class="sxs-lookup"><span data-stu-id="77746-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="77746-149">No WebMatrix, selecione a raiz do seu site.</span><span class="sxs-lookup"><span data-stu-id="77746-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="77746-150">Na guia **arquivos** da faixa de faixas, clique na seta sob o botão **novo** e clique em **nova pasta**.</span><span class="sxs-lookup"><span data-stu-id="77746-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![A opção ' nova pasta ' em novo na faixa de opções.](layouts/_static/image3.png)

<span data-ttu-id="77746-152">Nomeie os novos *estilos*de pasta.</span><span class="sxs-lookup"><span data-stu-id="77746-152">Name the new folder *Styles*.</span></span>

![Nomeando a nova pasta ' Styles '](layouts/_static/image4.png)

<span data-ttu-id="77746-154">Dentro da nova pasta *estilos* , crie um arquivo chamado *Movies. css*.</span><span class="sxs-lookup"><span data-stu-id="77746-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Criando um novo arquivo Movies. css](layouts/_static/image5.png)

<span data-ttu-id="77746-156">Substitua o conteúdo do novo arquivo *. css* pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="77746-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="77746-157">Não vamos dizer muito sobre essas regras de CSS, exceto para observar duas coisas.</span><span class="sxs-lookup"><span data-stu-id="77746-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="77746-158">Uma delas é que, além de definir fontes e tamanhos, as regras usam o posicionamento absoluto para estabelecer o local do cabeçalho, do rodapé e da área de conteúdo principal.</span><span class="sxs-lookup"><span data-stu-id="77746-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="77746-159">Se você for novo no posicionamento em CSS, poderá ler o tutorial de [posicionamento de CSS](http://www.w3schools.com/css/css_positioning.asp) no site do w3schools.</span><span class="sxs-lookup"><span data-stu-id="77746-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="77746-160">Outra coisa a ser observada é que, na parte inferior, copiamos as regras de estilo que foram originalmente definidas individualmente no arquivo *Movies. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="77746-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="77746-161">Essas regras foram usadas na [introdução à exibição de dados usando páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial para fazer com que o auxiliar de `WebGrid` processe a marcação que adicionou as faixas à tabela.</span><span class="sxs-lookup"><span data-stu-id="77746-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="77746-162">(Se você for usar um arquivo *. css* para definições de estilo, você também poderá colocar as regras de estilo para todo o site nele.)</span><span class="sxs-lookup"><span data-stu-id="77746-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="77746-163">Atualizando o arquivo de filmes para usar o layout</span><span class="sxs-lookup"><span data-stu-id="77746-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="77746-164">Agora você pode atualizar os arquivos existentes no seu site para usar o novo layout.</span><span class="sxs-lookup"><span data-stu-id="77746-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="77746-165">Abra o arquivo *Movies. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="77746-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="77746-166">Na parte superior, como a primeira linha de código, adicione o seguinte:</span><span class="sxs-lookup"><span data-stu-id="77746-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="77746-167">A página agora começa desta forma:</span><span class="sxs-lookup"><span data-stu-id="77746-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="77746-168">Essa linha de código informa ao ASP.NET que, quando a página de *filmes* é executada, ela deve ser mesclada com o arquivo *layout. cshtml de\_* .</span><span class="sxs-lookup"><span data-stu-id="77746-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="77746-169">Como o arquivo *Movies. cshtml* agora usa uma página de layout, você pode remover a marcação da página *Movies. cshtml* que é manipulada pelo arquivo *layout. cshtml de\_* .</span><span class="sxs-lookup"><span data-stu-id="77746-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="77746-170">Retire as marcas `<!DOCTYPE>`, `<html>`e `<body>` de abertura e fechamento.</span><span class="sxs-lookup"><span data-stu-id="77746-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="77746-171">Retire todo o elemento `<head>` e seu conteúdo, que inclui as regras de estilo para a grade, já que você tem essas regras em um arquivo *. css* .</span><span class="sxs-lookup"><span data-stu-id="77746-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="77746-172">Enquanto estiver, altere o elemento `<h1>` existente para um elemento `<h2>`; Você já tem um elemento `<h1>` na página de layout.</span><span class="sxs-lookup"><span data-stu-id="77746-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="77746-173">Altere o texto do `<h2>` para "listar filmes".</span><span class="sxs-lookup"><span data-stu-id="77746-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="77746-174">Normalmente, você não precisaria fazer esses tipos de alterações em uma página de conteúdo.</span><span class="sxs-lookup"><span data-stu-id="77746-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="77746-175">Ao iniciar o site com uma página de layout, você cria páginas de conteúdo sem todos esses elementos para começar.</span><span class="sxs-lookup"><span data-stu-id="77746-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="77746-176">Nesse caso, no entanto, você está convertendo uma página autônoma para uma que usa um layout, portanto, há um pouco de limpeza.</span><span class="sxs-lookup"><span data-stu-id="77746-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="77746-177">Quando tiver terminado, a página *Movies. cshtml* será parecida com a seguinte:</span><span class="sxs-lookup"><span data-stu-id="77746-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="77746-178">Testando o layout</span><span class="sxs-lookup"><span data-stu-id="77746-178">Testing the Layout</span></span>

<span data-ttu-id="77746-179">Agora você pode ver qual é a aparência do layout.</span><span class="sxs-lookup"><span data-stu-id="77746-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="77746-180">No WebMatrix, clique com o botão direito do mouse na página *Movies. cshtml* e selecione **Iniciar no navegador**.</span><span class="sxs-lookup"><span data-stu-id="77746-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="77746-181">Quando o navegador exibe a página, ele é semelhante a esta página:</span><span class="sxs-lookup"><span data-stu-id="77746-181">When the browser displays the page, it looks like this page:</span></span>

![Página de filmes renderizada usando um layout](layouts/_static/image6.png)

<span data-ttu-id="77746-183">ASP.NET mesclou o conteúdo da página Movies. cshtml na página *\_layout. cshtml* , onde o método `RenderBody` é.</span><span class="sxs-lookup"><span data-stu-id="77746-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="77746-184">E, claro, a página *\_layout. cshtml* faz referência a um arquivo *. css* que define a aparência da página.</span><span class="sxs-lookup"><span data-stu-id="77746-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="77746-185">Atualizando a página addmovie para usar o layout</span><span class="sxs-lookup"><span data-stu-id="77746-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="77746-186">O verdadeiro benefício dos layouts é que você pode usá-los para todas as páginas do seu site.</span><span class="sxs-lookup"><span data-stu-id="77746-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="77746-187">Abra a página *addmovie. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="77746-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="77746-188">Você pode se lembrar de que a página *addmovie. cshtml* tinha originalmente algumas regras de CSS nela para definir a aparência das mensagens de erro de validação.</span><span class="sxs-lookup"><span data-stu-id="77746-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="77746-189">Como você tem um arquivo *. css* para seu site agora, você pode mover essas regras para o arquivo *. css* .</span><span class="sxs-lookup"><span data-stu-id="77746-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="77746-190">Remova-os do arquivo *addmovie. cshtml* e adicione-os à parte inferior do arquivo *Movies. css* .</span><span class="sxs-lookup"><span data-stu-id="77746-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="77746-191">Você está movendo as seguintes regras:</span><span class="sxs-lookup"><span data-stu-id="77746-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="77746-192">Agora faça os mesmos tipos de alterações em *addmovie. cshtml* que você fez para *Movies. cshtml* — adicione `Layout="~/_Layout.cshtml;` e remova a marcação HTML que agora é estranha.</span><span class="sxs-lookup"><span data-stu-id="77746-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="77746-193">Altere o elemento `<h1>` para `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="77746-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="77746-194">Quando terminar, a página se parecerá com este exemplo:</span><span class="sxs-lookup"><span data-stu-id="77746-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="77746-195">Execute a página.</span><span class="sxs-lookup"><span data-stu-id="77746-195">Run the page.</span></span> <span data-ttu-id="77746-196">Agora, ele é semelhante a esta ilustração:</span><span class="sxs-lookup"><span data-stu-id="77746-196">Now it looks like this illustration:</span></span>

![Página ' Adicionar filmes ' processada usando um layout](layouts/_static/image7.png)

<span data-ttu-id="77746-198">Você deseja fazer alterações semelhantes às páginas no site — *EditMovie. cshtml* e *DeleteMovie. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="77746-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="77746-199">No entanto, antes de fazer isso, você pode fazer outra alteração no layout que o torna um pouco mais flexível.</span><span class="sxs-lookup"><span data-stu-id="77746-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="77746-200">Passando informações de título para a página de layout</span><span class="sxs-lookup"><span data-stu-id="77746-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="77746-201">A página *\_layout. cshtml* que você criou tem um elemento `<title>` definido como "meu site de filmes".</span><span class="sxs-lookup"><span data-stu-id="77746-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="77746-202">A maioria dos navegadores exibe o conteúdo deste elemento como o texto em uma guia:</span><span class="sxs-lookup"><span data-stu-id="77746-202">Most browsers display the content of this element as the text on a tab:</span></span>

![O título da &lt;da página&gt; elemento exibido em uma guia do navegador](layouts/_static/image8.png)

<span data-ttu-id="77746-204">Essa informação de título é genérica.</span><span class="sxs-lookup"><span data-stu-id="77746-204">This title information is generic.</span></span> <span data-ttu-id="77746-205">Suponha que você deseja que o texto do título seja mais específico para a página atual.</span><span class="sxs-lookup"><span data-stu-id="77746-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="77746-206">(O texto do título também é usado pelos mecanismos de pesquisa para determinar a sua página.) Você pode passar informações de uma página de conteúdo como *Movies. cshtml* ou *addmovie. cshtml* para a página de layout e, em seguida, usar essas informações para personalizar o que a página de layout renderiza.</span><span class="sxs-lookup"><span data-stu-id="77746-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="77746-207">Abra a página *Movies. cshtml* novamente.</span><span class="sxs-lookup"><span data-stu-id="77746-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="77746-208">No código na parte superior, adicione a seguinte linha:</span><span class="sxs-lookup"><span data-stu-id="77746-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="77746-209">O objeto `Page` está disponível em todas as páginas *. cshtml* e é para essa finalidade, ou seja, para compartilhar informações entre uma página e seu layout.</span><span class="sxs-lookup"><span data-stu-id="77746-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="77746-210">Abra a página *\_layout. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="77746-210">Open the *\_Layout.cshtml* page.</span></span> <span data-ttu-id="77746-211">Altere o elemento `<title>` de forma que ele se pareça com esta marcação:</span><span class="sxs-lookup"><span data-stu-id="77746-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="77746-212">Esse código renderiza o que estiver na propriedade `Page.Title` diretamente nesse local na página.</span><span class="sxs-lookup"><span data-stu-id="77746-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="77746-213">Execute a página *Movies. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="77746-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="77746-214">Desta vez, a guia Navegador mostra o que você passou como o valor de `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="77746-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![Uma guia do navegador mostrando o título criado dinamicamente](layouts/_static/image9.png)

<span data-ttu-id="77746-216">Se desejar, exiba a origem da página no navegador.</span><span class="sxs-lookup"><span data-stu-id="77746-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="77746-217">Você pode ver que o elemento `<title>` é renderizado como `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="77746-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="77746-218">**O objeto Page**</span><span class="sxs-lookup"><span data-stu-id="77746-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="77746-219">Um recurso útil do `Page` é que ele é um objeto dinâmico — a propriedade `Title` não é um nome fixo ou reservado.</span><span class="sxs-lookup"><span data-stu-id="77746-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="77746-220">Você pode usar *qualquer* nome para um valor do objeto `Page`.</span><span class="sxs-lookup"><span data-stu-id="77746-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="77746-221">Por exemplo, você poderia facilmente passar o título usando uma propriedade chamada `Page.CurrentName` ou `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="77746-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="77746-222">A única restrição é que o nome precisa seguir as regras normais para quais propriedades podem ser nomeadas.</span><span class="sxs-lookup"><span data-stu-id="77746-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="77746-223">(Por exemplo, o nome não pode conter um espaço).</span><span class="sxs-lookup"><span data-stu-id="77746-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="77746-224">Você pode passar qualquer número de valores usando o objeto `Page`.</span><span class="sxs-lookup"><span data-stu-id="77746-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="77746-225">Se você quisesse passar informações de filme para a página de layout, poderá passar valores usando algo como `Page.MovieTitle` e `Page.Genre` e `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="77746-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="77746-226">(Ou quaisquer outros nomes que você inventou para armazenar as informações.) O único requisito, que provavelmente é óbvio, é que você precisa usar os mesmos nomes na página de conteúdo e na página de layout.</span><span class="sxs-lookup"><span data-stu-id="77746-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="77746-227">As informações que você passa usando o objeto `Page` não estão limitadas a apenas texto a ser exibido na página de layout.</span><span class="sxs-lookup"><span data-stu-id="77746-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="77746-228">Você pode passar um valor para a página de layout e, em seguida, o código na página de layout pode usar o valor para decidir se deve exibir uma seção da página, o arquivo *. css* a ser usado e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="77746-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="77746-229">Os valores que você passa no objeto `Page` são como quaisquer outros valores que você usa no código.</span><span class="sxs-lookup"><span data-stu-id="77746-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="77746-230">É apenas que os valores se originam na página de conteúdo e são passados para a página de layout.</span><span class="sxs-lookup"><span data-stu-id="77746-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>

<span data-ttu-id="77746-231">Abra a página *addmovie. cshtml* e adicione uma linha à parte superior do código que fornece um título para a página *addmovie. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="77746-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="77746-232">Execute a página *addmovie. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="77746-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="77746-233">Você verá o novo título lá:</span><span class="sxs-lookup"><span data-stu-id="77746-233">You see the new title there:</span></span>

![Uma guia do navegador mostrando o título ' Adicionar filmes ' criado dinamicamente](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="77746-235">Atualizando as páginas restantes para usar o layout</span><span class="sxs-lookup"><span data-stu-id="77746-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="77746-236">Agora você pode concluir as páginas restantes no seu site para que elas usem o novo layout.</span><span class="sxs-lookup"><span data-stu-id="77746-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="77746-237">Abra *EditMovie. cshtml* e *DeleteMovie. cshtml* por vez e faça as mesmas alterações em cada um.</span><span class="sxs-lookup"><span data-stu-id="77746-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="77746-238">Adicione a linha de código que vincula à página de layout:</span><span class="sxs-lookup"><span data-stu-id="77746-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="77746-239">Adicione uma linha para definir o título da página:</span><span class="sxs-lookup"><span data-stu-id="77746-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="77746-240">ou:</span><span class="sxs-lookup"><span data-stu-id="77746-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="77746-241">Remover toda a marcação HTML incorreta — basicamente, deixe apenas os bits que estão dentro do elemento `<body>` (mais o bloco de código na parte superior).</span><span class="sxs-lookup"><span data-stu-id="77746-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="77746-242">Altere o elemento `<h1>` para que seja um elemento `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="77746-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="77746-243">Quando você fez essas alterações, teste cada uma e verifique se ela está sendo exibida corretamente e se o título está correto.</span><span class="sxs-lookup"><span data-stu-id="77746-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="77746-244">Ideias de partes sobre páginas de layout</span><span class="sxs-lookup"><span data-stu-id="77746-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="77746-245">Neste tutorial, você criou uma página *\_layout. cshtml* e usou o método `RenderBody` para Mesclar conteúdo de outra página.</span><span class="sxs-lookup"><span data-stu-id="77746-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="77746-246">Esse é o padrão básico para usar layouts em páginas da Web.</span><span class="sxs-lookup"><span data-stu-id="77746-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="77746-247">As páginas de layout têm recursos adicionais que não abordamos aqui.</span><span class="sxs-lookup"><span data-stu-id="77746-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="77746-248">Por exemplo, você pode aninhar páginas de layout — uma página de layout pode, por sua vez, fazer referência a outra.</span><span class="sxs-lookup"><span data-stu-id="77746-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="77746-249">Layouts aninhados podem ser úteis se você estiver trabalhando com subseções de um site que exigem layouts diferentes.</span><span class="sxs-lookup"><span data-stu-id="77746-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="77746-250">Você também pode usar métodos adicionais (por exemplo, `RenderSection`) para configurar seções nomeadas na página layout.</span><span class="sxs-lookup"><span data-stu-id="77746-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="77746-251">A combinação de páginas de layout e arquivos *. css* é eficiente.</span><span class="sxs-lookup"><span data-stu-id="77746-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="77746-252">Como você verá na próxima série de tutoriais, no WebMatrix, você pode criar um site com base em um *modelo*, que fornece um site com funcionalidade predefinida.</span><span class="sxs-lookup"><span data-stu-id="77746-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="77746-253">Os modelos fazem uso bom das páginas de layout e do CSS para criar sites que têm uma aparência ótima e que têm recursos como menus.</span><span class="sxs-lookup"><span data-stu-id="77746-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="77746-254">Aqui está uma captura de tela da home page de um site com base em um modelo, mostrando recursos que usam páginas de layout e CSS:</span><span class="sxs-lookup"><span data-stu-id="77746-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Layout criado pelo modelo de site do WebMatrix mostrando o cabeçalho, área de navegação, área de conteúdo, seção opcional e links de logon](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="77746-256">Listagem completa da página de filme (atualizada para usar uma página de layout)</span><span class="sxs-lookup"><span data-stu-id="77746-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="77746-257">Lista completa de páginas para adicionar filme (atualizado para layout)</span><span class="sxs-lookup"><span data-stu-id="77746-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="77746-258">Concluir a listagem de página para excluir página de filme (atualizada para layout)</span><span class="sxs-lookup"><span data-stu-id="77746-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="77746-259">Lista completa de páginas para editar filme (atualizado para layout)</span><span class="sxs-lookup"><span data-stu-id="77746-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="77746-260">Chegando em seguida</span><span class="sxs-lookup"><span data-stu-id="77746-260">Coming Up Next</span></span>

<span data-ttu-id="77746-261">No próximo tutorial, você aprenderá a publicar seu site na Internet para que todos possam vê-lo.</span><span class="sxs-lookup"><span data-stu-id="77746-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="77746-262">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="77746-262">Additional Resources</span></span>

- <span data-ttu-id="77746-263">[Criando uma aparência consistente](https://go.microsoft.com/fwlink/?LinkID=202891) — um artigo que fornece mais detalhes sobre como trabalhar com layouts.</span><span class="sxs-lookup"><span data-stu-id="77746-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="77746-264">Ele também descreve como passar um valor para uma página de layout que mostra ou oculta parte do conteúdo.</span><span class="sxs-lookup"><span data-stu-id="77746-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="77746-265">[Páginas de layout aninhadas com Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind bloga um exemplo de como aninhar páginas de layout.</span><span class="sxs-lookup"><span data-stu-id="77746-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="77746-266">(Inclui um download das páginas.)</span><span class="sxs-lookup"><span data-stu-id="77746-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="77746-267">[Anterior](deleting-data.md)
> [Próximo](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="77746-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
