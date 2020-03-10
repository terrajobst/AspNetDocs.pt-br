---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Criando URLs legíveis em sites Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este artigo descreve o roteamento em um site Páginas da Web do ASP.NET (Razor) e como isso permite que você use URLs que são mais legíveis e melhores para SEO. O que você vai...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 832db8e144cab730f16c78f67c12feb9b7c92c7c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628390"
---
# <a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="7f8a0-104">Criando URLs legíveis em sites Páginas da Web do ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="7f8a0-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>

<span data-ttu-id="7f8a0-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7f8a0-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7f8a0-106">Este artigo descreve o roteamento em um site Páginas da Web do ASP.NET (Razor) e como isso permite que você use URLs que são mais legíveis e melhores para SEO.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="7f8a0-107">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="7f8a0-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="7f8a0-108">Como o ASP.NET usa o roteamento para permitir que você use URLs mais legíveis e pesquisáveis.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7f8a0-109">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="7f8a0-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="7f8a0-110">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="7f8a0-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="7f8a0-111">Este tutorial também funciona com o Páginas da Web do ASP.NET 2.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>

## <a name="about-routing"></a><span data-ttu-id="7f8a0-112">Sobre roteamento</span><span class="sxs-lookup"><span data-stu-id="7f8a0-112">About Routing</span></span>

<span data-ttu-id="7f8a0-113">As URLs para as páginas em seu site podem ter um impacto sobre o quão bem o site funciona.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="7f8a0-114">Uma URL &quot;&quot; amigável pode tornar mais fácil para as pessoas usarem o site.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="7f8a0-115">Ele também pode ajudar com a SEO (otimização do mecanismo de pesquisa) para o site.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="7f8a0-116">Os sites ASP.NET incluem a capacidade de usar URLs amigáveis automaticamente.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="7f8a0-117">O ASP.NET permite que você crie URLs significativas que descrevem as ações do usuário em vez de apenas apontar para um arquivo no servidor.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="7f8a0-118">Considere estas URLs para um blog fictício:</span><span class="sxs-lookup"><span data-stu-id="7f8a0-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="7f8a0-119">Compare essas URLs com as seguintes:</span><span class="sxs-lookup"><span data-stu-id="7f8a0-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="7f8a0-120">No primeiro par, um usuário precisa saber que o blog é exibido usando a página *blog. cshtml* e, em seguida, teria que construir uma cadeia de caracteres de consulta que obtenha a categoria ou o intervalo de datas corretos.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="7f8a0-121">O segundo conjunto de exemplos é muito mais fácil de compreender e criar.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="7f8a0-122">As URLs para o primeiro exemplo também apontam diretamente para um arquivo específico (*blog. cshtml*).</span><span class="sxs-lookup"><span data-stu-id="7f8a0-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="7f8a0-123">Se, por algum motivo, o blog fosse movido para outra pasta no servidor, ou se o blog tiver sido reescrito para usar uma página diferente, os links ficarão errados.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="7f8a0-124">O segundo conjunto de URLs não aponta para uma página específica, portanto, mesmo que a implementação do blog ou o local mudem, as URLs ainda seriam válidas.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="7f8a0-125">No Páginas da Web do ASP.NET, você pode criar URLs mais amigáveis, como aquelas nos exemplos acima, porque ASP.NET usa o *Roteamento*.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="7f8a0-126">O roteamento cria um mapeamento lógico de uma URL para uma página (ou páginas) que pode atender à solicitação.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="7f8a0-127">Como o mapeamento é lógico (não físico, para um arquivo específico), o roteamento fornece grande flexibilidade para definir as URLs para seu site.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="7f8a0-128">Como funciona o roteamento</span><span class="sxs-lookup"><span data-stu-id="7f8a0-128">How Routing Works</span></span>

<span data-ttu-id="7f8a0-129">Quando o ASP.NET processa uma solicitação, ele lê a URL para determinar como roteá-la.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="7f8a0-130">O ASP.NET tenta corresponder segmentos individuais da URL para arquivos no disco, indo da esquerda para a direita.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="7f8a0-131">Se houver uma correspondência, qualquer coisa restante na URL será passada para a página como *informações de caminho*.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="7f8a0-132">Imagine que alguém faça uma solicitação usando esta URL:</span><span class="sxs-lookup"><span data-stu-id="7f8a0-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="7f8a0-133">A pesquisa é assim:</span><span class="sxs-lookup"><span data-stu-id="7f8a0-133">The search goes like this:</span></span>

1. <span data-ttu-id="7f8a0-134">Há um arquivo com o caminho e o nome de */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="7f8a0-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="7f8a0-135">Nesse caso, execute essa página e passe nenhuma informação para ela.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="7f8a0-136">Caso contrário...</span><span class="sxs-lookup"><span data-stu-id="7f8a0-136">Otherwise ...</span></span>
2. <span data-ttu-id="7f8a0-137">Há um arquivo com o caminho e o nome de */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="7f8a0-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="7f8a0-138">Nesse caso, execute essa página e passe o valor `c` a ela.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="7f8a0-139">Caso contrário...</span><span class="sxs-lookup"><span data-stu-id="7f8a0-139">Otherwise …</span></span>
3. <span data-ttu-id="7f8a0-140">Há um arquivo com o caminho e o nome de */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="7f8a0-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="7f8a0-141">Nesse caso, execute essa página e passe o valor `b/c` a ela.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="7f8a0-142">Se a pesquisa não encontrou correspondências exatas para arquivos *. cshtml* em suas pastas especificadas, o ASP.net continuará procurando esses arquivos por sua vez:</span><span class="sxs-lookup"><span data-stu-id="7f8a0-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="7f8a0-143">*/a/b/c/default.cshtml* (nenhuma informação de caminho).</span><span class="sxs-lookup"><span data-stu-id="7f8a0-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="7f8a0-144">*/a/b/c/index.cshtml* (nenhuma informação de caminho).</span><span class="sxs-lookup"><span data-stu-id="7f8a0-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="7f8a0-145">Para ser claro, as solicitações para páginas específicas (ou seja, solicitações que incluem a extensão de nome de arquivo *. cshtml* ) funcionam da mesma forma que esperado.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="7f8a0-146">Uma solicitação como `http://www.contoso.com/a/b.cshtml` executará a página *b. cshtml* , bem.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>

<span data-ttu-id="7f8a0-147">Dentro de uma página, você pode obter as informações de caminho por meio da propriedade `UrlData` da página, que é um dicionário.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="7f8a0-148">Imagine que você tenha um arquivo chamado *ViewCustomers. cshtml* e o site receba esta solicitação:</span><span class="sxs-lookup"><span data-stu-id="7f8a0-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="7f8a0-149">Conforme descrito nas regras acima, a solicitação vai para sua página.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="7f8a0-150">Dentro da página, você pode usar um código como o seguinte para obter e exibir as informações de caminho (nesse caso, o valor &quot;1000&quot;):</span><span class="sxs-lookup"><span data-stu-id="7f8a0-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="7f8a0-151">Como o roteamento não envolve nomes de arquivo completos, pode haver ambiguidade se você tiver páginas com o mesmo nome, mas extensões de nome de arquivo diferentes (por exemplo, *mypage. cshtml* e *mypage. html*).</span><span class="sxs-lookup"><span data-stu-id="7f8a0-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="7f8a0-152">Para evitar problemas com o roteamento, é melhor certificar-se de que você não tem páginas em seu site cujos nomes diferem apenas em sua extensão.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="7f8a0-153">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="7f8a0-153">Additional Resources</span></span>

<span data-ttu-id="7f8a0-154">[WebMatrix-URLs, UrlData e roteamento para SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="7f8a0-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="7f8a0-155">Esta entrada de blog de Mike Brind fornece alguns detalhes adicionais sobre como funciona o roteamento no Páginas da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7f8a0-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
