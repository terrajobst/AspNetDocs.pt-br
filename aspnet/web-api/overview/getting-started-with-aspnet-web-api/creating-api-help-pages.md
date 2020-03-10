---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Criando páginas de ajuda para o ASP.NET Web API-ASP.NET 4. x
author: MikeWasson
description: Este tutorial com código mostra como criar páginas de ajuda para ASP.NET Web API no ASP.NET 4. x.
ms.author: riande
ms.date: 04/01/2013
ms.custom: seoapril2019
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8308dab8bd66aa8f5a3c5fb4133fc7a3df78f671
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556871"
---
# <a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="fdc29-103">Criando páginas de ajuda para ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="fdc29-103">Creating Help Pages for ASP.NET Web API</span></span>

<span data-ttu-id="fdc29-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fdc29-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="fdc29-105">Este tutorial com código mostra como criar páginas de ajuda para ASP.NET Web API no ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="fdc29-105">This tutorial with code shows how to create help pages for ASP.NET Web API in ASP.NET 4.x.</span></span>

<span data-ttu-id="fdc29-106">Quando você cria uma API da Web, geralmente é útil criar uma página de ajuda para que outros desenvolvedores saibam como chamar sua API.</span><span class="sxs-lookup"><span data-stu-id="fdc29-106">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="fdc29-107">Você pode criar toda a documentação manualmente, mas é melhor gerar o máximo possível.</span><span class="sxs-lookup"><span data-stu-id="fdc29-107">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span> <span data-ttu-id="fdc29-108">Para facilitar essa tarefa, ASP.NET Web API fornece uma biblioteca para gerar automaticamente páginas de ajuda em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="fdc29-108">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="fdc29-109">Criando páginas de ajuda da API</span><span class="sxs-lookup"><span data-stu-id="fdc29-109">Creating API Help Pages</span></span>

<span data-ttu-id="fdc29-110">Instale a [atualização do ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="fdc29-110">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="fdc29-111">Essa atualização integra as páginas de ajuda no modelo de projeto de API Web.</span><span class="sxs-lookup"><span data-stu-id="fdc29-111">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="fdc29-112">Em seguida, crie um novo projeto do ASP.NET MVC 4 e selecione o modelo de projeto da API Web.</span><span class="sxs-lookup"><span data-stu-id="fdc29-112">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="fdc29-113">O modelo de projeto cria um controlador de API de exemplo chamado `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="fdc29-113">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="fdc29-114">O modelo também cria as páginas de ajuda da API.</span><span class="sxs-lookup"><span data-stu-id="fdc29-114">The template also creates the API help pages.</span></span> <span data-ttu-id="fdc29-115">Todos os arquivos de código da página de ajuda são colocados na pasta áreas do projeto.</span><span class="sxs-lookup"><span data-stu-id="fdc29-115">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="fdc29-116">Quando você executa o aplicativo, o home page contém um link para a página de ajuda da API.</span><span class="sxs-lookup"><span data-stu-id="fdc29-116">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="fdc29-117">No home page, o caminho relativo é/help.</span><span class="sxs-lookup"><span data-stu-id="fdc29-117">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="fdc29-118">Esse link leva você a uma página de resumo da API.</span><span class="sxs-lookup"><span data-stu-id="fdc29-118">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="fdc29-119">A exibição MVC desta página é definida em areas/HelpPage/views/help/index. cshtml.</span><span class="sxs-lookup"><span data-stu-id="fdc29-119">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="fdc29-120">Você pode editar essa página para modificar o layout, a introdução, o título, os estilos e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="fdc29-120">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="fdc29-121">A parte principal da página é uma tabela de APIs, agrupada por controlador.</span><span class="sxs-lookup"><span data-stu-id="fdc29-121">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="fdc29-122">As entradas de tabela são geradas dinamicamente, usando a interface **IApiExplorer** .</span><span class="sxs-lookup"><span data-stu-id="fdc29-122">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="fdc29-123">(Falarei mais sobre essa interface mais tarde.) Se você adicionar um novo controlador de API, a tabela será atualizada automaticamente em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="fdc29-123">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="fdc29-124">A coluna "API" lista o método HTTP e o URI relativo.</span><span class="sxs-lookup"><span data-stu-id="fdc29-124">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="fdc29-125">A coluna "Descrição" contém a documentação para cada API.</span><span class="sxs-lookup"><span data-stu-id="fdc29-125">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="fdc29-126">Inicialmente, a documentação é apenas texto de espaço reservado.</span><span class="sxs-lookup"><span data-stu-id="fdc29-126">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="fdc29-127">Na próxima seção, mostrarei como adicionar documentação de comentários XML.</span><span class="sxs-lookup"><span data-stu-id="fdc29-127">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="fdc29-128">Cada API tem um link para uma página com informações mais detalhadas, incluindo corpos de solicitação e resposta de exemplo.</span><span class="sxs-lookup"><span data-stu-id="fdc29-128">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="fdc29-129">Adicionando páginas de ajuda a um projeto existente</span><span class="sxs-lookup"><span data-stu-id="fdc29-129">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="fdc29-130">Você pode adicionar páginas de ajuda a um projeto de API Web existente usando o Gerenciador de pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="fdc29-130">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="fdc29-131">Essa opção é útil que você inicia em um modelo de projeto diferente do modelo "API Web".</span><span class="sxs-lookup"><span data-stu-id="fdc29-131">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="fdc29-132">No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="fdc29-132">From the **Tools** menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="fdc29-133">Na janela do [console do Gerenciador de pacotes](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) , digite um dos seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="fdc29-133">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="fdc29-134">Para um **C#** aplicativo: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="fdc29-134">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="fdc29-135">Para um aplicativo **Visual Basic** : `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="fdc29-135">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="fdc29-136">Há dois pacotes, um para C# e um para Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="fdc29-136">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="fdc29-137">Certifique-se de usar aquela que corresponda ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="fdc29-137">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="fdc29-138">Esse comando instala os assemblies necessários e adiciona as exibições do MVC para as páginas de ajuda (localizadas na pasta areas/HelpPage).</span><span class="sxs-lookup"><span data-stu-id="fdc29-138">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="fdc29-139">Você precisará adicionar manualmente um link à página de ajuda.</span><span class="sxs-lookup"><span data-stu-id="fdc29-139">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="fdc29-140">O URI é/help.</span><span class="sxs-lookup"><span data-stu-id="fdc29-140">The URI is /Help.</span></span> <span data-ttu-id="fdc29-141">Para criar um link em uma exibição do Razor, adicione o seguinte:</span><span class="sxs-lookup"><span data-stu-id="fdc29-141">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="fdc29-142">Além disso, certifique-se de registrar áreas.</span><span class="sxs-lookup"><span data-stu-id="fdc29-142">Also, make sure to register areas.</span></span> <span data-ttu-id="fdc29-143">No arquivo global. asax, adicione o seguinte código ao **aplicativo\_** método de início, se ainda não estiver lá:</span><span class="sxs-lookup"><span data-stu-id="fdc29-143">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="fdc29-144">Adicionando documentação da API</span><span class="sxs-lookup"><span data-stu-id="fdc29-144">Adding API Documentation</span></span>

<span data-ttu-id="fdc29-145">Por padrão, as páginas de ajuda têm cadeias de caracteres de espaço reservado para documentação.</span><span class="sxs-lookup"><span data-stu-id="fdc29-145">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="fdc29-146">Você pode usar [comentários de documentação XML](https://msdn.microsoft.com/library/b2s063f7.aspx) para criar a documentação.</span><span class="sxs-lookup"><span data-stu-id="fdc29-146">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="fdc29-147">Para habilitar esse recurso, abra o arquivo areas/HelpPage/app\_Start/HelpPageConfig. cs e remova a marca de comentário da seguinte linha:</span><span class="sxs-lookup"><span data-stu-id="fdc29-147">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="fdc29-148">Agora, habilite a documentação XML.</span><span class="sxs-lookup"><span data-stu-id="fdc29-148">Now enable XML documentation.</span></span> <span data-ttu-id="fdc29-149">Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto e selecione **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="fdc29-149">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="fdc29-150">Selecione a página de **compilação** .</span><span class="sxs-lookup"><span data-stu-id="fdc29-150">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="fdc29-151">Em **saída**, verifique o **arquivo de documentação XML**.</span><span class="sxs-lookup"><span data-stu-id="fdc29-151">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="fdc29-152">Na caixa de edição, digite "app\_data/XmlDocument. xml".</span><span class="sxs-lookup"><span data-stu-id="fdc29-152">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="fdc29-153">Em seguida, abra o código para o controlador de API `ValuesController`, que é definido em/Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="fdc29-153">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesController.cs.</span></span> <span data-ttu-id="fdc29-154">Adicione alguns comentários de documentação aos métodos do controlador.</span><span class="sxs-lookup"><span data-stu-id="fdc29-154">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="fdc29-155">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="fdc29-155">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="fdc29-156">Dica: se você posicionar o cursor na linha acima do método e digitar três barras invertidas, o Visual Studio inserirá automaticamente os elementos XML.</span><span class="sxs-lookup"><span data-stu-id="fdc29-156">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="fdc29-157">Em seguida, você pode preencher os espaços em branco.</span><span class="sxs-lookup"><span data-stu-id="fdc29-157">Then you can fill in the blanks.</span></span>

<span data-ttu-id="fdc29-158">Agora, compile e execute o aplicativo novamente e navegue até as páginas de ajuda.</span><span class="sxs-lookup"><span data-stu-id="fdc29-158">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="fdc29-159">As cadeias de caracteres de documentação devem aparecer na tabela de API.</span><span class="sxs-lookup"><span data-stu-id="fdc29-159">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="fdc29-160">A página de ajuda lê as cadeias de caracteres do arquivo XML em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="fdc29-160">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="fdc29-161">(Quando você implantar o aplicativo, certifique-se de implantar o arquivo XML.)</span><span class="sxs-lookup"><span data-stu-id="fdc29-161">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="fdc29-162">Nos bastidores</span><span class="sxs-lookup"><span data-stu-id="fdc29-162">Under the Hood</span></span>

<span data-ttu-id="fdc29-163">As páginas de ajuda são criadas sobre a classe **ApiExplorer** , que faz parte da estrutura da API Web.</span><span class="sxs-lookup"><span data-stu-id="fdc29-163">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="fdc29-164">A classe **ApiExplorer** fornece o material bruto para criar uma página de ajuda.</span><span class="sxs-lookup"><span data-stu-id="fdc29-164">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="fdc29-165">Para cada API, **ApiExplorer** contém um **ApiDescription** que descreve a API.</span><span class="sxs-lookup"><span data-stu-id="fdc29-165">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="fdc29-166">Para essa finalidade, uma "API" é definida como a combinação do método HTTP e do URI relativo.</span><span class="sxs-lookup"><span data-stu-id="fdc29-166">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="fdc29-167">Por exemplo, aqui estão algumas APIs distintas:</span><span class="sxs-lookup"><span data-stu-id="fdc29-167">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="fdc29-168">OBTER/api/Products</span><span class="sxs-lookup"><span data-stu-id="fdc29-168">GET /api/Products</span></span>
- <span data-ttu-id="fdc29-169">OBTER/api/Products/{id}</span><span class="sxs-lookup"><span data-stu-id="fdc29-169">GET /api/Products/{id}</span></span>
- <span data-ttu-id="fdc29-170">POSTAR/api/Products</span><span class="sxs-lookup"><span data-stu-id="fdc29-170">POST /api/Products</span></span>

<span data-ttu-id="fdc29-171">Se uma ação do controlador oferecer suporte a vários métodos HTTP, o **ApiExplorer** tratará cada método como uma API distinta.</span><span class="sxs-lookup"><span data-stu-id="fdc29-171">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="fdc29-172">Para ocultar uma API do **ApiExplorer**, adicione o atributo **ApiExplorerSettings** à ação e defina *IgnoreApi* como true.</span><span class="sxs-lookup"><span data-stu-id="fdc29-172">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="fdc29-173">Você também pode adicionar esse atributo ao controlador para excluir o controlador inteiro.</span><span class="sxs-lookup"><span data-stu-id="fdc29-173">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="fdc29-174">A classe ApiExplorer Obtém as cadeias de caracteres de documentação da interface **IDocumentationProvider** .</span><span class="sxs-lookup"><span data-stu-id="fdc29-174">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="fdc29-175">Como vimos anteriormente, a biblioteca de páginas de ajuda fornece um **IDocumentationProvider** que obtém a documentação de cadeias de caracteres de documentação XML.</span><span class="sxs-lookup"><span data-stu-id="fdc29-175">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="fdc29-176">O código está localizado em/Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="fdc29-176">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="fdc29-177">Você pode obter a documentação de outra fonte escrevendo seu próprio **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="fdc29-177">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="fdc29-178">Para conectá-lo, chame o método de extensão **Setdocumentaprovider** , definido em **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="fdc29-178">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="fdc29-179">O **ApiExplorer** chama automaticamente a interface **IDocumentationProvider** para obter cadeias de caracteres de documentação para cada API.</span><span class="sxs-lookup"><span data-stu-id="fdc29-179">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="fdc29-180">Ele os armazena na propriedade de **documentação** dos objetos **ApiDescription** e **ApiParameterDescription** .</span><span class="sxs-lookup"><span data-stu-id="fdc29-180">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fdc29-181">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="fdc29-181">Next Steps</span></span>

<span data-ttu-id="fdc29-182">Você não está limitado às páginas de ajuda mostradas aqui.</span><span class="sxs-lookup"><span data-stu-id="fdc29-182">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="fdc29-183">Na verdade, o **ApiExplorer** não está limitado à criação de páginas de ajuda.</span><span class="sxs-lookup"><span data-stu-id="fdc29-183">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="fdc29-184">A Yao huangy escreveu algumas boas postagens no blog para que você tenha pensado na caixa:</span><span class="sxs-lookup"><span data-stu-id="fdc29-184">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="fdc29-185">Adicionando um cliente de teste simples à página ASP.NET Web API ajuda</span><span class="sxs-lookup"><span data-stu-id="fdc29-185">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="fdc29-186">Fazendo ASP.NET Web API página de ajuda para trabalhar em serviços hospedados internamente</span><span class="sxs-lookup"><span data-stu-id="fdc29-186">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="fdc29-187">Geração de tempo de design da página de ajuda (ou cliente) para ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="fdc29-187">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="fdc29-188">Personalizações de página de ajuda avançada</span><span class="sxs-lookup"><span data-stu-id="fdc29-188">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
