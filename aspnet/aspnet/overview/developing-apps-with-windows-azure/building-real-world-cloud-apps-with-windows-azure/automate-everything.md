---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Automatizar tudo (criando aplicativos de nuvem do mundo real com o Azure) | Microsoft Docs
author: MikeWasson
description: A criação de aplicativos de nuvem do mundo real com o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: e741a753a36ebdaefbff8eee0b38911785c716ac
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584941"
---
# <a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Automatizar tudo (criando aplicativos de nuvem do mundo real com o Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar o livro eletrônico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> A **criação de aplicativos de nuvem do mundo real com** o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem ajudá-lo a desenvolver com sucesso aplicativos Web para a nuvem. Para obter uma introdução ao livro eletrônico, consulte [o primeiro capítulo](introduction.md).

Os três primeiros padrões que veremos realmente se aplicam a qualquer projeto de desenvolvimento de software, mas especialmente a projetos em nuvem. Esse padrão é sobre a automação de tarefas de desenvolvimento. É um tópico importante porque processos manuais são lentos e sujeitos a erros; automatizar o máximo possível ajuda a configurar um fluxo de trabalho rápido, confiável e ágil. É exclusivamente importante para o desenvolvimento em nuvem, pois você pode automatizar facilmente muitas tarefas que são difíceis ou impossíveis de automatizar em um ambiente local. Por exemplo, você pode configurar ambientes de teste completos, incluindo o novo servidor Web e VMs de back-end, bancos de dados, armazenamento de BLOBs (armazenamento de arquivos), filas, etc.

## <a name="devops-workflow"></a>Fluxo de trabalho do DevOps

Cada vez que você ouve o termo "DevOps". O termo desenvolvido a partir de um reconhecimento que você precisa para integrar tarefas de desenvolvimento e operações a fim de desenvolver software com eficiência. O tipo de fluxo de trabalho que você deseja habilitar é aquele em que é possível desenvolver um aplicativo, implantá-lo, aprender com o uso de produção, alterá-lo em resposta ao que você aprendeu e repetir o ciclo de forma rápida e confiável.

Algumas equipes de desenvolvimento de nuvem bem-sucedidas são implantadas várias vezes por dia em um ambiente ativo. A equipe do Azure usou para implantar uma atualização principal a cada 2-3 meses, mas agora ela lança atualizações secundárias a cada 2-3 dias e versões principais a cada 2-3 semanas. Entrar nessa cadência realmente ajuda você a responder aos comentários dos clientes.

Para fazer isso, você precisa habilitar um ciclo de desenvolvimento e implantação que seja repetível, confiável, previsível e tenha um tempo de ciclo baixo.

![Fluxo de trabalho do DevOps](automate-everything/_static/image1.png)

Em outras palavras, o período de tempo entre o momento em que você tem uma ideia para um recurso e quando os clientes o estão usando e o fornecimento de comentários deve ser o mais curto possível. Os três primeiros padrões – automatizar tudo, controle do código-fonte e integração e entrega contínua--são todas as práticas recomendadas que recomendamos para habilitar esse tipo de processo.

## <a name="azure-management-scripts"></a>Scripts de gerenciamento do Azure

Na [introdução a este livro eletrônico](introduction.md), você viu o console baseado na Web, o portal de gerenciamento do Azure. O portal de gerenciamento permite que você monitore e gerencie todos os recursos implantados no Azure. É uma maneira fácil de criar e excluir serviços como aplicativos Web e VMs, configurar esses serviços, monitorar a operação de serviço e assim por diante. É uma ótima ferramenta, mas usá-la é um processo manual. Se você pretende desenvolver um aplicativo de produção de qualquer tamanho e, especialmente em um ambiente de equipe, recomendamos que você passe pela interface do usuário do portal para aprender e explorar o Azure e, em seguida, automatizar os processos que você fará repetidamente.

Quase tudo o que você pode fazer manualmente no portal de gerenciamento ou no Visual Studio também pode ser feito chamando a API de gerenciamento REST. Você pode escrever scripts usando o [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx)ou pode usar uma estrutura de software livre, como [chefe](http://www.opscode.com/chef/) ou [Puppet](http://puppetlabs.com/puppet/what-is-puppet). Você também pode usar a ferramenta de linha de comando bash em um ambiente Mac ou Linux. O Azure tem APIs de script para todos esses ambientes diferentes e tem uma [API de gerenciamento do .net](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) , caso você queira escrever código em vez de script.

Para o aplicativo corrigir ti, criamos alguns scripts do Windows PowerShell que automatizam os processos de criação de um ambiente de teste e implantação do projeto nesse ambiente, e examinaremos alguns dos conteúdos desses scripts.

## <a name="environment-creation-script"></a>Script de criação de ambiente

O primeiro script que veremos é chamado de *New-AzureWebsiteEnv. ps1*. Ele cria um ambiente do Azure no qual você pode implantar o aplicativo corrigir ti para teste. As principais tarefas que esse script executa são as seguintes:

- Crie um aplicativo Web.
- Criar uma conta de armazenamento. (Necessário para BLOBs e filas, como você verá em capítulos posteriores.)
- Crie um servidor do banco de dados SQL e dois bancos de dados: um banco de dados de aplicativo e um banco de dados de associação.
- Armazene as configurações no Azure que o aplicativo usará para acessar a conta de armazenamento e os bancos de dados.
- Crie arquivos de configurações que serão usados para automatizar a implantação.

### <a name="run-the-script"></a>Executar o script

> [!NOTE]
> Esta parte do capítulo mostra exemplos de scripts e os comandos que você insere para executá-los. Esta é uma demonstração e não fornece tudo o que você precisa saber para executar os scripts. Para obter instruções passo a passo, consulte [o apêndice: o aplicativo de exemplo corrigir ti](the-fix-it-sample-application.md#deploybase).

Para executar um script do PowerShell que gerencia os serviços do Azure, você precisa instalar o console do Azure PowerShell e configurá-lo para funcionar com sua assinatura do Azure. Quando estiver configurado, você poderá executar o script corrigir a criação do ambiente de ti com um comando como este:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

O parâmetro `Name` especifica o nome a ser usado ao criar o banco de dados e as contas de armazenamento, e o parâmetro `SqlDatabasePassword` especifica a senha da conta de administrador que será criada para o banco de dados SQL. Há outros parâmetros que você pode usar para examinar mais tarde.

![Janela do PowerShell](automate-everything/_static/image2.png)

Depois que o script for concluído, você poderá ver no portal de gerenciamento o que foi criado. Você encontrará dois bancos de dados:

![Bancos de dados](automate-everything/_static/image3.png)

Uma conta de armazenamento:

![Conta de armazenamento](automate-everything/_static/image4.png)

E um aplicativo Web:

![Site da Web](automate-everything/_static/image5.png)

Na guia **Configurar** do aplicativo Web, você pode ver que ele tem as configurações da conta de armazenamento e as cadeias de conexão do banco de dados SQL configuradas para o aplicativo corrigir ti.

![appSettings e connectionStrings](automate-everything/_static/image6.png)

A pasta de *automação* agora também contém um arquivo *&lt;websitename&gt;. pubxml* . Esse arquivo armazena as configurações que o MSBuild usará para implantar o aplicativo no ambiente do Azure que acabou de ser criado. Por exemplo:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Como você pode ver, o script criou um ambiente de teste completo e todo o processo é feito em cerca de 90 segundos.

Se outra pessoa da sua equipe quiser criar um ambiente de teste, poderá simplesmente executar o script. Isso não só é rápido, mas também pode ter certeza de que estão usando um ambiente idêntico ao que você está usando. Não seria tão confiante se todos estivesse configurando as coisas manualmente usando a interface do usuário do portal de gerenciamento.

### <a name="a-look-at-the-scripts"></a>Uma olhada nos scripts

Na verdade, há três scripts que fazem esse trabalho. Você chama um na linha de comando e ele usa automaticamente os outros dois para realizar algumas das tarefas:

- *New-AzureWebSiteEnv. ps1* é o script principal.

    - *New-AzureStorage. ps1* cria a conta de armazenamento.
    - *New-AzureSql. ps1* cria os bancos de dados.

### <a name="parameters-in-the-main-script"></a>Parâmetros no script principal

O script principal, *New-AzureWebSiteEnv. ps1*, define vários parâmetros:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

São necessários dois parâmetros:

- O nome do aplicativo Web que o script cria. (Isso também é usado para a URL: `<name>.azurewebsites.net`.)
- A senha para o novo usuário administrativo do servidor de banco de dados que o script cria.

Os parâmetros opcionais permitem especificar o local do data center (o padrão é "oeste dos EUA"), o nome do administrador do servidor de banco de dados (o padrão é "dbuser") e uma regra de firewall para o servidor de banco de dados.

### <a name="create-the-web-app"></a>Criar o aplicativo Web

A primeira coisa que o script faz é criar o aplicativo Web chamando o cmdlet `New-AzureWebsite`, passando o nome do aplicativo Web e os valores de parâmetro de local:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Criar a conta de armazenamento

Em seguida, o script principal executa o script *New-AzureStorage. ps1* , especificando " *&lt;websitename&gt;* Storage" para o nome da conta de armazenamento e o mesmo Data Center local que o aplicativo Web.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*New-AzureStorage. ps1* chama o cmdlet `New-AzureStorageAccount` para criar a conta de armazenamento e retorna o nome da conta e os valores de chave de acesso. O aplicativo precisará desses valores para acessar os BLOBs e as filas na conta de armazenamento.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Talvez você nem sempre queira criar uma nova conta de armazenamento; Você pode aprimorar o script adicionando um parâmetro que, opcionalmente, o direciona para usar uma conta de armazenamento existente.

### <a name="create-the-databases"></a>Criar os bancos de dados

Em seguida, o script principal executa o script de criação de banco de dados, *New-AzureSql. ps1*, após a configuração de nomes de regra de firewall e banco de dados padrão:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

O script de criação de banco de dados recupera o endereço IP da máquina de desenvolvimento e define uma regra de firewall para que o computador de desenvolvimento possa se conectar e gerenciar o servidor. Em seguida, o script de criação de banco de dados passa por várias etapas para configurar os banco de dados:

- Cria o servidor usando o cmdlet `New-AzureSqlDatabaseServer`.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Cria regras de firewall para permitir que o computador de desenvolvimento gerencie o servidor e habilite o aplicativo Web para se conectar a ele. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Cria um contexto de banco de dados que inclui o nome do servidor e as credenciais, usando o cmdlet `New-AzureSqlDatabaseServerContext`.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` é uma função no script que chama o cmdlet `ConvertTo-SecureString` para criptografar a senha e retorna um objeto `PSCredential`, o mesmo tipo que o cmdlet `Get-Credential` retorna.
- Cria o banco de dados do aplicativo e o banco de dados de associação usando o cmdlet `New-AzureSqlDatabase`.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Chama uma função definida localmente para criar uma cadeia de conexão para cada banco de dados. O aplicativo usará essas cadeias de conexão para acessar os bancos de dados. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get-SQLAzureDatabaseConnectionString é uma função definida no script que cria a cadeia de conexão dos valores de parâmetro fornecidos a ele.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Retorna uma tabela de hash com o nome do servidor de banco de dados e as cadeias de conexão.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

O aplicativo corrigir ti usa associação e bancos de dados de aplicativos separados. Também é possível colocar os dados da associação e do aplicativo em um único banco de dado.

### <a name="store-app-settings-and-connection-strings"></a>Armazenar configurações de aplicativo e cadeias de conexão

O Azure tem um recurso que permite que você armazene configurações e cadeias de conexão que substituem automaticamente o que é retornado ao aplicativo quando ele tenta ler as coleções de `appSettings` ou `connectionStrings` no arquivo Web. config. Essa é uma alternativa à aplicação de [transformações de Web. config](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) quando você implanta o. Para obter mais informações, consulte [armazenar dados confidenciais no Azure](source-control.md#appsettings) posteriormente neste livro eletrônico.

O script de criação de ambiente armazena no Azure todos os valores de `appSettings` e `connectionStrings` que o aplicativo precisa para acessar a conta de armazenamento e os bancos de dados quando ele é executado no Azure.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

O [New Relic](http://newrelic.com/) é uma estrutura de telemetria que demonstramos no capítulo [monitoramento e telemetria](monitoring-and-telemetry.md) . O script de criação de ambiente também reinicia o aplicativo Web para certificar-se de que ele pega as novas configurações de Relic.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Preparando para implantação

No final do processo, o script de criação de ambiente chama duas funções para criar arquivos que serão usados pelo script de implantação.

Uma dessas funções cria um perfil de publicação *(&lt;arquivo websitename&gt;. pubxml* ). O código chama a API REST do Azure para obter as configurações de publicação e salva as informações em um arquivo *. publishsettings* . Em seguida, ele usa as informações desse arquivo junto com um arquivo de modelo (*pubxml. Template*) para criar o arquivo *. pubxml* que contém o perfil de publicação. Esse processo de duas etapas simula o que você faz no Visual Studio: baixar um arquivo *. publishsettings* e importá-lo para criar um perfil de publicação.

A outra função usa outro arquivo de modelo (site-Environment. template) para criar um arquivo *website-Environment. xml* que contém as configurações que o script de implantação usará junto com o arquivo *. pubxml* .

### <a name="troubleshooting-and-error-handling"></a>Solução de problemas e tratamento de erros

Os scripts são como programas: eles podem falhar e quando você deseja saber o máximo possível sobre a falha e o que o causou. Por esse motivo, o script de criação de ambiente altera o valor da variável `VerbosePreference` de `SilentlyContinue` para `Continue` para que todas as mensagens detalhadas sejam exibidas. Ele também altera o valor da variável `ErrorActionPreference` de `Continue` para `Stop`, para que o script pare mesmo quando encontrar erros de não encerramento:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Antes de qualquer trabalho, o script armazena a hora de início para que possa calcular o tempo decorrido quando terminar:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Depois de concluir seu trabalho, o script exibe o tempo decorrido:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

E para cada operação de chave, o script grava mensagens detalhadas, por exemplo:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Script de implantação

O que o script *New-AzureWebsiteEnv. ps1* faz para a criação do ambiente, o script *Publish-AzureWebsite. ps1* faz para a implantação do aplicativo.

O script de implantação Obtém o nome do aplicativo Web do arquivo *website-Environment. xml* criado pelo script de criação de ambiente.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Ele obtém a senha do usuário de implantação do arquivo *. publishsettings* :

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Ele executa o comando do [MSBuild](http://msbuildbook.com/) que cria e implanta o projeto:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

E se você tiver especificado o parâmetro `Launch` na linha de comando, ele chamará o cmdlet `Show-AzureWebsite` para abrir o navegador padrão para a URL do site.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

Você pode executar o script de implantação com um comando como este:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

E, quando terminar, o navegador será aberto com o site em execução na nuvem na URL do `<websitename>.azurewebsites.net`.

![Corrigir o aplicativo de ti implantado no Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>Resumo

Com esses scripts, você pode ter certeza de que as mesmas etapas sempre serão executadas na mesma ordem usando as mesmas opções. Isso ajuda a garantir que cada desenvolvedor da equipe não perca nada ou se preocupe ou implante algo personalizado em sua própria máquina que realmente não funcionará da mesma maneira no ambiente de outro membro da equipe ou em produção.

De forma semelhante, você pode automatizar a maioria das funções de gerenciamento do Azure que pode fazer no portal de gerenciamento, usando a API REST, scripts do Windows PowerShell, uma API de linguagem .NET ou um utilitário bash que pode ser executado no Linux ou Mac.

No [próximo capítulo](source-control.md) , veremos o código-fonte e explicamos por que é importante incluir seus scripts em seu repositório de código-fonte.

## <a name="resources"></a>Recursos

- [Instale e configure o Windows PowerShell para Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Explica como instalar os cmdlets Azure PowerShell e como instalar o certificado que você precisa em seu computador para gerenciar sua conta do Azure. Este é um ótimo lugar para começar porque ele também tem links para recursos para o aprendizado do próprio PowerShell.
- [Centro de script do Azure](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). Portal do WindowsAzure.com para recursos de desenvolvimento de scripts que gerenciam os serviços do Azure, com links para tutoriais de introdução, documentação de referência do cmdlet e código-fonte e scripts de exemplo
- [Script de fim de semana: introdução com o Azure e o PowerShell](https://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). Em um blog dedicado ao Windows PowerShell, esta postagem fornece uma ótima introdução ao uso do PowerShell para funções de gerenciamento do Azure.
- [Instale e configure a interface de linha de comando de plataforma cruzada do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Tutorial de introdução para uma estrutura de script do Azure que funciona em Mac e Linux, bem como em sistemas Windows.
- [Seção de ferramentas de linha de comando do tópico baixar SDKs e ferramentas do Azure](https://azure.microsoft.com/downloads/). Página do portal para documentação e downloads relacionados a ferramentas de linha de comando para o Azure.
- [Automatizando tudo com as bibliotecas de gerenciamento do Azure e o .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman apresenta a API de gerenciamento do .NET para o Azure.
- [Usando scripts do Windows PowerShell para publicar em ambientes de desenvolvimento e de teste](https://msdn.microsoft.com/library/azure/dn642480.aspx). Documentação do MSDN que explica como usar scripts de publicação que o Visual Studio gera automaticamente para projetos Web.
- [PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Extensão do Visual Studio que adiciona suporte a idiomas para o Windows PowerShell no Visual Studio.

> [!div class="step-by-step"]
> [Anterior](introduction.md)
> [Próximo](source-control.md)
