---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Auxiliar do Twitter com Páginas da Web do ASP.NET | Microsoft Docs
author: Rick-Anderson
description: Este tópico e aplicativo mostram como adicionar um auxiliar do Twitter ao seu projeto do WebMatrix 3. Ele contém o código auxiliar do Twitter e mostra como chamar o auxiliar...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 76e32b7c808467a9a87c70017dac02bdb895e1df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638554"
---
# <a name="twitter-helper-with-aspnet-web-pages"></a><span data-ttu-id="9a613-104">Auxiliar do Twitter com Páginas da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9a613-104">Twitter Helper with ASP.NET Web Pages</span></span>

<span data-ttu-id="9a613-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="9a613-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9a613-106">Os auxiliares do Twitter estão obsoletos.</span><span class="sxs-lookup"><span data-stu-id="9a613-106">Twitter Helpers are obsolete.</span></span> <span data-ttu-id="9a613-107">Para obter as últimas ferramentas de envolvimento do Twitter para sites, consulte [visão geral do Twitter for sites](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span><span class="sxs-lookup"><span data-stu-id="9a613-107">For Twitter's latest engagement tools for websites, see [Twitter for Websites Overview](https://developer.twitter.com/en/docs/twitter-for-websites/overview).</span></span>

> <span data-ttu-id="9a613-108">Este tópico e aplicativo mostram como adicionar um auxiliar do Twitter ao seu projeto do WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="9a613-108">This topic and application show how to add a Twitter Helper to your WebMatrix 3 project.</span></span> <span data-ttu-id="9a613-109">Ele contém o código auxiliar do Twitter e mostra como chamar os métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="9a613-109">It contains the Twitter Helper code and shows how to call the helper methods.</span></span>
> 
> <span data-ttu-id="9a613-110">Esse código para o arquivo Twitter. cshtml foi desenvolvido pela **Tian Pan** da Microsoft.</span><span class="sxs-lookup"><span data-stu-id="9a613-110">This code for the Twitter.cshtml file was developed by **Tian Pan** of Microsoft.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="9a613-111">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="9a613-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="9a613-112">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="9a613-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="9a613-113">Este tutorial também funciona com o Páginas da Web do ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="9a613-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="introduction"></a><span data-ttu-id="9a613-114">Introdução</span><span class="sxs-lookup"><span data-stu-id="9a613-114">Introduction</span></span>

<span data-ttu-id="9a613-115">Este tópico demonstra como adicionar um auxiliar do Twitter ao seu aplicativo e usar sintaxe Razor para chamar os métodos auxiliares.</span><span class="sxs-lookup"><span data-stu-id="9a613-115">This topic demonstrates how to add a Twitter Helper to your application and use Razor syntax to call the helper methods.</span></span> <span data-ttu-id="9a613-116">O auxiliar do Twitter facilita a incorporação de botões e widgets do Twitter em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="9a613-116">The Twitter Helper makes it easy to incorporate Twitter buttons and widgets in your application.</span></span> <span data-ttu-id="9a613-117">Para usar um widget do Twitter, como a linha do tempo de um usuário ou os resultados da pesquisa para uma hashtag, primeiro você deve criar o [widget no Twitter](https://twitter.com/settings/widgets).</span><span class="sxs-lookup"><span data-stu-id="9a613-117">To use a Twitter widget, such as a user's timeline or the search results for a hashtag, you must first create the [widget on Twitter](https://twitter.com/settings/widgets).</span></span> <span data-ttu-id="9a613-118">Depois de criar o widget, você receberá uma ID de widget. Você passa essa ID de widget como um parâmetro ao chamar os métodos auxiliares que mostram widget.</span><span class="sxs-lookup"><span data-stu-id="9a613-118">After creating your widget, you will receive a widget id. You pass this widget id as a parameter when calling the helper methods that show widget.</span></span>

<span data-ttu-id="9a613-119">Este tópico foi escrito para a versão 1,1 da API do Twitter.</span><span class="sxs-lookup"><span data-stu-id="9a613-119">This topic was written for version 1.1 of the Twitter API.</span></span> <span data-ttu-id="9a613-120">Ao adicionar diretamente o código auxiliar do Twitter ao seu projeto, você poderá atualizar o código auxiliar se a API do Twitter for alterada.</span><span class="sxs-lookup"><span data-stu-id="9a613-120">By directly adding the Twitter Helper code to your project, you can update the helper code if the Twitter API changes.</span></span>

<span data-ttu-id="9a613-121">Para obter informações sobre como instalar o WebMatrix, consulte [introducing páginas da Web do ASP.NET 2-introdução](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="9a613-121">For information about installing WebMatrix, see [Introducing ASP.NET Web Pages 2 - Getting Started](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).</span></span>

## <a name="add-twitter-helper-to-your-project"></a><span data-ttu-id="9a613-122">Adicionar o auxiliar do Twitter ao seu projeto</span><span class="sxs-lookup"><span data-stu-id="9a613-122">Add Twitter Helper to your project</span></span>

<span data-ttu-id="9a613-123">Para adicionar o auxiliar do Twitter, primeiro adicione uma pasta chamada **App\_Code** ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="9a613-123">To add the Twitter Helper, first, add a folder named **App\_Code** to your project.</span></span> <span data-ttu-id="9a613-124">Em seguida, crie um arquivo chamado **Twitter. cshtml**.</span><span class="sxs-lookup"><span data-stu-id="9a613-124">Then, create a file named **Twitter.cshtml**.</span></span>

![App_Code pasta](twitter-helper/_static/image1.png)

<span data-ttu-id="9a613-126">Substitua o código padrão no Twitter. cshtml pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="9a613-126">Replace the default code in Twitter.cshtml with the following code.</span></span>

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a><span data-ttu-id="9a613-127">Chamar métodos do Twitter de suas páginas da Web</span><span class="sxs-lookup"><span data-stu-id="9a613-127">Call Twitter methods from your web pages</span></span>

<span data-ttu-id="9a613-128">O exemplo a seguir mostra como usar os métodos auxiliares do Twitter de uma página em seu projeto.</span><span class="sxs-lookup"><span data-stu-id="9a613-128">The following example shows how to use the Twitter Helper methods from a page in your project.</span></span> <span data-ttu-id="9a613-129">Em seu projeto, você desejará substituir os valores de parâmetro por valores relevantes às suas necessidades.</span><span class="sxs-lookup"><span data-stu-id="9a613-129">In your project, you will want to replace the parameter values with values that are relevant to your needs.</span></span> <span data-ttu-id="9a613-130">Você pode usar as IDs de widget fornecidas para explorar como os métodos funcionam, mas você vai querer gerar seus próprios widgets para seu projeto.</span><span class="sxs-lookup"><span data-stu-id="9a613-130">You can use the provided widget ids to explore how the methods work, but you will want to generate your own widgets for your project.</span></span>

<span data-ttu-id="9a613-131">Nem todos os parâmetros mostrados abaixo são obrigatórios.</span><span class="sxs-lookup"><span data-stu-id="9a613-131">Not all of the parameters shown below are required.</span></span> <span data-ttu-id="9a613-132">Os parâmetros opcionais são usados para personalizar como o botão ou widget é exibido.</span><span class="sxs-lookup"><span data-stu-id="9a613-132">The optional parameters are used to customize how the button or widget is displayed.</span></span> <span data-ttu-id="9a613-133">Por exemplo, o botão seguir requer apenas o nome de usuário a seguir, mas o exemplo mostra como incluir o número de seguidores e como especificar o tamanho do botão e o idioma.</span><span class="sxs-lookup"><span data-stu-id="9a613-133">For example, the Follow Button only requires the user name to follow, but the example shows how to include the number of followers, and how specify the size of the button and the language.</span></span>

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a><span data-ttu-id="9a613-134">Confira os resultados</span><span class="sxs-lookup"><span data-stu-id="9a613-134">See the results</span></span>

<span data-ttu-id="9a613-135">O código acima produz os botões e widgets a seguir.</span><span class="sxs-lookup"><span data-stu-id="9a613-135">The above code produces the following buttons and widgets.</span></span> <span data-ttu-id="9a613-136">Esses botões e widgets são totalmente funcionais, e não capturas de tela.</span><span class="sxs-lookup"><span data-stu-id="9a613-136">These buttons and widgets are fully-functional, not screenshots.</span></span> <span data-ttu-id="9a613-137">O botão seguir é exibido em espanhol porque o parâmetro de idioma foi definido como **es**.</span><span class="sxs-lookup"><span data-stu-id="9a613-137">The Follow Button is displayed in Spanish because the language parameter was set to **es**.</span></span>

### <a name="follow-button"></a><span data-ttu-id="9a613-138">Botão seguir</span><span class="sxs-lookup"><span data-stu-id="9a613-138">Follow Button</span></span>

<span data-ttu-id="9a613-139">[Siga @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span><span class="sxs-lookup"><span data-stu-id="9a613-139">[Follow @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="tweet-button"></a><span data-ttu-id="9a613-140">Botão tweet</span><span class="sxs-lookup"><span data-stu-id="9a613-140">Tweet Button</span></span>

<span data-ttu-id="9a613-141">`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>` de [tweet](https://twitter.com/share)</span><span class="sxs-lookup"><span data-stu-id="9a613-141">[Tweet](https://twitter.com/share)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`</span></span>

### <a name="user-timeline-profile"></a><span data-ttu-id="9a613-142">Linha do tempo do usuário (perfil)</span><span class="sxs-lookup"><span data-stu-id="9a613-142">User Timeline (Profile)</span></span>

<span data-ttu-id="9a613-143">[Tweets por @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="9a613-143">[Tweets by @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="favorites"></a><span data-ttu-id="9a613-144">Favoritos</span><span class="sxs-lookup"><span data-stu-id="9a613-144">Favorites</span></span>

<span data-ttu-id="9a613-145">[Tweets favoritos por @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="9a613-145">[Favorite Tweets by @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="list"></a><span data-ttu-id="9a613-146">Lista</span><span class="sxs-lookup"><span data-stu-id="9a613-146">List</span></span>

<span data-ttu-id="9a613-147">[Tweets de @Microsoft/MS\_faixas de\_do consumidor](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="9a613-147">[Tweets from @Microsoft/MS\_Consumer\_Bands](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>

### <a name="search"></a><span data-ttu-id="9a613-148">Search</span><span class="sxs-lookup"><span data-stu-id="9a613-148">Search</span></span>

<span data-ttu-id="9a613-149">[Tweets sobre &quot;#asp .net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span><span class="sxs-lookup"><span data-stu-id="9a613-149">[Tweets about &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`</span></span>
