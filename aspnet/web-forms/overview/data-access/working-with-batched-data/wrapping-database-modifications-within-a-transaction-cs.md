---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
title: Quebrando modificações de banco de dadosC#em uma transação () | Microsoft Docs
author: rick-anderson
description: Este tutorial é o primeiro de quatro que examina a atualização, a exclusão e a inserção de lotes de dados. Neste tutorial, aprendemos como as transações de banco de dados permitem...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: b45fede3-c53a-4ea1-824b-20200808dbae
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
msc.type: authoredcontent
ms.openlocfilehash: da69e466a5b506b869dc8fc0624f3e6a479199a8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74624649"
---
# <a name="wrapping-database-modifications-within-a-transaction-c"></a>Encapsulamento de modificações de banco de dados em uma transação (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_CS.zip) ou [baixar PDF](wrapping-database-modifications-within-a-transaction-cs/_static/datatutorial63cs1.pdf)

> Este tutorial é o primeiro de quatro que examina a atualização, a exclusão e a inserção de lotes de dados. Neste tutorial, aprendemos como as transações de banco de dados permitem que as modificações em lote sejam executadas como uma operação atômica, o que garante que todas as etapas tenham êxito ou que todas as etapas falhem.

## <a name="introduction"></a>Introdução

Como vimos começando com [uma visão geral do tutorial inserir, atualizar e excluir dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , o GridView fornece suporte interno para edição e exclusão em nível de linha. Com alguns cliques do mouse, é possível criar uma interface de modificação de dados rica sem escrever uma linha de código, desde que você tenha conteúdo com edição e exclusão em uma base por linha. No entanto, em determinados cenários, isso é insuficiente e precisamos fornecer aos usuários a capacidade de editar ou excluir um lote de registros.

Por exemplo, a maioria dos clientes de email baseados na Web usa uma grade para listar cada mensagem em que cada linha inclui uma caixa de seleção junto com as informações do email s (assunto, remetente e assim por diante). Essa interface permite que o usuário exclua várias mensagens verificando-as e clicando em um botão excluir mensagens selecionadas. Uma interface de edição em lote é ideal em situações em que os usuários normalmente editam vários registros diferentes. Em vez de forçar o usuário a clicar em Editar, fazer suas alterações e, em seguida, clicar em atualizar para cada registro que precisa ser modificado, uma interface de edição do lote renderiza cada linha com sua interface de edição. O usuário pode modificar rapidamente o conjunto de linhas que precisam ser alteradas e, em seguida, salvar essas alterações clicando em um botão atualizar tudo. Neste conjunto de tutoriais, examinaremos como criar interfaces para inserção, edição e exclusão de lotes de dados.

Ao executar operações em lote, é importante determinar se deve ser possível que algumas das operações no lote tenham êxito enquanto outras falham. Considere uma interface de exclusão de lote – o que deve acontecer se o primeiro registro selecionado for excluído com êxito, mas o segundo falhar, digamos, devido a uma violação de restrição de chave estrangeira? Se a exclusão de s do primeiro registro for revertida ou for aceitável que o primeiro registro permaneça excluído?

Se você quiser que a operação em lote seja tratada como uma [operação atômica](http://en.wikipedia.org/wiki/Atomic_operation), uma em que todas as etapas tenham êxito ou todas as etapas falharem, a camada de acesso a dados precisará ser aumentada para incluir suporte para [Transações de banco](http://en.wikipedia.org/wiki/Database_transaction)de dado. As transações de banco de dados garantem atomicidade para o conjunto de instruções `INSERT`, `UPDATE`e `DELETE` executadas sob a abrangência da transação e são um recurso com suporte na maioria dos sistemas de banco de dados modernos.

Neste tutorial, veremos como estender a DAL para usar transações de banco de dados. Os tutoriais subsequentes examinarão a implementação de páginas da Web para inserção, atualização e exclusão de interfaces em lotes. Vamos começar!

> [!NOTE]
> Ao modificar dados em uma transação em lote, a atomicidade nem sempre é necessária. Em alguns cenários, pode ser aceitável ter algumas modificações de dados bem sucedidas e outras no mesmo lote falham, como ao excluir um conjunto de emails de um cliente de email baseado na Web. Se houver um erro de banco de dados no centro do processo de exclusão, provavelmente os registros processados sem erros permanecerão excluídos. Nesses casos, a DAL não precisa ser modificada para dar suporte a transações de banco de dados. Há outros cenários de operação em lote, no entanto, onde a atomicidade é vital. Quando um cliente move seus fundos de uma conta bancária para outra, duas operações devem ser executadas: os fundos devem ser deduzidos da primeira conta e, em seguida, adicionados ao segundo. Embora o banco não tenha em mente que a primeira etapa é bem-sucedida, mas a segunda etapa falha, seus clientes poderiam ser aborrecidos. Recomendo que você trabalhe neste tutorial e implemente os aprimoramentos na DAL para dar suporte a transações de banco de dados, mesmo que não planeje usá-las no lote inserindo, atualizando e excluindo interfaces que vamos criar nos três tutoriais a seguir.

## <a name="an-overview-of-transactions"></a>Uma visão geral das transações

A maioria dos bancos de dados inclui suporte para *Transações*, que permitem que vários comandos de banco de dados sejam agrupados em uma única unidade lógica de trabalho. Os comandos de banco de dados que compõem uma transação têm a garantia de serem atômicas, o que significa que todos os comandos falharão ou todos terão êxito.

Em geral, as transações são implementadas por meio de instruções SQL usando o seguinte padrão:

1. Indique o início de uma transação.
2. Execute as instruções SQL que compõem a transação.
3. Se houver um erro em uma das instruções da etapa 2, reverta a transação.
4. Se todas as instruções da etapa 2 forem concluídas sem erros, confirme a transação.

As instruções SQL usadas para criar, confirmar e reverter a transação podem ser inseridas manualmente ao gravar scripts SQL ou criar procedimentos armazenados, ou por meio de programação, usando ADO.NET ou as classes no [namespace`System.Transactions`](https://msdn.microsoft.com/library/system.transactions.aspx). Neste tutorial, examinaremos apenas o gerenciamento de transações usando ADO.NET. Em um futuro tutorial, veremos como usar procedimentos armazenados na camada de acesso a dados, em que, neste momento, exploraremos as instruções SQL para criar, reverter e confirmar transações. Enquanto isso, consulte [Gerenciando transações em SQL Server procedimentos armazenados](http://www.4guysfromrolla.com/webtech/080305-1.shtml) para obter mais informações.

> [!NOTE]
> A [classe`TransactionScope`](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) no namespace `System.Transactions` permite que os desenvolvedores envolvam programaticamente uma série de instruções dentro do escopo de uma transação e inclui suporte para transações complexas que envolvem várias fontes, como dois bancos de dados diferentes ou até mesmo tipos heterogêneos de armazenamentos, como um banco de dado Microsoft SQL Server, um Oracle Database e um Web Service. Eu decidi usar as transações de ADO.NET para este tutorial em vez da classe `TransactionScope` porque ADO.NET é mais específico para transações de banco de dados e, em muitos casos, tem muito menos uso intensivo de recursos. Além disso, em determinados cenários, a classe `TransactionScope` usa o Microsoft Coordenador de Transações Distribuídas (MSDTC). A configuração, a implementação e os problemas de desempenho em torno do MSDTC o tornam um tópico bastante especializado e avançado e além do escopo desses tutoriais.

Ao trabalhar com o provedor SqlClient no ADO.NET, as transações são iniciadas por meio de uma chamada para a [classe`SqlConnection`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) [`BeginTransaction` método](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), que retorna um [objeto`SqlTransaction`](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). As instruções de modificação de dados que fazem a composição da transação são colocadas dentro de um bloco de `try...catch`. Se ocorrer um erro em uma instrução no bloco de `try`, a execução será transferida para o bloco de `catch` em que a transação pode ser revertida por meio do [método`Rollback`](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx)s do objeto `SqlTransaction`. Se todas as instruções forem concluídas com êxito, uma chamada para o objeto `SqlTransaction` s [`Commit` o método](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) no final do bloco de `try` confirmará a transação. O trecho de código a seguir ilustra esse padrão. Consulte [mantendo a consistência do banco de dados com transações](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) para obter mais sintaxe e exemplos de como usar transações com ADO.net.

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample1.cs)]

Por padrão, os TableAdapters em um DataSet tipado não usam transações. Para fornecer suporte para transações, precisamos aumentar as classes do TableAdapter para incluir métodos adicionais que usam o padrão acima para executar uma série de instruções de modificação de dados dentro do escopo de uma transação. Na etapa 2, veremos como usar classes parciais para adicionar esses métodos.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Etapa 1: criando o trabalho com páginas da Web de dados em lote

Antes de começarmos a explorar como aumentar a DAL para dar suporte a transações de banco de dados, vamos primeiro reservar um momento para criar as páginas da Web ASP.NET que precisaremos para este tutorial e as três que seguem. Comece adicionando uma nova pasta chamada `BatchData` e, em seguida, adicione as páginas ASP.NET a seguir, associando cada página à `Site.master` página mestra.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`

![Adicionar as páginas ASP.NET para os tutoriais relacionados ao SqlDataSource](wrapping-database-modifications-within-a-transaction-cs/_static/image1.gif)

**Figura 1**: adicionar as páginas ASP.net para os tutoriais relacionados ao SqlDataSource

Assim como ocorre com as outras pastas, `Default.aspx` usará o controle de usuário `SectionLevelTutorialListing.ascx` para listar os tutoriais em sua seção. Portanto, adicione esse controle de usuário a `Default.aspx` arrastando-o da Gerenciador de Soluções para a página s modo de exibição de Design.

[![adicionar o controle de usuário SectionLevelTutorialListing. ascx a default. aspx](wrapping-database-modifications-within-a-transaction-cs/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image1.png)

**Figura 2**: Adicionar o controle de usuário `SectionLevelTutorialListing.ascx` ao `Default.aspx` ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-cs/_static/image2.png))

Por fim, adicione essas quatro páginas como entradas ao arquivo de `Web.sitemap`. Especificamente, adicione a seguinte marcação após a personalização do mapa do site `<siteMapNode>`:

[!code-xml[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample2.xml)]

Depois de atualizar `Web.sitemap`, Reserve um momento para exibir o site de tutoriais por meio de um navegador. O menu à esquerda agora inclui itens para os tutoriais de trabalho com dados em lote.

![O mapa do site agora inclui entradas para os tutoriais de trabalho com dados em lote](wrapping-database-modifications-within-a-transaction-cs/_static/image3.gif)

**Figura 3**: o mapa do site agora inclui entradas para os tutoriais de trabalho com dados em lote

## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Etapa 2: atualizando a camada de acesso a dados para dar suporte a transações de banco de dado

Como discutimos de volta no primeiro tutorial, [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md), o dataset tipado em nossa Dal é composto por tabelas e TableAdapters. As tabelas contêm dados enquanto os TableAdapters fornecem a funcionalidade de leitura de dados do banco de dado para as DataTables, a fim de atualizá-lo com as alterações feitas nas tabelas e assim por diante. Lembre-se de que os TableAdapters fornecem dois padrões para a atualização de dados, que eu chamei de atualização de lote e de BD-Direct. Com o padrão de atualização do lote, o TableAdapter recebe um DataSet, DataTable ou uma coleção de DataRows. Esses dados são enumerados e para cada linha inserida, modificada ou excluída, o `InsertCommand`, `UpdateCommand`ou `DeleteCommand` é executado. Com o padrão DB-Direct, o TableAdapter é, em vez disso, passado os valores das colunas necessárias para inserir, atualizar ou excluir um único registro. O método de padrão direto do BD usa esses valores passados para executar a instrução `InsertCommand`, `UpdateCommand`ou `DeleteCommand` apropriada.

Independentemente do padrão de atualização usado, os métodos gerados automaticamente pelos TableAdapters não usam transações. Por padrão, cada INSERT, Update ou DELETE executado pelo TableAdapter é tratado como uma única operação discreta. Por exemplo, imagine que o padrão de BD-Direct é usado por algum código na BLL para inserir dez registros no banco de dados. Esse código chamaria o método do TableAdapter s `Insert` dez vezes. Se as cinco primeiras inserções forem realizadas com sucesso, mas a sexta resultar em uma exceção, os primeiros cinco registros inseridos permanecerão no banco de dados. Da mesma forma, se o padrão de atualização do lote for usado para executar inserções, atualizações e exclusões nas linhas inseridas, modificadas e excluídas em uma DataTable, se as várias modificações forem bem-sucedidas, mas uma versão posterior encontrou um erro, as modificações anteriores que concluído permaneceria no banco de dados.

Em determinados cenários, queremos garantir a atomicidade em uma série de modificações. Para fazer isso, devemos estender manualmente o TableAdapter adicionando novos métodos que executam o `InsertCommand`, `UpdateCommand`e `DeleteCommand` s sob a proteção de uma transação. Ao [criar uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md) , examinamos o uso de [classes parciais](http://en.wikipedia.org/wiki/Partial_type) para estender a funcionalidade das DataTables dentro do dataset tipado. Essa técnica também pode ser usada com TableAdapters.

O conjunto de `Northwind.xsd` de DataSet tipado está localizado na subpasta `App_Code` Folder s `DAL`. Crie uma subpasta na pasta `DAL` chamada `TransactionSupport` e adicione um novo arquivo de classe chamado `ProductsTableAdapter.TransactionSupport.cs` (consulte a Figura 4). Esse arquivo manterá a implementação parcial do `ProductsTableAdapter` que inclui métodos para executar modificações de dados usando uma transação.

![Adicione uma pasta chamada TransactionSupport e um arquivo de classe chamado ProductsTableAdapter.TransactionSupport.cs](wrapping-database-modifications-within-a-transaction-cs/_static/image4.gif)

**Figura 4**: adicionar uma pasta chamada `TransactionSupport` e um arquivo de classe chamado `ProductsTableAdapter.TransactionSupport.cs`

Digite o seguinte código no arquivo de `ProductsTableAdapter.TransactionSupport.cs`:

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample3.cs)]

A palavra-chave `partial` na declaração de classe aqui indica para o compilador que os membros adicionados dentro devem ser adicionados à classe `ProductsTableAdapter` no namespace `NorthwindTableAdapters`. Observe a instrução `using System.Data.SqlClient` na parte superior do arquivo. Como o TableAdapter foi configurado para usar o provedor SqlClient, internamente ele usa um [`SqlDataAdapter`](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) objeto para emitir seus comandos para o banco de dados. Consequentemente, precisamos usar a classe `SqlTransaction` para iniciar a transação e, em seguida, confirmá-la ou revertida-la. Se você estiver usando um armazenamento de dados diferente de Microsoft SQL Server, precisará usar o provedor apropriado.

Esses métodos fornecem os blocos de construção necessários para iniciar, reverter e confirmar uma transação. Elas são marcadas `public`, permitindo que elas sejam usadas no `ProductsTableAdapter`, de outra classe na DAL ou de outra camada na arquitetura, como a BLL. `BeginTransaction` abre o `SqlConnection` interno do TableAdapter (se necessário), inicia a transação e a atribui à propriedade `Transaction` e anexa a transação aos objetos internos `SqlDataAdapter` s `SqlCommand`. `CommitTransaction` e `RollbackTransaction` chamar os métodos `Commit` e `Rollback` do objeto `Transaction`, respectivamente, antes de fechar o objeto `Connection` interno.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Etapa 3: adicionando métodos para atualizar e excluir dados sob a abrangência de uma transação

Com esses métodos concluídos, estamos prontos para adicionar métodos a `ProductsDataTable` ou a BLL que executa uma série de comandos sob a proteção de uma transação. O método a seguir usa o padrão de atualização do lote para atualizar uma instância de `ProductsDataTable` usando uma transação. Ele inicia uma transação chamando o método `BeginTransaction` e, em seguida, usa um bloco `try...catch` para emitir as instruções de modificação de dados. Se a chamada para o método de `Update` do objeto `Adapter` resultar em uma exceção, a execução será transferida para o bloco de `catch` em que a transação será revertida e a exceção relançada. Lembre-se de que o método `Update` implementa o padrão de atualização em lote enumerando as linhas do `ProductsDataTable` fornecido e executando os `InsertCommand`, `UpdateCommand`e `DeleteCommand` necessários. Se qualquer um desses comandos resultar em um erro, a transação será revertida, desfazendo as modificações anteriores feitas durante o tempo de vida de s da transação. Caso a instrução de `Update` seja concluída sem erros, a transação é confirmada em sua totalidade.

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample4.cs)]

Adicione o método `UpdateWithTransaction` à classe `ProductsTableAdapter` por meio da classe Partial em `ProductsTableAdapter.TransactionSupport.cs`. Como alternativa, esse método pode ser adicionado à classe de `ProductsBLL` da camada de lógica de negócios com algumas pequenas alterações sintáticas. Ou seja, a palavra-chave em `this.BeginTransaction()`, `this.CommitTransaction()`e `this.RollbackTransaction()` precisaria ser substituída por `Adapter` (Lembre-se de que `Adapter` é o nome de uma propriedade em `ProductsBLL` do tipo `ProductsTableAdapter`).

O método `UpdateWithTransaction` usa o padrão de atualização do lote, mas uma série de chamadas de BD-Direct também pode ser usada dentro do escopo de uma transação, como mostra o método a seguir. O método `DeleteProductsWithTransaction` aceita como entrada um `List<T>` do tipo `int`, que são os `ProductID` s a serem excluídos. O método inicia a transação por meio de uma chamada para `BeginTransaction` e, em seguida, no bloco de `try`, itera pela lista fornecida chamando o método de `Delete` de padrões direto de DB para cada valor de `ProductID`. Se qualquer uma das chamadas para `Delete` falhar, o controle será transferido para o bloco de `catch` em que a transação é revertida e a exceção é relançada. Se todas as chamadas para `Delete` tiverem sucesso, a transação será confirmada. Adicione este método à classe `ProductsBLL`.

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample5.cs)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Aplicando transações em vários TableAdapters

O código relacionado à transação examinado neste tutorial permite que várias instruções em relação ao `ProductsTableAdapter` sejam tratadas como uma operação atômica. Mas e se várias modificações em tabelas de banco de dados diferentes precisarem ser executadas atomicamente? Por exemplo, ao excluir uma categoria, primeiro convémos reatribuir seus produtos atuais a alguma outra categoria. Essas duas etapas reatribuindo os produtos e excluindo a categoria devem ser executadas como uma operação atômica. Mas a `ProductsTableAdapter` inclui apenas métodos para modificar a tabela `Products` e a `CategoriesTableAdapter` inclui apenas métodos para modificar a tabela `Categories`. Então, como uma transação pode abranger ambos os TableAdapters?

Uma opção é adicionar um método ao `CategoriesTableAdapter` chamado `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` e fazer com que esse método chame um procedimento armazenado que reatribui os produtos e exclua a categoria dentro do escopo de uma transação definida no procedimento armazenado. Veremos como começar, confirmar e reverter transações em procedimentos armazenados em um tutorial futuro.

Outra opção é criar uma classe auxiliar na DAL que contenha o método `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)`. Esse método criaria uma instância do `CategoriesTableAdapter` e o `ProductsTableAdapter` e, em seguida, definiria esses dois TableAdapters `Connection` Propriedades para a mesma instância `SqlConnection`. Nesse ponto, qualquer um dos dois TableAdapters iniciaria a transação com uma chamada para `BeginTransaction`. Os métodos TableAdapters para reatribuir os produtos e excluir a categoria seriam invocados em um bloco de `try...catch` com a transação confirmada ou revertida conforme necessário.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Etapa 4: adicionando o método de`UpdateWithTransaction`à camada de lógica de negócios

Na etapa 3, adicionamos um método `UpdateWithTransaction` à `ProductsTableAdapter` na DAL. Devemos adicionar um método correspondente à BLL. Embora a camada de apresentação pudesse chamar diretamente para a DAL para invocar o método `UpdateWithTransaction`, esses tutoriais se empenhavam em definir uma arquitetura em camadas que separa a DAL da camada de apresentação. Portanto, ele nos convém para continuar essa abordagem.

Abra o arquivo de classe `ProductsBLL` e adicione um método chamado `UpdateWithTransaction` que simplesmente chame para o método DAL correspondente. Agora deve haver dois novos métodos no `ProductsBLL`: `UpdateWithTransaction`, que você acabou de adicionar e `DeleteProductsWithTransaction`, que foi adicionado na etapa 3.

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample6.cs)]

> [!NOTE]
> Esses métodos não incluem o atributo `DataObjectMethodAttribute` atribuído à maioria dos outros métodos na classe `ProductsBLL` porque iremos invocar esses métodos diretamente das classes code-behind de páginas ASP.NET. Lembre-se de que `DataObjectMethodAttribute` é usado para sinalizar quais métodos devem aparecer no assistente de configuração de fonte de dados ObjectDataSource s e em qual guia (selecionar, atualizar, inserir ou excluir). Como o GridView não tem suporte interno para edição ou exclusão em lotes, precisaremos invocar esses métodos programaticamente em vez de usar a abordagem declarativa sem código.

## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Etapa 5: Atualizando atomicamente os dados do banco da camada de apresentação

Para ilustrar o efeito que a transação tem ao atualizar um lote de registros, vamos criar uma interface do usuário que liste todos os produtos em um GridView e inclui um controle da Web de botão que, quando clicado, reatribui os produtos `CategoryID` valores. Em particular, a atribuição de categoria irá progredir para que os primeiros produtos recebam um valor de `CategoryID` válido, enquanto outros são atribuídos intencionalmente a um valor de `CategoryID` não existente. Se tentarmos atualizar o banco de dados com um produto cujo `CategoryID` não corresponde a um `CategoryID`de categoria existente, uma violação de restrição de chave estrangeira ocorrerá e uma exceção será gerada. O que veremos neste exemplo é que, ao usar uma transação, a exceção gerada pela violação de restrição de chave estrangeira fará com que as alterações de `CategoryID` válidas anteriores sejam revertidas. No entanto, quando você não estiver usando uma transação, as modificações nas categorias iniciais permanecerão.

Comece abrindo a página `Transactions.aspx` na pasta `BatchData` e arraste um GridView da caixa de ferramentas para o designer. Defina seu `ID` como `Products` e, em sua marca inteligente, associe-o a um novo ObjectDataSource chamado `ProductsDataSource`. Configure o ObjectDataSource para efetuar pull de seus dados do método de `GetProducts` da classe `ProductsBLL`. Esse será um GridView somente leitura, portanto, defina as listas suspensas nas guias atualizar, inserir e excluir como (nenhum) e clique em concluir.

[![Figura 5: configurar o ObjectDataSource para usar o método GetProducts da classe ProductsBLL](wrapping-database-modifications-within-a-transaction-cs/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image3.png)

**Figura 5**: configurar o ObjectDataSource para usar o método de `GetProducts` da classe `ProductsBLL` ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-cs/_static/image4.png))

[![definir as listas suspensas nas guias atualizar, inserir e excluir para (nenhum)](wrapping-database-modifications-within-a-transaction-cs/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image5.png)

**Figura 6**: definir as listas suspensas nas guias atualizar, inserir e excluir para (nenhum) ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-cs/_static/image6.png))

Depois de concluir o assistente para configurar fonte de dados, o Visual Studio criará BoundFields e um CheckBoxField para os campos de dados do produto. Remova todos esses campos, exceto `ProductID`, `ProductName`, `CategoryID`e `CategoryName` e renomeie os `ProductName` e `CategoryName` BoundFields `HeaderText` Propriedades para Product e Category, respectivamente. Na marca inteligente, marque a opção habilitar paginação. Depois de fazer essas modificações, a marcação declarativa de GridView e ObjectDataSource s deve ser parecida com a seguinte:

[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample7.aspx)]

Em seguida, adicione os controles da Web de três botões acima do GridView. Defina a propriedade de texto s do primeiro botão como atualizar grade, a segunda s para modificar categorias (com transação) e a terceira para modificar categorias (sem transação).

[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample8.aspx)]

Neste ponto, o modo de exibição de Design no Visual Studio deve ser semelhante à captura de tela mostrada na Figura 7.

[![a página contém um controle GridView e três botões da Web](wrapping-database-modifications-within-a-transaction-cs/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image7.png)

**Figura 7**: a página contém um controle GridView e três botões da Web ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-cs/_static/image8.png))

Crie manipuladores de eventos para cada um dos três eventos de `Click` de botão e use o seguinte código:

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample9.cs)]

O manipulador de eventos do botão atualizar s `Click` simplesmente reassocia os dados ao GridView chamando o método `Products` GridView `DataBind`.

O segundo manipulador de eventos reatribui os produtos `CategoryID` s e usa o novo método de transação da BLL para executar as atualizações de banco de dados sob a proteção de uma transação. Observe que cada produto s `CategoryID` é arbitrariamente definido com o mesmo valor que seu `ProductID`. Isso funcionará bem para os primeiros produtos, pois esses produtos têm `ProductID` valores que acontecem para mapear para os `CategoryID` válidos. Mas depois que a `ProductID` s começar a ficar muito grande, essa sobreposição coincidente de `ProductID` s e `CategoryID` s não se aplicará mais.

O terceiro manipulador de eventos de `Click` atualiza os produtos `CategoryID` s da mesma maneira, mas envia a atualização para o banco de dados usando o método de `Update` padrão `ProductsTableAdapter` s. Esse método de `Update` não encapsula a série de comandos em uma transação, portanto, essas alterações são feitas antes que o primeiro erro de violação de restrição de chave estrangeira seja persistido.

Para demonstrar esse comportamento, visite esta página por meio de um navegador. Inicialmente, você deve ver a primeira página de dados, como mostra a Figura 8. Em seguida, clique no botão modificar categorias (com transação). Isso causará um postback e tentará atualizar todos os produtos `CategoryID` valores, mas resultará em uma violação de restrição de chave estrangeira (consulte a Figura 9).

[![os produtos são exibidos em um GridView paginável](wrapping-database-modifications-within-a-transaction-cs/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image9.png)

**Figura 8**: os produtos são exibidos em um GridView paginável ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-cs/_static/image10.png))

[![reatribuir as categorias resulta em uma violação de restrição de chave estrangeira](wrapping-database-modifications-within-a-transaction-cs/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image11.png)

**Figura 9**: Reatribuir as categorias resulta em uma violação de restrição de chave estrangeira ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-cs/_static/image12.png))

Agora, pressione o botão voltar do navegador e clique no botão atualizar grade. Ao atualizar os dados, você deve ver exatamente a mesma saída, conforme mostrado na Figura 8. Ou seja, embora alguns dos produtos `CategoryID` s tenham sido alterados para valores válidos e atualizados no banco de dados, eles foram revertidos quando ocorreu a violação da restrição de chave estrangeira.

Agora, tente clicar no botão modificar categorias (sem transação). Isso resultará no mesmo erro de violação de restrição de chave estrangeira (consulte a Figura 9), mas desta vez os produtos cujos valores de `CategoryID` foram alterados para um valor válido não serão revertidos. Pressione o botão voltar do navegador e, em seguida, o botão atualizar grade. Como mostra a Figura 10, os `CategoryID` s dos primeiros oito produtos foram reatribuídos. Por exemplo, na Figura 8, Chang tinha um `CategoryID` de 1, mas na Figura 10 ele foi reatribuído para 2.

[![alguns produtos os valores de CategoryID foram atualizados enquanto outros não foram](wrapping-database-modifications-within-a-transaction-cs/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image13.png)

**Figura 10**: alguns produtos `CategoryID` valores foram atualizados enquanto outros não eram ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-cs/_static/image14.png))

## <a name="summary"></a>Resumo

Por padrão, os métodos do TableAdapter s não encapsulam as instruções de banco de dados executadas dentro do escopo de uma transação, mas com um pouco de trabalho, podemos adicionar métodos que criarão, confirmarão e reverterão uma transação. Neste tutorial, criamos três métodos, na classe `ProductsTableAdapter`: `BeginTransaction`, `CommitTransaction`e `RollbackTransaction`. Vimos como usar esses métodos junto com um bloco de `try...catch` para fazer uma série de instruções de modificação de dados atômicas. Em particular, criamos o método `UpdateWithTransaction` no `ProductsTableAdapter`, que usa o padrão de atualização do lote para executar as modificações necessárias nas linhas de um `ProductsDataTable`fornecido. Também adicionamos o método `DeleteProductsWithTransaction` à classe `ProductsBLL` na BLL, que aceita uma `List` de valores de `ProductID` como sua entrada e chama o método de padrão DB-Direct `Delete` para cada `ProductID`. Os dois métodos começam criando uma transação e, em seguida, executando as instruções de modificação de dados em um bloco de `try...catch`. Se ocorrer uma exceção, a transação será revertida, caso contrário, ela será confirmada.

A etapa 5 ilustra o efeito de atualizações de lote transacionais versus atualizações em lotes que não se desativam para usar uma transação. Nos próximos três tutoriais, vamos criar a base apresentada neste tutorial e criar interfaces do usuário para executar atualizações, exclusões e inserções em lotes.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Mantendo a consistência do banco de dados com transações](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Gerenciando transações em SQL Server procedimentos armazenados](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Transações facilitadas: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope e DataAdapters](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Usando transações de Oracle Database no .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Dave Gardner, Hilton Giesenow e Teresa Murphy. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](batch-updating-cs.md)
