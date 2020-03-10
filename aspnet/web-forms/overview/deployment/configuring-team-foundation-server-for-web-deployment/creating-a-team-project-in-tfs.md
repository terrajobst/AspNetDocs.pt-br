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
# <a name="creating-a-team-project-in-tfs"></a><span data-ttu-id="b9074-103">Criação de um projeto de equipe no TFS</span><span class="sxs-lookup"><span data-stu-id="b9074-103">Creating a Team Project in TFS</span></span>

<span data-ttu-id="b9074-104">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="b9074-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="b9074-105">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="b9074-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="b9074-106">Este tópico descreve como criar um novo projeto de equipe no Team Foundation Server (TFS) 2010.</span><span class="sxs-lookup"><span data-stu-id="b9074-106">This topic describes how to create a new team project in Team Foundation Server (TFS) 2010.</span></span>

<span data-ttu-id="b9074-107">Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b9074-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

## <a name="task-overview"></a><span data-ttu-id="b9074-108">Visão geral da tarefa</span><span class="sxs-lookup"><span data-stu-id="b9074-108">Task Overview</span></span>

<span data-ttu-id="b9074-109">Para provisionar e usar um novo projeto de equipe no TFS, você precisará concluir estas etapas de alto nível:</span><span class="sxs-lookup"><span data-stu-id="b9074-109">To provision and use a new team project in TFS, you'll need to complete these high-level steps:</span></span>

- <span data-ttu-id="b9074-110">Conceda permissões ao usuário que criará o novo projeto de equipe.</span><span class="sxs-lookup"><span data-stu-id="b9074-110">Grant permissions to the user who will create the new team project.</span></span>
- <span data-ttu-id="b9074-111">Crie o projeto de equipe.</span><span class="sxs-lookup"><span data-stu-id="b9074-111">Create the team project.</span></span>
- <span data-ttu-id="b9074-112">Conceda permissões para os membros da equipe que trabalharão no projeto.</span><span class="sxs-lookup"><span data-stu-id="b9074-112">Grant permissions to the team members who will work on the project.</span></span>
- <span data-ttu-id="b9074-113">Verifique algum conteúdo.</span><span class="sxs-lookup"><span data-stu-id="b9074-113">Check in some content.</span></span>

<span data-ttu-id="b9074-114">Este tópico mostrará como executar esses procedimentos e identificará os usuários e as funções de trabalho que provavelmente serão responsáveis por cada procedimento.</span><span class="sxs-lookup"><span data-stu-id="b9074-114">This topic will show you how to perform these procedures, and it will identify the users and job roles that are likely to be responsible for each procedure.</span></span> <span data-ttu-id="b9074-115">Lembre-se de que, dependendo da estrutura da sua organização, cada uma dessas tarefas pode ser responsabilidade de uma pessoa diferente.</span><span class="sxs-lookup"><span data-stu-id="b9074-115">Be aware that, depending on the structure of your organization, each of these tasks may be the responsibility of a different person.</span></span>

<span data-ttu-id="b9074-116">As tarefas e orientações neste tópico pressupõem que você instalou e configurou o TFS e que você criou uma coleção de projetos de equipe como parte do processo de configuração.</span><span class="sxs-lookup"><span data-stu-id="b9074-116">The tasks and walkthroughs in this topic assume that you've installed and configured TFS, and that you've created a team project collection as part of the configuration process.</span></span> <span data-ttu-id="b9074-117">Para obter mais informações sobre essas suposições e informações gerais sobre o cenário, consulte [configurar um servidor de compilação do TFS para implantação da Web](configuring-a-tfs-build-server-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="b9074-117">For more information on these assumptions, and for more general background information on the scenario, see [Configure a TFS Build Server for Web Deployment](configuring-a-tfs-build-server-for-web-deployment.md).</span></span>

## <a name="grant-permissions-to-the-team-project-creator"></a><span data-ttu-id="b9074-118">Conceder permissões para o criador do projeto de equipe</span><span class="sxs-lookup"><span data-stu-id="b9074-118">Grant Permissions to the Team Project Creator</span></span>

<span data-ttu-id="b9074-119">Para criar um novo projeto de equipe, você precisará destas permissões:</span><span class="sxs-lookup"><span data-stu-id="b9074-119">In order to create a new team project, you need these permissions:</span></span>

- <span data-ttu-id="b9074-120">Você deve ter a permissão **criar novos projetos** na camada de aplicativo do TFS.</span><span class="sxs-lookup"><span data-stu-id="b9074-120">You must have the **Create new projects** permission on the TFS application tier.</span></span> <span data-ttu-id="b9074-121">Normalmente, você concede essa permissão adicionando usuários ao grupo TFS de **Administradores de coleção de projetos** .</span><span class="sxs-lookup"><span data-stu-id="b9074-121">You typically grant this permission by adding users to the **Project Collection Administrators** TFS group.</span></span> <span data-ttu-id="b9074-122">O grupo global de **Administradores do Team Foundation** também inclui essa permissão.</span><span class="sxs-lookup"><span data-stu-id="b9074-122">The **Team Foundation Administrators** global group also includes this permission.</span></span>
- <span data-ttu-id="b9074-123">Você deve ter permissão para criar novos sites de equipe dentro do conjunto de sites do SharePoint que corresponde à coleção de projetos de equipe do TFS.</span><span class="sxs-lookup"><span data-stu-id="b9074-123">You must have permission to create new team sites within the SharePoint site collection that corresponds to the TFS team project collection.</span></span> <span data-ttu-id="b9074-124">Normalmente, você concede essa permissão adicionando o usuário a um grupo do SharePoint com direitos de **controle total** na coleção de sites do SharePoint.</span><span class="sxs-lookup"><span data-stu-id="b9074-124">You typically grant this permission by adding the user to a SharePoint group with **Full Control** rights on the SharePoint site collection.</span></span>
- <span data-ttu-id="b9074-125">Se você estiver usando recursos SQL Server Reporting Services, deverá ser um membro da função **gerente de conteúdo do Team Foundation** no Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="b9074-125">If you're using SQL Server Reporting Services features, you must be a member of the **Team Foundation Content Manager** role in Reporting Services.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="b9074-126">Quem executa esses procedimentos?</span><span class="sxs-lookup"><span data-stu-id="b9074-126">Who Performs These Procedures?</span></span>

<span data-ttu-id="b9074-127">Normalmente, a pessoa ou o grupo que administra a implantação do TFS também executa esses procedimentos.</span><span class="sxs-lookup"><span data-stu-id="b9074-127">Typically, the person or group who administers the TFS deployment also performs these procedures.</span></span>

<span data-ttu-id="b9074-128">Como esse é um conjunto de permissões altamente privilegiado, novos projetos de equipe são normalmente criados por um pequeno subconjunto de usuários com responsabilidade pela administração de uma implantação do TFS.</span><span class="sxs-lookup"><span data-stu-id="b9074-128">Because this is a highly privileged set of permissions, new team projects are typically created by a small subset of users with responsibility for administering a TFS deployment.</span></span> <span data-ttu-id="b9074-129">Normalmente, os desenvolvedores não receberão as permissões necessárias para criar novos projetos de equipe.</span><span class="sxs-lookup"><span data-stu-id="b9074-129">Developers will not usually be granted the permissions required to create new team projects.</span></span>

### <a name="grant-permissions-in-tfs"></a><span data-ttu-id="b9074-130">Conceder permissões no TFS</span><span class="sxs-lookup"><span data-stu-id="b9074-130">Grant Permissions in TFS</span></span>

<span data-ttu-id="b9074-131">Se você quiser permitir que um usuário crie novos projetos de equipe, a primeira tarefa de alto nível será adicionar o usuário ao grupo de **Administradores da coleção de projetos** para a coleção de projetos de equipe.</span><span class="sxs-lookup"><span data-stu-id="b9074-131">If you want to enable a user to create new team projects, the first high-level task is to add the user to the **Project Collection Administrators** group for the team project collection.</span></span>

<span data-ttu-id="b9074-132">**Para adicionar um usuário ao grupo de administradores da coleção de projetos**</span><span class="sxs-lookup"><span data-stu-id="b9074-132">**To add a user to the Project Collection Administrators group**</span></span>

1. <span data-ttu-id="b9074-133">No servidor TFS, no menu **Iniciar** , aponte para **todos os programas**, clique em **Microsoft Team Foundation Server 2010**e, em seguida, clique em **console de administração do Team Foundation**.</span><span class="sxs-lookup"><span data-stu-id="b9074-133">On the TFS server, on the **Start** menu, point to **All Programs**, click **Microsoft Team Foundation Server 2010**, and then click **Team Foundation Administration Console**.</span></span>
2. <span data-ttu-id="b9074-134">No modo de exibição árvore de navegação, expanda **camada de aplicativo**e clique em coleções de **projetos de equipe**.</span><span class="sxs-lookup"><span data-stu-id="b9074-134">In the navigation tree view, expand **Application Tier**, and then click **Team Project Collections**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. <span data-ttu-id="b9074-135">No painel **coleções de projetos de equipe** , selecione a coleção de projetos de equipe que você deseja gerenciar.</span><span class="sxs-lookup"><span data-stu-id="b9074-135">In the **Team Project Collections** pane, select the team project collection you want to manage.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. <span data-ttu-id="b9074-136">Na guia **geral** , clique em **Associação de grupo**.</span><span class="sxs-lookup"><span data-stu-id="b9074-136">On the **General** tab, click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. <span data-ttu-id="b9074-137">Na caixa de diálogo **grupos globais** , selecione o grupo **Administradores da coleção de projetos** e clique em **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="b9074-137">In the **Global Groups** dialog box, select the **Project Collection Administrators** group, and then click **Properties**.</span></span>
6. <span data-ttu-id="b9074-138">Na caixa de diálogo **Propriedades do grupo de Team Foundation Server** , selecione **usuário ou grupo do Windows**e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b9074-138">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. <span data-ttu-id="b9074-139">Na caixa de diálogo **Selecionar usuários, computadores ou grupos** , digite o nome de usuário do usuário que você deseja que seja capaz de criar novos projetos de equipe, clique em **verificar nomes**e, em seguida, clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="b9074-139">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to be able to create new team projects, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. <span data-ttu-id="b9074-140">Na caixa de diálogo **Propriedades do grupo de Team Foundation Server** , clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="b9074-140">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
9. <span data-ttu-id="b9074-141">Na caixa de diálogo **grupos globais** , clique em **fechar**.</span><span class="sxs-lookup"><span data-stu-id="b9074-141">In the **Global Groups** dialog box, click **Close**.</span></span>

### <a name="grant-permissions-in-sharepoint-services"></a><span data-ttu-id="b9074-142">Conceder permissões nos serviços do SharePoint</span><span class="sxs-lookup"><span data-stu-id="b9074-142">Grant Permissions in SharePoint Services</span></span>

<span data-ttu-id="b9074-143">Em seguida, você precisa conceder ao usuário permissão para criar novos sites de equipe no conjunto de sites do SharePoint que corresponde à sua coleção de projetos de equipe do TFS.</span><span class="sxs-lookup"><span data-stu-id="b9074-143">Next, you need to give the user permission to create new team sites in the SharePoint site collection that corresponds to your TFS team project collection.</span></span>

<span data-ttu-id="b9074-144">**Para conceder permissões de controle total no conjunto de sites do SharePoint**</span><span class="sxs-lookup"><span data-stu-id="b9074-144">**To grant Full Control permissions on the SharePoint site collection**</span></span>

1. <span data-ttu-id="b9074-145">Na Console de Administração do Team Foundation Server, na página **coleções de projetos de equipe** , selecione a coleção de projetos de equipe que você deseja gerenciar.</span><span class="sxs-lookup"><span data-stu-id="b9074-145">In the Team Foundation Server Administration Console, on the **Team Project Collections** page, select the team project collection you want to manage.</span></span>
2. <span data-ttu-id="b9074-146">Na guia **site do SharePoint** , observe o valor da URL do **local do site padrão atual** .</span><span class="sxs-lookup"><span data-stu-id="b9074-146">On the **SharePoint Site** tab, note the value of the **Current Default Site Location** URL.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. <span data-ttu-id="b9074-147">Abra o Internet Explorer e, em seguida, vá para a URL que você anotou na etapa 2.</span><span class="sxs-lookup"><span data-stu-id="b9074-147">Open Internet Explorer, and then go to the URL you noted in step 2.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b9074-148">Se não estiver conectado ao Windows como o usuário que criou a coleção de projetos de equipe, você precisará entrar no SharePoint como esse usuário para continuar.</span><span class="sxs-lookup"><span data-stu-id="b9074-148">If you're not logged on to Windows as the user who created the team project collection, you'll need to sign in to SharePoint as this user in order to continue.</span></span>
4. <span data-ttu-id="b9074-149">No menu **Ações do Site** , clique em **Configurações de Site**.</span><span class="sxs-lookup"><span data-stu-id="b9074-149">On the **Site Actions** menu, click **Site Settings**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. <span data-ttu-id="b9074-150">Na página **configurações do site** , em **usuários e permissões**, clique em **pessoas e grupos**.</span><span class="sxs-lookup"><span data-stu-id="b9074-150">On the **Site Settings** page, under **Users and Permissions**, click **People and groups**.</span></span>
6. <span data-ttu-id="b9074-151">No painel de navegação esquerdo, clique em **grupos**.</span><span class="sxs-lookup"><span data-stu-id="b9074-151">In the left navigation panel, click **Groups**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. <span data-ttu-id="b9074-152">Na página **pessoas e grupos: todos os grupos** , clique em **configurar grupos para este site**.</span><span class="sxs-lookup"><span data-stu-id="b9074-152">On the **People and Groups: All Groups** page, click **Set Up Groups for this Site**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > <span data-ttu-id="b9074-153">Você pode receber um erro <strong>HTTP 404 não encontrado</strong> devido a um bug de codificação http duplo.</span><span class="sxs-lookup"><span data-stu-id="b9074-153">You may receive an <strong>HTTP 404 Not Found</strong> error due to a double HTTP encoding bug.</span></span> <span data-ttu-id="b9074-154">Se isso ocorrer, substitua a URL por:</span><span class="sxs-lookup"><span data-stu-id="b9074-154">If this occurs, replace the URL with this:</span></span>   
   > <span data-ttu-id="b9074-155">`[site_collection_URL]/_layouts/permsetup.aspx` Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="b9074-155">`[site_collection_URL]/_layouts/permsetup.aspx` For example:</span></span>  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. <span data-ttu-id="b9074-156">Na página **configurar grupos para este site** , adicione o usuário que criará projetos de equipe ao grupo **proprietários** e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="b9074-156">On the **Set Up Groups for this Site** page, add the user who will create team projects to the **Owners** group, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image10.png)

<span data-ttu-id="b9074-157">Para obter mais informações sobre como permitir que os usuários criem novos projetos de equipe dentro de uma coleção de projetos de equipe, consulte [definir permissões de administrador para coleções de projetos de equipe](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9074-157">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span>

## <a name="create-a-new-team-project-and-add-users"></a><span data-ttu-id="b9074-158">Criar um novo projeto de equipe e adicionar usuários</span><span class="sxs-lookup"><span data-stu-id="b9074-158">Create a New Team Project and Add Users</span></span>

<span data-ttu-id="b9074-159">Depois de ter as permissões necessárias, você pode usar a janela **Team Explorer** no Visual Studio 2010 para criar um novo projeto de equipe.</span><span class="sxs-lookup"><span data-stu-id="b9074-159">Once you have the necessary permissions, you can use the **Team Explorer** window in Visual Studio 2010 to create a new team project.</span></span> <span data-ttu-id="b9074-160">Essa abordagem fornece um assistente que coleta todas as informações necessárias e executa as tarefas necessárias no TFS, no SharePoint e no SQL Server Reporting Services.</span><span class="sxs-lookup"><span data-stu-id="b9074-160">This approach provides a wizard that collects all the required information and performs the necessary tasks in TFS, SharePoint, and SQL Server Reporting Services.</span></span> <span data-ttu-id="b9074-161">Você também precisará conceder permissões no novo projeto de equipe aos membros da equipe do desenvolvedor, para permitir que eles adicionem e modifiquem o conteúdo.</span><span class="sxs-lookup"><span data-stu-id="b9074-161">You'll also need to grant permissions on the new team project to members of the developer team, to enable them to add and modify content.</span></span>

### <a name="who-performs-these-procedures"></a><span data-ttu-id="b9074-162">Quem executa esses procedimentos?</span><span class="sxs-lookup"><span data-stu-id="b9074-162">Who Performs These Procedures?</span></span>

<span data-ttu-id="b9074-163">Geralmente, um administrador do TFS ou um líder da equipe do desenvolvedor executa esses procedimentos.</span><span class="sxs-lookup"><span data-stu-id="b9074-163">Usually either a TFS administrator or a developer team leader performs these procedures.</span></span>

### <a name="create-a-new-team-project"></a><span data-ttu-id="b9074-164">Criar um novo projeto de equipe</span><span class="sxs-lookup"><span data-stu-id="b9074-164">Create a New Team Project</span></span>

<span data-ttu-id="b9074-165">O procedimento a seguir descreve como criar um novo projeto de equipe no TFS 2010.</span><span class="sxs-lookup"><span data-stu-id="b9074-165">The next procedure describes how to create a new team project in TFS 2010.</span></span>

<span data-ttu-id="b9074-166">**Para criar um novo projeto de equipe**</span><span class="sxs-lookup"><span data-stu-id="b9074-166">**To create a new team project**</span></span>

1. <span data-ttu-id="b9074-167">No menu **Iniciar** , aponte para **todos os programas**, clique **em Microsoft Visual Studio 2010**, clique com o botão direito do mouse em **Microsoft Visual Studio 2010**e clique em **Executar como administrador**.</span><span class="sxs-lookup"><span data-stu-id="b9074-167">On the **Start** menu, point to **All Programs**, click **Microsoft Visual Studio 2010**, right-click **Microsoft Visual Studio 2010**, and then click **Run as administrator**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b9074-168">Se você não executar o Visual Studio 2010 como administrador, o assistente do novo projeto de equipe falhará na última etapa.</span><span class="sxs-lookup"><span data-stu-id="b9074-168">If you don't run Visual Studio 2010 as an administrator, the New Team Project Wizard will fail on the last step.</span></span>
2. <span data-ttu-id="b9074-169">(Se a caixa de diálogo **Controle de Conta de Usuário** aparecer, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="b9074-169">If the **User Account Control** dialog box appears, click **Yes**.</span></span>
3. <span data-ttu-id="b9074-170">No Visual Studio, no menu **equipe** , clique em **conectar-se a Team Foundation Server**.</span><span class="sxs-lookup"><span data-stu-id="b9074-170">In Visual Studio, on the **Team** menu, click **Connect to Team Foundation Server**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b9074-171">Se você já tiver configurado uma conexão com um servidor TFS, poderá omitir as etapas 4-7.</span><span class="sxs-lookup"><span data-stu-id="b9074-171">If you have already configured a connection to a TFS server, you can omit steps 4-7.</span></span>
4. <span data-ttu-id="b9074-172">Na caixa de diálogo **conexão com o projeto de equipe** , clique em **servidores**.</span><span class="sxs-lookup"><span data-stu-id="b9074-172">In the **Connection to Team Project** dialog box, click **Servers**.</span></span>
5. <span data-ttu-id="b9074-173">Na caixa de diálogo **Adicionar/remover Team Foundation Server** , clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b9074-173">In the **Add/Remove Team Foundation Server** dialog box, click **Add**.</span></span>
6. <span data-ttu-id="b9074-174">Na caixa de diálogo **adicionar Team Foundation Server** , forneça os detalhes da sua instância do TFS e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="b9074-174">In the **Add Team Foundation Server** dialog box, provide the details of your TFS instance, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. <span data-ttu-id="b9074-175">Na caixa de diálogo **Adicionar/remover Team Foundation Server** , clique em **fechar**.</span><span class="sxs-lookup"><span data-stu-id="b9074-175">In the **Add/Remove Team Foundation Server** dialog box, click **Close**.</span></span>
8. <span data-ttu-id="b9074-176">Na caixa de diálogo **conectar ao projeto de equipe** , selecione a instância do TFS à qual você deseja se conectar, selecione a coleção de projetos de equipe à qual você deseja adicionar e clique em **conectar**.</span><span class="sxs-lookup"><span data-stu-id="b9074-176">In the **Connect to Team Project** dialog box, select the TFS instance you want to connect to, select the team project collection you want to add to, and then click **Connect**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. <span data-ttu-id="b9074-177">Na janela **Team Explorer** , clique com o botão direito do mouse na coleção de projetos de equipe e clique em **novo projeto de equipe**.</span><span class="sxs-lookup"><span data-stu-id="b9074-177">In the **Team Explorer** window, right-click the team project collection, and then click **New Team Project**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. <span data-ttu-id="b9074-178">Na caixa de diálogo **novo projeto de equipe** , forneça um nome e uma descrição para o projeto de equipe e clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="b9074-178">In the **New Team Project** dialog box, provide a name and a description for the team project, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b9074-179">Se o seu projeto de equipe incluir espaços, você poderá enfrentar alguns problemas quando vier a usar a Implantação da Web ferramenta de implantação da Web do Serviços de Informações da Internet (IIS) para implantar pacotes do caminho de saída.</span><span class="sxs-lookup"><span data-stu-id="b9074-179">If your team project includes spaces, you may face some issues when you come to use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy packages from the output path.</span></span> <span data-ttu-id="b9074-180">Os espaços no caminho podem dificultar muito a execução de Implantação da Web comandos.</span><span class="sxs-lookup"><span data-stu-id="b9074-180">Spaces in the path can make it a lot more difficult to run Web Deploy commands.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. <span data-ttu-id="b9074-181">Na página **selecionar um modelo de processo** , selecione o modelo de processo que você deseja usar para gerenciar o processo de desenvolvimento e clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="b9074-181">On the **Select a Process Template** page, select the process template that you want to use to manage the development process, and then click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b9074-182">Para obter mais informações sobre modelos de processo do TFS, consulte [modelos e ferramentas de processo](https://msdn.microsoft.com/vstudio/aa718795).</span><span class="sxs-lookup"><span data-stu-id="b9074-182">For more information on process templates for TFS, see [Process Templates and Tools](https://msdn.microsoft.com/vstudio/aa718795).</span></span>
12. <span data-ttu-id="b9074-183">Na página **configurações do site de equipe** , deixe as configurações padrão inalteradas e clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="b9074-183">On the **Team Site Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
13. <span data-ttu-id="b9074-184">Essa configuração cria ou identifica um site de equipe do SharePoint associado ao projeto de equipe do TFS.</span><span class="sxs-lookup"><span data-stu-id="b9074-184">This setting creates, or identifies, a SharePoint team site that is associated with the TFS team project.</span></span> <span data-ttu-id="b9074-185">Sua equipe de desenvolvimento pode usar este site para gerenciar a documentação, participar de threads de discussão, criar páginas wiki e executar várias outras tarefas que não estão relacionadas ao código.</span><span class="sxs-lookup"><span data-stu-id="b9074-185">Your development team can use this site to manage documentation, participate in discussion threads, create wiki pages, and perform various other tasks that are not related to code.</span></span> <span data-ttu-id="b9074-186">Para obter mais informações, consulte [interações entre produtos e Team Foundation Server do SharePoint](https://msdn.microsoft.com/library/ms253177.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9074-186">For more information, see [Interactions Between SharePoint Products and Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).</span></span>
14. <span data-ttu-id="b9074-187">Na página **especificar configurações de controle do código-fonte** , deixe as configurações padrão inalteradas e clique em **Avançar**.</span><span class="sxs-lookup"><span data-stu-id="b9074-187">On the **Specify Source Control Settings** page, leave the default settings unchanged, and then click **Next**.</span></span>
15. <span data-ttu-id="b9074-188">Essa configuração identifica ou cria o local na hierarquia de pastas do TFS que atuará como uma pasta raiz para seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="b9074-188">This setting identifies or creates the location in the TFS folder hierarchy that will act as a root folder for your content.</span></span>
16. <span data-ttu-id="b9074-189">Na página **confirmar as configurações do projeto de equipe** , clique em **concluir**.</span><span class="sxs-lookup"><span data-stu-id="b9074-189">On the **Confirm Team Project Settings** page, click **Finish**.</span></span>
17. <span data-ttu-id="b9074-190">Quando o novo projeto de equipe for criado com êxito, na página **projeto de equipe criado** , clique em **fechar**.</span><span class="sxs-lookup"><span data-stu-id="b9074-190">When the new team project is successfully created, on the **Team Project Created** page, click **Close**.</span></span>

### <a name="add-users-to-a-team-project"></a><span data-ttu-id="b9074-191">Adicionar usuários a um projeto de equipe</span><span class="sxs-lookup"><span data-stu-id="b9074-191">Add Users to a Team Project</span></span>

<span data-ttu-id="b9074-192">Agora que você criou o novo projeto de equipe, você pode conceder permissões aos usuários para habilitá-los a começar a adicionar e colaborar no conteúdo.</span><span class="sxs-lookup"><span data-stu-id="b9074-192">Now that you've created the new team project, you can grant permissions to users to enable them to start adding and collaborating on content.</span></span>

<span data-ttu-id="b9074-193">**Para adicionar usuários a um projeto de equipe**</span><span class="sxs-lookup"><span data-stu-id="b9074-193">**To add users to a team project**</span></span>

1. <span data-ttu-id="b9074-194">No Visual Studio 2010, na janela **Team Explorer** , clique com o botão direito do mouse no projeto de equipe, aponte para **configurações do projeto de equipe**e clique em associação de **grupo**.</span><span class="sxs-lookup"><span data-stu-id="b9074-194">In Visual Studio 2010, in the **Team Explorer** window, right-click the team project, point to **Team Project Settings**, and then click **Group Membership**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. <span data-ttu-id="b9074-195">Para permitir que um usuário adicione, modifique e remova código sob controle do código-fonte, você precisa adicioná-lo ao grupo **colaboradores** .</span><span class="sxs-lookup"><span data-stu-id="b9074-195">To enable a user to add, modify, and remove code under source control, you need to add him or her to the **Contributors** group.</span></span>
3. <span data-ttu-id="b9074-196">Na caixa de diálogo **grupos de projetos** , selecione o grupo **colaboradores** e clique em **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="b9074-196">In the **Project Groups** dialog box, select the **Contributors** group, and then click **Properties**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. <span data-ttu-id="b9074-197">Na caixa de diálogo **Propriedades do grupo de Team Foundation Server** , selecione **usuário ou grupo do Windows**e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="b9074-197">In the **Team Foundation Server Group Properties** dialog box, select **Windows User or Group**, and then click **Add**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. <span data-ttu-id="b9074-198">Na caixa de diálogo **Selecionar usuários, computadores ou grupos** , digite o nome de usuário do usuário que você deseja adicionar ao projeto de equipe, clique em **verificar nomes**e, em seguida, clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="b9074-198">In the **Select Users, Computers, or Groups** dialog box, type the user name of the user you want to add to the team project, click **Check Names**, and then click **OK**.</span></span>

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. <span data-ttu-id="b9074-199">Na caixa de diálogo **Propriedades do grupo de Team Foundation Server** , clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="b9074-199">In the **Team Foundation Server Group Properties** dialog box, click **OK**.</span></span>
7. <span data-ttu-id="b9074-200">Na caixa de diálogo **grupos de projetos** , clique em **fechar**.</span><span class="sxs-lookup"><span data-stu-id="b9074-200">In the **Project Groups** dialog box, click **Close**.</span></span>

## <a name="conclusion"></a><span data-ttu-id="b9074-201">Conclusão</span><span class="sxs-lookup"><span data-stu-id="b9074-201">Conclusion</span></span>

<span data-ttu-id="b9074-202">Neste ponto, seu novo projeto de equipe está pronto para uso, e sua equipe de desenvolvedores pode começar a adicionar conteúdo e colaborar no processo de desenvolvimento.</span><span class="sxs-lookup"><span data-stu-id="b9074-202">At this point, your new team project is ready to use, and your developer team can start adding content and collaborating on the development process.</span></span>

<span data-ttu-id="b9074-203">O próximo tópico, [adicionando conteúdo ao controle do código-fonte](adding-content-to-source-control.md), descreve como adicionar conteúdo ao controle do código-fonte.</span><span class="sxs-lookup"><span data-stu-id="b9074-203">The next topic, [Adding Content to Source Control](adding-content-to-source-control.md), describes how to add content to source control.</span></span>

## <a name="further-reading"></a><span data-ttu-id="b9074-204">Leitura adicional</span><span class="sxs-lookup"><span data-stu-id="b9074-204">Further Reading</span></span>

<span data-ttu-id="b9074-205">Para obter uma orientação mais ampla sobre a criação de projetos de equipe no TFS, consulte [criar um projeto de equipe](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="b9074-205">For broader guidance on creating team projects in TFS, see [Create a Team Project](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx).</span></span> <span data-ttu-id="b9074-206">Para obter mais informações sobre como permitir que os usuários criem novos projetos de equipe dentro de uma coleção de projetos de equipe, consulte [definir permissões de administrador para coleções de projetos de equipe](https://msdn.microsoft.com/library/dd547204.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9074-206">For more information on enabling users to create new team projects within a team project collection, see [Set Administrator Permissions for Team Project Collections](https://msdn.microsoft.com/library/dd547204.aspx).</span></span> <span data-ttu-id="b9074-207">Para obter mais informações sobre como adicionar usuários a projetos de equipe, consulte [Adicionar usuários a projetos de equipe](https://msdn.microsoft.com/library/bb558971.aspx).</span><span class="sxs-lookup"><span data-stu-id="b9074-207">For more information on adding users to team projects, see [Add Users to Team Projects](https://msdn.microsoft.com/library/bb558971.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b9074-208">[Anterior](configuring-team-foundation-server-for-web-deployment.md)
> [Próximo](adding-content-to-source-control.md)</span><span class="sxs-lookup"><span data-stu-id="b9074-208">[Previous](configuring-team-foundation-server-for-web-deployment.md)
[Next](adding-content-to-source-control.md)</span></span>
