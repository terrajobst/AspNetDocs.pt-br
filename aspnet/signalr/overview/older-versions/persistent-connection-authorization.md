---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Autenticação e autorização para conexões persistentes do Signalr (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: Este tópico descreve como impor a autorização em uma conexão persistente. Para obter informações gerais sobre como integrar a segurança em um aplicativo Signalr,...
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9ccc59e3ea502daf12ce82382ab30ca73ca0f9b5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78536536"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Autenticação e autorização para conexões persistentes do SignalR (SignalR 1.x)

por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este tópico descreve como impor a autorização em uma conexão persistente. Para obter informações gerais sobre como integrar a segurança em um aplicativo Signalr, consulte [introdução à segurança](index.md).

## <a name="enforce-authorization"></a>Impor autorização

Para impor regras de autorização ao usar um [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) , você deve substituir o método `AuthorizeRequest`. Você não pode usar o atributo `Authorize` com conexões persistentes. O método `AuthorizeRequest` é chamado pela estrutura do Signalr antes de cada solicitação para verificar se o usuário está autorizado a executar a ação solicitada. O método `AuthorizeRequest` não é chamado a partir do cliente; em vez disso, você autentica o usuário por meio do mecanismo de autenticação padrão do aplicativo.

O exemplo a seguir mostra como limitar solicitações a usuários autenticados.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Você pode adicionar qualquer lógica de autorização personalizada no método AuthorizeRequest; como, verificando se um usuário pertence a uma função específica.
