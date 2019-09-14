---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Resultados da ação na API Web 2-ASP.NET 4. x
author: MikeWasson
description: Descreve como ASP.NET Web API converte o valor de retorno de uma ação do controlador em uma mensagem de resposta HTTP no ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: f00ac0db453053e53d6d6942dd1557b409f4167b
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985847"
---
# <a name="action-results-in-web-api-2"></a><span data-ttu-id="1c7a8-103">Resultados da ação na API Web 2</span><span class="sxs-lookup"><span data-stu-id="1c7a8-103">Action Results in Web API 2</span></span>

[!INCLUDE[](~/includes/coreWebAPI.md)]

<span data-ttu-id="1c7a8-104">Este tópico descreve como ASP.NET Web API converte o valor de retorno de uma ação do controlador em uma mensagem de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-104">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="1c7a8-105">Uma ação do controlador da API Web pode retornar qualquer um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="1c7a8-105">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="1c7a8-106">void</span><span class="sxs-lookup"><span data-stu-id="1c7a8-106">void</span></span>
2. <span data-ttu-id="1c7a8-107">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="1c7a8-107">**HttpResponseMessage**</span></span>
3. <span data-ttu-id="1c7a8-108">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="1c7a8-108">**IHttpActionResult**</span></span>
4. <span data-ttu-id="1c7a8-109">Algum outro tipo</span><span class="sxs-lookup"><span data-stu-id="1c7a8-109">Some other type</span></span>

<span data-ttu-id="1c7a8-110">Dependendo de quais delas é retornado, a API da Web usa um mecanismo diferente para criar a resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-110">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="1c7a8-111">Tipo de retorno</span><span class="sxs-lookup"><span data-stu-id="1c7a8-111">Return type</span></span> | <span data-ttu-id="1c7a8-112">Como a API da Web cria a resposta</span><span class="sxs-lookup"><span data-stu-id="1c7a8-112">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="1c7a8-113">void</span><span class="sxs-lookup"><span data-stu-id="1c7a8-113">void</span></span> | <span data-ttu-id="1c7a8-114">Retornar 204 vazio (sem conteúdo)</span><span class="sxs-lookup"><span data-stu-id="1c7a8-114">Return empty 204 (No Content)</span></span> |
| <span data-ttu-id="1c7a8-115">**HttpResponseMessage**</span><span class="sxs-lookup"><span data-stu-id="1c7a8-115">**HttpResponseMessage**</span></span> | <span data-ttu-id="1c7a8-116">Converter diretamente em uma mensagem de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-116">Convert directly to an HTTP response message.</span></span> |
| <span data-ttu-id="1c7a8-117">**IHttpActionResult**</span><span class="sxs-lookup"><span data-stu-id="1c7a8-117">**IHttpActionResult**</span></span> | <span data-ttu-id="1c7a8-118">Chame **ExecuteAsync** para criar um **HttpResponseMessage**e, em seguida, converta em uma mensagem de resposta http.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-118">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="1c7a8-119">Outro tipo</span><span class="sxs-lookup"><span data-stu-id="1c7a8-119">Other type</span></span> | <span data-ttu-id="1c7a8-120">Gravar o valor de retorno serializado no corpo da resposta; retornar 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="1c7a8-120">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="1c7a8-121">O restante deste tópico descreve cada opção mais detalhadamente.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-121">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="1c7a8-122">void</span><span class="sxs-lookup"><span data-stu-id="1c7a8-122">void</span></span>

<span data-ttu-id="1c7a8-123">Se o tipo de retorno `void`for, a API Web simplesmente retornará uma resposta http vazia com o código de status 204 (sem conteúdo).</span><span class="sxs-lookup"><span data-stu-id="1c7a8-123">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="1c7a8-124">Controlador de exemplo:</span><span class="sxs-lookup"><span data-stu-id="1c7a8-124">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="1c7a8-125">Resposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="1c7a8-125">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="1c7a8-126">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="1c7a8-126">HttpResponseMessage</span></span>

<span data-ttu-id="1c7a8-127">Se a ação retornar um [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), a API da Web converterá o valor de retorno diretamente em uma mensagem de resposta http, usando as propriedades do objeto **HttpResponseMessage** para popular a resposta.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-127">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="1c7a8-128">Essa opção lhe dá muito controle sobre a mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-128">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="1c7a8-129">Por exemplo, a ação do controlador a seguir define o cabeçalho Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-129">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="1c7a8-130">Responde</span><span class="sxs-lookup"><span data-stu-id="1c7a8-130">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="1c7a8-131">Se você passar um modelo de domínio para o método **CreateResponse** , a API da Web usará um [formatador de mídia](../formats-and-model-binding/media-formatters.md) para gravar o modelo serializado no corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-131">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="1c7a8-132">A API Web usa o cabeçalho Accept na solicitação para escolher o formatador.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-132">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="1c7a8-133">Para obter mais informações, consulte [negociação de conteúdo](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="1c7a8-133">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="1c7a8-134">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="1c7a8-134">IHttpActionResult</span></span>

<span data-ttu-id="1c7a8-135">A interface **IHttpActionResult** foi introduzida na API Web 2.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-135">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="1c7a8-136">Essencialmente, ele define uma fábrica **HttpResponseMessage** .</span><span class="sxs-lookup"><span data-stu-id="1c7a8-136">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="1c7a8-137">Aqui estão algumas vantagens de usar a interface **IHttpActionResult** :</span><span class="sxs-lookup"><span data-stu-id="1c7a8-137">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="1c7a8-138">Simplifica o [teste de unidade](../testing-and-debugging/unit-testing-controllers-in-web-api.md) de seus controladores.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-138">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="1c7a8-139">Move a lógica comum para criar respostas HTTP em classes separadas.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-139">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="1c7a8-140">Torna a intenção da ação do controlador mais clara, ocultando os detalhes de baixo nível da construção da resposta.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-140">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="1c7a8-141">**IHttpActionResult** contém um único método, **ExecuteAsync**, que cria de forma assíncrona uma instância de **HttpResponseMessage** .</span><span class="sxs-lookup"><span data-stu-id="1c7a8-141">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="1c7a8-142">Se uma ação do controlador retornar um **IHttpActionResult**, a API Web chamará o método **ExecuteAsync** para criar um **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-142">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="1c7a8-143">Em seguida, ele converte o **HttpResponseMessage** em uma mensagem de resposta http.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-143">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="1c7a8-144">Aqui está uma implementação simples de **IHttpActionResult** que cria uma resposta de texto sem formatação:</span><span class="sxs-lookup"><span data-stu-id="1c7a8-144">Here is a simple implementation of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="1c7a8-145">Exemplo de ação do controlador:</span><span class="sxs-lookup"><span data-stu-id="1c7a8-145">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="1c7a8-146">Responde</span><span class="sxs-lookup"><span data-stu-id="1c7a8-146">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="1c7a8-147">Com mais frequência, você usa as implementações **IHttpActionResult** definidas no namespace **[System. Web. http. Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="1c7a8-147">More often, you use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="1c7a8-148">A classe **ApiController** define os métodos auxiliares que retornam esses resultados de ação internos.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-148">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="1c7a8-149">No exemplo a seguir, se a solicitação não corresponder a uma ID de produto existente, o controlador chamará [ApiController. não encontrado](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) para criar uma resposta 404 (não encontrada).</span><span class="sxs-lookup"><span data-stu-id="1c7a8-149">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="1c7a8-150">Caso contrário, o controlador chama [ApiController. ok](https://msdn.microsoft.com/library/dn314591.aspx), que cria uma resposta de 200 (OK) que contém o produto.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-150">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="1c7a8-151">Outros tipos de retorno</span><span class="sxs-lookup"><span data-stu-id="1c7a8-151">Other Return Types</span></span>

<span data-ttu-id="1c7a8-152">Para todos os outros tipos de retorno, a API da Web usa um [formatador de mídia](../formats-and-model-binding/media-formatters.md) para serializar o valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-152">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="1c7a8-153">A API da Web grava o valor serializado no corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-153">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="1c7a8-154">O código de status de resposta é 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="1c7a8-154">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="1c7a8-155">Uma desvantagem dessa abordagem é que você não pode retornar diretamente um código de erro, como 404.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-155">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="1c7a8-156">No entanto, você pode gerar um **httpresponseexception** para códigos de erro.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-156">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="1c7a8-157">Para obter mais informações, consulte [tratamento de exceção em ASP.NET Web API](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="1c7a8-157">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="1c7a8-158">A API Web usa o cabeçalho Accept na solicitação para escolher o formatador.</span><span class="sxs-lookup"><span data-stu-id="1c7a8-158">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="1c7a8-159">Para obter mais informações, consulte [negociação de conteúdo](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="1c7a8-159">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="1c7a8-160">Exemplo de solicitação</span><span class="sxs-lookup"><span data-stu-id="1c7a8-160">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="1c7a8-161">Resposta de exemplo</span><span class="sxs-lookup"><span data-stu-id="1c7a8-161">Example response</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
