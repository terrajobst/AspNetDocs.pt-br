---
uid: web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
title: '[Como faço para:] Escolher entre métodos de atualizações de página AJAX? | Microsoft Docs'
author: JoeStagner
description: Neste vídeo, Joe Stagner compara os dois principais métodos de execução de atualizações de página de estilo AJAX em um aplicativo ASP.NET. O primeiro método é usar um UPD...
ms.author: riande
ms.date: 07/09/2007
ms.assetid: a5e33a7d-ccb2-483f-a955-3d39f72ba4ec
msc.legacyurl: /web-forms/videos/aspnet-ajax/how-do-i-choose-between-methods-of-ajax-page-updates
msc.type: video
ms.openlocfilehash: 56e3ebfbe0b5af4234791136725de79e38171cc1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78523663"
---
# <a name="how-do-i-choose-between-methods-of-ajax-page-updates"></a><span data-ttu-id="3569c-105">[Como faço para:] Escolher entre métodos de atualizações de página AJAX?</span><span class="sxs-lookup"><span data-stu-id="3569c-105">[How Do I:] Choose Between Methods of AJAX Page Updates?</span></span>

<span data-ttu-id="3569c-106">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="3569c-106">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

<span data-ttu-id="3569c-107">Neste vídeo, Joe Stagner compara os dois principais métodos de execução de atualizações de página de estilo AJAX em um aplicativo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3569c-107">In this video Joe Stagner compares the two primary methods of performing AJAX-style page updates in an ASP.NET application.</span></span> <span data-ttu-id="3569c-108">O primeiro método é usar um UpdatePanel, em que nenhum código adicional precisa ser escrito no lado do cliente ou no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="3569c-108">The first method is to use an UpdatePanel, where no additional code needs to be written on either the client side or the server side.</span></span> <span data-ttu-id="3569c-109">O benefício de usar o UpdatePanel é que tudo funciona automaticamente.</span><span class="sxs-lookup"><span data-stu-id="3569c-109">The benefit of using the UpdatePanel is that everything works automatically.</span></span> <span data-ttu-id="3569c-110">A penalidade é que, no cliente, ele requer muitos dados a serem incluídos na solicitação e resposta do AJAX e, no servidor, ele requer que um ciclo de vida de página inteira seja executado.</span><span class="sxs-lookup"><span data-stu-id="3569c-110">The penalty is that at the client it requires a lot of data to be included in the AJAX request and response, and at the server it requires a full page lifecycle to be executed.</span></span> <span data-ttu-id="3569c-111">O segundo método é usar retornos de chamada de rede, em que o código adicional precisa ser escrito tanto no lado do cliente quanto no lado do servidor.</span><span class="sxs-lookup"><span data-stu-id="3569c-111">The second method is to use network callbacks, where additional code needs to be written on both the client side and the server side.</span></span> <span data-ttu-id="3569c-112">O benefício de usar retornos de chamada de rede é que, no cliente, ele exige que poucos dados sejam incluídos na solicitação e resposta do AJAX e no servidor ele exige que apenas o método de serviço chamado seja executado.</span><span class="sxs-lookup"><span data-stu-id="3569c-112">The benefit of using network callbacks is that at the client it requires very little data to be included in the AJAX request and response, and at the server it requires only the called service method to be executed.</span></span> <span data-ttu-id="3569c-113">O penality é o tempo e o esforço necessário para escrever o código necessário.</span><span class="sxs-lookup"><span data-stu-id="3569c-113">The penality is the time and effort it takes to write the necessary code.</span></span> <span data-ttu-id="3569c-114">Joe conclui o vídeo discutindo o que você deve considerar ao escolher entre os dois métodos principais de atualizações de página de estilo AJAX.</span><span class="sxs-lookup"><span data-stu-id="3569c-114">Joe concludes the video by discussing what you should consider when choosing between the two primary methods of AJAX-style page updates.</span></span> <span data-ttu-id="3569c-115">(Este vídeo usa o código da [introdução ao vídeo do ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) e [como fazer retornos de chamada de rede do lado do cliente com](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) o vídeo do ASP.NET AJAX.)</span><span class="sxs-lookup"><span data-stu-id="3569c-115">(This video uses the code from the [How Do I Get Started with ASP.NET AJAX](how-do-i-get-started-with-aspnet-ajax.md) video and the [How Do I Make Client-Side Network Callbacks with ASP.NET AJAX](how-do-i-make-client-side-network-callbacks-with-aspnet-ajax.md) video.)</span></span>

[<span data-ttu-id="3569c-116">&#9654;Assistir ao vídeo (11 minutos)</span><span class="sxs-lookup"><span data-stu-id="3569c-116">&#9654; Watch video (11 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-choose-between-methods-of-ajax-page-updates)

> [!div class="step-by-step"]
> <span data-ttu-id="3569c-117">[Anterior](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
> [Próximo](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span><span class="sxs-lookup"><span data-stu-id="3569c-117">[Previous](how-do-i-update-multiple-regions-of-a-page-with-aspnet-ajax.md)
[Next](how-do-i-use-other-javascript-user-interface-libraries-with-aspnet-ajax.md)</span></span>
