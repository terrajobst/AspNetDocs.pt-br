---
uid: webhooks/receiving/dependencies
title: Dependências do receptor de WebHooks do ASP.NET | Microsoft Docs
author: rick-anderson
description: Dependências do receptor e injeção de dependência em WebHooks ASP.NET.
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: 477b8828209d0da1d485ef883b0f99b4e1b9b5bf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637280"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>Dependências do receptor de WebHooks do ASP.NET

Microsoft ASP.NET WebHooks é projetado com a injeção de dependência em mente. A maioria das dependências no sistema pode ser substituída por implementações alternativas usando um mecanismo de injeção de dependência.

Consulte [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) para obter uma lista de dependências do destinatário. Se nenhuma dependência tiver sido registrada, uma implementação padrão será usada. Confira [destinatárioservices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) para obter uma lista de implementações padrão.
