---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Validação de modelo no ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Visão geral da validação de modelo no ASP.NET Web API para ASP.NET 4. x.
ms.author: riande
ms.date: 07/20/2012
ms.custom: seoapril2019
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 531a66b7ab642bd012663517640f2766f1917f25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557235"
---
# <a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="7c67c-103">Validação de modelo no ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="7c67c-103">Model Validation in ASP.NET Web API</span></span>

<span data-ttu-id="7c67c-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7c67c-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="7c67c-105">Este artigo mostra como anotar seus modelos, usar as anotações para validação de dados e tratar erros de validação em sua API Web.</span><span class="sxs-lookup"><span data-stu-id="7c67c-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span> <span data-ttu-id="7c67c-106">Quando um cliente envia dados para sua API Web, geralmente você deseja validar os dados antes de fazer qualquer processamento.</span><span class="sxs-lookup"><span data-stu-id="7c67c-106">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> 

## <a name="data-annotations"></a><span data-ttu-id="7c67c-107">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="7c67c-107">Data Annotations</span></span>

<span data-ttu-id="7c67c-108">No ASP.NET Web API, você pode usar atributos do namespace [System. ComponentModel. Annotations](/dotnet/api/system.componentmodel.dataannotations) para definir regras de validação para propriedades em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="7c67c-108">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="7c67c-109">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="7c67c-109">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="7c67c-110">Se você usou a validação de modelo no ASP.NET MVC, isso deve parecer familiar.</span><span class="sxs-lookup"><span data-stu-id="7c67c-110">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="7c67c-111">O atributo **Required** indica que a propriedade `Name` não deve ser nula.</span><span class="sxs-lookup"><span data-stu-id="7c67c-111">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="7c67c-112">O atributo **Range** indica que `Weight` deve estar entre zero e 999.</span><span class="sxs-lookup"><span data-stu-id="7c67c-112">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="7c67c-113">Suponha que um cliente envie uma solicitação POST com a seguinte representação JSON:</span><span class="sxs-lookup"><span data-stu-id="7c67c-113">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="7c67c-114">Você pode ver que o cliente não incluiu a propriedade `Name`, que está marcada como necessária.</span><span class="sxs-lookup"><span data-stu-id="7c67c-114">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="7c67c-115">Quando a API da Web converte o JSON em uma `Product` instância, ela valida a `Product` em relação aos atributos de validação.</span><span class="sxs-lookup"><span data-stu-id="7c67c-115">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="7c67c-116">Na ação do controlador, você pode verificar se o modelo é válido:</span><span class="sxs-lookup"><span data-stu-id="7c67c-116">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="7c67c-117">A validação de modelo não garante que os dados do cliente sejam seguros.</span><span class="sxs-lookup"><span data-stu-id="7c67c-117">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="7c67c-118">A validação adicional pode ser necessária em outras camadas do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7c67c-118">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="7c67c-119">(Por exemplo, a camada de dados pode impor restrições Foreign Key). O tutorial [usando a API Web com o Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explora alguns desses problemas.</span><span class="sxs-lookup"><span data-stu-id="7c67c-119">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="7c67c-120">**"Em lançamento"** : em lançamento acontece quando o cliente sai de algumas propriedades.</span><span class="sxs-lookup"><span data-stu-id="7c67c-120">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="7c67c-121">Por exemplo, suponha que o cliente envie o seguinte:</span><span class="sxs-lookup"><span data-stu-id="7c67c-121">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="7c67c-122">Aqui, o cliente não especificou valores para `Price` ou `Weight`.</span><span class="sxs-lookup"><span data-stu-id="7c67c-122">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="7c67c-123">O formatador JSON atribui um valor padrão igual a zero às propriedades ausentes.</span><span class="sxs-lookup"><span data-stu-id="7c67c-123">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="7c67c-124">O estado do modelo é válido, pois zero é um valor válido para essas propriedades.</span><span class="sxs-lookup"><span data-stu-id="7c67c-124">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="7c67c-125">Se esse é um problema depende do seu cenário.</span><span class="sxs-lookup"><span data-stu-id="7c67c-125">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="7c67c-126">Por exemplo, em uma operação de atualização, talvez você queira distinguir entre "zero" e "não definido".</span><span class="sxs-lookup"><span data-stu-id="7c67c-126">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="7c67c-127">Para forçar os clientes a definir um valor, torne a propriedade anulável e defina o atributo **necessário** :</span><span class="sxs-lookup"><span data-stu-id="7c67c-127">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="7c67c-128">**"Excesso de postagem"** : um cliente também pode enviar *mais* dados do que o esperado.</span><span class="sxs-lookup"><span data-stu-id="7c67c-128">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="7c67c-129">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7c67c-129">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="7c67c-130">Aqui, o JSON inclui uma propriedade ("Color") que não existe no modelo de `Product`.</span><span class="sxs-lookup"><span data-stu-id="7c67c-130">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="7c67c-131">Nesse caso, o formatador JSON simplesmente ignora esse valor.</span><span class="sxs-lookup"><span data-stu-id="7c67c-131">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="7c67c-132">(O formatador XML faz o mesmo.) O excesso de postagens causará problemas se seu modelo tiver propriedades que você pretende que sejam somente leitura.</span><span class="sxs-lookup"><span data-stu-id="7c67c-132">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="7c67c-133">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="7c67c-133">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="7c67c-134">Você não quer que os usuários atualizem a propriedade `IsAdmin` e se elevem a administradores!</span><span class="sxs-lookup"><span data-stu-id="7c67c-134">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="7c67c-135">A estratégia mais segura é usar uma classe de modelo que corresponda exatamente ao que o cliente tem permissão para enviar:</span><span class="sxs-lookup"><span data-stu-id="7c67c-135">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="7c67c-136">A postagem do blog de Brad Wilson "[validação de entrada versus validação de modelo no ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" tem uma boa discussão sobre lançamento e excesso de postagem.</span><span class="sxs-lookup"><span data-stu-id="7c67c-136">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="7c67c-137">Embora a postagem seja sobre o ASP.NET MVC 2, os problemas ainda são relevantes para a API da Web.</span><span class="sxs-lookup"><span data-stu-id="7c67c-137">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>

## <a name="handling-validation-errors"></a><span data-ttu-id="7c67c-138">Tratamento de erros de validação</span><span class="sxs-lookup"><span data-stu-id="7c67c-138">Handling Validation Errors</span></span>

<span data-ttu-id="7c67c-139">A API da Web não retorna automaticamente um erro para o cliente quando a validação falha.</span><span class="sxs-lookup"><span data-stu-id="7c67c-139">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="7c67c-140">Cabe à ação do controlador verificar o estado do modelo e responder adequadamente.</span><span class="sxs-lookup"><span data-stu-id="7c67c-140">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="7c67c-141">Você também pode criar um filtro de ação para verificar o estado do modelo antes que a ação do controlador seja invocada.</span><span class="sxs-lookup"><span data-stu-id="7c67c-141">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="7c67c-142">O código a seguir mostra um exemplo:</span><span class="sxs-lookup"><span data-stu-id="7c67c-142">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="7c67c-143">Se a validação do modelo falhar, esse filtro retornará uma resposta HTTP que contém os erros de validação.</span><span class="sxs-lookup"><span data-stu-id="7c67c-143">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="7c67c-144">Nesse caso, a ação do controlador não é invocada.</span><span class="sxs-lookup"><span data-stu-id="7c67c-144">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="7c67c-145">Para aplicar esse filtro a todos os controladores de API da Web, adicione uma instância do filtro à coleção **HttpConfiguration. Filters** durante a configuração:</span><span class="sxs-lookup"><span data-stu-id="7c67c-145">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="7c67c-146">Outra opção é definir o filtro como um atributo em controladores individuais ou ações do controlador:</span><span class="sxs-lookup"><span data-stu-id="7c67c-146">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
