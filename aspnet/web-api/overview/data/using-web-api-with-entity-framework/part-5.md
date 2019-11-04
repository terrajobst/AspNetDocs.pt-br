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
ms.sourcegitcommit: 84b1681d4e6253e30468c8df8a09fe03beea9309
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2019
ms.locfileid: "73445763"
---
# <a name="create-data-transfer-objects-dtos"></a>Criar DTOs (objetos de transferência de dados)

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar projeto concluído](https://github.com/MikeWasson/BookService)

No momento, nossa API Web expõe as entidades de banco de dados ao cliente. O cliente recebe dados que mapeiam diretamente para suas tabelas de banco de dado. No entanto, isso nem sempre é uma boa ideia. Às vezes, você deseja alterar a forma dos dados que você envia para o cliente. Por exemplo, é possível:

- Remova as referências circulares (consulte a seção anterior).
- Oculte propriedades específicas que os clientes não devem exibir.
- Omita algumas propriedades para reduzir o tamanho da carga.
- Nivelar grafos de objeto que contêm objetos aninhados, para torná-los mais convenientes para os clientes.
- Evite vulnerabilidades de "excesso de postagens". (Consulte [validação de modelo](../../formats-and-model-binding/model-validation-in-aspnet-web-api.md) para uma discussão de excesso de postagem.)
- Desassocie sua camada de serviço da camada de banco de dados.

Para fazer isso, você pode definir um DTO ( *objeto de transferência de dados* ). Um DTO é um objeto que define como os dados serão enviados pela rede. Vamos ver como isso funciona com a entidade de livro. Na pasta modelos, adicione duas classes DTO:

[!code-csharp[Main](part-5/samples/sample1.cs)]

A classe `BookDetailDto` inclui todas as propriedades do modelo de livro, exceto que `AuthorName` é uma cadeia de caracteres que conterá o nome do autor. A classe `BookDto` contém um subconjunto de propriedades de `BookDetailDto`.

Em seguida, substitua os dois métodos GET na classe `BooksController`, por versões que retornam DTOs. Usaremos a instrução LINQ **Select** para converter de entidades de livro em DTOs.

[!code-csharp[Main](part-5/samples/sample2.cs)]

Aqui está o SQL gerado pelo novo método `GetBooks`. Você pode ver que o EF converte o LINQ **Select** em uma instrução SQL SELECT.

[!code-sql[Main](part-5/samples/sample3.sql)]

Por fim, modifique o método `PostBook` para retornar um DTO.

[!code-csharp[Main](part-5/samples/sample4.cs)]

> [!NOTE]
> Neste tutorial, estamos convertendo para DTOs manualmente no código. Outra opção é usar uma biblioteca como o [AutoMapper](http://automapper.org/) que manipula a conversão automaticamente.
> 
> [!div class="step-by-step"]
> [Anterior](part-4.md)
> [Próximo](part-6.md)
