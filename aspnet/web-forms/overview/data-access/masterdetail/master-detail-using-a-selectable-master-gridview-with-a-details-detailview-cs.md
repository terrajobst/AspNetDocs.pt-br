---
uid: web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
title: Mestre/detalhes usando um GridView mestre selecionável com um Details DetailViewC#() | Microsoft Docs
author: rick-anderson
description: Este tutorial terá um GridView cujas linhas incluem o nome e o preço de cada produto junto com um botão de seleção. Clicando no botão Selecionar para um particu...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0f982827-f8f9-420d-b36b-57b23f5aa519
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs
msc.type: authoredcontent
ms.openlocfilehash: 04c427341f063729bd23b416a96f5acb20702792
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74632178"
---
# <a name="masterdetail-using-a-selectable-master-gridview-with-a-details-detailview-c"></a>Mestre/detalhes usando um GridView mestre selecionável com um DetailView de detalhes (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_10_CS.exe) ou [baixar PDF](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/datatutorial10cs1.pdf)

> Este tutorial terá um GridView cujas linhas incluem o nome e o preço de cada produto junto com um botão de seleção. Clicar no botão Selecionar para um produto específico fará com que seus detalhes completos sejam exibidos em um controle DetailsView na mesma página.

## <a name="introduction"></a>Introdução

No [tutorial anterior](master-detail-filtering-across-two-pages-cs.md) , vimos como criar um relatório mestre/detalhado usando duas páginas da Web: uma página da Web "mestre", da qual exibimos a lista de fornecedores; e uma página da Web "detalhes" que listou os produtos fornecidos pelo fornecedor selecionado. Esse formato de relatório de duas páginas pode ser condensado em uma página. Este tutorial terá um GridView cujas linhas incluem o nome e o preço de cada produto junto com um botão de seleção. Clicar no botão Selecionar para um produto específico fará com que seus detalhes completos sejam exibidos em um controle DetailsView na mesma página.

[![clicar no botão Selecionar exibe os detalhes do produto](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image2.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image1.png)

**Figura 1**: clicar no botão Selecionar exibe os detalhes do produto ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image3.png))

## <a name="step-1-creating-a-selectable-gridview"></a>Etapa 1: Criando um GridView selecionável

Lembre-se de que, no relatório mestre/detalhes de duas páginas, cada registro mestre incluía um hiperlink que, quando clicado, enviou o usuário para a página de detalhes passando o valor de `SupplierID` da linha clicado na QueryString. Esse hiperlink foi adicionado a cada linha GridView usando um HyperLinkField. Para o relatório mestre/detalhes de página única, precisamos de um botão para cada linha GridView que, quando clicada, mostre os detalhes. O controle GridView pode ser configurado para incluir um botão de seleção para cada linha que causa um postback e marca essa linha como o [SelectedRow](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedrow.aspx)do GridView.

Comece adicionando um controle GridView à página `DetailsBySelecting.aspx` na pasta `Filtering`, definindo sua propriedade `ID` como `ProductsGrid`. Em seguida, adicione um novo ObjectDataSource chamado `AllProductsDataSource` que invoca o método de `GetProducts()` da classe `ProductsBLL`.

[![criar um ObjectDataSource chamado AllProductsDataSource](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image5.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image4.png)

**Figura 2**: criar um ObjectDataSource chamado `AllProductsDataSource` ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image6.png))

[![usar a classe ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image8.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image7.png)

**Figura 3**: usar a classe `ProductsBLL` ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image9.png))

[![configurar o ObjectDataSource para invocar o método GetProducts ()](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image11.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image10.png)

**Figura 4**: configurar o ObjectDataSource para invocar o método de `GetProducts()` ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image12.png))

Edite os campos do GridView removendo todos os `ProductName` e `UnitPrice` BoundFields. Além disso, sinta-se à vontade para personalizar esses BoundFields conforme necessário, como formatar o `UnitPrice` BoundField como uma moeda e alterar as propriedades `HeaderText` de BoundFields. Essas etapas podem ser realizadas graficamente, clicando no link Editar colunas da marca inteligente do GridView ou configurando manualmente a sintaxe declarativa.

[![remover tudo, exceto o NomeDoProduto e o UnitPrice BoundFields](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image14.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image13.png)

**Figura 5**: remover todas as `ProductName` e `UnitPrice` boundfields ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image15.png))

A marcação final para GridView é:

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample1.aspx)]

Em seguida, precisamos marcar o GridView como selecionável, que adicionará um botão Select a cada linha. Para fazer isso, basta marcar a caixa de seleção Habilitar seleção na marca inteligente do GridView.

[![tornar as linhas do GridView selecionáveis](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image17.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image16.png)

**Figura 6**: tornar as linhas do GridView selecionáveis ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image18.png))

Marcar a opção habilitar seleção adiciona um CommandField ao `ProductsGrid` GridView com sua propriedade `ShowSelectButton` definida como true. Isso resulta em um botão de seleção para cada linha do GridView, como ilustra a Figura 6. Por padrão, os botões de seleção são renderizados como LinkButtons, mas você pode usar botões ou ImageButtons, em vez da propriedade de `ButtonType` de comando.

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample2.aspx)]

Quando o botão Selecionar de uma linha GridView é clicado em um postback massacre e a propriedade `SelectedRow` do GridView é atualizada. Além da propriedade `SelectedRow`, o GridView fornece as propriedades [SelectedIndex](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex%28VS.80%29.aspx), [SelectedValue](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue%28VS.80%29.aspx)e [SelectedDataKey](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selecteddatakey%28VS.80%29.aspx) . A propriedade `SelectedIndex` retorna o índice da linha selecionada, enquanto as propriedades `SelectedValue` e `SelectedDataKey` retornam valores com base na [propriedade DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames%28VS.80%29.aspx)do GridView.

A propriedade `DataKeyNames` é usada para associar um ou mais valores de campo de dados a cada linha e é comumente usada para atribuir informações de identificação exclusiva dos dados subjacentes a cada linha GridView. A propriedade `SelectedValue` retorna o valor do primeiro campo de dados de `DataKeyNames` para a linha selecionada, em que a propriedade `SelectedDataKey` retorna o objeto `DataKey` da linha selecionada, que contém todos os valores dos campos de chave de dados especificados para essa linha.

A propriedade `DataKeyNames` é definida automaticamente para a identificação exclusiva de campos de dados quando você associa uma fonte de dados a um GridView, DetailsView ou FormView por meio do designer. Embora essa propriedade tenha sido definida para nós automaticamente nos tutoriais anteriores, os exemplos funcionaram sem a propriedade de `DataKeyNames` especificada. No entanto, para o GridView selecionável neste tutorial, bem como para tutoriais futuros nos quais iremos examinar inserção, atualização e exclusão, a propriedade `DataKeyNames` deve ser definida corretamente. Reserve um tempo para garantir que a propriedade `DataKeyNames` do seu GridView esteja definida como `ProductID`.

Vamos exibir nosso progresso até o momento, por meio de um navegador. Observe que o GridView lista o nome e o preço de todos os produtos junto com um SELECT LinkButton. Clicar no botão Selecionar causa um postback. Na etapa 2, veremos como fazer com que um DetailsView responda a esse postback exibindo os detalhes do produto selecionado.

[![cada linha de produto contém uma seleção de LinkButton](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image20.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image19.png)

**Figura 7**: cada linha de produto contém um SELECT LinkButton ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image21.png))

### <a name="highlighting-the-selected-row"></a>Realçando a linha selecionada

O `ProductsGrid` GridView tem uma propriedade `SelectedRowStyle` que pode ser usada para ditar o estilo visual da linha selecionada. Usado corretamente, isso pode melhorar a experiência do usuário mostrando mais claramente qual linha do GridView está selecionada no momento. Para este tutorial, vamos fazer com que a linha selecionada seja realçada com um plano de fundo amarelo.

Assim como nos tutoriais anteriores, vamos nos esforçar para manter as configurações relacionadas à estética definidas como classes CSS. Portanto, crie uma nova classe CSS em `Styles.css` chamada `SelectedRowStyle`.

[!code-css[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample3.css)]

Para aplicar essa classe CSS à propriedade `SelectedRowStyle` de *todos os* GridViews em nossa série de tutoriais, edite a capa `GridView.skin` no tema `DataWebControls` para incluir as configurações de `SelectedRowStyle`, conforme mostrado abaixo:

[!code-aspx[Main](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/samples/sample4.aspx)]

Com essa adição, a linha GridView selecionada agora é realçada com uma cor de fundo amarela.

[![personalizar a aparência da linha selecionada usando a Propriedade SelectedRowStyle de GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image23.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image22.png)

**Figura 8**: personalizar a aparência da linha selecionada usando a propriedade `SelectedRowStyle` do GridView ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image24.png))

## <a name="step-2-displaying-the-selected-products-details-in-a-detailsview"></a>Etapa 2: exibindo os detalhes do produto selecionado em um DetailsView

Com o `ProductsGrid` GridView concluído, tudo o que resta é adicionar um DetailsView que exibe informações sobre o produto específico selecionado. Adicione um controle DetailsView acima do GridView e crie um novo ObjectDataSource chamado `ProductDetailsDataSource`. Como queremos que este DetailsView exiba informações específicas sobre o produto selecionado, configure o `ProductDetailsDataSource` para usar o método de `GetProductByProductID(productID)` da classe `ProductsBLL`.

[![invocar o método GetProductByProductID (productID) da classe ProductsBLL](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image26.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image25.png)

**Figura 9**: invocar o método `GetProductByProductID(productID)` da classe `ProductsBLL` ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image27.png))

Ter o valor do parâmetro *`productID`* obtido da propriedade `SelectedValue` do controle GridView. Como discutimos anteriormente, a propriedade `SelectedValue` do GridView retorna o primeiro valor de chave de dados para a linha selecionada. Portanto, é imperativo que a propriedade `DataKeyNames` do GridView seja definida como `ProductID`, de modo que o valor de `ProductID` da linha selecionada seja retornado pelo `SelectedValue`.

[![definir o parâmetro productID como a propriedade SelectedValue do GridView](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image29.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image28.png)

**Figura 10**: definir o parâmetro *`productID`* como a propriedade `SelectedValue` do GridView ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image30.png))

Depois que o `productDetailsDataSource` ObjectDataSource tiver sido configurado corretamente e associado a DetailsView, este tutorial será concluído! Quando a página é visitada pela primeira vez, nenhuma linha é selecionada, portanto, a propriedade `SelectedValue` do GridView retorna `null`. Como não há produtos com um valor de `NULL` `ProductID`, nenhum registro é retornado pelo método `GetProductByProductID(productID)`, o que significa que o DetailsView não é exibido (consulte a Figura 11). Ao clicar no botão de seleção de uma linha GridView, um postback massacre e o DetailsView são atualizados. Desta vez, a propriedade `SelectedValue` do GridView retorna o `ProductID` da linha selecionada, o método `GetProductByProductID(productID)` retorna um `ProductsDataTable` com informações sobre esse produto específico e o DetailsView mostra esses detalhes (veja a Figura 12).

[![quando visitado pela primeira vez, somente o GridView é exibido](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image32.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image31.png)

**Figura 11**: quando visitado pela primeira vez, somente o GridView é exibido ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image33.png))

[![na seleção de uma linha, os detalhes do produto são exibidos](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image35.png)](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image34.png)

**Figura 12**: ao selecionar uma linha, os detalhes do produto são exibidos ([clique para exibir a imagem em tamanho normal](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs/_static/image36.png))

## <a name="summary"></a>Resumo

Neste e nos três tutoriais anteriores, vimos várias técnicas para exibir relatórios mestre/de detalhes. Neste tutorial, examinamos o uso de um GridView selecionável para alojar os registros mestres e um DetailsView para exibir detalhes sobre o registro mestre selecionado na mesma página. Nos tutoriais anteriores, vimos como exibir relatórios mestre/detalhes usando DropDownLists e exibindo registros mestre em uma página da Web e registros de detalhes em outra.

Este tutorial conclui nosso exame de relatórios mestre/detalhados. A partir do próximo tutorial, começaremos nossa exploração de formatação personalizada com GridView, DetailsView e FormView. Veremos como personalizar a aparência desses controles com base nos dados associados a eles, como resumir dados no rodapé do GridView e como usar modelos para obter um grau maior de controle sobre o layout.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Hilton Giesenow. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-across-two-pages-cs.md)
> [Próximo](master-detail-filtering-with-a-dropdownlist-vb.md)
