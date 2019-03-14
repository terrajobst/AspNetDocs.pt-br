---
ms.openlocfilehash: 2481b10abae9aefce2d5173d8071e4c2236db928
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048213"
---

> [!NOTE]
> <span data-ttu-id="b5572-101">Muitas operações de alteração de esquema não têm suporte do provedor EF Core do SQLite.</span><span class="sxs-lookup"><span data-stu-id="b5572-101">Many schema change operations are not supported by the EF Core SQLite provider.</span></span> <span data-ttu-id="b5572-102">Por exemplo, há suporte para adicionar uma coluna, mas não há suporte para a remoção de uma coluna.</span><span class="sxs-lookup"><span data-stu-id="b5572-102">For example, adding a column is supported, but removing a column is not supported.</span></span> <span data-ttu-id="b5572-103">Se uma migração é criada para remover uma coluna, o `ef migrations add` comando for bem-sucedido, mas o `ef database update` comando falhará.</span><span class="sxs-lookup"><span data-stu-id="b5572-103">If a migration is created to remove a column, the `ef migrations add` command succeeds but the `ef database update` command fails.</span></span> <span data-ttu-id="b5572-104">Algumas dessas limitações podem ser superadas manualmente escrevendo código de migrações para executar uma recriação de tabela.</span><span class="sxs-lookup"><span data-stu-id="b5572-104">Some of these limitations can be overcome by manually writing migrations code to perform a table rebuild.</span></span> <span data-ttu-id="b5572-105">Envolve a recriação de tabela:</span><span class="sxs-lookup"><span data-stu-id="b5572-105">A table rebuild involves:</span></span>

>* <span data-ttu-id="b5572-106">Renomear a tabela existente.</span><span class="sxs-lookup"><span data-stu-id="b5572-106">Renaming the existing table.</span></span>
>* <span data-ttu-id="b5572-107">Criando uma nova tabela.</span><span class="sxs-lookup"><span data-stu-id="b5572-107">Creating a new table.</span></span>
>* <span data-ttu-id="b5572-108">Copiando dados da tabela antiga para a nova tabela.</span><span class="sxs-lookup"><span data-stu-id="b5572-108">Copying data from the old table to the new table.</span></span>
>* <span data-ttu-id="b5572-109">Descartar a tabela antiga.</span><span class="sxs-lookup"><span data-stu-id="b5572-109">Dropping the old table.</span></span>

<span data-ttu-id="b5572-110">Para obter mais informações, consulte os seguintes recursos:</span><span class="sxs-lookup"><span data-stu-id="b5572-110">For more information, see the following resources:</span></span>
> * [<span data-ttu-id="b5572-111">Limitações do Provedor de Banco de Dados EF Core do SQLite</span><span class="sxs-lookup"><span data-stu-id="b5572-111">SQLite EF Core Database Provider Limitations</span></span>](/ef/core/providers/sqlite/limitations)
> * [<span data-ttu-id="b5572-112">Personalizar o código de migração</span><span class="sxs-lookup"><span data-stu-id="b5572-112">Customize migration code</span></span>](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [<span data-ttu-id="b5572-113">Propagação de dados</span><span class="sxs-lookup"><span data-stu-id="b5572-113">Data seeding</span></span>](/ef/core/modeling/data-seeding)