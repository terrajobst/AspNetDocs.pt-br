---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Roteamento e seleção de ação no ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 12/14/2018
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 62114e56fb29e80c93b82dcb78ce2bc2a123a83b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554883"
---
# <a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="5f0d5-102">Roteamento e seleção de ação no ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="5f0d5-102">Routing and Action Selection in ASP.NET Web API</span></span>

<span data-ttu-id="5f0d5-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5f0d5-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5f0d5-104">Este artigo descreve como ASP.NET Web API roteia uma solicitação HTTP para uma ação específica em um controlador.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="5f0d5-105">Para obter uma visão geral de alto nível do roteamento, consulte [Roteamento no ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="5f0d5-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="5f0d5-106">Este artigo analisa os detalhes do processo de roteamento.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="5f0d5-107">Se você criar um projeto de API Web e achar que algumas solicitações não são roteadas da maneira esperada, espero que este artigo o ajude.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="5f0d5-108">O roteamento tem três fases principais:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="5f0d5-109">Correspondendo o URI a um modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="5f0d5-110">Selecionando um controlador.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-110">Selecting a controller.</span></span>
3. <span data-ttu-id="5f0d5-111">Selecionando uma ação.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-111">Selecting an action.</span></span>

<span data-ttu-id="5f0d5-112">Você pode substituir algumas partes do processo por seus próprios comportamentos personalizados.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="5f0d5-113">Neste artigo, descrevo o comportamento padrão.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="5f0d5-114">No final, observei os locais em que você pode personalizar o comportamento.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="5f0d5-115">Modelos de rota</span><span class="sxs-lookup"><span data-stu-id="5f0d5-115">Route Templates</span></span>

<span data-ttu-id="5f0d5-116">Um modelo de rota é semelhante a um caminho de URI, mas pode ter valores de espaço reservado, indicado com chaves:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="5f0d5-117">Ao criar uma rota, você pode fornecer valores padrão para alguns ou todos os espaços reservados:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="5f0d5-118">Você também pode fornecer restrições, que restringem como um segmento URI pode corresponder a um espaço reservado:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="5f0d5-119">A estrutura tenta corresponder os segmentos no caminho do URI para o modelo.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="5f0d5-120">Os literais no modelo devem corresponder exatamente.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="5f0d5-121">Um espaço reservado corresponde a qualquer valor, a menos que você especifique restrições.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="5f0d5-122">A estrutura não corresponde a outras partes do URI, como o nome do host ou os parâmetros de consulta.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="5f0d5-123">A estrutura seleciona a primeira rota na tabela de rotas que corresponde ao URI.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="5f0d5-124">Há dois espaços reservados especiais: "{Controller}" e "{Action}".</span><span class="sxs-lookup"><span data-stu-id="5f0d5-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="5f0d5-125">"{Controller}" fornece o nome do controlador.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="5f0d5-126">"{Action}" fornece o nome da ação.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="5f0d5-127">Na API Web, a convenção usual é omitir "{Action}".</span><span class="sxs-lookup"><span data-stu-id="5f0d5-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="5f0d5-128">Padrões</span><span class="sxs-lookup"><span data-stu-id="5f0d5-128">Defaults</span></span>

<span data-ttu-id="5f0d5-129">Se você fornecer padrões, a rota corresponderá a um URI que não tem esses segmentos.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="5f0d5-130">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="5f0d5-131">Os URIs `http://localhost/api/products/all` e `http://localhost/api/products` correspondem à rota anterior.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-131">The URIs `http://localhost/api/products/all` and `http://localhost/api/products` match the preceding route.</span></span> <span data-ttu-id="5f0d5-132">No último URI, o valor padrão `all`é atribuído ao segmento de `{category}` ausente.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-132">In the latter URI, the missing `{category}` segment is assigned the default value `all`.</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="5f0d5-133">Dicionário de rotas</span><span class="sxs-lookup"><span data-stu-id="5f0d5-133">Route Dictionary</span></span>

<span data-ttu-id="5f0d5-134">Se a estrutura encontrar uma correspondência para um URI, ela criará um dicionário que contém o valor de cada espaço reservado.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="5f0d5-135">As chaves são os nomes de espaço reservado, não incluindo as chaves.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="5f0d5-136">Os valores são extraídos do caminho do URI ou dos padrões.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="5f0d5-137">O dicionário é armazenado no objeto **IHttpRouteData** .</span><span class="sxs-lookup"><span data-stu-id="5f0d5-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="5f0d5-138">Durante essa fase de correspondência de rota, os espaços reservados especiais "{Controller}" e "{Action}" são tratados exatamente como os outros espaços reservados.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="5f0d5-139">Eles são simplesmente armazenados no dicionário com os outros valores.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="5f0d5-140">Um padrão pode ter o valor especial **RouteParameter. optional**.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="5f0d5-141">Se um espaço reservado receber esse valor, o valor não será adicionado ao dicionário de rota.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="5f0d5-142">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="5f0d5-143">Para o caminho do URI "API/produtos", o dicionário da rota conterá:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="5f0d5-144">controlador: "produtos"</span><span class="sxs-lookup"><span data-stu-id="5f0d5-144">controller: "products"</span></span>
- <span data-ttu-id="5f0d5-145">Categoria: "todos"</span><span class="sxs-lookup"><span data-stu-id="5f0d5-145">category: "all"</span></span>

<span data-ttu-id="5f0d5-146">Para "API/produtos/Toys/123", no entanto, o dicionário de rota conterá:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="5f0d5-147">controlador: "produtos"</span><span class="sxs-lookup"><span data-stu-id="5f0d5-147">controller: "products"</span></span>
- <span data-ttu-id="5f0d5-148">Categoria: "brinquedos"</span><span class="sxs-lookup"><span data-stu-id="5f0d5-148">category: "toys"</span></span>
- <span data-ttu-id="5f0d5-149">id: "123"</span><span class="sxs-lookup"><span data-stu-id="5f0d5-149">id: "123"</span></span>

<span data-ttu-id="5f0d5-150">Os padrões também podem incluir um valor que não aparece em nenhum lugar no modelo de rota.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="5f0d5-151">Se a rota corresponder, esse valor será armazenado no dicionário.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="5f0d5-152">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="5f0d5-153">Se o caminho do URI for "API/root/8", o dicionário conterá dois valores:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="5f0d5-154">controlador: "clientes"</span><span class="sxs-lookup"><span data-stu-id="5f0d5-154">controller: "customers"</span></span>
- <span data-ttu-id="5f0d5-155">id: "8"</span><span class="sxs-lookup"><span data-stu-id="5f0d5-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="5f0d5-156">Selecionando um controlador</span><span class="sxs-lookup"><span data-stu-id="5f0d5-156">Selecting a Controller</span></span>

<span data-ttu-id="5f0d5-157">A seleção do controlador é tratada pelo método **IHttpControllerSelector. SelectController** .</span><span class="sxs-lookup"><span data-stu-id="5f0d5-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="5f0d5-158">Esse método usa uma instância **HttpRequestMessage** e retorna um **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="5f0d5-159">A implementação padrão é fornecida pela classe **DefaultHttpControllerSelector** .</span><span class="sxs-lookup"><span data-stu-id="5f0d5-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="5f0d5-160">Essa classe usa um algoritmo simples:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="5f0d5-161">Examine o dicionário de rotas para a chave "Controller".</span><span class="sxs-lookup"><span data-stu-id="5f0d5-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="5f0d5-162">Pegue o valor dessa chave e acrescente a cadeia de caracteres "Controller" para obter o nome do tipo de controlador.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="5f0d5-163">Procure um controlador de API da Web com este nome de tipo.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="5f0d5-164">Por exemplo, se o dicionário de rotas contiver o par chave-valor "Controller" = "Products", o tipo de controlador será "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="5f0d5-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="5f0d5-165">Se não houver nenhum tipo correspondente ou várias correspondências, a estrutura retornará um erro ao cliente.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="5f0d5-166">Para a etapa 3, **DefaultHttpControllerSelector** usa a interface **IHttpControllerTypeResolver** para obter a lista de tipos de controlador da API Web.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="5f0d5-167">A implementação padrão de **IHttpControllerTypeResolver** retorna todas as classes públicas que (a) implementam **IHttpController**, (b) não são abstratas e (c) têm um nome que termina em "Controller".</span><span class="sxs-lookup"><span data-stu-id="5f0d5-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="5f0d5-168">Seleção de ação</span><span class="sxs-lookup"><span data-stu-id="5f0d5-168">Action Selection</span></span>

<span data-ttu-id="5f0d5-169">Depois de selecionar o controlador, a estrutura seleciona a ação chamando o método **IHttpActionSelector. SelectAction** .</span><span class="sxs-lookup"><span data-stu-id="5f0d5-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="5f0d5-170">Esse método usa um **HttpControllerContext** e retorna um **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="5f0d5-171">A implementação padrão é fornecida pela classe **ApiControllerActionSelector** .</span><span class="sxs-lookup"><span data-stu-id="5f0d5-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="5f0d5-172">Para selecionar uma ação, ela examina o seguinte:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="5f0d5-173">O método HTTP da solicitação.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="5f0d5-174">O espaço reservado "{Action}" no modelo de rota, se presente.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="5f0d5-175">Os parâmetros das ações no controlador.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="5f0d5-176">Antes de examinar o algoritmo de seleção, precisamos entender algumas coisas sobre as ações do controlador.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="5f0d5-177">**Quais métodos no controlador são considerados "ações"?**</span><span class="sxs-lookup"><span data-stu-id="5f0d5-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="5f0d5-178">Ao selecionar uma ação, a estrutura examina apenas os métodos de instância pública no controlador.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="5f0d5-179">Além disso, ele exclui métodos de ["nome especial"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) (construtores, eventos, sobrecargas de operador e assim por diante) e métodos herdados da classe **ApiController** .</span><span class="sxs-lookup"><span data-stu-id="5f0d5-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="5f0d5-180">**Métodos HTTP.**</span><span class="sxs-lookup"><span data-stu-id="5f0d5-180">**HTTP Methods.**</span></span> <span data-ttu-id="5f0d5-181">A estrutura escolhe apenas as ações que correspondem ao método HTTP da solicitação, determinado da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="5f0d5-182">Você pode especificar o método HTTP com um atributo: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **httpoptions**, **HttpPatch**, **HttpPost**ou **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="5f0d5-183">Caso contrário, se o nome do método do controlador começar com "Get", "post", "Put", "excluir", "Head", "Options" ou "patch", por convenção, a ação dará suporte a esse método HTTP.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="5f0d5-184">Se não houver nenhum dos anteriores, o método dará suporte a POST.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="5f0d5-185">**Associações de parâmetro.**</span><span class="sxs-lookup"><span data-stu-id="5f0d5-185">**Parameter Bindings.**</span></span> <span data-ttu-id="5f0d5-186">Uma associação de parâmetro é como a API da Web cria um valor para um parâmetro.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="5f0d5-187">Aqui está a regra padrão para a associação de parâmetro:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="5f0d5-188">Os tipos simples são obtidos do URI.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="5f0d5-189">Os tipos complexos são extraídos do corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="5f0d5-190">Os tipos simples incluem todos os [tipos primitivos .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), mais **DateTime**, **decimal**, **GUID**, **cadeia de caracteres**e **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="5f0d5-191">Para cada ação, no máximo um parâmetro pode ler o corpo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="5f0d5-192">É possível substituir as regras de associação padrão.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="5f0d5-193">Consulte [Associação de parâmetro WebAPI nos bastidores](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="5f0d5-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>

<span data-ttu-id="5f0d5-194">Com esse plano de fundo, aqui está o algoritmo de seleção de ação.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="5f0d5-195">Crie uma lista de todas as ações no controlador que correspondem ao método de solicitação HTTP.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="5f0d5-196">Se o dicionário de rotas tiver uma entrada "Action", remova as ações cujo nome não corresponde a esse valor.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="5f0d5-197">Tente corresponder os parâmetros de ação ao URI, da seguinte maneira:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="5f0d5-198">Para cada ação, obtenha uma lista dos parâmetros que são um tipo simples, em que a associação Obtém o parâmetro do URI.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="5f0d5-199">Exclua os parâmetros opcionais.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="5f0d5-200">Nessa lista, tente encontrar uma correspondência para cada nome de parâmetro, seja no dicionário de rota ou na cadeia de caracteres de consulta de URI.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="5f0d5-201">As correspondências não diferenciam maiúsculas de minúsculas e não dependem da ordem dos parâmetros.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="5f0d5-202">Selecione uma ação em que cada parâmetro na lista tenha uma correspondência no URI.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="5f0d5-203">Se mais de uma ação atender a esses critérios, escolha aquela com as correspondências mais parâmetro.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="5f0d5-204">Ignore ações com o atributo **[NonAction]** .</span><span class="sxs-lookup"><span data-stu-id="5f0d5-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="5f0d5-205">A etapa #3 é provavelmente a mais confusa.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="5f0d5-206">A ideia básica é que um parâmetro pode obter seu valor a partir do URI, do corpo da solicitação ou de uma associação personalizada.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="5f0d5-207">Para parâmetros provenientes do URI, queremos garantir que o URI realmente contenha um valor para esse parâmetro, seja no caminho (por meio do dicionário de rota) ou na cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="5f0d5-208">Por exemplo, considere a seguinte ação:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="5f0d5-209">O parâmetro *ID* é associado ao URI.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="5f0d5-210">Portanto, essa ação só pode corresponder a um URI que contém um valor para "ID", no dicionário de rota ou na cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="5f0d5-211">Os parâmetros opcionais são uma exceção, pois são opcionais.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="5f0d5-212">Para um parâmetro opcional, ele será OK se a associação não puder obter o valor do URI.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="5f0d5-213">Tipos complexos são uma exceção por um motivo diferente.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="5f0d5-214">Um tipo complexo só pode ser associado ao URI por meio de uma associação personalizada.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="5f0d5-215">Mas nesse caso, a estrutura não pode saber com antecedência se o parâmetro se associaria a um URI específico.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="5f0d5-216">Para descobrir, seria necessário invocar a associação.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="5f0d5-217">A meta do algoritmo de seleção é selecionar uma ação da descrição estática, antes de invocar qualquer associação.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="5f0d5-218">Portanto, tipos complexos são excluídos do algoritmo de correspondência.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="5f0d5-219">Depois que a ação é selecionada, todas as associações de parâmetro são invocadas.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="5f0d5-220">Resumo:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-220">Summary:</span></span>

- <span data-ttu-id="5f0d5-221">A ação deve corresponder ao método HTTP da solicitação.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="5f0d5-222">O nome da ação deve corresponder à entrada "ação" no dicionário de rota, se presente.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="5f0d5-223">Para cada parâmetro da ação, se o parâmetro for extraído do URI, o nome do parâmetro deverá ser encontrado no dicionário de rota ou na cadeia de caracteres de consulta do URI.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="5f0d5-224">(Parâmetros opcionais e parâmetros com tipos complexos são excluídos.)</span><span class="sxs-lookup"><span data-stu-id="5f0d5-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="5f0d5-225">Tente corresponder ao maior número de parâmetros.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="5f0d5-226">A melhor correspondência pode ser um método sem parâmetros.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="5f0d5-227">Exemplo estendido</span><span class="sxs-lookup"><span data-stu-id="5f0d5-227">Extended Example</span></span>

<span data-ttu-id="5f0d5-228">Circula</span><span class="sxs-lookup"><span data-stu-id="5f0d5-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="5f0d5-229">Controlador:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="5f0d5-230">Solicitação HTTP:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="5f0d5-231">Correspondência de rota</span><span class="sxs-lookup"><span data-stu-id="5f0d5-231">Route Matching</span></span>

<span data-ttu-id="5f0d5-232">O URI corresponde à rota chamada "DefaultApi".</span><span class="sxs-lookup"><span data-stu-id="5f0d5-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="5f0d5-233">O dicionário de rotas contém as seguintes entradas:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="5f0d5-234">controlador: "produtos"</span><span class="sxs-lookup"><span data-stu-id="5f0d5-234">controller: "products"</span></span>
- <span data-ttu-id="5f0d5-235">id: "1"</span><span class="sxs-lookup"><span data-stu-id="5f0d5-235">id: "1"</span></span>

<span data-ttu-id="5f0d5-236">O dicionário de rotas não contém os parâmetros de cadeia de caracteres de consulta, "Version" e "Details", mas eles ainda serão considerados durante a seleção da ação.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="5f0d5-237">Seleção de controlador</span><span class="sxs-lookup"><span data-stu-id="5f0d5-237">Controller Selection</span></span>

<span data-ttu-id="5f0d5-238">A partir da entrada "controlador" no dicionário de rota, o tipo de controlador é `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="5f0d5-239">Seleção de ação</span><span class="sxs-lookup"><span data-stu-id="5f0d5-239">Action Selection</span></span>

<span data-ttu-id="5f0d5-240">A solicitação HTTP é uma solicitação GET.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="5f0d5-241">As ações do controlador que dão suporte a GET são `GetAll`, `GetById`e `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="5f0d5-242">O dicionário de rotas não contém uma entrada para "Action", portanto, não precisamos corresponder ao nome da ação.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="5f0d5-243">Em seguida, tentamos corresponder os nomes de parâmetro das ações, observando apenas as ações GET.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="5f0d5-244">Ação</span><span class="sxs-lookup"><span data-stu-id="5f0d5-244">Action</span></span> | <span data-ttu-id="5f0d5-245">Parâmetros para correspondência</span><span class="sxs-lookup"><span data-stu-id="5f0d5-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="5f0d5-246">none</span><span class="sxs-lookup"><span data-stu-id="5f0d5-246">none</span></span> |
| `GetById` | <span data-ttu-id="5f0d5-247">"id"</span><span class="sxs-lookup"><span data-stu-id="5f0d5-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="5f0d5-248">nomes</span><span class="sxs-lookup"><span data-stu-id="5f0d5-248">"name"</span></span> |

<span data-ttu-id="5f0d5-249">Observe que o parâmetro de *versão* de `GetById` não é considerado, porque é um parâmetro opcional.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="5f0d5-250">O método `GetAll` corresponde trivialmente.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="5f0d5-251">O método `GetById` também corresponde, porque o dicionário de rotas contém "ID".</span><span class="sxs-lookup"><span data-stu-id="5f0d5-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="5f0d5-252">O método de `FindProductsByName` não corresponde.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="5f0d5-253">O método `GetById` vence, pois ele corresponde a um parâmetro, versus nenhum parâmetro para `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="5f0d5-254">O método é invocado com os seguintes valores de parâmetro:</span><span class="sxs-lookup"><span data-stu-id="5f0d5-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="5f0d5-255">*ID* = 1</span><span class="sxs-lookup"><span data-stu-id="5f0d5-255">*id* = 1</span></span>
- <span data-ttu-id="5f0d5-256">*versão* = 1,5</span><span class="sxs-lookup"><span data-stu-id="5f0d5-256">*version* = 1.5</span></span>

<span data-ttu-id="5f0d5-257">Observe que, embora a *versão* não tenha sido usada no algoritmo de seleção, o valor do parâmetro é proveniente da cadeia de caracteres de consulta do URI.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="5f0d5-258">Pontos de extensão</span><span class="sxs-lookup"><span data-stu-id="5f0d5-258">Extension Points</span></span>

<span data-ttu-id="5f0d5-259">A API Web fornece pontos de extensão para algumas partes do processo de roteamento.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="5f0d5-260">Interface</span><span class="sxs-lookup"><span data-stu-id="5f0d5-260">Interface</span></span> | <span data-ttu-id="5f0d5-261">DESCRIÇÃO</span><span class="sxs-lookup"><span data-stu-id="5f0d5-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="5f0d5-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="5f0d5-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="5f0d5-263">Seleciona o controlador.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-263">Selects the controller.</span></span> |
| <span data-ttu-id="5f0d5-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="5f0d5-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="5f0d5-265">Obtém a lista de tipos de controlador.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-265">Gets the list of controller types.</span></span> <span data-ttu-id="5f0d5-266">O **DefaultHttpControllerSelector** escolhe o tipo de controlador nesta lista.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="5f0d5-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="5f0d5-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="5f0d5-268">Obtém a lista de assemblies do projeto.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="5f0d5-269">A interface **IHttpControllerTypeResolver** usa essa lista para localizar os tipos de controlador.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="5f0d5-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="5f0d5-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="5f0d5-271">Cria novas instâncias do controlador.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="5f0d5-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="5f0d5-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="5f0d5-273">Seleciona a ação.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-273">Selects the action.</span></span> |
| <span data-ttu-id="5f0d5-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="5f0d5-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="5f0d5-275">Invoca a ação.</span><span class="sxs-lookup"><span data-stu-id="5f0d5-275">Invokes the action.</span></span> |

<span data-ttu-id="5f0d5-276">Para fornecer sua própria implementação para qualquer uma dessas interfaces, use a coleção de **Serviços** no objeto **HttpConfiguration** :</span><span class="sxs-lookup"><span data-stu-id="5f0d5-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
