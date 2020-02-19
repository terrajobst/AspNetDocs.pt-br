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
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457603"
---
# <a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Examinar como o ASP.NET MVC faz scaffold do auxiliar DropDownList

por [Rick Anderson](https://twitter.com/RickAndMSFT)

Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *controladores* e selecione **Adicionar controlador**. Nomeie o controlador **StoreManagerController**. Defina as opções para a caixa de diálogo **Adicionar controlador** , conforme mostrado na imagem abaixo.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Edite a exibição *StoreManager\Index.cshtml* e remova `AlbumArtUrl`. Remover `AlbumArtUrl` tornará a apresentação mais legível. O código completo é mostrado abaixo.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Abra o arquivo *Controllers\StoreManagerController.cs* e localize o método `Index`. Adicione a cláusula `OrderBy` para que os álbuns sejam classificados por preço. O código completo é mostrado abaixo.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Classificar por preço tornará mais fácil testar as alterações no banco de dados. Ao testar os métodos editar e criar, você pode usar um preço baixo para que os dados salvos sejam exibidos primeiro.

Abra o arquivo *StoreManager\Edit.cshtml* . Adicione a linha a seguir logo após a marca de legenda.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

O código a seguir mostra o contexto dessa alteração:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

O `AlbumId` é necessário para fazer alterações em um registro de álbum.

Pressione CTRL+F5 para executar o aplicativo. Selecione para o link de **administrador** e, em seguida, selecione o link **criar novo** para criar um novo álbum. Verifique se as informações do álbum foram salvas. Edite um álbum e verifique se as alterações feitas foram persistidas.

### <a name="the-album-schema"></a>O esquema do álbum

O controlador de `StoreManager` criado pelo mecanismo scaffolding do MVC permite o acesso CRUD (criar, ler, atualizar, excluir) aos álbuns no banco de dados da loja de música. O esquema para informações de álbum é mostrado abaixo:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

A tabela `Albums` não armazena o gênero e a descrição do álbum, armazena uma chave estrangeira na tabela `Genres`. A tabela `Genres` contém o nome do gênero e a descrição. Da mesma forma, a tabela `Albums` não contém o nome dos artistas do álbum, mas uma chave estrangeira para a tabela `Artists`. A tabela `Artists` contém o nome do artista. Se você examinar os dados na tabela `Albums`, poderá ver que cada linha contém uma chave estrangeira para a tabela `Genres` e uma chave estrangeira para a tabela `Artists`. A imagem abaixo mostra alguns dados de tabela da tabela `Albums`.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>A marca HTML Select

O elemento HTML `<select>` (criado pelo auxiliar HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) ) é usado para exibir uma lista completa de valores (como a lista de gêneros). Para editar formulários, quando o valor atual é conhecido, a lista de seleção pode exibir o valor atual. Vimos isso anteriormente quando definimos o valor selecionado como **comédia**. A lista de seleção é ideal para exibir dados de categoria ou de chave estrangeira. O elemento `<select>` para a chave estrangeira do gênero exibe a lista de possíveis nomes de gênero, mas quando você salva o formulário, a propriedade gênero é atualizada com o valor de chave estrangeira do gênero, não o nome do gênero exibido. Na imagem abaixo, o gênero selecionado é o **disco** e o artista é o **Verão de Donna**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Examinando o código com Scaffold do ASP.NET MVC

Abra o arquivo *Controllers\StoreManagerController.cs* e localize o método `HTTP GET Create`.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

O método `Create` adiciona dois objetos [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) à `ViewBag`, um para conter as informações de gênero e um para conter as informações do artista. A sobrecarga do construtor [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) usada acima usa três argumentos:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *itens*: um [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) contendo os itens na lista. No exemplo acima, a lista de gêneros retornada por `db.Genres`.
2. *DataValueField*: o nome da propriedade na lista **IEnumerable** que contém o valor da chave. No exemplo acima, `GenreId` e `ArtistId`.
3. *DataTextField*: o nome da propriedade na lista **IEnumerable** que contém as informações a serem exibidas. Na tabela artistas e gênero, o campo `name` é usado.

Abra o arquivo *Views\StoreManager\Create.cshtml* e examine a marcação auxiliar `Html.DropDownList` para o campo gênero.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

A primeira linha mostra que o modo de exibição de criação usa um modelo de `Album`. No método `Create` mostrado acima, nenhum modelo foi passado, portanto, a exibição Obtém um modelo de `Album` **nulo** . Neste ponto, estamos criando um novo álbum para que não tenhamos dados de `Album` para ele.

A sobrecarga de [HTML. DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) mostrada acima usa o nome do campo para associar ao modelo. Ele também usa esse nome para procurar um objeto **ViewBag** contendo um objeto [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) . Usando essa sobrecarga, é necessário nomear o objeto de lista de **seleção ViewBag** `GenreId`. O segundo parâmetro (`String.Empty`) é o texto a ser exibido quando nenhum item é selecionado. Isso é exatamente o que queremos ao criar um novo álbum. Se você removeu o segundo parâmetro e usou o seguinte código:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

A lista de seleção teria como padrão o primeiro elemento ou uma pedra em nosso exemplo.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Examinando o método `HTTP POST Create`.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Essa sobrecarga do método `Create` usa um objeto `album`, criado pelo sistema de associação de modelo MVC ASP.NET dos valores de formulário postados. Quando você enviar um novo álbum, se o estado do modelo for válido e não houver erros de banco de dados, o novo álbum será adicionado ao banco de dados. A imagem a seguir mostra a criação de um novo álbum.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Você pode usar a [ferramenta Fiddler](http://www.fiddler2.com/fiddler2/) para examinar os valores de formulário postados que a associação de modelo MVC ASP.NET usa para criar o objeto de álbum.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Refatorando a criação de seleção de ViewBag

Os métodos `Edit` e o método `HTTP POST Create` têm um código idêntico para configurar alist de **seleção** em **ViewBag**. No espírito de [seca](http://en.wikipedia.org/wiki/Don't_repeat_yourself), iremos refatorar esse código. Vamos usar esse código refatorado posteriormente.

Crie um novo método para adicionar uma **seleção** de gênero e artista à **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Substitua as duas linhas definindo as `ViewBag` em cada um dos métodos `Create` e `Edit` com uma chamada para o método `SetGenreArtistViewBag`. O código completo é mostrado abaixo.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Crie um novo álbum e edite um álbum para verificar se as alterações funcionam.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Passando explicitamente alist de seleção para DropDownList

As exibições criar e editar criadas pelo ASP.NET MVC scaffolding usam a seguinte sobrecarga de **DropDownList** :

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

A marcação de `DropDownList` para a exibição criar é mostrada abaixo.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Como a propriedade `ViewBag` para o `SelectList` é denominada `GenreId`, o auxiliar **DropDownList** usará a `GenreId`**SelectList** em **ViewBag**. Na seguinte sobrecarga de **DropDownList** , a `SelectList` é passada explicitamente.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Abra o arquivo *Views\StoreManager\Edit.cshtml* e altere a chamada **DropDownList** para explicitamente passar nalist de **seleção**, usando a sobrecarga acima. Faça isso para a categoria de gênero. O código completo é mostrado abaixo:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Execute o aplicativo e clique no link **administrador** , navegue até um álbum de jazz e selecione o link **Editar** .

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

Em vez de mostrar jazz como o gênero selecionado no momento, a pedra é exibida. Quando o argumento da cadeia de caracteres (a propriedade a ser ligada) e o objeto **SelectList** tiverem o mesmo nome, o valor selecionado não será usado. Quando não há nenhum valor selecionado fornecido, os navegadores assumem como padrão o primeiro elemento nalist de **seleção**(que é a **pedra** no exemplo acima). Essa é uma limitação conhecida do auxiliar **DropDownList** .

Abra o arquivo *Controllers\StoreManagerController.cs* e altere os nomes de objeto daList de **seleção** para `Genres` e `Artists`. O código completo é mostrado abaixo:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Os nomes de gêneros e artistas são nomes melhores para as categorias, pois eles contêm mais do que apenas a ID de cada categoria. A refatoração que fizemos anteriormente pagou. Em vez de alterar **ViewBag** em quatro métodos, nossas alterações foram isoladas para o método `SetGenreArtistViewBag`.

Altere a chamada **DropDownList** nas exibições criar e editar para usar os novos nomes de **selecionarlist** . A nova marcação para o modo de exibição de edição é mostrada abaixo:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

A exibição Create requer uma cadeia de caracteres vazia para impedir que o primeiro item na SelectList seja exibido.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Crie um novo álbum e edite um álbum para verificar se as alterações funcionam. Teste o código de edição selecionando um álbum com um gênero diferente de rock.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Usando um modelo de exibição com o auxiliar DropDownList

Crie uma nova classe na pasta ViewModels chamada `AlbumSelectListViewModel`. Substitua o código na classe `AlbumSelectListViewModel` pelo seguinte:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

O construtor de `AlbumSelectListViewModel` usa um álbum, uma lista de artistas e gêneros e cria um objeto que contém o álbum e um `SelectList` para gêneros e artistas.

Compile o projeto para que o `AlbumSelectListViewModel` esteja disponível quando criarmos uma exibição na próxima etapa.

Adicione um método `EditVM` à `StoreManagerController`. O código completo é mostrado abaixo.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Clique com o botão direito do mouse `AlbumSelectListViewModel`, selecione **resolver**e, em seguida, **usando MvcMusicStore. ViewModels;** .

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Como alternativa, você pode adicionar a seguinte instrução Using:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Clique com o botão direito do mouse em `EditVM` e selecione **Adicionar exibição**. Use as opções mostradas abaixo.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Selecione **Adicionar**e substitua o conteúdo do arquivo *Views\StoreManager\EditVM.cshtml* pelo seguinte:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

A marcação de `EditVM` é muito semelhante à marcação de `Edit` original com as seguintes exceções.

- As propriedades do modelo na exibição `Edit` são do formato `model.property`(por exemplo, `model.Title`). As propriedades do modelo na exibição `EditVm` são do formato `model.Album.property`(por exemplo, `model.Album.Title`). Isso ocorre porque a exibição de `EditVM` é passada para um contêiner de um `Album`, não uma `Album` como na exibição `Edit`.
- O segundo parâmetro **DropDownList** vem do modelo de exibição, não da **ViewBag**.
- O auxiliar **BeginForm** na exibição de `EditVM` posta explicitamente de volta para o método de ação `Edit`. Ao fazer o postback para a ação de `Edit`, não precisamos escrever uma ação de `HTTP POST EditVM` e reutilizar a ação de `Edit` `HTTP POST`.

Execute o aplicativo e edite um álbum. Altere a URL a ser usada `EditVM`. Altere um campo e clique no botão **salvar** para verificar se o código está funcionando.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>Qual abordagem você deve usar?

Todas as três abordagens mostradas são aceitáveis. Muitos desenvolvedores preferem passar explicitamente o `SelectList` para o `DropDownList` usando o `ViewBag`. Essa abordagem tem a vantagem adicional de fornecer a você a flexibilidade de usar um nome mais apropriado para a coleção. A única limitação é que você não pode nomear o objeto `ViewBag SelectList` mesmo nome que a propriedade do modelo.

Alguns desenvolvedores preferem a abordagem ViewModel. Outras consideram a marcação mais detalhada e o HTML gerado da abordagem do ViewModel é uma desvantagem.

Nesta seção, aprendemos três abordagens ao uso de **DropDownList** com dados de categoria. Na próxima seção, mostraremos como adicionar uma nova categoria.

> [!div class="step-by-step"]
> [Anterior](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Próximo](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
