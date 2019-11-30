---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
title: Interagindo com a página de conteúdo da página mestra (C#) | Microsoft Docs
author: rick-anderson
description: Examina como chamar métodos, definir propriedades, etc. da página de conteúdo do código na página mestra.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 3282df5e-516c-4972-8666-313828b90fb5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 2cf57665aa584285351d874267949d61db69c7fc
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74635673"
---
# <a name="interacting-with-the-content-page-from-the-master-page-c"></a>Interagir com a página de conteúdo através da página mestra (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_CS.zip) ou [baixar PDF](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_CS.pdf)

> Examina como chamar métodos, definir propriedades, etc. da página de conteúdo do código na página mestra.

## <a name="introduction"></a>Introdução

O tutorial anterior examinou como fazer com que a página de conteúdo interaja de forma programática com sua página mestra. Lembre-se de que atualizamos a página mestra para incluir um controle GridView que listou os cinco produtos adicionados mais recentemente. Em seguida, criamos uma página de conteúdo a partir da qual o usuário poderia adicionar um novo produto. Ao adicionar um novo produto, a página de conteúdo precisava instruir a página mestra a atualizar seu GridView para que ele inclua o produto recém-adicionado. Essa funcionalidade foi realizada adicionando um método público à página mestra que atualizava os dados associados ao GridView e, em seguida, invocando esse método na página de conteúdo.

A forma mais comum de conteúdo e a interação da página mestra é proveniente da página de conteúdo. No entanto, é possível que a página mestra Rouse a página de conteúdo atual em ação, e essa funcionalidade pode ser necessária se a página mestra contiver elementos da interface do usuário que permitem aos usuários modificar dados que também são exibidos na página de conteúdo. Considere uma página de conteúdo que exibe as informações de produtos em um controle GridView e uma página mestra que inclui um controle de botão que, quando clicado, dobra os preços de todos os produtos. Assim como o exemplo no tutorial anterior, o GridView precisa ser atualizado depois que o botão de preço duplo é clicado para que ele exiba os novos preços, mas nesse cenário é a página mestra que precisa Rouse a página de conteúdo em ação.

Este tutorial explora como fazer com que a página mestra invoque a funcionalidade definida na página conteúdo.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Atraindo a interação programática por meio de um evento e manipuladores de eventos

Invocar a funcionalidade da página de conteúdo de uma página mestra é mais desafiador do que o contrário. Como uma página de conteúdo tem uma única página mestra, ao instigar a interação programática na página de conteúdo, sabemos quais métodos e propriedades públicos estão em nossa disposição. Uma página mestra, no entanto, pode ter várias páginas de conteúdo diferentes, cada uma com seu próprio conjunto de propriedades e métodos. Como, então, podemos escrever código na página mestra para executar alguma ação em sua página de conteúdo quando não sabemos qual página de conteúdo será invocada até o tempo de execução?

Considere um controle da Web ASP.NET, como o controle de botão. Um controle de botão pode aparecer em qualquer número de páginas ASP.NET e precisa de um mecanismo pelo qual ele possa alertar a página em que foi clicado. Isso é feito usando *eventos*. Em particular, o controle de botão gera seu evento de `Click` quando ele é clicado; a página ASP.NET que contém o botão pode, opcionalmente, responder a essa notificação por meio de um *manipulador de eventos*.

Esse mesmo padrão pode ser usado para ter uma funcionalidade de gatilho de página mestra em suas páginas de conteúdo:

1. Adicione um evento à página mestra.
2. Gere o evento sempre que a página mestra precisar se comunicar com sua página de conteúdo. Por exemplo, se a página mestra precisar alertar sua página de conteúdo de que o usuário tiver duplicado os preços, seu evento será gerado imediatamente depois que os preços tiverem sido duplicados.
3. Crie um manipulador de eventos nessas páginas de conteúdo que precisam executar alguma ação.

Este restante deste tutorial implementa o exemplo descrito na introdução; a saber, uma página de conteúdo que lista os produtos no banco de dados e uma página mestra que inclui um controle de botão para dobrar os preços.

## <a name="step-1-displaying-products-in-a-content-page"></a>Etapa 1: exibindo produtos em uma página de conteúdo

Nossa primeira ordem de negócios é criar uma página de conteúdo que liste os produtos do banco de dados Northwind. (Adicionamos o banco de dados Northwind ao projeto no tutorial anterior, [*interagindo com a página mestra da página de conteúdo*](interacting-with-the-master-page-from-the-content-page-cs.md).) Comece adicionando uma nova página ASP.NET à pasta `~/Admin` chamada `Products.aspx`, conectando-a à página mestra `Site.master`. A Figura 1 mostra o Gerenciador de Soluções depois que essa página tiver sido adicionada ao site.

[![adicionar uma nova página ASP.NET à pasta admin](interacting-with-the-content-page-from-the-master-page-cs/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image1.png)

**Figura 01**: adicionar uma nova página do ASP.net à pasta `Admin` ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-cs/_static/image3.png))

Lembre-se de que, no tutorial [*especificando o título, as marcas meta e outros cabeçalhos HTML na página mestra*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) , criamos uma classe de página de base personalizada chamada `BasePage` que gera o título da página se ela não estiver definida explicitamente. Vá para a classe code-behind da página `Products.aspx` e faça com que ela derive de `BasePage` (em vez de `System.Web.UI.Page`).

Por fim, atualize o arquivo de `Web.sitemap` para incluir uma entrada para esta lição. Adicione a seguinte marcação abaixo do `<siteMapNode>` da lição conteúdo para a interação da página mestra:

[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample1.xml)]

A adição desse elemento de `<siteMapNode>` é refletida na lista de lições (consulte a Figura 5).

Retornar para `Products.aspx`. No controle de conteúdo para `MainContent`, adicione um controle GridView e nomeie-o `ProductsGrid`. Associe o GridView a um novo controle SqlDataSource chamado `ProductsDataSource`.

[![associar o GridView a um novo controle SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image4.png)

**Figura 02**: associar o GridView a um novo controle SqlDataSource ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-cs/_static/image6.png))

Configure o assistente para que ele use o banco de dados Northwind. Se você trabalhou no tutorial anterior, você já deve ter uma cadeia de conexão chamada `NorthwindConnectionString` em `Web.config`. Escolha essa cadeia de conexão na lista suspensa, conforme mostrado na Figura 3.

[![configurar o SqlDataSource para usar o banco de dados Northwind](interacting-with-the-content-page-from-the-master-page-cs/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image7.png)

**Figura 03**: configurar o SqlDataSource para usar o banco de dados Northwind ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-cs/_static/image9.png))

Em seguida, especifique a instrução de `SELECT` do controle da fonte de dados escolhendo a tabela produtos na lista suspensa e retornando as colunas `ProductName` e `UnitPrice` (consulte a Figura 4). Clique em avançar e em concluir para concluir o assistente para configurar fonte de dados.

[![retornar os campos NomeDoProduto e PreçoUnitário da tabela produtos](interacting-with-the-content-page-from-the-master-page-cs/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image10.png)

**Figura 04**: retornar os campos `ProductName` e `UnitPrice` da tabela `Products` ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-cs/_static/image12.png))

E isso é tudo! Depois de concluir o assistente, o Visual Studio adiciona dois BoundFields ao GridView para espelhar os dois campos retornados pelo controle SqlDataSource. A marcação de controles GridView e SqlDataSource é a seguinte. A Figura 5 mostra os resultados quando exibidos por meio de um navegador.

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample2.aspx)]

[![cada produto e seu preço estão listados no GridView](interacting-with-the-content-page-from-the-master-page-cs/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image13.png)

**Figura 05**: cada produto e seu preço são listados no GridView ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-cs/_static/image15.png))

> [!NOTE]
> Sinta-se à vontade para limpar a aparência do GridView. Algumas sugestões incluem a formatação do valor UnitPrice exibido como uma moeda e o uso de fontes e cores de plano de fundo para melhorar a aparência da grade. Para obter mais informações sobre como exibir e formatar dados no ASP.NET, consulte minha [série de tutoriais de trabalho com dados](../../data-access/index.md).

## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Etapa 2: adicionando um botão de preços duplo à página mestra

Nossa próxima tarefa é adicionar um controle de botão da Web à página mestra que, quando clicada, dobrará o preço de todos os produtos no banco de dados. Abra a página `Site.master` mestra e arraste um botão da caixa de ferramentas para o designer, colocando-o sob o controle `RecentProductsDataSource` SqlDataSource que adicionamos no tutorial anterior. Defina a propriedade `ID` do botão como `DoublePrice` e sua propriedade `Text` como "preços de produtos duplos".

Em seguida, adicione um controle SqlDataSource à página mestra, nomeando-o `DoublePricesDataSource`. Esse SqlDataSource será usado para executar a instrução `UPDATE` para dobrar todos os preços. Especificamente, precisamos definir suas propriedades `ConnectionString` e `UpdateCommand` para a cadeia de conexão apropriada e a instrução `UPDATE`. Em seguida, precisamos chamar esse método de `Update` do controle SqlDataSource quando o botão `DoublePrice` for clicado. Para definir as propriedades `ConnectionString` e `UpdateCommand`, selecione o controle SqlDataSource e, em seguida, vá para a janela Propriedades. A propriedade `ConnectionString` lista as cadeias de conexão já armazenadas em `Web.config` em uma lista suspensa; escolha a opção `NorthwindConnectionString`, conforme mostrado na Figura 6.

[![configurar o SqlDataSource para usar o NorthwindConnectionString](interacting-with-the-content-page-from-the-master-page-cs/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image16.png)

**Figura 06**: configurar o SqlDataSource para usar o `NorthwindConnectionString` ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-cs/_static/image18.png))

Para definir a propriedade `UpdateCommand`, localize a opção UpdateQuery no janela Propriedades. Essa propriedade, quando selecionada, exibe um botão com reticências; Clique nesse botão para exibir a caixa de diálogo Editor de comandos e parâmetros mostrada na Figura 7. Digite a instrução de `UPDATE` a seguir na caixa de texto do diálogo:

[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample3.sql)]

Essa instrução, quando executada, dobrará o valor de `UnitPrice` para cada registro na tabela `Products`.

[![definir a propriedade UpdateCommand de SqlDataSource](interacting-with-the-content-page-from-the-master-page-cs/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image19.png)

**Figura 07**: definir a propriedade de `UpdateCommand` do SqlDataSource ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-cs/_static/image21.png))

Depois de definir essas propriedades, a marcação declarativa do botão e dos controles SqlDataSource deve ser semelhante ao seguinte:

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample4.aspx)]

Tudo o que resta é chamar seu método de `Update` quando o botão de `DoublePrice` é clicado. Crie um manipulador de eventos `Click` para o botão `DoublePrice` e adicione o seguinte código:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample5.cs)]

Para testar essa funcionalidade, visite a página `~/Admin/Products.aspx` que criamos na etapa 1 e clique no botão "preços de produtos duplos". Clicar no botão causa um postback e executa o manipulador de eventos `Click` do botão de `DoublePrice`, dobrando os preços de todos os produtos. A página é renderizada novamente e a marcação é retornada e exibida novamente no navegador. No entanto, o GridView na página de conteúdo lista os mesmos preços que o botão "preços de produtos duplos" foi clicado. Isso ocorre porque os dados carregados inicialmente no GridView tinham seu estado armazenado no estado de exibição, portanto, ele não é recarregado em postbacks, a menos que seja instruído de outra forma. Se você visitar uma página diferente e retornar à página `~/Admin/Products.aspx`, verá os preços atualizados.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Etapa 3: gerando um evento quando os preços são duplicados

Como o GridView na página de `~/Admin/Products.aspx` não reflete imediatamente a dobra do preço, um usuário pode imaginar que ele não clicou no botão "preços de produtos duplos" ou que não funcionou. Eles podem tentar clicar no botão mais algumas vezes, dobrar os preços novamente e novamente. Para corrigir isso, precisamos que a grade na página conteúdo exiba os novos preços imediatamente depois que eles forem duplicados.

Conforme discutido anteriormente neste tutorial, precisamos gerar um evento na página mestra sempre que o usuário clicar no botão `DoublePrice`. Um evento é uma maneira para uma classe (um editor de eventos) notificar outro conjunto de outras classes (os assinantes de eventos) de que algo interessante ocorreu. Neste exemplo, a página mestra é o editor de eventos; as páginas de conteúdo que se preocupam quando o botão de `DoublePrice` é clicado são os assinantes.

Uma classe assina um evento criando um *manipulador de eventos*, que é um método que é executado em resposta ao evento que está sendo gerado. O Publicador define os eventos que ele gera definindo um *delegado de evento*. O delegado de evento especifica quais parâmetros de entrada o manipulador de eventos deve aceitar. No .NET Framework, os delegados de evento não retornam nenhum valor e aceitam dois parâmetros de entrada:

- Um `Object`, que identifica a origem do evento e
- Uma classe derivada de `System.EventArgs`

O segundo parâmetro passado para um manipulador de eventos pode incluir informações adicionais sobre o evento. Embora a classe base `EventArgs` não transmita nenhuma informação, a .NET Framework inclui um número de classes que estendem `EventArgs` e englobam propriedades adicionais. Por exemplo, uma instância de `CommandEventArgs` é passada para manipuladores de eventos que respondem ao evento `Command` e inclui duas propriedades informativas: `CommandArgument` e `CommandName`.

> [!NOTE]
> Para obter mais informações sobre como criar, gerar e manipular eventos, consulte [eventos e delegados](https://msdn.microsoft.com/library/17sde2xt.aspx) e [delegados de evento em inglês simples](http://www.codeproject.com/KB/cs/eventdelegates.aspx).

Para definir um evento, use a seguinte sintaxe:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample6.cs)]

Como só precisamos alertar a página de conteúdo quando o usuário clicou no botão `DoublePrice` e não precisa passar por outras informações adicionais, podemos usar o `EventHandler`delegado de evento, que define um manipulador de eventos que aceita como seu segundo parâmetro um objeto do tipo `System.EventArgs`. Para criar o evento na página mestra, adicione a seguinte linha de código à classe code-behind da página mestra:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample7.cs)]

O código acima adiciona um evento público à página mestra chamada `PricesDoubled`. Agora, precisamos gerar esse evento depois que os preços tiverem sido duplicados. Para gerar um evento, use a seguinte sintaxe:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample8.cs)]

Em que *Sender* e *EventArgs* são os valores que você deseja passar para o manipulador de eventos do Assinante.

Atualize o manipulador de eventos de `Click` `DoublePrice` com o seguinte código:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample9.cs)]

Como antes, o manipulador de eventos `Click` começa chamando o método de `Update` do controle SqlDataSource `DoublePricesDataSource` para dobrar os preços de todos os produtos. A seguir, há duas adições ao manipulador de eventos. Primeiro, os dados do `RecentProducts` GridView são atualizados. Este GridView foi adicionado à página mestra no tutorial anterior e exibe os cinco produtos adicionados mais recentemente. Precisamos atualizar essa grade para que ela mostre os preços dedobrados para esses cinco produtos. Depois disso, o evento `PricesDoubled` é gerado. Uma referência à página mestra em si (`this`) é enviada para o manipulador de eventos como a origem do evento e um objeto de `EventArgs` vazio é enviado como os argumentos do evento.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Etapa 4: manipulando o evento na página de conteúdo

Neste ponto, a página mestra gera seu evento de `PricesDoubled` sempre que o controle de botão de `DoublePrice` é clicado. No entanto, essa é apenas metade da batalha – ainda precisamos manipular o evento no Assinante. Isso envolve duas etapas: criar o manipulador de eventos e adicionar o código de fiação de evento para que, quando o evento for gerado, o manipulador de eventos seja executado.

Comece criando um manipulador de eventos chamado `Master_PricesDoubled`. Por causa de como definimos o evento `PricesDoubled` na página mestra, os dois parâmetros de entrada do manipulador de eventos devem ser dos tipos `Object` e `EventArgs`, respectivamente. No manipulador de eventos, chame o método `DataBind` do `ProductsGrid` GridView para reassociar os dados à grade.

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample10.cs)]

O código para o manipulador de eventos está concluído, mas ainda estamos vinculando o evento de `PricesDoubled` da página mestra a esse manipulador de eventos. O assinante conecta um evento a um manipulador de eventos por meio da seguinte sintaxe:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample11.cs)]

o *Publicador* é uma referência ao objeto que oferece o evento *EventName*, e *MethodName* é o nome do manipulador de eventos definido no Assinante que tem uma assinatura correspondente ao *eventDelegate*. Em outras palavras, se o delegado de evento for `EventHandler`, o *MethodName* deve ser o nome de um método no Assinante que não retorna um valor e aceita dois parâmetros de entrada dos tipos `Object` e `EventArgs`, respectivamente.

Esse código de fiação de evento deve ser executado na primeira página visita e postbacks subsequentes e deve ocorrer em um ponto no ciclo de vida da página que precede quando o evento pode ser gerado. Um bom momento para adicionar o código de fiação de evento está no estágio PreInit, que ocorre muito cedo no ciclo de vida da página.

Abra `~/Admin/Products.aspx` e crie um manipulador de eventos `Page_PreInit`:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample12.cs)]

Para concluir esse código de fiação, precisamos de uma referência programática para a página mestra na página de conteúdo. Conforme observado no tutorial anterior, há duas maneiras de fazer isso:

- Ao converter a propriedade de `Page.Master` de tipo flexível para o Type de página mestra apropriado, ou
- Adicionando uma diretiva `@MasterType` na página `.aspx` e, em seguida, usando a propriedade `Master` fortemente tipada.

Vamos usar a última abordagem. Adicione a seguinte diretiva `@MasterType` à parte superior da marcação declarativa da página:

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample13.aspx)]

Em seguida, adicione o seguinte código de fiação de evento no manipulador de eventos `Page_PreInit`:

[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample14.cs)]

Com esse código em vigor, o GridView na página de conteúdo é atualizado sempre que o botão de `DoublePrice` é clicado.

As figuras 8 e 9 ilustram esse comportamento. A Figura 8 mostra a página quando visitada pela primeira vez. Observe que os valores de preço no GridView do `RecentProducts` (na coluna à esquerda da página mestra) e o `ProductsGrid` GridView (na página conteúdo). A Figura 9 mostra a mesma tela imediatamente após o clique do botão de `DoublePrice`. Como você pode ver, os novos preços são refletidos instantaneamente em ambos os GridViews.

[![os valores de preço inicial](interacting-with-the-content-page-from-the-master-page-cs/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image22.png)

**Figura 08**: os valores de preço iniciais ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-cs/_static/image24.png))

[![os preços recém-duplos são exibidos nos GridViews](interacting-with-the-content-page-from-the-master-page-cs/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image25.png)

**Figura 09**: os preços recém-duplos são exibidos em GridViews ([clique para exibir a imagem em tamanho normal](interacting-with-the-content-page-from-the-master-page-cs/_static/image27.png))

## <a name="summary"></a>Resumo

O ideal é que uma página mestra e suas páginas de conteúdo sejam completamente separadas umas das outras e não exijam nenhum nível de interação. No entanto, se você tiver uma página mestra ou uma página de conteúdo que exiba dados que possam ser modificados na página mestra ou na página de conteúdo, talvez seja necessário que a página mestra Alerte a página de conteúdo (ou vice-versa) quando os dados forem modificados para que a exibição possa ser atualizada. No tutorial anterior, vimos como fazer com que uma página de conteúdo interaja de forma programática com sua página mestra; Neste tutorial, vimos como fazer uma página mestra iniciar a interação.

Embora a interação programática entre um conteúdo e uma página mestra possa ser originada no conteúdo ou na página mestra, o padrão de interação usado depende da origem. As diferenças são devido ao fato de uma página de conteúdo ter uma única página mestra, mas uma página mestra pode ter várias páginas de conteúdo diferentes. Em vez de ter uma página mestra diretamente interagir com uma página de conteúdo, uma abordagem melhor é fazer com que a página mestra gere um evento para sinalizar que alguma ação ocorreu. As páginas de conteúdo que se preocupam com a ação podem criar manipuladores de eventos.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Acessando e atualizando dados no ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Eventos e delegados](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Passando informações entre conteúdo e páginas mestras](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Trabalhando com dados em Tutoriais do ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP. net e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 3,5 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott pode ser contatado em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Banerjee. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](interacting-with-the-master-page-from-the-content-page-cs.md)
> [Próximo](master-pages-and-asp-net-ajax-cs.md)
