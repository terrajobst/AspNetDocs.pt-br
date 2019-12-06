---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Criar um aplicativo Web Secure ASP.NET MVC 5 com logon, confirmação de email e redefinição deC#senha () | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como criar um aplicativo Web ASP.NET MVC 5 com confirmação de email e redefinição de senha usando o sistema de associação ASP.NET Identity. Sua autoridade de certificação...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 07f5b290b73f75000e6f29fe09e4dc25e144452f
ms.sourcegitcommit: 969e7db924ebad3cc0f0cb0d65d148e8b9221b9a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74899703"
---
# <a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="e9da0-104">Criar um aplicativo Web seguro do ASP.NET MVC 5 com logon, confirmação por email e redefinição de senha (C#)</span><span class="sxs-lookup"><span data-stu-id="e9da0-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>

<span data-ttu-id="e9da0-105">por [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="e9da0-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

<span data-ttu-id="e9da0-106">Este tutorial mostra como criar um aplicativo Web ASP.NET MVC 5 com confirmação de email e redefinição de senha usando o sistema de associação ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="e9da0-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span>

<span data-ttu-id="e9da0-107">Para obter uma versão atualizada deste tutorial que usa o .NET Core, consulte [confirmação da conta e recuperação de senha no ASP.NET Core [/ASPNET/Core/Security/Authentication/accconfirm).</span><span class="sxs-lookup"><span data-stu-id="e9da0-107">For an updated version of this tutorial that uses .NET Core, see [Account confirmation and password recovery in ASP.NET Core[/aspnet/core/security/authentication/accconfirm).</span></span>

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="e9da0-108">Criar um aplicativo MVC do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e9da0-108">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="e9da0-109">Comece Instalando e executando o [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="e9da0-109">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="e9da0-110">Instale [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior.</span><span class="sxs-lookup"><span data-stu-id="e9da0-110">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="e9da0-111">Aviso: você deve instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior para concluir este tutorial.</span><span class="sxs-lookup"><span data-stu-id="e9da0-111">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>

1. <span data-ttu-id="e9da0-112">Crie um novo projeto Web ASP.NET e selecione o modelo MVC.</span><span class="sxs-lookup"><span data-stu-id="e9da0-112">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="e9da0-113">Web Forms também dá suporte a ASP.NET Identity, portanto, você pode seguir etapas semelhantes em um aplicativo Web Forms.</span><span class="sxs-lookup"><span data-stu-id="e9da0-113">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="e9da0-114">Deixe a autenticação padrão como **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="e9da0-114">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="e9da0-115">Se você quiser hospedar o aplicativo no Azure, deixe a caixa de seleção marcada.</span><span class="sxs-lookup"><span data-stu-id="e9da0-115">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="e9da0-116">Posteriormente, no tutorial, iremos implantar no Azure.</span><span class="sxs-lookup"><span data-stu-id="e9da0-116">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="e9da0-117">Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="e9da0-117">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="e9da0-118">Defina o [projeto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="e9da0-118">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="e9da0-119">Execute o aplicativo, clique no link **registrar** e registre um usuário.</span><span class="sxs-lookup"><span data-stu-id="e9da0-119">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="e9da0-120">Neste ponto, a única validação no email é com o atributo [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) .</span><span class="sxs-lookup"><span data-stu-id="e9da0-120">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="e9da0-121">Em Gerenciador de Servidores, navegue até **Data Connections\DefaultConnection\Tables\AspNetUsers**, clique com o botão direito do mouse e selecione **Abrir definição de tabela**.</span><span class="sxs-lookup"><span data-stu-id="e9da0-121">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="e9da0-122">A imagem a seguir mostra o esquema de `AspNetUsers`:</span><span class="sxs-lookup"><span data-stu-id="e9da0-122">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="e9da0-123">Clique com o botão direito do mouse na tabela **AspNetUsers** e selecione **Mostrar dados da tabela**.</span><span class="sxs-lookup"><span data-stu-id="e9da0-123">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="e9da0-124">Neste ponto, o email não foi confirmado.</span><span class="sxs-lookup"><span data-stu-id="e9da0-124">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="e9da0-125">Clique na linha e selecione Excluir.</span><span class="sxs-lookup"><span data-stu-id="e9da0-125">Click on the row and select delete.</span></span> <span data-ttu-id="e9da0-126">Você adicionará esse email novamente na próxima etapa e enviará um email de confirmação.</span><span class="sxs-lookup"><span data-stu-id="e9da0-126">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="e9da0-127">Confirmação de email</span><span class="sxs-lookup"><span data-stu-id="e9da0-127">Email confirmation</span></span>

<span data-ttu-id="e9da0-128">É uma prática recomendada confirmar o email de um novo registro de usuário para verificar se eles não estão representando outra pessoa (ou seja, se eles não se registraram no email de outra pessoa).</span><span class="sxs-lookup"><span data-stu-id="e9da0-128">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="e9da0-129">Suponha que você tenha um fórum de discussão, você desejaria impedir que `"bob@example.com"` se registrem como `"joe@contoso.com"`.</span><span class="sxs-lookup"><span data-stu-id="e9da0-129">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="e9da0-130">Sem confirmação por email, `"joe@contoso.com"` pode obter email indesejado de seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e9da0-130">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="e9da0-131">Suponha que Bob seja acidentalmente registrado como `"bib@example.com"` e não tenha notado, ele não seria capaz de usar a recuperação de senha porque o aplicativo não tem seu email correto.</span><span class="sxs-lookup"><span data-stu-id="e9da0-131">Suppose Bob accidentally registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="e9da0-132">A confirmação por email fornece apenas proteção limitada de bots e não fornece proteção contra remetentes de spam determinados, eles têm muitos aliases de email de trabalho que podem usar para se registrar.</span><span class="sxs-lookup"><span data-stu-id="e9da0-132">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="e9da0-133">Em geral, você deseja impedir que novos usuários enviem dados para seu site antes de terem sido confirmados por email, uma mensagem de texto SMS ou outro mecanismo.</span><span class="sxs-lookup"><span data-stu-id="e9da0-133">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="e9da0-134">Nas seções a seguir, Habilitaremos a confirmação de email e modificaremos o código para impedir que usuários registrados recentemente façam logon até que seu email seja confirmado.</span><span class="sxs-lookup"><span data-stu-id="e9da0-134">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="e9da0-135">Conectar SendGrid</span><span class="sxs-lookup"><span data-stu-id="e9da0-135">Hook up SendGrid</span></span>

<span data-ttu-id="e9da0-136">As instruções nesta seção não são atuais.</span><span class="sxs-lookup"><span data-stu-id="e9da0-136">The instructions in this section are not current.</span></span> <span data-ttu-id="e9da0-137">Consulte [Configurar provedor de email do SendGrid](/aspnet/core/security/authentication/accconfirm#configure-email-provider) para obter instruções atualizadas.</span><span class="sxs-lookup"><span data-stu-id="e9da0-137">See [Configure SendGrid email provider](/aspnet/core/security/authentication/accconfirm#configure-email-provider) for updated instructions.</span></span>

<span data-ttu-id="e9da0-138">Embora este tutorial mostre apenas como adicionar notificação por email por meio do [SendGrid](http://sendgrid.com/), você pode enviar emails usando SMTP e outros mecanismos (consulte [recursos adicionais](#addRes)).</span><span class="sxs-lookup"><span data-stu-id="e9da0-138">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="e9da0-139">No Console do Gerenciador de Pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="e9da0-139">In the Package Manager Console, enter the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="e9da0-140">Vá para a [página de inscrição do SendGrid do Azure](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) e registre-se para uma conta gratuita do SendGrid.</span><span class="sxs-lookup"><span data-stu-id="e9da0-140">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="e9da0-141">Configure o SendGrid adicionando um código semelhante ao seguinte no *App_Start/identityconfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="e9da0-141">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="e9da0-142">Você precisará adicionar o seguinte:</span><span class="sxs-lookup"><span data-stu-id="e9da0-142">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="e9da0-143">Para manter esse exemplo simples, armazenaremos as configurações do aplicativo no arquivo *Web. config* :</span><span class="sxs-lookup"><span data-stu-id="e9da0-143">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="e9da0-144">Segurança-nunca armazene dados confidenciais em seu código-fonte.</span><span class="sxs-lookup"><span data-stu-id="e9da0-144">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="e9da0-145">A conta e as credenciais são armazenadas em appSetting.</span><span class="sxs-lookup"><span data-stu-id="e9da0-145">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="e9da0-146">No Azure, você pode armazenar esses valores com segurança na guia **[Configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** na portal do Azure.</span><span class="sxs-lookup"><span data-stu-id="e9da0-146">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="e9da0-147">Consulte [as práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.net e no Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="e9da0-147">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>

### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="e9da0-148">Habilitar confirmação de email no controlador de conta</span><span class="sxs-lookup"><span data-stu-id="e9da0-148">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="e9da0-149">Verifique se o arquivo *Views\Account\ConfirmEmail.cshtml* tem a sintaxe correta do Razor.</span><span class="sxs-lookup"><span data-stu-id="e9da0-149">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="e9da0-150">(O caractere @ na primeira linha pode estar ausente.</span><span class="sxs-lookup"><span data-stu-id="e9da0-150">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="e9da0-151">)</span><span class="sxs-lookup"><span data-stu-id="e9da0-151">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="e9da0-152">Execute o aplicativo e clique no link registrar.</span><span class="sxs-lookup"><span data-stu-id="e9da0-152">Run the app and click the Register link.</span></span> <span data-ttu-id="e9da0-153">Depois de enviar o formulário de registro, você está conectado.</span><span class="sxs-lookup"><span data-stu-id="e9da0-153">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="e9da0-154">Verifique sua conta de email e clique no link para confirmar seu email.</span><span class="sxs-lookup"><span data-stu-id="e9da0-154">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="e9da0-155">Exigir confirmação de email antes de fazer logon</span><span class="sxs-lookup"><span data-stu-id="e9da0-155">Require email confirmation before log in</span></span>

<span data-ttu-id="e9da0-156">Atualmente, quando um usuário conclui o formulário de registro, ele está conectado.</span><span class="sxs-lookup"><span data-stu-id="e9da0-156">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="e9da0-157">Em geral, você deseja confirmar seu email antes de fazer logon.</span><span class="sxs-lookup"><span data-stu-id="e9da0-157">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="e9da0-158">Na seção abaixo, modificaremos o código para exigir que novos usuários tenham um email confirmado antes de serem conectados (autenticados).</span><span class="sxs-lookup"><span data-stu-id="e9da0-158">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="e9da0-159">Atualize o método `HttpPost Register` com as seguintes alterações realçadas:</span><span class="sxs-lookup"><span data-stu-id="e9da0-159">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="e9da0-160">Ao comentar o método `SignInAsync`, o usuário não será conectado pelo registro.</span><span class="sxs-lookup"><span data-stu-id="e9da0-160">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="e9da0-161">A linha de `TempData["ViewBagLink"] = callbackUrl;` pode ser usada para [depurar o aplicativo e o registro de](#dbg) teste sem enviar email.</span><span class="sxs-lookup"><span data-stu-id="e9da0-161">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="e9da0-162">`ViewBag.Message` é usado para exibir as instruções de confirmação.</span><span class="sxs-lookup"><span data-stu-id="e9da0-162">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="e9da0-163">O [exemplo de download](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contém código para testar a confirmação de email sem configurar o email e também pode ser usado para depurar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e9da0-163">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="e9da0-164">Crie um arquivo de `Views\Shared\Info.cshtml` e adicione a seguinte marcação Razor:</span><span class="sxs-lookup"><span data-stu-id="e9da0-164">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="e9da0-165">Adicione o [atributo Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) ao método de ação `Contact` do controlador Home.</span><span class="sxs-lookup"><span data-stu-id="e9da0-165">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="e9da0-166">Você pode clicar no link de **contato** para verificar se os usuários anônimos não têm acesso e os usuários autenticados têm o Access.</span><span class="sxs-lookup"><span data-stu-id="e9da0-166">You can click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="e9da0-167">Você também deve atualizar o método de ação `HttpPost Login`:</span><span class="sxs-lookup"><span data-stu-id="e9da0-167">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="e9da0-168">Atualize a exibição *Views\Shared\Error.cshtml* para exibir a mensagem de erro:</span><span class="sxs-lookup"><span data-stu-id="e9da0-168">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="e9da0-169">Exclua todas as contas na tabela **AspNetUsers** que contêm o alias de email com o qual você deseja testar.</span><span class="sxs-lookup"><span data-stu-id="e9da0-169">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="e9da0-170">Execute o aplicativo e verifique se não é possível fazer logon até que você tenha confirmado seu endereço de email.</span><span class="sxs-lookup"><span data-stu-id="e9da0-170">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="e9da0-171">Depois de confirmar seu endereço de email, clique no link de **contato** .</span><span class="sxs-lookup"><span data-stu-id="e9da0-171">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="e9da0-172">Recuperação/redefinição de senha</span><span class="sxs-lookup"><span data-stu-id="e9da0-172">Password recovery/reset</span></span>

<span data-ttu-id="e9da0-173">Remova os caracteres de comentário do método de ação `HttpPost ForgotPassword` no controlador de conta:</span><span class="sxs-lookup"><span data-stu-id="e9da0-173">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="e9da0-174">Remova os caracteres de comentário do `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) no arquivo de exibição do Razor do *Views\Account\Login.cshtml* :</span><span class="sxs-lookup"><span data-stu-id="e9da0-174">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="e9da0-175">Agora, a página de logon mostrará um link para redefinir a senha.</span><span class="sxs-lookup"><span data-stu-id="e9da0-175">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="e9da0-176">Link reenviar confirmação de email</span><span class="sxs-lookup"><span data-stu-id="e9da0-176">Resend email confirmation link</span></span>

<span data-ttu-id="e9da0-177">Quando um usuário cria uma nova conta local, ele envia por email um link de confirmação que eles precisam usar antes que possam fazer logon.</span><span class="sxs-lookup"><span data-stu-id="e9da0-177">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="e9da0-178">Se o usuário excluir acidentalmente o email de confirmação ou se o email nunca chegar, ele precisará do link de confirmação enviado novamente.</span><span class="sxs-lookup"><span data-stu-id="e9da0-178">If the user accidentally deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="e9da0-179">As alterações de código a seguir mostram como habilitar isso.</span><span class="sxs-lookup"><span data-stu-id="e9da0-179">The following code changes show how to enable this.</span></span>

<span data-ttu-id="e9da0-180">Adicione o seguinte método auxiliar à parte inferior do arquivo *Controllers\AccountController.cs* :</span><span class="sxs-lookup"><span data-stu-id="e9da0-180">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="e9da0-181">Atualize o método Register para usar o novo auxiliar:</span><span class="sxs-lookup"><span data-stu-id="e9da0-181">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="e9da0-182">Atualize o método de logon para reenviar a senha se a conta de usuário não tiver sido confirmada:</span><span class="sxs-lookup"><span data-stu-id="e9da0-182">Update the Login method to resend the password if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="e9da0-183">Combinar contas de logon sociais e locais</span><span class="sxs-lookup"><span data-stu-id="e9da0-183">Combine social and local login accounts</span></span>

<span data-ttu-id="e9da0-184">Você pode combinar contas locais e sociais clicando no link de email.</span><span class="sxs-lookup"><span data-stu-id="e9da0-184">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="e9da0-185">Na sequência a seguir **RickAndMSFT@gmail.com** é criado pela primeira vez como um logon local, mas você pode criar a conta como um log social primeiro e, em seguida, adicionar um logon local.</span><span class="sxs-lookup"><span data-stu-id="e9da0-185">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="e9da0-186">Clique no link **gerenciar** .</span><span class="sxs-lookup"><span data-stu-id="e9da0-186">Click on the **Manage** link.</span></span> <span data-ttu-id="e9da0-187">Observe os **logons externos: 0** associados a esta conta.</span><span class="sxs-lookup"><span data-stu-id="e9da0-187">Note the **External Logins: 0** associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="e9da0-188">Clique no link para outro serviço de logon e aceite as solicitações do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="e9da0-188">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="e9da0-189">As duas contas foram combinadas, você poderá fazer logon com qualquer uma das contas.</span><span class="sxs-lookup"><span data-stu-id="e9da0-189">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="e9da0-190">Talvez você queira que os usuários adicionem contas locais caso o serviço de autenticação de seu log social esteja inativo ou, provavelmente, tenha perdido o acesso à sua conta social.</span><span class="sxs-lookup"><span data-stu-id="e9da0-190">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="e9da0-191">Na imagem a seguir, José é um logon social (que pode ser visto nos **logons externos: 1** mostrado na página).</span><span class="sxs-lookup"><span data-stu-id="e9da0-191">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="e9da0-192">Clicar em **escolher uma senha** permite que você adicione um log local associado à mesma conta.</span><span class="sxs-lookup"><span data-stu-id="e9da0-192">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="e9da0-193">Confirmação de email em mais profundidade</span><span class="sxs-lookup"><span data-stu-id="e9da0-193">Email confirmation in more depth</span></span>

<span data-ttu-id="e9da0-194">Minha [confirmação de conta do tutorial e recuperação de senha com ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) entrará neste tópico com mais detalhes.</span><span class="sxs-lookup"><span data-stu-id="e9da0-194">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="e9da0-195">Depurando o aplicativo</span><span class="sxs-lookup"><span data-stu-id="e9da0-195">Debugging the app</span></span>

<span data-ttu-id="e9da0-196">Se você não receber um email contendo o link:</span><span class="sxs-lookup"><span data-stu-id="e9da0-196">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="e9da0-197">Verifique sua pasta de lixo eletrônico ou spam.</span><span class="sxs-lookup"><span data-stu-id="e9da0-197">Check your junk or spam folder.</span></span>
- <span data-ttu-id="e9da0-198">Faça logon em sua conta do SendGrid e clique no [link atividade de email](https://sendgrid.com/logs/index).</span><span class="sxs-lookup"><span data-stu-id="e9da0-198">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="e9da0-199">Para testar o link de verificação sem email, baixe o [exemplo concluído](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="e9da0-199">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="e9da0-200">O link de confirmação e os códigos de confirmação serão exibidos na página.</span><span class="sxs-lookup"><span data-stu-id="e9da0-200">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="e9da0-201">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="e9da0-201">Additional Resources</span></span>

- [<span data-ttu-id="e9da0-202">Links para ASP.NET Identity recursos recomendados</span><span class="sxs-lookup"><span data-stu-id="e9da0-202">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="e9da0-203">[Confirmação de conta e recuperação de senha com ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Apresenta mais detalhes sobre a recuperação de senha e a confirmação da conta.</span><span class="sxs-lookup"><span data-stu-id="e9da0-203">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="e9da0-204">[Aplicativo MVC 5 com logon do Facebook, Twitter, LinkedIn e Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Este tutorial mostra como escrever um aplicativo ASP.NET MVC 5 com a autorização do Facebook e do Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="e9da0-204">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="e9da0-205">Ele também mostra como adicionar dados adicionais ao banco de dado de identidade.</span><span class="sxs-lookup"><span data-stu-id="e9da0-205">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="e9da0-206">[Implante um aplicativo MVC do ASP.net seguro com associação, OAuth e banco de dados SQL no Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="e9da0-206">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="e9da0-207">Este tutorial adiciona a implantação do Azure, como proteger seu aplicativo com funções, como usar a API de associação para adicionar usuários e funções e recursos de segurança adicionais.</span><span class="sxs-lookup"><span data-stu-id="e9da0-207">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="e9da0-208">Criando um aplicativo do Google para OAuth 2 e conectando o aplicativo ao projeto</span><span class="sxs-lookup"><span data-stu-id="e9da0-208">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="e9da0-209">Criando o aplicativo no Facebook e conectando o aplicativo ao projeto</span><span class="sxs-lookup"><span data-stu-id="e9da0-209">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="e9da0-210">Configurando o SSL no projeto</span><span class="sxs-lookup"><span data-stu-id="e9da0-210">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
