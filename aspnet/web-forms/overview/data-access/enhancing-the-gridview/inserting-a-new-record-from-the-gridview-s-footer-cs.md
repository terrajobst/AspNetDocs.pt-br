---
uid: web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
title: Inserindo um novo registro no rodapé do GridView (C#) | Microsoft Docs
author: rick-anderson
description: Embora o controle GridView não forneça suporte interno para inserir um novo registro de dados, este tutorial mostra como aumentar o GridView para incluir um...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 49545652-98af-46ba-9dbc-9ab529805d9b
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/inserting-a-new-record-from-the-gridview-s-footer-cs
msc.type: authoredcontent
ms.openlocfilehash: 6b337fe395a0ed54b03767111e73ea6d6c0ba23a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74592174"
---
# <a name="inserting-a-new-record-from-the-gridviews-footer-c"></a>Inserir um novo registro do rodapé do GridView (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_53_CS.exe) ou [baixar PDF](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/datatutorial53cs1.pdf)

> Embora o controle GridView não forneça suporte interno para inserir um novo registro de dados, este tutorial mostra como aumentar o GridView para incluir uma interface de inserção.

## <a name="introduction"></a>Introdução

Conforme discutido na [visão geral do tutorial inserir, atualizar e excluir dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , os controles da Web GridView, DetailsView e FormView incluem recursos internos de modificação de dados. Quando usados com controles de fonte de dados declarativos, esses três controles da Web podem ser configurados de forma rápida e fácil para modificar dados e em cenários sem a necessidade de escrever uma única linha de código. Infelizmente, somente os controles DetailsView e FormView fornecem recursos internos de inserção, edição e exclusão. O GridView oferece suporte apenas para edição e exclusão. No entanto, com uma pequena graxa de cotovelo, podemos aumentar o GridView para incluir uma interface de inserção.

Ao adicionar recursos de inserção ao GridView, somos responsáveis por decidir como novos registros serão adicionados, criando a interface de inserção e gravando o código para inserir o novo registro. Neste tutorial, veremos como adicionar a interface de inserção à linha de rodapé do GridView (consulte a Figura 1). A célula footer de cada coluna inclui o elemento de interface do usuário de coleta de dados apropriado (uma caixa de texto para o nome do produto, uma DropDownList para o fornecedor e assim por diante). Também precisamos de uma coluna para um botão Adicionar que, quando clicado, causará um postback e inserirá um novo registro na tabela `Products` usando os valores fornecidos na linha de rodapé.

[![a linha de rodapé fornece uma interface para adicionar novos produtos](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image1.png)

**Figura 1**: a linha de rodapé fornece uma interface para adicionar novos produtos ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.png))

## <a name="step-1-displaying-product-information-in-a-gridview"></a>Etapa 1: exibindo informações do produto em um GridView

Antes de nos preocuparmos com a criação da interface de inserção no rodapé do GridView, vamos primeiro nos concentrar na adição de um GridView à página que lista os produtos no banco de dados. Comece abrindo a página de `InsertThroughFooter.aspx` na pasta `EnhancedGridView` e arraste um GridView da caixa de ferramentas para o designer, definindo a Propriedade GridView s `ID` como `Products`. Em seguida, use a marca inteligente s do GridView para associá-la a um novo ObjectDataSource chamado `ProductsDataSource`.

[![criar um novo ObjectDataSource chamado ProductsDataSource](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image2.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.png)

**Figura 2**: criar um novo ObjectDataSource chamado `ProductsDataSource` ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.png))

Configure o ObjectDataSource para usar o método de `GetProducts()` da classe `ProductsBLL` para recuperar informações do produto. Para este tutorial, vamos nos concentrar estritamente em Adicionar recursos de inserção e não se preocupar com edição e exclusão. Portanto, verifique se a lista suspensa na guia Inserir está definida como `AddProduct()` e se as listas suspensas nas guias atualizar e excluir estão definidas como (nenhum).

[![mapear o método addproduct para o método ObjectDataSource s Insert ()](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image3.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.png)

**Figura 3**: mapear o método `AddProduct` para o método ObjectDataSource s `Insert()` ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.png))

[![definir as listas suspensas atualizar e excluir guias como (nenhum)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image4.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.png)

**Figura 4**: definir as listas suspensas atualizar e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.png))

Depois de concluir o assistente de configuração de fonte de dados ObjectDataSource s, o Visual Studio adicionará automaticamente campos ao GridView para cada um dos campos de dados correspondentes. Por enquanto, deixe todos os campos adicionados pelo Visual Studio. Posteriormente neste tutorial, voltaremos e removeremos alguns dos campos cujos valores não precisam ser especificados ao adicionar um novo registro.

Como há cerca de 80 produtos no banco de dados, um usuário terá que rolar até o final da página da Web para adicionar um novo registro. Portanto, deixe que o s habilite a paginação para tornar a interface de inserção mais visível e acessível. Para ativar a paginação, basta marcar a caixa de seleção habilitar paginação na marca inteligente s do GridView.

Neste ponto, a marcação declarativa de GridView e ObjectDataSource s deve ser semelhante ao seguinte:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample1.aspx)]

[![todos os campos de dados do produto são exibidos em um GridView paginado](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image5.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.png)

**Figura 5**: todos os campos de dados do produto são exibidos em um GridView paginado ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.png))

## <a name="step-2-adding-a-footer-row"></a>Etapa 2: adicionando uma linha de rodapé

Junto com suas linhas de dados e cabeçalho, o GridView inclui uma linha de rodapé. As linhas de cabeçalho e rodapé são exibidas dependendo dos valores das propriedades GridView s [`ShowHeader`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.showheader.aspx) e [`ShowFooter`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.showfooter.aspx) . Para mostrar a linha de rodapé, basta definir a propriedade `ShowFooter` como `true`. Como a Figura 6 ilustra, a definição da propriedade `ShowFooter` como `true` adiciona uma linha de rodapé à grade.

[![para exibir a linha de rodapé, defina mostrar rodapé como verdadeiro](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image6.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.png)

**Figura 6**: para exibir a linha do rodapé, defina `ShowFooter` como `True` ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.png))

Observe que a linha de rodapé tem uma cor de fundo vermelha escura. Isso ocorre devido ao tema datawebcontrols que criamos e aplicados a todas as páginas de volta no tutorial [exibindo dados com o ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) . Especificamente, o arquivo `GridView.skin` configura a propriedade `FooterStyle` de modo que use a classe CSS `FooterStyle`. A classe `FooterStyle` é definida em `Styles.css` da seguinte maneira:

[!code-css[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample2.css)]

> [!NOTE]
> Nós exploramos usando a linha de rodapé do GridView nos tutoriais anteriores. Se necessário, consulte as informações de [Resumo de exibição no tutorial de rodapé do GridView](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) para obter um atualizador.

Depois de definir a propriedade `ShowFooter` como `true`, Reserve um momento para exibir a saída em um navegador. Atualmente, a linha de rodapé não contém nenhum controle de texto ou da Web. Na etapa 3, modificaremos o rodapé de cada campo GridView para que ele inclua a interface de inserção apropriada.

[![a linha de rodapé vazia é exibida acima dos controles da interface de paginação](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image7.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image13.png)

**Figura 7**: a linha de rodapé vazia é exibida acima dos controles da interface de paginação ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image14.png))

## <a name="step-3-customizing-the-footer-row"></a>Etapa 3: Personalizando a linha de rodapé

De volta ao tutorial [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) , vimos como personalizar bastante a exibição de uma determinada coluna GridView usando TemplateFields (em oposição a boundfields ou checkboxfields); ao [Personalizar a interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) , examinamos o uso de TemplateFields para personalizar a interface de edição em um GridView. Lembre-se de que um TemplateField é composto por vários modelos que definem a combinação de marcação, controles da Web e a sintaxe de DataBinding usada para determinados tipos de linhas. O `ItemTemplate`, por exemplo, especifica o modelo usado para linhas somente leitura, enquanto a `EditItemTemplate` define o modelo para a linha editável.

Junto com o `ItemTemplate` e `EditItemTemplate`, o TemplateField também inclui um `FooterTemplate` que especifica o conteúdo da linha de rodapé. Portanto, podemos adicionar os controles da Web necessários para cada campo que insere a interface na `FooterTemplate`. Para começar, converta todos os campos em GridView para TemplateFields. Isso pode ser feito clicando no link Editar colunas na marca inteligente GridView s, selecionando cada campo no canto inferior esquerdo e clicando no link converter este campo em um modelo.

![Converter cada campo em um TemplateField](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image8.gif)

**Figura 8**: converter cada campo em um TemplateField

Clicar em converter este campo em um TemplateField transforma o tipo de campo atual em um TemplateField equivalente. Por exemplo, cada BoundField é substituído por um TemplateField por um `ItemTemplate` que contém um rótulo que exibe o campo de dados correspondente e um `EditItemTemplate` que exibe o campo de dados em uma caixa de texto. O `ProductName` BoundField foi convertido na seguinte marcação de TemplateField:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample3.aspx)]

Da mesma forma, o CheckBoxField `Discontinued` foi convertido em um TemplateField cujo `ItemTemplate` e `EditItemTemplate` contêm um controle da Web de caixa de seleção (com a caixa de seleção `ItemTemplate` s desabilitada). O `ProductID` BoundField somente leitura foi convertido em um TemplateField com um controle rótulo no `ItemTemplate` e `EditItemTemplate`. Em suma, a conversão de um campo GridView existente em um TemplateField é uma maneira rápida e fácil de alternar para o TemplateField mais personalizável sem perder nenhuma das funcionalidades existentes do campo.

Como o GridView que estamos trabalhando com não dá suporte à edição, sinta-se à vontade para remover o `EditItemTemplate` de cada TemplateField, deixando apenas o `ItemTemplate`. Depois de fazer isso, sua marcação declarativa s de GridView deve ser semelhante ao seguinte:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample4.aspx)]

Agora que cada campo GridView foi convertido em um TemplateField, podemos inserir a interface de inserção apropriada em cada campo s `FooterTemplate`. Alguns dos campos não terão uma interface de inserção (`ProductID`, por exemplo); outras irão variar nos controles da Web usados para coletar as informações do novo produto.

Para criar a interface de edição, escolha o link editar modelos na marca inteligente de GridView. Em seguida, na lista suspensa, selecione o campo apropriado `FooterTemplate` e arraste o controle apropriado da caixa de ferramentas para o designer.

[![adicionar a interface de inserção apropriada para cada campo Rodapétemplate](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image9.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image15.png)

**Figura 9**: adicionar a interface de inserção apropriada a cada s `FooterTemplate` de campo ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image16.png))

A lista com marcadores a seguir enumera os campos GridView, especificando a interface de inserção a ser adicionada:

- `ProductID` nenhum.
- `ProductName` adicionar uma caixa de texto e definir sua `ID` como `NewProductName`. Adicione um controle RequiredFieldValidator também para garantir que o usuário insira um valor para o novo nome do produto.
- `SupplierID` nenhum.
- `CategoryID` nenhum.
- `QuantityPerUnit` adicionar uma caixa de texto, definindo sua `ID` como `NewQuantityPerUnit`.
- `UnitPrice` adicionar uma caixa de texto chamada `NewUnitPrice` e um CompareValidator que garanta que o valor inserido é um valor de moeda maior ou igual a zero.
- `UnitsInStock` usar uma caixa de texto cuja `ID` está definida como `NewUnitsInStock`. Inclua um CompareValidator que garanta que o valor inserido seja um valor inteiro maior ou igual a zero.
- `UnitsOnOrder` usar uma caixa de texto cuja `ID` está definida como `NewUnitsOnOrder`. Inclua um CompareValidator que garanta que o valor inserido seja um valor inteiro maior ou igual a zero.
- `ReorderLevel` usar uma caixa de texto cuja `ID` está definida como `NewReorderLevel`. Inclua um CompareValidator que garanta que o valor inserido seja um valor inteiro maior ou igual a zero.
- `Discontinued` adicionar uma caixa de seleção, definindo sua `ID` como `NewDiscontinued`.
- `CategoryName` adicionar uma DropDownList e definir sua `ID` como `NewCategoryID`. Associe-o a um novo ObjectDataSource chamado `CategoriesDataSource` e configure-o para usar o método de `GetCategories()` da classe `CategoriesBLL`. Faça com que os s `ListItem` s exibam o `CategoryName` campo de dados, usando o campo de dados `CategoryID` como seus valores.
- `SupplierName` adicionar uma DropDownList e definir sua `ID` como `NewSupplierID`. Associe-o a um novo ObjectDataSource chamado `SuppliersDataSource` e configure-o para usar o método de `GetSuppliers()` da classe `SuppliersBLL`. Faça com que os s `ListItem` s exibam o `CompanyName` campo de dados, usando o campo de dados `SupplierID` como seus valores.

Para cada um dos controles de validação, desmarque a propriedade `ForeColor` de forma que a cor `FooterStyle` de primeiro plano branco da classe CSS s será usada no lugar do vermelho padrão. Use também a propriedade `ErrorMessage` para obter uma descrição detalhada, mas defina a propriedade `Text` como um asterisco. Para impedir que o texto do controle de validação faça com que a interface de inserção seja encapsulada em duas linhas, defina a propriedade `FooterStyle` s `Wrap` como false para cada uma das `FooterTemplate` s que usam um controle de validação. Por fim, adicione um controle ValidationSummary sob o GridView e defina sua propriedade `ShowMessageBox` como `true` e sua propriedade `ShowSummary` como `false`.

Ao adicionar um novo produto, precisamos fornecer o `CategoryID` e `SupplierID`. Essas informações são capturadas por meio de DropDownLists nas células de rodapé para os campos `CategoryName` e `SupplierName`. Optei por usar esses campos em oposição ao `CategoryID` e `SupplierID` TemplateFields, pois nas linhas de dados da grade, o usuário provavelmente está mais interessado em Ver os nomes de categoria e de fornecedor em vez de seus valores de ID. Como os valores de `CategoryID` e `SupplierID` agora estão sendo capturados nos `CategoryName` e `SupplierName` campo s inserindo interfaces, podemos remover os `CategoryID` e `SupplierID` TemplateFields do GridView.

Da mesma forma, o `ProductID` não é usado ao adicionar um novo produto, de modo que o `ProductID` TemplateField também pode ser removido. No entanto, vamos deixar o campo `ProductID` na grade. Além das caixas de entrada, DropDownLists, caixas de seleção e controles de validação que compõem a interface de inserção, também precisaremos de um botão Adicionar que, quando clicado, execute a lógica para adicionar o novo produto ao banco de dados. Na etapa 4, incluiremos um botão Adicionar na interface de inserção na `FooterTemplate``ProductID` TemplateField.

Sinta-se à vontade para melhorar a aparência dos vários campos GridView. Por exemplo, você pode querer formatar os valores de `UnitPrice` como uma moeda, alinhar à direita os campos `UnitsInStock`, `UnitsOnOrder`e `ReorderLevel` e atualizar os valores de `HeaderText` para o TemplateFields.

Depois de criar a velocidade de inserção de interfaces no `FooterTemplate` s, remover o `SupplierID`e `CategoryID` TemplateFields e melhorar os estética da grade por meio da formatação e do alinhamento da TemplateFields, sua marcação declarativa de GridView deve ser semelhante ao seguinte:

[!code-aspx[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample5.aspx)]

Quando visualizado por meio de um navegador, a linha de rodapé do GridView agora inclui a interface de inserção concluída (consulte a Figura 10). Neste ponto, a interface de inserção não inclui um meio para que o usuário indique que ela s inseriu os dados do novo produto e deseja inserir um novo registro no banco de dado. Além disso, também conseguimos abordar como os dados inseridos no rodapé serão convertidos em um novo registro no banco de dados de `Products`. Na etapa 4, veremos como incluir um botão Add na interface de inserção e como executar código em postback quando clicados. A etapa 5 mostra como inserir um novo registro usando os dados do rodapé.

[![o rodapé GridView fornece uma interface para adicionar um novo registro](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image10.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image17.png)

**Figura 10**: o rodapé GridView fornece uma interface para adicionar um novo registro ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image18.png))

## <a name="step-4-including-an-add-button-in-the-inserting-interface"></a>Etapa 4: incluindo um botão Adicionar na interface de inserção

Precisamos incluir um botão Adicionar em algum lugar na interface de inserção, pois a linha de rodapé que está inserindo a interface atualmente não tem os meios para o usuário indicar que eles concluíram a inserção das informações do novo produto. Isso pode ser colocado em um dos `FooterTemplate` s existentes ou poderíamos adicionar uma nova coluna à grade para essa finalidade. Para este tutorial, vamos posicionar o botão Adicionar na `FooterTemplate``ProductID` TemplateField.

No designer, clique no link editar modelos na marca inteligente s de GridView e escolha o campo `ProductID` s `FooterTemplate` na lista suspensa. Adicione um controle Web de botão (ou um LinkButton ou ImageButton, se preferir) ao modelo, definindo sua ID como `AddProduct`, sua `CommandName` para inserir e sua propriedade `Text` para adicionar, conforme mostrado na Figura 11.

[![Coloque o botão Adicionar no modelo de ProductID do](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image11.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image19.png)

**Figura 11**: Coloque o botão adicionar na `FooterTemplate` `ProductID` TemplateField ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image20.png))

Depois de incluir o botão Adicionar, teste a página em um navegador. Observe que, ao clicar no botão Adicionar com dados inválidos na interface de inserção, o postback é curto circuito e o controle ValidationSummary indica os dados inválidos (consulte a Figura 12). Com os dados apropriados inseridos, clicar no botão Adicionar causa um postback. No entanto, nenhum registro é adicionado ao banco de dados. Precisaremos escrever um pouco de código para realmente executar a inserção.

[![o postback de s de botão Adicionar é curto circuitado se houver dados inválidos na interface de inserção](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image12.gif)](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image21.png)

**Figura 12**: o postback s do botão Adicionar é curto circuito se houver dados inválidos na interface de inserção ([clique para exibir a imagem em tamanho normal](inserting-a-new-record-from-the-gridview-s-footer-cs/_static/image22.png))

> [!NOTE]
> Os controles de validação na interface de inserção não foram atribuídos a um grupo de validação. Isso funciona bem, desde que a interface de inserção seja o único conjunto de controles de validação na página. No entanto, se houver outros controles de validação na página (como controles de validação na interface de edição da grade s), os controles de validação nas propriedades inserindo interface e adicionar s `ValidationGroup` devem receber o mesmo valor para associar esses controles a um grupo de validação específico. Consulte disparando [os controles de validação no ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx) para obter mais informações sobre como particionar os controles de validação e os botões em uma página em grupos de validação.

## <a name="step-5-inserting-a-new-record-into-theproductstable"></a>Etapa 5: inserindo um novo registro na tabela`Products`

Ao utilizar os recursos de edição internos do GridView, o GridView manipula automaticamente todo o trabalho necessário para a execução da atualização. Em particular, quando o botão Atualizar é clicado, ele copia os valores inseridos da interface de edição para os parâmetros na coleção ObjectDataSource s `UpdateParameters` e inicia a atualização invocando o método ObjectDataSource s `Update()`. Como o GridView não fornece essa funcionalidade interna para inserção, devemos implementar o código que chama o método ObjectDataSource s `Insert()` e copiar os valores da interface de inserção para a coleção ObjectDataSource s `InsertParameters`.

Essa lógica de inserção deve ser executada Depois que o botão Adicionar for clicado. Conforme discutido nos [botões adicionando e respondendo a em um tutorial GridView](../custom-button-actions/adding-and-responding-to-buttons-to-a-gridview-cs.md) , sempre que um botão, LinkButton ou ImageButton em um GridView é clicado, o evento GridView `RowCommand` é acionado no postback. Esse evento dispara se o botão, LinkButton ou ImageButton foi adicionado explicitamente, como o botão Adicionar na linha de rodapé ou se foi adicionado automaticamente pelo GridView (como LinkButtons na parte superior de cada coluna quando Habilitar classificação está selecionado ou o LinkButtons na interface de paginação quando habilitar a paginação é selecionado).

Portanto, para responder ao usuário clicando no botão Adicionar, precisamos criar um manipulador de eventos para o evento GridView s `RowCommand`. Como esse evento é acionado sempre que *qualquer* Button, LinkButton ou ImageButton no GridView é clicado, é vital que continuemos apenas com a lógica de inserção se a propriedade `CommandName` passada para o manipulador de eventos for mapeada para o valor `CommandName` do botão Adicionar (Insert). Além disso, também devemos continuar apenas se os controles de validação relatarem dados válidos. Para acomodar isso, crie um manipulador de eventos para o evento `RowCommand` com o seguinte código:

[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample6.cs)]

> [!NOTE]
> Você deve estar se perguntando por que os dois manipuladores de eventos verificam a propriedade `Page.IsValid`. Afinal, o postback não será suprimido se forem fornecidos dados inválidos na interface de inserção? Essa suposição está correta, desde que o usuário não tenha desabilitado o JavaScript ou tenha levado em consideração as etapas para burlar a lógica de validação do lado do cliente. Em resumo, um nunca deve depender estritamente da validação do lado do cliente; uma verificação do lado do servidor para validade sempre deve ser executada antes de trabalhar com os dados.

Na etapa 1, criamos o `ProductsDataSource` ObjectDataSource de forma que seu método `Insert()` seja mapeado para o método `AddProduct` da classe `ProductsBLL`. Para inserir o novo registro na tabela `Products`, podemos simplesmente invocar o método ObjectDataSource s `Insert()`:

[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample7.cs)]

Agora que o método `Insert()` foi invocado, tudo o que resta é copiar os valores da interface de inserção para os parâmetros passados para o método `ProductsBLL` Class s `AddProduct`. Como vimos de volta no tutorial [examinando os eventos associados à inserção, atualização e exclusão](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md) , isso pode ser feito por meio do evento ObjectDataSource s `Inserting`. No evento `Inserting` precisamos fazer referência programaticamente aos controles da linha de rodapé `Products` GridView e atribuir seus valores à coleção de `e.InputParameters`. Se o usuário omitir um valor como, por exemplo, deixar a caixa de texto `ReorderLevel` em branco, precisaremos especificar que o valor inserido no banco de dados deve ser `NULL`. Como o método `AddProducts` aceita tipos anuláveis para os campos de banco de dados anuláveis, basta usar um tipo anulável e definir seu valor como `null` no caso em que a entrada do usuário for omitida.

[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample8.cs)]

Com o manipulador de eventos `Inserting` concluído, novos registros podem ser adicionados à tabela de banco de dados `Products` por meio da linha de rodapé GridView. Vá em frente e tente adicionar vários novos produtos.

## <a name="enhancing-and-customizing-the-add-operation"></a>Aprimorando e personalizando a operação de adição

Atualmente, clicar no botão Adicionar Adiciona um novo registro à tabela de banco de dados, mas não fornece nenhum tipo de comentário visual que o registro foi adicionado com êxito. O ideal é que um controle da Web de rótulo ou de alerta do lado do cliente informe ao usuário que sua inserção foi concluída com êxito. Eu deixe isso como um exercício para o leitor.

O GridView usado neste tutorial não aplica nenhuma ordem de classificação aos produtos listados nem permite que o usuário final Classifique os dados. Consequentemente, os registros são ordenados como estão no banco de dados pelo campo de chave primária. Como cada novo registro tem um valor `ProductID` maior do que o último, sempre que um novo produto é adicionado, ele é direcionado para o final da grade. Portanto, talvez você queira enviar automaticamente o usuário para a última página do GridView depois de adicionar um novo registro. Isso pode ser feito adicionando a seguinte linha de código após a chamada para `ProductsDataSource.Insert()` no manipulador de eventos `RowCommand` para indicar que o usuário precisa ser enviado para a última página depois de associar os dados ao GridView:

[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample9.cs)]

`SendUserToLastPage` é uma variável booliana de nível de página inicialmente atribuída a um valor de `false`. No manipulador de eventos GridView s `DataBound`, se `SendUserToLastPage` for false, a propriedade `PageIndex` será atualizada para enviar o usuário para a última página.

[!code-csharp[Main](inserting-a-new-record-from-the-gridview-s-footer-cs/samples/sample10.cs)]

O motivo pelo qual a propriedade `PageIndex` é definida no manipulador de eventos de `DataBound` (ao contrário do manipulador de eventos `RowCommand`) é porque quando o manipulador de eventos de `RowCommand` é acionado, ainda assim adicionamos o novo registro à tabela de banco de dados `Products`. Portanto, no manipulador de eventos `RowCommand` o índice da última página (`PageCount - 1`) representa o último índice de página *antes que* o novo produto tenha sido adicionado. Para a maioria dos produtos que estão sendo adicionados, o índice da última página é o mesmo após a adição do novo produto. Mas, quando o produto adicionado resultar em um novo índice de última página, se você atualizar incorretamente o `PageIndex` no manipulador de eventos `RowCommand`, será levado à segunda para a última página (o último índice de página antes de adicionar o novo produto), em oposição ao novo índice da última página. Como o manipulador de eventos `DataBound` é acionado após o novo produto ter sido adicionado e os dados reassociados à grade, definindo a propriedade `PageIndex` sabemos que obtemos o índice de última página correto.

Por fim, o GridView usado neste tutorial é bem grande devido ao número de campos que devem ser coletados para a adição de um novo produto. Devido a essa largura, um layout vertical de DetailsView pode ser preferido. A largura geral de GridView s poderia ser reduzida com a coleta de menos entradas. Talvez não seja necessário coletar os campos `UnitsOnOrder`, `UnitsInStock`e `ReorderLevel` ao adicionar um novo produto; nesse caso, esses campos podem ser removidos do GridView.

Para ajustar os dados coletados, podemos usar uma das duas abordagens:

- Continue a usar o método `AddProduct` que espera valores para os campos `UnitsOnOrder`, `UnitsInStock`e `ReorderLevel`. No manipulador de eventos `Inserting`, forneça valores padrão embutidos em código a serem usados para essas entradas que foram removidas da interface de inserção.
- Crie uma nova sobrecarga do método `AddProduct` na classe `ProductsBLL` que não aceita entradas para os campos `UnitsOnOrder`, `UnitsInStock`e `ReorderLevel`. Em seguida, na página ASP.NET, configure o ObjectDataSource para usar essa nova sobrecarga.

Qualquer opção também funcionará igualmente. Nos tutoriais anteriores, usamos a última opção, criando várias sobrecargas para o método `ProductsBLL` `UpdateProduct` da classe.

## <a name="summary"></a>Resumo

O GridView não tem os recursos internos de inserção encontrados em DetailsView e FormView, mas com um pouco de esforço, uma interface de inserção pode ser adicionada à linha de rodapé. Para exibir a linha de rodapé em um GridView, basta definir sua propriedade `ShowFooter` como `true`. O conteúdo da linha de rodapé pode ser personalizado para cada campo, convertendo o campo em um TemplateField e adicionando a interface de inserção ao `FooterTemplate`. Como vimos neste tutorial, o `FooterTemplate` pode conter botões, caixas de Text, DropDownLists, caixas de seleção, controles de fonte de dados para popular controles da Web controlados por dados (como DropDownLists) e controles de validação. Junto com controles para coletar a entrada de s do usuário, é necessário um botão Adicionar, LinkButton ou ImageButton.

Quando o botão Adicionar é clicado, o método ObjectDataSource s `Insert()` é invocado para iniciar o fluxo de trabalho de inserção. O ObjectDataSource Então chamará o método Insert configurado (o método de `AddProduct` da classe `ProductsBLL`, neste tutorial). Devemos copiar os valores da interface de inserção de GridView s para a coleção ObjectDataSource s `InsertParameters` antes do método Insert ser invocado. Isso pode ser feito fazendo referência programática aos controles da Web da interface de inserção no manipulador de eventos ObjectDataSource s `Inserting`.

Este tutorial conclui nossa visão técnica para aprimorar a aparência de GridView. O próximo conjunto de tutoriais examinará como trabalhar com dados binários, como imagens, PDFs, documentos do Word e assim por diante e os controles da Web de dados.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi a Bernadette Leigh. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](adding-a-gridview-column-of-checkboxes-cs.md)
> [Próximo](adding-a-gridview-column-of-radio-buttons-vb.md)
