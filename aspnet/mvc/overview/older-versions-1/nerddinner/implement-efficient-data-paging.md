---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Implementar paginação de dados eficiente | Microsoft Docs
author: microsoft
description: A etapa 8 mostra como adicionar suporte à paginação à nossa URL/Dinners para que, em vez de exibir milhares de jantares de uma só vez, vamos exibir 10 próximos jantares em...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 2d9a0dba381b71755ac626f76d52bc5bcb434447
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601048"
---
# <a name="implement-efficient-data-paging"></a>Implementar a paginação eficiente de dados

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 8 de um [tutorial de aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito que percorre como criar um aplicativo Web pequeno, mas completo usando o ASP.NET MVC 1.
> 
> A etapa 8 mostra como adicionar suporte à paginação à nossa URL/Dinners para que, em vez de exibir milhares de jantares de uma só vez, vamos exibir apenas 10 jantares próximos por vez – e permitir que os usuários finais retornem para trás e para frente por meio da lista inteira de uma maneira amigável de SEO.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos seguir as [introdução com](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) os tutoriais da [loja de música](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou MVC.

## <a name="nerddinner-step-8-paging-support"></a>Etapa 8 do NerdDinner: suporte à paginação

Se nosso site for bem-sucedido, ele terá milhares de jantares futuros. Precisamos ter certeza de que nossa interface do usuário pode ser dimensionada para lidar com todos esses jantares e permite que os usuários os naveguem. Para habilitar isso, adicionaremos suporte de paginação à nossa URL */Dinners* para que, em vez de exibir milhares de jantares de uma só vez, vamos exibir 10 jantares próximos por vez – e permitir que os usuários finais retornem a página inteira de uma forma amigável de SEO.

### <a name="index-action-method-recap"></a>Recapitulação do método de ação index ()

O método de ação index () em nossa classe DinnersController atualmente se parece com o seguinte:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Quando uma solicitação é feita para a URL */Dinners* , ela recupera uma lista de todos os próximos jantares e, em seguida, renderiza uma listagem de todos eles:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iqueryablelttgt"></a>Compreendendo IQueryable&lt;T&gt;

*IQueryable&lt;t&gt;* é uma interface que foi introduzida com LINQ como parte do .net 3,5. Ele permite cenários avançados de "execução adiada" que podemos aproveitar para implementar o suporte de paginação.

Em nosso DinnerRepository, estamos retornando um IQueryable&lt;jantar&gt; sequência do nosso método FindUpcomingDinners ():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

O objeto IQueryable&lt;jantar&gt; retornado pelo nosso método FindUpcomingDinners () encapsula uma consulta para recuperar objetos de jantar de nosso banco de dados usando LINQ to SQL. O mais importante é que ele não executará a consulta no banco de dados até tentarmos acessar/iterar em relação aos dados na consulta ou até chamarmos o método ToList () nele. O código que chama nosso método FindUpcomingDinners () pode, opcionalmente, optar por adicionar operações/filtros "encadeadas" adicionais ao IQueryable&lt;jantar&gt; objeto antes de executar a consulta. LINQ to SQL, em seguida, é inteligente o suficiente para executar a consulta combinada no banco de dados quando eles são solicitados.

Para implementar a lógica de paginação, podemos atualizar nosso método de ação index () de DinnersController para que ele aplique operadores "Skip" e "Take" adicionais à seqüência de&gt; de jantar de&lt;do IQueryable retornado antes de chamar ToList () nele:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

O código acima ignora os 10 primeiros jantares futuros no banco de dados e, em seguida, retorna 20 jantares. LINQ to SQL é inteligente o suficiente para construir uma consulta SQL otimizada que executa isso ignorando a lógica no banco de dados SQL – e não no servidor Web. Isso significa que, mesmo que tenhamos milhões de jantares próximos no banco de dados, somente os 10 que desejamos serão recuperados como parte dessa solicitação (tornando-o eficiente e escalonável).

### <a name="adding-a-page-value-to-the-url"></a>Adicionando um valor de "página" à URL

Em vez de embutir em código um intervalo de páginas específico, queremos que nossas URLs incluam um parâmetro "Page" que indica qual intervalo de jantar um usuário está solicitando.

#### <a name="using-a-querystring-value"></a>Usando um valor de QueryString

O código a seguir demonstra como podemos atualizar nosso método de ação index () para dar suporte a um parâmetro QueryString e habilitar URLs como */Dinners? Page = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

O método de ação index () acima tem um parâmetro chamado "Page". O parâmetro é declarado como um inteiro anulável (ou seja, int? indica). Isso significa que a URL */Dinners? Page = 2* fará com que um valor de "2" seja passado como o valor do parâmetro. A URL */Dinners* (sem um valor de QueryString) fará com que um valor nulo seja passado.

Estamos multiplicando o valor da página pelo tamanho da página (neste caso, 10 linhas) para determinar quantos jantares ignorar. Estamos usando o [ C# operador "União" nulo (??)](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) , que é útil ao lidar com tipos anuláveis. O código acima atribui a página o valor de 0 se o parâmetro de página for nulo.

#### <a name="using-embedded-url-values"></a>Usando valores de URL inseridos

Uma alternativa ao uso de um valor de QueryString seria inserir o parâmetro Page dentro da própria URL propriamente dita. Por exemplo: */Dinners/Page/2* ou */Dinners/2*. O ASP.NET MVC inclui um poderoso mecanismo de roteamento de URL que torna mais fácil oferecer suporte a cenários como esse.

Podemos registrar as regras de roteamento personalizadas que mapeiam qualquer URL de entrada ou formato de URL para qualquer classe de controlador ou método de ação que desejamos. Tudo o que precisamos fazer é abrir o arquivo global. asax em nosso projeto:

![](implement-efficient-data-paging/_static/image2.png)

E, em seguida, registrar uma nova regra de mapeamento usando o método auxiliar MapRoute () como a primeira chamada para rotas. MapRoute () abaixo:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Acima, estamos registrando uma nova regra de roteamento denominada "UpcomingDinners". Estamos indicando que ele tem o formato de URL "JANTARS/Page/{Page}" – em que {Page} é um valor de parâmetro inserido na URL. O terceiro parâmetro para o método MapRoute () indica que devemos mapear URLs que correspondam a esse formato para o método de ação index () na classe DinnersController.

Podemos usar exatamente o mesmo código de índice () que tínhamos antes com nosso cenário de QueryString – exceto que agora nosso parâmetro "Page" será proveniente da URL e não da QueryString:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

E agora, quando executarmos o aplicativo e digitarmos */Dinners* , veremos os 10 primeiros jantares futuros:

![](implement-efficient-data-paging/_static/image3.png)

E quando digitarmos o */Dinners/Page/1* , veremos a próxima página de jantares:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Adicionando interface do usuário de navegação de página

A última etapa para concluir nosso cenário de paginação será implementar a interface do usuário de navegação "Next" e "Previous" em nosso modelo de exibição para permitir que os usuários ignorem facilmente os dados do jantar.

Para implementar isso corretamente, precisaremos saber o número total de jantares no banco de dados, bem como quantas páginas de dado isso se traduz. Em seguida, precisaremos calcular se o valor de "página" solicitado no momento está no início ou no final dos dados e mostrar ou ocultar a interface do usuário "anterior" e "próximo" de acordo. Poderíamos implementar essa lógica dentro do nosso método de ação index (). Como alternativa, podemos adicionar uma classe auxiliar ao nosso projeto que encapsula essa lógica de forma mais reutilizável.

Veja abaixo uma classe auxiliar simples "PaginatedList" que deriva da lista&lt;T&gt; classe de coleção interna ao .NET Framework. Ele implementa uma classe de coleção reutilizável que pode ser usada para paginar qualquer sequência de dados IQueryable. Em nosso aplicativo NerdDinner, ele funcionará em IQueryable&lt;jantar&gt; resultados, mas poderia ser facilmente usado em relação ao IQueryable&lt;&gt; do produto ou IQueryable&lt;os resultados do cliente&gt; em outros cenários de aplicativos:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Observe que, acima de como ele calcula e expõe propriedades como "PageIndex", "PageSize", "TotalCount" e "TotalPages". Ele também expõe duas propriedades auxiliares "HasPreviousPage" e "HasNextPage" que indicam se a página de dados na coleção está no início ou no final da sequência original. O código acima fará com que duas consultas SQL sejam executadas-o primeiro para recuperar a contagem do número total de objetos do jantar (isso não retorna os objetos – em vez disso, ele executa uma instrução "SELECT COUNT" que retorna um inteiro) e o segundo para recuperar apenas as linhas de dados que precisamos de nosso banco de dados para a página atual de data.

Em seguida, podemos atualizar nosso método auxiliar DinnersController. Index () para criar um PaginatedList&lt;&gt; de jantar de nosso resultado DinnerRepository. FindUpcomingDinners () e passá-lo para nosso modelo de exibição:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

Em seguida, podemos atualizar o modelo de exibição \Views\Dinners\Index.aspx para herdar de ViewPage&lt;NerdDinner. Helpers. PaginatedList&lt;jantar&gt;&gt; em vez de ViewPage&lt;IEnumerable&lt;jantar&gt;&gt;e, em seguida, adicione o seguinte código à parte inferior de nosso modelo de exibição para mostrar ou ocultar a próxima e a interface do usuário de navegação anterior:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Observe que, acima de como estamos usando o método auxiliar HTML. RouteLink () para gerar nossos hiperlinks. Esse método é semelhante ao método auxiliar HTML. ActionLink () que usamos anteriormente. A diferença é que estamos gerando a URL usando a regra de roteamento "UpcomingDinners" que configuramos em nosso arquivo global. asax. Isso garante que geraremos URLs para nosso método de ação index () que tem o formato: */Dinners/Page/{Page}* – em que o valor {Page} é uma variável que estamos fornecendo acima com base na pageIndex atual.

E agora, quando executarmos nosso aplicativo novamente, veremos 10 jantares de cada vez em nosso navegador:

![](implement-efficient-data-paging/_static/image5.png)

Também temos &lt;&lt;&lt; e &gt;&gt;interface do usuário &gt; navegação na parte inferior da página que nos permite pular para frente e para trás sobre nossos dados usando URLs acessíveis do mecanismo de pesquisa:

![](implement-efficient-data-paging/_static/image6.png)

| **Tópico lateral: Noções básicas sobre as implicações de IQueryable&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt; é um recurso muito potente que permite uma variedade de cenários de execução deferidas interessantes (como consultas baseadas em paginação e composição). Assim como acontece com todos os recursos avançados, você deve ter cuidado com o modo de usá-lo e verificar se ele não está com abuso. É importante reconhecer que retornar um IQueryable&lt;T&gt; resultado de seu repositório permite que o código de chamada Acrescente aos métodos de operador encadeados a ele e, portanto, participe da execução da consulta definitiva. Se você não quiser fornecer ao código de chamada essa capacidade, você deve retornar IList&lt;T&gt; ou IEnumerable&lt;T&gt; resultados-que contêm os resultados de uma consulta que já foi executada. Para cenários de paginação, isso exigiria enviar por push a lógica de paginação de dados real para o método de repositório que está sendo chamado. Neste cenário, podemos atualizar nosso método localizador FindUpcomingDinners () para ter uma assinatura que retornou um PaginatedList: PaginatedList&lt; jantar&gt; FindUpcomingDinners (int pageIndex, int pageSize) {} ou retornou uma IList&lt;&gt;de jantar e usa um parâmetro de saída "totalCount" para retornar a contagem total de jantares: IList&lt;jantar&gt; FindUpcomingDinners (int pageIndex, int pageSize, out int totalCount) {} |

### <a name="next-step"></a>Próxima etapa

Agora, vamos examinar como podemos adicionar suporte de autenticação e autorização ao nosso aplicativo.

> [!div class="step-by-step"]
> [Anterior](re-use-ui-using-master-pages-and-partials.md)
> [Próximo](secure-applications-using-authentication-and-authorization.md)
