---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Exibindo mapas em um site Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este artigo explica como exibir mapas interativos em páginas em um site Páginas da Web do ASP.NET (Razor) com base nos serviços de mapeamento fornecidos pelo Bing, Google, ma...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 36f3b753cf312504892872ff54bef49854588990
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638673"
---
# <a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a>Exibindo mapas em um site Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como exibir mapas interativos em páginas em um site Páginas da Web do ASP.NET (Razor) com base nos serviços de mapeamento fornecidos pelo Bing, Google, MapQuest e Yahoo.
> 
> O que você aprenderá:
> 
> - Como gerar um mapa com base em um endereço.
> - Como gerar um mapa com base nas coordenadas de latitude e longitude.
> - Como registrar uma conta de desenvolvedor do Bing Maps e obter uma chave para usar com o Bing Maps.
> 
> Esse é o recurso ASP.NET introduzido no artigo:
> 
> - O auxiliar de `Maps`.
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

Em páginas da Web, você pode exibir mapas em uma página usando `Maps` auxiliar. Você pode gerar mapas com base em um endereço ou em um conjunto de coordenadas de longitude e latitude. A classe `Maps` permite que você chame mecanismos populares de mapa, incluindo Bing, Google, MapQuest e Yahoo.

As etapas para adicionar o mapeamento a uma página são as mesmas, independentemente dos mecanismos de mapa que você chama. Basta adicionar uma referência de arquivo JavaScript que torna os métodos disponíveis para exibir o mapa e, em seguida, você chama métodos do auxiliar de `Maps`.

Você escolhe um serviço de mapa com base em qual `Maps` método auxiliar você usa. Você pode usar qualquer um dos seguintes:

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a>Instalando as peças de que você precisa

Para exibir mapas, você precisa destas partes:

- O auxiliar de `Maps`. Esse auxiliar está na versão 2 da biblioteca de auxiliares Web do ASP.NET. Se você ainda não tiver adicionado a biblioteca, poderá instalá-la em seu site como um pacote NuGet. Para obter detalhes, consulte [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372). (Na Galeria, procure o pacote `microsoft-web-helpers`.)
- A biblioteca jQuery. Vários dos modelos de site do WebMatrix já incluem bibliotecas jQuery em suas pastas de *script* . Se você não tiver essas bibliotecas, poderá baixar a biblioteca jQuery mais recente diretamente do site [jQuery.org](http://jQuery.org) . Ou você pode criar um novo site usando um modelo (por exemplo, o modelo de **site inicial** ) e, em seguida, copiar os arquivos jQuery desse site para o site atual.

Por fim, se você quiser usar o Bing Maps, deverá primeiro criar uma conta (gratuita) e obter uma chave. Para obter uma chave, siga estas etapas:

1. Crie uma conta na [conta de desenvolvedor do Bing Maps](https://www.microsoft.com/maps/developers/web.aspx). Você também deve ter um conta Microsoft (Windows Live ID).

    Você pode especificar que deseja usar a chave para **avaliação/teste**. Se você estiver testando a função de mapeamento em seu próprio computador usando o WebMatrix e o IIS Express, acesse o espaço de trabalho do **site** e anote a URL do seu site (por exemplo, `http://localhost:50408`, embora o número da porta seja provavelmente diferente). Você pode usar esse endereço de *localhost* como o site ao se registrar.
2. Depois de se registrar para uma conta, vá para o centro de contas do Bing Maps e clique em **criar ou exibir chaves**:

    ![mapeamento-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. Registre a chave que o Bing cria.

## <a name="creating-a-map-based-on-an-address-using-google"></a>Criando um mapa com base em um endereço (usando o Google)

O exemplo a seguir mostra como criar uma página que renderiza um mapa com base em um endereço. Este exemplo mostra como usar o Google Maps.

1. Crie um arquivo chamado *MapAddress. cshtml* na raiz do site. Esta página gerará um mapa com base em um endereço que você passa para ele.
2. Copie o código a seguir no arquivo, substituindo o conteúdo existente.

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Observe os seguintes recursos da página:

    - O elemento `<script>` no elemento `<head>`. No exemplo, o elemento `<script>` faz referência ao arquivo *jQuery-1.6.4. min. js* , que é uma versão reduzidos (compactada) da biblioteca jQuery, versão 1.6.4. Observe que a referência pressupõe que o arquivo *. js* esteja na pasta *scripts* do seu site. 

        > [!NOTE]
        > Se você estiver usando uma versão diferente da biblioteca jQuery, apenas certifique-se de que você está apontando para essa versão corretamente.
    - A chamada para o `@Maps.GetGoogleHtml` no corpo da página. Para mapear um endereço, você deve passar uma cadeia de caracteres de endereço. Os métodos para os outros mecanismos de mapa funcionam de maneira semelhante (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).
3. Execute a página e insira um endereço. A página exibe um mapa, com base no Google Maps, que mostra o local que você especificou.

     ![mapeamento-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a>Criando um mapa baseado em coordenadas de latitude e longitude (usando o Bing)

Este exemplo mostra como criar um mapa com base em coordenadas. Este exemplo mostra como usar o Bing Maps e como incluir sua chave do Bing. (Você pode criar um mapa com base nas coordenadas usando os outros mecanismos de mapa também, sem usar uma chave do Bing.)

1. Crie um arquivo chamado *MapCoordinates. cshtml* na raiz do site e substitua o conteúdo existente pelo seguinte código e marcação:

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. Substitua `your-key-here` pela chave do Bing Maps que você gerou anteriormente.
3. Execute a página *MapCoordinates. cshtml* , insira as coordenadas de latitude e longitude e, em seguida, clique no **mapa!** . (Se você não souber nenhuma coordenada, tente o seguinte. Este é um local no campus da Microsoft Redmond.)

   - Latitude: 47.6781005859375
   - Longitude:-122.158317565918

     A página é exibida usando as coordenadas que você especificou.

     ![mapeamento-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

[Referência da API do Microsoft. Maps](https://msdn.microsoft.com/library/gg427611.aspx)
