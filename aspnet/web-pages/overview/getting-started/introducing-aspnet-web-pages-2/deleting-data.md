---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Introdução ao Páginas da Web do ASP.NET-excluindo dados do banco de dados | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como excluir uma entrada de banco de dados individual. Ele pressupõe que você concluiu a série por meio da atualização de dados do banco de dados no ASP.NET Web PA...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: c8620fc1abc61d514bdc039c66f7a84e67e89abe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78629034"
---
# <a name="introducing-aspnet-web-pages---deleting-database-data"></a><span data-ttu-id="16ff3-104">Apresentando Páginas da Web do ASP.NET-excluindo dados do banco</span><span class="sxs-lookup"><span data-stu-id="16ff3-104">Introducing ASP.NET Web Pages - Deleting Database Data</span></span>

<span data-ttu-id="16ff3-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="16ff3-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="16ff3-106">Este tutorial mostra como excluir uma entrada de banco de dados individual.</span><span class="sxs-lookup"><span data-stu-id="16ff3-106">This tutorial shows you how to delete an individual database entry.</span></span> <span data-ttu-id="16ff3-107">Ele pressupõe que você concluiu a série por meio da [atualização de dados do banco de dados no páginas da Web do ASP.net](updating-data.md).</span><span class="sxs-lookup"><span data-stu-id="16ff3-107">It assumes you have completed the series through [Updating Database Data in ASP.NET Web Pages](updating-data.md).</span></span>
> 
> <span data-ttu-id="16ff3-108">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="16ff3-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="16ff3-109">Como selecionar um registro individual de uma lista de registros.</span><span class="sxs-lookup"><span data-stu-id="16ff3-109">How to select an individual record from a listing of records.</span></span>
> - <span data-ttu-id="16ff3-110">Como excluir um único registro de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="16ff3-110">How to delete a single record from a database.</span></span>
> - <span data-ttu-id="16ff3-111">Como verificar se um botão específico foi clicado em um formulário.</span><span class="sxs-lookup"><span data-stu-id="16ff3-111">How to check that a specific button was clicked in a form.</span></span>
>   
> 
> <span data-ttu-id="16ff3-112">Recursos/tecnologias abordados:</span><span class="sxs-lookup"><span data-stu-id="16ff3-112">Features/technologies discussed:</span></span>
> 
> - <span data-ttu-id="16ff3-113">O auxiliar de `WebGrid`.</span><span class="sxs-lookup"><span data-stu-id="16ff3-113">The `WebGrid` helper.</span></span>
> - <span data-ttu-id="16ff3-114">O comando SQL `Delete`.</span><span class="sxs-lookup"><span data-stu-id="16ff3-114">The SQL `Delete` command.</span></span>
> - <span data-ttu-id="16ff3-115">O método `Database.Execute` para executar um comando SQL `Delete`.</span><span class="sxs-lookup"><span data-stu-id="16ff3-115">The `Database.Execute` method to run a SQL `Delete` command.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="16ff3-116">O que você vai construir</span><span class="sxs-lookup"><span data-stu-id="16ff3-116">What You'll Build</span></span>

<span data-ttu-id="16ff3-117">No tutorial anterior, você aprendeu a atualizar um registro de banco de dados existente.</span><span class="sxs-lookup"><span data-stu-id="16ff3-117">In the previous tutorial, you learned how to update an existing database record.</span></span> <span data-ttu-id="16ff3-118">Este tutorial é semelhante, exceto que, em vez de atualizar o registro, você o excluirá.</span><span class="sxs-lookup"><span data-stu-id="16ff3-118">This tutorial is similar, except that instead of updating the record, you'll delete it.</span></span> <span data-ttu-id="16ff3-119">Os processos são muito semelhantes, exceto que a exclusão é mais simples, portanto, este tutorial será curto.</span><span class="sxs-lookup"><span data-stu-id="16ff3-119">The processes are much the same, except that deleting is simpler, so this tutorial will be short.</span></span>

<span data-ttu-id="16ff3-120">Na página *filmes* , você atualizará o `WebGrid` auxiliar para que ele exiba um link de **exclusão** ao lado de cada filme para acompanhar o link de **edição** adicionado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="16ff3-120">In the *Movies* page, you'll update the `WebGrid` helper so that it displays a **Delete** link next to each movie to accompany the **Edit** link you added earlier.</span></span>

![Página de filmes mostrando um link de exclusão para cada filme](deleting-data/_static/image1.png)

<span data-ttu-id="16ff3-122">Assim como acontece com a edição, quando você clica no link **excluir** , ele leva para uma página diferente, onde as informações do filme já estão em um formato:</span><span class="sxs-lookup"><span data-stu-id="16ff3-122">As with editing, when you click the **Delete** link, it takes you to a different page, where the movie information is already in a form:</span></span>

![Excluir página de filme com um filme exibido](deleting-data/_static/image2.png)

<span data-ttu-id="16ff3-124">Em seguida, você pode clicar no botão para excluir o registro permanentemente.</span><span class="sxs-lookup"><span data-stu-id="16ff3-124">You can then click the button to delete the record permanently.</span></span>

## <a name="adding-a-delete-link-to-the-movie-listing"></a><span data-ttu-id="16ff3-125">Adicionando um link de exclusão à listagem de filmes</span><span class="sxs-lookup"><span data-stu-id="16ff3-125">Adding a Delete Link to the Movie Listing</span></span>

<span data-ttu-id="16ff3-126">Você começará adicionando um link de **exclusão** ao auxiliar de `WebGrid`.</span><span class="sxs-lookup"><span data-stu-id="16ff3-126">You'll start by adding a **Delete** link to the `WebGrid` helper.</span></span> <span data-ttu-id="16ff3-127">Esse link é semelhante ao link de **edição** que você adicionou em um tutorial anterior.</span><span class="sxs-lookup"><span data-stu-id="16ff3-127">This link is similar to the **Edit** link you added in a previous tutorial.</span></span>

<span data-ttu-id="16ff3-128">Abra o arquivo *Movies. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="16ff3-128">Open the *Movies.cshtml* file.</span></span>

<span data-ttu-id="16ff3-129">Altere a marcação de `WebGrid` no corpo da página adicionando uma coluna.</span><span class="sxs-lookup"><span data-stu-id="16ff3-129">Change the `WebGrid` markup in the body of the page by adding a column.</span></span> <span data-ttu-id="16ff3-130">Aqui está a marcação modificada:</span><span class="sxs-lookup"><span data-stu-id="16ff3-130">Here's the modified markup:</span></span>

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

<span data-ttu-id="16ff3-131">A nova coluna é esta:</span><span class="sxs-lookup"><span data-stu-id="16ff3-131">The new column is this one:</span></span>

[!code-html[Main](deleting-data/samples/sample2.html)]

<span data-ttu-id="16ff3-132">A maneira como a grade é configurada, a coluna de **edição** fica mais à esquerda na grade e a coluna **excluir** é a mais à direita.</span><span class="sxs-lookup"><span data-stu-id="16ff3-132">The way the grid is configured, the **Edit** column is leftmost in the grid and the **Delete** column is rightmost.</span></span> <span data-ttu-id="16ff3-133">(Há uma vírgula após a `Year` coluna agora, caso você não tenha notado isso.) Não há nada de especial sobre onde essas colunas de link vão, e você poderia colocá-las facilmente ao lado umas das outras.</span><span class="sxs-lookup"><span data-stu-id="16ff3-133">(There's a comma after the `Year` column now, in case you didn't notice that.) There's nothing special about where these link columns go, and you could as easily put them next to each other.</span></span> <span data-ttu-id="16ff3-134">Nesse caso, eles são separados para torná-los mais difíceis de serem misturados.</span><span class="sxs-lookup"><span data-stu-id="16ff3-134">In this case, they're separate to make them harder to get mixed up.</span></span>

![Página de filmes com links de edição e detalhes marcados para mostrar que eles não estão próximos uns dos outros](deleting-data/_static/image3.png)

<span data-ttu-id="16ff3-136">A nova coluna mostra um link (`<a>` elemento) cujo texto diz "excluir".</span><span class="sxs-lookup"><span data-stu-id="16ff3-136">The new column shows a link (`<a>` element) whose text says "Delete".</span></span> <span data-ttu-id="16ff3-137">O destino do link (seu atributo `href`) é o código que, em última instância, é resolvido para algo como essa URL, com o valor de `id` diferente para cada filme:</span><span class="sxs-lookup"><span data-stu-id="16ff3-137">The target of the link (its `href` attribute) is code that ultimately resolves to something like this URL, with the `id` value different for each movie:</span></span>

[!code-css[Main](deleting-data/samples/sample3.css)]

<span data-ttu-id="16ff3-138">Esse link invocará uma página chamada *DeleteMovie* e passará a ID do filme que você selecionou.</span><span class="sxs-lookup"><span data-stu-id="16ff3-138">This link will invoke a page named *DeleteMovie* and pass it the ID of the movie you've selected.</span></span>

<span data-ttu-id="16ff3-139">Este tutorial não entrará em detalhes sobre como esse link é construído, pois ele é quase idêntico ao link de **edição** do tutorial anterior ([atualizando dados de banco em páginas da Web do ASP.net](updating-data.md)).</span><span class="sxs-lookup"><span data-stu-id="16ff3-139">This tutorial won't go into detail about how this link is constructed, because it's almost identical to the **Edit** link from the previous tutorial ([Updating Database Data in ASP.NET Web Pages](updating-data.md)).</span></span>

## <a name="creating-the-delete-page"></a><span data-ttu-id="16ff3-140">Criando a página excluir</span><span class="sxs-lookup"><span data-stu-id="16ff3-140">Creating the Delete Page</span></span>

<span data-ttu-id="16ff3-141">Agora você pode criar a página que será o destino do link de **exclusão** na grade.</span><span class="sxs-lookup"><span data-stu-id="16ff3-141">Now you can create the page that will be the target for the **Delete** link in the grid.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="16ff3-142">**Importante** A técnica de primeiro selecionar um registro para excluir e, em seguida, usar uma página e um botão separados para confirmar se o processo é extremamente importante para a segurança.</span><span class="sxs-lookup"><span data-stu-id="16ff3-142">**Important** The technique of first selecting a record to delete and then using a separate page and button to confirm the process is extremely important for security.</span></span> <span data-ttu-id="16ff3-143">Como você leu nos tutoriais anteriores, fazer *qualquer* tipo de alteração em seu site *sempre* deve ser feito usando um formulário &mdash; ou seja, usando uma operação http post.</span><span class="sxs-lookup"><span data-stu-id="16ff3-143">As you've read in previous tutorials, making *any* sort of change to your website should *always* be done using a form &mdash; that is, using an HTTP POST operation.</span></span> <span data-ttu-id="16ff3-144">Se você tornou possível alterar o site apenas clicando em um link (ou seja, usando uma operação GET), as pessoas podem fazer solicitações simples ao seu site e excluir seus dados.</span><span class="sxs-lookup"><span data-stu-id="16ff3-144">If you made it possible to change the site just by clicking a link (that is, using a GET operation), people could make simple requests to your site and delete your data.</span></span> <span data-ttu-id="16ff3-145">Até mesmo um rastreador de mecanismo de pesquisa que está indexando seu site pode excluir dados inadvertidamente apenas pelos links a seguir.</span><span class="sxs-lookup"><span data-stu-id="16ff3-145">Even a search-engine crawler that's indexing your site could inadvertently delete data just by following links.</span></span>
> 
> <span data-ttu-id="16ff3-146">Quando seu aplicativo permite que as pessoas alterem um registro, você precisa apresentar o registro ao usuário para edição de qualquer forma.</span><span class="sxs-lookup"><span data-stu-id="16ff3-146">When your app lets people change a record, you have to present the record to the user for editing anyway.</span></span> <span data-ttu-id="16ff3-147">Mas você pode estar tentado a ignorar esta etapa para excluir um registro.</span><span class="sxs-lookup"><span data-stu-id="16ff3-147">But you might be tempted to skip this step for deleting a record.</span></span> <span data-ttu-id="16ff3-148">No entanto, não pule essa etapa.</span><span class="sxs-lookup"><span data-stu-id="16ff3-148">Don't skip that step, though.</span></span> <span data-ttu-id="16ff3-149">(Também é útil para os usuários ver o registro e confirmar que estão excluindo o registro que pretendiam.)</span><span class="sxs-lookup"><span data-stu-id="16ff3-149">(It's also helpful for users to see the record and confirm that they're deleting the record that they intended.)</span></span>
> 
> <span data-ttu-id="16ff3-150">Em um conjunto de tutorial subsequente, você verá como adicionar a funcionalidade de logon para que um usuário precise fazer logon antes de excluir um registro.</span><span class="sxs-lookup"><span data-stu-id="16ff3-150">In a subsequent tutorial set, you'll see how to add login functionality so a user would have to log in before deleting a record.</span></span>

<span data-ttu-id="16ff3-151">Crie uma página chamada *DeleteMovie. cshtml* e substitua o que está no arquivo pela seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="16ff3-151">Create a page named *DeleteMovie.cshtml* and replace what's in the file with the following markup:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

<span data-ttu-id="16ff3-152">Essa marcação é como as páginas *EditMovie* , exceto que, em vez de usar caixas de texto (`<input type="text">`), a marcação inclui elementos `<span>`.</span><span class="sxs-lookup"><span data-stu-id="16ff3-152">This markup is like the *EditMovie* pages, except that instead of using text boxes (`<input type="text">`), the markup includes `<span>` elements.</span></span> <span data-ttu-id="16ff3-153">Não há nada aqui para editar.</span><span class="sxs-lookup"><span data-stu-id="16ff3-153">There's nothing here to edit.</span></span> <span data-ttu-id="16ff3-154">Tudo o que você precisa fazer é exibir os detalhes do filme para que os usuários possam ter certeza de que estão excluindo o filme correto.</span><span class="sxs-lookup"><span data-stu-id="16ff3-154">All you have to do is display the movie details so that users can make sure that they're deleting the right movie.</span></span>

<span data-ttu-id="16ff3-155">A marcação já contém um link que permite que o usuário retorne à página de listagem de filmes.</span><span class="sxs-lookup"><span data-stu-id="16ff3-155">The markup already contains a link that lets the user return to the movie listing page.</span></span>

<span data-ttu-id="16ff3-156">Como na página *EditMovie* , a ID do filme selecionado é armazenada em um campo oculto.</span><span class="sxs-lookup"><span data-stu-id="16ff3-156">As in the *EditMovie* page, the ID of the selected movie is stored in a hidden field.</span></span> <span data-ttu-id="16ff3-157">(Ele é passado para a página em primeiro lugar como um valor de cadeia de caracteres de consulta.) Há uma chamada `Html.ValidationSummary` que exibirá erros de validação.</span><span class="sxs-lookup"><span data-stu-id="16ff3-157">(It's passed into the page in the first place as a query string value.) There's an `Html.ValidationSummary` call that will display validation errors.</span></span> <span data-ttu-id="16ff3-158">Nesse caso, o erro pode ser que nenhuma ID de filme foi passada para a página ou que a ID do filme seja inválida.</span><span class="sxs-lookup"><span data-stu-id="16ff3-158">In this case, the error might be that no movie ID was passed to the page or that the movie ID is invalid.</span></span> <span data-ttu-id="16ff3-159">Essa situação pode ocorrer se alguém executar essa página sem primeiro selecionar um filme na página *filmes* .</span><span class="sxs-lookup"><span data-stu-id="16ff3-159">This situation could occur if someone ran this page without first selecting a movie in the *Movies* page.</span></span>

<span data-ttu-id="16ff3-160">A legenda do botão é **excluir filme**e seu atributo Name é definido como `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="16ff3-160">The button caption is **Delete Movie**, and its name attribute is set to `buttonDelete`.</span></span> <span data-ttu-id="16ff3-161">O atributo `name` será usado no código para identificar o botão que enviou o formulário.</span><span class="sxs-lookup"><span data-stu-id="16ff3-161">The `name` attribute will be used in the code to identify the button that submitted the form.</span></span>

<span data-ttu-id="16ff3-162">Você precisará escrever código para 1) ler os detalhes do filme quando a página for exibida pela primeira vez e 2), na verdade, excluir o filme quando o usuário clicar no botão.</span><span class="sxs-lookup"><span data-stu-id="16ff3-162">You'll have to write code to 1) read the movie details when the page is first displayed and 2) actually delete the movie when the user clicks the button.</span></span>

## <a name="adding-code-to-read-a-single-movie"></a><span data-ttu-id="16ff3-163">Adicionando código para ler um único filme</span><span class="sxs-lookup"><span data-stu-id="16ff3-163">Adding Code to Read a Single Movie</span></span>

<span data-ttu-id="16ff3-164">Na parte superior da página *DeleteMovie. cshtml* , adicione o seguinte bloco de código:</span><span class="sxs-lookup"><span data-stu-id="16ff3-164">At the top of the *DeleteMovie.cshtml* page, add the following code block:</span></span>

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

<span data-ttu-id="16ff3-165">Essa marcação é igual ao código correspondente na página *EditMovie* .</span><span class="sxs-lookup"><span data-stu-id="16ff3-165">This markup is the same as the corresponding code in the *EditMovie* page.</span></span> <span data-ttu-id="16ff3-166">Ele obtém a ID do filme da cadeia de caracteres de consulta e usa a ID para ler um registro do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="16ff3-166">It gets the movie ID out of the query string and uses the ID to read a record from the database.</span></span> <span data-ttu-id="16ff3-167">O código inclui o teste de validação (`IsInt()` e `row != null`) para garantir que a ID do filme que está sendo passada para a página seja válida.</span><span class="sxs-lookup"><span data-stu-id="16ff3-167">The code includes the validation test (`IsInt()` and `row != null`) to make sure that the movie ID being passed to the page is valid.</span></span>

<span data-ttu-id="16ff3-168">Lembre-se de que esse código só deve ser executado na primeira vez em que a página for executada.</span><span class="sxs-lookup"><span data-stu-id="16ff3-168">Remember that this code should only run the first time the page runs.</span></span> <span data-ttu-id="16ff3-169">Você não deseja ler novamente o registro do filme do banco de dados quando o usuário clica no botão **excluir filme** .</span><span class="sxs-lookup"><span data-stu-id="16ff3-169">You don't want to re-read the movie record from the database when the user clicks the **Delete Movie** button.</span></span> <span data-ttu-id="16ff3-170">Portanto, o código para ler o filme está dentro de um teste que diz `if(!IsPost)` &mdash; ou seja, *se a solicitação não for uma operação post (envio de formulário)* .</span><span class="sxs-lookup"><span data-stu-id="16ff3-170">Therefore, code to read the movie is inside a test that says `if(!IsPost)` &mdash; that is, *if the request is not a post operation (form submission)*.</span></span>

## <a name="adding-code-to-delete-the-selected-movie"></a><span data-ttu-id="16ff3-171">Adicionando código para excluir o filme selecionado</span><span class="sxs-lookup"><span data-stu-id="16ff3-171">Adding Code to Delete the Selected Movie</span></span>

<span data-ttu-id="16ff3-172">Para excluir o filme quando o usuário clicar no botão, adicione o seguinte código apenas dentro da chave de fechamento do bloco de `@`:</span><span class="sxs-lookup"><span data-stu-id="16ff3-172">To delete the movie when the user clicks the button, add the following code just inside the closing brace of the `@` block:</span></span>

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

<span data-ttu-id="16ff3-173">Esse código é semelhante ao código para atualizar um registro existente, mas mais simples.</span><span class="sxs-lookup"><span data-stu-id="16ff3-173">This code is similar to the code for updating an existing record, but simpler.</span></span> <span data-ttu-id="16ff3-174">O código basicamente executa uma instrução SQL `Delete`.</span><span class="sxs-lookup"><span data-stu-id="16ff3-174">The code basically runs a SQL `Delete` statement.</span></span>

 <span data-ttu-id="16ff3-175">Como na página *EditMovie* , o código está em um bloco de `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="16ff3-175">As in the *EditMovie* page, the code is in an `if(IsPost)` block.</span></span> <span data-ttu-id="16ff3-176">Desta vez, a condição de `if()` é um pouco mais complicada:</span><span class="sxs-lookup"><span data-stu-id="16ff3-176">This time, the `if()` condition is a little more complicated:</span></span> 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

<span data-ttu-id="16ff3-177">Há duas condições aqui.</span><span class="sxs-lookup"><span data-stu-id="16ff3-177">There are two conditions here.</span></span> <span data-ttu-id="16ff3-178">A primeira é que a página está sendo enviada, como você viu antes de &mdash; `if(IsPost)`.</span><span class="sxs-lookup"><span data-stu-id="16ff3-178">The first is that the page is being submitted, as you've seen before &mdash; `if(IsPost)`.</span></span>

<span data-ttu-id="16ff3-179">A segunda condição é `!Request["buttonDelete"].IsEmpty()`, o que significa que a solicitação tem um objeto chamado `buttonDelete`.</span><span class="sxs-lookup"><span data-stu-id="16ff3-179">The second condition is `!Request["buttonDelete"].IsEmpty()`, meaning that the request has an object named `buttonDelete`.</span></span> <span data-ttu-id="16ff3-180">Reconhecidamente, é uma maneira indireta de testar qual botão enviou o formulário.</span><span class="sxs-lookup"><span data-stu-id="16ff3-180">Admittedly, it's an indirect way of testing which button submitted the form.</span></span> <span data-ttu-id="16ff3-181">Se um formulário contiver vários botões de envio, somente o nome do botão que foi clicado aparecerá na solicitação.</span><span class="sxs-lookup"><span data-stu-id="16ff3-181">If a form contains multiple submit buttons, only the name of the button that was clicked appears in the request.</span></span> <span data-ttu-id="16ff3-182">Portanto, logicamente, se o nome de um botão específico for exibido na solicitação &mdash; ou conforme indicado no código, se esse botão não estiver vazio &mdash; é o botão que enviou o formulário.</span><span class="sxs-lookup"><span data-stu-id="16ff3-182">Therefore, logically, if the name of a particular button appears in the request &mdash; or as stated in the code, if that button isn't empty &mdash; that's the button that submitted the form.</span></span>

<span data-ttu-id="16ff3-183">O operador de `&&` significa "and" (and lógico).</span><span class="sxs-lookup"><span data-stu-id="16ff3-183">The `&&` operator means "and" (logical AND).</span></span> <span data-ttu-id="16ff3-184">Portanto, toda a condição de `if` é...</span><span class="sxs-lookup"><span data-stu-id="16ff3-184">Therefore the entire `if` condition is ...</span></span>

<span data-ttu-id="16ff3-185">*Esta solicitação é uma postagem (não é uma solicitação de primeira vez)*</span><span class="sxs-lookup"><span data-stu-id="16ff3-185">*This request is a post (not a first-time request)*</span></span>  
  
 <span data-ttu-id="16ff3-186">AND</span><span class="sxs-lookup"><span data-stu-id="16ff3-186">AND</span></span>  
  
<span data-ttu-id="16ff3-187">*O botão de* `buttonDelete`*foi o botão que enviou o formulário.*</span><span class="sxs-lookup"><span data-stu-id="16ff3-187">*The* `buttonDelete`*button was the button that submitted the form.*</span></span>

<span data-ttu-id="16ff3-188">Esse formulário (na verdade, essa página) contém apenas um botão, portanto, o teste adicional para `buttonDelete` tecnicamente não é necessário.</span><span class="sxs-lookup"><span data-stu-id="16ff3-188">This form (in fact, this page) contains only one button, so the additional test for `buttonDelete` is technically not required.</span></span> <span data-ttu-id="16ff3-189">Ainda assim, você está prestes a executar uma operação que removerá permanentemente os dados.</span><span class="sxs-lookup"><span data-stu-id="16ff3-189">Still, you're about to perform an operation that will permanently remove data.</span></span> <span data-ttu-id="16ff3-190">Portanto, você deve ter a certeza de que está executando a operação somente quando o usuário a solicitou explicitamente.</span><span class="sxs-lookup"><span data-stu-id="16ff3-190">So you want to be as sure as possible that you're performing the operation only when the user has explicitly requested it.</span></span> <span data-ttu-id="16ff3-191">Por exemplo, suponha que você expandiu essa página posteriormente e adicionou outros botões a ela.</span><span class="sxs-lookup"><span data-stu-id="16ff3-191">For example, suppose that you expanded this page later and added other buttons to it.</span></span> <span data-ttu-id="16ff3-192">Mesmo assim, o código que exclui o filme será executado somente se o botão de `buttonDelete` foi clicado.</span><span class="sxs-lookup"><span data-stu-id="16ff3-192">Even then, the code that deletes the movie will run only if the `buttonDelete` button was clicked.</span></span>

<span data-ttu-id="16ff3-193">Como na página *EditMovie* , você obtém a ID do campo oculto e, em seguida, executa o comando SQL.</span><span class="sxs-lookup"><span data-stu-id="16ff3-193">As in the *EditMovie* page, you get the ID from the hidden field and then run the SQL command.</span></span> <span data-ttu-id="16ff3-194">A sintaxe para a instrução `Delete` é:</span><span class="sxs-lookup"><span data-stu-id="16ff3-194">The syntax for the `Delete` statement is:</span></span>

`DELETE FROM table WHERE ID = value`

<span data-ttu-id="16ff3-195">É vital incluir a cláusula `WHERE` e a ID.</span><span class="sxs-lookup"><span data-stu-id="16ff3-195">It's vital to include the `WHERE` clause and the ID.</span></span> <span data-ttu-id="16ff3-196">Se você sair da cláusula WHERE, *todos os registros na tabela serão excluídos*.</span><span class="sxs-lookup"><span data-stu-id="16ff3-196">If you leave out the WHERE clause, *all the records in the table will be deleted*.</span></span> <span data-ttu-id="16ff3-197">Como já foi visto, você passa o valor de ID para o comando SQL usando um espaço reservado.</span><span class="sxs-lookup"><span data-stu-id="16ff3-197">As you have seen, you pass the ID value to the SQL command by using a placeholder.</span></span>

## <a name="testing-the-movie-delete-process"></a><span data-ttu-id="16ff3-198">Testando o processo de exclusão do filme</span><span class="sxs-lookup"><span data-stu-id="16ff3-198">Testing the Movie Delete Process</span></span>

<span data-ttu-id="16ff3-199">Agora você pode testar.</span><span class="sxs-lookup"><span data-stu-id="16ff3-199">Now you can test.</span></span> <span data-ttu-id="16ff3-200">Execute a página *filmes* e clique em **excluir** ao lado de um filme.</span><span class="sxs-lookup"><span data-stu-id="16ff3-200">Run the *Movies* page, and click **Delete** next to a movie.</span></span> <span data-ttu-id="16ff3-201">Quando a página *DeleteMovie* for exibida, clique em **excluir filme**.</span><span class="sxs-lookup"><span data-stu-id="16ff3-201">When the *DeleteMovie* page appears, click **Delete Movie**.</span></span>

![Página excluir filme com o botão excluir filme realçado](deleting-data/_static/image4.png)

<span data-ttu-id="16ff3-203">Quando você clica no botão, o código exclui os filmes e retorna para a listagem de filmes.</span><span class="sxs-lookup"><span data-stu-id="16ff3-203">When you click the button, the code deletes the movies and returns to the movie listing.</span></span> <span data-ttu-id="16ff3-204">Lá, você pode pesquisar o filme excluído e confirmar que ele foi excluído.</span><span class="sxs-lookup"><span data-stu-id="16ff3-204">There you can search for the deleted movie and confirm that it's been deleted.</span></span>

## <a name="coming-up-next"></a><span data-ttu-id="16ff3-205">Chegando em seguida</span><span class="sxs-lookup"><span data-stu-id="16ff3-205">Coming Up Next</span></span>

<span data-ttu-id="16ff3-206">O próximo tutorial mostra como dar a você uma aparência e um layout comuns a todas as páginas de seu site.</span><span class="sxs-lookup"><span data-stu-id="16ff3-206">The next tutorial shows you how to give all the pages on your site a common look and layout.</span></span>

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a><span data-ttu-id="16ff3-207">Listagem completa da página de filme (atualizada com links de exclusão)</span><span class="sxs-lookup"><span data-stu-id="16ff3-207">Complete Listing for Movie Page (Updated with Delete Links)</span></span>

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a><span data-ttu-id="16ff3-208">Listagem completa da página DeleteMovie</span><span class="sxs-lookup"><span data-stu-id="16ff3-208">Complete Listing for DeleteMovie Page</span></span>

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="16ff3-209">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="16ff3-209">Additional Resources</span></span>

- [<span data-ttu-id="16ff3-210">Introdução à programação da Web do ASP.NET usando a sintaxe do Razor</span><span class="sxs-lookup"><span data-stu-id="16ff3-210">Introduction to ASP.NET Web Programming by Using the Razor Syntax</span></span>](../introducing-razor-syntax-c.md)
- <span data-ttu-id="16ff3-211">[Instrução SQL Delete](http://www.w3schools.com/sql/sql_delete.asp) no site W3Schools</span><span class="sxs-lookup"><span data-stu-id="16ff3-211">[SQL DELETE Statement](http://www.w3schools.com/sql/sql_delete.asp) on the W3Schools site</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="16ff3-212">[Anterior](updating-data.md)
> [Próximo](layouts.md)</span><span class="sxs-lookup"><span data-stu-id="16ff3-212">[Previous](updating-data.md)
[Next](layouts.md)</span></span>
