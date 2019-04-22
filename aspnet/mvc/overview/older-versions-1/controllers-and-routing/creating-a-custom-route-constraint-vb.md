---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Criar uma restrição de rota personalizada (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther demonstra como você pode criar uma restrição de rota personalizados. Implementamos um simples restrição personalizada que impede que uma rota que está sendo correspondido w...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: febba98be86f0151724af6d6c00fb14760ce1b91
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59378943"
---
# <a name="creating-a-custom-route-constraint-vb"></a><span data-ttu-id="14a52-104">Criação de uma restrição de rota personalizada (VB)</span><span class="sxs-lookup"><span data-stu-id="14a52-104">Creating a Custom Route Constraint (VB)</span></span>

<span data-ttu-id="14a52-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="14a52-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="14a52-106">Stephen Walther demonstra como você pode criar uma restrição de rota personalizados.</span><span class="sxs-lookup"><span data-stu-id="14a52-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="14a52-107">Implementamos uma restrição personalizada simple que impede que uma rota que está sendo correspondido quando é feita uma solicitação de navegador de um computador remoto.</span><span class="sxs-lookup"><span data-stu-id="14a52-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="14a52-108">O objetivo deste tutorial é demonstrar como você pode criar uma restrição de rota personalizados.</span><span class="sxs-lookup"><span data-stu-id="14a52-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="14a52-109">Uma restrição de rota personalizada permite impedir que uma rota que está sendo correspondido a menos que alguma condição personalizada é correspondida.</span><span class="sxs-lookup"><span data-stu-id="14a52-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="14a52-110">Neste tutorial, podemos criar uma restrição de rota do Localhost.</span><span class="sxs-lookup"><span data-stu-id="14a52-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="14a52-111">A restrição de rota Localhost corresponde apenas a solicitações feitas do computador local.</span><span class="sxs-lookup"><span data-stu-id="14a52-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="14a52-112">Não correspondem às solicitações remotas pela Internet.</span><span class="sxs-lookup"><span data-stu-id="14a52-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="14a52-113">Você pode implementar uma restrição de rota personalizada, Implementando a interface IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="14a52-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="14a52-114">Esta é uma interface muito simple que descreve um único método:</span><span class="sxs-lookup"><span data-stu-id="14a52-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="14a52-115">O método retorna um valor booliano.</span><span class="sxs-lookup"><span data-stu-id="14a52-115">The method returns a Boolean value.</span></span> <span data-ttu-id="14a52-116">Se você retornar False, a rota associada com a restrição não corresponde a solicitação do navegador.</span><span class="sxs-lookup"><span data-stu-id="14a52-116">If you return False, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="14a52-117">A restrição de Localhost está contida na listagem 1.</span><span class="sxs-lookup"><span data-stu-id="14a52-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="14a52-118">**Listagem 1 - LocalhostConstraint.vb**</span><span class="sxs-lookup"><span data-stu-id="14a52-118">**Listing 1 - LocalhostConstraint.vb**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="14a52-119">A restrição na listagem 1 tira proveito da propriedade IsLocal exposto pela classe HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="14a52-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="14a52-120">Essa propriedade retorna true quando o endereço IP da solicitação é qualquer 127.0.0.1 ou quando o IP da solicitação é o mesmo endereço IP do servidor.</span><span class="sxs-lookup"><span data-stu-id="14a52-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="14a52-121">Você usar uma restrição personalizada dentro de uma rota definida no arquivo global. asax.</span><span class="sxs-lookup"><span data-stu-id="14a52-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="14a52-122">O arquivo global asax na listagem 2 usa a restrição de Localhost para evitar que qualquer pessoa que solicite uma página de administração, a menos que eles fazem a solicitação do servidor local.</span><span class="sxs-lookup"><span data-stu-id="14a52-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="14a52-123">Por exemplo, uma solicitação para /Admin/DeleteAll falhará quando feita de um servidor remoto.</span><span class="sxs-lookup"><span data-stu-id="14a52-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="14a52-124">**Listagem 2 - global. asax**</span><span class="sxs-lookup"><span data-stu-id="14a52-124">**Listing 2 - Global.asax**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="14a52-125">A restrição de Localhost é usada na definição de rota de administrador.</span><span class="sxs-lookup"><span data-stu-id="14a52-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="14a52-126">Essa rota não ser correspondida por uma solicitação remota do navegador.</span><span class="sxs-lookup"><span data-stu-id="14a52-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="14a52-127">No entanto, observe que outras rotas definidas em global. asax podem corresponder a mesma solicitação.</span><span class="sxs-lookup"><span data-stu-id="14a52-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="14a52-128">É importante entender que uma restrição impede que uma rota específica de correspondência de uma solicitação e não todas as rotas definidas no arquivo global. asax.</span><span class="sxs-lookup"><span data-stu-id="14a52-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="14a52-129">Observe que a rota padrão foi comentada do arquivo global. asax na listagem 2.</span><span class="sxs-lookup"><span data-stu-id="14a52-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="14a52-130">Se você incluir a rota padrão, a rota padrão corresponderia solicitações para o controlador de administrador.</span><span class="sxs-lookup"><span data-stu-id="14a52-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="14a52-131">Nesse caso, os usuários remotos ainda pode invocar ações do controlador de administrador, mesmo que suas solicitações não correspondem à rota Admin.</span><span class="sxs-lookup"><span data-stu-id="14a52-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="14a52-132">Anterior</span><span class="sxs-lookup"><span data-stu-id="14a52-132">Previous</span></span>](creating-a-route-constraint-vb.md)
