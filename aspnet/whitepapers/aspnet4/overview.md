---
uid: whitepapers/aspnet4/overview
title: Visão geral de desenvolvimento para a Web do ASP.NET 4 e do Visual Studio 2010 | Microsoft Docs
author: rick-anderson
description: Este documento fornece uma visão geral de muitos dos novos recursos do ASP.NET que estão incluídos no the.NET Framework 4 e no Visual Studio 2010.
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 8c93952adb33d1ce7008ebff9d032a71eb2a5f74
ms.sourcegitcommit: b67ffd5b2c5cff01ec4c8eb12a21f693f2e11887
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69995351"
---
# <a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>Visão geral sobre desenvolvimento para a Web do ASP.NET 4 e Visual Studio 2010

> Este documento fornece uma visão geral de muitos dos novos recursos do ASP.NET que estão incluídos no the.NET Framework 4 e no Visual Studio 2010.
> 
> [Baixe este White Paper](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)

**Conteúdo**

**[Core Services](#0.2__Toc253429238 "_Toc253429238")**  
Refatoração de [arquivo Web. config] (#0.2__Toc253429239 "_Toc253429239")  
[Cache de saída extensível] (#0.2__Toc253429240 "_Toc253429240")  
[Aplicativos Web de início automático] (#0.2__Toc253429241 "_Toc253429241")  
[Redirecionando permanentemente uma página] (#0.2__Toc253429242 "_Toc253429242")  
[Reduzindo o estado da sessão] (#0.2__Toc253429243 "_Toc253429243")  
[Expandindo o intervalo de URLs permitidas] (#0.2__Toc253429244 "_Toc253429244")  
[Validação de solicitação extensível] (#0.2__Toc253429245 "_Toc253429245")  
[Cache de objetos e extensibilidade de objetos de cache] (#0.2__Toc253429246 "_Toc253429246")  
[Codificação de Extensible HTML, URL e cabeçalho http] (#0.2__Toc253429247 "_Toc253429247")  
[Monitoramento de desempenho para aplicativos individuais em um único processo de trabalho] (#0.2__Toc253429248 "_Toc253429248")  
[Multi-Targeting](#0.2__Toc253429249 "_Toc253429249")

**[Ajax](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery incluído com Web Forms e MVC] (#0.2__Toc253429251 "_Toc253429251")  
[Suporte à rede de distribuição de conteúdo] (#0.2__Toc253429252 "_Toc253429252")  
[Scripts explícitos do ScriptManager] (#0.2__Toc253429253 "_Toc253429253")

**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[Definindo marcas meta com as propriedades Page. Metapalavra-chave e Page. MetaDescription] (#0.2__Toc253429257 "_Toc253429257")  
[Habilitando o estado de exibição para controles individuais] (#0.2__Toc253429258 "_Toc253429258")  
[Alterações nos recursos do navegador] (#0.2__Toc253429259 "_Toc253429259")  
[Roteamento no ASP.NET 4] (#0.2__Toc253429260 "_Toc253429260")  
[Definindo IDs de cliente] (#0.2__Toc253429261 "_Toc253429261")  
[Persistência da seleção de linha em controles de dados] (#0.2__Toc253429262 "_Toc253429262")  
[Controle de gráfico ASP.net] (#0.2__Toc253429263 "_Toc253429263")  
[Filtrando dados com o controle QueryExtender] (#0.2__Toc253429264 "_Toc253429264")  
[Expressões de código codificadas em HTML] (#0.2__Toc253429265 "_Toc253429265")  
[Alterações de modelo de projeto] (#0.2__Toc253429266 "_Toc253429266")  
[Aprimoramentos de CSS] (#0.2__Toc253429267 "_Toc253429267")  
[Ocultando elementos div em campos ocultos] (#0.2__Toc253429268 "_Toc253429268")  
[Renderizando uma tabela externa para controles de modelo] (#0.2__Toc253429269 "_Toc253429269")  
[Aprimoramentos de controle ListView] (#0.2__Toc253429270 "_Toc253429270")  
[Aprimoramentos no controle CheckBoxList e RadioButtonList] (#0.2__Toc253429271 "_Toc253429271")  
[Aprimoramentos de controle de menu] (#0.2__Toc253429272 "_Toc253429272")  
[Assistente e controles CreateUserWizard 56] (#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Suporte a áreas] (#0.2__Toc253429275 "_Toc253429275")  
[Suporte à validação de atributo de anotação de dados] (#0.2__Toc253429276 "_Toc253429276")  
[Auxiliares] modelados (#0.2__Toc253429277 "_Toc253429277")

**[Dynamic Data](#0.2__Toc253429278 "_Toc253429278")**  
[Habilitando dados dinâmicos para projetos existentes] (#0.2__Toc253429279 "_Toc253429279")  
[Sintaxe de controle declarativo DynamicDataManager] (#0.2__Toc253429280 "_Toc253429280")  
[Modelos de entidade] (#0.2__Toc253429281 "_Toc253429281")  
[Novos modelos de campo para URLs e endereços de email] (#0.2__Toc253429282 "_Toc253429282")  
[Criando links com o controle DynamicHyperLink] (#0.2__Toc253429283 "_Toc253429283")  
[Suporte para herança no modelo de dados] (#0.2__Toc253429284 "_Toc253429284")  
[Suporte para relações muitos para muitos (somente Entity Framework)] (#0.2__Toc253429285 "_Toc253429285")  
[Novos atributos para controlar as enumerações de exibição e suporte] (#0.2__Toc253429286 "_Toc253429286")  
[Suporte aprimorado para filtros] (#0.2__Toc253429287 "_Toc253429287")

**[Aprimoramentos no desenvolvimento para a Web do Visual Studio 2010] (#0.2__Toc253429288 "_Toc253429288")**  
[Compatibilidade com CSS aprimorada] (#0.2__Toc253429289 "_Toc253429289")  
[Trechos de código HTML e JavaScript] (#0.2__Toc253429290 "_Toc253429290")  
[Aprimoramentos de IntelliSense do JavaScript] (#0.2__Toc253429291 "_Toc253429291")

**[Implantação de aplicativo Web com o Visual Studio 2010] (#0.2__Toc253429292 "_Toc253429292")**  
[Web Packaging](#0.2__Toc253429293 "_Toc253429293")  
[Web.config Transformation](#0.2__Toc253429294 "_Toc253429294")  
[Implantação de banco de dados] (#0.2__Toc253429295 "_Toc253429295")  
[Publicação com um clique para aplicativos Web] (#0.2__Toc253429296 "_Toc253429296")  
[Resources](#0.2__Toc253429297 "_Toc253429297")

**[Disclaimer](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Serviços principais

O ASP.NET 4 introduz uma série de recursos que melhoram os principais serviços ASP.NETs, como cache de saída e armazenamento de estado de sessão.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Refatoração de arquivo Web. config

O `Web.config` arquivo que contém a configuração de um aplicativo Web cresceu consideravelmente nas últimas versões do .NET Framework à medida que novos recursos foram adicionados, como AJAX, roteamento e integração com o IIS 7. Isso dificultou a configuração ou a inicialização de novos aplicativos Web sem uma ferramenta como o Visual Studio. No. o .NET Framework 4, os principais elementos de configuração foram movidos para `machine.config` o arquivo, e os aplicativos agora herdam essas configurações. Isso permite que `Web.config` o arquivo em ASP.NET 4 aplicativos seja vazio ou contenha apenas as seguintes linhas, que especificam para o Visual Studio qual versão do Framework o aplicativo está direcionando:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>Cache de saída extensível

Desde o lançamento do ASP.NET 1,0, o cache de saída permitiu que os desenvolvedores armazenassem a saída gerada de páginas, controles e respostas HTTP na memória. Em solicitações da Web subsequentes, o ASP.NET pode fornecer conteúdo mais rapidamente recuperando a saída gerada da memória em vez de regenerar a saída do zero. No entanto, essa abordagem tem uma limitação — o conteúdo gerado sempre precisa ser armazenado na memória e, em servidores que estão apresentando tráfego pesado, a memória consumida pelo cache de saída pode competir com demandas de memória de outras partes de um aplicativo Web.

O ASP.NET 4 adiciona um ponto de extensibilidade ao cache de saída que permite configurar um ou mais provedores de cache de saída personalizados. Provedores de cache de saída podem usar qualquer mecanismo de armazenamento para manter o conteúdo HTML. Isso possibilita criar provedores de cache de saída personalizados para diferentes mecanismos de persistência, que podem incluir discos locais ou remotos, armazenamento em nuvem e mecanismos de cache distribuído.

Você cria um provedor de cache de saída personalizado como uma classe derivada do novo tipo *System. Web. Caching. OutputCacheProvider* . Em seguida, você pode configurar o provedor `Web.config` no arquivo usando a subseção novos *provedores* do elemento *OutputCache* , conforme mostrado no exemplo a seguir:

[!code-xml[Main](overview/samples/sample2.xml)]

Por padrão, no ASP.NET 4, todas as respostas HTTP, páginas renderizadas e controles usam o cache de saída na memória, conforme mostrado no exemplo anterior, em que o atributo defaultfornecetor está definido como AspNetInternalProvider. Você pode alterar o provedor de cache de saída padrão usado para um aplicativo Web especificando um nome de provedor diferentepara defaultProvider.

Além disso, você pode selecionar diferentes provedores de cache de saída por controle e por solicitação. A maneira mais fácil de escolher um provedor de cache de saída diferente para diferentes controles de usuário da Web é fazer isso de forma declarativa usando o novo atributo *ProviderName* em uma diretiva de controle, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample3.aspx)]

A especificação de um provedor de cache de saída diferente para uma solicitação HTTP requer um pouco mais de trabalho. Em vez de especificar declarativamente o provedor, você substitui o novo método *GetOuputCacheProviderName* no `Global.asax` arquivo para especificar programaticamente qual provedor usar para uma solicitação específica. O exemplo a seguir mostra como fazer isso.

[!code-csharp[Main](overview/samples/sample4.cs)]

Com a adição da extensibilidade do provedor de cache de saída ao ASP.NET 4, agora você pode buscar estratégias de cache de saída mais agressivas e inteligentes para seus sites. Por exemplo, agora é possível armazenar em cache as páginas "10 principais" de um site na memória, ao mesmo tempo em que armazena em cache as páginas que obtêm o tráfego inferior em disco. Como alternativa, você pode armazenar em cache todas as combinações variadas para uma página renderizada, mas usar um cache distribuído para que o consumo de memória seja descarregado dos servidores Web front-end.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Aplicativos Web de início automático

Alguns aplicativos Web precisam carregar grandes quantidades de dados ou executar um processamento de inicialização caro antes de atender à primeira solicitação. Em versões anteriores do ASP.net, para essas situações, era necessário planejar abordagens personalizadas para "acordar" um aplicativo ASP.net e, em seguida, executar o código de inicialização durante o método de `Global.asax` *carregamento do aplicativo\_* no arquivo.

Um novo recurso de escalabilidade denominado *início automático* que endereça diretamente esse cenário está disponível quando o ASP.NET 4 é executado no IIS 7,5 no Windows Server 2008 R2. O recurso de início automático fornece uma abordagem controlada para iniciar um pool de aplicativos, inicializar um aplicativo ASP.NET e, em seguida, aceitar solicitações HTTP.

> [!NOTE] 
> 
> Módulo de aquecimento do aplicativo IIS para o IIS 7,5
> 
> A equipe do IIS lançou a primeira versão de teste beta do módulo de aquecimento do aplicativo para o IIS 7,5. Isso torna o aquecimento de seus aplicativos ainda mais fácil do que o descrito anteriormente. Em vez de escrever código personalizado, você especifica as URLs de recursos a serem executados antes que o aplicativo Web aceite solicitações da rede. Esse aquecimento ocorre durante a inicialização do serviço IIS (se você configurou o pool de aplicativos do IIS como *AlwaysRunning*) e quando um processo de trabalho do IIS é reciclado. Durante a reciclagem, o antigo processo de trabalho do IIS continua a executar solicitações até que o processo de trabalho recém-criado seja totalmente executado, de forma que os aplicativos não tenham interrupções ou outros problemas devido a caches não-primos. Observe que esse módulo funciona com qualquer versão do ASP.NET, começando com a versão 2,0.
> 
> Para obter mais informações, consulte [aquecimento do aplicativo](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) no site da IIS.net. Para obter instruções que ilustram como usar o recurso de aquecimento, consulte [introdução com o módulo de aquecimento do aplicativo IIS 7,5](https://www.iis.net/learn/manage) no site do IIS.net.

Para usar o recurso de início automático, um administrador do IIS define um pool de aplicativos no IIS 7,5 para ser iniciado automaticamente usando a seguinte configuração no `applicationHost.config` arquivo:

[!code-xml[Main](overview/samples/sample5.xml)]

Como um único pool de aplicativos pode conter vários aplicativos, você especifica que aplicativos individuais serão iniciados automaticamente usando a seguinte configuração no `applicationHost.config` arquivo:

[!code-xml[Main](overview/samples/sample6.xml)]

Quando um servidor IIS 7,5 é iniciado por frio ou quando um pool de aplicativos individual é reciclado, o IIS 7,5 usa as `applicationHost.config` informações no arquivo para determinar quais aplicativos Web precisam ser iniciados automaticamente. Para cada aplicativo marcado para inicialização automática, o IIS 7.5 envia uma solicitação para ASP.NET 4 para iniciar o aplicativo em um estado durante o qual o aplicativo temporariamente não aceita solicitações HTTP. Quando está nesse estado, ASP.NET instancia o tipo definido pelo atributo *serviceAutoStartProvider* (como mostrado no exemplo anterior) e chama seu ponto de entrada público.

Você cria um tipo de início automático gerenciado com o ponto de entrada necessário implementando a interface *IProcessHostPreloadClient* , conforme mostrado no exemplo a seguir:

[!code-csharp[Main](overview/samples/sample7.cs)]

Depois que o código de inicialização é executado no método *Preload* e o método retorna, o aplicativo ASP.net está pronto para processar solicitações.

Com a adição de início automático ao IIS 0,5 e ASP.NET 4, agora você tem uma abordagem bem definida para realizar uma inicialização de aplicativo dispendiosa antes de processar a primeira solicitação HTTP. Por exemplo, você pode usar o novo recurso de início automático para inicializar um aplicativo e, em seguida, sinalizar um balanceador de carga que o aplicativo foi inicializado e pronto para aceitar o tráfego HTTP.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Redirecionando permanentemente uma página

É uma prática comum em aplicativos Web mover páginas e outros conteúdos ao longo do tempo, o que pode levar a uma acumulação de links obsoletos nos mecanismos de pesquisa. No ASP.NET, os desenvolvedores costumavam lidar com solicitações para URLs antigas usando o método *Response. reredirect* para encaminhar uma solicitação para a nova URL. No entanto , o método de redirecionamento emite uma resposta HTTP 302 encontrada (redirecionamento temporário), o que resulta em uma viagem de ida e volta http extra quando os usuários tentam acessar as URLs antigas.

O ASP.NET 4 adiciona um novo método auxiliar *RedirectPermanent* que torna mais fácil emitir respostas HTTP 301 movidas permanentemente, como no exemplo a seguir:

[!code-csharp[Main](overview/samples/sample8.cs)]

Os mecanismos de pesquisa e outros agentes de usuário que reconhecem redirecionamentos permanentes armazenarão a nova URL associada ao conteúdo, o que elimina a viagem de ida e volta desnecessária feita pelo navegador para redirecionamentos temporários.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Reduzindo o estado da sessão

O ASP.NET fornece duas opções padrão para armazenar o estado de sessão em um Web farm: um provedor de estado de sessão que invoca um servidor de estado de sessão fora do processo e um provedor de estado de sessão que armazena dados em um banco de Microsoft SQL Server. Como as duas opções envolvem o armazenamento de informações de estado fora do processo de trabalho de um aplicativo Web, o estado da sessão deve ser serializado antes de ser enviado para o armazenamento remoto. Dependendo da quantidade de informações que um desenvolvedor salva no estado da sessão, o tamanho dos dados serializados pode crescer muito grande.

O ASP.NET 4 introduz uma nova opção de compactação para os dois tipos de provedores de estado de sessão fora do processo. Quando a opção de configuração *CompressionEnabled* mostrada no exemplo a seguir for definida como *true*, o ASP.net compactará (e descompactar) o estado de sessão serializado usando a classe .NET Framework *System. IO. Compression. GZipStream* .

[!code-xml[Main](overview/samples/sample9.xml)]

Com a adição simples do novo atributo ao `Web.config` arquivo, os aplicativos com ciclos de CPU sobressalentes em servidores Web podem obter reduções substanciais no tamanho dos dados serializados de estado da sessão.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Expandindo o intervalo de URLs permitidas

O ASP.NET 4 introduz novas opções para expandir o tamanho das URLs de aplicativo. As versões anteriores do caminho da URL restrita de ASP.NET para 260 caracteres, com base no limite de caminho de arquivo NTFS. No ASP.NET 4, você tem a opção de aumentar (ou diminuir) esse limite conforme apropriado para seus aplicativos, usando dois novos atributos de configuração de *httpRuntime* . O exemplo a seguir mostra esses novos atributos.

[!code-xml[Main](overview/samples/sample10.xml)]

Para permitir caminhos mais longos ou menores (a parte da URL que não inclui o protocolo, o nome do servidor e a cadeia de caracteres de consulta), modifique o atributo *[maxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* . Para permitir cadeias de caracteres de consulta mais longas ou mais curtas, modifique o valor do atributo *[MaxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* .

O ASP.NET 4 também permite que você configure os caracteres que são usados pela verificação de caractere de URL. Quando o ASP.NET encontra um caractere inválido na parte do caminho de uma URL, ele rejeita a solicitação e emite um erro HTTP 400. Nas versões anteriores do ASP.NET, as verificações de caracteres de URL eram limitadas a um conjunto fixo de caracteres. No ASP.NET 4, você pode personalizar o conjunto de caracteres válidos usando o novo atributo *RequestPathInvalidCharacters* do elemento de configuração *httpRuntime* , conforme mostrado no exemplo a seguir:

[!code-xml[Main](overview/samples/sample11.xml)]

Por padrão, o atributo *RequestPathInvalidCharacters* define oito caracteres como inválidos. (Na cadeia de caracteres atribuída a *RequestPathInvalidCharacters* por padrão, os caracteres menor que (&lt;), maior que (&gt;) e e comercial (&amp;) são codificados, pois o `Web.config` arquivo é um arquivo XML.) Você pode personalizar o conjunto de caracteres inválidos conforme necessário.

> [!NOTE]
> Observação ASP.NET 4 sempre rejeita caminhos de URL que contêm caracteres no intervalo ASCII de 0x00 a 0x1F, pois eles são caracteres de URL inválidos, conforme definido no RFC 2396 da[http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)IETF (). Em versões do Windows Server que executam o IIS 6 ou superior, o driver de dispositivo do protocolo http. sys rejeita automaticamente as URLs com esses caracteres.

<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Validação de solicitação extensível

A validação de solicitação ASP.NET pesquisa dados de solicitação HTTP de entrada para cadeias de caracteres que são comumente usadas em ataques XSS (script entre sites). Se forem encontradas possíveis cadeias de erro de XSS, a validação da solicitação sinalizará a cadeia de caracteres suspeita e retornará um erro. A validação de solicitação interna retorna um erro somente quando encontra as cadeias de caracteres mais comuns usadas em ataques XSS. As tentativas anteriores de tornar a validação de XSS mais agressiva resultaram em muitos falsos positivos. No entanto, os clientes podem querer uma validação de solicitação mais agressiva ou, por outro lado, talvez queiram relaxar intencionalmente as verificações de XSS para páginas específicas ou para tipos específicos de solicitações.

No ASP.NET 4, o recurso de validação de solicitação foi tornado extensível para que você possa usar a lógica de validação de solicitação personalizada. Para estender a validação de solicitação, você cria uma classe que deriva do novo tipo *System. Web. util. RequestValidator* e configura o aplicativo (na `Web.config` seção *httpRuntime* do arquivo) para usar o tipo personalizado. O exemplo a seguir mostra como configurar uma classe de validação de solicitação personalizada:

[!code-xml[Main](overview/samples/sample12.xml)]

O novo atributo *RequestValidationType* requer uma cadeia de caracteres de identificador de tipo de .NET Framework padrão que especifica a classe que fornece validação de solicitação personalizada. Para cada solicitação, ASP.NET invoca o tipo personalizado para processar cada parte dos dados de solicitação HTTP de entrada. A URL de entrada, todos os cabeçalhos HTTP (cookies e cabeçalhos personalizados) e o corpo da entidade estão todos disponíveis para inspeção por uma classe de validação de solicitação personalizada como a mostrada no exemplo a seguir:

[!code-csharp[Main](overview/samples/sample13.cs)]

Para casos em que você não deseja inspecionar uma parte dos dados HTTP de entrada, a classe de validação de solicitação pode retornar para permitir que a validação de solicitação padrão ASP.NET seja executada simplesmente chamando *base. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Cache de objetos e extensibilidade de objetos de cache

Desde sua primeira versão, o ASP.NET incluiu um cache de objeto na memória poderoso (*System. Web. Caching. cache*). A implementação do cache foi tão popular que foi usada em aplicativos que não são da Web. No entanto, é estranho que um aplicativo Windows Forms ou WPF inclua uma referência a `System.Web.dll` apenas para poder usar o cache de objeto ASP.net.

Para disponibilizar o Caching para todos os aplicativos, o .NET Framework 4 introduz um novo assembly, um novo namespace, alguns tipos base e uma implementação de cache concreto. O novo `System.Runtime.Caching.dll` assembly contém uma nova API de cache no namespace *System. Runtime. Caching* . O namespace contém dois conjuntos principais de classes:

- Tipos abstratos que fornecem a base para a criação de qualquer tipo de implementação de cache personalizado.
- Uma implementação concreta do cache de objetos na memória (a classe *System. Runtime. Caching. MemoryCache* ).

A nova classe *MemoryCache* é modelada com mais detalhes no cache ASP.net e compartilha grande parte da lógica do mecanismo de cache interno com o ASP.net. Embora as APIs de cache público em *System. Runtime. Caching* tenham sido atualizadas para dar suporte ao desenvolvimento de caches personalizados, se você tiver usado o objeto de *cache* ASP.net, encontrará conceitos familiares nas novas APIs.

Uma discussão detalhada da nova classe *MemoryCache* e das APIs base de suporte exigiria um documento inteiro. No entanto, o exemplo a seguir lhe dá uma ideia de como funciona a nova API de cache. O exemplo foi escrito para um aplicativo Windows Forms, sem nenhuma dependência `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Codificação de Extensible HTML, URL e cabeçalho HTTP

No ASP.NET 4, você pode criar rotinas de codificação personalizadas para as seguintes tarefas comuns de codificação de texto:

- Codificação HTML.
- Codificação de URL.
- Codificação de atributo HTML.
- Codificando cabeçalhos HTTP de saída.

Você pode criar um codificador personalizado derivando do novo tipo *System. Web. util. HttpEncoder* e, em seguida, configurando ASP.net para usar o tipo personalizado na `Web.config` seção httpRuntime do arquivo, conforme mostrado no exemplo a seguir:

[!code-xml[Main](overview/samples/sample15.xml)]

Depois que um codificador personalizado tiver sido configurado, o ASP.NET chamará automaticamente a implementação de codificação personalizada sempre que os métodos de codificação públicos das classes *System. Web. HttpUtility* ou *System. Web. HttpServerUtility* forem chamados. Isso permite que uma parte de uma equipe de desenvolvimento da Web crie um codificador personalizado que implemente uma codificação agressiva de caracteres, enquanto o restante da equipe de desenvolvimento da Web continua a usar as APIs de codificação ASP.NET públicas. Ao configurar centralmente um codificador personalizado no elemento *httpRuntime* , você garante que todas as chamadas de codificação de texto das APIs de codificação de ASP.net públicas sejam roteadas por meio do codificador personalizado.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Monitoramento de desempenho para aplicativos individuais em um único processo de trabalho

Para aumentar o número de sites da Web que podem ser hospedados em um único servidor, muitos hosters executam vários aplicativos ASP.NET em um único processo de trabalho. No entanto, se vários aplicativos usarem um único processo de trabalho compartilhado, será difícil para os administradores de servidor identificarem um aplicativo individual que está enfrentando problemas.

O ASP.NET 4 aproveita a nova funcionalidade de monitoramento de recursos introduzida pelo CLR. Para habilitar essa funcionalidade, você pode adicionar o seguinte trecho de configuração XML ao `aspnet.config` arquivo de configuração.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Observe que `aspnet.config` o arquivo está no diretório em que o .NET Framework está instalado. Não é o `Web.config` arquivo.

Quando o recurso *appDomainResourceMonitoring* foi habilitado, dois novos contadores de desempenho estão disponíveis na categoria de desempenho "aplicativos ASP.net": *% tempo de processador gerenciado* e *memória gerenciada usada*. Ambos os contadores de desempenho usam o novo recurso de gerenciamento de recursos de domínio do aplicativo CLR para acompanhar o tempo estimado de CPU e a utilização de memória gerenciada de aplicativos ASP.NET individuais. Como resultado, com o ASP.NET 4, os administradores agora têm uma visão mais granular do consumo de recursos de aplicativos individuais em execução em um único processo de trabalho.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Multiplataforma

Você pode criar um aplicativo que se destina a uma versão específica do .NET Framework. No ASP.NET 4, um novo atributo no elemento *Compilation* do `Web.config` arquivo permite que você direcione o .NET Framework 4 e posterior. Se você destinar explicitamente o .NET Framework 4 e se incluir elementos opcionais no `Web.config` arquivo, como as entradas para *System. CodeDom*, esses elementos deverão estar corretos para o .NET Framework 4. (Se você não tiver como alvo explicitamente o .NET Framework 4, a estrutura de destino será inferida da falta de uma entrada no `Web.config` arquivo.)

O exemplo a seguir mostra o uso do atributo *TargetFramework* no `Web.config` elemento *Compilation* do arquivo.

[!code-xml[Main](overview/samples/sample17.xml)]

Observe o seguinte sobre como direcionar uma versão específica do .NET Framework:

- Em um pool de aplicativos .NET Framework 4, o sistema de compilação ASP.net assume o .NET Framework 4 como um destino `Web.config` se o arquivo não incluir o atributo *TargetFramework* ou se `Web.config` o arquivo estiver ausente. (Talvez você precise fazer alterações de codificação em seu aplicativo para que ele seja executado no .NET Framework 4.)
- Se você incluir o atributo *TargetFramework* e se o elemento *System. CodeDom* for `Web.config` definido no arquivo, esse arquivo deverá conter as entradas corretas para o .NET Framework 4.
- Se você estiver usando o comando do *compilador ASPNET\_* para pré-compilar seu aplicativo (como em um ambiente de compilação), deverá usar a versão correta do comando *do\_compilador ASPNET* para a estrutura de destino. Use o compilador fornecido com o .NET Framework 2,0 (%WINDIR%\Microsoft.NET\Framework\v2.0.50727) para compilar para o .NET Framework 3,5 e versões anteriores. Use o compilador fornecido com o .NET Framework 4 para compilar aplicativos criados usando essa estrutura ou usando versões posteriores.
- Em tempo de execução, o compilador usa os assemblies de estrutura mais recentes que estão instalados no computador (e, portanto, no GAC). Se uma atualização for feita posteriormente na estrutura (por exemplo, uma versão hipotética 4,1 está instalada), você poderá usar os recursos na versão mais recente do Framework, embora o atributo *TargetFramework* tenha como alvo uma versão inferior (como 4,0). (No entanto, em tempo de design no Visual Studio 2010 ou quando você usa o comando do *compilador ASPNET\_* , o uso de recursos mais recentes da estrutura causará erros de compilador).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery incluído com Web Forms e MVC

Os modelos do Visual Studio para o Web Forms e o MVC incluem a biblioteca jQuery de software livre. Quando você cria um novo site ou projeto, uma pasta de scripts contendo os três arquivos a seguir é criada:

- jQuery-1.4.1. js – a versão de unminified legível da biblioteca jQuery.
- jQuery-14,1. min. js – a versão reduzidos da biblioteca jQuery.
- jQuery-1.4.1-vsdoc. js – o arquivo de documentação do IntelliSense para a biblioteca jQuery.

Inclua a versão unminified do jQuery ao desenvolver um aplicativo. Inclua a versão reduzidos do jQuery para aplicativos de produção.

Por exemplo, a página Web Forms a seguir ilustra como você pode usar o jQuery para alterar a cor do plano de fundo dos controles de caixa de texto ASP.NET para amarelo quando eles têm foco.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Suporte à rede de distribuição de conteúdo

A CDN (rede de distribuição de conteúdo) do Microsoft Ajax permite que você adicione facilmente scripts ASP.NET AJAX e jQuery aos seus aplicativos Web. Por exemplo, você pode começar a usar a biblioteca jQuery simplesmente adicionando uma `<script>` marca à sua página que aponta para AJAX.Microsoft.com assim:

[!code-html[Main](overview/samples/sample19.html)]

Aproveitando a CDN do Microsoft Ajax, você pode melhorar significativamente o desempenho de seus aplicativos AJAX. O conteúdo da CDN do Microsoft AJAX é armazenado em cache em servidores localizados em todo o mundo. Além disso, a CDN do Microsoft Ajax permite que os navegadores reutilizem arquivos JavaScript em cache para sites que estão localizados em domínios diferentes.

A rede de distribuição de conteúdo do Microsoft Ajax dá suporte a SSL (HTTPS) caso você precise fornecer uma página da Web usando o protocolo SSL.

Implemente um fallback quando a CDN não estiver disponível. Teste o fallback.

Para saber mais sobre a CDN do Microsoft Ajax, visite o seguinte site:

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

O ASP.NET ScriptManager dá suporte à CDN do Microsoft Ajax. Simplesmente definindo uma propriedade, a propriedade EnableCdn, você pode recuperar todos os arquivos JavaScript do ASP.NET Framework da CDN:

[!code-aspx[Main](overview/samples/sample20.aspx)]

Depois de definir a propriedade EnableCdn como o valor true, a estrutura ASP.NET recuperará todos os arquivos JavaScript do ASP.NET Framework da CDN, incluindo todos os arquivos JavaScript usados para validação e o UpdatePanel. Definir essa propriedade pode ter um impacto considerável sobre o desempenho do seu aplicativo Web.

Você pode definir o caminho CDN para seus próprios arquivos JavaScript usando o atributo WebResource. A nova Propriedade CdnPath especifica o caminho para a CDN usada quando você define a propriedade EnableCdn para o valor true:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Scripts explícitos do ScriptManager

No passado, se você usava o ASP.NET ScriptManger, era necessário carregar toda a biblioteca monolítico do ASP.NET AJAX. Aproveitando a nova Propriedade ScriptManager. AjaxFrameworkMode, você pode controlar exatamente quais componentes da biblioteca do ASP.NET AJAX são carregados e carregar apenas os componentes da biblioteca do ASP.NET AJAX de que você precisa.

A Propriedade ScriptManager. AjaxFrameworkMode pode ser definida com os seguintes valores:

- Enabled--especifica que o controle ScriptManager inclui automaticamente o arquivo de script MicrosoftAjax. js, que é um arquivo de script combinado de cada script de estrutura principal (comportamento herdado).
- Disabled--especifica que todos os recursos de script do Microsoft Ajax estão desabilitados e que o controle ScriptManager não faz referência a nenhum script automaticamente.
- Explicit--especifica que você incluirá explicitamente referências de script ao arquivo de script de núcleo do Framework individual que sua página requer e que você incluirá referências às dependências que cada arquivo de script requer.

Por exemplo, se você definir a propriedade AjaxFrameworkMode para o valor Explicit, poderá especificar os scripts de componente do ASP.NET AJAX específicos necessários:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web Forms

Web Forms tem sido um recurso fundamental no ASP.NET desde o lançamento do ASP.NET 1,0. Muitos aprimoramentos estão nessa área para ASP.NET 4, incluindo o seguinte:

- A capacidade de definir marcas *meta* .
- Mais controle sobre o estado de exibição.
- Maneiras mais fáceis de trabalhar com recursos de navegador.
- Suporte para o uso de roteamento ASP.NET com Web Forms.
- Mais controle sobre as IDs geradas.
- A capacidade de manter linhas selecionadas em controles de dados.
- Mais controle sobre HTML renderizado nos controles *FormView* e *ListView* .
- Suporte de filtragem para controles de fonte de dados.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Definindo marcas meta com as propriedades Page. metapalavra-chave e Page. MetaDescription

ASP.NET 4 adiciona duas propriedades à classe de *página* , *metapalavra-chave* e Metadescrição. Essas duas propriedades representam marcas *meta* correspondentes em sua página, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Essas duas propriedades funcionam da mesma forma que a propriedade *title* da página. Eles seguem estas regras:

1. Se não houver marcas *meta* no elemento *Head* que correspondam aos nomes de propriedade (ou seja, nome = "palavras-chave" para *Page.* meta subpalavras e Name = "Description" para *Page. MetaDescription*, o que significa que essas propriedades não foram definidas ), as marcas *meta* serão adicionadas à página quando ela for renderizada.
2. Se já houver marcas *meta* com esses nomes, essas propriedades atuarão como métodos get e Set para o conteúdo das marcas existentes.

Você pode definir essas propriedades em tempo de execução, o que permite obter o conteúdo de um banco de dados ou de outra fonte e que permite definir as marcas dinamicamente para descrever a finalidade de uma página específica.

Você também pode definir as propriedades de *palavras-chave* e *Descrição* na diretiva *@ Page* na parte superior da marcação de página Web Forms, como no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Isso substituirá o conteúdo da marca *meta* (se houver) já declarado na página.

O conteúdo da marca *meta* de descrição é usado para melhorar as visualizações de listagem de pesquisa no Google. (Para obter detalhes, consulte [melhorar trechos de código com uma meta](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) de Metadescrição no blog do Google Webmaster Central.) O Google e o Windows Live Search não usam o conteúdo das palavras-chave para qualquer coisa, mas outros mecanismos de pesquisa podem. Para obter mais informações, consulte [dicas de palavras-chave de meta](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) no site do guia do mecanismo de pesquisa.

Essas novas propriedades são um recurso simples, mas elas o salvam da necessidade de adicioná-las manualmente ou de escrever seu próprio código para criar as marcas *meta* .

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Habilitando o estado de exibição para controles individuais

Por padrão, o estado de exibição é habilitado para a página, com o resultado de que cada controle na página potencialmente armazena o estado de exibição, mesmo que ele não seja necessário para o aplicativo. Os dados de estado de exibição são incluídos na marcação que uma página gera e aumenta a quantidade de tempo que leva para enviar uma página ao cliente e lançá-la de volta. Armazenar mais estado de exibição do que o necessário pode causar uma degradação significativa do desempenho. Em versões anteriores do ASP.NET, os desenvolvedores podiam desabilitar o estado de exibição de controles individuais para reduzir o tamanho da página, mas precisou fazê-lo explicitamente para controles individuais. No ASP.NET 4, os controles de servidor Web incluem uma propriedade ViewStateMode que permite desabilitar o estado de exibição por padrão e, em seguida, habilitá-lo somente para os controles que o exigem na página.

A Propriedade ViewStateMode usa uma enumeração que tem três valores: *Habilitado*, *desabilitado*e *herdado*. *Habilitado* habilita o estado de exibição para esse controle e para todos os controles filho definidos como *Inherit* ou que não têm nada definido. *Disabled desabilita* o estado de exibição e *Inherit* especifica que o controle usa a configuração *ViewStateMode* do controle pai.

O exemplo a seguir mostra como a propriedade ViewStateMode funciona. A marcação e o código para os controles na página a seguir incluem valores para a propriedade ViewStateMode:

[!code-aspx[Main](overview/samples/sample25.aspx)]

Como você pode ver, o código desabilita o estado de exibição para o controle PlaceHolder1. O controle Label1 filho herda esse valor de propriedade (*Inherit* é o valor padrão para *ViewStateMode* para Controls) e, portanto, não salva nenhum estado de exibição. No controle PlaceHolder2, *ViewStateMode* é definido como *Enabled*, portanto, Label2 herda essa propriedade e salva o estado de exibição. Quando a página é carregada pela primeira vez, a propriedade *Text* de ambos os controles *Label* é definida como a cadeia de caracteres "[dynamicvalue]".

O efeito dessas configurações é que, quando a página é carregada pela primeira vez, a seguinte saída é exibida no navegador:

Desabilitado`: [DynamicValue]`

Habilitado`[DynamicValue]`

Depois de um postback, no entanto, a seguinte saída é exibida:

Desabilitado`: [DeclaredValue]`

Habilitado`[DynamicValue]`

O controle Label1 (cujo valor de ViewStateMode está definidocomo desabilitado) não preserva o valor definido como no código. No entanto, o controle Label2 (cujo valor de ViewStateMode está definido como *habilitado*) preserva seu estado.

Você também pode definir *ViewStateMode* na diretiva *@ Page* , como no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample26.aspx)]

A classe de *página* é apenas outro controle; Ele atua como o controle pai para todos os outros controles na página. O valor padrão de *ViewStateMode* é *habilitado* para instâncias da *página*. Como os controles padrãosão herdados, os controles herdarão o valor da propriedade *Enabled* , a menos que você defina *ViewStateMode* na página ou no nível de controle.

O valor da propriedade *ViewStateMode* determina se o estado de exibição será mantido somente se a propriedade *EnableViewState* estiver definida como *true*. Se a propriedade *EnableViewState* for definida como *false*, o estado de exibição não será mantido mesmo se *ViewStateMode* estiver definido como *habilitado*.

Um bom uso para esse recurso é com controles *ContentPlaceHolder* em páginas mestras, onde você pode definir *ViewStateMode* como *desabilitado* para a página mestra e, em seguida, habilitá-lo individualmente para controles *ContentPlaceHolder* que, por sua vez, contêm controles que exigem o estado de exibição.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Alterações nos recursos do navegador

ASP.NET determina os recursos do navegador que um usuário está usando para navegar pelo seu site usando um recurso chamado *recursos do navegador*. Os recursos do navegador são representados pelo objeto *HttpBrowserCapabilities* (exposto pela propriedade *Request. browser* ). Por exemplo, você pode usar o objeto *HttpBrowserCapabilities* para determinar se o tipo e a versão do navegador atual dão suporte a uma versão específica do JavaScript. Ou, você pode usar o objeto *HttpBrowserCapabilities* para determinar se a solicitação foi originada de um dispositivo móvel.

O objeto *HttpBrowserCapabilities* é controlado por um conjunto de arquivos de definição do navegador. Esses arquivos contêm informações sobre os recursos de navegadores específicos. No ASP.NET 4, esses arquivos de definição de navegador foram atualizados para conter informações sobre os navegadores e dispositivos introduzidos recentemente, como o Google Chrome, a pesquisa em smartphones com o Motion BlackBerry e o iPhone da Apple.

A lista a seguir mostra novos arquivos de definição de navegador:

- *blackberry.browser*
- *chrome.browser*
- *Default.browser*
- *firefox.browser*
- *gateway.browser*
- *generic.browser*
- *ie.browser*
- *iemobile.browser*
- *iphone.browser*
- *opera.browser*
- *safari.browser*

#### <a name="using-browser-capabilities-providers"></a>Usando provedores de recursos de navegador

No ASP.NET versão 3,5 Service Pack 1, você pode definir os recursos que um navegador tem das seguintes maneiras:

- No nível do computador, você cria ou atualiza um `.browser` arquivo XML na seguinte pasta:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Depois de definir a capacidade do navegador, execute o seguinte comando no prompt de comando do Visual Studio para recriar o assembly de recursos do navegador e adicioná-lo ao GAC:

- [!code-console[Main](overview/samples/sample28.cmd)]

- Para um aplicativo individual, você cria um `.browser` arquivo na pasta do `App_Browsers` aplicativo.

Essas abordagens exigem que você altere arquivos XML e, para alterações no nível do computador, você deve reiniciar o aplicativo depois de executar o\_processo ASPNET regbrowsers. exe.

O ASP.NET 4 inclui um recurso chamado de *provedores de recursos de navegador*. Como o nome sugere, isso permite que você crie um provedor que, por sua vez, permite que você use seu próprio código para determinar os recursos do navegador.

Na prática, os desenvolvedores geralmente não definem recursos de navegador personalizados. Os arquivos de navegador são difíceis de atualizar, o processo para atualizá-los é razoavelmente complicado, e `.browser` a sintaxe XML para arquivos pode ser complexa para uso e definição. O que tornaria esse processo muito mais fácil é se houvesse uma sintaxe de definição de navegador comum ou um banco de dados que contivesse definições de navegador atualizadas ou até mesmo um serviço Web para tal banco de dados. O novo recurso de provedores de recursos de navegador torna esses cenários possíveis e práticos para desenvolvedores de terceiros.

Há duas abordagens principais para usar o novo recurso de provedor de recursos de navegador do ASP.NET 4: estendendo a funcionalidade de definição de funcionalidades de navegador ASP.NET ou substituindo-a totalmente. As seções a seguir descrevem primeiro como substituir a funcionalidade e como estendê-la.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Substituindo a funcionalidade de recursos do navegador ASP.NET

Para substituir completamente a funcionalidade de definição de funcionalidades de navegador do ASP.NET, siga estas etapas:

1. Crie uma classe de provedor que derive de *HttpCapabilitiesProvider* e que substitua o método *GetBrowserCapabilities* , como no exemplo a seguir: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    O código neste exemplo cria um novo objeto *HttpBrowserCapabilities* , especificando apenas o recurso chamado browser e definindo essa funcionalidade como MyCustomBrowser.
2. Registre o provedor com o aplicativo. 

    Para usar um provedor com um aplicativo, você deve adicionar o atributo do *provedor* à `Web.config` seção browserCaps nos arquivos ou. `Machine.config` (Você também pode definir os atributos do provedor em um elemento *Location* para diretórios específicos no aplicativo, como em uma pasta para um dispositivo móvel específico.) O exemplo a seguir mostra como definir o atributo do *provedor* em um arquivo de configuração:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Outra maneira de registrar a nova definição de funcionalidade do navegador é usar o código, conforme mostrado no exemplo a seguir:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Esse código deve ser executado no evento de *início do aplicativo\_* do `Global.asax` arquivo. Qualquer alteração na classe *BrowserCapabilitiesProvider* deve ocorrer antes que qualquer código no aplicativo seja executado, a fim de garantir que o cache permaneça em um estado válido para o objeto *HttpCapabilitiesBase* resolvido.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>Armazenando em cache o objeto HttpBrowserCapabilities

O exemplo anterior tem um problema, que é que o código seria executado cada vez que o provedor personalizado é invocado para obter o objeto *HttpBrowserCapabilities* . Isso pode ocorrer várias vezes durante cada solicitação. No exemplo, o código para o provedor não faz muito. No entanto, se o código em seu provedor personalizado executar um trabalho significativo para obter o objeto *HttpBrowserCapabilities* , isso poderá afetar o desempenho. Para evitar que isso aconteça, você pode armazenar em cache o objeto *HttpBrowserCapabilities* . Siga estas etapas:

1. Crie uma classe que derive de *HttpCapabilitiesProvider*, como a do exemplo a seguir: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    No exemplo, o código gera uma chave de cache chamando um método BuildCacheKey personalizado e Obtém o tempo de cache chamando um método getcachetime personalizado. Em seguida, o código adiciona o objeto *HttpBrowserCapabilities* resolvido ao cache. O objeto pode ser recuperado do cache e reutilizado em solicitações subsequentes que fazem uso do provedor personalizado.
2. Registre o provedor com o aplicativo, conforme descrito no procedimento anterior.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Estendendo a funcionalidade de recursos do navegador ASP.NET

A seção anterior descreveu como criar um novo objeto *HttpBrowserCapabilities* no ASP.NET 4. Você também pode estender a funcionalidade de recursos do navegador ASP.NET adicionando novas definições de recursos do navegador às que já estão no ASP.NET. Você pode fazer isso sem usar as definições do navegador XML. O procedimento a seguir mostra como.

1. Crie uma classe que derive de *HttpCapabilitiesEvaluator* e que substitua o método *GetBrowserCapabilities* , conforme mostrado no exemplo a seguir: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Esse código usa primeiro a funcionalidade de recursos do navegador ASP.NET para tentar identificar o navegador. No entanto, se nenhum navegador for identificado com base nas informações definidas na solicitação (ou seja, se a propriedade *browser* do objeto *HttpBrowserCapabilities* for a cadeia de caracteres "Unknown"), o código chamará o provedor personalizado ( MyBrowserCapabilitiesEvaluator) para identificar o navegador.
2. Registre o provedor com o aplicativo, conforme descrito no exemplo anterior.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Estendendo a funcionalidade de recursos do navegador adicionando novos recursos às definições de recursos existentes

Além de criar um provedor de definição de navegador personalizado e criar dinamicamente novas definições de navegador, você pode estender definições de navegador existentes com recursos adicionais. Isso permite que você use uma definição que esteja perto do que você deseja, mas que não tem apenas alguns recursos. Para fazer isso, use as etapas a seguir.

1. Crie uma classe que derive de *HttpCapabilitiesEvaluator* e que substitua o método *GetBrowserCapabilities* , conforme mostrado no exemplo a seguir: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    O código de exemplo estende a classe ASP.NET *HttpCapabilitiesEvaluator* existente e Obtém o objeto *HttpBrowserCapabilities* que corresponde à definição de solicitação atual usando o seguinte código:

    [!code-csharp[Main](overview/samples/sample35.cs)]

    O código pode adicionar ou modificar um recurso para este navegador. Há duas maneiras de especificar um novo recurso de navegador:

    - Adicione um par chave/valor ao objeto *IDictionary* que é exposto pela propriedade *Capabilities* do objeto *HttpCapabilitiesBase* . No exemplo anterior, o código adiciona um recurso chamado multitoque com um valor de *true*.
    - Defina as propriedades existentes do objeto *HttpCapabilitiesBase* . No exemplo anterior, o código define a propriedade *frames* como *true*. Essa propriedade é simplesmente um acessador para o objeto *IDictionary* que é exposto pela propriedade *Capabilities* . 

        > [!NOTE]
        > Observe que esse modelo se aplica a qualquer propriedade de *HttpBrowserCapabilities*, incluindo adaptadores de controle.
2. Registre o provedor com o aplicativo, conforme descrito no procedimento anterior.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Roteamento no ASP.NET 4

O ASP.NET 4 adiciona suporte interno ao uso de roteamento com Web Forms. O roteamento permite configurar um aplicativo para aceitar URLs de solicitação que não são mapeadas para arquivos físicos. Em vez disso, você pode usar o roteamento para definir URLs que são significativas para os usuários e que podem ajudar com a SEO (otimização do mecanismo de pesquisa) para seu aplicativo. Por exemplo, a URL de uma página que exibe categorias de produtos em um aplicativo existente pode ser semelhante ao exemplo a seguir:

[!code-console[Main](overview/samples/sample36.cmd)]

Usando o roteamento, você pode configurar o aplicativo para aceitar a seguinte URL para renderizar as mesmas informações:

[!code-console[Main](overview/samples/sample37.cmd)]

O roteamento foi disponibilizado a partir do ASP.NET 3,5 SP1. (Para obter um exemplo de como usar o roteamento no ASP.NET 3,5 SP1, consulte a entrada usando o título de [Roteamento com WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "desta entrada.") no blog de Phil Haack.) No entanto, o ASP.NET 4 inclui alguns recursos que facilitam o uso do roteamento, incluindo o seguinte:

- A classe *PageRouteHandler* , que é um manipulador http simples que você usa ao definir rotas. A classe passa dados para a página para a qual a solicitação é roteada.
- As novas propriedades *HttpRequest. RequestContext* e *Page. RouteData* (que é um proxy para o objeto *HttpRequest. RequestContext. RouteData* ). Essas propriedades facilitam o acesso a informações passadas da rota.
- Os novos construtores de expressão a seguir, que são definidos em *System. Web. Compilation. RouteUrlExpressionBuilder* e *System. Web. Compilation. RouteValueExpressionBuilder*:
- *RouteUrl*, que fornece uma maneira simples de criar uma URL que corresponde a uma URL de rota dentro de um controle de servidor ASP.net.
- *RouteValue*, que fornece uma maneira simples de extrair informações do objeto *RouteContext* .
- A classe *RouteParameter* , que torna mais fácil passar os dados contidos em um objeto *RouteContext* para uma consulta de um controle da fonte de dados (semelhante a [*FormParameter*](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Roteamento para páginas de Web Forms

O exemplo a seguir mostra como definir um Web Forms rota usando o novo método *MapPageRoute* da classe *Route* :

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 introduz o método *MapPageRoute* . O exemplo a seguir é equivalente à definição de SearchRoute mostrada no exemplo anterior, mas usa a classe *PageRouteHandler* .

[!code-csharp[Main](overview/samples/sample39.cs)]

O código no exemplo mapeia a rota para uma página física (na primeira rota, para `~/search.aspx`). A primeira definição de rota também especifica que o parâmetro chamado SearchTerm deve ser extraído da URL e passado para a página.

O método *MapPageRoute* dá suporte às sobrecargas de método a seguir:

- *MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess)*
- *MapPageRoute (cadeia de caracteres RouteName, Cadeia de caracteres routeUrl, String physicalfile, bool checkPhysicalUrlAccess, padrões de RouteValueDictionary)*
- *MapPageRoute (cadeia de caracteres RouteName, Cadeia de caracteres routeUrl, Cadeia de caracteres physicalfile, bool checkPhysicalUrlAccess, padrões de RouteValueDictionary, restrições de RouteValueDictionary)*

O parâmetro *checkPhysicalUrlAccess* especifica se a rota deve verificar as permissões de segurança para a página física que está sendo roteada (neste caso, Search. aspx) e as permissões na URL de entrada (nesse caso, Search/{SearchTerm}). Se o valor de *checkPhysicalUrlAccess* for *false*, somente as permissões da URL de entrada serão verificadas. Essas permissões são definidas no `Web.config` arquivo usando configurações como as seguintes:

[!code-xml[Main](overview/samples/sample40.xml)]

Na configuração de exemplo, o acesso é negado à página `search.aspx` física para todos os usuários, exceto aqueles que estão na função de administrador. Quando o parâmetro *checkPhysicalUrlAccess* é definido como *true* (que é seu valor padrão), somente usuários administradores têm permissão para acessar a URL/Search/{SearchTerm}, pois a página física Search. aspx é restrita aos usuários nessa função. Se *checkPhysicalUrlAccess* for definido como *false* e o site estiver configurado como mostrado no exemplo anterior, todos os usuários autenticados terão permissão para acessar a URL/Search/{SearchTerm}.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Lendo informações de roteamento em uma página de Web Forms

No código da página física Web Forms, você pode acessar as informações que o roteamento extraiu da URL (ou outras informações que outro objeto adicionou ao objeto *RouteData* ) usando duas novas propriedades: *HttpRequest. RequestContext* e *Page. RouteData*. (*Page. RouteData* encapsula *HttpRequest. RequestContext. RouteData*.) O exemplo a seguir mostra como usar *Page. RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

O código extrai o valor que foi passado para o parâmetro SearchTerm, conforme definido na rota de exemplo anterior. Considere a seguinte URL de solicitação:

[!code-console[Main](overview/samples/sample42.cmd)]

Quando essa solicitação é feita, a palavra "Scott" seria renderizada na `search.aspx` página.

#### <a name="accessing-routing-information-in-markup"></a>Acessando informações de roteamento na marcação

O método descrito na seção anterior mostra como obter dados de rota no código em uma página de Web Forms. Você também pode usar expressões em marcação que lhe dão acesso às mesmas informações. Construtores de expressões são uma maneira avançada e elegante de trabalhar com código declarativo. (Para obter mais informações, consulte a entrada [Express você mesmo com construtores de expressão personalizados](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) no blog de Phil Haack.)

O ASP.NET 4 inclui dois novos construtores de expressão para roteamento de Web Forms. O exemplo a seguir mostra como usá-los.

[!code-aspx[Main](overview/samples/sample43.aspx)]

No exemplo, a expressão *RouteUrl* é usada para definir uma URL baseada em um parâmetro de rota. Isso evita que você tenha que embutir em código a URL completa na marcação e permite alterar a estrutura da URL mais tarde, sem a necessidade de qualquer alteração nesse link.

Com base na rota definida anteriormente, essa marcação gera a seguinte URL:

[!code-console[Main](overview/samples/sample44.cmd)]

O ASP.NET automaticamente funciona a rota correta (ou seja, gera a URL correta) com base nos parâmetros de entrada. Você também pode incluir um nome de rota na expressão, que permite especificar uma rota a ser usada.

O exemplo a seguir mostra como usar a expressão RouteValue.

[!code-aspx[Main](overview/samples/sample45.aspx)]

Quando a página que contém esse controle é executada, o valor "Scott" é exibido no rótulo.

A expressão RouteValue torna simples o uso de dados de rota na marcação e evita que você precise trabalhar com a sintaxe de página mais complexa. RouteData ["x"] na marcação.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Usando dados de rota para parâmetros de controle da fonte de dados

A classe *RouteParameter* permite especificar dados de rota como um valor de parâmetro para consultas em um controle de fonte de dados. Ele [funciona de forma muito parecida com a](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) classe, conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample46.aspx)]

Nesse caso, o valor do parâmetro de rota SearchTerm será usado para o @companyname parâmetro na instrução *Select* .

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Definindo IDs de cliente

A nova propriedade ClientIDMode aborda um problema de longa duração em ASP.net, ou seja, como os controles criam o atributo *ID* para os elementos que eles renderizam. Saber o atributo *ID* para elementos renderizados é importante se seu aplicativo incluir script de cliente que referencie esses elementos.

O atributo de *ID* em HTML que é renderizado para controles de servidor Web é gerado com base na propriedade *ClientID* do controle. Até o ASP.NET 4, o algoritmo para gerar o atributo *ID* da propriedade *ClientID* foi concatenar o contêiner de nomenclatura (se houver) com a ID e, no caso de controles repetidos (como nos controles de dados), para adicionar um prefixo e uma sequência automática. Embora isso sempre tenha garantido que as IDs dos controles na página são exclusivas, o algoritmo resultou em IDs de controle que não eram previsíveis e, portanto, eram difíceis de fazer referência no script de cliente.

A nova propriedade ClientIDMode permite especificar mais precisamente como a ID do cliente é gerada para controles. Você pode definir a propriedade ClientIDMode para qualquer controle, incluindo para a página. As configurações possíveis são as seguintes:

- *AutoID* – isso é equivalente ao algoritmo para gerar valores de propriedade *ClientID* que foram usados em versões anteriores do ASP.net.
- *Estático* – isso especifica que o valor *ClientID* será o mesmo que a ID sem concatenar as IDs dos contêineres de nomenclatura pai. Isso pode ser útil em controles de usuário da Web. Como um controle de usuário da Web pode ser localizado em páginas diferentes e em controles de contêiner diferentes, pode ser difícil escrever script de cliente para controles que usam o algoritmo AutoID, pois não é possível prever quais serão os valores de ID.
- *Previsível* – essa opção é usada principalmente em controles de dados que usam modelos repetitivos. Ele concatena as propriedades de ID dos contêineres de nomenclatura do controle, mas os valores *ClientID* gerados não contêm cadeias de caracteres como "ctlxxx". Essa configuração funciona em conjunto com a propriedade *ClientIDRowSuffix* do controle. Você define a propriedade *ClientIDRowSuffix* como o nome de um campo de dados e o valor desse campo é usado como o sufixo para o valor *ClientID* gerado. Normalmente, você usaria a chave primária de um registro de dados como o valor *ClientIDRowSuffix* .
- *Inherit* – essa configuração é o comportamento padrão para controles; Ele especifica que a geração de ID de um controle é igual à do seu pai.

Você pode definir a propriedade ClientIDMode no nível da página. Isso define o valor de ClientIDMode padrão para todos os controles na página atual.

O valor de ClientIDMode padrão no nível de páginaé AutoID, e o valor de *ClientIDMode* padrão no nível de controle é *Inherit*. Como resultado, se você não definir essa propriedade em qualquer lugar no seu código, todos os controles serão padronizados para o algoritmo AutoID.

Você define o valor de nível de página na diretiva *@ Page* , conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample47.aspx)]

Você também pode definir o valor ClientIDMode no arquivo de configuração, seja no nível do computador (máquina) ou no nível do aplicativo. Isso define a configuração padrão ClientIDMode para todos os controles em todas as páginas no aplicativo. Se você definir o valor no nível do computador, ele define a configuração padrão de ClientIDMode para todos os sites nesse computador. O exemplo a seguir mostra a configuração ClientIDMode no arquivo de configuração:

[!code-xml[Main](overview/samples/sample48.xml)]

Conforme observado anteriormente, o valor da propriedade *ClientID* é derivado do contêiner de nomenclatura para o pai de um controle. Em alguns cenários, como quando você está usando páginas mestras, os controles podem terminar com IDs como as do seguinte HTML renderizado:

[!code-html[Main](overview/samples/sample49.html)]

Embora o elemento *Input* mostrado na marcação (de um controle *TextBox* ) seja apenas dois contêineres de nomenclatura em profundidade na página (os controles *ContentPlaceHolder* aninhados), devido à maneira como as páginas mestras são processadas, o resultado final é um ID de controle semelhante ao seguinte:

[!code-console[Main](overview/samples/sample50.cmd)]

Essa ID é garantida como exclusiva na página, mas é desnecessariamente longa para a maioria das finalidades. Imagine que você deseja reduzir o tamanho da ID renderizada e ter mais controle sobre como a ID é gerada. (Por exemplo, você deseja eliminar prefixos "ctlxxx".) A maneira mais fácil de conseguir isso é definindo a propriedade *ClientIDMode* , conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample51.aspx)]

Neste exemplo, a propriedade *ClientIDMode* é definida como *estática* para o elemento *NamingPanel* mais externo e é definida como *previsível* para o elemento *NamingControl* interno. Essas configurações resultam na seguinte marcação (o restante da página e a página mestra são considerados iguais ao exemplo anterior):

[!code-html[Main](overview/samples/sample52.html)]

A configuração *estática* tem o efeito de redefinir a hierarquia de nomenclatura para todos os controles dentro do elemento *NamingPanel* mais externo e de eliminar as IDs *ContentPlaceHolder* e *MasterPage* da ID gerada. (O atributo *Name* dos elementos renderizados não é afetado, portanto a funcionalidade normal de ASP.net é mantida para eventos, estado de exibição e assim por diante.) Um efeito colateral da redefinição da hierarquia de nomenclatura é que, mesmo que você mova a marcação para os elementos *NamingPanel* para um controle *ContentPlaceHolder* diferente, as IDs de cliente renderizadas permanecem as mesmas.

> [!NOTE]
> Observe que cabe a você certificar-se de que as IDs de controle renderizadas sejam exclusivas. Se não forem, ele poderá interromper qualquer funcionalidade que exija IDs exclusivas para elementos HTML individuais, como a função *Document. getElementById* do cliente.

#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Criando IDs de cliente previsíveis em controles vinculados a dados

Os valores *ClientID* que são gerados para controles em um controle de lista associada a dados pelo algoritmo herdado podem ser longos e não são realmente previsíveis. A funcionalidade ClientIDMode pode ajudá-lo a ter mais controle sobre como essas IDs são geradas.

A marcação no exemplo a seguir inclui um controle *ListView* :

[!code-aspx[Main](overview/samples/sample53.aspx)]

No exemplo anterior, as propriedades *ClientIDMode* e *RowClientIDRowSuffix* são definidas na marcação. A propriedade *ClientIDRowSuffix* pode ser usada somente em controles vinculados a dados, e seu comportamento difere dependendo do controle que você está usando. As diferenças são estas:

- Controle *GridView* — você pode especificar o nome de uma ou mais colunas na fonte de dados, que são combinadas em tempo de execução para criar as IDs de cliente. Por exemplo, se você definir *RowClientIDRowSuffix* como "ProductName, ProductID", as IDs de controle para os elementos renderizados terão um formato semelhante ao seguinte:

- [!code-console[Main](overview/samples/sample54.cmd)]

- Controle *ListView* — você pode especificar uma única coluna na fonte de dados que é anexada à ID do cliente. Por exemplo, se você definir *ClientIDRowSuffix* como "ProductName", as IDs de controle renderizadas terão um formato semelhante ao seguinte:

- [!code-console[Main](overview/samples/sample55.cmd)]

- Nesse caso, a trilha 1 é derivada da ID do produto do item de dados atual.

- Controle *Repeater* — esse controle não oferece suporte à propriedade *ClientIDRowSuffix* . Em um controle *Repeater* , o índice da linha atual é usado. Quando você usa ClientIDmode = "previsível" com um controle *Repeater* , as IDs de cliente são geradas com o seguinte formato:

- [!code-console[Main](overview/samples/sample56.cmd)]

- O 0 à direita é o índice da linha atual.

Os controles *FormView* e *DetailsView* não exibem várias linhas, portanto, não dão suporte à propriedade *ClientIDRowSuffix* .

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Persistência da seleção de linha em controles de dados

Os controles *GridView* e *ListView* podem permitir que os usuários selecionem uma linha. Nas versões anteriores do ASP.NET, a seleção tem sido baseada no índice de linha na página. Por exemplo, se você selecionar o terceiro item na página 1 e, em seguida, mover para a página 2, o terceiro item dessa página será selecionado.

Inicialmente, há suporte para a seleção persistente apenas em projetos Dados Dinâmicos no .NET Framework 3,5 SP1. Quando esse recurso é habilitado, o item selecionado atual é baseado na chave de dados do item. Isso significa que, se você selecionar a terceira linha na página 1 e mover para a página 2, nada será selecionado na página 2. Quando você volta para a página 1, a terceira linha ainda é selecionada. A seleção persistida agora tem suporte para os controles *GridView* e *ListView* em todos os projetos usando a propriedade *EnablePersistedSelection* , conforme mostrado no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>Controle de gráfico ASP.NET

O controle de *gráfico* ASP.net expande as ofertas de visualização de dados no .NET Framework. Usando o controle de *gráfico* , você pode criar páginas ASP.NET que têm gráficos intuitivos e visualmente atraentes para análise estatística ou financeira complexa. O controle do *gráfico* ASP.net foi introduzido como um complemento para a versão .NET Framework SP1 do 3,5 e faz parte da versão .NET Framework 4.

O controle inclui os seguintes recursos:

- 35 tipos de gráfico distintos.
- Um número ilimitado de áreas de gráfico, títulos, legendas e anotações.
- Uma ampla variedade de configurações de aparência para todos os elementos do gráfico.
- suporte a 3-D para a maioria dos tipos de gráfico.
- Rótulos de dados inteligentes que podem se ajustar automaticamente aos pontos de dados.
- Faixas, quebras de escala e dimensionamento logarítmica.
- Mais de 50 fórmulas financeiras e estatísticas para análise e transformação de dados.
- Vinculação simples e manipulação de dados do gráfico.
- Suporte para formatos de dados comuns, como datas, horas e moedas.
- Suporte para interatividade e personalização orientada por evento, incluindo eventos de clique de cliente usando AJAX.
- Gerenciamento de estado.
- Streaming binário.

As figuras a seguir mostram exemplos de gráficos financeiros produzidos pelo controle de gráfico ASP.NET.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Figura 2: Exemplos de controle do gráfico ASP.NET

Para obter mais exemplos de como usar o controle de gráfico ASP.NET, baixe o código de exemplo na página de [exemplos do ambiente de modelo do Microsoft Chart](https://go.microsoft.com/fwlink/?LinkId=128300) no site do MSDN. Você pode encontrar mais exemplos de conteúdo da Comunidade no [Fórum de controle do gráfico](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Adicionando o controle de gráfico a uma página do ASP.NET

O exemplo a seguir mostra como adicionar um controle de *gráfico* a uma página ASP.NET usando marcação. No exemplo, o controle de *gráfico* produz um gráfico de colunas para pontos de dados estáticos.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Usando gráficos 3D

O controle *gráfico* contém uma coleção *ChartAreas* , que pode conter objetos *ChartArea* que definem características de áreas de gráfico. Por exemplo, para usar 3-D para uma área de gráfico, use a propriedade *Area3DStyle* como no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample59.aspx)]

A figura a seguir mostra um gráfico 3D com quatro séries do tipo de gráfico de *barras* .

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Figura 3: Gráfico de barras 3-D

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Usando quebras de escala e escalas logarítmicas

As quebras de escala e as escalas logarítmicas são duas maneiras adicionais de adicionar sofisticação ao gráfico. Esses recursos são específicos para cada eixo em uma área do gráfico. Por exemplo, para usar esses recursos no eixo Y primário de uma área do gráfico, use as propriedades *axisal. islogarítmica* e *ScaleBreakStyle* em um objeto *ChartArea* . O trecho a seguir mostra como usar quebras de escala no eixo Y primário.

[!code-aspx[Main](overview/samples/sample60.aspx)]

A figura a seguir mostra o eixo Y com quebras de escala habilitadas.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Figura 4: Quebras de escala

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtrando dados com o controle QueryExtender

Uma tarefa muito comum para os desenvolvedores que criam páginas da Web controladas por dados é filtrar dados. Tradicionalmente, isso foi realizado com a criação de cláusulas *Where* em controles de fonte de dados. Essa abordagem pode ser complicada e, em alguns casos, a sintaxe *Where* não permite que você aproveite a funcionalidade completa do banco de dados subjacente.

Para facilitar a filtragem, um novo controle *QueryExtender* foi adicionado no ASP.NET 4. Esse controle pode ser adicionado aos controles *EntityDataSource* ou *LinqDataSource* para filtrar os dados retornados por esses controles. Como o controle *QueryExtender* depende do LINQ, o filtro é aplicado no servidor de banco de dados antes que os dados sejam enviados para a página, o que resulta em operações muito eficientes.

O controle *QueryExtender* dá suporte a uma variedade de opções de filtro. As seções a seguir descrevem essas opções e fornecem exemplos de como usá-las.

#### <a name="search"></a>Pesquisar

Para a opção de pesquisa, o controle *QueryExtender* executa uma pesquisa em campos especificados. No exemplo a seguir, o controle usa o texto que é inserido no controle TextBoxSearch e procura seu conteúdo nas `ProductName` colunas e `Supplier.CompanyName` nos dados retornados do controle *LinqDataSource* .

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Intervalo

A opção de intervalo é semelhante à opção de pesquisa, mas especifica um par de valores para definir o intervalo. No exemplo a seguir, o controle *QueryExtender* pesquisa a `UnitPrice` coluna nos dados retornados do controle *LinqDataSource* . O intervalo é lido nos controles TextBoxFrom e TextBoxto na página.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

A opção expressão de propriedade permite definir uma comparação com um valor de propriedade. Se a expressão for avaliada como *true*, os dados que estão sendo examinados serão retornados. No exemplo a seguir, o controle *QueryExtender* filtra os dados comparando os dados `Discontinued` na coluna para o valor do controle CheckBoxDiscontinued na página.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>Personalizador

Por fim, você pode especificar uma expressão personalizada a ser usada com o controle *QueryExtender* . Essa opção permite chamar uma função na página que define a lógica do filtro personalizado. O exemplo a seguir mostra como especificar declarativamente uma expressão personalizada no controle *QueryExtender* .

[!code-aspx[Main](overview/samples/sample64.aspx)]

O exemplo a seguir mostra a função personalizada que é invocada pelo controle *QueryExtender* . Nesse caso, em vez de usar uma consulta de banco de dados que inclui uma cláusula *Where* , o código usa uma consulta LINQ para filtrar os dados.

[!code-csharp[Main](overview/samples/sample65.cs)]

Esses exemplos mostram apenas uma expressão que está sendo usada no controle *QueryExtender* de cada vez. No entanto, você pode incluir várias expressões dentro do controle *QueryExtender* .

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Expressões de código codificadas em HTML

Alguns sites ASP.net (especialmente com o ASP.NET MVC) dependem muito do `<%` uso =  `expression %>` da sintaxe (geralmente chamada de "código Nuggets") para gravar texto na resposta. Quando você usa expressões de código, é fácil esquecer de codificar o texto em HTML, se o texto vier da entrada do usuário, ele poderá deixar as páginas abertas para um ataque XSS (script entre sites).

O ASP.NET 4 apresenta a seguinte nova sintaxe para expressões de código:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Essa sintaxe usa a codificação HTML por padrão ao gravar na resposta. Essa nova expressão é efetivamente convertida para o seguinte:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Por exemplo, &lt;%: A solicitação ["userinput"]&gt; % executa a codificação HTML no valor da *solicitação ["userinput"]* .

O objetivo desse recurso é tornar possível substituir todas as instâncias da sintaxe antiga pela nova sintaxe, de modo que você não seja forçado a decidir em cada etapa qual delas usar. No entanto, há casos em que o texto que está sendo impresso é destinado a ser HTML ou já está codificado; nesse caso, isso pode levar a uma codificação dupla.

Nesses casos, o ASP.NET 4 introduz uma nova interface, *IHtmlString*, juntamente com uma implementação concreta, *HtmlString*. As instâncias desses tipos permitem que você indique que o valor de retorno já está codificado corretamente (ou examinado de outra forma) para exibição como HTML e que, portanto, o valor não deve ser codificado em HTML novamente. Por exemplo, o seguinte não deve ser (e não) codificado em HTML:

[!code-aspx[Main](overview/samples/sample68.aspx)]

Os métodos auxiliares do ASP.NET MVC 2 foram atualizados para funcionar com essa nova sintaxe, de forma que elas não sejam codificadas duas vezes, mas somente quando você estiver executando o ASP.NET 4. Essa nova sintaxe não funciona quando você executa um aplicativo usando o ASP.NET 3,5 SP1.

Tenha em mente que isso não garante a proteção contra ataques de XSS. Por exemplo, HTML que usa valores de atributo que não estão entre aspas pode conter entrada do usuário que ainda é suscetível. Observe que a saída dos controles ASP.NET e dos auxiliares do MVC ASP.NET sempre inclui valores de atributo entre aspas, que é a abordagem recomendada.

Da mesma forma, essa sintaxe não executa a codificação JavaScript, como quando você cria uma cadeia de caracteres JavaScript com base na entrada do usuário.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Alterações de modelo de projeto

Em versões anteriores do ASP.net, quando você usa o Visual Studio para criar um novo projeto de site da Web ou projeto de aplicativo Web, os projetos resultantes contêm apenas uma página Default `Web.config` . aspx, um `App_Data` arquivo padrão e a pasta, conforme mostrado no seguinte ilustra

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

O Visual Studio também dá suporte a um tipo de projeto de site vazio, que não contém nenhum arquivo, conforme mostrado na figura a seguir:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

O resultado é que, para o iniciante, há uma pequena orientação sobre como criar um aplicativo Web de produção. Portanto, o ASP.NET 4 introduz três novos modelos, um para um projeto de aplicativo Web vazio e um para um aplicativo Web e um projeto de site.

#### <a name="empty-web-application-template"></a>Modelo de aplicativo Web vazio

Como o nome sugere, o modelo de aplicativo Web vazio é um projeto de aplicativo Web removido. Você seleciona esse modelo de projeto na caixa de diálogo novo projeto do Visual Studio, conforme mostrado na figura a seguir:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Clique para exibir a imagem em tamanho normal](overview/_static/image8.png))

Quando você cria um aplicativo Web ASP.NET vazio, o Visual Studio cria o seguinte layout de pasta:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Isso é semelhante ao layout do site da Web vazio de versões anteriores do ASP.NET, com uma exceção. No Visual Studio 2010, o aplicativo Web vazio e os projetos de site vazio contêm o `Web.config` seguinte arquivo mínimo que contém informações usadas pelo Visual Studio para identificar a estrutura de destino do projeto:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Sem essa propriedade *TargetFramework* , o Visual Studio assume como padrão o direcionamento do .NET Framework 2,0 para preservar a compatibilidade ao abrir aplicativos mais antigos.

#### <a name="web-application-and-web-site-project-templates"></a>Modelos de projeto de aplicativo Web e de site

Os outros dois novos modelos de projeto que são fornecidos com o Visual Studio 2010 contêm alterações importantes. A figura a seguir mostra o layout do projeto que é criado quando você cria um novo projeto de aplicativo Web. (O layout de um projeto de site é praticamente idêntico.)

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

O projeto inclui vários arquivos que não foram criados em versões anteriores. Além disso, o novo projeto de aplicativo Web é configurado com a funcionalidade de associação básica, que permite que você comece rapidamente a proteger o acesso ao novo aplicativo. Devido a essa inclusão, o `Web.config` arquivo para o novo projeto inclui entradas que são usadas para configurar associação, funções e perfis. O exemplo a seguir mostra `Web.config` o arquivo para um novo projeto de aplicativo Web. (Nesse caso, *roleManager* está desabilitado.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Clique para exibir a imagem em tamanho normal](overview/_static/image14.png))

O projeto também contém um segundo `Web.config` arquivo `Account` no diretório. O segundo arquivo de configuração fornece uma maneira de proteger o acesso à página ChangePassword. aspx para usuários não registrados. O exemplo a seguir mostra o conteúdo do segundo `Web.config` arquivo.

![](overview/_static/image15.png)

As páginas criadas por padrão nos novos modelos de projeto também contêm mais conteúdo do que nas versões anteriores. O projeto contém uma página mestra padrão e um arquivo CSS, e a página padrão (default. aspx) é configurada para usar a página mestra por padrão. O resultado é que quando você executa o aplicativo Web ou o site pela primeira vez, a página padrão (Home) já está funcional. Na verdade, é semelhante à página padrão que você vê quando inicia um novo aplicativo MVC.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Clique para exibir a imagem em tamanho normal](overview/_static/image18.png))

A intenção dessas alterações nos modelos de projeto é fornecer orientações sobre como começar a criar um novo aplicativo Web. Com a marcação semanticamente correta, em conformidade com o XHTML 1,0 e com o layout especificado usando o CSS, as páginas nos modelos representam as práticas recomendadas para a criação de aplicativos Web ASP.NET 4. As páginas padrão também têm um layout de duas colunas que você pode personalizar facilmente.

Por exemplo, imagine que, para um novo aplicativo Web, você queira alterar algumas das cores e inserir o logotipo da empresa no lugar do logotipo do meu aplicativo ASP.NET. Para fazer isso, você cria um novo diretório em `Content` para armazenar a imagem do logotipo:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Para adicionar a imagem à página, abra o `Site.Master` arquivo, localize onde o texto do aplicativo meu ASP.net está definido e substitua-o por um elemento *Image* cujo atributo *src* esteja definido como a nova imagem de logotipo, como no exemplo a seguir:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Clique para exibir a imagem em tamanho normal](overview/_static/image22.png))

Em seguida, você pode ir para o arquivo site. css e modificar as definições de classe CSS para alterar a cor do plano de fundo da página, bem como a do cabeçalho.

O resultado dessas alterações é que você pode exibir um home page personalizado com muito pouco esforço:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Clique para exibir a imagem em tamanho normal](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>Aprimoramentos de CSS

Uma das principais áreas de trabalho no ASP.NET 4 tem sido para ajudar a renderizar HTML em conformidade com os padrões de HTML mais recentes. Isso inclui alterações de como os controles de servidor Web ASP.NET usam estilos CSS.

#### <a name="compatibility-setting-for-rendering"></a>Configuração de compatibilidade para renderização

Por padrão, quando um aplicativo ou site da Web tem como alvo o .NET Framework 4, o atributo *controlRenderingCompatibilityVersion* do elemento *pages* é definido como "4,0". Esse elemento é definido no arquivo de nível `Web.config` de máquina e, por padrão, se aplica a todos os aplicativos ASP.NET 4:

[!code-xml[Main](overview/samples/sample69.xml)]

O valor de *controlRenderingCompatibility* é uma cadeia de caracteres, que permite novas definições de versão em potencial em versões futuras. Na versão atual, os seguintes valores têm suporte para essa propriedade:

- "3.5". Essa configuração indica renderização e marcação herdadas. A marcação renderizada por controles é de 100% compatível com versões anteriores e a configuração da propriedade *xhtmlConformance* é respeitada.
- "4.0". Se a propriedade tiver essa configuração, os controles de servidor Web ASP.NET farão o seguinte:
- A propriedade *xhtmlConformance* é sempre tratada como "estrita". Como resultado, os controles renderizam a marcação de XHTML 1,0 estrita.
- A desabilitação de controles que não são de entrada não renderiza mais estilos inválidos.
- os elementos da *div* sobre os campos ocultos agora são estilizados para que não interfiram nas regras de CSS criadas pelo usuário.
- Os controles de menu renderizam a marcação que é semanticamente correta e em conformidade com as diretrizes de acessibilidade.
- Os controles de validação não processam estilos embutidos.
- Controles que anteriormente renderizavam border = "0" (controles que derivam do controle de *tabela* ASP.net e o controle de *imagem* ASP.net) não renderizam mais esse atributo.

#### <a name="disabling-controls"></a>Desabilitando controles

No ASP.NET 3,5 SP1 e versões anteriores, a estrutura renderiza o atributo *Disabled* na marcação HTML para qualquer controle cuja propriedade *habilitada* esteja definida como *false*. No entanto, de acordo com a especificação HTML 4, 1, somente os elementos de *entrada* devem ter esse atributo.

No ASP.NET 4, você pode definir a propriedade *controlRenderingCompatibilityVersion* como "3,5", como no exemplo a seguir:

[!code-xml[Main](overview/samples/sample70.xml)]

Você pode criar marcação para um controle *rótulo* como o seguinte, que desabilita o controle:

[!code-aspx[Main](overview/samples/sample71.aspx)]

O controle *rótulo* renderizaria o seguinte HTML:

[!code-html[Main](overview/samples/sample72.html)]

No ASP.NET 4, você pode definir o *controlRenderingCompatibilityVersion* como "4,0". Nesse caso, somente os controles que renderizam elementos de *entrada* renderizarão um atributo *desabilitado* quando a propriedade *habilitada* do controle for definida como *false*. Controles que não processam elementos de *entrada* HTML, em vez disso, processam um atributo de *classe* que faz referência a uma classe CSS que você pode usar para definir uma aparência desabilitada para o controle. Por exemplo, o controle *rótulo* mostrado no exemplo anterior geraria a seguinte marcação:

[!code-html[Main](overview/samples/sample73.html)]

O valor padrão para a classe especificada para esse controle é "aspNetDisabled". No entanto, você pode alterar esse valor padrão definindo a propriedade estática *DisabledCssClass* static da classe *WebControl* . Para desenvolvedores de controle, o comportamento a ser usado para um controle específico também pode ser definido usando a propriedade *SupportsDisabledAttribute* .

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Ocultando elementos div em campos ocultos

O ASP.NET 2,0 e versões posteriores renderizam campos ocultos específicos do sistema (como o elemento *oculto* usado para armazenar informações de estado de exibição) dentro do elemento *div* a fim de obedecer ao padrão XHTML. No entanto, isso pode causar um problema quando uma regra CSS afeta elementos *div* em uma página. Por exemplo, isso pode resultar na exibição de uma linha de um pixel na página ao torno de elementos de *div* ocultos. No ASP.NET 4, os elementos *div* que contêm os campos ocultos gerados por ASP.NET adicionam uma referência de classe CSS como no exemplo a seguir:

[!code-html[Main](overview/samples/sample74.html)]

Em seguida, você pode definir uma classe CSS que se aplica somente aos elementos ocultos que são gerados por ASP.net, como no exemplo a seguir:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Renderizando uma tabela externa para controles de modelo

Por padrão, os seguintes controles de servidor Web ASP.NET que dão suporte a modelos são automaticamente encapsulados em uma tabela externa que é usada para aplicar estilos embutidos:

- *FormView*
- *Entrar*
- *PasswordRecovery*
- *ChangePassword*
- *Para*
- *CreateUserWizard*

Uma nova propriedade chamada *RenderOuterTable* foi adicionada a esses controles que permite que a tabela externa seja removida da marcação. Por exemplo, considere o seguinte exemplo de um controle *FormView* :

[!code-aspx[Main](overview/samples/sample76.aspx)]

Essa marcação renderiza a saída a seguir para a página, que inclui uma tabela HTML:

[!code-html[Main](overview/samples/sample77.html)]

Para impedir que a tabela seja renderizada, você pode definir a propriedade *RenderOuterTable* do controle *FormView* , como no exemplo a seguir:

[!code-aspx[Main](overview/samples/sample78.aspx)]

O exemplo anterior renderiza a saída a seguir, sem os elementos *Table*, *TR*e *td* :

> Conteúdo

Esse aprimoramento pode facilitar o estilo do conteúdo do controle com CSS, pois nenhuma marca inesperada é renderizada pelo controle.

> [!NOTE]
> Observe que essa alteração desabilita o suporte para a função de formato automático no designer do Visual Studio 2010, porque não há mais um elemento *Table* que possa hospedar atributos de estilo que são gerados pela opção de formato automático.

<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>Aprimoramentos de controle ListView

O controle *ListView* tornou-se mais fácil de usar no ASP.NET 4. A versão anterior do controle exigia que você especifique um modelo de layout que continha um controle de servidor com uma ID conhecida. A marcação a seguir mostra um exemplo típico de como usar o controle *ListView* no ASP.net 3,5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

No ASP.NET 4, o controle *ListView* não requer um modelo de layout. A marcação mostrada no exemplo anterior pode ser substituída pela seguinte marcação:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>Aprimoramentos no controle CheckBoxList e RadioButtonList

No ASP.NET 3,5, você pode especificar o layout para o *CheckBoxList* e o *RadioButtonList* usando as duas configurações a seguir:

- *Flow*. O controle renderiza os elementos de *span* para conter seu conteúdo.
- *Tabela*. O controle renderiza um elemento *Table* para conter seu conteúdo.

O exemplo a seguir mostra a marcação para cada um desses controles.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Por padrão, os controles renderizam HTML semelhante ao seguinte:

[!code-html[Main](overview/samples/sample82.html)]

Como esses controles contêm listas de itens, para renderizar o HTML semanticamente correto, eles devem renderizar seu conteúdo usando elementos de lista de HTML (*li*). Isso facilita para os usuários que lêem páginas da Web usando tecnologia assistencial e facilitam o estilo de controles usando o CSS.

No ASP.NET 4, os controles *CheckBoxList* e *RadioButtonList* dão suporte aos seguintes novos valores para a propriedade *RepeatLayout* :

- *Ordenadalist* – o conteúdo é renderizado como elementos *li* dentro de um elemento *ol* .
- UnorderedList – o conteúdo é renderizado como elementos *li* em um elemento *UL* .

O exemplo a seguir mostra como usar esses novos valores.

[!code-aspx[Main](overview/samples/sample83.aspx)]

A marcação anterior gera o seguinte HTML:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Observação Se você definir *RepeatLayout* como *ordenadalist* ouUnorderedList, a propriedade *RepeatDirection* não poderá mais ser usada e gerará uma exceção em tempo de execução se a propriedade tiver sido definida dentro de sua marcação ou código. A propriedade não teria nenhum valor porque o layout visual desses controles é definido usando CSS em vez disso.

<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Aprimoramentos de controle de menu

Antes de ASP.NET 4, o controle de *menu* renderizou uma série de tabelas HTML. Isso dificultou a aplicação de estilos CSS fora da configuração de propriedades embutidas e também não está em conformidade com os padrões de acessibilidade.

No ASP.NET 4, o controle agora renderiza HTML usando marcação semântica que consiste em uma lista não ordenada e elementos de lista. O exemplo a seguir mostra a marcação em uma página ASP.NET para o controle *menu* .

[!code-aspx[Main](overview/samples/sample85.aspx)]

Quando a página é renderizada, o controle produz o seguinte HTML ( o código OnClick foi omitido para maior clareza):

[!code-html[Main](overview/samples/sample86.html)]

Além dos aprimoramentos de renderização, a navegação por teclado do menu foi aprimorada usando o gerenciamento de foco. Quando o controle de *menu* Obtém foco, você pode usar as teclas de direção para navegar pelos elementos. O controle *menu* agora também anexa funções e atributos de Aria (aplicativos avançados de Internet) acessíveis seg[o](http://www.w3.org/TR/wai-aria-practices/#menu "menu diretrizes do Aria")para melhorar a acessibilidade.

Os estilos para o controle de menu são renderizados em um bloco style na parte superior da página, em vez de alinhar com os elementos HTML renderizados. Se você quiser assumir controle total sobre o estilo do controle, poderá definir a nova propriedade *IncludeStyleBlock* como *false*; nesse caso, o bloco de estilo não é emitido. Uma maneira de usar essa propriedade é usar o recurso de formatação automática no designer do Visual Studio para definir a aparência do menu. Em seguida, você pode executar a página, abrir a fonte da página e, em seguida, copiar o bloco de estilo processado para um arquivo CSS externo. No Visual Studio, desfaça o estilo e defina *IncludeStyleBlock* como *false*. O resultado é que a aparência do menu é definida usando estilos em uma folha de estilos externa.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Assistente e controles CreateUserWizard

O *Assistente* de ASP.net e os controles *CreateUserWizard* dão suporte a modelos que permitem definir o HTML que eles renderizam. (*CreateUserWizard* deriva do *Assistente*.) O exemplo a seguir mostra a marcação para um controle *CreateUserWizard* totalmente modelado:

[!code-aspx[Main](overview/samples/sample87.aspx)]

O controle renderiza HTML semelhante ao seguinte:

[!code-html[Main](overview/samples/sample88.html)]

No ASP.NET 3,5 SP1, embora você possa alterar o conteúdo do modelo, ainda tem controle limitado sobre a saída do controle *Wizard* . No ASP.NET 4, você pode criar um modelo LayoutTemplate e inserir controles de *espaço reservado* (usando nomes reservados) para especificar como deseja que o *controle do assistente* seja renderizado. O exemplo a seguir mostra isso:

[!code-aspx[Main](overview/samples/sample89.aspx)]

O exemplo contém os seguintes espaços reservados nomeados no elemento LayoutTemplate:

- *headerPlaceholder* – em tempo de execução, isso é substituído pelo conteúdo do elemento *HeaderTemplate* .
- *sideBarPlaceholder* – em tempo de execução, isso é substituído pelo conteúdo do elemento *SideBarTemplate* .
- *wizardStepPlaceHolder* – em tempo de execução, isso é substituído pelo conteúdo do elemento *wizardsteptemplate* .
- *navigationPlaceholder* – em tempo de execução, isso é substituído por todos os modelos de navegação que você definiu.

A marcação no exemplo que usa espaços reservados renderiza o HTML a seguir (sem o conteúdo realmente definido nos modelos):

[!code-html[Main](overview/samples/sample90.html)]

O único HTML que agora não é definido pelo usuário é um elemento *span* . (Prevemos isso em versões futuras, até mesmo o elemento *span* não será renderizado.) Isso agora lhe dá controle total sobre praticamente todo o conteúdo que é gerado pelo controle *Wizard* .

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

O ASP.NET MVC foi introduzido como uma estrutura complementar para o ASP.NET 3,5 SP1 em março de 2009. O Visual Studio 2010 inclui o ASP.NET MVC 2, que inclui novos recursos e funcionalidades.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Suporte a áreas

As áreas permitem agrupar controladores e exibições em seções de um aplicativo grande em isolamento relativo de outras seções. Cada área pode ser implementada como um projeto ASP.NET MVC separado que pode ser referenciado pelo aplicativo principal. Isso ajuda a gerenciar a complexidade quando você cria um aplicativo grande e torna mais fácil para várias equipes trabalharem em conjunto em um único aplicativo.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Suporte à validação de atributo de anotação de dados

Os atributos Annotations permitem anexar lógica de validação a um modelo usando atributos de metadados. Os atributos Annotations foram introduzidos no ASP.net dados dinâmicos no ASP.net 3,5 SP1. Esses atributos foram integrados ao associador de modelo padrão e fornecem um meio controlado por metadados para validar a entrada do usuário.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Auxiliares modelados

Os auxiliares com modelo permitem associar automaticamente modelos de edição e exibição com tipos de dados. Por exemplo, você pode usar um auxiliar de modelo para especificar que um elemento de interface do usuário de seletor de data seja automaticamente renderizado para um valor *System. DateTime* . Isso é semelhante aos modelos de campo no ASP.NET Dados Dinâmicos.

Os métodos auxiliares *HTML. EditorFor* e *HTML. DisplayFor* têm suporte interno para a renderização de tipos de dados padrão, bem como objetos complexos com várias propriedades. Eles também personalizam a renderização, permitindo que você aplique atributos de anotação de dados como *DisplayName* e *ScaffoldColumn* ao objeto *ViewModel* .

Geralmente, você deseja personalizar a saída de auxiliares de interface do usuário ainda mais e ter controle total sobre o que é gerado. Os métodos auxiliares *HTML. EditorFor* e *HTML. DisplayFor* dão suporte a isso usando um mecanismo de modelagem que permite que você defina modelos externos que podem substituir e controlar a saída renderizada. Os modelos podem ser renderizados individualmente para uma classe.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dados Dinâmicos

Dados Dinâmicos foi introduzido na versão .NET Framework 3,5 SP1 em meados de 2008. Esse recurso fornece vários aprimoramentos para a criação de aplicativos controlados por dados, incluindo o seguinte:

- Uma experiência de RAD para criar rapidamente um site orientado a dados.
- Validação automática baseada em restrições definidas no modelo de dados.
- A capacidade de alterar facilmente a marcação que é gerada para campos nos controles *GridView* e *DetailsView* usando modelos de campo que fazem parte de seu projeto de dados dinâmicos.

> [!NOTE]
> Observação para obter mais informações, consulte a [documentação dados dinâmicos](https://msdn.microsoft.com/library/cc488545.aspx) na biblioteca MSDN.

Para o ASP.NET 4, a Dados Dinâmicos foi aprimorada para dar aos desenvolvedores ainda mais poder para a criação rápida de sites orientados a dados.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Habilitando Dados Dinâmicos para projetos existentes

Dados Dinâmicos recursos fornecidos no .NET Framework 3,5 SP1 trouxe novos recursos, como os seguintes:

- Modelos de campo – fornecem modelos baseados em tipos de dados para controles vinculados a dados. Os modelos de campo fornecem uma maneira mais simples de personalizar a aparência dos controles de dados do que usar campos de modelo para cada campo.
- Validação – Dados Dinâmicos permite que você use atributos em classes de dados para especificar a validação para cenários comuns, como campos obrigatórios, verificação de intervalo, verificação de tipo, correspondência de padrões usando expressões regulares e validação personalizada. A validação é imposta pelos controles de dados.

No entanto, esses recursos tinham os seguintes requisitos:

- A camada de acesso a dados tinha que ser baseada em Entity Framework ou LINQ to SQL.
- Os únicos controles de fonte de dados com suporte para esses recursos eram os controles *EntityDataSource* ou *LinqDataSource* .
- Os recursos exigiram um projeto Web que foi criado usando o Dados Dinâmicos ou Dados Dinâmicos modelos de entidades para ter todos os arquivos necessários para dar suporte ao recurso.

Um grande objetivo do suporte Dados Dinâmicos no ASP.NET 4 é habilitar a nova funcionalidade de Dados Dinâmicos para qualquer aplicativo ASP.NET. O exemplo a seguir mostra a marcação para controles que podem aproveitar Dados Dinâmicos funcionalidade em uma página existente.

[!code-aspx[Main](overview/samples/sample91.aspx)]

No código da página, o código a seguir deve ser adicionado para habilitar Dados Dinâmicos suporte para esses controles:

[!code-csharp[Main](overview/samples/sample92.cs)]

Quando o controle *GridView* está no modo de edição, dados dinâmicos valida automaticamente que os dados inseridos estão no formato correto. Se não for, será exibida uma mensagem de erro.

Essa funcionalidade também fornece outros benefícios, como a possibilidade de especificar valores padrão para o modo de inserção. Sem Dados Dinâmicos, para implementar um valor padrão para um campo, você deve anexar a um evento, localizar o controle (usando *FindControl*) e definir seu valor. No ASP.NET 4, a chamada *EnableDynamicData* dá suporte a um segundo parâmetro que permite que você passe valores padrão para qualquer campo no objeto, conforme mostrado neste exemplo:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Sintaxe de controle declarativo DynamicDataManager

O controle *DynamicDataManager* foi aprimorado para que você possa configurá-lo de forma declarativa, assim como acontece com a maioria dos controles em ASP.net, em vez de apenas no código. A marcação para o controle *DynamicDataManager* é semelhante ao exemplo a seguir:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Essa marcação habilita o comportamento de Dados Dinâmicos para o controle GridView1 que é referenciado na seção DataControls do controle *DynamicDataManager* .

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Modelos de entidade

Os modelos de entidade oferecem uma nova maneira de personalizar o layout dos dados sem exigir que você crie uma página personalizada. Os modelos de página usam o controle *FormView* (em vez do controle *DetailsView* , conforme usado em modelos de página em versões anteriores do dados dinâmicos) e o controle *DynamicEntity* para renderizar modelos de entidade. Isso lhe dá mais controle sobre a marcação que é renderizada pelo Dados Dinâmicos.

A lista a seguir mostra o novo layout do diretório do projeto que contém modelos de entidade:

[!code-console[Main](overview/samples/sample95.cmd)]

O `EntityTemplate` diretório contém modelos de como exibir objetos de modelo de dados. Por padrão, os objetos são renderizados usando `Default.ascx` o modelo, que fornece marcação parecida com a marcação criada pelo controle *DetailsView* usado pelo dados dinâmicos no ASP.net 3,5 SP1. O exemplo a seguir mostra a marcação para `Default.ascx` o controle:

[!code-aspx[Main](overview/samples/sample96.aspx)]

Os modelos padrão podem ser editados para alterar a aparência do site inteiro. Há modelos para operações de exibição, edição e inserção. Novos modelos podem ser adicionados com base no nome do objeto de dados para alterar a aparência de apenas um tipo de objeto. Por exemplo, você pode adicionar o seguinte modelo:

[!code-console[Main](overview/samples/sample97.cmd)]

O modelo pode conter a seguinte marcação:

[!code-aspx[Main](overview/samples/sample98.aspx)]

Os novos modelos de entidade são exibidos em uma página usando o novo controle *DynamicEntity* . Em tempo de execução, esse controle é substituído pelo conteúdo do modelo de entidade. A marcação a seguir mostra o controle *FormView* no `Detail.aspx` modelo de página que usa o modelo de entidade. Observe o elemento *DynamicEntity* na marcação.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Novos modelos de campo para URLs e endereços de email

O ASP.NET 4 introduz dois novos modelos `EmailAddress.ascx` de campo internos e. `Url.ascx` Esses modelos são usados para campos marcados como *EmailAddress* ou *URL* com o atributo *DataType* . Para objetos *EmailAddress* , o campo é exibido como um hiperlink que é criado usando o protocolo *mailto:* . Quando os usuários clicam no link, ele abre o cliente de email do usuário e cria uma mensagem de esqueleto. Os objetos digitados como *URL* são exibidos como hiperlinks comuns.

O exemplo a seguir mostra como os campos seriam marcados.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Criando links com o controle DynamicHyperLink

Dados Dinâmicos usa o novo recurso de roteamento que foi adicionado no .NET Framework 3,5 SP1 para controlar as URLs que os usuários finais veem quando acessam o site da Web. O novo controle *DynamicHyperLink* torna mais fácil criar links para páginas em um dados dinâmicos site. O exemplo a seguir mostra como usar o controle *DynamicHyperLink* :

[!code-aspx[Main](overview/samples/sample101.aspx)]

Essa marcação cria um link que aponta para a página de lista da `Products` tabela com base nas rotas definidas `Global.asax` no arquivo. O controle usa automaticamente o nome de tabela padrão no qual a página de Dados Dinâmicos se baseia.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Suporte para herança no modelo de dados

O Entity Framework e o LINQ to SQL dão suporte à herança em seus modelos de dados. Um exemplo disso pode ser um banco de dados que tem `InsurancePolicy` uma tabela. Ele também pode conter `CarPolicy` e `HousePolicy` tabelas que `InsurancePolicy` têm os mesmos campos de e, em seguida, adicionam mais campos. Dados Dinâmicos foi modificado para entender objetos herdados no modelo de dados e para dar suporte a scaffolding para as tabelas herdadas.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Suporte para relações muitos para muitos (somente Entity Framework)

O Entity Framework tem suporte avançado para relações muitos para muitos entre tabelas, que é implementada expondo a relação como uma coleção em um objeto de *entidade* . Novos `ManyToMany.ascx` e`ManyToMany_Edit.ascx` modelos de campo foram adicionados para fornecer suporte para exibir e editar dados envolvidos em relações muitos para muitos.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Novos atributos para controlar as enumerações de exibição e suporte

O DisplayAttribute foi adicionado para dar a você controle adicional sobre como os campos são exibidos. O atributo *DisplayName* em versões anteriores do dados dinâmicos permitia que você alterasse o nome usado como uma legenda para um campo. A nova classe DisplayAttribute permite que você especifique mais opções para exibir um campo, como a ordem na qual um campo é exibido e se um campo será usado como um filtro. O atributo também fornece controle independente do nome usado para os rótulos em um controle *GridView* , o nome usado em um controle *DetailsView* , o texto de ajuda para o campo e a marca-d ' água usada para o campo (se o campo aceitar entrada de texto).

A classe *EnumDataTypeAttribute* foi adicionada para permitir que você mapeie campos para enumerações. Ao aplicar esse atributo a um campo, você especifica um tipo de enumeração. Dados dinâmicos usa o novo `Enumeration.ascx` modelo de campo para criar a interface do usuário para exibir e editar valores de enumeração. O modelo mapeia os valores do banco de dados para os nomes na enumeração.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Suporte aprimorado para filtros

Dados Dinâmicos 1,0 fornecido com filtros internos para colunas booleanas e colunas de chave estrangeira. Os filtros não permitiram que você especifique se eles foram exibidos ou em qual ordem eles foram exibidos. O novo atributo DisplayAttribute aborda esses dois problemas fornecendo a você o controle sobre se uma coluna é exibida como um filtro e em qual ordem ela será exibida.

Um aprimoramento adicional é que o suporte à filtragem foi[reescrito para usar o novo]recurso(#0.2__QueryExtender "_QueryExtender") da Web Forms. Isso permite que você crie filtros sem a necessidade de conhecimento do controle da fonte de dados com o qual os filtros serão usados. Juntamente com essas extensões, os filtros também foram transformados em controles de modelo, o que permite que você adicione novos. Por fim, a classe DisplayAttribute mencionada anteriormente permite que o filtro padrão seja substituído, da mesma forma que *UIHint* permite que o modelo de campo padrão para uma coluna seja substituída.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Aprimoramentos no desenvolvimento para a Web do Visual Studio 2010

O desenvolvimento para a Web no Visual Studio 2010 foi aprimorado para maior compatibilidade com CSS, maior produtividade por meio de trechos de marcação HTML e ASP.NET e novos JavaScript do IntelliSense dinâmico.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Compatibilidade com CSS aprimorada

O designer do Visual Web Developer no Visual Studio 2010 foi atualizado para melhorar a conformidade com os padrões CSS 2,1. O designer preserva melhor a integridade da origem HTML e é mais robusto do que nas versões anteriores do Visual Studio. Nos bastidores, também foram feitas melhorias na arquitetura que permitirão aprimoramentos futuros na renderização, no layout e na manutenção.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>Trechos de código HTML e JavaScript

No editor de HTML, o IntelliSense conclui automaticamente nomes de marca. O recurso de trechos de código do IntelliSense conclui automaticamente as marcas inteiras e muito mais. No Visual Studio 2010, os trechos do IntelliSense têm suporte para JavaScript C# , juntamente com e Visual Basic, que eram compatíveis com versões anteriores do Visual Studio.

O Visual Studio 2010 inclui mais de 200 trechos de código que ajudam você a completar automaticamente marcas ASP.NET e HTML comuns, incluindo atributos necessários (como runat = "Server") e atributos comuns específicos de uma marca (como *ID*, *DataSourceID*,  *ControlToValidate*e *Text*).

Você pode baixar trechos de código adicionais ou pode escrever seus próprios trechos de código que encapsulam os blocos de marcação que você ou sua equipe usam para tarefas comuns.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>Aprimoramentos de IntelliSense do JavaScript

No Visual 2010, o JavaScript IntelliSense foi reprojetado para fornecer uma experiência de edição ainda mais rica. O IntelliSense agora reconhece objetos que foram gerados dinamicamente por métodos como *registerNamespace* e por técnicas semelhantes usadas por outras estruturas do JavaScript. O desempenho foi aprimorado para analisar grandes bibliotecas de script e para exibir o IntelliSense com pouco ou nenhum atraso de processamento. A compatibilidade foi drasticamente aumentada para dar suporte a quase todas as bibliotecas de terceiros e para dar suporte a diversos estilos de codificação. Os comentários da documentação agora são analisados conforme você digita e são aproveitados imediatamente pelo IntelliSense.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Implantação de aplicativo Web com o Visual Studio 2010

Quando os desenvolvedores de ASP.NET implantam um aplicativo Web, eles geralmente consideram que eles encontram problemas como os seguintes:

- A implantação em um site de hospedagem compartilhado requer tecnologias como FTP, o que pode ser lento. Além disso, você deve executar tarefas manualmente, como a execução de scripts SQL para configurar um banco de dados e deve alterar as configurações do IIS, como a configuração de uma pasta de diretório virtual como um aplicativo.
- Em um ambiente corporativo, além de implantar os arquivos do aplicativo Web, os administradores frequentemente devem modificar os arquivos de configuração do ASP.NET e as configurações do IIS. Os administradores de banco de dados devem executar uma série de scripts SQL para colocar o banco de dados de aplicativo em execução. Essas instalações são intensas, muitas vezes levam horas para serem concluídas e devem ser documentadas com cuidado.

O Visual Studio 2010 inclui tecnologias que resolvem esses problemas e permitem que você implante aplicativos Web diretamente. Uma dessas tecnologias é a ferramenta de implantação da Web do IIS (MsDeploy. exe).

Os recursos de implantação na Web do Visual Studio 2010 incluem as seguintes áreas principais:

- Empacotamento da Web
- Transformação de Web. config
- Implantação de banco de dados
- Publicação com um clique para aplicativos Web

As seções a seguir fornecem detalhes sobre esses recursos.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Empacotamento da Web

O Visual Studio 2010 usa a ferramenta MSDeploy para criar um arquivo compactado (. zip) para seu aplicativo, que é conhecido como um *pacote da Web*. O arquivo de pacote contém metadados sobre seu aplicativo, além do seguinte conteúdo:

- Configurações do IIS, que incluem configurações de pool de aplicativos, configurações de página de erro e assim por diante.
- O conteúdo da Web real, que inclui páginas da Web, controles de usuário, conteúdo estático (imagens e arquivos HTML) e assim por diante.
- SQL Server dados e esquemas de banco de dado.
- Certificados de segurança, componentes para instalar no GAC, configurações do registro e assim por diante.

Um pacote da Web pode ser copiado para qualquer servidor e instalado manualmente usando o Gerenciador do IIS. Como alternativa, para a implantação automatizada, o pacote pode ser instalado usando comandos de linha de comando ou usando APIs de implantação.

O Visual Studio 2010 fornece tarefas e destinos do MSBuild criados para criar pacotes da Web. Para obter mais informações, consulte [visão geral da implantação do projeto de aplicativo Web do ASP.net](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) no site do MSDN e [10 + 20 motivos pelos quais você deve criar um pacote da Web](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) no blog do Vishal Joshi.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Transformação de Web. config

Para a implantação de aplicativos Web, o Visual Studio 2010 apresenta a [transformação de documento XML (xdt)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), que é um recurso `Web.config` que permite transformar um arquivo de configurações de desenvolvimento para as configurações de produção. As configurações de transformação são especificadas em arquivos `web.debug.config`de `web.release.config`transformação chamados, e assim por diante. (Os nomes desses arquivos correspondem às configurações do MSBuild.) Um arquivo de transformação inclui apenas as alterações que você precisa fazer em um arquivo `Web.config` implantado. Você especifica as alterações usando a sintaxe simples.

O exemplo a seguir mostra uma parte de `web.release.config` um arquivo que pode ser produzido para implantação da sua configuração de versão. A palavra-chave Replace no exemplo especifica que, durante a implantação, o `Web.config` nó ConnectionString no arquivo será substituído pelos valores listados no exemplo.

[!code-xml[Main](overview/samples/sample102.xml)]

Para obter mais informações, consulte [sintaxe de transformação Web. config para implantação de projeto de aplicativo Web](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) no site[do MSDN <a id="0.2_a"></a> e implantação da Web: Transformação](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) de Web. config no blog de Vishal Joshi.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Implantação de banco de dados

Um pacote de implantação do Visual Studio 2010 pode incluir dependências em bancos de dados SQL Server. Como parte da definição do pacote, você fornece a cadeia de conexão para o banco de dados de origem. Quando você cria o pacote da Web, o Visual Studio 2010 cria scripts do SQL para o esquema de banco de dados e, opcionalmente, para o dado e, em seguida, os adiciona ao pacote. Você também pode fornecer scripts SQL personalizados e especificar a sequência na qual eles devem ser executados no servidor. No momento da implantação, você fornece uma cadeia de conexão apropriada para o servidor de destino; em seguida, o processo de implantação usa essa cadeia de conexão para executar os scripts que criam o esquema de banco de dados e os adicionam.

Além disso, usando a publicação com um clique, você pode configurar a implantação para publicar seu banco de dados diretamente quando o aplicativo é publicado em um site de hospedagem compartilhado remoto. Para obter mais informações, confira [Como: Implante um banco de dados com um projeto](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) de aplicativo Web no site do MSDN e na [implantação de banco de dados com o vs 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) no blog de Vishal Joshi.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>Publicação com um clique para aplicativos Web

O Visual Studio 2010 também permite que você use o serviço de gerenciamento remoto do IIS para publicar um aplicativo Web em um servidor remoto. Você pode criar um perfil de publicação para sua conta de hospedagem ou para servidores de teste ou servidores de preparo. Cada perfil pode salvar as credenciais apropriadas com segurança. Em seguida, você pode implantar em qualquer um dos servidores de destino com um clique usando a barra de ferramentas de publicação com um clique da Web. Com o Visual Studio 2010, você também pode publicar usando a linha de comando do MSBuild. Isso permite que você configure seu ambiente do Team Build para incluir a publicação em um modelo de integração contínua.

Para obter mais informações, confira [Como: Implantar um projeto de aplicativo Web usando a publicação de um clique](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) e implantação da Web no site do MSDN e [na Web 1-clique em publicar com vs 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) no blog de Vishal Joshi. Para exibir apresentações em vídeo sobre a implantação de aplicativos Web no Visual Studio 2010, consulte [VS 2010 for Web Developer visualizações](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) no blog de Vishal Joshi.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Recursos

Os sites a seguir fornecem informações adicionais sobre o ASP.NET 4 e o Visual Studio 2010.

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) – a documentação oficial do ASP.NET 4 no site do MSDN.
- [https://www.asp.net/](https://www.asp.net/)— O próprio site da equipe do ASP.NET.
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx)e [ASP.NET dados dinâmicos o mapa de conteúdo](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) — recursos online no site da equipe do ASP.net e na documentação oficial do ASP.net dados dinâmicos.
- [https://www.asp.net/ajax/](../../ajax/index.md)— O principal recurso da Web para o desenvolvimento do ASP.NET AJAX.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/)— O blog da equipe do Visual Web Developer, que inclui informações sobre os recursos do Visual Studio 2010.
- [Webstack do ASP.net](https://github.com/aspnet/AspNetWebStack) — o principal recurso da Web para versões de visualização do ASP.net.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Aviso de isenção de responsabilidade

Este é um documento preliminar e pode ser alterado substancialmente antes da versão comercial final do software descrito aqui.

As informações contidas neste documento representam a visão atual da Microsoft Corporation sobre os problemas discutidos a partir da data da publicação. Como a Microsoft deve responder às condições do mercado em constante mudança, este documento não deve ser interpretado como um compromisso por parte da Microsoft, e a Microsoft não pode garantir a exatidão de nenhuma informação apresentada após a data da publicação.

Este whitepaper é apenas para fins informativos. A MICROSOFT NÃO OFERECE GARANTIA, EXPRESSA, IMPLÍCITA OU ESTATUTÁRIA, DAS INFORMAÇÕES CONTIDAS NESTE DOCUMENTO.

Estar em conformidade com todas as leis de direitos autorais aplicáveis é responsabilidade do usuário. Sem limitar os direitos autorais, nenhuma parte deste documento pode ser reproduzida, armazenada ou inserida em um sistema de recuperação, ou transmitida de qualquer forma, por qualquer meio (eletrônico, mecânico, fotocópia, gravação ou outro) ou para qualquer fim, sem a permissão expressa, por escrito, da Microsoft Corporation.

A Microsoft pode ter patentes, solicitações de patente, marcas comerciais, direitos autorais ou outros direitos de propriedade intelectual que abrangem o conteúdo deste documento. A não ser que especificado expressamente em um contrato de licença da Microsoft, o fornecimento deste documento não confere a você nenhum direito sobre as supracitadas patentes, marcas comerciais, direitos autorais ou outra propriedade intelectual.

Salvo indicação em contrário, os exemplos de empresas, organizações, produtos, nomes de domínio, endereços de email, logotipos, pessoas, lugares e eventos aqui mencionados são fictícios e nenhuma associação com qualquer empresa, organização, produto, nome de domínio, emails reais Endereço, logotipo, pessoa, local ou evento é intencional ou deve ser inferido.

© 2009 Microsoft Corporation. Todos os direitos reservados.

Microsoft e Windows são marcas registradas ou comerciais da Microsoft Corporation nos Estados Unidos e/ou em outros países.

Os nomes de empresas reais e produtos mencionados aqui podem ser marcas comerciais de seus respectivos proprietários.
