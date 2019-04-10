---
uid: web-api/overview/advanced/http-cookies
title: Cookies HTTP na API Web ASP.NET - ASP.NET 4.x
author: MikeWasson
description: Descreve como enviar e receber cookies HTTP na API da Web para ASP.NET 4. x.
ms.author: riande
ms.date: 09/17/2012
ms.custom: seoapril2019
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: cd6391582f05ab80c4bd45a455a2ce488d1186c1
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59418320"
---
# <a name="http-cookies-in-aspnet-web-api"></a><span data-ttu-id="3f7f2-103">Cookies HTTP no ASP.NET API Web</span><span class="sxs-lookup"><span data-stu-id="3f7f2-103">HTTP Cookies in ASP.NET Web API</span></span>

<span data-ttu-id="3f7f2-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3f7f2-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="3f7f2-105">Este tópico descreve como enviar e receber cookies HTTP na API da Web.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-105">This topic describes how to send and receive HTTP cookies in Web API.</span></span>

## <a name="background-on-http-cookies"></a><span data-ttu-id="3f7f2-106">Informações básicas sobre Cookies HTTP</span><span class="sxs-lookup"><span data-stu-id="3f7f2-106">Background on HTTP Cookies</span></span>

<span data-ttu-id="3f7f2-107">Esta seção fornece uma visão geral de como os cookies são implementados no nível HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-107">This section gives a brief overview of how cookies are implemented at the HTTP level.</span></span> <span data-ttu-id="3f7f2-108">Para obter detalhes, consulte [6265 RFC](http://tools.ietf.org/html/rfc6265).</span><span class="sxs-lookup"><span data-stu-id="3f7f2-108">For details, consult [RFC 6265](http://tools.ietf.org/html/rfc6265).</span></span>

<span data-ttu-id="3f7f2-109">Um cookie é uma parte dos dados enviados de um servidor na resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-109">A cookie is a piece of data that a server sends in the HTTP response.</span></span> <span data-ttu-id="3f7f2-110">(Opcionalmente) o cliente armazena o cookie e o retorna em solicitações subsequentes.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-110">The client (optionally) stores the cookie and returns it on subsequent requests.</span></span> <span data-ttu-id="3f7f2-111">Isso permite que o cliente e servidor compartilhar o estado.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-111">This allows the client and server to share state.</span></span> <span data-ttu-id="3f7f2-112">Para definir um cookie, o servidor inclui um cabeçalho Set-Cookie na resposta.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-112">To set a cookie, the server includes a Set-Cookie header in the response.</span></span> <span data-ttu-id="3f7f2-113">O formato de um cookie é um par nome-valor, com atributos opcionais.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-113">The format of a cookie is a name-value pair, with optional attributes.</span></span> <span data-ttu-id="3f7f2-114">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="3f7f2-114">For example:</span></span>

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

<span data-ttu-id="3f7f2-115">Aqui está um exemplo com atributos:</span><span class="sxs-lookup"><span data-stu-id="3f7f2-115">Here is an example with attributes:</span></span>

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

<span data-ttu-id="3f7f2-116">Para retornar um cookie para o servidor, o cliente inclui um cabeçalho de Cookie em solicitações posteriores.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-116">To return a cookie to the server, the client includes a Cookie header in later requests.</span></span>

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

<span data-ttu-id="3f7f2-117">Uma resposta HTTP pode incluir vários cabeçalhos Set-Cookie.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-117">An HTTP response can include multiple Set-Cookie headers.</span></span>

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

<span data-ttu-id="3f7f2-118">O cliente retorna vários cookies usando um único cabeçalho de Cookie.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-118">The client returns multiple cookies using a single Cookie header.</span></span>

[!code-console[Main](http-cookies/samples/sample5.cmd)]

<span data-ttu-id="3f7f2-119">O escopo e a duração de um cookie são controladas por atributos a seguir no cabeçalho Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="3f7f2-119">The scope and duration of a cookie are controlled by following attributes in the Set-Cookie header:</span></span>

- <span data-ttu-id="3f7f2-120">**Domínio**: Informa ao cliente qual domínio deve receber o cookie.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-120">**Domain**: Tells the client which domain should receive the cookie.</span></span> <span data-ttu-id="3f7f2-121">Por exemplo, se o domínio for "example.com", o cliente retorna o cookie para cada subdomínio de example.com.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-121">For example, if the domain is "example.com", the client returns the cookie to every subdomain of example.com.</span></span> <span data-ttu-id="3f7f2-122">Se não for especificado, o domínio é o servidor de origem.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-122">If not specified, the domain is the origin server.</span></span>
- <span data-ttu-id="3f7f2-123">**Caminho**: Restringe o cookie para o caminho especificado dentro do domínio.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-123">**Path**: Restricts the cookie to the specified path within the domain.</span></span> <span data-ttu-id="3f7f2-124">Se não for especificado, o caminho do URI da solicitação é usado.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-124">If not specified, the path of the request URI is used.</span></span>
- <span data-ttu-id="3f7f2-125">**Expira**: Define uma data de expiração do cookie.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-125">**Expires**: Sets an expiration date for the cookie.</span></span> <span data-ttu-id="3f7f2-126">O cliente exclui o cookie quando ela expirar.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-126">The client deletes the cookie when it expires.</span></span>
- <span data-ttu-id="3f7f2-127">**Max-Age**: Define a idade máxima para o cookie.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-127">**Max-Age**: Sets the maximum age for the cookie.</span></span> <span data-ttu-id="3f7f2-128">O cliente exclui o cookie quando ele atinge a idade máxima.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-128">The client deletes the cookie when it reaches the maximum age.</span></span>

<span data-ttu-id="3f7f2-129">Se os dois `Expires` e `Max-Age` estiverem definidas, `Max-Age` terá precedência.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-129">If both `Expires` and `Max-Age` are set, `Max-Age` takes precedence.</span></span> <span data-ttu-id="3f7f2-130">Se nenhuma for configurada, o cliente exclui o cookie quando termina a sessão atual.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-130">If neither is set, the client deletes the cookie when the current session ends.</span></span> <span data-ttu-id="3f7f2-131">(O significado exato de "sessão" é determinado pelo agente do usuário).</span><span class="sxs-lookup"><span data-stu-id="3f7f2-131">(The exact meaning of "session" is determined by the user-agent.)</span></span>

<span data-ttu-id="3f7f2-132">No entanto, lembre-se de que os clientes podem ignorar cookies.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-132">However, be aware that clients may ignore cookies.</span></span> <span data-ttu-id="3f7f2-133">Por exemplo, um usuário pode desabilitar cookies por motivos de privacidade.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-133">For example, a user might disable cookies for privacy reasons.</span></span> <span data-ttu-id="3f7f2-134">Os clientes podem excluir cookies antes de expirarem ou limitar o número de cookies armazenados.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-134">Clients may delete cookies before they expire, or limit the number of cookies stored.</span></span> <span data-ttu-id="3f7f2-135">Por motivos de privacidade, os clientes geralmente rejeitam cookies "third party", em que o domínio não coincide com o servidor de origem.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-135">For privacy reasons, clients often reject "third party" cookies, where the domain does not match the origin server.</span></span> <span data-ttu-id="3f7f2-136">Em resumo, o servidor não deve depender voltando os cookies que ela define.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-136">In short, the server should not rely on getting back the cookies that it sets.</span></span>

## <a name="cookies-in-web-api"></a><span data-ttu-id="3f7f2-137">Cookies no API da Web</span><span class="sxs-lookup"><span data-stu-id="3f7f2-137">Cookies in Web API</span></span>

<span data-ttu-id="3f7f2-138">Para adicionar um cookie para uma resposta HTTP, crie uma **CookieHeaderValue** instância que representa o cookie.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-138">To add a cookie to an HTTP response, create a **CookieHeaderValue** instance that represents the cookie.</span></span> <span data-ttu-id="3f7f2-139">Em seguida, chame o **AddCookies** método de extensão, que é definido no **System.NET. HTTP. HttpResponseHeadersExtensions** classe, para adicionar o cookie.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-139">Then call the **AddCookies** extension method, which is defined in the **System.Net.Http. HttpResponseHeadersExtensions** class, to add the cookie.</span></span>

<span data-ttu-id="3f7f2-140">Por exemplo, o código a seguir adiciona um cookie dentro de uma ação do controlador:</span><span class="sxs-lookup"><span data-stu-id="3f7f2-140">For example, the following code adds a cookie within a controller action:</span></span>

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

<span data-ttu-id="3f7f2-141">Observe que **AddCookies** pega uma matriz de **CookieHeaderValue** instâncias.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-141">Notice that **AddCookies** takes an array of **CookieHeaderValue** instances.</span></span>

<span data-ttu-id="3f7f2-142">Para extrair os cookies de solicitação do cliente, chame o **GetCookies** método:</span><span class="sxs-lookup"><span data-stu-id="3f7f2-142">To extract the cookies from a client request, call the **GetCookies** method:</span></span>

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

<span data-ttu-id="3f7f2-143">Um **CookieHeaderValue** contém uma coleção de **CookieState** instâncias.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-143">A **CookieHeaderValue** contains a collection of **CookieState** instances.</span></span> <span data-ttu-id="3f7f2-144">Cada **CookieState** representa um cookie.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-144">Each **CookieState** represents one cookie.</span></span> <span data-ttu-id="3f7f2-145">Use o método de indexador para obter um **CookieState** por nome, conforme mostrado.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-145">Use the indexer method to get a **CookieState** by name, as shown.</span></span>

## <a name="structured-cookie-data"></a><span data-ttu-id="3f7f2-146">Dados estruturados de Cookie</span><span class="sxs-lookup"><span data-stu-id="3f7f2-146">Structured Cookie Data</span></span>

<span data-ttu-id="3f7f2-147">Muitos navegadores limitam quantos cookies eles armazenará&#8212;o número total e o número por domínio.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-147">Many browsers limit how many cookies they will store&#8212;both the total number, and the number per domain.</span></span> <span data-ttu-id="3f7f2-148">Portanto, ele pode ser útil colocar dados estruturados em um cookie único, em vez de definir vários cookies.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-148">Therefore, it can be useful to put structured data into a single cookie, instead of setting multiple cookies.</span></span>

> [!NOTE]
> <span data-ttu-id="3f7f2-149">RFC 6265 não define a estrutura dos dados de cookie.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-149">RFC 6265 does not define the structure of cookie data.</span></span>


<span data-ttu-id="3f7f2-150">Usando o **CookieHeaderValue** classe, você pode passar uma lista de pares nome-valor para os dados do cookie.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-150">Using the **CookieHeaderValue** class, you can pass a list of name-value pairs for the cookie data.</span></span> <span data-ttu-id="3f7f2-151">Esses pares nome-valor são codificados como dados de formato codificado de URL no cabeçalho Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="3f7f2-151">These name-value pairs are encoded as URL-encoded form data in the Set-Cookie header:</span></span>

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

<span data-ttu-id="3f7f2-152">O código anterior produz o seguinte cabeçalho Set-Cookie:</span><span class="sxs-lookup"><span data-stu-id="3f7f2-152">The previous code produces the following Set-Cookie header:</span></span>

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

<span data-ttu-id="3f7f2-153">O **CookieState** classe fornece um método de indexador para ler os valores inferiores de um cookie na mensagem de solicitação:</span><span class="sxs-lookup"><span data-stu-id="3f7f2-153">The **CookieState** class provides an indexer method to read the sub-values from a cookie in the request message:</span></span>

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a><span data-ttu-id="3f7f2-154">Exemplo: Definir e recuperar Cookies em um manipulador de mensagens</span><span class="sxs-lookup"><span data-stu-id="3f7f2-154">Example: Set and Retrieve Cookies in a Message Handler</span></span>

<span data-ttu-id="3f7f2-155">Os exemplos anteriores mostraram como usar cookies de dentro de um controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-155">The previous examples showed how to use cookies from within a Web API controller.</span></span> <span data-ttu-id="3f7f2-156">Outra opção é usar [manipuladores de mensagem](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="3f7f2-156">Another option is to use [message handlers](http-message-handlers.md).</span></span> <span data-ttu-id="3f7f2-157">Manipuladores de mensagens são invocados antes no pipeline que controladores.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-157">Message handlers are invoked earlier in the pipeline than controllers.</span></span> <span data-ttu-id="3f7f2-158">Um manipulador de mensagens pode ler cookies da solicitação antes que a solicitação atinja o controlador ou adicionar cookies para a resposta depois que o controlador de gera a resposta.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-158">A message handler can read cookies from the request before the request reaches the controller, or add cookies to the response after the controller generates the response.</span></span>

![](http-cookies/_static/image2.png)

<span data-ttu-id="3f7f2-159">O código a seguir mostra um manipulador de mensagens para a criação de IDs de sessão.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-159">The following code shows a message handler for creating session IDs.</span></span> <span data-ttu-id="3f7f2-160">A ID de sessão é armazenada em um cookie.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-160">The session ID is stored in a cookie.</span></span> <span data-ttu-id="3f7f2-161">O manipulador verifica a solicitação para o cookie de sessão.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-161">The handler checks the request for the session cookie.</span></span> <span data-ttu-id="3f7f2-162">Se a solicitação não inclui o cookie, o manipulador gera uma ID de sessão nova.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-162">If the request does not include the cookie, the handler generates a new session ID.</span></span> <span data-ttu-id="3f7f2-163">Em ambos os casos, o manipulador armazena a ID de sessão na **HttpRequestMessage.Properties** recipiente de propriedades.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-163">In either case, the handler stores the session ID in the **HttpRequestMessage.Properties** property bag.</span></span> <span data-ttu-id="3f7f2-164">Ele também adiciona o cookie de sessão para a resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-164">It also adds the session cookie to the HTTP response.</span></span>

<span data-ttu-id="3f7f2-165">Essa implementação não valida que a ID de sessão do cliente realmente foi emitida pelo servidor.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-165">This implementation does not validate that the session ID from the client was actually issued by the server.</span></span> <span data-ttu-id="3f7f2-166">Não usá-lo como uma forma de autenticação!</span><span class="sxs-lookup"><span data-stu-id="3f7f2-166">Don't use it as a form of authentication!</span></span> <span data-ttu-id="3f7f2-167">O ponto desse exemplo é mostrar o gerenciamento de cookie HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-167">The point of the example is to show HTTP cookie management.</span></span>

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

<span data-ttu-id="3f7f2-168">Um controlador pode obter a ID de sessão do **HttpRequestMessage.Properties** recipiente de propriedades.</span><span class="sxs-lookup"><span data-stu-id="3f7f2-168">A controller can get the session ID from the **HttpRequestMessage.Properties** property bag.</span></span>

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
