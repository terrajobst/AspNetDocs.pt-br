---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Adicionando conteúdo ao controle do código-fonte | Microsoft Docs
author: jrjlee
description: Este tópico explica como adicionar conteúdo ao controle do código-fonte no Team Foundation Server (TFS) 2010. Ele descreve como adicionar soluções e projetos a um proj... da equipe...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 16073dd2fb0ea1cc4ddbc94c843181933dc174c1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634459"
---
# <a name="adding-content-to-source-control"></a>Adição de conteúdo ao controle do código-fonte

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico explica como adicionar conteúdo ao controle do código-fonte no Team Foundation Server (TFS) 2010. Ele descreve como adicionar soluções e projetos a um projeto de equipe no TFS e explica como adicionar dependências externas como estruturas ou assemblies ao controle do código-fonte.

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

## <a name="task-overview"></a>Visão geral da tarefa

Na maioria dos casos, todos os membros da equipe de desenvolvedores devem ser capazes de adicionar conteúdo ao controle do código-fonte. Para adicionar uma solução ao controle do código-fonte no TFS, você precisará concluir estas etapas de alto nível:

- Conecte-se a um projeto de equipe.
- Mapeie a estrutura de pastas do projeto de equipe no servidor para uma estrutura de pastas no computador local.
- Adicione a solução e seu conteúdo ao controle do código-fonte.
- Adicione quaisquer dependências externas ao controle do código-fonte.

Este tópico mostrará como executar esses procedimentos.

As tarefas e orientações neste tópico pressupõem que você já criou um novo projeto de equipe do TFS para gerenciar seu conteúdo. Para obter mais informações sobre como criar um novo projeto de equipe, consulte [criando um projeto de equipe no TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Quem executa esses procedimentos?

Na maioria dos casos, todos os membros da equipe de desenvolvedores devem ser capazes de adicionar e modificar conteúdo dentro de projetos de equipe específicos.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Conectar-se a um projeto de equipe e criar um mapeamento de pasta

Antes de adicionar qualquer conteúdo ao controle do código-fonte, você precisa se conectar a um projeto de equipe e criar um mapeamento entre a estrutura de pastas no servidor e o sistema de arquivos em seu computador local.

**Para se conectar a um projeto de equipe e mapear um caminho local**

1. Na estação de trabalho do desenvolvedor, abra o Visual Studio 2010.
2. No Visual Studio, no menu **equipe** , clique em **conectar-se a Team Foundation Server**.

    > [!NOTE]
    > Se você já tiver configurado uma conexão com um servidor TFS, poderá omitir as etapas 3-6.
3. Na caixa de diálogo **conexão com o projeto de equipe** , clique em **servidores**.
4. Na caixa de diálogo **Adicionar/remover Team Foundation Server** , clique em **Adicionar**.
5. Na caixa de diálogo **adicionar Team Foundation Server** , forneça os detalhes da sua instância do TFS e clique em **OK**.

    ![](adding-content-to-source-control/_static/image1.png)
6. Na caixa de diálogo **Adicionar/remover Team Foundation Server** , clique em **fechar**.
7. Na caixa de diálogo **conectar ao projeto de equipe** , selecione a instância do TFS à qual você deseja se conectar, selecione a coleção de projetos de equipe, selecione o projeto de equipe ao qual você deseja adicionar e, em seguida, clique em **conectar**.

    ![](adding-content-to-source-control/_static/image2.png)
8. Na janela **Team Explorer** , expanda seu projeto de equipe e clique duas vezes em **controle do código-fonte**.

    ![](adding-content-to-source-control/_static/image3.png)
9. Na guia **Source Control Explorer** , clique em **não mapeado**.

    ![](adding-content-to-source-control/_static/image4.png)
10. Na caixa de diálogo **mapa** , na caixa **pasta local** , navegue até (ou crie) uma pasta local para atuar como a pasta raiz do projeto de equipe e clique em **mapear**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Quando for solicitado a baixar os arquivos de origem, clique em **Sim**.

    ![](adding-content-to-source-control/_static/image6.png)

Neste ponto, você mapeou a pasta do lado do servidor para o projeto de equipe para uma pasta local na estação de trabalho do desenvolvedor. Você também baixou qualquer conteúdo existente do projeto de equipe para sua estrutura de pasta local. Agora você pode começar a adicionar seu próprio conteúdo ao controle do código-fonte.

## <a name="add-projects-and-solutions-to-source-control"></a>Adicionar projetos e soluções ao controle do código-fonte

Para adicionar projetos e soluções ao controle do código-fonte, primeiro você precisa movê-los para a pasta mapeada para o projeto de equipe no computador local. Em seguida, você pode fazer o check-in do conteúdo para sincronizar suas adições com o servidor.

**Para adicionar projetos ao controle do código-fonte**

1. Na estação de trabalho do desenvolvedor, mova seus projetos e soluções para um local apropriado dentro da estrutura de pastas mapeada para o projeto de equipe.

    > [!NOTE]
    > Muitas organizações terão uma abordagem preferida para como projetos e soluções devem ser organizados no controle do código-fonte. Para obter orientação sobre como estruturar pastas, consulte [como: estruturar suas pastas de controle do código-fonte no Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).
2. Abra a solução no Visual Studio 2010.
3. Na janela **Gerenciador de soluções** , clique com o botão direito do mouse na solução e clique em **Adicionar solução ao controle do código-fonte**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > Em alguns casos, dependendo de como sua organização gosta de estruturar o conteúdo no TFS, talvez seja necessário adicionar projetos ao controle do código-fonte individualmente para fornecer um controle mais refinado sobre como seu código-fonte é organizado.
4. Verifique se a guia **Source Control Explorer** exibe o conteúdo que você adicionou na estrutura de pasta do servidor para o projeto de equipe.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > A guia **Source Control Explorer** exibe o conteúdo sem solicitação adicional porque você adicionou sua solução a uma pasta mapeada no sistema de arquivos local. Se sua solução estava em um local não mapeado, você será solicitado a especificar locais de pasta no TFS e no sistema de arquivos local.
5. Na guia **Source Control Explorer** , no painel **pastas** , clique com o botão direito do mouse no projeto de equipe (por exemplo, **ContactManager**) e clique em **fazer check-in de alterações pendentes**.
6. Na caixa de diálogo **check-in – arquivos de origem** , digite um comentário e, em seguida, clique em **check-in**.

    ![](adding-content-to-source-control/_static/image9.png)

Neste ponto, você adicionou sua solução ao controle do código-fonte no TFS.

## <a name="add-external-dependencies-to-source-control"></a>Adicionar dependências externas ao controle do código-fonte

Quando você adiciona um projeto ou uma solução ao controle do código-fonte, todos os arquivos e pastas dentro de seu projeto ou solução também serão adicionados. No entanto, em muitos casos, projetos e soluções também dependem de dependências externas, como assemblies locais, para funcionar corretamente. Você precisa adicionar esses recursos ao controle do código-fonte para permitir que tanto o Team Build quanto outros membros da equipe do desenvolvedor criem seu código com êxito.

Por exemplo, a estrutura de pastas da solução de exemplo do Contact Manager inclui uma pasta chamada Packages. Ele contém o assembly e vários recursos de suporte para o ADO.NET Entity Framework 4,1. A pasta pacotes não faz parte da solução Contact Manager, mas a solução não será compilada com êxito sem ele. Para habilitar o Team Build a criar a solução, você precisa adicionar a pasta Packages ao controle do código-fonte.

> [!NOTE]
> A inclusão de uma pasta de pacotes é típica do que acontece quando você adiciona o Entity Framework, ou recursos semelhantes, à sua solução usando a extensão NuGet para o Visual Studio 2010.

**Para adicionar conteúdo que não seja do projeto ao controle do código-fonte**

1. Verifique se os itens que você deseja adicionar (por exemplo, a pasta pacotes) estão em um local apropriado dentro de uma pasta mapeada no sistema de arquivos local.
2. No Visual Studio 2010, na janela **Team Explorer** , expanda seu projeto de equipe e clique duas vezes em **controle do código-fonte**.

    ![](adding-content-to-source-control/_static/image10.png)
3. Na guia **Source Control Explorer** , no painel **pastas** , selecione a pasta que contém o item ou os itens que você deseja adicionar.
4. Clique no botão **Adicionar itens à pasta** .

    ![](adding-content-to-source-control/_static/image11.png)
5. Na caixa de diálogo **Adicionar ao controle do código-fonte** , selecione a pasta ou os itens que você deseja adicionar e clique em **Avançar**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Na guia **itens excluídos** , selecione os itens necessários que foram excluídos automaticamente (por exemplo, assemblies) e clique em **incluir Item (ns)** .

    ![](adding-content-to-source-control/_static/image13.png)
7. Na guia **itens a serem adicionados** , verifique se todos os arquivos que você deseja incluir estão listados e clique em **concluir**.

    ![](adding-content-to-source-control/_static/image14.png)
8. Na janela **Source Control Explorer** , clique no botão **check-in** .

    ![](adding-content-to-source-control/_static/image15.png)
9. Na caixa de diálogo **check-in – arquivos de origem** , digite um comentário e, em seguida, clique em **check-in**.

Neste ponto, você adicionou as dependências externas para sua solução ao controle do código-fonte.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como se conectar a um projeto de equipe, mapear uma estrutura de pastas e adicionar conteúdo ao controle do código-fonte. Para obter mais informações sobre como trabalhar com itens sob controle do código-fonte, consulte [usando o controle de versão](https://msdn.microsoft.com/library/ms181368.aspx).

O próximo tópico, [Configurando um servidor de compilação do TFS para implantação da Web](configuring-a-tfs-build-server-for-web-deployment.md), descreve como preparar um servidor do TFS Team Build para compilar e implantar sua solução.

## <a name="further-reading"></a>Leitura adicional

Para obter informações mais abrangentes sobre como trabalhar com o controle do código-fonte no TFS, consulte [usando o controle de versão](https://msdn.microsoft.com/library/ms181368.aspx).

> [!div class="step-by-step"]
> [Anterior](creating-a-team-project-in-tfs.md)
> [Próximo](configuring-a-tfs-build-server-for-web-deployment.md)
