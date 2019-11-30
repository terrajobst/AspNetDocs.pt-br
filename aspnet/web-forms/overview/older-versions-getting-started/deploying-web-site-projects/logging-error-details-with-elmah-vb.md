---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
title: Registrando detalhes do erro com o ELMAH (VB) | Microsoft Docs
author: rick-anderson
description: Os módulos e manipuladores de log de erros (ELMAH) oferecem outra abordagem para registrar em log erros em tempo de execução em um ambiente de produção. O ELMAH é um erro de código aberto gratuito...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: a5f0439f-18b2-4c89-96ab-75b02c616f46
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
msc.type: authoredcontent
ms.openlocfilehash: 46b7fc22807c8cb9f47ff035639815d7b6104735
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74622428"
---
# <a name="logging-error-details-with-elmah-vb"></a>Registro em log de detalhes de erros com o ELMAH (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_VB.zip) ou [baixar PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_vb.pdf)

> Os módulos e manipuladores de log de erros (ELMAH) oferecem outra abordagem para registrar em log erros em tempo de execução em um ambiente de produção. O ELMAH é uma biblioteca de registro em log de erros de software livre que inclui recursos como filtragem de erros e a capacidade de exibir o log de erros de uma página da Web, como um RSS feed, ou para baixá-lo como um arquivo delimitado por vírgulas. Este tutorial percorre o download e a configuração do ELMAH.

## <a name="introduction"></a>Introdução

O [tutorial anterior](logging-error-details-with-asp-net-health-monitoring-vb.md) examinou ASP. O sistema de monitoramento de integridade da rede, que oferece uma biblioteca pronta para gravação de uma ampla gama de eventos da Web. Muitos desenvolvedores usam o monitoramento de integridade para registrar em log e enviar por email os detalhes de exceções sem tratamento. No entanto, há alguns pontos problemáticos com esse sistema. Em primeiro lugar, a falta de qualquer tipo de interface do usuário para exibir informações sobre os eventos registrados. Se você quiser ver um resumo dos 10 últimos erros ou exibir os detalhes de um erro que ocorreu na semana passada, você deve fazer uma busca no banco de dados, examinar sua caixa de entrada de email ou criar uma página da Web que exiba informações da tabela `aspnet_WebEvent_Events`.

Outro ponto problemático se concentra na complexidade do monitoramento de integridade. Como o monitoramento de integridade pode ser usado para registrar uma grande quantidade de eventos diferentes, e como há uma variedade de opções para instruir como e quando os eventos são registrados, configurar corretamente o sistema de monitoramento de integridade pode ser uma tarefa oneroso. Por fim, há problemas de compatibilidade. Como o monitoramento de integridade foi adicionado pela primeira vez ao .NET Framework na versão 2,0, ele não está disponível para aplicativos Web mais antigos criados usando ASP.NET versão 1. x. Além disso, a classe `SqlWebEventProvider`, que usamos no tutorial anterior para registrar detalhes de erro em um banco de dados, funciona apenas com bancos de Microsoft SQL Server. Você precisará criar uma classe de provedor de log personalizada, caso precise registrar erros em um repositório de dados alternativo, como um arquivo XML ou um Oracle Database.

Uma alternativa ao sistema de monitoramento de integridade são os módulos e manipuladores de log de erros (ELMAH), um sistema de registro em log de erros de código aberto gratuito criado por [Atif Aziz](http://www.raboof.com/). A diferença mais notável entre os dois sistemas é a capacidade do ELAMH de exibir uma lista de erros e os detalhes de um erro específico de uma página da Web e de um RSS feed. O ELMAH é mais fácil de configurar do que o monitoramento de integridade porque apenas registra erros. Além disso, o ELMAH inclui suporte para aplicativos ASP.NET 1. x, ASP.NET 2,0 e ASP.NET 3,5 e é fornecido com uma variedade de provedores de origem de log.

Este tutorial percorre as etapas envolvidas na adição do ELMAH a um aplicativo ASP.NET. Vamos começar!

> [!NOTE]
> O sistema de monitoramento de integridade e o ELMAH têm seus próprios conjuntos de prós e contras. Recomendo que você experimente ambos os sistemas e decida qual deles é mais adequado às suas necessidades.

## <a name="adding-elmah-to-an-aspnet-web-application"></a>Adicionando o ELMAH a um aplicativo Web ASP.NET

A integração do ELMAH em um aplicativo ASP.NET novo ou existente é um processo fácil e direto que leva menos de cinco minutos. Resumindo, envolve quatro etapas simples:

1. Baixe o ELMAH e adicione o assembly `Elmah.dll` ao seu aplicativo Web,
2. Registrar módulos e manipuladores HTTP do ELMAH no `Web.config`,
3. Especifique as opções de configuração do ELMAH e
4. Crie a infraestrutura de origem de log de erros, se necessário.

Vamos examinar cada uma dessas quatro etapas, uma de cada vez.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Etapa 1: baixando os arquivos de projeto do ELMAH e adicionando`Elmah.dll`ao seu aplicativo Web

O ELMAH 1,0 BETA 3 (Build 10617), a versão mais recente no momento da escrita, está incluído no download disponível com este tutorial. Como alternativa, você pode visitar o [site do ELMAH](https://code.google.com/p/elmah/) para obter a versão mais recente ou baixar o código-fonte. Extraia o download do ELMAH para uma pasta em sua área de trabalho e localize o arquivo de assembly do ELMAH (`Elmah.dll`).

> [!NOTE]
> O arquivo de `Elmah.dll` está localizado na pasta `Bin` do download, que tem subpastas para diferentes versões de .NET Framework e para compilações de versão e depuração. Use a compilação de versão para a versão apropriada do Framework. Por exemplo, se você estiver criando um aplicativo Web ASP.NET 3,5, copie o arquivo `Elmah.dll` da pasta `Bin\net-3.5\Release`.

Em seguida, abra o Visual Studio e adicione o assembly ao seu projeto clicando com o botão direito do mouse no nome do site na Gerenciador de Soluções e escolhendo Adicionar referência no menu de contexto. Isso abre a caixa de diálogo Adicionar referência. Navegue até a guia procurar e escolha o arquivo de `Elmah.dll`. Essa ação adiciona o arquivo de `Elmah.dll` à pasta `Bin` do aplicativo Web.

> [!NOTE]
> O tipo de projeto de aplicativo Web (WAP) não mostra a pasta `Bin` no Gerenciador de Soluções. Em vez disso, ele lista esses itens na pasta References.

O assembly `Elmah.dll` inclui as classes usadas pelo sistema ELMAH. Essas classes se enquadram em uma das três categorias:

- **Módulos http** – um módulo HTTP é uma classe que define manipuladores de eventos para eventos de `HttpApplication`, como o evento `Error`. O ELMAH inclui vários módulos HTTP, os três mais alemãs que estão sendo: 

    - `ErrorLogModule`-registra exceções sem tratamento em uma origem de log.
    - `ErrorMailModule` – envia os detalhes de uma exceção sem tratamento em uma mensagem de email.
    - `ErrorFilterModule` – aplica filtros especificados pelo desenvolvedor para determinar quais exceções são registradas e quais são ignoradas.
- **Manipuladores HTTP** – um manipulador http é uma classe que é responsável por gerar a marcação para um tipo específico de solicitação. O ELMAH inclui manipuladores HTTP que processam detalhes de erro como uma página da Web, como um RSS feed ou como um CSV (arquivo delimitado por vírgula).
- **Fontes de log de erros** -a partir da caixa o ELMAH pode registrar erros na memória, em um banco de dados Microsoft SQL Server, em um banco de dados do Microsoft Access, em um banco de dados Oracle, em um arquivo XML, em um banco de dados SQLite ou em um banco de dados do vista DB. Como o sistema de monitoramento de integridade, a arquitetura do ELMAH foi criada usando o modelo de provedor, o que significa que você pode criar e integrar perfeitamente seus próprios provedores de origem de log personalizados, se necessário.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Etapa 2: registrando o módulo e o manipulador HTTP do ELMAH

Embora o arquivo de `Elmah.dll` contenha os módulos HTTP e o manipulador necessários para registrar automaticamente as exceções sem tratamento e para exibir detalhes de erro de uma página da Web, eles devem ser explicitamente registrados na configuração do aplicativo Web. O módulo HTTP `ErrorLogModule`, uma vez registrado, assina o evento de `Error` do `HttpApplication`. Sempre que esse evento for acionado, o `ErrorLogModule` registrará em log os detalhes da exceção em uma fonte de logs especificada. Veremos como definir o provedor de origem de log na próxima seção, "Configurando o ELMAH". A fábrica de manipuladores HTTP `ErrorLogPageFactory` é responsável por gerar a marcação ao exibir o log de erros de uma página da Web.

A sintaxe específica para registrar módulos e manipuladores HTTP depende do servidor Web que está ligando o site. Para o ASP.NET Development Server e o IIS da Microsoft versão 6,0 e anteriores, os módulos e manipuladores HTTP são registrados nas seções `<httpModules>` e `<httpHandlers>`, que aparecem dentro do elemento `<system.web>`. Se você estiver usando o IIS 7,0, eles precisarão ser registrados nas seções `<modules>` e `<handlers>` do elemento de `<system.webServer>`. Felizmente, você pode definir os módulos e manipuladores HTTP em *ambos os* locais, independentemente do servidor Web que está sendo usado. Essa opção é a mais portátil, pois permite que a mesma configuração seja usada nos ambientes de desenvolvimento e produção, independentemente do servidor Web que está sendo usado.

Comece registrando o módulo HTTP `ErrorLogModule` e o manipulador HTTP `ErrorLogPageFactory` na seção `<httpModules>` e `<httpHandlers>` no `<system.web>`. Se sua configuração já definir esses dois elementos, simplesmente inclua o elemento `<add>` para o módulo e o manipulador HTTP do ELMAH.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample1.xml)]

Em seguida, registre o módulo e o manipulador HTTP do ELMAH no elemento `<system.webServer>`. Como antes, se esse elemento ainda não estiver presente em sua configuração, adicione-o.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample2.xml)]

Por padrão, o IIS 7 reclama se os módulos e manipuladores HTTP estiverem registrados na seção `<system.web>`. O atributo `validateIntegratedModeConfiguration` no elemento `<validation>` instrui o IIS 7 a suprimir essas mensagens de erro.

Observe que a sintaxe para registrar o manipulador HTTP `ErrorLogPageFactory` inclui um atributo `path`, que é definido como `elmah.axd`. Esse atributo informa ao aplicativo Web que se uma solicitação chega para uma página chamada `elmah.axd`, a solicitação deve ser processada pelo manipulador HTTP `ErrorLogPageFactory`. Veremos o manipulador HTTP `ErrorLogPageFactory` em ação posteriormente neste tutorial.

### <a name="step-3-configuring-elmah"></a>Etapa 3: Configurando o ELMAH

O ELMAH procura suas opções de configuração no arquivo de `Web.config` do site em uma seção de configuração personalizada chamada `<elmah>`. Para usar uma seção personalizada em `Web.config` ela deve primeiro ser definida no elemento `<configSections>`. Abra o arquivo `Web.config` e adicione a seguinte marcação ao `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample3.xml)]

> [!NOTE]
> Se você estiver configurando o ELMAH para um aplicativo ASP.NET 1. x, remova o atributo `requirePermission="false"` dos elementos `<section>` acima.

A sintaxe acima registra a seção `<elmah>` personalizada e suas subseções: `<security>`, `<errorLog>`, `<errorMail>`e `<errorFilter>`.

Em seguida, adicione a seção `<elmah>` ao `Web.config`. Esta seção deve aparecer no mesmo nível que o elemento `<system.web>`. Na seção `<elmah>`, adicione as seções `<security>` e `<errorLog>` desta forma:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample4.xml)]

O atributo `allowRemoteAccess` da seção `<security>` indica se o acesso remoto é permitido. Se esse valor for definido como 0, a página da Web de log de erros só poderá ser exibida localmente. Se esse atributo for definido como 1, a página da Web de log de erros será habilitada para os visitantes remotos e locais. Por enquanto, vamos desabilitar a página da Web de log de erros para visitantes remotos. Vamos permitir o acesso remoto posteriormente, depois que tivermos uma oportunidade para discutir as preocupações de segurança de fazer isso.

A seção `<errorLog>` define a origem do log de erros, que determina onde os detalhes do erro são gravados; é semelhante à seção `<providers>` no sistema de monitoramento de integridade. A sintaxe acima Especifica a classe `SqlErrorLog` como a origem do log de erros, que registra os erros em um banco de dados Microsoft SQL Server especificado pelo valor do atributo `connectionStringName`.

> [!NOTE]
> O ELMAH é fornecido com provedores de log de erros adicionais que podem ser usados para registrar erros em um arquivo XML, em um banco de dados do Microsoft Access, em um Oracle Database e em outros repositórios de armazenamento. Consulte o arquivo de `Web.config` de exemplo que está incluído no download do ELMAH para obter informações sobre como usar esses provedores de log de erros alternativos.

### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Etapa 4: criando a infraestrutura de origem de log de erros

O provedor de `SqlErrorLog` do ELMAH registra detalhes de erro em um banco de dados Microsoft SQL Server especificado. O provedor de `SqlErrorLog` espera que esse banco de dados tenha uma tabela chamada `ELMAH_Error` e três procedimentos armazenados: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`e `ELMAH_LogError`. O download do ELMAH inclui um arquivo chamado `SQLServer.sql` na pasta `db` que contém o T-SQL para criar essa tabela e esses procedimentos armazenados. Você precisará executar essas instruções em seu banco de dados para usar o provedor de `SqlErrorLog`.

As **figuras 1** e **2** mostram o Gerenciador de banco de dados no Visual Studio após a adição dos objetos de banco de dados necessários pelo provedor de `SqlErrorLog`.

[![](logging-error-details-with-elmah-vb/_static/image2.png)](logging-error-details-with-elmah-vb/_static/image1.png)

**Figura 1**: o provedor de `SqlErrorLog` registra erros na tabela `ELMAH_Error`

[![](logging-error-details-with-elmah-vb/_static/image4.png)](logging-error-details-with-elmah-vb/_static/image3.png)

**Figura 2**: o provedor de `SqlErrorLog` usa três procedimentos armazenados

## <a name="elmah-in-action"></a>ELMAH em ação

Neste ponto, adicionamos o ELMAH ao aplicativo Web, registramos o módulo HTTP `ErrorLogModule` e o manipulador HTTP `ErrorLogPageFactory`, especificamos as opções de configuração do ELMAH no `Web.config`e adicionamos os objetos de banco de dados necessários para o provedor de log de erros `SqlErrorLog`. Agora estamos prontos para ver o ELMAH em ação! Visite o site de análises de livros e visite uma página que gera um erro de tempo de execução, como `Genre.aspx?ID=foo`ou uma página não existente, como `NoSuchPage.aspx`. O que você vê ao visitar essas páginas depende da configuração de `<customErrors>` e se você está visitando local ou remotamente. (Consulte novamente o tutorial [ *exibindo uma página de erro personalizada* ](displaying-a-custom-error-page-vb.md) para obter um atualizador sobre este tópico.)

O ELMAH não afeta o conteúdo mostrado ao usuário quando ocorre uma exceção sem tratamento; Ele apenas registra seus detalhes. Esse log de erros pode ser acessado na página da Web `elmah.axd` da raiz do seu site, como `http://localhost/BookReviews/elmah.axd`. (Esse arquivo não existe fisicamente no seu projeto, mas quando uma solicitação chega por `elmah.axd` o tempo de execução o distribui para o manipulador HTTP `ErrorLogPageFactory`, que gera a marcação enviada de volta para o navegador.)

> [!NOTE]
> Você também pode usar a página `elmah.axd` para instruir o ELMAH a gerar um erro de teste. A visita `elmah.axd/test` (como em, `http://localhost/BookReviews/elmah.axd/test`) faz com que o ELMAH lance uma exceção do tipo `Elmah.TestException`, que tem a mensagem de erro: "esta é uma exceção de teste que pode ser ignorada com segurança".

A **Figura 3** mostra o log de erros ao visitar `elmah.axd` do ambiente de desenvolvimento.

[![](logging-error-details-with-elmah-vb/_static/image6.png)](logging-error-details-with-elmah-vb/_static/image5.png)

**Figura 3**: `Elmah.axd` exibe o log de erros de uma página da Web  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image7.png))

O log de erros da **Figura 3** contém seis entradas de erro. Cada entrada inclui o código de status HTTP (404 ou 500, para esses erros), o tipo, a descrição, o nome do usuário conectado quando o erro ocorreu e a data e hora. Clicar no link detalhes exibe uma página que inclui a mesma mensagem de erro mostrada na tela amarela de detalhes do erro (consulte a **Figura 4**) junto com os valores das variáveis de servidor no momento do erro (consulte a **Figura 5**). Você também pode exibir o XML bruto no qual os detalhes do erro são salvos, o que inclui informações adicionais, como os valores no cabeçalho HTTP POST.

[![](logging-error-details-with-elmah-vb/_static/image9.png)](logging-error-details-with-elmah-vb/_static/image8.png)

**Figura 4**: exibir os detalhes do erro YSOD  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image10.png))

[![](logging-error-details-with-elmah-vb/_static/image12.png)](logging-error-details-with-elmah-vb/_static/image11.png)

**Figura 5**: explorar os valores da coleção de variáveis de servidor no momento do erro  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image13.png))

A implantação do ELMAH no site de produção envolve:

- Copiar o arquivo de `Elmah.dll` para a pasta `Bin` na produção,
- Copiar as definições de configuração específicas do ELMAH para o arquivo de `Web.config` usado na produção e
- Adicionar a infraestrutura de origem do log de erros ao banco de dados de produção.

Exploramos técnicas para copiar arquivos de desenvolvimento para produção em Tutoriais anteriores. Talvez a maneira mais fácil de obter a infraestrutura de origem de log de erros no banco de dados de produção é usar SQL Server Management Studio para se conectar ao banco de dados de produção e, em seguida, executar o arquivo de script de `SqlServer.sql`, que criará a tabela necessária e os procedimentos armazenados.

### <a name="viewing-the-error-details-page-on-production"></a>Exibindo a página de detalhes do erro na produção

Depois de implantar seu site para produção, visite o site de produção e gere uma exceção sem tratamento. Como no ambiente de desenvolvimento, o ELMAH não tem nenhum efeito na página de erro exibida quando ocorre uma exceção sem tratamento; em vez disso, ele simplesmente registra o erro. Se você tentar visitar a página log de erros (`elmah.axd`) do ambiente de produção, você será saudado com a página proibido mostrada na **Figura 6**.

[![](logging-error-details-with-elmah-vb/_static/image15.png)](logging-error-details-with-elmah-vb/_static/image14.png)

**Figura 6**: por padrão, os visitantes remotos não podem exibir a página da Web de log de erros  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image16.png))

Lembre-se de que na seção `<security>` da configuração do ELMAH, definimos o atributo `allowRemoteAccess` como 0, o que proíbe que os usuários remotos exibam o log de erros. É importante proibir os visitantes anônimos de exibir o log de erros, pois os detalhes do erro podem revelar vulnerabilidades de segurança ou outras informações confidenciais. Se você decidir definir esse atributo como 1 e habilitar o acesso remoto ao log de erros, será importante bloquear o `elmah.axd` caminho para que somente os visitantes autorizados possam acessá-lo. Isso pode ser obtido com a adição de um elemento `<location>` ao arquivo `Web.config`.

A configuração a seguir permite que somente os usuários na função de administrador acessem a página da Web de log de erros:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample5.xml)]

> [!NOTE]
> A função de administrador e os três usuários no sistema-Scott, Jisun e Alice-foram adicionados no [tutorial *Configurando um site que usa serviços de aplicativos* ](configuring-a-website-that-uses-application-services-vb.md). Os usuários Scott e Jisun são membros da função de administrador. Para obter mais informações sobre autenticação e autorização, consulte meus [tutoriais de segurança do site](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

O log de erros no ambiente de produção agora pode ser exibido por usuários remotos; consulte novamente as **figuras 3**, **4**e **5** para capturas de tela da página da Web de log de erros. No entanto, se um usuário anônimo ou não administrador tentar exibir a página de log de erros, ele será redirecionado automaticamente para a página de logon (`Login.aspx`), como mostra a **Figura 7** .

[![](logging-error-details-with-elmah-vb/_static/image18.png)](logging-error-details-with-elmah-vb/_static/image17.png)

**Figura 7**: usuários não autorizados são automaticamente redirecionados para a página de logon  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Erros de log programaticamente

O módulo HTTP `ErrorLogModule` do ELMAH registra automaticamente as exceções sem tratamento na origem de log especificada. Como alternativa, você pode registrar um erro sem ter que gerar uma exceção não tratada usando a classe `ErrorSignal` e seu método `Raise`. O método `Raise` é passado um objeto `Exception` e o registra como se essa exceção tivesse sido gerada e atingiu o tempo de execução ASP.NET sem ser manipulado. No entanto, a diferença é que a solicitação continua executando normalmente depois que o método `Raise` foi chamado, enquanto uma exceção gerada, sem tratamento, interrompe a execução normal da solicitação e faz com que o tempo de execução ASP.NET exiba a página de erro configurada.

A classe `ErrorSignal` é útil em situações em que há alguma ação que pode falhar, mas sua falha não é catastrófica para a operação geral que está sendo executada. Por exemplo, um site pode conter um formulário que usa a entrada do usuário, armazena-o em um banco de dados e envia ao usuário um email informando que as informações foram processadas. O que deve acontecer se as informações forem salvas no banco de dados com êxito, mas houver um erro ao enviar a mensagem de email? Uma opção seria lançar uma exceção e enviar o usuário para a página de erro. No entanto, isso pode confundir o usuário a pensar que as informações inseridas não foram salvas. Outra abordagem seria registrar o erro relacionado ao email, mas não alterar a experiência do usuário de nenhuma forma. É aí que a classe `ErrorSignal` é útil.

[!code-vb[Main](logging-error-details-with-elmah-vb/samples/sample6.vb)]

## <a name="error-notification-via-email"></a>Notificação de erro por email

Juntamente com erros de log em um banco de dados, o ELMAH também pode ser configurado para enviar por email detalhes de erro para um destinatário especificado. Essa funcionalidade é fornecida pelo módulo HTTP `ErrorMailModule`; Portanto, você deve registrar esse módulo HTTP no `Web.config` para enviar detalhes do erro por email.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample7.xml)]

Em seguida, especifique as informações sobre o email de erro na seção `<errorMail>` do elemento de `<elmah>`, indicando o remetente e o destinatário do email, o assunto e se o email é enviado de forma assíncrona.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample8.xml)]

Com as configurações acima em vigor, sempre que ocorrer um erro de tempo de execução, o ELMAH envia um email para support@example.com com os detalhes do erro. O email de erro do ELMAH inclui as mesmas informações mostradas na página da Web detalhes do erro, ou seja, a mensagem de erro, o rastreamento de pilha e as variáveis de servidor (consulte novamente as **figuras 4** e **5**). O email de erro também inclui os detalhes da exceção tela amarela do conteúdo da morte como um anexo (`YSOD.html`).

A **Figura 8** mostra o email de erro do ELMAH gerado visitando `Genre.aspx?ID=foo`. Enquanto a **Figura 8** mostra apenas a mensagem de erro e o rastreamento de pilha, as variáveis de servidor são incluídas mais abaixo no corpo do email.

[![](logging-error-details-with-elmah-vb/_static/image21.png)](logging-error-details-with-elmah-vb/_static/image20.png)

**Figura 8**: você pode configurar o ELMAH para enviar detalhes do erro por email  
([Clique para exibir a imagem em tamanho normal](logging-error-details-with-elmah-vb/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Apenas erros de log de interesse

Por padrão, o ELMAH registra em log os detalhes de cada exceção sem tratamento, incluindo 404 e outros erros de HTTP. Você pode instruir o ELMAH a ignorar esses ou outros tipos de erros usando a filtragem de erros. A lógica de filtragem é executada pelo módulo HTTP `ErrorFilterModule` do ELMAH, que você precisará registrar no `Web.config` para usar a lógica de filtragem. As regras para filtragem são especificadas na seção `<errorFilter>`.

A marcação a seguir instrui o ELMAH a não registrar erros 404.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample9.xml)]

> [!NOTE]
> Não se esqueça de que, para usar a filtragem de erros, você deve registrar o módulo HTTP `ErrorFilterModule`.

O elemento `<equal>` dentro da seção `<test>` é conhecido como uma asserção. Se a asserção for avaliada como true, o erro será filtrado do log do ELMAH. Há outras declarações disponíveis, incluindo: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`e assim por diante. Você também pode combinar asserções usando os operadores boolianos `<and>` e `<or>`. Além de tudo, você pode até mesmo incluir uma expressão JavaScript simples como uma asserção ou escrever suas próprias asserções no C# ou Visual Basic.

Para obter mais informações sobre os recursos de filtragem de erro do ELMAH, consulte a [seção filtragem de erros](https://code.google.com/p/elmah/wiki/ErrorFiltering) no [wiki do ELMAH](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Resumo

O ELMAH fornece um mecanismo simples, mas poderoso, para registrar erros em um aplicativo Web ASP.NET. Como o sistema de monitoramento de integridade da Microsoft, o ELMAH pode registrar erros em um banco de dados e pode enviar os detalhes do erro para um desenvolvedor por email. Diferentemente do sistema de monitoramento de integridade, o ELMAH inclui suporte pronto para uma maior variedade de armazenamentos de dados de log de erros, incluindo: Microsoft SQL Server, Microsoft Access, Oracle, arquivos XML e vários outros. Além disso, o ELMAH oferece um mecanismo interno para exibir o log de erros e detalhes sobre um erro específico de uma página da Web, `elmah.axd`. A página de `elmah.axd` também pode renderizar informações de erro como um RSS feed ou como um CSV (arquivo de valores separados por vírgula), que você pode ler usando o Microsoft Excel. Você também pode instruir o ELMAH a filtrar erros do log usando asserções declarativas ou programáticas. E o ELMAH pode ser usado com os aplicativos ASP.NET versão 1. x.

Cada aplicativo implantado deve ter algum mecanismo para registrar automaticamente as exceções sem tratamento e enviar notificações para a equipe de desenvolvimento. Se isso é feito usando o monitoramento de integridade ou o ELMAH é secundário. Em outras palavras, não importa muito se você usa o monitoramento de integridade ou o ELMAH; Avalie ambos os sistemas e escolha aquele que melhor atenda às suas necessidades. O que é fundamentalmente importante é que algum mecanismo seja colocado em vigor para registrar exceções sem tratamento no ambiente de produção.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [ELMAH-módulos e manipuladores de log de erros](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [Página do projeto do ELMAH](https://code.google.com/p/elmah/) (código-fonte, amostras, wiki)
- [Conectando o ELMAH em um aplicativo Web para capturar exceções sem tratamento](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (vídeo)
- [Páginas de log de erros de segurança](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Usando módulos e manipuladores HTTP para criar componentes ASP.NET conectáveis](https://msdn.microsoft.com/library/aa479332.aspx)
- [Tutoriais de segurança do site](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Anterior](logging-error-details-with-asp-net-health-monitoring-vb.md)
> [Próximo](precompiling-your-website-vb.md)
