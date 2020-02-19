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
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455718"
---
# <a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Adicionar uma nova categoria ao DropDownList usando o jQuery UI

por [Rick Anderson](https://twitter.com/RickAndMSFT)

A marca HTML `Select` é ideal para apresentar uma lista de dados de categoria fixas, mas muitas vezes você precisa adicionar uma nova categoria. Suponha que queremos adicionar o gênero "Opera" às categorias em nosso banco de dados? Nesta seção, usaremos a interface do usuário do jQuery para adicionar uma caixa de diálogo que podemos usar para adicionar uma nova categoria. A imagem abaixo mostra como a interface do usuário estará presente no navegador.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Quando um usuário seleciona o link **Adicionar novo gênero** , uma caixa de diálogo pop-up solicita ao usuário um novo nome de gênero (e, opcionalmente, uma descrição). A imagem abaixo mostra a caixa de diálogo Adicionar pop-up de **gênero** .

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Quando um novo nome de gênero é inserido e o botão **salvar** é enviado por push, acontece o seguinte:

1. Uma chamada AJAX posta os dados no método Create do controlador gênero, que salva o novo gênero no banco de dados e retorna as novas informações de gênero (nome e ID do gênero) como JSON.
2. O JavaScript adiciona os novos dados de gênero à lista de seleção.
3. O JavaScript torna o novo gênero o item selecionado.

   Na imagem abaixo, o **Opera** foi adicionado ao banco de dados e selecionado na lista suspensa **gênero** . 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Abra o arquivo *Views\StoreManager\Create.cshtml* e substitua a marcação de gênero pelo seguinte código:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

A exibição parcial `_ChooseGenre` conterá toda a lógica para conectar o JavaScript e o jQuery usados para implementar o recurso Adicionar novo gênero. Depois de concluirmos o código, será simples fazer o mesmo com a interface do usuário do artista.

Em Gerenciador de Soluções, clique com o botão direito do mouse na pasta *Views\StoreManager* e selecione **Adicionar**e, em seguida, **Exibir**. Na entrada **nome da exibição** , digite `_ChooseGenre`, em seguida, selecione **Adicionar**. Substitua a marcação no arquivo *Views\StoreManager\\_ChooseGenre. cshtml* pelo seguinte:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

A primeira linha declara que estamos passando um `Album` como nosso modelo, exatamente a mesma instrução Model encontrada na exibição Create. As próximas linhas são a marcação auxiliar de **rótulo** . A próxima linha é a chamada do auxiliar **DropDownList** , exatamente igual à exibição Create original. A próxima linha adiciona um link com o nome `Add New Genre`e o estilo como um botão. A linha que contém `ValidationMessageFor` é copiada diretamente da exibição Create. As seguintes linhas:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

Cria uma div oculta, com a ID de `genreDialog`. Usaremos o jQuery para conectar nossa caixa de diálogo **Adicionar gênero** com a ID `genreDialog` nesta div. As duas últimas marcas de script contêm links para os arquivos JavaScript que usaremos para implementar o recurso Adicionar novo gênero. O arquivo */scripts/chooseGenre.js* é fornecido para você no projeto, vamos examiná-lo posteriormente no tutorial.

Execute o aplicativo e clique no botão **Adicionar novo gênero** . Na caixa de diálogo **Adicionar gênero** , digite **Opera** na caixa de entrada **nome** .

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Clique no botão **Salvar**. Uma chamada AJAX cria a categoria Opera e, em seguida, popula a lista suspensa com Opera e define Opera como o gênero selecionado.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Insira um artista, título e preço e, em seguida, selecione o botão **criar** . Se você inserir um preço menor que $8.99, o novo álbum aparecerá na parte superior da exibição do índice. Verifique se a nova entrada de álbum foi salva no banco de dados.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Tente criar um novo gênero com apenas uma letra. O código a seguir no arquivo *Models\Genre.cs* define o comprimento mínimo e máximo do nome do gênero.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Relatórios de validação do lado do cliente você deve inserir uma cadeia de caracteres entre 2 e 20 caracteres.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Examinando como um novo gênero é adicionado ao banco de dados e à lista de seleção.

Abra o arquivo *Scripts\chooseGenre.js* e examine o código.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

A segunda linha usa a ID `genreDialog` para criar uma caixa de diálogo na marca DIV no arquivo *Views\StoreManager\\_ChooseGenre. cshtml* . A maioria dos parâmetros nomeados é auto-explicativa. O parâmetro `autoOpen` é definido como false, a seleção do botão **criar gênero** abrirá a caixa de diálogo explicitamente (isso é descrito por último em). A caixa de diálogo tem dois botões, **salvar** e **Cancelar**. O botão **Cancelar** fecha a caixa de diálogo. O código a seguir mostra a função de botão **salvar** .

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

O `var createGenreForm` é selecionado na ID de `createGenreForm`. A ID de `createGenreForm` foi definida no código a seguir encontrado no arquivo *Views\Genre\\_CreateGenre. cshtml* .

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

A sobrecarga auxiliar [HTML. BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) usada no arquivo *Views\Genre\\_CreateGenre. cshtml* gera HTML com um atributo action contendo a URL para enviar o formulário. Você pode ver isso exibindo a página Criar álbum em um navegador e selecionando Mostrar fonte no navegador. A marcação a seguir mostra o HTML gerado que contém a marca de formulário.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

A linha jQuery `$.post` faz uma chamada AJAX para o atributo Action (`/StoreManager/Create`) e passa os dados da caixa de diálogo **criar gênero** . Os dados consistem no nome do novo gênero e uma descrição opcional. Se a chamada AJAX for bem-sucedida, o novo nome e valor do gênero serão adicionados à marcação Select e o novo gênero será definido como o valor selecionado. Como essa é uma marcação gerada dinamicamente, você não pode ver a nova opção Select exibindo a origem no navegador. Você pode ver o novo HTML com as ferramentas de desenvolvedor do IE 9 F12. Para exibir a nova opção de seleção, no Internet Explorer 9, pressione a tecla F12 para iniciar as ferramentas de desenvolvedor F12. Navegue até a página criar e adicione um novo gênero para que o novo gênero seja selecionado na lista de seleção de gênero. Nas ferramentas de desenvolvedor F12:

1. Selecione a guia HTML.
2. Pressione o ícone de atualização.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. Na caixa de pesquisa, insira Gêneroid.
4. Usando o ícone seguinte,   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   Navegue até a seguinte marca de seleção:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Expanda o último valor de opção.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

O código a seguir no arquivo *Scripts\chooseGenre.js* mostra como o botão **Adicionar novo gênero** é conectado ao evento de clique e como a caixa de diálogo **Adicionar novo gênero** é criada.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

A primeira linha cria uma função de clique anexada ao botão **Adicionar novo gênero** . A marcação a seguir do arquivo Views\StoreManager\\_ChooseGenre. cshtml mostra como o botão **Adicionar novo gênero** é criado:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

O método Load cria e abre a caixa de diálogo Adicionar gênero e chama o método jQuery `parse` para que a validação do cliente ocorra nos dados inseridos na caixa de diálogo.

Nesta seção, você aprendeu a criar uma caixa de diálogo que pode ser usada para adicionar novos dados de categoria a uma lista de seleção. Você pode seguir o mesmo procedimento para criar a interface do usuário para adicionar um novo artista à lista de seleção do artista. Este tutorial forneceu uma visão geral de como trabalhar com **o auxiliar HTML**do MVC do ASP.net. Para obter informações adicionais sobre como trabalhar com **DropDownList**, consulte a seção de referências de adição abaixo. Informe-nos se este tutorial foi útil.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Referências adicionais

- [ASP.NET MVC – tutorial de listas suspensas em cascata](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) por [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Escolhido](https://harvesthq.github.com/chosen/) Um plug-in de JavaScript que dá suporte a seleção múltipla e filtragem.

### <a name="contributors"></a>Colaboradores

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Revisores

- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike Papa
- Tom Dykstra

> [!div class="step-by-step"]
> [Anterior](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
