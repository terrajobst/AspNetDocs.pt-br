---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Agrupamento e Minificação de ativos em uma Web do ASP.NET (Razor) sites de páginas | Microsoft Docs
author: microsoft
description: Agrupamento e minificação são maneiras de tornar o seu site com mais rapidez. Agrupamento permite que você combine vários arquivos JavaScript (. js) ou vários folha estilos em cascata (...
ms.author: riande
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 7d2cb2fe311b8aff20bfb378b329286701ed5b7d
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57059913"
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="4f72a-104">Agrupamento e minificação de ativos em um site de Páginas da Web do ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="4f72a-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="4f72a-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="4f72a-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="4f72a-106">Agrupamento e minificação são maneiras de tornar o seu site com mais rapidez.</span><span class="sxs-lookup"><span data-stu-id="4f72a-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="4f72a-107">Agrupamento permite que você combine vários JavaScript (*. js*) arquivos ou vários folha de estilos em cascata (*. CSS*) arquivos para que eles podem ser baixados como uma unidade, em vez de um de cada vez.</span><span class="sxs-lookup"><span data-stu-id="4f72a-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="4f72a-108">Minimização comprime-out de espaço em branco e executa outros tipos de compactação para tornar os arquivos baixados tão pequeno uma possível.</span><span class="sxs-lookup"><span data-stu-id="4f72a-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="4f72a-109">A versão RC do ASP.NET Web Pages 2 não oferece suporte a agrupamento e minificação porque o pacote que contém os elementos necessários ainda não está disponível no Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4f72a-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="4f72a-110">Pedimos desculpas pelo inconveniente.</span><span class="sxs-lookup"><span data-stu-id="4f72a-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="4f72a-111">O pacote deve estar disponível na versão final do ASP.NET Web Pages 2 e o WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="4f72a-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
