---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: Guia da API dos hubs 1. x do signalr-cliente JavaScript | Microsoft Docs
author: bradygaster
description: Este documento fornece uma introdução ao uso da API de hubs para a versão 1,1 do Signalr em clientes JavaScript, como navegadores e Windows Store (WinJS)...
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 24850fe5229490bf600e09ad4718abb575a845fa
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536438"
---
# <a name="signalr-1x-hubs-api-guide---javascript-client"></a>Guia da API Hubs do SignalR 1.x – Cliente JavaScript

por [Patrick Fletcher](https://github.com/pfletcher), [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este documento fornece uma introdução ao uso da API de hubs para a versão 1,1 do Signalr em clientes JavaScript, como navegadores e aplicativos de WinJS (Windows Store).
> 
> A API de hubs de Signalr permite que você faça RPCs (chamadas de procedimento remoto) de um servidor para clientes conectados e de clientes para o servidor. No código do servidor, você define métodos que podem ser chamados por clientes e chama métodos que são executados no cliente. No código do cliente, você define métodos que podem ser chamados do servidor e chama métodos que são executados no servidor. O signalr cuida de todo o direcionamento de cliente para servidor para você.
> 
> O signalr também oferece uma API de nível inferior chamada conexões persistentes. Para obter uma introdução ao Signalr, hubs e conexões persistentes, ou para um tutorial que mostra como criar um aplicativo Signalr completo, consulte [signalr-introdução](../getting-started/index.md).

## <a name="overview"></a>Visão geral

Este documento contém as seguintes seções:

- [O proxy gerado e o que ele faz por você](#genproxy)

    - [Quando usar o proxy gerado](#cantusegenproxy)
- [Configuração do cliente](#clientsetup)

    - [Como referenciar o proxy gerado dinamicamente](#dynamicproxy)
    - [Como criar um arquivo físico para o proxy gerado pelo Signalr](#manualproxy)
- [Como estabelecer uma conexão](#establishconnection)

    - [$. Connection. Hub é o mesmo objeto que $. hubConnection () cria](#connequivalence)
    - [Execução assíncrona do método Start](#asyncstart)
- [Como estabelecer uma conexão entre domínios](#crossdomain)
- [Como configurar a conexão](#configureconnection)

    - [Como especificar parâmetros de cadeia de caracteres de consulta](#querystring)
    - [Como especificar o método de transporte](#transport)
- [Como obter um proxy para uma classe de Hub](#getproxy)
- [Como definir métodos no cliente que o servidor pode chamar](#callclient)
- [Como chamar métodos de servidor do cliente](#callserver)
- [Como tratar eventos de tempo de vida da conexão](#connectionlifetime)
- [Como tratar erros](#handleerrors)
- [Como habilitar o log do lado do cliente](#logging)

Para obter a documentação sobre como programar os clientes do servidor ou do .NET, consulte os seguintes recursos:

- [Guia da API de hubs de signalr-servidor](../guide-to-the-api/hubs-api-guide-server.md)
- [Guia da API de hubs do signalr-cliente .NET](../guide-to-the-api/hubs-api-guide-net-client.md)

Links para os tópicos de referência de API são para a versão 4,5 do .NET da API. Se você estiver usando o .NET 4, consulte [a versão .NET 4 dos tópicos da API](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx).

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>O proxy gerado e o que ele faz por você

Você pode programar um cliente JavaScript para se comunicar com um serviço de sinalização com ou sem um proxy que o Signalr gera para você. O que o proxy faz para você é simplificar a sintaxe do código que você usa para se conectar, escrever métodos que o servidor chama e chamar métodos no servidor.

Quando você escreve o código para chamar métodos de servidor, o proxy gerado permite que você use a sintaxe que parece que você estava executando uma função local: você pode gravar `serverMethod(arg1, arg2)` em vez de `invoke('serverMethod', arg1, arg2)`. A sintaxe de proxy gerada também permite um erro imediato e inteligível do lado do cliente se você digitar indigitadamente um nome de método de servidor. E se você criar manualmente o arquivo que define os proxies, também poderá obter suporte IntelliSense para escrever código que chama métodos de servidor.

Por exemplo, suponha que você tenha a seguinte classe de Hub no servidor:

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

Os exemplos de código a seguir mostram a aparência do código JavaScript para invocar o método `NewContosoChatMessage` no servidor e receber invocações do método `addContosoChatMessageToPage` do servidor.

**Com o proxy gerado**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**Sem o proxy gerado**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>Quando usar o proxy gerado

Se você quiser registrar vários manipuladores de eventos para um método de cliente chamado pelo servidor, não poderá usar o proxy gerado. Caso contrário, você pode optar por usar o proxy gerado ou não com base na sua preferência de codificação. Se você optar por não usá-lo, não precisará fazer referência à URL "signalr/hubs" em um elemento `script` no seu código de cliente.

<a id="clientsetup"></a>

## <a name="client-setup"></a>Configuração do cliente

Um cliente JavaScript requer referências ao jQuery e ao arquivo JavaScript do Signalr Core. A versão do jQuery deve ser 1.6.4 ou as principais versões posteriores, como 1.7.2, 1.8.2 ou 1.9.1. Se você decidir usar o proxy gerado, também precisará de uma referência ao arquivo JavaScript de proxy gerado pelo Signalr. O exemplo a seguir mostra como as referências podem parecer em uma página HTML que usa o proxy gerado.

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

Essas referências devem ser incluídas nesta ordem: jQuery First, Core do Signalr depois disso e proxies do Signalr por último.

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>Como referenciar o proxy gerado dinamicamente

No exemplo anterior, a referência ao proxy gerado pelo Signalr é gerada dinamicamente o código JavaScript, não para um arquivo físico. O signalr cria o código JavaScript para o proxy imediatamente e o serve para o cliente em resposta à URL "/signalr/hubs". Se você especificou uma URL base diferente para conexões de Signalr no servidor em seu método de `MapHubs`, a URL para o arquivo de proxy gerado dinamicamente será a URL personalizada com "/hubs" acrescentado a ela.

> [!NOTE]
> Para clientes JavaScript do Windows 8 (Windows Store), use o arquivo de proxy físico em vez de um gerado dinamicamente. Para obter mais informações, consulte [como criar um arquivo físico para o proxy gerado pelo signalr](#manualproxy) mais adiante neste tópico.

Em uma exibição Razor do ASP.NET MVC 4, use o til para se referir à raiz do aplicativo em sua referência de arquivo de proxy:

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

Para obter mais informações sobre como usar o Signalr no MVC 4, consulte [introdução com signalr e MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).

Em uma exibição Razor do ASP.NET MVC 3, use `Url.Content` para a referência do arquivo de proxy:

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

Em um aplicativo de Web Forms ASP.NET, use `ResolveClientUrl` para a referência de arquivo de proxies ou registre-o por meio do ScriptManager usando um caminho relativo de raiz de aplicativo (começando com um til):

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

Como regra geral, use o mesmo método para especificar a URL "/signalr/hubs" que você usa para arquivos CSS ou JavaScript. Se você especificar uma URL sem usar um til, em alguns cenários, seu aplicativo funcionará corretamente quando você testar no Visual Studio usando IIS Express, mas falhará com um erro 404 quando você implantar no IIS completo. Para obter mais informações, consulte **Resolvendo referências a recursos de nível raiz** em [servidores Web no Visual Studio para projetos Web do ASP.net](https://msdn.microsoft.com/library/58wxa9w5.aspx) no site do MSDN.

Ao executar um projeto Web no Visual Studio 2012 no modo de depuração e, se você usar o Internet Explorer como navegador, poderá ver o arquivo de proxy em **Gerenciador de soluções** em **documentos de script**, conforme mostrado na ilustração a seguir.

![Arquivo de proxy gerado por JavaScript no Gerenciador de Soluções](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

Para ver o conteúdo do arquivo, clique duas vezes em **hubs**. Se você não estiver usando o Visual Studio 2012 e o Internet Explorer, ou se não estiver no modo de depuração, também poderá obter o conteúdo do arquivo navegando até a URL "/signalR/hubs". Por exemplo, se seu site estiver em execução em `http://localhost:56699`, vá para `http://localhost:56699/SignalR/hubs` em seu navegador.

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>Como criar um arquivo físico para o proxy gerado pelo Signalr

Como alternativa ao proxy gerado dinamicamente, você pode criar um arquivo físico que tem o código de proxy e fazer referência a esse arquivo. Talvez você queira fazer isso para controlar o comportamento de cache ou agrupamento, ou para obter o IntelliSense quando estiver codificando chamadas para métodos de servidor.

Para criar um arquivo de proxy, execute as seguintes etapas:

1. Instale o pacote NuGet [Microsoft. AspNet. signalr. utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) .
2. Abra um prompt de comando e navegue até a pasta *ferramentas* que contém o arquivo signalr. exe. A pasta ferramentas está no seguinte local:

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. Insira o seguinte comando:

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    O caminho para o *. dll* é normalmente a pasta *bin* na pasta do projeto.

    Esse comando cria um arquivo chamado *Server. js* na mesma pasta que *signalr. exe*.
4. Coloque o arquivo *Server. js* em uma pasta apropriada em seu projeto, renomeie-o conforme apropriado para seu aplicativo e adicione uma referência a ele no lugar da referência de "signalr/hubs".

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>Como estabelecer uma conexão

Antes de poder estabelecer uma conexão, você precisa criar um objeto de conexão, criar um proxy e registrar manipuladores de eventos para métodos que podem ser chamados do servidor. Quando o proxy e os manipuladores de eventos são configurados, estabeleça a conexão chamando o método `start`.

Se você estiver usando o proxy gerado, não precisará criar o objeto de conexão em seu próprio código porque o código de proxy gerado faz isso para você.

<a id="nogenconnection"></a>

**Estabelecer uma conexão (com o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**Estabelecer uma conexão (sem o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

O código de exemplo usa a URL "/signalr" padrão para se conectar ao seu serviço Signalr. Para obter informações sobre como especificar uma URL base diferente, consulte [guia da API de hubs do ASP.net signalr-Server-a URL do/signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

> [!NOTE]
> Normalmente, você registra manipuladores de eventos antes de chamar o método `start` para estabelecer a conexão. Se você quiser registrar alguns manipuladores de eventos depois de estabelecer a conexão, poderá fazer isso, mas deverá registrar pelo menos um dos manipuladores de eventos antes de chamar o método `start`. Um dos motivos para isso é que pode haver muitos hubs em um aplicativo, mas você não desejaria disparar o evento `OnConnected` em cada Hub se usar apenas um deles. Quando a conexão é estabelecida, a presença de um método de cliente no proxy de um hub é o que informa ao Signalr para disparar o evento de `OnConnected`. Se você não registrar nenhum manipulador de eventos antes de chamar o método `start`, poderá invocar métodos no Hub, mas o método `OnConnected` do Hub não será chamado e nenhum método de cliente será invocado do servidor.

<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$. Connection. Hub é o mesmo objeto que $. hubConnection () cria

Como você pode ver nos exemplos, ao usar o proxy gerado, `$.connection.hub` se refere ao objeto de conexão. Esse é o mesmo objeto que você obtém chamando `$.hubConnection()` quando você não está usando o proxy gerado. O código de proxy gerado cria a conexão para você executando a seguinte instrução:

![Criando uma conexão no arquivo de proxy gerado](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

Quando você estiver usando o proxy gerado, poderá fazer algo com `$.connection.hub` que você pode fazer com um objeto de conexão quando não estiver usando o proxy gerado.

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Execução assíncrona do método Start

O método `start` é executado de forma assíncrona. Ele retorna um [objeto jQuery adiado](http://api.jquery.com/category/deferred-object/), o que significa que você pode adicionar funções de retorno de chamada chamando métodos como `pipe`, `done`e `fail`. Se você tiver um código que deseja executar depois que a conexão for estabelecida, como uma chamada para um método de servidor, coloque esse código em uma função de retorno de chamada ou chame-o de uma função de retorno de chamada. O método de retorno de chamada `.done` é executado depois que a conexão é estabelecida e, depois que qualquer código que você tiver em seu método de manipulador de eventos `OnConnected` no servidor, concluir a execução.

Se você colocar a instrução "agora conectado" do exemplo anterior como a próxima linha de código após a chamada do método de `start` (não em um retorno de `.done`), a linha de `console.log` será executada antes que a conexão seja estabelecida, conforme mostrado no exemplo a seguir:

![Maneira incorreta de escrever código que é executado depois que a conexão é estabelecida](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>Como estabelecer uma conexão entre domínios

Normalmente, se o navegador carregar uma página de `http://contoso.com`, a conexão do Signalr estará no mesmo domínio, em `http://contoso.com/signalr`. Se a página de `http://contoso.com` fizer uma conexão com `http://fabrikam.com/signalr`, essa será uma conexão entre domínios. Por motivos de segurança, as conexões entre domínios são desabilitadas por padrão. Para estabelecer uma conexão entre domínios, verifique se as conexões entre domínios estão habilitadas no servidor e especifique a URL de conexão ao criar o objeto de conexão. O signalr usará a tecnologia apropriada para conexões entre domínios, como [JSONP](http://en.wikipedia.org/wiki/JSONP) ou [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing).

No servidor, habilite as conexões entre domínios selecionando essa opção ao chamar o método `MapHubs`.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

No cliente, especifique a URL ao criar o objeto de conexão (sem o proxy gerado) ou antes de chamar o método Start (com o proxy gerado).

**Código do cliente que especifica uma conexão entre domínios (com o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

**Código do cliente que especifica uma conexão entre domínios (sem o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

Ao usar o Construtor `$.hubConnection`, você não precisa incluir `signalr` na URL porque ela é adicionada automaticamente (a menos que você especifique `useDefaultUrl` como `false`).

Você pode criar várias conexões com diferentes pontos de extremidade.

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - Não defina `jQuery.support.cors` como true em seu código.
> 
>     ![Não defina jQuery. support. CORS como true](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     O signalr lida com o uso de JSONP ou CORS. A definição de `jQuery.support.cors` como true desabilita JSONP porque faz com que o Signalr assuma que o navegador dá suporte a CORS.
> - Quando você estiver se conectando a uma URL de localhost, o Internet Explorer 10 não considerará uma conexão entre domínios, de modo que o aplicativo funcionará localmente com o IE 10 mesmo que você não tenha habilitado conexões entre domínios no servidor.
> - Para obter informações sobre como usar conexões entre domínios com o Internet Explorer 9, consulte [este thread StackOverflow](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work).
> - Para obter informações sobre como usar conexões entre domínios com o Chrome, consulte [este thread StackOverflow](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome).
> - O código de exemplo usa a URL "/signalr" padrão para se conectar ao seu serviço Signalr. Para obter informações sobre como especificar uma URL base diferente, consulte [guia da API de hubs do ASP.net signalr-Server-a URL do/signalr](../guide-to-the-api/hubs-api-guide-server.md#signalrurl).

<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>Como configurar a conexão

Antes de estabelecer uma conexão, você pode especificar parâmetros de cadeia de caracteres de consulta ou especificar o método de transporte.

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>Como especificar parâmetros de cadeia de caracteres de consulta

Se você quiser enviar dados ao servidor quando o cliente se conectar, poderá adicionar parâmetros de cadeia de caracteres de consulta ao objeto de conexão. Os exemplos a seguir mostram como definir um parâmetro de cadeia de caracteres de consulta no código do cliente.

**Definir um valor de cadeia de caracteres de consulta antes de chamar o método Start (com o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

**Definir um valor de cadeia de caracteres de consulta antes de chamar o método Start (sem o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

O exemplo a seguir mostra como ler um parâmetro de cadeia de caracteres de consulta no código do servidor.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>Como especificar o método de transporte

Como parte do processo de conexão, um cliente do Signalr normalmente negocia com o servidor para determinar o melhor transporte que é suportado pelo servidor e pelo cliente. Se você já souber qual transporte deseja usar, poderá ignorar esse processo de negociação especificando o método de transporte ao chamar o método de `start`.

**O código do cliente que especifica o método de transporte (com o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**O código do cliente que especifica o método de transporte (sem o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

Como alternativa, você pode especificar vários métodos de transporte na ordem em que deseja que o Signalr os experimente:

**O código do cliente que especifica um esquema de fallback de transporte personalizado (com o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

**O código do cliente que especifica um esquema de fallback de transporte personalizado (sem o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

Você pode usar os seguintes valores para especificar o método de transporte:

- WebSockets
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

Os exemplos a seguir mostram como descobrir qual método de transporte está sendo usado por uma conexão.

**O código do cliente que exibe o método de transporte usado por uma conexão (com o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

**O código do cliente que exibe o método de transporte usado por uma conexão (sem o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

Para obter informações sobre como verificar o método de transporte no código do servidor, consulte [guia da API de hubs do signaler ASP.NET-Server-como obter informações sobre o cliente a partir da propriedade Context](../guide-to-the-api/hubs-api-guide-server.md#contextproperty). Para obter mais informações sobre transportes e fallbacks, consulte [introdução ao signalr-transportes e fallbacks](../getting-started/introduction-to-signalr.md#transports).

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>Como obter um proxy para uma classe de Hub

Cada objeto de conexão que você cria encapsula informações sobre uma conexão com um serviço de sinalização que contém uma ou mais classes de Hub. Para se comunicar com uma classe de Hub, você usa um objeto proxy que você mesmo cria (se você não estiver usando o proxy gerado) ou que é gerado para você.

No cliente, o nome do proxy é uma versão do camel case do nome da classe do Hub. O signalr faz essa alteração automaticamente para que o código JavaScript possa estar em conformidade com as convenções de JavaScript.

**Classe de Hub no servidor**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

**Obter uma referência para o proxy de cliente gerado para o Hub**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

**Criar proxy de cliente para a classe de Hub (sem proxy gerado)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

Se você decorar a classe de Hub com um atributo `HubName`, use o nome exato sem alterar o caso.

**Classe de Hub no servidor com o atributo HubName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

**Obter uma referência para o proxy de cliente gerado para o Hub**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

**Criar proxy de cliente para a classe de Hub (sem proxy gerado)**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>Como definir métodos no cliente que o servidor pode chamar

Para definir um método que o servidor pode chamar de um Hub, adicione um manipulador de eventos ao proxy de Hub usando a propriedade `client` do proxy gerado ou chame o método `on` se você não estiver usando o proxy gerado. Os parâmetros podem ser objetos complexos.

Adicione o manipulador de eventos antes de chamar o método `start` para estabelecer a conexão. (Se você quiser adicionar manipuladores de eventos depois de chamar o método `start`, consulte a observação em [como estabelecer uma conexão](#establishconnection) anteriormente neste documento e use a sintaxe mostrada para definir um método sem usar o proxy gerado.)

A correspondência de nome de método não diferencia maiúsculas de minúsculas. Por exemplo, `Clients.All.addContosoChatMessageToPage` no servidor será executado `AddContosoChatMessageToPage`, `addContosoChatMessageToPage`ou `addcontosochatmessagetopage` no cliente.

**Definir o método no cliente (com o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

**Maneira alternativa de definir o método no cliente (com o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

**Definir o método no cliente (sem o proxy gerado ou ao adicionar depois de chamar o método Start)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

**Código de servidor que chama o método de cliente**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

Os exemplos a seguir incluem um objeto complexo como um parâmetro de método.

**Definir o método no cliente que usa um objeto complexo (com o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

**Definir o método no cliente que usa um objeto complexo (sem o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

**Código de servidor que define o objeto complexo**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

**Código de servidor que chama o método de cliente usando um objeto complexo**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>Como chamar métodos de servidor do cliente

Para chamar um método de servidor do cliente, use a propriedade `server` do proxy gerado ou o método `invoke` no proxy de Hub se você não estiver usando o proxy gerado. O valor ou os parâmetros de retorno podem ser objetos complexos.

Passe uma versão do camel case do nome do método no Hub. O signalr faz essa alteração automaticamente para que o código JavaScript possa estar em conformidade com as convenções de JavaScript.

Os exemplos a seguir mostram como chamar um método de servidor que não tem um valor de retorno e como chamar um método de servidor que tem um valor de retorno.

**Método de servidor sem atributo HubMethodName**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

**Código de servidor que define o objeto complexo passado em um parâmetro**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

**Código de cliente que invoca o método de servidor (com o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

**Código de cliente que invoca o método de servidor (sem o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

Se você tiver decorado o método Hub com um atributo `HubMethodName`, use esse nome sem alterar o caso.

**Método de servidor** com um atributo HubMethodName

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

**Código de cliente que invoca o método de servidor (com o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

**Código de cliente que invoca o método de servidor (sem o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

Os exemplos anteriores mostram como chamar um método de servidor que não tem nenhum valor de retorno. Os exemplos a seguir mostram como chamar um método de servidor que tem um valor de retorno.

**Código do servidor para um método que tem um valor de retorno**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

**A classe Stock usada para o valor de** retorno

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

**Código de cliente que invoca o método de servidor (com o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

**Código de cliente que invoca o método de servidor (sem o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>Como tratar eventos de tempo de vida da conexão

O signalr fornece os seguintes eventos de tempo de vida de conexão que você pode manipular:

- `starting`: gerado antes de qualquer dado ser enviado pela conexão.
- `received`: ocorre quando os dados são recebidos na conexão. Fornece os dados recebidos.
- `connectionSlow`: ocorre quando o cliente detecta uma conexão lenta ou com frequência.
- `reconnecting`: gerado quando o transporte subjacente começa a se reconectar.
- `reconnected`: ocorre quando o transporte subjacente é reconectado.
- `stateChanged`: ocorre quando o estado da conexão é alterado. Fornece o estado antigo e o novo estado (conectando, conectado, reconectando ou desconectado).
- `disconnected`: gerado quando a conexão é desconectada.

Por exemplo, se você quiser exibir mensagens de aviso quando houver problemas de conexão que possam causar atrasos perceptíveis, manipule o evento `connectionSlow`.

**Manipular o evento connectionSlow (com o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

**Manipular o evento connectionSlow (sem o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

Para obter mais informações, consulte [noções básicas e manipulação de eventos de tempo de vida de conexão no signalr](index.md).

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>Como tratar erros

O cliente do sinalizador JavaScript fornece um evento `error` para o qual você pode adicionar um manipulador. Você também pode usar o método Fail para adicionar um manipulador de erros que resultam de uma invocação de método de servidor.

Se você não habilitar explicitamente mensagens de erro detalhadas no servidor, o objeto de exceção que o Signalr retorna após um erro contém informações mínimas sobre o erro. Por exemplo, se uma chamada para `newContosoChatMessage` falhar, a mensagem de erro no objeto de erro conterá "`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`" enviar mensagens de erro detalhadas para clientes em produção não é recomendada por motivos de segurança, mas se você quiser habilitar mensagens de erro detalhadas para fins de solução de problemas, use o código a seguir no servidor.

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

O exemplo a seguir mostra como adicionar um manipulador para o evento de erro.

**Adicionar um manipulador de erro (com o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

**Adicionar um manipulador de erro (sem o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

O exemplo a seguir mostra como tratar um erro de uma invocação de método.

**Manipular um erro de uma invocação de método (com o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

**Manipular um erro de uma invocação de método (sem o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

Se uma invocação de método falhar, o evento `error` também será gerado, de modo que seu código no manipulador de método `error` e no retorno de chamada do método `.fail` seria executado.

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>Como habilitar o log do lado do cliente

Para habilitar o log do lado do cliente em uma conexão, defina a propriedade `logging` no objeto de conexão antes de chamar o método `start` para estabelecer a conexão.

**Habilitar registro em log (com o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

**Habilitar o registro em log (sem o proxy gerado)**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

Para ver os logs, abra as ferramentas de desenvolvedor do navegador e vá para a guia Console. Para obter um tutorial que mostra instruções passo a passo e capturas de tela que mostram como fazer isso, consulte [transmissão de servidor com o ASP.net signalr-habilitar registro em log](index.md).
