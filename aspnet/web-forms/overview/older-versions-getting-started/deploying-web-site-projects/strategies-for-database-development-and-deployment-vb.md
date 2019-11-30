---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
title: Estratégias para desenvolvimento e implantação de banco de dados (VB) | Microsoft Docs
author: rick-anderson
description: Ao implantar um aplicativo controlado por dados pela primeira vez, você pode copiar o banco de dado de um ambiente de desenvolvimento para o ambiente de produção. B...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 07b8905d-78ac-4252-97fb-8675b3fb0bbf
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
msc.type: authoredcontent
ms.openlocfilehash: 1ea4ade32fb05b9e69647ece7d1a4c400fe9fb21
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74616115"
---
# <a name="strategies-for-database-development-and-deployment-vb"></a>Estratégias de desenvolvimento e implantação de banco de dados (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_vb.pdf)

> Ao implantar um aplicativo controlado por dados pela primeira vez, você pode copiar o banco de dado de um ambiente de desenvolvimento para o ambiente de produção. Mas executar uma cópia oculta em implantações subsequentes substituirá todos os dados inseridos no banco de dado de produção. Em vez disso, a implantação de um banco de dados envolve a aplicação das alterações feitas no banco de dados de desenvolvimento desde a última implantação no banco de dados de produção. Este tutorial examina esses desafios e oferece várias estratégias para auxiliar no chronicling e aplicar as alterações feitas no banco de dados desde a última implantação.

## <a name="introduction"></a>Introdução

Conforme discutido nos tutoriais anteriores, implantar um aplicativo ASP.NET envolve copiar o conteúdo pertinente do ambiente de desenvolvimento para o ambiente de produção. A implantação não é um evento único, mas sim algo que acontece toda vez que uma nova versão do software é lançada ou quando são identificados e considerados bugs ou questões de segurança. Ao copiar páginas, imagens, arquivos JavaScript e outros arquivos do ASP.NET para o ambiente de produção, você não precisa se preocupar com a forma como esses arquivos foram alterados desde a última implantação. Você pode copiar o arquivo para produção, substituindo o conteúdo existente. Infelizmente, essa simplicidade não se estende à implantação do banco de dados.

Ao implantar um aplicativo controlado por dados pela primeira vez, você pode copiar o banco de dado de um ambiente de desenvolvimento para o ambiente de produção. Mas executar uma cópia oculta em implantações subsequentes substituirá todos os dados inseridos no banco de dado de produção. Em vez disso, a implantação de um banco de dados envolve a aplicação das *alterações* feitas no banco de dados de desenvolvimento desde a última implantação no banco de dados de produção. Este tutorial examina esses desafios e oferece várias estratégias para auxiliar no chronicling e aplicar as alterações feitas no banco de dados desde a última implantação.

## <a name="the-challenges-of-deploying-a-database"></a>Os desafios da implantação de um banco de dados

Antes que um aplicativo controlado por dados tenha sido implantado pela primeira vez, há apenas um banco de dado, ou seja, o banco do dados no ambiente de desenvolvimento, que é o motivo da implantação de um aplicativo controlado por dados pela primeira vez em que você pode copiar o banco do dados no ambiente de desenvolvimento para o ambiente de produção. Mas depois que o aplicativo tiver sido implantado, haverá duas cópias do banco de dados: uma em desenvolvimento e outra em produção.

Entre as implantações, os bancos de dados de desenvolvimento e produção podem ficar fora de sincronia. Embora o esquema do banco de dados de produção permaneça inalterado, o esquema do banco de dados de desenvolvimento pode mudar à medida que novos recursos são adicionados. Você pode adicionar ou remover colunas, tabelas, exibições ou procedimentos armazenados. Também pode haver dados importantes que são adicionados ao banco de dado de desenvolvimento. Muitos aplicativos controlados por dados incluem tabelas de pesquisa preenchidas com dados embutidos em código e específicos do aplicativo que não são editáveis pelo usuário. Por exemplo, um site de leilão pode ter uma lista suspensa com opções que descrevem a condição do item que está sendo leilões: novo, como novo, bom e justo. Em vez de embutir essas opções diretamente na lista suspensa, geralmente é melhor colocá-las em uma tabela de banco de dados. Se, durante o desenvolvimento, uma nova condição chamada ruim for adicionada à tabela, ao implantar o aplicativo, esse mesmo registro precisará ser adicionado à tabela de pesquisa no banco de dados de produção.

O ideal é que a implantação do banco de dados envolva a cópia do banco de dados do desenvolvimento para a produção. Mas tenha em mente que, depois de ter implantado o aplicativo e retomado o desenvolvimento, o banco de dados de produção estará sendo populado com real data de usuários reais. Portanto, se fosse simplesmente copiar o banco de dados do desenvolvimento para a produção na próxima implantação, você substituiria o banco de dados de produção e perderia os existentes. O resultado líquido é que a implantação do banco de dados se resume à aplicação das alterações feitas no banco de dados de desenvolvimento desde a última implantação.

Como a implantação de um banco de dados envolve aplicar as alterações no esquema e, possivelmente, os dados desde a última implantação, um histórico de alterações deve ser mantido (ou determinado em tempo de implantação) para que essas alterações possam ser aplicadas na produção. Há uma variedade de técnicas para gerenciar e aplicar alterações ao modelo de dados.

### <a name="defining-the-baseline"></a>Definindo a linha de base

Para manter as alterações no banco de dados de seu aplicativo, você precisa ter algum estado inicial, uma linha de base à qual as alterações são aplicadas. Em um extremo, o estado inicial pode ser um banco de dados vazio sem tabelas, exibições ou procedimentos armazenados. Essa linha de base resulta em um grande log de alterações porque ela deve incluir a criação de todas as tabelas, exibições e procedimentos armazenados do banco de dados, juntamente com as alterações feitas após a implantação inicial. Na outra extremidade do espectro, você poderia definir a linha de base como a versão do banco de dados que é inicialmente implantada no ambiente de produção. Essa opção resulta em um log de alterações muito menor porque inclui apenas as alterações feitas no banco de dados após a primeira implantação. Essa é a abordagem que eu prefiro. E é claro que você pode escolher um meio maior da abordagem de estrada, definindo a linha de base como algum ponto entre a criação inicial do banco de dados e quando o banco de dados é implantado pela primeira vez.

Depois de escolher uma linha de base, considere a possibilidade de gerar um script SQL que possa ser executado para recriar a versão de linha de base. Tal script torna possível recriar rapidamente a versão de linha de base do banco de dados. Essa funcionalidade é especialmente útil em projetos maiores, em que pode haver vários desenvolvedores trabalhando no projeto ou em ambientes adicionais, como teste ou preparo, que cada um precisa de sua própria cópia do banco de dados.

Há uma variedade de ferramentas à sua disposição para gerar um script SQL da versão de linha de base. No SQL Server Management Studio (SSMS), você pode clicar com o botão direito do mouse no banco de dados, ir até o submenu tarefas e escolher a opção gerar scripts. Isso inicia o assistente de script, que você pode instruir a gerar um arquivo que contém os comandos SQL para criar os objetos de s do banco de dados. Outra opção é o assistente de publicação de banco de dados, que pode gerar os comandos do SQL para não apenas criar o esquema de banco de dados, mas também os que estão nas tabelas. O assistente de publicação de banco de dados foi examinado em detalhes no tutorial *implantando um banco de dados* . Independentemente de qual ferramenta usar, no final você deve ter um arquivo de script que você pode usar para recriar a versão de linha de base do seu banco de dados, caso a necessidade seja necessária.

## <a name="documenting-the-database-changes-in-prose"></a>Documentando as alterações no banco de dados no Proseware

A maneira mais simples de manter um log de alterações no modelo de dados durante a fase de desenvolvimento é registrar as alterações na proseware. Por exemplo, se durante o desenvolvimento de um aplicativo já implantado você adicionar uma nova coluna à tabela `Employees`, remover uma coluna da tabela `Orders` e adicionar uma nova tabela (`ProductCategories`), você manteria um arquivo de texto ou documento do Microsoft Word com o seguinte histórico:

<a id="0.8_table01"></a>

| **Alterar Data** | **Alterar detalhes** |
| --- | --- |
| 2009-02-03: | Adição de `DepartmentID` de coluna (`int`, não nula) à tabela de `Employees`. Adicionada uma restrição FOREIGN KEY de `Departments.DepartmentID` para `Employees.DepartmentID`. |
| 2009-02-05: | Removida a `TotalWeight` de coluna da tabela `Orders`. Dados já capturados em registros de `OrderDetails` associados. |
| 2009-02-12: | Criou a tabela `ProductCategories`. Há três colunas: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`) e `Active` (`bit`, `NOT NULL`). Adicionada uma restrição PRIMARY KEY a `ProductCategoryID`e um valor padrão de 1 a `Active`. |

Há uma série de desvantagens nessa abordagem. Para iniciantes, não há espera para a automação. Sempre que essas alterações precisam ser aplicadas a um banco de dados, como quando o aplicativo é implantado, um desenvolvedor deve implementar manualmente cada alteração, uma de cada vez. Além disso, se você precisar reconstruir uma versão específica do banco de dados da linha de base usando o log de alterações, isso levará cada vez mais tempo à medida que o tamanho do log aumentar. Outra desvantagem desse método é que a clareza e o nível de detalhe de cada entrada do log de alterações são deixados para a pessoa que está registrando a alteração. Em uma equipe com vários desenvolvedores, alguns podem fazer entradas mais detalhadas, mais legíveis ou mais precisas do que outras. Além disso, são possíveis erros de digitação e de outros tipos de entrada de dados relacionados a pessoas.

O principal benefício de documentar as alterações no banco de dados no Proseware é a simplicidade. Você não precisa estar familiarizado com a sintaxe SQL para criar e alterar objetos de banco de dados. Em vez disso, você pode registrar as alterações na Proseware e implementá-las por meio da interface gráfica do usuário SQL Server Management Studio s.

Manter seu log de alterações na Proseware é, de forma reconhecida, não muito sofisticada e não funcionará bem com determinados projetos, como aqueles que são grandes no escopo, têm alterações frequentes no modelo de dados ou envolvem vários desenvolvedores. Mas já vi que essa abordagem funcionava bem em projetos pequenos, um homem que têm apenas alterações ocasionais no modelo de dados e onde o desenvolvedor solo não tem um forte plano de fundo na sintaxe SQL para criar e alterar objetos de banco de dados.

> [!NOTE]
> Embora as informações no log de alterações sejam, tecnicamente, necessárias apenas até o tempo de implantação, recomendo manter um histórico das alterações. Mas, em vez de manter um único arquivo de log de alterações cada vez maior, considere ter um arquivo de log de alterações diferente para cada versão do banco de dados. Normalmente, você desejará criar a versão do banco de dados sempre que ele for implantado. Ao manter um log dos logs de alterações que você pode, iniciando na linha de base, recrie qualquer versão do banco de dados executando os scripts de log de alteração a partir da versão 1 e continuando até chegar à versão que você precisa recriar.

## <a name="recording-the-sql-change-statements"></a>Registrando as instruções de alteração do SQL

A principal desvantagem de manter o log de alterações na Proseware é a falta de automação. O ideal é que a implementação de alterações de banco de dados no banco de dados de produção em tempo de implantação seja tão fácil quanto clicar em um botão para executar um script em vez de ter que executar manualmente uma lista de instruções. Essa automação é possível mantendo um log de alterações que contém os comandos SQL usados para alterar o modelo de dados.

A sintaxe SQL inclui várias instruções para criar e modificar vários objetos de banco de dados. Por exemplo, a [*instrução CREATE TABLE*](https://msdn.microsoft.com/library/ms174979.aspx), quando executada, cria uma nova tabela com as colunas e restrições especificadas. A [*instrução ALTER TABLE*](https://msdn.microsoft.com/library/ms190273.aspx) modifica uma tabela existente, adicionando, removendo ou modificando suas colunas ou restrições. Há também instruções para criar, modificar e descartar índices, exibições, funções definidas pelo usuário, procedimentos armazenados, gatilhos e outros objetos de banco de dados.

Voltando ao nosso exemplo anterior, imagem que, durante o desenvolvimento de um aplicativo já implantado, você adiciona uma nova coluna à tabela `Employees`, remove uma coluna da tabela `Orders` e adiciona uma nova tabela (`ProductCategories`). Essas ações resultaria em um arquivo de log de alterações com os seguintes comandos SQL:

[!code-sql[Main](strategies-for-database-development-and-deployment-vb/samples/sample1.sql)]

Enviar essas alterações para o banco de dados de produção no momento da implantação é uma operação de um clique: Abra SQL Server Management Studio, conecte-se ao banco de dados de produção, abra uma nova janela de consulta, Cole o conteúdo do log de alterações e clique em executar para executar o script.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Usando uma ferramenta de comparação para sincronizar os modelos de dados

É fácil documentar as alterações no banco de dados, mas implementar as alterações exige que um desenvolvedor faça cada alteração no banco de dados de produção, um de cada vez; documentar a alteração dos comandos SQL faz com que a implementação dessas alterações no banco de dados de produção seja fácil e rápida como clicar em um botão, mas requer aprendizado e dominar as instruções SQL e a sintaxe para criar e alterar objetos de banco de dados. As ferramentas de comparação de banco de dados levam o melhor das duas abordagens e descartam a pior.

Uma ferramenta de comparação de banco de dados compara o esquema ou os dados de dois bancos e exibe um relatório resumido mostrando a diferença entre eles. Em seguida, com o clique de um botão, você pode gerar os comandos SQL para sincronizar um ou mais objetos de banco de dados. Resumindo, você pode usar uma ferramenta de comparação de banco de dados para comparar os bancos de dados de desenvolvimento e produção no momento da implantação, gerando um arquivo que contém os comandos SQL que, quando executados, aplicará as alterações ao esquema do banco de dados de produção para que ele espelha o esquema de s do banco de dados de desenvolvimento.

Há uma variedade de ferramentas de comparação de banco de dados de terceiros oferecidas por vários fornecedores diferentes. Um exemplo é o [*SQL Compare*](http://www.red-gate.com/products/SQL_Compare/), por [*software Red Gate*](http://www.red-gate.com/). Vamos examinar o processo de uso do SQL Compare para comparar e sincronizar os esquemas de bancos de dados de desenvolvimento e de produção.

> [!NOTE]
> No momento em que este artigo foi escrito, a versão atual do SQL Compare era a versão 7,1, com o custo de edição Standard $395. Você pode acompanhar o download de uma avaliação gratuita de 14 dias.

Quando a comparação de SQL inicia a caixa de diálogo projetos de comparação é aberta, mostrando os projetos de comparação SQL salvos. Crie um novo projeto. Isso inicia o assistente de configuração de projeto, que solicita informações sobre os bancos de dados a serem comparados (consulte a Figura 1). Insira as informações para os bancos de dados do ambiente de desenvolvimento e de produção.

[![comparar os bancos de dados de desenvolvimento e produção](strategies-for-database-development-and-deployment-vb/_static/image2.jpg)](strategies-for-database-development-and-deployment-vb/_static/image1.jpg)

**Figura 1**: comparar os bancos de dados de desenvolvimento e de produção ([clique para exibir a imagem em tamanho normal](strategies-for-database-development-and-deployment-vb/_static/image3.jpg))

> [!NOTE]
> Se o banco de dados do ambiente de desenvolvimento for um arquivo de banco de dados do SQL Express Edition na pasta `App_Data` do seu site, você precisará registrar o banco de dados no servidor de banco de dados SQL Server Express para selecioná-lo na caixa de diálogo mostrada na Figura 1. A maneira mais fácil de fazer isso é abrir o SQL Server Management Studio (SSMS), conectar-se ao servidor de banco de dados SQL Server Express e anexar o banco de dados. Se você não tiver o SSMS instalado no seu computador, poderá baixar e instalar a versão gratuita do [*SQL Server 2008 Management Studio Basic*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).

Além de selecionar os bancos de dados a serem comparados, você também pode especificar uma variedade de configurações de comparação na guia Opções. Uma opção que talvez você queira ativar é "ignorar os nomes de restrição e de índice". Lembre-se de que, no tutorial anterior, adicionamos os objetos de banco de dados dos serviços de aplicativos aos bancos de dados de desenvolvimento e produção. Se você usou a ferramenta de `aspnet_regsql.exe` para criar esses objetos no banco de dados de produção, descobrirá que os nomes de chave primária e de restrição UNIQUE diferem entre os bancos de dados de desenvolvimento e produção. Consequentemente, o SQL Compare sinalizará todas as tabelas de serviços de aplicativos como sendo diferentes. Você pode deixar as opções "ignorar restrição e nomes de índice" desmarcadas e sincronizar os nomes de restrição ou instruir o SQL Compare a ignorar essas diferenças.

Depois de selecionar os bancos de dados para comparar (e revisar as opções de comparação), clique no botão comparar agora para iniciar a comparação. Nos próximos segundos, o SQL Compare examina os esquemas dos dois bancos de dados e gera um relatório de como eles diferem. Fiz algumas modificações feitas no banco de dados de desenvolvimento para mostrar como essas discrepâncias são indicadas na interface SQL Compare. Como mostra a Figura 2, eu adicionei uma coluna `BirthDate` à tabela `Authors`, removi a coluna `ISBN` da tabela `Books` e adicionei uma nova tabela, `Ratings`, que é destinada a permitir que os usuários visitem a taxa de sites dos livros revisados.

> [!NOTE]
> As alterações no modelo de dados feitas neste tutorial foram feitas para ilustrar o uso de uma ferramenta de comparação de banco de dado. Você não encontrará essas alterações no banco de dados em Tutoriais futuros.

[![SQL Compare lista as diferenças entre os bancos de dados de desenvolvimento e produção](strategies-for-database-development-and-deployment-vb/_static/image5.jpg)](strategies-for-database-development-and-deployment-vb/_static/image4.jpg)

**Figura 2**: SQL Compare lista as diferenças entre os bancos de dados de desenvolvimento e produção ([clique para exibir a imagem em tamanho normal](strategies-for-database-development-and-deployment-vb/_static/image6.jpg))

O SQL Compare divide os objetos de banco de dados em grupos, mostrando rapidamente quais objetos existem em ambos os bancos, mas são diferentes, quais objetos existem em um banco de dados, mas não o outro, e quais objetos são idênticos. Como você pode ver, há dois objetos que existem em ambos os bancos de dados, mas são diferentes: a tabela `Authors`, que tinha uma coluna adicionada e a tabela `Books`, que tinha uma removida. Há um objeto que existe somente no banco de dados de desenvolvimento, ou seja, a tabela de `Ratings` recém-criada. E há 117 objetos que são idênticos em ambos os bancos de dados.

A seleção de um objeto de banco de dados exibe a janela SQL Differences, que mostra como esses objetos diferem. A janela diferenças de SQL, exibida na parte inferior da Figura 2, realça que a tabela `Authors` no banco de dados de desenvolvimento tem a coluna `BirthDate`, que não é encontrada na tabela `Authors` no banco de dados de produção.

Depois de revisar as diferenças e selecionar quais objetos você deseja sincronizar, a próxima etapa é gerar os comandos SQL necessários para atualizar o esquema do banco de dados de produção para corresponder ao banco de dados de desenvolvimento. Isso é feito por meio do assistente de sincronização. O assistente de sincronização confirma quais objetos sincronizar e resume o plano de ação (consulte a Figura 3). Você pode sincronizar os bancos de dados imediatamente ou gerar um script com os comandos SQL que podem ser executados ao seu lazer.

[![usar o assistente de sincronização para sincronizar os esquemas de bancos de dados](strategies-for-database-development-and-deployment-vb/_static/image8.jpg)](strategies-for-database-development-and-deployment-vb/_static/image7.jpg)

**Figura 3**: usar o assistente de sincronização para sincronizar os esquemas de bancos de dados ([clique para exibir a imagem em tamanho normal](strategies-for-database-development-and-deployment-vb/_static/image9.jpg))

Ferramentas de comparação de banco de dados como Red Gate software s SQL Compare fazer aplicando as alterações ao esquema de banco de dados de desenvolvimento ao banco de dados de produção tão fácil quanto apontar e clicar.

> [!NOTE]
> O SQL Compare compara e sincroniza dois *esquemas*de bancos de dados. Infelizmente, ele não compara e sincroniza os dados em duas tabelas de bancos de dados. O software Red Gate oferece um produto chamado [*SQL Data Compare*](http://www.red-gate.com/products/SQL_Data_Compare/) que compara e sincroniza os dados entre dois bancos de dados, mas é um produto separado do SQL compare e custa outro $395.

## <a name="taking-the-application-offline-during-deployment"></a>Colocando o aplicativo offline durante a implantação

Como vimos em todos esses tutoriais, a implantação é um processo que envolve várias etapas: copiar as páginas do ASP.NET, as páginas mestras, os arquivos CSS, os arquivos JavaScript, as imagens e outros conteúdos necessários do ambiente de desenvolvimento para a produção ambiente copiando as informações de configuração específicas do ambiente de produção, se necessário; e aplicando as alterações ao modelo de dados desde a última implantação. Dependendo do número de arquivos e da complexidade das alterações no banco de dados, essas etapas podem levar de alguns segundos a vários minutos para serem concluídas. Durante essa janela, o aplicativo Web está em fluxo e os usuários que visitam o site podem apresentar erros ou comportamento inesperado.

Ao implantar um site, é melhor colocar o aplicativo Web "offline" até que a implantação seja concluída. Colocar o aplicativo offline (e colocá-lo novamente após a conclusão do processo de implantação) é tão fácil quanto carregar um arquivo e, em seguida, excluí-lo. A partir do ASP.NET 2,0, a mera presença de um arquivo chamado `app_offline.htm` no diretório raiz do aplicativo usa todo o site "offline". Qualquer solicitação para uma página do ASP.NET nesse site é automaticamente respondida com o conteúdo do arquivo de `app_offline.htm`. Depois que o arquivo é removido, o aplicativo volta a ficar online.

Colocar um aplicativo offline durante a implantação é tão simples quanto carregar um arquivo de `app_offline.htm` no diretório raiz do ambiente de produção antes de iniciar o processo de implantação e, em seguida, excluí-lo (ou renomeá-lo para outra coisa) após a conclusão da implantação. Para obter mais informações sobre essa técnica, consulte o artigo John Peterson s, colocando um [*aplicativo ASP.net offline*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Resumo

O principal desafio na implantação de um centro de aplicativos orientado a dados em relação à implantação do Database. Como há duas versões do banco de dados – uma no ambiente de desenvolvimento e outra no ambiente de produção-esses dois esquemas de bancos de dados podem ficar fora de sincronia à medida que novos recursos são adicionados ao desenvolvimento. O que mais, porque o banco de dados de produção está sendo populado com real data de usuários reais, você não pode substituir o banco de dado de produção pelo Database de desenvolvimento modificado como você pode ao implantar os arquivos que compõem o aplicativo (as páginas ASP.NET, arquivos de imagem e assim por diante). Em vez disso, a implantação de um banco de dados envolve a implementação do conjunto preciso de alterações feitas no banco de dados de desenvolvimento, desde a última implantação.

Este tutorial examinou três técnicas para manter e aplicar um log das alterações no banco de dados. A abordagem mais simples é registrar as alterações na proseware. Embora essa tática faça com que a implementação dessas alterações no banco de dados de produção seja um processo manual, ela não exige o conhecimento dos comandos SQL para criar e alterar objetos de banco de dados. Uma abordagem mais sofisticada, e uma que é muito mais agradável em projetos ou projetos maiores com vários desenvolvedores, é registrar as alterações como uma série de comandos SQL. Isso Hastens enormemente a distribuição dessas alterações para o banco de dados de destino. O melhor das duas abordagens pode ser obtido usando uma ferramenta de comparação de banco de dados, como o Red Gate software s SQL Compare.

Este tutorial conclui nosso foco na implantação de um aplicativo controlado por dados. O próximo conjunto de tutoriais analisa como responder a erros no ambiente de produção. Veremos como exibir uma página de erro amigável em vez da tela amarela de morte. E veremos como registrar os detalhes do erro e como alertá-lo quando esses erros ocorrerem.

Boa programação!

> [!div class="step-by-step"]
> [Anterior](configuring-a-website-that-uses-application-services-vb.md)
> [Próximo](displaying-a-custom-error-page-vb.md)
