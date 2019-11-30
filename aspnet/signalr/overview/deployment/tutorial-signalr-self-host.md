---
uid: signalr/overview/deployment/tutorial-signalr-self-host
title: 'Tutorial: auto-host do Signalr | Microsoft Docs'
author: bradygaster
description: Este tutorial mostra como criar um servidor Signaler 2 auto-hospedado e como se conectar a ele com um cliente JavaScript. Versões de software usadas no tutorial V...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 400db427-27af-4f2f-abf0-5486d5e024b5
msc.legacyurl: /signalr/overview/deployment/tutorial-signalr-self-host
msc.type: authoredcontent
ms.openlocfilehash: 41c8c3803923e76ef238a5c5937cbe7f81e6aa82
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74578573"
---
# <a name="tutorial-signalr-self-host"></a><span data-ttu-id="e4097-104">Tutorial: auto-host do Signalr</span><span class="sxs-lookup"><span data-stu-id="e4097-104">Tutorial: SignalR Self-Host</span></span>

<span data-ttu-id="e4097-105">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="e4097-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[<span data-ttu-id="e4097-106">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="e4097-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/SignalR-Self-Host-Sample-6da0f383)

> <span data-ttu-id="e4097-107">Este tutorial mostra como criar um servidor Signaler 2 auto-hospedado e como se conectar a ele com um cliente JavaScript.</span><span class="sxs-lookup"><span data-stu-id="e4097-107">This tutorial shows how to create a self-hosted SignalR 2 server, and how to connect to it with a JavaScript client.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e4097-108">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="e4097-108">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="e4097-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e4097-109">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="e4097-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="e4097-110">.NET 4.5</span></span>
> - <span data-ttu-id="e4097-111">Sinalização versão 2</span><span class="sxs-lookup"><span data-stu-id="e4097-111">SignalR version 2</span></span>
>
>
>
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="e4097-112">Usando o Visual Studio 2012 com este tutorial</span><span class="sxs-lookup"><span data-stu-id="e4097-112">Using Visual Studio 2012 with this tutorial</span></span>
>
>
> <span data-ttu-id="e4097-113">Para usar o Visual Studio 2012 com este tutorial, faça o seguinte:</span><span class="sxs-lookup"><span data-stu-id="e4097-113">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
>
> - <span data-ttu-id="e4097-114">Atualize o [Gerenciador de pacotes](http://docs.nuget.org/docs/start-here/installing-nuget) para a versão mais recente.</span><span class="sxs-lookup"><span data-stu-id="e4097-114">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="e4097-115">Instale o [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="e4097-115">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="e4097-116">No Web Platform Installer, procure e instale o **ASP.NET and Web Tools 2013,1 para Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="e4097-116">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="e4097-117">Isso instalará modelos do Visual Studio para classes de Signalr, como **Hub**.</span><span class="sxs-lookup"><span data-stu-id="e4097-117">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="e4097-118">Alguns modelos (como a **classe de inicialização OWIN**) não estarão disponíveis; para isso, use um arquivo de classe em vez disso.</span><span class="sxs-lookup"><span data-stu-id="e4097-118">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="e4097-119">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="e4097-119">Questions and comments</span></span>
>
> <span data-ttu-id="e4097-120">Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="e4097-120">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="e4097-121">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="e4097-121">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="e4097-122">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="e4097-122">Overview</span></span>

<span data-ttu-id="e4097-123">Geralmente, um servidor de sinalização é hospedado em um aplicativo ASP.NET no IIS, mas também pode ser hospedado internamente (como em um aplicativo de console ou serviço do Windows) usando a biblioteca de autohost.</span><span class="sxs-lookup"><span data-stu-id="e4097-123">A SignalR server is usually hosted in an ASP.NET application in IIS, but it can also be self-hosted (such as in a console application or Windows service) using the self-host library.</span></span> <span data-ttu-id="e4097-124">Essa biblioteca, como todo o Signalr 2, se baseia no OWIN ([Open Web interface para .net](http://owin.org)).</span><span class="sxs-lookup"><span data-stu-id="e4097-124">This library, like all of SignalR 2, is built on OWIN ([Open Web Interface for .NET](http://owin.org)).</span></span> <span data-ttu-id="e4097-125">OWIN define uma abstração entre os servidores Web e os aplicativos Web do .NET.</span><span class="sxs-lookup"><span data-stu-id="e4097-125">OWIN defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="e4097-126">OWIN dissocia o aplicativo Web do servidor, o que torna o OWIN ideal para hospedar internamente um aplicativo Web em seu próprio processo, fora do IIS.</span><span class="sxs-lookup"><span data-stu-id="e4097-126">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>

<span data-ttu-id="e4097-127">Os motivos para não hospedar no IIS incluem:</span><span class="sxs-lookup"><span data-stu-id="e4097-127">Reasons for not hosting in IIS include:</span></span>

- <span data-ttu-id="e4097-128">Ambientes em que o IIS não está disponível ou é desejável, como um farm de servidores existente sem o IIS.</span><span class="sxs-lookup"><span data-stu-id="e4097-128">Environments where IIS is not available or desirable, such as an existing server farm without IIS.</span></span>
- <span data-ttu-id="e4097-129">A sobrecarga de desempenho do IIS precisa ser evitada.</span><span class="sxs-lookup"><span data-stu-id="e4097-129">The performance overhead of IIS needs to be avoided.</span></span>
- <span data-ttu-id="e4097-130">A funcionalidade do signalr deve ser adicionada a um aplicativo existente que é executado em um serviço do Windows, na função de trabalho do Azure ou em outro processo.</span><span class="sxs-lookup"><span data-stu-id="e4097-130">SignalR functionality is to be added to an existing application that runs in a Windows Service, Azure worker role, or other process.</span></span>

<span data-ttu-id="e4097-131">Se uma solução estiver sendo desenvolvida como hospedagem interna por motivos de desempenho, é recomendável também testar o aplicativo hospedado no IIS para determinar o benefício do desempenho.</span><span class="sxs-lookup"><span data-stu-id="e4097-131">If a solution is being developed as self-host for performance reasons, it's recommended to also test the application hosted in IIS to determine the performance benefit.</span></span>

<span data-ttu-id="e4097-132">Este tutorial contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="e4097-132">This tutorial contains the following sections:</span></span>

- [<span data-ttu-id="e4097-133">Criando o servidor</span><span class="sxs-lookup"><span data-stu-id="e4097-133">Creating the server</span></span>](#server)
- [<span data-ttu-id="e4097-134">Acessando o servidor com um cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="e4097-134">Accessing the server with a JavaScript client</span></span>](#js)

<a id="server"></a>

## <a name="creating-the-server"></a><span data-ttu-id="e4097-135">Criando o servidor</span><span class="sxs-lookup"><span data-stu-id="e4097-135">Creating the server</span></span>

<span data-ttu-id="e4097-136">Neste tutorial, você criará um servidor que está hospedado em um aplicativo de console, mas o servidor pode ser hospedado em qualquer tipo de processo, como um serviço do Windows ou uma função de trabalho do Azure.</span><span class="sxs-lookup"><span data-stu-id="e4097-136">In this tutorial, you'll create a server that's hosted in a console application, but the server can be hosted in any sort of process, such as a Windows service or Azure worker role.</span></span> <span data-ttu-id="e4097-137">Para obter um exemplo de código para hospedar um servidor de sinalização em um serviço do Windows, consulte [auto-hospedagem de signalr em um serviço do Windows](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span><span class="sxs-lookup"><span data-stu-id="e4097-137">For sample code for hosting a SignalR server in a Windows Service, see [Self-Hosting SignalR in a Windows Service](https://code.msdn.microsoft.com/SignalR-self-hosted-in-6ff7e6c3).</span></span>

1. <span data-ttu-id="e4097-138">Abra Visual Studio 2013 com privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="e4097-138">Open Visual Studio 2013 with administrator privileges.</span></span> <span data-ttu-id="e4097-139">Selecione **arquivo**, **novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="e4097-139">Select **File**, **New Project**.</span></span> <span data-ttu-id="e4097-140">Selecione **Windows** no nó **Visual C#**  no painel **modelos** e selecione o modelo aplicativo de **console** .</span><span class="sxs-lookup"><span data-stu-id="e4097-140">Select **Windows** under the **Visual C#** node in the **Templates** pane, and select the **Console Application** template.</span></span> <span data-ttu-id="e4097-141">Nomeie o novo projeto como "SignalRSelfHost" e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4097-141">Name the new project "SignalRSelfHost" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image1.png)
2. <span data-ttu-id="e4097-142">Abra o console do Gerenciador de pacotes NuGet selecionando **ferramentas** > **Gerenciador de pacotes NuGet** > **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="e4097-142">Open the NuGet package manager console by selecting **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
3. <span data-ttu-id="e4097-143">No console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="e4097-143">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample1.ps1)]

    <span data-ttu-id="e4097-144">Esse comando adiciona as bibliotecas de hospedagem interna do Signalr 2 ao projeto.</span><span class="sxs-lookup"><span data-stu-id="e4097-144">This command adds the SignalR 2 Self-Host libraries to the project.</span></span>
4. <span data-ttu-id="e4097-145">No console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="e4097-145">In the package manager console, enter the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample2.ps1)]

    <span data-ttu-id="e4097-146">Esse comando adiciona a biblioteca Microsoft. Owin. CORS ao projeto.</span><span class="sxs-lookup"><span data-stu-id="e4097-146">This command adds the Microsoft.Owin.Cors library to the project.</span></span> <span data-ttu-id="e4097-147">Essa biblioteca será usada para suporte entre domínios, que é necessário para aplicativos que hospedam o Signalr e um cliente de página da Web em domínios diferentes.</span><span class="sxs-lookup"><span data-stu-id="e4097-147">This library will be used for cross-domain support, which is required for applications that host SignalR and a web page client in different domains.</span></span> <span data-ttu-id="e4097-148">Como você hospedará o servidor do Signalr e o cliente Web em portas diferentes, isso significa que o domínio cruzado deve estar habilitado para comunicação entre esses componentes.</span><span class="sxs-lookup"><span data-stu-id="e4097-148">Since you'll be hosting the SignalR server and the web client on different ports, this means that cross-domain must be enabled for communication between these components.</span></span>
5. <span data-ttu-id="e4097-149">Substitua o conteúdo de Program.cs pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e4097-149">Replace the contents of Program.cs with the following code.</span></span>

    [!code-csharp[Main](tutorial-signalr-self-host/samples/sample3.cs)]

    <span data-ttu-id="e4097-150">O código acima inclui três classes:</span><span class="sxs-lookup"><span data-stu-id="e4097-150">The above code includes three classes:</span></span>

    - <span data-ttu-id="e4097-151">**Programa**, incluindo o método **Main** que define o caminho principal de execução.</span><span class="sxs-lookup"><span data-stu-id="e4097-151">**Program**, including the **Main** method defining the primary path of execution.</span></span> <span data-ttu-id="e4097-152">Nesse método, um aplicativo Web do tipo **Startup** é iniciado na URL especificada (`http://localhost:8080`).</span><span class="sxs-lookup"><span data-stu-id="e4097-152">In this method, a web application of type **Startup** is started at the specified URL (`http://localhost:8080`).</span></span> <span data-ttu-id="e4097-153">Se a segurança for necessária no ponto de extremidade, o SSL poderá ser implementado.</span><span class="sxs-lookup"><span data-stu-id="e4097-153">If security is required on the endpoint, SSL can be implemented.</span></span> <span data-ttu-id="e4097-154">Consulte [como: configurar uma porta com um certificado SSL](https://msdn.microsoft.com/library/ms733791.aspx) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="e4097-154">See [How to: Configure a Port with an SSL Certificate](https://msdn.microsoft.com/library/ms733791.aspx) for more information.</span></span>
    - <span data-ttu-id="e4097-155">**Inicialização**, a classe que contém a configuração para o servidor signalr (a única configuração que este tutorial usa é a chamada para `UseCors`) e a chamada para `MapSignalR`, que cria rotas para quaisquer objetos de Hub no projeto.</span><span class="sxs-lookup"><span data-stu-id="e4097-155">**Startup**, the class containing the configuration for the SignalR server (the only configuration this tutorial uses is the call to `UseCors`), and the call to `MapSignalR`, which creates routes for any Hub objects in the project.</span></span>
    - <span data-ttu-id="e4097-156">**MyHub**, a classe de Hub do signalr que o aplicativo fornecerá aos clientes.</span><span class="sxs-lookup"><span data-stu-id="e4097-156">**MyHub**, the SignalR Hub class that the application will provide to clients.</span></span> <span data-ttu-id="e4097-157">Essa classe tem um único método, **Send**, que os clientes chamarão para transmitir uma mensagem a todos os outros clientes conectados.</span><span class="sxs-lookup"><span data-stu-id="e4097-157">This class has a single method, **Send**, that clients will call to broadcast a message to all other connected clients.</span></span>
6. <span data-ttu-id="e4097-158">Compile e execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e4097-158">Compile and run the application.</span></span> <span data-ttu-id="e4097-159">O endereço que o servidor está executando deve aparecer em uma janela de console.</span><span class="sxs-lookup"><span data-stu-id="e4097-159">The address that the server is running should show in a console window.</span></span>

    ![](tutorial-signalr-self-host/_static/image2.png)
7. <span data-ttu-id="e4097-160">Se a execução falhar com a exceção `System.Reflection.TargetInvocationException was unhandled`, será necessário reiniciar o Visual Studio com privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="e4097-160">If execution fails with the exception `System.Reflection.TargetInvocationException was unhandled`, you will need to restart Visual Studio with administrator privileges.</span></span>
8. <span data-ttu-id="e4097-161">Pare o aplicativo antes de prosseguir para a próxima seção.</span><span class="sxs-lookup"><span data-stu-id="e4097-161">Stop the application before proceeding to the next section.</span></span>

<a id="js"></a>

## <a name="accessing-the-server-with-a-javascript-client"></a><span data-ttu-id="e4097-162">Acessando o servidor com um cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="e4097-162">Accessing the server with a JavaScript client</span></span>

<span data-ttu-id="e4097-163">Nesta seção, você usará o mesmo cliente JavaScript do [tutorial introdução](../getting-started/tutorial-getting-started-with-signalr.md).</span><span class="sxs-lookup"><span data-stu-id="e4097-163">In this section, you'll use the same JavaScript client from the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md).</span></span> <span data-ttu-id="e4097-164">Faremos apenas uma modificação no cliente, que é definir explicitamente a URL do Hub.</span><span class="sxs-lookup"><span data-stu-id="e4097-164">We'll only make one modification to the client, which is to explicitly define the hub URL.</span></span> <span data-ttu-id="e4097-165">Com um aplicativo auto-hospedado, o servidor pode não estar necessariamente no mesmo endereço que a URL de conexão (devido a proxies invertidos e balanceadores de carga), portanto, a URL precisa ser definida explicitamente.</span><span class="sxs-lookup"><span data-stu-id="e4097-165">With a self-hosted application, the server may not necessarily be at the same address as the connection URL (due to reverse proxies and load balancers), so the URL needs to be defined explicitly.</span></span>

1. <span data-ttu-id="e4097-166">Em **Gerenciador de soluções**, clique com o botão direito do mouse na solução e selecione **Adicionar**, **novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="e4097-166">In **Solution Explorer**, right-click on the solution and select **Add**, **New Project**.</span></span> <span data-ttu-id="e4097-167">Selecione o nó **da Web** e selecione o modelo de **aplicativo Web ASP.net** .</span><span class="sxs-lookup"><span data-stu-id="e4097-167">Select the **Web** node, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="e4097-168">Nomeie o projeto "JavascriptClient" e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="e4097-168">Name the project "JavascriptClient" and click **OK**.</span></span>

    ![](tutorial-signalr-self-host/_static/image3.png)
2. <span data-ttu-id="e4097-169">Selecione o modelo **vazio** e deixe as opções restantes desmarcadas.</span><span class="sxs-lookup"><span data-stu-id="e4097-169">Select the **Empty** template, and leave the remaining options unselected.</span></span> <span data-ttu-id="e4097-170">Selecione **criar projeto**.</span><span class="sxs-lookup"><span data-stu-id="e4097-170">Select **Create Project**.</span></span>

    ![](tutorial-signalr-self-host/_static/image4.png)
3. <span data-ttu-id="e4097-171">No console do Gerenciador de pacotes, selecione o projeto "JavascriptClient" na lista suspensa **projeto padrão** e execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="e4097-171">In the package manager console, select the "JavascriptClient" project in the **Default project** drop-down, and execute the following command:</span></span>

    [!code-powershell[Main](tutorial-signalr-self-host/samples/sample4.ps1)]

    <span data-ttu-id="e4097-172">Esse comando instala as bibliotecas Signalr e JQuery que você precisará no cliente.</span><span class="sxs-lookup"><span data-stu-id="e4097-172">This command installs the SignalR and JQuery libraries that you'll need in the client.</span></span>
4. <span data-ttu-id="e4097-173">Clique com o botão direito do mouse em seu projeto e selecione **Adicionar**, **novo item**.</span><span class="sxs-lookup"><span data-stu-id="e4097-173">Right-click on your project and select **Add**, **New Item**.</span></span> <span data-ttu-id="e4097-174">Selecione o nó **da Web** e selecione página HTML.</span><span class="sxs-lookup"><span data-stu-id="e4097-174">Select the **Web** node, and select HTML Page.</span></span> <span data-ttu-id="e4097-175">Nomeie a página **como default. html**.</span><span class="sxs-lookup"><span data-stu-id="e4097-175">Name the page **Default.html**.</span></span>

    ![](tutorial-signalr-self-host/_static/image5.png)
5. <span data-ttu-id="e4097-176">Substitua o conteúdo da nova página HTML pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="e4097-176">Replace the contents of the new HTML page with the following code.</span></span> <span data-ttu-id="e4097-177">Verifique se as referências de script aqui correspondem aos scripts na pasta scripts do projeto.</span><span class="sxs-lookup"><span data-stu-id="e4097-177">Verify that the script references here match the scripts in the Scripts folder of the project.</span></span>

    [!code-html[Main](tutorial-signalr-self-host/samples/sample5.html?highlight=31-32)]

    <span data-ttu-id="e4097-178">O código a seguir (realçado no exemplo de código acima) é a adição que você fez ao cliente usado no tutorial de introdução (além de atualizar o código para o Signalr versão 2 beta).</span><span class="sxs-lookup"><span data-stu-id="e4097-178">The following code (highlighted in the code sample above) is the addition that you've made to the client used in the Getting Stared tutorial (in addition to upgrading the code to SignalR version 2 beta).</span></span> <span data-ttu-id="e4097-179">Essa linha de código define explicitamente a URL de conexão base para o Signalr no servidor.</span><span class="sxs-lookup"><span data-stu-id="e4097-179">This line of code explicitly sets the base connection URL for SignalR on the server.</span></span>

    [!code-javascript[Main](tutorial-signalr-self-host/samples/sample6.js)]
6. <span data-ttu-id="e4097-180">Clique com o botão direito do mouse na solução e selecione **definir projetos de inicialização...** . Selecione o botão de opção **vários projetos de inicialização** e defina a **ação** de ambos os projetos como **Iniciar**.</span><span class="sxs-lookup"><span data-stu-id="e4097-180">Right-click on the solution, and select **Set Startup Projects...**. Select the **Multiple startup projects** radio button, and set both projects' **Action** to **Start**.</span></span>

    ![](tutorial-signalr-self-host/_static/image6.png)
7. <span data-ttu-id="e4097-181">Clique com o botão direito do mouse em "default. html" e selecione **definir como página inicial**.</span><span class="sxs-lookup"><span data-stu-id="e4097-181">Right-click on "Default.html" and select **Set As Start Page**.</span></span>
8. <span data-ttu-id="e4097-182">Execute o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e4097-182">Run the application.</span></span> <span data-ttu-id="e4097-183">O servidor e a página serão iniciados.</span><span class="sxs-lookup"><span data-stu-id="e4097-183">The server and page will launch.</span></span> <span data-ttu-id="e4097-184">Talvez seja necessário recarregar a página da Web (ou selecione **continuar** no depurador) se a página for carregada antes de o servidor ser iniciado.</span><span class="sxs-lookup"><span data-stu-id="e4097-184">You may need to reload the web page (or select **Continue** in the debugger) if the page loads before the server is started.</span></span>
9. <span data-ttu-id="e4097-185">No navegador, forneça um nome de usuário quando solicitado.</span><span class="sxs-lookup"><span data-stu-id="e4097-185">In the browser, provide a username when prompted.</span></span> <span data-ttu-id="e4097-186">Copie a URL da página em outra guia ou janela do navegador e forneça um nome de usuário diferente.</span><span class="sxs-lookup"><span data-stu-id="e4097-186">Copy the page's URL into another browser tab or window and provide a different username.</span></span> <span data-ttu-id="e4097-187">Você poderá enviar mensagens de um painel do navegador para o outro, como no tutorial de Introdução.</span><span class="sxs-lookup"><span data-stu-id="e4097-187">You will be able to send messages from one browser pane to the other, as in the Getting Started tutorial.</span></span>
