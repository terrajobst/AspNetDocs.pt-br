---
ms.openlocfilehash: e40d28e9a7ca12efe45988fabef23dece893d428
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57049103"
---
# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="f2d8b-101">Como exemplo de dados de usuário segura de build/execução</span><span class="sxs-lookup"><span data-stu-id="f2d8b-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="f2d8b-102">Definir a senha com a ferramenta Secret Manager:</span><span class="sxs-lookup"><span data-stu-id="f2d8b-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="f2d8b-103">Atualize o banco de dados:</span><span class="sxs-lookup"><span data-stu-id="f2d8b-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="f2d8b-104">Habilitar o HTTPS no projeto</span><span class="sxs-lookup"><span data-stu-id="f2d8b-104">Enable HTTPS in the project</span></span>
