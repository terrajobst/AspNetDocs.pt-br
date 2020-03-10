---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
title: 'Gerenciamento do ciclo de vida do aplicativo: de desenvolvimento para produção | Microsoft Docs'
author: jrjlee
description: Este tópico ilustra como uma empresa fictícia gerencia a implantação de um aplicativo Web ASP.NET por meio de ambientes de teste, de preparo e de produção como par...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f97a1145-6470-4bca-8f15-ccfb25fb903c
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/application-lifecycle-management-from-development-to-production
msc.type: authoredcontent
ms.openlocfilehash: 230cf4393db0ee19cfc42ed54359d61e7926a49d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640661"
---
# <a name="application-lifecycle-management-from-development-to-production"></a>Gerenciamento do ciclo de vida do aplicativo: de desenvolvimento para produção

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico ilustra como uma empresa fictícia gerencia a implantação de um aplicativo Web ASP.NET por meio de ambientes de teste, de preparo e de produção como parte de um processo de desenvolvimento contínuo. Em todo o tópico, são fornecidos links para mais informações e orientações sobre como executar tarefas específicas.
> 
> O tópico foi projetado para fornecer uma visão geral de alto nível para uma [série de tutoriais](deploying-web-applications-in-enterprise-scenarios.md) sobre implantação na Web na empresa. Não se preocupe se você não estiver familiarizado com alguns dos conceitos descritos aqui&#x2014;os tutoriais a seguir fornecem informações detalhadas sobre todas essas tarefas e técnicas.
> 
> > [!NOTE]
> > Para simplificar, este tópico não aborda a atualização de bancos de dados como parte do processo de implantação. No entanto, fazer atualizações incrementais em recursos de bancos de dados é um requisito de muitos cenários de implantação empresarial, e você pode encontrar orientações sobre como fazer isso posteriormente nesta série de tutoriais. Para obter mais informações, consulte [Implantando projetos de banco de dados](../web-deployment-in-the-enterprise/deploying-database-projects.md).

## <a name="overview"></a>Visão geral

O processo de implantação ilustrado aqui se baseia no cenário de implantação Fabrikam, Inc. descrito em [Enterprise Web Deployment: visão geral do cenário](enterprise-web-deployment-scenario-overview.md). Você deve ler a visão geral do cenário antes de estudar este tópico. Essencialmente, o cenário examina como uma organização gerencia a implantação de um aplicativo Web razoavelmente complexo, a [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), por meio de várias fases em um ambiente corporativo típico.

Em um alto nível, a solução Contact Manager passa por esses estágios como parte do processo de desenvolvimento e implantação:

1. Um desenvolvedor verifica algum código em Team Foundation Server (TFS) 2010.
2. O TFS cria o código e executa qualquer teste de unidade associado ao projeto de equipe.
3. O TFS implanta a solução no ambiente de teste.
4. A equipe de desenvolvedores verifica e valida a solução no ambiente de teste.
5. O administrador do ambiente de preparo executa uma implantação "e se" no ambiente de preparo, para estabelecer se a implantação causará problemas.
6. O administrador do ambiente de preparo executa uma implantação ao vivo no ambiente de preparo.
7. A solução passa por testes de aceitação do usuário no ambiente de preparo.
8. Os pacotes de implantação da Web são importados manualmente para o ambiente de produção.

Esses estágios formam parte de um ciclo de desenvolvimento contínuo.

![](application-lifecycle-management-from-development-to-production/_static/image1.png)

Na prática, o processo é um pouco mais complicado do que isso, como você verá quando examinamos cada estágio em mais detalhes. A Fabrikam, Inc. usa uma abordagem diferente para a implantação de cada ambiente de destino.

![](application-lifecycle-management-from-development-to-production/_static/image2.png)

O restante deste tópico examina estes estágios-chave deste ciclo de vida de implantação:

- **Pré-requisitos**: como você precisa configurar sua infraestrutura de servidor antes de colocar a lógica de implantação em vigor.
- **Desenvolvimento e implantação iniciais**: o que você precisa fazer antes de implantar sua solução pela primeira vez.
- **Implantação para teste**: como empacotar e implantar conteúdo em um ambiente de teste automaticamente quando um desenvolvedor faz o check-in de um novo código.
- **Implantação para preparo**: como implantar compilações específicas em um ambiente de preparo e como executar implantações "e se" para garantir que uma implantação não causará problemas.
- **Implantação para produção**: como importar pacotes da Web para um ambiente de produção quando a infraestrutura de rede impede a implantação remota.

## <a name="prerequisites"></a>Prerequisites

A primeira tarefa em qualquer cenário de implantação é garantir que sua infraestrutura de servidor atenda aos requisitos das suas ferramentas e técnicas de implantação. Nesse caso, a Fabrikam, Inc. configurou sua infraestrutura de servidor da seguinte maneira:

- O TFS está configurado para incluir uma coleção de projetos de equipe, controladores de compilação e agentes de compilação. Consulte [Configurando Team Foundation Server para implantação automatizada da Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md) para obter mais informações.
- O ambiente de teste está configurado para aceitar implantações remotas usando o serviço de Deployment Agent da Web (o "agente remoto"), conforme descrito em [cenário: Configurando um ambiente de teste para implantação da Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-test-environment-for-web-deployment.md) e [configurar um servidor Web para implantação da Web publicação (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md).
- O ambiente de preparo está configurado para aceitar implantações remotas usando o ponto de extremidade do manipulador de Implantação da Web, conforme descrito em [cenário: Configurando um ambiente de preparo para implantação da Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-staging-environment-for-web-deployment.md) e [configurar um servidor Web para implantação da Web publicação (manipulador de implantação da Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md).
- O ambiente de produção é configurado para permitir que um administrador importe pacotes de implantação da Web manualmente no Serviços de Informações da Internet (IIS), conforme descrito em [cenário: Configurando um ambiente de produção para implantação da Web](../configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment.md) e [configurar um servidor Web para implantação da Web publicação (implantação offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

## <a name="initial-development-and-deployment"></a>Desenvolvimento e implantação iniciais

Antes que a equipe de desenvolvimento da Fabrikam, Inc. possa implantar a solução Contact Manager pela primeira vez, ela precisa executar estas tarefas:

- Crie um novo projeto de equipe no TFS.
- Crie os arquivos de projeto do Microsoft Build Engine (MSBuild) que contêm a lógica de implantação.
- Crie as definições de compilação do TFS que disparam os processos de implantação.

### <a name="create-a-new-team-project"></a>Criar um novo projeto de equipe

- O administrador do TFS, Rob Fábio, cria um novo projeto de equipe para o aplicativo, conforme descrito em [criando um projeto de equipe no TFS](../configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs.md). Em seguida, o desenvolvedor líder, Matt hink, cria uma solução de esqueleto. Ele verifica seus arquivos no novo projeto de equipe no TFS, conforme descrito em [adicionando conteúdo ao controle do código-fonte](../configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control.md).

### <a name="create-the-deployment-logic"></a>Criar a lógica de implantação

Matt hink cria vários arquivos de projeto do MSBuild personalizados, usando a abordagem de arquivo de projeto dividido descrita em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Matt cria:

- Um arquivo de projeto chamado *Publish. proj* que executa o processo de implantação. Esse arquivo contém destinos do MSBuild que criam os projetos na solução, criam pacotes da Web e implantam os pacotes em um ambiente de servidor de destino.
- Arquivos de projeto específicos do ambiente denominado *env-dev. proj* e *env-Stage. proj*. Elas contêm configurações específicas para o ambiente de teste e o ambiente de preparo, respectivamente, como cadeias de conexão, pontos de extremidade de serviço e os detalhes do serviço remoto que receberá o pacote da Web. Para obter orientação sobre como escolher as configurações corretas para ambientes de destino específicos, consulte [Configurar Propriedades de implantação para um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Para executar a implantação, um usuário executa o arquivo *Publish. proj* usando o MSBuild ou o Team Build e especifica o local do arquivo de projeto específico do ambiente relevante (*env-dev. proj* ou *env-Stage. proj*) como um argumento de linha de comando. Em seguida, o arquivo *Publish. proj* importa o arquivo de projeto específico do ambiente para criar um conjunto completo de instruções de publicação para cada ambiente de destino.

> [!NOTE]
> A maneira como esses arquivos de projeto personalizados funcionam é independente do mecanismo que você usa para invocar o MSBuild. Por exemplo, você pode usar a linha de comando do MSBuild diretamente, conforme descrito em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Você pode executar os arquivos de projeto de um arquivo de comando, conforme descrito em [criar e executar um arquivo de comando de implantação](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md). Como alternativa, você pode executar os arquivos de projeto de uma definição de compilação no TFS, conforme descrito em [criando uma definição de compilação que dá suporte à implantação](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).  
> Em cada caso, o resultado final é o&#x2014;mesmo que o MSBuild executa o arquivo de projeto mesclado e implanta sua solução no ambiente de destino. Isso proporciona uma grande flexibilidade na forma como você dispara o processo de publicação.

Depois de criar os arquivos de projeto personalizados, Matt os adiciona a uma pasta de solução e os verifica no controle do código-fonte.

### <a name="create-build-definitions"></a>Criar definições de compilação

Como uma tarefa de preparação final, Matt e Rob funcionam em conjunto para criar três definições de compilação para o novo projeto de equipe:

- **DeployToTest**. Isso cria a solução Contact Manager e a implanta no ambiente de teste sempre que um check-in ocorre.
- **DeployToStaging**. Isso implanta recursos de uma compilação anterior especificada para o ambiente de preparo quando um desenvolvedor enfileira a compilação.
- **DeployToStaging-WhatIf**. Isso executa uma implantação "e se" no ambiente de preparo quando um desenvolvedor enfileira a compilação.

As seções a seguir fornecem mais detalhes sobre cada uma dessas definições de compilação.

## <a name="deployment-to-test"></a>Implantação para teste

A equipe de desenvolvimento da Fabrikam, Inc. mantém ambientes de teste para conduzir uma variedade de atividades de teste de software, como verificação e validação, testes de usabilidade, testes de compatibilidade e testes ad hoc ou exploratório.

A equipe de desenvolvimento criou uma definição de compilação no TFS chamada **DeployToTest**. Essa definição de compilação usa um gatilho de integração contínua, o que significa que o processo de compilação é executado toda vez que um membro da equipe de desenvolvimento da Fabrikam, Inc. executa um check-in. Quando uma compilação é disparada, a definição de compilação:

- Compile a solução ContactManager. sln. Isso, por sua vez, compila todos os projetos dentro da solução.
- Executar qualquer teste de unidade na estrutura de pastas da solução (se a solução for compilada com êxito).
- Execute os arquivos de projeto personalizados que controlam o processo de implantação (se a solução for compilada com êxito e passar por qualquer teste de unidade).

O resultado final é que, se a solução for compilada com êxito e passar testes de unidade, os pacotes da Web e quaisquer outros recursos de implantação serão implantados no ambiente de teste.

![](application-lifecycle-management-from-development-to-production/_static/image3.png)

### <a name="how-does-the-deployment-process-work"></a>Como funciona o processo de implantação?

A definição de compilação **DeployToTest** fornece esses argumentos ao MSBuild:

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample1.cmd)]

As propriedades de pacote **DeployOnBuild = true** e **DeployTarget =** são usadas quando o Team Build cria os projetos dentro da solução. Quando o projeto é um projeto de aplicativo Web, essas propriedades instruem o MSBuild a criar um pacote de implantação da Web para o projeto. A propriedade **TargetEnvPropsFile** informa ao arquivo *Publish. proj* onde encontrar o arquivo de projeto específico do ambiente a ser importado.

> [!NOTE]
> Para obter instruções detalhadas sobre como criar uma definição de compilação como esta, consulte [criando uma definição de compilação que dá suporte à implantação](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

O arquivo *Publish. proj* contém destinos que criam cada projeto na solução. No entanto, ele também inclui a lógica condicional que ignora esses destinos de compilação se você estiver executando o arquivo no Team Build. Isso permite que você aproveite a funcionalidade de compilação adicional que o Team Build oferece, como a capacidade de executar testes de unidade. Se a compilação da solução ou os testes de unidade falharem, o arquivo *Publish. proj* não será executado e o aplicativo não será implantado.

A lógica condicional é realizada avaliando a propriedade **BuildingInTeamBuild** . Essa é uma propriedade do MSBuild que é definida automaticamente como **true** quando você usa o Team Build para compilar seus projetos.

## <a name="deployment-to-staging"></a>Implantação para preparo

Quando uma compilação atende a todos os requisitos da equipe de desenvolvedores no ambiente de teste, a equipe pode querer implantar a mesma compilação em um ambiente de preparo. Ambientes de preparo normalmente são configurados para corresponder às características do ambiente de produção ou "ao vivo" o mais próximo possível, por exemplo, em termos de especificações de servidor, sistemas operacionais e software e configuração de rede. Ambientes de preparo geralmente são usados para testes de carga, testes de aceitação do usuário e análises internas mais amplas. As compilações são implantadas no ambiente de preparo diretamente do servidor de compilação.

![](application-lifecycle-management-from-development-to-production/_static/image4.png)

As definições de compilação usadas para implantar a solução no ambiente de preparo, **DeployToStaging-WhatIf** e **DeployToStaging**, compartilham essas características:

- Na verdade, eles não criam nada. Quando Rob implanta a solução no ambiente de preparo, ele deseja implantar uma compilação específica existente que já foi verificada e validada no ambiente de teste. As definições de Build precisam apenas executar os arquivos de projeto personalizados que controlam o processo de implantação.
- Quando Rob dispara uma compilação, ele usa os parâmetros de compilação para especificar qual compilação contém os recursos que deseja implantar do servidor de compilação.
- As definições de compilação não são disparadas automaticamente. Rob enfileira manualmente uma compilação quando deseja implantar a solução no ambiente de preparo.

Este é o processo de alto nível para uma implantação no ambiente de preparo:

1. O administrador do ambiente de preparo, Rob Fábio, enfileira uma compilação usando a definição de compilação **DeployToStaging-WhatIf** . Rob usa os parâmetros de definição de compilação para especificar qual compilação deseja implantar.
2. A definição de compilação **DeployToStaging-WhatIf** executa os arquivos de projeto personalizados no modo "e se". Isso gera arquivos de log como se Rob estivesse executando uma implantação dinâmica, mas na verdade ele não faz nenhuma alteração no ambiente de destino.
3. Rob revisa os arquivos de log para verificar os efeitos da implantação no ambiente de preparo. Em particular, Rob quer verificar o que será adicionado, o que será atualizado e o que será excluído.
4. Se Rob for convencido de que a implantação não fará nenhuma alteração indesejável em recursos ou dados existentes, ele colocará uma compilação em fila usando a definição de compilação **DeployToStaging** .
5. A definição de compilação **DeployToStaging** executa os arquivos de projeto personalizados. Eles publicam os recursos de implantação no servidor Web primário no ambiente de preparo.
6. O controlador Web farm Framework (WFF) sincroniza os servidores Web no ambiente de preparo. Isso torna o aplicativo disponível em todos os servidores Web no farm de servidores.

### <a name="how-does-the-deployment-process-work"></a>Como funciona o processo de implantação?

A definição de compilação **DeployToStaging** fornece esses argumentos ao MSBuild:

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample2.cmd)]

A propriedade **TargetEnvPropsFile** informa ao arquivo *Publish. proj* onde encontrar o arquivo de projeto específico do ambiente a ser importado. A propriedade **OutputRoot** substitui o valor interno e indica o local da pasta de compilação que contém os recursos que você deseja implantar. Quando Rob enfileira a compilação, ele usa a guia **parâmetros** para fornecer um valor atualizado para a propriedade **OutputRoot** .

![](application-lifecycle-management-from-development-to-production/_static/image5.png)

> [!NOTE]
> Para obter mais informações sobre como criar uma definição de compilação como esta, consulte [implantar uma compilação específica](../configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build.md).

A definição de compilação **DeployToStaging-WhatIf** contém a mesma lógica de implantação que a definição de compilação **DeployToStaging** . No entanto, ele inclui o argumento adicional **WhatIf = true**:

[!code-console[Main](application-lifecycle-management-from-development-to-production/samples/sample3.cmd)]

No arquivo *Publish. proj* , a propriedade **WhatIf** indica que todos os recursos de implantação devem ser publicados no modo "e se". Em outras palavras, os arquivos de log são gerados como se a implantação tivesse prosseguido, mas nada é realmente alterado no ambiente de destino. Isso permite que você avalie o impacto de uma implantação&#x2014;proposta em particular, o que será adicionado, o que será atualizado e o que será excluído&#x2014;antes de realmente fazer qualquer alteração.

> [!NOTE]
> Para obter mais informações sobre como configurar implantações "e se", consulte [executando uma implantação de "What If"](../advanced-enterprise-web-deployment/performing-a-what-if-deployment.md).

Depois de implantar seu aplicativo no servidor Web primário no ambiente de preparo, o WFF sincronizará automaticamente o aplicativo em todos os servidores no farm de servidores.

> [!NOTE]
> Para obter mais informações sobre como configurar o WFF para sincronizar servidores Web, consulte [criar um farm de servidores com a estrutura do Web farm](../configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework.md).

## <a name="deployment-to-production"></a>Implantação para produção

Quando um Build foi aprovado no ambiente de preparo, a equipe da Fabrikam, Inc. pode publicar o aplicativo no ambiente de produção. O ambiente de produção é onde o aplicativo fica "ao vivo" e atinge seu público-alvo de usuários finais.

O ambiente de produção está em uma rede de perímetro voltada para a Internet. Isso é isolado da rede interna que contém o servidor de compilação. O administrador do ambiente de produção, Lisa Andrews, deve copiar manualmente os pacotes de implantação da Web do servidor de compilação e importá-los para o IIS no servidor Web de produção primário.

![](application-lifecycle-management-from-development-to-production/_static/image6.png)

Este é o processo de alto nível para uma implantação no ambiente de produção:

1. A equipe de desenvolvedores aconselha Lisa que uma compilação está pronta para implantação na produção. A equipe aconselha a Lisa sobre o local dos pacotes de implantação na Web dentro da pasta de destino no servidor de compilação.
2. Lisa coleta os pacotes da Web do servidor de compilação e os copia para o servidor Web primário no ambiente de produção.
3. Lisa usa o Gerenciador do IIS para importar e publicar os pacotes da Web no servidor Web primário.
4. O controlador WFF sincroniza os servidores Web no ambiente de produção. Isso torna o aplicativo disponível em todos os servidores Web no farm de servidores.

### <a name="how-does-the-deployment-process-work"></a>Como funciona o processo de implantação?

O Gerenciador do IIS inclui um assistente para importar pacote de aplicativos que facilita a publicação de pacotes da Web em um site do IIS. Para obter instruções sobre como executar esse procedimento, consulte [Instalando manualmente pacotes da Web](../web-deployment-in-the-enterprise/manually-installing-web-packages.md).

## <a name="conclusion"></a>Conclusão

Este tópico forneceu uma ilustração do ciclo de vida da implantação para um aplicativo Web típico de escala empresarial.

Este tópico forma parte de uma série de tutoriais que fornecem orientação sobre vários aspectos da implantação de aplicativos Web. Na prática, há muitas tarefas e considerações adicionais em cada estágio do processo de implantação, e não é possível cobri-las em um único passo a passos. Para obter mais informações, consulte estes tutoriais:

- [Implantação da Web na empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Este tutorial fornece uma introdução abrangente às técnicas de implantação da Web usando o MSBuild e a ferramenta de implantação da Web do IIS (Implantação da Web).
- [Configurando ambientes de servidor para implantação na Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md). Este tutorial fornece orientação sobre como configurar ambientes do Windows Server para dar suporte a vários cenários de implantação.
- [Configurando Team Foundation Server para implantação automatizada da Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Este tutorial fornece orientação sobre como integrar a lógica de implantação nos processos de compilação do TFS.
- [Implantação da Web empresarial avançada](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md). Este tutorial fornece orientação sobre como atender a alguns dos desafios de implantação mais complexos que as organizações enfrentam.

> [!div class="step-by-step"]
> [Anterior](enterprise-web-deployment-scenario-overview.md)
