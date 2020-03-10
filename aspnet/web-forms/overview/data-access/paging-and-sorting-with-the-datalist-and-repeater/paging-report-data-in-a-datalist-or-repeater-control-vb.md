---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
title: Paginando dados de relatório em um controle DataList ou Repeater (VB) | Microsoft Docs
author: rick-anderson
description: Embora nem o DataList nem o Repeater ofereçam suporte automático à paginação ou classificação, este tutorial mostra como adicionar suporte de paginação ao DataList ou ao repetidor,...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: bbd6b7f7-b98a-48b4-93f3-341d6a4f53c0
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 5c65ca1f263e41748d99323dbdf1c28fdd077246
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78620438"
---
# <a name="paging-report-data-in-a-datalist-or-repeater-control-vb"></a>Paginação de dados de relatório em um controle DataList ou Repeater (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_VB.exe) ou [baixar PDF](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial44vb1.pdf)

> Embora nem o DataList nem o Repeater ofereçam suporte automático de paginação ou classificação, este tutorial mostra como adicionar suporte de paginação ao DataList ou Repeater, que permite interfaces de exibição de dados e paginação muito mais flexíveis.

## <a name="introduction"></a>Introdução

A paginação e a classificação são dois recursos muito comuns ao exibir dados em um aplicativo online. Por exemplo, ao procurar livros de ASP.NET em uma livraria online, pode haver centenas desses livros, mas o relatório que lista os resultados da pesquisa lista apenas dez correspondências por página. Além disso, os resultados podem ser classificados por título, preço, contagem de páginas, nome do autor e assim por diante. Como discutimos no tutorial de [dados de relatório de paginação e classificação](../paging-and-sorting/paging-and-sorting-report-data-vb.md) , os controles GridView, DetailsView e FormView fornecem suporte de paginação interna que pode ser habilitado no tique de uma caixa de seleção. O GridView também inclui suporte à classificação.

Infelizmente, nem o DataList nem o Repeater oferecem suporte automático de paginação ou classificação. Neste tutorial, examinaremos como adicionar suporte à paginação ao DataList ou Repeater. Devemos criar manualmente a interface de paginação, exibir a página de registros apropriada e lembrar a página que está sendo visitada em postagens. Embora isso leve mais tempo e código do que com GridView, DetailsView ou FormView, o DataList e o Repeater permitem uma paginação muito mais flexível e interfaces de exibição de dados.

> [!NOTE]
> Este tutorial se concentra exclusivamente na paginação. No próximo tutorial, vamos transformar nossa atenção em Adicionar recursos de classificação.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Etapa 1: adicionar as páginas da Web do tutorial de paginação e classificação

Antes de iniciar este tutorial, vamos primeiro reservar um momento para adicionar as páginas ASP.NETs que precisaremos para este tutorial e a próxima. Comece criando uma nova pasta no projeto chamada `PagingSortingDataListRepeater`. Em seguida, adicione as cinco páginas do ASP.NET a seguir a essa pasta, fazendo com que todas elas sejam configuradas para usar a página mestra `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`

![Criar uma pasta PagingSortingDataListRepeater e adicionar as páginas do tutorial ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Figura 1**: criar uma pasta `PagingSortingDataListRepeater` e adicionar as páginas do tutorial ASP.net

Em seguida, abra a página `Default.aspx` e arraste o `SectionLevelTutorialListing.ascx` controle de usuário da pasta `UserControls` para a superfície de design. Esse controle de usuário, que criamos nas [páginas mestras e](../introduction/master-pages-and-site-navigation-vb.md) no tutorial de navegação do site, enumera o mapa do site e exibe esses tutoriais na seção atual em uma lista com marcadores.

[![adicionar o controle de usuário SectionLevelTutorialListing. ascx a default. aspx](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)

**Figura 2**: Adicionar o controle de usuário `SectionLevelTutorialListing.ascx` ao `Default.aspx` ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image4.png))

Para que a lista com marcadores exiba os tutoriais de paginação e classificação que vamos criar, precisamos adicioná-los ao mapa do site. Abra o arquivo `Web.sitemap` e adicione a marcação a seguir após a edição e a exclusão com a marcação do nó do mapa do site DataList:

[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample1.xml)]

![Atualizar o mapa do site para incluir as novas páginas do ASP.NET](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)

**Figura 3**: atualizar o mapa do site para incluir as novas páginas do ASP.net

## <a name="a-review-of-paging"></a>Uma revisão da paginação

Nos tutoriais anteriores, vimos como paginar os dados nos controles GridView, DetailsView e FormView. Esses três controles oferecem uma forma simples de paginação chamada paginação *padrão* que pode ser implementada simplesmente marcando a opção habilitar paginação na marca inteligente controlar s. Com a paginação padrão, cada vez que uma página de dados é solicitada na primeira página visite ou quando o usuário navega para uma página diferente de dados, o controle GridView, DetailsView ou FormView solicita novamente *todos* os dados do ObjectDataSource. Em seguida, ele captura o conjunto específico de registros a serem exibidos, dado o índice de página solicitado e o número de registros a serem exibidos por página. Discutimos a paginação padrão em detalhes no tutorial de [dados de relatório de paginação e classificação](../paging-and-sorting/paging-and-sorting-report-data-vb.md) .

Como a paginação padrão solicita novamente todos os registros de cada página, não é prática ao paginar por meio de grandes quantidades de dados. Por exemplo, imagine paginar por meio de registros 50.000 com um tamanho de página de 10. Cada vez que o usuário se move para uma nova página, todos os registros 50.000 devem ser recuperados do banco de dados, embora apenas dez deles sejam exibidos.

A *paginação personalizada* resolve as preocupações de desempenho da paginação padrão, pegando apenas o subconjunto preciso de registros a serem exibidos na página solicitada. Ao implementar a paginação personalizada, devemos escrever a consulta SQL que retornará com eficiência apenas o conjunto correto de registros. Vimos como criar uma consulta desse tipo usando a nova [palavra-chave](http://www.4guysfromrolla.com/webtech/010406-1.shtml) do SQL Server 2005 s no novo`ROW_NUMBER()` na [paginação eficiente por meio de grandes quantidades de dados](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) .

Para implementar a paginação padrão nos controles DataList ou Repeater, podemos usar a [classe`PagedDataSource`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) como um wrapper em volta da `ProductsDataTable` cujo conteúdo está sendo paginado. A classe `PagedDataSource` tem uma propriedade `DataSource` que pode ser atribuída a qualquer objeto enumerável e as propriedades [`PageSize`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) e [`CurrentPageIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) que indicam quantos registros serão mostrados por página e o índice de página atual. Depois que essas propriedades tiverem sido definidas, o `PagedDataSource` pode ser usado como a fonte de dados de qualquer controle da Web de dados. A `PagedDataSource`, quando enumerada, retornará apenas o subconjunto apropriado de registros de sua `DataSource` interna com base nas propriedades `PageSize` e `CurrentPageIndex`. A Figura 4 descreve a funcionalidade da classe `PagedDataSource`.

![O PagedDataSource encapsula um objeto enumerável com uma interface paginável](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image6.png)

**Figura 4**: a `PagedDataSource` encapsula um objeto enumerável com uma interface paginável

O objeto `PagedDataSource` pode ser criado e configurado diretamente da camada de lógica de negócios e associado a um DataList ou Repeater por meio de um ObjectDataSource, ou pode ser criado e configurado diretamente na classe code-behind da página ASP.NET s. Se a última abordagem for usada, devemos abrem mão usando o ObjectDataSource e, em vez disso, associar os dados paginados ao DataList ou Repeater de forma programática.

O objeto `PagedDataSource` também tem propriedades para dar suporte à paginação personalizada. No entanto, podemos ignorar o uso de um `PagedDataSource` para paginação personalizada porque já temos métodos de BLL na classe `ProductsBLL` projetadas para paginação personalizada que retornam os registros precisos a serem exibidos.

Neste tutorial, vamos examinar a implementação de paginação padrão em um DataList adicionando um novo método à classe `ProductsBLL` que retorna um objeto de `PagedDataSource` configurado adequadamente. No próximo tutorial, veremos como usar a paginação personalizada.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Etapa 2: adicionando um método de paginação padrão na camada de lógica de negócios

A classe `ProductsBLL` atualmente tem um método para retornar todas as informações do produto `GetProducts()` e outra para retornar um subconjunto específico de produtos em um índice inicial `GetProductsPaged(startRowIndex, maximumRows)`. Com a paginação padrão, os controles GridView, DetailsView e FormView usam o método `GetProducts()` para recuperar todos os produtos, mas, em seguida, usam um `PagedDataSource` internamente para exibir apenas o subconjunto correto de registros. Para replicar essa funcionalidade com os controles DataList e Repeater, podemos criar um novo método na BLL que imita esse comportamento.

Adicione um método à classe `ProductsBLL` chamada `GetProductsAsPagedDataSource` que usa dois parâmetros de entrada de inteiro:

- `pageIndex` o índice da página a ser exibido, indexado em zero e
- `pageSize` o número de registros a serem exibidos por página.

`GetProductsAsPagedDataSource` inicia recuperando *todos* os registros de `GetProducts()`. Em seguida, ele cria um objeto `PagedDataSource`, definindo suas `CurrentPageIndex` e `PageSize` Propriedades com os valores dos parâmetros de `pageIndex` e de `pageSize` passados. O método termina retornando esta `PagedDataSource`configurada:

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>Etapa 3: exibindo informações do produto em um DataList usando paginação padrão

Com o método `GetProductsAsPagedDataSource` adicionado à classe `ProductsBLL`, agora podemos criar um DataList ou um repetidor que fornece paginação padrão. Comece abrindo a página `Paging.aspx` na pasta `PagingSortingDataListRepeater` e arraste uma DataList da caixa de ferramentas para o designer, definindo a propriedade DataList s `ID` como `ProductsDefaultPaging`. Na marca inteligente DataList s, crie um novo ObjectDataSource chamado `ProductsDefaultPagingDataSource` e configure-o para que ele recupere dados usando o método `GetProductsAsPagedDataSource`.

[![criar um ObjectDataSource e configurá-lo para usar o método GetProductsAsPagedDataSource ()](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Figura 5**: criar um ObjectDataSource e configurá-lo para usar o método `GetProductsAsPagedDataSource` `()` ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))

Defina as listas suspensas nas guias atualizar, inserir e excluir como (nenhum).

[![definir as listas suspensas nas guias atualizar, inserir e excluir para (nenhum)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Figura 6**: definir as listas suspensas nas guias atualizar, inserir e excluir para (nenhum) ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))

Como o método `GetProductsAsPagedDataSource` espera dois parâmetros de entrada, o assistente nos solicita a fonte desses valores de parâmetro.

Os valores de índice de página e tamanho de página devem ser lembrados em postagens. Eles podem ser armazenados no estado de exibição, persistidos na QueryString, armazenados em variáveis de sessão ou lembrados usando alguma outra técnica. Para este tutorial, usaremos a QueryString, que tem a vantagem de permitir que uma determinada página de dados seja marcada como indicador.

Em particular, use os campos QueryString pageIndex e pageSize para os parâmetros `pageIndex` e `pageSize`, respectivamente (veja a Figura 7). Reserve um momento para definir os valores padrão para esses parâmetros, pois os valores de QueryString não estarão presentes quando um usuário visitar essa página pela primeira vez. Por `pageIndex`, defina o valor padrão como 0 (que mostrará a primeira página de dados) e `pageSize` valor padrão como 4.

[![usar a QueryString como a origem dos parâmetros pageIndex e pageSize](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Figura 7**: Use o QueryString como a origem para os parâmetros `pageIndex` e `pageSize` ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image15.png))

Depois de configurar o ObjectDataSource, o Visual Studio cria automaticamente um `ItemTemplate` para o DataList. Personalize o `ItemTemplate` para que apenas o nome, a categoria e o fornecedor do produto sejam mostrados. Defina também a propriedade DataList s `RepeatColumns` como 2, sua `Width` como 100% e sua `ItemStyle` s `Width` a 50%. Essas configurações de largura fornecerão espaçamento igual para as duas colunas.

Depois de fazer essas alterações, a marcação DataList e ObjectDataSource s deve ser semelhante ao seguinte:

[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

> [!NOTE]
> Como não estamos executando nenhuma funcionalidade de atualização ou exclusão neste tutorial, você pode desabilitar o estado de exibição de DataList s para reduzir o tamanho da página renderizada.

Ao visitar esta página inicialmente por meio de um navegador, nem os parâmetros de `pageIndex` nem `pageSize` QueryString são fornecidos. Portanto, os valores padrão de 0 e 4 são usados. Como mostra a Figura 8, isso resulta em um DataList que exibe os quatro primeiros produtos.

[![os quatro primeiros produtos estão listados](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image16.png)

**Figura 8**: os quatro primeiros produtos são listados ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image18.png))

Sem uma interface de paginação, atualmente não há nenhum meio direto para que um usuário navegue até a segunda página de dados. Vamos criar uma interface de paginação na etapa 4. Por enquanto, no entanto, a paginação só pode ser realizada especificando diretamente os critérios de paginação no QueryString. Por exemplo, para exibir a segunda página, altere a URL na barra de endereços do navegador de `Paging.aspx` para `Paging.aspx?pageIndex=2` e pressione Enter. Isso faz com que a segunda página de dados seja exibida (consulte a Figura 9).

[![a segunda página de dados é exibida](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image19.png)

**Figura 9**: a segunda página de dados é exibida ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image21.png))

## <a name="step-4-creating-the-paging-interface"></a>Etapa 4: criando a interface de paginação

Há uma variedade de interfaces de paginação diferentes que podem ser implementadas. Os controles GridView, DetailsView e FormView fornecem quatro interfaces diferentes para escolher entre:

- Em **seguida,** os usuários anteriores podem mover uma página por vez, para a próxima ou anterior.
- Em **seguida, anterior, primeiro, último,** além dos botões Avançar e anterior, essa interface inclui o primeiro e o último botões para mover para a primeira ou a última página.
- **Numeric** lista os números de página na interface de paginação, permitindo que um usuário salte rapidamente para uma determinada página.
- **Numérico, primeiro, último,** além dos números de página numéricos, inclui botões para mover para a primeira ou a última página.

Para o DataList e o Repeater, somos responsáveis por decidir sobre uma interface de paginação e implementá-la. Isso envolve a criação dos controles da Web necessários na página e a exibição da página solicitada quando um botão de interface de paginação específico é clicado. Além disso, certos controles de interface de paginação talvez precisem ser desabilitados. Por exemplo, ao exibir a primeira página de dados usando a próxima, a anterior, a primeira, a última interface, os botões primeiros e anteriores seriam desabilitados.

Para este tutorial, vamos usar a próxima interface, anterior, primeiro, a última. Adicione quatro botões de controles da Web à página e defina seus `ID` s para `FirstPage`, `PrevPage`, `NextPage`e `LastPage`. Defina as propriedades `Text` como &lt;&lt; primeiro, &lt; anterior, próximo &gt;e última &gt;&gt;.

[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample4.aspx)]

Em seguida, crie um manipulador de eventos `Click` para cada um desses botões. Em alguns instantes, adicionaremos o código necessário para exibir a página solicitada.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Lembrando o número total de registros sendo paginados

Independentemente da interface de paginação selecionada, precisamos calcular e lembrar o número total de registros sendo paginados. A contagem total de linhas (em conjunto com o tamanho da página) determina quantas páginas de dados estão sendo paginadas, o que determina quais controles da interface de paginação são adicionados ou estão habilitados. Na próxima interface, anterior, primeira, última que estamos criando, a contagem de páginas é usada de duas maneiras:

- Para determinar se estamos exibindo a última página, nesse caso, os botões Avançar e último estão desabilitados.
- Se o usuário clicar no último botão, precisaremos encaixar esses itens na última página, cujo índice é um menor que a contagem de páginas.

A contagem de páginas é calculada como o teto da contagem total de linhas dividida pelo tamanho da página. Por exemplo, se estivermos paginando por meio de 79 registros com quatro registros por página, a contagem de páginas será 20 (o teto de 79/4). Se estivermos usando a interface de paginação numérica, essas informações informarão a quantos botões de página numéricos devem ser exibidos; Se nossa interface de paginação incluir botões Next ou Last, a contagem de páginas será usada para determinar quando desabilitar os botões Next ou Last.

Se a interface de paginação incluir um último botão, é imperativo que o número total de registros sendo paginados por meio de ser lembrado entre postbacks para que, quando o último botão for clicado, possamos determinar o índice da última página. Para facilitar isso, crie uma propriedade `TotalRowCount` na classe code-behind da página ASP.NET que persiste seu valor para o estado de exibição:

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

Além de `TotalRowCount`, Reserve um minuto para criar propriedades de nível de página somente leitura para acessar facilmente o índice de página, o tamanho da página e a contagem de páginas:

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample6.vb)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Determinando o número total de registros sendo paginados

O objeto `PagedDataSource` retornado do método ObjectDataSource s `Select()` tem *todos* os registros de produto, mesmo que apenas um subconjunto deles seja exibido no DataList. A propriedade `PagedDataSource` s [`Count`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) retorna apenas o número de itens que serão exibidos no DataList; a [propriedade`DataSourceCount`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) retorna o número total de itens dentro do `PagedDataSource`. Portanto, precisamos atribuir a propriedade ASP.NET s da página de `TotalRowCount` o valor da propriedade `PagedDataSource` s `DataSourceCount`.

Para fazer isso, crie um manipulador de eventos para o evento ObjectDataSource s `Selected`. No manipulador de eventos `Selected`, temos acesso ao valor de retorno do método ObjectDataSource s `Select()` nesse caso, o `PagedDataSource`.

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

## <a name="displaying-the-requested-page-of-data"></a>Exibindo a página de dados solicitada

Quando o usuário clica em um dos botões na interface de paginação, precisamos exibir a página de dados solicitada. Como os parâmetros de paginação são especificados por meio de QueryString, para mostrar a página solicitada de dados, use `Response.Redirect(url)` para que o navegador do usuário solicite novamente a página de `Paging.aspx` com os parâmetros de paginação apropriados. Por exemplo, para exibir a segunda página de dados, redirecionamos o usuário para `Paging.aspx?pageIndex=1`.

Para facilitar isso, crie um método `RedirectUser(sendUserToPageIndex)` que redireciona o usuário para `Paging.aspx?pageIndex=sendUserToPageIndex`. Em seguida, chame esse método do botão quatro `Click` manipuladores de eventos. No manipulador de eventos `FirstPage` `Click`, chame `RedirectUser(0)`para enviá-los para a primeira página; no manipulador de eventos `PrevPage` `Click`, use `PageIndex - 1` como o índice de página; e assim por diante.

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample8.vb)]

Com a `Click` os manipuladores de eventos são concluídos, os registros dos s DataList podem ser paginados clicando nos botões. Reserve um tempo para experimentá-lo!

## <a name="disabling-paging-interface-controls"></a>Desabilitando controles de interface de paginação

Atualmente, todos os quatro botões estão habilitados, independentemente da página que está sendo exibida. No entanto, queremos desabilitar os botões primeiros e anteriores ao mostrar a primeira página de dados e os botões Next e Last ao mostrar a última página. O objeto `PagedDataSource` retornado pelo método ObjectDataSource s `Select()` tem as propriedades [`IsFirstPage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) e [`IsLastPage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) que podemos examinar para determinar se estamos exibindo a primeira ou a última página de dados.

Adicione o seguinte ao manipulador de eventos ObjectDataSource s `Selected`:

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

Com essa adição, os botões primeiros e anteriores serão desabilitados ao exibir a primeira página, enquanto os botões Next e Last serão desabilitados ao exibir a última página.

Vamos concluir a interface de paginação informando ao usuário qual página está exibindo no momento e quantas páginas de total existem. Adicione um controle rótulo da Web à página e defina sua propriedade `ID` como `CurrentPageNumber`. Defina sua propriedade `Text` no manipulador de eventos ObjectDataSource s Selected, de modo que ele inclui a página atual que está sendo exibida (`PageIndex + 1`) e o número total de páginas (`PageCount`).

[!code-vb[Main](paging-report-data-in-a-datalist-or-repeater-control-vb/samples/sample10.vb)]

A Figura 10 mostra `Paging.aspx` quando visitado pela primeira vez. Como a QueryString está vazia, o DataList assume como padrão a exibição dos quatro primeiros produtos; o primeiro e o botão anterior estão desabilitados. Clicar em Avançar exibirá os quatro seguintes registros (veja a Figura 11); os botões primeiro e anterior agora estão habilitados.

[![a primeira página de dados é exibida](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image22.png)

**Figura 10**: a primeira página de dados é exibida ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image24.png))

[![a segunda página de dados é exibida](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image25.png)

**Figura 11**: a segunda página de dados é exibida ([clique para exibir a imagem em tamanho normal](paging-report-data-in-a-datalist-or-repeater-control-vb/_static/image27.png))

> [!NOTE]
> A interface de paginação pode ser aprimorada, permitindo que o usuário especifique quantas páginas serão exibidas por página. Por exemplo, um DropDownList poderia ser adicionado listando opções de tamanho de página como 5, 10, 25, 50 e todos. Ao selecionar um tamanho de página, o usuário precisaria ser Redirecionado de volta para `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Eu deixe de implementar esse aprimoramento como um exercício para o leitor.

## <a name="using-custom-paging"></a>Usando paginação personalizada

As páginas DataList por meio de seus dados usando a técnica de paginação padrão ineficiente. Ao paginar por meio de quantidades de dados suficientemente grandes, é imperativo que a paginação personalizada seja usada. Embora os detalhes da implementação sejam ligeiramente diferentes, os conceitos por trás da implementação de paginação personalizada em um DataList são os mesmos que com a paginação padrão. Com a paginação personalizada, use a classe `ProductBLL` `GetProductsPaged` método (em vez de `GetProductsAsPagedDataSource`). Conforme discutido no tutorial de [paginação eficiente por meio de grandes quantidades de dados](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) , `GetProductsPaged` deve ser passado o índice de linha inicial e o número máximo de linhas a serem retornadas. Esses parâmetros podem ser mantidos por meio de QueryString exatamente como os parâmetros `pageIndex` e `pageSize` usados na paginação padrão.

Como não há nenhum `PagedDataSource` com paginação personalizada, técnicas alternativas devem ser usadas para determinar o número total de registros sendo paginados e se exibimos novamente a primeira ou última página de dados. O método `TotalNumberOfProducts()` na classe `ProductsBLL` retorna o número total de produtos que estão sendo paginados. Para determinar se a primeira página de dados está sendo exibida, examine o índice de linha inicial se ele for zero, a primeira página será exibida. A última página será exibida se o índice de linha inicial mais o máximo de linhas a serem retornadas for maior ou igual ao número total de registros sendo paginados.

Vamos explorar a implementação de paginação personalizada com mais detalhes no próximo tutorial.

## <a name="summary"></a>Resumo

Embora nem o DataList nem o Repeater ofereçam o suporte de paginação pronto para uso nos controles GridView, DetailsView e FormView, essa funcionalidade pode ser adicionada com esforço mínimo. A maneira mais fácil de implementar a paginação padrão é encapsular todo o conjunto de produtos dentro de um `PagedDataSource` e, em seguida, associar os `PagedDataSource` ao DataList ou Repeater. Neste tutorial, adicionamos o método `GetProductsAsPagedDataSource` à classe `ProductsBLL` para retornar o `PagedDataSource`. A classe `ProductsBLL` já contém os métodos necessários para `GetProductsPaged` de paginação personalizada e `TotalNumberOfProducts`.

Juntamente com a recuperação do conjunto preciso de registros a serem exibidos para a paginação personalizada ou todos os registros em um `PagedDataSource` para paginação padrão, também precisamos adicionar manualmente a interface de paginação. Para este tutorial, criamos uma próxima interface, anterior, primeiro, a última, com quatro controles de botão da Web. Além disso, um controle rótulo exibindo o número de página atual e o número total de páginas foi adicionado.

No próximo tutorial, veremos como adicionar suporte à classificação ao DataList e ao Repeater. Também veremos como criar um DataList que pode ser paginado e classificado (com exemplos usando a paginação padrão e personalizada).

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Liz Shulok, Ken Pespisa e Bernadette Leigh. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](sorting-data-in-a-datalist-or-repeater-control-cs.md)
> [Próximo](sorting-data-in-a-datalist-or-repeater-control-vb.md)
