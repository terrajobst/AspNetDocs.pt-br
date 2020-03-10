---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Usando a API Web com ASP.NET Web Forms-ASP.NET 4. x
author: MikeWasson
description: Tutorial com código passo a passo para adicionar a API Web a um aplicativo do ASP.NET Forms para o ASP.NET 4. x
ms.author: riande
ms.date: 04/03/2012
ms.custom: seoapril2019
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: ae553b62998fefd128e12711cbde958ea42d8c63
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556773"
---
# <a name="using-web-api-with-aspnet-web-forms"></a><span data-ttu-id="41725-103">Usar a API da Web com formulários da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="41725-103">Using Web API with ASP.NET Web Forms</span></span>

<span data-ttu-id="41725-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="41725-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="41725-105">Este tutorial orienta você pelas etapas para adicionar a API da Web a um aplicativo ASP.NET Web Forms tradicional no ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="41725-105">This tutorial walks you through the steps to add Web API to a traditional ASP.NET Web Forms application in ASP.NET 4.x.</span></span> 

## <a name="overview"></a><span data-ttu-id="41725-106">Visão geral</span><span class="sxs-lookup"><span data-stu-id="41725-106">Overview</span></span>

<span data-ttu-id="41725-107">Embora ASP.NET Web API seja empacotado com o ASP.NET MVC, é fácil adicionar a API Web a um aplicativo ASP.NET Web Forms tradicional.</span><span class="sxs-lookup"><span data-stu-id="41725-107">Although ASP.NET Web API is packaged with ASP.NET MVC, it is easy to add Web API to a traditional ASP.NET Web Forms application.</span></span>

<span data-ttu-id="41725-108">Para usar a API Web em um aplicativo Web Forms, há duas etapas principais:</span><span class="sxs-lookup"><span data-stu-id="41725-108">To use Web API in a Web Forms application, there are two main steps:</span></span>

- <span data-ttu-id="41725-109">Adicione um controlador de API da Web derivado da classe **ApiController** .</span><span class="sxs-lookup"><span data-stu-id="41725-109">Add a Web API controller that derives from the **ApiController** class.</span></span>
- <span data-ttu-id="41725-110">Adicione uma tabela de rotas ao método **Start do\_do aplicativo** .</span><span class="sxs-lookup"><span data-stu-id="41725-110">Add a route table to the **Application\_Start** method.</span></span>

## <a name="create-a-web-forms-project"></a><span data-ttu-id="41725-111">Criar um projeto Web Forms</span><span class="sxs-lookup"><span data-stu-id="41725-111">Create a Web Forms Project</span></span>

<span data-ttu-id="41725-112">Inicie o Visual Studio e selecione **novo projeto** na página **inicial** .</span><span class="sxs-lookup"><span data-stu-id="41725-112">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="41725-113">Ou, no menu **arquivo** , selecione **novo** e **projeto**.</span><span class="sxs-lookup"><span data-stu-id="41725-113">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="41725-114">No painel **modelos** , selecione **modelos instalados** e expanda o **nó C# Visual** .</span><span class="sxs-lookup"><span data-stu-id="41725-114">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="41725-115">Em **Visual C#** , selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="41725-115">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="41725-116">Na lista de modelos de projeto, selecione **ASP.NET Web Forms aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="41725-116">In the list of project templates, select **ASP.NET Web Forms Application**.</span></span> <span data-ttu-id="41725-117">Insira um nome para o projeto e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="41725-117">Enter a name for the project and click **OK**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="41725-118">Criar o modelo e o controlador</span><span class="sxs-lookup"><span data-stu-id="41725-118">Create the Model and Controller</span></span>

<span data-ttu-id="41725-119">Este tutorial usa as mesmas classes de modelo e controlador do tutorial de [introdução](tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="41725-119">This tutorial uses the same model and controller classes as the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="41725-120">Primeiro, adicione uma classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="41725-120">First, add a model class.</span></span> <span data-ttu-id="41725-121">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar classe**.</span><span class="sxs-lookup"><span data-stu-id="41725-121">In **Solution Explorer**, right-click the project and select **Add Class**.</span></span> <span data-ttu-id="41725-122">Nomeie o produto de classe e adicione a seguinte implementação:</span><span class="sxs-lookup"><span data-stu-id="41725-122">Name the class Product, and add the following implementation:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

<span data-ttu-id="41725-123">Em seguida, adicione um controlador de API Web ao projeto., um *controlador* é o objeto que MANIPULA solicitações HTTP para a API da Web.</span><span class="sxs-lookup"><span data-stu-id="41725-123">Next, add a Web API controller to the project., A *controller* is the object that handles HTTP requests for Web API.</span></span>

<span data-ttu-id="41725-124">No **Gerenciador de Soluções**, clique com o botão direito do mouse no nome do projeto.</span><span class="sxs-lookup"><span data-stu-id="41725-124">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="41725-125">Selecione **Adicionar novo item**.</span><span class="sxs-lookup"><span data-stu-id="41725-125">Select **Add New Item**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

<span data-ttu-id="41725-126">Em **modelos instalados**, expanda **Visual C#**  e selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="41725-126">Under **Installed Templates**, expand **Visual C#** and select **Web**.</span></span> <span data-ttu-id="41725-127">Em seguida, na lista de modelos, selecione **classe de controlador da API Web**.</span><span class="sxs-lookup"><span data-stu-id="41725-127">Then, from the list of templates, select **Web API Controller Class**.</span></span> <span data-ttu-id="41725-128">Nomeie o controlador como "ProductsController" e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="41725-128">Name the controller "ProductsController" and click **Add**.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

<span data-ttu-id="41725-129">O assistente **Adicionar novo item** criará um arquivo chamado ProductsController.cs.</span><span class="sxs-lookup"><span data-stu-id="41725-129">The **Add New Item** wizard will create a file named ProductsController.cs.</span></span> <span data-ttu-id="41725-130">Exclua os métodos incluídos pelo assistente e adicione os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="41725-130">Delete the methods that the wizard included and add the following methods:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

<span data-ttu-id="41725-131">Para obter mais informações sobre o código nesse controlador, consulte o tutorial de [introdução](tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="41725-131">For more information about the code in this controller, see the [Getting Started](tutorial-your-first-web-api.md) tutorial.</span></span>

## <a name="add-routing-information"></a><span data-ttu-id="41725-132">Adicionar informações de roteamento</span><span class="sxs-lookup"><span data-stu-id="41725-132">Add Routing Information</span></span>

<span data-ttu-id="41725-133">Em seguida, adicionaremos uma rota de URI para que os URIs do formulário &quot;/API/Products/&quot; sejam roteados para o controlador.</span><span class="sxs-lookup"><span data-stu-id="41725-133">Next, we'll add a URI route so that URIs of the form &quot;/api/products/&quot; are routed to the controller.</span></span>

<span data-ttu-id="41725-134">Em **Gerenciador de soluções**, clique duas vezes em global. asax para abrir o arquivo code-behind global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="41725-134">In **Solution Explorer**, double-click Global.asax to open the code-behind file Global.asax.cs.</span></span> <span data-ttu-id="41725-135">Adicione a seguinte instrução **using** .</span><span class="sxs-lookup"><span data-stu-id="41725-135">Add the following **using** statement.</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

<span data-ttu-id="41725-136">Em seguida, adicione o seguinte código ao **aplicativo\_** método de início:</span><span class="sxs-lookup"><span data-stu-id="41725-136">Then add the following code to the **Application\_Start** method:</span></span>

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

<span data-ttu-id="41725-137">Para obter mais informações sobre tabelas de roteamento, consulte [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="41725-137">For more information about routing tables, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="add-client-side-ajax"></a><span data-ttu-id="41725-138">Adicionar AJAX do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="41725-138">Add Client-Side AJAX</span></span>

<span data-ttu-id="41725-139">Isso é tudo o que você precisa para criar uma API Web que os clientes podem acessar.</span><span class="sxs-lookup"><span data-stu-id="41725-139">That's all you need to create a web API that clients can access.</span></span> <span data-ttu-id="41725-140">Agora, vamos adicionar uma página HTML que usa jQuery para chamar a API.</span><span class="sxs-lookup"><span data-stu-id="41725-140">Now let's add an HTML page that uses jQuery to call the API.</span></span>

<span data-ttu-id="41725-141">Verifique se a página mestra (por exemplo, *site. Master*) inclui um `ContentPlaceHolder` com `ID="HeadContent"`:</span><span class="sxs-lookup"><span data-stu-id="41725-141">Make sure your master page (for example, *Site.Master*) includes a `ContentPlaceHolder` with `ID="HeadContent"`:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

<span data-ttu-id="41725-142">Abra o arquivo default. aspx.</span><span class="sxs-lookup"><span data-stu-id="41725-142">Open the file Default.aspx.</span></span> <span data-ttu-id="41725-143">Substitua o texto clichê que está na seção de conteúdo principal, conforme mostrado:</span><span class="sxs-lookup"><span data-stu-id="41725-143">Replace the boilerplate text that is in the main content section, as shown:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

<span data-ttu-id="41725-144">Em seguida, adicione uma referência ao arquivo de origem jQuery na seção `HeaderContent`:</span><span class="sxs-lookup"><span data-stu-id="41725-144">Next, add a reference to the jQuery source file in the `HeaderContent` section:</span></span>

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

<span data-ttu-id="41725-145">Observação: você pode adicionar facilmente a referência de script arrastando e soltando o arquivo de **Gerenciador de soluções** para a janela Editor de código.</span><span class="sxs-lookup"><span data-stu-id="41725-145">Note: You can easily add the script reference by dragging and dropping the file from **Solution Explorer** into the code editor window.</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

<span data-ttu-id="41725-146">Abaixo da marca de script jQuery, adicione o seguinte bloco de script:</span><span class="sxs-lookup"><span data-stu-id="41725-146">Below the jQuery script tag, add the following script block:</span></span>

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

<span data-ttu-id="41725-147">Quando o documento é carregado, esse script faz uma solicitação AJAX para &quot;&quot;de API/produtos.</span><span class="sxs-lookup"><span data-stu-id="41725-147">When the document loads, this script makes an AJAX request to &quot;api/products&quot;.</span></span> <span data-ttu-id="41725-148">A solicitação retorna uma lista de produtos no formato JSON.</span><span class="sxs-lookup"><span data-stu-id="41725-148">The request returns a list of products in JSON format.</span></span> <span data-ttu-id="41725-149">O script adiciona as informações do produto à tabela HTML.</span><span class="sxs-lookup"><span data-stu-id="41725-149">The script adds the product information to the HTML table.</span></span>

<span data-ttu-id="41725-150">Quando você executa o aplicativo, ele deve ter a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="41725-150">When you run the application, it should look like this:</span></span>

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
