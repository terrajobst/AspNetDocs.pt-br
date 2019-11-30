---
uid: web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
title: Filtragem de mestre/detalhes em duas páginasC#() | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como separar um relatório mestre/detalhado em duas páginas. Na página ' mestre ', usamos um controle Repeater para renderizar uma lista de categ...
ms.author: riande
ms.date: 10/30/2010
ms.assetid: 68b8c023-92fa-4df6-9563-1764e16e4b04
msc.legacyurl: /web-forms/overview/data-access/filtering-scenarios-with-the-datalist-and-repeater/master-detail-filtering-acess-two-pages-datalist-cs
msc.type: authoredcontent
ms.openlocfilehash: 545b24a66476c55aff88ac62d3a6528105fea6c0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74631361"
---
# <a name="masterdetail-filtering-across-two-pages-c"></a>Filtragem mestre/detalhes em duas páginas (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_34_CS.exe) ou [baixar PDF](master-detail-filtering-acess-two-pages-datalist-cs/_static/datatutorial34cs1.pdf)

> Neste tutorial, veremos como separar um relatório mestre/detalhado em duas páginas. Na página "mestre", usamos um controle Repeater para renderizar uma lista de categorias que, quando clicada, levará o usuário para a página "detalhes", em que um DataList de duas colunas mostra os produtos que pertencem à categoria selecionada.

## <a name="introduction"></a>Introdução

No [tutorial anterior](master-detail-filtering-with-a-dropdownlist-datalist-cs.md) , vimos como exibir relatórios mestre/detalhados em uma única página da Web usando DropDownLists para exibir os registros "mestre" e um DataList para exibir os "detalhes". Outro padrão comum usado para relatórios mestre/de detalhes é ter os registros mestres em uma página da Web e os detalhes em outra. No tutorial de [filtragem mestre/detalhes anterior em duas páginas](../masterdetail/master-detail-filtering-across-two-pages-cs.md) , examinamos esse padrão usando um GridView para exibir todos os fornecedores no sistema. Esse GridView incluía um HyperLinkField, que é renderizado como um link para uma segunda página, passando o `SupplierID` na QueryString. A segunda página usou um GridView para listar os produtos fornecidos pelo fornecedor selecionado.

Esses relatórios mestre/detalhes de duas páginas também podem ser realizados usando Controles DataList e Repeater. A única diferença é que nem o DataList nem o Repeater fornecem suporte para o controle HyperLinkField. Em vez disso, devemos adicionar um controle Web de hiperlink ou um elemento HTML de ancoragem (`<a>`) dentro do `ItemTemplate`do controle. A propriedade `NavigateUrl` do hiperlink ou o atributo `href` da âncora podem ser personalizados usando abordagens declarativas ou programáticas.

Neste tutorial, exploraremos um exemplo que lista as categorias em uma lista com marcadores em uma página usando um controle Repeater. Cada item de lista incluirá o nome e a descrição da categoria, com o nome da categoria exibido como um link para uma segunda página. Clicar nesse link irá encaixar o usuário para a segunda página, em que um DataList mostrará os produtos que pertencem à categoria selecionada.

## <a name="step-1-displaying-the-categories-in-a-bulleted-list"></a>Etapa 1: exibindo as categorias em uma lista com marcadores

A primeira etapa na criação de qualquer relatório mestre/detalhado é começar exibindo os registros "mestre". Portanto, nossa primeira tarefa é exibir as categorias na página "mestre". Abra a página `CategoryListMaster.aspx` na pasta `DataListRepeaterFiltering`, adicione um controle Repeater e, na marca inteligente, opte por adicionar um novo ObjectDataSource. Configure o novo ObjectDataSource para que ele acesse seus dados do método `GetCategories` da classe `CategoriesBLL` (consulte a Figura 1).

[![configurar o ObjectDataSource para usar o método GetCategories da classe CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image2.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image1.png)

**Figura 1**: configurar o ObjectDataSource para usar o método `GetCategories` da classe `CategoriesBLL` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-cs/_static/image3.png))

Em seguida, defina os modelos do repetidor de modo que ele exiba cada nome de categoria e descrição como um item em uma lista com marcadores. Vamos ainda não se preocupar em ter cada link de categoria na página de detalhes. O seguinte mostra a marcação declarativa para o Repeater e ObjectDataSource:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample1.aspx)]

Com essa marcação concluída, Reserve um momento para exibir nosso progresso por meio de um navegador. Como mostra a Figura 2, o repetidor é renderizado como uma lista com marcadores mostrando o nome e a descrição de cada categoria.

[![cada categoria é exibida como um item de lista com marcadores](master-detail-filtering-acess-two-pages-datalist-cs/_static/image5.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image4.png)

**Figura 2**: cada categoria é exibida como um item de lista com marcadores ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-cs/_static/image6.png))

## <a name="step-2-turning-the-category-name-into-a-link-to-the-details-page"></a>Etapa 2: transformar o nome da categoria em um link para a página de detalhes

Para permitir que um usuário exiba as informações de "detalhes" de uma determinada categoria, precisamos adicionar um link para cada item de lista com marcadores que, quando clicado, levará o usuário para a segunda página (`ProductsForCategoryDetails.aspx`). Essa segunda página exibirá os produtos da categoria selecionada usando um DataList. Para determinar a categoria cujo link foi clicado, precisamos passar a `CategoryID` da categoria clicada para a segunda página por meio de um mecanismo. A maneira mais simples e direta de transferir dados escalares de uma página para outra é por meio da QueryString, que é a opção que usaremos neste tutorial. Em particular, a página `ProductsForCategoryDetails.aspx` esperará que o valor de *`categoryID`* selecionado seja passado por meio de um campo querystring chamado `CategoryID`. Por exemplo, para exibir os produtos da categoria bebidas, que tem um `CategoryID` de 1, um usuário visita `ProductsForCategoryDetails.aspx?CategoryID=1`.

Para criar um hiperlink para cada item de lista com marcadores no repetidor, precisamos adicionar um controle Web de hiperlink ou um elemento de âncora HTML (`<a>`) ao `ItemTemplate`. Em cenários em que o hiperlink é exibido da mesma forma para cada linha, qualquer abordagem será suficiente. Para repetidores, prefiro usar o elemento Anchor. Para usar o elemento Anchor, atualize o ItemTemplate do Repeater para:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample2.aspx)]

Observe que o `CategoryID` pode ser injetado diretamente no atributo `href` do elemento de âncora; no entanto, para fazer isso, você deve delimitar o valor do atributo `href` com apóstrofos (e aspas de nota), já que o método `Eval` dentro do atributo `href` delimita sua cadeia de caracteres (`"CategoryID"`) com aspas. Como alternativa, um controle de hiperlink da Web pode ser usado em vez disso:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample3.aspx)]

Observe como a parte estática da URL — `ProductsForCategoryDetails.aspx?CategoryID` — é acrescentada ao resultado de `Eval("CategoryID")` diretamente na sintaxe de DataBinding usando a concatenação de cadeia de caracteres.

Um benefício de usar o controle HyperLink é que ele pode ser acessado programaticamente por meio do manipulador de eventos `ItemDataBound` do Repeater, se necessário. Por exemplo, talvez você queira exibir o nome da categoria como texto, e não como um link para categorias sem produtos associados. Essa verificação pode ser realizada programaticamente no manipulador de eventos `ItemDataBound`; para categorias sem produtos associados, a propriedade de `NavigateUrl` do hiperlink pode ser definida como uma cadeia de caracteres em branco, resultando na renderização desse nome de categoria específico como texto sem formatação (em vez de um link). Consulte a [formatação do DataList e do repetidor com base](../displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs.md) no tutorial de dados para obter mais informações sobre como formatar o conteúdo do DataList e do repetidor com base na lógica programática por meio do manipulador de eventos `ItemDataBound`.

Se você estiver acompanhando, sinta-se à vontade para usar o elemento de ancoragem ou a abordagem de controle de hiperlink em sua página. Independentemente da abordagem, ao exibir a página por meio de um navegador, cada nome de categoria deve ser renderizado como um link para `ProductsForCategoryDetails.aspx`, passando o valor de `CategoryID` aplicável (consulte a Figura 3).

[![os nomes de categoria agora são vinculados a ProductsForCategoryDetails. aspx](master-detail-filtering-acess-two-pages-datalist-cs/_static/image8.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image7.png)

**Figura 3**: os nomes de categoria agora são vinculados a `ProductsForCategoryDetails.aspx` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-cs/_static/image9.png))

## <a name="step-3-listing-the-products-that-belong-to-the-selected-category"></a>Etapa 3: listando os produtos que pertencem à categoria selecionada

Com a página de `CategoryListMaster.aspx` concluída, estamos prontos para transformar nossa atenção em implementar a página "detalhes" `ProductsForCategoryDetails.aspx`. Abra esta página, arraste uma DataList da caixa de ferramentas para o designer e defina sua propriedade `ID` como `ProductsInCategory`. Em seguida, na marca inteligente do DataList, escolha Adicionar um novo ObjectDataSource à página, nomeando-o `ProductsInCategoryDataSource`. Configure-o de modo que ele chame o método `GetProductsByCategoryID(categoryID)` da classe `ProductsBLL`; Defina as listas suspensas nas guias inserir, atualizar e excluir como (nenhum).

[![configurar o ObjectDataSource para usar o método GetProductsByCategoryID (categoryID) da classe ProductsBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image11.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image10.png)

**Figura 4**: configurar o ObjectDataSource para usar o método `GetProductsByCategoryID(categoryID)` da classe `ProductsBLL` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-cs/_static/image12.png))

Como o método `GetProductsByCategoryID(categoryID)` aceita um parâmetro de entrada ( *`categoryID`* ), o assistente escolher fonte de dados nos oferece uma oportunidade para especificar a origem do parâmetro. Defina a origem do parâmetro como QueryString usando o `CategoryID`QueryStringfield.

[![usar o campo CódigoDaCategoria de QueryString como a origem do parâmetro](master-detail-filtering-acess-two-pages-datalist-cs/_static/image14.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image13.png)

**Figura 5**: Use o campo QueryString `CategoryID` como a origem do parâmetro ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-cs/_static/image15.png))

Como vimos nos tutoriais anteriores, depois de concluir o assistente escolher fonte de dados, o Visual Studio cria automaticamente um `ItemTemplate` para o DataList que lista cada nome e valor de campo de dados. Substitua esse modelo por um que liste apenas o nome, o fornecedor e o preço do produto. Além disso, defina a propriedade `RepeatColumns` do DataList como 2. Após essas alterações, a marcação declarativa de DataList e ObjectDataSource deve ser semelhante ao seguinte:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample4.aspx)]

Para exibir esta página em ação, inicie na página `CategoryListMaster.aspx`; em seguida, clique em um link na lista categorias com marcadores. Isso levará você para `ProductsForCategoryDetails.aspx`, passando o `CategoryID` por meio da QueryString. O `ProductsInCategoryDataSource` ObjectDataSource em `ProductsForCategoryDetails.aspx` obterá apenas esses produtos para a categoria especificada e os exibirá no DataList, que renderiza dois produtos por linha. A Figura 6 mostra uma captura de tela de `ProductsForCategoryDetails.aspx` ao exibir as bebidas.

[![as bebidas são exibidas, duas por linha](master-detail-filtering-acess-two-pages-datalist-cs/_static/image17.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image16.png)

**Figura 6**: as bebidas são exibidas, duas por linha ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-cs/_static/image18.png))

## <a name="step-4-displaying-category-information-on-productsforcategorydetailsaspx"></a>Etapa 4: exibindo informações de categoria em ProductsForCategoryDetails. aspx

Quando um usuário clica em uma categoria no `CategoryListMaster.aspx`, ele é levado para `ProductsForCategoryDetails.aspx` e mostra os produtos que pertencem à categoria selecionada. No entanto, em `ProductsForCategoryDetails.aspx` não há indicações visuais sobre qual categoria foi selecionada. Um usuário que pretendia clicar em bebidas, mas clicou acidentalmente em condiments, não tem como perceber o erro assim que atingir `ProductsForCategoryDetails.aspx`. Para aliviar esse possível problema, podemos exibir informações sobre a categoria selecionada — seu nome e descrição — na parte superior da página de `ProductsForCategoryDetails.aspx`.

Para fazer isso, adicione um FormView acima do controle Repeater em `ProductsForCategoryDetails.aspx`. Em seguida, adicione uma nova ObjectDataSource à página a partir da marca inteligente do FormView chamada `CategoryDataSource` e configure-a para usar o método `GetCategoryByCategoryID(categoryID)` da classe `CategoriesBLL`.

[![acessar informações sobre a categoria por meio do método GetCategoryByCategoryID (categoryID) da classe CategoriesBLL](master-detail-filtering-acess-two-pages-datalist-cs/_static/image20.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image19.png)

**Figura 7**: acessar informações sobre a categoria por meio do método `GetCategoryByCategoryID(categoryID)` da classe `CategoriesBLL` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-cs/_static/image21.png))

Assim como com o `ProductsInCategoryDataSource` ObjectDataSource adicionado na etapa 3, o assistente para configurar fonte de dados do `CategoryDataSource`solicita uma fonte para o parâmetro de entrada do método de `GetCategoryByCategoryID(categoryID)`. Use exatamente as mesmas configurações de antes, definindo a origem do parâmetro como QueryString e o valor QueryStringfield como `CategoryID` (consulte novamente a Figura 5).

Depois de concluir o assistente, o Visual Studio cria automaticamente um `ItemTemplate`, `EditItemTemplate`e `InsertItemTemplate` para o FormView. Como estamos fornecendo uma interface somente leitura, sinta-se à vontade para remover o `EditItemTemplate` e `InsertItemTemplate`. Além disso, sinta-se à vontade para personalizar o `ItemTemplate`do FormView. Depois de remover os modelos supérfluos e personalizar o ItemTemplate, a marcação declarativa de FormView e ObjectDataSource deve ser semelhante ao seguinte:

[!code-aspx[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample5.aspx)]

A Figura 8 mostra uma captura de tela ao exibir esta página por meio de um navegador.

> [!NOTE]
> Além do FormView, também adicionei um controle HyperLink acima do FormView que levará o usuário de volta para a lista de categorias (`CategoryListMaster.aspx`). Fique à vontade para posicionar esse link em outro lugar ou omiti-lo completamente.

[![informações de categoria agora são exibidas na parte superior da página](master-detail-filtering-acess-two-pages-datalist-cs/_static/image23.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image22.png)

**Figura 8**: as informações de categoria agora são exibidas na parte superior da página ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-cs/_static/image24.png))

## <a name="step-5-displaying-a-message-if-no-products-belong-to-the-selected-category"></a>Etapa 5: exibindo uma mensagem se nenhum produto pertencer à categoria selecionada

A página de `CategoryListMaster.aspx` lista todas as categorias no sistema, independentemente de os produtos associados estarem. Se um usuário clicar em uma categoria sem produtos associados, o DataList em `ProductsForCategoryDetails.aspx` não será renderizado, pois sua fonte de dados não terá nenhum item. Como vimos nos tutoriais anteriores, o GridView fornece uma propriedade `EmptyDataText` que pode ser usada para especificar uma mensagem de texto a ser exibida se não houver nenhum registro em sua fonte de dados. Infelizmente, nem o DataList nem o Repeater têm tal propriedade.

Para exibir uma mensagem informando ao usuário que não há produtos correspondentes para a categoria selecionada, precisamos adicionar um controle rótulo à página cuja propriedade `Text` seja atribuída à mensagem para ser exibida no caso de não haver nenhum produto correspondente. Em seguida, precisamos definir de forma programática sua propriedade `Visible` com base em se o DataList contém ou não os itens.

Para fazer isso, comece adicionando um rótulo abaixo do DataList. Defina sua propriedade `ID` como `NoProductsMessage` e sua propriedade `Text` como "não há produtos para a categoria selecionada..." Em seguida, precisamos definir programaticamente a propriedade `Visible` do rótulo com base em se os dados foram associados ao `ProductsInCategory` DataList. Essa atribuição deve ser feita depois que os dados tiverem sido associados ao DataList. Para GridView, DetailsView e FormView, poderíamos criar um manipulador de eventos para o evento de `DataBound` do controle, que é acionado após a conclusão da Associação de dados. No entanto, nem o DataList nem o Repeater têm um evento `DataBound` disponível.

Para esse exemplo específico, podemos atribuir a propriedade de `Visible` do rótulo no manipulador de eventos `Page_Load`, já que os dados terão sido atribuídos ao DataList antes do evento de `Load` da página. No entanto, essa abordagem não funcionaria no caso geral, pois os dados do ObjectDataSource podem estar associados ao DataList mais tarde no ciclo de vida da página. Por exemplo, se os dados exibidos forem baseados no valor de outro controle, como é ao exibir um relatório mestre/detalhado usando uma DropDownList para manter os registros "mestre", os dados poderão não ser reassociados ao controle da Web de dados até o estágio de `PreRender` no ciclo de vida da página.

Uma solução que funcionará para todos os casos é atribuir a propriedade `Visible` para `False` no manipulador de eventos `ItemDataBound` (ou `ItemCreated`) do DataList ao associar um tipo de item `Item` ou `AlternatingItem`. Nesse caso, sabemos que há pelo menos um item de dados na fonte de dados e, portanto, pode ocultar o rótulo de `NoProductsMessage`. Além desse manipulador de eventos, também precisamos de um manipulador de eventos para o evento `DataBinding` do DataList, no qual inicializamos a propriedade `Visible` do rótulo como `True`. Como o evento `DataBinding` é disparado antes dos eventos `ItemDataBound`, a propriedade `Visible` do rótulo será inicialmente definida como `True`; no entanto, se houver algum item de dados, ele será definido como `False`. O código a seguir implementa essa lógica:

[!code-csharp[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample6.cs)]

Todas as categorias no banco de dados Northwind estão associadas a um ou mais produtos. Para testar esse recurso, ajustei manualmente o banco de dados Northwind para este tutorial, reatribuindo todos os produtos associados à categoria de produção (`CategoryID` = 7) à categoria de frutos do mar (`CategoryID` = 8). Isso pode ser feito no Gerenciador de Servidores escolhendo nova consulta e usando a seguinte instrução `UPDATE`:

[!code-sql[Main](master-detail-filtering-acess-two-pages-datalist-cs/samples/sample7.sql)]

Depois de atualizar o banco de dados de acordo, volte para a página `CategoryListMaster.aspx` e clique no link produzir. Como não há mais nenhum produto que pertença à categoria de produção, você deverá ver a "não há produtos para a categoria selecionada..." , como mostra a Figura 9.

[![uma mensagem será exibida se não houver produtos pertencentes à categoria selecionada](master-detail-filtering-acess-two-pages-datalist-cs/_static/image26.png)](master-detail-filtering-acess-two-pages-datalist-cs/_static/image25.png)

**Figura 9**: uma mensagem será exibida se não houver produtos pertencentes à categoria selecionada ([clique para exibir a imagem em tamanho normal](master-detail-filtering-acess-two-pages-datalist-cs/_static/image27.png))

## <a name="summary"></a>Resumo

Embora os relatórios mestre/de detalhes possam exibir os registros mestre e de detalhes em uma única página, em muitos sites eles são separados em duas páginas da Web. Neste tutorial, examinamos como implementar esse relatório mestre/detalhado com as categorias listadas em uma lista com marcadores usando um repetidor na página da Web "mestre" e os produtos associados listados na página "detalhes". Cada item de lista na página da Web mestra continha um link para a página de detalhes que passou ao longo do valor de `CategoryID` da linha.

Na página de detalhes, a recuperação desses produtos para o fornecedor especificado foi realizada por meio do método `GetProductsByCategoryID(categoryID)` da classe `ProductsBLL`. O valor do parâmetro *`categoryID`* foi especificado declarativamente usando o valor de QueryString `CategoryID` como a origem do parâmetro. Também vimos como exibir detalhes da categoria na página de detalhes usando um FormView e como exibir uma mensagem se não havia produtos pertencentes à categoria selecionada.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Zack Jones e Liz Shulok. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-with-a-dropdownlist-datalist-cs.md)
> [Próximo](master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md)
