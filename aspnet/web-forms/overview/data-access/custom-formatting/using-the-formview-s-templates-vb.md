---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
title: Usando os modelos de FormView (VB) | Microsoft Docs
author: rick-anderson
description: Ao contrário de DetailsView, o FormView não é composto de campos. Em vez disso, FormView é renderizado usando modelos. Neste tutorial, examinaremos o uso do F...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 67b25f4c-2823-42b6-b07d-1d650b3fd711
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
msc.type: authoredcontent
ms.openlocfilehash: cafe47cf5766bb14503852ec6e9f305d1e6d426f
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74618356"
---
# <a name="using-the-formviews-templates-vb"></a>Usando os modelos de FormView (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe) ou [baixar PDF](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)

> Ao contrário de DetailsView, o FormView não é composto de campos. Em vez disso, FormView é renderizado usando modelos. Neste tutorial, examinaremos o uso do controle FormView para apresentar uma exibição menos rígida de dados.

## <a name="introduction"></a>Introdução

Nos dois últimos tutoriais, vimos como personalizar as saídas dos controles GridView e DetailsView usando TemplateFields. O TemplateFields permite que o conteúdo de um campo específico seja altamente personalizado, mas no final, o GridView e o DetailsView têm uma aparência boxy, em vez de uma grade. Para muitos cenários, um layout semelhante a uma grade é ideal, mas às vezes uma exibição mais fluida e menos rígida é necessária. Ao exibir um único registro, um layout fluido é possível usando o controle FormView.

Ao contrário de DetailsView, o FormView não é composto de campos. Você não pode adicionar um BoundField ou TemplateField a um FormView. Em vez disso, FormView é renderizado usando modelos. Imagine o FormView como um controle DetailsView que contém um único TemplateField. O FormView dá suporte aos seguintes modelos:

- `ItemTemplate` usado para processar o registro específico exibido no FormView
- `HeaderTemplate` usado para especificar uma linha de cabeçalho opcional
- `FooterTemplate` usado para especificar uma linha de rodapé opcional
- `EmptyDataTemplate` quando o `DataSource` do FormView não tem nenhum registro, o `EmptyDataTemplate` é usado no lugar do `ItemTemplate` para renderizar a marcação do controle
- `PagerTemplate` pode ser usado para personalizar a interface de paginação para FormViews que têm a paginação habilitada
- `EditItemTemplate` / `InsertItemTemplate` usado para personalizar a interface de edição ou inserir a interface para FormViews que dão suporte a essa funcionalidade

Neste tutorial, examinaremos o uso do controle FormView para apresentar uma exibição menos rígida de produtos. Em vez de ter campos para o nome, a categoria, o fornecedor e assim por diante, o `ItemTemplate` do FormView mostrará esses valores usando uma combinação de um elemento de cabeçalho e uma `<table>` (consulte a Figura 1).

[![o FormView sai do layout do tipo grade visto no DetailsView](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)

**Figura 1**: o FormView se divide no layout de grade visto no DetailsView ([clique para exibir a imagem em tamanho normal](using-the-formview-s-templates-vb/_static/image3.png))

## <a name="step-1-binding-the-data-to-the-formview"></a>Etapa 1: ligando os dados ao FormView

Abra a página `FormView.aspx` e arraste um FormView da caixa de ferramentas para o designer. Ao adicionar pela primeira vez o FormView, ele aparece como uma caixa cinza, instruindo-nos que um `ItemTemplate` é necessário.

[![FormView não pode ser renderizado no designer até que um ItemTemplate seja fornecido](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)

**Figura 2**: o FormView não pode ser renderizado no designer até que um `ItemTemplate` seja fornecido ([clique para exibir a imagem em tamanho normal](using-the-formview-s-templates-vb/_static/image6.png))

O `ItemTemplate` pode ser criado manualmente (por meio da sintaxe declarativa) ou pode ser criado automaticamente ligando o FormView a um controle da fonte de dados por meio do designer. Esse `ItemTemplate` criado automaticamente contém HTML que lista o nome de cada campo e um controle de rótulo cuja propriedade `Text` está associada ao valor do campo. Essa abordagem também cria automaticamente um `InsertItemTemplate` e `EditItemTemplate`, ambos preenchidos com controles de entrada para cada um dos campos de dados retornados pelo controle da fonte de dados.

Se você quiser criar automaticamente o modelo, a partir da marca inteligente do FormView, adicione um novo controle ObjectDataSource que invoca o método `GetProducts()` da classe `ProductsBLL`. Isso criará um FormView com um `ItemTemplate`, `InsertItemTemplate`e `EditItemTemplate`. Na exibição de origem, remova o `InsertItemTemplate` e `EditItemTemplate`, já que não estamos interessados em criar um FormView que dê suporte à edição ou à inserção ainda. Em seguida, desmarque a marcação dentro do `ItemTemplate` para que tenhamos um Slate para trabalhar.

Se você preferir criar o `ItemTemplate` manualmente, poderá adicionar e configurar o ObjectDataSource arrastando-o da caixa de ferramentas para o designer. No entanto, não defina a fonte de dados do FormView do designer. Em vez disso, acesse a exibição da fonte e defina manualmente a propriedade `DataSourceID` do FormView como o valor de `ID` de ObjectDataSource. Em seguida, adicione manualmente o `ItemTemplate`.

Independentemente da abordagem que você decidiu tomar, neste ponto a marcação declarativa de FormView deve ser semelhante a:

[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample1.aspx)]

Reserve um tempo para marcar a caixa de seleção habilitar paginação na marca inteligente do FormView; Isso adicionará o atributo `AllowPaging="True"` à sintaxe declarativa do FormView. Além disso, defina a propriedade `EnableViewState` como false.

## <a name="step-2-defining-theitemtemplates-markup"></a>Etapa 2: definindo a marcação do`ItemTemplate`

Com o FormView associado ao controle ObjectDataSource e configurado para dar suporte à paginação, estamos prontos para especificar o conteúdo para o `ItemTemplate`. Para este tutorial, vamos ter o nome do produto exibido em um título de `<h3>`. Depois disso, vamos usar um `<table>` HTML para exibir as propriedades de produto restantes em uma tabela de quatro colunas em que a primeira e terceira colunas listam os nomes de propriedade e a segunda e a quarta listam seus valores.

Essa marcação pode ser inserida por meio da interface de edição de modelo do FormView no designer ou inserida manualmente por meio da sintaxe declarativa. Ao trabalhar com modelos, normalmente acho mais rápido trabalhar diretamente com a sintaxe declarativa, mas sinta-se à vontade para usar qualquer técnica com a qual esteja mais confortável.

A marcação a seguir mostra a marcação declarativa de FormView depois que a estrutura do `ItemTemplate`foi concluída:

[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample2.aspx)]

Observe que a sintaxe de DataBinding-`<%# Eval("ProductName") %>`, por exemplo, pode ser injetada diretamente na saída do modelo. Ou seja, ele não precisa ser atribuído à propriedade `Text` de um controle de rótulo. Por exemplo, temos o valor `ProductName` exibido em um elemento `<h3>` usando `<h3><%# Eval("ProductName") %></h3>`, que para o produto Chai será renderizado como `<h3>Chai</h3>`.

As classes `ProductPropertyLabel` e `ProductPropertyValue` CSS são usadas para especificar o estilo dos nomes e valores de Propriedade do produto na `<table>`. Essas classes CSS são definidas em `Styles.css` e fazem com que os nomes de propriedade sejam negrito e alinhado à direita e adicionem um preenchimento à direita aos valores de propriedade.

Como não há CheckBoxFields disponíveis com o FormView, para mostrar o valor de `Discontinued` como uma caixa de seleção, devemos adicionar nosso próprio controle de caixa de seleção. A propriedade `Enabled` é definida como false, tornando-a somente leitura e a propriedade `Checked` da caixa de seleção está associada ao valor do campo de dados `Discontinued`.

Com o `ItemTemplate` concluído, as informações do produto são exibidas de maneira muito mais fluida. Compare a saída DetailsView do último tutorial (Figura 3) com a saída gerada pelo FormView neste tutorial (Figura 4).

[![a saída de DetailsView rígida](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)

**Figura 3**: a saída DetailsView rígida ([clique para exibir a imagem em tamanho normal](using-the-formview-s-templates-vb/_static/image9.png))

[![a saída do FormView de fluido](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)

**Figura 4**: a saída do FormView de fluido ([clique para exibir a imagem em tamanho normal](using-the-formview-s-templates-vb/_static/image12.png))

## <a name="summary"></a>Resumo

Embora os controles GridView e DetailsView possam ter sua saída personalizada usando TemplateFields, ambos ainda apresentam seus dados em um formato boxy de grade. Para aqueles horários em que um único registro precisa ser mostrado usando um layout menos rígido, o FormView é uma opção ideal. Como o DetailsView, o FormView renderiza um único registro de sua `DataSource`, mas ao contrário de DetailsView, ele é composto apenas de modelos e não oferece suporte a campos.

Como vimos neste tutorial, o FormView permite um layout mais flexível ao exibir um único registro. Em Tutoriais futuros, examinaremos os controles DataList e Repeater, que fornecem o mesmo nível de flexibilidade que o FormsView, mas são capazes de exibir vários registros (como o GridView).

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de cliente potencial deste tutorial foi E.R. Gilmore. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-templatefields-in-the-detailsview-control-vb.md)
> [Próximo](displaying-summary-information-in-the-gridview-s-footer-vb.md)
