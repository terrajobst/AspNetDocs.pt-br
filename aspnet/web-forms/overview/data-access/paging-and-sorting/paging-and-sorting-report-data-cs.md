---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
title: Paginando e classificandoC#dados de relatório () | Microsoft Docs
author: rick-anderson
description: A paginação e a classificação são dois recursos muito comuns ao exibir dados em um aplicativo online. Neste tutorial, vamos dar uma olhada primeiro na adição de classificação e...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 811a6ef2-ec66-4c8e-a089-6f795056e288
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 2f77040316dadc218b8183e52628dc0cfe3b35a1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78619514"
---
# <a name="paging-and-sorting-report-data-c"></a>Paginação e classificação de dados de relatórios (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_CS.exe) ou [baixar PDF](paging-and-sorting-report-data-cs/_static/datatutorial24cs1.pdf)

> A paginação e a classificação são dois recursos muito comuns ao exibir dados em um aplicativo online. Neste tutorial, vamos examinar primeiro a adição de classificação e paginação aos nossos relatórios, que, em seguida, vamos desenvolver em Tutoriais futuros.

## <a name="introduction"></a>Introdução

A paginação e a classificação são dois recursos muito comuns ao exibir dados em um aplicativo online. Por exemplo, ao procurar livros de ASP.NET em uma livraria online, pode haver centenas desses livros, mas o relatório que lista os resultados da pesquisa lista apenas dez correspondências por página. Além disso, os resultados podem ser classificados por título, preço, contagem de páginas, nome do autor e assim por diante. Enquanto os últimos 23 tutoriais examinaram como criar uma variedade de relatórios, incluindo interfaces que permitem adicionar, editar e excluir dados, não vimos como classificar dados e os únicos exemplos de paginação que vimos com o DetailsView e o FormView controles.

Neste tutorial, veremos como adicionar classificação e paginação aos nossos relatórios, que podem ser realizados simplesmente marcando algumas caixas de seleção. Infelizmente, essa implementação simples tem suas desvantagens. a interface de classificação deixa um pouco desejado e as rotinas de paginação não são projetadas para paginar com eficiência por meio de grandes conjuntos de resultados. Os tutoriais futuros irão explorar como superar as limitações das soluções de paginação e classificação prontas para uso.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Etapa 1: adicionar as páginas da Web do tutorial de paginação e classificação

Antes de iniciar este tutorial, vamos primeiro reservar um momento para adicionar as páginas ASP.NETs que precisaremos para este tutorial e os três seguintes. Comece criando uma nova pasta no projeto chamada `PagingAndSorting`. Em seguida, adicione as cinco páginas do ASP.NET a seguir a essa pasta, fazendo com que todas elas sejam configuradas para usar a página mestra `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`

![Criar uma pasta PagingAndSorting e adicionar as páginas do tutorial ASP.NET](paging-and-sorting-report-data-cs/_static/image1.png)

**Figura 1**: criar uma pasta PagingAndSorting e adicionar as páginas do tutorial ASP.net

Em seguida, abra a página `Default.aspx` e arraste o `SectionLevelTutorialListing.ascx` controle de usuário da pasta `UserControls` para a superfície de design. Esse controle de usuário, que criamos nas [páginas mestras e](../introduction/master-pages-and-site-navigation-cs.md) no tutorial de navegação do site, enumera o mapa do site e exibe esses tutoriais na seção atual em uma lista com marcadores.

![Adicione o controle de usuário SectionLevelTutorialListing. ascx a default. aspx](paging-and-sorting-report-data-cs/_static/image2.png)

**Figura 2**: Adicionar o controle de usuário SectionLevelTutorialListing. ascx a default. aspx

Para que a lista com marcadores exiba os tutoriais de paginação e classificação que vamos criar, precisamos adicioná-los ao mapa do site. Abra o arquivo `Web.sitemap` e adicione a marcação a seguir após a edição, inserção e exclusão da marcação de nó do mapa do site:

[!code-xml[Main](paging-and-sorting-report-data-cs/samples/sample1.xml)]

![Atualizar o mapa do site para incluir as novas páginas do ASP.NET](paging-and-sorting-report-data-cs/_static/image3.png)

**Figura 3**: atualizar o mapa do site para incluir as novas páginas do ASP.net

## <a name="step-2-displaying-product-information-in-a-gridview"></a>Etapa 2: exibindo informações do produto em um GridView

Antes de realmente implementar os recursos de paginação e classificação, vamos primeiro criar um GridView padrão não classificável e não-paginável que liste as informações do produto. Essa é uma tarefa que fizemos muitas vezes antes desta série de tutoriais para que essas etapas devam ser familiares. Comece abrindo a página `SimplePagingSorting.aspx` e arraste um controle GridView da caixa de ferramentas para o designer, definindo sua propriedade `ID` como `Products`. Em seguida, crie um novo ObjectDataSource que usa o método ProductsBLL da classe s `GetProducts()` para retornar todas as informações do produto.

![Recuperar informações sobre todos os produtos usando o método GetProducts ()](paging-and-sorting-report-data-cs/_static/image4.png)

**Figura 4**: recuperar informações sobre todos os produtos usando o método GetProducts ()

Como esse relatório é um relatório somente leitura, não há necessidade de mapear os métodos ObjectDataSource s `Insert()`, `Update()`ou `Delete()` para os métodos `ProductsBLL` correspondentes; Portanto, escolha (nenhum) na lista suspensa para as guias atualizar, inserir e excluir.

![Escolha a opção (nenhum) na lista suspensa nas guias atualizar, inserir e excluir](paging-and-sorting-report-data-cs/_static/image5.png)

**Figura 5**: escolha a opção (nenhum) na lista suspensa nas guias atualizar, inserir e excluir

Em seguida, vamos personalizar os campos GridView de forma que apenas os nomes de produtos, fornecedores, categorias, preços e status descontinuados sejam exibidos. Além disso, sinta-se à vontade para fazer alterações de formatação no nível de campo, como ajustar as propriedades de `HeaderText` ou formatar o preço como uma moeda. Após essas alterações, sua marcação declarativa de GridView deve ser semelhante ao seguinte:

[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample2.aspx)]

A Figura 6 mostra nosso progresso até o momento, quando visualizado por meio de um navegador. Observe que a página lista todos os produtos em uma tela, mostrando o nome, a categoria, o fornecedor, o preço e o status descontinuados do produto.

[![cada um dos produtos está listado](paging-and-sorting-report-data-cs/_static/image7.png)](paging-and-sorting-report-data-cs/_static/image6.png)

**Figura 6**: cada um dos produtos listados ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image8.png))

## <a name="step-3-adding-paging-support"></a>Etapa 3: adicionando suporte à paginação

Listar *todos* os produtos em uma tela pode levar à sobrecarga de informações para o usuário usando os dados. Para ajudar a tornar os resultados mais gerenciáveis, podemos dividir os dados em páginas menores de dados e permitir que o usuário percorra os dados uma página por vez. Para fazer isso, basta marcar a caixa de seleção habilitar paginação na marca inteligente s de GridView (isso define a Propriedade GridView s [`AllowPaging`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) como `true`).

[![marque a caixa de seleção habilitar paginação para adicionar suporte à paginação](paging-and-sorting-report-data-cs/_static/image10.png)](paging-and-sorting-report-data-cs/_static/image9.png)

**Figura 7**: marque a caixa de seleção habilitar paginação para adicionar suporte à paginação ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image11.png))

Habilitar a paginação limita o número de registros mostrados por página e adiciona uma *interface de paginação* ao GridView. A interface de paginação padrão, mostrada na Figura 7, é uma série de números de página, permitindo que o usuário navegue rapidamente de uma página de dados para outra. Essa interface de paginação deve parecer familiar, conforme vimos ao adicionar suporte de paginação aos controles DetailsView e FormView nos tutoriais anteriores.

Os controles DetailsView e FormView mostram apenas um único registro por página. O GridView, no entanto, consulta sua [propriedade`PageSize`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) para determinar quantos registros mostrar por página (essa propriedade usa como padrão um valor de 10).

Esta interface de paginação de GridView, DetailsView e FormView s pode ser personalizada usando as seguintes propriedades:

- `PagerStyle` indica as informações de estilo para a interface de paginação; pode especificar configurações como `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`e assim por diante.
- `PagerSettings` contém um apresentação de propriedades que pode personalizar a funcionalidade da interface de paginação; `PageButtonCount` indica o número máximo de números de página numéricos exibidos na interface de paginação (o padrão é 10); a [propriedade`Mode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) indica como a interface de paginação Opera e pode ser definida como: 

    - `NextPrevious` mostra os botões seguintes e anteriores, permitindo que o usuário percorra ou avance uma página de cada vez
    - `NextPreviousFirstLast` além dos botões Next e Previous, os primeiros e os últimos botões também estão incluídos, permitindo que o usuário se mova rapidamente para a primeira ou a última página de dados
    - `Numeric` mostra uma série de números de página, permitindo que o usuário salte imediatamente para qualquer página
    - `NumericFirstLast` além dos números de página, inclui o primeiro e o último botões, permitindo que o usuário passe rapidamente para a primeira ou última página de dados; os primeiros/últimos botões são mostrados apenas se todos os números de página numéricos não couberem

Além disso, o GridView, o DetailsView e o FormView oferecem as propriedades `PageIndex` e `PageCount`, que indicam a página atual que está sendo exibida e o número total de páginas de dados, respectivamente. A propriedade `PageIndex` é indexada a partir de 0, o que significa que, ao exibir a primeira página de dados `PageIndex` será igual a 0. `PageCount`, por outro lado, começa a contagem em 1, o que significa que `PageIndex` é limitado aos valores entre 0 e `PageCount - 1`.

Vamos reservar um momento para melhorar a aparência padrão de nossa interface de paginação de GridView. Especificamente, deixe que os s tenham a interface de paginação alinhada à direita com um plano de fundo cinza claro. Em vez de definir essas propriedades diretamente por meio da Propriedade GridView s `PagerStyle`, vamos criar uma classe CSS em `Styles.css` chamado `PagerRowStyle` e atribuir a propriedade `PagerStyle` s `CssClass` por meio de nosso tema. Comece abrindo `Styles.css` e adicionando a seguinte definição de classe CSS:

[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample3.css)]

Em seguida, abra o arquivo `GridView.skin` na pasta `DataWebControls` dentro da pasta `App_Themes`. Como discutimos nas *páginas mestras e* no tutorial de navegação do site, os arquivos de capa podem ser usados para especificar os valores de propriedade padrão para um controle da Web. Portanto, aumente as configurações existentes para incluir a definição da propriedade `PagerStyle` s `CssClass` como `PagerRowStyle`. Além disso, vamos configurar a interface de paginação para mostrar no máximo cinco botões de página numéricos usando a interface de paginação `NumericFirstLast`.

[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>A experiência do usuário de paginação

A Figura 8 mostra a página da Web quando visitada por meio de um navegador depois que a caixa de seleção de habilitar paginação do GridView é marcada e as configurações de `PagerStyle` e `PagerSettings` foram feitas por meio do arquivo de `GridView.skin`. Observe como apenas dez registros são mostrados e a interface de paginação indica que estamos exibindo a primeira página de dados.

[![com a paginação habilitada, apenas um subconjunto dos registros é exibido por vez](paging-and-sorting-report-data-cs/_static/image13.png)](paging-and-sorting-report-data-cs/_static/image12.png)

**Figura 8**: com a paginação habilitada, apenas um subconjunto dos registros é exibido de cada vez ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image14.png))

Quando o usuário clica em um dos números de página na interface de paginação, um postback massacre e a página recarrega mostrando os registros de s de página solicitados. A Figura 9 mostra os resultados depois de optar por exibir a página final dos dados. Observe que a página final tem apenas um registro; Isso ocorre porque há 81 registros no total, resultando em oito páginas de 10 registros por página, além de uma página com um registro solitário.

[![clicar em um número de página causa um postback e mostra o subconjunto apropriado de registros](paging-and-sorting-report-data-cs/_static/image16.png)](paging-and-sorting-report-data-cs/_static/image15.png)

**Figura 9**: clicar em um número de página causa um postback e mostra o subconjunto apropriado de registros ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image17.png))

## <a name="paging-s-server-side-workflow"></a>Paginando o fluxo de trabalho do lado do servidor

Quando o usuário final clica em um botão na interface de paginação, um postback massacre e o seguinte fluxo de trabalho do lado do servidor são iniciados:

1. O evento de `PageIndexChanging` GridView s (ou DetailsView ou FormView) é acionado
2. O ObjectDataSource solicita novamente *todos* os dados da BLL; os valores de propriedade de `PageIndex` e de `PageSize` GridView são usados para determinar quais registros retornados da BLL precisam ser exibidos no GridView
3. O evento de `PageIndexChanged` do GridView é acionado

Na etapa 2, o ObjectDataSource solicita novamente todos os dados de sua fonte de dados. Esse estilo de paginação é conhecido como *paginação padrão*, pois ele é o comportamento de paginação usado por padrão ao definir a propriedade `AllowPaging` como `true`. Com a paginação padrão, o controle da Web de dados naively recupera todos os registros de cada página de dados, mesmo que apenas um subconjunto de registros seja realmente processado no HTML enviado ao navegador. A menos que os dados do banco de dados sejam armazenados em cache pela BLL ou ObjectDataSource, a paginação padrão não funcionará para conjuntos de resultados suficientemente grandes ou para aplicativos Web com muitos usuários simultâneos.

No próximo tutorial, examinaremos como implementar a *paginação personalizada*. Com a paginação personalizada, você pode instruir especificamente o ObjectDataSource a recuperar apenas o conjunto preciso de registros necessários para a página de dados solicitada. Como você pode imaginar, a paginação personalizada melhora muito a eficiência da paginação por meio de grandes conjuntos de resultados.

> [!NOTE]
> Embora a paginação padrão não seja adequada durante a paginação por meio de conjuntos de resultados suficientemente grandes ou para sites com muitos usuários simultâneos, perceba que a paginação personalizada requer mais alterações e esforço para implementar e não é tão simples quanto marcar uma caixa de seleção (como é o padrão paginação). Portanto, a paginação padrão pode ser a opção ideal para sites pequenos de tráfego baixo ou ao paginar por meio de conjuntos de resultados relativamente pequenos, à medida que eles são muito mais fáceis e rápidos de implementar.

Por exemplo, se sabemos que nunca teremos mais de 100 produtos em nosso banco de dados, o máximo de impacto de desempenho pela paginação personalizada é provavelmente compensado pelo esforço necessário para implementá-lo. No entanto, se um dia pode ter milhares ou dezenas de milhares de produtos, *não* implementar a paginação personalizada dificultaria muito a escalabilidade de nosso aplicativo.

## <a name="step-4-customizing-the-paging-experience"></a>Etapa 4: Personalizando a experiência de paginação

Os controles da Web de dados fornecem várias propriedades que podem ser usadas para aprimorar a experiência de paginação do usuário. A propriedade `PageCount`, por exemplo, indica quantas páginas no total existem, enquanto a propriedade `PageIndex` indica a página atual que está sendo visitada e pode ser definida para mover rapidamente um usuário para uma página específica. Para ilustrar como usar essas propriedades para melhorar a experiência de paginação do usuário, vamos adicionar um controle rótulo da Web à nossa página que informa ao usuário qual página ele está visitando no momento, junto com um controle DropDownList que permite que eles saltem rapidamente para qualquer página específica .

Primeiro, adicione um controle rótulo da Web à sua página, defina sua propriedade `ID` como `PagingInformation`e desmarque sua propriedade `Text`. Em seguida, crie um manipulador de eventos para o evento GridView s `DataBound` e adicione o seguinte código:

[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample5.cs)]

Esse manipulador de eventos atribui o `PagingInformation` rótulo s `Text` Propriedade a uma mensagem informando ao usuário a página que ele está visitando atualmente `Products.PageIndex + 1` de quantas páginas de total `Products.PageCount` (adicionamos 1 à propriedade `Products.PageIndex` porque `PageIndex` é indexado a partir de 0). Escolhi a propriedade atribuir este rótulo `Text` no manipulador de eventos `DataBound` em oposição ao manipulador de eventos `PageIndexChanged` porque o evento `DataBound` é acionado sempre que os dados são associados ao GridView, enquanto o manipulador de eventos `PageIndexChanged` só é acionado quando o índice da página é alterado. Quando o GridView é inicialmente ligado a dados na primeira página, o evento `PageIndexChanging` não é acionado (enquanto o evento `DataBound` faz).

Com essa adição, agora o usuário mostra uma mensagem indicando qual página está visitando e quantas páginas do total de dados existem.

[![o número de página atual e o número total de páginas são exibidos](paging-and-sorting-report-data-cs/_static/image19.png)](paging-and-sorting-report-data-cs/_static/image18.png)

**Figura 10**: o número de página atual e o número total de páginas são exibidos ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image20.png))

Além do controle rótulo, vamos adicionar também um controle DropDownList que lista os números de página no GridView com a página exibida atualmente selecionada. A ideia aqui é que o usuário pode rapidamente ir da página atual para outra simplesmente selecionando o novo índice de página da DropDownList. Comece adicionando um DropDownList ao designer, definindo sua propriedade `ID` como `PageList` e marcando a opção Habilitar AutoPostBack a partir de sua marca inteligente.

Em seguida, retorne ao manipulador de eventos `DataBound` e adicione o seguinte código:

[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample6.cs)]

Esse código começa limpando os itens na `PageList` DropDownList. Isso pode parecer supérfluo, uma vez que não esperaria que o número de páginas fosse alterado, mas outros usuários podem estar usando o sistema simultaneamente, adicionando ou removendo registros da tabela `Products`. Essas inserções ou exclusões podem alterar o número de páginas de dados.

Em seguida, precisamos criar os números de página novamente e ter aquele que mapeia para o GridView atual `PageIndex` selecionado por padrão. Realizamos isso com um loop de 0 a `PageCount - 1`, adicionando um novo `ListItem` em cada iteração e definindo sua propriedade `Selected` como true se o índice de iteração atual for igual à Propriedade GridView s `PageIndex`.

Finalmente, precisamos criar um manipulador de eventos para o evento DropDownList s `SelectedIndexChanged`, que é acionado sempre que o usuário escolhe um item diferente da lista. Para criar esse manipulador de eventos, basta clicar duas vezes em DropDownList no designer e, em seguida, adicionar o seguinte código:

[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample7.cs)]

Como mostra a Figura 11, a simples alteração da Propriedade GridView s `PageIndex` faz com que os dados sejam reassociados ao GridView. No manipulador de eventos GridView s `DataBound`, o `ListItem` DropDownList apropriado é selecionado.

[![o usuário é levado automaticamente para a sexta página ao selecionar o item da lista suspensa página 6](paging-and-sorting-report-data-cs/_static/image22.png)](paging-and-sorting-report-data-cs/_static/image21.png)

**Figura 11**: o usuário é levado automaticamente para a sexta página ao selecionar o item de lista suspensa página 6 ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image23.png))

## <a name="step-5-adding-bi-directional-sorting-support"></a>Etapa 5: adicionando suporte à classificação bidirecional

Adicionar suporte à classificação bidirecional é tão simples quanto adicionar suporte à paginação, basta marcar a opção Habilitar classificação na marca inteligente s de GridView (que define a Propriedade GridView s [`AllowSorting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) como `true`). Isso renderiza cada um dos cabeçalhos dos campos GridView como LinkButtons que, quando clicados, causa um postback e retorna os dados classificados pela coluna clicada em ordem crescente. Clicar no mesmo cabeçalho LinkButton novamente reclassifica os dados em ordem decrescente.

> [!NOTE]
> Se você estiver usando uma camada de acesso a dados personalizada em vez de um DataSet tipado, talvez não tenha uma opção Habilitar classificação na marca inteligente s do GridView. Somente os GridViews vinculados a fontes de dados que dão suporte à classificação nativamente têm essa caixa de seleção disponível. O conjunto de dado tipado fornece suporte de classificação pronto para uso, já que a DataTable ADO.NET fornece um método de `Sort` que, quando invocado, classifica as colunas de DataTable s usando os critérios especificados.

Se sua DAL não retornar objetos que oferecem suporte nativo à classificação, você precisará configurar o ObjectDataSource para passar informações de classificação para a camada de lógica de negócios, que pode classificar os dados ou ter os dados classificados pela DAL. Exploraremos como classificar dados na lógica de negócios e nas camadas de acesso a dados em um tutorial futuro.

Os LinkButtons de classificação são renderizados como hiperlinks HTML, cujas cores atuais (azuis para um link não visitado e um vermelho escuro para um link visitado) estão em conflito com a cor do plano de fundo da linha do cabeçalho. Em vez disso, deixe que s todos os links de linha de cabeçalho sejam exibidos em branco, independentemente de terem sido visitados ou não. Isso pode ser feito adicionando o seguinte à classe `Styles.css`:

[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample8.css)]

Essa sintaxe indica o uso de texto em branco ao exibir esses hiperlinks dentro de um elemento que usa a classe HeaderStyle.

Após essa adição de CSS, ao visitar a página por meio de um navegador, sua tela deverá ser semelhante à figura 12. Em particular, a Figura 12 mostra os resultados depois que o link do cabeçalho s do campo de preço é clicado.

[![os resultados foram classificados pelo PreçoUnitário em ordem crescente](paging-and-sorting-report-data-cs/_static/image25.png)](paging-and-sorting-report-data-cs/_static/image24.png)

**Figura 12**: os resultados foram classificados pelo PreçoUnitário em ordem crescente ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image26.png))

## <a name="examining-the-sorting-workflow"></a>Examinando o fluxo de trabalho de classificação

Todos os campos GridView de BoundField, CheckBoxField, TemplateField e assim por diante têm uma propriedade `SortExpression` que indica a expressão que deve ser usada para classificar os dados quando o link de cabeçalho de classificação do campo s é clicado. O GridView também tem uma propriedade `SortExpression`. Quando um cabeçalho de classificação LinkButton é clicado, o GridView atribui esse campo `SortExpression` valor à sua propriedade `SortExpression`. Em seguida, os dados são recuperados novamente do ObjectDataSource e classificados de acordo com a Propriedade GridView s `SortExpression`. A lista a seguir fornece detalhes sobre a sequência de etapas que ocorre quando um usuário final classifica os dados em um GridView:

1. O evento de [classificação](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) GridView s é acionado
2. A Propriedade GridView s [`SortExpression`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) é definida como a `SortExpression` do campo cujo cabeçalho de classificação LinkButton foi clicado
3. O ObjectDataSource recupera novamente todos os dados da BLL e, em seguida, classifica os dados usando o `SortExpression` de controle GridView
4. A Propriedade GridView s `PageIndex` é redefinida como 0, o que significa que, quando a classificação do usuário é retornada para a primeira página de dados (supondo que o suporte à paginação tenha sido implementado)
5. O evento de [`Sorted`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) do GridView é acionado

Assim como na paginação padrão, a opção de classificação padrão recupera novamente *todos* os registros da BLL. Ao usar a classificação sem paginação ou ao usar a classificação com paginação padrão, não há nenhuma maneira de evitar esse impacto de desempenho (a menor do cache dos dados do banco de dados). No entanto, como veremos em um futuro tutorial, é possível classificar dados com eficiência ao usar a paginação personalizada.

Ao associar um ObjectDataSource ao GridView por meio da lista suspensa na marca inteligente s, cada campo GridView tem automaticamente sua propriedade `SortExpression` atribuída ao nome do campo de dados na classe `ProductsRow`. Por exemplo, o `SortExpression` `ProductName` BoundField s é definido como `ProductName`, conforme mostrado na seguinte marcação declarativa:

[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample9.aspx)]

Um campo pode ser configurado para que não seja classificável desmarcando sua propriedade `SortExpression` (atribuindo-a a uma cadeia de caracteres vazia). Para ilustrar isso, imagine que não queremos deixar nossos clientes classificarem nossos produtos por preço. A propriedade `UnitPrice` BoundField s `SortExpression` pode ser removida da marcação declarativa ou da caixa de diálogo campos (que pode ser acessada clicando no link Editar colunas na marca inteligente s do GridView).

![Os resultados foram classificados pelo PreçoUnitário em ordem crescente](paging-and-sorting-report-data-cs/_static/image27.png)

**Figura 13**: os resultados foram classificados pelo PreçoUnitário em ordem crescente

Depois que a propriedade `SortExpression` tiver sido removida para o `UnitPrice` BoundField, o cabeçalho será renderizado como texto, e não como um link, impedindo, assim, que os usuários classifiquem os dados por preço.

[![removendo a propriedade SortExpression, os usuários não podem mais classificar os produtos por preço](paging-and-sorting-report-data-cs/_static/image29.png)](paging-and-sorting-report-data-cs/_static/image28.png)

**Figura 14**: ao remover a propriedade SortExpression, os usuários não podem mais classificar os produtos por preço ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image30.png))

## <a name="programmatically-sorting-the-gridview"></a>Classificando programaticamente o GridView

Você também pode classificar o conteúdo do GridView programaticamente usando o método GridView s [`Sort`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx). Basta passar o valor de `SortExpression` para classificar junto com a [`SortDirection`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` ou `Descending`) e os dados de GridView serão reclassificados.

Imagine que o motivo pelo qual desativamos a classificação pela `UnitPrice` era porque ficamos preocupados com o fato de nossos clientes simplesmente comprarem apenas os produtos com preços mais baixos. No entanto, queremos encoraja-los a comprar os produtos mais caros, portanto, d como eles podem classificar os produtos por preço, mas apenas do preço mais caro pelo menos.

Para fazer isso, adicione um controle de botão da Web à página, defina sua propriedade `ID` como `SortPriceDescending`e sua propriedade `Text` para classificar por preço. Em seguida, crie um manipulador de eventos para o botão s `Click` evento clicando duas vezes no controle Button no designer. Adicione o seguinte código a este manipulador de eventos:

[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample10.cs)]

Clicar nesse botão retorna o usuário para a primeira página com os produtos classificados por preço, do mais caro ao menos dispendioso (consulte a Figura 15).

[![clicar no botão ordena os produtos do mais caro para o menos](paging-and-sorting-report-data-cs/_static/image32.png)](paging-and-sorting-report-data-cs/_static/image31.png)

**Figura 15**: clicar no botão ordena os produtos do mais caro para o menos ([clique para exibir a imagem em tamanho normal](paging-and-sorting-report-data-cs/_static/image33.png))

## <a name="summary"></a>Resumo

Neste tutorial, vimos como implementar recursos de paginação e de classificação padrão, ambos que eram tão fáceis quanto marcar uma caixa de seleção! Quando um usuário classifica ou páginas por meio de dados, um fluxo de trabalho semelhante é desdobrado:

1. Um massacre de postback
2. O evento de pré nível do controle de Web de dados é acionado (`PageIndexChanging` ou `Sorting`)
3. Todos os dados são recuperados novamente pelo ObjectDataSource
4. O evento de nível de s do controle da Web de dados é acionado (`PageIndexChanged` ou `Sorted`)

Embora a implementação de paginação e classificação básica seja uma Breeze, mais esforço deve ser utilizado para utilizar a paginação personalizada mais eficiente ou para aprimorar ainda mais a interface de paginação ou classificação. Os tutoriais futuros irão explorar esses tópicos.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Próximo](efficiently-paging-through-large-amounts-of-data-cs.md)
