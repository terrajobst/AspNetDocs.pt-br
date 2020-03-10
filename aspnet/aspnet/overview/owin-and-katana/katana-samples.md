---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Exemplos de Katana | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 1238f7d09492a6856d49dece5de75184ccfa4838
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584556"
---
# <a name="katana-samples"></a><span data-ttu-id="e076c-102">Exemplos do Katana</span><span class="sxs-lookup"><span data-stu-id="e076c-102">Katana Samples</span></span>

<span data-ttu-id="e076c-103">pela [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="e076c-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="e076c-104">Exemplos do Katana</span><span class="sxs-lookup"><span data-stu-id="e076c-104">Katana Samples</span></span>

<span data-ttu-id="e076c-105">**Exemplo de rotas ASP.NET** | [código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span><span class="sxs-lookup"><span data-stu-id="e076c-105">**ASP.NET Routes Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes)</span></span>  
<span data-ttu-id="e076c-106">Em alguns aplicativos, você desejará conectar componentes do OWIN na tabela de rotas do Asp.Net lado a lado com componentes não OWIN.</span><span class="sxs-lookup"><span data-stu-id="e076c-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="e076c-107">Este exemplo mostra como usar os métodos de extensão RouteCollection MapOwinPath e MapOwinRoute fornecidos pelo Microsoft. Owin. host. SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="e076c-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="e076c-108">**Exemplo de pipelines de ramificação** | [código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span><span class="sxs-lookup"><span data-stu-id="e076c-108">**Branching Pipelines Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)</span></span>  
<span data-ttu-id="e076c-109">Os pipelines de processamento de solicitação OWIN não precisam ser lineares, eles podem ser ramificados para processar solicitações de maneiras diferentes.</span><span class="sxs-lookup"><span data-stu-id="e076c-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="e076c-110">Este exemplo mostra como construir um pipeline de ramificação baseado em caminhos de solicitação ou outros dados de solicitação, como cabeçalhos.</span><span class="sxs-lookup"><span data-stu-id="e076c-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="e076c-111">Esses componentes estão disponíveis no pacote NuGet Microsoft. Owin. Mapping.</span><span class="sxs-lookup"><span data-stu-id="e076c-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="e076c-112">**Exemplo de servidor personalizado** | [código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span><span class="sxs-lookup"><span data-stu-id="e076c-112">**Custom Server Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer) </span></span>  
<span data-ttu-id="e076c-113">Mostra como usar um servidor OWIN personalizado ao hospedar internamente o OWIN.</span><span class="sxs-lookup"><span data-stu-id="e076c-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="e076c-114">[Código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded) de | de **exemplo inserido**</span><span class="sxs-lookup"><span data-stu-id="e076c-114">**Embedded Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded)</span></span>  
<span data-ttu-id="e076c-115">Alguns servidores OWIN podem ser executados dentro de seu próprio processo (&quot;&quot;auto-hospedado).</span><span class="sxs-lookup"><span data-stu-id="e076c-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="e076c-116">Este exemplo mostra como iniciar um aplicativo OWIN usando as ferramentas fornecidas pelo pacote NuGet Microsoft. Owin. Hosting.</span><span class="sxs-lookup"><span data-stu-id="e076c-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="e076c-117">[Código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld) do | de **exemplo HelloWorld**</span><span class="sxs-lookup"><span data-stu-id="e076c-117">**HelloWorld Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)</span></span>  
<span data-ttu-id="e076c-118">OWIN é uma abstração de API de servidor HTTP que habilita a portabilidade do aplicativo entre vários servidores.</span><span class="sxs-lookup"><span data-stu-id="e076c-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="e076c-119">Este exemplo demonstra como escrever um aplicativo Olá, Mundo usando alguns **wrappers simples** em relação à abstração OWIN bruta e executá-lo em um servidor web como ASP.net.</span><span class="sxs-lookup"><span data-stu-id="e076c-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="e076c-120">**Olá, Mundo | de exemplo OWIN bruto** de [código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span><span class="sxs-lookup"><span data-stu-id="e076c-120">**Hello World Raw OWIN Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin)</span></span>  
<span data-ttu-id="e076c-121">Este exemplo demonstra como gravar um aplicativo Olá, Mundo usando a abstração OWIN **bruta** e executá-lo em um servidor web como ASP.net.</span><span class="sxs-lookup"><span data-stu-id="e076c-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="e076c-122">[Código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR) do | de **exemplo do signalr**</span><span class="sxs-lookup"><span data-stu-id="e076c-122">**SignalR Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR)</span></span>  
<span data-ttu-id="e076c-123">Mostra como usar o OWIN/Katana por autoatendimento.</span><span class="sxs-lookup"><span data-stu-id="e076c-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="e076c-124">Para obter mais informações sobre o Signalr de hospedagem interna, consulte [tutorial: auto-host do signalr](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="e076c-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="e076c-125">**Exemplos de arquivos estáticos** | [código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span><span class="sxs-lookup"><span data-stu-id="e076c-125">**Static Files Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample) </span></span>  
<span data-ttu-id="e076c-126">Mostra como dar suporte a solicitações HTTP para arquivos estáticos usando OWIN/katana.</span><span class="sxs-lookup"><span data-stu-id="e076c-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="e076c-127"> | de [código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) **da API Web** </span><span class="sxs-lookup"><span data-stu-id="e076c-127">**Web API** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) </span></span>  
<span data-ttu-id="e076c-128">Este exemplo mostra como hospedar OWIN no IIS e adicionar API Web ao pipeline do OWIN.</span><span class="sxs-lookup"><span data-stu-id="e076c-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="e076c-129">**Exemplo de soquete da Web** | [código-fonte](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span><span class="sxs-lookup"><span data-stu-id="e076c-129">**Web Socket Sample** | [Source Code](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample) </span></span>  
<span data-ttu-id="e076c-130">Mostra como dar suporte a soquetes da Web no OWIN usando a classe [System .net. WebSockets. WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="e076c-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
