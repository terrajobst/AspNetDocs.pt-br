---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Laboratório prático: Visual Studio 2013 Web Tools | Microsoft Docs'
author: rick-anderson
description: O Visual Studio é um excelente ambiente de desenvolvimento para o. Projetos da Web e do Windows baseados em rede. Ele inclui um editor de texto poderoso que pode ser facilmente usado para...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 2fb987dd9b26ad9f0e8a88fd881bde4505ec4148
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622671"
---
# <a name="hands-on-lab-visual-studio-2013-web-tools"></a><span data-ttu-id="54579-104">Laboratório prático: ferramentas da Web do Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="54579-104">Hands On Lab: Visual Studio 2013 Web Tools</span></span>

<span data-ttu-id="54579-105">por [equipe de acampamentos da Web](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="54579-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="54579-106">Baixe o kit de treinamento do Web acampamentos</span><span class="sxs-lookup"><span data-stu-id="54579-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

> <span data-ttu-id="54579-107">O Visual Studio é um excelente ambiente de desenvolvimento para o. Projetos da Web e do Windows baseados em rede.</span><span class="sxs-lookup"><span data-stu-id="54579-107">Visual Studio is an excellent development environment for .NET-based Windows and web projects.</span></span> <span data-ttu-id="54579-108">Ele inclui um editor de texto poderoso que pode ser facilmente usado para editar arquivos autônomos sem um projeto.</span><span class="sxs-lookup"><span data-stu-id="54579-108">It includes a powerful text editor that can easily be used to edit standalone files without a project.</span></span>
> 
> <span data-ttu-id="54579-109">O Visual Studio mantém uma árvore de análise com recursos completos conforme você edita cada arquivo.</span><span class="sxs-lookup"><span data-stu-id="54579-109">Visual Studio maintains a full-featured parse tree as you edit each file.</span></span> <span data-ttu-id="54579-110">Isso permite que o Visual Studio forneça ações de preenchimento automático e com base em documentos sem paralelo e, ao mesmo tempo, torna a experiência de desenvolvimento muito mais rápida e agradável.</span><span class="sxs-lookup"><span data-stu-id="54579-110">This allows Visual Studio to provide unparalleled auto-completion and document-based actions while making the development experience much faster and more pleasant.</span></span> <span data-ttu-id="54579-111">Esses recursos são especialmente poderosos em documentos HTML e CSS.</span><span class="sxs-lookup"><span data-stu-id="54579-111">These features are especially powerful in HTML and CSS documents.</span></span>
> 
> <span data-ttu-id="54579-112">Toda essa potência também está disponível para extensões, tornando simples estender os editores com novos recursos avançados para atender às suas necessidades.</span><span class="sxs-lookup"><span data-stu-id="54579-112">All of this power is also available for extensions, making it simple to extend the editors with powerful new features to suit your needs.</span></span> <span data-ttu-id="54579-113">O Web Essentials é uma coleção (principalmente) aprimoramentos relacionados à Web para o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54579-113">Web Essentials is a collection of (mostly) web-related enhancements to Visual Studio.</span></span> <span data-ttu-id="54579-114">Ele inclui muitas novas conclusões do IntelliSense (especialmente para CSS), novos recursos de link do navegador, JSHint automático para arquivos JavaScript, novos avisos para HTML e CSS e muitos outros recursos que são essenciais para o desenvolvimento para a Web moderno.</span><span class="sxs-lookup"><span data-stu-id="54579-114">It includes lots of new IntelliSense completions (especially for CSS), new Browser Link features, automatic JSHint for JavaScript files, new warnings for HTML and CSS, and many other features that are essential to modern web development.</span></span>
> 
> <span data-ttu-id="54579-115">Todos os códigos de exemplo e trechos de código estão incluídos no kit de treinamento do acampamentos da Web, disponível em [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="54579-115">All sample code and snippets are included in the Web Camps Training Kit, available at [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).</span></span>

<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="54579-116">Visão geral</span><span class="sxs-lookup"><span data-stu-id="54579-116">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="54579-117">Objetivos</span><span class="sxs-lookup"><span data-stu-id="54579-117">Objectives</span></span>

<span data-ttu-id="54579-118">Neste laboratório prático, você aprenderá a:</span><span class="sxs-lookup"><span data-stu-id="54579-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="54579-119">Usar novos recursos do editor de HTML incluídos no Web Essentials, como trechos de código do HTML5 e codificação de Zen avançados</span><span class="sxs-lookup"><span data-stu-id="54579-119">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="54579-120">Usar os novos recursos do editor de CSS incluídos no Web Essentials, como o seletor de cores e a dica de ferramenta da matriz do navegador</span><span class="sxs-lookup"><span data-stu-id="54579-120">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="54579-121">Usar novos recursos do editor de JavaScript incluídos no Web Essentials, como extrair para arquivo e IntelliSense para todos os elementos HTML</span><span class="sxs-lookup"><span data-stu-id="54579-121">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="54579-122">Trocar dados entre o navegador e o Visual Studio usando o link do navegador</span><span class="sxs-lookup"><span data-stu-id="54579-122">Exchange data between your browser and Visual Studio using Browser Link</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="54579-123">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="54579-123">Prerequisites</span></span>

<span data-ttu-id="54579-124">O seguinte é necessário para concluir este laboratório prático:</span><span class="sxs-lookup"><span data-stu-id="54579-124">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="54579-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) ou superior</span><span class="sxs-lookup"><span data-stu-id="54579-125">[Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="54579-126">Web Essentials 2013</span><span class="sxs-lookup"><span data-stu-id="54579-126">Web Essentials 2013</span></span>](http://vswebessentials.com/)
- [<span data-ttu-id="54579-127">Google Chrome</span><span class="sxs-lookup"><span data-stu-id="54579-127">Google Chrome</span></span>](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="54579-128">Instalação</span><span class="sxs-lookup"><span data-stu-id="54579-128">Setup</span></span>

<span data-ttu-id="54579-129">Para executar os exercícios neste laboratório prático, você precisará configurar seu ambiente primeiro...</span><span class="sxs-lookup"><span data-stu-id="54579-129">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="54579-130">Abra uma janela do Windows Explorer e navegue até a pasta de **origem** do laboratório.</span><span class="sxs-lookup"><span data-stu-id="54579-130">Open a Windows Explorer window and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="54579-131">Clique com o botão direito do mouse em **Setup. cmd** e selecione **Executar como administrador** para iniciar o processo de instalação que irá configurar seu ambiente e instalar os trechos de código do Visual Studio para este laboratório.</span><span class="sxs-lookup"><span data-stu-id="54579-131">Right-click **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="54579-132">Se a caixa de diálogo controle de conta de usuário for exibida, confirme a ação para continuar.</span><span class="sxs-lookup"><span data-stu-id="54579-132">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="54579-133">Verifique se você verificou todas as dependências deste laboratório antes de executar a instalação.</span><span class="sxs-lookup"><span data-stu-id="54579-133">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="54579-134">Usando os trechos de código</span><span class="sxs-lookup"><span data-stu-id="54579-134">Using the Code Snippets</span></span>

<span data-ttu-id="54579-135">Em todo o documento do laboratório, você será instruído a inserir blocos de código.</span><span class="sxs-lookup"><span data-stu-id="54579-135">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="54579-136">Para sua conveniência, a maior parte desse código é fornecida como trechos de Visual Studio Code, que podem ser acessados em Visual Studio 2013 para evitar a necessidade de adicioná-lo manualmente.</span><span class="sxs-lookup"><span data-stu-id="54579-136">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="54579-137">Cada exercício é acompanhado por uma solução inicial localizada na pasta **begin** do exercício que permite que você siga cada exercício independentemente dos outros.</span><span class="sxs-lookup"><span data-stu-id="54579-137">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="54579-138">Lembre-se de que os trechos de código que são adicionados durante um exercício estão ausentes dessas soluções iniciais e podem não funcionar até que você conclua o exercício.</span><span class="sxs-lookup"><span data-stu-id="54579-138">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="54579-139">Dentro do código-fonte de um exercício, você também encontrará uma pasta **final** contendo uma solução do Visual Studio com o código que resulta da conclusão das etapas no exercício correspondente.</span><span class="sxs-lookup"><span data-stu-id="54579-139">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="54579-140">Você pode usar essas soluções como diretrizes se precisar de ajuda adicional ao trabalhar com este laboratório prático.</span><span class="sxs-lookup"><span data-stu-id="54579-140">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>

---

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="54579-141">Exercícios</span><span class="sxs-lookup"><span data-stu-id="54579-141">Exercises</span></span>

<span data-ttu-id="54579-142">Este laboratório prático inclui os seguintes exercícios:</span><span class="sxs-lookup"><span data-stu-id="54579-142">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="54579-143">Trabalhando com o link do navegador e o Web Essentials</span><span class="sxs-lookup"><span data-stu-id="54579-143">Working with Browser Link and Web Essentials</span></span>](#Exercise1)
2. [<span data-ttu-id="54579-144">Tirando proveito dos trechos de código e do IntelliSense</span><span class="sxs-lookup"><span data-stu-id="54579-144">Taking Advantage of Code Snippets and IntelliSense</span></span>](#Exercise2)

> [!NOTE]
> <span data-ttu-id="54579-145">Ao iniciar o Visual Studio pela primeira vez, você deve selecionar uma das coleções de configurações predefinidas.</span><span class="sxs-lookup"><span data-stu-id="54579-145">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="54579-146">Cada coleção predefinida é projetada para corresponder a um estilo de desenvolvimento específico e determina layouts de janela, comportamento do editor, trechos de código IntelliSense e opções da caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="54579-146">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="54579-147">Os procedimentos neste laboratório descrevem as ações necessárias para realizar uma determinada tarefa no Visual Studio ao usar a coleção de **configurações de desenvolvimento geral** .</span><span class="sxs-lookup"><span data-stu-id="54579-147">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="54579-148">Se você escolher uma coleção de configurações diferentes para seu ambiente de desenvolvimento, poderá haver diferenças nas etapas que você deve levar em conta.</span><span class="sxs-lookup"><span data-stu-id="54579-148">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a><span data-ttu-id="54579-149">Exercício 1: trabalhando com o link do navegador e o Web Essentials</span><span class="sxs-lookup"><span data-stu-id="54579-149">Exercise 1: Working with Browser Link and Web Essentials</span></span>

<span data-ttu-id="54579-150">O **Web Essentials** é uma extensão do Visual Studio que adiciona uma variedade de recursos úteis para o desenvolvimento para a Web moderno, principalmente focado em tornar a experiência de desenvolvimento para a Web muito mais rápida e agradável.</span><span class="sxs-lookup"><span data-stu-id="54579-150">**Web Essentials** is a Visual Studio extension that adds a variety of useful features for modern web development, mostly focused on making the web development experience much faster and more pleasant.</span></span> <span data-ttu-id="54579-151">Você pode instalar o Web Essentials da Galeria de extensões no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54579-151">You can install Web Essentials from the Extension Gallery in Visual Studio.</span></span>

<span data-ttu-id="54579-152">O **link do navegador** é um novo recurso incluído no Visual Studio 2013 que fornece um canal entre o IDE do Visual Studio e qualquer navegador aberto para trocar dados entre o aplicativo Web e o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54579-152">**Browser Link** is a new feature included in Visual Studio 2013 that provides a channel between the Visual Studio IDE and any open browser to exchange data between your web application and Visual Studio.</span></span> <span data-ttu-id="54579-153">O Web Essentials estende o link do navegador com ferramentas para manipular o modelo de objeto DOM e os estilos CSS de suas páginas da Web diretamente do navegador.</span><span class="sxs-lookup"><span data-stu-id="54579-153">Web Essentials extends Browser Link with tools to manipulate the DOM object model and the CSS styles of your web pages directly from the browser.</span></span>

<span data-ttu-id="54579-154">Neste exercício, você explorará alguns dos recursos com suporte no **Web Essentials** e no **link do navegador** para aprimorar uma página de teste simples.</span><span class="sxs-lookup"><span data-stu-id="54579-154">In this exercise, you will explore some of the features supported by **Web Essentials** and **Browser Link** to enhance a simple quiz page.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a><span data-ttu-id="54579-155">Tarefa 1-executando o projeto em vários navegadores</span><span class="sxs-lookup"><span data-stu-id="54579-155">Task 1 - Running the Project in Multiple Browsers</span></span>

<span data-ttu-id="54579-156">Nesta tarefa, você configurará seu aplicativo Web para ser executado em vários navegadores de uma vez, o que é útil para testes entre navegadores.</span><span class="sxs-lookup"><span data-stu-id="54579-156">In this task, you will configure your web application to run in multiple browsers at once, which is useful for cross-browser testing.</span></span>

1. <span data-ttu-id="54579-157">Abra **Microsoft Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="54579-157">Open **Microsoft Visual Studio**.</span></span>
2. <span data-ttu-id="54579-158">No menu **arquivo** , selecione **abrir | Projeto/solução...** e navegue até **EX1-WorkingwithBrowserLinkandWebEssentials\Begin** na pasta de **origem** do laboratório (C:\WebCampsTK\HOL\VSWebTooling\Source).</span><span class="sxs-lookup"><span data-stu-id="54579-158">In the **File** menu, select **Open | Project/Solution...** and browse to **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in the **Source** folder of the lab (C:\WebCampsTK\HOL\VSWebTooling\Source).</span></span> <span data-ttu-id="54579-159">Selecione **Iniciar. sln** e clique em **abrir**.</span><span class="sxs-lookup"><span data-stu-id="54579-159">Select **Begin.sln** and click **Open**.</span></span>
3. <span data-ttu-id="54579-160">Na barra de ferramentas do Visual Studio, expanda o menu navegador e selecione **procurar com.** ...</span><span class="sxs-lookup"><span data-stu-id="54579-160">In the Visual Studio toolbar, expand the browser menu and select **Browse With...**.</span></span>

    <span data-ttu-id="54579-161">![Navegar com opção de menu](visual-studio-2013-web-tools/_static/image1.png "Procurar com... no menu do navegador")</span><span class="sxs-lookup"><span data-stu-id="54579-161">![Browse With menu option](visual-studio-2013-web-tools/_static/image1.png "Browse with... in browser menu")</span></span>

    <span data-ttu-id="54579-162">*Navegar com opção de menu*</span><span class="sxs-lookup"><span data-stu-id="54579-162">*Browse With menu option*</span></span>
4. <span data-ttu-id="54579-163">Na caixa de diálogo **procurar por** , selecione o **Google Chrome** e o **Internet Explorer** mantendo a tecla **Ctrl** pressionada e clique em **definir como padrão**.</span><span class="sxs-lookup"><span data-stu-id="54579-163">In the **Browse With** dialog box, select both **Google Chrome** and **Internet Explorer** by holding down the **CTRL** key and click **Set as Default**.</span></span>

    <span data-ttu-id="54579-164">![Caixa de diálogo Procurar com](visual-studio-2013-web-tools/_static/image2.png "Caixa de diálogo Procurar com")</span><span class="sxs-lookup"><span data-stu-id="54579-164">![Browse with dialog box](visual-studio-2013-web-tools/_static/image2.png "Browse with dialog box")</span></span>

    <span data-ttu-id="54579-165">*Selecionando vários navegadores padrão*</span><span class="sxs-lookup"><span data-stu-id="54579-165">*Selecting multiple default browsers*</span></span>
5. <span data-ttu-id="54579-166">O Google Chrome e o Internet Explorer agora devem aparecer como os navegadores padrão.</span><span class="sxs-lookup"><span data-stu-id="54579-166">Both Google Chrome and Internet Explorer should now appear as the default browsers.</span></span> <span data-ttu-id="54579-167">Clique em **Cancelar** para fechar a caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="54579-167">Click **Cancel** to close the dialog box.</span></span>

    <span data-ttu-id="54579-168">![Google Chrome e Internet Explorer como navegadores padrão](visual-studio-2013-web-tools/_static/image3.png "Navegadores padrão do Google Chrome e do Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="54579-168">![Google Chrome and Internet Explorer as default browsers](visual-studio-2013-web-tools/_static/image3.png "Google Chrome and Internet Explorer default browsers")</span></span>

    <span data-ttu-id="54579-169">*Google Chrome e Internet Explorer como navegadores padrão*</span><span class="sxs-lookup"><span data-stu-id="54579-169">*Google Chrome and Internet Explorer as default browsers*</span></span>

    > [!NOTE]
    > <span data-ttu-id="54579-170">Depois de configurar os navegadores padrão, a opção **vários navegadores** é selecionada no menu do navegador.</span><span class="sxs-lookup"><span data-stu-id="54579-170">After configuring the default browsers, the **Multiple Browsers** option is selected in the browser menu.</span></span>
    > 
    > <span data-ttu-id="54579-171">![Vários navegadores](visual-studio-2013-web-tools/_static/image4.png "Vários navegadores")</span><span class="sxs-lookup"><span data-stu-id="54579-171">![Multiple browsers](visual-studio-2013-web-tools/_static/image4.png "Multiple browsers")</span></span>
6. <span data-ttu-id="54579-172">Pressione **CTRL** + **F5** para executar o aplicativo sem depuração.</span><span class="sxs-lookup"><span data-stu-id="54579-172">Press **CTRL** + **F5** to run the application without debugging.</span></span>
7. <span data-ttu-id="54579-173">Quando as janelas do navegador estiverem abertas, coloque uma delas acima da outra para ver as atualizações em ambos os navegadores simultaneamente.</span><span class="sxs-lookup"><span data-stu-id="54579-173">When both browser windows open, place one of them above the other in order to see the updates on both browsers simultaneously.</span></span> <span data-ttu-id="54579-174">Os navegadores devem exibir uma página da Web com um retângulo azul claro.</span><span class="sxs-lookup"><span data-stu-id="54579-174">The browsers should display a web page with a light-blue rectangle.</span></span>

    <span data-ttu-id="54579-175">![Colocando um navegador acima do outro](visual-studio-2013-web-tools/_static/image5.png "Colocando um navegador acima do outro")</span><span class="sxs-lookup"><span data-stu-id="54579-175">![Placing one browser above the other](visual-studio-2013-web-tools/_static/image5.png "Placing one browser above the other")</span></span>

    <span data-ttu-id="54579-176">*Colocando um navegador acima do outro*</span><span class="sxs-lookup"><span data-stu-id="54579-176">*Placing one browser above the other*</span></span>
8. <span data-ttu-id="54579-177">Não feche os navegadores.</span><span class="sxs-lookup"><span data-stu-id="54579-177">Do not close the browsers.</span></span> <span data-ttu-id="54579-178">Você os usará na próxima tarefa.</span><span class="sxs-lookup"><span data-stu-id="54579-178">You will use them in the next task.</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a><span data-ttu-id="54579-179">Tarefa 2-usando a codificação de Zen para criar elementos HTML</span><span class="sxs-lookup"><span data-stu-id="54579-179">Task 2 - Using Zen Coding to Create HTML Elements</span></span>

<span data-ttu-id="54579-180">A **codificação Zen** é um plug-in de editor para código e edição de HTML de alta velocidade, XML, XSL (ou qualquer outro formato de código estruturado).</span><span class="sxs-lookup"><span data-stu-id="54579-180">**Zen Coding** is an editor plugin for high-speed HTML, XML, XSL (or any other structured code format) coding and editing.</span></span> <span data-ttu-id="54579-181">O núcleo desse plug-in é um mecanismo de abreviação poderoso que permite expandir expressões, semelhantes aos seletores CSS, em código HTML.</span><span class="sxs-lookup"><span data-stu-id="54579-181">The core of this plugin is a powerful abbreviation engine which allows you to expand expressions -similar to CSS selectors- into HTML code.</span></span> <span data-ttu-id="54579-182">A codificação do Zen é uma maneira rápida de escrever HTML usando uma sintaxe de seletor de estilo CSS.</span><span class="sxs-lookup"><span data-stu-id="54579-182">Zen Coding is a fast way to write HTML using a CSS style selector syntax.</span></span>

<span data-ttu-id="54579-183">Neste exercício, você usará o recurso de codificação do Zen fornecido pelo Web Essentials para gerar os botões HTML que representam as opções da pergunta.</span><span class="sxs-lookup"><span data-stu-id="54579-183">In this exercise, you will use the Zen Coding feature provided by Web Essentials to generate the HTML buttons that represent the options of the question.</span></span>

1. <span data-ttu-id="54579-184">Volte para o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54579-184">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="54579-185">Abra o arquivo **index. cshtml** localizado nas **exibições** | pasta **base** .</span><span class="sxs-lookup"><span data-stu-id="54579-185">Open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="54579-186">Substitua o **&lt;!--todo: adicionar opções aqui--&gt;** comentário com o código a seguir e pressione **Tab**.</span><span class="sxs-lookup"><span data-stu-id="54579-186">Replace the **&lt;!-- TODO: add options here--&gt;** comment with the following code, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. <span data-ttu-id="54579-187">O código deve ser expandido para HTML.</span><span class="sxs-lookup"><span data-stu-id="54579-187">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="54579-188">![HTML expandido](visual-studio-2013-web-tools/_static/image6.png "HTML expandido")</span><span class="sxs-lookup"><span data-stu-id="54579-188">![Expanded HTML](visual-studio-2013-web-tools/_static/image6.png "Expanded HTML")</span></span>

    <span data-ttu-id="54579-189">*HTML expandido*</span><span class="sxs-lookup"><span data-stu-id="54579-189">*Expanded HTML*</span></span>

    > [!NOTE]
    > <span data-ttu-id="54579-190">Para saber mais sobre a sintaxe de codificação do Zen, consulte o [artigo](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/)a seguir.</span><span class="sxs-lookup"><span data-stu-id="54579-190">To learn more about Zen Coding syntax, see the following [article](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).</span></span>
5. <span data-ttu-id="54579-191">Clique no botão **Atualizar navegadores vinculados** para atualizar ambos os navegadores.</span><span class="sxs-lookup"><span data-stu-id="54579-191">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="54579-192">![Atualizar navegadores vinculados](visual-studio-2013-web-tools/_static/image7.png "Atualizar navegadores vinculados")</span><span class="sxs-lookup"><span data-stu-id="54579-192">![Refresh linked browsers](visual-studio-2013-web-tools/_static/image7.png "Refresh linked browsers")</span></span>

    <span data-ttu-id="54579-193">*Atualizar navegadores vinculados*</span><span class="sxs-lookup"><span data-stu-id="54579-193">*Refresh linked browsers*</span></span>

    <span data-ttu-id="54579-194">![Internet Explorer-página atualizada com quatro botões](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer-página atualizada com quatro botões")</span><span class="sxs-lookup"><span data-stu-id="54579-194">![Internet Explorer - Page updated with four buttons](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Page updated with four buttons")</span></span>

    <span data-ttu-id="54579-195">*Internet Explorer-página atualizada com quatro botões*</span><span class="sxs-lookup"><span data-stu-id="54579-195">*Internet Explorer - Page updated with four buttons*</span></span>

    <span data-ttu-id="54579-196">![Google Chrome – página atualizada com quatro botões](visual-studio-2013-web-tools/_static/image9.png "Google Chrome – página atualizada com quatro botões")</span><span class="sxs-lookup"><span data-stu-id="54579-196">![Google Chrome - Page updated with four buttons](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Page updated with four buttons")</span></span>

    <span data-ttu-id="54579-197">*Google Chrome – página atualizada com quatro botões*</span><span class="sxs-lookup"><span data-stu-id="54579-197">*Google Chrome - Page updated with four buttons*</span></span>
6. <span data-ttu-id="54579-198">Volte para o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54579-198">Switch back to Visual Studio.</span></span>
7. <span data-ttu-id="54579-199">Você adicionou os botões à página, mas ainda precisa adicionar uma pergunta de modelo.</span><span class="sxs-lookup"><span data-stu-id="54579-199">You have added the buttons to the page, but you still need to add a template question.</span></span> <span data-ttu-id="54579-200">Para fazer isso, você usará um novo recurso no Web Essentials chamado **gerador do Lorem Ipsum**.</span><span class="sxs-lookup"><span data-stu-id="54579-200">To do so, you will use a new feature in Web Essentials called **Lorem Ipsum generator**.</span></span> <span data-ttu-id="54579-201">Localize o elemento **div** com o atributo de **classe** **front**.</span><span class="sxs-lookup"><span data-stu-id="54579-201">Locate the **div** element with the **class** attribute **front**.</span></span>
8. <span data-ttu-id="54579-202">Adicione o código a seguir como o primeiro elemento filho da **div**e pressione **Tab**.</span><span class="sxs-lookup"><span data-stu-id="54579-202">Add the following code as the first child element of the **div**, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. <span data-ttu-id="54579-203">O código deve ser expandido para HTML.</span><span class="sxs-lookup"><span data-stu-id="54579-203">The code should be expanded to HTML.</span></span>

    <span data-ttu-id="54579-204">![Lorem com a ipsum gerada automaticamente](visual-studio-2013-web-tools/_static/image10.png "Lorem com a ipsum gerada automaticamente")</span><span class="sxs-lookup"><span data-stu-id="54579-204">![Lorem Ipsum autogenerated](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum autogenerated")</span></span>

    <span data-ttu-id="54579-205">*Lorem com a ipsum gerada automaticamente*</span><span class="sxs-lookup"><span data-stu-id="54579-205">*Lorem Ipsum autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="54579-206">Como parte da codificação do Zen, agora você pode gerar código Lorem Ipsum diretamente no editor de HTML.</span><span class="sxs-lookup"><span data-stu-id="54579-206">As part of Zen Coding, you can now generate Lorem Ipsum code directly in the HTML editor.</span></span> <span data-ttu-id="54579-207">Basta digitar **Lorem** e pressionar **Tab** e um texto de 30 palavras Lorem Ipsum será inserido.</span><span class="sxs-lookup"><span data-stu-id="54579-207">Simply type **lorem** and hit **TAB** and a 30 word Lorem Ipsum text will be inserted.</span></span> <span data-ttu-id="54579-208">Por ex.:</span><span class="sxs-lookup"><span data-stu-id="54579-208">E.g.</span></span> <span data-ttu-id="54579-209">*lorem10* insere dez palavras Lorem Ipsum.</span><span class="sxs-lookup"><span data-stu-id="54579-209">*lorem10* inserts 10 Lorem Ipsum words.</span></span>
10. <span data-ttu-id="54579-210">Você adicionará um logotipo na parte superior da pergunta usando outro recurso novo no Web Essentials chamado de **gerador de pixels de lorem**.</span><span class="sxs-lookup"><span data-stu-id="54579-210">You will add a logo at the top of the question by using another new feature in Web Essentials called **Lorem Pixel generator**.</span></span> <span data-ttu-id="54579-211">Adicione o código a seguir como o primeiro elemento filho do elemento **div** com **contêiner** como valor de **classe** e pressione **Tab**.</span><span class="sxs-lookup"><span data-stu-id="54579-211">Add the following code as the first child element of the **div** element with **container** as **class** value, and press **TAB**.</span></span>

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. <span data-ttu-id="54579-212">O código deve expandir para HTML.</span><span class="sxs-lookup"><span data-stu-id="54579-212">The code should expand to HTML.</span></span>

    <span data-ttu-id="54579-213">![Pixel gerado automaticamente em Lorem](visual-studio-2013-web-tools/_static/image11.png "Pixel gerado automaticamente em Lorem")</span><span class="sxs-lookup"><span data-stu-id="54579-213">![Lorem Pixel autogenerated](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel autogenerated")</span></span>

    <span data-ttu-id="54579-214">*Pixel gerado automaticamente em Lorem*</span><span class="sxs-lookup"><span data-stu-id="54579-214">*Lorem Pixel autogenerated*</span></span>

    > [!NOTE]
    > <span data-ttu-id="54579-215">Como parte da codificação do Zen, você também pode gerar código de pixel Lorem diretamente no editor de HTML.</span><span class="sxs-lookup"><span data-stu-id="54579-215">As part of Zen Coding, you can also generate Lorem Pixel code directly in the HTML editor.</span></span> <span data-ttu-id="54579-216">Basta digitar o **PIX-200 x 200-animais** e a **guia** de pressionamento e uma marca **img** com uma imagem 200 x 200 de um animal de animais será inserida.</span><span class="sxs-lookup"><span data-stu-id="54579-216">Simply type **pix-200x200-animals** and hit **TAB** and an **img** tag with a 200x200 image of an animal will be inserted.</span></span> <span data-ttu-id="54579-217">Para obter mais informações, consulte [Lorem pixel](http://www.lorempixel.com).</span><span class="sxs-lookup"><span data-stu-id="54579-217">For more information, refer to [Lorem Pixel](http://www.lorempixel.com).</span></span>
12. <span data-ttu-id="54579-218">Clique no botão **Atualizar navegadores vinculados** para atualizar ambos os navegadores.</span><span class="sxs-lookup"><span data-stu-id="54579-218">Click the **Refresh linked browsers** button to update both browsers.</span></span>

    <span data-ttu-id="54579-219">![Internet Explorer-imagem e texto gerados automaticamente](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer-imagem e texto gerados automaticamente")</span><span class="sxs-lookup"><span data-stu-id="54579-219">![Internet Explorer - Autogenerated image and text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - Autogenerated image and text")</span></span>

    <span data-ttu-id="54579-220">*Internet Explorer-imagem e texto gerados automaticamente*</span><span class="sxs-lookup"><span data-stu-id="54579-220">*Internet Explorer - Autogenerated image and text*</span></span>

    <span data-ttu-id="54579-221">![Google Chrome-imagem e texto gerados automaticamente](visual-studio-2013-web-tools/_static/image13.png "Google Chrome-imagem e texto gerados automaticamente")</span><span class="sxs-lookup"><span data-stu-id="54579-221">![Google Chrome - Autogenerated image and text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - Autogenerated image and text")</span></span>

    <span data-ttu-id="54579-222">*Google Chrome-imagem e texto gerados automaticamente*</span><span class="sxs-lookup"><span data-stu-id="54579-222">*Google Chrome - Autogenerated image and text*</span></span>

    > [!NOTE]
    > <span data-ttu-id="54579-223">Como a imagem é selecionada aleatoriamente ao adicionar o trecho de código, a imagem mostrada nos navegadores pode diferir.</span><span class="sxs-lookup"><span data-stu-id="54579-223">Because the image is selected randomly when adding the code snippet, the image shown in the browsers may differ.</span></span>
13. <span data-ttu-id="54579-224">Não feche os navegadores.</span><span class="sxs-lookup"><span data-stu-id="54579-224">Do not close the browsers.</span></span> <span data-ttu-id="54579-225">Você os usará na próxima tarefa.</span><span class="sxs-lookup"><span data-stu-id="54579-225">You will use them in the next task.</span></span>

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a><span data-ttu-id="54579-226">Tarefa 3-atualizando uma propriedade de estilo</span><span class="sxs-lookup"><span data-stu-id="54579-226">Task 3 - Updating a Style Property</span></span>

<span data-ttu-id="54579-227">Nesta tarefa, você usará o recurso **modo inspecionar** do link do navegador para detectar o local exato em que o elemento DOM específico é gerado e, em seguida, atualizar a Propriedade Color desse elemento usando um seletor de cores fornecido pelo Web Essentials.</span><span class="sxs-lookup"><span data-stu-id="54579-227">In this task, you will use the Browser Link's **Inspect Mode** feature to detect the exact location where the specific DOM element is generated and then update the color property of that element using a color picker provided by Web Essentials.</span></span>

1. <span data-ttu-id="54579-228">No navegador Internet Explorer, pressione **CTRL** + **ALT** \*\* + para\*\* habilitar o modo de inspeção.</span><span class="sxs-lookup"><span data-stu-id="54579-228">In the Internet Explorer browser, press **CTRL** + **ALT** + **I** to enable Inspect Mode.</span></span>
2. <span data-ttu-id="54579-229">Mova o ponteiro sobre a borda azul clara e clique em.</span><span class="sxs-lookup"><span data-stu-id="54579-229">Move the pointer over the light blue border and click.</span></span>

    <span data-ttu-id="54579-230">![Movendo o ponteiro sobre a borda azul clara](visual-studio-2013-web-tools/_static/image14.png "Movendo o ponteiro sobre a borda azul clara")</span><span class="sxs-lookup"><span data-stu-id="54579-230">![Moving the pointer over the light blue border](visual-studio-2013-web-tools/_static/image14.png "Moving the pointer over the light blue border")</span></span>

    <span data-ttu-id="54579-231">*Movendo o ponteiro sobre a borda azul clara*</span><span class="sxs-lookup"><span data-stu-id="54579-231">*Moving the pointer over the light blue border*</span></span>
3. <span data-ttu-id="54579-232">Volte para o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54579-232">Switch back to Visual Studio.</span></span> <span data-ttu-id="54579-233">Observe como o elemento HTML que você selecionou no navegador também está selecionado no editor de HTML do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54579-233">Notice how the HTML element that you selected in the browser is also selected in the Visual Studio HTML editor.</span></span>

    <span data-ttu-id="54579-234">![Elemento HTML selecionado no editor de HTML do Visual Studio](visual-studio-2013-web-tools/_static/image15.png "Elemento HTML selecionado no editor de HTML do Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="54579-234">![HTML element selected in the Visual Studio HTML editor](visual-studio-2013-web-tools/_static/image15.png "HTML element selected in the Visual Studio HTML editor")</span></span>

    <span data-ttu-id="54579-235">*Elemento HTML selecionado no editor de HTML do Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="54579-235">*HTML element selected in the Visual Studio HTML editor*</span></span>
4. <span data-ttu-id="54579-236">Agora, você atualizará a classe CSS **frontal** para alterar o estilo do elemento selecionado.</span><span class="sxs-lookup"><span data-stu-id="54579-236">You will now update the **front** CSS class in order to change the styling of the selected element.</span></span> <span data-ttu-id="54579-237">Para fazer isso, pressione **CTRL** + **para** abrir a caixa **navegar até a** pesquisa.</span><span class="sxs-lookup"><span data-stu-id="54579-237">To do so, press **CTRL** + **,** to open the **Navigate To** search box.</span></span> <span data-ttu-id="54579-238">Digite **site. css** e pressione **Enter** para abrir o arquivo.</span><span class="sxs-lookup"><span data-stu-id="54579-238">Type **site.css** and press **ENTER** to open the file.</span></span>

    <span data-ttu-id="54579-239">![Abrindo o arquivo site. css](visual-studio-2013-web-tools/_static/image16.png "Abrindo o arquivo site. css")</span><span class="sxs-lookup"><span data-stu-id="54579-239">![Opening file Site.css](visual-studio-2013-web-tools/_static/image16.png "Opening file Site.css")</span></span>

    <span data-ttu-id="54579-240">*Abrindo o arquivo site. css*</span><span class="sxs-lookup"><span data-stu-id="54579-240">*Opening file Site.css*</span></span>
5. <span data-ttu-id="54579-241">Pressione **CTRL** + **F** e digite **. flip-container. Front** para localizar o seletor de CSS.</span><span class="sxs-lookup"><span data-stu-id="54579-241">Press **CTRL** + **F** and type **.flip-container .front** to find the CSS selector.</span></span>
6. <span data-ttu-id="54579-242">Clique no quadrado azul claro na propriedade border da classe para abrir o seletor de cores.</span><span class="sxs-lookup"><span data-stu-id="54579-242">Click the light blue square in the border property of the class to open the Color Picker.</span></span>

    <span data-ttu-id="54579-243">![Abrindo o seletor de cores](visual-studio-2013-web-tools/_static/image17.png "Abrindo o seletor de cores")</span><span class="sxs-lookup"><span data-stu-id="54579-243">![Opening the Color Picker](visual-studio-2013-web-tools/_static/image17.png "Opening the Color Picker")</span></span>

    <span data-ttu-id="54579-244">*Abrindo o seletor de cores*</span><span class="sxs-lookup"><span data-stu-id="54579-244">*Opening the Color Picker*</span></span>
7. <span data-ttu-id="54579-245">Expanda o seletor de cores clicando no botão de divisa e selecione uma nova cor.</span><span class="sxs-lookup"><span data-stu-id="54579-245">Expand the Color Picker by clicking the chevron button and select a new color.</span></span>

    <span data-ttu-id="54579-246">![Expandindo o seletor de cores](visual-studio-2013-web-tools/_static/image18.png "Expandindo o seletor de cores")</span><span class="sxs-lookup"><span data-stu-id="54579-246">![Expanding the Color Picker](visual-studio-2013-web-tools/_static/image18.png "Expanding the Color Picker")</span></span>

    <span data-ttu-id="54579-247">*Expandindo o seletor de cores*</span><span class="sxs-lookup"><span data-stu-id="54579-247">*Expanding the Color Picker*</span></span>
8. <span data-ttu-id="54579-248">Pressione **CTRL** + **ALT** + **Enter** para atualizar os navegadores vinculados.</span><span class="sxs-lookup"><span data-stu-id="54579-248">Press **CTRL** + **ALT** + **ENTER** to refresh linked browsers.</span></span>
9. <span data-ttu-id="54579-249">Alterne para o Internet Explorer e observe como a cor da borda foi alterada.</span><span class="sxs-lookup"><span data-stu-id="54579-249">Switch to Internet Explorer and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="54579-250">![Internet Explorer-cor da borda atualizada](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer-cor da borda atualizada")</span><span class="sxs-lookup"><span data-stu-id="54579-250">![Internet Explorer - Border color updated](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Border color updated")</span></span>

    <span data-ttu-id="54579-251">*Internet Explorer-cor da borda atualizada*</span><span class="sxs-lookup"><span data-stu-id="54579-251">*Internet Explorer - Border color updated*</span></span>
10. <span data-ttu-id="54579-252">Alterne para o Google Chrome e observe como a cor da borda foi alterada.</span><span class="sxs-lookup"><span data-stu-id="54579-252">Switch to Google Chrome and notice how the color of the border has changed.</span></span>

    <span data-ttu-id="54579-253">![Google Chrome-cor da borda atualizada](visual-studio-2013-web-tools/_static/image20.png "Google Chrome-cor da borda atualizada")</span><span class="sxs-lookup"><span data-stu-id="54579-253">![Google Chrome - Border color updated](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Border color updated")</span></span>

    <span data-ttu-id="54579-254">*Google Chrome-cor da borda atualizada*</span><span class="sxs-lookup"><span data-stu-id="54579-254">*Google Chrome - Border color updated*</span></span>
11. <span data-ttu-id="54579-255">Volte para o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54579-255">Switch back to Visual Studio.</span></span>
12. <span data-ttu-id="54579-256">Vá para o final do arquivo **site. css** e pressione **Ctrl** + **F** para localizar o seletor **. btn** .</span><span class="sxs-lookup"><span data-stu-id="54579-256">Go to the end of the **Site.css** file and press **CTRL** + **F** to locate the **.btn** selector.</span></span>
13. <span data-ttu-id="54579-257">Observe que a propriedade **-webkit-border-radius** está sublinhada em verde.</span><span class="sxs-lookup"><span data-stu-id="54579-257">Notice that the **-webkit-border-radius** property is underlined in green.</span></span>

    <span data-ttu-id="54579-258">![-webkit-border-radius Propriedade do seletor de BTN](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius Propriedade do seletor de BTN")</span><span class="sxs-lookup"><span data-stu-id="54579-258">![-webkit-border-radius property of the btn selector](visual-studio-2013-web-tools/_static/image21.png "-webkit-border-radius property of the btn selector")</span></span>

    <span data-ttu-id="54579-259">*-webkit-border-radius Propriedade do seletor de BTN*</span><span class="sxs-lookup"><span data-stu-id="54579-259">*-webkit-border-radius property of the btn selector*</span></span>
14. <span data-ttu-id="54579-260">Coloque o acento circunflexo na propriedade **-webkit-border-radius** .</span><span class="sxs-lookup"><span data-stu-id="54579-260">Place the caret in the **-webkit-border-radius** property.</span></span> <span data-ttu-id="54579-261">Uma linha azul deve aparecer na primeira letra da primeira palavra da propriedade.</span><span class="sxs-lookup"><span data-stu-id="54579-261">A blue line should appear under the first letter of the first word of the property.</span></span> <span data-ttu-id="54579-262">Esta é a **marca inteligente**.</span><span class="sxs-lookup"><span data-stu-id="54579-262">This is the **smart tag**.</span></span>
15. <span data-ttu-id="54579-263">Pressione **CTRL** +  **.**</span><span class="sxs-lookup"><span data-stu-id="54579-263">Press **CTRL** + **.**</span></span> <span data-ttu-id="54579-264">para abrir o menu sugestões e clique em **adicionar propriedade padrão ausente (borda-raio)** .</span><span class="sxs-lookup"><span data-stu-id="54579-264">to open the suggestions menu and click **Add missing standard property (border-radius)**.</span></span>

    <span data-ttu-id="54579-265">![Adicionar sugestão de propriedade padrão ausente](visual-studio-2013-web-tools/_static/image22.png "Adicionar sugestão de propriedade padrão ausente")</span><span class="sxs-lookup"><span data-stu-id="54579-265">![Add missing standard property suggestion](visual-studio-2013-web-tools/_static/image22.png "Add missing standard property suggestion")</span></span>

    <span data-ttu-id="54579-266">*Adicionar sugestão de propriedade padrão ausente*</span><span class="sxs-lookup"><span data-stu-id="54579-266">*Add missing standard property suggestion*</span></span>
16. <span data-ttu-id="54579-267">A propriedade **border-radius** é adicionada automaticamente à regra de CSS.</span><span class="sxs-lookup"><span data-stu-id="54579-267">The **border-radius** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="54579-268">![Propriedade padrão ausente adicionada](visual-studio-2013-web-tools/_static/image23.png "Propriedade padrão ausente adicionada")</span><span class="sxs-lookup"><span data-stu-id="54579-268">![Missing standard property added](visual-studio-2013-web-tools/_static/image23.png "Missing standard property added")</span></span>

    <span data-ttu-id="54579-269">*Propriedade padrão ausente adicionada*</span><span class="sxs-lookup"><span data-stu-id="54579-269">*Missing standard property added*</span></span>
17. <span data-ttu-id="54579-270">Mova o ponteiro sobre a propriedade **border-radius** para exibir a **dica de ferramenta da matriz do navegador**.</span><span class="sxs-lookup"><span data-stu-id="54579-270">Move the pointer over the **border-radius** property to display the **Browser matrix tooltip**.</span></span> <span data-ttu-id="54579-271">A **dica de ferramenta de matriz do navegador** mostra a disponibilidade da propriedade em cada navegador.</span><span class="sxs-lookup"><span data-stu-id="54579-271">The **Browser matrix tooltip** shows the availability of the property in each browser.</span></span>

    <span data-ttu-id="54579-272">![Dica de ferramenta de matriz do navegador](visual-studio-2013-web-tools/_static/image24.png "Dica de ferramenta de matriz do navegador")</span><span class="sxs-lookup"><span data-stu-id="54579-272">![Browser matrix tooltip](visual-studio-2013-web-tools/_static/image24.png "Browser matrix tooltip")</span></span>

    <span data-ttu-id="54579-273">*Dica de ferramenta de matriz do navegador*</span><span class="sxs-lookup"><span data-stu-id="54579-273">*Browser matrix tooltip*</span></span>
18. <span data-ttu-id="54579-274">Observe que o valor da propriedade **border-radius** ainda está sublinhado.</span><span class="sxs-lookup"><span data-stu-id="54579-274">Notice that the value of the **border-radius** property is still underlined.</span></span> <span data-ttu-id="54579-275">Mova o ponteiro sobre o valor para ver a mensagem de aviso.</span><span class="sxs-lookup"><span data-stu-id="54579-275">Move the pointer over the value to see the warning message.</span></span>

    <span data-ttu-id="54579-276">![Aviso de valor da propriedade border-radius](visual-studio-2013-web-tools/_static/image25.png "Aviso de valor da propriedade border-radius")</span><span class="sxs-lookup"><span data-stu-id="54579-276">![Border-radius property value warning](visual-studio-2013-web-tools/_static/image25.png "Border-radius property value warning")</span></span>

    <span data-ttu-id="54579-277">*Aviso de valor da propriedade border-radius*</span><span class="sxs-lookup"><span data-stu-id="54579-277">*Border-radius property value warning*</span></span>
19. <span data-ttu-id="54579-278">Remova a unidade do valor da propriedade **border-radius** conforme sugerido pela dica de ferramenta.</span><span class="sxs-lookup"><span data-stu-id="54579-278">Remove the unit of the **border-radius** property value as suggested by the tooltip.</span></span>
20. <span data-ttu-id="54579-279">Como **border-radius** é a propriedade padrão para definir como os cantos de borda arredondados são, você pode remover a propriedade **-webkit-border-radius** e o valor da regra de CSS.</span><span class="sxs-lookup"><span data-stu-id="54579-279">As **border-radius** is the standard property for defining how rounded border corners are, you can remove the **-webkit-border-radius** property and value from the CSS rule.</span></span>
21. <span data-ttu-id="54579-280">Coloque o cursor na propriedade de **quebra automática de texto** e observe que a marca inteligente também aparece abaixo.</span><span class="sxs-lookup"><span data-stu-id="54579-280">Place the caret in the **word-wrap** property and notice that the smart tag also appears below.</span></span>
22. <span data-ttu-id="54579-281">Abra o menu e clique em **Adicionar especificações de fornecedor ausentes**.</span><span class="sxs-lookup"><span data-stu-id="54579-281">Open the menu and click **Add missing vendor specifics**.</span></span>

    <span data-ttu-id="54579-282">![Adicionar sugestão de especificações de fornecedor ausente](visual-studio-2013-web-tools/_static/image26.png "Adicionar sugestão de especificações de fornecedor ausente")</span><span class="sxs-lookup"><span data-stu-id="54579-282">![Add missing vendor specifics suggestion](visual-studio-2013-web-tools/_static/image26.png "Add missing vendor specifics suggestion")</span></span>

    <span data-ttu-id="54579-283">*Adicionar sugestão de especificações de fornecedor ausente*</span><span class="sxs-lookup"><span data-stu-id="54579-283">*Add missing vendor specifics suggestion*</span></span>
23. <span data-ttu-id="54579-284">A propriedade **-MS-Word-Wrap** é adicionada automaticamente à regra de CSS.</span><span class="sxs-lookup"><span data-stu-id="54579-284">The **-ms-word-wrap** property is automatically added to the CSS rule.</span></span>

    <span data-ttu-id="54579-285">![Propriedade específica do fornecedor adicionada](visual-studio-2013-web-tools/_static/image27.png "Propriedade específica do fornecedor adicionada")</span><span class="sxs-lookup"><span data-stu-id="54579-285">![Vendor specific property added](visual-studio-2013-web-tools/_static/image27.png "Vendor specific property added")</span></span>

    <span data-ttu-id="54579-286">*Propriedade específica do fornecedor adicionada*</span><span class="sxs-lookup"><span data-stu-id="54579-286">*Vendor specific property added*</span></span>

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a><span data-ttu-id="54579-287">Tarefa 4-Atualizando o código HTML do navegador</span><span class="sxs-lookup"><span data-stu-id="54579-287">Task 4 - Updating the HTML Code from the Browser</span></span>

<span data-ttu-id="54579-288">Nesta tarefa, você usará o recurso de modo de **design** do link do navegador para editar o objeto dom do navegador e transferir as alterações para o arquivo de origem HTML no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54579-288">In this task, you will use the Browser Link's **Design Mode** feature to edit the DOM object from the browser and transfer the changes to the HTML source file in Visual Studio.</span></span>

1. <span data-ttu-id="54579-289">No Google Chrome, pressione **CTRL** + **ALT** + **D** para habilitar o modo de design.</span><span class="sxs-lookup"><span data-stu-id="54579-289">In Google Chrome, press **CTRL** + **ALT** + **D** to enable Design Mode.</span></span>
2. <span data-ttu-id="54579-290">Mova o ponteiro sobre o rótulo **Lorem Ipsum dolor Sit Amet** e clique em.</span><span class="sxs-lookup"><span data-stu-id="54579-290">Move the pointer over the **Lorem Ipsum dolor sit amet** label and click.</span></span>

    <span data-ttu-id="54579-291">![Editando a pergunta](visual-studio-2013-web-tools/_static/image28.png "Editando a pergunta")</span><span class="sxs-lookup"><span data-stu-id="54579-291">![Editing the question](visual-studio-2013-web-tools/_static/image28.png "Editing the question")</span></span>

    <span data-ttu-id="54579-292">*Editando a pergunta*</span><span class="sxs-lookup"><span data-stu-id="54579-292">*Editing the question*</span></span>
3. <span data-ttu-id="54579-293">Um cursor deve aparecer.</span><span class="sxs-lookup"><span data-stu-id="54579-293">A cursor should appear.</span></span> <span data-ttu-id="54579-294">Substitua o texto original pelo *que ele parece quando escrevo uma pergunta mais longa?* e pressione **ESC** para sair do modo de design.</span><span class="sxs-lookup"><span data-stu-id="54579-294">Replace the original text with *What does it look like when I write a longer question?*, and then press **ESC** to exit Design Mode.</span></span>

    <span data-ttu-id="54579-295">![Pergunta editada](visual-studio-2013-web-tools/_static/image29.png "Pergunta editada")</span><span class="sxs-lookup"><span data-stu-id="54579-295">![Question edited](visual-studio-2013-web-tools/_static/image29.png "Question edited")</span></span>

    <span data-ttu-id="54579-296">*Pergunta editada*</span><span class="sxs-lookup"><span data-stu-id="54579-296">*Question edited*</span></span>
4. <span data-ttu-id="54579-297">Volte para o Visual Studio e abra **index. cshtml**, se ainda não estiver aberto.</span><span class="sxs-lookup"><span data-stu-id="54579-297">Switch back to Visual Studio and open **Index.cshtml**, if not already opened.</span></span> <span data-ttu-id="54579-298">Observe que o texto interno do elemento **&lt;p&gt;** foi atualizado.</span><span class="sxs-lookup"><span data-stu-id="54579-298">Notice that the inner text of the **&lt;p&gt;** element has been updated.</span></span>

    <span data-ttu-id="54579-299">![Pergunta atualizada na página HTML](visual-studio-2013-web-tools/_static/image30.png "Pergunta atualizada na página HTML")</span><span class="sxs-lookup"><span data-stu-id="54579-299">![Updated question in the HTML page](visual-studio-2013-web-tools/_static/image30.png "Updated question in the HTML page")</span></span>

    <span data-ttu-id="54579-300">*Pergunta atualizada na página HTML*</span><span class="sxs-lookup"><span data-stu-id="54579-300">*Updated question in the HTML page*</span></span>

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a><span data-ttu-id="54579-301">Tarefa 5-revisando avisos relacionados à SEO</span><span class="sxs-lookup"><span data-stu-id="54579-301">Task 5 - Reviewing SEO Related Warnings</span></span>

<span data-ttu-id="54579-302">A SEO ( **otimização do mecanismo de pesquisa** ) é o processo de tornar uma classificação de site mais alta na lista de resultados de um mecanismo de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="54579-302">**Search Engine Optimization** (SEO) is the process of making a website rank higher on a search engine's list of results.</span></span> <span data-ttu-id="54579-303">Quanto mais alta for a classificação do site e mais consistentemente ela estiver listada, mais visitantes o site receberá desse mecanismo de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="54579-303">The higher the site ranks and the more consistently it is listed, the more visitors the site will get from that search engine.</span></span> <span data-ttu-id="54579-304">O Web Essentials incorpora uma ferramenta analítica que examina o HTML, relata os problemas encontrados e fornece assistência para corrigi-los.</span><span class="sxs-lookup"><span data-stu-id="54579-304">Web Essentials incorporates an analytical tool that examines HTML, reports the issues found and provides assistance to fix them.</span></span>

1. <span data-ttu-id="54579-305">Vá para o menu **Exibir** e clique em **lista de erros** para abrir a janela **lista de erros** .</span><span class="sxs-lookup"><span data-stu-id="54579-305">Go to the **View** menu and click **Error List** to open the **Error List** window.</span></span>

    <span data-ttu-id="54579-306">![Lista de Erros no menu Exibir](visual-studio-2013-web-tools/_static/image31.png "Lista de Erros no menu Exibir")</span><span class="sxs-lookup"><span data-stu-id="54579-306">![Error List in View menu](visual-studio-2013-web-tools/_static/image31.png "Error List in View menu")</span></span>

    <span data-ttu-id="54579-307">*Lista de Erros no menu Exibir*</span><span class="sxs-lookup"><span data-stu-id="54579-307">*Error List in View menu*</span></span>
2. <span data-ttu-id="54579-308">Observe que há um aviso de SEO notificando que uma marca de **&gt;de&lt;** para a descrição da página está ausente.</span><span class="sxs-lookup"><span data-stu-id="54579-308">Notice that there is an SEO warning notifying that a **&lt;meta&gt;** tag for the page description is missing.</span></span> <span data-ttu-id="54579-309">Clique duas vezes na entrada de aviso do SEO para corrigi-lo.</span><span class="sxs-lookup"><span data-stu-id="54579-309">Double-click the SEO warning entry to fix it.</span></span>

    <span data-ttu-id="54579-310">![Janela Lista de Erros](visual-studio-2013-web-tools/_static/image32.png "Janela Lista de Erros")</span><span class="sxs-lookup"><span data-stu-id="54579-310">![Error List window](visual-studio-2013-web-tools/_static/image32.png "Error List window")</span></span>

    <span data-ttu-id="54579-311">*Janela Lista de Erros*</span><span class="sxs-lookup"><span data-stu-id="54579-311">*Error List window*</span></span>
3. <span data-ttu-id="54579-312">Na caixa de diálogo **Web Essentials** , clique em **Sim** para inserir uma descrição &lt;marca de meta&gt;.</span><span class="sxs-lookup"><span data-stu-id="54579-312">In the **Web Essentials** dialog box, click **Yes** to insert a description &lt;meta&gt; tag.</span></span>

    <span data-ttu-id="54579-313">![Caixa de diálogo Web Essentials](visual-studio-2013-web-tools/_static/image33.png "Caixa de diálogo Web Essentials")</span><span class="sxs-lookup"><span data-stu-id="54579-313">![Web Essentials dialog box](visual-studio-2013-web-tools/_static/image33.png "Web Essentials dialog box")</span></span>

    <span data-ttu-id="54579-314">*Caixa de diálogo Web Essentials*</span><span class="sxs-lookup"><span data-stu-id="54579-314">*Web Essentials dialog box*</span></span>
4. <span data-ttu-id="54579-315">O editor para **\_layout. cshtml** é aberto e a marca **&lt;meta&gt;** é adicionada automaticamente à seção de **cabeçalho** do arquivo HTML.</span><span class="sxs-lookup"><span data-stu-id="54579-315">The editor for **\_Layout.cshtml** opens and the **&lt;meta&gt;** tag is automatically added to the **head** section of the HTML file.</span></span>

    <span data-ttu-id="54579-316">![Marca meta adicionada automaticamente na página _Layout](visual-studio-2013-web-tools/_static/image34.png "Marca meta adicionada automaticamente na página _Layout")</span><span class="sxs-lookup"><span data-stu-id="54579-316">![Meta tag automatically added in _Layout page](visual-studio-2013-web-tools/_static/image34.png "Meta tag automatically added in _Layout page")</span></span>

    <span data-ttu-id="54579-317">*Marca meta adicionada automaticamente à página de layout \_*</span><span class="sxs-lookup"><span data-stu-id="54579-317">*Meta tag automatically added to \_Layout page*</span></span>
5. <span data-ttu-id="54579-318">Altere o valor do atributo **Content** para *GeekQuiz* e salve o arquivo.</span><span class="sxs-lookup"><span data-stu-id="54579-318">Change the value of the **content** attribute to *GeekQuiz* and save the file.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a><span data-ttu-id="54579-319">Exercício 2: tirando proveito dos trechos de código e do IntelliSense</span><span class="sxs-lookup"><span data-stu-id="54579-319">Exercise 2: Taking Advantage of Code Snippets and IntelliSense</span></span>

<span data-ttu-id="54579-320">Com o Web Essentials, o editor de HTML foi estendido com funcionalidade extra.</span><span class="sxs-lookup"><span data-stu-id="54579-320">With Web Essentials, the HTML editor has been extended with extra functionality.</span></span> <span data-ttu-id="54579-321">Neste exercício, você verá alguns recursos novos que são úteis ao desenvolver aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="54579-321">In this exercise, you will see some new features that are helpful when developing web applications.</span></span>

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a><span data-ttu-id="54579-322">Tarefa 1-usando o IntelliSense em documentos HTML</span><span class="sxs-lookup"><span data-stu-id="54579-322">Task 1 - Using IntelliSense in HTML Documents</span></span>

<span data-ttu-id="54579-323">O primeiro recurso novo que você verá nessa tarefa é chamado de **Dynamic IntelliSense**.</span><span class="sxs-lookup"><span data-stu-id="54579-323">The first new feature you will see in this task is called **Dynamic IntelliSense**.</span></span> <span data-ttu-id="54579-324">O IntelliSense dinâmico lê outras marcas e atributos para inferir as possíveis IDs que serão usadas.</span><span class="sxs-lookup"><span data-stu-id="54579-324">Dynamic IntelliSense reads other tags and attributes to infer the possible ids you will use.</span></span>

<span data-ttu-id="54579-325">Nesta tarefa, você criará um novo elemento de formulário HTML que contém um rótulo e um campo de entrada.</span><span class="sxs-lookup"><span data-stu-id="54579-325">In this task, you will create a new HTML form element which contains a label and an input field.</span></span> <span data-ttu-id="54579-326">Em seguida, você adicionará um atributo **for** ao rótulo para associá-lo à entrada e verá as sugestões do IntelliSense com base nas IDs das entradas no escopo.</span><span class="sxs-lookup"><span data-stu-id="54579-326">Then you will add a **for** attribute to the label to bind it to the input, and you will see IntelliSense suggestions based on the ids of the inputs in scope.</span></span>

1. <span data-ttu-id="54579-327">Abra **Visual Studio Express 2013 para Web** e a solução **begin. sln** localizada na pasta **Source/EX2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** .</span><span class="sxs-lookup"><span data-stu-id="54579-327">Open **Visual Studio Express 2013 for Web** and the **Begin.sln** solution located in the **Source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** folder.</span></span> <span data-ttu-id="54579-328">Como alternativa, você pode continuar com a solução que você obteve no exercício anterior.</span><span class="sxs-lookup"><span data-stu-id="54579-328">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="54579-329">No **Gerenciador de soluções**, abra o arquivo **index. cshtml** localizado nas **exibições** | pasta **base** .</span><span class="sxs-lookup"><span data-stu-id="54579-329">In **Solution Explorer**, open the **Index.cshtml** file located in the **Views** | **Home** folder.</span></span>
3. <span data-ttu-id="54579-330">Adicione o seguinte formulário dentro do elemento **&lt;seção&gt;** .</span><span class="sxs-lookup"><span data-stu-id="54579-330">Add the following form inside the **&lt;section&gt;** element.</span></span>

    <span data-ttu-id="54579-331">(Trecho de código- *VisualStudio2013WebTooling* - *EX2* - *Form*)</span><span class="sxs-lookup"><span data-stu-id="54579-331">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *Form*)</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. <span data-ttu-id="54579-332">A marca de entrada deve ser precedida por um rótulo com alguma descrição do campo.</span><span class="sxs-lookup"><span data-stu-id="54579-332">The input tag should be preceded by a label with some description of the field.</span></span> <span data-ttu-id="54579-333">Adicione o seguinte rótulo antes da marca de entrada.</span><span class="sxs-lookup"><span data-stu-id="54579-333">Add the following label before the input tag.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. <span data-ttu-id="54579-334">O atributo **for** de um **rótulo de&lt;&gt;** especifica a qual elemento de formulário um rótulo está associado.</span><span class="sxs-lookup"><span data-stu-id="54579-334">The **for** attribute of a **&lt;label&gt;** specifies which form element a label is bound to.</span></span> <span data-ttu-id="54579-335">O valor do atributo deve ser igual à ID do elemento relacionado.</span><span class="sxs-lookup"><span data-stu-id="54579-335">The attribute's value should be equal to the id of the related element.</span></span> <span data-ttu-id="54579-336">Adicione o atributo **for** ao elemento de **&lt;rótulo&gt;** .</span><span class="sxs-lookup"><span data-stu-id="54579-336">Add the **for** attribute to the **&lt;label&gt;** element.</span></span> <span data-ttu-id="54579-337">Conforme mostrado na figura a seguir, o nome da &quot;&quot; valor é exibido na caixa IntelliSense, com base na ID dos elementos dentro do mesmo escopo (o **&gt;do formulário de&lt;** de circunscrição).</span><span class="sxs-lookup"><span data-stu-id="54579-337">As shown in the following figure, the &quot;name&quot; value pops up in the IntelliSense box, based on the id of the elements within the same scope (the enclosing **&lt;form&gt;**).</span></span>

    <span data-ttu-id="54579-338">![Mostrando a ID no IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Mostrando a ID no IntelliSense")</span><span class="sxs-lookup"><span data-stu-id="54579-338">![Showing the id in IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Showing the id in IntelliSense")</span></span>

    <span data-ttu-id="54579-339">*Mostrando a ID no IntelliSense*</span><span class="sxs-lookup"><span data-stu-id="54579-339">*Showing the id in IntelliSense*</span></span>
6. <span data-ttu-id="54579-340">Exclua o **formulário de&lt;** adicionado recentemente&gt;elemento e seu conteúdo.</span><span class="sxs-lookup"><span data-stu-id="54579-340">Delete the recently added **&lt;form&gt;** element and its content.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a><span data-ttu-id="54579-341">Tarefa 2-usando trechos de código HTML</span><span class="sxs-lookup"><span data-stu-id="54579-341">Task 2 - Using HTML Code Snippets</span></span>

<span data-ttu-id="54579-342">O HTML5 introduziu mais de 25 novas marcas semânticas.</span><span class="sxs-lookup"><span data-stu-id="54579-342">HTML5 introduced more than 25 new semantic tags.</span></span> <span data-ttu-id="54579-343">O Visual Studio já tinha suporte ao IntelliSense para essas marcas, mas Visual Studio 2013 torna mais rápido e fácil escrever marcação adicionando novos trechos de código.</span><span class="sxs-lookup"><span data-stu-id="54579-343">Visual Studio already had IntelliSense support for these tags, but Visual Studio 2013 makes it faster and easier to write markup by adding new code snippets.</span></span> <span data-ttu-id="54579-344">Embora essas marcas não sejam complicadas, elas vêm com algumas sutilezas pequenas, como adicionar os fallbacks de codec corretos para a marca de *áudio* .</span><span class="sxs-lookup"><span data-stu-id="54579-344">Though these tags are not complicated, they come with a few small subtleties, such as adding the correct codec fallbacks for the *audio* tag.</span></span> <span data-ttu-id="54579-345">Nesta tarefa, você verá os trechos de código HTML para a marca de áudio.</span><span class="sxs-lookup"><span data-stu-id="54579-345">In this task, you will see the HTML code snippets for the audio tag.</span></span>

1. <span data-ttu-id="54579-346">No arquivo **index. cshtml** , digite **&lt;AUD** dentro da **seção&lt;&gt;** elemento, conforme mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="54579-346">In the **Index.cshtml** file, type **&lt;aud** inside the **&lt;section&gt;** element as shown in the following figure.</span></span>

    <span data-ttu-id="54579-347">![Inserindo um elemento de áudio](visual-studio-2013-web-tools/_static/image36.png "Inserindo um elemento de áudio")</span><span class="sxs-lookup"><span data-stu-id="54579-347">![Inserting an audio element](visual-studio-2013-web-tools/_static/image36.png "Inserting an audio element")</span></span>

    <span data-ttu-id="54579-348">*Inserindo um elemento de áudio*</span><span class="sxs-lookup"><span data-stu-id="54579-348">*Inserting an audio element*</span></span>
2. <span data-ttu-id="54579-349">Pressione **Tab** duas vezes e observe como o código a seguir é adicionado na página e o cursor é colocado no atributo **src** da primeira origem.</span><span class="sxs-lookup"><span data-stu-id="54579-349">Press **TAB** twice and notice how the following code is added on the page and the cursor is placed on the **src** attribute of the first source.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > <span data-ttu-id="54579-350">Ao pressionar a tecla **Tab** duas vezes, o trecho de código é inserido.</span><span class="sxs-lookup"><span data-stu-id="54579-350">By pressing the **TAB** key twice, the code snippet is inserted.</span></span> <span data-ttu-id="54579-351">O trecho de áudio mostra o uso padrão da marca de *áudio* , com dois arquivos de origem para obter suporte aprimorado.</span><span class="sxs-lookup"><span data-stu-id="54579-351">The audio snippet shows the standard usage of the *audio* tag, with two source files for improved support.</span></span>
3. <span data-ttu-id="54579-352">Exclua a segunda linha e atualize a origem da primeira linha com o link a seguir para o WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span><span class="sxs-lookup"><span data-stu-id="54579-352">Delete the second line and update the source of the first line with the following link to the WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3).</span></span> <span data-ttu-id="54579-353">O código resultante é mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="54579-353">The resulting code is shown below.</span></span>

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > <span data-ttu-id="54579-354">O arquivo de origem *KatanaProject. mp3* é usado como exemplo.</span><span class="sxs-lookup"><span data-stu-id="54579-354">The source file *KatanaProject.mp3* is used as an example.</span></span> <span data-ttu-id="54579-355">Você pode usar outra fonte, se preferir.</span><span class="sxs-lookup"><span data-stu-id="54579-355">You can use another source if you prefer.</span></span>
4. <span data-ttu-id="54579-356">Pressione **CTRL** + **S** para salvar o arquivo.</span><span class="sxs-lookup"><span data-stu-id="54579-356">Press **CTRL** + **S** to save the file.</span></span>
5. <span data-ttu-id="54579-357">Pressione **CTRL** + **F5** para iniciar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="54579-357">Press **CTRL** + **F5** to start the application.</span></span>
6. <span data-ttu-id="54579-358">Observe que um player de áudio foi adicionado ao aplicativo.</span><span class="sxs-lookup"><span data-stu-id="54579-358">Notice that an audio player was added to the application.</span></span>

    <span data-ttu-id="54579-359">![Player de áudio no Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Player de áudio no Internet Explorer")</span><span class="sxs-lookup"><span data-stu-id="54579-359">![Audio player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio player in Internet Explorer")</span></span>

    <span data-ttu-id="54579-360">*Player de áudio no Internet Explorer*</span><span class="sxs-lookup"><span data-stu-id="54579-360">*Audio player in Internet Explorer*</span></span>

    <span data-ttu-id="54579-361">![Player de áudio no Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Player de áudio no Google Chrome")</span><span class="sxs-lookup"><span data-stu-id="54579-361">![Audio player in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio player in Google Chrome")</span></span>

    <span data-ttu-id="54579-362">*Player de áudio no Google Chrome*</span><span class="sxs-lookup"><span data-stu-id="54579-362">*Audio player in Google Chrome*</span></span>
7. <span data-ttu-id="54579-363">Não feche os navegadores.</span><span class="sxs-lookup"><span data-stu-id="54579-363">Do not close the browsers.</span></span> <span data-ttu-id="54579-364">Você os usará na próxima tarefa.</span><span class="sxs-lookup"><span data-stu-id="54579-364">You will use them in the next task.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a><span data-ttu-id="54579-365">Tarefa 3-usando o IntelliSense em documentos JavaScript</span><span class="sxs-lookup"><span data-stu-id="54579-365">Task 3 - Using IntelliSense in JavaScript Documents</span></span>

<span data-ttu-id="54579-366">Com o Web Essentials 2013, as folhas de estilo e as páginas HTML produzem uma lista de IDs e nomes de classe.</span><span class="sxs-lookup"><span data-stu-id="54579-366">With Web Essentials 2013, style sheets and HTML pages produce a list of IDs and class names.</span></span> <span data-ttu-id="54579-367">Nesta tarefa, você aprenderá como essas listas aprimoram o suporte ao JavaScript IntelliSense no Web Essentials 2013.</span><span class="sxs-lookup"><span data-stu-id="54579-367">In this task, you will learn how those lists improve JavaScript IntelliSense support in Web Essentials 2013.</span></span>

1. <span data-ttu-id="54579-368">No arquivo **index. cshtml** , adicione o código a seguir para definir uma marca de **script** para o código JavaScript.</span><span class="sxs-lookup"><span data-stu-id="54579-368">In the **Index.cshtml** file, add the following code to define a **script** tag for JavaScript code.</span></span>

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. <span data-ttu-id="54579-369">Adicione o código a seguir dentro da marca de **script** para definir a função de retorno de chamada pronta.</span><span class="sxs-lookup"><span data-stu-id="54579-369">Add the following code inside the **script** tag to define the ready callback function.</span></span>

    <span data-ttu-id="54579-370">(Trecho de código- *VisualStudio2013WebTooling* - *EX2* - *ReadyFunction*)</span><span class="sxs-lookup"><span data-stu-id="54579-370">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. <span data-ttu-id="54579-371">Coloque o cursor na marca de **script** e pressione **Ctrl** +  **.**</span><span class="sxs-lookup"><span data-stu-id="54579-371">Place the caret in the **script** tag and press **CTRL** + **.**</span></span> <span data-ttu-id="54579-372">para abrir o menu de sugestões.</span><span class="sxs-lookup"><span data-stu-id="54579-372">to open the suggestion menu.</span></span>
4. <span data-ttu-id="54579-373">Clique em **extrair para arquivo**.</span><span class="sxs-lookup"><span data-stu-id="54579-373">Click **Extract To File**.</span></span>

    <span data-ttu-id="54579-374">![Sugestão de extração de JavaScript para arquivo](visual-studio-2013-web-tools/_static/image39.png "Sugestão de extração de JavaScript para arquivo")</span><span class="sxs-lookup"><span data-stu-id="54579-374">![JavaScript extract to file suggestion](visual-studio-2013-web-tools/_static/image39.png "JavaScript extract to file suggestion")</span></span>

    <span data-ttu-id="54579-375">*Sugestão de extração de JavaScript para arquivo*</span><span class="sxs-lookup"><span data-stu-id="54579-375">*JavaScript extract to file suggestion*</span></span>
5. <span data-ttu-id="54579-376">Na janela **salvar como** , selecione a pasta **scripts** , nomeie o arquivo como **init. js** e clique em **salvar**.</span><span class="sxs-lookup"><span data-stu-id="54579-376">In the **Save As** window, select the **Scripts** folder, name the file **init.js** and click **Save**.</span></span>

    <span data-ttu-id="54579-377">![Salvar como janela](visual-studio-2013-web-tools/_static/image40.png "Salvar como janela")</span><span class="sxs-lookup"><span data-stu-id="54579-377">![Save As window](visual-studio-2013-web-tools/_static/image40.png "Save As window")</span></span>

    <span data-ttu-id="54579-378">*Salvar como janela*</span><span class="sxs-lookup"><span data-stu-id="54579-378">*Save As window*</span></span>

    > [!NOTE]
    > <span data-ttu-id="54579-379">O arquivo **init. js** é criado e o conteúdo do script é movido para o arquivo.</span><span class="sxs-lookup"><span data-stu-id="54579-379">The **init.js** file is created and the content of the script is moved to the file.</span></span>
    > 
    > <span data-ttu-id="54579-380">![Arquivo init. js criado com o conteúdo incluído](visual-studio-2013-web-tools/_static/image41.png "Arquivo init. js criado com o conteúdo incluído")</span><span class="sxs-lookup"><span data-stu-id="54579-380">![Init.js file created with the content included](visual-studio-2013-web-tools/_static/image41.png "Init.js file created with the content included")</span></span>
    > 
    > <span data-ttu-id="54579-381">*Arquivo init. js criado com o conteúdo incluído*</span><span class="sxs-lookup"><span data-stu-id="54579-381">*Init.js file created with the content included*</span></span>
6. <span data-ttu-id="54579-382">Abra o arquivo **index. cshtml** e verifique se a marca do script foi substituída por uma referência ao arquivo **init. js** .</span><span class="sxs-lookup"><span data-stu-id="54579-382">Open the **Index.cshtml** file and check that the script tag was replaced with a reference to the **init.js** file.</span></span>

    <span data-ttu-id="54579-383">![Referência HTML init. js](visual-studio-2013-web-tools/_static/image42.png "Referência HTML init. js")</span><span class="sxs-lookup"><span data-stu-id="54579-383">![Init.js html reference](visual-studio-2013-web-tools/_static/image42.png "Init.js html reference")</span></span>

    <span data-ttu-id="54579-384">*Referência HTML init. js*</span><span class="sxs-lookup"><span data-stu-id="54579-384">*Init.js html reference*</span></span>
7. <span data-ttu-id="54579-385">Vá para a **Gerenciador de soluções** e observe que o arquivo **init. js** foi incluído automaticamente na solução.</span><span class="sxs-lookup"><span data-stu-id="54579-385">Go to the **Solution Explorer** and notice that the **init.js** file was included automatically in the solution.</span></span>

    <span data-ttu-id="54579-386">![Arquivo init. js incluído na solução](visual-studio-2013-web-tools/_static/image43.png "Arquivo init. js incluído na solução")</span><span class="sxs-lookup"><span data-stu-id="54579-386">![Init.js file included in solution](visual-studio-2013-web-tools/_static/image43.png "Init.js file included in solution")</span></span>

    <span data-ttu-id="54579-387">*Arquivo init. js incluído na solução*</span><span class="sxs-lookup"><span data-stu-id="54579-387">*Init.js file included in solution*</span></span>
8. <span data-ttu-id="54579-388">Volte para o arquivo **init. js** para atualizar o retorno de chamada da função **pronta** .</span><span class="sxs-lookup"><span data-stu-id="54579-388">Switch back to the **init.js** file to update the **ready** function callback.</span></span>
9. <span data-ttu-id="54579-389">Dentro da definição de retorno de chamada de função que é passada para *pronto*, adicione o código a seguir para obter todos os elementos por um atributo de classe específico.</span><span class="sxs-lookup"><span data-stu-id="54579-389">Inside the function callback definition that is passed to *ready*, add the following code to get all the elements by a specific class attribute.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. <span data-ttu-id="54579-390">Pressione **CTRL** + **espaço** entre as aspas dentro da chamada de função **getElementsByClassName** .</span><span class="sxs-lookup"><span data-stu-id="54579-390">Press **CTRL** + **Space** between the quotes inside the **getElementsByClassName** function call.</span></span>

    <span data-ttu-id="54579-391">![Mostrando o IntelliSense para a função getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "Mostrando o IntelliSense para a função getElementsByClassName")</span><span class="sxs-lookup"><span data-stu-id="54579-391">![Showing IntelliSense for the getElementsByClassName function](visual-studio-2013-web-tools/_static/image44.png "Showing IntelliSense for the getElementsByClassName function")</span></span>

    <span data-ttu-id="54579-392">*Mostrando o IntelliSense para a função getElementsByClassName*</span><span class="sxs-lookup"><span data-stu-id="54579-392">*Showing IntelliSense for the getElementsByClassName function*</span></span>

    > [!NOTE]
    > <span data-ttu-id="54579-393">Observe que o IntelliSense mostra as classes definidas nas folhas de estilo do projeto.</span><span class="sxs-lookup"><span data-stu-id="54579-393">Notice that IntelliSense shows the classes defined in the project style sheets.</span></span>
11. <span data-ttu-id="54579-394">Substitua a linha que você criou com o código a seguir.</span><span class="sxs-lookup"><span data-stu-id="54579-394">Replace the line that you have created with the following code.</span></span>

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. <span data-ttu-id="54579-395">Posicione o cursor após a **au** dentro das aspas na função **GetElementsByTagName** e pressione **Ctrl** + **espaço**.</span><span class="sxs-lookup"><span data-stu-id="54579-395">Position the cursor after **au** inside the quotes in the **getElementsByTagName** function and press **CTRL** + **Space**.</span></span>

    <span data-ttu-id="54579-396">![Mostrando o IntelliSense para o método getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "Mostrando o IntelliSense para o método getElementByTagName")</span><span class="sxs-lookup"><span data-stu-id="54579-396">![Showing IntelliSense for the getElementByTagName method](visual-studio-2013-web-tools/_static/image45.png "Showing IntelliSense for the getElementByTagName method")</span></span>

    <span data-ttu-id="54579-397">*Mostrando o IntelliSense para o método getElementsByTagName*</span><span class="sxs-lookup"><span data-stu-id="54579-397">*Showing IntelliSense for the getElementsByTagName method*</span></span>
13. <span data-ttu-id="54579-398">Selecione **&quot;&quot;de áudio** na lista e pressione **Enter**.</span><span class="sxs-lookup"><span data-stu-id="54579-398">Select **&quot;audio&quot;** from the list and press **ENTER**.</span></span> <span data-ttu-id="54579-399">O resultado é mostrado na figura a seguir.</span><span class="sxs-lookup"><span data-stu-id="54579-399">The result is shown in the following figure.</span></span>

    <span data-ttu-id="54579-400">![Recuperando elementos de áudio](visual-studio-2013-web-tools/_static/image46.png "Recuperando elementos de áudio")</span><span class="sxs-lookup"><span data-stu-id="54579-400">![Retrieving Audio Elements](visual-studio-2013-web-tools/_static/image46.png "Retrieving Audio Elements")</span></span>

    <span data-ttu-id="54579-401">*Recuperando elementos de áudio*</span><span class="sxs-lookup"><span data-stu-id="54579-401">*Retrieving Audio Elements*</span></span>
14. <span data-ttu-id="54579-402">Em **Gerenciador de soluções**, clique com o botão direito do mouse no arquivo **init. js** na pasta **scripts** e selecione **arquivo (s) JavaScript reduzir** no menu do **Web Essentials** .</span><span class="sxs-lookup"><span data-stu-id="54579-402">In **Solution Explorer**, right-click the **init.js** file in the **Scripts** folder and select **Minify JavaScript file(s)** from the **Web Essentials** menu.</span></span>

    <span data-ttu-id="54579-403">![Arquivo (s) reduzir JavaScript](visual-studio-2013-web-tools/_static/image47.png "Arquivos JavaScript reduzir")</span><span class="sxs-lookup"><span data-stu-id="54579-403">![Minify JavaScript file(s)](visual-studio-2013-web-tools/_static/image47.png "Minify JavaScript files")</span></span>

    <span data-ttu-id="54579-404">*Arquivo (s) reduzir JavaScript*</span><span class="sxs-lookup"><span data-stu-id="54579-404">*Minify JavaScript file(s)*</span></span>
15. <span data-ttu-id="54579-405">Quando for solicitado a habilitar o minificação automático quando o arquivo de origem for alterado, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="54579-405">When prompted to enable automatic minification when the source file changes click **Yes**.</span></span>

    <span data-ttu-id="54579-406">![Habilitando aviso automático de minificação](visual-studio-2013-web-tools/_static/image48.png "Habilitando aviso automático de minificação")</span><span class="sxs-lookup"><span data-stu-id="54579-406">![Enabling automatic minification warning](visual-studio-2013-web-tools/_static/image48.png "Enabling automatic minification warning")</span></span>

    <span data-ttu-id="54579-407">*Habilitando aviso automático de minificação*</span><span class="sxs-lookup"><span data-stu-id="54579-407">*Enabling automatic minification warning*</span></span>

    > [!NOTE]
    > <span data-ttu-id="54579-408">O **init. min. js** é criado e adicionado como uma dependência do arquivo **init. js** .</span><span class="sxs-lookup"><span data-stu-id="54579-408">The **init.min.js** is created and is added as a dependency of the **init.js** file.</span></span>
    > 
    > <span data-ttu-id="54579-409">![Arquivo init. min. js criado](visual-studio-2013-web-tools/_static/image49.png "Arquivo init. min. js criado")</span><span class="sxs-lookup"><span data-stu-id="54579-409">![Init.min.js file created](visual-studio-2013-web-tools/_static/image49.png "Init.min.js file created")</span></span>
    > 
    > <span data-ttu-id="54579-410">*Arquivo init. min. js criado*</span><span class="sxs-lookup"><span data-stu-id="54579-410">*Init.min.js file created*</span></span>
16. <span data-ttu-id="54579-411">Abra o arquivo **init. min. js** e observe que o arquivo é reduzidos.</span><span class="sxs-lookup"><span data-stu-id="54579-411">Open the **init.min.js** file and notice that the file is minified.</span></span>

    <span data-ttu-id="54579-412">![Conteúdo do arquivo init. min. js](visual-studio-2013-web-tools/_static/image50.png "Conteúdo do arquivo init. min. js")</span><span class="sxs-lookup"><span data-stu-id="54579-412">![Init.min.js file content](visual-studio-2013-web-tools/_static/image50.png "Init.min.js file content")</span></span>

    <span data-ttu-id="54579-413">*Conteúdo do arquivo init. min. js*</span><span class="sxs-lookup"><span data-stu-id="54579-413">*Init.min.js file content*</span></span>
17. <span data-ttu-id="54579-414">No arquivo **init. js** , adicione o seguinte código abaixo da chamada de função **GetElementsByTagName** para reproduzir todos os elementos de áudio.</span><span class="sxs-lookup"><span data-stu-id="54579-414">In the **init.js** file, add the following code below the **getElementsByTagName** function call to play all the audio elements.</span></span>

    <span data-ttu-id="54579-415">(Trecho de código- *VisualStudio2013WebTooling* - *EX2* - *PlayAudioElements*)</span><span class="sxs-lookup"><span data-stu-id="54579-415">(Code Snippet - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)</span></span>

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. <span data-ttu-id="54579-416">Clique em **CTRL** + **S** para salvar o arquivo.</span><span class="sxs-lookup"><span data-stu-id="54579-416">Click **CTRL** + **S** to save the file.</span></span> <span data-ttu-id="54579-417">Como o arquivo reduzidos já está aberto, você verá uma caixa de diálogo informando que o arquivo foi modificado fora do editor de origem.</span><span class="sxs-lookup"><span data-stu-id="54579-417">Since the minified file is already opened, you will see a dialog box saying that the file was modified outside of the source editor.</span></span> <span data-ttu-id="54579-418">Clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="54579-418">Click **Yes**.</span></span>

    <span data-ttu-id="54579-419">![Aviso de Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "Aviso de Microsoft Visual Studio")</span><span class="sxs-lookup"><span data-stu-id="54579-419">![Microsoft Visual Studio warning](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="54579-420">*Aviso de Microsoft Visual Studio*</span><span class="sxs-lookup"><span data-stu-id="54579-420">*Microsoft Visual Studio warning*</span></span>
19. <span data-ttu-id="54579-421">Volte para o arquivo **init. min. js** para verificar se o arquivo foi atualizado com o novo código.</span><span class="sxs-lookup"><span data-stu-id="54579-421">Switch back to the **init.min.js** file to verify that the file was updated with the new code.</span></span>

    <span data-ttu-id="54579-422">![Arquivo init. min. js atualizado](visual-studio-2013-web-tools/_static/image52.png "Arquivo init. min. js atualizado")</span><span class="sxs-lookup"><span data-stu-id="54579-422">![Init.min.js file updated](visual-studio-2013-web-tools/_static/image52.png "Init.min.js file updated")</span></span>

    <span data-ttu-id="54579-423">*Arquivo init. min. js atualizado*</span><span class="sxs-lookup"><span data-stu-id="54579-423">*Init.min.js file updated*</span></span>
20. <span data-ttu-id="54579-424">Clique no botão **Atualizar do link do navegador** .</span><span class="sxs-lookup"><span data-stu-id="54579-424">Click the **Browser Link Refresh** button.</span></span>
21. <span data-ttu-id="54579-425">Depois que ambos os navegadores forem atualizados, os players de áudio vistos na tarefa anterior começarão a ser automaticamente reproduzidos.</span><span class="sxs-lookup"><span data-stu-id="54579-425">Once both browsers are refreshed the audio players you saw in the previous task will start playing automatically.</span></span>

    <span data-ttu-id="54579-426">![Player de áudio incluído na exibição](visual-studio-2013-web-tools/_static/image53.png "Player de áudio incluído na exibição")</span><span class="sxs-lookup"><span data-stu-id="54579-426">![Audio player included in view](visual-studio-2013-web-tools/_static/image53.png "Audio player included in view")</span></span>

    <span data-ttu-id="54579-427">*Player de áudio incluído na exibição*</span><span class="sxs-lookup"><span data-stu-id="54579-427">*Audio player included in view*</span></span>

---

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="54579-428">Resumo</span><span class="sxs-lookup"><span data-stu-id="54579-428">Summary</span></span>

<span data-ttu-id="54579-429">Ao concluir este laboratório prático, você aprendeu a:</span><span class="sxs-lookup"><span data-stu-id="54579-429">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="54579-430">Usar novos recursos do editor de HTML incluídos no Web Essentials, como trechos de código do HTML5 e codificação de Zen avançados</span><span class="sxs-lookup"><span data-stu-id="54579-430">Use new HTML editor features included in Web Essentials such as rich HTML5 code snippets and Zen coding</span></span>
- <span data-ttu-id="54579-431">Usar os novos recursos do editor de CSS incluídos no Web Essentials, como o seletor de cores e a dica de ferramenta da matriz do navegador</span><span class="sxs-lookup"><span data-stu-id="54579-431">Use new CSS editor features included in Web Essentials such as the Color picker and Browser matrix tooltip</span></span>
- <span data-ttu-id="54579-432">Usar novos recursos do editor de JavaScript incluídos no Web Essentials, como extrair para arquivo e IntelliSense para todos os elementos HTML</span><span class="sxs-lookup"><span data-stu-id="54579-432">Use new JavaScript editor features included in Web Essentials such as Extract to File and IntelliSense for all HTML elements</span></span>
- <span data-ttu-id="54579-433">Trocar dados entre o navegador e o Visual Studio usando o link do navegador</span><span class="sxs-lookup"><span data-stu-id="54579-433">Exchange data between your browser and Visual Studio using Browser Link</span></span>
