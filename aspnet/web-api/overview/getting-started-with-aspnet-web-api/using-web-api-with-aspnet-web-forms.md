---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Usando a API da Web com Web Forms do ASP.NET - ASP.NET 4.x
author: MikeWasson
description: Tutorial com o código passo a passo para adicionar a API da Web a um aplicativo de formulários do ASP.NET para ASP.NET 4. x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59422571"
---
# <a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="611d1-103">Uso da API Web com Web Forms do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="611d1-103">Using Web API with ASP.NET Web Forms</span></span>

<span data-ttu-id="611d1-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="611d1-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="611d1-105">Este tutorial orienta você pelas etapas para adicionar a API da Web a um aplicativo Web Forms do ASP.NET tradicional no ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="611d1-105">This tutorial walks you through the steps to add Web API to a traditional ASP.NET Web Forms application in ASP.NET 4.x.</span></span> 

## <a name="overview"></a><span data-ttu-id="611d1-106">Visão geral</span><span class="sxs-lookup"><span data-stu-id="611d1-106">Overview</span></span>

<span data-ttu-id="611d1-107">Embora a API Web ASP.NET é empacotada com o ASP.NET MVC, é fácil adicionar a API da Web a um aplicativo Web Forms do ASP.NET tradicional.</span><span class="sxs-lookup"><span data-stu-id="611d1-107">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span>

<span data-ttu-id="611d1-108">Para usar a API da Web em um aplicativo Web Forms, há duas etapas principais:</span><span class="sxs-lookup"><span data-stu-id="611d1-108">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="611d1-109">Adicionar um controlador de API da Web que deriva de **ApiController** classe.</span><span class="sxs-lookup"><span data-stu-id="611d1-109">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="611d1-110">Adicionar uma tabela de rotas para o **Application\_iniciar** método.</span><span class="sxs-lookup"><span data-stu-id="611d1-110">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="611d1-111">Criar um projeto de formulários da Web</span><span class="sxs-lookup"><span data-stu-id="611d1-111">Create a Web Forms Project</span></span>

<span data-ttu-id="611d1-112">Inicie o Visual Studio e selecione **Novo projeto** na página **Iniciar**.</span><span class="sxs-lookup"><span data-stu-id="611d1-112">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="611d1-113">Ou, no menu **Arquivo**, selecione **Novo** e, em seguida, **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="611d1-113">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="611d1-114">No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="611d1-114">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="611d1-115">Em **Visual C#**, selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="611d1-115">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="611d1-116">Na lista de modelos de projeto, selecione **aplicativo do ASP.NET Web Forms**.</span><span class="sxs-lookup"><span data-stu-id="611d1-116">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="611d1-117">Insira um nome para o projeto e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="611d1-117">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="611d1-118">Criar o modelo e o controlador</span><span class="sxs-lookup"><span data-stu-id="611d1-118">Create the Model and Controller</span></span>

<span data-ttu-id="611d1-119">Este tutorial usa as mesmas classes de modelo e controlador como o [guia de Introdução](tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="611d1-119">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="611d1-120">Primeiro, adicione uma classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="611d1-120">First, add a model class.</span></span> <span data-ttu-id="611d1-121">Na **Gerenciador de soluções**, clique com botão direito no projeto e selecione **Adicionar classe**.</span><span class="sxs-lookup"><span data-stu-id="611d1-121">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="611d1-122">Nomeie a classe Product e, em seguida, adicione a seguinte implementação:</span><span class="sxs-lookup"><span data-stu-id="611d1-122">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="611d1-123">Em seguida, adicione um controlador de API da Web ao projeto., uma *controlador* é o objeto que manipula as solicitações HTTP para a API da Web.</span><span class="sxs-lookup"><span data-stu-id="611d1-123">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="611d1-124">No **Gerenciador de Soluções**, clique com o botão direito do mouse no nome do projeto.</span><span class="sxs-lookup"><span data-stu-id="611d1-124">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="611d1-125">Selecione **Adicionar Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="611d1-125">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="611d1-126">Sob **modelos instalados**, expanda **Visual c#** e selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="611d1-126">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="611d1-127">Em seguida, na lista de modelos, selecione **Web API Controller Class**.</span><span class="sxs-lookup"><span data-stu-id="611d1-127">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="611d1-128">Nomeie o controlador "ProductsController" e clique em **adicionar**.</span><span class="sxs-lookup"><span data-stu-id="611d1-128">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="611d1-129">O **Adicionar Novo Item** assistente criará um arquivo chamado ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="611d1-129">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="611d1-130">Exclua os métodos que o assistente incluído e adicione os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="611d1-130">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="611d1-131">Para obter mais informações sobre o código neste controlador, consulte o [guia de Introdução](tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="611d1-131">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="611d1-132">Adicionar informações de roteamento</span><span class="sxs-lookup"><span data-stu-id="611d1-132">Add Routing Information</span></span>

<span data-ttu-id="611d1-133">Em seguida, vamos adicionar uma rota URI assim que URIs do formulário &quot;/API/produtos/&quot; são roteadas para o controlador.</span><span class="sxs-lookup"><span data-stu-id="611d1-133">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="611d1-134">Na **Gerenciador de soluções**, clique duas vezes em global. asax para abrir o arquivo code-behind Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="611d1-134">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="611d1-135">Adicione o seguinte **usando** instrução.</span><span class="sxs-lookup"><span data-stu-id="611d1-135">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="611d1-136">Em seguida, adicione o seguinte código para o **Application\_iniciar** método:</span><span class="sxs-lookup"><span data-stu-id="611d1-136">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="611d1-137">Para obter mais informações sobre tabelas de roteamento, consulte [roteamento na API Web ASP.NET](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="611d1-137">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="611d1-138">Adicionar o AJAX do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="611d1-138">Add Client-Side AJAX</span></span>

<span data-ttu-id="611d1-139">Isso é tudo o que você precisa criar uma API web que os clientes podem acessar.</span><span class="sxs-lookup"><span data-stu-id="611d1-139">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="611d1-140">Agora vamos adicionar uma página HTML que usa o jQuery para chamar a API.</span><span class="sxs-lookup"><span data-stu-id="611d1-140">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="611d1-141">Verifique se a página mestra (por exemplo, *Master*) inclui um `ContentPlaceHolder` com `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="611d1-141">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="611d1-142">Abra o arquivo default. aspx.</span><span class="sxs-lookup"><span data-stu-id="611d1-142">Open the file Default.aspx.</span></span> <span data-ttu-id="611d1-143">Substitua o texto clichê que está na seção de conteúdo principal, conforme mostrado:</span><span class="sxs-lookup"><span data-stu-id="611d1-143">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="611d1-144">Em seguida, adicione uma referência para o arquivo de origem do jQuery no `HeaderContent` seção:</span><span class="sxs-lookup"><span data-stu-id="611d1-144">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="611d1-145">Observação: Você pode adicionar facilmente a referência de script arrastando e soltando o arquivo a partir **Gerenciador de soluções** na janela do editor de código.</span><span class="sxs-lookup"><span data-stu-id="611d1-145">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="611d1-146">Abaixo da marca de script do jQuery, adicione o seguinte bloco de script:</span><span class="sxs-lookup"><span data-stu-id="611d1-146">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="611d1-147">Quando o documento for carregado, esse script faz uma solicitação AJAX ao &quot;api/produtos&quot;.</span><span class="sxs-lookup"><span data-stu-id="611d1-147">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="611d1-148">A solicitação retorna uma lista de produtos no formato JSON.</span><span class="sxs-lookup"><span data-stu-id="611d1-148">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="611d1-149">O script adiciona as informações do produto na tabela de HTML.</span><span class="sxs-lookup"><span data-stu-id="611d1-149">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="611d1-150">Quando você executa o aplicativo, ele deve ser assim:</span><span class="sxs-lookup"><span data-stu-id="611d1-150">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
