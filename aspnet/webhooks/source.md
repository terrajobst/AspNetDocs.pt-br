---
uid: webhooks/source
title: Código-fonte e pacotes NuGet do ASP.NET WebHooks | Microsoft Docs
author: rick-anderson
description: Links para o código-fonte e os pacotes NuGet do ASP.NET WebHooks
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: 8d07848754d9efda9c893b8ba54ac6d0c0214a53
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78633059"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>Código-fonte de WebHooks do ASP.NET e pacotes NuGet

Microsoft ASP.NET WebHooks faz parte da família de Microsoft ASP.NET de módulos e é hospedada como um projeto de software livre [no GitHub](https://github.com/aspnet/WebHooks). Isso significa que aceitamos contribuições, mas Confira as [diretrizes de contribuição](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) antes de enviar uma solicitação de pull.

Esta documentação online que você está lendo agora também é hospedada como [código-fonte aberto no GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) e também aceita contribuições.

## <a name="nuget-packages"></a>Pacotes NuGet

Os [pacotes NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) são divididos em três partes:

* [Comum](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): um pacote comum que é compartilhado entre remetentes e receptores.

* [Remetente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): um conjunto de pacotes que dão suporte ao envio de seus próprios WebHooks para outras pessoas. A funcionalidade para enviar WebHooks é descrita mais detalhadamente no [envio de WebHooks](sending/senders.md).

* [Receptores](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): um conjunto de pacotes que dão suporte ao recebimento de WebHooks de outros. A funcionalidade para receber WebHooks é descrita mais detalhadamente no [recebimento de WebHooks](receiving/index.md).
