---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Implantando projetos de banco de dados | Microsoft Docs
author: jrjlee
description: 'Observação: em vários cenários de implantação empresarial, você precisa da capacidade de publicar atualizações incrementais em um banco de dados implantado. A alternativa é recriar...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 221808758492aedb8e8329364e511df28fd11105
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634263"
---
# <a name="deploying-database-projects"></a>Implantação de projetos de banco de dados

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> Em vários cenários de implantação empresarial, você precisa da capacidade de publicar atualizações incrementais em um banco de dados implantado. A alternativa é recriar o banco de dados em todas as implantações, o que significa que você perde todos os dados existentes nele. Quando você trabalha com o Visual Studio 2010, o uso do VSDBCMD é a abordagem recomendada para a publicação incremental do banco de dados. No entanto, a próxima versão do Visual Studio e do WPP (Web Publishing Pipeline) incluirão ferramentas que dão suporte à publicação incremental diretamente.

Se você abrir a solução de exemplo Contact Manager no Visual Studio 2010, verá que o projeto de banco de dados inclui uma pasta Properties que contém quatro arquivos.

![](deploying-database-projects/_static/image1.png)

Junto com o arquivo de projeto (*ContactManager. Database. dbproj* , nesse caso), esses arquivos controlam vários aspectos do processo de compilação e implantação:

- O arquivo *Database. sqlcmdvars* fornece valores para quaisquer variáveis do sqlcmd que você usar ao implantar o projeto. Cada configuração de solução (por exemplo, Debug e Release) pode especificar um arquivo. sqlcmdvars diferente.
- O arquivo *Database. sqldeployment* fornece configurações específicas de implantação, como usar o agrupamento definido em seu projeto ou o agrupamento do servidor de destino, se deseja recriar o banco de dados de destino toda vez ou simplesmente corrigir o banco de dados existente para atualizá-lo e assim por diante. Cada configuração de solução pode especificar um arquivo. sqldeployment diferente.
- O arquivo *Database. sqlpermissions* é um documento XML que você pode usar para definir as permissões que deseja adicionar ao banco de dados de destino. Todas as configurações de solução compartilham o mesmo arquivo. sqlpermissions.
- O arquivo *Database. Configurations* especifica as propriedades no nível do banco de dados a serem usadas ao criar o banco de dados, como o agrupamento a ser usado, o comportamento dos operadores de comparação e assim por diante. Todas as configurações de solução compartilham o mesmo arquivo. sqlsettings.

Vale a pena reservar um momento para abrir esses arquivos no Visual Studio e se familiarizar com o conteúdo.

Quando você cria um projeto de banco de dados, o processo de compilação cria dois arquivos:

- Um *esquema de banco de dados* (arquivo. dbschema). Isso descreve o esquema do banco de dados que você deseja criar no formato XML.
- Um *manifesto de implantação* (arquivo. DeployManifest). Isso contém todas as informações necessárias para criar e implantar seu banco de dados. Ele faz referência ao arquivo. dbschema junto com outros recursos, como as instruções de implantação (o arquivo. sqldeployment) e quaisquer scripts SQL pré-implantação ou pós-implantação.

Isso mostra a relação entre estes recursos:

![](deploying-database-projects/_static/image2.png)

Como você pode ver, o arquivo. Configurations e o arquivo. sqlpermissions são entradas para o processo de compilação. Junto com o arquivo de projeto de banco de dados, esses arquivos são usados para criar o arquivo de esquema de banco de dados. O arquivo. sqldeployment e o arquivo. sqlcmdvars passam pelo processo de compilação inalterados. O manifesto de implantação indica o local do esquema de banco de dados, o arquivo. sqldeployment, o arquivo. sqlcmdvars e quaisquer scripts SQL pré-implantação ou pós-implantação.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Por que usar o VSDBCMD para implantar um projeto de banco de dados?

Há várias abordagens diferentes para implantar projetos de banco de dados. No entanto, nem todos eles são adequados para implantar um projeto de banco de dados em servidores remotos em um ambiente corporativo. Considere o que você deseja em uma implantação de projeto de banco de dados. Em cenários de implantação empresarial, você provavelmente vai querer:

- A capacidade de implantar o projeto de banco de dados de um local remoto.
- A capacidade de fazer atualizações incrementais em um banco de dados existente.
- A capacidade de incluir scripts de pré-implantação ou scripts pós-implantação.
- A capacidade de personalizar a implantação em vários ambientes de destino.
- A capacidade de implantar o projeto de banco de dados como parte de uma implantação de solução de uma única etapa, geralmente com script.

Há três abordagens principais que você pode usar para implantar um projeto de banco de dados:

- Você pode usar a funcionalidade de implantação com o tipo de projeto de banco de dados no Visual Studio 2010. Quando você cria e implanta um projeto de banco de dados no Visual Studio 2010, o processo de implantação usa o manifesto de implantação para gerar um arquivo de implantação baseado em SQL específico para a configuração de compilação. Isso criará o banco de dados, caso ele ainda não exista ou faça as alterações necessárias no banco de dados, caso ele já exista. Você pode usar o SQLCMD. exe para executar esse arquivo no servidor de destino ou pode definir o Visual Studio para criar e executar o arquivo. A desvantagem dessa abordagem é que você tem apenas controle limitado sobre as configurações de implantação. Geralmente, você também precisa modificar o arquivo de implantação do SQL para fornecer valores de variáveis específicos do ambiente. Você só pode usar essa abordagem de um computador com o Visual Studio 2010 instalado, e o desenvolvedor precisaria saber e fornecer cadeias de conexão e credenciais para todos os ambientes de destino.
- Você pode usar a Implantação da Web (ferramenta de implantação da Web) do Serviços de Informações da Internet (IIS) para [implantar um banco de dados como parte de um projeto de aplicativo Web](https://msdn.microsoft.com/library/dd465343.aspx). No entanto, essa abordagem é muito mais complexa se você quiser implantar um projeto de banco de dados em vez de simplesmente replicar um banco de dados local existente em um servidor de destino. Você pode configurar Implantação da Web para executar o script de implantação do SQL que o projeto de banco de dados gera, mas para fazer isso, você precisa criar um arquivo de destino de WPP personalizado para seu projeto de aplicativo Web. Isso adiciona uma grande quantidade de complexidade ao processo de implantação. Além disso, Implantação da Web não dá suporte diretamente a atualizações incrementais para bancos de dados existentes. Para obter mais informações sobre essa abordagem, consulte [estendendo o pipeline de publicação da Web para projeto de banco de dados de pacote implantação de arquivo SQL](https://go.microsoft.com/?linkid=9805121).
- Você pode usar o utilitário VSDBCMD para implantar o banco de dados, usando o esquema de banco de dados ou o manifesto de implantação. Você pode chamar VSDBCMD. exe de um destino do MSBuild, que permite publicar bancos de dados como parte de um processo de implantação com script maior. Você pode substituir as variáveis no arquivo. sqlcmdvars e muitas outras propriedades de banco de dados de um comando VSDBCMD, que permite que você personalize sua implantação para ambientes diferentes sem criar várias configurações de compilação. O VSDBCMD fornece a funcionalidade de diferenciação, o que significa que ele fará apenas as alterações necessárias para alinhar um banco de dados de destino ao seu esquema de banco de dados. O VSDBCMD também oferece uma ampla variedade de opções de linha de comando, que oferecem um controle refinado sobre o processo de implantação.

Nesta visão geral, você pode ver que usar o VSDBCMD com o MSBuild é a abordagem mais adequada para um cenário de implantação empresarial típico:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Dá suporte à implantação remota? | Sim | Sim | Sim |
| Dá suporte a atualizações incrementais? | Sim | Não | Sim |
| Dá suporte a scripts pré/pós-implantação? | Sim | Sim | Sim |
| Dá suporte à implantação de vários ambientes? | Limitado | Limitado | Sim |
| Dá suporte à implantação com script? | Limitado | Sim | Sim |

O restante deste tópico descreve o uso do VSDBCMD com o MSBuild para implantar projetos de banco de dados.

## <a name="understanding-the-deployment-process"></a>Noções básicas sobre o processo de implantação

O utilitário VSDBCMD permite que você implante um banco de dados usando o esquema de banco de dados (o arquivo. dbschema) ou o manifesto de implantação (o arquivo. DeployManifest). Na prática, você quase sempre usará o manifesto de implantação, pois o manifesto de implantação permite que você forneça valores padrão para várias propriedades de implantação e identifique quaisquer scripts SQL de pré-implantação ou pós-implantação que deseja executar. Por exemplo, esse comando VSDBCMD é usado para implantar o banco de dados **ContactManager** em um servidor de banco de dados em um ambiente de teste:

[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]

Nesse caso:

- A opção **/a** (ou **/Action**) especifica o que você deseja que o VSDBCMD faça. Você pode definir isso para **importar** ou **implantar**. A opção de **importação** é usada para gerar um arquivo. dbschema de um banco de dados existente e a opção **implantar** é usada para implantar um arquivo. dbschema em um banco de dados de destino.
- A opção **/manifest** (ou **/MANIFESTFILE**) identifica o arquivo. DeployManifest que você deseja implantar. Se você quisesse usar o arquivo. dbschema, usaria a opção **/Model** (ou **/ModelFile**).
- A opção **/cs** (ou **/ConnectionString**) fornece a cadeia de conexão para o servidor de banco de dados de destino. Observe que isso não inclui o nome do banco de&#x2014;dados VSDBCMD precisa se conectar ao servidor para criar o banco de dados; Ele não precisa se conectar a um banco de dados individual. Se o arquivo. DeployManifest incluir uma cadeia de conexão, você poderá omitir essa opção. Se você usar a opção mesmo assim, o valor da opção substituirá o valor de. DeployManifest.
- A propriedade <strong>/p: TargetDatabase</strong> fornece o nome que você deseja atribuir ao banco de dados de destino na criação. Isso substitui o valor da propriedade <strong>TargetDatabase</strong> no arquivo. DeployManifest. Você pode usar a sintaxe <strong>/p:</strong> <em>[nome da propriedade]</em>para definir uma ampla variedade de propriedades de implantação e substituir quaisquer variáveis SQLCMD declaradas em seu arquivo. sqlcmdvars.
- A opção **/DD +** (ou **/DeployToDatabase +** ) indica que você deseja criar uma implantação e implantá-la no ambiente de destino. Se você especificar **/DD-** ou omitir a opção, o VSDBCMD irá gerar um script de implantação, mas não o implantará no ambiente de destino. Essa opção é geralmente a fonte de confusão e é explicada com mais detalhes na próxima seção.
- A opção **/script** (ou **/DeploymentScriptFile**) especifica onde você deseja gerar o script de implantação. Esse valor não afeta o processo de implantação.

Para obter mais informações sobre VSDBCMD, consulte [referência de linha de comando para VSDBCMD. EXE (implantação e importação de esquema)](https://msdn.microsoft.com/library/dd193283.aspx) e [como preparar um banco de dados para implantação a partir de um prompt de comando usando o VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx).

Para obter um exemplo de como você pode usar o VSDBCMD de um arquivo de projeto do MSBuild, consulte [noções básicas sobre o processo de compilação](understanding-the-build-process.md). Para obter exemplos de como definir as configurações de implantação de banco de dados para vários ambientes, consulte [Personalizando implantações de banco de dados para vários ambientes](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>Compreendendo a opção DeployToDatabase

O comportamento da opção **/DD** ou **/DeployToDatabase** depende se você está usando o VSDBCMD com um arquivo. dbschema ou um arquivo. DeployManifest. Se você estiver usando um arquivo. dbschema, o comportamento será bem simples:

- Se você especificar **/DD +** ou **/DD**, VSDBCMD irá gerar um script de implantação e implantar o banco de dados.
- Se você especificar **/DD-** ou omitir a opção, o VSDBCMD irá gerar apenas um script de implantação.

Se você estiver usando um arquivo. DeployManifest, o comportamento será muito mais complicado. Isso ocorre porque o arquivo. DeployManifest contém um nome de propriedade **DeployToDatabase** que também determina se o banco de dados está implantado.

[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]

O valor dessa propriedade é definido de acordo com as propriedades do projeto de banco de dados. Se você definir a **ação implantar** para **criar um script de implantação (. Sql)** , o valor será **false**. Se você definir a **ação implantar** para **criar um script de implantação (. Sql) e implantar no banco de dados**, o valor será **true**.

> [!NOTE]
> Essas configurações estão associadas a uma plataforma e configuração de compilação específicas. Por exemplo, se você definir as configurações para a configuração de **depuração** e, em seguida, publicar usando a configuração de **versão** , suas configurações não serão usadas.

![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> Nesse cenário, a **ação implantar** sempre deve ser definida para **criar um script de implantação (. Sql)** , porque você não quer que o Visual Studio 2010 implante seu banco de dados. Em outras palavras, a propriedade **DeployToDatabase** sempre deve ser **falsa**.

Quando uma propriedade **DeployToDatabase** é especificada, a opção **/DD** só substituirá a propriedade se o valor da propriedade for **false**:

- Se a propriedade **DeployToDatabase** for **false**e você especificar **/DD +** ou **/DD**, VSDBCMD substituirá a propriedade **DeployToDatabase** e implantará o banco de dados.
- Se a propriedade **DeployToDatabase** for **false**e você especificar **/DD-** ou omitir a opção, VSDBCMD não implantará o banco de dados.
- Se a propriedade **DeployToDatabase** for **true**, VSDBCMD ignorará a opção e implantará o banco de dados.
- Um script de implantação é gerado em cada caso, independentemente de você estar implantando o banco de dados também.

## <a name="conclusion"></a>Conclusão

Este tópico forneceu uma visão geral do processo de compilação e implantação para projetos de banco de dados no Visual Studio 2010. Ele também descreveu como você pode usar o VSDBCMD. exe com o MSBuild para dar suporte à implantação de banco de dados de escala empresarial.

Para obter mais informações sobre como isso funciona na prática, consulte [Personalizando implantações de banco de dados para vários ambientes](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Leitura adicional

Para obter informações sobre como personalizar implantações de banco de dados criando um arquivo de configuração de implantação separado para cada ambiente, consulte [Personalizando implantações de banco de dados para vários ambientes](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Para obter orientação sobre como configurar associações de função de banco de dados executando um script pós-implantação, consulte [implantando associações de função de banco de dados para ambientes de teste](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Para obter orientação sobre como gerenciar alguns dos desafios exclusivos que os bancos de dados de associação impõem, consulte [implantando bancos de dados de associação em ambientes corporativos](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Estes tópicos sobre o MSDN fornecem diretrizes mais amplas e informações básicas sobre projetos de banco de dados do Visual Studio e o processo de implantação de banco de dados:

- [Projetos de banco de dados do Visual Studio 2010 SQL Server](https://msdn.microsoft.com/library/ff678491.aspx)
- [Gerenciando a alteração do banco de dados](https://msdn.microsoft.com/library/aa833404.aspx)
- [Como preparar um banco de dados para implantação a partir de um prompt de comando usando VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [Uma visão geral do Compilação e Implantação de banco de dados](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-web-packages.md)
> [Próximo](creating-and-running-a-deployment-command-file.md)
