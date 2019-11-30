---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: Botões personalizados no DataList e no Repeater (C#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, criaremos uma interface que usa um repetidor para listar as categorias no sistema, com cada categoria fornecendo um botão para mostrar seu Associ...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: e8cb1054068327c25e057b6df1cc7506feec8d37
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74601736"
---
# <a name="custom-buttons-in-the-datalist-and-repeater-c"></a>Botões personalizados no DataList e Repeater (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe) ou [baixar PDF](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> Neste tutorial, criaremos uma interface que usa um repetidor para listar as categorias no sistema, com cada categoria fornecendo um botão para mostrar seus produtos associados usando um controle BulletedList.

## <a name="introduction"></a>Introdução

Nos últimos tutoriais do dezessete DataList e do Repeater, nós criamos exemplos somente leitura e editando e excluindo exemplos. Para facilitar a edição e a exclusão de recursos dentro de um DataList, adicionamos botões à `ItemTemplate` DataList s que, quando clicado, causaram um postback e geramos um evento DataList correspondente à propriedade s `CommandName` do botão. Por exemplo, adicionar um botão à `ItemTemplate` com um valor de propriedade de `CommandName` de editar faz com que o `EditCommand` DataList seja acionado no postback; uma com a `CommandName` Delete gera o `DeleteCommand`.

Além dos botões editar e excluir, os controles DataList e Repeater também podem incluir botões, LinkButtons ou ImageButtons que, quando clicados, executam alguma lógica personalizada no lado do servidor. Neste tutorial, criaremos uma interface que usa um repetidor para listar as categorias no sistema. Para cada categoria, o repetidor incluirá um botão para mostrar os produtos associados à categoria usando um controle BulletedList (consulte a Figura 1).

[![clicar no link mostrar produtos exibe os produtos da categoria s em uma lista com marcadores](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**Figura 1**: clicar no link mostrar produtos exibe a categoria s produtos em uma lista com marcadores ([clique para exibir a imagem em tamanho normal](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))

## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>Etapa 1: adicionando as páginas da Web do tutorial do botão personalizado

Antes de examinarmos como adicionar um botão personalizado, vamos primeiro reservar um momento para criar as páginas do ASP.NET em nosso projeto de site que precisaremos para este tutorial. Comece adicionando uma nova pasta chamada `CustomButtonsDataListRepeater`. Em seguida, adicione as duas páginas ASP.NET a seguir a essa pasta, assegurando a associação de cada página com a página mestra de `Site.master`:

- `Default.aspx`
- `CustomButtons.aspx`

![Adicionar as páginas ASP.NET para os tutoriais relacionados aos botões personalizados](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**Figura 2**: adicionar as páginas ASP.net para os tutoriais relacionados aos botões personalizados

Assim como nas outras pastas, `Default.aspx` na pasta `CustomButtonsDataListRepeater` listará os tutoriais em sua seção. Lembre-se de que o controle de usuário `SectionLevelTutorialListing.ascx` fornece essa funcionalidade. Adicione esse controle de usuário a `Default.aspx` arrastando-o da Gerenciador de Soluções para a página modo de exibição de Design de s.

[![adicionar o controle de usuário SectionLevelTutorialListing. ascx a default. aspx](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**Figura 3**: Adicionar o controle de usuário `SectionLevelTutorialListing.ascx` ao `Default.aspx` ([clique para exibir a imagem em tamanho normal](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))

Por fim, adicione as páginas como entradas ao arquivo de `Web.sitemap`. Especificamente, adicione a seguinte marcação após a paginação e a classificação com o DataList e o repetidor `<siteMapNode>`:

[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

Depois de atualizar `Web.sitemap`, Reserve um momento para exibir o site de tutoriais por meio de um navegador. O menu à esquerda agora inclui itens para os tutoriais de edição, inserção e exclusão.

![O mapa do site agora inclui a entrada para o tutorial de botões personalizados](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**Figura 4**: o mapa do site agora inclui a entrada para o tutorial de botões personalizados

## <a name="step-2-adding-the-list-of-categories"></a>Etapa 2: adicionando a lista de categorias

Para este tutorial, precisamos criar um repetidor que liste todas as categorias junto com um LinkButton show Products que, quando clicado, exibe os produtos da categoria associada em uma lista com marcadores. Primeiro, vamos criar um repetidor simples que liste as categorias no sistema. Comece abrindo a página de `CustomButtons.aspx` na pasta `CustomButtonsDataListRepeater`. Arraste um Repeater da caixa de ferramentas para o designer e defina sua propriedade `ID` como `Categories`. Em seguida, crie um novo controle da fonte de dados a partir da marca inteligente s do repetidor. Especificamente, crie um novo controle ObjectDataSource chamado `CategoriesDataSource` que selecione seus dados do método `CategoriesBLL` Class s `GetCategories()`.

[![configurar o ObjectDataSource para usar o método CategoriesBLL da classe s GetCategories ()](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**Figura 5**: configurar o ObjectDataSource para usar o método de `GetCategories()` da classe `CategoriesBLL` ([clique para exibir a imagem em tamanho normal](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))

Ao contrário do controle DataList, para o qual o Visual Studio cria um `ItemTemplate` padrão com base na fonte de dados, os modelos do repetidor devem ser definidos manualmente. Além disso, os modelos do repetidor devem ser criados e editados declarativamente (ou seja, não há nenhuma opção Editar modelos na marca inteligente s do repetidor).

Clique na guia origem no canto inferior esquerdo e adicione um `ItemTemplate` que exibe o nome da categoria em um elemento `<h3>` e sua descrição em uma marca de parágrafo; inclua um `SeparatorTemplate` que exiba uma regra horizontal (`<hr />`) entre cada categoria. Adicione também um LinkButton com sua propriedade `Text` definida para mostrar produtos. Depois de concluir essas etapas, a marcação declarativa de s de página deve ser parecida com a seguinte:

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

A Figura 6 mostra a página quando exibida por meio de um navegador. Cada nome de categoria e descrição é listado. O botão Mostrar produtos, quando clicado, causa um postback, mas ainda não executa nenhuma ação.

[![o nome e a descrição de cada categoria são exibidos, juntamente com um LinkButton de mostrar produtos](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**Figura 6**: o nome e a descrição de cada categoria são exibidos, juntamente com um LinkButton de mostrar produtos ([clique para exibir a imagem em tamanho normal](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))

## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>Etapa 3: executando a lógica do servidor ao clicar no botão Mostrar os produtos LinkButton

Sempre que um botão, LinkButton ou ImageButton dentro de um modelo em um DataList ou Repeater é clicado, ocorre um postback e o evento DataList ou Repeater s `ItemCommand` é acionado. Além do evento `ItemCommand`, o controle DataList também poderá gerar outro evento mais específico se o botão s `CommandName` propriedade for definido como uma das cadeias de caracteres reservadas (excluir, editar, cancelar, atualizar ou selecionar), mas o evento `ItemCommand` *sempre* será acionado.

Quando um botão é clicado dentro de um DataList ou Repeater, muitas vezes precisamos passar em qual botão foi clicado (no caso de poder haver vários botões dentro do controle, como um botão Editar e excluir) e talvez algumas informações adicionais (como o valor de chave primária do item cujo botão foi clicado). O botão, LinkButton e ImageButton fornecem duas propriedades cujos valores são passados para o manipulador de eventos `ItemCommand`:

- `CommandName` uma cadeia de caracteres normalmente usada para identificar cada botão no modelo
- `CommandArgument` normalmente usado para manter o valor de algum campo de dados, como o valor da chave primária

Para este exemplo, defina a Propriedade LinkButton s `CommandName` como todos os produtos e associe o valor de chave primária do registro atual `CategoryID` à propriedade `CommandArgument` usando a sintaxe de DataBinding `CategoryArgument='<%# Eval("CategoryID") %>'`. Depois de especificar essas duas propriedades, a sintaxe de Declarative s de LinkButton deve ser parecida com a seguinte:

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

Quando o botão é clicado, ocorre um postback e o evento DataList ou Repeater s `ItemCommand` é acionado. O manipulador de eventos é passado para o botão s `CommandName` e valores de `CommandArgument`.

Crie um manipulador de eventos para o evento repetido do `ItemCommand` s e observe o segundo parâmetro passado para o manipulador de eventos (chamado `e`). Esse segundo parâmetro é do tipo [`RepeaterCommandEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) e tem as quatro propriedades a seguir:

- `CommandArgument` o valor do botão clicado s `CommandArgument` Propriedade
- `CommandName` o valor da Propriedade do botão s `CommandName`
- `CommandSource` uma referência ao controle de botão que foi clicado
- `Item` uma referência à [`RepeaterItem`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) que contém o botão que foi clicado; cada registro associado ao repetidor é manifestado como um `RepeaterItem`

Como a categoria selecionada s `CategoryID` é passada por meio da propriedade `CommandArgument`, podemos obter o conjunto de produtos associados à categoria selecionada no manipulador de eventos `ItemCommand`. Esses produtos podem então ser associados a um controle BulletedList na `ItemTemplate` (que ainda adicionamos). Tudo o que resta, é adicionar o BulletedList, referenciá-lo no manipulador de eventos `ItemCommand` e associá-lo ao conjunto de produtos para a categoria selecionada, que abordaremos na etapa 4.

> [!NOTE]
> O manipulador de eventos DataList s `ItemCommand` recebe um objeto do tipo [`DataListCommandEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), que oferece as mesmas quatro propriedades que a classe `RepeaterCommandEventArgs`.

## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>Etapa 4: exibindo os produtos da categoria selecionada em uma lista com marcadores

Os produtos selecionados da categoria s podem ser exibidos dentro do repetidor s `ItemTemplate` usando qualquer número de controles. Poderíamos adicionar outro repetidor aninhado, um DataList, um DropDownList, um GridView e assim por diante. Como queremos exibir os produtos como uma lista com marcadores, usaremos o controle BulletedList. Retornando à marcação declarativa de s de página `CustomButtons.aspx`, adicione um controle BulletedList para o `ItemTemplate` após a exibição de Products LinkButton. Defina a `ID` de BulletedLists s como `ProductsInCategory`. O BulletedList exibe o valor do campo de dados especificado por meio da propriedade `DataTextField`; como esse controle terá informações de produto associadas a ele, defina a propriedade `DataTextField` como `ProductName`.

[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

No manipulador de eventos `ItemCommand`, referencie esse controle usando `e.Item.FindControl("ProductsInCategory")` e associe-o ao conjunto de produtos associados à categoria selecionada.

[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

Antes de executar qualquer ação no manipulador de eventos `ItemCommand`, é prudente primeiro verificar o valor do `CommandName`de entrada. Como o manipulador de eventos `ItemCommand` é acionado quando *qualquer* botão é clicado, se houver vários botões no modelo, use o valor `CommandName` para discernir a ação a ser tomada. Verificar o `CommandName` aqui é sentido, já que temos apenas um único botão, mas é um bom hábito de formar. Em seguida, a `CategoryID` da categoria selecionada é recuperada da propriedade `CommandArgument`. O controle BulletedList no modelo é, então, referenciado e associado aos resultados do método `ProductsBLL` `GetProductsByCategoryID(categoryID)` da classe.

Nos tutoriais anteriores que usaram os botões em um DataList, como [uma visão geral da edição e exclusão de dados no DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md), determinamos o valor da chave primária de um determinado item por meio da coleção `DataKeys`. Embora essa abordagem funcione bem com o DataList, o repetidor não tem uma propriedade `DataKeys`. Em vez disso, devemos usar uma abordagem alternativa para fornecer o valor da chave primária, como por meio do botão s `CommandArgument` propriedade ou atribuindo o valor da chave primária a um controle da Web de rótulo oculto dentro do modelo e lendo seu valor de volta no manipulador de eventos `ItemCommand` usando `e.Item.FindControl("LabelID")`.

Depois de concluir o manipulador de eventos `ItemCommand`, Reserve um tempo para testar essa página em um navegador. Como mostra a Figura 7, clicar no link mostrar produtos causa um postback e exibe os produtos da categoria selecionada em uma BulletedList. Além disso, observe que essas informações de produto permanecem, mesmo se outras categorias mostrarem links de produtos forem clicadas.

> [!NOTE]
> Se você quiser modificar o comportamento desse relatório, de modo que os únicos produtos da categoria s sejam listados de cada vez, basta definir a propriedade do controle BulletedList s `EnableViewState` como `False`.

[![um BulletedList é usado para exibir os produtos da categoria selecionada](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**Figura 7**: uma BulletedList é usada para exibir os produtos da categoria selecionada ([clique para exibir a imagem em tamanho normal](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))

## <a name="summary"></a>Resumo

Os controles DataList e Repeater podem incluir qualquer número de botões, LinkButtons ou ImageButtons dentro de seus modelos. Esses botões, quando clicados, causam um postback e geram o evento `ItemCommand`. Para associar uma ação personalizada do lado do servidor a um botão que está sendo clicado, crie um manipulador de eventos para o evento `ItemCommand`. Nesse manipulador de eventos, primeiro verifique o valor de entrada `CommandName` para determinar qual botão foi clicado. Opcionalmente, as informações adicionais podem ser fornecidas por meio do botão s `CommandArgument` propriedade.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Dennis Patterson. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](custom-buttons-in-the-datalist-and-repeater-vb.md)
