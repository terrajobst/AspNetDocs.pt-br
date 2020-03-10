---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Adicionando uma nova categoria à DropDownList usando a interface do usuário do jQuery | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 3207079ee468232e5f75b081421241c232936baf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538832"
---
# <a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="fea45-102">Adicionar uma nova categoria ao DropDownList usando o jQuery UI</span><span class="sxs-lookup"><span data-stu-id="fea45-102">Adding a New Category to the DropDownList using jQuery UI</span></span>

<span data-ttu-id="fea45-103">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fea45-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fea45-104">A marca HTML `Select` é ideal para apresentar uma lista de dados de categoria fixas, mas muitas vezes você precisa adicionar uma nova categoria.</span><span class="sxs-lookup"><span data-stu-id="fea45-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="fea45-105">Suponha que queremos adicionar o gênero "Opera" às categorias em nosso banco de dados?</span><span class="sxs-lookup"><span data-stu-id="fea45-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="fea45-106">Nesta seção, usaremos a interface do usuário do jQuery para adicionar uma caixa de diálogo que podemos usar para adicionar uma nova categoria.</span><span class="sxs-lookup"><span data-stu-id="fea45-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="fea45-107">A imagem abaixo mostra como a interface do usuário estará presente no navegador.</span><span class="sxs-lookup"><span data-stu-id="fea45-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="fea45-108">Quando um usuário seleciona o link **Adicionar novo gênero** , uma caixa de diálogo pop-up solicita ao usuário um novo nome de gênero (e, opcionalmente, uma descrição).</span><span class="sxs-lookup"><span data-stu-id="fea45-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="fea45-109">A imagem abaixo mostra a caixa de diálogo Adicionar pop-up de **gênero** .</span><span class="sxs-lookup"><span data-stu-id="fea45-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="fea45-110">Quando um novo nome de gênero é inserido e o botão **salvar** é enviado por push, acontece o seguinte:</span><span class="sxs-lookup"><span data-stu-id="fea45-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="fea45-111">Uma chamada AJAX posta os dados no método Create do controlador gênero, que salva o novo gênero no banco de dados e retorna as novas informações de gênero (nome e ID do gênero) como JSON.</span><span class="sxs-lookup"><span data-stu-id="fea45-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="fea45-112">O JavaScript adiciona os novos dados de gênero à lista de seleção.</span><span class="sxs-lookup"><span data-stu-id="fea45-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="fea45-113">O JavaScript torna o novo gênero o item selecionado.</span><span class="sxs-lookup"><span data-stu-id="fea45-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="fea45-114">Na imagem abaixo, o **Opera** foi adicionado ao banco de dados e selecionado na lista suspensa **gênero** .</span><span class="sxs-lookup"><span data-stu-id="fea45-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="fea45-115">Abra o arquivo *Views\StoreManager\Create.cshtml* e substitua a marcação de gênero pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="fea45-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="fea45-116">A exibição parcial `_ChooseGenre` conterá toda a lógica para conectar o JavaScript e o jQuery usados para implementar o recurso Adicionar novo gênero.</span><span class="sxs-lookup"><span data-stu-id="fea45-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="fea45-117">Depois de concluirmos o código, será simples fazer o mesmo com a interface do usuário do artista.</span><span class="sxs-lookup"><span data-stu-id="fea45-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="fea45-118">Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta *Views\StoreManager* e selecione **Adicionar**e, em seguida, **Exibir**.</span><span class="sxs-lookup"><span data-stu-id="fea45-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="fea45-119">Na entrada **nome da exibição** , digite `_ChooseGenre`, em seguida, selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="fea45-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="fea45-120">Substitua a marcação no arquivo *Views\StoreManager\\_ChooseGenre. cshtml* pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="fea45-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="fea45-121">A primeira linha declara que estamos passando um `Album` como nosso modelo, exatamente a mesma instrução Model encontrada na exibição Create.</span><span class="sxs-lookup"><span data-stu-id="fea45-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="fea45-122">As próximas linhas são a marcação auxiliar de **rótulo** .</span><span class="sxs-lookup"><span data-stu-id="fea45-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="fea45-123">A próxima linha é a chamada do auxiliar **DropDownList** , exatamente igual à exibição Create original.</span><span class="sxs-lookup"><span data-stu-id="fea45-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="fea45-124">A próxima linha adiciona um link com o nome `Add New Genre`e o estilo como um botão.</span><span class="sxs-lookup"><span data-stu-id="fea45-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="fea45-125">A linha que contém `ValidationMessageFor` é copiada diretamente da exibição Create.</span><span class="sxs-lookup"><span data-stu-id="fea45-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="fea45-126">As seguintes linhas:</span><span class="sxs-lookup"><span data-stu-id="fea45-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="fea45-127">Cria uma div oculta, com a ID de `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="fea45-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="fea45-128">Usaremos o jQuery para conectar nossa caixa de diálogo **Adicionar gênero** com a ID `genreDialog` nesta div.</span><span class="sxs-lookup"><span data-stu-id="fea45-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="fea45-129">As duas últimas marcas de script contêm links para os arquivos JavaScript que usaremos para implementar o recurso Adicionar novo gênero.</span><span class="sxs-lookup"><span data-stu-id="fea45-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="fea45-130">O arquivo */scripts/chooseGenre.js* é fornecido para você no projeto, vamos examiná-lo posteriormente no tutorial.</span><span class="sxs-lookup"><span data-stu-id="fea45-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="fea45-131">Execute o aplicativo e clique no botão **Adicionar novo gênero** .</span><span class="sxs-lookup"><span data-stu-id="fea45-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="fea45-132">Na caixa de diálogo **Adicionar gênero** , digite **Opera** na caixa de entrada **nome** .</span><span class="sxs-lookup"><span data-stu-id="fea45-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="fea45-133">Clique no botão **Salvar** .</span><span class="sxs-lookup"><span data-stu-id="fea45-133">Click the **Save** button.</span></span> <span data-ttu-id="fea45-134">Uma chamada AJAX cria a categoria Opera e, em seguida, popula a lista suspensa com Opera e define Opera como o gênero selecionado.</span><span class="sxs-lookup"><span data-stu-id="fea45-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="fea45-135">Insira um artista, título e preço e, em seguida, selecione o botão **criar** .</span><span class="sxs-lookup"><span data-stu-id="fea45-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="fea45-136">Se você inserir um preço menor que $8.99, o novo álbum aparecerá na parte superior da exibição do índice.</span><span class="sxs-lookup"><span data-stu-id="fea45-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="fea45-137">Verifique se a nova entrada de álbum foi salva no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="fea45-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="fea45-138">Tente criar um novo gênero com apenas uma letra.</span><span class="sxs-lookup"><span data-stu-id="fea45-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="fea45-139">O código a seguir no arquivo *Models\Genre.cs* define o comprimento mínimo e máximo do nome do gênero.</span><span class="sxs-lookup"><span data-stu-id="fea45-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="fea45-140">Relatórios de validação do lado do cliente você deve inserir uma cadeia de caracteres entre 2 e 20 caracteres.</span><span class="sxs-lookup"><span data-stu-id="fea45-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="fea45-141">Examinando como um novo gênero é adicionado ao banco de dados e à lista de seleção.</span><span class="sxs-lookup"><span data-stu-id="fea45-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="fea45-142">Abra o arquivo *Scripts\chooseGenre.js* e examine o código.</span><span class="sxs-lookup"><span data-stu-id="fea45-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="fea45-143">A segunda linha usa a ID `genreDialog` para criar uma caixa de diálogo na marca DIV no arquivo *Views\StoreManager\\_ChooseGenre. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="fea45-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="fea45-144">A maioria dos parâmetros nomeados é auto-explicativa.</span><span class="sxs-lookup"><span data-stu-id="fea45-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="fea45-145">O parâmetro `autoOpen` é definido como false, a seleção do botão **criar gênero** abrirá a caixa de diálogo explicitamente (isso é descrito por último em).</span><span class="sxs-lookup"><span data-stu-id="fea45-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="fea45-146">A caixa de diálogo tem dois botões, **salvar** e **Cancelar**.</span><span class="sxs-lookup"><span data-stu-id="fea45-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="fea45-147">O botão **Cancelar** fecha a caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="fea45-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="fea45-148">O código a seguir mostra a função de botão **salvar** .</span><span class="sxs-lookup"><span data-stu-id="fea45-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="fea45-149">O `var createGenreForm` é selecionado na ID de `createGenreForm`.</span><span class="sxs-lookup"><span data-stu-id="fea45-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="fea45-150">A ID de `createGenreForm` foi definida no código a seguir encontrado no arquivo *Views\Genre\\_CreateGenre. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="fea45-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="fea45-151">A sobrecarga auxiliar [HTML. BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) usada no arquivo *Views\Genre\\_CreateGenre. cshtml* gera HTML com um atributo action contendo a URL para enviar o formulário.</span><span class="sxs-lookup"><span data-stu-id="fea45-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="fea45-152">Você pode ver isso exibindo a página Criar álbum em um navegador e selecionando Mostrar fonte no navegador.</span><span class="sxs-lookup"><span data-stu-id="fea45-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="fea45-153">A marcação a seguir mostra o HTML gerado que contém a marca de formulário.</span><span class="sxs-lookup"><span data-stu-id="fea45-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="fea45-154">A linha jQuery `$.post` faz uma chamada AJAX para o atributo Action (`/StoreManager/Create`) e passa os dados da caixa de diálogo **criar gênero** .</span><span class="sxs-lookup"><span data-stu-id="fea45-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="fea45-155">Os dados consistem no nome do novo gênero e uma descrição opcional.</span><span class="sxs-lookup"><span data-stu-id="fea45-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="fea45-156">Se a chamada AJAX for bem-sucedida, o novo nome e valor do gênero serão adicionados à marcação Select e o novo gênero será definido como o valor selecionado.</span><span class="sxs-lookup"><span data-stu-id="fea45-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="fea45-157">Como essa é uma marcação gerada dinamicamente, você não pode ver a nova opção Select exibindo a origem no navegador.</span><span class="sxs-lookup"><span data-stu-id="fea45-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="fea45-158">Você pode ver o novo HTML com as ferramentas de desenvolvedor do IE 9 F12.</span><span class="sxs-lookup"><span data-stu-id="fea45-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="fea45-159">Para exibir a nova opção de seleção, no Internet Explorer 9, pressione a tecla F12 para iniciar as ferramentas de desenvolvedor F12.</span><span class="sxs-lookup"><span data-stu-id="fea45-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="fea45-160">Navegue até a página criar e adicione um novo gênero para que o novo gênero seja selecionado na lista de seleção de gênero.</span><span class="sxs-lookup"><span data-stu-id="fea45-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="fea45-161">Nas ferramentas de desenvolvedor F12:</span><span class="sxs-lookup"><span data-stu-id="fea45-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="fea45-162">Selecione a guia HTML.</span><span class="sxs-lookup"><span data-stu-id="fea45-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="fea45-163">Pressione o ícone de atualização.</span><span class="sxs-lookup"><span data-stu-id="fea45-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="fea45-164">Na caixa de pesquisa, insira Gêneroid.</span><span class="sxs-lookup"><span data-stu-id="fea45-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="fea45-165">Usando o ícone seguinte,</span><span class="sxs-lookup"><span data-stu-id="fea45-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="fea45-166">Navegue até a seguinte marca de seleção:</span><span class="sxs-lookup"><span data-stu-id="fea45-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="fea45-167">Expanda o último valor de opção.</span><span class="sxs-lookup"><span data-stu-id="fea45-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="fea45-168">O código a seguir no arquivo *Scripts\chooseGenre.js* mostra como o botão **Adicionar novo gênero** é conectado ao evento de clique e como a caixa de diálogo **Adicionar novo gênero** é criada.</span><span class="sxs-lookup"><span data-stu-id="fea45-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="fea45-169">A primeira linha cria uma função de clique anexada ao botão **Adicionar novo gênero** .</span><span class="sxs-lookup"><span data-stu-id="fea45-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="fea45-170">A marcação a seguir do arquivo Views\StoreManager\\_ChooseGenre. cshtml mostra como o botão **Adicionar novo gênero** é criado:</span><span class="sxs-lookup"><span data-stu-id="fea45-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="fea45-171">O método Load cria e abre a caixa de diálogo Adicionar gênero e chama o método jQuery `parse` para que a validação do cliente ocorra nos dados inseridos na caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="fea45-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="fea45-172">Nesta seção, você aprendeu a criar uma caixa de diálogo que pode ser usada para adicionar novos dados de categoria a uma lista de seleção.</span><span class="sxs-lookup"><span data-stu-id="fea45-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="fea45-173">Você pode seguir o mesmo procedimento para criar a interface do usuário para adicionar um novo artista à lista de seleção do artista.</span><span class="sxs-lookup"><span data-stu-id="fea45-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="fea45-174">Este tutorial forneceu uma visão geral de como trabalhar com **o auxiliar HTML**do MVC do ASP.net.</span><span class="sxs-lookup"><span data-stu-id="fea45-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="fea45-175">Para obter informações adicionais sobre como trabalhar com **DropDownList**, consulte a seção de referências de adição abaixo.</span><span class="sxs-lookup"><span data-stu-id="fea45-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="fea45-176">Informe-nos se este tutorial foi útil.</span><span class="sxs-lookup"><span data-stu-id="fea45-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="fea45-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="fea45-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="fea45-178">Referências adicionais</span><span class="sxs-lookup"><span data-stu-id="fea45-178">Additional References</span></span>

- <span data-ttu-id="fea45-179">[ASP.NET MVC – tutorial de listas suspensas em cascata](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) por [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="fea45-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="fea45-180">[Escolhido](https://harvesthq.github.com/chosen/) Um plug-in de JavaScript que dá suporte a seleção múltipla e filtragem.</span><span class="sxs-lookup"><span data-stu-id="fea45-180">[Chosen](https://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="fea45-181">Colaboradores</span><span class="sxs-lookup"><span data-stu-id="fea45-181">Contributors</span></span>

- [<span data-ttu-id="fea45-182">Radu Enuca</span><span class="sxs-lookup"><span data-stu-id="fea45-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="fea45-183">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="fea45-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="fea45-184">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="fea45-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="fea45-185">Revisores</span><span class="sxs-lookup"><span data-stu-id="fea45-185">Reviewers</span></span>

- <span data-ttu-id="fea45-186">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="fea45-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="fea45-187">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="fea45-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="fea45-188">Mike Papa</span><span class="sxs-lookup"><span data-stu-id="fea45-188">Mike Pope</span></span>
- <span data-ttu-id="fea45-189">Tom Dykstra</span><span class="sxs-lookup"><span data-stu-id="fea45-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="fea45-190">Anterior</span><span class="sxs-lookup"><span data-stu-id="fea45-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
