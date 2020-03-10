---
uid: signalr/overview/testing-and-debugging/enabling-signalr-tracing
title: Habilitando o rastreamento do Signalr | Microsoft Docs
author: bradygaster
description: Este documento descreve como habilitar e configurar o rastreamento para servidores e clientes do Signalr. O rastreamento permite que você exiba informações de diagnóstico sobre eventos...
ms.author: bradyg
ms.date: 08/08/2014
ms.assetid: 30060acb-be3e-4347-996f-3870f0c37829
msc.legacyurl: /signalr/overview/testing-and-debugging/enabling-signalr-tracing
msc.type: authoredcontent
ms.openlocfilehash: 34fe2cdb10c4b41a6e8cac7fb1741d53c02dfc80
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578830"
---
# <a name="enabling-signalr-tracing"></a>Habilitar o rastreamento do SignalR

por [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este documento descreve como habilitar e configurar o rastreamento para servidores e clientes do Signalr. O rastreamento permite que você exiba informações de diagnóstico sobre eventos em seu aplicativo Signalr.
>
> Este tópico foi escrito originalmente por Patrick Fletcher.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET Framework 4.5
> - Sinalização versão 2
>
>
>
> ## <a name="questions-and-comments"></a>Perguntas e comentários
>
> Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).

Quando o rastreamento está habilitado, um aplicativo Signalr cria entradas de log para eventos. Você pode registrar em log eventos do cliente e do servidor. O rastreamento nos eventos conexão de logs do servidor, provedor de scale out e barramento de mensagem. O rastreamento no cliente registra eventos de conexão. No Signalr 2,1 e posterior, o rastreamento no cliente registra em log o conteúdo completo das mensagens de invocação de Hub.

## <a name="contents"></a>Conteúdo

- [Habilitando o rastreamento no servidor](#server)

    - [Registrando eventos de servidor em arquivos de texto](#server_text)
    - [Registrando eventos de servidor no log de eventos](#server_eventlog)
- [Habilitando o rastreamento no cliente .NET (aplicativos da área de trabalho do Windows)](#net_client)

    - [Registrando eventos do cliente da área de trabalho no console](#desktop_console)
    - [Registrando eventos do cliente da área de trabalho em um arquivo de texto](#desktop_text)
- [Habilitando o rastreamento em clientes Windows Phone 8](#phone)

    - [Registrando Windows Phone eventos de cliente na interface do usuário](#phone_ui)
    - [Registrando Windows Phone eventos de cliente no console de depuração](#phone_debug)
- [Habilitando o rastreamento no cliente JavaScript](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a>Habilitando o rastreamento no servidor

Você habilita o rastreamento no servidor dentro do arquivo de configuração do aplicativo (App. config ou Web. config, dependendo do tipo de projeto). Especifique quais categorias de eventos você deseja registrar. No arquivo de configuração, você também especifica se deseja registrar os eventos em um arquivo de texto, no log de eventos do Windows ou em um log personalizado usando uma implementação do [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).

As categorias de evento do servidor incluem os seguintes tipos de mensagens:

| Fonte | Mensagens |
| --- | --- |
| SignalR.SqlMessageBus | Configuração do provedor de escalabilidade de barramento de mensagem SQL, operação de banco de dados, erro e eventos de tempo limite |
| SignalR.ServiceBusMessageBus | Eventos de criação e assinatura, erro e mensagens do tópico do provedor de expansão do barramento de serviço |
| SignalR.RedisMessageBus | Redis de conexão do provedor de escalabilidade, desconexão e eventos de erro |
| SignalR.ScaleoutMessageBus | Eventos de mensagens de scale out |
| SignalR.Transports.WebSocketTransport | Conexão de transporte WebSocket, desconexão, mensagens e eventos de erro |
| SignalR.Transports.ServerSentEventsTransport | Conexão de transporte ServerSentEvents, desconexão, mensagens e eventos de erro |
| SignalR.Transports.ForeverFrameTransport | Conexão de transporte ForeverFrame, desconexão, mensagens e eventos de erro |
| SignalR.Transports.LongPollingTransport | Conexão de transporte LongPolling, desconexão, mensagens e eventos de erro |
| SignalR.Transports.TransportHeartBeat | Eventos de conexão de transporte, desconexão e KeepAlive |
| SignalR.ReflectedHubDescriptorProvider | Eventos de descoberta de Hub |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a>Registrando eventos de servidor em arquivos de texto

O código a seguir mostra como habilitar o rastreamento para cada categoria de evento. Esta amostra configura o aplicativo para registrar eventos em arquivos de texto.

**Código do servidor XML para habilitar o rastreamento**

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

No código acima, a entrada `SignalRSwitch` especifica a [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) usada para os eventos enviados para o log especificado. Nesse caso, ele é definido como `Verbose`, o que significa que todas as mensagens de depuração e rastreamento são registradas.

A saída a seguir mostra as entradas do arquivo `transports.log.txt` para um aplicativo usando o arquivo de configuração acima. Ele mostra uma nova conexão, uma conexão removida e eventos de pulsação de transporte.

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a>Registrando eventos de servidor no log de eventos

Para registrar eventos no log de eventos, em vez de um arquivo de texto, altere os valores das entradas no nó `sharedListeners`. O código a seguir mostra como registrar eventos de servidor no log de eventos:

**Código do servidor XML para registrar eventos no log de eventos**

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

Os eventos são registrados no log do aplicativo e estão disponíveis por meio do Visualizador de Eventos, conforme mostrado abaixo:

![Visualizador de Eventos mostrando os logs do Signalr](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> Ao usar o log de eventos, defina **TraceLevel** como **erro** para manter o número de Mensagens gerenciáveis.

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a>Habilitando o rastreamento no cliente .NET (aplicativos da área de trabalho do Windows)

O cliente .NET pode registrar eventos no console, em um arquivo de texto ou em um log personalizado usando uma implementação de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).

Para habilitar o registro em log no cliente .NET, defina a propriedade `TraceLevel` da conexão com um valor de [tracelevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) e a propriedade `TraceWriter` como uma instância [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) válida.

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a>Registrando eventos do cliente da área de trabalho no console

O código C# a seguir mostra como registrar em log eventos no cliente .net para o console do:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a>Registrando eventos do cliente da área de trabalho em um arquivo de texto

O código C# a seguir mostra como registrar eventos no cliente .net em um arquivo de texto:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

A saída a seguir mostra as entradas do arquivo `ClientLog.txt` para um aplicativo usando o arquivo de configuração acima. Ele mostra o cliente que está se conectando ao servidor e o Hub que invoca um método de cliente chamado `addMessage`:

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a>Habilitando o rastreamento em clientes Windows Phone 8

Os aplicativos signalr para Windows Phone aplicativos usam o mesmo cliente .NET que os aplicativos da área de trabalho, mas o [console. out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) e a gravação em um arquivo com o [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) não estão disponíveis. Em vez disso, você precisa criar uma implementação personalizada de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) para rastreamento.

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a>Registrando Windows Phone eventos de cliente na interface do usuário

A [base de código do signalr](https://github.com/SignalR/SignalR/archive/master.zip) inclui um exemplo de Windows Phone que grava a saída de rastreamento em um [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) usando uma implementação [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) personalizada chamada `TextBlockWriter`. Essa classe pode ser encontrada no projeto **Samples/Microsoft. AspNet. signaler. Client. WP8. Samples** . Ao criar uma instância do `TextBlockWriter`, passe o [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)atual e um [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) onde ele criará um [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) para usar na saída de rastreamento:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

A saída do rastreamento será então gravada em um novo [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) criado no [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) que você passou:

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a>Registrando Windows Phone eventos de cliente no console de depuração

Para enviar a saída para o console de depuração em vez da interface do usuário, crie uma implementação de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) que grave na janela de depuração e atribua-a à propriedade [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) da sua conexão:

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

As informações de rastreamento serão então gravadas na janela de depuração no Visual Studio:

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a>Habilitando o rastreamento no cliente JavaScript

Para habilitar o log do lado do cliente em uma conexão, defina a propriedade `logging` no objeto de conexão antes de chamar o método `start` para estabelecer a conexão.

**Código JavaScript do cliente para habilitar o rastreamento para o console do navegador (com o proxy gerado)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

**Código JavaScript do cliente para habilitar o rastreamento para o console do navegador (sem o proxy gerado)**

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

Quando o rastreamento está habilitado, o cliente JavaScript registra eventos no console do navegador. Para acessar o console do navegador, consulte [monitoramento de transportes](../getting-started/introduction-to-signalr.md#MonitoringTransports).

A captura de tela a seguir mostra um cliente JavaScript do Signalr com rastreamento habilitado. Ele mostra os eventos de invocação de conexão e de Hub no console do navegador:

![Eventos de rastreamento do signalr no console do navegador](enabling-signalr-tracing/_static/image4.png)
