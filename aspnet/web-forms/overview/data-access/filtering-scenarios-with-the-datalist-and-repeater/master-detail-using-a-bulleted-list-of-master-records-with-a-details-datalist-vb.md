---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
title: Mestre/detalhes usando uma lista com marcadores de registros mestres com um DataList (VB) de detalhes | Microsoft Docs
author: rick-anderson
description: Neste tutorial, compactaremos o relatório mestre/detalhes de duas páginas do tutorial anterior em uma única página, mostrando uma lista com marcadores de nomes de categoria em t...
ms.author: riande
ms.date: 10/17/2006
ms.assetid: ee20742f-6fb7-49a0-a009-058fe363aacb
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 81d72c666925e89729464e7ea696bde8d323e277
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74641672"
---
# <a name="masterdetail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb"></a>Mestre/detalhes usando uma lista com marcadores de registros mestre com um DataList de detalhes (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_35_VB.exe) ou [baixar PDF](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/datatutorial35vb1.pdf)

> Neste tutorial, compactaremos o relatório mestre/detalhes de duas páginas do tutorial anterior em uma única página, mostrando uma lista com marcadores de nomes de categoria no lado esquerdo da tela e os produtos da categoria selecionada à direita da tela.

## <a name="introduction"></a>Introdução

No [tutorial anterior](master-detail-filtering-acess-two-pages-datalist-vb.md) , examinamos como separar um relatório mestre/detalhado em duas páginas. Na página mestra, usamos um controle Repeater para renderizar uma lista de categorias com marcadores. Cada nome de categoria era um hiperlink que, quando clicado, levava o usuário para a página de detalhes, em que um DataList de duas colunas mostrou esses produtos que pertencem à categoria selecionada.

Neste tutorial, compactaremos o tutorial de duas páginas em uma única página, mostrando uma lista com marcadores de nomes de categoria no lado esquerdo da tela com cada nome de categoria renderizado como um LinkButton. Clicar em um dos nomes de categoria LinkButtons induzi um postback e associa os produtos da categoria selecionada a um DataList de duas colunas à direita da tela. Além de exibir o nome de cada categoria, o repetidor à esquerda mostra quantos produtos totais existem para uma determinada categoria (consulte a Figura 1).

[![o nome da categoria e o número total de produtos são exibidos à esquerda](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image2.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image1.png)

**Figura 1**: o nome da categoria e o número total de produtos são exibidos à esquerda ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image3.png))

## <a name="step-1-displaying-a-repeater-in-the-left-portion-of-the-screen"></a>Etapa 1: exibindo um repetidor na parte esquerda da tela

Para este tutorial, precisamos ter a lista de categorias com marcadores à esquerda dos produtos da categoria selecionada. O conteúdo em uma página da Web pode ser posicionado usando marcas de parágrafo de elementos HTML padrão, espaços sem quebra, `<table>` s e assim por diante ou por meio de técnicas de CSS (folha de estilos em cascata). Todos os nossos tutoriais, até agora, usaram as técnicas de CSS para posicionamento. Quando criamos a interface do usuário de navegação em nossa página mestra nas [páginas mestras e](../introduction/master-pages-and-site-navigation-vb.md) no tutorial de navegação do site, usamos o *posicionamento absoluto*, indicando o deslocamento de pixel preciso para a lista de navegação e o conteúdo principal. Como alternativa, o CSS pode ser usado para posicionar um elemento à direita ou à esquerda de outro por meio de *flutuante*. Podemos fazer com que a lista de categorias com marcadores apareça à esquerda dos produtos da categoria selecionada, flutuando o repetidor à esquerda do DataList

Abra a página `CategoriesAndProducts.aspx` da pasta `DataListRepeaterFiltering` e adicione à página um repetidor e um DataList. Defina o `ID` do repetidor como `Categories` e os s DataList para `CategoryProducts`. Vá para a exibição de código-fonte e coloque os controles Repeater e DataList em seus próprios elementos de `<div>`. Ou seja, coloque o repetidor dentro de um elemento `<div>` primeiro e, em seguida, o DataList em seu próprio elemento `<div>` diretamente após o repetidor. Sua marcação neste ponto deve ser semelhante ao seguinte:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample1.aspx)]

Para flutuar o repetidor à esquerda do DataList, precisamos usar o atributo estilo CSS `float`, da seguinte forma:

[!code-html[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample2.html)]

O `float: left;` flutua o primeiro elemento `<div>` à esquerda do segundo. As configurações de `width` e `padding-right` indicam a primeira `<div>` s `width` e a quantidade de preenchimento que é adicionada entre o conteúdo do elemento `<div>` e sua margem direita. Para obter mais informações sobre elementos flutuantes em CSS, confira [Floatutorial](http://css.maxdesign.com.au/floatutorial/).

Em vez de especificar a configuração de estilo diretamente por meio do primeiro atributo de `style` do elemento `<p>`, em vez disso, vamos criar uma nova classe CSS em `Styles.css` chamada `FloatLeft`:

[!code-css[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample3.css)]

Em seguida, podemos substituir o `<div>` por `<div class="FloatLeft">`.

Depois de adicionar a classe CSS e configurar a marcação na página `CategoriesAndProducts.aspx`, vá para o designer. Você deve ver o repetidor flutuante à esquerda do DataList (embora, no momento, ambos apareçam apenas como caixas cinzas, já que ainda estamos Configurando suas fontes de dados ou modelos).

[![o repetidor é flutuado à esquerda do DataList](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image5.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image4.png)

**Figura 2**: o repetidor é flutuado à esquerda do DataList ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image6.png))

## <a name="step-2-determining-the-number-of-products-for-each-category"></a>Etapa 2: determinando o número de produtos para cada categoria

Com o repetidor e o DataList s ao redor da marcação concluída, estamos prontos para associar os dados da categoria ao controle Repeater. No entanto, como a lista com marcadores de categorias na Figura 1 mostra, além de cada categoria, o nome também precisa exibir o número de produtos associados à categoria. Para acessar essas informações, também podemos:

- **Determine essas informações a partir da classe code-behind da página ASP.NET s.** Dado um determinado *`categoryID`* podemos determinar o número de produtos associados chamando o método `ProductsBLL` class s `GetProductsByCategoryID(categoryID)`. Esse método retorna um objeto `ProductsDataTable` cuja propriedade `Count` indica quantos `ProductsRow` s existe, que é o número de produtos para o *`categoryID`* especificado. Podemos criar um manipulador de eventos `ItemDataBound` para o repetidor que, para cada categoria associada ao repetidor, chama o método de `GetProductsByCategoryID(categoryID)` da classe `ProductsBLL` e inclui sua contagem na saída.
- **Atualize o `CategoriesDataTable` no DataSet tipado para incluir uma coluna `NumberOfProducts`.** Em seguida, podemos atualizar o método `GetCategories()` no `CategoriesDataTable` para incluir essas informações ou, como alternativa, deixar o `GetCategories()` como está e criar um novo método `CategoriesDataTable` chamado `GetCategoriesAndNumberOfProducts()`.

Vamos explorar essas duas técnicas. A primeira abordagem é mais simples de implementar, já que não precisamos atualizar a camada de acesso a dados; no entanto, ele requer mais comunicações com o banco de dados. A chamada para o método de `GetProductsByCategoryID(categoryID)` da classe `ProductsBLL` no manipulador de eventos `ItemDataBound` adiciona uma chamada de banco de dados extra para cada categoria exibida no repetidor. Com essa técnica, há *n* + 1 chamadas de banco de dados, em que *N* é o número de categorias exibidas no repetidor. Com a segunda abordagem, a contagem de produtos é retornada com informações sobre cada categoria do método `CategoriesBLL` `GetCategories()` de classe (ou `GetCategoriesAndNumberOfProducts()`), resultando assim em apenas uma viagem ao banco de dados.

## <a name="determining-the-number-of-products-in-the-itemdatabound-event-handler"></a>Determinando o número de produtos no manipulador de eventos de Hiperligação

A determinação do número de produtos para cada categoria no manipulador de eventos repetidos `ItemDataBound` não exige nenhuma modificação em nossa camada de acesso a dados existente. Todas as modificações podem ser feitas diretamente na página de `CategoriesAndProducts.aspx`. Comece adicionando um novo ObjectDataSource chamado `CategoriesDataSource` por meio da marca inteligente s do repetidor. Em seguida, configure o `CategoriesDataSource` ObjectDataSource para que ele recupere seus dados do método `CategoriesBLL` Class s `GetCategories()`.

[![configurar o ObjectDataSource para usar o método CategoriesBLL da classe s GetCategories ()](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image8.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image7.png)

**Figura 3**: configurar o ObjectDataSource para usar o método de `GetCategories()` da classe `CategoriesBLL` ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image9.png))

Cada item no repetidor de `Categories` precisa ser clicável e, quando clicado, faz com que o `CategoryProducts` DataList exiba esses produtos para a categoria selecionada. Isso pode ser feito ao tornar cada categoria um hiperlink, vinculando-se novamente a essa mesma página (`CategoriesAndProducts.aspx`), mas passando o `CategoryID` por meio da QueryString, da mesma forma que vimos no tutorial anterior. A vantagem dessa abordagem é que uma página que exibe os produtos de uma categoria específica pode ser marcada e indexada por um mecanismo de pesquisa.

Como alternativa, podemos tornar cada categoria um LinkButton, que é a abordagem que usaremos para este tutorial. O LinkButton é renderizado no navegador do usuário como um hiperlink, mas, quando clicado, induzi um postback; no postback, o DataList s ObjectDataSource precisa ser atualizado para exibir os produtos que pertencem à categoria selecionada. Para este tutorial, usar um hiperlink faz mais sentido do que usar um LinkButton; no entanto, pode haver outros cenários em que o uso de um LinkButton é mais vantajoso. Embora a abordagem de hiperlink seja ideal para este exemplo, vamos, em vez disso, explorar usando o LinkButton. Como veremos, o uso de um LinkButton apresenta alguns desafios que, caso contrário, não surgiria com um hiperlink. Portanto, o uso de um LinkButton neste tutorial destacará esses desafios e ajudará a fornecer soluções para os cenários em que poderemos usar um LinkButton em vez de um hiperlink.

> [!NOTE]
> Você é incentivado a repetir este tutorial usando um controle de hiperlink ou `<a>` elemento no lugar do LinkButton.

A marcação a seguir mostra a sintaxe declarativa para o Repeater e o ObjectDataSource. Observe que os modelos do Repeater s renderizam uma lista com marcadores com cada item como um LinkButton:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample4.aspx)]

> [!NOTE]
> Para este tutorial, o repetidor deve ter seu estado de exibição habilitado (Observe a omissão do `EnableViewState="False"` da sintaxe declarativa s do repetidor). Na etapa 3, criaremos um manipulador de eventos para o evento repetitivo `ItemCommand` no qual atualizaremos a coleção DataList s ObjectDataSource s `SelectParameters`. No entanto, os s `ItemCommand`repetidos não serão acionados se o estado de exibição estiver desabilitado. Veja [uma Stumper de uma pergunta de ASP.net](http://scottonwriting.net/sowblog/posts/1263.aspx) e [sua solução](http://scottonwriting.net/sowBlog/posts/1268.aspx) para obter mais informações sobre por que o estado de exibição deve ser habilitado para um evento de `ItemCommand` de repetições a ser acionado.

O LinkButton com o valor da propriedade `ID` de `ViewCategory` não tem sua propriedade `Text` definida. Se tivéssemos apenas que quisesse exibir o nome da categoria, definimos a propriedade Text declarativamente, por meio da sintaxe DataBinding, da seguinte forma:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample5.aspx)]

No entanto, queremos mostrar o nome da categoria *e* o número de produtos pertencentes a essa categoria. Essas informações podem ser recuperadas do Gerenciador de eventos do repetidor `ItemDataBound` fazendo uma chamada para o método `ProductBLL` classe s `GetCategoriesByProductID(categoryID)` e determinando quantos registros são retornados no `ProductsDataTable`resultante, como ilustra o código a seguir:

[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample6.vb)]

Começamos garantindo que vamos trabalhar com um item de dados (um cujo `ItemType` seja `Item` ou `AlternatingItem`) e, em seguida, fazer referência à instância de `CategoriesRow` que acabou de ser associada à `RepeaterItem`atual. Em seguida, determinamos o número de produtos para essa categoria criando uma instância da classe `ProductsBLL`, chamando seu método `GetCategoriesByProductID(categoryID)` e determinando o número de registros retornados usando a propriedade `Count`. Por fim, o `ViewCategory` LinkButton no ItemTemplate é referenciado e sua propriedade `Text` é definida como *CategoryName* (*NumberOfProductsInCategory*), em que *NumberOfProductsInCategory* é formatado como um número com zero casas decimais.

> [!NOTE]
> Como alternativa, poderíamos ter adicionado uma *função de formatação* à classe code-behind da página ASP.net, que aceita uma categoria s `CategoryName` e valores de `CategoryID` e retorna o `CategoryName` concatenado com o número de produtos na categoria (conforme determinado pela chamada do método `GetCategoriesByProductID(categoryID)`). Os resultados dessa função de formatação podem ser atribuídos declarativamente à propriedade Text de s de LinkButton, substituindo a necessidade do manipulador de eventos `ItemDataBound`. Consulte o [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) ou [Formatando o DataList e o Repeater com base nos](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-vb.md) tutoriais de dados para obter mais informações sobre como usar funções de formatação.

Depois de adicionar esse manipulador de eventos, Reserve um tempo para testar a página por meio de um navegador. Observe como cada categoria é listada em uma lista com marcadores, exibindo o nome da categoria e o número de produtos associados à categoria (consulte a Figura 4).

[![o nome de cada categoria e o número de produtos são exibidos](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image11.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image10.png)

**Figura 4**: cada nome de categoria e número de produtos são exibidos ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image12.png))

## <a name="updating-thecategoriesdatatableandcategoriestableadapterto-include-the-number-of-products-for-each-category"></a>Atualizando o`CategoriesDataTable`e`CategoriesTableAdapter`para incluir o número de produtos para cada categoria

Em vez de determinar o número de produtos para cada categoria à medida que eles estão vinculados ao repetidor, podemos simplificar esse processo ajustando o `CategoriesDataTable` e `CategoriesTableAdapter` na camada de acesso a dados para incluir essas informações nativamente. Para conseguir isso, devemos adicionar uma nova coluna à `CategoriesDataTable` para manter o número de produtos associados. Para adicionar uma nova coluna a uma DataTable, abra o DataSet tipado (`App_Code\DAL\Northwind.xsd`), clique com o botão direito do mouse na DataTable a ser modificada e escolha Adicionar/coluna. Adicione uma nova coluna à `CategoriesDataTable` (consulte a Figura 5).

[![adicionar uma nova coluna ao CategoriesDataSource](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image14.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image13.png)

**Figura 5**: adicionar uma nova coluna à `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image15.png))

Isso adicionará uma nova coluna chamada `Column1`, que você pode alterar simplesmente digitando um nome diferente. Renomeie essa nova coluna como `NumberOfProducts`. Em seguida, precisamos configurar esta coluna s Propriedades. Clique na nova coluna e vá para a janela Propriedades. Altere a propriedade s `DataType` da coluna de `System.String` para `System.Int32` e defina a propriedade `ReadOnly` como `True`, conforme mostrado na Figura 6.

![Definir as propriedades DataType e ReadOnly da nova coluna](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image16.png)

**Figura 6**: definir as propriedades `DataType` e `ReadOnly` da nova coluna

Embora o `CategoriesDataTable` agora tenha uma coluna de `NumberOfProducts`, seu valor não é definido por nenhuma das consultas do TableAdapter s correspondentes. Podemos atualizar o método `GetCategories()` para retornar essas informações se quisermos que essas informações sejam retornadas toda vez que as informações da categoria forem recuperadas. No entanto, se só precisamos obter o número de produtos associados para as categorias em raras instâncias (como apenas para este tutorial), podemos deixar `GetCategories()` como está e criar um novo método que retorne essas informações. Vamos usar essa última abordagem, criando um novo método chamado `GetCategoriesAndNumberOfProducts()`.

Para adicionar esse novo método de `GetCategoriesAndNumberOfProducts()`, clique com o botão direito do mouse na `CategoriesTableAdapter` e escolha nova consulta. Isso abre o assistente de configuração de consulta do TableAdapter, que nós usamos várias vezes nos tutoriais anteriores. Para esse método, inicie o assistente indicando que a consulta usa uma instrução SQL ad hoc que retorna linhas.

[![criar o método usando uma instrução SQL ad hoc](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image18.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image17.png)

**Figura 7**: criar o método usando uma instrução SQL ad hoc ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image19.png))

[![a instrução SQL retorna linhas](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image21.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image20.png)

**Figura 8**: a instrução SQL retorna linhas ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image22.png))

A próxima tela do assistente nos solicitará que a consulta seja usada. Para retornar os campos `CategoryID`, `CategoryName`e `Description` da categoria, juntamente com o número de produtos associados à categoria, use a seguinte instrução `SELECT`:

[!code-sql[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample7.sql)]

[![especificar a consulta a ser usada](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image24.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image23.png)

**Figura 9**: especificar a consulta a ser usada ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image25.png))

Observe que a subconsulta que computa o número de produtos associados à categoria é alias como `NumberOfProducts`. Essa correspondência de nomenclatura faz com que o valor retornado por essa subconsulta seja associado à coluna `CategoriesDataTable` s `NumberOfProducts`.

Depois de inserir essa consulta, a última etapa é escolher o nome do novo método. Use `FillWithNumberOfProducts` e `GetCategoriesAndNumberOfProducts` para preencher uma DataTable e retornar os padrões de DataTable, respectivamente.

[![nomear os novos métodos TableAdapter s FillWithNumberOfProducts e GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image27.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image26.png)

**Figura 10**: Nomeie os novos métodos TableAdapter s `FillWithNumberOfProducts` e `GetCategoriesAndNumberOfProducts` ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image28.png))

Neste ponto, a camada de acesso a dados foi estendida para incluir o número de produtos por categoria. Como todas as nossas camadas de apresentação roteiam todas as chamadas para a DAL por meio de uma camada de lógica de negócios separada, precisamos adicionar um método de `GetCategoriesAndNumberOfProducts` correspondente à classe `CategoriesBLL`:

[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample8.vb)]

Com a DAL e a BLL concluídas, estamos prontos para associar esses dados ao repetidor de `Categories` no `CategoriesAndProducts.aspx`! Se você já tiver criado um ObjectDataSource para o repetidor a partir da seção determinando o número de produtos na `ItemDataBound` manipulador de eventos, exclua esse ObjectDataSource e remova a configuração da propriedade s `DataSourceID` do repetidor; Além disso, desconecte o `ItemDataBound` do repetidor de eventos do manipulador de eventos removendo a sintaxe `Handles Categories.OnItemDataBound` na classe code-behind ASP.NET.

Com o repetidor de volta em seu estado original, adicione um novo ObjectDataSource chamado `CategoriesDataSource` por meio da marca inteligente s do repetidor. Configure o ObjectDataSource para usar a classe `CategoriesBLL`, mas em vez de fazê-lo usar o método `GetCategories()`, use `GetCategoriesAndNumberOfProducts()` em vez disso (consulte a Figura 11).

[![configurar o ObjectDataSource para usar o método GetCategoriesAndNumberOfProducts](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image30.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image29.png)

**Figura 11**: configurar o ObjectDataSource para usar o método `GetCategoriesAndNumberOfProducts` ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image31.png))

Em seguida, atualize o `ItemTemplate` para que a propriedade de `Text` LinkButton s seja declarativamente atribuída usando a sintaxe DataBinding e inclua os campos de dados `CategoryName` e `NumberOfProducts`. A marcação declarativa completa para o repetidor e o `CategoriesDataSource` ObjectDataSource seguem:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample9.aspx)]

A saída renderizada pela atualização da DAL para incluir uma `NumberOfProducts` coluna é a mesma que usar a abordagem do manipulador de eventos `ItemDataBound` (consulte a Figura 4 para ver uma captura de tela do repetidor que mostra os nomes de categoria e o número de produtos).

## <a name="step-3-displaying-the-selected-category-s-products"></a>Etapa 3: exibindo os produtos da categoria selecionada

Neste ponto, temos o `Categories` Repeater exibindo a lista de categorias junto com o número de produtos em cada categoria. O Repeater usa um LinkButton para cada categoria que, quando clicado, causa um postback, em que é necessário exibir esses produtos para a categoria selecionada na `CategoryProducts` DataList.

Um desafio voltado para nós é como fazer com que o DataList exiba apenas esses produtos para a categoria selecionada. No [mestre/detalhes usando um GridView mestre selecionável com um tutorial de detalhes DetailsView](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-vb.md) , vimos como criar um GridView cujas linhas podem ser selecionadas, com os detalhes da linha selecionada sendo exibidos em um DetailsView na mesma página. O GridView s ObjectDataSource retornou informações sobre todos os produtos usando o método `ProductsBLL` s `GetProducts()` enquanto o DetailsView s ObjectDataSource recuperou informações sobre o produto selecionado usando o método `GetProductsByProductID(productID)`. O valor do parâmetro *`productID`* foi fornecido declarativamente associando-o ao valor da Propriedade GridView s `SelectedValue`. Infelizmente, o repetidor não tem uma propriedade `SelectedValue` e não pode servir como uma origem de parâmetro.

> [!NOTE]
> Esse é um dos desafios que aparecem ao usar o LinkButton em um repetidor. Se tivéssemos usado um hiperlink para passar o `CategoryID` por meio da QueryString, poderíamos usar esse campo QueryString como a origem do valor s do parâmetro.

Antes de nos preocuparmos com a falta de uma propriedade `SelectedValue` para o repetidor, no entanto, vamos primeiro associar o DataList a um ObjectDataSource e especificar seu `ItemTemplate`.

Na marca inteligente DataList s, opte por adicionar um novo ObjectDataSource chamado `CategoryProductsDataSource` e configure-o para usar o método de `GetProductsByCategoryID(categoryID)` da classe `ProductsBLL`. Como o DataList neste tutorial oferece uma interface somente leitura, sinta-se à vontade para definir as listas suspensas nas guias inserir, atualizar e excluir para (nenhum).

[![configurar o ObjectDataSource para usar o método ProductsBLL Class s GetProductsByCategoryID (categoryID)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image33.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image32.png)

**Figura 12**: configurar o ObjectDataSource para usar `ProductsBLL` método de `GetProductsByCategoryID(categoryID)` de classe ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image34.png))

Como o método `GetProductsByCategoryID(categoryID)` espera um parâmetro de entrada ( *`categoryID`* ), o assistente para configurar fonte de dados nos permite especificar a origem do parâmetro. Se as categorias tivessem sido listadas em um GridView ou um DataList, nós d definimos a lista suspensa de origem do parâmetro para Control e a ControlID para a `ID` do controle da Web de dados. No entanto, como o repetidor não tem uma propriedade `SelectedValue`, ele não pode ser usado como uma origem de parâmetro. Se você verificar, descobrirá que a lista suspensa ControlID contém apenas um controle `ID``CategoryProducts`, a `ID` do DataList.

Por enquanto, defina a lista suspensa origem do parâmetro como None. Terminaremos programaticamente atribuindo esse valor de parâmetro quando um LinkButton de categoria for clicado no repetidor.

[![não especificar uma origem de parâmetro para o parâmetro categoryID](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image36.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image35.png)

**Figura 13**: não especifique uma origem de parâmetro para o parâmetro *`categoryID`* ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image37.png))

Depois de concluir o assistente para configurar fonte de dados, o Visual Studio gera automaticamente o `ItemTemplate`DataList s. Substitua esse `ItemTemplate` padrão pelo modelo que usamos no tutorial anterior; Além disso, defina a propriedade DataList s `RepeatColumns` como 2. Depois de fazer essas alterações, a marcação declarativa para o DataList e seu ObjectDataSource associado devem ser semelhantes ao seguinte:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample10.aspx)]

Atualmente, o parâmetro `CategoryProductsDataSource` ObjectDataSource s *`categoryID`* nunca é definido, portanto, nenhum produto é exibido ao exibir a página. O que precisamos fazer é ter esse valor de parâmetro definido com base no `CategoryID` da categoria clicada no repetidor. Isso apresenta dois desafios: primeiro, como determinamos quando um LinkButton no s repetidor `ItemTemplate` foi clicado; e, segundo, como podemos determinar a `CategoryID` da categoria correspondente cujo LinkButton foi clicado?

O LinkButton como os controles Button e ImageButton tem um evento `Click` e um [evento`Command`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.command.aspx). O evento `Click` foi projetado para simplesmente observar que o LinkButton foi clicado. Às vezes, no entanto, além de observar que o LinkButton foi clicado, também precisamos passar algumas informações extras para o manipulador de eventos. Se esse for o caso, as propriedades LinkButton s [`CommandName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandname.aspx) e [`CommandArgument`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.linkbutton.commandargument.aspx) poderão receber essas informações adicionais. Em seguida, quando o LinkButton é clicado, seu evento `Command` é acionado (em vez de seu evento `Click`) e o manipulador de eventos recebe os valores das propriedades `CommandName` e `CommandArgument`.

Quando um evento de `Command` é gerado de dentro de um modelo no repetidor, o [evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeater.itemcommand.aspx) repetido s`ItemCommand` é acionado e recebe os valores `CommandName` e `CommandArgument` do LinkButton clicado (ou Button ou ImageButton). Portanto, para determinar quando um LinkButton de categoria no repetidor foi clicado, precisamos fazer o seguinte:

1. Defina a propriedade `CommandName` do LinkButton no `ItemTemplate` do repetidor como um valor (eu me usei ListProducts). Ao definir esse `CommandName` valor, o evento LinkButton s `Command` é acionado quando o LinkButton é clicado.
2. Defina a Propriedade LinkButton s `CommandArgument` como o valor do item atual s `CategoryID`.
3. Crie um manipulador de eventos para o evento repetido `ItemCommand` do s. No manipulador de eventos, defina o parâmetro `CategoryProductsDataSource` ObjectDataSource s `CategoryID` como o valor do `CommandArgument`passado.

A marcação de `ItemTemplate` a seguir para o repetidor de categorias implementa as etapas 1 e 2. Observe como o valor de `CommandArgument` é atribuído ao item de dados s `CategoryID` usando a sintaxe DataBinding:

[!code-aspx[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample11.aspx)]

Sempre que criar um manipulador de eventos de `ItemCommand`, é prudente sempre verificar o valor de entrada `CommandName`, pois *qualquer* evento de `Command` gerado por *qualquer* botão, LinkButton ou ImageButton dentro do repetidor fará com que o evento de `ItemCommand` seja acionado. Embora, atualmente, tenhamos apenas um desses LinkButton agora, no futuro (ou outro desenvolvedor em nossa equipe) poderia adicionar controles Web de botão adicionais ao repetidor que, quando clicado, gera o mesmo `ItemCommand` manipulador de eventos. Portanto, é melhor garantir que você verifique a propriedade `CommandName` e continue com a lógica programática se ela corresponder ao valor esperado.

Depois de garantir que o valor de `CommandName` passado é igual a ListProducts, o manipulador de eventos atribui o parâmetro `CategoryProductsDataSource` ObjectDataSource s `CategoryID` ao valor do `CommandArgument`passado. Essa modificação no `SelectParameters` ObjectDataSource s faz com que o DataList se reassocie automaticamente à fonte de dados, mostrando os produtos para a categoria selecionada recentemente.

[!code-vb[Main](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/samples/sample12.vb)]

Com essas adições, nosso tutorial foi concluído! Reserve um tempo para testá-lo em um navegador. A Figura 14 mostra a tela ao visitar a página pela primeira vez. Como uma categoria ainda deve ser selecionada, nenhum produto é exibido. Clicar em uma categoria, como produzir, exibe esses produtos na categoria produto em uma exibição de duas colunas (consulte a Figura 15).

[![nenhum produto é exibido ao visitar a página pela primeira vez](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image39.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image38.png)

**Figura 14**: nenhum produto é exibido ao visitar a página pela primeira vez ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image40.png))

[![clicar na categoria produzir lista os produtos correspondentes à direita](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image42.png)](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image41.png)

**Figura 15**: clicar na categoria produzir lista os produtos correspondentes à direita ([clique para exibir a imagem em tamanho normal](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb/_static/image43.png))

## <a name="summary"></a>Resumo

Como vimos neste tutorial e o anterior, os relatórios mestre/de detalhes podem ser distribuídos em duas páginas ou consolidados em um. No entanto, exibir um relatório mestre/detalhes em uma única página apresenta alguns desafios sobre a melhor forma de layout dos registros mestre e de detalhes na página. No *mestre/detalhes usando um GridView mestre selecionável com um tutorial de detalhes DetailsView* , tínhamos os registros de detalhes exibidos acima dos registros mestres; Neste tutorial, usamos técnicas de CSS para que os registros mestres flutuassem à esquerda dos detalhes.

Juntamente com a exibição de relatórios mestre/detalhes, também tivemos a oportunidade de explorar como recuperar o número de produtos associados a cada categoria, bem como executar a lógica do lado do servidor quando um LinkButton (ou um botão ou ImageButton) for clicado em um repetidor.

Este tutorial conclui nosso exame de relatórios de mestre/detalhes com o DataList e o Repeater. Nosso próximo conjunto de tutoriais ilustrará como adicionar recursos de edição e exclusão ao controle DataList.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Floatutorial](http://css.maxdesign.com.au/floatutorial/) um tutorial sobre elementos CSS flutuantes com CSS
- [Posicionamento de CSS](http://www.brainjar.com/css/positioning/) mais informações sobre o posicionamento de elementos com CSS
- [Layout de conteúdo com HTML](http://www.w3schools.com/html/html_layout.asp) usando `<table>` s e outros elementos HTML para posicionamento

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Zack Jones. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-acess-two-pages-datalist-vb.md)
