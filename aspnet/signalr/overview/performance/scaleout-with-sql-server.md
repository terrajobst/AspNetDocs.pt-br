---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Dimensionamento do signalr com SQL Server | Microsoft Docs
author: bradygaster
description: Versões de software usadas neste tópico Visual Studio 2013 o .NET 4,5 Signalr versão 2 versões anteriores deste tópico para obter informações sobre versões anteriores do...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: 709a9ebf8f3396842bee0d87e621c00ae1418ec1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579180"
---
# <a name="signalr-scaleout-with-sql-server"></a><span data-ttu-id="024bb-103">Expansão do SignalR com o SQL Server</span><span class="sxs-lookup"><span data-stu-id="024bb-103">SignalR Scaleout with SQL Server</span></span>

<span data-ttu-id="024bb-104">por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="024bb-104">by [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="024bb-105">Versões de software usadas neste tópico</span><span class="sxs-lookup"><span data-stu-id="024bb-105">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="024bb-106">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="024bb-106">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="024bb-107">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="024bb-107">.NET 4.5</span></span>
> - <span data-ttu-id="024bb-108">Sinalização versão 2</span><span class="sxs-lookup"><span data-stu-id="024bb-108">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="024bb-109">Versões anteriores deste tópico</span><span class="sxs-lookup"><span data-stu-id="024bb-109">Previous versions of this topic</span></span>
>
> <span data-ttu-id="024bb-110">Para obter informações sobre versões anteriores do Signalr, confira [versões mais antigas do signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="024bb-110">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="024bb-111">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="024bb-111">Questions and comments</span></span>
>
> <span data-ttu-id="024bb-112">Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="024bb-112">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="024bb-113">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="024bb-113">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

<span data-ttu-id="024bb-114">Neste tutorial, você usará SQL Server para distribuir mensagens em um aplicativo Signalr que é implantado em duas instâncias do IIS separadas.</span><span class="sxs-lookup"><span data-stu-id="024bb-114">In this tutorial, you will use SQL Server to distribute messages across a SignalR application that is deployed in two separate IIS instances.</span></span> <span data-ttu-id="024bb-115">Você também pode executar este tutorial em um único computador de teste, mas para obter o efeito completo, você precisa implantar o aplicativo Signalr em dois ou mais servidores.</span><span class="sxs-lookup"><span data-stu-id="024bb-115">You can also run this tutorial on a single test machine, but to get the full effect, you need to deploy the SignalR application to two or more servers.</span></span> <span data-ttu-id="024bb-116">Você também deve instalar o SQL Server em um dos servidores ou em um servidor dedicado separado.</span><span class="sxs-lookup"><span data-stu-id="024bb-116">You must also install SQL Server on one of the servers, or on a separate dedicated server.</span></span> <span data-ttu-id="024bb-117">Outra opção é executar o tutorial usando VMs no Azure.</span><span class="sxs-lookup"><span data-stu-id="024bb-117">Another option is to run the tutorial using VMs on Azure.</span></span>

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a><span data-ttu-id="024bb-118">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="024bb-118">Prerequisites</span></span>

<span data-ttu-id="024bb-119">Microsoft SQL Server 2005 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="024bb-119">Microsoft SQL Server 2005 or later.</span></span> <span data-ttu-id="024bb-120">O backplane dá suporte a edições de desktop e de servidor do SQL Server.</span><span class="sxs-lookup"><span data-stu-id="024bb-120">The backplane supports both desktop and server editions of SQL Server.</span></span> <span data-ttu-id="024bb-121">Ele não dá suporte à edição SQL Server Compact ou ao banco de dados SQL do Azure.</span><span class="sxs-lookup"><span data-stu-id="024bb-121">It does not support SQL Server Compact Edition or Azure SQL Database.</span></span> <span data-ttu-id="024bb-122">(Se seu aplicativo estiver hospedado no Azure, considere o backplane do barramento de serviço.)</span><span class="sxs-lookup"><span data-stu-id="024bb-122">(If your application is hosted on Azure, consider the Service Bus backplane instead.)</span></span>

## <a name="overview"></a><span data-ttu-id="024bb-123">Visão geral</span><span class="sxs-lookup"><span data-stu-id="024bb-123">Overview</span></span>

<span data-ttu-id="024bb-124">Antes de chegarmos ao tutorial detalhado, aqui está uma visão geral rápida do que você fará.</span><span class="sxs-lookup"><span data-stu-id="024bb-124">Before we get to the detailed tutorial, here is a quick overview of what you will do.</span></span>

1. <span data-ttu-id="024bb-125">Crie um novo banco de dados vazio.</span><span class="sxs-lookup"><span data-stu-id="024bb-125">Create a new empty database.</span></span> <span data-ttu-id="024bb-126">O backplane criará as tabelas necessárias neste banco de dados.</span><span class="sxs-lookup"><span data-stu-id="024bb-126">The backplane will create the necessary tables in this database.</span></span>
2. <span data-ttu-id="024bb-127">Adicione esses pacotes NuGet ao seu aplicativo:</span><span class="sxs-lookup"><span data-stu-id="024bb-127">Add these NuGet packages to your application:</span></span>

    - [<span data-ttu-id="024bb-128">Microsoft. AspNet. Signalr</span><span class="sxs-lookup"><span data-stu-id="024bb-128">Microsoft.AspNet.SignalR</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [<span data-ttu-id="024bb-129">Microsoft. AspNet. Signalr. SqlServer</span><span class="sxs-lookup"><span data-stu-id="024bb-129">Microsoft.AspNet.SignalR.SqlServer</span></span>](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. <span data-ttu-id="024bb-130">Crie um aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="024bb-130">Create a SignalR application.</span></span>
4. <span data-ttu-id="024bb-131">Adicione o seguinte código a Startup.cs para configurar o backplane:</span><span class="sxs-lookup"><span data-stu-id="024bb-131">Add the following code to Startup.cs to configure the backplane:</span></span>

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   <span data-ttu-id="024bb-132">Esse código configura o backplane com os valores padrão para [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) e [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span><span class="sxs-lookup"><span data-stu-id="024bb-132">This code configures the backplane with the default values for [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) and [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx).</span></span> <span data-ttu-id="024bb-133">Para obter informações sobre como alterar esses valores, consulte [desempenho do signalr: métricas de scale](signalr-performance.md#scaleout_metrics)out.</span><span class="sxs-lookup"><span data-stu-id="024bb-133">For information on changing these values, see [SignalR Performance: Scaleout Metrics](signalr-performance.md#scaleout_metrics).</span></span>

## <a name="configure-the-database"></a><span data-ttu-id="024bb-134">Configurar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="024bb-134">Configure the Database</span></span>

<span data-ttu-id="024bb-135">Decida se o aplicativo usará a autenticação do Windows ou SQL Server autenticação para acessar o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="024bb-135">Decide whether the application will use Windows authentication or SQL Server authentication to access the database.</span></span> <span data-ttu-id="024bb-136">Independentemente, verifique se o usuário do banco de dados tem permissões para fazer logon, criar esquemas e criar tabelas.</span><span class="sxs-lookup"><span data-stu-id="024bb-136">Regardless, make sure the database user has permissions to log in, create schemas, and create tables.</span></span>

<span data-ttu-id="024bb-137">Crie um novo banco de dados para o backplane usar.</span><span class="sxs-lookup"><span data-stu-id="024bb-137">Create a new database for the backplane to use.</span></span> <span data-ttu-id="024bb-138">Você pode dar qualquer nome ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="024bb-138">You can give the database any name.</span></span> <span data-ttu-id="024bb-139">Você não precisa criar nenhuma tabela no banco de dados; o backplane criará as tabelas necessárias.</span><span class="sxs-lookup"><span data-stu-id="024bb-139">You don't need to create any tables in the database; the backplane will create the necessary tables.</span></span>

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a><span data-ttu-id="024bb-140">Habilitar Service Broker</span><span class="sxs-lookup"><span data-stu-id="024bb-140">Enable Service Broker</span></span>

<span data-ttu-id="024bb-141">É recomendável habilitar Service Broker para o banco de dados do backplane.</span><span class="sxs-lookup"><span data-stu-id="024bb-141">It is recommended to enable Service Broker for the backplane database.</span></span> <span data-ttu-id="024bb-142">O Service Broker fornece suporte nativo para mensagens e enfileiramento no SQL Server, o que permite que o backplane Receba atualizações com mais eficiência.</span><span class="sxs-lookup"><span data-stu-id="024bb-142">Service Broker provides native support for messaging and queuing in SQL Server, which lets the backplane receive updates more efficiently.</span></span> <span data-ttu-id="024bb-143">(No entanto, o backplane também funciona sem Service Broker.)</span><span class="sxs-lookup"><span data-stu-id="024bb-143">(However, the backplane also works without Service Broker.)</span></span>

<span data-ttu-id="024bb-144">Para verificar se Service Broker está habilitado, consulte a coluna **is\_Broker\_Enabled** na exibição do catálogo **Sys. databases** .</span><span class="sxs-lookup"><span data-stu-id="024bb-144">To check whether Service Broker is enabled, query the **is\_broker\_enabled** column in the **sys.databases** catalog view.</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

<span data-ttu-id="024bb-145">Para habilitar Service Broker, use a seguinte consulta SQL:</span><span class="sxs-lookup"><span data-stu-id="024bb-145">To enable Service Broker, use the following SQL query:</span></span>

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> <span data-ttu-id="024bb-146">Se essa consulta aparecer para deadlock, verifique se não há aplicativos conectados ao BD.</span><span class="sxs-lookup"><span data-stu-id="024bb-146">If this query appears to deadlock, make sure there are no applications connected to the DB.</span></span>

<span data-ttu-id="024bb-147">Se você tiver habilitado o rastreamento, os rastreamentos também mostrarão se Service Broker está habilitado.</span><span class="sxs-lookup"><span data-stu-id="024bb-147">If you have enabled tracing, the traces will also show whether Service Broker is enabled.</span></span>

## <a name="create-a-signalr-application"></a><span data-ttu-id="024bb-148">Criar um aplicativo Signalr</span><span class="sxs-lookup"><span data-stu-id="024bb-148">Create a SignalR Application</span></span>

<span data-ttu-id="024bb-149">Crie um aplicativo Signalr seguindo um destes tutoriais:</span><span class="sxs-lookup"><span data-stu-id="024bb-149">Create a SignalR application by following either of these tutorials:</span></span>

- [<span data-ttu-id="024bb-150">Introdução com Signalr 2,0</span><span class="sxs-lookup"><span data-stu-id="024bb-150">Getting Started with SignalR 2.0</span></span>](../getting-started/tutorial-getting-started-with-signalr.md)
- [<span data-ttu-id="024bb-151">Introdução com Signalr 2,0 e MVC 5</span><span class="sxs-lookup"><span data-stu-id="024bb-151">Getting Started with SignalR 2.0 and MVC 5</span></span>](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

<span data-ttu-id="024bb-152">Em seguida, modificaremos o aplicativo de chat para dar suporte ao scale out com SQL Server.</span><span class="sxs-lookup"><span data-stu-id="024bb-152">Next, we'll modify the chat application to support scaleout with SQL Server.</span></span> <span data-ttu-id="024bb-153">Primeiro, adicione o pacote NuGet do Signalr. SqlServer ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="024bb-153">First, add the SignalR.SqlServer NuGet package to your project.</span></span> <span data-ttu-id="024bb-154">No Visual Studio, no menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="024bb-154">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="024bb-155">Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="024bb-155">In the Package Manager Console window, enter the following command:</span></span>

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

<span data-ttu-id="024bb-156">Em seguida, abra o arquivo Startup.cs.</span><span class="sxs-lookup"><span data-stu-id="024bb-156">Next, open the Startup.cs file.</span></span> <span data-ttu-id="024bb-157">Adicione o seguinte código ao método **Configure** :</span><span class="sxs-lookup"><span data-stu-id="024bb-157">Add the following code to the **Configure** method:</span></span>

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a><span data-ttu-id="024bb-158">Implantar e executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="024bb-158">Deploy and Run the Application</span></span>

<span data-ttu-id="024bb-159">Prepare suas instâncias do Windows Server para implantar o aplicativo Signalr.</span><span class="sxs-lookup"><span data-stu-id="024bb-159">Prepare your Windows Server instances to deploy the SignalR application.</span></span>

<span data-ttu-id="024bb-160">Adicione a função IIS.</span><span class="sxs-lookup"><span data-stu-id="024bb-160">Add the IIS role.</span></span> <span data-ttu-id="024bb-161">Inclua recursos de "desenvolvimento de aplicativos", incluindo o protocolo WebSocket.</span><span class="sxs-lookup"><span data-stu-id="024bb-161">Include "Application Development" features, including the WebSocket Protocol.</span></span>

![](scaleout-with-sql-server/_static/image4.png)

<span data-ttu-id="024bb-162">Inclua também o serviço de gerenciamento (listado em "ferramentas de gerenciamento").</span><span class="sxs-lookup"><span data-stu-id="024bb-162">Also include the Management Service (listed under "Management Tools").</span></span>

![](scaleout-with-sql-server/_static/image5.png)

<span data-ttu-id="024bb-163">**Instale o Implantação da Web 3,0.**</span><span class="sxs-lookup"><span data-stu-id="024bb-163">**Install Web Deploy 3.0.**</span></span> <span data-ttu-id="024bb-164">Quando você executar o Gerenciador do IIS, ele solicitará que você instale a plataforma Web da Microsoft ou você poderá [baixar o instalador](https://go.microsoft.com/fwlink/?LinkId=255386).</span><span class="sxs-lookup"><span data-stu-id="024bb-164">When you run IIS Manager, it will prompt you to install Microsoft Web Platform, or you can [download the installer](https://go.microsoft.com/fwlink/?LinkId=255386).</span></span> <span data-ttu-id="024bb-165">No instalador da plataforma, procure Implantação da Web e instale o Implantação da Web 3,0</span><span class="sxs-lookup"><span data-stu-id="024bb-165">In the Platform Installer, search for Web Deploy and install Web Deploy 3.0</span></span>

![](scaleout-with-sql-server/_static/image6.png)

<span data-ttu-id="024bb-166">Verifique se o serviço de gerenciamento da Web está em execução.</span><span class="sxs-lookup"><span data-stu-id="024bb-166">Check that the Web Management Service is running.</span></span> <span data-ttu-id="024bb-167">Caso contrário, inicie o serviço.</span><span class="sxs-lookup"><span data-stu-id="024bb-167">If not, start the service.</span></span> <span data-ttu-id="024bb-168">(Se você não vir o serviço de gerenciamento da Web na lista de serviços do Windows, certifique-se de que você instalou o serviço de gerenciamento quando adicionou a função do IIS.)</span><span class="sxs-lookup"><span data-stu-id="024bb-168">(If you don't see Web Management Service in the list of Windows services, make sure that you installed the Management Service when you added the IIS role.)</span></span>

<span data-ttu-id="024bb-169">Por fim, abra a porta 8172 para TCP.</span><span class="sxs-lookup"><span data-stu-id="024bb-169">Finally, open port 8172 for TCP.</span></span> <span data-ttu-id="024bb-170">Essa é a porta que a ferramenta de Implantação da Web usa.</span><span class="sxs-lookup"><span data-stu-id="024bb-170">This is the port that the Web Deploy tool uses.</span></span>

<span data-ttu-id="024bb-171">Agora você está pronto para implantar o projeto do Visual Studio do computador de desenvolvimento no servidor.</span><span class="sxs-lookup"><span data-stu-id="024bb-171">Now you are ready to deploy the Visual Studio project from your development machine to the server.</span></span> <span data-ttu-id="024bb-172">Em Gerenciador de Soluções, clique com o botão direito do mouse na solução e clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="024bb-172">In Solution Explorer, right-click the solution and click **Publish**.</span></span>

<span data-ttu-id="024bb-173">Para obter uma documentação mais detalhada sobre a implantação da Web, consulte [mapa de conteúdo de implantação da Web para Visual Studio e ASP.net](../../../whitepapers/aspnet-web-deployment-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="024bb-173">For more detailed documentation about web deployment, see [Web Deployment Content Map for Visual Studio and ASP.NET](../../../whitepapers/aspnet-web-deployment-content-map.md).</span></span>

<span data-ttu-id="024bb-174">Se você implantar o aplicativo em dois servidores, poderá abrir cada instância em uma janela separada do navegador e ver que cada uma recebe mensagens do Signalr da outra.</span><span class="sxs-lookup"><span data-stu-id="024bb-174">If you deploy the application to two servers, you can open each instance in a separate browser window and see that they each receive SignalR messages from the other.</span></span> <span data-ttu-id="024bb-175">(Naturalmente, em um ambiente de produção, os dois servidores ficam atrás de um balanceador de carga.)</span><span class="sxs-lookup"><span data-stu-id="024bb-175">(Of course, in a production environment, the two servers would sit behind a load balancer.)</span></span>

![](scaleout-with-sql-server/_static/image7.png)

<span data-ttu-id="024bb-176">Depois de executar o aplicativo, você pode ver que o Signalr criou automaticamente as tabelas no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="024bb-176">After you run the application, you can see that SignalR has automatically created tables in the database:</span></span>

![](scaleout-with-sql-server/_static/image8.png)

<span data-ttu-id="024bb-177">O signalr gerencia as tabelas.</span><span class="sxs-lookup"><span data-stu-id="024bb-177">SignalR manages the tables.</span></span> <span data-ttu-id="024bb-178">Desde que seu aplicativo seja implantado, não exclua linhas, modifique a tabela e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="024bb-178">As long as your application is deployed, don't delete rows, modify the table, and so forth.</span></span>
