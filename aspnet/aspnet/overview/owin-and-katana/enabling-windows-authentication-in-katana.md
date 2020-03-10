---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Habilitando a autenticação do Windows no Katana | Microsoft Docs
author: MikeWasson
description: 'Este artigo mostra como habilitar a autenticação do Windows no katana. Ele abrange dois cenários: usando o IIS para hospedar Katana e usar HttpListener para Kat de hospedagem interna...'
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 3d81e7e1bf13ab63417378fba0c5ab80213f404b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617176"
---
# <a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="c4863-104">Habilitar a autenticação do Windows no Katana</span><span class="sxs-lookup"><span data-stu-id="c4863-104">Enabling Windows Authentication in Katana</span></span>

<span data-ttu-id="c4863-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c4863-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="c4863-106">Este artigo mostra como habilitar a autenticação do Windows no katana.</span><span class="sxs-lookup"><span data-stu-id="c4863-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="c4863-107">Ele abrange dois cenários: usando o IIS para hospedar Katana e usar HttpListener para Katana de hospedagem interna em um processo personalizado.</span><span class="sxs-lookup"><span data-stu-id="c4863-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="c4863-108">Obrigado a Barry Dorrans, David Matson e Chris Ross por revisar este artigo.</span><span class="sxs-lookup"><span data-stu-id="c4863-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>

<span data-ttu-id="c4863-109">Katana é a implementação de [OWIN](http://owin.org/)da Microsoft, a interface da Web aberta para .net.</span><span class="sxs-lookup"><span data-stu-id="c4863-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="c4863-110">Você pode ler uma introdução a OWIN e Katana [aqui](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="c4863-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="c4863-111">A arquitetura OWIN tem várias camadas:</span><span class="sxs-lookup"><span data-stu-id="c4863-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="c4863-112">Host: gerencia o processo no qual o pipeline OWIN é executado.</span><span class="sxs-lookup"><span data-stu-id="c4863-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="c4863-113">Servidor: abre um soquete de rede e escuta as solicitações.</span><span class="sxs-lookup"><span data-stu-id="c4863-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="c4863-114">Middleware: processa a solicitação e resposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="c4863-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="c4863-115">Atualmente, o Katana fornece dois servidores, os quais oferecem suporte à autenticação integrada do Windows:</span><span class="sxs-lookup"><span data-stu-id="c4863-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="c4863-116">**Microsoft. Owin. host. SystemWeb**.</span><span class="sxs-lookup"><span data-stu-id="c4863-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="c4863-117">Usa o IIS com o pipeline ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c4863-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="c4863-118">**Microsoft. Owin. host. HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="c4863-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="c4863-119">Usa [System .net. HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="c4863-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="c4863-120">Atualmente, esse servidor é a opção padrão ao hospedar internamente katana.</span><span class="sxs-lookup"><span data-stu-id="c4863-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="c4863-121">No momento, o Katana não fornece o middleware OWIN para autenticação do Windows, pois essa funcionalidade já está disponível nos servidores.</span><span class="sxs-lookup"><span data-stu-id="c4863-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>

## <a name="windows-authentication-in-iis"></a><span data-ttu-id="c4863-122">Autenticação do Windows no IIS</span><span class="sxs-lookup"><span data-stu-id="c4863-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="c4863-123">Usando Microsoft. Owin. host. SystemWeb, você pode simplesmente habilitar a autenticação do Windows no IIS.</span><span class="sxs-lookup"><span data-stu-id="c4863-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="c4863-124">Vamos começar criando um novo aplicativo ASP.NET, usando o modelo de projeto "ASP.NET vazio aplicativo Web".</span><span class="sxs-lookup"><span data-stu-id="c4863-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="c4863-125">Em seguida, adicione pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="c4863-125">Next, add NuGet packages.</span></span> <span data-ttu-id="c4863-126">No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="c4863-126">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="c4863-127">Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="c4863-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="c4863-128">Agora, adicione uma classe chamada `Startup` com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="c4863-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="c4863-129">Isso é tudo o que você precisa para criar um aplicativo "Olá, mundo" para OWIN, em execução no IIS.</span><span class="sxs-lookup"><span data-stu-id="c4863-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="c4863-130">Pressione F5 para depurar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="c4863-130">Press F5 to debug the application.</span></span> <span data-ttu-id="c4863-131">Você deve ver o "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="c4863-131">You should see "Hello World!"</span></span> <span data-ttu-id="c4863-132">na janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="c4863-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="c4863-133">Em seguida, Habilitaremos a autenticação do Windows no IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c4863-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="c4863-134">No menu **Exibir** , selecione **Propriedades**.</span><span class="sxs-lookup"><span data-stu-id="c4863-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="c4863-135">Clique no nome do projeto em Gerenciador de Soluções para exibir as propriedades do projeto.</span><span class="sxs-lookup"><span data-stu-id="c4863-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="c4863-136">Na janela **Propriedades** , defina **autenticação anônima** como **desabilitada** e defina **autenticação do Windows** como **habilitada**.</span><span class="sxs-lookup"><span data-stu-id="c4863-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="c4863-137">Quando você executa o aplicativo do Visual Studio, IIS Express exigirá as credenciais do Windows do usuário.</span><span class="sxs-lookup"><span data-stu-id="c4863-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="c4863-138">Você pode ver isso usando o [Fiddler](http://fiddler2.com/home) ou outra ferramenta de depuração de http.</span><span class="sxs-lookup"><span data-stu-id="c4863-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="c4863-139">Aqui está um exemplo de resposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="c4863-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="c4863-140">Os cabeçalhos WWW-Authenticate nessa resposta indicam que o servidor dá suporte ao protocolo [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) , que usa o Kerberos ou NTLM.</span><span class="sxs-lookup"><span data-stu-id="c4863-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="c4863-141">Posteriormente, quando você implantar o aplicativo em um servidor, siga [estas etapas](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) para habilitar a autenticação do Windows no IIS nesse servidor.</span><span class="sxs-lookup"><span data-stu-id="c4863-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="c4863-142">Autenticação do Windows em HttpListener</span><span class="sxs-lookup"><span data-stu-id="c4863-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="c4863-143">Se estiver usando Microsoft. Owin. host. HttpListener para Katana de hospedagem interna, você poderá habilitar a autenticação do Windows diretamente na instância do **HttpListener** .</span><span class="sxs-lookup"><span data-stu-id="c4863-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="c4863-144">Primeiro, crie um novo aplicativo de console.</span><span class="sxs-lookup"><span data-stu-id="c4863-144">First, create a new console application.</span></span> <span data-ttu-id="c4863-145">Em seguida, adicione pacotes NuGet.</span><span class="sxs-lookup"><span data-stu-id="c4863-145">Next, add NuGet packages.</span></span> <span data-ttu-id="c4863-146">No menu **ferramentas** , selecione **Gerenciador de pacotes NuGet**e, em seguida, selecione **console do Gerenciador de pacotes**.</span><span class="sxs-lookup"><span data-stu-id="c4863-146">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="c4863-147">Na janela Console do Gerenciador de Pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="c4863-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="c4863-148">Agora, adicione uma classe chamada `Startup` com o seguinte código:</span><span class="sxs-lookup"><span data-stu-id="c4863-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="c4863-149">Essa classe implementa o mesmo exemplo "Olá mundo" de antes, mas também define a autenticação do Windows como o esquema de autenticação.</span><span class="sxs-lookup"><span data-stu-id="c4863-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="c4863-150">Dentro da função `Main`, inicie o pipeline OWIN:</span><span class="sxs-lookup"><span data-stu-id="c4863-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="c4863-151">Você pode enviar uma solicitação no Fiddler para confirmar que o aplicativo está usando a autenticação do Windows:</span><span class="sxs-lookup"><span data-stu-id="c4863-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="c4863-152">Tópicos Relacionados</span><span class="sxs-lookup"><span data-stu-id="c4863-152">Related Topics</span></span>

[<span data-ttu-id="c4863-153">Uma visão geral do projeto Katana</span><span class="sxs-lookup"><span data-stu-id="c4863-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="c4863-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="c4863-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="c4863-155">Noções básicas sobre a autenticação de formulários OWIN no MVC 5</span><span class="sxs-lookup"><span data-stu-id="c4863-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
