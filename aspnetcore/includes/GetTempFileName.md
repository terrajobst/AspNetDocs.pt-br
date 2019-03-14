---
ms.openlocfilehash: 2cd201e16cd491d0f468e8ef141b522f1694a257
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57165848"
---
<span data-ttu-id="15684-101">**Aviso**: O seguinte código usa `GetTempFileName`, que gera um `IOException` se mais de 65.535 arquivos são criados sem excluir os arquivos temporários anteriores.</span><span class="sxs-lookup"><span data-stu-id="15684-101">**Warning**: The following code uses `GetTempFileName`, which throws an `IOException` if more than 65535 files are created without deleting previous temporary files.</span></span> <span data-ttu-id="15684-102">Um aplicativo real deve excluir arquivos temporários ou usar `GetTempPath` e `GetRandomFileName` para criar nomes de arquivo temporários.</span><span class="sxs-lookup"><span data-stu-id="15684-102">A real app should either delete temporary files or use `GetTempPath` and `GetRandomFileName` to create temporary file names.</span></span> <span data-ttu-id="15684-103">O limite de 65.535 arquivos é por servidor, então outro aplicativo no servidor poderá usar todos os 65.535 arquivos.</span><span class="sxs-lookup"><span data-stu-id="15684-103">The 65535 files limit is per server, so another app on the server can use up all 65535 files.</span></span> 
