---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
title: Adicionando controles de validação à interface de edição do DataList (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como é fácil adicionar controles de validação ao EditItemTemplate do DataList para fornecer um usuário de edição mais infalível int...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 6b073fc6-524d-453d-be7c-0c30986de391
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: f952a7bb95e956a2ad935f8bdef5c3efa7437ecb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621891"
---
# <a name="adding-validation-controls-to-the-datalists-editing-interface-vb"></a>Adicionar controles de validação à interface de edição do DataList (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_VB.exe) ou [baixar PDF](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/datatutorial39vb1.pdf)

> Neste tutorial, veremos como é fácil adicionar controles de validação ao EditItemTemplate do DataList para fornecer uma interface do usuário de edição mais infalível.

## <a name="introduction"></a>Introdução

Nos tutoriais de edição de DataList até agora, as interfaces de edição de listas de dados não incluíram nenhuma validação de entrada de usuário proativa, embora uma entrada de usuário inválida, como um nome de produto ausente ou um preço negativo, resulte em uma exceção. No [tutorial anterior](handling-bll-and-dal-level-exceptions-vb.md) , examinamos como adicionar o código de tratamento de exceção ao manipulador de eventos DataList s `UpdateCommand` para capturar e exibir informações normalmente sobre quaisquer exceções que foram geradas. O ideal é que, no entanto, a interface de edição inclua controles de validação para impedir que um usuário insira esses dados inválidos em primeiro lugar.

Neste tutorial, veremos como é fácil adicionar controles de validação ao `EditItemTemplate` s de DataList para fornecer uma interface de usuário de edição mais infalível. Especificamente, este tutorial usa o exemplo criado no tutorial anterior e aumenta a interface de edição para incluir a validação apropriada.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-vbmd"></a>Etapa 1: replicando o exemplo do[tratamento de exceções de nível de BLL e Dal](handling-bll-and-dal-level-exceptions-vb.md)

No tutorial [lidando com exceções de nível de BLL e Dal](handling-bll-and-dal-level-exceptions-vb.md) , criamos uma página que listou os nomes e os preços dos produtos em um DataList editável de duas colunas. Nosso objetivo deste tutorial é aumentar a interface de edição de s DataList para incluir controles de validação. Em particular, nossa lógica de validação irá:

- Exigir que o nome do produto seja fornecido
- Verifique se o valor inserido para o preço é um formato de moeda válido
- Verifique se o valor inserido para o preço é maior ou igual a zero, uma vez que um valor de `UnitPrice` negativo é inválido

Antes que possamos examinar o aumento do exemplo anterior para incluir a validação, primeiro precisamos replicar o exemplo da página `ErrorHandling.aspx` na pasta `EditDeleteDataList` para a página deste tutorial, `UIValidation.aspx`. Para conseguir isso, precisamos copiar a marcação declarativa de s de `ErrorHandling.aspx` página e seu código-fonte. Primeiro copie sobre a marcação declarativa executando as seguintes etapas:

1. Abrir a página `ErrorHandling.aspx` no Visual Studio
2. Ir para a marcação declarativa de s de página (clique no botão origem na parte inferior da página)
3. Copie o texto dentro das marcas `<asp:Content>` e `</asp:Content>` (linhas 3 a 32), conforme mostrado na Figura 1.

[![copiar o texto dentro do &lt;ASP: content&gt; Control](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image1.png)

**Figura 2**: copiar o texto dentro do controle de `<asp:Content>` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image3.png))

1. Abrir a página `UIValidation.aspx`
2. Ir para a marcação declarativa de página
3. Cole o texto dentro do controle de `<asp:Content>`.

Para copiar sobre o código-fonte, abra a página `ErrorHandling.aspx.vb` e copie apenas o texto *dentro* da classe `EditDeleteDataList_ErrorHandling`. Copie os três manipuladores de eventos (`Products_EditCommand`, `Products_CancelCommand`e `Products_UpdateCommand`) junto com o método `DisplayExceptionDetails`, mas **não** Copie a declaração de classe nem as instruções de `using`. Cole o texto copiado *dentro* da classe `EditDeleteDataList_UIValidation` em `UIValidation.aspx.vb`.

Depois de mover o conteúdo e o código de `ErrorHandling.aspx` para `UIValidation.aspx`, Reserve um tempo para testar as páginas em um navegador. Você deve ver a mesma saída e experimentar a mesma funcionalidade em cada uma dessas duas páginas (consulte a Figura 2).

[![a página UIValidation. aspx imita a funcionalidade em ErrorHandling. aspx](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image4.png)

**Figura 2**: a página de `UIValidation.aspx` imita a funcionalidade no `ErrorHandling.aspx` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image6.png))

## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Etapa 2: adicionando os controles de validação para o controle DataList s EditItemTemplate

Ao construir formulários de entrada de dados, é importante que os usuários insiram todos os campos obrigatórios e que todas as suas entradas fornecidas sejam valores legais e corretamente formatados. Para ajudar a garantir que as entradas de um usuário sejam válidas, o ASP.NET fornece cinco controles de validação internos projetados para validar o valor de um único controle da Web de entrada:

- O [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) garante que um valor tenha sido fornecido
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) valida um valor em relação a outro valor de controle da Web ou um valor constante, ou garante que o formato s de valor seja válido para um tipo de dados especificado
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garante que um valor esteja dentro de um intervalo de valores
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) valida um valor em relação a uma [expressão regular](http://en.wikipedia.org/wiki/Regular_expression)
- O [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) valida um valor em relação a um método personalizado definido pelo usuário

Para obter mais informações sobre esses cinco controles, consulte o tutorial [adicionando controles de validação para as interfaces de edição e inserção](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) ou confira a [seção controles de validação](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) dos tutoriais de [início rápido do ASP.net](https://quickstarts.asp.net).

Para nosso tutorial, precisaremos usar um RequiredFieldValidator para garantir que um valor para o nome do produto tenha sido fornecido e um CompareValidator para garantir que o preço inserido tenha um valor maior ou igual a 0 e seja apresentado em um formato de moeda válido.

> [!NOTE]
> Embora ASP.NET 1. x tenha esses mesmos cinco controles de validação, o ASP.NET 2,0 adicionou vários aprimoramentos, os dois principais que são o suporte de script do lado do cliente para navegadores, além do Internet Explorer e a capacidade de particionar controles de validação em uma página em grupos de validação. Para obter mais informações sobre os novos recursos de controle de validação no 2,0, consulte disparando [os controles de validação no ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).

Deixe que os s comecem adicionando os controles de validação necessários à `EditItemTemplate`de DataList s. Essa tarefa pode ser executada por meio do Designer clicando no link editar modelos da marca inteligente DataList s ou por meio da sintaxe declarativa. Vamos percorrer o processo usando a opção Editar modelos do modo de exibição de Design. Depois de optar por editar o `EditItemTemplate`s de DataList, adicione um RequiredFieldValidator arrastando-o da caixa de ferramentas para a interface de edição de modelo, colocando-o após a caixa de texto `ProductName`.

[![adicionar um RequiredFieldValidator ao EditItemTemplate após a caixa de texto NomeDoProduto](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image7.png)

**Figura 3**: adicionar um RequiredFieldValidator ao `EditItemTemplate After` caixa de texto `ProductName` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image9.png))

Todos os controles de validação funcionam validando a entrada de um único controle da Web ASP.NET. Portanto, precisamos indicar que o RequiredFieldValidator que acabamos de adicionar deve ser validado em relação à caixa de texto `ProductName`; Isso é feito definindo a [Propriedade s`ControlToValidate`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) do controle de validação como a `ID` do controle da Web apropriado (`ProductName`, nessa instância). Em seguida, defina a [propriedade`ErrorMessage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) como você deve fornecer o nome do produto e a [propriedade`Text`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) como \*. O valor da propriedade `Text`, se fornecido, é o texto que será exibido pelo controle de validação se a validação falhar. O valor da propriedade `ErrorMessage`, que é necessário, é usado pelo controle ValidationSummary; Se o valor da propriedade `Text` for omitido, o valor da propriedade `ErrorMessage` será exibido pelo controle de validação em entrada inválida.

Depois de definir essas três propriedades do RequiredFieldValidator, sua tela deve ser semelhante à figura 4.

[![definir as propriedades do RequiredFieldValidator s ControlToValidate, ErrorMessage e Text](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image10.png)

**Figura 4**: definir as propriedades RequiredFieldValidator s `ControlToValidate`, `ErrorMessage`e `Text` ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image12.png))

Com o RequiredFieldValidator adicionado ao `EditItemTemplate`, tudo o que resta é adicionar a validação necessária para a caixa de texto preço s do produto. Como o `UnitPrice` é opcional ao editar um registro, não precisamos adicionar um RequiredFieldValidator. No entanto, precisamos adicionar um CompareValidator para garantir que o `UnitPrice`, se fornecido, esteja formatado corretamente como uma moeda e seja maior ou igual a 0.

Adicione o CompareValidator à `EditItemTemplate` e defina sua propriedade `ControlToValidate` como `UnitPrice`, sua propriedade `ErrorMessage` para o preço deve ser maior ou igual a zero e não pode incluir o símbolo de moeda e sua propriedade `Text` como \*. Para indicar que o valor de `UnitPrice` deve ser maior ou igual a 0, defina a Propriedade CompareValidator s [`Operator`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) como `GreaterThanEqual`, sua [Propriedade`ValueToCompare`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) como 0 e sua [Propriedade`Type`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) como `Currency`.

Depois de adicionar esses dois controles de validação, a sintaxe declarativa s `EditItemTemplate` s deve ser semelhante ao seguinte:

[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Depois de fazer essas alterações, abra a página em um navegador. Se você tentar omitir o nome ou inserir um valor de preço inválido ao editar um produto, um asterisco será exibido ao lado da caixa de texto. Como mostra a Figura 5, um valor de preço que inclui o símbolo de moeda, como $19.95, é considerado inválido. O CompareValidator s `Currency` `Type` permite separadores de dígitos (como vírgulas ou pontos, dependendo das configurações de cultura) e um sinal de mais ou menos à esquerda, mas *não permite um* símbolo de moeda. Esse comportamento pode perplex os usuários, pois a interface de edição processa atualmente o `UnitPrice` usando o formato de moeda.

[![um asterisco aparece ao lado das caixas de Textcom entrada inválida](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image13.png)

**Figura 5**: um asterisco é exibido ao lado das caixas de Textcom entrada inválida ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image15.png))

Enquanto a validação funciona no estado em que se encontra, o usuário precisa remover manualmente o símbolo de moeda ao editar um registro, o que não é aceitável. Além disso, se houver entradas inválidas na interface de edição nem os botões Atualizar nem cancelar, quando clicado, o invocará um postback. O ideal é que o botão Cancelar retorne o DataList para seu estado de pré-edição, independentemente da validade das entradas do usuário. Além disso, precisamos garantir que os dados de s da página sejam válidos antes de atualizar as informações do produto no manipulador de eventos DataList s `UpdateCommand`, uma vez que a validação da lógica do lado do cliente pode ser ignorada pelos usuários cujos navegadores não dão suporte a JavaScript ou que têm seu suporte desabilitado.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Removendo o símbolo de moeda da caixa de texto PreçoUnitário do EditItemTemplate

Ao usar o `Currency``Type`s CompareValidator, a entrada que está sendo validada não deve incluir nenhum símbolo de moeda. A presença desses símbolos faz com que o CompareValidator marque a entrada como inválida. No entanto, nossa interface de edição atualmente inclui um símbolo de moeda na caixa de texto `UnitPrice`, o que significa que o usuário deve remover explicitamente o símbolo de moeda antes de salvar suas alterações. Para corrigir isso, temos três opções:

1. Configure o `EditItemTemplate` para que o valor da caixa de texto `UnitPrice` não seja formatado como uma moeda.
2. Permita que o usuário insira um símbolo de moeda removendo o CompareValidator e substituindo-o por um RegularExpressionValidator que verifica um valor de moeda formatado corretamente. O desafio aqui é que a expressão regular para validar um valor de moeda não é tão simples quanto a CompareValidator e exigiria escrever código se quiséssemos incorporar as configurações de cultura.
3. Remova completamente o controle de validação e conte com a lógica de validação personalizada no lado do servidor no manipulador de eventos GridView s `RowUpdating`.

Deixe que o s vá para a opção 1 para este tutorial. Atualmente, a `UnitPrice` é formatada como um valor de moeda devido à expressão DataBinding para a caixa de texto na `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Altere a instrução `Eval` para `Eval("UnitPrice", "{0:n2}")`, que formata o resultado como um número com dois dígitos de precisão. Isso pode ser feito diretamente por meio da sintaxe declarativa ou clicando no link editar DataBindings da caixa de texto `UnitPrice` no `EditItemTemplate`DataList s.

Com essa alteração, o preço formatado na interface de edição inclui vírgulas como separador de grupo e um ponto como separador decimal, mas deixa fora do símbolo de moeda.

> [!NOTE]
> Ao remover o formato de moeda da interface editável, acho útil colocar o símbolo de moeda como texto fora da caixa de texto. Isso serve como uma dica ao usuário de que eles não precisam fornecer o símbolo de moeda.

## <a name="fixing-the-cancel-button"></a>Corrigindo o botão Cancelar

Por padrão, os controles da Web de validação emitem JavaScript para executar a validação no lado do cliente. Quando um botão, LinkButton ou ImageButton é clicado, os controles de validação na página são verificados no lado do cliente antes de o postback ocorrer. Se houver dados inválidos, o postback será cancelado. Para certos botões, no entanto, a validade dos dados pode ser irrelevante; Nesse caso, ter o postback cancelado devido a dados inválidos é um incômodo.

O botão Cancelar é um exemplo desse tipo. Imagine que um usuário insira dados inválidos, como omitir o nome do produto e, em seguida, decide que ele não deseja salvar o produto depois de todos e pressionar o botão Cancelar. Atualmente, o botão Cancelar dispara os controles de validação na página, o que relata que o nome do produto está ausente e impede o postback. Nosso usuário precisa digitar algum texto na caixa de texto `ProductName` apenas para cancelar o processo de edição.

Felizmente, o Button, LinkButton e ImageButton têm uma [propriedade`CausesValidation`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) que pode indicar se clicar ou não o botão deve iniciar a lógica de validação (o padrão é `True`). Defina a propriedade s `CausesValidation` do botão Cancelar como `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Garantindo que as entradas sejam válidas no manipulador de eventos UpdateCommand

Devido ao script do lado do cliente emitido pelos controles de validação, se um usuário inserir entrada inválida, os controles de validação cancelarão os postbacks iniciados pelos controles Button, LinkButton ou ImageButton cujas propriedades `CausesValidation` são `True` (o padrão). No entanto, se um usuário estiver visitando um navegador antiquada ou um cujo suporte a JavaScript foi desabilitado, as verificações de validação no lado do cliente não serão executadas.

Todos os controles de validação ASP.NET repetim sua lógica de validação imediatamente no postback e relatam a validade geral das entradas da página s por meio da [propriedade`Page.IsValid`](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). No entanto, o fluxo de página não é interrompido ou interrompido de forma alguma com base no valor de `Page.IsValid`. Como desenvolvedores, é nossa responsabilidade garantir que a propriedade `Page.IsValid` tenha um valor de `True` antes de prosseguir com um código que assuma dados de entrada válidos.

Se um usuário tiver o JavaScript desabilitado, visitar nossa página, editar um produto, inserir um valor de preço muito caro e clicar no botão atualizar, a validação do lado do cliente será ignorada e um postback será acontecerdo. No postback, a página ASP.NET s `UpdateCommand` manipulador de eventos é executada e uma exceção é gerada ao tentar analisar muito caro para um `Decimal`. Como temos o tratamento de exceções, tal exceção será tratada normalmente, mas poderíamos impedir que os dados inválidos passem em primeiro lugar, apenas com o manipulador de eventos `UpdateCommand` se `Page.IsValid` tiver um valor de `True`.

Adicione o seguinte código ao início do manipulador de eventos `UpdateCommand`, imediatamente antes do bloco de `Try`:

[!code-vb[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample2.vb)]

Com essa adição, o produto tentará ser atualizado somente se os dados enviados forem válidos. A maioria dos usuários não poderá postar dados inválidos devido à validação de scripts do lado do cliente, mas os usuários cujos navegadores não dão suporte a JavaScript ou com suporte a JavaScript desabilitado, podem ignorar as verificações do lado do cliente e enviar dados inválidos.

> [!NOTE]
> O leitor do astuto se lembrará de que, ao atualizar dados com o GridView, não precisamos verificar explicitamente a propriedade `Page.IsValid` em nossa classe code-behind de página. Isso ocorre porque o GridView consulta a propriedade `Page.IsValid` para nós e só continua com a atualização se retornar um valor de `True`.

## <a name="step-3-summarizing-data-entry-problems"></a>Etapa 3: Resumindo problemas de entrada de dados

Além dos cinco controles de validação, ASP.NET inclui o [controle ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), que exibe os `ErrorMessage` s desses controles de validação que detectaram dados inválidos. Esses dados de resumo podem ser exibidos como texto na página da Web ou por meio de uma MessageBox restrita do lado do cliente. Vamos aprimorar este tutorial para incluir uma MessageBox do lado do cliente Resumindo quaisquer problemas de validação.

Para fazer isso, arraste um controle ValidationSummary da caixa de ferramentas para o designer. O local do controle ValidationSummary não importa muito, já que vamos configurá-lo para exibir apenas o resumo como uma MessageBox. Depois de adicionar o controle, defina sua [propriedade`ShowSummary`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) como `False` e sua [propriedade`ShowMessageBox`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) como `True`. Com essa adição, todos os erros de validação são resumidos em uma MessageBox do lado do cliente (consulte a Figura 6).

[![os erros de validação são resumidos em uma MessageBox do lado do cliente](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image16.png)

**Figura 6**: os erros de validação são resumidos em uma MessageBox do lado do cliente ([clique para exibir a imagem em tamanho normal](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image18.png))

## <a name="summary"></a>Resumo

Neste tutorial, vimos como reduzir a probabilidade de exceções usando controles de validação para garantir proativamente que nossas entradas de usuários sejam válidas antes de tentar usá-las no fluxo de trabalho de atualização. O ASP.NET fornece cinco controles da Web de validação que são projetados para inspecionar uma entrada específica de s de controle da Web e relatar a validade de s de entrada. Neste tutorial, usamos dois desses cinco controles o RequiredFieldValidator e o CompareValidator para garantir que o nome do produto foi fornecido e que o preço tinha um formato de moeda com um valor maior ou igual a zero.

Adicionar controles de validação à interface DataList s Editing é tão simples quanto arrastá-los para a `EditItemTemplate` da caixa de ferramentas e definir algumas propriedades. Por padrão, os controles de validação emitem automaticamente o script de validação do lado do cliente; Eles também fornecem validação no lado do servidor no postback, armazenando o resultado cumulativo na propriedade `Page.IsValid`. Para ignorar a validação do lado do cliente quando um botão, LinkButton ou ImageButton for clicado, defina o botão s `CausesValidation` Propriedade como `False`. Além disso, antes de executar qualquer tarefa com os dados enviados no postback, verifique se a propriedade `Page.IsValid` retorna `True`.

Todos os tutoriais de edição DataList que nós examinamos até agora têm interfaces de edição muito simples uma caixa de texto para o nome do produto e outro para o preço. A interface de edição, no entanto, pode conter uma combinação de diferentes controles da Web, como DropDownLists, calendários, botões de opção, caixas de seleção e assim por diante. Em nosso próximo tutorial, vamos examinar a criação de uma interface que usa uma variedade de controles da Web.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Dennis Patterson, Ken Pespisa e Liz Shulok. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](handling-bll-and-dal-level-exceptions-vb.md)
> [Próximo](customizing-the-datalist-s-editing-interface-vb.md)
