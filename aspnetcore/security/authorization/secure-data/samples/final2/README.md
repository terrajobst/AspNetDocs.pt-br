---
ms.openlocfilehash: e40d28e9a7ca12efe45988fabef23dece893d428
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049103"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a>Como exemplo de dados de usuário segura de build/execução

* Definir a senha com a ferramenta Secret Manager:

  `dotnet user-secrets set SeedUserPW <pw>`

* Atualize o banco de dados:

    `dotnet ef database update`

* Habilitar o HTTPS no projeto
