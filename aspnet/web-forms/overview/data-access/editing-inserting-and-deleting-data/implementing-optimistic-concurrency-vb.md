---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
title: Implementando simultaneidade otimista (VB) | Microsoft Docs
author: rick-anderson
description: Para um aplicativo Web que permite que vários usuários editem dados, há o risco de que dois usuários possam estar editando os mesmos dados ao mesmo tempo. Neste tutorial...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 2646968c-2826-4418-b1d0-62610ed177e3
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
msc.type: authoredcontent
ms.openlocfilehash: 28c39fe2a290cc3a5b093fdd09de341630606137
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78607908"
---
# <a name="implementing-optimistic-concurrency-vb"></a>Implementar a simultaneidade otimista (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_VB.exe) ou [baixar PDF](implementing-optimistic-concurrency-vb/_static/datatutorial21vb1.pdf)

> Para um aplicativo Web que permite que vários usuários editem dados, há o risco de que dois usuários possam estar editando os mesmos dados ao mesmo tempo. Neste tutorial, implementaremos o controle de simultaneidade otimista para lidar com esse risco.

## <a name="introduction"></a>Introdução

Para aplicativos Web que só permitem que os usuários exibam dados ou para aqueles que incluem apenas um único usuário que pode modificar dados, não há nenhuma ameaça de dois usuários simultâneos substituindo as alterações mais uma vez. No entanto, para aplicativos Web que permitem que vários usuários atualizem ou excluam dados, no entanto, há o potencial para que as modificações de um usuário entrem em conflito com outros usuários simultâneos. Sem nenhuma política de simultaneidade em vigor, quando dois usuários estão editando um único registro ao mesmo tempo, o usuário que confirma suas alterações por último substituirá as alterações feitas pelo primeiro.

Por exemplo, imagine que dois usuários, Jisun e Sam, estavam visitando uma página em nosso aplicativo que permitia que os visitantes atualizassem e excluíssem os produtos por meio de um controle GridView. Ambos clicam no botão Editar no GridView ao mesmo tempo. Jisun altera o nome do produto para "Chai chá" e clica no botão atualizar. O resultado líquido é uma instrução `UPDATE` que é enviada ao banco de dados, que define *todos* os campos atualizáveis do produto (mesmo que Jisun tenha atualizado apenas um campo, `ProductName`). Nesse momento, o banco de dados tem os valores "Chai chá", as bebidas da categoria, o fornecedor exóticas liquids e assim por diante para esse produto específico. No entanto, o GridView na tela do Sam ainda mostra o nome do produto na linha GridView editável como "Chai". Alguns segundos depois que as alterações de Jisun tiverem sido confirmadas, o Sam atualizará a categoria para condiments e clicará em atualizar. Isso resulta em uma instrução de `UPDATE` enviada ao banco de dados que define o nome do produto como "Chai", a `CategoryID` para a ID da categoria de bebidas correspondente e assim por diante. As alterações do Jisun para o nome do produto foram substituídas. A Figura 1 descreve graficamente esta série de eventos.

[![quando dois usuários atualizam simultaneamente um registro com potencial para as alterações de um usuário para substituir o outro](implementing-optimistic-concurrency-vb/_static/image2.png)](implementing-optimistic-concurrency-vb/_static/image1.png)

**Figura 1**: quando dois usuários atualizam simultaneamente um registro, há um potencial para as alterações de um usuário para substituir o outro ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image3.png))

Da mesma forma, quando dois usuários estão visitando uma página, um usuário pode estar no meio da atualização de um registro quando ele é excluído por outro usuário. Ou, entre quando um usuário carrega uma página e quando clica no botão excluir, outro usuário pode ter modificado o conteúdo desse registro.

Há três estratégias de [controle de simultaneidade](http://en.wikipedia.org/wiki/Concurrency_control) disponíveis:

- **Não fazer nada** -se usuários simultâneos estiverem modificando o mesmo registro, deixe a última confirmação vencer (o comportamento padrão)
- [**Simultaneidade otimista**](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -suponha que, embora possa haver conflitos de simultaneidade a cada momento e, em seguida, a grande maioria do tempo que esses conflitos não surgirão; Portanto, se ocorrer um conflito, basta informar ao usuário que suas alterações não podem ser salvas porque outro usuário modificou os mesmos dados
- **Simultaneidade pessimista** -assuma que os conflitos de simultaneidade são comuns e que os usuários não toleram que suas alterações não tenham sido salvas devido à atividade simultânea de outro usuário; Portanto, quando um usuário inicia a atualização de um registro, o bloqueia, impedindo que outros usuários editem ou excluam esse registro até que o usuário confirme suas modificações

Todos os nossos tutoriais, até agora, usaram a estratégia de resolução de simultaneidade padrão, ou seja, deixamos o último ganho de gravação. Neste tutorial, examinaremos como implementar o controle de simultaneidade otimista.

> [!NOTE]
> Não veremos exemplos de simultaneidade pessimista nesta série de tutoriais. A simultaneidade pessimista raramente é usada porque esses bloqueios, se não forem corretamente desalocados, podem impedir que outros usuários atualizem dados. Por exemplo, se um usuário bloquear um registro para edição e, em seguida, sair para o dia antes de desbloqueá-lo, nenhum outro usuário poderá atualizar esse registro até que o usuário original retorne e conclua sua atualização. Portanto, em situações em que a simultaneidade pessimista é usada, normalmente há um tempo limite que, se atingido, cancela o bloqueio. Sites de vendas de tíquetes, que bloqueiam um local de assentos específico por um curto período, enquanto o usuário conclui o processo de pedido, é um exemplo de controle de simultaneidade pessimista.

## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Etapa 1: examinando como a simultaneidade otimista é implementada

O controle de simultaneidade otimista funciona garantindo que o registro que está sendo atualizado ou excluído tenha os mesmos valores que faziam quando o processo de atualização ou exclusão foi iniciado. Por exemplo, ao clicar no botão Editar em um GridView editável, os valores do registro são lidos do banco de dados e exibidos em caixas de Texte outros controles da Web. Esses valores originais são salvos pelo GridView. Posteriormente, depois que o usuário fizer alterações e clicar no botão atualizar, os valores originais mais os novos valores serão enviados para a camada lógica de negócios e, em seguida, para a camada de acesso a dados. A camada de acesso a dados deve emitir uma instrução SQL que só atualizará o registro se os valores originais que o usuário iniciou a edição forem idênticos aos valores que ainda estão no banco de dados. A Figura 2 descreve essa sequência de eventos.

[![para que a atualização ou exclusão seja realizada com sucesso, os valores originais devem ser iguais aos valores do banco de dados atual](implementing-optimistic-concurrency-vb/_static/image5.png)](implementing-optimistic-concurrency-vb/_static/image4.png)

**Figura 2**: para que a atualização ou exclusão seja realizada com sucesso, os valores originais devem ser iguais aos valores do banco de dados atual ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image6.png))

Há várias abordagens para implementar a simultaneidade otimista (consulte a [lógica de atualização de simultaneidade otimista](http://www.eggheadcafe.com/articles/20050719.asp) de [Peter A. Bromberg](http://peterbromberg.net/)para obter uma breve visão de várias opções). O conjunto de ADO.NET digitado fornece uma implementação que pode ser configurada apenas com o tique de uma caixa de seleção. Habilitar a simultaneidade otimista para um TableAdapter no DataSet tipado aumenta as instruções `UPDATE` e `DELETE` do TableAdapter para incluir uma comparação de todos os valores originais na cláusula `WHERE`. A instrução `UPDATE` a seguir, por exemplo, atualiza o nome e o preço de um produto somente se os valores do banco de dados atual forem iguais aos valores que foram originalmente recuperados ao atualizar o registro no GridView. Os parâmetros `@ProductName` e `@UnitPrice` contêm os novos valores inseridos pelo usuário, enquanto `@original_ProductName` e `@original_UnitPrice` contêm os valores que foram carregados originalmente no GridView quando o botão Editar foi clicado:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample1.sql)]

> [!NOTE]
> Esta instrução de `UPDATE` foi simplificada para facilitar a leitura. Na prática, a `UnitPrice` verificação na cláusula `WHERE` seria mais envolvida, já que `UnitPrice` pode conter `NULL` s e verificar se `NULL = NULL` sempre retorna false (em vez disso, você deve usar `IS NULL`).

Além de usar uma instrução de `UPDATE` subjacente diferente, configurar um TableAdapter para usar a simultaneidade otimista também modifica a assinatura de seus métodos diretos de banco de BD. Lembre-se do nosso primeiro tutorial, [*criando uma camada de acesso a dados*](../introduction/creating-a-data-access-layer-cs.md), que os métodos diretos do DB eram aqueles que aceitam uma lista de valores escalares como parâmetros de entrada (em vez de uma instância de DataRow ou DataTable fortemente tipada). Ao usar a simultaneidade otimista, o `Update()` do DB Direct e os métodos de `Delete()` também incluem parâmetros de entrada para os valores originais. Além disso, o código na BLL para usar o padrão de atualização do lote (o `Update()` sobrecargas de método que aceitam DataRows e DataTables em vez de valores escalares) também deve ser alterado.

Em vez de estender os TableAdapters de DAL existentes para usar a simultaneidade otimista (o que precisaria alterar a BLL para acomodar), vamos criar um novo conjunto de um DataSet com tipo chamado `NorthwindOptimisticConcurrency`, ao qual adicionaremos um TableAdapter de `Products` que usa simultaneidade otimista. Depois disso, criaremos uma classe de camada de lógica de negócios `ProductsOptimisticConcurrencyBLL` que tem as modificações apropriadas para dar suporte à DAL de simultaneidade otimista. Depois que essa base tiver sido disposta, estaremos prontos para criar a página ASP.NET.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Etapa 2: criando uma camada de acesso a dados que dá suporte à simultaneidade otimista

Para criar um novo DataSet tipado, clique com o botão direito do mouse na pasta `DAL` dentro da pasta `App_Code` e adicione um novo conjunto de novos conjuntos de um nome `NorthwindOptimisticConcurrency`. Como vimos no primeiro tutorial, isso irá adicionar um novo TableAdapter ao DataSet tipado, iniciando automaticamente o assistente de configuração do TableAdapter. Na primeira tela, é solicitado que você especifique o banco de dados para conectar-se ao mesmo banco de dados Northwind usando a configuração de `NORTHWNDConnectionString` de `Web.config`.

[![conectar-se ao mesmo banco de dados Northwind](implementing-optimistic-concurrency-vb/_static/image8.png)](implementing-optimistic-concurrency-vb/_static/image7.png)

**Figura 3**: conectar-se ao mesmo banco de dados Northwind ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image9.png))

Em seguida, é solicitado que você veja como consultar os dados: por meio de uma instrução SQL ad hoc, um novo procedimento armazenado ou um procedimento armazenado existente. Como usamos consultas SQL ad hoc em nossa DAL original, use essa opção aqui também.

[![especificar os dados a serem recuperados usando uma instrução SQL ad hoc](implementing-optimistic-concurrency-vb/_static/image11.png)](implementing-optimistic-concurrency-vb/_static/image10.png)

**Figura 4**: especificar os dados a serem recuperados usando uma instrução SQL ad hoc ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image12.png))

Na tela a seguir, insira a consulta SQL a ser usada para recuperar as informações do produto. Vamos usar exatamente a mesma consulta SQL usada para o `Products` TableAdapter de nossa DAL original, que retorna todas as colunas de `Product` junto com os nomes de fornecedores e categorias do produto:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample2.sql)]

[![usar a mesma consulta SQL do TableAdapter de produtos na DAL original](implementing-optimistic-concurrency-vb/_static/image14.png)](implementing-optimistic-concurrency-vb/_static/image13.png)

**Figura 5**: usar a mesma consulta SQL do `Products` TABLEADAPTER na Dal original ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image15.png))

Antes de passar para a próxima tela, clique no botão Opções avançadas. Para que esse TableAdapter empregue o controle de simultaneidade otimista, basta marcar a caixa de seleção "usar simultaneidade otimista".

[![habilitar o controle de simultaneidade otimista marcando a caixa de seleção &quot;usar a simultaneidade otimista&quot;](implementing-optimistic-concurrency-vb/_static/image17.png)](implementing-optimistic-concurrency-vb/_static/image16.png)

**Figura 6**: habilitar o controle de simultaneidade otimista marcando a caixa de seleção "usar simultaneidade otimista" ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image18.png))

Por fim, indique que o TableAdapter deve usar os padrões de acesso a dados que preenchem uma DataTable e retornam uma DataTable; Além disso, indique que os métodos diretos do banco de forma devem ser criados. Altere o nome do método para o padrão Return a DataTable de GetData para GetProducts, para espelhar as convenções de nomenclatura que usamos em nossa DAL original.

[![fazer com que o TableAdapter utilize todos os padrões de acesso a dados](implementing-optimistic-concurrency-vb/_static/image20.png)](implementing-optimistic-concurrency-vb/_static/image19.png)

**Figura 7**: fazer com que o TableAdapter utilize todos os padrões de acesso a dados ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image21.png))

Depois de concluir o assistente, o designer de conjunto de os incluirá uma DataTable de `Products` fortemente tipada e o TableAdapter. Reserve um tempo para renomear a DataTable de `Products` para `ProductsOptimisticConcurrency`, que você pode fazer clicando com o botão direito do mouse na barra de título da DataTable e escolhendo renomear no menu de contexto.

[![uma DataTable e um TableAdapter foram adicionados ao DataSet tipado](implementing-optimistic-concurrency-vb/_static/image23.png)](implementing-optimistic-concurrency-vb/_static/image22.png)

**Figura 8**: uma DataTable e um TableAdapter foram adicionados ao dataset tipado ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image24.png))

Para ver as diferenças entre as consultas `UPDATE` e `DELETE` entre o `ProductsOptimisticConcurrency` TableAdapter (que usa simultaneidade otimista) e o TableAdapter Products (que não é), clique no TableAdapter e vá para a janela Propriedades. Nas propriedades `DeleteCommand` e `UpdateCommand` ' `CommandText` subpropriedades, você pode ver a sintaxe SQL real que é enviada ao banco de dados quando os métodos relacionados à atualização ou à exclusão da DAL são invocados. Para o `ProductsOptimisticConcurrency` TableAdapter, a instrução `DELETE` usada é:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample3.sql)]

Enquanto a instrução de `DELETE` para o TableAdapter do produto em nossa DAL original é a mais simples:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample4.sql)]

Como você pode ver, a cláusula `WHERE` na instrução `DELETE` do TableAdapter que usa a simultaneidade otimista inclui uma comparação entre cada um dos valores de coluna existentes da tabela `Product` e os valores originais no momento em que o GridView (ou DetailsView ou FormView) foi preenchido pela última vez. Como todos os campos diferentes de `ProductID`, `ProductName`e `Discontinued` podem ter valores de `NULL`, parâmetros e verificações adicionais são incluídos para comparar corretamente os valores de `NULL` na cláusula `WHERE`.

Não adicionaremos nenhuma tabela adicional ao conjunto de dados habilitado para simultaneidade otimista para este tutorial, pois nossa página ASP.NET fornecerá apenas a atualização e a exclusão de informações do produto. No entanto, ainda precisamos adicionar o método `GetProductByProductID(productID)` ao TableAdapter do `ProductsOptimisticConcurrency`.

Para fazer isso, clique com o botão direito do mouse na barra de título do TableAdapter (a área à direita acima do `Fill` e `GetProducts` nomes de método) e escolha Adicionar consulta no menu de contexto. Isso iniciará o assistente de configuração de consulta do TableAdapter. Assim como na configuração inicial do TableAdapter, opte por criar o método de `GetProductByProductID(productID)` usando uma instrução SQL ad hoc (consulte a Figura 4). Como o método `GetProductByProductID(productID)` retorna informações sobre um produto específico, indique que essa consulta é um tipo de consulta `SELECT` que retorna linhas.

[![marcar o tipo de consulta como um &quot;selecione que retorna linhas&quot;](implementing-optimistic-concurrency-vb/_static/image26.png)](implementing-optimistic-concurrency-vb/_static/image25.png)

**Figura 9**: marcar o tipo de consulta como um "`SELECT` que retorna linhas" ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image27.png))

Na próxima tela, é solicitado que a consulta SQL use, com a consulta padrão do TableAdapter carregada previamente. Aumente a consulta existente para incluir a cláusula `WHERE ProductID = @ProductID`, conforme mostrado na Figura 10.

[![adicionar uma cláusula WHERE à consulta pré-carregada para retornar um registro de produto específico](implementing-optimistic-concurrency-vb/_static/image29.png)](implementing-optimistic-concurrency-vb/_static/image28.png)

**Figura 10**: adicionar uma cláusula `WHERE` à consulta pré-carregada para retornar um registro de produto específico ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image30.png))

Por fim, altere os nomes de métodos gerados para `FillByProductID` e `GetProductByProductID`.

[![renomear os métodos como FillByProductID e GetProductByProductID](implementing-optimistic-concurrency-vb/_static/image32.png)](implementing-optimistic-concurrency-vb/_static/image31.png)

**Figura 11**: renomear os métodos para `FillByProductID` e `GetProductByProductID` ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image33.png))

Com esse assistente concluído, o TableAdapter agora contém dois métodos para recuperar dados: `GetProducts()`, que retorna *todos os* produtos; e `GetProductByProductID(productID)`, que retorna o produto especificado.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Etapa 3: criando uma camada de lógica de negócios para a DAL otimista habilitada para simultaneidade

Nossa classe de `ProductsBLL` existente tem exemplos de como usar os padrões de atualização do lote e do BD Direct. O método `AddProduct` e `UpdateProduct` sobrecargas usam o padrão de atualização do lote, passando uma instância `ProductRow` para o método Update do TableAdapter. O método `DeleteProduct`, por outro lado, usa o padrão DB Direct, chamando o método `Delete(productID)` do TableAdapter.

Com o novo `ProductsOptimisticConcurrency` TableAdapter, os métodos do DB Direct agora exigem que os valores originais também sejam passados. Por exemplo, o método `Delete` agora espera dez parâmetros de entrada: o `ProductID`original, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`e `Discontinued`. Ele usa esses valores de parâmetros de entrada adicionais na cláusula `WHERE` da instrução `DELETE` enviada ao banco de dados, excluindo apenas o registro especificado se os valores atuais do banco de dados forem mapeados para os originais.

Embora a assinatura do método para o método `Update` do TableAdapter usado no padrão de atualização do lote não tenha sido alterada, o código necessário para registrar os valores originais e novos tem. Portanto, em vez de tentar usar a DAL otimista habilitada para simultaneidade com nossa classe de `ProductsBLL` existente, vamos criar uma nova classe de camada de lógica de negócios para trabalhar com nossa nova DAL.

Adicione uma classe chamada `ProductsOptimisticConcurrencyBLL` à pasta `BLL` dentro da pasta `App_Code`.

![Adicionar a classe ProductsOptimisticConcurrencyBLL à pasta BLL](implementing-optimistic-concurrency-vb/_static/image34.png)

**Figura 12**: adicionar a classe `ProductsOptimisticConcurrencyBLL` à pasta BLL

Em seguida, adicione o seguinte código à classe `ProductsOptimisticConcurrencyBLL`:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample5.vb)]

Observe a instrução using `NorthwindOptimisticConcurrencyTableAdapters` acima do início da declaração de classe. O namespace `NorthwindOptimisticConcurrencyTableAdapters` contém a classe `ProductsOptimisticConcurrencyTableAdapter`, que fornece os métodos da DAL. Além disso, antes da declaração de classe, você encontrará o atributo `System.ComponentModel.DataObject`, que instrui o Visual Studio a incluir essa classe na lista suspensa do assistente ObjectDataSource.

A propriedade `Adapter` do `ProductsOptimisticConcurrencyBLL`fornece acesso rápido a uma instância da classe `ProductsOptimisticConcurrencyTableAdapter` e segue o padrão usado em nossas classes BLL originais (`ProductsBLL`, `CategoriesBLL`e assim por diante). Finalmente, o método `GetProducts()` simplesmente chama o método `GetProducts()` da DAL e retorna um objeto `ProductsOptimisticConcurrencyDataTable` populado com uma instância `ProductsOptimisticConcurrencyRow` para cada registro de produto no banco de dados.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Excluindo um produto usando o padrão de BD direto com simultaneidade otimista

Ao usar o padrão de BD direto em uma DAL que usa simultaneidade otimista, os métodos devem ser passados para os valores novos e originais. Para excluir, não há nenhum novo valor, portanto, somente os valores originais precisam ser passados. Em nossa BLL, devemos aceitar todos os parâmetros originais como parâmetros de entrada. Vamos ter o método `DeleteProduct` na classe `ProductsOptimisticConcurrencyBLL` usar o método DB Direct. Isso significa que esse método precisa executar todos os dez campos de dados de produto como parâmetros de entrada e passá-los para a DAL, conforme mostrado no código a seguir:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample6.vb)]

Se os valores originais-os valores que foram carregados pela última vez no GridView (ou DetailsView ou FormView)-forem diferentes dos valores no banco de dados quando o usuário clicar no botão excluir, a cláusula `WHERE` não corresponderá a nenhum registro de banco de dados e os registros não serão afetados. Portanto, o método `Delete` do TableAdapter retornará `0` e o método `DeleteProduct` da BLL retornará `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Atualizando um produto usando o padrão de atualização do lote com simultaneidade otimista

Conforme observado anteriormente, o método `Update` do TableAdapter para o padrão de atualização do lote tem a mesma assinatura de método, independentemente de a simultaneidade otimista ser empregada ou não. Ou seja, o método `Update` espera uma DataRow, uma matriz de DataRows, uma DataTable ou um DataSet tipado. Não há parâmetros de entrada adicionais para especificar os valores originais. Isso é possível porque a DataTable controla os valores originais e modificados para suas DataRows. Quando a DAL emite sua instrução `UPDATE`, os parâmetros `@original_ColumnName` são populados com os valores originais da DataRow, enquanto os parâmetros `@ColumnName` são populados com os valores modificados da DataRow.

Na classe `ProductsBLL` (que usa nossa DAL de simultaneidade original e não otimista), ao usar o padrão de atualização do lote para atualizar as informações do produto, nosso código executa a seguinte sequência de eventos:

1. Ler as informações do produto do banco de dados atual em uma instância `ProductRow` usando o método `GetProductByProductID(productID)` do TableAdapter
2. Atribua os novos valores à instância de `ProductRow` da etapa 1
3. Chame o método `Update` do TableAdapter, passando a instância `ProductRow`

Essa sequência de etapas, no entanto, não dará suporte corretamente à simultaneidade otimista porque a `ProductRow` populada na etapa 1 é preenchida diretamente do banco de dados, o que significa que os valores originais usados pela DataRow são aqueles que existem atualmente no banco de dados, e não aqueles que foram associados ao GridView no início do processo de edição. Em vez disso, ao usar uma DAL otimista habilitada para simultaneidade, precisamos alterar o `UpdateProduct` sobrecargas do método para usar as seguintes etapas:

1. Ler as informações do produto do banco de dados atual em uma instância `ProductsOptimisticConcurrencyRow` usando o método `GetProductByProductID(productID)` do TableAdapter
2. Atribua os valores *originais* à instância de `ProductsOptimisticConcurrencyRow` da etapa 1
3. Chame o método `AcceptChanges()` da instância de `ProductsOptimisticConcurrencyRow`, que instrui a DataRow de que seus valores atuais são os "originais"
4. Atribuir os *novos* valores à instância de `ProductsOptimisticConcurrencyRow`
5. Chame o método `Update` do TableAdapter, passando a instância `ProductsOptimisticConcurrencyRow`

A etapa 1 lê todos os valores de banco de dados atuais para o registro de produto especificado. Essa etapa é supérflua na sobrecarga de `UpdateProduct` que atualiza *todas* as colunas de produto (pois esses valores são substituídos na etapa 2), mas é essencial para essas sobrecargas em que apenas um subconjunto dos valores de coluna é passado como parâmetros de entrada. Depois que os valores originais tiverem sido atribuídos à instância de `ProductsOptimisticConcurrencyRow`, o método `AcceptChanges()` será chamado, que marca os valores de DataRow atuais como os valores originais a serem usados nos parâmetros de `@original_ColumnName` na instrução `UPDATE`. Em seguida, os novos valores de parâmetro são atribuídos à `ProductsOptimisticConcurrencyRow` e, finalmente, o método `Update` é invocado, passando o DataRow.

O código a seguir mostra a sobrecarga de `UpdateProduct` que aceita todos os campos de dados de produto como parâmetros de entrada. Embora não seja mostrado aqui, a classe `ProductsOptimisticConcurrencyBLL` incluída no download para este tutorial também contém uma sobrecarga `UpdateProduct` que aceita apenas o nome e o preço do produto como parâmetros de entrada.

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample7.vb)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Etapa 4: passando os valores originais e novos da página ASP.NET para os métodos de BLL

Com a DAL e a BLL concluídas, tudo o que resta é criar uma página ASP.NET que possa utilizar a lógica de simultaneidade otimista interna no sistema. Especificamente, o controle da Web de dados (GridView, DetailsView ou FormView) deve se lembrar de seus valores originais e o ObjectDataSource deve passar os dois conjuntos de valores para a camada de lógica de negócios. Além disso, a página ASP.NET deve ser configurada para lidar normalmente com violações de simultaneidade.

Comece abrindo a página de `OptimisticConcurrency.aspx` na pasta `EditInsertDelete` e adicionando um GridView ao designer, definindo sua propriedade `ID` como `ProductsGrid`. Na marca inteligente do GridView, opte por criar um novo ObjectDataSource chamado `ProductsOptimisticConcurrencyDataSource`. Como queremos que esse ObjectDataSource use a DAL que dá suporte à simultaneidade otimista, configure-a para usar o objeto `ProductsOptimisticConcurrencyBLL`.

[![fazer com que o ObjectDataSource use o objeto ProductsOptimisticConcurrencyBLL](implementing-optimistic-concurrency-vb/_static/image36.png)](implementing-optimistic-concurrency-vb/_static/image35.png)

**Figura 13**: fazer com que o ObjectDataSource use o objeto `ProductsOptimisticConcurrencyBLL` ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image37.png))

Escolha os métodos `GetProducts`, `UpdateProduct`e `DeleteProduct` nas listas suspensas no assistente. Para o método UpdateProduct, use a sobrecarga que aceita todos os campos de dados do produto.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Configurando as propriedades do controle ObjectDataSource

Depois de concluir o assistente, a marcação declarativa do ObjectDataSource deve ser semelhante ao seguinte:

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample8.aspx)]

Como você pode ver, a coleção de `DeleteParameters` contém uma instância de `Parameter` para cada um dos dez parâmetros de entrada no método `DeleteProduct` da classe `ProductsOptimisticConcurrencyBLL`. Da mesma forma, a coleção de `UpdateParameters` contém uma instância de `Parameter` para cada um dos parâmetros de entrada no `UpdateProduct`.

Para os tutoriais anteriores que envolvevam a modificação de dados, removemos a propriedade de `OldValuesParameterFormatString` do ObjectDataSource neste ponto, pois essa propriedade indica que o método BLL espera que os valores antigos (ou originais) sejam passados, bem como os novos valores. Além disso, esse valor de propriedade indica os nomes de parâmetro de entrada para os valores originais. Como estamos passando os valores originais para a BLL, *não* remova essa propriedade.

> [!NOTE]
> O valor da propriedade `OldValuesParameterFormatString` deve mapear para os nomes de parâmetro de entrada na BLL que esperam os valores originais. Como nomeamos esses parâmetros `original_productName`, `original_supplierID`e assim por diante, você pode deixar o valor da propriedade `OldValuesParameterFormatString` como `original_{0}`. No entanto, se os parâmetros de entrada dos métodos de BLL tivessem nomes como `old_productName`, `old_supplierID`e assim por diante, você precisará atualizar a propriedade `OldValuesParameterFormatString` para `old_{0}`.

Há uma configuração de propriedade final que precisa ser feita para que o ObjectDataSource passe corretamente os valores originais para os métodos de BLL. O ObjectDataSource tem uma [propriedade ConflictDetection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) que pode ser atribuída a [um dos dois valores](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges`-o valor padrão; não envia os valores originais para os parâmetros de entrada originais dos métodos de BLL
- `CompareAllValues`-envia os valores originais para os métodos de BLL; Escolha esta opção ao usar a simultaneidade otimista

Reserve um momento para definir a propriedade `ConflictDetection` como `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Configurando as propriedades e os campos de GridView

Com as propriedades do ObjectDataSource configuradas corretamente, vamos voltar nossa atenção para configurar o GridView. Primeiro, como queremos que o GridView dê suporte à edição e exclusão, clique nas caixas de seleção Habilitar edição e habilitar exclusão na marca inteligente do GridView. Isso adicionará um CommandField cujos `ShowEditButton` e `ShowDeleteButton` estão definidos como `true`.

Quando associado ao `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, o GridView contém um campo para cada um dos campos de dados do produto. Embora tal GridView possa ser editada, a experiência do usuário é qualquer coisa, mas aceitável. O `CategoryID` e o `SupplierID` BoundFields serão renderizados como caixas de Text, exigindo que o usuário insira a categoria e o fornecedor apropriados como números de identificação. Não haverá formatação para os campos numéricos e nenhum controle de validação para garantir que o nome do produto tenha sido fornecido e que o preço unitário, as unidades em estoque, as unidades em ordem e os valores de nível de reordenação sejam valores numéricos adequados e sejam maiores ou iguais para zero.

Como discutimos na *adição de controles de validação para as interfaces de edição e inserção* e *personalização dos tutoriais da interface de modificação de dados* , a interface do usuário pode ser personalizada por meio da substituição de boundfields por TemplateFields. Modifiquei este GridView e sua interface de edição das seguintes maneiras:

- Removidas as `ProductID`, `SupplierName`e `CategoryName` BoundFields
- Convertemos o `ProductName` BoundField em um TemplateField e adicionamos um controle RequiredFieldValidation.
- Convertemos o `CategoryID` e `SupplierID` BoundFields em TemplateFields e ajustamos a interface de edição para usar DropDownLists em vez de TextBoxes. Nesses TemplateFields ' `ItemTemplates`, os campos de dados `CategoryName` e `SupplierName` são exibidos.
- Converteu os `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`e `ReorderLevel` BoundFields em TemplateFields e adicionou controles CompareValidator.

Como já examinamos como realizar essas tarefas nos tutoriais anteriores, apenas listarei a sintaxe declarativa final aqui e deixarei a implementação como prática.

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample9.aspx)]

Estamos muito próximos de ter um exemplo totalmente funcional. No entanto, há algumas sutilezas que serão acumuladas e causarão problemas. Além disso, ainda precisamos de uma interface que alerta o usuário quando ocorreu uma violação de simultaneidade.

> [!NOTE]
> Para que um controle da Web de dados passe corretamente os valores originais para o ObjectDataSource (que são passados para a BLL), é vital que a propriedade `EnableViewState` do GridView seja definida como `true` (o padrão). Se você desabilitar o estado de exibição, os valores originais serão perdidos no postback.

## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Passando os valores originais corretos para o ObjectDataSource

Há alguns problemas com a maneira como o GridView foi configurado. Se a propriedade `ConflictDetection` do ObjectDataSource for definida como `CompareAllValues` (como a nossa), quando os métodos `Update()` ou `Delete()` do ObjectDataSource forem invocados pelo GridView (ou DetailsView ou FormView), o ObjectDataSource tentará copiar os valores originais do GridView em suas instâncias de `Parameter` apropriadas. Consulte novamente a Figura 2 para uma representação gráfica desse processo.

Especificamente, os valores originais do GridView são atribuídos aos valores nas instruções de ligação de dados bidirecionais cada vez que os dados são associados ao GridView. Portanto, é essencial que todos os valores originais necessários sejam capturados por meio de ligação de dados bidirecional e que eles sejam fornecidos em um formato conversível.

Para ver por que isso é importante, Reserve um tempo para visitar nossa página em um navegador. Como esperado, o GridView lista cada produto com um botão Editar e excluir na coluna mais à esquerda.

[![os produtos estão listados em um GridView](implementing-optimistic-concurrency-vb/_static/image39.png)](implementing-optimistic-concurrency-vb/_static/image38.png)

**Figura 14**: os produtos são listados em um GridView ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image40.png))

Se você clicar no botão excluir de qualquer produto, um `FormatException` será lançado.

[![tentar excluir qualquer resultado de produto em um FormatException](implementing-optimistic-concurrency-vb/_static/image42.png)](implementing-optimistic-concurrency-vb/_static/image41.png)

**Figura 15**: tentando excluir qualquer resultado de produto em um `FormatException` ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image43.png))

O `FormatException` é gerado quando o ObjectDataSource tenta ler no valor de `UnitPrice` original. Como o `ItemTemplate` tem o `UnitPrice` formatado como uma moeda (`<%# Bind("UnitPrice", "{0:C}") %>`), ele inclui um símbolo de moeda, como $19.95. O `FormatException` ocorre quando o ObjectDataSource tenta converter essa cadeia de caracteres em um `decimal`. Para contornar esse problema, temos várias opções:

- Remova a formatação de moeda da `ItemTemplate`. Ou seja, em vez de usar `<%# Bind("UnitPrice", "{0:C}") %>`, basta usar `<%# Bind("UnitPrice") %>`. A desvantagem disso é que o preço não está mais formatado.
- Exiba o `UnitPrice` formatado como uma moeda na `ItemTemplate`, mas use a palavra-chave `Eval` para fazer isso. Lembre-se de que `Eval` executa a ligação de dados unidirecional. Ainda precisamos fornecer o valor `UnitPrice` para os valores originais, portanto, ainda precisaremos de uma instrução DataBinding bidirecional na `ItemTemplate`, mas isso pode ser colocado em um controle da Web de rótulo cuja propriedade `Visible` esteja definida como `false`. Poderíamos usar a seguinte marcação no ItemTemplate:

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample10.aspx)]

- Remova a formatação de moeda da `ItemTemplate`, usando `<%# Bind("UnitPrice") %>`. No manipulador de eventos `RowDataBound` do GridView, acesse o controle de rótulo da Web dentro do qual o valor de `UnitPrice` é exibido e defina sua propriedade `Text` como a versão formatada.
- Deixe o `UnitPrice` formatado como uma moeda. No manipulador de eventos `RowDeleting` do GridView, substitua o valor de `UnitPrice` original existente ($19.95) por um valor decimal real usando `Decimal.Parse`. Vimos como realizar algo semelhante no manipulador de eventos de `RowUpdating` na manipulação de [*exceções de nível de BLL e Dal em um tutorial de página do ASP.net*](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) .

Para meu exemplo, optei pela segunda abordagem, adicionando um controle da Web de rótulo oculto cuja propriedade `Text` é uma associação de dados bidirecional ao valor de `UnitPrice` não formatado.

Depois de resolver esse problema, tente clicar no botão excluir de qualquer produto novamente. Desta vez, você obterá um `InvalidOperationException` quando o ObjectDataSource tentar invocar o método `UpdateProduct` da BLL.

[![o ObjectDataSource não pode localizar um método com os parâmetros de entrada que deseja enviar](implementing-optimistic-concurrency-vb/_static/image45.png)](implementing-optimistic-concurrency-vb/_static/image44.png)

**Figura 16**: o ObjectDataSource não pode localizar um método com os parâmetros de entrada que deseja enviar ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image46.png))

Observando a mensagem da exceção, fica claro que o ObjectDataSource deseja invocar um método de `DeleteProduct` de BLL que inclui `original_CategoryName` e `original_SupplierName` parâmetros de entrada. Isso ocorre porque as `ItemTemplate` s para o `CategoryID` e `SupplierID` TemplateFields atualmente contêm instruções BIND de duas vias com os campos de dados `CategoryName` e `SupplierName`. Em vez disso, precisamos incluir `Bind` instruções com os campos de dados `CategoryID` e `SupplierID`. Para fazer isso, substitua as instruções BIND existentes por instruções `Eval` e, em seguida, adicione controles de rótulo ocultos cujas propriedades `Text` estejam associadas aos campos `CategoryID` e `SupplierID` dados usando DataBinding bidirecional, conforme mostrado abaixo:

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample11.aspx)]

Com essas alterações, agora é possível excluir e editar informações do produto com êxito! Na etapa 5, veremos como verificar se as violações de simultaneidade estão sendo detectadas. Mas, por enquanto, reserve alguns minutos para tentar atualizar e excluir alguns registros para garantir que a atualização e a exclusão de um único usuário funcione conforme o esperado.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Etapa 5: testando o suporte à simultaneidade otimista

Para verificar se as violações de simultaneidade estão sendo detectadas (em vez de resultar em dados sendo substituídos de forma oculta), precisamos abrir duas janelas de navegador nessa página. Em ambas as instâncias do navegador, clique no botão Editar para Chai. Em seguida, em apenas um dos navegadores, altere o nome para "Chai chá" e clique em atualizar. A atualização deve ter sucesso e retornar o GridView para seu estado de pré-edição, com "Chai chá" como o novo nome do produto.

Na outra instância de janela do navegador, no entanto, a caixa de texto nome do produto ainda mostra "Chai". Nessa segunda janela do navegador, atualize o `UnitPrice` para `25.00`. Sem o suporte de simultaneidade otimista, clicar em atualizar na segunda instância do navegador alteraria o nome do produto de volta para "Chai", substituindo assim as alterações feitas pela primeira instância do navegador. Com a simultaneidade otimista empregada, no entanto, clicar no botão atualizar na segunda instância do navegador resulta em um [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).

[![quando uma violação de simultaneidade é detectada, um DBConcurrencyException é gerado](implementing-optimistic-concurrency-vb/_static/image48.png)](implementing-optimistic-concurrency-vb/_static/image47.png)

**Figura 17**: quando uma violação de simultaneidade é detectada, um `DBConcurrencyException` é gerado ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image49.png))

O `DBConcurrencyException` é lançado somente quando o padrão de atualização em lotes da DAL é utilizado. O padrão de BD direto não gera uma exceção, ele simplesmente indica que nenhuma linha foi afetada. Para ilustrar isso, retorne o GridView das instâncias do navegador para o estado anterior à edição. Em seguida, na primeira instância do navegador, clique no botão Editar e altere o nome do produto de "Chai chá" de volta para "Chai" e clique em atualizar. Na segunda janela do navegador, clique no botão excluir para Chai.

Ao clicar em excluir, a página é postada novamente, o GridView invoca o método de `Delete()` do ObjectDataSource e o ObjectDataSource chama o método `DeleteProduct` da classe `ProductsOptimisticConcurrencyBLL`, passando os valores originais. O valor de `ProductName` original para a segunda instância do navegador é "Chai chá", que não corresponde ao valor de `ProductName` atual no banco de dados. Portanto, a instrução `DELETE` emitida para o banco de dados afeta zero linhas, já que não há registro no banco de dados que a cláusula `WHERE` atende. O método `DeleteProduct` retorna `false` e os dados do ObjectDataSource são reassociados ao GridView.

Da perspectiva do usuário final, clicar no botão excluir para o chá de Chai na segunda janela do navegador fez com que a tela fosse flash e, ao voltar, o produto ainda está lá, embora agora esteja listado como "Chai" (a alteração do nome do produto feita pelo primeiro navegador) instância). Se o usuário clicar no botão excluir novamente, a exclusão terá sucesso, pois o valor original de `ProductName` do GridView ("Chai") agora corresponde ao valor no banco de dados.

Em ambos os casos, a experiência do usuário está longe de ser a ideal. Claramente, não queremos mostrar ao usuário os detalhes essenciais da exceção de `DBConcurrencyException` ao usar o padrão de atualização do lote. E o comportamento ao usar o padrão de DB Direct é um pouco confuso, pois o comando Users falhou, mas não havia nenhuma indicação precisa do porquê.

Para corrigir esses dois problemas, podemos criar controles de rótulo da Web na página que fornecem uma explicação de por que uma atualização ou exclusão falhou. Para o padrão de atualização do lote, podemos determinar se uma exceção `DBConcurrencyException` ocorreu ou não no manipulador de eventos de pós-nível do GridView, exibindo o rótulo de aviso conforme necessário. Para o método de banco de dados direto, podemos examinar o valor de retorno do método BLL (que é `true` se uma linha foi afetada, `false` caso contrário) e exibir uma mensagem informativa, conforme necessário.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Etapa 6: adicionar mensagens informativas e exibi-las em face de uma violação de simultaneidade

Quando ocorre uma violação de simultaneidade, o comportamento exibido depende de o padrão de atualização do lote da DAL ou do DB Direct ser usado. Nosso tutorial usa ambos os padrões, com o padrão de atualização do lote que está sendo usado para atualização e o padrão de BD direto usado para exclusão. Para começar, vamos adicionar dois controles de rótulo da Web à nossa página que explicam que uma violação de simultaneidade ocorreu ao tentar excluir ou atualizar dados. Defina as propriedades `Visible` e `EnableViewState` do controle de rótulo como `false`; Isso fará com que eles sejam ocultados em cada página visitada, exceto para as visitas de página em particular em que sua propriedade `Visible` é definida de forma programática como `true`.

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample12.aspx)]

Além de definir suas propriedades `Visible`, `EnabledViewState`e `Text`, também defini a propriedade `CssClass` como `Warning`, o que faz com que o rótulo seja exibido em uma fonte grande, vermelha, itálico e negrito. Essa classe de `Warning` CSS foi definida e adicionada ao Styles. CSS de volta no tutorial *examinando os eventos associados com inserção, atualização e exclusão* .

Depois de adicionar esses rótulos, o designer no Visual Studio deve ser semelhante à figura 18.

[![dois controles rótulo foram adicionados à página](implementing-optimistic-concurrency-vb/_static/image51.png)](implementing-optimistic-concurrency-vb/_static/image50.png)

**Figura 18**: dois controles de rótulo foram adicionados à página ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image52.png))

Com esses controles da Web de rótulo em vigor, estamos prontos para examinar como determinar quando ocorreu uma violação de simultaneidade, no ponto em que a propriedade de `Visible` do rótulo apropriada pode ser definida como `true`, exibindo a mensagem informativa.

## <a name="handling-concurrency-violations-when-updating"></a>Tratamento de violações de simultaneidade ao atualizar

Primeiro, vamos examinar como lidar com violações de simultaneidade ao usar o padrão de atualização em lote. Como essas violações com o padrão de atualização do lote causam uma `DBConcurrencyException` exceção a ser lançada, precisamos adicionar código à nossa página ASP.NET para determinar se uma exceção `DBConcurrencyException` ocorreu durante o processo de atualização. Nesse caso, devemos exibir uma mensagem para o usuário explicando que suas alterações não foram salvas porque outro usuário modificou os mesmos dados entre quando eles começaram a editar o registro e quando clicaram no botão atualizar.

Como vimos na manipulação de *exceções de nível de BLL e Dal em um* tutorial de página do ASP.net, essas exceções podem ser detectadas e suprimidas nos manipuladores de eventos de nível posterior do controle da Web de dados. Portanto, precisamos criar um manipulador de eventos para o evento `RowUpdated` do GridView que verifica se uma exceção de `DBConcurrencyException` foi gerada. Esse manipulador de eventos recebe uma referência a qualquer exceção que foi gerada durante o processo de atualização, conforme mostrado no código do manipulador de eventos abaixo:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample13.vb)]

Diante de uma exceção de `DBConcurrencyException`, esse manipulador de eventos exibe o controle de rótulo de `UpdateConflictMessage` e indica que a exceção foi tratada. Com esse código em vigor, quando ocorre uma violação de simultaneidade durante a atualização de um registro, as alterações do usuário são perdidas, pois elas teriam substituídos as modificações de outro usuário ao mesmo tempo. Em particular, o GridView é retornado para seu estado de pré-edição e associado aos dados atuais do banco. Isso atualizará a linha GridView com as alterações do outro usuário, que não estavam visíveis anteriormente. Além disso, o controle rótulo de `UpdateConflictMessage` explicará ao usuário o que acabou de acontecer. Essa sequência de eventos é detalhada na Figura 19.

[![as atualizações de um usuário são perdidas na face de uma violação de simultaneidade](implementing-optimistic-concurrency-vb/_static/image54.png)](implementing-optimistic-concurrency-vb/_static/image53.png)

**Figura 19**: as atualizações de um usuário são perdidas na face de uma violação de simultaneidade ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image55.png))

> [!NOTE]
> Como alternativa, em vez de retornar o GridView para o estado anterior à edição, poderíamos deixar o GridView em seu estado de edição, definindo a propriedade `KeepInEditMode` do objeto de `GridViewUpdatedEventArgs` passado como true. No entanto, se você usar essa abordagem, certifique-se de associar os dados ao GridView (invocando seu método `DataBind()`) para que os valores do outro usuário sejam carregados na interface de edição. O código disponível para download com este tutorial tem estas duas linhas de código no manipulador de eventos `RowUpdated` comentado; Basta remover os comentários dessas linhas de código para que o GridView permaneça no modo de edição após uma violação de simultaneidade.

## <a name="responding-to-concurrency-violations-when-deleting"></a>Respondendo a violações de simultaneidade ao excluir

Com o padrão de BD direto, não há nenhuma exceção gerada na face de uma violação de simultaneidade. Em vez disso, a instrução Database simplesmente não afeta registros, pois a cláusula WHERE não corresponde a nenhum registro. Todos os métodos de modificação de dados criados na BLL foram projetados de forma que retornam um valor booliano que indica se eles afetaram precisamente um registro. Portanto, para determinar se uma violação de simultaneidade ocorreu ao excluir um registro, podemos examinar o valor de retorno do método de `DeleteProduct` da BLL.

O valor de retorno para um método BLL pode ser examinado nos manipuladores de eventos de pós-nível do ObjectDataSource por meio da propriedade `ReturnValue` do objeto `ObjectDataSourceStatusEventArgs` passado para o manipulador de eventos. Como estamos interessados em determinar o valor de retorno do método `DeleteProduct`, precisamos criar um manipulador de eventos para o evento `Deleted` do ObjectDataSource. A propriedade `ReturnValue` é do tipo `object` e pode ser `null` se uma exceção foi gerada e o método foi interrompido antes de poder retornar um valor. Portanto, primeiro devemos garantir que a propriedade `ReturnValue` não seja `null` e seja um valor booliano. Supondo que essa verificação passe, mostraremos o `DeleteConflictMessage` controle rótulo se o `ReturnValue` for `false`. Isso pode ser feito usando o seguinte código:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample14.vb)]

Diante de uma violação de simultaneidade, a solicitação de exclusão do usuário é cancelada. O GridView é atualizado, mostrando as alterações que ocorreram para esse registro entre o momento em que o usuário carregou a página e clicou no botão excluir. Quando essa violação ocorre, o rótulo de `DeleteConflictMessage` é mostrado, explicando o que acabou de acontecer (consulte a figura 20).

[![uma exclusão de usuário é cancelada diante de uma violação de simultaneidade](implementing-optimistic-concurrency-vb/_static/image57.png)](implementing-optimistic-concurrency-vb/_static/image56.png)

**Figura 20**: uma exclusão do usuário é cancelada diante de uma violação de simultaneidade ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-vb/_static/image58.png))

## <a name="summary"></a>Resumo

Existem oportunidades para violações de simultaneidade em todos os aplicativos que permitem que vários usuários simultâneos atualizem ou excluam dados. Se essas violações não forem levadas em conta, quando dois usuários atualizarem simultaneamente os mesmos dados que receberem na última gravação "WINS", substituindo as alterações de alterações do outro usuário. Como alternativa, os desenvolvedores podem implementar o controle de simultaneidade otimista ou pessimista. O controle de simultaneidade otimista pressupõe que as violações de simultaneidade são infrequentes e simplesmente não permite um comando Update ou DELETE que constituiria uma violação de simultaneidade. O controle de simultaneidade pessimista pressupõe que as violações de simultaneidade são frequentes e simplesmente rejeitar o comando de atualização ou exclusão de um usuário não é aceitável. Com o controle de simultaneidade pessimista, a atualização de um registro envolve bloqueá-lo, impedindo que outros usuários modifiquem ou excluam o registro enquanto estiverem bloqueados.

O DataSet tipado no .NET fornece a funcionalidade para dar suporte ao controle de simultaneidade otimista. Em particular, as instruções `UPDATE` e `DELETE` emitidas para o banco de dados incluem todas as colunas da tabela, garantindo assim que a atualização ou exclusão só ocorrerá se os dados atuais do registro forem correspondentes aos dados originais que o usuário tinha ao executar a atualização ou exclusão. Depois que a DAL tiver sido configurada para dar suporte à simultaneidade otimista, os métodos de BLL precisarão ser atualizados. Além disso, a página ASP.NET que chama para baixo na BLL deve ser configurada de modo que o ObjectDataSource recupere os valores originais de seu controle da Web de dados e os passe para a BLL.

Como vimos neste tutorial, implementar o controle de simultaneidade otimista em um aplicativo Web ASP.NET envolve atualizar a DAL e a BLL e adicionar suporte na página ASP.NET. Esse trabalho adicionado ou não é um investimento inteligente de tempo e esforço depende do seu aplicativo. Se você raramente tiver usuários simultâneos atualizando dados ou se os dados que eles estão atualizando forem diferentes uns dos outros, o controle de simultaneidade não será um problema importante. No entanto, se você tiver uma rotina de vários usuários em seu site trabalhando com os mesmos dados, o controle de simultaneidade poderá ajudar a impedir que as atualizações ou exclusões de um usuário reescrevam de outra involuntariamente.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](customizing-the-data-modification-interface-vb.md)
> [Próximo](adding-client-side-confirmation-when-deleting-vb.md)
