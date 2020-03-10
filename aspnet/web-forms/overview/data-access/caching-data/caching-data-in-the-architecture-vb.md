---
uid: web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
title: Armazenando dados em cache na arquitetura (VB) | Microsoft Docs
author: rick-anderson
description: No tutorial anterior, aprendemos como aplicar o Caching na camada de apresentação. Neste tutorial, aprendemos como tirar proveito de nossa arquitetada em camadas...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 5e189dd7-f4f9-4f28-9b3a-6cb7d392e9c7
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-in-the-architecture-vb
msc.type: authoredcontent
ms.openlocfilehash: dc991a205fa7e61f604bc0f26e9b24b3faefd3d3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78551005"
---
# <a name="caching-data-in-the-architecture-vb"></a>Armazenar dados em cache na arquitetura (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o aplicativo de exemplo](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_59_VB.exe) ou [baixar PDF](caching-data-in-the-architecture-vb/_static/datatutorial59vb1.pdf)

> No tutorial anterior, aprendemos como aplicar o Caching na camada de apresentação. Neste tutorial, aprendemos a aproveitar nossa arquitetura em camadas para armazenar dados em cache na camada de lógica de negócios. Fazemos isso estendendo a arquitetura para incluir uma camada de cache.

## <a name="introduction"></a>Introdução

Como vimos no tutorial anterior, o Caching dos dados de ObjectDataSource s é tão simples quanto definir algumas propriedades. Infelizmente, o ObjectDataSource aplica o Caching na camada de apresentação, que acopla rigidamente as políticas de cache com a página ASP.NET. Um dos motivos para criar uma arquitetura em camadas é permitir que esses acoplamentos sejam quebrados. A camada de lógica de negócios, por exemplo, dissocia a lógica de negócios das páginas ASP.NET, enquanto a camada de acesso a dados dissocia os detalhes de acesso a dados. Essa desassociação da lógica de negócios e dos detalhes de acesso a dados é preferida, em parte, porque torna o sistema mais legível, mais passível de manutenção e mais flexível para mudar. Ele também permite o conhecimento do domínio e a divisão de mão-de-obra que um desenvolvedor que trabalha na camada de apresentação não precisa estar familiarizado com os detalhes do banco de dados para fazer seu trabalho. A separação da política de cache da camada de apresentação oferece benefícios semelhantes.

Neste tutorial, ampliaremos nossa arquitetura para incluir uma *camada de cache* (ou CL por curto) que emprega nossa política de cache. A camada de cache incluirá uma classe `ProductsCL` que fornece acesso a informações do produto com métodos como `GetProducts()`, `GetProductsByCategoryID(categoryID)`e assim por diante, que, quando invocado, tentará primeiro recuperar os dados do cache. Se o cache estiver vazio, esses métodos invocarão o método de `ProductsBLL` apropriado na BLL, que, por sua vez, obteria os dados da DAL. Os métodos de `ProductsCL` armazenam em cache os dados recuperados da BLL antes de retorná-los.

Como mostra a Figura 1, o CL reside entre as camadas de lógica de negócios e de apresentação.

![A camada de cache (CL) é outra camada em nossa arquitetura](caching-data-in-the-architecture-vb/_static/image1.png)

**Figura 1**: a camada de cache (CL) é outra camada em nossa arquitetura

## <a name="step-1-creating-the-caching-layer-classes"></a>Etapa 1: criando as classes de camada de cache

Neste tutorial, criaremos um CL muito simples com uma única classe `ProductsCL` que tem apenas alguns métodos. Criar uma camada de cache completa para todo o aplicativo exigiria a criação de classes `CategoriesCL`, `EmployeesCL`e `SuppliersCL` e o fornecimento de um método nessas classes de camada de cache para cada método de acesso a dados ou modificação na BLL. Assim como com a BLL e a DAL, a camada de cache deve, idealmente, ser implementada como um projeto de biblioteca de classes separado; no entanto, iremos implementá-lo como uma classe na pasta `App_Code`.

Para separar com mais clareza as classes CL das classes DAL e BLL, vamos criar uma nova subpasta na pasta `App_Code`. Clique com o botão direito do mouse na pasta `App_Code` no Gerenciador de Soluções, escolha nova pasta e nomeie a nova pasta `CL`. Depois de criar essa pasta, adicione a ela uma nova classe chamada `ProductsCL.vb`.

![Adicione uma nova pasta chamada CL e uma classe chamada ProductsCL. vb](caching-data-in-the-architecture-vb/_static/image2.png)

**Figura 2**: adicionar uma nova pasta chamada `CL` e uma classe chamada `ProductsCL.vb`

A classe `ProductsCL` deve incluir o mesmo conjunto de métodos de acesso e modificação de dados, conforme encontrado em sua classe de camada lógica de negócios correspondente (`ProductsBLL`). Em vez de criar todos esses métodos, vamos apenas criar dois aqui para ter uma ideia dos padrões usados pelo CL. Em particular, adicionaremos os métodos `GetProducts()` e `GetProductsByCategoryID(categoryID)` na etapa 3 e uma sobrecarga `UpdateProduct` na etapa 4. Você pode adicionar os métodos de `ProductsCL` restantes e as classes `CategoriesCL`, `EmployeesCL`e `SuppliersCL` ao seu lazer.

## <a name="step-2-reading-and-writing-to-the-data-cache"></a>Etapa 2: lendo e gravando no cache de dados

O recurso de cache ObjectDataSource explorado no tutorial anterior usa internamente o cache de dados ASP.NET para armazenar os dados recuperados da BLL. O cache de dados também pode ser acessado programaticamente de classes de código-behind de páginas ASP.NET ou das classes na arquitetura do aplicativo Web. Para ler e gravar no cache de dados de uma classe code-behind da página ASP.NET, use o seguinte padrão:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample1.vb)]

O [método`Insert`](https://msdn.microsoft.com/library/system.web.caching.cache.insert.aspx) da [classe`Cache`](https://msdn.microsoft.com/library/system.web.caching.cache.aspx) tem várias sobrecargas. `Cache("key") = value` e `Cache.Insert(key, value)` são sinônimos e ambos adicionam um item ao cache usando a chave especificada sem uma expiração definida. Normalmente, queremos especificar uma expiração ao adicionar um item ao cache, seja como uma dependência, uma expiração baseada em tempo ou ambos. Use um dos outros `Insert` sobrecargas de método para fornecer informações de expiração com base na dependência ou em tempo.

Os métodos s da camada de cache precisam primeiro verificar se os dados solicitados estão no cache e, em caso afirmativo, retorná-los a partir daí. Se os dados solicitados não estiverem no cache, o método BLL apropriado precisará ser invocado. Seu valor de retorno deve ser armazenado em cache e, em seguida, retornado, como ilustra o diagrama de sequência a seguir.

![Os métodos s da camada de cache retornam dados do cache se estiverem disponíveis](caching-data-in-the-architecture-vb/_static/image3.png)

**Figura 3**: os métodos s da camada de cache retornam dados do cache, se estiverem disponíveis

A sequência representada na Figura 3 é realizada nas classes CL usando o seguinte padrão:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample2.vb)]

Aqui, *Type* é o tipo de dados que estão sendo armazenados no cache `Northwind.ProductsDataTable`, por exemplo, enquanto *Key* é a chave que identifica exclusivamente o item de cache. Se o item com a *chave* especificada não estiver no cache, a *instância* será `Nothing` e os dados serão recuperados do método BLL apropriado e adicionados ao cache. Quando o `Return instance` de tempo é atingido, a *instância* contém uma referência aos dados, do cache ou extraído da BLL.

Certifique-se de usar o padrão acima ao acessar dados do cache. O padrão a seguir, que, à primeira vista, parece equivalente, contém uma diferença sutil que introduz uma condição de corrida. As condições de corrida são difíceis de depurar porque revelam esporadicamente e são difíceis de reproduzir.

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample3.vb)]

A diferença neste segundo, trecho de código incorreto, é que, em vez de armazenar uma referência ao item armazenado em cache em uma variável local, o cache de dados é acessado diretamente na instrução condicional *e* no `Return`. Imagine que, quando esse código for atingido, `Cache("key")` não seja `Nothing`, mas antes que a instrução `Return` seja atingida, o sistema removerá a *chave* do cache. Nesse caso raro, o código retornará `Nothing` em vez de um objeto do tipo esperado.

> [!NOTE]
> O cache de dados é thread-safe, portanto, você não precisa sincronizar o acesso ao thread para leituras ou gravações simples. No entanto, se você precisar executar várias operações em dados no cache que precisam ser atômicas, será responsável pela implementação de um bloqueio ou algum outro mecanismo para garantir a segurança do thread. Consulte [sincronizando o acesso ao Cache ASP.net](http://www.ddj.com/184406369) para obter mais informações.

Um item pode ser removido programaticamente do cache de dados usando o [método`Remove`](https://msdn.microsoft.com/library/system.web.caching.cache.remove.aspx) da seguinte forma:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample4.vb)]

## <a name="step-3-returning-product-information-from-theproductsclclass"></a>Etapa 3: retornando informações do produto da classe`ProductsCL`

Para este tutorial, vamos implementar dois métodos para retornar informações de produto da classe `ProductsCL`: `GetProducts()` e `GetProductsByCategoryID(categoryID)`. Assim como com a classe `ProductsBL` na camada de lógica de negócios, o método `GetProducts()` no CL retorna informações sobre todos os produtos como um objeto `Northwind.ProductsDataTable`, enquanto `GetProductsByCategoryID(categoryID)` retorna todos os produtos de uma categoria especificada.

O código a seguir mostra uma parte dos métodos na classe `ProductsCL`:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample5.vb)]

Primeiro, observe os atributos `DataObject` e `DataObjectMethodAttribute` aplicados à classe e aos métodos. Esses atributos fornecem informações para o assistente ObjectDataSource s, indicando quais classes e métodos devem aparecer nas etapas do assistente. Como as classes e os métodos CL serão acessados de um ObjectDataSource na camada de apresentação, adicionei esses atributos para aprimorar a experiência em tempo de design. Consulte o tutorial [criando uma camada de lógica de negócios](../introduction/creating-a-business-logic-layer-vb.md) para obter uma descrição mais completa sobre esses atributos e seus efeitos.

Nos métodos `GetProducts()` e `GetProductsByCategoryID(categoryID)`, os dados retornados do método `GetCacheItem(key)` são atribuídos a uma variável local. O método `GetCacheItem(key)`, que examinaremos em breve, retorna um item específico do cache com base na *chave*especificada. Se esses dados não forem encontrados no cache, eles serão recuperados do método de classe de `ProductsBLL` correspondente e, em seguida, adicionados ao cache usando o método `AddCacheItem(key, value)`.

A interface de métodos `GetCacheItem(key)` e `AddCacheItem(key, value)` com o cache de dados, leitura e gravação de valores, respectivamente. O método `GetCacheItem(key)` é o mais simples dos dois. Ele simplesmente retorna o valor da classe de cache usando a *chave*passada:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample6.vb)]

`GetCacheItem(key)` não usa o valor de *chave* conforme fornecido, mas chama o método `GetCacheKey(key)`, que retorna a *chave* precedida por ProductsCache-. O `MasterCacheKeyArray`, que contém a cadeia de caracteres ProductsCache, também é usado pelo método `AddCacheItem(key, value)`, como veremos momentaneamente.

De uma classe code-behind da página ASP.NET s, o cache de dados pode ser acessado usando a [propriedade `Page``Cache`](https://msdn.microsoft.com/library/system.web.ui.page.cache.aspx)classe e permite sintaxe como `Cache("key") = value`, conforme discutido na etapa 2. De uma classe dentro da arquitetura, o cache de dados pode ser acessado usando `HttpRuntime.Cache` ou `HttpContext.Current.Cache`. A entrada de blog de [Peter Johnson](https://weblogs.asp.net/pjohnson/default.aspx)em [HttpRuntime. cache versus HttpContext. Current. cache](https://weblogs.asp.net/pjohnson/httpruntime-cache-vs-httpcontext-current-cache) observa a ligeira vantagem de desempenho no uso de `HttpRuntime` em vez de `HttpContext.Current`; Consequentemente, `ProductsCL` usa `HttpRuntime`.

> [!NOTE]
> Se sua arquitetura for implementada usando projetos de biblioteca de classes, você precisará adicionar uma referência ao assembly de `System.Web` para usar as classes [`HttpRuntime`](https://msdn.microsoft.com/library/system.web.httpruntime.aspx) e [`HttpContext`](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) .

Se o item não for encontrado no cache, os métodos s da classe `ProductsCL` obterão os dados da BLL e o adicionarão ao cache usando o método `AddCacheItem(key, value)`. Para adicionar *valor* ao cache, poderíamos usar o código a seguir, que usa um período de expiração de 60 segundos:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample7.vb)]

`DateTime.Now.AddSeconds(CacheDuration)` especifica a expiração baseada em tempo de 60 segundos no futuro, enquanto [`System.Web.Caching.Cache.NoSlidingExpiration`](https://msdn.microsoft.com/library/system.web.caching.cache.noslidingexpiration(vs.80).aspx) indica que não há nenhuma expiração deslizante. Embora esse `Insert` sobrecarga de método tenha parâmetros de entrada para uma expiração absoluta e deslizante, você só pode fornecer um dos dois. Se você tentar especificar uma hora absoluta e um período de tempo, o método `Insert` gerará uma exceção de `ArgumentException`.

> [!NOTE]
> Essa implementação do método `AddCacheItem(key, value)` atualmente tem algumas deficiências. Abordaremos e superaremos esses problemas na etapa 4.

## <a name="step-4-invalidating-the-cache-when-the-data-is-modified-through-the-architecture"></a>Etapa 4: invalidando o cache quando os dados são modificados por meio da arquitetura

Juntamente com os métodos de recuperação de dados, a camada de cache precisa fornecer os mesmos métodos que a BLL para inserir, atualizar e excluir dados. Os métodos de modificação de dados CI s não modificam os dados armazenados em cache, mas chamam o método de modificação de dados correspondente da BLL e, em seguida, invalidam o cache. Como vimos no tutorial anterior, esse é o mesmo comportamento que o ObjectDataSource aplica quando seus recursos de caching estão habilitados e seus `Insert`, `Update`ou `Delete` métodos são invocados.

A sobrecarga de `UpdateProduct` a seguir ilustra como implementar os métodos de modificação de dados no CL:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample8.vb)]

O método apropriado da camada de lógica de negócios de modificação de dados é invocado, mas antes de sua resposta ser retornada, precisamos invalidar o cache. Infelizmente, a invalidação do cache não é simples porque a classe `ProductsCL` s `GetProducts()` e os métodos `GetProductsByCategoryID(categoryID)` cada um adiciona itens ao cache com chaves diferentes, e o método `GetProductsByCategoryID(categoryID)` adiciona um item de cache diferente para cada *CategoryID*exclusiva.

Ao invalidar o cache, precisamos remover *todos* os itens que podem ter sido adicionados pela classe `ProductsCL`. Isso pode ser feito associando uma *dependência de cache* a cada item adicionado ao cache no método `AddCacheItem(key, value)`. Em geral, uma dependência de cache pode ser outro item no cache, um arquivo no sistema de arquivos ou dados de um banco de dado Microsoft SQL Server. Quando a dependência é alterada ou removida do cache, os itens de cache aos quais ele está associado são automaticamente removidos do cache. Para este tutorial, queremos criar um item adicional no cache que serve como uma dependência de cache para todos os itens adicionados por meio da classe `ProductsCL`. Dessa forma, todos esses itens podem ser removidos do cache simplesmente removendo a dependência de cache.

Deixe que os s atualizem o método `AddCacheItem(key, value)` para que cada item adicionado ao cache por meio desse método esteja associado a uma única dependência de cache:

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample9.vb)]

`MasterCacheKeyArray` é uma matriz de cadeia de caracteres que contém um único valor, ProductsCache. Primeiro, um item de cache é adicionado ao cache e atribuído a data e hora atuais. Se o item de cache já existir, ele será atualizado. Em seguida, uma dependência de cache é criada. O construtor [classe s`CacheDependency`](https://msdn.microsoft.com/library/system.web.caching.cachedependency(VS.80).aspx) tem várias sobrecargas, mas o que está sendo usado aqui espera duas entradas de matriz `String`. O primeiro especifica o conjunto de arquivos a ser usado como dependências. Como não queremos usar nenhuma dependência baseada em arquivo, um valor de `Nothing` é usado para o primeiro parâmetro de entrada. O segundo parâmetro de entrada especifica o conjunto de chaves de cache a ser usado como dependências. Aqui, especificamos nossa única dependência, `MasterCacheKeyArray`. Em seguida, o `CacheDependency` é passado para o método `Insert`.

Com essa modificação para `AddCacheItem(key, value)`, invalidar o cache é tão simples quanto remover a dependência.

[!code-vb[Main](caching-data-in-the-architecture-vb/samples/sample10.vb)]

## <a name="step-5-calling-the-caching-layer-from-the-presentation-layer"></a>Etapa 5: chamando a camada de cache da camada de apresentação

As classes e os métodos da camada de cache podem ser usados para trabalhar com dados usando as técnicas que veremos em todos esses tutoriais. Para ilustrar como trabalhar com dados armazenados em cache, salve as alterações na classe `ProductsCL` e, em seguida, abra a página `FromTheArchitecture.aspx` na pasta `Caching` e adicione um GridView. Na marca inteligente s GridView, crie um novo ObjectDataSource. Na primeira etapa do assistente, você deve ver a classe `ProductsCL` como uma das opções da lista suspensa.

[![a classe ProductsCL está incluída na lista suspensa objeto comercial](caching-data-in-the-architecture-vb/_static/image5.png)](caching-data-in-the-architecture-vb/_static/image4.png)

**Figura 4**: a classe `ProductsCL` está incluída na lista suspensa objeto comercial ([clique para exibir a imagem em tamanho normal](caching-data-in-the-architecture-vb/_static/image6.png))

Depois de selecionar `ProductsCL`, clique em Avançar. A lista suspensa na guia selecionar tem dois itens-`GetProducts()` e `GetProductsByCategoryID(categoryID)` e a guia atualizar tem a única sobrecarga de `UpdateProduct`. Escolha o método `GetProducts()` na guia selecionar e o método `UpdateProducts` na guia atualizar e clique em concluir.

[![os métodos de s da classe ProductsCL são listados nas listas suspensas](caching-data-in-the-architecture-vb/_static/image8.png)](caching-data-in-the-architecture-vb/_static/image7.png)

**Figura 5**: os métodos da classe s `ProductsCL` são listados nas listas suspensas ([clique para exibir a imagem em tamanho normal](caching-data-in-the-architecture-vb/_static/image9.png))

Depois de concluir o assistente, o Visual Studio definirá a Propriedade ObjectDataSource s `OldValuesParameterFormatString` como `original_{0}` e adicionará os campos apropriados ao GridView. Altere a propriedade `OldValuesParameterFormatString` de volta para seu valor padrão, `{0}`e configure o GridView para dar suporte à paginação, classificação e edição. Como a sobrecarga de `UploadProducts` usada pelo CL aceita apenas o nome e o preço do produto editado, limite o GridView para que apenas esses campos sejam editáveis.

No tutorial anterior, definimos um GridView para incluir campos para os campos `ProductName`, `CategoryName`e `UnitPrice`. Sinta-se à vontade para replicar essa formatação e estrutura, caso em que a marcação declarativa de GridView e ObjectDataSource s deve ser semelhante ao seguinte:

[!code-aspx[Main](caching-data-in-the-architecture-vb/samples/sample11.aspx)]

Neste ponto, temos uma página que usa a camada de cache. Para ver o cache em ação, defina os pontos de interrupção nos métodos `GetProducts()` e `UpdateProduct` da classe `ProductsCL`. Visite a página em um navegador e percorra o código ao classificar e paginar para ver os dados extraídos do cache. Em seguida, atualize um registro e observe que o cache é invalidado e, consequentemente, ele é recuperado da BLL quando os dados são reassociados ao GridView.

> [!NOTE]
> A camada de cache fornecida no download que acompanha este artigo não está completa. Ele contém apenas uma classe, `ProductsCL`, que apenas apresenta alguns métodos. Além disso, apenas uma única página ASP.NET usa o CL (`~/Caching/FromTheArchitecture.aspx`), todos os outros ainda referenciam a BLL diretamente. Se você planeja usar um CL em seu aplicativo, todas as chamadas da camada de apresentação devem ir para CL, o que exigiria que as classes e métodos da CL abrangissem essas classes e métodos na BLL usados atualmente pela camada de apresentação.

## <a name="summary"></a>Resumo

Embora o cache possa ser aplicado na camada de apresentação com os controles SqlDataSource e ObjectDataSource de ASP.NET 2,0 s, as responsabilidades de cache ideais seriam delegadas a uma camada separada na arquitetura. Neste tutorial, criamos uma camada de cache que reside entre a camada de apresentação e a camada de lógica de negócios. A camada de cache precisa fornecer o mesmo conjunto de classes e métodos que existem na BLL e são chamados da camada de apresentação.

Os exemplos de camada de cache que exploramos neste e os tutoriais anteriores exibiram o *carregamento reativo*. Com o carregamento reativo, os dados são carregados no cache somente quando uma solicitação de dados é feita e esses dados estão ausentes do cache. Os dados também podem ser *carregados proativamente* no cache, uma técnica que carrega os dados no cache antes que seja realmente necessário. No próximo tutorial, veremos um exemplo de carregamento proativo quando examinamos como armazenar valores estáticos no cache na inicialização do aplicativo.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Teresa Murphy. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](caching-data-with-the-objectdatasource-vb.md)
> [Próximo](caching-data-at-application-startup-vb.md)
