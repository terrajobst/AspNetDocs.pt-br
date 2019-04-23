---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Tutorial: Bate-papo em tempo real com SignalR 2 | Microsoft Docs'
author: bradygaster
description: Este tutorial mostra como usar o SignalR para criar um aplicativo de chat em tempo real. Adicionar o SignalR para um aplicativo de web ASP.NET vazio.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: ecc235454d4b95ce660a4373387f44720826b076
ms.sourcegitcommit: 2d53ed9e4c8b19d3526cbc689bfa8394c9449cec
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59905638"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a><span data-ttu-id="64744-104">Tutorial: Bate-papo em tempo real com SignalR 2</span><span class="sxs-lookup"><span data-stu-id="64744-104">Tutorial: Real-time chat with SignalR 2</span></span>

<span data-ttu-id="64744-105">Este tutorial mostra como usar o SignalR para criar um aplicativo de bate-papo em tempo real.</span><span class="sxs-lookup"><span data-stu-id="64744-105">This tutorial shows you how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="64744-106">Adicionar o SignalR para um aplicativo de web ASP.NET vazio e criar uma página HTML para enviar e exibir as mensagens.</span><span class="sxs-lookup"><span data-stu-id="64744-106">You add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

<span data-ttu-id="64744-107">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="64744-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="64744-108">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="64744-108">Set up the project</span></span>
> * <span data-ttu-id="64744-109">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="64744-109">Run the sample</span></span>
> * <span data-ttu-id="64744-110">Examinar o código</span><span class="sxs-lookup"><span data-stu-id="64744-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="64744-111">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="64744-111">Prerequisites</span></span>

* <span data-ttu-id="64744-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) com o **ASP.NET e desenvolvimento web** carga de trabalho.</span><span class="sxs-lookup"><span data-stu-id="64744-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="64744-113">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="64744-113">Set up the Project</span></span>

<span data-ttu-id="64744-114">Esta seção mostra como usar o Visual Studio 2017 e SignalR 2 para criar um aplicativo web ASP.NET vazio, adicione o SignalR e criar o aplicativo de bate-papo.</span><span class="sxs-lookup"><span data-stu-id="64744-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

1. <span data-ttu-id="64744-115">No Visual Studio, crie um aplicativo Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="64744-115">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Criar web](tutorial-getting-started-with-signalr/_static/image2.png)

1. <span data-ttu-id="64744-117">No **novo projeto ASP.NET - SignalRChat** janela, deixe **vazia** selecionado e selecione **Okey**.</span><span class="sxs-lookup"><span data-stu-id="64744-117">In the **New ASP.NET Project - SignalRChat** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="64744-118">Na **Gerenciador de soluções**, clique com botão direito no projeto e selecione **Add** > **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="64744-118">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="64744-119">Na **Adicionar Novo Item – SignalRChat**, selecione **instalado** > **Visual C#**   >  **Web**  >  **SignalR** e, em seguida, selecione **classe de Hub do SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="64744-119">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="64744-120">Nomeie a classe *ChatHub* e adicioná-lo ao projeto.</span><span class="sxs-lookup"><span data-stu-id="64744-120">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="64744-121">Esta etapa cria o *ChatHub.cs* arquivo de classe e adiciona um conjunto de arquivos de script e referências de assembly que dão suporte ao SignalR ao projeto.</span><span class="sxs-lookup"><span data-stu-id="64744-121">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="64744-122">Substitua o código no novo *ChatHub.cs* arquivo de classe com este código:</span><span class="sxs-lookup"><span data-stu-id="64744-122">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="64744-123">Na **Gerenciador de soluções**, clique com botão direito no projeto e selecione **Add** > **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="64744-123">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="64744-124">Na **Adicionar Novo Item – SignalRChat** selecionar **instalado** > **Visual C#**   >  **Web** e, em seguida, Selecione **classe de inicialização OWIN**.</span><span class="sxs-lookup"><span data-stu-id="64744-124">In **Add New Item - SignalRChat** select **Installed** > **Visual C#** > **Web**  and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="64744-125">Nomeie a classe *inicialização* e adicioná-lo ao projeto.</span><span class="sxs-lookup"><span data-stu-id="64744-125">Name the class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="64744-126">Substitua o código padrão no *inicialização* classe com este código:</span><span class="sxs-lookup"><span data-stu-id="64744-126">Replace the default code in *Startup* class with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]

1. <span data-ttu-id="64744-127">Na **Gerenciador de soluções**, clique com botão direito no projeto e selecione **Add** > **página HTML**.</span><span class="sxs-lookup"><span data-stu-id="64744-127">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="64744-128">Nomeie a nova página *índice* e selecione **Okey**.</span><span class="sxs-lookup"><span data-stu-id="64744-128">Name the new page *index* and select **OK**.</span></span>

1. <span data-ttu-id="64744-129">Na **Gerenciador de soluções**, a página HTML que você criou com o botão direito e selecione **definir como página inicial**.</span><span class="sxs-lookup"><span data-stu-id="64744-129">In **Solution Explorer**, right-click the HTML page you created and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="64744-130">Substitua o código padrão na página HTML com este código:</span><span class="sxs-lookup"><span data-stu-id="64744-130">Replace the default code in the HTML page with this code:</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. <span data-ttu-id="64744-131">Na **Gerenciador de soluções**, expanda **Scripts**.</span><span class="sxs-lookup"><span data-stu-id="64744-131">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="64744-132">Bibliotecas de script para o jQuery e o SignalR são visíveis no projeto.</span><span class="sxs-lookup"><span data-stu-id="64744-132">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="64744-133">O Gerenciador de pacotes pode ter instalado uma versão mais recente dos scripts do SignalR.</span><span class="sxs-lookup"><span data-stu-id="64744-133">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="64744-134">Verifique se as referências de script no bloco de código correspondem às versões dos arquivos de script no projeto.</span><span class="sxs-lookup"><span data-stu-id="64744-134">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="64744-135">Referências de script de bloco de código original:</span><span class="sxs-lookup"><span data-stu-id="64744-135">Script references from the original code block:</span></span>

    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. <span data-ttu-id="64744-136">Se eles não corresponderem, atualize o *. HTML* arquivo.</span><span class="sxs-lookup"><span data-stu-id="64744-136">If they don't match, update the *.html* file.</span></span>

1. <span data-ttu-id="64744-137">Na barra de menus, selecione **arquivo** > **Salvar tudo**.</span><span class="sxs-lookup"><span data-stu-id="64744-137">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="64744-138">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="64744-138">Run the Sample</span></span>

1. <span data-ttu-id="64744-139">Na barra de ferramentas, ative **depuração de Script** e, em seguida, selecione o botão Reproduzir para executar o exemplo no modo de depuração.</span><span class="sxs-lookup"><span data-stu-id="64744-139">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![Insira o nome de usuário](tutorial-getting-started-with-signalr/_static/image3.png)

1. <span data-ttu-id="64744-141">Quando o navegador for aberta, insira um nome para sua identidade de bate-papo.</span><span class="sxs-lookup"><span data-stu-id="64744-141">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="64744-142">Copie a URL do navegador, abra dois outros navegadores e cole as URLs em barras de endereço.</span><span class="sxs-lookup"><span data-stu-id="64744-142">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="64744-143">Em cada navegador, insira um nome exclusivo.</span><span class="sxs-lookup"><span data-stu-id="64744-143">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="64744-144">Agora, adicione um comentário e selecione **enviar**.</span><span class="sxs-lookup"><span data-stu-id="64744-144">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="64744-145">Repita que em outros navegadores.</span><span class="sxs-lookup"><span data-stu-id="64744-145">Repeat that in the other browsers.</span></span> <span data-ttu-id="64744-146">Os comentários são exibidos em tempo real.</span><span class="sxs-lookup"><span data-stu-id="64744-146">The comments appear in real-time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="64744-147">Este aplicativo de bate-papo simples não mantém o contexto de discussão no servidor.</span><span class="sxs-lookup"><span data-stu-id="64744-147">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="64744-148">O hub transmite seus comentários para todos os usuários atuais.</span><span class="sxs-lookup"><span data-stu-id="64744-148">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="64744-149">Usuários que participam de bate-papo mais tarde verá mensagens adicionadas desde o momento em que entrarem.</span><span class="sxs-lookup"><span data-stu-id="64744-149">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="64744-150">Veja como o aplicativo de bate-papo é executado em três diferentes navegadores.</span><span class="sxs-lookup"><span data-stu-id="64744-150">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="64744-151">Quando o Tom, Anand e Susan enviam mensagens, todos os navegadores são atualizados em tempo real:</span><span class="sxs-lookup"><span data-stu-id="64744-151">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![Todos os três navegadores exibem ao mesmo histórico de bate-papo](tutorial-getting-started-with-signalr/_static/image4.png)

1. <span data-ttu-id="64744-153">Na **Gerenciador de soluções**, inspecione o **documentos de Script** nó para o aplicativo em execução.</span><span class="sxs-lookup"><span data-stu-id="64744-153">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="64744-154">Há um arquivo de script denominado *hubs* que a biblioteca SignalR gera em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="64744-154">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="64744-155">Este arquivo gerencia a comunicação entre o script jQuery e o código do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="64744-155">This file manages the communication between jQuery script and server-side code.</span></span>

    ![script de hubs gerado automaticamente no nó documentos de Script](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="64744-157">Examinar o código</span><span class="sxs-lookup"><span data-stu-id="64744-157">Examine the Code</span></span>

<span data-ttu-id="64744-158">O aplicativo SignalRChat demonstra duas tarefas básicas de desenvolvimento de SignalR.</span><span class="sxs-lookup"><span data-stu-id="64744-158">The SignalRChat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="64744-159">Ele mostra como criar um hub.</span><span class="sxs-lookup"><span data-stu-id="64744-159">It shows you how to create a hub.</span></span> <span data-ttu-id="64744-160">O servidor usa esse hub de que o objeto principal de coordenação.</span><span class="sxs-lookup"><span data-stu-id="64744-160">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="64744-161">O hub usa a biblioteca jQuery SignalR para enviar e receber mensagens.</span><span class="sxs-lookup"><span data-stu-id="64744-161">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="64744-162">Hubs de SignalR no ChatHub.cs</span><span class="sxs-lookup"><span data-stu-id="64744-162">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="64744-163">No exemplo de código acima, o `ChatHub` classe deriva de `Microsoft.AspNet.SignalR.Hub` classe.</span><span class="sxs-lookup"><span data-stu-id="64744-163">In the code sample above, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="64744-164">Derivando a `Hub` classe é uma maneira útil para criar um aplicativo do SignalR.</span><span class="sxs-lookup"><span data-stu-id="64744-164">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="64744-165">Você pode criar métodos públicos em sua classe de hub e, em seguida, usar esses métodos chamando-as de scripts em uma página da web.</span><span class="sxs-lookup"><span data-stu-id="64744-165">You can create public methods on your hub class and then use those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="64744-166">No código de bate-papo, os clientes chamam o `ChatHub.Send` método para enviar uma nova mensagem.</span><span class="sxs-lookup"><span data-stu-id="64744-166">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="64744-167">O hub, em seguida, envia a mensagem a todos os clientes chamando `Clients.All.broadcastMessage`.</span><span class="sxs-lookup"><span data-stu-id="64744-167">The hub then sends the message to all clients by calling `Clients.All.broadcastMessage`.</span></span>

<span data-ttu-id="64744-168">O `Send` método demonstra vários conceitos de hub:</span><span class="sxs-lookup"><span data-stu-id="64744-168">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="64744-169">Declare métodos públicos em um hub para que os clientes possam chamá-los.</span><span class="sxs-lookup"><span data-stu-id="64744-169">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="64744-170">Use o `Microsoft.AspNet.SignalR.Hub.Clients` propriedade dinâmica para se comunicar com todos os clientes conectados a esse hub.</span><span class="sxs-lookup"><span data-stu-id="64744-170">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="64744-171">Chamar uma função no cliente (como o `broadcastMessage` função) para atualizar os clientes.</span><span class="sxs-lookup"><span data-stu-id="64744-171">Call a function on the client (like the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a><span data-ttu-id="64744-172">O SignalR e jQuery em index. HTML</span><span class="sxs-lookup"><span data-stu-id="64744-172">SignalR and jQuery in the index.html</span></span>

<span data-ttu-id="64744-173">O *index. HTML* página no exemplo de código mostra como usar a biblioteca jQuery SignalR para se comunicar com um hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="64744-173">The *index.html* page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="64744-174">O código realiza várias tarefas importantes.</span><span class="sxs-lookup"><span data-stu-id="64744-174">The code carries out many important tasks.</span></span> <span data-ttu-id="64744-175">Ele declara um proxy para o hub de referência, declara uma função que o servidor pode chamar para enviar conteúdo aos clientes, e ele inicia uma conexão para enviar mensagens ao hub.</span><span class="sxs-lookup"><span data-stu-id="64744-175">It declares a proxy to reference the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> <span data-ttu-id="64744-176">Em JavaScript a referência para a classe de servidor e seus membros deve ser camelCase.</span><span class="sxs-lookup"><span data-stu-id="64744-176">In JavaScript the reference to the server class and its members has to be camelCase.</span></span> <span data-ttu-id="64744-177">As referências de exemplo de código a C# *ChatHub* classe no JavaScript, enquanto `chatHub`.</span><span class="sxs-lookup"><span data-stu-id="64744-177">The code sample references the C# *ChatHub* class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="64744-178">Este bloco de código, você cria uma função de retorno de chamada no script.</span><span class="sxs-lookup"><span data-stu-id="64744-178">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

<span data-ttu-id="64744-179">A classe hub no servidor chama essa função para enviar atualizações de conteúdo para cada cliente.</span><span class="sxs-lookup"><span data-stu-id="64744-179">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="64744-180">As duas linhas que a codificação HTML o conteúdo antes de exibi-los são opcionais e mostrar uma boa maneira de evitar a injeção de script.</span><span class="sxs-lookup"><span data-stu-id="64744-180">The two lines that HTML-encode the content before displaying it are optional and show a good way to prevent script injection.</span></span>

<span data-ttu-id="64744-181">Esse código abre uma conexão com o hub.</span><span class="sxs-lookup"><span data-stu-id="64744-181">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> <span data-ttu-id="64744-182">Essa abordagem garante que o código estabelece uma conexão antes de ser executado o manipulador de eventos.</span><span class="sxs-lookup"><span data-stu-id="64744-182">This approach ensures that the code establishes a connection before the event handler executes.</span></span>

<span data-ttu-id="64744-183">O código começa a conexão e, em seguida, passa a ele uma função para manipular o evento de clique no **enviar** botão na página HTML.</span><span class="sxs-lookup"><span data-stu-id="64744-183">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="64744-184">Obter o código</span><span class="sxs-lookup"><span data-stu-id="64744-184">Get the code</span></span>

[<span data-ttu-id="64744-185">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="64744-185">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

## <a name="additional-resources"></a><span data-ttu-id="64744-186">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="64744-186">Additional resources</span></span>

<span data-ttu-id="64744-187">Para obter mais informações sobre o SignalR, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="64744-187">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="64744-188">Projeto de SignalR</span><span class="sxs-lookup"><span data-stu-id="64744-188">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="64744-189">Github do SignalR e exemplos</span><span class="sxs-lookup"><span data-stu-id="64744-189">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="64744-190">Wiki do SignalR</span><span class="sxs-lookup"><span data-stu-id="64744-190">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="64744-191">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="64744-191">Next steps</span></span>

<span data-ttu-id="64744-192">Neste tutorial você:</span><span class="sxs-lookup"><span data-stu-id="64744-192">In this tutorial you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="64744-193">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="64744-193">Set up the project</span></span>
> * <span data-ttu-id="64744-194">Executar a amostra</span><span class="sxs-lookup"><span data-stu-id="64744-194">Ran the sample</span></span>
> * <span data-ttu-id="64744-195">Examinado o código</span><span class="sxs-lookup"><span data-stu-id="64744-195">Examined the code</span></span>

<span data-ttu-id="64744-196">Avance para o próximo artigo para saber como usar o SignalR e ao MVC 5.</span><span class="sxs-lookup"><span data-stu-id="64744-196">Advance to the next article to learn how to use SignalR and MVC 5.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="64744-197">O SignalR 2 e MVC 5</span><span class="sxs-lookup"><span data-stu-id="64744-197">SignalR 2 and MVC 5</span></span>](tutorial-getting-started-with-signalr-and-mvc.md)