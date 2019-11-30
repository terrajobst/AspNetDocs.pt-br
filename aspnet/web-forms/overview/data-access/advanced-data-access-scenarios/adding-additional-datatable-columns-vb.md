---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
title: Adicionando colunas DataTable adicionais (VB) | Microsoft Docs
author: rick-anderson
description: Ao usar o assistente TableAdapter para criar um conjunto de dados tipado, a DataTable correspondente contém as colunas retornadas pela consulta de banco de dados principal. Mas lá...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 1e8e65f9-fe3e-4250-810b-c90227786bed
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: 3a55f8bc4d3508387927ca81674073a001867de7
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74608248"
---
# <a name="adding-additional-datatable-columns-vb"></a>Adicionar mais colunas DataTable (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_VB.zip) ou [baixar PDF](adding-additional-datatable-columns-vb/_static/datatutorial70vb1.pdf)

> Ao usar o assistente TableAdapter para criar um conjunto de dados tipado, a DataTable correspondente contém as colunas retornadas pela consulta de banco de dados principal. Mas há ocasiões em que a DataTable precisa incluir colunas adicionais. Neste tutorial, aprendemos por que procedimentos armazenados são recomendados quando precisamos de colunas de DataTable adicionais.

## <a name="introduction"></a>Introdução

Ao adicionar um TableAdapter a um DataSet tipado, o esquema DataTable s correspondente é determinado pela consulta principal do TableAdapter s. Por exemplo, se a consulta principal retornar os campos *de dados A*, *b*e *c*, a DataTable terá três colunas correspondentes chamadas *A*, *b*e *c*. Além de sua consulta principal, um TableAdapter pode incluir consultas adicionais que retornam, talvez, um subconjunto dos dados com base em algum parâmetro. Por exemplo, além da consulta principal `ProductsTableAdapter` s, que retorna informações sobre todos os produtos, ele também contém métodos como `GetProductsByCategoryID(categoryID)` e `GetProductByProductID(productID)`, que retornam informações específicas do produto com base em um parâmetro fornecido.

O modelo de ter o esquema DataTable s reflete a consulta principal TableAdapter s funciona bem se todos os métodos TableAdapter s retornarem os mesmos ou menos campos de dados do que os especificados na consulta principal. Se um método TableAdapter precisar retornar campos de dados adicionais, então, devemos expandir o esquema DataTable s de acordo. No [mestre/detalhes usando uma lista com marcadores de registros mestres com um tutorial de DataList detalhes](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) , adicionamos um método ao `CategoriesTableAdapter` que retornava os campos de dados `CategoryID`, `CategoryName`e `Description` definidos na consulta principal mais `NumberOfProducts`, um campo de dados adicional que reportou o número de produtos associados a cada categoria. Adicionamos manualmente uma nova coluna à `CategoriesDataTable` para capturar o valor do campo de dados de `NumberOfProducts` desse novo método.

Conforme discutido no tutorial [carregar arquivos](../working-with-binary-files/uploading-files-vb.md) , é preciso ter muito cuidado com os TableAdapters que usam instruções SQL ad hoc e têm métodos cujos campos de dados não correspondem precisamente à consulta principal. Se o assistente de configuração do TableAdapter for executado novamente, ele atualizará todos os métodos TableAdapter s para que sua lista de campos de dados corresponda à consulta principal. Consequentemente, todos os métodos com listas de colunas personalizadas serão revertidos para a lista de colunas da consulta principal e não retornarão os dados esperados. Esse problema não ocorre ao usar procedimentos armazenados.

Neste tutorial, veremos como estender um esquema DataTable s para incluir colunas adicionais. Devido à fragilidade do TableAdapter ao usar instruções SQL ad hoc, neste tutorial, usaremos procedimentos armazenados. Consulte a [criando novos procedimentos armazenados para os TableAdapters do conjunto de dados tipados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) e [usando procedimentos armazenados existentes para os tutoriais dos TableAdapters do DataSet tipados](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) para obter mais informações sobre como configurar um TableAdapter para usar procedimentos armazenados.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Etapa 1: adicionando uma coluna de`PriceQuartile`à`ProductsDataTable`

No tutorial *criando novos procedimentos armazenados para o DataSet s tipados* , criamos um conjunto de tipos chamado `NorthwindWithSprocs`. No momento, esse conjunto de conteúdo contém duas datatabelas: `ProductsDataTable` e `EmployeesDataTable`. O `ProductsTableAdapter` tem os três métodos a seguir:

- `GetProducts`-a consulta principal, que retorna todos os registros da tabela `Products`
- `GetProductsByCategoryID(categoryID)`-retorna todos os produtos com a *CategoryID*especificada.
- `GetProductByProductID(productID)`-retorna o produto específico com o *ProductID*especificado.

A consulta principal e os dois métodos adicionais retornam o mesmo conjunto de campos de dados, ou seja, todas as colunas da tabela `Products`. Não há Subconsultas correlacionadas ou `JOIN` s recebendo dados relacionados das tabelas `Categories` ou `Suppliers`. Portanto, o `ProductsDataTable` tem uma coluna correspondente para cada campo na tabela `Products`.

Para este tutorial, vamos adicionar um método ao `ProductsTableAdapter` chamado `GetProductsWithPriceQuartile` que retorna todos os produtos. Além dos campos de dados do produto padrão, `GetProductsWithPriceQuartile` também incluirá um campo de dados de `PriceQuartile` que indica sob qual quartil o preço do produto se enquadra. Por exemplo, os produtos cujos preços estão nos 25% mais caros terão um valor de `PriceQuartile` de 1, enquanto aqueles cujos preços se enquadram nos 25% inferiores terão um valor de 4. Antes de nos preocuparmos em criar o procedimento armazenado para retornar essas informações, no entanto, primeiro precisamos atualizar o `ProductsDataTable` para incluir uma coluna para manter os resultados da `PriceQuartile` quando o método `GetProductsWithPriceQuartile` for usado.

Abra o conjunto de `NorthwindWithSprocs` DataSet e clique com o botão direito do mouse na `ProductsDataTable`. Escolha Adicionar no menu de contexto e, em seguida, selecione coluna.

[![adicionar uma nova coluna ao ProductsDataTable](adding-additional-datatable-columns-vb/_static/image2.png)](adding-additional-datatable-columns-vb/_static/image1.png)

**Figura 1**: adicionar uma nova coluna à `ProductsDataTable` ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-vb/_static/image3.png))

Isso adicionará uma nova coluna à DataTable chamada Coluna1 do tipo `System.String`. Precisamos atualizar esse nome de coluna para PriceQuartile e seu tipo para `System.Int32`, pois ele será usado para manter um número entre 1 e 4. Selecione a coluna recém-adicionada no `ProductsDataTable` e, na janela Propriedades, defina a propriedade `Name` como PriceQuartile e a propriedade `DataType` como `System.Int32`.

[![definir as propriedades nome e tipo de dados da nova coluna](adding-additional-datatable-columns-vb/_static/image5.png)](adding-additional-datatable-columns-vb/_static/image4.png)

**Figura 2**: definir as propriedades da nova coluna s `Name` e `DataType` ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-vb/_static/image6.png))

Como mostra a Figura 2, há propriedades adicionais que podem ser definidas, como se os valores na coluna devem ser exclusivos, se a coluna for uma coluna de incremento automático, se o banco de dados `NULL` valores são permitidos e assim por diante. Deixe esses valores definidos como seus padrões.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Etapa 2: criando o método de`GetProductsWithPriceQuartile`

Agora que o `ProductsDataTable` foi atualizado para incluir a coluna `PriceQuartile`, estamos prontos para criar o método `GetProductsWithPriceQuartile`. Comece clicando com o botão direito do mouse no TableAdapter e escolhendo Adicionar consulta no menu de contexto. Isso abre o assistente de configuração de consulta do TableAdapter, que primeiro nos solicita se desejamos usar instruções SQL ad hoc ou um procedimento armazenado novo ou existente. Como ainda não temos um procedimento armazenado que retorna os dados do preço quartil, vamos permitir que o TableAdapter crie esse procedimento armazenado para nós. Selecione a opção Criar novo procedimento armazenado e clique em Avançar.

[![instrua o assistente TableAdapter a criar o procedimento armazenado para nós](adding-additional-datatable-columns-vb/_static/image8.png)](adding-additional-datatable-columns-vb/_static/image7.png)

**Figura 3**: Instrua o assistente do TableAdapter a criar o procedimento armazenado para nós ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-vb/_static/image9.png))

Na tela subsequente, mostrada na Figura 4, o assistente nos pergunta que tipo de consulta adicionar. Como o método `GetProductsWithPriceQuartile` retornará todas as colunas e registros da tabela `Products`, selecione a opção Selecionar que retorna linhas e clique em Avançar.

[![nossa consulta será uma instrução SELECT que retorna várias linhas](adding-additional-datatable-columns-vb/_static/image11.png)](adding-additional-datatable-columns-vb/_static/image10.png)

**Figura 4**: nossa consulta será uma instrução `SELECT` que retorna várias linhas ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-vb/_static/image12.png))

Em seguida, solicitamos a consulta `SELECT`. Insira a seguinte consulta no assistente:

[!code-sql[Main](adding-additional-datatable-columns-vb/samples/sample1.sql)]

A consulta acima usa SQL Server a nova [função`NTILE`](https://msdn.microsoft.com/library/ms175126.aspx) do 2005 para dividir os resultados em quatro grupos nos quais os grupos são determinados pelos valores de `UnitPrice` classificados em ordem decrescente.

Infelizmente, o Construtor de Consultas não sabe como analisar a palavra-chave `OVER` e exibirá um erro ao analisar a consulta acima. Portanto, insira a consulta acima diretamente na caixa de texto no assistente sem usar o Construtor de Consultas.

> [!NOTE]
> Para obter mais informações sobre as outras funções de classificação do NTILE e SQL Server 2005 s, consulte [retornando resultados classificados com Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) e a [seção funções de classificação](https://msdn.microsoft.com/library/ms189798.aspx) dos [manuais online do SQL Server 2005](https://msdn.microsoft.com/library/ms189798.aspx).

Depois de inserir a consulta `SELECT` e clicar em avançar, o assistente nos solicitará que forneça um nome para o procedimento armazenado que ele criará. Nomeie o novo procedimento armazenado `Products_SelectWithPriceQuartile` e clique em Avançar.

[![nomear o procedimento armazenado Products_SelectWithPriceQuartile](adding-additional-datatable-columns-vb/_static/image14.png)](adding-additional-datatable-columns-vb/_static/image13.png)

**Figura 5**: nomear o procedimento armazenado `Products_SelectWithPriceQuartile` ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-vb/_static/image15.png))

Por fim, são solicitados a nomear os métodos TableAdapter. Deixe as caixas de seleção preencher uma DataTable e retornar uma DataTable marcadas e nomeie os métodos `FillWithPriceQuartile` e `GetProductsWithPriceQuartile`.

[![nomear os métodos TableAdapter s e clicar em concluir](adding-additional-datatable-columns-vb/_static/image17.png)](adding-additional-datatable-columns-vb/_static/image16.png)

**Figura 6**: Nomeie os métodos TableAdapter s e clique em concluir ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-vb/_static/image18.png))

Com a consulta `SELECT` especificada e os métodos de procedimento armazenado e TableAdapter chamados, clique em concluir para concluir o assistente. Neste ponto, você pode receber um ou dois avisos do assistente informando que não há suporte para a construção ou instrução SQL `OVER`. Esses avisos podem ser ignorados.

Depois de concluir o assistente, o TableAdapter deve incluir os métodos `FillWithPriceQuartile` e `GetProductsWithPriceQuartile` e o banco de dados deve incluir um procedimento armazenado chamado `Products_SelectWithPriceQuartile`. Reserve um tempo para verificar se o TableAdapter realmente contém esse novo método e se o procedimento armazenado foi adicionado corretamente ao banco de dados. Ao verificar o banco de dados, se você não vir o procedimento armazenado, tente clicar com o botão direito do mouse na pasta procedimentos armazenados e escolher atualizar.

![Verifique se um novo método foi adicionado ao TableAdapter](adding-additional-datatable-columns-vb/_static/image19.png)

**Figura 7**: verificar se um novo método foi adicionado ao TableAdapter

[![garantir que o banco de dados contenha o procedimento armazenado Products_SelectWithPriceQuartile](adding-additional-datatable-columns-vb/_static/image21.png)](adding-additional-datatable-columns-vb/_static/image20.png)

**Figura 8**: Verifique se o banco de dados contém o procedimento armazenado `Products_SelectWithPriceQuartile` ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-vb/_static/image22.png))

> [!NOTE]
> Um dos benefícios de usar procedimentos armazenados em vez de instruções SQL ad hoc é que executar novamente o assistente de configuração do TableAdapter não modificará as listas de colunas de procedimentos armazenados. Verifique isso clicando com o botão direito do mouse no TableAdapter, escolhendo a opção configurar no menu de contexto para iniciar o assistente e, em seguida, clicando em concluir para concluí-lo. Em seguida, vá para o banco de dados e exiba o procedimento armazenado `Products_SelectWithPriceQuartile`. Observe que sua lista de colunas não foi modificada. Estava usando instruções SQL ad hoc, executar novamente o assistente de configuração do TableAdapter reverteria essa lista de colunas de consulta para corresponder à lista de colunas da consulta principal, removendo assim a instrução NTILE da consulta usada pelo método `GetProductsWithPriceQuartile`.

Quando o método de `GetProductsWithPriceQuartile` da camada de acesso a dados é invocado, o TableAdapter executa o procedimento armazenado `Products_SelectWithPriceQuartile` e adiciona uma linha à `ProductsDataTable` para cada registro retornado. Os campos de dados retornados pelo procedimento armazenado são mapeados para as colunas `ProductsDataTable` s. Como há um campo de dados `PriceQuartile` retornado do procedimento armazenado, seu valor é atribuído à coluna `ProductsDataTable` s `PriceQuartile`.

Para esses métodos TableAdapter cujas consultas não retornam um campo de dados `PriceQuartile`, o valor de s da coluna `PriceQuartile` é o valor especificado por sua propriedade `DefaultValue`. Como mostra a Figura 2, esse valor é definido como `DBNull`, o padrão. Se você preferir um valor padrão diferente, basta definir a propriedade `DefaultValue` de acordo. Apenas certifique-se de que o valor de `DefaultValue` é válido, dado à coluna s `DataType` (ou seja, `System.Int32` para a coluna `PriceQuartile`).

Neste ponto, realizamos as etapas necessárias para adicionar uma coluna adicional a uma DataTable. Para verificar se essa coluna adicional funciona conforme o esperado, vamos criar uma página ASP.NET que exibe cada nome de produto, preço e preço quartil. No entanto, antes de fazermos isso, primeiro precisamos atualizar a camada de lógica de negócios para incluir um método que chame para o método da DAL s `GetProductsWithPriceQuartile`. Atualizaremos a BLL em seguida, na etapa 3, e, em seguida, criaremos a página ASP.NET na etapa 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Etapa 3: aumentando a camada de lógica de negócios

Antes de usarmos o novo método `GetProductsWithPriceQuartile` da camada de apresentação, primeiro devemos adicionar um método correspondente à BLL. Abra o arquivo de classe `ProductsBLLWithSprocs` e adicione o seguinte código:

[!code-vb[Main](adding-additional-datatable-columns-vb/samples/sample2.vb)]

Como os outros métodos de recuperação de dados no `ProductsBLLWithSprocs`, o método `GetProductsWithPriceQuartile` simplesmente chama o método `GetProductsWithPriceQuartile` DAL s correspondente e retorna seus resultados.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Etapa 4: exibindo as informações do quartil de preço em uma página da Web do ASP.NET

Com a adição de BLL concluída, estamos prontos para criar uma página ASP.NET que mostra o preço quartil para cada produto. Abra a página `AddingColumns.aspx` na pasta `AdvancedDAL` e arraste um GridView da caixa de ferramentas para o designer, definindo sua propriedade `ID` como `Products`. Na marca inteligente s GridView, associe-o a um novo ObjectDataSource chamado `ProductsDataSource`. Configure o ObjectDataSource para usar o método de `GetProductsWithPriceQuartile` da classe `ProductsBLLWithSprocs`. Como essa será uma grade somente leitura, defina as listas suspensas nas guias atualizar, inserir e excluir como (nenhum).

[![configurar o ObjectDataSource para usar a classe ProductsBLLWithSprocs](adding-additional-datatable-columns-vb/_static/image24.png)](adding-additional-datatable-columns-vb/_static/image23.png)

**Figura 9**: configurar o ObjectDataSource para usar a classe `ProductsBLLWithSprocs` ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-vb/_static/image25.png))

[![recuperar informações do produto do método GetProductsWithPriceQuartile](adding-additional-datatable-columns-vb/_static/image27.png)](adding-additional-datatable-columns-vb/_static/image26.png)

**Figura 10**: recuperar informações do produto do método `GetProductsWithPriceQuartile` ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-vb/_static/image28.png))

Depois de concluir o assistente para configurar fonte de dados, o Visual Studio adicionará automaticamente um BoundField ou CheckBoxField ao GridView para cada um dos campos de dados retornados pelo método. Um desses campos de dados é `PriceQuartile`, que é a coluna que adicionamos ao `ProductsDataTable` na etapa 1.

Edite os campos GridView, removendo todos os `ProductName`, `UnitPrice`e `PriceQuartile` BoundFields. Configure o `UnitPrice` BoundField para formatar seu valor como uma moeda e ter os `UnitPrice` e `PriceQuartile` BoundFields à direita e centralizado, respectivamente. Por fim, atualize os BoundFields restantes `HeaderText` Propriedades para produto, preço e preço quartil, respectivamente. Além disso, marque a caixa de seleção Habilitar classificação na marca inteligente s GridView.

Após essas modificações, a marcação declarativa de GridView e ObjectDataSource s deve ser parecida com a seguinte:

[!code-aspx[Main](adding-additional-datatable-columns-vb/samples/sample3.aspx)]

A Figura 11 mostra essa página quando visitada por meio de um navegador. Observe que, inicialmente, os produtos são ordenados por seu preço em ordem decrescente, com cada produto atribuído um valor de `PriceQuartile` apropriado. É claro que esses dados podem ser classificados por outros critérios com o valor da coluna Price quartil ainda refletindo a classificação do produto em relação ao preço (consulte a Figura 12).

[![os produtos são ordenados por seus preços](adding-additional-datatable-columns-vb/_static/image30.png)](adding-additional-datatable-columns-vb/_static/image29.png)

**Figura 11**: os produtos são ordenados por seus preços ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-vb/_static/image31.png))

[![os produtos são ordenados por seus nomes](adding-additional-datatable-columns-vb/_static/image33.png)](adding-additional-datatable-columns-vb/_static/image32.png)

**Figura 12**: os produtos são ordenados por seus nomes ([clique para exibir a imagem em tamanho normal](adding-additional-datatable-columns-vb/_static/image34.png))

> [!NOTE]
> Com algumas linhas de código, poderíamos ampliar o GridView para que ele tenha colorido as linhas do produto com base em seu valor `PriceQuartile`. Podemos colorir esses produtos no primeiro quartil um verde claro, aqueles no segundo quartil a amarelo claro e assim por diante. Recomendo que você reserve um tempo para adicionar essa funcionalidade. Se você precisar de um atualizador na formatação de um GridView, consulte o tutorial [formatação personalizada com base no data](../custom-formatting/custom-formatting-based-upon-data-vb.md) .

## <a name="an-alternative-approach---creating-another-tableadapter"></a>Uma abordagem alternativa – criar outro TableAdapter

Como vimos neste tutorial, ao adicionar um método a um TableAdapter que retorna campos de dados diferentes daqueles escritos pela consulta principal, podemos adicionar colunas correspondentes à DataTable. Essa abordagem, no entanto, funciona bem apenas se houver um pequeno número de métodos no TableAdapter que retornam campos de dados diferentes e se esses campos de dados alternativos não variarem muito da consulta principal.

Em vez de adicionar colunas à DataTable, você pode adicionar outro TableAdapter ao DataSet que contém os métodos do primeiro TableAdapter que retornam campos de dados diferentes. Para este tutorial, em vez de adicionar a coluna de `PriceQuartile` à `ProductsDataTable` (onde ela é usada apenas pelo método de `GetProductsWithPriceQuartile`), poderíamos ter adicionado um TableAdapter adicional ao conjunto de conjuntos chamado `ProductsWithPriceQuartileTableAdapter` que usava o procedimento armazenado `Products_SelectWithPriceQuartile` como sua consulta principal. ASP.NET páginas que precisavam obter informações de produto com o preço quartil usaria o `ProductsWithPriceQuartileTableAdapter`, enquanto aqueles que não podiam continuar a usar o `ProductsTableAdapter`.

Ao adicionar um novo TableAdapter, as tabelas de dados permanecem untarnished e suas colunas espelham com precisão os campos de dado retornados pelos seus métodos TableAdapter s. No entanto, TableAdapters adicionais podem introduzir tarefas repetitivas e funcionalidades. Por exemplo, se as páginas ASP.NET que exibirem a coluna `PriceQuartile` também precisarem fornecer suporte para inserir, atualizar e excluir, a `ProductsWithPriceQuartileTableAdapter` deverá ter suas propriedades `InsertCommand`, `UpdateCommand`e `DeleteCommand` configuradas corretamente. Embora essas propriedades espelhem o `ProductsTableAdapter` s, essa configuração apresenta uma etapa extra. Além disso, agora há duas maneiras de atualizar, excluir ou adicionar um produto ao banco de dados, por meio das classes `ProductsTableAdapter` e `ProductsWithPriceQuartileTableAdapter`.

O download para este tutorial inclui uma classe `ProductsWithPriceQuartileTableAdapter` no conjunto de `NorthwindWithSprocs` DataSet que ilustra essa abordagem alternativa.

## <a name="summary"></a>Resumo

Na maioria dos cenários, todos os métodos em um TableAdapter retornarão o mesmo conjunto de campos de dados, mas há ocasiões em que um determinado método ou dois pode precisar retornar um campo adicional. Por exemplo, no [mestre/detalhes usando uma lista com marcadores de registros mestres com um tutorial de DataList detalhes](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) , adicionamos um método ao `CategoriesTableAdapter` que, além dos campos de dados da consulta principal, retornava um campo `NumberOfProducts` que reportou o número de produtos associados a cada categoria. Neste tutorial, examinamos a adição de um método no `ProductsTableAdapter` que retornava um campo `PriceQuartile` além dos campos de dados da consulta principal. Para capturar campos de dados adicionais retornados pelos métodos TableAdapter s, precisamos adicionar colunas correspondentes à DataTable.

Se você planeja adicionar colunas manualmente à DataTable, é recomendável que o TableAdapter use procedimentos armazenados. Se o TableAdapter usar instruções SQL ad hoc, sempre que o assistente de configuração do TableAdapter for executado, todas as listas de campos de dados de métodos serão revertidas para os campos de dados retornados pela consulta principal. Esse problema não se estende aos procedimentos armazenados, que é o motivo pelo qual eles são recomendados e que foram usados neste tutorial.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Randy Schmidt, Jacky Goor, Bernadette Leigh e Hilton Giesenow. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](updating-the-tableadapter-to-use-joins-vb.md)
> [Próximo](working-with-computed-columns-vb.md)
