---
uid: web-pages/overview/ui-layouts-and-themes/10-working-with-video
title: Exibindo vídeo em um site Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este capítulo explica como exibir vídeo em um Páginas da Web do ASP.NET com sintaxe Razor página.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 332fb3da-e2a5-460d-bb90-dd911e1e2c95
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/10-working-with-video
msc.type: authoredcontent
ms.openlocfilehash: 516d46f38ce8910209f4207c474b0404bf012950
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628943"
---
# <a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="aa268-103">Exibindo vídeo em um site Páginas da Web do ASP.NET (Razor)</span><span class="sxs-lookup"><span data-stu-id="aa268-103">Displaying Video in an ASP.NET Web Pages (Razor) Site</span></span>

<span data-ttu-id="aa268-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="aa268-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="aa268-105">Este artigo explica como usar um player de vídeo (mídia) em um site Páginas da Web do ASP.NET (Razor) para permitir que os usuários exibam vídeos armazenados no site.</span><span class="sxs-lookup"><span data-stu-id="aa268-105">This article explains how to use a video (media) player in an ASP.NET Web Pages (Razor) website to let users view videos that are stored on the site.</span></span> <span data-ttu-id="aa268-106">Páginas da Web do ASP.NET com sintaxe Razor permite reproduzir vídeos Flash ( *. swf*), Media Player ( *. wmv*) e Silverlight ( *. xap*).</span><span class="sxs-lookup"><span data-stu-id="aa268-106">ASP.NET Web Pages with Razor syntax lets you play Flash (*.swf*), Media Player (*.wmv*), and Silverlight (*.xap*) videos.</span></span>
> 
> <span data-ttu-id="aa268-107">O que você aprenderá:</span><span class="sxs-lookup"><span data-stu-id="aa268-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="aa268-108">Como escolher um player de vídeo.</span><span class="sxs-lookup"><span data-stu-id="aa268-108">How to choose a video player.</span></span>
> - <span data-ttu-id="aa268-109">Como adicionar vídeo a uma página da Web.</span><span class="sxs-lookup"><span data-stu-id="aa268-109">How to add video to a web page.</span></span>
> - <span data-ttu-id="aa268-110">Como definir atributos de player de vídeo.</span><span class="sxs-lookup"><span data-stu-id="aa268-110">How to set video player attributes.</span></span>
> 
> <span data-ttu-id="aa268-111">Estes são os recursos de páginas do Razor do ASP.NET apresentados no artigo:</span><span class="sxs-lookup"><span data-stu-id="aa268-111">These are the ASP.NET Razor pages features introduced in the article:</span></span>
> 
> - <span data-ttu-id="aa268-112">O auxiliar de `Video`.</span><span class="sxs-lookup"><span data-stu-id="aa268-112">The `Video` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="aa268-113">Versões de software usadas no tutorial</span><span class="sxs-lookup"><span data-stu-id="aa268-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="aa268-114">Páginas da Web do ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="aa268-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="aa268-115">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="aa268-115">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="aa268-116">Este tutorial também funciona com o WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="aa268-116">This tutorial also works with WebMatrix 3.</span></span>

## <a name="introduction"></a><span data-ttu-id="aa268-117">Introdução</span><span class="sxs-lookup"><span data-stu-id="aa268-117">Introduction</span></span>

<span data-ttu-id="aa268-118">Talvez você queira exibir um vídeo em seu site.</span><span class="sxs-lookup"><span data-stu-id="aa268-118">You might want to display a video on your site.</span></span> <span data-ttu-id="aa268-119">Uma maneira de fazer isso é vincular a um site que já tem o vídeo, como o YouTube.</span><span class="sxs-lookup"><span data-stu-id="aa268-119">One way to do that is to link to a site that already has the video, like YouTube.</span></span> <span data-ttu-id="aa268-120">Se você quiser inserir um vídeo desses sites diretamente em suas próprias páginas, você geralmente pode obter marcação HTML do site e, em seguida, copiá-lo para sua página.</span><span class="sxs-lookup"><span data-stu-id="aa268-120">If you want to embed a video from these sites directly in your own pages, you can usually get HTML markup from the site and then copy it into your page.</span></span> <span data-ttu-id="aa268-121">Por exemplo, o exemplo a seguir mostra como inserir um vídeo do YouTube:</span><span class="sxs-lookup"><span data-stu-id="aa268-121">For example, the following example shows how to embed a YouTube video:</span></span>

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

<span data-ttu-id="aa268-122">Se você quiser reproduzir um vídeo que esteja no seu próprio site (não em um site de compartilhamento de vídeo público), não poderá vincular diretamente a ele usando marcação inserida como esta.</span><span class="sxs-lookup"><span data-stu-id="aa268-122">If you want to play a video that's on your own website (not on a public video-sharing site), you can't directly link to it using embedded markup like this.</span></span> <span data-ttu-id="aa268-123">No entanto, você pode reproduzir vídeos de seu site usando o auxiliar de `Video`, que renderiza um player de mídia diretamente em uma página.</span><span class="sxs-lookup"><span data-stu-id="aa268-123">However, you can play videos from your site by using the `Video` helper, which renders a media player directly in a page.</span></span>

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a><span data-ttu-id="aa268-124">Escolhendo um player de vídeo</span><span class="sxs-lookup"><span data-stu-id="aa268-124">Choosing a Video Player</span></span>

<span data-ttu-id="aa268-125">Há muitos formatos para arquivos de vídeo, e cada formato geralmente requer um player diferente e uma maneira diferente de configurar o Player.</span><span class="sxs-lookup"><span data-stu-id="aa268-125">There are lots of formats for video files, and each format typically requires a different player and a different way to configure the player.</span></span> <span data-ttu-id="aa268-126">Em páginas Razor do ASP.NET, você pode reproduzir um vídeo em uma página da Web usando o auxiliar `Video`.</span><span class="sxs-lookup"><span data-stu-id="aa268-126">In ASP.NET Razor pages, you can play a video in a web page using the `Video` helper.</span></span> <span data-ttu-id="aa268-127">O auxiliar de `Video` simplifica o processo de inserção de vídeos em uma página da Web, pois gera automaticamente os `object` e `embed` elementos HTML que normalmente são usados para adicionar vídeo à página.</span><span class="sxs-lookup"><span data-stu-id="aa268-127">The `Video` helper simplifies the process of embedding videos in a web page because it automatically generates the `object` and `embed` HTML elements that are normally used to add video to the page.</span></span>

<span data-ttu-id="aa268-128">O auxiliar de `Video` dá suporte aos seguintes players de mídia:</span><span class="sxs-lookup"><span data-stu-id="aa268-128">The `Video` helper supports the following media players:</span></span>

- <span data-ttu-id="aa268-129">Adobe Flash</span><span class="sxs-lookup"><span data-stu-id="aa268-129">Adobe Flash</span></span>
- <span data-ttu-id="aa268-130">Windows MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="aa268-130">Windows MediaPlayer</span></span>
- <span data-ttu-id="aa268-131">Microsoft Silverlight</span><span class="sxs-lookup"><span data-stu-id="aa268-131">Microsoft Silverlight</span></span>

### <a name="the-flash-player"></a><span data-ttu-id="aa268-132">O Flash Player</span><span class="sxs-lookup"><span data-stu-id="aa268-132">The Flash Player</span></span>

<span data-ttu-id="aa268-133">O `Flash` Player do auxiliar de `Video` permite reproduzir vídeos Flash (arquivos *. swf* ) em uma página da Web.</span><span class="sxs-lookup"><span data-stu-id="aa268-133">The `Flash` player of the `Video` helper let you play Flash videos (*.swf* files) in a web page.</span></span> <span data-ttu-id="aa268-134">No mínimo, você precisa fornecer um caminho para o arquivo de vídeo.</span><span class="sxs-lookup"><span data-stu-id="aa268-134">At a minimum, you have to provide a path to the video file.</span></span> <span data-ttu-id="aa268-135">Se você não especificar nada, exceto o caminho, o Player usará valores padrão definidos pela versão atual do flash.</span><span class="sxs-lookup"><span data-stu-id="aa268-135">If you specify nothing but the path, the player uses default values that are set by the current version of Flash.</span></span> <span data-ttu-id="aa268-136">As configurações padrão típicas são:</span><span class="sxs-lookup"><span data-stu-id="aa268-136">Typical default settings are:</span></span>

- <span data-ttu-id="aa268-137">O vídeo é exibido usando sua largura e altura padrão e sem uma cor de plano de fundo.</span><span class="sxs-lookup"><span data-stu-id="aa268-137">The video is displayed using its default width and height and without a background color.</span></span>
- <span data-ttu-id="aa268-138">O vídeo é reproduzido automaticamente quando a página é carregada.</span><span class="sxs-lookup"><span data-stu-id="aa268-138">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="aa268-139">O vídeo faz o loop continuamente até ser interrompido explicitamente.</span><span class="sxs-lookup"><span data-stu-id="aa268-139">The video loops continuously until it's explicitly stopped.</span></span>
- <span data-ttu-id="aa268-140">O vídeo é dimensionado para mostrar todo o vídeo, em vez de cortar o vídeo para se ajustar a um tamanho específico.</span><span class="sxs-lookup"><span data-stu-id="aa268-140">The video is scaled to show all of the video, rather than cropping the video to fit a specific size.</span></span>
- <span data-ttu-id="aa268-141">O vídeo é reproduzido em uma janela.</span><span class="sxs-lookup"><span data-stu-id="aa268-141">The video plays in a window.</span></span>

### <a name="the-mediaplayer-player"></a><span data-ttu-id="aa268-142">O jogador do MediaPlayer</span><span class="sxs-lookup"><span data-stu-id="aa268-142">The MediaPlayer Player</span></span>

<span data-ttu-id="aa268-143">O `MediaPlayer` Player do auxiliar do `Video` permite que você reproduza vídeos do Windows Media (arquivos *. wmv* ), áudio do Windows Media (arquivos *. WMA* ) e MP3 (arquivos *. mp3* ) em uma página da Web.</span><span class="sxs-lookup"><span data-stu-id="aa268-143">The `MediaPlayer` player of the `Video` helper lets you play Windows Media videos (*.wmv* files), Windows Media audio (*.wma* files), and MP3 (*.mp3* files) in a web page.</span></span> <span data-ttu-id="aa268-144">Você deve incluir o caminho do arquivo de mídia para reprodução; todos os outros parâmetros são opcionais.</span><span class="sxs-lookup"><span data-stu-id="aa268-144">You must include path of the media file to play; all other parameters are optional.</span></span> <span data-ttu-id="aa268-145">Se você especificar apenas um caminho, o Player usará as configurações padrão definidas pela versão atual do MediaPlayer, como:</span><span class="sxs-lookup"><span data-stu-id="aa268-145">If you specify only a path, the player uses default settings set by the current version of MediaPlayer, such as:</span></span>

- <span data-ttu-id="aa268-146">O vídeo é exibido usando sua largura e altura padrão.</span><span class="sxs-lookup"><span data-stu-id="aa268-146">The video is displayed using its default width and height.</span></span>
- <span data-ttu-id="aa268-147">O vídeo é reproduzido automaticamente quando a página é carregada.</span><span class="sxs-lookup"><span data-stu-id="aa268-147">The video plays automatically when the page loads.</span></span>
- <span data-ttu-id="aa268-148">O vídeo é reproduzido uma vez (não faz loop).</span><span class="sxs-lookup"><span data-stu-id="aa268-148">The video plays once (it doesn't loop).</span></span>
- <span data-ttu-id="aa268-149">O Player exibe o conjunto completo de controles na interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="aa268-149">The player displays the full set of controls in the user interface.</span></span>
- <span data-ttu-id="aa268-150">O vídeo é reproduzido em uma janela.</span><span class="sxs-lookup"><span data-stu-id="aa268-150">The video plays in a window.</span></span>

### <a name="the-silverlight-player"></a><span data-ttu-id="aa268-151">O player do Silverlight</span><span class="sxs-lookup"><span data-stu-id="aa268-151">The Silverlight Player</span></span>

<span data-ttu-id="aa268-152">O `Silverlight` Player do auxiliar do `Video` permite reproduzir vídeo do Windows Media (arquivos *. wmv* ), áudio do Windows Media (arquivos *. WMA* ) e MP3 (arquivos *. mp3* ).</span><span class="sxs-lookup"><span data-stu-id="aa268-152">The `Silverlight` player of the `Video` helper lets you play Windows Media Video (*.wmv* files), Windows Media Audio (*.wma* files), and MP3 (*.mp3* files).</span></span> <span data-ttu-id="aa268-153">Você deve definir o parâmetro Path para apontar para um pacote de aplicativos baseado em Silverlight (arquivo *. xap* ).</span><span class="sxs-lookup"><span data-stu-id="aa268-153">You must set the path parameter to point to a Silverlight-based application package (*.xap* file).</span></span> <span data-ttu-id="aa268-154">Você também deve definir os parâmetros Width e Height.</span><span class="sxs-lookup"><span data-stu-id="aa268-154">You also must set the width and height parameters.</span></span> <span data-ttu-id="aa268-155">Todos os outros parâmetros são opcionais.</span><span class="sxs-lookup"><span data-stu-id="aa268-155">All other parameters are optional.</span></span> <span data-ttu-id="aa268-156">Ao usar o player do Silverlight para vídeo, se você definir apenas os parâmetros necessários, o player do Silverlight exibirá o vídeo sem uma cor de plano de fundo.</span><span class="sxs-lookup"><span data-stu-id="aa268-156">When you use the Silverlight player for video, if you set only the required parameters, the Silverlight player displays the video without a background color.</span></span>

> [!NOTE]
> <span data-ttu-id="aa268-157">Caso você ainda não conheça o Silverlight: o arquivo *. xap* é um arquivo compactado que contém instruções de layout em um arquivo *. XAML* , código gerenciado em assemblies e recursos opcionais.</span><span class="sxs-lookup"><span data-stu-id="aa268-157">In case you don't already know Silverlight: the *.xap* file is a compressed file that contains layout instructions in a *.xaml* file, managed code in assemblies, and optional resources.</span></span> <span data-ttu-id="aa268-158">Você pode criar um arquivo *. xap* no Visual Studio como um projeto de aplicativo do Silverlight.</span><span class="sxs-lookup"><span data-stu-id="aa268-158">You can create a *.xap* file in Visual Studio as a Silverlight application project.</span></span>

<span data-ttu-id="aa268-159">O player de vídeo `Silverlight` usa as configurações que você fornece para o Player e as configurações que são fornecidas no arquivo *. xap* .</span><span class="sxs-lookup"><span data-stu-id="aa268-159">The `Silverlight` video player uses both the settings that you provide for the player and the settings that are provided in the *.xap* file.</span></span>

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a><span data-ttu-id="aa268-160">Tipos de MIME</span><span class="sxs-lookup"><span data-stu-id="aa268-160">MIME Types</span></span>
> 
> <span data-ttu-id="aa268-161">Quando um navegador baixa um arquivo, o navegador garante que o tipo de arquivo corresponda ao tipo MIME especificado para o documento que está sendo processado.</span><span class="sxs-lookup"><span data-stu-id="aa268-161">When a browser downloads a file, the browser makes sure that the file type matches the MIME type that's specified for the document that's being rendered.</span></span> <span data-ttu-id="aa268-162">O tipo de MIME é o tipo de conteúdo ou o tipo de mídia de um arquivo.</span><span class="sxs-lookup"><span data-stu-id="aa268-162">The MIME type is the content type or media type of a file.</span></span> <span data-ttu-id="aa268-163">O auxiliar de `Video` usa os seguintes tipos de MIME:</span><span class="sxs-lookup"><span data-stu-id="aa268-163">The `Video` helper uses the following MIME types:</span></span>
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`

<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a><span data-ttu-id="aa268-164">Reprodução de vídeos Flash (. swf)</span><span class="sxs-lookup"><span data-stu-id="aa268-164">Playing Flash (.swf) Videos</span></span>

<span data-ttu-id="aa268-165">Este procedimento mostra como reproduzir um vídeo Flash chamado *Sample. swf*.</span><span class="sxs-lookup"><span data-stu-id="aa268-165">This procedure shows you how to play a Flash video named *sample.swf*.</span></span> <span data-ttu-id="aa268-166">O procedimento pressupõe que você tem uma pasta chamada *mídia* no seu site e que o arquivo *. swf* está nessa pasta.</span><span class="sxs-lookup"><span data-stu-id="aa268-166">The procedure assumes that you've got a folder named *Media* on your site and that the *.swf* file is in that folder.</span></span>

1. <span data-ttu-id="aa268-167">Adicione a biblioteca de auxiliares Web do ASP.NET ao seu site, conforme descrito em [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não o tiver adicionado.</span><span class="sxs-lookup"><span data-stu-id="aa268-167">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="aa268-168">No site, adicione uma página e nomeie-a *FlashVideo. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="aa268-168">In the website, add a page and name it *FlashVideo.cshtml*.</span></span>
3. <span data-ttu-id="aa268-169">Adicione a seguinte marcação à página:</span><span class="sxs-lookup"><span data-stu-id="aa268-169">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. <span data-ttu-id="aa268-170">Execute a página em um navegador.</span><span class="sxs-lookup"><span data-stu-id="aa268-170">Run the page in a browser.</span></span> <span data-ttu-id="aa268-171">(Verifique se a página está selecionada no espaço de trabalho **arquivos** antes de executá-la.) A página é exibida e o vídeo é reproduzido automaticamente.</span><span class="sxs-lookup"><span data-stu-id="aa268-171">(Make sure the page is selected in the **Files** workspace before you run it.) The page is displayed and the video plays automatically.</span></span> 

    <span data-ttu-id="aa268-172">![imagem](10-working-with-video/_static/image1.jpg "ch08_video-1. jpg")</span><span class="sxs-lookup"><span data-stu-id="aa268-172">![[image]](10-working-with-video/_static/image1.jpg "ch08_video-1.jpg")</span></span>

<span data-ttu-id="aa268-173">Você pode definir o parâmetro `quality` para um vídeo Flash para `low`, `autolow`, `autohigh`, `medium`, `high`e `best`:</span><span class="sxs-lookup"><span data-stu-id="aa268-173">You can set the `quality` parameter for a Flash video to `low`, `autolow`, `autohigh`, `medium`, `high`, and `best`:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

<span data-ttu-id="aa268-174">Você pode alterar o vídeo Flash para reproduzir em um tamanho específico usando o parâmetro `scale`, que pode ser definido como o seguinte:</span><span class="sxs-lookup"><span data-stu-id="aa268-174">You can change the Flash video to play at a specific size using the `scale` parameter, which you can set to the following:</span></span>

- <span data-ttu-id="aa268-175">`showall`.</span><span class="sxs-lookup"><span data-stu-id="aa268-175">`showall`.</span></span> <span data-ttu-id="aa268-176">Isso torna todo o vídeo visível enquanto mantém a taxa de proporção original.</span><span class="sxs-lookup"><span data-stu-id="aa268-176">This makes the entire video visible while maintaining the original aspect ratio.</span></span> <span data-ttu-id="aa268-177">No entanto, você pode acabar com bordas em cada lado.</span><span class="sxs-lookup"><span data-stu-id="aa268-177">However, you might end up with borders on each side.</span></span>
- <span data-ttu-id="aa268-178">`noorder`.</span><span class="sxs-lookup"><span data-stu-id="aa268-178">`noorder`.</span></span> <span data-ttu-id="aa268-179">Isso dimensiona o vídeo mantendo a taxa de proporção original, mas pode ser cortada.</span><span class="sxs-lookup"><span data-stu-id="aa268-179">This scales the video while maintaining the original aspect ratio, but it might be cropped.</span></span>
- <span data-ttu-id="aa268-180">`exactfit`.</span><span class="sxs-lookup"><span data-stu-id="aa268-180">`exactfit`.</span></span> <span data-ttu-id="aa268-181">Isso torna todo o vídeo visível sem preservar a taxa de proporção original, mas pode ocorrer distorção.</span><span class="sxs-lookup"><span data-stu-id="aa268-181">This makes the entire video visible without preserving the original aspect ratio, but distortion may occur.</span></span>

<span data-ttu-id="aa268-182">Se você não especificar um parâmetro `scale`, todo o vídeo ficará visível e a taxa de proporção original será mantida sem qualquer corte.</span><span class="sxs-lookup"><span data-stu-id="aa268-182">If you don't specify a `scale` parameter, the entire video will be visible and the original aspect ratio will be maintained without any cropping.</span></span> <span data-ttu-id="aa268-183">O exemplo a seguir mostra como usar o parâmetro `scale`:</span><span class="sxs-lookup"><span data-stu-id="aa268-183">The following example shows how to use the `scale` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

<span data-ttu-id="aa268-184">O Flash Player dá suporte a uma configuração de modo de vídeo chamada `windowMode`.</span><span class="sxs-lookup"><span data-stu-id="aa268-184">The Flash player supports a video mode setting named `windowMode`.</span></span> <span data-ttu-id="aa268-185">Você pode definir isso como `window`, `opaque`e `transparent`.</span><span class="sxs-lookup"><span data-stu-id="aa268-185">You can set this to `window`, `opaque`, and `transparent`.</span></span> <span data-ttu-id="aa268-186">Por padrão, a `windowMode` é definida como `window`, que exibe o vídeo em uma janela separada na página da Web.</span><span class="sxs-lookup"><span data-stu-id="aa268-186">By default, the `windowMode` is set to `window`, which displays the video in a separate window on the web page.</span></span> <span data-ttu-id="aa268-187">A configuração de `opaque` oculta tudo por trás do vídeo na página da Web.</span><span class="sxs-lookup"><span data-stu-id="aa268-187">The `opaque` setting hides everything behind the video on the web page.</span></span> <span data-ttu-id="aa268-188">A configuração `transparent` permite que o plano de fundo da página da Web seja exibido pelo vídeo, supondo que qualquer parte do vídeo seja transparente.</span><span class="sxs-lookup"><span data-stu-id="aa268-188">The `transparent` setting lets the background of the web page show through the video, assuming any part of the video is transparent.</span></span>

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a><span data-ttu-id="aa268-189">Reprodução de vídeos do MediaPlayer ( *. wmv*)</span><span class="sxs-lookup"><span data-stu-id="aa268-189">Playing MediaPlayer (*.wmv*) Videos</span></span>

<span data-ttu-id="aa268-190">O procedimento a seguir mostra como reproduzir um vídeo de mídia de janela chamado *Sample. wmv* que está na pasta de *mídia* .</span><span class="sxs-lookup"><span data-stu-id="aa268-190">The following procedure shows you how to play a Window Media video named *sample.wmv* that's in the *Media* folder.</span></span>

1. <span data-ttu-id="aa268-191">Adicione a biblioteca de auxiliares Web do ASP.NET ao seu site, conforme descrito em [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não tiver feito isso.</span><span class="sxs-lookup"><span data-stu-id="aa268-191">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="aa268-192">Crie uma nova página chamada *MediaPlayerVideo. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="aa268-192">Create a new page named *MediaPlayerVideo.cshtml*.</span></span>
3. <span data-ttu-id="aa268-193">Adicione a seguinte marcação à página:</span><span class="sxs-lookup"><span data-stu-id="aa268-193">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. <span data-ttu-id="aa268-194">Execute a página em um navegador.</span><span class="sxs-lookup"><span data-stu-id="aa268-194">Run the page in a browser.</span></span> <span data-ttu-id="aa268-195">O vídeo é carregado e reproduzido automaticamente.</span><span class="sxs-lookup"><span data-stu-id="aa268-195">The video loads and plays automatically.</span></span> 

    <span data-ttu-id="aa268-196">![imagem](10-working-with-video/_static/image2.jpg "ch08_video -2. jpg")</span><span class="sxs-lookup"><span data-stu-id="aa268-196">![[image]](10-working-with-video/_static/image2.jpg "ch08_video-2.jpg")</span></span>

<span data-ttu-id="aa268-197">Você pode definir `playCount` como um inteiro que indica quantas vezes executar o vídeo automaticamente:</span><span class="sxs-lookup"><span data-stu-id="aa268-197">You can set `playCount` to an integer that indicates how many times to play the video automatically:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

<span data-ttu-id="aa268-198">O parâmetro `uiMode` permite especificar quais controles aparecem na interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="aa268-198">The `uiMode` parameter lets you specify which controls show up in the user interface.</span></span> <span data-ttu-id="aa268-199">Você pode definir `uiMode` como `invisible`, `none`, `mini`ou `full`.</span><span class="sxs-lookup"><span data-stu-id="aa268-199">You can set `uiMode` to `invisible`, `none`, `mini`, or `full`.</span></span> <span data-ttu-id="aa268-200">Se você não especificar um parâmetro `uiMode`, o vídeo será exibido com a janela de status, a barra de busca, os botões de controle e os controles de volume, além da janela de vídeo.</span><span class="sxs-lookup"><span data-stu-id="aa268-200">If you don't specify a `uiMode` parameter, the video will be displayed with the status window, seek bar, control buttons, and volume controls in addition to the video window.</span></span> <span data-ttu-id="aa268-201">Esses controles também serão exibidos se você usar o Player para reproduzir um arquivo de áudio.</span><span class="sxs-lookup"><span data-stu-id="aa268-201">These controls will also be displayed if you use the player to play an audio file.</span></span> <span data-ttu-id="aa268-202">Aqui está um exemplo de como usar o parâmetro `uiMode`:</span><span class="sxs-lookup"><span data-stu-id="aa268-202">Here's an example of how to use the `uiMode` parameter:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

<span data-ttu-id="aa268-203">Por padrão, áudio é ativado quando o vídeo é reproduzido.</span><span class="sxs-lookup"><span data-stu-id="aa268-203">By default, audio is on when the video plays.</span></span> <span data-ttu-id="aa268-204">Você pode ativar mudo do áudio definindo o parâmetro `mute` como true:</span><span class="sxs-lookup"><span data-stu-id="aa268-204">You can mute the audio by setting the `mute` parameter to true:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

<span data-ttu-id="aa268-205">Você pode controlar o nível de áudio do vídeo do MediaPlayer definindo o parâmetro `volume` como um valor entre 0 e 100.</span><span class="sxs-lookup"><span data-stu-id="aa268-205">You can control the audio level of the MediaPlayer video by setting the `volume` parameter to a value between 0 and 100.</span></span> <span data-ttu-id="aa268-206">O valor padrão é 50.</span><span class="sxs-lookup"><span data-stu-id="aa268-206">The default value is 50.</span></span> <span data-ttu-id="aa268-207">Aqui está um exemplo:</span><span class="sxs-lookup"><span data-stu-id="aa268-207">Here's an example:</span></span>

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a><span data-ttu-id="aa268-208">Reprodução de vídeos do Silverlight</span><span class="sxs-lookup"><span data-stu-id="aa268-208">Playing Silverlight Videos</span></span>

<span data-ttu-id="aa268-209">Este procedimento mostra como reproduzir vídeo contido em uma página do Silverlight *. xap* que está em uma pasta chamada *Media*.</span><span class="sxs-lookup"><span data-stu-id="aa268-209">This procedure shows you how to play video contained in a Silverlight *.xap* page that's in a folder named *Media*.</span></span>

1. <span data-ttu-id="aa268-210">Adicione a biblioteca de auxiliares Web do ASP.NET ao seu site, conforme descrito em [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não tiver feito isso.</span><span class="sxs-lookup"><span data-stu-id="aa268-210">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already .</span></span>
2. <span data-ttu-id="aa268-211">Crie uma nova página chamada *SilverlightVideo. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="aa268-211">Create a new page named *SilverlightVideo.cshtml*.</span></span>
3. <span data-ttu-id="aa268-212">Adicione a seguinte marcação à página:</span><span class="sxs-lookup"><span data-stu-id="aa268-212">Add the following markup to the page:</span></span> 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. <span data-ttu-id="aa268-213">Execute a página em um navegador.</span><span class="sxs-lookup"><span data-stu-id="aa268-213">Run the page in a browser.</span></span> 

    <span data-ttu-id="aa268-214">![imagem](10-working-with-video/_static/image3.jpg "ch08_video -3. jpg")</span><span class="sxs-lookup"><span data-stu-id="aa268-214">![[image]](10-working-with-video/_static/image3.jpg "ch08_video-3.jpg")</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="aa268-215">Recursos adicionais</span><span class="sxs-lookup"><span data-stu-id="aa268-215">Additional Resources</span></span>

<span data-ttu-id="aa268-216">[Visão geral do Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span><span class="sxs-lookup"><span data-stu-id="aa268-216">[Silverlight Overview](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)</span></span>

[<span data-ttu-id="aa268-217">Atributos de marcação OBJECT e EMBED de objeto flash</span><span class="sxs-lookup"><span data-stu-id="aa268-217">Flash OBJECT and EMBED tag attributes</span></span>](http://kb2.adobe.com/cps/127/tn_12701.html)

<span data-ttu-id="aa268-218">[Marcas de parâmetro do SDK do Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="aa268-218">[Windows Media Player 11 SDK PARAM Tags](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)</span></span>
