---
uid: signalr/overview/security/persistent-connection-authorization
title: Autenticação e autorização para conexões persistentes do Signalr | Microsoft Docs
author: bradygaster
description: Este tópico descreve como impor a autorização em uma conexão persistente. Para obter informações gerais sobre como integrar a segurança em um aplicativo Signalr,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 7e69d3c1a18f1239142891734ba58cd2b0078f84
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578872"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections"></a>Autenticação e autorização para conexões persistentes do SignalR

por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este tópico descreve como impor a autorização em uma conexão persistente. Para obter informações gerais sobre como integrar a segurança em um aplicativo Signalr, consulte [introdução à segurança](introduction-to-security.md).
>
> ## <a name="software-versions-used-in-this-topic"></a>Versões de software usadas neste tópico
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Sinalização versão 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versões anteriores deste tópico
>
> Para obter informações sobre versões anteriores do Signalr, confira [versões mais antigas do signalr](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Perguntas e comentários
>
> Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).

## <a name="enforce-authorization"></a>Impor autorização

Para impor regras de autorização ao usar um [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) , você deve substituir o método `AuthorizeRequest`. Você não pode usar o atributo `Authorize` com conexões persistentes. O método `AuthorizeRequest` é chamado pela estrutura do Signalr antes de cada solicitação para verificar se o usuário está autorizado a executar a ação solicitada. O método `AuthorizeRequest` não é chamado a partir do cliente; em vez disso, você autentica o usuário por meio do mecanismo de autenticação padrão do aplicativo.

O exemplo a seguir mostra como limitar solicitações a usuários autenticados.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Você pode adicionar qualquer lógica de autorização personalizada no método AuthorizeRequest; como, verificando se um usuário pertence a uma função específica.
