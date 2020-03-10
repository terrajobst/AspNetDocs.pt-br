---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Implantação da Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros, por usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3881833bfe2a50a38a357614f92f434a04a8ab08
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78567399"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="cdfa2-103">Implantação da Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código</span><span class="sxs-lookup"><span data-stu-id="cdfa2-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>

<span data-ttu-id="cdfa2-104">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="cdfa2-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="cdfa2-105">Baixar o projeto inicial</span><span class="sxs-lookup"><span data-stu-id="cdfa2-105">Download Starter Project</span></span>](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="cdfa2-106">Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros usando o Visual Studio 2012 ou o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="cdfa2-107">Para obter informações sobre a série, consulte [o primeiro tutorial da série](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="cdfa2-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="cdfa2-108">Visão geral</span><span class="sxs-lookup"><span data-stu-id="cdfa2-108">Overview</span></span>

<span data-ttu-id="cdfa2-109">Após a implantação inicial, seu trabalho de manutenção e desenvolvimento de seu site continua e, antes de longo, você desejará implantar uma atualização.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="cdfa2-110">Este tutorial orienta você pelo processo de implantação de uma atualização no código do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="cdfa2-111">A atualização implementada e implantada neste tutorial não envolve uma alteração no banco de dados; Você verá o que há de diferente na implantação de uma alteração de banco de dados no próximo tutorial.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="cdfa2-112">Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="cdfa2-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="cdfa2-113">Alterar um código</span><span class="sxs-lookup"><span data-stu-id="cdfa2-113">Make a code change</span></span>

<span data-ttu-id="cdfa2-114">Como um exemplo simples de uma atualização para seu aplicativo, você adicionará à página **instrutores** uma lista de cursos ensinados pelo instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="cdfa2-115">Se você executar a página **instrutores** , observará que há links **selecionados** na grade, mas eles não fazem nada além de deixar o plano de fundo da linha ficar cinza.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Página de instrutores com seleção](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="cdfa2-117">Agora você adicionará o código que é executado quando o link **selecionar** é clicado e exibe uma lista de cursos ensinados pelo instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="cdfa2-118">Em *instrutores. aspx*, adicione a seguinte marcação imediatamente após o controle de `Label` **ErrorMessageLabel** :</span><span class="sxs-lookup"><span data-stu-id="cdfa2-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="cdfa2-119">Execute a página e selecione um instrutor.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-119">Run the page and select an instructor.</span></span> <span data-ttu-id="cdfa2-120">Você verá uma lista de cursos ensinados por esse instrutor.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-120">You see a list of courses taught by that instructor.</span></span>

    ![Página de instrutores com cursos ensinados](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="cdfa2-122">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="cdfa2-123">Implantar a atualização de código no ambiente de teste</span><span class="sxs-lookup"><span data-stu-id="cdfa2-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="cdfa2-124">Antes de poder usar seus perfis de publicação para implantar para teste, preparo e produção, você precisa alterar as opções de publicação do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="cdfa2-125">Você não precisa mais executar os scripts de concessão e de implantação de dados para o banco de dado de associação.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="cdfa2-126">Abra o assistente **publicar na Web** clicando com o botão direito do mouse no projeto ContosoUniversity e clicando em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="cdfa2-127">Clique no perfil de **teste** na lista suspensa **perfil** .</span><span class="sxs-lookup"><span data-stu-id="cdfa2-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="cdfa2-128">Clique na guia **Configurações** .</span><span class="sxs-lookup"><span data-stu-id="cdfa2-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="cdfa2-129">Em **DefaultConnection** na seção **databases** , desmarque a caixa de seleção **Atualizar banco de dados** .</span><span class="sxs-lookup"><span data-stu-id="cdfa2-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="cdfa2-130">Clique na guia **perfil** e, em seguida, clique no perfil de **preparo** na lista suspensa **perfil** .</span><span class="sxs-lookup"><span data-stu-id="cdfa2-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="cdfa2-131">Quando for perguntado se deseja salvar as alterações feitas no perfil de **teste** , clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="cdfa2-132">Faça a mesma alteração no perfil de preparo.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="cdfa2-133">Repita o processo para fazer a mesma alteração no perfil de produção.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="cdfa2-134">Feche o assistente **publicar Web** .</span><span class="sxs-lookup"><span data-stu-id="cdfa2-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="cdfa2-135">A implantação no ambiente de teste agora é uma simples questão de executar a publicação com um clique novamente.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="cdfa2-136">Para tornar esse processo mais rápido, você pode usar a barra de ferramentas de **publicação de um clique da Web** .</span><span class="sxs-lookup"><span data-stu-id="cdfa2-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="cdfa2-137">No menu **Exibir** , escolha **barras de ferramentas** e, em seguida, selecione **Web um clique em publicar**.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="cdfa2-139">Em **Gerenciador de soluções**, selecione o projeto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="cdfa2-140">a barra de ferramentas de **publicação de um clique da Web** , escolha o perfil de publicação de **teste** e clique em **publicar Web** (o ícone com setas apontando para a esquerda e para a direita).</span><span class="sxs-lookup"><span data-stu-id="cdfa2-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="cdfa2-142">O Visual Studio implanta o aplicativo atualizado e o navegador é aberto automaticamente na home page.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="cdfa2-143">Execute a página instrutores e selecione um instrutor para verificar se a atualização foi implantada com êxito.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="cdfa2-144">Normalmente, você também faria testes de regressão (ou seja, teste o restante do site para garantir que a nova alteração não interrompa nenhuma funcionalidade existente).</span><span class="sxs-lookup"><span data-stu-id="cdfa2-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="cdfa2-145">Mas, para este tutorial, você ignorará essa etapa e continuará a implantar a atualização para preparo e produção.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="cdfa2-146">Quando você reimplanta, o Implantação da Web determina automaticamente quais arquivos foram alterados e copia apenas os arquivos alterados para o servidor.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="cdfa2-147">Por padrão, Implantação da Web usa as datas da última alteração nos arquivos para determinar quais foram alterados.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="cdfa2-148">Alguns sistemas de controle do código-fonte alteram datas do arquivo mesmo quando você não altera o conteúdo do arquivo.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="cdfa2-149">Nesse caso, talvez você queira configurar Implantação da Web para usar as somas de verificação de arquivo para determinar quais arquivos foram alterados.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="cdfa2-150">Para obter mais informações, consulte [por que todos os meus arquivos são reimplantados, embora eu não os tenha alterado?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) nas perguntas frequentes sobre a implantação do ASP.net.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="cdfa2-151">Colocar o aplicativo offline durante a implantação</span><span class="sxs-lookup"><span data-stu-id="cdfa2-151">Take the application offline during deployment</span></span>

<span data-ttu-id="cdfa2-152">A alteração que você está implantando agora é uma simples alteração em uma única página.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="cdfa2-153">Mas, às vezes, você implanta alterações maiores ou implanta alterações de código e de banco de dados, e o site poderá se comportar incorretamente se um usuário solicitar uma página antes da conclusão da implantação.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="cdfa2-154">Para impedir que os usuários acessem o site enquanto a implantação está em andamento, você pode usar um *aplicativo\_arquivo offline. htm* .</span><span class="sxs-lookup"><span data-stu-id="cdfa2-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="cdfa2-155">Quando você coloca um arquivo chamado *app\_offline. htm* na pasta raiz do seu aplicativo, o IIS exibe automaticamente esse arquivo em vez de executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="cdfa2-156">Portanto, para evitar o acesso durante a implantação, coloque o *aplicativo\_offline. htm* na pasta raiz, execute o processo de implantação e remova o *aplicativo\_offline. htm* após a implantação bem-sucedida.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="cdfa2-157">Você pode configurar Implantação da Web para colocar automaticamente um *aplicativo padrão\_arquivo offline. htm* no servidor quando ele iniciar a implantação e removê-lo quando for concluído.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="cdfa2-158">Para fazer isso, tudo o que você precisa fazer é adicionar o seguinte elemento XML ao arquivo de perfil de publicação (. pubxml):</span><span class="sxs-lookup"><span data-stu-id="cdfa2-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="cdfa2-159">Para este tutorial, você verá como criar e usar um aplicativo personalizado *\_arquivo offline. htm* .</span><span class="sxs-lookup"><span data-stu-id="cdfa2-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="cdfa2-160">O uso do *aplicativo\_offline. htm* no site de preparo não é necessário, pois você não tem usuários acessando o site de preparo.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="cdfa2-161">Mas é uma boa prática usar o preparo para testar tudo, como você planeja implantar na produção.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-app_offlinehtm"></a><span data-ttu-id="cdfa2-162">Criar aplicativo\_offline. htm</span><span class="sxs-lookup"><span data-stu-id="cdfa2-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="cdfa2-163">Em **Gerenciador de soluções**, clique com o botão direito do mouse na solução e clique em **Adicionar**e em **novo item**.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="cdfa2-164">Crie uma **página HTML** denominada *app\_offline. htm* (exclua o "l" final na extensão *. html* que o Visual Studio cria por padrão).</span><span class="sxs-lookup"><span data-stu-id="cdfa2-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="cdfa2-165">Substitua a marcação do modelo pela seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="cdfa2-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="cdfa2-166">Salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-166">Save and close the file.</span></span>

### <a name="copy-app_offlinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="cdfa2-167">Copie o aplicativo\_offline. htm para a pasta raiz do site da Web</span><span class="sxs-lookup"><span data-stu-id="cdfa2-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="cdfa2-168">Você pode usar qualquer ferramenta de FTP para copiar arquivos para o site.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="cdfa2-169">O [FileZilla](http://filezilla-project.org/) é uma ferramenta de FTP popular e é mostrada nas capturas de tela.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="cdfa2-170">Para usar uma ferramenta de FTP, você precisa de três coisas: a URL de FTP, o nome de usuário e a senha.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="cdfa2-171">A URL é mostrada na página painel do site na Portal de Gerenciamento do Azure e o nome de usuário e a senha do FTP podem ser encontrados no arquivo *. publishsettings* que você baixou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="cdfa2-172">As etapas a seguir mostram como obter esses valores.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="cdfa2-173">Na Portal de Gerenciamento do Azure, clique na guia **sites** e clique no site de preparo.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="cdfa2-174">Na página **painel** , role para baixo até localizar o nome do host FTP na seção **visão rápida** .</span><span class="sxs-lookup"><span data-stu-id="cdfa2-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![Nome do host do FTP](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="cdfa2-176">Abra o arquivo *. publishsettings* de preparo no bloco de notas ou em outro editor de texto.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="cdfa2-177">Localize o elemento `publishProfile` para o perfil de FTP.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="cdfa2-178">Copie os valores de `userName` e `userPWD`.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-178">Copy the `userName` and `userPWD` values.</span></span>

    ![Credenciais de FTP](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="cdfa2-180">Abra sua ferramenta FTP e faça logon na URL do FTP.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="cdfa2-181">Copie o *aplicativo\_offline. htm* da pasta da solução para a pasta */site/wwwroot* no site de preparo.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Copiar app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="cdfa2-183">Navegue até a URL do site de preparo.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="cdfa2-184">Você verá que a página do *aplicativo\_offline. htm* agora é exibida em vez de seu Home Page.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![app_offline. htm na janela do navegador](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="cdfa2-186">Agora você está pronto para implantar no preparo.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="cdfa2-187">Implantar a atualização de código para preparo e produção</span><span class="sxs-lookup"><span data-stu-id="cdfa2-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="cdfa2-188">Na barra de ferramentas de **publicação de um clique da Web** , escolha o perfil de publicação de **preparo** e clique em **publicar Web**.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="cdfa2-189">O Visual Studio implanta o aplicativo atualizado e abre o navegador para o home page do site.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="cdfa2-190">O arquivo *offline. htm do aplicativo\_* é exibido.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="cdfa2-191">Antes de testar para verificar a implantação bem-sucedida, você deve remover o arquivo *\_offline. htm do aplicativo* .</span><span class="sxs-lookup"><span data-stu-id="cdfa2-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="cdfa2-192">Retorne à ferramenta de FTP e exclua o **aplicativo\_offline. htm** do site de preparo.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="cdfa2-193">No navegador, abra a página instrutores no site de preparo e selecione um instrutor para verificar se a atualização foi implantada com êxito.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="cdfa2-194">Siga o mesmo procedimento para produção, como fez para preparo.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="cdfa2-195">Revisando alterações e implantando arquivos específicos</span><span class="sxs-lookup"><span data-stu-id="cdfa2-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="cdfa2-196">O Visual Studio 2012 também oferece a capacidade de implantar arquivos individuais.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="cdfa2-197">Para um arquivo selecionado, você pode exibir as diferenças entre a versão local e a versão implantada, implantar o arquivo no ambiente de destino ou copiar o arquivo do ambiente de destino para o projeto local.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="cdfa2-198">Nesta seção do tutorial, você verá como usar esses recursos.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="cdfa2-199">Fazer uma alteração para implantar</span><span class="sxs-lookup"><span data-stu-id="cdfa2-199">Make a change to deploy</span></span>

1. <span data-ttu-id="cdfa2-200">Abra *Content/site. css*e localize o bloco para a marca de `body`.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="cdfa2-201">Altere o valor de `background-color` de `#fff` para `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="cdfa2-202">Exibir a alteração na janela de visualização de publicação</span><span class="sxs-lookup"><span data-stu-id="cdfa2-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="cdfa2-203">Ao usar o assistente **publicar Web** para publicar o projeto, você pode ver quais alterações serão publicadas clicando duas vezes no arquivo na janela de **Visualização** .</span><span class="sxs-lookup"><span data-stu-id="cdfa2-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="cdfa2-204">Clique com o botão direito do mouse no projeto ContosoUniversity e clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="cdfa2-205">Na lista suspensa **perfil** , selecione o perfil de publicação de **teste** .</span><span class="sxs-lookup"><span data-stu-id="cdfa2-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="cdfa2-206">Clique em **Visualizar**e, em seguida, clique em **Iniciar visualização**.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="cdfa2-207">No painel de **Visualização** , clique duas vezes em **site. css**.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Clique duas vezes em site. css](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="cdfa2-209">A caixa de diálogo **Visualizar alterações** mostra uma visualização das alterações que serão implantadas.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Visualizar alterações em site. css](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="cdfa2-211">Se você clicar duas vezes no arquivo *Web. config* , a caixa de diálogo **Visualizar alterações** mostrará o efeito de suas transformações de configuração de compilação e publicação das transformações de perfil.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="cdfa2-212">Neste ponto, você não fez nada que faria com que o arquivo *Web. config* no servidor fosse alterado, portanto, você espera não ver nenhuma alteração.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="cdfa2-213">No entanto, a janela **Visualizar alterações** incorretamente mostra duas alterações.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="cdfa2-214">Parece que dois elementos XML serão removidos.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="cdfa2-215">Esses elementos são adicionados pelo processo de publicação quando você seleciona **executar migrações do Code First no início do aplicativo** para uma classe de contexto de Code First.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="cdfa2-216">A comparação é feita antes que o processo de publicação adicione esses elementos, portanto, parece que eles estão sendo removidos, embora não sejam removidos.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="cdfa2-217">Esse erro será corrigido em uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="cdfa2-218">Clique em **fechar**</span><span class="sxs-lookup"><span data-stu-id="cdfa2-218">Click **Close**.</span></span>
6. <span data-ttu-id="cdfa2-219">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-219">Click **Publish**.</span></span>
7. <span data-ttu-id="cdfa2-220">Quando o navegador for aberto na home page do site de teste, pressione CTRL + F5 para fazer uma atualização impressa para ver o efeito da alteração do CSS.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Efeito da alteração de CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="cdfa2-222">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="cdfa2-223">Publicar arquivos específicos de Gerenciador de Soluções</span><span class="sxs-lookup"><span data-stu-id="cdfa2-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="cdfa2-224">Suponha que você não goste do plano de fundo azul e queira reverter para a cor original.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="cdfa2-225">Nesta seção, você irá restaurar as configurações originais publicando um arquivo específico diretamente do **Gerenciador de soluções**.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="cdfa2-226">Abra *Content/site. css* e restaure a configuração de `background-color` para `#fff`.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="cdfa2-227">Em **Gerenciador de soluções**, clique com o botão direito do mouse no arquivo *Content/site. css* .</span><span class="sxs-lookup"><span data-stu-id="cdfa2-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="cdfa2-228">O menu de contexto mostra três opções de publicação.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-228">The context menu shows three publish options.</span></span>

    ![Opções de publicação de Gerenciador de Soluções](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="cdfa2-230">Clique em **Visualizar alterações em site. css**.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="cdfa2-231">Uma janela é aberta para mostrar as diferenças entre o arquivo local e a versão dele no ambiente de destino.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="cdfa2-233">Em **Gerenciador de soluções**, clique com o botão direito do mouse em **site. css** novamente e clique em **publicar site. css**.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="cdfa2-234">A janela **atividade de publicação na Web** mostra que o arquivo foi publicado.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Janela de atividade de publicação na Web](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="cdfa2-236">Abra um navegador para a URL de `http://localhost/contosouniversity` e pressione CTRL + F5 para fazer uma atualização rígida para ver o efeito da alteração de CSS.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Home Page com CSS normal](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="cdfa2-238">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="cdfa2-239">Resumo</span><span class="sxs-lookup"><span data-stu-id="cdfa2-239">Summary</span></span>

<span data-ttu-id="cdfa2-240">Agora você viu várias maneiras de implantar uma atualização de aplicativo que não envolve uma alteração de banco de dados e viu como visualizar as alterações para verificar se o que será atualizado é o que você espera.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="cdfa2-241">A página de instrutores agora tem uma seção **cursos ensinados** .</span><span class="sxs-lookup"><span data-stu-id="cdfa2-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Página de instrutores com cursos ensinados](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="cdfa2-243">O próximo tutorial mostra como implantar uma alteração no banco de dados: você adicionará um campo DataDeNascimento ao banco de dados e à página instrutores.</span><span class="sxs-lookup"><span data-stu-id="cdfa2-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cdfa2-244">[Anterior](deploying-to-production.md)
> [Próximo](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="cdfa2-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
