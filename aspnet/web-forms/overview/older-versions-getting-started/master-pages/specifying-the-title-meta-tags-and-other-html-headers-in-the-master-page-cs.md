---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
title: Especificando o título, as marcas meta e outros cabeçalhos HTML na página mestra (C#) | Microsoft Docs
author: rick-anderson
description: Examina técnicas diferentes para definir os elementos de&gt; de cabeçalho de &lt;de classificação na página mestra da página de conteúdo.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: 0aa1c84f-c9e2-4699-b009-0e28643ecbc6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: a4ec96a5b90f664655d554c064f9d50e76ad2d58
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587104"
---
# <a name="specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-c"></a>Especificando o título, as metamarcas e outros cabeçalhos de HTML na página mestra (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_03_CS.zip) ou [baixar PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_03_CS.pdf)

> Examina técnicas diferentes para definir os elementos de&gt; de cabeçalho de &lt;de classificação na página mestra da página de conteúdo.

## <a name="introduction"></a>Introdução

As novas páginas mestras criadas no Visual Studio 2008 têm, por padrão, dois controles ContentPlaceHolder: um cabeçalho nomeado e estão localizados no elemento `<head>`; e um chamado `ContentPlaceHolder1`, colocado dentro do formulário da Web. A finalidade do `ContentPlaceHolder1` é definir uma região no formulário da Web que possa ser personalizada página por página. O `head` ContentPlaceHolder permite que as páginas adicionem conteúdo personalizado à seção `<head>`. (É claro que esses dois ContentPlaceHolders podem ser modificados ou removidos, e ContentPlaceHolder adicional pode ser adicionado à página mestra. Nossa página mestra, `Site.master`, atualmente tem quatro controles ContentPlaceHolder.)

O elemento HTML `<head>` serve como um repositório para obter informações sobre o documento da página da Web que não faz parte do próprio documento. Isso inclui informações como o título da página da Web, meta-informações usadas por mecanismos de pesquisa ou rastreadores internos e links para recursos externos, como RSS feeds, JavaScript e arquivos CSS. Algumas dessas informações podem ser pertinentes a todas as páginas no site. Por exemplo, talvez você queira importar globalmente as mesmas regras CSS e arquivos JavaScript para cada página do ASP.NET. No entanto, há partes do `<head>` elemento que são específicas de página. O título da página é um exemplo primo.

Neste tutorial, examinaremos como definir a marcação de seção `<head>` global e específica da página na página mestra e em suas páginas de conteúdo.

## <a name="examining-the-master-pagesheadsection"></a>Examinando a seção`<head>`da página mestra

O arquivo de página mestra padrão criado pelo Visual Studio 2008 contém a seguinte marcação em sua `<head>` seção:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample1.aspx)]

Observe que o elemento `<head>` contém um atributo `runat="server"`, que indica que se trata de um controle de servidor (em vez de HTML estático). Todas as páginas ASP.NET derivam da [classe`Page`](https://msdn.microsoft.com/library/system.web.ui.page.aspx), localizada no namespace `System.Web.UI`. Essa classe contém uma propriedade `Header` que fornece acesso à região `<head>` da página. Usando a [propriedade`Header`](https://msdn.microsoft.com/library/system.web.ui.page.header.aspx) , podemos definir o título de uma página ASP.net ou adicionar marcação adicional à seção `<head>` renderizada. É possível, então, personalizar o elemento `<head>` de uma página de conteúdo escrevendo um pouco de código no manipulador de eventos `Page_Load` da página. Examinaremos como definir programaticamente o título da página na etapa 1.

A marcação mostrada no elemento `<head>` acima também inclui um controle ContentPlaceHolder chamado Head. Esse controle ContentPlaceHolder não é necessário, pois as páginas de conteúdo podem adicionar conteúdo personalizado ao elemento `<head>` de forma programática. No entanto, é útil em situações em que uma página de conteúdo precisa adicionar marcação estática ao elemento `<head>`, uma vez que a marcação estática pode ser adicionada declarativamente ao controle de conteúdo correspondente em vez de programaticamente.

Além do elemento `<title>` e Head ContentPlaceHolder, o elemento `<head>` da página mestra deve conter qualquer marcação de nível de `<head>`que seja comum a todas as páginas. Em nosso site, todas as páginas usam as regras de CSS definidas no arquivo `Styles.css`. Consequentemente, atualizamos o elemento `<head>` no tutorial [*criando um layout em todo o site com páginas mestras*](creating-a-site-wide-layout-using-master-pages-cs.md) para incluir um elemento `<link>` correspondente. A marcação `<head>` atual de `Site.master` página mestra é mostrada abaixo.

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample2.aspx)]

## <a name="step-1-setting-a-content-pages-title"></a>Etapa 1: definindo o título de uma página de conteúdo

O título da página da Web é especificado por meio do elemento `<title>`. É importante definir o título de cada página para um valor apropriado. Ao visitar uma página, seu título é exibido na barra de título do navegador. Além disso, ao marcar uma página, os navegadores usam o título da página como o nome sugerido para o indicador. Além disso, muitos mecanismos de pesquisa mostram o título da página ao exibir os resultados da pesquisa.

> [!NOTE]
> Por padrão, o Visual Studio define o elemento `<title>` na página mestra como "página sem título". Da mesma forma, novas páginas ASP.NET têm suas `<title>` definidas como "página sem título" também. Como pode ser fácil esquecer de definir o título da página para um valor apropriado, há muitas páginas na Internet com o título "página sem título". Pesquisar o Google para páginas da Web com este título retorna aproximadamente 2.460.000 resultados. Mesmo a Microsoft está suscetível a publicar páginas da Web com o título "página sem título". No momento da redação deste artigo, uma pesquisa do Google relatou 236 dessas páginas da Web no domínio Microsoft.com.

Uma página ASP.NET pode especificar seu título de uma das seguintes maneiras:

- Colocando o valor diretamente no elemento `<title>`
- Usando o atributo `Title` na diretiva `<%@ Page %>`
- Definindo programaticamente a propriedade `Title` da página usando código como `Page.Title="title"` ou `Page.Header.Title="title"`.

As páginas de conteúdo não têm um elemento `<title>`, pois elas são definidas na página mestra. Portanto, para definir o título de uma página de conteúdo, você pode usar o atributo `Title` da diretiva de `<%@ Page %>` ou defini-lo programaticamente.

### <a name="setting-the-pages-title-declaratively"></a>Definindo o título da página declarativamente

O título de uma página de conteúdo pode ser definido declarativamente por meio do atributo `Title` da [diretiva`<%@ Page %>`](https://msdn.microsoft.com/library/ydy4x04a.aspx). Essa propriedade pode ser definida pela modificação direta da diretiva `<%@ Page %>` ou por meio do janela Propriedades. Vamos examinar ambas as abordagens.

Na exibição de origem, localize a diretiva `<%@ Page %>`, que está na parte superior da marcação declarativa da página. A diretiva `<%@ Page %>` para `Default.aspx` segue:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample3.aspx)]

A diretiva `<%@ Page %>` especifica atributos específicos de página usados pelo mecanismo ASP.NET ao analisar e compilar a página. Isso inclui seu arquivo de página mestra, o local de seu arquivo de código e seu título, entre outras informações.

Por padrão, ao criar uma nova página de conteúdo, o Visual Studio define o atributo `Title` como uma página sem título. Altere o atributo de `Title` do `Default.aspx`de "página sem título" para "tutoriais da página mestra" e exiba a página por meio de um navegador. A Figura 1 mostra a barra de título do navegador, que reflete o título da nova página.

![A barra de título do navegador agora mostra &quot;tutoriais de página mestra&quot; em vez de &quot;página sem título&quot;](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image1.png)

**Figura 01**: a barra de título do navegador agora mostra "tutoriais de página mestra" em vez de "página sem título"

O título da página também pode ser definido no janela Propriedades. No janela Propriedades, selecione documento na lista suspensa para carregar as propriedades no nível da página, que inclui a propriedade `Title`. A Figura 2 mostra o janela Propriedades depois que `Title` foi definido como "tutoriais da página mestra".

![Você pode configurar o título na janela Propriedades, também](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image2.png)

**Figura 02**: você pode configurar o título na janela Propriedades, também

### <a name="setting-the-pages-title-programmatically"></a>Definindo o título da página programaticamente

A marcação de `<head runat="server">` da página mestra é convertida em uma instância de [classe de`HtmlHead`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.aspx) quando a página é renderizada pelo mecanismo de ASP.net. A classe `HtmlHead` tem uma [propriedade`Title`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlhead.title.aspx) cujo valor é refletido no elemento `<title>` renderizado. Essa propriedade pode ser acessada da classe code-behind de uma página ASP.NET por meio de `Page.Header.Title`; essa mesma propriedade também pode ser acessada por meio de `Page.Title`.

Para praticar a configuração do título da página programaticamente, navegue até a classe code-behind da página `About.aspx` e crie um manipulador de eventos para o evento `Load` da página. Em seguida, defina o título da página como "tutoriais da página mestra:: sobre:: *Date*", em que *Data* é a data atual. Depois de adicionar esse código, seu manipulador de eventos `Page_Load` deve ser semelhante ao seguinte:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample4.cs)]

A Figura 3 mostra a barra de título do navegador ao visitar a página `About.aspx`.

![O título da página é definido de forma programática e inclui a data atual](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image3.png)

**Figura 03**: o título da página é definido por meio de programação e inclui a data atual

## <a name="step-2-automatically-assigning-a-page-title"></a>Etapa 2: atribuindo automaticamente um título de página

Como vimos na etapa 1, o título de uma página pode ser definido declarativamente ou de forma programática. Se você esquecer de alterar explicitamente o título para algo mais descritivo, no entanto, sua página terá o título padrão, "página sem título". O ideal é que o título da página seja definido automaticamente para nós no caso de não especificar explicitamente seu valor. Por exemplo, se no tempo de execução o título da página for "página sem título", talvez queiramos que o título seja atualizado automaticamente para ser o mesmo que o nome de arquivo da página ASP.NET. A boa notícia é que, com um pouco de trabalho antecipado, é possível ter o título atribuído automaticamente.

Todas as páginas da Web ASP.NET derivam da classe `Page` no namespace `System.Web.UI`. A classe `Page` define a funcionalidade mínima necessária para uma página ASP.NET e expõe propriedades essenciais como `IsPostBack`, `IsValid`, `Request`e `Response`, entre muitas outras. Muitas vezes, cada página em um aplicativo Web requer recursos ou funcionalidades adicionais. Uma maneira comum de fornecer isso é criar uma classe de página de base personalizada. Uma classe de página de base personalizada é uma classe que você cria que deriva da classe `Page` e inclui funcionalidade adicional. Depois que essa classe base tiver sido criada, você poderá fazer com que suas páginas ASP.NET derivem dela (em vez da classe `Page`), oferecendo, assim, a funcionalidade estendida às suas páginas do ASP.NET.

Nesta etapa, criamos uma página de base que define automaticamente o título da página para o nome de arquivo da página ASP.NET se o título não tiver sido definido de outra forma explicitamente. A etapa 3 examina a configuração do título da página com base no mapa do site.

> [!NOTE]
> Um exame completo de criação e uso de classes de página de base personalizadas está além do escopo desta série de tutoriais. Para obter mais informações, leia [usando uma classe base personalizada para as classes code-behind de páginas ASP.net](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx).

### <a name="creating-the-base-page-class"></a>Criando a classe da página de base

Nossa primeira tarefa é criar uma classe de página de base, que é uma classe que estende a classe `Page`. Comece adicionando uma pasta `App_Code` ao seu projeto clicando com o botão direito do mouse no nome do projeto na Gerenciador de Soluções, escolhendo Adicionar pasta ASP.NET e, em seguida, selecionando `App_Code`. Em seguida, clique com o botão direito do mouse na pasta `App_Code` e adicione uma nova classe chamada `BasePage.cs`. A Figura 4 mostra a Gerenciador de Soluções depois que a pasta `App_Code` e a `BasePage.cs` classe tiverem sido adicionadas.

![Adicione uma pasta App_Code e uma classe chamada BasePage](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image4.png)

**Figura 04**: adicionar uma pasta `App_Code` e uma classe chamada `BasePage`

> [!NOTE]
> O Visual Studio dá suporte a dois modos de gerenciamento de projeto: projetos de site e projetos de aplicativos Web. A pasta `App_Code` foi projetada para ser usada com o modelo de projeto de site. Se você estiver usando o modelo de projeto de aplicativo Web, coloque a classe `BasePage.cs` em uma pasta chamada algo diferente de `App_Code`, como `Classes`. Para obter mais informações sobre este tópico, consulte [migrando um projeto de site para um projeto de aplicativo Web](http://webproject.scottgu.com/CSharp/Migration2/Migration2.aspx).

Como a página de base personalizada serve como a classe base para classes code-behind de páginas ASP.NET, ele precisa estender a classe `Page`.

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample5.cs)]

Sempre que uma página ASP.NET é solicitada, ela prossegue em uma série de estágios, culminando na página solicitada que está sendo renderizada em HTML. Podemos tocar em um estágio substituindo o método `OnEvent` da classe `Page`. Para nossa página de base, vamos definir automaticamente o título se ele não tiver sido explicitamente especificado pelo estágio de `LoadComplete` (que, como você já deve ter adivinhado, ocorre após o estágio de `Load`).

Para fazer isso, substitua o método `OnLoadComplete` e insira o código a seguir:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample6.cs)]

O método `OnLoadComplete` começa determinando se a propriedade `Title` ainda não foi definida explicitamente. Se a propriedade `Title` for `null`, uma cadeia de caracteres vazia ou tiver o valor "página sem título", ela será atribuída ao nome do arquivo da página ASP.NET solicitada. O caminho físico para a página ASP.NET solicitada-`C:\MySites\Tutorial03\Login.aspx`, por exemplo, é acessível por meio da propriedade `Request.PhysicalPath`. O método `Path.GetFileNameWithoutExtension` é usado para extrair apenas a parte do nome do arquivo, e esse filename é atribuído à propriedade `Page.Title`.

> [!NOTE]
> Convido você a aprimorar essa lógica para melhorar o formato do título. Por exemplo, se o nome de arquivo da página for `Company-Products.aspx`, o código acima produzirá o título "produtos da empresa", mas, idealmente, o traço será substituído por um espaço, como em "produtos da empresa". Além disso, considere adicionar um espaço sempre que houver uma alteração de maiúsculas e minúsculas. Ou seja, considere adicionar código que transforma o nome de arquivo `OurBusinessHours.aspx` a um título de "nosso horário comercial".

### <a name="having-the-content-pages-inherit-the-base-page-class"></a>Fazer com que as páginas de conteúdo herdem a classe de página de base

Agora, precisamos atualizar as páginas ASP.NET em nosso site para derivar da página de base personalizada (`BasePage`) em vez da classe `Page`. Para fazer isso, vá para cada classe code-behind e altere a declaração de classe de:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample7.cs)]

Para:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample8.cs)]

Depois de fazer isso, visite o site por meio de um navegador. Se você visitar uma página cujo título é definido explicitamente, como `Default.aspx` ou `About.aspx`, o título explicitamente especificado será usado. No entanto, se você visitar uma página cujo título não tenha sido alterado do padrão ("página sem título"), a classe de página de base definirá o título como o nome de arquivo da página.

A Figura 5 mostra a página de `MultipleContentPlaceHolders.aspx` quando exibida por meio de um navegador. Observe que o título é precisamente o nome de arquivo da página (menos a extensão), "MultipleContentPlaceHolders".

[![se um título não for especificado explicitamente, o nome de arquivo da página será usado automaticamente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image6.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image5.png)

**Figura 05**: se um título não for especificado explicitamente, o nome de arquivo da página será usado automaticamente ([clique para exibir a imagem em tamanho normal](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image7.png))

## <a name="step-3-basing-the-page-title-on-the-site-map"></a>Etapa 3: baseando o título da página no mapa do site

O ASP.NET oferece uma estrutura de mapa do site robusta que permite que os desenvolvedores de página definam um mapa de site hierárquico em um recurso externo (como um arquivo XML ou uma tabela de banco de dados) junto com controles da Web para exibir informações sobre o mapa do site (como SiteMapPath, Menu e controles TreeView).

A estrutura do mapa do site também pode ser acessada programaticamente por meio de uma classe code-behind da página ASP.NET. Dessa maneira, podemos definir automaticamente o título de uma página para o título de seu nó correspondente no mapa do site. Vamos aprimorar a classe de `BasePage` criada na etapa 2 para que ela ofereça essa funcionalidade. Mas primeiro precisamos criar um mapa do site para nosso site.

> [!NOTE]
> Este tutorial pressupõe que o leitor já esteja familiarizado com o ASP. Recursos do mapa do site da rede. Para obter mais informações sobre como usar o mapa do site, consulte minha série de artigos de várias partes, [examinando ASP. Navegação do site da rede](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx).

### <a name="creating-the-site-map"></a>Criando o mapa do site

O sistema de mapa do site é criado acima do [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que DISSOCIA a API do mapa do site da lógica que serializa informações de mapa do site entre a memória e um repositório persistente. O .NET Framework é fornecido com a [classe`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx), que é o provedor de mapa do site padrão. Como o nome indica, `XmlSiteMapProvider` usa um arquivo XML como seu repositório de mapa do site. Vamos usar esse provedor para definir nosso mapa de site.

Comece criando um arquivo de mapa do site na pasta raiz do site chamado `Web.sitemap`. Para fazer isso, clique com o botão direito do mouse no nome do site em Gerenciador de Soluções, escolha Adicionar novo item e selecione o modelo de mapa do site. Verifique se o arquivo é nomeado `Web.sitemap` e clique em Adicionar.

[![adicionar um arquivo chamado Web. sitemap à pasta raiz do site](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image9.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image8.png)

**Figura 06**: adicionar um arquivo chamado `Web.sitemap` à pasta raiz do site ([clique para exibir a imagem em tamanho normal](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image10.png))

Adicione o seguinte XML ao arquivo de `Web.sitemap`:

[!code-xml[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample9.xml)]

Esse XML define a estrutura hierárquica do mapa do site mostrada na Figura 7.

![O mapa do site é composto atualmente de três nós de mapa do site](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image11.png)

**Figura 07**: o mapa do site é composto atualmente de três nós de mapa do site

Atualizaremos a estrutura do mapa do site em Tutoriais futuros à medida que adicionarmos novos exemplos.

### <a name="updating-the-master-page-to-include-navigation-web-controls"></a>Atualizando a página mestra para incluir controles de navegação na Web

Agora que temos um mapa do site definido, vamos atualizar a página mestra para incluir controles de navegação na Web. Especificamente, vamos adicionar um controle ListView à coluna à esquerda na seção lições que renderiza uma lista não ordenada com um item de lista para cada nó definido no mapa do site.

> [!NOTE]
> O controle ListView é novo no ASP.NET versão 3,5. Se você estiver usando uma versão anterior do ASP.NET, use o controle Repeater em vez disso. Para obter mais informações sobre o controle ListView, consulte [usando os controles ListView e DataPager do ASP.net 3.5](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).

Comece removendo a marcação de lista não ordenada existente da seção lições. Em seguida, arraste um controle ListView da caixa de ferramentas e solte-o abaixo do título de lições. O ListView está localizado na seção de dados da caixa de ferramentas, junto com os outros controles de exibição: GridView, DetailsView e FormView. Defina a propriedade ID do ListView como `LessonsList`.

No assistente de configuração da fonte de dados, escolha associar o ListView a um novo controle SiteMapDataSource chamado `LessonsDataSource`. O controle SiteMapDataSource retorna a estrutura hierárquica do sistema de mapa do site.

[![associar um controle SiteMapDataSource ao controle de ListView da Liçõeslist](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image13.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image12.png)

**Figura 08**: associar um controle SiteMapDataSource ao controle `LessonsList` ListView ([clique para exibir a imagem em tamanho normal](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image14.png))

Depois de criar o controle SiteMapDataSource, precisamos definir os modelos de ListView para que ele processe uma lista não ordenada com um item de lista para cada nó retornado pelo controle SiteMapDataSource. Isso pode ser feito usando a seguinte marcação de modelo:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample10.aspx)]

O `LayoutTemplate` gera a marcação para uma lista não ordenada (`<ul>...</ul>`) enquanto o `ItemTemplate` renderiza cada item retornado por SiteMapDataSource como um item de lista (`<li>`) que contém um link para a lição específica.

Depois de configurar os modelos de ListView, visite o site. Como mostra a Figura 9, a seção de lições contém um único item com marcadores, página inicial. Onde estão as lições sobre e usando vários controles ContentPlaceHolder? O SiteMapDataSource é projetado para retornar um conjunto hierárquico de dados, mas o controle ListView só pode exibir um único nível da hierarquia. Consequentemente, somente o primeiro nível dos nós de mapa do site retornado por SiteMapDataSource é exibido.

[![seção de lições contém um único item de lista](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image16.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image15.png)

**Figura 09**: a seção de lições contém um único item de lista ([clique para exibir a imagem em tamanho normal](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image17.png))

Para exibir vários níveis, poderíamos aninhar vários ListViews dentro do `ItemTemplate`. Essa técnica foi examinada nas [ *páginas mestras e* no tutorial de navegação do site](../../data-access/introduction/master-pages-and-site-navigation-cs.md) da [série de tutoriais de trabalho com dados](../../data-access/index.md). No entanto, para esta série de tutoriais, nosso mapa de site conterá apenas dois níveis: Home (o nível superior); e cada lição como um filho de casa. Em vez de criar um ListView aninhado, podemos instruir o SiteMapDataSource a não retornar o nó inicial definindo sua [propriedade`ShowStartingNode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) como `false`. O efeito de rede é que o SiteMapDataSource começa retornando a segunda camada de nós de mapa do site.

Com essa alteração, o ListView exibe itens de marcador para as lições sobre e usando vários controles ContentPlaceHolder, mas omite um item de marcador para página inicial. Para corrigir isso, podemos adicionar explicitamente um item de marcador para Home no `LayoutTemplate`:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample11.aspx)]

Ao configurar o SiteMapDataSource para omitir o nó inicial e adicionar explicitamente um item de marcador doméstico, a seção lições exibe a saída pretendida.

[![seção de lições contém um item de marcador para página inicial e cada nó filho](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image19.png)](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image18.png)

**Figura 10**: a seção de lições contém um item de marcador para página inicial e cada nó filho ([clique para exibir a imagem em tamanho normal](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image20.png))

### <a name="setting-the-title-based-on-the-site-map"></a>Definindo o título com base no mapa do site

Com o mapa do site em vigor, podemos atualizar nossa classe `BasePage` para usar o título especificado no mapa do site. Como fizemos na etapa 2, só queremos usar o título do nó do mapa do site se o título da página não tiver sido explicitamente definido pelo desenvolvedor da página. Se a página que está sendo solicitada não tiver um título de página definido explicitamente e não for encontrada no mapa do site, voltaremos a usar o nome de arquivo da página solicitada (menos a extensão), como fizemos na etapa 2. A Figura 11 ilustra esse processo de decisão.

![Na ausência de um título de página definido explicitamente, o título do nó do mapa do site correspondente é usado](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image21.png)

**Figura 11**: na ausência de um título de página definido explicitamente, o título do nó do mapa do site correspondente é usado

Atualize o método `OnLoadComplete` da classe `BasePage` para incluir o código a seguir:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample12.cs)]

Como antes, o método `OnLoadComplete` começa determinando se o título da página foi definido explicitamente. Se `Page.Title` for `null`, uma cadeia de caracteres vazia ou for atribuído o valor "página sem título", o código atribuirá automaticamente um valor a `Page.Title`.

Para determinar o título a ser usado, o código começa referenciando a [propriedade`CurrentNode`](https://msdn.microsoft.com/library/system.web.sitemap.currentnode.aspx)da [classe`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx). `CurrentNode` retorna a instância de [`SiteMapNode`](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) no mapa do site que corresponde à página solicitada no momento. Supondo que a página solicitada no momento seja encontrada no mapa do site, a propriedade `Title` do `SiteMapNode`é atribuída ao título da página. Se a página solicitada no momento não estiver no mapa do site, `CurrentNode` retornará `null` e o nome de arquivo da página solicitada será usado como o título (como foi feito na etapa 2).

A Figura 12 mostra a página de `MultipleContentPlaceHolders.aspx` quando exibida por meio de um navegador. Como o título desta página não é definido explicitamente, o título do nó do mapa do site correspondente é usado.

![O título da página MultipleContentPlaceHolders. aspx é extraído do mapa do site](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/_static/image22.png)

**Figura 12**: o título da página `MultipleContentPlaceHolders.aspx` é extraído do mapa do site

## <a name="step-4-adding-other-page-specific-markup-to-theheadsection"></a>Etapa 4: adicionar outra marcação específica de página à seção`<head>`

As etapas 1, 2 e 3 observaram a personalização do elemento `<title>` de acordo com a página. Além de `<title>`, a seção `<head>` pode conter elementos de `<meta>` e elementos de `<link>`. Conforme observado anteriormente neste tutorial, `Site.master`seção `<head>` inclui um elemento `<link>` para `Styles.css`. Como esse elemento de `<link>` é definido na página mestra, ele é incluído na seção `<head>` para todas as páginas de conteúdo. Mas como vamos adicionar `<meta>` e `<link>` elementos de uma base página por página?

A maneira mais fácil de adicionar conteúdo específico à página na seção `<head>` é criando um controle ContentPlaceHolder na página mestra. Já temos esse ContentPlaceHolder (chamado `head`). Portanto, para adicionar marcação de `<head>` personalizada, crie um controle de conteúdo correspondente na página e coloque a marcação nela.

Para ilustrar como adicionar marcação de `<head>` personalizada a uma página, vamos incluir um elemento de descrição de `<meta>` para nosso conjunto atual de páginas de conteúdo. O elemento de descrição de `<meta>` fornece uma breve descrição sobre a página da Web; a maioria dos mecanismos de pesquisa incorpora essas informações em algum formato ao exibir os resultados da pesquisa.

Um elemento de descrição de `<meta>` tem o seguinte formato:

[!code-html[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample13.html)]

Para adicionar essa marcação a uma página de conteúdo, adicione o texto acima ao controle de conteúdo que mapeia para o ContentPlaceHolder de cabeçalho da página mestra. Por exemplo, para definir um elemento de descrição de `<meta>` para `Default.aspx`, adicione a seguinte marcação:

[!code-aspx[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample14.aspx)]

Como o ContentPlaceHolder de cabeçalho não está dentro do corpo da página HTML, a marcação adicionada ao controle de conteúdo não é exibida no modo de exibição de Design. Para ver o elemento de descrição de `<meta>`, visite `Default.aspx` por meio de um navegador. Depois que a página for carregada, exiba a origem e observe que a seção `<head>` inclui a marcação especificada no controle de conteúdo.

Reserve um tempo para adicionar `<meta>` elementos de descrição a `About.aspx`, `MultipleContentPlaceHolders.aspx`e `Login.aspx`.

### <a name="programmatically-adding-markup-to-theheadregion"></a>Adicionando marcação de forma programática à região de`<head>`

O ContentPlaceHolder de cabeçalho nos permite adicionar declarativamente uma marcação personalizada à região `<head>` da página mestra. A marcação personalizada também pode ser adicionada programaticamente. Lembre-se de que a propriedade `Header` da classe `Page` retorna a instância `HtmlHead` definida na página mestra (a `<head runat="server">`).

A capacidade de adicionar conteúdo ao `<head>` região de forma programática é útil quando o conteúdo a ser adicionado é dinâmico. Talvez seja baseado no usuário que visita a página; Talvez ele esteja sendo extraído de um banco de dados. Independentemente do motivo, você pode adicionar conteúdo ao `HtmlHead` adicionando controles à sua coleção de controles, desta forma:

[!code-csharp[Main](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs/samples/sample15.cs)]

O código acima adiciona o elemento de palavras-chave `<meta>` à região `<head>`, que fornece uma lista delimitada por vírgulas de palavras-chave que descrevem a página. Observe que para adicionar uma marcação de `<meta>` você cria uma instância de [`HtmlMeta`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmlmeta.aspx) , define suas propriedades `Name` e `Content` e, em seguida, adiciona-as à coleção `Header`do `Controls`. Da mesma forma, para adicionar programaticamente um elemento `<link>`, crie um objeto [`HtmlLink`](https://msdn.microsoft.com/library/system.web.ui.htmlcontrols.htmllink.aspx) , defina suas propriedades e, em seguida, adicione-o à coleção de `Controls` do `Header`.

> [!NOTE]
> Para adicionar uma marcação arbitrária, crie uma instância de [`LiteralControl`](https://msdn.microsoft.com/library/system.web.ui.literalcontrol.aspx) , defina sua propriedade `Text` e, em seguida, adicione-a à coleção de `Controls` do `Header`.

## <a name="summary"></a>Resumo

Neste tutorial, examinamos várias maneiras de adicionar `<head>` marcação de região de acordo com a página. Uma página mestra deve incluir uma instância de `HtmlHead` (`<head runat="server">`) com um ContentPlaceHolder. A instância de `HtmlHead` permite que as páginas de conteúdo acessem programaticamente a região de `<head>` e para definir de forma declarativa e programática o título da página; o controle ContentPlaceHolder permite que a marcação personalizada seja adicionada à seção `<head>` declarativamente por meio de um controle de conteúdo.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Definindo dinamicamente o título da página em ASP.NET](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Examinando ASP. Navegação do site da rede](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Como usar marcas meta de HTML](http://searchenginewatch.com/showPage.html?page=2167931)
- [Páginas mestras em ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Usando os controles ListView e DataPager do ASP.NET 3.5](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Usando uma classe base personalizada para as classes code-behind de suas páginas ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP. net e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 3,5 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott pode ser contatado em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Zack Jones e Banerjee. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](multiple-contentplaceholders-and-default-content-cs.md)
> [Próximo](urls-in-master-pages-cs.md)
