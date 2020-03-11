---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
title: Filtragem de mestre/detalhes em duas páginasC#() | Microsoft Docs
author: rick-anderson
description: Neste tutorial, implementaremos esse padrão usando um GridView para listar os fornecedores no banco de dados. Cada linha de fornecedor no GridView conterá um doformat...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 552d2d50-fe73-4153-9a7f-2b379bec4625
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-across-two-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: ccb3bfa5f215ba6e65b8a10b40041d5c2896c7e3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78529172"
---
# <a name="masterdetail-filtering-across-two-pages-c"></a>Filtragem mestre/detalhes em duas páginas (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_9_CS.exe) ou [baixar PDF](master-detail-filtering-across-two-pages-cs/_static/datatutorial09cs1.pdf)

> Neste tutorial, implementaremos esse padrão usando um GridView para listar os fornecedores no banco de dados. Cada linha de fornecedor no GridView conterá um link exibir produtos que, quando clicado, levará o usuário para uma página separada que lista esses produtos para o fornecedor selecionado.

## <a name="introduction"></a>Introdução

Nos dois tutoriais anteriores, vimos como [exibir relatórios mestre/detalhados em uma única página da Web usando DropDownLists](master-detail-filtering-with-a-dropdownlist-cs.md) para [exibir os registros "mestre" e um controle GridView ou DetailsView](master-detail-filtering-with-two-dropdownlists-cs.md) para exibir os "detalhes". Outro padrão comum usado para relatórios mestre/detalhes é ter os registros mestres em uma página da Web e os detalhes mostrados em outro. Um site de fórum, como [os fóruns do ASP.net](https://forums.asp.net/), é um ótimo exemplo desse padrão na prática. Os fóruns do ASP.NET são compostos por uma variedade de fóruns Introdução, Web Forms, controles de apresentação de dados e assim por diante. Cada fórum é composto por vários threads e cada thread é composto por várias postagens. Na home page dos fóruns do ASP.NET, os fóruns são listados. Clicar em um fórum na caixa estreita para uma página `ShowForum.aspx`, que lista os threads para esse fórum. Da mesma forma, clicar em um thread leva você até `ShowPost.aspx`, que exibe as postagens para o thread que foi clicado.

Neste tutorial, implementaremos esse padrão usando um GridView para listar os fornecedores no banco de dados. Cada linha de fornecedor no GridView conterá um link exibir produtos que, quando clicado, levará o usuário para uma página separada que lista esses produtos para o fornecedor selecionado.

## <a name="step-1-addingsupplierlistmasteraspxandproductsforsupplierdetailsaspxpages-to-thefilteringfolder"></a>Etapa 1: Adicionar`SupplierListMaster.aspx`e`ProductsForSupplierDetails.aspx`páginas à pasta`Filtering`

Ao definir o layout de página no terceiro tutorial, adicionamos várias páginas de "início" nas pastas `BasicReporting`, `Filtering`e `CustomFormatting`. No entanto, não adicionamos uma página inicial para este tutorial naquele momento, Reserve um tempo para adicionar duas novas páginas à pasta `Filtering`: `SupplierListMaster.aspx` e `ProductsForSupplierDetails.aspx`. `SupplierListMaster.aspx` listará os registros "mestre" (os fornecedores) enquanto `ProductsForSupplierDetails.aspx` exibirá os produtos para o fornecedor selecionado.

Ao criar essas duas novas páginas, certifique-se de associá-las à página mestra de `Site.master`.

![Adicione as páginas SupplierListMaster. aspx e ProductsForSupplierDetails. aspx à pasta de filtragem](master-detail-filtering-across-two-pages-cs/_static/image1.png)

**Figura 1**: adicionar as páginas `SupplierListMaster.aspx` e `ProductsForSupplierDetails.aspx` à pasta `Filtering`

Além disso, ao adicionar novas páginas ao projeto, certifique-se de atualizar o arquivo do mapa do site, `Web.sitemap`, de acordo. Para este tutorial, basta adicionar a página de `SupplierListMaster.aspx` ao mapa do site usando o seguinte conteúdo XML como um filho do elemento de filtragem de relatórios `<siteMapNode>`:

[!code-xml[Main](master-detail-filtering-across-two-pages-cs/samples/sample1.xml)]

> [!NOTE]
> Você pode ajudar a automatizar o processo de atualização do arquivo de mapa do site ao adicionar novas páginas do ASP.NET usando a [macro de mapa do site](http://odetocode.com/Blogs/scott/archive/2005/11/29/2537.aspx)gratuita da [K. Scott Allen](http://odetocode.com/Blogs/scott/).

## <a name="step-2-displaying-the-supplier-list-insupplierlistmasteraspx"></a>Etapa 2: exibindo a lista de fornecedores no`SupplierListMaster.aspx`

Com as páginas `SupplierListMaster.aspx` e `ProductsForSupplierDetails.aspx` criadas, nossa próxima etapa é criar o GridView dos fornecedores no `SupplierListMaster.aspx`. Adicione um GridView à página e associe-o a um novo ObjectDataSource. Esse ObjectDataSource deve usar o método `GetSuppliers()` da classe `SuppliersBLL` para retornar todos os fornecedores.

[![selecionar a classe SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image3.png)](master-detail-filtering-across-two-pages-cs/_static/image2.png)

**Figura 2**: selecione a classe `SuppliersBLL` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image4.png))

[![configurar o ObjectDataSource para usar o método getsuppliers ()](master-detail-filtering-across-two-pages-cs/_static/image6.png)](master-detail-filtering-across-two-pages-cs/_static/image5.png)

**Figura 3**: configurar o ObjectDataSource para usar o método `GetSuppliers()` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image7.png))

Precisamos incluir um link intitulado View Products em cada linha GridView que, quando clicada, leva o usuário para `ProductsForSupplierDetails.aspx` passando o valor de `SupplierID` da linha selecionada por meio de QueryString. Por exemplo, se o usuário clicar no link exibir produtos para o fornecedor de Tokyo Traders (que tem um valor de `SupplierID` de 4), ele deverá ser enviado para `ProductsForSupplierDetails.aspx?SupplierID=4`.

Para fazer isso, adicione um [HyperLinkField](https://msdn.microsoft.com/library/system.web.ui.webcontrols.hyperlinkfield.aspx) ao GridView, que adiciona um hiperlink a cada linha GridView. Comece clicando no link Editar colunas da marca inteligente do GridView. Em seguida, selecione o HyperLinkField na lista no canto superior esquerdo e clique em Adicionar para incluir o HyperLinkField na lista de campos do GridView.

[![adicionar um HyperLinkField ao GridView](master-detail-filtering-across-two-pages-cs/_static/image9.png)](master-detail-filtering-across-two-pages-cs/_static/image8.png)

**Figura 4**: adicionar um HyperLinkField ao GridView ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image10.png))

O HyperLinkField pode ser configurado para usar os mesmos valores de texto ou URL no link em cada linha GridView, ou pode basear esses valores nos valores de dados associados a cada linha específica. Para especificar um valor estático em todas as linhas, use as propriedades `Text` ou `NavigateUrl` do HyperLinkField. Como queremos que o texto do link seja o mesmo para todas as linhas, defina a propriedade `Text` do HyperLinkField para exibir produtos.

[![definir a propriedade Text do HyperLinkField para exibir produtos](master-detail-filtering-across-two-pages-cs/_static/image12.png)](master-detail-filtering-across-two-pages-cs/_static/image11.png)

**Figura 5**: definir a propriedade `Text` do HyperLinkField para exibir produtos ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image13.png))

Para definir os valores de texto ou de URL com base nos dados subjacentes associados à linha GridView, especifique os campos de dados dos quais os valores de texto ou de URL devem ser extraídos nas propriedades `DataTextField` ou `DataNavigateUrlFields`. `DataTextField` só pode ser definida para um único campo de dados; o `DataNavigateUrlFields`, no entanto, pode ser definido como uma lista delimitada por vírgulas de campos de dados. Frequentemente, precisamos basear o texto ou a URL em uma combinação do valor do campo de dados da linha atual e de alguma marcação estática. Neste tutorial, por exemplo, queremos que a URL dos links do HyperLinkField seja `ProductsForSupplierDetails.aspx?SupplierID=supplierID`, em que *`supplierID`* é o valor de `SupplierID` de cada Row's de GridView. Observe que precisamos de valores estáticos e controlados por dados aqui: a parte `ProductsForSupplierDetails.aspx?SupplierID=` da URL do link é estática, enquanto a parte *`supplierID`* é controlada por dados, pois seu valor é o próprio valor de `SupplierID` da linha.

Para indicar uma combinação de valores estáticos e controlados por dados, use as propriedades `DataTextFormatString` e `DataNavigateUrlFormatString`. Nessas propriedades, insira a marcação estática conforme necessário e, em seguida, use o marcador `{0}` em que você deseja que o valor do campo especificado nas propriedades `DataTextField` ou `DataNavigateUrlFields` seja exibido. Se a propriedade `DataNavigateUrlFields` tiver vários campos especificados, use `{0}` em que você deseja inserir o primeiro valor de campo, `{1}` para o segundo valor de campo e assim por diante.

Aplicando isso ao nosso tutorial, precisamos definir a propriedade `DataNavigateUrlFields` como `SupplierID`, já que esse é o campo de dados cujo valor precisamos personalizar por linha e a propriedade `DataNavigateUrlFormatString` como `ProductsForSupplierDetails.aspx?SupplierID={0}`.

[![configurar o HyperLinkField para incluir a URL de link apropriada com base na CódigoDoFornecedor](master-detail-filtering-across-two-pages-cs/_static/image15.png)](master-detail-filtering-across-two-pages-cs/_static/image14.png)

**Figura 6**: configurar o HyperLinkField para incluir a URL de link apropriada com base na `SupplierID` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image16.png))

Depois de adicionar o HyperLinkField, sinta-se à vontade para personalizar e reordenar os campos do GridView. A marcação a seguir mostra o GridView depois de ter feito algumas personalizações de nível de campo secundárias.

[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample2.aspx)]

Reserve um tempo para exibir a página de `SupplierListMaster.aspx` por meio de um navegador. Como mostra a Figura 7, a página atualmente lista todos os fornecedores, incluindo um link exibir produtos. Clicar no link exibir produtos levará você para `ProductsForSupplierDetails.aspx`, passando ao longo do `SupplierID` do fornecedor na QueryString.

[![cada linha de fornecedor contém um link exibir produtos](master-detail-filtering-across-two-pages-cs/_static/image18.png)](master-detail-filtering-across-two-pages-cs/_static/image17.png)

**Figura 7**: cada linha de fornecedor contém um link exibir produtos ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image19.png))

## <a name="step-3-listing-the-suppliers-products-inproductsforsupplierdetailsaspx"></a>Etapa 3: listando os produtos do fornecedor em`ProductsForSupplierDetails.aspx`

Neste ponto, a página de `SupplierListMaster.aspx` está enviando usuários para `ProductsForSupplierDetails.aspx`, passando a `SupplierID` do fornecedor selecionado na QueryString. A etapa final do tutorial é exibir os produtos em um GridView em `ProductsForSupplierDetails.aspx` cuja `SupplierID` é igual à `SupplierID` passada por meio da QueryString. Para realizar esse início, adicionando um GridView à página `ProductsForSupplierDetails.aspx`, usando um novo controle ObjectDataSource chamado `ProductsBySupplierDataSource` que invoca o método `GetProductsBySupplierID(supplierID)` da classe `ProductsBLL`.

[![adicionar um novo ObjectDataSource chamado ProductsBySupplierDataSource](master-detail-filtering-across-two-pages-cs/_static/image21.png)](master-detail-filtering-across-two-pages-cs/_static/image20.png)

**Figura 8**: adicionar um novo ObjectDataSource chamado `ProductsBySupplierDataSource` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image22.png))

[![selecionar a classe ProductsBLL](master-detail-filtering-across-two-pages-cs/_static/image24.png)](master-detail-filtering-across-two-pages-cs/_static/image23.png)

**Figura 9**: selecionar a classe de `ProductsBLL` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image25.png))

[![fazer com que ObjectDataSource invoque o método GetProductsBySupplierID (CódigoDoFornecedor)](master-detail-filtering-across-two-pages-cs/_static/image27.png)](master-detail-filtering-across-two-pages-cs/_static/image26.png)

**Figura 10**: fazer com que o ObjectDataSource invoque o método `GetProductsBySupplierID(supplierID)` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image28.png))

A etapa final do assistente para configurar fonte de dados nos pede para fornecer a fonte do parâmetro *`supplierID`* do método de `GetProductsBySupplierID(supplierID)`. Para usar o valor de QueryString, defina a origem do parâmetro como QueryString e insira o nome do valor de QueryString a ser usado na caixa de texto QueryStringfield (`SupplierID`).

[![popular o valor do parâmetro CódigoDoFornecedor do valor de QueryString do CódigoDoFornecedor](master-detail-filtering-across-two-pages-cs/_static/image30.png)](master-detail-filtering-across-two-pages-cs/_static/image29.png)

**Figura 11**: popular o valor do parâmetro *`supplierID`* do valor de QueryString `SupplierID` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image31.png))

Isso é tudo! A Figura 12 mostra a página `ProductsForSupplierDetails.aspx` quando visitada clicando no link Tokyo Traders da `SupplierListMaster.aspx`.

[![os produtos fornecidos por Tokyo Traders são mostrados](master-detail-filtering-across-two-pages-cs/_static/image33.png)](master-detail-filtering-across-two-pages-cs/_static/image32.png)

**Figura 12**: os produtos fornecidos por Tokyo Traders são mostrados ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image34.png))

## <a name="displaying-supplier-information-inproductsforsupplierdetailsaspx"></a>Exibindo informações do fornecedor no`ProductsForSupplierDetails.aspx`

Como mostra a Figura 12, a página `ProductsForSupplierDetails.aspx` apenas lista os produtos fornecidos pelo `SupplierID` especificado na QueryString. Alguém enviado diretamente para essa página, no entanto, não saberá que a Figura 12 está mostrando os produtos de Tokyo Traders. Para corrigir isso, também podemos exibir informações de fornecedores nesta página.

Comece adicionando um FormView acima do GridView dos produtos. Crie um novo controle ObjectDataSource chamado `SuppliersDataSource` que invoca o método de `GetSupplierBySupplierID(supplierID)` da classe `SuppliersBLL`.

[![selecionar a classe SuppliersBLL](master-detail-filtering-across-two-pages-cs/_static/image36.png)](master-detail-filtering-across-two-pages-cs/_static/image35.png)

**Figura 13**: selecione a classe `SuppliersBLL` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image37.png))

[![fazer com que ObjectDataSource invoque o método GetSupplierBySupplierID (CódigoDoFornecedor)](master-detail-filtering-across-two-pages-cs/_static/image39.png)](master-detail-filtering-across-two-pages-cs/_static/image38.png)

**Figura 14**: fazer com que o ObjectDataSource invoque o método `GetSupplierBySupplierID(supplierID)` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image40.png))

Assim como com o `ProductsBySupplierDataSource`, tenha o parâmetro *`supplierID`* atribuído o valor do valor de `SupplierID` QueryString.

[![popular o valor do parâmetro CódigoDoFornecedor do valor de QueryString do CódigoDoFornecedor](master-detail-filtering-across-two-pages-cs/_static/image42.png)](master-detail-filtering-across-two-pages-cs/_static/image41.png)

**Figura 15**: popular o valor do parâmetro *`supplierID`* do valor de QueryString `SupplierID` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image43.png))

Ao ligar o FormView ao ObjectDataSource no modo de exibição de Design, o Visual Studio criará automaticamente o `ItemTemplate`, `InsertItemTemplate`e `EditItemTemplate` do FormView com controles da Web Label e TextBox para cada um dos campos de dados retornados pelo ObjectDataSource. Como queremos apenas exibir informações do fornecedor, sinta-se à vontade para remover o `InsertItemTemplate` e `EditItemTemplate`. Em seguida, edite o ItemTemplate para que ele exiba o nome da empresa do fornecedor em um elemento `<h3>` e o endereço, a cidade, o país e o número de telefone abaixo do nome da empresa. Como alternativa, você pode definir manualmente o `DataSourceID` do FormView e criar a marcação de `ItemTemplate`, como fizemos de volta no tutorial "[exibindo dados com o ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md)".

Depois que elas são editadas, a marcação declarativa do FormView deve ser semelhante ao seguinte:

[!code-aspx[Main](master-detail-filtering-across-two-pages-cs/samples/sample3.aspx)]

A Figura 16 mostra uma captura de tela da página `ProductsForSupplierDetails.aspx` depois que as informações do fornecedor detalhadas acima tiverem sido incluídas.

[![a lista de produtos inclui um resumo sobre o fornecedor](master-detail-filtering-across-two-pages-cs/_static/image45.png)](master-detail-filtering-across-two-pages-cs/_static/image44.png)

**Figura 16**: a lista de produtos inclui um resumo sobre o fornecedor ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image46.png))

## <a name="applying-the-final-touches-for-theproductsforsupplierdetailsaspxui"></a>Aplicando os toques finais para a interface do usuário do`ProductsForSupplierDetails.aspx`

Para melhorar a experiência do usuário para esse relatório, há algumas adições que devemos fazer na página de `ProductsForSupplierDetails.aspx`. Atualmente, a única maneira que um usuário pode passar da página `ProductsForSupplierDetails.aspx` de volta para a lista de fornecedores é clicar no botão voltar do navegador. Vamos adicionar um controle HyperLink à página `ProductsForSupplierDetails.aspx` que se vincula de volta a `SupplierListMaster.aspx`, fornecendo outra maneira para o usuário retornar à lista mestra.

[![adicionar um controle de hiperlink para levar o usuário de volta ao SupplierListMaster. aspx](master-detail-filtering-across-two-pages-cs/_static/image48.png)](master-detail-filtering-across-two-pages-cs/_static/image47.png)

**Figura 17**: adicionar um controle de hiperlink para levar o usuário de volta ao `SupplierListMaster.aspx` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image49.png))

Se o usuário clicar no link exibir produtos para um fornecedor que não tem nenhum produto, o `ProductsBySupplierDataSource` ObjectDataSource em `ProductsForSupplierDetails.aspx` não retornará nenhum resultado. O GridView vinculado ao ObjectDataSource não renderizará nenhuma marcação que resulte em uma região em branco na página no navegador do usuário. Para se comunicar com mais clareza ao usuário de que não há produtos associados ao fornecedor selecionado, podemos definir a propriedade `EmptyDataText` do GridView para a mensagem que queremos exibir quando tal situação surgir. Defini essa propriedade como "não há produtos fornecidos por este fornecedor"

Por padrão, todos os fornecedores no banco de dados Northwind fornecem pelo menos um produto. No entanto, para este tutorial, modifiquei manualmente a tabela `Products` para que o fornecedor escargots Nouveaux não esteja mais associado a nenhum produto. A Figura 18 mostra a página de detalhes para escargots Nouveaux depois que essa alteração tiver sido feita.

[![usuários são informados de que o fornecedor não fornece nenhum produto](master-detail-filtering-across-two-pages-cs/_static/image51.png)](master-detail-filtering-across-two-pages-cs/_static/image50.png)

**Figura 18**: os usuários são informados de que o fornecedor não fornece nenhum produto ([clique para exibir a imagem em tamanho normal](master-detail-filtering-across-two-pages-cs/_static/image52.png))

## <a name="summary"></a>Resumo

Embora os relatórios mestre/de detalhes possam exibir os registros mestre e de detalhes em uma única página, em muitos sites eles são separados em duas páginas da Web. Neste tutorial, vimos como implementar esse relatório mestre/detalhado com os fornecedores listados em um GridView na página da Web "mestre" e os produtos associados listados na página "detalhes". Cada linha de fornecedor na página da Web mestra continha um link para a página de detalhes que passou ao longo do valor de `SupplierID` da linha. Esses links específicos de linha podem ser facilmente adicionados usando o HyperLinkField do GridView.

Na página de detalhes, a recuperação desses produtos para o fornecedor especificado foi realizada invocando o método `GetProductsBySupplierID(supplierID)` da classe de `ProductsBLL`. O valor do parâmetro *`supplierID`* foi especificado declarativamente usando o QueryString como a origem do parâmetro. Também vimos como exibir os detalhes do fornecedor na página de detalhes usando um FormView.

Nosso [próximo tutorial](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) é o final nos relatórios mestre/de detalhes. Veremos como exibir uma lista de produtos em um GridView em que cada linha tem um botão de seleção. Clicar no botão Selecionar exibirá os detalhes do produto em um controle DetailsView na mesma página.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Hilton Giesenow. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-with-two-dropdownlists-cs.md)
> [Próximo](master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md)
