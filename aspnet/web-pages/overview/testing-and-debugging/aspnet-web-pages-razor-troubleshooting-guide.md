---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: Guia de solução de problemas do Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este artigo descreve os problemas que você pode ter ao trabalhar com Páginas da Web do ASP.NET (Razor) e algumas soluções sugeridas. Versões de software ASP.NET Web pag...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: fc03767c16f46c1e282d24ee3a7df2409a7c38bb
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78585760"
---
# <a name="aspnet-web-pages-razor-troubleshooting-guide"></a><span data-ttu-id="27aca-104">Guia de solução de problemas de Páginas da Web do ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="27aca-104">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>

<span data-ttu-id="27aca-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="27aca-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="27aca-106">Este artigo descreve os problemas que você pode ter ao trabalhar com Páginas da Web do ASP.NET (Razor) e algumas soluções sugeridas.</span><span class="sxs-lookup"><span data-stu-id="27aca-106">This article describes issues that you might have when working with ASP.NET Web Pages (Razor) and some suggested solutions.</span></span>
> 
> ## <a name="software-versions"></a><span data-ttu-id="27aca-107">Versões de software</span><span class="sxs-lookup"><span data-stu-id="27aca-107">Software versions</span></span>
> 
> 
> - <span data-ttu-id="27aca-108">Páginas da Web do ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="27aca-108">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="27aca-109">Este tutorial também funciona com Páginas da Web do ASP.NET 2 e Páginas da Web do ASP.NET 1,0.</span><span class="sxs-lookup"><span data-stu-id="27aca-109">This tutorial also works with ASP.NET Web Pages 2 and ASP.NET Web Pages 1.0.</span></span>

<span data-ttu-id="27aca-110">Este tópico contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="27aca-110">This topic contains the following sections:</span></span>

- [<span data-ttu-id="27aca-111">Problemas com páginas em execução</span><span class="sxs-lookup"><span data-stu-id="27aca-111">Issues with Running Pages</span></span>](#Issues_Running_.cshtml_Pages)
- [<span data-ttu-id="27aca-112">Problemas com o código do Razor</span><span class="sxs-lookup"><span data-stu-id="27aca-112">Issues with Razor Code</span></span>](#IssuesWithRazorCode)
- [<span data-ttu-id="27aca-113">Problemas de segurança e Associação</span><span class="sxs-lookup"><span data-stu-id="27aca-113">Issues with Security and Membership</span></span>](#membership)
- [<span data-ttu-id="27aca-114">Problemas com o envio de email</span><span class="sxs-lookup"><span data-stu-id="27aca-114">Issues with Sending Email</span></span>](#email)
- [<span data-ttu-id="27aca-115">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="27aca-115">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="27aca-116">Para perguntas gerais, consulte [FAQ do páginas da Web do ASP.net (Razor)](https://go.microsoft.com/fwlink/?LinkId=253000).</span><span class="sxs-lookup"><span data-stu-id="27aca-116">For general questions, see [ASP.NET Web Pages (Razor) FAQ](https://go.microsoft.com/fwlink/?LinkId=253000).</span></span>

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a><span data-ttu-id="27aca-117">Problemas com páginas em execução</span><span class="sxs-lookup"><span data-stu-id="27aca-117">Issues with Running Pages</span></span>

<span data-ttu-id="27aca-118">Uma variedade de problemas pode impedir que as páginas *. cshtml* e *. vbhtml* sejam executadas corretamente.</span><span class="sxs-lookup"><span data-stu-id="27aca-118">A variety of issues can prevent *.cshtml* and *.vbhtml* pages from running properly.</span></span> <span data-ttu-id="27aca-119">Esta seção lista mensagens de erro comuns e causas prováveis.</span><span class="sxs-lookup"><span data-stu-id="27aca-119">This section lists common error messages and likely causes.</span></span>

### <a name="http-error-403---forbidden-access-is-denied"></a><span data-ttu-id="27aca-120">Erro HTTP 403-Proibido: acesso negado</span><span class="sxs-lookup"><span data-stu-id="27aca-120">HTTP Error 403 - Forbidden: Access is denied</span></span>

<span data-ttu-id="27aca-121">*Você não tem permissão para exibir este diretório ou página usando as credenciais fornecidas.*</span><span class="sxs-lookup"><span data-stu-id="27aca-121">*You do not have permission to view this directory or page using the credentials that you supplied.*</span></span>

<span data-ttu-id="27aca-122">Esse erro pode ocorrer se o servidor não estiver executando a versão correta do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="27aca-122">This error can occur if the server is not running the correct version of the .NET Framework.</span></span> <span data-ttu-id="27aca-123">Certifique-se de que o computador que está executando o servidor (local ou remotamente) tenha pelo menos o .NET Framework 4 instalado.</span><span class="sxs-lookup"><span data-stu-id="27aca-123">Make sure that the computer that's running the server (locally or remotely) has at least the .NET Framework 4 installed.</span></span> <span data-ttu-id="27aca-124">Verifique também se o próprio aplicativo está configurado para executar a versão correta.</span><span class="sxs-lookup"><span data-stu-id="27aca-124">Also make sure that the application itself is configured to run the right version.</span></span>

<span data-ttu-id="27aca-125">Se você vir esse problema localmente enquanto trabalha no WebMatrix, clique no espaço de trabalho **site** e, em seguida, em TreeView, clique em **configurações**.</span><span class="sxs-lookup"><span data-stu-id="27aca-125">If you see this problem locally while working in WebMatrix, click the **Site** workspace, and then in the treeview click **Settings**.</span></span> <span data-ttu-id="27aca-126">Na lista **selecionar versão .NET Framework** , selecione **.NET 4 (integrado)** .</span><span class="sxs-lookup"><span data-stu-id="27aca-126">In the **Select .NET Framework Version** list, select **.NET 4 (Integrated)**.</span></span> <span data-ttu-id="27aca-127">Se essa versão já estiver definida, tente executar o WebMatrix como administrador.</span><span class="sxs-lookup"><span data-stu-id="27aca-127">If this version is already set, try running WebMatrix as an administrator.</span></span>

<span data-ttu-id="27aca-128">Certifique-se de que a raiz do seu site tenha pelo menos um arquivo *. cshtml* nele.</span><span class="sxs-lookup"><span data-stu-id="27aca-128">Make sure that the root of your website has at least one *.cshtml* file in it.</span></span>

<span data-ttu-id="27aca-129">Se você vir esse erro quando o servidor Web estiver em um servidor remoto, contate o administrador do servidor.</span><span class="sxs-lookup"><span data-stu-id="27aca-129">If you see this error when the web server is on a remote server, contact the server administrator.</span></span> <span data-ttu-id="27aca-130">Verifique se o servidor tem o .NET Framework 4 ou posterior instalado.</span><span class="sxs-lookup"><span data-stu-id="27aca-130">Make sure that the server has the .NET Framework 4 or later installed.</span></span> <span data-ttu-id="27aca-131">Verifique também se o aplicativo está em execução em um pool de aplicativos que está configurado para usar essa versão do the.NET Framework.</span><span class="sxs-lookup"><span data-stu-id="27aca-131">Also make sure that the application is running in an application pool that's configured to use that version of the.NET Framework.</span></span>

<span data-ttu-id="27aca-132">Se você tiver controle sobre o servidor, verifique se ele está executando a versão correta do .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="27aca-132">If you have control over the server, make sure it's running the correct version of the .NET Framework.</span></span> <span data-ttu-id="27aca-133">Você também pode tentar reparar a instalação executando o comando `aspnet_regiis -iru`.</span><span class="sxs-lookup"><span data-stu-id="27aca-133">You might also try repairing the installation by running the `aspnet_regiis -iru` command.</span></span> <span data-ttu-id="27aca-134">(Por exemplo, se você instalar o IIS depois de instalar o .NET Framework, o IIS não será configurado corretamente para executar páginas do ASP.NET.) Para obter mais informações, consulte [ferramenta de registro do IIS do ASP.net (Aspnet\_regiis. exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span><span class="sxs-lookup"><span data-stu-id="27aca-134">(For example, if you install IIS after you install the .NET Framework, IIS will not be correctly configured to run ASP.NET pages.) For more information, see [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).</span></span>

### <a name="http-error-40314---forbidden"></a><span data-ttu-id="27aca-135">Erro HTTP 403,14-proibido</span><span class="sxs-lookup"><span data-stu-id="27aca-135">HTTP Error 403.14 - Forbidden</span></span>

<span data-ttu-id="27aca-136">*O servidor Web está configurado para não listar o conteúdo deste diretório.*</span><span class="sxs-lookup"><span data-stu-id="27aca-136">*The Web server is configured to not list the contents of this directory.*</span></span>

<span data-ttu-id="27aca-137">Esse erro pode ocorrer se você solicitar um recurso protegido (como o arquivo *Web. config* ) ou se ele estiver em uma pasta protegida (como *aplicativo\_dados* ou *código de\_de aplicativo*).</span><span class="sxs-lookup"><span data-stu-id="27aca-137">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="http-error-40417---not-found"></a><span data-ttu-id="27aca-138">Erro HTTP 404,17-não encontrado</span><span class="sxs-lookup"><span data-stu-id="27aca-138">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="27aca-139">*O conteúdo solicitado parece ser um script e não será servido pelo manipulador de arquivo estático.*</span><span class="sxs-lookup"><span data-stu-id="27aca-139">*The requested content appears to be script and will not be served by the static file handler.*</span></span>

<span data-ttu-id="27aca-140">Esse erro pode ocorrer se o servidor não estiver configurado corretamente para usar o .NET Framework 4 ou posterior e, portanto, não reconhecer o código em blocos de `@{ }`.</span><span class="sxs-lookup"><span data-stu-id="27aca-140">This error can occur if the server is not configured correctly to use the .NET Framework 4 or later, and therefore does not recognize the code in `@{ }` blocks.</span></span> <span data-ttu-id="27aca-141">Consulte a descrição anterior para o *erro HTTP 403-Proibido: acesso negado*.</span><span class="sxs-lookup"><span data-stu-id="27aca-141">See the description earlier for *HTTP Error 403 - Forbidden: Access is denied*.</span></span>

### <a name="http-error-4047---not-found"></a><span data-ttu-id="27aca-142">Erro HTTP 404,7-não encontrado</span><span class="sxs-lookup"><span data-stu-id="27aca-142">HTTP Error 404.7 - Not Found</span></span>

<span data-ttu-id="27aca-143">*O módulo de filtragem de solicitação está configurado para negar a extensão de arquivo*</span><span class="sxs-lookup"><span data-stu-id="27aca-143">*The request filtering module is configured to deny the file extension*</span></span>

<span data-ttu-id="27aca-144">Esse erro pode ocorrer se as extensões *. cshtml* ou *. vbhtml* tiverem sido explicitamente bloqueadas no servidor.</span><span class="sxs-lookup"><span data-stu-id="27aca-144">This error can occur if *.cshtml* or *.vbhtml* extensions have been explicitly blocked on the server.</span></span> <span data-ttu-id="27aca-145">Um sintoma desse problema é que as URLs funcionam quando não incluem a extensão, mas as URLs que incluem *. cshtml* ou *. vbhtml* não funcionam.</span><span class="sxs-lookup"><span data-stu-id="27aca-145">A symptom of this problem is that URLs work when they do not include the extension, but URLs that include *.cshtml* or *.vbhtml* do not work.</span></span> <span data-ttu-id="27aca-146">Uma solução possível é reabilitar as extensões no arquivo *Web. config* do site.</span><span class="sxs-lookup"><span data-stu-id="27aca-146">A possible solution is to re-enable the extensions in the site's *Web.config* file.</span></span> <span data-ttu-id="27aca-147">O exemplo a seguir mostra como habilitar a extensão *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="27aca-147">The following example shows how to enable the *.cshtml* extension.</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a><span data-ttu-id="27aca-148">Erro HTTP 404,8-não encontrado</span><span class="sxs-lookup"><span data-stu-id="27aca-148">HTTP Error 404.8 - Not Found</span></span>

<span data-ttu-id="27aca-149">*O módulo de filtragem de solicitação está configurado para negar um caminho na URL que contém uma seção hiddenSegment.*</span><span class="sxs-lookup"><span data-stu-id="27aca-149">*The request filtering module is configured to deny a path in the URL that contains a hiddenSegment section.*</span></span>

<span data-ttu-id="27aca-150">Esse erro pode ocorrer se você solicitar um recurso protegido (como o arquivo *Web. config* ) ou se ele estiver em uma pasta protegida (como *aplicativo\_dados* ou *código de\_de aplicativo*).</span><span class="sxs-lookup"><span data-stu-id="27aca-150">This error can occur if you request a resource that's protected (like the *Web.config* file) or that's in a folder that's protected (like *App\_Data* or *App\_Code*).</span></span>

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a><span data-ttu-id="27aca-151">Este tipo de página não é atendido (erro de servidor no aplicativo '/')</span><span class="sxs-lookup"><span data-stu-id="27aca-151">This type of page is not served (Server Error in '/' Application)</span></span>

<span data-ttu-id="27aca-152">Consulte a descrição anterior para o erro HTTP 404,17.</span><span class="sxs-lookup"><span data-stu-id="27aca-152">See the description earlier for HTTP Error 404.17.</span></span>

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a><span data-ttu-id="27aca-153">Problemas com o código do Razor</span><span class="sxs-lookup"><span data-stu-id="27aca-153">Issues with Razor code</span></span>

### <a name="the-name-class-does-not-exist-in-the-current-context"></a><span data-ttu-id="27aca-154">O nome '*Class*' não existe no contexto atual</span><span class="sxs-lookup"><span data-stu-id="27aca-154">The name '*class*' does not exist in the current context</span></span>

<span data-ttu-id="27aca-155">Geralmente, um motivo para você ver esse erro é que `class` referencia um auxiliar, mas o auxiliar não está instalado.</span><span class="sxs-lookup"><span data-stu-id="27aca-155">Often, a reason you see this error is that `class` references a helper, but the helper is not installed.</span></span> <span data-ttu-id="27aca-156">Por exemplo, se você tentar usar um auxiliar, mas se não tiver instalado o pacote do NuGet, você verá esse erro.</span><span class="sxs-lookup"><span data-stu-id="27aca-156">For example, if you try to use a helper, but if you haven't installed the package from NuGet, you'll see this error.</span></span> <span data-ttu-id="27aca-157">Use a galeria do WebMatrix para localizar e instalar o auxiliar.</span><span class="sxs-lookup"><span data-stu-id="27aca-157">Use the Gallery in WebMatrix to find and install the helper.</span></span>

<span data-ttu-id="27aca-158">Se o auxiliar estiver instalado, mas a página ainda não o reconhecer, tente adicionar uma instrução de `using` ao código.</span><span class="sxs-lookup"><span data-stu-id="27aca-158">If the helper is installed, but the page still doesn't recognize it, try adding add a `using` statement to the code.</span></span> <span data-ttu-id="27aca-159">Na instrução `using`, referencie o namespace que inclui o auxiliar.</span><span class="sxs-lookup"><span data-stu-id="27aca-159">In the `using` statement, reference the namespace that includes the helper.</span></span> <span data-ttu-id="27aca-160">Por exemplo, os auxiliares básicos que estão no pacote auxiliares da Web do ASP.NET estão no namespace `System.Web.Helpers`.</span><span class="sxs-lookup"><span data-stu-id="27aca-160">For example, the basic helpers that are in the ASP.NET Web Helpers package are in the `System.Web.Helpers` namespace.</span></span> <span data-ttu-id="27aca-161">Na parte superior da página em que você deseja usar o auxiliar, adicione esta linha:</span><span class="sxs-lookup"><span data-stu-id="27aca-161">At the top of the page where you want to use the helper, add this line:</span></span>

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a><span data-ttu-id="27aca-162">Problemas de segurança e Associação</span><span class="sxs-lookup"><span data-stu-id="27aca-162">Issues with Security and Membership</span></span>

<span data-ttu-id="27aca-163">Se você estiver usando o sistema interno de segurança (Associação) no Páginas da Web do ASP.NET (Razor), poderá encontrar os seguintes problemas.</span><span class="sxs-lookup"><span data-stu-id="27aca-163">If you are using the built-in security (membership) system in ASP.NET Web Pages (Razor), you might encounter the following issues.</span></span>

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a><span data-ttu-id="27aca-164">Para chamar esse método, a propriedade "Membership. Provider" deve ser uma instância de "ExtendedMembershipProvider"</span><span class="sxs-lookup"><span data-stu-id="27aca-164">To call this method, the "Membership.Provider" property must be an instance of "ExtendedMembershipProvider"</span></span>

<span data-ttu-id="27aca-165">Esse erro pode indicar que nenhuma classe de `AspNetSqlMembershipProvider` está configurada.</span><span class="sxs-lookup"><span data-stu-id="27aca-165">This error can indicate that no `AspNetSqlMembershipProvider` class is configured.</span></span> <span data-ttu-id="27aca-166">(Um sintoma é que o site funciona bem localmente, mas gera esse erro quando você o publica no servidor de um provedor de hospedagem.) Uma correção para esse problema é habilitar explicitamente a associação simples adicionando o seguinte ao arquivo *Web. config* do site:</span><span class="sxs-lookup"><span data-stu-id="27aca-166">(A symptom is that the site works fine locally but throws this error when you publish it to a hosting provider's server.) One fix for this problem is to explicitly enable simple membership by adding the following to the site's *Web.config* file:</span></span>

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a><span data-ttu-id="27aca-167">Problemas com o envio de email</span><span class="sxs-lookup"><span data-stu-id="27aca-167">Issues with Sending Email</span></span>

<span data-ttu-id="27aca-168">Problemas com o envio de emails podem ser desafiadores de depurar.</span><span class="sxs-lookup"><span data-stu-id="27aca-168">Problems with sending email can be challenging to debug.</span></span> <span data-ttu-id="27aca-169">Um problema inicial pode ser que você não pode se conectar ao servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="27aca-169">An initial problem can be that you can't connect to the SMTP server.</span></span> <span data-ttu-id="27aca-170">Se a conexão for bem-sucedida, o ASP.NET entrega a mensagem para o servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="27aca-170">If the connection is successful, ASP.NET hands the message off to the SMTP server.</span></span> <span data-ttu-id="27aca-171">No entanto, pode haver problemas com a própria mensagem que impede o envio do servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="27aca-171">However, there can be problems with the message itself that prevents the SMTP server from sending it.</span></span>

<span data-ttu-id="27aca-172">Se seu aplicativo não enviar email com êxito, tente o seguinte:</span><span class="sxs-lookup"><span data-stu-id="27aca-172">If your application does not successfully send email, try the following:</span></span>

- <span data-ttu-id="27aca-173">O nome do servidor SMTP geralmente é algo como `smtp.provider.com` ou `smtp.provider.net`.</span><span class="sxs-lookup"><span data-stu-id="27aca-173">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="27aca-174">No entanto, se você publicar seu site em um provedor de hospedagem, o nome do servidor SMTP nesse ponto poderá ser `localhost`.</span><span class="sxs-lookup"><span data-stu-id="27aca-174">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="27aca-175">Essa situação ocorre porque, depois de você ter publicado e seu site está em execução no servidor do provedor, o servidor SMTP pode ser local a partir da perspectiva do seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="27aca-175">This situation occurs because after you've published and your site is running on the provider's server, the SMTP server might be local from the perspective of your application.</span></span> <span data-ttu-id="27aca-176">Essa alteração nos nomes de servidor pode significar que você precisa alterar o nome do servidor SMTP como parte do processo de publicação.</span><span class="sxs-lookup"><span data-stu-id="27aca-176">This change in server names might mean that you have to change the SMTP server name as part of your publishing process.</span></span>
- <span data-ttu-id="27aca-177">O número da porta geralmente é 25.</span><span class="sxs-lookup"><span data-stu-id="27aca-177">The port number is usually 25.</span></span> <span data-ttu-id="27aca-178">No entanto, alguns provedores exigem que você use a porta 587 ou alguma outra porta.</span><span class="sxs-lookup"><span data-stu-id="27aca-178">However, some providers require you to use port 587 or some other port.</span></span> <span data-ttu-id="27aca-179">Verifique com o proprietário do servidor SMTP qual número de porta eles esperam que você use.</span><span class="sxs-lookup"><span data-stu-id="27aca-179">Check with the owner of the SMTP server what port number they expect you to use.</span></span>
- <span data-ttu-id="27aca-180">Certifique-se de usar as credenciais corretas.</span><span class="sxs-lookup"><span data-stu-id="27aca-180">Make sure that you use the right credentials.</span></span> <span data-ttu-id="27aca-181">Se você publicou seu site em um provedor de hospedagem, use as credenciais que o provedor indicou especificamente para email.</span><span class="sxs-lookup"><span data-stu-id="27aca-181">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="27aca-182">Essas credenciais podem ser diferentes das credenciais que você usa para publicar.</span><span class="sxs-lookup"><span data-stu-id="27aca-182">These credentials might be different from the credentials you use to publish.</span></span>
- <span data-ttu-id="27aca-183">Às vezes, você não precisa de credenciais.</span><span class="sxs-lookup"><span data-stu-id="27aca-183">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="27aca-184">Se você estiver enviando emails usando seu provedor de Internet pessoal, seu provedor de email talvez já conheça suas credenciais.</span><span class="sxs-lookup"><span data-stu-id="27aca-184">If you're sending email by using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="27aca-185">Depois de publicar, talvez seja necessário usar credenciais diferentes do que quando você testar em seu computador local.</span><span class="sxs-lookup"><span data-stu-id="27aca-185">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
- <span data-ttu-id="27aca-186">Se seu provedor de email usar criptografia, defina `WebMail.EnableSsl` como `true`.</span><span class="sxs-lookup"><span data-stu-id="27aca-186">If your email provider uses encryption, set `WebMail.EnableSsl` to `true`.</span></span>

<span data-ttu-id="27aca-187">Se houver um erro ao enviar email, você poderá ver uma mensagem de erro padrão do ASP.NET, que tem esta aparência:</span><span class="sxs-lookup"><span data-stu-id="27aca-187">If there is an error sending email, you might see a standard ASP.NET error message, which looks like this:</span></span>

![Mensagem de erro do ASP.NET quando há um problema com o email](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

<span data-ttu-id="27aca-189">Você também pode depurar problemas com o envio de email usando um bloco de `try-catch`, como no exemplo a seguir.</span><span class="sxs-lookup"><span data-stu-id="27aca-189">You can also debug problems with sending email by using a `try-catch` block, as in the following example.</span></span> <span data-ttu-id="27aca-190">Quando você usa um bloco de `try-catch`, o ASP.NET não exibe suas mensagens de erro padrão.</span><span class="sxs-lookup"><span data-stu-id="27aca-190">When you use a `try-catch` block, ASP.NET does not display its standard error messages.</span></span> <span data-ttu-id="27aca-191">Em vez disso, você pode capturar o erro na parte `catch` do bloco.</span><span class="sxs-lookup"><span data-stu-id="27aca-191">Instead, you can capture the error in the `catch` portion of the block.</span></span>

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

<span data-ttu-id="27aca-192">Substitua os valores apropriados para `your-SMTP-server-name`e assim por diante.</span><span class="sxs-lookup"><span data-stu-id="27aca-192">Substitute the appropriate values for `your-SMTP-server-name`, and so on.</span></span> <span data-ttu-id="27aca-193">Algumas das mensagens de erro que você pode ver dessa maneira incluem o seguinte:</span><span class="sxs-lookup"><span data-stu-id="27aca-193">Some of the error messages you might see this way include the following:</span></span>

- <span data-ttu-id="27aca-194">*Falha ao enviar email.*</span><span class="sxs-lookup"><span data-stu-id="27aca-194">*Failure sending mail.*</span></span>

    <span data-ttu-id="27aca-195">-ou-</span><span class="sxs-lookup"><span data-stu-id="27aca-195">-or-</span></span>

    <span data-ttu-id="27aca-196">*Uma tentativa de conexão falhou porque a parte conectada não respondeu corretamente após um período de tempo ou a conexão estabelecida falhou porque o host conectado falhou ao responder*</span><span class="sxs-lookup"><span data-stu-id="27aca-196">*A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond*</span></span>

    <span data-ttu-id="27aca-197">Esse erro normalmente significa que o aplicativo não pôde se conectar ao servidor SMTP.</span><span class="sxs-lookup"><span data-stu-id="27aca-197">This error usually means that the application could not connect to the SMTP server.</span></span> <span data-ttu-id="27aca-198">Verifique o nome do servidor e o número da porta.</span><span class="sxs-lookup"><span data-stu-id="27aca-198">Check the server name and port number.</span></span>
- <span data-ttu-id="27aca-199">*Caixa de correio não disponível. A resposta do servidor foi: 5.1.0 &lt;someuser@invaliddomain&gt; remetente rejeitado: domínio do remetente inválido*</span><span class="sxs-lookup"><span data-stu-id="27aca-199">*Mailbox unavailable. The server response was: 5.1.0 &lt;someuser@invaliddomain&gt; sender rejected : invalid sender domain*</span></span>

    <span data-ttu-id="27aca-200">Esta mensagem pode indicar que o endereço de `From` não está correto ou ausente.</span><span class="sxs-lookup"><span data-stu-id="27aca-200">This message can indicate that the `From` address is not correct or is missing.</span></span>
- <span data-ttu-id="27aca-201">*A cadeia de caracteres especificada não está no formato necessário para um endereço de email.*</span><span class="sxs-lookup"><span data-stu-id="27aca-201">*The specified string is not in the form required for an email address.*</span></span>

    <span data-ttu-id="27aca-202">Esse erro pode indicar que o valor das propriedades `To` ou `From` não são reconhecidos como endereços de email.</span><span class="sxs-lookup"><span data-stu-id="27aca-202">This error might indicate that the value of the `To` or `From` properties are not recognized as email addresses.</span></span> <span data-ttu-id="27aca-203">(ASP.NET não pode verificar se o endereço de email é válido, apenas se ele está no formato correto, como *name@domain.com* .)</span><span class="sxs-lookup"><span data-stu-id="27aca-203">(ASP.NET cannot check that the email address is valid, only that it's in the correct format, like *name@domain.com*.)</span></span>

> [!NOTE]
> <span data-ttu-id="27aca-204">Remova a marcação que exibe o erro (`@errorMessage`) antes de publicar a página em um site ativo.</span><span class="sxs-lookup"><span data-stu-id="27aca-204">Remove the markup that displays the error (`@errorMessage`) before you publish the page to a live site.</span></span> <span data-ttu-id="27aca-205">Não é uma boa ideia permitir que os usuários vejam mensagens de erro obtidas de um servidor.</span><span class="sxs-lookup"><span data-stu-id="27aca-205">It's not a good idea to let users see error messages that you get from a server.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="27aca-206">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="27aca-206">Additional Resources</span></span>

[<span data-ttu-id="27aca-207">Perguntas frequentes sobre Páginas da Web do ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="27aca-207">ASP.NET Web Pages (Razor) FAQ</span></span>](https://go.microsoft.com/fwlink/?LinkId=253000)

<span data-ttu-id="27aca-208">Fórum do [WebMatrix e do páginas da Web do ASP.net](https://forums.asp.net/1224.aspx/1?WebMatrix) no site do ASP.net</span><span class="sxs-lookup"><span data-stu-id="27aca-208">[WebMatrix and ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) forum on the ASP.NET website</span></span>
