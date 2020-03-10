---
uid: signalr/overview/performance/scaleout-in-signalr
title: Introdução ao ScaleOut no Signalr | Microsoft Docs
author: bradygaster
description: Versões de software usadas neste tópico Visual Studio 2013 o .NET 4,5 Signalr versão 2 versões anteriores deste tópico para obter informações sobre versões anteriores do...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: 7e781fc1-1c1f-45a8-bc1d-338e96dbe9c9
msc.legacyurl: /signalr/overview/performance/scaleout-in-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14dc22f99a43b566903c59fb23b7d419350f4a25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579229"
---
# <a name="introduction-to-scaleout-in-signalr"></a>Introdução à expansão no SignalR

por [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> ## <a name="software-versions-used-in-this-topic"></a>Versões de software usadas neste tópico
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Sinalização versão 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versões anteriores deste tópico
>
> Para obter informações sobre versões anteriores do Signalr, confira [versões mais antigas do signalr](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Perguntas e comentários
>
> Deixe comentários sobre como você gostou deste tutorial e o que poderíamos melhorar nos comentários na parte inferior da página. Se você tiver dúvidas que não estão diretamente relacionadas ao tutorial, poderá lançá-las no fórum do [signalr ASP.net](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) ou [stackoverflow.com](http://stackoverflow.com/).

Em geral, há duas maneiras de dimensionar um aplicativo Web: *escalar verticalmente* e *escalar horizontalmente*.

- Escalar verticalmente significa usar um servidor maior (ou uma VM maior) com mais RAM, CPUs, etc.
- Scale out significa adicionar mais servidores para lidar com a carga.

O problema com o dimensionamento é que você rapidamente atingiu um limite no tamanho do computador. Além disso, você precisa escalar horizontalmente. No entanto, quando você expande horizontalmente, os clientes podem ser roteados para servidores diferentes. Um cliente que está conectado a um servidor não receberá mensagens enviadas de outro servidor.

![](scaleout-in-signalr/_static/image1.png)

Uma solução é encaminhar mensagens entre servidores, usando um componente chamado *backplane*. Com um backplane habilitado, cada instância do aplicativo envia mensagens para o backplane e o backplane as encaminha para as outras instâncias do aplicativo. (Em eletrônicos, um backplane é um grupo de conectores paralelos. Por analogia, um backplane sinalizador conecta vários servidores.)

![](scaleout-in-signalr/_static/image2.png)

Atualmente, o signalr fornece três backplanes:

- **Barramento de Serviço do Azure**. O barramento de serviço é uma infraestrutura de mensagens que permite que os componentes enviem mensagens de uma maneira menos rígida.
- **Redis**. Redis é um repositório de chave-valor na memória. O Redis dá suporte a um padrão de publicação/assinatura ("pub/sub") para enviar mensagens.
- **SQL Server**. O backplane SQL Server grava mensagens em tabelas SQL. O backplane usa Service Broker para mensagens eficientes. No entanto, ele também funcionará se o Service Broker não estiver habilitado.

Se você implantar seu aplicativo no Azure, considere usar o backplane Redis usando o [cache Redis do Azure](https://azure.microsoft.com/services/cache/). Se você estiver implantando em seu próprio farm de servidores, considere os backplanes SQL Server ou Redis.

Os tópicos a seguir contêm tutoriais passo a passo para cada backplane:

- [Expansão do SignalR com o Barramento de Serviço do Azure](scaleout-with-windows-azure-service-bus.md)
- [Expansão do SignalR com Redis](scaleout-with-redis.md)
- [Expansão do SignalR com o SQL Server](scaleout-with-sql-server.md)

## <a name="implementation"></a>Implementação

No Signalr, cada mensagem é enviada por meio de um barramento de mensagem. Um barramento de mensagem implementa a interface [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) , que fornece uma abstração de publicação/assinatura. As backplanes funcionam substituindo o padrão **IMessageBus** por um barramento projetado para esse backplane. Por exemplo, o barramento de mensagem para Redis é [RedisMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.redis.redismessagebus(v=vs.100).aspx)e usa o mecanismo Redis [pub/sub](http://redis.io/topics/pubsub) para enviar e receber mensagens.

Cada instância de servidor conecta-se ao backplane através do barramento. Quando uma mensagem é enviada, ela vai para o backplane e o backplane a envia para todos os servidores. Quando um servidor recebe uma mensagem do backplane, ele coloca a mensagem em seu cache local. Em seguida, o servidor entrega mensagens a clientes de seu cache local.

Para cada conexão de cliente, o progresso do cliente na leitura do fluxo de mensagens é rastreado usando um cursor. (Um cursor representa uma posição no fluxo de mensagens.) Se um cliente se desconectar e, em seguida, reconectar, ele solicitará o barramento para todas as mensagens que chegaram após o valor do cursor do cliente. A mesma coisa acontece quando uma conexão usa [sondagem longa](../getting-started/introduction-to-signalr.md#transports). Após a conclusão de uma solicitação de sondagem longa, o cliente abre uma nova conexão e solicita as mensagens que chegaram após o cursor.

O mecanismo de cursor funciona mesmo que um cliente seja roteado para um servidor diferente na reconexão. O backplane está ciente de todos os servidores e não importa a qual servidor um cliente se conecta.

## <a name="limitations"></a>Limitações

Usando um backplane, a taxa de transferência máxima da mensagem é menor do que quando os clientes falam diretamente com um único nó de servidor. Isso ocorre porque o backplane encaminha todas as mensagens para cada nó, de modo que o backplane pode se tornar um afunilamento. Se essa limitação é um problema depende do aplicativo. Por exemplo, aqui estão alguns cenários típicos do Signalr:

- Difusão de servidor (por exemplo, cotação de ações): os planos de [retransmissão](../getting-started/tutorial-server-broadcast-with-signalr.md) funcionam bem para esse cenário, pois o servidor controla a taxa na qual as mensagens são enviadas.
- [Cliente para cliente](../getting-started/tutorial-getting-started-with-signalr.md) (por exemplo, chat): nesse cenário, o backplane poderá ser um afunilamento se o número de mensagens for dimensionado com o número de clientes; ou seja, se a taxa de mensagens aumentar proporcionalmente à medida que mais clientes ingressarem.
- [Tempo real de alta frequência](../getting-started/tutorial-high-frequency-realtime-with-signalr.md) (por exemplo, jogos em tempo real): um backplane não é recomendado para esse cenário.

## <a name="enabling-tracing-for-signalr-scaleout"></a>Habilitando o rastreamento para o redimensionamento de sinalização

Para habilitar o rastreamento para os backplanes, adicione as seções a seguir ao arquivo Web. config, sob o elemento de **configuração** raiz:

[!code-html[Main](scaleout-in-signalr/samples/sample1.html)]
