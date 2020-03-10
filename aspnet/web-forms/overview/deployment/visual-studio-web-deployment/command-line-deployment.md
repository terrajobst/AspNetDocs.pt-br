---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Implantação da Web do ASP.NET usando o Visual Studio: implantação de linha de comando | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros, por usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13cfe4492398b59f2c80394689cc113ccb218c60
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630917"
---
# <a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="b2b9f-103">Implantação da Web do ASP.NET usando o Visual Studio: implantação de linha de comando</span><span class="sxs-lookup"><span data-stu-id="b2b9f-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>

<span data-ttu-id="b2b9f-104">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="b2b9f-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="b2b9f-105">Baixar o projeto inicial</span><span class="sxs-lookup"><span data-stu-id="b2b9f-105">Download Starter Project</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="b2b9f-106">Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros usando o Visual Studio 2012 ou o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="b2b9f-107">Para obter informações sobre a série, consulte [o primeiro tutorial da série](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b2b9f-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="b2b9f-108">Visão geral</span><span class="sxs-lookup"><span data-stu-id="b2b9f-108">Overview</span></span>

<span data-ttu-id="b2b9f-109">Este tutorial mostra como invocar o pipeline de publicação da Web do Visual Studio a partir da linha de comando.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="b2b9f-110">Isso é útil para cenários em que você deseja [automatizar o processo de implantação](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) em vez de fazê-lo manualmente no Visual Studio, normalmente usando um [sistema de controle de versão de código-fonte](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span><span class="sxs-lookup"><span data-stu-id="b2b9f-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="b2b9f-111">Fazer uma alteração para implantar</span><span class="sxs-lookup"><span data-stu-id="b2b9f-111">Make a change to deploy</span></span>

<span data-ttu-id="b2b9f-112">Atualmente, a página sobre exibe o código do modelo.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-112">Currently the About page displays the template code.</span></span>

![Página sobre com código de modelo](command-line-deployment/_static/image1.png)

<span data-ttu-id="b2b9f-114">Você o substituirá pelo código que exibe um resumo do registro do aluno.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="b2b9f-115">Abra a página *about. aspx* , exclua toda a marcação dentro do elemento `MainContent` `Content` e insira a seguinte marcação em seu lugar:</span><span class="sxs-lookup"><span data-stu-id="b2b9f-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="b2b9f-116">Execute o projeto e selecione a página **sobre** .</span><span class="sxs-lookup"><span data-stu-id="b2b9f-116">Run the project and select the **About** page.</span></span>

![Página Sobre](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="b2b9f-118">Implantar para teste usando a linha de comando</span><span class="sxs-lookup"><span data-stu-id="b2b9f-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="b2b9f-119">Você não implantará outra alteração no banco de dados, portanto, desabilite a implantação do banco de dados dbDacFx para o banco de dados ASPNET-ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="b2b9f-120">Abra o assistente **publicar Web** e, em cada um dos três perfis de publicação, desmarque a caixa de seleção **Atualizar banco de dados** na guia **configurações** .</span><span class="sxs-lookup"><span data-stu-id="b2b9f-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="b2b9f-121">Na página inicial do Windows 8, pesquise por **prompt de comando do desenvolvedor para VS2012**.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="b2b9f-122">Clique com o botão direito do mouse no ícone para **prompt de comando do desenvolvedor para VS2012** e clique em **Executar como administrador**.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="b2b9f-123">Digite o seguinte comando no prompt de comando, substituindo o caminho para o arquivo de solução pelo caminho para o arquivo de solução:</span><span class="sxs-lookup"><span data-stu-id="b2b9f-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="b2b9f-124">O MSBuild compila a solução e a implanta no ambiente de teste.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![Saída da linha de comando](command-line-deployment/_static/image3.png)

<span data-ttu-id="b2b9f-126">Abra um navegador e vá para `http://localhost/ContosoUniversity`e, em seguida, clique na página **sobre** para verificar se a implantação foi bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="b2b9f-127">Se você não tiver criado nenhum aluno em teste, verá uma página vazia no título de **estatísticas do corpo do aluno** .</span><span class="sxs-lookup"><span data-stu-id="b2b9f-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="b2b9f-128">Vá para a página **alunos** , clique em **Adicionar aluno**e adicione alguns alunos e, em seguida, retorne à página **sobre** para ver as estatísticas dos alunos.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![Página sobre no ambiente de teste](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="b2b9f-130">Opções de linha de comando de chave</span><span class="sxs-lookup"><span data-stu-id="b2b9f-130">Key command line options</span></span>

<span data-ttu-id="b2b9f-131">O comando que você inseriu passou o caminho do arquivo da solução e duas propriedades para o MSBuild:</span><span class="sxs-lookup"><span data-stu-id="b2b9f-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="b2b9f-132">Implantando a solução em vez de implantar projetos individuais</span><span class="sxs-lookup"><span data-stu-id="b2b9f-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="b2b9f-133">A especificação do arquivo de solução faz com que todos os projetos na solução sejam compilados.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="b2b9f-134">Se você tiver vários projetos Web na solução, o seguinte comportamento do MSBuild se aplicará:</span><span class="sxs-lookup"><span data-stu-id="b2b9f-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="b2b9f-135">As propriedades que você especificar na linha de comando são passadas para cada projeto.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="b2b9f-136">Portanto, cada projeto Web deve ter um perfil de publicação com o nome que você especificar.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="b2b9f-137">Se você especificar `/p:PublishProfile=Test`, cada projeto da Web deverá ter um perfil de publicação chamado *teste*.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="b2b9f-138">Você pode publicar um projeto com êxito quando outro não mesmo é compilado.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="b2b9f-139">Para obter mais informações, consulte o StackOverflow thread [MSBuild falha com dois pacotes](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span><span class="sxs-lookup"><span data-stu-id="b2b9f-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="b2b9f-140">Se você especificar um projeto individual em vez de uma solução, precisará adicionar um parâmetro que especifica a versão do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="b2b9f-141">Se você estiver usando o Visual Studio 2012, a linha de comando será semelhante ao exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="b2b9f-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="b2b9f-142">O número de versão do Visual Studio 2010 é 10,0.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="b2b9f-143">Para obter mais informações, consulte [compatibilidade de projetos do Visual Studio e VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) no blog do sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-143">For more information, see [Visual Studio project compatibility and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="b2b9f-144">Especificando o perfil de publicação</span><span class="sxs-lookup"><span data-stu-id="b2b9f-144">Specifying the publish profile</span></span>

<span data-ttu-id="b2b9f-145">Você pode especificar o perfil de publicação por nome ou pelo caminho completo para o arquivo *. pubxml* , conforme mostrado no exemplo a seguir:</span><span class="sxs-lookup"><span data-stu-id="b2b9f-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="b2b9f-146">Métodos de publicação na Web com suporte para publicação de linha de comando</span><span class="sxs-lookup"><span data-stu-id="b2b9f-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="b2b9f-147">Há suporte para três métodos de publicação para publicação de linha de comando:</span><span class="sxs-lookup"><span data-stu-id="b2b9f-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="b2b9f-148">`MSDeploy`-publicar usando Implantação da Web.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="b2b9f-149">`Package`-publicar criando um pacote de Implantação da Web.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="b2b9f-150">Você precisa instalar o pacote separadamente do comando MSBuild que o cria.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="b2b9f-151">`FileSystem`-publicar copiando arquivos em uma pasta especificada.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="b2b9f-152">Especificando a plataforma e a configuração de compilação</span><span class="sxs-lookup"><span data-stu-id="b2b9f-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="b2b9f-153">A configuração e a plataforma de compilação devem ser definidas no Visual Studio ou na linha de comando.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="b2b9f-154">Os perfis de publicação incluem propriedades nomeadas `LastUsedBuildConfiguration` e `LastUsedPlatform`, mas você não pode definir essas propriedades para determinar como o projeto é compilado.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="b2b9f-155">Para obter mais informações, consulte [MSBuild: como definir a propriedade de configuração](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) no blog de sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="b2b9f-156">Implantar no preparo</span><span class="sxs-lookup"><span data-stu-id="b2b9f-156">Deploy to staging</span></span>

<span data-ttu-id="b2b9f-157">Para implantar no Azure, você deve adicionar a senha à linha de comando.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="b2b9f-158">Se você salvou a senha no perfil de publicação no Visual Studio, ela foi armazenada em formato criptografado no arquivo *. pubxml. User* .</span><span class="sxs-lookup"><span data-stu-id="b2b9f-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="b2b9f-159">Esse arquivo não é acessado pelo MSBuild quando você faz uma implantação de linha de comando, portanto, você precisa passar a senha em um parâmetro de linha de comando.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="b2b9f-160">Copie a senha de que você precisa do arquivo *. publishsettings* que você baixou anteriormente para o site de preparo.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="b2b9f-161">A senha é o valor do atributo `userPWD` para o elemento Implantação da Web `publishProfile`.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Implantação da Web senha](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="b2b9f-163">Na página inicial do Windows 8, procure **prompt de comando do desenvolvedor para VS2012**e clique no ícone para abrir o prompt de comando.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="b2b9f-164">(Você não precisa abri-lo como administrador desta vez porque não está implantando no IIS no computador local.)</span><span class="sxs-lookup"><span data-stu-id="b2b9f-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="b2b9f-165">Digite o seguinte comando no prompt de comando, substituindo o caminho para o arquivo de solução pelo caminho para o arquivo de solução e a senha com sua senha:</span><span class="sxs-lookup"><span data-stu-id="b2b9f-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="b2b9f-166">Observe que essa linha de comando inclui um parâmetro extra: `/p:AllowUntrustedCertificate=true`.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="b2b9f-167">Como este tutorial está sendo gravado, a propriedade `AllowUntrustedCertificate` deve ser definida quando você publica no Azure por meio da linha de comando.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="b2b9f-168">Quando a correção para esse bug for liberada, você não precisará desse parâmetro.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="b2b9f-169">Abra um navegador e acesse a URL do seu site de preparo e clique na página **sobre** para verificar se a implantação foi bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="b2b9f-170">Como vimos anteriormente para o ambiente de teste, talvez seja necessário criar alguns alunos para ver as estatísticas na página **sobre** .</span><span class="sxs-lookup"><span data-stu-id="b2b9f-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="b2b9f-171">Implantar na produção</span><span class="sxs-lookup"><span data-stu-id="b2b9f-171">Deploy to production</span></span>

<span data-ttu-id="b2b9f-172">O processo de implantação em produção é semelhante ao processo de preparo.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="b2b9f-173">Copie a senha de que você precisa do arquivo *. publishsettings* que você baixou anteriormente para o site de produção.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="b2b9f-174">Abra **prompt de comando do desenvolvedor para VS2012**.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="b2b9f-175">Digite o seguinte comando no prompt de comando, substituindo o caminho para o arquivo de solução pelo caminho para o arquivo de solução e a senha com sua senha:</span><span class="sxs-lookup"><span data-stu-id="b2b9f-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="b2b9f-176">Para um site de produção real, se também houvesse uma alteração no banco de dados, você normalmente copiaria o arquivo do *aplicativo\_offline. htm* para o site antes da implantação e excluí-lo após a implantação bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="b2b9f-177">Abra um navegador e acesse a URL do seu site de preparo e clique na página **sobre** para verificar se a implantação foi bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="b2b9f-178">Resumo</span><span class="sxs-lookup"><span data-stu-id="b2b9f-178">Summary</span></span>

<span data-ttu-id="b2b9f-179">Agora você implantou uma atualização de aplicativo usando a linha de comando.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-179">You have now deployed an application update by using the command line.</span></span>

![Página sobre no ambiente de teste](command-line-deployment/_static/image6.png)

<span data-ttu-id="b2b9f-181">No próximo tutorial, você verá um exemplo de como estender o pipeline de publicação na Web.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="b2b9f-182">O exemplo mostrará como implantar arquivos que não estão incluídos no projeto.</span><span class="sxs-lookup"><span data-stu-id="b2b9f-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b2b9f-183">[Anterior](deploying-a-database-update.md)
> [Próximo](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="b2b9f-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
