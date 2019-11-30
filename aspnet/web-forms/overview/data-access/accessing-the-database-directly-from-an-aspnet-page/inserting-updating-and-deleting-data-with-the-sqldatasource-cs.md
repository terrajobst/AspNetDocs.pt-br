---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: Inserção, atualização e exclusão de dados com o SqlDataSource (C#) | Microsoft Docs
author: rick-anderson
description: Nos tutoriais anteriores, aprendemos como o controle ObjectDataSource permitia inserir, atualizar e excluir dados. O controle SqlDataSource dá suporte a t...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 495e0e81a67e6926e1c4fa92e29ebbda747cd418
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74610502"
---
# <a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>Inserir, atualizar e excluir dados com o SqlDataSource (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe) ou [baixar PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> Nos tutoriais anteriores, aprendemos como o controle ObjectDataSource permitia inserir, atualizar e excluir dados. O controle SqlDataSource dá suporte às mesmas operações, mas a abordagem é diferente, e este tutorial mostra como configurar o SqlDataSource para inserir, atualizar e excluir dados.

## <a name="introduction"></a>Introdução

Conforme discutido em [uma visão geral de inserção, atualização e exclusão](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), o controle GridView fornece recursos internos de atualização e exclusão, enquanto os controles DetailsView e FormView incluem inserção de suporte juntamente com a funcionalidade de edição e exclusão. Esses recursos de modificação de dados podem ser conectados diretamente a um controle da fonte de dados sem a necessidade de escrever uma linha de código. [Uma visão geral de inserção, atualização e exclusão](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) examinados usando o ObjectDataSource para facilitar a inserção, atualização e exclusão com os controles GridView, DetailsView e FormView. Como alternativa, o SqlDataSource pode ser usado no lugar do ObjectDataSource.

Lembre-se de que para dar suporte à inserção, atualização e exclusão, com o ObjectDataSource, precisávamos especificar os métodos de camada de objeto a serem invocados para executar a ação de inserção, atualização ou exclusão. Com o SqlDataSource, precisamos fornecer instruções SQL `INSERT`, `UPDATE`e `DELETE` (ou procedimentos armazenados) para executar. Como veremos neste tutorial, essas instruções podem ser criadas manualmente ou podem ser geradas automaticamente pelo Assistente para configurar fonte de dados do SqlDataSource s.

> [!NOTE]
> Como já vimos os recursos de inserção, edição e exclusão dos controles GridView, DetailsView e FormView, este tutorial se concentrará na configuração do controle SqlDataSource para dar suporte a essas operações. Se você precisar começar a implementar esses recursos dentro de GridView, DetailsView e FormView, retorne aos tutoriais de edição, inserção e exclusão de dados, começando com [uma visão geral de inserção, atualização e exclusão](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md).

## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Etapa 1: especificando instruções`INSERT`,`UPDATE`e`DELETE`

Como vimos nos dois tutoriais anteriores, para recuperar dados de um controle SqlDataSource, precisamos definir duas propriedades:

1. `ConnectionString`, que especifica para qual banco de dados enviar a consulta e
2. `SelectCommand`, que especifica a instrução SQL ad hoc ou o nome do procedimento armazenado a ser executado para retornar os resultados.

Para `SelectCommand` valores com parâmetros, os valores de parâmetro são especificados por meio da coleção SqlDataSource s `SelectParameters` e podem incluir valores embutidos em código, valores de origem de parâmetro comuns (campos QueryString, variáveis de sessão, valores de controle da Web e assim por diante) ou podem ser atribuídos de forma programática. Quando o método do controle SqlDataSource s `Select()` é invocado programaticamente ou automaticamente de um controle da Web de dados, uma conexão com o banco de dado é estabelecida, os valores de parâmetro são atribuídos à consulta e o comando é desconectado ao banco de dados. Os resultados são retornados como um DataSet ou DataReader, dependendo do valor da propriedade Control s `DataSourceMode`.

Juntamente com a seleção de dados, o controle SqlDataSource pode ser usado para inserir, atualizar e excluir dados, fornecendo `INSERT`, `UPDATE`e `DELETE` instruções SQL da mesma maneira. Basta atribuir as propriedades `InsertCommand`, `UpdateCommand`e `DeleteCommand` as instruções SQL `INSERT`, `UPDATE`e `DELETE` a serem executadas. Se as instruções tiverem parâmetros (como sempre serão), inclua-os nas coleções `InsertParameters`, `UpdateParameters`e `DeleteParameters`.

Depois que um valor `InsertCommand`, `UpdateCommand`ou `DeleteCommand` tiver sido especificado, a opção Habilitar inserção, habilitar edição ou habilitar a exclusão na marca inteligente s de controle da Web de dados correspondente será disponibilizada. Para ilustrar isso, vamos pegar um exemplo da página `Querying.aspx` que criamos nos [dados de consulta com o tutorial de controle SqlDataSource](querying-data-with-the-sqldatasource-control-cs.md) e aumentá-lo para incluir recursos de exclusão.

Comece abrindo as páginas `InsertUpdateDelete.aspx` e `Querying.aspx` da pasta `SqlDataSource`. No designer na página `Querying.aspx`, selecione o SqlDataSource e o GridView do primeiro exemplo (os controles `ProductsDataSource` e `GridView1`). Depois de selecionar os dois controles, vá para o menu Editar e escolha copiar (ou apenas pressione CTRL + C). Em seguida, vá para o designer de `InsertUpdateDelete.aspx` e cole os controles. Depois de mover os dois controles para `InsertUpdateDelete.aspx`, teste a página em um navegador. Você deve ver os valores das colunas `ProductID`, `ProductName`e `UnitPrice` para todos os registros na tabela `Products` banco de dados.

[![todos os produtos estão listados, ordenados por ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**Figura 1**: todos os produtos são listados, ordenados por `ProductID` ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))

## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Adicionando as propriedades SqlDataSource s`DeleteCommand`e`DeleteParameters`

Neste ponto, temos um SqlDataSource que simplesmente retorna todos os registros da tabela `Products` e um GridView que renderiza esses dados. Nosso objetivo é estender este exemplo para permitir que o usuário exclua produtos por meio do GridView. Para fazer isso, precisamos especificar valores para as propriedades do controle SqlDataSource s `DeleteCommand` e `DeleteParameters` e, em seguida, configurar o GridView para dar suporte à exclusão.

As propriedades `DeleteCommand` e `DeleteParameters` podem ser especificadas de várias maneiras:

- Por meio da sintaxe declarativa
- Do janela Propriedades no designer
- Na tela especificar uma instrução SQL personalizada ou um procedimento armazenado no Assistente para configurar fonte de dados
- Por meio do botão Avançado, na tela especificar colunas de uma tabela de exibição no Assistente para configurar fonte de dados, o que gerará automaticamente a instrução `DELETE` SQL e a coleção de parâmetros usada nas propriedades `DeleteCommand` e `DeleteParameters`

Examinaremos como fazer automaticamente a instrução de `DELETE` criada na etapa 2. Por enquanto, vamos usar o janela Propriedades no designer, embora o assistente para configurar fonte de dados ou a opção de sintaxe declarativa também funcione bem.

No designer no `InsertUpdateDelete.aspx`, clique no `ProductsDataSource` SqlDataSource e, em seguida, ative o janela Propriedades (no menu Exibir, escolha janela Propriedades ou simplesmente pressione F4). Selecione a Propriedade DeleteQuery, que abrirá um conjunto de reticências.

![Selecione a Propriedade DeleteQuery na janela Propriedades](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**Figura 2**: selecionar a Propriedade DeleteQuery na janela Propriedades

> [!NOTE]
> O SqlDataSource não tem uma Propriedade DeleteQuery. Em vez disso, DeleteQuery é uma combinação das propriedades `DeleteCommand` e `DeleteParameters` e é listada somente no janela Propriedades ao exibir a janela por meio do designer. Se você estiver olhando para a janela Propriedades na exibição de origem, encontrará a propriedade `DeleteCommand` em vez disso.

Clique nas reticências na Propriedade DeleteQuery para abrir a caixa de diálogo Editor de comando e parâmetro (consulte a Figura 3). Nessa caixa de diálogo, você pode especificar a instrução `DELETE` SQL e especificar os parâmetros. Insira a consulta a seguir na caixa de texto do comando `DELETE` (manualmente ou usando o Construtor de Consultas, se preferir):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

Em seguida, clique no botão atualizar parâmetros para adicionar o parâmetro `@ProductID` à lista de parâmetros abaixo.

[![selecionar a Propriedade DeleteQuery na janela Propriedades](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**Figura 3**: selecione a Propriedade DeleteQuery na janela Propriedades ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))

*Não* forneça um valor para esse parâmetro (Deixe sua origem de parâmetro em nenhum). Depois de adicionarmos o suporte de exclusão ao GridView, o GridView fornecerá automaticamente esse valor de parâmetro, usando o valor de sua coleção de `DataKeys` para a linha cujo botão de exclusão foi clicado.

> [!NOTE]
> O nome do parâmetro usado na consulta `DELETE` *deve* ser igual ao nome do valor de `DataKeyNames` em GridView, DetailsView ou FormView. Ou seja, o parâmetro na instrução `DELETE` é intencionalmente nomeado `@ProductID` (em vez de, digamos, `@ID`), porque o nome da coluna de chave primária na tabela Products (e, portanto, o valor DataKeyNames no GridView) é `ProductID`.

Se o nome do parâmetro e `DataKeyNames` valor não corresponder, o GridView não poderá atribuir automaticamente o parâmetro ao valor da coleção de `DataKeys`.

Depois de inserir as informações relacionadas à exclusão na caixa de diálogo Editor de comandos e parâmetros, clique em OK e vá para a exibição código-fonte para examinar a marcação declarativa resultante:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

Observe a adição da propriedade `DeleteCommand`, bem como a seção `<DeleteParameters>` e o objeto de parâmetro chamado `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Configurando o GridView para exclusão

Com a propriedade `DeleteCommand` adicionada, a marca inteligente s GridView agora contém a opção Habilitar exclusão. Vá em frente e marque essa caixa de seleção. Conforme discutido em [uma visão geral de inserção, atualização e exclusão](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md), isso faz com que o GridView adicione um CommandField com sua propriedade `ShowDeleteButton` definida como `true`. Como mostra a Figura 4, quando a página é visitada por meio de um navegador, um botão excluir é incluído. Teste essa página excluindo alguns produtos.

[![cada linha GridView agora inclui um botão Delete](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**Figura 4**: cada linha GridView agora inclui um botão Delete ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))

Ao clicar em um botão de exclusão, um postback ocorre, o GridView atribui o parâmetro `ProductID` o valor do valor da coleção `DataKeys` para a linha cujo botão de exclusão foi clicado e invoca o método SqlDataSource s `Delete()`. Em seguida, o controle SqlDataSource se conecta ao banco de dados e executa a instrução `DELETE`. Em seguida, o GridView reassocia ao SqlDataSource, voltando e exibindo o conjunto atual de produtos (que não inclui mais o registro recém-excluído).

> [!NOTE]
> Como o GridView usa sua coleção de `DataKeys` para popular os parâmetros SqlDataSource, é vital que a Propriedade GridView s `DataKeyNames` seja definida como as colunas que constituem a chave primária e que o SqlDataSource s `SelectCommand` retorna essas colunas. Além disso, é importante que o nome do parâmetro na `DeleteCommand` de SqlDataSource s seja definido como `@ProductID`. Se a propriedade `DataKeyNames` não estiver definida ou o parâmetro não for nomeado `@ProductsID`, clicar no botão excluir causará um postback, mas não excluirá realmente nenhum registro.

A Figura 5 ilustra essa interação graficamente. Consulte o tutorial [examinando os eventos associados à inserção, atualização e exclusão](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) para obter uma discussão mais detalhada sobre a cadeia de eventos associada à inserção, atualização e exclusão de um controle da Web de dados.

![Clicar no botão excluir no GridView invoca o método SqlDataSource s Delete ()](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**Figura 5**: clicar no botão Delete no GridView invoca o método SqlDataSource s `Delete()`

## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Etapa 2: gerando automaticamente as instruções`INSERT`,`UPDATE`e`DELETE`

Como a etapa 1 examinou, `INSERT`, `UPDATE`e `DELETE` instruções SQL podem ser especificadas por meio do janela Propriedades ou da sintaxe declarativa de controle s. No entanto, essa abordagem exige que as instruções SQL sejam gravadas manualmente, o que pode ser monótono e propenso a erros. Felizmente, o assistente para configurar fonte de dados fornece uma opção para ter as instruções `INSERT`, `UPDATE`e `DELETE` geradas automaticamente ao usar a tela especificar colunas de uma tabela de exibição.

Vamos explorar essa opção de geração automática. Adicione um DetailsView ao designer no `InsertUpdateDelete.aspx` e defina sua propriedade `ID` como `ManageProducts`. Em seguida, na marca inteligente DetailsView, escolha criar uma nova fonte de dados e criar um SqlDataSource chamado `ManageProductsDataSource`.

[![criar um novo SqlDataSource nomeado ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**Figura 6**: criar um novo SqlDataSource nomeado `ManageProductsDataSource` ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))

No Assistente para configurar fonte de dados, opte por usar a cadeia de conexão `NORTHWINDConnectionString` e clique em Avançar. Na tela configurar a instrução SELECT, deixe o botão de opção especificar colunas de uma tabela ou exibição selecionado e escolha a tabela `Products` na lista suspensa. Selecione as colunas `ProductID`, `ProductName`, `UnitPrice`e `Discontinued` na lista de caixas de seleção.

[![usar a tabela Products, retornar as colunas ProductID, ProductName, PreçoUnitário e descontinuadas](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**Figura 7**: usando a tabela `Products`, retorne as colunas `ProductID`, `ProductName`, `UnitPrice`e `Discontinued` ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))

Para gerar automaticamente instruções `INSERT`, `UPDATE`e `DELETE` com base na tabela e nas colunas selecionadas, clique no botão Avançado e marque a caixa de seleção gerar `INSERT`, `UPDATE`e `DELETE` instruções.

![Marque a caixa de seleção gerar instruções INSERT, UPDATE e DELETE](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**Figura 8**: marque a caixa de seleção gerar `INSERT`, `UPDATE`e `DELETE` instruções

A caixa de seleção gerar `INSERT`, `UPDATE`e `DELETE` de instruções só poderá ser verificada se a tabela selecionada tiver uma chave primária e a coluna de chave primária (ou colunas) estiver incluída na lista de colunas retornadas. A caixa de seleção usar simultaneidade otimista, que se torna selecionável após a verificação da caixa de seleção gerar `INSERT`, `UPDATE`e `DELETE` instruções, aumentará as cláusulas `WHERE` nas instruções `UPDATE` e `DELETE` resultantes para fornecer controle de simultaneidade otimista. Por enquanto, deixe essa caixa de seleção desmarcada; Examinaremos a simultaneidade otimista com o controle SqlDataSource no próximo tutorial.

Depois de marcar a caixa de seleção gerar `INSERT`, `UPDATE`e `DELETE` instruções, clique em OK para retornar à tela configurar instrução SELECT, clique em avançar e em concluir para concluir o assistente para configurar fonte de dados. Após a conclusão do assistente, o Visual Studio adicionará BoundFields ao DetailsView para as colunas `ProductID`, `ProductName`e `UnitPrice` e um CheckBoxField para a coluna `Discontinued`. Na marca inteligente DetailsView s, marque a opção habilitar paginação para que o usuário que visitar esta página possa percorrer os produtos. Além disso, desmarque as propriedades de `Width` e `Height` DetailsView s.

Observe que a marca inteligente tem as opções Habilitar inserção, habilitar edição e habilitar exclusão disponíveis. Isso ocorre porque o SqlDataSource contém valores para seu `InsertCommand`, `UpdateCommand`e `DeleteCommand`, como mostra a seguinte sintaxe declarativa:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

Observe como o controle SqlDataSource tinha valores definidos automaticamente para suas propriedades `InsertCommand`, `UpdateCommand`e `DeleteCommand`. O conjunto de colunas referenciadas nas propriedades `InsertCommand` e `UpdateCommand` se baseia neles na instrução `SELECT`. Ou seja, em vez de ter *cada* coluna de produtos na `InsertCommand` e `UpdateCommand`, há apenas as colunas especificadas no `SelectCommand` (menos `ProductID`, que é omitido porque ele é uma [coluna de`IDENTITY`](http://www.sqlteam.com/item.asp?ItemID=102), cujo valor não pode ser alterado quando editado e que é atribuído automaticamente ao inserir). Além disso, para cada parâmetro nas propriedades `InsertCommand`, `UpdateCommand`e `DeleteCommand` há parâmetros correspondentes nas coleções `InsertParameters`, `UpdateParameters`e `DeleteParameters`.

Para ativar os recursos de modificação de dados de DetailsView s, marque as opções Habilitar inserção, habilitar edição e habilitar a exclusão em sua marca inteligente. Isso adiciona um CommandField com suas propriedades `ShowInsertButton`, `ShowEditButton`e `ShowDeleteButton` definidas como `true`.

Visite a página em um navegador e observe os botões editar, excluir e novo incluídos no DetailsView. Clicar no botão Editar transforma o DetailsView em modo de edição, que exibe cada BoundField cuja propriedade `ReadOnly` está definida como `false` (o padrão) como uma caixa de texto e o CheckBoxField como uma caixa de seleção.

[![a interface de edição padrão de DetailsView s](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**Figura 9**: a interface de edição padrão de DetailsView s ([clique para exibir a imagem em tamanho normal](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))

Da mesma forma, você pode excluir o produto selecionado no momento ou adicionar um novo produto ao sistema. Como a instrução `InsertCommand` só funciona com as colunas `ProductName`, `UnitPrice`e `Discontinued`, as outras colunas têm `NULL` ou seu valor padrão atribuído pelo banco de dados na inserção. Assim como com o ObjectDataSource, se a `InsertCommand` não tiver nenhuma coluna de tabela de banco de dados que Don t permita `NULL` s e Don t tiver um valor padrão, ocorrerá um erro de SQL ao tentar executar a instrução `INSERT`.

> [!NOTE]
> As interfaces de inserção e edição do DetailsView s não têm qualquer tipo de personalização ou validação. Para adicionar controles de validação ou personalizar as interfaces, você precisa converter os BoundFields em TemplateFields. Consulte o [adicionando controles de validação para as interfaces de edição e inserção](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) e [Personalizando os tutoriais da interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) para obter mais informações.

Além disso, lembre-se de que, para atualizar e excluir, o DetailsView usa o valor de `DataKey` do produto atual, que só estará presente se a propriedade `DataKeyNames` estiver configurada. Se a edição ou exclusão parecer não ter efeito, verifique se a propriedade `DataKeyNames` está definida.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Limitações da geração automática de instruções SQL

Como a opção gerar instruções `INSERT`, `UPDATE`e `DELETE` só está disponível ao separar colunas de uma tabela, para consultas mais complexas, você terá que escrever suas próprias instruções `INSERT`, `UPDATE`e `DELETE` como fizemos na etapa 1. Normalmente, as instruções do SQL `SELECT` usam `JOIN` s para trazer de volta dados de uma ou mais tabelas de pesquisa para fins de exibição (como retornar o campo de `CategoryName` da tabela de `Categories` ao exibir informações do produto). Ao mesmo tempo, talvez queiramos permitir que o usuário edite, atualize ou insira dados na tabela principal (`Products`, nesse caso).

Embora as instruções `INSERT`, `UPDATE`e `DELETE` possam ser inseridas manualmente, considere a dica de economia de tempo a seguir. Inicialmente, configure o SqlDataSource para que ele Extraia os dados apenas da tabela `Products`. Use a tela configurar o assistente de fonte de dados s especificar colunas de uma tabela ou exibição para que você possa gerar automaticamente as instruções `INSERT`, `UPDATE`e `DELETE`. Em seguida, depois de concluir o assistente, escolha configurar o SelectQuery do janela Propriedades (ou, como alternativa, volte para o assistente para configurar fonte de dados, mas use a opção especificar uma instrução SQL personalizada ou procedimento armazenado). Em seguida, atualize a instrução `SELECT` para incluir a sintaxe de `JOIN`. Essa técnica oferece os benefícios de economia de tempo das instruções SQL geradas automaticamente e permite uma instrução de `SELECT` mais personalizada.

Outra limitação de gerar automaticamente as instruções `INSERT`, `UPDATE`e `DELETE` é que as colunas nas instruções `INSERT` e `UPDATE` são baseadas nas colunas retornadas pela instrução `SELECT`. No entanto, talvez seja necessário atualizar ou inserir mais ou menos campos. Por exemplo, no exemplo da etapa 2, talvez queiramos que o `UnitPrice` BoundField seja somente leitura. Nesse caso, ele não deve aparecer na `UpdateCommand`. Ou talvez queiramos definir o valor de um campo de tabela que não aparece no GridView. Por exemplo, ao adicionar um novo registro, talvez queiramos que o valor `QuantityPerUnit` definido como TODO.

Se essas personalizações forem necessárias, você precisará torná-las manualmente, por meio do janela Propriedades, a opção especificar uma instrução SQL personalizada ou um procedimento armazenado no assistente ou por meio da sintaxe declarativa.

> [!NOTE]
> Ao adicionar parâmetros que não têm campos correspondentes no controle da Web de dados, tenha em mente que esses valores de parâmetros precisarão ser atribuídos a valores de alguma maneira. Esses valores podem ser: embutidos no código diretamente no `InsertCommand` ou `UpdateCommand`; pode vir de alguma fonte predefinida (a QueryString, o estado da sessão, os controles da Web na página e assim por diante); ou pode ser atribuído programaticamente, como vimos no tutorial anterior.

## <a name="summary"></a>Resumo

Para que os controles da Web de dados utilizem seus recursos internos de inserção, edição e exclusão, o controle da fonte de dados ao qual eles estão vinculados deve oferecer essa funcionalidade. Para o SqlDataSource, isso significa que `INSERT`, `UPDATE`e `DELETE` instruções SQL devem ser atribuídas às propriedades `InsertCommand`, `UpdateCommand`e `DeleteCommand`. Essas propriedades e as coleções de parâmetros correspondentes podem ser adicionadas manualmente ou geradas automaticamente por meio do assistente para configurar fonte de dados. Neste tutorial, examinamos as duas técnicas.

Examinamos o uso de simultaneidade otimista com o ObjectDataSource no tutorial [implementando simultaneidade otimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) . O controle SqlDataSource também fornece suporte de simultaneidade otimista. Conforme observado na etapa 2, ao gerar automaticamente as instruções `INSERT`, `UPDATE`e `DELETE`, o assistente oferece uma opção usar simultaneidade otimista. Como veremos no próximo tutorial, o uso de simultaneidade otimista com o SqlDataSource modifica as cláusulas `WHERE` nas instruções `UPDATE` e `DELETE` para garantir que os valores das outras colunas não sejam alterados desde que os dados foram exibidos pela última vez na página.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [Próximo](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
