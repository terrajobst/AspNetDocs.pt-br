---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
title: Atualizando o TableAdapter para usar junções (VB) | Microsoft Docs
author: rick-anderson
description: Ao trabalhar com um banco de dados, é comum a solicitação de dado que é distribuído por várias tabelas. Para recuperar dados de duas tabelas diferentes, podemos usar...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: e624a3e0-061b-4efc-8b0e-5877f9ff6714
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
msc.type: authoredcontent
ms.openlocfilehash: 5c94baa99b126cdd24d69afc3d02bfe8b069419b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78532266"
---
# <a name="updating-the-tableadapter-to-use-joins-vb"></a>Atualizar o TableAdapter para usar JOINs (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_VB.zip) ou [baixar PDF](updating-the-tableadapter-to-use-joins-vb/_static/datatutorial69vb1.pdf)

> Ao trabalhar com um banco de dados, é comum a solicitação de dado que é distribuído por várias tabelas. Para recuperar dados de duas tabelas diferentes, podemos usar uma subconsulta correlacionada ou uma operação de junção. Neste tutorial, comparamos Subconsultas correlacionadas e a sintaxe de junção antes de examinar como criar um TableAdapter que inclui uma junção em sua consulta principal.

## <a name="introduction"></a>Introdução

Com bancos de dados relacionais, com os quais estamos interessados em trabalhar, muitas vezes se espalham em várias tabelas. Por exemplo, ao exibir as informações do produto, provavelmente, queremos listar os nomes de categoria e fornecedor correspondentes de cada produto. A tabela `Products` tem valores de `CategoryID` e `SupplierID`, mas os nomes reais de categoria e fornecedor estão nas tabelas `Categories` e `Suppliers`, respectivamente.

Para recuperar informações de outra tabela relacionada, podemos usar *Subconsultas correlacionadas* ou `JOIN`*s*. Uma subconsulta correlacionada é uma consulta `SELECT` aninhada que faz referência a colunas na consulta externa. Por exemplo, no tutorial [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-vb.md) , usamos duas Subconsultas correlacionadas na consulta principal `ProductsTableAdapter` s para retornar os nomes da categoria e do fornecedor para cada produto. Uma `JOIN` é uma construção SQL que mescla linhas relacionadas de duas tabelas diferentes. Usamos um `JOIN` nos dados de [consulta com o tutorial de controle SqlDataSource](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb.md) para exibir informações de categoria junto com cada produto.

O motivo pelo qual temos abstained de usar `JOIN` s com os TableAdapters é devido a limitações no assistente do TableAdapter s para gerar automaticamente as instruções `INSERT`, `UPDATE`e `DELETE` correspondentes. Mais especificamente, se a consulta principal do TableAdapter s contiver qualquer `JOIN` s, o TableAdapter não poderá criar automaticamente as instruções SQL ad hoc ou procedimentos armazenados para suas propriedades `InsertCommand`, `UpdateCommand`e `DeleteCommand`.

Neste tutorial, vamos comparar brevemente e contrastars com Subconsultas correlacionadas e `JOIN` s antes de explorar como criar um TableAdapter que inclua `JOIN` s em sua consulta principal.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Comparando e contrastanting Subconsultas correlacionadas e`JOIN` s

Lembre-se de que o `ProductsTableAdapter` criado no primeiro tutorial no conjunto de `Northwind` DataSet usa Subconsultas correlacionadas para trazer de volta cada categoria e nome de fornecedor correspondentes. A consulta principal `ProductsTableAdapter` s é mostrada abaixo.

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample1.sql)]

As duas Subconsultas correlacionadas-`(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` e `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)`-são consultas `SELECT` que retornam um único valor por produto como uma coluna adicional na lista de colunas da instrução de `SELECT` externa.

Como alternativa, um `JOIN` pode ser usado para retornar cada nome de fornecedor e categoria do produto. A consulta a seguir retorna a mesma saída que a acima, mas usa `JOIN` s no lugar de subconsultas:

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample2.sql)]

Uma `JOIN` mescla os registros de uma tabela com registros de outra tabela com base em alguns critérios. Na consulta acima, por exemplo, o `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` instrui SQL Server a Mesclar cada registro de produto com o registro de categoria cujo valor de `CategoryID` corresponde ao valor de `CategoryID` do produto. O resultado mesclado nos permite trabalhar com os campos de categoria correspondentes para cada produto (como `CategoryName`).

> [!NOTE]
> os `JOIN` s são comumente usados ao consultar dados de bancos de dados relacionais. Se você for novo na sintaxe do `JOIN` ou precisar criar um pouco sobre seu uso, eu d recomendo o tutorial de [junção do SQL](http://www.w3schools.com/sql/sql_join.asp) em [escolas da w3](http://www.w3schools.com/). Também vale [`JOIN`](https://msdn.microsoft.com/library/ms191517.aspx) a pena ler as seções de [conceitos básicos de subconsultas](https://msdn.microsoft.com/library/ms189575.aspx) e conceitos dos [manuais online do SQL](https://msdn.microsoft.com/library/ms130214.aspx).

Como as subconsultas `JOIN` s e correlacionadas podem ser usadas para recuperar dados relacionados de outras tabelas, muitos desenvolvedores estão à esquerda e se perguntam qual abordagem usar. Todos os especialistas em SQL que falei para falaram aproximadamente da mesma coisa, que ele não importa muito em termos de desempenho, pois SQL Server produzirão planos de execução aproximadamente idênticos. Seu Conselho, então, é usar a técnica com a qual você e sua equipe estão mais confortáveis. Ele merece observar que, depois de fazer parte desse Conselho, esses especialistas expressam imediatamente sua preferência de `JOIN` s sobre Subconsultas correlacionadas.

Ao criar uma camada de acesso a dados usando datasets tipados, as ferramentas funcionam melhor ao usar subconsultas. Em particular, o assistente do TableAdapter s não gerará automaticamente instruções `INSERT`, `UPDATE`e `DELETE` correspondentes se a consulta principal contiver quaisquer `JOIN` s, mas irá gerar automaticamente essas instruções quando Subconsultas correlacionadas forem usadas.

Para explorar essa deficiência, crie um DataSet tipado temporário na pasta `~/App_Code/DAL`. Durante o assistente de configuração do TableAdapter, escolha usar instruções SQL ad hoc e insira a seguinte consulta de `SELECT` (consulte a Figura 1):

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample3.sql)]

[![inserir uma consulta principal que contém junções](updating-the-tableadapter-to-use-joins-vb/_static/image2.png)](updating-the-tableadapter-to-use-joins-vb/_static/image1.png)

**Figura 1**: Insira uma consulta principal que contenha `JOIN` s ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image3.png))

Por padrão, o TableAdapter criará automaticamente `INSERT`, `UPDATE`e `DELETE` instruções com base na consulta principal. Se você clicar no botão Avançado, poderá ver que esse recurso está habilitado. Apesar dessa configuração, o TableAdapter não será capaz de criar as instruções `INSERT`, `UPDATE`e `DELETE` porque a consulta principal contém uma `JOIN`.

![Insira uma consulta principal que contém junções](updating-the-tableadapter-to-use-joins-vb/_static/image4.png)

**Figura 2**: inserir uma consulta principal que contém `JOIN` s

Clique em Concluir para finalizar o assistente. Neste ponto, o designer do seu conjunto de seus DataSet incluirá um único TableAdapter com uma DataTable com colunas para cada um dos campos retornados na lista de colunas da consulta `SELECT`. Isso inclui o `CategoryName` e `SupplierName`, como mostra a Figura 3.

![A DataTable inclui uma coluna para cada campo retornado na lista de colunas](updating-the-tableadapter-to-use-joins-vb/_static/image5.png)

**Figura 3**: a DataTable inclui uma coluna para cada campo retornado na lista de colunas

Embora a DataTable tenha as colunas apropriadas, o TableAdapter não tem valores para suas propriedades `InsertCommand`, `UpdateCommand`e `DeleteCommand`. Para confirmar isso, clique no TableAdapter no designer e, em seguida, vá para a janela Propriedades. Lá, você verá que as propriedades `InsertCommand`, `UpdateCommand`e `DeleteCommand` são definidas como (nenhum).

[![as propriedades InsertCommand, UpdateCommand e DeleteCommand são definidas como (None)](updating-the-tableadapter-to-use-joins-vb/_static/image7.png)](updating-the-tableadapter-to-use-joins-vb/_static/image6.png)

**Figura 4**: as propriedades `InsertCommand`, `UpdateCommand`e `DeleteCommand` são definidas como (nenhum) ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image8.png))

Para contornar essa deficiência, podemos fornecer manualmente as instruções SQL e os parâmetros para as propriedades `InsertCommand`, `UpdateCommand`e `DeleteCommand` por meio do janela Propriedades. Como alternativa, poderíamos começar Configurando a consulta principal do TableAdapter s para *não* incluir nenhum `JOIN` s. Isso permitirá que as instruções `INSERT`, `UPDATE`e `DELETE` sejam geradas automaticamente para nós. Depois de concluir o assistente, poderíamos atualizar manualmente o `SelectCommand` do TableAdapter s a partir do janela Propriedades para que ele inclua a sintaxe `JOIN`.

Embora essa abordagem funcione, é muito frágil ao usar consultas SQL ad hoc, pois sempre que a consulta principal do TableAdapter s é reconfigurada por meio do assistente, as instruções `INSERT`geradas automaticamente, `UPDATE`e `DELETE` são recriadas. Isso significa que todas as personalizações feitas posteriormente seriam perdidas se clicarmos com o botão direito do mouse no TableAdapter, escolhemos configurar no menu de contexto e concluimos o assistente novamente.

A fragilidade das instruções `INSERT`das geradas automaticamente pelo TableAdapter s, `UPDATE`e `DELETE` é, felizmente, limitada a instruções SQL ad hoc. Se o seu TableAdapter usar procedimentos armazenados, você poderá personalizar os `SelectCommand`, `InsertCommand`, `UpdateCommand`ou `DeleteCommand` procedimentos armazenados e executar novamente o assistente de configuração do TableAdapter sem ter que temer que os procedimentos armazenados serão modificados.

Nas próximas etapas, criaremos um TableAdapter que, inicialmente, usa uma consulta principal que omite qualquer `JOIN` s para que os procedimentos armazenados de inserção, atualização e exclusão correspondentes sejam gerados automaticamente. Em seguida, atualizaremos o `SelectCommand` para que use um `JOIN` que retorne colunas adicionais de tabelas relacionadas. Por fim, criaremos uma classe de camada de lógica de negócios correspondente e demonstraremos o uso do TableAdapter em uma página da Web ASP.NET.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Etapa 1: criando o TableAdapter usando uma consulta principal simplificada

Para este tutorial, adicionaremos um TableAdapter e uma DataTable fortemente tipada para a tabela `Employees` no conjunto de `NorthwindWithSprocs` DataSet. A tabela de `Employees` contém um campo de `ReportsTo` que especificou a `EmployeeID` do gerente do funcionário s. Por exemplo, o funcionário Anne Dodsworth tem um valor `ReportTo` de 5, que é o `EmployeeID` de Steven Buchanan. Consequentemente, Anne relata para Steven, seu gerente. Juntamente com o relatório de `ReportsTo` valor de cada funcionário, talvez também queiramos recuperar o nome do seu gerente. Isso pode ser feito usando uma `JOIN`. Mas usar um `JOIN` ao criar inicialmente o TableAdapter impede que o assistente gere automaticamente os recursos correspondentes de inserção, atualização e exclusão. Portanto, começaremos criando um TableAdapter cuja consulta principal não contém nenhum `JOIN` s. Em seguida, na etapa 2, atualizaremos o procedimento armazenado de consulta principal para recuperar o nome do Gerenciador por meio de um `JOIN`.

Comece abrindo o conjunto de `NorthwindWithSprocs` DataSet na pasta `~/App_Code/DAL`. Clique com o botão direito do mouse no designer, selecione a opção Adicionar no menu de contexto e selecione o item de menu TableAdapter. Isso iniciará o assistente de configuração do TableAdapter. Como mostra a Figura 5, faça com que o assistente crie novos procedimentos armazenados e clique em Avançar. Para obter um atualizador sobre a criação de novos procedimentos armazenados a partir do assistente do TableAdapter s, consulte o tutorial [criando novos procedimentos armazenados para o DataSets do tipo s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) .

[![selecionar a opção criar novos procedimentos armazenados](updating-the-tableadapter-to-use-joins-vb/_static/image10.png)](updating-the-tableadapter-to-use-joins-vb/_static/image9.png)

**Figura 5**: selecione a opção criar novos procedimentos armazenados ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image11.png))

Use a seguinte instrução de `SELECT` para a consulta principal do TableAdapter s:

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample4.sql)]

Como essa consulta não inclui nenhuma `JOIN` s, o assistente TableAdapter criará automaticamente procedimentos armazenados com as instruções `INSERT`, `UPDATE`e `DELETE` correspondentes, bem como um procedimento armazenado para executar a consulta principal.

A etapa a seguir nos permite nomear os procedimentos armazenados do TableAdapter. Use os nomes `Employees_Select`, `Employees_Insert`, `Employees_Update`e `Employees_Delete`, como mostra a Figura 6.

[![nomear os procedimentos armazenados do TableAdapter s](updating-the-tableadapter-to-use-joins-vb/_static/image13.png)](updating-the-tableadapter-to-use-joins-vb/_static/image12.png)

**Figura 6**: nomear os procedimentos armazenados do TableAdapter s ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image14.png))

A etapa final solicita que nomeiem os métodos TableAdapter s. Use `Fill` e `GetEmployees` como os nomes de método. Lembre-se também de deixar a caixa de seleção criar métodos para enviar atualizações diretamente para o banco de dados (GenerateDBDirectMethods) marcada.

[![nomear os métodos TableAdapter s Fill e GetEmployees](updating-the-tableadapter-to-use-joins-vb/_static/image16.png)](updating-the-tableadapter-to-use-joins-vb/_static/image15.png)

**Figura 7**: nomear os métodos TableAdapter s `Fill` e `GetEmployees` ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image17.png))

Depois de concluir o assistente, reserve alguns instantes para examinar os procedimentos armazenados no banco de dados. Você deve ver quatro novos: `Employees_Select`, `Employees_Insert`, `Employees_Update`e `Employees_Delete`. Em seguida, inspecione o `EmployeesDataTable` e `EmployeesTableAdapter` recém-criado. A DataTable contém uma coluna para cada campo retornado pela consulta principal. Clique no TableAdapter e, em seguida, vá para a janela Propriedades. Lá, você verá que as propriedades `InsertCommand`, `UpdateCommand`e `DeleteCommand` estão configuradas corretamente para chamar os procedimentos armazenados correspondentes.

[![o TableAdapter inclui recursos de inserção, atualização e exclusão](updating-the-tableadapter-to-use-joins-vb/_static/image19.png)](updating-the-tableadapter-to-use-joins-vb/_static/image18.png)

**Figura 8**: o TableAdapter inclui recursos de inserção, atualização e exclusão ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image20.png))

Com os procedimentos armazenados INSERT, Update e Delete criados automaticamente e as propriedades `InsertCommand`, `UpdateCommand`e `DeleteCommand` configuradas corretamente, estamos prontos para personalizar o procedimento armazenado `SelectCommand` s para retornar informações adicionais sobre cada funcionário s Manager. Especificamente, precisamos atualizar o `Employees_Select` procedimento armazenado para usar um `JOIN` e retornar os valores de `FirstName` e `LastName` do Manager s. Depois que o procedimento armazenado tiver sido atualizado, precisaremos atualizar a DataTable para que ela inclua essas colunas adicionais. Iremos lidar com essas duas tarefas nas etapas 2 e 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Etapa 2: Personalizando o procedimento armazenado para incluir um`JOIN`

Comece indo para a Gerenciador de Servidores, fazendo Drill down na pasta Northwind Database s stored procedures e abrindo o procedimento armazenado `Employees_Select`. Se você não vir esse procedimento armazenado, clique com o botão direito do mouse na pasta procedimentos armazenados e escolha Atualizar. Atualize o procedimento armazenado para que ele use um `LEFT JOIN` para retornar o nome e o sobrenome do Gerenciador:

[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample5.sql)]

Depois de atualizar a instrução `SELECT`, salve as alterações Acessando o menu arquivo e escolhendo salvar `Employees_Select`. Como alternativa, você pode clicar no ícone salvar na barra de ferramentas ou pressionar CTRL + S. Depois de salvar as alterações, clique com o botão direito do mouse no procedimento armazenado `Employees_Select` no Gerenciador de Servidores e escolha Executar. Isso executará o procedimento armazenado e mostrará seus resultados na janela de saída (consulte a Figura 9).

[![os resultados dos procedimentos armazenados são exibidos no Janela de Saída](updating-the-tableadapter-to-use-joins-vb/_static/image22.png)](updating-the-tableadapter-to-use-joins-vb/_static/image21.png)

**Figura 9**: os resultados dos procedimentos armazenados são exibidos na janela de saída ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image23.png))

## <a name="step-3-updating-the-datatable-s-columns"></a>Etapa 3: atualizando as colunas DataTable s

Neste ponto, o procedimento armazenado `Employees_Select` retorna `ManagerFirstName` e `ManagerLastName` valores, mas a `EmployeesDataTable` está perdendo essas colunas. Essas colunas ausentes podem ser adicionadas à DataTable de uma das duas maneiras:

- Clique **manualmente** com o botão direito do mouse na DataTable no designer de conjunto de e, no menu Adicionar, escolha coluna. Em seguida, você pode nomear a coluna e definir suas propriedades de acordo.
- **Automaticamente** -o assistente de configuração do TableAdapter atualizará as colunas DataTable s para refletir os campos retornados pelo procedimento armazenado `SelectCommand`. Ao usar instruções SQL ad hoc, o assistente também removerá as propriedades `InsertCommand`, `UpdateCommand`e `DeleteCommand`, já que a `SelectCommand` agora contém uma `JOIN`. Mas, ao usar procedimentos armazenados, essas propriedades de comando permanecem intactas.

Exploramos manualmente a adição de colunas DataTable nos tutoriais anteriores, incluindo [mestre/detalhes, usando uma lista com marcadores de registros mestres com detalhes DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) e [carregando arquivos](../working-with-binary-files/uploading-files-vb.md), e veremos esse processo novamente mais detalhadamente em nosso próximo tutorial. Para este tutorial, no entanto, vamos usar a abordagem automática por meio do assistente de configuração do TableAdapter.

Comece clicando com o botão direito do mouse na `EmployeesTableAdapter` e selecionando configurar no menu de contexto. Isso abre o assistente de configuração do TableAdapter, que lista os procedimentos armazenados usados para selecionar, inserir, atualizar e excluir, juntamente com seus valores de retorno e parâmetros (se houver). A Figura 10 mostra esse assistente. Aqui, podemos ver que o procedimento armazenado `Employees_Select` agora retorna os campos `ManagerFirstName` e `ManagerLastName`.

[![o assistente mostra a lista de colunas atualizada para o procedimento armazenado Employees_Select](updating-the-tableadapter-to-use-joins-vb/_static/image25.png)](updating-the-tableadapter-to-use-joins-vb/_static/image24.png)

**Figura 10**: o assistente mostra a lista de colunas atualizada para o procedimento armazenado `Employees_Select` ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image26.png))

Conclua o assistente clicando em concluir. Ao retornar ao DataSet Designer, o `EmployeesDataTable` inclui duas colunas adicionais: `ManagerFirstName` e `ManagerLastName`.

[![o EmployeesDataTable contém duas novas colunas](updating-the-tableadapter-to-use-joins-vb/_static/image28.png)](updating-the-tableadapter-to-use-joins-vb/_static/image27.png)

**Figura 11**: a `EmployeesDataTable` contém duas novas colunas ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image29.png))

Para ilustrar que o procedimento armazenado `Employees_Select` atualizado está em vigor e que os recursos de inserção, atualização e exclusão do TableAdapter ainda estão funcionais, vamos criar uma página da Web que permita aos usuários exibir e excluir funcionários. Antes de criarmos essa página, no entanto, precisamos primeiro criar uma nova classe na camada de lógica de negócios para trabalhar com os funcionários do conjunto de `NorthwindWithSprocs` DataSet. Na etapa 4, criaremos uma classe de `EmployeesBLLWithSprocs`. Na etapa 5, usaremos essa classe em uma página do ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Etapa 4: implementando a camada de lógica de negócios

Crie um novo arquivo de classe na pasta `~/App_Code/BLL` chamada `EmployeesBLLWithSprocs.vb`. Essa classe imita a semântica da classe de `EmployeesBLL` existente, somente essa nova fornece menos métodos e usa o conjunto de `NorthwindWithSprocs` DataSet (em vez do conjunto de `Northwind` DataSet). Adicione o código a seguir à classe `EmployeesBLLWithSprocs` .

[!code-vb[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample6.vb)]

A propriedade `EmployeesBLLWithSprocs` s `Adapter` de classe retorna uma instância do `EmployeesTableAdapter`de `NorthwindWithSprocs` do conjunto de os s. Isso é usado pelos métodos de classe s `GetEmployees` e `DeleteEmployee`. O método `GetEmployees` chama o método `GetEmployees` `EmployeesTableAdapter` correspondente, que invoca o procedimento armazenado `Employees_Select` e popula seus resultados em um `EmployeeDataTable`. O método `DeleteEmployee`, de forma semelhante, chama o método `EmployeesTableAdapter` s `Delete`, que invoca o procedimento armazenado `Employees_Delete`.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Etapa 5: trabalhando com os dados na camada de apresentação

Com a classe `EmployeesBLLWithSprocs` concluída, estamos prontos para trabalhar com dados de funcionários por meio de uma página ASP.NET. Abra a página `JOINs.aspx` na pasta `AdvancedDAL` e arraste um GridView da caixa de ferramentas para o designer, definindo sua propriedade `ID` como `Employees`. Em seguida, na marca inteligente do GridView, associe a grade a um novo controle ObjectDataSource chamado `EmployeesDataSource`.

Configure o ObjectDataSource para usar a classe `EmployeesBLLWithSprocs` e, nas guias SELECT e DELETE, verifique se os métodos `GetEmployees` e `DeleteEmployee` estão selecionados nas listas suspensas. Clique em concluir para concluir a configuração de ObjectDataSource s.

[![configurar o ObjectDataSource para usar a classe EmployeesBLLWithSprocs](updating-the-tableadapter-to-use-joins-vb/_static/image31.png)](updating-the-tableadapter-to-use-joins-vb/_static/image30.png)

**Figura 12**: configurar o ObjectDataSource para usar a classe `EmployeesBLLWithSprocs` ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image32.png))

[![fazer com que o ObjectDataSource use os métodos GetEmployees e DeleteEmployee](updating-the-tableadapter-to-use-joins-vb/_static/image34.png)](updating-the-tableadapter-to-use-joins-vb/_static/image33.png)

**Figura 13**: fazer com que o ObjectDataSource use os métodos `GetEmployees` e `DeleteEmployee` ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image35.png))

O Visual Studio adicionará um BoundField ao GridView para cada uma das colunas `EmployeesDataTable` s. Remova todos esses BoundFields, exceto `Title`, `LastName`, `FirstName`, `ManagerFirstName`e `ManagerLastName` e renomeie as propriedades `HeaderText` para os últimos quatro BoundFields para sobrenome, nome, nome do gerente e sobrenome do gerente, respectivamente.

Para permitir que os usuários excluam os funcionários desta página, precisamos fazer duas coisas. Primeiro, instrua o GridView a fornecer recursos de exclusão marcando a opção Habilitar exclusão em sua marca inteligente. Em segundo lugar, altere a Propriedade ObjectDataSource s `OldValuesParameterFormatString` do valor definido pelo assistente ObjectDataSource (`original_{0}`) para seu valor padrão (`{0}`). Depois de fazer essas alterações, a marcação declarativa de GridView e ObjectDataSource s deve ser semelhante ao seguinte:

[!code-aspx[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample7.aspx)]

Teste a página visitando-a por meio de um navegador. Como mostra a Figura 14, a página listará cada funcionário e seu nome de gerente (supondo que eles tenham um).

[![a junção no procedimento armazenado Employees_Select retorna o nome do gerente](updating-the-tableadapter-to-use-joins-vb/_static/image37.png)](updating-the-tableadapter-to-use-joins-vb/_static/image36.png)

**Figura 14**: a `JOIN` no procedimento armazenado `Employees_Select` retorna o nome do gerente ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image38.png))

Clicar no botão excluir inicia o fluxo de trabalho de exclusão, que culmina na execução do procedimento armazenado `Employees_Delete`. No entanto, a instrução tentada `DELETE` no procedimento armazenado falha devido a uma violação de restrição de chave estrangeira (consulte a Figura 15). Especificamente, cada funcionário tem um ou mais registros na tabela `Orders`, fazendo com que a exclusão falhe.

[![excluir um funcionário que tem pedidos correspondentes resulta em uma violação de restrição de chave estrangeira](updating-the-tableadapter-to-use-joins-vb/_static/image40.png)](updating-the-tableadapter-to-use-joins-vb/_static/image39.png)

**Figura 15**: a exclusão de um funcionário que tem pedidos correspondentes resulta em uma violação de restrição de chave estrangeira ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image41.png))

Para permitir que um funcionário seja excluído, você poderia:

- Atualizar a restrição FOREIGN KEY para as exclusões em cascata,
- Exclua manualmente os registros da tabela `Orders` para os funcionários que você deseja excluir ou
- Atualize o procedimento armazenado `Employees_Delete` para primeiro excluir os registros relacionados da tabela `Orders` antes de excluir o registro `Employees`. Discutimos essa técnica no tutorial [usando os procedimentos armazenados existentes para os TableAdapters do DataSet s tipados](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) .

Eu deixe isso como um exercício para o leitor.

## <a name="summary"></a>Resumo

Ao trabalhar com bancos de dados relacionais, é comum que as consultas recebam seus dados de várias tabelas relacionadas. Subconsultas correlacionadas e `JOIN` s fornecem duas técnicas diferentes para acessar dados de tabelas relacionadas em uma consulta. Nos tutoriais anteriores, mais comumente usamos as Subconsultas correlacionadas porque o TableAdapter não pode gerar automaticamente instruções `INSERT`, `UPDATE`e `DELETE` para consultas que envolvem `JOIN` s. Embora esses valores possam ser fornecidos manualmente, ao usar instruções SQL ad hoc, todas as personalizações serão substituídas quando o assistente de configuração do TableAdapter for concluído.

Felizmente, os TableAdapters criados usando procedimentos armazenados não sofrem com a mesma fragilidade que aquelas criadas usando instruções SQL ad hoc. Portanto, é viável criar um TableAdapter cuja consulta principal usa um `JOIN` ao usar procedimentos armazenados. Neste tutorial, vimos como criar um TableAdapter desse tipo. Começamos usando uma consulta `SELECT` sem `JOIN`para a consulta principal do TableAdapter s para que os procedimentos armazenados de inserção, atualização e exclusão correspondentes sejam criados automaticamente. Com a configuração inicial do TableAdapter s concluída, aumentamos o `SelectCommand` procedimento armazenado para usar um `JOIN` e executar novamente o assistente de configuração do TableAdapter para atualizar as colunas do `EmployeesDataTable` s.

Executar novamente o assistente de configuração do TableAdapter atualizou automaticamente as colunas de `EmployeesDataTable` para refletir os campos de dados retornados pelo procedimento armazenado `Employees_Select`. Como alternativa, poderíamos ter adicionado manualmente essas colunas à DataTable. Exploraremos manualmente a adição de colunas à DataTable no próximo tutorial.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Hilton Geisenow, David Suru e Teresa Murphy. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Próximo](adding-additional-datatable-columns-vb.md)
