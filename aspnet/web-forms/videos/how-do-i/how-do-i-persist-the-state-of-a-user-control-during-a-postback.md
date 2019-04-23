---
uid: web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
title: '[How Do I]: Persist the State of a User Control During a Postback | Microsoft Docs'
author: rick-anderson
description: Neste vídeo, Chris Pels mostra como persistir o estado de um ou mais objetos em um controle de usuário. Primeiro, um controle de usuário é criado que representa o abilit...
ms.author: riande
ms.date: 04/02/2009
ms.assetid: d1bca4c6-838c-40f7-87ec-80bb67e483e5
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-persist-the-state-of-a-user-control-during-a-postback
msc.type: video
ms.openlocfilehash: c87bd6c5c993a1bde8f8a84f6d53b431e54541d9
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59414771"
---
# <a name="how-do-i-persist-the-state-of-a-user-control-during-a-postback"></a>[How Do I]: Manter o estado de um controle de usuário durante um postback

<span data-ttu-id="e22fa-104">por [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="e22fa-104">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="e22fa-105">Neste vídeo, Chris Pels mostra como persistir o estado de um ou mais objetos em um controle de usuário.</span><span class="sxs-lookup"><span data-stu-id="e22fa-105">In this video Chris Pels shows how to persist the state of one or more objects in a user control.</span></span> <span data-ttu-id="e22fa-106">Primeiro, um controle de usuário é criado que representa a capacidade de um usuário especificar critérios de filtragem para uma pesquisa.</span><span class="sxs-lookup"><span data-stu-id="e22fa-106">First, a user control is created that represents the ability for a user to specify filter criteria for a search.</span></span> <span data-ttu-id="e22fa-107">Além disso, um complemento de classe de filtro é criado para armazenar as informações de filtro.</span><span class="sxs-lookup"><span data-stu-id="e22fa-107">In addition, a companion Filter class is created to store the filter information.</span></span> <span data-ttu-id="e22fa-108">Vários elementos de interface do usuário são adicionados ao controle de filtro, juntamente com alguns métodos e propriedades para armazenar as informações de filtro atual na instância da classe de filtro.</span><span class="sxs-lookup"><span data-stu-id="e22fa-108">Several user interface elements are added to the filter control along with some methods and properties to store the current filter information in the Filter class instance.</span></span> <span data-ttu-id="e22fa-109">Em seguida, a persistência de controle de usuário é implementada usando o método RegisterRequiresControlState e métodos de salvar/restaurar associados.</span><span class="sxs-lookup"><span data-stu-id="e22fa-109">Next, the user control persistence is implemented using the RegisterRequiresControlState method and associated Save/Restore methods.</span></span> <span data-ttu-id="e22fa-110">Esses métodos armazenam a instância da classe de filtro e seus dados durante postbacks de página.</span><span class="sxs-lookup"><span data-stu-id="e22fa-110">These methods store the instance of the filter class and its data during page postbacks.</span></span> <span data-ttu-id="e22fa-111">Por fim, há uma discussão de como armazenar vários objetos na implementação de estado do controle.</span><span class="sxs-lookup"><span data-stu-id="e22fa-111">Finally, there is a discussion of how to store multiple objects in control state implementation.</span></span>

[<span data-ttu-id="e22fa-112">&#9654;Assista ao vídeo (23 minutos)</span><span class="sxs-lookup"><span data-stu-id="e22fa-112">&#9654; Watch video (23 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-persist-the-state-of-a-user-control-during-a-postback)
