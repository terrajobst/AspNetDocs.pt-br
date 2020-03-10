---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Caching | Microsoft Docs
author: microsoft
description: Uma compreensão do cache é importante para um aplicativo ASP.NET com bom desempenho. O ASP.NET 1. x oferecia três opções diferentes para o cache; ,... de cache de saída
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 4f0b021ca6ca151544dd9fb0587ed9e0cf14ff65
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575932"
---
# <a name="caching"></a>Cache

pela [Microsoft](https://github.com/microsoft)

> Uma compreensão do cache é importante para um aplicativo ASP.NET com bom desempenho. O ASP.NET 1. x oferecia três opções diferentes para o cache; cache de saída, cache de fragmento e a API de cache.

Uma compreensão do cache é importante para um aplicativo ASP.NET com bom desempenho. O ASP.NET 1. x oferecia três opções diferentes para o cache; cache de saída, cache de fragmento e a API de cache. O ASP.NET 2,0 oferece todos os três métodos, mas adiciona alguns recursos adicionais significativos. Há várias novas dependências de cache e os desenvolvedores agora também têm a opção de criar dependências de cache personalizadas. A configuração do cache também foi significativamente aprimorada no ASP.NET 2,0.

## <a name="new-features"></a>Novos recursos

## <a name="cache-profiles"></a>Perfis de cache

Os perfis de cache permitem que os desenvolvedores definam configurações de cache específicas que podem ser aplicadas a páginas individuais. Por exemplo, se você tiver algumas páginas que devem expirar do cache após 12 horas, você poderá criar facilmente um perfil de cache que possa ser aplicado a essas páginas. Para adicionar um novo perfil de cache, use a seção &lt;outputCacheSettings&gt; no arquivo de configuração. Por exemplo, abaixo está a configuração de um perfil de cache chamado *Twoday* que configura uma duração de cache de 12 horas.

[!code-xml[Main](caching/samples/sample1.xml)]

Para aplicar esse perfil de cache a uma página específica, use o atributo CacheProfile da diretiva @ OutputCache, conforme mostrado abaixo:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Dependências de cache personalizado

ASP.NET 1. x os desenvolvedores chorei para dependências de cache personalizadas. No ASP.NET 1. x, a classe CacheDependency foi selada, o que impediu os desenvolvedores de derivarem suas próprias classes. No ASP.NET 2,0, essa limitação é removida e os desenvolvedores estão livres para desenvolver suas próprias dependências de cache personalizadas. A classe CacheDependency permite a criação de uma dependência de cache personalizada com base em arquivos, diretórios ou chaves de cache.

Por exemplo, o código a seguir cria uma nova dependência de cache personalizada com base em um arquivo chamado. xml localizado na raiz do aplicativo Web:

[!code-csharp[Main](caching/samples/sample3.cs)]

Nesse cenário, quando o arquivo. xml é alterado, o item armazenado em cache é invalidado.

Também é possível criar uma dependência de cache Personalizada usando chaves de cache. Usando esse método, a remoção da chave de cache invalidará os dados armazenados em cache. O exemplo a seguir ilustra isso:

[!code-csharp[Main](caching/samples/sample4.cs)]

Para invalidar o item que foi inserido acima, basta remover o item que foi inserido no cache para atuar como a chave de cache.

[!code-csharp[Main](caching/samples/sample5.cs)]

Observe que a chave do item que atua como a chave de cache deve ser a mesma que o valor adicionado à matriz de chaves de cache.

## <a name="polling-based-sql-cache-dependenciesalso-called-table-based-dependencies"></a>Dependências do cache SQL baseado em sondagem (também chamadas de dependências baseadas em tabela)

SQL Server 7 e 2000 usam o modelo baseado em sondagem para dependências do cache SQL. O modelo baseado em sondagem usa um gatilho em uma tabela de banco de dados que é disparada quando os dados na tabela são alterados. Esse gatilho atualiza um campo **ChangeId** na tabela de notificação que ASP.NET verifica periodicamente. Se o campo **alterid** tiver sido atualizado, ASP.net saberá que os dados foram alterados e invalidará os dados armazenados em cache.

> [!NOTE]
> SQL Server 2005 também pode usar o modelo baseado em sondagem, mas como o modelo baseado em sondagem não é o modelo mais eficiente, é aconselhável usar um modelo baseado em consulta (discutido posteriormente) com SQL Server 2005.

Para que uma dependência de cache do SQL usando o modelo baseado em sondagem funcione corretamente, as tabelas devem ter notificações habilitadas. Isso pode ser feito programaticamente usando a classe SqlCacheDependencyAdmin ou usando o utilitário ASPNET\_RegSql. exe.

A linha de comando a seguir registra a tabela Products no banco de dados Northwind localizado em uma instância SQL Server chamada *dBASE* para dependência de cache do SQL.

[!code-console[Main](caching/samples/sample6.cmd)]

Veja a seguir uma explicação das opções de linha de comando usadas no comando acima:

| **Opção de linha de comando** | **Finalidade** |
| --- | --- |
| -S *server* | Especifica o nome do servidor. |
| -Ed | Especifica que o banco de dados deve ser habilitado para dependência de cache do SQL. |
| -d *nome do\_do banco de dados* | Especifica o nome do banco de dados que deve ser habilitado para dependência de cache do SQL. |
| -E | Especifica que o ASPNET\_RegSql deve usar a autenticação do Windows ao se conectar ao banco de dados. |
| -et | Especifica que estamos habilitando uma tabela de banco de dados para dependência de cache do SQL. |
| -t *\_nome da tabela* | Especifica o nome da tabela de banco de dados a ser habilitada para a dependência de cache do SQL. |

> [!NOTE]
> Há outras opções disponíveis para o ASPNET\_RegSql. exe. Para obter uma lista completa, execute ASPNET\_RegSql. exe-? de uma linha de comando.

Quando esse comando é executado, as seguintes alterações são feitas no banco de dados SQL Server:

- Uma tabela **SqlCacheTablesForChangeNotification AspNet\_** é adicionada. Esta tabela contém uma linha para cada tabela no banco de dados para o qual uma dependência de cache SQL foi habilitada.
- Os seguintes procedimentos armazenados são criados no banco de dados:

| SqlCachePollingStoredProcedure AspNet\_ | Consulta a tabela AspNet\_SqlCacheTablesForChangeNotification e retorna todas as tabelas que estão habilitadas para dependência de cache SQL e o valor de ChangeId para cada tabela. Esse procedimento armazenado é usado para sondagem para determinar se os dados foram alterados. |
| --- | --- |
| SqlCacheQueryRegisteredTablesStoredProcedure AspNet\_ | Retorna todas as tabelas habilitadas para dependência de cache do SQL consultando a tabela AspNet\_SqlCacheTablesForChangeNotification e retorna todas as tabelas habilitadas para dependência de cache do SQL. |
| SqlCacheRegisterTableStoredProcedure AspNet\_ | Registra uma tabela para dependência de cache do SQL adicionando a entrada necessária na tabela de notificação e adiciona o gatilho. |
| SqlCacheUnRegisterTableStoredProcedure AspNet\_ | Cancela o registro de uma tabela para dependência de cache do SQL removendo a entrada na tabela de notificação e remove o gatilho. |
| SqlCacheUpdateChangeIdStoredProcedure AspNet\_ | Atualiza a tabela de notificação incrementando a ChangeId para a tabela alterada. ASP.NET usa esse valor para determinar se os dados foram alterados. Conforme indicado abaixo, esse procedimento armazenado é executado pelo gatilho criado quando a tabela está habilitada. |

- Um gatilho SQL Server chamado  **_\_nome da tabela_\_AspNet\_SqlCacheNotification\_gatilho** é criado para a tabela. Esse gatilho executa o AspNet\_SqlCacheUpdateChangeIdStoredProcedure quando uma inserção, atualização ou exclusão é executada na tabela.
- Uma função SQL Server chamada **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** é adicionada ao banco de dados.

O **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** SQL Server função tem permissões exec para o ASPNET\_SqlCachePollingStoredProcedure. Para que o modelo de sondagem funcione corretamente, você deve adicionar sua conta de processo ao ASPNET\_ChangeNotification\_função ReceiveNotificationsOnlyAccess. A ferramenta ASPNET\_RegSql. exe não fará isso para você.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Configurando dependências do cache SQL baseado em sondagem

Há várias etapas necessárias para configurar dependências de cache SQL baseadas em sondagem. A primeira etapa é habilitar o banco de dados e a tabela conforme discutido acima. Depois que essa etapa for concluída, o restante da configuração será o seguinte:

- Configurando o arquivo de configuração ASP.NET.
- Configurando o SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Configurando o arquivo de configuração ASP.NET

Além de adicionar uma cadeia de conexão, conforme discutido em um módulo anterior, você também deve configurar um elemento de&gt; de cache &lt;com um elemento &lt;sqlCacheDependency&gt;, conforme mostrado abaixo:

[!code-xml[Main](caching/samples/sample7.xml)]

Essa configuração habilita uma dependência de cache do SQL no banco de dados *pubs* . Observe que o atributo PollTime no elemento &lt;sqlCacheDependency&gt; usa como padrão 60000 milissegundos ou 1 minuto. (Esse valor não pode ser inferior a 500 milissegundos.) Neste exemplo, o elemento &lt;adicionar&gt; adiciona um novo banco de dados e substitui o pollTime, definindo-o como 9 milhões milissegundos.

#### <a name="configuring-the-sqlcachedependency"></a>Configurando o SqlCacheDependency

A próxima etapa é configurar o SqlCacheDependency. A maneira mais fácil de fazer isso é especificar o valor para o atributo SqlDependency na diretiva @ outcache da seguinte maneira:

[!code-aspx[Main](caching/samples/sample8.aspx)]

Na diretiva @ OutputCache acima, uma dependência de cache do SQL é configurada para a tabela *autores* no banco de dados *pubs* . Várias dependências podem ser configuradas separando-as com um ponto e vírgula como este:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Outro método de configurar o SqlCacheDependency é fazer isso programaticamente. O código a seguir cria uma nova dependência de cache do SQL na tabela *autores* no banco de dados *pubs* .

[!code-csharp[Main](caching/samples/sample10.cs)]

Um dos benefícios de definir programaticamente a dependência do cache do SQL é que você pode manipular quaisquer exceções que possam ocorrer. Por exemplo, se você tentar definir uma dependência de cache do SQL para um banco de dados que não foi habilitado para notificação, uma exceção **DatabaseNotEnabledForNotificationException** será lançada. Nesse caso, você pode tentar habilitar o banco de dados para notificações chamando o método **SqlCacheDependencyAdmin. EnableNotifications** e passando-o o nome do banco de dados.

Da mesma forma, se você tentar definir uma dependência de cache do SQL para uma tabela que não foi habilitada para notificação, um **TableNotEnabledForNotificationException** será gerado. Em seguida, você pode chamar o método **SqlCacheDependencyAdmin. EnableTableForNotifications** passando o nome do banco de dados e o nome da tabela.

O exemplo de código a seguir ilustra como configurar corretamente o tratamento de exceções ao configurar uma dependência de cache do SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Mais informações: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Dependências de cache SQL baseadas em consulta (somente SQL Server 2005)

Ao usar SQL Server 2005 para dependência de cache do SQL, o modelo baseado em sondagem não é necessário. Quando usado com SQL Server 2005, as dependências de cache do SQL se comunicam diretamente por meio de conexões SQL com a instância de SQL Server (nenhuma configuração adicional é necessária) usando notificações de consulta SQL Server 2005.

A maneira mais simples de habilitar a notificação baseada em consulta é fazer isso declarativamente definindo o atributo **SqlCacheDependency** do objeto de fonte de dados como **CommandNotification** e definindo o atributo **EnableCaching** como **true**. Usando esse método, nenhum código é necessário. Se o resultado de um comando executado em relação à fonte de dados for alterado, ele invalidará os dados de cache.

O exemplo a seguir configura um controle da fonte de dados para a dependência de cache do SQL:

[!code-aspx[Main](caching/samples/sample12.aspx)]

Nesse caso, se a consulta especificada no **SelectCommand** retornar um resultado diferente do que era originalmente, os resultados armazenados em cache serão invalidados.

Você também pode especificar que todas as suas fontes de dados sejam habilitadas para dependências do cache SQL definindo o atributo **SqlDependency** da diretiva **@ OutputCache** como **CommandNotification**. O exemplo a seguir ilustra isso.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Para obter mais informações sobre notificações de consulta no SQL Server 2005, consulte o Manuais Online do SQL Server.

Outro método de configurar uma dependência de cache do SQL baseada em consulta é fazer isso programaticamente usando a classe SqlCacheDependency. O exemplo de código a seguir ilustra como isso é feito.

[!code-csharp[Main](caching/samples/sample14.cs)]

Mais informações: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Substituição pós-cache

Armazenar em cache uma página pode aumentar drasticamente o desempenho de um aplicativo Web. No entanto, em alguns casos, você precisa da maior parte da página para ser armazenada em cache e alguns fragmentos dentro da página são dinâmicos. Por exemplo, se você criar uma página de histórias de notícias que seja totalmente estática por períodos de tempo definidos, poderá definir a página inteira a ser armazenada em cache. Se você quisesse incluir uma faixa do anúncio em rotação que foi alterada em cada solicitação de página, a parte da página que contém o anúncio precisa ser dinâmica. Para permitir que você armazene em cache uma página, mas substitua um conteúdo dinamicamente, você pode usar a substituição pós-cache ASP.NET. Com a substituição pós-cache, toda a página é armazenada em cache de saída com partes específicas marcadas como isentas do cache. No exemplo das faixas do AD, o controle AdRotator permite que você aproveite a substituição após o cache para que os anúncios criados dinamicamente para cada usuário e para cada página sejam atualizados.

Há três maneiras de implementar a substituição após o cache:

- Declarativamente, usando o controle Substitution.
- Programaticamente, usando a API de controle de substituição.
- Implicitamente, usando o controle AdRotator.

### <a name="substitution-control"></a>Controle de substituição

O controle de substituição ASP.NET especifica uma seção de uma página armazenada em cache que é criada dinamicamente em vez de em cache. Coloque um controle de substituição no local da página em que você deseja que o conteúdo dinâmico apareça. Em tempo de execução, o controle de substituição chama um método que você especifica com a Propriedade MethodName. O método deve retornar uma cadeia de caracteres, que substitui o conteúdo do controle de substituição. O método deve ser um método estático na página recipiente ou no controle UserControl. Usar o controle de substituição faz com que a cache do lado do cliente seja alterada para a cache do servidor, para que a página não seja armazenada em cache no cliente. Isso garante que as solicitações futuras para a página chamem o método novamente para gerar conteúdo dinâmico.

### <a name="substitution-api"></a>API de substituição

Para criar conteúdo dinâmico para uma página armazenada em cache programaticamente, você pode chamar o método [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) em seu código de página, passando-o para o nome de um método como um parâmetro. O método que manipula a criação do conteúdo dinâmico usa um único parâmetro [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) e retorna uma cadeia de caracteres. A cadeia de caracteres de retorno é o conteúdo que será substituído no local especificado. Uma vantagem de chamar o método WriteSubstitution em vez de usar o controle Substitution declarativamente é que você pode chamar um método de qualquer objeto arbitrário em vez de chamar um método estático da página ou do objeto UserControl.

Chamar o método WriteSubstitution faz com que a cache do lado do cliente seja alterada para a cache do servidor, para que a página não seja armazenada em cache no cliente. Isso garante que as solicitações futuras para a página chamem o método novamente para gerar conteúdo dinâmico.

### <a name="adrotator-control"></a>Controle AdRotator

O controle de servidor AdRotator implementa o suporte para a substituição pós-cache internamente. Se você colocar um controle AdRotator em sua página, ele processará anúncios exclusivos em cada solicitação, independentemente de a página pai ser armazenada em cache. Como resultado, uma página que inclui um controle AdRotator só é armazenada em cache no lado do servidor.

## <a name="controlcachepolicy-class"></a>Classe ControlCachePolicy

A classe ControlCachePolicy permite o controle programático do cache de fragmento usando controles de usuário. ASP.NET insere controles de usuário em uma instância de [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) . A classe BasePartialCachingControl representa um controle de usuário que tem o cache de saída habilitado.

Ao acessar a propriedade [BasePartialCachingControl. CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) de um controle [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) , você sempre receberá um objeto ControlCachePolicy válido. No entanto, se você acessar a propriedade [UserControl. CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) de um controle [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) , você receberá um objeto ControlCachePolicy válido somente se o controle de usuário já estiver encapsulado por um controle BasePartialCachingControl. Se não estiver encapsulado, o objeto ControlCachePolicy retornado pela propriedade gerará exceções quando você tentar manipulá-lo porque ele não tem um BasePartialCachingControl associado. Para determinar se uma instância de UserControl dá suporte ao Caching sem gerar exceções, inspecione a propriedade [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) .

Usar a classe ControlCachePolicy é uma das várias maneiras que você pode habilitar o cache de saída. A lista a seguir descreve os métodos que você pode usar para habilitar o cache de saída:

- Use a diretiva [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) para habilitar o cache de saída em cenários declarativos.
- Use o atributo [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) para habilitar o cache para um controle de usuário em um arquivo code-behind.
- Use a classe ControlCachePolicy para especificar configurações de cache em cenários programáticos nos quais você está trabalhando com instâncias do BasePartialCachingControl que foram habilitadas para cache usando um dos métodos anteriores e carregados dinamicamente usando o método [System. Web. UI. TemplateControl. GetControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) .

Uma instância de ControlCachePolicy pode ser manipulada com êxito somente entre os estágios init e PreRender do ciclo de vida do controle. Se você modificar um objeto ControlCachePolicy após a fase PreRender, o ASP.NET lançará uma exceção, pois as alterações feitas após o controle ser renderizado não poderão realmente afetar as configurações de cache (um controle é armazenado em cache durante o estágio de renderização). Por fim, uma instância de controle de usuário (e, portanto, seu objeto ControlCachePolicy) só está disponível para manipulação programática quando é realmente renderizada.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Alterações na configuração de cache-o elemento de&gt; de cache &lt;

Há várias alterações na configuração de cache no ASP.NET 2,0. O elemento&gt; de cache &lt;é novo no ASP.NET 2,0 e permite que você faça alterações de configuração de cache no arquivo de configuração. Os atributos a seguir estão disponíveis.

| **Element** | **Descrição** |
| --- | --- |
| **armazenar** | Elemento opcional. Define as configurações do cache de aplicativo global. |
| **outputCache** | Elemento opcional. Especifica as configurações de cache de saída de todo o aplicativo. |
| **outputCacheSettings** | Elemento opcional. Especifica as configurações de cache de saída que podem ser aplicadas a páginas no aplicativo. |
| **sqlCacheDependency** | Elemento opcional. Configura as dependências de cache do SQL para um aplicativo ASP.NET. |

### <a name="the-ltcachegt-element"></a>O elemento&gt; do cache &lt;

Os atributos a seguir estão disponíveis no elemento&gt; do cache de &lt;:

| **Atributo** | **Descrição** |
| --- | --- |
| **disableMemoryCollection** | Atributo **Boolean** opcional. Obtém ou define um valor que indica se a coleção de memória de cache que ocorre quando a máquina está sob pressão de memória está desabilitada. |
| **disableExpiration** | Atributo **Boolean** opcional. Obtém ou define um valor que indica se a expiração do cache está desabilitada. Quando desabilitado, os itens armazenados em cache não expiram e a limpeza em segundo plano dos itens de cache expirados não ocorre. |
| **privateBytesLimit** | Atributo **Int64** opcional. Obtém ou define um valor que indica o tamanho máximo dos bytes particulares de um aplicativo antes de o cache começar a liberar os itens expirados e tentar recuperar memória. Esse limite inclui a memória usada pelo cache, bem como a sobrecarga de memória normal do aplicativo em execução. Uma configuração de zero indica que o ASP.NET usará sua própria heurística para determinar quando começar a recuperar a memória. |
| **percentagePhysicalMemoryUsedLimit** | Atributo **Int32** opcional. Obtém ou define um valor que indica a porcentagem máxima da memória física de um computador que pode ser consumida por um aplicativo antes que o cache comece a liberar itens expirados e tentando recuperar memória esse uso de memória também inclui a memória usada pelo cache como o uso de memória normal do aplicativo em execução. Uma configuração de zero indica que o ASP.NET usará sua própria heurística para determinar quando começar a recuperar a memória. |
| **privateBytesPollTime** | Atributo **TimeSpan** opcional. Obtém ou define um valor que indica o intervalo de tempo entre a sondagem para o uso de memória de bytes particulares do aplicativo. |

### <a name="the-ltoutputcachegt-element"></a>O elemento&gt; &lt;outputCache

Os atributos a seguir estão disponíveis para o elemento &lt;outputCache&gt;.

|       <strong>Atributo</strong>        |                                                                                                                                                                                                                                                       <strong>Descrição</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          Atributo <strong>Boolean</strong> opcional. Habilita/desabilita o cache de saída de página. Se desabilitado, nenhuma página será armazenada em cache independentemente das configurações programáticas ou declarativas. O valor padrão é <strong>true</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                Atributo <strong>Boolean</strong> opcional. Habilita/desabilita o cache de fragmento de aplicativo. Se desabilitado, nenhuma página será armazenada em cache, independentemente da diretiva [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) ou do perfil de cache usado. Inclui um cabeçalho Cache-Control que indica que os servidores proxy upstream, bem como os clientes de navegador, não devem tentar armazenar em cache a saída da página. O valor padrão é <strong>false</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      Atributo <strong>Boolean</strong> opcional. Obtém ou define um valor que indica se o cabeçalho <strong>Cache-Control: Private</strong> é enviado pelo módulo cache de saída por padrão. O valor padrão é <strong>false</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | Atributo <strong>Boolean</strong> opcional. Habilita/desabilita o envio de um cabeçalho http "<strong>variar: \</strong ><em>" na resposta. Com a configuração padrão de false, um cabeçalho "</em>* Vary: \*<strong>" é enviado para páginas armazenadas em cache de saída. Quando o cabeçalho Vary é enviado, ele permite que versões diferentes sejam armazenadas em cache com base no que está especificado no cabeçalho Vary. Por exemplo, <em>Vary: os agentes de usuário</em> armazenarão versões diferentes de uma página com base no agente do usuário que emite a solicitação. O valor padrão é * * false</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>O elemento &lt;outputCacheSettings&gt;

O elemento &lt;outputCacheSettings&gt; permite a criação de perfis de cache conforme descrito anteriormente. O único elemento filho para o elemento &lt;outputCacheSettings&gt; é o elemento &lt;&gt; de outputCacheProfiles para configurar perfis de cache.

### <a name="the-ltsqlcachedependencygt-element"></a>O elemento de&gt; &lt;sqlCacheDependency

Os atributos a seguir estão disponíveis para o elemento &lt;sqlCacheDependency&gt;.

| **Atributo** | **Descrição** |
| --- | --- |
| **habilitado** | Atributo **booliano** necessário. Indica se as alterações estão sendo sondadas ou não. |
| **pollTime** | Atributo **Int32** opcional. Define a frequência com a qual o SqlCacheDependency sonda a tabela de banco de dados para alterações. Esse valor corresponde ao número de milissegundos entre sondagens sucessivas. Ele não pode ser definido para menos de 500 milissegundos. O valor padrão é 1 minuto. |

### <a name="more-information"></a>Mais informações

Há algumas informações adicionais das quais você deve estar atento em relação à configuração de cache.

- Se o limite de bytes particulares do processo de trabalho não for definido, o cache usará um dos seguintes limites: 

    - x86 GB: 800 MB ou 60% da RAM física, o que for menor
    - x86 3GB: 1800MB ou 60% da RAM física, o que for menor
    - x64:1 terabyte ou 60% da RAM física, o que for menor
- Se o limite de bytes particulares do processo de trabalho &lt;e o privateBytesLimit/&gt; do cache estiverem definidos, o cache usará o mínimo dos dois.
- Assim como no 1. x, descartamos as entradas de cache e chamamos o GC. Coletar por dois motivos: 

    - Estamos muito próximos do limite de bytes particulares
    - A memória disponível é próxima ou menor que 10%
- Você pode desabilitar efetivamente o corte e o cache para condições de memória baixa disponíveis definindo &lt;cache percentagePhysicalMemoryUseLimit/&gt; como 100.
- Ao contrário de 1. x, 2,0 suspenderá o corte e coletará chamadas se o último GC. A coleta não reduziu os bytes privados ou o tamanho dos heaps gerenciados por mais de 1% do limite de memória (cache).

## <a name="lab1-custom-cache-dependencies"></a>Lab1: dependências de cache personalizadas

1. Crie um novo site da Web.
2. Adicione um novo arquivo XML chamado cache. xml e salve-o na raiz do aplicativo Web.
3. Adicione o código a seguir à página\_método Load no code-behind de default. aspx: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Adicione o seguinte à parte superior de default. aspx na exibição de código-fonte: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Procure default. aspx. O que o tempo diz?
6. Atualize o navegador. O que o tempo diz?
7. Abra o cache. xml e adicione o seguinte código: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Salve cache. xml.
9. Atualize seu navegador. O que o tempo diz?
10. Explique por que o tempo foi atualizado em vez de exibir os valores armazenados em cache anteriormente:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Laboratório 2: usando dependências de cache baseadas em sondagem

Este laboratório usa o projeto que você criou no módulo anterior que permite a edição de dados no banco de dados Northwind por meio de um controle GridView e DetailsView.

1. Abra o projeto no Visual Studio 2005.
2. Execute o utilitário ASPNET\_RegSql no banco de dados Northwind para habilitar o banco de dados e a tabela Products. Use o seguinte comando em um prompt de comando do Visual Studio: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Adicione o seguinte ao seu arquivo Web. config: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Adicione um novo formulário da WebForms chamado defaultdata. aspx.
5. Adicione a seguinte diretiva @ OutputCache à página defaultdata. aspx: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Adicione o código a seguir à página\_carregar o defaultdata. aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Adicione um novo controle SqlDataSource a defaultdata. aspx e configure-o para usar a conexão de banco de dados Northwind. Clique em Avançar.
8. Marque as caixas de seleção NomeDoProduto e ProductID e clique em Avançar.
9. Clique em Concluir.
10. Adicione um novo GridView à página de dados. aspx.
11. Escolha SqlDataSource1 na lista suspensa.
12. Salve e procure nadata. aspx. Anote o horário exibido.
