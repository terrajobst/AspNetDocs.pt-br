---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 'Tutorial: criar um aplicativo em tempo real de alta frequência com o Signalr 2 | Microsoft Docs'
author: bradygaster
description: Este tutorial mostra como criar um aplicativo Web que usa o Signalr ASP.NET para fornecer funcionalidade de mensagens de alta frequência.
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 2503e90735d6cfa445ee08c9e43f8443aa106096
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558621"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a><span data-ttu-id="c474d-103">Tutorial: criar um aplicativo em tempo real de alta frequência com o Signalr 2</span><span class="sxs-lookup"><span data-stu-id="c474d-103">Tutorial: Create high-frequency real-time app with SignalR 2</span></span>

<span data-ttu-id="c474d-104">Este tutorial mostra como criar um aplicativo Web que usa o Signalr ASP.NET 2 para fornecer funcionalidade de mensagens de alta frequência.</span><span class="sxs-lookup"><span data-stu-id="c474d-104">This tutorial shows how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span> <span data-ttu-id="c474d-105">Nesse caso, "mensagens de alta frequência" significa que o servidor envia atualizações a uma taxa fixa.</span><span class="sxs-lookup"><span data-stu-id="c474d-105">In this case, "high-frequency messaging" means the server sends updates at a fixed rate.</span></span> <span data-ttu-id="c474d-106">Você envia até 10 mensagens por um segundo.</span><span class="sxs-lookup"><span data-stu-id="c474d-106">You send up to 10 messages a second.</span></span>

<span data-ttu-id="c474d-107">O aplicativo que você cria exibe uma forma que os usuários podem arrastar.</span><span class="sxs-lookup"><span data-stu-id="c474d-107">The application you create displays a shape that users can drag.</span></span> <span data-ttu-id="c474d-108">O servidor atualiza a posição da forma em todos os navegadores conectados para corresponder à posição da forma arrastada usando atualizações cronometradas.</span><span class="sxs-lookup"><span data-stu-id="c474d-108">The server updates the position of the shape in all connected browsers to match the position of the dragged shape using timed updates.</span></span>

<span data-ttu-id="c474d-109">Os conceitos apresentados neste tutorial têm aplicativos em jogos em tempo real e em outros aplicativos de simulação.</span><span class="sxs-lookup"><span data-stu-id="c474d-109">Concepts introduced in this tutorial have applications in real-time gaming and other simulation applications.</span></span>

<span data-ttu-id="c474d-110">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="c474d-110">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c474d-111">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="c474d-111">Set up the project</span></span>
> * <span data-ttu-id="c474d-112">Criar o aplicativo base</span><span class="sxs-lookup"><span data-stu-id="c474d-112">Create the base application</span></span>
> * <span data-ttu-id="c474d-113">Mapear para o Hub quando o aplicativo for iniciado</span><span class="sxs-lookup"><span data-stu-id="c474d-113">Map to the hub when app starts</span></span>
> * <span data-ttu-id="c474d-114">Adicionar o cliente</span><span class="sxs-lookup"><span data-stu-id="c474d-114">Add the client</span></span>
> * <span data-ttu-id="c474d-115">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="c474d-115">Run the app</span></span>
> * <span data-ttu-id="c474d-116">Adicionar o loop do cliente</span><span class="sxs-lookup"><span data-stu-id="c474d-116">Add the client loop</span></span>
> * <span data-ttu-id="c474d-117">Adicionar o loop do servidor</span><span class="sxs-lookup"><span data-stu-id="c474d-117">Add the server loop</span></span>
> * <span data-ttu-id="c474d-118">Adicionar animação suave</span><span class="sxs-lookup"><span data-stu-id="c474d-118">Add smooth animation</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="c474d-119">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="c474d-119">Prerequisites</span></span>

* <span data-ttu-id="c474d-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) com a carga de trabalho **ASP.NET e desenvolvimento para a Web**.</span><span class="sxs-lookup"><span data-stu-id="c474d-120">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="c474d-121">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="c474d-121">Set up the project</span></span>

<span data-ttu-id="c474d-122">Nesta seção, você criará o projeto no Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="c474d-122">In this section, you create the project in Visual Studio 2017.</span></span>

<span data-ttu-id="c474d-123">Esta seção mostra como usar o Visual Studio 2017 para criar um aplicativo Web ASP.NET vazio e adicionar as bibliotecas Signalr e jQuery. UI.</span><span class="sxs-lookup"><span data-stu-id="c474d-123">This section shows how to use Visual Studio 2017 to create an empty ASP.NET Web Application and add the SignalR and jQuery.UI libraries.</span></span>

1. <span data-ttu-id="c474d-124">No Visual Studio, crie um aplicativo Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c474d-124">In Visual Studio, create an ASP.NET Web Application.</span></span>

    ![Criar Web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. <span data-ttu-id="c474d-126">Na janela **novo aplicativo Web ASP.net-MoveShapeDemo** , deixe a seleção **vazia** e selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="c474d-126">In the **New ASP.NET Web Application - MoveShapeDemo** window, leave **Empty** selected and select **OK**.</span></span>

1. <span data-ttu-id="c474d-127">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **novo item**.</span><span class="sxs-lookup"><span data-stu-id="c474d-127">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="c474d-128">Em **Adicionar novo item-MoveShapeDemo**, selecione **instalado** >  \*\*C# > \*\* **Web** > **signalr** e selecione **classe de Hub do signalr (v2)** .</span><span class="sxs-lookup"><span data-stu-id="c474d-128">In **Add New Item - MoveShapeDemo**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="c474d-129">Nomeie a classe *MoveShapeHub* e adicione-a ao projeto.</span><span class="sxs-lookup"><span data-stu-id="c474d-129">Name the class *MoveShapeHub* and add it to the project.</span></span>

    <span data-ttu-id="c474d-130">Esta etapa cria o arquivo de classe *MoveShapeHub.cs* .</span><span class="sxs-lookup"><span data-stu-id="c474d-130">This step creates the *MoveShapeHub.cs* class file.</span></span> <span data-ttu-id="c474d-131">Simultaneamente, ele adiciona um conjunto de arquivos de script e referências de assembly que dão suporte ao Signalr para o projeto.</span><span class="sxs-lookup"><span data-stu-id="c474d-131">Simultaneously, it adds  a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="c474d-132">Selecione **Ferramentas** > **Gerenciador de Pacotes NuGet** > **Console do Gerenciador de Pacotes**.</span><span class="sxs-lookup"><span data-stu-id="c474d-132">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

1. <span data-ttu-id="c474d-133">No **console do Gerenciador de pacotes**, execute este comando:</span><span class="sxs-lookup"><span data-stu-id="c474d-133">In **Package Manager Console**, run this command:</span></span>

    ```console
    Install-Package jQuery.UI.Combined
    ```

    <span data-ttu-id="c474d-134">O comando instala a biblioteca da interface do usuário do jQuery.</span><span class="sxs-lookup"><span data-stu-id="c474d-134">The command installs the jQuery UI library.</span></span> <span data-ttu-id="c474d-135">Você o usa para animar a forma.</span><span class="sxs-lookup"><span data-stu-id="c474d-135">You use it to animate the shape.</span></span>

1. <span data-ttu-id="c474d-136">Em **Gerenciador de soluções**, expanda o nó scripts.</span><span class="sxs-lookup"><span data-stu-id="c474d-136">In **Solution Explorer**, expand the Scripts node.</span></span>

    ![Referências da biblioteca de scripts](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    <span data-ttu-id="c474d-138">As bibliotecas de script para jQuery, jQueryUI e Signalr são visíveis no projeto.</span><span class="sxs-lookup"><span data-stu-id="c474d-138">Script libraries for jQuery, jQueryUI, and SignalR are visible in the project.</span></span>

## <a name="create-the-base-application"></a><span data-ttu-id="c474d-139">Criar o aplicativo base</span><span class="sxs-lookup"><span data-stu-id="c474d-139">Create the base application</span></span>

<span data-ttu-id="c474d-140">Nesta seção, você criará um aplicativo de navegador.</span><span class="sxs-lookup"><span data-stu-id="c474d-140">In this section, you create a browser application.</span></span> <span data-ttu-id="c474d-141">O aplicativo envia o local da forma para o servidor durante cada evento de movimentação do mouse.</span><span class="sxs-lookup"><span data-stu-id="c474d-141">The app sends the location of the shape to the server during each mouse move event.</span></span> <span data-ttu-id="c474d-142">O servidor transmite essas informações para todos os outros clientes conectados em tempo real.</span><span class="sxs-lookup"><span data-stu-id="c474d-142">The server broadcasts this information to all other connected clients in real time.</span></span> <span data-ttu-id="c474d-143">Você aprenderá mais sobre esse aplicativo em seções posteriores.</span><span class="sxs-lookup"><span data-stu-id="c474d-143">You learn more about this application in later sections.</span></span>

1. <span data-ttu-id="c474d-144">Abra o arquivo *MoveShapeHub.cs* .</span><span class="sxs-lookup"><span data-stu-id="c474d-144">Open the *MoveShapeHub.cs* file.</span></span>

1. <span data-ttu-id="c474d-145">Substitua o código no arquivo *MoveShapeHub.cs* por este código:</span><span class="sxs-lookup"><span data-stu-id="c474d-145">Replace the code in the *MoveShapeHub.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. <span data-ttu-id="c474d-146">Salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="c474d-146">Save the file.</span></span>

<span data-ttu-id="c474d-147">A classe `MoveShapeHub` é uma implementação de um hub Signalr.</span><span class="sxs-lookup"><span data-stu-id="c474d-147">The `MoveShapeHub` class is an implementation of a SignalR hub.</span></span> <span data-ttu-id="c474d-148">Como no tutorial de [introdução com signalr](tutorial-getting-started-with-signalr.md) , o Hub tem um método que os clientes chamam diretamente.</span><span class="sxs-lookup"><span data-stu-id="c474d-148">As in the [Getting Started with SignalR](tutorial-getting-started-with-signalr.md) tutorial, the hub has a method that the clients call directly.</span></span> <span data-ttu-id="c474d-149">Nesse caso, o cliente envia um objeto com as novas coordenadas X e Y da forma para o servidor.</span><span class="sxs-lookup"><span data-stu-id="c474d-149">In this case, the client sends an object with the new X and Y coordinates of the shape to the server.</span></span> <span data-ttu-id="c474d-150">Essas coordenadas são transmitidas para todos os outros clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="c474d-150">Those coordinates get broadcasted to all other connected clients.</span></span> <span data-ttu-id="c474d-151">O signalr serializa automaticamente esse objeto usando JSON.</span><span class="sxs-lookup"><span data-stu-id="c474d-151">SignalR automatically serializes this object using JSON.</span></span>

<span data-ttu-id="c474d-152">O aplicativo envia o objeto de `ShapeModel` para o cliente.</span><span class="sxs-lookup"><span data-stu-id="c474d-152">The app sends the `ShapeModel` object to the client.</span></span> <span data-ttu-id="c474d-153">Ele tem membros para armazenar a posição da forma.</span><span class="sxs-lookup"><span data-stu-id="c474d-153">It has members to store the position of the shape.</span></span> <span data-ttu-id="c474d-154">A versão do objeto no servidor também tem um membro para controlar quais dados do cliente estão sendo armazenados.</span><span class="sxs-lookup"><span data-stu-id="c474d-154">The version of the object on the server also has a member to track which client's data is being stored.</span></span> <span data-ttu-id="c474d-155">Esse objeto impede que o servidor envie dados de um cliente de volta para ele mesmo.</span><span class="sxs-lookup"><span data-stu-id="c474d-155">This object prevents the server from sending a client's data back to itself.</span></span> <span data-ttu-id="c474d-156">Esse membro usa o atributo `JsonIgnore` para impedir que o aplicativo faça a serialização dos dados e o envie de volta para o cliente.</span><span class="sxs-lookup"><span data-stu-id="c474d-156">This member uses the `JsonIgnore` attribute to keep the application from serializing the data and sending it back to the client.</span></span>

## <a name="map-to-the-hub-when-app-starts"></a><span data-ttu-id="c474d-157">Mapear para o Hub quando o aplicativo for iniciado</span><span class="sxs-lookup"><span data-stu-id="c474d-157">Map to the hub when app starts</span></span>

<span data-ttu-id="c474d-158">Em seguida, configure o mapeamento para o Hub quando o aplicativo for iniciado.</span><span class="sxs-lookup"><span data-stu-id="c474d-158">Next, you set up mapping to the hub when the application starts.</span></span> <span data-ttu-id="c474d-159">No Signalr 2, a adição de uma classe de inicialização OWIN cria o mapeamento.</span><span class="sxs-lookup"><span data-stu-id="c474d-159">In SignalR 2, adding an OWIN startup class creates the mapping.</span></span>

1. <span data-ttu-id="c474d-160">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **novo item**.</span><span class="sxs-lookup"><span data-stu-id="c474d-160">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="c474d-161">Em **Adicionar novo item-MoveShapeDemo,** selecione **instalado** > **Visual C#**  > **Web** e, em seguida, selecione **classe de inicialização OWIN**.</span><span class="sxs-lookup"><span data-stu-id="c474d-161">In **Add New Item - MoveShapeDemo** select **Installed** > **Visual C#** > **Web** and then select **OWIN Startup Class**.</span></span>

1. <span data-ttu-id="c474d-162">Nomeie a classe como *Startup* e selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="c474d-162">Name the class *Startup* and select **OK**.</span></span>

1. <span data-ttu-id="c474d-163">Substitua o código padrão no arquivo *Startup.cs* por este código:</span><span class="sxs-lookup"><span data-stu-id="c474d-163">Replace the default code in the *Startup.cs* file with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<span data-ttu-id="c474d-164">A classe de inicialização OWIN chama `MapSignalR` quando o aplicativo executa o método `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="c474d-164">The OWIN startup class calls `MapSignalR` when the app executes the `Configuration` method.</span></span> <span data-ttu-id="c474d-165">O aplicativo adiciona a classe ao processo de inicialização do OWIN usando o atributo `OwinStartup` assembly.</span><span class="sxs-lookup"><span data-stu-id="c474d-165">The app adds the class to OWIN's startup process using the `OwinStartup` assembly attribute.</span></span>

## <a name="add-the-client"></a><span data-ttu-id="c474d-166">Adicionar o cliente</span><span class="sxs-lookup"><span data-stu-id="c474d-166">Add the client</span></span>

<span data-ttu-id="c474d-167">Adicione a página HTML do cliente.</span><span class="sxs-lookup"><span data-stu-id="c474d-167">Add the HTML page for the client.</span></span>

1. <span data-ttu-id="c474d-168">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **página HTML**.</span><span class="sxs-lookup"><span data-stu-id="c474d-168">In **Solution Explorer**, right-click the project and select **Add** > **HTML Page**.</span></span>

1. <span data-ttu-id="c474d-169">Nomeie a página **como padrão** e selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="c474d-169">Name the page **Default** and select **OK**.</span></span>

1. <span data-ttu-id="c474d-170">Em **Gerenciador de soluções**, clique com o botão direito do mouse em *Default. html* e selecione **definir como página inicial**.</span><span class="sxs-lookup"><span data-stu-id="c474d-170">In **Solution Explorer**, right-click *Default.html* and select **Set as Start Page**.</span></span>

1. <span data-ttu-id="c474d-171">Substitua o código padrão no arquivo *Default. html* por este código:</span><span class="sxs-lookup"><span data-stu-id="c474d-171">Replace the default code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. <span data-ttu-id="c474d-172">Em **Gerenciador de soluções**, expanda **scripts**.</span><span class="sxs-lookup"><span data-stu-id="c474d-172">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="c474d-173">As bibliotecas de script para jQuery e Signalr são visíveis no projeto.</span><span class="sxs-lookup"><span data-stu-id="c474d-173">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="c474d-174">O Gerenciador de pacotes instala uma versão mais recente dos scripts do Signalr.</span><span class="sxs-lookup"><span data-stu-id="c474d-174">The package manager installs a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="c474d-175">Atualize as referências de script no bloco de código para corresponder às versões dos arquivos de script no projeto.</span><span class="sxs-lookup"><span data-stu-id="c474d-175">Update the script references in the code block to correspond to the versions of the script files in the project.</span></span>

<span data-ttu-id="c474d-176">Esse código HTML e JavaScript cria um `div` vermelho chamado `shape`.</span><span class="sxs-lookup"><span data-stu-id="c474d-176">This HTML and JavaScript code creates a red `div` called `shape`.</span></span> <span data-ttu-id="c474d-177">Ele habilita o comportamento de arrastar da forma usando a biblioteca jQuery e usa o evento `drag` para enviar a posição da forma para o servidor.</span><span class="sxs-lookup"><span data-stu-id="c474d-177">It enables the shape's dragging behavior using the jQuery library and uses the `drag` event to send the shape's position to the server.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="c474d-178">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="c474d-178">Run the app</span></span>

<span data-ttu-id="c474d-179">Você pode executar o aplicativo para se'e-lo funcionar.</span><span class="sxs-lookup"><span data-stu-id="c474d-179">You can run the app to se\`e it work.</span></span> <span data-ttu-id="c474d-180">Quando você arrasta a forma em uma janela do navegador, a forma também se move em outros navegadores.</span><span class="sxs-lookup"><span data-stu-id="c474d-180">When you drag the shape around a browser window, the shape moves in the other browsers too.</span></span>

1. <span data-ttu-id="c474d-181">Na barra de ferramentas, ative a **depuração de script** e, em seguida, selecione o botão reproduzir para executar o aplicativo no modo de depuração.</span><span class="sxs-lookup"><span data-stu-id="c474d-181">In the toolbar, turn on **Script Debugging** and then select the play button to run the application in Debug mode.</span></span>

    ![Captura de tela do usuário que está ligando o modo de depuração e selecionando reproduzir.](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    <span data-ttu-id="c474d-183">Uma janela do navegador é aberta com a forma vermelha no canto superior direito.</span><span class="sxs-lookup"><span data-stu-id="c474d-183">A browser window opens with the red shape in the upper-right corner.</span></span>

1. <span data-ttu-id="c474d-184">Copie a URL da página.</span><span class="sxs-lookup"><span data-stu-id="c474d-184">Copy the page's URL.</span></span>

1. <span data-ttu-id="c474d-185">Abra outro navegador e cole a URL na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="c474d-185">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="c474d-186">Arraste a forma em uma das janelas do navegador.</span><span class="sxs-lookup"><span data-stu-id="c474d-186">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="c474d-187">A forma na outra janela do navegador é a seguinte.</span><span class="sxs-lookup"><span data-stu-id="c474d-187">The shape in the other browser window follows.</span></span>

<span data-ttu-id="c474d-188">Embora o aplicativo funcione usando esse método, ele não é um modelo de programação recomendado.</span><span class="sxs-lookup"><span data-stu-id="c474d-188">While the application functions using this method, it's not a recommended programming model.</span></span> <span data-ttu-id="c474d-189">Não há nenhum limite máximo para o número de mensagens enviadas.</span><span class="sxs-lookup"><span data-stu-id="c474d-189">There's no upper limit to the number of messages getting sent.</span></span> <span data-ttu-id="c474d-190">Como resultado, os clientes e o servidor ficarão sobrecarregados com mensagens e degradações de desempenho.</span><span class="sxs-lookup"><span data-stu-id="c474d-190">As a result, the clients and server get overwhelmed with messages and performance degrades.</span></span> <span data-ttu-id="c474d-191">Além disso, o aplicativo exibe uma animação não conjunta no cliente.</span><span class="sxs-lookup"><span data-stu-id="c474d-191">Also, the app displays a disjointed animation on the client.</span></span> <span data-ttu-id="c474d-192">Essa animação jerky acontece porque a forma se move instantaneamente por cada método.</span><span class="sxs-lookup"><span data-stu-id="c474d-192">This jerky animation happens because the shape moves instantly by each method.</span></span> <span data-ttu-id="c474d-193">É melhor se a forma se mover suavemente para cada novo local.</span><span class="sxs-lookup"><span data-stu-id="c474d-193">It's better if the shape moves smoothly to each new location.</span></span> <span data-ttu-id="c474d-194">Em seguida, você aprenderá a corrigir esses problemas.</span><span class="sxs-lookup"><span data-stu-id="c474d-194">Next, you learn how to fix those issues.</span></span>

## <a name="add-the-client-loop"></a><span data-ttu-id="c474d-195">Adicionar o loop do cliente</span><span class="sxs-lookup"><span data-stu-id="c474d-195">Add the client loop</span></span>

<span data-ttu-id="c474d-196">O envio do local da forma em cada evento de movimentação do mouse cria uma quantidade desnecessária de tráfego de rede.</span><span class="sxs-lookup"><span data-stu-id="c474d-196">Sending the location of the shape on every mouse move event creates an unnecessary amount of network traffic.</span></span> <span data-ttu-id="c474d-197">O aplicativo precisa limitar as mensagens do cliente.</span><span class="sxs-lookup"><span data-stu-id="c474d-197">The app needs to throttle the messages from the client.</span></span>

<span data-ttu-id="c474d-198">Use a função JavaScript `setInterval` para configurar um loop que envia novas informações de posição ao servidor em uma taxa fixa.</span><span class="sxs-lookup"><span data-stu-id="c474d-198">Use the javascript `setInterval` function to set up a loop that sends new position information to the server at a fixed rate.</span></span> <span data-ttu-id="c474d-199">Esse loop é uma representação básica de um "loop de jogo".</span><span class="sxs-lookup"><span data-stu-id="c474d-199">This loop is a basic representation of a "game loop."</span></span> <span data-ttu-id="c474d-200">É uma função chamada repetidamente que orienta toda a funcionalidade de um jogo.</span><span class="sxs-lookup"><span data-stu-id="c474d-200">It's a repeatedly called function that drives all the functionality of a game.</span></span>

1. <span data-ttu-id="c474d-201">Substitua o código do cliente no arquivo *Default. html* por este código:</span><span class="sxs-lookup"><span data-stu-id="c474d-201">Replace the client code in the *Default.html* file with this code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > <span data-ttu-id="c474d-202">Você precisa substituir as referências de script novamente.</span><span class="sxs-lookup"><span data-stu-id="c474d-202">You have to replace the script references again.</span></span> <span data-ttu-id="c474d-203">Eles devem corresponder às versões dos scripts no projeto.</span><span class="sxs-lookup"><span data-stu-id="c474d-203">They must match the versions of the scripts in the project.</span></span>

    <span data-ttu-id="c474d-204">Esse novo código adiciona a função `updateServerModel`.</span><span class="sxs-lookup"><span data-stu-id="c474d-204">This new code adds the `updateServerModel` function.</span></span> <span data-ttu-id="c474d-205">Ele é chamado em uma frequência fixa.</span><span class="sxs-lookup"><span data-stu-id="c474d-205">It gets called on a fixed frequency.</span></span> <span data-ttu-id="c474d-206">A função envia os dados de posição para o servidor sempre que o sinalizador de `moved` indica que há novos dados de posição a serem enviados.</span><span class="sxs-lookup"><span data-stu-id="c474d-206">The function sends the position data to the server whenever the `moved` flag indicates that there's new position data to send.</span></span>

1. <span data-ttu-id="c474d-207">Selecione o botão reproduzir para iniciar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="c474d-207">Select the play button to start the application</span></span>

1. <span data-ttu-id="c474d-208">Copie a URL da página.</span><span class="sxs-lookup"><span data-stu-id="c474d-208">Copy the page's URL.</span></span>

1. <span data-ttu-id="c474d-209">Abra outro navegador e cole a URL na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="c474d-209">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="c474d-210">Arraste a forma em uma das janelas do navegador.</span><span class="sxs-lookup"><span data-stu-id="c474d-210">Drag the shape in one of the browser windows.</span></span> <span data-ttu-id="c474d-211">A forma na outra janela do navegador é a seguinte.</span><span class="sxs-lookup"><span data-stu-id="c474d-211">The shape in the other browser window follows.</span></span>

<span data-ttu-id="c474d-212">Como o aplicativo limita o número de mensagens que são enviadas ao servidor, a animação não aparecerá como um Smooth.</span><span class="sxs-lookup"><span data-stu-id="c474d-212">Since the app throttles the number of messages that get sent to the server, the animation won't appear as smooth did at first.</span></span>

## <a name="add-the-server-loop"></a><span data-ttu-id="c474d-213">Adicionar o loop do servidor</span><span class="sxs-lookup"><span data-stu-id="c474d-213">Add the server loop</span></span>

<span data-ttu-id="c474d-214">No aplicativo atual, as mensagens enviadas do servidor para o cliente saem com a frequência em que são recebidas.</span><span class="sxs-lookup"><span data-stu-id="c474d-214">In the current application, messages sent from the server to the client go out as often as they're received.</span></span> <span data-ttu-id="c474d-215">Esse tráfego de rede apresenta um problema semelhante ao que vemos no cliente.</span><span class="sxs-lookup"><span data-stu-id="c474d-215">This network traffic presents a similar problem as we see on the client.</span></span>

<span data-ttu-id="c474d-216">O aplicativo pode enviar mensagens com mais frequência do que são necessárias.</span><span class="sxs-lookup"><span data-stu-id="c474d-216">The app can send messages more often than they're needed.</span></span> <span data-ttu-id="c474d-217">A conexão pode ser inundada como resultado.</span><span class="sxs-lookup"><span data-stu-id="c474d-217">The connection can become flooded as a result.</span></span> <span data-ttu-id="c474d-218">Esta seção descreve como atualizar o servidor para adicionar um temporizador que limita a taxa de mensagens de saída.</span><span class="sxs-lookup"><span data-stu-id="c474d-218">This section describes how to update the server to add a timer that throttles the rate of the outgoing messages.</span></span>

1. <span data-ttu-id="c474d-219">Substitua o conteúdo de `MoveShapeHub.cs` com este código:</span><span class="sxs-lookup"><span data-stu-id="c474d-219">Replace the contents of `MoveShapeHub.cs` with this code:</span></span>

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. <span data-ttu-id="c474d-220">Selecione o botão reproduzir para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c474d-220">Select the play button to start the application.</span></span>

1. <span data-ttu-id="c474d-221">Copie a URL da página.</span><span class="sxs-lookup"><span data-stu-id="c474d-221">Copy the page's URL.</span></span>

1. <span data-ttu-id="c474d-222">Abra outro navegador e cole a URL na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="c474d-222">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="c474d-223">Arraste a forma em uma das janelas do navegador.</span><span class="sxs-lookup"><span data-stu-id="c474d-223">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="c474d-224">Esse código expande o cliente para adicionar a classe de `Broadcaster`.</span><span class="sxs-lookup"><span data-stu-id="c474d-224">This code expands the client to add the `Broadcaster` class.</span></span> <span data-ttu-id="c474d-225">A nova classe limita as mensagens de saída usando a classe `Timer` do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c474d-225">The new class throttles the outgoing messages using the `Timer` class from the .NET framework.</span></span>

<span data-ttu-id="c474d-226">É bom saber que o próprio Hub é transitório.</span><span class="sxs-lookup"><span data-stu-id="c474d-226">It's good to learn that the hub itself is transitory.</span></span> <span data-ttu-id="c474d-227">Ele é criado toda vez que é necessário.</span><span class="sxs-lookup"><span data-stu-id="c474d-227">It's created every time it's needed.</span></span> <span data-ttu-id="c474d-228">Portanto, o aplicativo cria o `Broadcaster` como um singleton.</span><span class="sxs-lookup"><span data-stu-id="c474d-228">So the app creates the `Broadcaster` as a singleton.</span></span> <span data-ttu-id="c474d-229">Ele usa a inicialização lenta para adiar a criação do `Broadcaster`até que seja necessário.</span><span class="sxs-lookup"><span data-stu-id="c474d-229">It uses lazy initialization to defer the `Broadcaster`'s creation until it's needed.</span></span> <span data-ttu-id="c474d-230">Isso garante que o aplicativo crie a primeira instância de Hub completamente antes de iniciar o temporizador.</span><span class="sxs-lookup"><span data-stu-id="c474d-230">That guarantees that the app creates the first hub instance completely before starting the timer.</span></span>

<span data-ttu-id="c474d-231">A chamada para a função de `UpdateShape` dos clientes é então movida do método `UpdateModel` do Hub.</span><span class="sxs-lookup"><span data-stu-id="c474d-231">The call to the clients' `UpdateShape` function is then moved out of the hub's `UpdateModel` method.</span></span> <span data-ttu-id="c474d-232">Ele não é mais chamado imediatamente sempre que o aplicativo recebe mensagens de entrada.</span><span class="sxs-lookup"><span data-stu-id="c474d-232">It's no longer called immediately whenever the app receives incoming messages.</span></span> <span data-ttu-id="c474d-233">Em vez disso, o aplicativo envia as mensagens para os clientes a uma taxa de 25 chamadas por segundo.</span><span class="sxs-lookup"><span data-stu-id="c474d-233">Instead, the app sends the messages to the clients at a rate of 25 calls per second.</span></span> <span data-ttu-id="c474d-234">O processo é gerenciado pelo temporizador `_broadcastLoop` de dentro da classe `Broadcaster`.</span><span class="sxs-lookup"><span data-stu-id="c474d-234">The process is  managed by the `_broadcastLoop` timer from within the `Broadcaster` class.</span></span>

<span data-ttu-id="c474d-235">Por fim, em vez de chamar o método de cliente diretamente do Hub, a classe `Broadcaster` precisa obter uma referência para o Hub de `_hubContext` operacional no momento.</span><span class="sxs-lookup"><span data-stu-id="c474d-235">Lastly, instead of calling the client method from the hub directly, the `Broadcaster` class needs to get a reference to the currently operating `_hubContext` hub.</span></span> <span data-ttu-id="c474d-236">Ele obtém a referência com a `GlobalHost`.</span><span class="sxs-lookup"><span data-stu-id="c474d-236">It gets the reference with the `GlobalHost`.</span></span>

## <a name="add-smooth-animation"></a><span data-ttu-id="c474d-237">Adicionar animação suave</span><span class="sxs-lookup"><span data-stu-id="c474d-237">Add smooth animation</span></span>

<span data-ttu-id="c474d-238">O aplicativo está quase terminando, mas poderíamos fazer mais um aperfeiçoamento.</span><span class="sxs-lookup"><span data-stu-id="c474d-238">The application is almost finished, but we could make one more improvement.</span></span> <span data-ttu-id="c474d-239">O aplicativo move a forma no cliente em resposta às mensagens do servidor.</span><span class="sxs-lookup"><span data-stu-id="c474d-239">The app moves the shape on the client in response to server messages.</span></span> <span data-ttu-id="c474d-240">Em vez de definir a posição da forma como o novo local fornecido pelo servidor, use a função `animate` da biblioteca de interface do usuário do JQuery.</span><span class="sxs-lookup"><span data-stu-id="c474d-240">Instead of setting the position of the shape to the new location given by the server, use the JQuery UI library's `animate` function.</span></span> <span data-ttu-id="c474d-241">Ele pode mover a forma suavemente entre sua posição atual e nova.</span><span class="sxs-lookup"><span data-stu-id="c474d-241">It can move the shape smoothly between its current and new position.</span></span>

1. <span data-ttu-id="c474d-242">Atualize o método de `updateShape` do cliente no arquivo *Default. html* para se parecer com o código realçado:</span><span class="sxs-lookup"><span data-stu-id="c474d-242">Update the client's `updateShape` method in the *Default.html* file to look like the highlighted code:</span></span>

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. <span data-ttu-id="c474d-243">Selecione o botão reproduzir para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c474d-243">Select the play button to start the application.</span></span>

1. <span data-ttu-id="c474d-244">Copie a URL da página.</span><span class="sxs-lookup"><span data-stu-id="c474d-244">Copy the page's URL.</span></span>

1. <span data-ttu-id="c474d-245">Abra outro navegador e cole a URL na barra de endereços.</span><span class="sxs-lookup"><span data-stu-id="c474d-245">Open another browser and paste the URL into the address bar.</span></span>

1. <span data-ttu-id="c474d-246">Arraste a forma em uma das janelas do navegador.</span><span class="sxs-lookup"><span data-stu-id="c474d-246">Drag the shape in one of the browser windows.</span></span>

<span data-ttu-id="c474d-247">A movimentação da forma na outra janela aparece menos jerky.</span><span class="sxs-lookup"><span data-stu-id="c474d-247">The movement of the shape in the other window appears less jerky.</span></span> <span data-ttu-id="c474d-248">O aplicativo interpola seu movimento ao longo do tempo em vez de ser definido uma vez por mensagem de entrada.</span><span class="sxs-lookup"><span data-stu-id="c474d-248">The app interpolates its movement over time rather than being set once per incoming message.</span></span>

<span data-ttu-id="c474d-249">Esse código move a forma do local antigo para o novo.</span><span class="sxs-lookup"><span data-stu-id="c474d-249">This code moves the shape from the old location to the new one.</span></span> <span data-ttu-id="c474d-250">O servidor fornece a posição da forma no decorrer do intervalo de animação.</span><span class="sxs-lookup"><span data-stu-id="c474d-250">The server gives the position of the shape over the course of the animation interval.</span></span> <span data-ttu-id="c474d-251">Nesse caso, são 100 milissegundos.</span><span class="sxs-lookup"><span data-stu-id="c474d-251">In this case, that's 100 milliseconds.</span></span> <span data-ttu-id="c474d-252">O aplicativo limpa qualquer animação anterior em execução na forma antes que a nova animação seja iniciada.</span><span class="sxs-lookup"><span data-stu-id="c474d-252">The app clears any previous animation running on the shape before the new animation starts.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="c474d-253">Obter o código</span><span class="sxs-lookup"><span data-stu-id="c474d-253">Get the code</span></span>

[<span data-ttu-id="c474d-254">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="c474d-254">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a><span data-ttu-id="c474d-255">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="c474d-255">Additional resources</span></span>

<span data-ttu-id="c474d-256">O paradigma de comunicação sobre o qual você aprendeu é útil para desenvolver jogos online e outras simulações, como [o jogo do emissor criado com o signalr](https://shootr.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="c474d-256">The communication paradigm you just learned about is useful for developing online games and other simulations, like [the ShootR game created with SignalR](https://shootr.azurewebsites.net/).</span></span>

<span data-ttu-id="c474d-257">Para obter mais informações sobre o Signalr, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="c474d-257">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="c474d-258">Projeto do signalr</span><span class="sxs-lookup"><span data-stu-id="c474d-258">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="c474d-259">GitHub e exemplos do signalr</span><span class="sxs-lookup"><span data-stu-id="c474d-259">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="c474d-260">Wiki do signalr</span><span class="sxs-lookup"><span data-stu-id="c474d-260">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="c474d-261">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="c474d-261">Next steps</span></span>

<span data-ttu-id="c474d-262">Neste tutorial, você:</span><span class="sxs-lookup"><span data-stu-id="c474d-262">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c474d-263">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="c474d-263">Set up the project</span></span>
> * <span data-ttu-id="c474d-264">Criou o aplicativo base</span><span class="sxs-lookup"><span data-stu-id="c474d-264">Created the base application</span></span>
> * <span data-ttu-id="c474d-265">Mapeado para o Hub quando o aplicativo é iniciado</span><span class="sxs-lookup"><span data-stu-id="c474d-265">Mapped to the hub when app starts</span></span>
> * <span data-ttu-id="c474d-266">Adicionado o cliente</span><span class="sxs-lookup"><span data-stu-id="c474d-266">Added the client</span></span>
> * <span data-ttu-id="c474d-267">Executou o aplicativo</span><span class="sxs-lookup"><span data-stu-id="c474d-267">Ran the app</span></span>
> * <span data-ttu-id="c474d-268">Adicionado o loop do cliente</span><span class="sxs-lookup"><span data-stu-id="c474d-268">Added the client loop</span></span>
> * <span data-ttu-id="c474d-269">Adicionado o loop do servidor</span><span class="sxs-lookup"><span data-stu-id="c474d-269">Added the server loop</span></span>
> * <span data-ttu-id="c474d-270">Animação suave adicionada</span><span class="sxs-lookup"><span data-stu-id="c474d-270">Added smooth animation</span></span>

<span data-ttu-id="c474d-271">Avance para o próximo artigo para saber como criar um aplicativo Web que usa o Signalr ASP.NET 2 para fornecer a funcionalidade de difusão do servidor.</span><span class="sxs-lookup"><span data-stu-id="c474d-271">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide server broadcast functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="c474d-272">Sinalização 2 e difusão do servidor</span><span class="sxs-lookup"><span data-stu-id="c474d-272">SignalR 2 and server broadcasting</span></span>](tutorial-server-broadcast-with-signalr.md)