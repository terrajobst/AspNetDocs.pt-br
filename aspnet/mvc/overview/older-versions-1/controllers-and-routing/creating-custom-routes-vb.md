---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Criando rotas personalizadas (VB) | Microsoft Docs
author: microsoft
description: Saiba como adicionar rotas personalizadas a um aplicativo MVC ASP.NET. Neste tutorial, você aprenderá a modificar a tabela de rotas padrão no arquivo global. asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: 22b44e9e575c9d404881a23ee735bb0c8b7109e1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601293"
---
# <a name="creating-custom-routes-vb"></a>Criação de rotas personalizadas (VB)

pela [Microsoft](https://github.com/microsoft)

> Saiba como adicionar rotas personalizadas a um aplicativo MVC ASP.NET. Neste tutorial, você aprenderá a modificar a tabela de rotas padrão no arquivo global. asax.

Neste tutorial, você aprenderá a adicionar uma rota personalizada a um aplicativo MVC ASP.NET. Você aprende a modificar a tabela de rotas padrão no arquivo global. asax com uma rota personalizada.

Em aplicativos MVC ASP.NET, a tabela de rotas padrão funcionará bem. No entanto, você pode descobrir que tem necessidades de roteamento especializadas. Nesse caso, você pode criar uma rota personalizada.

Imagine, por exemplo, que você está criando um aplicativo de blog. Talvez você queira lidar com as solicitações de entrada parecidas com esta:

/Archive/12-25-2009

Quando um usuário insere essa solicitação, você deseja retornar a entrada de blog que corresponde à data 12/25/2009. Para lidar com esse tipo de solicitação, você precisa criar uma rota personalizada.

O arquivo global. asax na Listagem 1 contém uma nova rota personalizada, chamada blog, que lida com solicitações que se parecem com a*data de entrada*/Archive/.

**Listagem 1-global. asax (com rota personalizada)**

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

A ordem das rotas que você adiciona à tabela de rotas é importante. Nossa nova rota de blog personalizada é adicionada antes da rota padrão existente. Se você inverter a ordem, a rota padrão sempre será chamada em vez da rota personalizada.

A rota de blog personalizada corresponde a qualquer solicitação que comece com/Archive/. Portanto, ele corresponde a todas as seguintes URLs:

- /Archive/12-25-2009

- /Archive/10-6-2004

- /Archive/apple

A rota personalizada mapeia a solicitação de entrada para um controlador chamado arquivo morto e invoca a ação de entrada (). Quando o método Entry () é chamado, a data de entrada é passada como um parâmetro chamado entryDate.

Você pode usar a rota personalizada do blog com o controlador na Listagem 2.

**Listagem 2-ArchiveController. vb**

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

Observe que o método Entry () na Listagem 2 aceita um parâmetro do tipo DateTime. A MVC Framework é inteligente o suficiente para converter a data de entrada da URL em um valor DateTime automaticamente. Se o parâmetro de data de entrada da URL não puder ser convertido em um DateTime, um erro será gerado (consulte a Figura 1).

**Figura 1-erro ao converter parâmetro**

[![caixa de diálogo novo projeto](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)

**Figura 01**: erro ao converter o parâmetro ([clique para exibir a imagem em tamanho normal](creating-custom-routes-vb/_static/image2.png))

## <a name="summary"></a>Resumo

O objetivo deste tutorial foi demonstrar como você pode criar uma rota personalizada. Você aprendeu a adicionar uma rota personalizada à tabela de rotas no arquivo global. asax que representa entradas de blog. Discutimos como mapear solicitações de entradas de blog para um controlador chamado ArchiveController e uma ação de controlador chamada Entry ().

> [!div class="step-by-step"]
> [Anterior](asp-net-mvc-controller-overview-vb.md)
> [Próximo](creating-a-route-constraint-vb.md)
