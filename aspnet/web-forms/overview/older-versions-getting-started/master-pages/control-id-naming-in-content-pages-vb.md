---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
title: Nomenclatura de ID de controle nas páginas de conteúdo (VB) | Microsoft Docs
author: rick-anderson
description: Ilustra como os controles ContentPlaceHolder servem como um contêiner de nomenclatura e, portanto, tornam o trabalho programático com um controle difícil (via FindControl)...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: dbb024a6-f043-4fc5-ad66-56556711875b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 3cb8dec47040bc65f1a024325c91590729ffbdb7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639275"
---
# <a name="control-id-naming-in-content-pages-vb"></a>Controlar nomenclatura de ID em páginas de conteúdo (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_VB.zip) ou [baixar PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_VB.pdf)

> Ilustra como os controles ContentPlaceHolder servem como um contêiner de nomenclatura e, portanto, tornam o trabalho programático com um controle difícil (por meio de FindControl). O analisa esse problema e as soluções alternativas. Também discute como acessar programaticamente o valor ClientID resultante.

## <a name="introduction"></a>Introdução

Todos os controles de servidor ASP.NET incluem uma propriedade `ID` que identifica exclusivamente o controle e é o meio pelo qual o controle é acessado por meio de programação na classe code-behind. Da mesma forma, os elementos em um documento HTML podem incluir um atributo `id` que identifica exclusivamente o elemento; esses valores de `id` geralmente são usados no script do lado do cliente para fazer referência programaticamente a um determinado elemento HTML. Considerando isso, você pode supor que quando um controle de servidor ASP.NET é processado em HTML, seu valor `ID` é usado como o valor de `id` do elemento HTML renderizado. Isso não é necessariamente o caso porque, em determinadas circunstâncias, um único controle com um único valor de `ID` pode aparecer várias vezes na marcação renderizada. Considere um controle GridView que inclui um TemplateField com um controle rótulo da Web com um valor `ID` de `ProductName`. Quando o GridView está associado à sua fonte de dados em tempo de execução, esse rótulo é repetido uma vez para cada linha GridView. Cada rótulo renderizado precisa de um valor de `id` exclusivo.

Para lidar com esses cenários, o ASP.NET permite que determinados controles sejam denotados como contêineres de nomenclatura. Um contêiner de nomenclatura serve como um novo namespace `ID`. Todos os controles de servidor que aparecem dentro do contêiner de nomeação têm seus valores processados `id` prefixados com a `ID` do controle de contêiner de nomenclatura. Por exemplo, as classes `GridView` e `GridViewRow` são contêineres de nomes. Consequentemente, um controle rótulo definido em um modelo GridView com `ID` `ProductName` recebe um valor de `id` renderizado de `GridViewID_GridViewRowID_ProductName`. Como *GridViewRowID* é exclusivo para cada linha GridView, os valores de `id` resultantes são exclusivos.

> [!NOTE]
> A [interface`INamingContainer`](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) é usada para indicar que um determinado controle de servidor ASP.NET deve funcionar como um contêiner de nomenclatura. A interface `INamingContainer` não dá como preenchimento nenhum método que o controle de servidor deve implementar; em vez disso, ele é usado como um marcador. Ao gerar a marcação renderizada, se um controle implementar essa interface, o mecanismo ASP.NET prefixará automaticamente seu valor de `ID` para seus descendentes `id` valores de atributo. Esse processo é discutido mais detalhadamente na etapa 2.

Os contêineres de nomenclatura não apenas alteram o valor do atributo `id` processado, mas também afetam como o controle pode ser referenciado por meio de programação da classe code-behind da página ASP.NET. O método `FindControl("controlID")` geralmente é usado para fazer referência programaticamente a um controle da Web. No entanto, `FindControl` não faz a penetração por meio de contêineres de nomenclatura. Consequentemente, você não pode usar diretamente o método `Page.FindControl` para referenciar controles dentro de um GridView ou outro contêiner de nomenclatura.

Como você pode ter imaginou, as páginas mestras e os ContentPlaceHolders são implementados como contêineres de nomenclatura. Neste tutorial, examinaremos como as páginas mestras afetam os valores de `id` de elemento HTML e maneiras de referenciar controles da Web de forma programática em uma página de conteúdo usando `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Etapa 1: adicionando uma nova página do ASP.NET

Para demonstrar os conceitos discutidos neste tutorial, vamos adicionar uma nova página do ASP.NET ao nosso site. Crie uma nova página de conteúdo denominada `IDIssues.aspx` na pasta raiz, associando-a à página mestra `Site.master`.

![Adicione a página de conteúdo IDIssues. aspx à pasta raiz](control-id-naming-in-content-pages-vb/_static/image1.png)

**Figura 01**: adicionar a página de conteúdo `IDIssues.aspx` à pasta raiz

O Visual Studio cria automaticamente um controle de conteúdo para cada um dos quatro ContentPlaceHolders da página mestra. Conforme observado no tutorial [*vários ContentPlaceHolders e conteúdo padrão*](multiple-contentplaceholders-and-default-content-vb.md) , se um controle de conteúdo não estiver presente, o conteúdo padrão ContentPlaceHolder da página mestra será emitido. Como o `QuickLoginUI` e `LeftColumnContent` ContentPlaceHolders contêm uma marcação padrão adequada para essa página, vá em frente e remova seus controles de conteúdo correspondentes de `IDIssues.aspx`. Neste ponto, a marcação declarativa da página de conteúdo deve ser parecida com a seguinte:

[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample1.aspx)]

No tutorial [*especificando o título, as marcas meta e outros cabeçalhos HTML na página mestra*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) , criamos uma classe de página de base personalizada (`BasePage`) que configura automaticamente o título da página se ela não estiver definida explicitamente. Para a página de `IDIssues.aspx` empregar essa funcionalidade, a classe code-behind da página deve derivar da classe `BasePage` (em vez de `System.Web.UI.Page`). Modifique a definição da classe code-behind para que fique semelhante ao seguinte:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample2.vb)]

Por fim, atualize o arquivo de `Web.sitemap` para incluir uma entrada para essa nova lição. Adicione um elemento `<siteMapNode>` e defina seus atributos `title` e `url` como "problemas de nomenclatura de ID de controle" e `~/IDIssues.aspx`, respectivamente. Depois de fazer essa adição, a marcação do `Web.sitemap` arquivo deve ser semelhante ao seguinte:

[!code-xml[Main](control-id-naming-in-content-pages-vb/samples/sample3.xml)]

Como a Figura 2 ilustra, a nova entrada do mapa do site no `Web.sitemap` é refletida imediatamente na seção lições na coluna à esquerda.

![A seção de lições agora inclui um link para &quot;problemas de nomenclatura da ID de controle&quot;](control-id-naming-in-content-pages-vb/_static/image2.png)

**Figura 02**: a seção de lições agora inclui um link para "problemas de nomenclatura de ID de controle"

## <a name="step-2-examining-the-renderedidchanges"></a>Etapa 2: examinando as alterações`ID`processadas

Para entender melhor as modificações que o mecanismo ASP.NET faz para os valores de `id` processados dos controles de servidor, vamos adicionar alguns controles da Web à página `IDIssues.aspx` e, em seguida, exibir a marcação renderizada enviada ao navegador. Especificamente, digite o texto "Insira sua idade:" seguido por um controle da Web TextBox. Mais adiante na página, adicione um controle Web de botão e um controle de rótulo da Web. Defina as propriedades `ID` e `Columns` da caixa de texto como `Age` e 3, respectivamente. Defina as propriedades `Text` e `ID` do botão como "enviar" e `SubmitButton`. Desmarque a propriedade `Text` do rótulo e defina sua `ID` como `Results`.

Neste ponto, a marcação declarativa do controle de conteúdo deve ser semelhante ao seguinte:

[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample4.aspx)]

A Figura 3 mostra a página quando exibida por meio do designer do Visual Studio.

[![a página inclui três controles da Web: uma caixa de texto, um botão e um rótulo](control-id-naming-in-content-pages-vb/_static/image4.png)](control-id-naming-in-content-pages-vb/_static/image3.png)

**Figura 03**: a página inclui três controles da Web: uma caixa de texto, um botão e um rótulo ([clique para exibir a imagem em tamanho normal](control-id-naming-in-content-pages-vb/_static/image5.png))

Visite a página por meio de um navegador e, em seguida, exiba a fonte HTML. Como mostra a marcação a seguir, os `id` valores dos elementos HTML dos controles TextBox, Button e Label da Web são uma combinação dos valores de `ID` dos controles da Web e dos valores de `ID` dos contêineres de nomenclatura na página.

[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample5.html)]

Conforme observado anteriormente neste tutorial, a página mestra e seus ContentPlaceHolders servem como contêineres de nomenclatura. Consequentemente, ambos contribuem com os valores de `ID` renderizados de seus controles aninhados. Use o atributo `id` da caixa de texto, por exemplo: `ctl00_MainContent_Age`. Lembre-se de que o valor de `ID` do controle TextBox foi `Age`. Isso é prefixado com o valor de `ID` do controle ContentPlaceHolder `MainContent`. Além disso, esse valor é prefixado com o valor de `ID` da página mestra `ctl00`. O efeito de rede é um valor de atributo `id` que consiste nos valores de `ID` da página mestra, no controle ContentPlaceHolder e na própria caixa de texto.

A Figura 4 ilustra esse comportamento. Para determinar o `id` renderizado da caixa de texto `Age`, comece com o valor `ID` do controle TextBox `Age`. Em seguida, trabalhe do seu jeito com a hierarquia de controle. Em cada contêiner de nomenclatura (esses nós com uma cor pêssego), Prefixe o `id` renderizado atual com o `id`do contêiner de nomenclatura.

![Os atributos de ID renderizados se baseiam nos valores de ID dos contêineres de nomenclatura](control-id-naming-in-content-pages-vb/_static/image6.png)

**Figura 04**: os atributos de `id` renderizados são baseados nos valores de `ID` dos contêineres de nomenclatura

> [!NOTE]
> Como discutimos, a parte `ctl00` do atributo `id` renderizado constitui o valor `ID` da página mestra, mas você pode estar se perguntando como esse valor de `ID` surgiu. Não especificamos em nenhum lugar em nossa página mestra ou de conteúdo. A maioria dos controles de servidor em uma página ASP.NET são adicionados explicitamente por meio da marcação declarativa da página. O controle `MainContent` ContentPlaceHolder foi especificado explicitamente na marcação de `Site.master`; a caixa de texto `Age` foi definida `IDIssues.aspx`marcação. Podemos especificar os valores de `ID` para esses tipos de controles por meio da janela Propriedades ou da sintaxe declarativa. Outros controles, como a própria página mestra, não são definidos na marcação declarativa. Consequentemente, seus valores de `ID` devem ser gerados automaticamente para nós. O mecanismo ASP.NET define os valores de `ID` em tempo de execução para os controles cujas IDs não foram definidas explicitamente. Ele usa o padrão de nomenclatura `ctlXX`, em que *XX* é um valor inteiro que aumenta sequencialmente.

Como a própria página mestra serve como um contêiner de nomenclatura, os controles da Web definidos na página mestra também têm alterado `id` valores de atributo. Por exemplo, o rótulo de `DisplayDate` que adicionamos à página mestra no tutorial [*criando um layout em todo o site com páginas mestras*](creating-a-site-wide-layout-using-master-pages-vb.md) tem a seguinte marcação renderizada:

[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample6.html)]

Observe que o atributo `id` inclui o valor de `ID` da página mestra (`ctl00`) e o valor `ID` do controle da Web do rótulo (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Etapa 3: referenciando controles da Web por meio de programação via`FindControl`

Cada controle de servidor ASP.NET inclui um método `FindControl("controlID")` que pesquisa os descendentes do controle em busca de um controle chamado *ControlID*. Se esse controle for encontrado, ele será retornado; Se nenhum controle correspondente for encontrado, `FindControl` retornará `Nothing`.

`FindControl` é útil em cenários em que você precisa acessar um controle, mas você não tem uma referência direta a ele. Ao trabalhar com controles da Web de dados como o GridView, por exemplo, os controles dentro dos campos do GridView são definidos uma vez na sintaxe declarativa, mas no tempo de execução uma instância do controle é criada para cada linha GridView. Consequentemente, os controles gerados no tempo de execução existem, mas não temos uma referência direta disponível na classe code-behind. Como resultado, precisamos usar `FindControl` para trabalhar programaticamente com um controle específico dentro dos campos do GridView. (Para obter mais informações sobre como usar `FindControl` para acessar os controles em modelos de um controle da Web de dados, consulte [formatação personalizada com base nos dados](../../data-access/custom-formatting/custom-formatting-based-upon-data-vb.md).) Esse mesmo cenário ocorre ao adicionar dinamicamente controles da Web a um formulário da Web, um tópico abordado na [criação de dados dinâmicos interfaces de usuário de entrada](https://msdn.microsoft.com/library/aa479330.aspx).

Para ilustrar o uso do método `FindControl` para pesquisar controles dentro de uma página de conteúdo, crie um manipulador de eventos para o evento de `Click` do `SubmitButton`. No manipulador de eventos, adicione o código a seguir, que referencia programaticamente a caixa de texto `Age` e `Results` rótulo usando o método `FindControl` e, em seguida, exibe uma mensagem no `Results` com base na entrada do usuário.

> [!NOTE]
> É claro que não precisamos usar `FindControl` para fazer referência aos controles Label e TextBox para este exemplo. Poderíamos referenciá-los diretamente por meio de seus `ID` valores de propriedade. Uso `FindControl` aqui para ilustrar o que acontece ao usar o `FindControl` de uma página de conteúdo.

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample7.vb)]

Embora a sintaxe usada para chamar o método `FindControl` difere ligeiramente nas duas primeiras linhas de `SubmitButton_Click`, elas são semanticamente equivalentes. Lembre-se de que todos os controles do servidor ASP.NET incluem um método `FindControl`. Isso inclui a classe `Page`, da qual todas as classes code-behind ASP.NET devem derivar. Portanto, chamar `FindControl("controlID")` é equivalente a chamar `Page.FindControl("controlID")`, supondo que você não tenha substituído o método `FindControl` em sua classe code-behind ou em uma classe base personalizada.

Depois de inserir esse código, visite a página `IDIssues.aspx` por meio de um navegador, insira sua idade e clique no botão "enviar". Ao clicar no botão "enviar", um `NullReferenceException` é gerado (consulte a Figura 5).

[![uma NullReferenceException é gerada](control-id-naming-in-content-pages-vb/_static/image8.png)](control-id-naming-in-content-pages-vb/_static/image7.png)

**Figura 05**: um `NullReferenceException` é gerado ([clique para exibir a imagem em tamanho normal](control-id-naming-in-content-pages-vb/_static/image9.png))

Se você definir um ponto de interrupção no manipulador de eventos `SubmitButton_Click`, verá que ambas as chamadas para `FindControl` retornam `Nothing`. O `NullReferenceException` é gerado quando tentamos acessar a propriedade `Text` da `Age` caixa de texto.

O problema é que `Control.FindControl` somente pesquisa os descendentes do *controle*que estão no mesmo contêiner de nomenclatura. Como a página mestra constitui um novo contêiner de nomenclatura, uma chamada para `Page.FindControl("controlID")` nunca percarne o objeto da página mestra `ctl00`. (Consulte a Figura 4 para exibir a hierarquia de controle, que mostra o objeto `Page` como o pai do objeto da página mestra `ctl00`.) Portanto, o rótulo de `Results` e a caixa de texto `Age` não são encontrados e `ResultsLabel` e `AgeTextBox` são valores atribuídos de `Nothing`.

Há duas soluções alternativas para esse desafio: podemos fazer drill down, um contêiner de nomeação por vez, para o controle apropriado; ou podemos criar nosso próprio método de `FindControl` que percarne os contêineres de nomenclatura. Vamos examinar cada uma dessas opções.

### <a name="drilling-into-the-appropriate-naming-container"></a>Detalhando o contêiner de nomenclatura apropriado

Para usar `FindControl` para referenciar o rótulo de `Results` ou a caixa de texto `Age`, precisamos chamar `FindControl` de um controle ancestral no mesmo contêiner de nomenclatura. Como mostra a Figura 4, o controle `MainContent` ContentPlaceHolder é o único ancestral de `Results` ou `Age` que está dentro do mesmo contêiner de nomenclatura. Em outras palavras, chamar o método `FindControl` do controle `MainContent`, conforme mostrado no trecho de código abaixo, retorna corretamente uma referência para os controles `Results` ou `Age`.

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample8.vb)]

No entanto, não podemos trabalhar com o `MainContent` ContentPlaceHolder da classe code-behind da página de conteúdo usando a sintaxe acima porque o ContentPlaceHolder é definido na página mestra. Em vez disso, precisamos usar `FindControl` para obter uma referência a `MainContent`. Substitua o código no manipulador de eventos `SubmitButton_Click` com as seguintes modificações:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample9.vb)]

Se você visitar a página por meio de um navegador, inserir sua idade e clicar no botão "enviar", um `NullReferenceException` será gerado. Se você definir um ponto de interrupção no manipulador de eventos `SubmitButton_Click`, verá que essa exceção ocorre ao tentar chamar o método de `FindControl` do objeto `MainContent`. O objeto `MainContent` é igual a `Nothing` porque o método `FindControl` não pode localizar um objeto chamado "MainContent". O motivo subjacente é o mesmo que com o rótulo de `Results` e `Age` controles de caixa de texto: `FindControl` inicia sua pesquisa a partir da parte superior da hierarquia de controle e não penetra os contêineres de nomenclatura, mas o `MainContent` ContentPlaceHolder está dentro da página mestra, que é um contêiner de nomenclatura.

Antes de poder usar `FindControl` para obter uma referência a `MainContent`, precisamos primeiro de uma referência ao controle da página mestra. Assim que tivermos uma referência à página mestra, podemos obter uma referência para o `MainContent` ContentPlaceHolder por meio de `FindControl` e, a partir daí, referências ao rótulo de `Results` e `Age` caixa de texto (novamente, usando `FindControl`). Mas como obtemos uma referência à página mestra? Inspecionando os atributos de `id` na marcação renderizada, é evidente que o valor `ID` da página mestra é `ctl00`. Portanto, poderíamos usar `Page.FindControl("ctl00")` para obter uma referência à página mestra, usar esse objeto para obter uma referência a `MainContent`e assim por diante. O trecho a seguir ilustra essa lógica:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample10.vb)]

Embora esse código certamente funcione, ele pressupõe que o `ID` gerado automaticamente da página mestra sempre será `ctl00`. Nunca é uma boa ideia fazer suposições sobre valores gerados automaticamente.

Felizmente, uma referência à página mestra pode ser acessada por meio da propriedade `Master` da classe `Page`. Portanto, em vez de ter que usar `FindControl("ctl00")` para obter uma referência da página mestra a fim de acessar o `MainContent` ContentPlaceHolder, podemos usar `Page.Master.FindControl("MainContent")`. Atualize o manipulador de eventos `SubmitButton_Click` com o seguinte código:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample11.vb)]

Desta vez, visitar a página por meio de um navegador, entrar em sua idade e clicar no botão "enviar" exibe a mensagem no rótulo `Results`, conforme esperado.

[![a idade do usuário é exibida no rótulo](control-id-naming-in-content-pages-vb/_static/image11.png)](control-id-naming-in-content-pages-vb/_static/image10.png)

**Figura 06**: a idade do usuário é exibida no rótulo ([clique para exibir a imagem em tamanho normal](control-id-naming-in-content-pages-vb/_static/image12.png))

### <a name="recursively-searching-through-naming-containers"></a>Pesquisa recursiva de contêineres de nomenclatura

O motivo pelo qual o exemplo de código anterior referenciou o controle `MainContent` ContentPlaceHolder da página mestra e, em seguida, os controles `Results` Label e `Age` TextBox de `MainContent`, é porque o método `Control.FindControl` só pesquisa no contêiner de nomenclatura do *controle*. Ter `FindControl` permanecer dentro do contêiner de nomenclatura faz sentido na maioria dos cenários, pois dois controles em dois contêineres de nomenclatura diferentes podem ter os mesmos valores de `ID`. Considere o caso de um GridView que define um controle de rótulo da Web chamado `ProductName` dentro de um de seus TemplateFields. Quando os dados são associados ao GridView em tempo de execução, um rótulo de `ProductName` é criado para cada linha GridView. Se `FindControl` pesquisou por todos os contêineres de nomenclatura e chamamos `Page.FindControl("ProductName")`, qual instância de rótulo deve ser retornada pelo `FindControl`? O rótulo `ProductName` na primeira linha GridView? O que está na última linha?

Assim, ter `Control.FindControl` Pesquisar o contêiner de nomeação do *controle*faz sentido na maioria dos casos. Mas há outros casos, como o voltado para nós, onde temos um `ID` exclusivo em todos os contêineres de nomenclatura e desejam evitar ter que fazer referência meticulosa a cada contêiner de nomenclatura na hierarquia de controle para acessar um controle. Ter uma `FindControl` variante que pesquisa recursivamente todos os contêineres de nomenclatura também faz sentido. Infelizmente, o .NET Framework não inclui tal método.

A boa notícia é que podemos criar nosso próprio método de `FindControl` que pesquisa recursivamente todos os contêineres de nomenclatura. Na verdade, usando *métodos de extensão* , podemos acrescentar um método `FindControlRecursive` à classe `Control` para acompanhar seu método de `FindControl` existente.

> [!NOTE]
> Os métodos de extensão são um recurso C# novo para 3,0 e Visual Basic 9, que são os idiomas fornecidos com o .NET Framework versão 3,5 e o Visual Studio 2008. Em suma, os métodos de extensão permitem que um desenvolvedor crie um novo método para um tipo de classe existente por meio de uma sintaxe especial. Para obter mais informações sobre esse recurso útil, consulte meu artigo, [estendendo a funcionalidade de tipo base com métodos de extensão](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).

Para criar o método de extensão, adicione um novo arquivo à pasta `App_Code` chamada `PageExtensionMethods.vb`. Adicione um método de extensão chamado `FindControlRecursive` que usa como entrada um parâmetro de `String` chamado `controlID`. Para que os métodos de extensão funcionem corretamente, é vital que a classe seja marcada como um `Module` e que os métodos de extensão sejam prefixados com o atributo `<Extension()>`. Além disso, todos os métodos de extensão devem aceitar como seu primeiro parâmetro um objeto do tipo ao qual o método de extensão se aplica.

Adicione o seguinte código ao arquivo de `PageExtensionMethods.vb` para definir esse `Module` e o método de extensão `FindControlRecursive`:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample12.vb)]

Com esse código em vigor, retorne à classe code-behind da página `IDIssues.aspx` e comente as chamadas de método `FindControl` atuais. Substitua-os por chamadas para `Page.FindControlRecursive("controlID")`. O que é mais claro sobre os métodos de extensão é que eles aparecem diretamente nas listas suspensas do IntelliSense. Como mostra a Figura 7, quando você digita `Page` e, em seguida, o período de tempo, o método `FindControlRecursive` é incluído na lista suspensa IntelliSense junto com os outros métodos de classe `Control`.

[![métodos de extensão estão incluídos nos menus suspensos do IntelliSense](control-id-naming-in-content-pages-vb/_static/image14.png)](control-id-naming-in-content-pages-vb/_static/image13.png)

**Figura 07**: os métodos de extensão são incluídos nos menus suspensos do IntelliSense ([clique para exibir a imagem em tamanho normal](control-id-naming-in-content-pages-vb/_static/image15.png))

Insira o código a seguir no manipulador de eventos `SubmitButton_Click` e, em seguida, teste-o visitando a página, inserindo sua idade e clicando no botão "enviar". Conforme mostrado na Figura 6, a saída resultante será a mensagem "você tem anos de idade".

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample13.vb)]

> [!NOTE]
> Como os métodos de extensão são C# novos para 3,0 e Visual Basic 9, se você estiver usando o Visual Studio 2005, não poderá usar métodos de extensão. Em vez disso, você precisará implementar o método `FindControlRecursive` em uma classe auxiliar. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) tem um exemplo desse tipo em sua postagem de blog, [páginas de maser ASP.net e `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx).

## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Etapa 4: usando o valor de atributo de`id`correto no script do lado do cliente

Conforme observado na introdução deste tutorial, um atributo `id` renderizado de um controle da Web é usado muitas vezes no script do lado do cliente para fazer referência programaticamente a um determinado elemento HTML. Por exemplo, o JavaScript a seguir faz referência a um elemento HTML por seu `id` e, em seguida, exibe seu valor em uma caixa de mensagem modal:

[!code-csharp[Main](control-id-naming-in-content-pages-vb/samples/sample14.cs)]

Lembre-se de que em páginas ASP.NET que não incluem um contêiner de nomenclatura, o atributo de `id` do elemento HTML renderizado é idêntico ao valor da propriedade `ID` do controle da Web. Por isso, é tentador embutir código em `id` valores de atributo em código JavaScript. Ou seja, se você souber que deseja acessar o controle da Web de caixa de texto `Age` por meio de script do lado do cliente, faça isso por meio de uma chamada para `document.getElementById("Age")`.

O problema dessa abordagem é que, ao usar páginas mestras (ou outros controles de contêiner de nomenclatura), o HTML renderizado `id` não é sinônimo da propriedade `ID` do controle da Web. Sua primeira inclinação pode ser visitar a página por meio de um navegador e exibir a origem para determinar o atributo de `id` real. Depois de saber o valor de `id` renderizado, você pode colá-lo na chamada para `getElementById` para acessar o elemento HTML com o qual você precisa trabalhar por meio do script do lado do cliente. Essa abordagem é menor que o ideal, pois determinadas alterações na hierarquia de controle da página ou alterações nas propriedades `ID` dos controles de nomenclatura alterarão o atributo `id` resultante, dividindo assim o código JavaScript.

A boa notícia é que o valor do atributo de `id` que é processado é acessível no código do lado do servidor por meio da [propriedade`ClientID`](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx)do controle da Web. Você deve usar essa propriedade para determinar o valor do atributo `id` usado no script do lado do cliente. Por exemplo, para adicionar uma função JavaScript à página que, quando chamada, exibe o valor da caixa de texto `Age` em uma janela de mensagem modal, adicione o seguinte código ao manipulador de eventos `Page_Load`:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample15.vb)]

O código acima injeta o valor da propriedade `ClientID` do `Age` caixa de texto na chamada JavaScript para `getElementById`. Se você visitar esta página por meio de um navegador e exibir a origem HTML, encontrará o seguinte código JavaScript:

[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample16.html)]

Observe como o valor de atributo de `id` correto, `ctl00_MainContent_Age`, aparece na chamada para `getElementById`. Como esse valor é calculado em tempo de execução, ele funciona independentemente das alterações posteriores na hierarquia de controle de página.

> [!NOTE]
> Este exemplo de JavaScript mostra apenas como adicionar uma função JavaScript que referencia corretamente o elemento HTML processado por um controle de servidor. Para usar essa função, você precisaria criar JavaScript adicional para chamar a função quando o documento for carregado ou quando alguma ação de usuário específica for transacionada. Para obter mais informações sobre esses e tópicos relacionados, leia [trabalhando com o script do lado do cliente](https://msdn.microsoft.com/library/aa479302.aspx).

## <a name="summary"></a>Resumo

Determinados controles de servidor ASP.NET atuam como contêineres de nomenclatura, que afetam os valores de atributo `id` renderizados de seus controles descendentes, bem como o escopo de controles canvased pelo método `FindControl`. Com relação às páginas mestras, a própria página mestra e seus controles ContentPlaceHolder são contêineres de nomenclatura. Consequentemente, precisamos colocar um pouco mais de trabalho para fazer referência programaticamente a controles na página de conteúdo usando `FindControl`. Neste tutorial, examinamos duas técnicas: Drilling no controle ContentPlaceHolder e chamar seu método `FindControl`; e a distribuição de nossa própria implementação de `FindControl` que pesquisa recursivamente todos os contêineres de nomenclatura.

Além dos erros do lado do servidor, os contêineres de nomenclatura introduzem com relação à referência de controles da Web, também há problemas no lado do cliente. Na ausência de contêineres de nomenclatura, o valor da propriedade `ID` do controle da Web e o valor do atributo `id` processados são um no mesmo. Mas com a adição do contêiner de nomenclatura, o atributo `id` processado inclui os valores de `ID` do controle da Web e os contêineres de nomenclatura em sua hierarquia de controle ancestrais. Essas preocupações com nomes não são uma questão, desde que você use a propriedade `ClientID` do controle da Web para determinar o valor do atributo de `id` renderizado no seu script do lado do cliente.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [ASP.NET páginas mestras e `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Criando Dados Dinâmicos interfaces de usuário de entrada](https://msdn.microsoft.com/library/aa479330.aspx)
- [Estendendo a funcionalidade de tipo base com métodos de extensão](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Como referenciar o conteúdo da página mestra ASP.NET](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Páginas do mate: dicas, truques e armadilhas](http://www.odetocode.com/articles/450.aspx)
- [Trabalhando com script do lado do cliente](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP. net e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 3,5 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott pode ser contatado em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Zack Jones e Barnerjee. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](urls-in-master-pages-vb.md)
> [Próximo](interacting-with-the-master-page-from-the-content-page-vb.md)
