---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Como faço para:] Use a propriedade resposta. Filter para substituir o HTML em uma página do ASP.NET | Microsoft Docs'
author: rick-anderson
description: Neste vídeo, Chris pixels mostra como usar a propriedade resposta. Filter para interceptar e alterar o HTML que está sendo enviado para uma página. Primeiro, uma página de exemplo é criada w...
ms.author: riande
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 2ebd9162f81f5270c92c6b8d55e2d2dad4660701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78602812"
---
# <a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="45280-104">[Como faço para:] Usar a propriedade resposta. Filter para substituir o HTML em uma página ASP.NET</span><span class="sxs-lookup"><span data-stu-id="45280-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>

<span data-ttu-id="45280-105">por [Chris pixels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="45280-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="45280-106">Neste vídeo, Chris pixels mostra como usar a propriedade resposta. Filter para interceptar e alterar o HTML que está sendo enviado para uma página.</span><span class="sxs-lookup"><span data-stu-id="45280-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="45280-107">Primeiro, uma página de exemplo é criada com um texto simples.</span><span class="sxs-lookup"><span data-stu-id="45280-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="45280-108">Em seguida, é criada uma classe de fluxo personalizada que serve como o fluxo de substituição para o fluxo atual que está sendo enviado ao navegador do usuário.</span><span class="sxs-lookup"><span data-stu-id="45280-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="45280-109">Nessa classe de fluxo personalizada, o conteúdo da página é recuperado do fluxo, alterado e, em seguida, escrito para o fluxo de resposta.</span><span class="sxs-lookup"><span data-stu-id="45280-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="45280-110">Nessa classe de fluxo personalizada, o método Write é personalizado para substituir o HTML no fluxo de resposta base, alterando assim o que é enviado ao navegador do usuário.</span><span class="sxs-lookup"><span data-stu-id="45280-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="45280-111">Por fim, a nova classe Stream é atribuída à propriedade Response. Filter na página\_evento Load, assim, fornecendo o mecanismo para alterar o conteúdo da página.</span><span class="sxs-lookup"><span data-stu-id="45280-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="45280-112">&#9654;Assistir ao vídeo (13 minutos)</span><span class="sxs-lookup"><span data-stu-id="45280-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
