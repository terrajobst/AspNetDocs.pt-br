---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Tutorial: chat em tempo real com o Signalr 2 | Microsoft Docs'
author: bradygaster
description: Este tutorial mostra como usar o SignalR para criar um aplicativo de chat em tempo real. Você adiciona o Signalr a um aplicativo Web ASP.NET vazio.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: bc4ef190b6e36812b6fe7ca4e16eb763431e0e82
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600467"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a><span data-ttu-id="2ac0f-104">Tutorial: chat em tempo real com o Signalr 2</span><span class="sxs-lookup"><span data-stu-id="2ac0f-104">Tutorial: Real-time chat with SignalR 2</span></span>

<span data-ttu-id="2ac0f-105">Este tutorial mostra como usar o Signalr para criar um aplicativo de chat em tempo real.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-105">This tutorial shows you how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="2ac0f-106">Você adiciona o Signalr a um aplicativo Web ASP.NET vazio e cria uma página HTML para enviar e exibir mensagens.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-106">You add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

<span data-ttu-id="2ac0f-107">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="2ac0f-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2ac0f-108">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="2ac0f-108">Set up the project</span></span>
> * <span data-ttu-id="2ac0f-109">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="2ac0f-109">Run the sample</span></span>
> * <span data-ttu-id="2ac0f-110">Examinar o código</span><span class="sxs-lookup"><span data-stu-id="2ac0f-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="2ac0f-111">{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2ac0f-111">Prerequisites</span></span>

* <span data-ttu-id="2ac0f-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) com a ASP.net e a carga de trabalho de **desenvolvimento da Web** .</span><span class="sxs-lookup"><span data-stu-id="2ac0f-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="2ac0f-113">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="2ac0f-113">Set up the Project</span></span>

<span data-ttu-id="2ac0f-114">Esta seção mostra como usar o Visual Studio 2017 e o Signalr 2 para criar um aplicativo Web ASP.NET vazio, adicionar o Signalr e criar o aplicativo de chat.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

1. <span data-ttu-id="2ac0f-115">No Visual Studio, crie um aplicativo Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-115">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Criar Web](tutorial-getting-started-with-signalr/_static/image2.png)

1. <span data-ttu-id="2ac0f-117">Na janela **novo projeto do ASP.net-SignalRChat** , deixe a seleção **vazia** e selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-117">In the **New ASP.NET Project - SignalRChat** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="2ac0f-118">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **novo item**.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-118">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="2ac0f-119">Em **Adicionar novo item-SignalRChat**, selecione **instalado** >  \*\*C# > \*\* **Web** > **signalr** e selecione **classe de Hub do signalr (v2)** .</span><span class="sxs-lookup"><span data-stu-id="2ac0f-119">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="2ac0f-120">Nomeie a classe *ChatHub* e adicione-a ao projeto.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-120">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="2ac0f-121">Esta etapa cria o arquivo de classe *ChatHub.cs* e adiciona um conjunto de arquivos de script e referências de assembly que dão suporte a signalr ao projeto.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-121">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="2ac0f-122">Substitua o código no novo arquivo da classe *ChatHub.cs* por este código:</span><span class="sxs-lookup"><span data-stu-id="2ac0f-122">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="2ac0f-123">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **novo item**.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-123">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="2ac0f-124">Em **Adicionar novo item-SignalRChat,** selecione **instalado** > **Visual C#**  > **Web** e, em seguida, selecione **classe de inicialização OWIN**.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-124">In **Add New Item - SignalRChat** select **Installed** > **Visual C#** > **Web**  and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="2ac0f-125">Nomeie a classe como *Startup* e adicione-a ao projeto.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-125">Name the class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="2ac0f-126">Substitua o código padrão na classe de *inicialização* por este código:</span><span class="sxs-lookup"><span data-stu-id="2ac0f-126">Replace the default code in *Startup* class with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="2ac0f-127">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **página HTML**.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-127">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="2ac0f-128">Nomeie o novo *índice* de página e selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-128">Name the new page *index* and select **OK**.</span></span>

1. <span data-ttu-id="2ac0f-129">Em **Gerenciador de soluções**, clique com o botão direito do mouse na página HTML que você criou e selecione **definir como página inicial**.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-129">In **Solution Explorer**, right-click the HTML page you created and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="2ac0f-130">Substitua o código padrão na página HTML por este código:</span><span class="sxs-lookup"><span data-stu-id="2ac0f-130">Replace the default code in the HTML page with this code:</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. <span data-ttu-id="2ac0f-131">Em **Gerenciador de soluções**, expanda **scripts**.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-131">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="2ac0f-132">As bibliotecas de script para jQuery e Signalr são visíveis no projeto.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-132">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="2ac0f-133">O Gerenciador de pacotes pode ter instalado uma versão mais recente dos scripts do Signalr.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-133">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="2ac0f-134">Verifique se as referências de script no bloco de código correspondem às versões dos arquivos de script no projeto.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-134">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="2ac0f-135">Referências de script do bloco de código original:</span><span class="sxs-lookup"><span data-stu-id="2ac0f-135">Script references from the original code block:</span></span>

    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. <span data-ttu-id="2ac0f-136">Se eles não corresponderem, atualize o arquivo *. html* .</span><span class="sxs-lookup"><span data-stu-id="2ac0f-136">If they don't match, update the *.html* file.</span></span>

1. <span data-ttu-id="2ac0f-137">Na barra de menus, selecione **arquivo** > **salvar tudo**.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-137">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="2ac0f-138">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="2ac0f-138">Run the Sample</span></span>

1. <span data-ttu-id="2ac0f-139">Na barra de ferramentas, ative a **depuração de script** e, em seguida, selecione o botão reproduzir para executar o exemplo no modo de depuração.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-139">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Inserir nome de usuário](tutorial-getting-started-with-signalr/_static/image3.png)

1. <span data-ttu-id="2ac0f-141">Quando o navegador for aberto, insira um nome para sua identidade de chat.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-141">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="2ac0f-142">Copie a URL do navegador, abra dois outros navegadores e cole as URLs nas barras de endereço.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-142">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="2ac0f-143">Em cada navegador, insira um nome exclusivo.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-143">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="2ac0f-144">Agora, adicione um comentário e selecione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-144">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="2ac0f-145">Repita isso nos outros navegadores.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-145">Repeat that in the other browsers.</span></span> <span data-ttu-id="2ac0f-146">Os comentários aparecem em tempo real.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-146">The comments appear in real-time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2ac0f-147">Esse aplicativo de chat simples não mantém o contexto de discussão no servidor.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-147">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="2ac0f-148">O Hub transmite comentários para todos os usuários atuais.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-148">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="2ac0f-149">Os usuários que ingressarem no chat mais tarde verão as mensagens adicionadas a partir do momento em que ingressarem.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-149">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="2ac0f-150">Veja como o aplicativo de chat é executado em três navegadores diferentes.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-150">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="2ac0f-151">Quando Tom, Anand e Susan enviam mensagens, todos os navegadores são atualizados em tempo real:</span><span class="sxs-lookup"><span data-stu-id="2ac0f-151">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Todos os três navegadores exibem o mesmo histórico de chat](tutorial-getting-started-with-signalr/_static/image4.png)

1. <span data-ttu-id="2ac0f-153">Em **Gerenciador de soluções**, inspecione o nó **documentos de script** para o aplicativo em execução.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-153">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="2ac0f-154">Há um arquivo de script chamado *hubs* que a biblioteca do signalr gera no tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-154">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="2ac0f-155">Esse arquivo gerencia a comunicação entre o script do jQuery e o código do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-155">This file manages the communication between jQuery script and server-side code.</span></span>

    ![script de hubs gerados automaticamente no nó documentos de script](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="2ac0f-157">Examinar o código</span><span class="sxs-lookup"><span data-stu-id="2ac0f-157">Examine the Code</span></span>

<span data-ttu-id="2ac0f-158">O aplicativo SignalRChat demonstra duas tarefas básicas de desenvolvimento do Signalr.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-158">The SignalRChat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="2ac0f-159">Ele mostra como criar um Hub.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-159">It shows you how to create a hub.</span></span> <span data-ttu-id="2ac0f-160">O servidor usa esse Hub como o objeto de coordenação principal.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-160">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="2ac0f-161">O Hub usa a biblioteca do Signalr jQuery para enviar e receber mensagens.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-161">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="2ac0f-162">Hubs de sinalização no ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="2ac0f-162">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="2ac0f-163">No exemplo de código acima, a classe `ChatHub` deriva da classe `Microsoft.AspNet.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-163">In the code sample above, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="2ac0f-164">Derivar da classe `Hub` é uma maneira útil de criar um aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-164">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="2ac0f-165">Você pode criar métodos públicos em sua classe Hub e, em seguida, usar esses métodos chamando-os de scripts em uma página da Web.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-165">You can create public methods on your hub class and then use those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="2ac0f-166">No código do chat, os clientes chamam o método `ChatHub.Send` para enviar uma nova mensagem.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-166">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="2ac0f-167">O Hub, em seguida, envia a mensagem a todos os clientes chamando `Clients.All.broadcastMessage`.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-167">The hub then sends the message to all clients by calling `Clients.All.broadcastMessage`.</span></span>

<span data-ttu-id="2ac0f-168">O método `Send` demonstra vários conceitos de Hub:</span><span class="sxs-lookup"><span data-stu-id="2ac0f-168">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="2ac0f-169">Declare métodos públicos em um hub para que os clientes possam chamá-los.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-169">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="2ac0f-170">Use a propriedade dinâmica `Microsoft.AspNet.SignalR.Hub.Clients` para se comunicar com todos os clientes conectados a esse Hub.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-170">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="2ac0f-171">Chame uma função no cliente (como a função `broadcastMessage`) para atualizar clientes.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-171">Call a function on the client (like the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a><span data-ttu-id="2ac0f-172">Signalr e jQuery no index. html</span><span class="sxs-lookup"><span data-stu-id="2ac0f-172">SignalR and jQuery in the index.html</span></span>

<span data-ttu-id="2ac0f-173">A página *index. html* no exemplo de código mostra como usar a biblioteca do signalr jQuery para se comunicar com um hub signalr.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-173">The *index.html* page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="2ac0f-174">O código executa muitas tarefas importantes.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-174">The code carries out many important tasks.</span></span> <span data-ttu-id="2ac0f-175">Ele declara um proxy para referenciar o Hub, declara uma função que o servidor pode chamar para enviar conteúdo por push aos clientes e inicia uma conexão para envio de mensagens ao Hub.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-175">It declares a proxy to reference the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="2ac0f-176">Em JavaScript, a referência à classe de servidor e seus membros deve ser camelCase.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-176">In JavaScript the reference to the server class and its members has to be camelCase.</span></span> <span data-ttu-id="2ac0f-177">O exemplo de código faz C# referência à classe *ChatHub* em JavaScript como `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-177">The code sample references the C# *ChatHub* class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="2ac0f-178">Nesse bloco de código, você cria uma função de retorno de chamada no script.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-178">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="2ac0f-179">A classe Hub no servidor chama essa função para enviar por push atualizações de conteúdo para cada cliente.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-179">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="2ac0f-180">As duas linhas que codificam o conteúdo em HTML antes de exibi-las são opcionais e mostram uma boa maneira de evitar a injeção de script.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-180">The two lines that HTML-encode the content before displaying it are optional and show a good way to prevent script injection.</span></span>

<span data-ttu-id="2ac0f-181">Esse código abre uma conexão com o Hub.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-181">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> <span data-ttu-id="2ac0f-182">Essa abordagem garante que o código estabeleça uma conexão antes que o manipulador de eventos seja executado.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-182">This approach ensures that the code establishes a connection before the event handler executes.</span></span>

<span data-ttu-id="2ac0f-183">O código inicia a conexão e, em seguida, passa a ela uma função para manipular o evento de clique no botão **Enviar** na página HTML.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-183">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="2ac0f-184">Obter o código</span><span class="sxs-lookup"><span data-stu-id="2ac0f-184">Get the code</span></span>

[<span data-ttu-id="2ac0f-185">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="2ac0f-185">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a><span data-ttu-id="2ac0f-186">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="2ac0f-186">Additional resources</span></span>

<span data-ttu-id="2ac0f-187">Para obter mais informações sobre o Signalr, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="2ac0f-187">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="2ac0f-188">Projeto do signalr</span><span class="sxs-lookup"><span data-stu-id="2ac0f-188">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="2ac0f-189">GitHub e exemplos do signalr</span><span class="sxs-lookup"><span data-stu-id="2ac0f-189">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="2ac0f-190">Wiki do signalr</span><span class="sxs-lookup"><span data-stu-id="2ac0f-190">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="2ac0f-191">{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}</span><span class="sxs-lookup"><span data-stu-id="2ac0f-191">Next steps</span></span>

<span data-ttu-id="2ac0f-192">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="2ac0f-192">In this tutorial you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2ac0f-193">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="2ac0f-193">Set up the project</span></span>
> * <span data-ttu-id="2ac0f-194">O exemplo foi executado</span><span class="sxs-lookup"><span data-stu-id="2ac0f-194">Ran the sample</span></span>
> * <span data-ttu-id="2ac0f-195">Examinou o código</span><span class="sxs-lookup"><span data-stu-id="2ac0f-195">Examined the code</span></span>

<span data-ttu-id="2ac0f-196">Avance para o próximo artigo para aprender a usar o Signalr e o MVC 5.</span><span class="sxs-lookup"><span data-stu-id="2ac0f-196">Advance to the next article to learn how to use SignalR and MVC 5.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="2ac0f-197">Signalr 2 e MVC 5</span><span class="sxs-lookup"><span data-stu-id="2ac0f-197">SignalR 2 and MVC 5</span></span>](tutorial-getting-started-with-signalr-and-mvc.md)