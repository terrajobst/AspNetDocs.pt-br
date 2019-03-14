---
ms.openlocfilehash: 72e33ea44976963193d2560427fc418875be450e
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57038863"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="8d9be-101">Adicione um modelo a um aplicativo ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="8d9be-101">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="8d9be-102">Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8d9be-102">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="8d9be-103">Nesta seção, você adicionará algumas classes para o gerenciamento de filmes em um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8d9be-103">In this section, you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="8d9be-104">Essas classes serão a parte “**M**odel” parte do aplicativo **M**VC.</span><span class="sxs-lookup"><span data-stu-id="8d9be-104">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="8d9be-105">Você usa essas classes com o [EF Core](/ef/core) (Entity Framework Core) para trabalhar com um banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8d9be-105">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="8d9be-106">O EF Core é uma estrutura ORM (mapeamento relacional de objetos) que simplifica o código de acesso a dados que você precisa escrever.</span><span class="sxs-lookup"><span data-stu-id="8d9be-106">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span> <span data-ttu-id="8d9be-107">[O EF Core dá suporte a vários mecanismos de banco de dados](/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="8d9be-107">[EF Core supports many database engines](/ef/core/providers/).</span></span>

<span data-ttu-id="8d9be-108">As classes de modelo que serão criadas são conhecidas como classes POCO (de “objetos CLR básicos”) porque elas não têm nenhuma dependência do EF Core.</span><span class="sxs-lookup"><span data-stu-id="8d9be-108">The model classes you'll create are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="8d9be-109">Elas apenas definem as propriedades dos dados que serão armazenados no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8d9be-109">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="8d9be-110">Neste tutorial, você escreverá as classes de modelo primeiro e o EF Core criará o banco de dados.</span><span class="sxs-lookup"><span data-stu-id="8d9be-110">In this tutorial you'll write the model classes first, and EF Core will create the database.</span></span> <span data-ttu-id="8d9be-111">Uma abordagem alternativa não abordada aqui é gerar classes de modelo com base em um banco de dados já existente.</span><span class="sxs-lookup"><span data-stu-id="8d9be-111">An alternate approach not covered here is to generate model classes from an already-existing database.</span></span> <span data-ttu-id="8d9be-112">Para obter informações sobre essa abordagem, consulte [ASP.NET Core – Banco de dados existente](/ef/core/get-started/aspnetcore/existing-db).</span><span class="sxs-lookup"><span data-stu-id="8d9be-112">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

## <a name="add-a-data-model-class"></a><span data-ttu-id="8d9be-113">Adicionar uma classe de modelo de dados</span><span class="sxs-lookup"><span data-stu-id="8d9be-113">Add a data model class</span></span>
