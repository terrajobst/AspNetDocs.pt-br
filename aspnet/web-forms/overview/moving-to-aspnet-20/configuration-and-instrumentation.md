---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Configuração e instrumentação | Microsoft Docs
author: microsoft
description: Há grandes alterações na configuração e instrumentação no ASP.NET 2,0. A nova API de configuração do ASP.NET permite que as alterações de configuração sejam feitas PR...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: cd5bedce5459e8cf8e72df8de69ebd82f2d97789
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78626024"
---
# <a name="configuration-and-instrumentation"></a>Configuração e instrumentação

pela [Microsoft](https://github.com/microsoft)

> Há grandes alterações na configuração e instrumentação no ASP.NET 2,0. A nova API de configuração do ASP.NET permite que as alterações de configuração sejam feitas programaticamente. Além disso, muitas novas definições de configuração existem permitem novas configurações e instrumentação.

Há grandes alterações na configuração e instrumentação no ASP.NET 2,0. A nova API de configuração do ASP.NET permite que as alterações de configuração sejam feitas programaticamente. Além disso, muitas novas definições de configuração existem permitem novas configurações e instrumentação.

Neste módulo, discutiremos a ASP.NET da API de configuração, pois ela está relacionada à leitura e gravação de arquivos de configuração do ASP.NET, e também abordaremos a instrumentação do ASP.NET. Também abordaremos os novos recursos disponíveis no rastreamento do ASP.NET.

## <a name="aspnet-configuration-api"></a>API de configuração do ASP.NET

A API de configuração do ASP.NET permite que você desenvolva, implante e gerencie dados de configuração do aplicativo usando uma única interface de programação. Você pode usar a API de configuração do para desenvolver e modificar configurações de ASP.NET completas programaticamente sem editar diretamente o XML nos arquivos de configuração. Além disso, você pode usar a API de configuração em aplicativos de console e scripts que você desenvolve, em ferramentas de gerenciamento baseadas na Web e nos snap-ins do MMC (console de gerenciamento Microsoft).

As duas ferramentas de gerenciamento de configuração a seguir usam a API de configuração e estão incluídas com o .NET Framework versão 2,0:

- O snap-in do MMC do ASP.NET, que usa a API de configuração para simplificar tarefas administrativas, fornecendo uma exibição integrada dos dados de configuração local de todos os níveis da hierarquia de configuração.
- A ferramenta de administração de site, que permite que você gerencie definições de configuração para aplicativos locais e remotos, incluindo sites hospedados.

A API de configuração do ASP.NET consiste em um conjunto de objetos de gerenciamento do ASP.NET que você pode usar para configurar sites e aplicativos de forma programática. Os objetos de gerenciamento são implementados como uma biblioteca de classes .NET Framework. O modelo de programação de API de configuração ajuda a garantir a consistência e a confiabilidade do código impondo tipos de dados em tempo de compilação. Para facilitar o gerenciamento de configurações de aplicativo, a API de configuração permite que você exiba dados mesclados de todos os pontos na hierarquia de configuração como uma única coleção, em vez de exibir os dados como coleções separadas de diferentes arquivos de configuração. Além disso, a API de configuração permite que você manipule as configurações do aplicativo inteiro sem editar diretamente o XML nos arquivos de configuração. Por fim, a API simplifica as tarefas de configuração, oferecendo suporte a ferramentas administrativas, como a ferramenta de administração de site. A API de configuração simplifica a implantação ao dar suporte à criação de arquivos de configuração em um computador e à execução de scripts de configuração em vários computadores.

> [!NOTE]
> A API de configuração não oferece suporte à criação de aplicativos do IIS.

## <a name="working-with-local-and-remote-configuration-settings"></a>Trabalhando com definições de configuração local e remota

Um objeto de configuração representa a exibição mesclada das definições de configuração que se aplicam a uma entidade física específica, como um computador ou a uma entidade lógica, como um aplicativo ou um site da Web. A entidade lógica especificada pode existir no computador local ou em um servidor remoto. Quando não existe nenhum arquivo de configuração para uma entidade especificada, o objeto de configuração representa as definições de configuração padrão, conforme definido pelo arquivo Machine. config.

Você pode obter um objeto de configuração usando um dos métodos de configuração aberta das seguintes classes:

1. A classe ConfigurationManager, se sua entidade for um aplicativo cliente.
2. A classe WebConfigurationManager, se sua entidade for um aplicativo Web.

Esses métodos retornarão um objeto de configuração que, por sua vez, fornece os métodos e as propriedades necessários para lidar com os arquivos de configuração subjacentes. Você pode acessar esses arquivos para leitura ou gravação.

### <a name="reading"></a>Lendo

Você usa o método GetSection ou GetSection para ler as informações de configuração. O usuário ou processo que lê deve ter permissões de leitura em todos os arquivos de configuração na hierarquia.

> [!NOTE]
> Se você usar um método GetSection estático que usa um parâmetro Path, o parâmetro Path deverá se referir ao aplicativo no qual o código está sendo executado. Caso contrário, o parâmetro será ignorado e as informações de configuração para o aplicativo em execução no momento serão retornadas.

### <a name="writing"></a>Gravação

Você usa um dos métodos Save para gravar informações de configuração. O usuário ou processo que grava deve ter permissões de gravação no arquivo de configuração e no diretório no nível da hierarquia de configuração atual, bem como permissões de leitura em todos os arquivos de configuração na hierarquia.

Para gerar um arquivo de configuração que representa as definições de configuração herdadas para uma entidade especificada, use um dos seguintes métodos Save-Configuration:

1. O método Save para criar um novo arquivo de configuração.
2. O método SaveAs para gerar um novo arquivo de configuração em outro local.

## <a name="configuration-classes-and-namespaces"></a>Classes de configuração e namespaces

Muitos métodos e classes de configuração são semelhantes uns aos outros. A tabela a seguir descreve os namespaces e as classes de configuração mais comumente usadas.

| **Classe de configuração ou namespace** | **Descrição** |
| --- | --- |
| Namespace [System. Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) | Contém as classes de configuração principais para todos os aplicativos de .NET Framework. As classes de manipulador de seção são usadas para obter dados de configuração para uma seção de métodos, como getseção e GetSection. Esses dois métodos são não estáticos. |
| Classe System. Configuration. Configuration | Representa um conjunto de dados de configuração para um computador, aplicativo, diretório da Web ou outro recurso. Essa classe contém métodos úteis, como getseção e GetSection, para atualizar definições de configuração e obter referências a seções e grupos de seção. Essa classe é usada como um tipo de retorno para métodos que obtêm dados de configuração de tempo de design, como os métodos das classes WebConfigurationManager e ConfigurationManager. |
| Namespace System. Web. Configuration | Contém as classes de manipulador de seção para as seções de configuração ASP.NET definidas em [ASP.net definições de configuração](https://msdn.microsoft.com/library/b5ysx397.aspx). As classes de manipulador de seção são usadas para obter dados de configuração para uma seção de métodos, como getseção e GetSection. |
| Classe System. Web. Configuration. WebConfigurationManager | Fornece métodos úteis para obter referências a definições de configuração de tempo de execução e de tempo de design. Esses métodos usam a classe System. Configuration. Configuration como um tipo de retorno. Você pode usar o método GetSection estático dessa classe ou o método GetSection não estático da classe System. Configuration. ConfigurationManager de forma intercambiável. Para configurações de aplicativo Web, a classe System. Web. Configuration. WebConfigurationManager é recomendada em vez da classe System. Configuration. ConfigurationManager. |
| Namespace [System. Configuration. Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) | Fornece uma maneira de personalizar e estender o provedor de configuração. Essa é a classe base para todas as classes de provedor no sistema de configuração. |
| Namespace [System. Web. Management](https://msdn.microsoft.com/library/system.web.management.aspx) | Contém classes e interfaces para gerenciar e monitorar a integridade de aplicativos Web. Estritamente falando, esse namespace não é considerado parte da API de configuração. Por exemplo, o rastreamento e o acionamento de eventos são realizados pelas classes neste namespace. |
| Namespace [System. Management. Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) | Fornece as classes necessárias para a instrumentação de aplicativos exporem suas informações de gerenciamento e eventos por meio de Instrumentação de Gerenciamento do Windows (WMI) para consumidores potenciais. O monitoramento de integridade do ASP.NET usa o WMI para entregar eventos. Estritamente falando, esse namespace não é considerado parte da API de configuração. |

## <a name="reading-from-aspnet-configuration-files"></a>Lendo de arquivos de configuração do ASP.NET

A classe WebConfigurationManager é a classe principal para leitura de arquivos de configuração do ASP.NET. Há basicamente três etapas para ler os arquivos de configuração do ASP.NET:

1. Obtenha um objeto de configuração usando o método OpenWebConfiguration.
2. Obtenha uma referência à seção desejada no arquivo de configuração.
3. Leia as informações desejadas no arquivo de configuração.

O objeto de configuração representa não representa um arquivo de configuração específico. Em vez disso, ele representa uma exibição mesclada da configuração de um computador, aplicativo ou site. O exemplo de código a seguir instancia um objeto de configuração que representa a configuração de um aplicativo Web chamado *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Observe que, se o caminho/ProductInfo não existir, o código acima retornará a configuração padrão, conforme especificado no arquivo Machine. config.

Assim que tiver o objeto de configuração, você poderá usar o método GetSection ou GetSection para detalhar as definições de configuração. O exemplo a seguir obtém uma referência às configurações de representação para o aplicativo ProductInfo acima:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Gravando em arquivos de configuração do ASP.NET

Como na leitura de arquivos de configuração, a classe WebConfigurationManager é o núcleo para gravar em arquivos de configuração Asp.NET. Também há três etapas para gravar nos arquivos de configuração do ASP.NET.

1. Obtenha um objeto de configuração usando o método OpenWebConfiguration.
2. Obtenha uma referência à seção desejada no arquivo de configuração.
3. Grave as informações desejadas do arquivo de configuração usando o método salvar ou salvar.

O código a seguir altera o atributo **debug** do elemento de&gt; de compilação &lt;para false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Quando esse código é executado, o atributo **debug** do elemento de&gt; de compilação do &lt;será definido como false para o arquivo Web. config do aplicativo *webApp* .

## <a name="systemwebmanagement-namespace"></a>Namespace System. Web. Management

O namespace System. Web. Management fornece as classes e interfaces para gerenciar e monitorar a integridade dos aplicativos ASP.NET.

O registro em log é realizado definindo uma regra que associa eventos a um provedor. A regra define o tipo de eventos que são enviados ao provedor. Os seguintes eventos base estão disponíveis para você registrar:

| **WebBaseEvent** | A classe de evento base para todos os eventos. Contém as propriedades necessárias para todos os eventos, como código de evento, código de detalhes do evento, a data e a hora em que o evento foi gerado, o número de sequência, a mensagem de evento e os detalhes do evento. |
| --- | --- |
| **WebManagementEvent** | A classe de evento base para eventos de gerenciamento, como tempo de vida do aplicativo, solicitação, erro e eventos de auditoria. |
| **WebHeartbeatEvent** | O evento gerado pelo aplicativo em intervalos regulares para capturar informações úteis de estado de tempo de execução. |
| **WebAuditEvent** | A classe base para eventos de auditoria de segurança, que são usados para marcar condições como falha de autorização, falha de descriptografia, *etc.* |
| **WebRequestEvent** | A classe base para todos os eventos de solicitação informativos. |
| **WebBaseErrorEvent** | A classe base para todos os eventos que indicam condições de erro. |

Os tipos de provedores disponíveis permitem que você envie a saída do evento para Visualizador de Eventos, SQL Server, Instrumentação de Gerenciamento do Windows (WMI) e email. Os provedores pré-configurados e os mapeamentos de eventos reduzem a quantidade de trabalho necessária para obter a saída do evento registrada.

O ASP.NET 2,0 usa o provedor de log de eventos pronto para fazer o log de eventos com base em domínios de aplicativo Iniciando e parando, bem como registrando quaisquer exceções sem tratamento. Isso ajuda a abordar alguns dos cenários básicos. Por exemplo, digamos que seu aplicativo gere uma exceção, mas o usuário não salvará o erro e você não poderá reproduzi-lo. Com a regra de log de eventos padrão, você poderá coletar as informações de exceção e de pilha para obter uma ideia melhor do tipo de erro. Outro exemplo se aplica se seu aplicativo estiver perdendo o estado da sessão. Nesse caso, você pode procurar no log de eventos para determinar se o domínio do aplicativo está sendo reciclado e por que o domínio do aplicativo parou em primeiro lugar.

Além disso, o sistema de monitoramento de integridade é extensível. Por exemplo, você pode definir eventos da Web personalizados, disfire-los em seu aplicativo e, em seguida, definir uma regra para enviar as informações do evento para um provedor, como seu email. Isso permite que você vincule facilmente sua instrumentação aos provedores de monitoramento de integridade. Como outro exemplo, você pode acionar um evento cada vez que um pedido é processado e configurar uma regra que envia cada evento para o banco de dados de SQL Server. Você também pode acionar um evento quando um usuário não conseguir fazer logon várias vezes em uma linha e configurar o evento para usar os provedores baseados em email.

A configuração para os provedores e eventos padrão é armazenada no arquivo Web. config global. O arquivo Web. config global armazena todas as configurações baseadas na Web que foram armazenadas no arquivo Machine. config no ASP.NET 1x. O arquivo Web. config global está localizado no seguinte diretório:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

A seção &lt;healthMonitoring&gt; do arquivo Web. config global fornece as definições de configuração padrão. Você pode substituir essa configuração ou definir suas próprias configurações implementando a seção &lt;healthMonitoring&gt; no arquivo Web. config do seu aplicativo.

A seção &lt;healthMonitoring&gt; do arquivo Web. config global contém os seguintes itens:

| **providers** | Contém os provedores configurados para o Visualizador de Eventos, o WMI e o SQL Server. |
| --- | --- |
| **EventMappings** | Contém mapeamentos para as várias classes do webbase. Você pode estender essa lista se você gerar sua própria classe de evento. Gerar sua própria classe de evento proporciona uma granularidade mais refinada sobre os provedores para os quais você envia informações. Por exemplo, você pode configurar exceções sem tratamento a serem enviadas ao SQL Server, enquanto envia seus próprios eventos personalizados para email. |
| **regras** | Vincula os EventMappings ao provedor. |
| **buffer** | Usado com SQL Server e provedores de email para determinar a frequência de liberação dos eventos para o provedor. |

Veja abaixo um exemplo de código do arquivo Web. config global.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Como armazenar eventos para Visualizador de Eventos

Como mencionado anteriormente, o provedor para registrar eventos no Visualizador de Eventos está configurado para você no arquivo Web. config global. Por padrão, todos os eventos baseados em **WebBaseErrorEvent** e **WebFailureAuditEvent** são registrados. Você pode adicionar outras regras para registrar informações adicionais no log de eventos. Por exemplo, se você quisesse registrar todos os eventos (*ou seja*, todos os eventos com base em **WebBaseEvent**), poderá adicionar a regra a seguir ao seu arquivo Web. config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Essa regra vincularia o mapa de eventos **todos os eventos** ao provedor de log de eventos. O EventMappings e o provedor estão incluídos no arquivo Web. config global.

## <a name="how-to-store-events-to-sql-server"></a>Como armazenar eventos para SQL Server

Esse método usa o banco de dados **aspnetdb** , que é gerado pela ferramenta ASPNET\_RegSql. exe. O provedor padrão usa a cadeia de conexão LocalSqlServer, que usa um banco de dados baseado em arquivo na pasta Data\_do aplicativo ou na instância SqlExpress do SQL Server. A cadeia de conexão LocalSqlServer e o sqlfornecetor são configurados no arquivo Web. config global.

A cadeia de conexão LocalSqlServer no arquivo global Web. config tem a seguinte aparência:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Se você quiser usar outra instância de SQL Server, será necessário usar a ferramenta ASPNET\_RegSql. exe, que pode ser encontrada no%windir%\Microsoft.Net\Framework\v2.0.\* pasta. Use a ferramenta ASPNET\_RegSql. exe para gerar um banco de dados personalizado de **aspnetdb** na instância do SQL Server, depois adicione a cadeia de conexão ao arquivo de configuração de aplicativos e, em seguida, adicione um provedor usando a nova cadeia de conexão. Depois de criar o banco de dados **aspnetdb** , você precisará definir uma regra para vincular um EventMappings ao sqlfornecetor.

Se você usar o SqlProvider padrão ou configurar seu próprio provedor, precisará adicionar uma regra vinculando o provedor a um mapa de eventos. A regra a seguir vincula o novo provedor que você criou acima ao mapa de eventos **todos os eventos** . Essa regra registrará em log todos os eventos com base em **WebBaseEvent** e os enviará para o MySqlWebEventProvider que usará a cadeia de conexão MYASPNETDB. O código a seguir adiciona uma regra para vincular o provedor a um mapa de eventos:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Se você quisesse enviar apenas erros para SQL Server, poderá adicionar a seguinte regra:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Como encaminhar eventos para o WMI

Você também pode encaminhar os eventos para o WMI. O provedor WMI é configurado para você no arquivo Web. config global por padrão.

O exemplo de código a seguir adiciona uma regra para encaminhar os eventos para o WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Você precisará adicionar uma regra para associar um EventMappings ao provedor e também um aplicativo de ouvinte WMI para escutar os eventos. O exemplo de código a seguir adiciona uma regra para vincular o provedor WMI ao mapa de eventos **todos os eventos** :

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Como encaminhar eventos para email

Você também pode encaminhar eventos para email. Tenha cuidado com as regras de evento que você mapeia para seu provedor de email, pois você pode enviar involuntariamente muitas informações que podem ser mais adequadas para SQL Server ou o log de eventos. Há dois provedores de email; SimpleMailWebEventProvider e TemplatedMailWebEventProvider. Cada um tem os mesmos atributos de configuração, com exceção dos atributos "template" e "detailedTemplateErrors", ambos disponíveis apenas no TemplatedMailWebEventProvider.

> [!NOTE]
> Nenhum desses provedores de email está configurado para você. Você precisará adicioná-los ao seu arquivo Web. config.

A principal diferença entre esses dois provedores de email é que o SimpleMailWebEventProvider envia emails em um modelo genérico que não pode ser modificado. O arquivo Web. config de exemplo adiciona este provedor de email à lista de provedores configurados usando a seguinte regra:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

A regra a seguir também é adicionada para vincular o provedor de email ao mapa de eventos **todos os eventos** :

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>Rastreamento do ASP.NET 2,0

Há três aprimoramentos principais a serem rastreados no ASP.NET 2,0.

1. Funcionalidade de rastreamento integrada
2. Acesso programático a mensagens de rastreamento
3. Rastreamento aprimorado no nível de aplicativo

## <a name="integrated-tracing-functionality"></a>Funcionalidade de rastreamento integrada

Agora você pode rotear mensagens emitidas pela classe System. Diagnostics. Trace para a saída de rastreamento ASP.NET e rotear mensagens emitidas pelo rastreamento ASP.NET para System. Diagnostics. Trace. Você também pode encaminhar eventos de instrumentação ASP.NET para System. Diagnostics. Trace. Essa funcionalidade é fornecida pelo novo atributo **WriteToDiagnosticsTrace** do elemento&gt; de rastreamento de &lt;. Quando esse valor booliano é true, as mensagens de rastreamento ASP.NET são encaminhadas para a infraestrutura de rastreamento System. Diagnostics para uso por qualquer ouvinte registrado para exibir mensagens de rastreamento.

## <a name="programmatic-access-to-trace-messages"></a>Acesso programático a mensagens de rastreamento

O ASP.NET 2,0 permite acesso programático a todas as mensagens de rastreamento por meio da classe **TraceContextRecord** e da coleção **TraceRecords** . A maneira mais eficiente de acessar mensagens de rastreamento é registrar um delegado **TraceContextEventHandler** (também novo no ASP.NET 2,0) para lidar com o novo evento **TraceFinished** . Em seguida, você pode fazer um loop pelas mensagens de rastreamento como desejar.

O exemplo de código a seguir ilustra isso:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

No exemplo acima, eu executo um loop pela coleção TraceRecords e escrevo cada mensagem no fluxo de resposta.

## <a name="improved-application-level-tracing"></a>Rastreamento aprimorado no nível de aplicativo

O rastreamento em nível de aplicativo é melhorado por meio da introdução do novo atributo **MostRecent** do elemento&gt; de rastreamento de &lt;. Esse atributo especifica se a saída de rastreamento de nível de aplicativo mais recente é exibida e os dados de rastreamento mais antigos além dos limites indicados pelo requestLimit são descartados. Se for false, os dados de rastreamento serão exibidos para solicitações até que o atributo requestLimit seja atingido.

## <a name="aspnet-command-line-tools"></a>Ferramentas de linha de comando do ASP.NET

Há várias ferramentas de linha de comando para auxiliar na configuração do ASP.NET. Os desenvolvedores de ASP.NET devem estar familiarizados com a ferramenta ASPNET\_regiis. exe. O ASP.NET 2,0 fornece três outras ferramentas de linha de comando para auxiliar na configuração.

As seguintes ferramentas de linha de comando estão disponíveis:

| **Ferramenta** | **Uso** |
| --- | --- |
| **o ASPNET\_regiis. exe** | Permite o registro de ASP.NET com o IIS. Há duas versões dessas ferramentas fornecidas com o ASP.NET 2,0, uma para sistemas de 32 bits (na pasta Framework) e outra para sistemas de 64 bits (na pasta Framework64). A versão de 64 bits não será instalada em um sistema operacional de 32 bits. |
| **ASPNET\_RegSql. exe** | A ferramenta de registro de SQL Server ASP.NET é usada para criar um banco de dados Microsoft SQL Server para uso pelos provedores de SQL Server no ASP.NET ou para adicionar ou remover opções de um banco de dados existente. O arquivo Aspnet\_RegSql. exe está localizado na pasta [Drive:] \WINDOWS\Microsoft.NET\Framework\versionNumber no seu servidor Web. |
| **ASPNET\_regbrowsers. exe** | A ferramenta de registro do navegador ASP.NET analisa e compila todas as definições do navegador em todo o sistema em um assembly e instala o assembly no cache de assembly global. A ferramenta usa os arquivos de definição do navegador (. Arquivos de navegador) do subdiretório navegadores .NET Framework. A ferramenta pode ser encontrada no diretório%SystemRoot%\Microsoft.NET\Framework\version\ |
| **ASPNET\_Compiler. exe** | A ferramenta de compilação ASP.NET permite que você compile um aplicativo Web ASP.NET, seja no local ou para implantação em um local de destino, como um servidor de produção. A compilação in-loco ajuda o desempenho do aplicativo porque os usuários finais não encontram um atraso na primeira solicitação para o aplicativo enquanto o aplicativo é compilado. |

Como o ASPNET\_a ferramenta regiis. exe não é novidade no ASP.NET 2,0, não vamos discuti-lo aqui.

## <a name="aspnet-sql-server-registration-tool---aspnet_regsqlexe"></a>Ferramenta de registro do ASP.NET SQL Server-ASPNET\_RegSql. exe

Você pode definir vários tipos de opções usando a ferramenta de registro do ASP.NET SQL Server. Você pode especificar uma conexão SQL, especificar quais serviços de aplicativo ASP.NET usam SQL Server para gerenciar informações, indicar qual banco de dados ou tabela é usado para dependência de cache do SQL e adicionar ou remover suporte para usar SQL Server para armazenar procedimentos e estado de sessão.

Vários serviços de aplicativo ASP.NET dependem de um provedor para gerenciar o armazenamento e a recuperação de dados de uma fonte de dados. Cada provedor é específico para a fonte de dados. O ASP.NET inclui um provedor de SQL Server para os seguintes recursos de ASP.NET:

- Membership (a classe [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) ).
- Gerenciamento de função (a classe [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) ).
- Profile (a classe [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) ).
- Web Parts a personalização (a classe [Sqlpersonalizaprovider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) ).
- Eventos da Web (a classe [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) ).

Quando você instala o ASP.NET, o arquivo Machine. config para o servidor inclui elementos de configuração que especificam provedores de SQL Server para cada um dos recursos do ASP.NET que dependem de um provedor. Esses provedores são configurados, por padrão, para se conectar a uma instância de usuário local do SQL Server Express 2005. Se você alterar a cadeia de conexão padrão usada pelos provedores, antes de poder usar qualquer um dos recursos de ASP.NET configurados na configuração do computador, será necessário instalar o banco de dados do SQL Server e os elementos do banco de dados para o recurso escolhido usando ASPNET\_RegSql. exe. Se o banco de dados que você especificar com a ferramenta de registro do SQL ainda não existir (aspnetdb será o banco de dados padrão se um não for especificado na linha de comando), o usuário atual deverá ter direitos para criar bancos de dados no SQL Server, bem como para criar o esquema e lements em um banco de dados.

### <a name="sql-cache-dependency"></a>Dependência de cache do SQL

Um recurso avançado do cache de saída do ASP.NET é a dependência do cache do SQL. A dependência de cache do SQL dá suporte a dois modos diferentes de operação: um que usa uma implementação ASP.NET da sondagem de tabela e um segundo modo que usa os recursos de notificação de consulta do SQL Server 2005. A ferramenta de registro do SQL pode ser usada para configurar o modo de sondagem de tabela da operação.

### <a name="session-state"></a>Estado de sessão

Por padrão, os valores e as informações de estado da sessão são armazenados na memória no processo ASP.NET. Como alternativa, você pode armazenar dados de sessão em um banco de SQL Server, em que ele pode ser compartilhado por vários servidores Web. Se o banco de dados que você especificar para o estado de sessão com a ferramenta de registro do SQL ainda não existir, o usuário atual deverá ter direitos para criar bancos de dados no SQL Server, bem como para criar elementos de esquema dentro de um banco de dados. Se o banco de dados existir, o usuário atual deverá ter direitos para criar elementos de esquema no banco de dados existente.

Para instalar o banco de dados de estado de sessão no SQL Server, execute a ferramenta ASPNET\_RegSql. exe e forneça as seguintes informações com o comando:

- O nome da instância de SQL Server, usando a opção **-S** .
- As credenciais de logon para uma conta que tenha permissão para criar um banco de dados em um computador que executa o SQL Server. Use a opção **-E** para usar o usuário conectado no momento ou use a opção **-U** para especificar uma ID de usuário junto com a opção **-P** para especificar uma senha.
- A opção de linha de comando **-ssadd** para adicionar o banco de dados de estado de sessão.

Por padrão, você não pode usar a ferramenta ASPNET\_RegSql. exe para instalar o banco de dados de estado de sessão em um computador que executa o SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnet_regbrowsersexe"></a>A ferramenta de registro do navegador ASP.NET-ASPNET\_regbrowsers. exe

No ASP.NET versão 1,1, o arquivo Machine. config continha uma seção chamada &lt;browserCaps&gt;. Esta seção continha uma série de entradas XML que definiram as configurações para vários navegadores com base em uma expressão regular. Para o ASP.NET versão 2,0, um novo. O arquivo de navegador define os parâmetros de um navegador específico usando entradas XML. Você adiciona informações em um novo navegador adicionando um novo. Arquivo de navegador para a pasta localizada em%SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers em seu sistema.

Como um aplicativo não está lendo um arquivo. config toda vez que ele requer informações do navegador, você pode criar um novo. Arquivo de navegador e execute ASPNET\_regbrowsers. exe para adicionar as alterações necessárias ao assembly. Isso permite que o servidor acesse as novas informações do navegador imediatamente para que você não precise desligar nenhum de seus aplicativos para obter as informações. Um aplicativo pode acessar os recursos do navegador por meio da propriedade navegador da HttpRequest atual.

As opções a seguir estão disponíveis ao executar o ASPNET\_regbrowser. exe:

| **Opção** | **Descrição** |
| --- | --- |
| **-?** | Exibe o texto de ajuda do ASPNET\_regbbrowsers. exe na janela de comando. |
| **-i** | Cria o assembly de recursos do navegador de tempo de execução e o instala no cache de assembly global. |
| **-u** | Desinstala o assembly de recursos do navegador de tempo de execução do cache de assembly global. |

## <a name="the-aspnet-compilation-tool---aspnet_compilerexe"></a>A ferramenta de compilação ASP.NET-ASPNET\_Compiler. exe

A ferramenta de compilação ASP.NET pode ser usada de duas maneiras gerais: para compilação in-loco e compilação para implantação, onde um diretório de saída de destino é especificado.

### <a name="compiling-an-application-in-place"></a>[Compilando um aplicativo no local](https://msdn.microsoft.com/library/ms229863.aspx)

A ferramenta de compilação ASP.NET pode compilar um aplicativo em vigor, ou seja, ele imita o comportamento de fazer várias solicitações para o aplicativo, causando, assim, a compilação regular. Os usuários de um site pré-compilado não sofrerão um atraso causado pela compilação da página na primeira solicitação.

Quando você pré-compila um site no local, os seguintes itens se aplicam:

- O site retém seus arquivos e a estrutura de diretórios.
- Você deve ter compiladores para todas as linguagens de programação usadas pelo site no servidor.
- Se algum arquivo falhar na compilação, todo o site falhará na compilação.

Você também pode recompilar um aplicativo no local depois de adicionar novos arquivos de origem a ele. A ferramenta compila apenas os arquivos novos ou alterados, a menos que você inclua a opção **-c** .

> [!NOTE]
> A compilação de um aplicativo que contém um aplicativo aninhado não compila o aplicativo aninhado. O aplicativo aninhado deve ser compilado separadamente.

### <a name="compiling-an-application-for-deployment"></a>[Compilando um aplicativo para implantação](https://msdn.microsoft.com/library/ms229863.aspx)

Você compila um aplicativo para implantação (compilação em um local de destino) especificando o parâmetro targetDir. TargetDir pode ser o local final do aplicativo Web ou o aplicativo compilado pode ser implantado ainda mais. Usar a opção **-u** compila o aplicativo de forma que você possa fazer alterações em determinados arquivos no aplicativo compilado sem recompilá-lo. O ASPNET\_Compiler. exe faz uma distinção entre tipos de arquivo estáticos e dinâmicos e os manipula de maneira diferente ao criar o aplicativo resultante.

- Os tipos de arquivo estáticos são aqueles que não têm um compilador associado ou um provedor de compilação, como arquivos cujos nomes têm extensões como. CSS,. gif,. htm,. html,. jpg,. js e assim por diante. Esses arquivos são simplesmente copiados para o local de destino, com seus locais relativos na estrutura de diretório preservados.
- Os tipos de arquivo dinâmicos são aqueles que têm um compilador ou provedor de compilação associado, incluindo arquivos com extensões de nome de arquivo específicas de ASP.NET como. asax,. ascx,. ashx,. aspx,. browser,. Master e assim por diante. A ferramenta de compilação ASP.NET gera assemblies desses arquivos. Se a opção **-u** for omitida, a ferramenta também criará arquivos com a extensão de nome de arquivo. COMPILADOs que mapeiam os arquivos de origem originais para seu assembly. Para garantir que a estrutura de diretório da origem do aplicativo seja preservada, a ferramenta gera arquivos de espaço reservado nos locais correspondentes no aplicativo de destino.

Você deve usar a opção **-u** para indicar que o conteúdo do aplicativo compilado pode ser modificado. Caso contrário, as modificações subsequentes serão ignoradas ou causarão erros de tempo de execução.

A tabela a seguir descreve como a ferramenta de compilação ASP.NET lida com diferentes tipos de arquivo quando a opção **-u** é incluída.

| **Tipo de arquivo** | **Ação do compilador** |
| --- | --- |
| .ascx, .aspx, .master | Esses arquivos são divididos em marcação e código-fonte, o que inclui arquivos code-behind e qualquer código que esteja entre &lt;script runat = "Server"&gt; elementos. O código-fonte é compilado em assemblies, com nomes que são derivados de um algoritmo de hash e os assemblies são colocados no diretório bin. Qualquer código embutido, ou seja, o código entre o **&lt;%** e%colchetes de **&gt;** , é incluído com marcação e não compilado. Novos arquivos com o mesmo nome que os arquivos de origem são criados para conter a marcação e colocados nos diretórios de saída correspondentes. |
| .ashx, .asmx | Esses arquivos não são compilados e são movidos para os diretórios de saída como estão e não são compilados. Se você quiser que o código do manipulador seja compilado, coloque o código em arquivos de código-fonte no diretório do código do aplicativo\_. |
| . cs,. vb,. jsl,. cpp (sem incluir arquivos code-behind para os tipos de arquivo listados anteriormente) | Esses arquivos são compilados e incluídos como um recurso em assemblies que fazem referência a eles. Os arquivos de origem não são copiados para o diretório de saída. Se um arquivo de código não for referenciado, ele não será compilado. |
| Tipos de arquivo personalizados | Esses arquivos não são compilados. Esses arquivos são copiados para os diretórios de saída correspondentes. |
| Arquivos de código-fonte no subdiretório de código do\_de aplicativo | Esses arquivos são compilados em assemblies e colocados no diretório bin. |
| arquivos. resx e. Resource no aplicativo\_subdiretório GlobalResources | Esses arquivos são compilados em assemblies e colocados no diretório bin. Nenhum aplicativo\_subdiretório GlobalResources é criado no diretório de saída principal, e nenhum arquivo. resx ou. Resources localizado no diretório de origem são copiados para os diretórios de saída. |
| arquivos. resx e. Resource no aplicativo\_subdiretório LocalResources | Esses arquivos não são compilados e são copiados para os diretórios de saída correspondentes. |
| arquivos. Skin no subdiretório de temas do\_de aplicativo | Os arquivos. Skin e os arquivos de tema estáticos não são compilados e copiados para os diretórios de saída correspondentes. |
| . os assemblies de tipos de arquivo estáticos Web. config do navegador já estão presentes no diretório bin | Esses arquivos são copiados como estão nos diretórios de saída. |

A tabela a seguir descreve como a ferramenta de compilação ASP.NET lida com diferentes tipos de arquivo quando a opção **-u** é omitida.

| **Tipo de arquivo** | **Ação do compilador** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Esses arquivos são divididos em marcação e código-fonte, o que inclui arquivos code-behind e qualquer código que esteja entre &lt;script runat = "Server"&gt; elementos. O código-fonte é compilado em assemblies, com nomes que são derivados de um algoritmo de hash. Os assemblies resultantes são colocados no diretório bin. Qualquer código embutido, ou seja, o código entre o **&lt;%** e%colchetes de **&gt;** , é incluído com marcação e não compilado. O compilador cria novos arquivos para conter a marcação com o mesmo nome dos arquivos de origem. Esses arquivos resultantes são colocados no diretório bin. O compilador também cria arquivos com o mesmo nome que os arquivos de origem, mas com a extensão. COMPILAdas que contêm informações de mapeamento. Dos. Os arquivos COMPILADOs são colocados nos diretórios de saída correspondentes ao local original dos arquivos de origem. |
| .ascx | Esses arquivos são divididos em marcação e código-fonte. O código-fonte é compilado em assemblies e colocado no diretório bin, com nomes que são derivados de um algoritmo de hash. Nenhum arquivo de marcação é gerado. |
| . cs,. vb,. jsl,. cpp (sem incluir arquivos code-behind para os tipos de arquivo listados anteriormente) | O código-fonte que é referenciado pelos assemblies gerados de arquivos. ascx,. ashx ou. aspx é compilado em assemblies e colocado no diretório bin. Nenhum arquivo de origem é copiado. |
| Tipos de arquivo personalizados | Esses arquivos são compilados como arquivos dinâmicos. Dependendo do tipo de arquivo no qual eles se baseiam, o compilador pode posicionar arquivos de mapeamento nos diretórios de saída. |
| Arquivos no subdiretório de código do\_de aplicativo | Os arquivos de código-fonte nesse subdiretório são compilados em assemblies e colocados no diretório bin. |
| Arquivos no subdiretório\_do aplicativo GlobalResources | Esses arquivos são compilados em assemblies e colocados no diretório bin. Nenhum aplicativo\_subdiretório GlobalResources é criado no diretório de saída principal. Se o arquivo de configuração especificar AppliesTo = "All", os arquivos. resx e. Resources serão copiados para os diretórios de saída. Eles não serão copiados se forem referenciados por um [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| arquivos. resx e. Resource no aplicativo\_subdiretório LocalResources | Esses arquivos são compilados em assemblies com nomes exclusivos e colocados no diretório bin. Nenhum arquivo. resx ou. Resource é copiado para os diretórios de saída. |
| arquivos. Skin no subdiretório de temas do\_de aplicativo | Os temas são compilados em assemblies e colocados no diretório bin. Os arquivos stub são criados para arquivos. Skin e colocados no diretório de saída correspondente. Arquivos estáticos (como. css) são copiados para os diretórios de saída. |
| . os assemblies de tipos de arquivo estáticos Web. config do navegador já estão presentes no diretório bin | Esses arquivos são copiados como estão para o diretório de saída. |

### <a name="fixed-assembly-names"></a>[Nomes de assembly fixos](https://msdn.microsoft.com/library/ms229863.aspx##)

Alguns cenários, como a implantação de um aplicativo Web usando o Windows Installer MSI, exigem o uso de nomes de arquivo e conteúdo consistentes, bem como estruturas de diretório consistentes para identificar assemblies ou definições de configuração para atualizações. Nesses casos, você pode usar a opção **-fixednames** para especificar que a ferramenta de compilação ASP.NET deve compilar um assembly para cada arquivo de origem em vez de usar o onde várias páginas são compiladas em assemblies. Isso pode levar a um grande número de assemblies, portanto, se você estiver preocupado com a escalabilidade, deverá usar essa opção com cuidado.

### <a name="strong-name-compilation"></a>[Compilação de nome forte](https://msdn.microsoft.com/library/ms229863.aspx##)

As opções **-APTCA**, **-delaysign**, **-keycontainer** e **-keyfile** são fornecidas para que você possa usar o ASPNET\_Compiler. exe para criar assemblies com nome forte sem usar a [ferramenta Strong Name (SN. exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) separadamente. Essas opções correspondem, respectivamente, a **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**e **AssemblyKeyFileAttribute**.

A discussão desses atributos está fora do escopo deste curso.

## <a name="labs"></a>Laboratórios

Cada um dos laboratórios a seguir se baseia nos laboratórios anteriores. Você precisará fazer isso na ordem.

## <a name="lab-1-using-the-configuration-api"></a>Laboratório 1: usando a API de configuração

1. Crie um novo site chamado *mod9lab*.
2. Adicione um novo arquivo de configuração da Web ao site.
3. Adicione o seguinte ao arquivo Web. config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Isso garantirá que você tenha permissão para salvar as alterações no arquivo Web. config.

1. Adicione um novo controle rótulo a default. aspx e altere a ID para **lblDebugStatus**.
2. Adicione um novo controle de botão a default. aspx.
3. Altere a ID do controle de botão para **btnToggleDebug** e o texto para **Ativar/desativar o status de depuração**.
4. Abra a exibição de código do arquivo code-behind de default. aspx e adicione uma instrução **using** para **System. Web. Configuration** da seguinte maneira:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Adicione duas variáveis privadas à classe e uma página\_método Init, conforme mostrado abaixo:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Adicione o seguinte código à página\_carregar:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Salve e procure default. aspx. Observe que o controle rótulo exibe o status de depuração atual.
2. Clique duas vezes no controle de botão no designer e adicione o seguinte código ao evento de clique para o controle de botão:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Salve e procure default. aspx e clique no botão.
2. Abra o arquivo Web. config depois de cada clique no botão e observe o atributo **debug** na seção &lt;compilação&gt;.

## <a name="lab-2-logging-application-restarts"></a>Laboratório 2: log de reinicializações do aplicativo

Neste laboratório, você criará um código que permitirá que você alterne o log de desligamentos, inicializações e recompilações de aplicativos no Visualizador de Eventos.

1. Adicione uma DropDownList a default. aspx e altere a ID para ddlLogAppEvents.
2. Defina a propriedade **AutoPostBack** para DropDownList como **true**.
3. Adicione três itens à coleção de itens para a DropDownList. Transforme o **texto** para o primeiro item *selecione Value* e o valor-1. Torne o **texto** e o **valor** do segundo item **verdadeiro** e o **texto** e o **valor** do terceiro item **falso**.
4. Adicione um novo rótulo a default. aspx. Altere a ID para **lblLogAppEvents**.
5. Abra a exibição code-behind para default. aspx e adicione uma nova declaração para uma variável do tipo HealthMonitoringSection, conforme mostrado abaixo:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Adicione o código a seguir ao código existente na página\_init:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Clique duas vezes em DropDownList e adicione o seguinte código ao evento SelectedIndexchanged:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Procure default. aspx.
2. Defina a lista suspensa como **false**.
3. Limpe o log do aplicativo no Visualizador de Eventos.
4. Clique no botão para alterar o atributo de depuração do aplicativo.
5. Atualize o log do aplicativo no Visualizador de Eventos. 

    1. Algum evento foi registrado?
    2. Por que ou por que não?
6. Defina a lista suspensa como **true.**
7. Clique no botão para alternar o atributo de depuração do aplicativo.
8. Atualize o logon do aplicativo na Visualizador de Eventos. 

    1. Algum evento foi registrado?
    2. Qual foi o motivo do desligamento do aplicativo?
9. Experimente ativar e desativar o registro em log e examinar as alterações feitas no arquivo Web. config.

## <a name="more-information"></a>Obter mais informações:

O modelo de provedor do ASP.NET 2.0 permite que você crie seus próprios provedores para não apenas a instrumentação de aplicativo, mas para muitos outros usos, bem como associação, perfis, etc. Para obter informações detalhadas sobre como gravar um provedor personalizado para registrar eventos de aplicativo em um arquivo de texto, visite [este link](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
