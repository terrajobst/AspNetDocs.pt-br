---
ms.openlocfilehash: b90e7963c5d9e5ef09fb519b72672c63bdffabee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57188713"
---
> Alguns comandos não têm suporte se o aplicativo usa o SQLite como seu armazenamento de dados de identidade. Devido a limitações no mecanismo de banco de dados, `Alter` comandos geram a exceção a seguir:
>
> "System.NotSupportedException: SQLite não oferece suporte a operação de migração." 
>
> Como uma solução alternativa, execute migrações do Code First no banco de dados para alterar as tabelas.
