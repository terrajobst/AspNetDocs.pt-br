---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Executando scripts do Windows PowerShell de arquivos de projeto do MSBuild | Microsoft Docs
author: jrjlee
description: Este tópico descreve como executar um script do Windows PowerShell como parte de um processo de compilação e implantação. Você pode executar um script localmente (em outras palavras, na b...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: 7b09c07b8b7c2a61ca534f7a66a929593f3d04ca
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548506"
---
# <a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Execução de scripts do Windows PowerShell de arquivos de projeto do MSBuild

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como executar um script do Windows PowerShell como parte de um processo de compilação e implantação. Você pode executar um script localmente (em outras palavras, no servidor de compilação) ou remotamente, como em um servidor Web de destino ou servidor de banco de dados.
> 
> Há muitos motivos pelos quais você talvez queira executar um script pós-implantação do Windows PowerShell. Por exemplo, você pode querer:
> 
> - Adicione uma fonte de evento Personalizada ao registro.
> - Gere um diretório do sistema de arquivos para carregamentos.
> - Limpar diretórios de compilação.
> - Gravar entradas em um arquivo de log personalizado.
> - Enviar emails que convidam usuários a um aplicativo Web provisionado recentemente.
> - Crie contas de usuário com as permissões apropriadas.
> - Configure a replicação entre instâncias de SQL Server.
> 
> Este tópico mostrará como executar scripts do Windows PowerShell tanto localmente quanto remotamente de um destino personalizado em um arquivo de projeto do Microsoft Build Engine (MSBuild).

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no coração desses tutoriais baseia-se na abordagem de arquivo de projeto dividido descrita em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), no qual o processo de compilação é&#x2014;controlado por dois arquivos de projeto, um contendo instruções de Build que se aplicam a todos os ambientes de destino e um contendo configurações específicas de ambiente e de implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto independente do ambiente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Para executar um script do Windows PowerShell como parte de um processo de implantação automatizado ou de uma única etapa, você precisará concluir estas tarefas de alto nível:

- Adicione o script do Windows PowerShell à sua solução e ao controle do código-fonte.
- Crie um comando que invoque o script do Windows PowerShell.
- Escapar de qualquer caractere XML reservado em seu comando.
- Crie um destino em seu arquivo de projeto do MSBuild personalizado e use a tarefa **exec** para executar o comando.

Este tópico mostrará como executar esses procedimentos. As tarefas e orientações neste tópico pressupõem que você já esteja familiarizado com as propriedades e os destinos do MSBuild, e que você entende como usar um arquivo de projeto do MSBuild personalizado para conduzir um processo de compilação e implantação. Para obter mais informações, consulte [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Criando e adicionando scripts do Windows PowerShell

As tarefas neste tópico usam um script de exemplo do Windows PowerShell chamado **LogDeploy. ps1** para ilustrar como executar scripts do MSBuild. O script **LogDeploy. ps1** contém uma função simples que grava uma entrada de linha única em um arquivo de log:

[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.ps1)]

O script **LogDeploy. ps1** aceita dois parâmetros. O primeiro parâmetro representa o caminho completo para o arquivo de log ao qual você deseja adicionar uma entrada e o segundo parâmetro representa o destino de implantação que você deseja registrar no arquivo de log. Quando você executa o script, ele adiciona uma linha ao arquivo de log neste formato:

[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]

Para tornar o script **LogDeploy. ps1** disponível para o MSBuild, você precisa:

- Adicione o script ao controle do código-fonte.
- Adicione o script à sua solução no Visual Studio 2010.

Você não precisa implantar o script com o conteúdo da solução, independentemente se planeja executar o script no servidor de compilação ou em um computador remoto. Uma opção é adicionar o script a uma pasta de solução. No exemplo do Gerenciador de contatos, como você deseja usar o script do Windows PowerShell como parte do processo de implantação, faz sentido adicionar o script à pasta da solução de publicação.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

O conteúdo das pastas de solução é copiado para servidores de compilação como material de origem. No entanto, eles não formam nenhuma parte de qualquer saída de projeto.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Executando um script do Windows PowerShell no servidor de compilação

Em alguns cenários, talvez você queira executar scripts do Windows PowerShell no computador que cria seus projetos. Por exemplo, você pode usar um script do Windows PowerShell para limpar pastas de Build ou gravar entradas em um arquivo de log personalizado.

Em termos de sintaxe, executar um script do Windows PowerShell de um arquivo de projeto do MSBuild é o mesmo que executar um script do Windows PowerShell de um prompt de comando regular. Você precisa invocar o executável PowerShell. exe e usar a opção **– Command** para fornecer os comandos que deseja que o Windows PowerShell execute. (No Windows PowerShell v2, você também pode usar a opção **– File** ). O comando deve usar este formato:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]

Por exemplo:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]

Se o caminho para o script incluir espaços, você precisará colocar o caminho do arquivo entre aspas simples, precedida por um e comercial. Não é possível usar aspas duplas, pois você já as usou para colocar o comando:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]

Há algumas considerações adicionais quando você invoca esse comando do MSBuild. Primeiro, você deve incluir o sinalizador **– não interativo** para garantir que o script seja executado silenciosamente. Em seguida, você deve incluir o sinalizador **– ExecutionPolicy** com um valor de argumento apropriado. Isso especifica a política de execução que o Windows PowerShell aplicará ao seu script e permitirá que você substitua a política de execução padrão, o que pode impedir a execução do script. Você pode escolher um destes valores de argumento:

- Um valor **irrestrito** permitirá que o Windows PowerShell execute o script, independentemente de o script ser assinado ou não.
- Um valor de **RemoteSigned** permitirá que o Windows PowerShell execute scripts não assinados que foram criados no computador local. No entanto, os scripts que foram criados em outro lugar devem ser assinados. (Na prática, é muito improvável que você tenha criado um script do Windows PowerShell localmente em um servidor de compilação).
- Um valor de **AllSigned** permitirá que o Windows PowerShell execute somente scripts assinados.

A política de execução padrão é **restrita**, o que impede que o Windows PowerShell execute arquivos de script.

Por fim, você precisa escapar de qualquer caractere XML reservado que ocorra em seu comando do Windows PowerShell:

- Substitua aspas simples por **&amp;apos;**
- Substitua aspas duplas por **&amp;quot;**
- Substitua o e comercial por **&amp;amp;**

- Quando você fizer essas alterações, o comando será semelhante a este:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]

Em seu arquivo de projeto do MSBuild personalizado, você pode criar um novo destino e usar a tarefa **exec** para executar este comando:

[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]

Neste exemplo, observe que:

- Todas as variáveis, como valores de parâmetro e o local do executável do Windows PowerShell, são declaradas como propriedades do MSBuild.
- As condições são incluídas para permitir que os usuários substituam esses valores da linha de comando.
- A propriedade **MSDeployComputerName** é declarada em outro lugar no arquivo de projeto.

Quando você executar esse destino como parte do seu processo de compilação, o Windows PowerShell executará o comando e gravará uma entrada de log no arquivo especificado.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Executando um script do Windows PowerShell em um computador remoto

O Windows PowerShell é capaz de executar scripts em computadores remotos por meio de [gerenciamento remoto do Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). Para fazer isso, você precisa usar o cmdlet [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) . Isso permite executar o script em um ou mais computadores remotos sem copiar o script para os computadores remotos. Todos os resultados são retornados para o computador local do qual você executou o script.

> [!NOTE]
> Antes de usar o cmdlet **Invoke-Command** para executar scripts do Windows PowerShell em um computador remoto, você precisa configurar um ouvinte do WinRM para aceitar mensagens remotas. Você pode fazer isso executando o comando **winrm quickconfig** no computador remoto. Para obter mais informações, consulte [instalação e configuração para gerenciamento remoto do Windows](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).

Em uma janela do Windows PowerShell, você usaria essa sintaxe para executar o script **LogDeploy. ps1** em um computador remoto:

[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]

> [!NOTE]
> Há várias outras maneiras de usar o **Invoke-Command** para executar um arquivo de script, mas essa abordagem é a mais simples quando você precisa fornecer valores de parâmetro e gerenciar caminhos com espaços.

Ao executar isso em um prompt de comando, você precisa invocar o executável do Windows PowerShell e usar o parâmetro **– Command** para fornecer suas instruções:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]

Como antes, você precisa fornecer alguns comutadores adicionais e escapar de qualquer caractere XML reservado ao executar o comando do MSBuild:

[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]

Por fim, como antes, você pode usar a tarefa **exec** em um destino do MSBuild personalizado para executar o comando:

[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]

Quando você executar esse destino como parte do seu processo de compilação, o Windows PowerShell executará o script no computador especificado no argumento **– ComputerName** .

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como executar um script do Windows PowerShell de um arquivo de projeto do MSBuild. Você pode usar essa abordagem para executar um script do Windows PowerShell, seja localmente ou em um computador remoto, como parte de um processo de compilação e implantação automatizado ou de uma única etapa.

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre como assinar scripts do Windows PowerShell e gerenciar políticas de execução, consulte [executando scripts do Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx). Para obter orientação sobre como executar comandos do Windows PowerShell em um computador remoto, consulte [executando comandos remotos](https://technet.microsoft.com/library/dd819505.aspx).

Para obter mais informações sobre como usar arquivos de projeto do MSBuild personalizados para controlar o processo de implantação, consulte [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Anterior](taking-web-applications-offline-with-web-deploy.md)
> [Próximo](troubleshooting-the-packaging-process.md)
