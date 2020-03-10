---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: O que não fazer em ASP.NET e o que fazer em vez disso | Microsoft Docs
author: Rick-Anderson
description: Este tópico descreve vários erros comuns que as pessoas fazem em projetos da Web do ASP.NET. Ele fornece recomendações sobre o que você deve fazer para evitar esses comentários...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 980d3544df70643043391e6573803ce21b3a824f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616987"
---
# <a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="b8db8-104">O que fazer e o que não fazer em ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b8db8-104">What not to do in ASP.NET, and what to do instead</span></span>

> <span data-ttu-id="b8db8-105">Este tópico descreve vários erros comuns que as pessoas fazem em projetos da Web do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b8db8-105">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="b8db8-106">Ele fornece recomendações para o que você deve fazer para evitar esses erros comuns.</span><span class="sxs-lookup"><span data-stu-id="b8db8-106">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="b8db8-107">Ele se baseia em uma [apresentação](http://vimeo.com/68390507) da **Damian Edwards** na conferência de desenvolvedores de norueguês.</span><span class="sxs-lookup"><span data-stu-id="b8db8-107">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>

## <a name="disclaimer"></a><span data-ttu-id="b8db8-108">Isenção de responsabilidade</span><span class="sxs-lookup"><span data-stu-id="b8db8-108">Disclaimer</span></span>

<span data-ttu-id="b8db8-109">Este tópico não é destinado a um guia completo para garantir que seu aplicativo seja seguro e eficiente.</span><span class="sxs-lookup"><span data-stu-id="b8db8-109">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="b8db8-110">Você ainda precisa seguir as práticas recomendadas de segurança e desempenho que não estão descritos neste tópico.</span><span class="sxs-lookup"><span data-stu-id="b8db8-110">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="b8db8-111">Ele apenas sugere como evitar erros comuns relacionados a processos e classes do .NET.</span><span class="sxs-lookup"><span data-stu-id="b8db8-111">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="b8db8-112">Visão geral</span><span class="sxs-lookup"><span data-stu-id="b8db8-112">Overview</span></span>

<span data-ttu-id="b8db8-113">Este tópico contém as seguintes seções:</span><span class="sxs-lookup"><span data-stu-id="b8db8-113">This topic contains the following sections:</span></span>

- [<span data-ttu-id="b8db8-114">Conformidade com os padrões</span><span class="sxs-lookup"><span data-stu-id="b8db8-114">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="b8db8-115">Adaptadores de controle</span><span class="sxs-lookup"><span data-stu-id="b8db8-115">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="b8db8-116">Propriedades de estilo em controles</span><span class="sxs-lookup"><span data-stu-id="b8db8-116">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="b8db8-117">Retornos de chamada de página e controle</span><span class="sxs-lookup"><span data-stu-id="b8db8-117">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="b8db8-118">Detecção de capacidade do navegador</span><span class="sxs-lookup"><span data-stu-id="b8db8-118">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="b8db8-119">Segurança</span><span class="sxs-lookup"><span data-stu-id="b8db8-119">Security</span></span>](#security)

    - [<span data-ttu-id="b8db8-120">Validação de solicitação</span><span class="sxs-lookup"><span data-stu-id="b8db8-120">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="b8db8-121">Autenticação e sessão de formulários sem cookie</span><span class="sxs-lookup"><span data-stu-id="b8db8-121">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="b8db8-122">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="b8db8-122">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="b8db8-123">Confiança média</span><span class="sxs-lookup"><span data-stu-id="b8db8-123">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="b8db8-124">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="b8db8-124">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="b8db8-125">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="b8db8-125">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="b8db8-126">Confiabilidade e desempenho</span><span class="sxs-lookup"><span data-stu-id="b8db8-126">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="b8db8-127">PreSendRequestHeaders e PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="b8db8-127">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="b8db8-128">Eventos de página assíncrona com Web Forms</span><span class="sxs-lookup"><span data-stu-id="b8db8-128">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="b8db8-129">Trabalho de disparar e esquecer</span><span class="sxs-lookup"><span data-stu-id="b8db8-129">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="b8db8-130">Corpo da entidade de solicitação</span><span class="sxs-lookup"><span data-stu-id="b8db8-130">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="b8db8-131">Response. redirecionamento e resposta. end</span><span class="sxs-lookup"><span data-stu-id="b8db8-131">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="b8db8-132">EnableViewState e ViewStatemode</span><span class="sxs-lookup"><span data-stu-id="b8db8-132">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="b8db8-133">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="b8db8-133">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="b8db8-134">Solicitações de execução longa (> 110 segundos)</span><span class="sxs-lookup"><span data-stu-id="b8db8-134">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="b8db8-135">Conformidade com os padrões</span><span class="sxs-lookup"><span data-stu-id="b8db8-135">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="b8db8-136">Adaptadores de controle</span><span class="sxs-lookup"><span data-stu-id="b8db8-136">Control adapters</span></span>

<span data-ttu-id="b8db8-137">Recomendação: Pare de usar adaptadores de controle para processamento adaptável e, em vez disso, use consultas de mídia CSS e HTML compatível com padrões.</span><span class="sxs-lookup"><span data-stu-id="b8db8-137">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="b8db8-138">Os adaptadores de controles foram introduzidos no .NET 2,0 para renderizar o código de apresentação que foi personalizado para diferentes dispositivos e ambientes.</span><span class="sxs-lookup"><span data-stu-id="b8db8-138">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="b8db8-139">Agora, essa renderização adaptável pode ser realizada com CSS e HTML.</span><span class="sxs-lookup"><span data-stu-id="b8db8-139">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="b8db8-140">Você deve parar de usar os adaptadores de controle e converter quaisquer adaptadores existentes em CSS e HTML.</span><span class="sxs-lookup"><span data-stu-id="b8db8-140">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="b8db8-141">Para obter mais informações, consulte [consultas de mídia](http://www.w3.org/TR/css3-mediaqueries/) e [como adicionar páginas móveis ao seu aplicativo ASP.NET Web Forms/MVC](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="b8db8-141">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="b8db8-142">Propriedades de estilo em controles</span><span class="sxs-lookup"><span data-stu-id="b8db8-142">Style properties on controls</span></span>

<span data-ttu-id="b8db8-143">Recomendação: Pare a configuração de valores de estilo na marcação de controle e, em vez disso, defina valores de formatação em folhas de estilos CSS.</span><span class="sxs-lookup"><span data-stu-id="b8db8-143">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="b8db8-144">Os controles de servidor Web contêm dezenas de propriedades que podem ser usadas para definir propriedades de estilo na linha.</span><span class="sxs-lookup"><span data-stu-id="b8db8-144">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="b8db8-145">Por exemplo, a propriedade ForeColor define a cor do texto de um controle.</span><span class="sxs-lookup"><span data-stu-id="b8db8-145">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="b8db8-146">Você pode realizar esse mesmo efeito com mais eficiência por meio de folhas de estilo CSS.</span><span class="sxs-lookup"><span data-stu-id="b8db8-146">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="b8db8-147">As folhas de estilos permitem centralizar valores de estilo e evitar definir esses valores em todo o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b8db8-147">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="b8db8-148">O exemplo a seguir mostra uma classe CSS que define o texto como vermelho.</span><span class="sxs-lookup"><span data-stu-id="b8db8-148">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="b8db8-149">O exemplo a seguir mostra como aplicar dinamicamente a classe CSS.</span><span class="sxs-lookup"><span data-stu-id="b8db8-149">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="b8db8-150">Retornos de chamada de página e controle</span><span class="sxs-lookup"><span data-stu-id="b8db8-150">Page and control callbacks</span></span>

<span data-ttu-id="b8db8-151">Recomendação: Pare de usar retornos de chamada de página e controle e, em vez disso, use qualquer um dos seguintes: AJAX, UpdatePanel, métodos de ação MVC, API Web ou Signalr.</span><span class="sxs-lookup"><span data-stu-id="b8db8-151">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="b8db8-152">Em versões anteriores do ASP.NET, os métodos de retorno de chamada Page e Control permitiam que você atualizasse parte da página da Web sem Atualizar uma página inteira.</span><span class="sxs-lookup"><span data-stu-id="b8db8-152">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="b8db8-153">Agora você pode realizar atualizações de página parcial por meio de [Ajax](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [API da Web](../../../web-api/index.md) ou [signalr](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="b8db8-153">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="b8db8-154">Você deve interromper o uso de métodos de retorno de chamada porque eles podem causar problemas com URLs e roteamento amigáveis.</span><span class="sxs-lookup"><span data-stu-id="b8db8-154">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="b8db8-155">Por padrão, os controles não habilitam métodos de retorno de chamada, mas se você habilitou esse recurso em um controle, você deve desabilitá-lo.</span><span class="sxs-lookup"><span data-stu-id="b8db8-155">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="b8db8-156">Detecção de capacidade do navegador</span><span class="sxs-lookup"><span data-stu-id="b8db8-156">Browser capability detection</span></span>

<span data-ttu-id="b8db8-157">Recomendação: Pare de usar a detecção de capacidade de navegador estático e, em vez disso, use a detecção dinâmica de recursos.</span><span class="sxs-lookup"><span data-stu-id="b8db8-157">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="b8db8-158">Em versões anteriores do ASP.NET, os recursos com suporte para cada navegador foram armazenados em um arquivo XML.</span><span class="sxs-lookup"><span data-stu-id="b8db8-158">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="b8db8-159">A detecção de suporte a recursos por meio de uma pesquisa estática não é a melhor abordagem.</span><span class="sxs-lookup"><span data-stu-id="b8db8-159">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="b8db8-160">Agora, você pode detectar dinamicamente os recursos com suporte do navegador usando uma estrutura de detecção de recursos, como o [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="b8db8-160">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="b8db8-161">A detecção de recursos determina o suporte ao tentar usar um método ou uma propriedade e, em seguida, verificar se o navegador produziu o resultado desejado.</span><span class="sxs-lookup"><span data-stu-id="b8db8-161">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="b8db8-162">Por padrão, o Modernizr está incluído nos modelos de aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="b8db8-162">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="b8db8-163">Segurança</span><span class="sxs-lookup"><span data-stu-id="b8db8-163">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="b8db8-164">Validação de solicitação</span><span class="sxs-lookup"><span data-stu-id="b8db8-164">Request validation</span></span>

<span data-ttu-id="b8db8-165">Recomendação: validar a entrada do usuário e codificar a saída de usuários.</span><span class="sxs-lookup"><span data-stu-id="b8db8-165">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="b8db8-166">A validação de solicitação é um recurso de ASP.NET que inspeciona cada solicitação e interrompe a solicitação se uma ameaça percebida for encontrada.</span><span class="sxs-lookup"><span data-stu-id="b8db8-166">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="b8db8-167">Não dependa da validação de solicitação para proteger seu aplicativo contra ataques de script entre sites.</span><span class="sxs-lookup"><span data-stu-id="b8db8-167">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="b8db8-168">Em vez disso, valide todas as entradas de usuários e codifique a saída.</span><span class="sxs-lookup"><span data-stu-id="b8db8-168">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="b8db8-169">Em alguns casos limitados, você pode usar expressões regulares para validar a entrada, mas em casos mais complicados, você deve validar a entrada do usuário usando classes do .NET que determinam se o valor corresponde aos valores permitidos.</span><span class="sxs-lookup"><span data-stu-id="b8db8-169">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="b8db8-170">O exemplo a seguir mostra como usar um método estático na classe URI para determinar se o URI fornecido por um usuário é válido.</span><span class="sxs-lookup"><span data-stu-id="b8db8-170">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="b8db8-171">No entanto, para verificar o URI suficientemente, você também deve verificar se ele especifica `http` ou `https`.</span><span class="sxs-lookup"><span data-stu-id="b8db8-171">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="b8db8-172">O exemplo a seguir usa métodos de instância para verificar se o URI é válido.</span><span class="sxs-lookup"><span data-stu-id="b8db8-172">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="b8db8-173">Antes de renderizar a entrada do usuário como HTML ou incluir a entrada do usuário em uma consulta SQL, codifique os valores para garantir que o código mal-intencionado não seja incluído.</span><span class="sxs-lookup"><span data-stu-id="b8db8-173">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="b8db8-174">Você pode codificar o valor em marcação com a sintaxe &lt;%:%&gt;, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="b8db8-174">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="b8db8-175">Ou, em sintaxe Razor, você pode codificar HTML com @, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="b8db8-175">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="b8db8-176">O exemplo a seguir mostra como codificar em HTML um valor no code-behind.</span><span class="sxs-lookup"><span data-stu-id="b8db8-176">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="b8db8-177">Para codificar com segurança um valor para comandos SQL, use parâmetros de comando como o [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8db8-177">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="b8db8-178">Autenticação e sessão de formulários sem cookie</span><span class="sxs-lookup"><span data-stu-id="b8db8-178">Cookieless forms authentication and session</span></span>

<span data-ttu-id="b8db8-179">Recomendação: exigir cookies.</span><span class="sxs-lookup"><span data-stu-id="b8db8-179">Recommendation: Require cookies.</span></span>

<span data-ttu-id="b8db8-180">A passagem de informações de autenticação na cadeia de caracteres de consulta não é segura.</span><span class="sxs-lookup"><span data-stu-id="b8db8-180">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="b8db8-181">Portanto, exija cookies quando seu aplicativo incluir autenticação.</span><span class="sxs-lookup"><span data-stu-id="b8db8-181">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="b8db8-182">Se seu cookie armazena informações confidenciais, considere exigir SSL para o cookie.</span><span class="sxs-lookup"><span data-stu-id="b8db8-182">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="b8db8-183">O exemplo a seguir mostra como especificar no arquivo Web. config que a autenticação de formulários requer um cookie que é transmitido por SSL.</span><span class="sxs-lookup"><span data-stu-id="b8db8-183">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="b8db8-184">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="b8db8-184">EnableViewStateMac</span></span>

<span data-ttu-id="b8db8-185">Recomendação: nunca definido como false.</span><span class="sxs-lookup"><span data-stu-id="b8db8-185">Recommendation: Never set to false.</span></span>

<span data-ttu-id="b8db8-186">Por padrão, o EnableViewStateMac é definido como true.</span><span class="sxs-lookup"><span data-stu-id="b8db8-186">By default, EnableViewStateMac is set to true.</span></span> <span data-ttu-id="b8db8-187">Mesmo que seu aplicativo não esteja usando o estado de exibição, não defina o EnableViewStateMac como false.</span><span class="sxs-lookup"><span data-stu-id="b8db8-187">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="b8db8-188">Definir esse valor como false tornará seu aplicativo vulnerável a scripts entre sites.</span><span class="sxs-lookup"><span data-stu-id="b8db8-188">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="b8db8-189">Começando com ASP.NET 4.5.2, o tempo de execução impõe **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="b8db8-189">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="b8db8-190">Mesmo se você defini-lo como false, o tempo de execução ignora esse valor e prossegue com o valor definido como true.</span><span class="sxs-lookup"><span data-stu-id="b8db8-190">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="b8db8-191">Para obter mais informações, consulte [ASP.NET 4.5.2 e EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8db8-191">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="b8db8-192">O exemplo a seguir mostra como definir o EnableViewStateMac como true.</span><span class="sxs-lookup"><span data-stu-id="b8db8-192">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="b8db8-193">Você não precisa definir esse valor como true, pois ele é true por padrão.</span><span class="sxs-lookup"><span data-stu-id="b8db8-193">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="b8db8-194">No entanto, se você definiu-a como false em qualquer página em seu aplicativo, deverá corrigir esse valor imediatamente.</span><span class="sxs-lookup"><span data-stu-id="b8db8-194">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="b8db8-195">Confiança média</span><span class="sxs-lookup"><span data-stu-id="b8db8-195">Medium trust</span></span>

<span data-ttu-id="b8db8-196">Recomendação: não depende de confiança média (ou qualquer outro nível de confiança) como um limite de segurança.</span><span class="sxs-lookup"><span data-stu-id="b8db8-196">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="b8db8-197">A confiança parcial não protege adequadamente seu aplicativo e não deve ser usada.</span><span class="sxs-lookup"><span data-stu-id="b8db8-197">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="b8db8-198">Em vez disso, use confiança total e isole aplicativos não confiáveis em pools de aplicativos separados.</span><span class="sxs-lookup"><span data-stu-id="b8db8-198">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="b8db8-199">Além disso, execute cada pool de aplicativos em uma identidade exclusiva.</span><span class="sxs-lookup"><span data-stu-id="b8db8-199">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="b8db8-200">Para obter mais informações, consulte [confiança parcial do ASP.net não garante o isolamento do aplicativo](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="b8db8-200">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="b8db8-201">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="b8db8-201">&lt;appSettings&gt;</span></span>

<span data-ttu-id="b8db8-202">Recomendação: Não desabilite as configurações de segurança no elemento &lt;appSettings&gt;.</span><span class="sxs-lookup"><span data-stu-id="b8db8-202">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="b8db8-203">O elemento appSettings contém muitos valores que são necessários para atualizações de segurança.</span><span class="sxs-lookup"><span data-stu-id="b8db8-203">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="b8db8-204">Você não deve alterar nem desabilitar esses valores.</span><span class="sxs-lookup"><span data-stu-id="b8db8-204">You should not change or disable these values.</span></span> <span data-ttu-id="b8db8-205">Se você precisar desabilitar esses valores ao implantar uma atualização, reabilitar imediatamente depois de concluir a implantação.</span><span class="sxs-lookup"><span data-stu-id="b8db8-205">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="b8db8-206">Para obter detalhes, consulte o [elemento ASP.net appSettings](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8db8-206">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="b8db8-207">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="b8db8-207">UrlPathEncode</span></span>

<span data-ttu-id="b8db8-208">Recomendação: em vez disso, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) .</span><span class="sxs-lookup"><span data-stu-id="b8db8-208">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="b8db8-209">O método UrlPathEncode foi adicionado à .NET Framework para resolver um problema de compatibilidade de navegador muito específico.</span><span class="sxs-lookup"><span data-stu-id="b8db8-209">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="b8db8-210">Ele não codifica uma URL de forma adequada e não protege seu aplicativo contra scripts entre sites.</span><span class="sxs-lookup"><span data-stu-id="b8db8-210">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="b8db8-211">Você nunca deve usá-lo em seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b8db8-211">You should never use it in your application.</span></span> <span data-ttu-id="b8db8-212">Em vez disso, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8db8-212">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="b8db8-213">O exemplo a seguir mostra como passar uma URL codificada como um parâmetro de cadeia de caracteres de consulta para um controle de hiperlink.</span><span class="sxs-lookup"><span data-stu-id="b8db8-213">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="b8db8-214">Confiabilidade e desempenho</span><span class="sxs-lookup"><span data-stu-id="b8db8-214">Reliability and performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="b8db8-215">PreSendRequestHeaders e PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="b8db8-215">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="b8db8-216">Recomendação: não use esses eventos com módulos gerenciados.</span><span class="sxs-lookup"><span data-stu-id="b8db8-216">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="b8db8-217">Em vez disso, escreva um módulo do IIS nativo para executar a tarefa necessária.</span><span class="sxs-lookup"><span data-stu-id="b8db8-217">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="b8db8-218">Consulte [criando módulos http de código nativo](https://msdn.microsoft.com/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8db8-218">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="b8db8-219">Você pode usar os eventos [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) e [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) com módulos nativos do IIS.</span><span class="sxs-lookup"><span data-stu-id="b8db8-219">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="b8db8-220">Não use `PreSendRequestHeaders` e `PreSendRequestContent` com módulos gerenciados que implementam `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="b8db8-220">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="b8db8-221">A definição dessas propriedades pode causar problemas com solicitações assíncronas.</span><span class="sxs-lookup"><span data-stu-id="b8db8-221">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="b8db8-222">A combinação de roteamento solicitado do aplicativo (ARR) e WebSockets pode levar a exceções de violação de acesso que podem causar falhas em w3wp.</span><span class="sxs-lookup"><span data-stu-id="b8db8-222">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="b8db8-223">Por exemplo, iiscore! W3_CONTEXT_BASE:: GetIsLastNotification + 68 em iiscore. dll causou uma exceção de violação de acesso (0xC0000005).</span><span class="sxs-lookup"><span data-stu-id="b8db8-223">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="b8db8-224">Eventos de página assíncrona com Web Forms</span><span class="sxs-lookup"><span data-stu-id="b8db8-224">Asynchronous page events with web forms</span></span>

<span data-ttu-id="b8db8-225">Recomendação: em Web Forms, evite escrever métodos Async void para eventos de ciclo de vida da página e, em vez disso, use [Page. RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) para código assíncrono.</span><span class="sxs-lookup"><span data-stu-id="b8db8-225">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="b8db8-226">Ao marcar um evento de página com **Async** e **void**, você não pode determinar quando o código assíncrono foi concluído.</span><span class="sxs-lookup"><span data-stu-id="b8db8-226">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="b8db8-227">Em vez disso, use Page. RegisterAsyncTask para executar o código assíncrono de forma a permitir que você acompanhe sua conclusão.</span><span class="sxs-lookup"><span data-stu-id="b8db8-227">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="b8db8-228">O exemplo a seguir mostra um manipulador de clique de botão que contém código assíncrono.</span><span class="sxs-lookup"><span data-stu-id="b8db8-228">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="b8db8-229">Este exemplo inclui a leitura de um valor de cadeia de caracteres de forma assíncrona, que é fornecido apenas como um exemplo simplificado de uma tarefa assíncrona e não como uma prática recomendada.</span><span class="sxs-lookup"><span data-stu-id="b8db8-229">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="b8db8-230">Se você estiver usando tarefas assíncronas, defina a estrutura de destino de tempo de execução http como 4,5 (ou posterior) no arquivo Web. config.</span><span class="sxs-lookup"><span data-stu-id="b8db8-230">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 (or later) in the Web.config file.</span></span> <span data-ttu-id="b8db8-231">Definir a estrutura de destino como 4,5 ativa o novo contexto de sincronização que foi adicionado no .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="b8db8-231">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="b8db8-232">Esse valor é definido por padrão em novos projetos no Visual Studio, mas não será definido se você estiver trabalhando com um projeto existente.</span><span class="sxs-lookup"><span data-stu-id="b8db8-232">This value is set by default in new projects in Visual Studio, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="b8db8-233">Trabalho de disparar e esquecer</span><span class="sxs-lookup"><span data-stu-id="b8db8-233">Fire-and-forget work</span></span>

<span data-ttu-id="b8db8-234">Recomendação: ao lidar com uma solicitação no ASP.NET, evite iniciar o trabalho de incêndio e esquecer (chamando o método ThreadPool. QueueUserWorkItem ou criando um temporizador que chame repetidamente um delegado).</span><span class="sxs-lookup"><span data-stu-id="b8db8-234">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="b8db8-235">Se seu aplicativo tiver um trabalho de acionamento e esquecer que seja executado no ASP.NET, seu aplicativo poderá ficar fora de sincronia. A qualquer momento, o domínio do aplicativo pode ser destruído, o que significa que o processo em andamento pode não corresponder mais ao estado atual do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b8db8-235">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="b8db8-236">Você deve mover esse tipo de trabalho para fora do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b8db8-236">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="b8db8-237">Você pode usar trabalhos da Web, serviço do Windows ou uma função de trabalho no Azure para executar o trabalho em andamento e executar esse código a partir de outro processo.</span><span class="sxs-lookup"><span data-stu-id="b8db8-237">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="b8db8-238">Se for necessário executar esse trabalho em ASP.NET, você poderá adicionar o pacote NuGet chamado [Webbackgrounder](http://www.nuget.org/packages/webbackgrounder) para executar o código.</span><span class="sxs-lookup"><span data-stu-id="b8db8-238">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="b8db8-239">Corpo da entidade de solicitação</span><span class="sxs-lookup"><span data-stu-id="b8db8-239">Request entity body</span></span>

<span data-ttu-id="b8db8-240">Recomendação: Evite ler Request. Form ou Request. InputStream antes do evento de execução do manipulador.</span><span class="sxs-lookup"><span data-stu-id="b8db8-240">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="b8db8-241">O mais antigo que você deve ler de Request. Form ou Request. InputStream é durante o evento de execução do manipulador.</span><span class="sxs-lookup"><span data-stu-id="b8db8-241">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="b8db8-242">No MVC, o controlador é o manipulador e o evento execute é quando o método de ação é executado.</span><span class="sxs-lookup"><span data-stu-id="b8db8-242">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="b8db8-243">Em Web Forms, a página é o manipulador e o evento execute é quando o evento Page. Init é acionado.</span><span class="sxs-lookup"><span data-stu-id="b8db8-243">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="b8db8-244">Se você ler o corpo da entidade de solicitação anterior ao evento de execução, você interferirá no processamento da solicitação.</span><span class="sxs-lookup"><span data-stu-id="b8db8-244">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="b8db8-245">Se você precisar ler o corpo da entidade de solicitação antes do evento execute, use [Request. GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) ou [Request. GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8db8-245">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="b8db8-246">Ao usar o GetBufferlessInputStream, você obtém o fluxo bruto da solicitação e assume a responsabilidade pelo processamento de toda a solicitação.</span><span class="sxs-lookup"><span data-stu-id="b8db8-246">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="b8db8-247">Depois de chamar GetBufferlessInputStream, Request. Form e Request. InputStream não estão disponíveis porque não foram populados pelo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b8db8-247">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="b8db8-248">Ao usar o GetBufferedInputStream, você obtém uma cópia do fluxo da solicitação.</span><span class="sxs-lookup"><span data-stu-id="b8db8-248">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="b8db8-249">Request. Form e Request. InputStream ainda estão disponíveis mais tarde na solicitação porque ASP.NET popula a outra cópia.</span><span class="sxs-lookup"><span data-stu-id="b8db8-249">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="b8db8-250">Response. redirecionamento e resposta. end</span><span class="sxs-lookup"><span data-stu-id="b8db8-250">Response.Redirect and Response.End</span></span>

<span data-ttu-id="b8db8-251">Recomendação: esteja atento às diferenças em como o thread é tratado depois [de chamar Response. reredirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8db8-251">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="b8db8-252">O método [Response. reredirect (String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) chama o método Response. end.</span><span class="sxs-lookup"><span data-stu-id="b8db8-252">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="b8db8-253">Em um processo síncrono, chamar Request. redirecionar faz com que o thread atual seja anulado imediatamente.</span><span class="sxs-lookup"><span data-stu-id="b8db8-253">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="b8db8-254">No entanto, em um processo assíncrono, chamar Response. redirecionamento não anula o thread atual, portanto, a execução do código continua a solicitação.</span><span class="sxs-lookup"><span data-stu-id="b8db8-254">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="b8db8-255">Em um processo assíncrono, você deve retornar a tarefa do método para interromper a execução do código.</span><span class="sxs-lookup"><span data-stu-id="b8db8-255">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="b8db8-256">Em um projeto MVC, você não deve chamar Response. redirect.</span><span class="sxs-lookup"><span data-stu-id="b8db8-256">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="b8db8-257">Em vez disso, retorne um RedirectResult.</span><span class="sxs-lookup"><span data-stu-id="b8db8-257">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="b8db8-258">EnableViewState e ViewStatemode</span><span class="sxs-lookup"><span data-stu-id="b8db8-258">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="b8db8-259">Recomendação: Use ViewStatemode, em vez de EnableViewState, para fornecer controle granular sobre quais controles usam o estado de exibição.</span><span class="sxs-lookup"><span data-stu-id="b8db8-259">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="b8db8-260">Quando você define EnableViewState como false na diretiva Page, o estado de exibição é desabilitado para todos os controles dentro da página e não pode ser habilitado.</span><span class="sxs-lookup"><span data-stu-id="b8db8-260">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="b8db8-261">Se você quiser habilitar o estado de exibição somente para determinados controles em sua página, defina ViewStatemode como desabilitado para a página.</span><span class="sxs-lookup"><span data-stu-id="b8db8-261">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="b8db8-262">Em seguida, defina ViewStatemode como habilitado somente nos controles que realmente precisam de estado de exibição.</span><span class="sxs-lookup"><span data-stu-id="b8db8-262">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="b8db8-263">Ao habilitar o estado de exibição somente para os controles que precisam dele, você pode reduzir o tamanho do estado de exibição para suas páginas da Web.</span><span class="sxs-lookup"><span data-stu-id="b8db8-263">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="b8db8-264">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="b8db8-264">SqlMembershipProvider</span></span>

<span data-ttu-id="b8db8-265">Recomendação: Use Provedores Universais.</span><span class="sxs-lookup"><span data-stu-id="b8db8-265">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="b8db8-266">Nos modelos de projeto atuais, o SqlMembershipProvider foi substituído por [provedores universais ASP.net](http://www.nuget.org/packages/Microsoft.AspNet.Providers), que está disponível como um pacote NuGet.</span><span class="sxs-lookup"><span data-stu-id="b8db8-266">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="b8db8-267">Se você estiver usando o SqlMembershipProvider em um projeto criado com uma versão anterior dos modelos, você deverá alternar para Provedores Universais.</span><span class="sxs-lookup"><span data-stu-id="b8db8-267">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="b8db8-268">O Provedores Universais trabalhar com todos os bancos de dados com suporte no Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b8db8-268">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="b8db8-269">Para obter mais informações, consulte [introducing provedores universais ASP.net](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="b8db8-269">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="b8db8-270">Solicitações de execução longa (> 110 segundos)</span><span class="sxs-lookup"><span data-stu-id="b8db8-270">Long-running requests (>110 seconds)</span></span>

<span data-ttu-id="b8db8-271">Recomendação: Use o [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) ou O [signalr](../../../signalr/index.md) para clientes conectados e use operações de e/s assíncronas.</span><span class="sxs-lookup"><span data-stu-id="b8db8-271">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="b8db8-272">Solicitações de execução longa podem causar resultados imprevisíveis e baixo desempenho em seu aplicativo Web.</span><span class="sxs-lookup"><span data-stu-id="b8db8-272">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="b8db8-273">A configuração de tempo limite padrão para uma solicitação é de 110 segundos.</span><span class="sxs-lookup"><span data-stu-id="b8db8-273">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="b8db8-274">Se você estiver usando o estado de sessão com uma solicitação de execução longa, o ASP.NET liberará o bloqueio no objeto de sessão após 110 segundos.</span><span class="sxs-lookup"><span data-stu-id="b8db8-274">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="b8db8-275">No entanto, seu aplicativo pode estar no meio de uma operação no objeto de sessão quando o bloqueio é liberado e a operação pode não ser concluída com êxito.</span><span class="sxs-lookup"><span data-stu-id="b8db8-275">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="b8db8-276">Se uma segunda solicitação do usuário for bloqueada enquanto a primeira solicitação estiver em execução, a segunda solicitação poderá acessar o objeto de sessão em um estado inconsistente.</span><span class="sxs-lookup"><span data-stu-id="b8db8-276">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="b8db8-277">Se seu aplicativo incluir operações de e/s de bloqueio (ou síncrona), o aplicativo não responderá.</span><span class="sxs-lookup"><span data-stu-id="b8db8-277">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="b8db8-278">Para melhorar o desempenho, use as operações de e/s assíncronas no .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b8db8-278">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="b8db8-279">Além disso, use WebSockets ou Signalr para conectar clientes ao servidor.</span><span class="sxs-lookup"><span data-stu-id="b8db8-279">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="b8db8-280">Esses recursos são projetados para lidar com eficiência com as solicitações de longa execução.</span><span class="sxs-lookup"><span data-stu-id="b8db8-280">These features are designed to efficiently handle long-running requests.</span></span>
