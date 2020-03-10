---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
title: Criando uma interface de usuário de classificação personalizada (VB) | Microsoft Docs
author: rick-anderson
description: Ao exibir uma longa lista de dados classificados, pode ser muito útil agrupar dados relacionados introduzindo linhas separadoras. Neste tutorial, veremos como CRE...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: f3897a74-cc6a-4032-8f68-465f155e296a
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 66127630560141cd795beb15f525a7fba85f3993
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78619920"
---
# <a name="creating-a-customized-sorting-user-interface-vb"></a>Criação de uma interface do usuário de classificação personalizada (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_VB.exe) ou [baixar PDF](creating-a-customized-sorting-user-interface-vb/_static/datatutorial27vb1.pdf)

> Ao exibir uma longa lista de dados classificados, pode ser muito útil agrupar dados relacionados introduzindo linhas separadoras. Neste tutorial, veremos como criar uma interface de usuário de classificação.

## <a name="introduction"></a>Introdução

Ao exibir uma longa lista de dados classificados em que há apenas alguns valores diferentes na coluna classificada, um usuário final pode achar difícil distinguir onde, exatamente, os limites de diferença ocorrem. Por exemplo, há 81 produtos no banco de dados, mas apenas nove opções de categoria diferentes (oito categorias exclusivas mais a opção `NULL`). Considere o caso de um usuário que esteja interessado em examinar os produtos que se enquadram na categoria de frutos do mar. Em uma página que lista *todos* os produtos em um único GridView, o usuário pode decidir que a melhor aposta é classificar os resultados por categoria, que agrupará todos os produtos de frutos do mar. Após a classificação por categoria, o usuário precisará procurar na lista, procurando onde os produtos em grupo de frutos do mar começam e terminam. Como os resultados são ordenados alfabeticamente pelo nome da categoria encontrar os produtos do frutos do mar não é difícil, mas ele ainda requer uma verificação minuciosa da lista de itens na grade.

Para ajudar a realçar os limites entre grupos classificados, muitos sites empregam uma interface do usuário que adiciona um separador entre esses grupos. Os separadores como os mostrados na Figura 1 permitem que um usuário encontre mais rapidamente um determinado grupo e identifique seus limites, além de determinar quais grupos distintos existem nos dados.

[![cada grupo de categorias é claramente identificado](creating-a-customized-sorting-user-interface-vb/_static/image2.png)](creating-a-customized-sorting-user-interface-vb/_static/image1.png)

**Figura 1**: cada grupo de categorias é identificado claramente ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image3.png))

Neste tutorial, veremos como criar uma interface de usuário de classificação.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Etapa 1: Criando um GridView padrão e classificável

Antes de explorarmos como aumentar o GridView para fornecer a interface de classificação avançada, vamos primeiro criar um GridView padrão e classificável que liste os produtos. Comece abrindo a página de `CustomSortingUI.aspx` na pasta `PagingAndSorting`. Adicione um GridView à página, defina sua propriedade `ID` como `ProductList`e associe-a a um novo ObjectDataSource. Configure o ObjectDataSource para usar o método de `GetProducts()` da classe `ProductsBLL` para selecionar registros.

Em seguida, configure o GridView de modo que ele contenha apenas os `ProductName`, `CategoryName`, `SupplierName`e `UnitPrice` BoundFields e o CheckBoxField descontinuado. Por fim, configure o GridView para dar suporte à classificação marcando a caixa de seleção Habilitar classificação na marca inteligente s GridView (ou definindo sua propriedade `AllowSorting` como `true`). Depois de fazer essas adições à página de `CustomSortingUI.aspx`, a marcação declarativa deve ser semelhante ao seguinte:

[!code-aspx[Main](creating-a-customized-sorting-user-interface-vb/samples/sample1.aspx)]

Reserve um tempo para exibir nosso progresso até o momento em um navegador. A Figura 2 mostra o GridView classificável quando seus dados são classificados por categoria em ordem alfabética.

[![os dados GridView classificável são ordenados por categoria](creating-a-customized-sorting-user-interface-vb/_static/image5.png)](creating-a-customized-sorting-user-interface-vb/_static/image4.png)

**Figura 2**: os dados de GridView s classificável são ordenados por categoria ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image6.png))

## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Etapa 2: explorando as técnicas para adicionar as linhas do separador

Com o GridView genérico, classificável concluído, tudo o que resta é poder adicionar as linhas separadoras no GridView antes de cada grupo classificado exclusivo. Mas como essas linhas podem ser injetadas no GridView? Basicamente, precisamos iterar nas linhas GridView, determinar onde ocorrem as diferenças entre os valores na coluna classificada e, em seguida, adicionar a linha separadora apropriada. Ao pensar sobre esse problema, parece natural que a solução está em algum lugar no manipulador de eventos GridView s `RowDataBound`. Como discutimos no tutorial [formatação personalizada com base no data](../custom-formatting/custom-formatting-based-upon-data-vb.md) , esse manipulador de eventos é comumente usado ao aplicar a formatação em nível de linha com base nos dados de linha s. No entanto, o manipulador de eventos `RowDataBound` não é a solução aqui, pois as linhas não podem ser adicionadas ao GridView programaticamente desse manipulador de eventos. A coleção GridView s `Rows`, na verdade, é somente leitura.

Para adicionar linhas adicionais ao GridView, temos três opções:

- Adicionar essas linhas de separador de metadados aos dados reais associados ao GridView
- Depois que o GridView tiver sido associado aos dados, adicione instâncias de `TableRow` adicionais à coleção de controle GridView s
- Criar um controle de servidor personalizado que estende o controle GridView e substitui os métodos responsáveis pela construção da estrutura GridView

A criação de um controle de servidor personalizado seria a melhor abordagem se essa funcionalidade fosse necessária em muitas páginas da Web ou em vários sites. No entanto, isso envolveria um pouco de código e uma exploração completa sobre as profundidades dos trabalhos internos de GridView. Portanto, não vamos considerar essa opção para este tutorial.

As outras duas opções adicionam linhas separadoras aos dados reais que estão sendo associados ao GridView e manipulando a coleção de controle GridView s depois de ser associada-atacar o problema de forma diferente e merece uma discussão.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Adicionando linhas aos dados associados ao GridView

Quando o GridView está associado a uma fonte de dados, ele cria um `GridViewRow` para cada registro retornado pela fonte de dados. Portanto, podemos injetar as linhas de separador necessárias adicionando registros de separador à fonte de dados antes de associá-la ao GridView. A Figura 3 ilustra esse conceito.

![Uma técnica envolve a adição de linhas separadoras à fonte de dados](creating-a-customized-sorting-user-interface-vb/_static/image7.png)

**Figura 3**: uma técnica envolve a adição de linhas separadoras à fonte de dados

Uso os registros do separador de termos entre aspas porque não há nenhum registro de separador especial; em vez disso, devemos sinalizar que um registro específico na fonte de dados serve como um separador em vez de uma linha de dados normal. Para nossos exemplos, reassociamos uma instância de `ProductsDataTable` ao GridView, que é composto por `ProductRows`. Podemos sinalizar um registro como uma linha separadora definindo sua propriedade `CategoryID` como `-1` (já que esse valor não pode existir normalmente).

Para utilizar essa técnica, precisamos executar as seguintes etapas:

1. Recuperar programaticamente os dados para associar ao GridView (uma instância de `ProductsDataTable`)
2. Classificar os dados com base nas propriedades GridView s `SortExpression` e `SortDirection`
3. Iterar através da `ProductsRows` no `ProductsDataTable`, procurando onde as diferenças na coluna classificada se encontram
4. Em cada limite de grupo, insira um registro de separador `ProductsRow` instância na DataTable, um que tenha o s `CategoryID` definido como `-1` (ou que qualquer designação tenha sido decidida para marcar um registro como um registro de separador)
5. Depois de injetar as linhas do separador, vincule programaticamente os dados ao GridView

Além dessas cinco etapas, a d também precisa fornecer um manipulador de eventos para o evento GridView s `RowDataBound`. Aqui, nós d verificamos cada `DataRow` e determinamos se ele era uma linha separadora, uma cuja configuração `CategoryID` foi `-1`. Nesse caso, é provável que seja necessário ajustar sua formatação ou o texto exibido nas células.

Usar essa técnica para injetar os limites do grupo de classificação requer um pouco mais de trabalho do que descrito acima, pois você também precisa fornecer um manipulador de eventos para o evento de `Sorting` do GridView e controlar os valores de `SortExpression` e `SortDirection`.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Manipulando a coleção de controle GridView s depois que ele tiver sido vinculado

Em vez de enviar as mensagens dos dados antes de vinculá-la ao GridView, podemos adicionar as linhas do separador *após* os dados serem associados ao GridView. O processo de vinculação de dados cria a hierarquia de controle GridView s, que, na realidade, é simplesmente uma instância de `Table` composta de uma coleção de linhas, cada uma composta de uma coleção de células. Especificamente, a coleção de controle GridView s contém um objeto `Table` em sua raiz, um `GridViewRow` (que é derivado da classe `TableRow`) para cada registro no `DataSource` associado ao GridView e um objeto `TableCell` em cada instância de `GridViewRow` para cada campo de dados na `DataSource`.

Para adicionar linhas separadoras entre cada grupo de classificação, podemos manipular diretamente essa hierarquia de controle depois que ela tiver sido criada. Podemos ter certeza de que a hierarquia de controle GridView s foi criada pela última vez até a hora em que a página está sendo processada. Portanto, essa abordagem substitui a classe `Page` `Render` método, ponto em que a hierarquia de controle final de GridView s é atualizada para incluir as linhas de separador necessárias. A Figura 4 ilustra esse processo.

[![uma técnica alternativa manipula a hierarquia de controle GridView s](creating-a-customized-sorting-user-interface-vb/_static/image9.png)](creating-a-customized-sorting-user-interface-vb/_static/image8.png)

**Figura 4**: uma técnica alternativa manipula a hierarquia de controle GridView s ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image10.png))

Para este tutorial, usaremos essa última abordagem para personalizar a experiência do usuário de classificação.

> [!NOTE]
> O código que eu m apresentando neste tutorial é baseado no exemplo fornecido na entrada de blog [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) s, [jogando um pouco com o agrupamento de classificação de GridView](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).

## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Etapa 3: adicionar as linhas do separador à hierarquia de controle GridView s

Como queremos apenas adicionar as linhas separadoras à hierarquia de controle GridView s depois que sua hierarquia de controle tiver sido criada e criada pela última vez nessa página, queremos executar essa adição no final do ciclo de vida da página, mas antes do GridView c real a hierarquia ontrole foi renderizada em HTML. O ponto mais recente possível no qual podemos fazer isso é o `Page` classe s `Render` evento, que podemos substituir em nossa classe code-behind usando a seguinte assinatura de método:

[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample2.vb)]

Quando o método `Render` original da classe `Page` é invocado `base.Render(writer)` cada um dos controles na página será renderizado, gerando a marcação com base em sua hierarquia de controle. Portanto, é imperativo que chamamos de `base.Render(writer)`, para que a página seja renderizada, e que manipulemos a hierarquia de controle GridView s antes de chamar `base.Render(writer)`, de forma que as linhas separadoras tenham sido adicionadas à hierarquia de controle GridView s antes de serem renderizadas.

Para injetar os cabeçalhos de grupo de classificação, primeiro precisamos garantir que o usuário solicitou que os dados sejam classificados. Por padrão, o conteúdo de GridView s não é classificado e, portanto, não precisamos inserir nenhum cabeçalho de classificação de grupo.

> [!NOTE]
> Se você quiser que o GridView seja classificado por uma determinada coluna quando a página for carregada pela primeira vez, chame o método GridView s `Sort` na primeira página (mas não em postbacks subsequentes). Para fazer isso, adicione essa chamada no manipulador de eventos `Page_Load` dentro de um `if (!Page.IsPostBack)` condicional. Consulte as informações do tutorial de [dados de relatório de paginação e classificação](paging-and-sorting-report-data-vb.md) para saber mais sobre o método `Sort`.

Supondo que os dados tenham sido classificados, nossa próxima tarefa é determinar em qual coluna os dados foram classificados e, em seguida, verificar as linhas procurando diferenças nos valores de s da coluna. O código a seguir garante que os dados tenham sido classificados e localize a coluna pela qual os dados foram classificados:

[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample3.vb)]

Se o GridView ainda tiver que ser classificado, a Propriedade GridView s `SortExpression` não terá sido definida. Portanto, só queremos adicionar as linhas do separador se essa propriedade tiver algum valor. Em caso de, precisamos determinar o índice da coluna pela qual os dados foram classificados. Isso é feito por meio de um loop por meio da coleção GridView s `Columns`, pesquisando a coluna cuja propriedade `SortExpression` é igual à Propriedade GridView s `SortExpression`. Além do índice de coluna s, também pegamos a propriedade `HeaderText`, que é usada ao exibir as linhas do separador.

Com o índice da coluna pela qual os dados são classificados, a etapa final é enumerar as linhas do GridView. Para cada linha, precisamos determinar se o valor de s da coluna classificada é diferente do valor de s de coluna classificado por s de linha anterior. Nesse caso, precisamos injetar uma nova instância de `GridViewRow` na hierarquia de controle. Isso é feito com o seguinte código:

[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample4.vb)]

Esse código começa fazendo referência programática ao objeto de `Table` encontrado na raiz da hierarquia de controle GridView s e criando uma variável de cadeia de caracteres chamada `lastValue`. `lastValue` é usado para comparar o valor da coluna classificada de s da linha atual com o valor de s da linha anterior. Em seguida, a coleção GridView s `Rows` é enumerada e, para cada linha, o valor da coluna classificada é armazenado na variável `currentValue`.

> [!NOTE]
> Para determinar o valor da coluna de linhas s classificada específica, uso a propriedade da célula s `Text`. Isso funciona bem para os BoundFields, mas não funcionará conforme desejado para TemplateFields, CheckBoxFields e assim por diante. Veremos como considerar os campos GridView alternativos em breve.

As variáveis `currentValue` e `lastValue` são comparadas. Se forem diferentes, precisamos adicionar uma nova linha de separador à hierarquia de controle. Isso é feito determinando o índice do `GridViewRow` na coleção `Rows` objeto `Table`, criando novas instâncias de `GridViewRow` e `TableCell` e, em seguida, adicionando `TableCell` e `GridViewRow` à hierarquia de controle.

Observe que a linha separadora s solitário `TableCell` é formatada de forma que ela abranja toda a largura do GridView, seja formatada usando a classe CSS `SortHeaderRowStyle` e tenha sua propriedade `Text`, de modo que ela mostre o nome do grupo de classificação (como categoria) e o valor do grupo (como bebidas). Por fim, `lastValue` é atualizado para o valor de `currentValue`.

A classe CSS usada para formatar a linha de cabeçalho do grupo de classificação `SortHeaderRowStyle` precisa ser especificada no arquivo `Styles.css`. Sinta-se à vontade para usar quaisquer configurações de estilo atraentes para você; Usei o seguinte:

[!code-css[Main](creating-a-customized-sorting-user-interface-vb/samples/sample5.css)]

Com o código atual, a interface de classificação adiciona cabeçalhos de grupo de classificação ao classificar por qualquer BoundField (consulte a Figura 5, que mostra uma captura de tela ao classificar por fornecedor). No entanto, ao classificar por qualquer outro tipo de campo (como CheckBoxField ou TemplateField), os cabeçalhos do grupo de classificação não são encontrados em nenhum lugar (veja a Figura 6).

[![a interface de classificação inclui classificar cabeçalhos de grupo ao classificar por BoundFields](creating-a-customized-sorting-user-interface-vb/_static/image12.png)](creating-a-customized-sorting-user-interface-vb/_static/image11.png)

**Figura 5**: a interface de classificação inclui classificar cabeçalhos de grupo ao classificar por boundfields ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image13.png))

[![os cabeçalhos do grupo de classificação estão ausentes ao classificar um CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image15.png)](creating-a-customized-sorting-user-interface-vb/_static/image14.png)

**Figura 6**: os cabeçalhos do grupo de classificação estão ausentes ao classificar um CheckBoxField ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image16.png))

O motivo pelo qual os cabeçalhos de grupo de classificação estão ausentes ao classificar por um CheckBoxField é porque o código atualmente usa apenas a propriedade `TableCell` s `Text` para determinar o valor da coluna classificada para cada linha. Para CheckBoxFields, a propriedade `TableCell` s `Text` é uma cadeia de caracteres vazia; em vez disso, o valor está disponível por meio de um controle da Web de caixa de seleção que reside dentro da coleção `TableCell` s `Controls`.

Para lidar com tipos de campo diferentes de BoundFields, precisamos aumentar o código em que a variável `currentValue` é atribuída para verificar a existência de uma caixa de seleção na coleção `TableCell` s `Controls`. Em vez de usar `currentValue = gvr.Cells(sortColumnIndex).Text`, substitua esse código pelo seguinte:

[!code-vb[Main](creating-a-customized-sorting-user-interface-vb/samples/sample6.vb)]

Esse código examina a coluna classificada `TableCell` para a linha atual para determinar se há algum controle na coleção de `Controls`. Se houver, e o primeiro controle for uma caixa de seleção, a variável `currentValue` será definida como Sim ou não, dependendo da propriedade s da caixa de seleção `Checked`. Caso contrário, o valor será obtido da propriedade `TableCell` s `Text`. Essa lógica pode ser replicada para lidar com a classificação de qualquer TemplateFields que possa existir no GridView.

Com a adição de código acima, os cabeçalhos de grupo de classificação agora estão presentes ao classificar pelo CheckBoxField descontinuado (consulte a Figura 7).

[![os cabeçalhos de grupo de classificação agora estão presentes ao classificar um CheckBoxField](creating-a-customized-sorting-user-interface-vb/_static/image18.png)](creating-a-customized-sorting-user-interface-vb/_static/image17.png)

**Figura 7**: os cabeçalhos do grupo de classificação agora estão presentes ao classificar um CheckBoxField ([clique para exibir a imagem em tamanho normal](creating-a-customized-sorting-user-interface-vb/_static/image19.png))

> [!NOTE]
> Se você tiver produtos com `NULL` valores de banco de dados para os campos `CategoryID`, `SupplierID`ou `UnitPrice`, esses valores aparecerão como cadeias de caracteres vazias no GridView por padrão, o que significa que o texto da linha separadora dos produtos com `NULL` valores será lido como categoria: (ou seja, não há nenhum nome após a categoria: como com Categoria: bebidas). Se você quiser um valor exibido aqui, poderá definir a propriedade BoundFields [`NullDisplayText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) como o texto que deseja exibir ou pode adicionar uma instrução condicional no método render ao atribuir o `currentValue` à propriedade `Text` linha do separador.

## <a name="summary"></a>Resumo

O GridView não inclui muitas opções internas para personalizar a interface de classificação. No entanto, com um pouco de código de nível baixo, é possível ajustar a hierarquia de controle GridView s para criar uma interface mais personalizada. Neste tutorial, vimos como adicionar uma linha de separador de grupo de classificação para um GridView classificável, que identifica mais facilmente os grupos distintos e os limites de grupos. Para obter exemplos adicionais de interfaces de classificação personalizadas, confira [Scott Guthrie](https://weblogs.asp.net/scottgu/) s, [algumas dicas de classificação ASP.NET 2,0 GridView e truques](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) de entrada de blog.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](sorting-custom-paged-data-vb.md)
