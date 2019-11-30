---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: Atualização do lote (VB) | Microsoft Docs
author: rick-anderson
description: Saiba como atualizar vários registros de banco de dados em uma única operação. Na camada da interface do usuário, criamos um GridView em que cada linha é editável. Nos dados...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: f0bb83b17585876dd6d28a5893a223cce15da31d
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74591073"
---
# <a name="batch-updating-vb"></a>Atualização em lote (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip) ou [baixar PDF](batch-updating-vb/_static/datatutorial64vb1.pdf)

> Saiba como atualizar vários registros de banco de dados em uma única operação. Na camada da interface do usuário, criamos um GridView em que cada linha é editável. Na camada de acesso a dados, Encapsulamos as várias operações de atualização em uma transação para garantir que todas as atualizações tenham sucesso ou todas as atualizações sejam revertidas.

## <a name="introduction"></a>Introdução

No [tutorial anterior](wrapping-database-modifications-within-a-transaction-vb.md) , vimos como estender a camada de acesso a dados para adicionar suporte para transações de banco de dado. As transações de banco de dados garantem que uma série de instruções de modificação de dado será tratada como uma operação atômica, o que garante que todas as modificações falhem ou todas tenham êxito. Com essa funcionalidade de nível inferior da DAL, estamos prontos para transformar nossa atenção em criar interfaces de modificação de dados em lotes.

Neste tutorial, criaremos um GridView em que cada linha é editável (veja a Figura 1). Como cada linha é renderizada em sua interface de edição, não há necessidade de uma coluna de botões editar, atualizar e cancelar. Em vez disso, há dois botões de produtos de atualização na página que, quando clicados, enumeram as linhas GridView e atualizam o banco de dados.

[![cada linha em GridView é editável](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**Figura 1**: cada linha no GridView é editável ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image2.png))

Vamos começar!

> [!NOTE]
> No tutorial [executando atualizações em lote](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) , criamos uma interface de edição em lote usando o controle DataList. Este tutorial é diferente do anterior no que usa um GridView e a atualização do lote é executada dentro do escopo de uma transação. Depois de concluir este tutorial, recomendo que você retorne ao tutorial anterior e atualize-o para usar a funcionalidade relacionada à transação do banco de dados adicionada no tutorial anterior.

## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Examinando as etapas para tornar todas as linhas GridView editáveis

Conforme discutido em [uma visão geral do tutorial inserir, atualizar e excluir dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) , o GridView oferece suporte interno para a edição de seus dados subjacentes em uma base por linha. Internamente, o GridView observa qual linha é editável por meio de sua [propriedade`EditIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Como o GridView está sendo associado à sua fonte de dados, ele verifica cada linha para ver se o índice da linha é igual ao valor de `EditIndex`. Nesse caso, os campos da linha s são renderizados usando suas interfaces de edição. Para BoundFields, a interface de edição é uma caixa de texto cuja propriedade `Text` é atribuída ao valor do campo de dados especificado pela propriedade `DataField` BoundField s. Para TemplateFields, o `EditItemTemplate` é usado no lugar do `ItemTemplate`.

Lembre-se de que o fluxo de trabalho de edição é iniciado quando um usuário clica em um botão de edição de linha. Isso causa um postback, define a Propriedade GridView s `EditIndex` como o índice s de linha clicado e reassocia os dados à grade. Quando um botão de cancelamento de uma linha é clicado, no postback, o `EditIndex` é definido com um valor de `-1` antes de reassociar os dados à grade. Como as linhas de GridView começam a indexação em zero, definir `EditIndex` como `-1` tem o efeito de exibir o GridView no modo somente leitura.

A propriedade `EditIndex` funciona bem para a edição por linha, mas não é projetada para edição em lotes. Para tornar todo o GridView editável, precisamos que cada linha seja renderizada usando sua interface de edição. A maneira mais fácil de fazer isso é criar onde cada campo editável é implementado como um TemplateField com sua interface de edição definida no `ItemTemplate`.

Nas próximas várias etapas, criaremos um GridView totalmente editável. Na etapa 1, vamos começar criando o GridView e seu ObjectDataSource e converter seus BoundFields e CheckBoxField em TemplateFields. Nas etapas 2 e 3, moveremos as interfaces de edição do TemplateFields `EditItemTemplate` s para suas `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Etapa 1: exibindo informações do produto

Antes de nos preocuparmos em criar um GridView em que as linhas são editáveis, vamos começar simplesmente exibindo as informações do produto. Abra a página `BatchUpdate.aspx` na pasta `BatchData` e arraste um GridView da caixa de ferramentas para o designer. Defina o `ID` s de GridView como `ProductsGrid` e, em sua marca inteligente, escolha associá-lo a um novo ObjectDataSource chamado `ProductsDataSource`. Configure o ObjectDataSource para recuperar seus dados do método de `GetProducts` da classe `ProductsBLL`.

[![configurar o ObjectDataSource para usar a classe ProductsBLL](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**Figura 2**: configurar o ObjectDataSource para usar a classe `ProductsBLL` ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image4.png))

[![recuperar os dados do produto usando o método GetProducts](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**Figura 3**: recuperar os dados do produto usando o método `GetProducts` ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image6.png))

Como o GridView, os recursos de modificação do ObjectDataSource s são projetados para funcionar por linha. Para atualizar um conjunto de registros, precisaremos escrever um pouco de código na classe code-behind da página ASP.NET, que coloca os dados em lotes e os transmite para a BLL. Portanto, defina as listas suspensas nas guias ObjectDataSource s UPDATE, INSERT e DELETE como (None). Clique em Concluir para concluir o assistente.

[![definir as listas suspensas nas guias atualizar, inserir e excluir para (nenhum)](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**Figura 4**: definir as listas suspensas nas guias atualizar, inserir e excluir para (nenhum) ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image8.png))

Depois de concluir o assistente para configurar fonte de dados, a marcação declarativa de ObjectDataSource s deve ser parecida com a seguinte:

[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

A conclusão do assistente para configurar fonte de dados também faz com que o Visual Studio crie BoundFields e um CheckBoxField para os campos de dados do produto no GridView. Para este tutorial, vamos permitir que apenas o usuário exiba e edite o nome, a categoria, o preço e o status descontinuados do produto. Remova todos os campos, exceto os `ProductName`, `CategoryName`, `UnitPrice`e `Discontinued` e renomeie as propriedades `HeaderText` dos três primeiros campos para produto, categoria e preço, respectivamente. Por fim, marque as caixas de seleção habilitar paginação e Habilitar classificação na marca inteligente s do GridView.

Neste ponto, o GridView tem três BoundFields (`ProductName`, `CategoryName`e `UnitPrice`) e um CheckBoxField (`Discontinued`). Precisamos converter esses quatro campos em TemplateFields e, em seguida, mover a interface de edição do `EditItemTemplate` de modelos s para sua `ItemTemplate`.

> [!NOTE]
> Exploramos a criação e a personalização do TemplateFields no tutorial [Personalizando a interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) . Vamos percorrer as etapas de conversão de BoundFields e CheckBoxField em TemplateFields e definir suas interfaces de edição em seus `ItemTemplate` s, mas se você ficar preso ou precisar de um atualizador, Don t hesitará em consultar este tutorial anterior.

Na marca inteligente de GridView, clique no link Editar colunas para abrir a caixa de diálogo campos. Em seguida, selecione cada campo e clique no link converter este campo em um modelo.

![Converter os BoundFields e CheckBoxField existentes em TemplateFields](batch-updating-vb/_static/image5.gif)

**Figura 5**: converter os boundfields e CheckBoxField existentes em TemplateFields

Agora que cada campo é um TemplateField, estamos prontos para mover a interface de edição do `EditItemTemplate` s para o `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Etapa 2: criando as interfaces de edição`ProductName`,`UnitPrice`e`Discontinued`

A criação das interfaces de edição `ProductName`, `UnitPrice`e `Discontinued` é o tópico desta etapa e é bem simples, pois cada interface já está definida no `EditItemTemplate`de modelos. Criar a interface de edição de `CategoryName` é um pouco mais envolvido, já que precisamos criar uma DropDownList das categorias aplicáveis. Esse `CategoryName` interface de edição é resolvido na etapa 3.

Deixe que s comecem com o `ProductName` TemplateField. Clique no link editar modelos na marca inteligente GridView s e faça uma busca detalhada até a `ProductName` TemplateField s `EditItemTemplate`. Selecione a caixa de texto, copie-a para a área de transferência e cole-a no `ProductName` TemplateField s `ItemTemplate`. Altere a Propriedade TextBox s `ID` para `ProductName`.

Em seguida, adicione um RequiredFieldValidator ao `ItemTemplate` para garantir que o usuário forneça um valor para cada nome de produto. Defina a propriedade `ControlToValidate` como ProductName, a propriedade `ErrorMessage` para você deve fornecer o nome do produto. e a propriedade `Text` como \*. Depois de fazer essas adições ao `ItemTemplate`, sua tela deve ser semelhante à figura 6.

[![o TemplateField do ProductName agora inclui uma caixa de texto e um RequiredFieldValidator](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**Figura 6**: o `ProductName` TemplateField agora inclui uma caixa de texto e um RequiredFieldValidator ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image10.png))

Para a interface de edição de `UnitPrice`, comece copiando a caixa de texto da `EditItemTemplate` para a `ItemTemplate`. Em seguida, coloque um $ na frente da caixa de texto e defina sua propriedade `ID` como UnitPrice e sua propriedade `Columns` como 8.

Além disso, adicione um CompareValidator à `UnitPrice` s `ItemTemplate` para garantir que o valor inserido pelo usuário seja um valor de moeda válido maior ou igual a $0. Defina a propriedade `ControlToValidate` do validador como PreçoUnitário, sua propriedade `ErrorMessage` para você deve inserir um valor de moeda válido. Omita todos os símbolos de moeda., sua propriedade `Text` para \*, sua propriedade `Type` como `Currency`, sua propriedade `Operator` como `GreaterThanEqual`e sua propriedade `ValueToCompare` como 0.

[![adicionar um CompareValidator para garantir que o preço inserido é um valor de moeda não negativo](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**Figura 7**: adicionar um CompareValidator para garantir que o preço inserido é um valor de moeda não negativo ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image12.png))

Para o `Discontinued` TemplateField, você pode usar a caixa de seleção já definida no `ItemTemplate`. Basta definir seu `ID` como descontinuado e sua propriedade `Enabled` como `True`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Etapa 3: criando a interface de edição de`CategoryName`

A interface de edição no `CategoryName` TemplateField s `EditItemTemplate` contém uma caixa de texto que exibe o valor do campo de dados `CategoryName`. Precisamos substituir isso por uma DropDownList que liste as categorias possíveis.

> [!NOTE]
> O tutorial [Personalizando a interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) contém uma discussão mais completa e completa sobre como personalizar um modelo para incluir uma DropDownList em oposição a uma caixa de texto. Embora as etapas aqui sejam concluídas, elas são apresentadas tersely. Para obter uma visão mais detalhada sobre como criar e configurar as categorias DropDownList, consulte o tutorial [Personalizando a interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) .

Arraste uma DropDownList da caixa de ferramentas até a `CategoryName` TemplateField s `ItemTemplate`, definindo sua `ID` como `Categories`. Neste ponto, normalmente definimos a fonte de dados DropDownLists s por meio de sua marca inteligente, criando um novo ObjectDataSource. No entanto, isso adicionará o ObjectDataSource no `ItemTemplate`, o que resultará em uma instância de ObjectDataSource criada para cada linha GridView. Em vez disso, vamos criar o ObjectDataSource fora do TemplateFields GridView. Finalize a edição do modelo e arraste um ObjectDataSource da caixa de ferramentas para o designer sob o `ProductsDataSource` ObjectDataSource. Nomeie o novo ObjectDataSource `CategoriesDataSource` e configure-o para usar o método `CategoriesBLL` Class s `GetCategories`.

[![configurar o ObjectDataSource para usar a classe CategoriesBLL](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**Figura 8**: configurar o ObjectDataSource para usar a classe `CategoriesBLL` ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image14.png))

[![recuperar os dados da categoria usando o método GetCategories](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**Figura 9**: recuperar os dados da categoria usando o método `GetCategories` ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image16.png))

Como esse ObjectDataSource é usado apenas para recuperar dados, defina as listas suspensas nas guias atualizar e excluir como (nenhum). Clique em Concluir para concluir o assistente.

[![definir as listas suspensas nas guias atualizar e excluir para (nenhum)](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**Figura 10**: definir as listas suspensas nas guias atualizar e excluir para (nenhum) ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image18.png))

Depois de concluir o assistente, a marcação declarativa de `CategoriesDataSource` s deve ser parecida com a seguinte:

[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

Com a `CategoriesDataSource` criada e configurada, retorne ao `CategoryName` TemplateField s `ItemTemplate` e, na marca inteligente DropDownList s, clique no link escolher fonte de dados. No assistente de configuração da fonte de dados, selecione a opção `CategoriesDataSource` na primeira lista suspensa e escolha `CategoryName` usada para a exibição e `CategoryID` como o valor.

[![associar a DropDownList ao CategoriesDataSource](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**Figura 11**: associar o DropDownList ao `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image20.png))

Neste ponto, o `Categories` DropDownList lista todas as categorias, mas ela ainda não seleciona automaticamente a categoria apropriada para o produto associado à linha GridView. Para fazer isso, precisamos definir o `Categories` DropDownList s `SelectedValue` ao valor de `CategoryID` do produto. Clique no link editar DataBindings da marca inteligente DropDownList s e associe a propriedade `SelectedValue` com o campo de dados `CategoryID`, como mostrado na Figura 12.

![Associar o valor de CategoryID do produto à propriedade SelectedValue da DropDownList](batch-updating-vb/_static/image12.gif)

**Figura 12**: associar o valor de `CategoryID` do produto à Propriedade DropDownList s `SelectedValue`

Um último problema permanece: se o produto não tiver um valor `CategoryID` especificado, a instrução DataBinding em `SelectedValue` resultará em uma exceção. Isso ocorre porque a DropDownList contém apenas itens para as categorias e não oferece uma opção para os produtos que têm um valor de banco de dados `NULL` para `CategoryID`. Para corrigir isso, defina a Propriedade DropDownList s `AppendDataBoundItems` como `True` e adicione um novo item à DropDownList, omitindo a propriedade `Value` da sintaxe declarativa. Ou seja, certifique-se de que a sintaxe declarativa `Categories` DropDownList s seja parecida com a seguinte:

[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

Observe como o `<asp:ListItem Value="">`--Select One--tem seu atributo `Value` definido explicitamente como uma cadeia de caracteres vazia. Consulte o tutorial [Personalizando a interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) para obter uma discussão mais completa sobre por que esse item DropDownList adicional é necessário para lidar com o `NULL` caso e por que a atribuição da propriedade `Value` a uma cadeia de caracteres vazia é essencial.

> [!NOTE]
> Há um problema potencial de desempenho e escalabilidade aqui que vale a pena mencionar. Como cada linha tem uma DropDownList que usa o `CategoriesDataSource` como sua fonte de dados, a classe `CategoriesBLL` s `GetCategories` método será chamada *n* vezes por página de visita, em que *n* é o número de linhas no GridView. Essas *n* chamadas para `GetCategories` resultam em consultas *n* para o banco de dados. Esse impacto no banco de dados pode ser diminuído armazenando em cache as categorias retornadas em um cache por solicitação ou por meio da camada de cache usando uma dependência de cache do SQL ou uma expiração muito curta baseada em tempo. Para obter mais informações sobre a opção de cache por solicitação, consulte [`HttpContext.Items` um repositório de cache por solicitação](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).

## <a name="step-4-completing-the-editing-interface"></a>Etapa 4: concluindo a interface de edição

Fizemos várias alterações nos modelos GridView s sem pausar para exibir nosso progresso. Reserve um tempo para exibir nosso progresso por meio de um navegador. Como a Figura 13 mostra, cada linha é renderizada usando seu `ItemTemplate`, que contém a interface de edição da célula.

[![cada linha GridView é editável](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**Figura 13**: cada linha GridView é editável ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image22.png))

Há alguns problemas de formatação menores que devemos cuidar deste ponto. Primeiro, observe que o valor de `UnitPrice` contém quatro pontos decimais. Para corrigir isso, retorne ao `UnitPrice` TemplateField s `ItemTemplate` e, na marca inteligente s da caixa de texto, clique no link editar DataBindings. Em seguida, especifique que a propriedade `Text` deve ser formatada como um número.

![Formatar a propriedade de texto como um número](batch-updating-vb/_static/image14.gif)

**Figura 14**: Formatar a propriedade `Text` como um número

Em segundo lugar, deixe o s centralizar a caixa de seleção na coluna `Discontinued` (em vez de tê-la alinhada à esquerda). Clique em Editar colunas na marca inteligente s de GridView e selecione o `Discontinued` TemplateField na lista de campos no canto inferior esquerdo. Faça uma busca detalhada em `ItemStyle` e defina a propriedade `HorizontalAlign` como centralizar, conforme mostrado na Figura 15.

![Centralizar a caixa de seleção descontinuada](batch-updating-vb/_static/image15.gif)

**Figura 15**: centralizar a caixa de seleção de `Discontinued`

Em seguida, adicione um controle ValidationSummary à página e defina sua propriedade `ShowMessageBox` como `True` e sua propriedade `ShowSummary` como `False`. Além disso, adicione o botão controles da Web que, quando clicado, atualizará as alterações do usuário. Especificamente, adicione dois controles de botão da Web, um acima do GridView e um abaixo dele, definindo ambos os controles `Text` Propriedades para atualizar produtos.

Como a interface de edição de GridView está definida em seu TemplateFields `ItemTemplate` s, as `EditItemTemplate` s são supérfluas e podem ser excluídas.

Depois de fazer as alterações de formatação mencionadas acima, adicionar os controles de botão e remover as `EditItemTemplate` s desnecessárias, a sintaxe declarativa de s de página deverá ser parecida com a seguinte:

[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

A Figura 16 mostra essa página quando exibida por meio de um navegador depois que os controles da Web do botão são adicionados e as alterações de formatação feitas.

[![a página agora inclui dois botões atualizar produtos](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**Figura 16**: a página agora inclui dois botões de produtos de atualização ([clique para exibir a imagem em tamanho normal](batch-updating-vb/_static/image24.png))

## <a name="step-5-updating-the-products"></a>Etapa 5: atualizando os produtos

Quando um usuário visitar essa página, ele fará suas modificações e, em seguida, clicará em um dos dois botões atualizar produtos. Nesse ponto, precisamos, de alguma forma, salvar os valores inseridos pelo usuário para cada linha em uma `ProductsDataTable` instância e, em seguida, passá-lo para um método BLL que passará essa instância de `ProductsDataTable` para o método da DAL s `UpdateWithTransaction`. O método `UpdateWithTransaction`, que criamos no [tutorial anterior](wrapping-database-modifications-within-a-transaction-vb.md), garante que o lote de alterações será atualizado como uma operação atômica.

Crie um método chamado `BatchUpdate` em `BatchUpdate.aspx.vb` e adicione o seguinte código:

[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

Esse método começa obtendo todos os produtos em um `ProductsDataTable` por meio de uma chamada para o método de `GetProducts` de BLL. Em seguida, ele enumera a coleção `ProductGrid` GridView s [`Rows`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). A coleção de `Rows` contém uma [instância de`GridViewRow`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) para cada linha exibida no GridView. Como estamos mostrando no máximo dez linhas por página, a coleção GridView s `Rows` não terá mais de dez itens.

Para cada linha, a `ProductID` é capturada da coleção de `DataKeys` e a `ProductsRow` apropriada é selecionada no `ProductsDataTable`. Os quatro controles de entrada TemplateField são referenciados por meio de programação e seus valores atribuídos às propriedades da instância do `ProductsRow` s. Depois de cada linha de GridView, os valores de s foram usados para atualizar o `ProductsDataTable`, os s passados para o método de `UpdateWithTransaction` de BLL que, como vimos no tutorial anterior, simplesmente chamam o método de `UpdateWithTransaction` DAL.

O algoritmo de atualização do lote usado para este tutorial atualiza cada linha no `ProductsDataTable` que corresponde a uma linha no GridView, independentemente de as informações do produto terem sido alteradas. Embora essas atualizações cegas não sejam geralmente um problema de desempenho, elas podem levar a registros supérfluos se você reauditar as alterações na tabela do banco de dados. De volta ao tutorial [executando atualizações em lote](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) , exploramos uma interface de atualização do lote com o DataList e adicionei o código que só atualizaria os registros que foram realmente modificados pelo usuário. Sinta-se à vontade para usar as técnicas da [execução de atualizações em lote](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) para atualizar o código neste tutorial, se desejado.

> [!NOTE]
> Ao associar a fonte de dados ao GridView por meio de sua marca inteligente, o Visual Studio atribui automaticamente os valores de chave primária da fonte de dados à Propriedade GridView s `DataKeyNames`. Se você não tiver associado o ObjectDataSource ao GridView por meio da marca inteligente GridView s, conforme descrito na etapa 1, você precisará definir manualmente a Propriedade GridView `DataKeyNames` como ProductID para acessar o valor de `ProductID` para cada linha por meio da coleção de `DataKeys`.

O código usado em `BatchUpdate` é semelhante ao usado nos métodos de `UpdateProduct` de BLL. a principal diferença é que, nos métodos de `UpdateProduct`, apenas uma única instância de `ProductRow` é recuperada da arquitetura. O código que atribui as propriedades do `ProductRow` é o mesmo entre os métodos `UpdateProducts` e o código dentro do loop `For Each` no `BatchUpdate`, assim como o padrão geral.

Para concluir este tutorial, precisamos ter o método `BatchUpdate` invocado quando um dos botões atualizar produtos for clicado. Crie manipuladores de eventos para os eventos de `Click` desses dois controles de botão e adicione o seguinte código nos manipuladores de eventos:

[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

Primeiro, é feita uma chamada para `BatchUpdate`. Em seguida, a [propriedade`ClientScript`](https://msdn.microsoft.com/library/system.web.ui.page.clientscript(VS.80).aspx) é usada para injetar JavaScript que exibirá uma MessageBox que leia os produtos foram atualizados.

Reserve um minuto para testar esse código. Visite `BatchUpdate.aspx` por meio de um navegador, edite um número de linhas e clique em um dos botões atualizar produtos. Supondo que não haja erros de validação de entrada, você deverá ver uma MessageBox que leia os produtos foram atualizados. Para verificar a atomicidade da atualização, considere adicionar uma restrição de `CHECK` aleatória, como uma que despermita `UnitPrice` valores de 1234,56. Em seguida, em `BatchUpdate.aspx`, edite um número de registros, certificando-se de definir um valor de `UnitPrice` do produto para o valor proibido (1234,56). Isso deve resultar em um erro ao clicar em atualizar produtos com as outras alterações durante essa operação de lote revertidas para seus valores originais.

## <a name="an-alternativebatchupdatemethod"></a>Um método de`BatchUpdate`alternativo

O método `BatchUpdate` que acabamos de examinar recupera *todos* os produtos do método de `GetProducts` BLL s e, em seguida, atualiza apenas os registros que aparecem no GridView. Essa abordagem é ideal se o GridView não usar paginação, mas, se isso ocorrer, pode haver centenas, milhares ou dezenas de milhares de produtos, mas apenas dez linhas no GridView. Nesse caso, obter todos os produtos do banco de dados apenas para modificar 10 deles é menor do que o ideal.

Para esses tipos de situações, considere o uso do seguinte método `BatchUpdateAlternate` em vez disso:

[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate` começa criando um novo `ProductsDataTable` vazio chamado `products`. Em seguida, ele percorre a coleção GridView s `Rows` e, para cada linha, obtém as informações específicas do produto usando o método de `GetProductByProductID(productID)` de BLL. A instância do `ProductsRow` recuperada tem suas propriedades atualizadas da mesma maneira que `BatchUpdate`, mas depois de atualizar a linha, ela é importada para a `products` `ProductsDataTable` por meio do método DataTable s [`ImportRow(DataRow)`](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Após a conclusão do loop de `For Each`, `products` contém uma instância `ProductsRow` para cada linha no GridView. Uma vez que cada uma das instâncias de `ProductsRow` foram adicionadas ao `products` (em vez de atualizadas), se forem repassadas para o método `UpdateWithTransaction`, a `ProductsTableAdapter` tentará inserir cada um dos registros no banco de dados. Em vez disso, precisamos especificar que cada uma dessas linhas foi modificada (não adicionada).

Isso pode ser feito adicionando um novo método à BLL chamado `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, mostrado abaixo, define o `RowState` de cada uma das instâncias de `ProductsRow` no `ProductsDataTable` para `Modified` e, em seguida, passa o `ProductsDataTable` para o método da DAL s `UpdateWithTransaction`.

[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>Resumo

O GridView fornece recursos internos de edição por linha, mas não tem suporte para a criação de interfaces totalmente editáveis. Como vimos neste tutorial, essas interfaces são possíveis, mas exigem um pouco de trabalho. Para criar um GridView em que cada linha é editável, precisamos converter os campos de GridView em TemplateFields e definir a interface de edição dentro do `ItemTemplate` s. Além disso, os controles da Web do botão atualizar todos os tipos devem ser adicionados à página, separados do GridView. Esses botões `Click` manipuladores de eventos precisam enumerar a coleção GridView s `Rows`, armazenar as alterações em uma `ProductsDataTable`e passar as informações atualizadas para o método BLL apropriado.

No próximo tutorial, veremos como criar uma interface para exclusão em lote. Em particular, cada linha GridView incluirá uma caixa de seleção e, em vez de atualizar os botões de todos os tipos, os botões excluir linhas selecionadas serão excluídos.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Teresa Murphy e David Suru. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](wrapping-database-modifications-within-a-transaction-vb.md)
> [Próximo](batch-deleting-vb.md)
