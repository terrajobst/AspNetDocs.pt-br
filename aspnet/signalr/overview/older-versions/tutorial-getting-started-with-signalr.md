---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 'Tutorial: Introdução com Signalr 1. x | Microsoft Docs'
author: bradygaster
description: Use o Signalr ASP.NET para criar um aplicativo de chat em tempo real em uma página HTML.
ms.author: bradyg
ms.date: 02/18/2013
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 87a90b47ae30bee43e0b0c1e078597db54b8e67d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623539"
---
# <a name="tutorial-getting-started-with-signalr-1x"></a><span data-ttu-id="9f13e-103">Tutorial: Introdução com Signalr 1. x</span><span class="sxs-lookup"><span data-stu-id="9f13e-103">Tutorial: Getting Started with SignalR 1.x</span></span>

<span data-ttu-id="9f13e-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="9f13e-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="9f13e-105">Este tutorial mostra como usar o SignalR para criar um aplicativo de chat em tempo real.</span><span class="sxs-lookup"><span data-stu-id="9f13e-105">This tutorial shows how to use SignalR to create a real-time chat application.</span></span> <span data-ttu-id="9f13e-106">Você adicionará o Signalr a um aplicativo Web ASP.NET vazio e criará uma página HTML para enviar e exibir mensagens.</span><span class="sxs-lookup"><span data-stu-id="9f13e-106">You will add SignalR to an empty ASP.NET web application and create an HTML page to send and display messages.</span></span>

## <a name="overview"></a><span data-ttu-id="9f13e-107">Visão geral</span><span class="sxs-lookup"><span data-stu-id="9f13e-107">Overview</span></span>

<span data-ttu-id="9f13e-108">Este tutorial apresenta o desenvolvimento do Signalr, mostrando como criar um aplicativo de chat simples baseado em navegador.</span><span class="sxs-lookup"><span data-stu-id="9f13e-108">This tutorial introduces SignalR development by showing how to build a simple browser-based chat application.</span></span> <span data-ttu-id="9f13e-109">Você adicionará a biblioteca do Signalr a um aplicativo Web ASP.NET vazio, criará uma classe de Hub para enviar mensagens aos clientes e criará uma página HTML que permite aos usuários enviar e receber mensagens de bate-papo.</span><span class="sxs-lookup"><span data-stu-id="9f13e-109">You will add the SignalR library to an empty ASP.NET web application, create a hub class for sending messages to clients, and create an HTML page that lets users send and receive chat messages.</span></span> <span data-ttu-id="9f13e-110">Para obter um tutorial semelhante que mostra como criar um aplicativo de chat no MVC 4 usando uma exibição do MVC, consulte [introdução com signalr e MVC 4](index.md).</span><span class="sxs-lookup"><span data-stu-id="9f13e-110">For a similar tutorial that shows how to create a chat application in MVC 4 using an MVC view, see [Getting Started with SignalR and MVC 4](index.md).</span></span>

> [!NOTE]
> <span data-ttu-id="9f13e-111">Este tutorial usa a versão de lançamento (1. x) do Signalr.</span><span class="sxs-lookup"><span data-stu-id="9f13e-111">This tutorial uses the release (1.x) version of SignalR.</span></span> <span data-ttu-id="9f13e-112">Para obter detalhes sobre as alterações entre o Signalr 1. x e o 2,0, consulte [Atualizando projetos do signalr 1. x](../releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="9f13e-112">For details on changes between SignalR 1.x and 2.0, see [Upgrading SignalR 1.x Projects](../releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<span data-ttu-id="9f13e-113">O signalr é uma biblioteca .NET de software livre para a criação de aplicativos Web que exigem interação do usuário ao vivo ou atualizações de dados em tempo real.</span><span class="sxs-lookup"><span data-stu-id="9f13e-113">SignalR is an open-source .NET library for building web applications that require live user interaction or real-time data updates.</span></span> <span data-ttu-id="9f13e-114">Os exemplos incluem aplicativos sociais, jogos multiusuário, colaboração de negócios e notícias, clima ou aplicativos de atualização financeira.</span><span class="sxs-lookup"><span data-stu-id="9f13e-114">Examples include social applications, multiuser games, business collaboration, and news, weather, or financial update applications.</span></span> <span data-ttu-id="9f13e-115">Eles costumam ser chamados de aplicativos em tempo real.</span><span class="sxs-lookup"><span data-stu-id="9f13e-115">These are often called real-time applications.</span></span>

<span data-ttu-id="9f13e-116">O signalr simplifica o processo de criação de aplicativos em tempo real.</span><span class="sxs-lookup"><span data-stu-id="9f13e-116">SignalR simplifies the process of building real-time applications.</span></span> <span data-ttu-id="9f13e-117">Ele inclui uma biblioteca de servidores ASP.NET e uma biblioteca de cliente JavaScript para facilitar o gerenciamento de conexões cliente-servidor e o envio por push de atualizações de conteúdo aos clientes.</span><span class="sxs-lookup"><span data-stu-id="9f13e-117">It includes an ASP.NET server library and a JavaScript client library to make it easier to manage client-server connections and push content updates to clients.</span></span> <span data-ttu-id="9f13e-118">Você pode adicionar a biblioteca do Signalr a um aplicativo ASP.NET existente para obter funcionalidades em tempo real.</span><span class="sxs-lookup"><span data-stu-id="9f13e-118">You can add the SignalR library to an existing ASP.NET application to gain real-time functionality.</span></span>

<span data-ttu-id="9f13e-119">O tutorial demonstra as seguintes tarefas de desenvolvimento do Signalr:</span><span class="sxs-lookup"><span data-stu-id="9f13e-119">The tutorial demonstrates the following SignalR development tasks:</span></span>

- <span data-ttu-id="9f13e-120">Adicionar a biblioteca do Signalr a um aplicativo Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="9f13e-120">Adding the SignalR library to an ASP.NET web application.</span></span>
- <span data-ttu-id="9f13e-121">Criar uma classe de Hub para enviar conteúdo por push aos clientes.</span><span class="sxs-lookup"><span data-stu-id="9f13e-121">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="9f13e-122">Usar a biblioteca do Signalr jQuery em uma página da Web para enviar mensagens e exibir atualizações do Hub.</span><span class="sxs-lookup"><span data-stu-id="9f13e-122">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="9f13e-123">A captura de tela a seguir mostra o aplicativo de chat em execução em um navegador.</span><span class="sxs-lookup"><span data-stu-id="9f13e-123">The following screen shot shows the chat application running in a browser.</span></span> <span data-ttu-id="9f13e-124">Cada novo usuário pode postar comentários e ver comentários adicionados após o usuário ingressar no chat.</span><span class="sxs-lookup"><span data-stu-id="9f13e-124">Each new user can post comments and see comments added after the user joins the chat.</span></span>

![Instâncias de chat](tutorial-getting-started-with-signalr/_static/image1.png)

<span data-ttu-id="9f13e-126">As</span><span class="sxs-lookup"><span data-stu-id="9f13e-126">Sections:</span></span>

- [<span data-ttu-id="9f13e-127">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="9f13e-127">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="9f13e-128">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="9f13e-128">Run the Sample</span></span>](#run)
- [<span data-ttu-id="9f13e-129">Examinar o código</span><span class="sxs-lookup"><span data-stu-id="9f13e-129">Examine the Code</span></span>](#code)
- [<span data-ttu-id="9f13e-130">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="9f13e-130">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="9f13e-131">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="9f13e-131">Set up the Project</span></span>

<span data-ttu-id="9f13e-132">Esta seção mostra como criar um aplicativo Web ASP.NET vazio, adicionar o Signalr e criar o aplicativo de chat.</span><span class="sxs-lookup"><span data-stu-id="9f13e-132">This section shows how to create an empty ASP.NET web application, add SignalR, and create the chat application.</span></span>

<span data-ttu-id="9f13e-133">Pré-requisitos:</span><span class="sxs-lookup"><span data-stu-id="9f13e-133">Prerequisites:</span></span>

- <span data-ttu-id="9f13e-134">Visual Studio 2010 SP1 ou 2012.</span><span class="sxs-lookup"><span data-stu-id="9f13e-134">Visual Studio 2010 SP1 or 2012.</span></span> <span data-ttu-id="9f13e-135">Se você não tiver o Visual Studio, consulte [downloads do ASP.net](https://www.asp.net/downloads) para obter a ferramenta gratuita de desenvolvimento do Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="9f13e-135">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="9f13e-136">[Microsoft ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=279941).</span><span class="sxs-lookup"><span data-stu-id="9f13e-136">[Microsoft ASP.NET and Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941).</span></span> <span data-ttu-id="9f13e-137">Para o Visual Studio 2012, esse instalador adiciona novos recursos do ASP.NET, incluindo modelos de sinalização ao Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9f13e-137">For Visual Studio 2012, this installer adds new ASP.NET features including SignalR templates to Visual Studio.</span></span> <span data-ttu-id="9f13e-138">Para o Visual Studio 2010 SP1, um instalador não está disponível, mas você pode concluir o tutorial instalando o pacote NuGet do Signalr, conforme descrito nas etapas de instalação.</span><span class="sxs-lookup"><span data-stu-id="9f13e-138">For Visual Studio 2010 SP1, an installer is not available but you can complete the tutorial by installing the SignalR NuGet package as described in the setup steps.</span></span>

<span data-ttu-id="9f13e-139">As etapas a seguir usam o Visual Studio 2012 para criar um aplicativo Web ASP.NET vazio e adicionar a biblioteca do Signalr:</span><span class="sxs-lookup"><span data-stu-id="9f13e-139">The following steps use Visual Studio 2012 to create an ASP.NET Empty Web Application and add the SignalR library:</span></span>

1. <span data-ttu-id="9f13e-140">No Visual Studio, crie um aplicativo Web ASP.NET vazio.</span><span class="sxs-lookup"><span data-stu-id="9f13e-140">In Visual Studio create an ASP.NET Empty Web Application.</span></span>

    ![Criar Web vazia](tutorial-getting-started-with-signalr/_static/image2.png)
2. <span data-ttu-id="9f13e-142">Abra o **console do Gerenciador de pacotes** selecionando **ferramentas | Gerenciador de pacotes NuGet | Console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="9f13e-142">Open the **Package Manager Console** by selecting **Tools | NuGet Package Manager | Package Manager Console**.</span></span> <span data-ttu-id="9f13e-143">Digite o seguinte comando na janela do console:</span><span class="sxs-lookup"><span data-stu-id="9f13e-143">Enter the following command into the console window:</span></span>

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    <span data-ttu-id="9f13e-144">Esse comando instala a versão mais recente do Signalr 1. x.</span><span class="sxs-lookup"><span data-stu-id="9f13e-144">This command installs the latest version of SignalR 1.x.</span></span>
3. <span data-ttu-id="9f13e-145">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto, selecione **Adicionar | Classe**.</span><span class="sxs-lookup"><span data-stu-id="9f13e-145">In **Solution Explorer**, right-click the project, select **Add | Class**.</span></span> <span data-ttu-id="9f13e-146">Nomeie a nova classe **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="9f13e-146">Name the new class **ChatHub**.</span></span>
4. <span data-ttu-id="9f13e-147">Em **Gerenciador de soluções** expanda o nó scripts.</span><span class="sxs-lookup"><span data-stu-id="9f13e-147">In **Solution Explorer** expand the Scripts node.</span></span> <span data-ttu-id="9f13e-148">As bibliotecas de script para jQuery e Signalr são visíveis no projeto.</span><span class="sxs-lookup"><span data-stu-id="9f13e-148">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    ![Referências de biblioteca](tutorial-getting-started-with-signalr/_static/image3.png)
5. <span data-ttu-id="9f13e-150">Substitua o código na classe **ChatHub** pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="9f13e-150">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. <span data-ttu-id="9f13e-151">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e clique em **Adicionar | Novo item**.</span><span class="sxs-lookup"><span data-stu-id="9f13e-151">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="9f13e-152">Na caixa de diálogo **Adicionar novo item** , selecione **classe de aplicativo global** e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="9f13e-152">In the **Add New Item** dialog, select **Global Application Class** and click **Add**.</span></span>

    ![Adicionar global](tutorial-getting-started-with-signalr/_static/image4.png)
7. <span data-ttu-id="9f13e-154">Adicione as instruções de `using` a seguir após as instruções `using` fornecidas na classe Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="9f13e-154">Add the following `using` statements after the provided `using` statements in the Global.asax.cs class.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. <span data-ttu-id="9f13e-155">Adicione a seguinte linha de código no método `Application_Start` da classe global para registrar a rota padrão para os hubs do Signalr.</span><span class="sxs-lookup"><span data-stu-id="9f13e-155">Add the following line of code in the `Application_Start` method of the Global class to register the default route for SignalR hubs.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. <span data-ttu-id="9f13e-156">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e clique em **Adicionar | Novo item**.</span><span class="sxs-lookup"><span data-stu-id="9f13e-156">In **Solution Explorer**, right-click the project, then click **Add | New Item**.</span></span> <span data-ttu-id="9f13e-157">Na caixa de diálogo **Adicionar novo item** , selecione página HTML e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="9f13e-157">In the **Add New Item** dialog, select Html Page and click **Add**.</span></span>
10. <span data-ttu-id="9f13e-158">No **Gerenciador de soluções**, clique com o botão direito do mouse na página HTML que você acabou de criar e clique em **definir como página inicial**.</span><span class="sxs-lookup"><span data-stu-id="9f13e-158">In **Solution Explorer**, right-click the HTML page you just created and click **Set as Start Page**.</span></span>
11. <span data-ttu-id="9f13e-159">Substitua o código padrão na página HTML pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="9f13e-159">Replace the default code in the HTML page with the following code.</span></span>

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. <span data-ttu-id="9f13e-160">**Salve tudo** para o projeto.</span><span class="sxs-lookup"><span data-stu-id="9f13e-160">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="9f13e-161">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="9f13e-161">Run the Sample</span></span>

1. <span data-ttu-id="9f13e-162">Pressione F5 para executar o projeto no modo de depuração.</span><span class="sxs-lookup"><span data-stu-id="9f13e-162">Press F5 to run the project in debug mode.</span></span> <span data-ttu-id="9f13e-163">A página HTML é carregada em uma instância do navegador e solicita um nome de usuário.</span><span class="sxs-lookup"><span data-stu-id="9f13e-163">The HTML page loads in a browser instance and prompts for a user name.</span></span>

    ![Inserir nome de usuário](tutorial-getting-started-with-signalr/_static/image5.png)
2. <span data-ttu-id="9f13e-165">Insira um nome de usuário.</span><span class="sxs-lookup"><span data-stu-id="9f13e-165">Enter a user name.</span></span>
3. <span data-ttu-id="9f13e-166">Copie a URL da linha de endereço do navegador e use-a para abrir mais duas instâncias do navegador.</span><span class="sxs-lookup"><span data-stu-id="9f13e-166">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="9f13e-167">Em cada instância do navegador, insira um nome de usuário exclusivo.</span><span class="sxs-lookup"><span data-stu-id="9f13e-167">In each browser instance, enter a unique user name.</span></span>
4. <span data-ttu-id="9f13e-168">Em cada instância do navegador, adicione um comentário e clique em **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="9f13e-168">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="9f13e-169">Os comentários devem ser exibidos em todas as instâncias do navegador.</span><span class="sxs-lookup"><span data-stu-id="9f13e-169">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9f13e-170">Esse aplicativo de chat simples não mantém o contexto de discussão no servidor.</span><span class="sxs-lookup"><span data-stu-id="9f13e-170">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="9f13e-171">O Hub transmite comentários para todos os usuários atuais.</span><span class="sxs-lookup"><span data-stu-id="9f13e-171">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="9f13e-172">Os usuários que ingressarem no chat mais tarde verão as mensagens adicionadas a partir do momento em que ingressarem.</span><span class="sxs-lookup"><span data-stu-id="9f13e-172">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="9f13e-173">A captura de tela a seguir mostra o aplicativo de chat em execução em três instâncias de navegador, todas que são atualizadas quando uma instância envia uma mensagem:</span><span class="sxs-lookup"><span data-stu-id="9f13e-173">The following screen shot shows the chat application running in three browser instances, all of which are updated when one instance sends a message:</span></span>

    ![Navegadores de chat](tutorial-getting-started-with-signalr/_static/image6.png)
5. <span data-ttu-id="9f13e-175">Em **Gerenciador de soluções**, inspecione o nó **documentos de script** para o aplicativo em execução.</span><span class="sxs-lookup"><span data-stu-id="9f13e-175">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="9f13e-176">Há um arquivo de script chamado **hubs** que a biblioteca do signalr gera dinamicamente no tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="9f13e-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="9f13e-177">Esse arquivo gerencia a comunicação entre o script do jQuery e o código do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="9f13e-177">This file manages the communication between jQuery script and server-side code.</span></span>

    ![Script de Hub gerado](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="9f13e-179">Examinar o código</span><span class="sxs-lookup"><span data-stu-id="9f13e-179">Examine the Code</span></span>

<span data-ttu-id="9f13e-180">O aplicativo de chat do Signalr demonstra duas tarefas básicas de desenvolvimento do Signalr: criar um hub como o objeto principal de coordenação no servidor e usar a biblioteca do Signalr jQuery para enviar e receber mensagens.</span><span class="sxs-lookup"><span data-stu-id="9f13e-180">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="9f13e-181">Hubs de sinalização</span><span class="sxs-lookup"><span data-stu-id="9f13e-181">SignalR Hubs</span></span>

<span data-ttu-id="9f13e-182">No exemplo de código, a classe **ChatHub** deriva da classe **Microsoft. AspNet. signalr. Hub** .</span><span class="sxs-lookup"><span data-stu-id="9f13e-182">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="9f13e-183">Derivar da classe **Hub** é uma maneira útil de criar um aplicativo signalr.</span><span class="sxs-lookup"><span data-stu-id="9f13e-183">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="9f13e-184">Você pode criar métodos públicos em sua classe Hub e, em seguida, acessar esses métodos chamando-os de scripts jQuery em uma página da Web.</span><span class="sxs-lookup"><span data-stu-id="9f13e-184">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="9f13e-185">No código do chat, os clientes chamam o método **ChatHub. Send** para enviar uma nova mensagem.</span><span class="sxs-lookup"><span data-stu-id="9f13e-185">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="9f13e-186">O Hub, por sua vez, envia a mensagem a todos os clientes chamando **clientes. All. broadcastMessage**.</span><span class="sxs-lookup"><span data-stu-id="9f13e-186">The hub in turn sends the message to all clients by calling **Clients.All.broadcastMessage**.</span></span>

<span data-ttu-id="9f13e-187">O método **Send** demonstra vários conceitos de Hub:</span><span class="sxs-lookup"><span data-stu-id="9f13e-187">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="9f13e-188">Declare métodos públicos em um hub para que os clientes possam chamá-los.</span><span class="sxs-lookup"><span data-stu-id="9f13e-188">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="9f13e-189">Use a propriedade dinâmica **Microsoft. AspNet. signalr. Hub. clients** para acessar todos os clientes conectados a esse Hub.</span><span class="sxs-lookup"><span data-stu-id="9f13e-189">Use the **Microsoft.AspNet.SignalR.Hub.Clients** dynamic property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="9f13e-190">Chame uma função do jQuery no cliente (como a função `broadcastMessage`) para atualizar clientes.</span><span class="sxs-lookup"><span data-stu-id="9f13e-190">Call a jQuery function on the client (such as the `broadcastMessage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="9f13e-191">Signalr e jQuery</span><span class="sxs-lookup"><span data-stu-id="9f13e-191">SignalR and jQuery</span></span>

<span data-ttu-id="9f13e-192">A página HTML no exemplo de código mostra como usar a biblioteca do Signalr jQuery para se comunicar com um Hub do Signalr.</span><span class="sxs-lookup"><span data-stu-id="9f13e-192">The HTML page in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="9f13e-193">As tarefas essenciais no código estão declarando um proxy para referenciar o Hub, declarando uma função que o servidor pode chamar para enviar conteúdo por push aos clientes e iniciando uma conexão para envio de mensagens para o Hub.</span><span class="sxs-lookup"><span data-stu-id="9f13e-193">The essential tasks in the code are declaring a proxy to reference the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="9f13e-194">O código a seguir declara um proxy para um Hub.</span><span class="sxs-lookup"><span data-stu-id="9f13e-194">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="9f13e-195">No jQuery, a referência à classe de servidor e seus membros estão no camel case.</span><span class="sxs-lookup"><span data-stu-id="9f13e-195">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="9f13e-196">O exemplo de código faz C# referência à classe **ChatHub** no jQuery como **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="9f13e-196">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span>

<span data-ttu-id="9f13e-197">O código a seguir é como você cria uma função de retorno de chamada no script.</span><span class="sxs-lookup"><span data-stu-id="9f13e-197">The following code is how you create a callback function in the script.</span></span> <span data-ttu-id="9f13e-198">A classe Hub no servidor chama essa função para enviar por push atualizações de conteúdo para cada cliente.</span><span class="sxs-lookup"><span data-stu-id="9f13e-198">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="9f13e-199">As duas linhas em que o HTML codifica o conteúdo antes de exibi-lo são opcionais e mostram uma maneira simples de evitar a injeção de script.</span><span class="sxs-lookup"><span data-stu-id="9f13e-199">The two lines that HTML encode the content before displaying it are optional and show a simple way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

<span data-ttu-id="9f13e-200">O código a seguir mostra como abrir uma conexão com o Hub.</span><span class="sxs-lookup"><span data-stu-id="9f13e-200">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="9f13e-201">O código inicia a conexão e, em seguida, passa a ela uma função para manipular o evento de clique no botão **Enviar** na página HTML.</span><span class="sxs-lookup"><span data-stu-id="9f13e-201">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the HTML page.</span></span>

> [!NOTE]
> <span data-ttu-id="9f13e-202">Essa abordagem garante que a conexão seja estabelecida antes que o manipulador de eventos seja executado.</span><span class="sxs-lookup"><span data-stu-id="9f13e-202">This approach insures that the connection is established before the event handler executes.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="9f13e-203">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="9f13e-203">Next Steps</span></span>

<span data-ttu-id="9f13e-204">Você aprendeu que o Signalr é uma estrutura para a criação de aplicativos Web em tempo real.</span><span class="sxs-lookup"><span data-stu-id="9f13e-204">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="9f13e-205">Você também aprendeu várias tarefas de desenvolvimento do Signalr: como adicionar o Signalr a um aplicativo ASP.NET, como criar uma classe de Hub e como enviar e receber mensagens do Hub.</span><span class="sxs-lookup"><span data-stu-id="9f13e-205">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="9f13e-206">Você pode tornar o aplicativo de exemplo neste tutorial ou outros aplicativos Signaler disponíveis pela Internet implantando-os em um provedor de hospedagem.</span><span class="sxs-lookup"><span data-stu-id="9f13e-206">You can make the sample application in this tutorial or other SignalR applications available over the Internet by deploying them to a hosting provider.</span></span> <span data-ttu-id="9f13e-207">A Microsoft oferece hospedagem na Web gratuita para até 10 sites em uma conta de avaliação gratuita do [Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="9f13e-207">Microsoft offers free web hosting for up to 10 web sites in a free [Windows Azure trial account](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="9f13e-208">Para obter instruções sobre como implantar o aplicativo Signalr de exemplo, consulte [publicar o signalr introdução amostra como um site do Windows Azure](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span><span class="sxs-lookup"><span data-stu-id="9f13e-208">For a walkthrough on how to deploy the sample SignalR application, see [Publish the SignalR Getting Started Sample as a Windows Azure Web Site](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx).</span></span> <span data-ttu-id="9f13e-209">Para obter informações detalhadas sobre como implantar um projeto Web do Visual Studio em um site do Windows Azure, consulte [implantando um aplicativo ASP.net em um site do Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span><span class="sxs-lookup"><span data-stu-id="9f13e-209">For detailed information about how to deploy a Visual Studio web project to a Windows Azure Web Site, see [Deploying an ASP.NET Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span> <span data-ttu-id="9f13e-210">(Observação: o transporte WebSocket não tem suporte no momento para sites do Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="9f13e-210">(Note: The WebSocket transport is not currently supported for Windows Azure Web Sites.</span></span> <span data-ttu-id="9f13e-211">Quando o transporte WebSocket não está disponível, o Signalr usa os outros transportes disponíveis, conforme descrito na seção transportes do [tópico introdução ao signalr](index.md).)</span><span class="sxs-lookup"><span data-stu-id="9f13e-211">When WebSocket transport is not available, SignalR uses the other available transports as described in the Transports section of the [Introduction to SignalR topic](index.md).)</span></span>

<span data-ttu-id="9f13e-212">Para saber mais sobre os conceitos de desenvolvimento do Signalr avançado, visite os seguintes sites para código-fonte do Signalr e recursos:</span><span class="sxs-lookup"><span data-stu-id="9f13e-212">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources:</span></span>

- [<span data-ttu-id="9f13e-213">Projeto do signalr</span><span class="sxs-lookup"><span data-stu-id="9f13e-213">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="9f13e-214">GitHub e exemplos do signalr</span><span class="sxs-lookup"><span data-stu-id="9f13e-214">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="9f13e-215">Wiki do signalr</span><span class="sxs-lookup"><span data-stu-id="9f13e-215">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
