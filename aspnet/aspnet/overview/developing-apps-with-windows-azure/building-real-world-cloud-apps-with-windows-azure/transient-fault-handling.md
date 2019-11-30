---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Tratamento de falhas transitórias (criando aplicativos de nuvem do mundo real com o Azure) | Microsoft Docs
author: MikeWasson
description: A criação de aplicativos de nuvem do mundo real com o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem...
ms.author: riande
ms.date: 11/03/2015
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: fc281e3d8f7c9edd4d98b029a67e58113132a8b3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74583651"
---
# <a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Tratamento de falhas transitórias (criando aplicativos de nuvem do mundo real com o Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar o livro eletrônico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> A **criação de aplicativos de nuvem do mundo real com** o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem ajudá-lo a desenvolver com sucesso aplicativos Web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).

Quando você está criando um aplicativo de nuvem do mundo real, uma das coisas que você precisa pensar é como lidar com interrupções de serviço temporário. Esse problema é exclusivamente importante em aplicativos de nuvem porque você depende de conexões de rede e de serviços externos. Com freqüência, você pode obter poucos problemas que normalmente são de auto-recuperação e, se você não estiver preparado para tratá-los de forma inteligente, eles resultarão em uma experiência inadequada para seus clientes.

## <a name="causes-of-transient-failures"></a>Causas de falhas transitórias

No ambiente de nuvem, você descobrirá que as conexões de banco de dados com falha e descartadas acontecem periodicamente. Isso é parcialmente porque você está passando por mais balanceadores de carga em comparação com o ambiente local onde o servidor Web e o servidor de banco de dados têm uma conexão física direta. Além disso, às vezes, quando você depende de um serviço de multilocatário, verá que as chamadas para o serviço ficam mais lentas ou com tempo limite porque outra pessoa que usa o serviço está atingindo muito. Em outros casos, você pode ser o usuário que está acessando o serviço com muita frequência e o serviço restrição deliberadamente – nega conexões – para evitar que você afete negativamente outros locatários do serviço.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Use a lógica de repetição/retirada inteligente para atenuar o efeito de falhas transitórias

Em vez de lançar uma exceção e exibir uma página de erro ou não disponível para o cliente, você pode reconhecer os erros que normalmente são transitórios e repetir automaticamente a operação que resultou no erro, na esperança de que, antes de longo, você seja bem-sucedido. Na maioria das vezes, a operação terá êxito na segunda tentativa, e você se recuperará do erro sem que o cliente já tenha se informado de que houve um problema.

Há várias maneiras de implementar a lógica de repetição inteligente.

- O grupo de práticas de &amp; de padrões da Microsoft tem um [bloco de aplicativo de tratamento de falhas transitórios](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) que faz tudo para você se você estiver usando ADO.net para acesso ao banco de dados SQL (não por meio de Entity Framework). Basta definir uma política para repetições – quantas vezes repetir uma consulta ou um comando e por quanto tempo aguardar entre as tentativas – e encapsular o código SQL em um bloco *using* .

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    O TFH também dá suporte ao [Azure cache na função](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) e ao [barramento de serviço](https://azure.microsoft.com/services/service-bus/).
- Quando você usa o Entity Framework normalmente não está trabalhando diretamente com conexões SQL, portanto, não é possível usar esse pacote de padrões e práticas, mas Entity Framework 6 compila esse tipo de lógica de repetição diretamente na estrutura. De maneira semelhante, você especifica a estratégia de repetição e, em seguida, o EF usa essa estratégia sempre que acessa o banco de dados.

    Para usar esse recurso no aplicativo Fix it, tudo o que precisamos fazer é adicionar uma classe derivada de *DbConfiguration* e ativar a lógica de repetição.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Para exceções do banco de dados SQL que a estrutura identifica como erros transitórios, o código mostrado instrui o EF a tentar novamente a operação até três vezes, com um atraso de retirada exponencial entre as repetições e um atraso máximo de 5 segundos. O retirada exponencial significa que, após cada repetição com falha, ele aguardará por um período de tempo maior antes de tentar novamente. Se três tentativas em uma linha falharem, ele gerará uma exceção. A seção a seguir sobre os separadores de circuito explica por que você deseja retirada exponencial e um número limitado de repetições.

    Você pode ter problemas semelhantes ao usar o serviço de armazenamento do Azure, como o aplicativo Fix it faz para BLOBs, e a API do cliente de armazenamento .NET já implementa o mesmo tipo de lógica. Você apenas especifica a política de repetição ou não precisa nem fazer isso se estiver satisfeito com as configurações padrão.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Disjuntores de circuito

Há várias razões pelas quais você não deseja repetir muitas vezes por um período longo demais:

- Muitos usuários repetindo de forma persistente as solicitações com falha podem prejudicar a experiência de outros usuários. Se milhões de pessoas estiverem fazendo solicitações repetidas de repetição, você poderá estar se vinculando a filas de expedição do IIS e impedindo que seu aplicativo atenda a solicitações que, de outra forma, pudesse lidar com êxito.
- Se todos estiverem tentando novamente devido a uma falha de serviço, pode haver tantas solicitações enfileiradas que o serviço é inundado quando ele começa a ser recuperado.
- Se o erro for devido à limitação e houver uma janela de tempo que o serviço usa para limitação, as tentativas contínuas podem mover essa janela para fora e fazer com que a limitação continue.
- Você pode ter um usuário aguardando que uma página da Web seja renderizada. Fazer as pessoas aguardarem muito tempo pode ser mais irritante que, de modo relativamente rápido, aconselha-las a tentar novamente mais tarde.

A retirada exponencial aborda alguns desses problemas limitando a frequência de novas tentativas que um serviço pode obter do seu aplicativo. Mas você também precisa ter separadores de *circuito*: isso significa que, em um determinado limite de repetição, seu aplicativo para de tentar novamente e executa alguma outra ação, como uma das seguintes:

- Fallback personalizado. Se não for possível obter um preço de estoque de Reuters, talvez você possa obtê-lo do Bloomberg; ou, se você não puder obter dados do banco de dado, talvez você possa obtê-los do cache.
- Falha silenciosa. Se o que você precisa de um serviço não é tudo ou nada para seu aplicativo, basta retornar NULL quando não for possível obter os dados. Se você estiver exibindo uma tarefa corrigir ti e o serviço BLOB não estiver respondendo, você poderá exibir os detalhes da tarefa sem a imagem.
- Falha rápida. Erro do usuário para evitar a inundação do serviço com solicitações de repetição que poderiam causar interrupção do serviço para outros usuários ou estender uma janela de limitação. Você pode exibir uma mensagem amigável "tentar novamente mais tarde".

Não há nenhuma política de repetição única para todos os tamanhos. Você pode repetir mais vezes e aguardar mais tempo em um processo de trabalho em segundo plano assíncrono do que em um aplicativo Web síncrono em que um usuário está aguardando uma resposta. Você pode esperar mais tempo entre as repetições de um serviço de banco de dados relacional do que você faria para um serviço de cache. Aqui estão alguns exemplos de políticas de repetição recomendadas para dar uma ideia de como os números podem variar. ("Fast First" significa sem atraso antes da primeira repetição.

![Políticas de repetição de exemplo](transient-fault-handling/_static/image1.png)

Para obter diretrizes de política de repetição do banco de dados SQL, consulte [solucionar falhas transitórias e erros de conexão para o banco de dados SQL](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Resumo

Uma estratégia de repetição/retirada pode ajudar a tornar os erros temporários visíveis para o cliente na maior parte do tempo, e a Microsoft fornece estruturas que você pode usar para minimizar o trabalho que está implementando uma estratégia, independentemente de você estar usando o ADO.NET, Entity Framework ou o serviço de armazenamento do Azure.

No [próximo capítulo](distributed-caching.md), veremos como melhorar o desempenho e a confiabilidade usando o Caching distribuído.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os seguintes recursos:

Documentação

- [Práticas recomendadas para o design de serviços em grande escala nos serviços de nuvem do Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White Paper por Mark Simms e Michael Thomassy. Semelhante à série FailSafe, mas apresenta detalhes mais detalhados. Consulte a seção telemetria e diagnóstico.
- [FailSafe: Diretrizes para arquiteturas de nuvem resilientes](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White Paper por Marc Mercuri, Ulrich Homann e Andrew TOWNHILL. Versão da página da Web da série de vídeos FailSafe.
- [Práticas e padrões da Microsoft – diretrizes do Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte padrão de repetição, padrão de supervisor de agente do Agendador.
- [Tolerância a falhas no banco de dados SQL do Azure](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Postagem do blog de Tony Petrossian.
- [Entity Framework-resiliência de conexão/lógica de repetição](https://msdn.microsoft.com/data/dn456835). Como usar e personalizar o recurso de tratamento de falhas transitórias do Entity Framework 6.
- [Resiliência de conexão e interceptação de comando com o Entity Framework em um aplicativo MVC ASP.net](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). O quarto de uma série de tutoriais de nove partes, mostra como configurar o recurso de resiliência de conexão do EF 6 para o banco de dados SQL.

Vídeos

- [FailSafe: criando serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de nove partes de Ulrich Homann, Marc Mercuri e de Mark Simms. Apresenta conceitos de alto nível e princípios arquitetônicos de uma maneira muito acessível e interessante, com histórias extraídas da experiência CAT (equipe de consultoria para clientes) da Microsoft com os clientes reais. Consulte a discussão de disjuntores de circuito no episódio 3 a partir de 40:55.
- [Criando Big: lições aprendidas de clientes do Azure-parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms fala sobre o design de falhas, tratamento de falhas transitórias e instrumentação de tudo.

Exemplos de código

- [Conceitos básicos do serviço de nuvem no Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicativo de exemplo criado pelo Microsoft Azure equipe de consultoria ao cliente que demonstra como usar o TFH ( [bloco de tratamento de falhas transitórias) da Enterprise Library](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) . Para obter mais informações, consulte [camada de acesso a dados dos conceitos básicos do serviço de nuvem – tratamento de falhas transitórias](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH é recomendado para acesso ao banco de dados usando o ADO.NET diretamente (sem usar Entity Framework).

> [!div class="step-by-step"]
> [Anterior](monitoring-and-telemetry.md)
> [Próximo](distributed-caching.md)
