---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
title: Trabalhando com colunas computadasC#() | Microsoft Docs
author: rick-anderson
description: Ao criar uma tabela de banco de dados, Microsoft SQL Server permite que você defina uma coluna computada cujo valor é calculado a partir de uma expressão que geralmente se refere...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 57459065-ed7c-4dfe-ac9c-54c093abc261
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: ad6a96f2721510c2478f707c8eed018ae797f27a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74603163"
---
# <a name="working-with-computed-columns-c"></a>Trabalhar com colunas computadas (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_CS.zip) ou [baixar PDF](working-with-computed-columns-cs/_static/datatutorial71cs1.pdf)

> Ao criar uma tabela de banco de dados, Microsoft SQL Server permite que você defina uma coluna computada cujo valor é calculado a partir de uma expressão que geralmente faz referência a outros valores no mesmo registro de banco de dados. Esses valores são somente leitura no banco de dados, o que exige considerações especiais ao trabalhar com TableAdapters. Neste tutorial, aprendemos como atender aos desafios apresentados por colunas computadas.

## <a name="introduction"></a>Introdução

Microsoft SQL Server permite *[colunas computadas](https://msdn.microsoft.com/library/ms191250.aspx)* , que são colunas cujos valores são calculados a partir de uma expressão que geralmente referencia os valores de outras colunas na mesma tabela. Por exemplo, um modelo de dados de rastreamento de tempo pode ter uma tabela chamada `ServiceLog` com colunas, incluindo `ServicePerformed`, `EmployeeID`, `Rate`e `Duration`, entre outras. Embora a quantia devida por item de serviço (sendo a taxa multiplicada pela duração) pudesse ser calculada por meio de uma página da Web ou outra interface programática, pode ser útil incluir uma coluna na tabela `ServiceLog` chamada `AmountDue` que reportou essas informações. Esta coluna pode ser criada como uma coluna normal, mas precisa ser atualizada sempre que o `Rate` ou `Duration` valores de coluna forem alterados. Uma abordagem melhor seria tornar a coluna `AmountDue` uma coluna computada usando a expressão `Rate * Duration`. Isso faria com que SQL Server calcular automaticamente o valor da coluna `AmountDue` sempre que ele fosse referenciado em uma consulta.

Como um valor de s de coluna computada é determinado por uma expressão, essas colunas são somente leitura e, portanto, não podem ter valores atribuídos a elas em instruções `INSERT` ou `UPDATE`. No entanto, quando as colunas computadas fazem parte da consulta principal para um TableAdapter que usa instruções SQL ad hoc, elas são incluídas automaticamente nas instruções `INSERT` geradas automaticamente e `UPDATE`. Consequentemente, as consultas do TableAdapter s `INSERT` e `UPDATE` e as propriedades `InsertCommand` e `UpdateCommand` devem ser atualizadas para remover referências a quaisquer colunas computadas.

Um desafio de usar colunas computadas com um TableAdapter que usa instruções SQL ad hoc é que as consultas do TableAdapter s `INSERT` e `UPDATE` são regeneradas automaticamente sempre que o assistente de configuração do TableAdapter é concluído. Portanto, as colunas computadas são removidas manualmente da `INSERT` e `UPDATE` consultas reaparecerão se o assistente for executado novamente. Embora os TableAdapters que usam procedimentos armazenados Don t sofram dessa fragilidade, eles têm suas próprias sutilezas que iremos abordar na etapa 3.

Neste tutorial, adicionaremos uma coluna computada à tabela `Suppliers` no banco de dados Northwind e, em seguida, criaremos um TableAdapter correspondente para trabalhar com essa tabela e sua coluna computada. Nosso TableAdapter usará procedimentos armazenados em vez de instruções SQL ad hoc para que nossas personalizações não sejam perdidas quando o assistente de configuração do TableAdapter for usado.

Vamos começar!

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Etapa 1: adicionar uma coluna computada à tabela de`Suppliers`

O banco de dados Northwind não tem nenhuma coluna computada, portanto, precisaremos adicionar um por si só. Para este tutorial, vamos adicionar uma coluna computada à tabela de `Suppliers` chamada `FullContactName` que retorna o nome, o título e a empresa do contato em que eles trabalham no seguinte formato: `ContactName` (`ContactTitle`, `CompanyName`). Essa coluna computada pode ser usada em relatórios ao exibir informações sobre fornecedores.

Comece abrindo a definição da tabela de `Suppliers` clicando com o botão direito do mouse na tabela `Suppliers` no Gerenciador de Servidores e escolhendo Abrir definição de tabela no menu de contexto. Isso exibirá as colunas da tabela e suas propriedades, como o tipo de dados, se elas permitem `NULL` s e assim por diante. Para adicionar uma coluna computada, comece digitando o nome da coluna na definição da tabela. Em seguida, insira sua expressão na caixa de texto (fórmula) na seção especificação de coluna computada na janela Propriedades da coluna (consulte a Figura 1). Nomeie a coluna computada `FullContactName` e use a seguinte expressão:

[!code-sql[Main](working-with-computed-columns-cs/samples/sample1.sql)]

Observe que as cadeias de caracteres podem ser concatenadas no SQL usando o operador `+`. A instrução `CASE` pode ser usada como uma condicional em uma linguagem de programação tradicional. Na expressão acima, a instrução `CASE` pode ser lida como: se `ContactTitle` não estiver `NULL`, em seguida, gere o valor de `ContactTitle` concatenado com uma vírgula; caso contrário, não emitirá nada. Para obter mais informações sobre a utilidade da instrução `CASE`, consulte [o poder das instruções SQL `CASE`](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Em vez de usar uma instrução `CASE` aqui, poderíamos ter usado `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) retornará *CheckId* se ela não for nula, caso contrário retornará *replacevalue*. Embora `ISNULL` ou `CASE` funcionem nessa instância, há cenários mais complexos em que a flexibilidade da instrução `CASE` não pode corresponder ao `ISNULL`.

Depois de adicionar essa coluna computada, sua tela deve ser parecida com a captura de tela na Figura 1.

[![adicionar uma coluna computada chamada FullContactName à tabela Suppliers](working-with-computed-columns-cs/_static/image2.png)](working-with-computed-columns-cs/_static/image1.png)

**Figura 1**: adicionar uma coluna computada chamada `FullContactName` à tabela `Suppliers` ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image3.png))

Depois de nomear a coluna computada e inserir sua expressão, salve as alterações na tabela clicando no ícone salvar na barra de ferramentas, pressionando CTRL + S ou acessando o menu arquivo e escolhendo salvar `Suppliers`.

Salvar a tabela deve atualizar a Gerenciador de Servidores, incluindo a coluna recém-adicionada na lista de colunas da tabela de `Suppliers`. Além disso, a expressão inserida na caixa de texto (fórmula) se ajustará automaticamente a uma expressão equivalente que retira o espaço em branco desnecessário, circunda os nomes das colunas com colchetes (`[]`) e inclui parênteses para mostrar mais explicitamente a ordem das operações:

[!code-sql[Main](working-with-computed-columns-cs/samples/sample2.sql)]

Para obter mais informações sobre colunas computadas no Microsoft SQL Server, consulte a [documentação técnica](https://msdn.microsoft.com/library/ms191250.aspx). Além disso, confira a seção [como especificar as colunas computadas](https://msdn.microsoft.com/library/ms188300.aspx) para um passo a passo de criação de colunas computadas.

> [!NOTE]
> Por padrão, as colunas computadas não são fisicamente armazenadas na tabela, mas, em vez disso, são recalculadas sempre que são referenciadas em uma consulta. Ao marcar a caixa de seleção é persistente, no entanto, você pode instruir SQL Server a armazenar fisicamente a coluna computada na tabela. Isso permite que um índice seja criado na coluna computada, o que pode melhorar o desempenho de consultas que usam o valor da coluna computada em suas cláusulas de `WHERE`. Consulte [criando índices em colunas computadas](https://msdn.microsoft.com/library/ms189292.aspx) para obter mais informações.

## <a name="step-2-viewing-the-computed-column-s-values"></a>Etapa 2: exibindo os valores de s de coluna computada

Antes de começarmos a trabalhar na camada de acesso a dados, deixe-nos levar um minuto para exibir os valores de `FullContactName`. No Gerenciador de Servidores, clique com o botão direito do mouse no nome da tabela de `Suppliers` e escolha nova consulta no menu de contexto. Isso abrirá uma janela de consulta que nos solicitará a escolha quais tabelas serão incluídas na consulta. Adicione a tabela `Suppliers` e clique em fechar. Em seguida, verifique as colunas `CompanyName`, `ContactName`, `ContactTitle`e `FullContactName` da tabela fornecedores. Por fim, clique no ícone de ponto de exclamação vermelho na barra de ferramentas para executar a consulta e exibir os resultados.

Como mostra a Figura 2, os resultados incluem `FullContactName`, que lista as colunas `CompanyName`, `ContactName`e `ContactTitle` usando o formato ldquo;`ContactName` (`ContactTitle`, `CompanyName`).

[![o FullContactName usa o formato ContactName (ContactTitle, CompanyName)](working-with-computed-columns-cs/_static/image5.png)](working-with-computed-columns-cs/_static/image4.png)

**Figura 2**: a `FullContactName` usa o formato `ContactName` (`ContactTitle``CompanyName`) ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image6.png))

## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Etapa 3: Adicionar o`SuppliersTableAdapter`à camada de acesso a dados

Para trabalhar com as informações do fornecedor em nosso aplicativo, precisamos primeiro criar um TableAdapter e DataTable em nossa DAL. O ideal é que isso seja feito usando as mesmas etapas simples examinadas nos tutoriais anteriores. No entanto, trabalhar com colunas computadas apresenta algumas rugas que abordam o mérito.

Se você estiver usando um TableAdapter que usa instruções SQL ad hoc, poderá simplesmente incluir a coluna computada na consulta principal do TableAdapter s por meio do assistente de configuração do TableAdapter. No entanto, isso gerará automaticamente `INSERT` e `UPDATE` instruções que incluem a coluna computada. Se você tentar executar um desses métodos, uma `SqlException` com a mensagem a coluna *ColumnName* não poderá ser modificada porque ela é uma coluna computada ou será o resultado de um operador Union será gerado. Embora a instrução `INSERT` e `UPDATE` possa ser manualmente ajustada por meio das propriedades `InsertCommand` do TableAdapter e `UpdateCommand`, essas personalizações serão perdidas sempre que o assistente de configuração do TableAdapter for executado novamente.

Devido à fragilidade dos TableAdapters que usam instruções SQL ad hoc, é recomendável que possamos usar procedimentos armazenados ao trabalhar com colunas computadas. Se você estiver usando procedimentos armazenados existentes, bastará configurar o TableAdapter conforme discutido no tutorial [usando os procedimentos armazenados existentes para o conjunto de DataSets s tipados](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) . Se você tiver o assistente TableAdapter criar os procedimentos armazenados para você, no entanto, é importante omitir inicialmente todas as colunas computadas da consulta principal. Se você incluir uma coluna computada na consulta principal, o assistente de configuração do TableAdapter informará, após a conclusão, que não poderá criar os procedimentos armazenados correspondentes. Em suma, precisamos configurar inicialmente o TableAdapter usando uma consulta principal computada sem coluna e, em seguida, atualizar manualmente o procedimento armazenado correspondente e o TableAdapter s `SelectCommand` para incluir a coluna computada. Essa abordagem é semelhante à usada no tutorial [atualizando o TableAdapter para usar](updating-the-tableadapter-to-use-joins-cs.md)`JOIN`*s* .

Para este tutorial, vamos adicionar um novo TableAdapter e fazer com que ele crie automaticamente os procedimentos armazenados para nós. Consequentemente, precisaremos omitir inicialmente a `FullContactName` coluna computada da consulta principal.

Comece abrindo o conjunto de `NorthwindWithSprocs` DataSet na pasta `~/App_Code/DAL`. Clique com o botão direito do mouse no designer e, no menu de contexto, escolha Adicionar um novo TableAdapter. Isso iniciará o assistente de configuração do TableAdapter. Especifique o banco de dados do qual será consultada a data (`NORTHWNDConnectionString` de `Web.config`) e clique em Avançar. Como ainda não criamos nenhum procedimento armazenado para consultar ou modificar a tabela de `Suppliers`, selecione a opção criar novos procedimentos armazenados para que o assistente os crie para nós e clique em Avançar.

[![escolher a opção criar novos procedimentos armazenados](working-with-computed-columns-cs/_static/image8.png)](working-with-computed-columns-cs/_static/image7.png)

**Figura 3**: escolha a opção criar novos procedimentos armazenados ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image9.png))

A etapa subsequente solicita a consulta principal. Insira a consulta a seguir, que retorna as colunas `SupplierID`, `CompanyName`, `ContactName`e `ContactTitle` para cada fornecedor. Observe que essa consulta omite propositadamente a coluna computada (`FullContactName`); atualizaremos o procedimento armazenado correspondente para incluir essa coluna na etapa 4.

[!code-sql[Main](working-with-computed-columns-cs/samples/sample3.sql)]

Depois de inserir a consulta principal e clicar em avançar, o assistente nos permite nomear os quatro procedimentos armazenados que serão gerados. Nomeie esses procedimentos armazenados `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`e `Suppliers_Delete`, como ilustra a Figura 4.

[![personalizar os nomes dos procedimentos armazenados gerados automaticamente](working-with-computed-columns-cs/_static/image11.png)](working-with-computed-columns-cs/_static/image10.png)

**Figura 4**: personalizar os nomes dos procedimentos armazenados gerados automaticamente ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image12.png))

A próxima etapa do assistente nos permite nomear os métodos TableAdapter s e especificar os padrões usados para acessar e atualizar os dados. Deixe todas as três caixas de seleção marcadas, mas renomeie o método `GetData` como `GetSuppliers`. Clique em Concluir para concluir o assistente.

[![renomear o método GetData para getsuppliers](working-with-computed-columns-cs/_static/image14.png)](working-with-computed-columns-cs/_static/image13.png)

**Figura 5**: renomear o método `GetData` como `GetSuppliers` ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image15.png))

Ao clicar em concluir, o assistente criará os quatro procedimentos armazenados e adicionará o TableAdapter e a DataTable correspondente ao DataSet tipado.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Etapa 4: incluindo a coluna computada na consulta principal do TableAdapter s

Agora, precisamos atualizar o TableAdapter e DataTable criado na etapa 3 para incluir a `FullContactName` coluna computada. Isso é feito em duas etapas:

1. Atualizando o procedimento armazenado `Suppliers_Select` para retornar a `FullContactName` coluna computada e
2. Atualizando DataTable para incluir uma coluna de `FullContactName` correspondente.

Comece navegando até a Gerenciador de Servidores e fazendo Drill down na pasta procedimentos armazenados. Abra o procedimento armazenado `Suppliers_Select` e atualize a consulta `SELECT` para incluir a `FullContactName` coluna computada:

[!code-sql[Main](working-with-computed-columns-cs/samples/sample4.sql)]

Salve as alterações no procedimento armazenado clicando no ícone salvar na barra de ferramentas, pressionando CTRL + S ou escolhendo a opção salvar `Suppliers_Select` no menu arquivo.

Em seguida, retorne ao DataSet Designer, clique com o botão direito do mouse no `SuppliersTableAdapter`e escolha configurar no menu de contexto. Observe que a coluna `Suppliers_Select` agora inclui a coluna `FullContactName` em sua coleção de colunas de dados.

[![executar o assistente de configuração do TableAdapter s para atualizar as colunas DataTable s](working-with-computed-columns-cs/_static/image17.png)](working-with-computed-columns-cs/_static/image16.png)

**Figura 6**: executar o assistente de configuração do TableAdapter s para atualizar as colunas DataTable s ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image18.png))

Clique em Concluir para concluir o assistente. Isso adicionará automaticamente uma coluna correspondente à `SuppliersDataTable`. O assistente TableAdapter é inteligente o suficiente para detectar que a coluna `FullContactName` é uma coluna computada e, portanto, somente leitura. Consequentemente, ele define a propriedade s `ReadOnly` da coluna como `true`. Para verificar isso, selecione a coluna na `SuppliersDataTable` e, em seguida, vá para a janela Propriedades (veja a Figura 7). Observe que as propriedades `FullContactName` s `DataType` e `MaxLength` de coluna também são definidas de acordo.

[![a coluna FullContactName está marcada como somente leitura](working-with-computed-columns-cs/_static/image20.png)](working-with-computed-columns-cs/_static/image19.png)

**Figura 7**: a coluna `FullContactName` é marcada como somente leitura ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image21.png))

## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Etapa 5: adicionando um método`GetSupplierBySupplierID`ao TableAdapter

Para este tutorial, criaremos uma página ASP.NET que exibe os fornecedores em uma grade atualizável. Nos tutoriais anteriores, atualizamos um único registro da camada de lógica de negócios recuperando esse registro específico da DAL como uma DataTable com rigidez de tipos, atualizando suas propriedades e enviando a DataTable atualizada de volta para a DAL para propagar as alterações para o banco de dados. Para realizar essa primeira etapa – recuperando o registro que está sendo atualizado da DAL-precisamos primeiro adicionar um método `GetSupplierBySupplierID(supplierID)` à DAL.

Clique com o botão direito do mouse na `SuppliersTableAdapter` no design do conjunto de conjuntos e escolha a opção Adicionar consulta no menu de contexto. Como fizemos na etapa 3, deixe que o assistente gere um novo procedimento armazenado para nós selecionando a opção Criar novo procedimento armazenado (consulte a Figura 3 para obter uma captura de tela desta etapa do assistente). Como esse método retornará um registro com várias colunas, indique que queremos usar uma consulta SQL que é uma seleção que retorna linhas e clica em Avançar.

[![escolher a opção Selecionar que retorna linhas](working-with-computed-columns-cs/_static/image23.png)](working-with-computed-columns-cs/_static/image22.png)

**Figura 8**: escolha a opção Selecionar que retorna linhas ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image24.png))

A etapa subsequente solicita que a consulta seja usada para esse método. Insira o seguinte, que retorna os mesmos campos de dados que a consulta principal, mas para um fornecedor específico.

[!code-sql[Main](working-with-computed-columns-cs/samples/sample5.sql)]

A próxima tela nos pede para nomear o procedimento armazenado que será gerado automaticamente. Nomeie esse procedimento armazenado `Suppliers_SelectBySupplierID` e clique em Avançar.

[![nomear o procedimento armazenado Suppliers_SelectBySupplierID](working-with-computed-columns-cs/_static/image26.png)](working-with-computed-columns-cs/_static/image25.png)

**Figura 9**: nomear o procedimento armazenado `Suppliers_SelectBySupplierID` ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image27.png))

Por fim, o assistente nos solicita os padrões de acesso a dados e os nomes de método a serem usados para o TableAdapter. Deixe ambas as caixas de seleção marcadas, mas renomeie os métodos `FillBy` e `GetDataBy` como `FillBySupplierID` e `GetSupplierBySupplierID`, respectivamente.

[![nomear os métodos TableAdapter FillBySupplierID e GetSupplierBySupplierID](working-with-computed-columns-cs/_static/image29.png)](working-with-computed-columns-cs/_static/image28.png)

**Figura 10**: nomear os métodos TableAdapter `FillBySupplierID` e `GetSupplierBySupplierID` ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image30.png))

Clique em Concluir para concluir o assistente.

## <a name="step-6-creating-the-business-logic-layer"></a>Etapa 6: criando a camada de lógica de negócios

Antes de criarmos uma página ASP.NET que usa a coluna computada criada na etapa 1, primeiro precisamos adicionar os métodos correspondentes na BLL. Nossa página ASP.NET, que vamos criar na etapa 7, permitirá que os usuários exibam e editem fornecedores. Portanto, precisamos de nossa BLL para fornecer, no mínimo, um método para obter todos os fornecedores e outro para atualizar um fornecedor específico.

Crie um novo arquivo de classe chamado `SuppliersBLLWithSprocs` na pasta `~/App_Code/BLL` e adicione o seguinte código:

[!code-csharp[Main](working-with-computed-columns-cs/samples/sample6.cs)]

Assim como as outras classes de BLL, `SuppliersBLLWithSprocs` tem uma propriedade `protected` `Adapter` que retorna uma instância da classe `SuppliersTableAdapter` juntamente com dois métodos `public`: `GetSuppliers` e `UpdateSupplier`. O método `GetSuppliers` chama e retorna o `SuppliersDataTable` retornado pelo método `GetSupplier` correspondente na camada de acesso a dados. O método `UpdateSupplier` recupera informações sobre o fornecedor específico que está sendo atualizado por meio de uma chamada para o método de `GetSupplierBySupplierID(supplierID)` DAL. Em seguida, ele atualiza as propriedades `CategoryName`, `ContactName`e `ContactTitle` e confirma essas alterações no banco de dados chamando o método de `Update` da camada de acesso a data, passando o objeto de `SuppliersRow` modificado.

> [!NOTE]
> Exceto para `SupplierID` e `CompanyName`, todas as colunas na tabela Suppliers permitem valores `NULL`. Portanto, se os parâmetros `contactName` ou `contactTitle` passados forem `null` precisaremos definir as propriedades `ContactName` e `ContactTitle` correspondentes como um valor de banco de dados `NULL` usando os métodos `SetContactNameNull` e `SetContactTitleNull`, respectivamente.

## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Etapa 7: trabalhando com a coluna computada da camada de apresentação

Com a coluna computada adicionada à tabela de `Suppliers` e a DAL e a BLL atualizadas de acordo, estamos prontos para criar uma página ASP.NET que funcione com a coluna computada `FullContactName`. Comece abrindo a página `ComputedColumns.aspx` na pasta `AdvancedDAL` e arraste um GridView da caixa de ferramentas para o designer. Defina a Propriedade GridView s `ID` como `Suppliers` e, em sua marca inteligente, associe-a a um novo ObjectDataSource chamado `SuppliersDataSource`. Configure o ObjectDataSource para usar a classe `SuppliersBLLWithSprocs` que adicionamos de volta na etapa 6 e clique em Avançar.

[![configurar o ObjectDataSource para usar a classe SuppliersBLLWithSprocs](working-with-computed-columns-cs/_static/image32.png)](working-with-computed-columns-cs/_static/image31.png)

**Figura 11**: configurar o ObjectDataSource para usar a classe `SuppliersBLLWithSprocs` ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image33.png))

Há apenas dois métodos definidos na classe `SuppliersBLLWithSprocs`: `GetSuppliers` e `UpdateSupplier`. Verifique se esses dois métodos estão especificados nas guias selecionar e atualizar, respectivamente, e clique em concluir para concluir a configuração do ObjectDataSource.

Após a conclusão do assistente de configuração de fonte de dados, o Visual Studio adicionará um BoundField para cada um dos campos de dados retornados. Remova o `SupplierID` BoundField e altere as propriedades de `HeaderText` do `CompanyName`, `ContactName`, `ContactTitle`e `FullContactName` BoundFields para empresa, nome de contato, título e nome de contato completo, respectivamente. Na marca inteligente, marque a caixa de seleção Habilitar edição para ativar os recursos de edição internos do GridView.

Além de adicionar BoundFields ao GridView, a conclusão do assistente de fonte de dados também faz com que o Visual Studio defina a Propriedade ObjectDataSource s `OldValuesParameterFormatString` como original\_{0}. Reverta essa configuração de volta para o valor padrão, {0}.

Depois de fazer essas edições no GridView e no ObjectDataSource, sua marcação declarativa deve ser semelhante ao seguinte:

[!code-aspx[Main](working-with-computed-columns-cs/samples/sample7.aspx)]

Em seguida, visite esta página por meio de um navegador. Como mostra a Figura 12, cada fornecedor é listado em uma grade que inclui a coluna `FullContactName`, cujo valor é simplesmente a concatenação das outras três colunas formatadas como `ContactName` (`ContactTitle``CompanyName`).

[![cada fornecedor está listado na grade](working-with-computed-columns-cs/_static/image35.png)](working-with-computed-columns-cs/_static/image34.png)

**Figura 12**: cada fornecedor é listado na grade ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image36.png))

Clicar no botão Editar de um fornecedor específico causa um postback e tem essa linha renderizada em sua interface de edição (consulte a Figura 13). As três primeiras colunas são renderizadas em sua interface de edição padrão – um controle TextBox cuja propriedade `Text` é definida como o valor do campo de dados. No entanto, a coluna `FullContactName` permanece como texto. Quando os BoundFields foram adicionados ao GridView na conclusão do assistente de configuração da fonte de dados, a propriedade `FullContactName` BoundField s `ReadOnly` foi definida como `true` porque a coluna `FullContactName` correspondente na `SuppliersDataTable` tem sua propriedade `ReadOnly` definida como `true`. Conforme observado na etapa 4, a propriedade `FullContactName` s `ReadOnly` foi definida como `true` porque o TableAdapter detectou que a coluna era uma coluna computada.

[![a coluna FullContactName não é editável](working-with-computed-columns-cs/_static/image38.png)](working-with-computed-columns-cs/_static/image37.png)

**Figura 13**: a coluna `FullContactName` não é editável ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image39.png))

Vá em frente e atualize o valor de uma ou mais das colunas editáveis e clique em atualizar. Observe como o valor de `FullContactName` s é atualizado automaticamente para refletir a alteração.

> [!NOTE]
> O GridView atualmente usa BoundFields para os campos editáveis, resultando na interface de edição padrão. Como o campo `CompanyName` é necessário, ele deve ser convertido em um TemplateField que inclui um RequiredFieldValidator. Deixe isso como um exercício para o leitor interessado. Consulte o tutorial [adicionando controles de validação para as interfaces de edição e inserção](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) para obter instruções passo a passo sobre como converter um BoundField em um TemplateField e adicionar controles de validação.

## <a name="summary"></a>Resumo

Ao definir o esquema para uma tabela, Microsoft SQL Server permite a inclusão de colunas computadas. Essas são as colunas cujos valores são calculados a partir de uma expressão que geralmente referencia os valores de outras colunas no mesmo registro. Como os valores das colunas computadas se baseiam em uma expressão, eles são somente leitura e não podem ser atribuídos a um valor em uma instrução `INSERT` ou `UPDATE`. Isso apresenta desafios ao usar uma coluna computada na consulta principal de um TableAdapter que tenta gerar automaticamente instruções `INSERT`, `UPDATE`e `DELETE` correspondentes.

Neste tutorial, discutimos técnicas para burlar os desafios apresentados por colunas computadas. Em particular, usamos procedimentos armazenados em nosso TableAdapter para superar a fragilidade inerente em TableAdapters que usam instruções SQL ad hoc. Quando o assistente TableAdapter cria novos procedimentos armazenados, é importante que tenhamos a consulta principal omitir inicialmente todas as colunas computadas porque sua presença impede que os procedimentos armazenados de modificação de dados sejam gerados. Depois que o TableAdapter tiver sido inicialmente configurado, seu procedimento armazenado `SelectCommand` pode ser reinicializado para incluir colunas computadas.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Hilton Geisenow e Teresa Murphy. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](adding-additional-datatable-columns-cs.md)
> [Próximo](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
