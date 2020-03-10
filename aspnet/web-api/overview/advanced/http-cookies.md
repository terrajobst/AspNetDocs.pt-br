---
uid: web-api/overview/advanced/http-cookies
title: Cookies HTTP no ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Descreve como enviar e receber cookies HTTP na API Web para ASP.NET 4. x.
ms.author: riande
ms.date: 09/17/2012
ms.custom: seoapril2019
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 8ca26ff6776daa13bc4f8b06c2eba61afcfefba2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557683"
---
# <a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="cd8c0-103">Cookies HTTP no ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="cd8c0-103">HTTP Cookies in ASP.NET Web API</span></span>

<span data-ttu-id="cd8c0-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="cd8c0-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="cd8c0-105">Este tópico descreve como enviar e receber cookies HTTP na API da Web.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-105">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="cd8c0-106">Plano de fundo em cookies HTTP</span><span class="sxs-lookup"><span data-stu-id="cd8c0-106">Background on HTTP Cookies</span></span>

<span data-ttu-id="cd8c0-107">Esta seção fornece uma breve visão geral de como os cookies são implementados no nível de HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-107">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="cd8c0-108">Para obter detalhes, consulte [RFC 6265](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="cd8c0-108">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="cd8c0-109">Um cookie é um dado que um servidor envia na resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-109">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="cd8c0-110">O cliente (opcionalmente) armazena o cookie e o retorna em solicitações subsequentes.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-110">The client (optionally) stores the cookie and returns it on subsequent requests.</span></span> <span data-ttu-id="cd8c0-111">Isso permite que o cliente e o servidor compartilhem o estado.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-111">This allows the client and server to share state.</span></span> <span data-ttu-id="cd8c0-112">Para definir um cookie, o servidor inclui um cabeçalho Set-cookie na resposta.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-112">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="cd8c0-113">O formato de um cookie é um par de nome-valor, com atributos opcionais.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-113">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="cd8c0-114">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="cd8c0-114">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="cd8c0-115">Aqui está um exemplo com atributos:</span><span class="sxs-lookup"><span data-stu-id="cd8c0-115">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="cd8c0-116">Para retornar um cookie ao servidor, o cliente inclui um cabeçalho de cookie em solicitações posteriores.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-116">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="cd8c0-117">Uma resposta HTTP pode incluir vários cabeçalhos Set-cookie.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-117">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="cd8c0-118">O cliente retorna vários cookies usando um único cabeçalho de cookie.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-118">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="cd8c0-119">O escopo e a duração de um cookie são controlados pelos seguintes atributos no cabeçalho Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="cd8c0-119">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="cd8c0-120">**Domínio**: informa ao cliente qual domínio deve receber o cookie.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-120">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="cd8c0-121">Por exemplo, se o domínio for "example.com", o cliente retornará o cookie para cada subdomínio de example.com.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-121">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="cd8c0-122">Se não for especificado, o domínio será o servidor de origem.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-122">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="cd8c0-123">**Caminho**: restringe o cookie para o caminho especificado dentro do domínio.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-123">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="cd8c0-124">Se não for especificado, o caminho do URI de solicitação será usado.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-124">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="cd8c0-125">**Expira**em: define uma data de validade para o cookie.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-125">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="cd8c0-126">O cliente exclui o cookie quando ele expira.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-126">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="cd8c0-127">**Max-age**: define a idade máxima para o cookie.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-127">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="cd8c0-128">O cliente exclui o cookie quando atinge a idade máxima.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-128">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="cd8c0-129">Se `Expires` e `Max-Age` forem definidos, `Max-Age` terá precedência.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-129">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="cd8c0-130">Se nenhum for definido, o cliente excluirá o cookie quando a sessão atual terminar.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-130">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="cd8c0-131">(O significado exato de "sessão" é determinado pelo agente do usuário.)</span><span class="sxs-lookup"><span data-stu-id="cd8c0-131">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="cd8c0-132">No entanto, lembre-se de que os clientes podem ignorar cookies.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-132">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="cd8c0-133">Por exemplo, um usuário pode desabilitar cookies por motivos de privacidade.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-133">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="cd8c0-134">Os clientes podem excluir cookies antes de expirarem ou limitar o número de cookies armazenados.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-134">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="cd8c0-135">Por motivos de privacidade, os clientes geralmente rejeitam cookies "terceiros", em que o domínio não corresponde ao servidor de origem.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-135">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="cd8c0-136">Em suma, o servidor não deve confiar na obtenção dos cookies que ele define.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-136">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="cd8c0-137">Cookies na API da Web</span><span class="sxs-lookup"><span data-stu-id="cd8c0-137">Cookies in Web API</span></span>

<span data-ttu-id="cd8c0-138">Para adicionar um cookie a uma resposta HTTP, crie uma instância **CookieHeaderValue** que representa o cookie.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-138">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="cd8c0-139">Em seguida, chame o método de extensão **Addcookies** , que é definido no **sistema .net. http. Classe HttpResponseHeadersExtensions** , para adicionar o cookie.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-139">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="cd8c0-140">Por exemplo, o código a seguir adiciona um cookie dentro de uma ação do controlador:</span><span class="sxs-lookup"><span data-stu-id="cd8c0-140">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="cd8c0-141">Observe que **Addcookies** usa uma matriz de instâncias de **CookieHeaderValue** .</span><span class="sxs-lookup"><span data-stu-id="cd8c0-141">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="cd8c0-142">Para extrair os cookies de uma solicitação do cliente, chame o método **getCookies** :</span><span class="sxs-lookup"><span data-stu-id="cd8c0-142">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="cd8c0-143">Um **CookieHeaderValue** contém uma coleção de instâncias de **cookiestate** .</span><span class="sxs-lookup"><span data-stu-id="cd8c0-143">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="cd8c0-144">Cada **cookiestate** representa um cookie.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-144">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="cd8c0-145">Use o método indexador para obter um **cookiestate** por nome, como mostrado.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-145">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="cd8c0-146">Dados de cookie estruturados</span><span class="sxs-lookup"><span data-stu-id="cd8c0-146">Structured Cookie Data</span></span>

<span data-ttu-id="cd8c0-147">Muitos navegadores limitam quantos cookies armazenarão&#8212;o número total e o número por domínio.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-147">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="cd8c0-148">Portanto, pode ser útil colocar dados estruturados em um único cookie, em vez de definir vários cookies.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-148">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="cd8c0-149">A RFC 6265 não define a estrutura de dados do cookie.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-149">RFC 6265 does not define the structure of cookie data.</span></span>

<span data-ttu-id="cd8c0-150">Usando a classe **CookieHeaderValue** , você pode passar uma lista de pares de nome-valor para os dados do cookie.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-150">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="cd8c0-151">Esses pares de nome-valor são codificados como dados de formulário codificados em URL no cabeçalho Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="cd8c0-151">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="cd8c0-152">O código anterior produz o seguinte cabeçalho Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="cd8c0-152">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="cd8c0-153">A classe **cookiestate** fornece um método de indexador para ler os subvalors de um cookie na mensagem de solicitação:</span><span class="sxs-lookup"><span data-stu-id="cd8c0-153">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="cd8c0-154">Exemplo: definir e recuperar cookies em um manipulador de mensagens</span><span class="sxs-lookup"><span data-stu-id="cd8c0-154">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="cd8c0-155">Os exemplos anteriores mostraram como usar cookies de dentro de um controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-155">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="cd8c0-156">Outra opção é usar [manipuladores de mensagens](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="cd8c0-156">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="cd8c0-157">Os manipuladores de mensagens são invocados anteriormente no pipeline do que os controladores.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-157">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="cd8c0-158">Um manipulador de mensagens pode ler cookies da solicitação antes que a solicitação atinja o controlador ou adicionar cookies à resposta depois que o controlador gerar a resposta.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-158">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="cd8c0-159">O código a seguir mostra um manipulador de mensagens para a criação de IDs de sessão.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-159">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="cd8c0-160">A ID da sessão é armazenada em um cookie.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-160">The session ID is stored in a cookie.</span></span> <span data-ttu-id="cd8c0-161">O manipulador verifica a solicitação para o cookie de sessão.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-161">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="cd8c0-162">Se a solicitação não incluir o cookie, o manipulador gerará uma nova ID de sessão.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-162">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="cd8c0-163">Em ambos os casos, o manipulador armazena a ID de sessão no recipiente de propriedades **HttpRequestMessage. Properties** .</span><span class="sxs-lookup"><span data-stu-id="cd8c0-163">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="cd8c0-164">Ele também adiciona o cookie de sessão à resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-164">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="cd8c0-165">Essa implementação não valida se a ID da sessão do cliente foi realmente emitida pelo servidor.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-165">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="cd8c0-166">Não o use como uma forma de autenticação!</span><span class="sxs-lookup"><span data-stu-id="cd8c0-166">Don't use it as a form of authentication!</span></span> <span data-ttu-id="cd8c0-167">O ponto do exemplo é mostrar o gerenciamento de cookies HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd8c0-167">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="cd8c0-168">Um controlador pode obter a ID de sessão do recipiente de propriedades **HttpRequestMessage. Properties** .</span><span class="sxs-lookup"><span data-stu-id="cd8c0-168">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
