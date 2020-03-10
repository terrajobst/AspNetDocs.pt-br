---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Tutorial: Introdução com Signalr 1. x e MVC 4 | Microsoft Docs'
author: bradygaster
description: Use o Signalr ASP.NET e o ASP.NET MVC 4 para criar um aplicativo de chat em tempo real.
ms.author: bradyg
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 9186915df6d5de6bc20dfc0adabc54056d2f3a8c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579565"
---
# <a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a><span data-ttu-id="01d48-103">Tutorial: Introdução com Signalr 1. x e MVC 4</span><span class="sxs-lookup"><span data-stu-id="01d48-103">Tutorial: Getting Started with SignalR 1.x and MVC 4</span></span>

<span data-ttu-id="01d48-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span><span class="sxs-lookup"><span data-stu-id="01d48-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="01d48-105">Este tutorial mostra como usar o Signalr ASP.NET para criar um aplicativo de chat em tempo real.</span><span class="sxs-lookup"><span data-stu-id="01d48-105">This tutorial shows how to use ASP.NET SignalR to create a real-time chat application.</span></span> <span data-ttu-id="01d48-106">Você adicionará o Signalr a um aplicativo MVC 4 e criará um modo de exibição de chat para enviar e exibir mensagens.</span><span class="sxs-lookup"><span data-stu-id="01d48-106">You will add SignalR to an MVC 4 application and create a chat view to send and display messages.</span></span>

## <a name="overview"></a><span data-ttu-id="01d48-107">Visão geral</span><span class="sxs-lookup"><span data-stu-id="01d48-107">Overview</span></span>

<span data-ttu-id="01d48-108">Este tutorial apresenta o desenvolvimento de aplicativos Web em tempo real com o ASP.NET Signalr e o ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="01d48-108">This tutorial introduces you to real-time web application development with ASP.NET SignalR and ASP.NET MVC 4.</span></span> <span data-ttu-id="01d48-109">O tutorial usa o mesmo código de aplicativo de chat que o [introdução o tutorial do signalr](tutorial-getting-started-with-signalr.md), mas mostra como adicioná-lo a um aplicativo MVC 4 com base no modelo de Internet.</span><span class="sxs-lookup"><span data-stu-id="01d48-109">The tutorial uses the same chat application code as the [SignalR Getting Started tutorial](tutorial-getting-started-with-signalr.md), but shows how to add it to an MVC 4 application based on the Internet template.</span></span>

<span data-ttu-id="01d48-110">Neste tópico, você aprenderá as seguintes tarefas de desenvolvimento do Signalr:</span><span class="sxs-lookup"><span data-stu-id="01d48-110">In this topic you will learn the following SignalR development tasks:</span></span>

- <span data-ttu-id="01d48-111">Adicionar a biblioteca do Signalr a um aplicativo MVC 4.</span><span class="sxs-lookup"><span data-stu-id="01d48-111">Adding the SignalR library to an MVC 4 application.</span></span>
- <span data-ttu-id="01d48-112">Criar uma classe de Hub para enviar conteúdo por push aos clientes.</span><span class="sxs-lookup"><span data-stu-id="01d48-112">Creating a hub class to push content to clients.</span></span>
- <span data-ttu-id="01d48-113">Usar a biblioteca do Signalr jQuery em uma página da Web para enviar mensagens e exibir atualizações do Hub.</span><span class="sxs-lookup"><span data-stu-id="01d48-113">Using the SignalR jQuery library in a web page to send messages and display updates from the hub.</span></span>

<span data-ttu-id="01d48-114">A captura de tela a seguir mostra o aplicativo de chat concluído em execução em um navegador.</span><span class="sxs-lookup"><span data-stu-id="01d48-114">The following screen shot shows the completed chat application running in a browser.</span></span>

![Instâncias de chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

<span data-ttu-id="01d48-116">As</span><span class="sxs-lookup"><span data-stu-id="01d48-116">Sections:</span></span>

- [<span data-ttu-id="01d48-117">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="01d48-117">Set up the Project</span></span>](#setup)
- [<span data-ttu-id="01d48-118">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="01d48-118">Run the Sample</span></span>](#run)
- [<span data-ttu-id="01d48-119">Examinar o código</span><span class="sxs-lookup"><span data-stu-id="01d48-119">Examine the Code</span></span>](#code)
- [<span data-ttu-id="01d48-120">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="01d48-120">Next Steps</span></span>](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a><span data-ttu-id="01d48-121">Configurar o projeto</span><span class="sxs-lookup"><span data-stu-id="01d48-121">Set up the Project</span></span>

<span data-ttu-id="01d48-122">Pré-requisitos:</span><span class="sxs-lookup"><span data-stu-id="01d48-122">Prerequisites:</span></span>

- <span data-ttu-id="01d48-123">Visual Studio 2010 SP1, Visual Studio 2012 ou Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="01d48-123">Visual Studio 2010 SP1, Visual Studio 2012, or Visual Studio 2012 Express.</span></span> <span data-ttu-id="01d48-124">Se você não tiver o Visual Studio, consulte [downloads do ASP.net](https://www.asp.net/downloads) para obter a ferramenta gratuita de desenvolvimento do Visual Studio 2012 Express.</span><span class="sxs-lookup"><span data-stu-id="01d48-124">If you do not have Visual Studio, see [ASP.NET Downloads](https://www.asp.net/downloads) to get the free Visual Studio 2012 Express Development Tool.</span></span>
- <span data-ttu-id="01d48-125">Para o Visual Studio 2010, instale o [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span><span class="sxs-lookup"><span data-stu-id="01d48-125">For Visual Studio 2010, install [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).</span></span>

<span data-ttu-id="01d48-126">Esta seção mostra como criar um aplicativo ASP.NET MVC 4, adicionar a biblioteca do Signalr e criar o aplicativo de chat.</span><span class="sxs-lookup"><span data-stu-id="01d48-126">This section shows how to create an ASP.NET MVC 4 application, add the SignalR library, and create the chat application.</span></span>

1. 1. <span data-ttu-id="01d48-127">No Visual Studio, crie um aplicativo ASP.NET MVC 4, nomeie-o SignalRChat e clique em OK.</span><span class="sxs-lookup"><span data-stu-id="01d48-127">In Visual Studio create an ASP.NET MVC 4 application, name it SignalRChat, and click OK.</span></span>

        > [!NOTE]
        > <span data-ttu-id="01d48-128">No VS 2010, selecione **.NET Framework 4** no controle suspenso de versão do Framework.</span><span class="sxs-lookup"><span data-stu-id="01d48-128">In VS 2010, select **.NET Framework 4** in the Framework version dropdown control.</span></span> <span data-ttu-id="01d48-129">O código do signalr é executado nas versões 4 e 4,5 do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="01d48-129">SignalR code runs on .NET Framework versions 4 and 4.5.</span></span>

        ![Criar Web MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. <span data-ttu-id="01d48-131">Selecione o modelo de aplicativo de Internet, desmarque a opção para **criar um projeto de teste de unidade**e clique em OK.</span><span class="sxs-lookup"><span data-stu-id="01d48-131">Select the Internet Application template, clear the option to **Create a unit test project**, and click OK.</span></span>

         ![Criar site da Internet MVC](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. <span data-ttu-id="01d48-133">Abra as **ferramentas > Gerenciador de pacotes NuGet > console do Gerenciador de pacotes** e execute o comando a seguir.</span><span class="sxs-lookup"><span data-stu-id="01d48-133">Open the **Tools > NuGet Package Manager > Package Manager Console** and run the following command.</span></span> <span data-ttu-id="01d48-134">Esta etapa adiciona ao projeto um conjunto de arquivos de script e referências de assembly que habilitam a funcionalidade do Signalr.</span><span class="sxs-lookup"><span data-stu-id="01d48-134">This step adds to the project a set of script files and assembly references that enable SignalR functionality.</span></span>

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. <span data-ttu-id="01d48-135">Em **Gerenciador de soluções** expanda a pasta scripts.</span><span class="sxs-lookup"><span data-stu-id="01d48-135">In **Solution Explorer** expand the Scripts folder.</span></span> <span data-ttu-id="01d48-136">Observe que as bibliotecas de script para Signalr foram adicionadas ao projeto.</span><span class="sxs-lookup"><span data-stu-id="01d48-136">Note that script libraries for SignalR have been added to the project.</span></span>

         ![Referências de biblioteca](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. <span data-ttu-id="01d48-138">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto, selecione **Adicionar | Nova pasta**e adicione uma nova pasta denominada **hubs**.</span><span class="sxs-lookup"><span data-stu-id="01d48-138">In **Solution Explorer**, right-click the project, select **Add | New Folder**, and add a new folder named **Hubs**.</span></span>
      6. <span data-ttu-id="01d48-139">Clique com o botão direito do mouse na pasta **hubs** e clique em **Adicionar |** E crie uma nova C# classe chamada **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="01d48-139">Right-click the **Hubs** folder, click **Add | Class**, and create a new C# class named **ChatHub.cs**.</span></span> <span data-ttu-id="01d48-140">Você usará essa classe como um hub de servidor Signalr que envia mensagens para todos os clientes.</span><span class="sxs-lookup"><span data-stu-id="01d48-140">You will use this class as a SignalR server hub that sends messages to all clients.</span></span>

> [!NOTE]
> <span data-ttu-id="01d48-141">Se você usar o Visual Studio 2012 e tiver instalado a [atualização do ASP.NET and Web Tools 2012,2](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), poderá usar o novo modelo de item do signalr para criar a classe Hub.</span><span class="sxs-lookup"><span data-stu-id="01d48-141">If you use Visual Studio 2012 and have installed the [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), you can use the new SignalR item template to create the hub class.</span></span> <span data-ttu-id="01d48-142">Para fazer isso, clique com o botão direito do mouse na pasta **hubs** e clique em **Adicionar | Novo item**, selecione **classe de Hub do signalr (v1)** e nomeie a classe **ChatHub.cs**.</span><span class="sxs-lookup"><span data-stu-id="01d48-142">To do that, right-click the **Hubs** folder, click **Add | New Item**, select **SignalR Hub Class (v1)**, and name the class **ChatHub.cs**.</span></span>

1. <span data-ttu-id="01d48-143">Substitua o código na classe **ChatHub** pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="01d48-143">Replace the code in the **ChatHub** class with the following code.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. <span data-ttu-id="01d48-144">Abra o arquivo **global. asax** para o projeto e adicione uma chamada para o método `RouteTable.Routes.MapHubs();` como a primeira linha de código no método `Application_Start`.</span><span class="sxs-lookup"><span data-stu-id="01d48-144">Open the **Global.asax** file for the project, and add a call to the method `RouteTable.Routes.MapHubs();` as the first line of code in the `Application_Start` method.</span></span> <span data-ttu-id="01d48-145">Esse código registra a rota padrão para hubs de sinalização e deve ser chamado antes de registrar qualquer outra rota.</span><span class="sxs-lookup"><span data-stu-id="01d48-145">This code registers the default route for SignalR hubs and must be called before you register any other routes.</span></span> <span data-ttu-id="01d48-146">O método `Application_Start` concluído é semelhante ao exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="01d48-146">The completed `Application_Start` method looks like the following example.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. <span data-ttu-id="01d48-147">Edite a classe `HomeController` encontrada em **Controllers/HomeController. cs** e adicione o método a seguir à classe.</span><span class="sxs-lookup"><span data-stu-id="01d48-147">Edit the `HomeController` class found in **Controllers/HomeController.cs** and add the following method to the class.</span></span> <span data-ttu-id="01d48-148">Esse método retorna o modo de exibição de **chat** que será criado em uma etapa posterior.</span><span class="sxs-lookup"><span data-stu-id="01d48-148">This method returns the **Chat** view that you will create in a later step.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. <span data-ttu-id="01d48-149">Clique com o botão direito do mouse no método de `Chat` que você acabou de criar e clique em **Adicionar exibição** para criar um novo arquivo de exibição.</span><span class="sxs-lookup"><span data-stu-id="01d48-149">Right-click within the `Chat` method you just created, and click **Add View** to create a new view file.</span></span>
5. <span data-ttu-id="01d48-150">No diálogo **Adicionar exibição** , verifique se a caixa de seleção está marcada para **usar um layout ou uma página mestra** (desmarque as outras caixas de seleção) e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="01d48-150">In the **Add View** dialog, make sure the check box is selected to **Use a layout or master page** (clear the other check boxes), and then click **Add**.</span></span>

    ![Adicionar uma exibição](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. <span data-ttu-id="01d48-152">Edite o novo arquivo de exibição chamado **chat. cshtml**.</span><span class="sxs-lookup"><span data-stu-id="01d48-152">Edit the new view file named **Chat.cshtml**.</span></span> <span data-ttu-id="01d48-153">Após a marcação &lt;H2&gt;, Cole a seguinte seção &lt;div&gt; e `@section scripts` bloco de código na página.</span><span class="sxs-lookup"><span data-stu-id="01d48-153">After the &lt;h2&gt; tag, paste the following &lt;div&gt; section and `@section scripts` code block into the page.</span></span> <span data-ttu-id="01d48-154">Esse script permite que a página envie mensagens de chat e exiba mensagens do servidor.</span><span class="sxs-lookup"><span data-stu-id="01d48-154">This script enables the page to send chat messages and display messages from the server.</span></span> <span data-ttu-id="01d48-155">O código completo para o modo de exibição de chat aparece no bloco de código a seguir.</span><span class="sxs-lookup"><span data-stu-id="01d48-155">The complete code for the chat view appears in the following code block.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="01d48-156">Quando você adiciona o Signalr e outras bibliotecas de scripts ao seu projeto do Visual Studio, o Gerenciador de pacotes pode instalar versões dos scripts mais recentes do que as versões mostradas neste tópico.</span><span class="sxs-lookup"><span data-stu-id="01d48-156">When you add SignalR and other script libraries to your Visual Studio project, the Package Manager might install versions of the scripts that are more recent than the versions shown in this topic.</span></span> <span data-ttu-id="01d48-157">Certifique-se de que as referências de script no seu código correspondam às versões das bibliotecas de script instaladas em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="01d48-157">Make sure that the script references in your code match the versions of the script libraries installed in your project.</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. <span data-ttu-id="01d48-158">**Salve tudo** para o projeto.</span><span class="sxs-lookup"><span data-stu-id="01d48-158">**Save All** for the project.</span></span>

<a id="run"></a>

## <a name="run-the-sample"></a><span data-ttu-id="01d48-159">Executar o exemplo</span><span class="sxs-lookup"><span data-stu-id="01d48-159">Run the Sample</span></span>

1. <span data-ttu-id="01d48-160">Pressione F5 para executar o projeto no modo de depuração.</span><span class="sxs-lookup"><span data-stu-id="01d48-160">Press F5 to run the project in debug mode.</span></span>
2. <span data-ttu-id="01d48-161">Na linha de endereço do navegador, acrescente **/Home/chat** à URL da página padrão do projeto.</span><span class="sxs-lookup"><span data-stu-id="01d48-161">In the browser address line, append **/home/chat** to the URL of the default page for the project.</span></span> <span data-ttu-id="01d48-162">A página de chat é carregada em uma instância do navegador e solicita um nome de usuário.</span><span class="sxs-lookup"><span data-stu-id="01d48-162">The Chat page loads in a browser instance and prompts for a user name.</span></span>

    ![Inserir nome de usuário](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. <span data-ttu-id="01d48-164">Insira um nome de usuário.</span><span class="sxs-lookup"><span data-stu-id="01d48-164">Enter a user name.</span></span>
4. <span data-ttu-id="01d48-165">Copie a URL da linha de endereço do navegador e use-a para abrir mais duas instâncias do navegador.</span><span class="sxs-lookup"><span data-stu-id="01d48-165">Copy the URL from the address line of the browser and use it to open two more browser instances.</span></span> <span data-ttu-id="01d48-166">Em cada instância do navegador, insira um nome de usuário exclusivo.</span><span class="sxs-lookup"><span data-stu-id="01d48-166">In each browser instance, enter a unique user name.</span></span>
5. <span data-ttu-id="01d48-167">Em cada instância do navegador, adicione um comentário e clique em **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="01d48-167">In each browser instance, add a comment and click **Send**.</span></span> <span data-ttu-id="01d48-168">Os comentários devem ser exibidos em todas as instâncias do navegador.</span><span class="sxs-lookup"><span data-stu-id="01d48-168">The comments should display in all browser instances.</span></span>

    > [!NOTE]
    > <span data-ttu-id="01d48-169">Esse aplicativo de chat simples não mantém o contexto de discussão no servidor.</span><span class="sxs-lookup"><span data-stu-id="01d48-169">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="01d48-170">O Hub transmite comentários para todos os usuários atuais.</span><span class="sxs-lookup"><span data-stu-id="01d48-170">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="01d48-171">Os usuários que ingressarem no chat mais tarde verão as mensagens adicionadas a partir do momento em que ingressarem.</span><span class="sxs-lookup"><span data-stu-id="01d48-171">Users who join the chat later will see messages added from the time they join.</span></span>
6. <span data-ttu-id="01d48-172">A captura de tela a seguir mostra o aplicativo de chat em execução em um navegador.</span><span class="sxs-lookup"><span data-stu-id="01d48-172">The following screen shot shows the chat application running in a browser.</span></span>

    ![Navegadores de chat](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. <span data-ttu-id="01d48-174">Em **Gerenciador de soluções**, inspecione o nó **documentos de script** para o aplicativo em execução.</span><span class="sxs-lookup"><span data-stu-id="01d48-174">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="01d48-175">Esse nó ficará visível no modo de depuração se você estiver usando o Internet Explorer como seu navegador.</span><span class="sxs-lookup"><span data-stu-id="01d48-175">This node is visible in debug mode if you are using Internet Explorer as your browser.</span></span> <span data-ttu-id="01d48-176">Há um arquivo de script chamado **hubs** que a biblioteca do signalr gera dinamicamente no tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="01d48-176">There is a script file named **hubs** that the SignalR library dynamically generates at runtime.</span></span> <span data-ttu-id="01d48-177">Esse arquivo gerencia a comunicação entre o script do jQuery e o código do lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="01d48-177">This file manages the communication between jQuery script and server-side code.</span></span> <span data-ttu-id="01d48-178">Se você usar um navegador diferente do Internet Explorer, também poderá acessar o arquivo de **hubs** dinâmicos navegando até ele diretamente, por exemplo http://mywebsite/signalr/hubs.</span><span class="sxs-lookup"><span data-stu-id="01d48-178">If you use a browser other than Internet Explorer, you can also access the dynamic **hubs** file by browsing to it directly, for example http://mywebsite/signalr/hubs.</span></span>

    ![Script de Hub gerado](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a><span data-ttu-id="01d48-180">Examinar o código</span><span class="sxs-lookup"><span data-stu-id="01d48-180">Examine the Code</span></span>

<span data-ttu-id="01d48-181">O aplicativo de chat do Signalr demonstra duas tarefas básicas de desenvolvimento do Signalr: criar um hub como o objeto principal de coordenação no servidor e usar a biblioteca do Signalr jQuery para enviar e receber mensagens.</span><span class="sxs-lookup"><span data-stu-id="01d48-181">The SignalR chat application demonstrates two basic SignalR development tasks: creating a hub as the main coordination object on the server, and using the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs"></a><span data-ttu-id="01d48-182">Hubs de sinalização</span><span class="sxs-lookup"><span data-stu-id="01d48-182">SignalR Hubs</span></span>

<span data-ttu-id="01d48-183">No exemplo de código, a classe **ChatHub** deriva da classe **Microsoft. AspNet. signalr. Hub** .</span><span class="sxs-lookup"><span data-stu-id="01d48-183">In the code sample the **ChatHub** class derives from the **Microsoft.AspNet.SignalR.Hub** class.</span></span> <span data-ttu-id="01d48-184">Derivar da classe **Hub** é uma maneira útil de criar um aplicativo signalr.</span><span class="sxs-lookup"><span data-stu-id="01d48-184">Deriving from the **Hub** class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="01d48-185">Você pode criar métodos públicos em sua classe Hub e, em seguida, acessar esses métodos chamando-os de scripts jQuery em uma página da Web.</span><span class="sxs-lookup"><span data-stu-id="01d48-185">You can create public methods on your hub class and then access those methods by calling them from jQuery scripts in a web page.</span></span>

<span data-ttu-id="01d48-186">No código do chat, os clientes chamam o método **ChatHub. Send** para enviar uma nova mensagem.</span><span class="sxs-lookup"><span data-stu-id="01d48-186">In the chat code, clients call the **ChatHub.Send** method to send a new message.</span></span> <span data-ttu-id="01d48-187">O Hub, por sua vez, envia a mensagem a todos os clientes chamando **clientes. All. addNewMessageToPage**.</span><span class="sxs-lookup"><span data-stu-id="01d48-187">The hub in turn sends the message to all clients by calling **Clients.All.addNewMessageToPage**.</span></span>

<span data-ttu-id="01d48-188">O método **Send** demonstra vários conceitos de Hub:</span><span class="sxs-lookup"><span data-stu-id="01d48-188">The **Send** method demonstrates several hub concepts :</span></span>

- <span data-ttu-id="01d48-189">Declare métodos públicos em um hub para que os clientes possam chamá-los.</span><span class="sxs-lookup"><span data-stu-id="01d48-189">Declare public methods on a hub so that clients can call them.</span></span>
- <span data-ttu-id="01d48-190">Use a propriedade **Microsoft. AspNet. signalr. Hub. clients** para acessar todos os clientes conectados a esse Hub.</span><span class="sxs-lookup"><span data-stu-id="01d48-190">Use the **Microsoft.AspNet.SignalR.Hub.Clients** property to access all clients connected to this hub.</span></span>
- <span data-ttu-id="01d48-191">Chame uma função do jQuery no cliente (como a função `addNewMessageToPage`) para atualizar clientes.</span><span class="sxs-lookup"><span data-stu-id="01d48-191">Call a jQuery function on the client (such as the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a><span data-ttu-id="01d48-192">Signalr e jQuery</span><span class="sxs-lookup"><span data-stu-id="01d48-192">SignalR and jQuery</span></span>

<span data-ttu-id="01d48-193">O arquivo de exibição **chat. cshtml** no exemplo de código mostra como usar a biblioteca do signalr jQuery para se comunicar com um Hub do signalr.</span><span class="sxs-lookup"><span data-stu-id="01d48-193">The **Chat.cshtml** view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span> <span data-ttu-id="01d48-194">As tarefas essenciais no código estão criando uma referência para o proxy gerado automaticamente para o Hub, declarando uma função que o servidor pode chamar para enviar conteúdo por push aos clientes e iniciando uma conexão de envio de mensagens para o Hub.</span><span class="sxs-lookup"><span data-stu-id="01d48-194">The essential tasks in the code are creating a reference to the auto-generated proxy for the hub, declaring a function that the server can call to push content to clients, and starting a connection to send messages to the hub.</span></span>

<span data-ttu-id="01d48-195">O código a seguir declara um proxy para um Hub.</span><span class="sxs-lookup"><span data-stu-id="01d48-195">The following code declares a proxy for a hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="01d48-196">No jQuery, a referência à classe de servidor e seus membros estão no camel case.</span><span class="sxs-lookup"><span data-stu-id="01d48-196">In jQuery the reference to the server class and its members is in camel case.</span></span> <span data-ttu-id="01d48-197">O exemplo de código faz C# referência à classe **ChatHub** no jQuery como **ChatHub**.</span><span class="sxs-lookup"><span data-stu-id="01d48-197">The code sample references the C# **ChatHub** class in jQuery as **chatHub**.</span></span> <span data-ttu-id="01d48-198">Se você quiser fazer referência à classe `ChatHub` no jQuery com a capitalização convencional do Pascal como C#faria no, edite o arquivo da classe ChatHub.cs.</span><span class="sxs-lookup"><span data-stu-id="01d48-198">If you want to reference the `ChatHub` class in jQuery with conventional Pascal casing as you would in C#, edit the ChatHub.cs class file.</span></span> <span data-ttu-id="01d48-199">Adicione uma instrução `using` para fazer referência ao namespace `Microsoft.AspNet.SignalR.Hubs`.</span><span class="sxs-lookup"><span data-stu-id="01d48-199">Add a `using` statement to reference the `Microsoft.AspNet.SignalR.Hubs` namespace.</span></span> <span data-ttu-id="01d48-200">Em seguida, adicione o atributo `HubName` à classe `ChatHub`, por exemplo `[HubName("ChatHub")]`.</span><span class="sxs-lookup"><span data-stu-id="01d48-200">Then add the `HubName` attribute to the `ChatHub` class, for example `[HubName("ChatHub")]`.</span></span> <span data-ttu-id="01d48-201">Por fim, atualize sua referência do jQuery para a classe `ChatHub`.</span><span class="sxs-lookup"><span data-stu-id="01d48-201">Finally, update your jQuery reference to the `ChatHub` class.</span></span>

<span data-ttu-id="01d48-202">O código a seguir mostra como criar uma função de retorno de chamada no script.</span><span class="sxs-lookup"><span data-stu-id="01d48-202">The following code shows how to create a callback function in the script.</span></span> <span data-ttu-id="01d48-203">A classe Hub no servidor chama essa função para enviar por push atualizações de conteúdo para cada cliente.</span><span class="sxs-lookup"><span data-stu-id="01d48-203">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="01d48-204">A chamada opcional para a função `htmlEncode` mostra uma maneira de codificar o conteúdo da mensagem em HTML antes de exibi-la na página, como uma maneira de evitar a injeção de script.</span><span class="sxs-lookup"><span data-stu-id="01d48-204">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page, as a way to prevent script injection.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

<span data-ttu-id="01d48-205">O código a seguir mostra como abrir uma conexão com o Hub.</span><span class="sxs-lookup"><span data-stu-id="01d48-205">The following code shows how to open a connection with the hub.</span></span> <span data-ttu-id="01d48-206">O código inicia a conexão e, em seguida, passa a ela uma função para manipular o evento de clique no botão **Enviar** na página de bate-papo.</span><span class="sxs-lookup"><span data-stu-id="01d48-206">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

> [!NOTE]
> <span data-ttu-id="01d48-207">Essa abordagem garante que a conexão seja estabelecida antes que o manipulador de eventos seja executado.</span><span class="sxs-lookup"><span data-stu-id="01d48-207">This approach ensures that the connection is established before the event handler executes.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a><span data-ttu-id="01d48-208">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="01d48-208">Next Steps</span></span>

<span data-ttu-id="01d48-209">Você aprendeu que o Signalr é uma estrutura para a criação de aplicativos Web em tempo real.</span><span class="sxs-lookup"><span data-stu-id="01d48-209">You learned that SignalR is a framework for building real-time web applications.</span></span> <span data-ttu-id="01d48-210">Você também aprendeu várias tarefas de desenvolvimento do Signalr: como adicionar o Signalr a um aplicativo ASP.NET, como criar uma classe de Hub e como enviar e receber mensagens do Hub.</span><span class="sxs-lookup"><span data-stu-id="01d48-210">You also learned several SignalR development tasks: how to add SignalR to an ASP.NET application, how to create a hub class, and how to send and receive messages from the hub.</span></span>

<span data-ttu-id="01d48-211">Para saber mais sobre os conceitos de desenvolvimento do Signalr avançado, visite os seguintes sites para código-fonte do Signalr e recursos:</span><span class="sxs-lookup"><span data-stu-id="01d48-211">To learn more advanced SignalR developments concepts, visit the following sites for SignalR source code and resources :</span></span>

- [<span data-ttu-id="01d48-212">Projeto do signalr</span><span class="sxs-lookup"><span data-stu-id="01d48-212">SignalR Project</span></span>](http://signalr.net)
- [<span data-ttu-id="01d48-213">GitHub e exemplos do signalr</span><span class="sxs-lookup"><span data-stu-id="01d48-213">SignalR Github and Samples</span></span>](https://github.com/SignalR/SignalR)
- [<span data-ttu-id="01d48-214">Wiki do signalr</span><span class="sxs-lookup"><span data-stu-id="01d48-214">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)
