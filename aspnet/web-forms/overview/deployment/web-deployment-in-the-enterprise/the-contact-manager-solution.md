---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: A solução do Gerenciador de contatos | Microsoft Docs
author: jrjlee
description: Esta série de tutoriais usa uma solução&#x2014;de exemplo da solução&#x2014;Contact Manager para representar um aplicativo de escala empresarial com um leve realista...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 12ed7827f7392e559e04121386f7cd045de8462b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586264"
---
# <a name="the-contact-manager-solution"></a>A solução Gerenciador de Contatos

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Esta [série de tutoriais](web-deployment-in-the-enterprise.md) usa uma solução&#x2014;de exemplo da solução&#x2014;Contact Manager para representar um aplicativo de escala empresarial com um nível realista de complexidade. Este tópico apresenta a solução Contact Manager, descreve os principais componentes da solução e identifica os desafios na implantação desse tipo de aplicativo em várias plataformas de destino em um ambiente corporativo.
> 
> Ao trabalhar com os tópicos nestes tutoriais, você pode usar a solução Contact Manager como uma implementação de referência que demonstra como você pode atender a desafios específicos em cenários de implantação corporativa. O próximo tópico, [Configurando a solução Contact Manager](setting-up-the-contact-manager-solution.md), descreve como baixar e executar a solução em sua estação de trabalho do desenvolvedor.

## <a name="solution-overview"></a>Visão geral da solução

A solução Contact Manager consiste em quatro projetos individuais:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager. Mvc**. Este é um projeto de aplicativo Web ASP.NET MVC 3 que representa o ponto de entrada para a solução. Ele oferece uma funcionalidade básica de aplicativo Web, como fornecer aos usuários a capacidade de criar e exibir detalhes de contato. O aplicativo depende de um serviço de Windows Communication Foundation (WCF) para gerenciar contatos e um banco de dados de serviços de aplicativos ASP.NET para gerenciar a autenticação e a autorização.
- **ContactManager. Database**. Este é um projeto de banco de dados do Visual Studio. O projeto define o esquema para um banco de dados que armazena detalhes de contato.
- **ContactManager. Service**. Este é um projeto de serviço Web WCF. O serviço WCF expõe um ponto de extremidade que permite aos chamadores executar operações de criação, recuperação, atualização e exclusão (CRUD) no banco de dados **ContactManager** . O serviço depende do banco de dados **ContactManager** e do assembly **ContactManager. Common. dll** .
- **ContactManager. Common**. Este é um projeto de biblioteca de classes. O serviço WCF depende de tipos definidos neste assembly.

A solução também inclui uma pasta de solução chamada Publish. Ele contém vários arquivos de projeto personalizados e arquivos de comando que demonstram como você pode controlar e manipular o processo de compilação e implantação. Eles serão abordados em mais detalhes posteriormente neste tutorial.

Em um nível conceitual, os componentes da solução se ajustam da seguinte maneira:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Enquanto o aplicativo Web ASP.NET MVC 3 usa o provedor de associação ASP.NET, todas as páginas do aplicativo Web permitem acesso anônimo. Essa não é claramente uma configuração realista. No entanto, a solução é configurada dessa forma para facilitar a implantação e o teste da solução sem configurar contas de usuário e funções.

## <a name="deployment-challenges"></a>Desafios de implantação

A solução Contact Manager ilustra vários desafios de implantação que são comuns a muitos cenários de implantação empresarial:

- A solução consiste em vários projetos dependentes. Você precisa implantar esses projetos simultaneamente.
- As cadeias de conexão e os pontos de extremidade de serviço precisam ser atualizados para cada ambiente e, em muitos casos, essas informações não estarão disponíveis para o desenvolvedor.
- Ao implantar o banco de dados **ContactManager** em ambientes de preparo e produção, você precisa preservar os dados existentes nas implantações subsequentes.
- Quando você implanta o banco de dados de serviços de aplicativos do ASP.NET, você precisa implantar alguns dado de configuração, mas omitir quaisquer dados de conta de usuário.
- Os projetos incluem alguns arquivos e pastas que não devem ser implantados. Você precisa excluir esses arquivos e pastas do processo de implantação.
- A solução precisa dar suporte à implantação automatizada de um servidor de compilação Team Foundation Server (TFS).

## <a name="conclusion"></a>Conclusão

Este tópico forneceu uma visão geral de alto nível da solução Contact Manager e identificou alguns dos desafios de implantação inerentes que são comuns a muitos cenários de implantação empresarial. Os tópicos restantes neste tutorial descrevem algumas das técnicas que você pode usar para atender a esses desafios.

O próximo tópico, [Configurando a solução Contact Manager](setting-up-the-contact-manager-solution.md), descreve como baixar e executar a solução em sua estação de trabalho do desenvolvedor.

> [!div class="step-by-step"]
> [Anterior](web-deployment-in-the-enterprise.md)
> [Próximo](setting-up-the-contact-manager-solution.md)
