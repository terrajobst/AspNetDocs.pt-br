---
uid: visual-studio/overview/2013/using-browser-link
title: Usando o link do navegador no Visual Studio 2013 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 723a38de4569b0bb58817c70aabb84fef8e19591
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622902"
---
# <a name="using-browser-link-in-visual-studio-2013"></a><span data-ttu-id="5ab4e-102">Usando o link do navegador no Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="5ab4e-102">Using Browser Link in Visual Studio 2013</span></span>

<span data-ttu-id="5ab4e-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5ab4e-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5ab4e-104">O link do navegador é um novo recurso do Visual Studio 2013 que cria um canal de comunicação entre o ambiente de desenvolvimento e um ou mais navegadores da Web.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-104">Browser Link is a new feature in Visual Studio 2013 that creates a communication channel between the development environment and one or more web browsers.</span></span> <span data-ttu-id="5ab4e-105">Você pode usar o link do navegador para atualizar seu aplicativo Web em vários navegadores de uma vez, o que é útil para testes entre navegadores.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-105">You can use Browser Link to refresh your web application in several browsers at once, which is useful for cross-browser testing.</span></span>

- [<span data-ttu-id="5ab4e-106">Atualização do navegador</span><span class="sxs-lookup"><span data-stu-id="5ab4e-106">Browser Refresh</span></span>](#browser-refresh)
- [<span data-ttu-id="5ab4e-107">Exibindo o painel do link do navegador</span><span class="sxs-lookup"><span data-stu-id="5ab4e-107">Viewing the Browser Link Dashboard</span></span>](#dashboard)
- [<span data-ttu-id="5ab4e-108">Habilitando o link do navegador para arquivos HTML estáticos</span><span class="sxs-lookup"><span data-stu-id="5ab4e-108">Enabling Browser Link for Static HTML Files</span></span>](#static-html)
- [<span data-ttu-id="5ab4e-109">Desativando o link do navegador</span><span class="sxs-lookup"><span data-stu-id="5ab4e-109">Disabling Browser Link</span></span>](#disabling)
- [<span data-ttu-id="5ab4e-110">Como funciona?</span><span class="sxs-lookup"><span data-stu-id="5ab4e-110">How Does It Work?</span></span>](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a><span data-ttu-id="5ab4e-111">Atualização do navegador</span><span class="sxs-lookup"><span data-stu-id="5ab4e-111">Browser Refresh</span></span>

<span data-ttu-id="5ab4e-112">Com a atualização do navegador, você pode atualizar vários navegadores que estão conectados ao Visual Studio por meio do link do navegador.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-112">With Browser Refresh, you can refresh multiple browsers that are connected to Visual Studio through Browser Link.</span></span>

<span data-ttu-id="5ab4e-113">Para usar a atualização do navegador, primeiro crie um aplicativo ASP.NET, usando qualquer um dos modelos de projeto.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-113">To use Browser Refresh, first create an ASP.NET application, using any of the project templates.</span></span> <span data-ttu-id="5ab4e-114">Depure o aplicativo pressionando F5 ou clicando no ícone de seta na barra de ferramentas:</span><span class="sxs-lookup"><span data-stu-id="5ab4e-114">Debug the application by pressing F5 or clicking the arrow icon in the toolbar:</span></span>

![](using-browser-link/_static/image1.png)

<span data-ttu-id="5ab4e-115">Você também pode usar a lista suspensa para selecionar um navegador específico para depuração.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-115">You can also use the dropdown to select a specific browser for debugging.</span></span>

![](using-browser-link/_static/image2.png)

<span data-ttu-id="5ab4e-116">Para depurar com vários navegadores, selecione **procurar com**.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-116">To debug with multiple browsers, select **Browse With**.</span></span> <span data-ttu-id="5ab4e-117">Na caixa de diálogo **procurar com** , mantenha pressionada a tecla CTRL para selecionar mais de um navegador.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-117">In the **Browse With** dialog, hold down the CTRL key to select more than one browser.</span></span> <span data-ttu-id="5ab4e-118">Clique em **procurar** para depurar com os navegadores selecionados.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-118">Click **Browse** to debug with the selected browsers.</span></span> <span data-ttu-id="5ab4e-119">O link do navegador também funcionará se você iniciar um navegador fora do Visual Studio e navegar até a URL do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-119">Browser Link also works if you launch a browser from outside Visual Studio and navigate to the application URL.</span></span>

![](using-browser-link/_static/image3.png)

<span data-ttu-id="5ab4e-120">Os controles de link do navegador estão localizados no menu suspenso com o ícone de seta circular.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-120">The Browser Link controls are located in the dropdown with the circular arrow icon.</span></span> <span data-ttu-id="5ab4e-121">O ícone de seta é o botão **Atualizar** .</span><span class="sxs-lookup"><span data-stu-id="5ab4e-121">The arrow icon is the **Refresh** button.</span></span>

![](using-browser-link/_static/image4.png)

<span data-ttu-id="5ab4e-122">Para ver quais navegadores estão conectados, passe o mouse sobre o botão **Atualizar** durante a depuração.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-122">To see which browsers are connected, hover the mouse over the **Refresh** button while debugging.</span></span> <span data-ttu-id="5ab4e-123">Os navegadores conectados são mostrados em uma janela de dica de ferramenta.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-123">The connected browsers are shown in a ToolTip window.</span></span>

![](using-browser-link/_static/image5.png)

<span data-ttu-id="5ab4e-124">Para atualizar os navegadores conectados, clique no botão **Atualizar** ou pressione Ctrl + Alt + Enter.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-124">To refresh the connected browsers, click the **Refresh** button or press CTRL+ALT+ENTER.</span></span> <span data-ttu-id="5ab4e-125">Por exemplo, a captura de tela a seguir mostra um projeto ASP.NET, que criei usando o modelo de projeto MVC 5.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-125">For example, the following screenshot shows an ASP.NET project, which I created using the MVC 5 project template.</span></span> <span data-ttu-id="5ab4e-126">Você pode ver o aplicativo em execução em dois navegadores na parte superior.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-126">You can see the application running in two browsers at the top.</span></span> <span data-ttu-id="5ab4e-127">Na parte inferior, o projeto é aberto no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-127">At the bottom, the project is open in Visual Studio.</span></span>

![](using-browser-link/_static/image6.png)

<span data-ttu-id="5ab4e-128">No Visual Studio, alterei o rumo &lt;H1&gt; para o home page:</span><span class="sxs-lookup"><span data-stu-id="5ab4e-128">In Visual Studio, I changed the &lt;h1&gt; heading for the home page:</span></span>

![](using-browser-link/_static/image7.png)

<span data-ttu-id="5ab4e-129">Quando eu cliquei no botão **Atualizar** , a alteração apareceu nas janelas do navegador:</span><span class="sxs-lookup"><span data-stu-id="5ab4e-129">When I clicked the **Refresh** button, the change appeared in both browser windows:</span></span>

![](using-browser-link/_static/image8.png)

<span data-ttu-id="5ab4e-130">**Observações**</span><span class="sxs-lookup"><span data-stu-id="5ab4e-130">**Notes**</span></span>

- <span data-ttu-id="5ab4e-131">Para habilitar o link do navegador, defina `debug=true` no elemento [&lt;de compilação&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) no arquivo Web. config do projeto.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-131">To enable Browser Link, set `debug=true` in the [&lt;compilation&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) element in the project's Web.config file.</span></span>
- <span data-ttu-id="5ab4e-132">O aplicativo deve estar em execução no localhost.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-132">The application must be running on localhost.</span></span>
- <span data-ttu-id="5ab4e-133">O aplicativo deve ter como destino o .NET 4,0 ou posterior.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-133">The application must target .NET 4.0 or later.</span></span>

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a><span data-ttu-id="5ab4e-134">Exibindo o painel do link do navegador</span><span class="sxs-lookup"><span data-stu-id="5ab4e-134">Viewing the Browser Link Dashboard</span></span>

<span data-ttu-id="5ab4e-135">O painel link do navegador mostra informações sobre as conexões de link do navegador.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-135">The Browser Link dashboard shows information about the Browser Link connections.</span></span> <span data-ttu-id="5ab4e-136">Para exibir o painel, selecione o menu suspenso link do navegador (a pequena seta ao lado do botão **Atualizar** ).</span><span class="sxs-lookup"><span data-stu-id="5ab4e-136">To view the dashboard, select the Browser Link dropdown menu (the small arrow next to the **Refresh** button).</span></span> <span data-ttu-id="5ab4e-137">Em seguida, clique em **painel de link do navegador**.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-137">Then click **Browser Link Dashboard**.</span></span>

![](using-browser-link/_static/image9.png)

<span data-ttu-id="5ab4e-138">O painel lista os navegadores conectados e a URL para a qual cada navegador navegou.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-138">The dashboard lists the connected Browsers and the URL to which each browser has navigated.</span></span>

![](using-browser-link/_static/image10.png)

<span data-ttu-id="5ab4e-139">A seção **pré-requisitos** mostra as etapas necessárias para habilitar o link do navegador para esse projeto.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-139">The **Prerequisites** section shows any steps needed to enable Browser Link for that project.</span></span> <span data-ttu-id="5ab4e-140">Por exemplo, a captura de tela a seguir mostra um projeto em que "debug" está definido como false no arquivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-140">For example, the following screenshot shows a project where "debug" is set to false in the Web.config file.</span></span>

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a><span data-ttu-id="5ab4e-141">Habilitando o link do navegador para arquivos HTML estáticos</span><span class="sxs-lookup"><span data-stu-id="5ab4e-141">Enabling Browser Link for Static HTML Files</span></span>

<span data-ttu-id="5ab4e-142">Para habilitar o link do navegador para arquivos HTML estáticos, adicione o seguinte ao seu arquivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-142">To enable Browser Link for static HTML files, add the following to your Web.config file.</span></span>

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

<span data-ttu-id="5ab4e-143">Por motivos de desempenho, remova essa configuração ao publicar seu projeto.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-143">For performance reasons, remove this setting when you publish your project.</span></span>

<a id="disabling"></a>
## <a name="disabling-browser-link"></a><span data-ttu-id="5ab4e-144">Desativando o link do navegador</span><span class="sxs-lookup"><span data-stu-id="5ab4e-144">Disabling Browser Link</span></span>

<span data-ttu-id="5ab4e-145">O link do navegador está habilitado por padrão.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-145">Browser Link is enabled by default.</span></span> <span data-ttu-id="5ab4e-146">Há várias maneiras de desabilitá-lo:</span><span class="sxs-lookup"><span data-stu-id="5ab4e-146">There are several ways to disable it:</span></span>

- <span data-ttu-id="5ab4e-147">No menu suspenso link do navegador, desmarque **habilitar link do navegador**.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-147">In the Browser Link dropdown menu, uncheck **Enable Browser Link**.</span></span> 

    ![](using-browser-link/_static/image12.png)
- <span data-ttu-id="5ab4e-148">No arquivo Web. config, adicione uma chave chamada "vs: EnableBrowserLink" com o valor "false" na seção appSettings.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-148">In the Web.config file, add a key named "vs:EnableBrowserLink" with the value "false" in the appSettings section.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- <span data-ttu-id="5ab4e-149">No arquivo Web. config, defina depurar como false.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-149">In the Web.config file, set debug to false.</span></span> 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a><span data-ttu-id="5ab4e-150">Como funciona?</span><span class="sxs-lookup"><span data-stu-id="5ab4e-150">How Does It Work?</span></span>

<span data-ttu-id="5ab4e-151">O link do navegador usa o [signalr](../../../signalr/index.md) para criar um canal de comunicação entre o Visual Studio e o navegador.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-151">Browser Link uses [SignalR](../../../signalr/index.md) to create a communication channel between Visual Studio and the browser.</span></span> <span data-ttu-id="5ab4e-152">Quando o link do navegador está habilitado, o Visual Studio atua como um servidor de sinalização ao qual vários clientes (navegadores) podem se conectar.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-152">When Browser Link is enabled, Visual Studio acts as a SignalR server that multiple clients (browsers) can connect to.</span></span> <span data-ttu-id="5ab4e-153">O link do navegador também registra um módulo HTTP com ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-153">Browser Link also registers an HTTP module with ASP.NET.</span></span> <span data-ttu-id="5ab4e-154">Esse módulo injeta &lt;de script especiais&gt; referências a todas as solicitações de página do servidor.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-154">This module injects special &lt;script&gt; references into every page request from the server.</span></span> <span data-ttu-id="5ab4e-155">Você pode ver as referências de script selecionando "Exibir origem" no navegador.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-155">You can see the script references by selecting "View source" in the browser.</span></span>

![](using-browser-link/_static/image13.png)

<span data-ttu-id="5ab4e-156">Os arquivos de origem não são modificados.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-156">Your source files are not modified.</span></span> <span data-ttu-id="5ab4e-157">O módulo HTTP injeta as referências de script dinamicamente.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-157">The HTTP module injects the script references dynamically.</span></span>

<span data-ttu-id="5ab4e-158">Como o código do lado do navegador é todo o JavaScript, ele funciona em todos os navegadores que o [signalr dá suporte](../../../signalr/overview/getting-started/supported-platforms.md), sem a necessidade de nenhum plug-in de navegador.</span><span class="sxs-lookup"><span data-stu-id="5ab4e-158">Because the browser-side code is all JavaScript, it works on all browsers that [SignalR supports](../../../signalr/overview/getting-started/supported-platforms.md), without requiring any browser plug-in.</span></span>
