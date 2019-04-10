---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: Ação resultará na API Web 2 - ASP.NET 4.x
author: MikeWasson
description: Descreve como a API Web ASP.NET converte o valor de retorno de uma ação do controlador em uma mensagem de resposta HTTP no ASP.NET 4. x.
ms.author: riande
ms.date: 02/03/2014
ms.custom: seoapril2019
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 87f71938a5c5f38d3a456ba9339540f67e236e1a
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59400887"
---
# <a name="action-results-in-web-api-2"></a><span data-ttu-id="6b126-103">Resultados da ação na API Web 2</span><span class="sxs-lookup"><span data-stu-id="6b126-103">Action Results in Web API 2</span></span>

<span data-ttu-id="6b126-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6b126-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="6b126-105">Este tópico descreve como a API Web ASP.NET converte o valor de retorno de uma ação do controlador em uma mensagem de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6b126-105">This topic describes how ASP.NET Web API converts the return value from a controller action into an HTTP response message.</span></span>

<span data-ttu-id="6b126-106">Uma ação do controlador API Web pode retornar qualquer um dos seguintes:</span><span class="sxs-lookup"><span data-stu-id="6b126-106">A Web API controller action can return any of the following:</span></span>

1. <span data-ttu-id="6b126-107">void</span><span class="sxs-lookup"><span data-stu-id="6b126-107">void</span></span>
2. **<span data-ttu-id="6b126-108">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="6b126-108">HttpResponseMessage</span></span>**
3. **<span data-ttu-id="6b126-109">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="6b126-109">IHttpActionResult</span></span>**
4. <span data-ttu-id="6b126-110">Algum outro tipo</span><span class="sxs-lookup"><span data-stu-id="6b126-110">Some other type</span></span>

<span data-ttu-id="6b126-111">Dependendo de qual deles é retornada, API Web usa um mecanismo diferente para criar a resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6b126-111">Depending on which of these is returned, Web API uses a different mechanism to create the HTTP response.</span></span>

| <span data-ttu-id="6b126-112">Tipo de retorno</span><span class="sxs-lookup"><span data-stu-id="6b126-112">Return type</span></span> | <span data-ttu-id="6b126-113">Como a API da Web cria a resposta</span><span class="sxs-lookup"><span data-stu-id="6b126-113">How Web API creates the response</span></span> |
| --- | --- |
| <span data-ttu-id="6b126-114">void</span><span class="sxs-lookup"><span data-stu-id="6b126-114">void</span></span> | <span data-ttu-id="6b126-115">Retorna vazio 204 (sem conteúdo)</span><span class="sxs-lookup"><span data-stu-id="6b126-115">Return empty 204 (No Content)</span></span> |
| **<span data-ttu-id="6b126-116">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="6b126-116">HttpResponseMessage</span></span>** | <span data-ttu-id="6b126-117">Converta diretamente em uma mensagem de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6b126-117">Convert directly to an HTTP response message.</span></span> |
| **<span data-ttu-id="6b126-118">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="6b126-118">IHttpActionResult</span></span>** | <span data-ttu-id="6b126-119">Chame **ExecuteAsync** para criar um **HttpResponseMessage**, em seguida, converter em uma mensagem de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6b126-119">Call **ExecuteAsync** to create an **HttpResponseMessage**, then convert to an HTTP response message.</span></span> |
| <span data-ttu-id="6b126-120">Outro tipo</span><span class="sxs-lookup"><span data-stu-id="6b126-120">Other type</span></span> | <span data-ttu-id="6b126-121">Gravar o valor de retorno serializado no corpo da resposta; retorne 200 (Okey).</span><span class="sxs-lookup"><span data-stu-id="6b126-121">Write the serialized return value into the response body; return 200 (OK).</span></span> |

<span data-ttu-id="6b126-122">O restante deste tópico descreve cada opção em mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="6b126-122">The rest of this topic describes each option in more detail.</span></span>

## <a name="void"></a><span data-ttu-id="6b126-123">void</span><span class="sxs-lookup"><span data-stu-id="6b126-123">void</span></span>

<span data-ttu-id="6b126-124">Se o tipo de retorno é `void`, API da Web simplesmente retorna uma resposta HTTP vazia com o código de status 204 (sem conteúdo).</span><span class="sxs-lookup"><span data-stu-id="6b126-124">If the return type is `void`, Web API simply returns an empty HTTP response with status code 204 (No Content).</span></span>

<span data-ttu-id="6b126-125">Controlador de exemplo:</span><span class="sxs-lookup"><span data-stu-id="6b126-125">Example controller:</span></span>

[!code-csharp[Main](action-results/samples/sample1.cs)]

<span data-ttu-id="6b126-126">Resposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="6b126-126">HTTP response:</span></span>

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a><span data-ttu-id="6b126-127">HttpResponseMessage</span><span class="sxs-lookup"><span data-stu-id="6b126-127">HttpResponseMessage</span></span>

<span data-ttu-id="6b126-128">Se a ação retorna um [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), API Web converte o valor de retorno diretamente em uma mensagem de resposta HTTP, usando as propriedades da **HttpResponseMessage** objeto para popular o resposta.</span><span class="sxs-lookup"><span data-stu-id="6b126-128">If the action returns an [HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx), Web API converts the return value directly into an HTTP response message, using the properties of the **HttpResponseMessage** object to populate the response.</span></span>

<span data-ttu-id="6b126-129">Essa opção fornece muito controle sobre a mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="6b126-129">This option gives you a lot of control over the response message.</span></span> <span data-ttu-id="6b126-130">Por exemplo, a ação de controlador a seguir define o cabeçalho Cache-Control.</span><span class="sxs-lookup"><span data-stu-id="6b126-130">For example, the following controller action sets the Cache-Control header.</span></span>

[!code-csharp[Main](action-results/samples/sample3.cs)]

<span data-ttu-id="6b126-131">Resposta:</span><span class="sxs-lookup"><span data-stu-id="6b126-131">Response:</span></span>

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

<span data-ttu-id="6b126-132">Se você passar um modelo de domínio para o **CreateResponse** método, a API da Web usa um [formatador de mídia](../formats-and-model-binding/media-formatters.md) para gravar o modelo serializado no corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="6b126-132">If you pass a domain model to the **CreateResponse** method, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to write the serialized model into the response body.</span></span>

[!code-csharp[Main](action-results/samples/sample5.cs)]

<span data-ttu-id="6b126-133">API da Web usa o cabeçalho Accept na solicitação para escolher o formatador.</span><span class="sxs-lookup"><span data-stu-id="6b126-133">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="6b126-134">Para obter mais informações, consulte [negociação de conteúdo](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="6b126-134">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

## <a name="ihttpactionresult"></a><span data-ttu-id="6b126-135">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="6b126-135">IHttpActionResult</span></span>

<span data-ttu-id="6b126-136">O **IHttpActionResult** interface foi introduzida na API Web 2.</span><span class="sxs-lookup"><span data-stu-id="6b126-136">The **IHttpActionResult** interface was introduced in Web API 2.</span></span> <span data-ttu-id="6b126-137">Essencialmente, ele define uma **HttpResponseMessage** factory.</span><span class="sxs-lookup"><span data-stu-id="6b126-137">Essentially, it defines an **HttpResponseMessage** factory.</span></span> <span data-ttu-id="6b126-138">Aqui estão algumas vantagens de usar o **IHttpActionResult** interface:</span><span class="sxs-lookup"><span data-stu-id="6b126-138">Here are some advantages of using the **IHttpActionResult** interface:</span></span>

- <span data-ttu-id="6b126-139">Simplifica [testes de unidade](../testing-and-debugging/unit-testing-controllers-in-web-api.md) seus controladores.</span><span class="sxs-lookup"><span data-stu-id="6b126-139">Simplifies [unit testing](../testing-and-debugging/unit-testing-controllers-in-web-api.md) your controllers.</span></span>
- <span data-ttu-id="6b126-140">Move a lógica comum para a criação de respostas HTTP em classes separadas.</span><span class="sxs-lookup"><span data-stu-id="6b126-140">Moves common logic for creating HTTP responses into separate classes.</span></span>
- <span data-ttu-id="6b126-141">Torna a intenção da ação de controlador mais clara, ocultando os detalhes de nível baixo de construir a resposta.</span><span class="sxs-lookup"><span data-stu-id="6b126-141">Makes the intent of the controller action clearer, by hiding the low-level details of constructing the response.</span></span>

<span data-ttu-id="6b126-142">**IHttpActionResult** contém um único método, **ExecuteAsync**, que cria de maneira assíncrona uma **HttpResponseMessage** instância.</span><span class="sxs-lookup"><span data-stu-id="6b126-142">**IHttpActionResult** contains a single method, **ExecuteAsync**, which asynchronously creates an **HttpResponseMessage** instance.</span></span>

[!code-csharp[Main](action-results/samples/sample6.cs)]

<span data-ttu-id="6b126-143">Se uma ação do controlador retorna um **IHttpActionResult**, chamadas de API da Web a **ExecuteAsync** método para criar um **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="6b126-143">If a controller action returns an **IHttpActionResult**, Web API calls the **ExecuteAsync** method to create an **HttpResponseMessage**.</span></span> <span data-ttu-id="6b126-144">Em seguida, ele converte o **HttpResponseMessage** em uma mensagem de resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="6b126-144">Then it converts the **HttpResponseMessage** into an HTTP response message.</span></span>

<span data-ttu-id="6b126-145">Aqui está uma implementação simples da **IHttpActionResult** que cria uma resposta de texto sem formatação:</span><span class="sxs-lookup"><span data-stu-id="6b126-145">Here is a simple implementation of **IHttpActionResult** that creates a plain text response:</span></span>

[!code-csharp[Main](action-results/samples/sample7.cs)]

<span data-ttu-id="6b126-146">Ação do controlador de exemplo:</span><span class="sxs-lookup"><span data-stu-id="6b126-146">Example controller action:</span></span>

[!code-csharp[Main](action-results/samples/sample8.cs)]

<span data-ttu-id="6b126-147">Resposta:</span><span class="sxs-lookup"><span data-stu-id="6b126-147">Response:</span></span>

[!code-console[Main](action-results/samples/sample9.cmd)]

<span data-ttu-id="6b126-148">Mais frequentemente, você usará o **IHttpActionResult** implementações definidas na **[Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span><span class="sxs-lookup"><span data-stu-id="6b126-148">More often, you will use the **IHttpActionResult** implementations defined in the **[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** namespace.</span></span> <span data-ttu-id="6b126-149">O **ApiController** classe define os métodos auxiliares que retornam esses resultados de ação interna.</span><span class="sxs-lookup"><span data-stu-id="6b126-149">The **ApiController** class defines helper methods that return these built-in action results.</span></span>

<span data-ttu-id="6b126-150">No exemplo a seguir, se a solicitação não corresponder a uma ID de produto existente, o controlador chama [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) para criar uma resposta 404 (não encontrado).</span><span class="sxs-lookup"><span data-stu-id="6b126-150">In the following example, if the request does not match an existing product ID, the controller calls [ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx) to create a 404 (Not Found) response.</span></span> <span data-ttu-id="6b126-151">Caso contrário, o controlador chama [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), que cria uma resposta de 200 (Okey) que contém o produto.</span><span class="sxs-lookup"><span data-stu-id="6b126-151">Otherwise, the controller calls [ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx), which creates a 200 (OK) response that contains the product.</span></span>

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a><span data-ttu-id="6b126-152">Outros tipos de retorno</span><span class="sxs-lookup"><span data-stu-id="6b126-152">Other Return Types</span></span>

<span data-ttu-id="6b126-153">Para todos os outros tipos de retorno, a API Web usa um [formatador de mídia](../formats-and-model-binding/media-formatters.md) para serializar o valor de retorno.</span><span class="sxs-lookup"><span data-stu-id="6b126-153">For all other return types, Web API uses a [media formatter](../formats-and-model-binding/media-formatters.md) to serialize the return value.</span></span> <span data-ttu-id="6b126-154">API da Web grava o valor serializado no corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="6b126-154">Web API writes the serialized value into the response body.</span></span> <span data-ttu-id="6b126-155">O código de status de resposta é 200 (Okey).</span><span class="sxs-lookup"><span data-stu-id="6b126-155">The response status code is 200 (OK).</span></span>

[!code-csharp[Main](action-results/samples/sample11.cs)]

<span data-ttu-id="6b126-156">Uma desvantagem dessa abordagem é que você não pode retornar diretamente um código de erro, como 404.</span><span class="sxs-lookup"><span data-stu-id="6b126-156">A disadvantage of this approach is that you cannot directly return an error code, such as 404.</span></span> <span data-ttu-id="6b126-157">No entanto, você pode lançar uma **HttpResponseException** para códigos de erro.</span><span class="sxs-lookup"><span data-stu-id="6b126-157">However, you can throw an **HttpResponseException** for error codes.</span></span> <span data-ttu-id="6b126-158">Para obter mais informações, consulte [tratamento de exceções em API Web ASP.NET](../error-handling/exception-handling.md).</span><span class="sxs-lookup"><span data-stu-id="6b126-158">For more information, see [Exception Handling in ASP.NET Web API](../error-handling/exception-handling.md).</span></span>

<span data-ttu-id="6b126-159">API da Web usa o cabeçalho Accept na solicitação para escolher o formatador.</span><span class="sxs-lookup"><span data-stu-id="6b126-159">Web API uses the Accept header in the request to choose the formatter.</span></span> <span data-ttu-id="6b126-160">Para obter mais informações, consulte [negociação de conteúdo](../formats-and-model-binding/content-negotiation.md).</span><span class="sxs-lookup"><span data-stu-id="6b126-160">For more information, see [Content Negotiation](../formats-and-model-binding/content-negotiation.md).</span></span>

<span data-ttu-id="6b126-161">Exemplo de solicitação</span><span class="sxs-lookup"><span data-stu-id="6b126-161">Example request</span></span>

[!code-console[Main](action-results/samples/sample12.cmd)]

<span data-ttu-id="6b126-162">Resposta de exemplo:</span><span class="sxs-lookup"><span data-stu-id="6b126-162">Example response:</span></span>

[!code-console[Main](action-results/samples/sample13.cmd)]
