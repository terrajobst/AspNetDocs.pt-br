---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Manipulando relações de entidade | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: be4948e5443a5eb4e1824c63dd0c445a7ee1928e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557459"
---
# <a name="handling-entity-relations"></a><span data-ttu-id="aa04f-102">Tratamento de relações de entidade</span><span class="sxs-lookup"><span data-stu-id="aa04f-102">Handling Entity Relations</span></span>

<span data-ttu-id="aa04f-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="aa04f-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="aa04f-104">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="aa04f-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="aa04f-105">Esta seção descreve alguns detalhes de como o EF carrega entidades relacionadas e como lidar com propriedades de navegação circular em suas classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="aa04f-105">This section describes some details of how EF loads related entities, and how to handle circular navigation properties in your model classes.</span></span> <span data-ttu-id="aa04f-106">(Esta seção fornece conhecimento em segundo plano e não é necessária para concluir o tutorial.</span><span class="sxs-lookup"><span data-stu-id="aa04f-106">(This section provides background knowledge, and is not required to complete the tutorial.</span></span> <span data-ttu-id="aa04f-107">Se preferir, pule para a [parte 5.](part-5.md).)</span><span class="sxs-lookup"><span data-stu-id="aa04f-107">If you prefer, skip to [Part 5.](part-5.md).)</span></span>

## <a name="eager-loading-versus-lazy-loading"></a><span data-ttu-id="aa04f-108">Carregamento adiantado versus carregamento lento</span><span class="sxs-lookup"><span data-stu-id="aa04f-108">Eager Loading versus Lazy Loading</span></span>

<span data-ttu-id="aa04f-109">Ao usar o EF com um banco de dados relacional, é importante entender como o EF carrega dados relacionados.</span><span class="sxs-lookup"><span data-stu-id="aa04f-109">When using EF with a relational database, it's important to understand how EF loads related data.</span></span>

<span data-ttu-id="aa04f-110">Também é útil ver as consultas SQL que o EF gera.</span><span class="sxs-lookup"><span data-stu-id="aa04f-110">It's also useful to see the SQL queries that EF generates.</span></span> <span data-ttu-id="aa04f-111">Para rastrear o SQL, adicione a seguinte linha de código ao construtor de `BookServiceContext`:</span><span class="sxs-lookup"><span data-stu-id="aa04f-111">To trace the SQL, add the following line of code to the `BookServiceContext` constructor:</span></span>

[!code-csharp[Main](part-4/samples/sample1.cs)]

<span data-ttu-id="aa04f-112">Se você enviar uma solicitação GET para/API/Books, ela retornará JSON como o seguinte:</span><span class="sxs-lookup"><span data-stu-id="aa04f-112">If you send a GET request to /api/books, it returns JSON like the following:</span></span>

[!code-console[Main](part-4/samples/sample2.cmd)]

<span data-ttu-id="aa04f-113">Você pode ver que a propriedade autor é nula, embora o livro contenha uma AuthorId válida.</span><span class="sxs-lookup"><span data-stu-id="aa04f-113">You can see that the Author property is null, even though the book contains a valid AuthorId.</span></span> <span data-ttu-id="aa04f-114">Isso ocorre porque o EF não está carregando as entidades do autor relacionadas.</span><span class="sxs-lookup"><span data-stu-id="aa04f-114">That's because EF is not loading the related Author entities.</span></span> <span data-ttu-id="aa04f-115">O log de rastreamento da consulta SQL confirma isso:</span><span class="sxs-lookup"><span data-stu-id="aa04f-115">The trace log of the SQL query confirms this:</span></span>

[!code-console[Main](part-4/samples/sample3.sql)]

<span data-ttu-id="aa04f-116">A instrução SELECT usa a tabela books e não faz referência à tabela de autor.</span><span class="sxs-lookup"><span data-stu-id="aa04f-116">The SELECT statement takes from the Books table, and does not reference the Author table.</span></span>

<span data-ttu-id="aa04f-117">Para referência, aqui está o método na classe `BooksController` que retorna a lista de livros.</span><span class="sxs-lookup"><span data-stu-id="aa04f-117">For reference, here is the method in the `BooksController` class that returns the list of books.</span></span>

[!code-csharp[Main](part-4/samples/sample4.cs)]

<span data-ttu-id="aa04f-118">Vejamos como podemos retornar o autor como parte dos dados JSON.</span><span class="sxs-lookup"><span data-stu-id="aa04f-118">Let's see how we can return the Author as part of the JSON data.</span></span> <span data-ttu-id="aa04f-119">Há três maneiras de carregar dados relacionados no Entity Framework: carregamento adiantado, carregamento lento e carregamento explícito.</span><span class="sxs-lookup"><span data-stu-id="aa04f-119">There are three ways to load related data in Entity Framework: eager loading, lazy loading, and explicit loading.</span></span> <span data-ttu-id="aa04f-120">Há compensações com cada técnica, portanto, é importante entender como elas funcionam.</span><span class="sxs-lookup"><span data-stu-id="aa04f-120">There are trade-offs with each technique, so it's important to understand how they work.</span></span>

### <a name="eager-loading"></a><span data-ttu-id="aa04f-121">Carregamento adiantado</span><span class="sxs-lookup"><span data-stu-id="aa04f-121">Eager Loading</span></span>

<span data-ttu-id="aa04f-122">Com o *carregamento adiantado*, o EF carrega entidades relacionadas como parte da consulta de banco de dados inicial.</span><span class="sxs-lookup"><span data-stu-id="aa04f-122">With *eager loading*, EF loads related entities as part of the initial database query.</span></span> <span data-ttu-id="aa04f-123">Para executar o carregamento adiantado, use o método de extensão **System. Data. Entity. include** .</span><span class="sxs-lookup"><span data-stu-id="aa04f-123">To perform eager loading, use the **System.Data.Entity.Include** extension method.</span></span>

[!code-csharp[Main](part-4/samples/sample5.cs)]

<span data-ttu-id="aa04f-124">Isso informa ao EF para incluir os dados do autor na consulta.</span><span class="sxs-lookup"><span data-stu-id="aa04f-124">This tells EF to include the Author data in the query.</span></span> <span data-ttu-id="aa04f-125">Se você fizer essa alteração e executar o aplicativo, agora os dados JSON têm a seguinte aparência:</span><span class="sxs-lookup"><span data-stu-id="aa04f-125">If you make this change and run the app, now the JSON data looks like this:</span></span>

[!code-console[Main](part-4/samples/sample6.cmd)]

<span data-ttu-id="aa04f-126">O log de rastreamento mostra que o EF executou uma junção no livro e cria tabelas.</span><span class="sxs-lookup"><span data-stu-id="aa04f-126">The trace log shows that EF performed a join on the Book and Author tables.</span></span>

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a><span data-ttu-id="aa04f-127">Carregamento lento</span><span class="sxs-lookup"><span data-stu-id="aa04f-127">Lazy Loading</span></span>

<span data-ttu-id="aa04f-128">Com o carregamento lento, o EF carrega automaticamente uma entidade relacionada quando a propriedade de navegação para essa entidade é desreferenciada.</span><span class="sxs-lookup"><span data-stu-id="aa04f-128">With lazy loading, EF automatically loads a related entity when the navigation property for that entity is dereferenced.</span></span> <span data-ttu-id="aa04f-129">Para habilitar o carregamento lento, torne a propriedade de navegação virtual.</span><span class="sxs-lookup"><span data-stu-id="aa04f-129">To enable lazy loading, make the navigation property virtual.</span></span> <span data-ttu-id="aa04f-130">Por exemplo, na classe Book:</span><span class="sxs-lookup"><span data-stu-id="aa04f-130">For example, in the Book class:</span></span>

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

<span data-ttu-id="aa04f-131">Agora, considere o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="aa04f-131">Now consider the following code:</span></span>

[!code-csharp[Main](part-4/samples/sample9.cs)]

<span data-ttu-id="aa04f-132">Quando o carregamento lento está habilitado, o acesso à propriedade `Author` em `books[0]` faz com que o EF consulte o banco de dados para o autor.</span><span class="sxs-lookup"><span data-stu-id="aa04f-132">When lazy loading is enabled, accessing the `Author` property on `books[0]` causes EF to query the database for the author.</span></span>

<span data-ttu-id="aa04f-133">O carregamento lento requer várias viagens de banco de dados, porque o EF envia uma consulta cada vez que recupera uma entidade relacionada.</span><span class="sxs-lookup"><span data-stu-id="aa04f-133">Lazy loading requires multiple database trips, because EF sends a query each time it retrieves a related entity.</span></span> <span data-ttu-id="aa04f-134">Em geral, você deseja que o carregamento lento seja desabilitado para objetos serializados.</span><span class="sxs-lookup"><span data-stu-id="aa04f-134">Generally, you want lazy loading disabled for objects that you serialize.</span></span> <span data-ttu-id="aa04f-135">O serializador precisa ler todas as propriedades no modelo, que dispara o carregamento das entidades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="aa04f-135">The serializer has to read all of the properties on the model, which triggers loading the related entities.</span></span> <span data-ttu-id="aa04f-136">Por exemplo, aqui estão as consultas SQL quando o EF serializa a lista de livros com o carregamento lento habilitado.</span><span class="sxs-lookup"><span data-stu-id="aa04f-136">For example, here are the SQL queries when EF serializes the list of books with lazy loading enabled.</span></span> <span data-ttu-id="aa04f-137">Você pode ver que o EF faz três consultas separadas para os três autores.</span><span class="sxs-lookup"><span data-stu-id="aa04f-137">You can see that EF makes three separate queries for the three authors.</span></span>

[!code-console[Main](part-4/samples/sample10.sql)]

<span data-ttu-id="aa04f-138">Ainda há momentos em que você talvez queira usar o carregamento lento.</span><span class="sxs-lookup"><span data-stu-id="aa04f-138">There are still times when you might want to use lazy loading.</span></span> <span data-ttu-id="aa04f-139">O carregamento adiantado pode fazer com que o EF gere uma junção muito complexa.</span><span class="sxs-lookup"><span data-stu-id="aa04f-139">Eager loading can cause EF to generate a very complex join.</span></span> <span data-ttu-id="aa04f-140">Ou talvez você precise de entidades relacionadas para um pequeno subconjunto dos dados e o carregamento lento seria mais eficiente.</span><span class="sxs-lookup"><span data-stu-id="aa04f-140">Or you might need related entities for a small subset of the data, and lazy loading would be more efficient.</span></span>

<span data-ttu-id="aa04f-141">Uma maneira de evitar problemas de serialização é serializar os DTOs (objetos de transferência de dados) em vez de objetos de entidade.</span><span class="sxs-lookup"><span data-stu-id="aa04f-141">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects.</span></span> <span data-ttu-id="aa04f-142">Mostrarei essa abordagem posteriormente neste artigo.</span><span class="sxs-lookup"><span data-stu-id="aa04f-142">I'll show this approach later in the article.</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="aa04f-143">Carregamento explícito</span><span class="sxs-lookup"><span data-stu-id="aa04f-143">Explicit Loading</span></span>

<span data-ttu-id="aa04f-144">O carregamento explícito é semelhante ao carregamento lento, exceto pelo fato de que você obtém explicitamente os dados relacionados no código; Ele não acontece automaticamente quando você acessa uma propriedade de navegação.</span><span class="sxs-lookup"><span data-stu-id="aa04f-144">Explicit loading is similar to lazy loading, except that you explicitly get the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="aa04f-145">O carregamento explícito oferece mais controle sobre quando carregar dados relacionados, mas requer código extra.</span><span class="sxs-lookup"><span data-stu-id="aa04f-145">Explicit loading gives you more control over when to load related data, but requires extra code.</span></span> <span data-ttu-id="aa04f-146">Para obter mais informações sobre o carregamento explícito, consulte [carregando entidades relacionadas](https://msdn.microsoft.com/data/jj574232#explicit).</span><span class="sxs-lookup"><span data-stu-id="aa04f-146">For more information about explicit loading, see [Loading Related Entities](https://msdn.microsoft.com/data/jj574232#explicit).</span></span>

## <a name="navigation-properties-and-circular-references"></a><span data-ttu-id="aa04f-147">Propriedades de navegação e referências circulares</span><span class="sxs-lookup"><span data-stu-id="aa04f-147">Navigation Properties and Circular References</span></span>

<span data-ttu-id="aa04f-148">Quando defini os modelos Book e Author, defini uma propriedade de navegação na classe `Book` para a relação Book-Author, mas não defini uma propriedade de navegação na outra direção.</span><span class="sxs-lookup"><span data-stu-id="aa04f-148">When I defined the Book and Author models, I defined a navigation property on the `Book` class for the Book-Author relationship, but I did not define a navigation property in the other direction.</span></span>

<span data-ttu-id="aa04f-149">O que acontecerá se você adicionar a propriedade de navegação correspondente à classe `Author`?</span><span class="sxs-lookup"><span data-stu-id="aa04f-149">What happens if you add the corresponding navigation property to the `Author` class?</span></span>

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

<span data-ttu-id="aa04f-150">Infelizmente, isso cria um problema ao serializar os modelos.</span><span class="sxs-lookup"><span data-stu-id="aa04f-150">Unfortunately, this creates a problem when you serialize the models.</span></span> <span data-ttu-id="aa04f-151">Se você carregar os dados relacionados, ele criará um gráfico de objeto circular.</span><span class="sxs-lookup"><span data-stu-id="aa04f-151">If you load the related data, it creates a circular object graph.</span></span>

![](part-4/_static/image1.png)

<span data-ttu-id="aa04f-152">Quando o formatador JSON ou XML tenta serializar o grafo, ele gerará uma exceção.</span><span class="sxs-lookup"><span data-stu-id="aa04f-152">When the JSON or XML formatter tries to serialize the graph, it will throw an exception.</span></span> <span data-ttu-id="aa04f-153">Os dois formatadores lançam mensagens de exceção diferentes.</span><span class="sxs-lookup"><span data-stu-id="aa04f-153">The two formatters throw different exception messages.</span></span> <span data-ttu-id="aa04f-154">Aqui está um exemplo para o formatador JSON:</span><span class="sxs-lookup"><span data-stu-id="aa04f-154">Here is an example for the JSON formatter:</span></span>

[!code-console[Main](part-4/samples/sample12.cmd)]

<span data-ttu-id="aa04f-155">Este é o formatador XML:</span><span class="sxs-lookup"><span data-stu-id="aa04f-155">Here is the XML formatter:</span></span>

[!code-xml[Main](part-4/samples/sample13.xml)]

<span data-ttu-id="aa04f-156">Uma solução é usar DTOs, que descreverei na próxima seção.</span><span class="sxs-lookup"><span data-stu-id="aa04f-156">One solution is to use DTOs, which I describe in the next section.</span></span> <span data-ttu-id="aa04f-157">Como alternativa, você pode configurar os formatadores JSON e XML para lidar com os ciclos do grafo.</span><span class="sxs-lookup"><span data-stu-id="aa04f-157">Alternatively, you can configure the JSON and XML formatters to handle graph cycles.</span></span> <span data-ttu-id="aa04f-158">Para obter mais informações, consulte [lidando com referências a objetos circulares](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span><span class="sxs-lookup"><span data-stu-id="aa04f-158">For more information, see [Handling Circular Object References](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).</span></span>

<span data-ttu-id="aa04f-159">Para este tutorial, você não precisa da propriedade de navegação `Author.Book`, para que você possa deixá-la fora.</span><span class="sxs-lookup"><span data-stu-id="aa04f-159">For this tutorial, you don't need the `Author.Book` navigation property, so you can leave it out.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="aa04f-160">[Anterior](part-3.md)
> [Próximo](part-5.md)</span><span class="sxs-lookup"><span data-stu-id="aa04f-160">[Previous](part-3.md)
[Next](part-5.md)</span></span>
