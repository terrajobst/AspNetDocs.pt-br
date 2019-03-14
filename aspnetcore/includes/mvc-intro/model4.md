---
ms.openlocfilehash: 568fa161b27e554fd8b474670b8f9a7ec2f39f45
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57047123"
---
A tabela a seguir detalha os parâmetros do gerador de código do ASP.NET Core:

| Parâmetro               | Descrição|
| ----------------- | ------------ |
| -m  | O nome do modelo. |
| -dc  | O contexto de dados. |
| -udl | Use o layout padrão. |
| --relativeFolderPath | O caminho da pasta de saída relativa para criar as exibições. |
| --useDefaultLayout | O layout padrão deve ser usado para as exibições. |
| --referenceScriptLibraries | Adiciona `_ValidationScriptsPartial` para editar e criar páginas |

Use a opção `h` para obter ajuda sobre o comando `aspnet-codegenerator controller`:

```console
dotnet aspnet-codegenerator controller -h
```