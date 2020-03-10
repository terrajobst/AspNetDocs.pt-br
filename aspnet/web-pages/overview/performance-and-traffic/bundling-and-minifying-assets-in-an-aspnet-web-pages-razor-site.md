---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: Agrupamento e Minificar ativos em um site Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: microsoft
description: Agrupamento e minificação são maneiras de tornar seu site mais rápido. O agrupamento permite combinar vários arquivos JavaScript (. js) ou várias folhas de estilo em cascata (...
ms.author: riande
ms.date: 06/21/2012
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: 5e42111ad71ec65581e56c73822e23ecd5fcbd58
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78635705"
---
# <a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="35a60-104">Agrupamento e minificação de ativos em um site de Páginas da Web do ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="35a60-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="35a60-105">pela [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="35a60-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="35a60-106">Agrupamento e minificação são maneiras de tornar seu site mais rápido.</span><span class="sxs-lookup"><span data-stu-id="35a60-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="35a60-107">O agrupamento permite combinar vários arquivos JavaScript ( *. js*) ou vários arquivos de folha de estilo em cascata ( *. css*) para que eles possam ser baixados como uma unidade, em vez de um de cada vez.</span><span class="sxs-lookup"><span data-stu-id="35a60-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="35a60-108">O minificação compacta o espaço em branco e executa outros tipos de compactação para tornar os arquivos baixados o mais pequenos possível.</span><span class="sxs-lookup"><span data-stu-id="35a60-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="35a60-109">A versão RC do Páginas da Web do ASP.NET 2 não oferece suporte a agrupamento e minificação porque o pacote que contém os elementos necessários ainda não está disponível no Microsoft WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="35a60-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="35a60-110">Pedimos desculpas por esta inconveniência.</span><span class="sxs-lookup"><span data-stu-id="35a60-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="35a60-111">Espera-se que o pacote esteja disponível na versão final do Páginas da Web do ASP.NET 2 e do WebMatrix 2.</span><span class="sxs-lookup"><span data-stu-id="35a60-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
