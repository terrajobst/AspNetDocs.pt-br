---
ms.openlocfilehash: 2cd201e16cd491d0f468e8ef141b522f1694a257
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57165848"
---
**Aviso**: O seguinte código usa `GetTempFileName`, que gera um `IOException` se mais de 65.535 arquivos são criados sem excluir os arquivos temporários anteriores. Um aplicativo real deve excluir arquivos temporários ou usar `GetTempPath` e `GetRandomFileName` para criar nomes de arquivo temporários. O limite de 65.535 arquivos é por servidor, então outro aplicativo no servidor poderá usar todos os 65.535 arquivos. 
