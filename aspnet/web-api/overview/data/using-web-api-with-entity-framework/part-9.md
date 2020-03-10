---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Adicionar um novo item ao banco de dados | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 692269a2c11e529af78f24feca74bba704b5b54b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557284"
---
# <a name="add-a-new-item-to-the-database"></a><span data-ttu-id="3070a-102">Adicionar um novo item ao banco de dados</span><span class="sxs-lookup"><span data-stu-id="3070a-102">Add a New Item to the Database</span></span>

<span data-ttu-id="3070a-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3070a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3070a-104">Baixar projeto concluído</span><span class="sxs-lookup"><span data-stu-id="3070a-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="3070a-105">Nesta seção, você adicionará a capacidade para os usuários criarem um novo livro.</span><span class="sxs-lookup"><span data-stu-id="3070a-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="3070a-106">No app. js, adicione o seguinte código ao modelo de exibição:</span><span class="sxs-lookup"><span data-stu-id="3070a-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="3070a-107">Em index. cshtml, substitua a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="3070a-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="3070a-108">Por:</span><span class="sxs-lookup"><span data-stu-id="3070a-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="3070a-109">Essa marcação cria um formulário para enviar um novo autor.</span><span class="sxs-lookup"><span data-stu-id="3070a-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="3070a-110">Os valores para a lista suspensa autor são associados a dados para os `authors` observáveis no modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="3070a-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="3070a-111">Para as entradas de outro formulário, os valores são associados a dados para a propriedade `newBook` do modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="3070a-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="3070a-112">O manipulador de envio no formulário está associado à função `addBook`:</span><span class="sxs-lookup"><span data-stu-id="3070a-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="3070a-113">A função `addBook` lê os valores atuais das entradas de formulário associadas a dados para criar um objeto JSON.</span><span class="sxs-lookup"><span data-stu-id="3070a-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="3070a-114">Em seguida, ele posta o objeto JSON em `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="3070a-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3070a-115">[Anterior](part-8.md)
> [Próximo](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="3070a-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
