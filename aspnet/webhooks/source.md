---
uid: webhooks/source
title: Código-fonte WebHooks do ASP.NET e pacotes do NuGet | Microsoft Docs
author: rick-anderson
description: Links para código-fonte WebHooks do ASP.NET e pacotes do NuGet
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: f88d9247f9d8aa0c5edc1ffc462be21d9319a725
ms.sourcegitcommit: dd0dc556a3d99a31d8fdbc763e9a2e53f3441b70
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67410798"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>Código-fonte WebHooks do ASP.NET e pacotes do NuGet

WebHooks do Microsoft ASP.NET faz parte da família de módulos do Microsoft ASP.NET e está hospedado como um [Abrir projeto de código-fonte no GitHub](https://github.com/aspnet/WebHooks). Isso significa que podemos aceitar contribuições, mas examine os [diretrizes de contribuição](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) antes de enviar uma solicitação de pull.

Esta documentação online que você está lendo agora também está hospedada como [software livre no GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) e também aceita contribuições.

## <a name="nuget-packages"></a>Pacotes NuGet

O [pacotes do NuGet](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) são divididas em três partes:

* [Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): Um pacote comum que é compartilhado entre remetentes e receptores.

* [Remetente](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): Um conjunto de pacotes que dão suporte a enviar seus próprios WebHooks a outras pessoas. A funcionalidade para o envio de WebHooks é descrita mais detalhadamente na [WebHooks enviando](sending/senders).

* [Receptores](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): Um conjunto de pacotes que dão suporte a recebimento de WebHooks de outras pessoas. A funcionalidade para o recebimento de WebHooks é descrita mais detalhadamente na [WebHooks recebendo](receiving/index.md).
