---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
title: Criando uma camada de lógica deC#negócios () | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como centralizar suas regras de negócios em uma camada de lógica de negócios (BLL) que serve como um intermediário para troca de dados entre t...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 85554606-47cb-4e4f-9848-eed9da579056
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: df96f3e7422a0537bf1b003a33fe8d71a671ac33
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74586188"
---
# <a name="creating-a-business-logic-layer-c"></a>Criação de uma camada de lógica de negócios (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_2_CS.exe) ou [baixar PDF](creating-a-business-logic-layer-cs/_static/datatutorial02cs1.pdf)

> Neste tutorial, veremos como centralizar suas regras de negócios em uma camada de lógica de negócios (BLL) que serve como um intermediário para troca de dados entre a camada de apresentação e a DAL.

## <a name="introduction"></a>Introdução

A DAL (camada de acesso a dados) criada no [primeiro tutorial](creating-a-data-access-layer-cs.md) separa de forma limpa a lógica de acesso a dados da lógica de apresentação. No entanto, embora a DAL separe de forma limpa os detalhes de acesso a dados da camada de apresentação, ela não impõe nenhuma regra de negócio que possa ser aplicada. Por exemplo, para nosso aplicativo, talvez queiramos não permitir que os campos `CategoryID` ou `SupplierID` da tabela de `Products` sejam modificados quando o campo `Discontinued` estiver definido como 1, ou talvez queiramos impor regras de senioridade, proibindo situações em que um funcionário é gerenciado por alguém que foi contratado depois delas. Outro cenário comum é a autorização, talvez somente os usuários em uma função específica possam excluir produtos ou alterar o valor de `UnitPrice`.

Neste tutorial, veremos como centralizar essas regras de negócio em uma camada de lógica de negócios (BLL) que serve como um intermediário para troca de dados entre a camada de apresentação e a DAL. Em um aplicativo do mundo real, a BLL deve ser implementada como um projeto de biblioteca de classes separado; no entanto, para esses tutoriais, implementaremos a BLL como uma série de classes em nossa pasta `App_Code` para simplificar a estrutura do projeto. A Figura 1 ilustra as relações de arquitetura entre a camada de apresentação, a BLL e a DAL.

![A BLL separa a camada de apresentação da camada de acesso a dados e impõe regras de negócio](creating-a-business-logic-layer-cs/_static/image1.png)

**Figura 1**: a BLL separa a camada de apresentação da camada de acesso a dados e impõe regras de negócio

## <a name="step-1-creating-the-bll-classes"></a>Etapa 1: criando as classes de BLL

Nossa BLL será composta de quatro classes, uma para cada TableAdapter na DAL; cada uma dessas classes de BLL terá métodos para recuperar, inserir, atualizar e excluir do respectivo TableAdapter na DAL, aplicando as regras de negócios apropriadas.

Para separar com mais clareza as classes relacionadas à DAL e à BLL, vamos criar duas subpastas na pasta `App_Code`, `DAL` e `BLL`. Basta clicar com o botão direito do mouse na pasta `App_Code` na Gerenciador de Soluções e escolher nova pasta. Depois de criar essas duas pastas, mova o conjunto de tipos criado no primeiro tutorial para a subpasta `DAL`.

Em seguida, crie os quatro arquivos da classe BLL na subpasta `BLL`. Para fazer isso, clique com o botão direito do mouse na subpasta `BLL`, escolha Adicionar um novo item e escolha o modelo de classe. Nomeie as quatro classes `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`e `EmployeesBLL`.

![Adicione quatro novas classes à pasta App_Code](creating-a-business-logic-layer-cs/_static/image2.png)

**Figura 2**: adicionar quatro novas classes à pasta `App_Code`

Em seguida, vamos adicionar métodos a cada uma das classes para simplesmente encapsular os métodos definidos para os TableAdapters do primeiro tutorial. Por enquanto, esses métodos simplesmente serão chamados diretamente para a DAL; Retornaremos mais tarde para adicionar qualquer lógica de negócios necessária.

> [!NOTE]
> Se você estiver usando o Visual Studio Standard Edition ou superior (ou seja, *não* estiver usando o Visual Web Developer), você pode, opcionalmente, criar suas classes visualmente usando o [Designer de classe](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Consulte o [Blog designer de classe](https://blogs.msdn.com/classdesigner/default.aspx) para obter mais informações sobre esse novo recurso no Visual Studio.

Para a classe `ProductsBLL` precisamos adicionar um total de sete métodos:

- `GetProducts()` retorna todos os produtos
- `GetProductByProductID(productID)` retorna o produto com a ID de produto especificada
- `GetProductsByCategoryID(categoryID)` retorna todos os produtos da categoria especificada
- `GetProductsBySupplier(supplierID)` retorna todos os produtos do fornecedor especificado
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` insere um novo produto no banco de dados usando os valores passados; Retorna o valor de `ProductID` do registro recentemente inserido
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` atualiza um produto existente no banco de dados usando os valores passados; Retorna `true` se precisamente uma linha foi atualizada, `false` caso contrário
- `DeleteProduct(productID)` exclui o produto especificado do banco de dados

ProductsBLL.cs

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample1.cs)]

Os métodos que simplesmente retornam dados `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`e `GetProductBySuppliersID` são razoavelmente simples, pois eles simplesmente chamam a DAL. Embora, em alguns cenários, possa haver regras de negócios que precisam ser implementadas nesse nível (como as regras de autorização com base no usuário conectado no momento ou a função à qual o usuário pertence), simplesmente deixaremos esses métodos como estão. Para esses métodos, a BLL serve apenas como um proxy por meio do qual a camada de apresentação acessa os dados subjacentes da camada de acesso a dados.

Os métodos `AddProduct` e `UpdateProduct` usam como parâmetros os valores para os vários campos de produto e adicionam um novo produto ou atualizam um existente, respectivamente. Como muitas das colunas da tabela `Product` podem aceitar valores de `NULL` (`CategoryID`, `SupplierID`e `UnitPrice`, para citar alguns), esses parâmetros de entrada para `AddProduct` e `UpdateProduct` que são mapeados para essas colunas usam [tipos anuláveis](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Os tipos anuláveis são novos no .NET 2,0 e fornecem uma técnica para indicar se um tipo de valor deve, em vez disso, ser `null`. No C# , você pode sinalizar um tipo de valor como um tipo anulável adicionando `?` após o tipo (como `int? x;`). Consulte a seção [tipos anuláveis](https://msdn.microsoft.com/library/1t3y8s4s.aspx) no [ C# guia de programação](https://msdn.microsoft.com/library/67ef8sbd%28VS.80%29.aspx) para obter mais informações.

Todos os três métodos retornam um valor booliano que indica se uma linha foi inserida, atualizada ou excluída, uma vez que a operação pode não resultar em uma linha afetada. Por exemplo, se o desenvolvedor da página chamar `DeleteProduct` passando um `ProductID` para um produto não existente, a instrução `DELETE` emitida para o banco de dados não terá nenhum efeito e, portanto, o método `DeleteProduct` retornará `false`.

Observe que, ao adicionar um novo produto ou atualizar um existente, pegamos os valores de campo do produto novo ou modificado como uma lista de escalares em vez de aceitar uma instância de `ProductsRow`. Essa abordagem foi escolhida porque a classe `ProductsRow` deriva da classe ADO.NET `DataRow`, que não tem um construtor padrão sem parâmetros. Para criar uma nova instância de `ProductsRow`, primeiro devemos criar uma instância de `ProductsDataTable` e, em seguida, invocar seu método de `NewProductRow()` (que fazemos no `AddProduct`). Essa deficiência coloca seu cabeçalho na parte, quando podemos inserir e atualizar produtos usando o ObjectDataSource. Em suma, o ObjectDataSource tentará criar uma instância dos parâmetros de entrada. Se o método BLL espera uma instância de `ProductsRow`, o ObjectDataSource tentará criar uma, mas falhará devido à falta de um construtor padrão sem parâmetros. Para obter mais informações sobre esse problema, consulte as duas Postagens de fóruns ASP.NET a seguir: [atualizando ObjectDataSource com conjuntos de dados fortemente tipados](https://forums.asp.net/1098630/ShowPost.aspx)e [problema com ObjectDataSource e DataSet fortemente tipado](https://forums.asp.net/1048212/ShowPost.aspx).

Em seguida, em `AddProduct` e `UpdateProduct`, o código cria uma instância de `ProductsRow` e a popula com os valores que acabamos de passar. Quando são atribuídas valores a colunas de uma DataRow, várias verificações de validação no nível de campo podem ocorrer. Portanto, colocar manualmente os valores passados de volta em uma DataRow ajuda a garantir a validade dos dados que estão sendo passados para o método BLL. Infelizmente, as classes DataRow fortemente tipadas geradas pelo Visual Studio não usam tipos anuláveis. Em vez disso, para indicar que uma DataColumn específica em uma DataRow deve corresponder a um valor de banco de dados `NULL`, devemos usar o método `SetColumnNameNull()`.

No `UpdateProduct` primeiro carregamos no produto para atualizar usando `GetProductByProductID(productID)`. Embora isso possa parecer uma viagem desnecessária para o banco de dados, essa viagem extra se comprovará em Tutoriais futuros que exploram simultaneidade otimista. A simultaneidade otimista é uma técnica para garantir que dois usuários que trabalham simultaneamente nos mesmos dados não substituam acidentalmente as alterações de outro. A captura de todo o registro também facilita a criação de métodos de atualização na BLL que modificam apenas um subconjunto das colunas da DataRow. Quando exploramos a classe `SuppliersBLL`, veremos um exemplo desse tipo.

Por fim, observe que a classe `ProductsBLL` tem o [atributo DataObject](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) aplicado a ele (a sintaxe de `[System.ComponentModel.DataObject]` logo antes da instrução de classe na parte superior do arquivo) e os métodos têm [atributos DataObjectMethodAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). O atributo `DataObject` marca a classe como sendo um objeto adequado para associação a um [controle ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), enquanto o `DataObjectMethodAttribute` indica a finalidade do método. Como veremos em Tutoriais futuros, a ObjectDataSource do ASP.NET 2.0 facilita o acesso de dados de uma classe de forma declarativa. Para ajudar a filtrar a lista de classes possíveis a serem associadas no assistente do ObjectDataSource, por padrão, somente as classes marcadas como `DataObjects` são mostradas na lista suspensa do assistente. A classe `ProductsBLL` funcionará tão bem sem esses atributos, mas adicioná-los torna mais fácil trabalhar com o assistente do ObjectDataSource.

## <a name="adding-the-other-classes"></a>Adicionando as outras classes

Com a classe `ProductsBLL` concluída, ainda precisamos adicionar as classes para trabalhar com categorias, fornecedores e funcionários. Reserve um momento para criar as seguintes classes e métodos usando os conceitos do exemplo acima:

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

O método que vale a pena observar é o método `UpdateSupplierAddress` da classe `SuppliersBLL`. Esse método fornece uma interface para atualizar apenas as informações de endereço do fornecedor. Internamente, esse método lê o objeto `SupplierDataRow` para o `supplierID` especificado (usando `GetSupplierBySupplierID`), define suas propriedades relacionadas a endereços e, em seguida, chama o método `Update` do `SupplierDataTable`. O método `UpdateSupplierAddress` segue:

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample2.cs)]

Consulte o download deste artigo para minha implementação completa das classes de BLL.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Etapa 2: acessando os conjuntos de valores tipados por meio das classes de BLL

No primeiro tutorial, vimos exemplos de como trabalhar diretamente com o DataSet tipado por meio de programação, mas com a adição de nossas classes de BLL, a camada de apresentação deve funcionar em vez da BLL. No exemplo de `AllProducts.aspx` do primeiro tutorial, o `ProductsTableAdapter` foi usado para associar a lista de produtos a um GridView, conforme mostrado no código a seguir:

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample3.cs)]

Para usar as novas classes BLL, tudo o que precisa ser alterado é que a primeira linha de código simplesmente substitui o objeto `ProductsTableAdapter` por um objeto `ProductBLL`:

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample4.cs)]

As classes de BLL também podem ser acessadas de forma declarativa (como pode ser o DataSet tipado) usando o ObjectDataSource. Abordaremos o ObjectDataSource mais detalhadamente nos tutoriais a seguir.

[![a lista de produtos é exibida em um GridView](creating-a-business-logic-layer-cs/_static/image4.png)](creating-a-business-logic-layer-cs/_static/image3.png)

**Figura 3**: a lista de produtos é exibida em um GridView ([clique para exibir a imagem em tamanho normal](creating-a-business-logic-layer-cs/_static/image5.png))

## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Etapa 3: Adicionar validação em nível de campo às classes de DataRow

A validação em nível de campo são verificações que pertencem aos valores de propriedade dos objetos comerciais ao inserir ou atualizar. Algumas regras de validação no nível de campo para produtos incluem:

- O campo de `ProductName` deve ter de 40 caracteres ou menos de comprimento
- O campo de `QuantityPerUnit` deve ter 20 caracteres ou menos de comprimento
- Os campos `ProductID`, `ProductName`e `Discontinued` são necessários, mas todos os outros campos são opcionais
- Os campos `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`e `ReorderLevel` devem ser maiores ou iguais a zero

Essas regras podem e devem ser expressas no nível do banco de dados. O limite de caracteres nos campos `ProductName` e `QuantityPerUnit` são capturados pelos tipos de dados dessas colunas na tabela `Products` (`nvarchar(40)` e `nvarchar(20)`, respectivamente). Se os campos são obrigatórios e opcionais são expressos pelo se a coluna da tabela de banco de dados permite `NULL` s. Existem quatro [restrições check](https://msdn.microsoft.com/library/ms188258.aspx) que garantem que apenas valores maiores ou iguais a zero possam torná-lo nas colunas `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`ou `ReorderLevel`.

Além de impor essas regras no banco de dados, elas também devem ser impostas no nível do DataSet. Na verdade, o tamanho do campo e se um valor é necessário ou opcional já está capturado para cada conjunto de colunas de DataTable. Para ver a validação no nível de campo existente fornecida automaticamente, vá para o designer de conjunto de os, selecione um campo de uma das tabelas de tabela e, em seguida, vá para a janela Propriedades. Como mostra a Figura 4, o `QuantityPerUnit` DataColumn no `ProductsDataTable` tem um comprimento máximo de 20 caracteres e permite valores de `NULL`. Se tentarmos definir a propriedade `QuantityPerUnit` do `ProductsDataRow`como um valor de cadeia de caracteres com mais de 20 caracteres, uma `ArgumentException` será lançada.

[![a DataColumn fornece validação básica em nível de campo](creating-a-business-logic-layer-cs/_static/image7.png)](creating-a-business-logic-layer-cs/_static/image6.png)

**Figura 4**: a DataColumn fornece validação básica em nível de campo ([clique para exibir a imagem em tamanho normal](creating-a-business-logic-layer-cs/_static/image8.png))

Infelizmente, não podemos especificar verificações de limites, como o valor de `UnitPrice` deve ser maior ou igual a zero, por meio do janela Propriedades. Para fornecer esse tipo de validação em nível de campo, precisamos criar um manipulador de eventos para o evento [ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) da DataTable. Conforme mencionado no [tutorial anterior](creating-a-data-access-layer-cs.md), o conjunto de tabelas, os conjuntos de tabela e os objetos DataRow criados pelo dataset tipado podem ser estendidos por meio do uso de classes parciais. Usando essa técnica, podemos criar um manipulador de eventos `ColumnChanging` para a classe `ProductsDataTable`. Comece criando uma classe na pasta `App_Code` chamada `ProductsDataTable.ColumnChanging.cs`.

[![adicionar uma nova classe à pasta App_Code](creating-a-business-logic-layer-cs/_static/image10.png)](creating-a-business-logic-layer-cs/_static/image9.png)

**Figura 5**: adicionar uma nova classe à pasta `App_Code` ([clique para exibir a imagem em tamanho normal](creating-a-business-logic-layer-cs/_static/image11.png))

Em seguida, crie um manipulador de eventos para o evento `ColumnChanging` que garante que os valores de coluna `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`e `ReorderLevel` (se não `NULL`) sejam maiores ou iguais a zero. Se qualquer coluna desse tipo estiver fora do intervalo, lance um `ArgumentException`.

ProductsDataTable.ColumnChanging.cs

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample5.cs)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Etapa 4: adicionar regras de negócio personalizadas às classes da BLL

Além da validação em nível de campo, pode haver regras de negócios personalizadas de alto nível que envolvem diferentes entidades ou conceitos que não podem ser expressos no nível de coluna única, como:

- Se um produto for descontinuado, seu `UnitPrice` não poderá ser atualizado
- O país de residência de um funcionário deve ser igual ao país de residência de seu gerente
- Um produto não poderá ser descontinuado se for o único produto fornecido pelo fornecedor

As classes de BLL devem conter verificações para garantir a adesão às regras de negócio do aplicativo. Essas verificações podem ser adicionadas diretamente aos métodos aos quais se aplicam.

Imagine que nossas regras de negócios ditem que um produto não pode ser marcado como descontinuado se fosse o único produto de um determinado fornecedor. Ou seja, se o produto *X* fosse o único produto que adquirimos do fornecedor *Y*, não pudemos marcar *X* como descontinuado; no entanto, se *o*fornecedor *Y* nos forneceu três produtos, A, *B*e *C*, poderíamos marcar qualquer um deles como descontinuado. Uma regra de negócios estranha, mas regras de negócios e senso comum nem sempre estão alinhadas!

Para impor essa regra de negócio no método `UpdateProducts`, começamos verificando se `Discontinued` foi definido como `true` e, em caso afirmativo, chamamos `GetProductsBySupplierID` para determinar quantos produtos adquirimos do fornecedor do produto. Se apenas um produto for adquirido deste fornecedor, lançaremos um `ApplicationException`.

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample6.cs)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Respondendo a erros de validação na camada de apresentação

Ao chamar a BLL da camada de apresentação, podemos decidir se deve tentar lidar com as exceções que podem ser geradas ou deixá-las emergir em ASP.NET (o que irá gerar o evento de `Error` do `HttpApplication`). Para tratar uma exceção ao trabalhar com a BLL programaticamente, podemos usar uma [tentativa... Catch](https://msdn.microsoft.com/library/0yd65esw.aspx) , como mostra o exemplo a seguir:

[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample7.cs)]

Como veremos em Tutoriais futuros, lidar com exceções que se emergim da BLL ao usar um controle da Web de dados para inserção, atualização ou exclusão de dados podem ser tratados diretamente em um manipulador de eventos em vez de ter que encapsular o código em blocos de `try...catch`.

## <a name="summary"></a>Resumo

Um aplicativo bem projetado é criado em camadas distintas, cada um encapsulando uma função específica. No primeiro tutorial desta série de artigos, criamos uma camada de acesso a dados usando datasets tipados; Neste tutorial, criamos uma camada de lógica de negócios como uma série de classes na pasta `App_Code` do nosso aplicativo que se chamam de nossa DAL. A BLL implementa a lógica de nível de campo e de negócios para nosso aplicativo. Além de criar uma BLL separada, como fizemos neste tutorial, outra opção é estender os métodos TableAdapters por meio do uso de classes parciais. No entanto, o uso dessa técnica não nos permite substituir os métodos existentes, nem separar nossa DAL e nossa BLL como a abordagem que fizemos neste artigo.

Com a DAL e a BLL concluídas, estamos prontos para começar em nossa camada de apresentação. No [próximo tutorial](master-pages-and-site-navigation-cs.md) , vamos fazer um breve desvio dos tópicos de acesso a dados e definir um layout de página consistente para uso em todos os tutoriais.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Liz Shulok, Dennis Patterson, Carlos Santos e Hilton Giesenow. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-a-data-access-layer-cs.md)
> [Próximo](master-pages-and-site-navigation-cs.md)
