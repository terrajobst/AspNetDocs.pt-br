---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Usando postbacks com reordenalist (C#) | Microsoft Docs
author: wenz
description: O controle reorganizelist no AJAX Control Toolkit fornece uma lista que pode ser reordenada pelo usuário por meio de arrastar e soltar. Sempre que a lista é reordenada, um po...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: f83201fc6fd458e730b6bb5ffee184d303b52e90
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627228"
---
# <a name="using-postbacks-with-reorderlist-c"></a><span data-ttu-id="dcd6b-104">Uso de postbacks com ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="dcd6b-104">Using Postbacks with ReorderList (C#)</span></span>

<span data-ttu-id="dcd6b-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="dcd6b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="dcd6b-106">[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="dcd6b-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span></span>

> <span data-ttu-id="dcd6b-107">O controle reorganizelist no AJAX Control Toolkit fornece uma lista que pode ser reordenada pelo usuário por meio de arrastar e soltar.</span><span class="sxs-lookup"><span data-stu-id="dcd6b-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="dcd6b-108">Sempre que a lista for reordenada, um postback deverá informar o servidor da alteração.</span><span class="sxs-lookup"><span data-stu-id="dcd6b-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="overview"></a><span data-ttu-id="dcd6b-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="dcd6b-109">Overview</span></span>

<span data-ttu-id="dcd6b-110">O controle de `ReorderList` no AJAX Control Toolkit fornece uma lista que pode ser reordenada pelo usuário por meio de arrastar e soltar.</span><span class="sxs-lookup"><span data-stu-id="dcd6b-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="dcd6b-111">Sempre que a lista for reordenada, um postback deverá informar o servidor da alteração.</span><span class="sxs-lookup"><span data-stu-id="dcd6b-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="dcd6b-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="dcd6b-112">Steps</span></span>

<span data-ttu-id="dcd6b-113">Há várias fontes de dados possíveis para o controle de `ReorderList`.</span><span class="sxs-lookup"><span data-stu-id="dcd6b-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="dcd6b-114">Uma é usar um controle de `XmlDataSource`:</span><span class="sxs-lookup"><span data-stu-id="dcd6b-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="dcd6b-115">Para associar esse XML a um controle de `ReorderList` e habilitar postbacks, os seguintes atributos devem ser definidos:</span><span class="sxs-lookup"><span data-stu-id="dcd6b-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="dcd6b-116">`DataSourceID`: a ID da fonte de dados</span><span class="sxs-lookup"><span data-stu-id="dcd6b-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="dcd6b-117">`SortOrderField`: a propriedade a ser classificada por</span><span class="sxs-lookup"><span data-stu-id="dcd6b-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="dcd6b-118">`AllowReorder`: se deseja permitir que o usuário reordene os elementos da lista</span><span class="sxs-lookup"><span data-stu-id="dcd6b-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="dcd6b-119">`PostBackOnReorder`: se deseja criar um postback sempre que a lista é reorganizada</span><span class="sxs-lookup"><span data-stu-id="dcd6b-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="dcd6b-120">Aqui está a marcação apropriada para o controle:</span><span class="sxs-lookup"><span data-stu-id="dcd6b-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="dcd6b-121">Dentro do controle de `ReorderList`, dados específicos da fonte de dados podem ser associados usando o método `Eval()`:</span><span class="sxs-lookup"><span data-stu-id="dcd6b-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

<span data-ttu-id="dcd6b-122">Em uma posição arbitrária na página, um rótulo manterá as informações quando a última reordenação ocorrer:</span><span class="sxs-lookup"><span data-stu-id="dcd6b-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="dcd6b-123">Este rótulo é preenchido com texto no código do lado do servidor, tratando o postback:</span><span class="sxs-lookup"><span data-stu-id="dcd6b-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

<span data-ttu-id="dcd6b-124">Por fim, para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado na página:</span><span class="sxs-lookup"><span data-stu-id="dcd6b-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]

<span data-ttu-id="dcd6b-125">[![cada reordenação dispara um postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dcd6b-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="dcd6b-126">Cada reordenação dispara um postback ([clique para exibir a imagem em tamanho normal](using-postbacks-with-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="dcd6b-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="dcd6b-127">Próximo</span><span class="sxs-lookup"><span data-stu-id="dcd6b-127">Next</span></span>](drag-and-drop-via-reorderlist-cs.md)
