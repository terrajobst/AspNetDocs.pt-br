---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
title: Classificando dados em um controle DataList ou Repeater (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, examinaremos como incluir o suporte à classificação no DataList e no Repeater, além de como construir um DataList ou um repetidor cujos dados possam...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: 97c13898-0741-45f9-b3fa-7540ab1679e6
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 81e07bec8569b9ee987dfaa84dec9eec95a2692f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78576282"
---
# <a name="sorting-data-in-a-datalist-or-repeater-control-vb"></a>Classificação de dados em um controle DataList ou Repeater (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_VB.exe) ou [baixar PDF](sorting-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial45vb1.pdf)

> Neste tutorial, examinaremos como incluir o suporte à classificação no DataList e no Repeater, além de como construir um DataList ou um repetidor cujos dados possam ser paginados e classificados.

## <a name="introduction"></a>Introdução

No [tutorial anterior](paging-report-data-in-a-datalist-or-repeater-control-vb.md) , examinamos como adicionar suporte à paginação a um DataList. Criamos um novo método na classe `ProductsBLL` (`GetProductsAsPagedDataSource`) que retornava um objeto `PagedDataSource`. Quando associado a um DataList ou Repeater, o DataList ou o Repeater exibiria apenas a página de dados solicitada. Essa técnica é semelhante ao que é usado internamente pelos controles GridView, DetailsView e FormView para fornecer sua funcionalidade interna de paginação padrão.

Além de oferecer suporte à paginação, o GridView também inclui suporte à classificação pronta para uso. Nem o DataList nem o Repeater fornecem funcionalidade de classificação interna; no entanto, os recursos de classificação podem ser adicionados com um pouco de código. Neste tutorial, examinaremos como incluir o suporte à classificação no DataList e no Repeater, além de como construir um DataList ou um repetidor cujos dados possam ser paginados e classificados.

## <a name="a-review-of-sorting"></a>Uma revisão da classificação

Como vimos no tutorial de [dados de relatório de paginação e classificação](../paging-and-sorting/paging-and-sorting-report-data-vb.md) , o controle GridView fornece suporte de classificação pronto para uso. Cada campo GridView pode ter um `SortExpression`associado, que indica o campo de dados pelo qual classificar os dados. Quando a Propriedade GridView s `AllowSorting` é definida como `true`, cada campo GridView que tem um valor de propriedade `SortExpression` tem seu cabeçalho renderizado como um LinkButton. Quando um usuário clica em um cabeçalho de um campo GridView específico, um postback ocorre e os dados são classificados de acordo com o `SortExpression`do campo clicado.

O controle GridView também tem uma propriedade `SortExpression`, que armazena a `SortExpression` do campo GridView pelo qual os dados são classificados. Além disso, uma propriedade `SortDirection` indica se os dados devem ser classificados em ordem crescente ou decrescente (se um usuário clicar em um link de cabeçalho s de um campo GridView específico duas vezes em sucessivamente, a ordem de classificação será alternada).

Quando o GridView é associado a seu controle da fonte de dados, ele entrega seu `SortExpression` e `SortDirection` Propriedades ao controle da fonte de dados. O controle da fonte de dados recupera os dados e, em seguida, os classifica de acordo com as propriedades fornecidas `SortExpression` e `SortDirection`. Depois de classificar os dados, o controle da fonte de dados o retorna para o GridView.

Para replicar essa funcionalidade com os controles DataList ou Repeater, devemos:

- Criar uma interface de classificação
- Lembre-se do campo de dados para classificar por e se deve ser classificado em ordem crescente ou decrescente
- Instrua o ObjectDataSource a classificar os dados por um campo de dados específico

Iremos lidar com essas três tarefas nas etapas 3 e 4. Depois disso, examinaremos como incluir o suporte de paginação e classificação em um DataList ou Repeater.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>Etapa 2: exibindo os produtos em um repetidor

Antes de nos preocuparmos em implementar qualquer uma das funcionalidades relacionadas à classificação, vamos começar listando os produtos em um controle Repeater. Comece abrindo a página de `Sorting.aspx` na pasta `PagingSortingDataListRepeater`. Adicione um controle Repeater à página da Web, definindo sua propriedade `ID` como `SortableProducts`. Na marca inteligente do repetidor, crie um novo ObjectDataSource chamado `ProductsDataSource` e configure-o para recuperar dados do método de `GetProducts()` da classe `ProductsBLL`. Selecione a opção (nenhum) nas listas suspensas nas guias inserir, atualizar e excluir.

[![criar um ObjectDataSource e configurá-lo para usar o método GetProductsAsPagedDataSource ()](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Figura 1**: criar um ObjectDataSource e configurá-lo para usar o método `GetProductsAsPagedDataSource()` ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image3.png))

[![definir as listas suspensas nas guias atualizar, inserir e excluir para (nenhum)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image4.png)

**Figura 2**: definir as listas suspensas nas guias atualizar, inserir e excluir para (nenhum) ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image6.png))

Ao contrário de DataList, o Visual Studio não cria automaticamente um `ItemTemplate` para o controle Repeater depois de associá-lo a uma fonte de dados. Além disso, devemos adicionar esse `ItemTemplate` declarativamente, já que a marca inteligente s de controle Repeater não tem a opção Editar modelos encontrada nos s DataList. Deixe que os s usem o mesmo `ItemTemplate` do tutorial anterior, que exibia o nome, o fornecedor e a categoria do produto.

Depois de adicionar o `ItemTemplate`, a marcação declarativa de Repeater e ObjectDataSource s deve ser semelhante ao seguinte:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample1.aspx)]

A Figura 3 mostra essa página quando exibida por meio de um navegador.

[![cada nome, fornecedor e categoria do produto é exibido](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Figura 3**: cada nome, fornecedor e categoria do produto é exibido ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))

## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>Etapa 3: instruindo o ObjectDataSource a classificar os dados

Para classificar os dados exibidos no repetidor, precisamos informar o ObjectDataSource da expressão de classificação pela qual os dados devem ser classificados. Antes de o ObjectDataSource recuperar seus dados, ele primeiro dispara seu [evento`Selecting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), que fornece uma oportunidade para que possamos especificar uma expressão de classificação. O manipulador de eventos `Selecting` é passado a um objeto do tipo [`ObjectDataSourceSelectingEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), que tem uma propriedade chamada [`Arguments`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) do tipo [`DataSourceSelectArguments`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx). A classe `DataSourceSelectArguments` foi projetada para passar solicitações relacionadas a dados de um consumidor de dados para o controle da fonte de dados e inclui uma [propriedade`SortExpression`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

Para passar informações de classificação da página ASP.NET para o ObjectDataSource, crie um manipulador de eventos para o evento `Selecting` e use o seguinte código:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

O valor de *SortExpression* deve ser atribuído ao nome do campo de dados para classificar os dados (como ProductName). Não há nenhuma propriedade relacionada à direção de classificação, portanto, se você quiser classificar os dados em ordem decrescente, acrescente a cadeia de caracteres DESC ao valor *SortExpression* (como ProductName DESC).

Vá em frente e experimente alguns valores embutidos em código para *SortExpression* e teste os resultados em um navegador. Como mostra a Figura 4, ao usar ProductName DESC como *SortExpression*, os produtos são classificados por seu nome em ordem alfabética inversa.

[![os produtos são classificados por seu nome em ordem alfabética inversa](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Figura 4**: os produtos são classificados por seu nome em ordem alfabética inversa ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))

## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Etapa 4: criando a interface de classificação e lembrando a expressão de classificação e a direção

Ativar a classificação de suporte no GridView converte cada texto de cabeçalho s do campo classificável em um LinkButton que, quando clicado, classifica os dados de forma adequada. Essa interface de classificação faz sentido para o GridView, onde seus dados são dispostos de forma organizada em colunas. No entanto, para os controles DataList e Repeater, é necessária uma interface de classificação diferente. Uma interface de classificação comum para uma lista de dados (em oposição a uma grade de dados) é uma lista suspensa que fornece os campos pelos quais os dados podem ser classificados. Deixe que o s implemente uma interface desse tutorial.

Adicione um controle da Web DropDownList acima do repetidor de `SortableProducts` e defina sua propriedade `ID` como `SortBy`. Na janela Propriedades, clique nas reticências na propriedade `Items` para abrir o editor de coleção ListItem. Adicione `ListItem` s para classificar os dados pelos campos `ProductName`, `CategoryName`e `SupplierName`. Além disso, adicione um `ListItem` para classificar os produtos por seu nome em ordem alfabética inversa.

As propriedades de `Text` `ListItem` podem ser definidas como qualquer valor (como nome), mas as propriedades de `Value` devem ser definidas como o nome do campo de dados (como ProductName). Para classificar os resultados em ordem decrescente, acrescente a cadeia de caracteres DESC ao nome do campo de dados, como NomeDoProduto DESC.

![Adicionar um ListItem para cada um dos campos de dados classificável](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Figura 5**: adicionar um `ListItem` para cada um dos campos de dados classificável

Por fim, adicione um controle de botão da Web à direita de DropDownList. Defina seu `ID` como `RefreshRepeater` e sua propriedade `Text` para atualizar.

Depois de criar o `ListItem` s e adicionar o botão atualizar, a sintaxe declarativa e o botão s declarativo deve ser semelhante ao seguinte:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

Com a classificação de DropDownList concluída, precisamos atualizar o manipulador de eventos ObjectDataSource s `Selecting` para que ele use a propriedade de `Value` de `SortBy``ListItem` selecionada, em oposição a uma expressão de classificação embutida em código.

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample4.vb)]

Neste ponto, ao visitar a página pela primeira vez, os produtos serão classificados inicialmente pelo `ProductName` campo de dados, como s `SortBy` `ListItem` selecionado por padrão (veja a Figura 6). Selecionar uma opção de classificação diferente, como categoria, e clicar em atualizar, causará um postback e reclassificará os dados pelo nome da categoria, como mostra a Figura 7.

[![os produtos são classificados inicialmente por seu nome](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)

**Figura 6**: os produtos são classificados inicialmente por seu nome ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image16.png))

[![os produtos agora estão classificados por categoria](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)

**Figura 7**: os produtos agora são classificados por categoria ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image19.png))

> [!NOTE]
> Clicar no botão atualizar faz com que os dados sejam automaticamente reclassificados porque o estado de exibição do repetidor foi desabilitado, fazendo com que o repetidor reassocie a sua fonte de dados em cada postback. Se você tiver deixado o estado de exibição do repetidor habilitado, alterar a lista suspensa classificação não terá nenhum efeito na ordem de classificação. Para corrigir isso, crie um manipulador de eventos para o botão atualizar s `Click` evento e reassocie o repetidor à sua fonte de dados (chamando o método s `DataBind()` do repetidor).

## <a name="remembering-the-sort-expression-and-direction"></a>Lembrando a expressão de classificação e a direção

Ao criar um DataList ou Repeater classificável em uma página em que os postbacks relacionados à não classificação possam ocorrer, é imperativo que a expressão de classificação e a direção sejam lembradas entre os postbacks. Por exemplo, imagine que atualizamos o repetidor neste tutorial para incluir um botão de exclusão com cada produto. Quando o usuário clica no botão excluir, executamos um código para excluir o produto selecionado e, em seguida, revinculo os dados ao repetidor. Se os detalhes de classificação não persistirem no postback, os dados exibidos na tela serão revertidos para a ordem de classificação original.

Para este tutorial, a DropDownList salva implicitamente a expressão de classificação e a direção em seu estado de exibição para nós. Se estivessemos usando uma interface de classificação diferente com, digamos, LinkButtons que forneciam as várias opções de classificação que precisamos tomar para se lembrar da ordem de classificação entre postbacks. Isso pode ser feito armazenando os parâmetros de classificação no estado de exibição da página s, incluindo o parâmetro de classificação na QueryString ou por meio de alguma outra técnica de persistência de estado.

Exemplos futuros neste tutorial exploram como manter os detalhes de classificação no estado de exibição da página s.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>Etapa 5: adicionar suporte de classificação a um DataList que usa paginação padrão

No [tutorial anterior](paging-report-data-in-a-datalist-or-repeater-control-vb.md) , examinamos como implementar a paginação padrão com um DataList. Deixe que o s estenda este exemplo anterior para incluir a capacidade de classificar os dados paginados. Comece abrindo as páginas `SortingWithDefaultPaging.aspx` e `Paging.aspx` na pasta `PagingSortingDataListRepeater`. Na página `Paging.aspx`, clique no botão origem para exibir a marcação declarativa de s de página. Copie o texto selecionado (veja a Figura 8) e cole-o na marcação declarativa de `SortingWithDefaultPaging.aspx` entre as marcas de `<asp:Content>`.

[![replicar a marcação declarativa nas marcas &lt;ASP: content&gt; de paging. aspx para SortingWithDefaultPaging. aspx](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)

**Figura 8**: replicar a marcação declarativa nas marcas de `<asp:Content>` de `Paging.aspx` para `SortingWithDefaultPaging.aspx` ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image22.png))

Depois de copiar a marcação declarativa, copie os métodos e as propriedades na classe `Paging.aspx` s code-behind da página para a classe code-behind para `SortingWithDefaultPaging.aspx`. Em seguida, Reserve um momento para exibir a página de `SortingWithDefaultPaging.aspx` em um navegador. Ele deve apresentar a mesma funcionalidade e a mesma aparência que `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Aprimorando o ProductsBLL para incluir um método de paginação e classificação padrão

No tutorial anterior, criamos um método `GetProductsAsPagedDataSource(pageIndex, pageSize)` na classe `ProductsBLL` que retornava um objeto `PagedDataSource`. Este objeto de `PagedDataSource` foi populado com *todos* os produtos (por meio do método de `GetProducts()` de BLL), mas quando ligado ao DataList, somente os registros correspondentes aos parâmetros de entrada *pageIndex* e *PageSize* especificados foram exibidos.

Anteriormente neste tutorial, adicionamos suporte à classificação, especificando a expressão de classificação do manipulador de eventos ObjectDataSource s `Selecting`. Isso funciona bem quando o ObjectDataSource retorna um objeto que pode ser classificado, como o `ProductsDataTable` retornado pelo método `GetProducts()`. No entanto, o objeto `PagedDataSource` retornado pelo método `GetProductsAsPagedDataSource` não oferece suporte à classificação de sua fonte de dados interna. Em vez disso, precisamos classificar os resultados retornados do método `GetProducts()` *antes* de colocá-lo no `PagedDataSource`.

Para fazer isso, crie um novo método na classe `ProductsBLL` `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Para classificar o `ProductsDataTable` retornado pelo método `GetProducts()`, especifique a propriedade `Sort` de seu `DataTableView`padrão:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

O método `GetProductsSortedAsPagedDataSource` difere apenas um pouco do método `GetProductsAsPagedDataSource` criado no tutorial anterior. Em particular, `GetProductsSortedAsPagedDataSource` aceita um parâmetro de entrada adicional `sortExpression` e atribui esse valor à propriedade `Sort` do `ProductDataTable` s `DefaultView`. Algumas linhas de código mais tarde, a `PagedDataSource` objetos de fonte de os s é atribuída ao `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>Chamando o método GetProductsSortedAsPagedDataSource e especificando o valor para o parâmetro de entrada SortExpression

Com o método `GetProductsSortedAsPagedDataSource` concluído, a próxima etapa é fornecer o valor para esse parâmetro. O ObjectDataSource no `SortingWithDefaultPaging.aspx` está atualmente configurado para chamar o método `GetProductsAsPagedDataSource` e passa os dois parâmetros de entrada por meio de seus dois `QueryStringParameters`, que são especificados na coleção `SelectParameters`. Esses dois `QueryStringParameters` indicam que a origem dos parâmetros do método `GetProductsAsPagedDataSource` s *pageIndex* e *PageSize* são provenientes dos campos QueryString `pageIndex` e `pageSize`.

Atualize a Propriedade ObjectDataSource s `SelectMethod` para que ela invoque o novo método `GetProductsSortedAsPagedDataSource`. Em seguida, adicione um novo `QueryStringParameter` para que o parâmetro de entrada *SortExpression* seja acessado no campo QueryString `sortExpression`. Defina o `QueryStringParameter` s `DefaultValue` como ProductName.

Após essas alterações, a marcação declarativa s de ObjectDataSource deve ser semelhante a:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample6.aspx)]

Neste ponto, a página `SortingWithDefaultPaging.aspx` classificará seus resultados em ordem alfabética pelo nome do produto (consulte a Figura 9). Isso ocorre porque, por padrão, um valor de NomeDoProduto é passado como o parâmetro de `GetProductsSortedAsPagedDataSource` do método s *SortExpression* .

[![por padrão, os resultados são classificados por NomeDoProduto](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)

**Figura 9**: por padrão, os resultados são classificados por `ProductName` ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image25.png))

Se você adicionar manualmente um campo `sortExpression` QueryString, como `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` os resultados serão classificados pelo `sortExpression`especificado. No entanto, esse parâmetro de `sortExpression` não é incluído na QueryString ao se mover para uma página de dados diferente. Na verdade, clicar nos botões da próxima ou da última página nos levará de volta para `Paging.aspx`! Além disso, atualmente, não há nenhuma interface de classificação. A única maneira como um usuário pode alterar a ordem de classificação dos dados paginados é manipulando a QueryString diretamente.

## <a name="creating-the-sorting-interface"></a>Criando a interface de classificação

Primeiro, precisamos atualizar o método `RedirectUser` para enviar o usuário para `SortingWithDefaultPaging.aspx` (em vez de `Paging.aspx`) e incluir o valor de `sortExpression` na QueryString. Também devemos adicionar um nível de página somente leitura chamado `SortExpression` propriedade. Essa propriedade, semelhante às propriedades `PageIndex` e `PageSize` criadas no tutorial anterior, retorna o valor do campo `sortExpression` QueryString, se existir, e o valor padrão (ProductName), caso contrário.

Atualmente, o método `RedirectUser` aceita apenas um único parâmetro de entrada o índice da página a ser exibido. No entanto, pode haver ocasiões em que desejamos redirecionar o usuário para uma determinada página de dados usando uma expressão de classificação diferente das s especificadas na QueryString. Em alguns instantes, criaremos a interface de classificação para essa página, que incluirá uma série de controles de botão da Web para classificar os dados por uma coluna especificada. Quando um desses botões é clicado, queremos redirecionar o usuário passando o valor de expressão de classificação apropriado. Para fornecer essa funcionalidade, crie duas versões do método `RedirectUser`. O primeiro deve aceitar apenas o índice de página a ser exibido, enquanto o segundo aceita o índice de página e a expressão de classificação.

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

No primeiro exemplo deste tutorial, criamos uma interface de classificação usando uma DropDownList. Para este exemplo, vamos usar os controles da Web de três botões posicionados acima do DataList para classificar por `ProductName`, um para `CategoryName`e outro para `SupplierName`. Adicione os três controles de botão da Web, definindo suas propriedades `ID` e `Text` adequadamente:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample8.aspx)]

Em seguida, crie um manipulador de eventos `Click` para cada um. Os manipuladores de eventos devem chamar o método `RedirectUser`, retornando o usuário para a primeira página usando a expressão de classificação apropriada.

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Ao visitar a página pela primeira vez, os dados são classificados pelo nome do produto em ordem alfabética (consulte novamente a Figura 9). Clique no botão Avançar para avançar para a segunda página de dados e, em seguida, clique no botão Classificar por categoria. Isso nos retorna à primeira página de dados, classificados por nome de categoria (veja a Figura 10). Da mesma forma, clicar no botão Classificar por fornecedor classifica os dados por fornecedor, começando na primeira página de dados. A opção de classificação é lembrada à medida que os dados são paginados. A Figura 11 mostra a página após a classificação por categoria e, em seguida, avançando para a décima terceira página de dados.

[![os produtos são classificados por categoria](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)

**Figura 10**: os produtos são classificados por categoria ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image28.png))

[![a expressão de classificação é lembrada ao paginar os dados](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image29.png)

**Figura 11**: a expressão de classificação é lembrada ao paginar os dados ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image31.png))

## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>Etapa 6: paginação personalizada por meio de registros em um repetidor

O exemplo DataList examinou na etapa 5 páginas por meio de seus dados usando a técnica ineficiente de paginação padrão. Ao paginar por meio de quantidades de dados suficientemente grandes, é imperativo que a paginação personalizada seja usada. De volta à [paginação eficiente por meio de grandes quantidades de dados e na](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) classificação de tutoriais de [dados paginados personalizados](../paging-and-sorting/sorting-custom-paged-data-vb.md) , examinamos as diferenças entre paginação padrão e personalizada e métodos criados na BLL para utilização de paginação personalizada e classificação de dados da página personalizada. Em particular, nesses dois tutoriais anteriores, adicionamos os três métodos a seguir à classe `ProductsBLL`:

- `GetProductsPaged(startRowIndex, maximumRows)` retorna um subconjunto específico de registros começando em *StartRowIndex* e não excedendo *MaximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)` retorna um subconjunto específico de registros classificados pelo parâmetro de entrada *SortExpression* especificado.
- `TotalNumberOfProducts()` fornece o número total de registros na tabela de banco de dados `Products`.

Esses métodos podem ser usados para paginar e classificar com eficiência os dados usando um controle DataList ou Repeater. Para ilustrar isso, vamos começar criando um controle Repeater com suporte de paginação personalizado; em seguida, adicionaremos recursos de classificação.

Abra a página `SortingWithCustomPaging.aspx` na pasta `PagingSortingDataListRepeater` e adicione um repetidor à página, definindo sua propriedade `ID` como `Products`. Na marca inteligente repetida de s, crie um novo ObjectDataSource chamado `ProductsDataSource`. Configure-o para selecionar seus dados do método de `GetProductsPaged` da classe `ProductsBLL`.

[![configurar o ObjectDataSource para usar o método GetProductsPaged da classe ProductsBLL](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image32.png)

**Figura 12**: configurar o ObjectDataSource para usar o método de `GetProductsPaged` da classe `ProductsBLL` ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image34.png))

Defina as listas suspensas nas guias atualizar, inserir e excluir como (nenhum) e, em seguida, clique no botão Avançar. O assistente para configurar fonte de dados agora solicita as fontes dos parâmetros de entrada do `GetProductsPaged` método s *StartRowIndex* e *MaximumRows* . Na realidade, esses parâmetros de entrada são ignorados. Em vez disso, os valores *StartRowIndex* e *MaximumRows* serão passados por meio da propriedade `Arguments` no manipulador de eventos ObjectDataSource s `Selecting`, assim como especificamos o *SortExpression* neste tutorial s First demo. Portanto, deixe as listas suspensas de origem do parâmetro no assistente definidas em nenhum.

[![deixar as fontes de parâmetros definidas como None](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image35.png)

**Figura 13**: Deixe as fontes de parâmetro definidas como nenhum ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image37.png))

> [!NOTE]
> Não *defina a* Propriedade ObjectDataSource s `EnablePaging` como `true`. Isso fará com que o ObjectDataSource inclua automaticamente seus próprios parâmetros *StartRowIndex* e *MaximumRows* na lista de parâmetros existentes do `SelectMethod` s. A propriedade `EnablePaging` é útil ao associar dados paginados personalizados a um controle GridView, DetailsView ou FormView, pois esses controles esperam determinado comportamento do ObjectDataSource que s está disponível somente quando `EnablePaging` propriedade é `true`. Como precisamos adicionar manualmente o suporte de paginação para DataList e Repeater, deixe essa propriedade definida como `false` (o padrão), como iremos distortar a funcionalidade necessária diretamente em nossa página ASP.NET.

Por fim, defina o repetidor s `ItemTemplate` para que o nome, a categoria e o fornecedor do produto sejam mostrados. Após essas alterações, a sintaxe declarativa de Repeater e ObjectDataSource s deve ser semelhante ao seguinte:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample10.aspx)]

Reserve um tempo para visitar a página por meio de um navegador e observar que nenhum registro é retornado. Isso ocorre porque ainda podemos especificar os valores de parâmetro *StartRowIndex* e *MaximumRows* ; Portanto, os valores de 0 estão sendo passados para ambos. Para especificar esses valores, crie um manipulador de eventos para o evento de `Selecting` ObjectDataSource s e defina esses valores de parâmetros programaticamente para valores embutidos em código de 0 e 5, respectivamente:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample11.vb)]

Com essa alteração, a página, quando exibida por meio de um navegador, mostra os cinco primeiros produtos.

[![os primeiros cinco registros são exibidos](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image38.png)

**Figura 14**: os primeiros cinco registros são exibidos ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image40.png))

> [!NOTE]
> Os produtos listados na Figura 14 acontecem para serem classificados por nome de produto porque o `GetProductsPaged` procedimento armazenado que executa a consulta de paginação personalizada eficiente ordena os resultados por `ProductName`.

Para permitir que o usuário percorra as páginas, precisamos acompanhar o índice de linha inicial e o máximo de linhas e lembrar esses valores entre postbacks. No exemplo de paginação padrão, usamos campos QueryString para persistir esses valores; para esta demonstração, vamos manter essas informações no estado de exibição da página s. Crie as duas propriedades a seguir:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample12.vb)]

Em seguida, atualize o código no manipulador de eventos de seleção para que ele use as propriedades `StartRowIndex` e `MaximumRows` em vez dos valores embutidos em código de 0 e 5:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample13.vb)]

Neste ponto, nossa página ainda mostra apenas os cinco primeiros registros. No entanto, com essas propriedades no lugar, estamos prontos para criar nossa interface de paginação.

## <a name="adding-the-paging-interface"></a>Adicionando a interface de paginação

Vamos usar a mesma primeira, anterior, próxima, última interface de paginação usada no exemplo de paginação padrão, incluindo o controle de rótulo da Web que exibe qual página de dados está sendo exibida e quantas páginas de total existem. Adicione os quatro botões de controles da Web e o rótulo abaixo do repetidor.

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample14.aspx)]

Em seguida, crie `Click` manipuladores de eventos para os quatro botões. Quando um desses botões é clicado, precisamos atualizar o `StartRowIndex` e reassociar os dados ao repetidor. O código para os botões primeiros, anterior e próximo é simples o suficiente, mas, para o último botão, como determinamos o índice de linha inicial da última página de dados? Para computar esse índice, bem como ser capaz de determinar se os botões Next e Last devem ser habilitados, precisamos saber quantos registros no total estão sendo paginados. Podemos determinar isso chamando o método de `TotalNumberOfProducts()` da classe `ProductsBLL`. Deixe que o s Crie uma propriedade de nível de página somente leitura chamada `TotalRowCount` que retorna os resultados do método `TotalNumberOfProducts()`:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample15.vb)]

Com essa propriedade, agora podemos determinar o índice de linha inicial da última página. Especificamente, o resultado inteiro do `TotalRowCount` menos 1 dividido por `MaximumRows`, multiplicado por `MaximumRows`. Agora podemos escrever os manipuladores de eventos `Click` para os quatro botões de interface de paginação:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample16.vb)]

Por fim, precisamos desabilitar os botões primeiros e anteriores na interface de paginação ao exibir a primeira página de dados e os botões Next e Last ao exibir a última página. Para fazer isso, adicione o seguinte código ao manipulador de eventos ObjectDataSource s `Selecting`:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample17.vb)]

Depois de adicionar esses `Click` manipuladores de eventos e o código para habilitar ou desabilitar os elementos da interface de paginação com base no índice de linha inicial atual, teste a página em um navegador. Como a Figura 15 ilustra, ao visitar a página pela primeira vez, os botões primeiros e anteriores serão desabilitados. Clicar em Avançar mostra a segunda página de dados, enquanto clicar em último exibe a página final (consulte as figuras 16 e 17). Ao exibir a última página de dados, os botões Avançar e último são desabilitados.

[![os botões anterior e último estão desabilitados ao exibir a primeira página de produtos](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image41.png)

**Figura 15**: os botões anterior e último são desabilitados ao exibir a primeira página de produtos ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image43.png))

[![a segunda página de produtos é exibida](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image44.png)

**Figura 16**: a segunda página de produtos é exibida ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image46.png))

[![clicar em último exibe a página final dos dados](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image47.png)

**Figura 17**: clicar em último exibe a página final dos dados ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image49.png))

## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>Etapa 7: incluindo suporte à classificação com o repetidor personalizado paginado

Agora que a paginação personalizada foi implementada, estamos prontos para incluir suporte à classificação. O método `GetProductsPagedAndSorted` da classe `ProductsBLL` tem os mesmos parâmetros de entrada *StartRowIndex* e *MaximumRows* como `GetProductsPaged`, mas permite um parâmetro de entrada *SortExpression* adicional. Para usar o método `GetProductsPagedAndSorted` do `SortingWithCustomPaging.aspx`, precisamos executar as seguintes etapas:

1. Altere a Propriedade ObjectDataSource s `SelectMethod` de `GetProductsPaged` para `GetProductsPagedAndSorted`.
2. Adicione um objeto *sortExpression* `Parameter` à coleção ObjectDataSource s `SelectParameters`.
3. Crie uma propriedade de `SortExpression` privada em nível de página que persiste seu valor entre postbacks por meio do estado de exibição da página s.
4. Atualize o manipulador de eventos ObjectDataSource s `Selecting` para atribuir o parâmetro ObjectDataSource s *SortExpression* ao valor da propriedade `SortExpression` no nível de página.
5. Crie a interface de classificação.

Comece atualizando a Propriedade ObjectDataSource s `SelectMethod` e adicionando uma `Parameter`*SortExpression* . Verifique se a propriedade *sortExpression* `Parameter` s `Type` está definida como `String`. Depois de concluir essas duas primeiras tarefas, a marcação declarativa de ObjectDataSource s deve ser parecida com a seguinte:

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample18.aspx)]

Em seguida, precisamos de um nível de página `SortExpression` propriedade cujo valor é serializado para exibir o estado. Se nenhum valor de expressão de classificação tiver sido definido, use ProductName como o padrão:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample19.vb)]

Antes de o ObjectDataSource invocar o método `GetProductsPagedAndSorted`, precisamos definir o `Parameter` *SortExpression* como o valor da propriedade `SortExpression`. No manipulador de eventos `Selecting`, adicione a seguinte linha de código:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample20.vb)]

Tudo o que resta é implementar a interface de classificação. Como fizemos no último exemplo, vamos ter a interface de classificação implementada usando os três controles de botão da Web que permitem ao usuário classificar os resultados por nome do produto, categoria ou fornecedor.

[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample21.aspx)]

Crie `Click` manipuladores de eventos para esses três controles de botão. No manipulador de eventos, redefina o `StartRowIndex` como 0, defina o `SortExpression` como o valor apropriado e reassocie os dados ao repetidor:

[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample22.vb)]

Isso é tudo! Embora haja várias etapas para obter a paginação e a classificação personalizadas implementadas, as etapas eram muito semelhantes às necessárias para paginação padrão. A Figura 18 mostra os produtos ao exibir a última página de dados quando classificados por categoria.

[![a última página de dados, classificada por categoria, é exibida](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image50.png)

**Figura 18**: a última página de dados, classificada por categoria, é exibida ([clique para exibir a imagem em tamanho normal](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image52.png))

> [!NOTE]
> Nos exemplos anteriores, ao classificar pelo fornecedor Supplier, foi usado como a expressão de classificação. No entanto, para a implementação de paginação personalizada, precisamos usar CompanyName. Isso ocorre porque o procedimento armazenado responsável pela implementação de paginação personalizada `GetProductsPagedAndSorted` passa a expressão de classificação para a palavra-chave `ROW_NUMBER()`, a palavra-chave `ROW_NUMBER()` requer o nome real da coluna em vez de um alias. Portanto, devemos usar `CompanyName` (o nome da coluna na tabela `Suppliers`) em vez do alias usado na consulta `SELECT` (`SupplierName`) para a expressão de classificação.

## <a name="summary"></a>Resumo

Nem o DataList nem o Repeater oferecem suporte interno à classificação, mas com um pouco de código e uma interface de classificação personalizada, essa funcionalidade pode ser adicionada. Ao implementar a classificação, mas não a paginação, a expressão de classificação pode ser especificada por meio do objeto `DataSourceSelectArguments` passado para o método ObjectDataSource s `Select`. Esta `DataSourceSelectArguments` objeto s `SortExpression` propriedade pode ser atribuída no manipulador de eventos ObjectDataSource s `Selecting`.

Para adicionar recursos de classificação a um DataList ou Repeater que já fornece suporte à paginação, a abordagem mais fácil é personalizar a camada de lógica de negócios para incluir um método que aceite uma expressão de classificação. Essas informações podem então ser passadas por meio de um parâmetro no `SelectParameters`ObjectDataSource s.

Este tutorial conclui nosso exame de paginação e classificação com os controles DataList e Repeater. Nosso tutorial de próximo e último examinará como adicionar controles de botão da Web aos modelos DataList e Repeater para fornecer algumas funcionalidades personalizadas e iniciadas pelo usuário por item.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de cliente potencial para este tutorial foi David Suru. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
