---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Design para sobreviver a falhas (criando aplicativos de nuvem do mundo real com o Azure) | Microsoft Docs
author: MikeWasson
description: A criação de aplicativos de nuvem do mundo real com o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 9bf9acb8b4f8521d03c053c124c5fc4a07d6cb9a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585662"
---
# <a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Design para sobreviver a falhas (criando aplicativos de nuvem do mundo real com o Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar o livro eletrônico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> A **criação de aplicativos de nuvem do mundo real com** o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem ajudá-lo a desenvolver com sucesso aplicativos Web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).

Uma das coisas que você precisa pensar ao criar qualquer tipo de aplicativo, mas especialmente um que será executado na nuvem em que muitas pessoas vão usá-lo, é como projetar o aplicativo para que ele possa lidar normalmente com falhas e continuar a fornecer valor tanto quanto possível. Com tempo suficiente, as coisas vão incorretas em qualquer ambiente ou em qualquer sistema de software. O modo como seu aplicativo lida com essas situações determina o quanto os clientes terão e quanto tempo você terá para passar a analisar e corrigir problemas.

## <a name="types-of-failures"></a>Tipos de falhas

Há duas categorias básicas de falhas que você desejará manipular de maneira diferente:

- Falhas transitórias e de auto-recuperação, como problemas intermitentes de conectividade de rede.
- Ocorreram falhas que exigem intervenção.

Para falhas transitórias, você pode implementar uma política de repetição para garantir que a maior parte do tempo que o aplicativo recupera de forma rápida e automática. Seus clientes podem notar um tempo de resposta um pouco mais longo, mas, caso contrário, eles não serão afetados. Mostraremos algumas maneiras de lidar com esses erros no [capítulo tratamento de falhas transitórias](transient-fault-handling.md).

Em caso de falhas, você pode implementar a funcionalidade de monitoramento e registro em log que notifica você imediatamente quando surgirem problemas e facilitar a análise da causa raiz. Mostraremos algumas maneiras de ajudá-lo a se manter em cima desses tipos de erros no [capítulo monitoramento e telemetria](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Escopo de falha

Você também precisa pensar sobre o escopo de falha – se um único computador é afetado, um serviço inteiro, como banco de dados SQL ou armazenamento, ou uma região inteira.

![Escopo de falha](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Falhas de máquina

No Azure, um servidor com falha é substituído automaticamente por um novo e um aplicativo de nuvem bem projetado se recupera desse tipo de falha de forma automática e rápida. Anteriormente, analisamos os benefícios de escalabilidade de uma camada da Web sem estado, e a facilidade de recuperação de um servidor com falha é outro benefício do sem estado. A facilidade de recuperação também é um dos benefícios dos recursos de PaaS (plataforma como serviço), como o banco de dados SQL e os aplicativos Web do serviço de Azure App. As falhas de hardware são raras, mas quando ocorrem, esses serviços os manipulam automaticamente; Você nem precisa escrever código para lidar com falhas de máquina quando estiver usando um desses serviços.

### <a name="service-failures"></a>Falhas de serviço

Os aplicativos de nuvem normalmente usam vários serviços. Por exemplo, o aplicativo corrigir ti usa o serviço de banco de dados SQL, o serviço de armazenamento e o aplicativo Web é implantado no serviço Azure App. O que o seu aplicativo fará se um dos serviços dos quais você depende falhar? Para algumas falhas de serviço, uma mensagem amigável "Desculpe, tentar novamente mais tarde" pode ser o melhor que você pode fazer. Mas, em muitos cenários, você pode fazer melhor. Por exemplo, quando o armazenamento de dados de back-end está inativo, você pode aceitar a entrada do usuário, exibir "sua solicitação foi recebida" e armazenar a entrada em outro lugar temporariamente; em seguida, quando o serviço de que você precisa estiver operacional novamente, você poderá recuperar a entrada e processá-la.

O capítulo de [padrão de trabalho centrado em fila](queue-centric-work-pattern.md) mostra uma maneira de lidar com esse cenário. O aplicativo corrigir ti armazena tarefas no banco de dados SQL, mas não precisa fechar o trabalho quando o banco de dados SQL está inoperante. Nesse capítulo, veremos como armazenar a entrada do usuário para uma tarefa em uma fila e usar um processo de trabalho para ler a fila e atualizar a tarefa. Se o SQL estiver inativo, a capacidade de criar as tarefas de correção de ti não será afetada; o processo de trabalho pode aguardar e processar novas tarefas quando o banco de dados SQL está disponível.

### <a name="region-failures"></a>Falhas de região

Regiões inteiras podem falhar. Um desastre natural pode destruir um data center, ele pode ser nivelado por um Meteor, a linha de tronco no datacenter pode ser recortada por um pecuarista que está disparando um vaca com um escavadeira, etc. Se seu aplicativo estiver hospedado no datacenter do stricken, o que você faz? É possível configurar seu aplicativo no Azure para ser executado em várias regiões simultaneamente para que, se houver um desastre em um, você continue executando em outra região. Essas falhas são ocorrências extremamente raras e a maioria dos aplicativos não percorre os arcos necessários para garantir o serviço ininterrupto por meio de falhas desse tipo. Consulte a seção recursos no final do capítulo para obter informações sobre como manter seu aplicativo disponível mesmo por meio de uma falha de região.

Uma meta do Azure é tornar a manipulação de todos esses tipos de falhas muito mais fácil, e você verá alguns exemplos de como estamos fazendo isso nos capítulos a seguir.

## <a name="slas"></a>Elimina

As pessoas geralmente ouvem os SLAs (contratos de nível de serviço) no ambiente de nuvem. Basicamente, essas são as promessas de que as empresas fazem sobre a confiabilidade de seu serviço. Um SLA de 99,9% significa que você deve esperar que o serviço esteja funcionando corretamente 99,9% do tempo. Esse é um valor bastante típico para um SLA e parece ser um número muito alto, mas você pode não perceber quanto tempo de 0,1% realmente vale para. Aqui está uma tabela que mostra quanto tempo de inatividade vários percentuais de SLA valem para mais de um ano, um mês e uma semana.

![Tabela SLA](design-to-survive-failures/_static/image2.png)

Portanto, um SLA de 99,9% significa que seu serviço pode estar inoperante em 8,76 horas por ano ou 43,2 minutos por mês. É mais tempo do que a maioria das pessoas percebe. Assim, como um desenvolvedor, você deseja estar ciente de que uma determinada quantidade de tempo de inatividade é possível e tratá-lo de uma maneira normal. Em algum momento, alguém vai usar seu aplicativo, e um serviço será desativado e você desejará minimizar o impacto negativo disso no cliente.

Uma coisa que você deve saber sobre um SLA é a qual período ele se refere: o relógio é redefinido toda semana, todos os meses ou todos os anos? No Azure, redefinimos o relógio todos os meses, o que é melhor para você do que um SLA anual, uma vez que um SLA anual pode ocultar meses incorretos, escondendo-os com uma série de bons meses.

É claro que sempre almeje a fazer melhor do que o SLA; Normalmente, você ficará muito menor do que isso. A promessa é que, se estivermos sempre inativo por mais tempo do que o máximo, você poderá pedir dinheiro de volta. A quantidade de dinheiro que você obtém, provavelmente, não comprometerá totalmente o impacto nos negócios do excesso de tempo de inatividade, mas esse aspecto do SLA atua como uma política de imposição e permite que você saiba que levamos muito a sério.

### <a name="composite-slas"></a>SLAs compostos

Um aspecto importante a ser pensado quando você está olhando para os SLAs é o impacto de usar vários serviços em um aplicativo, sendo que cada serviço tem um SLA separado. Por exemplo, o aplicativo corrigir ti usa Azure App aplicativos Web do serviço, o armazenamento do Azure e o banco de dados SQL. Aqui estão seus números de SLA a partir da data em que este e-book está sendo gravado em dezembro de 2013:

![Site de SLA, armazenamento, banco de dados SQL](design-to-survive-failures/_static/image3.png)

Qual é o tempo máximo de inatividade que você esperaria para o aplicativo com base nesses SLAs de serviço? Você pode imaginar que o tempo de inatividade seria igual ao pior percentual de SLA, ou 99,9% nesse caso. Isso seria verdadeiro se todos os três serviços sempre falharem ao mesmo tempo, mas isso não é necessariamente o que realmente acontece. Cada serviço pode falhar de forma independente em momentos diferentes, portanto, você precisa calcular o SLA composto multiplicando os números de SLA individuais.

![SLA composto](design-to-survive-failures/_static/image4.png)

Portanto, seu aplicativo pode ficar inoperante não apenas 43,2 minutos por mês, mas 3 vezes esse valor, 108 minutos por mês, e ainda estar dentro dos limites de SLA do Azure.

Esse problema não é exclusivo do Azure. Na verdade, fornecemos os melhores SLAs de nuvem de qualquer serviço de nuvem disponível, e você terá problemas semelhantes para lidar se usar os serviços de nuvem de qualquer fornecedor. O que esses destaques é a importância de pensar sobre como você pode projetar seu aplicativo para lidar com as falhas de serviço inevitáveis normalmente, porque elas podem ocorrer com frequência suficiente para afetar seus clientes ou usuários.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>SLAs de nuvem em comparação com a experiência de tempo de vida empresarial

Às vezes, as pessoas dizem, "em meu aplicativo empresarial, nunca tenho esses problemas". Se você perguntar quanto tempo de inatividade um mês realmente tem, eles geralmente dizem, "bem, isso acontece ocasionalmente". E se você perguntar com que frequência, eles admitirão que "às vezes, precisamos fazer backup ou instalar um novo servidor ou atualizar o software". É claro que isso conta como tempo de inatividade. A maioria dos aplicativos empresariais, a menos que sejam especialmente críticos, estão na verdade por mais tempo do que os SLAs de serviço. Mas, quando é o seu servidor e sua infraestrutura e você é responsável por ele e, no controle, você tende a sentir menos comoção sobre os tempos de inatividade. Em um ambiente de nuvem, você depende de outra pessoa e não sabe o que está acontecendo, portanto, talvez você queira se preocupar mais com ela.

Quando uma empresa atinge uma porcentagem maior de tempo de inversão do que você obtém de um SLA de nuvem, ele faz isso gastando muito mais dinheiro em hardware. Um serviço de nuvem poderia fazer isso, mas teria de cobrar muito mais por seus serviços. Em vez disso, você tira proveito de um serviço econômico e cria seu software para que as falhas inevitáveis causem interrupção mínima para seus clientes. Seu trabalho como um designer de aplicativo de nuvem não é muito para evitar uma falha ao evitar catástrofe e você faz isso concentrando-se em software, não em hardware. Enquanto os aplicativos empresariais se esforçam para maximizar o tempo médio entre falhas, os aplicativos de nuvem se esforçam para minimizar o tempo médio de recuperação.

### <a name="not-all-cloud-services-have-slas"></a>Nem todos os serviços de nuvem têm SLAs

Lembre-se também de que nem todos os serviços de nuvem têm um SLA. Se seu aplicativo for dependente de um serviço sem garantia de tempo de atividade, você poderá estar muito mais demorado do que você imagina. Por exemplo, se você habilitar o logon em seu site usando um provedor social, como o Facebook ou o Twitter, verifique com o provedor de serviços se há um SLA, e você pode descobrir que não há um. Mas se o serviço de autenticação ficar inativo ou não puder dar suporte ao volume de solicitações que você lança, seus clientes serão bloqueados de seu aplicativo. Você pode estar inoperante por dias ou mais. Os criadores de um novo aplicativo esperavam centenas de milhões de downloads e tiveram uma dependência na autenticação do Facebook – mas não conversava com o Facebook antes de ficar em tempo real e descoberto muito tarde que não havia nenhum SLA para esse serviço.

### <a name="not-all-downtime-counts-toward-slas"></a>Nem todas as contagens de tempo de inatividade para SLAs

Alguns serviços de nuvem podem negar deliberadamente o serviço se seu aplicativo o utiliza. Isso é chamado de *limitação*. Se um serviço tiver um SLA, ele deverá declarar as condições sob as quais você pode ser limitado, e o design do aplicativo deverá evitar essas condições e reagir adequadamente à limitação se isso acontecer. Por exemplo, se as solicitações para um serviço começarem a falhar quando você exceder um determinado número por segundo, você deseja garantir que as tentativas automáticas não ocorram tão rapidamente que façam com que a limitação continue. Teremos mais a dizer sobre a limitação no capítulo de [tratamento de falhas transitórias](transient-fault-handling.md).

## <a name="summary"></a>Resumo

Este capítulo tentou ajudá-lo a perceber por que um aplicativo de nuvem real precisa ser projetado para sobreviver a falhas normalmente. A partir do [próximo capítulo](monitoring-and-telemetry.md), os padrões restantes desta série entram em mais detalhes sobre algumas estratégias que você pode usar para fazer isso:

- Tenha bom [monitoramento e telemetria](monitoring-and-telemetry.md), para que você descubra rapidamente as falhas que exigem intervenção, e você tem informações suficientes para resolvê-las.
- [Manipule falhas transitórias](transient-fault-handling.md) implementando a lógica de repetição inteligente, para que seu aplicativo seja recuperado automaticamente quando puder e retorne à lógica do [disjuntor](transient-fault-handling.md#circuitbreakers) quando não puder.
- Use o [Caching distribuído](distributed-caching.md) para minimizar problemas de taxa de transferência, latência e conexão com acesso ao banco de dados.
- Implemente o acoplamento flexível por meio do [padrão de trabalho centrado em fila](queue-centric-work-pattern.md), para que o front-end do aplicativo possa continuar funcionando quando o back-end estiver inativo.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os capítulos posteriores neste livro eletrônico e os recursos a seguir.

Documentação:

- [FailSafe: Diretrizes para arquiteturas de nuvem resilientes](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White Paper por Marc Mercuri, Ulrich Homann e Andrew TOWNHILL. Versão da página da Web da série de vídeos FailSafe.
- [Práticas recomendadas para o design de serviços em grande escala nos serviços de nuvem do Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White Paper por Mark Simms e Michael Thomassy.
- [Diretrizes técnicas de continuidade de negócios do Azure](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). White Paper de Patrick Wickline e Jason Roth.
- [Recuperação de desastre e alta disponibilidade para aplicativos do Azure](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). White Paper de Michael McKeown, Hanu Kommalapati e Jason Roth.
- [Práticas e padrões da Microsoft – diretrizes do Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte Diretrizes de implantação de vários data centers, padrão de disjuntor.
- [Suporte do Azure-contratos de nível de serviço](https://azure.microsoft.com/support/legal/sla/).
- [Continuidade de negócios no banco de dados SQL do Azure](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). Documentação sobre recursos de alta disponibilidade e recuperação de desastre do banco de dados SQL.
- [Alta disponibilidade e recuperação de desastre para SQL Server em máquinas virtuais do Azure](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Vídeos:

- [FailSafe: criando serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de nove partes de Ulrich Homann, Marc Mercuri e de Mark Simms. Apresenta conceitos de alto nível e princípios arquitetônicos de uma maneira muito acessível e interessante, com histórias extraídas da experiência CAT (equipe de consultoria para clientes) da Microsoft com os clientes reais. Os episódios 1 e 8 se aprofundam nos motivos para criar aplicativos de nuvem a fim de sobreviver a falhas. Veja também a discussão de acompanhamento de limitação no episódio 2 a partir de 49:57, a discussão sobre pontos de falha e modos de falha no episódio 2 a partir de 56:05, e a discussão de separadores de circuito no episódio 3 a partir de 40:55.
- [Criando Big: lições aprendidas de clientes do Azure-parte II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms fala sobre o design de falhas e instrumentação de tudo. Semelhante à série FailSafe, mas apresenta detalhes mais detalhados.

> [!div class="step-by-step"]
> [Anterior](unstructured-blob-storage.md)
> [Próximo](monitoring-and-telemetry.md)
