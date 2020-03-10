---
uid: web-api/overview/error-handling/exception-handling
title: Manipulação de exceção em ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: ''
ms.author: riande
ms.date: 03/12/2012
ms.custom: seoapril2019
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: dbdbab6aefec840e2fec9e9cd33f3d124093750e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622314"
---
# <a name="exception-handling-in-aspnet-web-api"></a><span data-ttu-id="4cba1-102">Manipulação de exceção no ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="4cba1-102">Exception Handling in ASP.NET Web API</span></span>

<span data-ttu-id="4cba1-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4cba1-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4cba1-104">Este artigo descreve o tratamento de erros e exceções no ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="4cba1-104">This article describes error and exception handling in ASP.NET Web API.</span></span>

- [<span data-ttu-id="4cba1-105">HttpResponseexception</span><span class="sxs-lookup"><span data-stu-id="4cba1-105">HttpResponseException</span></span>](#httpresponserexception)
- [<span data-ttu-id="4cba1-106">Filtros de exceção</span><span class="sxs-lookup"><span data-stu-id="4cba1-106">Exception Filters</span></span>](#exception_filters)
- [<span data-ttu-id="4cba1-107">Registrando filtros de exceção</span><span class="sxs-lookup"><span data-stu-id="4cba1-107">Registering Exception Filters</span></span>](#registering_exception_filters)
- [<span data-ttu-id="4cba1-108">HttpError</span><span class="sxs-lookup"><span data-stu-id="4cba1-108">HttpError</span></span>](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a><span data-ttu-id="4cba1-109">HttpResponseException</span><span class="sxs-lookup"><span data-stu-id="4cba1-109">HttpResponseException</span></span>

<span data-ttu-id="4cba1-110">O que acontece se um controlador da API da Web gerar uma exceção não capturada?</span><span class="sxs-lookup"><span data-stu-id="4cba1-110">What happens if a Web API controller throws an uncaught exception?</span></span> <span data-ttu-id="4cba1-111">Por padrão, a maioria das exceções são convertidas em uma resposta HTTP com o código de status 500, erro interno do servidor.</span><span class="sxs-lookup"><span data-stu-id="4cba1-111">By default, most exceptions are translated into an HTTP response with status code 500, Internal Server Error.</span></span>

<span data-ttu-id="4cba1-112">O tipo **httpresponseexception** é um caso especial.</span><span class="sxs-lookup"><span data-stu-id="4cba1-112">The **HttpResponseException** type is a special case.</span></span> <span data-ttu-id="4cba1-113">Essa exceção retorna qualquer código de status HTTP que você especificar no construtor de exceção.</span><span class="sxs-lookup"><span data-stu-id="4cba1-113">This exception returns any HTTP status code that you specify in the exception constructor.</span></span> <span data-ttu-id="4cba1-114">Por exemplo, o método a seguir retorna 404, não encontrado, se o parâmetro *ID* não for válido.</span><span class="sxs-lookup"><span data-stu-id="4cba1-114">For example, the following method returns 404, Not Found, if the *id* parameter is not valid.</span></span>

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

<span data-ttu-id="4cba1-115">Para obter mais controle sobre a resposta, você também pode construir a mensagem de resposta inteira e incluí-la com o **httpresponseexception:**</span><span class="sxs-lookup"><span data-stu-id="4cba1-115">For more control over the response, you can also construct the entire response message and include it with the **HttpResponseException:**</span></span> 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a><span data-ttu-id="4cba1-116">Filtros de exceção</span><span class="sxs-lookup"><span data-stu-id="4cba1-116">Exception Filters</span></span>

<span data-ttu-id="4cba1-117">Você pode personalizar como a API da Web lida com exceções escrevendo um *filtro de exceção*.</span><span class="sxs-lookup"><span data-stu-id="4cba1-117">You can customize how Web API handles exceptions by writing an *exception filter*.</span></span> <span data-ttu-id="4cba1-118">Um filtro de exceção é executado quando um método de controlador gera qualquer exceção sem tratamento que *não* seja uma exceção **httpresponseexception** .</span><span class="sxs-lookup"><span data-stu-id="4cba1-118">An exception filter is executed when a controller method throws any unhandled exception that is *not* an **HttpResponseException** exception.</span></span> <span data-ttu-id="4cba1-119">O tipo **httpresponseexception** é um caso especial, pois ele foi projetado especificamente para retornar uma resposta http.</span><span class="sxs-lookup"><span data-stu-id="4cba1-119">The **HttpResponseException** type is a special case, because it is designed specifically for returning an HTTP response.</span></span>

<span data-ttu-id="4cba1-120">Os filtros de exceção implementam a interface **System. Web. http. Filters. IExceptionFilter** .</span><span class="sxs-lookup"><span data-stu-id="4cba1-120">Exception filters implement the **System.Web.Http.Filters.IExceptionFilter** interface.</span></span> <span data-ttu-id="4cba1-121">A maneira mais simples de escrever um filtro de exceção é derivar da classe **System. Web. http. Filters. ExceptionFilterAttribute** e substituir o método **OnException** .</span><span class="sxs-lookup"><span data-stu-id="4cba1-121">The simplest way to write an exception filter is to derive from the **System.Web.Http.Filters.ExceptionFilterAttribute** class and override the **OnException** method.</span></span>

> [!NOTE]
> <span data-ttu-id="4cba1-122">Filtros de exceção no ASP.NET Web API são semelhantes aos do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="4cba1-122">Exception filters in ASP.NET Web API are similar to those in ASP.NET MVC.</span></span> <span data-ttu-id="4cba1-123">No entanto, eles são declarados em um namespace e uma função separados separadamente.</span><span class="sxs-lookup"><span data-stu-id="4cba1-123">However, they are declared in a separate namespace and function separately.</span></span> <span data-ttu-id="4cba1-124">Em particular, a classe **HandleErrorAttribute** usada no MVC não trata as exceções geradas pelos controladores de API da Web.</span><span class="sxs-lookup"><span data-stu-id="4cba1-124">In particular, the **HandleErrorAttribute** class used in MVC does not handle exceptions thrown by Web API controllers.</span></span>

<span data-ttu-id="4cba1-125">Aqui está um filtro que converte exceções **NotImplementedException** no código de status HTTP 501, não implementado:</span><span class="sxs-lookup"><span data-stu-id="4cba1-125">Here is a filter that converts **NotImplementedException** exceptions into HTTP status code 501, Not Implemented:</span></span>

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

<span data-ttu-id="4cba1-126">A propriedade **Response** do objeto **HttpActionExecutedContext** contém a mensagem de resposta http que será enviada ao cliente.</span><span class="sxs-lookup"><span data-stu-id="4cba1-126">The **Response** property of the **HttpActionExecutedContext** object contains the HTTP response message that will be sent to the client.</span></span>

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a><span data-ttu-id="4cba1-127">Registrando filtros de exceção</span><span class="sxs-lookup"><span data-stu-id="4cba1-127">Registering Exception Filters</span></span>

<span data-ttu-id="4cba1-128">Há várias maneiras de registrar um filtro de exceção da API Web:</span><span class="sxs-lookup"><span data-stu-id="4cba1-128">There are several ways to register a Web API exception filter:</span></span>

- <span data-ttu-id="4cba1-129">por ação</span><span class="sxs-lookup"><span data-stu-id="4cba1-129">By action</span></span>
- <span data-ttu-id="4cba1-130">pelo controlador</span><span class="sxs-lookup"><span data-stu-id="4cba1-130">By controller</span></span>
- <span data-ttu-id="4cba1-131">globalmente</span><span class="sxs-lookup"><span data-stu-id="4cba1-131">Globally</span></span>

<span data-ttu-id="4cba1-132">Para aplicar o filtro em uma ação específica, adicione o filtro como um atributo à ação:</span><span class="sxs-lookup"><span data-stu-id="4cba1-132">To apply the filter to a specific action, add the filter as an attribute to the action:</span></span>

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

<span data-ttu-id="4cba1-133">Para aplicar o filtro a todas as ações em um controlador, adicione o filtro como um atributo à classe do controlador:</span><span class="sxs-lookup"><span data-stu-id="4cba1-133">To apply the filter to all of the actions on a controller, add the filter as an attribute to the controller class:</span></span>

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

<span data-ttu-id="4cba1-134">Para aplicar o filtro globalmente a todos os controladores de API da Web, adicione uma instância do filtro à coleção **GlobalConfiguration. Configuration. Filters** .</span><span class="sxs-lookup"><span data-stu-id="4cba1-134">To apply the filter globally to all Web API controllers, add an instance of the filter to the **GlobalConfiguration.Configuration.Filters** collection.</span></span> <span data-ttu-id="4cba1-135">Os filtros de exceção nesta coleção aplicam-se a qualquer ação do controlador da API Web.</span><span class="sxs-lookup"><span data-stu-id="4cba1-135">Exception filters in this collection apply to any Web API controller action.</span></span>

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

<span data-ttu-id="4cba1-136">Se você usar o modelo de projeto "aplicativo Web ASP.NET MVC 4" para criar seu projeto, coloque o código de configuração da API Web dentro da classe `WebApiConfig`, localizada na pasta de início do aplicativo\_:</span><span class="sxs-lookup"><span data-stu-id="4cba1-136">If you use the "ASP.NET MVC 4 Web Application" project template to create your project, put your Web API configuration code inside the `WebApiConfig` class, which is located in the App\_Start folder:</span></span>

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a><span data-ttu-id="4cba1-137">HttpError</span><span class="sxs-lookup"><span data-stu-id="4cba1-137">HttpError</span></span>

<span data-ttu-id="4cba1-138">O objeto **HttpError** fornece uma maneira consistente de retornar informações de erro no corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="4cba1-138">The **HttpError** object provides a consistent way to return error information in the response body.</span></span> <span data-ttu-id="4cba1-139">O exemplo a seguir mostra como retornar o código de status HTTP 404 (não encontrado) com um **HttpError** no corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="4cba1-139">The following example shows how to return HTTP status code 404 (Not Found) with an **HttpError** in the response body.</span></span>

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

<span data-ttu-id="4cba1-140">**CreateErrorResponse** é um método de extensão definido na classe **System .net. http. HttpRequestMessageExtensions** .</span><span class="sxs-lookup"><span data-stu-id="4cba1-140">**CreateErrorResponse** is an extension method defined in the **System.Net.Http.HttpRequestMessageExtensions** class.</span></span> <span data-ttu-id="4cba1-141">Internamente, o **CreateErrorResponse** cria uma instância de **HttpError** e, em seguida, cria um **HttpResponseMessage** que contém o **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="4cba1-141">Internally, **CreateErrorResponse** creates an **HttpError** instance and then creates an **HttpResponseMessage** that contains the **HttpError**.</span></span>

<span data-ttu-id="4cba1-142">Neste exemplo, se o método for bem-sucedido, ele retornará o produto na resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="4cba1-142">In this example, if the method is successful, it returns the product in the HTTP response.</span></span> <span data-ttu-id="4cba1-143">Mas se o produto solicitado não for encontrado, a resposta HTTP conterá um **HttpError** no corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="4cba1-143">But if the requested product is not found, the HTTP response contains an **HttpError** in the request body.</span></span> <span data-ttu-id="4cba1-144">A resposta pode ser semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="4cba1-144">The response might look like the following:</span></span>

[!code-console[Main](exception-handling/samples/sample9.cmd)]

<span data-ttu-id="4cba1-145">Observe que o **HttpError** foi SERIALIZADO para JSON neste exemplo.</span><span class="sxs-lookup"><span data-stu-id="4cba1-145">Notice that the **HttpError** was serialized to JSON in this example.</span></span> <span data-ttu-id="4cba1-146">Uma vantagem de usar o **HttpError** é que ele passa pela mesma [negociação de conteúdo](../formats-and-model-binding/content-negotiation.md) e processo de serialização que qualquer outro modelo fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="4cba1-146">One advantage of using **HttpError** is that it goes through the same [content-negotiation](../formats-and-model-binding/content-negotiation.md) and serialization process as any other strongly-typed model.</span></span>

### <a name="httperror-and-model-validation"></a><span data-ttu-id="4cba1-147">HttpError e validação de modelo</span><span class="sxs-lookup"><span data-stu-id="4cba1-147">HttpError and Model Validation</span></span>

<span data-ttu-id="4cba1-148">Para a validação do modelo, você pode passar o estado do modelo para **CreateErrorResponse**, para incluir os erros de validação na resposta:</span><span class="sxs-lookup"><span data-stu-id="4cba1-148">For model validation, you can pass the model state to **CreateErrorResponse**, to include the validation errors in the response:</span></span>

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

<span data-ttu-id="4cba1-149">Este exemplo pode retornar a seguinte resposta:</span><span class="sxs-lookup"><span data-stu-id="4cba1-149">This example might return the following response:</span></span>

[!code-console[Main](exception-handling/samples/sample11.cmd)]

<span data-ttu-id="4cba1-150">Para obter mais informações sobre a validação de modelo, consulte [validação de modelo em ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="4cba1-150">For more information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>

### <a name="using-httperror-with-httpresponseexception"></a><span data-ttu-id="4cba1-151">Usando HttpError com HttpResponseexception</span><span class="sxs-lookup"><span data-stu-id="4cba1-151">Using HttpError with HttpResponseException</span></span>

<span data-ttu-id="4cba1-152">Os exemplos anteriores retornam uma mensagem **HttpResponseMessage** da ação do controlador, mas você também pode usar **httpresponseexception** para retornar um **HttpError**.</span><span class="sxs-lookup"><span data-stu-id="4cba1-152">The previous examples return an **HttpResponseMessage** message from the controller action, but you can also use **HttpResponseException** to return an **HttpError**.</span></span> <span data-ttu-id="4cba1-153">Isso permite que você retorne um modelo fortemente tipado no caso de êxito normal, enquanto ainda retorna **HttpError** se houver um erro:</span><span class="sxs-lookup"><span data-stu-id="4cba1-153">This lets you return a strongly-typed model in the normal success case, while still returning **HttpError** if there is an error:</span></span>

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
