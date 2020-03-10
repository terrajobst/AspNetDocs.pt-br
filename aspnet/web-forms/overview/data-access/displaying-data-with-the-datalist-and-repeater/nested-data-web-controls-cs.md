---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
title: Controles da Web de dadosC#aninhados () | Microsoft Docs
author: rick-anderson
description: Neste tutorial, exploraremos como usar um repetidor aninhado dentro de outro repetidor. Os exemplos ilustram como preencher o repetidor interno d...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: ad3cb0ec-26cf-42d7-b81b-184a34ec9f86
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/nested-data-web-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 8ef15bebb2c29976274b0cca1d6ace434ccc55ce
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78594965"
---
# <a name="nested-data-web-controls-c"></a>Controles da Web de dados aninhadas (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_32_CS.exe) ou [baixar PDF](nested-data-web-controls-cs/_static/datatutorial32cs1.pdf)

> Neste tutorial, exploraremos como usar um repetidor aninhado dentro de outro repetidor. Os exemplos ilustram como preencher o repetidor interno de forma declarativa e programaticamente.

## <a name="introduction"></a>Introdução

Além da sintaxe estática HTML e DataBinding, os modelos também podem incluir controles da Web e controles de usuário. Esses controles da Web podem ter suas propriedades atribuídas por meio da sintaxe declarativa, DataBinding ou podem ser acessados programaticamente nos manipuladores de eventos apropriados do servidor.

Ao inserir controles dentro de um modelo, a aparência e a experiência do usuário podem ser personalizadas e aprimoradas no. Por exemplo, no tutorial [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) , vimos como personalizar a exibição de GridView s adicionando um controle Calendar em um TemplateField para mostrar a data de contratação de um funcionário; na [adição de controles de validação à edição e à inserção de interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) e [à personalização dos tutoriais da interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) , vimos como personalizar a edição e a inserção de interfaces adicionando controles de validação, TextBoxes, DropDownLists e outros controles da Web.

Os modelos também podem conter outros controles da Web de dados. Ou seja, podemos ter um DataList que contenha outro DataList (ou Repeater ou GridView ou DetailsView, e assim por diante) em seus modelos. O desafio com essa interface é associar os dados apropriados ao controle da Web de dados internos. Há algumas abordagens diferentes disponíveis, variando de opções declarativas usando o ObjectDataSource para os de programação.

Neste tutorial, exploraremos como usar um repetidor aninhado dentro de outro repetidor. O repetidor externo conterá um item para cada categoria no banco de dados, exibindo o nome e a descrição da categoria. Cada repetidor interno de item de categoria exibirá informações para cada produto que pertence a essa categoria (consulte a Figura 1) em uma lista com marcadores. Nossos exemplos ilustram como preencher o repetidor interno de forma declarativa e programaticamente.

[![cada categoria, juntamente com seus produtos, estão listadas](nested-data-web-controls-cs/_static/image2.png)](nested-data-web-controls-cs/_static/image1.png)

**Figura 1**: cada categoria, juntamente com seus produtos, são listadas ([clique para exibir a imagem em tamanho normal](nested-data-web-controls-cs/_static/image3.png))

## <a name="step-1-creating-the-category-listing"></a>Etapa 1: criando a listagem de categoria

Ao criar uma página que usa controles da Web de dados aninhados, acho útil projetar, criar e testar o controle da Web de dados mais externo primeiro, sem sequer se preocupar com o controle interno aninhado. Portanto, vamos começar percorrendo as etapas necessárias para adicionar um repetidor à página que lista o nome e a descrição de cada categoria.

Comece abrindo a página `NestedControls.aspx` na pasta `DataListRepeaterBasics` e adicione um controle Repeater à página, definindo sua propriedade `ID` como `CategoryList`. Na marca inteligente repetida de s, escolha criar um novo ObjectDataSource chamado `CategoriesDataSource`.

[![nomear o novo ObjectDataSource CategoriesDataSource](nested-data-web-controls-cs/_static/image5.png)](nested-data-web-controls-cs/_static/image4.png)

**Figura 2**: nomeie o novo ObjectDataSource `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](nested-data-web-controls-cs/_static/image6.png))

Configure o ObjectDataSource para que ele Extraia seus dados do método de `GetCategories` da classe `CategoriesBLL`.

[![configurar o ObjectDataSource para usar o método GetCategories da classe CategoriesBLL](nested-data-web-controls-cs/_static/image8.png)](nested-data-web-controls-cs/_static/image7.png)

**Figura 3**: configurar o ObjectDataSource para usar o método de `GetCategories` da classe `CategoriesBLL` ([clique para exibir a imagem em tamanho normal](nested-data-web-controls-cs/_static/image9.png))

Para especificar o conteúdo do modelo s do Repeater, precisamos ir para a exibição da fonte e inserir manualmente a sintaxe declarativa. Adicione um `ItemTemplate` que exibe o nome da categoria em um elemento `<h4>` e a descrição da categoria em um elemento de parágrafo (`<p>`). Além disso, vamos separar cada categoria com uma regra horizontal (`<hr>`). Depois de fazer essas alterações, sua página deve conter sintaxe declarativa para o Repeater e ObjectDataSource semelhante ao seguinte:

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample1.aspx)]

A Figura 4 mostra nosso progresso quando visualizado por meio de um navegador.

[![o nome e a descrição de cada categoria são listados, separados por uma regra horizontal](nested-data-web-controls-cs/_static/image11.png)](nested-data-web-controls-cs/_static/image10.png)

**Figura 4**: o nome e a descrição de cada categoria são listados, separados por uma regra horizontal ([clique para exibir a imagem em tamanho normal](nested-data-web-controls-cs/_static/image12.png))

## <a name="step-2-adding-the-nested-product-repeater"></a>Etapa 2: adicionando o repetidor de produto aninhado

Com a listagem de categorias concluída, nossa próxima tarefa é adicionar um repetidor ao `CategoryList` s `ItemTemplate` que exibe informações sobre esses produtos que pertencem à categoria apropriada. Há várias maneiras de recuperar os dados para esse repetidor interno, dois dos quais vamos explorar em breve. Por enquanto, vamos apenas criar o repetidor de produtos dentro do `ItemTemplate``CategoryList` repetidor de s. Especificamente, deixe que o repetidor de produto exiba cada produto em uma lista com marcadores com cada item de lista, incluindo o nome e o preço do produto.

Para criar esse repetidor, precisamos inserir manualmente a sintaxe declarativa s e os modelos do repetidor interno no `ItemTemplate`de `CategoryList` s. Adicione a seguinte marcação dentro do `ItemTemplate`repetidor `CategoryList` s:

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample2.aspx)]

## <a name="step-3-binding-the-category-specific-products-to-the-productsbycategorylist-repeater"></a>Etapa 3: associando os produtos específicos à categoria ao repetidor ProductsByCategoryList

Se você visitar a página por meio de um navegador neste ponto, sua tela terá a mesma aparência da Figura 4, pois ainda gostaríamos de associar quaisquer dados ao repetidor. Há algumas maneiras pelas quais podemos obter os registros de produtos apropriados e associá-los ao repetidor, mais eficientes do que outros. O principal desafio aqui é obter novamente os produtos apropriados para a categoria especificada.

Os dados a serem associados ao controle Repeater interno podem ser acessados de forma declarativa, por meio de um ObjectDataSource no `ItemTemplate`repetidor de `CategoryList` s, ou programaticamente, da página code-behind da página ASP.NET. Da mesma forma, esses dados podem ser vinculados ao repetidor interno, seja declarativamente através da propriedade s `DataSourceID` do Repeater interno ou por meio da sintaxe declarativa de DataBinding ou programaticamente referenciando o repetidor interno no manipulador de eventos `ItemDataBound` Repeater `CategoryList`, definindo programaticamente sua propriedade `DataSource` e chamando seu método `DataBind()`. Vamos explorar cada uma dessas abordagens.

## <a name="accessing-the-data-declaratively-with-an-objectdatasource-control-and-theitemdataboundevent-handler"></a>Acessando os dados de forma declarativa com um controle ObjectDataSource e o manipulador de eventos`ItemDataBound`

Como usamos o ObjectDataSource extensivamente durante esta série de tutoriais, a escolha mais natural para acessar dados para este exemplo é com o ObjectDataSource. A classe `ProductsBLL` tem um método `GetProductsByCategoryID(categoryID)` que retorna informações sobre os produtos que pertencem à *`categoryID`* especificada. Portanto, podemos adicionar um ObjectDataSource ao `ItemTemplate` repetidor `CategoryList` s e configurá-lo para acessar seus dados desse método de classe s.

Infelizmente, o repetidor não permite que seus modelos sejam editados por meio do modo de exibição de Design, portanto, precisamos adicionar a sintaxe declarativa para esse controle de ObjectDataSource manualmente. A sintaxe a seguir mostra as `ItemTemplate` repetidas `CategoryList` s depois de adicionar esse novo ObjectDataSource (`ProductsByCategoryDataSource`):

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample3.aspx)]

Ao usar a abordagem ObjectDataSource, precisamos definir a propriedade `ProductsByCategoryList` Repeater s `DataSourceID` como a `ID` do ObjectDataSource (`ProductsByCategoryDataSource`). Além disso, observe que o ObjectDataSource tem um elemento `<asp:Parameter>` que especifica o valor *`categoryID`* que será passado para o método `GetProductsByCategoryID(categoryID)`. Mas como especificamos esse valor? O ideal é poder apenas definir a propriedade `DefaultValue` do elemento `<asp:Parameter>` usando a sintaxe DataBinding, da seguinte forma:

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample4.aspx)]

Infelizmente, a sintaxe de DataBinding só é válida em controles que têm um evento `DataBinding`. A classe `Parameter` não tem tal evento e, portanto, a sintaxe acima é inválida e resultará em um erro de tempo de execução.

Para definir esse valor, precisamos criar um manipulador de eventos para o evento `CategoryList` repetition s `ItemDataBound`. Lembre-se de que o evento `ItemDataBound` é acionado uma vez para cada item associado ao repetidor. Portanto, sempre que esse evento é acionado para o repetidor externo, podemos atribuir o valor de `CategoryID` atual ao parâmetro `ProductsByCategoryDataSource` ObjectDataSource s `CategoryID`.

Crie um manipulador de eventos para o evento `CategoryList` repetitivo `ItemDataBound` com o seguinte código:

[!code-csharp[Main](nested-data-web-controls-cs/samples/sample5.cs)]

Esse manipulador de eventos começa garantindo que estamos lidando com um item de dados em vez do cabeçalho, rodapé ou item separador. Em seguida, fazemos referência à instância real de `CategoriesRow` que acabou de ser associada ao `RepeaterItem`atual. Por fim, referenciamos o ObjectDataSource no `ItemTemplate` e atribuímos seu valor de parâmetro `CategoryID` à `CategoryID` do `RepeaterItem`atual.

Com esse manipulador de eventos, o repetidor de `ProductsByCategoryList` em cada `RepeaterItem` está associado a esses produtos na categoria `RepeaterItem` s. A Figura 5 mostra uma captura de tela da saída resultante.

[![o repetidor externo lista cada categoria; o interior lista os produtos para essa categoria](nested-data-web-controls-cs/_static/image14.png)](nested-data-web-controls-cs/_static/image13.png)

**Figura 5**: o repetidor externo lista cada categoria; o interior lista os produtos para essa categoria ([clique para exibir a imagem em tamanho normal](nested-data-web-controls-cs/_static/image15.png))

## <a name="accessing-the-products-by-category-data-programmatically"></a>Acessando os produtos por dados de categoria programaticamente

Em vez de usar um ObjectDataSource para recuperar os produtos para a categoria atual, poderíamos criar um método em nossa classe code-behind de página ASP.NET s (ou na pasta `App_Code` ou em um projeto de biblioteca de classes separado) que retorna o conjunto apropriado de produtos quando passado em uma `CategoryID`. Imagine que tínhamos esse método em nossa classe code-behind de página ASP.NET s e que ele foi nomeado `GetProductsInCategory(categoryID)`. Com esse método em vigor, poderíamos associar os produtos da categoria atual ao repetidor interno usando a seguinte sintaxe declarativa:

[!code-aspx[Main](nested-data-web-controls-cs/samples/sample6.aspx)]

A Propriedade Repeater s `DataSource` usa a sintaxe DataBinding para indicar que seus dados vêm do método `GetProductsInCategory(categoryID)`. Como `Eval("CategoryID")` retorna um valor do tipo `Object`, convertemos o objeto em um `Integer` antes de passá-lo para o método `GetProductsInCategory(categoryID)`. Observe que o `CategoryID` acessado aqui por meio da sintaxe de DataBinding é o `CategoryID` no repetidor *externo* (`CategoryList`), aquele que s está vinculado aos registros na tabela `Categories`. Portanto, sabemos que `CategoryID` não pode ser um valor de `NULL` de banco de dados, e é por isso que podemos converter o método de `Eval` de forma oculta sem verificar se retratamos de uma `DBNull`.

Com essa abordagem, precisamos criar o método `GetProductsInCategory(categoryID)` e fazer com que ele recupere o conjunto apropriado de produtos, considerando o *`categoryID`* fornecido. Podemos fazer isso simplesmente retornando o `ProductsDataTable` retornado pelo método `ProductsBLL` Class s `GetProductsByCategoryID(categoryID)`. Vamos criar o método `GetProductsInCategory(categoryID)` na classe code-behind para nossa página de `NestedControls.aspx`. Faça isso usando o código a seguir:

[!code-csharp[Main](nested-data-web-controls-cs/samples/sample7.cs)]

Esse método simplesmente cria uma instância do método `ProductsBLL` e retorna os resultados do método `GetProductsByCategoryID(categoryID)`. Observe que o método deve ser marcado `Public` ou `Protected`; Se o método estiver marcado `Private`, ele não poderá ser acessado da marcação declarativa de s da página ASP.NET.

Depois de fazer essas alterações para usar essa nova técnica, Reserve um momento para exibir a página por meio de um navegador. A saída deve ser idêntica à saída ao usar a abordagem de manipulador de eventos ObjectDataSource e `ItemDataBound` (consulte novamente a Figura 5 para ver uma captura de tela).

> [!NOTE]
> Pode parecer muito ocupado criar o método `GetProductsInCategory(categoryID)` na classe code-behind da página ASP.NET s. Afinal, esse método simplesmente cria uma instância da classe `ProductsBLL` e retorna os resultados de seu método `GetProductsByCategoryID(categoryID)`. Por que não basta chamar esse método diretamente da sintaxe DataBinding no repetidor interno, como: `DataSource='<%# ProductsBLL.GetProductsByCategoryID((int)(Eval("CategoryID"))) %>'`? Embora essa sintaxe não funcione com nossa implementação atual da classe `ProductsBLL` (já que o método `GetProductsByCategoryID(categoryID)` é um método de instância), você pode modificar `ProductsBLL` para incluir um método de `GetProductsByCategoryID(categoryID)` estático ou fazer com que a classe inclua um método `Instance()` estático para retornar uma nova instância da classe `ProductsBLL`.

Embora essas modificações eliminem a necessidade do método de `GetProductsInCategory(categoryID)` na classe code-behind da página ASP.NET, o método de classe code-behind nos dá mais flexibilidade ao trabalhar com os dados recuperados, como veremos em breve.

## <a name="retrieving-all-of-the-product-information-at-once"></a>Recuperando todas as informações do produto de uma vez

As duas técnicas de anterior que examinamos pegam esses produtos para a categoria atual fazendo uma chamada para o método de `GetProductsByCategoryID(categoryID)` da classe `ProductsBLL` (a primeira abordagem fez isso por meio de um ObjectDataSource, a segunda através do método `GetProductsInCategory(categoryID)` na classe code-behind). Cada vez que esse método é invocado, a camada de lógica de negócios é chamada para a camada de acesso a dados, que consulta o banco de dado com uma instrução SQL que retorna linhas da tabela `Products` cujo campo de `CategoryID` corresponde ao parâmetro de entrada fornecido.

Dadas *n* categorias no sistema, essa abordagem conecta *n* + 1 chamadas ao banco de dados uma consulta de banco de dados para obter todas as categorias e, em seguida, *N* chamadas para obter os produtos específicos de cada categoria. No entanto, podemos recuperar todos os dados necessários em apenas duas chamadas de banco de dado uma chamada para obter todas as categorias e outra para obter todos os produtos. Assim que tivermos todos os produtos, podemos filtrar esses produtos para que apenas os produtos correspondentes aos `CategoryID` atuais estejam associados a esse repetidor interno da categoria.

Para fornecer essa funcionalidade, precisamos apenas fazer uma pequena modificação no método `GetProductsInCategory(categoryID)` em nossa classe code-behind da página ASP.NET. Em vez de retornar indistintamente os resultados do método `ProductsBLL` `GetProductsByCategoryID(categoryID)` da classe, podemos primeiro acessar *todos* os produtos (se eles ainda não tiverem sido acessados) e retornar apenas a exibição filtrada dos produtos com base no `CategoryID`passado.

[!code-csharp[Main](nested-data-web-controls-cs/samples/sample8.cs)]

Observe a adição da variável em nível de página, `allProducts`. Isso contém informações sobre todos os produtos e é preenchido na primeira vez que o método de `GetProductsInCategory(categoryID)` é invocado. Depois de garantir que o objeto de `allProducts` foi criado e populado, o método filtra os resultados da DataTable, de forma que somente as linhas cujo `CategoryID` corresponde ao `CategoryID` especificado estejam acessíveis. Essa abordagem reduz o número de vezes que o banco de dados é acessado de *N* + 1 até dois.

Esse aprimoramento não introduz nenhuma alteração na marcação renderizada da página, nem retorna menos registros do que a outra abordagem. Ele simplesmente reduz o número de chamadas para o banco de dados.

> [!NOTE]
> Uma delas pode ter uma razão intuitiva de reduzir o número de acessos ao banco de dados assuredly a melhorar o desempenho. No entanto, esse pode não ser o caso. Se você tiver um grande número de produtos cujo `CategoryID` é `NULL`, por exemplo, a chamada para o método `GetProducts` retornará vários produtos que nunca são exibidos. Além disso, retornar todos os produtos pode ser desnecessário se você apenas mostrar um subconjunto das categorias, o que pode ser o caso se você tiver implementado a paginação.

Como sempre, quando se trata de analisar o desempenho de duas técnicas, a única medida certeiras é executar testes controlados adaptados para os cenários de caso comum de seus aplicativos.

## <a name="summary"></a>Resumo

Neste tutorial, vimos como aninhar um controle da Web de dados dentro de outro, examinando especificamente como fazer um repetidor externo exibir um item para cada categoria com um repetidor interno listando os produtos de cada categoria em uma lista com marcadores. O principal desafio na criação de uma interface de usuário aninhada está no acesso e na vinculação dos dados corretos ao controle da Web de dados internos. Há uma variedade de técnicas disponíveis, duas das quais examinamos neste tutorial. A primeira abordagem examinada usou um ObjectDataSource no `ItemTemplate` de controle da Web de dados externos que foi associado ao controle da Web de dados internos por meio de sua propriedade `DataSourceID`. A segunda técnica acessou os dados por meio de um método na classe code-behind da página ASP.NET s. Esse método pode então ser associado à propriedade `DataSource` do controle da Web de dados internos por meio da sintaxe DataBinding.

Embora a interface do usuário aninhada examinada neste tutorial tenha usado um repetidor aninhado em um repetidor, essas técnicas podem ser estendidas para os outros controles da Web de dados. Você pode aninhar um repetidor em um GridView ou em um GridView dentro de um DataList, e assim por diante.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Zack Jones e Liz Shulok. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
> [Próximo](displaying-data-with-the-datalist-and-repeater-controls-vb.md)
