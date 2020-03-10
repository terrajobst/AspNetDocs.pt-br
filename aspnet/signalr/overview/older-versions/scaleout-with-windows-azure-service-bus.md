---
uid: signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
title: Expansão do signalr com o barramento de serviço do Azure (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 501db899-e68c-49ff-81b2-1dc561bfe908
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: e64f84db00b571c01ea52f48d1ac1af46698d391
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558411"
---
# <a name="signalr-scaleout-with-azure-service-bus-signalr-1x"></a><span data-ttu-id="449fa-102">Expansão do SignalR com o Barramento de Serviço do Azure (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="449fa-102">SignalR Scaleout with Azure Service Bus (SignalR 1.x)</span></span>

<span data-ttu-id="449fa-103">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="449fa-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="449fa-104">Neste tutorial, você implantará um aplicativo Signalr em uma função Web do Windows Azure, usando o backplane do barramento de serviço para distribuir mensagens para cada instância de função.</span><span class="sxs-lookup"><span data-stu-id="449fa-104">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="449fa-105">Pré-requisitos:</span><span class="sxs-lookup"><span data-stu-id="449fa-105">Prerequisites:</span></span>

- <span data-ttu-id="449fa-106">Uma conta do Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="449fa-106">A Windows Azure account.</span></span>
- <span data-ttu-id="449fa-107">O [SDK do Windows Azure](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="449fa-107">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="449fa-108">Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="449fa-108">Visual Studio 2012.</span></span>

<span data-ttu-id="449fa-109">O backplane do barramento de serviço também é compatível com o [barramento de serviço para Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versão 1,1.</span><span class="sxs-lookup"><span data-stu-id="449fa-109">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="449fa-110">No entanto, ele não é compatível com a versão 1,0 do barramento de serviço para Windows Server.</span><span class="sxs-lookup"><span data-stu-id="449fa-110">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="449fa-111">Preços</span><span class="sxs-lookup"><span data-stu-id="449fa-111">Pricing</span></span>

<span data-ttu-id="449fa-112">O backplane do barramento de serviço usa tópicos para enviar mensagens.</span><span class="sxs-lookup"><span data-stu-id="449fa-112">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="449fa-113">Para obter as informações de preços mais recentes, consulte [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="449fa-113">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="449fa-114">No momento da redação deste artigo, você pode enviar 1 milhão mensagens por mês para menos de $1.</span><span class="sxs-lookup"><span data-stu-id="449fa-114">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="449fa-115">O backplane envia uma mensagem do barramento de serviço para cada invocação de um método de Hub do Signalr.</span><span class="sxs-lookup"><span data-stu-id="449fa-115">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="449fa-116">Há também algumas mensagens de controle para conexões, desconexões, junção ou saída de grupos e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="449fa-116">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="449fa-117">Na maioria dos aplicativos, a maior parte do tráfego de mensagens será invocações de método de Hub.</span><span class="sxs-lookup"><span data-stu-id="449fa-117">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="449fa-118">Visão geral</span><span class="sxs-lookup"><span data-stu-id="449fa-118">Overview</span></span>

<span data-ttu-id="449fa-119">Antes de chegarmos ao tutorial detalhado, aqui está uma visão geral rápida do que você fará.</span><span class="sxs-lookup"><span data-stu-id="449fa-119">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="449fa-120">Use o portal do Azure do Windows para criar um novo namespace do barramento de serviço.</span><span class="sxs-lookup"><span data-stu-id="449fa-120">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="449fa-121">Adicione esses pacotes NuGet ao seu aplicativo:</span><span class="sxs-lookup"><span data-stu-id="449fa-121">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="449fa-122">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="449fa-122">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="449fa-123">Microsoft. AspNet. Signalr. ServiceBus</span><span class="sxs-lookup"><span data-stu-id="449fa-123">Microsoft.AspNet.SignalR.ServiceBus</span></span>](http://www.nuget.org/packages/SignalR.WindowsAzureServiceBus)
3. <span data-ttu-id="449fa-124">Crie um aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="449fa-124">Create a SignalR application.</span></span>
4. <span data-ttu-id="449fa-125">Adicione o seguinte código ao global. asax para configurar o backplane:</span><span class="sxs-lookup"><span data-stu-id="449fa-125">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="449fa-126">Para cada aplicativo, escolha um valor diferente para "Nomedoseuaplicativo".</span><span class="sxs-lookup"><span data-stu-id="449fa-126">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="449fa-127">Não use o mesmo valor em vários aplicativos.</span><span class="sxs-lookup"><span data-stu-id="449fa-127">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="449fa-128">Criar os serviços do Azure</span><span class="sxs-lookup"><span data-stu-id="449fa-128">Create the Azure Services</span></span>

<span data-ttu-id="449fa-129">Crie um serviço de nuvem, conforme descrito em [como criar e implantar um serviço de nuvem](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="449fa-129">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="449fa-130">Siga as etapas na seção "como criar um serviço de nuvem usando a criação rápida".</span><span class="sxs-lookup"><span data-stu-id="449fa-130">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="449fa-131">Para este tutorial, você não precisa carregar um certificado.</span><span class="sxs-lookup"><span data-stu-id="449fa-131">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="449fa-132">Crie um novo namespace do barramento de serviço, conforme descrito em [como usar os tópicos/assinaturas do barramento de serviço](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="449fa-132">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="449fa-133">Siga as etapas na seção "criar um namespace de serviço".</span><span class="sxs-lookup"><span data-stu-id="449fa-133">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="449fa-134">Certifique-se de selecionar a mesma região para o serviço de nuvem e o namespace do barramento de serviço.</span><span class="sxs-lookup"><span data-stu-id="449fa-134">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="449fa-135">Criar o projeto do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="449fa-135">Create the Visual Studio Project</span></span>

<span data-ttu-id="449fa-136">Inicie o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="449fa-136">Start Visual Studio.</span></span> <span data-ttu-id="449fa-137">No menu **Arquivo**, clique em **Novo Projeto**.</span><span class="sxs-lookup"><span data-stu-id="449fa-137">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="449fa-138">Na caixa de diálogo **novo projeto** , expanda **Visual C#** .</span><span class="sxs-lookup"><span data-stu-id="449fa-138">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="449fa-139">Em **modelos instalados**, selecione **nuvem** e, em seguida, selecione **serviço de nuvem do Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="449fa-139">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="449fa-140">Mantenha o .NET Framework padrão 4,5.</span><span class="sxs-lookup"><span data-stu-id="449fa-140">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="449fa-141">Nomeie o aplicativo ChatService e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="449fa-141">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="449fa-142">Na caixa de diálogo **novo serviço de nuvem do Windows Azure** , selecione função Web do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="449fa-142">In the **New Windows Azure Cloud Service** dialog, select ASP.NET MVC 4 Web Role.</span></span> <span data-ttu-id="449fa-143">Clique no botão de seta para a direita ( **&gt;** ) para adicionar a função à sua solução.</span><span class="sxs-lookup"><span data-stu-id="449fa-143">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="449fa-144">Passe o mouse sobre a nova função, portanto, o ícone de lápis está visível.</span><span class="sxs-lookup"><span data-stu-id="449fa-144">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="449fa-145">Clique nesse ícone para renomear a função.</span><span class="sxs-lookup"><span data-stu-id="449fa-145">Click this icon to rename the role.</span></span> <span data-ttu-id="449fa-146">Nomeie a função "SignalRChat" e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="449fa-146">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="449fa-147">No assistente **novo projeto do ASP.NET MVC 4** , selecione **aplicativo de Internet**.</span><span class="sxs-lookup"><span data-stu-id="449fa-147">In the **New ASP.NET MVC 4 Project** wizard, select **Internet Application**.</span></span> <span data-ttu-id="449fa-148">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="449fa-148">Click **OK**.</span></span> <span data-ttu-id="449fa-149">O assistente de projeto cria dois projetos:</span><span class="sxs-lookup"><span data-stu-id="449fa-149">The project wizard creates two projects:</span></span>

- <span data-ttu-id="449fa-150">ChatService: este projeto é o aplicativo do Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="449fa-150">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="449fa-151">Ele define as funções do Azure e outras opções de configuração.</span><span class="sxs-lookup"><span data-stu-id="449fa-151">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="449fa-152">SignalRChat: este projeto é seu projeto do ASP.NET MVC 4.</span><span class="sxs-lookup"><span data-stu-id="449fa-152">SignalRChat: This project is your ASP.NET MVC 4 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="449fa-153">Criar o aplicativo de chat do Signalr</span><span class="sxs-lookup"><span data-stu-id="449fa-153">Create the SignalR Chat Application</span></span>

<span data-ttu-id="449fa-154">Para criar o aplicativo de chat, siga as etapas no tutorial [introdução com signalr e MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span><span class="sxs-lookup"><span data-stu-id="449fa-154">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 4](tutorial-getting-started-with-signalr-and-mvc-4.md).</span></span>

<span data-ttu-id="449fa-155">Use o NuGet para instalar as bibliotecas necessárias.</span><span class="sxs-lookup"><span data-stu-id="449fa-155">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="449fa-156">No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="449fa-156">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="449fa-157">Na janela do **console do Gerenciador de pacotes** , digite os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="449fa-157">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="449fa-158">Use a opção `-ProjectName` para instalar os pacotes no projeto MVC do ASP.NET, em vez de no projeto do Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="449fa-158">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="449fa-159">Configurar o backplane</span><span class="sxs-lookup"><span data-stu-id="449fa-159">Configure the Backplane</span></span>

<span data-ttu-id="449fa-160">No arquivo global. asax do seu aplicativo, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="449fa-160">In your application's Global.asax file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="449fa-161">Agora você precisa obter a cadeia de conexão do barramento de serviço.</span><span class="sxs-lookup"><span data-stu-id="449fa-161">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="449fa-162">No portal do Azure, selecione o namespace do barramento de serviço que você criou e clique no ícone de chave de acesso.</span><span class="sxs-lookup"><span data-stu-id="449fa-162">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="449fa-163">Copie a cadeia de conexão para a área de transferência e cole-a na variável *ConnectionString* .</span><span class="sxs-lookup"><span data-stu-id="449fa-163">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="449fa-164">Implantar no Azure</span><span class="sxs-lookup"><span data-stu-id="449fa-164">Deploy to Azure</span></span>

<span data-ttu-id="449fa-165">Em Gerenciador de Soluções, expanda a pasta **funções** dentro do projeto ChatService.</span><span class="sxs-lookup"><span data-stu-id="449fa-165">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

<span data-ttu-id="449fa-166">Clique com o botão direito do mouse na função SignalRChat e selecione **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="449fa-166">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="449fa-167">Selecione a guia **configuração** . Em **instâncias** , selecione 2.</span><span class="sxs-lookup"><span data-stu-id="449fa-167">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="449fa-168">Você também pode definir o tamanho da VM para **extra pequena**.</span><span class="sxs-lookup"><span data-stu-id="449fa-168">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="449fa-169">Salve as alterações.</span><span class="sxs-lookup"><span data-stu-id="449fa-169">Save the changes.</span></span>

<span data-ttu-id="449fa-170">Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto ChatService.</span><span class="sxs-lookup"><span data-stu-id="449fa-170">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="449fa-171">Selecione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="449fa-171">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="449fa-172">Se esta for a primeira vez que você publica no Windows Azure, você deve baixar suas credenciais.</span><span class="sxs-lookup"><span data-stu-id="449fa-172">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="449fa-173">No assistente de **publicação** , clique em "entrar para baixar credenciais".</span><span class="sxs-lookup"><span data-stu-id="449fa-173">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="449fa-174">Isso solicitará que você entre no portal do Azure do Windows e baixe um arquivo de configurações de publicação.</span><span class="sxs-lookup"><span data-stu-id="449fa-174">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="449fa-175">Clique em **importar** e selecione o arquivo de configurações de publicação que você baixou.</span><span class="sxs-lookup"><span data-stu-id="449fa-175">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="449fa-176">Clique em **Próximo**.</span><span class="sxs-lookup"><span data-stu-id="449fa-176">Click **Next**.</span></span> <span data-ttu-id="449fa-177">Na caixa de diálogo **configurações de publicação** , em **serviço de nuvem**, selecione o serviço de nuvem que você criou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="449fa-177">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="449fa-178">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="449fa-178">Click **Publish**.</span></span> <span data-ttu-id="449fa-179">Pode levar alguns minutos para implantar o aplicativo e iniciar as VMs.</span><span class="sxs-lookup"><span data-stu-id="449fa-179">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="449fa-180">Agora, quando você executa o aplicativo de chat, as instâncias de função se comunicam por meio do barramento de serviço do Azure, usando um tópico do barramento de serviço.</span><span class="sxs-lookup"><span data-stu-id="449fa-180">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="449fa-181">Um tópico é uma fila de mensagens que permite vários assinantes.</span><span class="sxs-lookup"><span data-stu-id="449fa-181">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="449fa-182">O backplane cria automaticamente o tópico e as assinaturas.</span><span class="sxs-lookup"><span data-stu-id="449fa-182">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="449fa-183">Para ver a atividade de assinaturas e mensagens, abra o portal do Azure, selecione o namespace do barramento de serviço e clique em "tópicos".</span><span class="sxs-lookup"><span data-stu-id="449fa-183">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="449fa-184">Reserve alguns minutos para que a atividade de mensagem seja exibida no painel.</span><span class="sxs-lookup"><span data-stu-id="449fa-184">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="449fa-185">O signalr gerencia o tempo de vida do tópico.</span><span class="sxs-lookup"><span data-stu-id="449fa-185">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="449fa-186">Desde que seu aplicativo seja implantado, não tente excluir manualmente os tópicos ou alterar as configurações no tópico.</span><span class="sxs-lookup"><span data-stu-id="449fa-186">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>
