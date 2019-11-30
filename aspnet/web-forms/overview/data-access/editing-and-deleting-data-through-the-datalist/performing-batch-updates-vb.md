---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
title: Executando atualizações em lotes (VB) | Microsoft Docs
author: rick-anderson
description: Saiba como criar um DataList totalmente editável em que todos os seus itens estão no modo de edição e cujos valores podem ser salvos clicando em um botão ' atualizar tudo ' no...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 8dac22a7-91de-4e3b-888f-a4c438b03851
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb
msc.type: authoredcontent
ms.openlocfilehash: e54c9fc12da278492b54164cf657eea142a3dae6
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74632566"
---
# <a name="performing-batch-updates-vb"></a>Executar atualizações em lote (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_VB.exe) ou [baixar PDF](performing-batch-updates-vb/_static/datatutorial37vb1.pdf)

> Saiba como criar um DataList totalmente editável em que todos os seus itens estão no modo de edição e cujos valores podem ser salvos clicando em um botão "atualizar tudo" na página.

## <a name="introduction"></a>Introdução

No [tutorial anterior](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md) , examinamos como criar um DataList de nível de item. Como o GridView editável padrão, cada item no DataList incluía um botão de edição que, quando clicado, tornaria o item editável. Embora essa edição em nível de item funcione bem para dados que são atualizados apenas ocasionalmente, certos cenários de caso de uso exigem que o usuário edite vários registros. Se um usuário precisar editar dezenas de registros e for forçado a clicar em Editar, fazer suas alterações e clicar em atualizar para cada um deles, a quantidade de cliques poderá dificultar a produtividade. Nessas situações, uma opção melhor é fornecer um DataList totalmente editável, um em que *todos os* seus itens estejam no modo de edição e cujos valores possam ser editados clicando em um botão atualizar tudo na página (consulte a Figura 1).

[![cada item em um DataList totalmente editável pode ser modificado](performing-batch-updates-vb/_static/image2.png)](performing-batch-updates-vb/_static/image1.png)

**Figura 1**: cada item em um DataList totalmente editável pode ser modificado ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image3.png))

Neste tutorial, examinaremos como permitir que os usuários atualizem informações de endereço de fornecedores usando um DataList totalmente editável.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Etapa 1: criar a interface do usuário editável no ItemTemplate s do DataList

No tutorial anterior, em que criamos um DataList editável padrão de nível de item, usamos dois modelos:

- `ItemTemplate` continha a interface do usuário somente leitura (o rótulo controles da Web para exibir cada nome e preço do produto).
- `EditItemTemplate` continha a interface do usuário do modo de edição (os dois controles da Web TextBox).

A propriedade DataList s `EditItemIndex` determina o que `DataListItem` (se houver) é renderizado usando o `EditItemTemplate`. Em particular, o `DataListItem` cujo valor de `ItemIndex` corresponde à Propriedade DataList s `EditItemIndex` é renderizado usando o `EditItemTemplate`. Esse modelo funciona bem quando apenas um item pode ser editado de cada vez, mas está de fora ao criar um DataList totalmente editável.

Para um DataList totalmente editável, queremos que *todos* os `DataListItem` s sejam renderizados usando a interface editável. A maneira mais simples de fazer isso é definir a interface editável na `ItemTemplate`. Para modificar as informações de endereço dos fornecedores, a interface editável contém o nome do fornecedor como texto e, em seguida, Textboxes para os valores de endereço, cidade e país.

Comece abrindo a página `BatchUpdate.aspx`, adicione um controle DataList e defina sua propriedade `ID` como `Suppliers`. Na marca inteligente DataList s, opte por adicionar um novo controle ObjectDataSource chamado `SuppliersDataSource`.

[![criar um novo ObjectDataSource chamado SuppliersDataSource](performing-batch-updates-vb/_static/image5.png)](performing-batch-updates-vb/_static/image4.png)

**Figura 2**: criar um novo ObjectDataSource chamado `SuppliersDataSource` ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image6.png))

Configure o ObjectDataSource para recuperar dados usando o método de `GetSuppliers()` da classe `SuppliersBLL` (consulte a Figura 3). Assim como no tutorial anterior, em vez de atualizar as informações do fornecedor por meio de ObjectDataSource, trabalharemos diretamente com a camada de lógica de negócios. Portanto, defina a lista suspensa como (nenhum) na guia atualizar (consulte a Figura 4).

[![recuperar informações do fornecedor usando o método getsuppliers ()](performing-batch-updates-vb/_static/image8.png)](performing-batch-updates-vb/_static/image7.png)

**Figura 3**: recuperar informações do fornecedor usando o método `GetSuppliers()` ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image9.png))

[![definir a lista suspensa como (nenhum) na guia atualizar](performing-batch-updates-vb/_static/image11.png)](performing-batch-updates-vb/_static/image10.png)

**Figura 4**: defina a lista suspensa como (nenhum) na guia atualizar ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image12.png))

Depois de concluir o assistente, o Visual Studio gera automaticamente o `ItemTemplate` s de DataList para exibir cada campo de dados retornado pela fonte de dados em um controle de rótulo da Web. Precisamos modificar esse modelo para que ele forneça a interface de edição em vez disso. O `ItemTemplate` pode ser personalizado por meio do designer usando a opção Editar modelos da marca inteligente s de DataList ou diretamente por meio da sintaxe declarativa.

Reserve um tempo para criar uma interface de edição que exibe o nome do fornecedor como texto, mas inclui TextBoxes para o endereço, a cidade e os valores de país do fornecedor. Depois de fazer essas alterações, a sintaxe declarativa da página s deve ser semelhante ao seguinte:

[!code-aspx[Main](performing-batch-updates-vb/samples/sample1.aspx)]

> [!NOTE]
> Assim como no tutorial anterior, o DataList deste tutorial deve ter seu estado de exibição habilitado.

Na `ItemTemplate` I m usando duas novas classes CSS, `SupplierPropertyLabel` e `SupplierPropertyValue`, que foram adicionadas à classe `Styles.css` e configuradas para usar as mesmas configurações de estilo das classes CSS `ProductPropertyLabel` e `ProductPropertyValue`.

[!code-css[Main](performing-batch-updates-vb/samples/sample2.css)]

Depois de fazer essas alterações, visite esta página por meio de um navegador. Como mostra a Figura 5, cada item DataList exibe o nome do fornecedor como texto e usa TextBoxes para exibir o endereço, a cidade e o país.

[![cada fornecedor no DataList é editável](performing-batch-updates-vb/_static/image14.png)](performing-batch-updates-vb/_static/image13.png)

**Figura 5**: cada fornecedor no DataList é editável ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image15.png))

## <a name="step-2-adding-an-update-all-button"></a>Etapa 2: adicionando um botão atualizar tudo

Embora cada fornecedor da Figura 5 tenha seus campos address, City e Country exibidos em uma caixa de texto, no momento não há nenhum botão de atualização disponível. Em vez de ter um botão de atualização por item, com listas de itens totalmente editáveis, normalmente há um único botão atualizar tudo na página que, quando clicado, atualiza *todos* os registros no DataList. Para este tutorial, vamos adicionar dois botões atualizar todos – um na parte superior da página e um na parte inferior (embora clicar em um dos botões tenha o mesmo efeito).

Comece adicionando um controle da Web de botão acima da DataList e defina sua propriedade `ID` como `UpdateAll1`. Em seguida, adicione o segundo botão controle da Web abaixo do DataList, definindo seu `ID` como `UpdateAll2`. Defina as propriedades de `Text` para os dois botões para atualizar tudo. Por fim, crie manipuladores de eventos para os dois botões `Click` eventos. Em vez de duplicar a lógica de atualização em cada um dos manipuladores de eventos, vamos refatorar essa lógica a um terceiro método, `UpdateAllSupplierAddresses`, fazendo com que os manipuladores de eventos simplesmente invoquem esse terceiro método.

[!code-vb[Main](performing-batch-updates-vb/samples/sample3.vb)]

A Figura 6 mostra a página após a adição de todos os botões Atualizar.

[![dois botões atualizar tudo foram adicionados à página](performing-batch-updates-vb/_static/image17.png)](performing-batch-updates-vb/_static/image16.png)

**Figura 6**: dois botões atualizar todos foram adicionados à página ([clique para exibir a imagem em tamanho normal](performing-batch-updates-vb/_static/image18.png))

## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Etapa 3: atualizando todas as informações de endereço dos fornecedores

Com todos os itens de DataList s exibindo a interface de edição e com a adição dos botões atualizar tudo, tudo o que resta é gravar o código para executar a atualização do lote. Especificamente, precisamos executar um loop nos itens DataList e chamar o método `SuppliersBLL` `UpdateSupplierAddress` da classe para cada um.

A coleção de instâncias de `DataListItem` que composição o DataList pode ser acessada por meio da Propriedade DataList s [`Items`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx). Com uma referência a uma `DataListItem`, podemos obter o `SupplierID` correspondente da coleção de `DataKeys` e fazer referência programaticamente aos controles da Web TextBox dentro do `ItemTemplate`, uma vez que o código a seguir ilustra:

[!code-vb[Main](performing-batch-updates-vb/samples/sample4.vb)]

Quando o usuário clica em um dos botões atualizar tudo, o método `UpdateAllSupplierAddresses` itera em cada `DataListItem` na `Suppliers` DataList e chama o método `UpdateSupplierAddress` da classe `SuppliersBLL`, passando os valores correspondentes. Um valor não inserido para passagens de endereço, cidade ou país é um valor de `Nothing` para `UpdateSupplierAddress` (em vez de uma cadeia de caracteres em branco), o que resulta em um `NULL` de banco de dados para os campos de registro subjacentes.

> [!NOTE]
> Como um aprimoramento, talvez você queira adicionar um controle de status de rótulo da Web à página que fornece uma mensagem de confirmação após a execução da atualização do lote.

## <a name="updating-only-those-addresses-that-have-been-modified"></a>Atualizando somente os endereços que foram modificados

O algoritmo de atualização do lote usado para este tutorial chama o método `UpdateSupplierAddress` para *cada* fornecedor no DataList, independentemente de suas informações de endereço terem sido alteradas. Embora essas atualizações cegas não sejam geralmente um problema de desempenho, elas podem levar a registros supérfluos se você reauditar as alterações na tabela do banco de dados. Por exemplo, se você usar gatilhos para registrar todos os `UPDATE` s na tabela `Suppliers` para uma tabela de auditoria, sempre que um usuário clicar no botão atualizar tudo, um novo registro de auditoria será criado para cada fornecedor no sistema, independentemente de o usuário ter feito alterações.

As classes DataTable e DataAdapter do ADO.NET são projetadas para dar suporte a atualizações em lotes, onde somente os registros modificados, excluídos e novos resultam em qualquer comunicação de banco de dados. Cada linha na DataTable tem uma [propriedade`RowState`](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) que indica se a linha foi adicionada à DataTable, excluída dela, modificada ou permanece inalterada. Quando uma DataTable é inicialmente populada, todas as linhas são marcadas inalteradas. Alterar o valor de qualquer uma das colunas de s de linha marca a linha como modificada.

Na classe `SuppliersBLL`, atualizamos as informações de endereço do fornecedor especificado primeiro lendo no registro de fornecedor único em uma `SuppliersDataTable` e, em seguida, definimos os valores de coluna `Address`, `City`e `Country` usando o seguinte código:

[!code-vb[Main](performing-batch-updates-vb/samples/sample5.vb)]

Esse código naively atribui os valores de endereço, cidade e país passados ao `SuppliersRow` no `SuppliersDataTable`, independentemente de os valores terem sido alterados ou não. Essas modificações fazem com que a propriedade `SuppliersRow` s `RowState` seja marcada como modificada. Quando o método `Update` da camada de acesso a dados é chamado, ele vê que o `SupplierRow` foi modificado e, portanto, envia um comando `UPDATE` ao banco de dados.

No entanto, imagine que adicionamos o código a esse método para atribuir apenas os valores de endereço, cidade e país passados, se forem diferentes dos valores existentes de `SuppliersRow` s. No caso em que o endereço, a cidade e o país são os mesmos que os dados existentes, nenhuma alteração será feita e as `SupplierRow` s `RowState` serão marcadas como inalteradas. O resultado líquido é que quando o método s `Update` é chamado, nenhuma chamada de banco de dados será feita porque o `SuppliersRow` não foi modificado.

Para aplicar essa alteração, substitua as instruções que atribuem de forma oculta os valores de endereço, cidade e país passados com o seguinte código:

[!code-vb[Main](performing-batch-updates-vb/samples/sample6.vb)]

Com esse código adicionado, o método da DAL s `Update` envia uma instrução `UPDATE` ao banco de dados somente para os registros cujos valores relacionados ao endereço foram alterados.

Como alternativa, poderíamos controlar se há alguma diferença entre os campos de endereço passados e os dados do banco e, se não houver nenhum, basta ignorar a chamada para o método de `Update` do DAL. Essa abordagem funciona bem se você reutilizar o método de banco de dados direto, já que o método de banco de dados direto não passou uma instância de `SuppliersRow`, cuja `RowState` pode ser verificada para determinar se uma chamada de Database é realmente necessária.

> [!NOTE]
> Cada vez que o método de `UpdateSupplierAddress` é invocado, uma chamada é feita ao banco de dados para recuperar informações sobre o registro atualizado. Em seguida, se houver alguma alteração nos dados, será feita outra chamada para o banco de dado para atualizar a linha da tabela. Esse fluxo de trabalho pode ser otimizado criando uma sobrecarga de método de `UpdateSupplierAddress` que aceita uma instância de `EmployeesDataTable` que tem *todas* as alterações da página `BatchUpdate.aspx`. Em seguida, ele poderia fazer uma chamada para o banco de dados para obter todos os registros da tabela `Suppliers`. Os dois conjuntos de resultados podem ser enumerados e somente os registros onde ocorreram alterações podem ser atualizados.

## <a name="summary"></a>Resumo

Neste tutorial, vimos como criar um DataList totalmente editável, permitindo que um usuário modifique rapidamente as informações de endereço de vários fornecedores. Começamos definindo a interface de edição um controle da Web TextBox para o endereço do fornecedor, a cidade e os valores de país na `ItemTemplate`DataList s. Em seguida, adicionamos atualizar todos os botões acima e abaixo do DataList. Depois que um usuário tiver feito suas alterações e clicado em um dos botões atualizar todos, os `DataListItem` s serão enumerados e uma chamada para o método `SuppliersBLL` classe s `UpdateSupplierAddress` será feita.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Zack Jones e Ken Pespisa. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
> [Próximo](handling-bll-and-dal-level-exceptions-vb.md)
