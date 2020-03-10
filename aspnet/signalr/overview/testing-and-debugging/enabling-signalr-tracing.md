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
# <a name="enabling-signalr-tracing"></a><span data-ttu-id="fc611-104">Habilitar o rastreamento do SignalR</span><span class="sxs-lookup"><span data-stu-id="fc611-104">Enabling SignalR Tracing</span></span>

<span data-ttu-id="fc611-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="fc611-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="fc611-106">Este documento descreve como habilitar e configurar o rastreamento para servidores e clientes do Signalr.</span><span class="sxs-lookup"><span data-stu-id="fc611-106">This document describes how to enable and configure tracing for SignalR servers and clients.</span></span> <span data-ttu-id="fc611-107">O rastreamento permite que você exiba informações de diagnóstico sobre eventos em seu aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="fc611-107">Tracing enables you to view diagnostic information about events in your SignalR application.</span></span>
>
> <span data-ttu-id="fc611-108">Este tópico foi escrito originalmente por Patrick Fletcher.</span><span class="sxs-lookup"><span data-stu-id="fc611-108">This topic was originally written by Patrick Fletcher.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="fc611-109">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="fc611-109">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="fc611-110">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="fc611-110">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="fc611-111">.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="fc611-111">.NET Framework 4.5</span></span>
> - <span data-ttu-id="fc611-112">Sinalização versão 2</span><span class="sxs-lookup"><span data-stu-id="fc611-112">SignalR version 2</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="fc611-113">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="fc611-113">Questions and comments</span></span>
>
> <span data-ttu-id="fc611-114">Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="fc611-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="fc611-115">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="fc611-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="fc611-116">Quando o rastreamento está habilitado, um aplicativo Signalr cria entradas de log para eventos.</span><span class="sxs-lookup"><span data-stu-id="fc611-116">When tracing is enabled, a SignalR application creates log entries for events.</span></span> <span data-ttu-id="fc611-117">Você pode registrar em log eventos do cliente e do servidor.</span><span class="sxs-lookup"><span data-stu-id="fc611-117">You can log events from both the client and the server.</span></span> <span data-ttu-id="fc611-118">O rastreamento nos eventos conexão de logs do servidor, provedor de scale out e barramento de mensagem.</span><span class="sxs-lookup"><span data-stu-id="fc611-118">Tracing on the server logs connection, scaleout provider, and message bus events.</span></span> <span data-ttu-id="fc611-119">O rastreamento no cliente registra eventos de conexão.</span><span class="sxs-lookup"><span data-stu-id="fc611-119">Tracing on the client logs connection events.</span></span> <span data-ttu-id="fc611-120">No Signalr 2,1 e posterior, o rastreamento no cliente registra em log o conteúdo completo das mensagens de invocação de Hub.</span><span class="sxs-lookup"><span data-stu-id="fc611-120">In SignalR 2.1 and later, tracing on the client logs the full content of hub invocation messages.</span></span>

## <a name="contents"></a><span data-ttu-id="fc611-121">Conteúdo</span><span class="sxs-lookup"><span data-stu-id="fc611-121">Contents</span></span>

- [<span data-ttu-id="fc611-122">Habilitando o rastreamento no servidor</span><span class="sxs-lookup"><span data-stu-id="fc611-122">Enabling tracing on the server</span></span>](#server)

    - [<span data-ttu-id="fc611-123">Registrando eventos de servidor em arquivos de texto</span><span class="sxs-lookup"><span data-stu-id="fc611-123">Logging server events to text files</span></span>](#server_text)
    - [<span data-ttu-id="fc611-124">Registrando eventos de servidor no log de eventos</span><span class="sxs-lookup"><span data-stu-id="fc611-124">Logging server events to the event log</span></span>](#server_eventlog)
- [<span data-ttu-id="fc611-125">Habilitando o rastreamento no cliente .NET (aplicativos da área de trabalho do Windows)</span><span class="sxs-lookup"><span data-stu-id="fc611-125">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>](#net_client)

    - [<span data-ttu-id="fc611-126">Registrando eventos do cliente da área de trabalho no console</span><span class="sxs-lookup"><span data-stu-id="fc611-126">Logging Desktop client events to the console</span></span>](#desktop_console)
    - [<span data-ttu-id="fc611-127">Registrando eventos do cliente da área de trabalho em um arquivo de texto</span><span class="sxs-lookup"><span data-stu-id="fc611-127">Logging Desktop client events to a text file</span></span>](#desktop_text)
- [<span data-ttu-id="fc611-128">Habilitando o rastreamento em clientes Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="fc611-128">Enabling tracing in Windows Phone 8 clients</span></span>](#phone)

    - [<span data-ttu-id="fc611-129">Registrando Windows Phone eventos de cliente na interface do usuário</span><span class="sxs-lookup"><span data-stu-id="fc611-129">Logging Windows Phone client events to the UI</span></span>](#phone_ui)
    - [<span data-ttu-id="fc611-130">Registrando Windows Phone eventos de cliente no console de depuração</span><span class="sxs-lookup"><span data-stu-id="fc611-130">Logging Windows Phone client events to the debug console</span></span>](#phone_debug)
- [<span data-ttu-id="fc611-131">Habilitando o rastreamento no cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="fc611-131">Enabling tracing in the JavaScript client</span></span>](#javascript)

<a id="server"></a>
## <a name="enabling-tracing-on-the-server"></a><span data-ttu-id="fc611-132">Habilitando o rastreamento no servidor</span><span class="sxs-lookup"><span data-stu-id="fc611-132">Enabling tracing on the server</span></span>

<span data-ttu-id="fc611-133">Você habilita o rastreamento no servidor dentro do arquivo de configuração do aplicativo (App. config ou Web. config, dependendo do tipo de projeto). Especifique quais categorias de eventos você deseja registrar.</span><span class="sxs-lookup"><span data-stu-id="fc611-133">You enable tracing on the server within the application's configuration file (either App.config or Web.config depending on the type of project.) You specify which categories of events you want to log.</span></span> <span data-ttu-id="fc611-134">No arquivo de configuração, você também especifica se deseja registrar os eventos em um arquivo de texto, no log de eventos do Windows ou em um log personalizado usando uma implementação do [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span><span class="sxs-lookup"><span data-stu-id="fc611-134">In the configuration file, you also specify whether to log the events to a text file, the Windows event log, or a custom log using an implementation of [TraceListener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener(v=vs.110).aspx).</span></span>

<span data-ttu-id="fc611-135">As categorias de evento do servidor incluem os seguintes tipos de mensagens:</span><span class="sxs-lookup"><span data-stu-id="fc611-135">The server event categories include the following sorts of messages:</span></span>

| <span data-ttu-id="fc611-136">Fonte</span><span class="sxs-lookup"><span data-stu-id="fc611-136">Source</span></span> | <span data-ttu-id="fc611-137">Mensagens</span><span class="sxs-lookup"><span data-stu-id="fc611-137">Messages</span></span> |
| --- | --- |
| <span data-ttu-id="fc611-138">SignalR.SqlMessageBus</span><span class="sxs-lookup"><span data-stu-id="fc611-138">SignalR.SqlMessageBus</span></span> | <span data-ttu-id="fc611-139">Configuração do provedor de escalabilidade de barramento de mensagem SQL, operação de banco de dados, erro e eventos de tempo limite</span><span class="sxs-lookup"><span data-stu-id="fc611-139">SQL Message Bus scaleout provider setup, database operation, error, and timeout events</span></span> |
| <span data-ttu-id="fc611-140">SignalR.ServiceBusMessageBus</span><span class="sxs-lookup"><span data-stu-id="fc611-140">SignalR.ServiceBusMessageBus</span></span> | <span data-ttu-id="fc611-141">Eventos de criação e assinatura, erro e mensagens do tópico do provedor de expansão do barramento de serviço</span><span class="sxs-lookup"><span data-stu-id="fc611-141">Service bus scaleout provider topic creation and subscription, error, and messaging events</span></span> |
| <span data-ttu-id="fc611-142">SignalR.RedisMessageBus</span><span class="sxs-lookup"><span data-stu-id="fc611-142">SignalR.RedisMessageBus</span></span> | <span data-ttu-id="fc611-143">Redis de conexão do provedor de escalabilidade, desconexão e eventos de erro</span><span class="sxs-lookup"><span data-stu-id="fc611-143">Redis scaleout provider connection, disconnection, and error events</span></span> |
| <span data-ttu-id="fc611-144">SignalR.ScaleoutMessageBus</span><span class="sxs-lookup"><span data-stu-id="fc611-144">SignalR.ScaleoutMessageBus</span></span> | <span data-ttu-id="fc611-145">Eventos de mensagens de scale out</span><span class="sxs-lookup"><span data-stu-id="fc611-145">Scaleout messaging events</span></span> |
| <span data-ttu-id="fc611-146">SignalR.Transports.WebSocketTransport</span><span class="sxs-lookup"><span data-stu-id="fc611-146">SignalR.Transports.WebSocketTransport</span></span> | <span data-ttu-id="fc611-147">Conexão de transporte WebSocket, desconexão, mensagens e eventos de erro</span><span class="sxs-lookup"><span data-stu-id="fc611-147">WebSocket transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="fc611-148">SignalR.Transports.ServerSentEventsTransport</span><span class="sxs-lookup"><span data-stu-id="fc611-148">SignalR.Transports.ServerSentEventsTransport</span></span> | <span data-ttu-id="fc611-149">Conexão de transporte ServerSentEvents, desconexão, mensagens e eventos de erro</span><span class="sxs-lookup"><span data-stu-id="fc611-149">ServerSentEvents transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="fc611-150">SignalR.Transports.ForeverFrameTransport</span><span class="sxs-lookup"><span data-stu-id="fc611-150">SignalR.Transports.ForeverFrameTransport</span></span> | <span data-ttu-id="fc611-151">Conexão de transporte ForeverFrame, desconexão, mensagens e eventos de erro</span><span class="sxs-lookup"><span data-stu-id="fc611-151">ForeverFrame transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="fc611-152">SignalR.Transports.LongPollingTransport</span><span class="sxs-lookup"><span data-stu-id="fc611-152">SignalR.Transports.LongPollingTransport</span></span> | <span data-ttu-id="fc611-153">Conexão de transporte LongPolling, desconexão, mensagens e eventos de erro</span><span class="sxs-lookup"><span data-stu-id="fc611-153">LongPolling transport connection, disconnection, messaging, and error events</span></span> |
| <span data-ttu-id="fc611-154">SignalR.Transports.TransportHeartBeat</span><span class="sxs-lookup"><span data-stu-id="fc611-154">SignalR.Transports.TransportHeartBeat</span></span> | <span data-ttu-id="fc611-155">Eventos de conexão de transporte, desconexão e KeepAlive</span><span class="sxs-lookup"><span data-stu-id="fc611-155">Transport connection, disconnection, and keepalive events</span></span> |
| <span data-ttu-id="fc611-156">SignalR.ReflectedHubDescriptorProvider</span><span class="sxs-lookup"><span data-stu-id="fc611-156">SignalR.ReflectedHubDescriptorProvider</span></span> | <span data-ttu-id="fc611-157">Eventos de descoberta de Hub</span><span class="sxs-lookup"><span data-stu-id="fc611-157">Hub discovery events</span></span> |

<a id="server_text"></a>
### <a name="logging-server-events-to-text-files"></a><span data-ttu-id="fc611-158">Registrando eventos de servidor em arquivos de texto</span><span class="sxs-lookup"><span data-stu-id="fc611-158">Logging server events to text files</span></span>

<span data-ttu-id="fc611-159">O código a seguir mostra como habilitar o rastreamento para cada categoria de evento.</span><span class="sxs-lookup"><span data-stu-id="fc611-159">The following code shows how to enable tracing for each category of event.</span></span> <span data-ttu-id="fc611-160">Esta amostra configura o aplicativo para registrar eventos em arquivos de texto.</span><span class="sxs-lookup"><span data-stu-id="fc611-160">This sample configures the application to log events to text files.</span></span>

<span data-ttu-id="fc611-161">**Código do servidor XML para habilitar o rastreamento**</span><span class="sxs-lookup"><span data-stu-id="fc611-161">**XML server code for enabling tracing**</span></span>

[!code-html[Main](enabling-signalr-tracing/samples/sample1.html)]

<span data-ttu-id="fc611-162">No código acima, a entrada `SignalRSwitch` especifica a [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) usada para os eventos enviados para o log especificado.</span><span class="sxs-lookup"><span data-stu-id="fc611-162">In the code above, the `SignalRSwitch` entry specifies the [TraceLevel](https://msdn.microsoft.com/library/system.diagnostics.tracelevel(v=vs.110).aspx) used for events sent to the specified log.</span></span> <span data-ttu-id="fc611-163">Nesse caso, ele é definido como `Verbose`, o que significa que todas as mensagens de depuração e rastreamento são registradas.</span><span class="sxs-lookup"><span data-stu-id="fc611-163">In this case, it is set to `Verbose` which means all debugging and tracing messages are logged.</span></span>

<span data-ttu-id="fc611-164">A saída a seguir mostra as entradas do arquivo `transports.log.txt` para um aplicativo usando o arquivo de configuração acima.</span><span class="sxs-lookup"><span data-stu-id="fc611-164">The following output shows entries from the `transports.log.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="fc611-165">Ele mostra uma nova conexão, uma conexão removida e eventos de pulsação de transporte.</span><span class="sxs-lookup"><span data-stu-id="fc611-165">It shows a new connection, a removed connection, and transport heartbeat events.</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample2.cmd)]

<a id="server_eventlog"></a>
### <a name="logging-server-events-to-the-event-log"></a><span data-ttu-id="fc611-166">Registrando eventos de servidor no log de eventos</span><span class="sxs-lookup"><span data-stu-id="fc611-166">Logging server events to the event log</span></span>

<span data-ttu-id="fc611-167">Para registrar eventos no log de eventos, em vez de um arquivo de texto, altere os valores das entradas no nó `sharedListeners`.</span><span class="sxs-lookup"><span data-stu-id="fc611-167">To log events to the event log rather than a text file, change the values for the entries in the `sharedListeners` node.</span></span> <span data-ttu-id="fc611-168">O código a seguir mostra como registrar eventos de servidor no log de eventos:</span><span class="sxs-lookup"><span data-stu-id="fc611-168">The following code shows how to log server events to the event log:</span></span>

<span data-ttu-id="fc611-169">**Código do servidor XML para registrar eventos no log de eventos**</span><span class="sxs-lookup"><span data-stu-id="fc611-169">**XML server code for logging events to the event log**</span></span>

[!code-xml[Main](enabling-signalr-tracing/samples/sample3.xml)]

<span data-ttu-id="fc611-170">Os eventos são registrados no log do aplicativo e estão disponíveis por meio do Visualizador de Eventos, conforme mostrado abaixo:</span><span class="sxs-lookup"><span data-stu-id="fc611-170">The events are logged in the Application log, and are available through the Event Viewer, as shown below:</span></span>

![Visualizador de Eventos mostrando os logs do Signalr](enabling-signalr-tracing/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="fc611-172">Ao usar o log de eventos, defina **TraceLevel** como **erro** para manter o número de Mensagens gerenciáveis.</span><span class="sxs-lookup"><span data-stu-id="fc611-172">When using the event log, set the **TraceLevel** to **Error** to keep the number of messages manageable.</span></span>

<a id="net_client"></a>
## <a name="enabling-tracing-in-the-net-client-windows-desktop-apps"></a><span data-ttu-id="fc611-173">Habilitando o rastreamento no cliente .NET (aplicativos da área de trabalho do Windows)</span><span class="sxs-lookup"><span data-stu-id="fc611-173">Enabling tracing in the .NET client (Windows Desktop apps)</span></span>

<span data-ttu-id="fc611-174">O cliente .NET pode registrar eventos no console, em um arquivo de texto ou em um log personalizado usando uma implementação de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span><span class="sxs-lookup"><span data-stu-id="fc611-174">The .NET client can log events to the console, a text file, or to a custom log using an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx).</span></span>

<span data-ttu-id="fc611-175">Para habilitar o registro em log no cliente .NET, defina a propriedade `TraceLevel` da conexão com um valor de [tracelevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) e a propriedade `TraceWriter` como uma instância [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) válida.</span><span class="sxs-lookup"><span data-stu-id="fc611-175">To enable logging in the .NET client, set the connection's `TraceLevel` property to a [TraceLevels](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.tracelevels(v=vs.118).aspx) value, and the `TraceWriter` property to a valid [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) instance.</span></span>

<a id="desktop_console"></a>
### <a name="logging-desktop-client-events-to-the-console"></a><span data-ttu-id="fc611-176">Registrando eventos do cliente da área de trabalho no console</span><span class="sxs-lookup"><span data-stu-id="fc611-176">Logging Desktop client events to the console</span></span>

<span data-ttu-id="fc611-177">O código C# a seguir mostra como registrar em log eventos no cliente .net para o console do:</span><span class="sxs-lookup"><span data-stu-id="fc611-177">The following C# code shows how to log events in the .NET client to the console:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample4.cs?highlight=2-3)]

<a id="desktop_text"></a>
### <a name="logging-desktop-client-events-to-a-text-file"></a><span data-ttu-id="fc611-178">Registrando eventos do cliente da área de trabalho em um arquivo de texto</span><span class="sxs-lookup"><span data-stu-id="fc611-178">Logging Desktop client events to a text file</span></span>

<span data-ttu-id="fc611-179">O código C# a seguir mostra como registrar eventos no cliente .net em um arquivo de texto:</span><span class="sxs-lookup"><span data-stu-id="fc611-179">The following C# code shows how to log events in the .NET client to a text file:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample5.cs?highlight=4-5)]

<span data-ttu-id="fc611-180">A saída a seguir mostra as entradas do arquivo `ClientLog.txt` para um aplicativo usando o arquivo de configuração acima.</span><span class="sxs-lookup"><span data-stu-id="fc611-180">The following output shows entries from the `ClientLog.txt` file for an application using the above configuration file.</span></span> <span data-ttu-id="fc611-181">Ele mostra o cliente que está se conectando ao servidor e o Hub que invoca um método de cliente chamado `addMessage`:</span><span class="sxs-lookup"><span data-stu-id="fc611-181">It shows the client connecting to the server, and the hub invoking a client method called `addMessage`:</span></span>

[!code-console[Main](enabling-signalr-tracing/samples/sample6.cmd)]

<a id="phone"></a>
## <a name="enabling-tracing-in-windows-phone-8-clients"></a><span data-ttu-id="fc611-182">Habilitando o rastreamento em clientes Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="fc611-182">Enabling tracing in Windows Phone 8 clients</span></span>

<span data-ttu-id="fc611-183">Os aplicativos signalr para Windows Phone aplicativos usam o mesmo cliente .NET que os aplicativos da área de trabalho, mas o [console. out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) e a gravação em um arquivo com o [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) não estão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="fc611-183">SignalR applications for Windows Phone apps use the same .NET client as desktop apps, but [Console.Out](https://msdn.microsoft.com/library/system.console.out(v=vs.110).aspx) and writing to a file with [StreamWriter](https://msdn.microsoft.com/library/system.io.streamwriter(v=vs.110).aspx) are not available.</span></span> <span data-ttu-id="fc611-184">Em vez disso, você precisa criar uma implementação personalizada de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) para rastreamento.</span><span class="sxs-lookup"><span data-stu-id="fc611-184">Instead, you need to create a custom implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) for tracing.</span></span>

<a id="phone_ui"></a>
### <a name="logging-windows-phone-client-events-to-the-ui"></a><span data-ttu-id="fc611-185">Registrando Windows Phone eventos de cliente na interface do usuário</span><span class="sxs-lookup"><span data-stu-id="fc611-185">Logging Windows Phone client events to the UI</span></span>

<span data-ttu-id="fc611-186">A [base de código do signalr](https://github.com/SignalR/SignalR/archive/master.zip) inclui um exemplo de Windows Phone que grava a saída de rastreamento em um [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) usando uma implementação [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) personalizada chamada `TextBlockWriter`.</span><span class="sxs-lookup"><span data-stu-id="fc611-186">The [SignalR codebase](https://github.com/SignalR/SignalR/archive/master.zip) includes a Windows Phone sample that writes trace output to a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) using a custom [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) implementation called `TextBlockWriter`.</span></span> <span data-ttu-id="fc611-187">Essa classe pode ser encontrada no projeto **Samples/Microsoft. AspNet. signaler. Client. WP8. Samples** .</span><span class="sxs-lookup"><span data-stu-id="fc611-187">This class can be found in the **samples/Microsoft.AspNet.SignalR.Client.WP8.Samples** project.</span></span> <span data-ttu-id="fc611-188">Ao criar uma instância do `TextBlockWriter`, passe o [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx)atual e um [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) onde ele criará um [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) para usar na saída de rastreamento:</span><span class="sxs-lookup"><span data-stu-id="fc611-188">When creating an instance of `TextBlockWriter`, pass in the current [SynchronizationContext](https://msdn.microsoft.com/library/system.threading.synchronizationcontext(v=vs.110).aspx), and a [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) where it will create a [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) to use for trace output:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample7.cs)]

<span data-ttu-id="fc611-189">A saída do rastreamento será então gravada em um novo [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) criado no [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) que você passou:</span><span class="sxs-lookup"><span data-stu-id="fc611-189">The trace output will then be written to a new [TextBlock](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textblock.aspx) created in the [StackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.stackpanel.aspx) you passed in:</span></span>

![](enabling-signalr-tracing/_static/image2.png)

<a id="phone_debug"></a>
### <a name="logging-windows-phone-client-events-to-the-debug-console"></a><span data-ttu-id="fc611-190">Registrando Windows Phone eventos de cliente no console de depuração</span><span class="sxs-lookup"><span data-stu-id="fc611-190">Logging Windows Phone client events to the debug console</span></span>

<span data-ttu-id="fc611-191">Para enviar a saída para o console de depuração em vez da interface do usuário, crie uma implementação de [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) que grave na janela de depuração e atribua-a à propriedade [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) da sua conexão:</span><span class="sxs-lookup"><span data-stu-id="fc611-191">To send output to the debug console rather than the UI, create an implementation of [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter(v=vs.110).aspx) that writes to the debug window, and assign it to your connection's [TraceWriter](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.connection.tracewriter(v=vs.118).aspx) property:</span></span>

[!code-csharp[Main](enabling-signalr-tracing/samples/sample8.cs)]

<span data-ttu-id="fc611-192">As informações de rastreamento serão então gravadas na janela de depuração no Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="fc611-192">Trace information will then be written to the debug window in Visual Studio:</span></span>

![](enabling-signalr-tracing/_static/image3.png)

<a id="javascript"></a>
## <a name="enabling-tracing-in-the-javascript-client"></a><span data-ttu-id="fc611-193">Habilitando o rastreamento no cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="fc611-193">Enabling tracing in the JavaScript client</span></span>

<span data-ttu-id="fc611-194">Para habilitar o log do lado do cliente em uma conexão, defina a propriedade `logging` no objeto de conexão antes de chamar o método `start` para estabelecer a conexão.</span><span class="sxs-lookup"><span data-stu-id="fc611-194">To enable client-side logging on a connection, set the `logging` property on the connection object before you call the `start` method to establish the connection.</span></span>

<span data-ttu-id="fc611-195">**Código JavaScript do cliente para habilitar o rastreamento para o console do navegador (com o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="fc611-195">**Client JavaScript code for enabling tracing to the browser console (with the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample9.js?highlight=1)]

<span data-ttu-id="fc611-196">**Código JavaScript do cliente para habilitar o rastreamento para o console do navegador (sem o proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="fc611-196">**Client JavaScript code for enabling tracing to the browser console (without the generated proxy)**</span></span>

[!code-javascript[Main](enabling-signalr-tracing/samples/sample10.js?highlight=2)]

<span data-ttu-id="fc611-197">Quando o rastreamento está habilitado, o cliente JavaScript registra eventos no console do navegador.</span><span class="sxs-lookup"><span data-stu-id="fc611-197">When tracing is enabled, the JavaScript client logs events to the browser console.</span></span> <span data-ttu-id="fc611-198">Para acessar o console do navegador, consulte [monitoramento de transportes](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span><span class="sxs-lookup"><span data-stu-id="fc611-198">To access the browser console, see [Monitoring Transports](../getting-started/introduction-to-signalr.md#MonitoringTransports).</span></span>

<span data-ttu-id="fc611-199">A captura de tela a seguir mostra um cliente JavaScript do Signalr com rastreamento habilitado.</span><span class="sxs-lookup"><span data-stu-id="fc611-199">The following screenshot shows a SignalR JavaScript client with tracing enabled.</span></span> <span data-ttu-id="fc611-200">Ele mostra os eventos de invocação de conexão e de Hub no console do navegador:</span><span class="sxs-lookup"><span data-stu-id="fc611-200">It shows connection and hub invocation events in the browser console:</span></span>

![Eventos de rastreamento do signalr no console do navegador](enabling-signalr-tracing/_static/image4.png)
