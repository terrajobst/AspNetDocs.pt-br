---
uid: signalr/overview/older-versions/scaleout-with-redis
title: Escalador de enparador de sinalização com Redis (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 6abecf80-8ffa-41ba-b0d9-1d9edbe7687b
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-redis
msc.type: authoredcontent
ms.openlocfilehash: 5079cf3fa773faeac911eddc75dc66e94ad98bb0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536557"
---
# <a name="signalr-scaleout-with-redis-signalr-1x"></a><span data-ttu-id="d1802-102">Expansão do SignalR com Redis (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="d1802-102">SignalR Scaleout with Redis (SignalR 1.x)</span></span>

<span data-ttu-id="d1802-103">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="d1802-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="d1802-104">Neste tutorial, você usará o [Redis](http://redis.io/) para distribuir mensagens em um aplicativo signalr que é implantado em duas instâncias do IIS separadas.</span><span class="sxs-lookup"><span data-stu-id="d1802-104">In this tutorial, you will use [Redis](http://redis.io/) to distribute messages across a SignalR application that is deployed on two separate IIS instances.</span></span>

<span data-ttu-id="d1802-105">Redis é um repositório de chave-valor na memória.</span><span class="sxs-lookup"><span data-stu-id="d1802-105">Redis is an in-memory key-value store.</span></span> <span data-ttu-id="d1802-106">Ele também dá suporte a um sistema de mensagens com um modelo de publicação/assinatura.</span><span class="sxs-lookup"><span data-stu-id="d1802-106">It also supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="d1802-107">O backplane Redis do Signalr usa o recurso pub/sub para encaminhar mensagens para outros servidores.</span><span class="sxs-lookup"><span data-stu-id="d1802-107">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span>

![](scaleout-with-redis/_static/image1.png)

<span data-ttu-id="d1802-108">Para este tutorial, você usará três servidores:</span><span class="sxs-lookup"><span data-stu-id="d1802-108">For this tutorial, you will use three servers:</span></span>

- <span data-ttu-id="d1802-109">Dois servidores executando o Windows, que serão usados para implantar um aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="d1802-109">Two servers running Windows, which you will use to deploy a SignalR application.</span></span>
- <span data-ttu-id="d1802-110">Um servidor que executa o Linux, que será usado para executar o Redis.</span><span class="sxs-lookup"><span data-stu-id="d1802-110">One server running Linux, which you will use to run Redis.</span></span> <span data-ttu-id="d1802-111">Para as capturas de tela deste tutorial, usei o Ubuntu 12, 4 TLS.</span><span class="sxs-lookup"><span data-stu-id="d1802-111">For the screenshots in this tutorial, I used Ubuntu 12.04 TLS.</span></span>

<span data-ttu-id="d1802-112">Se você não tiver três servidores físicos para usar, poderá criar VMs no Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="d1802-112">If you don't have three physical servers to use, you can create VMs on Hyper-V.</span></span> <span data-ttu-id="d1802-113">Outra opção é criar VMs no Azure.</span><span class="sxs-lookup"><span data-stu-id="d1802-113">Another option is to create VMs on Azure.</span></span>

<span data-ttu-id="d1802-114">Embora este tutorial use a implementação oficial do Redis, também há uma [porta do Windows de Redis](https://github.com/MSOpenTech/redis) do MSOpenTech.</span><span class="sxs-lookup"><span data-stu-id="d1802-114">Although this tutorial uses the official Redis implementation, there is also a [Windows port of Redis](https://github.com/MSOpenTech/redis) from MSOpenTech.</span></span> <span data-ttu-id="d1802-115">A instalação e a configuração são diferentes, mas, caso contrário, as etapas são as mesmas.</span><span class="sxs-lookup"><span data-stu-id="d1802-115">Setup and configuration are different, but otherwise the steps are the same.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="d1802-116">O dimensionamento do signalr com Redis não dá suporte a clusters Redis.</span><span class="sxs-lookup"><span data-stu-id="d1802-116">SignalR scaleout with Redis does not support Redis clusters.</span></span>

## <a name="overview"></a><span data-ttu-id="d1802-117">Visão geral</span><span class="sxs-lookup"><span data-stu-id="d1802-117">Overview</span></span>

<span data-ttu-id="d1802-118">Antes de chegarmos ao tutorial detalhado, aqui está uma visão geral rápida do que você fará.</span><span class="sxs-lookup"><span data-stu-id="d1802-118">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="d1802-119">Instale o Redis e inicie o servidor Redis.</span><span class="sxs-lookup"><span data-stu-id="d1802-119">Install Redis and start the Redis server.</span></span>
2. <span data-ttu-id="d1802-120">Adicione esses pacotes NuGet ao seu aplicativo:</span><span class="sxs-lookup"><span data-stu-id="d1802-120">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="d1802-121">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="d1802-121">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="d1802-122">Microsoft. AspNet. Signalr. Redis</span><span class="sxs-lookup"><span data-stu-id="d1802-122">Microsoft.AspNet.SignalR.Redis</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.Redis)
3. <span data-ttu-id="d1802-123">Crie um aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="d1802-123">Create a SignalR application.</span></span>
4. <span data-ttu-id="d1802-124">Adicione o seguinte código ao global. asax para configurar o backplane:</span><span class="sxs-lookup"><span data-stu-id="d1802-124">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-redis/samples/sample1.cs)]

## <a name="ubuntu-on-hyper-v"></a><span data-ttu-id="d1802-125">Ubuntu no Hyper-V</span><span class="sxs-lookup"><span data-stu-id="d1802-125">Ubuntu on Hyper-V</span></span>

<span data-ttu-id="d1802-126">Usando o Windows Hyper-V, você pode criar facilmente uma VM do Ubuntu no Windows Server.</span><span class="sxs-lookup"><span data-stu-id="d1802-126">Using Windows Hyper-V, you can easily create an Ubuntu VM on Windows Server.</span></span>

<span data-ttu-id="d1802-127">Baixe o ISO do Ubuntu de [http://www.ubuntu.com](http://www.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="d1802-127">Download the Ubuntu ISO from [http://www.ubuntu.com](http://www.ubuntu.com/).</span></span>

<span data-ttu-id="d1802-128">No Hyper-V, adicione uma nova VM.</span><span class="sxs-lookup"><span data-stu-id="d1802-128">In Hyper-V, add a new VM.</span></span> <span data-ttu-id="d1802-129">Na etapa **conectar disco rígido virtual** , selecione **criar um disco rígido virtual**.</span><span class="sxs-lookup"><span data-stu-id="d1802-129">In the **Connect Virtual Hard Disk** step, select **Create a virtual hard disk**.</span></span>

![](scaleout-with-redis/_static/image2.png)

<span data-ttu-id="d1802-130">Na etapa **Opções de instalação** , selecione **arquivo de imagem (. ISO)** , clique em **procurar**e navegue até o ISO de instalação do Ubuntu.</span><span class="sxs-lookup"><span data-stu-id="d1802-130">In the **Installation Options** step, select **Image file (.iso)**, click **Browse**, and browse to the Ubuntu installation ISO.</span></span>

![](scaleout-with-redis/_static/image3.png)

## <a name="install-redis"></a><span data-ttu-id="d1802-131">Instalar o Redis</span><span class="sxs-lookup"><span data-stu-id="d1802-131">Install Redis</span></span>

<span data-ttu-id="d1802-132">Siga as etapas em [http://redis.io/download](http://redis.io/download) para baixar e compilar o Redis.</span><span class="sxs-lookup"><span data-stu-id="d1802-132">Follow the steps at [http://redis.io/download](http://redis.io/download) to download and build Redis.</span></span>

[!code-console[Main](scaleout-with-redis/samples/sample2.cmd)]

<span data-ttu-id="d1802-133">Isso cria os binários Redis no diretório `src`.</span><span class="sxs-lookup"><span data-stu-id="d1802-133">This builds the Redis binaries in the `src` directory.</span></span>

<span data-ttu-id="d1802-134">Por padrão, o Redis não requer uma senha.</span><span class="sxs-lookup"><span data-stu-id="d1802-134">By default, Redis does not require a password.</span></span> <span data-ttu-id="d1802-135">Para definir uma senha, edite o arquivo `redis.conf`, localizado no diretório raiz do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="d1802-135">To set a password, edit the `redis.conf` file, which is located in the root directory of the source code.</span></span> <span data-ttu-id="d1802-136">(Faça uma cópia de backup do arquivo antes de editá-lo!) Adicione a seguinte diretiva a `redis.conf`:</span><span class="sxs-lookup"><span data-stu-id="d1802-136">(Make a backup copy of the file before you edit it!) Add the following directive to `redis.conf`:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample3.ps1)]

<span data-ttu-id="d1802-137">Agora, inicie o servidor Redis:</span><span class="sxs-lookup"><span data-stu-id="d1802-137">Now start the Redis server:</span></span>

[!code-css[Main](scaleout-with-redis/samples/sample4.css)]

![](scaleout-with-redis/_static/image4.png)

<span data-ttu-id="d1802-138">Abra a porta 6379, que é a porta padrão que o Redis escuta.</span><span class="sxs-lookup"><span data-stu-id="d1802-138">Open port 6379, which is the default port that Redis listens on.</span></span> <span data-ttu-id="d1802-139">(Você pode alterar o número da porta no arquivo de configuração.)</span><span class="sxs-lookup"><span data-stu-id="d1802-139">(You can change the port number in the configuration file.)</span></span>

## <a name="create-the-signalr-application"></a><span data-ttu-id="d1802-140">Criar o aplicativo Signalr</span><span class="sxs-lookup"><span data-stu-id="d1802-140">Create the SignalR Application</span></span>

<span data-ttu-id="d1802-141">Crie um aplicativo Signalr seguindo um destes tutoriais:</span><span class="sxs-lookup"><span data-stu-id="d1802-141">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="d1802-142">Introdução com Signalr</span><span class="sxs-lookup"><span data-stu-id="d1802-142">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="d1802-143">Introdução com Signalr e MVC 4</span><span class="sxs-lookup"><span data-stu-id="d1802-143">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="d1802-144">Em seguida, modificaremos o aplicativo de chat para dar suporte ao ScaleOut com Redis.</span><span class="sxs-lookup"><span data-stu-id="d1802-144">Next, we'll modify the chat application to support scaleout with Redis.</span></span> <span data-ttu-id="d1802-145">Primeiro, adicione o pacote NuGet Signalr. Redis ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="d1802-145">First, add the SignalR.Redis NuGet package to your project.</span></span> <span data-ttu-id="d1802-146">No Visual Studio, no menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="d1802-146">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="d1802-147">Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="d1802-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-redis/samples/sample5.ps1)]

<span data-ttu-id="d1802-148">Em seguida, abra o arquivo global. asax.</span><span class="sxs-lookup"><span data-stu-id="d1802-148">Next, open the Global.asax file.</span></span> <span data-ttu-id="d1802-149">Adicione o seguinte código ao **aplicativo\_** método de início:</span><span class="sxs-lookup"><span data-stu-id="d1802-149">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample6.cs)]

- <span data-ttu-id="d1802-150">"servidor" é o nome do servidor que está executando o Redis.</span><span class="sxs-lookup"><span data-stu-id="d1802-150">"server" is the name of the server that is running Redis.</span></span>
- <span data-ttu-id="d1802-151">*Port* é o número da porta</span><span class="sxs-lookup"><span data-stu-id="d1802-151">*port* is the port number</span></span>
- <span data-ttu-id="d1802-152">"password" é a senha que você definiu no arquivo Redis. conf.</span><span class="sxs-lookup"><span data-stu-id="d1802-152">"password" is the password that you defined in the redis.conf file.</span></span>
- <span data-ttu-id="d1802-153">"AppName" é qualquer cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="d1802-153">"AppName" is any string.</span></span> <span data-ttu-id="d1802-154">O signalr cria um Redis pub/sub Channel com esse nome.</span><span class="sxs-lookup"><span data-stu-id="d1802-154">SignalR creates a Redis pub/sub channel with this name.</span></span>

<span data-ttu-id="d1802-155">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="d1802-155">For example:</span></span>

[!code-csharp[Main](scaleout-with-redis/samples/sample7.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="d1802-156">Implantar e executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="d1802-156">Deploy and Run the Application</span></span>

<span data-ttu-id="d1802-157">Prepare suas instâncias do Windows Server para implantar o aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="d1802-157">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="d1802-158">Adicione a função IIS.</span><span class="sxs-lookup"><span data-stu-id="d1802-158">Add the IIS role.</span></span> <span data-ttu-id="d1802-159">Inclua recursos de "desenvolvimento de aplicativos", incluindo o protocolo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d1802-159">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-redis/_static/image5.png)

<span data-ttu-id="d1802-160">Inclua também o serviço de gerenciamento (listado em "ferramentas de gerenciamento").</span><span class="sxs-lookup"><span data-stu-id="d1802-160">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-redis/_static/image6.png)

<span data-ttu-id="d1802-161">**Instale o Implantação da Web 3,0.**</span><span class="sxs-lookup"><span data-stu-id="d1802-161">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="d1802-162">Quando você executar o Gerenciador do IIS, ele solicitará que você instale a plataforma Web da Microsoft ou você poderá [baixar o instalador](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="d1802-162">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="d1802-163">No instalador da plataforma, procure Implantação da Web e instale o Implantação da Web 3,0</span><span class="sxs-lookup"><span data-stu-id="d1802-163">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-redis/_static/image7.png)

<span data-ttu-id="d1802-164">Verifique se o serviço de gerenciamento da Web está em execução.</span><span class="sxs-lookup"><span data-stu-id="d1802-164">Check that the Web Management Service is running.</span></span> <span data-ttu-id="d1802-165">Caso contrário, inicie o serviço.</span><span class="sxs-lookup"><span data-stu-id="d1802-165">If not, start the service.</span></span> <span data-ttu-id="d1802-166">(Se você não vir o serviço de gerenciamento da Web na lista de serviços do Windows, certifique-se de que você instalou o serviço de gerenciamento quando adicionou a função do IIS.)</span><span class="sxs-lookup"><span data-stu-id="d1802-166">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="d1802-167">Por padrão, o serviço de gerenciamento da Web escuta na porta TCP 8172.</span><span class="sxs-lookup"><span data-stu-id="d1802-167">By default, the Web Management Service listens on TCP port 8172.</span></span> <span data-ttu-id="d1802-168">No firewall do Windows, crie uma nova regra de entrada para permitir o tráfego TCP na porta 8172.</span><span class="sxs-lookup"><span data-stu-id="d1802-168">In Windows Firewall, create a new inbound rule to allow TCP traffic on port 8172.</span></span> <span data-ttu-id="d1802-169">Para obter mais informações, consulte [Configurando regras de firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span><span class="sxs-lookup"><span data-stu-id="d1802-169">For more information, see [Configuring Firewall Rules](https://technet.microsoft.com/library/dd448559(WS.10).aspx).</span></span> <span data-ttu-id="d1802-170">(Se você estiver hospedando as VMs no Azure, poderá fazer isso diretamente na portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="d1802-170">(If you are hosting the VMs on Azure, you can do this directly in the Azure portal.</span></span> <span data-ttu-id="d1802-171">Consulte [como configurar pontos de extremidade para uma máquina virtual](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span><span class="sxs-lookup"><span data-stu-id="d1802-171">See [How to Set Up Endpoints to a Virtual Machine](https://azure.microsoft.com/documentation/articles/virtual-machines-set-up-endpoints/).)</span></span>

<span data-ttu-id="d1802-172">Agora você está pronto para implantar o projeto do Visual Studio do computador de desenvolvimento no servidor.</span><span class="sxs-lookup"><span data-stu-id="d1802-172">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="d1802-173">Em Gerenciador de Soluções, clique com o botão direito do mouse na solução e clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="d1802-173">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="d1802-174">Para obter uma documentação mais detalhada sobre a implantação da Web, consulte [mapa de conteúdo de implantação da Web para Visual Studio e ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="d1802-174">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="d1802-175">Se você implantar o aplicativo em dois servidores, poderá abrir cada instância em uma janela separada do navegador e ver que cada uma recebe mensagens do Signalr da outra.</span><span class="sxs-lookup"><span data-stu-id="d1802-175">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="d1802-176">(Naturalmente, em um ambiente de produção, os dois servidores ficam atrás de um balanceador de carga.)</span><span class="sxs-lookup"><span data-stu-id="d1802-176">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-redis/_static/image8.png)

<span data-ttu-id="d1802-177">Se você estiver curioso para ver as mensagens que são enviadas para o Redis, você pode usar o cliente **Redis-CLI** , que é instalado com o Redis.</span><span class="sxs-lookup"><span data-stu-id="d1802-177">If you're curious to see the messages that are sent to Redis, you can use the **redis-cli** client, which installs with Redis.</span></span>

[!code-xml[Main](scaleout-with-redis/samples/sample8.xml)]

![](scaleout-with-redis/_static/image9.png)
