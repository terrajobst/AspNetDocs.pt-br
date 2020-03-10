---
uid: signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
title: Usando contadores de desempenho do Signalr em uma função Web do Azure | Microsoft Docs
author: guardrex
description: Como instalar e usar contadores de desempenho do Signalr em uma função Web do Azure.
keywords: ASP. NET, signalr, contador de desempenho, função Web do Azure
ms.author: bradyg
ms.date: 10/03/2018
ms.assetid: 2a127d3b-21ed-4cc9-bec0-cdab4e742a25
msc.legacyurl: /signalr/overview/performance/using-signalr-performance-counters-in-an-azure-web-role
msc.type: authoredcontent
ms.openlocfilehash: 969a2ce43a7cb8d649555daf282f900401c0c914
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578977"
---
# <a name="using-signalr-performance-counters-in-an-azure-web-role"></a><span data-ttu-id="4b33a-104">Usando contadores de desempenho do Signalr em uma função Web do Azure</span><span class="sxs-lookup"><span data-stu-id="4b33a-104">Using SignalR performance counters in an Azure Web Role</span></span>

<span data-ttu-id="4b33a-105">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4b33a-105">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="4b33a-106">Os contadores de desempenho do signalr são usados para monitorar o desempenho do aplicativo em uma função Web do Azure.</span><span class="sxs-lookup"><span data-stu-id="4b33a-106">SignalR performance counters are used to monitor your app's performance in an Azure Web Role.</span></span> <span data-ttu-id="4b33a-107">Os contadores são capturados por Diagnóstico do Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="4b33a-107">The counters are captured by Microsoft Azure Diagnostics.</span></span> <span data-ttu-id="4b33a-108">Você instala contadores de desempenho do Signalr no Azure com *signalr. exe*, a mesma ferramenta usada para aplicativos autônomos ou locais.</span><span class="sxs-lookup"><span data-stu-id="4b33a-108">You install SignalR performance counters on Azure with *signalr.exe*, the same tool used for standalone or on-premises apps.</span></span> <span data-ttu-id="4b33a-109">Como as funções do Azure são transitórias, você configura um aplicativo para instalar e registrar contadores de desempenho do Signalr na inicialização.</span><span class="sxs-lookup"><span data-stu-id="4b33a-109">Since Azure roles are transient, you configure an app to install and register SignalR performance counters upon startup.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4b33a-110">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="4b33a-110">Prerequisites</span></span>

* <span data-ttu-id="4b33a-111">Visual Studio 2015 ou [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span><span class="sxs-lookup"><span data-stu-id="4b33a-111">Visual Studio 2015 or [2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)</span></span>
* <span data-ttu-id="4b33a-112">[SDK do Microsoft Azure para Visual Studio](https://azure.microsoft.com/downloads/) **Observação: reinicie o computador depois de instalar o SDK.**</span><span class="sxs-lookup"><span data-stu-id="4b33a-112">[Microsoft Azure SDK for Visual Studio](https://azure.microsoft.com/downloads/) **Note: Restart your machine after installing the SDK.**</span></span>
* <span data-ttu-id="4b33a-113">Assinatura do Microsoft Azure: para se inscrever em uma conta de avaliação gratuita do Azure, consulte [avaliação gratuita do Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="4b33a-113">Microsoft Azure subscription: To sign up for a free Azure trial account, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="creating-an-azure-web-role-application-that-exposes-signalr-performance-counters"></a><span data-ttu-id="4b33a-114">Criando um aplicativo de função Web do Azure que expõe contadores de desempenho do Signalr</span><span class="sxs-lookup"><span data-stu-id="4b33a-114">Creating an Azure Web Role application that exposes SignalR performance counters</span></span>

1. <span data-ttu-id="4b33a-115">Abra o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b33a-115">Open Visual Studio.</span></span>

2. <span data-ttu-id="4b33a-116">No Visual Studio, selecione **Arquivo** > **Novo** > **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-116">In Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="4b33a-117">Na caixa de diálogo **novo projeto** , selecione a **categoria C# Visual** > **nuvem** à esquerda e, em seguida, selecione o modelo **serviço de nuvem do Azure** .</span><span class="sxs-lookup"><span data-stu-id="4b33a-117">In the **New Project** dialog box, select the **Visual C#** > **Cloud** category on the left, and then select the **Azure Cloud Service** template.</span></span> <span data-ttu-id="4b33a-118">Nomeie o aplicativo **SignalRPerfCounters** e selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-118">Name the app **SignalRPerfCounters** and select **OK**.</span></span>

   ![Novo aplicativo de nuvem](using-signalr-performance-counters-in-an-azure-web-role/_static/image1.png)

   > [!NOTE]
   > <span data-ttu-id="4b33a-120">Se você não vir a categoria de modelo de **nuvem** ou o modelo de **serviço de nuvem do Azure** , precisará instalar a carga de trabalho de **desenvolvimento do azure** para o Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="4b33a-120">If you don't see the **Cloud** template category or the **Azure Cloud Service** template, you need to install the **Azure development** workload for Visual Studio 2017.</span></span> <span data-ttu-id="4b33a-121">Escolha o link **abrir instalador do Visual Studio** no lado inferior esquerdo da caixa de diálogo **novo projeto** para abrir o instalador do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b33a-121">Choose the **Open Visual Studio Installer** link on the bottom-left side of the **New Project** dialog to open Visual Studio Installer.</span></span> <span data-ttu-id="4b33a-122">Selecione a carga de trabalho de **desenvolvimento do Azure** e, em seguida, escolha **Modificar** para iniciar a instalação da carga de trabalho.</span><span class="sxs-lookup"><span data-stu-id="4b33a-122">Select the **Azure development** workload, and then choose **Modify** to start installing the workload.</span></span>
   >
   > ![Carga de trabalho de desenvolvimento do Azure no Instalador do Visual Studio](using-signalr-performance-counters-in-an-azure-web-role/_static/azure-development-workload.png)

4. <span data-ttu-id="4b33a-124">Na caixa de diálogo **novo serviço de nuvem Microsoft Azure** , selecione **função Web ASP.net** e selecione o botão > para adicionar a função ao projeto.</span><span class="sxs-lookup"><span data-stu-id="4b33a-124">In the **New Microsoft Azure Cloud Service** dialog, select **ASP.NET Web Role** and select the > button to add the role to the project.</span></span> <span data-ttu-id="4b33a-125">Selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-125">Select **OK**.</span></span>

   ![Adicionar função Web ASP.NET](using-signalr-performance-counters-in-an-azure-web-role/_static/image2.png)

5. <span data-ttu-id="4b33a-127">Na caixa de diálogo **novo aplicativo Web ASP.net-WebRole1** , selecione o modelo **MVC** e, em seguida, selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-127">In the **New ASP.NET Web Application - WebRole1** dialog, select the **MVC** template, and then select **OK**.</span></span>

   ![Adicionar MVC e API Web](using-signalr-performance-counters-in-an-azure-web-role/_static/image3.png)

6. <span data-ttu-id="4b33a-129">Em **Gerenciador de soluções**, abra o arquivo *Diagnostics. wadcfgx* em **WebRole1**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-129">In **Solution Explorer**, open the *diagnostics.wadcfgx* file under **WebRole1**.</span></span>

   ![Gerenciador de Soluções Diagnostics. wadcfgx](using-signalr-performance-counters-in-an-azure-web-role/_static/image4.png)

7. <span data-ttu-id="4b33a-131">Substitua o conteúdo do arquivo pela configuração a seguir e salve o arquivo:</span><span class="sxs-lookup"><span data-stu-id="4b33a-131">Replace the contents of the file with the following configuration and save the file:</span></span>

   [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample1.xml)]

8. <span data-ttu-id="4b33a-132">Abra o **console do Gerenciador de pacotes** em **ferramentas** > **Gerenciador de pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-132">Open the **Package Manager Console** from **Tools** > **NuGet Package Manager**.</span></span> <span data-ttu-id="4b33a-133">Insira os seguintes comandos para instalar a versão mais recente do Signalr e o pacote de utilitários do Signalr:</span><span class="sxs-lookup"><span data-stu-id="4b33a-133">Enter the following commands to install the latest version of SignalR and the SignalR utilities package:</span></span>

   [!code-powershell[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample2.ps1)]

9. <span data-ttu-id="4b33a-134">Configure o aplicativo para instalar os contadores de desempenho do Signalr na instância de função quando ele for iniciado ou reciclado.</span><span class="sxs-lookup"><span data-stu-id="4b33a-134">Configure the app to install the SignalR performance counters into the role instance when it starts up or recycles.</span></span> <span data-ttu-id="4b33a-135">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **WebRole1** e selecione **Adicionar** > **nova pasta**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-135">In **Solution Explorer**, right-click on the **WebRole1** project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="4b33a-136">Nomeie a nova pasta de *inicialização*.</span><span class="sxs-lookup"><span data-stu-id="4b33a-136">Name the new folder *Startup*.</span></span>

   ![Adicionar pasta de inicialização](using-signalr-performance-counters-in-an-azure-web-role/_static/image5.png)

10. <span data-ttu-id="4b33a-138">Copie o arquivo *signalr. exe* (adicionado com o pacote **Microsoft. AspNet. Signaler. utils** ) de \<pasta do projeto >/SignalRPerfCounters/Packages/Microsoft.AspNet.SignalR.utils.\<versão >/Tools para a pasta de *inicialização* que você criou na etapa anterior.</span><span class="sxs-lookup"><span data-stu-id="4b33a-138">Copy the *signalr.exe* file (added with the **Microsoft.AspNet.SignalR.Utils** package) from \<project folder>/SignalRPerfCounters/packages/Microsoft.AspNet.SignalR.Utils.\<version>/tools to the *Startup* folder you created in the previous step.</span></span>

11. <span data-ttu-id="4b33a-139">Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta de *inicialização* e selecione **Adicionar** > **Item existente**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-139">In **Solution Explorer**, right-click the *Startup* folder and select **Add** > **Existing Item**.</span></span> <span data-ttu-id="4b33a-140">Na caixa de diálogo que aparece, selecione *signalr. exe* e selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-140">In the dialog that appears, select *signalr.exe* and select **Add**.</span></span>

    ![Adicionar signalr. exe ao projeto](using-signalr-performance-counters-in-an-azure-web-role/_static/image6.png)

12. <span data-ttu-id="4b33a-142">Clique com o botão direito do mouse na pasta de *inicialização* que você criou.</span><span class="sxs-lookup"><span data-stu-id="4b33a-142">Right-click on the *Startup* folder you created.</span></span> <span data-ttu-id="4b33a-143">Selecione **Adicionar** > **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-143">Select **Add** > **New Item**.</span></span> <span data-ttu-id="4b33a-144">Selecione o nó **geral** , selecione **arquivo de texto**e nomeie o novo item *SignalRPerfCounterInstall. cmd*.</span><span class="sxs-lookup"><span data-stu-id="4b33a-144">Select the **General** node, select **Text File**, and name the new item *SignalRPerfCounterInstall.cmd*.</span></span> <span data-ttu-id="4b33a-145">Esse arquivo de comando instalará os contadores de desempenho do Signalr na função Web.</span><span class="sxs-lookup"><span data-stu-id="4b33a-145">This command file will install the SignalR performance counters into the web role.</span></span>

    ![Criar arquivo em lotes de instalação do contador de desempenho do Signalr](using-signalr-performance-counters-in-an-azure-web-role/_static/image7.png)

13. <span data-ttu-id="4b33a-147">Quando o Visual Studio cria o arquivo *SignalRPerfCounterInstall. cmd* , ele será aberto automaticamente na janela principal.</span><span class="sxs-lookup"><span data-stu-id="4b33a-147">When Visual Studio creates the *SignalRPerfCounterInstall.cmd* file, it will automatically open in the main window.</span></span> <span data-ttu-id="4b33a-148">Substitua o conteúdo do arquivo pelo script a seguir e, em seguida, salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="4b33a-148">Replace the contents of the file with the following script, then save and close the file.</span></span> <span data-ttu-id="4b33a-149">Esse script executa o *signalr. exe*, que adiciona os contadores de desempenho do signalr à instância de função.</span><span class="sxs-lookup"><span data-stu-id="4b33a-149">This script executes *signalr.exe*, which adds the SignalR performance counters to the role instance.</span></span>

    [!code-console[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample3.cmd)]

14. <span data-ttu-id="4b33a-150">Selecione o arquivo *signalr. exe* em **Gerenciador de soluções**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-150">Select the *signalr.exe* file in **Solution Explorer**.</span></span> <span data-ttu-id="4b33a-151">Nas **Propriedades**do arquivo, defina **copiar para diretório de saída** como **copiar sempre**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-151">In the file's **Properties**, set **Copy to Output Directory** to **Copy Always**.</span></span>

    ![Defina copiar para diretório de saída como copiar sempre](using-signalr-performance-counters-in-an-azure-web-role/_static/image8.png)

15. <span data-ttu-id="4b33a-153">Repita a etapa anterior para o arquivo *SignalRPerfCounterInstall. cmd* .</span><span class="sxs-lookup"><span data-stu-id="4b33a-153">Repeat the previous step for the *SignalRPerfCounterInstall.cmd* file.</span></span>

16. <span data-ttu-id="4b33a-154">Clique com o botão direito do mouse no arquivo *SignalRPerfCounterInstall. cmd* e selecione **abrir com**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-154">Right-click on the *SignalRPerfCounterInstall.cmd* file and select **Open With**.</span></span> <span data-ttu-id="4b33a-155">Na caixa de diálogo exibida, selecione **Editor de binários** e selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-155">In the dialog that appears, select **Binary Editor** and select **OK**.</span></span>

    ![Abrir com editor binário](using-signalr-performance-counters-in-an-azure-web-role/_static/image9.png)

17. <span data-ttu-id="4b33a-157">No editor binário, selecione qualquer byte à esquerda no arquivo e exclua-os.</span><span class="sxs-lookup"><span data-stu-id="4b33a-157">In the binary editor, select any leading bytes in the file and delete them.</span></span> <span data-ttu-id="4b33a-158">Salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="4b33a-158">Save and close the file.</span></span>

    ![Excluir bytes à esquerda](using-signalr-performance-counters-in-an-azure-web-role/_static/image10.png)

18. <span data-ttu-id="4b33a-160">Abra o Service *Definition. csdef* e adicione uma tarefa de inicialização que execute o arquivo *SignalrPerfCounterInstall. cmd* quando o serviço for iniciado:</span><span class="sxs-lookup"><span data-stu-id="4b33a-160">Open *ServiceDefinition.csdef* and add a startup task that executes the *SignalrPerfCounterInstall.cmd* file when the service starts up:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample4.xml?highlight=4-7)]

19. <span data-ttu-id="4b33a-161">Abra `Views/Shared/_Layout.cshtml` e remova o script de pacote jQuery do final do arquivo.</span><span class="sxs-lookup"><span data-stu-id="4b33a-161">Open `Views/Shared/_Layout.cshtml` and remove the jQuery bundle script from the end of the file.</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample5.cshtml)]

20. <span data-ttu-id="4b33a-162">Adicione um cliente JavaScript que chama continuamente o método `increment` no servidor.</span><span class="sxs-lookup"><span data-stu-id="4b33a-162">Add a JavaScript client that continuously calls the `increment` method on the server.</span></span> <span data-ttu-id="4b33a-163">Abra `Views/Home/Index.cshtml` e substitua o conteúdo pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="4b33a-163">Open `Views/Home/Index.cshtml` and replace the contents with the following code:</span></span>

    [!code-cshtml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample6.cshtml)]

21. <span data-ttu-id="4b33a-164">Crie uma nova pasta no projeto **WebRole1** chamado *hubs*.</span><span class="sxs-lookup"><span data-stu-id="4b33a-164">Create a new folder in the **WebRole1** project named *Hubs*.</span></span> <span data-ttu-id="4b33a-165">Clique com o botão direito do mouse na pasta *hubs* em **Gerenciador de soluções** e selecione **Adicionar** > **novo item**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-165">Right-click the *Hubs* folder in **Solution Explorer** and select **Add** > **New Item**.</span></span> <span data-ttu-id="4b33a-166">Na caixa de diálogo **Adicionar novo item** , selecione **a categoria de** > **da Web** e, em seguida, selecione o modelo de item da **classe de Hub do signalr (v2)** .</span><span class="sxs-lookup"><span data-stu-id="4b33a-166">In the **Add New Item** dialog box, select the **Web** > **SignalR** category, and then select the **SignalR Hub Class (v2)** item template.</span></span> <span data-ttu-id="4b33a-167">Nomeie o novo *MyHub.cs* de Hub e selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-167">Name the new hub *MyHub.cs* and select **Add**.</span></span>

    ![Adicionando a classe de Hub do Signalr à pasta hubs na caixa de diálogo Adicionar novo item](using-signalr-performance-counters-in-an-azure-web-role/_static/image13.png)

22. <span data-ttu-id="4b33a-169">O *MyHub.cs* será aberto automaticamente na janela principal.</span><span class="sxs-lookup"><span data-stu-id="4b33a-169">*MyHub.cs* will automatically open in the main window.</span></span> <span data-ttu-id="4b33a-170">Substitua o conteúdo pelo código a seguir e, em seguida, salve e feche o arquivo:</span><span class="sxs-lookup"><span data-stu-id="4b33a-170">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample7.cs)]

23. <span data-ttu-id="4b33a-171">O *[pedais. exe](signalr-connection-density-testing-with-crank.md)* é uma ferramenta de teste de densidade de conexão fornecida com a base de código do signalr.</span><span class="sxs-lookup"><span data-stu-id="4b33a-171">*[Crank.exe](signalr-connection-density-testing-with-crank.md)* is a connection density testing tool provided with the SignalR codebase.</span></span> <span data-ttu-id="4b33a-172">Como o pedais requer uma conexão persistente, você adiciona uma ao seu site para uso durante o teste.</span><span class="sxs-lookup"><span data-stu-id="4b33a-172">Since Crank requires a persistent connection, you add one to your site for use when testing.</span></span> <span data-ttu-id="4b33a-173">Adicione uma nova pasta ao projeto **WebRole1** chamado *PersistentConnections*.</span><span class="sxs-lookup"><span data-stu-id="4b33a-173">Add a new folder to the **WebRole1** project called *PersistentConnections*.</span></span> <span data-ttu-id="4b33a-174">Clique com o botão direito do mouse nessa pasta e selecione **adicionar** > **classe**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-174">Right-click this folder and select **Add** > **Class**.</span></span> <span data-ttu-id="4b33a-175">Nomeie o novo arquivo de classe *MyPersistentConnections.cs* e selecione **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-175">Name the new class file *MyPersistentConnections.cs* and select **Add**.</span></span>

24. <span data-ttu-id="4b33a-176">O Visual Studio abrirá o arquivo *MyPersistentConnections.cs* na janela principal.</span><span class="sxs-lookup"><span data-stu-id="4b33a-176">Visual Studio will open the *MyPersistentConnections.cs* file in the main window.</span></span> <span data-ttu-id="4b33a-177">Substitua o conteúdo pelo código a seguir e, em seguida, salve e feche o arquivo:</span><span class="sxs-lookup"><span data-stu-id="4b33a-177">Replace the contents with the following code, then save and close the file:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample8.cs)]

25. <span data-ttu-id="4b33a-178">Usando a classe `Startup`, os objetos Signalr começam quando o OWIN é iniciado.</span><span class="sxs-lookup"><span data-stu-id="4b33a-178">Using the `Startup` class, the SignalR objects start when OWIN starts up.</span></span> <span data-ttu-id="4b33a-179">Abra ou crie *Startup.cs* e substitua o conteúdo pelo código a seguir:</span><span class="sxs-lookup"><span data-stu-id="4b33a-179">Open or create *Startup.cs* and replace the contents with the following code:</span></span>

    [!code-csharp[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample9.cs)]

    <span data-ttu-id="4b33a-180">No código acima, o atributo `OwinStartup` marca essa classe para iniciar OWIN.</span><span class="sxs-lookup"><span data-stu-id="4b33a-180">In the code above, the `OwinStartup` attribute marks this class to start OWIN.</span></span> <span data-ttu-id="4b33a-181">O método `Configuration` inicia o Signalr.</span><span class="sxs-lookup"><span data-stu-id="4b33a-181">The `Configuration` method starts SignalR.</span></span>

26. <span data-ttu-id="4b33a-182">Teste seu aplicativo no emulador de Microsoft Azure pressionando **F5**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-182">Test your application in the Microsoft Azure Emulator by pressing **F5**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4b33a-183">Se você encontrar um **FileLoadException** em **MapSignalR**, altere os redirecionamentos de associação em *Web. config* para o seguinte:</span><span class="sxs-lookup"><span data-stu-id="4b33a-183">If you encounter a **FileLoadException** at **MapSignalR**, change the binding redirects in *web.config* to the following:</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample12.xml?highlight=3,7)]

27. <span data-ttu-id="4b33a-184">Aguarde cerca de um minuto.</span><span class="sxs-lookup"><span data-stu-id="4b33a-184">Wait about one minute.</span></span> <span data-ttu-id="4b33a-185">Abra a janela de ferramentas do Cloud Explorer no Visual Studio (**exibir** > **Cloud Explorer**) e expanda o caminho `(Local)/Storage Accounts/(Development)/Tables`.</span><span class="sxs-lookup"><span data-stu-id="4b33a-185">Open the Cloud Explorer tool window in Visual Studio (**View** > **Cloud Explorer**) and expand the path `(Local)/Storage Accounts/(Development)/Tables`.</span></span> <span data-ttu-id="4b33a-186">Clique duas vezes em **WADPerformanceCountersTable**.</span><span class="sxs-lookup"><span data-stu-id="4b33a-186">Double-click **WADPerformanceCountersTable**.</span></span> <span data-ttu-id="4b33a-187">Você deve ver os contadores do Signalr nos dados da tabela.</span><span class="sxs-lookup"><span data-stu-id="4b33a-187">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="4b33a-188">Se você não vir a tabela, talvez seja necessário inserir novamente suas credenciais de armazenamento do Azure.</span><span class="sxs-lookup"><span data-stu-id="4b33a-188">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="4b33a-189">Talvez seja necessário selecionar o botão **Atualizar** para ver a tabela no **Cloud Explorer** ou selecionar o botão **Atualizar** na janela Abrir tabela para ver os dados na tabela.</span><span class="sxs-lookup"><span data-stu-id="4b33a-189">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

    ![Selecionando a tabela de contadores de desempenho WAD no Visual Studio Cloud Explorer](using-signalr-performance-counters-in-an-azure-web-role/_static/image11.png)

    ![Mostrando os contadores coletados na tabela de contadores de desempenho WAD](using-signalr-performance-counters-in-an-azure-web-role/_static/image12.png)

28. <span data-ttu-id="4b33a-192">Para testar seu aplicativo na nuvem, atualize o arquivo **. Cloud. cscfg** e defina o `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` como uma cadeia de conexão de conta de armazenamento do Azure válida.</span><span class="sxs-lookup"><span data-stu-id="4b33a-192">To test your application in the cloud, update the **ServiceConfiguration.Cloud.cscfg** file and set the `Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString` to a valid Azure Storage account connection string.</span></span>

    [!code-xml[Main](using-signalr-performance-counters-in-an-azure-web-role/samples/sample10.xml)]

29. <span data-ttu-id="4b33a-193">Implante o aplicativo em sua assinatura do Azure.</span><span class="sxs-lookup"><span data-stu-id="4b33a-193">Deploy the application to your Azure subscription.</span></span> <span data-ttu-id="4b33a-194">Para obter detalhes sobre como implantar um aplicativo no Azure, consulte [como criar e implantar um serviço de nuvem](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="4b33a-194">For details on how to deploy an application to Azure, see [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span>

30. <span data-ttu-id="4b33a-195">Aguarde alguns minutos.</span><span class="sxs-lookup"><span data-stu-id="4b33a-195">Wait a few minutes.</span></span> <span data-ttu-id="4b33a-196">No **Cloud Explorer**, localize a conta de armazenamento configurada acima e localize a tabela `WADPerformanceCountersTable` nela.</span><span class="sxs-lookup"><span data-stu-id="4b33a-196">In **Cloud Explorer**, locate the storage account you configured above and find the `WADPerformanceCountersTable` table in it.</span></span> <span data-ttu-id="4b33a-197">Você deve ver os contadores do Signalr nos dados da tabela.</span><span class="sxs-lookup"><span data-stu-id="4b33a-197">You should see SignalR counters in the table data.</span></span> <span data-ttu-id="4b33a-198">Se você não vir a tabela, talvez seja necessário inserir novamente suas credenciais de armazenamento do Azure.</span><span class="sxs-lookup"><span data-stu-id="4b33a-198">If you don't see the table, you may need to re-enter your Azure Storage credentials.</span></span> <span data-ttu-id="4b33a-199">Talvez seja necessário selecionar o botão **Atualizar** para ver a tabela no **Cloud Explorer** ou selecionar o botão **Atualizar** na janela Abrir tabela para ver os dados na tabela.</span><span class="sxs-lookup"><span data-stu-id="4b33a-199">You may need to select the **Refresh** button to see the table in **Cloud Explorer** or select the **Refresh** button in the open table window to see data in the table.</span></span>

<span data-ttu-id="4b33a-200">Obrigado especial a [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) pelo conteúdo original usado neste tutorial.</span><span class="sxs-lookup"><span data-stu-id="4b33a-200">Special thanks to [Martin Richard](https://social.msdn.microsoft.com/profile/Martin+Richard) for the original content used in this tutorial.</span></span>
