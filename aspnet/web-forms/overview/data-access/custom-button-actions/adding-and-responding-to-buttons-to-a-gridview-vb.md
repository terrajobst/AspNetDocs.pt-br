---
uid: web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
title: Adicionando e respondendo a botões para um GridView (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como adicionar botões personalizados, tanto a um modelo quanto aos campos de um controle GridView ou DetailsView. Em particular, vamos bui...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 06c6bbd2-4bdc-435b-87a3-df2c868f4baa
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-vb
msc.type: authoredcontent
ms.openlocfilehash: 8727d8faead02340d223c75845bf29f63d1a0834
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78549472"
---
# <a name="adding-and-responding-to-buttons-to-a-gridview-vb"></a>Adicionar e responder a botões em um GridView (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_28_VB.exe) ou [baixar PDF](adding-and-responding-to-buttons-to-a-gridview-vb/_static/datatutorial28vb1.pdf)

> Neste tutorial, veremos como adicionar botões personalizados, tanto a um modelo quanto aos campos de um controle GridView ou DetailsView. Em particular, criaremos uma interface que tem um FormView que permite ao usuário paginar os fornecedores.

## <a name="introduction"></a>Introdução

Embora muitos cenários de relatório envolvam acesso somente leitura aos dados do relatório, não é incomum que os relatórios incluam a capacidade de executar ações com base nos dados exibidos. Normalmente, isso envolvia a adição de um controle da Web Button, LinkButton ou ImageButton com cada registro exibido no relatório que, quando clicado, causa um postback e invoca algum código do servidor. A edição e a exclusão de dados em uma base registro por registro é o exemplo mais comum. Na verdade, como vimos começando com a [visão geral de inserção, atualização e exclusão](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) do tutorial de dados, editar e excluir é tão comum que os controles GridView, DetailsView e FormView possam dar suporte a essa funcionalidade sem a necessidade de escrever uma única linha de código.

Além dos botões editar e excluir, os controles GridView, DetailsView e FormView também podem incluir botões, LinkButtons ou ImageButtons que, quando clicados, executam alguma lógica personalizada no lado do servidor. Neste tutorial, veremos como adicionar botões personalizados, tanto a um modelo quanto aos campos de um controle GridView ou DetailsView. Em particular, criaremos uma interface que tem um FormView que permite ao usuário paginar os fornecedores. Para um determinado fornecedor, o FormView mostrará informações sobre o fornecedor junto com um controle da Web de botão que, se clicado, marcará todos os seus produtos associados como descontinuados. Além disso, um GridView lista os produtos fornecidos pelo fornecedor selecionado, com cada linha contendo os botões aumentar preço e preço de desconto que, se clicados, aumentam ou reduzem o número de `UnitPrice` do produto em 10% (consulte a Figura 1).

[![o FormView e o GridView contêm botões que executam ações personalizadas](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image2.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image1.png)

**Figura 1**: o FormView e o GridView contêm botões que executam ações personalizadas ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image3.png))

## <a name="step-1-adding-the-button-tutorial-web-pages"></a>Etapa 1: adicionando as páginas da Web do tutorial de botão

Antes de examinarmos como adicionar botões personalizados, vamos primeiro reservar um momento para criar as páginas ASP.NET em nosso projeto de site que precisaremos para este tutorial. Comece adicionando uma nova pasta chamada `CustomButtons`. Em seguida, adicione as duas páginas ASP.NET a seguir a essa pasta, assegurando a associação de cada página com a página mestra de `Site.master`:

- `Default.aspx`
- `CustomButtons.aspx`

![Adicionar as páginas ASP.NET para os tutoriais relacionados aos botões personalizados](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image4.png)

**Figura 2**: adicionar as páginas ASP.net para os tutoriais relacionados aos botões personalizados

Assim como nas outras pastas, `Default.aspx` na pasta `CustomButtons` listará os tutoriais em sua seção. Lembre-se de que o controle de usuário `SectionLevelTutorialListing.ascx` fornece essa funcionalidade. Portanto, adicione esse controle de usuário a `Default.aspx` arrastando-o da Gerenciador de Soluções para a página s modo de exibição de Design.

[![adicionar o controle de usuário SectionLevelTutorialListing. ascx a default. aspx](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image6.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image5.png)

**Figura 3**: Adicionar o controle de usuário `SectionLevelTutorialListing.ascx` ao `Default.aspx` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image7.png))

Por fim, adicione as páginas como entradas ao arquivo de `Web.sitemap`. Especificamente, adicione a seguinte marcação após a `<siteMapNode>`de paginação e classificação:

[!code-xml[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample1.xml)]

Depois de atualizar `Web.sitemap`, Reserve um momento para exibir o site de tutoriais por meio de um navegador. O menu à esquerda agora inclui itens para os tutoriais de edição, inserção e exclusão.

![O mapa do site agora inclui a entrada para o tutorial de botões personalizados](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image8.png)

**Figura 4**: o mapa do site agora inclui a entrada para o tutorial de botões personalizados

## <a name="step-2-adding-a-formview-that-lists-the-suppliers"></a>Etapa 2: adicionando um FormView que lista os fornecedores

Vamos começar com este tutorial adicionando o FormView que lista os fornecedores. Conforme discutido na introdução, este FormView permitirá que o usuário percorra os fornecedores, mostrando os produtos fornecidos pelo fornecedor em um GridView. Além disso, esse FormView incluirá um botão que, quando clicado, marcará todos os produtos do fornecedor como descontinuados. Antes de nos preocuparmos com a adição do botão personalizado ao FormView, vamos primeiro criar o FormView para que ele exiba as informações do fornecedor.

Comece abrindo a página de `CustomButtons.aspx` na pasta `CustomButtons`. Adicione um FormView à página arrastando-o da caixa de ferramentas para o designer e defina sua propriedade `ID` como `Suppliers`. Na marca inteligente FormView s, opte por criar um novo ObjectDataSource chamado `SuppliersDataSource`.

[![criar um novo ObjectDataSource chamado SuppliersDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image10.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image9.png)

**Figura 5**: criar um novo ObjectDataSource chamado `SuppliersDataSource` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image11.png))

Configure esse novo ObjectDataSource de modo que ele consulte o método de `GetSuppliers()` da classe `SuppliersBLL` (veja a Figura 6). Como este FormView não fornece uma interface para atualizar as informações do fornecedor, selecione a opção (nenhum) na lista suspensa na guia atualizar.

[![configurar a fonte de dados para usar o método SuppliersBLL da classe s getsuppliers ()](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image13.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image12.png)

**Figura 6**: configurar a fonte de dados para usar o método de `GetSuppliers()` da classe `SuppliersBLL` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image14.png))

Depois de configurar o ObjectDataSource, o Visual Studio irá gerar um `InsertItemTemplate`, `EditItemTemplate`e `ItemTemplate` para o FormView. Remova o `InsertItemTemplate` e `EditItemTemplate` e modifique o `ItemTemplate` para que ele exiba apenas o nome da empresa e o número de telefone do fornecedor. Por fim, ative o suporte de paginação para o FormView marcando a caixa de seleção habilitar paginação em sua marca inteligente (ou definindo sua propriedade `AllowPaging` como `True`). Após essas alterações, a marcação declarativa de s de página deve ser semelhante ao seguinte:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample2.aspx)]

A Figura 7 mostra a página CustomButtons. aspx quando exibida por meio de um navegador.

[![o FormView lista os campos CompanyName e Phone do fornecedor selecionado no momento](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image16.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image15.png)

**Figura 7**: o FormView lista os campos `CompanyName` e `Phone` do fornecedor atualmente selecionado ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image17.png))

## <a name="step-3-adding-a-gridview-that-lists-the-selected-supplier-s-products"></a>Etapa 3: adicionando um GridView que lista os produtos de fornecedor s selecionados

Antes de adicionarmos o botão descontinuar todos os produtos ao modelo FormView s, vamos primeiro adicionar um GridView abaixo do FormView que lista os produtos fornecidos pelo fornecedor selecionado. Para fazer isso, adicione um GridView à página, defina sua propriedade `ID` como `SuppliersProducts`e adicione um novo ObjectDataSource chamado `SuppliersProductsDataSource`.

[![criar um novo ObjectDataSource chamado SuppliersProductsDataSource](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image19.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image18.png)

**Figura 8**: criar um novo ObjectDataSource chamado `SuppliersProductsDataSource` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image20.png))

Configure esse ObjectDataSource para usar o método ProductsBLL da classe s `GetProductsBySupplierID(supplierID)` (consulte a Figura 9). Embora esse GridView permita que um preço de produto s seja ajustado, ele não usará os recursos internos de edição ou exclusão do GridView. Portanto, podemos definir a lista suspensa como (None) para as guias ObjectDataSource s UPDATE, INSERT e DELETE.

[![configurar a fonte de dados para usar o método ProductsBLL da classe s GetProductsBySupplierID (CódigoDoFornecedor)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image22.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image21.png)

**Figura 9**: configurar a fonte de dados para usar o método de `GetProductsBySupplierID(supplierID)` da classe `ProductsBLL` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image23.png))

Como o método `GetProductsBySupplierID(supplierID)` aceita um parâmetro de entrada, o assistente ObjectDataSource nos solicita a origem desse valor de parâmetro. Para passar o valor `SupplierID` de FormView, defina a lista suspensa origem do parâmetro como Control e a lista suspensa ControlID como `Suppliers` (a ID do FormView criada na etapa 2).

[![indicar que o parâmetro CódigoDoFornecedor deve vir do controle FormView de fornecedores](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image25.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image24.png)

**Figura 10**: indicar que o parâmetro *`supplierID`* deve vir do controle FormView de `Suppliers` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image26.png))

Depois de concluir o assistente ObjectDataSource, o GridView conterá um BoundField ou CheckBoxField para cada um dos campos de dados do produto. Vamos cortar isso para mostrar apenas o `ProductName` e `UnitPrice` BoundFields junto com o `Discontinued` CheckBoxField; Além disso, vamos formatar o `UnitPrice` BoundField de modo que seu texto seja formatado como uma moeda. Sua marcação declarativa de GridView e `SuppliersProductsDataSource` ObjectDataSource s deve ser semelhante à seguinte marcação:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample3.aspx)]

Neste ponto, nosso tutorial exibe um relatório mestre/detalhes, permitindo que o usuário escolha um fornecedor do FormView na parte superior e exiba os produtos fornecidos por esse fornecedor por meio do GridView na parte inferior. A Figura 11 mostra uma captura de tela desta página ao selecionar o fornecedor Tokyo Traders no FormView.

[![os produtos de fornecedor s selecionados são exibidos no GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image28.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image27.png)

**Figura 11**: os produtos de fornecedor s selecionados são exibidos no GridView ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image29.png))

## <a name="step-4-creating-dal-and-bll-methods-to-discontinue-all-products-for-a-supplier"></a>Etapa 4: criando métodos DAL e BLL para descontinuar todos os produtos de um fornecedor

Antes que possamos adicionar um botão ao FormView que, quando clicado, descontinua todos os produtos do fornecedor, precisamos primeiro adicionar um método tanto à DAL quanto à BLL que executa essa ação. Em particular, esse método será nomeado `DiscontinueAllProductsForSupplier(supplierID)`. Quando o botão FormView s é clicado, invocamos esse método na camada de lógica de negócios, passando o s `SupplierID`do fornecedor selecionado; em seguida, a BLL chamará para o método de camada de acesso a dados correspondente, que emitirá uma instrução `UPDATE` para o Database que descontinua os produtos do fornecedor especificado.

Como fizemos em nossos tutoriais anteriores, usaremos uma abordagem de baixo para cima, começando pela criação do método DAL, o método BLL e, finalmente, implementando a funcionalidade na página ASP.NET. Abra o `Northwind.xsd` DataSet tipado na pasta `App_Code/DAL` e adicione um novo método à `ProductsTableAdapter` (clique com o botão direito do mouse no `ProductsTableAdapter` e escolha Adicionar consulta). Isso abrirá o assistente de configuração de consulta do TableAdapter, que nos orienta pelo processo de adição do novo método. Comece indicando que nosso método DAL usará uma instrução SQL ad hoc.

[![criar o método DAL usando uma instrução SQL ad hoc](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image31.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image30.png)

**Figura 12**: criar o método Dal usando uma instrução SQL ad hoc ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image32.png))

Em seguida, o assistente nos solicitará o tipo de consulta a ser criada. Como o método `DiscontinueAllProductsForSupplier(supplierID)` precisará atualizar a tabela de banco de dados `Products`, definindo o campo `Discontinued` como 1 para todos os produtos fornecidos pelo *`supplierID`* especificado, precisamos criar uma consulta que atualiza os dados.

[![escolher o tipo de consulta de atualização](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image34.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image33.png)

**Figura 13**: escolha o tipo de consulta de atualização ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image35.png))

A próxima tela do assistente fornece a instrução `UPDATE` do TableAdapter s existente, que atualiza cada um dos campos definidos na DataTable `Products`. Substitua este texto da consulta pela seguinte instrução:

[!code-sql[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample4.sql)]

Depois de inserir essa consulta e clicar em avançar, a última tela do assistente solicitará o novo nome do método. Use `DiscontinueAllProductsForSupplier`. Conclua o assistente clicando no botão Concluir. Ao retornar ao designer de DataSet, você verá um novo método no `ProductsTableAdapter` chamado `DiscontinueAllProductsForSupplier(@SupplierID)`.

[![nomear o novo método DAL DiscontinueAllProductsForSupplier](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image37.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image36.png)

**Figura 14**: nomear o método DAL novo `DiscontinueAllProductsForSupplier` ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image38.png))

Com o método `DiscontinueAllProductsForSupplier(supplierID)` criado na camada de acesso a dados, nossa próxima tarefa é criar o método `DiscontinueAllProductsForSupplier(supplierID)` na camada de lógica de negócios. Para fazer isso, abra o arquivo de classe `ProductsBLL` e adicione o seguinte:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample5.vb)]

Esse método simplesmente chama para o método `DiscontinueAllProductsForSupplier(supplierID)` na DAL, passando o valor do parâmetro *`supplierID`* fornecido. Se houvesse alguma regra de negócio que permitisse a descontinuação de produtos de um fornecedor em determinadas circunstâncias, essas regras devem ser implementadas aqui, na BLL.

> [!NOTE]
> Ao contrário das sobrecargas de `UpdateProduct` na classe `ProductsBLL`, a assinatura do método `DiscontinueAllProductsForSupplier(supplierID)` não inclui o atributo `DataObjectMethodAttribute` (`<System.ComponentModel.DataObjectMethodAttribute(System.ComponentModel.DataObjectMethodType.Update, Boolean)>`). Isso impede o método `DiscontinueAllProductsForSupplier(supplierID)` da lista suspensa ObjectDataSource s configurar a fonte de dados do assistente na guia atualizar. Eu omiti esse atributo porque iremos chamar o método `DiscontinueAllProductsForSupplier(supplierID)` diretamente de um manipulador de eventos em nossa página ASP.NET.

## <a name="step-5-adding-a-discontinue-all-products-button-to-the-formview"></a>Etapa 5: adicionar um botão descontinuar todos os produtos ao FormView

Com o método `DiscontinueAllProductsForSupplier(supplierID)` na BLL e na DAL concluídas, a etapa final para adicionar a capacidade de descontinuar todos os produtos para o fornecedor selecionado é adicionar um controle de botão da Web ao `ItemTemplate`de FormView s. Deixe que o s adicione um botão abaixo do número de telefone do fornecedor com o texto do botão, descontinue todos os produtos e um valor de propriedade `ID` de `DiscontinueAllProductsForSupplier`. Você pode adicionar esse controle de botão da Web por meio do Designer clicando no link editar modelos na marca inteligente FormView s (consulte a Figura 15) ou diretamente por meio da sintaxe declarativa.

[![adicionar um controle da Web do botão descontinuar todos os produtos ao ItemTemplate s do FormView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image40.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image39.png)

**Figura 15**: adicionar um controle da Web do botão "descontinuar todos os produtos" para a `ItemTemplate` de FormView s ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image41.png))

Quando o botão é clicado por um usuário que visita a página, um postback massacre e o [evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.itemcommand.aspx) FormView s`ItemCommand` são acionados. Para executar código personalizado em resposta a esse botão ser clicado, podemos criar um manipulador de eventos para esse evento. No entanto, entenda que o evento `ItemCommand` é acionado sempre que *um* controle da Web Button, LinkButton ou ImageButton é clicado dentro do FormView. Isso significa que quando o usuário se move de uma página para outra no FormView, o evento `ItemCommand` é acionado; a mesma coisa quando o usuário clica em novo, editar ou excluir em um FormView que dá suporte à inserção, atualização ou exclusão.

Como o `ItemCommand` é acionado independentemente de qual botão é clicado, no manipulador de eventos, precisamos de uma maneira para determinar se o botão descontinuar todos os produtos foi clicado ou se foi um outro botão. Para fazer isso, podemos definir o botão controle da Web s `CommandName` Propriedade como um valor de identificação. Quando o botão é clicado, esse valor `CommandName` é passado para o manipulador de eventos `ItemCommand`, permitindo que possamos determinar se o botão descontinuar todos os produtos foi o botão clicado. Defina o botão descontinuar todos os produtos `CommandName` Propriedade como DiscontinueProducts.

Por fim, vamos usar uma caixa de diálogo de confirmação do lado do cliente para garantir que o usuário realmente queira interromper os produtos selecionados do fornecedor. Como vimos na adição de [confirmação do lado do cliente ao excluir](../editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb.md) o tutorial, isso pode ser feito com um pouco de JavaScript. Em particular, defina o botão controle da Web Propriedade s OnClientClick como `return confirm('This will mark _all_ of this supplier\'s products as discontinued. Are you certain you want to do this?');`

Depois de fazer essas alterações, a sintaxe declarativa s de FormView deve ser parecida com a seguinte:

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample6.aspx)]

Em seguida, crie um manipulador de eventos para o evento FormView s `ItemCommand`. Nesse manipulador de eventos, precisamos determinar primeiro se o botão descontinuar todos os produtos foi clicado. Nesse caso, desejamos criar uma instância da classe `ProductsBLL` e invocar seu método `DiscontinueAllProductsForSupplier(supplierID)`, passando a `SupplierID` do FormView selecionado:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample7.vb)]

Observe que a `SupplierID` do fornecedor selecionado atual no FormView pode ser acessada usando a Propriedade FormView s [`SelectedValue`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formview.selectedvalue.aspx). A propriedade `SelectedValue` retorna o primeiro valor de chave de dados para o registro que está sendo exibido no FormView. A Propriedade FormView s [`DataKeyNames`](https://msdn.microsoft.com/system.web.ui.webcontrols.formview.datakeynames.aspx), que indica os campos de dados dos quais os valores de chave de dados são extraídos, foi automaticamente definido como `SupplierID` pelo Visual Studio ao ligar o ObjectDataSource ao FormView de volta na etapa 2.

Com o manipulador de eventos `ItemCommand` criado, Reserve um momento para testar a página. Navegue até o fornecedor cooperativa de Quesos ' las cabras ' (ele é o quinto fornecedor no FormView para mim). Esse fornecedor fornece dois produtos, Queso Cabrales e Queso Manchego la-pastora, ambos *não* são descontinuados.

Imagine que cooperativa de Quesos ' las cabras ' saiu dos negócios e, portanto, seus produtos devem ser descontinuados. Clique no botão descontinuar todos os produtos. Isso exibirá a caixa de diálogo de confirmação do lado do cliente (consulte a Figura 16).

[![cooperativa de Quesos las cabras fornece dois produtos ativos](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image43.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image42.png)

**Figura 16**: cooperativa de Quesos las cabras fornece dois produtos ativos ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image44.png))

Se você clicar em OK na caixa de diálogo de confirmação do lado do cliente, o envio do formulário continuará, causando um postback no qual o evento FormView s `ItemCommand` será acionado. O manipulador de eventos que criaremos será executado, invocando o método `DiscontinueAllProductsForSupplier(supplierID)` e descontinuando os produtos Queso Cabrales e Queso Manchego la.

Se você tiver desabilitado o estado de exibição de GridView s, o GridView será reassociado ao armazenamento de dados subjacente em cada postback e, portanto, será atualizado imediatamente para refletir que esses dois produtos agora são descontinuados (veja a figura 17). Se, no entanto, você não tiver desabilitado o estado de exibição no GridView, será necessário reassociar manualmente os dados ao GridView depois de fazer essa alteração. Para fazer isso, basta fazer uma chamada para o método GridView s `DataBind()` imediatamente após invocar o método `DiscontinueAllProductsForSupplier(supplierID)`.

[![depois de clicar no botão descontinuar todos os produtos, os produtos do fornecedor serão atualizados de acordo](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image46.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image45.png)

**Figura 17**: depois de clicar no botão descontinuar todos os produtos, os produtos do fornecedor são atualizados de acordo ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image47.png))

## <a name="step-6-creating-an-updateproduct-overload-in-the-business-logic-layer-for-adjusting-a-product-s-price"></a>Etapa 6: criando uma sobrecarga de UpdateProduct na camada de lógica de negócios para ajustar o preço de um produto

Assim como com o botão descontinuar todos os produtos no FormView, para adicionar botões para aumentar e diminuir o preço de um produto no GridView, precisamos primeiro adicionar os métodos apropriados de camada de acesso a dados e de lógica de negócios. Como já temos um método que atualiza uma única linha de produto na DAL, podemos fornecer essa funcionalidade criando uma nova sobrecarga para o método `UpdateProduct` na BLL.

Nossas últimas `UpdateProduct` sobrecargas foram feitas em alguma combinação de campos de produto como valores de entrada escalares e, em seguida, atualizaram apenas esses campos para o produto especificado. Para essa sobrecarga, Vamos variar um pouco desse padrão e, em vez disso, passar o produto s `ProductID` e o percentual pelo qual ajustar o `UnitPrice` (em vez de passar o novo, ajustado `UnitPrice` em si). Essa abordagem simplificará o código que precisamos para escrever na classe code-behind da página ASP.NET, já que não precisamos nos preocupar em determinar a `UnitPrice`do produto atual.

A sobrecarga de `UpdateProduct` para este tutorial é mostrada abaixo:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample8.vb)]

Essa sobrecarga recupera informações sobre o produto especificado por meio do método `GetProductByProductID(productID)` s DAL. Em seguida, ele verifica se o `UnitPrice` do produto é atribuído a um valor de `NULL` de banco de dados. Se for, o preço será deixado inalterado. No entanto, se houver um valor de `UnitPrice` não`NULL`, o método atualizará o `UnitPrice` do produto pela porcentagem especificada (`unitPriceAdjustmentPercent`).

## <a name="step-7-adding-the-increase-and-decrease-buttons-to-the-gridview"></a>Etapa 7: adicionar os botões aumentar e diminuir ao GridView

O GridView (e DetailsView) são compostos de uma coleção de campos. Além de BoundFields, CheckBoxFields e TemplateFields, o ASP.NET inclui o ButtonField, que, como seu nome sugere, é renderizado como uma coluna com um Button, LinkButton ou ImageButton para cada linha. Semelhante ao FormView, clicar em *qualquer* botão nos botões de paginação GridView, editar ou excluir botões, classificar botões e assim por diante causa um postback e gera o [evento](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowcommand.aspx)GridView s`RowCommand`.

O ButtonField tem uma propriedade `CommandName` que atribui o valor especificado a cada um de seus botões `CommandName` Propriedades. Assim como com o FormView, o valor de `CommandName` é usado pelo manipulador de eventos `RowCommand` para determinar qual botão foi clicado.

Vamos adicionar dois novos ButtonFields ao GridView, um com um preço de texto de botão + 10% e outro com o preço de texto-10%. Para adicionar esses ButtonFields, clique no link Editar colunas na marca inteligente GridView s, selecione o tipo de campo ButtonField na lista no canto superior esquerdo e clique no botão Adicionar.

![Adicionar dois ButtonFields ao GridView](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image48.png)

**Figura 18**: adicionar dois ButtonFields ao GridView

Mova os dois ButtonFields para que eles apareçam como os dois primeiros campos GridView. Em seguida, defina as propriedades `Text` dessas duas ButtonFields como Price + 10% e Price-10% e as propriedades `CommandName` como IncreasePrice e DecreasePrice, respectivamente. Por padrão, um ButtonField renderiza sua coluna de botões como LinkButtons. No entanto, isso pode ser alterado por meio da [Propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.buttonfieldbase.buttontype.aspx)ButtonField s`ButtonType`. Vamos ter esses dois ButtonFields processados como botões de push regulares; Portanto, defina a propriedade `ButtonType` como `Button`. A Figura 19 mostra a caixa de diálogo campos após essas alterações terem sido feitas; a seguir, essa é a marcação declarativa s de GridView.

![Configurar as propriedades Text, CommandName e ButtonType do ButtonFields](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image49.png)

**Figura 19**: configurar as propriedades `Text`, `CommandName`e `ButtonType` ButtonFields

[!code-aspx[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample9.aspx)]

Com esses ButtonFields criados, a etapa final é criar um manipulador de eventos para o evento GridView s `RowCommand`. Esse manipulador de eventos, se disparado porque os botões preço + 10% ou preço-10% foram clicados, precisa determinar o `ProductID` para a linha cujo botão foi clicado e, em seguida, invocar o método `ProductsBLL` classe s `UpdateProduct`, passando o ajuste de porcentagem de `UnitPrice` apropriado junto com o `ProductID`. O código a seguir executa estas tarefas:

[!code-vb[Main](adding-and-responding-to-buttons-to-a-gridview-vb/samples/sample10.vb)]

Para determinar o `ProductID` para a linha cujo botão de preço + 10% ou preço-10% foi clicado, precisamos consultar a coleção GridView s `DataKeys`. Essa coleção contém os valores dos campos especificados na propriedade `DataKeyNames` para cada linha GridView. Como a Propriedade GridView s `DataKeyNames` foi definida como ProductID pelo Visual Studio ao associar o ObjectDataSource ao GridView, `DataKeys(rowIndex).Value` fornece a `ProductID` para o *RowIndex*especificado.

O ButtonField passa automaticamente no *RowIndex* da linha cujo botão foi clicado através do parâmetro `e.CommandArgument`. Portanto, para determinar a `ProductID` para a linha cujo botão de preço + 10% ou preço-10% foi clicado, usamos: `Convert.ToInt32(SuppliersProducts.DataKeys(Convert.ToInt32(e.CommandArgument)).Value)`.

Assim como no botão descontinuar todos os produtos, se você tiver desabilitado o estado de exibição GridView, o GridView será reassociado ao armazenamento de dados subjacente em cada postback e, portanto, será atualizado imediatamente para refletir uma alteração de preço que ocorre de clicar em qualquer um dos botões. Se, no entanto, você não tiver desabilitado o estado de exibição no GridView, será necessário reassociar manualmente os dados ao GridView depois de fazer essa alteração. Para fazer isso, basta fazer uma chamada para o método GridView s `DataBind()` imediatamente após invocar o método `UpdateProduct`.

A figura 20 mostra a página ao exibir os produtos fornecidos pelo Homestead de avó. A figura 21 mostra os resultados depois que o botão Price + 10% foi clicado duas vezes para a boysenberry Spread da avó e o botão Price-10% uma vez para Northwoods Cranberry ingrediente.

[![o GridView inclui os botões Price + 10% e Price-10%](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image51.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image50.png)

**Figura 20**: o GridView inclui os botões Price + 10% e Price-10% ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image52.png))

[![os preços do primeiro e do terceiro produto foram atualizados por meio dos botões preço + 10% e preço-10%](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image54.png)](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image53.png)

**Figura 21**: os preços para o primeiro e o terceiro produto foram atualizados por meio dos botões Price + 10% e Price-10% ([clique para exibir a imagem em tamanho normal](adding-and-responding-to-buttons-to-a-gridview-vb/_static/image55.png))

> [!NOTE]
> O GridView (e DetailsView) também pode ter botões, LinkButtons ou ImageButtons adicionados ao seu TemplateFields. Assim como com o BoundField, esses botões, quando clicados, induzirão a um postback, gerando o evento GridView s `RowCommand`. No entanto, ao adicionar botões em um TemplateField, o botão s `CommandArgument` não é definido automaticamente como o índice da linha como está ao usar ButtonFields. Se você precisar determinar o índice de linha do botão que foi clicado dentro do manipulador de eventos `RowCommand`, você precisará definir manualmente a propriedade s `CommandArgument` de botão em sua sintaxe declarativa dentro do TemplateField, usando um código como:  
> `<asp:Button runat="server" ... CommandArgument='<%# CType(Container, GridViewRow).RowIndex %>' />`.

## <a name="summary"></a>Resumo

Todos os controles GridView, DetailsView e FormView podem incluir botões, LinkButtons ou ImageButtons. Esses botões, quando clicados, causam um postback e geram o evento `ItemCommand` nos controles FormView e DetailsView e o evento `RowCommand` no GridView. Esses controles da Web de dados têm funcionalidade interna para lidar com ações comuns relacionadas a comandos, como excluir ou editar registros. No entanto, também podemos usar botões que, quando clicados, respondem com a execução de nosso próprio código personalizado.

Para fazer isso, precisamos criar um manipulador de eventos para o `ItemCommand` ou `RowCommand` evento. Nesse manipulador de eventos, primeiro verificamos o valor de entrada `CommandName` para determinar qual botão foi clicado e, em seguida, tomar a ação personalizada apropriada. Neste tutorial, vimos como usar os botões e ButtonFields para descontinuar todos os produtos de um fornecedor especificado ou para aumentar ou diminuir o preço de um produto específico em 10%.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-and-responding-to-buttons-to-a-gridview-cs.md)
