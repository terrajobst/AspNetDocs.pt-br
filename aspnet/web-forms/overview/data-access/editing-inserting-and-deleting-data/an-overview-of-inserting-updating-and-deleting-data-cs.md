---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
title: Uma visão geral de inserção, atualização e exclusão de dados (C#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como mapear os métodos Insert (), Update () e Delete () de ObjectDataSource para os métodos de classes de BLL, bem como para como configu...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: b651dc58-93c7-4f83-a74e-3b99f6d60848
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs
msc.type: authoredcontent
ms.openlocfilehash: e26b8e841f86272a158b0c09b62ab2790d01d191
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78592739"
---
# <a name="an-overview-of-inserting-updating-and-deleting-data-c"></a>Uma visão geral de inserção, atualização e exclusão de dados (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_16_CS.exe) ou [baixar PDF](an-overview-of-inserting-updating-and-deleting-data-cs/_static/datatutorial16cs1.pdf)

> Neste tutorial, veremos como mapear os métodos Insert (), Update () e Delete () de ObjectDataSource para os métodos de classes de BLL, bem como para configurar os controles GridView, DetailsView e FormView para fornecer recursos de modificação de dados.

## <a name="introduction"></a>Introdução

Nos últimos vários tutoriais examinamos como exibir dados em uma página ASP.NET usando os controles GridView, DetailsView e FormView. Esses controles simplesmente funcionam com os dados fornecidos a eles. Normalmente, esses controles acessam dados por meio do uso de um controle da fonte de dados, como o ObjectDataSource. Vimos como o ObjectDataSource atua como um proxy entre a página ASP.NET e os dados subjacentes. Quando um GridView precisa exibir dados, ele invoca o método de `Select()` de ObjectDataSource que, por sua vez, invoca um método de nossa camada de lógica de negócios (BLL), que chama um método no TableAdapter (DAL) apropriado da camada de acesso a dados que, por sua vez, envia uma consulta de `SELECT` para o Northwind.

Lembre-se de que quando criamos os TableAdapters na DAL em [nosso primeiro tutorial](../introduction/creating-a-data-access-layer-cs.md), o Visual Studio adicionou automaticamente os métodos para inserção, atualização e exclusão de dados da tabela de banco de dado subjacente. Além disso, ao [criar uma camada de lógica de negócios](../introduction/creating-a-business-logic-layer-cs.md) , criamos métodos na BLL que se chamaram para esses métodos da Dal de modificação de dados.

Além de seu método `Select()`, o ObjectDataSource também tem os métodos `Insert()`, `Update()`e `Delete()`. Como o método `Select()`, esses três métodos podem ser mapeados para métodos em um objeto subjacente. Quando configurado para inserir, atualizar ou excluir dados, os controles GridView, DetailsView e FormView fornecem uma interface do usuário para modificar os dados subjacentes. Essa interface do usuário chama os métodos `Insert()`, `Update()`e `Delete()` do ObjectDataSource, que invocam os métodos associados do objeto subjacente (consulte a Figura 1).

[![os métodos Insert (), Update () e Delete () do ObjectDataSource servem como um proxy para a BLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image2.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image1.png)

**Figura 1**: os métodos `Insert()`, `Update()`e `Delete()` do ObjectDataSource servem como um proxy para a BLL ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image3.png))

Neste tutorial, veremos como mapear os métodos `Insert()`, `Update()`e `Delete()` do ObjectDataSource para métodos de classes na BLL, bem como configurar os controles GridView, DetailsView e FormView para fornecer recursos de modificação de dados.

## <a name="step-1-creating-the-insert-update-and-delete-tutorials-web-pages"></a>Etapa 1: criando as páginas da Web de tutoriais de inserção, atualização e exclusão

Antes de começarmos a explorar como inserir, atualizar e excluir dados, primeiro vamos reservar um momento para criar as páginas ASP.NET em nosso projeto de site, que precisaremos para este tutorial e para as próximas várias delas. Comece adicionando uma nova pasta chamada `EditInsertDelete`. Em seguida, adicione as seguintes páginas ASP.NET a essa pasta, assegurando a associação de cada página com a página mestra `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `DataModificationEvents.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`

![Adicionar as páginas ASP.NET para os tutoriais relacionados à modificação de dados](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image4.png)

**Figura 2**: adicionar as páginas ASP.net para os tutoriais relacionados à modificação de dados

Assim como nas outras pastas, `Default.aspx` na pasta `EditInsertDelete` listará os tutoriais em sua seção. Lembre-se de que o controle de usuário `SectionLevelTutorialListing.ascx` fornece essa funcionalidade. Portanto, adicione esse controle de usuário a `Default.aspx` arrastando-o da Gerenciador de Soluções para a modo de exibição de Design da página.

[![adicionar o controle de usuário SectionLevelTutorialListing. ascx a default. aspx](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image6.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image5.png)

**Figura 3**: Adicionar o controle de usuário `SectionLevelTutorialListing.ascx` ao `Default.aspx` ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image7.png))

Por fim, adicione as páginas como entradas ao arquivo de `Web.sitemap`. Especificamente, adicione a seguinte marcação após a formatação personalizada `<siteMapNode>`:

[!code-xml[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample1.xml)]

Depois de atualizar `Web.sitemap`, Reserve um momento para exibir o site de tutoriais por meio de um navegador. O menu à esquerda agora inclui itens para os tutoriais de edição, inserção e exclusão.

![O mapa do site agora inclui entradas para os tutoriais de edição, inserção e exclusão](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image8.png)

**Figura 4**: o mapa do site agora inclui entradas para os tutoriais de edição, inserção e exclusão

## <a name="step-2-adding-and-configuring-the-objectdatasource-control"></a>Etapa 2: adicionando e Configurando o controle ObjectDataSource

Como GridView, DetailsView e FormView diferem em seus recursos e layout de modificação de dados, vamos examinar cada um deles individualmente. No entanto, em vez de ter cada controle usando seu próprio ObjectDataSource, vamos criar apenas um ObjectDataSource que todos os exemplos de controle possam compartilhar.

Abra a página `Basics.aspx`, arraste um ObjectDataSource da caixa de ferramentas para o designer e clique no link configurar fonte de dados de sua marca inteligente. Como a `ProductsBLL` é a única classe BLL que fornece os métodos de edição, inserção e exclusão, configure o ObjectDataSource para usar essa classe.

[![configurar o ObjectDataSource para usar a classe ProductsBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image10.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image9.png)

**Figura 5**: configurar o ObjectDataSource para usar a classe `ProductsBLL` ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image11.png))

Na próxima tela, podemos especificar quais métodos da classe `ProductsBLL` são mapeados para o `Select()`do ObjectDataSource, `Insert()`, `Update()`e `Delete()` selecionando a guia apropriada e escolhendo o método na lista suspensa. A Figura 6, que deve parecer familiar agora, mapeia o método `Select()` do ObjectDataSource para o método `GetProducts()` da classe `ProductsBLL`. Os métodos `Insert()`, `Update()`e `Delete()` podem ser configurados selecionando a guia apropriada na lista ao longo da parte superior.

[![fazer com que o ObjectDataSource retorne todos os produtos](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image13.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image12.png)

**Figura 6**: fazer com que o ObjectDataSource retorne todos os produtos ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image14.png))

As figuras 7, 8 e 9 mostram as guias UPDATE, INSERT e DELETE do ObjectDataSource. Configure essas guias para que os métodos `Insert()`, `Update()`e `Delete()` invoquem os métodos `UpdateProduct`, `AddProduct`e `DeleteProduct` da classe `ProductsBLL`, respectivamente.

[![mapear o método Update () do ObjectDataSource para o método UpdateProduct da classe ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image16.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image15.png)

**Figura 7**: mapear o método de `Update()` do ObjectDataSource para o método de `UpdateProduct` da classe `ProductBLL` ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image17.png))

[![mapear o método Insert () do ObjectDataSource para o método addproduct da classe ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image19.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image18.png)

**Figura 8**: mapear o método de `Insert()` do ObjectDataSource para o método Add `Product` da classe `ProductBLL` ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image20.png))

[![mapear o método Delete () do ObjectDataSource para o método DeleteProduct da classe ProductBLL](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image22.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image21.png)

**Figura 9**: mapear o método de `Delete()` do ObjectDataSource para o método de `DeleteProduct` da classe `ProductBLL` ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image23.png))

Talvez você tenha notado que as listas suspensas nas guias atualizar, inserir e excluir já tinham esses métodos selecionados. Isso é obrigado ao nosso uso do `DataObjectMethodAttribute` que decora os métodos de `ProductsBLL`. Por exemplo, o método DeleteProduct tem a seguinte assinatura:

[!code-csharp[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample2.cs)]

O atributo `DataObjectMethodAttribute` indica a finalidade de cada método, seja para seleção, inserção, atualização ou exclusão, e se é o valor padrão. Se você omitir esses atributos ao criar suas classes de BLL, precisará selecionar manualmente os métodos nas guias atualizar, inserir e excluir.

Depois de garantir que os métodos de `ProductsBLL` apropriados sejam mapeados para os métodos `Insert()`, `Update()`e `Delete()` do ObjectDataSource, clique em concluir para concluir o assistente.

## <a name="examining-the-objectdatasources-markup"></a>Examinando a marcação do ObjectDataSource

Depois de configurar o ObjectDataSource por meio de seu assistente, vá para a exibição de código-fonte para examinar a marcação declarativa gerada. A marca `<asp:ObjectDataSource>` especifica o objeto subjacente e os métodos a serem invocados. Além disso, há `DeleteParameters`, `UpdateParameters`e `InsertParameters` que mapeiam para os parâmetros de entrada dos métodos `AddProduct`, `UpdateProduct`e `DeleteProduct` da classe `ProductsBLL`:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample3.aspx)]

O ObjectDataSource inclui um parâmetro para cada um dos parâmetros de entrada para seus métodos associados, assim como uma lista de `SelectParameter` s está presente quando o ObjectDataSource está configurado para chamar um método Select que espera um parâmetro de entrada (como `GetProductsByCategoryID(categoryID)`). Como veremos em breve, os valores para esses `DeleteParameters`, `UpdateParameters`e `InsertParameters` são definidos automaticamente pelo GridView, DetailsView e FormView antes de invocar o método `Insert()`, `Update()`ou `Delete()` do ObjectDataSource. Esses valores também podem ser definidos programaticamente conforme necessário, como discutiremos em um tutorial futuro.

Um efeito colateral de usar o assistente para configurar o para ObjectDataSource é que o Visual Studio define a [propriedade OldValuesParameterFormatString](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.oldvaluesparameterformatstring(VS.80).aspx) como `original_{0}`. Esse valor de propriedade é usado para incluir os valores originais dos dados que estão sendo editados e é útil em dois cenários:

- Se, ao editar um registro, os usuários forem capazes de alterar o valor da chave primária. Nesse caso, o novo valor de chave primária e o valor da chave primária original devem ser fornecidos para que o registro com o valor da chave primária original possa ser encontrado e ter seu valor atualizado de acordo.
- Ao usar a simultaneidade otimista. A simultaneidade otimista é uma técnica para garantir que dois usuários simultâneos não substituam as alterações de outra, e é o tópico para um tutorial futuro.

A propriedade `OldValuesParameterFormatString` indica o nome dos parâmetros de entrada nos métodos Update e Delete do objeto subjacente para os valores originais. Discutiremos essa propriedade e sua finalidade mais detalhadamente quando explorarmos a simultaneidade otimista. No entanto, faço isso agora porque nossos métodos de BLL não esperam os valores originais e, portanto, é importante que removamos essa propriedade. Deixar a propriedade `OldValuesParameterFormatString` definida como qualquer coisa diferente do padrão (`{0}`) causará um erro quando um controle da Web de dados tentar invocar os métodos `Update()` ou `Delete()` de ObjectDataSource, pois o ObjectDataSource tentará passar o `UpdateParameters` ou `DeleteParameters` especificado, bem como os parâmetros de valor original.

Se isso não for extremamente claro neste momento, não se preocupe, vamos examinar essa propriedade e seu utilitário em um tutorial futuro. Por enquanto, apenas tenha certeza de remover essa declaração de propriedade por completo da sintaxe declarativa ou definir o valor para o valor padrão ({0}).

> [!NOTE]
> Se você simplesmente desmarcar o valor da propriedade `OldValuesParameterFormatString` da janela Propriedades no modo de exibição de Design, a propriedade ainda existirá na sintaxe declarativa, mas será definida como uma cadeia de caracteres vazia. Isso, infelizmente, ainda resultará no mesmo problema discutido acima. Portanto, remova a propriedade completamente da sintaxe declarativa ou, na janela Propriedades, defina o valor como o padrão, `{0}`.

## <a name="step-3-adding-a-data-web-control-and-configuring-it-for-data-modification"></a>Etapa 3: adicionando um controle da Web de dados e Configurando-o para modificação de dados

Depois que o ObjectDataSource tiver sido adicionado à página e configurado, estamos prontos para adicionar controles da Web de dados à página para exibir os dados e fornecer um meio para o usuário final modificá-lo. Veremos o GridView, o DetailsView e o FormView separadamente, pois esses controles da Web de dados diferem em suas configurações e recursos de modificação de dados.

Como veremos no restante deste artigo, adicionar suporte a edição, inserção e exclusão muito básicos por meio dos controles GridView, DetailsView e FormView é realmente tão simples quanto marcar algumas caixas de seleção. Há muitas sutilezas e casos de borda no mundo real que tornam essa funcionalidade mais envolvida do que apenas apontar e clicar. No entanto, este tutorial se concentra exclusivamente na prova de recursos de modificação de dados simplista. Os tutoriais futuros examinarão as preocupações que, sem dúvida, surgirão em uma configuração do mundo real.

## <a name="deleting-data-from-the-gridview"></a>Excluindo dados do GridView

Comece arrastando um GridView da caixa de ferramentas para o designer. Em seguida, associe o ObjectDataSource ao GridView selecionando-o na lista suspensa na marca inteligente do GridView. Neste ponto, a marcação declarativa do GridView será:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample4.aspx)]

Associar o GridView ao ObjectDataSource por meio de sua marca inteligente tem dois benefícios:

- Os BoundFields e CheckBoxFields são criados automaticamente para cada um dos campos retornados pelo ObjectDataSource. Além disso, as propriedades BoundField e CheckBoxField são definidas com base nos metadados do campo subjacente. Por exemplo, os campos `ProductID`, `CategoryName`e `SupplierName` são marcados como somente leitura no `ProductsDataTable` e, portanto, não devem ser atualizáveis durante a edição. Para acomodar isso, [as propriedades ReadOnly](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.readonly(VS.80).aspx) de boundfields são definidas como `true`.
- A [propriedade DataKeyNames](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.datakeynames(VS.80).aspx) é atribuída aos campos de chave primária do objeto subjacente. Isso é essencial ao usar o GridView para editar ou excluir dados, pois essa propriedade indica o campo (ou conjunto de campos) que identifica exclusivamente cada registro. Para obter mais informações sobre a propriedade `DataKeyNames`, consulte o [mestre/detalhes usando um GridView mestre selecionável com um tutorial de detalhes detailview](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) .

Embora o GridView possa ser associado ao ObjectDataSource por meio da sintaxe janela Propriedades ou declarativa, fazer isso exige que você adicione manualmente a marcação de BoundField e `DataKeyNames` apropriada.

O controle GridView fornece suporte interno para edição e exclusão em nível de linha. Configurar um GridView para dar suporte à exclusão adiciona uma coluna de botões de exclusão. Quando o usuário final clica no botão excluir para uma linha específica, um postback massacre e o GridView executa as seguintes etapas:

1. Os valores de `DeleteParameters` do ObjectDataSource são atribuídos
2. O método de `Delete()` do ObjectDataSource é invocado, excluindo o registro especificado
3. O GridView se reassocia ao ObjectDataSource invocando seu método de `Select()`

Os valores atribuídos à `DeleteParameters` são os valores dos campos de `DataKeyNames` para a linha cujo botão de exclusão foi clicado. Portanto, é vital que a propriedade `DataKeyNames` de um GridView seja definida corretamente. Se ele estiver ausente, o `DeleteParameters` será atribuído a um valor de `null` na etapa 1, que por sua vez não resultará em quaisquer registros excluídos na etapa 2.

> [!NOTE]
> A coleção de `DataKeys` é armazenada no estado de controle GridView s, o que significa que os valores de `DataKeys` serão lembrados por postagem, mesmo que o estado de exibição de GridView s tenha sido desabilitado. No entanto, é muito importante que o estado de exibição permaneça habilitado para GridViews que dão suporte à edição ou exclusão (o comportamento padrão). Se você definir a Propriedade GridView s `EnableViewState` como `false`, o comportamento de edição e exclusão funcionará bem para um único usuário, mas se houver usuários simultâneos excluindo dados, existirá a possibilidade de que esses usuários simultâneos excluam ou editem os registros que não pretendiam. Consulte minha entrada de blog, [AVISO: problema de simultaneidade com ASP.NET 2,0 GridViews/DetailsView/formviews que dão suporte à edição e/ou exclusão e cujo estado de exibição está desabilitado](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx), para obter mais informações.

Esse mesmo aviso também se aplica a DetailsViews e FormViews.

Para adicionar recursos de exclusão a um GridView, basta ir para sua marca inteligente e marcar a caixa de seleção Habilitar exclusão.

![Marque a caixa de seleção Habilitar exclusão](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image24.png)

**Figura 10**: marque a caixa de seleção Habilitar exclusão

Marcar a caixa de seleção Habilitar exclusão da marca inteligente adiciona um CommandField ao GridView. O CommandField renderiza uma coluna no GridView com botões para executar uma ou mais das seguintes tarefas: selecionando um registro, editando um registro e excluindo um registro. Vimos anteriormente o CommandField em ação com a seleção de registros no [mestre/detalhes usando um GridView mestre selecionável com um tutorial de detalhes detailview](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) .

O CommandField contém um número de `ShowXButton` propriedades que indicam qual série de botões são exibidos no comando. Marcando a caixa de seleção Habilitar exclusão, um CommandField cujo `ShowDeleteButton` propriedade é `true` foi adicionado à coleção de colunas do GridView.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample5.aspx)]

Neste ponto, acredite ou não, concluimos a adição de suporte à exclusão ao GridView! Como mostra a Figura 11, ao visitar esta página por meio de um navegador, uma coluna de botões de exclusão está presente.

[![o CommandField adiciona uma coluna de botões excluir](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image26.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image25.png)

**Figura 11**: o CommandField adiciona uma coluna de botões Delete ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image27.png))

Se você já está criando este tutorial do zero por conta própria, ao testar esta página, clicar no botão excluir gerará uma exceção. Continue lendo para saber por que essas exceções foram geradas e como corrigi-las.

> [!NOTE]
> Se você estiver seguindo usando o download que acompanha este tutorial, esses problemas já foram contados. No entanto, recomendo que você leia os detalhes listados abaixo para ajudar a identificar problemas que podem surgir e soluções alternativas adequadas.

Se, ao tentar excluir um produto, você receber uma exceção cuja mensagem é semelhante a "ObjectDataSource"*ObjectDataSource1 "não foi possível encontrar um método não genérico" DeleteProduct "que tenha parâmetros: ProductID, original\_ProductID*," você provavelmente esqueceu de remover a propriedade `OldValuesParameterFormatString` de ObjectDataSource. Com a propriedade `OldValuesParameterFormatString` especificada, o ObjectDataSource tenta passar os parâmetros de entrada `productID` e `original_ProductID` para o método `DeleteProduct`. o `DeleteProduct`, no entanto, só aceita um único parâmetro de entrada, portanto, a exceção. Remover a propriedade `OldValuesParameterFormatString` (ou defini-la como `{0}`) instrui o ObjectDataSource a não tentar passar o parâmetro de entrada original.

[![garantir que a propriedade OldValuesParameterFormatString foi apagada](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image29.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image28.png)

**Figura 12**: Verifique se a propriedade `OldValuesParameterFormatString` foi apagada ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image30.png))

Mesmo que você tenha removido a propriedade `OldValuesParameterFormatString`, você ainda receberá uma exceção ao tentar excluir um produto com a mensagem: "*a instrução DELETE entrou em conflito com a ordem de referência ' FK\_Order\_detalhes\_produtos '* ." O banco de dados Northwind contém uma restrição FOREIGN KEY entre a `Order Details` e a tabela `Products`, o que significa que um produto não poderá ser excluído do sistema se houver um ou mais registros para ele na tabela `Order Details`. Como cada produto no banco de dados Northwind tem pelo menos um registro no `Order Details`, não podemos excluir nenhum produto até que primeiro exclua os registros de detalhes do pedido associado do produto.

[![uma restrição FOREIGN KEY proíbe a exclusão de produtos](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image32.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image31.png)

**Figura 13**: uma restrição FOREIGN KEY proíbe a exclusão de produtos ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image33.png))

Para nosso tutorial, vamos apenas excluir todos os registros da tabela `Order Details`. Em um aplicativo do mundo real, precisaremos:

- Ter outra tela para gerenciar informações de detalhes do pedido
- Aumentar o método de `DeleteProduct` para incluir a lógica para excluir os detalhes do pedido do produto especificado
- Modificar a consulta SQL usada pelo TableAdapter para incluir a exclusão dos detalhes do pedido do produto especificado

Vamos apenas excluir todos os registros da tabela `Order Details` para burlar a restrição FOREIGN KEY. Vá para a Gerenciador de Servidores no Visual Studio, clique com o botão direito do mouse no nó `NORTHWND.MDF` e escolha nova consulta. Em seguida, na janela de consulta, execute a seguinte instrução SQL: `DELETE FROM [Order Details]`

[![excluir todos os registros da tabela detalhes do pedido](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image35.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image34.png)

**Figura 14**: excluir todos os registros da tabela de `Order Details` ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image36.png))

Depois de limpar a tabela de `Order Details`, clicar no botão excluir excluirá o produto sem erros. Se clicar no botão excluir não excluir o produto, verifique se a propriedade `DataKeyNames` do GridView está definida como o campo de chave primária (`ProductID`).

> [!NOTE]
> Ao clicar no botão excluir, um postback massacre e o registro são excluídos. Isso pode ser perigoso, pois é fácil clicar acidentalmente no botão excluir da linha errada. Em um futuro tutorial, veremos como adicionar uma confirmação do lado do cliente ao excluir um registro.

## <a name="editing-data-with-the-gridview"></a>Editando dados com o GridView

Juntamente com a exclusão, o controle GridView também fornece suporte interno à edição em nível de linha. Configurar um GridView para dar suporte à edição adiciona uma coluna de botões de edição. Da perspectiva do usuário final, clicar no botão Editar de uma linha faz com que essa linha se torne editável, transformando as células em caixas de Textque contêm os valores existentes e substituindo o botão Editar pelos botões atualizar e cancelar. Depois de fazer as alterações desejadas, o usuário final pode clicar no botão atualizar para confirmar as alterações ou no botão Cancelar para descartá-las. Em ambos os casos, depois de clicar em atualizar ou cancelar o GridView retorna para seu estado de pré-edição.

Da nossa perspectiva como o desenvolvedor da página, quando o usuário final clica no botão Editar para uma linha específica, um postback massacre e o GridView executa as seguintes etapas:

1. A propriedade `EditItemIndex` do GridView é atribuída ao índice da linha cujo botão de edição foi clicado
2. O GridView se reassocia ao ObjectDataSource invocando seu método de `Select()`
3. O índice de linha que corresponde ao `EditItemIndex` é renderizado no "modo de edição". Nesse modo, o botão Editar é substituído pelos botões Update e Cancel e BoundFields cujas propriedades `ReadOnly` são false (o padrão) são renderizadas como controles da Web TextBox cujas propriedades `Text` são atribuídas aos valores dos campos de dados.

Neste ponto, a marcação é retornada para o navegador, permitindo que o usuário final faça alterações nos dados da linha. Quando o usuário clica no botão atualizar, ocorre um postback e o GridView executa as seguintes etapas:

1. Os valores de `UpdateParameters` do ObjectDataSource são atribuídos aos valor inseridos pelo usuário final na interface de edição do GridView
2. O método de `Update()` do ObjectDataSource é invocado, atualizando o registro especificado
3. O GridView se reassocia ao ObjectDataSource invocando seu método de `Select()`

Os valores de chave primária atribuídos ao `UpdateParameters` na etapa 1 vêm dos valores especificados na propriedade `DataKeyNames`, enquanto os valores de chave não primária são provenientes do texto nos controles da Web da caixa de texto para a linha editada. Assim como acontece com a exclusão, é vital que a propriedade `DataKeyNames` de um GridView seja definida corretamente. Se ele estiver ausente, o valor da chave primária `UpdateParameters` será atribuído a um valor de `null` na etapa 1, que por sua vez não resultará em nenhum registro atualizado na etapa 2.

A funcionalidade de edição pode ser ativada simplesmente marcando a caixa de seleção Habilitar edição na marca inteligente do GridView.

![Marque a caixa de seleção Habilitar edição](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image37.png)

**Figura 15**: marcar a caixa de seleção Habilitar edição

Marcar a caixa de seleção Habilitar edição adicionará um CommandField (se necessário) e definirá sua propriedade `ShowEditButton` como `true`. Como vimos anteriormente, o CommandField contém um número de `ShowXButton` propriedades que indicam qual série de botões são exibidos no comando. Marcar a caixa de seleção Habilitar edição adiciona a propriedade `ShowEditButton` ao CommandField existente:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample6.aspx)]

Isso é tudo o que é para adicionar suporte à edição rudimentar. Como mostra o Figure16, a interface de edição é, em vez de crua, cada BoundField cuja propriedade `ReadOnly` está definida como `false` (o padrão) é renderizada como uma caixa de texto. Isso inclui campos como `CategoryID` e `SupplierID`, que são chaves para outras tabelas.

[![clicar no botão Chai s Edit exibe a linha no modo de edição](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image39.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image38.png)

**Figura 16**: clicar no botão Chai s Edit exibe a linha no modo de edição ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image40.png))

Além de solicitar aos usuários que editem valores de chave estrangeira diretamente, a interface da interface de edição não tem as seguintes maneiras:

- Se o usuário inserir um `CategoryID` ou `SupplierID` que não exista no banco de dados, o `UPDATE` violará uma restrição FOREIGN KEY, fazendo com que uma exceção seja gerada.
- A interface de edição não inclui nenhuma validação. Se você não fornecer um valor necessário (como `ProductName`) ou inserir um valor de cadeia de caracteres em que um valor numérico é esperado (como inserir "muito!" na caixa de texto `UnitPrice`), uma exceção será lançada. Um tutorial futuro examinará como adicionar controles de validação à interface do usuário de edição.
- Atualmente, *todos os* campos de produto que não são somente leitura devem ser incluídos no GridView. Se tivéssemos de remover um campo do GridView, digamos `UnitPrice`, ao atualizar os dados, o GridView não definiria o valor de `UpdateParameters` de `UnitPrice`, que alteraria o `UnitPrice` do registro do banco de dados para um valor de `NULL`. Da mesma forma, se um campo obrigatório, como `ProductName`, for removido do GridView, a atualização falhará com a mesma "*coluna ' ProductName ' não permite nulos*" exceção mencionada acima.
- A formatação da interface de edição deixa muito a desejar. O `UnitPrice` é mostrado com quatro pontos decimais. O ideal é que os valores `CategoryID` e `SupplierID` contenham DropDownLists que listem as categorias e os fornecedores no sistema.

Essas são todas as falhas com as quais teremos que viver por enquanto, mas serão abordadas em Tutoriais futuros.

## <a name="inserting-editing-and-deleting-data-with-the-detailsview"></a>Inserindo, editando e excluindo dados com o DetailsView

Como vimos nos tutoriais anteriores, o controle DetailsView exibe um registro de cada vez e, como o GridView, permite a edição e a exclusão do registro exibido no momento. A experiência do usuário final com edição e exclusão de itens de um DetailsView e o fluxo de trabalho do lado do ASP.NET é idêntica à do GridView. Onde o DetailsView difere do GridView é que ele também fornece suporte interno à inserção.

Para demonstrar os recursos de modificação de dados do GridView, comece adicionando um DetailsView à página de `Basics.aspx` acima do GridView existente e associe-o ao ObjectDataSource existente por meio da marca inteligente de DetailsView. Em seguida, desmarque as propriedades `Height` e `Width` de DetailsView e marque a opção habilitar paginação na marca inteligente. Para habilitar a edição, inserção e exclusão de suporte, basta marcar as caixas de seleção Habilitar edição, habilitar inserção e habilitar exclusão na marca inteligente.

![Configurar o DetailsView para dar suporte à edição, inserção e exclusão](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image41.png)

**Figura 17**: configurar o DetailsView para dar suporte à edição, inserção e exclusão

Assim como com o GridView, adicionar edição, inserção ou exclusão de suporte adiciona um CommandField ao DetailsView, como mostra a seguinte sintaxe declarativa:

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample7.aspx)]

Observe que para o DetailsView, o comando aparece no final da coleção de colunas por padrão. Como os campos de DetailsView são renderizados como linhas, o CommandField é exibido como uma linha com os botões inserir, editar e excluir na parte inferior do DetailsView.

[![configurar o DetailsView para dar suporte à edição, inserção e exclusão](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image43.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image42.png)

**Figura 18**: configurar o DetailsView para dar suporte à edição, inserção e exclusão ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image44.png))

Clicar no botão excluir inicia a mesma sequência de eventos que com o GridView: um postback; seguido pelo DetailsView que preenche sua `DeleteParameters` de ObjectDataSource com base nos valores de `DataKeyNames`; e concluído com uma chamada de seu método de `Delete()` de ObjectDataSource, que, na verdade, remove o produto do banco de dados. A edição no DetailsView também funciona de maneira idêntica à do GridView.

Para inserção, o usuário final recebe um novo botão que, quando clicado, renderiza o DetailsView no "modo de inserção". Com o "modo de inserção", o novo botão é substituído pelos botões inserir e cancelar e somente os BoundFields cuja propriedade `InsertVisible` está definida como `true` (o padrão) são exibidos. Esses campos de dados identificados como campos de incremento automático, como `ProductID`, têm sua [propriedade InsertVisible](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.insertvisible(VS.80).aspx) definida como `false` ao associar o DetailsView à fonte de dados por meio da marca inteligente.

Ao associar uma fonte de dados a um DetailsView por meio da marca inteligente, o Visual Studio define a propriedade `InsertVisible` como `false` somente para campos de incremento automático. Campos somente leitura, como `CategoryName` e `SupplierName`, serão exibidos na interface do usuário "modo de inserção", a menos que sua propriedade `InsertVisible` seja definida explicitamente como `false`. Reserve um momento para definir essas propriedades de `InsertVisible` de dois campos como `false`, por meio da sintaxe declarativa de DetailsView ou por meio do link editar campos na marca inteligente. A Figura 19 mostra a definição das propriedades de `InsertVisible` como `false` clicando no link editar campos.

[![Northwind Traders agora oferece a Acme chá](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image46.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image45.png)

**Figura 19**: a Northwind Traders agora oferece a Acme chá ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image47.png))

Depois de definir as propriedades de `InsertVisible`, exiba a página de `Basics.aspx` em um navegador e clique no botão novo. A figura 20 mostra o DetailsView ao adicionar uma nova bebida, Acme chá, à nossa linha de produtos.

[![Northwind Traders agora oferece a Acme chá](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image49.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image48.png)

**Figura 20**: a Northwind Traders agora oferece a Acme chá ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image50.png))

Depois de inserir os detalhes da Acme chá e clicar no botão Inserir, um postback massacre e o novo registro são adicionados à tabela `Products` banco de dados. Como este DetailsView lista os produtos na ordem em que eles existem na tabela de banco de dados, devemos paginar para o último produto a fim de ver o novo produto.

[![detalhes do chá de Acme](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image52.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image51.png)

**Figura 21**: detalhes do chá de Acme ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image53.png))

> [!NOTE]
> A [propriedade CurrentMode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.currentmode(VS.80).aspx) de DetailsView indica a interface que está sendo exibida e pode ser um dos seguintes valores: `Edit`, `Insert`ou `ReadOnly`. A [Propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode(VS.80).aspx) DefaultMode indica o modo em que o DetailsView retorna após uma edição ou inserção ter sido concluída e é útil para exibir um DetailsView que é permanentemente no modo Editar ou inserir.

Os recursos de inserção e edição de ponto e clique do DetailsView sofrem com as mesmas limitações que o GridView: o usuário deve inserir `CategoryID` existentes e valores de `SupplierID` por meio de uma caixa de texto; a interface não tem nenhuma lógica de validação; todos os campos de produto que não permitem `NULL` valores ou que não têm um valor padrão especificado no nível de banco de dados devem ser incluídos na interface de inserção e assim por diante.

As técnicas que examinaremos para estender e aprimorar a interface de edição de GridView em artigos futuros podem ser aplicadas às interfaces de edição e inserção do controle DetailsView também.

## <a name="using-the-formview-for-a-more-flexible-data-modification-user-interface"></a>Usando o FormView para uma interface de usuário de modificação de dados mais flexível

O FormView oferece suporte interno para inserção, edição e exclusão de dados, mas como ele usa modelos em vez de campos, não há nenhum lugar para adicionar o BoundFields ou o CommandField usado pelos controles GridView e DetailsView para fornecer os dados interface de modificação. Em vez disso, essa interface os controles da Web para coletar entrada do usuário ao adicionar um novo item ou editar um existente juntamente com os botões novo, editar, excluir, inserir, atualizar e cancelar devem ser adicionados manualmente aos modelos apropriados. Felizmente, o Visual Studio criará automaticamente a interface necessária ao ligar o FormView a uma fonte de dados por meio da lista suspensa em sua marca inteligente.

Para ilustrar essas técnicas, comece adicionando um FormView à página `Basics.aspx` e, na marca inteligente do FormView, associe-o ao ObjectDataSource já criado. Isso irá gerar um `EditItemTemplate`, `InsertItemTemplate`e `ItemTemplate` para o FormView com controles da Web TextBox para coletar os botões de entrada e de botão da Web do usuário para os novos, editar, excluir, inserir, atualizar e cancelar. Além disso, a propriedade `DataKeyNames` do FormView é definida como o campo de chave primária (`ProductID`) do objeto retornado pelo ObjectDataSource. Por fim, marque a opção habilitar paginação na marca inteligente do FormView.

O seguinte mostra a marcação declarativa para o `ItemTemplate` do FormView após o FormView ter sido associado a ObjectDataSource. Por padrão, cada campo de produto de valor não booliano é associado à propriedade `Text` de um controle de rótulo da Web, enquanto cada campo de valor booliano (`Discontinued`) é associado à propriedade `Checked` de um controle da Web de caixa de seleção desabilitado. Para que os botões novo, editar e excluir disparem determinado comportamento de FormView quando clicados, é imperativo que seus `CommandName` valores sejam definidos como `New`, `Edit`e `Delete`, respectivamente.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample8.aspx)]

A Figura 22 mostra a `ItemTemplate` do FormView quando exibida por meio de um navegador. Cada campo de produto é listado com os botões novo, editar e excluir na parte inferior.

[![ItemTemplate padrão FormView lista cada campo de produto juntamente com os botões novo, editar e excluir](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image55.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image54.png)

**Figura 22**: o `ItemTemplate` FormView padrão lista cada campo de produto juntamente com os botões novo, editar e excluir ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image56.png))

Assim como com GridView e DetailsView, clicar no botão Delete ou em qualquer Button, LinkButton ou ImageButton cuja propriedade `CommandName` está definida como delete causa um postback, popula o `DeleteParameters` do ObjectDataSource com base no valor de `DataKeyNames` do FormView e invoca o método `Delete()` do ObjectDataSource.

Quando o botão Editar é clicado em um postback massacre e os dados são reassociados ao `EditItemTemplate`, o que é responsável por renderizar a interface de edição. Essa interface inclui os controles da Web para editar dados juntamente com os botões atualizar e cancelar. O `EditItemTemplate` padrão gerado pelo Visual Studio contém um rótulo para todos os campos de incremento automático (`ProductID`), uma caixa de texto para cada campo de valor não booliano e uma caixa de seleção para cada campo de valor booliano. Esse comportamento é muito semelhante aos BoundFields gerados automaticamente nos controles GridView e DetailsView.

> [!NOTE]
> Um pequeno problema com a geração automática do FormView do `EditItemTemplate` é que ele processa controles da Web TextBox para os campos que são somente leitura, como `CategoryName` e `SupplierName`. Veremos como considerar isso em breve.

Os controles de caixa de texto no `EditItemTemplate` têm sua propriedade `Text` associada ao valor de seu campo de dados correspondente usando *DataBinding bidirecional*. A ligação de dados bidirecional, denotada por `<%# Bind("dataField") %>`, executa DataBinding ao associar dados ao modelo e ao preencher os parâmetros de ObjectDataSource para inserção ou edição de registros. Ou seja, quando o usuário clica no botão Editar na `ItemTemplate`, o método `Bind()` retorna o valor do campo de dados especificado. Depois que o usuário faz suas alterações e clica em atualizar, os valores postados que correspondem aos campos de dados especificados usando `Bind()` são aplicados à `UpdateParameters`de ObjectDataSource. Como alternativa, DataBinding unidirecional, indicada por `<%# Eval("dataField") %>`, recupera apenas os valores de campo de dados ao vincular dados ao modelo e *não* retorna os valores inseridos pelo usuário para os parâmetros da fonte de dados no postback.

A marcação declarativa a seguir mostra o `EditItemTemplate`do FormView. Observe que o método `Bind()` é usado na sintaxe DataBinding aqui e que os controles da Web do botão Update e Cancel têm suas propriedades `CommandName` definidas adequadamente.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample9.aspx)]

Nosso `EditItemTemplate`, neste ponto, fará com que uma exceção seja gerada se tentarmos usá-la. O problema é que os campos `CategoryName` e `SupplierName` são renderizados como controles da Web TextBox no `EditItemTemplate`. Precisamos alterar esses TextBoxes para rótulos ou removê-los completamente. Vamos simplesmente excluí-los totalmente da `EditItemTemplate`.

A Figura 23 mostra o FormView em um navegador depois que o botão de edição é clicado para Chai. Observe que os campos `SupplierName` e `CategoryName` mostrados no `ItemTemplate` não estão mais presentes, pois nós acabamos de removê-los do `EditItemTemplate`. Quando o botão de atualização é clicado, o FormView prossegue na mesma sequência de etapas que os controles GridView e DetailsView.

[![por padrão, o EditItemTemplate mostra cada campo de produto editável como uma caixa de texto ou caixa de seleção](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image58.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image57.png)

**Figura 23**: por padrão, a `EditItemTemplate` mostra cada campo de produto editável como uma caixa de texto ou caixa de seleção ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image59.png))

Quando o botão de inserção é clicado, o `ItemTemplate` do FormView é um massacre de postback. No entanto, nenhum dado é associado ao FormView porque um novo registro está sendo adicionado. A interface `InsertItemTemplate` inclui os controles da Web para adicionar um novo registro junto com os botões inserir e cancelar. O `InsertItemTemplate` padrão gerado pelo Visual Studio contém uma caixa de texto para cada campo de valor não booliano e uma caixa de seleção para cada campo de valor booliano, semelhante à interface de `EditItemTemplate`gerada automaticamente. Os controles TextBox têm sua propriedade `Text` associada ao valor de seu campo de dados correspondente usando DataBinding bidirecional.

A marcação declarativa a seguir mostra o `InsertItemTemplate`do FormView. Observe que o método `Bind()` é usado na sintaxe DataBinding aqui e que os controles da Web do botão Inserir e cancelar têm suas propriedades `CommandName` definidas adequadamente.

[!code-aspx[Main](an-overview-of-inserting-updating-and-deleting-data-cs/samples/sample10.aspx)]

Há uma sutileza com a geração automática do FormView do `InsertItemTemplate`. Especificamente, os controles da Web TextBox são criados mesmo para aqueles campos que são somente leitura, como `CategoryName` e `SupplierName`. Assim como com o `EditItemTemplate`, precisamos remover essas caixas de textdas `InsertItemTemplate`.

A figura 24 mostra o FormView em um navegador ao adicionar um novo produto, Acme Coffee. Observe que os campos `SupplierName` e `CategoryName` mostrados no `ItemTemplate` não estão mais presentes, pois nós acabamos de removê-los. Quando o botão de inserção é clicado, o FormView prossegue na mesma sequência de etapas que o controle DetailsView, adicionando um novo registro à tabela `Products`. A figura 25 mostra os detalhes do produto da Acme Coffee no FormView depois que ele é inserido.

[![o InsertItemTemplate determina a interface de inserção do FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image61.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image60.png)

**Figura 24**: a `InsertItemTemplate` determina a interface de inserção do FormView ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image62.png))

[![os detalhes do novo produto, Acme Coffee, são exibidos no FormView](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image64.png)](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image63.png)

**Figura 25**: os detalhes do novo produto, Acme Coffee, são exibidos no FormView ([clique para exibir a imagem em tamanho normal](an-overview-of-inserting-updating-and-deleting-data-cs/_static/image65.png))

Ao separar as interfaces somente leitura, editar e inserir em três modelos separados, o FormView permite um grau mais preciso de controle sobre essas interfaces do que o DetailsView e o GridView.

> [!NOTE]
> Como o DetailsView, a propriedade `CurrentMode` do FormView indica a interface que está sendo exibida e sua propriedade `DefaultMode` indica o modo que FormView retorna após uma edição ou inserção ser concluída.

## <a name="summary"></a>Resumo

Neste tutorial, examinamos as noções básicas de inserção, edição e exclusão de dados usando GridView, DetailsView e FormView. Todos esses três controles fornecem algum nível de recursos de modificação de dados internos que podem ser utilizados sem escrever uma única linha de código na página ASP.NET graças aos controles da Web de dados e ao ObjectDataSource. No entanto, as técnicas simples de ponto e clique renderizam uma interface do usuário de modificação de dados bastante Frail e ingênua. Para fornecer validação, injetar valores programáticos, tratar exceções normalmente, personalizar a interface do usuário e assim por diante, precisaremos contar com uma apresentação de técnicas que serão discutidas nos próximos vários tutoriais.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Próximo](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
