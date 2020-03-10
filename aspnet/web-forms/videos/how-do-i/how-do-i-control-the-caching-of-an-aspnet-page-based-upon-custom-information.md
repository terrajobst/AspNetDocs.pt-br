---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[Como faço para:] Controlar o cache de uma página ASP.NET com base nas informações personalizadas | Microsoft Docs'
author: rick-anderson
description: Neste vídeo, Chris pixels mostra como controlar os critérios para armazenar em cache uma página do ASP.NET com base nas informações personalizadas. Uma página de exemplo é criada e, em seguida, a...
ms.author: riande
ms.date: 02/19/2009
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: 676bc8f234a6e517104d07fd58a0ff14aec73e69
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624729"
---
# <a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="0aadf-104">[Como faço para:] Controlar o cache de uma página do ASP.NET com base nas informações personalizadas</span><span class="sxs-lookup"><span data-stu-id="0aadf-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>

<span data-ttu-id="0aadf-105">por [Chris pixels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="0aadf-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="0aadf-106">Neste vídeo, Chris pixels mostra como controlar os critérios para armazenar em cache uma página do ASP.NET com base nas informações personalizadas.</span><span class="sxs-lookup"><span data-stu-id="0aadf-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="0aadf-107">Uma página de exemplo é criada e, em seguida, a diretiva OutputCache é usada com o atributo VaryByCustom que contém um valor personalizado.</span><span class="sxs-lookup"><span data-stu-id="0aadf-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="0aadf-108">Em seguida, o método GetVaryCustomByString () é substituído no módulo global. asax, que fornece a manipulação do atributo personalizado.</span><span class="sxs-lookup"><span data-stu-id="0aadf-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="0aadf-109">Nesse método, é retornada uma cadeia de caracteres que identifica exclusivamente a versão armazenada em cache da página.</span><span class="sxs-lookup"><span data-stu-id="0aadf-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="0aadf-110">Por fim, há uma discussão sobre como o armazenamento em cache usando um valor personalizado pode ser usado de várias maneiras para um site da Web.</span><span class="sxs-lookup"><span data-stu-id="0aadf-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="0aadf-111">&#9654;Assistir ao vídeo (12 minutos)</span><span class="sxs-lookup"><span data-stu-id="0aadf-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
