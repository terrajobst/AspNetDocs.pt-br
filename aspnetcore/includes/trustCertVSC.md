---
ms.openlocfilehash: 476580d4bc2435f73ef37c5344ab7fea4b67b9b8
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032493"
---
Confie no certificado de desenvolvimento HTTPS executando o seguinte comando:

```console
dotnet dev-certs https --trust
```

O comando anterior exibe a caixa de diálogo a seguir:

![Caixa de diálogo de aviso de segurança](~/getting-started/_static/cert.png)

Selecione **Sim** se você concordar com confiar no certificado de desenvolvimento.

Para obter mais informações, veja [Confiar no certificado de desenvolvimento HTTPS do ASP.NET Core](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).