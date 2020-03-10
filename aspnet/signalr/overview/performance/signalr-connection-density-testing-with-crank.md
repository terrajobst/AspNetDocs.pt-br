---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: Teste de densidade de conexão do signalr com pedais | Microsoft Docs
author: bradygaster
description: Testes de densidade de conexão do SignalR com Crank
ms.author: bradyg
ms.date: 02/22/2015
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: 901e039fbb81651ed18d560c99745b7e7f716e01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558334"
---
# <a name="signalr-connection-density-testing-with-crank"></a>Testes de densidade de conexão do SignalR com Crank

por [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> Este artigo descreve como usar a ferramenta de pedais para testar um aplicativo com vários clientes simulados.

Depois que o aplicativo estiver em execução em seu ambiente de hospedagem (uma função Web do Azure, o IIS ou o autohospedado usando Owin), você poderá testar a resposta do aplicativo para um alto nível de densidade de conexão usando a ferramenta de distribuição. O ambiente de hospedagem pode ser um servidor Serviços de Informações da Internet (IIS), um host Owin ou uma função Web do Azure. (Observação: os contadores de desempenho não estão disponíveis em aplicativos Web do Azure App Service, portanto, você não poderá obter dados de desempenho de um teste de densidade de conexão.)

A densidade de conexão refere-se ao número de conexões TCP simultâneas que podem ser estabelecidas em um servidor. Cada conexão TCP incorre em sua própria sobrecarga e a abertura de um grande número de conexões ociosas eventualmente criará um afunilamento de memória.

[A base de código do signalr](https://github.com/signalr/signalr) inclui uma ferramenta de teste de carga chamada **pedais**. A versão mais recente do pedais pode ser encontrada no [Branch de desenvolvimento](https://github.com/SignalR/signalr/tree/dev) no github. Você pode baixar um arquivo zip da ramificação de desenvolvimento da base de código do Signalr [aqui](https://github.com/SignalR/SignalR/archive/dev.zip).

O pedais pode ser usado para saturar totalmente a memória do servidor a fim de calcular o número total de conexões ociosas possíveis no hardware do servidor. Como alternativa, você também pode usar o pedais para testar a carga do servidor sob uma determinada quantidade de pressão de memória, aumentando as conexões até que uma contagem específica ou um limite de memória específico seja atingido.

Ao testar, é importante usar os clientes remotos para evitar qualquer competição por recursos (por exemplo, conexões TCP e memória). Monitore os clientes para garantir que eles não estejam atingindo nenhum afunilamento que possa impedir que o servidor atinja sua capacidade total (memória ou CPU). Pode ser necessário aumentar o número de clientes para carregar totalmente o servidor.

### <a name="running-a-connection-density-test"></a>Executando um teste de densidade de conexão

Esta seção descreve as etapas necessárias para executar um teste de densidade de conexão em um aplicativo Signalr.

1. Baixe e Compile a [ramificação de desenvolvimento da base de código do signalr](https://github.com/SignalR/SignalR/archive/dev.zip). Em um prompt de comando, navegue até &lt;diretório do projeto&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Implante seu aplicativo em seu ambiente de hospedagem pretendido. Anote o ponto de extremidade que seu aplicativo usa; por exemplo, no aplicativo criado no [tutorial introdução](../getting-started/tutorial-getting-started-with-signalr.md), o ponto de extremidade é `http://<yourhost>:8080/signalr`.
3. Instale [contadores de desempenho do signalr](signalr-performance.md#perfcounters) no servidor. Se seu aplicativo estiver em execução no Azure, consulte [usando contadores de desempenho do signalr em uma função Web do Azure](using-signalr-performance-counters-in-an-azure-web-role.md).

Depois de baixar e criar a base de código e instalar os contadores de desempenho em seu host, a ferramenta de linha de comando do pedais pode ser encontrada na pasta `src\Microsoft.AspNet.SignalR.Crank\bin\Debug`.

As opções disponíveis para a ferramenta de pedais incluem:

- **/?** : Mostra a tela de ajuda. As opções disponíveis também serão exibidas se o parâmetro de **URL** for omitido.
- **/URL**: a URL para conexões de signalr. Este parâmetro é obrigatório. Para um aplicativo Signalr usando o mapeamento padrão, o caminho terminará em "/signalr".
- **/Transport**: o nome do transporte usado. O padrão é `auto`, que selecionará o melhor protocolo disponível. As opções incluem `WebSockets`, `ServerSentEvents`e `LongPolling` (`ForeverFrame` não é uma opção para o pedais, já que o cliente .NET em vez do Internet Explorer é usado). Para obter mais informações sobre como o Signalr seleciona transportes, consulte [transportes e fallbacks](../getting-started/introduction-to-signalr.md#transports).
- **/BatchSize**: o número de clientes adicionados em cada lote. O padrão é 50.
- **/ConnectInterval**: o intervalo em milissegundos entre a adição de conexões. O padrão é 500.
- **/Connections**: o número de conexões usadas para carregar e testar o aplicativo. O padrão é 100.000.
- **/ConnectTimeout**: o tempo limite em segundos antes de abortar o teste. O padrão é 300.
- **MinServerMBytes**: o mínimo de megabytes do servidor a alcançar. O padrão é 500.
- **SendBytes**: o tamanho da carga enviada ao servidor em bytes. O padrão é 0.
- **SendInterval**: o atraso em milissegundos entre as mensagens para o servidor. O padrão é 500.
- **SendTimeout**: o tempo limite em milissegundos para mensagens para o servidor. O padrão é 300.
- **ControllerUrl**: a URL em que um cliente hospedará um Hub do controlador. O padrão é NULL (sem Hub do controlador). O Hub do controlador é iniciado quando a sessão de pedais é iniciada; Não é feito nenhum contato adicional entre o Hub do controlador e o pedais.
- **NumClients**: o número de clientes simulados para se conectar ao aplicativo. O padrão é um.
- **Logfile**: o FileName do arquivo de log para a execução de teste. O padrão é `crank.csv`.
- **SampleInterval**: o tempo em milissegundos entre os exemplos do contador de desempenho. O padrão é 1000.
- **SignalRInstance**: o nome da instância para os contadores de desempenho no servidor. O padrão é usar o estado de conexão do cliente.

### <a name="example"></a>Exemplo

O comando a seguir testará um site chamado `pfsignalr` no Azure que hospeda um aplicativo na porta 8080 com um Hub chamado "ControllerHub", usando conexões 100.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
