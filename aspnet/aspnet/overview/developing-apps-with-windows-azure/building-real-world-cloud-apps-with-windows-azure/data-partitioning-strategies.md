---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: Estratégias de particionamento de dados (criando aplicativos de nuvem do mundo real com o Azure) | Microsoft Docs
author: MikeWasson
description: A criação de aplicativos de nuvem do mundo real com o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: 2f79b1f459aff3e81dab7ea7eb4ebf3f71084463
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585813"
---
# <a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>Estratégias de particionamento de dados (criando aplicativos de nuvem do mundo real com o Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar o livro eletrônico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> A **criação de aplicativos de nuvem do mundo real com** o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem ajudá-lo a desenvolver com sucesso aplicativos Web para a nuvem. Para obter informações sobre a série, consulte [o primeiro capítulo](introduction.md).

Vimos anteriormente como é fácil dimensionar a camada da Web de um aplicativo de nuvem, adicionando e removendo servidores Web. Mas se todos estiverem atingindo o mesmo armazenamento de dados, o afunilamento do aplicativo passará do front-end para o back-end e a camada de dados será a mais difícil de dimensionar. Neste capítulo, veremos como você pode tornar sua camada de dados escalonável por meio do particionamento de dados em vários bancos de dados relacionais ou combinando o armazenamento de banco de dado relacional com outras opções de armazenamento de dados.

A configuração de um esquema de particionamento é melhor até o mesmo motivo mencionado anteriormente: é muito difícil alterar sua estratégia de armazenamento de dados depois que um aplicativo está em produção. Se você pensar na frente sobre abordagens diferentes, poderá evitar ter um "momento do Twitter" quando seu aplicativo falhar ou ficar inativo por muito tempo enquanto você reorganiza os dados do aplicativo e o código de acesso a dados.

## <a name="the-three-vs-of-data-storage"></a>Os três vs de armazenamento de dados

Para determinar se você precisa de uma estratégia de particionamento e o que deve ser, considere três perguntas sobre seus dados:

- Volume – quantos dados serão armazenados em última análise? Alguns gigabytes? Algumas centenas de gigabytes? Terabytes? Gerencia?
- Velocidade – qual é a taxa de crescimento dos seus dados? É um aplicativo interno que não está gerando muitos dados? Um aplicativo externo para o qual os clientes estarão carregando imagens e vídeos?
- Variedade – que tipo de dados será armazenado? Relacionais, imagens, pares chave-valor, grafos sociais?

Se você pensar que vai ter muita quantidade de volume, velocidade ou variedade, considere cuidadosamente qual tipo de esquema de particionamento permitirá que seu aplicativo seja dimensionado de forma eficiente e eficaz à medida que ele crescer, e para garantir que você não fique com nenhum afunilamento.

Há basicamente três abordagens para o particionamento:

- Particionamento vertical
- Particionamento horizontal
- Particionamento híbrido

## <a name="vertical-partitioning"></a>Particionamento vertical

A porção vertical é como dividir uma tabela por colunas: um conjunto de colunas entra em um armazenamento de dados e outro conjunto de colunas entra em um repositório de dados diferente.

Por exemplo, suponha que meu aplicativo armazene dados sobre pessoas, incluindo imagens:

![Tabela de dados](data-partitioning-strategies/_static/image1.png)

Quando você representa esses dados como uma tabela e examina as diferentes variedades de dados, você pode ver que as três colunas à esquerda têm dados de cadeia de caracteres que podem ser armazenados de forma eficiente por um banco de dado relacional, enquanto as duas colunas à direita são basicamente matrizes de bytes que c ome de arquivos de imagem. É possível armazenar dados de arquivos de imagem em um banco de dado relacional, e muitas pessoas fazem isso porque não querem salvar os dados no sistema de arquivos. Eles podem não ter um sistema de arquivos capaz de armazenar os volumes de dados necessários ou talvez não queiram gerenciar um sistema de backup e restauração separado. Essa abordagem funciona bem para bancos de dados locais e para pequenas quantidades de data em bancos de dado na nuvem. No ambiente local, pode ser mais fácil deixar o DBA cuidar de tudo.

Mas, em um banco de dados de nuvem, o armazenamento é relativamente caro e um alto volume de imagens pode aumentar o tamanho do banco de dados além dos limites em que ele pode operar com eficiência. Você pode resolver esses problemas Particionando os dados verticalmente, o que significa que você escolhe o armazenamento de dados mais apropriado para cada coluna na tabela de dados. O que pode funcionar melhor para este exemplo é colocar os dados de cadeia de caracteres em um banco de dado relacional e as imagens no armazenamento de BLOBs.

![Tabela de dados particionada verticalmente](data-partitioning-strategies/_static/image2.png)

Armazenar imagens no armazenamento de BLOBs em vez de em um banco de dados é mais prático na nuvem do que em um ambiente local porque você não precisa se preocupar com a configuração de servidores de arquivos ou o gerenciamento de backup e restauração de dados armazenados fora do banco de dado relacional: todos Isso é tratado para você automaticamente pelo serviço de armazenamento de BLOBs.

Essa é a abordagem de particionamento que implementamos no aplicativo Fix it e veremos o código para isso no [capítulo armazenamento de BLOBs](unstructured-blob-storage.md). Sem esse esquema de particionamento e presumindo um tamanho médio de imagem de 3 megabytes, o aplicativo corrigir ti só poderá armazenar cerca de 40.000 tarefas antes de atingir o tamanho máximo do banco de dados de 150 gigabytes. Depois de remover as imagens, o banco de dados pode armazenar 10 vezes mais tarefas; Você pode ir muito mais tempo antes de ter que pensar na implementação de um esquema de particionamento horizontal. E conforme o aplicativo é dimensionado, suas despesas crescem mais lentamente porque a maior parte das suas necessidades de armazenamento está entrando em um armazenamento de blob muito barato.

## <a name="horizontal-partitioning-sharding"></a>Particionamento horizontal (fragmentação)

A porção horizontal é como dividir uma tabela por linhas: um conjunto de linhas entra em um armazenamento de dados e outro conjunto de linhas entra em um repositório de dados diferente.

Considerando o mesmo conjunto de dados, outra opção seria armazenar diferentes intervalos de nomes de clientes em bancos de dado diferentes.

![Tabela de dados particionada horizontalmente](data-partitioning-strategies/_static/image3.png)

Você quer ter muito cuidado com o esquema de fragmentação para garantir que os dados sejam distribuídos uniformemente para evitar pontos de acesso. Este exemplo simples usando a primeira letra do sobrenome não atende a esse requisito, pois muitas pessoas têm nomes que começam com determinadas letras comuns. Você atingiu as limitações de tamanho de tabela anteriores ao esperado porque alguns bancos de dados ficarão muito grandes, embora a maioria permaneça pequena.

Um lado inverso do particionamento horizontal é que pode ser difícil fazer consultas em todos os dados. Neste exemplo, uma consulta teria que ser extraída de até 26 bancos de dados diferentes para obter todos aqueles armazenados pelo aplicativo.

## <a name="hybrid-partitioning"></a>Particionamento híbrido

Você pode combinar o particionamento vertical e horizontal. Por exemplo, nos dados de exemplo, você pode armazenar as imagens no armazenamento de BLOB e particionar horizontalmente os dados de cadeia de caracteres.

![Partição híbrida da tabela de dados](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Particionando um aplicativo de produção

Conceitualmente, é fácil ver como um esquema de particionamento funcionaria, mas qualquer esquema de particionamento aumenta a complexidade do código e apresenta muitas complicações novas com as quais você precisa lidar. Se você estiver movendo imagens para o armazenamento de BLOBs, o que fazer quando o serviço de armazenamento estiver inoperante? Como você lida com a segurança do blob? O que acontece se o banco de dados e o armazenamento de BLOBs ficam fora de sincronia? Se estiver fragmentando, como você manipulará a consulta em todos os bancos de dados?

As complicações podem ser gerenciadas desde que você esteja planejando para elas antes de ir para a produção. Muitas pessoas que não faziam isso precisavam mais tarde. Em média, nossa equipe de CAT (equipe de consultoria ao cliente) obtém chamadas telefônicas panicked de uma vez por mês de clientes cujos aplicativos estão sendo retirados de uma forma muito grande e não faziam esse planejamento. E dizem algo como: "Help! Coloco tudo em um único repositório de dados e, em 45 dias, vou ficar sem espaço! " E, se você tiver uma grande lógica de negócios incorporada a como você acessa seu armazenamento de dados e tiver clientes que estão usando seu aplicativo, não há um bom tempo para ficar inativo por um dia durante a migração. Nós acabamos passando por esforços hercúleas para ajudar o cliente a particionar seus dados em tempo real, sem um período de inatividade. É muito empolgante e muito assustador, e não algo que você queira envolver se você pode evitar! Pensar nisso antecipadamente e integrá-lo ao seu aplicativo tornará sua vida muito mais fácil se o aplicativo crescer mais tarde.

## <a name="summary"></a>Resumo

Um esquema de particionamento eficaz pode permitir que seu aplicativo de nuvem seja dimensionado para petabytes de dados na nuvem sem afunilamentos. E você não precisa pagar antecipadamente por máquinas maciças ou ampla infraestrutura como você pode se estivesse executando o aplicativo em um data center local. Na nuvem, você pode adicionar a capacidade incrementalmente conforme necessário, e você está pagando apenas pelo que usar ao usá-la.

No [próximo capítulo](unstructured-blob-storage.md) , veremos como o aplicativo corrigir ti implementa o particionamento vertical armazenando imagens no armazenamento de BLOBs.

## <a name="resources"></a>Recursos

Para obter mais informações sobre estratégias de particionamento, consulte os recursos a seguir.

Documentação:

- [Práticas recomendadas para o design de serviços em grande escala nos serviços de nuvem do Windows Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White Paper por Mark Simms e Michael Thomassy.
- [Padrões e práticas da Microsoft – padrões de design de nuvem](https://msdn.microsoft.com/library/dn568099.aspx). Consulte Diretrizes de particionamento de dados, padrão de fragmentação.

Vídeos:

- [FailSafe: criando serviços de nuvem escalonáveis e resilientes](https://channel9.msdn.com/Series/FailSafe). Série de nove partes de Ulrich Homann, Marc Mercuri e de Mark Simms. Apresenta conceitos de alto nível e princípios arquitetônicos de uma maneira muito acessível e interessante, com histórias extraídas da experiência CAT (equipe de consultoria para clientes) da Microsoft com os clientes reais. Consulte a discussão sobre particionamento no episódio 7.
- [Criando Big: lições aprendidas de clientes do Windows Azure – parte I](https://channel9.msdn.com/Events/Build/2012/3-029). Mark Simms aborda esquemas de particionamento, estratégias de fragmentação, como implementar a fragmentação e federações de banco de dados SQL, começando em 19:49. Semelhante à série FailSafe, mas apresenta detalhes mais detalhados.

Código de exemplo:

- [Conceitos básicos do serviço de nuvem no Windows Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Aplicativo de exemplo que inclui um banco de dados fragmentado. Para obter uma descrição do esquema de fragmentação implementado, consulte [Dal – fragmentação de RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) no blog do Windows Azure.

> [!div class="step-by-step"]
> [Anterior](data-storage-options.md)
> [Próximo](unstructured-blob-storage.md)
