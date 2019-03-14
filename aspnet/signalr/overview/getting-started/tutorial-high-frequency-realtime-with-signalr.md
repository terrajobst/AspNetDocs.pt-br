---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Tutorial: Criar o aplicativo em tempo real de alta frequência com SignalR 2 | Microsoft Docs'
author: bradygaster
description: Este tutorial mostra como criar um aplicativo web que usa o SignalR do ASP.NET para fornecer funcionalidade de mensagens de alta frequência.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 44aaa2b0c059de310e963f642fa56c2f00a7e443
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57024993"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a><span data-ttu-id="0ab97-103">Tutorial: Criar o aplicativo em tempo real de alta frequência com SignalR 2</span><span class="sxs-lookup"><span data-stu-id="0ab97-103">Tutorial: Create high-frequency real-time app with SignalR 2</span></span>

<span data-ttu-id="0ab97-104">Este tutorial mostra como criar um aplicativo web que usa o ASP.NET SignalR 2 para fornecer a funcionalidade de mensagens de alta frequência.</span><span class="sxs-lookup"><span data-stu-id="0ab97-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="0ab97-105">Nesse caso, "mensagens de alta frequência" significa que o servidor envia atualizações a uma taxa fixa.</span><span class="sxs-lookup"><span data-stu-id="0ab97-105">In this case, "high-frequency messaging" means the server sends updates at a fixed rate.</span></span> <span data-ttu-id="0ab97-106">Você envia mensagens de até 10 um segundo.</span><span class="sxs-lookup"><span data-stu-id="0ab97-106">You send up to 10 messages a second.</span></span>

<span data-ttu-id="0ab97-107">O aplicativo que você cria exibe uma forma que os usuários poderão arrastar.</span><span class="sxs-lookup"><span data-stu-id="0ab97-107">The application you create displays a shape that users can drag.</span></span> <span data-ttu-id="0ab97-108">O servidor atualiza a posição da forma em todos os navegadores conectados para coincidir com a posição da forma arrastada usando atualizações constantes.</span><span class="sxs-lookup"><span data-stu-id="0ab97-108">The server updates the position of the shape in all connected browsers to match the position of the dragged shape using timed updates.</span></span>

<span data-ttu-id="0ab97-109">Os conceitos apresentados neste tutorial tem aplicativos em jogos em tempo real e outros aplicativos de simulação.</span><span class="sxs-lookup"><span data-stu-id="0ab97-109">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>

<span data-ttu-id="0ab97-110">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="0ab97-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0ab97-111">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="0ab97-111">Set up the project</span></span>
> * <span data-ttu-id="0ab97-112">Criar o aplicativo básico</span><span class="sxs-lookup"><span data-stu-id="0ab97-112">Create the base application</span></span>
> * <span data-ttu-id="0ab97-113">Mapear para o hub quando o aplicativo é iniciado</span><span class="sxs-lookup"><span data-stu-id="0ab97-113">Map to the hub when app starts</span></span>
> * <span data-ttu-id="0ab97-114">Adicionar o cliente</span><span class="sxs-lookup"><span data-stu-id="0ab97-114">Add the client</span></span>
> * <span data-ttu-id="0ab97-115">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="0ab97-115">Run the app</span></span>
> * <span data-ttu-id="0ab97-116">Adicionar o loop de cliente</span><span class="sxs-lookup"><span data-stu-id="0ab97-116">Add the client loop</span></span>
> * <span data-ttu-id="0ab97-117">Adicionar o loop do servidor</span><span class="sxs-lookup"><span data-stu-id="0ab97-117">Add the server loop</span></span>
> * <span data-ttu-id="0ab97-118">Adicionar animações suaves</span><span class="sxs-lookup"><span data-stu-id="0ab97-118">Add smooth animation</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="0ab97-119">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="0ab97-119">Prerequisites</span></span>

* <span data-ttu-id="0ab97-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) com o **ASP.NET e desenvolvimento web** carga de trabalho.</span><span class="sxs-lookup"><span data-stu-id="0ab97-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="0ab97-121">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="0ab97-121">Set up the project</span></span>

<span data-ttu-id="0ab97-122">Nesta seção, você criará o projeto no Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="0ab97-122">In this section, you create the project in Visual Studio 2017.</span></span>

<span data-ttu-id="0ab97-123">Esta seção mostra como usar o Visual Studio 2017 para criar um aplicativo de Web ASP.NET vazio e adicionar as bibliotecas de SignalR e jQuery.UI.</span><span class="sxs-lookup"><span data-stu-id="0ab97-123">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application and add the SignalR and jQuery.UI libraries.</span></span>

1. <span data-ttu-id="0ab97-124">No Visual Studio, crie um aplicativo Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="0ab97-124">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Criar web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. <span data-ttu-id="0ab97-126">No **novo aplicativo de Web do ASP.NET - MoveShapeDemo** janela, deixe **vazia** selecionado e selecione **Okey**.</span><span class="sxs-lookup"><span data-stu-id="0ab97-126">In the **New ASP.NET Web Application - MoveShapeDemo** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="0ab97-127">Na **Gerenciador de soluções**, clique com botão direito no projeto e selecione **Add** > **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="0ab97-127">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="0ab97-128">Na **Adicionar Novo Item – MoveShapeDemo**, selecione **instalado** > **Visual C#**   >  **Web**  >  **SignalR** e, em seguida, selecione **classe de Hub do SignalR (v2)**.</span><span class="sxs-lookup"><span data-stu-id="0ab97-128">In **Add New Item - MoveShapeDemo**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="0ab97-129">Nomeie a classe *MoveShapeHub* e adicioná-lo ao projeto.</span><span class="sxs-lookup"><span data-stu-id="0ab97-129">Name the class *MoveShapeHub* and add it to the project.</span></span>

    <span data-ttu-id="0ab97-130">Esta etapa cria o *MoveShapeHub.cs* arquivo de classe.</span><span class="sxs-lookup"><span data-stu-id="0ab97-130">This step creates the *MoveShapeHub.cs* class file.</span></span> <span data-ttu-id="0ab97-131">Ao mesmo tempo, ele adiciona um conjunto de arquivos de script e referências de assembly que dão suporte ao SignalR ao projeto.</span><span class="sxs-lookup"><span data-stu-id="0ab97-131">Simultaneously, it adds  a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="0ab97-132">Selecione **ferramentas** > **Gerenciador de pacotes NuGet** > **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="0ab97-132">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

1. <span data-ttu-id="0ab97-133">Na **Package Manager Console**, execute este comando:</span><span class="sxs-lookup"><span data-stu-id="0ab97-133">In **Package Manager Console**, run this command:</span></span>

    ```console
    Install-Package jQuery.UI.Combined
    ```

    <span data-ttu-id="0ab97-134">O comando instala a biblioteca de interface do usuário do jQuery.</span><span class="sxs-lookup"><span data-stu-id="0ab97-134">The command installs the jQuery UI library.</span></span> <span data-ttu-id="0ab97-135">Você pode usá-lo para animar a forma.</span><span class="sxs-lookup"><span data-stu-id="0ab97-135">You use it to animate the shape.</span></span>

1. <span data-ttu-id="0ab97-136">Na **Gerenciador de soluções**, expanda o nó de Scripts.</span><span class="sxs-lookup"><span data-stu-id="0ab97-136">In **Solution Explorer**, expand the Scripts node.</span></span>

    ![Referências da biblioteca de script](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    <span data-ttu-id="0ab97-138">Bibliotecas de script para o jQuery, jQueryUI e SignalR são visíveis no projeto.</span><span class="sxs-lookup"><span data-stu-id="0ab97-138">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

## <a name="create-the-base-application"></a><span data-ttu-id="0ab97-139">Criar o aplicativo básico</span><span class="sxs-lookup"><span data-stu-id="0ab97-139">Create the base application</span></span>

<span data-ttu-id="0ab97-140">Nesta seção, você criará um aplicativo de navegador.</span><span class="sxs-lookup"><span data-stu-id="0ab97-140">In this section, you create a browser application.</span></span> <span data-ttu-id="0ab97-141">O aplicativo envia o local da forma para o servidor durante cada evento de movimentação do mouse.</span><span class="sxs-lookup"><span data-stu-id="0ab97-141">The app sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="0ab97-142">O servidor transmite essas informações para todos os outros clientes conectados em tempo real.</span><span class="sxs-lookup"><span data-stu-id="0ab97-142">The server broadcasts this information to all other connected clients in real time.</span></span> <span data-ttu-id="0ab97-143">Você aprenderá mais sobre este aplicativo nas próximas seções.</span><span class="sxs-lookup"><span data-stu-id="0ab97-143">You learn more about this application in later sections.</span></span>

1. <span data-ttu-id="0ab97-144">Abra o *MoveShapeHub.cs* arquivo.</span><span class="sxs-lookup"><span data-stu-id="0ab97-144">Open the *MoveShapeHub.cs* file.</span></span>

1. <span data-ttu-id="0ab97-145">Substitua o código na *MoveShapeHub.cs* arquivo com este código:</span><span class="sxs-lookup"><span data-stu-id="0ab97-145">Replace the code in the *MoveShapeHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="0ab97-146">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="0ab97-146">Save the file.</span></span>

<span data-ttu-id="0ab97-147">O `MoveShapeHub` classe é uma implementação de um hub SignalR.</span><span class="sxs-lookup"><span data-stu-id="0ab97-147">The `MoveShapeHub` class is an implementation of a SignalR hub.</span></span> <span data-ttu-id="0ab97-148">Como mostra a [Introdução ao SignalR](tutorial-getting-started-with-signalr.md) tutorial, o hub tem um método que os clientes chamam diretamente.</span><span class="sxs-lookup"><span data-stu-id="0ab97-148">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients call directly.</span></span> <span data-ttu-id="0ab97-149">Nesse caso, o cliente envia coordena a um objeto com o novo X e Y da forma no servidor.</span><span class="sxs-lookup"><span data-stu-id="0ab97-149">In this case, the client sends an object with the new X and Y coordinates of the shape to the server.</span></span> <span data-ttu-id="0ab97-150">Essas coordenadas obterem envia para todos os outros clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="0ab97-150">Those coordinates get broadcasted to all other connected clients.</span></span> <span data-ttu-id="0ab97-151">O SignalR serializa automaticamente deste objeto usando JSON.</span><span class="sxs-lookup"><span data-stu-id="0ab97-151">SignalR automatically serializes this object using JSON.</span></span>

<span data-ttu-id="0ab97-152">O aplicativo envia o `ShapeModel` objeto para o cliente.</span><span class="sxs-lookup"><span data-stu-id="0ab97-152">The app sends the `ShapeModel` object to the client.</span></span> <span data-ttu-id="0ab97-153">Ele tem membros para armazenar a posição da forma.</span><span class="sxs-lookup"><span data-stu-id="0ab97-153">It has members to store the position of the shape.</span></span> <span data-ttu-id="0ab97-154">A versão do objeto no servidor também tem um membro para controlar dados do cliente que estão sendo armazenados.</span><span class="sxs-lookup"><span data-stu-id="0ab97-154">The version of the object on the server also has a member to track which client's data is being stored.</span></span> <span data-ttu-id="0ab97-155">Esse objeto impede que o servidor enviar dados de um cliente para si mesmo.</span><span class="sxs-lookup"><span data-stu-id="0ab97-155">This object prevents the server from sending a client's data back to itself.</span></span> <span data-ttu-id="0ab97-156">Esse membro usa o `JsonIgnore` atributo para manter o aplicativo de serializar os dados e enviá-la de volta ao cliente.</span><span class="sxs-lookup"><span data-stu-id="0ab97-156">This member uses the `JsonIgnore` attribute to keep the application from serializing the data and sending it back to the client.</span></span>

## <a name="map-to-the-hub-when-app-starts"></a><span data-ttu-id="0ab97-157">Mapear para o hub quando o aplicativo é iniciado</span><span class="sxs-lookup"><span data-stu-id="0ab97-157">Map to the hub when app starts</span></span>

<span data-ttu-id="0ab97-158">Em seguida, configure o mapeamento para o hub quando o aplicativo é iniciado.</span><span class="sxs-lookup"><span data-stu-id="0ab97-158">Next, you set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="0ab97-159">No SignalR 2, adicionar uma classe de inicialização OWIN cria o mapeamento.</span><span class="sxs-lookup"><span data-stu-id="0ab97-159">In SignalR 2, adding an OWIN startup class creates the mapping.</span></span>

1. <span data-ttu-id="0ab97-160">Na **Gerenciador de soluções**, clique com botão direito no projeto e selecione **Add** > **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="0ab97-160">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="0ab97-161">Na **Adicionar Novo Item – MoveShapeDemo** selecionar **instalado** > **Visual C#**   >  **Web** e, em seguida, Selecione **classe de inicialização OWIN**.</span><span class="sxs-lookup"><span data-stu-id="0ab97-161">In **Add New Item - MoveShapeDemo** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="0ab97-162">Nomeie a classe *inicialização* e selecione **Okey**.</span><span class="sxs-lookup"><span data-stu-id="0ab97-162">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="0ab97-163">Substitua o código padrão a *Startup.cs* arquivo com este código:</span><span class="sxs-lookup"><span data-stu-id="0ab97-163">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<span data-ttu-id="0ab97-164">As chamadas de classe de inicialização OWIN `MapSignalR` quando o aplicativo executa o `Configuration` método.</span><span class="sxs-lookup"><span data-stu-id="0ab97-164">The OWIN startup class calls `MapSignalR` when the app executes the `Configuration` method.</span></span> <span data-ttu-id="0ab97-165">O aplicativo adiciona a classe para inicialização do OWIN processar usando a `OwinStartup` atributo do assembly.</span><span class="sxs-lookup"><span data-stu-id="0ab97-165">The app adds the class to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

## <a name="add-the-client"></a><span data-ttu-id="0ab97-166">Adicionar o cliente</span><span class="sxs-lookup"><span data-stu-id="0ab97-166">Add the client</span></span>

<span data-ttu-id="0ab97-167">Adicione página HTML para o cliente.</span><span class="sxs-lookup"><span data-stu-id="0ab97-167">Add the HTML page for the client.</span></span>

1. <span data-ttu-id="0ab97-168">Na **Gerenciador de soluções**, clique com botão direito no projeto e selecione **Add** > **página HTML**.</span><span class="sxs-lookup"><span data-stu-id="0ab97-168">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="0ab97-169">Nomeie a página **padrão** e selecione **Okey**.</span><span class="sxs-lookup"><span data-stu-id="0ab97-169">Name the page **Default** and select **OK**.</span></span>

1. <span data-ttu-id="0ab97-170">Na **Gerenciador de soluções**, clique com botão direito *default. HTML* e selecione **Set as Start Page**.</span><span class="sxs-lookup"><span data-stu-id="0ab97-170">In **Solution Explorer**, right-click *Default.html* and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="0ab97-171">Substitua o código padrão a *default. HTML* arquivo com este código:</span><span class="sxs-lookup"><span data-stu-id="0ab97-171">Replace the default code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. <span data-ttu-id="0ab97-172">Na **Gerenciador de soluções**, expanda **Scripts**.</span><span class="sxs-lookup"><span data-stu-id="0ab97-172">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="0ab97-173">Bibliotecas de script para o jQuery e o SignalR são visíveis no projeto.</span><span class="sxs-lookup"><span data-stu-id="0ab97-173">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="0ab97-174">O Gerenciador de pacotes instala uma versão mais recente dos scripts do SignalR.</span><span class="sxs-lookup"><span data-stu-id="0ab97-174">The package manager installs a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="0ab97-175">Atualize as referências de script no bloco de código para corresponder às versões dos arquivos de script no projeto.</span><span class="sxs-lookup"><span data-stu-id="0ab97-175">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

<span data-ttu-id="0ab97-176">Esse código HTML e JavaScript cria um vermelho `div` chamado `shape`.</span><span class="sxs-lookup"><span data-stu-id="0ab97-176">This HTML and JavaScript code creates a red `div` called `shape`.</span></span> <span data-ttu-id="0ab97-177">Ele habilita o comportamento de arrastar da forma usando a biblioteca jQuery e usa o `drag` eventos para enviar a posição da forma para o servidor.</span><span class="sxs-lookup"><span data-stu-id="0ab97-177">It enables the shape's dragging behavior using the jQuery library and uses the `drag` event to send the shape's position to the server.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="0ab97-178">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="0ab97-178">Run the app</span></span>

<span data-ttu-id="0ab97-179">Você pode executar o aplicativo para se'e ele funciona.</span><span class="sxs-lookup"><span data-stu-id="0ab97-179">You can run the app to se\`e it work.</span></span> <span data-ttu-id="0ab97-180">Quando você arrasta a forma em torno de uma janela do navegador, a forma se move em outros navegadores muito.</span><span class="sxs-lookup"><span data-stu-id="0ab97-180">When you drag the shape around a browser window, the shape moves in the other browsers too.</span></span>

1. <span data-ttu-id="0ab97-181">Na barra de ferramentas, ative **depuração de Script** e, em seguida, selecione o botão Reproduzir para executar o aplicativo no modo de depuração.</span><span class="sxs-lookup"><span data-stu-id="0ab97-181">In the toolbar, turn on **Script Debugging** and then select the play button to run the application in Debug mode.</span></span>

    ![Captura de tela do usuário ativar o modo de depuração e selecionando play.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    <span data-ttu-id="0ab97-183">Uma janela do navegador é aberta com o círculo vermelho no canto superior direito.</span><span class="sxs-lookup"><span data-stu-id="0ab97-183">A browser window opens with the red shape in the upper-right corner.</span></span>

1. <span data-ttu-id="0ab97-184">Copie a URL da página.</span><span class="sxs-lookup"><span data-stu-id="0ab97-184">Copy the page's URL.</span></span>

1. <span data-ttu-id="0ab97-185">Abra outro navegador e cole a URL na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="0ab97-185">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="0ab97-186">Arraste a forma de uma das janelas do navegador.</span><span class="sxs-lookup"><span data-stu-id="0ab97-186">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="0ab97-187">Segue a forma na janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="0ab97-187">The shape in the other browser window follows.</span></span>

<span data-ttu-id="0ab97-188">Enquanto o aplicativo de funções usando esse método, não é um modelo de programação recomendado.</span><span class="sxs-lookup"><span data-stu-id="0ab97-188">While the application functions using this method, it's not a recommended programming model.</span></span> <span data-ttu-id="0ab97-189">Não há nenhum limite superior ao número de mensagens enviadas de Introdução.</span><span class="sxs-lookup"><span data-stu-id="0ab97-189">There's no upper limit to the number of messages getting sent.</span></span> <span data-ttu-id="0ab97-190">Como resultado, os clientes e servidor obterem sobrecarregados com mensagens e degrada o desempenho.</span><span class="sxs-lookup"><span data-stu-id="0ab97-190">As a result, the clients and server get overwhelmed with messages and performance degrades.</span></span> <span data-ttu-id="0ab97-191">Além disso, o aplicativo exibe uma animação não contíguo no cliente.</span><span class="sxs-lookup"><span data-stu-id="0ab97-191">Also, the app displays a disjointed animation on the client.</span></span> <span data-ttu-id="0ab97-192">Essa animação brusco acontece porque a forma se move instantaneamente por cada método.</span><span class="sxs-lookup"><span data-stu-id="0ab97-192">This jerky animation happens because the shape moves instantly by each method.</span></span> <span data-ttu-id="0ab97-193">É melhor se a forma se move tranquilamente para cada novo local.</span><span class="sxs-lookup"><span data-stu-id="0ab97-193">It's better if the shape moves smoothly to each new location.</span></span> <span data-ttu-id="0ab97-194">Em seguida, você aprenderá a corrigir esses problemas.</span><span class="sxs-lookup"><span data-stu-id="0ab97-194">Next, you learn how to fix those issues.</span></span>

## <a name="add-the-client-loop"></a><span data-ttu-id="0ab97-195">Adicionar o loop de cliente</span><span class="sxs-lookup"><span data-stu-id="0ab97-195">Add the client loop</span></span>

<span data-ttu-id="0ab97-196">Enviar o local da forma em cada evento de movimentação do mouse cria uma quantidade desnecessária de tráfego de rede.</span><span class="sxs-lookup"><span data-stu-id="0ab97-196">Sending the location of the shape on every mouse move event creates an unnecessary amount of network traffic.</span></span> <span data-ttu-id="0ab97-197">O aplicativo precisa acelerar as mensagens do cliente.</span><span class="sxs-lookup"><span data-stu-id="0ab97-197">The app needs to throttle the messages from the client.</span></span>

<span data-ttu-id="0ab97-198">Usar o javascript `setInterval` função para configurar um loop que envia novas informações de posição para o servidor a uma taxa fixa.</span><span class="sxs-lookup"><span data-stu-id="0ab97-198">Use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="0ab97-199">Esse loop é uma representação básica de um "loop do jogo".</span><span class="sxs-lookup"><span data-stu-id="0ab97-199">This loop is a basic representation of a "game loop."</span></span> <span data-ttu-id="0ab97-200">É uma função chamada repetidamente que conduz toda a funcionalidade de um jogo.</span><span class="sxs-lookup"><span data-stu-id="0ab97-200">It's a repeatedly called function that drives all the functionality of a game.</span></span>

1. <span data-ttu-id="0ab97-201">Substitua o código de cliente na *default. HTML* arquivo com este código:</span><span class="sxs-lookup"><span data-stu-id="0ab97-201">Replace the client code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > <span data-ttu-id="0ab97-202">Você precisará substituir as referências de script novamente.</span><span class="sxs-lookup"><span data-stu-id="0ab97-202">You have to replace the script references again.</span></span> <span data-ttu-id="0ab97-203">Eles devem coincidir com as versões dos scripts no projeto.</span><span class="sxs-lookup"><span data-stu-id="0ab97-203">They must match the versions of the scripts in the project.</span></span>

    <span data-ttu-id="0ab97-204">Esse novo código adiciona o `updateServerModel` função.</span><span class="sxs-lookup"><span data-stu-id="0ab97-204">This new code adds the `updateServerModel` function.</span></span> <span data-ttu-id="0ab97-205">Ele é chamado em uma frequência fixa.</span><span class="sxs-lookup"><span data-stu-id="0ab97-205">It gets called on a fixed frequency.</span></span> <span data-ttu-id="0ab97-206">A função envia os dados de posição para o servidor sempre que o `moved` sinalizador indica que há novos dados de posição para enviar.</span><span class="sxs-lookup"><span data-stu-id="0ab97-206">The function sends the position data to the server whenever the `moved` flag indicates that there's new position data to send.</span></span>

1. <span data-ttu-id="0ab97-207">Selecione o botão play para iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="0ab97-207">Select the play button to start the application</span></span>

1. <span data-ttu-id="0ab97-208">Copie a URL da página.</span><span class="sxs-lookup"><span data-stu-id="0ab97-208">Copy the page's URL.</span></span>

1. <span data-ttu-id="0ab97-209">Abra outro navegador e cole a URL na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="0ab97-209">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="0ab97-210">Arraste a forma de uma das janelas do navegador.</span><span class="sxs-lookup"><span data-stu-id="0ab97-210">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="0ab97-211">Segue a forma na janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="0ab97-211">The shape in the other browser window follows.</span></span>

<span data-ttu-id="0ab97-212">Uma vez que o aplicativo limita o número de mensagens que são enviados ao servidor, a animação não aparecerá como smooth fez no início.</span><span class="sxs-lookup"><span data-stu-id="0ab97-212">Since the app throttles the number of messages that get sent to the server, the animation won't appear as smooth did at first.</span></span>

## <a name="add-the-server-loop"></a><span data-ttu-id="0ab97-213">Adicionar o loop do servidor</span><span class="sxs-lookup"><span data-stu-id="0ab97-213">Add the server loop</span></span>

<span data-ttu-id="0ab97-214">No aplicativo atual, as mensagens enviadas do servidor para o cliente sair sempre que são recebidas.</span><span class="sxs-lookup"><span data-stu-id="0ab97-214">In the current application, messages sent from the server to the client go out as often as they're received.</span></span> <span data-ttu-id="0ab97-215">Esse tráfego de rede apresenta um problema semelhante, como vemos no cliente.</span><span class="sxs-lookup"><span data-stu-id="0ab97-215">This network traffic presents a similar problem as we see on the client.</span></span>

<span data-ttu-id="0ab97-216">O aplicativo pode enviar mensagens com mais frequência que eles são necessários.</span><span class="sxs-lookup"><span data-stu-id="0ab97-216">The app can send messages more often than they're needed.</span></span> <span data-ttu-id="0ab97-217">A conexão pode inundada como resultado.</span><span class="sxs-lookup"><span data-stu-id="0ab97-217">The connection can become flooded as a result.</span></span> <span data-ttu-id="0ab97-218">Esta seção descreve como atualizar o servidor para adicionar um temporizador que limita a taxa das mensagens de saída.</span><span class="sxs-lookup"><span data-stu-id="0ab97-218">This section describes how to update the server to add a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="0ab97-219">Substitua o conteúdo do `MoveShapeHub.cs` com este código:</span><span class="sxs-lookup"><span data-stu-id="0ab97-219">Replace the contents of `MoveShapeHub.cs` with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. <span data-ttu-id="0ab97-220">Selecione o botão play para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0ab97-220">Select the play button to start the application.</span></span>

1. <span data-ttu-id="0ab97-221">Copie a URL da página.</span><span class="sxs-lookup"><span data-stu-id="0ab97-221">Copy the page's URL.</span></span>

1. <span data-ttu-id="0ab97-222">Abra outro navegador e cole a URL na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="0ab97-222">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="0ab97-223">Arraste a forma de uma das janelas do navegador.</span><span class="sxs-lookup"><span data-stu-id="0ab97-223">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="0ab97-224">Esse código expande o cliente para adicionar o `Broadcaster` classe.</span><span class="sxs-lookup"><span data-stu-id="0ab97-224">This code expands the client to add the `Broadcaster` class.</span></span> <span data-ttu-id="0ab97-225">A nova classe limita as mensagens de saída usando o `Timer` classe do .NET framework.</span><span class="sxs-lookup"><span data-stu-id="0ab97-225">The new class throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

<span data-ttu-id="0ab97-226">É bom saber que o hub em si é transitório.</span><span class="sxs-lookup"><span data-stu-id="0ab97-226">It's good to learn that the hub itself is transitory.</span></span> <span data-ttu-id="0ab97-227">Ele é criado sempre que for necessário.</span><span class="sxs-lookup"><span data-stu-id="0ab97-227">It's created every time it's needed.</span></span> <span data-ttu-id="0ab97-228">Portanto, o aplicativo cria o `Broadcaster` como um singleton.</span><span class="sxs-lookup"><span data-stu-id="0ab97-228">So the app creates the `Broadcaster` as a singleton.</span></span> <span data-ttu-id="0ab97-229">Ele usa a inicialização lenta para adiar o `Broadcaster`da criação até que ele seja necessário.</span><span class="sxs-lookup"><span data-stu-id="0ab97-229">It uses lazy initialization to defer the `Broadcaster`'s creation until it's needed.</span></span> <span data-ttu-id="0ab97-230">Que garante que o aplicativo cria a primeira instância de hub completamente antes de iniciar o temporizador.</span><span class="sxs-lookup"><span data-stu-id="0ab97-230">That guarantees that the app creates the first hub instance completely before starting the timer.</span></span>

<span data-ttu-id="0ab97-231">A chamada para os clientes `UpdateShape` função, em seguida, é movida para fora do hub `UpdateModel` método.</span><span class="sxs-lookup"><span data-stu-id="0ab97-231">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method.</span></span> <span data-ttu-id="0ab97-232">Ele não é chamado imediatamente, sempre que o aplicativo recebe mensagens de entrada.</span><span class="sxs-lookup"><span data-stu-id="0ab97-232">It's no longer called immediately whenever the app receives incoming messages.</span></span> <span data-ttu-id="0ab97-233">Em vez disso, o aplicativo envia mensagens para os clientes a uma taxa de chamadas de 25 por segundo.</span><span class="sxs-lookup"><span data-stu-id="0ab97-233">Instead, the app sends the messages to the clients at a rate of 25 calls per second.</span></span> <span data-ttu-id="0ab97-234">O processo é gerenciado pelo `_broadcastLoop` temporizador de dentro de `Broadcaster` classe.</span><span class="sxs-lookup"><span data-stu-id="0ab97-234">The process is  managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

<span data-ttu-id="0ab97-235">Por fim, em vez de chamar o método de cliente do hub diretamente, o `Broadcaster` classe precisa obter uma referência à operação atualmente `_hubContext` hub.</span><span class="sxs-lookup"><span data-stu-id="0ab97-235">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to get a reference to the currently operating `_hubContext` hub.</span></span> <span data-ttu-id="0ab97-236">Ele obtém a referência com o `GlobalHost`.</span><span class="sxs-lookup"><span data-stu-id="0ab97-236">It gets the reference with the `GlobalHost`.</span></span>

## <a name="add-smooth-animation"></a><span data-ttu-id="0ab97-237">Adicionar animações suaves</span><span class="sxs-lookup"><span data-stu-id="0ab97-237">Add smooth animation</span></span>

<span data-ttu-id="0ab97-238">O aplicativo está quase terminando, mas poderíamos criar uma melhoria de mais.</span><span class="sxs-lookup"><span data-stu-id="0ab97-238">The application is almost finished, but we could make one more improvement.</span></span> <span data-ttu-id="0ab97-239">O aplicativo move a forma no cliente em resposta às mensagens de servidor.</span><span class="sxs-lookup"><span data-stu-id="0ab97-239">The app moves the shape on the client in response to server messages.</span></span> <span data-ttu-id="0ab97-240">Em vez de definir a posição da forma para o novo local fornecido pelo servidor, use a biblioteca de JQuery UI `animate` função.</span><span class="sxs-lookup"><span data-stu-id="0ab97-240">Instead of setting the position of the shape to the new location given by the server, use the JQuery UI library's `animate` function.</span></span> <span data-ttu-id="0ab97-241">Ele pode mover a forma sem problemas entre sua posição atual e novo.</span><span class="sxs-lookup"><span data-stu-id="0ab97-241">It can move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="0ab97-242">Atualizar o cliente `updateShape` método na *default. HTML* arquivo para se parecer com o código realçado:</span><span class="sxs-lookup"><span data-stu-id="0ab97-242">Update the client's `updateShape` method in the *Default.html* file to look like the highlighted code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. <span data-ttu-id="0ab97-243">Selecione o botão play para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="0ab97-243">Select the play button to start the application.</span></span>

1. <span data-ttu-id="0ab97-244">Copie a URL da página.</span><span class="sxs-lookup"><span data-stu-id="0ab97-244">Copy the page's URL.</span></span>

1. <span data-ttu-id="0ab97-245">Abra outro navegador e cole a URL na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="0ab97-245">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="0ab97-246">Arraste a forma de uma das janelas do navegador.</span><span class="sxs-lookup"><span data-stu-id="0ab97-246">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="0ab97-247">A movimentação da forma em outra janela aparece menos brusco.</span><span class="sxs-lookup"><span data-stu-id="0ab97-247">The movement of the shape in the other window appears less jerky.</span></span> <span data-ttu-id="0ab97-248">O aplicativo interpola seu movimento ao longo do tempo em vez de ser definida uma vez por mensagem de entrada.</span><span class="sxs-lookup"><span data-stu-id="0ab97-248">The app interpolates its movement over time rather than being set once per incoming message.</span></span>

<span data-ttu-id="0ab97-249">Esse código move a forma do local antigo para o novo.</span><span class="sxs-lookup"><span data-stu-id="0ab97-249">This code moves the shape from the old location to the new one.</span></span> <span data-ttu-id="0ab97-250">O servidor fornece a posição da forma ao longo do intervalo de animação.</span><span class="sxs-lookup"><span data-stu-id="0ab97-250">The server gives the position of the shape over the course of the animation interval.</span></span> <span data-ttu-id="0ab97-251">Nesse caso, que é 100 milissegundos.</span><span class="sxs-lookup"><span data-stu-id="0ab97-251">In this case, that's 100 milliseconds.</span></span> <span data-ttu-id="0ab97-252">O aplicativo limpa qualquer animação anterior em execução na forma antes de inicia a nova animação.</span><span class="sxs-lookup"><span data-stu-id="0ab97-252">The app clears any previous animation running on the shape before the new animation starts.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="0ab97-253">Obter o código</span><span class="sxs-lookup"><span data-stu-id="0ab97-253">Get the code</span></span>

[<span data-ttu-id="0ab97-254">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="0ab97-254">Download Completed Project</span></span>](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a><span data-ttu-id="0ab97-255">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="0ab97-255">Additional resources</span></span>

<span data-ttu-id="0ab97-256">O paradigma de comunicação que você acabou de aprender sobre é útil para desenvolvimento de jogos online e outras simulações, como [ShootR jogo criado com o SignalR](https://shootr.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="0ab97-256">The communication paradigm you just learned about is useful for developing online games and other simulations, like [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="0ab97-257">Para obter mais informações sobre o SignalR, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="0ab97-257">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="0ab97-258">Projeto de SignalR</span><span class="sxs-lookup"><span data-stu-id="0ab97-258">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="0ab97-259">GitHub do SignalR e exemplos</span><span class="sxs-lookup"><span data-stu-id="0ab97-259">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="0ab97-260">Wiki do SignalR</span><span class="sxs-lookup"><span data-stu-id="0ab97-260">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="0ab97-261">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="0ab97-261">Next steps</span></span>

<span data-ttu-id="0ab97-262">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="0ab97-262">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0ab97-263">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="0ab97-263">Set up the project</span></span>
> * <span data-ttu-id="0ab97-264">Criado o aplicativo básico</span><span class="sxs-lookup"><span data-stu-id="0ab97-264">Created the base application</span></span>
> * <span data-ttu-id="0ab97-265">Mapeado para o hub quando o aplicativo é iniciado</span><span class="sxs-lookup"><span data-stu-id="0ab97-265">Mapped to the hub when app starts</span></span>
> * <span data-ttu-id="0ab97-266">Adicionou o cliente</span><span class="sxs-lookup"><span data-stu-id="0ab97-266">Added the client</span></span>
> * <span data-ttu-id="0ab97-267">Executei o aplicativo</span><span class="sxs-lookup"><span data-stu-id="0ab97-267">Ran the app</span></span>
> * <span data-ttu-id="0ab97-268">Adicionado o loop de cliente</span><span class="sxs-lookup"><span data-stu-id="0ab97-268">Added the client loop</span></span>
> * <span data-ttu-id="0ab97-269">Adicionado o loop do servidor</span><span class="sxs-lookup"><span data-stu-id="0ab97-269">Added the server loop</span></span>
> * <span data-ttu-id="0ab97-270">Adição de animação suave</span><span class="sxs-lookup"><span data-stu-id="0ab97-270">Added smooth animation</span></span>

<span data-ttu-id="0ab97-271">Avance para o próximo artigo para saber como criar um aplicativo web que usa o ASP.NET SignalR 2 para fornecer funcionalidade de difusão de servidor.</span><span class="sxs-lookup"><span data-stu-id="0ab97-271">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="0ab97-272">SignalR 2 e a transmissão de servidor</span><span class="sxs-lookup"><span data-stu-id="0ab97-272">SignalR 2 and server broadcasting</span></span>](tutorial-server-broadcast-with-signalr.md)