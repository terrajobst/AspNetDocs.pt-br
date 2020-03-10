---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
title: Usando um ConfirmButton em um Repeater (C#) | Microsoft Docs
author: wenz
description: O extensor ConfirmButton no AJAX Control Toolkit cria um pop-up sim/não quando o usuário clica em um botão (incluindo o controle LinkButton). Somente se sim for...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a973ed3e-400c-4925-ace2-0b086b479301
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 468a830f01c48dc39b22bc5d826f80533df65c1a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554281"
---
# <a name="using-a-confirmbutton-in-a-repeater-c"></a>Uso de um ConfirmButton em um repetidor (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)

> O extensor ConfirmButton no AJAX Control Toolkit cria um pop-up sim/não quando o usuário clica em um botão (incluindo o controle LinkButton). Somente se sim for clicado, a ação do botão será executada, caso contrário, cancelada. Isso também é possível em um repetidor.

## <a name="overview"></a>Visão geral

O extensor ConfirmButton no AJAX Control Toolkit cria um pop-up sim/não quando o usuário clica em um botão (incluindo o controle LinkButton). Somente se sim for clicado, a ação do botão será executada, caso contrário, cancelada. Isso também é possível em um repetidor.

## <a name="steps"></a>Etapas

Em primeiro lugar, uma fonte de dados é necessária. Este exemplo usa o banco de dados AdventureWorks e o Microsoft SQL Server 2005 Express Edition. O banco de dados é uma parte opcional de uma instalação do Visual Studio (incluindo a edição Express) e também está disponível como um download separado em [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). O banco de dados AdventureWorks faz parte dos exemplos SQL Server 2005 e bancos de dados de exemplo (download em [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). A maneira mais fácil de definir o banco de dados é usar o Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e anexar o arquivo de banco de dados `AdventureWorks.mdf`.

Para este exemplo, presumimos que a instância do SQL Server 2005 Express Edition é chamada `SQLEXPRESS` e reside no mesmo computador que o servidor Web; Essa também é a configuração padrão. Se a configuração for diferente, você precisará adaptar as informações de conexão do banco de dados.

Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado em qualquer lugar na página (mas dentro do elemento `<form>`):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample1.aspx)]

Em seguida, uma fonte de dados é necessária. Para simplificar, somente as primeiras cinco entradas na tabela de fornecedores do AdventureWorks são recuperadas. Observe que, ao usar o assistente do Visual Studio para criar a fonte de dados, o nome da tabela (`Vendors`) atualmente não é corretamente prefixado com `Purchasing`. A marcação a seguir é a correta:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample2.aspx)]

Essa fonte de dados pode ser usada em um repetidor. Como de costume, o método `DataBinder.Eval()` recupera dados da fonte de dados. O controle de `ConfirmButtonExtender` deve ser colocado dentro da seção `<ItemTemplate>` do repetidor para que ele apareça para cada entrada na fonte de dados.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample3.aspx)]

[![o botão confirmar é exibido ao lado de cada entrada da fonte de dados](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)

O botão confirmar é exibido ao lado de cada entrada da fonte de dados ([clique para exibir a imagem em tamanho normal](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Próximo](using-a-confirmbutton-in-a-repeater-vb.md)
