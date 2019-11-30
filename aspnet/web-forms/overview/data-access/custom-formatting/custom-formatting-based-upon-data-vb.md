---
uid: web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
title: Formatação personalizada com base nos dados (VB) | Microsoft Docs
author: rick-anderson
description: O ajuste do formato de GridView, DetailsView ou FormView com base nos dados associados a ele pode ser realizado de várias maneiras. Neste tutorial, vamos para a l...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: df5a1525-386f-4632-972c-57b199870bc3
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/custom-formatting-based-upon-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 268dc763ef6954903f721a3015daaf10bf9bccb1
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74613211"
---
# <a name="custom-formatting-based-upon-data-vb"></a>Formatação personalizada baseada em dados (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_11_VB.exe) ou [baixar PDF](custom-formatting-based-upon-data-vb/_static/datatutorial11vb1.pdf)

> O ajuste do formato de GridView, DetailsView ou FormView com base nos dados associados a ele pode ser realizado de várias maneiras. Neste tutorial, veremos como realizar a formatação vinculada de dados por meio do uso dos manipuladores de eventos de ligação e vinculados.

## <a name="introduction"></a>Introdução

A aparência dos controles GridView, DetailsView e FormView pode ser personalizada por meio de uma infinidade de propriedades relacionadas ao estilo. Propriedades como `CssClass`, `Font`, `BorderWidth`, `BorderStyle`, `BorderColor`, `Width`e `Height`, entre outras, ditam a aparência geral do controle renderizado. As propriedades, incluindo `HeaderStyle`, `RowStyle`, `AlternatingRowStyle`e outras permitem que essas mesmas configurações de estilo sejam aplicadas a seções específicas. Da mesma forma, essas configurações de estilo podem ser aplicadas no nível do campo.

Em muitos cenários, no entanto, os requisitos de formatação dependem do valor dos dados exibidos. Por exemplo, para chamar a atenção para fora dos produtos de estoque, um relatório listando informações de produtos pode definir a cor do plano de fundo como amarelo para os produtos cujos campos `UnitsInStock` e `UnitsOnOrder` são iguais a 0. Para destacar os produtos mais caros, talvez queiramos exibir os preços desses produtos, custando mais de $75 em uma fonte em negrito.

O ajuste do formato de GridView, DetailsView ou FormView com base nos dados associados a ele pode ser realizado de várias maneiras. Neste tutorial, veremos como realizar a formatação vinculada de dados por meio do uso dos manipuladores de eventos `DataBound` e `RowDataBound`. No próximo tutorial, exploraremos uma abordagem alternativa.

## <a name="using-the-detailsview-controlsdataboundevent-handler"></a>Usando o manipulador de eventos de`DataBound`do controle DetailsView

Quando os dados são associados a um DetailsView, a partir de um controle da fonte de dados ou através da atribuição programática de dados à propriedade `DataSource` do controle e à chamada de seu método `DataBind()`, ocorre a seguinte sequência de etapas:

1. O evento de `DataBinding` do controle da Web de dados é acionado.
2. Os dados são associados ao controle da Web de dados.
3. O evento de `DataBound` do controle da Web de dados é acionado.

A lógica personalizada pode ser injetada imediatamente após as etapas 1 e 3 por meio de um manipulador de eventos. Ao criar um manipulador de eventos para o evento de `DataBound`, podemos determinar programaticamente os dados que foram associados ao controle da Web de dados e ajustar a formatação conforme necessário. Para ilustrar isso, vamos criar um DetailsView que listará informações gerais sobre um produto, mas exibirá o valor `UnitPrice` em uma ***fonte em negrito e itálico*** se ele exceder $75.

## <a name="step-1-displaying-the-product-information-in-a-detailsview"></a>Etapa 1: exibindo as informações do produto em um DetailsView

Abra a página `CustomColors.aspx` na pasta `CustomFormatting`, arraste um controle DetailsView da caixa de ferramentas para o designer, defina seu valor de propriedade `ID` como `ExpensiveProductsPriceInBoldItalic`e vincule-o a um novo controle ObjectDataSource que invoca o método `ProductsBLL` da classe `GetProducts()`. As etapas detalhadas para realizar isso são omitidas aqui para resumir, já que as examinamos em detalhes nos tutoriais anteriores.

Depois de associar o ObjectDataSource ao DetailsView, Reserve um momento para modificar a lista de campos. Eu optei por remover os `ProductID`, `SupplierID`, `CategoryID`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`e `Discontinued` BoundFields e renomeei e reformatei os BoundFields restantes. Também desmarquei as configurações `Width` e `Height`. Como o DetailsView exibe apenas um único registro, precisamos habilitar a paginação para permitir que o usuário final exiba todos os produtos. Faça isso marcando a caixa de seleção habilitar paginação na marca inteligente de DetailsView.

[![Figura 1: marque a caixa de seleção habilitar paginação na marca inteligente de DetailsView](custom-formatting-based-upon-data-vb/_static/image2.png)](custom-formatting-based-upon-data-vb/_static/image1.png)

**Figura 1**: Figura 1: marque a caixa de seleção habilitar paginação na marca inteligente de DetailsView ([clique para exibir a imagem em tamanho normal](custom-formatting-based-upon-data-vb/_static/image3.png))

Após essas alterações, a marcação DetailsView será:

[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample1.aspx)]

Reserve um tempo para testar esta página em seu navegador.

[![o controle DetailsView exibe um produto por vez](custom-formatting-based-upon-data-vb/_static/image5.png)](custom-formatting-based-upon-data-vb/_static/image4.png)

**Figura 2**: o controle DetailsView exibe um produto de cada vez ([clique para exibir a imagem em tamanho normal](custom-formatting-based-upon-data-vb/_static/image6.png))

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Etapa 2: determinar programaticamente o valor dos dados no manipulador de eventos de ligação

Para exibir o preço em negrito, a fonte em itálico para os produtos cujo valor de `UnitPrice` excede $75, precisamos primeiro ser capaz de determinar programaticamente o valor de `UnitPrice`. Para o DetailsView, isso pode ser feito no manipulador de eventos `DataBound`. Para criar o manipulador de eventos, clique em DetailsView no designer e, em seguida, navegue até a janela Propriedades. Pressione F4 para ativá-lo, se não estiver visível, ou vá para o menu exibir e selecione a opção de menu janela Propriedades. No janela Propriedades, clique no ícone de raio para listar os eventos de DetailsView. Em seguida, clique duas vezes no evento `DataBound` ou digite o nome do manipulador de eventos que você deseja criar.

![Criar um manipulador de eventos para o evento de vinculação de eventos](custom-formatting-based-upon-data-vb/_static/image7.png)

**Figura 3**: criar um manipulador de eventos para o evento `DataBound`

> [!NOTE]
> Você também pode criar um manipulador de eventos da parte de código da página ASP.NET. Lá, você encontrará duas listas suspensas na parte superior da página. Selecione o objeto na lista suspensa à esquerda e o evento para o qual você deseja criar um manipulador na lista suspensa à direita e o Visual Studio criará automaticamente o manipulador de eventos apropriado.

Isso criará automaticamente o manipulador de eventos e o levará para a parte de código onde ele foi adicionado. Neste ponto, você verá:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample2.vb)]

Os dados associados ao DetailsView podem ser acessados por meio da propriedade `DataItem`. Lembre-se de que estamos ligando nossos controles a uma DataTable fortemente tipada, que é composta de uma coleção de instâncias de DataRow fortemente tipadas. Quando DataTable é associado a DetailsView, a primeira DataRow na DataTable é atribuída à propriedade de `DataItem` de DetailsView. Especificamente, a propriedade `DataItem` é atribuída a um objeto `DataRowView`. Podemos usar a propriedade `Row` do `DataRowView`para obter acesso ao objeto DataRow subjacente, que é, na verdade, uma instância de `ProductsRow`. Assim que tivermos essa `ProductsRow` instância, podemos tomar nossa decisão simplesmente inspecionando os valores de Propriedade do objeto.

O código a seguir ilustra como determinar se o valor de `UnitPrice` associado ao controle DetailsView é maior que $75:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample3.vb)]

> [!NOTE]
> Como `UnitPrice` pode ter um valor de `NULL` no banco de dados, primeiro verificamos se não estamos lidando com um valor `NULL` antes de acessar a propriedade `UnitPrice` do `ProductsRow`. Essa verificação é importante porque, se tentarmos acessar a propriedade `UnitPrice` quando ela tiver um valor de `NULL`, o objeto `ProductsRow` lançará uma [exceção StrongTypingException](https://msdn.microsoft.com/library/system.data.strongtypingexception.aspx).

## <a name="step-3-formatting-the-unitprice-value-in-the-detailsview"></a>Etapa 3: Formatando o valor PreçoUnitário em DetailsView

Neste ponto, podemos determinar se o valor de `UnitPrice` associado a DetailsView tem um valor que exceda $75, mas ainda assim vemos como ajustar de forma programática a formatação de DetailsView. Para modificar a formatação de uma linha inteira em DetailsView, acesse a linha programaticamente usando `DetailsViewID.Rows(index)`; para modificar uma célula específica, use o `DetailsViewID.Rows(index).Cells(index)`de acesso. Assim que tivermos uma referência à linha ou célula, podemos ajustar sua aparência definindo suas propriedades relacionadas ao estilo.

O acesso a uma linha programaticamente requer que você saiba o índice da linha, que começa em 0. A linha de `UnitPrice` é a quinta linha no DetailsView, dando a ela um índice de 4 e tornando-a programaticamente acessível usando `ExpensiveProductsPriceInBoldItalic.Rows(4)`. Neste ponto, poderíamos ter o conteúdo da linha inteira exibido em negrito, com uma fonte em itálico usando o seguinte código:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample4.vb)]

No entanto, isso *fará com que o rótulo* (preço) e o valor seja negrito e itálico. Se quisermos tornar apenas o valor negrito e itálico, precisamos aplicar essa formatação à segunda célula da linha, que pode ser realizada usando o seguinte:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample5.vb)]

Como nossos tutoriais têm usado com mais folhas de estilos para manter uma separação clara entre a marcação renderizada e as informações relacionadas ao estilo, em vez de definir as propriedades específicas do estilo, conforme mostrado acima, vamos usar uma classe CSS. Abra a folha de estilo `Styles.css` e adicione uma nova classe CSS chamada `ExpensivePriceEmphasis` com a seguinte definição:

[!code-css[Main](custom-formatting-based-upon-data-vb/samples/sample6.css)]

Em seguida, no manipulador de eventos `DataBound`, defina a propriedade `CssClass` da célula como `ExpensivePriceEmphasis`. O código a seguir mostra o manipulador de eventos `DataBound` em sua totalidade:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample7.vb)]

Ao exibir Chai, que custa menos de $75, o preço é exibido em uma fonte normal (consulte a Figura 4). No entanto, ao exibir Mishi Kobe NIKU, que tem um preço de $97, o preço é exibido em uma fonte em negrito e itálico (consulte a Figura 5).

[![preços menores que $75 são exibidos em uma fonte normal](custom-formatting-based-upon-data-vb/_static/image9.png)](custom-formatting-based-upon-data-vb/_static/image8.png)

**Figura 4**: os preços menores que $75 são exibidos em uma fonte normal ([clique para exibir a imagem em tamanho](custom-formatting-based-upon-data-vb/_static/image10.png)normal)

[![preços de produtos caros são exibidos em negrito, com fonte em itálico](custom-formatting-based-upon-data-vb/_static/image12.png)](custom-formatting-based-upon-data-vb/_static/image11.png)

**Figura 5**: os preços dos produtos caros são exibidos em negrito, com fonte em itálico ([clique para exibir a imagem em tamanho normal](custom-formatting-based-upon-data-vb/_static/image13.png))

## <a name="using-the-formview-controlsdataboundevent-handler"></a>Usando o manipulador de eventos de`DataBound`do controle FormView

As etapas para determinar os dados subjacentes associados a um FormView são idênticas àquelas para um DetailsView criar um `DataBound` manipulador de eventos, converter a propriedade `DataItem` no tipo de objeto apropriado associado ao controle e determinar como proceder. O FormView e o DetailsView diferem, no entanto, de como a aparência da interface do usuário é atualizada.

O FormView não contém nenhum BoundField e, portanto, não tem a coleção de `Rows`. Em vez disso, um FormView é composto de modelos, que podem conter uma mistura de HTML estático, controles da Web e sintaxe de DataBinding. O ajuste do estilo de um FormView normalmente envolve o ajuste do estilo de um ou mais dos controles da Web dentro dos modelos de FormView.

Para ilustrar isso, vamos usar um FormView para listar produtos como no exemplo anterior, mas desta vez vamos exibir apenas o nome do produto e as unidades em estoque com as unidades em estoque exibidas em uma fonte vermelha se for menor ou igual a 10.

## <a name="step-4-displaying-the-product-information-in-a-formview"></a>Etapa 4: exibindo as informações do produto em um FormView

Adicione um FormView à página de `CustomColors.aspx` sob o DetailsView e defina sua propriedade `ID` como `LowStockedProductsInRed`. Associe o FormView ao controle ObjectDataSource criado na etapa anterior. Isso criará um `ItemTemplate`, `EditItemTemplate`e `InsertItemTemplate` para o FormView. Remova a `EditItemTemplate` e `InsertItemTemplate` e simplifique a `ItemTemplate` para incluir apenas os valores de `ProductName` e `UnitsInStock`, cada um em seus próprios controles de rótulo nomeados adequadamente. Assim como com o DetailsView do exemplo anterior, marque também a caixa de seleção habilitar paginação na marca inteligente do FormView.

Depois que essas edições, a marcação de FormView deve ser semelhante ao seguinte:

[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample8.aspx)]

Observe que o `ItemTemplate` contém:

- **HTML estático** o texto "Product:" e "Units in stock:" junto com os elementos `<br />` e `<b>`.
- **Web controla** os dois controles Label, `ProductNameLabel` e `UnitsInStockLabel`.
- **Sintaxe de DataBinding** a sintaxe `<%# Bind("ProductName") %>` e `<%# Bind("UnitsInStock") %>`, que atribui os valores desses campos para os controles de rótulo ' `Text` Propriedades.

## <a name="step-5-programmatically-determining-the-value-of-the-data-in-the-databound-event-handler"></a>Etapa 5: determinar programaticamente o valor dos dados no manipulador de eventos de vinculação

Com a marcação do FormView concluída, a próxima etapa é determinar programaticamente se o valor de `UnitsInStock` é menor ou igual a 10. Isso é feito exatamente da mesma maneira com o FormView, como era com o DetailsView. Comece criando um manipulador de eventos para o evento `DataBound` do FormView.

![Criar o manipulador de eventos de ligação](custom-formatting-based-upon-data-vb/_static/image14.png)

**Figura 6**: criar o manipulador de eventos `DataBound`

No manipulador de eventos, converta a propriedade `DataItem` do FormView em uma instância `ProductsRow` e determine se o valor `UnitsInPrice` é de que precisamos exibi-lo em uma fonte vermelha.

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample9.vb)]

## <a name="step-6-formatting-the-unitsinstocklabel-label-control-in-the-formviews-itemtemplate"></a>Etapa 6: Formatando o controle de rótulo UnitsInStockLabel no ItemTemplate do FormView

A etapa final é formatar o valor de `UnitsInStock` exibido em uma fonte vermelha se o valor for 10 ou menos. Para fazer isso, precisamos acessar programaticamente o controle de `UnitsInStockLabel` no `ItemTemplate` e definir suas propriedades de estilo para que seu texto seja exibido em vermelho. Para acessar um controle da Web em um modelo, use o método `FindControl("controlID")` como este:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample10.vb)]

Para nosso exemplo, queremos acessar um controle de rótulo cujo valor de `ID` seja `UnitsInStockLabel`, portanto, usaremos:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample11.vb)]

Assim que tivermos uma referência programática para o controle da Web, podemos modificar suas propriedades relacionadas ao estilo, conforme necessário. Assim como no exemplo anterior, criei uma classe CSS em `Styles.css` chamada `LowUnitsInStockEmphasis`. Para aplicar esse estilo ao controle de rótulo da Web, defina sua propriedade de `CssClass` de acordo.

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample12.vb)]

> [!NOTE]
> A sintaxe para formatar um modelo acessando programaticamente o controle da Web usando `FindControl("controlID")` e, em seguida, definindo suas propriedades relacionadas ao estilo também pode ser usada ao usar [TemplateFields](https://msdn.microsoft.com/library/system.web.ui.webcontrols.templatefield(VS.80).aspx) nos controles DetailsView ou GridView. Vamos examinar TemplateFields em nosso próximo tutorial.

As figuras 7 mostram o FormView ao exibir um produto cujo valor de `UnitsInStock` é maior que 10, enquanto o produto na Figura 8 tem seu valor menor que 10.

[![para produtos com unidades suficientemente grandes em estoque, nenhuma formatação personalizada é aplicada](custom-formatting-based-upon-data-vb/_static/image16.png)](custom-formatting-based-upon-data-vb/_static/image15.png)

**Figura 7**: para produtos com unidades suficientemente grandes em estoque, nenhuma formatação personalizada é aplicada ([clique para exibir a imagem em tamanho normal](custom-formatting-based-upon-data-vb/_static/image17.png))

[![as unidades no número de estoque são mostradas em vermelho para esses produtos com valores de 10 ou menos](custom-formatting-based-upon-data-vb/_static/image19.png)](custom-formatting-based-upon-data-vb/_static/image18.png)

**Figura 8**: as unidades no número de estoque são mostradas em vermelho para esses produtos com valores de 10 ou menos ([clique para exibir a imagem em tamanho normal](custom-formatting-based-upon-data-vb/_static/image20.png))

## <a name="formatting-with-the-gridviewsrowdataboundevent"></a>Formatando com o evento`RowDataBound`do GridView

Examinamos anteriormente a sequência de etapas pelas quais os controles DetailsView e FormView passam durante a associação de dados. Vamos examinar essas etapas novamente como um atualizador.

1. O evento de `DataBinding` do controle da Web de dados é acionado.
2. Os dados são associados ao controle da Web de dados.
3. O evento de `DataBound` do controle da Web de dados é acionado.

Essas três etapas simples são suficientes para DetailsView e FormView porque elas exibem apenas um único registro. Para o GridView, que exibe *todos os* registros associados a ele (não apenas o primeiro), a etapa 2 é um pouco mais envolvida.

Na etapa 2, o GridView enumera a fonte de dados e, para cada registro, cria uma instância de `GridViewRow` e associa o registro atual a ela. Para cada `GridViewRow` adicionado ao GridView, dois eventos são gerados:

- **`RowCreated`** acionado após a criação do `GridViewRow`
- **`RowDataBound`** é acionado após o registro atual ter sido associado ao `GridViewRow`.

Para o GridView, a vinculação de dados é mais precisamente descrita pela seguinte sequência de etapas:

1. O evento `DataBinding` do GridView é acionado.
2. Os dados estão associados ao GridView.   
  
   Para cada registro na fonte de dados 

    1. Criar um objeto `GridViewRow`
    2. Acionar o evento de `RowCreated`
    3. Associar o registro ao `GridViewRow`
    4. Acionar o evento de `RowDataBound`
    5. Adicionar o `GridViewRow` à coleção de `Rows`
3. O evento `DataBound` do GridView é acionado.

Para personalizar o formato dos registros individuais do GridView, precisamos criar um manipulador de eventos para o evento `RowDataBound`. Para ilustrar isso, vamos adicionar um GridView à página `CustomColors.aspx` que lista o nome, a categoria e o preço de cada produto, destacando os produtos cujo preço é menor que $10 com uma cor de fundo amarela.

## <a name="step-7-displaying-product-information-in-a-gridview"></a>Etapa 7: exibindo informações do produto em um GridView

Adicione um GridView abaixo do FormView do exemplo anterior e defina sua propriedade `ID` como `HighlightCheapProducts`. Como já temos um ObjectDataSource que retorna todos os produtos na página, associe o GridView a ele. Por fim, edite as BoundFields do GridView para incluir apenas os nomes, as categorias e os preços dos produtos. Depois que essas edições, a marcação do GridView deve ser semelhante a:

[!code-aspx[Main](custom-formatting-based-upon-data-vb/samples/sample13.aspx)]

A Figura 9 mostra nosso progresso para esse ponto quando exibido por meio de um navegador.

[![o GridView lista o nome, a categoria e o preço de cada produto](custom-formatting-based-upon-data-vb/_static/image22.png)](custom-formatting-based-upon-data-vb/_static/image21.png)

**Figura 9**: o GridView lista o nome, a categoria e o preço de cada produto ([clique para exibir a imagem em tamanho normal](custom-formatting-based-upon-data-vb/_static/image23.png))

## <a name="step-8-programmatically-determining-the-value-of-the-data-in-the-rowdatabound-event-handler"></a>Etapa 8: determinar programaticamente o valor dos dados no manipulador de eventos de transdatas

Quando o `ProductsDataTable` está associado ao GridView, suas instâncias de `ProductsRow` são enumeradas e para cada `ProductsRow` um `GridViewRow` é criado. A propriedade `DataItem` do `GridViewRow`é atribuída à `ProductRow`específica, após a qual o manipulador de eventos de `RowDataBound` do GridView é gerado. Para determinar o valor de `UnitPrice` para cada produto associado ao GridView, precisamos criar um manipulador de eventos para o evento `RowDataBound` do GridView. Nesse manipulador de eventos, podemos inspecionar o valor `UnitPrice` para o `GridViewRow` atual e tomar uma decisão de formatação para essa linha.

Esse manipulador de eventos pode ser criado usando a mesma série de etapas que com FormView e DetailsView.

![Criar um manipulador de eventos para o evento de teleligação do GridView](custom-formatting-based-upon-data-vb/_static/image24.png)

**Figura 10**: criar um manipulador de eventos para o evento `RowDataBound` do GridView

Criar o manipulador de eventos dessa maneira fará com que o seguinte código seja adicionado automaticamente à parte de código da página ASP.NET:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample14.vb)]

Quando o evento `RowDataBound` é acionado, o manipulador de eventos é passado como seu segundo parâmetro um objeto do tipo `GridViewRowEventArgs`, que tem uma propriedade denominada `Row`. Essa propriedade retorna uma referência à `GridViewRow` que estava apenas associada a dados. Para acessar a instância de `ProductsRow` associada ao `GridViewRow`, usamos a propriedade `DataItem` da seguinte forma:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample15.vb)]

Ao trabalhar com o manipulador de eventos `RowDataBound`, é importante ter em mente que o GridView é composto por diferentes tipos de linhas e que esse evento é acionado para *todos os* tipos de linha. Um tipo de `GridViewRow`pode ser determinado por sua propriedade `RowType` e pode ter um dos valores possíveis:

- `DataRow` uma linha associada a um registro da `DataSource` do GridView
- `EmptyDataRow` a linha exibida se o `DataSource` do GridView estiver vazio
- `Footer` a linha de rodapé; mostrado se a propriedade `ShowFooter` do GridView estiver definida como `True`
- `Header` a linha de cabeçalho; mostrado se a propriedade do DefaultHeader do GridView estiver definida como `True` (o padrão)
- `Pager` para GridView que implementam paginação, a linha que exibe a interface de paginação
- Não `Separator` usado para o GridView, mas usado pelas propriedades de `RowType` para os controles DataList e Repeater, dois controles da Web de dados que discutiremos em Tutoriais futuros

Como as linhas `EmptyDataRow`, `Header`, `Footer`e `Pager` não estão associadas a um registro de `DataSource`, elas sempre terão um valor de `Nothing` para sua propriedade `DataItem`. Por esse motivo, antes de tentar trabalhar com a propriedade `DataItem` do `GridViewRow`atual, primeiro devemos nos certificar de que estamos lidando com uma `DataRow`. Isso pode ser feito verificando a propriedade `RowType` do `GridViewRow`da seguinte maneira:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample16.vb)]

## <a name="step-9-highlighting-the-row-yellow-when-the-unitprice-value-is-less-than-1000"></a>Etapa 9: realçando a linha amarela quando o valor de PreçoUnitário é menor que $10

A última etapa é realçar programaticamente o `GridViewRow` inteiro se o valor de `UnitPrice` para essa linha for menor que $10. A sintaxe para acessar as linhas ou células de um GridView é a mesma que com o `GridViewID.Rows(index)` DetailsView para acessar a linha inteira, `GridViewID.Rows(index).Cells(index)` para acessar uma célula específica. No entanto, quando o manipulador de eventos `RowDataBound` dispara, o `GridViewRow` associado aos dados ainda deve ser adicionado à coleção de `Rows` do GridView. Portanto, você não pode acessar a instância de `GridViewRow` atual do manipulador de eventos `RowDataBound` usando a coleção de linhas.

Em vez de `GridViewID.Rows(index)`, podemos referenciar a instância `GridViewRow` atual no manipulador de eventos `RowDataBound` usando `e.Row`. Ou seja, para realçar a instância de `GridViewRow` atual do manipulador de eventos `RowDataBound`, usaremos:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample17.vb)]

Em vez de definir a propriedade de `BackColor` do `GridViewRow`diretamente, vamos usar as classes CSS. Criei uma classe CSS chamada `AffordablePriceEmphasis` que define a cor do plano de fundo como amarelo. O manipulador de eventos de `RowDataBound` concluído segue:

[!code-vb[Main](custom-formatting-based-upon-data-vb/samples/sample18.vb)]

[![os produtos mais acessíveis são amarelos realçados](custom-formatting-based-upon-data-vb/_static/image26.png)](custom-formatting-based-upon-data-vb/_static/image25.png)

**Figura 11**: os produtos mais acessíveis são amarelos realçados ([clique para exibir a imagem em tamanho normal](custom-formatting-based-upon-data-vb/_static/image27.png))

## <a name="summary"></a>Resumo

Neste tutorial, vimos como formatar o GridView, o DetailsView e o FormView com base nos dados associados ao controle. Para fazer isso, criamos um manipulador de eventos para os eventos `DataBound` ou `RowDataBound`, em que os dados subjacentes foram examinados junto com uma alteração de formatação, se necessário. Para acessar os dados associados a um DetailsView ou FormView, usamos a propriedade `DataItem` no manipulador de eventos `DataBound`; para um GridView, cada propriedade `DataItem` da instância de `GridViewRow` contém os dados associados a essa linha, que está disponível no manipulador de eventos `RowDataBound`.

A sintaxe para ajustar programaticamente a formatação do controle da Web de dados depende do controle da Web e de como os dados a serem formatados são exibidos. Para controles DetailsView e GridView, as linhas e células podem ser acessadas por um índice ordinal. Para o FormView, que usa modelos, o método `FindControl("controlID")` geralmente é usado para localizar um controle da Web de dentro do modelo.

No próximo tutorial, veremos como usar modelos com GridView e DetailsView. Além disso, veremos outra técnica para personalizar a formatação com base nos dados subjacentes.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram E.R. Gilmore, Dennis Patterson e Dan Jagers. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-summary-information-in-the-gridview-s-footer-cs.md)
> [Próximo](using-templatefields-in-the-gridview-control-vb.md)
