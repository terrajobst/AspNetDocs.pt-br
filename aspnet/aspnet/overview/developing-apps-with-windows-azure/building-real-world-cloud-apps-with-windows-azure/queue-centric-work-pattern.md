---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Padrão de trabalho centrado em fila (criando aplicativos de nuvem do mundo real com o Azure) | Microsoft Docs
author: MikeWasson
description: A criação de aplicativos de nuvem do mundo real com o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: c73b070f11366e781bcea70ffc84fd49a47d469a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74582757"
---
# <a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Padrão de trabalho centrado em fila (criando aplicativos de nuvem do mundo real com o Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar o livro eletrônico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> A **criação de aplicativos de nuvem do mundo real com** o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem ajudá-lo a desenvolver com sucesso aplicativos Web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).

Anteriormente, vimos que o uso de vários serviços pode resultar em um SLA "composto", no qual o SLA efetivo do aplicativo é o *produto* dos SLAs individuais. Por exemplo, o aplicativo corrigir ti usa sites da Web, armazenamento e banco de dados SQL. Se qualquer um desses serviços falhar, o aplicativo retornará um erro ao usuário.

O Caching é uma boa maneira de lidar com falhas transitórias para conteúdo somente leitura. Mas e se seu aplicativo precisar fazer o trabalho? Por exemplo, quando o usuário envia uma nova tarefa corrigir ti, o aplicativo não pode simplesmente colocar a tarefa no cache. O aplicativo precisa gravar a tarefa corrigir em um repositório de dados persistente, para que possa ser processada.

É aí que entra o padrão de trabalho centrado na fila. Esse padrão permite um acoplamento flexível entre uma camada da Web e um serviço de back-end.

Veja como o padrão funciona. Quando o aplicativo recebe uma solicitação, ele coloca um item de trabalho em uma fila e retorna imediatamente a resposta. Em seguida, um processo de back-end separado extrai itens de trabalho da fila e faz o trabalho.

O padrão de trabalho centrado em fila é útil para:

- Trabalho que é demorado (alta latência).
- Trabalho que requer um serviço externo que talvez nem sempre esteja disponível.
- Trabalhe com uso intensivo de recursos (alta CPU).
- O trabalho que se beneficiaria do nivelamento de taxa (sujeito a picos de carga repentinos).

## <a name="reduced-latency"></a>Latência reduzida

As filas são úteis sempre que você estiver realizando trabalho demorado. Se uma tarefa levar alguns segundos ou mais, em vez de bloquear o usuário final, coloque o item de trabalho em uma fila. Diga ao usuário "Estamos trabalhando nele" e, em seguida, use um ouvinte de fila para processar a tarefa em segundo plano.

Por exemplo, quando você compra algo em um varejista online, o site confirma seu pedido imediatamente. Mas isso não significa que suas coisas já estão em um caminhão sendo entregue. Eles colocam uma tarefa em uma fila e, em segundo plano, estão fazendo a verificação de crédito, preparando seus itens para envio e assim por diante.

Para cenários com latência curta, o tempo total de ponta a ponta pode ser mais usado em uma fila, comparada com a execução da tarefa de forma síncrona. Mas mesmo assim, os outros benefícios podem superar essa desvantagem.

## <a name="increased-reliability"></a>Maior confiabilidade

Na versão do Fix it que temos examinado até agora, o front-end da Web está rigidamente acoplado ao back-end do banco de dados SQL. Se o serviço do banco de dados SQL não estiver disponível, o usuário receberá um erro. Se as repetições não funcionarem (ou seja, a falha é mais do que transitória), a única coisa que você pode fazer é mostrar um erro e pedir que o usuário tente novamente mais tarde.

![Diagrama mostrando a falha de front-end da Web quando o back-end do banco de dados SQL falha](queue-centric-work-pattern/_static/image1.png)

Usando filas, quando um usuário envia uma tarefa corrigir, o aplicativo grava uma mensagem na fila. A carga da mensagem é uma representação [JSON](http://json.org/) da tarefa. Assim que a mensagem for gravada na fila, o aplicativo retornará e mostrará imediatamente uma mensagem de êxito ao usuário.

Se qualquer um dos serviços de back-end, como o banco de dados SQL ou o ouvinte de fila, ficar offline, os usuários ainda poderão enviar novas tarefas de correção de ti. As mensagens só serão colocadas em fila até que os serviços de back-end estejam disponíveis novamente. Nesse ponto, os serviços de back-end se acumularão na pendência.

![Diagrama mostrando o front-end da Web continuando a funcionar quando há um erro de banco de dados SQL](queue-centric-work-pattern/_static/image2.png)

Além disso, agora você pode adicionar mais lógica de back-end sem se preocupar com a resiliência do front-end. Por exemplo, talvez você queira enviar uma mensagem de email ou SMS para o proprietário sempre que uma nova correção for atribuída. Se o email ou o serviço SMS ficar indisponível, você poderá processar todo o resto e, em seguida, colocar uma mensagem em uma fila separada para enviar mensagens de email/SMS.

Anteriormente, nosso SLA eficaz era aplicativos Web &times; armazenamento &times; banco de dados SQL = 99,7%. (Consulte [design para sobreviver a falhas](design-to-survive-failures.md).)

Quando alteramos o aplicativo para usar uma fila, o front-end da Web depende apenas de aplicativos Web e armazenamento, para um SLA composto de 99,8%. (Observe que as filas fazem parte do serviço de armazenamento do Azure, para que elas sejam incluídas no mesmo SLA que o armazenamento de BLOBs.)

Se você precisar de maneira ainda melhor do que 99,8%, poderá criar duas filas em duas regiões diferentes. Designe um como o primário e o outro como o secundário. Em seu aplicativo, faça failover para a fila secundária se a fila primária não estiver disponível. A possibilidade de ambos estarem indisponíveis ao mesmo tempo é muito pequena.

## <a name="rate-leveling-and-independent-scaling"></a>Nivelamento de taxa e dimensionamento independente

As filas também são úteis para algo chamado *nivelamento de taxa* ou *nivelamento de carga*.

Os aplicativos Web geralmente são suscetíveis a picos repentinos no tráfego. Embora você possa usar o dimensionamento automático para adicionar automaticamente servidores Web para lidar com o aumento do tráfego da Web, a autoescala pode não ser capaz de reagir rapidamente o suficiente para lidar com picos repentinos na carga. Se os servidores Web puderem descarregar parte do trabalho que precisam fazer gravando uma mensagem em uma fila, eles poderão lidar com mais tráfego. Um serviço de back-end pode ler mensagens da fila e processá-las. A profundidade da fila aumentará ou diminuirá conforme a carga de entrada varia.

Com a maior parte de seu trabalho demorado, carregado para um serviço de back-end, a camada da Web pode responder mais facilmente a picos repentinos no tráfego. E você economiza dinheiro porque qualquer quantidade determinada de tráfego pode ser tratada por menos servidores Web.

Você pode dimensionar a camada da Web e o serviço de back-end independentemente. Por exemplo, você pode precisar de três servidores Web, mas apenas um servidor processa mensagens da fila. Ou, se você estiver executando uma tarefa de computação intensiva em segundo plano, talvez precise de mais servidores back-end.

![](queue-centric-work-pattern/_static/image3.png)

O dimensionamento automático funciona com os serviços de back-end, bem como com a camada da Web. Você pode escalar ou reduzir verticalmente o número de VMs que estão processando as tarefas na fila, com base no uso da CPU das VMs de back-end. Ou, você pode dimensionar automaticamente com base em quantos itens estão em uma fila. Por exemplo, você pode informar ao dimensionamento automático para tentar manter no máximo 10 itens na fila. Se a fila tiver mais de 10 itens, o dimensionamento automático adicionará VMs. Quando eles forem atualizados, o dimensionamento automático subdividirá as VMs extras.

## <a name="adding-queues-to-the-fix-it-application"></a>Adicionando filas ao aplicativo corrigir ti

Para implementar o padrão de fila, precisamos fazer duas alterações no aplicativo Fix it.

- Quando um usuário envia uma nova tarefa corrigir ti, coloca a tarefa na fila, em vez de gravá-la no banco de dados.
- Crie um serviço de back-end que processa mensagens na fila.

Para a fila, usaremos o serviço de [armazenamento de filas do Azure](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Outra opção é usar o [barramento de serviço do Azure](https://docs.microsoft.com/azure/service-bus/).

Para decidir qual serviço de fila usar, considere como seu aplicativo precisa enviar e receber as mensagens na fila:

- Se você tiver produtores de cooperação e consumidores concorrentes, considere usar o serviço de armazenamento de filas do Azure. "Produtores de cooperação" significa que vários processos estão adicionando mensagens a uma fila. "Consumidores concorrentes" significa que vários processos estão extraindo as mensagens da fila para processá-las, mas qualquer mensagem pode ser processada apenas por um "consumidor". Se você precisar de mais taxa de transferência do que pode obter com uma única fila, use filas adicionais e/ou contas de armazenamento adicionais.
- Se você precisar de um [modelo de publicação/assinatura](http://en.wikipedia.org/wiki/Publish/subscribe), considere o uso de filas do barramento de serviço do Azure.

O aplicativo corrigir ti se ajusta aos produtores de cooperação e aos consumidores concorrentes.

Outra consideração é a disponibilidade do aplicativo. O serviço de armazenamento de filas faz parte do mesmo serviço que estamos usando para armazenamento de BLOBs, portanto, usá-lo não tem nenhum efeito sobre nosso SLA. O barramento de serviço do Azure é um serviço separado com seu próprio SLA. Se utilizamos filas do barramento de serviço, teríamos que considerar uma porcentagem adicional de SLA, e nosso SLA de composição seria menor. Quando você estiver escolhendo um serviço fila, certifique-se de entender o impacto de sua escolha na disponibilidade do aplicativo. Para obter mais informações, consulte a seção [recursos](#resources) .

## <a name="creating-queue-messages"></a>Criando mensagens da fila

Para colocar uma tarefa corrigir na fila, o front-end da Web executa as seguintes etapas:

1. Crie uma instância de [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) . A instância de `CloudQueueClient` é usada para executar solicitações no serviço fila.
2. Crie a fila, caso ela ainda não exista.
3. Serialize a tarefa corrigir ti.
4. Chame [CloudQueue. AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) para colocar a mensagem na fila.

Faremos esse trabalho no construtor e `SendMessageAsync` método de uma nova classe `FixItQueueManager`.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Aqui estamos usando a biblioteca [JSON.net](https://github.com/JamesNK/Newtonsoft.Json) para serializar o FixIt para o formato JSON. Você pode usar qualquer abordagem de serialização que preferir. O JSON tem a vantagem de ser legível, embora seja menos detalhado do que XML.

Código de qualidade de produção adicionaria lógica de tratamento de erros, pausar se o banco de dados ficou indisponível, tratar a recuperação mais corretamente, criar a fila na inicialização do aplicativo e gerenciar[mensagens "suspeitas"](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Uma mensagem suspeita é uma mensagem que não pode ser processada por algum motivo. Você não quer que as mensagens suspeitas estejam na fila, em que a função de trabalho tentará processá-las continuamente, falhar, tentar novamente, falhar e assim por diante.)

No aplicativo MVC de front-end, precisamos atualizar o código que cria uma nova tarefa. Em vez de colocar a tarefa no repositório, chame o método `SendMessageAsync` mostrado acima.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Processando mensagens da fila

Para processar mensagens na fila, criaremos um serviço de back-end. O serviço de back-end executará um loop infinito que executa as seguintes etapas:

1. Obter a próxima mensagem da fila.
2. Desserializar a mensagem para uma tarefa corrigir ti.
3. Grave a tarefa corrigir ti no banco de dados.

Para hospedar o serviço de back-end, criaremos um serviço de nuvem do Azure que contém uma *função de trabalho*. Uma função de trabalho consiste em uma ou mais VMs que podem fazer o processamento de back-end. O código que é executado nessas VMs efetuará pull das mensagens da fila à medida que elas forem disponibilizadas. Para cada mensagem, desserializaremos a carga JSON e Gravaremos uma instância da entidade de tarefa corrigir ti no banco de dados, usando o mesmo repositório que usamos anteriormente na camada da Web.

As etapas a seguir mostram como adicionar um projeto de função de trabalho a uma solução que tem um projeto Web padrão. Essas etapas já foram feitas no projeto Fix it que você pode baixar.

Primeiro, adicione um projeto de serviço de nuvem à solução do Visual Studio. Clique com o botão direito do mouse na solução e selecione **Adicionar**e **novo projeto**. No painel esquerdo, expanda **Visual C#**  e selecione **nuvem**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

Na caixa de diálogo **novo serviço de nuvem do Azure** , expanda o nó **Visual C#**  no painel esquerdo. Selecione **função de trabalho** e clique no ícone de seta para a direita.

![](queue-centric-work-pattern/_static/image6.png)

(Observe que você também pode adicionar uma *função Web*. Poderíamos executar a correção de front-end no mesmo serviço de nuvem em vez de executá-lo em um site do Azure. Isso tem algumas vantagens em tornar as conexões entre front-end e back-end mais fáceis de coordenar. No entanto, para manter essa demonstração simples, estamos mantendo o front-end em um aplicativo Web de Azure App serviço e apenas executando o back-end em um serviço de nuvem.)

Um nome padrão é atribuído à função de trabalho. Para alterar o nome, passe o mouse sobre a função de trabalho no painel direito e clique no ícone de lápis.

![](queue-centric-work-pattern/_static/image7.png)

Clique em **OK** para concluir a caixa de diálogo. Isso adiciona dois projetos à solução do Visual Studio.

- um projeto do Azure que define o serviço de nuvem, incluindo informações de configuração.
- Um projeto de função de trabalho que define a função de trabalho.

![](queue-centric-work-pattern/_static/image8.png)

Para obter mais informações, consulte [criando um projeto do Azure com o Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

Dentro da função de trabalho, pesquisamos mensagens chamando o método `ProcessMessageAsync` da classe `FixItQueueManager` que vimos anteriormente.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

O método `ProcessMessagesAsync` verifica se há uma mensagem aguardando. Se houver um, ele desserializa a mensagem em uma entidade `FixItTask` e salva a entidade no banco de dados. Ele executa um loop até que a fila esteja vazia.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

A sondagem de mensagens da fila gera um pequeno encargo de transação, portanto, quando não há nenhuma mensagem aguardando para ser processada, o método `RunAsync` da função de trabalho aguarda um segundo antes de fazer a sondagem novamente chamando `Task.Delay(1000)`.

Em um projeto Web, a adição de código assíncrono pode melhorar o desempenho automaticamente, pois o IIS gerencia um pool de threads limitado. Esse não é o caso em um projeto de função de trabalho. Para melhorar a escalabilidade da função de trabalho, você pode escrever código multi-threaded ou usar código assíncrono para implementar a [programação paralela](https://msdn.microsoft.com/library/ff963553.aspx). O exemplo não implementa a programação paralela, mas mostra como tornar o código assíncrono para que você possa implementar a programação paralela.

## <a name="summary"></a>Resumo

Neste capítulo, você viu como melhorar a capacidade de resposta, a confiabilidade e a escalabilidade do aplicativo implementando o padrão de trabalho centrado em fila.

Este é o último dos 13 padrões abordados neste livro eletrônico, mas há, naturalmente, muitos outros padrões e práticas que podem ajudá-lo a criar aplicativos de nuvem bem-sucedidos. O [capítulo final](more-patterns-and-guidance.md) fornece links para recursos para tópicos que não foram abordados nesses 13 padrões.

<a id="resources"></a>
## <a name="resources"></a>Recursos

Para obter mais informações sobre filas, consulte os recursos a seguir.

Documentação:

- [Armazenamento do Microsoft Azure filas parte 1: introdução](https://www.red-gate.com/simple-talk/cloud/platform-as-a-service/microsoft-azure-storage-queues-part-1-getting-started/). Artigo por Roman Schacherl.
- [Executando tarefas em segundo plano](https://msdn.microsoft.com/library/ff803365.aspx), o capítulo 5 da [movimentação de aplicativos para a nuvem, a 3ª edição](https://msdn.microsoft.com/library/ff728592.aspx) de padrões e práticas da Microsoft. (Em particular, a seção ["usando filas de armazenamento do Azure"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Práticas recomendadas para maximizar a escalabilidade e a eficiência de custo de soluções de mensagens baseadas em fila no Azure](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). White Paper de Valery Mizonov.
- [Comparando filas do Azure e filas do barramento de serviço](https://msdn.microsoft.com/magazine/jj159884.aspx). O artigo da MSDN Magazine fornece informações adicionais que podem ajudá-lo a escolher qual serviço de fila usar. O artigo menciona que o barramento de serviço depende do ACS para autenticação, o que significa que as filas do SB não estarão disponíveis quando o ACS não estiver disponível. No entanto, como o artigo foi escrito, o SB foi alterado para permitir que você use [tokens SAS](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) como uma alternativa ao ACS.
- [Práticas e padrões da Microsoft – diretrizes do Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte primer de mensagens assíncronas, padrões de pipes e filtros, padrão de transação de compensação, padrão de consumidores concorrentes, padrão CQRS.
- [Jornada de CQRS](https://msdn.microsoft.com/library/jj554200). Livro eletrônico sobre o CQRS por padrões e práticas da Microsoft.

Vídeo:

- [FailSafe: criando serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de vídeos de nove partes por Ulrich Homann, Marc Mercuri e marcas Simms. Apresenta conceitos de alto nível e princípios arquitetônicos de uma maneira muito acessível e interessante, com histórias extraídas da experiência CAT (equipe de consultoria para clientes) da Microsoft com os clientes reais. Para obter uma introdução ao serviço de armazenamento do Azure e filas, consulte Episódio 5 a partir de 35:13.

> [!div class="step-by-step"]
> [Anterior](distributed-caching.md)
> [Próximo](more-patterns-and-guidance.md)
