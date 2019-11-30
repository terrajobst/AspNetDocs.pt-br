---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
title: Limitando a funcionalidade de modificação de dados com base no usuário (VB) | Microsoft Docs
author: rick-anderson
description: Em um aplicativo Web que permite aos usuários editar dados, diferentes contas de usuário podem ter privilégios de edição de dados diferentes. Neste tutorial, examinaremos como t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 9dc264a6-feb8-474b-8b91-008c50708065
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb
msc.type: authoredcontent
ms.openlocfilehash: c257a930e4d27fcd42591a541e700786bf413bf0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74627186"
---
# <a name="limiting-data-modification-functionality-based-on-the-user-vb"></a>Limitar a funcionalidade de modificação de dados com base no usuário (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_VB.exe) ou [baixar PDF](limiting-data-modification-functionality-based-on-the-user-vb/_static/datatutorial23vb1.pdf)

> Em um aplicativo Web que permite aos usuários editar dados, diferentes contas de usuário podem ter privilégios de edição de dados diferentes. Neste tutorial, examinaremos como ajustar dinamicamente os recursos de modificação de dados com base no usuário visitante.

## <a name="introduction"></a>Introdução

Vários aplicativos Web dão suporte a contas de usuário e fornecem opções, relatórios e funcionalidades diferentes com base no usuário conectado. Por exemplo, com nossos tutoriais, podemos permitir que os usuários das empresas do fornecedor façam logon no site e atualizem informações gerais sobre seus produtos – seu nome e quantidade por unidade, talvez-junto com as informações do fornecedor, como o nome da empresa, Endereço, as informações da pessoa de contato e assim por diante. Além disso, talvez queiramos incluir algumas contas de usuário para pessoas da nossa empresa para que possam fazer logon e atualizar informações de produtos, como unidades em estoque, reordenar nível e assim por diante. Nosso aplicativo Web também pode permitir que usuários anônimos visitem (pessoas que não fizeram logon), mas os limitariam a apenas exibir dados. Com esse sistema de conta de usuário em vigor, queremos que os controles da Web de dados em nossas páginas ASP.NET ofereçam os recursos de inserção, edição e exclusão apropriados para o usuário conectado no momento.

Neste tutorial, examinaremos como ajustar dinamicamente os recursos de modificação de dados com base no usuário visitante. Em particular, criaremos uma página que exibe as informações de fornecedores em um DetailsView editável, juntamente com um GridView que lista os produtos fornecidos pelo fornecedor. Se o usuário que está visitando a página for de nossa empresa, ele poderá: exibir todas as informações do fornecedor. editar seu endereço; e edite as informações de qualquer produto fornecido pelo fornecedor. Se, no entanto, o usuário for de uma empresa específica, ele só poderá exibir e editar suas próprias informações de endereço e só poderá editar seus produtos que não foram marcados como descontinuados.

[![um usuário de nossa empresa pode editar qualquer informação de fornecedor](limiting-data-modification-functionality-based-on-the-user-vb/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image1.png)

**Figura 1**: um usuário de nossa empresa pode editar qualquer informação do fornecedor ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image3.png))

[![um usuário de um fornecedor específico só pode exibir e editar suas informações](limiting-data-modification-functionality-based-on-the-user-vb/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image4.png)

**Figura 2**: um usuário de um fornecedor específico só pode exibir e editar suas informações ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image6.png))

Vamos começar!

> [!NOTE]
> O sistema de associação do ASP.NET 2,0 fornece uma plataforma padronizada e extensível para criar, gerenciar e validar contas de usuário. Como um exame do sistema de associação está além do escopo desses tutoriais, este tutorial é, em vez disso, uma associação "falsificações" permitindo que os visitantes anônimos escolham se são de um fornecedor específico ou de nossa empresa. Para obter mais informações sobre associação, consulte minha [2,0 ASP.net, funções e](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) série de artigos de perfil.

## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Etapa 1: permitir que o usuário especifique seus direitos de acesso

Em um aplicativo Web do mundo real, as informações da conta de um usuário incluem se elas funcionaram para a nossa empresa ou para um fornecedor específico, e essas informações seriam acessadas programaticamente de nossas páginas ASP.NET quando o usuário tiver feito logon no site. Essas informações podem ser capturadas por meio do sistema de funções do ASP.NET 2,0 s, como informações de conta de nível de usuário por meio do sistema de perfil ou por meio de alguns meios personalizados.

Como o objetivo deste tutorial é demonstrar como ajustar os recursos de modificação de dados com base no usuário conectado e não se destina a apresentar associação, funções e sistemas de perfis do ASP.NET 2,0 s, usaremos um mecanismo muito simples para determinar o recursos para o usuário que visita a página-uma DropDownList da qual o usuário pode indicar se deve ser capaz de exibir e editar qualquer uma das informações de fornecedores ou, como alternativa, quais informações de fornecedores específicas eles podem exibir e editar. Se o usuário indicar que ela pode exibir e editar todas as informações do fornecedor (o padrão), ela poderá paginar todos os fornecedores, editar qualquer informação de endereço do fornecedor e editar o nome e a quantidade por unidade para qualquer produto fornecido pelo fornecedor selecionado. No entanto, se o usuário indicar que ela só pode exibir e editar um fornecedor específico, ela poderá exibir apenas os detalhes e os produtos para aquele fornecedor e só poderá atualizar o nome e a quantidade por informações de unidade para os produtos que *não* foram descontinuados.

Nossa primeira etapa neste tutorial, então, é criar essa DropDownList e preenchê-la com os fornecedores no sistema. Abra a página `UserLevelAccess.aspx` na pasta `EditInsertDelete`, adicione uma DropDownList cuja propriedade `ID` esteja definida como `Suppliers`e associe essa DropDownList a um novo ObjectDataSource chamado `AllSuppliersDataSource`.

[![criar um novo ObjectDataSource chamado AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-vb/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image7.png)

**Figura 3**: criar um novo ObjectDataSource chamado `AllSuppliersDataSource` ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image9.png))

Como queremos que essa DropDownList inclua todos os fornecedores, configure o ObjectDataSource para invocar o método de `GetSuppliers()` da classe `SuppliersBLL`. Além disso, verifique se o método ObjectDataSource s `Update()` está mapeado para o método `SuppliersBLL` Class s `UpdateSupplierAddress`, pois esse ObjectDataSource também será usado pelo DetailsView que adicionaremos na etapa 2.

Depois de concluir o assistente ObjectDataSource, conclua as etapas Configurando o `Suppliers` DropDownList de modo que ele mostre o campo de dados de `CompanyName` e use o campo de dados `SupplierID` como o valor de cada `ListItem`.

[![configurar os fornecedores DropDownList para usar os campos de dados CompanyName e CódigoDoFornecedor](limiting-data-modification-functionality-based-on-the-user-vb/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image10.png)

**Figura 4**: configurar o `Suppliers` DropDownList para usar os campos de dados `CompanyName` e `SupplierID` ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image12.png))

Neste ponto, a DropDownList lista os nomes de empresa dos fornecedores no banco de dados. No entanto, também precisamos incluir uma opção "mostrar/editar todos os fornecedores" na DropDownList. Para fazer isso, defina a propriedade `Suppliers` DropDownList s `AppendDataBoundItems` como `true` e, em seguida, adicione um `ListItem` cuja propriedade `Text` seja "mostrar/editar todos os fornecedores" e cujo valor seja `-1`. Isso pode ser adicionado diretamente por meio da marcação declarativa ou por meio do designer acessando a janela Propriedades e clicando nas reticências na Propriedade DropDownList s `Items`.

> [!NOTE]
> Consulte novamente a [*filtragem mestre/detalhes com um tutorial DropDownList*](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) para obter uma discussão mais detalhada sobre como adicionar um item selecionar tudo a uma DropDownList de ligação.

Depois que a propriedade `AppendDataBoundItems` tiver sido definida e a `ListItem` adicionada, a marcação declarativa de s da DropDownList deverá ser semelhante a:

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample1.aspx)]

A Figura 5 mostra uma captura de tela de nosso progresso atual, quando exibida por meio de um navegador.

[![a DropDownList de fornecedores contém uma Mostrar todas as ListItems, além de uma para cada fornecedor](limiting-data-modification-functionality-based-on-the-user-vb/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image13.png)

**Figura 5**: a `Suppliers` DropDownList contém uma mostrar todos os `ListItem`, além de um para cada fornecedor ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image15.png))

Como queremos atualizar a interface do usuário imediatamente após o usuário ter alterado sua seleção, defina a propriedade `Suppliers` DropDownList s `AutoPostBack` como `true`. Na etapa 2, criaremos um controle DetailsView que mostrará as informações do (s) fornecedor (es) com base na seleção de DropDownList. Em seguida, na etapa 3, criaremos um manipulador de eventos para o evento DropDownList s `SelectedIndexChanged`, no qual adicionaremos o código que associa as informações de fornecedor apropriadas ao DetailsView com base no fornecedor selecionado.

## <a name="step-2-adding-a-detailsview-control"></a>Etapa 2: adicionando um controle DetailsView

Deixe que o s use um DetailsView para mostrar informações do fornecedor. Para o usuário que pode exibir e editar todos os fornecedores, o DetailsView dará suporte à paginação, permitindo que o usuário percorra as informações do fornecedor um registro por vez. Se o usuário trabalha para um fornecedor específico, no entanto, o DetailsView mostrará apenas as informações específicas do fornecedor e não incluirá uma interface de paginação. Em ambos os casos, o DetailsView precisa permitir que o usuário edite os campos Endereço, cidade e país do fornecedor.

Adicione um DetailsView à página abaixo da `Suppliers` DropDownList, defina sua propriedade `ID` como `SupplierDetails`e associe-a ao `AllSuppliersDataSource` ObjectDataSource criado na etapa anterior. Em seguida, marque as caixas de seleção habilitar paginação e habilitar edição na marca inteligente DetailsView.

> [!NOTE]
> Se você não vir uma opção Habilitar edição na marca inteligente de DetailsView s, é porque você não Mapau o método ObjectDataSource s `Update()` para o método `SuppliersBLL` Class s `UpdateSupplierAddress`. Reserve um tempo para voltar e fazer essa alteração de configuração, após a qual a opção Habilitar edição deve aparecer na marca inteligente DetailsView.

Como o método `UpdateSupplierAddress` da classe `SuppliersBLL` só aceita quatro parâmetros-`supplierID`, `address`, `city`e `country`, modifique os BoundFields de DetailsView s para que os BoundFields `CompanyName` e `Phone` sejam somente leitura. Além disso, remova o `SupplierID` BoundField completamente. Por fim, o `AllSuppliersDataSource` ObjectDataSource atualmente tem sua propriedade `OldValuesParameterFormatString` definida como `original_{0}`. Reserve um tempo para remover essa configuração de propriedade da sintaxe declarativa totalmente ou defini-la como o valor padrão, `{0}`.

Depois de configurar o `SupplierDetails` DetailsView e `AllSuppliersDataSource` ObjectDataSource, teremos a seguinte marcação declarativa:

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample2.aspx)]

Neste ponto, o DetailsView pode ser paginado e as informações de endereço do fornecedor selecionado podem ser atualizadas, independentemente da seleção feita na `Suppliers` DropDownList (consulte a Figura 6).

[![qualquer informação de fornecedores pode ser exibida e seu endereço atualizado](limiting-data-modification-functionality-based-on-the-user-vb/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image16.png)

**Figura 6**: quaisquer informações de fornecedores podem ser exibidas e seu endereço atualizado ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image18.png))

## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Etapa 3: exibindo apenas as informações de fornecedor s selecionadas

Nossa página atualmente exibe as informações de todos os fornecedores, independentemente de um fornecedor específico ter sido selecionado na `Suppliers` DropDownList. Para exibir apenas as informações do fornecedor selecionado, precisamos adicionar outro ObjectDataSource à nossa página, que recupera informações sobre um fornecedor específico.

Adicione um novo ObjectDataSource à página, nomeando-o `SingleSupplierDataSource`. Em sua marca inteligente, clique no link configurar fonte de dados e use o método de `GetSupplierBySupplierID(supplierID)` de `SuppliersBLL` classe. Assim como com o `AllSuppliersDataSource` ObjectDataSource, tenha o método `SingleSupplierDataSource` ObjectDataSource s `Update()` mapeado para a classe `SuppliersBLL` s `UpdateSupplierAddress` método.

[![configurar o ObjectDataSource SingleSupplierDataSource para usar o método GetSupplierBySupplierID (CódigoDoFornecedor)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image19.png)

**Figura 7**: configurar o `SingleSupplierDataSource` ObjectDataSource para usar o método `GetSupplierBySupplierID(supplierID)` ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image21.png))

Em seguida, solicitamos que especifiquemos a origem do parâmetro para o `GetSupplierBySupplierID(supplierID)` método s `supplierID` parâmetro de entrada. Como queremos mostrar as informações para o fornecedor selecionado na DropDownList, use a propriedade `Suppliers` DropDownList s `SelectedValue` como a origem do parâmetro.

[![usar a DropDownList suppliers como a origem do parâmetro CódigoDoFornecedor](limiting-data-modification-functionality-based-on-the-user-vb/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image22.png)

**Figura 8**: usar a `Suppliers` DropDownList como a origem do parâmetro de `supplierID` ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image24.png))

Mesmo com esse segundo ObjectDataSource adicionado, o controle DetailsView está atualmente configurado para sempre usar o `AllSuppliersDataSource` ObjectDataSource. Precisamos adicionar lógica para ajustar a fonte de dados usada pelo DetailsView, dependendo do item de `Suppliers` DropDownList selecionado. Para fazer isso, crie um manipulador de eventos `SelectedIndexChanged` para a DropDownList suppliers. Isso pode ser criado com mais facilidade clicando-se duas vezes em DropDownList no designer. Esse manipulador de eventos precisa determinar qual fonte de dados usar e deve reassociar os dados ao DetailsView. Isso é feito com o seguinte código:

[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample3.vb)]

O manipulador de eventos começa determinando se a opção "mostrar/editar todos os fornecedores" foi selecionada. Se foi, ele define o `DataSourceID` `SupplierDetails` DetailsView s para `AllSuppliersDataSource` e retorna o usuário para o primeiro registro no conjunto de fornecedores, definindo a propriedade `PageIndex` como 0. No entanto, se o usuário tiver selecionado um fornecedor específico da DropDownList, a `DataSourceID` de DetailsView s será atribuída a `SingleSuppliersDataSource`. Independentemente de qual fonte de dados é usada, o modo de `SuppliersDetails` é revertido para o modo somente leitura e os dados são reassociados ao DetailsView por uma chamada para o método `SuppliersDetails` Control s `DataBind()`.

Com esse manipulador de eventos em vigor, o controle DetailsView agora mostra o fornecedor selecionado, a menos que a opção "mostrar/editar todos os fornecedores" tenha sido selecionada. nesse caso, todos os fornecedores podem ser exibidos por meio da interface de paginação. A Figura 9 mostra a página com a opção "mostrar/editar todos os fornecedores" selecionada; Observe que a interface de paginação está presente, permitindo que o usuário visite e atualize qualquer fornecedor. A Figura 10 mostra a página com o fornecedor Ma Maison selecionado. Somente as informações de Ma Maison s são visíveis e editáveis nesse caso.

[![todas as informações de fornecedores podem ser exibidas e editadas](limiting-data-modification-functionality-based-on-the-user-vb/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image25.png)

**Figura 9**: todas as informações de fornecedores podem ser exibidas e editadas ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image27.png))

[![apenas as informações de fornecedor s selecionadas podem ser exibidas e editadas](limiting-data-modification-functionality-based-on-the-user-vb/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image28.png)

**Figura 10**: somente as informações de fornecedor s selecionadas podem ser exibidas e editadas ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image30.png))

> [!NOTE]
> Para este tutorial, a `EnableViewState` de controle DropDownList e DetailsView deve ser definida como `true` (o padrão) porque as alterações de s `SelectedIndex` DropDownList e DetailsView s `DataSourceID` devem ser lembradas entre os postbacks.

## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Etapa 4: listando os produtos de fornecedores em um GridView editável

Com o DetailsView concluído, nossa próxima etapa é incluir um GridView editável que liste os produtos fornecidos pelo fornecedor selecionado. Esse GridView deve permitir edições somente nos campos `ProductName` e `QuantityPerUnit`. Além disso, se o usuário que visita a página for de um fornecedor específico, ele só deverá permitir atualizações nesses produtos que *não* foram descontinuados. Para fazer isso, precisaremos primeiro adicionar uma sobrecarga da classe `ProductsBLL` s `UpdateProducts` método que usa apenas os campos `ProductID`, `ProductName`e `QuantityPerUnit` como entradas. Nós nos reunimos por esse processo com vários tutoriais, então vamos examinar o código aqui, que deve ser adicionado ao `ProductsBLL`:

[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample4.vb)]

Com essa sobrecarga criada, estamos prontos para adicionar o controle GridView e seu ObjectDataSource associado. Adicione um novo GridView à página, defina sua propriedade `ID` como `ProductsBySupplier`e configure-a para usar um novo ObjectDataSource chamado `ProductsBySupplierDataSource`. Como queremos que esse GridView liste esses produtos pelo fornecedor selecionado, use o método de `GetProductsBySupplierID(supplierID)` de `ProductsBLL` classe. Além disso, mapeie o método `Update()` para a nova sobrecarga de `UpdateProduct` que acabamos de criar.

[![configurar o ObjectDataSource para usar a sobrecarga UpdateProduct recém-criada](limiting-data-modification-functionality-based-on-the-user-vb/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image31.png)

**Figura 11**: configurar o ObjectDataSource para usar a sobrecarga de `UpdateProduct` recém-criada ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image33.png))

Solicitamos a seleção da origem do parâmetro para o `GetProductsBySupplierID(supplierID)` método s `supplierID` parâmetro de entrada. Como queremos mostrar os produtos para o fornecedor selecionado no DetailsView, use a propriedade `SuppliersDetails` DetailsView Control s `SelectedValue` como a origem do parâmetro.

[![usar a propriedade SuppliersDetails DetailsView s SelectedValue como a origem do parâmetro](limiting-data-modification-functionality-based-on-the-user-vb/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image34.png)

**Figura 12**: Use a propriedade `SuppliersDetails` DetailsView s `SelectedValue` como a origem do parâmetro ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image36.png))

Retornando para o GridView, remova todos os campos GridView, exceto `ProductName`, `QuantityPerUnit`e `Discontinued`, marcando a `Discontinued` CheckBoxField como somente leitura. Além disso, marque a opção Habilitar edição na marca inteligente s GridView. Após essas alterações terem sido feitas, a marcação declarativa para GridView e ObjectDataSource deve ser semelhante ao seguinte:

[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample5.aspx)]

Assim como acontece com nossos objectdatasources anteriores, esse s `OldValuesParameterFormatString` propriedade é definido como `original_{0}`, o que causará problemas ao tentar atualizar um nome de produto ou uma quantidade por unidade. Remova essa propriedade da sintaxe declarativa completamente ou defina-a para o padrão, `{0}`.

Com essa configuração concluída, nossa página agora lista os produtos fornecidos pelo fornecedor selecionado em GridView (consulte a Figura 13). No momento, *qualquer* nome ou quantidade de produto por unidade pode ser atualizado. No entanto, precisamos atualizar nossa lógica de página para que essa funcionalidade seja proibida para produtos descontinuados para os usuários associados a um fornecedor específico. Abordaremos essa parte final na etapa 5.

[![os produtos fornecidos pelo fornecedor selecionado são exibidos](limiting-data-modification-functionality-based-on-the-user-vb/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image37.png)

**Figura 13**: os produtos fornecidos pelo fornecedor selecionado são exibidos ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image39.png))

> [!NOTE]
> Com a adição desse GridView editável, o manipulador de eventos `Suppliers` DropDownList s `SelectedIndexChanged` deve ser atualizado para retornar o GridView a um estado somente leitura. Caso contrário, se um fornecedor diferente for selecionado enquanto estiver no meio da edição de informações do produto, o índice correspondente no GridView do novo fornecedor também será editável. Para evitar isso, basta definir a Propriedade GridView `EditIndex` como `-1` no manipulador de eventos `SelectedIndexChanged`.

Além disso, lembre-se de que é importante que o estado de exibição de GridView s seja habilitado (o comportamento padrão). Se você definir a Propriedade GridView s `EnableViewState` como `false`, correrá o risco de ter usuários simultâneos excluindo ou editando os registros de forma involuntária. Consulte [AVISO: problema de simultaneidade com ASP.NET 2,0 GridViews/DetailsView/formviews que dão suporte à edição e/ou exclusão e cujo estado de exibição está desabilitado](http://scottonwriting.net/sowblog/posts/10054.aspx) para obter mais informações.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Etapa 5: não permitir a edição de produtos descontinuados quando mostrar/editar todos os fornecedores não estiver selecionado

Embora o `ProductsBySupplier` GridView seja totalmente funcional, ele atualmente concede muito acesso aos usuários que são de um fornecedor específico. De acordo com nossas regras de negócio, esses usuários não devem ser capazes de atualizar produtos descontinuados. Para impor isso, podemos ocultar (ou desabilitar) o botão Editar nessas linhas GridView com produtos descontinuados quando a página está sendo visitada por um usuário de um fornecedor.

Crie um manipulador de eventos para o evento GridView s `RowDataBound`. Nesse manipulador de eventos, precisamos determinar se o usuário está associado a um fornecedor específico, que, para este tutorial, pode ser determinado verificando a propriedade de `SelectedValue` do DropDownList dos fornecedores, se for algo diferente de-1, o usuário será associado a um fornecedor específico. Para esses usuários, precisamos determinar se o produto foi descontinuado ou não. Podemos obter uma referência à instância de `ProductRow` real associada à linha GridView por meio da propriedade `e.Row.DataItem`, conforme discutido na exibição de [*informações resumidas no tutorial de rodapé do GridView*](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-vb.md) . Se o produto for descontinuado, podemos obter uma referência programática para o botão Editar no comando GridView s, usando as técnicas discutidas no tutorial anterior, adicionando a [*confirmação do lado do cliente ao excluir*](adding-client-side-confirmation-when-deleting-vb.md). Assim que tivermos uma referência, poderemos ocultar ou desabilitar o botão.

[!code-vb[Main](limiting-data-modification-functionality-based-on-the-user-vb/samples/sample6.vb)]

Com esse manipulador de eventos em vigor, ao visitar esta página como um usuário de um fornecedor específico, os produtos descontinuados não são editáveis, pois o botão Editar está oculto para esses produtos. Por exemplo, o chefe Anton s Gumbo Mix é um produto descontinuado para o novo fornecedor do Orleans Cajun. Ao visitar a página deste fornecedor específico, o botão Editar deste produto estará oculto da visão (consulte a Figura 14). No entanto, ao visitar usando o "mostrar/editar todos os fornecedores", o botão Editar está disponível (veja a Figura 15).

[![para usuários específicos do fornecedor, o botão Editar para o chefe Anton s Gumbo Mix está oculto](limiting-data-modification-functionality-based-on-the-user-vb/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image40.png)

**Figura 14**: para usuários específicos do fornecedor, o botão Editar para o chefe Anton s Gumbo Mix está oculto ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image42.png))

[![para mostrar/editar todos os usuários de fornecedores, o botão Editar para o chefe Anton s Gumbo Mix é exibido](limiting-data-modification-functionality-based-on-the-user-vb/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-vb/_static/image43.png)

**Figura 15**: para mostrar/editar todos os usuários de fornecedores, o botão Editar para o chefe Anton s Gumbo Mix é exibido ([clique para exibir a imagem em tamanho normal](limiting-data-modification-functionality-based-on-the-user-vb/_static/image45.png))

## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Verificando direitos de acesso na camada de lógica de negócios

Neste tutorial, a página ASP.NET manipula toda a lógica com relação às informações que o usuário pode ver e quais produtos ele pode atualizar. O ideal é que essa lógica também esteja presente na camada de lógica de negócios. Por exemplo, o `SuppliersBLL` classe s `GetSuppliers()` método (que retorna todos os fornecedores) pode incluir uma verificação para garantir que o usuário conectado no momento *não* esteja associado a um fornecedor específico. Da mesma forma, o método `UpdateSupplierAddress` pode incluir uma verificação para garantir que o usuário conectado no momento seja trabalhado para nossa empresa (e, portanto, pode atualizar todas as informações de endereço de fornecedores) ou associado ao fornecedor cujos dados estão sendo atualizados.

Eu não incluí essas verificações de camada de BLL aqui porque, em nosso tutorial, os direitos do usuário são determinados por uma DropDownList na página, que as classes de BLL não podem acessar. Ao usar o sistema de associação ou um dos esquemas de autenticação prontos fornecidos pelo ASP.NET (como a autenticação do Windows), as informações de informações e funções do usuário conectado no momento podem ser acessadas da BLL, o que torna esse acesso verificações de direitos possíveis nas camadas de apresentação e BLL.

## <a name="summary"></a>Resumo

A maioria dos sites que fornecem contas de usuário precisam personalizar a interface de modificação de dados com base no usuário conectado. Os usuários administrativos podem ser capazes de excluir e editar qualquer registro, enquanto os usuários não administrativos podem estar limitados apenas a atualizar ou excluir os registros criados por eles mesmos. Qualquer que seja o cenário, os controles da Web de dados, ObjectDataSource e as classes de camada de lógica de negócios podem ser estendidos para adicionar ou negar determinadas funcionalidades com base no usuário conectado. Neste tutorial, vimos como limitar os dados exibíveis e editáveis dependendo se o usuário estava associado a um fornecedor específico ou se funcionou para nossa empresa.

Este tutorial conclui nosso exame de inserção, atualização e exclusão de dados usando os controles GridView, DetailsView e FormView. A partir do próximo tutorial, vamos voltar nossa atenção para adicionar suporte à paginação e classificação.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-client-side-confirmation-when-deleting-vb.md)
