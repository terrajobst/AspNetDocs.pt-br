---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Validação do modelo na API Web ASP.NET - ASP.NET 4.x
author: MikeWasson
description: Visão geral da validação de modelo na API Web ASP.NET para ASP.NET 4. x.
ms.author: riande
ms.date: 07/20/2012
ms.custom: seoapril2019
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: d4e792f8cc2f79c2ab82c5a74fd50f49475fac4f
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59404566"
---
# <a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="bc417-103">Validação do modelo na API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bc417-103">Model Validation in ASP.NET Web API</span></span>

<span data-ttu-id="bc417-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bc417-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="bc417-105">Este artigo mostra como anotar seus modelos, use as anotações para validação de dados e tratar erros de validação em sua API da web.</span><span class="sxs-lookup"><span data-stu-id="bc417-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span> <span data-ttu-id="bc417-106">Quando um cliente envia dados para sua API da web, muitas vezes você deseja validar os dados antes de fazer qualquer processamento.</span><span class="sxs-lookup"><span data-stu-id="bc417-106">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> 

## <a name="data-annotations"></a><span data-ttu-id="bc417-107">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="bc417-107">Data Annotations</span></span>

<span data-ttu-id="bc417-108">Na API Web ASP.NET, você pode usar atributos do [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace para definir regras de validação para propriedades em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="bc417-108">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="bc417-109">Considere o modelo a seguir:</span><span class="sxs-lookup"><span data-stu-id="bc417-109">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="bc417-110">Se você tiver usado a validação do modelo no ASP.NET MVC, isso deve ser familiar.</span><span class="sxs-lookup"><span data-stu-id="bc417-110">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="bc417-111">O **necessária** atributo diz que o `Name` propriedade não pode ser nula.</span><span class="sxs-lookup"><span data-stu-id="bc417-111">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="bc417-112">O **intervalo** atributo diz que `Weight` deve estar entre zero e 999.</span><span class="sxs-lookup"><span data-stu-id="bc417-112">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="bc417-113">Suponha que um cliente envia uma solicitação POST com a seguinte representação JSON:</span><span class="sxs-lookup"><span data-stu-id="bc417-113">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="bc417-114">Você pode ver que o cliente não incluiu o `Name` propriedade, que é marcada como obrigatório.</span><span class="sxs-lookup"><span data-stu-id="bc417-114">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="bc417-115">Quando a API da Web converte o JSON em uma `Product` instância, ele valida o `Product` contra os atributos de validação.</span><span class="sxs-lookup"><span data-stu-id="bc417-115">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="bc417-116">A ação de controlador, você pode verificar se o modelo é válido:</span><span class="sxs-lookup"><span data-stu-id="bc417-116">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="bc417-117">Validação de modelo não garante que os dados de cliente estão seguros.</span><span class="sxs-lookup"><span data-stu-id="bc417-117">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="bc417-118">Validação adicional pode ser necessário em outras camadas do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="bc417-118">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="bc417-119">(Por exemplo, a camada de dados pode impor restrições de chave estrangeira.) O tutorial [usando a API da Web com o Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explora alguns desses problemas.</span><span class="sxs-lookup"><span data-stu-id="bc417-119">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="bc417-120">**"Under-Posting"**: Corrigiremos lançamento ocorre quando o cliente omite algumas propriedades.</span><span class="sxs-lookup"><span data-stu-id="bc417-120">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="bc417-121">Por exemplo, suponha que o cliente envia o seguinte:</span><span class="sxs-lookup"><span data-stu-id="bc417-121">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="bc417-122">Aqui, o cliente não especificou valores para `Price` ou `Weight`.</span><span class="sxs-lookup"><span data-stu-id="bc417-122">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="bc417-123">O formatador JSON atribui um valor padrão de zero para as propriedades ausentes.</span><span class="sxs-lookup"><span data-stu-id="bc417-123">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="bc417-124">O estado do modelo é válido, porque o zero é um valor válido para essas propriedades.</span><span class="sxs-lookup"><span data-stu-id="bc417-124">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="bc417-125">Se esse é um problema depende do seu cenário.</span><span class="sxs-lookup"><span data-stu-id="bc417-125">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="bc417-126">Por exemplo, em uma operação de atualização, convém distinguir entre "zero" e "não definido".</span><span class="sxs-lookup"><span data-stu-id="bc417-126">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="bc417-127">Para forçar os clientes para definir um valor, verifique a propriedade anulável e defina as **necessário** atributo:</span><span class="sxs-lookup"><span data-stu-id="bc417-127">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="bc417-128">**"Over-Posting"**: Um cliente pode também enviar *mais* dados que o esperado.</span><span class="sxs-lookup"><span data-stu-id="bc417-128">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="bc417-129">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="bc417-129">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="bc417-130">Aqui, o JSON inclui uma propriedade ("Color") que não existe no `Product` modelo.</span><span class="sxs-lookup"><span data-stu-id="bc417-130">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="bc417-131">Nesse caso, o formatador JSON simplesmente ignora esse valor.</span><span class="sxs-lookup"><span data-stu-id="bc417-131">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="bc417-132">(O formatador XML faz o mesmo.) Excesso de postagem causa problemas se seu modelo tem propriedades que devem ser somente leitura.</span><span class="sxs-lookup"><span data-stu-id="bc417-132">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="bc417-133">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="bc417-133">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="bc417-134">Você não deseja que os usuários atualizem o `IsAdmin` propriedade e elevar a mesmos para os administradores!</span><span class="sxs-lookup"><span data-stu-id="bc417-134">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="bc417-135">A estratégia mais segura é usar uma classe de modelo que corresponda exatamente o que o cliente tem permissão para enviar:</span><span class="sxs-lookup"><span data-stu-id="bc417-135">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="bc417-136">Postagem de blog de Brad Wilson "[vs de validação de entrada. Modelo de validação no ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"tem uma boa discussão do lançamento insuficiente e excesso de postagem.</span><span class="sxs-lookup"><span data-stu-id="bc417-136">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="bc417-137">Embora a postagem é sobre o ASP.NET MVC 2, os problemas ainda são relevantes para a API Web.</span><span class="sxs-lookup"><span data-stu-id="bc417-137">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>


## <a name="handling-validation-errors"></a><span data-ttu-id="bc417-138">Tratamento de erros de validação</span><span class="sxs-lookup"><span data-stu-id="bc417-138">Handling Validation Errors</span></span>

<span data-ttu-id="bc417-139">API da Web não automaticamente retornar um erro ao cliente quando a validação falha.</span><span class="sxs-lookup"><span data-stu-id="bc417-139">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="bc417-140">Cabe a ação do controlador para verificar o estado do modelo e responder adequadamente.</span><span class="sxs-lookup"><span data-stu-id="bc417-140">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="bc417-141">Você também pode criar um filtro de ação para verificar o estado do modelo antes que a ação do controlador seja invocada.</span><span class="sxs-lookup"><span data-stu-id="bc417-141">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="bc417-142">O código a seguir mostra um exemplo:</span><span class="sxs-lookup"><span data-stu-id="bc417-142">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="bc417-143">Se a validação do modelo falhar, esse filtro retorna uma resposta HTTP que contém os erros de validação.</span><span class="sxs-lookup"><span data-stu-id="bc417-143">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="bc417-144">Nesse caso, a ação do controlador não é invocada.</span><span class="sxs-lookup"><span data-stu-id="bc417-144">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="bc417-145">Para aplicar esse filtro para todos os controladores de API da Web, adicione uma instância do filtro para o **HttpConfiguration.Filters** coleção durante a configuração:</span><span class="sxs-lookup"><span data-stu-id="bc417-145">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="bc417-146">Outra opção é definir o filtro como um atributo em controladores ou ações do controlador individuais:</span><span class="sxs-lookup"><span data-stu-id="bc417-146">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
