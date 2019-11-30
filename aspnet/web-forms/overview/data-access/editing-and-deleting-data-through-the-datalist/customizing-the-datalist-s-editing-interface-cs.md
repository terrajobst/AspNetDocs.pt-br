---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
title: Personalizando a interface de edição do DataListC#() | Microsoft Docs
author: rick-anderson
description: Neste tutorial, criaremos uma interface de edição mais rica para o DataList, um que inclui DropDownLists e uma caixa de seleção.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: a5d13067-ddfb-4c36-8209-0f69fd40e45c
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 81f5a7f6737f544f577447f263dbd37dbc8279d9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74623680"
---
# <a name="customizing-the-datalists-editing-interface-c"></a>Personalizar a interface de edição do DataList (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_CS.exe) ou [baixar PDF](customizing-the-datalist-s-editing-interface-cs/_static/datatutorial40cs1.pdf)

> Neste tutorial, criaremos uma interface de edição mais rica para o DataList, um que inclui DropDownLists e uma caixa de seleção.

## <a name="introduction"></a>Introdução

A marcação e os controles da Web no `EditItemTemplate` DataList s definem sua interface editável. Em todos os exemplos de DataList editáveis que examinamos até agora, a interface editável foi composta por controles da Web TextBox. No [tutorial anterior](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md) , melhoramos a experiência do usuário em tempo de edição, adicionando controles de validação.

O `EditItemTemplate` pode ser ainda mais expandido para incluir controles da Web diferentes da caixa de texto, como DropDownLists, adradiobuttonlists, calendários e assim por diante. Assim como nas caixas de entrada, ao personalizar a interface de edição para incluir outros controles da Web, empregue as seguintes etapas:

1. Adicione o controle da Web ao `EditItemTemplate`.
2. Use a sintaxe de DataBinding para atribuir o valor do campo de dados correspondente à propriedade apropriada.
3. No manipulador de eventos `UpdateCommand`, acesse o valor de controle da Web programaticamente e passe-o para o método BLL apropriado.

Neste tutorial, criaremos uma interface de edição mais rica para o DataList, um que inclui DropDownLists e uma caixa de seleção. Em particular, criaremos um DataList que lista informações sobre produtos e permite que o nome, o fornecedor, a categoria e o status descontinuados do produto sejam atualizados (veja a Figura 1).

[![a interface de edição inclui uma caixa de texto, duas DropDownLists e uma caixa de seleção](customizing-the-datalist-s-editing-interface-cs/_static/image2.png)](customizing-the-datalist-s-editing-interface-cs/_static/image1.png)

**Figura 1**: a interface de edição inclui uma caixa de texto, duas DropDownLists e uma caixa de seleção ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image3.png))

## <a name="step-1-displaying-product-information"></a>Etapa 1: exibindo informações do produto

Antes que possamos criar a interface DataList s editable, primeiro precisamos criar a interface somente leitura. Comece abrindo a página de `CustomizedUI.aspx` da pasta `EditDeleteDataList` e, no designer, adicione uma DataList à página, definindo sua propriedade `ID` como `Products`. Na marca inteligente DataList s, crie um novo ObjectDataSource. Nomeie esse novo ObjectDataSource `ProductsDataSource` e configure-o para recuperar dados do método `ProductsBLL` Class s `GetProducts`. Assim como nos tutoriais de DataList editáveis anteriores, atualizaremos as informações editadas sobre o produto indo diretamente para a camada de lógica de negócios. De acordo, defina as listas suspensas nas guias atualizar, inserir e excluir como (nenhum).

[![definir as listas suspensas atualizar, inserir e excluir guias como (nenhum)](customizing-the-datalist-s-editing-interface-cs/_static/image5.png)](customizing-the-datalist-s-editing-interface-cs/_static/image4.png)

**Figura 2**: definir as listas suspensas de atualizar, inserir e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image6.png))

Depois de configurar o ObjectDataSource, o Visual Studio criará um `ItemTemplate` padrão para o DataList que lista o nome e o valor de cada um dos campos de dados retornados. Modifique o `ItemTemplate` para que o modelo liste o nome do produto em um elemento `<h4>` junto com o nome da categoria, nome do fornecedor, preço e status descontinuado. Além disso, adicione um botão Editar, certificando-se de que sua propriedade `CommandName` esteja definida como editar. A marcação declarativa para o meu `ItemTemplate` segue:

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

A marcação acima apresenta as informações do produto usando um cabeçalho &lt;H4&gt; para o nome do produto e uma `<table>` de quatro colunas para os campos restantes. As classes `ProductPropertyLabel` e `ProductPropertyValue` CSS, definidas no `Styles.css`, foram discutidas nos tutoriais anteriores. A Figura 3 mostra nosso progresso quando visualizado por meio de um navegador.

[![o nome, o fornecedor, a categoria, o status descontinuado e o preço de cada produto é exibido](customizing-the-datalist-s-editing-interface-cs/_static/image8.png)](customizing-the-datalist-s-editing-interface-cs/_static/image7.png)

**Figura 3**: o nome, fornecedor, categoria, status descontinuado e preço de cada produto é exibido ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image9.png))

## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Etapa 2: adicionando os controles da Web à interface de edição

A primeira etapa na criação da interface de edição DataList personalizada é adicionar os controles da Web necessários ao `EditItemTemplate`. Em particular, precisamos de uma DropDownList para a categoria, outra para o fornecedor e uma caixa de seleção para o estado descontinuado. Como o preço de s do produto não é editável neste exemplo, podemos continuar exibindo-o usando um controle de rótulo da Web.

Para personalizar a interface de edição, clique no link editar modelos na marca inteligente DataList s e escolha a opção `EditItemTemplate` na lista suspensa. Adicione uma DropDownList à `EditItemTemplate` e defina sua `ID` como `Categories`.

[![adicionar uma DropDownList para as categorias](customizing-the-datalist-s-editing-interface-cs/_static/image11.png)](customizing-the-datalist-s-editing-interface-cs/_static/image10.png)

**Figura 4**: adicionar uma DropDownList para as categorias ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image12.png))

Em seguida, na marca inteligente DropDownList s, selecione a opção escolher fonte de dados e crie um novo ObjectDataSource chamado `CategoriesDataSource`. Configure esse ObjectDataSource para usar o método de `GetCategories()` da classe `CategoriesBLL` (consulte a Figura 5). Em seguida, o assistente de configuração da fonte de dados DropDownList solicita os campos de dados a serem usados para cada `ListItem` s `Text` e `Value` Propriedades. Faça com que a DropDownList exiba o campo de dados `CategoryName` e use o `CategoryID` como o valor, como mostra a Figura 6.

[![criar um novo ObjectDataSource chamado CategoriesDataSource](customizing-the-datalist-s-editing-interface-cs/_static/image14.png)](customizing-the-datalist-s-editing-interface-cs/_static/image13.png)

**Figura 5**: criar um novo ObjectDataSource chamado `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image15.png))

[![configurar os campos de exibição e valor de DropDownList](customizing-the-datalist-s-editing-interface-cs/_static/image17.png)](customizing-the-datalist-s-editing-interface-cs/_static/image16.png)

**Figura 6**: configurar os campos de exibição e valor da DropDownList ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image18.png))

Repita esta série de etapas para criar uma DropDownList para os fornecedores. Defina o `ID` para este DropDownList como `Suppliers` e nomeie seu ObjectDataSource `SuppliersDataSource`.

Depois de adicionar os dois DropDownLists, adicione uma caixa de seleção para o estado descontinuado e uma caixa de texto para o nome do produto. Defina as `ID` s para a caixa de seleção e a caixa de texto como `Discontinued` e `ProductName`, respectivamente. Adicione um RequiredFieldValidator para garantir que o usuário forneça um valor para o nome do produto.

Por fim, adicione os botões atualizar e cancelar. Lembre-se de que, para esses dois botões, é imperativo que suas propriedades `CommandName` sejam definidas como Update e Cancel, respectivamente.

Fique à vontade para dispor a interface de edição, mas você gosta. Eu me optei por usar o mesmo layout de quatro colunas `<table>` da interface somente leitura, como a seguinte sintaxe declarativa e captura de tela ilustra:

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample2.aspx)]

[![a interface de edição está disposta como a interface somente leitura](customizing-the-datalist-s-editing-interface-cs/_static/image20.png)](customizing-the-datalist-s-editing-interface-cs/_static/image19.png)

**Figura 7**: a interface de edição é disposta como a interface somente leitura ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image21.png))

## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Etapa 3: criando os manipuladores de eventos EditCommand e CancelCommand

No momento, não há nenhuma sintaxe de DataBinding no `EditItemTemplate` (exceto para o `UnitPriceLabel`, que foi copiado sobre textual do `ItemTemplate`). Vamos adicionar a sintaxe DataBinding momentaneamente, mas primeiro vamos criar os manipuladores de eventos para os eventos DataList s `EditCommand` e `CancelCommand`. Lembre-se de que a responsabilidade do manipulador de eventos de `EditCommand` é renderizar a interface de edição para o item DataList cujo botão de edição foi clicado, enquanto que o trabalho de `CancelCommand` é retornar o DataList para seu estado de pré-edição.

Crie esses dois manipuladores de eventos e faça com que eles usem o seguinte código:

[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample3.cs)]

Com esses dois manipuladores de eventos em vigor, clicar no botão Editar exibe a interface de edição e, ao clicar no botão Cancelar, retorna o item editado para seu modo somente leitura. A Figura 8 mostra o DataList depois que o botão de edição foi clicado para o chefe Anton s Gumbo Mix. Como já vimos adicionar qualquer sintaxe de DataBinding à interface de edição, a caixa de texto `ProductName` está em branco, a caixa de seleção `Discontinued` desmarcada e os primeiros itens selecionados na `Categories` e `Suppliers` DropDownLists.

[![clicar no botão Editar exibe a interface de edição](customizing-the-datalist-s-editing-interface-cs/_static/image23.png)](customizing-the-datalist-s-editing-interface-cs/_static/image22.png)

**Figura 8**: clicar no botão Editar exibe a interface de edição ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image24.png))

## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Etapa 4: adicionando a sintaxe de DataBinding à interface de edição

Para que a interface de edição exiba os valores atuais do produto, precisamos usar a sintaxe DataBinding para atribuir os valores de campo de dados aos valores de controle da Web apropriados. A sintaxe de DataBinding pode ser aplicada por meio do designer acessando a tela editar modelos e selecionando o link editar DataBindings nas marcas inteligentes de controles da Web. Como alternativa, a sintaxe DataBinding pode ser adicionada diretamente à marcação declarativa.

Atribua o `ProductName` valor do campo de dados à propriedade `ProductName` `Text` de caixa de texto, os valores de campo de dados `CategoryID` e `SupplierID` ao `Categories` e `Suppliers` DropDownLists `SelectedValue` Propriedades e o valor do campo de dados `Discontinued` à propriedade `Discontinued` CheckBox s `Checked`. Depois de fazer essas alterações, por meio do designer ou diretamente por meio da marcação declarativa, reveja a página por meio de um navegador e clique no botão Editar para o chefe Anton s Gumbo Mix. Como mostra a Figura 9, a sintaxe de DataBinding adicionou os valores atuais em TextBox, DropDownLists e CheckBox.

[![clicar no botão Editar exibe a interface de edição](customizing-the-datalist-s-editing-interface-cs/_static/image26.png)](customizing-the-datalist-s-editing-interface-cs/_static/image25.png)

**Figura 9**: clicar no botão Editar exibe a interface de edição ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image27.png))

## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Etapa 5: salvar as alterações de s do usuário no manipulador de eventos UpdateCommand

Quando o usuário edita um produto e clica no botão atualizar, ocorre um postback e o evento DataList s `UpdateCommand` é disparado. No manipulador de eventos, precisamos ler os valores dos controles da Web no `EditItemTemplate` e na interface com a BLL para atualizar o produto no banco de dados. Como vimos nos tutoriais anteriores, a `ProductID` do produto atualizado é acessível por meio da coleção de `DataKeys`. Os campos inseridos pelo usuário são acessados por meio de programação fazendo referência aos controles da Web usando `FindControl("controlID")`, como mostra o código a seguir:

[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample4.cs)]

O código começa consultando a propriedade `Page.IsValid` para garantir que todos os controles de validação na página sejam válidos. Se `Page.IsValid` for `True`, o valor de `ProductID` do produto editado será lido na coleção de `DataKeys` e os controles da Web de entrada de dados no `EditItemTemplate` serão referenciados por meio de programação. Em seguida, os valores desses controles da Web são lidos em variáveis que são passadas para a sobrecarga de `UpdateProduct` apropriada. Depois de atualizar os dados, o DataList é retornado para seu estado de pré-edição.

> [!NOTE]
> Eu acabei omitido a lógica de tratamento de exceção adicionada no tutorial [lidando com exceções de nível de BLL e Dal](handling-bll-and-dal-level-exceptions-cs.md) para manter o código e esse exemplo focados. Como um exercício, adicione essa funcionalidade depois de concluir este tutorial.

## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Etapa 6: manipulando valores nulos de CategoryID e CódigoDoFornecedor

O banco de dados Northwind permite `NULL` valores para as colunas `CategoryID` de tabela `Products` e `SupplierID`. No entanto, nossa interface de edição não acomoda `NULL` valores no momento. Se tentarmos editar um produto que tem um valor `NULL` para suas colunas `CategoryID` ou `SupplierID`, obteremos um `ArgumentOutOfRangeException` com uma mensagem de erro semelhante a: *' categories ' tem um SelectedValue que é inválido porque não existe na lista de itens.* Além disso, atualmente não há como alterar uma categoria de produto ou valor de fornecedor de um valor não`NULL` para um `NULL`.

Para dar suporte a valores `NULL` para DropDownLists de categoria e de fornecedor, precisamos adicionar um `ListItem`adicional. Eu optei por usar (nenhum) como o valor de `Text` para esse `ListItem`, mas você pode alterá-lo para outra coisa (como uma cadeia de caracteres vazia), se você d desejar. Por fim, lembre-se de definir o `AppendDataBoundItems` DropDownLists como `True`; Se você se esquecer de fazer isso, as categorias e os fornecedores associados à DropDownList substituirão o `ListItem`adicionado estaticamente.

Depois de fazer essas alterações, a marcação DropDownLists na `EditItemTemplate` s de DataList deve ser semelhante ao seguinte:

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> `ListItem` s estáticos podem ser adicionados a uma DropDownList por meio do designer ou diretamente por meio da sintaxe declarativa. Ao adicionar um item DropDownList para representar um banco de dados `NULL` valor, certifique-se de adicionar o `ListItem` por meio da sintaxe declarativa. Se você usar o `ListItem` editor de coleção no designer, a sintaxe declarativa gerada omitirá a configuração de `Value` totalmente quando uma cadeia de caracteres em branco for atribuída, criando marcação declarativa como: `<asp:ListItem>(None)</asp:ListItem>`. Embora isso possa parecer inofensivo, o `Value` ausente faz com que o DropDownList use o valor da propriedade `Text` em seu lugar. Isso significa que, se essa `NULL` `ListItem` for selecionada, o valor (nenhum) será tentado ser atribuído ao campo de dados do produto (`CategoryID` ou `SupplierID`, neste tutorial), que resultará em uma exceção. Ao definir explicitamente `Value=""`, um valor de `NULL` será atribuído ao campo de dados do produto quando a `NULL` `ListItem` for selecionada.

Reserve um tempo para exibir nosso progresso por meio de um navegador. Ao editar um produto, observe que os DropDownLists `Categories` e `Suppliers` têm uma opção (nenhum) no início da DropDownList.

[![os DropDownLists de categorias e fornecedores incluem uma opção (nenhum)](customizing-the-datalist-s-editing-interface-cs/_static/image29.png)](customizing-the-datalist-s-editing-interface-cs/_static/image28.png)

**Figura 10**: os dropdownlists `Categories` e `Suppliers` incluem uma opção (nenhum) ([clique para exibir a imagem em tamanho normal](customizing-the-datalist-s-editing-interface-cs/_static/image30.png))

Para salvar a opção (nenhum) como um banco de dados `NULL` valor, precisamos retornar ao manipulador de eventos `UpdateCommand`. Altere as variáveis `categoryIDValue` e `supplierIDValue` para que sejam valores inteiros anuláveis e atribua a eles um valor diferente de `Nothing` somente se o `SelectedValue` de DropDownList s não for uma cadeia de caracteres vazia:

[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample6.cs)]

Com essa alteração, um valor de `Nothing` será passado para o método de `UpdateProduct` BLL se o usuário tiver selecionado a opção (nenhum) de qualquer uma das listas suspensas, que corresponde a um valor de banco de dados `NULL`.

## <a name="summary"></a>Resumo

Neste tutorial, vimos como criar uma interface de edição DataList mais complexa que incluía três controles da Web de entrada diferentes uma caixa de texto, duas DropDownLists e uma caixa de seleção junto com controles de validação. Ao criar a interface de edição, as etapas são as mesmas, independentemente dos controles da Web que estão sendo usados: comece adicionando os controles da Web para o DataList s `EditItemTemplate`; Use a sintaxe de DataBinding para atribuir os valores de campo de dados correspondentes com as propriedades de controle da Web apropriadas; e, no manipulador de eventos `UpdateCommand`, acesse programaticamente os controles da Web e suas propriedades apropriadas, passando seus valores para a BLL.

Ao criar uma interface de edição, seja composta por apenas TextBoxes ou uma coleção de diferentes controles da Web, certifique-se de tratar corretamente os valores de `NULL` de banco de dados. Ao contabilizar `NULL` s, é imperativo que você não só exiba corretamente um valor de `NULL` existente na interface de edição, mas também que você ofereça um meio para marcar um valor como `NULL`. Para DropDownLists em listas de controle, isso normalmente significa adicionar um `ListItem` estático cuja propriedade `Value` é explicitamente definida como uma cadeia de caracteres vazia (`Value=""`) e adicionar um pouco de código ao manipulador de eventos `UpdateCommand` para determinar se o `NULL``ListItem` foi selecionado ou não.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Dennis Patterson, David Suru e Randy Schmidt. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
> [Próximo](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
