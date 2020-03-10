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
# <a name="add-a-new-item-to-the-database"></a>Adicionar um novo item ao banco de dados

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://github.com/MikeWasson/BookService)

Nesta seção, você adicionará a capacidade para os usuários criarem um novo livro. No app. js, adicione o seguinte código ao modelo de exibição:

[!code-javascript[Main](part-9/samples/sample1.js)]

Em index. cshtml, substitua a seguinte marcação:

[!code-html[Main](part-9/samples/sample2.html)]

Por:

[!code-html[Main](part-9/samples/sample3.html)]

Essa marcação cria um formulário para enviar um novo autor. Os valores para a lista suspensa autor são associados a dados para os `authors` observáveis no modelo de exibição. Para as entradas de outro formulário, os valores são associados a dados para a propriedade `newBook` do modelo de exibição.

O manipulador de envio no formulário está associado à função `addBook`:

[!code-html[Main](part-9/samples/sample4.html)]

A função `addBook` lê os valores atuais das entradas de formulário associadas a dados para criar um objeto JSON. Em seguida, ele posta o objeto JSON em `/api/books`.

> [!div class="step-by-step"]
> [Anterior](part-8.md)
> [Próximo](part-10.md)
