---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
title: Usando consultas parametrizadas com o SqlDataSource (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, continuamos nossa visão do controle SqlDataSource e aprendemos a definir consultas parametrizadas. Os parâmetros podem ser especificados i...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: e322f34c-83b7-41ea-ab65-ab1e0bdcc609
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 19b93ff6c0878ae6ed546d347cafef95fd2a01e6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74597526"
---
# <a name="using-parameterized-queries-with-the-sqldatasource-vb"></a>Uso de consultas parametrizadas com o SqlDataSource (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_VB.exe) ou [baixar PDF](using-parameterized-queries-with-the-sqldatasource-vb/_static/datatutorial48vb1.pdf)

> Neste tutorial, continuamos nossa visão do controle SqlDataSource e aprendemos a definir consultas parametrizadas. Os parâmetros podem ser especificados de forma declarativa e programaticamente, e podem ser extraídos de vários locais, como QueryString, estado da sessão, outros controles e muito mais.

## <a name="introduction"></a>Introdução

No tutorial anterior, vimos como usar o controle SqlDataSource para recuperar dados diretamente de um banco de dado. Usando o assistente para configurar fonte de dados, poderíamos escolher o banco de dado e, em seguida: escolher as colunas a serem retornadas de uma tabela ou exibição; Insira uma instrução SQL personalizada; ou use um procedimento armazenado. Se selecionar colunas de uma tabela ou exibição ou inserir uma instrução SQL personalizada, a propriedade de `SelectCommand` do controle SqlDataSource é atribuída à instrução `SELECT` SQL ad hoc resultante e é essa instrução `SELECT` que é executada quando o método SqlDataSource s `Select()` é invocado (de forma programática ou automática de um controle da Web de dados).

As instruções SQL `SELECT` usadas nas demonstrações do tutorial anterior não tinham cláusulas `WHERE`. Em uma instrução `SELECT`, a cláusula `WHERE` pode ser usada para limitar os resultados retornados. Por exemplo, para exibir os nomes de produtos que custam mais de $50, poderíamos usar a seguinte consulta:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample1.sql)]

Normalmente, os valores usados em uma cláusula `WHERE` são determinados por alguma fonte externa, como um valor de QueryString, uma variável de sessão ou entrada do usuário de um controle da Web na página. O ideal é que essas entradas sejam especificadas por meio do uso de *parâmetros*. Com Microsoft SQL Server, os parâmetros são indicados usando `@parameterName`, como em:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample2.sql)]

O SqlDataSource dá suporte a consultas parametrizadas, tanto para instruções `SELECT` quanto para instruções `INSERT`, `UPDATE`e `DELETE`. Além disso, os valores de parâmetro podem ser extraídos automaticamente de uma variedade de fontes de QueryString, estado de sessão, controles na página e assim por diante ou podem ser atribuídos programaticamente. Neste tutorial, veremos como definir consultas parametrizadas, bem como especificar os valores de parâmetro de forma declarativa e programaticamente.

> [!NOTE]
> No tutorial anterior, comparamos o ObjectDataSource, que tem sido a nossa ferramenta de escolha nos primeiros 46 tutoriais com o SqlDataSource, observando suas semelhanças conceituais. Essas semelhanças também se estendem para parâmetros. Os parâmetros de ObjectDataSource s mapeados para os parâmetros de entrada para os métodos na camada de lógica de negócios. Com o SqlDataSource, os parâmetros são definidos diretamente na consulta SQL. Ambos os controles têm coleções de parâmetros para seus `Select()`, `Insert()`, `Update()`e métodos de `Delete()`, e ambos podem ter esses valores de parâmetro populados a partir de fontes predefinidas (valores de QueryString, variáveis de sessão e assim por diante) ou atribuídos programaticamente.

## <a name="creating-a-parameterized-query"></a>Criando uma consulta parametrizada

O assistente para configurar fonte de dados do controle SqlDataSource oferece três vias para definir o comando a ser executado para recuperar registros de banco de dados:

- Escolhendo as colunas de uma tabela ou exibição existente,
- Inserindo uma instrução SQL personalizada ou
- Escolhendo um procedimento armazenado

Ao separar colunas de uma tabela ou exibição existente, os parâmetros para a cláusula de `WHERE` devem ser especificados por meio da caixa de diálogo Adicionar cláusula de `WHERE`. No entanto, ao criar uma instrução SQL personalizada, você pode inserir os parâmetros diretamente na cláusula `WHERE` (usando `@parameterName` para denotar cada parâmetro). Um [procedimento armazenado](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) consiste em uma ou mais instruções SQL, e essas instruções podem ser parametrizadas. No entanto, os parâmetros usados nas instruções SQL devem ser passados como parâmetros de entrada para o procedimento armazenado.

Como a criação de uma consulta parametrizada depende de como o `SelectCommand` de SqlDataSource s é especificado, vamos dar uma olhada em todas as três abordagens. Para começar, abra a página `ParameterizedQueries.aspx` na pasta `SqlDataSource`, arraste um controle SqlDataSource da caixa de ferramentas para o designer e defina seu `ID` como `Products25BucksAndUnderDataSource`. Em seguida, clique no link configurar fonte de dados na marca inteligente controlar s. Selecione o banco de dados a ser usado (`NORTHWINDConnectionString`) e clique em Avançar.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Etapa 1: adicionando uma cláusula`WHERE`ao escolher as colunas de uma tabela ou exibição

Ao selecionar os dados a serem retornados do banco de dado com o controle SqlDataSource, o assistente para configurar fonte de dados nos permite simplesmente escolher as colunas a serem retornadas de uma tabela ou exibição existente (consulte a Figura 1). Fazer isso cria automaticamente uma instrução SQL `SELECT`, que é enviada ao banco de dados quando o método SqlDataSource s `Select()` é invocado. Como fizemos no tutorial anterior, selecione a tabela Products na lista suspensa e verifique as colunas `ProductID`, `ProductName`e `UnitPrice`.

[![escolher as colunas a serem retornadas de uma tabela ou exibição](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.png)

**Figura 1**: escolher as colunas a serem retornadas de uma tabela ou exibição ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.png))

Para incluir uma cláusula `WHERE` na instrução `SELECT`, clique no botão `WHERE`, que abre a caixa de diálogo Adicionar cláusula `WHERE` (consulte a Figura 2). Para adicionar um parâmetro para limitar os resultados retornados pela consulta `SELECT`, primeiro escolha a coluna para filtrar os dados. Em seguida, escolha o operador a ser usado para filtragem (=, &lt;, &lt;=, &gt;e assim por diante). Por fim, escolha a origem do valor do parâmetro s, como do estado da sessão ou da QueryString. Depois de configurar o parâmetro, clique no botão Adicionar para incluí-lo na consulta `SELECT`.

Para este exemplo, vamos retornar apenas os resultados em que o valor `UnitPrice` seja menor ou igual a $25. Portanto, escolha `UnitPrice` na lista suspensa coluna e &lt;= na lista suspensa operador. Ao usar um valor de parâmetro embutido em código (como $25) ou se o valor do parâmetro for ser especificado programaticamente, selecione nenhum na lista suspensa origem. Em seguida, insira o valor do parâmetro embutido em código na caixa de texto valor 25, 0 e conclua o processo clicando no botão Adicionar.

[![limitar os resultados retornados da caixa de diálogo Adicionar cláusula WHERE](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.png)

**Figura 2**: limitar os resultados retornados da caixa de diálogo Adicionar cláusula de `WHERE` ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.png))

Depois de adicionar o parâmetro, clique em OK para retornar ao Assistente para configurar fonte de dados. A instrução `SELECT` na parte inferior do assistente agora deve incluir uma cláusula `WHERE` com um parâmetro chamado `@UnitPrice`:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample3.sql)]

> [!NOTE]
> Se você especificar várias condições na cláusula `WHERE` da caixa de diálogo Adicionar cláusula `WHERE`, o assistente as unirá com o operador de `AND`. Se precisar incluir um `OR` na cláusula `WHERE` (como `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`), você precisará criar a instrução `SELECT` por meio da tela de instrução SQL personalizada.

Conclua a configuração de SqlDataSource (clique em avançar e depois em concluir) e inspecione a marcação declarativa de SqlDataSource s. A marcação agora inclui uma coleção de `<SelectParameters>`, que soletra as fontes para os parâmetros na `SelectCommand`.

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample4.aspx)]

Quando o método SqlDataSource s `Select()` é invocado, o valor do parâmetro `UnitPrice` (25, 0) é aplicado ao parâmetro `@UnitPrice` no `SelectCommand` antes de ser enviado ao banco de dados. O resultado líquido é que somente os produtos menores ou iguais a $25 são retornados da tabela `Products`. Para confirmar isso, adicione um GridView à página, associe-o a essa fonte de dados e, em seguida, exiba a página por meio de um navegador. Você só verá os produtos listados que sejam menores ou iguais a $25, como a Figura 3 confirma.

[![apenas os produtos menores ou iguais a $25 são exibidos](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.png)

**Figura 3**: somente os produtos menores ou iguais a $25 são exibidos ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.png))

## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Etapa 2: adicionando parâmetros a uma instrução SQL personalizada

Ao adicionar uma instrução SQL personalizada, você pode inserir a cláusula `WHERE` explicitamente ou especificar um valor na célula de filtro da Construtor de Consultas. Para demonstrar isso, vamos exibir apenas esses produtos em um GridView cujos preços sejam menores do que um determinado limite. Comece adicionando uma caixa de texto à página de `ParameterizedQueries.aspx` para coletar esse valor de limite do usuário. Defina a Propriedade TextBox s `ID` como `MaxPrice`. Adicione um controle Web de botão e defina sua propriedade `Text` para exibir produtos correspondentes.

Em seguida, arraste um GridView para a página e, em sua marca inteligente, escolha criar um novo SqlDataSource nomeado `ProductsFilteredByPriceDataSource`. No Assistente para configurar fonte de dados, vá para a tela especificar uma instrução SQL personalizada ou um procedimento armazenado (consulte a Figura 4) e insira a seguinte consulta:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample5.sql)]

Depois de inserir a consulta (manualmente ou por meio do Construtor de Consultas), clique em Avançar.

[![retornar apenas os produtos menores ou iguais a um valor de parâmetro](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.png)

**Figura 4**: retornar somente os produtos menores ou iguais a um valor de parâmetro ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.png))

Como a consulta inclui parâmetros, a próxima tela do assistente solicitará a origem dos valores dos parâmetros. Escolha controle na lista suspensa origem do parâmetro e `MaxPrice` (o valor de `ID` do controle TextBox) na lista suspensa ControlID. Você também pode inserir um valor padrão opcional para usar no caso em que o usuário não tenha inserido nenhum texto na caixa de texto `MaxPrice`. Por enquanto, não insira um valor padrão.

[![a propriedade Text de s MaxPrice TextBox é usada como a origem do parâmetro](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.png)

**Figura 5**: a propriedade `MaxPrice` caixa de texto s `Text` é usada como a origem do parâmetro ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.png))

Conclua o assistente para configurar fonte de dados clicando em avançar e em concluir. A marcação declarativa para GridView, TextBox, Button e SqlDataSource segue:

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample6.aspx)]

Observe que o parâmetro na seção de `<SelectParameters>` de SqlDataSource s é um `ControlParameter`, que inclui propriedades adicionais, como `ControlID` e `PropertyName`. Quando o método SqlDataSource s `Select()` é invocado, o `ControlParameter` captura o valor da propriedade de controle da Web especificada e o atribui ao parâmetro correspondente no `SelectCommand`. Neste exemplo, a propriedade de texto `MaxPrice` s é usada como o valor do parâmetro `@MaxPrice`.

Reserve um minuto para exibir esta página por meio de um navegador. Ao visitar a página pela primeira vez ou sempre que a caixa de texto `MaxPrice` não tem um valor, nenhum registro é exibido no GridView.

[![nenhum registro é exibido quando a caixa de texto MaxPrice está vazia](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.png)

**Figura 6**: nenhum registro é exibido quando a caixa de texto `MaxPrice` está vazia ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.png))

O motivo pelo qual nenhum produto é mostrado é porque, por padrão, uma cadeia de caracteres vazia para um valor de parâmetro é convertida em um banco de dados `NULL` valor. Como a comparação de `[UnitPrice] <= NULL` sempre é avaliada como false, nenhum resultado é retornado.

Insira um valor na caixa de texto, como 5, 0, e clique no botão Exibir produtos correspondentes. No postback, o SqlDataSource informa ao GridView que uma de suas origens de parâmetro foi alterada. Consequentemente, o GridView é reassociado ao SqlDataSource, exibindo os produtos menores ou iguais a $5.

[![produtos menores ou iguais a $5 são exibidos](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.png)

**Figura 7**: produtos menores ou iguais a $5 são exibidos ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.png))

## <a name="initially-displaying-all-products"></a>Exibindo inicialmente todos os produtos

Em vez de não exibir nenhum produto quando a página é carregada pela primeira vez, talvez queiramos exibir *todos os* produtos. Uma maneira de listar todos os produtos sempre que a caixa de texto `MaxPrice` está vazia é definir o valor padrão do parâmetro como um valor alto incrivelmente, como 1 milhão, já que o Northwind Traders nunca terá inventário cujo preço unitário exceda $1000000. No entanto, essa abordagem é falta e pode não funcionar em outras situações.

Nos tutoriais anteriores- [parâmetros declarativos](../basic-reporting/declarative-parameters-vb.md) e [filtragem de mestre/detalhes com uma DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) , estávamos enfrentando um problema semelhante. Nossa solução foi colocar essa lógica na camada de lógica de negócios. Especificamente, a BLL examinou o valor de entrada e, se foi `NULL` ou algum valor reservado, a chamada foi roteada para o método DAL que retornou todos os registros. Se o valor de entrada era um valor de filtragem normal, uma chamada foi feita ao método DAL que executou uma instrução SQL que usava uma cláusula de `WHERE` parametrizada com o valor fornecido.

Infelizmente, ignoramos a arquitetura ao usar o SqlDataSource. Em vez disso, precisamos personalizar a instrução SQL para capturar todos os registros de forma inteligente se o parâmetro `@MaximumPrice` for `NULL` ou algum valor reservado. Para este exercício, vamos fazer com que, se o parâmetro `@MaximumPrice` for igual a `-1.0`, *todos* os registros serão retornados (`-1.0` funcionará como um valor reservado, pois nenhum produto pode ter um valor de `UnitPrice` negativo). Para fazer isso, podemos usar a seguinte instrução SQL:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample7.sql)]

Essa cláusula `WHERE` retornará *todos os* registros se o parâmetro `@MaximumPrice` for igual a `-1.0`. Se o valor do parâmetro não for `-1.0`, somente os produtos cujo `UnitPrice` seja menor ou igual ao valor do parâmetro `@MaximumPrice` serão retornados. Ao definir o valor padrão do parâmetro `@MaximumPrice` como `-1.0`, na primeira carga da página (ou sempre que a caixa de texto `MaxPrice` estiver vazia), `@MaximumPrice` terá um valor de `-1.0` e todos os produtos serão exibidos.

[![agora todos os produtos são exibidos quando a caixa de texto MaxPrice está vazia](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.png)

**Figura 8**: agora todos os produtos são exibidos quando a caixa de texto `MaxPrice` está vazia ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image16.png))

Há algumas advertências a serem observadas com essa abordagem. Em primeiro lugar, perceba que o tipo de dados do parâmetro é inferido pelo uso do s na consulta SQL. Se você alterar a cláusula `WHERE` de `@MaximumPrice = -1.0` para `@MaximumPrice = -1`, o tempo de execução tratará o parâmetro como um inteiro. Se você tentar atribuir a caixa de texto `MaxPrice` a um valor decimal (como 5, 0), ocorrerá um erro porque ele não pode converter 5, 0 em um inteiro. Para corrigir isso, certifique-se de usar `@MaximumPrice = -1.0` na cláusula `WHERE` ou, melhor ainda, defina a propriedade `ControlParameter` objeto s `Type` como Decimal.

Em segundo lugar, ao adicionar o `OR @MaximumPrice = -1.0` à cláusula `WHERE`, o mecanismo de consulta não pode usar um índice em `UnitPrice` (supondo que exista um), resultando assim em uma verificação de tabela. Isso pode afetar o desempenho se houver um número suficientemente grande de registros na tabela `Products`. Uma abordagem melhor seria mover essa lógica para um procedimento armazenado em que uma instrução `IF` executaria uma consulta `SELECT` da tabela `Products` sem uma cláusula `WHERE` quando todos os registros precisarem ser retornados ou um cuja cláusula `WHERE` contiver apenas os critérios de `UnitPrice`, para que um índice possa ser usado.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Etapa 3: Criando e usando procedimentos armazenados com parâmetros

Os procedimentos armazenados podem incluir um conjunto de parâmetros de entrada que podem ser usados nas instruções SQL definidas no procedimento armazenado. Ao configurar o SqlDataSource para usar um procedimento armazenado que aceita parâmetros de entrada, esses valores de parâmetro podem ser especificados usando as mesmas técnicas que as instruções SQL ad hoc.

Para ilustrar o uso de procedimentos armazenados no SqlDataSource, vamos criar um novo procedimento armazenado no banco de dados Northwind chamado `GetProductsByCategory`, que aceita um parâmetro chamado `@CategoryID` e retorna todas as colunas dos produtos cuja coluna `CategoryID` corresponde a `@CategoryID`. Para criar um procedimento armazenado, vá para a Gerenciador de Servidores e faça uma busca detalhada no banco de dados `NORTHWND.MDF`. (Se você não vir o Gerenciador de Servidores, exiba-o indo para o menu exibir e selecione a opção Gerenciador de Servidores.)

No banco de dados `NORTHWND.MDF`, clique com o botão direito do mouse na pasta procedimentos armazenados, escolha Adicionar novo procedimento armazenado e digite a seguinte sintaxe:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample8.sql)]

Clique no ícone salvar (ou CTRL + S) para salvar o procedimento armazenado. Você pode testar o procedimento armazenado clicando com o botão direito do mouse na pasta procedimentos armazenados e escolhendo executar. Isso solicitará os parâmetros s do procedimento armazenado (`@CategoryID`, nesta instância), após o qual os resultados serão exibidos na janela de saída.

[![o procedimento armazenado GetProductsByCategory quando executado com um @CategoryID de 1](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image17.png)

**Figura 9**: o `GetProductsByCategory` procedimento armazenado quando executado com um `@CategoryID` de 1 ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image18.png))

Deixe que o s use esse procedimento armazenado para exibir todos os produtos na categoria bebidas em um GridView. Adicione um novo GridView à página e associe-o a um novo SqlDataSource nomeado `BeverageProductsDataSource`. Continue na tela especificar uma instrução SQL personalizada ou um procedimento armazenado, selecione o botão de opção procedimento armazenado e escolha o `GetProductsByCategory` procedimento armazenado na lista suspensa.

[![selecionar o procedimento armazenado GetProductsByCategory na lista suspensa](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image19.png)

**Figura 10**: selecione o `GetProductsByCategory` procedimento armazenado na lista suspensa ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image20.png))

Como o procedimento armazenado aceita um parâmetro de entrada (`@CategoryID`), clicar em Avançar solicita que especifiquemos a origem para esse valor de s de parâmetro. As bebidas `CategoryID` são 1, portanto, deixe a lista suspensa origem do parâmetro em nenhum e insira 1 na caixa de texto DefaultValue.

[![usar um valor embutido em código de 1 para retornar os produtos na categoria bebidas](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image21.png)

**Figura 11**: Use um valor embutido em código de 1 para retornar os produtos na categoria bebidas ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image22.png))

Como a marcação declarativa a seguir mostra, ao usar um procedimento armazenado, a Propriedade SqlDataSource s `SelectCommand` é definida como o nome do procedimento armazenado e a [propriedade`SelectCommandType`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) é definida como `StoredProcedure`, indicando que o `SelectCommand` é o nome de um procedimento armazenado em vez de uma instrução SQL ad hoc.

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample9.aspx)]

Teste a página em um navegador. Somente os produtos que pertencem à categoria bebidas são exibidos, embora *todos* os campos de produto sejam exibidos, pois o procedimento armazenado `GetProductsByCategory` retorna todas as colunas da tabela `Products`. Podemos, naturalmente, limitar ou personalizar os campos exibidos no GridView na caixa de diálogo Editar colunas de GridView.

[![todas as bebidas são exibidas](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image23.png)

**Figura 12**: todas as bebidas são exibidas ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image24.png))

## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Etapa 4: invocando programaticamente uma instrução SqlDataSource s`Select()`

Os exemplos que vimos no tutorial anterior e este tutorial até agora têm vinculado os controles SqlDataSource diretamente a um GridView. No entanto, os dados do controle SqlDataSource podem ser acessados e enumerados por meio de programação no código. Isso pode ser particularmente útil quando você precisa consultar dados para inspecioná-la, mas Don t precisa exibi-la. Em vez de escrever todo o código de ADO.NET clichê para se conectar ao banco de dados, especificar o comando e recuperar os resultados, você pode deixar que o indicador de SqlDataSource trate desse código de monótono.

Para ilustrar como trabalhar com os dados de SqlDataSource s programaticamente, imagine que seu chefe o abordou com uma solicitação para criar uma página da Web que exibe o nome de uma categoria selecionada aleatoriamente e seus produtos associados. Ou seja, quando um usuário visita essa página, queremos escolher aleatoriamente uma categoria na tabela `Categories`, exibir o nome da categoria e, em seguida, listar os produtos que pertencem a essa categoria.

Para fazer isso, precisamos de dois controles SqlDataSource um para pegar uma categoria aleatória da tabela de `Categories` e outra para obter os produtos da categoria. Vamos criar o SqlDataSource que recupera um registro de categoria aleatório nesta etapa; A etapa 5 examina a criação do SqlDataSource que recupera os produtos da categoria.

Comece adicionando um SqlDataSource a `ParameterizedQueries.aspx` e defina seu `ID` como `RandomCategoryDataSource`. Configure-o para que ele use a seguinte consulta SQL:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample10.sql)]

`ORDER BY NEWID()` retorna os registros classificados em ordem aleatória (consulte [usando `NEWID()` para classificar registros aleatoriamente](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1` retorna o primeiro registro do conjunto de resultados. Juntas, essa consulta retorna o `CategoryID` e `CategoryName` valores de coluna de uma única categoria selecionada aleatoriamente.

Para exibir a categoria `CategoryName` valor, adicione um controle rótulo da Web à página, defina sua propriedade `ID` como `CategoryNameLabel`e desmarque sua propriedade `Text`. Para recuperar programaticamente os dados de um controle SqlDataSource, precisamos invocar seu método `Select()`. O [método`Select()`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) espera um único parâmetro de entrada do tipo [`DataSourceSelectArguments`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), que especifica como os dados devem ser transformados antes de serem retornados. Isso pode incluir instruções sobre como classificar e filtrar os dados e é usado pelos controles da Web de dados ao classificar ou paginar os dados de um controle SqlDataSource. No entanto, para nosso exemplo, não precisamos que os dados sejam modificados antes de serem retornados e, portanto, passarão pelo objeto `DataSourceSelectArguments.Empty`.

O método `Select()` retorna um objeto que implementa `IEnumerable`. O tipo preciso retornado depende do valor da Propriedade SqlDataSource Control s [`DataSourceMode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx). Conforme discutido no tutorial anterior, essa propriedade pode ser definida com um valor de `DataSet` ou `DataReader`. Se definido como `DataSet`, o método `Select()` retornará um objeto [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx) ; Se definido como `DataReader`, ele retorna um objeto que implementa [`IDataReader`](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Como o `RandomCategoryDataSource` SqlDataSource tem sua propriedade `DataSourceMode` definida como `DataSet` (o padrão), iremos trabalhar com um objeto DataView.

O código a seguir ilustra como recuperar os registros do `RandomCategoryDataSource` SqlDataSource como um DataView, bem como ler o valor da coluna `CategoryName` da primeira linha DataView:

[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample11.vb)]

`randomCategoryView(0)` retorna a primeira `DataRowView` no DataView. `randomCategoryView(0)("CategoryName")` retorna o valor da coluna `CategoryName` nesta primeira linha. Observe que o DataView está com rigidez de tipos. Para fazer referência a um valor de coluna específico, precisamos passar o nome da coluna como uma cadeia de caracteres (NomeDaCategoria, nesse caso). A Figura 13 mostra a mensagem exibida no `CategoryNameLabel` ao exibir a página. É claro que o nome da categoria real exibido é selecionado aleatoriamente pelo `RandomCategoryDataSource` SqlDataSource em cada visita à página (incluindo postbacks).

[![o nome da categoria selecionada aleatoriamente é exibido](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image25.png)

**Figura 13**: o nome da categoria selecionada aleatoriamente é exibido ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image26.png))

> [!NOTE]
> Se a Propriedade SqlDataSource Control s `DataSourceMode` tivesse sido definida como `DataReader`, o valor de retorno do método `Select()` teria necessário ser convertido em `IDataReader`. Para ler o valor da coluna `CategoryName` da primeira linha, nós d usamos código como:

[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample12.vb)]

Com o SqlDataSource selecionando aleatoriamente uma categoria, estamos prontos para adicionar o GridView que lista os produtos da categoria s.

> [!NOTE]
> Em vez de usar um controle de rótulo da Web para exibir o nome da categoria, poderíamos ter adicionado um FormView ou DetailsView à página, associando-o ao SqlDataSource. No entanto, usar o rótulo nos permitia explorar como invocar a instrução SqlDataSource s `Select()` de forma programática e trabalhar com seus dados resultantes no código.

## <a name="step-5-assigning-parameter-values-programmatically"></a>Etapa 5: atribuindo valores de parâmetro programaticamente

Todos os exemplos que vimos até agora neste tutorial usaram um valor de parâmetro embutido em código ou um extraído de uma das fontes de parâmetro predefinidas (um valor de QueryString, um controle da Web na página e assim por diante). No entanto, os parâmetros do controle SqlDataSource s também podem ser definidos programaticamente. Para concluir nosso exemplo atual, precisamos de um SqlDataSource que retorna todos os produtos que pertencem a uma categoria especificada. Esse SqlDataSource terá um parâmetro `CategoryID` cujo valor precisa ser definido com base no valor da coluna `CategoryID` retornado pelo `RandomCategoryDataSource` SqlDataSource no manipulador de eventos `Page_Load`.

Comece adicionando um GridView à página e associe-o a um novo SqlDataSource nomeado `ProductsByCategoryDataSource`. Da mesma forma que fizemos na etapa 3, configurei o SqlDataSource para invocar o procedimento armazenado `GetProductsByCategory`. Deixe a lista suspensa origem do parâmetro definida como nenhum, mas não insira um valor padrão, pois vamos definir esse valor padrão programaticamente.

[![não especificar uma origem de parâmetro ou um valor padrão](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image27.png)

**Figura 14**: não especifique uma origem de parâmetro ou um valor padrão ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image28.png))

Depois de concluir o assistente SqlDataSource, a marcação declarativa resultante deve ser semelhante ao seguinte:

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample13.aspx)]

Podemos atribuir a `DefaultValue` do parâmetro `CategoryID` programaticamente no manipulador de eventos `Page_Load`:

[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample14.vb)]

Com essa adição, a página inclui um GridView que mostra os produtos associados à categoria selecionada aleatoriamente.

[![não especificar uma origem de parâmetro ou um valor padrão](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image29.png)

**Figura 15**: não especifique uma origem de parâmetro ou um valor padrão ([clique para exibir a imagem em tamanho normal](using-parameterized-queries-with-the-sqldatasource-vb/_static/image30.png))

## <a name="summary"></a>Resumo

O SqlDataSource permite que os desenvolvedores de página definam consultas parametrizadas cujos valores de parâmetro possam ser embutidos em código, extraídos de fontes de parâmetro predefinidas ou atribuídos programaticamente. Neste tutorial, vimos como criar uma consulta parametrizada do assistente para configurar fonte de dados para consultas SQL ad hoc e procedimentos armazenados. Também vimos o uso de fontes de parâmetro embutidas em código, um controle da Web como uma origem de parâmetro e a especificação programática do valor do parâmetro.

Assim como com o ObjectDataSource, o SqlDataSource também fornece recursos para modificar seus dados subjacentes. No próximo tutorial, veremos como definir `INSERT`, `UPDATE`e `DELETE` instruções com o SqlDataSource. Depois que essas instruções tiverem sido adicionadas, podemos utilizar os recursos internos de inserção, edição e exclusão inerentes aos controles GridView, DetailsView e FormView.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Scott Clyde, Randell Schmidt e Ken Pespisa. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](querying-data-with-the-sqldatasource-control-vb.md)
> [Próximo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
