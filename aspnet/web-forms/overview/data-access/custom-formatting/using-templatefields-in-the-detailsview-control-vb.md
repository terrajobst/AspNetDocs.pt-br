---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
title: Usando TemplateFields no controle DetailsView (VB) | Microsoft Docs
author: rick-anderson
description: Os mesmos recursos de TemplateFields disponíveis com o GridView também estão disponíveis com o controle DetailsView. Neste tutorial, vamos exibir um produto...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0b91d5f8-127d-4f6a-b204-f2e2b35ef703
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-detailsview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e96f954c27ae1c8ccc18a9c40fe7e541b487c1cc
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74624900"
---
# <a name="using-templatefields-in-the-detailsview-control-vb"></a>Uso de TemplateFields no controle DetailsView (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_13_VB.exe) ou [baixar PDF](using-templatefields-in-the-detailsview-control-vb/_static/datatutorial13vb1.pdf)

> Os mesmos recursos de TemplateFields disponíveis com o GridView também estão disponíveis com o controle DetailsView. Neste tutorial, vamos exibir um produto por vez usando um DetailsView contendo TemplateFields.

## <a name="introduction"></a>Introdução

O TemplateField oferece um grau mais alto de flexibilidade na renderização de dados do que os controles BoundField, CheckBoxField, HyperLinkField e outros campos de dados. No [tutorial anterior](using-templatefields-in-the-gridview-control-vb.md) , examinamos o uso de TemplateField em um GridView para:

- Exibe vários valores de campos de dados em uma coluna. Especificamente, os campos `FirstName` e `LastName` foram combinados em uma coluna GridView.
- Use um controle da Web alternativo para expressar um valor de campo de dados. Vimos como mostrar o valor `HiredDate` usando um controle Calendar.
- Mostrar informações de status com base nos dados subjacentes. Embora a tabela de `Employees` não contenha uma coluna que retorne o número de dias que um funcionário esteve no trabalho, pudemos exibir essas informações no exemplo de GridView no tutorial anterior com o uso de um TemplateField e um método de formatação.

Os mesmos recursos de TemplateFields disponíveis com o GridView também estão disponíveis com o controle DetailsView. Neste tutorial, vamos exibir um produto por vez usando um DetailsView que contenha dois TemplateFields. O primeiro TemplateField combinará os campos de dados `UnitPrice`, `UnitsInStock`e `UnitsOnOrder` em uma linha DetailsView. O segundo TemplateField exibirá o valor do campo `Discontinued`, mas usará um método de formatação para exibir "Sim" se `Discontinued` for `True`e "não" caso contrário.

[![dois TemplateFields são usados para personalizar a exibição](using-templatefields-in-the-detailsview-control-vb/_static/image2.png)](using-templatefields-in-the-detailsview-control-vb/_static/image1.png)

**Figura 1**: dois TemplateFields são usados para personalizar a exibição ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image3.png))

Vamos começar!

## <a name="step-1-binding-the-data-to-the-detailsview"></a>Etapa 1: associando os dados ao DetailsView

Conforme discutido no tutorial anterior, ao trabalhar com TemplateFields, geralmente é mais fácil começar criando o controle DetailsView que contém apenas BoundFields e, em seguida, adicionar novo TemplateFields ou converter os BoundFields existentes em TemplateFields, conforme necessário . Portanto, inicie este tutorial adicionando um DetailsView à página por meio do designer e ligando-o a um ObjectDataSource que retorna a lista de produtos. Essas etapas criarão um DetailsView com BoundFields para cada um dos campos de valor não booliano do produto e um CheckBoxField para um campo de valor booliano (descontinuado).

Abra a página `DetailsViewTemplateField.aspx` e arraste uma DetailsView da caixa de ferramentas para o designer. Da marca inteligente de DetailsView, escolha Adicionar um novo controle ObjectDataSource que invoca o método `GetProducts()` da classe `ProductsBLL`.

[![adicionar um novo controle ObjectDataSource que invoca o método GetProducts ()](using-templatefields-in-the-detailsview-control-vb/_static/image5.png)](using-templatefields-in-the-detailsview-control-vb/_static/image4.png)

**Figura 2**: adicionar um novo controle ObjectDataSource que invoca o método `GetProducts()` ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image6.png))

Para este relatório, remova as `ProductID`, `SupplierID`, `CategoryID`e `ReorderLevel` BoundFields. Em seguida, reordene os BoundFields para que os `CategoryName` e `SupplierName` BoundField apareçam imediatamente após o `ProductName` BoundField. Sinta-se à vontade para ajustar as propriedades de `HeaderText` e propriedades de formatação para os BoundFields como você pode ver. Assim como com o GridView, essas edições no nível de BoundField podem ser executadas por meio da caixa de diálogo campos (acessível clicando no link editar campos na marca inteligente de DetailsView) ou por meio da sintaxe declarativa. Por fim, desmarque os valores de propriedade `Height` e `Width` de DetailsView para permitir que o controle DetailsView se expanda com base nos dados exibidos e marque a caixa de seleção habilitar paginação na marca inteligente.

Depois de fazer essas alterações, a marcação declarativa do controle DetailsView deve ser semelhante ao seguinte:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample1.aspx)]

Reserve um tempo para exibir a página por meio de um navegador. Neste ponto, você deve ver um único produto listado (Chai) com linhas que mostram o nome do produto, a categoria, o fornecedor, o preço, as unidades em estoque, as unidades na ordem e seu status descontinuado.

[![os detalhes do produto são mostrados usando uma série de BoundFields](using-templatefields-in-the-detailsview-control-vb/_static/image8.png)](using-templatefields-in-the-detailsview-control-vb/_static/image7.png)

**Figura 3**: os detalhes do produto são mostrados usando uma série de boundfields ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image9.png))

## <a name="step-2-combining-the-price-units-in-stock-and-units-on-order-into-one-row"></a>Etapa 2: combinando o preço, as unidades em estoque e as unidades na ordem em uma linha

O DetailsView tem uma linha para os campos `UnitPrice`, `UnitsInStock`e `UnitsOnOrder`. Podemos combinar esses campos de dados em uma única linha com um TemplateField, adicionando um novo TemplateField ou convertendo um dos `UnitPrice`existentes, `UnitsInStock`e `UnitsOnOrder` BoundFields em um TemplateField. Embora eu prefira, pessoalmente, a conversão de BoundFields existentes, vamos praticar adicionando um novo TemplateField.

Comece clicando no link editar campos na marca inteligente de DetailsView para abrir a caixa de diálogo campos. Em seguida, adicione um novo TemplateField e defina sua propriedade `HeaderText` como "preço e inventário" e mova o novo TemplateField para que ele esteja posicionado acima do `UnitPrice` BoundField.

[![adicionar um novo TemplateField ao controle DetailsView](using-templatefields-in-the-detailsview-control-vb/_static/image11.png)](using-templatefields-in-the-detailsview-control-vb/_static/image10.png)

**Figura 4**: adicionar um novo TemplateField ao controle DetailsView ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image12.png))

Como esse novo TemplateField conterá os valores atualmente exibidos no `UnitPrice`, `UnitsInStock`e `UnitsOnOrder` BoundFields, vamos removê-los.

A última tarefa para esta etapa é definir a marcação de `ItemTemplate` para o preço e o modelo de inventário, que pode ser realizado por meio da interface de edição de modelo de DetailsView no designer ou manualmente por meio da sintaxe declarativa do controle. Assim como no GridView, a interface de edição do modelo DetailsView pode ser acessada clicando no link editar modelos na marca inteligente. A partir daqui, você pode selecionar o modelo a ser editado na lista suspensa e, em seguida, adicionar todos os controles da Web da caixa de ferramentas.

Para este tutorial, comece adicionando um controle de rótulo ao preço e ao `ItemTemplate`do modelo de inventário. Em seguida, clique no link editar DataBindings na marca inteligente do controle da Web do rótulo e associe a propriedade `Text` ao campo `UnitPrice`.

[![associar a propriedade Text do rótulo ao campo de dados UnitPrice](using-templatefields-in-the-detailsview-control-vb/_static/image14.png)](using-templatefields-in-the-detailsview-control-vb/_static/image13.png)

**Figura 5**: associar a propriedade de `Text` do rótulo ao campo de dados de `UnitPrice` ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image15.png))

## <a name="formatting-the-price-as-a-currency"></a>Formatando o preço como uma moeda

Com essa adição, o rótulo preço de controle da Web e inventário TemplateField agora exibirão apenas o preço do produto selecionado. A Figura 6 mostra uma captura de tela do nosso progresso até o momento quando exibida por meio de um navegador.

[![o preço e o modelo de inventário mostram o preço](using-templatefields-in-the-detailsview-control-vb/_static/image17.png)](using-templatefields-in-the-detailsview-control-vb/_static/image16.png)

**Figura 6**: o preço e o modelo de inventário mostram o preço ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image18.png))

Observe que o preço do produto não está formatado como uma moeda. Com um BoundField, a formatação é possível definindo a propriedade `HtmlEncode` como `False` e a propriedade `DataFormatString` como `{0:formatSpecifier}`. Para um TemplateField, no entanto, todas as instruções de formatação devem ser especificadas na sintaxe DataBinding ou pelo uso de um método de formatação definido em algum lugar dentro do código do aplicativo (como na classe code-behind da página ASP.NET).

Para especificar a formatação para a sintaxe de DataBinding usada no controle de rótulo da Web, retorne à caixa de diálogo DataBindings clicando no link editar DataBindings da marca inteligente do rótulo. Você pode digitar as instruções de formatação diretamente na lista suspensa formato ou selecionar uma das cadeias de caracteres de formato definidas. Assim como com a propriedade `DataFormatString` de BoundField, a formatação é especificada usando `{0:formatSpecifier}`.

Para o campo `UnitPrice`, use a formatação de moeda especificada selecionando o valor da lista suspensa apropriada ou digitando `{0:C}` manualmente.

[![formatar o preço como uma moeda](using-templatefields-in-the-detailsview-control-vb/_static/image20.png)](using-templatefields-in-the-detailsview-control-vb/_static/image19.png)

**Figura 7**: Formatar o preço como uma moeda ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image21.png))

Declarativamente, a especificação de formatação é indicada como um segundo parâmetro nos métodos `Bind` ou `Eval`. As configurações feitas apenas por meio do designer resultam na seguinte expressão de DataBinding na marcação declarativa:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample2.aspx)]

## <a name="adding-the-remaining-data-fields-to-the-templatefield"></a>Adicionando os campos de dados restantes ao TemplateField

Neste ponto, exibimos e Formatamos o campo de dados `UnitPrice` no modelo de preço e inventário, mas ainda precisamos exibir os campos `UnitsInStock` e `UnitsOnOrder`. Vamos exibi-las em uma linha abaixo do preço e entre parênteses. Da interface de edição de modelo no designer, essa marcação pode ser adicionada posicionando o cursor dentro do modelo e simplesmente digitando o texto a ser exibido. Como alternativa, essa marcação pode ser inserida diretamente na sintaxe declarativa.

Adicione a marcação estática, o rótulo controles da Web e a sintaxe de DataBinding para que o modelo de preço e inventário exiba as informações de preço e inventário da seguinte maneira:

*PreçoUnitário*  
(**Em estoque/em ordem:** *UnidadesEmEstoque* / *UnitsOnOrder*)

Depois de executar essa tarefa, a marcação declarativa de DetailsView deve ser semelhante ao seguinte:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample3.aspx)]

Com essas alterações, consolidamos as informações de preço e inventário em uma única linha DetailsView.

[![as informações de preço e inventário são exibidas em uma única linha](using-templatefields-in-the-detailsview-control-vb/_static/image23.png)](using-templatefields-in-the-detailsview-control-vb/_static/image22.png)

**Figura 8**: as informações de preço e de inventário são exibidas em uma única linha ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image24.png))

## <a name="step-3-customizing-the-discontinued-field-information"></a>Etapa 3: Personalizando as informações do campo descontinuado

A coluna `Discontinued` da tabela de `Products` é um valor de bit que indica se o produto foi descontinuado. Ao associar um DetailsView (ou GridView) a um controle da fonte de dados, os campos de valor booliano, como `Discontinued`, são implementados como CheckBoxFields, enquanto os campos de valor não booliano, como `ProductID`, `ProductName`e assim por diante, são implementados como BoundFields. O CheckBoxField é renderizado como uma caixa de seleção desabilitada que é marcada se o valor do campo de dados for true e desmarcado de outra forma.

Em vez de exibir o CheckBoxField, talvez queiramos exibir texto indicando se o produto foi descontinuado ou não. Para fazer isso, poderíamos remover o CheckBoxField do DetailsView e, em seguida, adicionar um BoundField cuja propriedade `DataField` foi definida como `Discontinued`. Reserve um tempo para fazer isso. Após essa alteração, o DetailsView mostra o texto "true" para produtos descontinuados e "false" para produtos que ainda estão ativos.

[![as cadeias de caracteres true e false são usadas para exibir o estado descontinuado](using-templatefields-in-the-detailsview-control-vb/_static/image26.png)](using-templatefields-in-the-detailsview-control-vb/_static/image25.png)

**Figura 9**: as cadeias de caracteres verdadeiro e falso são usadas para exibir o estado descontinuado ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image27.png))

Imagine que não queremos que as cadeias de caracteres "true" ou "false" sejam usadas, mas "YES" e "NO", em vez disso. Essa personalização pode ser executada com o auxílio de um TemplateField e um método de formatação. Um método de formatação pode assumir qualquer número de parâmetros de entrada, mas deve retornar o HTML (como uma cadeia de caracteres) para injetar no modelo.

Adicione um método de formatação à classe code-behind da página `DetailsViewTemplateField.aspx` chamada `DisplayDiscontinuedAsYESorNO` que aceita um objeto `Northwind.ProductsRow` como um parâmetro de entrada e retorna uma cadeia de caracteres. Conforme discutido no tutorial anterior, esse método *deve* ser marcado como `Protected` ou `Public` para poder ser acessado do modelo.

[!code-vb[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample4.vb)]

Esse método verifica o parâmetro de entrada (`discontinued`) e retorna "Sim" se for `True`, "não" caso contrário.

> [!NOTE]
> No método de formatação examinado no tutorial anterior, lembre-se de que estávamos passando um campo de dados que pode conter `NULL` s e, portanto, precisou verificar se o valor da propriedade de `HiredDate` do funcionário tinha um valor de `NULL` de banco de dado antes de acessar a propriedade `HiredDate` do `EmployeesRow`. Essa verificação não é necessária aqui, pois a coluna `Discontinued` nunca pode ter valores de `NULL` de banco de dados atribuídos. Além disso, é por isso que o método pode aceitar um parâmetro de entrada booliano em vez de ter que aceitar uma instância `ProductsRow` ou um parâmetro do tipo `Object`.

Com esse método de formatação concluído, tudo o que resta é chamá-lo do `ItemTemplate`do TemplateField. Para criar o TemplateField, remova o `Discontinued` BoundField e adicione um novo TemplateField ou converta o `Discontinued` BoundField em um TemplateField. Em seguida, na exibição de marcação declarativa, edite o TemplateField para que ele contenha apenas um ItemTemplate que invoque o método `DisplayDiscontinuedAsYESorNO`, passando o valor da propriedade `Discontinued` da instância de `ProductRow` atual. Isso pode ser acessado por meio do método `Eval`. Especificamente, a marcação do Modelofield deve ser semelhante a:

[!code-aspx[Main](using-templatefields-in-the-detailsview-control-vb/samples/sample5.aspx)]

Isso fará com que o método `DisplayDiscontinuedAsYESorNO` seja invocado durante a renderização de DetailsView, passando o valor de `Discontinued` da instância de `ProductRow`. Como o método `Eval` retorna um valor do tipo `Object`, mas o método `DisplayDiscontinuedAsYESorNO` espera um parâmetro de entrada do tipo `Boolean`, convertemos o valor de retorno dos métodos `Eval` em `Boolean`. O método `DisplayDiscontinuedAsYESorNO` retornará "Sim" ou "não", dependendo do valor que receber. O valor retornado é o que é exibido nesta linha DetailsView (consulte a Figura 10).

[![Sim ou nenhum valor agora é mostrado na linha descontinuada](using-templatefields-in-the-detailsview-control-vb/_static/image29.png)](using-templatefields-in-the-detailsview-control-vb/_static/image28.png)

**Figura 10**: Sim ou nenhum valor agora é mostrado na linha descontinuada ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-detailsview-control-vb/_static/image30.png))

## <a name="summary"></a>Resumo

O TemplateField no controle DetailsView permite um grau mais alto de flexibilidade na exibição de dados do que está disponível com os outros controles de campo e é ideal para situações em que:

- Vários campos de dados precisam ser exibidos em uma coluna GridView
- Os dados são mais bem expressos usando um controle da Web em vez de texto sem formatação
- A saída depende dos dados subjacentes, como a exibição de metadados ou a reformatação dos dados

Embora o TemplateFields permita um maior grau de flexibilidade na renderização dos dados subjacentes de DetailsView, a saída DetailsView ainda se sentirá um pouco boxy, pois cada campo é renderizado como uma linha em um `<table>`HTML.

O controle FormView oferece um grau maior de flexibilidade na configuração da saída renderizada. O FormView não contém campos, mas sim apenas uma série de modelos (`ItemTemplate`, `EditItemTemplate`, `HeaderTemplate`e assim por diante). Veremos como usar o FormView para obter ainda mais controle sobre o layout renderizado em nosso próximo tutorial.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Dan Jagers. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-templatefields-in-the-gridview-control-vb.md)
> [Próximo](using-the-formview-s-templates-vb.md)
