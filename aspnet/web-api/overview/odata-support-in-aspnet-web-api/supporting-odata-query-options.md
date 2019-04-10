---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Suporte a opções de consulta OData na API Web ASP.NET 2 - ASP.NET 4.x
author: MikeWasson
description: Visão geral com exemplos de código mostra as opções de consulta OData suporte na API Web 2 ASP.NET para ASP.NET 4. x.
ms.author: riande
ms.date: 02/04/2013
ms.custom: seoapril2019
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 428e4942e42436585049c1e84cd7b07a4a79c0d1
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59411560"
---
# <a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="aaad0-103">Suporte a opções de consulta OData na API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="aaad0-103">Supporting OData Query Options in ASP.NET Web API 2</span></span>

<span data-ttu-id="aaad0-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="aaad0-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="aaad0-105">Esta visão geral com exemplos de código demonstra as opções de consulta OData suporte na API Web 2 ASP.NET para ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="aaad0-105">This overview with code examples demonstrates the supporting OData Query Options in ASP.NET Web API 2 for ASP.NET 4.x.</span></span> 

<span data-ttu-id="aaad0-106">OData define os parâmetros que podem ser usados para modificar uma consulta de OData.</span><span class="sxs-lookup"><span data-stu-id="aaad0-106">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="aaad0-107">O cliente envia esses parâmetros na cadeia de caracteres de consulta do URI da solicitação.</span><span class="sxs-lookup"><span data-stu-id="aaad0-107">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="aaad0-108">Por exemplo, para classificar os resultados, um cliente usa o parâmetro $orderby:</span><span class="sxs-lookup"><span data-stu-id="aaad0-108">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="aaad0-109">A especificação de OData chama esses parâmetros *opções de consulta*.</span><span class="sxs-lookup"><span data-stu-id="aaad0-109">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="aaad0-110">Você pode habilitar as opções de consulta OData para qualquer controlador de API da Web em seu projeto &#8212; o controlador não precisa ser um ponto de extremidade OData.</span><span class="sxs-lookup"><span data-stu-id="aaad0-110">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="aaad0-111">Isso lhe dá uma maneira conveniente de adicionar recursos como filtragem e classificação para qualquer aplicativo de API da Web.</span><span class="sxs-lookup"><span data-stu-id="aaad0-111">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="aaad0-112">Antes de habilitar as opções de consulta, leia o tópico [diretrizes de segurança do OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="aaad0-112">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="aaad0-113">Habilitar opções de consulta OData</span><span class="sxs-lookup"><span data-stu-id="aaad0-113">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="aaad0-114">Consultas de exemplo</span><span class="sxs-lookup"><span data-stu-id="aaad0-114">Example Queries</span></span>](#examples)
- [<span data-ttu-id="aaad0-115">Paginação orientado para servidor</span><span class="sxs-lookup"><span data-stu-id="aaad0-115">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="aaad0-116">Limitando as opções de consulta</span><span class="sxs-lookup"><span data-stu-id="aaad0-116">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="aaad0-117">Opções de consulta invocando diretamente</span><span class="sxs-lookup"><span data-stu-id="aaad0-117">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="aaad0-118">Validação de consulta</span><span class="sxs-lookup"><span data-stu-id="aaad0-118">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="aaad0-119">Habilitar opções de consulta OData</span><span class="sxs-lookup"><span data-stu-id="aaad0-119">Enabling OData Query Options</span></span>

<span data-ttu-id="aaad0-120">API Web suporta as seguintes opções de consulta do OData:</span><span class="sxs-lookup"><span data-stu-id="aaad0-120">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="aaad0-121">Opção</span><span class="sxs-lookup"><span data-stu-id="aaad0-121">Option</span></span> | <span data-ttu-id="aaad0-122">Descrição</span><span class="sxs-lookup"><span data-stu-id="aaad0-122">Description</span></span> |
| --- | --- |
| <span data-ttu-id="aaad0-123">$expand</span><span class="sxs-lookup"><span data-stu-id="aaad0-123">$expand</span></span> | <span data-ttu-id="aaad0-124">Expande as entidades relacionadas embutido.</span><span class="sxs-lookup"><span data-stu-id="aaad0-124">Expands related entities inline.</span></span> |
| <span data-ttu-id="aaad0-125">$filter</span><span class="sxs-lookup"><span data-stu-id="aaad0-125">$filter</span></span> | <span data-ttu-id="aaad0-126">Filtra os resultados com base em uma condição booleana.</span><span class="sxs-lookup"><span data-stu-id="aaad0-126">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="aaad0-127">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="aaad0-127">$inlinecount</span></span> | <span data-ttu-id="aaad0-128">Informa ao servidor para incluir a contagem total de entidades correspondentes na resposta.</span><span class="sxs-lookup"><span data-stu-id="aaad0-128">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="aaad0-129">(Útil para paginação do lado do servidor).</span><span class="sxs-lookup"><span data-stu-id="aaad0-129">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="aaad0-130">$orderby</span><span class="sxs-lookup"><span data-stu-id="aaad0-130">$orderby</span></span> | <span data-ttu-id="aaad0-131">Classifica os resultados.</span><span class="sxs-lookup"><span data-stu-id="aaad0-131">Sorts the results.</span></span> |
| <span data-ttu-id="aaad0-132">$select</span><span class="sxs-lookup"><span data-stu-id="aaad0-132">$select</span></span> | <span data-ttu-id="aaad0-133">Seleciona quais propriedades a serem incluídas na resposta.</span><span class="sxs-lookup"><span data-stu-id="aaad0-133">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="aaad0-134">$skip</span><span class="sxs-lookup"><span data-stu-id="aaad0-134">$skip</span></span> | <span data-ttu-id="aaad0-135">Ignora os primeiros n resultados.</span><span class="sxs-lookup"><span data-stu-id="aaad0-135">Skips the first n results.</span></span> |
| <span data-ttu-id="aaad0-136">$top</span><span class="sxs-lookup"><span data-stu-id="aaad0-136">$top</span></span> | <span data-ttu-id="aaad0-137">Retorna somente as primeiras n os resultados.</span><span class="sxs-lookup"><span data-stu-id="aaad0-137">Returns only the first n the results.</span></span> |

<span data-ttu-id="aaad0-138">Para usar opções de consulta OData, você deve habilitá-las explicitamente.</span><span class="sxs-lookup"><span data-stu-id="aaad0-138">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="aaad0-139">Você pode habilitá-los globalmente para todo o aplicativo ou habilitá-los para controladores específicos ou ações específicas.</span><span class="sxs-lookup"><span data-stu-id="aaad0-139">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="aaad0-140">Para habilitar as opções de consulta OData globalmente, chame **EnableQuerySupport** sobre o **HttpConfiguration** classe na inicialização:</span><span class="sxs-lookup"><span data-stu-id="aaad0-140">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="aaad0-141">O **EnableQuerySupport** método permite que as opções de consulta globalmente para qualquer ação do controlador que retorna um **IQueryable** tipo.</span><span class="sxs-lookup"><span data-stu-id="aaad0-141">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="aaad0-142">Se você não quiser que as opções de consulta habilitadas para o aplicativo inteiro, você poderá habilitá-las para ações do controlador específico, adicionando os **[Queryable]** atributo ao método de ação.</span><span class="sxs-lookup"><span data-stu-id="aaad0-142">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="aaad0-143">Consultas de exemplo</span><span class="sxs-lookup"><span data-stu-id="aaad0-143">Example Queries</span></span>

<span data-ttu-id="aaad0-144">Esta seção mostra os tipos de consultas que são possíveis usando as opções de consulta OData.</span><span class="sxs-lookup"><span data-stu-id="aaad0-144">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="aaad0-145">Para obter detalhes específicos sobre as opções de consulta, consulte a documentação do OData em [www.odata.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="aaad0-145">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="aaad0-146">Para obter informações sobre $expandir e $select, consulte [usando $select, $expand e $value no ASP.NET Web API OData](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="aaad0-146">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

**<span data-ttu-id="aaad0-147">Paginação controlada por cliente</span><span class="sxs-lookup"><span data-stu-id="aaad0-147">Client-Driven Paging</span></span>**

<span data-ttu-id="aaad0-148">Para grandes conjuntos de entidade, o cliente talvez queira limitar o número de resultados.</span><span class="sxs-lookup"><span data-stu-id="aaad0-148">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="aaad0-149">Por exemplo, um cliente pode mostrar 10 entradas por vez, com links "Avançar" para a próxima página de resultados.</span><span class="sxs-lookup"><span data-stu-id="aaad0-149">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="aaad0-150">Para fazer isso, o cliente usa as opções $top e $skip.</span><span class="sxs-lookup"><span data-stu-id="aaad0-150">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="aaad0-151">A opção de $top fornece o número máximo de entradas a serem retornadas e a opção $skip fornece o número de entradas a serem ignoradas.</span><span class="sxs-lookup"><span data-stu-id="aaad0-151">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="aaad0-152">O exemplo anterior busca de entradas de 21 a 30.</span><span class="sxs-lookup"><span data-stu-id="aaad0-152">The previous example fetches entries 21 through 30.</span></span>

**<span data-ttu-id="aaad0-153">Filtragem</span><span class="sxs-lookup"><span data-stu-id="aaad0-153">Filtering</span></span>**

<span data-ttu-id="aaad0-154">A opção $filter permite que um cliente filtrar os resultados aplicando uma expressão booliana.</span><span class="sxs-lookup"><span data-stu-id="aaad0-154">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="aaad0-155">As expressões de filtro são muito poderosas; Eles incluem operadores aritméticos e lógicos, funções de cadeia de caracteres e funções de data.</span><span class="sxs-lookup"><span data-stu-id="aaad0-155">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="aaad0-156">Retorne todos os produtos com a categoria igual a "Toys".</span><span class="sxs-lookup"><span data-stu-id="aaad0-156">Return all products with category equal to "Toys".</span></span> | `http://localhost/Products?$filter=Category` <span data-ttu-id="aaad0-157">EQ 'Toys'</span><span class="sxs-lookup"><span data-stu-id="aaad0-157">eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="aaad0-158">Retorne todos os produtos com preços inferiores a 10.</span><span class="sxs-lookup"><span data-stu-id="aaad0-158">Return all products with price less than 10.</span></span> | `http://localhost/Products?$filter=Price` <span data-ttu-id="aaad0-159">lt 10</span><span class="sxs-lookup"><span data-stu-id="aaad0-159">lt 10</span></span> |
| <span data-ttu-id="aaad0-160">Operadores lógicos: Retornar todos os produtos em que o preço > = 5 e preço < = 15.</span><span class="sxs-lookup"><span data-stu-id="aaad0-160">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | `http://localhost/Products?$filter=Price` <span data-ttu-id="aaad0-161">GE 5 e preço le 15</span><span class="sxs-lookup"><span data-stu-id="aaad0-161">ge 5 and Price le 15</span></span> |
| <span data-ttu-id="aaad0-162">Funções de cadeia de caracteres: Retorne todos os produtos com "zz" no nome.</span><span class="sxs-lookup"><span data-stu-id="aaad0-162">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="aaad0-163">Funções de data: Retorne todos os produtos com ReleaseDate após 2005.</span><span class="sxs-lookup"><span data-stu-id="aaad0-163">Date functions: Return all products with ReleaseDate after 2005.</span></span> | `http://localhost/Products?$filter=year(ReleaseDate)` <span data-ttu-id="aaad0-164">gt 2005</span><span class="sxs-lookup"><span data-stu-id="aaad0-164">gt 2005</span></span> |

**<span data-ttu-id="aaad0-165">Classificação</span><span class="sxs-lookup"><span data-stu-id="aaad0-165">Sorting</span></span>**

<span data-ttu-id="aaad0-166">Para classificar os resultados, use o filtro de $orderby.</span><span class="sxs-lookup"><span data-stu-id="aaad0-166">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="aaad0-167">Classificar por preço.</span><span class="sxs-lookup"><span data-stu-id="aaad0-167">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="aaad0-168">Classificar por preço em decrescente (do mais alto ao mais baixo).</span><span class="sxs-lookup"><span data-stu-id="aaad0-168">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="aaad0-169">Classificar por categoria e, em seguida, classificar por preço em decrescente ordem dentro de categorias.</span><span class="sxs-lookup"><span data-stu-id="aaad0-169">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="aaad0-170">Paginação orientado para servidor</span><span class="sxs-lookup"><span data-stu-id="aaad0-170">Server-Driven Paging</span></span>

<span data-ttu-id="aaad0-171">Se seu banco de dados contiver milhões de registros, você não deseja enviá-los todos em uma carga.</span><span class="sxs-lookup"><span data-stu-id="aaad0-171">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="aaad0-172">Para evitar isso, o servidor pode limitar o número de entradas que ele envia em uma única resposta.</span><span class="sxs-lookup"><span data-stu-id="aaad0-172">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="aaad0-173">Para habilitar a paginação de servidor, defina as **PageSize** propriedade no **Queryable** atributo.</span><span class="sxs-lookup"><span data-stu-id="aaad0-173">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="aaad0-174">O valor é o número máximo de entradas a serem retornadas.</span><span class="sxs-lookup"><span data-stu-id="aaad0-174">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="aaad0-175">Se o controlador retorna o formato OData, o corpo da resposta conterá um link para a próxima página de dados:</span><span class="sxs-lookup"><span data-stu-id="aaad0-175">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="aaad0-176">O cliente pode usar este link para buscar a próxima página.</span><span class="sxs-lookup"><span data-stu-id="aaad0-176">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="aaad0-177">Para saber o número total de entradas no conjunto de resultados, o cliente pode definir a opção de consulta $inlinecount com o valor "allpages".</span><span class="sxs-lookup"><span data-stu-id="aaad0-177">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="aaad0-178">O valor "allpages" informa ao servidor para incluir a contagem total na resposta:</span><span class="sxs-lookup"><span data-stu-id="aaad0-178">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="aaad0-179">Links de próxima página e a contagem embutida exigem o formato OData.</span><span class="sxs-lookup"><span data-stu-id="aaad0-179">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="aaad0-180">O motivo é que o OData define campos especiais no corpo da resposta para manter o link e a contagem.</span><span class="sxs-lookup"><span data-stu-id="aaad0-180">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>


<span data-ttu-id="aaad0-181">Para formatos não OData, ainda é possível dar suporte à contagem de links e embutida de próxima página, encapsulando os resultados da consulta em uma **PageResult&lt;T&gt;**  objeto.</span><span class="sxs-lookup"><span data-stu-id="aaad0-181">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="aaad0-182">No entanto, ele requer um pouco mais de código.</span><span class="sxs-lookup"><span data-stu-id="aaad0-182">However, it requires a bit more code.</span></span> <span data-ttu-id="aaad0-183">Veja um exemplo:</span><span class="sxs-lookup"><span data-stu-id="aaad0-183">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="aaad0-184">Aqui está um exemplo de resposta JSON:</span><span class="sxs-lookup"><span data-stu-id="aaad0-184">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="aaad0-185">Limitando as opções de consulta</span><span class="sxs-lookup"><span data-stu-id="aaad0-185">Limiting the Query Options</span></span>

<span data-ttu-id="aaad0-186">As opções de consulta oferecem uma enorme de controle sobre a consulta que é executada no servidor de cliente.</span><span class="sxs-lookup"><span data-stu-id="aaad0-186">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="aaad0-187">Em alguns casos, você talvez queira limitar as opções disponíveis por motivos de segurança ou o desempenho.</span><span class="sxs-lookup"><span data-stu-id="aaad0-187">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="aaad0-188">O **[Queryable]** atributo tem alguns compilados nas propriedades para isso.</span><span class="sxs-lookup"><span data-stu-id="aaad0-188">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="aaad0-189">Aqui estão alguns exemplos.</span><span class="sxs-lookup"><span data-stu-id="aaad0-189">Here are some examples.</span></span>

<span data-ttu-id="aaad0-190">Permitir apenas $skip Skip e $ $top, para dar suporte à paginação e nada mais:</span><span class="sxs-lookup"><span data-stu-id="aaad0-190">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="aaad0-191">Permitir ordenação somente por determinadas propriedades, para impedir a classificação nas propriedades que não são indexadas no banco de dados:</span><span class="sxs-lookup"><span data-stu-id="aaad0-191">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="aaad0-192">Permitir que a função de lógica "eq", mas não há outras funções lógicas:</span><span class="sxs-lookup"><span data-stu-id="aaad0-192">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="aaad0-193">Não permitir todos os operadores aritméticos:</span><span class="sxs-lookup"><span data-stu-id="aaad0-193">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="aaad0-194">Você pode restringir opções globalmente, criando uma **QueryableAttribute** instância e passá-lo para o **EnableQuerySupport** função:</span><span class="sxs-lookup"><span data-stu-id="aaad0-194">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="aaad0-195">Opções de consulta invocando diretamente</span><span class="sxs-lookup"><span data-stu-id="aaad0-195">Invoking Query Options Directly</span></span>

<span data-ttu-id="aaad0-196">Em vez de usar o **[Queryable]** atributo, você pode invocar as opções de consulta diretamente no seu controlador.</span><span class="sxs-lookup"><span data-stu-id="aaad0-196">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="aaad0-197">Para fazer isso, adicione uma **ODataQueryOptions** parâmetro para o método do controlador.</span><span class="sxs-lookup"><span data-stu-id="aaad0-197">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="aaad0-198">Nesse caso, você não precisa de **[Queryable]** atributo.</span><span class="sxs-lookup"><span data-stu-id="aaad0-198">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="aaad0-199">API da Web preenche o **ODataQueryOptions** do URI de cadeia de caracteres de consulta.</span><span class="sxs-lookup"><span data-stu-id="aaad0-199">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="aaad0-200">Para aplicar a consulta, passe uma **IQueryable** para o **ApplyTo** método.</span><span class="sxs-lookup"><span data-stu-id="aaad0-200">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="aaad0-201">O método retorna outra **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="aaad0-201">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="aaad0-202">Para cenários avançados, se você não tiver um **IQueryable** provedor de consulta, você pode examinar as **ODataQueryOptions** e traduzir as opções de consulta em outra forma.</span><span class="sxs-lookup"><span data-stu-id="aaad0-202">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="aaad0-203">(Por exemplo, consulte de RaghuRam Nadiminti postagem de blog [consultas traduzindo OData para HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), que também inclui um [exemplo](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span><span class="sxs-lookup"><span data-stu-id="aaad0-203">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="aaad0-204">Validação de consulta</span><span class="sxs-lookup"><span data-stu-id="aaad0-204">Query Validation</span></span>

<span data-ttu-id="aaad0-205">O **[Queryable]** atributo valida a consulta antes de executá-lo.</span><span class="sxs-lookup"><span data-stu-id="aaad0-205">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="aaad0-206">A etapa de validação é executada na **QueryableAttribute.ValidateQuery** método.</span><span class="sxs-lookup"><span data-stu-id="aaad0-206">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="aaad0-207">Você também pode personalizar o processo de validação.</span><span class="sxs-lookup"><span data-stu-id="aaad0-207">You can also customize the validation process.</span></span>

<span data-ttu-id="aaad0-208">Consulte também [diretrizes de segurança do OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="aaad0-208">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="aaad0-209">Em primeiro lugar, a substituição de uma do validador de classes que é definida na **Web.Http.OData.Query.Validators** namespace.</span><span class="sxs-lookup"><span data-stu-id="aaad0-209">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="aaad0-210">Por exemplo, a seguinte classe de validador desabilita a opção 'desc' para a opção $orderby.</span><span class="sxs-lookup"><span data-stu-id="aaad0-210">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="aaad0-211">Subclasse de **[Queryable]** atributo para substituir a **ValidateQuery** método.</span><span class="sxs-lookup"><span data-stu-id="aaad0-211">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="aaad0-212">Em seguida, defina o atributo personalizado seja globalmente ou por controlador:</span><span class="sxs-lookup"><span data-stu-id="aaad0-212">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="aaad0-213">Se você estiver usando **ODataQueryOptions** diretamente, defina o validador nas opções:</span><span class="sxs-lookup"><span data-stu-id="aaad0-213">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
