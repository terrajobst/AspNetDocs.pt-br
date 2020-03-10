---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Examinando como o ASP.NET MVC aplica Scaffold o auxiliar DropDownList | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 275b20ad964b3e8ddc272a7448f0740ed0891eff
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78614901"
---
# <a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="f1c57-102">Examinar como o ASP.NET MVC faz scaffold do auxiliar DropDownList</span><span class="sxs-lookup"><span data-stu-id="f1c57-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>

<span data-ttu-id="f1c57-103">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f1c57-103">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f1c57-104">Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *controladores* e selecione **Adicionar controlador**.</span><span class="sxs-lookup"><span data-stu-id="f1c57-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="f1c57-105">Nomeie o controlador **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="f1c57-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="f1c57-106">Defina as opções para a caixa de diálogo **Adicionar controlador** , conforme mostrado na imagem abaixo.</span><span class="sxs-lookup"><span data-stu-id="f1c57-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="f1c57-107">Edite a exibição *StoreManager\Index.cshtml* e remova `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="f1c57-108">Remover `AlbumArtUrl` tornará a apresentação mais legível.</span><span class="sxs-lookup"><span data-stu-id="f1c57-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="f1c57-109">O código completo é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="f1c57-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="f1c57-110">Abra o arquivo *Controllers\StoreManagerController.cs* e localize o método `Index`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="f1c57-111">Adicione a cláusula `OrderBy` para que os álbuns sejam classificados por preço.</span><span class="sxs-lookup"><span data-stu-id="f1c57-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="f1c57-112">O código completo é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="f1c57-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="f1c57-113">Classificar por preço tornará mais fácil testar as alterações no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1c57-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="f1c57-114">Ao testar os métodos editar e criar, você pode usar um preço baixo para que os dados salvos sejam exibidos primeiro.</span><span class="sxs-lookup"><span data-stu-id="f1c57-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="f1c57-115">Abra o arquivo *StoreManager\Edit.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="f1c57-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="f1c57-116">Adicione a linha a seguir logo após a marca de legenda.</span><span class="sxs-lookup"><span data-stu-id="f1c57-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="f1c57-117">O código a seguir mostra o contexto dessa alteração:</span><span class="sxs-lookup"><span data-stu-id="f1c57-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="f1c57-118">O `AlbumId` é necessário para fazer alterações em um registro de álbum.</span><span class="sxs-lookup"><span data-stu-id="f1c57-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="f1c57-119">Pressione CTRL+F5 para executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="f1c57-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="f1c57-120">Selecione para o link de **administrador** e, em seguida, selecione o link **criar novo** para criar um novo álbum.</span><span class="sxs-lookup"><span data-stu-id="f1c57-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="f1c57-121">Verifique se as informações do álbum foram salvas.</span><span class="sxs-lookup"><span data-stu-id="f1c57-121">Verify the album information was saved.</span></span> <span data-ttu-id="f1c57-122">Edite um álbum e verifique se as alterações feitas foram persistidas.</span><span class="sxs-lookup"><span data-stu-id="f1c57-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="f1c57-123">O esquema do álbum</span><span class="sxs-lookup"><span data-stu-id="f1c57-123">The Album Schema</span></span>

<span data-ttu-id="f1c57-124">O controlador de `StoreManager` criado pelo mecanismo scaffolding do MVC permite o acesso CRUD (criar, ler, atualizar, excluir) aos álbuns no banco de dados da loja de música.</span><span class="sxs-lookup"><span data-stu-id="f1c57-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="f1c57-125">O esquema para informações de álbum é mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="f1c57-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="f1c57-126">A tabela `Albums` não armazena o gênero e a descrição do álbum, armazena uma chave estrangeira na tabela `Genres`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="f1c57-127">A tabela `Genres` contém o nome do gênero e a descrição.</span><span class="sxs-lookup"><span data-stu-id="f1c57-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="f1c57-128">Da mesma forma, a tabela `Albums` não contém o nome dos artistas do álbum, mas uma chave estrangeira para a tabela `Artists`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="f1c57-129">A tabela `Artists` contém o nome do artista.</span><span class="sxs-lookup"><span data-stu-id="f1c57-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="f1c57-130">Se você examinar os dados na tabela `Albums`, poderá ver que cada linha contém uma chave estrangeira para a tabela `Genres` e uma chave estrangeira para a tabela `Artists`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="f1c57-131">A imagem abaixo mostra alguns dados de tabela da tabela `Albums`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="f1c57-132">A marca HTML Select</span><span class="sxs-lookup"><span data-stu-id="f1c57-132">The HTML Select Tag</span></span>

<span data-ttu-id="f1c57-133">O elemento HTML `<select>` (criado pelo auxiliar HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) ) é usado para exibir uma lista completa de valores (como a lista de gêneros).</span><span class="sxs-lookup"><span data-stu-id="f1c57-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="f1c57-134">Para editar formulários, quando o valor atual é conhecido, a lista de seleção pode exibir o valor atual.</span><span class="sxs-lookup"><span data-stu-id="f1c57-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="f1c57-135">Vimos isso anteriormente quando definimos o valor selecionado como **comédia**.</span><span class="sxs-lookup"><span data-stu-id="f1c57-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="f1c57-136">A lista de seleção é ideal para exibir dados de categoria ou de chave estrangeira.</span><span class="sxs-lookup"><span data-stu-id="f1c57-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="f1c57-137">O elemento `<select>` para a chave estrangeira do gênero exibe a lista de possíveis nomes de gênero, mas quando você salva o formulário, a propriedade gênero é atualizada com o valor de chave estrangeira do gênero, não o nome do gênero exibido.</span><span class="sxs-lookup"><span data-stu-id="f1c57-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="f1c57-138">Na imagem abaixo, o gênero selecionado é o **disco** e o artista é o **Verão de Donna**.</span><span class="sxs-lookup"><span data-stu-id="f1c57-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="f1c57-139">Examinando o código com Scaffold do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="f1c57-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="f1c57-140">Abra o arquivo *Controllers\StoreManagerController.cs* e localize o método `HTTP GET Create`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="f1c57-141">O método `Create` adiciona dois objetos [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) à `ViewBag`, um para conter as informações de gênero e um para conter as informações do artista.</span><span class="sxs-lookup"><span data-stu-id="f1c57-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="f1c57-142">A sobrecarga do construtor [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) usada acima usa três argumentos:</span><span class="sxs-lookup"><span data-stu-id="f1c57-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="f1c57-143">*itens*: um [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contendo os itens na lista.</span><span class="sxs-lookup"><span data-stu-id="f1c57-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="f1c57-144">No exemplo acima, a lista de gêneros retornada por `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="f1c57-145">*DataValueField*: o nome da propriedade na lista **IEnumerable** que contém o valor da chave.</span><span class="sxs-lookup"><span data-stu-id="f1c57-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="f1c57-146">No exemplo acima, `GenreId` e `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="f1c57-147">*DataTextField*: o nome da propriedade na lista **IEnumerable** que contém as informações a serem exibidas.</span><span class="sxs-lookup"><span data-stu-id="f1c57-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="f1c57-148">Na tabela artistas e gênero, o campo `name` é usado.</span><span class="sxs-lookup"><span data-stu-id="f1c57-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="f1c57-149">Abra o arquivo *Views\StoreManager\Create.cshtml* e examine a marcação auxiliar `Html.DropDownList` para o campo gênero.</span><span class="sxs-lookup"><span data-stu-id="f1c57-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="f1c57-150">A primeira linha mostra que o modo de exibição de criação usa um modelo de `Album`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="f1c57-151">No método `Create` mostrado acima, nenhum modelo foi passado, portanto, a exibição Obtém um modelo de `Album` **nulo** .</span><span class="sxs-lookup"><span data-stu-id="f1c57-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="f1c57-152">Neste ponto, estamos criando um novo álbum para que não tenhamos dados de `Album` para ele.</span><span class="sxs-lookup"><span data-stu-id="f1c57-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="f1c57-153">A sobrecarga de [HTML. DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) mostrada acima usa o nome do campo para associar ao modelo.</span><span class="sxs-lookup"><span data-stu-id="f1c57-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="f1c57-154">Ele também usa esse nome para procurar um objeto **ViewBag** contendo um objeto [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) .</span><span class="sxs-lookup"><span data-stu-id="f1c57-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="f1c57-155">Usando essa sobrecarga, é necessário nomear o objeto de lista de **seleção ViewBag** `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="f1c57-156">O segundo parâmetro (`String.Empty`) é o texto a ser exibido quando nenhum item é selecionado.</span><span class="sxs-lookup"><span data-stu-id="f1c57-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="f1c57-157">Isso é exatamente o que queremos ao criar um novo álbum.</span><span class="sxs-lookup"><span data-stu-id="f1c57-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="f1c57-158">Se você removeu o segundo parâmetro e usou o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="f1c57-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="f1c57-159">A lista de seleção teria como padrão o primeiro elemento ou uma pedra em nosso exemplo.</span><span class="sxs-lookup"><span data-stu-id="f1c57-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="f1c57-160">Examinando o método `HTTP POST Create`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="f1c57-161">Essa sobrecarga do método `Create` usa um objeto `album`, criado pelo sistema de associação de modelo MVC ASP.NET dos valores de formulário postados.</span><span class="sxs-lookup"><span data-stu-id="f1c57-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="f1c57-162">Quando você enviar um novo álbum, se o estado do modelo for válido e não houver erros de banco de dados, o novo álbum será adicionado ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="f1c57-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="f1c57-163">A imagem a seguir mostra a criação de um novo álbum.</span><span class="sxs-lookup"><span data-stu-id="f1c57-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="f1c57-164">Você pode usar a [ferramenta Fiddler](http://www.fiddler2.com/fiddler2/) para examinar os valores de formulário postados que a associação de modelo MVC ASP.NET usa para criar o objeto de álbum.</span><span class="sxs-lookup"><span data-stu-id="f1c57-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="f1c57-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span><span class="sxs-lookup"><span data-stu-id="f1c57-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="f1c57-166">Refatorando a criação de seleção de ViewBag</span><span class="sxs-lookup"><span data-stu-id="f1c57-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="f1c57-167">Os métodos `Edit` e o método `HTTP POST Create` têm um código idêntico para configurar alist de **seleção** em **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="f1c57-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="f1c57-168">No espírito de [seca](http://en.wikipedia.org/wiki/Don't_repeat_yourself), iremos refatorar esse código.</span><span class="sxs-lookup"><span data-stu-id="f1c57-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="f1c57-169">Vamos usar esse código refatorado posteriormente.</span><span class="sxs-lookup"><span data-stu-id="f1c57-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="f1c57-170">Crie um novo método para adicionar uma **seleção** de gênero e artista à **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="f1c57-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="f1c57-171">Substitua as duas linhas definindo as `ViewBag` em cada um dos métodos `Create` e `Edit` com uma chamada para o método `SetGenreArtistViewBag`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="f1c57-172">O código completo é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="f1c57-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="f1c57-173">Crie um novo álbum e edite um álbum para verificar se as alterações funcionam.</span><span class="sxs-lookup"><span data-stu-id="f1c57-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="f1c57-174">Passando explicitamente alist de seleção para DropDownList</span><span class="sxs-lookup"><span data-stu-id="f1c57-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="f1c57-175">As exibições criar e editar criadas pelo ASP.NET MVC scaffolding usam a seguinte sobrecarga de **DropDownList** :</span><span class="sxs-lookup"><span data-stu-id="f1c57-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="f1c57-176">A marcação de `DropDownList` para a exibição criar é mostrada abaixo.</span><span class="sxs-lookup"><span data-stu-id="f1c57-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="f1c57-177">Como a propriedade `ViewBag` para o `SelectList` é denominada `GenreId`, o auxiliar **DropDownList** usará a `GenreId`**SelectList** em **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="f1c57-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="f1c57-178">Na seguinte sobrecarga de **DropDownList** , a `SelectList` é passada explicitamente.</span><span class="sxs-lookup"><span data-stu-id="f1c57-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="f1c57-179">Abra o arquivo *Views\StoreManager\Edit.cshtml* e altere a chamada **DropDownList** para explicitamente passar nalist de **seleção**, usando a sobrecarga acima.</span><span class="sxs-lookup"><span data-stu-id="f1c57-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="f1c57-180">Faça isso para a categoria de gênero.</span><span class="sxs-lookup"><span data-stu-id="f1c57-180">Do this for the Genre category.</span></span> <span data-ttu-id="f1c57-181">O código completo é mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="f1c57-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="f1c57-182">Execute o aplicativo e clique no link **administrador** , navegue até um álbum de jazz e selecione o link **Editar** .</span><span class="sxs-lookup"><span data-stu-id="f1c57-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="f1c57-183">Em vez de mostrar jazz como o gênero selecionado no momento, a pedra é exibida.</span><span class="sxs-lookup"><span data-stu-id="f1c57-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="f1c57-184">Quando o argumento da cadeia de caracteres (a propriedade a ser ligada) e o objeto **SelectList** tiverem o mesmo nome, o valor selecionado não será usado.</span><span class="sxs-lookup"><span data-stu-id="f1c57-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="f1c57-185">Quando não há nenhum valor selecionado fornecido, os navegadores assumem como padrão o primeiro elemento nalist de **seleção**(que é a **pedra** no exemplo acima).</span><span class="sxs-lookup"><span data-stu-id="f1c57-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="f1c57-186">Essa é uma limitação conhecida do auxiliar **DropDownList** .</span><span class="sxs-lookup"><span data-stu-id="f1c57-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="f1c57-187">Abra o arquivo *Controllers\StoreManagerController.cs* e altere os nomes de objeto daList de **seleção** para `Genres` e `Artists`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="f1c57-188">O código completo é mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="f1c57-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="f1c57-189">Os nomes de gêneros e artistas são nomes melhores para as categorias, pois eles contêm mais do que apenas a ID de cada categoria.</span><span class="sxs-lookup"><span data-stu-id="f1c57-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="f1c57-190">A refatoração que fizemos anteriormente pagou.</span><span class="sxs-lookup"><span data-stu-id="f1c57-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="f1c57-191">Em vez de alterar **ViewBag** em quatro métodos, nossas alterações foram isoladas para o método `SetGenreArtistViewBag`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="f1c57-192">Altere a chamada **DropDownList** nas exibições criar e editar para usar os novos nomes de **selecionarlist** .</span><span class="sxs-lookup"><span data-stu-id="f1c57-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="f1c57-193">A nova marcação para o modo de exibição de edição é mostrada abaixo:</span><span class="sxs-lookup"><span data-stu-id="f1c57-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="f1c57-194">A exibição Create requer uma cadeia de caracteres vazia para impedir que o primeiro item na SelectList seja exibido.</span><span class="sxs-lookup"><span data-stu-id="f1c57-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="f1c57-195">Crie um novo álbum e edite um álbum para verificar se as alterações funcionam.</span><span class="sxs-lookup"><span data-stu-id="f1c57-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="f1c57-196">Teste o código de edição selecionando um álbum com um gênero diferente de rock.</span><span class="sxs-lookup"><span data-stu-id="f1c57-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="f1c57-197">Usando um modelo de exibição com o auxiliar DropDownList</span><span class="sxs-lookup"><span data-stu-id="f1c57-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="f1c57-198">Crie uma nova classe na pasta ViewModels chamada `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="f1c57-199">Substitua o código na classe `AlbumSelectListViewModel` pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="f1c57-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="f1c57-200">O construtor de `AlbumSelectListViewModel` usa um álbum, uma lista de artistas e gêneros e cria um objeto que contém o álbum e um `SelectList` para gêneros e artistas.</span><span class="sxs-lookup"><span data-stu-id="f1c57-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="f1c57-201">Compile o projeto para que o `AlbumSelectListViewModel` esteja disponível quando criarmos uma exibição na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="f1c57-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="f1c57-202">Adicione um método `EditVM` à `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="f1c57-203">O código completo é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="f1c57-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="f1c57-204">Clique com o botão direito do mouse `AlbumSelectListViewModel`, selecione **resolver**e, em seguida, **usando MvcMusicStore. ViewModels;** .</span><span class="sxs-lookup"><span data-stu-id="f1c57-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="f1c57-205">Como alternativa, você pode adicionar a seguinte instrução Using:</span><span class="sxs-lookup"><span data-stu-id="f1c57-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="f1c57-206">Clique com o botão direito do mouse em `EditVM` e selecione **Adicionar exibição**.</span><span class="sxs-lookup"><span data-stu-id="f1c57-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="f1c57-207">Use as opções mostradas abaixo.</span><span class="sxs-lookup"><span data-stu-id="f1c57-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="f1c57-208">Selecione **Adicionar**e substitua o conteúdo do arquivo *Views\StoreManager\EditVM.cshtml* pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="f1c57-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="f1c57-209">A marcação de `EditVM` é muito semelhante à marcação de `Edit` original com as seguintes exceções.</span><span class="sxs-lookup"><span data-stu-id="f1c57-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="f1c57-210">As propriedades do modelo na exibição `Edit` são do formato `model.property`(por exemplo, `model.Title`).</span><span class="sxs-lookup"><span data-stu-id="f1c57-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="f1c57-211">As propriedades do modelo na exibição `EditVm` são do formato `model.Album.property`(por exemplo, `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="f1c57-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="f1c57-212">Isso ocorre porque a exibição de `EditVM` é passada para um contêiner de um `Album`, não uma `Album` como na exibição `Edit`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="f1c57-213">O segundo parâmetro **DropDownList** vem do modelo de exibição, não da **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="f1c57-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="f1c57-214">O auxiliar **BeginForm** na exibição de `EditVM` posta explicitamente de volta para o método de ação `Edit`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="f1c57-215">Ao fazer o postback para a ação de `Edit`, não precisamos escrever uma ação de `HTTP POST EditVM` e reutilizar a ação de `Edit` `HTTP POST`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="f1c57-216">Execute o aplicativo e edite um álbum.</span><span class="sxs-lookup"><span data-stu-id="f1c57-216">Run the application and edit an album.</span></span> <span data-ttu-id="f1c57-217">Altere a URL a ser usada `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="f1c57-218">Altere um campo e clique no botão **salvar** para verificar se o código está funcionando.</span><span class="sxs-lookup"><span data-stu-id="f1c57-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="f1c57-219">Qual abordagem você deve usar?</span><span class="sxs-lookup"><span data-stu-id="f1c57-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="f1c57-220">Todas as três abordagens mostradas são aceitáveis.</span><span class="sxs-lookup"><span data-stu-id="f1c57-220">All three approaches shown are acceptable.</span></span> <span data-ttu-id="f1c57-221">Muitos desenvolvedores preferem passar explicitamente o `SelectList` para o `DropDownList` usando o `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="f1c57-221">Many developers prefer to explicitly pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="f1c57-222">Essa abordagem tem a vantagem adicional de fornecer a você a flexibilidade de usar um nome mais apropriado para a coleção.</span><span class="sxs-lookup"><span data-stu-id="f1c57-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="f1c57-223">A única limitação é que você não pode nomear o objeto `ViewBag SelectList` mesmo nome que a propriedade do modelo.</span><span class="sxs-lookup"><span data-stu-id="f1c57-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="f1c57-224">Alguns desenvolvedores preferem a abordagem ViewModel.</span><span class="sxs-lookup"><span data-stu-id="f1c57-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="f1c57-225">Outras consideram a marcação mais detalhada e o HTML gerado da abordagem do ViewModel é uma desvantagem.</span><span class="sxs-lookup"><span data-stu-id="f1c57-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="f1c57-226">Nesta seção, aprendemos três abordagens ao uso de **DropDownList** com dados de categoria.</span><span class="sxs-lookup"><span data-stu-id="f1c57-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="f1c57-227">Na próxima seção, mostraremos como adicionar uma nova categoria.</span><span class="sxs-lookup"><span data-stu-id="f1c57-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f1c57-228">[Anterior](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Próximo](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="f1c57-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
