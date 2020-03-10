---
uid: signalr/overview/deployment/using-signalr-with-azure-web-sites
title: Usando o Signalr com aplicativos Web no serviço Azure App | Microsoft Docs
author: bradygaster
description: Este documento descreve como configurar um aplicativo Signalr que é executado no Microsoft Azure. Versões de software usadas no tutorial Visual Studio 2013 ou vis...
ms.author: bradyg
ms.date: 07/01/2015
ms.assetid: 2a7517a0-b88c-4162-ade3-9bf6ca7062fd
msc.legacyurl: /signalr/overview/deployment/using-signalr-with-azure-web-sites
msc.type: authoredcontent
ms.openlocfilehash: 0e6a18bdbe9cc47e89b5a458753845afb53595f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558698"
---
# <a name="using-signalr-with-web-apps-in-azure-app-service"></a><span data-ttu-id="c33cf-104">Usar o SignalR com aplicativos Web no Serviço de Aplicativo do Azure</span><span class="sxs-lookup"><span data-stu-id="c33cf-104">Using SignalR with Web Apps in Azure App Service</span></span>

<span data-ttu-id="c33cf-105">por [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="c33cf-105">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="c33cf-106">Este documento descreve como configurar um aplicativo Signalr que é executado no Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="c33cf-106">This document describes how to configure a SignalR application that runs on Microsoft Azure.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c33cf-107">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="c33cf-107">Software versions used in the tutorial</span></span>
>
>
> - <span data-ttu-id="c33cf-108">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) ou Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="c33cf-108">[Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) or Visual Studio 2012</span></span>
> - <span data-ttu-id="c33cf-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="c33cf-109">.NET 4.5</span></span>
> - <span data-ttu-id="c33cf-110">Sinalização versão 2</span><span class="sxs-lookup"><span data-stu-id="c33cf-110">SignalR version 2</span></span>
> - <span data-ttu-id="c33cf-111">SDK do Azure 2,3 para Visual Studio 2013 ou 2012</span><span class="sxs-lookup"><span data-stu-id="c33cf-111">Azure SDK 2.3 for Visual Studio 2013 or 2012</span></span>
>
>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="c33cf-112">Perguntas e comentários</span><span class="sxs-lookup"><span data-stu-id="c33cf-112">Questions and comments</span></span>
>
> <span data-ttu-id="c33cf-113">Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="c33cf-113">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="c33cf-114">Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, você poderá lançá-las no [Fórum do signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), no [stackoverflow.com](http://stackoverflow.com/)ou nos [fóruns de Microsoft Azure](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span><span class="sxs-lookup"><span data-stu-id="c33cf-114">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR), [StackOverflow.com](http://stackoverflow.com/), or the [Microsoft Azure forums](https://social.msdn.microsoft.com/Forums/windowsazure/home?category=windowsazureplatform).</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="c33cf-115">Sumário</span><span class="sxs-lookup"><span data-stu-id="c33cf-115">Table of Contents</span></span>

- [<span data-ttu-id="c33cf-116">Introdução</span><span class="sxs-lookup"><span data-stu-id="c33cf-116">Introduction</span></span>](#introduction)
- [<span data-ttu-id="c33cf-117">Implantando um aplicativo Web sinalizador no serviço Azure App</span><span class="sxs-lookup"><span data-stu-id="c33cf-117">Deploying a SignalR Web App to Azure App Service</span></span>](#deploying)
- [<span data-ttu-id="c33cf-118">Habilitando WebSockets no serviço Azure App</span><span class="sxs-lookup"><span data-stu-id="c33cf-118">Enabling WebSockets on Azure App Service</span></span>](#websocket)
- [<span data-ttu-id="c33cf-119">Usando o backplane do cache Redis do Azure</span><span class="sxs-lookup"><span data-stu-id="c33cf-119">Using the Azure Redis Cache Backplane</span></span>](#backplane)
- [<span data-ttu-id="c33cf-120">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="c33cf-120">Next Steps</span></span>](#nextsteps)

<a id="introduction"></a>
## <a name="introduction"></a><span data-ttu-id="c33cf-121">Introdução</span><span class="sxs-lookup"><span data-stu-id="c33cf-121">Introduction</span></span>

<span data-ttu-id="c33cf-122">O Signalr ASP.NET pode ser usado para trazer um novo nível de interatividade entre servidores e clientes Web ou .NET.</span><span class="sxs-lookup"><span data-stu-id="c33cf-122">ASP.NET SignalR can be used to bring a new level of interactivity between servers and web or .NET clients.</span></span> <span data-ttu-id="c33cf-123">Quando hospedados no Azure, os aplicativos Signalr podem aproveitar o ambiente altamente disponível, escalonável e de alto desempenho que é executado na nuvem.</span><span class="sxs-lookup"><span data-stu-id="c33cf-123">When hosted in Azure, SignalR applications can take advantage of the highly available, scalable, and performant environment that running in the cloud provides.</span></span>

<a id="deploying"></a>
## <a name="deploying-a-signalr-web-app-to-azure-app-service"></a><span data-ttu-id="c33cf-124">Implantando um aplicativo Web sinalizador no serviço Azure App</span><span class="sxs-lookup"><span data-stu-id="c33cf-124">Deploying a SignalR Web App to Azure App Service</span></span>

<span data-ttu-id="c33cf-125">O signalr não adiciona complicações específicas à implantação de um aplicativo no Azure em vez de implantar em um servidor local.</span><span class="sxs-lookup"><span data-stu-id="c33cf-125">SignalR doesn't add any particular complications to deploying an application to Azure versus deploying to an on-premises server.</span></span> <span data-ttu-id="c33cf-126">Um aplicativo que usa o Signalr pode ser hospedado no Azure sem nenhuma alteração na configuração ou outras configurações (no entanto, para suporte a WebSockets, consulte [habilitando WebSockets no serviço Azure app](#websocket) abaixo.) Para este tutorial, você implantará o aplicativo criado no [tutorial introdução](../getting-started/tutorial-getting-started-with-signalr.md) no Azure.</span><span class="sxs-lookup"><span data-stu-id="c33cf-126">An application that uses SignalR can be hosted in Azure without any changes in configuration or other settings (though for WebSockets support, see [Enabling WebSockets on Azure App Service](#websocket) below.) For this tutorial, you'll deploy the application created in the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) to Azure.</span></span>

<span data-ttu-id="c33cf-127">**Pré-requisitos**</span><span class="sxs-lookup"><span data-stu-id="c33cf-127">**Prerequisites**</span></span>

- <span data-ttu-id="c33cf-128">Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c33cf-128">Visual Studio 2013.</span></span> <span data-ttu-id="c33cf-129">Se você não tiver o Visual Studio, o Visual Studio 2013 Express para Web estará incluído na instalação do SDK do Azure.</span><span class="sxs-lookup"><span data-stu-id="c33cf-129">If you don't have Visual Studio, Visual Studio 2013 Express for Web is included in the install of the Azure SDK.</span></span>
- <span data-ttu-id="c33cf-130">[SDK do azure 2,3 para Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) ou [sdk do Azure 2,3 para Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span><span class="sxs-lookup"><span data-stu-id="c33cf-130">[Azure SDK 2.3 for Visual Studio 2013](https://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409) or [Azure SDK 2.3 for Visual Studio 2012](https://go.microsoft.com/fwlink/p/?linkid=323511).</span></span>
- <span data-ttu-id="c33cf-131">Para concluir este tutorial, você precisará de uma assinatura do Azure.</span><span class="sxs-lookup"><span data-stu-id="c33cf-131">To complete this tutorial, you will need an Azure subscription.</span></span> <span data-ttu-id="c33cf-132">Você pode [ativar seus benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)ou [inscrever-se para uma assinatura de avaliação](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c33cf-132">You can [activate your MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), or [sign up for a trial subscription](https://azure.microsoft.com/pricing/free-trial/).</span></span>

### <a name="deploying-a-signalr-web-app-to-azure"></a><span data-ttu-id="c33cf-133">Implantando um aplicativo Web sinalizador no Azure</span><span class="sxs-lookup"><span data-stu-id="c33cf-133">Deploying a SignalR web app to Azure</span></span>

1. <span data-ttu-id="c33cf-134">Conclua o [tutorial de introdução](../getting-started/tutorial-getting-started-with-signalr.md)ou baixe o projeto concluído na Galeria de [códigos](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="c33cf-134">Complete the [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the finished project from [Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
2. <span data-ttu-id="c33cf-135">No Visual Studio, selecione **criar**, **publicar o chat do signalr**.</span><span class="sxs-lookup"><span data-stu-id="c33cf-135">In Visual Studio, select **Build**, **Publish SignalR Chat**.</span></span>
3. <span data-ttu-id="c33cf-136">Na caixa de diálogo "publicar Web", selecione "sites do Windows Azure".</span><span class="sxs-lookup"><span data-stu-id="c33cf-136">In the "Publish Web" dialog, select "Windows Azure Web Sites".</span></span>

    ![Selecionar sites do Azure](using-signalr-with-azure-web-sites/_static/image1.png)
4. <span data-ttu-id="c33cf-138">Se você não estiver conectado à sua conta Microsoft, clique em **entrar...** na caixa de diálogo "Selecionar site existente" e entre.</span><span class="sxs-lookup"><span data-stu-id="c33cf-138">If you aren't signed in to your Microsoft account, click **Sign In...** in the "Select Existing Web Site" dialog, and sign in.</span></span>

    ![Selecionar site existente](using-signalr-with-azure-web-sites/_static/image2.png)    ![Entrar no Azure](using-signalr-with-azure-web-sites/_static/image3.png)
5. <span data-ttu-id="c33cf-141">Na caixa de diálogo "Selecionar site da Web existente", clique em **novo**.</span><span class="sxs-lookup"><span data-stu-id="c33cf-141">In the "Select Existing Web Site" dialog, click **New**.</span></span>

    ![Novo Site](using-signalr-with-azure-web-sites/_static/image4.png)
6. <span data-ttu-id="c33cf-143">Na caixa de diálogo "criar site no Windows Azure", insira um nome de aplicativo exclusivo.</span><span class="sxs-lookup"><span data-stu-id="c33cf-143">In the "Create site on Windows Azure" dialog, enter a unique app name.</span></span> <span data-ttu-id="c33cf-144">Selecione a região mais próxima de você na lista suspensa região.</span><span class="sxs-lookup"><span data-stu-id="c33cf-144">Select the region closest to you in the Region dropdown.</span></span> <span data-ttu-id="c33cf-145">Clique em **Criar**.</span><span class="sxs-lookup"><span data-stu-id="c33cf-145">Click **Create**.</span></span>

    ![Criar um site no Azure](using-signalr-with-azure-web-sites/_static/image5.png)
7. <span data-ttu-id="c33cf-147">Na caixa de diálogo "publicar Web", clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="c33cf-147">In the "Publish Web" dialog, click **Publish**.</span></span>

    ![Publicar site](using-signalr-with-azure-web-sites/_static/image6.png)
8. <span data-ttu-id="c33cf-149">Quando o aplicativo concluir a publicação, o aplicativo de chat do Signalr hospedado em aplicativos Web do serviço Azure App será aberto em um navegador.</span><span class="sxs-lookup"><span data-stu-id="c33cf-149">When the app has completed publishing, the SignalR Chat application hosted in Azure App Service Web Apps will open in a browser.</span></span>

    ![Abertura de site em um navegador](using-signalr-with-azure-web-sites/_static/image7.png)

<a id="websocket"></a>
### <a name="enabling-websockets-on-azure-app-service-web-apps"></a><span data-ttu-id="c33cf-151">Habilitando WebSockets em aplicativos Web do Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c33cf-151">Enabling WebSockets on Azure App Service Web Apps</span></span>

<span data-ttu-id="c33cf-152">O WebSockets precisa estar explicitamente habilitado em seu aplicativo Web para ser usado em um aplicativo Signalr; caso contrário, outros protocolos serão usados (consulte [transportes e fallbacks](../getting-started/introduction-to-signalr.md#transports) para obter detalhes).</span><span class="sxs-lookup"><span data-stu-id="c33cf-152">WebSockets needs to be explicitly enabled in your web app to be used in a SignalR application; otherwise, other protocols will be used (See [Transports and Fallbacks](../getting-started/introduction-to-signalr.md#transports) for details).</span></span>

<span data-ttu-id="c33cf-153">Para usar Websockets em Azure App aplicativos Web do serviço, habilite-os na seção de configuração do aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="c33cf-153">In order to use WebSockets on Azure App Service Web Apps, enable it in the configuration section of the web app.</span></span> <span data-ttu-id="c33cf-154">Para fazer isso, abra seu aplicativo Web no [portal de gerenciamento do Azure](https://manage.windowsazure.com/)e selecione Configurar.</span><span class="sxs-lookup"><span data-stu-id="c33cf-154">To do this, open your web app in the [Azure Management Portal](https://manage.windowsazure.com/), and select Configure.</span></span>

![Guia Configurar](using-signalr-with-azure-web-sites/_static/image8.png)

<span data-ttu-id="c33cf-156">Na parte superior da página de configuração, verifique se o .NET 4,5 é usado para seu aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="c33cf-156">At the top of the configuration page, ensure that .NET 4.5 is used for your web app.</span></span>

![Configuração do .NET Framework versão 4,5](using-signalr-with-azure-web-sites/_static/image9.png)

<span data-ttu-id="c33cf-158">Na página configuração, na configuração **WebSockets** , selecione **ativado**.</span><span class="sxs-lookup"><span data-stu-id="c33cf-158">On the configuration page, in the **WebSockets** setting, select **On**.</span></span>

![Configuração de WebSockets: ativado](using-signalr-with-azure-web-sites/_static/image10.png)

<span data-ttu-id="c33cf-160">Na parte inferior da página de configuração, selecione **salvar** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="c33cf-160">At the bottom of the Configuration page, select **Save** to save your changes.</span></span>

![Salvar configurações](using-signalr-with-azure-web-sites/_static/image11.png)

<a id="backplane"></a>
## <a name="using-the-azure-redis-cache-backplane"></a><span data-ttu-id="c33cf-162">Usando o backplane do cache Redis do Azure</span><span class="sxs-lookup"><span data-stu-id="c33cf-162">Using the Azure Redis Cache Backplane</span></span>

<span data-ttu-id="c33cf-163">Se você usar várias instâncias para seu aplicativo Web, e os usuários dessas instâncias precisarem interagir umas com as outras (de modo que, por exemplo, as mensagens de bate-papo criadas em uma instância possam acessar os usuários conectados a outras instâncias), o [backplane do cache Redis do Azure](../performance/scaleout-with-redis.md) deve ser implementado em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c33cf-163">If you use multiple instances for your web app, and the users of those instances need to interact with one another (so that, for instance, chat messages created in one instance can reach the users connected to other instances), the [Azure Redis Cache backplane](../performance/scaleout-with-redis.md) must be implemented in your application.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="c33cf-164">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="c33cf-164">Next Steps</span></span>

<span data-ttu-id="c33cf-165">Para obter mais informações sobre aplicativos Web no serviço Azure App, consulte [visão geral de aplicativos Web](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span><span class="sxs-lookup"><span data-stu-id="c33cf-165">For more information on Web Apps in Azure App Service, see [Web Apps overview](https://azure.microsoft.com/documentation/articles/app-service-web-overview/).</span></span>
