---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
title: Usando TemplateFields no controle GridView (C#) | Microsoft Docs
author: rick-anderson
description: Para fornecer flexibilidade, o GridView oferece o TemplateField, que renderiza usando um modelo. Um modelo pode incluir uma mistura de HTML estático, controles da Web e...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 11de31e8-a78a-4f96-bd75-66e994175902
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ec17a16d7bb487d1c5cacf2d5971bbeffc1ba031
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74627763"
---
# <a name="using-templatefields-in-the-gridview-control-c"></a>Uso de TemplateFields no controle GridView (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_12_CS.exe) ou [baixar PDF](using-templatefields-in-the-gridview-control-cs/_static/datatutorial12cs1.pdf)

> Para fornecer flexibilidade, o GridView oferece o TemplateField, que renderiza usando um modelo. Um modelo pode incluir uma combinação de HTML estático, controles da Web e sintaxe de DataBinding. Neste tutorial, examinaremos como usar o TemplateField para obter um grau maior de personalização com o controle GridView.

## <a name="introduction"></a>Introdução

O GridView é composto de um conjunto de campos que indicam quais propriedades do `DataSource` devem ser incluídas na saída renderizada junto com o modo como os dados serão exibidos. O tipo de campo mais simples é o BoundField, que exibe um valor de dados como texto. Outros tipos de campo exibem os dados usando elementos HTML alternativos. O CheckBoxField, por exemplo, é renderizado como uma caixa de seleção cujo estado marcado depende do valor de um campo de dados especificado; o ImageField renderiza uma imagem cuja fonte de imagem é baseada em um campo de dados especificado. Os hiperlinks e botões cujo estado depende de um valor de campo de dados subjacente podem ser renderizados usando os tipos de campo HyperLinkField e ButtonField.

Embora os tipos de campo CheckBoxField, ImageField, HyperLinkField e ButtonField permitam uma exibição alternativa dos dados, eles ainda são bastante limitados em relação à formatação. Um CheckBoxField só pode exibir uma única caixa de seleção, enquanto um ImageField pode exibir apenas uma única imagem. E se um campo específico precisar exibir algum texto, uma caixa de seleção *e* uma imagem, tudo com base em valores de campos de dados diferentes? Ou e se quiséssemos exibir os dados usando um controle da Web diferente da caixa de seleção, imagem, hiperlink ou botão? Além disso, o BoundField limita sua exibição a um único campo de dados. E se quiséssemos mostrar dois ou mais valores de campo de dados em uma única coluna GridView?

Para acomodar esse nível de flexibilidade, o GridView oferece o TemplateField, que renderiza usando um *modelo*. Um modelo pode incluir uma combinação de HTML estático, controles da Web e sintaxe de DataBinding. Além disso, o TemplateField tem uma variedade de modelos que podem ser usados para personalizar a renderização para diferentes situações. Por exemplo, a `ItemTemplate` é usada por padrão para renderizar a célula para cada linha, mas o modelo de `EditItemTemplate` pode ser usado para personalizar a interface durante a edição de dados.

Neste tutorial, examinaremos como usar o TemplateField para obter um grau maior de personalização com o controle GridView. No [tutorial anterior](custom-formatting-based-upon-data-cs.md) , vimos como personalizar a formatação com base nos dados subjacentes usando os manipuladores de eventos `DataBound` e `RowDataBound`. Outra maneira de personalizar a formatação com base nos dados subjacentes é chamar métodos de formatação de dentro de um modelo. Também veremos essa técnica neste tutorial.

Para este tutorial, usaremos TemplateFields para personalizar a aparência de uma lista de funcionários. Especificamente, listaremos todos os funcionários, mas exibiremos o nome e o sobrenome do funcionário em uma coluna, sua data de contratação em um controle de calendário e uma coluna de status que indica quantos dias eles foram empregados na empresa.

[![três TemplateFields são usadas para personalizar a exibição](using-templatefields-in-the-gridview-control-cs/_static/image2.png)](using-templatefields-in-the-gridview-control-cs/_static/image1.png)

**Figura 1**: três TemplateFields são usadas para personalizar a exibição ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-cs/_static/image3.png))

## <a name="step-1-binding-the-data-to-the-gridview"></a>Etapa 1: associando os dados ao GridView

Em cenários de relatórios em que você precisa usar o TemplateFields para personalizar a aparência, acho mais fácil começar criando um controle GridView que contenha apenas BoundFields primeiro e, em seguida, adicionar novos TemplateFields ou converter os BoundFields existentes em TemplateFields conforme necessário. Portanto, vamos iniciar este tutorial adicionando um GridView à página por meio do designer e ligando-o a um ObjectDataSource que retorna a lista de funcionários. Essas etapas criarão um GridView com BoundFields para cada um dos campos Employee.

Abra a página `GridViewTemplateField.aspx` e arraste um GridView da caixa de ferramentas para o designer. Da marca inteligente do GridView, escolha Adicionar um novo controle ObjectDataSource que invoca o método `GetEmployees()` da classe `EmployeesBLL`.

[![adicionar um novo controle ObjectDataSource que invoca o método GetEmployees ()](using-templatefields-in-the-gridview-control-cs/_static/image5.png)](using-templatefields-in-the-gridview-control-cs/_static/image4.png)

**Figura 2**: adicionar um novo controle ObjectDataSource que invoca o método `GetEmployees()` ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-cs/_static/image6.png))

Associar o GridView dessa maneira adicionará automaticamente um BoundField para cada uma das propriedades Employee: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`e `Country`. Para esse relatório, não vamos nos preocupar com a exibição das propriedades `EmployeeID`, `ReportsTo`ou `Country`. Para remover esses BoundFields, você pode:

- Use a caixa de diálogo campos, clique no link Editar colunas da marca inteligente do GridView para abrir essa caixa de diálogo. Em seguida, selecione os BoundFields na lista inferior esquerda e clique no botão X vermelho para remover o BoundField.
- Edite a sintaxe declarativa do GridView manualmente na exibição da fonte, exclua o elemento `<asp:BoundField>` para o BoundField que você deseja remover.

Depois de remover os `EmployeeID`, `ReportsTo`e `Country` BoundFields, a marcação de GridView deve ser semelhante a:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample1.aspx)]

Reserve um tempo para exibir nosso progresso em um navegador. Neste ponto, você deve ver uma tabela com um registro para cada funcionário e quatro colunas: uma para o sobrenome do funcionário, uma para seu nome, uma para o título e outra para a data de contratação.

[![os campos LastName, FirstName, title e HireDate são exibidos para cada funcionário](using-templatefields-in-the-gridview-control-cs/_static/image8.png)](using-templatefields-in-the-gridview-control-cs/_static/image7.png)

**Figura 3**: os campos `LastName`, `FirstName`, `Title`e `HireDate` são exibidos para cada funcionário ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-cs/_static/image9.png))

## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Etapa 2: exibindo o nome e o sobrenome em uma única coluna

Atualmente, o nome e o sobrenome de cada funcionário são exibidos em uma coluna separada. Em vez disso, pode ser interessante combiná-los em uma única coluna. Para fazer isso, precisamos usar um TemplateField. Podemos adicionar um novo TemplateField, adicionar a ele a sintaxe necessária de marcação e vinculação de dados e, em seguida, excluir os `FirstName` e `LastName` BoundFields, ou podemos converter o `FirstName` BoundField em um TemplateField, editar o Modelofield para incluir o valor de `LastName` e, em seguida, remover o `LastName` BoundField.

Ambas as abordagens líquim o mesmo resultado, mas pessoalmente eu gosto de converter BoundFields em TemplateFields quando possível, pois a conversão adiciona automaticamente um `ItemTemplate` e `EditItemTemplate` com controles da Web e a sintaxe de DataBinding para imitar a aparência e a funcionalidade do BoundField. O benefício é que precisaremos fazer menos trabalho com o TemplateField, pois o processo de conversão terá realizado parte do trabalho para nós.

Para converter um BoundField existente em um TemplateField, clique no link Editar colunas na marca inteligente do GridView, trazendo a caixa de diálogo campos. Selecione o BoundField a ser convertido da lista no canto inferior esquerdo e, em seguida, clique no link "converter este campo em um TemplateField" no canto inferior direito.

[![converter um BoundField em um TemplateField da caixa de diálogo campos](using-templatefields-in-the-gridview-control-cs/_static/image11.png)](using-templatefields-in-the-gridview-control-cs/_static/image10.png)

**Figura 4**: converter um BoundField em um TemplateField da caixa de diálogo campos ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-cs/_static/image12.png))

Vá em frente e converta o `FirstName` BoundField em um TemplateField. Após essa alteração, não há nenhuma diferença de perreceptiva no designer. Isso ocorre porque a conversão de BoundField em um TemplateField cria um TemplateField que mantém a aparência do BoundField. Apesar de não haver nenhuma diferença visual neste ponto no designer, esse processo de conversão substituiu a sintaxe declarativa de BoundField-`<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />`-pela seguinte sintaxe de TemplateField:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample2.aspx)]

Como você pode ver, o TemplateField consiste em dois modelos um `ItemTemplate` que tem um rótulo cuja propriedade `Text` é definida como o valor do campo de dados `FirstName` e uma `EditItemTemplate` com um controle TextBox cuja propriedade `Text` também é definida como o campo de dados `FirstName`. A sintaxe de DataBinding-`<%# Bind("fieldName") %>`-indica que o *`fieldName`* de campo de dados está associado à propriedade de controle da Web especificada.

Para adicionar o valor do campo de dados `LastName` a este TemplateField, precisamos adicionar outro controle rótulo da Web no `ItemTemplate` e associar sua propriedade `Text` a `LastName`. Isso pode ser feito manualmente ou por meio do designer. Para fazer isso manualmente, basta adicionar a sintaxe declarativa apropriada ao `ItemTemplate`:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample3.aspx)]

Para adicioná-lo por meio do designer, clique no link editar modelos na marca inteligente do GridView. Isso exibirá a interface de edição de modelo do GridView. Na marca inteligente desta interface está uma lista dos modelos no GridView. Como temos apenas um TemplateField neste ponto, os únicos modelos listados na lista suspensa são os modelos para o `FirstName` TemplateField junto com o `EmptyDataTemplate` e `PagerTemplate`. O modelo de `EmptyDataTemplate`, se especificado, é usado para renderizar a saída do GridView se não houver nenhum resultado nos dados associados ao GridView; o `PagerTemplate`, se especificado, é usado para renderizar a interface de paginação para um GridView que dá suporte à paginação.

[![os modelos de GridView podem ser editados por meio do designer](using-templatefields-in-the-gridview-control-cs/_static/image14.png)](using-templatefields-in-the-gridview-control-cs/_static/image13.png)

**Figura 5**: os modelos do GridView podem ser editados por meio do designer ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-cs/_static/image15.png))

Para exibir também o `LastName` no `FirstName` TemplateField, arraste o controle rótulo da caixa de ferramentas para o `FirstName` TemplateField `ItemTemplate` na interface de edição de modelo do GridView.

[![adicionar um controle rótulo da Web ao ItemTemplate do nome do modelo do FirstName](using-templatefields-in-the-gridview-control-cs/_static/image17.png)](using-templatefields-in-the-gridview-control-cs/_static/image16.png)

**Figura 6**: adicionar um controle de rótulo da Web ao ItemTemplate do `FirstName` TemplateField ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-cs/_static/image18.png))

Neste ponto, o controle da Web Label adicionado ao TemplateField tem sua propriedade `Text` definida como "Label". Precisamos alterar isso para que essa propriedade esteja associada ao valor do campo de dados `LastName` em vez disso. Para fazer isso, clique na marca inteligente do controle rótulo e escolha a opção Editar DataBindings.

[![escolher a opção Editar DataBindings na marca inteligente do rótulo](using-templatefields-in-the-gridview-control-cs/_static/image20.png)](using-templatefields-in-the-gridview-control-cs/_static/image19.png)

**Figura 7**: escolha a opção Editar DataBindings na marca inteligente do rótulo ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-cs/_static/image21.png))

Isso abrirá a caixa de diálogo DataBindings. Aqui, você pode selecionar a propriedade para participar do DataBinding na lista à esquerda e escolher o campo para associar os dados da lista suspensa à direita. Escolha a propriedade `Text` à esquerda e o campo `LastName` da direita e clique em OK.

[![associar a propriedade Text ao campo de dados LastName](using-templatefields-in-the-gridview-control-cs/_static/image23.png)](using-templatefields-in-the-gridview-control-cs/_static/image22.png)

**Figura 8**: associar a propriedade `Text` ao campo de dados `LastName` ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-cs/_static/image24.png))

> [!NOTE]
> A caixa de diálogo DataBindings permite que você indique se a ligação de dados bidirecional deve ser executada. Se você deixar essa desmarcada, a sintaxe de DataBinding `<%# Eval("LastName")%>` será usada em vez de `<%# Bind("LastName")%>`. Qualquer uma das abordagens é adequada para este tutorial. A ligação de dados bidirecional torna-se importante durante a inserção e edição do dado. No entanto, para simplesmente exibir dados, qualquer abordagem funcionará igualmente bem. Discutiremos a ligação de dados de duas vias em detalhes em Tutoriais futuros.

Reserve um tempo para exibir esta página por meio de um navegador. Como você pode ver, o GridView ainda inclui quatro colunas; no entanto, a coluna `FirstName` *agora lista os* valores de campo de dados `FirstName` e `LastName`.

[![os valores FirstName e LastName são mostrados em uma única coluna](using-templatefields-in-the-gridview-control-cs/_static/image26.png)](using-templatefields-in-the-gridview-control-cs/_static/image25.png)

**Figura 9**: os valores de `FirstName` e de `LastName` são mostrados em uma única coluna ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-cs/_static/image27.png))

Para concluir essa primeira etapa, remova o `LastName` BoundField e renomeie a propriedade de `HeaderText` do `FirstName` TemplateField como "Name". Após essas alterações, a marcação declarativa do GridView deve ser semelhante ao seguinte:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample4.aspx)]

[![o nome e o sobrenome de cada funcionário são exibidos em uma coluna](using-templatefields-in-the-gridview-control-cs/_static/image29.png)](using-templatefields-in-the-gridview-control-cs/_static/image28.png)

**Figura 10**: o nome e o sobrenome de cada funcionário são exibidos em uma coluna ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-cs/_static/image30.png))

## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Etapa 3: usando o controle Calendar para exibir o campo`HiredDate`

Exibir um valor de campo de dados como texto em um GridView é tão simples quanto usar um BoundField. Para determinados cenários, no entanto, os dados são mais bem expressos usando um controle da Web específico em vez de apenas texto. Essa personalização da exibição de dados é possível com o TemplateFields. Por exemplo, em vez de exibir a data de contratação do funcionário como texto, poderíamos mostrar um calendário (usando [o controle Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) com a data de contratação realçada.

Para fazer isso, comece convertendo o `HiredDate` BoundField em um TemplateField. Basta ir para a marca inteligente do GridView e clicar no link Editar colunas, trazendo a caixa de diálogo campos. Selecione o `HiredDate` BoundField e clique em "converter este campo em um TemplateField".

[![converter o HiredDate BoundField em um TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image32.png)](using-templatefields-in-the-gridview-control-cs/_static/image31.png)

**Figura 11**: converter o `HiredDate` BoundField em um TemplateField ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-cs/_static/image33.png))

Como vimos na etapa 2, isso substituirá o BoundField por um TemplateField que contém um `ItemTemplate` e `EditItemTemplate` com um rótulo e uma caixa de texto cujas propriedades `Text` são vinculadas ao valor de `HiredDate` usando a sintaxe de DataBinding `<%# Bind("HiredDate")%>`.

Para substituir o texto por um controle de calendário, edite o modelo removendo o rótulo e adicionando um controle de calendário. No designer, selecione Editar modelos na marca inteligente do GridView e escolha o `ItemTemplate` de `HireDate` TemplateField na lista suspensa. Em seguida, exclua o controle rótulo e arraste um controle Calendar da caixa de ferramentas para a interface de edição de modelo.

[![adicionar um controle de calendário ao ItemTemplate de TemplateField contratado](using-templatefields-in-the-gridview-control-cs/_static/image35.png)](using-templatefields-in-the-gridview-control-cs/_static/image34.png)

**Figura 12**: adicionar um controle de calendário à `ItemTemplate` `HireDate` TemplateField ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-cs/_static/image36.png))

Neste ponto, cada linha no GridView conterá um controle Calendar em seu `HiredDate` TemplateField. No entanto, o valor real de `HiredDate` do funcionário não é definido em qualquer lugar no controle de calendário, fazendo com que cada controle de calendário tenha como padrão a exibição do mês e da data atuais. Para corrigir isso, precisamos atribuir a `HiredDate` de cada funcionário às propriedades [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) e [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) do controle de calendário.

Na marca inteligente do controle de calendário, escolha Editar DataBindings. Em seguida, associe as propriedades `SelectedDate` e `VisibleDate` ao campo de dados `HiredDate`.

[![associar as propriedades SelectedDate e VisibleDate ao campo de dados HiredDate](using-templatefields-in-the-gridview-control-cs/_static/image38.png)](using-templatefields-in-the-gridview-control-cs/_static/image37.png)

**Figura 13**: associar as propriedades `SelectedDate` e `VisibleDate` ao campo de dados `HiredDate` ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-cs/_static/image39.png))

> [!NOTE]
> A data selecionada do controle de calendário não precisa necessariamente ser visível. Por exemplo, um calendário pode<sup>ter 1º de agosto de 1999</sup>como a data selecionada, mas está mostrando o mês e o ano atuais. A data selecionada e a data visível são especificadas pelas propriedades `SelectedDate` e `VisibleDate` do controle de calendário. Como queremos selecionar o `HiredDate` do funcionário e verificar se ele é mostrado, precisamos associar essas duas propriedades ao campo de dados `HireDate`.

Ao exibir a página em um navegador, o calendário agora mostra o mês da data contratada do funcionário e seleciona essa data específica.

[![o HiredDate do funcionário é mostrado no controle de calendário](using-templatefields-in-the-gridview-control-cs/_static/image41.png)](using-templatefields-in-the-gridview-control-cs/_static/image40.png)

**Figura 14**: a `HiredDate` do funcionário é mostrada no controle de calendário ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-cs/_static/image42.png))

> [!NOTE]
> Ao contrário de todos os exemplos que vimos até agora, para este tutorial, *não* definimos `EnableViewState` propriedade como `false` para esse GridView. A razão para essa decisão é porque clicar nas datas do controle de calendário causa um postback, definindo a data selecionada do calendário como a data em que acabou de ser clicado. No entanto, se o estado de exibição do GridView estiver desabilitado, em cada postback, os dados do GridView serão reassociados à sua fonte de dados subjacente, o que faz com que a data selecionada do calendário seja configurada de *volta* para a `HireDate`do funcionário, substituindo a data escolhida pelo usuário.

Para este tutorial, essa é uma discussão sentido, pois o usuário não é capaz de atualizar o `HireDate`do funcionário. Provavelmente seria melhor configurar o controle de calendário para que suas datas não sejam selecionáveis. Independentemente disso, este tutorial mostra que, em algumas circunstâncias, o estado de exibição deve ser habilitado para fornecer determinadas funcionalidades.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Etapa 4: mostrando o número de dias que o funcionário trabalhou para a empresa

Até agora, vimos dois aplicativos de TemplateFields:

- Combinando dois ou mais valores de campo de dados em uma coluna e
- Expressar um valor de campo de dados usando um controle da Web em vez de texto

Um terceiro uso de TemplateFields está na exibição de metadados sobre os dados subjacentes do GridView. Além de mostrar as datas de contratação dos funcionários, por exemplo, talvez também queiramos ter uma coluna que exibe o número total de dias em que eles estavam no trabalho.

Mas outro uso de TemplateFields surge em cenários quando os dados subjacentes precisam ser exibidos de forma diferente no relatório de página da Web do que no formato armazenado no banco de dados. Imagine que a tabela `Employees` tinha um campo `Gender` que armazenou o `M` de caracteres ou `F` para indicar o sexo do funcionário. Ao exibir essas informações em uma página da Web, talvez você queira mostrar o gênero como "masculino" ou "feminino", em vez de apenas "M" ou "F".

Ambos os cenários podem ser tratados pela criação de um *método de formatação* na classe code-behind da página ASP.net (ou em uma biblioteca de classes separada, implementada como um método `static`) que é invocado a partir do modelo. Esse método de formatação é invocado a partir do modelo usando a mesma sintaxe de DataBinding vista anteriormente. O método de formatação pode ter qualquer número de parâmetros, mas deve retornar uma cadeia de caracteres. Essa cadeia de caracteres retornada é o HTML que é injetado no modelo.

Para ilustrar esse conceito, vamos ampliar nosso tutorial para mostrar uma coluna que lista o número total de dias que um funcionário esteve no trabalho. Esse método de formatação usará um objeto de `Northwind.EmployeesRow` e retornará o número de dias que o funcionário foi empregado como uma cadeia de caracteres. Esse método pode ser adicionado à classe code-behind da página ASP.NET, mas *deve* ser marcado como `protected` ou `public` para poder ser acessado do modelo.

[!code-csharp[Main](using-templatefields-in-the-gridview-control-cs/samples/sample5.cs)]

Como o campo `HiredDate` pode conter `NULL` valores de banco de dados, primeiro devemos garantir que o valor não seja `NULL` antes de prosseguir com o cálculo. Se o valor de `HiredDate` for `NULL`, simplesmente retornaremos a cadeia de caracteres "Unknown"; Se não estiver `NULL`, computaremos a diferença entre a hora atual e o valor de `HiredDate` e retornaremos o número de dias.

Para utilizar esse método, precisamos chamá-lo de um TemplateField no GridView usando a sintaxe DataBinding. Comece adicionando um novo TemplateField ao GridView clicando no link Editar colunas na marca inteligente do GridView e adicionando um novo TemplateField.

[![adicionar um novo Modelofield ao GridView](using-templatefields-in-the-gridview-control-cs/_static/image44.png)](using-templatefields-in-the-gridview-control-cs/_static/image43.png)

**Figura 15**: adicionar um novo modelofield ao GridView ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-cs/_static/image45.png))

Defina essa nova propriedade `HeaderText` de TemplateField como "dias no trabalho" e sua propriedade de `HorizontalAlign` de `ItemStyle`como `Center`. Para chamar o método `DisplayDaysOnJob` do modelo, adicione um `ItemTemplate` e use a seguinte sintaxe de DataBinding:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample6.aspx)]

`Container.DataItem` retorna um objeto `DataRowView` correspondente ao registro `DataSource` associado à `GridViewRow`. Sua propriedade `Row` retorna o `Northwind.EmployeesRow`fortemente tipado, que é passado para o método `DisplayDaysOnJob`. Essa sintaxe de ligação de dados pode aparecer diretamente no `ItemTemplate` (conforme mostrado na sintaxe declarativa abaixo) ou pode ser atribuída à propriedade `Text` de um controle de rótulo da Web.

> [!NOTE]
> Como alternativa, em vez de passar em uma instância de `EmployeesRow`, poderíamos apenas passar o valor de `HireDate` usando `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. No entanto, o método `Eval` retorna um `object`, portanto, teríamos que alterar nossa assinatura de método de `DisplayDaysOnJob` para aceitar um parâmetro de entrada do tipo `object`, em vez disso. Não podemos converter cegomente a chamada de `Eval("HireDate")` para um `DateTime` porque a coluna `HireDate` na tabela `Employees` pode conter valores de `NULL`. Portanto, seria necessário aceitar um `object` como o parâmetro de entrada para o método `DisplayDaysOnJob`, verificar se ele tinha um banco de dados `NULL` valor (que pode ser realizado usando `Convert.IsDBNull(objectToCheck)`) e prosseguir de acordo.

Devido a essas sutilezas, optei por passar toda a instância de `EmployeesRow`. No próximo tutorial, veremos um exemplo de ajuste mais para usar a sintaxe `Eval("columnName")` para passar um parâmetro de entrada para um método de formatação.

O seguinte mostra a sintaxe declarativa para o GridView após o TemplateField ter sido adicionado e o método `DisplayDaysOnJob` chamado da `ItemTemplate`:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample7.aspx)]

A Figura 16 mostra o tutorial concluído, quando exibido por meio de um navegador.

[![o número de dias que o funcionário esteve no trabalho é exibido](using-templatefields-in-the-gridview-control-cs/_static/image47.png)](using-templatefields-in-the-gridview-control-cs/_static/image46.png)

**Figura 16**: o número de dias que o funcionário esteve no trabalho é exibido ([clique para exibir a imagem em tamanho normal](using-templatefields-in-the-gridview-control-cs/_static/image48.png))

## <a name="summary"></a>Resumo

O TemplateField no controle GridView permite um grau mais alto de flexibilidade na exibição de dados do que está disponível com os outros controles de campo. TemplateFields são ideais para situações em que:

- Vários campos de dados precisam ser exibidos em uma coluna GridView
- Os dados são mais bem expressos usando um controle da Web em vez de texto sem formatação
- A saída depende dos dados subjacentes, como a exibição de metadados ou a reformatação dos dados

Além de personalizar a exibição de dados, os TemplateFields também são usados para personalizar as interfaces do usuário usadas para edição e inserção de dados, como veremos em Tutoriais futuros.

Os próximos dois tutoriais continuam explorando modelos, começando com uma olhada no uso de TemplateFields em um DetailsView. Depois disso, vamos voltar para o FormView, que usa modelos no lugar de campos para fornecer maior flexibilidade no layout e na estrutura dos dados.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Dan Jagers. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](custom-formatting-based-upon-data-cs.md)
> [Próximo](using-templatefields-in-the-detailsview-control-cs.md)
