---
uid: signalr/overview/performance/scaleout-with-redis
title: Expansão do signalr com Redis | Microsoft Docs
author: bradygaster
description: Versões de software usadas neste tópico Visual Studio 2013 o .NET 4,5 Signalr versão 2 versões anteriores deste tópico para obter informações sobre versões anteriores do...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 6ecd08c1-e364-4cd7-ad4c-806521911585
msc.legacyurl: /signalr/overview/performance/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 58a7affa1769523955adc76455a1c33be6f49751
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579222"
---
# <a name="signalr-scaleout-with-redis"></a><span data-ttu-id="3413a-103">Expansão do SignalR com Redis</span><span class="sxs-lookup"><span data-stu-id="3413a-103">SignalR Scaleout with Redis</span></span>

<span data-ttu-id="3413a-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3413a-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="3413a-105">Versões de software usadas neste tópico</span><span class="sxs-lookup"><span data-stu-id="3413a-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="3413a-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3413a-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="3413a-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="3413a-107">.NET 4.5</span></span>
> - <span data-ttu-id="3413a-108">Sinalização versão 2,4</span><span class="sxs-lookup"><span data-stu-id="3413a-108">SignalR version 2.4</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="3413a-109">Versões anteriores deste tópico</span><span class="sxs-lookup"><span data-stu-id="3413a-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="3413a-110">Para obter informações sobre versões anteriores do Signalr, confira [versões mais antigas do signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="3413a-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="3413a-111">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="3413a-111">Questions and comments</span></span>
>
> <span data-ttu-id="3413a-112">Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="3413a-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="3413a-113">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="3413a-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="3413a-114">Neste tutorial, você usará o [Redis](http://redis.io/) para distribuir mensagens em um aplicativo signalr que é implantado em duas instâncias do IIS separadas.</span><span class="sxs-lookup"><span data-stu-id="3413a-114">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="3413a-115">Redis é um repositório de chave-valor na memória.</span><span class="sxs-lookup"><span data-stu-id="3413a-115">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="3413a-116">Ele também dá suporte a um sistema de mensagens com um modelo de publicação/assinatura.</span><span class="sxs-lookup"><span data-stu-id="3413a-116">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="3413a-117">O backplane Redis do Signalr usa o recurso pub/sub para encaminhar mensagens para outros servidores.</span><span class="sxs-lookup"><span data-stu-id="3413a-117">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="3413a-118">Para este tutorial, você usará três servidores:</span><span class="sxs-lookup"><span data-stu-id="3413a-118">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="3413a-119">Dois servidores executando o Windows, que serão usados para implantar um aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="3413a-119">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="3413a-120">Um servidor que executa o Linux, que será usado para executar o Redis.</span><span class="sxs-lookup"><span data-stu-id="3413a-120">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="3413a-121">Para as capturas de tela deste tutorial, usei o Ubuntu 12, 4 TLS.</span><span class="sxs-lookup"><span data-stu-id="3413a-121">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="3413a-122">Se você não tiver três servidores físicos para usar, poderá criar VMs no Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="3413a-122">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="3413a-123">Outra opção é criar VMs no Azure.</span><span class="sxs-lookup"><span data-stu-id="3413a-123">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="3413a-124">Embora este tutorial use a implementação oficial do Redis, também há uma [porta do Windows de Redis](https://github.com/MSOpenTech/redis) do MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="3413a-124">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="3413a-125">A instalação e a configuração são diferentes, mas, caso contrário, as etapas são as mesmas.</span><span class="sxs-lookup"><span data-stu-id="3413a-125">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE]
>
> <span data-ttu-id="3413a-126">O dimensionamento do signalr com Redis não dá suporte a clusters Redis.</span><span class="sxs-lookup"><span data-stu-id="3413a-126">SignalR scaleout with Redis does not support Redis clusters.</span></span>

## <a name="overview"></a><span data-ttu-id="3413a-127">Visão geral</span><span class="sxs-lookup"><span data-stu-id="3413a-127">Overview</span></span>

<span data-ttu-id="3413a-128">Antes de chegarmos ao tutorial detalhado, aqui está uma visão geral rápida do que você fará.</span><span class="sxs-lookup"><span data-stu-id="3413a-128">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="3413a-129">Instale o Redis e inicie o servidor Redis.</span><span class="sxs-lookup"><span data-stu-id="3413a-129">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="3413a-130">Adicione esses pacotes NuGet ao seu aplicativo:</span><span class="sxs-lookup"><span data-stu-id="3413a-130">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="3413a-131">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="3413a-131">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="3413a-132">Microsoft. AspNet. Signalr. StackExchangeRedis</span><span class="sxs-lookup"><span data-stu-id="3413a-132">Microsoft.AspNet.SignalR.StackExchangeRedis</span></span>](https://www.nuget.org/packages/Microsoft.AspNet.SignalR.StackExchangeRedis)
    
3. <span data-ttu-id="3413a-133">Crie um aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="3413a-133">Create a SignalR application.</span></span>
4. <span data-ttu-id="3413a-134">Adicione o seguinte código a Startup.cs para configurar o backplane:</span><span class="sxs-lookup"><span data-stu-id="3413a-134">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="3413a-135">Ubuntu no Hyper-V</span><span class="sxs-lookup"><span data-stu-id="3413a-135">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="3413a-136">Usando o Windows Hyper-V, você pode criar facilmente uma VM do Ubuntu no Windows Server.</span><span class="sxs-lookup"><span data-stu-id="3413a-136">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="3413a-137">Baixe o ISO do Ubuntu de [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="3413a-137">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="3413a-138">No Hyper-V, adicione uma nova VM.</span><span class="sxs-lookup"><span data-stu-id="3413a-138">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="3413a-139">Na etapa **conectar disco rígido virtual** , selecione **criar um disco rígido virtual**.</span><span class="sxs-lookup"><span data-stu-id="3413a-139">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="3413a-140">Na etapa **Opções de instalação** , selecione **arquivo de imagem (. ISO)** , clique em **procurar**e navegue até o ISO de instalação do Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="3413a-140">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="3413a-141">Instalar o Redis</span><span class="sxs-lookup"><span data-stu-id="3413a-141">Install Redis</span></span>

<span data-ttu-id="3413a-142">Siga as etapas em [http://redis.io/download](http://redis.io/download) para baixar e compilar o Redis.</span><span class="sxs-lookup"><span data-stu-id="3413a-142">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="3413a-143">Isso cria os binários Redis no diretório `src`.</span><span class="sxs-lookup"><span data-stu-id="3413a-143">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="3413a-144">Por padrão, o Redis não requer uma senha.</span><span class="sxs-lookup"><span data-stu-id="3413a-144">By default, Redis does not require a password.</span></span> <span data-ttu-id="3413a-145">Para definir uma senha, edite o arquivo `redis.conf`, localizado no diretório raiz do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="3413a-145">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="3413a-146">(Faça uma cópia de backup do arquivo antes de editá-lo!) Adicione a seguinte diretiva a `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="3413a-146">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="3413a-147">Agora, inicie o servidor Redis:</span><span class="sxs-lookup"><span data-stu-id="3413a-147">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="3413a-148">Abra a porta 6379, que é a porta padrão que o Redis escuta.</span><span class="sxs-lookup"><span data-stu-id="3413a-148">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="3413a-149">(Você pode alterar o número da porta no arquivo de configuração.)</span><span class="sxs-lookup"><span data-stu-id="3413a-149">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="3413a-150">Criar o aplicativo Signalr</span><span class="sxs-lookup"><span data-stu-id="3413a-150">Create the SignalR Application</span></span>

<span data-ttu-id="3413a-151">Crie um aplicativo Signalr seguindo um destes tutoriais:</span><span class="sxs-lookup"><span data-stu-id="3413a-151">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="3413a-152">Introdução com Signalr 2,0</span><span class="sxs-lookup"><span data-stu-id="3413a-152">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="3413a-153">Introdução com Signalr 2,0 e MVC 5</span><span class="sxs-lookup"><span data-stu-id="3413a-153">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="3413a-154">Em seguida, modificaremos o aplicativo de chat para dar suporte ao ScaleOut com Redis.</span><span class="sxs-lookup"><span data-stu-id="3413a-154">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="3413a-155">Primeiro, adicione o pacote NuGet `Microsoft.AspNet.SignalR.StackExchangeRedis` ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="3413a-155">First, add the `Microsoft.AspNet.SignalR.StackExchangeRedis` NuGet package to your project.</span></span> <span data-ttu-id="3413a-156">No Visual Studio, no menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="3413a-156">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="3413a-157">Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="3413a-157">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="3413a-158">Em seguida, abra o arquivo Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="3413a-158">Next, open the Startup.cs file.</span></span> <span data-ttu-id="3413a-159">Adicione o seguinte código ao método de **configuração** :</span><span class="sxs-lookup"><span data-stu-id="3413a-159">Add the following code to the **Configuration** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="3413a-160">"servidor" é o nome do servidor que está executando o Redis.</span><span class="sxs-lookup"><span data-stu-id="3413a-160">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="3413a-161">*Port* é o número da porta</span><span class="sxs-lookup"><span data-stu-id="3413a-161">*port* is the port number</span></span>
- <span data-ttu-id="3413a-162">"password" é a senha que você definiu no arquivo Redis. conf.</span><span class="sxs-lookup"><span data-stu-id="3413a-162">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="3413a-163">"AppName" é qualquer cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="3413a-163">"AppName" is any string.</span></span> <span data-ttu-id="3413a-164">O signalr cria um Redis pub/sub Channel com esse nome.</span><span class="sxs-lookup"><span data-stu-id="3413a-164">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="3413a-165">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3413a-165">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="3413a-166">Implantar e executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="3413a-166">Deploy and Run the Application</span></span>

<span data-ttu-id="3413a-167">Prepare suas instâncias do Windows Server para implantar o aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="3413a-167">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="3413a-168">Adicione a função IIS.</span><span class="sxs-lookup"><span data-stu-id="3413a-168">Add the IIS role.</span></span> <span data-ttu-id="3413a-169">Inclua recursos de "desenvolvimento de aplicativos", incluindo o protocolo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3413a-169">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="3413a-170">Inclua também o serviço de gerenciamento (listado em "ferramentas de gerenciamento").</span><span class="sxs-lookup"><span data-stu-id="3413a-170">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="3413a-171">**Instale o Implantação da Web 3,0.**</span><span class="sxs-lookup"><span data-stu-id="3413a-171">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="3413a-172">Quando você executar o Gerenciador do IIS, ele solicitará que você instale a plataforma Web da Microsoft ou você poderá [baixar o instalador](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="3413a-172">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="3413a-173">No instalador da plataforma, procure Implantação da Web e instale o Implantação da Web 3,0</span><span class="sxs-lookup"><span data-stu-id="3413a-173">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="3413a-174">Verifique se o serviço de gerenciamento da Web está em execução.</span><span class="sxs-lookup"><span data-stu-id="3413a-174">Check that the Web Management Service is running.</span></span> <span data-ttu-id="3413a-175">Caso contrário, inicie o serviço.</span><span class="sxs-lookup"><span data-stu-id="3413a-175">If not, start the service.</span></span> <span data-ttu-id="3413a-176">(Se você não vir o serviço de gerenciamento da Web na lista de serviços do Windows, certifique-se de que você instalou o serviço de gerenciamento quando adicionou a função do IIS.)</span><span class="sxs-lookup"><span data-stu-id="3413a-176">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="3413a-177">Por padrão, o serviço de gerenciamento da Web escuta na porta TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="3413a-177">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="3413a-178">No firewall do Windows, crie uma nova regra de entrada para permitir o tráfego TCP na porta 8172.</span><span class="sxs-lookup"><span data-stu-id="3413a-178">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="3413a-179">Para obter mais informações, consulte [Configurando regras de firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="3413a-179">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="3413a-180">(Se você estiver hospedando as VMs no Azure, poderá fazer isso diretamente na portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="3413a-180">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="3413a-181">Consulte [como configurar pontos de extremidade para uma máquina virtual](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="3413a-181">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="3413a-182">Agora você está pronto para implantar o projeto do Visual Studio do computador de desenvolvimento no servidor.</span><span class="sxs-lookup"><span data-stu-id="3413a-182">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="3413a-183">Em Gerenciador de Soluções, clique com o botão direito do mouse na solução e clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="3413a-183">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="3413a-184">Para obter uma documentação mais detalhada sobre a implantação da Web, consulte [mapa de conteúdo de implantação da Web para Visual Studio e ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="3413a-184">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="3413a-185">Se você implantar o aplicativo em dois servidores, poderá abrir cada instância em uma janela separada do navegador e ver que cada uma recebe mensagens do Signalr da outra.</span><span class="sxs-lookup"><span data-stu-id="3413a-185">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="3413a-186">(Naturalmente, em um ambiente de produção, os dois servidores ficam atrás de um balanceador de carga.)</span><span class="sxs-lookup"><span data-stu-id="3413a-186">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="3413a-187">Se você estiver curioso para ver as mensagens que são enviadas para o Redis, você pode usar o cliente **Redis-CLI** , que é instalado com o Redis.</span><span class="sxs-lookup"><span data-stu-id="3413a-187">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
