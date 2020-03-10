---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
title: Classificando dados personalizados paginados (C#) | Microsoft Docs
author: rick-anderson
description: No tutorial anterior, aprendemos como implementar a paginação personalizada ao apresentar dados em uma página da Web. Neste tutorial, vemos como estender o anterior...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 778baa4e-4af8-4665-947e-7a01d1a4dff2
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
msc.type: authoredcontent
ms.openlocfilehash: e55ed9b92814753e95bdfdf26c2f051df6f2630d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78589778"
---
# <a name="sorting-custom-paged-data-c"></a>Classificação de dados personalizados paginados (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_CS.exe) ou [baixar PDF](sorting-custom-paged-data-cs/_static/datatutorial26cs1.pdf)

> No tutorial anterior, aprendemos como implementar a paginação personalizada ao apresentar dados em uma página da Web. Neste tutorial, vemos como estender o exemplo anterior para incluir suporte à classificação de paginação personalizada.

## <a name="introduction"></a>Introdução

Em comparação com a paginação padrão, a paginação personalizada pode melhorar o desempenho da paginação por meio de dados por várias ordens de magnitude, tornando a paginação personalizada a opção de implementação da paginação de fato ao paginar por A implementação de paginação personalizada é mais envolvida do que a implementação de paginação padrão, no entanto, especialmente ao adicionar classificação à combinação. Neste tutorial, estenderemos o exemplo do anterior para incluir suporte para classificação *e* paginação personalizada.

> [!NOTE]
> Como este tutorial se baseia no anterior, antes de começar, Reserve um tempo para copiar a sintaxe declarativa dentro do elemento `<asp:Content>` da página da Web do tutorial anterior (`EfficientPaging.aspx`) e cole-a entre o elemento `<asp:Content>` na página `SortParameter.aspx`. Consulte novamente a etapa 1 do tutorial [adicionando controles de validação para as interfaces de edição e inserção](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) para obter uma discussão mais detalhada sobre como replicar a funcionalidade de uma página ASP.net para outra.

## <a name="step-1-reexamining-the-custom-paging-technique"></a>Etapa 1: reexaminando a técnica de paginação personalizada

Para que a paginação personalizada funcione corretamente, devemos implementar alguma técnica que possa pegar com eficiência um subconjunto específico de registros, de acordo com o índice de linha inicial e os parâmetros de linhas máximas. Há algumas técnicas que podem ser usadas para atingir esse objetivo. No tutorial anterior, vimos fazer isso usando Microsoft SQL Server nova função de classificação de `ROW_NUMBER()` de 2005. Em suma, a função de classificação `ROW_NUMBER()` atribui um número de linha a cada linha retornada por uma consulta que é classificada por uma ordem de classificação especificada. O subconjunto apropriado de registros é obtido retornando uma seção específica dos resultados numerados. A consulta a seguir ilustra como usar essa técnica para retornar os produtos numerados de 11 a 20 ao classificar os resultados ordenados alfabeticamente pelo `ProductName`:

[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample1.sql)]

Essa técnica funciona bem para paginação usando uma ordem de classificação específica (`ProductName` classificada alfabeticamente, nesse caso), mas a consulta precisa ser modificada para mostrar os resultados classificados por uma expressão de classificação diferente. O ideal é que a consulta acima pudesse ser reescrita para usar um parâmetro na cláusula `OVER`, desta forma:

[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample2.sql)]

Infelizmente, não são permitidas cláusulas de `ORDER BY` parametrizadas. Em vez disso, devemos criar um procedimento armazenado que aceita um parâmetro de entrada `@sortExpression`, mas usa uma das seguintes soluções alternativas:

- Escreva consultas embutidas em código para cada uma das expressões de classificação que podem ser usadas; em seguida, use `IF/ELSE` instruções T-SQL para determinar qual consulta deve ser executada.
- Use uma instrução `CASE` para fornecer expressões `ORDER BY` dinâmicas com base no parâmetro de entrada `@sortExpressio` n; consulte a seção usado para classificar dinamicamente os resultados da consulta no [Power of SQL `CASE` Statements](http://www.4guysfromrolla.com/webtech/102704-1.shtml) para obter mais informações.
- Crie a consulta apropriada como uma cadeia de caracteres no procedimento armazenado e, em seguida, use [o procedimento armazenado do sistema `sp_executesql`](https://msdn.microsoft.com/library/ms188001.aspx) para executar a consulta dinâmica.

Cada uma dessas soluções alternativas tem algumas desvantagens. A primeira opção não é tão passível de manutenção quanto as outras duas, pois requer que você crie uma consulta para cada expressão de classificação possível. Portanto, se você decidir posteriormente adicionar novos campos classificável ao GridView, também precisará voltar e atualizar o procedimento armazenado. A segunda abordagem tem algumas sutilezas que apresentam preocupações de desempenho ao classificar por colunas de banco de dados que não são de cadeia de caracteres e também sofrem com os mesmos problemas de manutenção que o primeiro. E a terceira opção, que usa o SQL dinâmico, apresenta o risco para um ataque de injeção de SQL se um invasor for capaz de executar o procedimento armazenado passando os valores de parâmetro de entrada de sua escolha.

Embora nenhuma dessas abordagens seja perfeita, acho que a terceira opção é a melhor das três. Com o uso do SQL dinâmico, ele oferece um nível de flexibilidade que os outros dois não têm. Além disso, um ataque de injeção de SQL só poderá ser explorado se um invasor puder executar o procedimento armazenado passando os parâmetros de entrada de sua escolha. Como a DAL usa consultas parametrizadas, o ADO.NET protegerá esses parâmetros que são enviados ao banco de dados por meio da arquitetura, o que significa que a vulnerabilidade de ataque de injeção de SQL só existe se o invasor puder executar o procedimento armazenado diretamente.

Para implementar essa funcionalidade, crie um novo procedimento armazenado no banco de dados Northwind chamado `GetProductsPagedAndSorted`. Esse procedimento armazenado deve aceitar três parâmetros de entrada: `@sortExpression`, um parâmetro de entrada do tipo `nvarchar(100`) que especifica como os resultados devem ser classificados e inseridos diretamente após o texto de `ORDER BY` na cláusula `OVER`; e `@startRowIndex` e `@maximumRows`, os mesmos dois parâmetros de entrada de inteiro do procedimento armazenado `GetProductsPaged` examinado no tutorial anterior. Crie o procedimento armazenado `GetProductsPagedAndSorted` usando o seguinte script:

[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample3.sql)]

O procedimento armazenado começa garantindo que um valor para o parâmetro `@sortExpression` foi especificado. Se estiver ausente, os resultados serão classificados por `ProductID`. Em seguida, a consulta SQL dinâmica é construída. Observe que a consulta SQL dinâmica aqui difere ligeiramente das consultas anteriores usadas para recuperar todas as linhas da tabela Products. Nos exemplos anteriores, obtivemos cada um dos produtos associados à categoria s e nomes de fornecedores usando uma subconsulta. Essa decisão foi tomada de volta no tutorial [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md) e foi feita em vez de usar `JOIN` s porque o TableAdapter não pode criar automaticamente os métodos INSERT, Update e Delete associados para essas consultas. O procedimento armazenado `GetProductsPagedAndSorted`, no entanto, deve usar `JOIN` s para que os resultados sejam ordenados pelos nomes de fornecedor ou categoria.

Essa consulta dinâmica é criada concatenando-se as partes de consulta estática e os parâmetros `@sortExpression`, `@startRowIndex`e `@maximumRows`. Como `@startRowIndex` e `@maximumRows` são parâmetros inteiros, eles devem ser convertidos em nvarchars para serem concatenados corretamente. Depois que essa consulta SQL dinâmica tiver sido construída, ela será executada por meio de `sp_executesql`.

Reserve um tempo para testar esse procedimento armazenado com valores diferentes para os parâmetros `@sortExpression`, `@startRowIndex`e `@maximumRows`. No Gerenciador de Servidores, clique com o botão direito do mouse no nome do procedimento armazenado e escolha Executar. Isso abrirá a caixa de diálogo Executar procedimento armazenado na qual você pode inserir os parâmetros de entrada (consulte a Figura 1). Para classificar os resultados pelo nome da categoria, use CategoryName para o valor do parâmetro `@sortExpression`; para classificar pelo nome da empresa do fornecedor, use CompanyName. Depois de fornecer os valores dos parâmetros, clique em OK. Os resultados são exibidos na janela saída. A Figura 2 mostra os resultados ao retornar produtos classificados de 11 a 20 ao ordenar pelo `UnitPrice` em ordem decrescente.

![Experimente valores diferentes para os três parâmetros de entrada do procedimento armazenado](sorting-custom-paged-data-cs/_static/image1.png)

**Figura 1**: Experimente valores diferentes para os três parâmetros de entrada do procedimento armazenado

[![os resultados dos procedimentos armazenados são mostrados no Janela de Saída](sorting-custom-paged-data-cs/_static/image3.png)](sorting-custom-paged-data-cs/_static/image2.png)

**Figura 2**: os resultados de s do procedimento armazenado são mostrados na janela de saída ([clique para exibir a imagem em tamanho normal](sorting-custom-paged-data-cs/_static/image4.png))

> [!NOTE]
> Ao ordenar os resultados pela coluna `ORDER BY` especificada na cláusula `OVER`, SQL Server deve classificar os resultados. Essa é uma operação rápida se houver um índice clusterizado sobre as colunas que os resultados estão sendo ordenados por ou se houver um índice de cobertura, mas pode ser mais dispendioso do contrário. Para melhorar o desempenho de consultas grandes o suficiente, considere adicionar um índice não clusterizado à coluna pela qual os resultados são ordenados. Consulte [classificando funções e desempenho no SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) para obter mais detalhes.

## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Etapa 2: aumentando as camadas de acesso a dados e da lógica de negócios

Com o procedimento armazenado `GetProductsPagedAndSorted` criado, nossa próxima etapa é fornecer um meio de executar esse procedimento armazenado por meio de nossa arquitetura de aplicativo. Isso envolve a adição de um método apropriado para a DAL e a BLL. Deixe que os s comecem adicionando um método à DAL. Abra o `Northwind.xsd` DataSet tipado, clique com o botão direito do mouse na `ProductsTableAdapter`e escolha a opção Adicionar consulta no menu de contexto. Como fizemos no tutorial anterior, queremos configurar esse novo método DAL para usar um procedimento armazenado existente – `GetProductsPagedAndSorted`, nesse caso. Comece indicando que você deseja que o novo método TableAdapter use um procedimento armazenado existente.

![Escolha usar um procedimento armazenado existente](sorting-custom-paged-data-cs/_static/image5.png)

**Figura 3**: escolher usar um procedimento armazenado existente

Para especificar o procedimento armazenado a ser usado, selecione o `GetProductsPagedAndSorted` procedimento armazenado na lista suspensa na próxima tela.

![Usar o procedimento armazenado GetProductsPagedAndSorted](sorting-custom-paged-data-cs/_static/image6.png)

**Figura 4**: usar o procedimento armazenado GetProductsPagedAndSorted

Esse procedimento armazenado retorna um conjunto de registros como seus resultados, portanto, na próxima tela, indique que ele retorna dados tabulares.

![Indicar que o procedimento armazenado retorna dados tabulares](sorting-custom-paged-data-cs/_static/image7.png)

**Figura 5**: indicar que o procedimento armazenado retorna dados tabulares

Por fim, crie métodos DAL que usam o Fill a DataTable e retornem os padrões de DataTable, nomeando os métodos `FillPagedAndSorted` e `GetProductsPagedAndSorted`, respectivamente.

![Escolha os nomes dos métodos](sorting-custom-paged-data-cs/_static/image8.png)

**Figura 6**: escolher os nomes dos métodos

Agora que estendemos a DAL, estamos prontos para voltar para a BLL. Abra o arquivo de classe `ProductsBLL` e adicione um novo método `GetProductsPagedAndSorted`. Esse método precisa aceitar três parâmetros de entrada `sortExpression`, `startRowIndex`e `maximumRows` e deve simplesmente chamar para baixo o método `GetProductsPagedAndSorted` s da DAL, desta forma:

[!code-csharp[Main](sorting-custom-paged-data-cs/samples/sample4.cs)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Etapa 3: Configurando o ObjectDataSource para passar o parâmetro SortExpression

Com o aumento da DAL e da BLL para incluir métodos que utilizam o procedimento armazenado `GetProductsPagedAndSorted`, tudo o que resta é configurar o ObjectDataSource na página `SortParameter.aspx` para usar o novo método BLL e passar o parâmetro `SortExpression` com base na coluna que o usuário solicitou a classificação dos resultados.

Comece alterando o `SelectMethod` ObjectDataSource s de `GetProductsPaged` para `GetProductsPagedAndSorted`. Isso pode ser feito por meio do assistente para configurar fonte de dados, da janela Propriedades ou diretamente por meio da sintaxe declarativa. Em seguida, precisamos fornecer um valor para a Propriedade ObjectDataSource s [`SortParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). Se essa propriedade for definida, o ObjectDataSource tentará passar a Propriedade GridView s `SortExpression` para a `SelectMethod`. Em particular, o ObjectDataSource procura um parâmetro de entrada cujo nome é igual ao valor da propriedade `SortParameterName`. Como o método de `GetProductsPagedAndSorted` de BLL s tem o parâmetro de entrada da expressão de classificação chamado `sortExpression`, defina a Propriedade ObjectDataSource s `SortExpression` como sortExpression.

Depois de fazer essas duas alterações, a sintaxe declarada de ObjectDataSource s deve ser semelhante ao seguinte:

[!code-aspx[Main](sorting-custom-paged-data-cs/samples/sample5.aspx)]

> [!NOTE]
> Assim como no tutorial anterior, verifique se ObjectDataSource *não inclui os* parâmetros de entrada SortExpression, StartRowIndex ou MaximumRows em sua coleção SelectParameters.

Para habilitar a classificação no GridView, basta marcar a caixa de seleção Habilitar classificação na marca inteligente de GridView, que define a Propriedade GridView s `AllowSorting` como `true` e fazendo com que o texto do cabeçalho de cada coluna seja renderizado como um LinkButton. Quando o usuário final clica em um dos LinkButton do cabeçalho, um postback massacre e as seguintes etapas se transseguirão:

1. O GridView atualiza sua [propriedade`SortExpression`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) para o valor da `SortExpression` do campo cujo link de cabeçalho foi clicado
2. O ObjectDataSource invoca o método `GetProductsPagedAndSorted` de BLL, passando a Propriedade GridView s `SortExpression` como o valor para o método s `sortExpression` parâmetro de entrada (juntamente com os valores apropriados `startRowIndex` e `maximumRows` de parâmetro de entrada)
3. A BLL invoca o método da DAL s `GetProductsPagedAndSorted`
4. A DAL executa o procedimento armazenado `GetProductsPagedAndSorted`, passando o parâmetro `@sortExpression` (juntamente com os valores de parâmetro de entrada `@startRowIndex` e `@maximumRows`)
5. O procedimento armazenado retorna o subconjunto apropriado de dados para a BLL, que o retorna para o ObjectDataSource; esses dados são então ligados ao GridView, renderizados em HTML e enviados para o usuário final

A Figura 7 mostra a primeira página de resultados quando classificada pelo `UnitPrice` em ordem crescente.

[![os resultados são classificados pelo PreçoUnitário](sorting-custom-paged-data-cs/_static/image10.png)](sorting-custom-paged-data-cs/_static/image9.png)

**Figura 7**: os resultados são classificados pelo PreçoUnitário ([clique para exibir a imagem em tamanho normal](sorting-custom-paged-data-cs/_static/image11.png))

Embora a implementação atual possa classificar corretamente os resultados por nome do produto, nome da categoria, quantidade por unidade e preço unitário, a tentativa de ordenar os resultados pelo nome do fornecedor resultará em uma exceção de tempo de execução (consulte a Figura 8).

![A tentativa de classificar os resultados pelo fornecedor resulta na seguinte exceção de tempo de execução](sorting-custom-paged-data-cs/_static/image12.png)

**Figura 8**: tentando classificar os resultados pelo fornecedor resulta na seguinte exceção de tempo de execução

Essa exceção ocorre porque o `SortExpression` de GridView s `SupplierName` BoundField está definido como `SupplierName`. No entanto, o nome do fornecedor na tabela `Suppliers` é, na verdade, chamado `CompanyName` temos um alias desse nome de coluna como `SupplierName`. No entanto, a cláusula `OVER` usada pela função `ROW_NUMBER()` não pode usar o alias e deve usar o nome real da coluna. Portanto, altere o `SupplierName` BoundField s `SortExpression` de Supplier para CompanyName (consulte a Figura 9). Como mostra a Figura 10, após essa alteração, os resultados podem ser classificados pelo fornecedor.

![Alterar o Supplier BoundField s SortExpression para CompanyName](sorting-custom-paged-data-cs/_static/image13.png)

**Figura 9**: alterar o Supplier BoundField s SortExpression para CompanyName

[![os resultados agora podem ser classificados por fornecedor](sorting-custom-paged-data-cs/_static/image15.png)](sorting-custom-paged-data-cs/_static/image14.png)

**Figura 10**: os resultados agora podem ser classificados por fornecedor ([clique para exibir a imagem em tamanho normal](sorting-custom-paged-data-cs/_static/image16.png))

## <a name="summary"></a>Resumo

A implementação de paginação personalizada que examinamos no tutorial anterior exigia que a ordem pela qual os resultados fossem classificados fosse especificada em tempo de design. Em suma, isso significava que a implementação de paginação personalizada que implementamos não pôde, ao mesmo tempo, fornecer recursos de classificação. Neste tutorial, superou essa limitação estendendo o procedimento armazenado do primeiro para incluir um `@sortExpression` parâmetro de entrada pelo qual os resultados podem ser classificados.

Depois de criar esse procedimento armazenado e criar novos métodos na DAL e na BLL, pudemos implementar um GridView que oferecia a classificação e a paginação personalizada, configurando o ObjectDataSource para passar a propriedade de `SortExpression` do GridView s para o `SelectMethod`de BLL.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de cliente potencial para este tutorial foi Carlos Santos. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](efficiently-paging-through-large-amounts-of-data-cs.md)
> [Próximo](creating-a-customized-sorting-user-interface-cs.md)
