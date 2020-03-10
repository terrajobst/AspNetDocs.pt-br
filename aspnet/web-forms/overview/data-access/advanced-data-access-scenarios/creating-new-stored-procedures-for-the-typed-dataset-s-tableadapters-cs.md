---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: Criando novos procedimentos armazenados para os TableAdapters do DataSet tipadosC#() | Microsoft Docs
author: rick-anderson
description: Nos tutoriais anteriores, criamos instruções SQL em nosso código e passamos as instruções para o banco de dados a ser executado. Uma abordagem alternativa é usar s...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 751282ca-5870-4d66-84e4-6cefae23eb4a
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: db0d83a0fd1f1f175001d20844b298be0cf7e1cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78533708"
---
# <a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Criar novos procedimentos armazenados para os TableAdapters do conjunto de dados tipado (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_CS.zip) ou [baixar PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial67cs1.pdf)

> Nos tutoriais anteriores, criamos instruções SQL em nosso código e passamos as instruções para o banco de dados a ser executado. Uma abordagem alternativa é usar procedimentos armazenados, em que as instruções SQL são predefinidas no banco de dados. Neste tutorial, aprendemos como fazer com que o assistente do TableAdapter gere novos procedimentos armazenados para nós.

## <a name="introduction"></a>Introdução

A DAL (camada de acesso a dados) para esses tutoriais usa datasets tipados. Conforme discutido no tutorial [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md) , os conjuntos de dados tipados consistem em DataTables e TableAdapters fortemente tipados. As tabelas de dados representam as entidades lógicas no sistema, enquanto a interface do TableAdapters com o banco de dados subjacente para executar o trabalho de acesso ao dado. Isso inclui popular as tabelas de dados, executar consultas que retornam dados escalares e inserir, atualizar e excluir registros do banco de dado.

Os comandos SQL executados pelos TableAdapters podem ser instruções SQL ad hoc, como `SELECT columnList FROM TableName`ou procedimentos armazenados. Os TableAdapters em nossa arquitetura usam instruções SQL ad hoc. Muitos desenvolvedores e administradores de banco de dados, no entanto, preferem procedimentos armazenados em instruções SQL ad hoc para fins de segurança, capacidade de manutenção e capacidade de atualização. Outros ardently preferem instruções SQL ad hoc para sua flexibilidade. Em meu próprio trabalho, prefiro procedimentos armazenados em instruções SQL ad hoc, mas optamos por usar instruções SQL ad hoc para simplificar os tutoriais anteriores.

Ao definir um TableAdapter ou adicionar novos métodos, o assistente do TableAdapter s facilita a criação de novos procedimentos armazenados ou o uso de procedimentos armazenados existentes, como faz para usar instruções SQL ad hoc. Neste tutorial, examinaremos como fazer com que o assistente do TableAdapter s gere procedimentos armazenados automaticamente. No próximo tutorial, veremos como configurar os métodos TableAdapter s para usar procedimentos armazenados existentes ou criados manualmente.

> [!NOTE]
> Veja que a entrada de blog de Rob Howard [ainda não usa procedimentos armazenados?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) e os procedimentos armazenados de entrada de blog [Frans Bouma](https://weblogs.asp.net/fbouma/) s [são ruins, M Kay?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) para um debate de debates sobre os prós e contras de procedimentos armazenados e SQL ad hoc.

## <a name="stored-procedure-basics"></a>Conceitos básicos de procedimento armazenado

As funções são uma construção comum a todas as linguagens de programação. Uma função é uma coleção de instruções que são executadas quando a função é chamada. As funções podem aceitar parâmetros de entrada e, opcionalmente, podem retornar um valor. Os *[procedimentos armazenados](http://en.wikipedia.org/wiki/Stored_procedure)* são construções de banco de dados que compartilham muitas semelhanças com funções em linguagens de programação. Um procedimento armazenado é composto de um conjunto de instruções T-SQL que são executadas quando o procedimento armazenado é chamado. Um procedimento armazenado pode aceitar zero para muitos parâmetros de entrada e pode retornar valores escalares, parâmetros de saída ou, mais comumente, conjuntos de resultados de `SELECT` consultas.

> [!NOTE]
> Procedimentos armazenados são, muitas vezes, chamados de sprocs ou SPs.

Os procedimentos armazenados são criados usando a instrução t-SQL [`CREATE PROCEDURE`](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) . Por exemplo, o script T-SQL a seguir cria um procedimento armazenado chamado `GetProductsByCategoryID` que usa um único parâmetro chamado `@CategoryID` e retorna os campos `ProductID`, `ProductName`, `UnitPrice`e `Discontinued` dessas colunas na tabela `Products` que têm um valor de `CategoryID` correspondente:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Depois que esse procedimento armazenado tiver sido criado, ele poderá ser chamado usando a seguinte sintaxe:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.sql)]

> [!NOTE]
> No próximo tutorial, examinaremos a criação de procedimentos armazenados por meio do IDE do Visual Studio. Para este tutorial, no entanto, vamos permitir que o assistente do TableAdapter gere automaticamente os procedimentos armazenados para nós.

Além de simplesmente retornar dados, os procedimentos armazenados costumam ser usados para executar vários comandos de banco dentro do escopo de uma única transação. Um procedimento armazenado chamado `DeleteCategory`, por exemplo, pode levar em um parâmetro `@CategoryID` e executar duas instruções `DELETE`: primeiro, uma para excluir os produtos relacionados e um segundo excluindo a categoria especificada. Várias instruções em um procedimento armazenado *não* são encapsuladas automaticamente em uma transação. Comandos T-SQL adicionais precisam ser emitidos para garantir que os s vários comandos do procedimento armazenado sejam tratados como uma operação atômica. Veremos como encapsular os comandos s de um procedimento armazenado dentro do escopo de uma transação no tutorial subsequente.

Ao usar procedimentos armazenados em uma arquitetura, os métodos s da camada de acesso a dados invocam um procedimento armazenado específico em vez de emitir uma instrução SQL ad hoc. Isso centraliza o local das instruções SQL executadas (no banco de dados) em vez de tê-las definidas na arquitetura do aplicativo. Essa centralização imtraimente torna mais fácil localizar, analisar e ajustar as consultas e fornece uma imagem muito mais clara sobre onde e como o banco de dados está sendo usado.

Para obter mais informações sobre os conceitos básicos do procedimento armazenado, consulte os recursos na seção leitura adicional no final deste tutorial.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Etapa 1: criando as páginas da Web cenários de camada de acesso a dados avançados

Antes de começarmos nossa discussão sobre a criação de uma DAL usando procedimentos armazenados, vamos primeiro reservar um momento para criar as páginas do ASP.NET em nosso projeto de site, que precisaremos para isso e os vários tutoriais a seguir. Comece adicionando uma nova pasta chamada `AdvancedDAL`. Em seguida, adicione as seguintes páginas ASP.NET a essa pasta, assegurando a associação de cada página com a página mestra `Site.master`:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`

![Adicionar as páginas ASP.NET para os tutoriais de cenários de camada de acesso a dados avançados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Figura 1**: adicionar as páginas ASP.net para os tutoriais de cenários de camada de acesso a dados avançados

Assim como nas outras pastas, `Default.aspx` na pasta `AdvancedDAL` listará os tutoriais em sua seção. Lembre-se de que o controle de usuário `SectionLevelTutorialListing.ascx` fornece essa funcionalidade. Portanto, adicione esse controle de usuário a `Default.aspx` arrastando-o da Gerenciador de Soluções para a página s modo de exibição de Design.

[![adicionar o controle de usuário SectionLevelTutorialListing. ascx a default. aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)

**Figura 2**: Adicionar o controle de usuário `SectionLevelTutorialListing.ascx` ao `Default.aspx` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png))

Por fim, adicione essas páginas como entradas ao arquivo de `Web.sitemap`. Especificamente, adicione a seguinte marcação após o trabalho com dados em lote `<siteMapNode>`:

[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.xml)]

Depois de atualizar `Web.sitemap`, Reserve um momento para exibir o site de tutoriais por meio de um navegador. O menu à esquerda agora inclui itens para os tutoriais de cenários avançados da DAL.

![O mapa do site agora inclui entradas para os tutoriais de cenários avançados da DAL](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)

**Figura 3**: o mapa do site agora inclui entradas para os tutoriais de cenários avançados da Dal

## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Etapa 2: Configurando um TableAdapter para criar novos procedimentos armazenados

Para demonstrar a criação de uma camada de acesso a dados que usa procedimentos armazenados em vez de instruções SQL ad hoc, vamos criar um novo conjunto de dados tipado na pasta `~/App_Code/DAL` chamada `NorthwindWithSprocs.xsd`. Como fizemos esse processo detalhadamente nos tutoriais anteriores, continuaremos rapidamente pelas etapas aqui. Se você ficar preso ou precisar de mais instruções passo a passo para criar e configurar um conjunto de dados tipado, consulte o tutorial [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md) .

Adicione um novo conjunto de conjuntos ao projeto clicando com o botão direito do mouse na pasta `DAL`, escolhendo Adicionar novo item e selecionando o modelo de conjunto de um, conforme mostrado na Figura 4.

[![adicionar um novo DataSet tipado ao projeto chamado NorthwindWithSprocs. xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png)

**Figura 4**: adicionar um novo dataset tipado ao projeto chamado `NorthwindWithSprocs.xsd` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png))

Isso criará o novo DataSet tipado, abrirá seu designer, criará um novo TableAdapter e iniciará o assistente de configuração do TableAdapter. A primeira etapa do assistente de configuração do TableAdapter nos pede para selecionar o banco de dados com o qual trabalhar. A cadeia de conexão para o banco de dados Northwind deve estar listada na lista suspensa. Selecione esta e clique em Avançar.

Nesta próxima tela, podemos escolher como o TableAdapter deve acessar o banco de dados. Nos tutoriais anteriores, selecionamos a primeira opção, usam instruções SQL. Para este tutorial, selecione a segunda opção, criar novos procedimentos armazenados e clique em Avançar.

[![instrua o TableAdapter a criar novos procedimentos armazenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png)

**Figura 5**: Instrua o TableAdapter a criar novos procedimentos armazenados ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png))

Assim como acontece com o uso de instruções SQL ad hoc, na etapa a seguir, é solicitado que forneçamos a instrução `SELECT` para a consulta principal do TableAdapter s. Mas, em vez de usar a instrução `SELECT` inserida aqui para executar uma consulta ad hoc diretamente, o assistente do TableAdapter s criará um procedimento armazenado que contém essa consulta `SELECT`.

Use a seguinte consulta de `SELECT` para este TableAdapter:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]

[![inserir a consulta SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png)

**Figura 6**: Insira a consulta de `SELECT` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png))

> [!NOTE]
> A consulta acima difere ligeiramente da consulta principal do `ProductsTableAdapter` no DataSet tipado `Northwind`. Lembre-se de que o `ProductsTableAdapter` no conjunto de `Northwind` digitado inclui duas Subconsultas correlacionadas para retornar o nome da categoria e o nome da empresa para cada fornecedor e categoria de produto. No futuro [atualizando o tutorial do TableAdapter para usar junções](updating-the-tableadapter-to-use-joins-cs.md) , veremos como adicionar esses dados relacionados a esse TableAdapter.

Reserve um momento para clicar no botão Opções avançadas. Aqui, podemos especificar se o assistente também deve gerar instruções INSERT, Update e Delete para o TableAdapter, se deseja usar a simultaneidade otimista e se a tabela de dados deve ser atualizada após inserções e atualizações. A opção gerar instruções INSERT, Update e Delete é marcada por padrão. Deixe-a marcada. Para este tutorial, deixe a opção usar opções de simultaneidade otimista desmarcada.

Ao fazer com que os procedimentos armazenados sejam criados automaticamente pelo assistente do TableAdapter, parece que a opção atualizar a tabela de dados é ignorada. Independentemente de essa caixa de seleção estar marcada, os procedimentos armazenados de inserção e atualização resultantes recuperam o registro apenas inserido ou apenas atualizado, como veremos na etapa 3.

![Deixe a opção gerar instruções INSERT, Update e Delete marcada](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png)

**Figura 7**: deixar a opção gerar instruções INSERT, Update e Delete marcada

> [!NOTE]
> Se a opção usar simultaneidade otimista estiver marcada, o assistente adicionará outras condições à cláusula `WHERE` que impedirá que os dados sejam atualizados se houver alterações em outros campos. Consulte o tutorial [implementando simultaneidade otimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) para obter mais informações sobre como usar o recurso interno de controle de simultaneidade otimista do TableAdapter s.

Depois de inserir a consulta de `SELECT` e confirmar que a opção gerar instruções INSERT, Update e Delete está marcada, clique em Avançar. A próxima tela, mostrada na Figura 8, solicita os nomes dos procedimentos armazenados que o assistente criará para selecionar, inserir, atualizar e excluir dados. Altere os nomes dos procedimentos armazenados para `Products_Select`, `Products_Insert`, `Products_Update`e `Products_Delete`.

[![renomear os procedimentos armazenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Figura 8**: renomear os procedimentos armazenados ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))

Para ver o T-SQL que o assistente do TableAdapter usará para criar os quatro procedimentos armazenados, clique no botão Visualizar script SQL. Na caixa de diálogo Visualizar script SQL, você pode salvar o script em um arquivo ou copiá-lo para a área de transferência.

![Visualizar o script SQL usado para gerar os procedimentos armazenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Figura 9**: Visualizar o script SQL usado para gerar os procedimentos armazenados

Depois de nomear os procedimentos armazenados, clique em avançar para nomear os métodos correspondentes do TableAdapter s. Assim como ao usar instruções SQL ad hoc, podemos criar métodos que preenchem uma DataTable existente ou retornam uma nova. Também podemos especificar se o TableAdapter deve incluir o padrão DB-Direct para inserção, atualização e exclusão de registros. Deixe todas as três caixas de seleção marcadas, mas renomeie o método Return a DataTable para `GetProducts` (como mostrado na Figura 10).

[![nomear os métodos Fill e GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)

**Figura 10**: nomear os métodos `Fill` e `GetProducts` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png))

Clique em avançar para ver um resumo das etapas que o assistente executará. Conclua o assistente clicando no botão Concluir. Depois que o assistente for concluído, você será retornado para o designer do DataSet, que agora deve incluir o `ProductsDataTable`.

[![o designer do DataSet s mostra os ProductsDataTable adicionados recentemente](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)

**Figura 11**: o designer do DataSet s mostra as `ProductsDataTable` adicionadas recentemente ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png))

## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Etapa 3: examinando os procedimentos armazenados recém-criados

O assistente do TableAdapter usado na etapa 2 criou automaticamente os procedimentos armazenados para selecionar, inserir, atualizar e excluir dados. Esses procedimentos armazenados podem ser exibidos ou modificados por meio do Visual Studio acessando a Gerenciador de Servidores e fazendo Drill down na pasta de procedimentos armazenados do banco de dados. Como mostra a Figura 12, o banco de dados Northwind contém quatro novos procedimentos armazenados: `Products_Delete`, `Products_Insert`, `Products_Select`e `Products_Update`.

![Os quatro procedimentos armazenados criados na etapa 2 podem ser encontrados na pasta Database s Stored Procedures](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)

**Figura 12**: os quatro procedimentos armazenados criados na etapa 2 podem ser encontrados na pasta Database s Stored Procedures

> [!NOTE]
> Se você não vir a Gerenciador de Servidores, vá para o menu exibir e escolha a opção Gerenciador de Servidores. Se você não vir os procedimentos armazenados relacionados ao produto adicionados da etapa 2, tente clicar com o botão direito do mouse na pasta procedimentos armazenados e escolher atualizar.

Para exibir ou modificar um procedimento armazenado, clique duas vezes em seu nome na Gerenciador de Servidores ou, como alternativa, clique com o botão direito do mouse no procedimento armazenado e escolha abrir. A Figura 13 mostra o procedimento armazenado `Products_Delete`, quando aberto.

[![procedimentos armazenados podem ser abertos e modificados no Visual Studio](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png)

**Figura 13**: os procedimentos armazenados podem ser abertos e modificados no Visual Studio ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png))

O conteúdo dos procedimentos armazenados `Products_Delete` e `Products_Select` é bem simples. O `Products_Insert` e `Products_Update` procedimentos armazenados, por outro lado, garantem uma inspeção mais próxima, pois ambos executam uma instrução `SELECT` depois das instruções `INSERT` e `UPDATE`. Por exemplo, o SQL a seguir compõe o procedimento armazenado `Products_Insert`:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

O procedimento armazenado aceita como parâmetros de entrada as colunas `Products` que foram retornadas pela consulta `SELECT` especificada no assistente do TableAdapter s e esses valores são usados em uma instrução `INSERT`. Após a instrução `INSERT`, uma consulta `SELECT` é usada para retornar os valores de coluna de `Products` (incluindo o `ProductID`) do registro recém-adicionado. Esse recurso de atualização é útil ao adicionar um novo registro usando o padrão de atualização do lote, pois ele atualiza automaticamente as instâncias de `ProductRow` adicionadas recentemente `ProductID` Propriedades com os valores incrementados automaticamente atribuídos pelo banco de dados.

O código a seguir ilustra esse recurso. Ele contém um `ProductsTableAdapter` e `ProductsDataTable` criados para o conjunto de `NorthwindWithSprocs` DataSet tipado. Um novo produto é adicionado ao banco de dados criando uma instância de `ProductsRow`, fornecendo seus valores e chamando o método TableAdapter s `Update`, passando o `ProductsDataTable`. Internamente, o método do TableAdapter s `Update` enumera as instâncias de `ProductsRow` na DataTable passada (neste exemplo, há apenas um-o que acabamos de adicionar) e executa o comando apropriado de inserir, atualizar ou excluir. Nesse caso, o procedimento armazenado `Products_Insert` é executado, o que adiciona um novo registro à tabela `Products` e retorna os detalhes do registro recém-adicionado. O valor de `ProductID` da instância de `ProductsRow` é então atualizado. Depois que o método de `Update` for concluído, poderemos acessar o valor de `ProductID` do registro recém-adicionado por meio da propriedade `ProductsRow` s `ProductID`.

[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.cs)]

O procedimento armazenado `Products_Update` de forma semelhante inclui uma instrução `SELECT` após sua instrução `UPDATE`.

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample7.sql)]

Observe que esse procedimento armazenado inclui dois parâmetros de entrada para `ProductID`: `@Original_ProductID` e `@ProductID`. Essa funcionalidade permite cenários em que a chave primária pode ser alterada. Por exemplo, em um banco de dados de funcionários, cada registro de funcionário pode usar o número de seguro social do funcionário como sua chave primária. Para alterar um número de previdência social de funcionário existente, o novo número de seguro social e o original devem ser fornecidos. Para a tabela `Products`, essa funcionalidade não é necessária porque a coluna `ProductID` é uma coluna `IDENTITY` e nunca deve ser alterada. Na verdade, a instrução `UPDATE` no procedimento armazenado `Products_Update` não inclui a coluna `ProductID` em sua lista de colunas. Portanto, embora `@Original_ProductID` seja usada na cláusula `UPDATE` Statement s `WHERE`, ela é supérflua para a tabela `Products` e pode ser substituída pelo parâmetro `@ProductID`. Ao modificar os parâmetros de s de um procedimento armazenado, é importante que os métodos do TableAdapter que usam esse procedimento armazenado também sejam atualizados.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Etapa 4: modificar os parâmetros de s de um procedimento armazenado e atualizar o TableAdapter

Como o parâmetro `@Original_ProductID` é supérfluo, vamos removê-lo do procedimento armazenado `Products_Update` totalmente. Abra o `Products_Update` procedimento armazenado, exclua o parâmetro `@Original_ProductID` e, na cláusula `WHERE` da instrução `UPDATE`, altere o nome do parâmetro usado de `@Original_ProductID` para `@ProductID`. Depois de fazer essas alterações, o T-SQL no procedimento armazenado deve ser semelhante ao seguinte:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample8.sql)]

Para salvar essas alterações no banco de dados, clique no ícone salvar na barra de ferramentas ou pressione Ctrl + S. Neste ponto, o procedimento armazenado `Products_Update` não espera um parâmetro de entrada `@Original_ProductID`, mas o TableAdapter é configurado para passar esse parâmetro. Você pode ver os parâmetros que o TableAdapter enviará para o procedimento armazenado `Products_Update` selecionando o TableAdapter no designer de conjunto de um, acessando o janela Propriedades e clicando nas reticências na coleção de `Parameters`s `UpdateCommand` s. Isso abre a caixa de diálogo Editor de coleções de parâmetros mostrada na Figura 14.

![O editor de coleção de parâmetros lista os parâmetros usados passados para o procedimento armazenado Products_Update](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png)

**Figura 14**: o editor de coleção de parâmetros lista os parâmetros usados passados para o procedimento armazenado `Products_Update`

Você pode remover esse parâmetro aqui simplesmente selecionando o parâmetro `@Original_ProductID` na lista de membros e clicando no botão remover.

Como alternativa, você pode atualizar os parâmetros usados para todos os métodos clicando com o botão direito do mouse no TableAdapter no designer e escolhendo configurar. Isso abrirá o assistente de configuração do TableAdapter, listando os procedimentos armazenados usados para selecionar, inserir, atualizar e excluir, juntamente com os parâmetros que os procedimentos armazenados esperam receber. Se você clicar na lista suspensa atualização, poderá ver os parâmetros de entrada esperados `Products_Update` procedimentos armazenados, que agora não incluirão `@Original_ProductID` (consulte a Figura 15). Basta clicar em concluir para atualizar automaticamente a coleção de parâmetros usada pelo TableAdapter.

[![Alternativamente, você pode usar o assistente de configuração do TableAdapter s para atualizar suas coleções de parâmetros de métodos](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Figura 15**: como alternativa, você pode usar o assistente de configuração do TableAdapter s para atualizar suas coleções de parâmetros de métodos ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))

## <a name="step-5-adding-additional-tableadapter-methods"></a>Etapa 5: adicionando métodos adicionais do TableAdapter

Como ilustrado na etapa 2, ao criar um novo TableAdapter, é fácil ter os procedimentos armazenados correspondentes gerados automaticamente. O mesmo é verdadeiro ao adicionar métodos adicionais a um TableAdapter. Para ilustrar isso, vamos adicionar um método `GetProductByProductID(productID)` ao `ProductsTableAdapter` criado na etapa 2. Esse método usará como entrada um valor de `ProductID` e retornará detalhes sobre o produto especificado.

Comece clicando com o botão direito do mouse no TableAdapter e escolhendo Adicionar consulta no menu de contexto.

![Adicionar uma nova consulta ao TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Figura 16**: adicionar uma nova consulta ao TableAdapter

Isso iniciará o assistente de configuração de consulta do TableAdapter, que primeiro solicita como o TableAdapter deve acessar o banco de dados. Para que um novo procedimento armazenado seja criado, escolha a opção criar um novo procedimento armazenado e clique em Avançar.

[![escolher a opção criar um novo procedimento armazenado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)

**Figura 17**: escolha a opção criar um novo procedimento armazenado ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png))

A próxima tela nos pede para identificar o tipo de consulta a ser executado, se ele retornará um conjunto de linhas ou um único valor escalar, ou se executará uma instrução `UPDATE`, `INSERT`ou `DELETE`. Como o método `GetProductByProductID(productID)` retornará uma linha, deixe a opção Selecionar que retorna a linha selecionada e pressione Avançar.

[![escolher a opção Selecionar que retorna a linha](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)

**Figura 18**: escolha a opção Selecionar que retorna a linha ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png))

A próxima tela exibe a consulta principal TableAdapter s, que apenas lista o nome do procedimento armazenado (`dbo.Products_Select`). Substitua o nome do procedimento armazenado pela seguinte instrução `SELECT`, que retorna todos os campos de produto de um produto especificado:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample9.sql)]

[![substituir o nome do procedimento armazenado por uma consulta SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)

**Figura 19**: substituir o nome do procedimento armazenado por uma consulta `SELECT` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png))

A tela subsequente solicita que você nomeie o procedimento armazenado que será criado. Insira o nome `Products_SelectByProductID` e clique em Avançar.

[![nomear o novo procedimento armazenado Products_SelectByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Figura 20**: nomeie o novo procedimento armazenado `Products_SelectByProductID` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image46.png))

A etapa final do assistente nos permite alterar os nomes de método gerados, bem como indicar se deve ser usado o padrão Fill a DataTable, retornar um padrão DataTable ou ambos. Para esse método, deixe as duas opções marcadas, mas renomeie os métodos para `FillByProductID` e `GetProductByProductID`. Clique em avançar para exibir um resumo das etapas que o assistente executará e, em seguida, clique em concluir para concluir o assistente.

[![renomear os métodos TableAdapter s para FillByProductID e GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image47.png)

**Figura 21**: renomear os métodos TableAdapter s para `FillByProductID` e `GetProductByProductID` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image49.png))

Depois de concluir o assistente, o TableAdapter tem um novo método disponível, `GetProductByProductID(productID)` que, quando invocado, executará o procedimento armazenado `Products_SelectByProductID` que acabou de ser criado. Reserve um tempo para exibir esse novo procedimento armazenado do Gerenciador de Servidores analisando a pasta procedimentos armazenados e abrindo `Products_SelectByProductID` (se você não o vir, clique com o botão direito do mouse na pasta procedimentos armazenados e escolha Atualizar).

Observe que o procedimento armazenado `SelectByProductID` usa `@ProductID` como um parâmetro de entrada e executa a instrução `SELECT` que inserimos no assistente.

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Etapa 6: criando uma classe de camada de lógica de negócios

Em toda a série de tutoriais, fizemos a manutenção de uma arquitetura em camadas na qual a camada de apresentação fez todas as suas chamadas para a camada de lógica de negócios (BLL). Para aderir a essa decisão de design, primeiro precisamos criar uma classe BLL para o novo conjunto de dados tipado antes de poder acessar os dados do produto da camada de apresentação.

Crie um novo arquivo de classe chamado `ProductsBLLWithSprocs.cs` na pasta `~/App_Code/BLL` e adicione-o ao seguinte código:

[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample11.cs)]

Essa classe imita a semântica de classe de `ProductsBLL` de tutoriais anteriores, mas usa os objetos `ProductsTableAdapter` e `ProductsDataTable` do conjunto de `NorthwindWithSprocs`. Por exemplo, em vez de ter uma instrução `using NorthwindTableAdapters` no início do arquivo de classe como `ProductsBLL`, a classe `ProductsBLLWithSprocs` usa `using NorthwindWithSprocsTableAdapters`. Da mesma forma, os objetos `ProductsDataTable` e `ProductsRow` usados nessa classe são prefixados com o namespace `NorthwindWithSprocs`. A classe `ProductsBLLWithSprocs` fornece dois métodos de acesso a dados, `GetProducts` e `GetProductByProductID`e métodos para adicionar, atualizar e excluir uma única instância de produto.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Etapa 7: trabalhando com o conjunto de`NorthwindWithSprocs`de trabalho da camada de apresentação

Neste ponto, criamos uma DAL que usa procedimentos armazenados para acessar e modificar os dados subjacentes do banco de dados. Também criamos uma BLL rudimentar com métodos para recuperar todos os produtos ou um produto específico junto com métodos para adicionar, atualizar e excluir produtos. Para arredondar este tutorial, vamos criar uma página ASP.NET que usa a classe de `ProductsBLLWithSprocs` da BLL para exibir, atualizar e excluir registros.

Abra a página `NewSprocs.aspx` na pasta `AdvancedDAL` e arraste um GridView da caixa de ferramentas para o designer, nomeando-o `Products`. Na marca inteligente de GridView s, escolha associá-lo a um novo ObjectDataSource chamado `ProductsDataSource`. Configure o ObjectDataSource para usar a classe `ProductsBLLWithSprocs`, conforme mostrado na Figura 22.

[![configurar o ObjectDataSource para usar a classe ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image50.png)

**Figura 22**: configurar o ObjectDataSource para usar a classe `ProductsBLLWithSprocs` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image52.png))

A lista suspensa na guia selecionar tem duas opções, `GetProducts` e `GetProductByProductID`. Como queremos exibir todos os produtos no GridView, escolha o método `GetProducts`. As listas suspensas nas guias UPDATE, INSERT e DELETE têm apenas um método. Verifique se cada uma dessas listas suspensas tem o método apropriado selecionado e clique em concluir.

Depois que o assistente ObjectDataSource for concluído, o Visual Studio adicionará BoundFields e um CheckBoxField ao GridView para os campos de dados do produto. Ative os recursos internos de edição e exclusão do GridView, marcando as opções Habilitar edição e habilitar exclusão presentes na marca inteligente.

[![a página contém um GridView com suporte para edição e exclusão habilitado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image53.png)

**Figura 23**: a página contém um GridView com suporte para edição e exclusão habilitado ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image55.png))

Como vimos nos tutoriais anteriores, na conclusão do assistente ObjectDataSource s, o Visual Studio define a propriedade `OldValuesParameterFormatString` como {0}original\_. Isso precisa ser revertido para seu valor padrão de {0} para que os recursos de modificação de dados funcionem corretamente, considerando os parâmetros esperados pelos métodos em nossa BLL. Portanto, certifique-se de definir a propriedade `OldValuesParameterFormatString` como {0} ou remover a propriedade completamente da sintaxe declarativa.

Depois de concluir o assistente para configurar fonte de dados, ativar a edição e excluir o suporte no GridView e retornar a propriedade de `OldValuesParameterFormatString` ObjectDataSource s para seu valor padrão, a marcação declarativa de s de página deve ser semelhante ao seguinte:

[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample12.aspx)]

Neste ponto, poderíamos organizar o GridView Personalizando a interface de edição para incluir a validação, tendo as colunas `CategoryID` e `SupplierID` processadas como DropDownLists e assim por diante. Também poderíamos adicionar uma confirmação do lado do cliente ao botão excluir e recomendo que você reserve algum tempo para implementar esses aprimoramentos. No entanto, como esses tópicos foram abordados nos tutoriais anteriores, não os abordaremos novamente aqui.

Independentemente de você aprimorar ou não o GridView, teste os recursos de núcleo da página em um navegador. Como mostra a figura 24, a página lista os produtos em um GridView que fornece recursos de edição e exclusão por linha.

[![os produtos podem ser exibidos, editados e excluídos do GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image56.png)

**Figura 24**: os produtos podem ser exibidos, editados e excluídos do GridView ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image58.png))

## <a name="summary"></a>Resumo

Os TableAdapters em um conjunto de dados tipado podem acessar dados do banco de dado usando instruções SQL ad hoc ou por meio de procedimentos armazenados. Ao trabalhar com procedimentos armazenados, os procedimentos armazenados existentes podem ser usados ou o assistente TableAdapter pode ser instruído a criar novos procedimentos armazenados com base em uma consulta `SELECT`. Neste tutorial, exploramos como fazer com que os procedimentos armazenados sejam criados automaticamente para nós.

Embora ter os procedimentos armazenados gerados automaticamente ajude a economizar tempo, há alguns casos em que o procedimento armazenado criado pelo assistente não se alinha com o que criamos por conta própria. Um exemplo é o `Products_Update` procedimento armazenado, que era esperado tanto `@Original_ProductID` quanto `@ProductID` parâmetros de entrada, embora o parâmetro `@Original_ProductID` tenha sido supérfluo.

Em muitos cenários, os procedimentos armazenados já podem ter sido criados ou talvez queiramos compilá-los manualmente para ter um grau mais preciso de controle sobre os comandos s do procedimento armazenado. Em ambos os casos, queremos instruir o TableAdapter a usar procedimentos armazenados existentes para seus métodos. Veremos como fazer isso no próximo tutorial.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Criando e mantendo procedimentos armazenados](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Recuperando dados escalares de um procedimento armazenado](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Noções básicas sobre SQL Server procedimento armazenado](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Procedimentos armazenados: uma visão geral](http://www.sqlteam.com/item.asp?ItemID=563)
- [Escrevendo um procedimento armazenado](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Hilton Geisenow. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Próximo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
