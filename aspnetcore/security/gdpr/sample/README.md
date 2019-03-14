---
ms.openlocfilehash: 96cd8c78178b02ad3eb4a471540950b4581af583
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57033373"
---
# <a name="gdpr-sample"></a>Exemplo GDPR

* Na *appSettings. JSON*, defina `CheckNotConsentNeeded` para `false` exigir consentimento; caso contrário, definida como true ou omitir. Testar o aplicativo com `CheckNotConsentNeeded` definido como `false` e defina como `true`.
* Criar cookies essenciais e não essenciais com cada variação de `CheckConsentNeeded` e consentimento concedido.
* Registre-se um usuário.
* Exclua cookies.
