---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
title: Uma visão geral da edição e exclusão de dados no DataList (VB) | Microsoft Docs
author: rick-anderson
description: Embora o DataList não tenha recursos internos de edição e exclusão, neste tutorial, veremos como criar um DataList com suporte para editar e excluir o...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 9410a23c-9697-4f07-bd71-e62b0ceac655
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-vb
msc.type: authoredcontent
ms.openlocfilehash: 49b09cbf0f12f7c7233c3bb2a8b3b2c073bf117e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78611268"
---
# <a name="an-overview-of-editing-and-deleting-data-in-the-datalist-vb"></a>Uma visão geral da edição e exclusão de dados no DataList (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_36_VB.exe) ou [baixar PDF](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/datatutorial36vb1.pdf)

> Embora o DataList não tenha recursos internos de edição e exclusão, neste tutorial, veremos como criar um DataList que dá suporte à edição e exclusão de seus dados subjacentes.

## <a name="introduction"></a>Introdução

Na [visão geral do tutorial inserir, atualizar e excluir dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , examinamos como inserir, atualizar e excluir dados usando a arquitetura do aplicativo, um ObjectDataSource e os controles GridView, DetailsView e FormView. Com o ObjectDataSource e esses três controles da Web de dados, a implementação de interfaces simples de modificação de dados foi um ajuste e envolveva apenas a marcação de uma caixa de seleção de uma marca inteligente. Nenhum código precisa ser gravado.

Infelizmente, o DataList não tem os recursos internos de edição e exclusão inerentes ao controle GridView. Essa funcionalidade ausente é devido, em parte, ao fato de que o DataList é um Relic da versão anterior do ASP.NET, quando os controles de fonte de dados declarativos e as páginas de modificação de dados sem código não estavam disponíveis. Embora o DataList no ASP.NET 2,0 não ofereça os mesmos recursos de modificação de dados prontos para uso que o GridView, podemos usar as técnicas do ASP.NET 1. x para incluir essa funcionalidade. Essa abordagem requer um pouco de código, mas como veremos neste tutorial, o DataList tem alguns eventos e propriedades em vigor para auxiliar nesse processo.

Neste tutorial, veremos como criar um DataList que dá suporte à edição e exclusão de seus dados subjacentes. Os tutoriais futuros examinarão os cenários de edição e exclusão mais avançados, incluindo a validação do campo de entrada, tratando normalmente as exceções geradas por camadas de acesso a dados ou lógica de negócios e assim por diante.

> [!NOTE]
> Como o DataList, o controle Repeater não tem a funcionalidade pronta para uso para inserção, atualização ou exclusão. Embora essa funcionalidade possa ser adicionada, o DataList inclui propriedades e eventos não encontrados no repetidor que simplificam a adição desses recursos. Portanto, este tutorial e os futuros que examinam a edição e a exclusão se concentrarão estritamente no DataList.

## <a name="step-1-creating-the-editing-and-deleting-tutorials-web-pages"></a>Etapa 1: criando as páginas da Web de tutoriais de edição e exclusão

Antes de começarmos a explorar como atualizar e excluir dados de um DataList, vamos primeiro reservar um momento para criar as páginas do ASP.NET em nosso projeto de site, que precisaremos para este tutorial e as próximas várias delas. Comece adicionando uma nova pasta chamada `EditDeleteDataList`. Em seguida, adicione as seguintes páginas ASP.NET a essa pasta, assegurando a associação de cada página com a página mestra `Site.master`:

- `Default.aspx`
- `Basics.aspx`
- `BatchUpdate.aspx`
- `ErrorHandling.aspx`
- `UIValidation.aspx`
- `CustomizedUI.aspx`
- `OptimisticConcurrency.aspx`
- `ConfirmationOnDelete.aspx`
- `UserLevelAccess.aspx`

![Adicionar as páginas ASP.NET para os tutoriais](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image1.png)

**Figura 1**: adicionar as páginas ASP.net para os tutoriais

Assim como nas outras pastas, `Default.aspx` na pasta `EditDeleteDataList` lista os tutoriais em sua seção. Lembre-se de que o controle de usuário `SectionLevelTutorialListing.ascx` fornece essa funcionalidade. Portanto, adicione esse controle de usuário a `Default.aspx` arrastando-o da Gerenciador de Soluções para a página s modo de exibição de Design.

[![adicionar o controle de usuário SectionLevelTutorialListing. ascx a default. aspx](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image3.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image2.png)

**Figura 2**: Adicionar o controle de usuário `SectionLevelTutorialListing.ascx` ao `Default.aspx` ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image4.png))

Por fim, adicione as páginas como entradas ao arquivo de `Web.sitemap`. Especificamente, adicione a seguinte marcação após os relatórios mestre/de detalhes com o DataList e o repetidor `<siteMapNode>`:

[!code-xml[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample1.xml)]

Depois de atualizar `Web.sitemap`, Reserve um momento para exibir o site de tutoriais por meio de um navegador. O menu à esquerda agora inclui itens para os tutoriais de edição e exclusão de DataList.

![O mapa do site agora inclui entradas para os tutoriais de edição e exclusão de DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image5.png)

**Figura 3**: o mapa do site agora inclui entradas para os tutoriais de edição e exclusão de DataList

## <a name="step-2-examining-techniques-for-updating-and-deleting-data"></a>Etapa 2: examinando técnicas para atualizar e excluir dados

Editar e excluir dados com o GridView é tão fácil porque, sob os bastidores, o GridView e o ObjectDataSource funcionam em conjunto. Conforme discutido no tutorial [examinando os eventos associados com inserção, atualização e exclusão](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) , quando um botão de atualização de linha s é clicado, o GridView atribui automaticamente seus campos que usaram a ligação de dados bidirecional à coleção de `UpdateParameters` de seu ObjectDataSource e, em seguida, invoca esse método objectdatasource s `Update()`.

Infelizmente, o DataList não fornece nenhuma dessas funcionalidades internas. É nossa responsabilidade garantir que os valores de s do usuário sejam atribuídos aos parâmetros de ObjectDataSource s e que seu método `Update()` seja chamado. Para nos ajudar nessa tarefa, o DataList fornece as seguintes propriedades e eventos:

- **A [Propriedade`DataKeyField`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeyfield.aspx)**  ao atualizar ou excluir, precisamos ser capaz de identificar exclusivamente cada item no DataList. Defina essa propriedade como o campo de chave primária dos dados exibidos. Isso irá preencher a coleção DataList s [`DataKeys`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basedatalist.datakeys.aspx) com o valor de `DataKeyField` especificado para cada item DataList.
- **O [evento`EditCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.editcommand.aspx)**  é acionado quando um Button, LinkButton ou ImageButton cuja propriedade `CommandName` é definida como Edit é clicado.
- **O [evento`CancelCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.cancelcommand.aspx)**  é acionado quando um Button, LinkButton ou ImageButton cuja propriedade `CommandName` é definida como Cancel é clicado.
- **O [evento`UpdateCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.updatecommand.aspx)**  é acionado quando um Button, LinkButton ou ImageButton cuja propriedade `CommandName` está definida como Update é clicada.
- **O [evento`DeleteCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.deletecommand.aspx)**  é acionado quando um Button, LinkButton ou ImageButton cuja propriedade `CommandName` está definida como Delete é clicado.

Usando essas propriedades e eventos, há quatro abordagens que podemos usar para atualizar e excluir dados do DataList:

1. **Usando as técnicas ASP.NET 1. x** , o DataList existia antes do ASP.NET 2,0 e do ObjectDataSource e foi capaz de atualizar e excluir dados inteiramente por meio de meios programáticos. Essa técnica Ditches a ObjectDataSource e requer que associemos os dados ao DataList diretamente da camada de lógica de negócios, na recuperação dos dados a serem exibidos e ao atualizar ou excluir um registro.
2. **Usando um único controle ObjectDataSource na página para seleção, atualização e exclusão** enquanto o DataList não tem os recursos de edição e exclusão inerentes ao GridView, não há motivo para não adicioná-los por si mesmos. Com essa abordagem, usamos um ObjectDataSource exatamente como nos exemplos de GridView, mas devemos criar um manipulador de eventos para o evento DataList s `UpdateCommand` em que definimos os parâmetros de ObjectDataSource s e chamamos seu método `Update()`.
3. **Usando um controle ObjectDataSource para selecionar, mas atualizar e excluir diretamente em relação à BLL** ao usar a opção 2, precisamos escrever um pouco de código no evento `UpdateCommand`, atribuindo valores de parâmetro e assim por diante. Em vez disso, podemos usar o ObjectDataSource para selecionar, mas fazer as chamadas de atualização e exclusão diretamente em relação à BLL (como com a opção 1). Na minha opinião, a atualização de dados por meio da interface direta com a BLL leva a um código mais legível do que a atribuição do `UpdateParameters` ObjectDataSource s e a chamada de seu método `Update()`.
4. **Usando significados declarativos por meio de várias ObjectDataSource** , as três abordagens anteriores exigem um pouco de código. Se você d preferir usar o máximo de sintaxe declarativa possível, uma opção final será incluir vários objectdatasources na página. O primeiro ObjectDataSource recupera os dados da BLL e os associa ao DataList. Para atualização, outro ObjectDataSource é adicionado, mas adicionado diretamente dentro do `EditItemTemplate`de DataList s. Para incluir o suporte à exclusão, ainda é necessário outro ObjectDataSource no `ItemTemplate`. Com essa abordagem, essas s ObjectDataSource inseridas usam `ControlParameters` para associar declarativamente os parâmetros de ObjectDataSource s aos controles de entrada do usuário (em vez de ter que especificá-los programaticamente no manipulador de eventos do s `UpdateCommand`). Essa abordagem ainda requer um pouco de código que precisamos para chamar o comando ObjectDataSource s `Update()` ou `Delete()`, mas que exige muito menos do que com as outras três abordagens. A desvantagem aqui é que as várias objectdatasources desorganizam a página, prejudicando a legibilidade geral.

Se forçada a usar apenas uma dessas abordagens, eu escolho a opção 1 porque ela fornece mais flexibilidade e porque o DataList foi originalmente projetado para acomodar esse padrão. Embora o DataList tenha sido estendido para funcionar com os controles de fonte de dados ASP.NET 2,0, ele não tem todos os pontos de extensibilidade ou recursos dos controles da Web de dados oficiais do ASP.NET 2,0 (GridView, DetailsView e FormView). No entanto, as opções 2 a 4 não estão sem mérito.

Este e os tutoriais de edição e exclusão futuros usarão um ObjectDataSource para recuperar os dados para exibir e direcionar chamadas para a BLL para atualizar e excluir dados (opção 3).

## <a name="step-3-adding-the-datalist-and-configuring-its-objectdatasource"></a>Etapa 3: adicionando o DataList e configurando seu ObjectDataSource

Neste tutorial, criaremos um DataList que lista informações sobre produtos e, para cada produto, fornece ao usuário a capacidade de editar o nome e o preço e excluir o produto completamente. Em particular, recuperaremos os registros a serem exibidos usando um ObjectDataSource, mas executaremos as ações Update e Delete fazendo a interface direta com a BLL. Antes de nos preocuparmos em implementar os recursos de edição e exclusão para o DataList, vamos primeiro obter a página para exibir os produtos em uma interface somente leitura. Como examinamos essas etapas nos tutoriais anteriores, passarei por elas rapidamente.

Comece abrindo a página de `Basics.aspx` na pasta `EditDeleteDataList` e, na modo de exibição de Design, adicione uma DataList à página. Em seguida, na marca inteligente DataList s, crie um novo ObjectDataSource. Como estamos trabalhando com os dados do produto, configure-o para usar a classe `ProductsBLL`. Para recuperar *todos os* produtos, escolha o método `GetProducts()` na guia selecionar.

[![configurar o ObjectDataSource para usar a classe ProductsBLL](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image7.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image6.png)

**Figura 4**: configurar o ObjectDataSource para usar a classe `ProductsBLL` ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image8.png))

[![retornar as informações do produto usando o método GetProducts ()](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image10.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image9.png)

**Figura 5**: retornar as informações do produto usando o método `GetProducts()` ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image11.png))

O DataList, como o GridView, não foi projetado para inserir novos dados; Portanto, selecione a opção (nenhum) na lista suspensa na guia Inserir. Além disso, escolha (nenhum) para as guias atualizar e excluir, pois as atualizações e exclusões serão executadas programaticamente por meio da BLL.

[![confirmar se as listas suspensas nas guias ObjectDataSource s INSERT, UPDATE e DELETE são definidas como (None)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image13.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image12.png)

**Figura 6**: Confirme se as listas suspensas nas guias OBJECTDATASOURCE s INSERT, Update e Delete estão definidas como (None) ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image14.png))

Depois de configurar o ObjectDataSource, clique em concluir, retornando ao designer. Como vimos nos exemplos anteriores, ao concluir a configuração de ObjectDataSource, o Visual Studio cria automaticamente um `ItemTemplate` para DropDownList, exibindo cada um dos campos de dados. Substitua essa `ItemTemplate` por uma que exibe apenas o nome e o preço do produto. Além disso, defina a propriedade `RepeatColumns` como 2.

> [!NOTE]
> Conforme discutido na *visão geral do tutorial inserir, atualizar e excluir dados* , ao modificar dados usando o ObjectDataSource, nossa arquitetura exige que removamos a propriedade `OldValuesParameterFormatString` da marcação declarativa s de ObjectDataSource (ou redefina-a para seu valor padrão, `{0}`). Neste tutorial, no entanto, estamos usando o ObjectDataSource somente para recuperar dados. Portanto, não precisamos modificar o valor da Propriedade ObjectDataSource s `OldValuesParameterFormatString` (embora não atrapalhe isso).

Depois de substituir o `ItemTemplate` DataList padrão por um personalizado, a marcação declarativa em sua página deve ser semelhante ao seguinte:

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample2.aspx)]

Reserve um tempo para exibir nosso progresso por meio de um navegador. Como mostra a Figura 7, o DataList exibe o nome do produto e o preço unitário de cada produto em duas colunas.

[![os nomes e os preços dos produtos são exibidos em um DataList de duas colunas](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image16.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image15.png)

**Figura 7**: os nomes e os preços dos produtos são exibidos em um DataList de duas colunas ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image17.png))

> [!NOTE]
> O DataList tem um número de propriedades que são necessárias para o processo de atualização e exclusão, e esses valores são armazenados em estado de exibição. Portanto, ao criar um DataList que ofereça suporte à edição ou exclusão de dados, é essencial que o estado de exibição DataList s esteja habilitado.  
>   
> O leitor do astuto pode se lembrar de que pudemos desabilitar o estado de exibição ao criar GridViews editáveis, DetailsViews e FormViews. Isso ocorre porque os controles da Web do ASP.NET 2,0 podem incluir o *estado do controle*, que é persistido em postagens como o estado de exibição, mas considerado essencial.

Desabilitar o estado de exibição no GridView meramente omite informações de estado trivial, mas mantém o estado de controle (que inclui o estado necessário para edição e exclusão). O DataList, que tinha sido criado no período de tempo ASP.NET 1. x, não utiliza o estado de controle e, portanto, deve ter o estado de exibição habilitado. Consulte estado de [controle versus estado de exibição](https://msdn.microsoft.com/library/1whwt1k7.aspx) para obter mais informações sobre a finalidade do estado de controle e como ele difere do estado de exibição.

## <a name="step-4-adding-an-editing-user-interface"></a>Etapa 4: adicionando uma interface do usuário de edição

O controle GridView é composto de uma coleção de campos (BoundFields, CheckBoxFields, TemplateFields e assim por diante). Esses campos podem ajustar sua marcação renderizada dependendo do seu modo. Por exemplo, quando no modo somente leitura, um BoundField exibe seu valor de campo de dados como texto; no modo de edição, ele renderiza um controle da Web TextBox cuja propriedade `Text` é atribuído ao valor do campo de dados.

O DataList, por outro lado, renderiza seus itens usando modelos. Os itens somente leitura são renderizados usando o `ItemTemplate`, enquanto os itens no modo de edição são renderizados por meio do `EditItemTemplate`. Neste ponto, nosso DataList tem apenas um `ItemTemplate`. Para dar suporte à funcionalidade de edição no nível de item, precisamos adicionar um `EditItemTemplate` que contém a marcação a ser exibida para o item editável. Para este tutorial, usaremos os controles da Web TextBox para editar o nome e o preço unitário do produto.

O `EditItemTemplate` pode ser criado de forma declarativa ou por meio do designer (selecionando a opção Editar modelos na marca inteligente DataList s). Para usar a opção Editar modelos, primeiro clique no link editar modelos na marca inteligente e, em seguida, selecione o `EditItemTemplate` item na lista suspensa.

[![optar por trabalhar com o controle DataList s EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image19.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image18.png)

**Figura 8**: optar por trabalhar com o `EditItemTemplate` s de DataList ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image20.png))

Em seguida, digite o nome do produto: e Price: e arraste dois controles TextBox da caixa de ferramentas para a interface `EditItemTemplate` no designer. Defina as caixas de entrada `ID` propriedades como `ProductName` e `UnitPrice`.

[![adicionar uma caixa de texto para o nome e o preço do produto](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image22.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image21.png)

**Figura 9**: adicionar uma caixa de texto para o nome e o preço do produto ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image23.png))

Precisamos associar os valores do campo de dados do produto correspondente às propriedades `Text` das duas caixas de entrada. Nas marcas inteligentes de TextBoxes, clique no link editar DataBindings e associe o campo de dados apropriado com a propriedade `Text`, conforme mostrado na Figura 10.

> [!NOTE]
> Ao vincular o campo de dados de `UnitPrice` ao campo `Text` da caixa de texto de preços, você pode formatá-lo como um valor de moeda (`{0:C}`), um número geral (`{0:N}`) ou deixá-lo não formatado.

![Associar os campos de dados NomeDoProduto e UnitPrice às propriedades Text das caixas de texto](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image24.png)

**Figura 10**: associar os campos de dados `ProductName` e `UnitPrice` às propriedades `Text` das caixas de entrada

Observe como a caixa de diálogo Editar DataBindings na Figura 10 *não inclui a* CheckBox de ligação de dados bidirecional que está presente ao editar um TemplateField em GridView ou DetailsView, ou um modelo no FormView. O recurso de ligação de dados bidirecional permitia que o valor inserido no controle da Web de entrada fosse atribuído automaticamente à `InsertParameters` ObjectDataSource s correspondente ou `UpdateParameters` ao inserir ou atualizar dados. O DataList não dá suporte à ligação de dados bidirecional, como veremos mais adiante neste tutorial, depois que o usuário fizer suas alterações e estiver pronto para atualizar os dados, precisaremos acessar esses TextBoxes de forma programática `Text` Propriedades e passar seus valores para o método `UpdateProduct` apropriado na classe `ProductsBLL`.

Por fim, precisamos adicionar os botões Update e Cancel ao `EditItemTemplate`. Como vimos no [mestre/detalhes usando uma lista com marcadores de registros mestres com um tutorial DataList detalhes](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) , quando um Button, LinkButton ou ImageButton cuja propriedade `CommandName` é definida é clicado em um Repeater ou DataList, o evento Repeater ou datalist s `ItemCommand` é gerado. Para o DataList, se a propriedade `CommandName` for definida como um determinado valor, um evento adicional também poderá ser gerado. Os valores de propriedade de `CommandName` especiais incluem, entre outros:

- Cancelar gera o evento `CancelCommand`
- Editar gera o evento `EditCommand`
- A atualização gera o evento `UpdateCommand`

Tenha em mente que esses eventos *são gerados além* do evento `ItemCommand`.

Adicione ao `EditItemTemplate` dois botões de controles da Web, um cujo `CommandName` está definido como atualizar e os outros s definidos como cancelar. Depois de adicionar esses dois controles de botão da Web, o designer deve ser semelhante ao seguinte:

[![adicionar os botões atualizar e cancelar ao EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image26.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image25.png)

**Figura 11**: adicionar os botões atualizar e cancelar à `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image27.png))

Com o `EditItemTemplate` concluir, a marcação declarativa s de DataList deve ser semelhante ao seguinte:

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample3.aspx)]

## <a name="step-5-adding-the-plumbing-to-enter-edit-mode"></a>Etapa 5: Adicionar o encanamento para entrar no modo de edição

Neste ponto, nosso DataList tem uma interface de edição definida por meio de sua `EditItemTemplate`; no entanto, atualmente, não há nenhuma maneira de um usuário visitar nossa página para indicar que ele deseja editar as informações de um produto. Precisamos adicionar um botão de edição a cada produto que, quando clicado, renderiza esse item DataList no modo de edição. Comece adicionando um botão de edição à `ItemTemplate`, seja por meio do designer ou declarativamente. Certifique-se de definir a propriedade s `CommandName` do botão Editar para editar.

Depois de adicionar esse botão de edição, Reserve um momento para exibir a página por meio de um navegador. Com essa adição, cada listagem de produto deve incluir um botão de edição.

[![adicionar os botões atualizar e cancelar ao EditItemTemplate](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image29.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image28.png)

**Figura 12**: adicionar os botões atualizar e cancelar à `EditItemTemplate` ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image30.png))

Clicar no botão causa um postback, mas *não* coloca a listagem do produto no modo de edição. Para tornar o produto editável, precisamos:

1. Defina a propriedade DataList s [`EditItemIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) como o índice do `DataListItem` cujo botão de edição foi clicado apenas.
2. Reassocie os dados ao DataList. Quando o DataList é renderizado novamente, o `DataListItem` cujas `ItemIndex` corresponde à `EditItemIndex` DataList s será renderizada usando seu `EditItemTemplate`.

Como o evento DataList s `EditCommand` é acionado quando o botão de edição é clicado, crie um manipulador de eventos `EditCommand` com o seguinte código:

[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample4.vb)]

O manipulador de eventos `EditCommand` é passado em um objeto do tipo `DataListCommandEventArgs` como seu segundo parâmetro de entrada, que inclui uma referência à `DataListItem` com o qual o botão de edição foi clicado (`e.Item`). O manipulador de eventos primeiro define os `EditItemIndex` DataList para a `ItemIndex` do `DataListItem` editável e, em seguida, reassocia os dados ao DataList chamando o método DataList s `DataBind()`.

Depois de adicionar esse manipulador de eventos, reveja a página em um navegador. Clicar no botão Editar agora torna editável o produto clicado (consulte a Figura 13).

[![clicar no botão Editar torna o produto editável](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image32.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image31.png)

**Figura 13**: clicar no botão Editar torna o produto editável ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image33.png))

## <a name="step-6-saving-the-user-s-changes"></a>Etapa 6: salvar as alterações de s do usuário

Clicar nos botões atualizar ou cancelar do produto editado não faz nada neste ponto; para adicionar essa funcionalidade, precisamos criar manipuladores de eventos para os eventos DataList s `UpdateCommand` e `CancelCommand`. Comece criando o manipulador de eventos `CancelCommand`, que será executado quando o botão Cancelar do produto editado for clicado e ele tiver a tarefa de retornar o DataList para seu estado de pré-edição.

Para que o DataList processe todos os seus itens no modo somente leitura, precisamos:

1. Defina a propriedade DataList s [`EditItemIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) como o índice de um índice `DataListItem` não existente. `-1` é uma opção segura, já que os índices de `DataListItem` começam em `0`.
2. Reassocie os dados ao DataList. Como nenhum `DataListItem` `ItemIndex` es corresponde à `EditItemIndex`DataList s, o DataList inteiro será renderizado em um modo somente leitura.

Essas etapas podem ser realizadas com o seguinte código de manipulador de eventos:

[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample5.vb)]

Com essa adição, clicar no botão Cancelar retorna o DataList ao seu estado de pré-edição.

O último manipulador de eventos que precisamos concluir é o manipulador de eventos `UpdateCommand`. Este manipulador de eventos precisa:

1. Acesse programaticamente o nome do produto e o preço digitados pelo usuário, bem como os s `ProductID`do produto editado.
2. Inicie o processo de atualização chamando a sobrecarga de `UpdateProduct` apropriada na classe `ProductsBLL`.
3. Defina a propriedade DataList s [`EditItemIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.edititemindex.aspx) como o índice de um índice `DataListItem` não existente. `-1` é uma opção segura, já que os índices de `DataListItem` começam em `0`.
4. Reassocie os dados ao DataList. Como nenhum `DataListItem` `ItemIndex` es corresponde à `EditItemIndex`DataList s, o DataList inteiro será renderizado em um modo somente leitura.

As etapas 1 e 2 são responsáveis por salvar as alterações de s do usuário; as etapas 3 e 4 retornam o DataList para seu estado de pré-edição depois que as alterações foram salvas e são idênticas às etapas executadas no manipulador de eventos `CancelCommand`.

Para obter o nome e o preço do produto atualizado, precisamos usar o método `FindControl` para fazer referência programaticamente aos controles da Web TextBox dentro do `EditItemTemplate`. Também precisamos obter o valor de `ProductID` do produto editado. Quando vinculamos inicialmente o ObjectDataSource ao DataList, o Visual Studio atribuiu a propriedade DataList s `DataKeyField` ao valor de chave primária da fonte de dados (`ProductID`). Esse valor pode então ser recuperado da coleção DataList s `DataKeys`. Reserve um tempo para garantir que a propriedade `DataKeyField` realmente esteja definida como `ProductID`.

O código a seguir implementa as quatro etapas:

[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample6.vb)]

O manipulador de eventos começa lendo no `ProductID` do produto editado na coleção de `DataKeys`. Em seguida, as duas caixas de textna `EditItemTemplate` são referenciadas e suas propriedades de `Text` armazenadas em variáveis locais, `productNameValue` e `unitPriceValue`. Usamos o método `Decimal.Parse()` para ler o valor da caixa de texto `UnitPrice` de forma que, se o valor inserido tiver um símbolo de moeda, ele ainda poderá ser convertido corretamente em um valor de `Decimal`.

> [!NOTE]
> Os valores das caixas de texto `ProductName` e `UnitPrice` são atribuídos somente às variáveis productNamevalue e unitPricevalue, se as propriedades Text TextBoxes tiverem um valor especificado. Caso contrário, um valor de `Nothing` é usado para as variáveis, que tem o efeito de atualizar os dados com um valor de banco de `NULL`. Ou seja, nosso código trata a conversão de cadeias de caracteres vazias em valores de `NULL` de banco de dados, que é o comportamento padrão da interface de edição nos controles GridView, DetailsView e FormView.

Depois de ler os valores, a classe `ProductsBLL` s `UpdateProduct` método é chamado, passando o nome, o preço e o `ProductID`do produto. O manipulador de eventos é concluído retornando o DataList para seu estado de pré-edição usando exatamente a mesma lógica que o manipulador de eventos `CancelCommand`.

Com os manipuladores de eventos `EditCommand`, `CancelCommand`e `UpdateCommand` concluídos, um visitante pode editar o nome e o preço de um produto. As figuras 14-16 mostram esse fluxo de trabalho de edição em ação.

[![ao visitar a página pela primeira vez, todos os produtos estão no modo somente leitura](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image35.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image34.png)

**Figura 14**: ao visitar a página pela primeira vez, todos os produtos estão no modo somente leitura ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image36.png))

[![para atualizar o nome ou o preço de um produto, clique no botão Editar](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image38.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image37.png)

**Figura 15**: para atualizar o nome ou o preço de um produto, clique no botão Editar ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image39.png))

[![depois de alterar o valor, clique em atualizar para retornar ao modo somente leitura](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image41.png)](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image40.png)

**Figura 16**: depois de alterar o valor, clique em atualizar para retornar ao modo somente leitura ([clique para exibir a imagem em tamanho normal](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/_static/image42.png))

## <a name="step-7-adding-delete-capabilities"></a>Etapa 7: adicionando recursos de exclusão

As etapas para adicionar recursos de exclusão a um DataList são semelhantes àquelas para adicionar recursos de edição. Em suma, precisamos adicionar um botão Delete ao `ItemTemplate` que, quando clicado:

1. Lê no `ProductID` do produto correspondente por meio da coleção de `DataKeys`.
2. Executa a exclusão chamando o método de `DeleteProduct` da classe `ProductsBLL`.
3. Reassocia os dados ao DataList.

Deixe que o s comece adicionando um botão de exclusão à `ItemTemplate`.

Quando clicado, um botão cujo `CommandName` é editar, atualizar ou cancelar gera o evento DataList s `ItemCommand` junto com um evento adicional (por exemplo, ao usar editar o evento `EditCommand` também é gerado). Da mesma forma, qualquer Button, LinkButton ou ImageButton no DataList cuja propriedade `CommandName` está definida como Delete faz com que o evento `DeleteCommand` seja acionado (juntamente com `ItemCommand`).

Adicione um botão excluir ao lado do botão Editar na `ItemTemplate`, definindo sua propriedade `CommandName` como excluir. Depois de adicionar esse botão, o controle da sintaxe declarativa s `ItemTemplate` de DataList deve ser semelhante a:

[!code-aspx[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample7.aspx)]

Em seguida, crie um manipulador de eventos para o evento DataList s `DeleteCommand`, usando o seguinte código:

[!code-vb[Main](an-overview-of-editing-and-deleting-data-in-the-datalist-vb/samples/sample8.vb)]

Clicar no botão excluir causa um postback e dispara o evento DataList s `DeleteCommand`. No manipulador de eventos, o valor de `ProductID` do produto clicado é acessado na coleção de `DataKeys`. Em seguida, o produto é excluído chamando o método de `DeleteProduct` da classe `ProductsBLL`.

Depois de excluir o produto, é importante que reassocie os dados ao DataList (`DataList1.DataBind()`), caso contrário, o DataList continuará mostrando o produto que acabou de ser excluído.

## <a name="summary"></a>Resumo

Embora o DataList não tenha o ponto e clique em Editar e excluir o suporte aproveitado pelo GridView, com um pouco de código, ele pode ser aprimorado para incluir esses recursos. Neste tutorial, vimos como criar uma listagem de duas colunas de produtos que poderiam ser excluídos e cujo nome e preço poderiam ser editados. Adicionar suporte à edição e exclusão é uma questão de incluir os controles da Web apropriados no `ItemTemplate` e `EditItemTemplate`, criar os manipuladores de eventos correspondentes, ler os valores de chave primária e inseridos pelo usuário e fazer a interface com a camada de lógica de negócios.

Embora tenhamos adicionado recursos básicos de edição e exclusão ao DataList, ele não tem recursos mais avançados. Por exemplo, não há nenhuma validação de campo de entrada – se um usuário inserir um preço muito caro, uma exceção será lançada por `Decimal.Parse` ao tentar converter muito caro em um `Decimal`. Da mesma forma, se houver um problema na atualização dos dados na lógica de negócios ou nas camadas de acesso a dados, o usuário será apresentado com a tela de erro padrão. Sem qualquer tipo de confirmação no botão excluir, a exclusão acidental de um produto é muito provável.

Em Tutoriais futuros, veremos como melhorar a experiência do usuário de edição.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Zack Jones, Ken Pespisa e Randy Schmidt. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](customizing-the-datalist-s-editing-interface-cs.md)
> [Próximo](performing-batch-updates-vb.md)
