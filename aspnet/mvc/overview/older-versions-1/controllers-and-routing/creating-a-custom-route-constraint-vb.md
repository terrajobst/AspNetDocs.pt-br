---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Criando uma restrição de rota personalizada (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther demonstra como você pode criar uma restrição de rota personalizada. Implementamos uma restrição personalizada simples que impede que uma rota seja correspondida w...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 2330708cf4a28180ce8a05f4696bf7a7a32092d6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601405"
---
# <a name="creating-a-custom-route-constraint-vb"></a><span data-ttu-id="20420-104">Criação de uma restrição de rota personalizada (VB)</span><span class="sxs-lookup"><span data-stu-id="20420-104">Creating a Custom Route Constraint (VB)</span></span>

<span data-ttu-id="20420-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="20420-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="20420-106">Stephen Walther demonstra como você pode criar uma restrição de rota personalizada.</span><span class="sxs-lookup"><span data-stu-id="20420-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="20420-107">Implementamos uma restrição personalizada simples que impede a correspondência de uma rota quando uma solicitação do navegador é feita de um computador remoto.</span><span class="sxs-lookup"><span data-stu-id="20420-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>

<span data-ttu-id="20420-108">O objetivo deste tutorial é demonstrar como você pode criar uma restrição de rota personalizada.</span><span class="sxs-lookup"><span data-stu-id="20420-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="20420-109">Uma restrição de rota personalizada permite impedir que uma rota seja correspondida, a menos que alguma condição personalizada seja correspondida.</span><span class="sxs-lookup"><span data-stu-id="20420-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="20420-110">Neste tutorial, criamos uma restrição de rota de localhost.</span><span class="sxs-lookup"><span data-stu-id="20420-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="20420-111">A restrição de rota do localhost só corresponde a solicitações feitas do computador local.</span><span class="sxs-lookup"><span data-stu-id="20420-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="20420-112">As solicitações remotas de pela Internet não são correspondidas.</span><span class="sxs-lookup"><span data-stu-id="20420-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="20420-113">Implemente uma restrição de rota personalizada implementando a interface IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="20420-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="20420-114">Esta é uma interface extremamente simples que descreve um único método:</span><span class="sxs-lookup"><span data-stu-id="20420-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="20420-115">O método retorna um valor booliano.</span><span class="sxs-lookup"><span data-stu-id="20420-115">The method returns a Boolean value.</span></span> <span data-ttu-id="20420-116">Se você retornar false, a rota associada à restrição não corresponderá à solicitação do navegador.</span><span class="sxs-lookup"><span data-stu-id="20420-116">If you return False, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="20420-117">A restrição de localhost está contida na Listagem 1.</span><span class="sxs-lookup"><span data-stu-id="20420-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="20420-118">**Listagem 1-LocalhostConstraint. vb**</span><span class="sxs-lookup"><span data-stu-id="20420-118">**Listing 1 - LocalhostConstraint.vb**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="20420-119">A restrição na Listagem 1 tira proveito da Propriedade IsLocal exposta pela classe HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="20420-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="20420-120">Essa propriedade retornará true quando o endereço IP da solicitação for 127.0.0.1 ou quando o IP da solicitação for o mesmo que o endereço IP do servidor.</span><span class="sxs-lookup"><span data-stu-id="20420-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="20420-121">Você usa uma restrição personalizada dentro de uma rota definida no arquivo global. asax.</span><span class="sxs-lookup"><span data-stu-id="20420-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="20420-122">O arquivo global. asax na Listagem 2 usa a restrição localhost para impedir que qualquer pessoa solicite uma página de administração, a menos que faça a solicitação do servidor local.</span><span class="sxs-lookup"><span data-stu-id="20420-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="20420-123">Por exemplo, uma solicitação de/Admin/DeleteAll falhará quando feita de um servidor remoto.</span><span class="sxs-lookup"><span data-stu-id="20420-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="20420-124">**Listagem 2-global. asax**</span><span class="sxs-lookup"><span data-stu-id="20420-124">**Listing 2 - Global.asax**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="20420-125">A restrição de localhost é usada na definição da rota do administrador.</span><span class="sxs-lookup"><span data-stu-id="20420-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="20420-126">Essa rota não será correspondida por uma solicitação de navegador remoto.</span><span class="sxs-lookup"><span data-stu-id="20420-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="20420-127">No entanto, perceba que outras rotas definidas em global. asax podem corresponder à mesma solicitação.</span><span class="sxs-lookup"><span data-stu-id="20420-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="20420-128">É importante entender que uma restrição impede que uma rota específica corresponda a uma solicitação e não a todas as rotas definidas no arquivo global. asax.</span><span class="sxs-lookup"><span data-stu-id="20420-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="20420-129">Observe que a rota padrão foi comentada do arquivo global. asax na Listagem 2.</span><span class="sxs-lookup"><span data-stu-id="20420-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="20420-130">Se você incluir a rota padrão, a rota padrão corresponderia às solicitações do controlador de administração.</span><span class="sxs-lookup"><span data-stu-id="20420-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="20420-131">Nesse caso, os usuários remotos ainda podiam invocar ações do controlador de administração, mesmo que suas solicitações não correspondam à rota do administrador.</span><span class="sxs-lookup"><span data-stu-id="20420-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="20420-132">Anterior</span><span class="sxs-lookup"><span data-stu-id="20420-132">Previous</span></span>](creating-a-route-constraint-vb.md)
