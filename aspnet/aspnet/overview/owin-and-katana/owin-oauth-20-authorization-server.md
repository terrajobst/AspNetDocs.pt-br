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
# <a name="owin-oauth-20-authorization-server"></a>Servidor de autorização OAuth 2.0 de OWIN

A [estrutura do OAuth 2,0](http://tools.ietf.org/html/rfc6749) permite que um aplicativo de terceiros obtenha acesso limitado a um serviço http. Em vez de usar as credenciais do proprietário do recurso para acessar um recurso protegido, o cliente obtém um token de acesso (que é uma cadeia de caracteres que indica um escopo específico, tempo de vida e outros atributos de acesso). Tokens de acesso são emitidos para clientes de terceiros por um servidor de autorização com a aprovação do proprietário do recurso.

O OWIN define uma interface padrão entre os servidores Web e os aplicativos Web do .NET. O objetivo da interface OWIN é desacoplar o servidor e o aplicativo, incentivar o desenvolvimento de módulos simples para desenvolvimento para a Web .NET e, por ser um padrão aberto, estimular o ecossistema de software livre das ferramentas de desenvolvimento para Web .NET.

Para obter mais informações, consulte [OWIN](http://owin.org/)
