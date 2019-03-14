---
ms.openlocfilehash: 71a40fcab8f1d1005cbe9327d01a42484765a421
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57059633"
---
# <a name="custom-model-binding-demo"></a>Demonstração de model binding personalizado

Teste o `ByteArrayModelBinder` executando o aplicativo e postando uma cadeia de caracteres codificada em Base64 no ponto de extremidade `ImageController` (`/api/image/`). Especifique as propriedades de arquivo e de nome de arquivo no corpo da solicitação como dados de formulário (usando o [Postman](https://www.getpostman.com/) ou uma ferramenta semelhante). Use [esta cadeia de caracteres de exemplo](Base64String.txt). O resultado é salvo na pasta *wwwroot/images/upload* com o nome do arquivo especificado.

Para testar o exemplo de associação personalizada, experimente os seguintes pontos de extremidade:

* /api/authors/1
* /api/authors/2 (NOT FOUND)
* /api/boundauthors/1
* /api/boundauthors/2 (NOT FOUND)
* /api/boundauthors/get/1
* /api/boundauthors/get/2 (NO CONTENT) &ndash; Essa ação não verifica se há nulos e retorna um *404 Não Encontrado*.
