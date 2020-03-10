---
uid: web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
title: Exibindo informações resumidas no rodapé do GridView (VB) | Microsoft Docs
author: rick-anderson
description: As informações de resumo são geralmente exibidas na parte inferior do relatório em uma linha de resumo. O controle GridView pode incluir uma linha de rodapé em cujas células que podemos PR...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 41c818b7-603a-402b-8847-890a63547b6f
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb
msc.type: authoredcontent
ms.openlocfilehash: c208b4a756f5700be46eec924d8cf8f49b9d2507
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78595910"
---
# <a name="displaying-summary-information-in-the-gridviews-footer-vb"></a>Exibir informações de resumo no rodapé do GridView (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_15_VB.exe) ou [baixar PDF](displaying-summary-information-in-the-gridview-s-footer-vb/_static/datatutorial15vb1.pdf)

> As informações de resumo são geralmente exibidas na parte inferior do relatório em uma linha de resumo. O controle GridView pode incluir uma linha de rodapé em cujas células podemos injetar dados de agregação programaticamente. Neste tutorial, veremos como exibir dados de agregação nesta linha de rodapé.

## <a name="introduction"></a>Introdução

Além de ver cada um dos preços dos produtos, unidades em estoque, unidades em ordem e níveis de reordenação, um usuário também pode estar interessado em informações de agregação, como o preço médio, o número total de unidades em estoque e assim por diante. Essas informações de resumo são geralmente exibidas na parte inferior do relatório em uma linha de resumo. O controle GridView pode incluir uma linha de rodapé em cujas células podemos injetar dados de agregação programaticamente.

Essa tarefa nos apresenta três desafios:

1. Configurando o GridView para exibir sua linha de rodapé
2. Determinando os dados de resumo; ou seja, como computamos o preço médio ou o total das unidades em estoque?
3. Injetando os dados de resumo nas células apropriadas da linha de rodapé

Neste tutorial, veremos como superar esses desafios. Especificamente, criaremos uma página que lista as categorias em uma lista suspensa com os produtos da categoria selecionada exibidos em um GridView. O GridView incluirá uma linha de rodapé que mostra o preço médio e o número total de unidades em estoque e na ordem dos produtos nessa categoria.

[![informações resumidas são exibidas na linha de rodapé do GridView](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image2.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image1.png)

**Figura 1**: as informações resumidas são exibidas na linha de rodapé do GridView ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image3.png))

Este tutorial, com sua categoria para a interface mestre/detalhes de produtos, baseia-se nos conceitos abordados na [filtragem mestre/detalhes anterior com um tutorial DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) . Se você ainda não trabalhou no tutorial anterior, faça isso antes de continuar com este.

## <a name="step-1-adding-the-categories-dropdownlist-and-products-gridview"></a>Etapa 1: adicionar as categorias DropDownList e Products GridView

Antes de se preocupar com a adição de informações resumidas ao rodapé do GridView, primeiro vamos criar o relatório mestre/detalhado. Depois de concluir essa primeira etapa, veremos como incluir dados de resumo.

Comece abrindo a página de `SummaryDataInFooter.aspx` na pasta `CustomFormatting`. Adicione um controle DropDownList e defina seu `ID` como `Categories`. Em seguida, clique no link escolher fonte de dados na marca inteligente da DropDownList e opte por adicionar um novo ObjectDataSource chamado `CategoriesDataSource` que invoca o método de `GetCategories()` da classe `CategoriesBLL`.

[![adicionar um novo ObjectDataSource chamado CategoriesDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image5.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image4.png)

**Figura 2**: adicionar um novo ObjectDataSource chamado `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image6.png))

[![fazer o ObjectDataSource invocar o método GetCategories () da classe CategoriesBLL](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image8.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image7.png)

**Figura 3**: fazer com que o ObjectDataSource invoque o método `GetCategories()` da classe de `CategoriesBLL` ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image9.png))

Depois de configurar o ObjectDataSource, o assistente nos retorna ao assistente de configuração da fonte de dados da DropDownList, do qual precisamos especificar qual valor de campo de dados deve ser exibido e qual deve corresponder ao valor de `ListItem` s da DropDownList. Faça com que o campo `CategoryName` exibido e use o `CategoryID` como o valor.

[![usar os campos CategoryName e CategoryID como o texto e o valor de ListItems, respectivamente](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image11.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image10.png)

**Figura 4**: Use os campos `CategoryName` e `CategoryID` como o `Text` e `Value` para os `ListItem` s, respectivamente ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image12.png))

Neste ponto, temos um DropDownList (`Categories`) que lista as categorias no sistema. Agora, precisamos adicionar um GridView que liste os produtos que pertencem à categoria selecionada. Antes de fazermos, no entanto, Reserve um tempo para marcar a caixa de seleção Enable AutoPostBack na marca inteligente da DropDownList. Conforme discutido na *filtragem de mestre/detalhes com um tutorial DropDownList* , definindo a propriedade `AutoPostBack` da dropdownlist como `True` a página será lançada de volta cada vez que o valor de DropDownList for alterado. Isso fará com que o GridView seja atualizado, mostrando os produtos para a categoria selecionada recentemente. Se a propriedade `AutoPostBack` for definida como `False` (o padrão), a alteração da categoria não causará um postback e, portanto, não atualizará os produtos listados.

[![marque a caixa de seleção Habilitar AutoPostBack na marca inteligente da DropDownList](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image14.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image13.png)

**Figura 5**: marque a caixa de seleção Habilitar AutoPostBack na marca inteligente da DropDownList ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image15.png))

Adicione um controle GridView à página para exibir os produtos da categoria selecionada. Defina o `ID` do GridView como `ProductsInCategory` e associe-o a um novo ObjectDataSource chamado `ProductsInCategoryDataSource`.

[![adicionar um novo ObjectDataSource chamado ProductsInCategoryDataSource](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image17.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image16.png)

**Figura 6**: adicionar um novo ObjectDataSource chamado `ProductsInCategoryDataSource` ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image18.png))

Configure o ObjectDataSource para que ele invoque o método `GetProductsByCategoryID(categoryID)` da classe `ProductsBLL`.

[![fazer com que ObjectDataSource invoque o método GetProductsByCategoryID (categoryID)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image20.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image19.png)

**Figura 7**: fazer com que o ObjectDataSource invoque o método `GetProductsByCategoryID(categoryID)` ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image21.png))

Como o método `GetProductsByCategoryID(categoryID)` usa um parâmetro de entrada, na etapa final do assistente, podemos especificar a origem do valor do parâmetro. Para exibir esses produtos da categoria selecionada, peça ao parâmetro extraído da `Categories` DropDownList.

[![obter o valor do parâmetro categoryID das categorias selecionadas DropDownList](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image23.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image22.png)

**Figura 8**: obter o valor do parâmetro *`categoryID`* das categorias selecionadas DropDownList ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image24.png))

Depois de concluir o assistente, o GridView terá um BoundField para cada uma das propriedades do produto. Vamos limpar esses BoundFields para que apenas os `ProductName`, `UnitPrice`, `UnitsInStock`e `UnitsOnOrder` BoundFields sejam exibidos. Sinta-se à vontade para adicionar qualquer configuração em nível de campo aos BoundFields restantes (como formatar o `UnitPrice` como uma moeda). Depois de fazer essas alterações, a marcação declarativa do GridView deve ser semelhante ao seguinte:

[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample1.aspx)]

Neste ponto, temos um relatório mestre/detalhes totalmente funcional que mostra o nome, o preço unitário, as unidades em estoque e as unidades na ordem dos produtos que pertencem à categoria selecionada.

[![obter o valor do parâmetro categoryID das categorias selecionadas DropDownList](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image26.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image25.png)

**Figura 9**: obter o valor do parâmetro *`categoryID`* das categorias selecionadas DropDownList ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image27.png))

## <a name="step-2-displaying-a-footer-in-the-gridview"></a>Etapa 2: exibindo um rodapé em GridView

O controle GridView pode exibir uma linha de cabeçalho e de rodapé. Essas linhas são exibidas dependendo dos valores das propriedades `ShowHeader` e `ShowFooter`, respectivamente, com `ShowHeader` padrão para `True` e `ShowFooter` `False`. Para incluir um rodapé no GridView, basta definir sua propriedade `ShowFooter` como `True`.

[![definir a propriedade de conrodapé do GridView como true](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image29.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image28.png)

**Figura 10**: definir a propriedade `ShowFooter` do GridView como `True` ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image30.png))

A linha de rodapé tem uma célula para cada um dos campos definidos no GridView; no entanto, essas células ficam vazias por padrão. Reserve um tempo para exibir nosso progresso em um navegador. Com a propriedade `ShowFooter` agora definida como `True`, o GridView inclui uma linha de rodapé vazia.

[![o GridView agora inclui uma linha de rodapé](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image32.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image31.png)

**Figura 11**: o GridView agora inclui uma linha de rodapé ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image33.png))

A linha de rodapé da Figura 11 não se destaca, pois tem um plano de fundo branco. Vamos criar uma classe CSS `FooterStyle` em `Styles.css` que especifica um plano de fundo vermelho escuro e, em seguida, configurar o arquivo de capa `GridView.skin` no tema `DataWebControls` para atribuir essa classe CSS à propriedade `FooterStyle`do `CssClass` do GridView. Se você precisar se pincelar em capas e temas, consulte novamente o tutorial [exibindo dados com o ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) .

Comece adicionando a seguinte classe CSS para `Styles.css`:

[!code-css[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample2.css)]

A classe CSS `FooterStyle` é semelhante em estilo à classe `HeaderStyle`, embora a cor do plano de fundo do `HeaderStyle`seja mais escura e seu texto seja exibido em uma fonte em negrito. Além disso, o texto no rodapé é alinhado à direita, enquanto o texto do cabeçalho é centralizado.

Em seguida, para associar essa classe CSS ao rodapé de cada GridView, abra o arquivo `GridView.skin` no tema `DataWebControls` e defina a propriedade `CssClass` do `FooterStyle`. Após essa adição, a marcação do arquivo deve ser semelhante a:

[!code-aspx[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample3.aspx)]

Como a captura de tela abaixo mostra, essa alteração faz com que o rodapé fique mais claro.

[![a linha de rodapé do GridView agora tem uma cor de fundo reddish](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image35.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image34.png)

**Figura 12**: a linha de rodapé do GridView agora tem uma cor de fundo reddish ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image36.png))

## <a name="step-3-computing-the-summary-data"></a>Etapa 3: computando os dados de resumo

Com o rodapé do GridView exibido, o próximo desafio voltado para nós é como computar os dados de resumo. Há duas maneiras de computar essas informações agregadas:

1. Por meio de uma consulta SQL, poderíamos emitir uma consulta adicional para o banco de dados para computar os dado de Resumo de uma determinada categoria. O SQL inclui várias funções de agregação junto com uma cláusula `GROUP BY` para especificar os dados sobre os quais os dados devem ser resumidos. A seguinte consulta SQL trazeria de volta as informações necessárias:  

    [!code-sql[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample4.sql)]

    É claro que você não iria querer emitir essa consulta diretamente na página `SummaryDataInFooter.aspx`, mas sim criando um método na `ProductsTableAdapter` e na `ProductsBLL`.
2. Computar essas informações à medida que elas estão sendo adicionadas ao GridView, conforme discutido no tutorial [formatação personalizado com base no data](custom-formatting-based-upon-data-cs.md) , o manipulador de eventos `RowDataBound` do GridView é acionado uma vez para cada linha que está sendo adicionada ao GridView após seu recebimento de dados. Ao criar um manipulador de eventos para esse evento, podemos manter um total acumulado dos valores que desejamos agregar. Depois que a última linha de dados tiver sido associada ao GridView, temos os totais e as informações necessárias para calcular a média.

Normalmente, usei a segunda abordagem, pois ela economiza uma viagem ao banco de dados e o esforço necessário para implementar a funcionalidade de resumo na camada de acesso a data e na camada de lógica de negócios, mas a abordagem seria suficiente. Para este tutorial, vamos usar a segunda opção e acompanhar o total em execução usando o manipulador de eventos `RowDataBound`.

Crie um manipulador de eventos `RowDataBound` para GridView selecionando o GridView no designer, clicando no ícone de raio da janela Propriedades e clicando duas vezes no evento `RowDataBound`. Como alternativa, você pode selecionar o GridView e seu evento de vinculação de borda nas listas suspensas na parte superior do arquivo de classe code-behind ASP.NET. Isso criará um novo manipulador de eventos chamado `ProductsInCategory_RowDataBound` na classe code-behind da página de `SummaryDataInFooter.aspx`.

[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample5.vb)]

Para manter um total em execução, precisamos definir variáveis fora do escopo do manipulador de eventos. Crie as quatro variáveis de nível de página a seguir:

- `_totalUnitPrice`, do tipo `Decimal`
- `_totalNonNullUnitPriceCount`, do tipo `Integer`
- `_totalUnitsInStock`, do tipo `Integer`
- `_totalUnitsOnOrder`, do tipo `Integer`

Em seguida, escreva o código para incrementar essas três variáveis para cada linha de dados encontrada no manipulador de eventos `RowDataBound`.

[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample6.vb)]

O manipulador de eventos `RowDataBound` começa garantindo que estamos lidando com uma DataRow. Depois de estabelecida, a instância `Northwind.ProductsRow` que acabou de ser associada ao objeto `GridViewRow` em `e.Row` é armazenada na variável `product`. Em seguida, as variáveis de total em execução são incrementadas pelos valores correspondentes do produto atual (supondo que elas não contenham um banco de dados `NULL` valor). Acompanhamos o total de `UnitPrice` em execução e o número de registros de `UnitPrice` não`NULL` porque o preço médio é o quociente desses dois números.

## <a name="step-4-displaying-the-summary-data-in-the-footer"></a>Etapa 4: exibindo os dados de resumo no rodapé

Com os dados de resumo totalizados, a última etapa é exibi-lo na linha de rodapé do GridView. Essa tarefa também pode ser realizada programaticamente por meio do manipulador de eventos `RowDataBound`. Lembre-se de que o manipulador de eventos `RowDataBound` é acionado para *cada* linha associada ao GridView, incluindo a linha de rodapé. Portanto, podemos ampliar nosso manipulador de eventos para exibir os dados na linha de rodapé usando o seguinte código:

[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample7.vb)]

Como a linha de rodapé é adicionada ao GridView depois que todas as linhas de dados tiverem sido adicionadas, podemos ter certeza de que, no momento em que estamos prontos para exibir os dados de resumo no rodapé, os cálculos totais em execução serão concluídos. A última etapa, então, é definir esses valores nas células do rodapé.

Para exibir o texto em uma determinada célula de rodapé, use `e.Row.Cells(index).Text = value`, em que a indexação de `Cells` começa em 0. O código a seguir computa o preço médio (o preço total dividido pelo número de produtos) e o exibe junto com o número total de unidades em estoque e unidades na ordem nas células de rodapé apropriadas do GridView.

[!code-vb[Main](displaying-summary-information-in-the-gridview-s-footer-vb/samples/sample8.vb)]

A Figura 13 mostra o relatório após a adição desse código. Observe como a `ToString("c")` faz com que as informações de Resumo de preço médio sejam formatadas como uma moeda.

[![a linha de rodapé do GridView agora tem uma cor de fundo reddish](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image38.png)](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image37.png)

**Figura 13**: a linha de rodapé do GridView agora tem uma cor de fundo reddish ([clique para exibir a imagem em tamanho normal](displaying-summary-information-in-the-gridview-s-footer-vb/_static/image39.png))

## <a name="summary"></a>Resumo

A exibição de dados de resumo é um requisito de relatório comum, e o controle GridView facilita a inclusão de informações em sua linha de rodapé. A linha de rodapé é exibida quando a propriedade `ShowFooter` do GridView é definida como `True` e pode fazer com que o texto em suas células seja definido programaticamente por meio do manipulador de eventos `RowDataBound`. A computação dos dados de resumo pode ser feita consultando o banco de dado novamente ou usando o código na classe code-behind da página ASP.NET para calcular programaticamente os dados de resumo.

Este tutorial conclui nosso exame de formatação personalizada com os controles GridView, DetailsView e FormView. Nosso próximo tutorial começa nossa exploração de inserção, atualização e exclusão de dados usando esses mesmos controles.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](using-the-formview-s-templates-vb.md)
