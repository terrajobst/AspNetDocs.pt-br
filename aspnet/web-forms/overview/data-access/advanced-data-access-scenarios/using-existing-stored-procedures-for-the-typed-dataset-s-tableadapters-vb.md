---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Usando procedimentos armazenados existentes para os TableAdapters do DataSet tipados (VB) | Microsoft Docs
author: rick-anderson
description: No tutorial anterior, aprendemos como usar o assistente TableAdapter para gerar novos procedimentos armazenados. Neste tutorial, aprendemos como o mesmo TableAdapter...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: e35c3d6a98516a07f6119e6cb9dbeb99bc28fe33
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78531797"
---
# <a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Uso de procedimentos armazenados existentes para os TableAdapters do conjunto de dados tipado (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) ou [baixar PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> No tutorial anterior, aprendemos como usar o assistente TableAdapter para gerar novos procedimentos armazenados. Neste tutorial, aprendemos como o mesmo assistente do TableAdapter pode trabalhar com procedimentos armazenados existentes. Também aprendemos como adicionar manualmente novos procedimentos armazenados ao nosso banco de dados.

## <a name="introduction"></a>Introdução

No [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) , vimos como os TableAdapters do DataSet tipados podiam ser configurados para usar procedimentos armazenados para acessar dados em vez de instruções SQL ad hoc. Em particular, examinamos como fazer com que o assistente TableAdapter crie automaticamente esses procedimentos armazenados. Ao portar um aplicativo herdado para o ASP.NET 2,0 ou ao criar um site do ASP.NET 2,0 em um modelo de dados existente, é provável que o Database já contenha os procedimentos armazenados de que precisamos. Como alternativa, você pode preferir criar seus procedimentos armazenados manualmente ou por meio de alguma ferramenta que não seja o assistente do TableAdapter que gera automaticamente os procedimentos armazenados.

Neste tutorial, veremos como configurar o TableAdapter para usar procedimentos armazenados existentes. Como o banco de dados Northwind tem apenas um pequeno conjunto de procedimentos armazenados internos, veremos também as etapas necessárias para adicionar manualmente novos procedimentos armazenados ao banco de dados por meio do ambiente do Visual Studio. Vamos começar!

> [!NOTE]
> Nas [modificações de banco de dados de disposição em um](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) tutorial de transação, adicionamos métodos ao TableAdapter para dar suporte a transações (`BeginTransaction`, `CommitTransaction`e assim por diante). Como alternativa, as transações podem ser gerenciadas inteiramente dentro de um procedimento armazenado, o que não requer modificações no código da camada de acesso a dados. Neste tutorial, exploraremos os comandos T-SQL usados para executar uma instrução de procedimento armazenado s dentro do escopo de uma transação.

## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Etapa 1: adicionando procedimentos armazenados ao banco de dados Northwind

O Visual Studio facilita a adição de novos procedimentos armazenados a um banco de dados. Vamos adicionar um novo procedimento armazenado ao banco de dados Northwind que retorna todas as colunas da tabela `Products` para aqueles que têm um valor de `CategoryID` específico. Na janela Gerenciador de Servidores, expanda o banco de dados Northwind para que suas pastas-diagramas de banco de dados, tabelas, exibições e assim por diante-sejam exibidos. Como vimos no tutorial anterior, a pasta procedimentos armazenados contém os procedimentos armazenados existentes do banco de dados. Para adicionar um novo procedimento armazenado, basta clicar com o botão direito do mouse na pasta procedimentos armazenados e escolher a opção Adicionar novo procedimento armazenado no menu de contexto.

[![clique com o botão direito do mouse na pasta procedimentos armazenados e adicione um novo procedimento armazenado](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Figura 1**: clique com o botão direito do mouse na pasta procedimentos armazenados e adicione um novo procedimento armazenado ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))

Como mostra a Figura 1, a seleção da opção Adicionar novo procedimento armazenado abre uma janela de script no Visual Studio com o contorno do script SQL necessário para criar o procedimento armazenado. É nosso trabalho elaborar esse script e executá-lo, ponto em que o procedimento armazenado será adicionado ao banco de dados.

Insira o seguinte script:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Esse script, quando executado, adicionará um novo procedimento armazenado ao banco de dados Northwind chamado `Products_SelectByCategoryID`. Esse procedimento armazenado aceita um único parâmetro de entrada (`@CategoryID`, do tipo `int`) e retorna todos os campos para esses produtos com um valor `CategoryID` correspondente.

Para executar esse `CREATE PROCEDURE` script e adicionar o procedimento armazenado ao banco de dados, clique no ícone salvar na barra de ferramentas ou pressione Ctrl + S. Depois de fazer isso, a pasta procedimentos armazenados é atualizada, mostrando o procedimento armazenado recém-criado. Além disso, o script na janela mudará as sutilezas de `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` para `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` adiciona um novo procedimento armazenado ao banco de dados, enquanto `ALTER PROCEDURE` atualiza um existente. Como o início do script foi alterado para `ALTER PROCEDURE`, alterar os parâmetros de entrada de procedimentos armazenados ou instruções SQL e clicar no ícone salvar atualizará o procedimento armazenado com essas alterações.

A Figura 2 mostra o Visual Studio depois que o procedimento armazenado `Products_SelectByCategoryID` foi salvo.

[![o procedimento armazenado Products_SelectByCategoryID foi adicionado ao banco de dados](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**Figura 2**: o procedimento armazenado `Products_SelectByCategoryID` foi adicionado ao banco de dados ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))

## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Etapa 2: Configurando o TableAdapter para usar um procedimento armazenado existente

Agora que o procedimento armazenado `Products_SelectByCategoryID` foi adicionado ao banco de dados, é possível configurar nossa camada de acesso de dado para usar esse procedimento armazenado quando um de seus métodos for invocado. Em particular, adicionaremos um método `GetProductsByCategoryID(<_i22_>categoryID)<!--_i22_-->` à `ProductsTableAdapter` no conjunto de dado `NorthwindWithSprocs` tipado que chama o procedimento armazenado `Products_SelectByCategoryID` que acabamos de criar.

Comece abrindo o conjunto de `NorthwindWithSprocs` DataSet. Clique com o botão direito do mouse na `ProductsTableAdapter` e escolha Adicionar consulta para iniciar o assistente de configuração de consulta do TableAdapter. No [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) , optamos por fazer com que o TableAdapter crie um novo procedimento armazenado para nós. Para este tutorial, no entanto, queremos conectar o novo método TableAdapter ao procedimento armazenado `Products_SelectByCategoryID` existente. Portanto, escolha a opção usar procedimento armazenado existente na primeira etapa do assistente e clique em Avançar.

[![escolher a opção usar procedimento armazenado existente](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Figura 3**: escolha a opção usar procedimento armazenado existente ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))

A tela a seguir fornece uma lista suspensa preenchida com os procedimentos armazenados de s do banco de dados. A seleção de um procedimento armazenado lista seus parâmetros de entrada à esquerda e os campos de dados retornados (se houver) à direita. Escolha o `Products_SelectByCategoryID` procedimento armazenado na lista e clique em Avançar.

[![escolher o procedimento armazenado Products_SelectByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Figura 4**: escolha o procedimento armazenado `Products_SelectByCategoryID` ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))

A próxima tela nos pergunta que tipo de dados é retornado pelo procedimento armazenado e nossa resposta aqui determina o tipo retornado pelo método TableAdapter s. Por exemplo, se indicarmos que dados tabulares são retornados, o método retornará um `ProductsDataTable` instância populada com os registros retornados pelo procedimento armazenado. Por outro lado, se indicarmos que esse procedimento armazenado retorna um único valor, o TableAdapter retornará um `Object` que recebe o valor na primeira coluna do primeiro registro retornado pelo procedimento armazenado.

Como o procedimento armazenado `Products_SelectByCategoryID` retorna todos os produtos que pertencem a uma determinada categoria, escolha os dados da primeira resposta-tabela e clique em Avançar.

[![indicar que o procedimento armazenado retorna dados tabulares](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Figura 5**: indicar que o procedimento armazenado retorna dados tabulares ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))

Tudo o que resta é indicar quais padrões de método usar seguidos pelos nomes para esses métodos. Deixe o preenchimento de uma DataTable e retorne as opções de DataTable marcadas, mas renomeie os métodos para `FillByCategoryID` e `GetProductsByCategoryID`. Em seguida, clique em avançar para examinar um resumo das tarefas que o assistente executará. Se tudo parecer correto, clique em concluir.

[![nomear os métodos FillByCategoryID e GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Figura 6**: nomear os métodos `FillByCategoryID` e `GetProductsByCategoryID` ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))

> [!NOTE]
> Os métodos do TableAdapter que acabamos de criar, `FillByCategoryID` e `GetProductsByCategoryID`, esperam um parâmetro de entrada do tipo `Integer`. Esse valor de parâmetro de entrada é passado para o procedimento armazenado por meio de seu parâmetro `@CategoryID`. Se você modificar os parâmetros de `Products_SelectByCategory` dos procedimentos armazenados, também precisará atualizar os parâmetros para esses métodos do TableAdapter. Conforme discutido no [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md), isso pode ser feito de duas maneiras: adicionando ou removendo manualmente os parâmetros da coleção de parâmetros ou executando novamente o assistente TableAdapter.

## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Etapa 3: adicionando um método`GetProductsByCategoryID(categoryID)`à BLL

Com o método DAL `GetProductsByCategoryID` concluído, a próxima etapa é fornecer acesso a esse método na camada de lógica de negócios. Abra o arquivo de classe `ProductsBLLWithSprocs` e adicione o seguinte método:

[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Esse método BLL simplesmente retorna o `ProductsDataTable` retornado do método `ProductsTableAdapter` s `GetProductsByCategoryID`. O atributo `DataObjectMethodAttribute` fornece metadados usados pelo assistente de configuração de fonte de dados ObjectDataSource s. Em particular, esse método aparecerá na lista suspensa Selecionar Tab s.

## <a name="step-4-displaying-products-by-category"></a>Etapa 4: exibindo produtos por categoria

Para testar o procedimento armazenado `Products_SelectByCategoryID` recém adicionado e os métodos DAL e BLL correspondentes, vamos criar uma página ASP.NET que contenha DropDownList e GridView. O DropDownList listará todas as categorias no banco de dados enquanto o GridView exibirá os produtos que pertencem à categoria selecionada.

> [!NOTE]
> Nós criamos interfaces mestras/de detalhes usando DropDownLists nos tutoriais anteriores. Para obter uma visão mais detalhada da implementação desse relatório mestre/detalhado, consulte a [filtragem mestre/detalhes com um tutorial DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) .

Abra a página `ExistingSprocs.aspx` na pasta `AdvancedDAL` e arraste uma DropDownList da caixa de ferramentas para o designer. Defina a propriedade de `ID` de DropDownList s como `Categories` e sua propriedade `AutoPostBack` como `True`. Em seguida, em sua marca inteligente, associe o DropDownList a um novo ObjectDataSource chamado `CategoriesDataSource`. Configure o ObjectDataSource para que ele recupere seus dados do método de `GetCategories` da classe `CategoriesBLL`. Defina as listas suspensas nas guias atualizar, inserir e excluir como (nenhum).

[![recuperar dados do método GetCategories da classe CategoriesBLL](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Figura 7**: recuperar dados do método de `GetCategories` da classe `CategoriesBLL` ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))

[![definir as listas suspensas nas guias atualizar, inserir e excluir para (nenhum)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Figura 8**: definir as listas suspensas nas guias atualizar, inserir e excluir para (nenhum) ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))

Depois de concluir o assistente ObjectDataSource, configure o DropDownList para exibir o `CategoryName` campo de dados e usar o campo `CategoryID` como o `Value` para cada `ListItem`.

Neste ponto, a marcação declarativa de DropDownList e ObjectDataSource s deve ser semelhante ao seguinte:

[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

Em seguida, arraste um GridView para o designer, colocando-o sob a DropDownList. Defina o `ID` s de GridView como `ProductsByCategory` e, em sua marca inteligente, associe-o a um novo ObjectDataSource chamado `ProductsByCategoryDataSource`. Configure o `ProductsByCategoryDataSource` ObjectDataSource para usar a classe `ProductsBLLWithSprocs`, fazendo com que ele recupere seus dados usando o método `GetProductsByCategoryID(categoryID)`. Como esse GridView será usado somente para exibir dados, defina as listas suspensas nas guias UPDATE, INSERT e DELETE como (nenhum) e clique em Avançar.

[![configurar o ObjectDataSource para usar a classe ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Figura 9**: configurar o ObjectDataSource para usar a classe `ProductsBLLWithSprocs` ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))

[![recuperar dados do método GetProductsByCategoryID (categoryID)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Figura 10**: recuperar dados do método `GetProductsByCategoryID(categoryID)` ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))

O método escolhido na guia selecionar espera um parâmetro, portanto, a etapa final do assistente nos solicita a origem do parâmetro. Defina a lista suspensa origem do parâmetro para controlar e escolha o controle de `Categories` na lista suspensa ControlID. Clique em Concluir para finalizar o assistente.

[![usar as categorias DropDownList como a origem do parâmetro categoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Figura 11**: Use o `Categories` DropDownList como a origem do parâmetro `categoryID` ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))

Após a conclusão do assistente ObjectDataSource, o Visual Studio adicionará BoundFields e um CheckBoxField para cada um dos campos de dados do produto. Sinta-se à vontade para personalizar esses campos como você pode ver.

Visite a página por meio de um navegador. Ao visitar a página, a categoria bebidas é selecionada e os produtos correspondentes listados na grade. Alterar a lista suspensa para uma categoria alternativa, como mostra a Figura 12, causa um postback e recarrega a grade com os produtos da categoria selecionada recentemente.

[![os produtos na categoria produzir são exibidos](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Figura 12**: os produtos na categoria produzir são exibidos ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))

## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Etapa 5: encapsulando as instruções s de um procedimento armazenado dentro do escopo de uma transação

Nas [modificações de banco de dados de disposição em um](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) tutorial de transação, discutimos técnicas para executar uma série de instruções de modificação de banco de dados dentro do escopo de uma transação. Lembre-se de que as modificações executadas sob a proteção de uma transação são todas com êxito ou todas falha, garantindo a atomicidade. As técnicas para o uso de transações incluem:

- Usando as classes no namespace `System.Transactions`,
- Fazer com que a camada de acesso a dados use classes ADO.NET como `SqlTransaction`e
- Adicionando os comandos de transação T-SQL diretamente no procedimento armazenado

As *modificações do banco de dados de disposição em um* tutorial de transação usaram as classes ADO.net na Dal. O restante deste tutorial examina como gerenciar uma transação usando comandos T-SQL de dentro de um procedimento armazenado.

Os três comandos principais do SQL para iniciar, confirmar e reverter uma transação manualmente são `BEGIN TRANSACTION`, `COMMIT TRANSACTION`e `ROLLBACK TRANSACTION`, respectivamente. Assim como com a abordagem ADO.NET, ao usar transações de dentro de um procedimento armazenado, precisamos aplicar o seguinte padrão:

1. Indique o início de uma transação.
2. Execute as instruções SQL que compõem a transação.
3. Se houver um erro em qualquer uma das instruções da etapa 2, reverta a transação.
4. Se todas as instruções da etapa 2 forem concluídas sem erros, confirme a transação.

Esse padrão pode ser implementado na sintaxe T-SQL usando o seguinte modelo:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

O modelo começa definindo um bloco de `TRY...CATCH`, uma construção nova para SQL Server 2005. Assim como com `Try...Catch` blocos no Visual Basic, o bloco de `TRY...CATCH` do SQL executa as instruções no bloco de `TRY`. Se qualquer instrução gerar um erro, o controle será transferido imediatamente para o bloco de `CATCH`.

Se não houver erros ao executar as instruções SQL que fazem a composição da transação, a instrução `COMMIT TRANSACTION` confirmará as alterações e concluirá a transação. No entanto, se uma das instruções resultar em um erro, a `ROLLBACK TRANSACTION` no bloco de `CATCH` retornará o banco de dados ao seu estado antes do início da transação. O procedimento armazenado também gera um erro usando o [comando RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), que faz com que um `SqlException` seja gerado no aplicativo.

> [!NOTE]
> Como o bloco de `TRY...CATCH` é novo no SQL Server 2005, o modelo acima não funcionará se você estiver usando versões anteriores do Microsoft SQL Server. Se você não estiver usando SQL Server 2005, consulte [Gerenciando transações em SQL Server procedimentos armazenados](http://www.4guysfromrolla.com/webtech/080305-1.shtml) para um modelo que funcionará com outras versões do SQL Server.

Vamos examinar um exemplo concreto. Existe uma restrição FOREIGN KEY entre as tabelas `Categories` e `Products`, o que significa que cada campo `CategoryID` na tabela `Products` deve ser mapeado para um valor de `CategoryID` na tabela `Categories`. Qualquer ação que viole essa restrição, como tentar excluir uma categoria que tenha produtos associados, resultará em uma violação de restrição de chave estrangeira. Para verificar isso, reveja o exemplo atualizando e excluindo dados binários existentes na seção trabalhando com dados binários (`~/BinaryData/UpdatingAndDeleting.aspx`). Esta página lista cada categoria no sistema, juntamente com os botões editar e excluir (consulte a Figura 13), mas se você tentar excluir uma categoria que tenha produtos associados, como bebidas, a exclusão falhará devido a uma violação de restrição de chave estrangeira (consulte a Figura 14).

[![cada categoria é exibida em um GridView com botões editar e excluir](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Figura 13**: cada categoria é exibida em um GridView com botões editar e excluir ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))

[![não é possível excluir uma categoria que tenha produtos existentes](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Figura 14**: não é possível excluir uma categoria que tenha produtos existentes ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))

No entanto, imagine que desejamos permitir que as categorias sejam excluídas independentemente de terem produtos associados. Caso uma categoria com produtos seja excluída, imagine que queremos também excluir seus produtos existentes (embora outra opção seja simplesmente definir seus produtos `CategoryID` valores como `NULL`). Essa funcionalidade pode ser implementada por meio das regras em cascata da restrição FOREIGN KEY. Como alternativa, poderíamos criar um procedimento armazenado que aceita um parâmetro de entrada `@CategoryID` e, quando invocado, exclui explicitamente todos os produtos associados e, em seguida, a categoria especificada.

Nossa primeira tentativa nesse procedimento armazenado desse tipo pode ser semelhante ao seguinte:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Embora isso definitivamente exclua os produtos e a categoria associados, ele não faz isso sob a abrangência de uma transação. Imagine que haja alguma outra restrição FOREIGN KEY em `Categories` que proíba a exclusão de um determinado valor de `@CategoryID`. O problema é que, nesse caso, todos os produtos serão excluídos antes de tentarmos excluir a categoria. O resultado final é que, para essa categoria, esse procedimento armazenado removeria todos os seus produtos enquanto a categoria permanecesse, pois ele ainda tem registros relacionados em alguma outra tabela.

No entanto, se o procedimento armazenado fosse encapsulado no escopo de uma transação, as exclusões para a tabela de `Products` seriam revertidas na face de uma exclusão com falha em `Categories`. O script de procedimento armazenado a seguir usa uma transação para garantir a atomicidade entre as duas instruções `DELETE`:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

Reserve um tempo para adicionar o procedimento armazenado `Categories_Delete` ao banco de dados Northwind. Consulte novamente a etapa 1 para obter instruções sobre como adicionar procedimentos armazenados a um banco de dados.

## <a name="step-6-updating-thecategoriestableadapter"></a>Etapa 6: atualizando o`CategoriesTableAdapter`

Enquanto adicionamos o procedimento armazenado `Categories_Delete` ao banco de dados, a DAL está atualmente configurada para usar instruções SQL ad hoc para executar a exclusão. Precisamos atualizar o `CategoriesTableAdapter` e instruí-lo para usar o procedimento armazenado `Categories_Delete`.

> [!NOTE]
> Anteriormente neste tutorial, estávamos trabalhando com o conjunto de `NorthwindWithSprocs` DataSet. Mas esse conjunto de um só tem uma única entidade, `ProductsDataTable`e precisamos trabalhar com categorias. Portanto, para o restante deste tutorial, quando falo sobre a camada de acesso a dados que m me referindo ao conjunto de dados `Northwind`, aquele que criamos pela primeira vez no tutorial [criando uma camada de acesso ao dado](../introduction/creating-a-data-access-layer-vb.md) .

Abra o conjunto de uma Northwind, selecione o `CategoriesTableAdapter`e vá para a janela Propriedades. O janela Propriedades lista os `InsertCommand`, `UpdateCommand`, `DeleteCommand`e `SelectCommand` usados pelo TableAdapter, bem como suas informações de nome e conexão. Expanda a propriedade `DeleteCommand` para ver seus detalhes. Como mostra a Figura 15, a propriedade `DeleteCommand` s `CommandType` é definida como texto, o que o instrui a enviar o texto na propriedade `CommandText` como uma consulta SQL ad hoc.

![Selecione o CategoriesTableAdapter no designer para exibir suas propriedades na janela Propriedades](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Figura 15**: selecionar a `CategoriesTableAdapter` no designer para exibir suas propriedades na janela Propriedades

Para alterar essas configurações, selecione o texto (DeleteCommand) na janela Propriedades e escolha (novo) na lista suspensa. Isso limpará as configurações das propriedades `CommandText`, `CommandType`e `Parameters`. Em seguida, defina a propriedade `CommandType` como `StoredProcedure` e digite o nome do procedimento armazenado para a `CommandText` (`dbo.Categories_Delete`). Se você tiver certeza de inserir as propriedades nesta ordem – primeiro o `CommandType` e, em seguida, o `CommandText`-Visual Studio preencherá automaticamente a coleção de parâmetros. Se você não inserir essas propriedades nesta ordem, precisará adicionar manualmente os parâmetros por meio do editor de coleção de parâmetros. Em ambos os casos, é prudente clicar nas reticências na Propriedade Parameters para abrir o editor de coleção de parâmetros para verificar se as alterações corretas das configurações de parâmetro foram feitas (consulte a Figura 16). Se você não vir nenhum parâmetro na caixa de diálogo, adicione o parâmetro `@CategoryID` manualmente (você não precisa adicionar o parâmetro `@RETURN_VALUE`).

![Verifique se as configurações de parâmetros estão corretas](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Figura 16**: verificar se as configurações de parâmetros estão corretas

Depois que a DAL tiver sido atualizada, a exclusão de uma categoria excluirá automaticamente todos os seus produtos associados e fará isso sob a proteção de uma transação. Para verificar isso, retorne para a página atualizando e excluindo dados binários existentes e clique no botão excluir de uma das categorias. Com um único clique do mouse, a categoria e todos os seus produtos associados serão excluídos.

> [!NOTE]
> Antes de testar o procedimento armazenado `Categories_Delete`, que excluirá vários produtos junto com a categoria selecionada, pode ser prudente fazer uma cópia de backup do banco de dados. Se você estiver usando o banco de dados `NORTHWND.MDF` no `App_Data`, basta fechar o Visual Studio e copiar os arquivos MDF e LDF no `App_Data` para alguma outra pasta. Depois de testar a funcionalidade, você pode restaurar o banco de dados fechando o Visual Studio e substituindo os arquivos MDF e LDF atuais em `App_Data` com as cópias de backup.

## <a name="summary"></a>Resumo

Embora o assistente do TableAdapter s gere automaticamente procedimentos armazenados para nós, há ocasiões em que talvez já tenhamos esses procedimentos armazenados criados ou desejam criá-los manualmente ou com outras ferramentas. Para acomodar esses cenários, o TableAdapter também pode ser configurado para apontar para um procedimento armazenado existente. Neste tutorial, vimos como adicionar manualmente procedimentos armazenados a um banco de dados por meio do ambiente do Visual Studio e como conectar os métodos do TableAdapter s a esses procedimentos armazenados. Também examinamos os comandos T-SQL e o padrão de script usados para iniciar, confirmar e reverter transações de dentro de um procedimento armazenado.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Hilton Geisenow, S Ren Jacob Lauritsen e Teresa Murphy. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Próximo](updating-the-tableadapter-to-use-joins-vb.md)
