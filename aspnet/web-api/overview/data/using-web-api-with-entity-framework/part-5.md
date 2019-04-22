---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-5
title: Criar objetos de transferência de dados (DTOs) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0fd07176-b74b-48f0-9fac-0f02e3ffa213
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-5
msc.type: authoredcontent
ms.openlocfilehash: 1af29955e8040c34840d4c77fc2006f59d2324dd
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59395271"
---
# <a name="create-data-transfer-objects-dtos"></a><span data-ttu-id="37772-102">Criar DTOs (objetos de transferência de dados)</span><span class="sxs-lookup"><span data-stu-id="37772-102">Create Data Transfer Objects (DTOs)</span></span>

<span data-ttu-id="37772-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="37772-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="37772-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="37772-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="37772-105">Direita agora, nossa API web expõe as entidades de banco de dados para o cliente.</span><span class="sxs-lookup"><span data-stu-id="37772-105">Right now, our web API exposes the database entities to the client.</span></span> <span data-ttu-id="37772-106">O cliente recebe os dados que mapeia diretamente para suas tabelas de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="37772-106">The client receives data that maps directly to your database tables.</span></span> <span data-ttu-id="37772-107">No entanto, que nem sempre é uma boa ideia.</span><span class="sxs-lookup"><span data-stu-id="37772-107">However, that's not always a good idea.</span></span> <span data-ttu-id="37772-108">Às vezes você deseja alterar a forma dos dados que você envia ao cliente.</span><span class="sxs-lookup"><span data-stu-id="37772-108">Sometimes you want to change the shape of the data that you send to client.</span></span> <span data-ttu-id="37772-109">Por exemplo, é possível:</span><span class="sxs-lookup"><span data-stu-id="37772-109">For example, you might want to:</span></span>

- <span data-ttu-id="37772-110">Remova referências circulares (consulte a seção anterior).</span><span class="sxs-lookup"><span data-stu-id="37772-110">Remove circular references (see previous section).</span></span>
- <span data-ttu-id="37772-111">Oculte propriedades específicas que os clientes não devem para exibir.</span><span class="sxs-lookup"><span data-stu-id="37772-111">Hide particular properties that clients are not supposed to view.</span></span>
- <span data-ttu-id="37772-112">Omita algumas propriedades para reduzir o tamanho da carga.</span><span class="sxs-lookup"><span data-stu-id="37772-112">Omit some properties in order to reduce payload size.</span></span>
- <span data-ttu-id="37772-113">Mescle gráficos de objetos que contêm objetos aninhados, para torná-las mais conveniente para clientes.</span><span class="sxs-lookup"><span data-stu-id="37772-113">Flatten object graphs that contain nested objects, to make them more convenient for clients.</span></span>
- <span data-ttu-id="37772-114">Evite "vulnerabilidades de overposting".</span><span class="sxs-lookup"><span data-stu-id="37772-114">Avoid "over-posting" vulnerabilities.</span></span> <span data-ttu-id="37772-115">(Consulte [validação de modelo](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) para uma discussão sobre excesso de postagem.)</span><span class="sxs-lookup"><span data-stu-id="37772-115">(See [Model Validation](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) for a discussion of over-posting.)</span></span>
- <span data-ttu-id="37772-116">Desacoplar a camada de serviço de sua camada de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="37772-116">Decouple your service layer from your database layer.</span></span>

<span data-ttu-id="37772-117">Para fazer isso, você pode definir um *objeto de transferência de dados* (DTO).</span><span class="sxs-lookup"><span data-stu-id="37772-117">To accomplish this, you can define a *data transfer object* (DTO).</span></span> <span data-ttu-id="37772-118">Um DTO é um objeto que define como os dados serão enviados pela rede.</span><span class="sxs-lookup"><span data-stu-id="37772-118">A DTO is an object that defines how the data will be sent over the network.</span></span> <span data-ttu-id="37772-119">Vamos ver como isso funciona com a entidade de livro.</span><span class="sxs-lookup"><span data-stu-id="37772-119">Let's see how that works with the Book entity.</span></span> <span data-ttu-id="37772-120">Na pasta modelos, adicione duas classes DTO:</span><span class="sxs-lookup"><span data-stu-id="37772-120">In the Models folder, add two DTO classes:</span></span>

[!code-csharp[Main](part-5/samples/sample1.cs)]

<span data-ttu-id="37772-121">O `BookDetailDTO` classe inclui todas as propriedades do modelo de livro, exceto que `AuthorName` é uma cadeia de caracteres que conterá o nome do autor.</span><span class="sxs-lookup"><span data-stu-id="37772-121">The `BookDetailDTO` class includes all of the properties from the Book model, except that `AuthorName` is a string that will hold the author name.</span></span> <span data-ttu-id="37772-122">O `BookDTO` classe contém um subconjunto de propriedades de `BookDetailDTO`.</span><span class="sxs-lookup"><span data-stu-id="37772-122">The `BookDTO` class contains a subset of properties from `BookDetailDTO`.</span></span>

<span data-ttu-id="37772-123">Em seguida, substitua os dois métodos GET no `BooksController` classe, com as versões que retornar DTOs.</span><span class="sxs-lookup"><span data-stu-id="37772-123">Next, replace the two GET methods in the `BooksController` class, with versions that return DTOs.</span></span> <span data-ttu-id="37772-124">Vamos usar o LINQ **selecionar** instrução para converter de entidades do livro em DTOs.</span><span class="sxs-lookup"><span data-stu-id="37772-124">We'll use the LINQ **Select** statement to convert from Book entities into DTOs.</span></span>

[!code-csharp[Main](part-5/samples/sample2.cs)]

<span data-ttu-id="37772-125">Aqui está o SQL gerado pelo novo `GetBooks` método.</span><span class="sxs-lookup"><span data-stu-id="37772-125">Here is the SQL generated by the new `GetBooks` method.</span></span> <span data-ttu-id="37772-126">Você pode ver que o EF converte o LINQ **selecionar** em uma instrução SQL SELECT.</span><span class="sxs-lookup"><span data-stu-id="37772-126">You can see that EF translates the LINQ **Select** into a SQL SELECT statement.</span></span>

[!code-sql[Main](part-5/samples/sample3.sql)]

<span data-ttu-id="37772-127">Por fim, modifique o `PostBook` método para retornar um DTO.</span><span class="sxs-lookup"><span data-stu-id="37772-127">Finally, modify the `PostBook` method to return a DTO.</span></span>

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="37772-128">Neste tutorial, estamos convertendo em DTOs manualmente no código.</span><span class="sxs-lookup"><span data-stu-id="37772-128">In this tutorial, we're converting to DTOs manually in code.</span></span> <span data-ttu-id="37772-129">Outra opção é usar uma biblioteca, como [AutoMapper](http://automapper.org/) que manipula a conversão automaticamente.</span><span class="sxs-lookup"><span data-stu-id="37772-129">Another option is to use a library like [AutoMapper](http://automapper.org/) that handles the conversion automatically.</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="37772-130">[Anterior](part-4.md)
> [Próximo](part-6.md)</span><span class="sxs-lookup"><span data-stu-id="37772-130">[Previous](part-4.md)
[Next](part-6.md)</span></span>
