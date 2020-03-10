---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Trabalhando com grupos no Signalr | Microsoft Docs
author: bradygaster
description: Este tópico descreve como manter informações de associação de grupo com a API de Hub.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 46dd952fc4902b37c981a111dfa344dad79bb668
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558579"
---
# <a name="working-with-groups-in-signalr"></a><span data-ttu-id="ef2e8-103">Trabalhar com grupos no SignalR</span><span class="sxs-lookup"><span data-stu-id="ef2e8-103">Working with Groups in SignalR</span></span>

<span data-ttu-id="ef2e8-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ef2e8-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="ef2e8-105">Este tópico descreve como adicionar usuários a grupos e manter informações de associação de grupo.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-105">This topic describes how to add users to groups and persist group membership information.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="ef2e8-106">Versões de software usadas neste tópico</span><span class="sxs-lookup"><span data-stu-id="ef2e8-106">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="ef2e8-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="ef2e8-107">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="ef2e8-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="ef2e8-108">.NET 4.5</span></span>
> - <span data-ttu-id="ef2e8-109">Sinalização versão 2</span><span class="sxs-lookup"><span data-stu-id="ef2e8-109">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="ef2e8-110">Versões anteriores deste tópico</span><span class="sxs-lookup"><span data-stu-id="ef2e8-110">Previous versions of this topic</span></span>
>
> <span data-ttu-id="ef2e8-111">Para obter informações sobre versões anteriores do Signalr, confira [versões mais antigas do signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="ef2e8-111">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="ef2e8-112">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="ef2e8-112">Questions and comments</span></span>
>
> <span data-ttu-id="ef2e8-113">Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="ef2e8-114">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="ef2e8-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="ef2e8-115">Visão geral</span><span class="sxs-lookup"><span data-stu-id="ef2e8-115">Overview</span></span>

<span data-ttu-id="ef2e8-116">Os grupos no Signalr fornecem um método para transmitir mensagens para os subconjuntos especificados de clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-116">Groups in SignalR provide a method for broadcasting messages to specified subsets of connected clients.</span></span> <span data-ttu-id="ef2e8-117">Um grupo pode ter qualquer número de clientes, e um cliente pode ser um membro de qualquer número de grupos.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-117">A group can have any number of clients, and a client can be a member of any number of groups.</span></span> <span data-ttu-id="ef2e8-118">Você não precisa criar grupos explicitamente.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-118">You don't have to explicitly create groups.</span></span> <span data-ttu-id="ef2e8-119">Na verdade, um grupo é criado automaticamente na primeira vez que você especifica seu nome em uma chamada para groups. Add, e é excluído quando você remove a última conexão da associação.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-119">In effect, a group is automatically created the first time you specify its name in a call to Groups.Add, and it is deleted when you remove the last connection from membership in it.</span></span> <span data-ttu-id="ef2e8-120">Para obter uma introdução ao uso de grupos, consulte [como gerenciar a associação de grupo da classe Hub](hubs-api-guide-server.md#groupsfromhub) no guia da API de hubs-servidor.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-120">For an introduction to using groups, see [How to manage group membership from the Hub class](hubs-api-guide-server.md#groupsfromhub) in the Hubs API - Server Guide.</span></span>

<span data-ttu-id="ef2e8-121">Não há API para obter uma lista de associação de grupo ou uma lista de grupos.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-121">There is no API for getting a group membership list or a list of groups.</span></span> <span data-ttu-id="ef2e8-122">O signalr envia mensagens para clientes e grupos com base em um modelo pub/sub e o servidor não mantém listas de grupos ou associações de grupo.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-122">SignalR sends messages to clients and groups based on a pub/sub model, and the server does not maintain lists of groups or group memberships.</span></span> <span data-ttu-id="ef2e8-123">Isso ajuda a maximizar a escalabilidade, porque sempre que você adiciona um nó a um web farm, qualquer Estado que o Signalr mantém deve ser propagado para o novo nó.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-123">This helps maximize scalability, because whenever you add a node to a web farm, any state that SignalR maintains has to be propagated to the new node.</span></span>

<span data-ttu-id="ef2e8-124">Quando você adiciona um usuário a um grupo usando o método `Groups.Add`, o usuário recebe mensagens direcionadas para esse grupo durante a conexão atual, mas a associação do usuário nesse grupo não é mantida além da conexão atual.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-124">When you add a user to a group using the `Groups.Add` method, the user receives messages directed to that group for the duration of the current connection, but the user's membership in that group is not persisted beyond the current connection.</span></span> <span data-ttu-id="ef2e8-125">Se você quiser reter permanentemente informações sobre grupos e Associação de grupo, deverá armazenar esses dados em um repositório, como um banco de dado ou armazenamento de tabelas do Azure.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-125">If you want to permanently retain information about groups and group membership, you must store that data in a repository such as a database or Azure table storage.</span></span> <span data-ttu-id="ef2e8-126">Em seguida, cada vez que um usuário se conecta ao seu aplicativo, você recupera do repositório a que pertence o usuário e adiciona manualmente esse usuário a esses grupos.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-126">Then, each time a user connects to your application, you retrieve from the repository which groups the user belongs to, and manually add that user to those groups.</span></span>

<span data-ttu-id="ef2e8-127">Ao se reconectar após uma interrupção temporária, o usuário automaticamente reingressa os grupos atribuídos anteriormente.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-127">When reconnecting after a temporary disruption, the user automatically re-joins the previously-assigned groups.</span></span> <span data-ttu-id="ef2e8-128">A rejunção automática de um grupo só se aplica ao se reconectar, não ao estabelecer uma nova conexão.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-128">Automatically rejoining a group only applies when reconnecting, not when establishing a new connection.</span></span> <span data-ttu-id="ef2e8-129">Um token assinado digitalmente é passado do cliente que contém a lista de grupos atribuídos anteriormente.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-129">A digitally-signed token is passed from the client that contains the list of previously-assigned groups.</span></span> <span data-ttu-id="ef2e8-130">Se você quiser verificar se o usuário pertence aos grupos solicitados, você pode substituir o comportamento padrão.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-130">If you want to verify whether the user belongs to the requested groups, you can override the default behavior.</span></span>

<span data-ttu-id="ef2e8-131">Este tópico inclui as seções a seguir:</span><span class="sxs-lookup"><span data-stu-id="ef2e8-131">This topic includes the following sections:</span></span>

- [<span data-ttu-id="ef2e8-132">Adicionando e removendo usuários</span><span class="sxs-lookup"><span data-stu-id="ef2e8-132">Adding and removing users</span></span>](#add)
- [<span data-ttu-id="ef2e8-133">Chamando membros de um grupo</span><span class="sxs-lookup"><span data-stu-id="ef2e8-133">Calling members of a group</span></span>](#call)
- [<span data-ttu-id="ef2e8-134">Armazenando a associação de grupo em um banco de dados</span><span class="sxs-lookup"><span data-stu-id="ef2e8-134">Storing group membership in a database</span></span>](#storedatabase)
- [<span data-ttu-id="ef2e8-135">Armazenando a associação de grupo no armazenamento de tabelas do Azure</span><span class="sxs-lookup"><span data-stu-id="ef2e8-135">Storing group membership in Azure table storage</span></span>](#storeazuretable)
- [<span data-ttu-id="ef2e8-136">Verificando a associação de grupo ao reconectar</span><span class="sxs-lookup"><span data-stu-id="ef2e8-136">Verifying group membership when reconnecting</span></span>](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a><span data-ttu-id="ef2e8-137">Adicionando e removendo usuários</span><span class="sxs-lookup"><span data-stu-id="ef2e8-137">Adding and removing users</span></span>

<span data-ttu-id="ef2e8-138">Para adicionar ou remover usuários de um grupo, você chama os métodos [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) ou [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) e passa o nome do grupo e a ID de conexão do usuário como parâmetros.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-138">To add or remove users from a group, you call the [Add](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) or [Remove](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) methods, and pass in the user's connection id and group's name as parameters.</span></span> <span data-ttu-id="ef2e8-139">Você não precisa remover manualmente um usuário de um grupo quando a conexão termina.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-139">You do not need to manually remove a user from a group when the connection ends.</span></span>

<span data-ttu-id="ef2e8-140">O exemplo a seguir mostra os métodos `Groups.Add` e `Groups.Remove` usados em métodos de Hub.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-140">The following example shows the `Groups.Add` and `Groups.Remove` methods used in Hub methods.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

<span data-ttu-id="ef2e8-141">Os métodos `Groups.Add` e `Groups.Remove` são executados de forma assíncrona.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-141">The `Groups.Add` and `Groups.Remove` methods execute asynchronously.</span></span>

<span data-ttu-id="ef2e8-142">Se você quiser adicionar um cliente a um grupo e enviar imediatamente uma mensagem para o cliente usando o grupo, será necessário certificar-se de que o método groups. Add termine primeiro.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-142">If you want to add a client to a group and immediately send a message to the client by using the group, you have to make sure that the Groups.Add method finishes first.</span></span> <span data-ttu-id="ef2e8-143">Os exemplos de código a seguir mostram como fazer isso.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-143">The following code examples show how to do that.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

<span data-ttu-id="ef2e8-144">Em geral, você não deve incluir `await` ao chamar o método `Groups.Remove` porque a ID de conexão que você está tentando remover pode não estar mais disponível.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-144">In general, you should not include `await` when calling the `Groups.Remove` method because the connection id that you are trying to remove might no longer be available.</span></span> <span data-ttu-id="ef2e8-145">Nesse caso, `TaskCanceledException` é lançada depois que a solicitação atinge o tempo limite. Se seu aplicativo precisar garantir que o usuário tenha sido removido do grupo antes de enviar uma mensagem para o grupo, você poderá adicionar `await` antes de `Groups.Remove`e, em seguida, capturar a exceção de `TaskCanceledException` que pode ser gerada.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-145">In that case, `TaskCanceledException` is thrown after the request times out. If your application must ensure that the user has been removed from the group before sending a message to the group, you can add `await` before `Groups.Remove`, and then catch the `TaskCanceledException` exception that might be thrown.</span></span>

<a id="call"></a>

## <a name="calling-members-of-a-group"></a><span data-ttu-id="ef2e8-146">Chamando membros de um grupo</span><span class="sxs-lookup"><span data-stu-id="ef2e8-146">Calling members of a group</span></span>

<span data-ttu-id="ef2e8-147">Você pode enviar mensagens para todos os membros de um grupo ou apenas membros especificados do grupo, conforme mostrado nos exemplos a seguir.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-147">You can send messages to all of the members of a group or only specified members of the group, as shown in the following examples.</span></span>

- <span data-ttu-id="ef2e8-148">**Todos** os clientes conectados em um grupo especificado.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-148">**All** connected clients in a specified group.</span></span>

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- <span data-ttu-id="ef2e8-149">Todos os clientes conectados em um grupo especificado, **exceto os clientes especificados**, identificados pela ID de conexão.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-149">All connected clients in a specified group **except the specified clients**, identified by connection ID.</span></span>

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- <span data-ttu-id="ef2e8-150">Todos os clientes conectados em um grupo especificado **, exceto o cliente de chamada**.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-150">All connected clients in a specified group **except the calling client**.</span></span>

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a><span data-ttu-id="ef2e8-151">Armazenando a associação de grupo em um banco de dados</span><span class="sxs-lookup"><span data-stu-id="ef2e8-151">Storing group membership in a database</span></span>

<span data-ttu-id="ef2e8-152">Os exemplos a seguir mostram como manter informações de grupo e de usuário em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-152">The following examples show how to retain group and user information in a database.</span></span> <span data-ttu-id="ef2e8-153">Você pode usar qualquer tecnologia de acesso a dados; no entanto, o exemplo a seguir mostra como definir modelos usando Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-153">You can use any data access technology; however, the example below shows how to define models using Entity Framework.</span></span> <span data-ttu-id="ef2e8-154">Esses modelos de entidade correspondem a tabelas e campos de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-154">These entity models correspond to database tables and fields.</span></span> <span data-ttu-id="ef2e8-155">Sua estrutura de dados pode variar consideravelmente dependendo dos requisitos do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-155">Your data structure could vary considerably depending on the requirements of your application.</span></span> <span data-ttu-id="ef2e8-156">Este exemplo inclui uma classe chamada `ConversationRoom` que seria exclusiva para um aplicativo que permite aos usuários ingressar em conversas sobre assuntos diferentes, como esportes ou jardinagem.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-156">This example includes a class named `ConversationRoom` which would be unique to an application that enables users to join conversations about different subjects, such as sports or gardening.</span></span> <span data-ttu-id="ef2e8-157">Este exemplo também inclui uma classe para as conexões.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-157">This example also includes a class for the connections.</span></span> <span data-ttu-id="ef2e8-158">A classe de conexão não é absolutamente necessária para controlar a associação de grupo, mas geralmente faz parte da solução robusta para controlar os usuários.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-158">The connection class is not absolutely required for tracking group membership but is frequently part of robust solution to tracking users.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

<span data-ttu-id="ef2e8-159">Em seguida, no Hub, você pode recuperar as informações do grupo e do usuário do banco de dados e adicionar manualmente o usuário aos grupos apropriados.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-159">Then, in the hub, you can retrieve the group and user information from the database and manually add the user to the appropriate groups.</span></span> <span data-ttu-id="ef2e8-160">O exemplo não inclui código para controlar as conexões de usuário.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-160">The example does not include code for tracking the user connections.</span></span> <span data-ttu-id="ef2e8-161">Neste exemplo, a palavra-chave `await` não é aplicada antes `Groups.Add` porque uma mensagem não é enviada imediatamente aos membros do grupo.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-161">In this example, the `await` keyword is not applied before `Groups.Add` because a message is not immediately sent to members of the group.</span></span> <span data-ttu-id="ef2e8-162">Se você quiser enviar uma mensagem a todos os membros do grupo imediatamente após adicionar o novo membro, convém aplicar a palavra-chave `await` para certificar-se de que a operação assíncrona foi concluída.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-162">If you want to send a message to all members of the group immediately after adding the new member, you would want to apply the `await` keyword to make sure the asynchronous operation has completed.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a><span data-ttu-id="ef2e8-163">Armazenando a associação de grupo no armazenamento de tabelas do Azure</span><span class="sxs-lookup"><span data-stu-id="ef2e8-163">Storing group membership in Azure table storage</span></span>

<span data-ttu-id="ef2e8-164">Usar o armazenamento de tabelas do Azure para armazenar informações de grupo e de usuário é semelhante ao uso de um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-164">Using Azure table storage to store group and user information is similar to using a database.</span></span> <span data-ttu-id="ef2e8-165">O exemplo a seguir mostra uma entidade de tabela que armazena o nome de usuário e o nome do grupo.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-165">The following example shows a table entity that stores the user name and group name.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<span data-ttu-id="ef2e8-166">No Hub, você recupera os grupos atribuídos quando o usuário se conecta.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-166">In the hub, you retrieve the assigned groups when the user connects.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a><span data-ttu-id="ef2e8-167">Verificando a associação de grupo ao reconectar</span><span class="sxs-lookup"><span data-stu-id="ef2e8-167">Verifying group membership when reconnecting</span></span>

<span data-ttu-id="ef2e8-168">Por padrão, o Signalr automaticamente atribui novamente um usuário aos grupos apropriados ao se reconectar de uma interrupção temporária, como quando uma conexão é descartada e restabelecida antes que a conexão expire. As informações de grupo do usuário são passadas em um token durante a reconexão e esse token é verificado no servidor.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-168">By default, SignalR automatically re-assigns a user to the appropriate groups when reconnecting from a temporary disruption, such as when a connection is dropped and re-established before the connection times out. The user's group information is passed in a token when reconnecting, and that token is verified on the server.</span></span> <span data-ttu-id="ef2e8-169">Para obter informações sobre o processo de verificação para a rejunção de usuários a grupos, consulte [rejunção de grupos ao reconectar](../security/introduction-to-security.md#rejoingroup).</span><span class="sxs-lookup"><span data-stu-id="ef2e8-169">For information about the verification process for rejoining users to groups, see [Rejoining groups when reconnecting](../security/introduction-to-security.md#rejoingroup).</span></span>

<span data-ttu-id="ef2e8-170">Em geral, você deve usar o comportamento padrão de reassociar automaticamente os grupos na reconexão.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-170">In general, you should use the default behavior of automatically rejoining groups on reconnect.</span></span> <span data-ttu-id="ef2e8-171">Os grupos de signalr não se destinam a um mecanismo de segurança para restringir o acesso a dados confidenciais.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-171">SignalR groups are not intended as a security mechanism for restricting access to sensitive data.</span></span> <span data-ttu-id="ef2e8-172">No entanto, se seu aplicativo precisar verificar a associação de grupo de um usuário ao reconectar-se, você poderá substituir o comportamento padrão.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-172">However, if your application must double-check a user's group membership when reconnecting, you can override the default behavior.</span></span> <span data-ttu-id="ef2e8-173">Alterar o comportamento padrão pode adicionar uma carga ao seu banco de dados porque a associação de grupo de um usuário deve ser recuperada para cada reconexão em vez de apenas quando o usuário se conecta.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-173">Changing the default behavior can add a burden to your database because a user's group membership must be retrieved for each reconnection rather than just when the user connects.</span></span>

<span data-ttu-id="ef2e8-174">Se você precisar verificar a associação de grupo ao reconectar, crie um novo módulo de pipeline de Hub que retorna uma lista de grupos atribuídos, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-174">If you must verify group membership on reconnect, create a new hub pipeline module that returns a list of assigned groups, as shown below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<span data-ttu-id="ef2e8-175">Em seguida, adicione esse módulo ao pipeline de Hub, conforme realçado abaixo.</span><span class="sxs-lookup"><span data-stu-id="ef2e8-175">Then, add that module to the hub pipeline, as highlighted below.</span></span>

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
