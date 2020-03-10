---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Acompanhando informações do visitante (Analytics) para um site Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Depois de acessar seu site, talvez você queira analisar o tráfego do site.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 095a5572c755446e0661c052ca9de82d636429fd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525182"
---
# <a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="3786c-103">Acompanhamento de informações do visitante (análise) para um site Páginas da Web do ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="3786c-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="3786c-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3786c-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3786c-105">Este artigo descreve como usar um auxiliar para adicionar a análise de site a páginas em um site Páginas da Web do ASP.NET (Razor).</span><span class="sxs-lookup"><span data-stu-id="3786c-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="3786c-106">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="3786c-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="3786c-107">Como enviar informações sobre o tráfego de seu site para um provedor de análise.</span><span class="sxs-lookup"><span data-stu-id="3786c-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="3786c-108">Estes são os recursos de programação do ASP.NET apresentados no artigo:</span><span class="sxs-lookup"><span data-stu-id="3786c-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="3786c-109">O auxiliar de `Analytics`.</span><span class="sxs-lookup"><span data-stu-id="3786c-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3786c-110">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="3786c-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3786c-111">Páginas da Web do ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="3786c-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="3786c-112">Biblioteca de auxiliares Web do ASP.NET (pacote NuGet)</span><span class="sxs-lookup"><span data-stu-id="3786c-112">ASP.NET Web Helpers Library (NuGet package)</span></span>

<span data-ttu-id="3786c-113">A análise é um termo geral para a tecnologia que mede o tráfego em seu site para que você possa entender como as pessoas usam o site.</span><span class="sxs-lookup"><span data-stu-id="3786c-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="3786c-114">Muitos serviços de análise estão disponíveis, incluindo serviços do Google, Yahoo, StatCounter e outros.</span><span class="sxs-lookup"><span data-stu-id="3786c-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="3786c-115">A maneira como o Analytics funciona é que você se inscreve em uma conta com o provedor de análise, no qual você registra o site que deseja acompanhar. O provedor envia um trecho de código JavaScript que inclui uma ID ou um código de rastreamento para sua conta.</span><span class="sxs-lookup"><span data-stu-id="3786c-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="3786c-116">Você adiciona o trecho de JavaScript às páginas da Web no site que deseja acompanhar. (Normalmente, você adiciona o trecho de análise a uma página de rodapé ou de layout ou a outra marcação HTML que aparece em cada página do site.) Quando os usuários solicitam uma página que contém um desses trechos de código JavaScript, o trecho de código envia informações sobre a página atual para o provedor de análise, que registra vários detalhes sobre a página.</span><span class="sxs-lookup"><span data-stu-id="3786c-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="3786c-117">Quando você quiser ter uma olhada nas estatísticas do site, faça logon no site do provedor de análise.</span><span class="sxs-lookup"><span data-stu-id="3786c-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="3786c-118">Você pode exibir todos os tipos de relatórios sobre seu site, como:</span><span class="sxs-lookup"><span data-stu-id="3786c-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="3786c-119">O número de exibições de página para páginas individuais.</span><span class="sxs-lookup"><span data-stu-id="3786c-119">The number of page views for individual pages.</span></span> <span data-ttu-id="3786c-120">Isso informa (aproximadamente) quantas pessoas estão visitando o site e quais páginas do seu site são as mais populares.</span><span class="sxs-lookup"><span data-stu-id="3786c-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="3786c-121">Quanto tempo as pessoas gastam em páginas específicas.</span><span class="sxs-lookup"><span data-stu-id="3786c-121">How long people spend on specific pages.</span></span> <span data-ttu-id="3786c-122">Isso pode lhe dizer coisas como se sua home page está mantendo o interesse das pessoas.</span><span class="sxs-lookup"><span data-stu-id="3786c-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="3786c-123">Em que sites as pessoas estavam antes de visitar seu site.</span><span class="sxs-lookup"><span data-stu-id="3786c-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="3786c-124">Isso ajuda você a entender se o tráfego está vindo de links, de pesquisas e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="3786c-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="3786c-125">Quando as pessoas visitam seu site e por quanto tempo elas permanecem.</span><span class="sxs-lookup"><span data-stu-id="3786c-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="3786c-126">De quais países seus visitantes são.</span><span class="sxs-lookup"><span data-stu-id="3786c-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="3786c-127">Quais navegadores e sistemas operacionais seus visitantes estão usando.</span><span class="sxs-lookup"><span data-stu-id="3786c-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="3786c-129">Usando um auxiliar para adicionar análise a uma página</span><span class="sxs-lookup"><span data-stu-id="3786c-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="3786c-130">O Páginas da Web do ASP.NET inclui vários auxiliares de análise (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`e `Analytics.GetStatCounterHtml`) que facilitam o gerenciamento dos trechos de JavaScript usados para análise.</span><span class="sxs-lookup"><span data-stu-id="3786c-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="3786c-131">Em vez de descobrir como e onde colocar o código JavaScript, tudo o que você precisa fazer é adicionar o auxiliar a uma página.</span><span class="sxs-lookup"><span data-stu-id="3786c-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="3786c-132">As únicas informações que você precisa fornecer são o nome da sua conta, ID ou código de rastreamento.</span><span class="sxs-lookup"><span data-stu-id="3786c-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="3786c-133">(Para StatCounter, você também precisa fornecer alguns valores adicionais.)</span><span class="sxs-lookup"><span data-stu-id="3786c-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="3786c-134">Neste procedimento, você criará uma página de layout que usa o auxiliar de `GetGoogleHtml`.</span><span class="sxs-lookup"><span data-stu-id="3786c-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="3786c-135">Se você já tiver uma conta com um dos outros provedores de análise, poderá usar essa conta e fazer pequenos ajustes conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="3786c-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="3786c-136">Ao criar uma conta de análise, você registra a URL do site que deseja controlar.</span><span class="sxs-lookup"><span data-stu-id="3786c-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="3786c-137">Se você estiver testando tudo em seu computador local, não estará controlando o tráfego real (o único tráfego é você), portanto, você não conseguirá registrar e exibir as estatísticas do site.</span><span class="sxs-lookup"><span data-stu-id="3786c-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="3786c-138">Mas esse procedimento mostra como você adiciona um auxiliar de análise a uma página.</span><span class="sxs-lookup"><span data-stu-id="3786c-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="3786c-139">Quando você publicar seu site, o site ativo enviará informações ao seu provedor de análise.</span><span class="sxs-lookup"><span data-stu-id="3786c-139">When you publish your site, the live site will send information to your analytics provider.</span></span>

1. <span data-ttu-id="3786c-140">Adicione a biblioteca de auxiliares Web do ASP.NET ao seu site, conforme descrito em [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não o tiver adicionado.</span><span class="sxs-lookup"><span data-stu-id="3786c-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="3786c-141">Crie uma conta com o Google Analytics e registre o nome da conta.</span><span class="sxs-lookup"><span data-stu-id="3786c-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="3786c-142">Crie uma página de layout chamada *Analytics. cshtml* e adicione a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="3786c-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="3786c-143">Você deve fazer a chamada para o auxiliar de `Analytics` no corpo da sua página da Web (antes da marca de `</body>`).</span><span class="sxs-lookup"><span data-stu-id="3786c-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="3786c-144">Caso contrário, o navegador não executará o script.</span><span class="sxs-lookup"><span data-stu-id="3786c-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="3786c-145">Se você estiver usando um provedor de análise diferente, use um dos seguintes auxiliares em vez disso:</span><span class="sxs-lookup"><span data-stu-id="3786c-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="3786c-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="3786c-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="3786c-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="3786c-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="3786c-148">Substitua `myaccount` pelo nome da conta, ID ou código de rastreamento que você criou na etapa 1.</span><span class="sxs-lookup"><span data-stu-id="3786c-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="3786c-149">Execute a página no navegador.</span><span class="sxs-lookup"><span data-stu-id="3786c-149">Run the page in the browser.</span></span> <span data-ttu-id="3786c-150">(Verifique se a página está selecionada no espaço de trabalho **arquivos** antes de executá-la.)</span><span class="sxs-lookup"><span data-stu-id="3786c-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="3786c-151">No navegador, exiba a origem da página.</span><span class="sxs-lookup"><span data-stu-id="3786c-151">In the browser, view the page source.</span></span> <span data-ttu-id="3786c-152">Você poderá ver o código de análise renderizado:</span><span class="sxs-lookup"><span data-stu-id="3786c-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="3786c-153">Faça logon no site do Google Analytics e examine as estatísticas do seu site.</span><span class="sxs-lookup"><span data-stu-id="3786c-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="3786c-154">Se estiver executando a página em um site ativo, você verá uma entrada que registra a visita à sua página.</span><span class="sxs-lookup"><span data-stu-id="3786c-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="3786c-155">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="3786c-155">Additional Resources</span></span>

- [<span data-ttu-id="3786c-156">Site do Google Analytics</span><span class="sxs-lookup"><span data-stu-id="3786c-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="3786c-157">Site do Yahoo! Web Analytics</span><span class="sxs-lookup"><span data-stu-id="3786c-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="3786c-158">Site do StatCounter</span><span class="sxs-lookup"><span data-stu-id="3786c-158">StatCounter site</span></span>](http://statcounter.com/)
