---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
title: Consultando dados com o controle SqlDataSource (C#) | Microsoft Docs
author: rick-anderson
description: Nos tutoriais anteriores, usamos o controle ObjectDataSource para separar totalmente a camada de apresentação da camada de acesso a dados. Começando com este tutorial...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: 60512d6a-b572-4b7a-beb3-3e44b4d2020c
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 5bda42965f7d1db71b207c0b76e251b8fff64e31
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606215"
---
# <a name="querying-data-with-the-sqldatasource-control-c"></a>Consultar dados com o controle SqlDataSource (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_CS.exe) ou [baixar PDF](querying-data-with-the-sqldatasource-control-cs/_static/datatutorial47cs1.pdf)

> Nos tutoriais anteriores, usamos o controle ObjectDataSource para separar totalmente a camada de apresentação da camada de acesso a dados. A partir deste tutorial, aprendemos como o controle SqlDataSource pode ser usado para aplicativos simples que não exigem uma separação tão estrita de acesso a dados e apresentação.

## <a name="introduction"></a>Introdução

Todos os tutoriais que examinamos até agora usaram uma arquitetura em camadas que consiste em apresentação, lógica de negócios e camadas de acesso a dados. A DAL (camada de acesso a dados) foi criada no primeiro tutorial ([criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md)) e a camada de lógica de negócios no segundo ([criando uma camada de lógica de negócios](../introduction/creating-a-business-logic-layer-cs.md)). Começando com o tutorial [exibindo dados com o ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) , vimos como usar o novo controle objectdatasource ASP.NET 2,0 s para interagir declarativamente com a arquitetura da camada de apresentação.

Embora todos os tutoriais tenham usado até agora a arquitetura para trabalhar com os dados, também é possível acessar, inserir, atualizar e excluir dados do banco de dados diretamente de uma página do ASP.NET, ignorando a arquitetura. Isso coloca as consultas de banco de dados e a lógica de negócios específicas diretamente na página da Web. Para aplicativos suficientemente grandes ou complexos, projetar, implementar e usar uma arquitetura em camadas é extremamente importante para o sucesso, a capacidade de atualização e a facilidade de manutenção do aplicativo. No entanto, o desenvolvimento de uma arquitetura robusta pode ser desnecessário ao criar aplicativos simples e únicos.

O ASP.NET 2,0 fornece cinco controles de fonte de dados internos [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx)e [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). O SqlDataSource pode ser usado para acessar e modificar dados diretamente de um banco de dado relacional, incluindo Microsoft SQL Server, Microsoft Access, Oracle, MySQL e outros. Neste tutorial e nos próximos três, examinaremos como trabalhar com o controle SqlDataSource, explorando como consultar e filtrar os dados do banco, bem como usar o SqlDataSource para inserir, atualizar e excluir dados.

![O ASP.NET 2,0 inclui cinco controles de fonte de dados internos](querying-data-with-the-sqldatasource-control-cs/_static/image1.gif)

**Figura 1**: ASP.NET 2,0 inclui cinco controles de fonte de dados internos

## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Comparando o ObjectDataSource e o SqlDataSource

Conceitualmente, os controles ObjectDataSource e SqlDataSource são simplesmente proxies para dados. Conforme discutido no tutorial [exibindo dados com o ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) , o ObjectDataSource tem propriedades que indicam o tipo de objeto que fornece os dados e os métodos a serem invocados para selecionar, inserir, atualizar e excluir dados do tipo de objeto subjacente. Depois que as propriedades de ObjectDataSource s foram configuradas, um controle da Web de dados como GridView, DetailsView ou DataList pode ser associado ao controle, usando os métodos ObjectDataSource s `Select()`, `Insert()`, `Delete()`e `Update()` para interagir com a arquitetura subjacente.

O SqlDataSource fornece a mesma funcionalidade, mas opera em um banco de dados relacional em vez de uma biblioteca de objetos. Com o SqlDataSource, devemos especificar a cadeia de conexão do banco de dados e as consultas SQL ad hoc ou procedimentos armazenados a serem executados para inserir, atualizar, excluir e recuperar dados. Os métodos SqlDataSource s `Select()`, `Insert()`, `Update()`e `Delete()`, quando invocados, se conectam ao banco de dados especificado e executam a consulta SQL apropriada. Como ilustra o diagrama a seguir, esses métodos fazem o trabalho Grunt de se conectar a um banco de dados, emitir uma consulta e retornar os resultados.

![O SqlDataSource serve como um proxy para o banco de dados](querying-data-with-the-sqldatasource-control-cs/_static/image2.gif)

**Figura 2**: o SqlDataSource serve como um proxy para o banco de dados

> [!NOTE]
> Neste tutorial, vamos nos concentrar na recuperação de dados do banco de dado. No tutorial [inserindo, atualizando e excluindo dados com o controle SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md) , veremos como configurar o SqlDataSource para dar suporte à inserção, atualização e exclusão.

## <a name="the-sqldatasource-and-accessdatasource-controls"></a>Os controles SqlDataSource e AccessDataSource

Além do controle SqlDataSource, ASP.NET 2,0 também inclui um controle AccessDataSource. Esses dois controles diferentes levam muitos desenvolvedores novos ao ASP.NET 2,0 para suspeitar que o controle AccessDataSource foi projetado para trabalhar exclusivamente com o Microsoft Access com o controle SqlDataSource projetado para trabalhar exclusivamente com Microsoft SQL Server. Embora o AccessDataSource seja projetado para funcionar especificamente com o Microsoft Access, o controle SqlDataSource funciona com *qualquer* banco de dados relacional que possa ser acessado por meio do .net. Isso inclui quaisquer armazenamentos de dados em conformidade com OleDb ou ODBC, como Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL e PostgreSQL, entre muitos outros.

A única diferença entre os controles AccessDataSource e SqlDataSource é como as informações de conexão do banco de dados são especificadas. O controle AccessDataSource precisa apenas do caminho do arquivo para o arquivo de banco de dados Access. O SqlDataSource, por outro lado, requer uma cadeia de conexão completa.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Etapa 1: criando as páginas da Web SqlDataSource

Antes de começarmos a explorar como trabalhar diretamente com os dados do banco usando o controle SqlDataSource, vamos primeiro reservar um momento para criar as páginas do ASP.NET em nosso projeto de site, que precisaremos para este tutorial e os três seguintes. Comece adicionando uma nova pasta chamada `SqlDataSource`. Em seguida, adicione as seguintes páginas ASP.NET a essa pasta, assegurando a associação de cada página com a página mestra `Site.master`:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`

![Adicionar as páginas ASP.NET para os tutoriais relacionados ao SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image3.gif)

**Figura 3**: adicionar as páginas ASP.net para os tutoriais relacionados ao SqlDataSource

Assim como nas outras pastas, `Default.aspx` na pasta `SqlDataSource` listará os tutoriais em sua seção. Lembre-se de que o controle de usuário `SectionLevelTutorialListing.ascx` fornece essa funcionalidade. Portanto, adicione esse controle de usuário a `Default.aspx` arrastando-o da Gerenciador de Soluções para a página s modo de exibição de Design.

[![adicionar o controle de usuário SectionLevelTutorialListing. ascx a default. aspx](querying-data-with-the-sqldatasource-control-cs/_static/image5.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image4.gif)

**Figura 4**: Adicionar o controle de usuário `SectionLevelTutorialListing.ascx` ao `Default.aspx` ([clique para exibir a imagem em tamanho normal](querying-data-with-the-sqldatasource-control-cs/_static/image6.gif))

Por fim, adicione essas quatro páginas como entradas ao arquivo de `Web.sitemap`. Especificamente, adicione a seguinte marcação após a adição de botões personalizados ao DataList e ao repetidor `<siteMapNode>`:

[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample1.sql)]

Depois de atualizar `Web.sitemap`, Reserve um momento para exibir o site de tutoriais por meio de um navegador. O menu à esquerda agora inclui itens para os tutoriais de edição, inserção e exclusão.

![O mapa do site agora inclui entradas para os tutoriais do SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image7.gif)

**Figura 5**: o mapa do site agora inclui entradas para os tutoriais do SqlDataSource

## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Etapa 2: adicionando e Configurando o controle SqlDataSource

Comece abrindo a página de `Querying.aspx` na pasta `SqlDataSource` e alterne para modo de exibição de Design. Arraste um controle SqlDataSource da caixa de ferramentas para o designer e defina seu `ID` como `ProductsDataSource`. Assim como com o ObjectDataSource, o SqlDataSource não produz nenhuma saída renderizada e, portanto, aparece como uma caixa cinza na superfície de design. Para configurar o SqlDataSource, clique no link configurar fonte de dados na marca inteligente SqlDataSource s.

![Clique no link configurar fonte de dados na marca inteligente SqlDataSource s](querying-data-with-the-sqldatasource-control-cs/_static/image8.gif)

**Figura 6**: clique no link configurar fonte de dados da marca inteligente SqlDataSource s

Isso abre o assistente para configurar fonte de dados do controle SqlDataSource s. Embora as etapas do assistente sejam diferentes do controle ObjectDataSource s, a meta final é a mesma para fornecer os detalhes sobre como recuperar, inserir, atualizar e excluir dados por meio da fonte de dados. Para o SqlDataSource, isso implica especificar o banco de dados subjacente a ser usado e fornecer as instruções SQL ad hoc ou procedimentos armazenados.

A primeira etapa do assistente solicita o banco de dados. A lista suspensa inclui os bancos de dados encontrados na pasta de `App_Data` do aplicativo Web e aqueles que foram adicionados ao nó conexões de data no Gerenciador de Servidores. Como já adicionamos uma cadeia de conexão para o `NORTHWIND.MDF` banco de dados na pasta `App_Data` ao nosso arquivo de `Web.config` do projeto, a lista suspensa inclui uma referência a essa cadeia de conexão, `NORTHWINDConnectionString`. Escolha este item na lista suspensa e clique em Avançar.

![Escolha o NORTHWINDConnectionString na lista suspensa](querying-data-with-the-sqldatasource-control-cs/_static/image9.gif)

**Figura 7**: escolha a `NORTHWINDConnectionString` na lista suspensa

Depois de escolher o banco de dados, o assistente solicitará que a consulta retorne dados. Podemos especificar as colunas de uma tabela ou exibição para retornar ou pode inserir uma instrução SQL personalizada ou especificar um procedimento armazenado. Você pode alternar entre essa opção por meio das opções especificar uma instrução SQL personalizada ou um procedimento armazenado e especificar colunas de uma tabela ou exibir botões de opção.

> [!NOTE]
> Para este primeiro exemplo, deixe s usar a opção especificar colunas de uma tabela ou exibição. Retornaremos ao assistente mais adiante neste tutorial e exploraremos a opção especificar uma instrução SQL personalizada ou um procedimento armazenado.

A Figura 8 mostra a tela configurar a instrução SELECT quando o botão de opção especificar colunas de uma tabela ou exibição está selecionado. A lista suspensa contém o conjunto de tabelas e exibições no banco de dados Northwind, com a tabela selecionada ou as colunas de exibição exibidas na lista de caixas de seleção abaixo. Para este exemplo, vamos retornar as colunas `ProductID`, `ProductName`e `UnitPrice` da tabela `Products`. Como a Figura 8 mostra, depois de fazer essas seleções, o assistente mostra a instrução SQL resultante `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.

![Retornar dados da tabela produtos](querying-data-with-the-sqldatasource-control-cs/_static/image10.gif)

**Figura 8**: retornar dados da tabela `Products`

Depois de configurar o assistente para retornar as colunas `ProductID`, `ProductName`e `UnitPrice` da tabela `Products`, clique no botão Avançar. Essa tela final fornece uma oportunidade para examinar os resultados da consulta configurada na etapa anterior. Clicar no botão Testar consulta executa a instrução de `SELECT` configurada e exibe os resultados em uma grade.

![Clique no botão Testar consulta para examinar sua consulta SELECT](querying-data-with-the-sqldatasource-control-cs/_static/image11.gif)

**Figura 9**: clique no botão Testar consulta para examinar sua consulta de `SELECT`

Para concluir o assistente, clique em concluir.

Assim como com o ObjectDataSource, o assistente de SqlDataSource s apenas atribui valores às propriedades do controle s, ou seja, as propriedades [`ConnectionString`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) e [`SelectCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) . Depois de concluir o assistente, sua marcação declarativa de controle de SqlDataSource deve ser semelhante ao seguinte:

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample2.aspx)]

A propriedade `ConnectionString` fornece informações sobre como se conectar ao banco de dados. Essa propriedade pode ser atribuída a um valor de cadeia de conexão completo e embutido em código ou pode apontar para uma cadeia de conexão no `Web.config`. Para fazer referência a um valor de cadeia de conexão em Web. config, use a sintaxe `<%$ expressionPrefix:expressionValue %>`. Normalmente, *ExpressionPrefix* é connectionStrings e *expressionvalue* é o nome da cadeia de conexão na seção `Web.config` [`<connectionStrings>`](https://msdn.microsoft.com/library/bf7sd233.aspx). No entanto, a sintaxe pode ser usada para fazer referência a elementos de `<appSettings>` ou conteúdo de arquivos de recursos. Consulte [visão geral das expressões ASP.net](https://msdn.microsoft.com/library/d5bd1tad.aspx) para obter mais informações sobre essa sintaxe.

A propriedade `SelectCommand` especifica a instrução SQL ad hoc ou o procedimento armazenado a ser executado para retornar os dados.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Etapa 3: adicionar um controle da Web de dados e associá-lo ao SqlDataSource

Depois que o SqlDataSource tiver sido configurado, ele poderá ser associado a um controle da Web de dados, como um GridView ou DetailsView. Para este tutorial, deixe que os s exibam os dados em um GridView. Na caixa de ferramentas, arraste um GridView para a página e associe-o ao `ProductsDataSource` SqlDataSource escolhendo a fonte de dados na lista suspensa na marca inteligente de GridView.

[![adicionar um GridView e associá-lo ao controle SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image13.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image12.gif)

**Figura 10**: adicionar um GridView e associá-lo ao controle SqlDataSource ([clique para exibir a imagem em tamanho normal](querying-data-with-the-sqldatasource-control-cs/_static/image14.gif))

Depois de selecionar o controle SqlDataSource na lista suspensa na marca inteligente de GridView, o Visual Studio adicionará automaticamente um BoundField ou CheckBoxField ao GridView para cada uma das colunas retornadas pelo controle da fonte de dados. Como o SqlDataSource retorna três colunas de banco de dados `ProductID`, `ProductName`e `UnitPrice` há três campos no GridView.

Reserve um momento para configurar o GridView s três BoundFields. Altere o campo de `ProductName` s `HeaderText` propriedade para nome do produto e o campo de `UnitPrice` s para preço. Também formate o campo `UnitPrice` como uma moeda. Depois de fazer essas modificações, sua marcação declarativa de GridView deve ser semelhante ao seguinte:

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample3.aspx)]

Visite esta página por meio de um navegador. Como mostra a Figura 11, o GridView lista cada produto s `ProductID`, `ProductName`e valores de `UnitPrice`.

[![o GridView exibe todos os valores de ProductID, NomeDoProduto e PreçoUnitário do produto](querying-data-with-the-sqldatasource-control-cs/_static/image16.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image15.gif)

**Figura 11**: o GridView exibe cada produto s `ProductID`, `ProductName`e valores de `UnitPrice` ([clique para exibir a imagem em tamanho normal](querying-data-with-the-sqldatasource-control-cs/_static/image17.gif))

Quando a página é visitada, o GridView invoca seu método `Select()` do controle da fonte de dados. Quando estávamos usando o controle ObjectDataSource, isso chamou o método `ProductsBLL` Class s `GetProducts()`. No entanto, com o SqlDataSource, o método `Select()` estabelece uma conexão com o banco de dados especificado e emite o `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, neste exemplo). O SqlDataSource retorna seus resultados que o GridView enumera, criando uma linha no GridView para cada registro de banco de dados retornado.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Os recursos internos de controle da Web de dados e o controle SqlDataSource

Em geral, os recursos inerentes à paginação de controles da Web de dados, classificação, edição, exclusão, inserção e assim por diante são específicos ao controle da Web de dados e não dependem do controle da fonte de dados usado. Ou seja, o GridView pode utilizar sua paginação interna, classificar, editar e excluir se está associado a um ObjectDataSource ou a um SqlDataSource. No entanto, determinados recursos de controle da Web de dados são sensíveis ao controle da fonte de dados que está sendo usado ou à configuração de s do controle da fonte de dados.

Por exemplo, na [paginação eficiente por meio de grandes quantidades de dados](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) , discutimos como, por padrão, a lógica de paginação para os controles da Web de dados naively retorna *todos os* registros da fonte de dados subjacente e, em seguida, exibe apenas o subconjunto apropriado de registros de acordo com o índice de página atual e o número de registros a serem exibidos por página. Esse modelo é altamente ineficiente durante a paginação por meio de conjuntos de resultados suficientemente grandes. Felizmente, o ObjectDataSource pode ser configurado para dar suporte à paginação personalizada, que retorna apenas o subconjunto preciso de registros a serem exibidos. No entanto, o controle SqlDataSource não tem as propriedades para implementar a paginação personalizada.

Outra sutileza da paginação e da classificação surge com o SqlDataSource. Por padrão, os dados retornados de um SqlDataSource podem ser paginados ou classificados por meio de GridView. Para demonstrar isso, marque as opções habilitar paginação e Habilitar classificação na marca inteligente GridView s em `Querying.aspx` e verifique se isso funciona conforme o esperado.

A classificação e a paginação funcionam porque o SqlDataSource recupera os dados do banco de dado em um conjunto de um tipo sem rigidez de tipos. O número total de registros retornados pela consulta um aspecto essencial para implementar a paginação pode ser determinado do conjunto de dado. Além disso, os resultados de s de DataSet podem ser classificados por meio de um DataView. Esses recursos são usados automaticamente pelo SqlDataSource quando o GridView solicita dados paginados ou classificados.

O SqlDataSource pode ser configurado para retornar um DataReader em vez de um DataSet, alterando sua [propriedade`DataSourceMode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) de `DataSet` (o padrão) para `DataReader`. Usar um DataReader pode ser preferido em situações ao passar os resultados de SqlDataSource s para o código existente que espera um DataReader. Além disso, como os datalêrs são objetos consideravelmente mais simples do que os conjuntos de dado, eles oferecem melhor desempenho. No entanto, se você fizer essa alteração, o controle da Web de dados não poderá classificar nem paginar, pois o SqlDataSource não pode determinar quantos registros são retornados pela consulta, nem o DataReader oferece nenhuma técnica para classificar os dados retornados.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Etapa 4: usando uma instrução SQL personalizada ou um procedimento armazenado

Ao configurar o controle SqlDataSource, a consulta usada para retornar dados pode ser especificada em uma das duas abordagens como uma instrução SQL personalizada ou um procedimento armazenado, ou como colunas de uma tabela ou exibição existente. Na etapa 2, examinamos a seleção de colunas na tabela `Products`. Vamos examinar usando uma instrução SQL personalizada.

Adicione outro controle GridView à página `Querying.aspx` e escolha criar uma nova fonte de dados na lista suspensa na marca inteligente. Em seguida, indique que os dados serão extraídos de um banco de dado. isso criará um novo controle SqlDataSource. Nomeie o `ProductsWithCategoryInfoDataSource`de controle.

![Criar um novo controle SqlDataSource chamado ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image18.gif)

**Figura 12**: criar um novo controle SqlDataSource chamado `ProductsWithCategoryInfoDataSource`

A próxima tela nos pede para especificar o banco de dados. Como fizemos de volta na Figura 7, selecione o `NORTHWINDConnectionString` na lista suspensa e clique em Avançar. Na tela configurar a instrução SELECT, escolha o botão de opção especificar uma instrução SQL personalizada ou um procedimento armazenado e clique em Avançar. Isso abrirá a tela definir instruções personalizadas ou procedimentos armazenados, que oferece guias rotuladas como selecionar, atualizar, inserir e excluir. Em cada guia, você pode inserir uma instrução SQL personalizada na caixa de texto ou escolher um procedimento armazenado na lista suspensa. Neste tutorial, veremos como inserir uma instrução SQL personalizada; o próximo tutorial inclui um exemplo que usa um procedimento armazenado.

![Insira uma instrução SQL personalizada ou escolha um procedimento armazenado](querying-data-with-the-sqldatasource-control-cs/_static/image19.gif)

**Figura 13**: inserir uma instrução SQL personalizada ou escolher um procedimento armazenado

A instrução SQL personalizada pode ser inserida à mão na caixa de texto ou pode ser construída graficamente clicando no botão Construtor de Consultas. No Construtor de Consultas ou na caixa de texto, use a consulta a seguir para retornar os campos `ProductID` e `ProductName` da tabela `Products` usando uma `JOIN` para recuperar o produto s `CategoryName` da tabela `Categories`:

[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample4.sql)]

![Você pode construir graficamente a consulta usando o Construtor de Consultas](querying-data-with-the-sqldatasource-control-cs/_static/image20.gif)

**Figura 14**: você pode construir graficamente a consulta usando o construtor de consultas

Depois de especificar a consulta, clique em avançar para prosseguir para a tela de consulta de teste. Clique em concluir para concluir o assistente de SqlDataSource.

Depois de concluir o assistente, o GridView terá três BoundFields adicionados a ele exibindo as colunas `ProductID`, `ProductName`e `CategoryName` retornadas da consulta e resultando na seguinte marcação declarativa:

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample5.aspx)]

[![o GridView mostra cada ID de produto, nome e nome de categoria associado](querying-data-with-the-sqldatasource-control-cs/_static/image22.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image21.gif)

**Figura 15**: o GridView mostra cada ID de produto, nome e nome de categoria associado ([clique para exibir a imagem em tamanho normal](querying-data-with-the-sqldatasource-control-cs/_static/image23.gif))

## <a name="summary"></a>Resumo

Neste tutorial, vimos como consultar e exibir dados usando o controle SqlDataSource. Como o ObjectDataSource, o SqlDataSource serve como um proxy, fornecendo uma abordagem declarativa para acessar dados. Suas propriedades especificam o banco de dados ao qual se conectar e a consulta do SQL `SELECT` a ser executada; Eles podem ser especificados por meio da janela Propriedades ou usando o assistente para configurar DataSource.

Os exemplos de consulta `SELECT` que examinamos neste tutorial retornaram todos os registros da consulta especificada. No entanto, o controle SqlDataSource pode incluir uma cláusula `WHERE` com parâmetros cujos valores são atribuídos programaticamente ou são automaticamente extraídos de uma fonte especificada. Vamos examinar como criar e usar consultas parametrizadas no próximo tutorial!

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Acessando dados de banco de dados relacional](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Visão geral do controle SqlDataSource](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Tutoriais de início rápido do ASP.NET: o controle SqlDataSource](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [O elemento de `<connectionStrings>` Web. config](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Referência de cadeia de conexão de banco de dados](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Janaína Connery, Bernadette Leigh e David Suru. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](using-parameterized-queries-with-the-sqldatasource-cs.md)
