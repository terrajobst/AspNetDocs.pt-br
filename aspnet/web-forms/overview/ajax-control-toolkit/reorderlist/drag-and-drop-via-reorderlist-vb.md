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
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78553917"
---
# <a name="drag-and-drop-via-reorderlist-vb"></a>Arrastar e soltar por meio de ReorderList (VB)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) ou [baixar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> O controle reorganizelist no AJAX Control Toolkit fornece uma lista que pode ser reordenada pelo usuário por meio de arrastar e soltar. A ordem atual da lista deve ser persistida no servidor.

## <a name="overview"></a>Visão geral

O controle de `ReorderList` no AJAX Control Toolkit fornece uma lista que pode ser reordenada pelo usuário por meio de arrastar e soltar. A ordem atual da lista deve ser persistida no servidor.

## <a name="steps"></a>Etapas

O controle `ReorderList` dá suporte à associação de dados de um banco de dado à lista. O melhor de tudo é que ele também dá suporte à gravação de alterações na ordem do elemento de lista de volta para o armazenamento de dados.

Este exemplo usa o Microsoft SQL Server 2005 Express Edition como o armazenamento de dados. O banco de dados é uma parte opcional (e gratuita) de uma instalação do Visual Studio, incluindo a Express Edition. Ele também está disponível como um download separado em [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Para este exemplo, presumimos que a instância do SQL Server 2005 Express Edition é chamada `SQLEXPRESS` e reside no mesmo computador que o servidor Web; Essa também é a configuração padrão. Se a configuração for diferente, você precisará adaptar as informações de conexão do banco de dados.

A maneira mais fácil de configurar o banco de dados é usar o Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Conecte-se ao servidor, clique duas vezes em `Databases` e crie um novo banco de dados (clique com o botão direito do mouse e escolha `New Database`) chamado `Tutorials`.

Neste banco de dados, crie uma nova tabela chamada `AJAX` com as quatro colunas a seguir:

- `id` (chave primária, inteiro, identidade, não nulo)
- `char` (Char (1), NULL)
- `description` (varchar (50), NULL)
- `position` (int, NULL)

[![o layout da tabela AJAX](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

O layout da tabela AJAX ([clique para exibir a imagem em tamanho normal](drag-and-drop-via-reorderlist-vb/_static/image3.png))

Em seguida, preencha a tabela com alguns valores. Observe que a coluna `position` contém a ordem de classificação dos elementos.

[![os dados iniciais na tabela AJAX](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

Os dados iniciais na tabela AJAX ([clique para exibir a imagem em tamanho normal](drag-and-drop-via-reorderlist-vb/_static/image6.png))

A próxima etapa requer a geração de um controle de `SqlDataSource` para se comunicar com o novo banco de dados e sua tabela. A fonte de dados deve dar suporte aos comandos `SELECT` e `UPDATE` SQL. Quando a ordem dos elementos da lista é alterada posteriormente, o controle de `ReorderList` envia automaticamente dois valores para o comando de `Update` da fonte de dados: a nova posição e a ID do elemento. Portanto, a fonte de dados precisa de uma `<UpdateParameters>` seção para esses dois valores:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

O controle de `ReorderList` precisa definir os seguintes atributos:

- `AllowReorder`: se os itens de lista podem ser reorganizados
- `DataSourceID`: a ID da fonte de dados
- `DataKeyField`: o nome da coluna de chave primária na fonte de dados
- `SortOrderField`: a coluna de fonte de dados que fornece a ordem de classificação para os itens de lista

Nas seções `<DragHandleTemplate>` e `<ItemTemplate>`, o layout da lista pode ser ajustado. Além disso, DataBinding é possível usando o método `Eval()`, como visto aqui:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

As informações de estilo CSS a seguir (referenciadas na seção `<DragHandleTemplate>` do controle de `ReorderList`) garantem que o ponteiro do mouse seja alterado adequadamente quando focalizado sobre a alça de arrasto:

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

Por fim, um controle de `ScriptManager` Inicializa o ASP.NET AJAX para a página:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

Execute este exemplo no navegador e reorganize os itens de lista um pouco. Em seguida, recarregue a página e/ou observe o banco de dados. As posições alteradas foram mantidas e também são refletidas pelos valores na coluna `position` no banco de dados e que tudo sem qualquer código, apenas usando marcação.

[![os dados no banco de dados são alterados de acordo com a nova ordem de item de lista](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

O banco de dados é alterado de acordo com a nova ordem de item de lista ([clique para exibir a imagem em tamanho normal](drag-and-drop-via-reorderlist-vb/_static/image9.png))

> [!div class="step-by-step"]
> [Anterior](using-postbacks-with-reorderlist-vb.md)
