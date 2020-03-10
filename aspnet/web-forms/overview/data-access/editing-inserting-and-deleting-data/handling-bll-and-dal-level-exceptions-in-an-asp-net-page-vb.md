---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
title: Manipulando exceções de nível de BLL e DAL em uma página ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como exibir uma mensagem de erro amigável e informativa caso ocorra uma exceção durante uma operação de inserção, atualização ou exclusão de...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 129d4338-1315-4f40-89b5-2b84b807707d
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb
msc.type: authoredcontent
ms.openlocfilehash: ee277596ade18d2603892d134b47c2c8697836bb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78592305"
---
# <a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-vb"></a>Tratar de exceções de nível BLL e DAL em uma página do ASP.NET (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_VB.exe) ou [baixar PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/datatutorial18vb1.pdf)

> Neste tutorial, veremos como exibir uma mensagem de erro amigável e informativa caso ocorra uma exceção durante uma operação de inserção, atualização ou exclusão de um controle da Web de dados ASP.NET.

## <a name="introduction"></a>Introdução

Trabalhar com dados de um aplicativo Web ASP.NET usando uma arquitetura de aplicativo em camadas envolve as três etapas gerais a seguir:

1. Determine qual método da camada de lógica de negócios precisa ser invocado e quais valores de parâmetro devem ser passados. Os valores de parâmetro podem ser embutidos em código, atribuídos por meio de programação ou entradas inseridas pelo usuário.
2. Invoque o método.
3. Processar os resultados. Ao chamar um método BLL que retorna dados, isso pode envolver a vinculação de dados a um controle da Web de dados. Para métodos de BLL que modificam dados, isso pode incluir a execução de alguma ação com base em um valor de retorno ou tratamento normal de qualquer exceção que surgiu na etapa 2.

Como vimos no [tutorial anterior](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md), o ObjectDataSource e os controles da Web de dados fornecem pontos de extensibilidade para as etapas 1 e 3. O GridView, por exemplo, dispara seu evento de `RowUpdating` antes de atribuir seus valores de campo à coleção de `UpdateParameters` do ObjectDataSource; seu evento de `RowUpdated` é gerado depois que ObjectDataSource conclui a operação.

Já examinamos os eventos que são acionados durante a etapa 1 e vimos como eles podem ser usados para personalizar os parâmetros de entrada ou cancelar a operação. Neste tutorial, vamos transformar nossa atenção nos eventos que são acionados após a conclusão da operação. Com esses manipuladores de eventos de postback, podemos, entre outras coisas, determinar se ocorreu uma exceção durante a operação e tratá-la normalmente, exibindo uma mensagem de erro amigável e informativa na tela, em vez de padronizar para a ASP.NET padrão página de exceção.

Para ilustrar como trabalhar com esses eventos de nível posterior, vamos criar uma página que lista os produtos em um GridView editável. Ao atualizar um produto, se uma exceção for gerada, nossa página ASP.NET exibirá uma mensagem curta acima do GridView explicando que ocorreu um problema. Vamos começar!

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Etapa 1: Criando um GridView editável dos produtos

No tutorial anterior, criamos um GridView editável com apenas dois campos, `ProductName` e `UnitPrice`. Isso exigia a criação de uma sobrecarga adicional para o método `UpdateProduct` da classe `ProductsBLL`, uma que aceitasse apenas três parâmetros de entrada (o nome do produto, o preço unitário e a ID), em oposição a um parâmetro para cada campo de produto. Para este tutorial, vamos praticar essa técnica novamente, criando um GridView editável que exibe o nome do produto, a quantidade por unidade, o preço unitário e as unidades em estoque, mas só permite o nome, o preço unitário e as unidades em estoque a serem editadas.

Para acomodar esse cenário, precisaremos de outra sobrecarga do método `UpdateProduct`, uma que aceite quatro parâmetros: o nome do produto, o preço unitário, as unidades em estoque e a ID. Adicione o seguinte método à classe `ProductsBLL`:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample1.vb)]

Com esse método concluído, estamos prontos para criar a página ASP.NET que permite editar esses quatro campos de produto específicos. Abra a página `ErrorHandling.aspx` na pasta `EditInsertDelete` e adicione um GridView à página por meio do designer. Associe o GridView a um novo ObjectDataSource, mapeando o método `Select()` para o método `GetProducts()` da classe `ProductsBLL` e o método `Update()` para a sobrecarga de `UpdateProduct` recém-criada.

[![usar a sobrecarga do método UpdateProduct que aceita quatro parâmetros de entrada](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image1.png)

**Figura 1**: usar a sobrecarga do método de `UpdateProduct` que aceita quatro parâmetros de entrada ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image3.png))

Isso criará um ObjectDataSource com uma coleção de `UpdateParameters` com quatro parâmetros e um GridView com um campo para cada um dos campos Product. A marcação declarativa do ObjectDataSource atribui a propriedade `OldValuesParameterFormatString` o valor `original_{0}`, que causará uma exceção, uma vez que nossa classe BLL não espera que um parâmetro de entrada chamado `original_productID` seja passado. Não se esqueça de remover essa configuração completamente da sintaxe declarativa (ou defini-la como o valor padrão, `{0}`).

Em seguida, decrescente do GridView para incluir apenas os `ProductName`, `QuantityPerUnit`, `UnitPrice`e `UnitsInStock` BoundFields. Também sinta-se à vontade para aplicar qualquer formatação em nível de campo que você julgar necessário (como alterar as propriedades de `HeaderText`).

No tutorial anterior, vimos como formatar o `UnitPrice` BoundField como uma moeda no modo somente leitura e no modo de edição. Vamos fazer o mesmo aqui. Lembre-se de que isso exigiu a configuração da propriedade `DataFormatString` de BoundField como `{0:c}`, sua propriedade `HtmlEncode` como `false`e sua `ApplyFormatInEditMode` para `true`, como mostra a Figura 2.

[![configurar o UnitPrice BoundField para exibir como uma moeda](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image4.png)

**Figura 2**: configurar o `UnitPrice` BoundField para exibir como uma moeda ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image6.png))

A formatação da `UnitPrice` como uma moeda na interface de edição requer a criação de um manipulador de eventos para o evento `RowUpdating` do GridView que analisa a cadeia de caracteres formatada em moeda em um valor `decimal`. Lembre-se de que o manipulador de eventos `RowUpdating` do último tutorial também verificou para garantir que o usuário forneceu um valor de `UnitPrice`. No entanto, para este tutorial, vamos permitir que o usuário omita o preço.

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample2.vb)]

Nosso GridView inclui um `QuantityPerUnit` BoundField, mas esse BoundField deve ser apenas para fins de exibição e não deve ser editável pelo usuário. Para organizar isso, basta definir a propriedade `ReadOnly` de BoundFields como `true`.

[![tornar o QuantityPerUnit BoundField somente leitura](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image7.png)

**Figura 3**: tornar o `QuantityPerUnit` BoundField somente leitura ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image9.png))

Por fim, marque a caixa de seleção Habilitar edição na marca inteligente do GridView. Depois de concluir essas etapas, o designer da página `ErrorHandling.aspx` deve ser semelhante à figura 4.

[![remover tudo, exceto os BoundFields necessários e marcar a caixa de seleção Habilitar edição](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image10.png)

**Figura 4**: remover tudo, exceto os boundfields necessários e marcar a caixa de seleção Habilitar edição ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image12.png))

Neste ponto, temos uma lista de todos os campos `ProductName`, `QuantityPerUnit`, `UnitPrice`e `UnitsInStock` dos produtos; no entanto, somente os campos `ProductName`, `UnitPrice`e `UnitsInStock` podem ser editados.

[![os usuários agora podem editar facilmente os nomes, os preços e as unidades dos produtos em campos de estoque](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image13.png)

**Figura 5**: os usuários agora podem editar facilmente os nomes, os preços e as unidades dos produtos em campos de estoque ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image15.png))

## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Etapa 2: lidando normalmente com exceções no nível da DAL

Embora o GridView editável funcione de forma maravilhosa quando os usuários inserem valores válidos para o nome, o preço e as unidades do produto editado no estoque, inserir valores ilegais resulta em uma exceção. Por exemplo, omitir o valor de `ProductName` faz com que um [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) seja gerado desde que a propriedade `ProductName` na classe `ProductsRow` tenha sua propriedade `AllowDBNull` definida como `false`; Se o banco de dados estiver inoperante, um `SqlException` será gerado pelo TableAdapter ao tentar se conectar ao banco de dados. Sem realizar nenhuma ação, essas exceções se comparam da camada de acesso a dados à camada de lógica de negócios e, em seguida, à página ASP.NET e, finalmente, ao tempo de execução do ASP.NET.

Dependendo de como seu aplicativo Web está configurado e se você está ou não visitando o aplicativo de `localhost`, uma exceção sem tratamento pode resultar em uma página de erro de servidor genérica, um relatório de erro detalhado ou uma página da Web amigável. Consulte [tratamento de erros do aplicativo Web em ASP.net](http://www.15seconds.com/issue/030102.htm) e o [elemento customErrors](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) para obter mais informações sobre como o tempo de execução do ASP.net responde a uma exceção não percebida.

A Figura 6 mostra a tela encontrada ao tentar atualizar um produto sem especificar o valor de `ProductName`. Este é o relatório de erro detalhado padrão exibido ao chegar ao `localhost`.

[![omitir o nome do produto exibirá os detalhes da exceção](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image16.png)

**Figura 6**: omitir o nome do produto exibirá detalhes da exceção ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image18.png))

Embora esses detalhes de exceção sejam úteis ao testar um aplicativo, apresentar um usuário final com uma tela como essa na face de uma exceção é menor do que o ideal. Um usuário final provavelmente não sabe o que é um `NoNullAllowedException` ou por que ele foi causado. Uma abordagem melhor é apresentar ao usuário uma mensagem mais amigável, explicando que houve problemas ao tentar atualizar o produto.

Se ocorrer uma exceção durante a execução da operação, os eventos de postback no controle da Web de ObjectDataSource e de dados fornecerão um meio para detectá-lo e cancelar a exceção de bolha até o tempo de execução do ASP.NET. Para nosso exemplo, vamos criar um manipulador de eventos para o evento `RowUpdated` do GridView que determina se uma exceção foi disparada e, em caso afirmativo, exibe os detalhes da exceção em um controle de rótulo da Web.

Comece adicionando um rótulo à página ASP.NET, definindo sua propriedade `ID` como `ExceptionDetails` e desmarcando sua propriedade `Text`. Para chamar o olho do usuário para essa mensagem, defina sua propriedade `CssClass` como `Warning`, que é uma classe CSS que adicionamos ao arquivo `Styles.css` no tutorial anterior. Lembre-se de que essa classe CSS faz com que o texto do rótulo seja exibido em uma fonte grande em vermelho, itálico, negrito e extra.

[![adicionar um controle de rótulo da Web à página](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image19.png)

**Figura 7**: adicionar um controle de rótulo da Web à página ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image21.png))

Como queremos que esse controle da Web de rótulo seja visível apenas imediatamente após a ocorrência de uma exceção, defina sua propriedade `Visible` como false no manipulador de eventos `Page_Load`:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample3.vb)]

Com esse código, na primeira página, visite e posteriores postbacks o controle de `ExceptionDetails` terá sua propriedade `Visible` definida como `false`. Diante de uma exceção de nível DAL ou BLL, que podemos detectar no manipulador de eventos `RowUpdated` do GridView, definiremos a propriedade `Visible` do controle de `ExceptionDetails` como true. Como os manipuladores de eventos de controle da Web ocorrem após o manipulador de eventos `Page_Load` no ciclo de vida da página, o rótulo será mostrado. No entanto, no próximo postback, o manipulador de eventos `Page_Load` reverterá a propriedade `Visible` de volta para `false`, ocultando-a da exibição novamente.

> [!NOTE]
> Como alternativa, poderíamos remover a necessidade de definir a propriedade `Visible` do controle de `ExceptionDetails` no `Page_Load` atribuindo sua propriedade `Visible` `false` na sintaxe declarativa e desabilitando seu estado de exibição (definindo sua propriedade `EnableViewState` como `false`). Usaremos essa abordagem alternativa em um tutorial futuro.

Com o controle rótulo adicionado, nossa próxima etapa é criar o manipulador de eventos para o evento `RowUpdated` do GridView. Selecione o GridView no designer, vá para o janela Propriedades e clique no ícone de raio, listando os eventos do GridView. Já deve existir uma entrada para o evento `RowUpdating` do GridView, pois criamos um manipulador de eventos para esse evento anteriormente neste tutorial. Crie um manipulador de eventos para o evento `RowUpdated` também.

![Criar um manipulador de eventos para o evento de userupdated do GridView](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image22.png)

**Figura 8**: criar um manipulador de eventos para o evento `RowUpdated` do GridView

> [!NOTE]
> Você também pode criar o manipulador de eventos por meio das listas suspensas na parte superior do arquivo de classe code-behind. Selecione o GridView na lista suspensa à esquerda e o evento `RowUpdated` do lado do lado direito.

A criação desse manipulador de eventos adicionará o seguinte código à classe code-behind da página ASP.NET:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample4.vb)]

O segundo parâmetro de entrada do manipulador de eventos é um objeto do tipo [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), que tem três propriedades de interesse para lidar com exceções:

- `Exception` uma referência à exceção gerada; Se nenhuma exceção tiver sido gerada, essa propriedade terá um valor de `null`
- `ExceptionHandled` um valor booliano que indica se a exceção foi tratada ou não no manipulador de eventos `RowUpdated`; se `false` (o padrão), a exceção será relançada, percolating até o tempo de execução do ASP.NET
- `KeepInEditMode` se definido como `true` a linha GridView editada permanece no modo de edição; se `false` (o padrão), a linha GridView reverterá para seu modo somente leitura

Nosso código, em seguida, deve verificar se `Exception` não está `null`, o que significa que uma exceção foi gerada durante a execução da operação. Se esse for o caso, queremos:

- Exibir uma mensagem amigável no rótulo de `ExceptionDetails`
- Indicar que a exceção foi tratada
- Manter a linha GridView no modo de edição

Este código a seguir realiza esses objetivos:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample5.vb)]

Esse manipulador de eventos começa verificando se `e.Exception` está `null`. Se não for, a propriedade `Visible` do rótulo de `ExceptionDetails` será definida como `true` e sua propriedade `Text` como "ocorreu um problema ao atualizar o produto". Os detalhes da exceção real que foi gerada residem na propriedade `InnerException` do objeto de `e.Exception`. Essa exceção interna é examinada e, se for de um tipo específico, uma mensagem útil adicional será acrescentada à propriedade `Text` do rótulo de `ExceptionDetails`. Por fim, as propriedades `ExceptionHandled` e `KeepInEditMode` são definidas como `true`.

A Figura 9 mostra uma captura de tela desta página ao omitir o nome do produto; A Figura 10 mostra os resultados ao inserir um valor de `UnitPrice` ilegal (-50).

[![a BoundField do NomeDoProduto deve conter um valor](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image23.png)

**Figura 9**: o `ProductName` BoundField deve conter um valor ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image25.png))

[![valores PreçoUnitário negativos não são permitidos](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image26.png)

**Figura 10**: valores de `UnitPrice` negativos não são permitidos ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image28.png))

Ao definir a propriedade `e.ExceptionHandled` como `true`, o manipulador de eventos `RowUpdated` indicou que ele tratou a exceção. Portanto, a exceção não propagará até o tempo de execução do ASP.NET.

> [!NOTE]
> As figuras 9 e 10 mostram uma maneira normal de lidar com exceções geradas devido à entrada inválida do usuário. O ideal é que, no entanto, essa entrada inválida nunca alcance a camada de lógica de negócios em primeiro lugar, pois a página ASP.NET deve garantir que as entradas do usuário sejam válidas antes de invocar o método `UpdateProduct` da classe `ProductsBLL`. Em nosso próximo tutorial, veremos como adicionar controles de validação às interfaces de edição e inserção para garantir que os dados enviados para a camada de lógica de negócios estejam em conformidade com as regras de negócio. Os controles de validação não apenas impedem a invocação do método `UpdateProduct` até que os dados fornecidos pelo usuário sejam válidos, mas também forneçam uma experiência de usuário mais informativa para identificar problemas de entrada de dados.

## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Etapa 3: lidando normalmente com exceções de nível de BLL

Ao inserir, atualizar ou excluir dados, a camada de acesso a dados pode lançar uma exceção na face de um erro relacionado a dados. O banco de dados pode estar offline, uma coluna de tabela de banco de dados necessária pode não ter um valor especificado ou uma restrição de nível de tabela pode ter sido violada. Além de exceções estritamente relacionadas a dados, a camada de lógica de negócios pode usar exceções para indicar quando as regras de negócio foram violadas. No tutorial [criando uma camada de lógica de negócios](../introduction/creating-a-business-logic-layer-vb.md) , por exemplo, adicionamos uma verificação de regra de negócio à sobrecarga de `UpdateProduct` original. Especificamente, se o usuário estava marcando um produto como descontinuado, exigimos que o produto não fosse o único fornecido por seu fornecedor. Se essa condição foi violada, um `ApplicationException` foi lançado.

Para a sobrecarga de `UpdateProduct` criada neste tutorial, vamos adicionar uma regra de negócio que proíba o campo `UnitPrice` de ser definido como um novo valor que seja mais do que o dobro do valor de `UnitPrice` original. Para fazer isso, ajuste a sobrecarga de `UpdateProduct` para que ele execute essa verificação e gere uma `ApplicationException` se a regra for violada. O método atualizado segue:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/samples/sample6.vb)]

Com essa alteração, qualquer atualização de preço mais do que o dobro do preço existente fará com que um `ApplicationException` seja gerado. Assim como a exceção gerada a partir da DAL, essa `ApplicationException` com BLL pode ser detectada e manipulada no manipulador de eventos de `RowUpdated` de GridView. Na verdade, o código do manipulador de eventos `RowUpdated`, conforme escrito, detectará corretamente essa exceção e exibirá o valor da propriedade de `Message` do `ApplicationException`. A Figura 11 mostra uma captura de tela quando um usuário tenta atualizar o preço de chai para $50, que é mais do que o dobro do preço atual de $19.95.

[![as regras de negócio não permitem que o preço aumente mais do que o dobro do preço do produto](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image29.png)

**Figura 11**: as regras de negócio não permitem que o preço aumente mais do que o dobro do preço do produto ([clique para exibir a imagem em tamanho normal](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb/_static/image31.png))

> [!NOTE]
> Idealmente, nossas regras de lógica de negócios seriam refatoras das sobrecargas do método `UpdateProduct` e em um método comum. Isso é deixado como um exercício para o leitor.

## <a name="summary"></a>Resumo

Durante a inserção, atualização e exclusão de operações, o controle da Web de dados e o ObjectDataSource envolveram eventos de pré e pós-nível de incêndio que delimitadom a operação real. Como vimos neste tutorial e o anterior, ao trabalhar com um GridView editável, o evento `RowUpdating` do GridView é acionado, seguido pelo evento de `Updating` do ObjectDataSource, no ponto em que o comando Update é feito no objeto subjacente do ObjectDataSource. Após a conclusão da operação, o evento de `Updated` do ObjectDataSource é acionado, seguido pelo evento de `RowUpdated` do GridView.

Podemos criar manipuladores de eventos para os eventos de nível anterior a fim de personalizar os parâmetros de entrada ou os eventos de nível posterior para inspecionar e responder aos resultados da operação. Manipuladores de eventos de nível posterior são usados com mais frequência para detectar se uma exceção ocorreu durante a operação. Diante de uma exceção, esses manipuladores de eventos de nível posterior podem, opcionalmente, lidar com a exceção por conta própria. Neste tutorial, vimos como lidar com essa exceção exibindo uma mensagem de erro amigável.

No próximo tutorial, veremos como diminuir a probabilidade de exceções decorrentes de problemas de formatação de dados (como inserir um `UnitPrice`negativo). Especificamente, veremos como adicionar controles de validação para as interfaces de edição e inserção.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Liz Shulok. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)
> [Próximo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
