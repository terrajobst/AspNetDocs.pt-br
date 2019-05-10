---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Hospedar internamente o ASP.NET Web API 1 (C#)-ASP.NET 4.x
author: MikeWasson
description: Tutorial com o código mostra como hospedar uma API da web dentro de um aplicativo de console.
ms.author: riande
ms.date: 01/26/2012
ms.custom: seoapril2019
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: bae1737ba5b16bc67fa0ed0474ff04df0add1b3a
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65134768"
---
# <a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="2bd9c-103">Hospedar internamente o ASP.NET Web API 1 (c#)</span><span class="sxs-lookup"><span data-stu-id="2bd9c-103">Self-Host ASP.NET Web API 1 (C#)</span></span>

<span data-ttu-id="2bd9c-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2bd9c-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="2bd9c-105">Este tutorial mostra como hospedar uma API da web dentro de um aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-105">This tutorial shows how to host a web API inside a console application.</span></span> <span data-ttu-id="2bd9c-106">API Web ASP.NET não requer o IIS.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-106">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="2bd9c-107">Você pode hospedar internamente uma API da web em seu próprio processo de host.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-107">You can self-host a web API in your own host process.</span></span> 
> 
> <span data-ttu-id="2bd9c-108">**Novos aplicativos devem usar o OWIN para auto-hospedar a API da Web.**</span><span class="sxs-lookup"><span data-stu-id="2bd9c-108">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="2bd9c-109">Ver [usar o OWIN para auto-hospedar a API Web ASP.NET 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="2bd9c-109">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="2bd9c-110">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="2bd9c-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="2bd9c-111">API Web 1</span><span class="sxs-lookup"><span data-stu-id="2bd9c-111">Web API 1</span></span>
> - <span data-ttu-id="2bd9c-112">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="2bd9c-112">Visual Studio 2012</span></span>

## <a name="create-the-console-application-project"></a><span data-ttu-id="2bd9c-113">Criar o projeto de aplicativo de Console</span><span class="sxs-lookup"><span data-stu-id="2bd9c-113">Create the Console Application Project</span></span>

<span data-ttu-id="2bd9c-114">Inicie o Visual Studio e selecione **Novo projeto** na página **Iniciar**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-114">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="2bd9c-115">Ou, no menu **Arquivo**, selecione **Novo** e, em seguida, **Projeto**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-115">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="2bd9c-116">No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-116">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="2bd9c-117">Sob **Visual c#**, selecione **Windows**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-117">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="2bd9c-118">Na lista de modelos de projeto, selecione **aplicativo de Console**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-118">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="2bd9c-119">Nomeie o projeto &quot;SelfHost&quot; e clique em **Okey**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-119">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="2bd9c-120">Defina a estrutura de destino (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="2bd9c-120">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="2bd9c-121">Se você estiver usando o Visual Studio 2010, altere a estrutura de destino para o .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-121">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="2bd9c-122">(Por padrão, os destinos de modelo de projeto do [perfil do .net Framework Client](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="2bd9c-122">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="2bd9c-123">No Gerenciador de soluções, clique com botão direito no projeto e selecione **propriedades**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-123">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="2bd9c-124">No **estrutura de destino** lista suspensa lista, altere a estrutura de destino para o .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-124">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="2bd9c-125">Quando solicitado para aplicar a alteração, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-125">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="2bd9c-126">Instalar o Gerenciador de pacotes NuGet</span><span class="sxs-lookup"><span data-stu-id="2bd9c-126">Install NuGet Package Manager</span></span>

<span data-ttu-id="2bd9c-127">O Gerenciador de pacotes do NuGet é a maneira mais fácil para adicionar os assemblies de API da Web a um projeto não seja ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-127">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="2bd9c-128">Para verificar se o Gerenciador de pacotes do NuGet está instalado, clique o **ferramentas** menu do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-128">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="2bd9c-129">Se você vir um item de menu chamado **Gerenciador de pacotes NuGet**, em seguida, você tem o Gerenciador de pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-129">If you see a menu item called **NuGet Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="2bd9c-130">Para instalar o Gerenciador de pacotes NuGet:</span><span class="sxs-lookup"><span data-stu-id="2bd9c-130">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="2bd9c-131">Inicie o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-131">Start Visual Studio.</span></span>
2. <span data-ttu-id="2bd9c-132">Dos **ferramentas** menu, selecione **extensões e atualizações**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-132">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="2bd9c-133">No **extensões e atualizações** caixa de diálogo, selecione **Online**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-133">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="2bd9c-134">Se você não vir "Gerenciador de pacotes do NuGet", digite "Gerenciador de pacotes do nuget" na caixa de pesquisa.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-134">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="2bd9c-135">Selecione o Gerenciador de pacotes do NuGet e clique em **baixar**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-135">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="2bd9c-136">Após a conclusão do download, você será solicitado a instalar.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-136">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="2bd9c-137">Depois que a instalação for concluída, você talvez seja solicitado para reiniciar o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-137">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="2bd9c-138">Adicione o pacote do NuGet da API Web</span><span class="sxs-lookup"><span data-stu-id="2bd9c-138">Add the Web API NuGet Package</span></span>

<span data-ttu-id="2bd9c-139">Depois de instalar o Gerenciador de pacotes NuGet, adicione o pacote de Self de API da Web ao seu projeto.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-139">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="2bd9c-140">Dos **ferramentas** menu, selecione **Gerenciador de pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-140">From the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="2bd9c-141">*Observação*: Se você não vê esse menu item, certifique-se de que NuGet Package Manager instalada corretamente.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-141">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="2bd9c-142">Selecione **gerenciar pacotes NuGet para solução**</span><span class="sxs-lookup"><span data-stu-id="2bd9c-142">Select **Manage NuGet Packages for Solution**</span></span>
3. <span data-ttu-id="2bd9c-143">No **gerenciar pacotes NuGet** caixa de diálogo, selecione **Online**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-143">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="2bd9c-144">Na caixa de pesquisa, digite &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-144">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="2bd9c-145">Selecione o pacote de Host de autoatendimento do ASP.NET Web API e clique em **instalar**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-145">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="2bd9c-146">Depois de instala o pacote, clique em **feche** para fechar a caixa de diálogo.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-146">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="2bd9c-147">Certifique-se de instalar o pacote denominado Microsoft.AspNet.WebApi.SelfHost, não AspNetWebApi.SelfHost.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-147">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="2bd9c-148">Criar o modelo e o controlador</span><span class="sxs-lookup"><span data-stu-id="2bd9c-148">Create the Model and Controller</span></span>

<span data-ttu-id="2bd9c-149">Este tutorial usa as mesmas classes de modelo e controlador como o [guia de Introdução](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-149">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="2bd9c-150">Adicione uma classe pública denominada `Product`.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-150">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="2bd9c-151">Adicione uma classe pública denominada `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-151">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="2bd9c-152">Derive essa classe de **ApiController**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-152">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="2bd9c-153">Para obter mais informações sobre o código neste controlador, consulte o [guia de Introdução](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-153">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="2bd9c-154">Esse controlador define três ações GET:</span><span class="sxs-lookup"><span data-stu-id="2bd9c-154">This controller defines three GET actions:</span></span>

| <span data-ttu-id="2bd9c-155">URI</span><span class="sxs-lookup"><span data-stu-id="2bd9c-155">URI</span></span> | <span data-ttu-id="2bd9c-156">Descrição</span><span class="sxs-lookup"><span data-stu-id="2bd9c-156">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2bd9c-157">produtos/api /</span><span class="sxs-lookup"><span data-stu-id="2bd9c-157">/api/products</span></span> | <span data-ttu-id="2bd9c-158">Obtenha uma lista de todos os produtos.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-158">Get a list of all products.</span></span> |
| <span data-ttu-id="2bd9c-159">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="2bd9c-159">/api/products/*id*</span></span> | <span data-ttu-id="2bd9c-160">Obter um produto por ID.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-160">Get a product by ID.</span></span> |
| <span data-ttu-id="2bd9c-161">/api/products/?category=*category*</span><span class="sxs-lookup"><span data-stu-id="2bd9c-161">/api/products/?category=*category*</span></span> | <span data-ttu-id="2bd9c-162">Obtenha uma lista de produtos por categoria.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-162">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="2bd9c-163">Hospedar a API da Web</span><span class="sxs-lookup"><span data-stu-id="2bd9c-163">Host the Web API</span></span>

<span data-ttu-id="2bd9c-164">Abra o arquivo Program.cs e adicione as seguintes instruções using:</span><span class="sxs-lookup"><span data-stu-id="2bd9c-164">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="2bd9c-165">Adicione o seguinte código para o **programa** classe.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-165">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="2bd9c-166">(Opcional) Adicionar uma reserva de Namespace de URL HTTP</span><span class="sxs-lookup"><span data-stu-id="2bd9c-166">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="2bd9c-167">Esse aplicativo atende a `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-167">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="2bd9c-168">Por padrão, escutando em um determinado endereço HTTP requer privilégios de administrador.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-168">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="2bd9c-169">Portanto, quando você executar o tutorial, você pode receber esse erro: "HTTP não foi possível registrar a URL http://+:8080/" há duas maneiras de evitar esse erro:</span><span class="sxs-lookup"><span data-stu-id="2bd9c-169">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="2bd9c-170">Executar o Visual Studio com permissões de administrador com privilégios elevados, ou</span><span class="sxs-lookup"><span data-stu-id="2bd9c-170">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="2bd9c-171">Use Netsh.exe para conceder as permissões de conta para reservar a URL.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-171">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="2bd9c-172">Para usar Netsh.exe, abra um prompt de comando com privilégios de administrador e digite o seguinte comando: comando a seguir:</span><span class="sxs-lookup"><span data-stu-id="2bd9c-172">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="2bd9c-173">em que *máquina \ nomedeusuário* é sua conta de usuário.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-173">where *machine\username* is your user account.</span></span>

<span data-ttu-id="2bd9c-174">Quando tiver terminado de hospedagem interna, não se esqueça de excluir a reserva:</span><span class="sxs-lookup"><span data-stu-id="2bd9c-174">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="2bd9c-175">Chamar a API Web de um aplicativo cliente (c#)</span><span class="sxs-lookup"><span data-stu-id="2bd9c-175">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="2bd9c-176">Vamos escrever um aplicativo de console simples que chama a API da web.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-176">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="2bd9c-177">Adicione um novo projeto de aplicativo de console à solução:</span><span class="sxs-lookup"><span data-stu-id="2bd9c-177">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="2bd9c-178">No Gerenciador de soluções, a solução com o botão direito e selecione **adicionar novo projeto**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-178">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="2bd9c-179">Criar um novo aplicativo de console chamado &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-179">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="2bd9c-180">Use o Gerenciador de pacotes de NuGet para adicionar o pacote de bibliotecas de núcleo do ASP.NET Web API:</span><span class="sxs-lookup"><span data-stu-id="2bd9c-180">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="2bd9c-181">No menu Ferramentas, selecione **Gerenciador de pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-181">From the Tools menu, select **NuGet Package Manager**.</span></span>
- <span data-ttu-id="2bd9c-182">Selecione **gerenciar pacotes NuGet para solução**</span><span class="sxs-lookup"><span data-stu-id="2bd9c-182">Select **Manage NuGet Packages for Solution**</span></span>
- <span data-ttu-id="2bd9c-183">No **gerenciar pacotes NuGet** caixa de diálogo, selecione **Online**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-183">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="2bd9c-184">Na caixa de pesquisa, digite &quot;Microsoft.AspNet.WebApi.Client&quot;.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-184">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="2bd9c-185">Selecione o pacote de bibliotecas de cliente do Microsoft ASP.NET Web API e clique em **instalar**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-185">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="2bd9c-186">Adicione uma referência no ClientApp ao projeto SelfHost:</span><span class="sxs-lookup"><span data-stu-id="2bd9c-186">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="2bd9c-187">No Gerenciador de soluções, clique com botão direito no projeto ClientApp.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-187">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="2bd9c-188">Selecione **Adicionar Referência**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-188">Select **Add Reference**.</span></span>
- <span data-ttu-id="2bd9c-189">No **Gerenciador de referências** caixa de diálogo, em **solução**, selecione **projetos**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-189">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="2bd9c-190">Selecione o projeto SelfHost.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-190">Select the SelfHost project.</span></span>
- <span data-ttu-id="2bd9c-191">Clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-191">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="2bd9c-192">Abra o arquivo Client/Program.cs.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-192">Open the Client/Program.cs file.</span></span> <span data-ttu-id="2bd9c-193">Adicione o seguinte **usando** instrução:</span><span class="sxs-lookup"><span data-stu-id="2bd9c-193">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="2bd9c-194">Adicionar um estático **HttpClient** instância:</span><span class="sxs-lookup"><span data-stu-id="2bd9c-194">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="2bd9c-195">Adicione os seguintes métodos para listar todos os produtos, um produto por ID de lista e listam os produtos por categoria.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-195">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="2bd9c-196">Cada um desses métodos segue o mesmo padrão:</span><span class="sxs-lookup"><span data-stu-id="2bd9c-196">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="2bd9c-197">Chame **HttpClient.GetAsync** para enviar uma solicitação GET para o URI apropriado.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-197">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="2bd9c-198">Chame **HttpResponseMessage.EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-198">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="2bd9c-199">Esse método gera uma exceção se o status da resposta HTTP é um código de erro.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-199">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="2bd9c-200">Chame **ReadAsAsync&lt;T&gt;**  para desserializar um tipo CLR da resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-200">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="2bd9c-201">Esse método é um método de extensão definido no **System.Net.Http.HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-201">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="2bd9c-202">O **GetAsync** e **ReadAsAsync** métodos são assíncronas.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-202">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="2bd9c-203">Elas retornam **tarefa** objetos que representam a operação assíncrona.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-203">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="2bd9c-204">Introdução a **resultado** propriedade bloqueará o thread até que a operação seja concluída.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-204">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="2bd9c-205">Para obter mais informações sobre como usar o HttpClient, incluindo como fazer chamadas sem bloqueio, consulte [chamar um Web API de um cliente .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="2bd9c-205">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="2bd9c-206">Antes de chamar esses métodos, defina a propriedade BaseAddress na instância do HttpClient para "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="2bd9c-206">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="2bd9c-207">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="2bd9c-207">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="2bd9c-208">Essa saída a seguir.</span><span class="sxs-lookup"><span data-stu-id="2bd9c-208">This should output the following.</span></span> <span data-ttu-id="2bd9c-209">(Lembre-se de executar o aplicativo SelfHost primeiro.)</span><span class="sxs-lookup"><span data-stu-id="2bd9c-209">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
