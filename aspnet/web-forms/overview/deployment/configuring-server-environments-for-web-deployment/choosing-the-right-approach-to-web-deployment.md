---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Escolhendo a abordagem certa para a implantação na Web | Microsoft Docs
author: jrjlee
description: Quando você trabalha com a ferramenta de implantação da Web do Serviços de Informações da Internet (IIS) (Implantação da Web) 2,0 ou posterior, há três abordagens principais que você pode usar para obter...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13f784dd8e6404806104d56b026b3c41ca178892
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548478"
---
# <a name="choosing-the-right-approach-to-web-deployment"></a>Escolha da abordagem correta para a Implantação da Web

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Quando você trabalha com a ferramenta de implantação da Web do Serviços de Informações da Internet (IIS) (Implantação da Web) 2,0 ou posterior, há três abordagens principais que podem ser usadas para obter aplicativos Web empacotados em um servidor Web. Você pode:
> 
> - Implante o aplicativo de um local remoto direcionando o *serviço Web Deployment Agent* (também conhecido como "agente remoto") no servidor de destino.
> - Implante o aplicativo de um local remoto usando Implantação da Web sob demanda (também conhecido como "agente Temp").
> - Implante o aplicativo de um local remoto direcionando o *manipulador de implantação da Web do IIS* no servidor de destino.
> - Implante o aplicativo copiando manualmente o pacote da Web para o servidor de destino e importando-o por meio do Gerenciador do IIS.
> 
> A maneira como você configura os servidores Web de destino dependerá da abordagem da implantação que você deseja usar. Este tópico ajudará você a decidir qual abordagem de implantação é adequada para você.

Esta tabela mostra as principais vantagens e desvantagens de cada abordagem de implantação, juntamente com os cenários que geralmente atendem a cada abordagem.

| Abordagem | Vantagens | Desvantagens | Cenários comuns |
| --- | --- | --- | --- |
| Agente remoto | É fácil de configurar. Ele é adequado para atualizações regulares de conteúdo e aplicativos Web. | O usuário deve ser um administrador no servidor de destino. o usuário não pode fornecer credenciais alternativas. | Ambientes de desenvolvimento. Ambientes de teste. |
| Agente temporário | Não é necessário instalar Implantação da Web no computador de destino. A versão mais recente do Implantação da Web é usada automaticamente. | O usuário deve ser um administrador no servidor de destino. o usuário não pode fornecer credenciais alternativas. | Ambientes de desenvolvimento. Ambientes de teste. |
| Manipulador de Implantação da Web | Usuários não administradores podem implantar conteúdo. Ele é adequado para atualizações regulares de conteúdo e aplicativos Web. | É muito mais complexo configurar. | Ambientes de preparo. Ambientes de produção de intranet. Ambientes hospedados. |
| Implantação offline | É muito fácil de configurar. Ele é adequado para ambientes isolados. | O administrador do servidor deve copiar e importar manualmente o pacote da Web todas as vezes. | Ambientes de produção voltados para a Internet. Ambientes de rede isolados. |

## <a name="using-the-remote-agent"></a>Usando o agente remoto

Quando você instala Implantação da Web usando as configurações padrão em um servidor de destino, o serviço Web Deployment Agent (o "agente remoto") é instalado e iniciado automaticamente. Por padrão, o agente remoto expõe um ponto de extremidade HTTP neste endereço:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]

> [!NOTE]
> Você pode substituir [*Server*] pelo nome do computador do seu servidor Web, um endereço IP para o servidor Web ou um nome de host que seja resolvido para o servidor Web.

Os administradores de servidor podem implantar pacotes da Web de um local remoto, como um computador de desenvolvedor ou um servidor de compilação, especificando esse endereço de ponto de extremidade. Por exemplo, suponha que Matt hink na Fabrikam, Inc. criou o projeto de aplicativo Web ContactManager. Mvc em seu computador de desenvolvedor. O processo de compilação gera um pacote da Web, junto com um arquivo *. Deploy. cmd* que contém os comandos de implantação da Web necessários para instalar o pacote. Se Matt for um administrador de servidor no servidor TESTWEB1, ele poderá implantar o aplicativo Web no servidor Web de teste executando este comando em seu computador de desenvolvedor:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]

No fato real, o Implantação da Web executável pode inferir o endereço do ponto de extremidade do agente remoto se você fornecer o nome do computador; portanto, Matt precisa apenas digitar:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]

> [!NOTE]
> Para obter mais informações sobre Implantação da Web sintaxe de linha de comando e arquivos *. Deploy. cmd* , consulte [como: instalar um pacote de implantação usando o arquivo Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

O agente remoto oferece uma maneira simples de implantar o conteúdo de um local remoto, e essa abordagem pode funcionar bem com uma implantação automatizada ou de um clique. No entanto, o usuário que executa o comando de implantação também deve ser um administrador de domínio ou um membro do grupo local de administradores no servidor de destino. Além disso, o agente remoto não dá suporte à autenticação básica, portanto, não é possível passar credenciais alternativas na linha de comando.

O agente remoto fornece uma abordagem útil para a implantação em cenários de desenvolvimento ou teste, em que não é incomum que os desenvolvedores tenham controle total de administrador sobre um ambiente de servidor de teste, e os aplicativos são normalmente recriados e reimplantados perguntas. No entanto, essa abordagem é geralmente menos aceitável para ambientes de preparo ou produção.

Para obter um exemplo de ponta a ponta de um cenário que usa a abordagem do agente remoto, consulte [cenário: Configurando um ambiente de teste para implantação da Web](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Usando o agente temporário

A abordagem do agente temporário para a implantação é semelhante à abordagem do agente remoto. No entanto, ao contrário da abordagem do agente remoto, você não precisa instalar Implantação da Web no servidor Web de destino. Em vez disso, ao executar a implantação, o Implantação da Web instalará uma versão temporária do serviço do agente de implantação da Web no servidor de destino e usará isso para implantar seu conteúdo no IIS. Quando a implantação for concluída, todos os arquivos temporários serão removidos.

Se você quiser usar a configuração do provedor de agente temporário, adicione o sinalizador **/g** ao comando de implantação:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]

> [!NOTE]
> Você não poderá usar o agente temporário se o serviço do agente de implantação da Web estiver instalado no computador de destino, mesmo que o serviço não esteja em execução.

A vantagem dessa abordagem é que você não precisa manter instalações do Implantação da Web nos servidores de destino. Além disso, você não precisa garantir que os computadores de origem e de destino estejam executando a mesma versão do Implantação da Web. No entanto, essa abordagem tem as mesmas limitações principais que a abordagem do agente remoto, ou seja, você deve ser um administrador local no servidor de destino para implantar o conteúdo e somente a autenticação NTLM tem suporte. A abordagem do Agente temp também requer muito mais configuração inicial do ambiente de destino.

Para obter mais informações sobre como usar o agente temporário, consulte [como instalar um pacote de implantação usando o arquivo Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx) e [implantação da Web sob demanda](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Usando o manipulador de Implantação da Web

Para o IIS 7 em diante, o Implantação da Web oferece uma abordagem de implantação alternativa por meio do manipulador de Implantação da Web do IIS. O manipulador de Implantação da Web está fortemente integrado ao serviço de gerenciamento da Web do IIS (WMSvc), que é projetado para permitir que os usuários gerenciem sites do IIS de locais remotos.

Por padrão, o agente remoto expõe um ponto de extremidade HTTP neste endereço:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]

> [!NOTE]
> Você pode substituir [*Server*] pelo nome do computador do seu servidor Web, um endereço IP para o servidor Web ou um nome de host que seja resolvido para o servidor Web.

A grande vantagem do manipulador de Implantação da Web sobre o agente remoto e o agente temporário é que você pode configurar o IIS para permitir que usuários não administradores implantem aplicativos e conteúdo em sites específicos do IIS. O manipulador de Implantação da Web também dá suporte à autenticação básica, para que você possa fornecer credenciais alternativas como parâmetros em seus comandos de Implantação da Web. A grande desvantagem é que o manipulador de Implantação da Web é inicialmente muito mais complicado para configurar e configurar.

No caso de usuários não administradores, o serviço de gerenciamento da Web (WMSvc) permitirá que o usuário se conecte ao IIS usando uma conexão no nível do site, em vez de uma conexão no nível do servidor. Para acessar um site específico, você pode incluir uma cadeia de caracteres de consulta específica do site no endereço do ponto de extremidade:

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]

Por exemplo, suponha que um processo de compilação esteja configurado para implantar automaticamente um aplicativo Web em um ambiente de preparo após cada compilação bem-sucedida. Se você usou a abordagem do agente remoto, precisaria tornar o processo de compilação a identidade de um administrador nos servidores de destino. Por outro lado, usando a abordagem do manipulador de implantação da Web, você pode dar a&#x2014;um usuário não administrador&#x2014;**FABRIKAM\stagingdeployer** nesse caso a permissão para apenas um site do IIS específico e o processo de compilação pode fornecer essas credenciais para implantar o pacote da Web.

[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]

> [!NOTE]
> Para obter mais informações sobre as operações de linha de comando e a sintaxe do Implantação da Web, consulte [implantação da Web referência de linha de comando](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Para obter mais informações sobre como usar o arquivo *. Deploy. cmd* , consulte [como instalar um pacote de implantação usando o arquivo Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

O manipulador de Implantação da Web fornece uma abordagem útil para implantação em ambientes de preparo, ambientes hospedados e ambientes de produção baseados em intranet, onde o acesso remoto ao servidor está disponível, mas as credenciais de administrador não são.

Para obter um exemplo de ponta a ponta de um cenário que usa a abordagem do manipulador de Implantação da Web, consulte [cenário: Configurando um ambiente de preparo para implantação da Web](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Usando a implantação offline

Em alguns casos, não é possível ou prático implantar aplicativos e conteúdo em um site do IIS a partir de um local remoto. Por exemplo, os computadores de origem e de destino podem estar em redes isoladas ou segmentos de rede, ou a política de firewall pode não permitir o acesso remoto.

Em cenários como esses, você ainda pode usar os recursos de empacotamento e publicação do Implantação da Web; Você simplesmente não pode usá-los de um local remoto. Em vez disso, um administrador no servidor de destino deve copiar o pacote da Web no servidor e importá-lo por meio do Gerenciador do IIS.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

A abordagem de implantação offline é normalmente útil em ambientes de produção voltados para a Internet, em que os servidores em uma rede de perímetro podem ter a conectividade restrita com computadores na rede interna.

Para obter um exemplo de ponta a ponta de um cenário que usa a abordagem de implantação offline, consulte [cenário: Configurando um ambiente de produção para implantação da Web](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre as operações de linha de comando e a sintaxe do Implantação da Web, consulte [implantação da Web referência de linha de comando](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Para obter mais informações sobre como usar o arquivo *. Deploy. cmd* , consulte [como instalar um pacote de implantação usando o arquivo Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx).

Para obter diretrizes mais gerais sobre as diferentes maneiras em que você pode implantar pacotes da Web de um computador remoto, consulte [usando o implantação da Web remotamente](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Para obter mais informações sobre como usar Implantação da Web sob demanda, consulte [implantação da Web sob demanda](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Anterior](configuring-server-environments-for-web-deployment.md)
> [Próximo](scenario-configuring-a-test-environment-for-web-deployment.md)
