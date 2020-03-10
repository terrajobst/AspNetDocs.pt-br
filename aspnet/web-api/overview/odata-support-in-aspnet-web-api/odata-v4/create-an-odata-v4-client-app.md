---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Criar um aplicativo cliente do OData v4C#() | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: a0016cf2cc7bffe6268664395ccb38e140090310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556290"
---
# <a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="78eb0-102">Criar um aplicativo cliente do OData v4 (C#)</span><span class="sxs-lookup"><span data-stu-id="78eb0-102">Create an OData v4 Client App (C#)</span></span>

<span data-ttu-id="78eb0-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="78eb0-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="78eb0-104">No tutorial anterior, você criou um serviço OData básico que oferece suporte a operações CRUD.</span><span class="sxs-lookup"><span data-stu-id="78eb0-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="78eb0-105">Agora, vamos criar um cliente para o serviço.</span><span class="sxs-lookup"><span data-stu-id="78eb0-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="78eb0-106">Inicie uma nova instância do Visual Studio e crie um novo projeto de aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="78eb0-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="78eb0-107">Na caixa de diálogo **novo projeto** , selecione **modelos** de &gt; **instalados** &gt;  **C# Visual** &gt; **área de trabalho do Windows**e selecione o modelo aplicativo de **console** .</span><span class="sxs-lookup"><span data-stu-id="78eb0-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="78eb0-108">Nomeie o projeto &quot;ProductsApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="78eb0-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="78eb0-109">Você também pode adicionar o aplicativo de console à mesma solução do Visual Studio que contém o serviço OData.</span><span class="sxs-lookup"><span data-stu-id="78eb0-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>

## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="78eb0-110">Instalar o gerador de código do cliente OData</span><span class="sxs-lookup"><span data-stu-id="78eb0-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="78eb0-111">No menu **Ferramentas**, selecione **Extensões e atualizações**.</span><span class="sxs-lookup"><span data-stu-id="78eb0-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="78eb0-112">Selecione **Online** &gt; **Galeria do Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="78eb0-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="78eb0-113">Na caixa de pesquisa, pesquise &quot;gerador de código do cliente OData&quot;.</span><span class="sxs-lookup"><span data-stu-id="78eb0-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="78eb0-114">Clique em **baixar** para instalar o VSIX.</span><span class="sxs-lookup"><span data-stu-id="78eb0-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="78eb0-115">Talvez seja solicitado que você reinicie o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="78eb0-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="78eb0-116">Executar o serviço OData localmente</span><span class="sxs-lookup"><span data-stu-id="78eb0-116">Run the OData Service Locally</span></span>

<span data-ttu-id="78eb0-117">Execute o projeto ProductService do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="78eb0-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="78eb0-118">Por padrão, o Visual Studio inicia um navegador na raiz do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="78eb0-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="78eb0-119">Observe o URI; Isso será necessário na próxima etapa.</span><span class="sxs-lookup"><span data-stu-id="78eb0-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="78eb0-120">Deixe o aplicativo em execução.</span><span class="sxs-lookup"><span data-stu-id="78eb0-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="78eb0-121">Se você colocar os dois projetos na mesma solução, certifique-se de executar o projeto ProductService sem depuração.</span><span class="sxs-lookup"><span data-stu-id="78eb0-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="78eb0-122">Na próxima etapa, você precisará manter o serviço em execução enquanto modifica o projeto de aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="78eb0-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>

## <a name="generate-the-service-proxy"></a><span data-ttu-id="78eb0-123">Gerar o proxy de serviço</span><span class="sxs-lookup"><span data-stu-id="78eb0-123">Generate the Service Proxy</span></span>

<span data-ttu-id="78eb0-124">O proxy de serviço é uma classe .NET que define métodos para acessar o serviço OData.</span><span class="sxs-lookup"><span data-stu-id="78eb0-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="78eb0-125">O proxy traduz as chamadas de método em solicitações HTTP.</span><span class="sxs-lookup"><span data-stu-id="78eb0-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="78eb0-126">Você criará a classe proxy executando um [modelo T4](https://msdn.microsoft.com/library/bb126445.aspx).</span><span class="sxs-lookup"><span data-stu-id="78eb0-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/library/bb126445.aspx).</span></span>

<span data-ttu-id="78eb0-127">Clique com o botão direito do mouse no projeto.</span><span class="sxs-lookup"><span data-stu-id="78eb0-127">Right-click the project.</span></span> <span data-ttu-id="78eb0-128">Selecione **adicionar** &gt; **novo item**.</span><span class="sxs-lookup"><span data-stu-id="78eb0-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="78eb0-129">Na caixa de diálogo **Adicionar novo item** , **selecione C# itens visuais** &gt; **código** &gt; **cliente OData**.</span><span class="sxs-lookup"><span data-stu-id="78eb0-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="78eb0-130">Nomeie o modelo &quot;ProductClient.tt&quot;.</span><span class="sxs-lookup"><span data-stu-id="78eb0-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="78eb0-131">Clique em **Adicionar** e clique no aviso de segurança.</span><span class="sxs-lookup"><span data-stu-id="78eb0-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="78eb0-132">Neste ponto, você receberá um erro, que pode ser ignorado.</span><span class="sxs-lookup"><span data-stu-id="78eb0-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="78eb0-133">O Visual Studio executa automaticamente o modelo, mas o modelo precisa de algumas definições de configuração primeiro.</span><span class="sxs-lookup"><span data-stu-id="78eb0-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="78eb0-134">Abra o arquivo ProductClient. OData. config. No elemento `Parameter`, Cole o URI do projeto ProductService (etapa anterior).</span><span class="sxs-lookup"><span data-stu-id="78eb0-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="78eb0-135">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="78eb0-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="78eb0-136">Execute o modelo novamente.</span><span class="sxs-lookup"><span data-stu-id="78eb0-136">Run the template again.</span></span> <span data-ttu-id="78eb0-137">Em Gerenciador de Soluções, clique com o botão direito do mouse no arquivo ProductClient.tt e selecione **executar ferramenta personalizada**.</span><span class="sxs-lookup"><span data-stu-id="78eb0-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="78eb0-138">O modelo cria um arquivo de código chamado ProductClient.cs que define o proxy.</span><span class="sxs-lookup"><span data-stu-id="78eb0-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="78eb0-139">Ao desenvolver seu aplicativo, se você alterar o ponto de extremidade OData, execute o modelo novamente para atualizar o proxy.</span><span class="sxs-lookup"><span data-stu-id="78eb0-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="78eb0-140">Usar o proxy de serviço para chamar o serviço OData</span><span class="sxs-lookup"><span data-stu-id="78eb0-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="78eb0-141">Abra o arquivo Program.cs e substitua o código clichê pelo seguinte.</span><span class="sxs-lookup"><span data-stu-id="78eb0-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="78eb0-142">Substitua o valor de *ServiceUri* pelo URI de serviço anterior.</span><span class="sxs-lookup"><span data-stu-id="78eb0-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="78eb0-143">Quando você executa o aplicativo, ele deve gerar o seguinte:</span><span class="sxs-lookup"><span data-stu-id="78eb0-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
