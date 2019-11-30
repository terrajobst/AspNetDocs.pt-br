---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
title: Adicionando uma coluna GridView de botões de opçãoC#() | Microsoft Docs
author: rick-anderson
description: Este tutorial analisa como adicionar uma coluna de botões de opção a um controle GridView para fornecer ao usuário uma maneira mais intuitiva de selecionar uma única linha de...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 32377145-ec25-4715-8370-a1c590a331d5
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
msc.type: authoredcontent
ms.openlocfilehash: b59cc64b14c6414e6558fdb8a281644db8386701
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74593222"
---
# <a name="adding-a-gridview-column-of-radio-buttons-c"></a>Adicionar uma coluna de GridView de botões de opção (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_CS.exe) ou [baixar PDF](adding-a-gridview-column-of-radio-buttons-cs/_static/datatutorial51cs1.pdf)

> Este tutorial analisa como adicionar uma coluna de botões de opção a um controle GridView para fornecer ao usuário uma maneira mais intuitiva de selecionar uma única linha do GridView.

## <a name="introduction"></a>Introdução

O controle GridView oferece uma grande quantidade de funcionalidades internas. Ele inclui vários campos diferentes para exibir texto, imagens, hiperlinks e botões. Ele dá suporte a modelos para personalização adicional. Com alguns cliques do mouse, é possível tornar um GridView onde cada linha pode ser selecionada por meio de um botão ou para habilitar a edição ou a exclusão de recursos. Apesar da grande quantidade de recursos fornecidos, muitas vezes haverá situações em que recursos adicionais sem suporte precisarão ser adicionados. Neste tutorial e nos próximos dois, examinaremos como aprimorar a funcionalidade do GridView para incluir recursos adicionais.

Este tutorial e o próximo se concentram no aprimoramento do processo de seleção de linha. Conforme examinado no [mestre/detalhes usando um GridView mestre selecionável com um Details detailview](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md), podemos adicionar um CommandField ao GridView que inclui um botão de seleção. Quando clicado, um massacre de postback e a Propriedade GridView s `SelectedIndex` são atualizados para o índice da linha cujo botão Select foi clicado. No [mestre/detalhes usando um GridView mestre selecionável com um tutorial de detalhes detailview](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) , vimos como usar esse recurso para exibir detalhes da linha GridView selecionada.

Embora o botão Selecionar funcione em muitas situações, ele pode não funcionar bem para outras pessoas. Em vez de usar um botão, dois outros elementos da interface do usuário são comumente usados para seleção: o botão de opção e a caixa de seleção. Podemos aumentar o GridView para que, em vez de um botão Select, cada linha contenha um botão de opção ou uma caixa de seleção. Em cenários em que o usuário só pode selecionar um dos registros GridView, o botão de opção pode ser preferível sobre o botão Selecionar. Em situações em que o usuário pode potencialmente selecionar vários registros, como em um aplicativo de email baseado na Web, onde um usuário pode querer selecionar várias mensagens para excluir a caixa de seleção oferece a funcionalidade que não está disponível no botão Selecionar ou botão de opção interfaces do usuário.

Este tutorial analisa como adicionar uma coluna de botões de opção ao GridView. O tutorial de continuação explora o uso de caixas de seleção.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Etapa 1: criando o aprimoramento das páginas da Web GridView

Antes de começarmos a aprimorar o GridView para incluir uma coluna de botões de opção, vamos primeiro reservar um momento para criar as páginas ASP.NET em nosso projeto de site, que precisaremos para este tutorial e os dois seguintes. Comece adicionando uma nova pasta chamada `EnhancedGridView`. Em seguida, adicione as seguintes páginas ASP.NET a essa pasta, assegurando a associação de cada página com a página mestra `Site.master`:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`

![Adicionar as páginas ASP.NET para os tutoriais relacionados ao SqlDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.gif)

**Figura 1**: adicionar as páginas ASP.net para os tutoriais relacionados ao SqlDataSource

Assim como nas outras pastas, `Default.aspx` na pasta `EnhancedGridView` listará os tutoriais em sua seção. Lembre-se de que o controle de usuário `SectionLevelTutorialListing.ascx` fornece essa funcionalidade. Portanto, adicione esse controle de usuário a `Default.aspx` arrastando-o da Gerenciador de Soluções para a página s modo de exibição de Design.

[![adicionar o controle de usuário SectionLevelTutorialListing. ascx a default. aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.png)

**Figura 2**: Adicionar o controle de usuário `SectionLevelTutorialListing.ascx` ao `Default.aspx` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.png))

Por fim, adicione essas quatro páginas como entradas ao arquivo de `Web.sitemap`. Especificamente, adicione a seguinte marcação após o uso do controle SqlDataSource `<siteMapNode>`:

[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample1.xml)]

Depois de atualizar `Web.sitemap`, Reserve um momento para exibir o site de tutoriais por meio de um navegador. O menu à esquerda agora inclui itens para os tutoriais de edição, inserção e exclusão.

![O mapa do site agora inclui entradas para aprimorar os tutoriais do GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.gif)

**Figura 3**: o mapa do site agora inclui entradas para aprimorar os tutoriais do GridView

## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Etapa 2: exibindo os fornecedores em um GridView

Para este tutorial, vamos criar um GridView que liste os fornecedores dos EUA, com cada linha GridView fornecendo um botão de opção. Depois de selecionar um fornecedor por meio do botão de opção, o usuário pode exibir os produtos do fornecedor clicando em um botão. Embora essa tarefa possa parecer trivial, há várias sutilezas que o tornam particularmente complicadas. Antes de mergulharmos nessas sutilezas, vamos primeiro obter um GridView listando os fornecedores.

Comece abrindo a página `RadioButtonField.aspx` na pasta `EnhancedGridView` arrastando um GridView da caixa de ferramentas para o designer. Defina as `ID` s GridView como `Suppliers` e, em sua marca inteligente, escolha criar uma nova fonte de dados. Especificamente, crie um ObjectDataSource chamado `SuppliersDataSource` que extrai seus dados do objeto `SuppliersBLL`.

[![criar um novo ObjectDataSource chamado SuppliersDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.png)

**Figura 4**: criar um novo ObjectDataSource chamado `SuppliersDataSource` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.png))

[![configurar o ObjectDataSource para usar a classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.png)

**Figura 5**: configurar o ObjectDataSource para usar a classe `SuppliersBLL` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.png))

Como queremos apenas listar esses fornecedores nos EUA, escolha o método `GetSuppliersByCountry(country)` na lista suspensa na guia selecionar.

[![configurar o ObjectDataSource para usar a classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.png)

**Figura 6**: configurar o ObjectDataSource para usar a classe `SuppliersBLL` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.png))

Na guia atualizar, selecione a opção (nenhum) e clique em Avançar.

[![configurar o ObjectDataSource para usar a classe SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.png)

**Figura 7**: configurar o ObjectDataSource para usar a classe `SuppliersBLL` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.png))

Como o método `GetSuppliersByCountry(country)` aceita um parâmetro, o assistente para configurar fonte de dados nos solicita a origem desse parâmetro. Para especificar um valor embutido em código (EUA, neste exemplo), deixe a lista suspensa origem do parâmetro definida como nenhum e insira o valor padrão na caixa de texto. Clique em Concluir para concluir o assistente.

[![usar os EUA como o valor padrão para o parâmetro Country](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.png)

**Figura 8**: Use os EUA como o valor padrão para o parâmetro `country` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.png))

Depois de concluir o assistente, o GridView incluirá um BoundField para cada um dos campos de dados do fornecedor. Remova tudo, exceto o `CompanyName`, `City`e `Country` BoundFields e renomeie a propriedade `CompanyName` BoundFields `HeaderText` para o fornecedor. Depois de fazer isso, a sintaxe declarativa de GridView e ObjectDataSource deve ser semelhante à seguinte.

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample2.aspx)]

Para este tutorial, vamos permitir que o usuário exiba os produtos de fornecedor s selecionados na mesma página que a lista de fornecedores ou em uma página diferente. Para acomodar isso, adicione dois controles de botão da Web à página. Eu vou definir o `ID` s desses dois botões como `ListProducts` e `SendToProducts`, com a ideia de que, quando `ListProducts` for clicado, um postback ocorrerá e os produtos do fornecedor s selecionados serão listados na mesma página, mas quando `SendToProducts` for clicado, o usuário será encaixado em uma outra página que lista os produtos.

A Figura 9 mostra os controles `Suppliers` GridView e os dois botões da Web quando exibidos por meio de um navegador.

[![os fornecedores dos EUA têm suas informações de nome, cidade e país listadas](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.png)

**Figura 9**: os fornecedores dos EUA têm suas informações de nome, cidade e país listadas ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.png))

## <a name="step-3-adding-a-column-of-radio-buttons"></a>Etapa 3: adicionando uma coluna de botões de opção

Neste ponto, o `Suppliers` GridView tem três BoundFields exibindo o nome da empresa, a cidade e o país de cada fornecedor nos EUA. No entanto, ainda falta uma coluna de botões de opção. Infelizmente, o GridView não inclui um RadioButtonfield interno. caso contrário, poderíamos apenas adicioná-lo à grade e ser feito. Em vez disso, podemos adicionar um TemplateField e configurar seu `ItemTemplate` para processar um botão de opção, resultando em um botão de opção para cada linha GridView.

Inicialmente, podemos pressupor que a interface do usuário desejada possa ser implementada adicionando um controle da Web RadioButton à `ItemTemplate` de um TemplateField. Embora isso realmente adicione um único botão de opção a cada linha do GridView, os botões de opção não podem ser agrupados e, portanto, não são mutuamente exclusivos. Ou seja, um usuário final é capaz de selecionar vários botões de opção simultaneamente no GridView.

Embora o uso de um TemplateField dos controles da Web RadioButton não ofereça a funcionalidade que precisamos, vamos implementar essa abordagem, já que vale a pena examinar por que os botões de opção resultantes não são agrupados. Comece adicionando um TemplateField ao GridView dos fornecedores, tornando-o o campo mais à esquerda. Em seguida, na marca inteligente do GridView, clique no link editar modelos e arraste um controle da Web de RadioButton da caixa de ferramentas para o `ItemTemplate` do TemplateField (consulte a Figura 10). Defina o botão de opção s `ID` Propriedade como `RowSelector` e a propriedade `GroupName` como `SuppliersGroup`.

[![adicionar um controle da Web RadioButton ao ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.png)

**Figura 10**: adicionar um controle da Web RadioButton à `ItemTemplate` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.png))

Depois de fazer essas adições por meio do designer, sua marcação de s de GridView deve ser semelhante ao seguinte:

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample3.aspx)]

O RadioButton s [`GroupName` Propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) é o que é usado para agrupar uma série de botões de opção. Todos os controles RadioButton com o mesmo valor `GroupName` são considerados agrupados; somente um botão de opção pode ser selecionado de um grupo por vez. A propriedade `GroupName` especifica o valor do atributo do botão de opção renderizado s `name`. O navegador examina os botões de opção `name` atributos para determinar os agrupamentos de botão de opção.

Com o controle da Web do botão de opção adicionado ao `ItemTemplate`, visite esta página por meio de um navegador e clique nos botões de opção nas linhas da grade. Observe como os botões de opção não são agrupados, possibilitando a seleção de todas as linhas, como mostra a Figura 11.

[![os botões de opção GridView s não estão agrupados](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.png)

**Figura 11**: os botões de opção GridView s não são agrupados ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.png))

O motivo pelo qual os botões de opção não são agrupados é porque seus atributos de `name` renderizados são diferentes, apesar de terem a mesma configuração de propriedade `GroupName`. Para ver essas diferenças, faça uma exibição/origem no navegador e examine a marcação do botão de opção:

[!code-html[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample4.html)]

Observe como os atributos `name` e `id` não são os valores exatos especificados no janela Propriedades, mas são anexados a um número de outros valores de `ID`. Os valores de `ID` adicionais adicionados à frente dos atributos renderizados `id` e `name` são os `ID` s dos botões de opção pai controlam os `GridViewRow` s `ID` s, o `ID`GridView, o `ID`de controle de conteúdo e o s `ID`de formulário da Web. Esses `ID` s são adicionados para que cada controle da Web renderizado no GridView tenha um `id` exclusivo e valores `name`.

Cada controle renderizado precisa de um `name` e `id` diferentes porque é assim que o navegador identifica exclusivamente cada controle no lado do cliente e como ele identifica ao servidor Web qual ação ou alteração ocorreu no postback. Por exemplo, imagine que queríamos executar algum código do lado do servidor sempre que o estado selecionado do RadioButton s fosse alterado. Poderíamos fazer isso definindo o RadioButton s `AutoPostBack` Propriedade como `true` e criando um manipulador de eventos para o evento `CheckChanged`. No entanto, se os valores de `name` e `id` renderizados para todos os botões de opção forem os mesmos, no postback, não foi possível determinar o botão de opção específico que foi clicado.

A curta delas é que não podemos criar uma coluna de botões de opção em um GridView usando o controle da Web RadioButton. Em vez disso, devemos usar técnicas em vez de arcaico para garantir que a marcação apropriada seja injetada em cada linha GridView.

> [!NOTE]
> Assim como o controle da Web RadioButton, o controle HTML do botão de opção, quando adicionado a um modelo, incluirá o atributo `name` exclusivo, tornando os botões de opção na grade desagrupados. Se você não estiver familiarizado com controles HTML, fique à vontade para desconsiderar essa observação, pois os controles HTML raramente são usados, especialmente no ASP.NET 2,0. Mas, se você estiver interessado em saber mais, consulte os [controles da Web de entrada de](http://www.odetocode.com/Articles/348.aspx)blog [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) s e controles HTML.

## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Usando um controle literal para injetar a marcação do botão de opção

Para agrupar corretamente todos os botões de opção no GridView, precisamos injetar manualmente a marcação dos botões de opção na `ItemTemplate`. Cada botão de opção precisa do mesmo atributo `name`, mas deve ter um atributo de `id` exclusivo (caso queiramos acessar um botão de opção por meio do script do lado do cliente). Depois que um usuário selecionar um botão de opção e postar novamente a página, o navegador enviará de volta o valor do atributo selecionado do botão de opção s `value`. Portanto, cada botão de opção precisará de um atributo de `value` exclusivo. Por fim, no postback, precisamos adicionar o atributo `checked` ao botão de opção selecionado, caso contrário, depois que o usuário fizer uma seleção e fizer postback, os botões de opção retornarão ao seu estado padrão (todos não selecionados).

Há duas abordagens que podem ser tomadas para injetar a marcação de nível baixo em um modelo. Uma é fazer uma mistura de marcação e chamadas para métodos de formatação definidos na classe code-behind. Essa técnica foi discutida pela primeira vez no tutorial [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) . Em nosso caso, ele pode ser semelhante a:

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample5.aspx)]

Aqui, `GetUniqueRadioButton` e `GetRadioButtonValue` seriam métodos definidos na classe code-behind que retornavam os `id` apropriados e `value` valores de atributo para cada botão de opção. Essa abordagem funciona bem para atribuir os atributos `id` e `value`, mas é curto quando precisa especificar o valor do atributo `checked` porque a sintaxe de DataBinding só é executada quando os dados são vinculados pela primeira vez ao GridView. Portanto, se GridView tiver o estado de exibição habilitado, os métodos de formatação só serão acionados quando a página for carregada pela primeira vez (ou quando GridView for reassociado explicitamente à fonte de dados) e, portanto, a função que define o atributo `checked` não será chamada no postback. É um problema bem sutil e um pouco além do escopo deste artigo, então vou deixá-lo. No entanto, eu recomendo que você tente usar a abordagem acima e trabalhe até o ponto onde você ficará preso. Embora esse exercício não fique mais perto de uma versão de trabalho, ele ajudará a promover uma compreensão mais profunda do controle de tempo de vida de GridView e de DataBinding.

A outra abordagem para injetar marcações personalizadas e de nível baixo em um modelo e a abordagem que usaremos para este tutorial é adicionar um [controle literal](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) ao modelo. Em seguida, no `RowCreated` ou no manipulador de eventos de `RowDataBound` GridView, o controle literal pode ser acessado programaticamente e sua propriedade `Text` definida como a marcação a ser emitida.

Comece removendo o botão de opção dos `ItemTemplate`TemplateField, substituindo-o por um controle literal. Defina o controle literal s `ID` como `RadioButtonMarkup`.

[![adicionar um controle literal para o ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.png)

**Figura 12**: adicionar um controle Literal à `ItemTemplate` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.png))

Em seguida, crie um manipulador de eventos para o evento GridView s `RowCreated`. O evento `RowCreated` é acionado uma vez para cada linha adicionada, independentemente de os dados estarem sendo reassociados ao GridView. Isso significa que mesmo em um postback quando os dados são recarregados do estado de exibição, o evento `RowCreated` ainda é acionado e esse é o motivo pelo qual estamos usando em vez de `RowDataBound` (que é acionado somente quando os dados estão explicitamente ligados ao controle da Web de dados).

Nesse manipulador de eventos, só queremos continuar se estivermos lidando com uma linha de dados. Para cada linha de dados, queremos referenciar programaticamente o controle literal `RadioButtonMarkup` e definir sua propriedade `Text` como a marcação a ser emitida. Como mostra o código a seguir, a marcação emitida cria um botão de opção cujo atributo `name` está definido como `SuppliersGroup`, cujo atributo `id` está definido como `RowSelectorX`, em que *X* é o índice da linha GridView, e cujo atributo `value` está definido como o índice da linha GridView.

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample6.cs)]

Quando uma linha GridView é selecionada e um postback ocorre, estamos interessados na `SupplierID` do fornecedor selecionado. Portanto, você pode imaginar que o valor de cada botão de opção deve ser o `SupplierID` real (em vez do índice da linha GridView). Embora isso possa funcionar em determinadas circunstâncias, seria um risco de segurança para aceitar e processar indistintamente um `SupplierID`. Nosso GridView, por exemplo, lista apenas os fornecedores dos EUA. No entanto, se o `SupplierID` for passado diretamente do botão de opção, quais s interromper um usuário gênio de manipular o valor de `SupplierID` enviado de volta no postback? Usando o índice de linha como o `value`e, em seguida, obtendo o `SupplierID` no postback da coleção de `DataKeys`, podemos garantir que o usuário use apenas um dos valores `SupplierID` associados a uma das linhas GridView.

Depois de adicionar esse código do manipulador de eventos, Reserve um minuto para testar a página em um navegador. Primeiro, observe que apenas um botão de opção na grade pode ser selecionado de cada vez. No entanto, ao selecionar um botão de opção e clicar em um dos botões, um postback ocorre e os botões de opção são revertidos para o estado inicial (ou seja, no postback, o botão de opção selecionado não está mais selecionado). Para corrigir isso, precisamos aumentar o `RowCreated` manipulador de eventos para que ele Inspecione o índice do botão de opção selecionado enviado do postback e adicione o atributo `checked="checked"` à marcação emitida das correspondências de índice de linha.

Quando ocorre um postback, o navegador envia de volta a `name` e `value` do botão de opção selecionado. O valor pode ser recuperado programaticamente usando `Request.Form["name"]`. A [propriedade`Request.Form`](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) fornece uma [`NameValueCollection`](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) que representa as variáveis de formulário. As variáveis de formulário são os nomes e valores dos campos de formulário na página da Web e são enviadas de volta pelo navegador da Web sempre que um massacre de postback. Como o atributo `name` processado dos botões de opção no GridView é `SuppliersGroup`, quando a página da Web é postada, o navegador enviará `SuppliersGroup=valueOfSelectedRadioButton` de volta ao servidor Web (juntamente com os outros campos de formulário). Essas informações podem então ser acessadas da propriedade `Request.Form` usando: `Request.Form["SuppliersGroup"]`.

Como precisaremos determinar o índice do botão de opção selecionado não apenas no manipulador de eventos `RowCreated`, mas nos manipuladores de eventos `Click` para o botão controles da Web, vamos adicionar uma propriedade `SuppliersSelectedIndex` à classe code-behind que retorna `-1` se nenhum botão de opção foi selecionado e o índice selecionado se um dos botões de opção estiver selecionado.

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample7.cs)]

Com essa propriedade adicionada, sabemos adicionar a marcação de `checked="checked"` no manipulador de eventos `RowCreated` quando `SuppliersSelectedIndex` é igual a `e.Row.RowIndex`. Atualize o manipulador de eventos para incluir essa lógica:

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample8.cs)]

Com essa alteração, o botão de opção selecionado permanece selecionado após um postback. Agora que temos a capacidade de especificar qual botão de opção está selecionado, poderíamos alterar o comportamento para que, quando a página foi visitada pela primeira vez, o primeiro botão de opção s de linha GridView fosse selecionado (em vez de não ter nenhum botão de opção selecionado por padrão, que é o atual comportamento). Para que o primeiro botão de opção seja selecionado por padrão, basta alterar a instrução `if (SuppliersSelectedIndex == e.Row.RowIndex)` para o seguinte: `if (SuppliersSelectedIndex == e.Row.RowIndex || (!Page.IsPostBack && e.Row.RowIndex == 0))`.

Neste ponto, adicionamos uma coluna de botões de opção agrupados ao GridView que permite que uma única linha GridView seja selecionada e lembrada entre postbacks. Nossas próximas etapas são exibir os produtos fornecidos pelo fornecedor selecionado. Na etapa 4, veremos como redirecionar o usuário para outra página, enviando ao longo do `SupplierID`selecionado. Na etapa 5, veremos como exibir os produtos do fornecedor s selecionados em um GridView na mesma página.

> [!NOTE]
> Em vez de usar um TemplateField (o foco dessa etapa 3), poderíamos criar uma classe de `DataControlField` personalizada que processa a interface do usuário e a funcionalidade apropriadas. A [classe`DataControlField`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) é a classe base da qual o BoundField, CheckBoxField, TemplateField e outros campos GridView e DetailsView internos derivam. Criar uma classe de `DataControlField` personalizada significaria que a coluna de botões de opção poderia ser adicionada apenas usando a sintaxe declarativa e também tornaria a replicação da funcionalidade em outras páginas da Web e de outros aplicativos Web significativamente mais fácil.

Se você já criou controles personalizados e compilados em ASP.NET, no entanto, sabe que fazer isso requer uma quantia justa de questões e traz a ele um host de sutilezas e casos de borda que devem ser tratados cuidadosamente. Portanto, vamos abrem mão implementar uma coluna de botões de opção como uma classe de `DataControlField` personalizada para agora e aderir com a opção TemplateField. Talvez tenhamos a chance de explorar a criação, o uso e a implantação de classes de `DataControlField` personalizadas em um tutorial futuro!

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Etapa 4: exibindo os produtos de fornecedor s selecionados em uma página separada

Depois que o usuário tiver selecionado uma linha GridView, precisaremos mostrar os produtos de fornecedor s selecionados. Em algumas circunstâncias, talvez queiramos exibir esses produtos em uma página separada, em outros, podemos preferir fazer isso na mesma página. Primeiro, vamos examinar como exibir os produtos em uma página separada; na etapa 5, veremos como adicionar um GridView a `RadioButtonField.aspx` para exibir os produtos de fornecedores selecionados.

Atualmente, há dois controles Web Button na página `ListProducts` e `SendToProducts`. Quando o botão de `SendToProducts` é clicado, queremos enviar o usuário para `~/Filtering/ProductsForSupplierDetails.aspx`. Esta página foi criada no tutorial [filtragem mestre/detalhes em duas páginas](../masterdetail/master-detail-filtering-across-two-pages-cs.md) e exibe os produtos para o fornecedor cujo `SupplierID` é passado pelo campo querystring chamado `SupplierID`.

Para fornecer essa funcionalidade, crie um manipulador de eventos para o `SendToProducts` botão `Click` evento. Na etapa 3, adicionamos a propriedade `SuppliersSelectedIndex`, que retorna o índice da linha cujo botão de opção está selecionado. Os `SupplierID` correspondentes podem ser recuperados da coleção GridView s `DataKeys` e o usuário pode ser enviado para `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` usando `Response.Redirect("url")`.

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample9.cs)]

Esse código funciona de forma maravilhosa, contanto que um dos botões de opção seja selecionado no GridView. Se, inicialmente, o GridView não tiver nenhum botão de opção selecionado e o usuário clicar no botão `SendToProducts`, `SuppliersSelectedIndex` será `-1`, o que fará com que uma exceção seja gerada, pois `-1` está fora do intervalo de índice da coleção de `DataKeys`. No entanto, isso não é uma preocupação, se você decidiu atualizar o manipulador de eventos `RowCreated` conforme discutido na etapa 3 para que o primeiro botão de opção no GridView seja selecionado inicialmente.

Para acomodar um `SuppliersSelectedIndex` valor de `-1`, adicione um controle rótulo da Web à página acima do GridView. Defina sua propriedade `ID` como `ChooseSupplierMsg`, sua propriedade `CssClass` como `Warning`, suas propriedades `EnableViewState` e `Visible` como `false`e sua propriedade `Text` para escolher um fornecedor da grade. A classe CSS `Warning` exibe texto em uma fonte vermelha, itálico, negrito e grande e é definida em `Styles.css`. Ao definir as propriedades `EnableViewState` e `Visible` como `false`, o rótulo não é renderizado, exceto apenas para os postbacks em que a propriedade `Visible` do controle é definida de forma programática como `true`.

[![adicionar um controle de rótulo da Web acima do GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image21.png)

**Figura 13**: adicionar um controle de rótulo da Web acima do GridView ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image22.png))

Em seguida, aumente o manipulador de eventos `Click` para exibir o rótulo de `ChooseSupplierMsg` se `SuppliersSelectedIndex` for menor que zero e redirecionar o usuário para `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` de outra forma.

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample10.cs)]

Visite a página em um navegador e clique no botão `SendToProducts` antes de selecionar um fornecedor do GridView. Como mostra a Figura 14, isso exibe o rótulo `ChooseSupplierMsg`. Em seguida, selecione um fornecedor e clique no botão `SendToProducts`. Isso irá encaixar você em uma página que lista os produtos fornecidos pelo fornecedor selecionado. A Figura 15 mostra a página `ProductsForSupplierDetails.aspx` quando o fornecedor Bigfoot Breweries foi selecionado.

[![o rótulo ChooseSupplierMsg será exibido se nenhum fornecedor for selecionado](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image23.png)

**Figura 14**: o rótulo de `ChooseSupplierMsg` será exibido se nenhum fornecedor for selecionado ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image24.png))

[![os produtos de fornecedor s selecionados são exibidos em ProductsForSupplierDetails. aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image25.png)

**Figura 15**: os produtos de fornecedor s selecionados são exibidos no `ProductsForSupplierDetails.aspx` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image26.png))

## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Etapa 5: exibindo os produtos do fornecedor s selecionados na mesma página

Na etapa 4, vimos como enviar o usuário para outra página da Web para exibir os produtos do fornecedor selecionado. Como alternativa, os produtos de fornecedor s selecionados podem ser exibidos na mesma página. Para ilustrar isso, adicionaremos outro GridView a `RadioButtonField.aspx` para exibir os produtos de fornecedores selecionados.

Como queremos apenas que este GridView de produtos seja exibido quando um fornecedor tiver sido selecionado, adicione um controle Web Panel abaixo do `Suppliers` GridView, definindo sua `ID` como `ProductsBySupplierPanel` e sua propriedade `Visible` como `false`. No painel, adicione os produtos de texto para o fornecedor selecionado, seguido por um GridView chamado `ProductsBySupplier`. Na marca inteligente de GridView, escolha associá-lo a um novo ObjectDataSource chamado `ProductsBySupplierDataSource`.

[![associar o GridView ProductsBySupplier a um novo ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image27.png)

**Figura 16**: associar o `ProductsBySupplier` GridView a um novo ObjectDataSource ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image28.png))

Em seguida, configure o ObjectDataSource para usar a classe `ProductsBLL`. Como queremos apenas recuperar os produtos fornecidos pelo fornecedor selecionado, especifique que o ObjectDataSource deve invocar o método `GetProductsBySupplierID(supplierID)` para recuperar seus dados. Selecione (nenhum) nas listas suspensas nas guias atualizar, inserir e excluir.

[![configurar o ObjectDataSource para usar o método GetProductsBySupplierID (CódigoDoFornecedor)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image29.png)

**Figura 17**: configurar o ObjectDataSource para usar o método `GetProductsBySupplierID(supplierID)` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image30.png))

[![definir as listas suspensas como (nenhum) nas guias atualizar, inserir e excluir](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image31.png)

**Figura 18**: definir as listas suspensas como (nenhum) nas guias atualizar, inserir e excluir ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image32.png))

Depois de configurar as guias selecionar, atualizar, inserir e excluir, clique em Avançar. Como o método `GetProductsBySupplierID(supplierID)` espera um parâmetro de entrada, o assistente para criar fonte de dados solicita que especifiquemos a origem do valor s do parâmetro.

Temos algumas opções aqui para especificar a origem do valor de s de parâmetro. Poderíamos usar o objeto de parâmetro padrão e atribuir programaticamente o valor da propriedade `SuppliersSelectedIndex` à propriedade do parâmetro s `DefaultValue` no manipulador de eventos ObjectDataSource s `Selecting`. Consulte de [forma programática a configuração do tutorial de valores de parâmetro do ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md) para obter um atualizador sobre como atribuir valores de forma programática aos parâmetros de ObjectDataSource s.

Como alternativa, podemos usar um ControlParameter e consultar a propriedade `Suppliers` GridView s [`SelectedValue`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (consulte a Figura 19). A Propriedade GridView s `SelectedValue` retorna o valor de `DataKey` correspondente à [propriedade`SelectedIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). Para que essa opção funcione, precisamos definir programaticamente a Propriedade GridView `SelectedIndex` para a linha selecionada quando o botão de `ListProducts` é clicado. Como um benefício adicional, ao definir o `SelectedIndex`, o registro selecionado assumirá o `SelectedRowStyle` definido no tema `DataWebControls` (um plano de fundo amarelo).

[![usar um ControlParameter para especificar o GridView s SelectedValue como a origem do parâmetro](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image33.png)

**Figura 19**: usar um ControlParameter para especificar o GridView s SelectedValue como a origem do parâmetro ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image34.png))

Após a conclusão do assistente, o Visual Studio adicionará automaticamente campos para os campos de dados do produto. Remova todos os `ProductName`, `CategoryName`e `UnitPrice` BoundFields, e altere as propriedades `HeaderText` para produto, categoria e preço. Configure o `UnitPrice` BoundField para que seu valor seja formatado como uma moeda. Depois de fazer essas alterações, a marcação declarativa do painel, do GridView e do ObjectDataSource s deve ser semelhante ao seguinte:

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample11.aspx)]

Para concluir este exercício, precisamos definir a Propriedade GridView s `SelectedIndex` como a `SelectedSuppliersIndex` e a propriedade `Visible` do painel de `ProductsBySupplierPanel` como `true` quando o botão `ListProducts` é clicado. Para fazer isso, crie um manipulador de eventos para o evento de `ListProducts` botão de `Click` do controle da Web e adicione o seguinte código:

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample12.cs)]

Se um fornecedor não tiver sido selecionado no GridView, o rótulo de `ChooseSupplierMsg` será exibido e o painel de `ProductsBySupplierPanel` oculto. Caso contrário, se um fornecedor tiver sido selecionado, o `ProductsBySupplierPanel` será exibido e a Propriedade GridView s `SelectedIndex` será atualizada.

A figura 20 mostra os resultados depois que o fornecedor Bigfoot Breweries foi selecionado e o botão Mostrar produtos na página foi clicado.

[![os produtos fornecidos pelo Bigfoot Breweries estão listados na mesma página](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image35.png)

**Figura 20**: os produtos fornecidos pelo Bigfoot Breweries estão listados na mesma página ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-radio-buttons-cs/_static/image36.png))

## <a name="summary"></a>Resumo

Conforme discutido no [mestre/detalhes usando um GridView mestre selecionável com um tutorial detailview de detalhes](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) , os registros podem ser selecionados em um GridView usando um CommandField cuja propriedade `ShowSelectButton` está definida como `true`. Mas o CommandField exibe seus botões como botões de push, links ou imagens regulares. Uma interface de usuário de seleção de linha alternativa é fornecer um botão de opção ou uma caixa de seleção em cada linha GridView. Neste tutorial, examinamos como adicionar uma coluna de botões de opção.

Infelizmente, a adição de uma coluna de botões de rádio não é tão simples ou simples quanto pode ser esperada. Não há nenhum RadioButtonfield interno que possa ser adicionado ao clique de um botão e usar o controle da Web RadioButton em um TemplateField apresenta seu próprio conjunto de problemas. No final, para fornecer uma interface desse tipo, precisamos criar uma classe de `DataControlField` personalizada ou recorrer à injeção do HTML apropriado em um TemplateField durante o evento `RowCreated`.

Tendo explorado como adicionar uma coluna de botões de opção, vamos voltar à nossa atenção para adicionar uma coluna de caixas de seleção. Com uma coluna de caixas de seleção, um usuário pode selecionar uma ou mais linhas GridView e, em seguida, executar alguma operação em todas as linhas selecionadas (como selecionar um conjunto de emails de um cliente de email baseado na Web e, em seguida, escolher excluir todos os emails selecionados). No próximo tutorial, veremos como adicionar tal coluna.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de cliente potencial para este tutorial foi David Suru. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](adding-a-gridview-column-of-checkboxes-cs.md)
