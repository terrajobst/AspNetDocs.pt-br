---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Como faço para:] Use a propriedade resposta. Filter para substituir o HTML em uma página do ASP.NET | Microsoft Docs'
author: rick-anderson
description: Neste vídeo, Chris pixels mostra como usar a propriedade resposta. Filter para interceptar e alterar o HTML que está sendo enviado para uma página. Primeiro, uma página de exemplo é criada w...
ms.author: riande
ms.date: 01/29/2009
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 2ebd9162f81f5270c92c6b8d55e2d2dad4660701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78602812"
---
# <a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Como faço para:] Usar a propriedade resposta. Filter para substituir o HTML em uma página ASP.NET

por [Chris pixels](https://twitter.com/chrispels)

Neste vídeo, Chris pixels mostra como usar a propriedade resposta. Filter para interceptar e alterar o HTML que está sendo enviado para uma página. Primeiro, uma página de exemplo é criada com um texto simples. Em seguida, é criada uma classe de fluxo personalizada que serve como o fluxo de substituição para o fluxo atual que está sendo enviado ao navegador do usuário. Nessa classe de fluxo personalizada, o conteúdo da página é recuperado do fluxo, alterado e, em seguida, escrito para o fluxo de resposta. Nessa classe de fluxo personalizada, o método Write é personalizado para substituir o HTML no fluxo de resposta base, alterando assim o que é enviado ao navegador do usuário. Por fim, a nova classe Stream é atribuída à propriedade Response. Filter na página\_evento Load, assim, fornecendo o mecanismo para alterar o conteúdo da página.

[&#9654;Assistir ao vídeo (13 minutos)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
