---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
title: Registrando detalhes do erro com o monitoramentoC#de integridade do ASP.net () | Microsoft Docs
author: rick-anderson
description: O sistema de monitoramento de integridade da Microsoft fornece uma maneira fácil e personalizável de registrar vários eventos da Web, incluindo exceções sem tratamento. Este tutorial percorre aos...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: b1abb452-642a-4ff3-8504-37b85590ff79
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
msc.type: authoredcontent
ms.openlocfilehash: e52ed94f78d053701771690fce432d5a1d465b62
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74637299"
---
# <a name="logging-error-details-with-aspnet-health-monitoring-c"></a>Registro em log de detalhes de erros com o monitoramento de integridade do ASP.NET (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_CS.zip) ou [baixar PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_cs.pdf)

> O sistema de monitoramento de integridade da Microsoft fornece uma maneira fácil e personalizável de registrar vários eventos da Web, incluindo exceções sem tratamento. Este tutorial orienta a configuração do sistema de monitoramento de integridade para registrar exceções sem tratamento em um banco de dados e notificar os desenvolvedores por meio de uma mensagem de email.

## <a name="introduction"></a>Introdução

O registro em log é uma ferramenta útil para monitorar a integridade de um aplicativo implantado e para diagnosticar quaisquer problemas que possam surgir. É especialmente importante registrar em log os erros que ocorrem em um aplicativo implantado para que eles possam ser remediados. O evento `Error` é gerado sempre que uma exceção não tratada ocorre em um aplicativo ASP.NET; o [tutorial anterior](processing-unhandled-exceptions-cs.md) mostrou como notificar um desenvolvedor de um erro e registrar seus detalhes criando um manipulador de eventos para o evento `Error`. No entanto, criar um manipulador de eventos `Error` para registrar em log os detalhes do erro e notificar um desenvolvedor é desnecessário, pois essa tarefa pode ser executada pelo ASP. Sistema de *monitoramento de integridade*da rede.

O sistema de monitoramento de integridade foi introduzido no ASP.NET 2,0 e foi projetado para monitorar a integridade de um aplicativo ASP.NET implantado registrando eventos que ocorrem durante o tempo de vida do aplicativo ou da solicitação. Os eventos registrados pelo sistema de monitoramento de integridade são chamados de *eventos de monitoramento de integridade* ou *eventos da Web*, e incluem:

- Eventos de tempo de vida do aplicativo, como quando um aplicativo é iniciado ou interrompido
- Eventos de segurança, incluindo tentativas de logon com falha e solicitações de autorização de URL com falha
- Erros de aplicativo, incluindo exceções sem tratamento, exceções de análise do estado de exibição, exceções de validação de solicitação e erros de compilação, entre outros tipos de erros.

Quando um evento de monitoramento de integridade é gerado, ele pode ser registrado em qualquer número de *fontes de log*especificadas. O sistema de monitoramento de integridade é fornecido com fontes de log que registram eventos da Web em um banco de dados Microsoft SQL Server, no log de eventos do Windows ou por meio de uma mensagem de email, entre outros. Você também pode criar suas próprias fontes de log.

Os eventos que o sistema de monitoramento de integridade registra, juntamente com as fontes de log usadas, são definidos em `Web.config`. Com algumas linhas de marcação de configuração, você pode usar o monitoramento de integridade para registrar em log todas as exceções sem tratamento em um banco de dados e notificá-lo da exceção por email.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Explorando a configuração do sistema de monitoramento de integridade

O comportamento do sistema de monitoramento de integridade é definido por suas informações de configuração, localizadas no [elemento`<healthMonitoring>`](https://msdn.microsoft.com/library/2fwh2ss9.aspx) no `Web.config`. Esta seção de configuração define, entre outras coisas, as três informações importantes a seguir:

1. Os eventos de monitoramento de integridade que, quando gerados, devem ser registrados em log,
2. As fontes de log e
3. Como cada evento de monitoramento de integridade definido em (1) é mapeado para as fontes de log definidas em (2).

Essas informações são especificadas por meio de três elementos de configuração filhos: [`<eventMappings>`](https://msdn.microsoft.com/library/yc5yk01w.aspx), [`<providers>`](https://msdn.microsoft.com/library/zaa41kz1.aspx)e [`<rules>`](https://msdn.microsoft.com/library/fe5wyxa0.aspx), respectivamente.

As informações de configuração do sistema de monitoramento de integridade padrão podem ser encontradas no arquivo `Web.config` na pasta `%WINDIR%\Microsoft.NET\Framework\version\CONFIG`. Essas informações de configuração padrão, com algumas marcações removidas para fins de brevidade, são mostradas abaixo:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample1.xml)]

Os eventos de monitoramento de integridade de interesse são definidos no elemento `<eventMappings>`, que fornece um nome amigável para uma classe de eventos de monitoramento de integridade. Na marcação acima, o elemento `<eventMappings>` atribui o nome amigável humano "todos os erros" aos eventos de monitoramento de integridade do tipo `WebBaseErrorEvent` e o nome "auditorias de falha" para eventos de monitoramento de integridade do tipo `WebFailureAuditEvent`.

O elemento `<providers>` define as fontes de log, dando a elas um nome amigável e especificando qualquer informação de configuração específica de origem de log. O primeiro elemento `<add>` define o provedor "EventLogprovider", que registra os eventos de monitoramento de integridade especificados usando a classe `EventLogWebEventProvider`. A classe `EventLogWebEventProvider` registra o evento no log de eventos do Windows. O segundo elemento `<add>` define o provedor "SqlWebEventProvider", que registra eventos em um banco de dados Microsoft SQL Server por meio da classe `SqlWebEventProvider`. A configuração "SqlWebEventProvider" especifica a cadeia de conexão do banco de dados (`connectionStringName`) entre outras opções de configuração.

O elemento `<rules>` mapeia os eventos especificados no elemento `<eventMappings>` para as fontes de log no elemento `<providers>`. Por padrão, os aplicativos Web ASP.NET registram em log todas as exceções sem tratamento e falhas de auditoria no log de eventos do Windows.

## <a name="logging-events-to-a-database"></a>Registrando eventos em log em um banco de dados

A configuração padrão do sistema de monitoramento de integridade pode ser personalizada em aplicativos Web por aplicativo Web, adicionando uma seção de `<healthMonitoring>` ao arquivo de `Web.config` do aplicativo. Você pode incluir elementos adicionais nas seções `<eventMappings>`, `<providers>`e `<rules>` usando o elemento `<add>`. Para remover uma configuração da configuração padrão, use o elemento `<remove>` ou use `<clear />` para remover todos os valores padrão de uma dessas seções. Vamos configurar o aplicativo Web de análises de livros para registrar em log todas as exceções sem tratamento em um banco de dados Microsoft SQL Server usando a classe `SqlWebEventProvider`.

A classe `SqlWebEventProvider` faz parte do sistema de monitoramento de integridade e registra um evento de monitoramento de integridade em um banco de dados SQL Server especificado. A classe `SqlWebEventProvider` espera que o banco de dados especificado inclua um procedimento armazenado chamado `aspnet_WebEvent_LogEvent`. Esse procedimento armazenado recebe os detalhes do evento e é tarefa com o armazenamento dos detalhes do evento. A boa notícia é que você não precisa criar esse procedimento armazenado nem a tabela para armazenar os detalhes do evento. Você pode adicionar esses objetos ao seu banco de dados usando a ferramenta `aspnet_regsql.exe`.

> [!NOTE]
> A ferramenta de `aspnet_regsql.exe` foi discutida de volta no [tutorial *Configurando um site que usa serviços de aplicativos* ](configuring-a-website-that-uses-application-services-cs.md) quando adicionamos suporte para ASP. Serviços de aplicativos da rede. Consequentemente, o banco de dados do site do Reviews já contém o procedimento armazenado `aspnet_WebEvent_LogEvent`, que armazena as informações do evento em uma tabela chamada `aspnet_WebEvent_Events`.

Depois que você tiver o procedimento armazenado e a tabela necessários adicionados ao seu banco de dados, tudo o que resta é instruir o monitoramento de integridade para registrar em log todas as exceções sem tratamento no banco de dados. Para fazer isso, adicione a seguinte marcação ao arquivo de `Web.config` do seu site:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample2.xml)]

A marcação de configuração de monitoramento de integridade acima usa `<clear />` elementos para apagar as informações de configuração de monitoramento de integridade predefinida das seções `<eventMappings>`, `<providers>`e `<rules>`. Em seguida, ele adiciona uma única entrada a cada uma dessas seções.

- O elemento `<eventMappings>` define um único evento de monitoramento de integridade de interesse chamado "todos os erros", que é gerado sempre que ocorre uma exceção sem tratamento.
- O elemento `<providers>` define uma única origem de log chamada "SqlWebEventProvider" que usa a classe `SqlWebEventProvider`. O atributo `connectionStringName` foi definido como "ReviewsConnectionString", que é o nome da nossa cadeia de conexão definida na seção `<connectionStrings>`.
- Por fim, o elemento regras de &lt;&gt; indica que quando um evento "todos os erros" ocorre, ele deve ser registrado usando o provedor "SqlWebEventProvider".

Essa informação de configuração instrui o sistema de monitoramento de integridade a registrar em log todas as exceções sem tratamento no banco de dados de revisões de livros.

> [!NOTE]
> O evento `WebBaseErrorEvent` só é gerado para erros do servidor; Ele não é gerado para erros HTTP, como uma solicitação para um recurso ASP.NET que não é encontrado. Isso difere do comportamento do evento `Error` da classe `HttpApplication`, que é gerado para erros de servidor e HTTP.

Para ver o sistema de monitoramento de integridade em ação, visite o site e gere um erro de tempo de execução visitando `Genre.aspx?ID=foo`. Você deve ver a página de erro apropriada – a exceção detalhes da tela amarela de morte (ao visitar localmente) ou a página de erro personalizada (ao visitar o site em produção). Nos bastidores, o sistema de monitoramento de integridade registrou as informações de erro no banco de dados. Deve haver um registro na tabela de `aspnet_WebEvent_Events` (consulte a **Figura 1**); Esse registro contém informações sobre o erro de tempo de execução que acabou de ocorrer.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image1.png)

**Figura 1**: os detalhes do erro foram registrados na tabela de `aspnet_WebEvent_Events`  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-asp-net-health-monitoring-cs/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Exibindo o log de erros em uma página da Web

Com a configuração atual do site, o sistema de monitoramento de integridade registra em log todas as exceções sem tratamento no banco de dados. No entanto, o monitoramento de integridade não fornece nenhum mecanismo para exibir o log de erros por meio de uma página da Web. No entanto, você pode criar uma página ASP.NET que exibe essas informações do banco de dados. (Como veremos momentaneamente, você pode optar por ter os detalhes do erro enviados a você em uma mensagem de email.)

Se você criar uma página desse tipo, certifique-se de tomar medidas para permitir que apenas usuários autorizados exibam os detalhes do erro. Se seu site já utiliza contas de usuário, você pode usar regras de autorização de URL para restringir o acesso à página a determinados usuários ou funções. Para obter mais informações sobre como conceder ou restringir o acesso a páginas da Web com base no usuário conectado, consulte meus [tutoriais de segurança do site](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> O tutorial subsequente explora um sistema de notificação e registro em log de erros alternativo chamado ELMAH. O ELMAH inclui um mecanismo interno para exibir o log de erros de uma página da Web e como um RSS feed.

## <a name="logging-events-to-email"></a>Registrando eventos em log para email

O sistema de monitoramento de integridade inclui um provedor de origem de log que "registra" um evento em uma mensagem de email. A origem do log inclui as mesmas informações que são registradas no banco de dados no corpo da mensagem de email. Você pode usar essa fonte de log para notificar um desenvolvedor quando ocorrer um determinado evento de monitoramento de integridade.

Vamos atualizar o livro Reviews da configuração do site para que recebamos um email sempre que ocorrer uma exceção. Para fazer isso, precisamos executar três tarefas:

1. Configure o aplicativo Web ASP.NET para enviar email. Isso é feito especificando como as mensagens de email são enviadas por meio do elemento de configuração `<system.net>`. Para obter mais informações sobre como enviar mensagens de email em um aplicativo ASP.NET, consulte [Enviar email em ASP.net](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) e as [perguntas frequentes sobre o System .net. mail](http://systemnetmail.com/).
2. Registre o provedor de origem de log de email no elemento `<providers>` e
3. Adicione uma entrada ao elemento `<rules>` que mapeia o evento "todos os erros" para o provedor de origem de log adicionado na etapa (2).

O sistema de monitoramento de integridade inclui duas classes de provedor de origem de log de email: `SimpleMailWebEventProvider` e `TemplatedMailWebEventProvider`. A [classe`SimpleMailWebEventProvider`](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) envia uma mensagem de email de texto sem formatação que inclui os detalhes do evento e fornece pouca personalização do corpo do email. Com a [classe`TemplatedMailWebEventProvider`](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) , você especifica uma página ASP.net cuja marcação renderizada é usada como o corpo da mensagem de email. A [classe`TemplatedMailWebEventProvider`](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) oferece um controle muito maior sobre o conteúdo e o formato da mensagem de email, mas requer um pouco mais de trabalho antecipado, pois você precisa criar a página ASP.NET que gera o corpo da mensagem de email. Este tutorial se concentra no uso da classe `SimpleMailWebEventProvider`.

Atualize o elemento `<providers>` do sistema de monitoramento de integridade no arquivo `Web.config` para incluir uma origem de log para a classe `SimpleMailWebEventProvider`:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample3.xml)]

A marcação acima usa a classe `SimpleMailWebEventProvider` como o provedor de origem de log e atribui o nome amigável "EmailWebEventProvider". Além disso, o atributo `<add>` inclui opções de configuração adicionais, como os endereços de e para da mensagem de email.

Com a origem do log de email definida, tudo o que resta é instruir o sistema de monitoramento de integridade a usar essa origem para "registrar" as exceções sem tratamento. Isso é feito com a adição de uma nova regra na seção `<rules>`:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample4.xml)]

A seção `<rules>` agora inclui duas regras. O primeiro, chamado "todos os erros de email", envia todas as exceções sem tratamento para a origem de log "EmailWebEventProvider". Essa regra tem o efeito de enviar detalhes sobre erros no site para o endereço de endereçamento especificado. A regra "todos os erros no banco de dados" registra os detalhes do erro no banco de dados do site. Consequentemente, sempre que uma exceção não tratada ocorrer no site, seus detalhes serão registrados no banco de dados e enviados ao endereço de email especificado.

A **Figura 2** mostra o email gerado pela classe `SimpleMailWebEventProvider` ao visitar `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image4.png)

**Figura 2**: os detalhes do erro são enviados em uma mensagem de email  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-asp-net-health-monitoring-cs/_static/image6.png))

## <a name="summary"></a>Resumo

O sistema de monitoramento de integridade do ASP.NET foi projetado para permitir que os administradores monitorem a integridade de um aplicativo Web implantado. Eventos de monitoramento de integridade são gerados quando determinadas ações ficam desdobradas, como quando o aplicativo é interrompido, quando um usuário faz logon com êxito no site ou quando ocorre uma exceção sem tratamento. Esses eventos podem ser registrados em qualquer número de fontes de log. Este tutorial mostrou como registrar em log os detalhes de exceções sem tratamento em um banco de dados e por meio de uma mensagem de email.

Este tutorial se concentrou no uso do monitoramento de integridade para registrar exceções sem tratamento, mas tenha em mente que o monitoramento de integridade foi projetado para medir a integridade geral de um aplicativo ASP.NET implantado e inclui uma infinidade de eventos de monitoramento de integridade e origens de log não explorado aqui. Além do mais, você pode criar seus próprios eventos de monitoramento de integridade e origens de log, caso seja necessário. Se você estiver interessado em aprender mais sobre o monitoramento de integridade, uma boa primeira etapa é ler as [perguntas frequentes sobre monitoramento de integridade](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)do [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). Depois disso, consulte [como: usar o monitoramento de integridade no ASP.NET 2,0](https://msdn.microsoft.com/library/ms998306.aspx).

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Visão geral do monitoramento de integridade do ASP.NET](https://msdn.microsoft.com/library/bb398933.aspx)
- [Configurando e personalizando o sistema de monitoramento de integridade do ASP.NET](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [Perguntas frequentes-monitoramento de integridade no ASP.NET 2,0](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Como: enviar email para notificações de monitoramento de integridade](https://msdn.microsoft.com/library/ms227553.aspx)
- [Como: usar o monitoramento de integridade no ASP.NET](https://msdn.microsoft.com/library/ms998306.aspx)
- [Monitoramento de integridade no ASP.NET](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [Anterior](processing-unhandled-exceptions-cs.md)
> [Próximo](logging-error-details-with-elmah-cs.md)
