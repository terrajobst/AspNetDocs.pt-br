---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
title: Armazenando dados em cache naC#inicialização do aplicativo () | Microsoft Docs
author: rick-anderson
description: Em qualquer aplicativo Web, alguns dados serão usados com frequência e alguns dados serão usados com pouca frequência. Podemos melhorar o desempenho de nosso aplicativo ASP.NET b...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 22ca8efa-7cd1-45a7-b9ce-ce6eb3b3ff95
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
msc.type: authoredcontent
ms.openlocfilehash: a0b55b0df1b7843120de284891e16178df23fabe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78576905"
---
# <a name="caching-data-at-application-startup-c"></a>Armazenar dados em cache na inicialização do aplicativo (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar PDF](caching-data-at-application-startup-cs/_static/datatutorial60cs1.pdf)

> Em qualquer aplicativo Web, alguns dados serão usados com frequência e alguns dados serão usados com pouca frequência. Podemos melhorar o desempenho de nosso aplicativo ASP.NET carregando com antecedência os dados usados com frequência, uma técnica conhecida como Caching. Este tutorial demonstra uma abordagem para o carregamento proativo, que é carregar dados no cache na inicialização do aplicativo.

## <a name="introduction"></a>Introdução

Os dois tutoriais anteriores analisavam o cache de dados nas camadas de apresentação e cache. Em [cache de dados com o ObjectDataSource](caching-data-with-the-objectdatasource-cs.md), examinamos o uso dos recursos de cache do ObjectDataSource para armazenar dados em cache na camada de apresentação. [O cache de dados na arquitetura](caching-data-in-the-architecture-cs.md) examinou o cache em uma nova camada de cache separada. Ambos os tutoriais usaram o *carregamento reativo* no trabalho com o cache de dados. Com o carregamento reativo, cada vez que os dados são solicitados, o sistema verifica primeiro se ele está no cache. Caso contrário, ele captura os dados da fonte de origem, como o banco de dado, e os armazena no cache. A principal vantagem do carregamento reativo é sua facilidade de implementação. Uma das suas desvantagens é o desempenho irregular entre as solicitações. Imagine uma página que usa a camada de cache do tutorial anterior para exibir informações sobre o produto. Quando essa página é visitada pela primeira vez ou visitada pela primeira vez, após a remoção dos dados armazenados em cache devido a restrições de memória ou à expiração especificada ter sido atingida, os dados devem ser recuperados do banco de dado. Portanto, essas solicitações de usuários demorarão mais do que os usuários solicitações que podem ser servidas pelo cache.

O *carregamento proativo* fornece uma estratégia de gerenciamento de cache alternativa que suaviza o desempenho entre solicitações carregando os dados armazenados em cache antes que eles sejam necessários. Normalmente, o carregamento proativo usa algum processo que verifica periodicamente ou é notificado quando houve uma atualização para os dados subjacentes. Esse processo então atualiza o cache para mantê-lo atualizado. O carregamento proativo é especialmente útil se os dados subjacentes vierem de uma conexão de banco de dados lenta, de um serviço Web ou de alguma outra fonte de dados particularmente lenta. Mas essa abordagem para o carregamento proativo é mais difícil de implementar, pois requer a criação, o gerenciamento e a implantação de um processo para verificar se há alterações e atualizar o cache.

Outra parte do carregamento proativo e o tipo que iremos explorar neste tutorial é carregar dados no cache na inicialização do aplicativo. Essa abordagem é especialmente útil para armazenar em cache dados estáticos, como os registros em tabelas de pesquisa de banco de dado.

> [!NOTE]
> Para obter uma visão mais detalhada das diferenças entre o carregamento proativo e o reativo, bem como listas de prós, contras e recomendações de implementação, consulte a seção [Managing the Contents of a cache](https://msdn.microsoft.com/library/ms978503.aspx) do [Guia de arquitetura de Caching para aplicativos .NET Framework](https://msdn.microsoft.com/library/ms978498.aspx).

## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Etapa 1: Determinando quais dados armazenar em cache na inicialização do aplicativo

Os exemplos de cache que usam o carregamento reativo que examinamos nos dois tutoriais anteriores funcionam bem com dados que podem ser alterados periodicamente e não leva exorbitantly tempo para gerar. Mas se os dados armazenados em cache nunca forem alterados, a expiração usada pelo carregamento reativo será supérflua. Da mesma forma, se os dados que estão sendo armazenados em cache levarem um tempo muito longo para gerar, os usuários cujas solicitações encontrarem o cache vazio precisarão resistir a uma espera longa enquanto os dados subjacentes são recuperados. Considere armazenar dados estáticos em cache e dados que levam um tempo excepcionalmente longo para gerar na inicialização do aplicativo.

Embora os bancos de dados tenham muitos valores dinâmicos e com alteração freqüente, a maior parte também tem uma quantidade razoável de dado estático. Por exemplo, praticamente todos os modelos de dados têm uma ou mais colunas que contêm um valor específico de um conjunto fixo de opções. Uma tabela de banco de dados `Patients` pode ter uma coluna de `PrimaryLanguage`, cujo conjunto de valores poderia ser inglês, espanhol, francês, russo, japonês e assim por diante. Muitas vezes, esses tipos de colunas são implementados usando *tabelas de pesquisa*. Em vez de armazenar a cadeia de caracteres em inglês ou francês na tabela `Patients`, é criada uma segunda tabela que tem, normalmente, duas colunas – um identificador exclusivo e uma descrição de cadeia de caracteres, com um registro para cada valor possível. A coluna `PrimaryLanguage` na tabela `Patients` armazena o identificador exclusivo correspondente na tabela de pesquisa. Na Figura 1, o idioma principal do paciente John Doe é inglês, enquanto Ed Johnson é o russo.

![A tabela de idiomas é uma tabela de pesquisa usada pela tabela pacientes](caching-data-at-application-startup-cs/_static/image1.png)

**Figura 1**: a tabela `Languages` é uma tabela de pesquisa usada pela tabela `Patients`

A interface do usuário para edição ou criação de um novo paciente incluiria uma lista suspensa de idiomas permitidos populados pelos registros na tabela de `Languages`. Sem o cache, cada vez que essa interface é visitada, o sistema deve consultar a tabela de `Languages`. Isso é dispendioso e desnecessário, pois os valores da tabela de pesquisa são alterados com muita frequência, se nunca.

Poderíamos armazenar em cache os dados de `Languages` usando as mesmas técnicas de carregamento reativa examinadas nos tutoriais anteriores. O carregamento reativo, no entanto, usa uma expiração baseada em tempo, que não é necessária para dados de tabela de pesquisa estática. Embora o cache usando o carregamento reativo seja melhor do que nenhum cache, a melhor abordagem seria carregar proativamente os dados da tabela de pesquisa no cache na inicialização do aplicativo.

Neste tutorial, veremos como armazenar em cache os dados da tabela de pesquisa e outras informações estáticas.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Etapa 2: examinando as diferentes maneiras de armazenar dados em cache

As informações podem ser armazenadas em cache programaticamente em um aplicativo ASP.NET usando uma variedade de abordagens. Já vimos como usar o cache de dados nos tutoriais anteriores. Como alternativa, os objetos podem ser armazenados em cache programaticamente usando *membros estáticos* ou o *estado do aplicativo*.

Ao trabalhar com uma classe, normalmente a classe deve primeiro ser instanciada antes que seus membros possam ser acessados. Por exemplo, para invocar um método de uma das classes em nossa camada de lógica de negócios, devemos primeiro criar uma instância da classe:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample1.cs)]

Antes que possamos invocar *SomeMethod* ou trabalhar com *algumaproperty*, devemos primeiro criar uma instância da classe usando a palavra-chave `new`. *SomeMethod* e *SomeProperty* são associados a uma instância específica. O tempo de vida desses membros está vinculado ao tempo de vida de seu objeto associado. Os *membros estáticos*, por outro lado, são variáveis, propriedades e métodos que são compartilhados entre *todas as* instâncias da classe e, consequentemente, têm um tempo de vida tão longo quanto a classe. Os membros estáticos são indicados pela palavra-chave `static`.

Além dos membros estáticos, os dados podem ser armazenados em cache usando o estado do aplicativo. Cada aplicativo ASP.NET mantém uma coleção de nome/valor que é compartilhada entre todos os usuários e páginas do aplicativo. Essa coleção pode ser acessada usando a [propriedade`Application`](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)da [classe`HttpContext`](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)e usada de uma classe code-behind da página ASP.NET da seguinte forma:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample2.cs)]

O cache de dados fornece uma API muito mais rica para armazenar dados em cache, fornecendo mecanismos para expirações baseadas em tempo e dependência, prioridades de item de cache e assim por diante. Com membros estáticos e o estado do aplicativo, esses recursos devem ser adicionados manualmente pelo desenvolvedor da página. No entanto, ao armazenar dados em cache na inicialização do aplicativo durante o tempo de vida do aplicativo, as vantagens do cache de dados são sentido. Neste tutorial, veremos o código que usa todas as três técnicas para armazenar em cache dados estáticos.

## <a name="step-3-caching-thesupplierstable-data"></a>Etapa 3: armazenar em cache os dados da tabela de`Suppliers`

As tabelas de banco de dados Northwind que foram implementadas na data não incluem nenhuma tabela de pesquisa tradicional. As quatro tabelas de tabela implementadas em nossa DAL todos os modelos cujos valores são não estáticos. Em vez de gastar tempo para adicionar uma nova DataTable à DAL e, depois, uma nova classe e métodos para a BLL, para este tutorial, vamos fingir que os dados da tabela `Suppliers` são estáticos. Portanto, poderíamos armazenar esses dados em cache na inicialização do aplicativo.

Para começar, crie uma nova classe chamada `StaticCache.cs` na pasta `CL`.

![Criar a classe StaticCache.cs na pasta CL](caching-data-at-application-startup-cs/_static/image2.png)

**Figura 2**: criar a classe de `StaticCache.cs` na pasta `CL`

Precisamos adicionar um método que carregue os dados na inicialização no armazenamento de cache apropriado, bem como métodos que retornam dados desse cache.

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample3.cs)]

O código acima usa uma variável de membro estático, `suppliers`, para manter os resultados do método `GetSuppliers()` da classe `SuppliersBLL`, que é chamado a partir do método `LoadStaticCache()`. O método `LoadStaticCache()` deve ser chamado durante o início do aplicativo. Depois que esses dados tiverem sido carregados na inicialização do aplicativo, qualquer página que precise trabalhar com dados do fornecedor poderá chamar o método `GetSuppliers()` da classe de `StaticCache`. Portanto, a chamada para o banco de dados para obter os fornecedores ocorre apenas uma vez, no início do aplicativo.

Em vez de usar uma variável de membro estático como o armazenamento de cache, poderíamos ter usado o estado do aplicativo ou o cache de dados como alternativa. O código a seguir mostra a classe referramenta para usar o estado do aplicativo:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample4.cs)]

No `LoadStaticCache()`, as informações do fornecedor são armazenadas na *chave*de variável do aplicativo. Ele é retornado como o tipo apropriado (`Northwind.SuppliersDataTable`) de `GetSuppliers()`. Embora o estado do aplicativo possa ser acessado nas classes code-behind das páginas ASP.NET usando `Application["key"]`, na arquitetura, devemos usar `HttpContext.Current.Application["key"]` para obter a `HttpContext`atual.

Da mesma forma, o cache de dados pode ser usado como um armazenamento de cache, como mostra o código a seguir:

[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample5.cs)]

Para adicionar um item ao cache de dados sem expiração baseada em tempo, use os valores `System.Web.Caching.Cache.NoAbsoluteExpiration` e `System.Web.Caching.Cache.NoSlidingExpiration` como parâmetros de entrada. Essa sobrecarga específica do método `Insert` do cache de dados foi selecionada para que possamos especificar a *prioridade* do item de cache. A prioridade é usada para determinar quais itens serão limpos do cache quando a memória disponível for baixa. Aqui, usamos o `NotRemovable`de prioridade, que garante que esse item de cache não será eliminado.

> [!NOTE]
> O download deste tutorial implementa a classe `StaticCache` usando a abordagem de variável de membro estático. O código para as técnicas de estado do aplicativo e cache de dados está disponível nos comentários no arquivo de classe.

## <a name="step-4-executing-code-at-application-startup"></a>Etapa 4: executando o código na inicialização do aplicativo

Para executar o código quando um aplicativo Web é iniciado pela primeira vez, precisamos criar um arquivo especial chamado `Global.asax`. Esse arquivo pode conter manipuladores de eventos para eventos de nível de aplicativo, sessão e solicitação, e aqui está aqui onde podemos adicionar código que será executado sempre que o aplicativo for iniciado.

Adicione o arquivo de `Global.asax` ao diretório raiz do seu aplicativo Web clicando com o botão direito do mouse no nome do projeto de site na Gerenciador de Soluções do Visual Studio e escolhendo Adicionar novo item. Na caixa de diálogo Adicionar novo item, selecione o tipo de item classe de aplicativo global e, em seguida, clique no botão Adicionar.

> [!NOTE]
> Se você já tiver um arquivo de `Global.asax` em seu projeto, o tipo de item de classe de aplicativo global não será listado na caixa de diálogo Adicionar novo item.

[![adicionar o arquivo global. asax ao diretório raiz do seu aplicativo Web](caching-data-at-application-startup-cs/_static/image4.png)](caching-data-at-application-startup-cs/_static/image3.png)

**Figura 3**: Adicionar o arquivo de `Global.asax` ao diretório raiz do seu aplicativo Web ([clique para exibir a imagem em tamanho normal](caching-data-at-application-startup-cs/_static/image5.png))

O modelo de arquivo de `Global.asax` padrão inclui cinco métodos em uma marca de `<script>` do lado do servidor:

- **`Application_Start`** é executado quando o aplicativo Web é iniciado pela primeira vez
- **`Application_End`** é executado quando o aplicativo está sendo desligado
- **`Application_Error`** é executado sempre que uma exceção sem tratamento atinge o aplicativo
- **`Session_Start`** é executado quando uma nova sessão é criada
- **`Session_End`** é executado quando uma sessão está expirada ou abandonada

O manipulador de eventos `Application_Start` é chamado apenas uma vez durante o ciclo de vida de um aplicativo. O aplicativo é iniciado na primeira vez que um recurso ASP.NET é solicitado do aplicativo e continua a ser executado até que o aplicativo seja reiniciado, o que pode acontecer modificando o conteúdo da pasta `/Bin`, modificando `Global.asax`, modificando o conteúdo na pasta `App_Code` ou modificando o arquivo de `Web.config`, entre outras causas. Consulte [visão geral do ciclo de vida do aplicativo ASP.net](https://msdn.microsoft.com/library/ms178473.aspx) para obter uma discussão mais detalhada sobre o ciclo de vida do aplicativo.

Para esses tutoriais, precisamos apenas adicionar código ao método `Application_Start`, portanto, sinta-se à vontade para remover os outros. Em `Application_Start`, basta chamar o método de `LoadStaticCache()` da classe `StaticCache`, que carregará e armazenará em cache as informações do fornecedor:

[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample6.aspx)]

Isso é tudo! Na inicialização do aplicativo, o método `LoadStaticCache()` obterá as informações do fornecedor da BLL e a armazenará em uma variável de membro estático (ou em qualquer armazenamento de cache que você tenha terminado usando a classe `StaticCache`). Para verificar esse comportamento, defina um ponto de interrupção no método `Application_Start` e execute o aplicativo. Observe que o ponto de interrupção é atingido no início do aplicativo. As solicitações subsequentes, no entanto, não fazem com que o método `Application_Start` seja executado.

[![usar um ponto de interrupção para verificar se o manipulador de eventos Application_Start está sendo executado](caching-data-at-application-startup-cs/_static/image7.png)](caching-data-at-application-startup-cs/_static/image6.png)

**Figura 4**: usar um ponto de interrupção para verificar se o manipulador de eventos `Application_Start` está sendo executado ([clique para exibir a imagem em tamanho normal](caching-data-at-application-startup-cs/_static/image8.png))

> [!NOTE]
> Se você não atingir o ponto de interrupção `Application_Start` quando iniciar a depuração pela primeira vez, isso ocorrerá porque seu aplicativo já foi iniciado. Force a reinicialização do aplicativo modificando seus arquivos `Global.asax` ou `Web.config` e tente novamente. Você pode simplesmente adicionar (ou remover) uma linha em branco no final de um desses arquivos para reiniciar rapidamente o aplicativo.

## <a name="step-5-displaying-the-cached-data"></a>Etapa 5: exibindo os dados armazenados em cache

Neste ponto, a classe `StaticCache` tem uma versão dos dados do fornecedor em cache na inicialização do aplicativo que pode ser acessada por meio de seu método `GetSuppliers()`. Para trabalhar com esses dados da camada de apresentação, podemos usar um ObjectDataSource ou invocar programaticamente o método `GetSuppliers()` da classe de `StaticCache` da classe code-behind de uma página ASP.NET. Vejamos como usar os controles ObjectDataSource e GridView para exibir as informações do fornecedor em cache.

Comece abrindo a página de `AtApplicationStartup.aspx` na pasta `Caching`. Arraste um GridView da caixa de ferramentas para o designer, definindo sua propriedade `ID` como `Suppliers`. Em seguida, na marca inteligente do GridView, escolha criar um novo ObjectDataSource chamado `SuppliersCachedDataSource`. Configure o ObjectDataSource para usar o método `GetSuppliers()` da classe `StaticCache`.

[![configurar o ObjectDataSource para usar a classe StaticCache](caching-data-at-application-startup-cs/_static/image10.png)](caching-data-at-application-startup-cs/_static/image9.png)

**Figura 5**: configurar o ObjectDataSource para usar a classe `StaticCache` ([clique para exibir a imagem em tamanho normal](caching-data-at-application-startup-cs/_static/image11.png))

[![usar o método getsuppliers () para recuperar os dados do fornecedor em cache](caching-data-at-application-startup-cs/_static/image13.png)](caching-data-at-application-startup-cs/_static/image12.png)

**Figura 6**: usar o método `GetSuppliers()` para recuperar os dados do fornecedor em cache ([clique para exibir a imagem em tamanho normal](caching-data-at-application-startup-cs/_static/image14.png))

Depois de concluir o assistente, o Visual Studio adicionará o BoundFields automaticamente para cada um dos campos de dados em `SuppliersDataTable`. A marcação declarativa de GridView e ObjectDataSource deve ser semelhante ao seguinte:

[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample7.aspx)]

A Figura 7 mostra a página quando exibida por meio de um navegador. A saída é a mesma que recebemos os dados da classe `SuppliersBLL` da BLL, mas usar a classe `StaticCache` retorna os dados do fornecedor como armazenados em cache na inicialização do aplicativo. Você pode definir pontos de interrupção no método `GetSuppliers()` da classe `StaticCache` para verificar esse comportamento.

[![os dados do fornecedor em cache são exibidos em um GridView](caching-data-at-application-startup-cs/_static/image16.png)](caching-data-at-application-startup-cs/_static/image15.png)

**Figura 7**: os dados do fornecedor em cache são exibidos em um GridView ([clique para exibir a imagem em tamanho normal](caching-data-at-application-startup-cs/_static/image17.png))

## <a name="summary"></a>Resumo

A maioria de cada modelo de dados contém uma quantidade justa de dados estáticos, geralmente implementados na forma de tabelas de pesquisa. Como essas informações são estáticas, não há motivo para acessar continuamente o banco de dados sempre que essas informações precisam ser exibidas. Além disso, devido à sua natureza estática, ao armazenar os dados em cache, não há necessidade de expiração. Neste tutorial, vimos como pegar esses dados e armazená-los no cache de dados, no estado do aplicativo e por meio de uma variável de membro estático. Essas informações são armazenadas em cache na inicialização do aplicativo e permanecem no cache durante todo o tempo de vida do aplicativo.

Neste tutorial e nos últimos dois, examinamos os dados de cache durante a duração do tempo de vida do aplicativo, bem como usando expirações baseadas em tempo. No entanto, ao armazenar em cache os dados de banco, uma expiração baseada em tempo pode ser menor do que o ideal. Em vez de liberar periodicamente o cache, seria ideal apenas remover o item armazenado em cache quando os dados subjacentes do banco de dados forem modificados. Esse ideal é possível por meio do uso de dependências de cache do SQL, que vamos examinar em nosso próximo tutorial.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP. net e fundador da [4guysfromrolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 2,0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser acessado em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial foram Teresa Murphy e Zack Jones. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](caching-data-in-the-architecture-cs.md)
> [Próximo](using-sql-cache-dependencies-cs.md)
