---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: Adicionando controles de validação às interfaces de edição e inserção (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como é fácil adicionar controles de validação ao EditItemTemplate e InsertItemTemplate de um controle da Web de dados, para fornecer mais...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: 5c5ad110ee0836f0a464b02a2b29254e2e06381e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74571349"
---
# <a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Adicionar controles de validação às interfaces de edição e inserção (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) ou [baixar PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> Neste tutorial, veremos como é fácil adicionar controles de validação ao EditItemTemplate e InsertItemTemplate de um controle da Web de dados, para fornecer uma interface do usuário mais infalível.

## <a name="introduction"></a>Introdução

Os controles GridView e DetailsView nos exemplos que exploramos nos últimos três tutoriais têm sido compostos por BoundFields e CheckBoxFields (os tipos de campo adicionados automaticamente pelo Visual Studio ao associar um GridView ou DetailsView a uma fonte de dados controle através da marca inteligente). Ao editar uma linha em um GridView ou DetailsView, os BoundFields que não são somente leitura são convertidos em caixas de Text, das quais o usuário final pode modificar os dados existentes. Da mesma forma, ao inserir um novo registro em um controle DetailsView, os BoundFields cuja propriedade `InsertVisible` está definida como `True` (o padrão) são renderizados como caixas de textvazias, nas quais o usuário pode fornecer os valores de campo do novo registro. Da mesma forma, os CheckBoxFields, que estão desabilitados na interface padrão, somente leitura, são convertidos em caixas de seleção habilitadas nas interfaces de edição e inserção.

Embora as interfaces padrão de edição e inserção para o BoundField e o CheckBoxField possam ser úteis, a interface não tem qualquer tipo de validação. Se um usuário fizer um erro de entrada de dados, como omitir o campo `ProductName` ou inserir um valor inválido para `UnitsInStock` (como-50), uma exceção será gerada de dentro das profundidades da arquitetura do aplicativo. Embora essa exceção possa ser manipulada normalmente conforme demonstrado no [tutorial anterior](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md), o ideal é que a interface do usuário de edição ou inserção inclua controles de validação para impedir que um usuário insira esses dados inválidos em primeiro lugar.

Para fornecer uma interface personalizada de edição ou inserção, precisamos substituir o BoundField ou CheckBoxField por um TemplateField. TemplateFields, que foi o tópico de discussão no [uso de TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) e [Usando TemplateFields nos tutoriais de controle DetailsView](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) , pode consistir em vários modelos que definem interfaces separadas para Estados de linha diferentes. O `ItemTemplate` do TemplateField é usado ao renderizar campos somente leitura ou linhas nos controles DetailsView ou GridView, enquanto o `EditItemTemplate` e `InsertItemTemplate` indicam as interfaces a serem usadas para os modos de edição e inserção, respectivamente.

Neste tutorial, veremos como é fácil adicionar controles de validação à `EditItemTemplate` do TemplateField e `InsertItemTemplate` para fornecer uma interface do usuário mais infalível. Especificamente, este tutorial usa o exemplo criado no tutorial [examinando os eventos associados à inserção, atualização e exclusão](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) e aumenta as interfaces de edição e inserção para incluir a validação apropriada.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>Etapa 1: replicando o exemplo de[examinando os eventos associados à inserção, atualização e exclusão](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

No tutorial [examinando os eventos associados com a inserção, atualização e exclusão,](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) criamos uma página que listou os nomes e os preços dos produtos em um GridView editável. Além disso, a página incluía um DetailsView cuja propriedade `DefaultMode` foi definida como `Insert`, sendo assim, sempre renderizando no modo de inserção. A partir deste DetailsView, o usuário poderia inserir o nome e o preço de um novo produto, clicar em inserir e adicioná-lo ao sistema (consulte a Figura 1).

[![o exemplo anterior permite que os usuários adicionem novos produtos e editem os existentes](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Figura 1**: o exemplo anterior permite que os usuários adicionem novos produtos e editem os existentes ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))

Nosso objetivo deste tutorial é aumentar o DetailsView e o GridView para fornecer controles de validação. Em particular, nossa lógica de validação irá:

- Exigir que o nome seja fornecido ao inserir ou editar um produto
- Exigir que o preço seja fornecido ao inserir um registro; ao editar um registro, ainda precisaremos de um preço, mas usaremos a lógica programática no manipulador de eventos `RowUpdating` do GridView já presente no tutorial anterior
- Verifique se o valor inserido para o preço é um formato de moeda válido

Antes que possamos examinar o aumento do exemplo anterior para incluir a validação, primeiro precisamos replicar o exemplo da página `DataModificationEvents.aspx` para a página deste tutorial, `UIValidation.aspx`. Para fazer isso, precisamos copiar sobre a marcação declarativa da página `DataModificationEvents.aspx` e seu código-fonte. Primeiro copie sobre a marcação declarativa executando as seguintes etapas:

1. Abrir a página `DataModificationEvents.aspx` no Visual Studio
2. Vá para a marcação declarativa da página (clique no botão origem na parte inferior da página)
3. Copie o texto dentro das marcas `<asp:Content>` e `</asp:Content>` (linhas 3 a 44), conforme mostrado na Figura 2.

[![copiar o texto dentro do &lt;ASP: content&gt; Control](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**Figura 2**: copiar o texto dentro do controle de `<asp:Content>` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))

1. Abrir a página `UIValidation.aspx`
2. Ir para a marcação declarativa da página
3. Cole o texto dentro do controle de `<asp:Content>`.

Para copiar sobre o código-fonte, abra a página `DataModificationEvents.aspx.vb` e copie apenas o texto *dentro* da classe `EditInsertDelete_DataModificationEvents`. Copie os três manipuladores de eventos (`Page_Load`, `GridView1_RowUpdating`e `ObjectDataSource1_Inserting`), mas **não** Copie a declaração de classe. Cole o texto copiado *dentro* da classe `EditInsertDelete_UIValidation` em `UIValidation.aspx.vb`.

Depois de mover o conteúdo e o código de `DataModificationEvents.aspx` para `UIValidation.aspx`, Reserve um tempo para testar seu progresso em um navegador. Você deve ver a mesma saída e experimentar a mesma funcionalidade em cada uma dessas duas páginas (consulte novamente a Figura 1 para obter uma captura de tela de `DataModificationEvents.aspx` em ação).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Etapa 2: convertendo os BoundFields em TemplateFields

Para adicionar controles de validação às interfaces de edição e inserção, os BoundFields usados pelos controles DetailsView e GridView precisam ser convertidos em TemplateFields. Para conseguir isso, clique nos links Editar colunas e editar campos nas marcas inteligentes de GridView e DetailsView, respectivamente. Lá, selecione cada um dos BoundFields e clique no link "converter este campo em um TemplateField".

[![converter cada um dos BoundFields de DetailsView e de GridView em TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Figura 3**: converter cada uma das boundfields de DetailsView e de GridView em TemplateFields ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))

A conversão de um BoundField em um TemplateField por meio da caixa de diálogo campos gera um TemplateField que exibe as mesmas interfaces somente leitura, edição e inserção que o próprio BoundField. A marcação a seguir mostra a sintaxe declarativa para o campo `ProductName` no DetailsView depois que ele tiver sido convertido em um TemplateField:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Observe que esse TemplateField tinha três modelos criados automaticamente `ItemTemplate`, `EditItemTemplate`e `InsertItemTemplate`. O `ItemTemplate` exibe um único valor de campo de dados (`ProductName`) usando um controle de rótulo da Web, enquanto o `EditItemTemplate` e `InsertItemTemplate` apresentam o valor do campo de dados em um controle da Web de caixa de texto que associa o campo de dados com a propriedade `Text` da caixa de vinculação usando a associação bidirecional. Como estamos usando apenas o DetailsView nesta página para inserção, você pode remover o `ItemTemplate` e `EditItemTemplate` dos dois TemplateFields, embora não haja nenhum dano em deixá-los.

Como o GridView não dá suporte aos recursos internos de inserção do DetailsView, a conversão do campo `ProductName` do GridView em um TemplateField resulta em apenas um `ItemTemplate` e `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

Ao clicar no botão "converter este campo em um modelo", o Visual Studio criou um TemplateField cujos modelos imitam a interface do usuário do BoundField convertido. Você pode verificar isso visitando esta página por meio de um navegador. Você descobrirá que a aparência e o comportamento do TemplateFields são idênticos à experiência em que os BoundFields foram usados.

> [!NOTE]
> Sinta-se à vontade para personalizar as interfaces de edição nos modelos, conforme necessário. Por exemplo, talvez queiramos ter a caixa de texto na `UnitPrice` TemplateFields processada como uma caixa de texto menor do que a caixa de texto `ProductName`. Para fazer isso, você pode definir a propriedade `Columns` da caixa de texto como um valor apropriado ou fornecer uma largura absoluta por meio da propriedade `Width`. No próximo tutorial, veremos como personalizar completamente a interface de edição, substituindo a caixa de texto por um controle da Web de entrada de dados alternativo.

## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Etapa 3: adicionando os controles de validação para os`EditItemTemplate` s do GridView

Ao construir formulários de entrada de dados, é importante que os usuários insiram todos os campos obrigatórios e que todas as entradas fornecidas sejam valores válidos e formatados corretamente. Para ajudar a garantir que as entradas de um usuário sejam válidas, o ASP.NET fornece cinco controles de validação internos projetados para serem usados para validar o valor de um único controle de entrada:

- O [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) garante que um valor tenha sido fornecido
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) valida um valor em relação a outro valor de controle da Web ou um valor constante, ou garante que o formato do valor seja válido para um tipo de dados especificado
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garante que um valor esteja dentro de um intervalo de valores
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) valida um valor em relação a uma [expressão regular](http://en.wikipedia.org/wiki/Regular_expression)
- O [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) valida um valor em relação a um método personalizado definido pelo usuário

Para obter mais informações sobre esses cinco controles, confira a [seção de controles de validação](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) dos tutoriais de [início rápido do ASP.net](https://asp.net/QuickStart/aspnet/).

Para nosso tutorial, precisaremos usar um RequiredFieldValidator tanto no DetailsView quanto no GridView `ProductName` TemplateFields e em um RequiredFieldValidator no `UnitPrice` TemplateField de DetailsView. Além disso, precisaremos adicionar um CompareValidator aos dois controles, `UnitPrice` TemplateFields, que garante que o preço inserido tenha um valor maior ou igual a 0 e seja apresentado em um formato de moeda válido.

> [!NOTE]
> Embora ASP.NET 1. x tenha esses mesmos cinco controles de validação, o ASP.NET 2,0 adicionou vários aprimoramentos, os dois principais que são o suporte de script do lado do cliente para navegadores diferentes do Internet Explorer e a capacidade de particionar controles de validação em uma página em grupos de validação. Para obter mais informações sobre os novos recursos de controle de validação no 2,0, consulte disparando [os controles de validação no ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).

Vamos começar adicionando os controles de validação necessários para os `EditItemTemplate` s no TemplateFields do GridView. Para fazer isso, clique no link editar modelos da marca inteligente do GridView para abrir a interface de edição do modelo. A partir daqui, você pode selecionar qual modelo editar na lista suspensa. Como desejamos aumentar a interface de edição, precisamos adicionar controles de validação ao `ProductName` e `UnitPrice`s do `EditItemTemplate`.

[![precisamos estender os EditItemTemplates do NomeDoProduto e do UnitPrice](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Figura 4**: é necessário estender as `EditItemTemplate` s de `ProductName` e `UnitPrice`([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))

Na `EditItemTemplate``ProductName`, adicione um RequiredFieldValidator arrastando-o da caixa de ferramentas para a interface de edição de modelo, colocando após a caixa de texto.

[![adicionar um RequiredFieldValidator ao EditItemTemplate do NomeDoProduto](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Figura 5**: adicionar um RequiredFieldValidator ao `EditItemTemplate` de `ProductName` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))

Todos os controles de validação funcionam validando a entrada de um único controle da Web ASP.NET. Portanto, precisamos indicar que o RequiredFieldValidator que acabamos de adicionar deve ser validado em relação à caixa de texto na `EditItemTemplate`; Isso é feito definindo a [propriedade ControlToValidate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) do controle de validação como a `ID` do controle da Web apropriado. Atualmente, a caixa de texto tem a `ID` em vez nondescript de `TextBox1`, mas vamos alterá-la para algo mais apropriado. Clique na caixa de texto no modelo e, em seguida, na janela Propriedades, altere o `ID` de `TextBox1` para `EditProductName`.

[![alterar a ID da caixa de texto para EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Figura 6**: alterar a `ID` da caixa de texto para `EditProductName` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))

Em seguida, defina a propriedade `ControlToValidate` do RequiredFieldValidator como `EditProductName`. Por fim, defina a [Propriedade ErrorMessage](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) como "você deve fornecer o nome do produto" e a [propriedade Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) como "\*". O valor da propriedade `Text`, se fornecido, é o texto que será exibido pelo controle de validação se a validação falhar. O valor da propriedade `ErrorMessage`, que é necessário, é usado pelo controle ValidationSummary; Se o valor da propriedade `Text` for omitido, o valor da propriedade `ErrorMessage` também será o texto exibido pelo controle de validação em entrada inválida.

Depois de definir essas três propriedades do RequiredFieldValidator, sua tela deve ser semelhante à figura 7.

[![definir as propriedades ControlToValidate, ErrorMessage e Text do RequiredFieldValidator](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Figura 7**: definir as propriedades `ControlToValidate`, `ErrorMessage`e `Text` do RequiredFieldValidator ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))

Com o RequiredFieldValidator adicionado ao `ProductName` `EditItemTemplate`, tudo o que resta é adicionar a validação necessária ao `EditItemTemplate`de `UnitPrice`. Como decidimos que, para essa página, a `UnitPrice` é opcional ao editar um registro, não precisamos adicionar um RequiredFieldValidator. No entanto, precisamos adicionar um CompareValidator para garantir que o `UnitPrice`, se fornecido, esteja formatado corretamente como uma moeda e seja maior ou igual a 0.

Antes de adicionarmos o CompareValidator ao `EditItemTemplate`de `UnitPrice`, vamos primeiro alterar a ID do controle da Web de TextBox de `TextBox2` para `EditUnitPrice`. Depois de fazer essa alteração, adicione o CompareValidator, definindo sua propriedade `ControlToValidate` como `EditUnitPrice`, sua propriedade `ErrorMessage` como "o preço deve ser maior ou igual a zero e não pode incluir o símbolo de moeda" e sua propriedade `Text` como "\*".

Para indicar que o valor de `UnitPrice` deve ser maior ou igual a 0, defina a propriedade do [operador](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) CompareValidator como `GreaterThanEqual`, sua [Propriedade ValueToCompare](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) como "0" e sua [Propriedade Type](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) como `Currency`. A sintaxe declarativa a seguir mostra o `EditItemTemplate` de `UnitPrice` TemplateField depois que essas alterações foram feitas:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

Depois de fazer essas alterações, abra a página em um navegador. Se você tentar omitir o nome ou inserir um valor de preço inválido ao editar um produto, um asterisco será exibido ao lado da caixa de texto. Como mostra a Figura 8, um valor de preço que inclui o símbolo de moeda, como $19.95, é considerado inválido. O `Currency` do CompareValidator `Type` permite separadores de dígitos (como vírgulas ou pontos, dependendo das configurações de cultura) e um sinal de mais ou menos à esquerda, mas *não permite um* símbolo de moeda. Esse comportamento pode perplex os usuários, pois a interface de edição processa atualmente o `UnitPrice` usando o formato de moeda.

> [!NOTE]
> Lembre-se de que, nos *eventos associados à inserção, atualização e exclusão* do tutorial, definimos a propriedade de `DataFormatString` de BoundField como `{0:c}` para formatá-la como uma moeda. Além disso, definimos a propriedade `ApplyFormatInEditMode` como true, fazendo com que a interface de edição do GridView formate a `UnitPrice` como uma moeda. Ao converter o BoundField em um TemplateField, o Visual Studio observou essas configurações e formatou a propriedade `Text` da caixa de texto como uma moeda usando a sintaxe de DataBinding `<%# Bind("UnitPrice", "{0:c}") %>`.

[![um asterisco aparece ao lado das caixas de Textcom entrada inválida](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Figura 8**: um asterisco é exibido ao lado das caixas de Textcom entrada inválida ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))

Enquanto a validação funciona no estado em que se encontra, o usuário precisa remover manualmente o símbolo de moeda ao editar um registro, o que não é aceitável. Para corrigir isso, temos três opções:

1. Configure o `EditItemTemplate` para que o valor de `UnitPrice` não seja formatado como uma moeda.
2. Permita que o usuário insira um símbolo de moeda removendo o CompareValidator e substituindo-o por um RegularExpressionValidator que verifica corretamente um valor de moeda formatado corretamente. O problema aqui é que a expressão regular para validar um valor de moeda não é muito necessária e exigiria escrever código se quiséssemos incorporar configurações de cultura.
3. Remova totalmente o controle de validação e confie na lógica de validação do lado do servidor no manipulador de eventos `RowUpdating` do GridView.

Vamos usar a opção #1 para este exercício. Atualmente, a `UnitPrice` é formatada como uma moeda devido à expressão DataBinding para a caixa de texto na `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Altere a instrução BIND para `Bind("UnitPrice", "{0:n2}")`, que formata o resultado como um número com dois dígitos de precisão. Isso pode ser feito diretamente por meio da sintaxe declarativa ou clicando no link editar DataBindings da caixa de texto `EditUnitPrice` na `EditItemTemplate` do `UnitPrice` TemplateField (consulte as figuras 9 e 10).

[![clique no link editar DataBindings da caixa de texto](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Figura 9**: clique no link editar DataBindings da caixa de texto ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))

[![especificar o especificador de formato na instrução BIND](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Figura 10**: especificar o especificador de formato na instrução `Bind` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))

Com essa alteração, o preço formatado na interface de edição inclui vírgulas como separador de grupo e um ponto como separador decimal, mas deixa fora do símbolo de moeda.

> [!NOTE]
> O `EditItemTemplate` `UnitPrice` não inclui um RequiredFieldValidator, permitindo que o postback acontecer e a lógica de atualização seja iniciado. No entanto, o manipulador de eventos `RowUpdating` copiado do tutorial *examinando os eventos associados com inserção, atualização e exclusão* inclui uma verificação programática que garante que o `UnitPrice` seja fornecido. Sinta-se à vontade para remover essa lógica, deixá-la no estado em que se encontra ou adicionar um RequiredFieldValidator ao `UnitPrice` `EditItemTemplate`.

## <a name="step-4-summarizing-data-entry-problems"></a>Etapa 4: Resumindo problemas de entrada de dados

Além dos cinco controles de validação, ASP.NET inclui o [controle ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), que exibe os `ErrorMessage` s desses controles de validação que detectaram dados inválidos. Esses dados de resumo podem ser exibidos como texto na página da Web ou por meio de uma MessageBox restrita do lado do cliente. Vamos aprimorar este tutorial para incluir uma MessageBox do lado do cliente Resumindo quaisquer problemas de validação.

Para fazer isso, arraste um controle ValidationSummary da caixa de ferramentas para o designer. O local do controle de validação não importa muito, pois vamos configurá-lo para exibir apenas o resumo como uma MessageBox. Depois de adicionar o controle, defina sua [propriedade de resumo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) como `False` e sua propriedade de de a [MessageBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) como `True`. Com essa adição, todos os erros de validação são resumidos em uma MessageBox do lado do cliente.

[![os erros de validação são resumidos em uma MessageBox do lado do cliente](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Figura 11**: os erros de validação são resumidos em uma MessageBox do lado do cliente ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))

## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Etapa 5: adicionar os controles de validação ao`InsertItemTemplate` de DetailsView

Tudo o que resta para este tutorial é adicionar os controles de validação à interface de inserção de DetailsView. O processo de adicionar controles de validação aos modelos de DetailsView é idêntico ao examinado na etapa 3; Portanto, a Breeze será a tarefa nesta etapa. Como fizemos com os `EditItemTemplate` s do GridView, recomendo que você renomeie as `ID` s das caixas de Textdo `TextBox1` de nondescript e `TextBox2` para `InsertProductName` e `InsertUnitPrice`.

Adicione um RequiredFieldValidator ao `InsertItemTemplate`de `ProductName`. Defina o `ControlToValidate` para a `ID` da caixa de texto no modelo, sua propriedade `Text` como "\*" e sua propriedade `ErrorMessage` como "você deve fornecer o nome do produto".

Como o `UnitPrice` é necessário para esta página ao adicionar um novo registro, adicione um RequiredFieldValidator ao `InsertItemTemplate`de `UnitPrice`, definindo suas propriedades `ControlToValidate`, `Text`e `ErrorMessage` adequadamente. Por fim, adicione um CompareValidator à `UnitPrice` `InsertItemTemplate` também, configurando suas propriedades `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`e `ValueToCompare` exatamente como fizemos com o CompareValidator do `UnitPrice`no `EditItemTemplate`do GridView.

Depois de adicionar esses controles de validação, um novo produto não poderá ser adicionado ao sistema se seu nome não for fornecido ou se seu preço for um número negativo ou formatado ilegalmente.

[a lógica de validação de ![foi adicionada à interface de inserção de DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Figura 12**: a lógica de validação foi adicionada à interface de inserção de DetailsView ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))

## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Etapa 6: Particionando os controles de validação em grupos de validação

Nossa página consiste em dois conjuntos logicamente distintos de controles de validação: aqueles que correspondem à interface de edição do GridView e aos que correspondem à interface de inserção de DetailsView. Por padrão, quando ocorre um postback, *todos os* controles de validação na página são verificados. No entanto, ao editar um registro, não queremos que os controles de validação da interface de inserção de DetailsView sejam validados. A Figura 13 ilustra nosso dilema atual quando um usuário está editando um produto com valores perfeitamente legais, clicar em atualizar causa um erro de validação porque os valores de nome e preço na interface de inserção estão em branco.

[![a atualização de um produto faz com que os controles de validação da interface inserida sejam acionados](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Figura 13**: a atualização de um produto faz com que os controles de validação da interface de inserção sejam acionados ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))

Os controles de validação no ASP.NET 2,0 podem ser particionados em grupos de validação por meio de sua propriedade `ValidationGroup`. Para associar um conjunto de controles de validação em um grupo, basta definir sua propriedade `ValidationGroup` com o mesmo valor. Para nosso tutorial, defina as propriedades `ValidationGroup` dos controles de validação no TemplateFields do GridView como `EditValidationControls` e as propriedades `ValidationGroup` do TemplateFields do DetailsView como `InsertValidationControls`. Essas alterações podem ser feitas diretamente na marcação declarativa ou por meio do janela Propriedades ao usar a interface de edição de modelo do designer.

Além dos controles de validação, os controles relacionados ao botão e ao botão no ASP.NET 2,0 também incluem uma propriedade `ValidationGroup`. Os validadores de um grupo de validação são verificados quanto à validade somente quando um postback é induzido por um botão que tem a mesma configuração de propriedade de `ValidationGroup`. Por exemplo, para que o botão de inserção de DetailsView dispare o grupo de validação de `InsertValidationControls`, precisamos definir a propriedade `ValidationGroup` CommandField como `InsertValidationControls` (consulte a Figura 14). Além disso, defina a propriedade `ValidationGroup` de CommandField's do GridView como `EditValidationControls`.

[![definir a propriedade do grupo de validação CommandField's de DetailsView como InsertValidationControls](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Figura 14**: definir a propriedade de `ValidationGroup` CommandField's de DetailsView como `InsertValidationControls` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))

Após essas alterações, os TemplateFields e CommandFields de DetailsView e GridView devem ser semelhantes ao seguinte:

O TemplateFields e o comando do DetailsView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

CommandField e TemplateFields do GridView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

Neste ponto, os controles de validação específicos de edição são acionados somente quando o botão de atualização do GridView é clicado e os controles de validação específicos de inserção são acionados somente quando o botão de inserção de DetailsView é clicado, resolvendo o problema realçado pela figura 13. No entanto, com essa alteração, nosso controle ValidationSummary não é mais exibido ao inserir dados inválidos. O controle ValidationSummary também contém uma propriedade `ValidationGroup` e mostra apenas informações resumidas para esses controles de validação em seu grupo de validação. Portanto, precisamos ter dois controles de validação nesta página, um para o grupo de validação `InsertValidationControls` e outro para `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

Com essa adição, nosso tutorial está completo!

## <a name="summary"></a>Resumo

Embora os BoundFields possam fornecer uma interface de inserção e de edição, a interface não é personalizável. Normalmente, queremos adicionar controles de validação à interface de edição e inserção para garantir que o usuário insira entradas necessárias em um formato legal. Para fazer isso, devemos converter os BoundFields em TemplateFields e adicionar os controles de validação aos modelos apropriados. Neste tutorial, estendemos o exemplo do tutorial *examinando os eventos associados à inserção, atualização e exclusão* , adicionando controles de validação à interface de inserção do DetailsView e à interface de edição do GridView. Além disso, vimos como exibir informações de validação de resumo usando o controle ValidationSummary e como particionar os controles de validação na página em grupos de validação distintos.

Como vimos neste tutorial, TemplateFields permite que as interfaces de edição e inserção sejam aumentadas para incluir controles de validação. O TemplateFields também pode ser estendido para incluir controles da Web de entrada adicionais, permitindo que a caixa de texto seja substituída por um controle da Web mais adequado. Em nosso próximo tutorial, veremos como substituir o controle TextBox por um controle DropDownList associado a dados, que é ideal ao editar uma chave estrangeira (como `CategoryID` ou `SupplierID` na tabela `Products`).

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Liz Shulok e Zack Jones. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
> [Próximo](customizing-the-data-modification-interface-vb.md)
