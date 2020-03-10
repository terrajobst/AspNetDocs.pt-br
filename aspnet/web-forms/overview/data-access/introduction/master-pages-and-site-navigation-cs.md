---
uid: web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
title: Páginas mestras e navegação doC#site () | Microsoft Docs
author: rick-anderson
description: Uma característica comum dos sites amigáveis para o usuário é que eles têm um esquema de navegação e layout de página de todo o site consistente. Este tutorial analisa como y...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 5aee8202-a4e3-4aa9-8a95-cd5d156cea4c
msc.legacyurl: /web-forms/overview/data-access/introduction/master-pages-and-site-navigation-cs
msc.type: authoredcontent
ms.openlocfilehash: e1ddd43524a61ff2e012171eba1a8dc8efbf8f1d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78530838"
---
# <a name="master-pages-and-site-navigation-c"></a>Páginas mestras e navegação no site (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_3_CS.exe) ou [baixar PDF](master-pages-and-site-navigation-cs/_static/datatutorial03cs1.pdf)

> Uma característica comum dos sites amigáveis para o usuário é que eles têm um esquema de navegação e layout de página de todo o site consistente. Este tutorial analisa como você pode criar uma aparência consistente em todas as páginas que podem ser facilmente atualizadas.

## <a name="introduction"></a>Introdução

Uma característica comum dos sites amigáveis para o usuário é que eles têm um esquema de navegação e layout de página de todo o site consistente. O ASP.NET 2,0 apresenta dois novos recursos que simplificam muito a implementação de um layout de página de todo o site e esquema de navegação: páginas mestras e navegação do site. As páginas mestras permitem que os desenvolvedores criem um modelo de todo o site com regiões editáveis designadas. Esse modelo pode então ser aplicado a páginas ASP.NET no site. Essas páginas ASP.NET só precisam fornecer conteúdo para as regiões editáveis especificadas da página mestra. todas as outras marcações na página mestra são idênticas em todas as páginas do ASP.NET que usam a página mestra. Esse modelo permite aos desenvolvedores definir e centralizar um layout de página de todo o site, facilitando a criação de uma aparência consistente em todas as páginas que possam ser facilmente atualizadas.

O [sistema de navegação do site](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) fornece um mecanismo para que os desenvolvedores de página definam um mapa do site e uma API para esse mapa do site seja consultado programaticamente. A nova Web de navegação controla o menu, TreeView e SiteMapPath para facilitar o processamento de todo ou parte do mapa do site em um elemento de interface de usuário de navegação comum. Usaremos o provedor de navegação do site padrão, o que significa que nosso mapa do site será definido em um arquivo formatado em XML.

Para ilustrar esses conceitos e tornar nosso site de tutoriais mais utilizável, vamos gastar esta lição definindo um layout de página de todo o site, implementando um mapa do site e adicionando a interface do usuário de navegação. Ao final deste tutorial, teremos um design de site elegante para a criação de nossas páginas da Web de tutorial.

[![o resultado final deste tutorial](master-pages-and-site-navigation-cs/_static/image2.png)](master-pages-and-site-navigation-cs/_static/image1.png)

**Figura 1**: o resultado final deste tutorial ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image3.png))

## <a name="step-1-creating-the-master-page"></a>Etapa 1: criando a página mestra

A primeira etapa é criar a página mestra para o site. No momento, nosso site consiste apenas no conjunto de dados tipado (`Northwind.xsd`, na pasta `App_Code`), nas classes BLL (`ProductsBLL.cs`, `CategoriesBLL.cs`e assim por diante, tudo na pasta `App_Code`), o banco de dados (`NORTHWND.MDF`, na pasta `App_Data`), o arquivo de configuração (`Web.config`) e um arquivo de folha de estilos CSS (`Styles.css`). Eu limpei essas páginas e arquivos demonstrando usando a DAL e a BLL dos primeiros dois tutoriais, já que iremos reexaminar esses exemplos mais detalhadamente em Tutoriais futuros.

![Os arquivos em nosso projeto](master-pages-and-site-navigation-cs/_static/image4.png)

**Figura 2**: os arquivos em nosso projeto

Para criar uma página mestra, clique com o botão direito do mouse no nome do projeto na Gerenciador de Soluções e escolha Adicionar novo item. Em seguida, selecione o tipo de página mestra na lista de modelos e nomeie-o `Site.master`.

[![adicionar uma nova página mestra ao site](master-pages-and-site-navigation-cs/_static/image6.png)](master-pages-and-site-navigation-cs/_static/image5.png)

**Figura 3**: adicionar uma nova página mestra ao site ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image7.png))

Defina o layout de página de todo o site aqui na página mestra. Você pode usar a modo de exibição de Design e adicionar quaisquer controles de layout ou Web necessários, ou pode adicionar manualmente a marcação à mão na exibição de origem. Em minha página mestra, uso [folhas de estilo em cascata](http://www.w3schools.com/css/default.asp) para posicionamento e estilos com as configurações de CSS definidas no arquivo externo `Style.css`. Embora não seja possível determinar a partir da marcação mostrada abaixo, as regras de CSS são definidas de modo que o conteúdo da `<div>`de navegação seja absolutamente posicionado para que ele apareça à esquerda e tenha uma largura fixa de 200 pixels.

Site.master

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample1.aspx)]

Uma página mestra define o layout de página estática e as regiões que podem ser editadas pelas páginas ASP.NET que usam a página mestra. Essas regiões editáveis de conteúdo são indicadas pelo controle ContentPlaceHolder, que pode ser visto dentro do conteúdo `<div>`. Nossa página mestra tem um único ContentPlaceHolder (`MainContent`), mas a página mestra pode ter vários ContentPlaceHolders.

Com a marcação inserida acima, alternar para o modo de exibição de Design mostra o layout da página mestra. Todas as páginas ASP.NET que usam essa página mestra terão esse layout uniforme, com a capacidade de especificar a marcação para a região de `MainContent`.

[![a página mestra, quando exibida por meio do modo de exibição de design](master-pages-and-site-navigation-cs/_static/image9.png)](master-pages-and-site-navigation-cs/_static/image8.png)

**Figura 4**: a página mestra, quando exibida por meio do modo de exibição de design ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image10.png))

## <a name="step-2-adding-a-homepage-to-the-website"></a>Etapa 2: adicionar uma página inicial ao site

Com a página mestra definida, estamos prontos para adicionar as páginas ASP.NET para o site. Vamos começar adicionando `Default.aspx`, a Home Page de nosso site. Clique com o botão direito do mouse no nome do projeto na Gerenciador de Soluções e escolha Adicionar novo item. Escolha a opção formulário da Web na lista modelo e nomeie o arquivo `Default.aspx`. Além disso, marque a caixa de seleção "selecionar página mestra".

[![adicionar um novo formulário da Web, marcando a caixa de seleção selecionar página mestra](master-pages-and-site-navigation-cs/_static/image12.png)](master-pages-and-site-navigation-cs/_static/image11.png)

**Figura 5**: adicionar um novo formulário da Web, marcando a caixa de seleção selecionar página mestra ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image13.png))

Depois de clicar no botão OK, será solicitado que você escolha qual página mestra essa nova página de ASP.NET deve usar. Embora você possa ter várias páginas mestras em seu projeto, temos apenas uma.

[![escolher a página mestra que esta página ASP.NET deve usar](master-pages-and-site-navigation-cs/_static/image15.png)](master-pages-and-site-navigation-cs/_static/image14.png)

**Figura 6**: escolha a página mestra que esta página ASP.NET deve usar ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image16.png))

Depois de escolher a página mestra, as novas páginas ASP.NET conterão a seguinte marcação:

Default.aspx

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample2.aspx)]

Na diretiva `@Page` há uma referência ao arquivo de página mestra usado (`MasterPageFile="~/Site.master"`), e a marcação da página ASP.NET contém um controle de conteúdo para cada um dos controles ContentPlaceHolder definidos na página mestra, com o `ContentPlaceHolderID` de controle de mapeamento do controle de conteúdo para um ContentPlaceHolder específico. O controle de conteúdo é onde você coloca a marcação que deseja que apareça no ContentPlaceHolder correspondente. Defina o atributo de `Title` da diretiva de `@Page` como Home e adicione um conteúdo de boas-vindas ao controle de conteúdo:

Default.aspx

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample3.aspx)]

O atributo `Title` na diretiva `@Page` nos permite definir o título da página na página ASP.NET, embora o elemento `<title>` seja definido na página mestra. Também podemos definir o título programaticamente, usando `Page.Title`. Observe também que as referências da página mestra a folhas de estilo (como `Style.css`) são atualizadas automaticamente para que funcionem em qualquer página do ASP.NET, independentemente de qual diretório a página ASP.NET está em relação à página mestra.

Alternando para o modo de exibição de Design podemos ver como nossa página será exibida em um navegador. Observe que na modo de exibição de Design da página ASP.NET que apenas as regiões editáveis de conteúdo são editáveis, a marcação não ContentPlaceHolder definida na página mestra está esmaecida.

[![a exibição de design da página ASP.NET mostra as regiões editáveis e não editáveis](master-pages-and-site-navigation-cs/_static/image18.png)](master-pages-and-site-navigation-cs/_static/image17.png)

**Figura 7**: a exibição de design da página ASP.net mostra as regiões editáveis e não editáveis ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image19.png))

Quando a página de `Default.aspx` é visitada por um navegador, o mecanismo de ASP.NET mescla automaticamente o conteúdo da página mestra da página e o ASP. O conteúdo da rede e renderiza o conteúdo mesclado no HTML final que é enviado para o navegador solicitante. Quando o conteúdo da página mestra é atualizado, todas as páginas ASP.NET que usam essa página mestra terão seu conteúdo remesclado com o novo conteúdo da página mestra na próxima vez que forem solicitados. Em suma, o modelo de página mestra permite que um modelo de layout de página única seja definido (a página mestra) cujas alterações são refletidas imediatamente em todo o site.

## <a name="adding-additional-aspnet-pages-to-the-website"></a>Adicionando páginas ASP.NET adicionais ao site

Vamos reservar um momento para adicionar mais stubs de página ASP.NET ao site que, eventualmente, manterá as várias demonstrações de relatório. Haverá mais de 35 demonstrações no total, portanto, em vez de criar todas as páginas de stub, vamos criar apenas as primeiras. Como também haverá muitas categorias de demonstrações, para gerenciar melhor as demonstrações, adicione uma pasta para as categorias. Adicione as três pastas a seguir por enquanto:

- `BasicReporting`
- `Filtering`
- `CustomFormatting`

Por fim, adicione novos arquivos conforme mostrado na Gerenciador de Soluções da Figura 8. Ao adicionar cada arquivo, lembre-se de marcar a caixa de seleção "selecionar página mestra".

![Adicione os seguintes arquivos](master-pages-and-site-navigation-cs/_static/image20.png)

**Figura 8**: adicionar os seguintes arquivos

## <a name="step-2-creating-a-site-map"></a>Etapa 2: Criando um mapa do site

Um dos desafios de gerenciar um site composto por mais de uma quantidade de páginas é fornecer uma maneira simples para os visitantes navegarem pelo site. Para começar, a estrutura de navegação do site deve ser definida. Em seguida, essa estrutura deve ser convertida em elementos de interface de usuário navegáveis, como menus ou trilhas. Por fim, todo esse processo precisa ser mantido e atualizado à medida que novas páginas são adicionadas ao site e as existentes foram removidas. Antes do ASP.NET 2,0, os desenvolvedores estavam por conta própria para criar a estrutura de navegação do site, mantê-lo e traduzi-lo para elementos de interface do usuário navegáveis. Com o ASP.NET 2,0, no entanto, os desenvolvedores podem utilizar o sistema de navegação de site muito flexível interno.

O sistema de navegação do site do ASP.NET 2,0 fornece um meio para um desenvolvedor definir um mapa do site e, em seguida, acessar essas informações por meio de uma API programática. O ASP.NET é fornecido com um provedor de mapa do site que espera que os dados do mapa do site sejam armazenados em um arquivo XML formatado de uma maneira específica. Mas, como o sistema de navegação do site é criado no [modelo do provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) , ele pode ser estendido para dar suporte a maneiras alternativas de serialização das informações do mapa do site. O artigo do Jeff Prosise, [o provedor do mapa do site do SQL que você estava esperando,](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx) mostra como criar um provedor de mapa do site que armazena o mapa do site em um banco de dados SQL Server; outra opção é criar [um provedor de mapa do site com base na estrutura do sistema de arquivos](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx).

Para este tutorial, no entanto, vamos usar o provedor de mapa do site padrão que acompanha o ASP.NET 2,0. Para criar o mapa do site, basta clicar com o botão direito do mouse no nome do projeto na Gerenciador de Soluções, escolher Adicionar novo item e escolher a opção mapa do site. Deixe o nome como `Web.sitemap` e clique no botão Adicionar.

[![adicionar um mapa do site ao seu projeto](master-pages-and-site-navigation-cs/_static/image22.png)](master-pages-and-site-navigation-cs/_static/image21.png)

**Figura 9**: adicionar um mapa do site ao seu projeto ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image23.png))

O arquivo de mapa do site é um arquivo XML. Observe que o Visual Studio fornece IntelliSense para a estrutura do mapa do site. O arquivo de mapa do site deve ter o nó `<siteMap>` como seu nó raiz, que deve conter precisamente um elemento filho `<siteMapNode>`. Esse primeiro elemento `<siteMapNode>` pode então conter um número arbitrário de elementos de `<siteMapNode>` descendentes.

Defina o mapa do site para imitar a estrutura do sistema de arquivos. Ou seja, adicione um elemento `<siteMapNode>` para cada uma das três pastas e os elementos de `<siteMapNode>` filhos para cada uma das páginas ASP.NET nessas pastas, desta forma:

Web.sitemap

[!code-xml[Main](master-pages-and-site-navigation-cs/samples/sample4.xml)]

O mapa do site define a estrutura de navegação do site, que é uma hierarquia que descreve as várias seções do site. Cada elemento `<siteMapNode>` no `Web.sitemap` representa uma seção na estrutura de navegação do site.

[![o mapa do site representa uma estrutura de navegação hierárquica](master-pages-and-site-navigation-cs/_static/image25.png)](master-pages-and-site-navigation-cs/_static/image24.png)

**Figura 10**: o mapa do site representa uma estrutura de navegação hierárquica ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image26.png))

ASP.NET expõe a estrutura do mapa do site por meio da [classe SiteMap](https://msdn.microsoft.com/library/system.web.sitemap.aspx)do .NET Framework. Essa classe tem uma propriedade `CurrentNode`, que retorna informações sobre a seção que o usuário está visitando no momento; a propriedade `RootNode` retorna a raiz do mapa do site (página inicial, em nosso mapa do site). As propriedades `CurrentNode` e `RootNode` retornam instâncias de [SiteMapNode](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) , que têm propriedades como `ParentNode`, `ChildNodes`, `NextSibling`, `PreviousSibling`e assim por diante, que permitem que a hierarquia do mapa do site seja movimentada.

## <a name="step-3-displaying-a-menu-based-on-the-site-map"></a>Etapa 3: exibindo um menu baseado no mapa do site

O acesso a dados no ASP.NET 2,0 pode ser realizado programaticamente, como no ASP.NET 1. x, ou declarativamente, por meio dos novos [controles de fonte de dados](https://msdn.microsoft.com/library/ms227679.aspx). Há vários controles de fonte de dados internos como, por exemplo, o controle SqlDataSource, para acessar dados de banco de dados relacional, o controle ObjectDataSource, para acessar dados de classes e outros. Você pode até mesmo criar seus próprios [controles de fonte de dados personalizados](https://msdn.microsoft.com/asp.net/reference/data/default.aspx?pull=/library/dnvs05/html/DataSourceCon1.asp).

Os controles da fonte de dados servem como um proxy entre sua página ASP.NET e os dados subjacentes. Para exibir os dados recuperados de um controle da fonte de dados, normalmente adicionaremos outro controle da Web à página e o associaremos ao controle da fonte de dados. Para associar um controle da Web a um controle da fonte de dados, basta definir a propriedade `DataSourceID` do controle da Web como o valor da propriedade `ID` do controle da fonte de dados.

Para ajudar a trabalhar com os dados do mapa do site, o ASP.NET inclui o controle SiteMapDataSource, que nos permite associar um controle da Web ao mapa do site do nosso site. Dois controles da Web o TreeView e o menu são comumente usados para fornecer uma interface de usuário de navegação. Para associar os dados do mapa do site a um desses dois controles, basta adicionar um SiteMapDataSource à página junto com um controle TreeView ou menu cuja propriedade `DataSourceID` esteja definida de acordo. Por exemplo, poderíamos adicionar um controle de menu à página mestra usando a seguinte marcação:

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample5.aspx)]

Para um grau mais preciso de controle sobre o HTML emitido, podemos ligar o controle SiteMapDataSource ao controle Repeater da seguinte forma:

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample6.aspx)]

O controle SiteMapDataSource retorna a hierarquia do mapa do site um nível por vez, começando com o nó de mapa do site raiz (Home, em nosso mapa do site), em seguida, o próximo nível (relatórios básicos, relatórios de filtragem e formatação personalizada) e assim por diante. Ao ligar o SiteMapDataSource a um repetidor, ele enumera o primeiro nível retornado e instancia o `ItemTemplate` para cada instância de `SiteMapNode` nesse primeiro nível. Para acessar uma propriedade específica do `SiteMapNode`, podemos usar `Eval(propertyName)`, que é como obtemos as propriedades `Url` e `Title` de cada `SiteMapNode`para o controle HyperLink.

O exemplo de Repeater acima processará a seguinte marcação:

[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample7.html)]

Esses nós de mapa do site (relatórios básicos, relatórios de filtragem e formatação personalizada) compõem o *segundo* nível do mapa do site que está sendo renderizado, não o primeiro. Isso ocorre porque a propriedade de `ShowStartingNode` de SiteMapDataSource é definida como false, fazendo com que o SiteMapDataSource ignore o nó do mapa do site raiz e, em vez disso, comece retornando o segundo nível na hierarquia do mapa do site.

Para exibir os filhos para os relatórios básicos, os relatórios de filtragem e a formatação personalizada `SiteMapNode` s, podemos adicionar outro repetidor à `ItemTemplate`do repetidor inicial. Esse segundo repetidor será associado à propriedade `ChildNodes` da instância de `SiteMapNode`, desta forma:

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample8.aspx)]

Esses dois repetidores resultam na seguinte marcação (algumas marcações foram removidas para fins de brevidade):

[!code-html[Main](master-pages-and-site-navigation-cs/samples/sample9.html)]

Usando estilos CSS escolhidos por meio do livro de [Andrew do Rachel](http://www.rachelandrew.co.uk/), [o css Anthology: 101 dicas essenciais, truques &amp; hackers](https://www.amazon.com/gp/product/0957921888/qid=1137565739/sr=8-1/ref=pd_bbs_1/103-0562306-3386214?n=507846&amp;s=books&amp;v=glance), os elementos `<ul>` e `<li>` são estilizados de forma que a marcação produz a seguinte saída Visual:

![Um menu composto de dois repetidores e um CSS](master-pages-and-site-navigation-cs/_static/image27.png)

**Figura 11**: um menu composto de dois repetidores e um CSS

Esse menu está na página mestra e está vinculado ao mapa do site definido em `Web.sitemap`, o que significa que qualquer alteração no mapa do site será refletida imediatamente em todas as páginas que usam a página mestra `Site.master`.

## <a name="disabling-viewstate"></a>Desabilitando ViewState

Todos os controles ASP.NET podem, opcionalmente, persistir seu estado para o [estado de exibição](https://msdn.microsoft.com/msdnmag/issues/03/02/CuttingEdge/), que é serializado como um campo de formulário oculto no HTML renderizado. O estado de exibição é usado pelos controles para lembrar seu estado alterado de forma programática entre postbacks, como os dados associados a um controle da Web de dados. Embora o estado de exibição permita que as informações sejam lembradas entre os postbacks, ele aumenta o tamanho da marcação que deve ser enviada ao cliente e pode levar a um inchar severo de página, se não for bastante monitorado. Os controles da Web de dados especialmente o GridView são particularmente evidentes para adicionar dezenas de kilobytes extras de marcação a uma página. Embora esse aumento possa ser insignificante para usuários de banda larga ou intranet, o estado de exibição pode adicionar vários segundos à viagem de ida e volta para usuários de conexão discada.

Para ver o impacto do estado de exibição, visite uma página em um navegador e, em seguida, exiba a origem enviada pela página da Web (no Internet Explorer, vá para o menu exibir e escolha a opção origem). Você também pode ativar o [rastreamento de página](https://msdn.microsoft.com/library/sfbfw58f.aspx) para ver a alocação de estado de exibição usada por cada um dos controles na página. As informações de estado de exibição são serializadas em um campo de formulário oculto chamado `__VIEWSTATE`, localizado em um elemento de `<div>` imediatamente após a marca de `<form>` de abertura. O estado de exibição só é mantido quando há um formulário da Web sendo usado; Se sua página ASP.NET não incluir um `<form runat="server">` em sua sintaxe declarativa, não haverá um campo de formulário `__VIEWSTATE` oculto na marcação renderizada.

O campo de formulário de `__VIEWSTATE` gerado pela página mestra adiciona aproximadamente 1.800 bytes à marcação gerada da página. Esse inflado extra se deve principalmente ao controle Repeater, pois o conteúdo do controle SiteMapDataSource é mantido para o estado de exibição. Embora um extra 1.800 bytes possa não parecer muito animados, ao usar um GridView com muitos campos e registros, o estado de exibição pode ser facilmente ultrapassardo por um fator de 10 ou mais.

O estado de exibição pode ser desabilitado no nível de página ou de controle, definindo a propriedade `EnableViewState` como `false`, reduzindo assim o tamanho da marcação renderizada. Como o estado de exibição de um controle da Web de dados persiste os dados associados ao controle da Web de dados em postagens, ao desabilitar o estado de exibição para um controle da Web de dados, os dados devem ser associados em cada postback. No ASP.NET versão 1. x, essa responsabilidade saiu do ombros do desenvolvedor da página; com o ASP.NET 2,0, no entanto, os controles da Web de dados serão reassociados ao controle da fonte de dados em cada postback, se necessário.

Para reduzir o estado de exibição da página, vamos definir a propriedade `EnableViewState` do controle Repeater como `false`. Isso pode ser feito por meio da janela Propriedades no designer ou declarativamente na exibição da fonte. Depois de fazer essa alteração, a marcação declarativa do repetidor deve ser semelhante a:

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample10.aspx)]

Após essa alteração, o tamanho do estado de exibição renderizado da página foi reduzido para um mero 52 bytes, uma economia de 97% no tamanho do estado de exibição! Nos tutoriais em toda esta série, desativaremos o estado de exibição dos controles da Web de dados por padrão para reduzir o tamanho da marcação renderizada. Na maioria dos exemplos, a propriedade `EnableViewState` será definida como `false` e feita sem menção. O único estado de exibição de tempo será discutido em cenários em que ele deve ser habilitado para que o controle da Web de dados forneça sua funcionalidade esperada.

## <a name="step-4-adding-breadcrumb-navigation"></a>Etapa 4: Adicionando navegação de trilha

Para concluir a página mestra, vamos adicionar um elemento de interface do usuário de navegação estrutural a cada página. A navegação rápida mostra aos usuários sua posição atual na hierarquia do site. Adicionar um breadcrumb no ASP.NET 2,0 é fácil simplesmente adicionar um controle SiteMapPath à página; nenhum código é necessário.

Para nosso site, adicione este controle ao cabeçalho `<div>`:

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample11.aspx)]

O breadcrumb mostra a página atual que o usuário está visitando na hierarquia do mapa do site, bem como os "ancestrais" do nó do mapa do site, até a raiz (Home, em nosso mapa de site).

![O breadcrumb exibe a página atual e seus ancestrais na hierarquia do mapa do site](master-pages-and-site-navigation-cs/_static/image28.png)

**Figura 12**: a navegação estrutural exibe a página atual e seus ancestrais na hierarquia do mapa do site

## <a name="step-5-adding-the-default-page-for-each-section"></a>Etapa 5: adicionando a página padrão para cada seção

Os tutoriais em nosso site são divididos em categorias diferentes, relatórios básicos, filtragem, formatação personalizada e assim por diante, com uma pasta para cada categoria e os tutoriais correspondentes como páginas ASP.NET dentro dessa pasta. Além disso, cada pasta contém uma página `Default.aspx`. Para esta página padrão, vamos exibir todos os tutoriais da seção atual. Ou seja, para o `Default.aspx` na pasta `BasicReporting`, temos links para `SimpleDisplay.aspx`, `DeclarativeParams.aspx`e `ProgrammaticParams.aspx`. Aqui, novamente, podemos usar a classe `SiteMap` e um controle da Web de dados para exibir essas informações com base no mapa do site definido em `Web.sitemap`.

Vamos exibir uma lista não ordenada usando um repetidor novamente, mas desta vez vamos exibir o título e a descrição dos tutoriais. Como a marcação e o código para fazer isso precisarão ser repetidos para cada página de `Default.aspx`, podemos encapsular essa lógica de interface do usuário em um [controle de usuários](https://msdn.microsoft.com/library/y6wb1a0e.aspx). Crie uma pasta no site chamada `UserControls` e adicione a ela um novo item do tipo controle de usuário da Web chamado `SectionLevelTutorialListing.ascx`e adicione a seguinte marcação:

[![adicionar um novo controle de usuário da Web à pasta UserControls](master-pages-and-site-navigation-cs/_static/image30.png)](master-pages-and-site-navigation-cs/_static/image29.png)

**Figura 13**: adicionar um novo controle de usuário da Web à pasta `UserControls` ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image31.png))

SectionLevelTutorialListing.ascx

[!code-aspx[Main](master-pages-and-site-navigation-cs/samples/sample12.aspx)]

SectionLevelTutorialListing.ascx.cs

[!code-csharp[Main](master-pages-and-site-navigation-cs/samples/sample13.cs)]

No exemplo de repetidor anterior, vinculamos os dados de `SiteMap` ao repetidor de forma declarativa; o controle de usuário `SectionLevelTutorialListing`, no entanto, faz isso programaticamente. No manipulador de eventos `Page_Load`, é feita uma verificação para garantir que essa URL de s de página seja mapeada para um nó no mapa do site. Se esse controle de usuário for usado em uma página que não tem uma entrada de `<siteMapNode>` correspondente, `SiteMap.CurrentNode` retornará `null` e nenhum dado será associado ao repetidor. Supondo que tenhamos um `CurrentNode`, vinculamos sua coleção de `ChildNodes` ao repetidor. Como nosso mapa do site é configurado de modo que a página `Default.aspx` em cada seção é o nó pai de todos os tutoriais dentro dessa seção, esse código exibirá links e descrições de todos os tutoriais da seção, conforme mostrado na captura de tela abaixo.

Depois que esse repetidor tiver sido criado, abra o `Default.aspx` páginas em cada uma das pastas, vá para a modo de exibição de Design e simplesmente arraste o controle de usuário do Gerenciador de Soluções para a superfície de design onde você deseja que a lista de tutoriais apareça.

[![o controle de usuário foi adicionado a default. aspx](master-pages-and-site-navigation-cs/_static/image33.png)](master-pages-and-site-navigation-cs/_static/image32.png)

**Figura 14**: o controle de usuário foi adicionado ao `Default.aspx` ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image34.png))

[![os tutoriais de relatórios básicos estão listados](master-pages-and-site-navigation-cs/_static/image36.png)](master-pages-and-site-navigation-cs/_static/image35.png)

**Figura 15**: os tutoriais de relatórios básicos são listados ([clique para exibir a imagem em tamanho normal](master-pages-and-site-navigation-cs/_static/image37.png))

## <a name="summary"></a>Resumo

Com o mapa do site definido e a página mestra concluída, agora temos um layout de página consistente e um esquema de navegação para nossos tutoriais relacionados a dados. Independentemente de quantas páginas adicionamos ao nosso site, atualizar o layout de página de todo o site ou as informações de navegação do site é um processo rápido e simples, pois essas informações estão sendo centralizadas. Especificamente, as informações de layout de página são definidas na página mestra `Site.master` e no mapa do site em `Web.sitemap`. Não precisamos escrever *nenhum* código para atingir este layout de página de todo o site e o mecanismo de navegação, e reguardamos o suporte completo a designer WYSIWYG no Visual Studio.

Depois de ter concluído a camada de acesso a dados e de lógica de negócios e ter um layout de página consistente e a navegação do site definidos, estamos prontos para começar a explorar os padrões de relatório comuns. Nos [próximos](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) três tutoriais, veremos as tarefas básicas de geração de relatórios que exibem os dados recuperados da BLL nos controles GridView, DetailsView e FormView.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Visão geral das páginas mestras do ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Páginas mestras no ASP.NET 2,0](http://odetocode.com/Articles/419.aspx)
- [Modelos de design do ASP.NET 2,0](https://msdn.microsoft.com/asp.net/reference/design/templates/default.aspx)
- [Visão geral de navegação do site ASP.NET](https://msdn.microsoft.com/library/e468hxky.aspx)
- [Examinando a navegação do site do ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Recursos de navegação do site do ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2005/11/20/431019.aspx)
- [Entendendo o estado de exibição do ASP.NET](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/viewstate.asp)
- [Como habilitar o rastreamento para uma página do ASP.NET](https://msdn.microsoft.com/library/94c55d08%28VS.80%29.aspx)
- [Controles de usuário ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Liz Shulok, Dennis Patterson e Hilton Giesenow. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-a-business-logic-layer-cs.md)
> [Próximo](creating-a-data-access-layer-vb.md)
