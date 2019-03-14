---
ms.openlocfilehash: 934db70fe49ba9a5330b25596dc694e0ac50c04e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037893"
---
<span data-ttu-id="9c7e5-101">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9c7e5-101">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9c7e5-102">Nesta seção, você adiciona classes para gerenciamento de filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9c7e5-102">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="9c7e5-103">Você usa essas classes com o [Entity Framework Core](/ef/core) (EF Core) para trabalhar com um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9c7e5-103">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="9c7e5-104">O EF Core é uma estrutura ORM (de mapeamento relacional de objetos) que simplifica o código de acesso a dados que você precisa escrever.</span><span class="sxs-lookup"><span data-stu-id="9c7e5-104">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="9c7e5-105">As classes de modelo que você cria são conhecidas como classes de dados POCO (de "objetos CLR básicos") porque elas não têm nenhuma dependência do EF Core.</span><span class="sxs-lookup"><span data-stu-id="9c7e5-105">The model classes you create are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="9c7e5-106">Elas definem as propriedades dos dados que são armazenados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9c7e5-106">They define the properties of the data that are stored in the database.</span></span>

<span data-ttu-id="9c7e5-107">Neste tutorial, você escreve as classes de modelo primeiro e o EF Core cria o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="9c7e5-107">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="9c7e5-108">Uma abordagem alternativa não abordada aqui é [gerar classes de modelo de um banco de dados existente](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="9c7e5-108">An alternate approach not covered here is to [generate model classes from an existing database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

<span data-ttu-id="9c7e5-109">[Exiba ou baixe](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) a amostra.</span><span class="sxs-lookup"><span data-stu-id="9c7e5-109">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) sample.</span></span>
