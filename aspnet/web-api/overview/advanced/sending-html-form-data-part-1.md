---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Enviando dados de formulário HTML em ASP.NET Web API: formulário-urlencoded data-ASP.NET 4. x'
author: MikeWasson
description: Este artigo mostra como postar dados de forma urlencoded em um controlador de API da Web com ASP.NET 4. x
ms.author: riande
ms.date: 06/15/2012
ms.custom: seoapril2019
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 7243069dbd8051b1374ed6e0112c273b8fe26f61
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557599"
---
# <a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="c2e21-103">Enviando dados de formulário HTML em ASP.NET Web API: formulário-dados urlencoded</span><span class="sxs-lookup"><span data-stu-id="c2e21-103">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>

<span data-ttu-id="c2e21-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c2e21-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="c2e21-105">Parte 1: formulário – dados urlencoded</span><span class="sxs-lookup"><span data-stu-id="c2e21-105">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="c2e21-106">Este artigo mostra como postar dados de formulário urlencoded em um controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="c2e21-106">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="c2e21-107">Visão geral de formulários HTML</span><span class="sxs-lookup"><span data-stu-id="c2e21-107">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="c2e21-108">Enviando tipos complexos</span><span class="sxs-lookup"><span data-stu-id="c2e21-108">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="c2e21-109">Enviando dados de formulário via AJAX</span><span class="sxs-lookup"><span data-stu-id="c2e21-109">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="c2e21-110">Enviando tipos simples</span><span class="sxs-lookup"><span data-stu-id="c2e21-110">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="c2e21-111">[Baixe o projeto concluído](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="c2e21-111">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>

<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="c2e21-112">Visão geral de formulários HTML</span><span class="sxs-lookup"><span data-stu-id="c2e21-112">Overview of HTML Forms</span></span>

<span data-ttu-id="c2e21-113">Os formulários HTML usam GET ou POST para enviar dados ao servidor.</span><span class="sxs-lookup"><span data-stu-id="c2e21-113">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="c2e21-114">O atributo **Method** do elemento **Form** fornece o método http:</span><span class="sxs-lookup"><span data-stu-id="c2e21-114">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="c2e21-115">O método padrão é GET.</span><span class="sxs-lookup"><span data-stu-id="c2e21-115">The default method is GET.</span></span> <span data-ttu-id="c2e21-116">Se o formulário usar GET, os dados do formulário serão codificados no URI como uma cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="c2e21-116">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="c2e21-117">Se o formulário usar POST, os dados do formulário serão colocados no corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="c2e21-117">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="c2e21-118">Para dados postados, o atributo **Enctype** especifica o formato do corpo da solicitação:</span><span class="sxs-lookup"><span data-stu-id="c2e21-118">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="c2e21-119">Enctype</span><span class="sxs-lookup"><span data-stu-id="c2e21-119">enctype</span></span> | <span data-ttu-id="c2e21-120">DESCRIÇÃO</span><span class="sxs-lookup"><span data-stu-id="c2e21-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c2e21-121">Application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="c2e21-121">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="c2e21-122">Os dados de formulário são codificados como pares de nome/valor, semelhante a uma cadeia de caracteres de consulta de URI.</span><span class="sxs-lookup"><span data-stu-id="c2e21-122">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="c2e21-123">Este é o formato padrão para POST.</span><span class="sxs-lookup"><span data-stu-id="c2e21-123">This is the default format for POST.</span></span> |
| <span data-ttu-id="c2e21-124">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="c2e21-124">multipart/form-data</span></span> | <span data-ttu-id="c2e21-125">Os dados de formulário são codificados como uma mensagem MIME de várias partes.</span><span class="sxs-lookup"><span data-stu-id="c2e21-125">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="c2e21-126">Use esse formato se você estiver carregando um arquivo para o servidor.</span><span class="sxs-lookup"><span data-stu-id="c2e21-126">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="c2e21-127">A parte 1 deste artigo analisa o formato x-www-form-urlencoded.</span><span class="sxs-lookup"><span data-stu-id="c2e21-127">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="c2e21-128">A [parte 2](sending-html-form-data-part-2.md) descreve o MIME de várias partes.</span><span class="sxs-lookup"><span data-stu-id="c2e21-128">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="c2e21-129">Enviando tipos complexos</span><span class="sxs-lookup"><span data-stu-id="c2e21-129">Sending Complex Types</span></span>

<span data-ttu-id="c2e21-130">Normalmente, você enviará um tipo complexo, composto por valores extraídos de vários controles de formulário.</span><span class="sxs-lookup"><span data-stu-id="c2e21-130">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="c2e21-131">Considere o seguinte modelo que representa uma atualização de status:</span><span class="sxs-lookup"><span data-stu-id="c2e21-131">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="c2e21-132">Aqui está um controlador de API da Web que aceita um objeto `Update` por meio de POST.</span><span class="sxs-lookup"><span data-stu-id="c2e21-132">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="c2e21-133">Esse controlador usa o [roteamento baseado em ação](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), portanto, o modelo de rota é &quot;API/{Controller}/{Action}/{id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="c2e21-133">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="c2e21-134">O cliente irá postar os dados para &quot;/API/updates/Complex&quot;.</span><span class="sxs-lookup"><span data-stu-id="c2e21-134">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>

<span data-ttu-id="c2e21-135">Agora, vamos escrever um formulário HTML para que os usuários enviem uma atualização de status.</span><span class="sxs-lookup"><span data-stu-id="c2e21-135">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="c2e21-136">Observe que o atributo **Action** no formulário é o URI da nossa ação do controlador.</span><span class="sxs-lookup"><span data-stu-id="c2e21-136">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="c2e21-137">Aqui está o formulário com alguns valores inseridos em:</span><span class="sxs-lookup"><span data-stu-id="c2e21-137">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="c2e21-138">Quando o usuário clica em enviar, o navegador envia uma solicitação HTTP semelhante à seguinte:</span><span class="sxs-lookup"><span data-stu-id="c2e21-138">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="c2e21-139">Observe que o corpo da solicitação contém os dados do formulário, formatados como pares de nome/valor.</span><span class="sxs-lookup"><span data-stu-id="c2e21-139">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="c2e21-140">A API da Web converte automaticamente os pares de nome/valor em uma instância da classe `Update`.</span><span class="sxs-lookup"><span data-stu-id="c2e21-140">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="c2e21-141">Enviando dados de formulário via AJAX</span><span class="sxs-lookup"><span data-stu-id="c2e21-141">Sending Form Data via AJAX</span></span>

<span data-ttu-id="c2e21-142">Quando um usuário envia um formulário, o navegador navega para fora da página atual e renderiza o corpo da mensagem de resposta.</span><span class="sxs-lookup"><span data-stu-id="c2e21-142">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="c2e21-143">Tudo bem quando a resposta é uma página HTML.</span><span class="sxs-lookup"><span data-stu-id="c2e21-143">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="c2e21-144">Com uma API Web, no entanto, o corpo da resposta geralmente está vazio ou contém dados estruturados, como JSON.</span><span class="sxs-lookup"><span data-stu-id="c2e21-144">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="c2e21-145">Nesse caso, faz mais sentido enviar os dados do formulário usando uma solicitação AJAX, para que a página possa processar a resposta.</span><span class="sxs-lookup"><span data-stu-id="c2e21-145">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="c2e21-146">O código a seguir mostra como postar dados de formulário usando o jQuery.</span><span class="sxs-lookup"><span data-stu-id="c2e21-146">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="c2e21-147">A função de **envio** do jQuery substitui a ação de formulário por uma nova função.</span><span class="sxs-lookup"><span data-stu-id="c2e21-147">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="c2e21-148">Isso substitui o comportamento padrão do botão enviar.</span><span class="sxs-lookup"><span data-stu-id="c2e21-148">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="c2e21-149">A função **Serialize** serializa os dados do formulário em pares de nome/valor.</span><span class="sxs-lookup"><span data-stu-id="c2e21-149">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="c2e21-150">Para enviar os dados do formulário para o servidor, chame `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="c2e21-150">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="c2e21-151">Quando a solicitação for concluída, o manipulador `.success()` ou `.error()` exibirá uma mensagem apropriada para o usuário.</span><span class="sxs-lookup"><span data-stu-id="c2e21-151">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="c2e21-152">Enviando tipos simples</span><span class="sxs-lookup"><span data-stu-id="c2e21-152">Sending Simple Types</span></span>

<span data-ttu-id="c2e21-153">Nas seções anteriores, enviamos um tipo complexo, que API da Web é desserializada para uma instância de uma classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="c2e21-153">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="c2e21-154">Você também pode enviar tipos simples, como uma cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="c2e21-154">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="c2e21-155">Antes de enviar um tipo simples, considere encapsular o valor em um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="c2e21-155">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="c2e21-156">Isso oferece os benefícios da validação de modelo no lado do servidor e torna mais fácil estender seu modelo, se necessário.</span><span class="sxs-lookup"><span data-stu-id="c2e21-156">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>

<span data-ttu-id="c2e21-157">As etapas básicas para enviar um tipo simples são as mesmas, mas há duas diferenças sutis.</span><span class="sxs-lookup"><span data-stu-id="c2e21-157">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="c2e21-158">Primeiro, no controlador, você deve decorar o nome do parâmetro com o atributo **FromBody** .</span><span class="sxs-lookup"><span data-stu-id="c2e21-158">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="c2e21-159">Por padrão, a API Web tenta obter tipos simples do URI de solicitação.</span><span class="sxs-lookup"><span data-stu-id="c2e21-159">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="c2e21-160">O atributo **FromBody** informa à API da Web para ler o valor do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="c2e21-160">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="c2e21-161">A API Web lê o corpo da resposta no máximo uma vez, de modo que apenas um parâmetro de uma ação pode vir do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="c2e21-161">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="c2e21-162">Se você precisar obter vários valores do corpo da solicitação, defina um tipo complexo.</span><span class="sxs-lookup"><span data-stu-id="c2e21-162">If you need to get multiple values from the request body, define a complex type.</span></span>

<span data-ttu-id="c2e21-163">Em segundo lugar, o cliente precisa enviar o valor com o seguinte formato:</span><span class="sxs-lookup"><span data-stu-id="c2e21-163">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="c2e21-164">Especificamente, a parte do nome do par nome/valor deve estar vazia para um tipo simples.</span><span class="sxs-lookup"><span data-stu-id="c2e21-164">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="c2e21-165">Nem todos os navegadores dão suporte a isso para formulários HTML, mas você cria esse formato em script da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="c2e21-165">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="c2e21-166">Veja um exemplo de formulário:</span><span class="sxs-lookup"><span data-stu-id="c2e21-166">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="c2e21-167">E aqui está o script para enviar o valor do formulário.</span><span class="sxs-lookup"><span data-stu-id="c2e21-167">And here is the script to submit the form value.</span></span> <span data-ttu-id="c2e21-168">A única diferença do script anterior é o argumento passado para a função **post** .</span><span class="sxs-lookup"><span data-stu-id="c2e21-168">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="c2e21-169">Você pode usar a mesma abordagem para enviar uma matriz de tipos simples:</span><span class="sxs-lookup"><span data-stu-id="c2e21-169">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="c2e21-170">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="c2e21-170">Additional Resources</span></span>

[<span data-ttu-id="c2e21-171">Parte 2: carregamento de arquivo e MIME de várias partes</span><span class="sxs-lookup"><span data-stu-id="c2e21-171">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
