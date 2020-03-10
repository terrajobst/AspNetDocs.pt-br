---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
title: Adicionando uma coluna GridView das caixas de seleçãoC#() | Microsoft Docs
author: rick-anderson
description: Este tutorial analisa como adicionar uma coluna de caixas de seleção a um controle GridView para fornecer ao usuário uma maneira intuitiva de selecionar várias linhas da G...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: f63a9443-2db0-4f80-8246-840d3e86c2a3
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: b9d1ed50e8d88202c3286b4cd0e9ebf111dfbe21
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78591465"
---
# <a name="adding-a-gridview-column-of-checkboxes-c"></a>Adicionar uma coluna de GridView de caixas de seleção (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_CS.exe) ou [baixar PDF](adding-a-gridview-column-of-checkboxes-cs/_static/datatutorial52cs1.pdf)

> Este tutorial analisa como adicionar uma coluna de caixas de seleção a um controle GridView para fornecer ao usuário uma maneira intuitiva de selecionar várias linhas do GridView.

## <a name="introduction"></a>Introdução

No tutorial anterior, examinamos como adicionar uma coluna de botões de opção ao GridView para a finalidade de selecionar um registro específico. Uma coluna de botões de opção é uma interface de usuário adequada quando o usuário está limitado a escolher no máximo um item da grade. Às vezes, no entanto, poderemos permitir que o usuário escolha um número arbitrário de itens da grade. Os clientes de email baseados na Web, por exemplo, normalmente exibem a lista de mensagens com uma coluna de caixas de seleção. O usuário pode selecionar um número arbitrário de mensagens e, em seguida, executar alguma ação, como mover os emails para outra pasta ou excluí-los.

Neste tutorial, veremos como adicionar uma coluna de caixas de seleção e como determinar quais caixas de seleção foram verificadas no postback. Em particular, criaremos um exemplo que imita de forma bastante a interface do usuário do cliente de email baseado na Web. Nosso exemplo incluirá um GridView paginado listando os produtos na tabela `Products` banco de dados com uma caixa de seleção em cada linha (consulte a Figura 1). Um botão excluir produtos selecionados, quando clicado, excluirá esses produtos selecionados.

[![cada linha de produto inclui uma caixa de seleção](adding-a-gridview-column-of-checkboxes-cs/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image1.png)

**Figura 1**: cada linha de produto inclui uma caixa de seleção ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image2.png))

## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a>Etapa 1: adicionando um GridView paginado que lista informações do produto

Antes de nos preocuparmos em adicionar uma coluna de caixas de seleção, vamos primeiro nos concentrar na listagem dos produtos em um GridView que dá suporte à paginação. Comece abrindo a página de `CheckBoxField.aspx` na pasta `EnhancedGridView` e arraste um GridView da caixa de ferramentas para o designer, definindo seu `ID` como `Products`. Em seguida, escolha associar o GridView a um novo ObjectDataSource chamado `ProductsDataSource`. Configure o ObjectDataSource para usar a classe `ProductsBLL`, chamando o método `GetProducts()` para retornar os dados. Como esse GridView será somente leitura, defina as listas suspensas nas guias atualizar, inserir e excluir como (nenhum).

[![criar um novo ObjectDataSource chamado ProductsDataSource](adding-a-gridview-column-of-checkboxes-cs/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image3.png)

**Figura 2**: criar um novo ObjectDataSource chamado `ProductsDataSource` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image4.png))

[![configurar o ObjectDataSource para recuperar dados usando o método GetProducts ()](adding-a-gridview-column-of-checkboxes-cs/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image5.png)

**Figura 3**: configurar o ObjectDataSource para recuperar dados usando o método `GetProducts()` ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image6.png))

[![definir as listas suspensas nas guias atualizar, inserir e excluir para (nenhum)](adding-a-gridview-column-of-checkboxes-cs/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image7.png)

**Figura 4**: definir as listas suspensas nas guias atualizar, inserir e excluir para (nenhum) ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image8.png))

Depois de concluir o assistente para configurar fonte de dados, o Visual Studio criará automaticamente as Colunaacopladas e uma CheckBoxColumn para os campos de dados relacionados ao produto. Como fizemos no tutorial anterior, remova todos os `ProductName`, `CategoryName`e `UnitPrice` BoundFields e altere as propriedades `HeaderText` para produto, categoria e preço. Configure o `UnitPrice` BoundField para que seu valor seja formatado como uma moeda. Também configure o GridView para dar suporte à paginação marcando a caixa de seleção habilitar paginação na marca inteligente.

Deixe que o s também adicione a interface do usuário para excluir os produtos selecionados. Adicione um controle Web de botão abaixo do GridView, definindo sua `ID` como `DeleteSelectedProducts` e sua propriedade `Text` para excluir os produtos selecionados. Em vez de realmente excluir produtos do banco de dados, neste exemplo, vamos apenas exibir uma mensagem informando os produtos que teriam sido excluídos. Para acomodar isso, adicione um controle de rótulo da Web abaixo do botão. Defina sua ID como `DeleteResults`, desmarque sua propriedade `Text` e defina suas propriedades `Visible` e `EnableViewState` como `false`.

Depois de fazer essas alterações, as marcações declarativas GridView, ObjectDataSource, Button e Label devem ser semelhantes às seguintes:

[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample1.aspx)]

Reserve um tempo para exibir a página em um navegador (consulte a Figura 5). Neste ponto, você deve ver o nome, a categoria e o preço dos dez primeiros produtos.

[![o nome, a categoria e o preço dos primeiros dez produtos estão listados](adding-a-gridview-column-of-checkboxes-cs/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image9.png)

**Figura 5**: o nome, a categoria e o preço dos primeiros dez produtos são listados ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image10.png))

## <a name="step-2-adding-a-column-of-checkboxes"></a>Etapa 2: adicionando uma coluna de caixas de seleção

Como ASP.NET 2,0 inclui um CheckBoxField, pode-se imaginar que ele pode ser usado para adicionar uma coluna de caixas de seleção a um GridView. Infelizmente, esse não é o caso, pois o CheckBoxField foi projetado para trabalhar com um campo de dados booliano. Ou seja, para usar o CheckBoxField, devemos especificar o campo de dados subjacente cujo valor é consultado para determinar se a caixa de seleção renderizada está marcada. Não podemos usar o CheckBoxField para apenas incluir uma coluna de caixas de seleção desmarcadas.

Em vez disso, devemos adicionar um TemplateField e adicionar um controle da Web CheckBox à sua `ItemTemplate`. Vá em frente e adicione um TemplateField ao `Products` GridView e torne-o o primeiro campo (extrema esquerda). Na marca inteligente de GridView, clique no link editar modelos e arraste um controle da Web de caixa de seleção da caixa de ferramentas para a `ItemTemplate`. Defina essa caixa de seleção `ID` Propriedade como `ProductSelector`.

[![adicionar um controle da Web de caixa de seleção denominado ProductSelector para o nome do modelo s ItemTemplate](adding-a-gridview-column-of-checkboxes-cs/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image11.png)

**Figura 6**: adicionar um controle da Web de caixa de seleção chamado `ProductSelector` à `ItemTemplate` TemplateField ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image12.png))

Com o controle da Web TemplateField e CheckBox adicionado, cada linha agora inclui uma caixa de seleção. A Figura 7 mostra essa página, quando exibida por meio de um navegador, após a adição de TemplateField e CheckBox.

[![cada linha de produto agora inclui uma caixa de seleção](adding-a-gridview-column-of-checkboxes-cs/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image13.png)

**Figura 7**: cada linha de produto agora inclui uma caixa de seleção ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image14.png))

## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a>Etapa 3: Determinando quais caixas de seleção foram verificadas no postback

Neste ponto, temos uma coluna de caixas de seleção, mas não há como determinar quais caixas de seleção foram verificadas no postback. No entanto, quando o botão excluir produtos selecionados é clicado, precisamos saber quais caixas de seleção foram verificadas para excluir esses produtos.

A Propriedade GridView s [`Rows`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows.aspx) fornece acesso às linhas de dados em GridView. Podemos iterar por meio dessas linhas, acessar programaticamente o controle CheckBox e, em seguida, consultar sua propriedade `Checked` para determinar se a caixa de seleção foi selecionada.

Crie um manipulador de eventos para o evento de `DeleteSelectedProducts` botão de `Click` do controle da Web e adicione o seguinte código:

[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample2.cs)]

A propriedade `Rows` retorna uma coleção de instâncias de `GridViewRow` que se subcomposiçãom nas linhas de dados GridView. O loop de `foreach` aqui enumera essa coleção. Para cada objeto de `GridViewRow`, a caixa de seleção s de linha é acessada por meio de programação usando `row.FindControl("controlID")`. Se a caixa de seleção estiver marcada, a linha s correspondente `ProductID` valor será recuperada da coleção de `DataKeys`. Neste exercício, simplesmente exibimos uma mensagem informativa no rótulo `DeleteResults`, embora em um aplicativo em funcionamento, em vez disso, fazemos uma chamada para o método `ProductsBLL` classe s `DeleteProduct(productID)`.

Com a adição desse manipulador de eventos, clicar no botão excluir produtos selecionados agora exibe as `ProductID` s dos produtos selecionados.

[![quando o botão excluir produtos selecionados é clicado nos produtos selecionados ProductIDs são listados](adding-a-gridview-column-of-checkboxes-cs/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image15.png)

**Figura 8**: quando o botão excluir produtos selecionados é clicado, os produtos selecionados `ProductID` s são listados ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image16.png))

## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a>Etapa 4: Adicionar marcar tudo e desmarcar todos os botões

Se um usuário quiser excluir todos os produtos na página atual, eles deverão marcar cada uma das dez caixas de seleção. Podemos ajudar a agilizar esse processo adicionando um botão Check All que, quando clicado, seleciona todas as caixas de seleção na grade. Um botão desmarcar tudo seria igualmente útil.

Adicione dois controles de botão da Web à página, colocando-os acima de GridView. Defina a primeira `ID` s como `CheckAll` e sua propriedade `Text` para verificar tudo; Defina a segunda `ID` s como `UncheckAll` e sua propriedade `Text` para desmarcar tudo.

[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample3.aspx)]

Em seguida, crie um método na classe code-behind chamado `ToggleCheckState(checkState)` que, quando invocada, enumera a coleção `Products` GridView `Rows` e define cada caixa de seleção s `Checked` como o valor do parâmetro *checkState* passado.

[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample4.cs)]

Em seguida, crie `Click` manipuladores de eventos para os botões `CheckAll` e `UncheckAll`. No manipulador de eventos `CheckAll` s, basta chamar `ToggleCheckState(true)`; em `UncheckAll`, chame `ToggleCheckState(false)`.

[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample5.cs)]

Com esse código, clicar no botão verificar tudo causa um postback e verifica todas as caixas de seleção no GridView. Da mesma forma, clicar em desmarcar todas as caixas de seleção cancelar marca. A Figura 9 mostra a tela após o botão marcar tudo ter sido verificado.

[![clicar no botão marcar tudo seleciona todas as caixas de seleção](adding-a-gridview-column-of-checkboxes-cs/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image17.png)

**Figura 9**: clicar no botão marcar tudo seleciona todas as caixas de seleção ([clique para exibir a imagem em tamanho normal](adding-a-gridview-column-of-checkboxes-cs/_static/image18.png))

> [!NOTE]
> Ao exibir uma coluna de caixas de seleção, uma abordagem para marcar ou desmarcar todas as caixas de seleção é por meio de uma caixa de seleção na linha de cabeçalho. Além disso, a nova verificação atual/desmarcar toda a implementação requer um postback. As caixas de seleção podem ser marcadas ou desmarcadas, no entanto, inteiramente por meio do script do lado do cliente, fornecendo assim uma experiência do usuário snappier. Para explorar usando uma caixa de seleção de cabeçalho para marcar tudo e desmarcar tudo em detalhes, juntamente com uma discussão sobre como usar técnicas do lado do cliente, confira [marcar todas as caixas de seleção em um GridView usando o script do lado do cliente e uma caixa de seleção marcar tudo](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).

## <a name="summary"></a>Resumo

Nos casos em que você precisa permitir que os usuários escolham um número arbitrário de linhas de um GridView antes de continuar, adicionar uma coluna de caixas de seleção é uma opção. Como vimos neste tutorial, incluir uma coluna de caixas de seleção no GridView envolve a adição de um TemplateField com um controle da Web de caixa de seleção. Ao usar um controle da Web (versus injetar marcação diretamente no modelo, como fizemos no tutorial anterior), o ASP.NET automaticamente lembra quais caixas de seleção foram e não foram verificadas no postback. Também podemos acessar programaticamente as caixas de seleção no código para determinar se uma determinada caixa de seleção está marcada ou para alterar o estado selecionado.

Este tutorial e o último vimos a adição de uma coluna de seletor de linha ao GridView. Em nosso próximo tutorial, examinaremos como, com um pouco de trabalho, podemos adicionar recursos de inserção ao GridView.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-a-gridview-column-of-radio-buttons-cs.md)
> [Próximo](inserting-a-new-record-from-the-gridview-s-footer-cs.md)
