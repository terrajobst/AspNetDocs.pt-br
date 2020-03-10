---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Criar um perfil e depurar seu aplicativo MVC do ASP.NET com a visão | Microsoft Docs
author: Rick-Anderson
description: A idéia é uma família de sucesso e crescente de pacotes NuGet de software livre que fornece informações detalhadas de desempenho, depuração e diagnóstico para ASP.NET a...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: d3689147a3bc3aa1f4180c377d2483a94bdd95a9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538531"
---
# <a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="22591-103">Analisar e depurar seu aplicativo do ASP.NET MVC com Glimpse</span><span class="sxs-lookup"><span data-stu-id="22591-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>

<span data-ttu-id="22591-104">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="22591-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="22591-105">A visão é uma família de sucesso e crescente de pacotes NuGet de software livre que fornece informações detalhadas de desempenho, depuração e diagnóstico para aplicativos ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="22591-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="22591-106">É trivial instalar, leve, extremamente rápido e exibir as principais métricas de desempenho na parte inferior de cada página.</span><span class="sxs-lookup"><span data-stu-id="22591-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="22591-107">Ele permite que você faça uma busca detalhada em seu aplicativo quando precisar descobrir o que está acontecendo no servidor.</span><span class="sxs-lookup"><span data-stu-id="22591-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="22591-108">A idéia fornece informações valiosas que recomendamos que você o use em todo o ciclo de desenvolvimento, incluindo o ambiente de teste do Azure.</span><span class="sxs-lookup"><span data-stu-id="22591-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="22591-109">Embora o [Fiddler](http://www.telerik.com/fiddler) e as [ferramentas de desenvolvimento F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) forneçam uma exibição do lado do cliente, a idéia fornece uma exibição detalhada do servidor.</span><span class="sxs-lookup"><span data-stu-id="22591-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="22591-110">Este tutorial se concentrará no uso dos pacotes ASP.NET MVC e EF da visão, mas muitos outros pacotes estarão disponíveis.</span><span class="sxs-lookup"><span data-stu-id="22591-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="22591-111">Quando possível, vou vincular aos documentos de [amostra](http://getglimpse.com/Docs/) apropriados que eu ajude a manter.</span><span class="sxs-lookup"><span data-stu-id="22591-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="22591-112">A idéia é um projeto de software livre, você também pode contribuir para o código-fonte e os documentos.</span><span class="sxs-lookup"><span data-stu-id="22591-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>

- [<span data-ttu-id="22591-113">Instalando a visão</span><span class="sxs-lookup"><span data-stu-id="22591-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="22591-114">Habilitar a visão do localhost</span><span class="sxs-lookup"><span data-stu-id="22591-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="22591-115">A guia linha do tempo</span><span class="sxs-lookup"><span data-stu-id="22591-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="22591-116">Model binding</span><span class="sxs-lookup"><span data-stu-id="22591-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="22591-117">Rotas</span><span class="sxs-lookup"><span data-stu-id="22591-117">Routes</span></span>](#route)
- [<span data-ttu-id="22591-118">Usando a visão no Azure</span><span class="sxs-lookup"><span data-stu-id="22591-118">Using Glimpse on Azure</span></span>](#da)
- [<span data-ttu-id="22591-119">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="22591-119">Additional Resources</span></span>](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="22591-120">Instalando a visão</span><span class="sxs-lookup"><span data-stu-id="22591-120">Installing Glimpse</span></span>

<span data-ttu-id="22591-121">Você pode instalar a visão do console do Gerenciador de pacotes NuGet ou do console **gerenciar pacotes NuGet** .</span><span class="sxs-lookup"><span data-stu-id="22591-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="22591-122">Para esta demonstração, instalarei os pacotes Mvc5 e EF6:</span><span class="sxs-lookup"><span data-stu-id="22591-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![instalar a visão do NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="22591-124">Pesquisar por *idéia. EF*</span><span class="sxs-lookup"><span data-stu-id="22591-124">Search for *Glimpse.EF*</span></span>

![. EF da instalação do NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="22591-126">Selecionando **pacotes instalados**, você pode ver os módulos dependentes da visão instalados:</span><span class="sxs-lookup"><span data-stu-id="22591-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Pacotes de amostra instalados de DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="22591-128">Os comandos a seguir instalam os módulos MVC5 e EF6 do console do Gerenciador de pacotes:</span><span class="sxs-lookup"><span data-stu-id="22591-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="22591-129">Habilitar a visão do localhost</span><span class="sxs-lookup"><span data-stu-id="22591-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="22591-130">Navegue até http://localhost:&lt;p classificar #&gt;/glimpse.axd e clique no botão <strong>Ativar visão</strong> .</span><span class="sxs-lookup"><span data-stu-id="22591-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Página de visualização do axd](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="22591-132">Se sua barra de favoritos for exibida, você poderá arrastar e soltar os botões de visão e adicioná-los como bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="22591-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![IE com bookmarklets de visão](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="22591-134">Agora você pode navegar no seu aplicativo e a **exibição de cabeçotes** (HUD) é mostrada na parte inferior da página.</span><span class="sxs-lookup"><span data-stu-id="22591-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Página do Gerenciador de contatos com HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="22591-136">A [página de visão de HUD](http://getglimpse.com/Docs/Heads-up-Display) detalha as informações de tempo mostradas acima.</span><span class="sxs-lookup"><span data-stu-id="22591-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="22591-137">Os dados de desempenho discretos exibidos pelo HUD podem notificá-lo de um problema imediatamente-antes de chegar ao ciclo de teste.</span><span class="sxs-lookup"><span data-stu-id="22591-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="22591-138">Clicar no &quot;g&quot; no canto inferior direito abre o painel de visão:</span><span class="sxs-lookup"><span data-stu-id="22591-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Painel de visão](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="22591-140">Na imagem acima, a [guia execução](http://getglimpse.com/Docs/Execution-Tab) é selecionada, que mostra os detalhes de tempo das ações e dos filtros no pipeline.</span><span class="sxs-lookup"><span data-stu-id="22591-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="22591-141">Você pode ver meu [temporizador parar filtro de monitoramento](http://www.nuget.org/packages/StopWatch/) iniciar no estágio 6 do pipeline.</span><span class="sxs-lookup"><span data-stu-id="22591-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="22591-142">Embora meu temporizador leve possa fornecer dados úteis de perfil/tempo, ele perde o tempo gasto na autorização e renderizando a exibição.</span><span class="sxs-lookup"><span data-stu-id="22591-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="22591-143">Você pode ler sobre meu temporizador no [perfil e cronometrar seu aplicativo ASP.NET MVC até o Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="22591-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="22591-144">A página [guias](http://getglimpse.com/Docs/Tabs) fornece links para informações detalhadas sobre cada guia.</span><span class="sxs-lookup"><span data-stu-id="22591-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="22591-145">A guia linha do tempo</span><span class="sxs-lookup"><span data-stu-id="22591-145">The Timeline tab</span></span>

<span data-ttu-id="22591-146">Modifiquei o [tutorial do EF 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) pendente de Tom Dykstra com a seguinte alteração de código para o controlador de instrutores:</span><span class="sxs-lookup"><span data-stu-id="22591-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="22591-147">O código acima permite que eu transmita uma cadeia de caracteres de consulta (`eager`) para controlar o carregamento rápido ou explícito dos dados.</span><span class="sxs-lookup"><span data-stu-id="22591-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="22591-148">Na imagem abaixo, o carregamento explícito é usado e a página de tempo mostra cada registro carregado no método de ação `Index`:</span><span class="sxs-lookup"><span data-stu-id="22591-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![carregamento explícito](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="22591-150">No código a seguir, o adiantamento é especificado e cada registro é obtido depois que a exibição de `Index` é chamada:</span><span class="sxs-lookup"><span data-stu-id="22591-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![adiantado é especificado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="22591-152">Você pode focalizar um segmento de tempo para obter informações detalhadas de tempo:</span><span class="sxs-lookup"><span data-stu-id="22591-152">You can hover over a time segment to get detailed timing information:</span></span>

![focalizar para ver o tempo detalhado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="22591-154">Model binding</span><span class="sxs-lookup"><span data-stu-id="22591-154">Model Binding</span></span>

<span data-ttu-id="22591-155">A [guia Associação de modelo](http://getglimpse.com/Docs/Model-Binding-Tab) fornece uma grande quantidade de informações para ajudá-lo a entender como as variáveis de formulário são ligadas e por que algumas delas não estão associadas como esperado.</span><span class="sxs-lookup"><span data-stu-id="22591-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="22591-156">A imagem abaixo mostra o **?**</span><span class="sxs-lookup"><span data-stu-id="22591-156">The image below shows the **?**</span></span> <span data-ttu-id="22591-157">, no qual você pode clicar para exibir a página de ajuda de visão desse recurso.</span><span class="sxs-lookup"><span data-stu-id="22591-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![exibição de associação de modelo de visão](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="22591-159">Rotas</span><span class="sxs-lookup"><span data-stu-id="22591-159">Routes</span></span>

 <span data-ttu-id="22591-160">A guia mostrar rotas pode ajudá-lo a depurar e entender o roteamento.</span><span class="sxs-lookup"><span data-stu-id="22591-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="22591-161">Na imagem abaixo, a rota do produto é selecionada (e mostra em verde, uma Convenção de visão).</span><span class="sxs-lookup"><span data-stu-id="22591-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="22591-162">![nome do produto selecionado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) restrições de rota, as áreas e os tokens de dados também são exibidos.</span><span class="sxs-lookup"><span data-stu-id="22591-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="22591-163">Consulte [visão de rotas](http://getglimpse.com/Docs/Routes-Tab) e [Roteamento de atributos no ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) para obter mais informações.</span><span class="sxs-lookup"><span data-stu-id="22591-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="22591-164">Usando a visão no Azure</span><span class="sxs-lookup"><span data-stu-id="22591-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="22591-165">A política de segurança padrão de exibição só permite que os dados da amostra sejam exibidos do host local.</span><span class="sxs-lookup"><span data-stu-id="22591-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="22591-166">Você pode alterar essa política de segurança para que possa exibir esses dados em um servidor remoto (como um aplicativo Web no Azure).</span><span class="sxs-lookup"><span data-stu-id="22591-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="22591-167">Para ambientes de teste no Azure, adicione a marca realçada à parte inferior do arquivo *Web. config* para habilitar a visão:</span><span class="sxs-lookup"><span data-stu-id="22591-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.config* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="22591-168">Com essa alteração sozinha, qualquer usuário pode ver seus dados de sua amostra em um site remoto.</span><span class="sxs-lookup"><span data-stu-id="22591-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="22591-169">Considere adicionar a marcação acima a um perfil de publicação para que ele seja implantado apenas quando você usar esse perfil de publicação (por exemplo, seu perfil de teste do Azure). Para restringir os dados da visão, adicionaremos a função `canViewGlimpseData` e apenas permitirá que os usuários nessa função exibam dados de visão.</span><span class="sxs-lookup"><span data-stu-id="22591-169">Consider adding the markup above to a publish profile so it's only deployed an applied when you use that publish profile (for example, your Azure test profile.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="22591-170">Remova os comentários do arquivo *GlimpseSecurityPolicy.cs* e altere a chamada [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) de `Administrator` para a função `canViewGlimpseData`:</span><span class="sxs-lookup"><span data-stu-id="22591-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="22591-171">Segurança-os dados avançados fornecidos pela visão podem expor a segurança do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="22591-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="22591-172">A Microsoft não realizou uma auditoria de segurança de idéia para uso em aplicativos de produções.</span><span class="sxs-lookup"><span data-stu-id="22591-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>

<span data-ttu-id="22591-173">Para obter informações sobre como adicionar funções, consulte o tutorial [implantar um aplicativo Web do ASP.NET MVC 5 com associação, OAuth e banco de dados SQL no Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) .</span><span class="sxs-lookup"><span data-stu-id="22591-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="22591-174">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="22591-174">Additional Resources</span></span>

- [<span data-ttu-id="22591-175">Implantar um aplicativo Secure ASP.NET MVC 5 com associação, OAuth e banco de dados SQL no Azure</span><span class="sxs-lookup"><span data-stu-id="22591-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="22591-176">[Visão](http://getglimpse.com/Docs/Configuration) da página configuração – documento na configuração de guias, política de tempo de execução, registro em log e muito mais.</span><span class="sxs-lookup"><span data-stu-id="22591-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
