---
ms.openlocfilehash: 6b2bc386ec179e786de205af0ca6dbd610e000d9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57056483"
---
# <a name="aspnet-core-background-tasks-sample-generic-host"></a>Exemplo de tarefas em segundo plano do ASP.NET Core (host genérico)

Este exemplo ilustra o uso de [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice). Este exemplo demonstra os recursos descritos no tópico [Tarefas em segundo plano com serviços hospedados no ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services).

Ao executar o exemplo no [Visual Studio Code](https://code.visualstudio.com/), defina o valor **console** da configuração de console em *.vscode/launch.json* como `externalTerminal` ou `integratedTerminal`. Usar o `internalConsole` é incompatível com a entrada por pressionamento de tecla do console que o aplicativo usa para enfileirar itens de trabalho em segundo plano.
