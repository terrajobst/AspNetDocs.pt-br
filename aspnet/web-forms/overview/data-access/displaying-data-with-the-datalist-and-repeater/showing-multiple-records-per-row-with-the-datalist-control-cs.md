---
uid: web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
title: Mostrando vários registros por linha com o controle DataList (C#) | Microsoft Docs
author: rick-anderson
description: Neste breve tutorial, exploraremos como personalizar o layout do DataList por meio de suas propriedades RepeatColumns e RepeatDirection.
ms.author: riande
ms.date: 09/13/2006
ms.assetid: cf5acaf5-d4f6-4957-badc-b89956b285f3
msc.legacyurl: /web-forms/overview/data-access/displaying-data-with-the-datalist-and-repeater/showing-multiple-records-per-row-with-the-datalist-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 3280a7b5f28207d3e640a6480f47869ce19692bc
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74638689"
---
# <a name="showing-multiple-records-per-row-with-the-datalist-control-c"></a>Exibir vários registros por linha com o controle DataList (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_31_CS.exe) ou [baixar PDF](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/datatutorial31cs1.pdf)

> Neste breve tutorial, exploraremos como personalizar o layout do DataList por meio de suas propriedades RepeatColumns e RepeatDirection.

## <a name="introduction"></a>Introdução

Os exemplos de DataList que vimos nos últimos dois tutoriais processaram cada registro de sua fonte de dados como uma linha em um `<table>`HTML de coluna única. Embora esse seja o comportamento padrão de DataList, é muito fácil personalizar a exibição DataList de modo que os itens da fonte de dados sejam distribuídos por uma tabela de várias colunas e várias linhas. Além disso, é possível ter todos os itens de fonte de dados exibidos em um DataList de linha única, com várias colunas.

Podemos personalizar o layout DataList s por meio de suas propriedades `RepeatColumns` e `RepeatDirection`, que, respectivamente, indicam Quantas colunas são renderizadas e se esses itens são dispostos vertical ou horizontalmente. A Figura 1, por exemplo, mostra um DataList que exibe informações do produto em uma tabela com três colunas.

[![o DataList mostra três produtos por linha](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image2.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image1.png)

**Figura 1**: o DataList mostra três produtos por linha ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image3.png))

Ao mostrar vários itens de fonte de dados por linha, o DataList pode utilizar com mais eficiência o espaço de tela horizontal. Neste breve tutorial, exploraremos essas duas propriedades DataList.

## <a name="step-1-displaying-product-information-in-a-datalist"></a>Etapa 1: exibindo informações do produto em um DataList

Antes de examinarmos as propriedades `RepeatColumns` e `RepeatDirection`, vamos criar primeiro um DataList em nossa página que liste as informações do produto usando o layout da tabela padrão de uma única linha e várias linhas. Para este exemplo, deixe que os s exibam o nome, a categoria e o preço do produto usando a seguinte marcação:

[!code-html[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample1.html)]

Vimos como associar dados a um DataList nos exemplos anteriores, portanto, passarei por essas etapas rapidamente. Comece abrindo a página `RepeatColumnAndDirection.aspx` na pasta `DataListRepeaterBasics` e arraste uma DataList da caixa de ferramentas para o designer. Na marca inteligente DataList s, opte por criar um novo ObjectDataSource e configure-o para efetuar pull de seus dados do método de `GetProducts` da classe `ProductsBLL`, escolhendo a opção (nenhum) nas guias inserir, atualizar e excluir do assistente.

Depois de criar e associar o novo ObjectDataSource ao DataList, o Visual Studio criará automaticamente um `ItemTemplate` que exibe o nome e o valor de cada um dos campos de dados do produto. Ajuste o `ItemTemplate` diretamente por meio da marcação declarativa ou da opção Editar modelos na marca inteligente s de DataList para que ele use a marcação mostrada acima, substituindo o *nome do produto*, o nome da *categoria*e o texto do *preço* por controles de rótulo que usam a sintaxe de DataBinding apropriada para atribuir valores às suas propriedades de `Text`. Depois de atualizar o `ItemTemplate`, a marcação declarativa de s de página deve ser semelhante ao seguinte:

[!code-aspx[Main](showing-multiple-records-per-row-with-the-datalist-control-cs/samples/sample2.aspx)]

Observe que eu incluí um especificador de formato na sintaxe `Eval` DataBinding para o `UnitPrice`, Formatando o valor retornado como uma moeda `Eval("UnitPrice", "{0:C}").`

Reserve um tempo para visitar sua página em um navegador. Como mostra a Figura 2, o DataList é renderizado como uma tabela de produtos de uma única coluna, com várias linhas.

[![por padrão, o DataList é renderizado como uma tabela de coluna única, de várias linhas](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image5.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image4.png)

**Figura 2**: por padrão, o DataList é renderizado como uma tabela de coluna única e várias linhas ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image6.png))

## <a name="step-2-changing-the-datalist-s-layout-direction"></a>Etapa 2: alterando a direção do layout de DataList s

Embora o comportamento padrão para o DataList seja dispor seus itens verticalmente em uma única tabela de várias linhas, esse comportamento pode ser facilmente alterado por meio da Propriedade DataList s [`RepeatDirection`](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatdirection.aspx). A propriedade `RepeatDirection` pode aceitar um dos dois valores possíveis: `Horizontal` ou `Vertical` (o padrão).

Ao alterar a propriedade `RepeatDirection` de `Vertical` para `Horizontal`, o DataList renderiza seus registros em uma única linha, criando uma coluna por item de fonte de dados. Para ilustrar esse efeito, clique no DataList no designer e, em seguida, na janela Propriedades, altere a propriedade `RepeatDirection` de `Vertical` para `Horizontal`. Imediatamente ao fazer isso, o designer ajusta o layout DataList s, criando uma interface de linha única e várias colunas (veja a Figura 3).

[![a propriedade RepeatDirection determina como a direção em que os itens DataList s são dispostos](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image8.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image7.png)

**Figura 3**: a propriedade `RepeatDirection` determina como a direção na qual os itens DataList s são dispostos ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image9.png))

Ao exibir pequenas quantidades de dados, uma tabela de linha única e várias colunas pode ser uma maneira ideal de maximizar o espaço da tela. No entanto, para volumes maiores de dados, uma única linha exigirá várias colunas, o que envia por push os itens que não cabem na tela à direita. A Figura 4 mostra os produtos quando renderizados em um DataList de linha única. Como há muitos produtos (mais de 80), o usuário precisará rolar para a direita para exibir informações sobre cada um dos produtos.

[![para fontes de dados suficientemente grandes, uma única coluna DataList exigirá a rolagem horizontal](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image11.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image10.png)

**Figura 4**: para fontes de dados suficientemente grandes, uma única coluna DataList exigirá a rolagem horizontal ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image12.png))

## <a name="step-3-displaying-data-in-a-multi-column-multi-row-table"></a>Etapa 3: Exibindo dados em uma tabela de várias colunas e várias linhas

Para criar um DataList de várias colunas com várias linhas, precisamos definir a [propriedade`RepeatColumns`](https://msdn.microsoft.com/system.web.ui.webcontrols.datalist.repeatcolumns.aspx) como o número de colunas a serem exibidas. Por padrão, a propriedade `RepeatColumns` é definida como 0, o que fará com que o DataList exiba todos os seus itens em uma única linha ou em uma coluna (dependendo do valor da propriedade `RepeatDirection`).

Para nosso exemplo, deixe que os s exibam três produtos por linha de tabela. Portanto, defina a propriedade `RepeatColumns` como 3. Depois de fazer essa alteração, Reserve um momento para exibir os resultados em um navegador. Como mostra a Figura 5, os produtos agora estão listados em uma tabela de três colunas, com várias linhas.

[![três produtos são exibidos por linha](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image14.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image13.png)

**Figura 5**: três produtos são exibidos por linha ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image15.png))

A propriedade `RepeatDirection` afeta como os itens no DataList são dispostos. A Figura 5 mostra os resultados com a propriedade `RepeatDirection` definida como `Horizontal`. Observe que os três primeiros produtos Chai, Chang e Aniseed Syrup são dispostos da esquerda para a direita, de cima para baixo. Os próximos três produtos (começando com o chefe Anton s Cajun) aparecem em uma linha abaixo dos três primeiros. A alteração da propriedade `RepeatDirection` de volta para `Vertical`, no entanto, apresenta esses produtos de cima para baixo, da esquerda para a direita, como ilustra a Figura 6.

[![aqui, os produtos são dispostos verticalmente](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image17.png)](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image16.png)

**Figura 6**: aqui, os produtos são dispostos verticalmente ([clique para exibir a imagem em tamanho normal](showing-multiple-records-per-row-with-the-datalist-control-cs/_static/image18.png))

O número de linhas exibidas na tabela resultante depende do número total de registros associados ao DataList. Precisamente, é o teto do número total de itens de fonte de dados dividido pelo valor da propriedade `RepeatColumns`. Como a tabela `Products` atualmente tem 84 produtos, que é divisível por 3, há 28 linhas. Se o número de itens na fonte de dados e o valor da propriedade `RepeatColumns` não forem divisíveis, a última linha ou coluna terá células em branco. Se a `RepeatDirection` for definida como `Vertical`, a última coluna terá células vazias; se `RepeatDirection` for `Horizontal`, a última linha terá as células vazias.

## <a name="summary"></a>Resumo

O DataList, por padrão, lista seus itens em uma tabela de coluna única, de várias linhas, que imita o layout de um GridView com um único TemplateField. Embora esse layout padrão seja aceitável, podemos maximizar o espaço real da tela exibindo vários itens de fonte de dados por linha. Fazer isso é simplesmente uma questão de definir a propriedade DataList s `RepeatColumns` como o número de colunas a serem exibidas por linha. Além disso, a propriedade DataList s `RepeatDirection` pode ser usada para indicar se o conteúdo da tabela de várias colunas, de várias linhas, deve ser disposto horizontalmente da esquerda para a direita, de cima para baixo ou verticalmente de cima para baixo, da esquerda para a direita.

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi John Suru. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](formatting-the-datalist-and-repeater-based-upon-data-cs.md)
> [Próximo](nested-data-web-controls-cs.md)
