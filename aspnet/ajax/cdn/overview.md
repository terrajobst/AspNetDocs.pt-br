---
uid: ajax/cdn/overview
title: Rede de distribuição de conteúdo do Microsoft Ajax | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2017
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 92fa428608d4ac2bf56d3c6dc9c50f1449295869
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544509"
---
# <a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="01f00-102">Rede de Distribuição de Conteúdo do Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="01f00-102">Microsoft Ajax Content Delivery Network</span></span>

> [!WARNING]
> <span data-ttu-id="01f00-103">Os aplicativos de produção não devem ter uma dependência sólida dos ativos da CDN.</span><span class="sxs-lookup"><span data-stu-id="01f00-103">Production applications should not take a hard dependency on CDN assets.</span></span> <span data-ttu-id="01f00-104">Os aplicativos devem testar o ativo CDN referenciado e usar um ativo de fallback quando a CDN não estiver disponível.</span><span class="sxs-lookup"><span data-stu-id="01f00-104">Applications should test for the CDN asset referenced, and use a fallback asset when the CDN is not available.</span></span>
>
> <span data-ttu-id="01f00-105">A CDN do Microsoft Ajax não tem nenhum SLA acima e além de usar uma CDN do Azure.</span><span class="sxs-lookup"><span data-stu-id="01f00-105">The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>
>
> <span data-ttu-id="01f00-106">Use [este problema do GitHub](https://github.com/dotnet/AspNetDocs/issues/116) para relatar problemas com a CDN do Microsoft Ajax.</span><span class="sxs-lookup"><span data-stu-id="01f00-106">Use [this GitHub issue](https://github.com/dotnet/AspNetDocs/issues/116) to report problems with the Microsoft Ajax CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="01f00-107">Sumário</span><span class="sxs-lookup"><span data-stu-id="01f00-107">Table of Contents</span></span>

<span data-ttu-id="01f00-108">**[ajax.microsoft.com renomeado como ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="01f00-108">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="01f00-109">**[Suporte ao Visual Studio. vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="01f00-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="01f00-110">**[Usando o ASP.NET AJAX da CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="01f00-110">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="01f00-111">**[Usando o jQuery da CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="01f00-111">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="01f00-112">**[Usando a interface do usuário do jQuery da CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="01f00-112">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="01f00-113">**[Arquivos de terceiros na CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="01f00-113">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="01f00-114">Versões do jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-114">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="01f00-115">Versões de migração do jQuery no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-115">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="01f00-116">Versões da interface do usuário do jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-116">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="01f00-117">Versões de validação do jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-117">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="01f00-118">Versões móveis do jQuery no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-118">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="01f00-119">Versões de modelos jQuery no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-119">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="01f00-120">Versões de ciclo jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-120">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="01f00-121">Versões do jQuery DataTables no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-121">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="01f00-122">Versões do Modernizr na CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-122">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="01f00-123">Versões JSHint no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-123">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="01f00-124">Versões do Knockout no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-124">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="01f00-125">Globalizar versões no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-125">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="01f00-126">Responder a versões no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-126">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="01f00-127">Versões de Bootstrap no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-127">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="01f00-128">Versões TouchCarousel de Bootstrap no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-128">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="01f00-129">Versões do martelo. js na CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-129">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="01f00-130">Versões Web Forms e AJAX do ASP.NET no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-130">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="01f00-131">Versões MVC do ASP.NET no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-131">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="01f00-132">Versões do Signalr ASP.NET no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-132">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="01f00-133">A Microsoft Ajax Content Delivery Network (CDN) hospeda bibliotecas JavaScript populares de terceiros, como o jQuery e permite que você os adicione facilmente para seus aplicativos Web.</span><span class="sxs-lookup"><span data-stu-id="01f00-133">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="01f00-134">Por exemplo, você pode começar a usar o jQuery, que é hospedado nessa CDN simplesmente adicionando um script &lt;&gt; marca à sua página que aponta para ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="01f00-134">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="01f00-135">Aproveitando a CDN, você pode melhorar significativamente o desempenho de seus aplicativos AJAX.</span><span class="sxs-lookup"><span data-stu-id="01f00-135">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="01f00-136">O conteúdo da CDN é armazenado em cache em servidores localizados em todo o mundo.</span><span class="sxs-lookup"><span data-stu-id="01f00-136">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="01f00-137">Além disso, a CDN permite que os navegadores reutilizem arquivos JavaScript de terceiros em cache para sites que estão localizados em domínios diferentes.</span><span class="sxs-lookup"><span data-stu-id="01f00-137">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="01f00-138">A CDN oferece suporte a SSL (HTTPS) caso você precise fornecer uma página da Web usando o protocolo SSL.</span><span class="sxs-lookup"><span data-stu-id="01f00-138">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="01f00-139">A CDN hospeda as seguintes bibliotecas de scripts de terceiros que foram carregadas e são licenciadas para você, pelos proprietários dessas bibliotecas:</span><span class="sxs-lookup"><span data-stu-id="01f00-139">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="01f00-140">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="01f00-140">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="01f00-141">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="01f00-141">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="01f00-142">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="01f00-142">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="01f00-143">Validação do jQuery (https://jqueryvalidation.org/)</span><span class="sxs-lookup"><span data-stu-id="01f00-143">jQuery Validation (https://jqueryvalidation.org/)</span></span>
- <span data-ttu-id="01f00-144">Ciclo do jQuery (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="01f00-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="01f00-145">http://datatables.net/) de tabelas jQuery (</span><span class="sxs-lookup"><span data-stu-id="01f00-145">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="01f00-146">A CDN do Microsoft AJAX também inclui as seguintes bibliotecas que foram carregadas pela Microsoft:</span><span class="sxs-lookup"><span data-stu-id="01f00-146">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="01f00-147">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="01f00-147">ASP.NET Ajax</span></span>
- <span data-ttu-id="01f00-148">Arquivos JavaScript do ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="01f00-148">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="01f00-149">Arquivos JavaScript do Signalr ASP.NET</span><span class="sxs-lookup"><span data-stu-id="01f00-149">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="01f00-150">Microsoft não reivindica a propriedade de todas as bibliotecas de terceiros hospedados nesta CDN.</span><span class="sxs-lookup"><span data-stu-id="01f00-150">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="01f00-151">Os proprietários dos direitos autorais das bibliotecas são licenciamento destas bibliotecas para você.</span><span class="sxs-lookup"><span data-stu-id="01f00-151">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="01f00-152">Quaisquer direitos que talvez você precise baixar para usar essas bibliotecas, são concedidos unicamente e exclusivamente pelos respectivos proprietários dos direitos autorais.</span><span class="sxs-lookup"><span data-stu-id="01f00-152">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="01f00-153">Como essas não são bibliotecas da Microsoft, a Microsoft não fornece nenhuma garantia ou licença de direitos de propriedade intelectual (incluindo nenhum direito de patente implícito) para as bibliotecas de terceiros hospedadas nessa CDN.</span><span class="sxs-lookup"><span data-stu-id="01f00-153">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="01f00-154">Se você quiser enviar sua biblioteca JavaScript e sua biblioteca for uma das principais bibliotecas JavaScript (conforme listado em http://trends.builtwith.com) ou extensões/plug-ins para essas bibliotecas que são (a) populares; ou (b) úteis para uso em ASP.NET, entre em contato com AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="01f00-154">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="01f00-155">ajax.microsoft.com renomeado como ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="01f00-155">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="01f00-156">A CDN usada para usar o nome de domínio microsoft.com e foi alterada para usar o nome de domínio aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="01f00-156">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="01f00-157">Essa alteração foi feita para aumentar o desempenho porque, quando um navegador referenciou o domínio microsoft.com, ele enviaria quaisquer cookies desse domínio pela conexão com cada solicitação.</span><span class="sxs-lookup"><span data-stu-id="01f00-157">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="01f00-158">Ao renomear para um nome de domínio diferente de microsoft.com, o desempenho pode ser aumentado em até 25%.</span><span class="sxs-lookup"><span data-stu-id="01f00-158">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="01f00-159">Observe que o ajax.microsoft.com continuará a funcionar, mas o ajax.aspnetcdn.com é recomendado.</span><span class="sxs-lookup"><span data-stu-id="01f00-159">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="01f00-160">Formato antigo: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="01f00-160">Old Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="01f00-161">Novo formato: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="01f00-161">New Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="01f00-162">Suporte ao Visual Studio. vsdoc</span><span class="sxs-lookup"><span data-stu-id="01f00-162">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="01f00-163">Para usar os arquivos. vsdoc corretamente com o Visual Studio 2008, você precisa certificar-se de que tem o VS 2008 SP1 instalado e o hotfix para arquivos vsdoc instalados.</span><span class="sxs-lookup"><span data-stu-id="01f00-163">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="01f00-164">Você pode obtê-los aqui:</span><span class="sxs-lookup"><span data-stu-id="01f00-164">You can get these from here:</span></span>

- [<span data-ttu-id="01f00-165">Baixe o Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="01f00-165">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Baixe o Visual Studio 2008 SP1")
- [<span data-ttu-id="01f00-166">Baixe o hotfix. vsdoc para o Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="01f00-166">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "Baixe o hotfix. vsdoc para o Visual Studio 2008 SP1")

<span data-ttu-id="01f00-167">O Visual Studio 2010 dá suporte a arquivos. vsdoc sem patches adicionais.</span><span class="sxs-lookup"><span data-stu-id="01f00-167">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="01f00-168">Usando o ASP.NET AJAX da CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-168">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="01f00-169">Ao usar o ASP.NET 4, você pode redirecionar todas as solicitações de scripts do ASP.NET Framework para a CDN.</span><span class="sxs-lookup"><span data-stu-id="01f00-169">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="01f00-170">Recuperar scripts da CDN em vez de seu servidor Web local pode melhorar substancialmente o desempenho de sites públicos do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="01f00-170">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="01f00-171">Use a propriedade EnableCDN do ScriptManager para redirecionar todas as solicitações de script do ASP.NET Framework para a CDN do Microsoft AJAX:</span><span class="sxs-lookup"><span data-stu-id="01f00-171">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="01f00-172">Usando o jQuery da CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-172">Using jQuery from the CDN</span></span>

<span data-ttu-id="01f00-173">Você pode usar scripts jQuery hospedados na CDN em seu aplicativo Web adicionando o seguinte elemento script a uma página:</span><span class="sxs-lookup"><span data-stu-id="01f00-173">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="01f00-174">A CDN também inclui a versão reduzidos do script jQuery, que você pode obter usando o seguinte elemento:</span><span class="sxs-lookup"><span data-stu-id="01f00-174">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="01f00-175">Para permitir que sua página faça fallback para o carregamento do jQuery de um caminho local em seu próprio site se a CDN não estiver disponível, adicione o seguinte elemento imediatamente após o elemento que faz referência à CDN:</span><span class="sxs-lookup"><span data-stu-id="01f00-175">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="01f00-176">A página de exemplo a seguir usa a versão CDN da biblioteca jQuery (com fallback para uma cópia local) para exibir o conteúdo de um elemento div quando um botão é clicado.</span><span class="sxs-lookup"><span data-stu-id="01f00-176">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="01f00-177">Você pode saber mais sobre o jQuery e baixar uma cópia local do jQuery visitando o site do [jQuery](http://jquery.com/) .</span><span class="sxs-lookup"><span data-stu-id="01f00-177">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="01f00-178">Usando a interface do usuário do jQuery da CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-178">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="01f00-179">A CDN também hospeda a biblioteca da interface do usuário do jQuery.</span><span class="sxs-lookup"><span data-stu-id="01f00-179">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="01f00-180">A biblioteca da interface do usuário do jQuery inclui um conjunto avançado de widgets e efeitos que você pode usar em seus aplicativos ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="01f00-180">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="01f00-181">Por exemplo, a página a seguir ilustra como você pode usar a interface do usuário do jQuery DatePicker no contexto de um aplicativo de Web Forms de ASP.NET para exibir um calendário pop-up:</span><span class="sxs-lookup"><span data-stu-id="01f00-181">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="01f00-182">Quando você move o foco para a caixa de texto usando o teclado, é exibido um calendário:</span><span class="sxs-lookup"><span data-stu-id="01f00-182">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendário pop-up criado com DatePicker](overview/_static/image1.png)

<span data-ttu-id="01f00-184">Observe que você deve incluir três arquivos da CDN no código acima:</span><span class="sxs-lookup"><span data-stu-id="01f00-184">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="01f00-185">A biblioteca jQuery &mdash; a biblioteca da interface do usuário do jQuery depende da biblioteca jQuery.</span><span class="sxs-lookup"><span data-stu-id="01f00-185">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="01f00-186">Você deve adicionar a biblioteca jQuery à sua página antes de adicionar a biblioteca da interface do usuário do jQuery.</span><span class="sxs-lookup"><span data-stu-id="01f00-186">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="01f00-187">A biblioteca da interface do usuário do jQuery &mdash; a biblioteca da interface do usuário do jQuery contém todos os efeitos e widgets da interface do usuário do jQuery, como o widget DatePicker usado na página acima.</span><span class="sxs-lookup"><span data-stu-id="01f00-187">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="01f00-188">Um tema da interface do usuário do jQuery &mdash; a interface do usuário do jQuery dá suporte a diferentes temas</span><span class="sxs-lookup"><span data-stu-id="01f00-188">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="01f00-189">A página acima inclui um link para um arquivo CSS para importar o tema Redmond.</span><span class="sxs-lookup"><span data-stu-id="01f00-189">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="01f00-190">Todos os temas padrão da interface do usuário do jQuery são hospedados na CDN.</span><span class="sxs-lookup"><span data-stu-id="01f00-190">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="01f00-191">[Visite esta página](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 na CDN do Microsoft Ajax") para exibir miniaturas de cada tema.</span><span class="sxs-lookup"><span data-stu-id="01f00-191">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="01f00-192">Para saber mais sobre a biblioteca de interface do usuário do jQuery, visite o [site oficial da interface do usuário do jQuery](http://jQueryUI.com "site da interface do usuário do jQuery").</span><span class="sxs-lookup"><span data-stu-id="01f00-192">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="01f00-193">Arquivos de terceiros na CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-193">Third-Party Files on the CDN</span></span>

<span data-ttu-id="01f00-194">A CDN hospeda algumas das mais populares bibliotecas JavaScript de terceiros.</span><span class="sxs-lookup"><span data-stu-id="01f00-194">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="01f00-195">Microsoft não reivindica a propriedade de todas as bibliotecas de terceiros hospedados nesta CDN.</span><span class="sxs-lookup"><span data-stu-id="01f00-195">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="01f00-196">Os proprietários dos direitos autorais das bibliotecas são licenciamento destas bibliotecas para você.</span><span class="sxs-lookup"><span data-stu-id="01f00-196">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="01f00-197">Quaisquer direitos que talvez você precise baixar para usar essas bibliotecas, são concedidos unicamente e exclusivamente pelos respectivos proprietários dos direitos autorais.</span><span class="sxs-lookup"><span data-stu-id="01f00-197">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="01f00-198">Como essas não são bibliotecas da Microsoft, a Microsoft não fornece nenhuma garantia ou licença de direitos de propriedade intelectual (incluindo nenhum direito de patente implícito) para as bibliotecas de terceiros hospedadas nessa CDN.</span><span class="sxs-lookup"><span data-stu-id="01f00-198">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="01f00-199">Versões do jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-199">jQuery Releases on the CDN</span></span>

<span data-ttu-id="01f00-200">As seguintes versões do jQuery são hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="01f00-200">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-341"></a><span data-ttu-id="01f00-201">jQuery versão 3.4.1</span><span class="sxs-lookup"><span data-stu-id="01f00-201">jQuery version 3.4.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.1.slim.min.map

#### <a name="jquery-version-340"></a><span data-ttu-id="01f00-202">jQuery versão 3.4.0</span><span class="sxs-lookup"><span data-stu-id="01f00-202">jQuery version 3.4.0</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.4.0.slim.min.map

#### <a name="jquery-version-331"></a><span data-ttu-id="01f00-203">jQuery versão 3.3.1</span><span class="sxs-lookup"><span data-stu-id="01f00-203">jQuery version 3.3.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="01f00-204">jQuery versão 3.2.1</span><span class="sxs-lookup"><span data-stu-id="01f00-204">jQuery version 3.2.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="01f00-205">jQuery versão 3.2.0</span><span class="sxs-lookup"><span data-stu-id="01f00-205">jQuery version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="01f00-206">jQuery versão 3.1.1</span><span class="sxs-lookup"><span data-stu-id="01f00-206">jQuery version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="01f00-207">jQuery versão 3.1.0</span><span class="sxs-lookup"><span data-stu-id="01f00-207">jQuery version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="01f00-208">jQuery versão 3.0.0</span><span class="sxs-lookup"><span data-stu-id="01f00-208">jQuery version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="01f00-209">jQuery versão 2.2.4</span><span class="sxs-lookup"><span data-stu-id="01f00-209">jQuery version 2.2.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="01f00-210">jQuery versão 2.2.3</span><span class="sxs-lookup"><span data-stu-id="01f00-210">jQuery version 2.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="01f00-211">jQuery versão 2.2.2</span><span class="sxs-lookup"><span data-stu-id="01f00-211">jQuery version 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="01f00-212">jQuery versão 2.2.1</span><span class="sxs-lookup"><span data-stu-id="01f00-212">jQuery version 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="01f00-213">jQuery versão 2.2.0</span><span class="sxs-lookup"><span data-stu-id="01f00-213">jQuery version 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="01f00-214">jQuery versão 2.1.4</span><span class="sxs-lookup"><span data-stu-id="01f00-214">jQuery version 2.1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="01f00-215">jQuery versão 2.1.3</span><span class="sxs-lookup"><span data-stu-id="01f00-215">jQuery version 2.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="01f00-216">jQuery versão 2.1.2</span><span class="sxs-lookup"><span data-stu-id="01f00-216">jQuery version 2.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="01f00-217">jQuery versão 2.1.1</span><span class="sxs-lookup"><span data-stu-id="01f00-217">jQuery version 2.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="01f00-218">jQuery versão 2.1.0</span><span class="sxs-lookup"><span data-stu-id="01f00-218">jQuery version 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="01f00-219">jQuery versão 2.0.3</span><span class="sxs-lookup"><span data-stu-id="01f00-219">jQuery version 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="01f00-220">jQuery versão 2.0.2</span><span class="sxs-lookup"><span data-stu-id="01f00-220">jQuery version 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="01f00-221">jQuery versão 2.0.1</span><span class="sxs-lookup"><span data-stu-id="01f00-221">jQuery version 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="01f00-222">jQuery versão 2.0.0</span><span class="sxs-lookup"><span data-stu-id="01f00-222">jQuery version 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="01f00-223">jQuery versão 1.12.4</span><span class="sxs-lookup"><span data-stu-id="01f00-223">jQuery version 1.12.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="01f00-224">jQuery versão 1.12.3</span><span class="sxs-lookup"><span data-stu-id="01f00-224">jQuery version 1.12.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="01f00-225">jQuery versão 1.12.2</span><span class="sxs-lookup"><span data-stu-id="01f00-225">jQuery version 1.12.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="01f00-226">jQuery versão 1.12.1</span><span class="sxs-lookup"><span data-stu-id="01f00-226">jQuery version 1.12.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="01f00-227">jQuery versão 1.12.0</span><span class="sxs-lookup"><span data-stu-id="01f00-227">jQuery version 1.12.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="01f00-228">jQuery versão 1.11.3</span><span class="sxs-lookup"><span data-stu-id="01f00-228">jQuery version 1.11.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="01f00-229">jQuery versão 1.11.2</span><span class="sxs-lookup"><span data-stu-id="01f00-229">jQuery version 1.11.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="01f00-230">jQuery versão 1.11.1</span><span class="sxs-lookup"><span data-stu-id="01f00-230">jQuery version 1.11.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="01f00-231">jQuery versão 1.11.0</span><span class="sxs-lookup"><span data-stu-id="01f00-231">jQuery version 1.11.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="01f00-232">jQuery versão 1.10.2</span><span class="sxs-lookup"><span data-stu-id="01f00-232">jQuery version 1.10.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="01f00-233">jQuery versão 1.10.1</span><span class="sxs-lookup"><span data-stu-id="01f00-233">jQuery version 1.10.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="01f00-234">jQuery versão 1.10.0</span><span class="sxs-lookup"><span data-stu-id="01f00-234">jQuery version 1.10.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="01f00-235">jQuery versão 1.9.1</span><span class="sxs-lookup"><span data-stu-id="01f00-235">jQuery version 1.9.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="01f00-236">jQuery versão 1.9.0</span><span class="sxs-lookup"><span data-stu-id="01f00-236">jQuery version 1.9.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="01f00-237">jQuery versão 1.8.3</span><span class="sxs-lookup"><span data-stu-id="01f00-237">jQuery version 1.8.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="01f00-238">jQuery versão 1.8.2</span><span class="sxs-lookup"><span data-stu-id="01f00-238">jQuery version 1.8.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="01f00-239">jQuery versão 1.8.1</span><span class="sxs-lookup"><span data-stu-id="01f00-239">jQuery version 1.8.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="01f00-240">jQuery versão 1.8.0</span><span class="sxs-lookup"><span data-stu-id="01f00-240">jQuery version 1.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="01f00-241">jQuery versão 1.7.2</span><span class="sxs-lookup"><span data-stu-id="01f00-241">jQuery version 1.7.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="01f00-242">jQuery versão 1.7.1</span><span class="sxs-lookup"><span data-stu-id="01f00-242">jQuery version 1.7.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="01f00-243">jQuery versão 1,7</span><span class="sxs-lookup"><span data-stu-id="01f00-243">jQuery version 1.7</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="01f00-244">jQuery versão 1.6.4</span><span class="sxs-lookup"><span data-stu-id="01f00-244">jQuery version 1.6.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="01f00-245">jQuery versão 1.6.3</span><span class="sxs-lookup"><span data-stu-id="01f00-245">jQuery version 1.6.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="01f00-246">jQuery versão 1.6.2</span><span class="sxs-lookup"><span data-stu-id="01f00-246">jQuery version 1.6.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="01f00-247">jQuery versão 1.6.1</span><span class="sxs-lookup"><span data-stu-id="01f00-247">jQuery version 1.6.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="01f00-248">jQuery versão 1,6</span><span class="sxs-lookup"><span data-stu-id="01f00-248">jQuery version 1.6</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="01f00-249">jQuery versão 1.5.2</span><span class="sxs-lookup"><span data-stu-id="01f00-249">jQuery version 1.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="01f00-250">jQuery versão 1.5.1</span><span class="sxs-lookup"><span data-stu-id="01f00-250">jQuery version 1.5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="01f00-251">jQuery versão 1,5</span><span class="sxs-lookup"><span data-stu-id="01f00-251">jQuery version 1.5</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="01f00-252">jQuery versão 1.4.4</span><span class="sxs-lookup"><span data-stu-id="01f00-252">jQuery version 1.4.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="01f00-253">jQuery versão 1.4.3</span><span class="sxs-lookup"><span data-stu-id="01f00-253">jQuery version 1.4.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="01f00-254">jQuery versão 1.4.2</span><span class="sxs-lookup"><span data-stu-id="01f00-254">jQuery version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="01f00-255">jQuery versão 1.4.1</span><span class="sxs-lookup"><span data-stu-id="01f00-255">jQuery version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="01f00-256">jQuery versão 1,4</span><span class="sxs-lookup"><span data-stu-id="01f00-256">jQuery version 1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="01f00-257">jQuery versão 1.3.2</span><span class="sxs-lookup"><span data-stu-id="01f00-257">jQuery version 1.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="01f00-258">Versões de migração do jQuery no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-258">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="01f00-259">As seguintes versões da migração do jQuery são hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="01f00-259">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="01f00-260">jQuery migrar versão 3.0.0</span><span class="sxs-lookup"><span data-stu-id="01f00-260">jQuery Migrate version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="01f00-261">Migrar versão 1.2.1 do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-261">jQuery Migrate version 1.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="01f00-262">jQuery migrar versão 1.2.0</span><span class="sxs-lookup"><span data-stu-id="01f00-262">jQuery Migrate version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="01f00-263">Migrar do jQuery versão 1.1.1</span><span class="sxs-lookup"><span data-stu-id="01f00-263">jQuery Migrate version 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="01f00-264">Migrar do jQuery versão 1.1.0</span><span class="sxs-lookup"><span data-stu-id="01f00-264">jQuery Migrate version 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="01f00-265">Migrar do jQuery versão 1.0.0</span><span class="sxs-lookup"><span data-stu-id="01f00-265">jQuery Migrate version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="01f00-266">Versões da interface do usuário do jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-266">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="01f00-267">As seguintes versões da biblioteca da interface do usuário do jQuery são hospedadas nesta CDN.</span><span class="sxs-lookup"><span data-stu-id="01f00-267">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="01f00-268">Clique em cada link para ver a lista real de arquivos.</span><span class="sxs-lookup"><span data-stu-id="01f00-268">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="01f00-269">1.12.1 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-269">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-270">1.12.0 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-270">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-271">1.11.4 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-271">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-272">1.11.3 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-272">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-273">1.11.2 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-273">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-274">1.11.1 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-274">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-275">1.11.0 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-275">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-276">1.10.4 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-276">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-277">1.10.3 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-277">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-278">1.10.2 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-278">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-279">1.10.1 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-279">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-280">1.10.0 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-280">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-281">1.9.2 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-281">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-282">1.9.1 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-282">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-283">1.9.0 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-283">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-284">1.8.24 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-284">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-285">1.8.23 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-285">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-286">1.8.22 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-286">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-287">1.8.21 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-287">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-288">1.8.20 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-288">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-289">1.8.19 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-289">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-290">1.8.18 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-290">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-291">1.8.17 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-291">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-292">1.8.16 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-292">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-293">1.8.15 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-293">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-294">1.8.14 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-294">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-295">1.8.13 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-295">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-296">1.8.12 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-296">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-297">1.8.11 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-297">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-298">1.8.10 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-298">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-299">1.8.9 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-299">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-300">1.8.8 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-300">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-301">1.8.7 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-301">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-302">1.8.6 da IU do jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-302">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-303">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="01f00-303">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="01f00-304">Versões de validação do jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-304">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="01f00-305">As seguintes versões do plug-in de [validação jQuery](https://jqueryvalidation.org/ "Plug-in de validação jQuery") são hospedadas nessa CDN.</span><span class="sxs-lookup"><span data-stu-id="01f00-305">The following releases of the [jQuery Validation](https://jqueryvalidation.org/ "jQuery Validation Plugin") plugin are hosted on this CDN.</span></span> <span data-ttu-id="01f00-306">Clique em cada link para ver a lista real de arquivos.</span><span class="sxs-lookup"><span data-stu-id="01f00-306">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="01f00-307">1.19.1 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-307">jQuery Validate 1.19.1</span></span>](jquery-validate/cdnjqueryvalidate1191.md "1\.19.1 de validação jQuery")
- [<span data-ttu-id="01f00-308">1.19.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-308">jQuery Validate 1.19.0</span></span>](jquery-validate/cdnjqueryvalidate1190.md "1\.19.0 de validação jQuery")
- [<span data-ttu-id="01f00-309">1.17.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-309">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "1\.17.0 de validação jQuery")
- [<span data-ttu-id="01f00-310">1.16.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-310">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "Validação do jQuery 1.16.0")
- [<span data-ttu-id="01f00-311">1.15.1 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-311">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "Validação do jQuery 1.15.1")
- [<span data-ttu-id="01f00-312">1.15.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-312">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "Validação do jQuery 1.15.0")
- [<span data-ttu-id="01f00-313">1.14.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-313">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "Validação do jQuery 1.14.0")
- [<span data-ttu-id="01f00-314">1.13.1 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-314">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "Validação do jQuery 1.13.1")
- [<span data-ttu-id="01f00-315">1.13.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-315">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "Validação do jQuery 1.13.0")
- [<span data-ttu-id="01f00-316">1.12.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-316">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "Validação do jQuery 1.12.0")
- [<span data-ttu-id="01f00-317">1.11.1 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-317">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "Validação do jQuery 1.11.1")
- [<span data-ttu-id="01f00-318">1.11.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-318">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "Validação do jQuery 1.11.0")
- [<span data-ttu-id="01f00-319">1.10.0 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-319">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "Validação do jQuery 1.10.0")
- [<span data-ttu-id="01f00-320">jQuery validar 1,9</span><span class="sxs-lookup"><span data-stu-id="01f00-320">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate versão 1.9")
- [<span data-ttu-id="01f00-321">1.8.1 de validação jQuery</span><span class="sxs-lookup"><span data-stu-id="01f00-321">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate versão 1.8.1")
- [<span data-ttu-id="01f00-322">jQuery validar 1,8</span><span class="sxs-lookup"><span data-stu-id="01f00-322">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate versão 1.8")
- [<span data-ttu-id="01f00-323">jQuery validar 1,7</span><span class="sxs-lookup"><span data-stu-id="01f00-323">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate versão 1.7")
- [<span data-ttu-id="01f00-324">Validar jQuery 1.6</span><span class="sxs-lookup"><span data-stu-id="01f00-324">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "Validar jQuery 1.6")
- [<span data-ttu-id="01f00-325">Validar jQuery 1.5.5</span><span class="sxs-lookup"><span data-stu-id="01f00-325">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "Validar jQuery 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="01f00-326">Versões móveis do jQuery no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-326">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="01f00-327">As seguintes versões da biblioteca jQuery Mobile são hospedadas nesta CDN.</span><span class="sxs-lookup"><span data-stu-id="01f00-327">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="01f00-328">Clique em cada link para ver a lista real de arquivos.</span><span class="sxs-lookup"><span data-stu-id="01f00-328">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="01f00-329">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="01f00-329">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-330">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="01f00-330">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-331">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="01f00-331">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-332">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="01f00-332">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-333">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="01f00-333">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-334">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="01f00-334">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-335">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="01f00-335">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-336">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="01f00-336">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-337">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="01f00-337">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-338">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="01f00-338">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-339">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="01f00-339">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-340">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="01f00-340">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-341">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="01f00-341">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-342">jQuery Mobile 1,0</span><span class="sxs-lookup"><span data-stu-id="01f00-342">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-343">jQuery Mobile 1,0 RC 2</span><span class="sxs-lookup"><span data-stu-id="01f00-343">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-344">jQuery Mobile 1,0 RC 1</span><span class="sxs-lookup"><span data-stu-id="01f00-344">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 na CDN do Microsoft Ajax")
- [<span data-ttu-id="01f00-345">jQuery Mobile 1,0 Beta 3</span><span class="sxs-lookup"><span data-stu-id="01f00-345">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 na CDN do Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="01f00-346">Versões de modelos jQuery no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-346">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="01f00-347">As seguintes versões do plug-in do jQuery Templates são hospedadas nesta CDN.</span><span class="sxs-lookup"><span data-stu-id="01f00-347">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="01f00-348">Clique em cada link para ver a lista real de arquivos.</span><span class="sxs-lookup"><span data-stu-id="01f00-348">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="01f00-349">Modelos jQuery Beta 1</span><span class="sxs-lookup"><span data-stu-id="01f00-349">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "Modelos jQuery Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="01f00-350">Versões de ciclo jQuery na CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-350">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="01f00-351">As seguintes versões do plug-in do jQuery Cycle são hospedadas nesta CDN.</span><span class="sxs-lookup"><span data-stu-id="01f00-351">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="01f00-352">Clique em cada link para ver a lista real de arquivos.</span><span class="sxs-lookup"><span data-stu-id="01f00-352">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="01f00-353">jQuery Cycle 2.99</span><span class="sxs-lookup"><span data-stu-id="01f00-353">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery Cycle 2.99")
- [<span data-ttu-id="01f00-354">jQuery Cycle 2.94</span><span class="sxs-lookup"><span data-stu-id="01f00-354">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Cycle 2.94")
- [<span data-ttu-id="01f00-355">jQuery Cycle 2.88</span><span class="sxs-lookup"><span data-stu-id="01f00-355">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery Cycle 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="01f00-356">Versões do jQuery DataTables no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-356">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="01f00-357">As seguintes versões do plug-in jQuery DataTables estão hospedadas nessa CDN.</span><span class="sxs-lookup"><span data-stu-id="01f00-357">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="01f00-358">Clique em cada link para ver a lista real de arquivos.</span><span class="sxs-lookup"><span data-stu-id="01f00-358">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="01f00-359">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="01f00-359">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="01f00-360">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="01f00-360">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="01f00-361">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="01f00-361">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="01f00-362">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="01f00-362">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="01f00-363">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="01f00-363">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="01f00-364">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="01f00-364">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="01f00-365">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="01f00-365">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="01f00-366">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="01f00-366">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="01f00-367">Versões do Modernizr na CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-367">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="01f00-368">As seguintes versões do [Modernizr](http://www.modernizr.com "Modernizr") estão hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="01f00-368">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-3.5.0.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="01f00-369">Versões JSHint no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-369">JSHint Releases on the CDN</span></span>

<span data-ttu-id="01f00-370">As seguintes versões do [JSHint](http://www.jshint.com "JSHint") são hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="01f00-370">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="01f00-371">Versões do Knockout no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-371">Knockout Releases on the CDN</span></span>

<span data-ttu-id="01f00-372">As seguintes versões do [Knockout](http://www.knockoutjs.com "Separação") são hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="01f00-372">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="01f00-373">Globalizar versões no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-373">Globalize Releases on the CDN</span></span>

<span data-ttu-id="01f00-374">As seguintes versões de [globalizate](https://github.com/jquery/globalize "Globalizar") são hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="01f00-374">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="01f00-375">Globalizar versão 1.0.0</span><span class="sxs-lookup"><span data-stu-id="01f00-375">Globalize version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="01f00-376">Globalizar versão 0.1.1</span><span class="sxs-lookup"><span data-stu-id="01f00-376">Globalize version 0.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="01f00-377">todas as culturas</span><span class="sxs-lookup"><span data-stu-id="01f00-377">all cultures</span></span>
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="01f00-378">Substitua "{Culture-Code}" pelo código de cultura desejado, por exemplo, globalizate. Culture. en-GB. js = = Microsoft files no CDN = = essas bibliotecas foram carregadas pela Microsoft.</span><span class="sxs-lookup"><span data-stu-id="01f00-378">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="01f00-379">Responder a versões no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-379">Respond Releases on the CDN</span></span>

<span data-ttu-id="01f00-380">As seguintes versões de [resposta](https://github.com/scottjehl/Respond "Responder") estão hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="01f00-380">The following releases of [Respond](https://github.com/scottjehl/Respond "Respond") are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="01f00-381">Responder à versão 1.4.2</span><span class="sxs-lookup"><span data-stu-id="01f00-381">Respond version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="01f00-382">Responder à versão 1.4.1</span><span class="sxs-lookup"><span data-stu-id="01f00-382">Respond version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="01f00-383">Responder à versão 1.4.0</span><span class="sxs-lookup"><span data-stu-id="01f00-383">Respond version 1.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="01f00-384">Responder à versão 1.3.0</span><span class="sxs-lookup"><span data-stu-id="01f00-384">Respond version 1.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="01f00-385">Responder à versão 1.2.0</span><span class="sxs-lookup"><span data-stu-id="01f00-385">Respond version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="01f00-386">Versões de Bootstrap no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-386">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="01f00-387">As seguintes versões da inicialização do [GetBootstrap.com](http://getbootstrap.com "getbootstrap.com") são hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="01f00-387">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-441"></a><span data-ttu-id="01f00-388">Versão de inicialização 4.4.1</span><span class="sxs-lookup"><span data-stu-id="01f00-388">Bootstrap version 4.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.4.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-431"></a><span data-ttu-id="01f00-389">Versão de Bootstrap 4.3.1</span><span class="sxs-lookup"><span data-stu-id="01f00-389">Bootstrap version 4.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.3.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-421"></a><span data-ttu-id="01f00-390">Versão de inicialização 4.2.1</span><span class="sxs-lookup"><span data-stu-id="01f00-390">Bootstrap version 4.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.2.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-411"></a><span data-ttu-id="01f00-391">Versão 4.1.1 do bootstrap</span><span class="sxs-lookup"><span data-stu-id="01f00-391">Bootstrap version 4.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-400"></a><span data-ttu-id="01f00-392">Versão de inicialização 4.0.0</span><span class="sxs-lookup"><span data-stu-id="01f00-392">Bootstrap version 4.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.bundle.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-341"></a><span data-ttu-id="01f00-393">Versão de inicialização 3.4.1</span><span class="sxs-lookup"><span data-stu-id="01f00-393">Bootstrap version 3.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.1/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-340"></a><span data-ttu-id="01f00-394">Versão de inicialização 3.4.0</span><span class="sxs-lookup"><span data-stu-id="01f00-394">Bootstrap version 3.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.4.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="01f00-395">Versão de inicialização 3.3.7</span><span class="sxs-lookup"><span data-stu-id="01f00-395">Bootstrap version 3.3.7</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="01f00-396">Versão de inicialização 3.3.6</span><span class="sxs-lookup"><span data-stu-id="01f00-396">Bootstrap version 3.3.6</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="01f00-397">Versão de inicialização 3.3.5</span><span class="sxs-lookup"><span data-stu-id="01f00-397">Bootstrap version 3.3.5</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="01f00-398">Versão de inicialização 3.3.4</span><span class="sxs-lookup"><span data-stu-id="01f00-398">Bootstrap version 3.3.4</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="01f00-399">Versão de inicialização 3.3.2</span><span class="sxs-lookup"><span data-stu-id="01f00-399">Bootstrap version 3.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="01f00-400">Versão de inicialização 3.3.1</span><span class="sxs-lookup"><span data-stu-id="01f00-400">Bootstrap version 3.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="01f00-401">Versão de inicialização 3.3.0</span><span class="sxs-lookup"><span data-stu-id="01f00-401">Bootstrap version 3.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="01f00-402">Versão de inicialização 3.2.0</span><span class="sxs-lookup"><span data-stu-id="01f00-402">Bootstrap version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="01f00-403">Versão de inicialização 3.1.1</span><span class="sxs-lookup"><span data-stu-id="01f00-403">Bootstrap version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="01f00-404">Versão de inicialização 3.1.0</span><span class="sxs-lookup"><span data-stu-id="01f00-404">Bootstrap version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="01f00-405">Versão de inicialização 3.0.3</span><span class="sxs-lookup"><span data-stu-id="01f00-405">Bootstrap version 3.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="01f00-406">Versão 3.0.2 do bootstrap</span><span class="sxs-lookup"><span data-stu-id="01f00-406">Bootstrap version 3.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="01f00-407">Versão de Bootstrap 3.0.1</span><span class="sxs-lookup"><span data-stu-id="01f00-407">Bootstrap version 3.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="01f00-408">Versão de inicialização 3.0.0</span><span class="sxs-lookup"><span data-stu-id="01f00-408">Bootstrap version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="01f00-409">Versão de inicialização 2.3.2</span><span class="sxs-lookup"><span data-stu-id="01f00-409">Bootstrap version 2.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="01f00-410">Versão do bootstrap 2.3.1</span><span class="sxs-lookup"><span data-stu-id="01f00-410">Bootstrap version 2.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="01f00-411">Versões TouchCarousel de Bootstrap no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-411">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="01f00-412">As seguintes versões do [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") versões de inicialização do TouchCarousel são hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="01f00-412">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="01f00-413">Bootstrap TouchCarousel versão 0.8.0</span><span class="sxs-lookup"><span data-stu-id="01f00-413">Bootstrap TouchCarousel version 0.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="01f00-414">Versões do martelo. js na CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-414">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="01f00-415">As seguintes versões do [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") versões do martelo. js são hospedadas na CDN:</span><span class="sxs-lookup"><span data-stu-id="01f00-415">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="01f00-416">Martelo. js versão 2.0.4</span><span class="sxs-lookup"><span data-stu-id="01f00-416">Hammer.js version 2.0.4</span></span>

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="01f00-417">Versões Web Forms e AJAX do ASP.NET no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-417">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="01f00-418">As seguintes versões da biblioteca do ASP.NET AJAX são hospedadas na CDN.</span><span class="sxs-lookup"><span data-stu-id="01f00-418">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="01f00-419">Clique em cada link para ver a lista real de arquivos.</span><span class="sxs-lookup"><span data-stu-id="01f00-419">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="01f00-420">ASP.NET Web Forms e Ajax versão 4.5.2</span><span class="sxs-lookup"><span data-stu-id="01f00-420">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "Web Forms do ASP.NET e Ajax 4.5.2")
- [<span data-ttu-id="01f00-421">ASP.NET Web Forms e Ajax versão 4</span><span class="sxs-lookup"><span data-stu-id="01f00-421">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "ASP.NET Web Forms e Ajax 4")
- [<span data-ttu-id="01f00-422">ASP.NET AJAX versão 3,5</span><span class="sxs-lookup"><span data-stu-id="01f00-422">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="01f00-423">Versões MVC do ASP.NET no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-423">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="01f00-424">Os seguintes arquivos JavaScript do ASP.NET MVC são hospedados nesta CDN:</span><span class="sxs-lookup"><span data-stu-id="01f00-424">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="01f00-425">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="01f00-425">ASP.NET MVC 5.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="01f00-426">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="01f00-426">ASP.NET MVC 5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="01f00-427">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="01f00-427">ASP.NET MVC 5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="01f00-428">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="01f00-428">ASP.NET MVC 4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="01f00-429">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="01f00-429">ASP.NET MVC 3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.min.js 
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.js 
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="01f00-430">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="01f00-430">ASP.NET MVC 2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="01f00-431">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="01f00-431">ASP.NET MVC 1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="01f00-432">Versões do Signalr ASP.NET no CDN</span><span class="sxs-lookup"><span data-stu-id="01f00-432">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="01f00-433">Os seguintes arquivos JavaScript do Signalr ASP.NET são hospedados nesta CDN:</span><span class="sxs-lookup"><span data-stu-id="01f00-433">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="01f00-434">ASP.NET Signalr 2.2.2</span><span class="sxs-lookup"><span data-stu-id="01f00-434">ASP.NET SignalR 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="01f00-435">ASP.NET Signalr 2.2.1</span><span class="sxs-lookup"><span data-stu-id="01f00-435">ASP.NET SignalR 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="01f00-436">ASP.NET Signalr 2.2.0</span><span class="sxs-lookup"><span data-stu-id="01f00-436">ASP.NET SignalR 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="01f00-437">ASP.NET Signalr 2.1.0</span><span class="sxs-lookup"><span data-stu-id="01f00-437">ASP.NET SignalR 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="01f00-438">ASP.NET Signalr 2.0.3</span><span class="sxs-lookup"><span data-stu-id="01f00-438">ASP.NET SignalR 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="01f00-439">ASP.NET Signalr 2.0.2</span><span class="sxs-lookup"><span data-stu-id="01f00-439">ASP.NET SignalR 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="01f00-440">ASP.NET Signalr 2.0.1</span><span class="sxs-lookup"><span data-stu-id="01f00-440">ASP.NET SignalR 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="01f00-441">ASP.NET Signalr 2.0.0</span><span class="sxs-lookup"><span data-stu-id="01f00-441">ASP.NET SignalR 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="01f00-442">ASP.NET Signalr 1.1.3</span><span class="sxs-lookup"><span data-stu-id="01f00-442">ASP.NET SignalR 1.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="01f00-443">Sinalização ASP.NET 1.1.2</span><span class="sxs-lookup"><span data-stu-id="01f00-443">ASP.NET SignalR 1.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="01f00-444">ASP.NET Signalr 1.1.1</span><span class="sxs-lookup"><span data-stu-id="01f00-444">ASP.NET SignalR 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="01f00-445">ASP.NET Signalr 1.1.0</span><span class="sxs-lookup"><span data-stu-id="01f00-445">ASP.NET SignalR 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="01f00-446">ASP.NET Signalr 1.0.1</span><span class="sxs-lookup"><span data-stu-id="01f00-446">ASP.NET SignalR 1.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="01f00-447">Para obter informações sobre os termos de uso da CDN, consulte [termos de uso da CDN do Microsoft Ajax](https://www.asp.net/terms-of-use "Termos de uso da CDN do Microsoft Ajax").</span><span class="sxs-lookup"><span data-stu-id="01f00-447">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
