---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Auto-host ASP.NET Web API 1 (C#)-ASP.NET 4. x
author: MikeWasson
description: O tutorial com código mostra como hospedar uma API Web dentro de um aplicativo de console.
ms.author: riande
ms.date: 01/26/2012
ms.custom: seoapril2019
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: bae1737ba5b16bc67fa0ed0474ff04df0add1b3a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78525084"
---
# <a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="bc9f1-103">Auto-host ASP.NET Web API 1 (C#)</span><span class="sxs-lookup"><span data-stu-id="bc9f1-103">Self-Host ASP.NET Web API 1 (C#)</span></span>

<span data-ttu-id="bc9f1-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bc9f1-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="bc9f1-105">Este tutorial mostra como hospedar uma API Web dentro de um aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-105">This tutorial shows how to host a web API inside a console application.</span></span> <span data-ttu-id="bc9f1-106">ASP.NET Web API não exige o IIS.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-106">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="bc9f1-107">Você pode hospedar automaticamente uma API Web em seu próprio processo de host.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-107">You can self-host a web API in your own host process.</span></span> 
> 
> <span data-ttu-id="bc9f1-108">**Os novos aplicativos devem usar o OWIN para a API Web de hospedagem própria.**</span><span class="sxs-lookup"><span data-stu-id="bc9f1-108">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="bc9f1-109">Confira [usar o OWIN para o autohost ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="bc9f1-109">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bc9f1-110">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="bc9f1-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="bc9f1-111">API Web 1</span><span class="sxs-lookup"><span data-stu-id="bc9f1-111">Web API 1</span></span>
> - <span data-ttu-id="bc9f1-112">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="bc9f1-112">Visual Studio 2012</span></span>

## <a name="create-the-console-application-project"></a><span data-ttu-id="bc9f1-113">Criar o projeto de aplicativo de console</span><span class="sxs-lookup"><span data-stu-id="bc9f1-113">Create the Console Application Project</span></span>

<span data-ttu-id="bc9f1-114">Inicie o Visual Studio e selecione **novo projeto** na página **inicial** .</span><span class="sxs-lookup"><span data-stu-id="bc9f1-114">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="bc9f1-115">Ou, no menu **arquivo** , selecione **novo** e **projeto**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-115">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="bc9f1-116">No painel **modelos** , selecione **modelos instalados** e expanda o **nó C# Visual** .</span><span class="sxs-lookup"><span data-stu-id="bc9f1-116">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="bc9f1-117">Em **Visual C#** , selecione **Windows**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-117">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="bc9f1-118">Na lista de modelos de projeto, selecione **aplicativo de console**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-118">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="bc9f1-119">Nomeie o projeto &quot;SelfHost&quot; e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-119">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="bc9f1-120">Definir a estrutura de destino (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="bc9f1-120">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="bc9f1-121">Se você estiver usando o Visual Studio 2010, altere a estrutura de destino para .NET Framework 4,0.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-121">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="bc9f1-122">(Por padrão, o modelo de projeto destina-se ao [perfil de cliente do .NET Framework](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="bc9f1-122">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="bc9f1-123">Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto e selecione **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-123">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="bc9f1-124">Na lista suspensa **estrutura de destino** , altere a estrutura de destino para .NET Framework 4,0.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-124">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="bc9f1-125">Quando for solicitado a aplicar a alteração, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-125">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="bc9f1-126">Instalar o Gerenciador de pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="bc9f1-126">Install NuGet Package Manager</span></span>

<span data-ttu-id="bc9f1-127">O Gerenciador de pacotes NuGet é a maneira mais fácil de adicionar os assemblies da API Web a um projeto do non-ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-127">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="bc9f1-128">Para verificar se o Gerenciador de pacotes NuGet está instalado, clique no menu **ferramentas** no Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-128">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="bc9f1-129">Se você vir um item de menu chamado **Gerenciador de pacotes NuGet**, você terá o Gerenciador de pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-129">If you see a menu item called **NuGet Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="bc9f1-130">Para instalar o Gerenciador de pacotes NuGet:</span><span class="sxs-lookup"><span data-stu-id="bc9f1-130">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="bc9f1-131">Inicie o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-131">Start Visual Studio.</span></span>
2. <span data-ttu-id="bc9f1-132">No menu **Ferramentas**, selecione **Extensões e atualizações**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-132">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="bc9f1-133">Na caixa de diálogo **extensões e atualizações** , selecione **online**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-133">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="bc9f1-134">Se você não vir o "Gerenciador de pacotes NuGet", digite "Gerenciador de pacotes NuGet" na caixa de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-134">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="bc9f1-135">Selecione o Gerenciador de pacotes NuGet e clique em **baixar**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-135">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="bc9f1-136">Após a conclusão do download, você será solicitado a instalar o.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-136">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="bc9f1-137">Após a conclusão da instalação, talvez seja solicitado que você reinicie o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-137">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="bc9f1-138">Adicionar o pacote NuGet da API Web</span><span class="sxs-lookup"><span data-stu-id="bc9f1-138">Add the Web API NuGet Package</span></span>

<span data-ttu-id="bc9f1-139">Depois que o Gerenciador de pacotes NuGet estiver instalado, adicione o pacote de autohospedagem da API Web ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-139">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="bc9f1-140">No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-140">From the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="bc9f1-141">*Observação*: se você não vir esse item de menu, verifique se o Gerenciador de pacotes NuGet foi instalado corretamente.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-141">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="bc9f1-142">Selecione **gerenciar pacotes NuGet para solução**</span><span class="sxs-lookup"><span data-stu-id="bc9f1-142">Select **Manage NuGet Packages for Solution**</span></span>
3. <span data-ttu-id="bc9f1-143">Na caixa de diálogo **gerenciar pacotes do preciosidade** , selecione **online**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-143">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="bc9f1-144">Na caixa de pesquisa, digite &quot;Microsoft. AspNet. WebApi. SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-144">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="bc9f1-145">Selecione o pacote do ASP.NET Web API Self host e clique em **instalar**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-145">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="bc9f1-146">Após a instalação do pacote, clique em **fechar** para fechar a caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-146">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="bc9f1-147">Certifique-se de instalar o pacote denominado Microsoft. AspNet. WebApi. SelfHost, não AspNetWebApi. SelfHost.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-147">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="bc9f1-148">Criar o modelo e o controlador</span><span class="sxs-lookup"><span data-stu-id="bc9f1-148">Create the Model and Controller</span></span>

<span data-ttu-id="bc9f1-149">Este tutorial usa as mesmas classes de modelo e controlador do tutorial de [introdução](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="bc9f1-149">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="bc9f1-150">Adicione uma classe pública chamada `Product`.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-150">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="bc9f1-151">Adicione uma classe pública chamada `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-151">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="bc9f1-152">Derive essa classe de **System. Web. http. ApiController**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-152">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="bc9f1-153">Para obter mais informações sobre o código nesse controlador, consulte o tutorial de [introdução](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="bc9f1-153">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="bc9f1-154">Esse controlador define três ações GET:</span><span class="sxs-lookup"><span data-stu-id="bc9f1-154">This controller defines three GET actions:</span></span>

| <span data-ttu-id="bc9f1-155">URI</span><span class="sxs-lookup"><span data-stu-id="bc9f1-155">URI</span></span> | <span data-ttu-id="bc9f1-156">DESCRIÇÃO</span><span class="sxs-lookup"><span data-stu-id="bc9f1-156">Description</span></span> |
| --- | --- |
| <span data-ttu-id="bc9f1-157">/api/products</span><span class="sxs-lookup"><span data-stu-id="bc9f1-157">/api/products</span></span> | <span data-ttu-id="bc9f1-158">Obtenha uma lista de todos os produtos.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-158">Get a list of all products.</span></span> |
| <span data-ttu-id="bc9f1-159">*ID* do/API/Products/</span><span class="sxs-lookup"><span data-stu-id="bc9f1-159">/api/products/*id*</span></span> | <span data-ttu-id="bc9f1-160">Obter um produto por ID.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-160">Get a product by ID.</span></span> |
| <span data-ttu-id="bc9f1-161">/API/Products/? categoria =*categoria*</span><span class="sxs-lookup"><span data-stu-id="bc9f1-161">/api/products/?category=*category*</span></span> | <span data-ttu-id="bc9f1-162">Obter uma lista de produtos por categoria.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-162">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="bc9f1-163">Hospedar a API Web</span><span class="sxs-lookup"><span data-stu-id="bc9f1-163">Host the Web API</span></span>

<span data-ttu-id="bc9f1-164">Abra o arquivo Program.cs e adicione as seguintes instruções using:</span><span class="sxs-lookup"><span data-stu-id="bc9f1-164">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="bc9f1-165">Adicione o código a seguir à classe **programa** .</span><span class="sxs-lookup"><span data-stu-id="bc9f1-165">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="bc9f1-166">Adicional Adicionar uma reserva de namespace de URL HTTP</span><span class="sxs-lookup"><span data-stu-id="bc9f1-166">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="bc9f1-167">Este aplicativo escuta `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-167">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="bc9f1-168">Por padrão, a escuta em um endereço HTTP específico requer privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-168">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="bc9f1-169">Ao executar o tutorial, portanto, você pode receber esse erro: "HTTP não pôde registrar a URL http://+:8080/" há duas maneiras de evitar esse erro:</span><span class="sxs-lookup"><span data-stu-id="bc9f1-169">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="bc9f1-170">Execute o Visual Studio com permissões de administrador elevadas ou</span><span class="sxs-lookup"><span data-stu-id="bc9f1-170">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="bc9f1-171">Use o netsh. exe para conceder permissões de conta para reservar a URL.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-171">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="bc9f1-172">Para usar o netsh. exe, abra um prompt de comando com privilégios de administrador e insira o comando a seguir:</span><span class="sxs-lookup"><span data-stu-id="bc9f1-172">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="bc9f1-173">em que *nomedeusuário* é sua conta de usuário.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-173">where *machine\username* is your user account.</span></span>

<span data-ttu-id="bc9f1-174">Quando você terminar a hospedagem interna, certifique-se de excluir a reserva:</span><span class="sxs-lookup"><span data-stu-id="bc9f1-174">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="bc9f1-175">Chamar a API da Web de um aplicativo clienteC#()</span><span class="sxs-lookup"><span data-stu-id="bc9f1-175">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="bc9f1-176">Vamos escrever um aplicativo de console simples que chama a API da Web.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-176">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="bc9f1-177">Adicione um novo projeto de aplicativo de console à solução:</span><span class="sxs-lookup"><span data-stu-id="bc9f1-177">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="bc9f1-178">Em Gerenciador de Soluções, clique com o botão direito do mouse na solução e selecione **Adicionar novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-178">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="bc9f1-179">Crie um novo aplicativo de console chamado &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-179">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="bc9f1-180">Use o Gerenciador de pacotes NuGet para adicionar o pacote ASP.NET Web API Core Libraries:</span><span class="sxs-lookup"><span data-stu-id="bc9f1-180">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="bc9f1-181">No menu ferramentas, selecione **Gerenciador de pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-181">From the Tools menu, select **NuGet Package Manager**.</span></span>
- <span data-ttu-id="bc9f1-182">Selecione **gerenciar pacotes NuGet para solução**</span><span class="sxs-lookup"><span data-stu-id="bc9f1-182">Select **Manage NuGet Packages for Solution**</span></span>
- <span data-ttu-id="bc9f1-183">Na caixa de diálogo **gerenciar pacotes NuGet** , selecione **online**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-183">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="bc9f1-184">Na caixa de pesquisa, digite &quot;Microsoft. AspNet. WebApi. Client&quot;.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-184">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="bc9f1-185">Selecione o pacote Microsoft ASP.NET bibliotecas de cliente da API Web e clique em **instalar**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-185">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="bc9f1-186">Adicione uma referência em ClientApp ao projeto SelfHost:</span><span class="sxs-lookup"><span data-stu-id="bc9f1-186">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="bc9f1-187">Em Gerenciador de Soluções, clique com o botão direito do mouse no projeto ClientApp.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-187">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="bc9f1-188">Selecione **Adicionar Referência**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-188">Select **Add Reference**.</span></span>
- <span data-ttu-id="bc9f1-189">Na caixa de diálogo **Gerenciador de referências** , em **solução**, selecione **projetos**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-189">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="bc9f1-190">Selecione o projeto SelfHost.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-190">Select the SelfHost project.</span></span>
- <span data-ttu-id="bc9f1-191">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-191">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="bc9f1-192">Abra o arquivo cliente/programa. cs.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-192">Open the Client/Program.cs file.</span></span> <span data-ttu-id="bc9f1-193">Adicione a seguinte instrução **using** :</span><span class="sxs-lookup"><span data-stu-id="bc9f1-193">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="bc9f1-194">Adicione uma instância **HttpClient** estática:</span><span class="sxs-lookup"><span data-stu-id="bc9f1-194">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="bc9f1-195">Adicione os seguintes métodos para listar todos os produtos, listar um produto por ID e listar produtos por categoria.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-195">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="bc9f1-196">Cada um desses métodos segue o mesmo padrão:</span><span class="sxs-lookup"><span data-stu-id="bc9f1-196">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="bc9f1-197">Chame **HttpClient. getasync** para enviar uma solicitação get para o URI apropriado.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-197">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="bc9f1-198">Chame **HttpResponseMessage. EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-198">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="bc9f1-199">Esse método gera uma exceção se o status de resposta HTTP é um código de erro.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-199">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="bc9f1-200">Chame **ReadAsAsync&lt;t&gt;** para desserializar um tipo CLR da resposta http.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-200">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="bc9f1-201">Esse método é um método de extensão, definido em **System .net. http. HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-201">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="bc9f1-202">Os métodos **getasync** e **ReadAsAsync** são assíncronos.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-202">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="bc9f1-203">Elas retornam objetos de **tarefa** que representam a operação assíncrona.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-203">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="bc9f1-204">Obter a propriedade **Result** bloqueia o thread até que a operação seja concluída.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-204">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="bc9f1-205">Para obter mais informações sobre como usar HttpClient, incluindo como fazer chamadas sem bloqueio, consulte [chamando uma API da Web de um cliente .net](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="bc9f1-205">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="bc9f1-206">Antes de chamar esses métodos, defina a propriedade BaseAddress na instância HttpClient como "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="bc9f1-206">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="bc9f1-207">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="bc9f1-207">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="bc9f1-208">Isso deve gerar o seguinte.</span><span class="sxs-lookup"><span data-stu-id="bc9f1-208">This should output the following.</span></span> <span data-ttu-id="bc9f1-209">(Lembre-se de executar o aplicativo SelfHost primeiro.)</span><span class="sxs-lookup"><span data-stu-id="bc9f1-209">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
