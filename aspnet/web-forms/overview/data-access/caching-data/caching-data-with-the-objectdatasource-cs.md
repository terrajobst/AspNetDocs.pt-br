---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
title: Armazenando dados em cache comC#o ObjectDataSource () | Microsoft Docs
author: rick-anderson
description: O Caching pode significar a diferença entre um aplicativo Web lento e rápido. Este tutorial é o primeiro de quatro que faz uma visão detalhada do cache em ASP.NET...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: bd87413c-8160-4520-a8a2-43b555c4183a
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: c9883314d6153b9816d9bad2a281ab3c0a816448
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74612627"
---
# <a name="caching-data-with-the-objectdatasource-c"></a>Armazenar dados em cache com o ObjectDataSource (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_CS.exe) ou [baixar PDF](caching-data-with-the-objectdatasource-cs/_static/datatutorial58cs1.pdf)

> O Caching pode significar a diferença entre um aplicativo Web lento e rápido. Este tutorial é o primeiro de quatro que faz uma visão detalhada do cache em ASP.NET. Aprenda os principais conceitos de cache e como aplicar o Caching à camada de apresentação por meio do controle ObjectDataSource.

## <a name="introduction"></a>Introdução

Na ciência da computação, o *Caching* é o processo de obtenção de dados ou informações que são caras para obter e armazenar uma cópia dela em um local mais rápido de acessar. Para aplicativos controlados por dados, consultas grandes e complexas normalmente consomem a maioria do tempo de execução do aplicativo. Esse tipo de desempenho de aplicativo, em seguida, pode ser melhorado com freqüência armazenando os resultados de consultas dispendiosas de banco de dados na memória s do aplicativo.

O ASP.NET 2,0 oferece uma variedade de opções de cache. Uma página da Web inteira ou a marcação renderizada s de controle de usuário pode ser armazenada em cache por meio do *cache de saída*. Os controles ObjectDataSource e SqlDataSource fornecem recursos de cache, permitindo assim que os dados sejam armazenados em cache no nível de controle. E o *cache de dados* do ASP.NET fornece uma API de cache rica que permite que os desenvolvedores de páginas armazenem em cache os objetos de forma programática. Neste tutorial e nos próximos três, examinaremos usando os recursos de cache do ObjectDataSource s, bem como o cache de dados. Também exploraremos como armazenar em cache os dados de todo o aplicativo na inicialização e como manter os dados em cache atualizados por meio do uso de dependências de cache do SQL. Esses tutoriais não exploram o cache de saída. Para obter uma visão detalhada do cache de saída, consulte [Caching de saída no ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

O Caching pode ser aplicado em qualquer lugar da arquitetura, desde a camada de acesso a dados até a camada de apresentação. Neste tutorial, veremos como aplicar o cache à camada de apresentação por meio do controle ObjectDataSource. No próximo tutorial, examinaremos o cache de dados na camada de lógica de negócios.

## <a name="key-caching-concepts"></a>Conceitos de cache de chave

O Caching pode melhorar muito o desempenho e a escalabilidade gerais de um aplicativo, obtendo dados caros para gerar e armazenar uma cópia dele em um local que pode ser acessado com mais eficiência. Como o cache mantém apenas uma cópia dos dados subjacentes reais, ele poderá ficar desatualizado ou *obsoleto*se os dados subjacentes forem alterados. Para combater isso, um desenvolvedor de página pode indicar critérios pelos quais o item de cache será *removido* do cache, usando:

- **Critérios baseados em tempo** um item pode ser adicionado ao cache por uma duração absoluta ou deslizante. Por exemplo, um desenvolvedor de página pode indicar uma duração de, digamos, 60 segundos. Com uma duração absoluta, o item armazenado em cache é removido 60 segundos depois de ser adicionado ao cache, independentemente da frequência com que ele foi acessado. Com uma duração deslizante, o item armazenado em cache é removido 60 segundos após o último acesso.
- **Critérios baseados em dependência** uma dependência pode ser associada a um item quando adicionado ao cache. Quando a dependência do item s é alterada, ela é removida do cache. A dependência pode ser um arquivo, outro item de cache ou uma combinação dos dois. O ASP.NET 2,0 também permite as dependências de cache do SQL, que permitem aos desenvolvedores adicionar um item ao cache e removê-lo quando os dados subjacentes do banco de dados forem alterados. Examinaremos as dependências do cache SQL no futuro usando o tutorial de [dependências do cache SQL](using-sql-cache-dependencies-cs.md) .

Independentemente dos critérios de remoção especificados, um item no cache pode ser *eliminado* antes que os critérios baseados no tempo ou na dependência sejam atendidos. Se o cache atingiu sua capacidade, os itens existentes devem ser removidos antes que novos possam ser adicionados. Consequentemente, ao trabalhar de forma programática com dados armazenados em cache, é vital que você sempre presuma que os dados armazenados em cache podem não estar presentes. Examinaremos o padrão a ser usado ao acessar dados do cache programaticamente em nosso próximo tutorial, *armazenando em cache os dados na arquitetura*.

O caching fornece um meio econômico para comprimir mais desempenho de um aplicativo. Como [Steven Smith](http://aspadvice.com/blogs/ssmith/) articula em seu artigo [ASP.net Caching: técnicas e práticas recomendadas](https://msdn.microsoft.com/library/aa478965.aspx):

O Caching pode ser uma boa maneira de obter um bom desempenho suficiente sem exigir muito tempo e análise. A memória é barata, portanto, se você puder obter o desempenho de que precisa armazenando em cache a saída por 30 segundos, em vez de gastar um dia ou uma semana tentando otimizar seu código ou banco de dados, faça a solução de cache (supondo que o dado de 30 segundos esteja OK) e passe. Por fim, um design ruim provavelmente será feito para você, portanto, é claro que você deve tentar projetar seus aplicativos corretamente. Mas se você só precisa obter um desempenho suficiente hoje, o Caching pode ser uma excelente [abordagem], comprando tempo para refatorar seu aplicativo posteriormente quando você tiver tempo para fazer isso.

Embora o Caching possa fornecer aprimoramentos de desempenho de apreciável, ele não é aplicável em todas as situações, como com aplicativos que usam dados em tempo real, atualizando com frequência ou onde mesmo os dados obsoletos em breve duração são inaceitáveis. Mas, para a maioria dos aplicativos, o Caching deve ser usado. Para obter mais informações sobre cache no ASP.NET 2,0, consulte a seção [cache for performance](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) dos [tutoriais de início rápido do ASP.NET 2,0](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Etapa 1: criando as páginas da Web de cache

Antes de começarmos nossa exploração dos recursos de cache de ObjectDataSource s, vamos primeiro reservar um momento para criar as páginas ASP.NET em nosso projeto de site, que precisaremos para este tutorial e os três seguintes. Comece adicionando uma nova pasta chamada `Caching`. Em seguida, adicione as seguintes páginas ASP.NET a essa pasta, assegurando a associação de cada página com a página mestra `Site.master`:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`

![Adicionar as páginas ASP.NET para os tutoriais relacionados ao cache](caching-data-with-the-objectdatasource-cs/_static/image1.png)

**Figura 1**: adicionar as páginas ASP.net para os tutoriais relacionados ao cache

Assim como nas outras pastas, `Default.aspx` na pasta `Caching` listará os tutoriais em sua seção. Lembre-se de que o controle de usuário `SectionLevelTutorialListing.ascx` fornece essa funcionalidade. Portanto, adicione esse controle de usuário a `Default.aspx` arrastando-o da Gerenciador de Soluções para a página s modo de exibição de Design.

[![Figura 2: Adicionar o controle de usuário SectionLevelTutorialListing. ascx a default. aspx](caching-data-with-the-objectdatasource-cs/_static/image3.png)](caching-data-with-the-objectdatasource-cs/_static/image2.png)

**Figura 2**: Adicionar o controle de usuário `SectionLevelTutorialListing.ascx` ao `Default.aspx` ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image4.png))

Por fim, adicione essas páginas como entradas ao arquivo de `Web.sitemap`. Especificamente, adicione a seguinte marcação após o trabalho com dados binários `<siteMapNode>`:

[!code-xml[Main](caching-data-with-the-objectdatasource-cs/samples/sample1.xml)]

Depois de atualizar `Web.sitemap`, Reserve um momento para exibir o site de tutoriais por meio de um navegador. O menu à esquerda agora inclui itens para os tutoriais de cache.

![O mapa do site agora inclui entradas para os tutoriais de cache](caching-data-with-the-objectdatasource-cs/_static/image5.png)

**Figura 3**: o mapa do site agora inclui entradas para os tutoriais de cache

## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Etapa 2: exibindo uma lista de produtos em uma página da Web

Este tutorial explora como usar os recursos de cache interno do controle ObjectDataSource s. Antes que possamos examinar esses recursos, no entanto, precisamos primeiro de uma página para trabalhar. Deixe que o s Crie uma página da Web que usa um GridView para listar informações de produtos recuperadas por um ObjectDataSource da classe `ProductsBLL`.

Comece abrindo a página de `ObjectDataSource.aspx` na pasta `Caching`. Arraste um GridView da caixa de ferramentas para o designer, defina sua propriedade `ID` como `Products`e, em sua marca inteligente, escolha associá-la a um novo controle ObjectDataSource chamado `ProductsDataSource`. Configure o ObjectDataSource para trabalhar com a classe `ProductsBLL`.

[![configurar o ObjectDataSource para usar a classe ProductsBLL](caching-data-with-the-objectdatasource-cs/_static/image7.png)](caching-data-with-the-objectdatasource-cs/_static/image6.png)

**Figura 4**: configurar o ObjectDataSource para usar a classe `ProductsBLL` ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image8.png))

Para esta página, vamos criar um GridView editável para que possamos examinar o que acontece quando os dados armazenados em cache em ObjectDataSource são modificados por meio da interface GridView. Deixe a lista suspensa na guia selecionar definida como seu padrão, `GetProducts()`, mas altere o item selecionado na guia atualizar para a sobrecarga `UpdateProduct` que aceita `productName`, `unitPrice`e `productID` como seus parâmetros de entrada.

[![definir a lista suspensa da guia atualizar para a sobrecarga UpdateProduct apropriada](caching-data-with-the-objectdatasource-cs/_static/image10.png)](caching-data-with-the-objectdatasource-cs/_static/image9.png)

**Figura 5**: definir a lista suspensa da guia atualizar s para a sobrecarga de `UpdateProduct` apropriada ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image11.png))

Por fim, defina as listas suspensas nas guias inserir e excluir como (nenhum) e clique em concluir. Após a conclusão do assistente para configurar fonte de dados, o Visual Studio define a Propriedade ObjectDataSource s `OldValuesParameterFormatString` como `original_{0}`. Conforme discutido na [visão geral do tutorial inserir, atualizar e excluir dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , essa propriedade precisa ser removida da sintaxe declarativa ou redefinida para o valor padrão, `{0}`, para que o fluxo de trabalho de atualização continue sem erros.

Além disso, na conclusão do assistente, o Visual Studio adiciona um campo ao GridView para cada um dos campos de dados do produto. Remova tudo, exceto o `ProductName`, o `CategoryName`e o `UnitPrice` BoundFields. Em seguida, atualize as propriedades `HeaderText` de cada um desses BoundFields para produto, categoria e preço, respectivamente. Como o campo `ProductName` é necessário, converta o BoundField em um TemplateField e adicione um RequiredFieldValidator ao `EditItemTemplate`. Da mesma forma, converta o `UnitPrice` BoundField em um TemplateField e adicione um CompareValidator para garantir que o valor inserido pelo usuário seja um valor de moeda válido que seja maior ou igual a zero. Além dessas modificações, sinta-se à vontade para executar quaisquer alterações estéticos, como o alinhamento à direita do valor `UnitPrice` ou a especificação da formatação para o texto `UnitPrice` em suas interfaces somente leitura e de edição.

Torne o GridView editável marcando a caixa de seleção Habilitar edição na marca inteligente s GridView. Marque também as caixas de seleção habilitar paginação e Habilitar classificação.

> [!NOTE]
> Precisa de uma revisão de como personalizar a interface de edição de GridView? Nesse caso, consulte o tutorial [Personalizando a interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) .

[![habilitar o suporte a GridView para edição, classificação e paginação](caching-data-with-the-objectdatasource-cs/_static/image13.png)](caching-data-with-the-objectdatasource-cs/_static/image12.png)

**Figura 6**: habilitar o suporte a GridView para edição, classificação e paginação ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image14.png))

Depois de fazer essas modificações em GridView, a marcação declarativa de GridView e ObjectDataSource s deve ser semelhante ao seguinte:

[!code-aspx[Main](caching-data-with-the-objectdatasource-cs/samples/sample2.aspx)]

Como mostra a Figura 7, o GridView editável lista o nome, a categoria e o preço de cada um dos produtos no banco de dados. Reserve um tempo para testar a funcionalidade de s de página, classificar os resultados, pageá-los e editar um registro.

[![cada nome, categoria e preço do produto está listado em um GridView classificável, paginável e editável](caching-data-with-the-objectdatasource-cs/_static/image16.png)](caching-data-with-the-objectdatasource-cs/_static/image15.png)

**Figura 7**: cada nome, categoria e preço do produto é listado em um GridView classificável, paginável e editável ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image17.png))

## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Etapa 3: examinando quando o ObjectDataSource está solicitando dados

O `Products` GridView recupera seus dados para exibição invocando o método `Select` do `ProductsDataSource` ObjectDataSource. Esse ObjectDataSource cria uma instância da classe de `ProductsBLL` da camada de lógica de negócios e chama seu método `GetProducts()`, que por sua vez chama o método de `GetProducts()` de camada de acesso a dados s `ProductsTableAdapter` s. O método DAL conecta-se ao banco de dados Northwind e emite a consulta de `SELECT` configurada. Esses dados são retornados para a DAL, que o empacota em um `NorthwindDataTable`. O objeto DataTable é retornado para a BLL, que retorna para o ObjectDataSource, que o retorna para o GridView. Em seguida, o GridView cria um objeto `GridViewRow` para cada `DataRow` na DataTable, e cada `GridViewRow` é eventualmente renderizado no HTML que é retornado ao cliente e exibido no navegador do visitante.

Essa sequência de eventos acontece cada uma e toda vez que o GridView precisa se associar aos seus dados subjacentes. Isso acontece quando a página é visitada pela primeira vez, ao mover de uma página de dados para outra, ao classificar o GridView ou ao modificar os dados de GridView por meio de suas interfaces internas de edição ou exclusão. Se o estado de exibição de GridView s estiver desabilitado, o GridView será reassociado em cada e todos os postbacks também. O GridView também pode ser reassociado explicitamente a seus dados chamando seu método `DataBind()`.

Para apreciar totalmente a frequência com que os dados são recuperados do banco de dados, vamos exibir uma mensagem indicando quando os dados estão sendo recuperados novamente. Adicione um controle de rótulo da Web acima do GridView chamado `ODSEvents`. Desmarque sua propriedade `Text` e defina sua propriedade `EnableViewState` como `false`. Abaixo do rótulo, adicione um controle de botão da Web e defina sua propriedade `Text` como postback.

[![adicionar um rótulo e um botão à página acima do GridView](caching-data-with-the-objectdatasource-cs/_static/image19.png)](caching-data-with-the-objectdatasource-cs/_static/image18.png)

**Figura 8**: adicionar um rótulo e um botão à página acima do GridView ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image20.png))

Durante o fluxo de trabalho de acesso a dados, o evento ObjectDataSource s `Selecting` é disparado antes de o objeto subjacente ser criado e seu método configurado invocado. Crie um manipulador de eventos para esse evento e adicione o seguinte código:

[!code-csharp[Main](caching-data-with-the-objectdatasource-cs/samples/sample3.cs)]

Cada vez que o ObjectDataSource faz uma solicitação para a arquitetura de dados, o rótulo exibirá o texto que seleciona o evento disparado.

Visite esta página em um navegador. Quando a página é visitada pela primeira vez, o evento de seleção de texto acionado é mostrado. Clique no botão postback e observe que o texto desaparece (supondo que a Propriedade GridView s `EnableViewState` esteja definida como `true`, o padrão). Isso ocorre porque, no postback, o GridView é reconstruído a partir de seu estado de exibição e, portanto, não é ativado para o ObjectDataSource para seus dados. No entanto, classificar, paginar ou editar os dados faz com que o GridView reassocie a sua fonte de dados e, portanto, o texto de seleção disparado é exibido novamente.

[![sempre que GridView é reassociado à sua fonte de dados, selecionar evento disparado é exibido](caching-data-with-the-objectdatasource-cs/_static/image22.png)](caching-data-with-the-objectdatasource-cs/_static/image21.png)

**Figura 9**: sempre que GridView é reassociado à sua fonte de dados, selecionar o evento disparado é exibido ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image23.png))

[![clicar no botão postback faz com que o GridView seja reconstruído a partir de seu estado de exibição](caching-data-with-the-objectdatasource-cs/_static/image25.png)](caching-data-with-the-objectdatasource-cs/_static/image24.png)

**Figura 10**: clicar no botão postback faz com que o GridView seja reconstruído a partir de seu estado de exibição ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image26.png))

Pode parecer um desperdício recuperar os dados do banco cada vez que os dados são paginados ou classificados. Afinal, como reutilizamos a paginação padrão, o ObjectDataSource recuperou todos os registros ao exibir a primeira página. Mesmo que o GridView não forneça suporte à classificação e à paginação, os dados deverão ser recuperados do banco cada vez que a página for visitada pela primeira vez por qualquer usuário (e em cada postagem, se o estado de exibição estiver desabilitado). Mas se GridView estiver mostrando os mesmos dados a todos os usuários, essas solicitações de banco de dados extras serão supérfluas. Por que não armazenar em cache os resultados retornados do método `GetProducts()` e associar o GridView a esses resultados armazenados em cache?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Etapa 4: armazenar em cache os dados usando o ObjectDataSource

Simplesmente definindo algumas propriedades, o ObjectDataSource pode ser configurado para armazenar em cache automaticamente seus dados recuperados no cache de dados ASP.NET. A lista a seguir resume as propriedades relacionadas ao cache de ObjectDataSource:

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) deve ser definido como `true` para habilitar o Caching. O padrão é `false`.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) a quantidade de tempo, em segundos, em que os dados são armazenados em cache. O padrão é 0. O ObjectDataSource só armazenará os dados em cache se `EnableCaching` estiver `true` e `CacheDuration` for definido como um valor maior que zero.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) pode ser definido como `Absolute` ou `Sliding`. Se `Absolute`, o ObjectDataSource armazenará em cache seus dados recuperados por `CacheDuration` segundos; se `Sliding`, os dados expirarão somente depois que não tiverem sido acessados por `CacheDuration` segundos. O padrão é `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) Use essa propriedade para associar as entradas de cache ObjectDataSource s a uma dependência de cache existente. As entradas de dados de ObjectDataSource s podem ser removidas prematuramente do cache, expirando seu `CacheKeyDependency`associado. Essa propriedade é mais comumente usada para associar uma dependência de cache do SQL ao cache ObjectDataSource s, um tópico que exploraremos no futuro usando o tutorial de [dependências do cache do SQL](using-sql-cache-dependencies-cs.md) .

Deixe que os s configurem o `ProductsDataSource` ObjectDataSource para armazenar em cache seus dados por 30 segundos em uma escala absoluta. Defina a Propriedade ObjectDataSource s `EnableCaching` como `true` e sua propriedade `CacheDuration` como 30. Deixe a propriedade `CacheExpirationPolicy` definida como seu padrão, `Absolute`.

[![configurar o ObjectDataSource para armazenar em cache seus dados por 30 segundos](caching-data-with-the-objectdatasource-cs/_static/image28.png)](caching-data-with-the-objectdatasource-cs/_static/image27.png)

**Figura 11**: configurar o ObjectDataSource para armazenar em cache seus dados por 30 segundos ([clique para exibir a imagem em tamanho normal](caching-data-with-the-objectdatasource-cs/_static/image29.png))

Salve suas alterações e revisite esta página em um navegador. O texto de seleção acionado será exibido quando você visitar a página pela primeira vez, pois inicialmente os dados não estão no cache. Mas os postbacks subsequentes disparados clicando no botão postback, classificando, paginando ou clicando nos botões editar ou *Cancelar não* exibem novamente o texto de seleção acionado. Isso ocorre porque o evento `Selecting` só é acionado quando o ObjectDataSource obtém seus dados de seu objeto subjacente; o evento `Selecting` não será acionado se os dados forem extraídos do cache de dados.

Após 30 segundos, os dados serão removidos do cache. Os dados também serão removidos do cache se os métodos ObjectDataSource s `Insert`, `Update`ou `Delete` forem invocados. Consequentemente, depois de 30 segundos ou o botão de atualização ter sido clicado, classificar, paginar ou clicar nos botões editar ou cancelar fará com que o ObjectDataSource obtenha seus dados de seu objeto subjacente, exibindo o evento selecionando o texto acionado quando o evento `Selecting` for acionado. Esses resultados retornados são colocados de volta no cache de dados.

> [!NOTE]
> Se você vir o texto de seleção acionado com frequência, mesmo quando espera que o ObjectDataSource esteja trabalhando com dados armazenados em cache, pode ser devido a restrições de memória. Se não houver memória livre suficiente, os dados adicionados ao cache pelo ObjectDataSource podem ter sido eliminados. Se a ObjectDataSource não parecer armazenar os dados em cache corretamente ou apenas armazenar os dados de forma esporádica, feche alguns aplicativos para liberar memória e tente novamente.

A Figura 12 ilustra o fluxo de trabalho de cache do ObjectDataSource. Quando o texto de seleção disparado é exibido na tela, isso ocorre porque os dados não estavam no cache e tiveram que ser recuperados do objeto subjacente. No entanto, quando esse texto está ausente, ele ocorre porque os dados estavam disponíveis no cache. Quando os dados são retornados do cache, não há nenhuma chamada para o objeto subjacente e, portanto, nenhuma consulta de banco de dados foi executada.

![O ObjectDataSource armazena e recupera seus dados do cache de dados](caching-data-with-the-objectdatasource-cs/_static/image30.png)

**Figura 12**: o ObjectDataSource armazena e recupera seus dados do cache de dados

Cada aplicativo ASP.NET tem sua própria instância de cache de dados que s compartilhou em todas as páginas e visitantes. Isso significa que os dados armazenados no cache de dados pelo ObjectDataSource, da mesma forma, são compartilhados entre todos os usuários que visitam a página. Para verificar isso, abra a página `ObjectDataSource.aspx` em um navegador. Ao visitar a página pela primeira vez, o texto de seleção disparado será exibido (supondo que os dados adicionados ao cache pelos testes anteriores tenham sido removidos agora). Abra uma segunda instância do navegador e copie e cole a URL da primeira instância do navegador para a segunda. Na segunda instância do navegador, o texto de seleção acionado não é mostrado porque ele usa os mesmos dados armazenados em cache como o primeiro.

Ao inserir seus dados recuperados no cache, o ObjectDataSource usa um valor de chave de cache que inclui: os valores de propriedade `CacheDuration` e `CacheExpirationPolicy`; o tipo do objeto comercial subjacente que está sendo usado pelo ObjectDataSource, que é especificado por meio da [propriedade`TypeName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, neste exemplo); o valor da propriedade `SelectMethod` e o nome e os valores dos parâmetros na coleção `SelectParameters`; e os valores de suas propriedades `StartRowIndex` e `MaximumRows`, que são usadas na implementação de [paginação personalizada.](../paging-and-sorting/paging-and-sorting-report-data-cs.md)

A criação do valor da chave de cache como uma combinação dessas propriedades garante uma entrada de cache exclusiva à medida que esses valores forem alterados. Por exemplo, nos tutoriais anteriores, veremos como usar o `GetProductsByCategoryID(categoryID)`da classe `ProductsBLL`, que retorna todos os produtos de uma categoria especificada. Um usuário pode chegar à página e exibir bebidas, que tem um `CategoryID` de 1. Se o ObjectDataSource armazenar em cache seus resultados sem considerar os valores de `SelectParameters`, quando outro usuário vier à página para exibir condiments enquanto os produtos de bebidas estivessem no cache, eles d verão os produtos de bebidas em cache em vez de condiments. Variando a chave de cache por essas propriedades, que incluem os valores da `SelectParameters`, o ObjectDataSource mantém uma entrada de cache separada para bebidas e condiments.

## <a name="stale-data-concerns"></a>Problemas de dados obsoletos

O ObjectDataSource remove automaticamente seus itens do cache quando qualquer um de seus métodos `Insert`, `Update`ou `Delete` é invocado. Isso ajuda a proteger contra dados obsoletos limpando as entradas de cache quando os dados são modificados por meio da página. No entanto, é possível que um ObjectDataSource usando Caching ainda exiba dados obsoletos. No caso mais simples, pode ser devido à alteração de dados diretamente no banco de dado. Talvez um administrador de banco de dados simplesmente executasse um script que modificasse alguns dos registros no banco de dados.

Esse cenário também pode desdobrar de maneira mais sutil. Enquanto o ObjectDataSource remove seus itens do cache quando um de seus métodos de modificação de dados é chamado, os itens armazenados em cache removidos são para a combinação de valores de propriedade de ObjectDataSource s (`CacheDuration`, `TypeName`, `SelectMethod`e assim por diante). Se você tiver duas objectdatasources que usam diferentes `SelectMethods` ou `SelectParameters`, mas ainda puder atualizar os mesmos dados, um ObjectDataSource poderá atualizar uma linha e invalidar suas próprias entradas de cache, mas a linha correspondente para o segundo ObjectDataSource ainda será servida a partir do cache. Recomendo que você crie páginas para exibir essa funcionalidade. Crie uma página que exibe um GridView editável que extrai seus dados de um ObjectDataSource que usa Caching e está configurado para obter dados do método de `GetProducts()` da classe `ProductsBLL`. Adicione outro GridView editável e ObjectDataSource a esta página (ou outra), mas, para esse segundo ObjectDataSource, use o método `GetProductsByCategoryID(categoryID)`. Como as duas propriedades de `SelectMethod` ObjectDataSource são diferentes, elas têm seus próprios valores armazenados em cache. Se você editar um produto em uma grade, na próxima vez que vincular os dados de volta à outra grade (por paginação, classificação e assim por diante), ele ainda servirá os dados antigos armazenados em cache e não refletirá a alteração feita na outra grade.

Em suma, use somente expirações baseadas em tempo se você estiver disposto a ter o potencial de dados obsoletos e usar expirações menores para cenários em que a atualização de dados é importante. Se os dados obsoletos não forem aceitáveis, abrem mão o cache ou use as dependências de cache do SQL (supondo-se que os dados do banco que você armazenam em cache Exploraremos as dependências do cache do SQL em um tutorial futuro.

## <a name="summary"></a>Resumo

Neste tutorial, examinamos as funcionalidades de cache internas do ObjectDataSource s. Simplesmente definindo algumas propriedades, podemos instruir o ObjectDataSource a armazenar em cache os resultados retornados do `SelectMethod` especificado no cache de dados ASP.NET. As propriedades `CacheDuration` e `CacheExpirationPolicy` indicam a duração em que o item é armazenado em cache e se é uma expiração absoluta ou deslizante. A propriedade `CacheKeyDependency` associa todas as entradas de cache ObjectDataSource s a uma dependência de cache existente. Isso pode ser usado para remover as entradas de ObjectDataSource s do cache antes que a expiração baseada em tempo seja atingida e normalmente é usada com dependências de cache do SQL.

Como o ObjectDataSource simplesmente armazena em cache seus valores no cache de dados, poderíamos replicar a funcionalidade interna de ObjectDataSource s de forma programática. Não faz sentido fazer isso na camada de apresentação, já que o ObjectDataSource oferece essa funcionalidade pronta para uso, mas podemos implementar recursos de cache em uma camada separada da arquitetura. Para fazer isso, precisaremos repetir a mesma lógica usada pelo ObjectDataSource. Exploraremos como trabalhar programaticamente com o cache de dados de dentro da arquitetura em nosso próximo tutorial.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Cache ASP.NET: técnicas e práticas recomendadas](https://msdn.microsoft.com/library/aa478965.aspx)
- [Guia de arquitetura de caching para aplicativos .NET Framework](https://msdn.microsoft.com/library/ee817645.aspx)
- [Cache de saída no ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Teresa Murphy. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](caching-data-in-the-architecture-cs.md)
