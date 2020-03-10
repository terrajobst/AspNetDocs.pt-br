---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Criando um projeto de equipe no TFS | Microsoft Docs
author: jrjlee
description: Este tópico descreve como criar um novo projeto de equipe no Team Foundation Server (TFS) 2010.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: d12e0636ce3b6239d125305db4354278f9f24960
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639653"
---
# <a name="creating-a-team-project-in-tfs"></a>Criação de um projeto de equipe no TFS

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como criar um novo projeto de equipe no Team Foundation Server (TFS) 2010.

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

## <a name="task-overview"></a>Visão geral da tarefa

Para provisionar e usar um novo projeto de equipe no TFS, você precisará concluir estas etapas de alto nível:

- Conceda permissões ao usuário que criará o novo projeto de equipe.
- Crie o projeto de equipe.
- Conceda permissões para os membros da equipe que trabalharão no projeto.
- Verifique algum conteúdo.

Este tópico mostrará como executar esses procedimentos e identificará os usuários e as funções de trabalho que provavelmente serão responsáveis por cada procedimento. Lembre-se de que, dependendo da estrutura da sua organização, cada uma dessas tarefas pode ser responsabilidade de uma pessoa diferente.

As tarefas e orientações neste tópico pressupõem que você instalou e configurou o TFS e que você criou uma coleção de projetos de equipe como parte do processo de configuração. Para obter mais informações sobre essas suposições e informações gerais sobre o cenário, consulte [configurar um servidor de compilação do TFS para implantação da Web](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Conceder permissões para o criador do projeto de equipe

Para criar um novo projeto de equipe, você precisará destas permissões:

- Você deve ter a permissão **criar novos projetos** na camada de aplicativo do TFS. Normalmente, você concede essa permissão adicionando usuários ao grupo TFS de **Administradores de coleção de projetos** . O grupo global de **Administradores do Team Foundation** também inclui essa permissão.
- Você deve ter permissão para criar novos sites de equipe dentro do conjunto de sites do SharePoint que corresponde à coleção de projetos de equipe do TFS. Normalmente, você concede essa permissão adicionando o usuário a um grupo do SharePoint com direitos de **controle total** na coleção de sites do SharePoint.
- Se você estiver usando recursos SQL Server Reporting Services, deverá ser um membro da função **gerente de conteúdo do Team Foundation** no Reporting Services.

### <a name="who-performs-these-procedures"></a>Quem executa esses procedimentos?

Normalmente, a pessoa ou o grupo que administra a implantação do TFS também executa esses procedimentos.

Como esse é um conjunto de permissões altamente privilegiado, novos projetos de equipe são normalmente criados por um pequeno subconjunto de usuários com responsabilidade pela administração de uma implantação do TFS. Normalmente, os desenvolvedores não receberão as permissões necessárias para criar novos projetos de equipe.

### <a name="grant-permissions-in-tfs"></a>Conceder permissões no TFS

Se você quiser permitir que um usuário crie novos projetos de equipe, a primeira tarefa de alto nível será adicionar o usuário ao grupo de **Administradores da coleção de projetos** para a coleção de projetos de equipe.

**Para adicionar um usuário ao grupo de administradores da coleção de projetos**

1. No servidor TFS, no menu **Iniciar** , aponte para **todos os programas**, clique em **Microsoft Team Foundation Server 2010**e, em seguida, clique em **console de administração do Team Foundation**.
2. No modo de exibição árvore de navegação, expanda **camada de aplicativo**e clique em coleções de **projetos de equipe**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. No painel **coleções de projetos de equipe** , selecione a coleção de projetos de equipe que você deseja gerenciar.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. Na guia **geral** , clique em **Associação de grupo**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. Na caixa de diálogo **grupos globais** , selecione o grupo **Administradores da coleção de projetos** e clique em **Propriedades**.
6. Na caixa de diálogo **Propriedades do grupo de Team Foundation Server** , selecione **usuário ou grupo do Windows**e clique em **Adicionar**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. Na caixa de diálogo **Selecionar usuários, computadores ou grupos** , digite o nome de usuário do usuário que você deseja que seja capaz de criar novos projetos de equipe, clique em **verificar nomes**e, em seguida, clique em **OK**.

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. Na caixa de diálogo **Propriedades do grupo de Team Foundation Server** , clique em **OK**.
9. Na caixa de diálogo **grupos globais** , clique em **fechar**.

### <a name="grant-permissions-in-sharepoint-services"></a>Conceder permissões nos serviços do SharePoint

Em seguida, você precisa conceder ao usuário permissão para criar novos sites de equipe no conjunto de sites do SharePoint que corresponde à sua coleção de projetos de equipe do TFS.

**Para conceder permissões de controle total no conjunto de sites do SharePoint**

1. Na Console de Administração do Team Foundation Server, na página **coleções de projetos de equipe** , selecione a coleção de projetos de equipe que você deseja gerenciar.
2. Na guia **site do SharePoint** , observe o valor da URL do **local do site padrão atual** .

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Abra o Internet Explorer e, em seguida, vá para a URL que você anotou na etapa 2.

    > [!NOTE]
    > Se não estiver conectado ao Windows como o usuário que criou a coleção de projetos de equipe, você precisará entrar no SharePoint como esse usuário para continuar.
4. No menu **Ações do Site** , clique em **Configurações de Site**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. Na página **configurações do site** , em **usuários e permissões**, clique em **pessoas e grupos**.
6. No painel de navegação esquerdo, clique em **grupos**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. Na página **pessoas e grupos: todos os grupos** , clique em **configurar grupos para este site**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > Você pode receber um erro <strong>HTTP 404 não encontrado</strong> devido a um bug de codificação http duplo. Se isso ocorrer, substitua a URL por:   
   > `[site_collection_URL]/_layouts/permsetup.aspx` Por exemplo:  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. Na página **configurar grupos para este site** , adicione o usuário que criará projetos de equipe ao grupo **proprietários** e clique em **OK**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Para obter mais informações sobre como permitir que os usuários criem novos projetos de equipe dentro de uma coleção de projetos de equipe, consulte [definir permissões de administrador para coleções de projetos de equipe](https://msdn.microsoft.com/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Criar um novo projeto de equipe e adicionar usuários

Depois de ter as permissões necessárias, você pode usar a janela **Team Explorer** no Visual Studio 2010 para criar um novo projeto de equipe. Essa abordagem fornece um assistente que coleta todas as informações necessárias e executa as tarefas necessárias no TFS, no SharePoint e no SQL Server Reporting Services. Você também precisará conceder permissões no novo projeto de equipe aos membros da equipe do desenvolvedor, para permitir que eles adicionem e modifiquem o conteúdo.

### <a name="who-performs-these-procedures"></a>Quem executa esses procedimentos?

Geralmente, um administrador do TFS ou um líder da equipe do desenvolvedor executa esses procedimentos.

### <a name="create-a-new-team-project"></a>Criar um novo projeto de equipe

O procedimento a seguir descreve como criar um novo projeto de equipe no TFS 2010.

**Para criar um novo projeto de equipe**

1. No menu **Iniciar** , aponte para **todos os programas**, clique **em Microsoft Visual Studio 2010**, clique com o botão direito do mouse em **Microsoft Visual Studio 2010**e clique em **Executar como administrador**.

    > [!NOTE]
    > Se você não executar o Visual Studio 2010 como administrador, o assistente do novo projeto de equipe falhará na última etapa.
2. (Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, clique em **Sim**.
3. No Visual Studio, no menu **equipe** , clique em **conectar-se a Team Foundation Server**.

    > [!NOTE]
    > Se você já tiver configurado uma conexão com um servidor TFS, poderá omitir as etapas 4-7.
4. Na caixa de diálogo **conexão com o projeto de equipe** , clique em **servidores**.
5. Na caixa de diálogo **Adicionar/remover Team Foundation Server** , clique em **Adicionar**.
6. Na caixa de diálogo **adicionar Team Foundation Server** , forneça os detalhes da sua instância do TFS e clique em **OK**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. Na caixa de diálogo **Adicionar/remover Team Foundation Server** , clique em **fechar**.
8. Na caixa de diálogo **conectar ao projeto de equipe** , selecione a instância do TFS à qual você deseja se conectar, selecione a coleção de projetos de equipe à qual você deseja adicionar e clique em **conectar**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. Na janela **Team Explorer** , clique com o botão direito do mouse na coleção de projetos de equipe e clique em **novo projeto de equipe**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. Na caixa de diálogo **novo projeto de equipe** , forneça um nome e uma descrição para o projeto de equipe e clique em **Avançar**.

    > [!NOTE]
    > Se o seu projeto de equipe incluir espaços, você poderá enfrentar alguns problemas quando vier a usar a Implantação da Web ferramenta de implantação da Web do Serviços de Informações da Internet (IIS) para implantar pacotes do caminho de saída. Os espaços no caminho podem dificultar muito a execução de Implantação da Web comandos.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. Na página **selecionar um modelo de processo** , selecione o modelo de processo que você deseja usar para gerenciar o processo de desenvolvimento e clique em **Avançar**.

    > [!NOTE]
    > Para obter mais informações sobre modelos de processo do TFS, consulte [modelos e ferramentas de processo](https://msdn.microsoft.com/vstudio/aa718795).
12. Na página **configurações do site de equipe** , deixe as configurações padrão inalteradas e clique em **Avançar**.
13. Essa configuração cria ou identifica um site de equipe do SharePoint associado ao projeto de equipe do TFS. Sua equipe de desenvolvimento pode usar este site para gerenciar a documentação, participar de threads de discussão, criar páginas wiki e executar várias outras tarefas que não estão relacionadas ao código. Para obter mais informações, consulte [interações entre produtos e Team Foundation Server do SharePoint](https://msdn.microsoft.com/library/ms253177.aspx).
14. Na página **especificar configurações de controle do código-fonte** , deixe as configurações padrão inalteradas e clique em **Avançar**.
15. Essa configuração identifica ou cria o local na hierarquia de pastas do TFS que atuará como uma pasta raiz para seu conteúdo.
16. Na página **confirmar as configurações do projeto de equipe** , clique em **concluir**.
17. Quando o novo projeto de equipe for criado com êxito, na página **projeto de equipe criado** , clique em **fechar**.

### <a name="add-users-to-a-team-project"></a>Adicionar usuários a um projeto de equipe

Agora que você criou o novo projeto de equipe, você pode conceder permissões aos usuários para habilitá-los a começar a adicionar e colaborar no conteúdo.

**Para adicionar usuários a um projeto de equipe**

1. No Visual Studio 2010, na janela **Team Explorer** , clique com o botão direito do mouse no projeto de equipe, aponte para **configurações do projeto de equipe**e clique em associação de **grupo**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. Para permitir que um usuário adicione, modifique e remova código sob controle do código-fonte, você precisa adicioná-lo ao grupo **colaboradores** .
3. Na caixa de diálogo **grupos de projetos** , selecione o grupo **colaboradores** e clique em **Propriedades**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. Na caixa de diálogo **Propriedades do grupo de Team Foundation Server** , selecione **usuário ou grupo do Windows**e clique em **Adicionar**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. Na caixa de diálogo **Selecionar usuários, computadores ou grupos** , digite o nome de usuário do usuário que você deseja adicionar ao projeto de equipe, clique em **verificar nomes**e, em seguida, clique em **OK**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. Na caixa de diálogo **Propriedades do grupo de Team Foundation Server** , clique em **OK**.
7. Na caixa de diálogo **grupos de projetos** , clique em **fechar**.

## <a name="conclusion"></a>Conclusão

Neste ponto, seu novo projeto de equipe está pronto para uso, e sua equipe de desenvolvedores pode começar a adicionar conteúdo e colaborar no processo de desenvolvimento.

O próximo tópico, [adicionando conteúdo ao controle do código-fonte](adding-content-to-source-control.md), descreve como adicionar conteúdo ao controle do código-fonte.

## <a name="further-reading"></a>Leitura adicional

Para obter uma orientação mais ampla sobre a criação de projetos de equipe no TFS, consulte [criar um projeto de equipe](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx). Para obter mais informações sobre como permitir que os usuários criem novos projetos de equipe dentro de uma coleção de projetos de equipe, consulte [definir permissões de administrador para coleções de projetos de equipe](https://msdn.microsoft.com/library/dd547204.aspx). Para obter mais informações sobre como adicionar usuários a projetos de equipe, consulte [Adicionar usuários a projetos de equipe](https://msdn.microsoft.com/library/bb558971.aspx).

> [!div class="step-by-step"]
> [Anterior](configuring-team-foundation-server-for-web-deployment.md)
> [Próximo](adding-content-to-source-control.md)
