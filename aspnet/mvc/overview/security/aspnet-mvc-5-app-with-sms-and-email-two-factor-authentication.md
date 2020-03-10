---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Aplicativo ASP.NET MVC 5 com SMS e email autenticação de dois fatores | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como criar um aplicativo Web ASP.NET MVC 5 com autenticação de dois fatores. Você deve concluir a criação de um aplicativo Web ASP.NET MVC 5 com...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: c14149d802bfc0a227a839a2981dc3e8a3849c25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538440"
---
# <a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="758b8-104">Aplicativo do ASP.NET MVC 5 com autenticação de dois fatores por SMS e email</span><span class="sxs-lookup"><span data-stu-id="758b8-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>

<span data-ttu-id="758b8-105">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="758b8-105">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

> <span data-ttu-id="758b8-106">Este tutorial mostra como criar um aplicativo Web ASP.NET MVC 5 com autenticação de dois fatores.</span><span class="sxs-lookup"><span data-stu-id="758b8-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="758b8-107">Você deve concluir [a criação de um aplicativo Web secure ASP.NET MVC 5 com logon, confirmação de email e redefinição de senha](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="758b8-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="758b8-108">Você pode baixar o aplicativo concluído [aqui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="758b8-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="758b8-109">O download contém auxiliares de depuração que permitem testar a confirmação de email e o SMS sem configurar um email ou provedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="758b8-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="758b8-110">Este tutorial foi escrito por [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="758b8-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>

- [<span data-ttu-id="758b8-111">Criar um aplicativo MVC do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="758b8-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="758b8-112">Configurar o SMS para autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="758b8-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="758b8-113">Habilitar a autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="758b8-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="758b8-114">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="758b8-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="758b8-115">Criar um aplicativo MVC do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="758b8-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="758b8-116">Comece Instalando e executando o [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="758b8-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="758b8-117">Instale [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior.</span><span class="sxs-lookup"><span data-stu-id="758b8-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="758b8-118">Aviso: você deve concluir [a criação de um aplicativo web ASP.NET MVC 5 com logon, confirmação de email e redefinição de senha](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de continuar.</span><span class="sxs-lookup"><span data-stu-id="758b8-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="758b8-119">Você deve instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior para concluir este tutorial.</span><span class="sxs-lookup"><span data-stu-id="758b8-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>

1. <span data-ttu-id="758b8-120">Crie um novo projeto Web ASP.NET e selecione o modelo MVC.</span><span class="sxs-lookup"><span data-stu-id="758b8-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="758b8-121">Web Forms também dá suporte a ASP.NET Identity, portanto, você pode seguir etapas semelhantes em um aplicativo Web Forms.</span><span class="sxs-lookup"><span data-stu-id="758b8-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="758b8-122">Deixe a autenticação padrão como **contas de usuário individuais**.</span><span class="sxs-lookup"><span data-stu-id="758b8-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="758b8-123">Se você quiser hospedar o aplicativo no Azure, deixe a caixa de seleção marcada.</span><span class="sxs-lookup"><span data-stu-id="758b8-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="758b8-124">Posteriormente, no tutorial, iremos implantar no Azure.</span><span class="sxs-lookup"><span data-stu-id="758b8-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="758b8-125">Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="758b8-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="758b8-126">Defina o [projeto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="758b8-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="758b8-127">Configurar o SMS para autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="758b8-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="758b8-128">Este tutorial fornece instruções para usar o twilio ou o ASPSMS, mas você pode usar qualquer outro provedor de SMS.</span><span class="sxs-lookup"><span data-stu-id="758b8-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="758b8-129">**Criando uma conta de usuário com um provedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="758b8-129">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="758b8-130">Crie uma conta do [twilio](https://www.twilio.com/try-twilio) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) .</span><span class="sxs-lookup"><span data-stu-id="758b8-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="758b8-131">**Instalando pacotes adicionais ou adicionando referências de serviço**</span><span class="sxs-lookup"><span data-stu-id="758b8-131">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="758b8-132">Twilio</span><span class="sxs-lookup"><span data-stu-id="758b8-132">Twilio:</span></span>  
   <span data-ttu-id="758b8-133">No Console do Gerenciador de Pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="758b8-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="758b8-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="758b8-134">ASPSMS:</span></span>  
   <span data-ttu-id="758b8-135">A seguinte referência de serviço precisa ser adicionada:</span><span class="sxs-lookup"><span data-stu-id="758b8-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="758b8-136">Endereço:</span><span class="sxs-lookup"><span data-stu-id="758b8-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="758b8-137">Namespace:</span><span class="sxs-lookup"><span data-stu-id="758b8-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="758b8-138">**Descobrindo credenciais de usuário do provedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="758b8-138">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="758b8-139">Twilio</span><span class="sxs-lookup"><span data-stu-id="758b8-139">Twilio:</span></span>  
   <span data-ttu-id="758b8-140">Na guia **painel** da sua conta do twilio, copie o **SID da conta** e o **token de autenticação**.</span><span class="sxs-lookup"><span data-stu-id="758b8-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="758b8-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="758b8-141">ASPSMS:</span></span>  
   <span data-ttu-id="758b8-142">Em suas configurações de conta, navegue até **userKey** e copie-o junto com sua **senha**definida automaticamente.</span><span class="sxs-lookup"><span data-stu-id="758b8-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="758b8-143">Mais tarde, armazenaremos esses valores no arquivo *Web. config* dentro das chaves `"SMSAccountIdentification"` e `"SMSAccountPassword"`.</span><span class="sxs-lookup"><span data-stu-id="758b8-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="758b8-144">**Especificando SenderId/originador**</span><span class="sxs-lookup"><span data-stu-id="758b8-144">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="758b8-145">Twilio</span><span class="sxs-lookup"><span data-stu-id="758b8-145">Twilio:</span></span>  
   <span data-ttu-id="758b8-146">Na guia **números** , copie o número de telefone twilio.</span><span class="sxs-lookup"><span data-stu-id="758b8-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="758b8-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="758b8-147">ASPSMS:</span></span>  
   <span data-ttu-id="758b8-148">No menu **desbloquear originadores** , desbloqueie um ou mais originadores ou escolha um originador alfanumérico (sem suporte em todas as redes).</span><span class="sxs-lookup"><span data-stu-id="758b8-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="758b8-149">Mais tarde, armazenaremos esse valor no arquivo *Web. config* dentro da chave `"SMSAccountFrom"`.</span><span class="sxs-lookup"><span data-stu-id="758b8-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="758b8-150">**Transferindo credenciais do provedor de SMS para o aplicativo**</span><span class="sxs-lookup"><span data-stu-id="758b8-150">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="758b8-151">Torne as credenciais e o número de telefone do remetente disponíveis para o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="758b8-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="758b8-152">Para manter as coisas simples, armazenaremos esses valores no arquivo *Web. config* .</span><span class="sxs-lookup"><span data-stu-id="758b8-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="758b8-153">Quando implantamos no Azure, podemos armazenar os valores com segurança na seção **configurações do aplicativo** na guia Configurar do site.</span><span class="sxs-lookup"><span data-stu-id="758b8-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="758b8-154">Segurança-nunca armazene dados confidenciais em seu código-fonte.</span><span class="sxs-lookup"><span data-stu-id="758b8-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="758b8-155">A conta e as credenciais são adicionadas ao código acima para manter o exemplo simples.</span><span class="sxs-lookup"><span data-stu-id="758b8-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="758b8-156">Consulte [as práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.net e no Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="758b8-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="758b8-157">**Implementação de transferência de dados para o provedor de SMS**</span><span class="sxs-lookup"><span data-stu-id="758b8-157">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="758b8-158">Configure a classe `SmsService` no arquivo de *\_Start\IdentityConfig.cs do aplicativo* .</span><span class="sxs-lookup"><span data-stu-id="758b8-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="758b8-159">Dependendo do provedor de SMS usado, ative a seção **twilio** ou **ASPSMS** :</span><span class="sxs-lookup"><span data-stu-id="758b8-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="758b8-160">Atualize a exibição do Razor do *Views\Manage\Index.cshtml* : (Observação: não apenas remova os comentários no código de saída, use o código abaixo.)</span><span class="sxs-lookup"><span data-stu-id="758b8-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="758b8-161">Verifique se os métodos de ação `EnableTwoFactorAuthentication` e `DisableTwoFactorAuthentication` no `ManageController` têm o atributo[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) :</span><span class="sxs-lookup"><span data-stu-id="758b8-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="758b8-162">Execute o aplicativo e faça logon com a conta que você registrou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="758b8-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="758b8-163">Clique em sua ID de usuário, que ativa o método de ação `Index` no controlador `Manage`.</span><span class="sxs-lookup"><span data-stu-id="758b8-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="758b8-164">Clique em Adicionar.</span><span class="sxs-lookup"><span data-stu-id="758b8-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="758b8-165">O método de ação `AddPhoneNumber` exibe uma caixa de diálogo para inserir um número de telefone que pode receber mensagens SMS.</span><span class="sxs-lookup"><span data-stu-id="758b8-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="758b8-166">Em alguns segundos, você receberá uma mensagem de texto com o código de verificação.</span><span class="sxs-lookup"><span data-stu-id="758b8-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="758b8-167">Insira-o e pressione **Enviar**.</span><span class="sxs-lookup"><span data-stu-id="758b8-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="758b8-168">O modo de exibição gerenciar mostra seu número de telefone foi adicionado.</span><span class="sxs-lookup"><span data-stu-id="758b8-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="758b8-169">Habilitar a autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="758b8-169">Enable two-factor authentication</span></span>

<span data-ttu-id="758b8-170">No aplicativo do modelo gerado, você precisa usar a interface do usuário para habilitar a autenticação de dois fatores (2FA).</span><span class="sxs-lookup"><span data-stu-id="758b8-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="758b8-171">Para habilitar o 2FA, clique em sua ID de usuário (alias de email) na barra de navegação.</span><span class="sxs-lookup"><span data-stu-id="758b8-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="758b8-172">Clique em Habilitar 2FA.</span><span class="sxs-lookup"><span data-stu-id="758b8-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="758b8-173">Faça logoff e logon novamente.</span><span class="sxs-lookup"><span data-stu-id="758b8-173">Logout, then log back in.</span></span> <span data-ttu-id="758b8-174">Se você habilitou o email (consulte meu [tutorial anterior](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), poderá selecionar o SMS ou email para 2FA.</span><span class="sxs-lookup"><span data-stu-id="758b8-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="758b8-175">A página verificar código é exibida onde você pode inserir o código (de SMS ou email).</span><span class="sxs-lookup"><span data-stu-id="758b8-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="758b8-176">Clicar na caixa de seleção **lembrar este navegador** lhe isentará de precisar usar o 2FA para fazer logon ao usar o navegador e o dispositivo em que você marcou a caixa.</span><span class="sxs-lookup"><span data-stu-id="758b8-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="758b8-177">Desde que os usuários mal-intencionados não possam obter acesso ao seu dispositivo, habilitar o 2FA e clicar no **Lembre-se de que este navegador** fornecerá a você o acesso de senha de uma etapa conveniente e, ao mesmo tempo, manterá uma proteção de 2FA forte para todo o acesso de dispositivos não confiáveis.</span><span class="sxs-lookup"><span data-stu-id="758b8-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="758b8-178">Você pode fazer isso em qualquer dispositivo privado que você usa regularmente.</span><span class="sxs-lookup"><span data-stu-id="758b8-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="758b8-179">Este tutorial fornece uma breve introdução à habilitação do 2FA em um novo aplicativo MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="758b8-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="758b8-180">Meu tutorial de [autenticação de dois fatores usando SMS e email com ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) entra em detalhes sobre o código por trás do exemplo.</span><span class="sxs-lookup"><span data-stu-id="758b8-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="758b8-181">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="758b8-181">Additional Resources</span></span>

- <span data-ttu-id="758b8-182">[Autenticação de dois fatores usando SMS e email com ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Entra em detalhes sobre a autenticação de dois fatores</span><span class="sxs-lookup"><span data-stu-id="758b8-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="758b8-183">Links para ASP.NET Identity recursos recomendados</span><span class="sxs-lookup"><span data-stu-id="758b8-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="758b8-184">[Confirmação de conta e recuperação de senha com ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Apresenta mais detalhes sobre a recuperação de senha e a confirmação da conta.</span><span class="sxs-lookup"><span data-stu-id="758b8-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="758b8-185">[Aplicativo MVC 5 com logon do Facebook, Twitter, LinkedIn e Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Este tutorial mostra como escrever um aplicativo ASP.NET MVC 5 com a autorização do Facebook e do Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="758b8-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="758b8-186">Ele também mostra como adicionar dados adicionais ao banco de dado de identidade.</span><span class="sxs-lookup"><span data-stu-id="758b8-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="758b8-187">[Implante um aplicativo MVC do ASP.net seguro com associação, OAuth e banco de dados SQL no Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="758b8-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="758b8-188">Este tutorial adiciona a implantação do Azure, como proteger seu aplicativo com funções, como usar a API de associação para adicionar usuários e funções e recursos de segurança adicionais.</span><span class="sxs-lookup"><span data-stu-id="758b8-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="758b8-189">Criando um aplicativo do Google para OAuth 2 e conectando o aplicativo ao projeto</span><span class="sxs-lookup"><span data-stu-id="758b8-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="758b8-190">Criando o aplicativo no Facebook e conectando o aplicativo ao projeto</span><span class="sxs-lookup"><span data-stu-id="758b8-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="758b8-191">Configurando o SSL no projeto</span><span class="sxs-lookup"><span data-stu-id="758b8-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [<span data-ttu-id="758b8-192">Como configurar seu e o C# ambiente de desenvolvimento do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="758b8-192">How to set up your C# and ASP.NET MVC development environment</span></span>](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)
