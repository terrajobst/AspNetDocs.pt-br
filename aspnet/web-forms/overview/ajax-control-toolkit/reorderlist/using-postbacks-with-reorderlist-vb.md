---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Usando postbacks com reordenalist (VB) | Microsoft Docs
author: wenz
description: O controle reorganizelist no AJAX Control Toolkit fornece uma lista que pode ser reordenada pelo usuário por meio de arrastar e soltar. Sempre que a lista é reordenada, um po...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 5d6075e40df2c32df6c0d801243eff98fa7790b2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611362"
---
# <a name="using-postbacks-with-reorderlist-vb"></a><span data-ttu-id="5b2b4-104">Uso de postbacks com ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="5b2b4-104">Using Postbacks with ReorderList (VB)</span></span>

<span data-ttu-id="5b2b4-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5b2b4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5b2b4-106">[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5b2b4-106">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span></span>

> <span data-ttu-id="5b2b4-107">O controle reorganizelist no AJAX Control Toolkit fornece uma lista que pode ser reordenada pelo usuário por meio de arrastar e soltar.</span><span class="sxs-lookup"><span data-stu-id="5b2b4-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="5b2b4-108">Sempre que a lista for reordenada, um postback deverá informar o servidor da alteração.</span><span class="sxs-lookup"><span data-stu-id="5b2b4-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="overview"></a><span data-ttu-id="5b2b4-109">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="5b2b4-109">Overview</span></span>

<span data-ttu-id="5b2b4-110">O controle de `ReorderList` no AJAX Control Toolkit fornece uma lista que pode ser reordenada pelo usuário por meio de arrastar e soltar.</span><span class="sxs-lookup"><span data-stu-id="5b2b4-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="5b2b4-111">Sempre que a lista for reordenada, um postback deverá informar o servidor da alteração.</span><span class="sxs-lookup"><span data-stu-id="5b2b4-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="5b2b4-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="5b2b4-112">Steps</span></span>

<span data-ttu-id="5b2b4-113">Há várias fontes de dados possíveis para o controle de `ReorderList`.</span><span class="sxs-lookup"><span data-stu-id="5b2b4-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="5b2b4-114">Uma é usar um controle de `XmlDataSource`:</span><span class="sxs-lookup"><span data-stu-id="5b2b4-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="5b2b4-115">Para associar esse XML a um controle de `ReorderList` e habilitar postbacks, os seguintes atributos devem ser definidos:</span><span class="sxs-lookup"><span data-stu-id="5b2b4-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="5b2b4-116">`DataSourceID`: a ID da fonte de dados</span><span class="sxs-lookup"><span data-stu-id="5b2b4-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="5b2b4-117">`SortOrderField`: a propriedade a ser classificada por</span><span class="sxs-lookup"><span data-stu-id="5b2b4-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="5b2b4-118">`AllowReorder`: se deseja permitir que o usuário reordene os elementos da lista</span><span class="sxs-lookup"><span data-stu-id="5b2b4-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="5b2b4-119">`PostBackOnReorder`: se deseja criar um postback sempre que a lista é reorganizada</span><span class="sxs-lookup"><span data-stu-id="5b2b4-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="5b2b4-120">Aqui está a marcação apropriada para o controle:</span><span class="sxs-lookup"><span data-stu-id="5b2b4-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="5b2b4-121">Dentro do controle de `ReorderList`, dados específicos da fonte de dados podem ser associados usando o método `Eval()`:</span><span class="sxs-lookup"><span data-stu-id="5b2b4-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

<span data-ttu-id="5b2b4-122">Em uma posição arbitrária na página, um rótulo manterá as informações quando a última reordenação ocorrer:</span><span class="sxs-lookup"><span data-stu-id="5b2b4-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="5b2b4-123">Este rótulo é preenchido com texto no código do lado do servidor, tratando o postback:</span><span class="sxs-lookup"><span data-stu-id="5b2b4-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

<span data-ttu-id="5b2b4-124">Por fim, para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado na página:</span><span class="sxs-lookup"><span data-stu-id="5b2b4-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]

<span data-ttu-id="5b2b4-125">[![cada reordenação dispara um postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5b2b4-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="5b2b4-126">Cada reordenação dispara um postback ([clique para exibir a imagem em tamanho normal](using-postbacks-with-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5b2b4-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5b2b4-127">[Anterior](drag-and-drop-via-reorderlist-cs.md)
> [Próximo](drag-and-drop-via-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="5b2b4-127">[Previous](drag-and-drop-via-reorderlist-cs.md)
[Next](drag-and-drop-via-reorderlist-vb.md)</span></span>
