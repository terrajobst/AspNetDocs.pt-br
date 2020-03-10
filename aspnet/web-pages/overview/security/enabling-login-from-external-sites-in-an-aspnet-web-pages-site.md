---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Fazendo logon usando sites externos em um site Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este artigo explica como fazer logon em seu site do Páginas da Web do ASP.NET (Razor) usando o Facebook, o Google, o Twitter, o Yahoo e outros sites, ou seja, como dar suporte a...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 860b75422c3df1d191ed861344963bfc19270e8f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638750"
---
# <a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="4b175-103">Fazendo logon usando sites externos em um site Páginas da Web do ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="4b175-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="4b175-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4b175-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="4b175-105">Este artigo explica como fazer logon em seu site do Páginas da Web do ASP.NET (Razor) usando o Facebook, o Google, o Twitter, o Yahoo e outros sites, ou seja, como dar suporte ao OAuth e ao OpenID no seu site.</span><span class="sxs-lookup"><span data-stu-id="4b175-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="4b175-106">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="4b175-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="4b175-107">Como habilitar o logon de outros sites ao usar o modelo de site inicial do WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4b175-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="4b175-108">Esse é o recurso ASP.NET introduzido no artigo:</span><span class="sxs-lookup"><span data-stu-id="4b175-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="4b175-109">O auxiliar de `OAuthWebSecurity`.</span><span class="sxs-lookup"><span data-stu-id="4b175-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4b175-110">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="4b175-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="4b175-111">Páginas da Web do ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="4b175-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="4b175-112">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="4b175-112">WebMatrix 3</span></span>

<span data-ttu-id="4b175-113">O Páginas da Web do ASP.NET inclui suporte para provedores [OAuth](http://oauth.net/) e [OpenID](http://openid.net/) .</span><span class="sxs-lookup"><span data-stu-id="4b175-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="4b175-114">Usando esses provedores, você pode permitir que os usuários façam logon em seu site usando suas credenciais existentes do Facebook, Twitter, Microsoft e Google.</span><span class="sxs-lookup"><span data-stu-id="4b175-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="4b175-115">Por exemplo, para fazer logon usando uma conta do Facebook, os usuários podem simplesmente escolher um ícone do Facebook, que os redireciona para a página de logon do Facebook onde eles inserem suas informações de usuário.</span><span class="sxs-lookup"><span data-stu-id="4b175-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="4b175-116">Em seguida, eles podem associar o logon do Facebook à sua conta no seu site.</span><span class="sxs-lookup"><span data-stu-id="4b175-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="4b175-117">Um aprimoramento relacionado aos recursos de associação de páginas da Web é que os usuários podem associar vários logons (incluindo logons de sites de rede social) a uma única conta no seu site.</span><span class="sxs-lookup"><span data-stu-id="4b175-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="4b175-118">Esta imagem mostra a página de logon do modelo de **site inicial** , em que um usuário pode escolher um ícone do Facebook, Twitter, Google ou Microsoft para habilitar o logon com uma conta externa:</span><span class="sxs-lookup"><span data-stu-id="4b175-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![provedores externos](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="4b175-120">Você pode habilitar a associação OAuth e OpenID removendo comentários de algumas linhas de código no modelo de **site inicial** .</span><span class="sxs-lookup"><span data-stu-id="4b175-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="4b175-121">Os métodos e as propriedades que você usa para trabalhar com os provedores OAuth e OpenID estão na classe `WebMatrix.Security.OAuthWebSecurity`.</span><span class="sxs-lookup"><span data-stu-id="4b175-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="4b175-122">O modelo de **site inicial** inclui uma infraestrutura de associação completa, completa com uma página de logon, um banco de dados de associação e todo o código necessário para permitir que os usuários façam logon em seu site usando as credenciais locais ou as de outro site.</span><span class="sxs-lookup"><span data-stu-id="4b175-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="4b175-123">Esta seção fornece um exemplo de como permitir que os usuários façam logon de sites externos em um site baseado no modelo de **site inicial** .</span><span class="sxs-lookup"><span data-stu-id="4b175-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="4b175-124">Depois de criar um site inicial, você faz isso (detalhes a seguir):</span><span class="sxs-lookup"><span data-stu-id="4b175-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="4b175-125">Para os sites que usam um provedor OAuth (Facebook, Twitter e Microsoft), você cria um aplicativo no site externo.</span><span class="sxs-lookup"><span data-stu-id="4b175-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="4b175-126">Isso fornece as chaves de aplicativo que você precisará para invocar o recurso de logon para esses sites.</span><span class="sxs-lookup"><span data-stu-id="4b175-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="4b175-127">Para sites que usam um provedor de OpenID (Google), você não precisa criar um aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4b175-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="4b175-128">Para todos esses sites, você deve ter uma conta para fazer logon e criar aplicativos de desenvolvedor.</span><span class="sxs-lookup"><span data-stu-id="4b175-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="4b175-129">Os aplicativos da Microsoft aceitam apenas uma URL ativa para um site de trabalho, portanto, você não pode usar uma URL do site local para testar logons.</span><span class="sxs-lookup"><span data-stu-id="4b175-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="4b175-130">Edite alguns arquivos em seu site para especificar o provedor de autenticação apropriado e enviar um logon para o site que você deseja usar.</span><span class="sxs-lookup"><span data-stu-id="4b175-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="4b175-131">Este artigo fornece instruções separadas para as seguintes tarefas:</span><span class="sxs-lookup"><span data-stu-id="4b175-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="4b175-132">Habilitando logons do Google</span><span class="sxs-lookup"><span data-stu-id="4b175-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="4b175-133">Habilitando logons do Facebook</span><span class="sxs-lookup"><span data-stu-id="4b175-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="4b175-134">Habilitando logons do Twitter</span><span class="sxs-lookup"><span data-stu-id="4b175-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="4b175-135">Habilitando logons do Google</span><span class="sxs-lookup"><span data-stu-id="4b175-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="4b175-136">Crie ou abra um site Páginas da Web do ASP.NET com base no modelo de site inicial do WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4b175-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="4b175-137">Abra a página *\_AppStart. cshtml* e remova a marca de comentário da linha de código a seguir.</span><span class="sxs-lookup"><span data-stu-id="4b175-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="4b175-138">Testando o logon do Google</span><span class="sxs-lookup"><span data-stu-id="4b175-138">Testing Google login</span></span>

1. <span data-ttu-id="4b175-139">Execute a página *Default. cshtml* do seu site e escolha o botão **fazer logon** .</span><span class="sxs-lookup"><span data-stu-id="4b175-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="4b175-140">Na página de *logon* , na seção **usar outro serviço para fazer logon** , escolha o botão enviar do **Google** ou **Yahoo** .</span><span class="sxs-lookup"><span data-stu-id="4b175-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="4b175-141">Este exemplo usa o logon do Google.</span><span class="sxs-lookup"><span data-stu-id="4b175-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="4b175-142">A página da Web redireciona a solicitação para a página de logon do Google.</span><span class="sxs-lookup"><span data-stu-id="4b175-142">The web page redirects the request to the Google login page.</span></span>

    ![Entrar no Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="4b175-144">Insira as credenciais para uma conta existente do Google.</span><span class="sxs-lookup"><span data-stu-id="4b175-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="4b175-145">Se o Google perguntar se você deseja permitir que o *localhost* use as informações da conta, clique em **permitir**.</span><span class="sxs-lookup"><span data-stu-id="4b175-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="4b175-146">O código usa o token do Google para autenticar o usuário e, em seguida, retorna a esta página no seu site.</span><span class="sxs-lookup"><span data-stu-id="4b175-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="4b175-147">Esta página permite que os usuários associem seu logon do Google a uma conta existente no seu site ou possam registrar uma nova conta em seu site para associar o logon externo ao.</span><span class="sxs-lookup"><span data-stu-id="4b175-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="4b175-149">Escolha o botão **associar** .</span><span class="sxs-lookup"><span data-stu-id="4b175-149">Choose the **Associate** button.</span></span> <span data-ttu-id="4b175-150">O navegador retorna para o home page do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4b175-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="4b175-151">Habilitando logons do Facebook</span><span class="sxs-lookup"><span data-stu-id="4b175-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="4b175-152">Vá para o [site de desenvolvedores do Facebook](https://developers.facebook.com/apps) (faça logon se você ainda não estiver conectado).</span><span class="sxs-lookup"><span data-stu-id="4b175-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="4b175-153">Escolha o botão **criar novo aplicativo** e siga os prompts para nomear e criar o novo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4b175-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="4b175-154">Na seção, **Selecione como seu aplicativo será integrado ao Facebook**, escolha a seção **site** .</span><span class="sxs-lookup"><span data-stu-id="4b175-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="4b175-155">Preencha o campo **URL do site** com a URL do seu site (por exemplo, `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="4b175-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="4b175-156">O campo **domínio** é opcional; Você pode usar isso para fornecer autenticação para um domínio inteiro (como *example.com*).</span><span class="sxs-lookup"><span data-stu-id="4b175-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="4b175-157">Se você estiver executando um site em seu computador local com uma URL como `http://localhost:12345` (em que o número é um número de porta local), poderá adicionar esse valor ao campo **URL do site** para testar seu site.</span><span class="sxs-lookup"><span data-stu-id="4b175-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="4b175-158">No entanto, sempre que o número da porta do site local for alterado, você precisará atualizar o campo **URL do site** do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4b175-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="4b175-159">Escolha o botão **salvar alterações** .</span><span class="sxs-lookup"><span data-stu-id="4b175-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="4b175-160">Escolha a guia **aplicativos** novamente e, em seguida, exiba a página inicial do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4b175-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="4b175-161">Copie a **ID do aplicativo** e os valores de **segredo** do aplicativo para seu aplicativo e cole-os em um arquivo de texto temporário.</span><span class="sxs-lookup"><span data-stu-id="4b175-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="4b175-162">Você passará esses valores para o provedor do Facebook no código do seu site.</span><span class="sxs-lookup"><span data-stu-id="4b175-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="4b175-163">Saia do site do desenvolvedor do Facebook.</span><span class="sxs-lookup"><span data-stu-id="4b175-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="4b175-164">Agora você faz alterações em duas páginas em seu site para que os usuários possam fazer logon no site usando suas contas do Facebook.</span><span class="sxs-lookup"><span data-stu-id="4b175-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="4b175-165">Crie ou abra um site Páginas da Web do ASP.NET com base no modelo de site inicial do WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4b175-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="4b175-166">Abra a página *\_AppStart. cshtml* e remova o comentário do código para o provedor OAuth do Facebook.</span><span class="sxs-lookup"><span data-stu-id="4b175-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="4b175-167">O bloco de código não comentado é semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="4b175-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="4b175-168">Copie o valor da **ID** do aplicativo do aplicativo do Facebook como o valor do parâmetro `appId` (dentro das aspas).</span><span class="sxs-lookup"><span data-stu-id="4b175-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="4b175-169">Copie o valor **secreto do aplicativo** do aplicativo Facebook como o valor do parâmetro `appSecret`.</span><span class="sxs-lookup"><span data-stu-id="4b175-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="4b175-170">Salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="4b175-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="4b175-171">Testando o logon do Facebook</span><span class="sxs-lookup"><span data-stu-id="4b175-171">Testing Facebook login</span></span>

1. <span data-ttu-id="4b175-172">Execute a página *Default. cshtml* do site e escolha o botão **logon** .</span><span class="sxs-lookup"><span data-stu-id="4b175-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="4b175-173">Na página de *logon* , na seção **usar outro serviço para fazer logon** , escolha o ícone do **Facebook** .</span><span class="sxs-lookup"><span data-stu-id="4b175-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="4b175-174">A página da Web redireciona a solicitação para a página de logon do Facebook.</span><span class="sxs-lookup"><span data-stu-id="4b175-174">The web page redirects the request to the Facebook login page.</span></span>

    ![OAuth-2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="4b175-176">Faça logon em uma conta do Facebook.</span><span class="sxs-lookup"><span data-stu-id="4b175-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="4b175-177">O código usa o token do Facebook para autenticá-lo e, em seguida, retorna a uma página na qual você pode associar o logon do Facebook ao logon do site.</span><span class="sxs-lookup"><span data-stu-id="4b175-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="4b175-178">Seu nome de usuário ou endereço de email é preenchido no campo **email** do formulário.</span><span class="sxs-lookup"><span data-stu-id="4b175-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="4b175-180">Escolha o botão **associar** .</span><span class="sxs-lookup"><span data-stu-id="4b175-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="4b175-181">O navegador retorna ao home page e você está conectado.</span><span class="sxs-lookup"><span data-stu-id="4b175-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="4b175-182">Habilitando logons do Twitter</span><span class="sxs-lookup"><span data-stu-id="4b175-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="4b175-183">Navegue até o [site de desenvolvedores do Twitter](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="4b175-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="4b175-184">Escolha o link **criar um aplicativo** e faça logon no site.</span><span class="sxs-lookup"><span data-stu-id="4b175-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="4b175-185">No formulário **criar um aplicativo** , preencha os campos **nome** e **Descrição** .</span><span class="sxs-lookup"><span data-stu-id="4b175-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="4b175-186">No campo **site** , insira a URL do seu site (por exemplo, `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="4b175-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="4b175-187">Se você estiver testando seu site localmente (usando uma URL como `http://localhost:12345`), o Twitter poderá não aceitar a URL.</span><span class="sxs-lookup"><span data-stu-id="4b175-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="4b175-188">No entanto, talvez você possa usar o endereço IP de loopback local (por exemplo `http://127.0.0.1:12345`).</span><span class="sxs-lookup"><span data-stu-id="4b175-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="4b175-189">Isso simplifica o processo de teste do seu aplicativo localmente.</span><span class="sxs-lookup"><span data-stu-id="4b175-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="4b175-190">No entanto, sempre que o número da porta do site local for alterado, você precisará atualizar o campo do **site** do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="4b175-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="4b175-191">No campo **URL de retorno de chamada** , insira uma URL para a página no site que você deseja que os usuários retornem depois de fazer logon no Twitter.</span><span class="sxs-lookup"><span data-stu-id="4b175-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="4b175-192">Por exemplo, para enviar usuários para o home page do site inicial (que reconhecerá seu status de logon), insira a mesma URL que você inseriu no campo **site** .</span><span class="sxs-lookup"><span data-stu-id="4b175-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="4b175-193">Aceite os termos e escolha o botão **criar seu aplicativo do Twitter** .</span><span class="sxs-lookup"><span data-stu-id="4b175-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="4b175-194">Na página inicial **meus aplicativos** , escolha o aplicativo que você criou.</span><span class="sxs-lookup"><span data-stu-id="4b175-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="4b175-195">Na guia **detalhes** , role até a parte inferior e escolha o botão **criar meu token de acesso** .</span><span class="sxs-lookup"><span data-stu-id="4b175-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="4b175-196">Na guia **detalhes** , copie os valores de **chave do consumidor** e segredo do **consumidor** para seu aplicativo e cole-os em um arquivo de texto temporário.</span><span class="sxs-lookup"><span data-stu-id="4b175-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="4b175-197">Você passará esses valores para o provedor do Twitter no código do seu site.</span><span class="sxs-lookup"><span data-stu-id="4b175-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="4b175-198">Saia do site do Twitter.</span><span class="sxs-lookup"><span data-stu-id="4b175-198">Exit the Twitter site.</span></span>

<span data-ttu-id="4b175-199">Agora você faz alterações em duas páginas em seu site para que os usuários possam fazer logon no site usando suas contas do Twitter.</span><span class="sxs-lookup"><span data-stu-id="4b175-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="4b175-200">Crie ou abra um site Páginas da Web do ASP.NET com base no modelo de site inicial do WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="4b175-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="4b175-201">Abra a página *\_AppStart. cshtml* e remova o comentário do código do provedor OAuth do Twitter.</span><span class="sxs-lookup"><span data-stu-id="4b175-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="4b175-202">O bloco de código não comentado tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="4b175-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="4b175-203">Copie o valor de **chave do consumidor** do aplicativo do Twitter como o valor do parâmetro `consumerKey` (dentro das aspas).</span><span class="sxs-lookup"><span data-stu-id="4b175-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="4b175-204">Copie o valor do **segredo do consumidor** do aplicativo Twitter como o valor do parâmetro `consumerSecret`.</span><span class="sxs-lookup"><span data-stu-id="4b175-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="4b175-205">Salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="4b175-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="4b175-206">Testando o logon do Twitter</span><span class="sxs-lookup"><span data-stu-id="4b175-206">Testing Twitter login</span></span>

1. <span data-ttu-id="4b175-207">Execute a página *Default. cshtml* do seu site e escolha o botão **logon** .</span><span class="sxs-lookup"><span data-stu-id="4b175-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="4b175-208">Na página de *logon* , na seção **usar outro serviço para fazer logon** , escolha o ícone do **Twitter** .</span><span class="sxs-lookup"><span data-stu-id="4b175-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="4b175-209">A página da Web redireciona a solicitação para uma página de logon do Twitter para o aplicativo que você criou.</span><span class="sxs-lookup"><span data-stu-id="4b175-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="4b175-211">Faça logon em uma conta do Twitter.</span><span class="sxs-lookup"><span data-stu-id="4b175-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="4b175-212">O código usa o token do Twitter para autenticar o usuário e, em seguida, retorna a uma página onde você pode associar seu logon à sua conta de site.</span><span class="sxs-lookup"><span data-stu-id="4b175-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="4b175-213">Seu nome ou endereço de email é preenchido no campo **email** do formulário.</span><span class="sxs-lookup"><span data-stu-id="4b175-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="4b175-215">Escolha o botão **associar** .</span><span class="sxs-lookup"><span data-stu-id="4b175-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="4b175-216">O navegador retorna ao home page e você está conectado.</span><span class="sxs-lookup"><span data-stu-id="4b175-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="4b175-217">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="4b175-217">Additional Resources</span></span>

- [<span data-ttu-id="4b175-218">Personalizar o comportamento de todo o site</span><span class="sxs-lookup"><span data-stu-id="4b175-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="4b175-219">Adicionando segurança e associação a um site Páginas da Web do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4b175-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)
