---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: Uso de Postbacks com ReorderList (c#) | Microsoft Docs
author: wenz
description: O controle ReorderList no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar. Sempre que a lista for reordenada, uma OC...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 6f8f74b74080104980e1db866d695fe7c6d9d5fc
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59393347"
---
# <a name="using-postbacks-with-reorderlist-c"></a><span data-ttu-id="19b16-104">Uso de postbacks com ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="19b16-104">Using Postbacks with ReorderList (C#)</span></span>

<span data-ttu-id="19b16-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="19b16-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="19b16-106">[Baixar o código](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) ou [baixar PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="19b16-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span></span>

> <span data-ttu-id="19b16-107">O controle ReorderList no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar.</span><span class="sxs-lookup"><span data-stu-id="19b16-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="19b16-108">Sempre que a lista for reordenada, deverá informar o servidor da alteração de um postback.</span><span class="sxs-lookup"><span data-stu-id="19b16-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="19b16-109">Visão geral</span><span class="sxs-lookup"><span data-stu-id="19b16-109">Overview</span></span>

<span data-ttu-id="19b16-110">O `ReorderList` controle no AJAX Control Toolkit fornece uma lista que pode ser reordenada por usuário por meio de arrastar e soltar.</span><span class="sxs-lookup"><span data-stu-id="19b16-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="19b16-111">Sempre que a lista for reordenada, deverá informar o servidor da alteração de um postback.</span><span class="sxs-lookup"><span data-stu-id="19b16-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="19b16-112">Etapas</span><span class="sxs-lookup"><span data-stu-id="19b16-112">Steps</span></span>

<span data-ttu-id="19b16-113">Há várias fontes de dados possíveis para o `ReorderList` controle.</span><span class="sxs-lookup"><span data-stu-id="19b16-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="19b16-114">Uma é usar um `XmlDataSource` controle:</span><span class="sxs-lookup"><span data-stu-id="19b16-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="19b16-115">Para associar esse XML para um `ReorderList` postbacks de controle e ativar os seguintes atributos devem ser definidas:</span><span class="sxs-lookup"><span data-stu-id="19b16-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- `DataSourceID`<span data-ttu-id="19b16-116">: A ID da fonte de dados</span><span class="sxs-lookup"><span data-stu-id="19b16-116">: The ID of the data source</span></span>
- `SortOrderField`<span data-ttu-id="19b16-117">: A propriedade de classificação</span><span class="sxs-lookup"><span data-stu-id="19b16-117">: The property to sort by</span></span>
- `AllowReorder`<span data-ttu-id="19b16-118">: Se deseja permitir que o usuário reordenar os elementos de lista</span><span class="sxs-lookup"><span data-stu-id="19b16-118">: Whether to allow the user to reorder the list elements</span></span>
- `PostBackOnReorder`<span data-ttu-id="19b16-119">: Se deseja criar uma nova postagem sempre que a lista é reorganizada</span><span class="sxs-lookup"><span data-stu-id="19b16-119">: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="19b16-120">Aqui está a marcação apropriada para o controle:</span><span class="sxs-lookup"><span data-stu-id="19b16-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="19b16-121">Dentro de `ReorderList` controle, dados específicos da fonte de dados pode ser associado usando o `Eval()` método:</span><span class="sxs-lookup"><span data-stu-id="19b16-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

<span data-ttu-id="19b16-122">Em uma posição arbitrária na página, um rótulo manterá as informações quando ocorreu a última reordenação:</span><span class="sxs-lookup"><span data-stu-id="19b16-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="19b16-123">Esse rótulo é preenchido com o texto no código do lado do servidor, manipular o postback:</span><span class="sxs-lookup"><span data-stu-id="19b16-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

<span data-ttu-id="19b16-124">Por fim, para ativar a funcionalidade do AJAX ASP.NET e o Kit de ferramentas de controle, o `ScriptManager` controle deve ser colocada na página:</span><span class="sxs-lookup"><span data-stu-id="19b16-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


[![E<span data-ttu-id="19b16-125">reordenação de ACH dispara um postback]</span><span class="sxs-lookup"><span data-stu-id="19b16-125">ach reordering triggers a postback]</span></span>(using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

<span data-ttu-id="19b16-126">Cada reordenação dispara um postback ([clique para exibir a imagem em tamanho normal](using-postbacks-with-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="19b16-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="19b16-127">Avançar</span><span class="sxs-lookup"><span data-stu-id="19b16-127">Next</span></span>](drag-and-drop-via-reorderlist-cs.md)
