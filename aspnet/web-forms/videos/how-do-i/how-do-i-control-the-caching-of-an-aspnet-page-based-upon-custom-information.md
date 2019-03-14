---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Como fazer:] O armazenamento em cache de uma página ASP.NET com base em informações personalizadas de controle | Microsoft Docs'
author: rick-anderson
description: Neste vídeo, Chris Pels mostra como controlar os critérios para uma página ASP.NET com base em informações personalizadas de cache. Uma página de exemplo é criada e, em seguida, ão....
ms.author: riande
ms.date: 02/19/2009
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: a9ed2baad3460441bc57d97bf74f6de5977db0c9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032223"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="ea493-104">[Como fazer:] O armazenamento em cache de uma página ASP.NET com base em informações personalizadas de controle</span><span class="sxs-lookup"><span data-stu-id="ea493-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="ea493-105">por [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="ea493-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="ea493-106">Neste vídeo, Chris Pels mostra como controlar os critérios para uma página ASP.NET com base em informações personalizadas de cache.</span><span class="sxs-lookup"><span data-stu-id="ea493-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="ea493-107">Uma página de exemplo é criada e, em seguida, a diretiva OutputCache é usada com o atributo VaryByCustom que contém um valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="ea493-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="ea493-108">Em seguida, o método GetVaryCustomByString() é substituído no módulo global. asax que fornece a manipulação do atributo personalizado.</span><span class="sxs-lookup"><span data-stu-id="ea493-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="ea493-109">Nesse método, uma cadeia de caracteres é retornada que identifica exclusivamente a versão em cache da página.</span><span class="sxs-lookup"><span data-stu-id="ea493-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="ea493-110">Por fim, há uma discussão sobre como cache usando um valor personalizado pode ser usado de várias maneiras para um site da web.</span><span class="sxs-lookup"><span data-stu-id="ea493-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="ea493-111">&#9654;Assista ao vídeo (12 minutos)</span><span class="sxs-lookup"><span data-stu-id="ea493-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
