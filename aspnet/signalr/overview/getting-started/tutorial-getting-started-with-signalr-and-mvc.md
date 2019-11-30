---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 'Tutorial: chat em tempo real com o Signalr 2 e o MVC 5 | Microsoft Docs'
author: bradygaster
description: Este tutorial mostra como usar o ASP.NET Signalr 2 para criar um aplicativo de chat em tempo real. Você adiciona o Signalr a um aplicativo MVC 5.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 5671e4f0123ca2b0cb5314336cf4411467feac70
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600479"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a><span data-ttu-id="ffe92-104">Tutorial: chat em tempo real com o Signalr 2 e o MVC 5</span><span class="sxs-lookup"><span data-stu-id="ffe92-104">Tutorial: Real-time chat with SignalR 2 and MVC 5</span></span>

<span data-ttu-id="ffe92-105">Este tutorial mostra como usar o ASP.NET Signalr 2 para criar um aplicativo de chat em tempo real.</span><span class="sxs-lookup"><span data-stu-id="ffe92-105">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="ffe92-106">Você adiciona o Signalr a um aplicativo MVC 5 e cria um modo de exibição de chat para enviar e exibir mensagens.</span><span class="sxs-lookup"><span data-stu-id="ffe92-106">You add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span>

<span data-ttu-id="ffe92-107">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="ffe92-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ffe92-108">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="ffe92-108">Set up the project</span></span>
> * <span data-ttu-id="ffe92-109">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="ffe92-109">Run the sample</span></span>
> * <span data-ttu-id="ffe92-110">Examinar o código</span><span class="sxs-lookup"><span data-stu-id="ffe92-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="ffe92-111">{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}</span><span class="sxs-lookup"><span data-stu-id="ffe92-111">Prerequisites</span></span>

* <span data-ttu-id="ffe92-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) com a ASP.net e a carga de trabalho de **desenvolvimento da Web** .</span><span class="sxs-lookup"><span data-stu-id="ffe92-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="ffe92-113">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="ffe92-113">Set up the Project</span></span>

<span data-ttu-id="ffe92-114">Esta seção mostra como usar o Visual Studio 2017 e o Signalr 2 para criar um aplicativo ASP.NET MVC 5 vazio, adicionar a biblioteca do Signalr e criar o aplicativo de chat.</span><span class="sxs-lookup"><span data-stu-id="ffe92-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="ffe92-115">No Visual Studio, crie um C# aplicativo ASP.NET que tenha como destino .NET Framework 4,5, nomeie-o SignalRChat e clique em OK.</span><span class="sxs-lookup"><span data-stu-id="ffe92-115">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![Criar Web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. <span data-ttu-id="ffe92-117">Em **novo aplicativo Web ASP.net-SignalRMvcChat**, selecione **MVC** e, em seguida, selecione **alterar autenticação**.</span><span class="sxs-lookup"><span data-stu-id="ffe92-117">In **New ASP.NET Web Application - SignalRMvcChat**, select **MVC** and then select **Change Authentication**.</span></span>

1. <span data-ttu-id="ffe92-118">Em **alterar autenticação**, selecione **sem autenticação** e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="ffe92-118">In **Change Authentication**, select **No Authentication** and click **OK**.</span></span>

    ![Não selecionar nenhuma autenticação](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. <span data-ttu-id="ffe92-120">Em **novo ASP.NET Web Application-SignalRMvcChat**, selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="ffe92-120">In **New ASP.NET Web Application - SignalRMvcChat**, select **OK**.</span></span>

1. <span data-ttu-id="ffe92-121">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **novo item**.</span><span class="sxs-lookup"><span data-stu-id="ffe92-121">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="ffe92-122">Em **Adicionar novo item-SignalRChat**, selecione **instalado** >  \*\*C# > \*\* **Web** > **signalr** e selecione **classe de Hub do signalr (v2)** .</span><span class="sxs-lookup"><span data-stu-id="ffe92-122">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="ffe92-123">Nomeie a classe *ChatHub* e adicione-a ao projeto.</span><span class="sxs-lookup"><span data-stu-id="ffe92-123">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="ffe92-124">Esta etapa cria o arquivo de classe *ChatHub.cs* e adiciona um conjunto de arquivos de script e referências de assembly que dão suporte a signalr ao projeto.</span><span class="sxs-lookup"><span data-stu-id="ffe92-124">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="ffe92-125">Substitua o código no novo arquivo da classe *ChatHub.cs* por este código:</span><span class="sxs-lookup"><span data-stu-id="ffe92-125">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. <span data-ttu-id="ffe92-126">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="ffe92-126">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="ffe92-127">Nomeie a nova classe *Startup* e adicione-a ao projeto.</span><span class="sxs-lookup"><span data-stu-id="ffe92-127">Name the new class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="ffe92-128">Substitua o código no arquivo da classe *Startup.cs* por este código:</span><span class="sxs-lookup"><span data-stu-id="ffe92-128">Replace the code in the *Startup.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. <span data-ttu-id="ffe92-129">Em **Gerenciador de soluções**, selecione **controladores** > **HomeController.cs**.</span><span class="sxs-lookup"><span data-stu-id="ffe92-129">In **Solution Explorer**, select **Controllers** > **HomeController.cs**.</span></span>

1. <span data-ttu-id="ffe92-130">Adicione esse método ao *HomeController.cs*.</span><span class="sxs-lookup"><span data-stu-id="ffe92-130">Add this method to the *HomeController.cs*.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    <span data-ttu-id="ffe92-131">Esse método retorna o modo de exibição de **chat** que você cria em uma etapa posterior.</span><span class="sxs-lookup"><span data-stu-id="ffe92-131">This method returns the **Chat** view that you create in a later step.</span></span>

1. <span data-ttu-id="ffe92-132">Em **Gerenciador de soluções**, clique com o botão direito do mouse em **exibições** > **página inicial**e selecione **Adicionar** >  **exibição**.</span><span class="sxs-lookup"><span data-stu-id="ffe92-132">In **Solution Explorer**, right-click **Views** > **Home**, and select **Add** >  **View**.</span></span>

1. <span data-ttu-id="ffe92-133">No **modo de exibição adicionar**, nomeie o novo modo de exibição de **chat** e selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="ffe92-133">In **Add View**, name the new view **Chat** and select **Add**.</span></span>

1. <span data-ttu-id="ffe92-134">Substitua o conteúdo de **chat. cshtml** por este código:</span><span class="sxs-lookup"><span data-stu-id="ffe92-134">Replace the contents of **Chat.cshtml** with this code:</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. <span data-ttu-id="ffe92-135">Em **Gerenciador de soluções**, expanda **scripts**.</span><span class="sxs-lookup"><span data-stu-id="ffe92-135">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="ffe92-136">As bibliotecas de script para jQuery e Signalr são visíveis no projeto.</span><span class="sxs-lookup"><span data-stu-id="ffe92-136">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ffe92-137">O Gerenciador de pacotes pode ter instalado uma versão mais recente dos scripts do Signalr.</span><span class="sxs-lookup"><span data-stu-id="ffe92-137">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="ffe92-138">Verifique se as referências de script no bloco de código correspondem às versões dos arquivos de script no projeto.</span><span class="sxs-lookup"><span data-stu-id="ffe92-138">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="ffe92-139">Referências de script do bloco de código original:</span><span class="sxs-lookup"><span data-stu-id="ffe92-139">Script references from the original code block:</span></span>

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. <span data-ttu-id="ffe92-140">Se eles não corresponderem, atualize o arquivo *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="ffe92-140">If they don't match, update the *.cshtml* file.</span></span>

1. <span data-ttu-id="ffe92-141">Na barra de menus, selecione **arquivo** > **salvar tudo**.</span><span class="sxs-lookup"><span data-stu-id="ffe92-141">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="ffe92-142">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="ffe92-142">Run the Sample</span></span>

1. <span data-ttu-id="ffe92-143">Na barra de ferramentas, ative a **depuração de script** e, em seguida, selecione o botão reproduzir para executar o exemplo no modo de depuração.</span><span class="sxs-lookup"><span data-stu-id="ffe92-143">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Inserir nome de usuário](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. <span data-ttu-id="ffe92-145">Quando o navegador for aberto, insira um nome para sua identidade de chat.</span><span class="sxs-lookup"><span data-stu-id="ffe92-145">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="ffe92-146">Copie a URL do navegador, abra dois outros navegadores e cole as URLs nas barras de endereço.</span><span class="sxs-lookup"><span data-stu-id="ffe92-146">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="ffe92-147">Em cada navegador, insira um nome exclusivo.</span><span class="sxs-lookup"><span data-stu-id="ffe92-147">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="ffe92-148">Agora, adicione um comentário e selecione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="ffe92-148">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="ffe92-149">Repita isso nos outros navegadores.</span><span class="sxs-lookup"><span data-stu-id="ffe92-149">Repeat that in the other browsers.</span></span> <span data-ttu-id="ffe92-150">Os comentários aparecem em tempo real.</span><span class="sxs-lookup"><span data-stu-id="ffe92-150">The comments appear in real time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ffe92-151">Esse aplicativo de chat simples não mantém o contexto de discussão no servidor.</span><span class="sxs-lookup"><span data-stu-id="ffe92-151">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="ffe92-152">O Hub transmite comentários para todos os usuários atuais.</span><span class="sxs-lookup"><span data-stu-id="ffe92-152">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="ffe92-153">Os usuários que ingressarem no chat mais tarde verão as mensagens adicionadas a partir do momento em que ingressarem.</span><span class="sxs-lookup"><span data-stu-id="ffe92-153">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="ffe92-154">Veja como o aplicativo de chat é executado em três navegadores diferentes.</span><span class="sxs-lookup"><span data-stu-id="ffe92-154">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="ffe92-155">Quando Tom, Anand e Susan enviam mensagens, todos os navegadores são atualizados em tempo real:</span><span class="sxs-lookup"><span data-stu-id="ffe92-155">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Todos os três navegadores exibem o mesmo histórico de chat](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. <span data-ttu-id="ffe92-157">Em **Gerenciador de soluções**, inspecione o nó **documentos de script** para o aplicativo em execução.</span><span class="sxs-lookup"><span data-stu-id="ffe92-157">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="ffe92-158">Há um arquivo de script chamado *hubs* que a biblioteca do signalr gera no tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="ffe92-158">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="ffe92-159">Esse arquivo gerencia a comunicação entre o script do jQuery e o código do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="ffe92-159">This file manages the communication between jQuery script and server-side code.</span></span>

    ![script de hubs gerados automaticamente no nó documentos de script](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="ffe92-161">Examinar o código</span><span class="sxs-lookup"><span data-stu-id="ffe92-161">Examine the Code</span></span>

<span data-ttu-id="ffe92-162">O aplicativo de chat do Signalr demonstra duas tarefas básicas de desenvolvimento do Signalr.</span><span class="sxs-lookup"><span data-stu-id="ffe92-162">The SignalR chat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="ffe92-163">Ele mostra como criar um Hub.</span><span class="sxs-lookup"><span data-stu-id="ffe92-163">It shows you how to create a hub.</span></span> <span data-ttu-id="ffe92-164">O servidor usa esse Hub como o objeto de coordenação principal.</span><span class="sxs-lookup"><span data-stu-id="ffe92-164">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="ffe92-165">O Hub usa a biblioteca do Signalr jQuery para enviar e receber mensagens.</span><span class="sxs-lookup"><span data-stu-id="ffe92-165">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="ffe92-166">Hubs de sinalização no ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="ffe92-166">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="ffe92-167">No exemplo de código, a classe `ChatHub` deriva da classe `Microsoft.AspNet.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="ffe92-167">In the code sample, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="ffe92-168">Derivar da classe `Hub` é uma maneira útil de criar um aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="ffe92-168">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="ffe92-169">Você pode criar métodos públicos em sua classe Hub e, em seguida, acessar esses métodos chamando-os de scripts em uma página da Web.</span><span class="sxs-lookup"><span data-stu-id="ffe92-169">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="ffe92-170">No código do chat, os clientes chamam o método `ChatHub.Send` para enviar uma nova mensagem.</span><span class="sxs-lookup"><span data-stu-id="ffe92-170">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="ffe92-171">O Hub, por sua vez, envia a mensagem a todos os clientes chamando `Clients.All.addNewMessageToPage`.</span><span class="sxs-lookup"><span data-stu-id="ffe92-171">The hub in turn sends the message to all clients by calling `Clients.All.addNewMessageToPage`.</span></span>

<span data-ttu-id="ffe92-172">O método `Send` demonstra vários conceitos de Hub:</span><span class="sxs-lookup"><span data-stu-id="ffe92-172">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="ffe92-173">Declare métodos públicos em um hub para que os clientes possam chamá-los.</span><span class="sxs-lookup"><span data-stu-id="ffe92-173">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="ffe92-174">Use a propriedade dinâmica `Microsoft.AspNet.SignalR.Hub.Clients` para se comunicar com todos os clientes conectados a esse Hub.</span><span class="sxs-lookup"><span data-stu-id="ffe92-174">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="ffe92-175">Chame uma função no cliente (como a função `addNewMessageToPage`) para atualizar clientes.</span><span class="sxs-lookup"><span data-stu-id="ffe92-175">Call a function on the client (like the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a><span data-ttu-id="ffe92-176">Signalr e jQuery chat. cshtml</span><span class="sxs-lookup"><span data-stu-id="ffe92-176">SignalR and jQuery Chat.cshtml</span></span>

<span data-ttu-id="ffe92-177">O arquivo de exibição *chat. cshtml* no exemplo de código mostra como usar a biblioteca do signalr jQuery para se comunicar com um Hub do signalr.</span><span class="sxs-lookup"><span data-stu-id="ffe92-177">The *Chat.cshtml* view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span>  <span data-ttu-id="ffe92-178">O código executa muitas tarefas importantes.</span><span class="sxs-lookup"><span data-stu-id="ffe92-178">The code carries out many important tasks.</span></span> <span data-ttu-id="ffe92-179">Ele cria uma referência ao proxy gerado automaticamente para o Hub, declara uma função que o servidor pode chamar para enviar conteúdo por push aos clientes e inicia uma conexão para envio de mensagens para o Hub.</span><span class="sxs-lookup"><span data-stu-id="ffe92-179">It creates a reference to the autogenerated proxy for the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="ffe92-180">No JavaScript, a referência à classe de servidor e seus membros estão em camelCase.</span><span class="sxs-lookup"><span data-stu-id="ffe92-180">In JavaScript, the reference to the server class and its members is in camelCase.</span></span> <span data-ttu-id="ffe92-181">O exemplo de código faz C# referência à classe `ChatHub` em JavaScript como `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="ffe92-181">The code sample references the C# `ChatHub` class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="ffe92-182">Nesse bloco de código, você cria uma função de retorno de chamada no script.</span><span class="sxs-lookup"><span data-stu-id="ffe92-182">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="ffe92-183">A classe Hub no servidor chama essa função para enviar por push atualizações de conteúdo para cada cliente.</span><span class="sxs-lookup"><span data-stu-id="ffe92-183">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="ffe92-184">A chamada opcional para a função `htmlEncode` mostra uma maneira de codificar o conteúdo da mensagem em HTML antes de exibi-lo na página.</span><span class="sxs-lookup"><span data-stu-id="ffe92-184">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page.</span></span> <span data-ttu-id="ffe92-185">É uma maneira de evitar a injeção de script.</span><span class="sxs-lookup"><span data-stu-id="ffe92-185">It's a way to prevent script injection.</span></span>

<span data-ttu-id="ffe92-186">Esse código abre uma conexão com o Hub.</span><span class="sxs-lookup"><span data-stu-id="ffe92-186">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> <span data-ttu-id="ffe92-187">Essa abordagem garante que você estabeleça uma conexão antes que o manipulador de eventos seja executado.</span><span class="sxs-lookup"><span data-stu-id="ffe92-187">This approach ensures that you establish a connection before the event handler executes.</span></span>

<span data-ttu-id="ffe92-188">O código inicia a conexão e, em seguida, passa a ela uma função para manipular o evento de clique no botão **Enviar** na página de bate-papo.</span><span class="sxs-lookup"><span data-stu-id="ffe92-188">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="ffe92-189">Obter o código</span><span class="sxs-lookup"><span data-stu-id="ffe92-189">Get the code</span></span>

[<span data-ttu-id="ffe92-190">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="ffe92-190">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

## <a name="additional-resources"></a><span data-ttu-id="ffe92-191">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="ffe92-191">Additional resources</span></span>

<span data-ttu-id="ffe92-192">Para obter mais informações sobre o Signalr, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="ffe92-192">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="ffe92-193">Projeto do signalr</span><span class="sxs-lookup"><span data-stu-id="ffe92-193">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="ffe92-194">GitHub e exemplos do signalr</span><span class="sxs-lookup"><span data-stu-id="ffe92-194">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="ffe92-195">Wiki do signalr</span><span class="sxs-lookup"><span data-stu-id="ffe92-195">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="ffe92-196">{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}</span><span class="sxs-lookup"><span data-stu-id="ffe92-196">Next steps</span></span>

<span data-ttu-id="ffe92-197">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="ffe92-197">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ffe92-198">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="ffe92-198">Set up the project</span></span>
> * <span data-ttu-id="ffe92-199">O exemplo foi executado</span><span class="sxs-lookup"><span data-stu-id="ffe92-199">Ran the sample</span></span>
> * <span data-ttu-id="ffe92-200">Examinou o código</span><span class="sxs-lookup"><span data-stu-id="ffe92-200">Examined the code</span></span>

<span data-ttu-id="ffe92-201">Avance para o próximo artigo para saber como criar um aplicativo Web que usa o Signalr ASP.NET 2 para fornecer funcionalidade de mensagens de alta frequência.</span><span class="sxs-lookup"><span data-stu-id="ffe92-201">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="ffe92-202">Aplicativo Web com mensagens de alta frequência</span><span class="sxs-lookup"><span data-stu-id="ffe92-202">Web app with high-frequency messaging</span></span>](tutorial-high-frequency-realtime-with-signalr.md)