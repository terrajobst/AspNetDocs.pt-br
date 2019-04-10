---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Criando páginas de ajuda para API Web ASP.NET - ASP.NET 4.x
author: MikeWasson
description: Este tutorial com o código mostra como criar páginas de ajuda para API Web do ASP.NET no ASP.NET 4. x.
ms.author: riande
ms.date: 04/01/2013
ms.custom: seoapril2019
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: e3f6a9b8a6835b034a075d580cd9a33136969990
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59395002"
---
# <a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="f5381-103">Criando páginas de ajuda para API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f5381-103">Creating Help Pages for ASP.NET Web API</span></span>

<span data-ttu-id="f5381-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f5381-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f5381-105">Este tutorial com o código mostra como criar páginas de ajuda para API Web do ASP.NET no ASP.NET 4. x.</span><span class="sxs-lookup"><span data-stu-id="f5381-105">This tutorial with code shows how to create help pages for ASP.NET Web API in ASP.NET 4.x.</span></span>

<span data-ttu-id="f5381-106">Quando você cria uma API da web, muitas vezes é útil criar uma página de Ajuda, para que outros desenvolvedores saibam como chamar sua API.</span><span class="sxs-lookup"><span data-stu-id="f5381-106">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="f5381-107">Você pode criar toda a documentação manualmente, mas é melhor usado para gerar automaticamente tanto quanto possível.</span><span class="sxs-lookup"><span data-stu-id="f5381-107">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span> <span data-ttu-id="f5381-108">Para facilitar essa tarefa, a API Web do ASP.NET fornece uma biblioteca em tempo de execução para a geração automática de páginas de Ajuda.</span><span class="sxs-lookup"><span data-stu-id="f5381-108">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="f5381-109">Criando páginas de Ajuda da API</span><span class="sxs-lookup"><span data-stu-id="f5381-109">Creating API Help Pages</span></span>

<span data-ttu-id="f5381-110">Instale [ASP.NET e Web Tools 2012.2 atualização](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="f5381-110">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="f5381-111">Essa atualização integra-se a páginas de ajuda para o modelo de projeto de API da Web.</span><span class="sxs-lookup"><span data-stu-id="f5381-111">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="f5381-112">Em seguida, crie um novo projeto ASP.NET MVC 4 e selecione o modelo de projeto de API da Web.</span><span class="sxs-lookup"><span data-stu-id="f5381-112">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="f5381-113">O modelo de projeto cria um controlador de API de exemplo chamado `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="f5381-113">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="f5381-114">O modelo também cria as páginas de Ajuda da API.</span><span class="sxs-lookup"><span data-stu-id="f5381-114">The template also creates the API help pages.</span></span> <span data-ttu-id="f5381-115">Todos os arquivos de código para a página de ajuda são colocados na pasta áreas do projeto.</span><span class="sxs-lookup"><span data-stu-id="f5381-115">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="f5381-116">Quando você executa o aplicativo, a página inicial contém um link para a página de Ajuda da API.</span><span class="sxs-lookup"><span data-stu-id="f5381-116">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="f5381-117">Na home page, o caminho relativo é /Help.</span><span class="sxs-lookup"><span data-stu-id="f5381-117">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="f5381-118">Este link leva você para uma página de resumo da API.</span><span class="sxs-lookup"><span data-stu-id="f5381-118">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="f5381-119">A exibição do MVC para essa página é definida em Areas/HelpPage/Views/Help/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="f5381-119">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="f5381-120">Você pode editar esta página para modificar o layout, introdução, título, estilos e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="f5381-120">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="f5381-121">A parte principal da página é uma tabela de APIs, agrupadas pelo controlador.</span><span class="sxs-lookup"><span data-stu-id="f5381-121">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="f5381-122">As entradas da tabela são geradas dinamicamente, usando o **IApiExplorer** interface.</span><span class="sxs-lookup"><span data-stu-id="f5381-122">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="f5381-123">(Falarei mais sobre essa interface posteriormente.) Se você adicionar um novo controlador de API, a tabela é automaticamente atualizada em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="f5381-123">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="f5381-124">A coluna de "API" lista o método HTTP e o URI relativo.</span><span class="sxs-lookup"><span data-stu-id="f5381-124">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="f5381-125">A coluna "Descrição" contém a documentação para cada API.</span><span class="sxs-lookup"><span data-stu-id="f5381-125">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="f5381-126">Inicialmente, a documentação é apenas o texto de espaço reservado.</span><span class="sxs-lookup"><span data-stu-id="f5381-126">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="f5381-127">Na próxima seção, mostrarei como adicionar a documentação de comentários XML.</span><span class="sxs-lookup"><span data-stu-id="f5381-127">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="f5381-128">Cada API tem um link para uma página com informações mais detalhadas, incluindo os corpos de solicitação e resposta de exemplo.</span><span class="sxs-lookup"><span data-stu-id="f5381-128">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="f5381-129">Adicionar páginas de ajuda para um projeto existente</span><span class="sxs-lookup"><span data-stu-id="f5381-129">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="f5381-130">Você pode adicionar páginas de ajuda para um projeto de API da Web existente usando o Gerenciador de pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="f5381-130">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="f5381-131">Essa opção é útil que começar a partir de um modelo de projeto diferente que o modelo de "API Web".</span><span class="sxs-lookup"><span data-stu-id="f5381-131">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="f5381-132">Dos **ferramentas** menu, selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="f5381-132">From the **Tools** menu, select **NuGet Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="f5381-133">No [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) janela, digite um dos seguintes comandos:</span><span class="sxs-lookup"><span data-stu-id="f5381-133">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="f5381-134">Para um **c#** aplicativo:</span><span class="sxs-lookup"><span data-stu-id="f5381-134">For a **C#** application:</span></span> `Install-Package Microsoft.AspNet.WebApi.HelpPage`

<span data-ttu-id="f5381-135">Para um **Visual Basic** aplicativo:</span><span class="sxs-lookup"><span data-stu-id="f5381-135">For a **Visual Basic** application:</span></span> `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

<span data-ttu-id="f5381-136">Há dois pacotes, um para c# e outro para o Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="f5381-136">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="f5381-137">Certifique-se de usar aquele que corresponde ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="f5381-137">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="f5381-138">Esse comando instala os assemblies necessários e adiciona as exibições do MVC para as páginas de Ajuda (localizadas na pasta áreas/HelpPage).</span><span class="sxs-lookup"><span data-stu-id="f5381-138">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="f5381-139">Você precisará adicionar manualmente um link para a página de Ajuda.</span><span class="sxs-lookup"><span data-stu-id="f5381-139">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="f5381-140">O URI é /Help.</span><span class="sxs-lookup"><span data-stu-id="f5381-140">The URI is /Help.</span></span> <span data-ttu-id="f5381-141">Para criar um link em um modo de exibição do razor, adicione o seguinte:</span><span class="sxs-lookup"><span data-stu-id="f5381-141">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="f5381-142">Além disso, certifique-se registrar áreas.</span><span class="sxs-lookup"><span data-stu-id="f5381-142">Also, make sure to register areas.</span></span> <span data-ttu-id="f5381-143">No arquivo global. asax, adicione o seguinte código para o **Application\_iniciar** método, se ainda não estiver lá:</span><span class="sxs-lookup"><span data-stu-id="f5381-143">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="f5381-144">Adicionando documentação da API</span><span class="sxs-lookup"><span data-stu-id="f5381-144">Adding API Documentation</span></span>

<span data-ttu-id="f5381-145">Por padrão, a Ajuda páginas têm cadeias de caracteres de espaço reservado para a documentação.</span><span class="sxs-lookup"><span data-stu-id="f5381-145">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="f5381-146">Você pode usar [comentários da documentação XML](https://msdn.microsoft.com/library/b2s063f7.aspx) para criar a documentação.</span><span class="sxs-lookup"><span data-stu-id="f5381-146">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="f5381-147">Para habilitar esse recurso, abra o arquivo HelpPage/aplicativo/áreas\_Start/HelpPageConfig.cs e remova a seguinte linha:</span><span class="sxs-lookup"><span data-stu-id="f5381-147">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="f5381-148">Agora habilite a documentação XML.</span><span class="sxs-lookup"><span data-stu-id="f5381-148">Now enable XML documentation.</span></span> <span data-ttu-id="f5381-149">No Gerenciador de soluções, clique com botão direito no projeto e selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="f5381-149">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="f5381-150">Selecione o **Build** página.</span><span class="sxs-lookup"><span data-stu-id="f5381-150">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="f5381-151">Sob **saída**, verifique **arquivo de documentação XML**.</span><span class="sxs-lookup"><span data-stu-id="f5381-151">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="f5381-152">Na caixa de edição, digite "aplicativo\_Data/XmlDocument.xml".</span><span class="sxs-lookup"><span data-stu-id="f5381-152">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="f5381-153">Em seguida, abra o código para o `ValuesController` controlador da API, que é definido no /Controllers/ValuesController.cs.</span><span class="sxs-lookup"><span data-stu-id="f5381-153">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesController.cs.</span></span> <span data-ttu-id="f5381-154">Adicione alguns comentários de documentação para os métodos do controlador.</span><span class="sxs-lookup"><span data-stu-id="f5381-154">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="f5381-155">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="f5381-155">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="f5381-156">Dica: Se você posiciona o cursor na linha acima do método e digite as três barras "/", o Visual Studio insere automaticamente os elementos XML.</span><span class="sxs-lookup"><span data-stu-id="f5381-156">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="f5381-157">Em seguida, você pode preencher os espaços em branco.</span><span class="sxs-lookup"><span data-stu-id="f5381-157">Then you can fill in the blanks.</span></span>


<span data-ttu-id="f5381-158">Agora, criar e executar o aplicativo novamente e navegue até as páginas de Ajuda.</span><span class="sxs-lookup"><span data-stu-id="f5381-158">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="f5381-159">As cadeias de caracteres de documentação devem aparecer na tabela de API.</span><span class="sxs-lookup"><span data-stu-id="f5381-159">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="f5381-160">A página de Ajuda lê as cadeias de caracteres do arquivo XML em tempo de execução.</span><span class="sxs-lookup"><span data-stu-id="f5381-160">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="f5381-161">(Quando você implanta o aplicativo, verifique se implantar o arquivo XML.)</span><span class="sxs-lookup"><span data-stu-id="f5381-161">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="f5381-162">Nos bastidores</span><span class="sxs-lookup"><span data-stu-id="f5381-162">Under the Hood</span></span>

<span data-ttu-id="f5381-163">As páginas de ajuda são criadas sobre o **ApiExplorer** classe, que é parte da estrutura da API Web.</span><span class="sxs-lookup"><span data-stu-id="f5381-163">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="f5381-164">O **ApiExplorer** classe fornece matéria-prima para a criação de uma página de Ajuda.</span><span class="sxs-lookup"><span data-stu-id="f5381-164">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="f5381-165">Para cada API **ApiExplorer** contém uma **ApiDescription** que descreve a API.</span><span class="sxs-lookup"><span data-stu-id="f5381-165">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="f5381-166">Para essa finalidade, "API" é definida como a combinação de URI relativo e o método HTTP.</span><span class="sxs-lookup"><span data-stu-id="f5381-166">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="f5381-167">Por exemplo, aqui estão algumas APIs distintas:</span><span class="sxs-lookup"><span data-stu-id="f5381-167">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="f5381-168">OBTER /api/Products</span><span class="sxs-lookup"><span data-stu-id="f5381-168">GET /api/Products</span></span>
- <span data-ttu-id="f5381-169">OBTER/API/produtos / {id}</span><span class="sxs-lookup"><span data-stu-id="f5381-169">GET /api/Products/{id}</span></span>
- <span data-ttu-id="f5381-170">POSTAR/api/produtos</span><span class="sxs-lookup"><span data-stu-id="f5381-170">POST /api/Products</span></span>

<span data-ttu-id="f5381-171">Se uma ação do controlador dá suporte a vários métodos HTTP, o **ApiExplorer** trata cada método como uma API distinta.</span><span class="sxs-lookup"><span data-stu-id="f5381-171">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="f5381-172">Para ocultar uma API do **ApiExplorer**, adicione o **ApiExplorerSettings** atributo para a ação e o conjunto *IgnoreApi* como true.</span><span class="sxs-lookup"><span data-stu-id="f5381-172">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="f5381-173">Você também pode adicionar esse atributo para o controlador, para excluir o controlador inteiro.</span><span class="sxs-lookup"><span data-stu-id="f5381-173">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="f5381-174">A classe ApiExplorer obtém cadeias de caracteres de documentação do **IDocumentationProvider** interface.</span><span class="sxs-lookup"><span data-stu-id="f5381-174">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="f5381-175">Como você viu anteriormente, a biblioteca de páginas de Ajuda fornece um **IDocumentationProvider** que obtém a documentação de cadeias de caracteres de documentação XML.</span><span class="sxs-lookup"><span data-stu-id="f5381-175">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="f5381-176">O código está localizado em /Areas/HelpPage/XmlDocumentationProvider.cs.</span><span class="sxs-lookup"><span data-stu-id="f5381-176">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="f5381-177">Você pode obter documentação de outra origem, escrevendo seus próprios **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="f5381-177">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="f5381-178">Para fazer a conexão, chame o **SetDocumentationProvider** definido no método de extensão, **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="f5381-178">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="f5381-179">**ApiExplorer** chama automaticamente o **IDocumentationProvider** a interface para obter cadeias de caracteres de documentação para cada API.</span><span class="sxs-lookup"><span data-stu-id="f5381-179">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="f5381-180">Ele armazena na **documentação** propriedade da **ApiDescription** e **ApiParameterDescription** objetos.</span><span class="sxs-lookup"><span data-stu-id="f5381-180">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f5381-181">Próximas etapas</span><span class="sxs-lookup"><span data-stu-id="f5381-181">Next Steps</span></span>

<span data-ttu-id="f5381-182">Você não fica limitado às páginas de ajuda mostradas aqui.</span><span class="sxs-lookup"><span data-stu-id="f5381-182">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="f5381-183">Na verdade, **ApiExplorer** não está limitado a criar páginas de Ajuda.</span><span class="sxs-lookup"><span data-stu-id="f5381-183">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="f5381-184">Yao Huang Lin escreveu que postagens no blog alguns ótimo para ajudá-lo a pensar fora da caixa:</span><span class="sxs-lookup"><span data-stu-id="f5381-184">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="f5381-185">Adicionando um cliente de teste simples para a página de Ajuda do ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="f5381-185">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="f5381-186">Tornando a página de Ajuda do ASP.NET Web API trabalhar em serviços de hospedagem interna</span><span class="sxs-lookup"><span data-stu-id="f5381-186">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="f5381-187">Geração de tempo de design da Ajuda page (ou cliente) para API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f5381-187">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="f5381-188">Personalizações avançadas da página de ajuda</span><span class="sxs-lookup"><span data-stu-id="f5381-188">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
