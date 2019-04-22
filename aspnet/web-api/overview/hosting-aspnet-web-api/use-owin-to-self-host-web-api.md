---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Usar o OWIN para auto-hospedar a API Web ASP.NET - ASP.NET 4.x
author: rick-anderson
description: Tutorial com código que mostra como hospedar a API Web ASP.NET em um aplicativo de console.
ms.author: riande
ms.date: 07/09/2013
ms.custom: seoapril2019
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: a67db0bd061846af2db3599e0843ed7c6a22db1e
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59386509"
---
# <a name="use-owin-to-self-host-aspnet-web-api"></a><span data-ttu-id="27e34-103">Usar o OWIN para auto-hospedar a API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="27e34-103">Use OWIN to Self-Host ASP.NET Web API</span></span> 


> <span data-ttu-id="27e34-104">Este tutorial mostra como hospedar a API Web ASP.NET em um aplicativo de console, usando o OWIN para auto-hospedar a estrutura da API Web.</span><span class="sxs-lookup"><span data-stu-id="27e34-104">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
>
> <span data-ttu-id="27e34-105">[Open Web Interface para .NET](http://owin.org) (OWIN) define uma abstração entre servidores de web do .NET e aplicativos da web.</span><span class="sxs-lookup"><span data-stu-id="27e34-105">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="27e34-106">OWIN separa o aplicativo web do servidor, o que torna o OWIN ideal para auto-hospedagem em um aplicativo web em seu próprio processo, fora do IIS.</span><span class="sxs-lookup"><span data-stu-id="27e34-106">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="27e34-107">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="27e34-107">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="27e34-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="27e34-108">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/) 
> - <span data-ttu-id="27e34-109">Web API 5.2.7</span><span class="sxs-lookup"><span data-stu-id="27e34-109">Web API 5.2.7</span></span>


> [!NOTE]
> <span data-ttu-id="27e34-110">Você pode encontrar o código-fonte completo para este tutorial em [github.com/aspnet/samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).</span><span class="sxs-lookup"><span data-stu-id="27e34-110">You can find the complete source code for this tutorial at [github.com/aspnet/samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="27e34-111">Criar um aplicativo de console</span><span class="sxs-lookup"><span data-stu-id="27e34-111">Create a console application</span></span>

<span data-ttu-id="27e34-112">Sobre o **arquivo** menu, **New**, em seguida, selecione **projeto**.</span><span class="sxs-lookup"><span data-stu-id="27e34-112">On the **File** menu,  **New**, then select **Project**.</span></span> <span data-ttu-id="27e34-113">Partir **instalados**, em **Visual C#** , selecione **área de trabalho do Windows** e, em seguida, selecione **aplicativo de Console (.Net Framework)**.</span><span class="sxs-lookup"><span data-stu-id="27e34-113">From **Installed**, under **Visual C#**, select **Windows Desktop** and then select **Console App (.Net Framework)**.</span></span> <span data-ttu-id="27e34-114">Nomeie o projeto "OwinSelfhostSample" e selecione **Okey**.</span><span class="sxs-lookup"><span data-stu-id="27e34-114">Name the project "OwinSelfhostSample" and select **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="27e34-115">Adicione os pacotes de API da Web e OWIN</span><span class="sxs-lookup"><span data-stu-id="27e34-115">Add the Web API and OWIN packages</span></span>

<span data-ttu-id="27e34-116">Dos **ferramentas** menu, selecione **Gerenciador de pacotes NuGet**, em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="27e34-116">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="27e34-117">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="27e34-117">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="27e34-118">Isso instalará o pacote de selfhost WebAPI OWIN e todos os pacotes necessários do OWIN.</span><span class="sxs-lookup"><span data-stu-id="27e34-118">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="27e34-119">Configurar API da Web para hospedar internamente</span><span class="sxs-lookup"><span data-stu-id="27e34-119">Configure Web API for self-host</span></span>

<span data-ttu-id="27e34-120">No Gerenciador de soluções, clique com botão direito no projeto e selecione **Add** / **classe** para adicionar uma nova classe.</span><span class="sxs-lookup"><span data-stu-id="27e34-120">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="27e34-121">Nomeie a classe `Startup`.</span><span class="sxs-lookup"><span data-stu-id="27e34-121">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="27e34-122">Substitua todo o código clichê nesse arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="27e34-122">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="27e34-123">Adicionar um controlador de API da Web</span><span class="sxs-lookup"><span data-stu-id="27e34-123">Add a Web API controller</span></span>

<span data-ttu-id="27e34-124">Em seguida, adicione uma classe de controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="27e34-124">Next, add a Web API controller class.</span></span> <span data-ttu-id="27e34-125">No Gerenciador de soluções, clique com botão direito no projeto e selecione **Add** / **classe** para adicionar uma nova classe.</span><span class="sxs-lookup"><span data-stu-id="27e34-125">In Solution Explorer, right-click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="27e34-126">Nomeie a classe `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="27e34-126">Name the class `ValuesController`.</span></span>

<span data-ttu-id="27e34-127">Substitua todo o código clichê nesse arquivo com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="27e34-127">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a><span data-ttu-id="27e34-128">Iniciar o Host do OWIN e fazer uma solicitação por HttpClient</span><span class="sxs-lookup"><span data-stu-id="27e34-128">Start the OWIN Host and make a request with HttpClient</span></span>

<span data-ttu-id="27e34-129">Substitua todo o código clichê no arquivo Program.cs pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="27e34-129">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a><span data-ttu-id="27e34-130">Executar o aplicativo</span><span class="sxs-lookup"><span data-stu-id="27e34-130">Run the application</span></span>

<span data-ttu-id="27e34-131">Para executar o aplicativo, pressione F5 no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="27e34-131">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="27e34-132">A saída deve se parecer com o seguinte:</span><span class="sxs-lookup"><span data-stu-id="27e34-132">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="27e34-133">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="27e34-133">Additional resources</span></span>

[<span data-ttu-id="27e34-134">Uma visão geral do projeto Katana</span><span class="sxs-lookup"><span data-stu-id="27e34-134">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="27e34-135">Hospedar a API da Web ASP.NET em uma função de trabalho do Azure</span><span class="sxs-lookup"><span data-stu-id="27e34-135">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
