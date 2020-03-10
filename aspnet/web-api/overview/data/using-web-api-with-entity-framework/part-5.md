---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Criar objetos de Transferência de Dados (DTOs) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: fc0463420207eba764014b8ec7123c5150e38247
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557347"
---
# <a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="77642-102">Criar DTOs (objetos de transferência de dados)</span><span class="sxs-lookup"><span data-stu-id="77642-102">Create Data Transfer Objects (DTOs)</span></span>

<span data-ttu-id="77642-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="77642-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="77642-104">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="77642-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="77642-105">No momento, nossa API Web expõe as entidades de banco de dados ao cliente.</span><span class="sxs-lookup"><span data-stu-id="77642-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="77642-106">O cliente recebe dados que mapeiam diretamente para suas tabelas de banco de dado.</span><span class="sxs-lookup"><span data-stu-id="77642-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="77642-107">No entanto, isso nem sempre é uma boa ideia.</span><span class="sxs-lookup"><span data-stu-id="77642-107">However, that's not always a good idea.</span></span> <span data-ttu-id="77642-108">Às vezes, você deseja alterar a forma dos dados que você envia para o cliente.</span><span class="sxs-lookup"><span data-stu-id="77642-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="77642-109">Por exemplo, você pode querer:</span><span class="sxs-lookup"><span data-stu-id="77642-109">For example, you might want to:</span></span>

- <span data-ttu-id="77642-110">Remova as referências circulares (consulte a seção anterior).</span><span class="sxs-lookup"><span data-stu-id="77642-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="77642-111">Oculte propriedades específicas que os clientes não devem exibir.</span><span class="sxs-lookup"><span data-stu-id="77642-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="77642-112">Omita algumas propriedades para reduzir o tamanho da carga.</span><span class="sxs-lookup"><span data-stu-id="77642-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="77642-113">Nivelar grafos de objeto que contêm objetos aninhados, para torná-los mais convenientes para os clientes.</span><span class="sxs-lookup"><span data-stu-id="77642-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="77642-114">Evite vulnerabilidades de "excesso de postagens".</span><span class="sxs-lookup"><span data-stu-id="77642-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="77642-115">(Consulte [validação de modelo](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) para uma discussão de excesso de postagem.)</span><span class="sxs-lookup"><span data-stu-id="77642-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="77642-116">Desassocie sua camada de serviço da camada de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="77642-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="77642-117">Para fazer isso, você pode definir um DTO ( *objeto de transferência de dados* ).</span><span class="sxs-lookup"><span data-stu-id="77642-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="77642-118">Um DTO é um objeto que define como os dados serão enviados pela rede.</span><span class="sxs-lookup"><span data-stu-id="77642-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="77642-119">Vamos ver como isso funciona com a entidade de livro.</span><span class="sxs-lookup"><span data-stu-id="77642-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="77642-120">Na pasta modelos, adicione duas classes DTO:</span><span class="sxs-lookup"><span data-stu-id="77642-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="77642-121">A classe `BookDetailDto` inclui todas as propriedades do modelo de livro, exceto que `AuthorName` é uma cadeia de caracteres que conterá o nome do autor.</span><span class="sxs-lookup"><span data-stu-id="77642-121">The `BookDetailDto` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="77642-122">A classe `BookDto` contém um subconjunto de propriedades de `BookDetailDto`.</span><span class="sxs-lookup"><span data-stu-id="77642-122">The `BookDto` class contains a subset of properties from `BookDetailDto`.</span></span>

<span data-ttu-id="77642-123">Em seguida, substitua os dois métodos GET na classe `BooksController`, por versões que retornam DTOs.</span><span class="sxs-lookup"><span data-stu-id="77642-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="77642-124">Usaremos a instrução LINQ **Select** para converter de entidades de livro em DTOs.</span><span class="sxs-lookup"><span data-stu-id="77642-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="77642-125">Aqui está o SQL gerado pelo novo método `GetBooks`.</span><span class="sxs-lookup"><span data-stu-id="77642-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="77642-126">Você pode ver que o EF converte o LINQ **Select** em uma instrução SQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="77642-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="77642-127">Por fim, modifique o método `PostBook` para retornar um DTO.</span><span class="sxs-lookup"><span data-stu-id="77642-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="77642-128">Neste tutorial, estamos convertendo para DTOs manualmente no código.</span><span class="sxs-lookup"><span data-stu-id="77642-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="77642-129">Outra opção é usar uma biblioteca como o [AutoMapper](http://automapper.org/) que manipula a conversão automaticamente.</span><span class="sxs-lookup"><span data-stu-id="77642-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="77642-130">[Anterior](part-4.md)
> [Próximo](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="77642-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
