---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: Usando ModalPopup com um controle Repeater (C#) | Microsoft Docs
author: wenz
description: O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente. Também é possível usar este contr...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 980f023c9e8256ad5323aaa6cbb6918b04e18974
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78613151"
---
# <a name="using-modalpopup-with-a-repeater-control-c"></a>Uso de ModalPopup com um controle repetidor (C#)

por [Christian Wenz](https://github.com/wenz)

[Baixar código](https://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) ou [baixar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)

> O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente. Também é possível usar esse controle dentro de um repetidor.

## <a name="overview"></a>Visão geral

O controle ModalPopup no AJAX Control Toolkit oferece uma maneira simples de criar um popup modal usando meios do lado do cliente. Também é possível usar esse controle dentro de um repetidor.

## <a name="steps"></a>Etapas

Em primeiro lugar, uma fonte de dados é necessária. Este exemplo usa o banco de dados AdventureWorks e o Microsoft SQL Server 2005 Express Edition. O banco de dados é uma parte opcional de uma instalação do Visual Studio (incluindo a edição Express) e também está disponível como um download separado em [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). O banco de dados AdventureWorks faz parte dos exemplos SQL Server 2005 e bancos de dados de exemplo (download em [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). A maneira mais fácil de definir o banco de dados é usar o Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e anexar o arquivo de banco de dados `AdventureWorks.mdf`. Para este exemplo, presumimos que a instância do SQL Server 2005 Express Edition é chamada `SQLEXPRESS` e reside no mesmo computador que o servidor Web; Essa também é a configuração padrão. Se a configuração for diferente, você precisará adaptar as informações de conexão do banco de dados. Para ativar a funcionalidade do ASP.NET AJAX e do kit de ferramentas de controle, o controle de `ScriptManager` deve ser colocado em qualquer lugar na página (mas dentro do elemento `<form>`):

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

Em seguida, adicione uma fonte de dados à página. Para usar uma quantidade limitada de dados, selecionamos apenas as cinco primeiras entradas na tabela de fornecedores do banco de dados AdventureWorks. Se você estiver usando o assistente do Visual Studio para criar a fonte de dados, lembre-se de que um bug na versão atual não prefixará o nome da tabela (`Vendor`) com `Purchasing`. A marcação a seguir mostra a sintaxe correta:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

Em seguida, adicione um painel que serve como o popup modal. Ele contém um controle `Button` para fechar o Popup novamente:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

Para fazer com que o pop-up funcione no repetidor, o controle de `ModalPopupExtender` deve ser colocado dentro da seção `<ItemTemplate>` do repetidor. Portanto, o painel está fora do repetidor, mas o extensor está dentro. Aqui está a marcação para o repetidor:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

Em seguida, cada item na fonte de dados é exibido com um botão ao lado dele que dispara o popup modal.

[![o popup modal pode ser disparado para cada entrada de fonte de dados](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)

O popup modal pode ser disparado para cada entrada de fonte de dados ([clique para exibir a imagem em tamanho normal](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](launching-a-modal-popup-window-from-server-code-cs.md)
> [Próximo](handling-postbacks-from-a-modalpopup-cs.md)
