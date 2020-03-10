---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
title: Filtragem de mestre/detalhes com DropDownList (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, vemos como exibir relatórios mestre/detalhados em uma única página da Web usando DropDownLists para exibir os registros ' mestre ' e um DataList para exibição...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: ad0f1014-1eff-465f-bdc6-93058de00e44
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-with-a-dropdownlist-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 537f8e76bc0cbfa759a014b63ae5f68b5d3ca64d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78606417"
---
# <a name="masterdetail-filtering-with-a-dropdownlist-vb"></a>Filtragem mestre/detalhes com uma DropDownList (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_33_VB.exe) ou [baixar PDF](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/datatutorial33vb1.pdf)

> Neste tutorial, vemos como exibir relatórios mestre/detalhados em uma única página da Web usando DropDownLists para exibir os registros "mestre" e um DataList para exibir os "detalhes".

## <a name="introduction"></a>Introdução

O relatório mestre/detalhado, que criamos pela primeira vez usando um GridView na [filtragem de mestre/detalhes anterior com um tutorial DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) , começa mostrando um conjunto de registros "Master". O usuário pode fazer uma busca detalhada em um dos registros mestres, exibindo assim os "detalhes" do registro mestre. Os relatórios mestre/de detalhes são uma opção ideal para a visualização de relações um-para-muitos e para exibir informações detalhadas de tabelas "amplas" especialmente (aquelas que têm muitas colunas). Exploramos como implementar relatórios mestre/detalhados usando os controles GridView e DetailsView nos tutoriais anteriores. Neste tutorial e nos próximos dois, examinaremos esses conceitos, mas nos concentraremos em usar os controles DataList e Repeater em vez disso.

Neste tutorial, veremos como usar uma DropDownList para conter os registros "Master", com os registros "Details" exibidos em um DataList.

## <a name="step-1-adding-the-masterdetail-tutorial-web-pages"></a>Etapa 1: adicionando as páginas da Web do tutorial mestre/detalhes

Antes de iniciar este tutorial, vamos primeiro reservar um momento para adicionar as páginas de pasta e ASP.NET que precisaremos para este tutorial e os dois seguintes lidando com relatórios mestre/detalhados usando os controles DataList e Repeater. Comece criando uma nova pasta no projeto chamada `DataListRepeaterFiltering`. Em seguida, adicione as cinco páginas do ASP.NET a seguir a essa pasta, fazendo com que todas elas sejam configuradas para usar a página mestra `Site.master`:

- `Default.aspx`
- `FilterByDropDownList.aspx`
- `CategoryListMaster.aspx`
- `ProductsForCategoryDetails.aspx`
- `CategoriesAndProducts.aspx`

![Criar uma pasta DataListRepeaterFiltering e adicionar as páginas do tutorial ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image1.png)

**Figura 1**: criar uma pasta `DataListRepeaterFiltering` e adicionar as páginas do tutorial ASP.net

Em seguida, abra a página `Default.aspx` e arraste o `SectionLevelTutorialListing.ascx` controle de usuário da pasta `UserControls` para a superfície de design. Esse controle de usuário, que criamos nas [páginas mestras e](../introduction/master-pages-and-site-navigation-vb.md) no tutorial de navegação do site, enumera o mapa do site e exibe os tutoriais da seção atual em uma lista com marcadores.

[![adicionar o controle de usuário SectionLevelTutorialListing. ascx a default. aspx](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image3.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image2.png)

**Figura 2**: Adicionar o controle de usuário `SectionLevelTutorialListing.ascx` ao `Default.aspx` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image4.png))

Para que a lista com marcadores exiba os tutoriais mestre/de detalhes que criaremos, precisamos adicioná-los ao mapa do site. Abra o arquivo `Web.sitemap` e adicione a seguinte marcação após a marcação de nó do mapa do site "Exibindo dados com o DataList e o repetidor":

[!code-xml[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample1.xml)]

![Atualizar o mapa do site para incluir as novas páginas do ASP.NET](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image5.png)

**Figura 3**: atualizar o mapa do site para incluir as novas páginas do ASP.net

## <a name="step-2-displaying-the-categories-in-a-dropdownlist"></a>Etapa 2: exibindo as categorias em uma DropDownList

Nosso relatório mestre/detalhado listará as categorias em uma DropDownList, com os produtos do item de lista selecionados exibidos mais abaixo na página em um DataList. A primeira tarefa à frente de nós, então, é ter as categorias exibidas em uma DropDownList. Comece abrindo a página `FilterByDropDownList.aspx` na pasta `DataListRepeaterFiltering` e arraste uma DropDownList da caixa de ferramentas para o designer da página. Em seguida, defina a propriedade `ID` da DropDownList como `Categories`. Clique no link escolher fonte de dados na marca inteligente da DropDownList e crie um novo ObjectDataSource chamado `CategoriesDataSource`.

[![adicionar um novo ObjectDataSource chamado CategoriesDataSource](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image7.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image6.png)

**Figura 4**: adicionar um novo ObjectDataSource chamado `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image8.png))

Configure o novo ObjectDataSource de modo que ele invoque o método `GetCategories()` da classe `CategoriesBLL`. Depois de configurar o ObjectDataSource, ainda precisamos especificar qual campo de fonte de dados deve ser exibido na DropDownList e qual deles deve ser associado como o valor para cada item de lista. Tenha o campo `CategoryName` como a exibição e `CategoryID` como o valor para cada item de lista.

[![fazer com que a DropDownList exiba o campo CategoryName e use CategoryID como o valor](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image10.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image9.png)

**Figura 5**: fazer com que a DropDownList exiba o campo `CategoryName` e use `CategoryID` como o valor ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image11.png))

Neste ponto, temos um controle DropDownList que é preenchido com os registros da tabela `Categories` (todos são realizados em cerca de seis segundos). A Figura 6 mostra nosso progresso até o momento, quando visualizado por meio de um navegador.

[![um menu suspenso lista as categorias atuais](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image13.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image12.png)

**Figura 6**: um menu suspenso lista as categorias atuais ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image14.png))

## <a name="step-2-adding-the-products-datalist"></a>Etapa 2: adicionando os produtos DataList

A última etapa em nosso relatório mestre/detalhado é listar os produtos associados à categoria selecionada. Para fazer isso, adicione um DataList à página e crie um novo ObjectDataSource chamado `ProductsByCategoryDataSource`. Faça com que o controle de `ProductsByCategoryDataSource` recupere seus dados do método de `GetProductsByCategoryID(categoryID)` da classe `ProductsBLL`. Como este relatório mestre/detalhado é somente leitura, escolha a opção (nenhum) nas guias inserir, atualizar e excluir.

[![selecionar o método GetProductsByCategoryID (categoryID)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image16.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image15.png)

**Figura 7**: selecione o método de `GetProductsByCategoryID(categoryID)` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image17.png))

Depois de clicar em avançar, o assistente ObjectDataSource nos solicitará a fonte do valor do parâmetro *`categoryID`* do método de `GetProductsByCategoryID(categoryID)`. Para usar o valor do item `categories` DropDownList selecionado, defina a origem do parâmetro como Control e a ControlID como `Categories`.

[![definir o parâmetro categoryID como o valor das categorias DropDownList](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image19.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image18.png)

**Figura 8**: definir o parâmetro *`categoryID`* para o valor da `Categories` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image20.png))

Após a conclusão do assistente para configurar fonte de dados, o Visual Studio gerará automaticamente um `ItemTemplate` para o DataList que exibe o nome e o valor de cada campo de dados. Vamos aprimorar o DataList para usar um `ItemTemplate` que exibe apenas o nome, a categoria, o fornecedor, a quantidade por unidade e o preço do produto, juntamente com um `SeparatorTemplate` que injeta um elemento `<hr>` entre cada item. Vou usar o `ItemTemplate` de um exemplo no tutorial [exibindo dados com os controles DataList e Repeater](../displaying-data-with-the-datalist-and-repeater/displaying-data-with-the-datalist-and-repeater-controls-vb.md) , mas sinta-se à vontade para usar qualquer marcação de modelo que você achar mais visualmente atraente.

Depois de fazer essas alterações, o DataList e a marcação de ObjectDataSource devem ser semelhantes ao seguinte:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample2.aspx)]

Reserve um tempo para conferir nosso progresso em um navegador. Ao visitar a página pela primeira vez, os produtos que pertencem à categoria selecionada (bebidas) são exibidos (como mostra a Figura 9), mas a alteração da DropDownList não atualiza os dados. Isso ocorre porque um postback deve ocorrer para que o DataList seja atualizado. Para fazer isso, podemos definir a propriedade `AutoPostBack` da DropDownList como `true` ou adicionar um controle de botão da Web à página. Para este tutorial, optei por definir a propriedade de `AutoPostBack` da DropDownList como `true`.

As figuras 9 e 10 ilustram o relatório mestre/detalhado em ação.

[![ao visitar a página pela primeira vez, os produtos de bebidas são exibidos](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image22.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image21.png)

**Figura 9**: ao visitar a página pela primeira vez, os produtos de bebidas são exibidos ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image23.png))

[![selecionar um novo produto (produzir) gera automaticamente um PostBack, atualizando o DataList](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image25.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image24.png)

**Figura 10**: selecionar um novo produto (produzir) gera automaticamente um postback, atualizando o DataList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image26.png))

## <a name="adding-a----choose-a-category----list-item"></a>Adicionando um item de lista "--escolher uma categoria--"

Ao visitar pela primeira vez o `FilterByDropDownList.aspx` página, o primeiro item de lista da DropDownList de categorias (bebidas) é selecionado por padrão, mostrando os produtos de bebidas no DataList. Na *filtragem de mestre/detalhes com um tutorial DropDownList* , adicionamos uma opção "--escolher uma categoria--" para a DropDownList que foi selecionada por padrão e, quando selecionado, exibia *todos* os produtos no banco de dados. Essa abordagem era gerenciável ao listar os produtos em um GridView, uma vez que cada linha de produto ocupava uma pequena quantidade de espaço real da tela. No entanto, com o DataList, as informações de cada produto consomem uma parte muito maior da tela. Vamos ainda adicionar uma opção "--escolher uma categoria--" e tê-la selecionada por padrão, mas em vez de fazer com que ela mostre todos os produtos quando selecionado, vamos configurá-lo para que ele não mostre nenhum produto.

Para adicionar um novo item de lista à DropDownList, vá para a janela Propriedades e clique nas reticências na propriedade `Items`. Adicione um novo item de lista com o `Text` "--escolha uma categoria--" e o `Value` `0`.

![Adicionar](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image27.png)

**Figura 11**: adicionar um item de lista "--escolher uma categoria--"

Como alternativa, você pode adicionar o item de lista adicionando a seguinte marcação ao DropDownList:

[!code-aspx[Main](master-detail-filtering-with-a-dropdownlist-datalist-vb/samples/sample3.aspx)]

Além disso, precisamos definir o `AppendDataBoundItems` do controle DropDownList como `true` porque, se ele estiver definido como `false` (o padrão), quando as categorias estiverem associadas à DropDownList do ObjectDataSource, eles substituirão quaisquer itens de lista adicionados manualmente.

![Defina a propriedade AppendDataBoundItems como true](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image28.png)

**Figura 12**: definir a propriedade `AppendDataBoundItems` como true

O motivo pelo qual escolhemos o valor `0` para o item de lista "--escolha uma categoria--" é porque não há nenhuma categoria no sistema com um valor de `0`, portanto nenhum registro de produto será retornado quando o item de lista "--escolher uma categoria--" for selecionado. Para confirmar isso, Reserve um instante para visitar a página por meio de um navegador. Como a Figura 13 mostra, ao exibir inicialmente a página, o item de lista "--escolher uma categoria--" é selecionado e nenhum produto é exibido.

[![quando o](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image30.png)](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image29.png)

**Figura 13**: quando o item de lista "--escolher uma categoria--" está selecionado, nenhum produto é exibido ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-a-dropdownlist-datalist-vb/_static/image31.png))

Se você preferir exibir *todos* os produtos quando a opção "--escolher uma categoria--" estiver selecionada, use um valor de `-1` em vez disso. O leitor do astuto se lembrará de volta na *filtragem mestre/de detalhes com um tutorial DropDownList* atualizamos o método `GetProductsByCategoryID(categoryID)` da classe `ProductsBLL` de forma que, se um valor *`categoryID`* de `-1` fosse passado, todos os registros de produto foram retornados.

## <a name="summary"></a>Resumo

Ao exibir dados relacionados hierarquicamente, ele geralmente ajuda a apresentar os dados usando relatórios mestre/detalhados, dos quais o usuário pode começar a usar os dados da parte superior da hierarquia e fazer uma busca detalhada nos detalhes. Neste tutorial, examinamos a criação de um relatório mestre/detalhado simples que mostra os produtos de uma categoria selecionada. Isso foi feito usando uma DropDownList para a lista de categorias e um DataList para os produtos que pertencem à categoria selecionada.

No próximo tutorial, veremos a separação dos registros mestre e de detalhes em duas páginas. Na primeira página, uma lista de registros "mestre" será exibida, com um link para exibir os detalhes. Clicar no link irá encaixar o usuário para a segunda página, que exibirá os detalhes do registro mestre selecionado.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Randy Schmidt. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
> [Próximo](master-detail-filtering-acess-two-pages-datalist-vb.md)
