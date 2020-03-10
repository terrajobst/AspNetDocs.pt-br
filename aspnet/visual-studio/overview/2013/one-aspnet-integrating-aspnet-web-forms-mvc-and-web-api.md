---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Laboratório prático: um ASP.NET: integrando o ASP.NET Web Forms, MVC e API da Web | Microsoft Docs'
author: rick-anderson
description: O ASP.NET é uma estrutura para a criação de sites, aplicativos e serviços usando tecnologias especializadas, como MVC, API Web e outros. Com a expansão ASP.NET h...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 165d104b5d3ef3281af449cc8673ad96f531d628
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78623196"
---
# <a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="55c24-104">Laboratório prático: um ASP.NET: integrando o ASP.NET Web Forms, MVC e API da Web</span><span class="sxs-lookup"><span data-stu-id="55c24-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>

<span data-ttu-id="55c24-105">por [equipe de acampamentos da Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="55c24-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="55c24-106">Baixe o kit de treinamento do Web acampamentos</span><span class="sxs-lookup"><span data-stu-id="55c24-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

> <span data-ttu-id="55c24-107">O ASP.NET é uma estrutura para a criação de sites, aplicativos e serviços usando tecnologias especializadas, como MVC, API Web e outros.</span><span class="sxs-lookup"><span data-stu-id="55c24-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="55c24-108">Com o ASP.NET de expansão visto desde sua criação e a expressa necessidade de ter essas tecnologias integradas, houve esforços recentes em trabalhar em direção a **uma ASP.net**.</span><span class="sxs-lookup"><span data-stu-id="55c24-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="55c24-109">Visual Studio 2013 introduz um novo sistema de projeto unificado que permite criar um aplicativo e usar todas as tecnologias ASP.NET em um projeto.</span><span class="sxs-lookup"><span data-stu-id="55c24-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="55c24-110">Esse recurso elimina a necessidade de escolher uma tecnologia no início de um projeto e aderir a ela e, em vez disso, incentiva o uso de várias estruturas ASP.NET dentro de um projeto.</span><span class="sxs-lookup"><span data-stu-id="55c24-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="55c24-111">Todos os códigos de exemplo e trechos de código estão incluídos no kit de treinamento do acampamentos da Web, disponível em [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="55c24-111">All sample code and snippets are included in the Web Camps Training Kit, available at [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span></span>

<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="55c24-112">Visão geral</span><span class="sxs-lookup"><span data-stu-id="55c24-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="55c24-113">Objetivos</span><span class="sxs-lookup"><span data-stu-id="55c24-113">Objectives</span></span>

<span data-ttu-id="55c24-114">Neste laboratório prático, você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="55c24-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="55c24-115">Criar um site com base em um tipo de projeto **ASP.net**</span><span class="sxs-lookup"><span data-stu-id="55c24-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="55c24-116">Usar estruturas **ASP.net** diferentes, como **MVC** e **API Web** , no mesmo projeto</span><span class="sxs-lookup"><span data-stu-id="55c24-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="55c24-117">Identificar os principais componentes de um aplicativo **ASP.net**</span><span class="sxs-lookup"><span data-stu-id="55c24-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="55c24-118">Aproveite a estrutura **ASP.net scaffolding** para criar automaticamente controladores e exibições para executar operações CRUD com base em suas classes de modelo</span><span class="sxs-lookup"><span data-stu-id="55c24-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="55c24-119">Expor o mesmo conjunto de informações em formatos legíveis para computador e humano usando a ferramenta certa para cada trabalho</span><span class="sxs-lookup"><span data-stu-id="55c24-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="55c24-120">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="55c24-120">Prerequisites</span></span>

<span data-ttu-id="55c24-121">O seguinte é necessário para concluir este laboratório prático:</span><span class="sxs-lookup"><span data-stu-id="55c24-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="55c24-122">[Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/) ou superior</span><span class="sxs-lookup"><span data-stu-id="55c24-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="55c24-123">Visual Studio 2013 Atualização 1</span><span class="sxs-lookup"><span data-stu-id="55c24-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="55c24-124">Instalação</span><span class="sxs-lookup"><span data-stu-id="55c24-124">Setup</span></span>

<span data-ttu-id="55c24-125">Para executar os exercícios neste laboratório prático, você precisará configurar seu ambiente primeiro...</span><span class="sxs-lookup"><span data-stu-id="55c24-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="55c24-126">Abra o Windows Explorer e navegue até a pasta de **origem** do laboratório.</span><span class="sxs-lookup"><span data-stu-id="55c24-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="55c24-127">Clique com o botão direito do mouse em **Setup. cmd** e selecione **Executar como administrador** para iniciar o processo de instalação que irá configurar seu ambiente e instalar os trechos de código do Visual Studio para este laboratório.</span><span class="sxs-lookup"><span data-stu-id="55c24-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="55c24-128">Se a caixa de diálogo controle de conta de usuário for exibida, confirme a ação para continuar.</span><span class="sxs-lookup"><span data-stu-id="55c24-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="55c24-129">Verifique se você verificou todas as dependências deste laboratório antes de executar a instalação.</span><span class="sxs-lookup"><span data-stu-id="55c24-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="55c24-130">Usando os trechos de código</span><span class="sxs-lookup"><span data-stu-id="55c24-130">Using the Code Snippets</span></span>

<span data-ttu-id="55c24-131">Em todo o documento do laboratório, você será instruído a inserir blocos de código.</span><span class="sxs-lookup"><span data-stu-id="55c24-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="55c24-132">Para sua conveniência, a maior parte desse código é fornecida como trechos de Visual Studio Code, que podem ser acessados em Visual Studio 2013 para evitar a necessidade de adicioná-lo manualmente.</span><span class="sxs-lookup"><span data-stu-id="55c24-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="55c24-133">Cada exercício é acompanhado por uma solução inicial localizada na pasta **begin** do exercício que permite que você siga cada exercício independentemente dos outros.</span><span class="sxs-lookup"><span data-stu-id="55c24-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="55c24-134">Lembre-se de que os trechos de código que são adicionados durante um exercício estão ausentes dessas soluções iniciais e podem não funcionar até que você conclua o exercício.</span><span class="sxs-lookup"><span data-stu-id="55c24-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="55c24-135">Dentro do código-fonte de um exercício, você também encontrará uma pasta **final** contendo uma solução do Visual Studio com o código que resulta da conclusão das etapas no exercício correspondente.</span><span class="sxs-lookup"><span data-stu-id="55c24-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="55c24-136">Você pode usar essas soluções como diretrizes se precisar de ajuda adicional ao trabalhar com este laboratório prático.</span><span class="sxs-lookup"><span data-stu-id="55c24-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>

---

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="55c24-137">Exercícios</span><span class="sxs-lookup"><span data-stu-id="55c24-137">Exercises</span></span>

<span data-ttu-id="55c24-138">Este laboratório prático inclui os seguintes exercícios:</span><span class="sxs-lookup"><span data-stu-id="55c24-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="55c24-139">Criando um novo projeto de Web Forms</span><span class="sxs-lookup"><span data-stu-id="55c24-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="55c24-140">Criando um controlador MVC usando scaffolding</span><span class="sxs-lookup"><span data-stu-id="55c24-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="55c24-141">Criando um controlador de API Web usando scaffolding</span><span class="sxs-lookup"><span data-stu-id="55c24-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="55c24-142">Tempo estimado para concluir este laboratório: **60 minutos**</span><span class="sxs-lookup"><span data-stu-id="55c24-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="55c24-143">Ao iniciar o Visual Studio pela primeira vez, você deve selecionar uma das coleções de configurações predefinidas.</span><span class="sxs-lookup"><span data-stu-id="55c24-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="55c24-144">Cada coleção predefinida é projetada para corresponder a um estilo de desenvolvimento específico e determina layouts de janela, comportamento do editor, trechos de código IntelliSense e opções da caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="55c24-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="55c24-145">Os procedimentos neste laboratório descrevem as ações necessárias para realizar uma determinada tarefa no Visual Studio ao usar a coleção de **configurações de desenvolvimento geral** .</span><span class="sxs-lookup"><span data-stu-id="55c24-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="55c24-146">Se você escolher uma coleção de configurações diferentes para seu ambiente de desenvolvimento, poderá haver diferenças nas etapas que você deve levar em conta.</span><span class="sxs-lookup"><span data-stu-id="55c24-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>

<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="55c24-147">Exercício 1: Criando um novo projeto de Web Forms</span><span class="sxs-lookup"><span data-stu-id="55c24-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="55c24-148">Neste exercício, você criará um novo site Web Forms no Visual Studio 2013 usando a experiência de projeto unificada **ASP.net** , que permitirá que você integre facilmente os componentes Web Forms, MVC e API Web no mesmo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="55c24-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="55c24-149">Em seguida, você irá explorar a solução gerada e identificar suas partes e, por fim, verá o site em ação.</span><span class="sxs-lookup"><span data-stu-id="55c24-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="55c24-150">Tarefa 1 – Criando um novo site usando uma experiência ASP.NET</span><span class="sxs-lookup"><span data-stu-id="55c24-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="55c24-151">Nesta tarefa, você começará a criar um novo site no Visual Studio com base em **um** tipo de projeto ASP.net.</span><span class="sxs-lookup"><span data-stu-id="55c24-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="55c24-152">**Uma ASP.net** unifica todas as tecnologias de ASP.net e oferece a opção de misturar e CORRESP-las conforme desejado.</span><span class="sxs-lookup"><span data-stu-id="55c24-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="55c24-153">Em seguida, você reconhecerá os diferentes componentes do Web Forms, MVC e API Web que residem lado a lado em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="55c24-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="55c24-154">Abra **Visual Studio Express 2013 para Web** e selecione **arquivo | Novo projeto...** para iniciar uma nova solução.</span><span class="sxs-lookup"><span data-stu-id="55c24-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![Criando um novo projeto](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="55c24-156">*Criando um novo projeto*</span><span class="sxs-lookup"><span data-stu-id="55c24-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="55c24-157">Na caixa de diálogo **novo projeto** , selecione **ASP.NET aplicativo Web** no **Visual C# |** Na guia Web e verifique se **.NET Framework 4,5** está selecionado.</span><span class="sxs-lookup"><span data-stu-id="55c24-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="55c24-158">Nomeie o projeto *MyHybridSite*, escolha um **local** e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="55c24-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![Novo projeto de aplicativo Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="55c24-160">*Criando um novo projeto de aplicativo Web ASP.NET*</span><span class="sxs-lookup"><span data-stu-id="55c24-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="55c24-161">Na caixa de diálogo **novo projeto ASP.net** , selecione o modelo de **Web Forms** e selecione as opções **MVC** e **API da Web** .</span><span class="sxs-lookup"><span data-stu-id="55c24-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="55c24-162">Além disso, verifique se a opção de **autenticação** está definida para **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="55c24-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="55c24-163">Clique em **OK** para continuar.</span><span class="sxs-lookup"><span data-stu-id="55c24-163">Click **OK** to continue.</span></span>

    ![Criando um novo projeto com o modelo de Web Forms, incluindo componentes da API Web e MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="55c24-165">*Criando um novo projeto com o modelo de Web Forms, incluindo componentes da API Web e MVC*</span><span class="sxs-lookup"><span data-stu-id="55c24-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="55c24-166">Agora você pode explorar a estrutura da solução gerada.</span><span class="sxs-lookup"><span data-stu-id="55c24-166">You can now explore the structure of the generated solution.</span></span>

    ![Explorando a solução gerada](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="55c24-168">*Explorando a solução gerada*</span><span class="sxs-lookup"><span data-stu-id="55c24-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="55c24-169">**Conta:** Essa pasta contém as páginas de formulário da Web para registrar, fazer logon e gerenciar as contas de usuário do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="55c24-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="55c24-170">Essa pasta é adicionada quando a opção de autenticação de **contas de usuário individuais** é selecionada durante a configuração do modelo de projeto Web Forms.</span><span class="sxs-lookup"><span data-stu-id="55c24-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="55c24-171">**Modelos:** Essa pasta conterá as classes que representam os dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="55c24-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="55c24-172">**Controladores** e **exibições**: essas pastas são necessárias para os componentes **ASP.NET MVC** e **ASP.NET Web API** .</span><span class="sxs-lookup"><span data-stu-id="55c24-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="55c24-173">Você explorará as tecnologias MVC e API da Web nos próximos exercícios.</span><span class="sxs-lookup"><span data-stu-id="55c24-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="55c24-174">Os arquivos **Default. aspx**, **Contact. aspx** e **about. aspx** são páginas de formulário da Web predefinidas que você pode usar como pontos de partida para criar as páginas específicas ao seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="55c24-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="55c24-175">A lógica de programação desses arquivos reside em um arquivo separado referido como o &quot;&quot; arquivo de código, que tem uma extensão &quot;. aspx. vb&quot; ou &quot;. aspx.cs&quot; (dependendo do idioma usado).</span><span class="sxs-lookup"><span data-stu-id="55c24-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="55c24-176">A lógica code-behind é executada no servidor e produz dinamicamente a saída HTML para sua página.</span><span class="sxs-lookup"><span data-stu-id="55c24-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="55c24-177">As páginas **site. Master** e **site. Mobile. Master** definem a aparência e o comportamento padrão de todas as páginas no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="55c24-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="55c24-178">Clique duas vezes no arquivo **Default. aspx** para explorar o conteúdo da página.</span><span class="sxs-lookup"><span data-stu-id="55c24-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Explorando a página default. aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="55c24-180">*Explorando a página default. aspx*</span><span class="sxs-lookup"><span data-stu-id="55c24-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="55c24-181">A diretiva **Page** na parte superior do arquivo define os atributos da página Web Forms.</span><span class="sxs-lookup"><span data-stu-id="55c24-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="55c24-182">Por exemplo, o atributo **MasterPageFile** especifica o caminho para a página mestra – nesse caso, a página *site. Master* -e o atributo **Inherits** definem a classe code-behind da página a ser herdada.</span><span class="sxs-lookup"><span data-stu-id="55c24-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="55c24-183">Essa classe está localizada no arquivo determinado pelo atributo **Codebehind** .</span><span class="sxs-lookup"><span data-stu-id="55c24-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="55c24-184">O controle **asp: content** contém o conteúdo real da página (texto, marcação e controles) e é mapeado para um controle **asp: ContentPlaceHolder** na página mestra.</span><span class="sxs-lookup"><span data-stu-id="55c24-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="55c24-185">Nesse caso, o conteúdo da página será renderizado dentro do controle *mainContent* definido na página *site. Master* .</span><span class="sxs-lookup"><span data-stu-id="55c24-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="55c24-186">Expanda o **aplicativo\_pasta inicial** e observe o arquivo **WebApiConfig.cs** .</span><span class="sxs-lookup"><span data-stu-id="55c24-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="55c24-187">O Visual Studio incluiu esse arquivo na solução gerada porque você incluiu a API da Web ao configurar seu projeto com o modelo One ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="55c24-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="55c24-188">Abra o arquivo **WebApiConfig.cs** .</span><span class="sxs-lookup"><span data-stu-id="55c24-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="55c24-189">Na classe *WebApiConfig* , você encontrará a configuração associada à API Web, que mapeia as rotas http para os **controladores da API Web**.</span><span class="sxs-lookup"><span data-stu-id="55c24-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="55c24-190">Abra o arquivo **RouteConfig.cs** .</span><span class="sxs-lookup"><span data-stu-id="55c24-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="55c24-191">Dentro do método *RegisterRoutes* , você encontrará a configuração associada ao MVC, que mapeia as rotas http para **controladores MVC**.</span><span class="sxs-lookup"><span data-stu-id="55c24-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="55c24-192">Tarefa 2 – executando a solução</span><span class="sxs-lookup"><span data-stu-id="55c24-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="55c24-193">Nesta tarefa, você executará a solução gerada, explorará o aplicativo e alguns de seus recursos, como a regravação de URL e a autenticação interna.</span><span class="sxs-lookup"><span data-stu-id="55c24-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="55c24-194">Para executar a solução, pressione **F5** ou clique no botão **Iniciar** localizado na barra de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="55c24-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="55c24-195">O home page do aplicativo deve ser aberto no navegador.</span><span class="sxs-lookup"><span data-stu-id="55c24-195">The application home page should open in the browser.</span></span>

    ![Executando a solução](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="55c24-197">Verifique se as páginas Web Forms estão sendo invocadas.</span><span class="sxs-lookup"><span data-stu-id="55c24-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="55c24-198">Para fazer isso, acrescente **/Contact.aspx** à URL na barra de endereços e pressione **Enter**.</span><span class="sxs-lookup"><span data-stu-id="55c24-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![URLs amigáveis](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="55c24-200">*URLs amigáveis*</span><span class="sxs-lookup"><span data-stu-id="55c24-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="55c24-201">Como você pode ver, a URL muda para **/Contact**.</span><span class="sxs-lookup"><span data-stu-id="55c24-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="55c24-202">A partir do **ASP.NET 4**, os recursos de roteamento de URL foram adicionados ao Web Forms, para que você possa gravar URLs como *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* em vez de *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)* .</span><span class="sxs-lookup"><span data-stu-id="55c24-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="55c24-203">Para obter mais informações, consulte [Roteamento de URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span><span class="sxs-lookup"><span data-stu-id="55c24-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="55c24-204">Agora, você explorará o fluxo de autenticação integrado ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="55c24-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="55c24-205">Para fazer isso, clique em **registrar** no canto superior direito da página.</span><span class="sxs-lookup"><span data-stu-id="55c24-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![Registrando um novo usuário](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="55c24-207">*Registrando um novo usuário*</span><span class="sxs-lookup"><span data-stu-id="55c24-207">*Registering a new user*</span></span>
4. <span data-ttu-id="55c24-208">Na página **registrar** , insira um **nome de usuário** e **senha**e, em seguida, clique em **registrar**.</span><span class="sxs-lookup"><span data-stu-id="55c24-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![Página de registro](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="55c24-210">*Página de registro*</span><span class="sxs-lookup"><span data-stu-id="55c24-210">*Register page*</span></span>
5. <span data-ttu-id="55c24-211">O aplicativo registra a nova conta e o usuário é autenticado.</span><span class="sxs-lookup"><span data-stu-id="55c24-211">The application registers the new account, and the user is authenticated.</span></span>

    ![Usuário autenticado](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="55c24-213">*Usuário autenticado*</span><span class="sxs-lookup"><span data-stu-id="55c24-213">*User authenticated*</span></span>
6. <span data-ttu-id="55c24-214">Volte para o Visual Studio e pressione **Shift + F5** para parar a depuração.</span><span class="sxs-lookup"><span data-stu-id="55c24-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="55c24-215">Exercício 2: Criando um controlador MVC usando o scaffolding</span><span class="sxs-lookup"><span data-stu-id="55c24-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="55c24-216">Neste exercício, você aproveitará a estrutura ASP.NET scaffolding fornecida pelo Visual Studio para criar um controlador ASP.NET MVC 5 com ações e exibições do Razor para executar operações CRUD, sem escrever uma única linha de código.</span><span class="sxs-lookup"><span data-stu-id="55c24-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="55c24-217">O processo scaffolding usará Entity Framework Code First para gerar o contexto de dados e o esquema de banco de dado no banco de dados SQL.</span><span class="sxs-lookup"><span data-stu-id="55c24-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="55c24-218">**Sobre Entity Framework Code First**</span><span class="sxs-lookup"><span data-stu-id="55c24-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="55c24-219">Entity Framework (EF) é um mapeador relacional de objeto (ORM) que permite que você crie aplicativos de acesso a dados programando com um modelo de aplicativo conceitual em vez de programar diretamente usando um esquema de armazenamento relacional.</span><span class="sxs-lookup"><span data-stu-id="55c24-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="55c24-220">O fluxo de trabalho de modelagem de Code First de Entity Framework permite que você use suas próprias classes de domínio para representar o modelo que o EF depende ao executar consultas, controle de alterações e atualização de funções.</span><span class="sxs-lookup"><span data-stu-id="55c24-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="55c24-221">Usando o fluxo de trabalho de desenvolvimento Code First, você não precisa iniciar seu aplicativo Criando um banco de dados ou especificando um esquema.</span><span class="sxs-lookup"><span data-stu-id="55c24-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="55c24-222">Em vez disso, você pode escrever classes .NET padrão que definem os objetos de modelo de domínio mais apropriados para seu aplicativo e Entity Framework criará o banco de dados para você.</span><span class="sxs-lookup"><span data-stu-id="55c24-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="55c24-223">Você pode saber mais sobre Entity Framework [aqui](../../../entity-framework.md).</span><span class="sxs-lookup"><span data-stu-id="55c24-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="55c24-224">Tarefa 1 – Criando um novo modelo</span><span class="sxs-lookup"><span data-stu-id="55c24-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="55c24-225">Agora você vai definir uma classe **Person** , que será o modelo usado pelo processo scaffolding para criar o controlador MVC e as exibições.</span><span class="sxs-lookup"><span data-stu-id="55c24-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="55c24-226">Você começará criando uma classe de modelo **Person** e as operações CRUD no controlador serão criadas automaticamente usando os recursos do scaffolding.</span><span class="sxs-lookup"><span data-stu-id="55c24-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="55c24-227">Abra **Visual Studio Express 2013 para Web** e a solução **MyHybridSite. sln** localizada na pasta **Source/EX2-MvcScaffolding/Begin** .</span><span class="sxs-lookup"><span data-stu-id="55c24-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="55c24-228">Como alternativa, você pode continuar com a solução que você obteve no exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="55c24-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="55c24-229">Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta **modelos** do projeto **MyHybridSite** e selecione **Adicionar | Classe...** .</span><span class="sxs-lookup"><span data-stu-id="55c24-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Adicionando a classe de modelo Person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="55c24-231">*Adicionando a classe de modelo Person*</span><span class="sxs-lookup"><span data-stu-id="55c24-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="55c24-232">Na caixa de diálogo **Adicionar novo item** , nomeie o arquivo *Person.cs* e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="55c24-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Criando a classe de modelo Person](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="55c24-234">*Criando a classe de modelo Person*</span><span class="sxs-lookup"><span data-stu-id="55c24-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="55c24-235">Substitua o conteúdo do arquivo **Person.cs** pelo código a seguir.</span><span class="sxs-lookup"><span data-stu-id="55c24-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="55c24-236">Pressione **Ctrl + S** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="55c24-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="55c24-237">(Trecho de código- *BringingTogetherOneAspNet-EX2-PersonClass*)</span><span class="sxs-lookup"><span data-stu-id="55c24-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="55c24-238">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **MyHybridSite** e selecione **COMPILAR**ou pressione **Ctrl + Shift + B** para compilar o projeto.</span><span class="sxs-lookup"><span data-stu-id="55c24-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="55c24-239">Tarefa 2 – Criando um controlador MVC</span><span class="sxs-lookup"><span data-stu-id="55c24-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="55c24-240">Agora que o modelo **Person** foi criado, você usará o ASP.NET MVC scaffolding com Entity Framework para criar as ações do controlador CRUD e exibições para **Person**.</span><span class="sxs-lookup"><span data-stu-id="55c24-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="55c24-241">Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta **controladores** do projeto **MyHybridSite** e selecione **Adicionar | Novo item de com Scaffold...**</span><span class="sxs-lookup"><span data-stu-id="55c24-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Criando um novo controlador com Scaffold](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="55c24-243">*Criando um novo controlador com Scaffold*</span><span class="sxs-lookup"><span data-stu-id="55c24-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="55c24-244">Na caixa de diálogo **Adicionar Scaffold** , selecione **controlador MVC 5 com exibições, usando Entity Framework** e clique em **Adicionar.**</span><span class="sxs-lookup"><span data-stu-id="55c24-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![Selecionando o controlador MVC 5 com exibições e Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="55c24-246">*Selecionando o controlador MVC 5 com exibições e Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="55c24-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="55c24-247">Defina *MvcPersonController* como o **nome do controlador**, selecione a opção **usar ações do controlador assíncrono** e selecione **pessoa (MyHybridSite. Models)** como a **classe do modelo**.</span><span class="sxs-lookup"><span data-stu-id="55c24-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![Adicionando um controlador MVC com scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="55c24-249">*Adicionando um controlador MVC com scaffolding*</span><span class="sxs-lookup"><span data-stu-id="55c24-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="55c24-250">Em **classe de contexto de dados**, clique em **novo contexto de dados...** .</span><span class="sxs-lookup"><span data-stu-id="55c24-250">Under **Data context class**, click **New data context...**.</span></span>

    ![Criando um novo contexto de dados](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="55c24-252">*Criando um novo contexto de dados*</span><span class="sxs-lookup"><span data-stu-id="55c24-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="55c24-253">Na caixa de diálogo **novo contexto de dados** , nomeie o novo contexto de dados *PersonContext* e clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="55c24-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![Criando o novo PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="55c24-255">*Criando o novo tipo de PersonContext*</span><span class="sxs-lookup"><span data-stu-id="55c24-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="55c24-256">Clique em **Adicionar** para criar o novo controlador para **pessoa** com scaffolding.</span><span class="sxs-lookup"><span data-stu-id="55c24-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="55c24-257">O Visual Studio irá gerar as ações do controlador, o contexto de dados da pessoa e os modos de exibição do Razor.</span><span class="sxs-lookup"><span data-stu-id="55c24-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![Depois de criar o controlador MVC com scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="55c24-259">*Depois de criar o controlador MVC com scaffolding*</span><span class="sxs-lookup"><span data-stu-id="55c24-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="55c24-260">Abra o arquivo **MvcPersonController.cs** na pasta **controladores** .</span><span class="sxs-lookup"><span data-stu-id="55c24-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="55c24-261">Observe que os métodos de ação CRUD foram gerados automaticamente.</span><span class="sxs-lookup"><span data-stu-id="55c24-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="55c24-262">Marcando a caixa de seleção **usar ações do controlador assíncrono** nas opções scaffolding nas etapas anteriores, o Visual Studio gera métodos de ação assíncronas para todas as ações que envolvem o acesso ao contexto de dados Person.</span><span class="sxs-lookup"><span data-stu-id="55c24-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="55c24-263">É recomendável que você use métodos de ação assíncronas para solicitações de execução longa e não de CPU para evitar impedir que o servidor Web execute o trabalho enquanto a solicitação está sendo processada.</span><span class="sxs-lookup"><span data-stu-id="55c24-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="55c24-264">Tarefa 3 – executando a solução</span><span class="sxs-lookup"><span data-stu-id="55c24-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="55c24-265">Nesta tarefa, você executará a solução novamente para verificar se as exibições da **pessoa** estão funcionando conforme o esperado.</span><span class="sxs-lookup"><span data-stu-id="55c24-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="55c24-266">Você adicionará uma nova pessoa para verificar se ela foi salva com êxito no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="55c24-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="55c24-267">Pressione **F5** para executar a solução.</span><span class="sxs-lookup"><span data-stu-id="55c24-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="55c24-268">Navegue até **/MvcPerson**.</span><span class="sxs-lookup"><span data-stu-id="55c24-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="55c24-269">A exibição com Scaffold que mostra a lista de pessoas deve aparecer.</span><span class="sxs-lookup"><span data-stu-id="55c24-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="55c24-270">Clique em **criar novo** para adicionar uma nova pessoa.</span><span class="sxs-lookup"><span data-stu-id="55c24-270">Click **Create New** to add a new person.</span></span>

    ![Navegando para as exibições do com Scaffold MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="55c24-272">*Navegando para as exibições do com Scaffold MVC*</span><span class="sxs-lookup"><span data-stu-id="55c24-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="55c24-273">Na exibição **criar** , forneça um **nome** e uma **idade** para a pessoa e clique em **criar**.</span><span class="sxs-lookup"><span data-stu-id="55c24-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![Adicionando uma nova pessoa](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="55c24-275">*Adicionando uma nova pessoa*</span><span class="sxs-lookup"><span data-stu-id="55c24-275">*Adding a new person*</span></span>
5. <span data-ttu-id="55c24-276">A nova pessoa é adicionada à lista.</span><span class="sxs-lookup"><span data-stu-id="55c24-276">The new person is added to the list.</span></span> <span data-ttu-id="55c24-277">Na lista elemento, clique em **detalhes** para exibir a exibição de detalhes da pessoa.</span><span class="sxs-lookup"><span data-stu-id="55c24-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="55c24-278">Em seguida, no modo de exibição de **detalhes** , clique em **voltar à lista** para voltar para a exibição de lista.</span><span class="sxs-lookup"><span data-stu-id="55c24-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![Exibição de detalhes da pessoa](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="55c24-280">*Exibição de detalhes da pessoa*</span><span class="sxs-lookup"><span data-stu-id="55c24-280">*Person's details view*</span></span>
6. <span data-ttu-id="55c24-281">Clique no link **excluir** para excluir a pessoa.</span><span class="sxs-lookup"><span data-stu-id="55c24-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="55c24-282">Na exibição **excluir** , clique em **excluir** para confirmar a operação.</span><span class="sxs-lookup"><span data-stu-id="55c24-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![Excluindo uma pessoa](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="55c24-284">*Excluindo uma pessoa*</span><span class="sxs-lookup"><span data-stu-id="55c24-284">*Deleting a person*</span></span>
7. <span data-ttu-id="55c24-285">Volte para o Visual Studio e pressione **Shift + F5** para parar a depuração.</span><span class="sxs-lookup"><span data-stu-id="55c24-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="55c24-286">Exercício 3: Criando um controlador de API Web usando scaffolding</span><span class="sxs-lookup"><span data-stu-id="55c24-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="55c24-287">A estrutura da API Web faz parte da pilha ASP.NET e foi projetada para tornar a implementação de serviços HTTP mais fácil, geralmente enviando e recebendo dados formatados em JSON ou XML por meio de uma API RESTful.</span><span class="sxs-lookup"><span data-stu-id="55c24-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="55c24-288">Neste exercício, você usará ASP.NET scaffolding novamente para gerar um controlador de API da Web.</span><span class="sxs-lookup"><span data-stu-id="55c24-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="55c24-289">Você usará as mesmas classes **Person** e **PersonContext** do exercício anterior para fornecer os mesmos dados de pessoa no formato JSON.</span><span class="sxs-lookup"><span data-stu-id="55c24-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="55c24-290">Você verá como é possível expor os mesmos recursos de diferentes maneiras no mesmo aplicativo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="55c24-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="55c24-291">Tarefa 1 – Criando um controlador de API da Web</span><span class="sxs-lookup"><span data-stu-id="55c24-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="55c24-292">Nesta tarefa, você criará um novo **controlador de API Web** que exporá os dados da pessoa em um formato de consumo de máquina como o JSON.</span><span class="sxs-lookup"><span data-stu-id="55c24-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="55c24-293">Se ainda não estiver aberto, abra **Visual Studio Express 2013 para Web** e abra a solução **MyHybridSite. sln** localizada na pasta **Source/EX3-WebAPI/Begin** .</span><span class="sxs-lookup"><span data-stu-id="55c24-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="55c24-294">Como alternativa, você pode continuar com a solução que você obteve no exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="55c24-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="55c24-295">Se você começar com a solução inicial do exercício 3, pressione **Ctrl + Shift + B** para compilar a solução.</span><span class="sxs-lookup"><span data-stu-id="55c24-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="55c24-296">Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta **controladores** do projeto **MyHybridSite** e selecione **Adicionar | Novo item de com Scaffold...**</span><span class="sxs-lookup"><span data-stu-id="55c24-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Criando um novo controlador com Scaffold](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="55c24-298">*Criando um novo controlador com Scaffold*</span><span class="sxs-lookup"><span data-stu-id="55c24-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="55c24-299">Na caixa de diálogo **Adicionar Scaffold** , selecione **API Web** no painel esquerdo, em seguida, **controlador da API Web 2 com ações, usando Entity Framework** no painel central e, em seguida, clique em **Adicionar.**</span><span class="sxs-lookup"><span data-stu-id="55c24-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="55c24-300">![Selecionando o controlador da API Web 2 com ações e Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecionando o controlador da API Web 2 com ações e Entity Framework")</span><span class="sxs-lookup"><span data-stu-id="55c24-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="55c24-301">*Selecionando o controlador da API Web 2 com ações e Entity Framework*</span><span class="sxs-lookup"><span data-stu-id="55c24-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="55c24-302">Defina *ApiPersonController* como o **nome do controlador**, selecione a opção **usar ações do controlador assíncrono** e selecione **pessoa (MyHybridSite. Models)** e **PersonContext (MyHybridSite. Models)** como as classes **modelo** e **contexto de dados** , respectivamente.</span><span class="sxs-lookup"><span data-stu-id="55c24-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="55c24-303">Clique em **Adicionar**.</span><span class="sxs-lookup"><span data-stu-id="55c24-303">Then click **Add**.</span></span>

    <span data-ttu-id="55c24-304">![Adicionando um controlador de API Web com scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adicionando um controlador de API Web com scaffolding")</span><span class="sxs-lookup"><span data-stu-id="55c24-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="55c24-305">*Adicionando um controlador de API Web com scaffolding*</span><span class="sxs-lookup"><span data-stu-id="55c24-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="55c24-306">Em seguida, o Visual Studio irá gerar a classe **ApiPersonController** com as quatro ações CRUD para trabalhar com seus dados.</span><span class="sxs-lookup"><span data-stu-id="55c24-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="55c24-307">![Depois de criar o controlador da API Web com scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "Depois de criar o controlador da API Web com scaffolding")</span><span class="sxs-lookup"><span data-stu-id="55c24-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="55c24-308">*Depois de criar o controlador da API Web com scaffolding*</span><span class="sxs-lookup"><span data-stu-id="55c24-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="55c24-309">Abra o arquivo **ApiPersonController.cs** e inspecione o método de ação *GetPeople* .</span><span class="sxs-lookup"><span data-stu-id="55c24-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="55c24-310">Esse método consulta o campo DB do tipo **PersonContext** para obter os dados de pessoas.</span><span class="sxs-lookup"><span data-stu-id="55c24-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="55c24-311">Agora, observe o comentário acima da definição do método.</span><span class="sxs-lookup"><span data-stu-id="55c24-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="55c24-312">Ele fornece o URI que expõe essa ação que será usada na próxima tarefa.</span><span class="sxs-lookup"><span data-stu-id="55c24-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="55c24-313">Por padrão, a API Web é configurada para capturar as consultas para o caminho */API* para evitar colisões com controladores MVC.</span><span class="sxs-lookup"><span data-stu-id="55c24-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="55c24-314">Se você precisar alterar essa configuração, consulte [Roteamento em ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="55c24-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="55c24-315">Tarefa 2 – executando a solução</span><span class="sxs-lookup"><span data-stu-id="55c24-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="55c24-316">Nesta tarefa, você usará as ferramentas de **desenvolvedor** do Internet Explorer F12 para inspecionar a resposta completa do controlador da API Web.</span><span class="sxs-lookup"><span data-stu-id="55c24-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="55c24-317">Você verá como é possível capturar o tráfego de rede para obter mais informações sobre os dados do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="55c24-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="55c24-318">Verifique se o **Internet Explorer** está selecionado no botão **Iniciar** localizado na barra de ferramentas do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="55c24-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Opção do Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="55c24-320">As **ferramentas de desenvolvedor F12** têm um amplo conjunto de funcionalidades que não são abordadas neste laboratório prático.</span><span class="sxs-lookup"><span data-stu-id="55c24-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="55c24-321">Se você quiser saber mais sobre isso, consulte [usando as ferramentas de desenvolvedor F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="55c24-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>

1. <span data-ttu-id="55c24-322">Pressione **F5** para executar a solução.</span><span class="sxs-lookup"><span data-stu-id="55c24-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="55c24-323">Para seguir essa tarefa corretamente, seu aplicativo precisa ter dados.</span><span class="sxs-lookup"><span data-stu-id="55c24-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="55c24-324">Se o banco de dados estiver vazio, você poderá voltar para a tarefa 3 no exercício 2 e seguir as etapas sobre como criar uma nova pessoa usando as exibições do MVC.</span><span class="sxs-lookup"><span data-stu-id="55c24-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="55c24-325">No navegador, pressione **F12** para abrir o painel de **ferramentas para desenvolvedores** .</span><span class="sxs-lookup"><span data-stu-id="55c24-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="55c24-326">Pressione **CTRL** + **4** ou clique no ícone de **rede** e, em seguida, clique no botão de seta verde para começar a capturar o tráfego de rede.</span><span class="sxs-lookup"><span data-stu-id="55c24-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="55c24-327">![Iniciando a captura de rede da API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Iniciando a captura de rede da API Web")</span><span class="sxs-lookup"><span data-stu-id="55c24-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="55c24-328">*Iniciando a captura de rede da API Web*</span><span class="sxs-lookup"><span data-stu-id="55c24-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="55c24-329">Acrescente **API/ApiPerson** à URL na barra de endereços do navegador.</span><span class="sxs-lookup"><span data-stu-id="55c24-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="55c24-330">Agora, você inspecionará os detalhes da resposta do **ApiPersonController**.</span><span class="sxs-lookup"><span data-stu-id="55c24-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="55c24-331">![Recuperando dados da pessoa por meio da API da Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Recuperando dados da pessoa por meio da API da Web")</span><span class="sxs-lookup"><span data-stu-id="55c24-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="55c24-332">*Recuperando dados da pessoa por meio da API da Web*</span><span class="sxs-lookup"><span data-stu-id="55c24-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="55c24-333">Quando o download for concluído, você será solicitado a fazer uma ação com o arquivo baixado.</span><span class="sxs-lookup"><span data-stu-id="55c24-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="55c24-334">Deixe a caixa de diálogo aberta para poder assistir ao conteúdo da resposta por meio da janela de ferramentas de desenvolvedores.</span><span class="sxs-lookup"><span data-stu-id="55c24-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="55c24-335">Agora, você inspecionará o corpo da resposta.</span><span class="sxs-lookup"><span data-stu-id="55c24-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="55c24-336">Para fazer isso, clique na guia **detalhes** e, em seguida, clique em **corpo da resposta**.</span><span class="sxs-lookup"><span data-stu-id="55c24-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="55c24-337">Você pode verificar se os dados baixados são uma lista de objetos com a **ID**de propriedades, o **nome** e a **idade** que correspondem à classe **Person** .</span><span class="sxs-lookup"><span data-stu-id="55c24-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="55c24-338">![Exibindo o corpo da resposta da API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Exibindo o corpo da resposta da API Web")</span><span class="sxs-lookup"><span data-stu-id="55c24-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="55c24-339">*Exibindo o corpo da resposta da API Web*</span><span class="sxs-lookup"><span data-stu-id="55c24-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="55c24-340">Tarefa 3 – adicionando páginas de ajuda da API Web</span><span class="sxs-lookup"><span data-stu-id="55c24-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="55c24-341">Quando você cria uma API da Web, é útil criar uma página de ajuda para que outros desenvolvedores saibam como chamar sua API.</span><span class="sxs-lookup"><span data-stu-id="55c24-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="55c24-342">Você pode criar e atualizar as páginas de documentação manualmente, mas é melhor gerá-las automaticamente para evitar ter que fazer o trabalho de manutenção.</span><span class="sxs-lookup"><span data-stu-id="55c24-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="55c24-343">Nesta tarefa, você usará um pacote NuGet para gerar automaticamente páginas de ajuda da API Web para a solução.</span><span class="sxs-lookup"><span data-stu-id="55c24-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="55c24-344">No menu **ferramentas** no Visual Studio, selecione **Gerenciador de pacotes NuGet**e clique em **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="55c24-344">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="55c24-345">Na janela do **console do Gerenciador de pacotes** , execute o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="55c24-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="55c24-346">O pacote **Microsoft. AspNet. WebApi. HelpPage** instala os assemblies necessários e adiciona exibições MVC para as páginas de ajuda na pasta **areas/HelpPage** .</span><span class="sxs-lookup"><span data-stu-id="55c24-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="55c24-347">![Área HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "Área HelpPage")</span><span class="sxs-lookup"><span data-stu-id="55c24-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="55c24-348">*Área HelpPage*</span><span class="sxs-lookup"><span data-stu-id="55c24-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="55c24-349">Por padrão, as páginas de ajuda têm cadeias de caracteres de espaço reservado para documentação.</span><span class="sxs-lookup"><span data-stu-id="55c24-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="55c24-350">Você pode usar comentários de documentação XML para criar a documentação.</span><span class="sxs-lookup"><span data-stu-id="55c24-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="55c24-351">Para habilitar esse recurso, abra o arquivo **HelpPageConfig.cs** localizado na pasta **areas/HelpPage/app\_Start** e remova a marca de comentário da seguinte linha:</span><span class="sxs-lookup"><span data-stu-id="55c24-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="55c24-352">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **MyHybridSite**, selecione **Propriedades** e clique na guia **Compilar** .</span><span class="sxs-lookup"><span data-stu-id="55c24-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="55c24-353">![Guia compilar](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Seção de Build")</span><span class="sxs-lookup"><span data-stu-id="55c24-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="55c24-354">*Guia compilar*</span><span class="sxs-lookup"><span data-stu-id="55c24-354">*Build tab*</span></span>
5. <span data-ttu-id="55c24-355">Em **saída**, selecione **arquivo de documentação XML**.</span><span class="sxs-lookup"><span data-stu-id="55c24-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="55c24-356">Na caixa de edição, digite **App\_data/XmlDocument. xml**.</span><span class="sxs-lookup"><span data-stu-id="55c24-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="55c24-357">![Seção de saída na guia Build](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Seção de saída na guia Build")</span><span class="sxs-lookup"><span data-stu-id="55c24-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="55c24-358">*Seção de saída na guia Build*</span><span class="sxs-lookup"><span data-stu-id="55c24-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="55c24-359">Pressione **CTRL** + **S** para salvar as alterações.</span><span class="sxs-lookup"><span data-stu-id="55c24-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="55c24-360">Abra o arquivo **ApiPersonController.cs** na pasta **controladores** .</span><span class="sxs-lookup"><span data-stu-id="55c24-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="55c24-361">Insira uma nova linha entre a assinatura do método *GetPeople* e o comentário *//Get API/ApiPerson* e, em seguida, digite três barras invertidas.</span><span class="sxs-lookup"><span data-stu-id="55c24-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="55c24-362">O Visual Studio insere automaticamente os elementos XML que definem a documentação do método.</span><span class="sxs-lookup"><span data-stu-id="55c24-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="55c24-363">Adicione um texto de resumo e o valor de retorno para o método *GetPeople* .</span><span class="sxs-lookup"><span data-stu-id="55c24-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="55c24-364">Ele deverá ter a seguinte aparência.</span><span class="sxs-lookup"><span data-stu-id="55c24-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="55c24-365">Pressione **F5** para executar a solução.</span><span class="sxs-lookup"><span data-stu-id="55c24-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="55c24-366">Acrescente **/Help** à URL na barra de endereços para navegar até a página de ajuda.</span><span class="sxs-lookup"><span data-stu-id="55c24-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="55c24-367">![Página de ajuda do ASP.NET Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "Página de ajuda do ASP.NET Web API")</span><span class="sxs-lookup"><span data-stu-id="55c24-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="55c24-368">*Página de ajuda do ASP.NET Web API*</span><span class="sxs-lookup"><span data-stu-id="55c24-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="55c24-369">O conteúdo principal da página é uma tabela de APIs, agrupada por controlador.</span><span class="sxs-lookup"><span data-stu-id="55c24-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="55c24-370">As entradas de tabela são geradas dinamicamente, usando a interface **IApiExplorer** .</span><span class="sxs-lookup"><span data-stu-id="55c24-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="55c24-371">Se você adicionar ou atualizar um controlador de API, a tabela será atualizada automaticamente na próxima vez que você compilar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="55c24-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="55c24-372">A coluna **API** lista o método http e o URI relativo.</span><span class="sxs-lookup"><span data-stu-id="55c24-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="55c24-373">A coluna **Descrição** contém informações que foram extraídas da documentação do método.</span><span class="sxs-lookup"><span data-stu-id="55c24-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="55c24-374">Observe que a descrição que você adicionou acima da definição do método é exibida na coluna Descrição.</span><span class="sxs-lookup"><span data-stu-id="55c24-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="55c24-375">![Descrição do método de API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "Descrição do método de API")</span><span class="sxs-lookup"><span data-stu-id="55c24-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="55c24-376">*Descrição do método de API*</span><span class="sxs-lookup"><span data-stu-id="55c24-376">*API method description*</span></span>
13. <span data-ttu-id="55c24-377">Clique em um dos métodos de API para navegar até uma página com informações mais detalhadas, incluindo corpos de resposta de exemplo.</span><span class="sxs-lookup"><span data-stu-id="55c24-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="55c24-378">![Página informações de detalhes](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Página informações de detalhes")</span><span class="sxs-lookup"><span data-stu-id="55c24-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="55c24-379">*Página de informações detalhadas*</span><span class="sxs-lookup"><span data-stu-id="55c24-379">*Detailed information page*</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="55c24-380">Resumo</span><span class="sxs-lookup"><span data-stu-id="55c24-380">Summary</span></span>

<span data-ttu-id="55c24-381">Ao concluir este laboratório prático, você aprendeu a:</span><span class="sxs-lookup"><span data-stu-id="55c24-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="55c24-382">Criar um novo aplicativo Web usando a experiência de ASP.NET no Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="55c24-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="55c24-383">Integre várias tecnologias ASP.NET em um único projeto</span><span class="sxs-lookup"><span data-stu-id="55c24-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="55c24-384">Gerar os controladores MVC e exibições de suas classes de modelo usando ASP.NET scaffolding</span><span class="sxs-lookup"><span data-stu-id="55c24-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="55c24-385">Gerar controladores de API Web, que usam recursos como programação assíncrona e acesso a dados por meio de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="55c24-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="55c24-386">Gerar automaticamente páginas de ajuda da API Web para seus controladores</span><span class="sxs-lookup"><span data-stu-id="55c24-386">Automatically generate Web API Help Pages for your controllers</span></span>
