---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Usando a API Web 2 com Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Este tutorial ensinará as noções básicas da criação de um aplicativo Web com um back-end ASP.NET Web API. O tutorial usa Entity Framework 6 para o Lay-in de dados...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 0f5dc960f494af5bd4ce87863a510d1892319908
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622475"
---
# <a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="7246c-104">Uso da API Web 2 com o Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="7246c-104">Using Web API 2 with Entity Framework 6</span></span>

[<span data-ttu-id="7246c-105">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="7246c-105">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="7246c-106">Este tutorial ensina as noções básicas da criação de um aplicativo Web com um back-end ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="7246c-106">This tutorial teaches you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="7246c-107">O tutorial usa Entity Framework 6 para a camada de dados e knockout. js para o aplicativo JavaScript do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="7246c-107">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="7246c-108">O tutorial também mostra como implantar o aplicativo em aplicativos Web do Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="7246c-108">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7246c-109">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="7246c-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="7246c-110">API Web 2,1</span><span class="sxs-lookup"><span data-stu-id="7246c-110">Web API 2.1</span></span>
> - <span data-ttu-id="7246c-111">Visual Studio 2017 (Baixe o Visual Studio 2017 [aqui](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="7246c-111">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="7246c-112">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="7246c-112">Entity Framework 6</span></span>
> - <span data-ttu-id="7246c-113">.NET 4.7</span><span class="sxs-lookup"><span data-stu-id="7246c-113">.NET 4.7</span></span>
> - <span data-ttu-id="7246c-114">[Knockout. js](http://knockoutjs.com/) 3,1</span><span class="sxs-lookup"><span data-stu-id="7246c-114">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>

<span data-ttu-id="7246c-115">Este tutorial usa ASP.NET Web API 2 com Entity Framework 6 para criar um aplicativo Web que manipula um banco de dados back-end.</span><span class="sxs-lookup"><span data-stu-id="7246c-115">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="7246c-116">Aqui está uma captura de tela do aplicativo que você criará.</span><span class="sxs-lookup"><span data-stu-id="7246c-116">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="7246c-117">O aplicativo usa um design de aplicativo de página única (SPA).</span><span class="sxs-lookup"><span data-stu-id="7246c-117">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="7246c-118">"Aplicativo de página única" é o termo geral para um aplicativo Web que carrega uma única página HTML e, em seguida, atualiza a página dinamicamente, em vez de carregar novas páginas.</span><span class="sxs-lookup"><span data-stu-id="7246c-118">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="7246c-119">Após o carregamento inicial da página, o aplicativo conversa com o servidor por meio de solicitações AJAX.</span><span class="sxs-lookup"><span data-stu-id="7246c-119">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="7246c-120">As solicitações AJAX retornam dados JSON, que o aplicativo usa para atualizar a interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="7246c-120">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="7246c-121">O AJAX não é novo, mas hoje há estruturas de JavaScript que facilitam a criação e a manutenção de um aplicativo SPA sofisticado grande.</span><span class="sxs-lookup"><span data-stu-id="7246c-121">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="7246c-122">Este tutorial usa [knockout. js](http://knockoutjs.com/), mas você pode usar qualquer estrutura de cliente JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7246c-122">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="7246c-123">Aqui estão os principais blocos de construção para este aplicativo:</span><span class="sxs-lookup"><span data-stu-id="7246c-123">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="7246c-124">O ASP.NET MVC cria a página HTML.</span><span class="sxs-lookup"><span data-stu-id="7246c-124">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="7246c-125">ASP.NET Web API manipula as solicitações AJAX e retorna dados JSON.</span><span class="sxs-lookup"><span data-stu-id="7246c-125">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="7246c-126">Dados knockout. js – associa os elementos HTML aos dados JSON.</span><span class="sxs-lookup"><span data-stu-id="7246c-126">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="7246c-127">Entity Framework conversa com o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7246c-127">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="7246c-128">Consulte este aplicativo em execução no Azure</span><span class="sxs-lookup"><span data-stu-id="7246c-128">See this app running on Azure</span></span>

<span data-ttu-id="7246c-129">Você gostaria de ver o site concluído em execução como um aplicativo Web ativo?</span><span class="sxs-lookup"><span data-stu-id="7246c-129">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="7246c-130">Você pode implantar uma versão completa do aplicativo em sua conta do Azure selecionando o botão a seguir.</span><span class="sxs-lookup"><span data-stu-id="7246c-130">You can deploy a complete version of the app to your Azure account by selecting the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="7246c-131">Você precisa de uma conta do Azure para implantar essa solução no Azure.</span><span class="sxs-lookup"><span data-stu-id="7246c-131">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="7246c-132">Se você ainda não tiver uma conta, terá as seguintes opções:</span><span class="sxs-lookup"><span data-stu-id="7246c-132">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="7246c-133">[Abra uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -você obtém créditos que podem ser usados para experimentar serviços pagos do Azure e, mesmo depois que eles são usados, você pode manter a conta e usar os serviços gratuitos do Azure.</span><span class="sxs-lookup"><span data-stu-id="7246c-133">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="7246c-134">[Ativar os benefícios do assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -sua assinatura do MSDN fornece créditos todos os meses que você pode usar para serviços pagos do Azure.</span><span class="sxs-lookup"><span data-stu-id="7246c-134">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="7246c-135">Criar o projeto</span><span class="sxs-lookup"><span data-stu-id="7246c-135">Create the project</span></span>

<span data-ttu-id="7246c-136">Abra o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7246c-136">Open Visual Studio.</span></span> <span data-ttu-id="7246c-137">No menu **arquivo** , selecione **novo**e **projeto**.</span><span class="sxs-lookup"><span data-stu-id="7246c-137">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="7246c-138">(Ou selecione **novo projeto** na página inicial.)</span><span class="sxs-lookup"><span data-stu-id="7246c-138">(Or select **New Project** on the Start page.)</span></span>

<span data-ttu-id="7246c-139">Na caixa de diálogo **novo projeto** , selecione **Web** no painel esquerdo e **ASP.NET aplicativo Web (.NET Framework)** no painel central.</span><span class="sxs-lookup"><span data-stu-id="7246c-139">In the **New Project** dialog, select **Web** in the left pane and **ASP.NET Web Application (.NET Framework)** in the middle pane.</span></span> <span data-ttu-id="7246c-140">Nomeie o projeto **BookService** e selecione **OK**.</span><span class="sxs-lookup"><span data-stu-id="7246c-140">Name the project **BookService** and select **OK**.</span></span>

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

<span data-ttu-id="7246c-141">Na caixa de diálogo **novo projeto ASP.net** , selecione o modelo **API Web** .</span><span class="sxs-lookup"><span data-stu-id="7246c-141">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image12.png)](part-1/_static/image12.png)

<span data-ttu-id="7246c-142">Selecione **OK** para criar o projeto.</span><span class="sxs-lookup"><span data-stu-id="7246c-142">Select **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="7246c-143">Definir configurações do Azure (opcional)</span><span class="sxs-lookup"><span data-stu-id="7246c-143">Configure Azure settings (optional)</span></span>

<span data-ttu-id="7246c-144">Depois de criar o projeto, você pode optar por implantar o Azure App aplicativos Web do serviço a qualquer momento.</span><span class="sxs-lookup"><span data-stu-id="7246c-144">After you create the project, you can choose to deploy to Azure App Service Web Apps at any time.</span></span> 

1. <span data-ttu-id="7246c-145">Em Gerenciador de Soluções, clique com o botão direito do mouse em seu projeto e selecione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="7246c-145">In Solution Explorer, right-click on your project and select **Publish**.</span></span>

2. <span data-ttu-id="7246c-146">Na janela exibida, selecione **Iniciar**.</span><span class="sxs-lookup"><span data-stu-id="7246c-146">In the window that appears, select **Start**.</span></span> <span data-ttu-id="7246c-147">A caixa de diálogo **selecionar um destino de publicação** é exibida.</span><span class="sxs-lookup"><span data-stu-id="7246c-147">The **Pick a publish target** dialog box appears.</span></span>

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. <span data-ttu-id="7246c-148">Selecione **Criar Perfil**.</span><span class="sxs-lookup"><span data-stu-id="7246c-148">Select **Create Profile**.</span></span> <span data-ttu-id="7246c-149">A caixa de diálogo **Criar Serviço de Aplicativo** é exibida.</span><span class="sxs-lookup"><span data-stu-id="7246c-149">The **Create App Service** dialog box appears.</span></span>

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   <span data-ttu-id="7246c-150">Aceite os padrões ou insira valores diferentes para o nome do aplicativo, grupo de recursos, plano de hospedagem, assinatura do Azure e região geográfica.</span><span class="sxs-lookup"><span data-stu-id="7246c-150">Accept the defaults, or enter different values for the application name, resource group, hosting plan, Azure subscription, and geographical region.</span></span> 

4. <span data-ttu-id="7246c-151">Selecione **criar um banco de dados SQL**.</span><span class="sxs-lookup"><span data-stu-id="7246c-151">Select **Create a SQL database**.</span></span> <span data-ttu-id="7246c-152">A caixa de diálogo **configurar SQL Server** é exibida.</span><span class="sxs-lookup"><span data-stu-id="7246c-152">The **Configure SQL Server** dialog box appears.</span></span> 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   <span data-ttu-id="7246c-153">Aceite os padrões ou insira valores diferentes.</span><span class="sxs-lookup"><span data-stu-id="7246c-153">Accept the defaults or enter different values.</span></span> <span data-ttu-id="7246c-154">Insira um **nome de usuário de administrador** e uma **senha de administrador** para o novo banco de dados.</span><span class="sxs-lookup"><span data-stu-id="7246c-154">Enter an **Administrator Username** and **Administrator Password** for your new database.</span></span> <span data-ttu-id="7246c-155">Selecione **OK** quando terminar.</span><span class="sxs-lookup"><span data-stu-id="7246c-155">Select **OK** when you're done.</span></span> <span data-ttu-id="7246c-156">A página **Criar serviço de aplicativo** é exibida novamente.</span><span class="sxs-lookup"><span data-stu-id="7246c-156">The **Create App Service** page reappears.</span></span>

5. <span data-ttu-id="7246c-157">Selecione **criar** para criar seu perfil.</span><span class="sxs-lookup"><span data-stu-id="7246c-157">Select **Create** to create your profile.</span></span> <span data-ttu-id="7246c-158">Uma mensagem é exibida no canto inferior direito, indicando que a implantação está em andamento.</span><span class="sxs-lookup"><span data-stu-id="7246c-158">A message appears in the lower-right corner indicating that deployment is in progress.</span></span> <span data-ttu-id="7246c-159">Depois de um pouco, a janela de **publicação** reaparece.</span><span class="sxs-lookup"><span data-stu-id="7246c-159">After a short while, the **Publish** window reappears.</span></span>

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    <span data-ttu-id="7246c-160">O perfil que você criou para implantar o aplicativo agora está disponível.</span><span class="sxs-lookup"><span data-stu-id="7246c-160">The profile you created to deploy the app is now available.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="7246c-161">Próximo</span><span class="sxs-lookup"><span data-stu-id="7246c-161">Next</span></span>](part-2.md)
