---
uid: signalr/overview/older-versions/signalr-performance
title: Desempenho do sinalizador (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: Desempenho do SignalR
ms.author: bradyg
ms.date: 07/03/2013
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: 915fd822caae9bbcb0a688c6dd7a5b2bda12c9df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579607"
---
# <a name="signalr-performance-signalr-1x"></a><span data-ttu-id="fdd13-103">Desempenho do SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="fdd13-103">SignalR Performance (SignalR 1.x)</span></span>

<span data-ttu-id="fdd13-104">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="fdd13-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="fdd13-105">Este tópico descreve como criar, medir e melhorar o desempenho em um aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="fdd13-105">This topic describes how to design for, measure, and improve performance in a SignalR application.</span></span>

<span data-ttu-id="fdd13-106">Para obter uma apresentação recente sobre o desempenho e o dimensionamento do Signalr, consulte [dimensionando a Web em tempo real com o signalr ASP.net](https://channel9.msdn.com/Events/Build/2013/3-502).</span><span class="sxs-lookup"><span data-stu-id="fdd13-106">For a recent presentation on SignalR performance and scaling, see [Scaling the Real-time Web with ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).</span></span>

<span data-ttu-id="fdd13-107">Este tópico contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="fdd13-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="fdd13-108">Considerações sobre o design</span><span class="sxs-lookup"><span data-stu-id="fdd13-108">Design considerations</span></span>](#design)
- [<span data-ttu-id="fdd13-109">Ajustando o servidor de sinalização para desempenho</span><span class="sxs-lookup"><span data-stu-id="fdd13-109">Tuning your SignalR server for performance</span></span>](#tuning)
- [<span data-ttu-id="fdd13-110">Solucionando problemas de desempenho</span><span class="sxs-lookup"><span data-stu-id="fdd13-110">Troubleshooting performance issues</span></span>](#troubleshooting)
- [<span data-ttu-id="fdd13-111">Usando contadores de desempenho do Signalr</span><span class="sxs-lookup"><span data-stu-id="fdd13-111">Using SignalR performance counters</span></span>](#perfcounters)
- [<span data-ttu-id="fdd13-112">Usando outros contadores de desempenho</span><span class="sxs-lookup"><span data-stu-id="fdd13-112">Using other performance counters</span></span>](#othercounters)
- [<span data-ttu-id="fdd13-113">Outros recursos</span><span class="sxs-lookup"><span data-stu-id="fdd13-113">Other resources</span></span>](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a><span data-ttu-id="fdd13-114">Considerações sobre o design</span><span class="sxs-lookup"><span data-stu-id="fdd13-114">Design considerations</span></span>

<span data-ttu-id="fdd13-115">Esta seção descreve os padrões que podem ser implementados durante o design de um aplicativo Signalr, para garantir que o desempenho não seja prejudicado pela geração de tráfego de rede desnecessário.</span><span class="sxs-lookup"><span data-stu-id="fdd13-115">This section describes patterns that can be implemented during the design of a SignalR application, to ensure that performance is not being hindered by generating unnecessary network traffic.</span></span>

### <a name="throttling-message-frequency"></a><span data-ttu-id="fdd13-116">Frequência da mensagem de limitação</span><span class="sxs-lookup"><span data-stu-id="fdd13-116">Throttling message frequency</span></span>

<span data-ttu-id="fdd13-117">Mesmo em um aplicativo que envia mensagens a uma alta frequência (como um aplicativo de jogos em tempo real), a maioria dos aplicativos não precisa enviar mais de algumas mensagens por um segundo.</span><span class="sxs-lookup"><span data-stu-id="fdd13-117">Even in an application that sends out messages at a high frequency (such as a realtime gaming application), most applications don't need to send more than a few messages a second.</span></span> <span data-ttu-id="fdd13-118">Para reduzir a quantidade de tráfego que cada cliente gera, um loop de mensagem pode ser implementado para que as filas e envie mensagens sem mais frequência do que uma taxa fixa (ou seja, até um determinado número de mensagens serão enviadas a cada segundo, se houver mensagens nesse tempo em Valo a ser enviado).</span><span class="sxs-lookup"><span data-stu-id="fdd13-118">To reduce the amount of traffic that each client generates, a message loop can be implemented that queues and sends out messages no more frequently than a fixed rate (that is, up to a certain number of messages will be sent every second, if there are messages in that time interval to be sent).</span></span> <span data-ttu-id="fdd13-119">Para um aplicativo de exemplo que limita as mensagens a uma determinada taxa (do cliente e do servidor), consulte [tempo real de alta frequência com signalr](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="fdd13-119">For a sample application that throttles messages to a certain rate (from both client and server), see [High-Frequency Realtime with SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).</span></span>

### <a name="reducing-message-size"></a><span data-ttu-id="fdd13-120">Reduzindo o tamanho da mensagem</span><span class="sxs-lookup"><span data-stu-id="fdd13-120">Reducing message size</span></span>

<span data-ttu-id="fdd13-121">Você pode reduzir o tamanho de uma mensagem de sinalização reduzindo o tamanho de seus objetos serializados.</span><span class="sxs-lookup"><span data-stu-id="fdd13-121">You can reduce the size of a SignalR message by reducing the size of your serialized objects.</span></span> <span data-ttu-id="fdd13-122">No código do servidor, se você estiver enviando um objeto que contém propriedades que não precisam ser transmitidas, impeça que essas propriedades sejam serializadas usando o atributo `JsonIgnore`.</span><span class="sxs-lookup"><span data-stu-id="fdd13-122">In server code, if you're sending an object that contains properties that don't need to be transmitted, prevent those properties from being serialized by using the `JsonIgnore` attribute.</span></span> <span data-ttu-id="fdd13-123">Os nomes das propriedades também são armazenados na mensagem; os nomes das propriedades podem ser reduzidos usando o atributo `JsonProperty`.</span><span class="sxs-lookup"><span data-stu-id="fdd13-123">The names of properties are also stored in the message; the names of properties can be shortened using the `JsonProperty` attribute.</span></span> <span data-ttu-id="fdd13-124">O exemplo de código a seguir demonstra como excluir uma propriedade de ser enviada ao cliente e como reduzir os nomes de propriedade:</span><span class="sxs-lookup"><span data-stu-id="fdd13-124">The following code sample demonstrates how to exclude a property from being sent to the client, and how to shorten property names:</span></span>

<span data-ttu-id="fdd13-125">**Código do servidor .NET que demonstra o atributo JsonIgnore para excluir dados de serem enviados ao cliente e o atributo Jsonproperty para reduzir o tamanho da mensagem**</span><span class="sxs-lookup"><span data-stu-id="fdd13-125">**.NET server code that demonstrates the JsonIgnore attribute to exclude data from being sent to the client, and the JsonProperty attribute to reduce message size**</span></span>

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

<span data-ttu-id="fdd13-126">Para manter a legibilidade/facilidade de manutenção no código do cliente, os nomes de propriedade abreviados podem ser remapeados para nomes amigáveis para os seres humanos depois que a mensagem é recebida.</span><span class="sxs-lookup"><span data-stu-id="fdd13-126">In order to retain readability/ maintainability in the client code, the abbreviated property names can be remapped to human-friendly names after the message is received.</span></span> <span data-ttu-id="fdd13-127">O exemplo de código a seguir demonstra uma possível maneira de remapear nomes reduzidos para mais longos, definindo um contrato de mensagem (mapeamento) e usando a função `reMap` para aplicar o contrato à classe de mensagem otimizada:</span><span class="sxs-lookup"><span data-stu-id="fdd13-127">The following code sample demonstrates one possible way of remapping shortened names to longer ones, by defining a message contract (mapping), and using the `reMap` function to apply the contract to the optimized message class:</span></span>

<span data-ttu-id="fdd13-128">**Código JavaScript do lado do cliente que remapeia nomes de propriedade reduzidos a nomes legíveis para pessoas**</span><span class="sxs-lookup"><span data-stu-id="fdd13-128">**Client-side JavaScript code that remaps shortened property names to human-readable names**</span></span>

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

<span data-ttu-id="fdd13-129">Os nomes também podem ser reduzidos em mensagens do cliente para o servidor, usando o mesmo método.</span><span class="sxs-lookup"><span data-stu-id="fdd13-129">Names can be shortened in messages from the client to the server as well, using the same method.</span></span>

<span data-ttu-id="fdd13-130">Reduzir a superfície de memória (ou seja, a quantidade de memória usada para a mensagem) do objeto de mensagem também pode melhorar o desempenho.</span><span class="sxs-lookup"><span data-stu-id="fdd13-130">Reducing the memory footprint (that is, the amount of memory used for the message) of the message object can also improve performance.</span></span> <span data-ttu-id="fdd13-131">Por exemplo, se o intervalo completo de um `int` não for necessário, um `short` ou `byte` poderá ser usado em vez disso.</span><span class="sxs-lookup"><span data-stu-id="fdd13-131">For example, if the full range of an `int` is not needed, a `short` or `byte` can be used instead.</span></span>

<span data-ttu-id="fdd13-132">Como as mensagens são armazenadas no barramento de mensagem na memória do servidor, a redução do tamanho das mensagens também pode resolver problemas de memória do servidor.</span><span class="sxs-lookup"><span data-stu-id="fdd13-132">Since messages are stored in the message bus in server memory, reducing the size of messages can also address server memory issues.</span></span>

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a><span data-ttu-id="fdd13-133">Ajustando o servidor de sinalização para desempenho</span><span class="sxs-lookup"><span data-stu-id="fdd13-133">Tuning your SignalR server for performance</span></span>

<span data-ttu-id="fdd13-134">As definições de configuração a seguir podem ser usadas para ajustar o servidor para melhorar o desempenho em um aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="fdd13-134">The following configuration settings can be used to tune your server for better performance in a SignalR application.</span></span> <span data-ttu-id="fdd13-135">Para obter informações gerais sobre como melhorar o desempenho em um aplicativo ASP.NET, consulte [melhorando o desempenho do ASP.net](https://msdn.microsoft.com/library/ff647787.aspx).</span><span class="sxs-lookup"><span data-stu-id="fdd13-135">For general information on how to improve performance in an ASP.NET application, see [Improving ASP.NET Performance](https://msdn.microsoft.com/library/ff647787.aspx).</span></span>

<span data-ttu-id="fdd13-136">**Definições de configuração do signalr**</span><span class="sxs-lookup"><span data-stu-id="fdd13-136">**SignalR configuration settings**</span></span>

- <span data-ttu-id="fdd13-137">**DefaultMessageBufferSize**: por padrão, o signalr retém 1000 mensagens na memória por Hub por conexão.</span><span class="sxs-lookup"><span data-stu-id="fdd13-137">**DefaultMessageBufferSize**: By default, SignalR retains 1000 messages in memory per hub per connection.</span></span> <span data-ttu-id="fdd13-138">Se mensagens grandes estiverem sendo usadas, isso poderá criar problemas de memória que podem ser aliviados reduzindo esse valor.</span><span class="sxs-lookup"><span data-stu-id="fdd13-138">If large messages are being used, this may create memory issues which can be alleviated by reducing this value.</span></span> <span data-ttu-id="fdd13-139">Essa configuração pode ser definida no manipulador de eventos `Application_Start` em um aplicativo ASP.NET ou no método `Configuration` de uma classe de inicialização OWIN em um aplicativo auto-hospedado.</span><span class="sxs-lookup"><span data-stu-id="fdd13-139">This setting can be set in the `Application_Start` event handler in an ASP.NET application, or in the `Configuration` method of an OWIN startup class in a self-hosted application.</span></span> <span data-ttu-id="fdd13-140">O exemplo a seguir demonstra como reduzir esse valor para reduzir a superfície de memória do seu aplicativo a fim de reduzir a quantidade de memória do servidor usada:</span><span class="sxs-lookup"><span data-stu-id="fdd13-140">The following sample demonstrates how to reduce this value in order to reduce the memory footprint of your application in order to reduce the amount of server memory used:</span></span>

    <span data-ttu-id="fdd13-141">**Código do servidor .NET no global. asax para diminuir o tamanho do buffer de mensagens padrão**</span><span class="sxs-lookup"><span data-stu-id="fdd13-141">**.NET server code in Global.asax for decreasing default message buffer size**</span></span>

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

<span data-ttu-id="fdd13-142">**Definições de configuração do IIS**</span><span class="sxs-lookup"><span data-stu-id="fdd13-142">**IIS configuration settings**</span></span>

- <span data-ttu-id="fdd13-143">**Máximo de solicitações simultâneas por aplicativo**: aumentar o número de solicitações simultâneas do IIS aumentará os recursos do servidor disponíveis para atender a solicitações.</span><span class="sxs-lookup"><span data-stu-id="fdd13-143">**Max concurrent requests per application**: Increasing the number of concurrent IIS requests will increase server resources available for serving requests.</span></span> <span data-ttu-id="fdd13-144">O valor padrão é 5000; para aumentar essa configuração, execute os seguintes comandos em um prompt de comandos com privilégios elevados:</span><span class="sxs-lookup"><span data-stu-id="fdd13-144">The default value is 5000; to increase this setting, execute the following commands in an elevated command prompt:</span></span>

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

<span data-ttu-id="fdd13-145">**Definições de configuração do ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="fdd13-145">**ASP.NET configuration settings**</span></span>

<span data-ttu-id="fdd13-146">Esta seção inclui definições de configuração que podem ser definidas no arquivo `aspnet.config`.</span><span class="sxs-lookup"><span data-stu-id="fdd13-146">This section includes configuration settings that can be set in the `aspnet.config` file.</span></span> <span data-ttu-id="fdd13-147">Esse arquivo é encontrado em um dos dois locais, dependendo da plataforma:</span><span class="sxs-lookup"><span data-stu-id="fdd13-147">This file is found in one of two locations, depending on platform:</span></span>

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

<span data-ttu-id="fdd13-148">As configurações de ASP.NET que podem melhorar o desempenho do Signalr incluem o seguinte:</span><span class="sxs-lookup"><span data-stu-id="fdd13-148">ASP.NET settings that may improve SignalR performance include the following:</span></span>

- <span data-ttu-id="fdd13-149">**Máximo de solicitações simultâneas por CPU**: aumentar essa configuração pode aliviar afunilamentos de desempenho.</span><span class="sxs-lookup"><span data-stu-id="fdd13-149">**Maximum concurrent requests per CPU**: Increasing this setting may alleviate performance bottlenecks.</span></span> <span data-ttu-id="fdd13-150">Para aumentar essa configuração, adicione o seguinte parâmetro de configuração ao arquivo de `aspnet.config`:</span><span class="sxs-lookup"><span data-stu-id="fdd13-150">To increase this setting, add the following configuration setting to the `aspnet.config` file:</span></span>

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- <span data-ttu-id="fdd13-151">**Limite da fila de solicitações**: quando o número total de conexões exceder a configuração de `maxConcurrentRequestsPerCPU`, o ASP.net começará a limitar as solicitações usando uma fila.</span><span class="sxs-lookup"><span data-stu-id="fdd13-151">**Request Queue Limit**: When the total number of connections exceeds the `maxConcurrentRequestsPerCPU` setting, ASP.NET will start throttling requests using a queue.</span></span> <span data-ttu-id="fdd13-152">Para aumentar o tamanho da fila, você pode aumentar a configuração de `requestQueueLimit`.</span><span class="sxs-lookup"><span data-stu-id="fdd13-152">To increase the size of the queue, you can increase the `requestQueueLimit` setting.</span></span> <span data-ttu-id="fdd13-153">Para fazer isso, adicione a definição de configuração a seguir ao nó `processModel` no `config/machine.config` (em vez de `aspnet.config`):</span><span class="sxs-lookup"><span data-stu-id="fdd13-153">To do this, add the following configuration setting to the `processModel` node in `config/machine.config` (rather than `aspnet.config`):</span></span>

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a><span data-ttu-id="fdd13-154">Solucionando problemas de desempenho</span><span class="sxs-lookup"><span data-stu-id="fdd13-154">Troubleshooting performance issues</span></span>

<span data-ttu-id="fdd13-155">Esta seção descreve maneiras de encontrar afunilamentos de desempenho em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fdd13-155">This section describes ways to find performance bottlenecks in your application.</span></span>

### <a name="verifying-that-websocket-is-being-used"></a><span data-ttu-id="fdd13-156">Verificando se o WebSocket está sendo usado</span><span class="sxs-lookup"><span data-stu-id="fdd13-156">Verifying that WebSocket is being used</span></span>

<span data-ttu-id="fdd13-157">Embora o Signalr possa usar uma variedade de transportes para comunicação entre o cliente e o servidor, o WebSocket oferece uma vantagem de desempenho significativa e deve ser usado se o cliente e o servidor oferecerem suporte a ele.</span><span class="sxs-lookup"><span data-stu-id="fdd13-157">While SignalR can use a variety of transports for communication between client and server, WebSocket offers a significant performance advantage, and should be used if the client and server support it.</span></span> <span data-ttu-id="fdd13-158">Para determinar se o cliente e o servidor atendem aos requisitos do WebSocket, consulte [transportes e fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="fdd13-158">To determine if your client and server meet the requirements for WebSocket, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span> <span data-ttu-id="fdd13-159">Para determinar qual transporte está sendo usado em seu aplicativo, você pode usar as ferramentas de desenvolvedor do navegador e examinar os logs para ver qual transporte está sendo usado para a conexão.</span><span class="sxs-lookup"><span data-stu-id="fdd13-159">To determine what transport is being used in your application, you can use the browser developer tools, and examine the logs to see what transport is being used for the connection.</span></span> <span data-ttu-id="fdd13-160">Para obter informações sobre como usar as ferramentas de desenvolvimento do navegador no Internet Explorer e no Chrome, consulte [transportes e fallbacks](../getting-started/introduction-to-signalr.md#transports).</span><span class="sxs-lookup"><span data-stu-id="fdd13-160">For information on using the browser development tools in Internet Explorer and Chrome, see [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports).</span></span>

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a><span data-ttu-id="fdd13-161">Usando contadores de desempenho do Signalr</span><span class="sxs-lookup"><span data-stu-id="fdd13-161">Using SignalR performance counters</span></span>

<span data-ttu-id="fdd13-162">Esta seção descreve como habilitar e usar contadores de desempenho do Signalr, encontrados no pacote `Microsoft.AspNet.SignalR.Utils`.</span><span class="sxs-lookup"><span data-stu-id="fdd13-162">This section describes how to enable and use SignalR performance counters, found in the `Microsoft.AspNet.SignalR.Utils` package.</span></span>

### <a name="installing-signalrexe"></a><span data-ttu-id="fdd13-163">Instalando o signalr. exe</span><span class="sxs-lookup"><span data-stu-id="fdd13-163">Installing signalr.exe</span></span>

<span data-ttu-id="fdd13-164">Os contadores de desempenho podem ser adicionados ao servidor usando um utilitário chamado Signalr. exe.</span><span class="sxs-lookup"><span data-stu-id="fdd13-164">Performance counters can be added to the server using a utility called SignalR.exe.</span></span> <span data-ttu-id="fdd13-165">Para instalar esse utilitário, siga estas etapas:</span><span class="sxs-lookup"><span data-stu-id="fdd13-165">To install this utility, follow these steps:</span></span>

1. <span data-ttu-id="fdd13-166">No Visual Studio, selecione **ferramentas** > **Gerenciador de pacotes NuGet** > **gerenciar pacotes NuGet para a solução**</span><span class="sxs-lookup"><span data-stu-id="fdd13-166">In Visual Studio, select **Tools** > **NuGet Package Manager** > **Manage NuGet Packages for Solution**</span></span>
2. <span data-ttu-id="fdd13-167">Procure **signalr. utils**e selecione instalar.</span><span class="sxs-lookup"><span data-stu-id="fdd13-167">Search for **signalr.utils**, and select Install.</span></span>

    ![](signalr-performance/_static/image1.png)
3. <span data-ttu-id="fdd13-168">Aceite o contrato de licença para instalar o pacote.</span><span class="sxs-lookup"><span data-stu-id="fdd13-168">Accept the license agreement to install the package.</span></span>
4. <span data-ttu-id="fdd13-169">O signalr. exe será instalado em `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span><span class="sxs-lookup"><span data-stu-id="fdd13-169">SignalR.exe will be installed to `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.</span></span>

### <a name="installing-performance-counters-with-signalrexe"></a><span data-ttu-id="fdd13-170">Instalando contadores de desempenho com Signalr. exe</span><span class="sxs-lookup"><span data-stu-id="fdd13-170">Installing performance counters with SignalR.exe</span></span>

<span data-ttu-id="fdd13-171">Para instalar contadores de desempenho do Signalr, execute Signalr. exe em um prompt de comando com privilégios elevados com o seguinte parâmetro:</span><span class="sxs-lookup"><span data-stu-id="fdd13-171">To install SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

<span data-ttu-id="fdd13-172">Para remover contadores de desempenho do Signalr, execute Signalr. exe em um prompt de comando com privilégios elevados com o seguinte parâmetro:</span><span class="sxs-lookup"><span data-stu-id="fdd13-172">To remove SignalR performance counters, run SignalR.exe in an elevated command prompt with the following parameter:</span></span>

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a><span data-ttu-id="fdd13-173">Contadores de desempenho do signalr</span><span class="sxs-lookup"><span data-stu-id="fdd13-173">SignalR Performance counters</span></span>

<span data-ttu-id="fdd13-174">O pacote de utilitários instala os seguintes contadores de desempenho.</span><span class="sxs-lookup"><span data-stu-id="fdd13-174">The utilities package installs the following performance counters.</span></span> <span data-ttu-id="fdd13-175">Os contadores "total" medem o número de eventos desde a última reinicialização do pool de aplicativos ou do servidor.</span><span class="sxs-lookup"><span data-stu-id="fdd13-175">The "Total" counters measure the number of events since the last application pool or server restart.</span></span>

<span data-ttu-id="fdd13-176">**Métricas de conexão**</span><span class="sxs-lookup"><span data-stu-id="fdd13-176">**Connection metrics**</span></span>

<span data-ttu-id="fdd13-177">As métricas a seguir medem os eventos de tempo de vida da conexão que ocorrem.</span><span class="sxs-lookup"><span data-stu-id="fdd13-177">The following metrics measure the connection lifetime events that occur.</span></span> <span data-ttu-id="fdd13-178">Para obter mais informações, consulte [noções básicas e manipulação de eventos de tempo de vida da conexão](../guide-to-the-api/handling-connection-lifetime-events.md).</span><span class="sxs-lookup"><span data-stu-id="fdd13-178">For more information, see [Understanding and Handling Connection Lifetime Events](../guide-to-the-api/handling-connection-lifetime-events.md).</span></span>

- <span data-ttu-id="fdd13-179">**Conexões conectadas**</span><span class="sxs-lookup"><span data-stu-id="fdd13-179">**Connections Connected**</span></span>
- <span data-ttu-id="fdd13-180">**Conexões reconectadas**</span><span class="sxs-lookup"><span data-stu-id="fdd13-180">**Connections Reconnected**</span></span>
- <span data-ttu-id="fdd13-181">**Conexões desconectadas**</span><span class="sxs-lookup"><span data-stu-id="fdd13-181">**Connections Disconnected**</span></span>
- <span data-ttu-id="fdd13-182">**Conexões atuais**</span><span class="sxs-lookup"><span data-stu-id="fdd13-182">**Connections Current**</span></span>

<span data-ttu-id="fdd13-183">**Métricas de mensagem**</span><span class="sxs-lookup"><span data-stu-id="fdd13-183">**Message metrics**</span></span>

<span data-ttu-id="fdd13-184">As métricas a seguir medem o tráfego de mensagens gerado pelo Signalr.</span><span class="sxs-lookup"><span data-stu-id="fdd13-184">The following metrics measure the message traffic generated by SignalR.</span></span>

- <span data-ttu-id="fdd13-185">**Total de mensagens de conexão recebidas**</span><span class="sxs-lookup"><span data-stu-id="fdd13-185">**Connection Messages Received Total**</span></span>
- <span data-ttu-id="fdd13-186">**Total de mensagens de conexão enviadas**</span><span class="sxs-lookup"><span data-stu-id="fdd13-186">**Connection Messages Sent Total**</span></span>
- <span data-ttu-id="fdd13-187">**Mensagens de conexão recebidas/s**</span><span class="sxs-lookup"><span data-stu-id="fdd13-187">**Connection Messages Received/Sec**</span></span>
- <span data-ttu-id="fdd13-188">**Mensagens de conexão enviadas/s**</span><span class="sxs-lookup"><span data-stu-id="fdd13-188">**Connection Messages Sent/Sec**</span></span>

<span data-ttu-id="fdd13-189">**Métricas do barramento de mensagem**</span><span class="sxs-lookup"><span data-stu-id="fdd13-189">**Message bus metrics**</span></span>

<span data-ttu-id="fdd13-190">As métricas a seguir medem o tráfego por meio do barramento de mensagem do Signalr interno, a fila na qual todas as mensagens de entrada e saída do Signalr são colocadas.</span><span class="sxs-lookup"><span data-stu-id="fdd13-190">The following metrics measure traffic through the internal SignalR message bus, the queue in which all incoming and outgoing SignalR messages are placed.</span></span> <span data-ttu-id="fdd13-191">Uma mensagem é **publicada** quando enviada ou difundida.</span><span class="sxs-lookup"><span data-stu-id="fdd13-191">A message is **Published** when it is sent or broadcast.</span></span> <span data-ttu-id="fdd13-192">Um **assinante** neste contexto é uma assinatura no barramento de mensagem; Isso deve ser igual ao número de clientes mais o próprio servidor.</span><span class="sxs-lookup"><span data-stu-id="fdd13-192">A **Subscriber** in this context is a subscription on the message bus; this should equal the number of clients plus the server itself.</span></span> <span data-ttu-id="fdd13-193">Um **trabalho alocado** é um componente que envia dados para conexões ativas; um **trabalho ocupado** é aquele que está enviando ativamente uma mensagem.</span><span class="sxs-lookup"><span data-stu-id="fdd13-193">An **Allocated Worker** is a component that sends data to active connections; a **Busy Worker** is one that is actively sending a message.</span></span>

- <span data-ttu-id="fdd13-194">**Total de mensagens de barramento de mensagem recebidas**</span><span class="sxs-lookup"><span data-stu-id="fdd13-194">**Message Bus Messages Received Total**</span></span>
- <span data-ttu-id="fdd13-195">**Mensagens de barramento de mensagem recebidas/s**</span><span class="sxs-lookup"><span data-stu-id="fdd13-195">**Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="fdd13-196">**Total de mensagens do barramento de mensagem publicadas**</span><span class="sxs-lookup"><span data-stu-id="fdd13-196">**Message Bus Messages Published Total**</span></span>
- <span data-ttu-id="fdd13-197">**Mensagens do barramento de mensagem publicadas/s**</span><span class="sxs-lookup"><span data-stu-id="fdd13-197">**Message Bus Messages Published/Sec**</span></span>
- <span data-ttu-id="fdd13-198">**Assinantes atuais do barramento de mensagem**</span><span class="sxs-lookup"><span data-stu-id="fdd13-198">**Message Bus Subscribers Current**</span></span>
- <span data-ttu-id="fdd13-199">**Total de assinantes do barramento de mensagem**</span><span class="sxs-lookup"><span data-stu-id="fdd13-199">**Message Bus Subscribers Total**</span></span>
- <span data-ttu-id="fdd13-200">**Assinantes do barramento de mensagem/s**</span><span class="sxs-lookup"><span data-stu-id="fdd13-200">**Message Bus Subscribers/Sec**</span></span>
- <span data-ttu-id="fdd13-201">**Trabalhadores alocados do barramento de mensagem**</span><span class="sxs-lookup"><span data-stu-id="fdd13-201">**Message Bus Allocated Workers**</span></span>
- <span data-ttu-id="fdd13-202">**Trabalhadores ocupados do barramento de mensagem**</span><span class="sxs-lookup"><span data-stu-id="fdd13-202">**Message Bus Busy Workers**</span></span>
- <span data-ttu-id="fdd13-203">**Tópicos do barramento de mensagem atual**</span><span class="sxs-lookup"><span data-stu-id="fdd13-203">**Message Bus Topics Current**</span></span>

<span data-ttu-id="fdd13-204">**Métricas de erro**</span><span class="sxs-lookup"><span data-stu-id="fdd13-204">**Error metrics**</span></span>

<span data-ttu-id="fdd13-205">As métricas a seguir medem os erros gerados pelo tráfego de mensagens do Signalr.</span><span class="sxs-lookup"><span data-stu-id="fdd13-205">The following metrics measure errors generated by SignalR message traffic.</span></span> <span data-ttu-id="fdd13-206">Os erros de **resolução de Hub** ocorrem quando um método de Hub ou Hub não pode ser resolvido.</span><span class="sxs-lookup"><span data-stu-id="fdd13-206">**Hub Resolution** errors occur when a hub or hub method cannot be resolved.</span></span> <span data-ttu-id="fdd13-207">Erros de **invocação de Hub** são exceções geradas ao invocar um método de Hub.</span><span class="sxs-lookup"><span data-stu-id="fdd13-207">**Hub Invocation** errors are exceptions thrown while invoking a hub method.</span></span> <span data-ttu-id="fdd13-208">Os erros de **transporte** são erros de conexão lançados durante uma solicitação ou resposta http.</span><span class="sxs-lookup"><span data-stu-id="fdd13-208">**Transport** errors are connection errors thrown during an HTTP request or response.</span></span>

- <span data-ttu-id="fdd13-209">**Erros: todos os totais**</span><span class="sxs-lookup"><span data-stu-id="fdd13-209">**Errors: All Total**</span></span>
- <span data-ttu-id="fdd13-210">**Erros: todos/s**</span><span class="sxs-lookup"><span data-stu-id="fdd13-210">**Errors: All/Sec**</span></span>
- <span data-ttu-id="fdd13-211">**Erros: resolução do Hub total**</span><span class="sxs-lookup"><span data-stu-id="fdd13-211">**Errors: Hub Resolution Total**</span></span>
- <span data-ttu-id="fdd13-212">**Erros: resolução do Hub/s**</span><span class="sxs-lookup"><span data-stu-id="fdd13-212">**Errors: Hub Resolution/Sec**</span></span>
- <span data-ttu-id="fdd13-213">**Erros: total de invocação de Hub**</span><span class="sxs-lookup"><span data-stu-id="fdd13-213">**Errors: Hub Invocation Total**</span></span>
- <span data-ttu-id="fdd13-214">**Erros: invocação de Hub/s**</span><span class="sxs-lookup"><span data-stu-id="fdd13-214">**Errors: Hub Invocation/Sec**</span></span>
- <span data-ttu-id="fdd13-215">**Erros: transporte total**</span><span class="sxs-lookup"><span data-stu-id="fdd13-215">**Errors: Transport Total**</span></span>
- <span data-ttu-id="fdd13-216">**Erros: transporte/s**</span><span class="sxs-lookup"><span data-stu-id="fdd13-216">**Errors: Transport/Sec**</span></span>

<span data-ttu-id="fdd13-217">**Métricas de dimensionamento**</span><span class="sxs-lookup"><span data-stu-id="fdd13-217">**Scaleout metrics**</span></span>

<span data-ttu-id="fdd13-218">As métricas a seguir medem o tráfego e os erros gerados pelo provedor de scale out.</span><span class="sxs-lookup"><span data-stu-id="fdd13-218">The following metrics measure traffic and errors generated by the scaleout provider.</span></span> <span data-ttu-id="fdd13-219">Um **fluxo** neste contexto é uma unidade de escala usada pelo provedor de scale out; Esta é uma tabela se SQL Server for usado, um tópico se o barramento de serviço for usado e uma assinatura se Redis for usado.</span><span class="sxs-lookup"><span data-stu-id="fdd13-219">A **Stream** in this context is a scale unit used by the scaleout provider; this is a table if SQL Server is used, a Topic if Service Bus is used, and a Subscription if Redis is used.</span></span> <span data-ttu-id="fdd13-220">Por padrão, apenas um fluxo é usado, mas isso pode ser aumentado por meio da configuração em SQL Server e barramento de serviço.</span><span class="sxs-lookup"><span data-stu-id="fdd13-220">By default, only one stream is used, but this can be increased through configuration on SQL Server and Service Bus.</span></span> <span data-ttu-id="fdd13-221">Um fluxo de **buffer** é aquele que entrou em um estado com falha; Quando o fluxo estiver no estado com falha, todas as mensagens enviadas ao backplane falharão imediatamente até que o fluxo não esteja mais falhando.</span><span class="sxs-lookup"><span data-stu-id="fdd13-221">A **Buffering** stream is one that has entered a faulted state; when the stream is in the faulted state, all messages sent to the backplane will fail immediately until the stream is no longer faulting.</span></span> <span data-ttu-id="fdd13-222">O **comprimento da fila de envio** é o número de mensagens que foram lançadas, mas ainda não enviadas.</span><span class="sxs-lookup"><span data-stu-id="fdd13-222">The **Send Queue Length** is the number of messages that have been posted but not yet sent.</span></span>

- <span data-ttu-id="fdd13-223">**Mensagens de barramento de mensagem de escalabilidade recebidas/s**</span><span class="sxs-lookup"><span data-stu-id="fdd13-223">**Scaleout Message Bus Messages Received/Sec**</span></span>
- <span data-ttu-id="fdd13-224">**Total de fluxos de expansão**</span><span class="sxs-lookup"><span data-stu-id="fdd13-224">**Scaleout Streams Total**</span></span>
- <span data-ttu-id="fdd13-225">**Fluxos de expansão abertos**</span><span class="sxs-lookup"><span data-stu-id="fdd13-225">**Scaleout Streams Open**</span></span>
- <span data-ttu-id="fdd13-226">**Buffer de fluxos de expansão**</span><span class="sxs-lookup"><span data-stu-id="fdd13-226">**Scaleout Streams Buffering**</span></span>
- <span data-ttu-id="fdd13-227">**Total de erros de scale out**</span><span class="sxs-lookup"><span data-stu-id="fdd13-227">**Scaleout Errors Total**</span></span>
- <span data-ttu-id="fdd13-228">**Erros de scale out/s**</span><span class="sxs-lookup"><span data-stu-id="fdd13-228">**Scaleout Errors/Sec**</span></span>
- <span data-ttu-id="fdd13-229">**Tamanho da fila de envio de expansão**</span><span class="sxs-lookup"><span data-stu-id="fdd13-229">**Scaleout Send Queue Length**</span></span>

<span data-ttu-id="fdd13-230">Para saber mais sobre o que esses contadores estão medindo, confira o [dimensionamento do signalr com o barramento de serviço do Azure](scaleout-with-windows-azure-service-bus.md).</span><span class="sxs-lookup"><span data-stu-id="fdd13-230">For more information on what these counters are measuring, see [SignalR Scaleout with Azure Service Bus](scaleout-with-windows-azure-service-bus.md).</span></span>

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a><span data-ttu-id="fdd13-231">Usando outros contadores de desempenho</span><span class="sxs-lookup"><span data-stu-id="fdd13-231">Using other performance counters</span></span>

<span data-ttu-id="fdd13-232">Os contadores de desempenho a seguir também podem ser úteis para monitorar o desempenho do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="fdd13-232">The following performance counters may also be useful in monitoring your application's performance.</span></span>

<span data-ttu-id="fdd13-233">**Memória**</span><span class="sxs-lookup"><span data-stu-id="fdd13-233">**Memory**</span></span>

- <span data-ttu-id="fdd13-234">Memória .NET CLR n º de bytes em todos os heaps (para w3wp)</span><span class="sxs-lookup"><span data-stu-id="fdd13-234">.NET CLR Memory# bytes in all Heaps (for w3wp)</span></span>

<span data-ttu-id="fdd13-235">**ASP.NET**</span><span class="sxs-lookup"><span data-stu-id="fdd13-235">**ASP.NET**</span></span>

- <span data-ttu-id="fdd13-236">ASP. solicitações atual</span><span class="sxs-lookup"><span data-stu-id="fdd13-236">ASP.NET\Requests Current</span></span>
- <span data-ttu-id="fdd13-237">ASP.NET\Queued</span><span class="sxs-lookup"><span data-stu-id="fdd13-237">ASP.NET\Queued</span></span>
- <span data-ttu-id="fdd13-238">ASP. NET\Rejected</span><span class="sxs-lookup"><span data-stu-id="fdd13-238">ASP.NET\Rejected</span></span>

<span data-ttu-id="fdd13-239">**CPU**</span><span class="sxs-lookup"><span data-stu-id="fdd13-239">**CPU**</span></span>

- <span data-ttu-id="fdd13-240">Tempo de Information\Processor do processador</span><span class="sxs-lookup"><span data-stu-id="fdd13-240">Processor Information\Processor Time</span></span>

<span data-ttu-id="fdd13-241">**TCP/IP**</span><span class="sxs-lookup"><span data-stu-id="fdd13-241">**TCP/IP**</span></span>

- <span data-ttu-id="fdd13-242">TCPv6/conexões estabelecidas</span><span class="sxs-lookup"><span data-stu-id="fdd13-242">TCPv6/Connections Established</span></span>
- <span data-ttu-id="fdd13-243">TCPv4/conexões estabelecidas</span><span class="sxs-lookup"><span data-stu-id="fdd13-243">TCPv4/Connections Established</span></span>

<span data-ttu-id="fdd13-244">**Serviço Web**</span><span class="sxs-lookup"><span data-stu-id="fdd13-244">**Web Service**</span></span>

- <span data-ttu-id="fdd13-245">Conexões de \ da Web</span><span class="sxs-lookup"><span data-stu-id="fdd13-245">Web Service\Current Connections</span></span>
- <span data-ttu-id="fdd13-246">Conexões de Service\Maximum da Web</span><span class="sxs-lookup"><span data-stu-id="fdd13-246">Web Service\Maximum Connections</span></span>

<span data-ttu-id="fdd13-247">**Threading**</span><span class="sxs-lookup"><span data-stu-id="fdd13-247">**Threading**</span></span>

- <span data-ttu-id="fdd13-248">.NET CLR LocksAndThreads\# de threads lógicos atuais</span><span class="sxs-lookup"><span data-stu-id="fdd13-248">.NET CLR LocksAndThreads\# of current logical Threads</span></span>
- <span data-ttu-id="fdd13-249">Threads LocksAnd do .NET CLR\# de threads físicos atuais</span><span class="sxs-lookup"><span data-stu-id="fdd13-249">.NET CLR LocksAnd Threads\# of current physical Threads</span></span>

<a id="otherresources"></a>

## <a name="other-resources"></a><span data-ttu-id="fdd13-250">Outros recursos</span><span class="sxs-lookup"><span data-stu-id="fdd13-250">Other Resources</span></span>

<span data-ttu-id="fdd13-251">Para obter mais informações sobre o monitoramento e o ajuste do desempenho do ASP.NET, consulte os seguintes tópicos:</span><span class="sxs-lookup"><span data-stu-id="fdd13-251">For more information on ASP.NET performance monitoring and tuning, see the following topics:</span></span>

- <span data-ttu-id="fdd13-252">[Visão geral do desempenho do ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span><span class="sxs-lookup"><span data-stu-id="fdd13-252">[ASP.NET Performance Overview](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)</span></span>
- [<span data-ttu-id="fdd13-253">Uso de thread ASP.NET no IIS 7,5, IIS 7,0 e IIS 6,0</span><span class="sxs-lookup"><span data-stu-id="fdd13-253">ASP.NET Thread Usage on IIS 7.5, IIS 7.0, and IIS 6.0</span></span>](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [<span data-ttu-id="fdd13-254">&lt;elemento de&gt; applicationPool (configurações da Web)</span><span class="sxs-lookup"><span data-stu-id="fdd13-254">&lt;applicationPool&gt; Element (Web Settings)</span></span>](https://msdn.microsoft.com/library/dd560842.aspx)
