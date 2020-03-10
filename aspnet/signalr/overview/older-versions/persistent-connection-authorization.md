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
# <a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a><span data-ttu-id="c5413-104">Autenticação e autorização para conexões persistentes do SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="c5413-104">Authentication and Authorization for SignalR Persistent Connections (SignalR 1.x)</span></span>

<span data-ttu-id="c5413-105">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c5413-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="c5413-106">Este tópico descreve como impor a autorização em uma conexão persistente.</span><span class="sxs-lookup"><span data-stu-id="c5413-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="c5413-107">Para obter informações gerais sobre como integrar a segurança em um aplicativo Signalr, consulte [introdução à segurança](index.md).</span><span class="sxs-lookup"><span data-stu-id="c5413-107">For general information about integrating security into a SignalR application, see [Introduction to Security](index.md).</span></span>

## <a name="enforce-authorization"></a><span data-ttu-id="c5413-108">Impor autorização</span><span class="sxs-lookup"><span data-stu-id="c5413-108">Enforce authorization</span></span>

<span data-ttu-id="c5413-109">Para impor regras de autorização ao usar um [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) , você deve substituir o método `AuthorizeRequest`.</span><span class="sxs-lookup"><span data-stu-id="c5413-109">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="c5413-110">Você não pode usar o atributo `Authorize` com conexões persistentes.</span><span class="sxs-lookup"><span data-stu-id="c5413-110">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="c5413-111">O método `AuthorizeRequest` é chamado pela estrutura do Signalr antes de cada solicitação para verificar se o usuário está autorizado a executar a ação solicitada.</span><span class="sxs-lookup"><span data-stu-id="c5413-111">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="c5413-112">O método `AuthorizeRequest` não é chamado a partir do cliente; em vez disso, você autentica o usuário por meio do mecanismo de autenticação padrão do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c5413-112">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="c5413-113">O exemplo a seguir mostra como limitar solicitações a usuários autenticados.</span><span class="sxs-lookup"><span data-stu-id="c5413-113">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="c5413-114">Você pode adicionar qualquer lógica de autorização personalizada no método AuthorizeRequest; como, verificando se um usuário pertence a uma função específica.</span><span class="sxs-lookup"><span data-stu-id="c5413-114">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
