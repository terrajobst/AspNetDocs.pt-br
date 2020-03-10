---
uid: web-api/overview/security/external-authentication-services
title: Serviços de autenticação externa com ASP.NET Web APIC#() | Microsoft Docs
author: rmcmurray
description: Descreve o uso de serviços de autenticação externa no ASP.NET Web API.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: b2571552a3f8040ff42bfa0a9fa48981f71a1e4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555471"
---
# <a name="external-authentication-services-with-aspnet-web-api-c"></a><span data-ttu-id="7aa7c-103">Serviços de autenticação externa com ASP.NET Web APIC#()</span><span class="sxs-lookup"><span data-stu-id="7aa7c-103">External Authentication Services with ASP.NET Web API (C#)</span></span>

<span data-ttu-id="7aa7c-104">O Visual Studio 2017 e o ASP.NET 4.7.2 expandem as opções de segurança para Spa ( [aplicativos de página única](../../../single-page-application/index.md) ) e serviços de [API da Web](../../index.md) para integrar com serviços de autenticação externa, que incluem vários serviços de autenticação OAuth/OpenID e de mídia social: contas da Microsoft, Twitter, Facebook e Google.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-104">Visual Studio 2017 and ASP.NET 4.7.2 expand the security options for [Single Page Applications](../../../single-page-application/index.md) (SPA) and [Web API](../../index.md) services to integrate with external authentication services, which include several OAuth/OpenID and social media authentication services: Microsoft Accounts, Twitter, Facebook, and Google.</span></span>  

### <a name="in-this-walkthrough"></a><span data-ttu-id="7aa7c-105">Neste tutorial</span><span class="sxs-lookup"><span data-stu-id="7aa7c-105">In this Walkthrough</span></span>

- [<span data-ttu-id="7aa7c-106">Usando serviços de autenticação externa</span><span class="sxs-lookup"><span data-stu-id="7aa7c-106">Using External Authentication Services</span></span>](#USING)
- [<span data-ttu-id="7aa7c-107">Criando o aplicativo Web de exemplo</span><span class="sxs-lookup"><span data-stu-id="7aa7c-107">Creating the Sample Web Application</span></span>](#SAMPLE)
- [<span data-ttu-id="7aa7c-108">Habilitando a autenticação do Facebook</span><span class="sxs-lookup"><span data-stu-id="7aa7c-108">Enabling Facebook Authentication</span></span>](#FACEBOOK)
- [<span data-ttu-id="7aa7c-109">Habilitando a autenticação do Google</span><span class="sxs-lookup"><span data-stu-id="7aa7c-109">Enabling Google Authentication</span></span>](#GOOGLE)
- [<span data-ttu-id="7aa7c-110">Habilitando a autenticação da Microsoft</span><span class="sxs-lookup"><span data-stu-id="7aa7c-110">Enabling Microsoft Authentication</span></span>](#MICROSOFT)
- [<span data-ttu-id="7aa7c-111">Habilitando a autenticação do Twitter</span><span class="sxs-lookup"><span data-stu-id="7aa7c-111">Enabling Twitter Authentication</span></span>](#TWITTER)
- [<span data-ttu-id="7aa7c-112">Informações adicionais</span><span class="sxs-lookup"><span data-stu-id="7aa7c-112">Additional Information</span></span>](#MOREINFO)

    - [<span data-ttu-id="7aa7c-113">Combinando serviços de autenticação externa</span><span class="sxs-lookup"><span data-stu-id="7aa7c-113">Combining External Authentication Services</span></span>](#COMBINE)
    - [<span data-ttu-id="7aa7c-114">Configurando IIS Express para usar um nome de domínio totalmente qualificado</span><span class="sxs-lookup"><span data-stu-id="7aa7c-114">Configuring IIS Express to use a Fully Qualified Domain Name</span></span>](#FQDN)
    - [<span data-ttu-id="7aa7c-115">Como obter as configurações do aplicativo para autenticação da Microsoft</span><span class="sxs-lookup"><span data-stu-id="7aa7c-115">How to Obtain your Application Settings for Microsoft Authentication</span></span>](#OBTAIN)
    - [<span data-ttu-id="7aa7c-116">Opcional: desabilitar o registro local</span><span class="sxs-lookup"><span data-stu-id="7aa7c-116">Optional: Disable Local Registration</span></span>](#DISABLE)

### <a name="prerequisites"></a><span data-ttu-id="7aa7c-117">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="7aa7c-117">Prerequisites</span></span>

<span data-ttu-id="7aa7c-118">Para seguir os exemplos neste passo a passos, você precisa ter o seguinte:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-118">To follow the examples in this walkthrough, you need to have the following:</span></span>

- <span data-ttu-id="7aa7c-119">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="7aa7c-119">Visual Studio 2017</span></span>
- <span data-ttu-id="7aa7c-120">Uma conta de desenvolvedor com o identificador de aplicativo e a chave secreta para um dos seguintes serviços de autenticação de mídia social:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-120">A developer account with the application identifier and secret key for one of the following social media authentication services:</span></span>

  - <span data-ttu-id="7aa7c-121">Contas da Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span><span class="sxs-lookup"><span data-stu-id="7aa7c-121">Microsoft Accounts ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span></span>
  - <span data-ttu-id="7aa7c-122">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span><span class="sxs-lookup"><span data-stu-id="7aa7c-122">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span></span>
  - <span data-ttu-id="7aa7c-123">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span><span class="sxs-lookup"><span data-stu-id="7aa7c-123">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span></span>
  - <span data-ttu-id="7aa7c-124">Google ([https://developers.google.com/](https://developers.google.com))</span><span class="sxs-lookup"><span data-stu-id="7aa7c-124">Google ([https://developers.google.com/](https://developers.google.com))</span></span>

<a id="USING"></a>
## <a name="using-external-authentication-services"></a><span data-ttu-id="7aa7c-125">Usando serviços de autenticação externa</span><span class="sxs-lookup"><span data-stu-id="7aa7c-125">Using External Authentication Services</span></span>

<span data-ttu-id="7aa7c-126">A abundância de serviços de autenticação externa que estão disponíveis atualmente para desenvolvedores da Web ajudam a reduzir o tempo de desenvolvimento ao criar novos aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-126">The abundance of external authentication services that are currently available to web developers help to reduce development time when creating new web applications.</span></span> <span data-ttu-id="7aa7c-127">Normalmente, os usuários da Web têm várias contas existentes para sites populares de serviços da Web e de mídia social. portanto, quando um aplicativo Web implementa os serviços de autenticação de um site de mídia social ou de um serviço Web externo, ele economiza o tempo de desenvolvimento que teria sido gasto criando uma implementação de autenticação.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-127">Web users typically have several existing accounts for popular web services and social media websites, therefore when a web application implements the authentication services from an external web service or social media website, it saves the development time that would have been spent creating an authentication implementation.</span></span> <span data-ttu-id="7aa7c-128">O uso de um serviço de autenticação externo evita que os usuários finais precisem criar outra conta para seu aplicativo Web e também não precisem se lembrar de outro nome de usuário e senha.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-128">Using an external authentication service saves end users from having to create another account for your web application, and also from having to remember another username and password.</span></span>

<span data-ttu-id="7aa7c-129">No passado, os desenvolvedores tinham duas opções: criar sua própria implementação de autenticação ou saber como integrar um serviço de autenticação externa em seus aplicativos.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-129">In the past, developers have had two choices: create their own authentication implementation, or learn how to integrate an external authentication service into their applications.</span></span> <span data-ttu-id="7aa7c-130">No nível mais básico, o diagrama a seguir ilustra um fluxo de solicitação simples para um agente do usuário (navegador da Web) que está solicitando informações de um aplicativo Web configurado para usar um serviço de autenticação externo:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-130">At the most basic level, the following diagram illustrates a simple request flow for a user agent (web browser) that is requesting information from a web application that is configured to use an external authentication service:</span></span>

[![](external-authentication-services/_static/image2.png "Click to Expand the Image")](external-authentication-services/_static/image1.png)

<span data-ttu-id="7aa7c-131">No diagrama anterior, o agente do usuário (ou navegador da Web neste exemplo) faz uma solicitação para um aplicativo Web, que redireciona o navegador da Web para um serviço de autenticação externa.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-131">In the preceding diagram, the user agent (or web browser in this example) makes a request to a web application, which redirects the web browser to an external authentication service.</span></span> <span data-ttu-id="7aa7c-132">O agente do usuário envia suas credenciais para o serviço de autenticação externa e, se o agente do usuário tiver sido autenticado com êxito, o serviço de autenticação externa redirecionará o agente do usuário para o aplicativo Web original com alguma forma de token que o o agente do usuário será enviado para o aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-132">The user agent sends its credentials to the external authentication service, and if the user agent has successfully authenticated, the external authentication service will redirect the user agent to the original web application with some form of token which the user agent will send to the web application.</span></span> <span data-ttu-id="7aa7c-133">O aplicativo Web usará o token para verificar se o agente do usuário foi autenticado com êxito pelo serviço de autenticação externa e se o aplicativo Web pode usar o token para coletar mais informações sobre o agente do usuário.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-133">The web application will use the token to verify that the user agent has been successfully authenticated by the external authentication service, and the web application may use the token to gather more information about the user agent.</span></span> <span data-ttu-id="7aa7c-134">Quando o aplicativo terminar de processar as informações do agente do usuário, o aplicativo Web retornará a resposta apropriada ao agente do usuário com base em suas configurações de autorização.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-134">Once the application is done processing the user agent's information, the web application will return the appropriate response to the user agent based on its authorization settings.</span></span>

<span data-ttu-id="7aa7c-135">Neste segundo exemplo, o agente do usuário negocia com o aplicativo Web e o servidor de autorização externo, e o aplicativo Web executa comunicação adicional com o servidor de autorização externo para recuperar informações adicionais sobre o usuário Agente</span><span class="sxs-lookup"><span data-stu-id="7aa7c-135">In this second example, the user agent negotiates with the web application and external authorization server, and the web application performs additional communication with the external authorization server to retrieve additional information about the user agent:</span></span>

[![](external-authentication-services/_static/image4.png "Click to Expand the Image")](external-authentication-services/_static/image3.png)

<span data-ttu-id="7aa7c-136">O Visual Studio 2017 e o ASP.NET 4.7.2 tornam a integração com os serviços de autenticação externa mais fácil para os desenvolvedores, fornecendo integração interna para os seguintes serviços de autenticação:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-136">Visual Studio 2017 and ASP.NET 4.7.2 make integration with external authentication services easier for developers by providing built-in integration for the following authentication services:</span></span>

- <span data-ttu-id="7aa7c-137">Facebook</span><span class="sxs-lookup"><span data-stu-id="7aa7c-137">Facebook</span></span>
- <span data-ttu-id="7aa7c-138">Google</span><span class="sxs-lookup"><span data-stu-id="7aa7c-138">Google</span></span>
- <span data-ttu-id="7aa7c-139">Contas da Microsoft (contas do Windows Live ID)</span><span class="sxs-lookup"><span data-stu-id="7aa7c-139">Microsoft Accounts (Windows Live ID accounts)</span></span>
- <span data-ttu-id="7aa7c-140">Twitter</span><span class="sxs-lookup"><span data-stu-id="7aa7c-140">Twitter</span></span>

<span data-ttu-id="7aa7c-141">Os exemplos neste tutorial demonstrarão como configurar cada um dos serviços de autenticação externa com suporte usando o novo modelo de aplicativo Web ASP.NET que é fornecido com o Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-141">The examples in this walkthrough will demonstrate how to configure each of the supported external authentication services by using the new ASP.NET Web Application template that ships with Visual Studio 2017.</span></span>

> [!NOTE]
> <span data-ttu-id="7aa7c-142">Se necessário, talvez seja necessário adicionar seu FQDN às configurações para o serviço de autenticação externa.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-142">If necessary, you may need to add your FQDN to the settings for your external authentication service.</span></span> <span data-ttu-id="7aa7c-143">Esse requisito é baseado em restrições de segurança para alguns serviços de autenticação externos que exigem o FQDN em suas configurações de aplicativo para corresponder ao FQDN usado por seus clientes.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-143">This requirement is based on security constraints for some external authentication services which require the FQDN in your application settings to match the FQDN that is used by your clients.</span></span> <span data-ttu-id="7aa7c-144">(As etapas para isso variam muito para cada serviço de autenticação externo; você precisará consultar a documentação de cada serviço de autenticação externo para ver se isso é necessário e como definir essas configurações.) Se você precisar configurar IIS Express para usar um FQDN para testar esse ambiente, consulte a seção [Configurando IIS Express para usar um nome de domínio totalmente qualificado](#FQDN) , mais adiante neste guia.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-144">(The steps for this will vary greatly for each external authentication service; you will need to consult the documentation for each external authentication service to see if this is required and how to configure these settings.) If you need to configure IIS Express to use an FQDN for testing this environment, see the [Configuring IIS Express to use a Fully Qualified Domain Name](#FQDN) section later in this walkthrough.</span></span>

<a id="SAMPLE"></a>
## <a name="create-a-sample-web-application"></a><span data-ttu-id="7aa7c-145">Criar um aplicativo Web de exemplo</span><span class="sxs-lookup"><span data-stu-id="7aa7c-145">Create a Sample Web Application</span></span>

<span data-ttu-id="7aa7c-146">As etapas a seguir irão orientá-lo na criação de um aplicativo de exemplo usando o modelo de aplicativo Web ASP.NET, e você usará esse aplicativo de exemplo para cada um dos serviços de autenticação externa posteriormente neste guia.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-146">The following steps will lead you through creating a sample application by using the ASP.NET Web Application template, and you will use this sample application for each of the external authentication services later in this walkthrough.</span></span>

<span data-ttu-id="7aa7c-147">Inicie o Visual Studio 2017 e selecione **novo projeto** na página inicial.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-147">Start Visual Studio 2017 and select **New Project** from the Start page.</span></span> <span data-ttu-id="7aa7c-148">Ou, no menu **arquivo** , selecione **novo** e **projeto**.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-148">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<!-- [![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png) -->

<span data-ttu-id="7aa7c-149">Quando a caixa de diálogo **novo projeto** for exibida, selecione **instalado** e expanda **Visual C#** .</span><span class="sxs-lookup"><span data-stu-id="7aa7c-149">When the **New Project** dialog box is displayed, select **Installed** and expand **Visual C#**.</span></span> <span data-ttu-id="7aa7c-150">Em **Visual C#** , selecione **Web**.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-150">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="7aa7c-151">Na lista de modelos de projeto, selecione **aplicativo Web ASP.net (.NET Framework)** .</span><span class="sxs-lookup"><span data-stu-id="7aa7c-151">In the list of project templates, select **ASP.NET Web Application (.Net Framework)**.</span></span> <span data-ttu-id="7aa7c-152">Insira um nome para seu projeto e clique em **OK**.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-152">Enter a name for your project and click **OK**.</span></span>

[![](external-authentication-services/_static/image71.png "Click to Expand the Image")](external-authentication-services/_static/image71.png)

<span data-ttu-id="7aa7c-153">Quando o **novo projeto ASP.net** for exibido, selecione o modelo **aplicativo de página única** e clique em **criar projeto**.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-153">When the **New ASP.NET Project** is displayed, select the **Single Page Application** template and click **Create Project**.</span></span>

[![](external-authentication-services/_static/image72.png "Click to Expand the Image")](external-authentication-services/_static/image72.png)

<span data-ttu-id="7aa7c-154">Aguarde, pois o Visual Studio 2017 cria seu projeto.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-154">Wait as Visual Studio 2017 creates your project.</span></span>

<!-- [![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png) -->

<span data-ttu-id="7aa7c-155">Quando o Visual Studio 2017 terminar de criar seu projeto, abra o arquivo *Startup.auth.cs* localizado na pasta **iniciar do aplicativo\_** .</span><span class="sxs-lookup"><span data-stu-id="7aa7c-155">When Visual Studio 2017 has finished creating your project, open the *Startup.Auth.cs* file that is located in the **App\_Start** folder.</span></span>

<span data-ttu-id="7aa7c-156">Quando você cria o projeto pela primeira vez, nenhum dos serviços de autenticação externa é habilitado no arquivo *Startup.auth.cs* ; o seguinte ilustra o que seu código pode parecer, com as seções realçadas para onde você habilitaria um serviço de autenticação externa e quaisquer configurações relevantes para usar as contas da Microsoft, o Twitter, o Facebook ou a autenticação do Google com seu aplicativo ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-156">When you first create the project, none of the external authentication services are enabled in *Startup.Auth.cs* file; the following illustrates what your code might resemble, with the sections highlighted for where you would enable an external authentication service and any relevant settings in order to use Microsoft Accounts, Twitter, Facebook, or Google authentication with your ASP.NET application:</span></span>

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

<span data-ttu-id="7aa7c-157">Quando você pressiona F5 para compilar e depurar seu aplicativo Web, ele exibirá uma tela de logon onde você verá que nenhum serviço de autenticação externa foi definido.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-157">When you press F5 to build and debug your web application, it will display a login screen where you will see that no external authentication services have been defined.</span></span>

[![](external-authentication-services/_static/image73.png "Click to Expand the Image")](external-authentication-services/_static/image73.png)

<span data-ttu-id="7aa7c-158">Nas seções a seguir, você aprenderá a habilitar cada um dos serviços de autenticação externa fornecidos com o ASP.NET no Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-158">In the following sections, you will learn how to enable each of the external authentication services that are provided with ASP.NET in Visual Studio 2017.</span></span>

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a><span data-ttu-id="7aa7c-159">Habilitando a autenticação do Facebook</span><span class="sxs-lookup"><span data-stu-id="7aa7c-159">Enabling Facebook authentication</span></span>

<span data-ttu-id="7aa7c-160">Usar a autenticação do Facebook exige que você crie uma conta de desenvolvedor do Facebook e seu projeto exigirá uma ID do aplicativo e uma chave secreta do Facebook para funcionar.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-160">Using Facebook authentication requires you to create a Facebook developer account, and your project will require an application ID and secret key from Facebook in order to function.</span></span> <span data-ttu-id="7aa7c-161">Para obter informações sobre como criar uma conta de desenvolvedor do Facebook e como obter a ID do aplicativo e a chave secreta, consulte [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span><span class="sxs-lookup"><span data-stu-id="7aa7c-161">For information about creating a Facebook developer account and obtaining your application ID and secret key, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="7aa7c-162">Depois de obter a ID do aplicativo e a chave secreta, use as seguintes etapas para habilitar a autenticação do Facebook para seu aplicativo Web:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-162">Once you have obtained your application ID and secret key, use the following steps to enable Facebook authentication for your web application:</span></span>

1. <span data-ttu-id="7aa7c-163">Quando o projeto estiver aberto no Visual Studio 2017, abra o arquivo *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="7aa7c-163">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="7aa7c-164">Localize a seção de autenticação do Facebook do código:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-164">Locate the Facebook authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. <span data-ttu-id="7aa7c-165">Remova o &quot;//&quot; caracteres para remover os comentários das linhas de código realçadas e, em seguida, adicione a ID do aplicativo e a chave secreta.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-165">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="7aa7c-166">Depois de adicionar esses parâmetros, você poderá Recompilar o projeto:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-166">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. <span data-ttu-id="7aa7c-167">Ao pressionar F5 para abrir seu aplicativo Web no navegador da Web, você verá que o Facebook foi definido como um serviço de autenticação externo:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-167">When you press F5 to open your web application in your web browser, you will see that Facebook has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image74.png "Click to Expand the Image")](external-authentication-services/_static/image74.png)
5. <span data-ttu-id="7aa7c-168">Quando você clicar no botão do **Facebook** , seu navegador será redirecionado para a página de logon do Facebook:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-168">When you click the **Facebook** button, your browser will be redirected to the Facebook login page:</span></span>

    [![](external-authentication-services/_static/image22.png "Click to Expand the Image")](external-authentication-services/_static/image21.png)
6. <span data-ttu-id="7aa7c-169">Depois de inserir suas credenciais do Facebook e clicar em **fazer logon**, seu navegador da Web será Redirecionado de volta para seu aplicativo Web, o que lhe solicitará o **nome de usuário** que você deseja associar à sua conta do Facebook:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-169">After you enter your Facebook credentials and click **Log in**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Facebook account:</span></span>

    [![](external-authentication-services/_static/image24.png "Click to Expand the Image")](external-authentication-services/_static/image23.png)
7. <span data-ttu-id="7aa7c-170">Depois de inserir seu nome de usuário e clicar no botão **inscrever-se** , seu aplicativo Web exibirá o **Home Page** padrão para sua conta do Facebook:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-170">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Facebook account:</span></span>

    [![](external-authentication-services/_static/image26.png "Click to Expand the Image")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a><span data-ttu-id="7aa7c-171">Habilitando a autenticação do Google</span><span class="sxs-lookup"><span data-stu-id="7aa7c-171">Enabling Google Authentication</span></span>

<span data-ttu-id="7aa7c-172">Usar a autenticação do Google exige que você crie uma conta de desenvolvedor do Google e seu projeto exigirá uma ID do aplicativo e uma chave secreta do Google para funcionar.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-172">Using Google authentication requires you to create a Google developer account, and your project will require an application ID and secret key from Google in order to function.</span></span> <span data-ttu-id="7aa7c-173">Para obter informações sobre como criar uma conta de desenvolvedor do Google e obter a ID do aplicativo e a chave secreta, consulte [https://developers.google.com](https://developers.google.com).</span><span class="sxs-lookup"><span data-stu-id="7aa7c-173">For information about creating a Google developer account and obtaining your application ID and secret key, see [https://developers.google.com](https://developers.google.com).</span></span>

<span data-ttu-id="7aa7c-174">Para habilitar a autenticação do Google para seu aplicativo Web, use as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-174">To enable Google authentication for your web application, use the following steps:</span></span>

1. <span data-ttu-id="7aa7c-175">Quando o projeto estiver aberto no Visual Studio 2017, abra o arquivo *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="7aa7c-175">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="7aa7c-176">Localize a seção de autenticação do Google do código:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-176">Locate the Google authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. <span data-ttu-id="7aa7c-177">Remova o &quot;//&quot; caracteres para remover os comentários das linhas de código realçadas e, em seguida, adicione a ID do aplicativo e a chave secreta.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-177">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="7aa7c-178">Depois de adicionar esses parâmetros, você poderá Recompilar o projeto:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-178">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. <span data-ttu-id="7aa7c-179">Quando você pressionar F5 para abrir seu aplicativo Web em seu navegador da Web, verá que o Google foi definido como um serviço de autenticação externo:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-179">When you press F5 to open your web application in your web browser, you will see that Google has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image75.png "Click to Expand the Image")](external-authentication-services/_static/image75.png)
5. <span data-ttu-id="7aa7c-180">Quando você clicar no botão **Google** , seu navegador será redirecionado para a página de logon do Google:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-180">When you click the **Google** button, your browser will be redirected to the Google login page:</span></span>

    [![](external-authentication-services/_static/image32.png "Click to Expand the Image")](external-authentication-services/_static/image31.png)
6. <span data-ttu-id="7aa7c-181">Depois de inserir suas credenciais do Google e clicar em **entrar**, o Google solicitará que você verifique se seu aplicativo Web tem permissões para acessar sua conta do Google:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-181">After you enter your Google credentials and click **Sign in**, Google will prompt you to verify that your web application has permissions to access your Google account:</span></span>

    [![](external-authentication-services/_static/image34.png "Click to Expand the Image")](external-authentication-services/_static/image33.png)
7. <span data-ttu-id="7aa7c-182">Quando você clicar em **aceitar**, seu navegador da Web será Redirecionado de volta para seu aplicativo Web, que solicitará o **nome de usuário** que você deseja associar à sua conta do Google:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-182">When you click **Accept**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Google account:</span></span>

    [![](external-authentication-services/_static/image36.png "Click to Expand the Image")](external-authentication-services/_static/image35.png)
8. <span data-ttu-id="7aa7c-183">Depois de inserir seu nome de usuário e clicar no botão **inscrever-se** , seu aplicativo Web exibirá o **Home Page** padrão para sua conta do Google:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-183">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Google account:</span></span>

    [![](external-authentication-services/_static/image38.png "Click to Expand the Image")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a><span data-ttu-id="7aa7c-184">Habilitando a autenticação da Microsoft</span><span class="sxs-lookup"><span data-stu-id="7aa7c-184">Enabling Microsoft Authentication</span></span>

<span data-ttu-id="7aa7c-185">A autenticação da Microsoft exige que você crie uma conta de desenvolvedor e exija uma ID do cliente e um segredo do cliente para funcionar.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-185">Microsoft authentication requires you to create a developer account, and it requires a client ID and client secret in order to function.</span></span> <span data-ttu-id="7aa7c-186">Para obter informações sobre como criar uma conta de desenvolvedor da Microsoft e como obter a ID do cliente e o segredo do cliente, consulte [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).</span><span class="sxs-lookup"><span data-stu-id="7aa7c-186">For information about creating a Microsoft developer account and obtaining your client ID and client secret, see [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).</span></span>

<span data-ttu-id="7aa7c-187">Depois de obter a chave do consumidor e o segredo do consumidor, use as seguintes etapas para habilitar a autenticação da Microsoft para seu aplicativo Web:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-187">Once you have obtained your consumer key and consumer secret, use the following steps to enable Microsoft authentication for your web application:</span></span>

1. <span data-ttu-id="7aa7c-188">Quando o projeto estiver aberto no Visual Studio 2017, abra o arquivo *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="7aa7c-188">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="7aa7c-189">Localize a seção Microsoft Authentication do código:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-189">Locate the Microsoft authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. <span data-ttu-id="7aa7c-190">Remova o &quot;//&quot; caracteres para remover os comentários das linhas de código realçadas e, em seguida, adicione a ID do cliente e o segredo do cliente.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-190">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your client ID and client secret.</span></span> <span data-ttu-id="7aa7c-191">Depois de adicionar esses parâmetros, você poderá Recompilar o projeto:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-191">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. <span data-ttu-id="7aa7c-192">Ao pressionar F5 para abrir seu aplicativo Web no navegador da Web, você verá que a Microsoft foi definida como um serviço de autenticação externo:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-192">When you press F5 to open your web application in your web browser, you will see that Microsoft has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image42.png "Click to Expand the Image")](external-authentication-services/_static/image41.png)
5. <span data-ttu-id="7aa7c-193">Quando você clicar no botão **Microsoft** , seu navegador será redirecionado para a página de logon da Microsoft:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-193">When you click the **Microsoft** button, your browser will be redirected to the Microsoft login page:</span></span>

    [![](external-authentication-services/_static/image44.png "Click to Expand the Image")](external-authentication-services/_static/image43.png)
6. <span data-ttu-id="7aa7c-194">Depois de inserir suas credenciais da Microsoft e clicar em **entrar**, você será solicitado a verificar se o seu aplicativo Web tem permissões para acessar seu conta Microsoft:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-194">After you enter your Microsoft credentials and click **Sign in**, you will be prompted to verify that your web application has permissions to access your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image46.png "Click to Expand the Image")](external-authentication-services/_static/image45.png)
7. <span data-ttu-id="7aa7c-195">Quando você clicar em **Sim**, seu navegador da Web será Redirecionado de volta para seu aplicativo Web, o que solicitará o **nome de usuário** que você deseja associar ao seu conta Microsoft:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-195">When you click **Yes**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image48.png "Click to Expand the Image")](external-authentication-services/_static/image47.png)
8. <span data-ttu-id="7aa7c-196">Depois de inserir seu nome de usuário e clicar no botão **inscrever-se** , seu aplicativo Web exibirá o **Home Page** padrão para seu conta Microsoft:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-196">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Microsoft account:</span></span>

    [![](external-authentication-services/_static/image50.png "Click to Expand the Image")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a><span data-ttu-id="7aa7c-197">Habilitando a autenticação do Twitter</span><span class="sxs-lookup"><span data-stu-id="7aa7c-197">Enabling Twitter Authentication</span></span>

<span data-ttu-id="7aa7c-198">A autenticação do Twitter exige que você crie uma conta de desenvolvedor e requer uma chave do consumidor e um segredo do consumidor para funcionar.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-198">Twitter authentication requires you to create a developer account, and it requires a consumer key and consumer secret in order to function.</span></span> <span data-ttu-id="7aa7c-199">Para obter informações sobre como criar uma conta de desenvolvedor do Twitter e como obter a chave do consumidor e o segredo do consumidor, consulte [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span><span class="sxs-lookup"><span data-stu-id="7aa7c-199">For information about creating a Twitter developer account and obtaining your consumer key and consumer secret, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="7aa7c-200">Depois de obter a chave do consumidor e o segredo do consumidor, use as seguintes etapas para habilitar a autenticação do Twitter para seu aplicativo Web:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-200">Once you have obtained your consumer key and consumer secret, use the following steps to enable Twitter authentication for your web application:</span></span>

1. <span data-ttu-id="7aa7c-201">Quando o projeto estiver aberto no Visual Studio 2017, abra o arquivo *Startup.auth.cs* .</span><span class="sxs-lookup"><span data-stu-id="7aa7c-201">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="7aa7c-202">Localize a seção de autenticação do Twitter do código:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-202">Locate the Twitter authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. <span data-ttu-id="7aa7c-203">Remova o &quot;//&quot; caracteres para remover os comentários das linhas de código realçadas e, em seguida, adicione a chave do consumidor e o segredo do consumidor.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-203">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your consumer key and consumer secret.</span></span> <span data-ttu-id="7aa7c-204">Depois de adicionar esses parâmetros, você poderá Recompilar o projeto:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-204">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. <span data-ttu-id="7aa7c-205">Ao pressionar F5 para abrir seu aplicativo Web no navegador da Web, você verá que o Twitter foi definido como um serviço de autenticação externo:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-205">When you press F5 to open your web application in your web browser, you will see that Twitter has been defined as an external authentication service:</span></span>

    [![](external-authentication-services/_static/image54.png "Click to Expand the Image")](external-authentication-services/_static/image53.png)
5. <span data-ttu-id="7aa7c-206">Quando você clicar no botão do **Twitter** , seu navegador será redirecionado para a página de logon do Twitter:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-206">When you click the **Twitter** button, your browser will be redirected to the Twitter login page:</span></span>

    [![](external-authentication-services/_static/image56.png "Click to Expand the Image")](external-authentication-services/_static/image55.png)
6. <span data-ttu-id="7aa7c-207">Depois de inserir suas credenciais do Twitter e clicar em **autorizar aplicativo**, seu navegador da Web será Redirecionado de volta para seu aplicativo Web, o que lhe solicitará o **nome de usuário** que você deseja associar à sua conta do Twitter:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-207">After you enter your Twitter credentials and click **Authorize app**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Twitter account:</span></span>

    [![](external-authentication-services/_static/image58.png "Click to Expand the Image")](external-authentication-services/_static/image57.png)
7. <span data-ttu-id="7aa7c-208">Depois de inserir seu nome de usuário e clicar no botão **inscrever-se** , seu aplicativo Web exibirá o **Home Page** padrão para sua conta do Twitter:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-208">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Twitter account:</span></span>

    [![](external-authentication-services/_static/image60.png "Click to Expand the Image")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a><span data-ttu-id="7aa7c-209">Informações adicionais</span><span class="sxs-lookup"><span data-stu-id="7aa7c-209">Additional Information</span></span>

<span data-ttu-id="7aa7c-210">Para obter informações adicionais sobre como criar aplicativos que usam OAuth e OpenID, consulte as seguintes URLs:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-210">For additional information about creating applications that use OAuth and OpenID, see the following URLs:</span></span>

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a><span data-ttu-id="7aa7c-211">Combinando serviços de autenticação externa</span><span class="sxs-lookup"><span data-stu-id="7aa7c-211">Combining External Authentication Services</span></span>

<span data-ttu-id="7aa7c-212">Para obter maior flexibilidade, você pode definir vários serviços de autenticação externa ao mesmo tempo – isso permite que os usuários do aplicativo Web usem uma conta de qualquer um dos serviços de autenticação externa habilitados:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-212">For greater flexibility, you can define multiple external authentication services at the same time - this allows your web application's users to use an account from any of the enabled external authentication services:</span></span>

[![](external-authentication-services/_static/image62.png "Click to Expand the Image")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configure-iis-express-to-use-a-fully-qualified-domain-name"></a><span data-ttu-id="7aa7c-213">Configurar IIS Express para usar um nome de domínio totalmente qualificado</span><span class="sxs-lookup"><span data-stu-id="7aa7c-213">Configure IIS Express to use a fully qualified domain name</span></span>

<span data-ttu-id="7aa7c-214">Alguns provedores de autenticação externa não dão suporte ao teste de seu aplicativo usando um endereço HTTP como `http://localhost:port/`.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-214">Some external authentication providers do not support testing your application by using an HTTP address like `http://localhost:port/`.</span></span> <span data-ttu-id="7aa7c-215">Para contornar esse problema, você pode adicionar um mapeamento de FQDN (nome de domínio totalmente qualificado) para o arquivo de HOSTs e configurar suas opções de projeto no Visual Studio 2017 para usar o FQDN para teste/depuração.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-215">To work around this issue, you can add a static Fully Qualified Domain Name (FQDN) mapping to your HOSTS file and configure your project options in Visual Studio 2017 to use the FQDN for testing/debugging.</span></span> <span data-ttu-id="7aa7c-216">Para fazer isso, execute as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-216">To do so, use the following steps:</span></span>

- <span data-ttu-id="7aa7c-217">Adicione um FQDN estático mapeando seu arquivo HOSTs:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-217">Add a static FQDN mapping your HOSTS file:</span></span>

  1. <span data-ttu-id="7aa7c-218">Abra um prompt de comando com privilégios elevados no Windows.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-218">Open an elevated command prompt in Windows.</span></span>
  2. <span data-ttu-id="7aa7c-219">Digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-219">Type the following command:</span></span>

      <span data-ttu-id="7aa7c-220"><kbd>%WinDir%\system32\drivers\etc\hosts do bloco de notas</kbd></span><span class="sxs-lookup"><span data-stu-id="7aa7c-220"><kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd></span></span>
  3. <span data-ttu-id="7aa7c-221">Adicione uma entrada como a seguinte ao arquivo de HOSTs:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-221">Add an entry like the following to the HOSTS file:</span></span>

      <span data-ttu-id="7aa7c-222"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span><span class="sxs-lookup"><span data-stu-id="7aa7c-222"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span></span>
  4. <span data-ttu-id="7aa7c-223">Salve e feche o arquivo de HOSTs.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-223">Save and close your HOSTS file.</span></span>

- <span data-ttu-id="7aa7c-224">Configure seu projeto do Visual Studio para usar o FQDN:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-224">Configure your Visual Studio project to use the FQDN:</span></span>

  1. <span data-ttu-id="7aa7c-225">Quando o projeto estiver aberto no Visual Studio 2017, clique no menu **projeto** e selecione as propriedades do projeto.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-225">When your project is open in Visual Studio 2017, click the **Project** menu, and then select your project's properties.</span></span> <span data-ttu-id="7aa7c-226">Por exemplo, você pode selecionar **Propriedades WebApplication1**.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-226">For example, you might select **WebApplication1 Properties**.</span></span>
  2. <span data-ttu-id="7aa7c-227">Selecione a guia **Web** .</span><span class="sxs-lookup"><span data-stu-id="7aa7c-227">Select the **Web** tab.</span></span>
  3. <span data-ttu-id="7aa7c-228">Insira seu FQDN para a <strong>URL do projeto</strong>.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-228">Enter your FQDN for the <strong>Project Url</strong>.</span></span> <span data-ttu-id="7aa7c-229">Por exemplo, você deve inserir <kbd><http://www.wingtiptoys.com></kbd> se esse fosse o mapeamento de FQDN que você adicionou ao arquivo de hosts.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-229">For example, you would enter <kbd><http://www.wingtiptoys.com></kbd> if that was the FQDN mapping that you added to your HOSTS file.</span></span>

- <span data-ttu-id="7aa7c-230">Configure IIS Express para usar o FQDN para seu aplicativo:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-230">Configure IIS Express to use the FQDN for your application:</span></span>

    1. <span data-ttu-id="7aa7c-231">Abra um prompt de comando com privilégios elevados no Windows.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-231">Open an elevated command prompt in Windows.</span></span>
    2. <span data-ttu-id="7aa7c-232">Digite o seguinte comando para alterar para sua pasta IIS Express:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-232">Type the following command to change to your IIS Express folder:</span></span>

        <span data-ttu-id="7aa7c-233"><kbd>CD/d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span><span class="sxs-lookup"><span data-stu-id="7aa7c-233"><kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span></span>
    3. <span data-ttu-id="7aa7c-234">Digite o seguinte comando para adicionar o FQDN ao seu aplicativo:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-234">Type the following command to add the FQDN to your application:</span></span>

        <span data-ttu-id="7aa7c-235"><kbd>Appcmd. exe set config-section: System. applicationHost/sites/+&quot;[name = ' WebApplication1 ']. Bindings. [Protocol = ' http ', bindingInformation = ' \*: 80: www. wingtiptoys. com ']&quot;/commit: appHost</kbd></span><span class="sxs-lookup"><span data-stu-id="7aa7c-235"><kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='\*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd></span></span>

  <span data-ttu-id="7aa7c-236">Em que **WebApplication1** é o nome do seu projeto e **bindingInformation** contém o número da porta e o FQDN que você deseja usar para o teste.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-236">Where **WebApplication1** is the name of your project and **bindingInformation** contains the port number and FQDN that you want to use for your testing.</span></span>

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a><span data-ttu-id="7aa7c-237">Como obter as configurações do aplicativo para autenticação da Microsoft</span><span class="sxs-lookup"><span data-stu-id="7aa7c-237">How to Obtain your Application Settings for Microsoft Authentication</span></span>

<span data-ttu-id="7aa7c-238">Vincular um aplicativo ao Windows Live para autenticação da Microsoft é um processo simples.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-238">Linking an application to Windows Live for Microsoft Authentication is a simple process.</span></span> <span data-ttu-id="7aa7c-239">Se você ainda não tiver vinculado um aplicativo ao Windows Live, poderá usar as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-239">If you have not already linked an application to Windows Live, you can use the following steps:</span></span>

1. <span data-ttu-id="7aa7c-240">Navegue até [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) e insira seu nome de conta Microsoft e senha quando solicitado e clique em **entrar**:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-240">Browse to [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) and enter your Microsoft account name and password when prompted, then click **Sign in**:</span></span>

   <!--  [![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png) -->
2. <span data-ttu-id="7aa7c-241">Selecione **Adicionar um aplicativo** e insira o nome do seu aplicativo quando solicitado e, em seguida, clique em **criar**:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-241">Select **Add an app** and enter the name of your application when prompted, and then click **Create**:</span></span>

    [![](external-authentication-services/_static/image79.png "Click to Expand the Image")](external-authentication-services/_static/image79.png)
3. <span data-ttu-id="7aa7c-242">Selecione seu aplicativo em **nome** e sua página de propriedades do aplicativo será exibida.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-242">Select your app under **Name** and its application properties page appears.</span></span>

4. <span data-ttu-id="7aa7c-243">Insira o domínio de redirecionamento para seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-243">Enter the redirect domain for your application.</span></span> <span data-ttu-id="7aa7c-244">Copie a **ID do aplicativo** e, em **segredos do aplicativo**, selecione **gerar senha**.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-244">Copy the **Application ID** and, under **Application Secrets**, select **Generate Password**.</span></span> <span data-ttu-id="7aa7c-245">Copie a senha que aparece.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-245">Copy the password that appears.</span></span> <span data-ttu-id="7aa7c-246">A ID do aplicativo e a senha são a ID do cliente e o segredo do cliente.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-246">The application ID and password are your client ID and client secret.</span></span> <span data-ttu-id="7aa7c-247">Selecione **OK** e, em seguida, **salvar**.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-247">Select **Ok** and then **Save**.</span></span>

    [![](external-authentication-services/_static/image77.png "Click to Expand the Image")](external-authentication-services/_static/image77.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a><span data-ttu-id="7aa7c-248">Opcional: desabilitar o registro local</span><span class="sxs-lookup"><span data-stu-id="7aa7c-248">Optional: Disable Local Registration</span></span>

<span data-ttu-id="7aa7c-249">A funcionalidade atual de registro local ASP.NET não impede que os bots (programas automatizados) criem contas de membro; por exemplo, usando uma tecnologia de validação e prevenção de bot, como [captcha](../../../web-pages/overview/security/16-adding-security-and-membership.md).</span><span class="sxs-lookup"><span data-stu-id="7aa7c-249">The current ASP.NET local registration functionality does not prevent automated programs (bots) from creating member accounts; for example, by using a bot-prevention and validation technology like [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md).</span></span> <span data-ttu-id="7aa7c-250">Por isso, você deve remover o formulário de logon local e o link de registro na página de logon.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-250">Because of this, you should remove the local login form and registration link on the login page.</span></span> <span data-ttu-id="7aa7c-251">Para fazer isso, abra a página *\_login. cshtml* em seu projeto e comente as linhas para o painel de logon local e o link de registro.</span><span class="sxs-lookup"><span data-stu-id="7aa7c-251">To do so, open the *\_Login.cshtml* page in your project, and then comment out the lines for the local login panel and the registration link.</span></span> <span data-ttu-id="7aa7c-252">A página resultante deve ser semelhante ao seguinte exemplo de código:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-252">The resulting page should look like the following code sample:</span></span>

[!code-html[Main](external-authentication-services/samples/sample10.html)]

<span data-ttu-id="7aa7c-253">Depois que o painel de logon local e o link de registro tiverem sido desabilitados, sua página de logon exibirá somente os provedores de autenticação externa que você tiver habilitado:</span><span class="sxs-lookup"><span data-stu-id="7aa7c-253">Once the local login panel and the registration link have been disabled, your login page will only display the external authentication providers that you have enabled:</span></span>

[![](external-authentication-services/_static/image70.png "Click to Expand the Image")](external-authentication-services/_static/image69.png)
