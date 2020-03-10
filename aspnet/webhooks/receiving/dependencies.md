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
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="019e2-103">Dependências do receptor de WebHooks do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="019e2-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="019e2-104">Microsoft ASP.NET WebHooks é projetado com a injeção de dependência em mente.</span><span class="sxs-lookup"><span data-stu-id="019e2-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="019e2-105">A maioria das dependências no sistema pode ser substituída por implementações alternativas usando um mecanismo de injeção de dependência.</span><span class="sxs-lookup"><span data-stu-id="019e2-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="019e2-106">Consulte [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) para obter uma lista de dependências do destinatário.</span><span class="sxs-lookup"><span data-stu-id="019e2-106">See [DependencyScopeExtensions](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="019e2-107">Se nenhuma dependência tiver sido registrada, uma implementação padrão será usada.</span><span class="sxs-lookup"><span data-stu-id="019e2-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="019e2-108">Confira [destinatárioservices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) para obter uma lista de implementações padrão.</span><span class="sxs-lookup"><span data-stu-id="019e2-108">See [ReceiverServices](https://github.com/aspnet/aspnetWebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
