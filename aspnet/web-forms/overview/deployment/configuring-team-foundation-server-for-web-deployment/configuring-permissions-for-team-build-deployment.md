---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
title: Configurando permissões para implantação do Team Build | Microsoft Docs
author: jrjlee
description: Este tópico descreve como configurar permissões para permitir que o servidor de compilação implante conteúdo em servidores Web e servidores de banco de dados como parte de um b automatizado...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 2488a91e-b0a8-465a-b874-3233f724b56b
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-permissions-for-team-build-deployment
msc.type: authoredcontent
ms.openlocfilehash: 5699f72af6b8d7f18d1a2c631dfdedd63c66e1e6
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638421"
---
# <a name="configuring-permissions-for-team-build-deployment"></a>Configuração de permissões para a implantação do Team Build

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como configurar permissões para permitir que o servidor de compilação implante conteúdo em servidores Web e servidores de banco de dados como parte de um processo de compilação automatizado.

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no coração desses tutoriais baseia-se na abordagem de arquivo de projeto dividido descrita em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), no qual o processo de compilação é&#x2014;controlado por dois arquivos de projeto, um contendo instruções de Build que se aplicam a todos os ambientes de destino e um contendo configurações específicas de ambiente e de implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto independente do ambiente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Ao instalar o serviço de compilação Team Foundation Server (TFS) 2010, você especifica a identidade com a qual deseja que o serviço seja executado. Por padrão, essa é a conta de serviço de rede. Como alternativa, você pode configurar o serviço de compilação para ser executado usando uma conta de domínio.

Qualquer tarefa de implantação que exija autenticação do Windows e que você planeja automatizar usando o Team Build, será executada usando a identidade do serviço de compilação. Assim, você precisará conceder à identidade do serviço de compilação todas as permissões necessárias nos servidores Web e nos servidores de banco de dados.

> [!NOTE]
> A conta de serviço de rede usa a conta da máquina para se autenticar em outros computadores. As contas de computador usam o formato * [nome do domínio]\[nome do computador] * **$** &#x2014;por exemplo, **FABRIKAM\TFSBUILD $** . Dessa forma, se o serviço de compilação for executado usando a identidade do serviço de rede, você deverá conceder as permissões necessárias à identidade da conta do computador para o servidor de compilação.

## <a name="configuring-web-server-permissions"></a>Configurando permissões do servidor Web

Conforme descrito em [escolhendo a abordagem certa para a implantação na Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md), há duas abordagens principais que você pode usar se quiser implantar pacotes da Web em um servidor Web remoto:

- Implante o aplicativo de um local remoto direcionando o *serviço Web Deployment Agent* (também conhecido como agente remoto) no servidor de destino.
- Implante o aplicativo de um local remoto direcionando o manipulador de Implantação da Web *serviços de informações da Internet* (*IIS)* no servidor de destino.

O agente remoto tem duas limitações principais nesse caso:

- O agente remoto dá suporte apenas à autenticação NTLM. Em outras palavras, a implantação deve usar a identidade&#x2014;do serviço de compilação que você não pode representar outra conta.
- Para usar o agente remoto, a conta que executa a implantação deve ser um administrador no servidor de destino.

Juntas, essas duas limitações tornam a abordagem do agente remoto indesejável para uma implantação automatizada do Team Build. Para usar essa abordagem, você precisaria tornar a conta de serviço de compilação um administrador em qualquer servidor Web de destino.

Por outro lado, a abordagem do manipulador de Implantação da Web oferece várias vantagens:

- O manipulador de Implantação da Web dá suporte à autenticação básica por HTTPS, que permite que você passe as credenciais de uma conta alternativa para a ferramenta de implantação da Web do IIS (Implantação da Web).
- Você pode configurar servidores Web de destino para permitir que usuários não administradores implantem conteúdo em sites do IIS específicos usando o manipulador de Implantação da Web.

Como resultado, é claramente preferível direcionar o manipulador de Implantação da Web ao automatizar a implantação de pacote da Web do Team Build. Este é o processo recomendado:

1. Crie uma conta de domínio com poucos privilégios para usar na implantação.
2. Configure o manipulador de Implantação da Web e conceda à conta as permissões necessárias para implantar o conteúdo em um site do IIS específico, conforme descrito em [Configurando um servidor Web para implantação da Web publicação (manipulador de implantação da Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
3. Invoque Implantação da Web e direcione o manipulador de Implantação da Web, usando a autenticação básica e fornecendo as credenciais da conta de domínio que você criou, para executar a implantação.

Na solução de exemplo do [Gerenciador de contatos](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , você especifica o tipo de autenticação (básico ou NTLM), as credenciais de implantação da Web e o endereço do ponto de extremidade (agente remoto ou manipulador de implantação da Web) no arquivo de projeto específico do ambiente. Esses valores são usados para formular e executar um comando Implantação da Web quando o arquivo de projeto é executado. Para obter mais informações, consulte [implantando pacotes da Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Para obter mais informações sobre como configurar o manipulador de Implantação da Web, incluindo como configurar permissões, consulte [Configurando um servidor Web para implantação da Web publicação (manipulador de implantação da Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md). Para obter mais informações sobre como configurar o agente remoto, consulte [Configurando um servidor Web para implantação da Web publicação (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).

## <a name="configuring-database-server-permissions"></a>Configurar permissões do servidor de banco de dados

Para implantar um banco de dados no SQL Server, você deve:

- Crie um logon para a conta de implantação na instância de SQL Server.
- Conceda ao logon permissões **dbcreator** na instância de SQL Server.
- Após a implantação inicial, adicione o logon ao **bd\_** função de proprietário no banco de dados de destino. Isso é necessário porque, em implantações subsequentes, você está modificando um banco de dados existente em vez de criar um novo banco de dados.

Você pode autenticar em uma instância do SQL Server usando a autenticação NTLM ou a autenticação do SQL Server:

- Se você usar a autenticação NTLM, precisará conceder as permissões descritas acima para a conta de serviço de compilação.
- Se você usar a autenticação SQL Server, precisará conceder as permissões descritas acima para a conta de SQL Server. Você também precisa incluir o SQL Server nome de usuário e a senha na cadeia de conexão usada para implantar o banco de dados.

Para obter detalhes passo a passo sobre como configurar permissões para implantação de banco de dados, consulte [Configurando um servidor de banco de dados para implantação da Web publicação](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

## <a name="conclusion"></a>Conclusão

Neste ponto, você deve entender as permissões necessárias, junto com as opções de autenticação abertas para você, ao automatizar implantações de banco de dados e aplicativo Web do Team Build. Você também deve ser capaz de implementar as permissões necessárias nos servidores Web do IIS e nos servidores de banco de dados do SQL Server.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como configurar ambientes do Windows Server para dar suporte à implantação remota, consulte [Configurando ambientes de servidor para implantação da Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

> [!div class="step-by-step"]
> [Anterior](deploying-a-specific-build.md)
