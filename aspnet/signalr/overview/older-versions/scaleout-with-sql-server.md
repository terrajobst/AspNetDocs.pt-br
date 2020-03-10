---
uid: signalr/overview/older-versions/scaleout-with-sql-server
title: Escalador de sinalização com SQL Server (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: ''
ms.author: bradyg
ms.date: 05/01/2013
ms.assetid: 1dca7967-8296-444a-9533-837eb284e78c
msc.legacyurl: /signalr/overview/older-versions/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 8674b06a2a751c9a7b1c6c9b19782974d0602a62
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536466"
---
# <a name="signalr-scaleout-with-sql-server-signalr-1x"></a><span data-ttu-id="1b2d3-102">Expansão do SignalR com o SQL Server (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="1b2d3-102">SignalR Scaleout with SQL Server (SignalR 1.x)</span></span>

<span data-ttu-id="1b2d3-103">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="1b2d3-103">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

<span data-ttu-id="1b2d3-104">Neste tutorial, você usará SQL Server para distribuir mensagens em um aplicativo Signalr que é implantado em duas instâncias do IIS separadas.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-104">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="1b2d3-105">Você também pode executar este tutorial em um único computador de teste, mas para obter o efeito completo, você precisa implantar o aplicativo Signalr em dois ou mais servidores.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-105">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="1b2d3-106">Você também deve instalar o SQL Server em um dos servidores ou em um servidor dedicado separado.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-106">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="1b2d3-107">Outra opção é executar o tutorial usando VMs no Azure.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-107">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="1b2d3-108">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="1b2d3-108">Prerequisites</span></span>

<span data-ttu-id="1b2d3-109">Microsoft SQL Server 2005 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-109">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="1b2d3-110">O backplane dá suporte a edições de desktop e de servidor do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-110">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="1b2d3-111">Ele não dá suporte à edição SQL Server Compact ou ao banco de dados SQL do Azure.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-111">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="1b2d3-112">(Se seu aplicativo estiver hospedado no Azure, considere o backplane do barramento de serviço.)</span><span class="sxs-lookup"><span data-stu-id="1b2d3-112">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="1b2d3-113">Visão geral</span><span class="sxs-lookup"><span data-stu-id="1b2d3-113">Overview</span></span>

<span data-ttu-id="1b2d3-114">Antes de chegarmos ao tutorial detalhado, aqui está uma visão geral rápida do que você fará.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-114">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="1b2d3-115">Crie um novo banco de dados vazio.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-115">Create a new empty database.</span></span> <span data-ttu-id="1b2d3-116">O backplane criará as tabelas necessárias neste banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-116">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="1b2d3-117">Adicione esses pacotes NuGet ao seu aplicativo:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-117">Add these NuGet packages to your application:</span></span> 

    - [<span data-ttu-id="1b2d3-118">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="1b2d3-118">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="1b2d3-119">Microsoft. AspNet. Signalr. SqlServer</span><span class="sxs-lookup"><span data-stu-id="1b2d3-119">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="1b2d3-120">Crie um aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-120">Create a SignalR application.</span></span>
4. <span data-ttu-id="1b2d3-121">Adicione o seguinte código ao global. asax para configurar o backplane:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-121">Add the following code to Global.asax to configure the backplane:</span></span> 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

## <a name="configure-the-database"></a><span data-ttu-id="1b2d3-122">Configurar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="1b2d3-122">Configure the Database</span></span>

<span data-ttu-id="1b2d3-123">Decida se o aplicativo usará a autenticação do Windows ou SQL Server autenticação para acessar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-123">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="1b2d3-124">Independentemente, verifique se o usuário do banco de dados tem permissões para fazer logon, criar esquemas e criar tabelas.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-124">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="1b2d3-125">Crie um novo banco de dados para o backplane usar.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-125">Create a new database for the backplane to use.</span></span> <span data-ttu-id="1b2d3-126">Você pode dar qualquer nome ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-126">You can give the database any name.</span></span> <span data-ttu-id="1b2d3-127">Você não precisa criar nenhuma tabela no banco de dados; o backplane criará as tabelas necessárias.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-127">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="1b2d3-128">Habilitar Service Broker</span><span class="sxs-lookup"><span data-stu-id="1b2d3-128">Enable Service Broker</span></span>

<span data-ttu-id="1b2d3-129">É recomendável habilitar Service Broker para o banco de dados do backplane.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-129">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="1b2d3-130">O Service Broker fornece suporte nativo para mensagens e enfileiramento no SQL Server, o que permite que o backplane Receba atualizações com mais eficiência.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-130">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="1b2d3-131">(No entanto, o backplane também funciona sem Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="1b2d3-131">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="1b2d3-132">Para verificar se Service Broker está habilitado, consulte a coluna **is\_Broker\_Enabled** na exibição do catálogo **Sys. databases** .</span><span class="sxs-lookup"><span data-stu-id="1b2d3-132">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="1b2d3-133">Para habilitar Service Broker, use a seguinte consulta SQL:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-133">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="1b2d3-134">Se essa consulta aparecer para deadlock, verifique se não há aplicativos conectados ao BD.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-134">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<span data-ttu-id="1b2d3-135">Se você tiver habilitado o rastreamento, os rastreamentos também mostrarão se Service Broker está habilitado.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-135">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="1b2d3-136">Criar um aplicativo Signalr</span><span class="sxs-lookup"><span data-stu-id="1b2d3-136">Create a SignalR Application</span></span>

<span data-ttu-id="1b2d3-137">Crie um aplicativo Signalr seguindo um destes tutoriais:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-137">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="1b2d3-138">Introdução com Signalr</span><span class="sxs-lookup"><span data-stu-id="1b2d3-138">Getting Started with SignalR</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="1b2d3-139">Introdução com Signalr e MVC 4</span><span class="sxs-lookup"><span data-stu-id="1b2d3-139">Getting Started with SignalR and MVC 4</span></span>](tutorial-getting-started-with-signalr-and-mvc-4.md)

<span data-ttu-id="1b2d3-140">Em seguida, modificaremos o aplicativo de chat para dar suporte ao scale out com SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-140">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="1b2d3-141">Primeiro, adicione o pacote NuGet do Signalr. SqlServer ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-141">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="1b2d3-142">No Visual Studio, no menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-142">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="1b2d3-143">Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-143">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="1b2d3-144">Em seguida, abra o arquivo global. asax.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-144">Next, open the Global.asax file.</span></span> <span data-ttu-id="1b2d3-145">Adicione o seguinte código ao **aplicativo\_** método de início:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-145">Add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="1b2d3-146">Implantar e executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="1b2d3-146">Deploy and Run the Application</span></span>

<span data-ttu-id="1b2d3-147">Prepare suas instâncias do Windows Server para implantar o aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-147">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="1b2d3-148">Adicione a função IIS.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-148">Add the IIS role.</span></span> <span data-ttu-id="1b2d3-149">Inclua recursos de "desenvolvimento de aplicativos", incluindo o protocolo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-149">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="1b2d3-150">Inclua também o serviço de gerenciamento (listado em "ferramentas de gerenciamento").</span><span class="sxs-lookup"><span data-stu-id="1b2d3-150">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="1b2d3-151">**Instale o Implantação da Web 3,0.**</span><span class="sxs-lookup"><span data-stu-id="1b2d3-151">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="1b2d3-152">Quando você executar o Gerenciador do IIS, ele solicitará que você instale a plataforma Web da Microsoft ou você poderá [baixar o instalador](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="1b2d3-152">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="1b2d3-153">No instalador da plataforma, procure Implantação da Web e instale o Implantação da Web 3,0</span><span class="sxs-lookup"><span data-stu-id="1b2d3-153">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="1b2d3-154">Verifique se o serviço de gerenciamento da Web está em execução.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-154">Check that the Web Management Service is running.</span></span> <span data-ttu-id="1b2d3-155">Caso contrário, inicie o serviço.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-155">If not, start the service.</span></span> <span data-ttu-id="1b2d3-156">(Se você não vir o serviço de gerenciamento da Web na lista de serviços do Windows, certifique-se de que você instalou o serviço de gerenciamento quando adicionou a função do IIS.)</span><span class="sxs-lookup"><span data-stu-id="1b2d3-156">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="1b2d3-157">Por fim, abra a porta 8172 para TCP.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-157">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="1b2d3-158">Essa é a porta que a ferramenta de Implantação da Web usa.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-158">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="1b2d3-159">Agora você está pronto para implantar o projeto do Visual Studio do computador de desenvolvimento no servidor.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-159">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="1b2d3-160">Em Gerenciador de Soluções, clique com o botão direito do mouse na solução e clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-160">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="1b2d3-161">Para obter uma documentação mais detalhada sobre a implantação da Web, consulte [mapa de conteúdo de implantação da Web para Visual Studio e ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="1b2d3-161">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="1b2d3-162">Se você implantar o aplicativo em dois servidores, poderá abrir cada instância em uma janela separada do navegador e ver que cada uma recebe mensagens do Signalr da outra.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-162">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="1b2d3-163">(Naturalmente, em um ambiente de produção, os dois servidores ficam atrás de um balanceador de carga.)</span><span class="sxs-lookup"><span data-stu-id="1b2d3-163">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="1b2d3-164">Depois de executar o aplicativo, você pode ver que o Signalr criou automaticamente as tabelas no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="1b2d3-164">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="1b2d3-165">O signalr gerencia as tabelas.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-165">SignalR manages the tables.</span></span> <span data-ttu-id="1b2d3-166">Desde que seu aplicativo seja implantado, não exclua linhas, modifique a tabela e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="1b2d3-166">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
