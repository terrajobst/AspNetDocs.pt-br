---
uid: web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
title: Criando um provedor de mapa de site baseado em banco de dados personalizado (VB) | Microsoft Docs
author: rick-anderson
description: O provedor de mapa do site padrão no ASP.NET 2,0 recupera seus dados de um arquivo XML estático. Embora o provedor baseado em XML seja adequado para muitos siz pequenos e médios...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: f904cd2c-a408-4484-9324-8b8d7fe33893
msc.legacyurl: /web-forms/overview/data-access/database-driven-site-maps/building-a-custom-database-driven-site-map-provider-vb
msc.type: authoredcontent
ms.openlocfilehash: 78051696bd75e1d574f55b1c5d5891fe67c3030d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78595364"
---
# <a name="building-a-custom-database-driven-site-map-provider-vb"></a>Criação de um provedor de mapa de site personalizado controlado por banco de dados (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_62_VB.zip) ou [baixar PDF](building-a-custom-database-driven-site-map-provider-vb/_static/datatutorial62vb1.pdf)

> O provedor de mapa do site padrão no ASP.NET 2,0 recupera seus dados de um arquivo XML estático. Embora o provedor baseado em XML seja adequado para muitos sites de pequeno e médio porte, aplicativos Web maiores exigem um mapa de site mais dinâmico. Neste tutorial, criaremos um provedor de mapa do site personalizado que recupera seus dados da camada de lógica de negócios, que, por sua vez, recupera os dados do Database.

## <a name="introduction"></a>Introdução

O recurso de mapa do site do ASP.NET 2,0 permite que um desenvolvedor de página defina um mapa do site do aplicativo Web em algum meio persistente, como em um arquivo XML. Uma vez definidas, os dados do mapa do site podem ser acessados programaticamente por meio da [classe`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx) no [namespace`System.Web`](https://msdn.microsoft.com/library/system.web.aspx) ou por meio de uma variedade de controles da Web de navegação, como os controles SiteMapPath, menu e TreeView. O sistema de mapa do site usa o [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx) para que diferentes implementações de serialização do mapa do site possam ser criadas e conectadas a um aplicativo Web. O provedor de mapa do site padrão que acompanha o ASP.NET 2,0 mantém a estrutura do mapa do site em um arquivo XML. De volta às [páginas mestras e](../introduction/master-pages-and-site-navigation-vb.md) ao tutorial de navegação do site, criamos um arquivo chamado `Web.sitemap` que continha essa estrutura e atualizamos seu XML com cada nova seção do tutorial.

O provedor de mapa do site baseado em XML padrão funcionará bem se a estrutura do mapa do site for razoavelmente estática, como para esses tutoriais. Em muitos cenários, no entanto, é necessário um mapa de site mais dinâmico. Considere o mapa do site mostrado na Figura 1, onde cada categoria e produto aparecerão como seções na estrutura do site s. Com esse mapa do site, a visita à página da Web correspondente ao nó raiz pode listar todas as categorias, enquanto que visitar uma página da Web de categoria específica listaria essa categoria de produtos e exibindo uma página da Web de um produto específico que mostraria os detalhes do produto.

[![as categorias e os produtos de composição da estrutura s do mapa do site](building-a-custom-database-driven-site-map-provider-vb/_static/image1.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image1.png)

**Figura 1**: as categorias e os produtos divisões da estrutura s do mapa do site ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-vb/_static/image2.png))

Embora essa estrutura baseada em categoria e em produto possa ser embutida em código no arquivo de `Web.sitemap`, o arquivo precisaria ser atualizado toda vez que uma categoria ou produto foi adicionado, removido ou renomeado. Consequentemente, a manutenção do mapa do site seria bastante simplificada se sua estrutura foi recuperada do banco de dados ou, idealmente, da camada de lógica de negócios da arquitetura do aplicativo. Dessa forma, como produtos e categorias foram adicionados, renomeados ou excluídos, o mapa do site será atualizado automaticamente para refletir essas alterações.

Como a serialização do mapa do site do ASP.NET 2,0 é criada acima do modelo do provedor, podemos criar nosso próprio provedor de mapa do site personalizado que coleta seus dados de um armazenamento de dados alternativo, como o banco de dado ou a arquitetura. Neste tutorial, criaremos um provedor personalizado que recupera seus dados da BLL. Vamos começar!

> [!NOTE]
> O provedor de mapa do site personalizado criado neste tutorial está rigidamente acoplado à arquitetura e ao modelo de dados do aplicativo. Jeff Prosise s [armazenando mapas de site no SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) e [o provedor de mapa do site do SQL que você estava esperando por](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx) artigos examina uma abordagem generalizada para armazenar dados de mapa do site em SQL Server.

## <a name="step-1-creating-the-custom-site-map-provider-web-pages"></a>Etapa 1: criando as páginas da Web do provedor de mapa do site personalizado

Antes de começarmos a criar um provedor de mapa do site personalizado, vamos primeiro adicionar as páginas ASP.NET necessárias para este tutorial. Comece adicionando uma nova pasta chamada `SiteMapProvider`. Em seguida, adicione as seguintes páginas ASP.NET a essa pasta, assegurando a associação de cada página com a página mestra `Site.master`:

- `Default.aspx`
- `ProductsByCategory.aspx`
- `ProductDetails.aspx`

Além disso, adicione uma subpasta `CustomProviders` à pasta `App_Code`.

![Adicionar as páginas ASP.NET para os tutoriais relacionados ao provedor do mapa do site](building-a-custom-database-driven-site-map-provider-vb/_static/image2.gif)

**Figura 2**: adicionar as páginas ASP.net para os tutoriais relacionados ao provedor do mapa do site

Como há apenas um tutorial para esta seção, não precisamos que `Default.aspx` liste os tutoriais da seção. Em vez disso, `Default.aspx` exibirá as categorias em um controle GridView. Abordaremos isso na etapa 2.

Em seguida, atualize `Web.sitemap` para incluir uma referência à página de `Default.aspx`. Especificamente, adicione a seguinte marcação após o `<siteMapNode>`de cache:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample1.xml)]

Depois de atualizar `Web.sitemap`, Reserve um momento para exibir o site de tutoriais por meio de um navegador. O menu à esquerda agora inclui um item para o único tutorial do provedor de mapa do site.

![O mapa do site agora inclui uma entrada para o tutorial do provedor do mapa do site](building-a-custom-database-driven-site-map-provider-vb/_static/image3.gif)

**Figura 3**: o mapa do site agora inclui uma entrada para o tutorial do provedor de mapa do site

O foco principal do tutorial é ilustrar a criação de um provedor de mapa do site personalizado e a configuração de um aplicativo Web para usar esse provedor. Em particular, criaremos um provedor que retorna um mapa do site que inclui um nó raiz junto com um nó para cada categoria e produto, conforme ilustrado na Figura 1. Em geral, cada nó no mapa do site pode especificar uma URL. Para nosso mapa de site, a URL de s do nó raiz será `~/SiteMapProvider/Default.aspx`, que listará todas as categorias no banco de dados. Cada nó de categoria no mapa do site terá uma URL que aponta para `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID`, que listará todos os produtos na *CategoryID*especificada. Por fim, cada nó do mapa do site do produto apontará para `~/SiteMapProvider/ProductDetails.aspx?ProductID=productID`, que exibirá os detalhes específicos do produto.

Para começar, precisamos criar as páginas `Default.aspx`, `ProductsByCategory.aspx`e `ProductDetails.aspx`. Essas páginas são concluídas nas etapas 2, 3 e 4, respectivamente. Como o ThrustMaster deste tutorial é sobre os provedores de mapa do site, e como os tutoriais anteriores abordaram a criação desses tipos de relatórios de mestre/detalhes de várias páginas, passaremos pelas etapas 2 a 4. Se você precisar de um atualizador sobre a criação de relatórios mestre/detalhados que abrangem várias páginas, consulte o tutorial [filtragem mestre/detalhes em duas páginas](../masterdetail/master-detail-filtering-across-two-pages-vb.md) .

## <a name="step-2-displaying-a-list-of-categories"></a>Etapa 2: exibindo uma lista de categorias

Abra a página `Default.aspx` na pasta `SiteMapProvider` e arraste um GridView da caixa de ferramentas para o designer, definindo seu `ID` como `Categories`. A partir da marca inteligente do GridView, associe-o a um novo ObjectDataSource chamado `CategoriesDataSource` e configure-o para que ele recupere seus dados usando o método `CategoriesBLL` Class s `GetCategories`. Como esse GridView apenas exibe as categorias e não fornece recursos de modificação de dados, defina as listas suspensas nas guias UPDATE, INSERT e DELETE como (None).

[![configurar o ObjectDataSource para retornar categorias usando o método GetCategories](building-a-custom-database-driven-site-map-provider-vb/_static/image4.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image3.png)

**Figura 4**: configurar o ObjectDataSource para retornar categorias usando o método `GetCategories` ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-vb/_static/image4.png))

[![definir as listas suspensas nas guias atualizar, inserir e excluir para (nenhum)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image5.png)

**Figura 5**: definir as listas suspensas nas guias atualizar, inserir e excluir para (nenhum) ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-vb/_static/image6.png))

Depois de concluir o assistente para configurar fonte de dados, o Visual Studio adicionará um BoundField para `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`e `BrochurePath`. Edite o GridView para que ele contenha apenas o `CategoryName` e `Description` BoundFields e atualize a propriedade `CategoryName` BoundField s `HeaderText` para categoria.

Em seguida, adicione um HyperLinkField e posicione-o para que ele seja o campo mais à esquerda. Defina a propriedade `DataNavigateUrlFields` como `CategoryID` e a propriedade `DataNavigateUrlFormatString` como `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID={0}`. Defina a propriedade `Text` para exibir produtos.

![Adicionar um HyperLinkField ao GridView das categorias](building-a-custom-database-driven-site-map-provider-vb/_static/image6.gif)

**Figura 6**: adicionar um HyperLinkField ao GridView do `Categories`

Depois de criar a ObjectDataSource e personalizar os campos de GridView, os dois controles marcação declarativa serão semelhantes ao seguinte:

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample2.aspx)]

A Figura 7 mostra `Default.aspx` quando visualizado por meio de um navegador. Clicar em um link categoria s exibir produtos leva você até `ProductsByCategory.aspx?CategoryID=categoryID`, que vamos criar na etapa 3.

[![cada categoria está listada junto com um link exibir produtos](building-a-custom-database-driven-site-map-provider-vb/_static/image7.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image7.png)

**Figura 7**: cada categoria é listada junto com um link exibir produtos ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-vb/_static/image8.png))

## <a name="step-3-listing-the-selected-category-s-products"></a>Etapa 3: listando os produtos da categoria selecionada

Abra a página `ProductsByCategory.aspx` e adicione um GridView, nomeando-o `ProductsByCategory`. A partir de sua marca inteligente, associe o GridView a um novo ObjectDataSource chamado `ProductsByCategoryDataSource`. Configure o ObjectDataSource para usar o método de `GetProductsByCategoryID(categoryID)` da classe `ProductsBLL` e defina as listas suspensas como (nenhum) nas guias atualizar, inserir e excluir.

[![usar o método ProductsBLL Class s GetProductsByCategoryID (categoryID)](building-a-custom-database-driven-site-map-provider-vb/_static/image8.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image9.png)

**Figura 8**: usar o método de `GetProductsByCategoryID(categoryID)` da classe `ProductsBLL` ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-vb/_static/image10.png))

A etapa final do assistente para configurar fonte de dados solicita uma origem de parâmetro para *CategoryID*. Como essas informações são passadas pelo campo QueryString `CategoryID`, selecione QueryString na lista suspensa e insira CategoryID na caixa de texto QueryStringfield, como mostra a Figura 9. Clique em Concluir para finalizar o assistente.

[![usar o campo CódigoDaCategoria QueryString para o parâmetro categoryID](building-a-custom-database-driven-site-map-provider-vb/_static/image9.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image11.png)

**Figura 9**: usar o campo `CategoryID` QueryString para o parâmetro *CategoryID* ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-vb/_static/image12.png))

Depois de concluir o assistente, o Visual Studio adicionará os BoundFields correspondentes e um CheckBoxField ao GridView dos campos de dados do produto. Remova tudo, exceto o `ProductName`, o `UnitPrice`e o `SupplierName` BoundFields. Personalize esses três BoundFields `HeaderText` Propriedades para ler produto, preço e fornecedor, respectivamente. Formate o `UnitPrice` BoundField como uma moeda.

Em seguida, adicione um HyperLinkField e mova-o para a posição mais à esquerda. Defina sua propriedade `Text` para exibir detalhes, sua propriedade `DataNavigateUrlFields` como `ProductID`e sua propriedade `DataNavigateUrlFormatString` como `~/SiteMapProvider/ProductDetails.aspx?ProductID={0}`.

![Adicione um HyperLinkField View Details que aponta para ProductDetails. aspx](building-a-custom-database-driven-site-map-provider-vb/_static/image10.gif)

**Figura 10**: adicionar um HyperLinkField de detalhes de exibição que aponta para `ProductDetails.aspx`

Depois de fazer essas personalizações, a marcação declarativa de GridView e ObjectDataSource s deve ser semelhante ao seguinte:

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample3.aspx)]

Retorne para exibir `Default.aspx` por meio de um navegador e clique no link exibir produtos para bebidas. Isso o levará para `ProductsByCategory.aspx?CategoryID=1`, exibindo os nomes, os preços e os fornecedores dos produtos no banco de dados Northwind que pertencem à categoria bebidas (consulte a Figura 11). Sinta-se à vontade para aprimorar ainda mais essa página para incluir um link para retornar usuários à página de listagem de categorias (`Default.aspx`) e um controle DetailsView ou FormView que exibe o nome e a descrição da categoria selecionada.

[![os nomes de bebidas, os preços e os fornecedores são exibidos](building-a-custom-database-driven-site-map-provider-vb/_static/image11.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image13.png)

**Figura 11**: os nomes de bebidas, os preços e os fornecedores são exibidos ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-vb/_static/image14.png))

## <a name="step-4-showing-a-product-s-details"></a>Etapa 4: mostrando os detalhes de um produto

A página final, `ProductDetails.aspx`, exibe os detalhes dos produtos selecionados. Abra `ProductDetails.aspx` e arraste um DetailsView da caixa de ferramentas para o designer. Defina a propriedade de `ID` de DetailsView s como `ProductInfo` e desmarque seus `Height` e `Width` valores de propriedade. A partir de sua marca inteligente, associe o DetailsView a um novo ObjectDataSource chamado `ProductDataSource`, configurando o ObjectDataSource para efetuar pull de seus dados do método `ProductsBLL` Class s `GetProductByProductID(productID)`. Assim como nas páginas da Web anteriores criadas nas etapas 2 e 3, defina as listas suspensas nas guias atualizar, inserir e excluir como (nenhum).

[![configurar o ObjectDataSource para usar o método GetProductByProductID (productID)](building-a-custom-database-driven-site-map-provider-vb/_static/image12.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.png)

**Figura 12**: configurar o ObjectDataSource para usar o método `GetProductByProductID(productID)` ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-vb/_static/image16.png))

A última etapa do assistente para configurar fonte de dados solicita a origem do parâmetro *ProductID* . Como esses dados são fornecidos pelo campo QueryString `ProductID`, defina a lista suspensa como QueryString e a caixa de texto QueryStringfield como ProductID. Por fim, clique no botão Concluir para concluir o assistente.

[![configurar o parâmetro productID para efetuar pull de seu valor do campo ProductID QueryString](building-a-custom-database-driven-site-map-provider-vb/_static/image13.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image17.png)

**Figura 13**: configurar o parâmetro *ProductID* para efetuar pull de seu valor do campo `ProductID` QueryString ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-vb/_static/image18.png))

Depois de concluir o assistente para configurar fonte de dados, o Visual Studio criará BoundFields correspondentes e um CheckBoxField no DetailsView para os campos de dados do produto. Remova as `ProductID`, `SupplierID`e `CategoryID` BoundFields e configure os campos restantes como você vê adequado. Depois de algumas configurações estética, minha marcação declarativa de DetailsView e ObjectDataSource s se parece com o seguinte:

[!code-aspx[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample4.aspx)]

Para testar esta página, retorne ao `Default.aspx` e clique em Exibir produtos para a categoria bebidas. Na lista de produtos de bebidas, clique no link exibir detalhes para o chá de Chai. Isso o levará para `ProductDetails.aspx?ProductID=1`, que mostra um número de detalhes de chá de Chai (consulte a Figura 14).

[![o fornecedor, a categoria, o preço e outras informações do Chai chá são exibidos](building-a-custom-database-driven-site-map-provider-vb/_static/image14.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image19.png)

**Figura 14**: fornecedor, categoria, preço e outras informações do Chai chá são exibidos ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-vb/_static/image20.png))

## <a name="step-5-understanding-the-inner-workings-of-a-site-map-provider"></a>Etapa 5: Noções básicas sobre o funcionamento interno de um provedor de mapa do site

O mapa do site é representado na memória do servidor Web como uma coleção de `SiteMapNode` instâncias que formam uma hierarquia. Deve haver exatamente uma raiz, todos os nós não raiz devem ter exatamente um nó pai, e todos os nós podem ter um número arbitrário de filhos. Cada objeto de `SiteMapNode` representa uma seção na estrutura do site s; Normalmente, essas seções têm uma página da Web correspondente. Consequentemente, a [classe`SiteMapNode`](https://msdn.microsoft.com/library/system.web.sitemapnode.aspx) tem propriedades como `Title`, `Url`e `Description`, que fornecem informações para a seção que o `SiteMapNode` representa. Também há uma propriedade `Key` que identifica exclusivamente cada `SiteMapNode` na hierarquia, bem como propriedades usadas para estabelecer essa hierarquia `ChildNodes`, `ParentNode`, `NextSibling`, `PreviousSibling`e assim por diante.

A Figura 15 mostra a estrutura geral do mapa do site da Figura 1, mas com os detalhes da implementação esboçados com mais detalhes.

[![cada SiteMapNode tem propriedades como title, URL, Key e assim por diante](building-a-custom-database-driven-site-map-provider-vb/_static/image16.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image15.gif)

**Figura 15**: cada `SiteMapNode` tem propriedades como `Title`, `Url`, `Key`e assim por diante ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-vb/_static/image17.gif))

O mapa do site pode ser acessado por meio da [classe`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx) no [namespace`System.Web`](https://msdn.microsoft.com/library/system.web.aspx). Esta propriedade s `RootNode` de classe retorna a instância de `SiteMapNode` raiz do mapa do site; `CurrentNode` retorna a `SiteMapNode` cuja propriedade `Url` corresponde à URL da página solicitada no momento. Essa classe é usada internamente por controles da Web de navegação ASP.NET 2,0 s.

Quando as propriedades de classe s `SiteMap` são acessadas, ela deve serializar a estrutura do mapa do site de um meio persistente na memória. No entanto, a lógica de serialização do mapa do site não está embutida em código na classe `SiteMap`. Em vez disso, no tempo de execução, a classe `SiteMap` determina qual *provedor* de mapa do site usar para serialização. Por padrão, a [classe`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx) é usada, que lê a estrutura do mapa do site de um arquivo XML formatado corretamente. No entanto, com um pouco de trabalho, podemos criar nosso próprio provedor personalizado de mapa do site.

Todos os provedores de mapa do site devem ser derivados da [classe`SiteMapProvider`](https://msdn.microsoft.com/library/system.web.sitemapprovider.aspx), que inclui os métodos essenciais e as propriedades necessárias para os provedores do mapa do site, mas omite muitos dos detalhes da implementação. Uma segunda classe, [`StaticSiteMapProvider`](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.aspx), estende a classe `SiteMapProvider` e contém uma implementação mais robusta da funcionalidade necessária. Internamente, o `StaticSiteMapProvider` armazena as instâncias de `SiteMapNode` do mapa do site em uma `Hashtable` e fornece métodos como `AddNode(child, parent)`, `RemoveNode(siteMapNode),` e `Clear()` que adicionam e removem `SiteMapNode` s para o `Hashtable`interno. `XmlSiteMapProvider` é derivado de `StaticSiteMapProvider`.

Ao criar um provedor de mapa do site personalizado que estenda `StaticSiteMapProvider`, há dois métodos abstratos que devem ser substituídos: [`BuildSiteMap`](https://msdn.microsoft.com/library/system.web.staticsitemapprovider.buildsitemap.aspx) e [`GetRootNodeCore`](https://msdn.microsoft.com/library/system.web.sitemapprovider.getrootnodecore.aspx). `BuildSiteMap`, como o nome indica, é responsável por carregar a estrutura do mapa do site do armazenamento persistente e construí-la na memória. `GetRootNodeCore` retorna o nó raiz no mapa do site.

Antes que um aplicativo Web possa usar um provedor de mapa do site, ele deve ser registrado na configuração do aplicativo. Por padrão, a classe `XmlSiteMapProvider` é registrada usando o nome `AspNetXmlSiteMapProvider`. Para registrar provedores de mapa do site adicionais, adicione a seguinte marcação a `Web.config`:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample5.xml)]

O valor de *nome* atribui um nome legível ao provedor enquanto o *tipo* especifica o nome de tipo totalmente qualificado do provedor de mapa do site. Vamos explorar os valores concretos para os valores de *nome* e *tipo* na etapa 7, depois de criarmos nosso provedor de mapa do site personalizado.

A classe de provedor do mapa do site é instanciada na primeira vez que é acessada da classe `SiteMap` e permanece na memória durante o tempo de vida do aplicativo Web. Como há apenas uma instância do provedor de mapa do site que pode ser invocada de vários visitantes simultâneos do site, é imperativo que os métodos s do provedor sejam *thread-safe*.

Por motivos de desempenho e escalabilidade, é importante que armazenamos em cache a estrutura do mapa do site na memória e retornamos essa estrutura armazenada em cache em vez de recriá-la sempre que o método `BuildSiteMap` for invocado. `BuildSiteMap` pode ser chamado várias vezes por solicitação de página por usuário, dependendo dos controles de navegação em uso na página e da profundidade da estrutura do mapa do site. Em qualquer caso, se não armazenarmos em cache a estrutura do mapa do site em `BuildSiteMap`, sempre que for invocado, precisaremos recuperar novamente as informações de produto e categoria da arquitetura (o que resultaria em uma consulta para o banco de dados). Como discutimos nos tutoriais de cache anteriores, os dados armazenados em cache podem se tornar obsoletos. Para combater isso, podemos usar expirações baseadas em dependência de cache de tempo ou SQL.

> [!NOTE]
> Um provedor de mapa do site pode, opcionalmente, substituir o [método`Initialize`](https://msdn.microsoft.com/library/system.web.sitemapprovider.initialize.aspx). `Initialize` é invocado quando o provedor de mapa do site é instanciado pela primeira vez e recebe todos os atributos personalizados atribuídos ao provedor em `Web.config` no elemento `<add>`, como: `<add name="name" type="type" customAttribute="value" />`. É útil se você quiser permitir que um desenvolvedor de página especifique várias configurações relacionadas ao provedor do mapa do site sem precisar modificar o código do provedor. Por exemplo, se estivéssemos lendo os dados de categoria e de produtos diretamente do banco de dado em oposição à arquitetura, nós d provavelmente desejam deixar o desenvolvedor da página especificar a cadeia de conexão do banco de dados por meio de `Web.config` em vez de usar um valor codificado no código s do provedor. O provedor de mapa do site personalizado que criaremos na etapa 6 não substitui esse `Initialize` método. Para obter um exemplo de como usar o método `Initialize`, consulte [Jeff Prosise](http://www.wintellect.com/Weblogs/CategoryView,category,Jeff%20Prosise.aspx) s [armazenando mapas de Site no artigo SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) .

## <a name="step-6-creating-the-custom-site-map-provider"></a>Etapa 6: criando o provedor de mapa do site personalizado

Para criar um provedor de mapa do site personalizado que cria o mapa do site de categorias e produtos no banco de dados Northwind, precisamos criar uma classe que estenda `StaticSiteMapProvider`. Na etapa 1, pedia que você adicionasse uma pasta `CustomProviders` na pasta `App_Code` – adicione uma nova classe a essa pasta denominada `NorthwindSiteMapProvider`. Adicione o código a seguir à classe `NorthwindSiteMapProvider`:

[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample6.vb)]

Vamos começar com a exploração desta classe s `BuildSiteMap` método, que começa com uma [instrução`lock`](https://msdn.microsoft.com/library/c5kehkcz.aspx). A instrução `lock` permite apenas um thread por vez para entrar, serializando, assim, o acesso ao seu código e impedindo que dois threads simultâneos percorrendo em um outro s dedos.

A variável de `SiteMapNode` no nível de classe `root` é usada para armazenar em cache a estrutura do mapa do site. Quando o mapa do site é construído pela primeira vez ou pela primeira vez após a modificação dos dados subjacentes, `root` será `Nothing` e a estrutura do mapa do site será construída. O nó raiz do mapa do site é atribuído a `root` durante o processo de construção, de modo que, na próxima vez que esse método for chamado, `root` não será `Nothing`. Consequentemente, desde que `root` não seja `Nothing` a estrutura do mapa do site será retornada ao chamador sem precisar recriá-lo.

Se a raiz for `Nothing`, a estrutura do mapa do site será criada a partir das informações de produto e categoria. O mapa do site é criado criando-se as instâncias de `SiteMapNode` e, em seguida, formando a hierarquia por meio de chamadas para o método de `AddNode` da classe `StaticSiteMapProvider`. `AddNode` executa a escrituração interna, armazenando as instâncias de `SiteMapNode` classificadas em uma `Hashtable`. Antes de começarmos a construir a hierarquia, começamos chamando o método `Clear`, que limpa os elementos da `Hashtable`interna. Em seguida, o método `GetProducts` da classe `ProductsBLL` e os `ProductsDataTable` resultantes são armazenados em variáveis locais.

A construção do mapa do site começa criando o nó raiz e atribuindo-o a `root`. A sobrecarga do [Construtor de`SiteMapNode` s](https://msdn.microsoft.com/library/system.web.sitemapnode.sitemapnode.aspx) usado aqui e em todo esse `BuildSiteMap` recebe as seguintes informações:

- Uma referência ao provedor de mapa do site (`Me`).
- O `SiteMapNode` s `Key`. Esse valor necessário deve ser exclusivo para cada `SiteMapNode`.
- O `SiteMapNode` s `Url`. `Url` é opcional, mas se fornecida, cada `SiteMapNode` `Url` valor deve ser exclusivo.
- O `SiteMapNode` s `Title`, que é necessário.

A chamada do método `AddNode(root)` adiciona o `SiteMapNode` `root` ao mapa do site como a raiz. Em seguida, cada `ProductRow` na `ProductsDataTable` é enumerada. Se já existir um `SiteMapNode` para a categoria do produto atual, ele será referenciado. Caso contrário, um novo `SiteMapNode` para a categoria é criado e adicionado como um filho do `SiteMapNode``root` por meio da chamada do método `AddNode(categoryNode, root)`. Depois que a categoria apropriada `SiteMapNode` nó foi encontrado ou criado, um `SiteMapNode` é criado para o produto atual e adicionado como um filho da categoria `SiteMapNode` por meio de `AddNode(productNode, categoryNode)`. Observe que a categoria `SiteMapNode` `Url` valor da propriedade é `~/SiteMapProvider/ProductsByCategory.aspx?CategoryID=categoryID` enquanto a propriedade `Url` do produto `SiteMapNode` é atribuída `~/SiteMapNode/ProductDetails.aspx?ProductID=productID`.

> [!NOTE]
> Os produtos que têm um banco de dados `NULL` valor para seus `CategoryID` são agrupados em uma categoria `SiteMapNode` cuja propriedade `Title` está definida como None e cuja propriedade `Url` está definida como uma cadeia de caracteres vazia. Decidi definir `Url` como uma cadeia de caracteres vazia, já que a classe `ProductBLL` `GetProductsByCategory(categoryID)` método atualmente não tem a capacidade de retornar apenas esses produtos com um valor de `CategoryID` `NULL`. Além disso, eu queria demonstrar como os controles de navegação renderizam um `SiteMapNode` que não tem um valor para sua propriedade `Url`. Recomendo que você estenda este tutorial para que a propriedade None `SiteMapNode` s `Url` aponte para `ProductsByCategory.aspx`, mas só exibirá os produtos com `NULL` `CategoryID` valores.

Depois de construir o mapa do site, um objeto arbitrário é adicionado ao cache de dados usando uma dependência de cache do SQL nas tabelas `Categories` e `Products` por meio de um objeto `AggregateCacheDependency`. Exploramos o uso das dependências do cache do SQL no tutorial anterior, *usando dependências do cache SQL*. O provedor de mapa do site personalizado, no entanto, usa uma sobrecarga do método `Insert` do cache de dados que ainda exploramos. Essa sobrecarga aceita como seu parâmetro de entrada final um delegado que é chamado quando o objeto é removido do cache. Especificamente, passamos um novo [delegado`CacheItemRemovedCallback`](https://msdn.microsoft.com/library/system.web.caching.cacheitemremovedcallback.aspx) que aponta para o método `OnSiteMapChanged` definido mais abaixo na classe `NorthwindSiteMapProvider`.

> [!NOTE]
> A representação na memória do mapa do site é armazenada em cache por meio da variável de nível de classe `root`. Como há apenas uma instância da classe de provedor do mapa do site personalizado e como essa instância é compartilhada entre todos os threads no aplicativo Web, essa variável de classe serve como um cache. O método `BuildSiteMap` também usa o cache de dados, mas apenas como um meio de receber notificação quando os dados de banco de dados subjacentes nas tabelas `Categories` ou `Products` são alterados. Observe que o valor colocado no cache de dados é apenas a data e hora atuais. Os dados reais do mapa do site *não* são colocados no cache de dados.

O método `BuildSiteMap` é concluído retornando o nó raiz do mapa do site.

Os métodos restantes são razoavelmente simples. `GetRootNodeCore` é responsável por retornar o nó raiz. Como `BuildSiteMap` retorna a raiz, `GetRootNodeCore` simplesmente retorna `BuildSiteMap` s valor de retorno. O método `OnSiteMapChanged` define `root` de volta para `Nothing` quando o item de cache é removido. Com a raiz definida de volta para `Nothing`, na próxima vez que `BuildSiteMap` for invocado, a estrutura do mapa do site será recriada. Por fim, a propriedade `CachedDate` retorna o valor de data e hora armazenado no cache de dados, se esse valor existir. Essa propriedade pode ser usada por um desenvolvedor de página para determinar quando os dados do mapa do site foram armazenados no último cache.

## <a name="step-7-registering-thenorthwindsitemapprovider"></a>Etapa 7: registrando o`NorthwindSiteMapProvider`

Para que nosso aplicativo Web Use o provedor de mapa do site `NorthwindSiteMapProvider` criado na etapa 6, precisamos registrá-lo na seção `<siteMap>` do `Web.config`. Especificamente, adicione a seguinte marcação dentro do elemento `<system.web>` no `Web.config`:

[!code-xml[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample7.xml)]

Essa marcação faz duas coisas: primeiro, ela indica que o `AspNetXmlSiteMapProvider` interno é o provedor de mapa do site padrão; em segundo lugar, ele registra o provedor de mapa do site personalizado criado na etapa 6 com o nome amigável da Northwind.

> [!NOTE]
> Para provedores de mapa do site localizados na pasta s `App_Code` do aplicativo, o valor do atributo `type` é simplesmente o nome da classe. Como alternativa, o provedor de mapa do site personalizado poderia ter sido criado em um projeto de biblioteca de classes separado com o assembly compilado colocado no diretório do aplicativo Web s `/Bin`. Nesse caso, o valor do atributo `type` seria *namespace*. *ClassName*, *AssemblyName* .

Depois de atualizar `Web.config`, Reserve um momento para exibir qualquer página dos tutoriais em um navegador. Observe que a interface de navegação à esquerda ainda mostra as seções e os tutoriais definidos em `Web.sitemap`. Isso ocorre porque deixamos `AspNetXmlSiteMapProvider` como o provedor padrão. Para criar um elemento de interface do usuário de navegação que usa o `NorthwindSiteMapProvider`, precisaremos especificar explicitamente que o provedor de mapa do site da Northwind deve ser usado. Veremos como fazer isso na etapa 8.

## <a name="step-8-displaying-site-map-information-using-the-custom-site-map-provider"></a>Etapa 8: exibindo informações de mapa do site usando o provedor de mapa do site personalizado

Com o provedor de mapa do site personalizado criado e registrado no `Web.config`, estamos prontos para adicionar controles de navegação às páginas `Default.aspx`, `ProductsByCategory.aspx`e `ProductDetails.aspx` na pasta `SiteMapProvider`. Comece abrindo a página `Default.aspx` e arraste uma `SiteMapPath` da caixa de ferramentas para o designer. O controle SiteMapPath está localizado na seção de navegação da caixa de ferramentas.

[![adicionar um SiteMapPath a default. aspx](building-a-custom-database-driven-site-map-provider-vb/_static/image19.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image18.gif)

**Figura 16**: adicionar um SiteMapPath a `Default.aspx` ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-vb/_static/image20.gif))

O controle SiteMapPath exibe um breadcrumb, indicando o local de s da página atual no mapa do site. Adicionamos um SiteMapPath à parte superior da página mestra de volta nas *páginas mestras e* no tutorial de navegação do site.

Reserve um tempo para exibir esta página por meio de um navegador. O SiteMapPath adicionado na Figura 16 usa o provedor de mapa do site padrão, extraindo seus dados de `Web.sitemap`. Portanto, o breadcrumb mostra Home &gt; Personalizando o mapa do site, assim como o breadcrumb no canto superior direito.

[![o breadcrumb usa o provedor de mapa do site padrão](building-a-custom-database-driven-site-map-provider-vb/_static/image22.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.gif)

**Figura 17**: o breadcrumb usa o provedor de mapa do site padrão ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-vb/_static/image23.gif))

Para que o SiteMapPath adicionado na Figura 16 use o provedor de mapa do site personalizado criado na etapa 6, defina sua [propriedade`SiteMapProvider`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemappath.sitemapprovider.aspx) como Northwind, o nome atribuído à `NorthwindSiteMapProvider` no `Web.config`. Infelizmente, o designer continua a usar o provedor de mapa do site padrão, mas se você visitar a página por meio de um navegador depois de fazer essa alteração de propriedade, verá que o breadcrumb agora usa o provedor de mapa do site personalizado.

[![o breadcrumb agora usa o provedor de mapa do site personalizado NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-vb/_static/image25.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image24.gif)

**Figura 18**: o breadcrumb agora usa o provedor de mapa do Site personalizado `NorthwindSiteMapProvider` ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-vb/_static/image26.gif))

O controle SiteMapPath exibe uma interface do usuário mais funcional nas páginas `ProductsByCategory.aspx` e `ProductDetails.aspx`. Adicione um SiteMapPath a essas páginas, definindo a propriedade `SiteMapProvider` em ambos como Northwind. Em `Default.aspx` clique no link exibir produtos para bebidas e, em seguida, no link exibir detalhes para o chá de Chai. Como mostra a Figura 19, o breadcrumb inclui a seção atual do mapa do site (Chai chá) e seus ancestrais: bebidas e todas as categorias.

[![o breadcrumb agora usa o provedor de mapa do site personalizado NorthwindSiteMapProvider](building-a-custom-database-driven-site-map-provider-vb/_static/image27.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image21.png)

**Figura 19**: o breadcrumb agora usa o provedor de mapa do Site personalizado `NorthwindSiteMapProvider` ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-vb/_static/image22.png))

Outros elementos da interface do usuário de navegação podem ser usados além do SiteMapPath, como os controles menu e TreeView. As páginas `Default.aspx`, `ProductsByCategory.aspx`e `ProductDetails.aspx` no download deste tutorial, por exemplo, todos os controles de menu incluem (consulte a figura 20). Consulte [examinando os recursos de navegação do site do ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx) e a seção [usando controles de navegação do site](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/navigation/sitenavcontrols.aspx) do guia de [início rápido do ASP.NET 2,0](https://quickstarts.asp.net/QuickStartv20/aspnet/) para obter uma visão mais detalhada dos controles de navegação e do sistema de mapa do site no ASP.NET 2,0.

[![o controle menu lista cada uma das categorias e produtos](building-a-custom-database-driven-site-map-provider-vb/_static/image29.gif)](building-a-custom-database-driven-site-map-provider-vb/_static/image28.gif)

**Figura 20**: o controle menu lista cada uma das categorias e produtos ([clique para exibir a imagem em tamanho normal](building-a-custom-database-driven-site-map-provider-vb/_static/image30.gif))

Como mencionado anteriormente neste tutorial, a estrutura do mapa do site pode ser acessada programaticamente por meio da classe `SiteMap`. O código a seguir retorna o `SiteMapNode` raiz do provedor padrão:

[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample8.vb)]

Como o `AspNetXmlSiteMapProvider` é o provedor padrão para nosso aplicativo, o código acima retornaria o nó raiz definido em `Web.sitemap`. Para fazer referência a um provedor de mapa do site diferente do padrão, use a [propriedade](https://msdn.microsoft.com/library/system.web.sitemap.providers.aspx) `SiteMap` classe s`Providers` como a seguinte:

[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample9.vb)]

Em que *Name* é o nome do provedor de mapa do site personalizado (Northwind, para nosso aplicativo Web).

Para acessar um membro específico de um provedor de mapa do site, use `SiteMap.Providers["name"]` para recuperar a instância do provedor e, em seguida, converta-a no tipo apropriado. Por exemplo, para exibir a propriedade `NorthwindSiteMapProvider` s `CachedDate` em uma página ASP.NET, use o seguinte código:

[!code-vb[Main](building-a-custom-database-driven-site-map-provider-vb/samples/sample10.vb)]

> [!NOTE]
> Certifique-se de testar o recurso de dependência de cache do SQL. Depois de visitar as páginas `Default.aspx`, `ProductsByCategory.aspx`e `ProductDetails.aspx`, vá para um dos tutoriais na seção edição, inserção e exclusão e edite o nome de uma categoria ou produto. Em seguida, retorne a uma das páginas na pasta `SiteMapProvider`. Supondo que o tempo suficiente tenha passado para que o mecanismo de sondagem Observe a alteração no banco de dados subjacente, o mapa do site deve ser atualizado para mostrar o novo nome do produto ou da categoria.

## <a name="summary"></a>Resumo

Os recursos do mapa do site do ASP.NET 2,0 incluem uma classe `SiteMap`, um número de controles da Web de navegação internos e um provedor de mapa do site padrão que espera que as informações do mapa do site sejam mantidas em um arquivo XML. Para usar informações de mapa do site de alguma outra fonte, como de um banco de dados, da arquitetura do aplicativo ou de um serviço Web remoto, precisamos criar um provedor de mapa do site personalizado. Isso envolve a criação de uma classe que deriva, direta ou indiretamente, da classe `SiteMapProvider`.

Neste tutorial, vimos como criar um provedor de mapa do site personalizado com base no mapa do site nas informações de produto e categoria, examinadas na arquitetura do aplicativo. Nosso provedor estendeu a classe `StaticSiteMapProvider` e envolveu a criação de um método `BuildSiteMap` que recuperou os dados, construiu a hierarquia do mapa do site e armazenou em cache a estrutura resultante em uma variável em nível de classe. Usamos uma dependência de cache do SQL com uma função de retorno de chamada para invalidar a estrutura armazenada em cache quando os dados subjacentes de `Categories` ou `Products` são modificados.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Armazenando mapas de site no SQL Server](https://msdn.microsoft.com/msdnmag/issues/05/06/WickedCode/) e [no provedor de mapa do site do SQL que você estava esperando](https://msdn.microsoft.com/msdnmag/issues/06/02/wickedcode/default.aspx)
- [Uma visão do modelo de provedor ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)
- [O kit de ferramentas do provedor](https://msdn.microsoft.com/asp.net/aa336558.aspx)
- [Examinando os recursos de navegação do site do ASP.NET 2,0 s](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Dave Gardner, Zack Jones, Teresa Murphy e Bernadette Leigh. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](building-a-custom-database-driven-site-map-provider-cs.md)
