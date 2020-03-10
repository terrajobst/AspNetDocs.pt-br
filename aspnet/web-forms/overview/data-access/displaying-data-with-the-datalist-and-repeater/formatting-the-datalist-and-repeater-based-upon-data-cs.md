---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
title: Formatando o DataList e o Repeater com baseC#nos dados () | Microsoft Docs
author: rick-anderson
description: Neste tutorial, vamos percorrer exemplos de como formatar a aparência dos controles DataList e Repeater, usando as funções de formatação com...
ms.author: riande
ms.date: 09/13/2006
ms.assetid: 83e3d759-82b8-41e6-8d62-f0f4b3edec41
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/formatting-the-datalist-and-repeater-based-upon-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 68de3450ed97fc7bd0efb27e089d9db8e3e85fb0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78611632"
---
# <a name="formatting-the-datalist-and-repeater-based-upon-data-c"></a>Formatação do DataList e Repeater com base nos dados (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_30_CS.exe) ou [baixar PDF](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/datatutorial30cs1.pdf)

> Neste tutorial, vamos percorrer exemplos de como formato a aparência dos controles DataList e Repeater, seja usando funções de formatação dentro de modelos ou manipulando o evento de vinculação de ligações.

## <a name="introduction"></a>Introdução

Como vimos no tutorial anterior, o DataList oferece várias propriedades relacionadas ao estilo que afetam sua aparência. Em particular, vimos como atribuir classes CSS padrão às propriedades DataList s `HeaderStyle`, `ItemStyle`, `AlternatingItemStyle`e `SelectedItemStyle`. Além dessas quatro propriedades, o DataList inclui várias outras propriedades relacionadas a estilo, como `Font`, `ForeColor`, `BackColor`e `BorderWidth`, para citar alguns. O controle Repeater não contém nenhuma propriedade relacionada ao estilo. Essas configurações de estilo devem ser feitas diretamente dentro da marcação nos modelos do repetidor.

No entanto, geralmente, como os dados devem ser formatados dependem dos dados em si. Por exemplo, ao listar produtos, podemos querer exibir as informações do produto em uma cor de fonte cinza-claro se ela for descontinuada ou talvez desejarmos realçar o valor de `UnitsInStock` se ele for zero. Como vimos nos tutoriais anteriores, o GridView, o DetailsView e o FormView oferecem duas maneiras distintas de Formatar sua aparência com base em seus dados:

- **O evento `DataBound`** cria um manipulador de eventos para o evento de `DataBound` apropriado, que é acionado depois que os dados são associados a cada item (para o GridView, ele era o evento de `RowDataBound`; para o DataList e o Repeater, ele é o evento de `ItemDataBound`). Nesse manipulador de eventos, os dados apenas associados podem ser examinados e as decisões de formatação tomadas. Examinamos essa técnica no tutorial [formatação personalizada com base no data](../custom-formatting/custom-formatting-based-upon-data-cs.md) .
- **Funções de formatação em modelos** ao usar TemplateFields nos controles DetailsView ou GridView, ou um modelo no controle FormView, podemos adicionar uma função de formatação à classe code-behind da página ASP.net s, à camada de lógica de negócios ou a qualquer outra biblioteca de classes acessível a partir do aplicativo Web. Essa função de formatação pode aceitar um número arbitrário de parâmetros de entrada, mas deve retornar o HTML para renderizar no modelo. As funções de formatação foram examinadas primeiro no tutorial [Usando TemplateFields no controle GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) .

Essas duas técnicas de formatação estão disponíveis com os controles DataList e Repeater. Neste tutorial, vamos percorrer exemplos usando as duas técnicas para ambos os controles.

## <a name="using-theitemdataboundevent-handler"></a>Usando o manipulador de eventos`ItemDataBound`

Quando os dados são associados a um DataList, de um controle da fonte de dados ou por meio de programaticamente atribuir dados à propriedade s `DataSource` do controle e chamar seu método `DataBind()`, o evento DataList s `DataBinding` é disparado, a fonte de dados enumerada e cada registro de dados é associado ao DataList. Para cada registro na fonte de dados, o DataList cria um objeto [`DataListItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.aspx) que é então associado ao registro atual. Durante esse processo, o DataList gera dois eventos:

- **`ItemCreated`** acionado após a criação do `DataListItem`
- **`ItemDataBound`** é acionado após o registro atual ter sido associado ao `DataListItem`

As etapas a seguir descrevem o processo de vinculação de dados para o controle DataList.

1. O evento DataList s [`DataBinding`](https://msdn.microsoft.com/library/system.web.ui.control.databinding.aspx) dispara
2. Os dados estão associados ao DataList  
  
   Para cada registro na fonte de dados 

    1. Criar um objeto `DataListItem`
    2. Acionar o [evento de`ItemCreated`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemcreated.aspx)
    3. Associar o registro ao `DataListItem`
    4. Acionar o [evento de`ItemDataBound`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.itemdatabound.aspx)
    5. Adicionar o `DataListItem` à coleção de `Items`

Ao vincular dados ao controle Repeater, ele percorre exatamente a mesma sequência de etapas. A única diferença é que, em vez de `DataListItem` instâncias que estão sendo criadas, o repetidor usa [`RepeaterItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem(VS.80).aspx)s.

> [!NOTE]
> O leitor do astuto pode ter notado uma ligeira anomalia entre a sequência de etapas que se transtentam quando o DataList e o Repeater estão associados a dados versus quando o GridView está associado a dados. Na extremidade final do processo de vinculação de dados, o GridView gera o evento `DataBound`; no entanto, nem o controle DataList nem Repeater tem tal evento. Isso ocorre porque os controles DataList e Repeater foram criados de volta no período de tempo ASP.NET 1. x, antes que o padrão de manipulador de eventos pré e pós-leve se tornar comum.

Assim como com o GridView, uma opção para formatação com base nos dados é criar um manipulador de eventos para o evento `ItemDataBound`. Esse manipulador de eventos inspecionaria os dados que tinham sido vinculados ao `DataListItem` ou `RepeaterItem` e afetaria a formatação do controle, conforme necessário.

Para o controle DataList, as alterações de formatação para o item inteiro podem ser implementadas usando as propriedades relacionadas ao estilo `DataListItem` s, que incluem o `Font`padrão, `ForeColor`, `BackColor`, `CssClass`e assim por diante. Para afetar a formatação de determinados controles da Web dentro do modelo DataList s, precisamos acessar e modificar o estilo desses controles da Web programaticamente. Vimos como fazer isso de volta no tutorial *formatação personalizada com base no data* . Como o controle Repeater, a classe `RepeaterItem` não tem nenhuma propriedade relacionada ao estilo; Portanto, todas as alterações relacionadas a estilo feitas a um `RepeaterItem` no manipulador de eventos `ItemDataBound` devem ser feitas por meio de programação e atualização de controles da Web dentro do modelo.

Como a técnica de formatação de `ItemDataBound` para DataList e Repeater são praticamente idênticas, nosso exemplo se concentrará em usar o DataList.

## <a name="step-1-displaying-product-information-in-the-datalist"></a>Etapa 1: exibindo informações do produto no DataList

Antes de nos preocuparmos com a formatação, vamos criar primeiro uma página que usa um DataList para exibir informações sobre o produto. No [tutorial anterior](displaying-data-with-the-datalist-and-repeater-controls-cs.md) , criamos um DataList cujo `ItemTemplate` exibia cada nome, categoria, fornecedor, quantidade por unidade e preço do produto. Vamos repetir essa funcionalidade aqui neste tutorial. Para fazer isso, você pode recriar o DataList e seu ObjectDataSource a partir do zero, ou você pode copiar sobre esses controles da página criada no tutorial anterior (`Basics.aspx`) e colá-los na página deste tutorial (`Formatting.aspx`).

Depois de replicar a funcionalidade DataList e ObjectDataSource de `Basics.aspx` para `Formatting.aspx`, Reserve um tempo para alterar a propriedade DataList s `ID` de `DataList1` para uma `ItemDataBoundFormattingExample`mais descritiva. Em seguida, exiba o DataList em um navegador. Como mostra a Figura 1, a única diferença de formatação entre cada produto é que a cor do plano de fundo alterna.

[![os produtos estão listados no controle DataList](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image2.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image1.png)

**Figura 1**: os produtos são listados no controle DataList ([clique para exibir a imagem em tamanho normal](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image3.png))

Para este tutorial, vamos formatar o DataList de modo que qualquer produto com um preço inferior a $20 terá o nome e o preço unitário realçado amarelo.

## <a name="step-2-programmatically-determining-the-value-of-the-data-in-the-itemdatabound-event-handler"></a>Etapa 2: determinar programaticamente o valor dos dados no manipulador de eventos de Hiperligação

Como somente os produtos com um preço abaixo de $20 terão a formatação personalizada aplicada, devemos ser capazes de determinar cada preço de produto. Ao vincular dados a um DataList, o DataList enumera os registros em sua fonte de dados e, para cada registro, cria uma instância de `DataListItem`, associando o registro da fonte de dados ao `DataListItem`. Depois que os dados de registro em particular tiverem sido associados ao objeto de `DataListItem` atual, o evento DataList s `ItemDataBound` será disparado. Podemos criar um manipulador de eventos para esse evento para inspecionar os valores de dados para o `DataListItem` atual e, com base nesses valores, fazer as alterações de formatação necessárias.

Crie um evento `ItemDataBound` para o DataList e adicione o seguinte código:

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample1.cs)]

Embora o conceito e a semântica por trás do manipulador de eventos DataList s `ItemDataBound` sejam os mesmos usados pelo manipulador de eventos GridView s `RowDataBound` no tutorial *formatação personalizada com base no data* , a sintaxe é ligeiramente diferente. Quando o evento `ItemDataBound` é acionado, o `DataListItem` apenas associado a dados é passado para o manipulador de eventos correspondente via `e.Item` (em vez de `e.Row`, como com o manipulador de eventos GridView s `RowDataBound`). O manipulador de eventos DataList s `ItemDataBound` é acionado para *cada* linha adicionada ao DataList, incluindo linhas de cabeçalho, linhas de rodapé e linhas separadoras. No entanto, as informações do produto só são associadas às linhas de dados. Portanto, ao usar o evento `ItemDataBound` para inspecionar os dados ligados ao DataList, precisamos primeiro garantir que vamos trabalhar com um item de dados. Isso pode ser feito verificando a propriedade `DataListItem` s [`ItemType`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistitem.itemtype.aspx), que pode ter um dos [oito valores a seguir](https://msdn.microsoft.com/library/system.web.ui.webcontrols.listitemtype.aspx):

- `AlternatingItem`
- `EditItem`
- `Footer`
- `Header`
- `Item`
- `Pager`
- `SelectedItem`
- `Separator`

Tanto `Item` quanto `AlternatingItem``DataListItem` emitem os itens de dados DataLists. Supondo que vamos trabalhar com um `Item` ou `AlternatingItem`, Acessamos a instância real `ProductsRow` associada à `DataListItem`atual. A propriedade `DataListItem` s [`DataItem`](https://msdn.microsoft.com/system.web.ui.webcontrols.datalistitem.dataitem.aspx) contém uma referência ao objeto `DataRowView`, cuja propriedade `Row` fornece uma referência ao objeto de `ProductsRow` real.

Em seguida, verificamos a propriedade `ProductsRow` s `UnitPrice` da instância. Como o campo `UnitPrice` da tabela produtos permite `NULL` valores, antes de tentar acessar a propriedade `UnitPrice` primeiro, devemos verificar se ele tem um valor de `NULL` usando o método `IsUnitPriceNull()`. Se o valor de `UnitPrice` não for `NULL`, verificaremos se ele é menor que $20. Se estiver de fato abaixo de $20, precisaremos aplicar a formatação personalizada.

## <a name="step-3-highlighting-the-product-s-name-and-price"></a>Etapa 3: realçando o nome e o preço do produto

Quando sabemos que o preço de um produto é menor que $20, tudo o que resta é destacar seu nome e preço. Para fazer isso, primeiro devemos fazer referência programaticamente aos controles Label no `ItemTemplate` que exibem o nome e o preço do produto. Em seguida, precisamos que eles exibam um plano de fundo amarelo. Essas informações de formatação podem ser aplicadas pela modificação direta dos rótulos `BackColor` Propriedades (`LabelID.BackColor = Color.Yellow`); no entanto, o ideal é que todas as coisas relacionadas à exibição sejam expressas por meio de folhas de estilos em cascata. Na verdade, já temos uma folha de estilos que fornece a formatação desejada definida em `Styles.css` - `AffordablePriceEmphasis`, que foi criada e discutida no tutorial *formatação personalizada com base no data* .

Para aplicar a formatação, basta definir os dois controles da Web de rótulo `CssClass` propriedades como `AffordablePriceEmphasis`, conforme mostrado no código a seguir:

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample2.cs)]

Com o manipulador de eventos `ItemDataBound` concluído, visite novamente a página `Formatting.aspx` em um navegador. Como a Figura 2 ilustra, os produtos com preço inferior a $20 têm o nome e o preço realçados.

[![os produtos menores que $20 estão realçados](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image5.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image4.png)

**Figura 2**: os produtos menores que $20 são realçados ([clique para exibir a imagem em tamanho normal](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image6.png))

> [!NOTE]
> Como o DataList é renderizado como um `<table>`HTML, suas instâncias de `DataListItem` têm propriedades relacionadas a estilo que podem ser definidas para aplicar um estilo específico a todo o item. Por exemplo, se quiséssemos destacar o item *inteiro* amarelo quando seu preço fosse menor que $20, poderíamos ter substituído o código que referenciou os rótulos e definir suas propriedades `CssClass` com a seguinte linha de código: `e.Item.CssClass = "AffordablePriceEmphasis"` (consulte a Figura 3).

Os `RepeaterItem` s que compõem o controle Repeater, no entanto, Don t oferecem essas propriedades de nível de estilo. Portanto, a aplicação da formatação personalizada ao repetidor exige a aplicação de propriedades de estilo aos controles da Web dentro dos modelos repetidos, assim como fizemos na Figura 2.

[![o item de produto inteiro é realçado para produtos em $20](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image8.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image7.png)

**Figura 3**: o item de produto inteiro é realçado para produtos em $20 ([clique para exibir a imagem em tamanho normal](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image9.png))

## <a name="using-formatting-functions-from-within-the-template"></a>Usando funções de formatação de dentro do modelo

No tutorial *Usando TemplateFields no controle GridView* , vimos como usar uma função de formatação em um modelo GridView para aplicar formatação personalizada com base nos dados associados às linhas GridView. Uma função de formatação é um método que pode ser invocado de um modelo e retorna o HTML a ser emitido em seu lugar. As funções de formatação podem residir na classe code-behind da página ASP.NET ou podem ser centralizadas em arquivos de classe na pasta `App_Code` ou em um projeto de biblioteca de classes separado. Mover a função de formatação para fora da classe code-behind da página ASP.NET é ideal se você planeja usar a mesma função de formatação em várias páginas do ASP.NET ou em outros aplicativos Web do ASP.NET.

Para demonstrar as funções de formatação, deixe que as informações do produto incluam o texto [descontinuado] ao lado do nome do produto, caso ele tenha sido descontinuado. Além disso, deixe o preço realçado amarelo se for menor que $20 (como fizemos no exemplo do manipulador de eventos `ItemDataBound`); Se o preço for $20 ou superior, não exibirá o preço real, mas, em vez disso, o texto, ligue para uma cotação de preços. A Figura 4 mostra uma captura de tela da listagem de produtos com essas regras de formatação aplicadas.

[![para produtos caros, o preço é substituído pelo texto, por favor, ligue para uma cotação de preços](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image11.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image10.png)

**Figura 4**: para produtos caros, o preço é substituído pelo texto, entre em contato com uma cotação de preços ([clique para exibir a imagem em tamanho normal](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image12.png))

## <a name="step-1-create-the-formatting-functions"></a>Etapa 1: criar as funções de formatação

Para este exemplo, precisamos de duas funções de formatação, uma que exiba o nome do produto junto com o texto [descontinuado], se necessário, e outro que exiba um preço realçado se for menor que $20 ou o texto, por favor, chame uma citação de preço de outra forma. Deixe que o s Crie essas funções na classe code-behind da página ASP.NET s e nomeie-as `DisplayProductNameAndDiscontinuedStatus` e `DisplayPrice`. Os dois métodos precisam retornar o HTML para renderização como uma cadeia de caracteres e ambos precisam ser marcados `Protected` (ou `Public`) para serem invocados na parte da sintaxe declarativa da página ASP.NET. O código para esses dois métodos segue:

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample3.cs)]

Observe que o método `DisplayProductNameAndDiscontinuedStatus` aceita os valores dos campos de dados `productName` e `discontinued` como valores escalares, enquanto o método `DisplayPrice` aceita uma instância `ProductsRow` (em vez de um valor escalar `unitPrice`). Qualquer abordagem funcionará; no entanto, se a função de formatação estiver funcionando com valores escalares que podem conter valores de `NULL` de banco de dados (como `UnitPrice`; nem `ProductName` nem `Discontinued` permitir `NULL` valores), deve-se tomar cuidado especial ao lidar com essas entradas escalares.

Em particular, o parâmetro de entrada deve ser do tipo `Object`, pois o valor de entrada pode ser uma instância `DBNull` em vez do tipo de dados esperado. Além disso, deve-se fazer uma verificação para determinar se o valor de entrada é um valor de `NULL` de banco de dados. Ou seja, se quiséssemos que o método `DisplayPrice` aceite o preço como um valor escalar, nós d precisaremos usar o código a seguir:

[!code-csharp[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample4.cs)]

Observe que o parâmetro de entrada `unitPrice` é do tipo `Object` e que a instrução condicional foi modificada para verificar se `unitPrice` é `DBNull` ou não. Além disso, como o parâmetro de entrada `unitPrice` é passado como um `Object`, ele deve ser convertido em um valor decimal.

## <a name="step-2-calling-the-formatting-function-from-the-datalist-s-itemtemplate"></a>Etapa 2: chamando a função de formatação do ItemTemplate s de DataList

Com as funções de formatação adicionadas à nossa classe code-behind da página ASP.NET, tudo o que resta é invocar essas funções de formatação a partir do `ItemTemplate`s do DataList. Para chamar uma função de formatação de um modelo, coloque a chamada de função dentro da sintaxe de DataBinding:

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample5.aspx)]

No `ItemTemplate` de DataList, o controle de `ProductNameLabel` rótulo da Web exibe atualmente o nome do produto, atribuindo sua propriedade `Text` o resultado de `<%# Eval("ProductName") %>`. Para que ele exiba o nome mais o texto [descontinuado], se necessário, atualize a sintaxe declarativa para que ele atribua à propriedade `Text` o valor do método `DisplayProductNameAndDiscontinuedStatus`. Ao fazer isso, devemos passar o nome do produto e os valores descontinuados usando a sintaxe `Eval("columnName")`. `Eval` retorna um valor do tipo `Object`, mas o método `DisplayProductNameAndDiscontinuedStatus` espera parâmetros de entrada do tipo `String` e `Boolean`; Portanto, devemos converter os valores retornados pelo método `Eval` para os tipos de parâmetro de entrada esperados, desta forma:

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample6.aspx)]

Para exibir o preço, podemos simplesmente definir o `UnitPriceLabel` rótulo s `Text` Propriedade como o valor retornado pelo método `DisplayPrice`, exatamente como fizemos para exibir o texto do produto e [descontinuado]. No entanto, em vez de passar o `UnitPrice` como um parâmetro de entrada escalar, em vez disso, passamos toda a instância `ProductsRow`:

[!code-aspx[Main](formatting-the-datalist-and-repeater-based-upon-data-cs/samples/sample7.aspx)]

Com as chamadas para as funções de formatação em vigor, Reserve um tempo para exibir nosso progresso em um navegador. Sua tela deve ser semelhante à figura 5, com os produtos descontinuados, incluindo o texto [descontinuado] e os produtos que custam mais de $20 com o preço substituído pelo texto, por favor, ligue para uma cotação de preços.

[![para produtos caros, o preço é substituído pelo texto, por favor, ligue para uma cotação de preços](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image14.png)](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image13.png)

**Figura 5**: para produtos caros, o preço é substituído pelo texto, entre em contato com uma cotação de preços ([clique para exibir a imagem em tamanho normal](formatting-the-datalist-and-repeater-based-upon-data-cs/_static/image15.png))

## <a name="summary"></a>Resumo

A formatação do conteúdo de um controle DataList ou Repeater com base nos dados pode ser realizada usando duas técnicas. A primeira técnica é criar um manipulador de eventos para o evento `ItemDataBound`, que é acionado, pois cada registro na fonte de dados é associado a um novo `DataListItem` ou `RepeaterItem`. No manipulador de eventos `ItemDataBound`, os dados do item s atuais podem ser examinados e, em seguida, a formatação pode ser aplicada ao conteúdo do modelo ou, para `DataListItem` s, ao próprio item inteiro.

Como alternativa, a formatação personalizada pode ser realizada por meio de funções de formatação. Uma função de formatação é um método que pode ser invocado dos modelos DataList ou Repeater s que retorna o HTML a ser emitido em seu lugar. Geralmente, o HTML retornado por uma função de formatação é determinado pelos valores que estão sendo associados ao item atual. Esses valores podem ser passados para a função de formatação, seja como valores escalares ou passando o objeto inteiro que está sendo associado ao item (como a instância de `ProductsRow`).

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Yaakov Ellis, Randy Schmidt e Liz Shulok. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-data-with-the-datalist-and-repeater-controls-cs.md)
> [Próximo](showing-multiple-records-per-row-with-the-datalist-control-cs.md)
