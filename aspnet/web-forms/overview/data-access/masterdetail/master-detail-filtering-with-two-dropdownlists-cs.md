---
uid: web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
title: Filtragem de mestre/detalhes com dois DropDownListsC#() | Microsoft Docs
author: rick-anderson
description: Este tutorial expande a relação mestre/detalhes para adicionar uma terceira camada, usando dois controles DropDownList para selecionar o pai desejado e o avô superiores...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: ac4b0d77-4816-4ded-afd0-88dab667aedd
msc.legacyurl: /web-forms/overview/data-access/masterdetail/master-detail-filtering-with-two-dropdownlists-cs
msc.type: authoredcontent
ms.openlocfilehash: e24b7f91d34fbce1676f7f28ebb7d23903157f7f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78528486"
---
# <a name="masterdetail-filtering-with-two-dropdownlists-c"></a>Filtragem mestre/detalhes com duas DropDownLists (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_8_CS.exe) ou [baixar PDF](master-detail-filtering-with-two-dropdownlists-cs/_static/datatutorial08cs1.pdf)

> Este tutorial expande a relação mestre/detalhes para adicionar uma terceira camada, usando dois controles DropDownList para selecionar os registros pai e avô desejados.

## <a name="introduction"></a>Introdução

No [tutorial anterior](master-detail-filtering-with-a-dropdownlist-cs.md) , examinamos como exibir um relatório mestre/detalhes simples usando uma única DropDownList populada com as categorias e um GridView mostrando os produtos que pertencem à categoria selecionada. Esse padrão de relatório funciona bem ao exibir registros que têm uma relação um-para-muitos e pode ser facilmente estendido para funcionar em cenários que incluem várias relações um-para-muitos. Por exemplo, um sistema de entrada de pedidos teria tabelas que correspondam a clientes, pedidos e itens de linha de pedido. Um determinado cliente pode ter vários pedidos com cada pedido que consiste em vários itens. Esses dados podem ser apresentados ao usuário com dois DropDownLists e um GridView. A primeira DropDownList teria um item de lista para cada cliente no banco de dados com o conteúdo do segundo, sendo os pedidos feitos pelo cliente selecionado. Um GridView listaria os itens de linha da ordem selecionada.

Embora o banco de dados Northwind inclua as informações de detalhes canônicos de cliente/pedido/pedido em suas tabelas `Customers`, `Orders`e `Order Details`, essas tabelas não são capturadas em nossa arquitetura. No entanto, ainda podemos ilustrar o uso de dois DropDownLists dependentes. A primeira DropDownList listará as categorias e o segundo os produtos que pertencem à categoria selecionada. Um DetailsView listará os detalhes do produto selecionado.

## <a name="step-1-creating-and-populating-the-categories-dropdownlist"></a>Etapa 1: Criando e preenchendo as categorias DropDownList

Nosso primeiro objetivo é adicionar o DropDownList que lista as categorias. Essas etapas foram examinadas em detalhes no tutorial anterior, mas são resumidas aqui para fins de integridade.

Abra a página `MasterDetailsDetails.aspx` na pasta `Filtering`, adicione uma DropDownList à página, defina sua propriedade `ID` como `Categories`e, em seguida, clique no link configurar fonte de dados em sua marca inteligente. No assistente de configuração da fonte de dados, escolha Adicionar uma nova fonte de dados.

[![adicionar uma nova fonte de dados para a DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image2.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image1.png)

**Figura 1**: adicionar uma nova fonte de dados para a DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image3.png))

A nova fonte de dados deve, naturalmente, ser um ObjectDataSource. Nomeie esse novo ObjectDataSource `CategoriesDataSource` e faça com que ele invoque o método de `GetCategories()` do objeto `CategoriesBLL`.

[![optar por usar a classe CategoriesBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image5.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image4.png)

**Figura 2**: escolha usar a classe `CategoriesBLL` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image6.png))

[![configurar o ObjectDataSource para usar o método GetCategories ()](master-detail-filtering-with-two-dropdownlists-cs/_static/image8.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image7.png)

**Figura 3**: configurar o ObjectDataSource para usar o método `GetCategories()` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image9.png))

Depois de configurar o ObjectDataSource, ainda precisamos especificar qual campo de fonte de dados deve ser exibido no `Categories` DropDownList e qual deles deve ser configurado como o valor para o item de lista. Defina o campo `CategoryName` como a exibição e `CategoryID` como o valor de cada item de lista.

[![fazer com que a DropDownList exiba o campo CategoryName e use CategoryID como o valor](master-detail-filtering-with-two-dropdownlists-cs/_static/image11.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image10.png)

**Figura 4**: fazer com que a DropDownList exiba o campo `CategoryName` e use `CategoryID` como o valor ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image12.png))

Neste ponto, temos um controle DropDownList (`Categories`) que é populado com os registros da tabela `Categories`. Quando o usuário escolhe uma nova categoria da DropDownList, queremos que um postback ocorra para atualizar o produto DropDownList que vamos criar na etapa 2. Portanto, marque a opção Habilitar AutoPostBack na marca inteligente da `categories` DropDownList.

[![habilitar AutoPostBack para as categorias DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image14.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image13.png)

**Figura 5**: habilitar AutoPostBack para o `Categories` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image15.png))

## <a name="step-2-displaying-the-selected-categorys-products-in-a-second-dropdownlist"></a>Etapa 2: exibindo os produtos da categoria selecionada em uma segunda DropDownList

Com a `Categories` DropDownList concluída, nossa próxima etapa é exibir uma DropDownList dos produtos que pertencem à categoria selecionada. Para fazer isso, adicione outra DropDownList à página chamada `ProductsByCategory`. Assim como ocorre com o `Categories` DropDownList, crie um novo ObjectDataSource para o `ProductsByCategory` DropDownList chamado `ProductsByCategoryDataSource`.

[![adicionar uma nova fonte de dados para o ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image17.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image16.png)

**Figura 6**: adicionar uma nova fonte de dados para o `ProductsByCategory` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image18.png))

[![criar um novo ObjectDataSource chamado ProductsByCategoryDataSource](master-detail-filtering-with-two-dropdownlists-cs/_static/image20.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image19.png)

**Figura 7**: criar um novo ObjectDataSource chamado `ProductsByCategoryDataSource` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image21.png))

Como o `ProductsByCategory` DropDownList precisa exibir apenas os produtos que pertencem à categoria selecionada, faça com que o ObjectDataSource invoque o método `GetProductsByCategoryID(categoryID)` do objeto `ProductsBLL`.

[![optar por usar a classe ProductsBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image23.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image22.png)

**Figura 8**: escolha usar a classe `ProductsBLL` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image24.png))

[![configurar o ObjectDataSource para usar o método GetProductsByCategoryID (categoryID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image26.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image25.png)

**Figura 9**: configurar o ObjectDataSource para usar o método `GetProductsByCategoryID(categoryID)` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image27.png))

Na etapa final do assistente, precisamos especificar o valor do parâmetro *`categoryID`* . Atribua esse parâmetro ao item selecionado da `Categories` DropDownList.

[![extrair o valor do parâmetro categoryID das categorias DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image29.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image28.png)

**Figura 10**: efetuar pull do valor do parâmetro *`categoryID`* da `Categories` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image30.png))

Com o ObjectDataSource configurado, tudo o que resta é especificar quais campos de fonte de dados são usados para a exibição e o valor dos itens da DropDownList. Exiba o campo `ProductName` e use o campo `ProductID` como o valor.

[![especificar os campos de fonte de dados usados para as propriedades Text e Value da DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image32.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image31.png)

**Figura 11**: especificar os campos de fonte de dados usados para as propriedades `ListItem` s `Text` e `Value` do DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image33.png))

Com o ObjectDataSource e `ProductsByCategory` DropDownList configurados, nossa página exibirá dois DropDownLists: o primeiro listará todas as categorias, enquanto a segunda listará os produtos que pertencem à categoria selecionada. Quando o usuário selecionar uma nova categoria da primeira DropDownList, um postback será acontecerdo e a segunda DropDownList será reassociada, mostrando os produtos que pertencem à categoria selecionada recentemente. As figuras 12 e 13 mostram `MasterDetailsDetails.aspx` em ação quando exibidas por meio de um navegador.

[![ao visitar a página pela primeira vez, a categoria bebidas está selecionada](master-detail-filtering-with-two-dropdownlists-cs/_static/image35.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image34.png)

**Figura 12**: ao visitar a página pela primeira vez, a categoria bebidas é selecionada ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image36.png))

[![escolher uma categoria diferente exibe os produtos da nova categoria](master-detail-filtering-with-two-dropdownlists-cs/_static/image38.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image37.png)

**Figura 13**: escolher uma categoria diferente exibe os produtos da nova categoria ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image39.png))

Atualmente, a `productsByCategory` DropDownList, quando alterada, *não* causa um postback. No entanto, queremos que um postback ocorra depois de adicionar um DetailsView para exibir os detalhes do produto selecionado (etapa 3). Portanto, marque a caixa de seleção Habilitar AutoPostBack na marca inteligente da `productsByCategory` DropDownList.

[![habilitar o recurso AutoPostBack para o productsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image41.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image40.png)

**Figura 14**: habilitar o recurso AutoPostBack para o `productsByCategory` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image42.png))

## <a name="step-3-using-a-detailsview-to-display-details-for-the-selected-product"></a>Etapa 3: usando um DetailsView para exibir detalhes do produto selecionado

A etapa final é exibir os detalhes do produto selecionado em um DetailsView. Para fazer isso, adicione um DetailsView à página, defina sua propriedade `ID` como `ProductDetails`e crie um novo ObjectDataSource para ele. Configure esse ObjectDataSource para efetuar pull de seus dados do método `GetProductByProductID(productID)` da classe `ProductsBLL` usando o valor selecionado do `ProductsByCategory` DropDownList para o valor do parâmetro *`productID`* .

[![optar por usar a classe ProductsBLL](master-detail-filtering-with-two-dropdownlists-cs/_static/image44.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image43.png)

**Figura 15**: escolha usar a classe `ProductsBLL` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image45.png))

[![configurar o ObjectDataSource para usar o método GetProductByProductID (productID)](master-detail-filtering-with-two-dropdownlists-cs/_static/image47.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image46.png)

**Figura 16**: configurar o ObjectDataSource para usar o método `GetProductByProductID(productID)` ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image48.png))

[![extrair o valor do parâmetro productID do ProductsByCategory DropDownList](master-detail-filtering-with-two-dropdownlists-cs/_static/image50.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image49.png)

**Figura 17**: efetuar pull do valor do parâmetro *`productID`* da `ProductsByCategory` DropDownList ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image51.png))

Você pode optar por exibir qualquer um dos campos disponíveis no DetailsView. Eu optei por remover os campos `ProductID`, `SupplierID`e `CategoryID` e reordenados e formatados os campos restantes. Além disso, Eu desmarquei as propriedades `Height` e `Width` de DetailsView, permitindo que o DetailsView expanda a largura necessária para exibir melhor seus dados em vez de tê-los restritos a um tamanho especificado. A marcação completa aparece abaixo:

[!code-aspx[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample1.aspx)]

Reserve um tempo para experimentar a página `MasterDetailsDetails.aspx` em um navegador. À primeira vista, pode parecer que tudo está funcionando conforme o desejado, mas há um problema sutil. Quando você escolhe uma nova categoria, o `ProductsByCategory` DropDownList é atualizado para incluir esses produtos para a categoria selecionada, mas o `ProductDetails` DetailsView continuou a mostrar as informações do produto anterior. O DetailsView é atualizado ao escolher um produto diferente para a categoria selecionada. Além disso, se você testar completamente o suficiente, descobrirá que, se você escolher continuamente novas categorias (como escolher bebidas do `Categories` DropDownList, depois condiments, então, doces) todas as outras seleções de categoria fazem com que o `ProductDetails` DetailsView seja atualizado.

Para ajudar a concretizarr esse problema, vamos dar uma olhada em um exemplo específico. Quando você visita a página pela primeira vez a categoria bebidas está selecionada e os produtos relacionados são carregados no `ProductsByCategory` DropDownList. Chai é o produto selecionado e seus detalhes são exibidos no `ProductDetails` DetailsView, como mostra a Figura 18.

[![os detalhes do produto selecionado são exibidos em um DetailsView](master-detail-filtering-with-two-dropdownlists-cs/_static/image53.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image52.png)

**Figura 18**: os detalhes do produto selecionado são exibidos em um DetailsView ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image54.png))

Se você alterar a seleção de categoria de bebidas para condiments, ocorrerá um postback e o `ProductsByCategory` DropDownList será atualizado de acordo, mas o DetailsView ainda exibirá detalhes de Chai.

[![os detalhes do produto selecionado anteriormente ainda são exibidos](master-detail-filtering-with-two-dropdownlists-cs/_static/image56.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image55.png)

**Figura 19**: os detalhes do produto selecionado anteriormente ainda são exibidos ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image57.png))

A escolha de um novo produto da lista atualiza o DetailsView conforme o esperado. Se você escolher uma nova categoria depois de alterar o produto, o DetailsView novamente não será atualizado. No entanto, se, em vez de escolher um novo produto, você selecionou uma nova categoria, o DetailsView seria atualizado. O que o mundo está acontecendo aqui?

O problema é um problema de tempo no ciclo de vida da página. Sempre que uma página é solicitada, ela prossegue por várias etapas como seu processamento. Em uma dessas etapas, os controles ObjectDataSource verificam se algum de seus `SelectParameters` valores foram alterados. Nesse caso, o controle da Web de dados associado ao ObjectDataSource sabe que ele precisa atualizar sua exibição. Por exemplo, quando uma nova categoria é selecionada, o `ProductsByCategoryDataSource` ObjectDataSource detecta que seus valores de parâmetro foram alterados e o `ProductsByCategory` DropDownList se reassocia a si mesmo, obtendo os produtos para a categoria selecionada.

O problema que surge nessa situação é que o ponto no ciclo de vida da página que os SQLDataSources verificam parâmetros alterados ocorre *antes* da reassociação dos controles da Web de dados associados. Portanto, ao selecionar uma nova categoria, o `ProductsByCategoryDataSource` ObjectDataSource detecta uma alteração no valor do parâmetro. O ObjectDataSource usado pelo `ProductDetails` DetailsView, no entanto, não anota essas alterações porque o `ProductsByCategory` DropDownList ainda precisa ser reassociado. Posteriormente no ciclo de vida, o `ProductsByCategory` DropDownList será reassociado a seu ObjectDataSource, captando os produtos para a categoria selecionada recentemente. Embora o valor da `ProductsByCategory` DropDownList tenha sido alterado, o ObjectDataSource da `ProductDetails` DetailsView já fez a verificação do valor do parâmetro; Portanto, o DetailsView exibe seus resultados anteriores. Essa interação é descrita na Figura 20.

[![o valor de DropDownList ProductsByCategory é alterado depois que o ObjectDataSource do ProductDetails DetailsView verifica se há alterações](master-detail-filtering-with-two-dropdownlists-cs/_static/image59.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image58.png)

**Figura 20**: o valor `ProductsByCategory` DropDownList é alterado depois que o ObjectDataSource do `ProductDetails` DetailsView verifica se há alterações ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image60.png))

Para corrigir isso, precisamos reassociar explicitamente o `ProductDetails` DetailsView depois que o `ProductsByCategory` DropDownList tiver sido associado. Podemos fazer isso chamando o método `DataBind()` do `ProductDetails` DetailsView quando o evento `DataBound` do `ProductsByCategory` DropDownList é acionado. Adicione o seguinte código de manipulador de eventos à classe code-behind da página de `MasterDetailsDetails.aspx` (consulte "[definindo programaticamente os valores de parâmetro de ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)" para obter uma discussão sobre como adicionar um manipulador de eventos):

[!code-csharp[Main](master-detail-filtering-with-two-dropdownlists-cs/samples/sample2.cs)]

Depois que essa chamada explícita para o método `DataBind()` do `ProductDetails` DetailsView tiver sido adicionada, o tutorial funcionará conforme o esperado. A figura 21 realça como essa alteração corrigiu nosso problema anterior.

[![o ProductDetails DetailsView é atualizado explicitamente quando o evento de ligação de eventos de ProductsByCategory DropDownList é acionado](master-detail-filtering-with-two-dropdownlists-cs/_static/image62.png)](master-detail-filtering-with-two-dropdownlists-cs/_static/image61.png)

**Figura 21**: a `ProductDetails` DetailsView é atualizada explicitamente quando o evento de `DataBound` da `ProductsByCategory` DropDownList é acionado ([clique para exibir a imagem em tamanho normal](master-detail-filtering-with-two-dropdownlists-cs/_static/image63.png))

## <a name="summary"></a>Resumo

O DropDownList serve como um elemento de interface de usuário ideal para relatórios mestre/detalhados em que há uma relação um-para-muitos entre os registros mestre e de detalhes. No tutorial anterior, vimos como usar uma única DropDownList para filtrar os produtos exibidos pela categoria selecionada. Neste tutorial, substituímos o GridView dos produtos por uma DropDownList e usavai um DetailsView para exibir os detalhes do produto selecionado. Os conceitos discutidos neste tutorial podem ser facilmente estendidos para modelos de dados que envolvem várias relações um-para-muitos, como clientes, pedidos e itens de pedidos. Em geral, você sempre pode adicionar uma DropDownList para cada uma das entidades "uma" nas relações um-para-muitos.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Hilton Giesenow. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-detail-filtering-with-a-dropdownlist-cs.md)
> [Próximo](master-detail-filtering-across-two-pages-cs.md)
