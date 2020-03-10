---
uid: signalr/overview/performance/scaleout-with-windows-azure-service-bus
title: Expansão do signalr com o barramento de serviço do Azure | Microsoft Docs
author: bradygaster
description: As versões de software usadas neste tópico Visual Studio 2013 o .NET 4,5 Signaler versão 2 versões anteriores deste tópico para a versão do Signalr 1. x deste tópico,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: ce1305f9-30fd-49e3-bf38-d0a78dfb06c3
msc.legacyurl: /signalr/overview/performance/scaleout-with-windows-azure-service-bus
msc.type: authoredcontent
ms.openlocfilehash: 73ed95c5027f57c7e390069dcb36b18a3714973f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579173"
---
# <a name="signalr-scaleout-with-azure-service-bus"></a><span data-ttu-id="b32ca-103">Expansão do SignalR com o Barramento de Serviço do Azure</span><span class="sxs-lookup"><span data-stu-id="b32ca-103">SignalR Scaleout with Azure Service Bus</span></span>

<span data-ttu-id="b32ca-104">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="b32ca-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="b32ca-105">Neste tutorial, você implantará um aplicativo Signalr em uma função Web do Windows Azure, usando o backplane do barramento de serviço para distribuir mensagens para cada instância de função.</span><span class="sxs-lookup"><span data-stu-id="b32ca-105">In this tutorial, you will deploy a SignalR application to a Windows Azure Web Role, using the Service Bus backplane to distribute messages to each role instance.</span></span> <span data-ttu-id="b32ca-106">(Você também pode usar o backplane do barramento de serviço com [aplicativos Web no serviço Azure app](https://docs.microsoft.com/azure/app-service-web/).)</span><span class="sxs-lookup"><span data-stu-id="b32ca-106">(You can also use the Service Bus backplane with [web apps in Azure App Service](https://docs.microsoft.com/azure/app-service-web/).)</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image1.png)

<span data-ttu-id="b32ca-107">Pré-requisitos:</span><span class="sxs-lookup"><span data-stu-id="b32ca-107">Prerequisites:</span></span>

- <span data-ttu-id="b32ca-108">Uma conta do Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b32ca-108">A Windows Azure account.</span></span>
- <span data-ttu-id="b32ca-109">O [SDK do Windows Azure](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="b32ca-109">The [Windows Azure SDK](https://go.microsoft.com/fwlink/?linkid=254364&amp;clcid=0x409).</span></span>
- <span data-ttu-id="b32ca-110">Visual Studio 2012 ou 2013.</span><span class="sxs-lookup"><span data-stu-id="b32ca-110">Visual Studio 2012 or 2013.</span></span>

<span data-ttu-id="b32ca-111">O backplane do barramento de serviço também é compatível com o [barramento de serviço para Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), versão 1,1.</span><span class="sxs-lookup"><span data-stu-id="b32ca-111">The service bus backplane is also compatible with [Service Bus for Windows Server](https://msdn.microsoft.com/library/windowsazure/dn282144.aspx), version 1.1.</span></span> <span data-ttu-id="b32ca-112">No entanto, ele não é compatível com a versão 1,0 do barramento de serviço para Windows Server.</span><span class="sxs-lookup"><span data-stu-id="b32ca-112">However, it is not compatible with version 1.0 of Service Bus for Windows Server.</span></span>

## <a name="pricing"></a><span data-ttu-id="b32ca-113">Preços</span><span class="sxs-lookup"><span data-stu-id="b32ca-113">Pricing</span></span>

<span data-ttu-id="b32ca-114">O backplane do barramento de serviço usa tópicos para enviar mensagens.</span><span class="sxs-lookup"><span data-stu-id="b32ca-114">The Service Bus backplane uses topics to send messages.</span></span> <span data-ttu-id="b32ca-115">Para obter as informações de preços mais recentes, consulte [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="b32ca-115">For the latest pricing information, see [Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).</span></span> <span data-ttu-id="b32ca-116">No momento da redação deste artigo, você pode enviar 1 milhão mensagens por mês para menos de $1.</span><span class="sxs-lookup"><span data-stu-id="b32ca-116">At the time of this writing, you can send 1,000,000 messages per month for less than $1.</span></span> <span data-ttu-id="b32ca-117">O backplane envia uma mensagem do barramento de serviço para cada invocação de um método de Hub do Signalr.</span><span class="sxs-lookup"><span data-stu-id="b32ca-117">The backplane sends a service bus message for each invocation of a SignalR hub method.</span></span> <span data-ttu-id="b32ca-118">Há também algumas mensagens de controle para conexões, desconexões, junção ou saída de grupos e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="b32ca-118">There are also some control messages for connections, disconnections, joining or leaving groups, and so forth.</span></span> <span data-ttu-id="b32ca-119">Na maioria dos aplicativos, a maior parte do tráfego de mensagens será invocações de método de Hub.</span><span class="sxs-lookup"><span data-stu-id="b32ca-119">In most applications, the majority of the message traffic will be hub method invocations.</span></span>

## <a name="overview"></a><span data-ttu-id="b32ca-120">Visão geral</span><span class="sxs-lookup"><span data-stu-id="b32ca-120">Overview</span></span>

<span data-ttu-id="b32ca-121">Antes de chegarmos ao tutorial detalhado, aqui está uma visão geral rápida do que você fará.</span><span class="sxs-lookup"><span data-stu-id="b32ca-121">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="b32ca-122">Use o portal do Azure do Windows para criar um novo namespace do barramento de serviço.</span><span class="sxs-lookup"><span data-stu-id="b32ca-122">Use the Windows Azure portal to create a new Service Bus namespace.</span></span>
2. <span data-ttu-id="b32ca-123">Adicione esses pacotes NuGet ao seu aplicativo:</span><span class="sxs-lookup"><span data-stu-id="b32ca-123">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="b32ca-124">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="b32ca-124">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - <span data-ttu-id="b32ca-125">[Microsoft. AspNet. signalr. ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) ou [Microsoft. AspNet. Signalr. ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span><span class="sxs-lookup"><span data-stu-id="b32ca-125">[Microsoft.AspNet.SignalR.ServiceBus3](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus3) or [Microsoft.AspNet.SignalR.ServiceBus](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.ServiceBus)</span></span>
3. <span data-ttu-id="b32ca-126">Crie um aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="b32ca-126">Create a SignalR application.</span></span>
4. <span data-ttu-id="b32ca-127">Adicione o seguinte código a Startup.cs para configurar o backplane:</span><span class="sxs-lookup"><span data-stu-id="b32ca-127">Add the following code to Startup.cs to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample1.cs)]

<span data-ttu-id="b32ca-128">Esse código configura o backplane com os valores padrão para [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) e [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="b32ca-128">This code configures the backplane with the default values for [TopicCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.servicebusscaleoutconfiguration.topiccount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="b32ca-129">Para obter informações sobre como alterar esses valores, consulte [desempenho do signalr: métricas de scale](signalr-performance.md#scaleout_metrics)out.</span><span class="sxs-lookup"><span data-stu-id="b32ca-129">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

<span data-ttu-id="b32ca-130">Para cada aplicativo, escolha um valor diferente para "Nomedoseuaplicativo".</span><span class="sxs-lookup"><span data-stu-id="b32ca-130">For each application, pick a different value for "YourAppName".</span></span> <span data-ttu-id="b32ca-131">Não use o mesmo valor em vários aplicativos.</span><span class="sxs-lookup"><span data-stu-id="b32ca-131">Do not use the same value across multiple applications.</span></span>

## <a name="create-the-azure-services"></a><span data-ttu-id="b32ca-132">Criar os serviços do Azure</span><span class="sxs-lookup"><span data-stu-id="b32ca-132">Create the Azure Services</span></span>

<span data-ttu-id="b32ca-133">Crie um serviço de nuvem, conforme descrito em [como criar e implantar um serviço de nuvem](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span><span class="sxs-lookup"><span data-stu-id="b32ca-133">Create a Cloud Service, as described in [How to Create and Deploy a Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy).</span></span> <span data-ttu-id="b32ca-134">Siga as etapas na seção "como criar um serviço de nuvem usando a criação rápida".</span><span class="sxs-lookup"><span data-stu-id="b32ca-134">Follow the steps in the section "How to: Create a cloud service using Quick Create".</span></span> <span data-ttu-id="b32ca-135">Para este tutorial, você não precisa carregar um certificado.</span><span class="sxs-lookup"><span data-stu-id="b32ca-135">For this tutorial, you do not need to upload a certificate.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image2.png)

<span data-ttu-id="b32ca-136">Crie um novo namespace do barramento de serviço, conforme descrito em [como usar os tópicos/assinaturas do barramento de serviço](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span><span class="sxs-lookup"><span data-stu-id="b32ca-136">Create a new Service Bus namespace, as described in [How to Use Service Bus Topics/Subscriptions](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="b32ca-137">Siga as etapas na seção "criar um namespace de serviço".</span><span class="sxs-lookup"><span data-stu-id="b32ca-137">Follow the steps in the section "Create a Service Namespace".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="b32ca-138">Certifique-se de selecionar a mesma região para o serviço de nuvem e o namespace do barramento de serviço.</span><span class="sxs-lookup"><span data-stu-id="b32ca-138">Make sure to select the same region for the cloud service and the Service Bus namespace.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="b32ca-139">Criar o projeto do Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b32ca-139">Create the Visual Studio Project</span></span>

<span data-ttu-id="b32ca-140">Inicie o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b32ca-140">Start Visual Studio.</span></span> <span data-ttu-id="b32ca-141">No menu **Arquivo**, clique em **Novo Projeto**.</span><span class="sxs-lookup"><span data-stu-id="b32ca-141">From the **File** menu, click **New Project**.</span></span>

<span data-ttu-id="b32ca-142">Na caixa de diálogo **novo projeto** , expanda **Visual C#** .</span><span class="sxs-lookup"><span data-stu-id="b32ca-142">In the **New Project** dialog box, expand **Visual C#**.</span></span> <span data-ttu-id="b32ca-143">Em **modelos instalados**, selecione **nuvem** e, em seguida, selecione **serviço de nuvem do Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="b32ca-143">Under **Installed Templates**, select **Cloud** and then select **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="b32ca-144">Mantenha o .NET Framework padrão 4,5.</span><span class="sxs-lookup"><span data-stu-id="b32ca-144">Keep the default .NET Framework 4.5.</span></span> <span data-ttu-id="b32ca-145">Nomeie o aplicativo ChatService e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="b32ca-145">Name the application ChatService and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image4.png)

<span data-ttu-id="b32ca-146">Na caixa de diálogo **novo serviço de nuvem do Windows Azure** , selecione função Web ASP.net.</span><span class="sxs-lookup"><span data-stu-id="b32ca-146">In the **New Windows Azure Cloud Service** dialog, select ASP.NET Web Role.</span></span> <span data-ttu-id="b32ca-147">Clique no botão de seta para a direita ( **&gt;** ) para adicionar a função à sua solução.</span><span class="sxs-lookup"><span data-stu-id="b32ca-147">Click the right-arrow button (**&gt;**) to add the role to your solution.</span></span>

<span data-ttu-id="b32ca-148">Passe o mouse sobre a nova função, portanto, o ícone de lápis está visível.</span><span class="sxs-lookup"><span data-stu-id="b32ca-148">Hover the mouse over the new role, so the pencil icon visible.</span></span> <span data-ttu-id="b32ca-149">Clique nesse ícone para renomear a função.</span><span class="sxs-lookup"><span data-stu-id="b32ca-149">Click this icon to rename the role.</span></span> <span data-ttu-id="b32ca-150">Nomeie a função "SignalRChat" e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="b32ca-150">Name the role "SignalRChat" and click **OK**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image5.png)

<span data-ttu-id="b32ca-151">Na caixa de diálogo **novo projeto ASP.net** , selecione **MVC**e clique em OK.</span><span class="sxs-lookup"><span data-stu-id="b32ca-151">In the **New ASP.NET Project** dialog, select **MVC**, and click OK.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image6.png)

<span data-ttu-id="b32ca-152">O assistente de projeto cria dois projetos:</span><span class="sxs-lookup"><span data-stu-id="b32ca-152">The project wizard creates two projects:</span></span>

- <span data-ttu-id="b32ca-153">ChatService: este projeto é o aplicativo do Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b32ca-153">ChatService: This project is the Windows Azure application.</span></span> <span data-ttu-id="b32ca-154">Ele define as funções do Azure e outras opções de configuração.</span><span class="sxs-lookup"><span data-stu-id="b32ca-154">It defines the Azure roles and other configuration options.</span></span>
- <span data-ttu-id="b32ca-155">SignalRChat: este projeto é seu projeto do ASP.NET MVC 5.</span><span class="sxs-lookup"><span data-stu-id="b32ca-155">SignalRChat: This project is your ASP.NET MVC 5 project.</span></span>

## <a name="create-the-signalr-chat-application"></a><span data-ttu-id="b32ca-156">Criar o aplicativo de chat do Signalr</span><span class="sxs-lookup"><span data-stu-id="b32ca-156">Create the SignalR Chat Application</span></span>

<span data-ttu-id="b32ca-157">Para criar o aplicativo de chat, siga as etapas no tutorial [introdução com signalr e MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span><span class="sxs-lookup"><span data-stu-id="b32ca-157">To create the chat application, follow the steps in the tutorial [Getting Started with SignalR and MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md).</span></span>

<span data-ttu-id="b32ca-158">Use o NuGet para instalar as bibliotecas necessárias.</span><span class="sxs-lookup"><span data-stu-id="b32ca-158">Use NuGet to install the required libraries.</span></span> <span data-ttu-id="b32ca-159">No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="b32ca-159">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="b32ca-160">Na janela do **console do Gerenciador de pacotes** , digite os seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="b32ca-160">In the **Package Manager Console** window, enter the following commands:</span></span>

[!code-powershell[Main](scaleout-with-windows-azure-service-bus/samples/sample2.ps1)]

<span data-ttu-id="b32ca-161">Use a opção `-ProjectName` para instalar os pacotes no projeto MVC do ASP.NET, em vez de no projeto do Windows Azure.</span><span class="sxs-lookup"><span data-stu-id="b32ca-161">Use the `-ProjectName` option to install the packages to the ASP.NET MVC project, rather than the Windows Azure project.</span></span>

## <a name="configure-the-backplane"></a><span data-ttu-id="b32ca-162">Configurar o backplane</span><span class="sxs-lookup"><span data-stu-id="b32ca-162">Configure the Backplane</span></span>

<span data-ttu-id="b32ca-163">No arquivo Startup.cs do seu aplicativo, adicione o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="b32ca-163">In your application's Startup.cs file, add the following code:</span></span>

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample3.cs)]

<span data-ttu-id="b32ca-164">Agora você precisa obter a cadeia de conexão do barramento de serviço.</span><span class="sxs-lookup"><span data-stu-id="b32ca-164">Now you need to get your service bus connection string.</span></span> <span data-ttu-id="b32ca-165">No portal do Azure, selecione o namespace do barramento de serviço que você criou e clique no ícone de chave de acesso.</span><span class="sxs-lookup"><span data-stu-id="b32ca-165">In the Azure portal, select the service bus namespace that you created and click the Access Key icon.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image7.png)

<span data-ttu-id="b32ca-166">Copie a cadeia de conexão para a área de transferência e cole-a na variável *ConnectionString* .</span><span class="sxs-lookup"><span data-stu-id="b32ca-166">Copy the connection string to the clipboard, then paste it into the *connectionString* variable.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image8.png)

[!code-csharp[Main](scaleout-with-windows-azure-service-bus/samples/sample4.cs)]

## <a name="deploy-to-azure"></a><span data-ttu-id="b32ca-167">Implantar no Azure</span><span class="sxs-lookup"><span data-stu-id="b32ca-167">Deploy to Azure</span></span>

<span data-ttu-id="b32ca-168">Em Gerenciador de Soluções, expanda a pasta **funções** dentro do projeto ChatService.</span><span class="sxs-lookup"><span data-stu-id="b32ca-168">In Solution Explorer, expand the **Roles** folder inside the ChatService project.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image9.png)

<span data-ttu-id="b32ca-169">Clique com o botão direito do mouse na função SignalRChat e selecione **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="b32ca-169">Right-click the SignalRChat role and select **Properties**.</span></span> <span data-ttu-id="b32ca-170">Selecione a guia **configuração** . Em **instâncias** , selecione 2.</span><span class="sxs-lookup"><span data-stu-id="b32ca-170">Select the **Configuration** tab. Under **Instances** select 2.</span></span> <span data-ttu-id="b32ca-171">Você também pode definir o tamanho da VM para **extra pequena**.</span><span class="sxs-lookup"><span data-stu-id="b32ca-171">You can also set the VM size to **Extra Small**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image10.png)

<span data-ttu-id="b32ca-172">Salve as alterações.</span><span class="sxs-lookup"><span data-stu-id="b32ca-172">Save the changes.</span></span>

<span data-ttu-id="b32ca-173">Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto ChatService.</span><span class="sxs-lookup"><span data-stu-id="b32ca-173">In Solution Explorer, right-click the ChatService project.</span></span> <span data-ttu-id="b32ca-174">Selecione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="b32ca-174">Select **Publish**.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image11.png)

<span data-ttu-id="b32ca-175">Se esta for a primeira vez que você publica no Windows Azure, você deve baixar suas credenciais.</span><span class="sxs-lookup"><span data-stu-id="b32ca-175">If this is your first time publishing to Windows Azure, you must download your credentials.</span></span> <span data-ttu-id="b32ca-176">No assistente de **publicação** , clique em "entrar para baixar credenciais".</span><span class="sxs-lookup"><span data-stu-id="b32ca-176">In the **Publish** wizard, click "Sign in to download credentials".</span></span> <span data-ttu-id="b32ca-177">Isso solicitará que você entre no portal do Azure do Windows e baixe um arquivo de configurações de publicação.</span><span class="sxs-lookup"><span data-stu-id="b32ca-177">This will prompt you to sign into the Windows Azure portal and download a publish settings file.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image12.png)

<span data-ttu-id="b32ca-178">Clique em **importar** e selecione o arquivo de configurações de publicação que você baixou.</span><span class="sxs-lookup"><span data-stu-id="b32ca-178">Click **Import** and select the publish settings file that you downloaded.</span></span>

<span data-ttu-id="b32ca-179">Clique em **Próximo**.</span><span class="sxs-lookup"><span data-stu-id="b32ca-179">Click **Next**.</span></span> <span data-ttu-id="b32ca-180">Na caixa de diálogo **configurações de publicação** , em **serviço de nuvem**, selecione o serviço de nuvem que você criou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b32ca-180">In the **Publish Settings** dialog, under **Cloud Service**, select the cloud service that you created earlier.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image13.png)

<span data-ttu-id="b32ca-181">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="b32ca-181">Click **Publish**.</span></span> <span data-ttu-id="b32ca-182">Pode levar alguns minutos para implantar o aplicativo e iniciar as VMs.</span><span class="sxs-lookup"><span data-stu-id="b32ca-182">It can take a few minutes to deploy the application and start the VMs.</span></span>

<span data-ttu-id="b32ca-183">Agora, quando você executa o aplicativo de chat, as instâncias de função se comunicam por meio do barramento de serviço do Azure, usando um tópico do barramento de serviço.</span><span class="sxs-lookup"><span data-stu-id="b32ca-183">Now when you run the chat application, the role instances communicate through Azure Service Bus, using a Service Bus topic.</span></span> <span data-ttu-id="b32ca-184">Um tópico é uma fila de mensagens que permite vários assinantes.</span><span class="sxs-lookup"><span data-stu-id="b32ca-184">A topic is a message queue that allows multiple subscribers.</span></span>

<span data-ttu-id="b32ca-185">O backplane cria automaticamente o tópico e as assinaturas.</span><span class="sxs-lookup"><span data-stu-id="b32ca-185">The backplane automatically creates the topic and the subscriptions.</span></span> <span data-ttu-id="b32ca-186">Para ver a atividade de assinaturas e mensagens, abra o portal do Azure, selecione o namespace do barramento de serviço e clique em "tópicos".</span><span class="sxs-lookup"><span data-stu-id="b32ca-186">To see the subscriptions and message activity, open the Azure portal, select the Service Bus namespace, and click on "Topics".</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image14.png)

<span data-ttu-id="b32ca-187">Reserve alguns minutos para que a atividade de mensagem seja exibida no painel.</span><span class="sxs-lookup"><span data-stu-id="b32ca-187">It make take a few minutes for the message activity to show up in the dashboard.</span></span>

![](scaleout-with-windows-azure-service-bus/_static/image15.png)

<span data-ttu-id="b32ca-188">O signalr gerencia o tempo de vida do tópico.</span><span class="sxs-lookup"><span data-stu-id="b32ca-188">SignalR manages the topic lifetime.</span></span> <span data-ttu-id="b32ca-189">Desde que seu aplicativo seja implantado, não tente excluir manualmente os tópicos ou alterar as configurações no tópico.</span><span class="sxs-lookup"><span data-stu-id="b32ca-189">As long as your application is deployed, don't try to manually delete topics or change settings on the topic.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="b32ca-190">solução de problemas</span><span class="sxs-lookup"><span data-stu-id="b32ca-190">Troubleshooting</span></span>

<span data-ttu-id="b32ca-191">**System. InvalidOperationException "o único IsolationLevel com suporte é ' IsolationLevel. Serializable '."**</span><span class="sxs-lookup"><span data-stu-id="b32ca-191">**System.InvalidOperationException "The only supported IsolationLevel is 'IsolationLevel.Serializable'."**</span></span>

<span data-ttu-id="b32ca-192">Esse erro poderá ocorrer se o nível de transação de uma operação for definido como algo diferente de `Serializable`.</span><span class="sxs-lookup"><span data-stu-id="b32ca-192">This error can occur if the transaction level for an operation is set to something other than `Serializable`.</span></span> <span data-ttu-id="b32ca-193">Verifique se nenhuma operação está sendo executada com outros níveis de transação.</span><span class="sxs-lookup"><span data-stu-id="b32ca-193">Verify that no operations are being performed with other transaction levels.</span></span>
