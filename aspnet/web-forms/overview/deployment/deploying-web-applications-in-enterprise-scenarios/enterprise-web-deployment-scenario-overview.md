---
uid: web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
title: 'Enterprise Web Deployment: visão geral do cenário | Microsoft Docs'
author: jrjlee
description: Esse conjunto de tutoriais usa uma solução de exemplo com um nível realista de complexidade, junto com um cenário de implantação empresarial fictício, para fornecer uma referência...
ms.author: riande
ms.date: 05/03/2012
ms.assetid: aa862153-4cd8-4e33-beeb-abf502c6664f
msc.legacyurl: /web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview
msc.type: authoredcontent
ms.openlocfilehash: 9786879844da13c21e6a953b1ab24b29ca8121e2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574126"
---
# <a name="enterprise-web-deployment-scenario-overview"></a>Enterprise Web Deployment: visão geral do cenário

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Esse conjunto de tutoriais usa uma solução de exemplo com um nível realista de complexidade, junto com um cenário de implantação empresarial fictício, para fornecer uma implementação de referência e dar às tarefas e passo a passos um contexto comum. Este tópico descreve o cenário do tutorial e apresenta a solução de exemplo.

## <a name="scenario-description"></a>Descrição do cenário

A Fabrikam, Inc., uma empresa fictícia, está criando uma solução que permite que as equipes de vendas remotas armazenem e recuperem informações de contato de uma interface da Web.

Os processos de gerenciamento do ciclo de vida do aplicativo (ALM) na Fabrikam, Inc. exigem que a solução seja implantada em três ambientes de servidor em vários estágios do processo de desenvolvimento de software:

- Um ambiente de teste do desenvolvedor ou "sandbox".
- Um ambiente de preparo baseado na intranet.
- Um ambiente de produção voltado para a Internet.

Cada um desses ambientes tem requisitos de configuração e segurança diferentes, e cada um apresenta desafios de implantação exclusivos.

### <a name="the-fabrikam-inc-server-infrastructure"></a>A infraestrutura de servidor da Fabrikam, Inc.

Esta é a infra-estrutura de desenvolvimento e implantação de alto nível na Fabrikam, Inc.

![](enterprise-web-deployment-scenario-overview/_static/image1.png)

As estações de trabalho do desenvolvedor, a infraestrutura de controle do código-fonte, o ambiente de teste do desenvolvedor e o ambiente de preparo residem na rede da intranet dentro do domínio Fabrikam.net. O ambiente de produção reside em uma rede de perímetro (também conhecida como DMZ, zona desmilitarizada e sub-rede filtrada), que é isolada da rede de intranet por um firewall. Esse é um cenário de implantação comum: você normalmente isola seus servidores Web voltados para a Internet da sua infraestrutura de servidor interna por meio do uso de firewalls ou servidores de gateway.

Neste exemplo:

- Um servidor Team Foundation Server (TFS) 2010 com um servidor de Build separado fornece a funcionalidade de controle do código-fonte e de CI (integração contínua).
- O ambiente de teste do desenvolvedor inclui um servidor Web Serviços de Informações da Internet (IIS) 7,5 e um servidor de banco de dados SQL Server 2008 R2.
- O ambiente de produção inclui vários servidores Web do IIS 7,5 sincronizados por um servidor de controlador de WFF (estrutura de Web farm), junto com um servidor de banco de dados SQL Server 2008 R2. Na prática, o servidor de banco de dados pode usar clustering ou espelhamento para melhorar a escalabilidade e a disponibilidade.
- O ambiente de preparo foi projetado para replicar a configuração do ambiente de produção o mais próximo possível.
- As diretivas de firewall e de isolamento de rede não permitem a implantação direta e automatizada da intranet na rede de perímetro.

A configuração de cada um desses ambientes é descrita mais detalhadamente no segundo tutorial, [Configurando ambientes de servidor para implantação da Web](../configuring-server-environments-for-web-deployment/configuring-server-environments-for-web-deployment.md).

### <a name="team-roles-for-alm"></a>Funções de equipe para ALM

Esses usuários estão envolvidos na criação, no gerenciamento, na criação e na publicação da solução Contact Manager:

- Matt hink é um desenvolvedor de aplicativos Web da Fabrikam, Inc. Ele faz parte da equipe que desenvolveu a solução Contact Manager usando o Visual Studio 2010. Matt tem direitos totais de administrador sobre os servidores no ambiente de teste do desenvolvedor, que permite que ele configure o ambiente para atender às suas necessidades. Ele também tem acesso de usuário à instância do TFS do Visual Studio 2010 em que armazena o código-fonte para a solução Contact Manager.
- Rob Fábio é um administrador de servidor da equipe de desenvolvimento da Fabrikam, Inc. Rob tem acesso administrativo no servidor TFS para que ele possa configurar todos os aspectos do TFS e do Team Build. Rob também tem acesso administrativo aos servidores Web de teste e de preparo e atua como o administrador de banco de dados (DBA) para os servidores de banco de dados nos ambientes de teste e de preparo. Rob configurou o Team Build no servidor TFS para executar estas tarefas:

    - Compilar e executar testes de unidade no aplicativo sempre que um usuário fizer check-in de um arquivo para o TFS. Isso é chamado de CI.
    - Implante o aplicativo Contact Manager no ambiente de teste automaticamente depois que o aplicativo passar testes de unidade. Isso inclui a publicação do banco de dados nos servidores de teste na implantação inicial e em todas as atualizações do banco de dados após a implantação inicial.
    - Implante o aplicativo Contact Manager no ambiente de preparo em um processo de etapa única.
    - Crie um pacote da Web que um administrador do servidor Web e um DBA possam usar para publicar o aplicativo no ambiente de produção.
- Lisa Andrews é um administrador de servidor responsável pela implantação de aplicativos nos servidores de produção Fabrikam, Inc. Ela tem acesso de leitura ao compartilhamento em que o TFS Team Build armazena o pacote de implantação da Web depois de criar o aplicativo Contact Manager. Ela também tem acesso administrativo aos servidores Web de produção para que ela possa implantar o aplicativo em produção. Além disso, ela atua como o DBA que implanta bancos de dados e atualizações de banco de dados no servidor de banco de dados no ambiente de produção.

<a id="_The_Contact_Manager"></a>

### <a name="the-contact-manager-solution"></a>A solução Gerenciador de Contatos

A solução Contact Manager foi projetada para permitir que usuários registrados e conectados adicionem e editem informações de contato por meio de uma interface da Web. A solução Contact Manager consiste em quatro projetos individuais:

![](enterprise-web-deployment-scenario-overview/_static/image2.png)

- **ContactManager. Mvc**. Este é um projeto de aplicativo Web ASP.NET MVC3 que representa o ponto de entrada para a solução. Ele oferece uma funcionalidade básica de aplicativo Web, como fornecer aos usuários a capacidade de criar e exibir detalhes de contato. O aplicativo depende de um serviço de Windows Communication Foundation (WCF) para gerenciar contatos e um banco de dados de serviços de aplicativos ASP.NET para gerenciar a autenticação e a autorização.
- **ContactManager. Database**. Este é um projeto de banco de dados do Visual Studio 2010. O projeto define o esquema para um banco de dados que armazena detalhes de contato.
- **ContactManager. Service**. Este é um projeto de serviço Web WCF. O WCF expõe um ponto de extremidade que permite que os chamadores realizem operações de criação, recuperação, atualização e exclusão (CRUD) no banco de dados do Gerenciador de contatos. O serviço depende do banco de dados do Contact Manager e do assembly ContactManager. Common. dll.
- **ContactManager. Common**. Este é um projeto de biblioteca de classes. O serviço WCF depende de tipos definidos neste assembly.

Uma revisão completa da solução e de seus requisitos de implantação é fornecida no primeiro tutorial desta série, [a implantação da Web na empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).

<a id="_Deployment_Tasks"></a>

## <a name="deployment-tasks"></a>Tarefas de implantação

Há várias tarefas distintas envolvidas na implantação de aplicativos em ambientes diferentes em uma grande organização. Estas são as principais tarefas que os tutoriais abordam:

![](enterprise-web-deployment-scenario-overview/_static/image3.png)

Aqui está uma lista de cada etapa no processo de implantação a partir da perspectiva dos usuários descritos anteriormente neste documento:

1. Todos os membros da equipe revisam a solução Contact Manager no Visual Studio 2010 para determinar os principais problemas e requisitos de implantação.
2. Matt hink pode implantar a solução Contact Manager diretamente da estação de trabalho do desenvolvedor para o ambiente de teste do desenvolvedor, para conduzir um teste inicial da lógica de implantação.
3. Matt hink adiciona o aplicativo ao controle do código-fonte no TFS.
4. Rob Fábio cria várias definições de Build para a solução Contact Manager no Team Build. Uma definição de compilação usa CI para implantar a solução no ambiente de teste do desenvolvedor sempre que um usuário faz o check-in de um novo código. Outra definição de compilação permite que os usuários disparem implantações no ambiente de preparo conforme necessário.
5. Toda vez que um usuário faz o check-in de um novo código, o Team Build cria automaticamente os componentes da solução, executa testes de unidade e implanta a solução no ambiente de teste do desenvolvedor se a compilação foi bem-sucedida e os testes de unidade são aprovados.
6. Quando um usuário dispara uma implantação para o ambiente de preparo, a solução é empacotada e implantada em um processo de etapa única. Esse processo também gera um pacote para implantação manual no ambiente de produção.
7. Lisa Andrews implanta o aplicativo no ambiente de produção importando manualmente o pacote da Web criado na etapa 6.

### <a name="key-deployment-issues"></a>Principais problemas de implantação

A solução Contact Manager e o cenário Fabrikam, Inc. destacam vários problemas comuns e desafios que você pode encontrar ao implantar soluções complexas de escala empresarial. Por exemplo:

- Você precisa ser capaz de implantar projetos em vários ambientes, como ambientes de desenvolvimento ou de teste, plataformas de preparo e servidores de produção. A solução precisa ser implantada com definições de configuração diferentes para cada ambiente.
- Você precisa implantar vários projetos dependentes simultaneamente como parte de um processo de compilação e implantação automatizado ou de etapa única.
- Você precisa ser capaz de impulsionar a implantação de um processo automatizado. Por exemplo, você deseja usar um processo de CI para implantar aplicativos Web em um ambiente de preparo quando o check-in do novo código é feito.
- Você precisa ser capaz de controlar o processo de implantação e definir variáveis de implantação de fora do Visual Studio, já que os desenvolvedores provavelmente têm as definições de configuração corretas ou as credenciais necessárias para cada ambiente de destino.
- Você precisa implantar projetos de banco de dados baseados em esquema e preservar os existentes em implantações subsequentes.
- Você precisa implantar bancos de dados de associação em uma base ad hoc sem implantar a conta de usuário. Talvez você também precise atualizar o esquema dos bancos de dados de associação implantados sem perder os existentes.
- Você precisa excluir determinados arquivos ou pastas ao implantar conteúdo em vários ambientes de destino.

Além disso, o gerenciamento da implantação quando as atualizações são frequentes e incrementais geram alguns desafios adicionais. Por exemplo:

- Você executa testes de unidade toda vez que um desenvolvedor faz check-in de um novo código. Você só deseja implantar a solução se o código passar pelos testes de unidade.
- Ao implantar um aplicativo Web em um ambiente de preparo ou de produção, você deseja redirecionar os usuários para um *aplicativo\_arquivo offline. htm* durante o processo de implantação.
- Você deseja registrar em log as atividades de implantação. O processo de implantação deve enviar notificações por email de implantações bem-sucedidas ou com falha para os destinatários designados.
- Se uma implantação automatizada falhar, o processo de implantação deverá repetir a implantação atual ou implantar o pacote da Web anterior em vez disso.

> [!div class="step-by-step"]
> [Anterior](deploying-web-applications-in-enterprise-scenarios.md)
> [Próximo](application-lifecycle-management-from-development-to-production.md)
