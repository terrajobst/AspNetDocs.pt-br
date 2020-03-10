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
# <a name="displaying-video-in-an-aspnet-web-pages-razor-site"></a>Exibindo vídeo em um site Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como usar um player de vídeo (mídia) em um site Páginas da Web do ASP.NET (Razor) para permitir que os usuários exibam vídeos armazenados no site. Páginas da Web do ASP.NET com sintaxe Razor permite reproduzir vídeos Flash ( *. swf*), Media Player ( *. wmv*) e Silverlight ( *. xap*).
> 
> O que você aprenderá:
> 
> - Como escolher um player de vídeo.
> - Como adicionar vídeo a uma página da Web.
> - Como definir atributos de player de vídeo.
> 
> Estes são os recursos de páginas do Razor do ASP.NET apresentados no artigo:
> 
> - O auxiliar de `Video`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - WebMatrix 2
>   
> 
> Este tutorial também funciona com o WebMatrix 3.

## <a name="introduction"></a>Introdução

Talvez você queira exibir um vídeo em seu site. Uma maneira de fazer isso é vincular a um site que já tem o vídeo, como o YouTube. Se você quiser inserir um vídeo desses sites diretamente em suas próprias páginas, você geralmente pode obter marcação HTML do site e, em seguida, copiá-lo para sua página. Por exemplo, o exemplo a seguir mostra como inserir um vídeo do YouTube:

[!code-html[Main](10-working-with-video/samples/sample1.html?highlight=10-14)]

Se você quiser reproduzir um vídeo que esteja no seu próprio site (não em um site de compartilhamento de vídeo público), não poderá vincular diretamente a ele usando marcação inserida como esta. No entanto, você pode reproduzir vídeos de seu site usando o auxiliar de `Video`, que renderiza um player de mídia diretamente em uma página.

<a id="Choosing_a_Video_Player"></a>
## <a name="choosing-a-video-player"></a>Escolhendo um player de vídeo

Há muitos formatos para arquivos de vídeo, e cada formato geralmente requer um player diferente e uma maneira diferente de configurar o Player. Em páginas Razor do ASP.NET, você pode reproduzir um vídeo em uma página da Web usando o auxiliar `Video`. O auxiliar de `Video` simplifica o processo de inserção de vídeos em uma página da Web, pois gera automaticamente os `object` e `embed` elementos HTML que normalmente são usados para adicionar vídeo à página.

O auxiliar de `Video` dá suporte aos seguintes players de mídia:

- Adobe Flash
- Windows MediaPlayer
- Microsoft Silverlight

### <a name="the-flash-player"></a>O Flash Player

O `Flash` Player do auxiliar de `Video` permite reproduzir vídeos Flash (arquivos *. swf* ) em uma página da Web. No mínimo, você precisa fornecer um caminho para o arquivo de vídeo. Se você não especificar nada, exceto o caminho, o Player usará valores padrão definidos pela versão atual do flash. As configurações padrão típicas são:

- O vídeo é exibido usando sua largura e altura padrão e sem uma cor de plano de fundo.
- O vídeo é reproduzido automaticamente quando a página é carregada.
- O vídeo faz o loop continuamente até ser interrompido explicitamente.
- O vídeo é dimensionado para mostrar todo o vídeo, em vez de cortar o vídeo para se ajustar a um tamanho específico.
- O vídeo é reproduzido em uma janela.

### <a name="the-mediaplayer-player"></a>O jogador do MediaPlayer

O `MediaPlayer` Player do auxiliar do `Video` permite que você reproduza vídeos do Windows Media (arquivos *. wmv* ), áudio do Windows Media (arquivos *. WMA* ) e MP3 (arquivos *. mp3* ) em uma página da Web. Você deve incluir o caminho do arquivo de mídia para reprodução; todos os outros parâmetros são opcionais. Se você especificar apenas um caminho, o Player usará as configurações padrão definidas pela versão atual do MediaPlayer, como:

- O vídeo é exibido usando sua largura e altura padrão.
- O vídeo é reproduzido automaticamente quando a página é carregada.
- O vídeo é reproduzido uma vez (não faz loop).
- O Player exibe o conjunto completo de controles na interface do usuário.
- O vídeo é reproduzido em uma janela.

### <a name="the-silverlight-player"></a>O player do Silverlight

O `Silverlight` Player do auxiliar do `Video` permite reproduzir vídeo do Windows Media (arquivos *. wmv* ), áudio do Windows Media (arquivos *. WMA* ) e MP3 (arquivos *. mp3* ). Você deve definir o parâmetro Path para apontar para um pacote de aplicativos baseado em Silverlight (arquivo *. xap* ). Você também deve definir os parâmetros Width e Height. Todos os outros parâmetros são opcionais. Ao usar o player do Silverlight para vídeo, se você definir apenas os parâmetros necessários, o player do Silverlight exibirá o vídeo sem uma cor de plano de fundo.

> [!NOTE]
> Caso você ainda não conheça o Silverlight: o arquivo *. xap* é um arquivo compactado que contém instruções de layout em um arquivo *. XAML* , código gerenciado em assemblies e recursos opcionais. Você pode criar um arquivo *. xap* no Visual Studio como um projeto de aplicativo do Silverlight.

O player de vídeo `Silverlight` usa as configurações que você fornece para o Player e as configurações que são fornecidas no arquivo *. xap* .

> [!TIP] 
> 
> <a id="SB_MimeTypes"></a>
> ### <a name="mime-types"></a>Tipos de MIME
> 
> Quando um navegador baixa um arquivo, o navegador garante que o tipo de arquivo corresponda ao tipo MIME especificado para o documento que está sendo processado. O tipo de MIME é o tipo de conteúdo ou o tipo de mídia de um arquivo. O auxiliar de `Video` usa os seguintes tipos de MIME:
> 
> - `application/x-shockwave-flash`
> - `application/x-mplayer2`
> - `application/x-silverlight-2`

<a id="Playing_Flash"></a>
## <a name="playing-flash-swf-videos"></a>Reprodução de vídeos Flash (. swf)

Este procedimento mostra como reproduzir um vídeo Flash chamado *Sample. swf*. O procedimento pressupõe que você tem uma pasta chamada *mídia* no seu site e que o arquivo *. swf* está nessa pasta.

1. Adicione a biblioteca de auxiliares Web do ASP.NET ao seu site, conforme descrito em [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não o tiver adicionado.
2. No site, adicione uma página e nomeie-a *FlashVideo. cshtml*.
3. Adicione a seguinte marcação à página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample2.cshtml)]
4. Execute a página em um navegador. (Verifique se a página está selecionada no espaço de trabalho **arquivos** antes de executá-la.) A página é exibida e o vídeo é reproduzido automaticamente. 

    ![imagem](10-working-with-video/_static/image1.jpg "ch08_video-1. jpg")

Você pode definir o parâmetro `quality` para um vídeo Flash para `low`, `autolow`, `autohigh`, `medium`, `high`e `best`:

[!code-cshtml[Main](10-working-with-video/samples/sample3.cshtml)]

Você pode alterar o vídeo Flash para reproduzir em um tamanho específico usando o parâmetro `scale`, que pode ser definido como o seguinte:

- `showall`. Isso torna todo o vídeo visível enquanto mantém a taxa de proporção original. No entanto, você pode acabar com bordas em cada lado.
- `noorder`. Isso dimensiona o vídeo mantendo a taxa de proporção original, mas pode ser cortada.
- `exactfit`. Isso torna todo o vídeo visível sem preservar a taxa de proporção original, mas pode ocorrer distorção.

Se você não especificar um parâmetro `scale`, todo o vídeo ficará visível e a taxa de proporção original será mantida sem qualquer corte. O exemplo a seguir mostra como usar o parâmetro `scale`:

[!code-cshtml[Main](10-working-with-video/samples/sample4.cshtml)]

O Flash Player dá suporte a uma configuração de modo de vídeo chamada `windowMode`. Você pode definir isso como `window`, `opaque`e `transparent`. Por padrão, a `windowMode` é definida como `window`, que exibe o vídeo em uma janela separada na página da Web. A configuração de `opaque` oculta tudo por trás do vídeo na página da Web. A configuração `transparent` permite que o plano de fundo da página da Web seja exibido pelo vídeo, supondo que qualquer parte do vídeo seja transparente.

<a id="Playing_MediaPlayer"></a>
## <a name="playing-mediaplayer-wmv-videos"></a>Reprodução de vídeos do MediaPlayer ( *. wmv*)

O procedimento a seguir mostra como reproduzir um vídeo de mídia de janela chamado *Sample. wmv* que está na pasta de *mídia* .

1. Adicione a biblioteca de auxiliares Web do ASP.NET ao seu site, conforme descrito em [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não tiver feito isso.
2. Crie uma nova página chamada *MediaPlayerVideo. cshtml*.
3. Adicione a seguinte marcação à página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample5.cshtml)]
4. Execute a página em um navegador. O vídeo é carregado e reproduzido automaticamente. 

    ![imagem](10-working-with-video/_static/image2.jpg "ch08_video -2. jpg")

Você pode definir `playCount` como um inteiro que indica quantas vezes executar o vídeo automaticamente:

[!code-cshtml[Main](10-working-with-video/samples/sample6.cshtml)]

O parâmetro `uiMode` permite especificar quais controles aparecem na interface do usuário. Você pode definir `uiMode` como `invisible`, `none`, `mini`ou `full`. Se você não especificar um parâmetro `uiMode`, o vídeo será exibido com a janela de status, a barra de busca, os botões de controle e os controles de volume, além da janela de vídeo. Esses controles também serão exibidos se você usar o Player para reproduzir um arquivo de áudio. Aqui está um exemplo de como usar o parâmetro `uiMode`:

[!code-cshtml[Main](10-working-with-video/samples/sample7.cshtml)]

Por padrão, áudio é ativado quando o vídeo é reproduzido. Você pode ativar mudo do áudio definindo o parâmetro `mute` como true:

[!code-cshtml[Main](10-working-with-video/samples/sample8.cshtml)]

Você pode controlar o nível de áudio do vídeo do MediaPlayer definindo o parâmetro `volume` como um valor entre 0 e 100. O valor padrão é 50. Aqui está um exemplo:

[!code-cshtml[Main](10-working-with-video/samples/sample9.cshtml)]

<a id="Playing_Silverlight"></a>
## <a name="playing-silverlight-videos"></a>Reprodução de vídeos do Silverlight

Este procedimento mostra como reproduzir vídeo contido em uma página do Silverlight *. xap* que está em uma pasta chamada *Media*.

1. Adicione a biblioteca de auxiliares Web do ASP.NET ao seu site, conforme descrito em [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não tiver feito isso.
2. Crie uma nova página chamada *SilverlightVideo. cshtml*.
3. Adicione a seguinte marcação à página: 

    [!code-cshtml[Main](10-working-with-video/samples/sample10.cshtml)]
4. Execute a página em um navegador. 

    ![imagem](10-working-with-video/_static/image3.jpg "ch08_video -3. jpg")

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

[Visão geral do Silverlight](https://msdn.microsoft.com/library/bb404700(VS.95).aspx)

[Atributos de marcação OBJECT e EMBED de objeto flash](http://kb2.adobe.com/cps/127/tn_12701.html)

[Marcas de parâmetro do SDK do Windows Media Player 11](https://msdn.microsoft.com/library/aa392321(VS.85).aspx)
