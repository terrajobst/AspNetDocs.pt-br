---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
title: Opções de armazenamento de dados (criando aplicativos de nuvem do mundo real com o Azure) | Microsoft Docs
author: MikeWasson
description: A criação de aplicativos de nuvem do mundo real com o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: e51fcecb-cb33-4f9e-8428-6d2b3d0fe1bf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options
msc.type: authoredcontent
ms.openlocfilehash: 9357ed5aef39bed501cdac9ac26d46c884d4fae0
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457174"
---
# <a name="data-storage-options-building-real-world-cloud-apps-with-azure"></a>Opções de armazenamento de dados (criando aplicativos de nuvem do mundo real com o Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar o livro eletrônico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> A **criação de aplicativos de nuvem do mundo real com** o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem ajudá-lo a desenvolver com sucesso aplicativos Web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).

A maioria das pessoas é usada para bancos de dados relacionais e eles tendem a ignorar outras opções de armazenamento quando estão criando um aplicativo de nuvem. O resultado pode ser desempenho inferior, alta despesa ou pior, porque bancos de dados [NoSQL](http://en.wikipedia.org/wiki/NoSQL) (não relacionais) podem manipular algumas tarefas com mais eficiência do que bancos de dados relacionais. Quando os clientes nos pedem ajuda para resolver um problema de armazenamento de dados crítico, isso geralmente ocorre porque eles têm um banco de dados relacional em que uma das opções NoSQL teria funcionado melhor. Nessas situações, o cliente teria sido melhor se tivesse implementado a solução NoSQL antes de implantar o aplicativo na produção.

Por outro lado, também seria um erro para pressupor que um banco de dados NoSQL possa fazer tudo bem ou bem. Não há nenhuma melhor opção de gerenciamento de dados para todas as tarefas de armazenamento de dados; soluções de gerenciamento de dados diferentes são otimizadas para tarefas diferentes. A maioria dos aplicativos de nuvem do mundo real tem uma variedade de requisitos de armazenamento de dados e costuma ser servida melhor por uma combinação de várias soluções de armazenamento de dados.

A finalidade deste capítulo é oferecer uma noção mais ampla das opções de armazenamento de dados disponíveis para um aplicativo de nuvem e algumas diretrizes básicas sobre como escolher as que se ajustam ao seu cenário. É melhor estar ciente das opções disponíveis para você e pensar em seus pontos fortes e fracos antes de desenvolver um aplicativo. Alterar as opções de armazenamento de dados em um aplicativo de produção pode ser extremamente difícil, como ter que alterar um mecanismo do Jet enquanto o plano está em trânsito.

## <a name="data-storage-options-on-azure"></a>Opções de armazenamento de dados no Azure

A nuvem torna relativamente fácil usar uma variedade de armazenamentos de dados relacionais e NoSQL. Aqui estão algumas das plataformas de armazenamento de dados que você pode usar no Azure.

![](data-storage-options/_static/image1.png)

A tabela mostra quatro tipos de bancos de dados NoSQL:

- [Os bancos de dados de chave/valor](https://msdn.microsoft.com/library/dn313285.aspx#sec7) armazenam um único objeto serializado para cada valor de chave. Eles são bons para armazenar grandes volumes de dados, quando você deseja obter um item para determinado valor de chave e não precisa consultar com base em outras propriedades do item.

    O [armazenamento de BLOBs do Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/) é um banco de dados de chave/valor que funciona como o armazenamento de arquivos na nuvem, com valores de chave que correspondem aos nomes de pastas e arquivos. Você recupera um arquivo por sua pasta e nome de arquivo, não pesquisando valores no conteúdo do arquivo.

    O [armazenamento de tabelas do Azure](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/) também é um banco de dados de chave/valor. Cada valor é chamado de *entidade* (semelhante a uma linha, identificada por uma chave de partição e chave de linha) e contém várias *Propriedades* (semelhantes a colunas, mas nem todas as entidades em uma tabela precisam compartilhar as mesmas colunas). Consultar colunas que não sejam a chave é extremamente ineficiente e deve ser evitado. Por exemplo, você pode armazenar dados de perfil do usuário, com uma partição armazenando informações sobre um único usuário. Você pode armazenar dados como nome de usuário, hash de senha, data de nascimento e assim por diante, em propriedades separadas de uma entidade ou em entidades separadas na mesma partição. Mas você não iria querer consultar todos os usuários com um determinado intervalo de datas de nascimento e não pode executar uma consulta de junção entre sua tabela de perfil e outra tabela. O armazenamento de tabela é mais escalonável e mais barato do que um banco de dados relacional, mas não permite consultas ou junções complexas.
- [Documentdatabases](https://msdn.microsoft.com/library/dn313285.aspx#sec8) são bancos de dados de chave/valor nos quais os valores são *documentos*. "Documento" aqui não é usado no sentido de um documento do Word ou do Excel, mas significa uma coleção de campos e valores nomeados, qualquer um dos quais pode ser um documento filho. Por exemplo, em uma tabela de histórico de pedidos, um documento de pedido pode ter os campos número do pedido, data do pedido e cliente; e o campo cliente pode ter campos de nome e endereço. O Database codifica os dados de campo em um formato como XML, YAML, JSON ou BSON; ou pode usar texto sem formatação. Um recurso que define bancos de dados de documentos além dos bancos de dados de chave/valor é a capacidade de consultar em campos não-chave e definir índices secundários para tornar a consulta mais eficiente. Essa capacidade torna um banco de dados de documentos mais adequado para os aplicativos que precisam recuperar informações com base em critérios mais complexos do que o valor da chave do documento. Por exemplo, em um banco de dados de documentos de histórico de pedidos de vendas, você pode consultar vários campos, como ID do produto, ID do cliente, nome do cliente e assim por diante. O [MongoDB](http://www.mongodb.org/) é um banco de dados de documentos popular.
- [Os bancos de dados de família de coluna](https://msdn.microsoft.com/library/dn313285.aspx#sec9) são armazenamentos de data de chave/valor que permitem estruturar o armazenamento de dados em coleções de colunas relacionadas chamadas de famílias de colunas. Por exemplo, um banco de dados censo pode ter um grupo de colunas para o nome de uma pessoa (primeiro, meio, último), um grupo para o endereço da pessoa e um grupo para as informações de perfil da pessoa (DOB, gênero, etc.). O banco de dados pode armazenar cada família de colunas em uma partição separada, mantendo todos os dados de uma pessoa relacionada à mesma chave. Você pode ler todas as informações de perfil sem precisar ler todas as informações de nome e endereço também. [Cassandra](http://cassandra.apache.org/) é um banco de dados popular da família de colunas.
- [Bancos](https://msdn.microsoft.com/library/dn313285.aspx#sec10) de dados de grafo armazenam informações como uma coleção de objetos e relações. A finalidade de um banco de dados de grafo é permitir que um aplicativo execute com eficiência consultas que atravessam a rede de objetos e as relações entre eles. Por exemplo, os objetos podem ser funcionários em um banco de dados de recursos humanos e talvez você deseje facilitar consultas como "encontrar todos os funcionários que trabalham direta ou indiretamente para Scott". [Neo4j](http://www.neo4j.org/) é um banco de dados de grafo popular.

Em comparação com bancos de dados relacionais, as opções de NoSQL oferecem escalabilidade muito maior e economia de custo para armazenamento e análise de dados não estruturados. A desvantagem é que eles não fornecem a capacidade de consulta avançada e recursos de integridade de dados robustos de bancos de dados relacionais. O NoSQL funcionaria bem para dados de log do IIS, o que envolve alto volume, sem necessidade de consultas de junção. O NoSQL não funcionaria tão bem para transações bancárias, o que requer integridade de dados absoluta e envolve muitas relações com outros dados relacionados à conta.

Há também uma categoria mais recente da plataforma de banco de dados chamada [NewSQL](http://en.wikipedia.org/wiki/NewSQL) que combina a escalabilidade de um banco de dados NoSQL com a capacidade de consulta e a integridade transacional de um banco de dados relacional. Os bancos de dados NewSQL são projetados para armazenamento distribuído e processamento de consultas, que muitas vezes é difícil de implementar em bancos de dados "OldSQL". [NuoDB](http://www.nuodb.com/) é um exemplo de um banco de dados NewSQL que pode ser usado no Azure.

<a id="hadoop"></a>
## <a name="hadoop-and-mapreduce"></a>Hadoop e MapReduce

Os grandes volumes de dados que você pode armazenar em bancos de dados NoSQL podem ser difíceis de analisar com eficiência oportuna. Para fazer isso, você pode usar uma estrutura como o [Hadoop](http://hadoop.apache.org/) , que implementa a funcionalidade do [MapReduce](http://en.wikipedia.org/wiki/MapReduce) . Basicamente, o que um processo MapReduce faz é o seguinte:

- Limite o tamanho dos dados que precisam ser processados selecionando o armazenamento de dados somente os dados que você realmente precisa analisar. Por exemplo, você deseja saber a composição de sua base de usuários por ano de nascimento, para que você selecione apenas os anos de nascimento do seu armazenamento de dados de perfil de usuário.
- Divida os dados em partes e envie-os para diferentes computadores para processamento. O computador A calcula o número de pessoas com 1950-1959 datas, o computador B faz a 1960-1969, etc. Esse grupo de computadores é chamado de *cluster Hadoop*.
- Coloque os resultados de cada parte novamente depois que o processamento nas partes for concluído. Agora você tem uma lista relativamente curta de quantas pessoas para cada ano de nascimento e a tarefa de calcular porcentagens nessa lista geral é gerenciável.

No Azure, o [HDInsight](https://azure.microsoft.com/services/hdinsight/) permite processar, analisar e obter novas informações de Big data usando o poder do Hadoop. Por exemplo, você pode usá-lo para analisar logs do servidor Web:

- Habilite o log do servidor Web para sua conta de armazenamento. Isso configura o Azure para gravar logs no serviço blob para cada solicitação HTTP para seu aplicativo. O serviço blob é basicamente o armazenamento de arquivos na nuvem e se integra perfeitamente com o HDInsight.

    ![Logs no armazenamento de BLOBs](data-storage-options/_static/image2.png)
- À medida que o aplicativo obtém o tráfego, os logs do IIS do servidor Web são gravados no armazenamento de BLOBs.

    ![Logs do Web Server](data-storage-options/_static/image3.png)
- No portal, clique em **novo** - **serviços de dados** - **HDInsight** - **criação rápida**e especifique um nome de cluster hdinsight, o tamanho do cluster (número de nós de dados do cluster hdinsight) e um nome de usuário e senha para o cluster HDInsight.

    ![HDInsight](data-storage-options/_static/image4.png)

Agora você pode configurar trabalhos MapReduce para analisar seus logs e obter respostas para perguntas como:

- Em que horas do dia o meu aplicativo obtém o maior ou menor tráfego?
- De quais países meu tráfego está vindo?
- Qual é a média de renda de vizinhança das áreas das quais meu tráfego vem. (Há um conjunto de registros público que fornece a você a receita de vizinhança por endereço IP e você pode fazer a correspondência com o endereço IP nos logs do servidor Web.)
- Como a renda de vizinhança se correlaciona a páginas ou produtos específicos no site?

Em seguida, você pode usar as respostas para perguntas como essas para direcionar anúncios com base na probabilidade de um cliente estar interessado ou provavelmente comprar um produto específico.

Conforme explicado no [capítulo automatizar tudo](automate-everything.md), a maioria das funções que você pode fazer no portal pode ser automatizada e isso inclui a configuração e a execução de trabalhos de análise do HDInsight. Um típico script do HDInsight pode conter as seguintes etapas:

- Provisione um cluster HDInsight e vincule-o à sua conta de armazenamento para entrada de armazenamento de BLOBs.
- Carregue os executáveis de trabalho MapReduce (arquivos. jar ou. exe) no cluster HDInsight.
- Envie um MapReduce que armazene os dados de saída no armazenamento de BLOBs.
- Aguarde a conclusão do trabalho.
- Excluir o cluster do HDInsight.
- Acesse a saída do armazenamento de BLOBs.

Ao executar um script que faz tudo isso, você minimiza a quantidade de tempo que o cluster HDInsight é provisionado, o que minimiza os custos.

<a id="paasiaas"></a>
## <a name="platform-as-a-service-paas-versus-infrastructure-as-a-service-iaas"></a>PaaS (plataforma como serviço) versus IaaS (infraestrutura como serviço)

As opções de armazenamento de dados listadas anteriormente incluem PaaS (plataforma como serviço) e soluções IaaS (infraestrutura como serviço). PaaS significa que gerenciamos a infraestrutura de hardware e software e você simplesmente usa o serviço. O banco de dados SQL é um recurso de PaaS do Azure. Você solicita bancos de dados e, nos bastidores, o Azure configura e configura as VMs e configura os bancos de dados neles. Você não tem acesso direto às VMs e não precisa gerenciá-las. IaaS significa que você configura, configura e gerencia VMs que são executadas em nossa infraestrutura de data center e você coloca tudo o que desejar neles. Fornecemos uma galeria de imagens de VM pré-configuradas para configurações de VM comuns. Por exemplo, você pode instalar imagens de VM pré-configuradas para o Windows Server 2008, Windows Server 2012, BizTalk Server, Oracle WebLogic Server, Oracle Database, etc.

As soluções de dados de PaaS que o Azure oferece incluem:

- Banco de dados SQL do Azure (anteriormente conhecido como SQL Azure). Um banco de dados relacional de nuvem baseado em SQL Server.
- Armazenamento de Tabelas do Azure. Um banco de dados NoSQL de chave/valor.
- Armazenamento de Blobs do Azure. Armazenamento de arquivos na nuvem.

Para IaaS, você pode executar qualquer coisa que possa carregar em uma VM, por exemplo:

- Bancos de dados relacionais como SQL Server, Oracle, MySQL, SQL Compact, SQLite ou Postgres.
- Armazenamentos de dados de chave/valor, como memcached, Redis, Cassandra e Riak.
- Armazenamentos de dados de coluna, como HBase.
- Bancos de dados de documentos como MongoDB, RavenDB e CouchDB.
- Bancos de dados de grafo, como neo4j.

![Opções de armazenamento de dados no Azure](data-storage-options/_static/image5.png)

A opção IaaS fornece opções de armazenamento de dados quase ilimitadas e muitas delas são especialmente fáceis de usar, pois você pode criar VMs usando imagens pré-configuradas. Por exemplo, no portal de gerenciamento, vá para **máquinas virtuais**, clique na guia **imagens** e clique em **procurar VM Depot**.

![Procurar VM Depot](data-storage-options/_static/image6.png)

Em seguida, você vê uma lista de [centenas de imagens de VM pré-configuradas](http://www.hanselman.com/blog/Over400VirtualMachineImagesOfOpenSourceSoftwareStacksInTheVMDepotAzureGallery.aspx)e pode criar uma VM com base em uma imagem que tem um sistema de gerenciamento de banco de dados pré-instalado, como MongoDB, Neo4J, Redis, Cassandra ou CouchDB:

![MongoDB no depósito de VM](data-storage-options/_static/image7.png)

O Azure torna as opções de armazenamento de dados IaaS tão fáceis de usar quanto possível, mas as ofertas de PaaS têm muitas vantagens para torná-las mais econômicas e práticas para muitos cenários:

- Você não precisa criar VMs, basta usar o portal ou um script para configurar um armazenamento de dados. Se você quiser um armazenamento de dados de 200 terabyte, basta clicar em um botão ou executar um comando e, em segundos, ele estará pronto para uso.
- Você não precisa gerenciar ou aplicar patch nas VMs usadas pelo serviço; A Microsoft faz isso para você automaticamente.-você não precisa se preocupar em configurar a infraestrutura para dimensionamento ou alta disponibilidade; A Microsoft cuida de tudo isso para você.
- Você não precisa comprar licenças; as taxas de licença são incluídas nas tarifas de serviço.
- Você paga apenas pelo que usa.

As opções de armazenamento de dados de PaaS no Azure incluem ofertas de provedores de terceiros. Por exemplo, você pode escolher o [complemento MongoLab](https://azure.microsoft.com/documentation/articles/store-mongolab-web-sites-dotnet-store-data-mongodb/) da Azure Store para provisionar um banco de dados do MongoDB como um serviço.

## <a name="choosing-a-data-storage-option"></a>Escolhendo uma opção de armazenamento de dados

Não há uma abordagem adequada para todos os cenários. Se alguém disser que essa tecnologia é a resposta, a primeira coisa a ser feita é "o que é a pergunta?", porque diferentes soluções são otimizadas para coisas diferentes. Há vantagens claras para o modelo relacional; é por isso que ela já existe há muito tempo. Mas também há lados para o SQL que podem ser abordados com uma solução NoSQL.

Em geral, o que vemos de melhor trabalho é uma abordagem composicional, em que você usa SQL e NoSQL em uma única solução. Mesmo quando as pessoas dizem que estão adotando o NoSQL, se você analisar o que eles estão fazendo com frequência você acha que eles estão usando várias estruturas NoSQL diferentes: estão usando [CouchDB](http://wiki.apache.org/couchdb/Introduction), [Redis](http://redis.io/)e [Riak](http://basho.com/riak/) para coisas diferentes. Mesmo o Facebook, que usa o NoSQL extensivamente, usa estruturas NoSQL diferentes para diferentes partes do serviço. A flexibilidade para misturar e combinar abordagens de armazenamento de dados é uma das coisas que é boa sobre a nuvem, pois é fácil usar várias soluções de dados e integrá-las a um único aplicativo.

Aqui estão algumas perguntas a considerar quando você está escolhendo uma abordagem:

| Semântica de dados | -O que é o armazenamento de dados principal e a semântica de acesso a dados (você armazena dados relacionais ou não estruturados)? Dados não estruturados, como arquivos de mídia, se adaptam melhor ao armazenamento de BLOBs; uma coleção de dados relacionados, como produtos, inventários, fornecedores, pedidos de clientes, etc., se adapta melhor em um banco de dados relacional. |
| --- | --- |
| Suporte de consulta | -Como é fácil consultar os dados? -Que tipos de perguntas podem ser feitas com eficiência? Os armazenamentos de dados de chave/valor são muito bons para obter uma única linha, dado um valor de chave, mas não tão bom para consultas complexas. Para um armazenamento de dados de perfil de usuário em que você sempre Obtém os dados para um usuário específico, um repositório de dados de chave/valor poderia funcionar bem; para um catálogo de produtos em que você deseja obter agrupamentos diferentes com base em vários atributos de produto, um banco de dados relacional pode funcionar melhor. Os bancos de dados NoSQL podem armazenar grandes volumes de data com eficiência, mas você precisa estruturar o banco de dados em volta de como o aplicativo consulta os dados e isso torna as consultas ad hoc mais difíceis de fazer. Com um banco de dados relacional, você pode criar praticamente qualquer tipo de consulta. |
| Projeção funcional | -As perguntas, agregações etc., podem ser executadas no lado do servidor? Se eu executar SELECT COUNT (\*) de uma tabela no SQL, isso fará com que todo o trabalho seja efetivamente executado no servidor e retornará o número que estou procurando. Se eu quiser o mesmo cálculo de um armazenamento de dados NoSQL que não dá suporte à agregação, essa é uma "consulta não associada" ineficiente e provavelmente atingirá o tempo limite. Mesmo que a consulta tenha sucesso, eu precise recuperar todos os dados do servidor para o cliente e contar as linhas no cliente. -Quais idiomas ou tipos de expressões podem ser usados? Com um banco de dados relacional, posso usar SQL. Com alguns bancos de dados NoSQL, como o armazenamento de tabelas do Azure, usarei [OData](http://www.odata.org/), e tudo o que posso fazer é filtrar em chave primária e obter projeções (selecione um subconjunto dos campos disponíveis). |
| Facilidade de escalabilidade | -Com que frequência e quanto os dados precisarão ser dimensionados? -A plataforma implementa nativamente a expansão? -Como é fácil adicionar/remover capacidade (tamanho e taxa de transferência)? Os bancos de dados relacionais e as tabelas não são particionados automaticamente para torná-los escalonáveis, para que sejam difíceis de dimensionar além de determinadas limitações. Armazenamentos de dados NoSQL como o armazenamento de tabelas do Azure particionam de forma inerente tudo e não há quase nenhum limite para adicionar partições. Você pode dimensionar o armazenamento de tabelas prontamente até 200 terabytes, mas o tamanho máximo do banco de dados do banco de dados SQL do Azure é de 500 gigabytes. Você pode dimensionar os dados relacionais Particionando-os em vários bancos, mas configurar um aplicativo para dar suporte a esse modelo envolve muito trabalho de programação. |
| Instrumentação e capacidade de gerenciamento | -Quão fácil é a plataforma para instrumentar, monitorar e gerenciar? Você precisará manter-se informado sobre a integridade e o desempenho do seu armazenamento de dados, portanto, você precisa saber antecipadamente quais métricas uma plataforma oferece gratuitamente e o que você precisa desenvolver por conta própria. |
| Operações | -Quão fácil é a plataforma para implantar e executar no Azure? PaaS? IaaS? Linux? O armazenamento de tabela e o banco de dados SQL são fáceis de configurar no Azure. As plataformas que não são soluções de PaaS do Azure internas exigem mais esforço. |
| Suporte a API | -É uma API disponível que torna mais fácil trabalhar com a plataforma? Para o serviço tabela do Azure, há um SDK com uma API .NET que dá suporte ao modelo de programação assíncrona do .NET 4,5. Se você estiver escrevendo um aplicativo .NET, será muito mais fácil escrever e testar o código para o serviço tabela do Azure em comparação com outra plataforma de armazenamento de dados de coluna de chave/valor que não tem nenhuma API ou uma menos abrangente. |
| Integridade transacional e consistência de dados | -É essencial que a plataforma suporte as transações para garantir a consistência dos dados? Para acompanhar os emails em massa enviados, o desempenho e o custo de armazenamento de dados baixo podem ser mais importantes do que o suporte automático para transações ou integridade referencial na plataforma de dados, tornando o serviço tabela do Azure uma boa escolha. Para rastrear saldos de conta bancária ou ordens de compra, uma plataforma de banco de dados relacional que fornece garantias transacionais fortes seria uma opção melhor. |
| Continuidade de negócios | -Quão fácil é o backup, a restauração e a recuperação de desastre? Os dados de produção mais cedo ou posteriores serão corrompidos e você precisará de uma função Undo. Os bancos de dados relacionais geralmente têm recursos de restauração mais refinados, como a capacidade de restaurar para um ponto no tempo. Entender quais recursos de restauração estão disponíveis em cada plataforma que você está considerando é um fator importante a ser considerado. |
| Custo | -Se mais de uma plataforma puder dar suporte à carga de trabalho de dados, como elas serão comparadas com custo? Por exemplo, se você usar ASP.NET Identity, poderá armazenar os dados de perfil do usuário no serviço tabela do Azure ou no banco de dados SQL do Azure. Se você não precisar das facilidades de consulta avançada do banco de dados SQL, poderá escolher as tabelas do Azure em parte porque ele custa muito menos para uma determinada quantidade de armazenamento. |

O que geralmente recomendamos é saber a resposta às perguntas em cada uma dessas categorias antes de escolher suas soluções de armazenamento de dados.

Além disso, sua carga de trabalho pode ter requisitos específicos para que algumas plataformas ofereçam suporte melhor do que outras. Por exemplo:

- Seu aplicativo requer recursos de auditoria?
- Quais são seus requisitos de longevidade de dados – você precisa de recursos de arquivamento ou limpeza automatizados?
- Você tem necessidades de segurança especializadas? Por exemplo, os dados incluem PII (informações de identificação pessoal), mas você precisa ser capaz de garantir que PII seja excluído dos resultados da consulta.
- Se você tiver alguns dados que não podem ser armazenados na nuvem por motivos regulatórios ou tecnológicos, talvez precise de uma plataforma de armazenamento de dados em nuvem que facilite a integração com seu armazenamento local.

## <a name="demo--using-sql-database-in-azure"></a>Demonstração – usando o banco de dados SQL no Azure

O aplicativo corrigir ti usa um banco de dados relacional para armazenar tarefas. O script de criação de ambiente do Windows PowerShell mostrado no [capítulo automatizar tudo](automate-everything.md) cria duas instâncias do banco de dados SQL. Você pode vê-los no portal clicando na guia **bancos de dados SQL** .

![Bancos de dados SQL no portal](data-storage-options/_static/image8.png)

Também é fácil criar bancos de dados usando o Portal.

Clique em **New--Data Services** -- **SQL Database** -- **criação rápida**, insira um nome de banco de dados, escolha um servidor que você já tem em sua conta ou crie um novo e clique em **criar banco de dados SQL**.

![Novo banco de dados SQL](data-storage-options/_static/image9.png)

Aguarde alguns segundos e você tem um banco de dados no Azure pronto para uso.

![Novo banco de dados SQL criado](data-storage-options/_static/image10.png)

Portanto, o Azure faz em alguns segundos o que pode levar um dia ou uma semana ou mais para ser realizado no ambiente local. E, como você pode facilmente criar bancos de dados automaticamente em um script ou usando uma API de gerenciamento, você pode escalar horizontalmente de forma dinâmica, distribuindo os mesmos em vários bancos de dado, desde que seu aplicativo tenha sido programado para isso.

Este é um exemplo de nosso modelo de plataforma como serviço. Você não precisa gerenciar os servidores, nós o fazemos. Você não precisa se preocupar com os backups, fazemos isso. Ele está sendo executado em alta disponibilidade – os dados no banco são replicados em três servidores automaticamente. Se um computador se tornar inativo, o failover automático e os dados não são perdidos. O servidor é corrigido regularmente, você não precisa se preocupar com isso.

Clique em um botão e você obtém a cadeia de conexão exata necessária e pode começar imediatamente usando o novo banco de dados.

![Cadeias de conexão](data-storage-options/_static/image11.png)

O painel mostra o histórico de conexão e a quantidade de armazenamento usado.

![Painel do banco de dados SQL](data-storage-options/_static/image12.png)

Você pode gerenciar bancos de dados no portal ou usando as ferramentas de SQL Server com as quais você já está familiarizado, incluindo SQL Server Management Studio (SSMS) e o SSOX (Visual Studio Tools Pesquisador de Objetos do SQL Server) e Gerenciador de Servidores.

![SSOX](data-storage-options/_static/image13.png)

Outra coisa interessante é o modelo de preços. Você pode iniciar o desenvolvimento com um banco de dados de 20 MB gratuito e um banco de dados de produção começa em cerca de $5 por mês. Você paga apenas pela quantidade de dados que realmente armazena no banco de dado, e não pela capacidade máxima. Você não precisa comprar uma licença.

O banco de dados SQL é fácil de dimensionar. Para o aplicativo corrigir ti, o banco de dados que criamos em nosso script de automação é limitado a 1 GB. Se você quiser dimensioná-lo para 150 Gig, bastará entrar no portal e alterar essa configuração, ou executar um comando de API REST e, em segundos, você terá um banco de dados de 150 Gig no qual você pode implantar os dados.

![Edições e tamanhos do banco de dados SQL](data-storage-options/_static/image14.png)

Esse é o poder da nuvem de acompanhar a infraestrutura de forma rápida e fácil e começar a usá-la imediatamente.

O aplicativo corrigir ti usa dois bancos de dados SQL, um para a associação (autenticação e autorização) e outro para o dado, e isso é tudo o que você precisa fazer para provisioná-lo e dimensioná-lo. Você viu anteriormente como provisionar os bancos de dados por meio de scripts do Windows PowerShell e agora também viu como é fácil fazer isso no Portal.

## <a name="entity-framework-versus-direct-database-access-using-adonet"></a>Entity Framework versus acesso direto ao banco de dados usando ADO.NET

O aplicativo corrigir ti acessa esses bancos de dados usando o Entity Framework, o ORM recomendado pela Microsoft (mapeador relacional de objeto) para aplicativos .NET. Um ORM é uma excelente ferramenta que facilita a produtividade do desenvolvedor, mas a produtividade acompanha a despesa de degradação do desempenho em alguns cenários. Em um aplicativo de nuvem do mundo real, você não fará uma escolha entre usar o EF ou usar o ADO.NET diretamente – você usará ambos. Na maioria das vezes em que você está escrevendo código que funciona com o banco de dados, obter o desempenho máximo não é essencial e você pode aproveitar a codificação e os testes simplificados que você obtém com o Entity Framework. Em situações em que a sobrecarga do EF causaria um desempenho inaceitável, você pode escrever e executar suas próprias consultas usando ADO.NET, idealmente chamando procedimentos armazenados.

Qualquer que seja o método usado para acessar o banco de dados, você deseja minimizar a "informação" tanto quanto possível. Em outras palavras, se você puder obter todos os dados necessários em um conjunto de resultados de consulta maior em vez de dezenas ou centenas de menores, isso geralmente é preferível. Por exemplo, se você precisar listar os alunos e os cursos nos quais eles estão registrados, geralmente é melhor obter todos os dados em uma consulta de junção, em vez de colocar os alunos em uma consulta e executar consultas separadas para os cursos de cada aluno.

## <a name="sql-databases-and-the-entity-framework-in-the-fix-it-app"></a>Bancos de dados SQL e o Entity Framework no aplicativo corrigir ti

No aplicativo Fix it, a classe `FixItContext`, que deriva da classe Entity Framework `DbContext`, identifica o banco de dados e especifica as tabelas no banco de dados. O contexto especifica um conjunto de entidades (tabela) para tarefas e o código passa para o contexto o nome da cadeia de conexão. Esse nome se refere a uma cadeia de conexão que é definida no arquivo Web. config.

[!code-csharp[Main](data-storage-options/samples/sample1.cs?highlight=4,8)]

A cadeia de conexão no arquivo *Web. config* é denominada appdb (aqui apontando para o banco de dados de desenvolvimento local):

[!code-xml[Main](data-storage-options/samples/sample2.xml?highlight=3)]

O Entity Framework cria uma tabela *FixItTasks* com base nas propriedades incluídas na classe de entidade `FixItTask`. Essa é uma classe simples POCO (objeto comum CLR), o que significa que ela não herda de ou tem nenhuma dependência no Entity Framework. Mas Entity Framework sabe como criar uma tabela com base nela e executar operações CRUD (criar-ler-atualizar-excluir) com ela.

[!code-csharp[Main](data-storage-options/samples/sample3.cs)]

![Tabela FixItTasks](data-storage-options/_static/image15.png)

O aplicativo corrigir ti inclui uma interface de repositório que ele usa para operações CRUD trabalhando com o armazenamento de dados.

[!code-csharp[Main](data-storage-options/samples/sample4.cs)]

Observe que os métodos de repositório são todos Async, portanto, todo o acesso a dados pode ser feito de maneira completamente assíncrona.

A implementação do repositório chama Entity Framework métodos assíncronos para trabalhar com os dados, incluindo consultas LINQ, bem como para operações de inserção, atualização e exclusão. Aqui está um exemplo do código para pesquisar uma tarefa corrigir ti.

[!code-csharp[Main](data-storage-options/samples/sample5.cs)]

Você observará que também há algum código de registro de tempo e de erro aqui, veremos isso posteriormente no [capítulo monitoramento e telemetria](monitoring-and-telemetry.md).

<a id="sqliaas"></a>
## <a name="choosing-sql-database-paas-versus-sql-server-in-a-vm-iaas-in-azure"></a>Escolhendo o banco de dados SQL (PaaS) versus SQL Server em uma VM (IaaS) no Azure

Uma boa coisa sobre o SQL Server e o banco de dados SQL do Azure é que o modelo de programação principal para ambos é idêntico. Você pode usar a maioria das mesmas habilidades em ambos os ambientes. Você pode até mesmo usar um banco de dados SQL Server em desenvolvimento e uma instância do banco de dados SQL na nuvem, que é como o aplicativo corrigir a ti está configurado.

Como alternativa, você pode executar o mesmo SQL Server na nuvem que você executa localmente, instalando-o em VMs IaaS. Para alguns aplicativos herdados, a execução de SQL Server em uma VM pode ser uma solução melhor. Como um banco de dados SQL Server é executado em uma VM dedicada, ele tem mais recursos disponíveis para ele do que um banco de dados do banco de dados SQL que é executado em um servidor compartilhado. Isso significa que um banco de dados SQL Server pode ser maior e ainda ter um bom desempenho. Em geral, quanto menor o tamanho do banco de dados e o tamanho da tabela, melhor o caso de uso funciona para o banco de dados SQL (PaaS).

Aqui estão algumas diretrizes sobre como escolher entre os dois modelos.

| Banco de Dados SQL do Azure (PaaS) | SQL Server em uma IaaS (máquina virtual) |
| --- | --- |
| **Prós** – você não precisa criar ou gerenciar VMS, atualizar ou aplicar patch para o sistema operacional ou SQL; O Azure faz isso para você. -Alta disponibilidade interna, com um SLA de nível de banco de dados. -TCO (custo total de propriedade) baixo porque você paga apenas pelo que usar (nenhuma licença é necessária). -Bom para lidar com grandes números de bancos de dados menores (&lt;= 500 GB cada). -Fácil criar novos bancos de dados dinamicamente para habilitar a expansão. | ***Prós*** – recursos compatíveis com SQL Server locais. -Pode implementar SQL Server [alta disponibilidade por meio do AlwaysOn](https://www.microsoft.com/sqlserver/solutions-technologies/mission-critical-operations/high-availability.aspx) em duas VMS, com SLA de nível de VM. -Você tem controle total sobre como o SQL é gerenciado. -Pode reutilizar as licenças do SQL que você já possui ou pagar por hora para uma. -Bom para lidar com menos de (1 TB +) bancos de dados maiores. |
| **Contras** -algumas lacunas de recursos em comparação com SQL Server locais (falta de [integração CLR](https://technet.microsoft.com/library/ms131102.aspx), [TDE](https://technet.microsoft.com/library/bb934049.aspx), [suporte à compactação](https://technet.microsoft.com/library/cc280449.aspx), [SQL Server Reporting Services](https://technet.microsoft.com/library/ms159106.aspx), etc.)-limite de tamanho do banco de dados de 500GB. | ***Contras*** -atualizações/patches (so e SQL) são sua criação de responsabilidade e o gerenciamento de bancos de dados são a sua responsabilidade-IOPS de disco (operações de entrada/saída por segundo) limitadas a cerca de 8000 (por meio de 16 unidades de dados). |

Se você quiser usar SQL Server em uma VM, poderá usar sua própria licença de SQL Server, ou você pode pagar uma por hora. Por exemplo, no portal ou por meio da API REST, você pode criar uma nova VM usando uma imagem de SQL Server.

![Criar VM com SQL Server](data-storage-options/_static/image16.png)

![Lista de imagens de VM SQL Server](data-storage-options/_static/image17.png)

Quando você cria uma VM com uma imagem SQL Server, o custo de licença de SQL Server é cobrado por hora com base no uso da VM. Se você tiver um projeto que só será executado por alguns meses, será mais barato pagar por hora. Se você considerar que seu projeto vai durar anos, será mais barato comprar a licença da maneira que você normalmente faz.

## <a name="summary"></a>Resumo

A computação em nuvem torna prática a combinação e a correspondência das abordagens de armazenamento de dados para atender melhor às necessidades do seu aplicativo. Se você estiver criando um novo aplicativo, pense atentamente sobre as perguntas listadas aqui para escolher abordagens que continuarão funcionando bem quando o aplicativo aumentar. O [próximo capítulo](data-partitioning-strategies.md) explicará algumas estratégias de particionamento que você pode usar para combinar várias abordagens de armazenamento de dados.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte os recursos a seguir.

Escolhendo uma plataforma de banco de dados:

- [Acesso a dados para soluções altamente escalonáveis: usando a persistência SQL, NoSQL e poliglota](https://aka.ms/dag-doc). Livro eletrônico por padrões e práticas da Microsoft que se aprofundam em diferentes tipos de armazenamentos de dados disponíveis para aplicativos em nuvem.
- [Práticas e padrões da Microsoft – diretrizes do Azure](https://msdn.microsoft.com/library/ff898430.aspx). Consulte o guia de consistência de dados, a orientação de sincronização e a replicação de dados, o padrão de tabela de índice, o padrão de exibição materializada.
- [Base: uma alternativa Acid](http://queue.acm.org/detail.cfm?id=1394128). Artigo sobre compensações entre consistência de dados e escalabilidade.
- [Sete bancos de dados em sete semanas: um guia para bancos de dados modernos e o movimento do NoSQL](https://www.amazon.com/Seven-Databases-Weeks-Modern-Movement/dp/1934356921). Livro de Eric Redmond e Jim R. Wilson. Altamente recomendado para apresentar a gama de plataformas de armazenamento de dados disponíveis atualmente.

Escolhendo entre SQL Server e o banco de dados SQL:

- [Visualização Premium para orientação do banco de dados SQL](https://msdn.microsoft.com/library/windowsazure/dn369873.aspx). Uma introdução a Banco de Dados SQL Premium e diretrizes sobre quando escolher o banco de dados SQL Web e Business Editions.
- [Diretrizes e limitações (banco de dados SQL do Azure)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx). Página do portal que vincula à documentação sobre limitações do banco de dados SQL, incluindo uma que se concentra em SQL Server recursos para os quais o banco de dados SQL não dá suporte.
- [SQL Server em máquinas virtuais do Azure](https://msdn.microsoft.com/library/windowsazure/jj823132.aspx). Página do portal que vincula a documentação sobre a execução de SQL Server no Azure.
- [Scott Guthrie explica os bancos de dados SQL no Azure](https://azure.microsoft.com/documentation/videos/sql-in-azure-scottgu/). vídeo de 6 minutos introdução ao banco de dados SQL de Scott Guthrie.
- [Padrões de aplicativo e estratégias de desenvolvimento para SQL Server em máquinas virtuais do Azure](https://msdn.microsoft.com/library/windowsazure/dn574746.aspx).

Usando o Entity Framework e o banco de dados SQL em um aplicativo Web ASP.NET

- [Introdução com o EF 6 usando o MVC 5](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Série de tutoriais de nove partes que orienta você pela criação de um aplicativo MVC que usa o EF e implanta o banco de dados no Azure e no banco de dados SQL.
- [Implantação da Web do ASP.NET usando o Visual Studio](../../../../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). Série de tutoriais de doze partes que se aprofunda em mais detalhes sobre como implantar um banco de dados usando Code First do EF.
- [Implante um aplicativo Secure ASP.NET MVC 5 com associação, OAuth e banco de dados SQL em um site do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/). O tutorial passo a passo que orienta você durante a criação de um aplicativo Web que usa a autenticação do, armazena tabelas de aplicativos no banco de dados de associação, modifica o esquema de banco de dados e implanta o aplicativo no Azure.
- [Mapa de conteúdo de acesso a dados do ASP.net](https://go.microsoft.com/fwlink/p/?LinkId=282414). Links para recursos para trabalhar com o EF e o banco de dados SQL.

Usando o MongoDB no Azure:

- [MongoLab – MongoDB no Azure](http://msopentech.com/opentech-projects/mongolab-mongodb-on-windows-azure/). Página do portal para obter a documentação sobre como executar o MongoDB no Azure.
- [Crie um site do Azure que se conecta ao MongoDB em execução em uma máquina virtual no Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-store-data-mongodb-vm/). Tutorial passo a passo que mostra como usar um banco de dados MongoDB em um aplicativo Web ASP.NET.

HDInsight (Hadoop no Azure):

- [HDInsight](https://azure.microsoft.com/documentation/services/hdinsight/). Portal para a documentação do HDInsight no site [do Azure](https://azure.microsoft.com/) .
- [Hadoop e HDInsight: Big data no Azure](https://msdn.microsoft.com/magazine/dn385705.aspx). Artigo da MSDN Magazine de Bruno Terkaly e Ricardo Villalobos, apresentando o Hadoop no Azure.
- [Práticas e padrões da Microsoft – diretrizes do Azure](https://msdn.microsoft.com/library/dn568099.aspx). Consulte padrão MapReduce.

> [!div class="step-by-step"]
> [Anterior](single-sign-on.md)
> [Próximo](data-partitioning-strategies.md)
