---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Diretrizes de segurança para a API Web ASP.NET 2 OData - ASP.NET 4.x
author: MikeWasson
description: Descreve problemas de segurança a serem considerados ao expor um conjunto de dados por meio do OData para API Web 2 ASP.NET no ASP.NET 4. x.
ms.author: riande
ms.date: 02/06/2013
ms.custom: seoapril2019
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 8194a368cb0629c30e32ec05bf4bed150d442ad8
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59393503"
---
# <a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="4f99c-103">Diretrizes de segurança para a API Web ASP.NET 2 OData</span><span class="sxs-lookup"><span data-stu-id="4f99c-103">Security Guidance for ASP.NET Web API 2 OData</span></span>

<span data-ttu-id="4f99c-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4f99c-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4f99c-105">Este tópico descreve alguns dos problemas de segurança que você deve considerar ao expor um conjunto de dados por meio do OData para API Web 2 ASP.NET no ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="4f99c-105">This topic describes some of the security issues that you should consider when exposing a dataset through OData for ASP.NET Web API 2 on ASP.NET 4.x.</span></span>

## <a name="edm-security"></a><span data-ttu-id="4f99c-106">Segurança EDM</span><span class="sxs-lookup"><span data-stu-id="4f99c-106">EDM Security</span></span>

<span data-ttu-id="4f99c-107">A semântica de consulta se baseia no modelo de dados de entidade (EDM), não os tipos de modelo subjacente.</span><span class="sxs-lookup"><span data-stu-id="4f99c-107">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="4f99c-108">Você pode excluir uma propriedade do EDM, e não será visível para a consulta.</span><span class="sxs-lookup"><span data-stu-id="4f99c-108">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="4f99c-109">Por exemplo, suponha que seu modelo incluir um tipo de funcionário com uma propriedade de salário.</span><span class="sxs-lookup"><span data-stu-id="4f99c-109">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="4f99c-110">Você talvez queira excluir essa propriedade do EDM para ocultá-la a partir de clientes.</span><span class="sxs-lookup"><span data-stu-id="4f99c-110">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="4f99c-111">Há duas maneiras de excluir uma propriedade do EDM.</span><span class="sxs-lookup"><span data-stu-id="4f99c-111">There are two ways to exclude a property from the EDM.</span></span> <span data-ttu-id="4f99c-112">Você pode definir as **[IgnoreDataMember]** atributo na propriedade na classe de modelo:</span><span class="sxs-lookup"><span data-stu-id="4f99c-112">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="4f99c-113">Você também pode remover programaticamente a propriedade do EDM:</span><span class="sxs-lookup"><span data-stu-id="4f99c-113">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="4f99c-114">Segurança de consulta</span><span class="sxs-lookup"><span data-stu-id="4f99c-114">Query Security</span></span>

<span data-ttu-id="4f99c-115">Um cliente mal-intencionado ou ingênua pode ser capaz de construir uma consulta que demora muito tempo para executar.</span><span class="sxs-lookup"><span data-stu-id="4f99c-115">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="4f99c-116">Na pior das hipóteses, isso pode interromper o acesso ao seu serviço.</span><span class="sxs-lookup"><span data-stu-id="4f99c-116">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="4f99c-117">O **[Queryable]** atributo é um filtro de ação que analisa, valida e aplica a consulta.</span><span class="sxs-lookup"><span data-stu-id="4f99c-117">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="4f99c-118">O filtro converte as opções de consulta em uma expressão LINQ.</span><span class="sxs-lookup"><span data-stu-id="4f99c-118">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="4f99c-119">Quando o controlador de OData retorna um **IQueryable** tipo, o **IQueryable** provedor LINQ converte a expressão LINQ em uma consulta.</span><span class="sxs-lookup"><span data-stu-id="4f99c-119">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="4f99c-120">Portanto, o desempenho depende no provedor LINQ que é usado e também as características específicas de seu esquema de banco de dados ou conjunto de dados.</span><span class="sxs-lookup"><span data-stu-id="4f99c-120">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="4f99c-121">Para obter mais informações sobre como usar as opções de consulta OData na API Web ASP.NET, consulte [que dão suporte a opções de consulta OData](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="4f99c-121">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="4f99c-122">Se você souber que todos os clientes são confiáveis (por exemplo, em um ambiente corporativo), ou se seu conjunto de dados for pequeno, o desempenho da consulta não pode ser um problema.</span><span class="sxs-lookup"><span data-stu-id="4f99c-122">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="4f99c-123">Caso contrário, você deve considerar as recomendações a seguir.</span><span class="sxs-lookup"><span data-stu-id="4f99c-123">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="4f99c-124">Testar o serviço com várias consultas e o banco de dados de perfil.</span><span class="sxs-lookup"><span data-stu-id="4f99c-124">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="4f99c-125">Habilite a paginação orientado para servidor evitar o retorno de um grande conjunto de dados em uma consulta.</span><span class="sxs-lookup"><span data-stu-id="4f99c-125">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="4f99c-126">Para obter mais informações, consulte [Server-Driven paginação](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="4f99c-126">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="4f99c-127">Você precisa $filter e $orderby?</span><span class="sxs-lookup"><span data-stu-id="4f99c-127">Do you need $filter and $orderby?</span></span> <span data-ttu-id="4f99c-128">Alguns aplicativos podem permitir que o cliente de paginação, usando $top e $skip, mas desabilitar as outras opções de consulta.</span><span class="sxs-lookup"><span data-stu-id="4f99c-128">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="4f99c-129">Considere restringir $orderby a propriedades em um índice clusterizado.</span><span class="sxs-lookup"><span data-stu-id="4f99c-129">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="4f99c-130">Classificação de dados grandes sem um índice clusterizado é lento.</span><span class="sxs-lookup"><span data-stu-id="4f99c-130">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="4f99c-131">Contagem máxima de nós: O **MaxNodeCount** propriedade **[Queryable]** define o número máximo de nós permitido na árvore de sintaxe $filter.</span><span class="sxs-lookup"><span data-stu-id="4f99c-131">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="4f99c-132">O valor padrão é 100, mas você talvez queira definir um valor mais baixo, pois um grande número de nós pode ser lento para compilar.</span><span class="sxs-lookup"><span data-stu-id="4f99c-132">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="4f99c-133">Isso é especialmente verdadeiro se você estiver usando o LINQ to Objects (ou seja, consultas LINQ em uma coleção na memória, sem o uso de um provedor LINQ intermediário).</span><span class="sxs-lookup"><span data-stu-id="4f99c-133">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="4f99c-134">Considere desabilitar as funções any () e All (), pois elas podem ser lentas.</span><span class="sxs-lookup"><span data-stu-id="4f99c-134">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="4f99c-135">Se quaisquer propriedades de cadeia de caracteres contém grandes cadeias de caracteres&#8212;por exemplo, uma descrição de produto ou uma entrada de blog&#8212;considere desabilitar as funções de cadeia de caracteres.</span><span class="sxs-lookup"><span data-stu-id="4f99c-135">If any string properties contain large strings&#8212;for example, a product description or a blog entry&#8212;consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="4f99c-136">Considere a possibilidade de desabilitação de filtragem nas propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="4f99c-136">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="4f99c-137">Filtrando em Propriedades de navegação pode resultar em uma junção, que pode ser lenta, dependendo de seu esquema de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f99c-137">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="4f99c-138">O código a seguir mostra um validador de consultas que impede a filtragem nas propriedades de navegação.</span><span class="sxs-lookup"><span data-stu-id="4f99c-138">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="4f99c-139">Para obter mais informações sobre os validadores de consulta, consulte [validação de consulta](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="4f99c-139">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="4f99c-140">A possibilidade de restringir $filter consultas escrevendo um validador personalizado para seu banco de dados.</span><span class="sxs-lookup"><span data-stu-id="4f99c-140">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="4f99c-141">Por exemplo, considere essas duas consultas:</span><span class="sxs-lookup"><span data-stu-id="4f99c-141">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="4f99c-142">Todos os filmes com atores cujo sobrenome começa com 'A'.</span><span class="sxs-lookup"><span data-stu-id="4f99c-142">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="4f99c-143">Todos os filmes lançados em 1994.</span><span class="sxs-lookup"><span data-stu-id="4f99c-143">All movies released in 1994.</span></span>

    <span data-ttu-id="4f99c-144">A menos que os filmes são indexados por atores, a primeira consulta pode exigir que o mecanismo de banco de dados para digitalizar toda a lista de filmes.</span><span class="sxs-lookup"><span data-stu-id="4f99c-144">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="4f99c-145">Enquanto a segunda consulta pode ser aceitável, assumindo a filmes são indexados por ano de lançamento.</span><span class="sxs-lookup"><span data-stu-id="4f99c-145">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="4f99c-146">O código a seguir mostra um validador que permite a filtragem de propriedades "ReleaseYear" e "Title", mas nenhuma outra propriedade.</span><span class="sxs-lookup"><span data-stu-id="4f99c-146">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="4f99c-147">Em geral, considere quais funções $filter que você precisa.</span><span class="sxs-lookup"><span data-stu-id="4f99c-147">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="4f99c-148">Se os clientes não é necessário a expressividade completa de $filter, você pode limitar as funções permitidas.</span><span class="sxs-lookup"><span data-stu-id="4f99c-148">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
