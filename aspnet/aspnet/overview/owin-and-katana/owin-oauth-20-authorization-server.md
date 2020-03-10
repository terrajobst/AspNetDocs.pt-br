---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: Servidor de autorização OWIN OAuth 2,0 | Microsoft Docs
author: hongyes
description: Este tutorial irá orientá-lo sobre como implementar um servidor de autorização OAuth 2,0 usando o middleware OAuth OWIN. Este é um tutorial avançado que só outlin...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: 39c78ee57f791a94af7a5fb66c3ac42d7239760f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617106"
---
# <a name="owin-oauth-20-authorization-server"></a><span data-ttu-id="780c8-104">Servidor de autorização OAuth 2.0 de OWIN</span><span class="sxs-lookup"><span data-stu-id="780c8-104">OWIN OAuth 2.0 Authorization Server</span></span>

<span data-ttu-id="780c8-105">A [estrutura do OAuth 2,0](http://tools.ietf.org/html/rfc6749) permite que um aplicativo de terceiros obtenha acesso limitado a um serviço http.</span><span class="sxs-lookup"><span data-stu-id="780c8-105">The [OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) enables a third-party app to obtain limited access to an HTTP service.</span></span> <span data-ttu-id="780c8-106">Em vez de usar as credenciais do proprietário do recurso para acessar um recurso protegido, o cliente obtém um token de acesso (que é uma cadeia de caracteres que indica um escopo específico, tempo de vida e outros atributos de acesso).</span><span class="sxs-lookup"><span data-stu-id="780c8-106">Instead of using the resource owner's credentials to access a protected resource, the client obtains an access token (which is a string denoting a specific scope, lifetime, and other access attributes).</span></span> <span data-ttu-id="780c8-107">Tokens de acesso são emitidos para clientes de terceiros por um servidor de autorização com a aprovação do proprietário do recurso.</span><span class="sxs-lookup"><span data-stu-id="780c8-107">Access tokens are issued to third-party clients by an authorization server with the approval of the resource owner.</span></span>

<span data-ttu-id="780c8-108">O OWIN define uma interface padrão entre os servidores Web e os aplicativos Web do .NET.</span><span class="sxs-lookup"><span data-stu-id="780c8-108">OWIN defines a standard interface between .NET web servers and web applications.</span></span> <span data-ttu-id="780c8-109">O objetivo da interface OWIN é desacoplar o servidor e o aplicativo, incentivar o desenvolvimento de módulos simples para desenvolvimento para a Web .NET e, por ser um padrão aberto, estimular o ecossistema de software livre das ferramentas de desenvolvimento para Web .NET.</span><span class="sxs-lookup"><span data-stu-id="780c8-109">The goal of the OWIN interface is to decouple server and application, encourage the development of simple modules for .NET web development, and, by being an open standard, stimulate the open source ecosystem of .NET web development tools.</span></span>

<span data-ttu-id="780c8-110">Para obter mais informações, consulte [OWIN](http://owin.org/)</span><span class="sxs-lookup"><span data-stu-id="780c8-110">For more information, see [OWIN](http://owin.org/)</span></span>
