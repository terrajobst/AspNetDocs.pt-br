---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
title: Examinando os eventos associados à inserção, atualização e exclusão (C#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, examinaremos o uso dos eventos que ocorrem antes, durante e após uma operação de inserção, atualização ou exclusão de um controle da Web de dados ASP.NET. W...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: dab291a0-a8b5-46fa-9dd8-3d35b249201f
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: a8c1388b73524a8bb918b67aa265db894c07636f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78609147"
---
# <a name="examining-the-events-associated-with-inserting-updating-and-deleting-c"></a>Examinar os eventos associados à inserção, atualização e exclusão (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_17_CS.exe) ou [baixar PDF](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/datatutorial17cs1.pdf)

> Neste tutorial, examinaremos o uso dos eventos que ocorrem antes, durante e após uma operação de inserção, atualização ou exclusão de um controle da Web de dados ASP.NET. Também veremos como personalizar a interface de edição para atualizar apenas um subconjunto dos campos Product.

## <a name="introduction"></a>Introdução

Ao usar os recursos internos de inserção, edição ou exclusão dos controles GridView, DetailsView ou FormView, uma variedade de etapas se transformará quando o usuário final concluir o processo de adicionar um novo registro ou atualizar ou excluir um registro existente. Como discutimos no [tutorial anterior](an-overview-of-inserting-updating-and-deleting-data-cs.md), quando uma linha é editada no GridView, o botão Editar é substituído pelos botões atualizar e cancelar e os boundfields se transformam em caixas de Text. Depois que o usuário final atualizar os dados e clicar em atualizar, as etapas a seguir serão executadas no postback:

1. O GridView preenche sua `UpdateParameters` de ObjectDataSource com os campos de identificação exclusivos do registro editado (por meio da propriedade `DataKeyNames`) junto com os valores inseridos pelo usuário
2. O GridView invoca o método de `Update()` de ObjectDataSource que, por sua vez, invoca o método apropriado no objeto subjacente (`ProductsDAL.UpdateProduct`, em nosso tutorial anterior)
3. Os dados subjacentes, que agora incluem as alterações atualizadas, são reassociados ao GridView

Durante essa sequência de etapas, vários eventos são disparados, permitindo criar manipuladores de eventos para adicionar lógica personalizada quando necessário. Por exemplo, antes da etapa 1, o evento `RowUpdating` do GridView é acionado. Podemos, neste ponto, cancelar a solicitação de atualização se houver algum erro de validação. Quando o método de `Update()` é invocado, o evento de `Updating` de ObjectDataSource é acionado, fornecendo a oportunidade de adicionar ou personalizar os valores de qualquer um dos `UpdateParameters`. Depois que o método do objeto subjacente do ObjectDataSource tiver concluído a execução, o evento de `Updated` do ObjectDataSource será gerado. Um manipulador de eventos para o evento `Updated` pode inspecionar os detalhes sobre a operação de atualização, como quantas linhas foram afetadas e se uma exceção ocorreu ou não. Finalmente, após a etapa 2, o evento `RowUpdated` do GridView é acionado; um manipulador de eventos para esse evento pode examinar informações adicionais sobre a operação de atualização que acabou de ser executada.

A Figura 1 descreve essa série de eventos e etapas ao atualizar um GridView. O padrão de evento na Figura 1 não é exclusivo para atualizar com um GridView. Inserir, atualizar ou excluir dados do GridView, DetailsView ou FormView precipitates a mesma sequência de eventos de pré e pós-nível para o controle da Web de dados e o ObjectDataSource.

[![uma série de pré e pós-eventos acionados ao atualizar dados em um GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image2.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image1.png)

**Figura 1**: uma série de pré e pós-eventos acionados ao atualizar dados em um GridView ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image3.png))

Neste tutorial, examinaremos o uso desses eventos para estender os recursos internos de inserção, atualização e exclusão dos controles da Web de dados ASP.NET. Também veremos como personalizar a interface de edição para atualizar apenas um subconjunto dos campos Product.

## <a name="step-1-updating-a-productsproductnameandunitpricefields"></a>Etapa 1: atualizando os campos de`ProductName`e`UnitPrice`de um produto

Nas interfaces de edição do tutorial anterior, *todos os* campos de produto que não eram somente leitura precisavam ser incluídos. Se tivéssemos de remover um campo do `QuantityPerUnit` de GridView – ao atualizar os dados, o controle da Web de dados não definirá o valor de `UpdateParameters` de `QuantityPerUnit` do ObjectDataSource. Em seguida, o ObjectDataSource passaria um valor de `null` para o método de camada de lógica de negócios (BLL) `UpdateProduct`, que alteraria a coluna de `QuantityPerUnit` do registro de banco de dados editado para um valor de `NULL`. Da mesma forma, se um campo obrigatório, como `ProductName`, for removido da interface de edição, a atualização falhará com uma exceção "a*coluna ' ProductName ' não permite valores nulos*". O motivo para esse comportamento era porque o ObjectDataSource foi configurado para chamar o método `UpdateProduct` da classe `ProductsBLL`, que era esperado um parâmetro de entrada para cada um dos campos Product. Portanto, a coleção de `UpdateParameters` do ObjectDataSource continha um parâmetro para cada um dos parâmetros de entrada do método.

Se quisermos fornecer um controle da Web de dados que permita ao usuário final atualizar apenas um subconjunto de campos, precisamos definir programaticamente os valores de `UpdateParameters` ausentes no manipulador de eventos `Updating` do ObjectDataSource ou criar e chamar um método BLL que espera apenas um subconjunto dos campos. Vamos explorar essa última abordagem.

Especificamente, vamos criar uma página que exibe apenas os campos `ProductName` e `UnitPrice` em um GridView editável. A interface de edição do GridView só permitirá que o usuário atualize os dois campos exibidos, `ProductName` e `UnitPrice`. Como essa interface de edição fornece apenas um subconjunto dos campos de um produto, precisamos criar um ObjectDataSource que usa o método de `UpdateProduct` de BLL existente e que tem os valores de campo de produto ausentes definidos programaticamente em seu manipulador de eventos `Updating`, ou precisamos criar um novo método BLL que espere apenas o subconjunto de campos definido no GridView. Para este tutorial, vamos usar a última opção e criar uma sobrecarga do método `UpdateProduct`, que usa apenas três parâmetros de entrada: `productName`, `unitPrice`e `productID`:

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample1.cs)]

Assim como o método de `UpdateProduct` original, essa sobrecarga começa verificando se há um produto no banco de dados com o `ProductID`especificado. Caso contrário, ele retornará `false`, indicando que a solicitação para atualizar as informações do produto falhou. Caso contrário, ele atualiza o `ProductName` do registro de produto existente e `UnitPrice` os campos de acordo e confirma a atualização chamando o método de `Update()` do TableAdapter, passando a instância de `ProductsRow`.

Com essa adição à nossa classe de `ProductsBLL`, estamos prontos para criar a interface GridView simplificada. Abra o `DataModificationEvents.aspx` na pasta `EditInsertDelete` e adicione um GridView à página. Crie um novo ObjectDataSource e configure-o para usar a classe `ProductsBLL` com seu mapeamento de método `Select()` para `GetProducts` e seu mapeamento de método `Update()` para a sobrecarga `UpdateProduct` que usa apenas os parâmetros de entrada `productName`, `unitPrice`e `productID`. A Figura 2 mostra o assistente para criar fonte de dados ao mapear o método de `Update()` do ObjectDataSource para a nova sobrecarga de método de `UpdateProduct` da classe `ProductsBLL`.

[![mapear o método Update () do ObjectDataSource para a nova sobrecarga UpdateProduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image5.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image4.png)

**Figura 2**: mapear o método de `Update()` do ObjectDataSource para a nova sobrecarga de `UpdateProduct` ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image6.png))

Como nosso exemplo inicialmente só precisará da capacidade de editar dados, mas não de inserir ou excluir registros, Reserve um momento para indicar explicitamente que os métodos de `Insert()` e `Delete()` do ObjectDataSource não devem ser mapeados para nenhum dos métodos da classe `ProductsBLL`, acessando as guias INSERT e DELETE e escolhendo (None) na lista suspensa.

[![escolha (nenhum) na lista suspensa para as guias inserir e excluir](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image8.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image7.png)

**Figura 3**: escolha (nenhum) na lista suspensa para as guias inserir e excluir ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image9.png))

Depois de concluir este assistente, marque a caixa de seleção Habilitar edição na marca inteligente do GridView.

Com a conclusão do assistente para criar fonte de dados e a ligação com o GridView, o Visual Studio criou a sintaxe declarativa para ambos os controles. Acesse a exibição de código-fonte para inspecionar a marcação declarativa do ObjectDataSource, que é mostrada abaixo:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample2.aspx)]

Como não há mapeamentos para os métodos `Insert()` e `Delete()` do ObjectDataSource, não há seções `InsertParameters` ou `DeleteParameters`. Além disso, como o método `Update()` é mapeado para a sobrecarga do método `UpdateProduct` que aceita apenas três parâmetros de entrada, a seção `UpdateParameters` tem apenas três instâncias `Parameter`.

Observe que a propriedade `OldValuesParameterFormatString` do ObjectDataSource está definida como `original_{0}`. Essa propriedade é definida automaticamente pelo Visual Studio ao usar o assistente para configurar fonte de dados. No entanto, como nossos métodos de BLL não esperam que o valor de `ProductID` original seja passado, remova essa atribuição de propriedade completamente da sintaxe declarativa do ObjectDataSource.

> [!NOTE]
> Se você simplesmente desmarcar o valor da propriedade `OldValuesParameterFormatString` da janela Propriedades no modo de exibição de Design, a propriedade ainda existirá na sintaxe declarativa, mas será definida como uma cadeia de caracteres vazia. Remova a propriedade completamente da sintaxe declarativa ou, na janela Propriedades, defina o valor como o padrão, `{0}`.

Embora o ObjectDataSource tenha apenas `UpdateParameters` para o nome, o preço e a ID do produto, o Visual Studio adicionou um BoundField ou CheckBoxField no GridView para cada um dos campos do produto.

[![o GridView contém um BoundField ou CheckBoxField para cada um dos campos do produto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image11.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image10.png)

**Figura 4**: o GridView contém um BoundField ou CheckBoxField para cada um dos campos do produto ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image12.png))

Quando o usuário final edita um produto e clica no botão atualizar, o GridView enumera os campos que não eram somente leitura. Em seguida, ele define o valor do parâmetro correspondente na coleção de `UpdateParameters` do ObjectDataSource para o valor inserido pelo usuário. Se não houver um parâmetro correspondente, o GridView adicionará um à coleção. Portanto, se nosso GridView contiver BoundFields e CheckBoxFields para todos os campos do produto, o ObjectDataSource acabará invocando a sobrecarga de `UpdateProduct` que usa todos esses parâmetros, apesar do fato de que a marcação declarativa do ObjectDataSource especifica apenas três parâmetros de entrada (consulte a Figura 5). Da mesma forma, se houver alguma combinação de campos de produto não somente leitura no GridView que não corresponde aos parâmetros de entrada para uma sobrecarga de `UpdateProduct`, uma exceção será gerada ao tentar atualizar.

[![GridView adicionará parâmetros à coleção UpdateParameters do ObjectDataSource](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image14.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image13.png)

**Figura 5**: o GridView adicionará parâmetros à coleção de `UpdateParameters` do ObjectDataSource ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image15.png))

Para garantir que o ObjectDataSource invoque a sobrecarga de `UpdateProduct` que usa apenas o nome, o preço e a ID do produto, precisamos restringir o GridView a ter campos editáveis apenas para o `ProductName` e `UnitPrice`. Isso pode ser feito removendo os outros BoundFields e CheckBoxFields, definindo os outros campos de `ReadOnly` Propriedade como `true`ou por alguma combinação dos dois. Para este tutorial, vamos simplesmente remover todos os campos GridView, exceto o `ProductName` e `UnitPrice` BoundFields, após o qual a marcação declarativa do GridView será semelhante a:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample3.aspx)]

Embora a sobrecarga de `UpdateProduct` Espere três parâmetros de entrada, temos apenas dois BoundFields em nosso GridView. Isso ocorre porque o parâmetro de entrada `productID` é um valor de chave primária e passado pelo valor da propriedade `DataKeyNames` para a linha editada.

Nosso GridView, junto com a sobrecarga de `UpdateProduct`, permite que um usuário edite apenas o nome e o preço de um produto sem perder nenhum dos outros campos de produto.

[![a interface permite editar apenas o nome e o preço do produto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image17.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image16.png)

**Figura 6**: a interface permite editar apenas o nome e o preço do produto ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image18.png))

> [!NOTE]
> Conforme discutido no tutorial anterior, é extremamente importante que o estado de exibição de GridView s seja habilitado (o comportamento padrão). Se você definir a Propriedade GridView s `EnableViewState` como `false`, correrá o risco de ter usuários simultâneos excluindo ou editando os registros de forma involuntária. Consulte [AVISO: problema de simultaneidade com ASP.NET 2,0 GridViews/DetailsView/formviews que dão suporte à edição e/ou exclusão e cujo estado de exibição está desabilitado](http://scottonwriting.net/sowblog/archive/2006/10/03/163215.aspx) para obter mais informações.

## <a name="improving-theunitpriceformatting"></a>Aprimorando a formatação de`UnitPrice`

Embora o exemplo de GridView mostrado na Figura 6 funcione, o campo `UnitPrice` não está formatado, resultando em uma exibição de preço que não tem nenhum símbolo de moeda e tem quatro casas decimais. Para aplicar uma formatação de moeda para as linhas não editáveis, basta definir a propriedade `DataFormatString` de `UnitPrice` BoundField como `{0:c}` e sua propriedade `HtmlEncode` como `false`.

[![definir as propriedades DataFormatString e HtmlEncode de UnitPrice adequadamente](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image20.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image19.png)

**Figura 7**: definir as propriedades de `DataFormatString` e `HtmlEncode` do `UnitPrice`de[forma adequada (clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image21.png))

Com essa alteração, as linhas não editáveis formatam o preço como uma moeda; a linha editada, no entanto, ainda exibe o valor sem o símbolo de moeda e com quatro casas decimais.

[![linhas não editáveis agora estão formatadas como valores de moeda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image23.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image22.png)

**Figura 8**: linhas não editáveis agora são formatadas como valores de moeda ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image24.png))

As instruções de formatação especificadas na propriedade `DataFormatString` podem ser aplicadas à interface de edição definindo a propriedade `ApplyFormatInEditMode` de BoundField como `true` (o padrão é `false`). Reserve um momento para definir essa propriedade como `true`.

[![definir a propriedade ApplyFormatInEditMode do UnitPrice BoundField como true](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image26.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image25.png)

**Figura 9**: definir a propriedade `ApplyFormatInEditMode` de `UnitPrice` BoundField como `true` ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image27.png))

Com essa alteração, o valor da `UnitPrice` exibido na linha editada também é formatado como uma moeda.

[![o valor PreçoUnitário da linha editada agora está formatado como uma moeda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image29.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image28.png)

**Figura 10**: o valor de `UnitPrice` da linha editado agora está formatado como uma moeda ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image30.png))

No entanto, a atualização de um produto com o símbolo de moeda na caixa de texto, como $19, gera um `FormatException`. Quando o GridView tenta atribuir os valores fornecidos pelo usuário à coleção de `UpdateParameters` do ObjectDataSource, ele não consegue converter a cadeia de caracteres de `UnitPrice` "$19" no `decimal` exigido pelo parâmetro (consulte a Figura 11). Para corrigir isso, podemos criar um manipulador de eventos para o evento `RowUpdating` do GridView e fazer com que ele analise o `UnitPrice` fornecido pelo usuário como uma `decimal`formatada por moeda.

O evento `RowUpdating` do GridView aceita como seu segundo parâmetro um objeto do tipo [GridViewUpdateEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdateeventargs(VS.80).aspx), que inclui um dicionário `NewValues` como uma de suas propriedades que mantém os valores fornecidos pelo usuário prontos para serem atribuídos à coleção de `UpdateParameters` do ObjectDataSource. Podemos substituir o valor de `UnitPrice` existente na coleção de `NewValues` com um valor decimal analisado usando o formato de moeda com as seguintes linhas de código no manipulador de eventos `RowUpdating`:

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample4.cs)]

Se o usuário tiver fornecido um valor de `UnitPrice` (como "$19"), esse valor será substituído pelo valor decimal calculado por [decimal. Parse](https://msdn.microsoft.com/library/system.decimal.parse(VS.80).aspx), analisando o valor como uma moeda. Isso analisará corretamente o decimal no caso de qualquer símbolo de moeda, vírgulas, pontos decimais e assim por diante, e usará a [Enumeração NumberStyles](https://msdn.microsoft.com/library/system.globalization.numberstyles(VS.80).aspx) no namespace [System. Globalization](https://msdn.microsoft.com/library/abeh092z(VS.80).aspx) .

A Figura 11 mostra o problema causado por símbolos de moeda no `UnitPrice`fornecido pelo usuário, juntamente com como o manipulador de eventos de `RowUpdating` do GridView pode ser utilizado para analisar corretamente essa entrada.

[![o valor PreçoUnitário da linha editada agora está formatado como uma moeda](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image32.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image31.png)

**Figura 11**: o valor de `UnitPrice` da linha editado agora está formatado como uma moeda ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image33.png))

## <a name="step-2-prohibitingnull-unitprices"></a>Etapa 2: proibindo`NULL UnitPrices`

Enquanto o banco de dados está configurado para permitir `NULL` valores na coluna `UnitPrice` da tabela `Products`, podemos querer impedir que os usuários que visitam essa página específica especifiquem um valor de `UnitPrice` de `NULL`. Ou seja, se um usuário não inserir um valor de `UnitPrice` ao editar uma linha de produto, em vez de salvar os resultados no banco de dados, queremos exibir uma mensagem informando ao usuário que, por meio dessa página, todos os produtos editados devem ter um preço especificado.

O objeto `GridViewUpdateEventArgs` passado para o manipulador de eventos `RowUpdating` do GridView contém uma propriedade `Cancel` que, se definida como `true`, encerra o processo de atualização. Vamos estender o manipulador de eventos `RowUpdating` para definir `e.Cancel` como `true` e exibir uma mensagem explicando por que se o valor de `UnitPrice` na coleção de `NewValues` for `null`.

Comece adicionando um controle de rótulo da Web à página chamada `MustProvideUnitPriceMessage`. Esse controle rótulo será exibido se o usuário não conseguir especificar um valor de `UnitPrice` ao atualizar um produto. Defina a propriedade `Text` do rótulo como "você deve fornecer um preço para o produto". Também criei uma nova classe CSS em `Styles.css` chamada `Warning` com a seguinte definição:

[!code-css[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample5.css)]

Por fim, defina a propriedade `CssClass` do rótulo como `Warning`. Neste ponto, o designer deve mostrar a mensagem de aviso em um tamanho de fonte grande vermelho, negrito, itálico e extra acima do GridView, conforme mostrado na Figura 12.

[![um rótulo foi adicionado acima do GridView](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image35.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image34.png)

**Figura 12**: um rótulo foi adicionado acima do GridView ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image36.png))

Por padrão, esse rótulo deve estar oculto, portanto, defina sua propriedade `Visible` como `false` no manipulador de eventos `Page_Load`:

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample6.cs)]

Se o usuário tentar atualizar um produto sem especificar o `UnitPrice`, desejamos cancelar a atualização e exibir o rótulo de aviso. Aumente o manipulador de eventos `RowUpdating` do GridView da seguinte maneira:

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample7.cs)]

Se um usuário tentar salvar um produto sem especificar um preço, a atualização será cancelada e uma mensagem útil será exibida. Embora o banco de dados (e a lógica de negócios) permita `NULL` `UnitPrice` s, essa página ASP.NET específica não.

[![um usuário não pode deixar o PreçoUnitário em branco](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image38.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image37.png)

**Figura 13**: um usuário não pode deixar `UnitPrice` em branco ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image39.png))

Até agora, vimos como usar o evento `RowUpdating` do GridView para alterar programaticamente os valores de parâmetro atribuídos à coleção de `UpdateParameters` do ObjectDataSource, bem como cancelar o processo de atualização. Esses conceitos são transferidos para os controles DetailsView e FormView e também se aplicam à inserção e exclusão.

Essas tarefas também podem ser feitas no nível de ObjectDataSource por meio de manipuladores de eventos para seus `Inserting`, `Updating`e eventos de `Deleting`. Esses eventos são acionados antes que o método associado do objeto subjacente seja invocado e forneça uma oportunidade de última chance para modificar a coleção de parâmetros de entrada ou cancelar a operação imediatamente. Os manipuladores de eventos para esses três eventos são passados para um objeto do tipo [ObjectDataSourceMethodEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs(VS.80).aspx) que tem duas propriedades de interesse:

- [Cancelar](https://msdn.microsoft.com/library/system.componentmodel.canceleventargs.cancel(VS.80).aspx), que, se for definido como `true`, cancelará a operação que está sendo executada
- [InputParameters](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasourcemethodeventargs.inputparameters(VS.80).aspx), que é a coleção de `InsertParameters`, `UpdateParameters`ou `DeleteParameters`, dependendo se o manipulador de eventos é para o evento `Inserting`, `Updating`ou `Deleting`

Para ilustrar como trabalhar com os valores de parâmetro no nível ObjectDataSource, vamos incluir um DetailsView em nossa página que permite que os usuários adicionem um novo produto. Este DetailsView será usado para fornecer uma interface para adicionar rapidamente um novo produto ao banco de dados. Para manter uma interface do usuário consistente ao adicionar um novo produto, vamos permitir que o usuário insira apenas valores para os campos `ProductName` e `UnitPrice`. Por padrão, os valores que não são fornecidos na interface de inserção de DetailsView serão definidos como um valor de banco de dados `NULL`. No entanto, podemos usar o evento `Inserting` do ObjectDataSource para injetar valores padrão diferentes, como veremos em breve.

## <a name="step-3-providing-an-interface-to-add-new-products"></a>Etapa 3: fornecer uma interface para adicionar novos produtos

Arraste um DetailsView da caixa de ferramentas para o designer acima do GridView, desmarque sua `Height` e `Width` Propriedades e associe-o ao ObjectDataSource já presente na página. Isso adicionará um BoundField ou CheckBoxField para cada um dos campos do produto. Como queremos usar este DetailsView para adicionar novos produtos, precisamos marcar a opção Habilitar inserção na marca inteligente; no entanto, não há essa opção porque o método de `Insert()` do ObjectDataSource não está mapeado para um método na classe `ProductsBLL` (Lembre-se de que definimos esse mapeamento para (nenhum) ao configurar a fonte de dados, consulte a Figura 3).

Para configurar o ObjectDataSource, selecione o link configurar fonte de dados em sua marca inteligente, iniciando o assistente. A primeira tela permite que você altere o objeto subjacente ao qual ObjectDataSource está associado; Deixe-o definido como `ProductsBLL`. A próxima tela lista os mapeamentos dos métodos ObjectDataSource para o objeto subjacente. Apesar de indicarmos explicitamente que os métodos `Insert()` e `Delete()` não devem ser mapeados para nenhum método, se você for para as guias inserir e excluir, verá que um mapeamento está lá. Isso ocorre porque os métodos `AddProduct` e `DeleteProduct` do `ProductsBLL`usam o atributo `DataObjectMethodAttribute` para indicar que eles são os métodos padrão para `Insert()` e `Delete()`, respectivamente. Portanto, o assistente ObjectDataSource seleciona essas vezes sempre que você executa o assistente, a menos que haja algum outro valor explicitamente especificado.

Deixe o método `Insert()` apontando para o método `AddProduct`, mas novamente defina a lista suspensa da guia excluir como (nenhum).

[![definir a lista suspensa da guia Inserir para o método addproduct](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image41.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image40.png)

**Figura 14**: definir a lista suspensa da guia Inserir para o método `AddProduct` ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image42.png))

[![definir a lista suspensa da guia excluir como (nenhum)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image44.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image43.png)

**Figura 15**: definir a lista suspensa da guia excluir para (nenhum) ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image45.png))

Depois de fazer essas alterações, a sintaxe declarativa do ObjectDataSource será expandida para incluir uma coleção de `InsertParameters`, conforme mostrado abaixo:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample8.aspx)]

A reexecução do assistente adicionou novamente a propriedade `OldValuesParameterFormatString`. Reserve um tempo para limpar essa propriedade definindo-a como o valor padrão (`{0}`) ou removendo-a completamente da sintaxe declarativa.

Com o ObjectDataSource fornecendo recursos de inserção, a marca inteligente de DetailsView agora incluirá a caixa de seleção Habilitar inserção; Retorne ao designer e marque essa opção. Em seguida, decrescente o DetailsView para que ele tenha apenas dois BoundFields-`ProductName` e `UnitPrice`-e o CommandField. Neste ponto, a sintaxe declarativa de DetailsView deve ser semelhante a:

[!code-aspx[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample9.aspx)]

A Figura 16 mostra essa página quando exibida por meio de um navegador neste ponto. Como você pode ver, o DetailsView lista o nome e o preço do primeiro produto (Chai). No entanto, o que queremos, é uma interface de inserção que fornece um meio para o usuário adicionar rapidamente um novo produto ao banco de dados.

[![o DetailsView está atualmente processado no modo somente leitura](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image47.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image46.png)

**Figura 16**: o DetailsView é processado no momento no modo somente leitura ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image48.png))

Para mostrar o DetailsView em seu modo de inserção, precisamos definir a propriedade `DefaultMode` como `Inserting`. Isso renderiza o DetailsView no modo de inserção quando visitado pela primeira vez e o mantém lá após a inserção de um novo registro. Como mostra a figura 17, tal DetailsView fornece uma interface rápida para adicionar um novo registro.

[![o DetailsView fornece uma interface para adicionar rapidamente um novo produto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image50.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image49.png)

**Figura 17**: o DetailsView fornece uma interface para adicionar rapidamente um novo produto ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image51.png))

Quando o usuário insere um nome de produto e um preço (como "Acme aquático" e 1,99, como na figura 17) e clica em inserir, um postback massacre e o fluxo de trabalho de inserção são iniciados, culminando em um novo registro de produto que está sendo adicionado ao banco de dados. O DetailsView mantém sua interface de inserção e o GridView é reassociado automaticamente à sua fonte de dados para incluir o novo produto, como mostra a Figura 18.

![O produto](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image52.png)

**Figura 18**: o produto "Acme aquático" foi adicionado ao banco de dados

Embora o GridView na Figura 18 não o mostre, os campos de produto não contidos na interface DetailsView `CategoryID`, `SupplierID`, `QuantityPerUnit`e assim por diante são atribuídos `NULL` valores de banco de dados. Você pode ver isso executando as seguintes etapas:

1. Vá para o Gerenciador de Servidores no Visual Studio
2. Expandindo o nó do banco de dados `NORTHWND.MDF`
3. Clique com o botão direito do mouse no nó da tabela de banco de dados `Products`
4. Selecionar Mostrar dados da tabela

Isso listará todos os registros na tabela `Products`. Como mostra a Figura 19, todas as colunas de nosso novo produto diferentes de `ProductID`, `ProductName`e `UnitPrice` têm valores de `NULL`.

[![os campos de produto não fornecidos no DetailsView recebem valores nulos](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image54.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image53.png)

**Figura 19**: os campos de produto não fornecidos no DetailsView são atribuídos `NULL` valores ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image55.png))

Talvez queiramos fornecer um valor padrão diferente de `NULL` para um ou mais desses valores de coluna, porque `NULL` não é a melhor opção padrão ou porque a própria coluna de banco de dados não permite `NULL` s. Para fazer isso, podemos definir programaticamente os valores dos parâmetros da coleção de `InputParameters` do DetailsView. Essa atribuição pode ser feita no manipulador de eventos para o evento `ItemInserting` do DetailsView ou o evento `Inserting` do ObjectDataSource. Como já vimos usar os eventos de pré e pós-nível no nível de controle da Web de dados, vamos explorar o uso dos eventos do ObjectDataSource desta vez.

## <a name="step-4-assigning-values-to-thecategoryidandsupplieridparameters"></a>Etapa 4: atribuindo valores aos parâmetros`CategoryID`e`SupplierID`

Para este tutorial, vamos imaginar que, para nosso aplicativo ao adicionar um novo produto por meio dessa interface, deve ser atribuído um `CategoryID` e `SupplierID` valor de 1. Como mencionado anteriormente, o ObjectDataSource tem um par de eventos de pré e pós-nível que são acionados durante o processo de modificação de dados. Quando seu método de `Insert()` é invocado, o ObjectDataSource primeiro gera seu evento `Inserting`, em seguida, chama o método ao qual seu método `Insert()` foi mapeado e, por fim, gera o evento `Inserted`. O manipulador de eventos `Inserting` nos dá uma última oportunidade de ajustar os parâmetros de entrada ou cancelar a operação imediatamente.

> [!NOTE]
> Em um aplicativo do mundo real, você provavelmente desejaria permitir que o usuário especifique a categoria e o fornecedor ou escolheria esse valor para eles com base em alguns critérios ou lógica de negócios (em vez de selecionar de forma oculta uma ID de 1). Independentemente disso, o exemplo ilustra como definir programaticamente o valor de um parâmetro de entrada do evento de pré nível do ObjectDataSource.

Reserve um tempo para criar um manipulador de eventos para o evento de `Inserting` do ObjectDataSource. Observe que o segundo parâmetro de entrada do manipulador de eventos é um objeto do tipo `ObjectDataSourceMethodEventArgs`, que tem uma propriedade para acessar a coleção de parâmetros (`InputParameters`) e uma propriedade para cancelar a operação (`Cancel`).

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample10.cs)]

Neste ponto, a propriedade `InputParameters` contém a coleção de `InsertParameters` do ObjectDataSource com os valores atribuídos de DetailsView. Para alterar o valor de um desses parâmetros, basta usar: `e.InputParameters["paramName"] = value`. Portanto, para definir o `CategoryID` e `SupplierID` com valores de 1, ajuste o manipulador de eventos `Inserting` para que fique semelhante ao seguinte:

[!code-csharp[Main](examining-the-events-associated-with-inserting-updating-and-deleting-cs/samples/sample11.cs)]

Desta vez, ao adicionar um novo produto (como a Acme soda), as colunas `CategoryID` e `SupplierID` do novo produto são definidas como 1 (consulte a figura 20).

[![novos produtos agora têm seus valores CategoryID e CódigoDoFornecedor definidos como 1](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image57.png)](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image56.png)

**Figura 20**: os novos produtos agora têm seus `CategoryID` e `SupplierID` valores definidos como 1 ([clique para exibir a imagem em tamanho normal](examining-the-events-associated-with-inserting-updating-and-deleting-cs/_static/image58.png))

## <a name="summary"></a>Resumo

Durante o processo de edição, inserção e exclusão, o controle da Web de dados e o ObjectDataSource passam por vários eventos de pré e pós-nível. Neste tutorial, examinamos os eventos de nível anterior e vimos como usá-los para personalizar os parâmetros de entrada ou cancelar a operação de modificação de dados no controle da Web de dados e nos eventos de ObjectDataSource. No próximo tutorial, vamos examinar a criação e o uso de manipuladores de eventos para os eventos de nível posterior.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Jackie Goor e Liz Shulok. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](an-overview-of-inserting-updating-and-deleting-data-cs.md)
> [Próximo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md)
