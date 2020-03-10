---
uid: signalr/overview/older-versions/troubleshooting
title: Solução de problemas de sinalização (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: Este artigo descreve problemas comuns com o desenvolvimento de aplicativos Signalr.
ms.author: bradyg
ms.date: 06/05/2013
ms.assetid: 347210ba-c452-4feb-886f-b51d89f58971
msc.legacyurl: /signalr/overview/older-versions/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 3dbf091ac2daa4da477b405852bb4d1316584fcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579600"
---
# <a name="signalr-troubleshooting-signalr-1x"></a><span data-ttu-id="11340-103">Solução de problemas do SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="11340-103">SignalR Troubleshooting (SignalR 1.x)</span></span>

<span data-ttu-id="11340-104">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="11340-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="11340-105">Este documento descreve problemas comuns de solução de problemas com o Signalr.</span><span class="sxs-lookup"><span data-stu-id="11340-105">This document describes common troubleshooting issues with SignalR.</span></span>

<span data-ttu-id="11340-106">Este documento contém as seções a seguir.</span><span class="sxs-lookup"><span data-stu-id="11340-106">This document contains the following sections.</span></span>

- [<span data-ttu-id="11340-107">A chamada de métodos entre o cliente e o servidor falha silenciosamente</span><span class="sxs-lookup"><span data-stu-id="11340-107">Calling methods between the client and server silently fails</span></span>](#connection)
- [<span data-ttu-id="11340-108">Outros problemas de conexão</span><span class="sxs-lookup"><span data-stu-id="11340-108">Other connection issues</span></span>](#other)
- [<span data-ttu-id="11340-109">Compilação e erros do lado do servidor</span><span class="sxs-lookup"><span data-stu-id="11340-109">Compilation and server-side errors</span></span>](#server)
- [<span data-ttu-id="11340-110">Problemas do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="11340-110">Visual Studio issues</span></span>](#vs)
- [<span data-ttu-id="11340-111">Problemas de Serviços de Informações da Internet</span><span class="sxs-lookup"><span data-stu-id="11340-111">Internet Information Services issues</span></span>](#iis)
- [<span data-ttu-id="11340-112">Problemas do Azure</span><span class="sxs-lookup"><span data-stu-id="11340-112">Azure issues</span></span>](#azure)

<a id="connection"></a>

## <a name="calling-methods-between-the-client-and-server-silently-fails"></a><span data-ttu-id="11340-113">A chamada de métodos entre o cliente e o servidor falha silenciosamente</span><span class="sxs-lookup"><span data-stu-id="11340-113">Calling methods between the client and server silently fails</span></span>

<span data-ttu-id="11340-114">Esta seção descreve as possíveis causas de uma chamada de método entre o cliente e o servidor falhar sem uma mensagem de erro significativa.</span><span class="sxs-lookup"><span data-stu-id="11340-114">This section describes possible causes for a method call between client and server to fail without a meaningful error message.</span></span> <span data-ttu-id="11340-115">Em um aplicativo Signalr, o servidor não tem informações sobre os métodos que o cliente implementa; Quando o servidor invoca um método de cliente, o nome do método e os dados do parâmetro são enviados ao cliente, e o método é executado somente se ele existir no formato especificado pelo servidor.</span><span class="sxs-lookup"><span data-stu-id="11340-115">In a SignalR application, the server has no information about the methods that the client implements; when the server invokes a client method, the method name and parameter data are sent to the client, and the method is executed only if it exists in the format that the server specified.</span></span> <span data-ttu-id="11340-116">Se nenhum método correspondente for encontrado no cliente, nada acontecerá e nenhuma mensagem de erro será gerada no servidor.</span><span class="sxs-lookup"><span data-stu-id="11340-116">If no matching method is found on the client, nothing happens, and no error message is raised on the server.</span></span>

<span data-ttu-id="11340-117">Para investigar melhor os métodos de cliente que não estão sendo chamados, você pode ativar o registro em log antes de chamar o método Start no Hub para ver quais chamadas são provenientes do servidor.</span><span class="sxs-lookup"><span data-stu-id="11340-117">To further investigate client methods not getting called, you can turn on logging before the calling the start method on the hub to see what calls are coming from the server.</span></span> <span data-ttu-id="11340-118">Para habilitar o log em um aplicativo JavaScript, consulte [como habilitar o log do lado do cliente (versão do cliente JavaScript)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging).</span><span class="sxs-lookup"><span data-stu-id="11340-118">To enable logging in a JavaScript application, see [How to enable client-side logging (JavaScript client version)](../guide-to-the-api/hubs-api-guide-javascript-client.md#logging).</span></span> <span data-ttu-id="11340-119">Para habilitar o log em um aplicativo cliente .NET, consulte [como habilitar o log do lado do cliente (versão do cliente .net)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).</span><span class="sxs-lookup"><span data-stu-id="11340-119">To enable logging in a .NET client application, see [How to enable client-side logging (.NET Client version)](../guide-to-the-api/hubs-api-guide-net-client.md#logging).</span></span>

### <a name="misspelled-method-incorrect-method-signature-or-incorrect-hub-name"></a><span data-ttu-id="11340-120">Método digitado incorretamente, assinatura de método incorreta ou nome de Hub incorreto</span><span class="sxs-lookup"><span data-stu-id="11340-120">Misspelled method, incorrect method signature, or incorrect hub name</span></span>

<span data-ttu-id="11340-121">Se o nome ou a assinatura de um método chamado não corresponder exatamente a um método apropriado no cliente, a chamada falhará.</span><span class="sxs-lookup"><span data-stu-id="11340-121">If the name or signature of a called method does not exactly match an appropriate method on the client, the call will fail.</span></span> <span data-ttu-id="11340-122">Verifique se o nome do método chamado pelo servidor corresponde ao nome do método no cliente.</span><span class="sxs-lookup"><span data-stu-id="11340-122">Verify that the method name called by the server matches the name of the method on the client.</span></span> <span data-ttu-id="11340-123">Além disso, o Signalr cria o proxy de Hub usando métodos camel-case, como é apropriado no JavaScript, de modo que um método chamado `SendMessage` no servidor seria chamado `sendMessage` no proxy do cliente.</span><span class="sxs-lookup"><span data-stu-id="11340-123">Also, SignalR creates the hub proxy using camel-cased methods, as is appropriate in JavaScript, so a method called `SendMessage` on the server would be called `sendMessage` in the client proxy.</span></span> <span data-ttu-id="11340-124">Se você usar o atributo `HubName` no código do servidor, verifique se o nome usado corresponde ao nome usado para criar o Hub no cliente.</span><span class="sxs-lookup"><span data-stu-id="11340-124">If you use the `HubName` attribute in your server-side code, verify that the name used matches the name used to create the hub on the client.</span></span> <span data-ttu-id="11340-125">Se você não usar o atributo `HubName`, verifique se o nome do Hub em um cliente JavaScript é camel-case, como chatHub em vez de ChatHub.</span><span class="sxs-lookup"><span data-stu-id="11340-125">If you do not use the `HubName` attribute, verify that the name of the hub in a JavaScript client is camel-cased, such as chatHub instead of ChatHub.</span></span>

### <a name="duplicate-method-name-on-client"></a><span data-ttu-id="11340-126">Nome de método duplicado no cliente</span><span class="sxs-lookup"><span data-stu-id="11340-126">Duplicate method name on client</span></span>

<span data-ttu-id="11340-127">Verifique se você não tem um método duplicado no cliente que difere somente por maiúsculas e minúsculas.</span><span class="sxs-lookup"><span data-stu-id="11340-127">Verify that you do not have a duplicate method on the client that differs only by case.</span></span> <span data-ttu-id="11340-128">Se o aplicativo cliente tiver um método chamado `sendMessage`, verifique se também não há um método chamado `SendMessage`.</span><span class="sxs-lookup"><span data-stu-id="11340-128">If your client application has a method called `sendMessage`, verify that there isn't also a method called `SendMessage` as well.</span></span>

### <a name="missing-json-parser-on-the-client"></a><span data-ttu-id="11340-129">Analisador JSON ausente no cliente</span><span class="sxs-lookup"><span data-stu-id="11340-129">Missing JSON parser on the client</span></span>

<span data-ttu-id="11340-130">O signalr requer que um analisador JSON esteja presente para serializar chamadas entre o servidor e o cliente.</span><span class="sxs-lookup"><span data-stu-id="11340-130">SignalR requires a JSON parser to be present to serialize calls between the server and the client.</span></span> <span data-ttu-id="11340-131">Se o cliente não tiver um analisador JSON interno (como o Internet Explorer 7), você precisará incluir um em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="11340-131">If your client doesn't have a built-in JSON parser (such as Internet Explorer 7), you'll need to include one in your application.</span></span> <span data-ttu-id="11340-132">Você pode baixar o analisador JSON [aqui](http://nuget.org/packages/json2).</span><span class="sxs-lookup"><span data-stu-id="11340-132">You can download the JSON parser [here](http://nuget.org/packages/json2).</span></span>

### <a name="mixing-hub-and-persistentconnection-syntax"></a><span data-ttu-id="11340-133">Mistura de Hub e sintaxe PersistentConnection</span><span class="sxs-lookup"><span data-stu-id="11340-133">Mixing Hub and PersistentConnection syntax</span></span>

<span data-ttu-id="11340-134">O signalr usa dois modelos de comunicação: hubs e PersistentConnections.</span><span class="sxs-lookup"><span data-stu-id="11340-134">SignalR uses two communication models: Hubs and PersistentConnections.</span></span> <span data-ttu-id="11340-135">A sintaxe para chamar esses dois modelos de comunicação é diferente no código do cliente.</span><span class="sxs-lookup"><span data-stu-id="11340-135">The syntax for calling these two communication models is different in the client code.</span></span> <span data-ttu-id="11340-136">Se você tiver adicionado um Hub em seu código de servidor, verifique se todo o seu código de cliente usa a sintaxe de Hub apropriada.</span><span class="sxs-lookup"><span data-stu-id="11340-136">If you have added a hub in your server code, verify that all of your client code uses the proper hub syntax.</span></span>

<span data-ttu-id="11340-137">**Código de cliente JavaScript que cria um PersistentConnection em um cliente JavaScript**</span><span class="sxs-lookup"><span data-stu-id="11340-137">**JavaScript client code that creates a PersistentConnection in a JavaScript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample1.js)]

<span data-ttu-id="11340-138">**Código de cliente JavaScript que cria um proxy de Hub em um cliente JavaScript**</span><span class="sxs-lookup"><span data-stu-id="11340-138">**JavaScript client code that creates a Hub Proxy in a Javascript client**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample2.js)]

<span data-ttu-id="11340-139">**C#código de servidor que mapeia uma rota para um PersistentConnection**</span><span class="sxs-lookup"><span data-stu-id="11340-139">**C# server code that maps a route to a PersistentConnection**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample3.cs)]

<span data-ttu-id="11340-140">**C#código de servidor que mapeia uma rota para um Hub ou para vários hubs se você tiver vários aplicativos**</span><span class="sxs-lookup"><span data-stu-id="11340-140">**C# server code that maps a route to a Hub, or to multiple hubs if you have multiple applications**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample4.cs)]

### <a name="connection-started-before-subscriptions-are-added"></a><span data-ttu-id="11340-141">Conexão iniciada antes da adição de assinaturas</span><span class="sxs-lookup"><span data-stu-id="11340-141">Connection started before subscriptions are added</span></span>

<span data-ttu-id="11340-142">Se a conexão do Hub for iniciada antes que os métodos que podem ser chamados do servidor sejam adicionados ao proxy, as mensagens não serão recebidas.</span><span class="sxs-lookup"><span data-stu-id="11340-142">If the Hub's connection is started before methods that can be called from the server are added to the proxy, messages will not be received.</span></span> <span data-ttu-id="11340-143">O código JavaScript a seguir não iniciará o Hub corretamente:</span><span class="sxs-lookup"><span data-stu-id="11340-143">The following JavaScript code will not start the hub properly:</span></span>

<span data-ttu-id="11340-144">**Código de cliente JavaScript incorreto que não permitirá que as mensagens de hubs sejam recebidas**</span><span class="sxs-lookup"><span data-stu-id="11340-144">**Incorrect JavaScript client code that will not allow Hubs messages to be received**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample5.js)]

<span data-ttu-id="11340-145">Em vez disso, adicione as assinaturas do método antes de chamar Start:</span><span class="sxs-lookup"><span data-stu-id="11340-145">Instead, add the method subscriptions before calling Start:</span></span>

<span data-ttu-id="11340-146">**Código de cliente JavaScript que adiciona corretamente assinaturas a um hub**</span><span class="sxs-lookup"><span data-stu-id="11340-146">**JavaScript client code that correctly adds subscriptions to a hub**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample6.js)]

### <a name="missing-method-name-on-the-hub-proxy"></a><span data-ttu-id="11340-147">Nome de método ausente no proxy de Hub</span><span class="sxs-lookup"><span data-stu-id="11340-147">Missing method name on the hub proxy</span></span>

<span data-ttu-id="11340-148">Verifique se o método definido no servidor está inscrito no cliente.</span><span class="sxs-lookup"><span data-stu-id="11340-148">Verify that the method defined on the server is subscribed to on the client.</span></span> <span data-ttu-id="11340-149">Embora o servidor defina o método, ele ainda deve ser adicionado ao proxy do cliente.</span><span class="sxs-lookup"><span data-stu-id="11340-149">Even though the server defines the method, it must still be added to the client proxy.</span></span> <span data-ttu-id="11340-150">Os métodos podem ser adicionados ao proxy do cliente das seguintes maneiras (Observe que o método é adicionado ao membro `client` do Hub, e não ao Hub diretamente):</span><span class="sxs-lookup"><span data-stu-id="11340-150">Methods can be added to the client proxy in the following ways (Note that the method is added to the `client` member of the hub, not the hub directly):</span></span>

<span data-ttu-id="11340-151">**Código de cliente JavaScript que adiciona métodos a um proxy de Hub**</span><span class="sxs-lookup"><span data-stu-id="11340-151">**JavaScript client code that adds methods to a hub proxy**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample7.js)]

### <a name="hub-or-hub-methods-not-declared-as-public"></a><span data-ttu-id="11340-152">Métodos de Hub ou Hub não declarados como públicos</span><span class="sxs-lookup"><span data-stu-id="11340-152">Hub or hub methods not declared as Public</span></span>

<span data-ttu-id="11340-153">Para estar visível no cliente, a implementação e os métodos do Hub devem ser declarados como `public`.</span><span class="sxs-lookup"><span data-stu-id="11340-153">To be visible on the client, the hub implementation and methods must be declared as `public`.</span></span>

### <a name="accessing-hub-from-a-different-application"></a><span data-ttu-id="11340-154">Acessando o Hub de um aplicativo diferente</span><span class="sxs-lookup"><span data-stu-id="11340-154">Accessing hub from a different application</span></span>

<span data-ttu-id="11340-155">Os hubs de sinalização só podem ser acessados por meio de aplicativos que implementam clientes do Signalr.</span><span class="sxs-lookup"><span data-stu-id="11340-155">SignalR Hubs can only be accessed through applications that implement SignalR clients.</span></span> <span data-ttu-id="11340-156">O signalr não pode interoperar com outras bibliotecas de comunicação (como SOAP ou WCF Web Services.) Se não houver nenhum cliente Signalr disponível para sua plataforma de destino, você não poderá acessar o ponto de extremidade do servidor diretamente.</span><span class="sxs-lookup"><span data-stu-id="11340-156">SignalR cannot interoperate with other communication libraries (like SOAP or WCF web services.) If there is no SignalR client available for your target platform, you cannot access the server's endpoint directly.</span></span>

### <a name="manually-serializing-data"></a><span data-ttu-id="11340-157">Serializando dados manualmente</span><span class="sxs-lookup"><span data-stu-id="11340-157">Manually serializing data</span></span>

<span data-ttu-id="11340-158">O signalr usará automaticamente o JSON para serializar os parâmetros do método – não é necessário fazê-lo por conta própria.</span><span class="sxs-lookup"><span data-stu-id="11340-158">SignalR will automatically use JSON to serialize your method parameters- there's no need to do it yourself.</span></span>

### <a name="remote-hub-method-not-executed-on-client-in-ondisconnected-function"></a><span data-ttu-id="11340-159">Método de Hub remoto não executado no cliente na função ondisconnectd</span><span class="sxs-lookup"><span data-stu-id="11340-159">Remote Hub method not executed on client in OnDisconnected function</span></span>

<span data-ttu-id="11340-160">Este comportamento ocorre por design.</span><span class="sxs-lookup"><span data-stu-id="11340-160">This behavior is by design.</span></span> <span data-ttu-id="11340-161">Quando `OnDisconnected` é chamado, o Hub já entrou no estado de `Disconnected`, o que não permite que mais métodos de Hub sejam chamados.</span><span class="sxs-lookup"><span data-stu-id="11340-161">When `OnDisconnected` is called, the hub has already entered the `Disconnected` state, which does not allow further hub methods to be called.</span></span>

<span data-ttu-id="11340-162">**C#código do servidor que executa corretamente o código no evento OnDisconnected**</span><span class="sxs-lookup"><span data-stu-id="11340-162">**C# server code that correctly executes code in the OnDisconnected event**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample8.cs)]

### <a name="connection-limit-reached"></a><span data-ttu-id="11340-163">Limite de conexão atingido</span><span class="sxs-lookup"><span data-stu-id="11340-163">Connection limit reached</span></span>

<span data-ttu-id="11340-164">Ao usar a versão completa do IIS em um sistema operacional cliente como o Windows 7, um limite de 10 conexões é imposto.</span><span class="sxs-lookup"><span data-stu-id="11340-164">When using the full version of IIS on a client operating system like Windows 7, a 10-connection limit is imposed.</span></span> <span data-ttu-id="11340-165">Ao usar um sistema operacional cliente, use IIS Express em vez disso para evitar esse limite.</span><span class="sxs-lookup"><span data-stu-id="11340-165">When using a client OS, use IIS Express instead to avoid this limit.</span></span>

### <a name="cross-domain-connection-not-set-up-properly"></a><span data-ttu-id="11340-166">Conexão entre domínios não configurada corretamente</span><span class="sxs-lookup"><span data-stu-id="11340-166">Cross-domain connection not set up properly</span></span>

<span data-ttu-id="11340-167">Se uma conexão entre domínios (uma conexão para a qual a URL de sinalização não estiver no mesmo domínio que a página de hospedagem) não estiver configurada corretamente, a conexão poderá falhar sem uma mensagem de erro.</span><span class="sxs-lookup"><span data-stu-id="11340-167">If a cross-domain connection (a connection for which the SignalR URL is not in the same domain as the hosting page) is not set up correctly, the connection may fail without an error message.</span></span> <span data-ttu-id="11340-168">Para obter informações sobre como habilitar a comunicação entre domínios, consulte [como estabelecer uma conexão entre domínios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="11340-168">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span>

### <a name="connection-using-ntlm-active-directory-not-working-in-net-client"></a><span data-ttu-id="11340-169">Conexão usando NTLM (Active Directory) não está funcionando no cliente .NET</span><span class="sxs-lookup"><span data-stu-id="11340-169">Connection using NTLM (Active Directory) not working in .NET client</span></span>

<span data-ttu-id="11340-170">Uma conexão em um aplicativo cliente .NET que usa segurança de domínio poderá falhar se a conexão não estiver configurada corretamente.</span><span class="sxs-lookup"><span data-stu-id="11340-170">A connection in a .NET client application that uses Domain security may fail if the connection is not configured properly.</span></span> <span data-ttu-id="11340-171">Para usar o Signalr em um ambiente de domínio, defina a propriedade de conexão requisito da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="11340-171">To use SignalR in a domain environment, set the requisite connection property as follows:</span></span>

<span data-ttu-id="11340-172">**C#código do cliente que implementa as credenciais de conexão**</span><span class="sxs-lookup"><span data-stu-id="11340-172">**C# client code that implements connection credentials**</span></span>

[!code-csharp[Main](troubleshooting/samples/sample9.cs)]

<a id="other"></a>

## <a name="other-connection-issues"></a><span data-ttu-id="11340-173">Outros problemas de conexão</span><span class="sxs-lookup"><span data-stu-id="11340-173">Other connection issues</span></span>

<span data-ttu-id="11340-174">Esta seção descreve as causas e soluções para sintomas específicos ou mensagens de erro que ocorrem durante uma conexão.</span><span class="sxs-lookup"><span data-stu-id="11340-174">This section describes the causes and solutions for specific symptoms or error messages that occur during a connection.</span></span>

### <a name="start-must-be-called-before-data-can-be-sent-error"></a><span data-ttu-id="11340-175">Erro "o início deve ser chamado antes que os dados possam ser enviados"</span><span class="sxs-lookup"><span data-stu-id="11340-175">"Start must be called before data can be sent" error</span></span>

<span data-ttu-id="11340-176">Esse erro geralmente é visto se o código faz referência a objetos de sinalização antes da conexão ser iniciada.</span><span class="sxs-lookup"><span data-stu-id="11340-176">This error is commonly seen if code references SignalR objects before the connection is started.</span></span> <span data-ttu-id="11340-177">O wireup para manipuladores e semelhantes que chamarão os métodos definidos no servidor deve ser adicionado após a conclusão da conexão.</span><span class="sxs-lookup"><span data-stu-id="11340-177">The wireup for handlers and the like that will call methods defined on the server must be added after the connection completes.</span></span> <span data-ttu-id="11340-178">Observe que a chamada para `Start` é assíncrona, portanto, o código após a chamada pode ser executado antes de ser concluído.</span><span class="sxs-lookup"><span data-stu-id="11340-178">Note that the call to `Start` is asynchronous, so code after the call may be executed before it completes.</span></span> <span data-ttu-id="11340-179">A melhor maneira de adicionar manipuladores depois que uma conexão é iniciada por completo é colocá-los em uma função de retorno de chamada que é passada como um parâmetro para o método Start:</span><span class="sxs-lookup"><span data-stu-id="11340-179">The best way to add handlers after a connection starts completely is to put them into a callback function that is passed as a parameter to the start method:</span></span>

<span data-ttu-id="11340-180">**Código de cliente JavaScript que adiciona corretamente manipuladores de eventos que referenciam objetos de sinalização**</span><span class="sxs-lookup"><span data-stu-id="11340-180">**JavaScript client code that correctly adds event handlers that reference SignalR objects**</span></span>

[!code-javascript[Main](troubleshooting/samples/sample10.js?highlight=1)]

<span data-ttu-id="11340-181">Esse erro também será visto se uma conexão for interrompida enquanto os objetos de sinalização ainda estão sendo referenciados.</span><span class="sxs-lookup"><span data-stu-id="11340-181">This error will also be seen if a connection stops while SignalR objects are still being referenced.</span></span>

### <a name="301-moved-permanently-or-302-moved-temporarily-error"></a><span data-ttu-id="11340-182">erro "301 movido permanentemente" ou "302 movido temporariamente"</span><span class="sxs-lookup"><span data-stu-id="11340-182">"301 Moved Permanently" or "302 Moved Temporarily" error</span></span>

<span data-ttu-id="11340-183">Esse erro pode ser visto se o projeto contiver uma pasta chamada Signalr, que irá interferir no proxy criado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="11340-183">This error may be seen if the project contains a folder called SignalR, which will interfere with the automatically-created proxy.</span></span> <span data-ttu-id="11340-184">Para evitar esse erro, não use uma pasta chamada `SignalR` em seu aplicativo ou desative a geração automática de proxy.</span><span class="sxs-lookup"><span data-stu-id="11340-184">To avoid this error, do not use a folder called `SignalR` in your application, or turn automatic proxy generation off.</span></span> <span data-ttu-id="11340-185">Consulte [o proxy gerado e o que ele faz para você](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) para obter mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="11340-185">See [The Generated Proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy) for more details.</span></span>

### <a name="403-forbidden-error-in-net-or-silverlight-client"></a><span data-ttu-id="11340-186">erro "403 Proibido" no cliente .NET ou Silverlight</span><span class="sxs-lookup"><span data-stu-id="11340-186">"403 Forbidden" error in .NET or Silverlight client</span></span>

<span data-ttu-id="11340-187">Esse erro pode ocorrer em ambientes de domínio cruzado em que a comunicação entre domínios não está habilitada corretamente.</span><span class="sxs-lookup"><span data-stu-id="11340-187">This error may occur in cross-domain environments where cross-domain communication is not properly enabled.</span></span> <span data-ttu-id="11340-188">Para obter informações sobre como habilitar a comunicação entre domínios, consulte [como estabelecer uma conexão entre domínios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span><span class="sxs-lookup"><span data-stu-id="11340-188">For information on how to enable cross-domain communication, see [How to establish a cross-domain connection](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain).</span></span> <span data-ttu-id="11340-189">Para estabelecer uma conexão entre domínios em um cliente Silverlight, consulte [conexões entre domínios de clientes do Silverlight](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).</span><span class="sxs-lookup"><span data-stu-id="11340-189">To establish a cross-domain connection in a Silverlight client, see [Cross-domain connections from Silverlight clients](../guide-to-the-api/hubs-api-guide-net-client.md#slcrossdomain).</span></span>

### <a name="404-not-found-error"></a><span data-ttu-id="11340-190">erro "404 não encontrado"</span><span class="sxs-lookup"><span data-stu-id="11340-190">"404 Not Found" error</span></span>

<span data-ttu-id="11340-191">Há várias causas para esse problema.</span><span class="sxs-lookup"><span data-stu-id="11340-191">There are several causes for this issue.</span></span> <span data-ttu-id="11340-192">Verifique todos os itens a seguir:</span><span class="sxs-lookup"><span data-stu-id="11340-192">Verify all of the following:</span></span>

- <span data-ttu-id="11340-193">**Referência de endereço de proxy de Hub não formatada corretamente:** Esse erro é normalmente visto se a referência ao endereço de proxy de Hub gerado não estiver formatada corretamente.</span><span class="sxs-lookup"><span data-stu-id="11340-193">**Hub proxy address reference not formatted correctly:** This error is commonly seen if the reference to the generated hub proxy address is not formatted correctly.</span></span> <span data-ttu-id="11340-194">Verifique se a referência ao endereço do Hub foi feita corretamente.</span><span class="sxs-lookup"><span data-stu-id="11340-194">Verify that the reference to the hub address is made properly.</span></span> <span data-ttu-id="11340-195">Consulte [como referenciar o proxy gerado dinamicamente](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="11340-195">See [How to reference the dynamically generated proxy](../guide-to-the-api/hubs-api-guide-javascript-client.md#dynamicproxy) for details.</span></span>
- <span data-ttu-id="11340-196">**Adicionando rotas ao aplicativo antes de adicionar a rota do Hub:** Se seu aplicativo usar outras rotas, verifique se a primeira rota adicionada é a chamada para `MapHubs`.</span><span class="sxs-lookup"><span data-stu-id="11340-196">**Adding routes to application before adding the hub route:** If your application uses other routes, verify that the first route added is the call to `MapHubs`.</span></span>

### <a name="500-internal-server-error"></a><span data-ttu-id="11340-197">"erro interno do servidor 500"</span><span class="sxs-lookup"><span data-stu-id="11340-197">"500 Internal Server Error"</span></span>

<span data-ttu-id="11340-198">Esse é um erro muito genérico que pode ter uma ampla variedade de causas.</span><span class="sxs-lookup"><span data-stu-id="11340-198">This is a very generic error that could have a wide variety of causes.</span></span> <span data-ttu-id="11340-199">Os detalhes do erro devem aparecer no log de eventos do servidor ou podem ser encontrados por meio da depuração do servidor.</span><span class="sxs-lookup"><span data-stu-id="11340-199">The details of the error should appear in the server's event log, or can be found through debugging the server.</span></span> <span data-ttu-id="11340-200">Informações de erro mais detalhadas podem ser obtidas ativando erros detalhados no servidor.</span><span class="sxs-lookup"><span data-stu-id="11340-200">More detailed error information may be obtained by turning on detailed errors on the server.</span></span> <span data-ttu-id="11340-201">Para obter mais informações, consulte [como tratar erros na classe Hub](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).</span><span class="sxs-lookup"><span data-stu-id="11340-201">For more information, see [How to handle errors in the Hub class](../guide-to-the-api/hubs-api-guide-server.md#handleErrors).</span></span>

### <a name="typeerror-lthubtypegt-is-undefined-error"></a><span data-ttu-id="11340-202">Erro "TypeError: &lt;hubType&gt; indefinido"</span><span class="sxs-lookup"><span data-stu-id="11340-202">"TypeError: &lt;hubType&gt; is undefined" error</span></span>

<span data-ttu-id="11340-203">Esse erro será resultado se a chamada para `MapHubs` não for feita corretamente.</span><span class="sxs-lookup"><span data-stu-id="11340-203">This error will result if the call to `MapHubs` is not made properly.</span></span> <span data-ttu-id="11340-204">Consulte [como registrar a rota do signalr e configurar as opções do signalr](../guide-to-the-api/hubs-api-guide-server.md#route) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="11340-204">See [How to register the SignalR route and configure SignalR options](../guide-to-the-api/hubs-api-guide-server.md#route) for more information.</span></span>

### <a name="jsonserializationexception-was-unhandled-by-user-code"></a><span data-ttu-id="11340-205">JsonSerializationException não foi tratado pelo código do usuário</span><span class="sxs-lookup"><span data-stu-id="11340-205">JsonSerializationException was unhandled by user code</span></span>

<span data-ttu-id="11340-206">Verifique se os parâmetros que você envia para seus métodos não incluem tipos não serializáveis (como identificadores de arquivo ou conexões de banco de dados).</span><span class="sxs-lookup"><span data-stu-id="11340-206">Verify that the parameters you send to your methods do not include non-serializable types (like file handles or database connections).</span></span> <span data-ttu-id="11340-207">Se você precisar usar membros em um objeto do lado do servidor que não deseja que sejam enviados ao cliente (para segurança ou por motivos de serialização), use o atributo `JSONIgnore`.</span><span class="sxs-lookup"><span data-stu-id="11340-207">If you need to use members on a server-side object that you don't want to be sent to the client (either for security or for reasons of serialization), use the `JSONIgnore` attribute.</span></span>

### <a name="protocol-error-unknown-transport-error"></a><span data-ttu-id="11340-208">Erro de "erro de protocolo: transporte desconhecido"</span><span class="sxs-lookup"><span data-stu-id="11340-208">"Protocol error: Unknown transport" error</span></span>

<span data-ttu-id="11340-209">Esse erro pode ocorrer se o cliente não oferecer suporte aos transportes que o Signalr usa.</span><span class="sxs-lookup"><span data-stu-id="11340-209">This error may occur if the client does not support the transports that SignalR uses.</span></span> <span data-ttu-id="11340-210">Consulte [transportes e fallbacks](../getting-started/introduction-to-signalr.md#transports) para obter informações sobre quais navegadores podem ser usados com o signalr.</span><span class="sxs-lookup"><span data-stu-id="11340-210">See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for information on which browsers can be used with SignalR.</span></span>

### <a name="javascript-hub-proxy-generation-has-been-disabled"></a><span data-ttu-id="11340-211">"A geração de proxy de Hub JavaScript foi desabilitada".</span><span class="sxs-lookup"><span data-stu-id="11340-211">"JavaScript Hub proxy generation has been disabled."</span></span>

<span data-ttu-id="11340-212">Esse erro ocorrerá se `DisableJavaScriptProxies` estiver definido, incluindo também uma referência ao proxy gerado dinamicamente em `signalr/hubs`.</span><span class="sxs-lookup"><span data-stu-id="11340-212">This error will occur if `DisableJavaScriptProxies` is set while also including a reference to the dynamically generated proxy at `signalr/hubs`.</span></span> <span data-ttu-id="11340-213">Para obter mais informações sobre como criar o proxy manualmente, consulte [o proxy gerado e o que ele faz para você](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).</span><span class="sxs-lookup"><span data-stu-id="11340-213">For more information on creating the proxy manually, see [The generated proxy and what it does for you](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy).</span></span>

### <a name="the-connection-id-is-in-the-incorrect-format-or-the-user-identity-cannot-change-during-an-active-signalr-connection-error"></a><span data-ttu-id="11340-214">"O ID da conexão está no formato incorreto" ou "a identidade do usuário não pode ser alterada durante um erro de conexão de sinal ativo"</span><span class="sxs-lookup"><span data-stu-id="11340-214">"The connection ID is in the incorrect format" or "The user identity cannot change during an active SignalR connection" error</span></span>

<span data-ttu-id="11340-215">Esse erro pode ser visto se a autenticação estiver sendo usada e o cliente for desconectado antes de a conexão ser interrompida.</span><span class="sxs-lookup"><span data-stu-id="11340-215">This error may be seen if authentication is being used, and the client is logged out before the connection is stopped.</span></span> <span data-ttu-id="11340-216">A solução é interromper a conexão do Signalr antes de registrar o cliente.</span><span class="sxs-lookup"><span data-stu-id="11340-216">The solution is to stop the SignalR connection before logging the client out.</span></span>

### <a name="uncaught-error-signalr-jquery-not-found-please-ensure-jquery-is-referenced-before-the-signalrjs-file-error"></a><span data-ttu-id="11340-217">"Erro não detectado: Signalr: jQuery não encontrado.</span><span class="sxs-lookup"><span data-stu-id="11340-217">"Uncaught Error: SignalR: jQuery not found.</span></span> <span data-ttu-id="11340-218">Verifique se o jQuery é referenciado antes do arquivo Signalr. js "</span><span class="sxs-lookup"><span data-stu-id="11340-218">Please ensure jQuery is referenced before the SignalR.js file" error</span></span>

<span data-ttu-id="11340-219">O cliente do Signalr JavaScript requer que o jQuery seja executado.</span><span class="sxs-lookup"><span data-stu-id="11340-219">The SignalR JavaScript client requires jQuery to run.</span></span> <span data-ttu-id="11340-220">Verifique se a referência ao jQuery está correta, se o caminho usado é válido e se a referência ao jQuery é anterior à referência ao Signalr.</span><span class="sxs-lookup"><span data-stu-id="11340-220">Verify that your reference to jQuery is correct, that the path used is valid, and that the reference to jQuery is before the reference to SignalR.</span></span>

### <a name="uncaught-typeerror-cannot-read-property-ltpropertygt-of-undefined-error"></a><span data-ttu-id="11340-221">"Uncapturoud TypeError: não é possível ler a propriedade '&lt;Property&gt;' of undefined" Error</span><span class="sxs-lookup"><span data-stu-id="11340-221">"Uncaught TypeError: Cannot read property '&lt;property&gt;' of undefined" error</span></span>

<span data-ttu-id="11340-222">Esse erro resulta de não ter jQuery ou o proxy de hubs referenciado corretamente.</span><span class="sxs-lookup"><span data-stu-id="11340-222">This error results from not having jQuery or the hubs proxy referenced properly.</span></span> <span data-ttu-id="11340-223">Verifique se a referência ao jQuery e ao proxy de hubs está correta, se o caminho usado é válido e se a referência ao jQuery é anterior à referência ao proxy de hubs.</span><span class="sxs-lookup"><span data-stu-id="11340-223">Verify that your reference to jQuery and the hubs proxy is correct, that the path used is valid, and that the reference to jQuery is before the reference to the hubs proxy.</span></span> <span data-ttu-id="11340-224">A referência padrão ao proxy de hubs deve ser semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="11340-224">The default reference to the hubs proxy should look like the following:</span></span>

<span data-ttu-id="11340-225">**Código HTML do lado do cliente que referencia corretamente o proxy de hubs**</span><span class="sxs-lookup"><span data-stu-id="11340-225">**HTML client-side code that correctly references the Hubs proxy**</span></span>

[!code-html[Main](troubleshooting/samples/sample11.html)]

### <a name="runtimebinderexception-was-unhandled-by-user-code-error"></a><span data-ttu-id="11340-226">Erro "RuntimeBinderException não foi tratado pelo código do usuário"</span><span class="sxs-lookup"><span data-stu-id="11340-226">"RuntimeBinderException was unhandled by user code" error</span></span>

<span data-ttu-id="11340-227">Esse erro pode ocorrer quando a sobrecarga incorreta de `Hub.On` é usada.</span><span class="sxs-lookup"><span data-stu-id="11340-227">This error may occur when the incorrect overload of `Hub.On` is used.</span></span> <span data-ttu-id="11340-228">Se o método tiver um valor de retorno, o tipo de retorno deverá ser especificado como um parâmetro de tipo genérico:</span><span class="sxs-lookup"><span data-stu-id="11340-228">If the method has a return value, the return type must be specified as a generic type parameter:</span></span>

<span data-ttu-id="11340-229">**Método definido no cliente (sem proxy gerado)**</span><span class="sxs-lookup"><span data-stu-id="11340-229">**Method defined on the client (without generated proxy)**</span></span>

[!code-html[Main](troubleshooting/samples/sample12.html?highlight=1)]

### <a name="connection-id-is-inconsistent-or-connection-breaks-between-page-loads"></a><span data-ttu-id="11340-230">A ID de conexão é inconsistente ou quebras de conexão entre cargas de página</span><span class="sxs-lookup"><span data-stu-id="11340-230">Connection ID is inconsistent or connection breaks between page loads</span></span>

<span data-ttu-id="11340-231">Este comportamento ocorre por design.</span><span class="sxs-lookup"><span data-stu-id="11340-231">This behavior is by design.</span></span> <span data-ttu-id="11340-232">Como o objeto Hub é hospedado no objeto Page, o Hub é destruído quando a página é atualizada.</span><span class="sxs-lookup"><span data-stu-id="11340-232">Since the hub object is hosted in the page object, the hub is destroyed when the page refreshes.</span></span> <span data-ttu-id="11340-233">Um aplicativo de várias páginas precisa manter a associação entre os usuários e as IDs de conexão para que eles sejam consistentes entre os carregamentos de página.</span><span class="sxs-lookup"><span data-stu-id="11340-233">A multi-page application needs to maintain the association between users and connection IDs so that they will be consistent between page loads.</span></span> <span data-ttu-id="11340-234">As IDs de conexão podem ser armazenadas no servidor em um objeto `ConcurrentDictionary` ou em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="11340-234">The connection IDs can be stored on the server in either a `ConcurrentDictionary` object or a database.</span></span>

### <a name="value-cannot-be-null-error"></a><span data-ttu-id="11340-235">Erro "o valor não pode ser nulo"</span><span class="sxs-lookup"><span data-stu-id="11340-235">"Value cannot be null" error</span></span>

<span data-ttu-id="11340-236">Os métodos do lado do servidor com parâmetros opcionais não têm suporte no momento; Se o parâmetro opcional for omitido, o método falhará.</span><span class="sxs-lookup"><span data-stu-id="11340-236">Server-side methods with optional parameters are not currently supported; if the optional parameter is omitted, the method will fail.</span></span> <span data-ttu-id="11340-237">Para obter mais informações, consulte [parâmetros opcionais](https://github.com/SignalR/SignalR/issues/324).</span><span class="sxs-lookup"><span data-stu-id="11340-237">For more information, see [Optional Parameters](https://github.com/SignalR/SignalR/issues/324).</span></span>

### <a name="firefox-cant-establish-a-connection-to-the-server-at-ltaddressgt-error-in-firebug"></a><span data-ttu-id="11340-238">Erro "o Firefox não pode estabelecer uma conexão com o servidor no endereço &lt;&gt;" no Firebug</span><span class="sxs-lookup"><span data-stu-id="11340-238">"Firefox can't establish a connection to the server at &lt;address&gt;" error in Firebug</span></span>

<span data-ttu-id="11340-239">Essa mensagem de erro pode ser vista no Firebug se a negociação do transporte do WebSocket falhar e outro transporte for usado em seu lugar.</span><span class="sxs-lookup"><span data-stu-id="11340-239">This error message can be seen in Firebug if negotiation of the WebSocket transport fails and another transport is used instead.</span></span> <span data-ttu-id="11340-240">Este comportamento ocorre por design.</span><span class="sxs-lookup"><span data-stu-id="11340-240">This behavior is by design.</span></span>

### <a name="the-remote-certificate-is-invalid-according-to-the-validation-procedure-error-in-net-client-application"></a><span data-ttu-id="11340-241">Erro "o certificado remoto é inválido de acordo com o procedimento de validação" no aplicativo cliente .NET</span><span class="sxs-lookup"><span data-stu-id="11340-241">"The remote certificate is invalid according to the validation procedure" error in .NET client application</span></span>

<span data-ttu-id="11340-242">Se o servidor exigir certificados de cliente personalizados, você poderá adicionar um X509Certificate à conexão antes que a solicitação seja feita.</span><span class="sxs-lookup"><span data-stu-id="11340-242">If your server requires custom client certificates, then you can add an x509certificate to the connection before the request is made.</span></span> <span data-ttu-id="11340-243">Adicione o certificado à conexão usando `Connection.AddClientCertificate`.</span><span class="sxs-lookup"><span data-stu-id="11340-243">Add the certificate to the connection using `Connection.AddClientCertificate`.</span></span>

### <a name="connection-drops-after-authentication-times-out"></a><span data-ttu-id="11340-244">A conexão cai após a autenticação expirar</span><span class="sxs-lookup"><span data-stu-id="11340-244">Connection drops after authentication times out</span></span>

<span data-ttu-id="11340-245">Este comportamento ocorre por design.</span><span class="sxs-lookup"><span data-stu-id="11340-245">This behavior is by design.</span></span> <span data-ttu-id="11340-246">As credenciais de autenticação não podem ser modificadas enquanto uma conexão estiver ativa; para atualizar as credenciais, a conexão deve ser interrompida e reiniciada.</span><span class="sxs-lookup"><span data-stu-id="11340-246">Authentication credentials cannot be modified while a connection is active; to refresh credentials, the connection must be stopped and restarted.</span></span>

### <a name="onconnected-gets-called-twice-when-using-jquery-mobile"></a><span data-ttu-id="11340-247">Onconnected é chamado duas vezes ao usar o jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="11340-247">OnConnected gets called twice when using jQuery Mobile</span></span>

<span data-ttu-id="11340-248">a função `initializePage` do jQuery Mobile força os scripts em cada página a ser executada novamente, criando uma segunda conexão.</span><span class="sxs-lookup"><span data-stu-id="11340-248">jQuery Mobile's `initializePage` function forces the scripts in each page to be re-executed, thus creating a second connection.</span></span> <span data-ttu-id="11340-249">As soluções para esse problema incluem:</span><span class="sxs-lookup"><span data-stu-id="11340-249">Solutions for this issue include:</span></span>

- <span data-ttu-id="11340-250">Inclua a referência ao jQuery Mobile antes do arquivo JavaScript.</span><span class="sxs-lookup"><span data-stu-id="11340-250">Include the reference to jQuery Mobile before your JavaScript file.</span></span>
- <span data-ttu-id="11340-251">Desabilite a função `initializePage` definindo `$.mobile.autoInitializePage = false`.</span><span class="sxs-lookup"><span data-stu-id="11340-251">Disable the `initializePage` function by setting `$.mobile.autoInitializePage = false`.</span></span>
- <span data-ttu-id="11340-252">Aguarde até que a página termine a inicialização antes de iniciar a conexão.</span><span class="sxs-lookup"><span data-stu-id="11340-252">Wait for the page to finish initializing before starting the connection.</span></span>

### <a name="messages-are-delayed-in-silverlight-applications-using-server-sent-events"></a><span data-ttu-id="11340-253">As mensagens são atrasadas em aplicativos Silverlight usando eventos enviados pelo servidor</span><span class="sxs-lookup"><span data-stu-id="11340-253">Messages are delayed in Silverlight applications using Server Sent Events</span></span>

<span data-ttu-id="11340-254">As mensagens são atrasadas ao usar eventos de servidor enviados no Silverlight.</span><span class="sxs-lookup"><span data-stu-id="11340-254">Messages are delayed when using server sent events on Silverlight.</span></span> <span data-ttu-id="11340-255">Para forçar o uso da sondagem longa, use o seguinte ao iniciar a conexão:</span><span class="sxs-lookup"><span data-stu-id="11340-255">To force long polling to be used instead, use the following when starting the connection:</span></span>

[!code-css[Main](troubleshooting/samples/sample13.css)]

### <a name="permission-denied-using-forever-frame-protocol"></a><span data-ttu-id="11340-256">"Permissão negada" usando o protocolo de quadro contínuo</span><span class="sxs-lookup"><span data-stu-id="11340-256">"Permission Denied" using Forever Frame protocol</span></span>

<span data-ttu-id="11340-257">Esse é um problema conhecido, descrito [aqui](https://github.com/SignalR/SignalR/issues/1963).</span><span class="sxs-lookup"><span data-stu-id="11340-257">This is a known issue, described [here](https://github.com/SignalR/SignalR/issues/1963).</span></span> <span data-ttu-id="11340-258">Esse sintoma pode ser visto usando a biblioteca JQuery mais recente; a solução alternativa é fazer downgrade de seu aplicativo para JQuery 1.8.2.</span><span class="sxs-lookup"><span data-stu-id="11340-258">This symptom may be seen using the latest JQuery library; the workaround is to downgrade your application to JQuery 1.8.2.</span></span>

<a id="server"></a>

## <a name="compilation-and-server-side-errors"></a><span data-ttu-id="11340-259">Compilação e erros do lado do servidor</span><span class="sxs-lookup"><span data-stu-id="11340-259">Compilation and server-side errors</span></span>

 <span data-ttu-id="11340-260">A seção a seguir contém possíveis soluções para erros de tempo de execução do compilador e do servidor.</span><span class="sxs-lookup"><span data-stu-id="11340-260">The following section contains possible solutions to compiler and server-side runtime errors.</span></span> 

### <a name="reference-to-hub-instance-is-null"></a><span data-ttu-id="11340-261">A referência à instância de Hub é nula</span><span class="sxs-lookup"><span data-stu-id="11340-261">Reference to Hub instance is null</span></span>

<span data-ttu-id="11340-262">Como uma instância de Hub é criada para cada conexão, você mesmo não pode criar uma instância de um Hub no seu código.</span><span class="sxs-lookup"><span data-stu-id="11340-262">Since a hub instance is created for each connection, you can't create an instance of a hub in your code yourself.</span></span> <span data-ttu-id="11340-263">Para chamar métodos em um hub de fora do próprio Hub, consulte [como chamar métodos de cliente e gerenciar grupos de fora da classe Hub](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) para saber como obter uma referência ao contexto do Hub.</span><span class="sxs-lookup"><span data-stu-id="11340-263">To call methods on a hub from outside the hub itself, see [How to call client methods and manage groups from outside the Hub class](../guide-to-the-api/hubs-api-guide-server.md#callfromoutsidehub) for how to obtain a reference to the hub context.</span></span>

### <a name="httpcontextcurrentsession-is-null"></a><span data-ttu-id="11340-264">HTTPContext. Current. Session é nulo</span><span class="sxs-lookup"><span data-stu-id="11340-264">HTTPContext.Current.Session is null</span></span>

<span data-ttu-id="11340-265">Este comportamento ocorre por design.</span><span class="sxs-lookup"><span data-stu-id="11340-265">This behavior is by design.</span></span> <span data-ttu-id="11340-266">O signalr não dá suporte ao estado de sessão ASP.NET, pois habilitar o estado de sessão quebraria mensagens duplex.</span><span class="sxs-lookup"><span data-stu-id="11340-266">SignalR does not support the ASP.NET session state, since enabling the session state would break duplex messaging.</span></span>

### <a name="no-suitable-method-to-override"></a><span data-ttu-id="11340-267">Nenhum método adequado para substituir</span><span class="sxs-lookup"><span data-stu-id="11340-267">No suitable method to override</span></span>

<span data-ttu-id="11340-268">Você poderá ver esse erro se estiver usando código de documentação ou Blogs mais antigos.</span><span class="sxs-lookup"><span data-stu-id="11340-268">You may see this error if you are using code from older documentation or blogs.</span></span> <span data-ttu-id="11340-269">Verifique se você não está referenciando nomes de métodos que foram alterados ou preteridos (como `OnConnectedAsync`).</span><span class="sxs-lookup"><span data-stu-id="11340-269">Verify that you are not referencing names of methods that have been changed or deprecated (like `OnConnectedAsync`).</span></span>

### <a name="hostcontextextensionswebsocketserverurl-is-null"></a><span data-ttu-id="11340-270">HostContextExtensions. WebSocketServerUrl é nulo</span><span class="sxs-lookup"><span data-stu-id="11340-270">HostContextExtensions.WebSocketServerUrl is null</span></span>

<span data-ttu-id="11340-271">Este comportamento ocorre por design.</span><span class="sxs-lookup"><span data-stu-id="11340-271">This behavior is by design.</span></span> <span data-ttu-id="11340-272">Este membro foi preterido e não deve ser usado.</span><span class="sxs-lookup"><span data-stu-id="11340-272">This member is deprecated and should not be used.</span></span>

### <a name="a-route-named-signalrhubs-is-already-in-the-route-collection-error"></a><span data-ttu-id="11340-273">"Uma rota chamada ' signalr. hubs ' já está no erro de coleção de rotas"</span><span class="sxs-lookup"><span data-stu-id="11340-273">"A route named 'signalr.hubs' is already in the route collection" error</span></span>

<span data-ttu-id="11340-274">Esse erro será visto se `MapHubs` for chamado duas vezes pelo seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="11340-274">This error will be seen if `MapHubs` is called twice by your application.</span></span> <span data-ttu-id="11340-275">Alguns aplicativos de exemplo chamam `MapHubs` diretamente no arquivo de aplicativo global; outros fazem a chamada em uma classe wrapper.</span><span class="sxs-lookup"><span data-stu-id="11340-275">Some example applications call `MapHubs` directly in the global application file; others make the call in a wrapper class.</span></span> <span data-ttu-id="11340-276">Certifique-se de que seu aplicativo não faça ambos.</span><span class="sxs-lookup"><span data-stu-id="11340-276">Ensure that your application does not do both.</span></span>

<a id="vs"></a>

## <a name="visual-studio-issues"></a><span data-ttu-id="11340-277">Problemas do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="11340-277">Visual Studio issues</span></span>

<span data-ttu-id="11340-278">Esta seção descreve os problemas encontrados no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="11340-278">This section describes issues encountered in Visual Studio.</span></span>

### <a name="script-documents-node-does-not-appear-in-solution-explorer"></a><span data-ttu-id="11340-279">O nó de documentos de script não aparece no Gerenciador de Soluções</span><span class="sxs-lookup"><span data-stu-id="11340-279">Script Documents node does not appear in Solution Explorer</span></span>

<span data-ttu-id="11340-280">Alguns dos nossos tutoriais direcionam você ao nó "documentos de script" no Gerenciador de Soluções durante a depuração.</span><span class="sxs-lookup"><span data-stu-id="11340-280">Some of our tutorials direct you to the "Script Documents" node in Solution Explorer while debugging.</span></span> <span data-ttu-id="11340-281">Esse nó é produzido pelo depurador do JavaScript e só aparecerá durante a depuração de clientes do navegador no Internet Explorer; o nó não será exibido se o Chrome ou Firefox forem usados.</span><span class="sxs-lookup"><span data-stu-id="11340-281">This node is produced by the JavaScript debugger, and will only appear while debugging browser clients in Internet Explorer; the node will not appear if Chrome or Firefox are used.</span></span> <span data-ttu-id="11340-282">O depurador do JavaScript também não será executado se outro depurador de cliente estiver em execução, como o depurador do Silverlight.</span><span class="sxs-lookup"><span data-stu-id="11340-282">The JavaScript debugger will also not run if another client debugger is running, such as the Silverlight debugger.</span></span>

### <a name="signalr-does-not-work-on-visual-studio-2008-or-earlier"></a><span data-ttu-id="11340-283">O signalr não funciona no Visual Studio 2008 ou anterior</span><span class="sxs-lookup"><span data-stu-id="11340-283">SignalR does not work on Visual Studio 2008 or earlier</span></span>

<span data-ttu-id="11340-284">Este comportamento ocorre por design.</span><span class="sxs-lookup"><span data-stu-id="11340-284">This behavior is by design.</span></span> <span data-ttu-id="11340-285">O signalr requer .NET Framework 4 ou posterior; Isso exige que os aplicativos Signalr sejam desenvolvidos no Visual Studio 2010 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="11340-285">SignalR requires .NET Framework 4 or later; this requires that SignalR applications be developed in Visual Studio 2010 or later.</span></span>

<a id="iis"></a>

## <a name="iis-issues"></a><span data-ttu-id="11340-286">Problemas do IIS</span><span class="sxs-lookup"><span data-stu-id="11340-286">IIS issues</span></span>

<span data-ttu-id="11340-287">Esta seção contém problemas com o Serviços de Informações da Internet.</span><span class="sxs-lookup"><span data-stu-id="11340-287">This section contains issues with Internet Information Services.</span></span>

### <a name="web-site-crashes-after-maphubs-call"></a><span data-ttu-id="11340-288">O site falha após a chamada MapHubs</span><span class="sxs-lookup"><span data-stu-id="11340-288">Web site crashes after MapHubs call</span></span>

<span data-ttu-id="11340-289">Esse problema foi corrigido na versão mais recente do Signalr.</span><span class="sxs-lookup"><span data-stu-id="11340-289">This issue has been fixed in the latest version of SignalR.</span></span> <span data-ttu-id="11340-290">Verifique se você está usando a versão mais recente do Signalr, atualizando sua instalação usando o NuGet.</span><span class="sxs-lookup"><span data-stu-id="11340-290">Verify that you are using the latest released version of SignalR by updating your installation using NuGet.</span></span>

<a id="azure"></a>

## <a name="azure-issues"></a><span data-ttu-id="11340-291">Problemas do Azure</span><span class="sxs-lookup"><span data-stu-id="11340-291">Azure issues</span></span>

<span data-ttu-id="11340-292">Esta seção contém problemas com o Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="11340-292">This section contains issues with Microsoft Azure.</span></span>

### <a name="messages-are-not-received-through-the-azure-backplane-after-altering-topic-names"></a><span data-ttu-id="11340-293">As mensagens não são recebidas pelo backplane do Azure após a alteração dos nomes de tópico</span><span class="sxs-lookup"><span data-stu-id="11340-293">Messages are not received through the Azure backplane after altering topic names</span></span>

<span data-ttu-id="11340-294">Os tópicos usados pelo Azure backplane não se destinam a ser configuráveis pelo usuário.</span><span class="sxs-lookup"><span data-stu-id="11340-294">The topics used by the Azure backplane are not intended to be user-configurable.</span></span>
