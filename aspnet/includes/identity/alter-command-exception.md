---
ms.openlocfilehash: b90e7963c5d9e5ef09fb519b72672c63bdffabee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57188713"
---
> <span data-ttu-id="6ac40-101">Alguns comandos não têm suporte se o aplicativo usa o SQLite como seu armazenamento de dados de identidade.</span><span class="sxs-lookup"><span data-stu-id="6ac40-101">Some commands aren't supported if the app uses SQLite as its Identity data store.</span></span> <span data-ttu-id="6ac40-102">Devido a limitações no mecanismo de banco de dados, `Alter` comandos geram a exceção a seguir:</span><span class="sxs-lookup"><span data-stu-id="6ac40-102">Due to limitations in the database engine, `Alter` commands throw the following exception:</span></span>
>
> <span data-ttu-id="6ac40-103">"System.NotSupportedException: SQLite não oferece suporte a operação de migração."</span><span class="sxs-lookup"><span data-stu-id="6ac40-103">"System.NotSupportedException: SQLite does not support this migration operation."</span></span> 
>
> <span data-ttu-id="6ac40-104">Como uma solução alternativa, execute migrações do Code First no banco de dados para alterar as tabelas.</span><span class="sxs-lookup"><span data-stu-id="6ac40-104">As a work around, run Code First migrations on the database to change the tables.</span></span>
