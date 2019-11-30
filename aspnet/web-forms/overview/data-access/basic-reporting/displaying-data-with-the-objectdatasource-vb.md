---
uid: web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
title: Exibindo dados com o ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: Este tutorial analisa o controle ObjectDataSource usando esse controle, você pode associar dados recuperados da BLL criada no tutorial anterior sem havi...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: d62c3a63-0940-4019-874e-4a4047df0c1c
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/displaying-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 754188352cbfb08e610027f5b7890a32bd88ae26
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74609556"
---
# <a name="displaying-data-with-the-objectdatasource-vb"></a>Exibir dados com o ObjectDataSource (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_4_VB.exe) ou [baixar PDF](displaying-data-with-the-objectdatasource-vb/_static/datatutorial04vb1.pdf)

> Este tutorial analisa o controle ObjectDataSource usando esse controle, você pode associar dados recuperados da BLL criada no tutorial anterior sem precisar escrever uma linha de código!

## <a name="introduction"></a>Introdução

Com a arquitetura do aplicativo e o layout da página do site concluídos, estamos prontos para começar a explorar como realizar uma variedade de tarefas comuns relacionadas a dados e relatórios. Nos tutoriais anteriores, vimos como associar de forma programática os dados da DAL e da BLL a um controle da Web de dados em uma página do ASP.NET. Essa sintaxe atribui a propriedade `DataSource` do controle da Web de dados aos dados a serem exibidos e, em seguida, chamar o método `DataBind()` do controle era o padrão usado em aplicativos ASP.NET 1. x e pode continuar a ser usado em seus aplicativos 2,0. No entanto, os novos controles da fonte de dados do ASP.NET 2.0 oferecem uma maneira declarativa de trabalhar com os dados. Usando esses controles, você pode associar dados recuperados da BLL criada no tutorial anterior sem precisar escrever uma linha de código!

O ASP.NET 2,0 é fornecido com cinco controles de fonte de dados internos [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx)e [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx) , embora você possa criar seus próprios controles de [fonte de dados personalizados](https://msdn.microsoft.com/library/default.asp?url=/library/dnvs05/html/DataSourceCon1.asp), se necessário. Como desenvolvemos uma arquitetura para nosso aplicativo de tutorial, usaremos o ObjectDataSource em nossas classes de BLL.

![O ASP.NET 2,0 inclui cinco controles de fonte de dados internos](displaying-data-with-the-objectdatasource-vb/_static/image1.png)

**Figura 1**: ASP.NET 2,0 inclui cinco controles de fonte de dados internos

O ObjectDataSource serve como um proxy para trabalhar com algum outro objeto. Para configurar o ObjectDataSource, especificamos esse objeto subjacente e como seus métodos são mapeados para os métodos `Select`, `Insert`, `Update`e `Delete` do ObjectDataSource. Depois que esse objeto subjacente tiver sido especificado e seus métodos forem mapeados para o ObjectDataSource, podemos associar o ObjectDataSource a um controle da Web de dados. O ASP.NET é fornecido com muitos controles da Web de dados, incluindo GridView, DetailsView, RadioButtonList e DropDownList, entre outros. Durante o ciclo de vida da página, o controle da Web de dados pode precisar acessar os dados aos quais está associado, invocando o método de `Select` de ObjectDataSource; Se o controle da Web de dados der suporte à inserção, atualização ou exclusão, podem ser feitas chamadas para os métodos `Insert`, `Update`ou `Delete` de ObjectDataSource. Essas chamadas são então roteadas pelo ObjectDataSource para os métodos do objeto subjacente apropriado, como ilustra o diagrama a seguir.

[![o ObjectDataSource serve como um proxy](displaying-data-with-the-objectdatasource-vb/_static/image3.png)](displaying-data-with-the-objectdatasource-vb/_static/image2.png)

**Figura 2**: o ObjectDataSource serve como um proxy ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image4.png))

Embora o ObjectDataSource possa ser usado para invocar métodos para inserção, atualização ou exclusão de dados, vamos apenas nos concentrar em retornar dados; os tutoriais futuros explorarão o uso dos controles ObjectDataSource e da Web de dados que modificam os dados.

## <a name="step-1-adding-and-configuring-the-objectdatasource-control"></a>Etapa 1: adicionando e Configurando o controle ObjectDataSource

Comece abrindo a página de `SimpleDisplay.aspx` na pasta `BasicReporting`, alterne para modo de exibição de Design e arraste um controle ObjectDataSource da caixa de ferramentas para a superfície de design da página. O ObjectDataSource é exibido como uma caixa cinza na superfície de design porque não produz nenhuma marcação; Ele simplesmente acessa os dados invocando um método de um objeto especificado. Os dados retornados por um ObjectDataSource podem ser exibidos por um controle da Web de dados, como GridView, DetailsView, FormView e assim por diante.

> [!NOTE]
> Como alternativa, você pode primeiro adicionar o controle da Web de dados à página e, em seguida, a partir de sua marca inteligente, escolha a opção &lt;nova fonte de dados&gt; na lista suspensa.

Para especificar o objeto subjacente do ObjectDataSource e como os métodos desse objeto são mapeados para o ObjectDataSource, clique no link configurar fonte de dados da marca inteligente ObjectDataSource.

[![clique no link configurar fonte de dados na marca inteligente](displaying-data-with-the-objectdatasource-vb/_static/image6.png)](displaying-data-with-the-objectdatasource-vb/_static/image5.png)

**Figura 3**: clique no link configurar fonte de dados na marca inteligente ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image7.png))

Isso abre o assistente para configurar fonte de dados. Primeiro, devemos especificar o objeto com o qual o ObjectDataSource deve trabalhar. Se a caixa de seleção "mostrar apenas componentes de dados" estiver marcada, a lista suspensa nessa tela listará somente os objetos que foram decorados com o atributo `DataObject`. Atualmente, nossa lista inclui os TableAdapters no DataSet tipado e as classes de BLL que criamos no tutorial anterior. Se você esqueceu de adicionar o atributo `DataObject` às classes da camada de lógica de negócios, você não os verá nesta lista. Nesse caso, desmarque a caixa de seleção "mostrar apenas componentes de dados" para exibir todos os objetos, que devem incluir as classes de BLL (juntamente com as outras classes no conjunto de dados digitado, as DataTables, as linhas de dados e assim por diante).

Nessa primeira tela, escolha a classe `ProductsBLL` na lista suspensa e clique em Avançar.

[![especificar o objeto a ser usado com o controle ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image9.png)](displaying-data-with-the-objectdatasource-vb/_static/image8.png)

**Figura 4**: especificar o objeto a ser usado com o controle ObjectDataSource ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image10.png))

A próxima tela do assistente solicita que você selecione o método que o ObjectDataSource deve invocar. O menu suspenso lista os métodos que retornam dados no objeto selecionado na tela anterior. Aqui, vemos `GetProductByProductID`, `GetProducts`, `GetProductsByCategoryID`e `GetProductsBySupplierID`. Selecione o método `GetProducts` na lista suspensa e clique em concluir (se você adicionou o `DataObjectMethodAttribute` aos métodos de `ProductBLL`, conforme mostrado no tutorial anterior, essa opção será selecionada por padrão).

[![escolher o método para retornar dados da guia selecionar](displaying-data-with-the-objectdatasource-vb/_static/image12.png)](displaying-data-with-the-objectdatasource-vb/_static/image11.png)

**Figura 5**: escolha o método para retornar dados da guia selecionar ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image13.png))

## <a name="configure-the-objectdatasource-manually"></a>Configurar o ObjectDataSource manualmente

O assistente para configurar fonte de dados do ObjectDataSource oferece uma maneira rápida de especificar o objeto que ele usa e associar quais métodos do objeto são invocados. No entanto, você pode configurar o ObjectDataSource por meio de suas propriedades, seja por meio da janela Propriedades ou diretamente na marcação declarativa. Basta definir a propriedade `TypeName` como o tipo do objeto subjacente a ser usado e o `SelectMethod` para o método a ser invocado durante a recuperação de dados.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample1.aspx)]

Mesmo que você prefira o assistente para configurar fonte de dados, pode haver ocasiões em que você precisa configurar o ObjectDataSource manualmente, pois o assistente lista apenas as classes criadas pelo desenvolvedor. Se você quiser associar o ObjectDataSource a uma classe no .NET Framework como a [classe Membership](https://msdn.microsoft.com/library/system.web.security.membership.aspx), para acessar as informações da conta de usuário ou a [classe de diretório](https://msdn.microsoft.com/library/system.io.directory.aspx) para trabalhar com as informações do sistema de arquivos, será necessário definir manualmente as propriedades do ObjectDataSource.

## <a name="step-2-adding-a-data-web-control-and-binding-it-to-the-objectdatasource"></a>Etapa 2: adicionar um controle da Web de dados e associá-lo ao ObjectDataSource

Depois que o ObjectDataSource tiver sido adicionado à página e configurado, estamos prontos para adicionar controles da Web de dados à página para exibir os dados retornados pelo método de `Select` do ObjectDataSource. Qualquer controle da Web de dados pode ser associado a um ObjectDataSource; Vejamos a exibição dos dados do ObjectDataSource em um GridView, DetailsView e FormView.

## <a name="binding-a-gridview-to-the-objectdatasource"></a>Associando um GridView ao ObjectDataSource

Adicione um controle GridView da caixa de ferramentas à superfície de design de `SimpleDisplay.aspx`. Na marca inteligente do GridView, escolha o controle ObjectDataSource que adicionamos na etapa 1. Isso criará automaticamente um BoundField no GridView para cada propriedade retornada pelos dados do método de `Select` do ObjectDataSource (ou seja, as propriedades definidas pela DataTable Products).

[![um GridView foi adicionado à página e associado a ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image15.png)](displaying-data-with-the-objectdatasource-vb/_static/image14.png)

**Figura 6**: um GridView foi adicionado à página e associado a ObjectDataSource ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image16.png))

Em seguida, você pode personalizar, reorganizar ou remover o BoundFields do GridView clicando na opção Editar colunas da marca inteligente.

[![gerenciar as BoundFields do GridView por meio da caixa de diálogo Editar colunas](displaying-data-with-the-objectdatasource-vb/_static/image18.png)](displaying-data-with-the-objectdatasource-vb/_static/image17.png)

**Figura 7**: gerenciar as boundfields do GridView por meio da caixa de diálogo Editar colunas ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image19.png))

Reserve um tempo para modificar os BoundFields do GridView, removendo os `ProductID`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitsInStock`, `UnitsOnOrder`e `ReorderLevel` BoundFields. Basta selecionar o BoundField na lista na parte inferior esquerda e clicar no botão excluir (o X vermelho) para removê-los. Em seguida, reorganize os BoundFields para que os `CategoryName` e `SupplierName` BoundFields precedem o `UnitPrice` BoundField selecionando esses BoundFields e clicando na seta para cima. Defina as propriedades `HeaderText` dos BoundFields restantes para `Products`, `Category`, `Supplier`e `Price`, respectivamente. Em seguida, tenha o `Price` BoundField formatado como uma moeda definindo a propriedade `HtmlEncode` de BoundField como false e sua propriedade `DataFormatString` como `{0:c}`. Por fim, alinhe horizontalmente o `Price` à direita e a caixa de seleção `Discontinued` no centro por meio da propriedade `ItemStyle`/`HorizontalAlign`.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample2.aspx)]

[![os BoundFields do GridView foram personalizados](displaying-data-with-the-objectdatasource-vb/_static/image21.png)](displaying-data-with-the-objectdatasource-vb/_static/image20.png)

**Figura 8**: os boundfields do GridView foram personalizados ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image22.png))

## <a name="using-themes-for-a-consistent-look"></a>Usando temas para uma aparência consistente

Esses tutoriais se esforçam para remover qualquer configuração de estilo de nível de controle, em vez disso, usando folhas de estilo em cascata definidas em um arquivo externo sempre que possível. O arquivo de `Styles.css` contém `DataWebControlStyle`, `HeaderStyle`, `RowStyle`e `AlternatingRowStyle` classes CSS que devem ser usadas para ditar a aparência dos controles da Web de dados usados nesses tutoriais. Para fazer isso, poderíamos definir a propriedade de `CssClass` do GridView como `DataWebControlStyle`, e `HeaderStyle`suas propriedades `AlternatingRowStyle`, `RowStyle`e `CssClass` Properties '.

Se definirmos essas `CssClass` Propriedades no controle da Web, precisaremos me lembrar de definir explicitamente esses valores de propriedade para cada controle da Web de dados adicionado aos nossos tutoriais. Uma abordagem mais gerenciável é definir as propriedades relacionadas à CSS padrão para os controles GridView, DetailsView e FormView usando um tema. Um tema é uma coleção de configurações de propriedade no nível de controle, imagens e classes CSS que podem ser aplicadas a páginas em um site para impor uma aparência comum.

Nosso tema não incluirá imagens ou arquivos CSS (deixaremos a folha de estilos `Styles.css` no estado em que se encontra, definido na pasta raiz do aplicativo Web), mas incluirá duas capas. Uma aparência é um arquivo que define as propriedades padrão para um controle da Web. Especificamente, teremos um arquivo de capa para os controles GridView e DetailsView, indicando as propriedades padrão relacionadas ao `CssClass`.

Comece adicionando um novo arquivo de capa ao seu projeto chamado `GridView.skin` clicando com o botão direito do mouse no nome do projeto na Gerenciador de Soluções e escolhendo Adicionar novo item.

[![adicionar um arquivo de capa chamado GridView. Skin](displaying-data-with-the-objectdatasource-vb/_static/image24.png)](displaying-data-with-the-objectdatasource-vb/_static/image23.png)

**Figura 9**: adicionar um arquivo de capa chamado `GridView.skin` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image25.png))

Os arquivos de capa precisam ser colocados em um tema, que estão localizados na pasta `App_Themes`. Como ainda não temos essa pasta, o Visual Studio oferecerá uma espécie para criar uma para nós ao adicionar nossa primeira capa. Clique em Sim para criar a pasta `App_Theme` e coloque o novo arquivo de `GridView.skin`.

[![deixar o Visual Studio criar a pasta App_Theme](displaying-data-with-the-objectdatasource-vb/_static/image27.png)](displaying-data-with-the-objectdatasource-vb/_static/image26.png)

**Figura 10**: Deixe que o Visual Studio crie a pasta `App_Theme` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image28.png))

Isso criará um novo tema na pasta `App_Themes` chamada GridView com o arquivo de capa `GridView.skin`.

![O tema GridView foi adicionado à pasta App_Theme](displaying-data-with-the-objectdatasource-vb/_static/image29.png)

**Figura 11**: o tema GridView foi adicionado à pasta `App_Theme`

Renomeie o tema GridView para datawebcontrols (clique com o botão direito do mouse na pasta GridView na pasta `App_Theme` e escolha Renomear). Em seguida, digite a seguinte marcação no arquivo de `GridView.skin`:

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample3.aspx)]

Isso define as propriedades padrão para as propriedades relacionadas a `CssClass`para qualquer GridView em qualquer página que usa o tema datawebcontrols. Vamos adicionar outra capa para o DetailsView, um controle da Web de dados que usaremos em breve. Adicione uma nova aparência ao tema datawebcontrols chamado `DetailsView.skin` e adicione a seguinte marcação:

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample4.aspx)]

Com nosso tema definido, a última etapa é aplicar o tema à nossa página ASP.NET. Um tema pode ser aplicado em uma base página por página ou em todas as páginas de um site. Vamos usar esse tema para todas as páginas no site. Para fazer isso, adicione a seguinte marcação à seção `<system.web>` do `Web.config`:

[!code-xml[Main](displaying-data-with-the-objectdatasource-vb/samples/sample5.xml)]

E isso é tudo! A configuração `styleSheetTheme` indica que as propriedades especificadas no tema *não* devem substituir as propriedades especificadas no nível de controle. Para especificar que as configurações de tema devem ser configurações de controle de trunfo, use o atributo `theme` no lugar de `styleSheetTheme`; Infelizmente, as configurações de tema não aparecem no modo de exibição de Design do Visual Studio. Consulte [visão geral de temas e capas do ASP.net](https://msdn.microsoft.com/library/ykzx33wh.aspx) e [estilos do lado do servidor usando temas](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx) para obter mais informações sobre temas e capas; consulte [como: aplicar temas do ASP.net](https://msdn.microsoft.com/library/0yy5hxdk%28VS.80%29.aspx) para saber mais sobre como configurar uma página para usar um tema.

[![o GridView exibe o nome, a categoria, o fornecedor, o preço e as informações descontinuadas do produto](displaying-data-with-the-objectdatasource-vb/_static/image31.png)](displaying-data-with-the-objectdatasource-vb/_static/image30.png)

**Figura 12**: o GridView exibe o nome do produto, a categoria, o fornecedor, o preço e as informações descontinuadas ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image32.png))

## <a name="displaying-one-record-at-a-time-in-the-detailsview"></a>Exibindo um registro por vez no DetailsView

O GridView exibe uma linha para cada registro retornado pelo controle da fonte de dados ao qual está associado. No entanto, há ocasiões em que talvez queiramos exibir um único registro ou apenas um registro por vez. O [controle DetailsView](https://msdn.microsoft.com/library/s3w1w7t4.aspx) oferece essa funcionalidade, renderizando como um `<table>` HTML com duas colunas e uma linha para cada coluna ou propriedade associada ao controle. Você pode considerar o DetailsView como um GridView com um único registro girado em 90 graus.

Comece adicionando um controle DetailsView *acima* do GridView no `SimpleDisplay.aspx`. Em seguida, associe-o ao mesmo controle ObjectDataSource que o GridView. Assim como com o GridView, um BoundField será adicionado a DetailsView para cada propriedade no objeto retornado pelo método de `Select` do ObjectDataSource. A única diferença é que os BoundFields de DetailsView são dispostos horizontalmente em vez de verticalmente.

[![adicionar um DetailsView à página e associá-lo ao ObjectDataSource](displaying-data-with-the-objectdatasource-vb/_static/image34.png)](displaying-data-with-the-objectdatasource-vb/_static/image33.png)

**Figura 13**: adicionar um DetailsView à página e associá-lo ao ObjectDataSource ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image35.png))

Como o GridView, os BoundFields de DetailsView podem ser ajustados para fornecer uma exibição mais personalizada dos dados retornados pelo ObjectDataSource. A Figura 14 mostra o DetailsView após seus BoundFields e `CssClass` propriedades foram configuradas para fazer sua aparência semelhante ao exemplo de GridView.

[![o DetailsView mostra um único registro](displaying-data-with-the-objectdatasource-vb/_static/image37.png)](displaying-data-with-the-objectdatasource-vb/_static/image36.png)

**Figura 14**: o DetailsView mostra um único registro ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image38.png))

Observe que o DetailsView exibe apenas o primeiro registro retornado por sua fonte de dados. Para permitir que o usuário percorra todos os registros, um de cada vez, devemos habilitar a paginação para o DetailsView. Para fazer isso, retorne ao Visual Studio e marque a caixa de seleção habilitar paginação na marca inteligente de DetailsView.

[![habilitar a paginação no controle DetailsView](displaying-data-with-the-objectdatasource-vb/_static/image40.png)](displaying-data-with-the-objectdatasource-vb/_static/image39.png)

**Figura 15**: habilitar a paginação no controle DetailsView ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image41.png))

[![com a paginação habilitada, o DetailsView permite que o usuário exiba qualquer um dos produtos](displaying-data-with-the-objectdatasource-vb/_static/image43.png)](displaying-data-with-the-objectdatasource-vb/_static/image42.png)

**Figura 16**: com a paginação habilitada, o DetailsView permite que o usuário exiba qualquer um dos produtos ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image44.png))

Falaremos mais sobre paginação em Tutoriais futuros.

## <a name="a-more-flexible-layout-for-showing-one-record-at-a-time"></a>Um layout mais flexível para mostrar um registro por vez

O DetailsView é bastante rígido em como ele exibe cada registro retornado de ObjectDataSource. Talvez queiramos uma exibição mais flexível dos dados. Por exemplo, em vez de mostrar o nome, a categoria, o fornecedor, o preço e as informações descontinuadas do produto, cada um em uma linha separada, talvez queiramos mostrar o nome do produto e o preço em um título de `<h4>`, com as informações de categoria e fornecedor aparecendo abaixo do nome e do preço em um tamanho de fonte menor. E não podemos nos preocupar em mostrar os nomes de propriedade (produto, categoria e assim por diante) ao lado dos valores.

O [controle FormView](https://msdn.microsoft.com/library/fyf1dk77.aspx) fornece esse nível de personalização. Em vez de usar campos (como GridView e DetailsView), o FormView usa modelos, que permitem uma combinação de controles da Web, HTML estático e sintaxe de [DataBinding](http://www.15seconds.com/issue/040630.htm). Se você estiver familiarizado com o controle Repeater do ASP.NET 1. x, poderá considerar o FormView como o repetidor para mostrar um único registro.

Adicione um controle FormView à superfície de design da página `SimpleDisplay.aspx`. Inicialmente, o FormView é exibido como um bloco cinza, informando que precisamos fornecer, no mínimo, o `ItemTemplate`do controle.

[![FormView deve incluir um ItemTemplate](displaying-data-with-the-objectdatasource-vb/_static/image46.png)](displaying-data-with-the-objectdatasource-vb/_static/image45.png)

**Figura 17**: o FormView deve incluir um `ItemTemplate` ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image47.png))

Você pode associar o FormView diretamente a um controle da fonte de dados por meio da marca inteligente do FormView, que criará um `ItemTemplate` padrão automaticamente (juntamente com um `EditItemTemplate` e `InsertItemTemplate`, se as propriedades `InsertMethod` e `UpdateMethod` do controle ObjectDataSource estiverem definidas). No entanto, para este exemplo, vamos associar os dados ao FormView e especificar seu `ItemTemplate` manualmente. Comece definindo a propriedade `DataSourceID` do FormView como a `ID` do controle ObjectDataSource `ObjectDataSource1`. Em seguida, crie o `ItemTemplate` para que ele exiba o nome e o preço do produto em um elemento de `<h4>` e os nomes de categoria e de transportador abaixo dele em um tamanho de fonte menor.

[!code-aspx[Main](displaying-data-with-the-objectdatasource-vb/samples/sample6.aspx)]

[![o primeiro produto (Chai) é exibido em um formato personalizado](displaying-data-with-the-objectdatasource-vb/_static/image49.png)](displaying-data-with-the-objectdatasource-vb/_static/image48.png)

**Figura 18**: o primeiro produto (Chai) é exibido em um formato personalizado ([clique para exibir a imagem em tamanho normal](displaying-data-with-the-objectdatasource-vb/_static/image50.png))

O `<%# Eval(propertyName) %>` é a sintaxe de DataBinding. O método `Eval` retorna o valor da propriedade especificada para o objeto atual que está sendo associado ao controle FormView. Confira o artigo de Alex Homer [e a sintaxe de ligação de dados estendida no ASP.NET 2,0](http://www.15seconds.com/issue/040630.htm) para obter mais informações sobre as vantagens e desvantagens da Associação de vínculo.

Como o DetailsView, o FormView mostra apenas o primeiro registro retornado de ObjectDataSource. Você pode habilitar a paginação no FormView para permitir que os visitantes percorram os produtos um de cada vez.

## <a name="summary"></a>Resumo

O acesso e a exibição de dados de uma camada de lógica de negócios podem ser realizados sem escrever uma linha de código graças ao controle ObjectDataSource do ASP.NET 2.0. O ObjectDataSource invoca um método especificado de uma classe e retorna os resultados. Esses resultados podem ser exibidos em um controle da Web de dados associado ao ObjectDataSource. Neste tutorial, examinamos a vinculação dos controles GridView, DetailsView e FormView ao ObjectDataSource.

Até agora, vimos como usar o ObjectDataSource para invocar um método sem parâmetros, mas e se quisermos invocar um método que espera parâmetros de entrada, como o `GetProductsByCategoryID(categoryID)`da classe `ProductBLL`? Para chamar um método que espera um ou mais parâmetros, devemos configurar o ObjectDataSource para especificar os valores para esses parâmetros. Veremos como fazer isso em nosso [próximo tutorial](declarative-parameters-vb.md).

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Criar seus próprios controles de fonte de dados](https://msdn.microsoft.com/library/ms364049.aspx)
- [Exemplos de GridView para ASP.NET 2,0](https://msdn.microsoft.com/library/aa479339.aspx)
- [Sintaxe de vinculação de dados simplificada e estendida no ASP.NET 2,0](http://www.15seconds.com/issue/040630.htm)
- [Temas no ASP.NET 2,0](http://www.odetocode.com/Articles/423.aspx)
- [Estilos do lado do servidor usando temas](https://quickstarts.asp.net/quickstartv20/aspnet/doc/themes/stylesheettheme.aspx)
- [Como aplicar temas de ASP.NET programaticamente](https://msdn.microsoft.com/library/tx35bd89.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Hilton Giesenow. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](programmatically-setting-the-objectdatasource-s-parameter-values-cs.md)
> [Próximo](declarative-parameters-vb.md)
