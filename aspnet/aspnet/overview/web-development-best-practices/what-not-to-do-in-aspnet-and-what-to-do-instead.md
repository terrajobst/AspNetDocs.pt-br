---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: O que não fazer em ASP.NET e o que fazer em vez disso | Microsoft Docs
author: Rick-Anderson
description: Este tópico descreve vários erros comuns que as pessoas fazem em projetos da Web do ASP.NET. Ele fornece recomendações sobre o que você deve fazer para evitar esses comentários...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 980d3544df70643043391e6573803ce21b3a824f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616987"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a>O que fazer e o que não fazer em ASP.NET

> Este tópico descreve vários erros comuns que as pessoas fazem em projetos da Web do ASP.NET. Ele fornece recomendações para o que você deve fazer para evitar esses erros comuns. Ele se baseia em uma [apresentação](http://vimeo.com/68390507) da **Damian Edwards** na conferência de desenvolvedores de norueguês.

## <a name="disclaimer"></a>Isenção de responsabilidade

Este tópico não é destinado a um guia completo para garantir que seu aplicativo seja seguro e eficiente. Você ainda precisa seguir as práticas recomendadas de segurança e desempenho que não estão descritos neste tópico. Ele apenas sugere como evitar erros comuns relacionados a processos e classes do .NET.

## <a name="overview"></a>Visão geral

Este tópico contém as seguintes seções:

- [Conformidade com os padrões](#standards)

    - [Adaptadores de controle](#adapters)
    - [Propriedades de estilo em controles](#styleprop)
    - [Retornos de chamada de página e controle](#callback)
    - [Detecção de capacidade do navegador](#browsercap)
- [Segurança](#security)

    - [Validação de solicitação](#validation)
    - [Autenticação e sessão de formulários sem cookie](#cookieless)
    - [EnableViewStateMac](#viewstatemac)
    - [Confiança média](#medium)
    - [&lt;appSettings&gt;](#appsettings)
    - [UrlPathEncode](#urlpathencode)
- [Confiabilidade e desempenho](#performance)

    - [PreSendRequestHeaders e PreSendRequestContent](#presend)
    - [Eventos de página assíncrona com Web Forms](#asyncevents)
    - [Trabalho de disparar e esquecer](#fire)
    - [Corpo da entidade de solicitação](#requestentity)
    - [Response. redirecionamento e resposta. end](#redirect)
    - [EnableViewState e ViewStatemode](#viewstatemode)
    - [SqlMembershipProvider](#sqlprovider)
    - [Solicitações de execução longa (> 110 segundos)](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a>Conformidade com os padrões

<a id="adapters"></a>

### <a name="control-adapters"></a>Adaptadores de controle

Recomendação: Pare de usar adaptadores de controle para processamento adaptável e, em vez disso, use consultas de mídia CSS e HTML compatível com padrões.

Os adaptadores de controles foram introduzidos no .NET 2,0 para renderizar o código de apresentação que foi personalizado para diferentes dispositivos e ambientes. Agora, essa renderização adaptável pode ser realizada com CSS e HTML. Você deve parar de usar os adaptadores de controle e converter quaisquer adaptadores existentes em CSS e HTML.

Para obter mais informações, consulte [consultas de mídia](http://www.w3.org/TR/css3-mediaqueries/) e [como adicionar páginas móveis ao seu aplicativo ASP.NET Web Forms/MVC](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a>Propriedades de estilo em controles

Recomendação: Pare a configuração de valores de estilo na marcação de controle e, em vez disso, defina valores de formatação em folhas de estilos CSS.

Os controles de servidor Web contêm dezenas de propriedades que podem ser usadas para definir propriedades de estilo na linha. Por exemplo, a propriedade ForeColor define a cor do texto de um controle. Você pode realizar esse mesmo efeito com mais eficiência por meio de folhas de estilo CSS. As folhas de estilos permitem centralizar valores de estilo e evitar definir esses valores em todo o aplicativo.

O exemplo a seguir mostra uma classe CSS que define o texto como vermelho.

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

O exemplo a seguir mostra como aplicar dinamicamente a classe CSS.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a>Retornos de chamada de página e controle

Recomendação: Pare de usar retornos de chamada de página e controle e, em vez disso, use qualquer um dos seguintes: AJAX, UpdatePanel, métodos de ação MVC, API Web ou Signalr.

Em versões anteriores do ASP.NET, os métodos de retorno de chamada Page e Control permitiam que você atualizasse parte da página da Web sem Atualizar uma página inteira. Agora você pode realizar atualizações de página parcial por meio de [Ajax](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API da Web](../../../web-api/index.md) ou [signalr](../../../signalr/index.md). Você deve interromper o uso de métodos de retorno de chamada porque eles podem causar problemas com URLs e roteamento amigáveis. Por padrão, os controles não habilitam métodos de retorno de chamada, mas se você habilitou esse recurso em um controle, você deve desabilitá-lo.

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a>Detecção de capacidade do navegador

Recomendação: Pare de usar a detecção de capacidade de navegador estático e, em vez disso, use a detecção dinâmica de recursos.

Em versões anteriores do ASP.NET, os recursos com suporte para cada navegador foram armazenados em um arquivo XML. A detecção de suporte a recursos por meio de uma pesquisa estática não é a melhor abordagem. Agora, você pode detectar dinamicamente os recursos com suporte do navegador usando uma estrutura de detecção de recursos, como o [Modernizr](http://modernizr.com/). A detecção de recursos determina o suporte ao tentar usar um método ou uma propriedade e, em seguida, verificar se o navegador produziu o resultado desejado. Por padrão, o Modernizr está incluído nos modelos de aplicativos Web.

<a id="security"></a>

## <a name="security"></a>Segurança

<a id="validation"></a>

### <a name="request-validation"></a>Validação de solicitação

Recomendação: validar a entrada do usuário e codificar a saída de usuários.

A validação de solicitação é um recurso de ASP.NET que inspeciona cada solicitação e interrompe a solicitação se uma ameaça percebida for encontrada. Não dependa da validação de solicitação para proteger seu aplicativo contra ataques de script entre sites. Em vez disso, valide todas as entradas de usuários e codifique a saída. Em alguns casos limitados, você pode usar expressões regulares para validar a entrada, mas em casos mais complicados, você deve validar a entrada do usuário usando classes do .NET que determinam se o valor corresponde aos valores permitidos.

O exemplo a seguir mostra como usar um método estático na classe URI para determinar se o URI fornecido por um usuário é válido.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

No entanto, para verificar o URI suficientemente, você também deve verificar se ele especifica `http` ou `https`. O exemplo a seguir usa métodos de instância para verificar se o URI é válido.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

Antes de renderizar a entrada do usuário como HTML ou incluir a entrada do usuário em uma consulta SQL, codifique os valores para garantir que o código mal-intencionado não seja incluído.

Você pode codificar o valor em marcação com a sintaxe &lt;%:%&gt;, conforme mostrado abaixo.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

Ou, em sintaxe Razor, você pode codificar HTML com @, conforme mostrado abaixo.

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

O exemplo a seguir mostra como codificar em HTML um valor no code-behind.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

Para codificar com segurança um valor para comandos SQL, use parâmetros de comando como o [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx). <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a>Autenticação e sessão de formulários sem cookie

Recomendação: exigir cookies.

A passagem de informações de autenticação na cadeia de caracteres de consulta não é segura. Portanto, exija cookies quando seu aplicativo incluir autenticação. Se seu cookie armazena informações confidenciais, considere exigir SSL para o cookie.

O exemplo a seguir mostra como especificar no arquivo Web. config que a autenticação de formulários requer um cookie que é transmitido por SSL.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a>EnableViewStateMac

Recomendação: nunca definido como false.

Por padrão, o EnableViewStateMac é definido como true. Mesmo que seu aplicativo não esteja usando o estado de exibição, não defina o EnableViewStateMac como false. Definir esse valor como false tornará seu aplicativo vulnerável a scripts entre sites.

Começando com ASP.NET 4.5.2, o tempo de execução impõe **EnableViewStateMac = true**. Mesmo se você defini-lo como false, o tempo de execução ignora esse valor e prossegue com o valor definido como true. Para obter mais informações, consulte [ASP.NET 4.5.2 e EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).

O exemplo a seguir mostra como definir o EnableViewStateMac como true. Você não precisa definir esse valor como true, pois ele é true por padrão. No entanto, se você definiu-a como false em qualquer página em seu aplicativo, deverá corrigir esse valor imediatamente.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a>Confiança média

Recomendação: não depende de confiança média (ou qualquer outro nível de confiança) como um limite de segurança.

A confiança parcial não protege adequadamente seu aplicativo e não deve ser usada. Em vez disso, use confiança total e isole aplicativos não confiáveis em pools de aplicativos separados. Além disso, execute cada pool de aplicativos em uma identidade exclusiva. Para obter mais informações, consulte [confiança parcial do ASP.net não garante o isolamento do aplicativo](https://support.microsoft.com/kb/2698981).

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a>&lt;appSettings&gt;

Recomendação: Não desabilite as configurações de segurança no elemento &lt;appSettings&gt;.

O elemento appSettings contém muitos valores que são necessários para atualizações de segurança. Você não deve alterar nem desabilitar esses valores. Se você precisar desabilitar esses valores ao implantar uma atualização, reabilitar imediatamente depois de concluir a implantação.

Para obter detalhes, consulte o [elemento ASP.net appSettings](https://msdn.microsoft.com/library/hh975440.aspx).

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a>UrlPathEncode

Recomendação: em vez disso, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) .

O método UrlPathEncode foi adicionado à .NET Framework para resolver um problema de compatibilidade de navegador muito específico. Ele não codifica uma URL de forma adequada e não protege seu aplicativo contra scripts entre sites. Você nunca deve usá-lo em seu aplicativo. Em vez disso, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).

O exemplo a seguir mostra como passar uma URL codificada como um parâmetro de cadeia de caracteres de consulta para um controle de hiperlink.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a>Confiabilidade e desempenho

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a>PreSendRequestHeaders e PreSendRequestContent

Recomendação: não use esses eventos com módulos gerenciados. Em vez disso, escreva um módulo do IIS nativo para executar a tarefa necessária. Consulte [criando módulos http de código nativo](https://msdn.microsoft.com/library/ms693629.aspx).

Você pode usar os eventos [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) e [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) com módulos nativos do IIS.
> [!WARNING]
> Não use `PreSendRequestHeaders` e `PreSendRequestContent` com módulos gerenciados que implementam `IHttpModule`. A definição dessas propriedades pode causar problemas com solicitações assíncronas. A combinação de roteamento solicitado do aplicativo (ARR) e WebSockets pode levar a exceções de violação de acesso que podem causar falhas em w3wp. Por exemplo, iiscore! W3_CONTEXT_BASE:: GetIsLastNotification + 68 em iiscore. dll causou uma exceção de violação de acesso (0xC0000005).

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a>Eventos de página assíncrona com Web Forms

Recomendação: em Web Forms, evite escrever métodos Async void para eventos de ciclo de vida da página e, em vez disso, use [Page. RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) para código assíncrono.

Ao marcar um evento de página com **Async** e **void**, você não pode determinar quando o código assíncrono foi concluído. Em vez disso, use Page. RegisterAsyncTask para executar o código assíncrono de forma a permitir que você acompanhe sua conclusão.

O exemplo a seguir mostra um manipulador de clique de botão que contém código assíncrono. Este exemplo inclui a leitura de um valor de cadeia de caracteres de forma assíncrona, que é fornecido apenas como um exemplo simplificado de uma tarefa assíncrona e não como uma prática recomendada.

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

Se você estiver usando tarefas assíncronas, defina a estrutura de destino de tempo de execução http como 4,5 (ou posterior) no arquivo Web. config. Definir a estrutura de destino como 4,5 ativa o novo contexto de sincronização que foi adicionado no .NET 4,5. Esse valor é definido por padrão em novos projetos no Visual Studio, mas não será definido se você estiver trabalhando com um projeto existente.

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a>Trabalho de disparar e esquecer

Recomendação: ao lidar com uma solicitação no ASP.NET, evite iniciar o trabalho de incêndio e esquecer (chamando o método ThreadPool. QueueUserWorkItem ou criando um temporizador que chame repetidamente um delegado).

Se seu aplicativo tiver um trabalho de acionamento e esquecer que seja executado no ASP.NET, seu aplicativo poderá ficar fora de sincronia. A qualquer momento, o domínio do aplicativo pode ser destruído, o que significa que o processo em andamento pode não corresponder mais ao estado atual do aplicativo.

Você deve mover esse tipo de trabalho para fora do ASP.NET. Você pode usar trabalhos da Web, serviço do Windows ou uma função de trabalho no Azure para executar o trabalho em andamento e executar esse código a partir de outro processo.

Se for necessário executar esse trabalho em ASP.NET, você poderá adicionar o pacote NuGet chamado [Webbackgrounder](http://www.nuget.org/packages/webbackgrounder) para executar o código.

<a id="requestentity"></a>

### <a name="request-entity-body"></a>Corpo da entidade de solicitação

Recomendação: Evite ler Request. Form ou Request. InputStream antes do evento de execução do manipulador.

O mais antigo que você deve ler de Request. Form ou Request. InputStream é durante o evento de execução do manipulador. No MVC, o controlador é o manipulador e o evento execute é quando o método de ação é executado. Em Web Forms, a página é o manipulador e o evento execute é quando o evento Page. Init é acionado. Se você ler o corpo da entidade de solicitação anterior ao evento de execução, você interferirá no processamento da solicitação.

Se você precisar ler o corpo da entidade de solicitação antes do evento execute, use [Request. GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) ou [Request. GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx). Ao usar o GetBufferlessInputStream, você obtém o fluxo bruto da solicitação e assume a responsabilidade pelo processamento de toda a solicitação. Depois de chamar GetBufferlessInputStream, Request. Form e Request. InputStream não estão disponíveis porque não foram populados pelo ASP.NET. Ao usar o GetBufferedInputStream, você obtém uma cópia do fluxo da solicitação. Request. Form e Request. InputStream ainda estão disponíveis mais tarde na solicitação porque ASP.NET popula a outra cópia.

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a>Response. redirecionamento e resposta. end

Recomendação: esteja atento às diferenças em como o thread é tratado depois [de chamar Response. reredirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).

O método [Response. reredirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) chama o método Response. end. Em um processo síncrono, chamar Request. redirecionar faz com que o thread atual seja anulado imediatamente. No entanto, em um processo assíncrono, chamar Response. redirecionamento não anula o thread atual, portanto, a execução do código continua a solicitação. Em um processo assíncrono, você deve retornar a tarefa do método para interromper a execução do código.

Em um projeto MVC, você não deve chamar Response. redirect. Em vez disso, retorne um RedirectResult.

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a>EnableViewState e ViewStatemode

Recomendação: Use ViewStatemode, em vez de EnableViewState, para fornecer controle granular sobre quais controles usam o estado de exibição.

Quando você define EnableViewState como false na diretiva Page, o estado de exibição é desabilitado para todos os controles dentro da página e não pode ser habilitado. Se você quiser habilitar o estado de exibição somente para determinados controles em sua página, defina ViewStatemode como desabilitado para a página.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

Em seguida, defina ViewStatemode como habilitado somente nos controles que realmente precisam de estado de exibição.

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

Ao habilitar o estado de exibição somente para os controles que precisam dele, você pode reduzir o tamanho do estado de exibição para suas páginas da Web.

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a>SqlMembershipProvider

Recomendação: Use Provedores Universais.

Nos modelos de projeto atuais, o SqlMembershipProvider foi substituído por [provedores universais ASP.net](http://www.nuget.org/packages/Microsoft.AspNet.Providers), que está disponível como um pacote NuGet. Se você estiver usando o SqlMembershipProvider em um projeto criado com uma versão anterior dos modelos, você deverá alternar para Provedores Universais. O Provedores Universais trabalhar com todos os bancos de dados com suporte no Entity Framework.

Para obter mais informações, consulte [introducing provedores universais ASP.net](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a>Solicitações de execução longa (> 110 segundos)

Recomendação: Use o [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) ou O [signalr](../../../signalr/index.md) para clientes conectados e use operações de e/s assíncronas.

Solicitações de execução longa podem causar resultados imprevisíveis e baixo desempenho em seu aplicativo Web. A configuração de tempo limite padrão para uma solicitação é de 110 segundos. Se você estiver usando o estado de sessão com uma solicitação de execução longa, o ASP.NET liberará o bloqueio no objeto de sessão após 110 segundos. No entanto, seu aplicativo pode estar no meio de uma operação no objeto de sessão quando o bloqueio é liberado e a operação pode não ser concluída com êxito. Se uma segunda solicitação do usuário for bloqueada enquanto a primeira solicitação estiver em execução, a segunda solicitação poderá acessar o objeto de sessão em um estado inconsistente.

Se seu aplicativo incluir operações de e/s de bloqueio (ou síncrona), o aplicativo não responderá.

Para melhorar o desempenho, use as operações de e/s assíncronas no .NET Framework. Além disso, use WebSockets ou Signalr para conectar clientes ao servidor. Esses recursos são projetados para lidar com eficiência com as solicitações de longa execução.
