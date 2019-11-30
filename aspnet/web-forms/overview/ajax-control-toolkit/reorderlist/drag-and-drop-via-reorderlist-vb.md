---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Arrastar e soltar por meio de reordenalist (VB) | Microsoft Docs
author: wenz
description: /data-access/tutorials/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 3f7c5749053d8bf587467fb1939fca05ce2872a4
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74598703"
---
# <a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="c805d-103">Arrastar e soltar por meio de ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="c805d-103">Drag and Drop via ReorderList (VB)</span></span>

<span data-ttu-id="c805d-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c805d-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c805d-105">[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c805d-105">[Download Code](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="c805d-106">O controle reorganizelist no AJAX Control Toolkit fornece uma lista que pode ser reordenada pelo usuário por meio de arrastar e soltar.</span><span class="sxs-lookup"><span data-stu-id="c805d-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="c805d-107">A ordem atual da lista deve ser persistida no servidor.</span><span class="sxs-lookup"><span data-stu-id="c805d-107">The current order of the list shall be persisted on the server.</span></span>

## <a name="overview"></a><span data-ttu-id="c805d-108">{1&gt;Visão Geral&lt;1}</span><span class="sxs-lookup"><span data-stu-id="c805d-108">Overview</span></span>

<span data-ttu-id="c805d-109">O controle de `ReorderList` no AJAX Control Toolkit fornece uma lista que pode ser reordenada pelo usuário por meio de arrastar e soltar.</span><span class="sxs-lookup"><span data-stu-id="c805d-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="c805d-110">A ordem atual da lista deve ser persistida no servidor.</span><span class="sxs-lookup"><span data-stu-id="c805d-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="c805d-111">Etapas</span><span class="sxs-lookup"><span data-stu-id="c805d-111">Steps</span></span>

<span data-ttu-id="c805d-112">O controle `ReorderList` dá suporte à associação de dados de um banco de dado à lista.</span><span class="sxs-lookup"><span data-stu-id="c805d-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="c805d-113">O melhor de tudo é que ele também dá suporte à gravação de alterações na ordem do elemento de lista de volta para o armazenamento de dados.</span><span class="sxs-lookup"><span data-stu-id="c805d-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="c805d-114">Este exemplo usa o Microsoft SQL Server 2005 Express Edition como o armazenamento de dados.</span><span class="sxs-lookup"><span data-stu-id="c805d-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="c805d-115">O banco de dados é uma parte opcional (e gratuita) de uma instalação do Visual Studio, incluindo a Express Edition.</span><span class="sxs-lookup"><span data-stu-id="c805d-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="c805d-116">Ele também está disponível como um download separado em [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="c805d-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="c805d-117">Para este exemplo, presumimos que a instância do SQL Server 2005 Express Edition é chamada `SQLEXPRESS` e reside no mesmo computador que o servidor Web; Essa também é a configuração padrão.</span><span class="sxs-lookup"><span data-stu-id="c805d-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="c805d-118">Se a configuração for diferente, você precisará adaptar as informações de conexão do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c805d-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="c805d-119">A maneira mais fácil de configurar o banco de dados é usar o Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="c805d-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="c805d-120">Conecte-se ao servidor, clique duas vezes em `Databases` e crie um novo banco de dados (clique com o botão direito do mouse e escolha `New Database`) chamado `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="c805d-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="c805d-121">Neste banco de dados, crie uma nova tabela chamada `AJAX` com as quatro colunas a seguir:</span><span class="sxs-lookup"><span data-stu-id="c805d-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="c805d-122">`id` (chave primária, inteiro, identidade, não nulo)</span><span class="sxs-lookup"><span data-stu-id="c805d-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="c805d-123">`char` (Char (1), NULL)</span><span class="sxs-lookup"><span data-stu-id="c805d-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="c805d-124">`description` (varchar (50), NULL)</span><span class="sxs-lookup"><span data-stu-id="c805d-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="c805d-125">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="c805d-125">`position` (int, NULL)</span></span>

<span data-ttu-id="c805d-126">[![o layout da tabela AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c805d-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="c805d-127">O layout da tabela AJAX ([clique para exibir a imagem em tamanho normal](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c805d-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>

<span data-ttu-id="c805d-128">Em seguida, preencha a tabela com alguns valores.</span><span class="sxs-lookup"><span data-stu-id="c805d-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="c805d-129">Observe que a coluna `position` contém a ordem de classificação dos elementos.</span><span class="sxs-lookup"><span data-stu-id="c805d-129">Note that the `position` column holds the sort order of the elements.</span></span>

<span data-ttu-id="c805d-130">[![os dados iniciais na tabela AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="c805d-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="c805d-131">Os dados iniciais na tabela AJAX ([clique para exibir a imagem em tamanho normal](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="c805d-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>

<span data-ttu-id="c805d-132">A próxima etapa requer a geração de um controle de `SqlDataSource` para se comunicar com o novo banco de dados e sua tabela.</span><span class="sxs-lookup"><span data-stu-id="c805d-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="c805d-133">A fonte de dados deve dar suporte aos comandos `SELECT` e `UPDATE` SQL.</span><span class="sxs-lookup"><span data-stu-id="c805d-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="c805d-134">Quando a ordem dos elementos da lista é alterada posteriormente, o controle de `ReorderList` envia automaticamente dois valores para o comando de `Update` da fonte de dados: a nova posição e a ID do elemento.</span><span class="sxs-lookup"><span data-stu-id="c805d-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="c805d-135">Portanto, a fonte de dados precisa de uma `<UpdateParameters>` seção para esses dois valores:</span><span class="sxs-lookup"><span data-stu-id="c805d-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="c805d-136">O controle de `ReorderList` precisa definir os seguintes atributos:</span><span class="sxs-lookup"><span data-stu-id="c805d-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="c805d-137">`AllowReorder`: se os itens de lista podem ser reorganizados</span><span class="sxs-lookup"><span data-stu-id="c805d-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="c805d-138">`DataSourceID`: a ID da fonte de dados</span><span class="sxs-lookup"><span data-stu-id="c805d-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="c805d-139">`DataKeyField`: o nome da coluna de chave primária na fonte de dados</span><span class="sxs-lookup"><span data-stu-id="c805d-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="c805d-140">`SortOrderField`: a coluna de fonte de dados que fornece a ordem de classificação para os itens de lista</span><span class="sxs-lookup"><span data-stu-id="c805d-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="c805d-141">Nas seções `<DragHandleTemplate>` e `<ItemTemplate>`, o layout da lista pode ser ajustado.</span><span class="sxs-lookup"><span data-stu-id="c805d-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="c805d-142">Além disso, DataBinding é possível usando o método `Eval()`, como visto aqui:</span><span class="sxs-lookup"><span data-stu-id="c805d-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="c805d-143">As informações de estilo CSS a seguir (referenciadas na seção `<DragHandleTemplate>` do controle de `ReorderList`) garantem que o ponteiro do mouse seja alterado adequadamente quando focalizado sobre a alça de arrasto:</span><span class="sxs-lookup"><span data-stu-id="c805d-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="c805d-144">Por fim, um controle de `ScriptManager` Inicializa o ASP.NET AJAX para a página:</span><span class="sxs-lookup"><span data-stu-id="c805d-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="c805d-145">Execute este exemplo no navegador e reorganize os itens de lista um pouco.</span><span class="sxs-lookup"><span data-stu-id="c805d-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="c805d-146">Em seguida, recarregue a página e/ou observe o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="c805d-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="c805d-147">As posições alteradas foram mantidas e também são refletidas pelos valores na coluna `position` no banco de dados e que tudo sem qualquer código, apenas usando marcação.</span><span class="sxs-lookup"><span data-stu-id="c805d-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>

<span data-ttu-id="c805d-148">[![os dados no banco de dados são alterados de acordo com a nova ordem de item de lista](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="c805d-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="c805d-149">O banco de dados é alterado de acordo com a nova ordem de item de lista ([clique para exibir a imagem em tamanho normal](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="c805d-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c805d-150">Anterior</span><span class="sxs-lookup"><span data-stu-id="c805d-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
