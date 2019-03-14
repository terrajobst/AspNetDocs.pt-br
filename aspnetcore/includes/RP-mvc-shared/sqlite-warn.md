---
ms.openlocfilehash: 2481b10abae9aefce2d5173d8071e4c2236db928
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048213"
---

> [!NOTE]
> Muitas operações de alteração de esquema não têm suporte do provedor EF Core do SQLite. Por exemplo, há suporte para adicionar uma coluna, mas não há suporte para a remoção de uma coluna. Se uma migração é criada para remover uma coluna, o `ef migrations add` comando for bem-sucedido, mas o `ef database update` comando falhará. Algumas dessas limitações podem ser superadas manualmente escrevendo código de migrações para executar uma recriação de tabela. Envolve a recriação de tabela:

>* Renomear a tabela existente.
>* Criando uma nova tabela.
>* Copiando dados da tabela antiga para a nova tabela.
>* Descartar a tabela antiga.

Para obter mais informações, consulte os seguintes recursos:
> * [Limitações do Provedor de Banco de Dados EF Core do SQLite](/ef/core/providers/sqlite/limitations)
> * [Personalizar o código de migração](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [Propagação de dados](/ef/core/modeling/data-seeding)