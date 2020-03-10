---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
title: Adicionando confirmação do lado do cliente ao excluir (VB) | Microsoft Docs
author: rick-anderson
description: Nas interfaces que criamos até agora, um usuário pode acidentalmente excluir dados clicando no botão excluir quando pretendia clicar no botão Editar. Neste t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 6331e02e-c465-4cdf-bd3f-f07680c289d6
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-vb
msc.type: authoredcontent
ms.openlocfilehash: addb5a1fdc5793309388c5f06b44fb3b145bc102
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78610512"
---
# <a name="adding-client-side-confirmation-when-deleting-vb"></a>Adicionar confirmação do lado do cliente ao excluir (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_VB.exe) ou [baixar PDF](adding-client-side-confirmation-when-deleting-vb/_static/datatutorial22vb1.pdf)

> Nas interfaces que criamos até agora, um usuário pode acidentalmente excluir dados clicando no botão excluir quando pretendia clicar no botão Editar. Neste tutorial, adicionaremos uma caixa de diálogo de confirmação do lado do cliente que aparece quando o botão excluir é clicado.

## <a name="introduction"></a>Introdução

Nos últimos tutoriais, vimos como usar nossa arquitetura de aplicativo, ObjectDataSource e os controles da Web de dados em conjunto para fornecer recursos de inserção, edição e exclusão. As interfaces de exclusão que examinamos até agora foram compostas por um botão de exclusão que, quando clicado, causa um postback e invoca o método ObjectDataSource s `Delete()`. O método `Delete()`, em seguida, invoca o método configurado da camada de lógica de negócios, que propaga a chamada para a camada de acesso a dados, emitindo a instrução `DELETE` real para o Database.

Embora essa interface do usuário permita que os visitantes excluam registros por meio dos controles GridView, DetailsView ou FormView, ele não tem qualquer tipo de confirmação quando o usuário clica no botão excluir. Se um usuário clicar acidentalmente no botão excluir quando pretendia clicar em Editar, o registro que pretendia atualizar será excluído em vez disso. Para ajudar a evitar isso, neste tutorial, adicionaremos uma caixa de diálogo de confirmação do lado do cliente que aparece quando o botão excluir é clicado.

A função JavaScript `confirm(string)` exibe seu parâmetro de entrada de cadeia de caracteres como o texto dentro de uma caixa de diálogo modal que vem equipado com dois botões-OK e Cancel (consulte a Figura 1). A função `confirm(string)` retorna um valor booliano dependendo de qual botão é clicado (`true`, se o usuário clicar em OK e `false` se clicar em Cancelar).

![O método Confirm (String) do JavaScript exibe uma MessageBox modal, do lado do cliente](adding-client-side-confirmation-when-deleting-vb/_static/image1.png)

**Figura 1**: o método JavaScript `confirm(string)` exibe uma MessageBox modal, do lado do cliente

Durante um envio de formulário, se um valor de `false` for retornado de um manipulador de eventos do lado do cliente, o envio do formulário será cancelado. Usando esse recurso, podemos fazer com que o manipulador de eventos do lado do cliente do botão excluir `onclick` retorne o valor de uma chamada para `confirm("Are you sure you want to delete this product?")`. Se o usuário clicar em cancelar, `confirm(string)` retornará false, fazendo com que o envio do formulário seja cancelado. Sem postback, o produto cujo botão de exclusão foi clicado não será excluído. Se, no entanto, o usuário clicar em OK na caixa de diálogo de confirmação, o postback continuará unabated e o produto será excluído. Consulte [usando JavaScript s `confirm()` método para controlar o envio de formulário](http://www.webreference.com/programming/javascript/confirm/) para obter mais informações sobre essa técnica.

Adicionar o script do lado do cliente necessário difere ligeiramente se estiver usando modelos do que ao usar um comando. Portanto, neste tutorial, veremos um exemplo FormView e GridView.

> [!NOTE]
> Usar técnicas de confirmação do lado do cliente, como as discutidas neste tutorial, pressupõe que os usuários estão visitando navegadores que dão suporte a JavaScript e que têm o JavaScript habilitado. Se uma dessas suposições não for verdadeira para um usuário específico, clicar no botão excluir fará com que um postback seja imediatamente (não exibindo uma confirmação MessageBox).

## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Etapa 1: Criando um FormView que dá suporte à exclusão

Comece adicionando um FormView à página `ConfirmationOnDelete.aspx` na pasta `EditInsertDelete`, associando-o a um novo ObjectDataSource que retorna as informações do produto por meio do método `GetProducts()` classe s `ProductsBLL`. Configure também o ObjectDataSource para que a classe `ProductsBLL` `DeleteProduct(productID)` método seja mapeada para o método ObjectDataSource s `Delete()`; Verifique se as listas suspensas inserir e atualizar guias estão definidas como (nenhum). Por fim, marque a caixa de seleção habilitar paginação na marca inteligente FormView s.

Após essas etapas, a nova marcação declarativa de ObjectDataSource s será parecida com a seguinte:

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample1.aspx)]

Como nos nossos exemplos anteriores que não usavam a simultaneidade otimista, Reserve um tempo para limpar a Propriedade ObjectDataSource s `OldValuesParameterFormatString`.

Como ele foi associado a um controle ObjectDataSource que dá suporte apenas à exclusão, o s `ItemTemplate` do FormView oferece apenas o botão excluir, sem os botões novo e atualizar. No entanto, a marcação declarativa s de FormView inclui um `EditItemTemplate` supérfluo e `InsertItemTemplate`, que podem ser removidos. Reserve um tempo para personalizar o `ItemTemplate` para que o exiba apenas um subconjunto dos campos de dados do produto. Eu configurei minha para mostrar o nome do produto em um `<h3>` título acima de seus nomes de fornecedor e categoria (juntamente com o botão de exclusão).

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample2.aspx)]

Com essas alterações, temos uma página da Web totalmente funcional que permite que um usuário alterne pelos produtos um de cada vez, com a capacidade de excluir um produto simplesmente clicando no botão excluir. A Figura 2 mostra uma captura de tela de nosso progresso até o momento quando visualizado por meio de um navegador.

[![o FormView mostra informações sobre um único produto](adding-client-side-confirmation-when-deleting-vb/_static/image3.png)](adding-client-side-confirmation-when-deleting-vb/_static/image2.png)

**Figura 2**: o FormView mostra informações sobre um único produto ([clique para exibir a imagem em tamanho normal](adding-client-side-confirmation-when-deleting-vb/_static/image4.png))

## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Etapa 2: chamando a função Confirm (String) por meio do evento onclick do lado do cliente de botões de exclusão

Com o FormView criado, a etapa final é configurar o botão de exclusão de modo que, quando clicado pelo visitante, a função JavaScript `confirm(string)` seja invocada. Adicionar um script do lado do cliente a um evento de `onclick` de botão, LinkButton ou ImageButton do lado do cliente pode ser realizado por meio do uso do `OnClientClick property`, que é novo no ASP.NET 2,0. Como queremos ter o valor da função `confirm(string)` retornada, basta definir essa propriedade como: `return confirm('Are you certain that you want to delete this product?');`

Após essa alteração, a sintaxe declarativa de Delete LinkButton s deve ser semelhante a:

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample3.aspx)]

Isso é tudo! A Figura 3 mostra uma captura de tela dessa confirmação em ação. Clicar no botão excluir abre a caixa de diálogo confirmar. Se o usuário clicar em cancelar, o postback será cancelado e o produto não será excluído. No entanto, se o usuário clicar em OK, o postback continuará e o método ObjectDataSource s `Delete()` será invocado, culminando no registro do banco de dados que está sendo excluído.

> [!NOTE]
> A cadeia de caracteres passada para a `confirm(string)` função JavaScript é delimitada com apóstrofos (e não aspas). No JavaScript, as cadeias de caracteres podem ser delimitadas usando qualquer caractere. Usamos apóstrofos aqui para que os delimitadores da cadeia de caracteres transmitidos para `confirm(string)` não introduzam uma ambiguidade com os delimitadores usados para o valor da propriedade `OnClientClick`.

[![uma confirmação agora é exibida ao clicar no botão excluir](adding-client-side-confirmation-when-deleting-vb/_static/image6.png)](adding-client-side-confirmation-when-deleting-vb/_static/image5.png)

**Figura 3**: uma confirmação agora é exibida ao clicar no botão excluir ([clique para exibir a imagem em tamanho normal](adding-client-side-confirmation-when-deleting-vb/_static/image7.png))

## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Etapa 3: Configurando a propriedade OnClientClick para o botão excluir em um CommandField

Ao trabalhar com um botão, LinkButton ou ImageButton diretamente em um modelo, uma caixa de diálogo de confirmação pode ser associada a ele simplesmente Configurando sua propriedade `OnClientClick` para retornar os resultados da função JavaScript `confirm(string)`. No entanto, o CommandField-que adiciona um campo de botões delete a um GridView ou DetailsView-não tem uma propriedade `OnClientClick` que pode ser definida declarativamente. Em vez disso, devemos referenciar programaticamente o botão de exclusão em GridView ou DetailsView s apropriado `DataBound` manipulador de eventos e, em seguida, definir sua propriedade `OnClientClick`.

> [!NOTE]
> Ao definir o botão de exclusão como s `OnClientClick` Propriedade no manipulador de eventos de `DataBound` apropriado, temos acesso aos dados associados ao registro atual. Isso significa que podemos estender a mensagem de confirmação para incluir detalhes sobre o registro específico, como "tem certeza de que deseja excluir o produto Chai?" Essa personalização também é possível em modelos que usam a sintaxe DataBinding.

Para praticar a configuração da propriedade `OnClientClick` para os botões Delete em um CommandField, deixe s adicionar um GridView à página. Configure este GridView para usar o mesmo controle ObjectDataSource que o FormView usa. Além disso, limitar o GridView s BoundFields para incluir apenas o nome, a categoria e o fornecedor do produto. Por fim, marque a caixa de seleção Habilitar exclusão na marca inteligente de GridView. Isso adicionará um CommandField à coleção GridView s `Columns` com sua propriedade `ShowDeleteButton` definida como `true`.

Depois de fazer essas alterações, sua marcação declarativa s de GridView deve ser semelhante ao seguinte:

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample4.aspx)]

O CommandField contém uma única instância de Delete LinkButton que pode ser acessada programaticamente por meio do manipulador de eventos GridView s `RowDataBound`. Depois de referenciado, podemos definir sua propriedade `OnClientClick` de acordo. Crie um manipulador de eventos para o evento `RowDataBound` usando o seguinte código:

[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample5.vb)]

Esse manipulador de eventos funciona com linhas de dados (aquelas que terão o botão excluir) e começa pela referência programática ao botão excluir. Em geral, use o seguinte padrão:

[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample6.vb)]

*ButtonType* é o tipo de botão que está sendo usado pelo comando-Button, LinkButton ou ImageButton. Por padrão, o CommandField usa LinkButtons, mas isso pode ser personalizado por meio do comando s `ButtonType property`. O *commandFieldIndex* é o índice ordinal do CommandField na coleção GridView s `Columns`, enquanto o *controlIndex* é o índice do botão de exclusão dentro da coleção CommandField s `Controls`. O valor de *controlIndex* depende da posição do botão em relação a outros botões no comando. Por exemplo, se o único botão exibido no CommandField for o botão excluir, use um índice de 0. No entanto, se houver um botão de edição que precede o botão excluir, use um índice de 2. O motivo pelo qual um índice de 2 é usado é que dois controles são adicionados pelo CommandField antes do botão de exclusão: o botão de edição e um LiteralControl que s usaram para adicionar algum espaço entre os botões editar e excluir.

Para nosso exemplo específico, o CommandField usa LinkButtons e, sendo o campo mais à esquerda, tem um *commandFieldIndex* de 0. Como não há outros botões, mas o botão excluir no CommandField, usamos um *controlIndex* de 0.

Depois de referenciar o botão Delete no CommandField, vamos capturar informações sobre o produto associado à linha GridView atual. Por fim, definimos o botão Delete como a propriedade s `OnClientClick` como o JavaScript apropriado, que inclui o nome do produto. Como a cadeia de caracteres JavaScript passada para a função `confirm(string)` é delimitada usando apóstrofos, devemos escapar de qualquer apóstrofo que apareça dentro do nome do produto. Em particular, todos os apóstrofos no nome do produto são ignorados com "`\'`".

Com essas alterações concluídas, clicar em um botão excluir no GridView exibe uma caixa de diálogo de confirmação personalizada (consulte a Figura 4). Assim como com a confirmação MessageBox do FormView, se o usuário clicar em cancelar, o postback será cancelado, impedindo que a exclusão ocorra.

> [!NOTE]
> Essa técnica também pode ser usada para acessar programaticamente o botão excluir no CommandField em um DetailsView. No entanto, para o DetailsView, você d cria um manipulador de eventos para o evento `DataBound`, pois DetailsView não tem um evento `RowDataBound`.

[![clicar no botão de exclusão de GridView exibe uma caixa de diálogo de confirmação personalizada](adding-client-side-confirmation-when-deleting-vb/_static/image9.png)](adding-client-side-confirmation-when-deleting-vb/_static/image8.png)

**Figura 4**: clicar no botão de exclusão de GridView exibe uma caixa de diálogo de confirmação personalizada ([clique para exibir a imagem em tamanho normal](adding-client-side-confirmation-when-deleting-vb/_static/image10.png))

## <a name="using-templatefields"></a>Usando TemplateFields

Uma das desvantagens do CommandField é que seus botões devem ser acessados por meio da indexação e que o objeto resultante deve ser convertido para o tipo de botão apropriado (Button, LinkButton ou ImageButton). Usar "números mágicos" e tipos embutidos em código convida problemas que não podem ser descobertos até o tempo de execução. Por exemplo, se você, ou outro desenvolvedor, adicionar novos botões ao CommandField em algum momento no futuro (como um botão de edição) ou alterar a propriedade `ButtonType`, o código existente ainda será compilado sem erros, mas visitar a página poderá causar uma exceção ou um comportamento inesperado, dependendo de como o código foi escrito e quais alterações foram feitas.

Uma abordagem alternativa é converter o GridView e DetailsView CommandFields em TemplateFields. Isso irá gerar um TemplateField com um `ItemTemplate` que tenha um LinkButton (ou um botão ou ImageButton) para cada botão no CommandField. Esses botões `OnClientClick` propriedades podem ser atribuídos declarativamente, como vimos com o FormView, ou podem ser acessados programaticamente no manipulador de eventos de `DataBound` apropriado usando o seguinte padrão:

[!code-vb[Main](adding-client-side-confirmation-when-deleting-vb/samples/sample7.vb)]

Em que *ControlID* é o valor da propriedade button s `ID`. Embora esse padrão ainda exija um tipo embutido em código para a conversão, ele elimina a necessidade de indexação, permitindo que o layout seja alterado sem resultar em um erro de tempo de execução.

## <a name="summary"></a>Resumo

A função JavaScript `confirm(string)` é uma técnica comumente usada para controlar o fluxo de trabalho de envio de formulário. Quando executada, a função exibe uma caixa de diálogo modal, do lado do cliente que inclui dois botões, OK e cancelar. Se o usuário clicar em OK, a função `confirm(string)` retornará `true`; clicar em cancelar retorna `false`. Essa funcionalidade, acoplada com um comportamento de navegador s para cancelar um envio de formulário, se um manipulador de eventos durante o processo de envio retornar `false`, poderá ser usado para exibir uma MessageBox de confirmação ao excluir um registro.

A função `confirm(string)` pode ser associada a um manipulador de eventos de `onclick` de controle da Web de botão no lado do cliente por meio da propriedade Control s `OnClientClick`. Ao trabalhar com um botão de exclusão em um modelo – em um dos modelos FormView s ou em um TemplateField em DetailsView ou GridView-essa propriedade pode ser definida de forma declarativa ou programaticamente, como vimos neste tutorial.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](implementing-optimistic-concurrency-vb.md)
> [Próximo](limiting-data-modification-functionality-based-on-the-user-vb.md)
