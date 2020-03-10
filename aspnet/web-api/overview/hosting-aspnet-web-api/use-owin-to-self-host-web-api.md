---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Usar OWIN para auto-host ASP.NET Web API-ASP.NET 4. x
author: rick-anderson
description: Tutorial com código mostrando como hospedar ASP.NET Web API em um aplicativo de console.
ms.author: riande
ms.date: 07/09/2013
ms.custom: seoapril2019
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 872b931391a63ef82b96e5b264c070c0b5e9605d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556535"
---
# <a name="use-owin-to-self-host-aspnet-web-api"></a><span data-ttu-id="55a5a-103">Usar o OWIN para hospedar automaticamente ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="55a5a-103">Use OWIN to Self-Host ASP.NET Web API</span></span> 

> <span data-ttu-id="55a5a-104">Este tutorial mostra como hospedar ASP.NET Web API em um aplicativo de console, usando OWIN para hospedar internamente a estrutura da API Web.</span><span class="sxs-lookup"><span data-stu-id="55a5a-104">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="55a5a-105">O [Open Web interface for .net](http://owin.org) (OWIN) define uma abstração entre servidores Web e aplicativos Web do .net.</span><span class="sxs-lookup"><span data-stu-id="55a5a-105">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="55a5a-106">OWIN dissocia o aplicativo Web do servidor, o que torna o OWIN ideal para hospedar internamente um aplicativo Web em seu próprio processo, fora do IIS.</span><span class="sxs-lookup"><span data-stu-id="55a5a-106">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="55a5a-107">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="55a5a-107">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="55a5a-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="55a5a-108">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/) 
> - <span data-ttu-id="55a5a-109">5\.2.7 da API Web</span><span class="sxs-lookup"><span data-stu-id="55a5a-109">Web API 5.2.7</span></span>

> [!NOTE]
> <span data-ttu-id="55a5a-110">Você pode encontrar o código-fonte completo deste tutorial em [github.com/ASPNET/Samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).</span><span class="sxs-lookup"><span data-stu-id="55a5a-110">You can find the complete source code for this tutorial at [github.com/aspnet/samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="55a5a-111">Criar um aplicativo de console</span><span class="sxs-lookup"><span data-stu-id="55a5a-111">Create a console application</span></span>

<span data-ttu-id="55a5a-112">No menu **arquivo** , **novo**, selecione **projeto**.</span><span class="sxs-lookup"><span data-stu-id="55a5a-112">On the **File** menu,  **New**, then select **Project**.</span></span> <span data-ttu-id="55a5a-113">Do **instalado**, em **Visual C#** , selecione **área de trabalho do Windows** e, em seguida, selecione **aplicativo de console (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="55a5a-113">From **Installed**, under **Visual C#**, select **Windows Desktop** and then select **Console App (.Net Framework)**.</span></span> <span data-ttu-id="55a5a-114">Nomeie o projeto "OwinSelfhostSample" e selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="55a5a-114">Name the project "OwinSelfhostSample" and select **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="55a5a-115">Adicionar a API Web e os pacotes OWIN</span><span class="sxs-lookup"><span data-stu-id="55a5a-115">Add the Web API and OWIN packages</span></span>

<span data-ttu-id="55a5a-116">No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="55a5a-116">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="55a5a-117">Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="55a5a-117">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="55a5a-118">Isso instalará o pacote WebAPI OWIN selfhost e todos os pacotes OWIN necessários.</span><span class="sxs-lookup"><span data-stu-id="55a5a-118">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="55a5a-119">Configurar API Web para hospedagem interna</span><span class="sxs-lookup"><span data-stu-id="55a5a-119">Configure Web API for self-host</span></span>

<span data-ttu-id="55a5a-120">Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto e selecione **adicionar** / **classe** para adicionar uma nova classe.</span><span class="sxs-lookup"><span data-stu-id="55a5a-120">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="55a5a-121">Nome da classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="55a5a-121">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="55a5a-122">Substitua todo o código clichê deste arquivo pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="55a5a-122">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="55a5a-123">Adicionar um controlador de API Web</span><span class="sxs-lookup"><span data-stu-id="55a5a-123">Add a Web API controller</span></span>

<span data-ttu-id="55a5a-124">Em seguida, adicione uma classe de controlador da API Web.</span><span class="sxs-lookup"><span data-stu-id="55a5a-124">Next, add a Web API controller class.</span></span> <span data-ttu-id="55a5a-125">Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto e selecione **adicionar** / **classe** para adicionar uma nova classe.</span><span class="sxs-lookup"><span data-stu-id="55a5a-125">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="55a5a-126">Nome da classe `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="55a5a-126">Name the class `ValuesController`.</span></span>

<span data-ttu-id="55a5a-127">Substitua todo o código clichê deste arquivo pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="55a5a-127">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a><span data-ttu-id="55a5a-128">Iniciar o host OWIN e fazer uma solicitação com HttpClient</span><span class="sxs-lookup"><span data-stu-id="55a5a-128">Start the OWIN Host and make a request with HttpClient</span></span>

<span data-ttu-id="55a5a-129">Substitua todo o código clichê no arquivo Program.cs pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="55a5a-129">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a><span data-ttu-id="55a5a-130">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="55a5a-130">Run the application</span></span>

<span data-ttu-id="55a5a-131">Para executar o aplicativo, pressione F5 no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55a5a-131">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="55a5a-132">A saída deve ser semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="55a5a-132">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="55a5a-133">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="55a5a-133">Additional resources</span></span>

[<span data-ttu-id="55a5a-134">Uma visão geral do projeto Katana</span><span class="sxs-lookup"><span data-stu-id="55a5a-134">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="55a5a-135">ASP.NET Web API de host em uma função de trabalho do Azure</span><span class="sxs-lookup"><span data-stu-id="55a5a-135">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
