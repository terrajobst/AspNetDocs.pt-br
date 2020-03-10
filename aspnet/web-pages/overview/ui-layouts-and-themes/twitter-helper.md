---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Auxiliar do Twitter com Páginas da Web do ASP.NET | Microsoft Docs
author: Rick-Anderson
description: Este tópico e aplicativo mostram como adicionar um auxiliar do Twitter ao seu projeto do WebMatrix 3. Ele contém o código auxiliar do Twitter e mostra como chamar o auxiliar...
ms.author: riande
ms.date: 11/26/2018
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 76e32b7c808467a9a87c70017dac02bdb895e1df
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638554"
---
# <a name="twitter-helper-with-aspnet-web-pages"></a>Auxiliar do Twitter com Páginas da Web do ASP.NET

por [Tom FitzMacken](https://github.com/tfitzmac)

> [!IMPORTANT]
> Os auxiliares do Twitter estão obsoletos. Para obter as últimas ferramentas de envolvimento do Twitter para sites, consulte [visão geral do Twitter for sites](https://developer.twitter.com/en/docs/twitter-for-websites/overview).

> Este tópico e aplicativo mostram como adicionar um auxiliar do Twitter ao seu projeto do WebMatrix 3. Ele contém o código auxiliar do Twitter e mostra como chamar os métodos auxiliares.
> 
> Esse código para o arquivo Twitter. cshtml foi desenvolvido pela **Tian Pan** da Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com o Páginas da Web do ASP.NET 2.

## <a name="introduction"></a>Introdução

Este tópico demonstra como adicionar um auxiliar do Twitter ao seu aplicativo e usar sintaxe Razor para chamar os métodos auxiliares. O auxiliar do Twitter facilita a incorporação de botões e widgets do Twitter em seu aplicativo. Para usar um widget do Twitter, como a linha do tempo de um usuário ou os resultados da pesquisa para uma hashtag, primeiro você deve criar o [widget no Twitter](https://twitter.com/settings/widgets). Depois de criar o widget, você receberá uma ID de widget. Você passa essa ID de widget como um parâmetro ao chamar os métodos auxiliares que mostram widget.

Este tópico foi escrito para a versão 1,1 da API do Twitter. Ao adicionar diretamente o código auxiliar do Twitter ao seu projeto, você poderá atualizar o código auxiliar se a API do Twitter for alterada.

Para obter informações sobre como instalar o WebMatrix, consulte [introducing páginas da Web do ASP.NET 2-introdução](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Adicionar o auxiliar do Twitter ao seu projeto

Para adicionar o auxiliar do Twitter, primeiro adicione uma pasta chamada **App\_Code** ao seu projeto. Em seguida, crie um arquivo chamado **Twitter. cshtml**.

![App_Code pasta](twitter-helper/_static/image1.png)

Substitua o código padrão no Twitter. cshtml pelo código a seguir.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Chamar métodos do Twitter de suas páginas da Web

O exemplo a seguir mostra como usar os métodos auxiliares do Twitter de uma página em seu projeto. Em seu projeto, você desejará substituir os valores de parâmetro por valores relevantes às suas necessidades. Você pode usar as IDs de widget fornecidas para explorar como os métodos funcionam, mas você vai querer gerar seus próprios widgets para seu projeto.

Nem todos os parâmetros mostrados abaixo são obrigatórios. Os parâmetros opcionais são usados para personalizar como o botão ou widget é exibido. Por exemplo, o botão seguir requer apenas o nome de usuário a seguir, mas o exemplo mostra como incluir o número de seguidores e como especificar o tamanho do botão e o idioma.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Confira os resultados

O código acima produz os botões e widgets a seguir. Esses botões e widgets são totalmente funcionais, e não capturas de tela. O botão seguir é exibido em espanhol porque o parâmetro de idioma foi definido como **es**.

### <a name="follow-button"></a>Botão seguir

[Siga @aspnet)](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>`

### <a name="tweet-button"></a>Botão tweet

`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>` de [tweet](https://twitter.com/share)

### <a name="user-timeline-profile"></a>Linha do tempo do usuário (perfil)

[Tweets por @aspnet](https://twitter.com/aspnet)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="favorites"></a>Favoritos

[Tweets favoritos por @Microsoft](https://twitter.com/Microsoft/favorites)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="list"></a>Lista

[Tweets de @Microsoft/MS\_faixas de\_do consumidor](https://twitter.com/microsoft/ms-consumer-brands/)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`

### <a name="search"></a>Search

[Tweets sobre &quot;#asp .net&quot;](https://twitter.com/search?q=%23asp.net)`<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>`
