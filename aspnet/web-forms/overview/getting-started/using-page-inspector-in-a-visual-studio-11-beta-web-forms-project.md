---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Usando o Inspetor de Página para o Visual Studio 2012 no ASP.NET Web Forms | Microsoft Docs
author: rick-anderson
description: O Inspetor de Página para Visual Studio 2012 é uma ferramenta de desenvolvimento para a Web com um navegador integrado. Selecione qualquer elemento no navegador integrado e Inspetor de Página...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: c165bbea505b4cb8eae1312cdd587f4ed36541a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78575918"
---
# <a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a><span data-ttu-id="6a7f9-104">Uso do Inspetor de Página para Visual Studio 2012 em Web Forms do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6a7f9-104">Using Page Inspector for Visual Studio 2012 in ASP.NET Web Forms</span></span>

<span data-ttu-id="6a7f9-105">por Tim Ammann</span><span class="sxs-lookup"><span data-stu-id="6a7f9-105">by Tim Ammann</span></span>

> <span data-ttu-id="6a7f9-106">O Inspetor de Página para Visual Studio 2012 é uma ferramenta de desenvolvimento para a Web com um navegador integrado.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-106">Page Inspector for Visual Studio 2012 is a web development tool with an integrated browser.</span></span> <span data-ttu-id="6a7f9-107">Selecione qualquer elemento no navegador integrado e Inspetor de Página realça instantaneamente a origem e o CSS do elemento.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-107">Select any element in the integrated browser, and Page Inspector instantly highlights the element's source and CSS.</span></span> <span data-ttu-id="6a7f9-108">Você pode procurar qualquer página em seu aplicativo, localizar rapidamente as fontes de marcação renderizada e usar as ferramentas de navegador diretamente no ambiente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-108">You can browse any page in your application, quickly find the sources of rendered markup, and use browser tools right within the Visual Studio environment.</span></span>
> 
> <span data-ttu-id="6a7f9-109">Este tutorial mostra como habilitar Modo de Inspeção e, em seguida, localizar e editar rapidamente as regras e o texto do CSS em seu projeto Web.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-109">This tutorial shows how to enable Inspection Mode and then quickly locate and edit CSS rules and text within your web project.</span></span> <span data-ttu-id="6a7f9-110">O tutorial usa um projeto de aplicativo Web Forms, mas você também pode usar Inspetor de Página para projetos de site e aplicativos [MVC](https://go.microsoft.com/?linkid=9802002) .</span><span class="sxs-lookup"><span data-stu-id="6a7f9-110">The tutorial uses a Web Forms Application Project, but you can also use Page Inspector for Web Site projects and [MVC](https://go.microsoft.com/?linkid=9802002) applications.</span></span>
> 
> <span data-ttu-id="6a7f9-111">O tutorial tem as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="6a7f9-111">The tutorial has the following sections:</span></span>
> 
> [<span data-ttu-id="6a7f9-112">Pré-requisitos</span><span class="sxs-lookup"><span data-stu-id="6a7f9-112">Prerequisites</span></span>](#_1_prerequisites)
> 
> [<span data-ttu-id="6a7f9-113">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="6a7f9-113">Create a Web Application</span></span>](#_2_creating_a)
> 
> [<span data-ttu-id="6a7f9-114">Usar Inspetor de Página para exibir o aplicativo</span><span class="sxs-lookup"><span data-stu-id="6a7f9-114">Use Page Inspector to View the Application</span></span>](#_3_using_page)
> 
> [<span data-ttu-id="6a7f9-115">Habilitar Modo de Inspeção</span><span class="sxs-lookup"><span data-stu-id="6a7f9-115">Enable Inspection Mode</span></span>](#_4_inspection_mode)
> 
> [<span data-ttu-id="6a7f9-116">Usar Inspetor de Página para fazer alterações na marcação</span><span class="sxs-lookup"><span data-stu-id="6a7f9-116">Use Page Inspector to Make Changes to Markup</span></span>](#_5_using_page)
> 
> [<span data-ttu-id="6a7f9-117">Modo de Inspeção e a janela HTML</span><span class="sxs-lookup"><span data-stu-id="6a7f9-117">Inspection Mode and the HTML Window</span></span>](#_6_inspection_mode)
> 
> [<span data-ttu-id="6a7f9-118">Visualizar alterações de CSS na janela estilos</span><span class="sxs-lookup"><span data-stu-id="6a7f9-118">Preview CSS Changes in the Styles Window</span></span>](#_7_previewing_css)
> 
> [<span data-ttu-id="6a7f9-119">Sincronização automática de CSS</span><span class="sxs-lookup"><span data-stu-id="6a7f9-119">CSS Auto Sync</span></span>](#css_auto_sync)
> 
> [<span data-ttu-id="6a7f9-120">Usando o seletor de cores CSS</span><span class="sxs-lookup"><span data-stu-id="6a7f9-120">Using the CSS Color Picker</span></span>](#css_color_picker)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="6a7f9-121">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="6a7f9-121">Prerequisites</span></span>

- <span data-ttu-id="6a7f9-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) ou [Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="6a7f9-122">[Visual Studio 2012](https://www.microsoft.com/visualstudio/11) or [Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span>

> [!NOTE]
> <span data-ttu-id="6a7f9-123">Para obter a versão mais recente do Inspetor de Página, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar o SDK do Azure para .NET 2,0.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-123">To get the latest version of Page Inspector, use [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=255386) to install the Azure SDK for .NET 2.0.</span></span>

<span data-ttu-id="6a7f9-124">Inspetor de Página é agrupado com Microsoft Web Developer Tools.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-124">Page Inspector is bundled with Microsoft Web Developer Tools.</span></span> <span data-ttu-id="6a7f9-125">A versão mais recente é 1,3.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-125">The latest version is 1.3.</span></span> <span data-ttu-id="6a7f9-126">Para verificar qual versão você tem, execute o Visual Studio e selecione **sobre Microsoft Visual Studio** no menu **ajuda** .</span><span class="sxs-lookup"><span data-stu-id="6a7f9-126">To check which version you have, run Visual Studio and select **About Microsoft Visual Studio** from the **Help** menu.</span></span>

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a><span data-ttu-id="6a7f9-127">Criar um aplicativo Web</span><span class="sxs-lookup"><span data-stu-id="6a7f9-127">Create a Web Application</span></span>

<span data-ttu-id="6a7f9-128">Primeiro, você criará um aplicativo Web com o qual usará Inspetor de Página.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-128">First, you will create a web application that you will use Page Inspector with.</span></span> <span data-ttu-id="6a7f9-129">No Visual Studio, escolha **arquivo** &gt; **novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-129">In Visual Studio, choose **File** &gt; **New Project**.</span></span> <span data-ttu-id="6a7f9-130">À esquerda, expanda **Visual C#** , selecione **Web**e, em seguida, selecione **ASP.NET Web Forms aplicativo**.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-130">On the left, expand **Visual C#**, select **Web**, and then select **ASP.NET Web Forms Application**.</span></span>

![Novo aplicativo Web Forms](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

<span data-ttu-id="6a7f9-132">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-132">Click **OK**.</span></span>

<span data-ttu-id="6a7f9-133">O aplicativo é aberto no modo de exibição de **código-fonte** .</span><span class="sxs-lookup"><span data-stu-id="6a7f9-133">The application opens in **Source** view.</span></span>

![Novo aplicativo Web Forms no modo de exibição de origem](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

<span data-ttu-id="6a7f9-135">Agora que você tem um aplicativo com o qual trabalhar, você pode usar Inspetor de Página para examiná-lo e modificá-lo.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-135">Now that you have an application to work with, you can use Page Inspector to examine and modify it.</span></span>

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a><span data-ttu-id="6a7f9-136">Usar Inspetor de Página para exibir o aplicativo</span><span class="sxs-lookup"><span data-stu-id="6a7f9-136">Use Page Inspector to View the Application</span></span>

<span data-ttu-id="6a7f9-137">Em seguida, você exibirá o aplicativo com Inspetor de Página.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-137">Next, you will view the application with Page Inspector.</span></span> <span data-ttu-id="6a7f9-138">Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e escolha **Exibir no Inspetor de página**.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-138">In **Solution Explorer**, right click the project, and then choose **View in Page Inspector**.</span></span>

![Exibir em inspetor de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

<span data-ttu-id="6a7f9-140">Por padrão, quando Inspetor de Página é iniciado pela primeira vez, ele é encaixado como uma janela estreita no lado esquerdo do ambiente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-140">By default, when Page Inspector launches for the first time, it is docked as a narrow window on the left side of the Visual Studio environment.</span></span> <span data-ttu-id="6a7f9-141">Deixe-o encaixado no lado esquerdo e defina-o com uma largura que seja confortável para você ou encaixe-o em uma das áreas de ferramentas na parte superior, inferior ou direita:</span><span class="sxs-lookup"><span data-stu-id="6a7f9-141">Leave it docked on the left side and set it to a width that is comfortable for you, or dock it in one of the tool areas on the top, bottom, or right:</span></span>

![Inspetor de Página posições de encaixe](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

<span data-ttu-id="6a7f9-143">Se você desencaixar a janela Inspetor de Página, poderá colocá-la fora do Visual Studio ou até mesmo em um segundo monitor, se tiver uma.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-143">If you undock the Page Inspector window, you can place it outside Visual Studio, or even on a second monitor if you have one.</span></span> <span data-ttu-id="6a7f9-144">No entanto, para ALT + TAB entre Inspetor de Página e o Visual Studio quando a janela de Inspetor de Página estiver desencaixada, vá para **ferramentas** &gt; **opções** &gt; **ambiente** &gt; **guias e janelas**e, em bem com a **guia**, desmarque a caixa de seleção chamada **Windows de ferramenta flutuante sempre fique na parte superior da janela principal**:</span><span class="sxs-lookup"><span data-stu-id="6a7f9-144">However, in order to ALT+TAB between Page Inspector and Visual Studio when the Page Inspector window is undocked, go to **Tools** &gt; **Options** &gt; **Environment** &gt; **Tabs and Windows**, and under **Tab Well**, clear the check box called **Floating tool windows always stay on top of the main window**:</span></span>

![Desmarque a caixa de seleção janelas de ferramentas flutuantes para ALT + TAB entre o Visual Studio e a janela de Inspetor de Página desencaixada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

<span data-ttu-id="6a7f9-146">O painel superior da janela de Inspetor de Página mostra a página atual em uma janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-146">The top pane of the Page Inspector window shows the current page in a browser window.</span></span> <span data-ttu-id="6a7f9-147">O painel inferior mostra a página na marcação HTML à esquerda e algumas guias à direita que permitem inspecionar diferentes aspectos da página.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-147">The bottom pane shows the page in HTML markup on the left, and some tabs on the right that let you inspect different aspects of the page.</span></span> <span data-ttu-id="6a7f9-148">O painel inferior é semelhante ao [ferramentas para desenvolvedores F12](https://msdn.microsoft.com/ie/aa740478) no Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-148">The bottom pane is similar to the [F12 Developer Tools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.</span></span> <span data-ttu-id="6a7f9-149">(No entanto, ao contrário das ferramentas de desenvolvedor, você pode usar Inspetor de Página diretamente no Visual Studio.)</span><span class="sxs-lookup"><span data-stu-id="6a7f9-149">(However, unlike the developer tools, you can use Page Inspector right within Visual Studio.)</span></span>

![{1&gt;Inspetor de Página&lt;1}](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

<span data-ttu-id="6a7f9-151">Neste tutorial, você usará o painel navegador Inspetor de Página e as guias **HTML** e **estilos** para ajudá-lo a navegar rapidamente e fazer alterações no aplicativo.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-151">In this tutorial, you will use the Page Inspector browser pane, and the **HTML** and **Styles** tabs to help you rapidly navigate and make changes to the application.</span></span>

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a><span data-ttu-id="6a7f9-152">Habilitar Modo de Inspeção</span><span class="sxs-lookup"><span data-stu-id="6a7f9-152">Enable Inspection Mode</span></span>

<span data-ttu-id="6a7f9-153">Em seguida, você verá como o Modo de Inspeção de Inspetor de Página funciona.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-153">Next, you will see how Page Inspector's Inspection Mode works.</span></span> <span data-ttu-id="6a7f9-154">Na janela Inspetor de Página, clique no botão **inspecionar** .</span><span class="sxs-lookup"><span data-stu-id="6a7f9-154">In the Page Inspector window, click the **Inspect** button.</span></span>

![Inspecionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

<span data-ttu-id="6a7f9-156">Para ver o modo de inspeção em ação, mova o mouse sobre diferentes partes da página dentro da janela Inspetor de Página navegador.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-156">To see inspection mode in action, move your mouse over different parts of the page within the Page Inspector browser window.</span></span> <span data-ttu-id="6a7f9-157">Como você faz, o ponteiro do mouse muda para um sinal de adição grande e o elemento abaixo é realçado:</span><span class="sxs-lookup"><span data-stu-id="6a7f9-157">As you do, the mouse pointer changes to a large plus sign, and the element underneath is highlighted:</span></span>

![Passando o mouse sobre div. Content-Wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

<span data-ttu-id="6a7f9-159">À medida que você move o ponteiro do mouse, observe que</span><span class="sxs-lookup"><span data-stu-id="6a7f9-159">As you move the mouse pointer, note that</span></span>

- <span data-ttu-id="6a7f9-160">O conteúdo no modo de exibição de **origem** é alterado para mostrar a marcação correspondente ao elemento selecionado na página.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-160">The content in **Source** view changes to show the markup corresponding to the selected element on the page.</span></span> <span data-ttu-id="6a7f9-161">A marcação relevante é realçada.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-161">The relevant markup is highlighted.</span></span> <span data-ttu-id="6a7f9-162">Se a origem estiver em outro arquivo, esse arquivo será aberto no modo de exibição de origem com a marcação relevante realçada.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-162">If the source is in another file, that file is opened in Source view with the relevant markup highlighted.</span></span>

- <span data-ttu-id="6a7f9-163">A marcação exibida na guia **HTML** em Inspetor de página também é alterada para corresponder ao elemento selecionado na página.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-163">The markup displayed in the **HTML** tab in Page Inspector also changes to correspond to the selected element on the page.</span></span> <span data-ttu-id="6a7f9-164">Na guia **HTML** , a marcação relevante é descrita.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-164">In the **HTML** tab, the relevant markup is outlined.</span></span>

- <span data-ttu-id="6a7f9-165">A guia **estilos** mostra as regras de CSS relevantes para a seleção atual.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-165">The **Styles** tab shows the CSS rules relevant to the current selection.</span></span>

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a><span data-ttu-id="6a7f9-166">Usar Inspetor de Página para fazer alterações na marcação</span><span class="sxs-lookup"><span data-stu-id="6a7f9-166">Use Page Inspector to Make Changes to Markup</span></span>

<span data-ttu-id="6a7f9-167">Agora, você verá como você pode usar Inspetor de Página para localizar e fazer alterações na marcação ou texto cujo local pode não ser imediatamente óbvio.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-167">Now you will see how you can use Page Inspector to find and make changes to markup or text whose location might not be immediately obvious.</span></span>

<span data-ttu-id="6a7f9-168">Coloque Inspetor de Página em Modo de Inspeção e, em seguida, role até a parte inferior da home page.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-168">Put Page Inspector in Inspection Mode and then scroll to the bottom of the home page.</span></span>

<span data-ttu-id="6a7f9-169">Assim que você inserir a área de rodapé, Inspetor de Página abrirá o arquivo de layout *site. Master* na exibição de **origem** em uma guia temporária à direita das outras guias e destacará a seção da página mestra que você selecionou.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-169">As soon as you enter the footer area, Page Inspector opens the *Site.Master* layout file in **Source** view in a temporary tab to the right of the other tabs and highlights the section of the master page that you have selected.</span></span> <span data-ttu-id="6a7f9-170">Isso mostra como Inspetor de Página pode localizar e exibir conteúdo em uma página que pode realmente vir de um arquivo diferente daquele que você abriu originalmente.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-170">This shows you how Page Inspector can find and display content on a page that might actually come from a different file than the one you originally opened.</span></span>

![Realces de rodapé no Modo de Inspeção](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

<span data-ttu-id="6a7f9-172">Na janela do navegador Inspetor de Página, mova o ponteiro do mouse sobre a linha com o <a id="a"> </a>aviso de direitos autorais.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-172">In the Page Inspector browser window, move your mouse pointer over the line with the copyright <a id="a"></a>notice.</span></span>

<span data-ttu-id="6a7f9-173">Na página *site. Master* , a linha correspondente é realçada.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-173">In the *Site.Master* page, the corresponding line is highlighted.</span></span>

![Linha de copyright de rodapé realçada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

<span data-ttu-id="6a7f9-175">Adicione texto ao final da linha no arquivo *site. Master* .</span><span class="sxs-lookup"><span data-stu-id="6a7f9-175">Add some text to the end of the line in the *Site.Master* file.</span></span>

<span data-ttu-id="6a7f9-176">&lt;p&gt;&amp;cópia; &lt;%: DateTime. Now. ano%&gt;-meu aplicativo ASP.NET Rocks!&lt;/p&gt;</span><span class="sxs-lookup"><span data-stu-id="6a7f9-176">&lt;p&gt;&amp;copy; &lt;%: DateTime.Now.Year %&gt; - My ASP.NET Application Rocks!&lt;/p&gt;</span></span>

<span data-ttu-id="6a7f9-177">Agora, pressione Ctrl + Alt + Enter ou clique na barra de atualização para ver os resultados na janela do navegador Inspetor de Página.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-177">Now, press Ctrl+Alt+Enter or click the Update Bar to see the results in the Page Inspector browser window.</span></span>

![Meu aplicativo ASP.NET Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

<span data-ttu-id="6a7f9-179">Talvez você tenha pensado que o rodapé estava na página *Default. aspx* , mas que ele se tornou na página de layout mestre e Inspetor de página encontrá-lo para você.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-179">You might have thought that the footer was on the *Default.aspx* page, but it turned out to be in the master layout page, and Page Inspector found it for you.</span></span>

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a><span data-ttu-id="6a7f9-180">Modo de Inspeção e a janela HTML</span><span class="sxs-lookup"><span data-stu-id="6a7f9-180">Inspection Mode and the HTML Window</span></span>

<span data-ttu-id="6a7f9-181">Em seguida, você terá uma visão rápida da janela HTML e de como ela mapeia os elementos para você.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-181">Next, you will have a quick look at the HTML window and how it maps elements for you.</span></span>

<span data-ttu-id="6a7f9-182">Coloque Inspetor de Página em Modo de Inspeção.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-182">Put Page Inspector in Inspection Mode.</span></span>

![Inspecionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

<span data-ttu-id="6a7f9-184">Clique na parte superior da página que diz "seu logotipo aqui".</span><span class="sxs-lookup"><span data-stu-id="6a7f9-184">Click the top part of the page that says "your logo here".</span></span> <span data-ttu-id="6a7f9-185">Você está examinando um elemento específico em mais detalhes, portanto, a exibição na janela do navegador não é mais alterada à medida que você move o ponteiro do mouse.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-185">You are examining a particular element in more detail, so the display in the browser window no longer changes as you move the mouse pointer.</span></span>

<span data-ttu-id="6a7f9-186">Agora, mova o ponteiro do mouse para a janela **HTML** .</span><span class="sxs-lookup"><span data-stu-id="6a7f9-186">Now move the mouse pointer to the **HTML** window.</span></span> <span data-ttu-id="6a7f9-187">À medida que você move o ponteiro do mouse, Inspetor de Página descreve o elemento dentro da janela **HTML** e realça o elemento correspondente na janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-187">As you move the mouse pointer, Page Inspector outlines the element within the **HTML** window and highlights the corresponding element in the browser window.</span></span>

![Janela HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

<span data-ttu-id="6a7f9-189">Como antes, Inspetor de Página abre o arquivo *site. Master* para você em uma guia temporária. Clique na guia site. Master e a marcação correspondente será realçada no cabeçalho &lt;&gt; seção:</span><span class="sxs-lookup"><span data-stu-id="6a7f9-189">As before, Page Inspector opens the *Site.Master* file for you in a temporary tab. Click the Site.Master tab, and the corresponding markup is highlighted in the &lt;header&gt; section:</span></span>

![Marcação realçada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a><span data-ttu-id="6a7f9-191">Visualizar alterações de CSS na janela estilos</span><span class="sxs-lookup"><span data-stu-id="6a7f9-191">Preview CSS Changes in the Styles Window</span></span>

<span data-ttu-id="6a7f9-192">Em seguida, você verá como é possível usar a janela **estilos** de Inspetor de página para visualizar alterações no CSS.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-192">Next, you will see how you can use the Page Inspector **Styles** window to preview changes to CSS.</span></span>

<span data-ttu-id="6a7f9-193">Clique no botão **inspecionar** para colocar Inspetor de Página em modo de inspeção.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-193">Click the **Inspect** button to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="6a7f9-194">Na janela do navegador Inspetor de Página, mova o ponteiro do mouse sobre a seção "Home Page" até que o rótulo **div. Content-wrapper** seja exibido.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-194">In the Page Inspector browser window, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span>

![Focalizando elementos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

<span data-ttu-id="6a7f9-196">Clique na seção div. Content-wrapper uma vez e mova o ponteiro do mouse para a janela **estilos** .</span><span class="sxs-lookup"><span data-stu-id="6a7f9-196">Click within the div.content-wrapper section once, and then move the mouse pointer to the **Styles** window.</span></span> <span data-ttu-id="6a7f9-197">No seletor de classe. Content-wrapper, desmarque e marque a caixa de seleção da propriedade Background-Color.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-197">Under the .featured .content-wrapper class selector, clear and select the checkbox for the background-color property.</span></span>

![Limpar cor da fundo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

<span data-ttu-id="6a7f9-199">Observe como as visualizações de alteração são instantaneamente na janela Inspetor de Página navegador.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-199">Notice how the change previews instantly in the Page Inspector browser window.</span></span>

<span data-ttu-id="6a7f9-200">Marque a caixa de seleção novamente, clique duas vezes no valor da propriedade e altere-o para `red`.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-200">Select the checkbox again, then double-click the property value and change it to `red`.</span></span> <span data-ttu-id="6a7f9-201">A alteração mostra imediatamente:</span><span class="sxs-lookup"><span data-stu-id="6a7f9-201">The change shows immediately:</span></span>

![Cor de fundo vermelha](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

<span data-ttu-id="6a7f9-203">A janela **estilos** facilita o teste e a visualização das alterações de CSS antes de você confirmar as alterações na própria folha de estilos.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-203">The **Styles** window makes it easy to test and preview CSS changes before you commit the changes to the style sheet itself.</span></span>

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a><span data-ttu-id="6a7f9-204">Sincronização automática de CSS</span><span class="sxs-lookup"><span data-stu-id="6a7f9-204">CSS Auto Sync</span></span>

> [!NOTE]
> <span data-ttu-id="6a7f9-205">Este recurso requer a versão 1,3 de Inspetor de Página.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-205">This feature requires version 1.3 of Page Inspector.</span></span>

<span data-ttu-id="6a7f9-206">O recurso de sincronização automática de CSS permite que você edite um arquivo CSS diretamente e veja as alterações imediatamente no navegador Inspetor de Página.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-206">The CSS Auto-Sync feature allows you to edit a CSS file directly, and see the changes immediately in the Page Inspector browser.</span></span>

<span data-ttu-id="6a7f9-207">Clique em **inspecionar** para colocar Inspetor de Página em modo de inspeção.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-207">Click **Inspect** to put Page Inspector in Inspection Mode.</span></span>

<span data-ttu-id="6a7f9-208">No navegador Inspetor de Página, mova o ponteiro do mouse sobre a seção "Home Page" até que o rótulo **div. Content-wrapper** seja exibido.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-208">In the Page Inspector browser, move the mouse pointer over the "Home Page" section until the **div.content-wrapper** label appears.</span></span> <span data-ttu-id="6a7f9-209">Clique em uma vez para selecionar este elemento.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-209">Click once to select this element.</span></span>

<span data-ttu-id="6a7f9-210">A janela **estilos** mostra todas as regras de CSS para este elemento.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-210">The **Styles** window shows all of the CSS rules for this element.</span></span> <span data-ttu-id="6a7f9-211">Role para baixo até localizar o seletor de classe. em destaque. Content-wrapper.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-211">Scroll down to find the .featured .content-wrapper class selector.</span></span> <span data-ttu-id="6a7f9-212">Clique em ". em destaque. Content-wrapper".</span><span class="sxs-lookup"><span data-stu-id="6a7f9-212">Click on ".featured .content-wrapper".</span></span> <span data-ttu-id="6a7f9-213">Inspetor de Página abre o arquivo CSS que define esse estilo (site. css) e realça o estilo CSS correspondente.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-213">Page Inspector opens the CSS file that defines this style (Site.css) and highlights the corresponding CSS style.</span></span>

![Arquivo CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

<span data-ttu-id="6a7f9-215">Agora, altere o valor de `background-color` para "vermelho".</span><span class="sxs-lookup"><span data-stu-id="6a7f9-215">Now change the value for `background-color` to "red".</span></span> <span data-ttu-id="6a7f9-216">A alteração aparece imediatamente no navegador Inspetor de Página.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-216">The change appears immediately in the Page Inspector browser.</span></span>

![Inspetor de Página navegador](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a><span data-ttu-id="6a7f9-218">Usando o seletor de cores CSS</span><span class="sxs-lookup"><span data-stu-id="6a7f9-218">Using the CSS Color Picker</span></span>

<span data-ttu-id="6a7f9-219">Em seguida, você aprenderá a usar Inspetor de Página para localizar e alterar rapidamente o CSS para texto realçado no aplicativo padrão.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-219">Next, you'll learn how to use Page Inspector to quickly find and change the CSS for highlighted text in the default application.</span></span> <span data-ttu-id="6a7f9-220">Neste exemplo, você decidiu que não gosta do realce azul e deseja alterá-lo para outra cor.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-220">In this example, you've decided that you don't like the blue highlighting and want to change it to another color.</span></span>

<span data-ttu-id="6a7f9-221">Clique no botão **inspecionar** .</span><span class="sxs-lookup"><span data-stu-id="6a7f9-221">Click the **Inspect** button.</span></span>

![Inspecionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

<span data-ttu-id="6a7f9-223">Na janela do navegador Inspetor de Página, mova o ponteiro do mouse sobre o texto realçado "vídeos, tutoriais e amostras" para que o rótulo de "marca" de CSS seja exibido.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-223">In the Page Inspector browser window, move the mouse pointer over the highlighted "videos, tutorials, and samples" text so that the CSS "mark" label appears.</span></span>

![Passando o mouse sobre o elemento Mark](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

<span data-ttu-id="6a7f9-225">Clique no texto para selecioná-lo.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-225">Click the text to select it.</span></span> <span data-ttu-id="6a7f9-226">O seletor de marca CSS correspondente aparece na parte inferior da janela **estilos** .</span><span class="sxs-lookup"><span data-stu-id="6a7f9-226">The corresponding CSS mark selector appears at the bottom of the **Styles** window.</span></span>

![marcar seletor na janela estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

<span data-ttu-id="6a7f9-228">Clique no seletor de marca.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-228">Click the mark selector.</span></span> <span data-ttu-id="6a7f9-229">Isso abre o arquivo *site. css* para o aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-229">This opens the *Site.css* file for the web application.</span></span> <span data-ttu-id="6a7f9-230">Clique na guia site. css e o CSS correspondente para o seletor será realçado:</span><span class="sxs-lookup"><span data-stu-id="6a7f9-230">Click the Site.css tab, and the corresponding CSS for the selector is highlighted:</span></span>

![marcar seletor na folha de estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

<span data-ttu-id="6a7f9-232">Selecione e remova a linha com a propriedade Background-Color.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-232">Select and remove the line with the background-color property.</span></span>

<span data-ttu-id="6a7f9-233">Agora, você usará o novo seletor de cores CSS do Visual Studio 2012 para escolher uma nova cor para a propriedade **Marcar** cor de segundo plano.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-233">You will now use the new Visual Studio 2012 CSS color picker to choose a new color for the **mark** background-color property.</span></span>

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a><span data-ttu-id="6a7f9-234">Usando o seletor de cores CSS do Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="6a7f9-234">Using the Visual Studio 2012 CSS Color Picker</span></span>

<span data-ttu-id="6a7f9-235">O editor de CSS no Visual Studio 2012 tem um seletor de cores que torna mais fácil escolher e inserir cores.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-235">The CSS editor in Visual Studio 2012 has a color picker that makes it easy to choose and insert colors.</span></span> <span data-ttu-id="6a7f9-236">Ele tem uma barra de cores simples e um seletor "pop-down" que oferece controle mais preciso.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-236">It has a simple color bar and a "pop-down" picker that offers finer control.</span></span>

<span data-ttu-id="6a7f9-237">O seletor de cores inclui uma paleta padrão de cores, dá suporte a nomes de cores padrão, códigos de hash, cores RGB, RGBA, HSL e HSLA e mantém uma lista das cores que você usou mais recentemente no documento.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-237">The color picker includes a standard palette of colors, supports standard color names, hash codes, RGB, RGBA, HSL, and HSLA colors, and maintains a list of the colors you've used most recently in the document.</span></span>

<span data-ttu-id="6a7f9-238">Na linha em que a propriedade Background-Color era, digite "BC" e pressione a seta para baixo uma vez.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-238">On the line where the background-color property was, type "bc" and press the down arrow once.</span></span>

<span data-ttu-id="6a7f9-239">Quando você digita o primeiro caractere de cada palavra em uma propriedade separada por hífen, como "background-color", o IntelliSense filtra a lista para que você mostre apenas as propriedades correspondentes:</span><span class="sxs-lookup"><span data-stu-id="6a7f9-239">When you type the first character of each word in a hyphen-separated property like "background-color", IntelliSense filters the list for you to show only the properties that match:</span></span>

![Valores filtrados do IntelliSense](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

<span data-ttu-id="6a7f9-241">Agora, digite dois-pontos.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-241">Now type a colon.</span></span> <span data-ttu-id="6a7f9-242">Quando você fizer isso, o nome completo da propriedade de cor de fundo será inserido.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-242">When you do, the full background-color property name is inserted.</span></span> <span data-ttu-id="6a7f9-243">Digite **#** ou **RGB (** e a barra do seletor de cores será exibida:</span><span class="sxs-lookup"><span data-stu-id="6a7f9-243">Type **#** or **rgb(**, and the color picker bar appears:</span></span>

![A barra do seletor de cores CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

<span data-ttu-id="6a7f9-245">Para ver como a barra do seletor de cores funciona, clique em suas cores com o ponteiro do mouse ou pressione a tecla seta para baixo e use as teclas de seta para a esquerda e para a direita para percorrer as cores.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-245">To see how the color picker bar works, click its colors with the mouse pointer, or press the down arrow key and then use the left and right arrow keys to traverse the colors.</span></span> <span data-ttu-id="6a7f9-246">Quando você visita uma cor, o valor correspondente para a propriedade Background-Color é visualizado:</span><span class="sxs-lookup"><span data-stu-id="6a7f9-246">When you visit a color, the corresponding value for the background-color property is previewed:</span></span>

![plano de fundo-valor da propriedade de cor visualizado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

<span data-ttu-id="6a7f9-248">Neste ponto, você pode pressionar Enter para selecionar o valor e, em seguida, um ponto-e-vírgula (;) para concluir a entrada CSS.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-248">At this point, you could press Enter to select the value and then a semicolon (;) to complete the CSS entry.</span></span> <span data-ttu-id="6a7f9-249">Por enquanto, vá para a próxima seção para que você possa ver como o pop-down seletor de cores funciona.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-249">For now, go on to the next section so that you can see how the color picker pop-down works.</span></span>

#### <a name="using-the-color-picker-pop-down"></a><span data-ttu-id="6a7f9-250">Usando o seletor de cores pop-down</span><span class="sxs-lookup"><span data-stu-id="6a7f9-250">Using the Color Picker Pop-Down</span></span>

<span data-ttu-id="6a7f9-251">Quando a barra de cores não tem a cor exata que você está procurando, você pode usar o pop-down seletor de cores.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-251">When the color bar doesn't have the exact color that you're looking for, you can use the color picker pop-down.</span></span>

<span data-ttu-id="6a7f9-252">Para abri-lo, clique na divisa dupla na extremidade direita da barra de cores ou pressione a seta para baixo uma ou duas vezes no teclado.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-252">To open it, click the double chevron at the right end of the color bar, or press the Down Arrow once or twice on the keyboard.</span></span>

![Pop-down seletor de cores CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

<span data-ttu-id="6a7f9-254">Clique em uma cor na barra vertical à direita.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-254">Click a color from the vertical bar on the right.</span></span> <span data-ttu-id="6a7f9-255">Isso mostra um gradiente para essa cor na janela principal.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-255">This shows a gradient for that color in the main window.</span></span> <span data-ttu-id="6a7f9-256">Escolha uma cor diretamente na barra vertical pressionando ENTER ou clique em qualquer ponto na janela principal para escolher com maior precisão.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-256">Choose a color directly from the vertical bar by pressing Enter, or click any point in the main window to choose with greater precision.</span></span>

<span data-ttu-id="6a7f9-257">Se houver uma cor na tela do computador que você deseja usar (ela não precisa estar dentro da interface do usuário do Visual Studio), você poderá capturar seu valor usando a ferramenta de conta-gotas no canto inferior direito.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-257">If there is a color on your computer screen that you want to use (it doesn't have to be inside the Visual Studio user interface), you can capture its value by using the eyedropper tool on the lower right.</span></span>

<span data-ttu-id="6a7f9-258">Você também pode alterar a opacidade de uma cor movendo o controle deslizante na parte inferior do seletor de cores.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-258">You also can change the opacity of a color by moving the slider at the bottom of the color picker.</span></span> <span data-ttu-id="6a7f9-259">Isso altera os valores de cor para os valores RGBA porque o formato RGBA pode representar a opacidade.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-259">Doing so changes color values to RGBA values because the RGBA format can represent opacity.</span></span>

<span data-ttu-id="6a7f9-260">Depois de escolher uma cor, pressione Enter e, em seguida, digite um ponto e vírgula para concluir a entrada de cor de fundo no arquivo *site. css* .</span><span class="sxs-lookup"><span data-stu-id="6a7f9-260">After you have chosen a color, press Enter, and then type a semicolon to complete the background-color entry in the *Site.css* file.</span></span>

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a><span data-ttu-id="6a7f9-261">A barra de atualização de Inspetor de Página</span><span class="sxs-lookup"><span data-stu-id="6a7f9-261">The Page Inspector Update Bar</span></span>

<span data-ttu-id="6a7f9-262">Inspetor de Página detecta imediatamente a alteração no arquivo *site. css* (ou em qualquer arquivo no aplicativo) e exibe um alerta em uma barra de atualização.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-262">Page Inspector immediately detects the change to the *Site.css* file (or to any file in the application) and displays an alert in an update bar.</span></span>

![Barra de atualização](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

<span data-ttu-id="6a7f9-264">Para salvar todos os arquivos e atualizar o navegador Inspetor de Página, pressione Ctrl + Alt + Enter ou clique na barra de atualização.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-264">To save all your files and refresh the Page Inspector browser, press Ctrl+Alt+Enter or click the update bar.</span></span> <span data-ttu-id="6a7f9-265">A alteração na cor de realce aparece no navegador:</span><span class="sxs-lookup"><span data-stu-id="6a7f9-265">The change in the highlight color appears in the browser:</span></span>

![Cor do realce alterada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a><span data-ttu-id="6a7f9-267">Observe que você atualizou convenientemente o Inspetor de Página navegador diretamente de dentro do ambiente do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-267">Notice that you conveniently refreshed the Page Inspector browser right from within the Visual Studio environment.</span></span> <span data-ttu-id="6a7f9-268">O uso de Inspetor de Página em vez de um navegador externo permite que você permaneça no editor ao desenvolver seus aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="6a7f9-268">Using Page Inspector instead of an external browser lets you stay in the editor when you develop your web applications.</span></span>

## <a name="see-also"></a><span data-ttu-id="6a7f9-269">Consulte Também</span><span class="sxs-lookup"><span data-stu-id="6a7f9-269">See Also</span></span>

<span data-ttu-id="6a7f9-270">[Introdução ao inspetor de página](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (vídeo do Channel 9)</span><span class="sxs-lookup"><span data-stu-id="6a7f9-270">[Introducing Page Inspector](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9 video)</span></span>

<span data-ttu-id="6a7f9-271">[Inspetor de página mensagens de erro](https://go.microsoft.com/?linkid=9813062) (MSDN)</span><span class="sxs-lookup"><span data-stu-id="6a7f9-271">[Page Inspector Error Messages](https://go.microsoft.com/?linkid=9813062) (MSDN)</span></span>
