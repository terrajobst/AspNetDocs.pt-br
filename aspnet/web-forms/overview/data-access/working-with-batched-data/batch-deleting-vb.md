---
uid: web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
title: Exclusão em lote (VB) | Microsoft Docs
author: rick-anderson
description: Saiba como excluir vários registros de banco de dados em uma única operação. Na camada de interface do usuário, criamos em um GridView aprimorado criado em um Tut anterior...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: 4fb72f75-32ab-4bf7-a764-be20367be726
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: 0974a16764eee2ef03cf36b4b15f9ef41f99982b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78589351"
---
# <a name="batch-deleting-vb"></a>Exclusão em lote (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_65_VB.zip) ou [baixar PDF](batch-deleting-vb/_static/datatutorial65vb1.pdf)

> Saiba como excluir vários registros de banco de dados em uma única operação. Na camada de interface do usuário, criamos um GridView aprimorado criado em um tutorial anterior. Na camada de acesso a dados, Encapsulamos as várias operações de exclusão em uma transação para garantir que todas as exclusões tenham sucesso ou todas as exclusões sejam revertidas.

## <a name="introduction"></a>Introdução

O [tutorial anterior](batch-updating-vb.md) explorou como criar uma interface de edição em lote usando um GridView totalmente editável. Em situações em que os usuários costumam editar muitos registros de uma só vez, uma interface de edição em lotes exigirá muito menos postbacks e opções de contexto de teclado para mouse, melhorando assim a eficiência do usuário final. Essa técnica é útil de forma semelhante para páginas em que é comum que os usuários excluam vários registros em um só lugar.

Qualquer pessoa que tenha usado um cliente de email online já está familiarizada com uma das interfaces de exclusão de lote mais comuns: uma caixa de seleção em cada linha em uma grade com um botão excluir todos os itens marcados correspondentes (consulte a Figura 1). Este tutorial é bem curto porque já fizemos todo o trabalho pesado nos tutoriais anteriores, criando a interface baseada na Web e um método para excluir uma série de registros como uma única operação atômica. No tutorial [adicionando uma coluna GridView de caixas de seleção](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) , criamos um GridView com uma coluna de caixas de seleção e nas [modificações de banco de dados de disposição em um](wrapping-database-modifications-within-a-transaction-vb.md) tutorial de transação, criamos um método na BLL que usaria uma transação para excluir um `List<T>` de valores de `ProductID`. Neste tutorial, vamos criar e mesclar nossas experiências anteriores para criar um exemplo de exclusão em lote de trabalho.

[![cada linha inclui uma caixa de seleção](batch-deleting-vb/_static/image1.gif)](batch-deleting-vb/_static/image1.png)

**Figura 1**: cada linha inclui uma caixa de seleção ([clique para exibir a imagem em tamanho normal](batch-deleting-vb/_static/image2.png))

## <a name="step-1-creating-the-batch-deleting-interface"></a>Etapa 1: criando a interface de exclusão de lote

Como já criamos a interface de exclusão de lote no tutorial [adicionando uma coluna GridView de caixas de seleção](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) , podemos simplesmente copiá-la para `BatchDelete.aspx` em vez de criá-la do zero. Comece abrindo a página de `BatchDelete.aspx` na pasta `BatchData` e a página `CheckBoxField.aspx` na pasta `EnhancedGridView`. Na página `CheckBoxField.aspx`, vá para o modo de exibição de origem e copie a marcação entre as marcas de `<asp:Content>`, conforme mostrado na Figura 2.

[![copiar a marcação declarativa de CheckBoxField. aspx para a área de transferência](batch-deleting-vb/_static/image2.gif)](batch-deleting-vb/_static/image3.png)

**Figura 2**: copiar a marcação declarativa de `CheckBoxField.aspx` para a área de transferência ([clique para exibir a imagem em tamanho normal](batch-deleting-vb/_static/image4.png))

Em seguida, vá para o modo de exibição de origem em `BatchDelete.aspx` e cole o conteúdo da área de transferência dentro das marcas de `<asp:Content>`. Além disso, copie e cole o código de dentro da classe code-behind no `CheckBoxField.aspx.vb` para dentro da classe code-behind no `BatchDelete.aspx.vb` (o botão de `DeleteSelectedProducts` s `Click` manipulador de eventos, o método `ToggleCheckState` e os manipuladores de eventos `Click` para os botões `CheckAll` e `UncheckAll`). Depois de copiar esse conteúdo, a classe code-behind da página `BatchDelete.aspx` deve conter o seguinte código:

[!code-vb[Main](batch-deleting-vb/samples/sample1.vb)]

Depois de copiar sobre a marcação declarativa e o código-fonte, Reserve um tempo para testar `BatchDelete.aspx` exibindo-o por meio de um navegador. Você deve ver um GridView listando os dez primeiros produtos em um GridView com cada linha listando o nome, a categoria e o preço do produto, juntamente com uma caixa de seleção. Deve haver três botões: marcar tudo, desmarcar todos e excluir produtos selecionados. Clicar no botão marcar tudo seleciona todas as caixas de seleção, ao desmarcar todas as caixas de seleção desmarcar todas. Clicar em excluir produtos selecionados exibe uma mensagem que lista os valores de `ProductID` dos produtos selecionados, mas, na verdade, não exclui os produtos.

[![a interface de CheckBoxField. aspx foi movida para BatchDeleting. aspx](batch-deleting-vb/_static/image3.gif)](batch-deleting-vb/_static/image5.png)

**Figura 3**: a Interface de `CheckBoxField.aspx` foi movida para `BatchDeleting.aspx` ([clique para exibir a imagem em tamanho normal](batch-deleting-vb/_static/image6.png))

## <a name="step-2-deleting-the-checked-products-using-transactions"></a>Etapa 2: excluindo os produtos marcados usando transações

Com a interface de exclusão de lote copiada com êxito para `BatchDeleting.aspx`, tudo o que resta é atualizar o código para que o botão excluir produtos selecionados exclua os produtos marcados usando o método `DeleteProductsWithTransaction` na classe `ProductsBLL`. Esse método, adicionado nas [modificações de quebra de banco de dados em um](wrapping-database-modifications-within-a-transaction-vb.md) tutorial de transação, aceita como entrada um `List(Of T)` de valores de `ProductID` e exclui cada `ProductID` correspondente no escopo de uma transação.

O botão de `DeleteSelectedProducts` s `Click` manipulador de eventos usa atualmente o seguinte loop de `For Each` para iterar em cada linha GridView:

[!code-vb[Main](batch-deleting-vb/samples/sample2.vb)]

Para cada linha, o controle da Web de caixa de seleção `ProductSelector` é referenciado por meio de programação. Se estiver marcada, a linha s `ProductID` será recuperada da coleção de `DataKeys` e a propriedade `DeleteResults` `Text` do rótulo será atualizada para incluir uma mensagem indicando que a linha foi selecionada para exclusão.

O código acima não exclui nenhum registro, pois a chamada para a classe `ProductsBLL` `Delete` método é comentada. Essa lógica de exclusão foi aplicada, o código excluiria os produtos, mas não dentro de uma operação atômica. Ou seja, se as primeiras exclusões na sequência tiverem êxito, mas uma falha posterior (talvez devido a uma violação de restrição de chave estrangeira), uma exceção será lançada, mas esses produtos já excluídos permanecerão excluídos.

Para garantir a atomicidade, precisamos usar o método de `DeleteProductsWithTransaction` `ProductsBLL` classe s. Como esse método aceita uma lista de valores de `ProductID`, precisamos primeiro compilar essa lista da grade e, em seguida, passá-la como um parâmetro. Primeiro, criamos uma instância de um `List(Of T)` do tipo `Integer`. Dentro do loop de `For Each` precisamos adicionar os produtos selecionados `ProductID` valores a essa `List(Of T)`. Após o loop, esse `List(Of T)` deve ser passado para o método de `DeleteProductsWithTransaction` da classe `ProductsBLL`. Atualize o botão de `DeleteSelectedProducts` s `Click` manipulador de eventos com o seguinte código:

[!code-vb[Main](batch-deleting-vb/samples/sample3.vb)]

O código atualizado cria uma `List(Of T)` do tipo `Integer` (`productIDsToDelete`) e a preenche com os valores `ProductID` a serem excluídos. Após o loop de `For Each`, se houver pelo menos um produto selecionado, o método de `DeleteProductsWithTransaction` da classe `ProductsBLL` será chamado e passado para essa lista. O rótulo de `DeleteResults` também é exibido e os dados são revinculados ao GridView (para que os registros excluídos recentemente não apareçam como linhas na grade).

A Figura 4 mostra o GridView após a seleção de um número de linhas para exclusão. A Figura 5 mostra a tela imediatamente depois que o botão excluir produtos selecionados tiver sido clicado. Observe que, na Figura 5, os valores de `ProductID` dos registros excluídos são exibidos no rótulo abaixo do GridView e essas linhas não estão mais no GridView.

[![os produtos selecionados serão excluídos](batch-deleting-vb/_static/image4.gif)](batch-deleting-vb/_static/image7.png)

**Figura 4**: os produtos selecionados serão excluídos ([clique para exibir a imagem em tamanho normal](batch-deleting-vb/_static/image8.png))

[![os valores de ProductID dos produtos excluídos são listados abaixo do GridView](batch-deleting-vb/_static/image5.gif)](batch-deleting-vb/_static/image9.png)

**Figura 5**: os produtos excluídos `ProductID` valores são listados abaixo de GridView ([clique para exibir a imagem em tamanho normal](batch-deleting-vb/_static/image10.png))

> [!NOTE]
> Para testar a atomicidade do método de `DeleteProductsWithTransaction`, adicione manualmente uma entrada para um produto na tabela `Order Details` e, em seguida, tente excluir esse produto (junto com outros). Você receberá uma violação de restrição de chave estrangeira ao tentar excluir o produto com uma ordem associada, mas observe como as exclusões de outros produtos selecionados são revertidas.

## <a name="summary"></a>Resumo

A criação de uma interface de exclusão em lote envolve a adição de um GridView com uma coluna de caixas de seleção e um controle de botão da Web que, quando clicado, excluirá todas as linhas selecionadas como uma única operação atômica. Neste tutorial, criamos uma interface desse tipo compondo em conjunto com o trabalho feito em dois tutoriais anteriores, [adicionando uma coluna GridView das caixas de seleção](../enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-vb.md) e [quebrando as modificações do banco de dados em uma transação](wrapping-database-modifications-within-a-transaction-vb.md). No primeiro tutorial, criamos um GridView com uma coluna de caixas de seleção e, no último, implementamos um método na BLL que, quando passava uma `List(Of T)` de valores de `ProductID`, excluí todas elas dentro do escopo de uma transação.

No próximo tutorial, criaremos uma interface para executar inserções em lote.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Hilton Giesenow e Teresa Murphy. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](batch-updating-vb.md)
> [Próximo](batch-inserting-vb.md)
