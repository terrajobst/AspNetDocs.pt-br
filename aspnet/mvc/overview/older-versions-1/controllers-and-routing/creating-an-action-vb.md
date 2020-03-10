---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Criando uma ação (VB) | Microsoft Docs
author: microsoft
description: Saiba como adicionar uma nova ação a um controlador MVC ASP.NET. Saiba mais sobre os requisitos para um método ser uma ação.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: b1b53bea899deecef203551b23c087944e3990ab
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582001"
---
# <a name="creating-an-action-vb"></a><span data-ttu-id="10793-104">Criar uma ação (VB)</span><span class="sxs-lookup"><span data-stu-id="10793-104">Creating an Action (VB)</span></span>

<span data-ttu-id="10793-105">pela [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="10793-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="10793-106">Saiba como adicionar uma nova ação a um controlador MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="10793-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="10793-107">Saiba mais sobre os requisitos para um método ser uma ação.</span><span class="sxs-lookup"><span data-stu-id="10793-107">Learn about the requirements for a method to be an action.</span></span>

<span data-ttu-id="10793-108">O objetivo deste tutorial é explicar como você pode criar uma nova ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="10793-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="10793-109">Você aprende sobre os requisitos de um método de ação.</span><span class="sxs-lookup"><span data-stu-id="10793-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="10793-110">Você também aprenderá a impedir que um método seja exposto como uma ação.</span><span class="sxs-lookup"><span data-stu-id="10793-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="10793-111">Adicionando uma ação a um controlador</span><span class="sxs-lookup"><span data-stu-id="10793-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="10793-112">Você adiciona uma nova ação a um controlador adicionando um novo método ao controlador.</span><span class="sxs-lookup"><span data-stu-id="10793-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="10793-113">Por exemplo, o controlador na Listagem 1 contém uma ação denominada index () e uma ação chamada SayHello ().</span><span class="sxs-lookup"><span data-stu-id="10793-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="10793-114">Ambos os métodos são expostos como ações.</span><span class="sxs-lookup"><span data-stu-id="10793-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="10793-115">**Listagem 1-Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="10793-115">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

<span data-ttu-id="10793-116">Para ser exposto ao universo como uma ação, um método deve atender a determinados requisitos:</span><span class="sxs-lookup"><span data-stu-id="10793-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="10793-117">O método deve ser público.</span><span class="sxs-lookup"><span data-stu-id="10793-117">The method must be public.</span></span>
- <span data-ttu-id="10793-118">O método não pode ser um método estático.</span><span class="sxs-lookup"><span data-stu-id="10793-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="10793-119">O método não pode ser um método de extensão.</span><span class="sxs-lookup"><span data-stu-id="10793-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="10793-120">O método não pode ser um construtor, getter ou setter.</span><span class="sxs-lookup"><span data-stu-id="10793-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="10793-121">O método não pode ter tipos genéricos abertos.</span><span class="sxs-lookup"><span data-stu-id="10793-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="10793-122">O método não é um método da classe base do controlador.</span><span class="sxs-lookup"><span data-stu-id="10793-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="10793-123">O método não pode conter parâmetros **ref** ou **out** .</span><span class="sxs-lookup"><span data-stu-id="10793-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="10793-124">Observe que não há restrições no tipo de retorno de uma ação de controlador.</span><span class="sxs-lookup"><span data-stu-id="10793-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="10793-125">Uma ação do controlador pode retornar uma cadeia de caracteres, um DateTime, uma instância da classe Random ou void.</span><span class="sxs-lookup"><span data-stu-id="10793-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="10793-126">O ASP.NET MVC Framework converterá qualquer tipo de retorno que não seja um resultado de ação em uma cadeia de caracteres e renderizará a cadeia de caracteres para o navegador.</span><span class="sxs-lookup"><span data-stu-id="10793-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="10793-127">Quando você adiciona qualquer método que não viole esses requisitos a um controlador, o método é exposto como uma ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="10793-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="10793-128">Tenha cuidado aqui.</span><span class="sxs-lookup"><span data-stu-id="10793-128">Be careful here.</span></span> <span data-ttu-id="10793-129">Uma ação do controlador pode ser invocada por qualquer pessoa conectada à Internet.</span><span class="sxs-lookup"><span data-stu-id="10793-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="10793-130">Não, por exemplo, criar uma ação do controlador DeleteMyWebsite ().</span><span class="sxs-lookup"><span data-stu-id="10793-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="10793-131">Impedindo que um método público seja invocado</span><span class="sxs-lookup"><span data-stu-id="10793-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="10793-132">Se você precisar criar um método público em uma classe de controlador e não quiser expor o método como uma ação de controlador, poderá impedir que o método seja invocado usando o atributo de&gt; &lt;não ação.</span><span class="sxs-lookup"><span data-stu-id="10793-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the &lt;NonAction&gt; attribute.</span></span> <span data-ttu-id="10793-133">Por exemplo, o controlador na Listagem 2 contém um método público denominado CompanySecrets () que é decorado com o atributo &lt;&gt; não ação.</span><span class="sxs-lookup"><span data-stu-id="10793-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the &lt;NonAction&gt; attribute.</span></span>

<span data-ttu-id="10793-134">**Listagem 2-Controllers\WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="10793-134">**Listing 2 - Controllers\WorkController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

<span data-ttu-id="10793-135">Se você tentar invocar a ação do controlador CompanySecrets () digitando/Work/CompanySecrets na barra de endereços do navegador, você receberá a mensagem de erro na Figura 1.</span><span class="sxs-lookup"><span data-stu-id="10793-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>

<span data-ttu-id="10793-136">[![invocando um método que não é de ação](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="10793-136">[![Invoking a NonAction method](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span></span>

<span data-ttu-id="10793-137">**Figura 01**: invocando um método que não é de ação ([clique para exibir a imagem em tamanho normal](creating-an-action-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="10793-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-vb/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="10793-138">[Anterior](creating-a-controller-vb.md)
> [Próximo](aspnet-mvc-controllers-overview-cs.md)</span><span class="sxs-lookup"><span data-stu-id="10793-138">[Previous](creating-a-controller-vb.md)
[Next](aspnet-mvc-controllers-overview-cs.md)</span></span>
