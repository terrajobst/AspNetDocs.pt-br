---
ms.openlocfilehash: 939231583679d469d01724cc1a405bd45e46d2c5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57175871"
---
```console
npm run release
```

Este comando suspende o fornecimento dos ativos do lado do cliente ao executar o aplicativo. Os ativos são colocados na pasta *wwwroot*.

O Webpack concluiu as seguintes tarefas:

* Limpou os conteúdos do diretório *wwwroot*.
* Converteu o TypeScript para JavaScript, um processo conhecido como *transpilação*.
* Reduziu o tamanho do arquivo JavaScript gerado, um processo conhecido como *minificação*.
* Copiou os arquivos JavaScript, CSS e HTML processados do *src* para o diretório *wwwroot*.
* Injetou os seguintes elementos no arquivo *wwwroot/index.html*:
    * Uma marca `<link>`, que referencia o arquivo *wwwroot/main.\<hash\>.css*. Essa marca é colocada imediatamente antes do fim da marca `</head>`.
    * Uma marca `<script>`, que referencia o arquivo minificado *wwwroot/main.\<hash\>.js*. Essa marca é colocada imediatamente antes do fim da marca `</body>`.
