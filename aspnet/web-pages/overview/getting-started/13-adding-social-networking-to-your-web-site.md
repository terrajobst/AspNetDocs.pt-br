---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Adicionando redes sociais a sites Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este capítulo explica como integrar seu site com os serviços de rede social. Neste capítulo, você aprenderá como permitir que as pessoas marquem/vinculem seu site...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 1637464b0473bba8133acbbf8918d92b4f552701
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526932"
---
# <a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>Adicionando redes sociais a sites Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como adicionar links de rede social para Facebook, Twitter, Reddit e Digg a páginas em um site Páginas da Web do ASP.NET (Razor) e como incluir feeds do Twitter, cartões de Game do Xbox e imagens do Gravatar.
> 
> O que você aprenderá:
> 
> - Como permitir que as pessoas marquem/vinculem seu site.
> - Como adicionar um feed do Twitter.
> - Como adicionar um botão **like** do Facebook a páginas.
> - Como renderizar imagens Gravatar.com.
> - Como exibir um cartão de Game do Xbox em seu site.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - Biblioteca auxiliar da Web do ASP.NET (pacote NuGet)
>   
> 
> Este tutorial também funciona com o Páginas da Web do ASP.NET 3, exceto para partes que usam a biblioteca auxiliar da Web do ASP.NET.

<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Vinculando seu site em sites de rede social

Se as pessoas gostam de algo em seu site, muitas vezes desejam compartilhá-las com amigos. Você pode facilitar essa tarefa exibindo glifos (ícones) que as pessoas podem clicar para compartilhar uma página em Digg, Reddit, Facebook, Twitter ou sites semelhantes.

Para exibir esses glifos, adicione o auxiliar de `LinkSharecode` a uma página. As pessoas que visitam sua página podem clicar em um glifo individual. Se eles tiverem uma conta com esse site de rede social, eles poderão postar um link para sua página nesse site.

![Imagem 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. Adicione a biblioteca de auxiliares Web do ASP.NET ao seu site, conforme descrito em [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372), se você ainda não o tiver adicionado.-Crie uma página chamada *ListLinkShare. cshtml* e adicione a seguinte marcação:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    Neste exemplo, quando o auxiliar de `LinkShare` é executado, o título da página é passado como um parâmetro que, por sua vez, passa o título da página para o site de rede social. No entanto, você pode passar qualquer cadeia de caracteres que desejar. Este exemplo também especifica quais sites de rede social incluir na lista. Você pode especificar os sites de rede social que são relevantes para seu site.
2. Execute a página *ListLinkShare. cshtml* em um navegador. (Verifique se a página está selecionada no espaço de trabalho **arquivos** antes de executá-la.)
3. Clique em um glifo para um dos sites para os quais você está inscrito. O link leva você para a página no site de rede social selecionado, onde você pode compartilhar um link. Por exemplo, se você clicar no link Reddit, você será levado para a página `submit to reddit` no site do reddit.

     ![Imagem 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Adicionando um feed do Twitter

Para obter informações sobre como usar um auxiliar do Twitter que seja compatível com a versão atual da API do Twitter, consulte [auxiliar do Twitter](../ui-layouts-and-themes/twitter-helper.md). Este exemplo mostra como escrever seu próprio auxiliar para que você possa reutilizar facilmente o código de várias páginas.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Exibindo um &quot;do Facebook como botão de&quot;

Em alguns casos, a melhor opção é obter o código diretamente do provedor de rede social em vez de depender de um auxiliar. Isso é especialmente verdadeiro se o provedor de rede social atualizar suas opções mais rapidamente do que o auxiliar é atualizado.

Para adicionar recursos do Facebook (como o botão like) ao seu site, você pode recuperar trechos de código do site [Developers.Facebook.com](https://developers.facebook.com/) . No site do Facebook, você usa suas ferramentas para gerar um trecho de código relevante para seu site.

O código realçado a seguir é o código que foi recuperado da ferramenta de botão like no site developers.facebook.com. Você deve fornecer sua própria ID de aplicativo.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Renderizando uma imagem Gravatar

Um *Gravatar* (um &quot;avatar reconhecido globalmente&quot;) é uma imagem que pode ser usada em vários sites como seu &#8212; avatar, uma imagem que o representa. Por exemplo, um gravatar pode identificar uma pessoa em uma postagem de fórum, em um comentário de blog e assim por diante. (Você pode registrar seu próprio Gravatar no site do gravatar em [http://www.gravatar.com/](http://www.gravatar.com/).) Se desejar exibir imagens ao lado de nomes de pessoas ou endereços de email em seu site, você poderá usar o auxiliar Gravatar.

Neste exemplo, você está usando um único Gravatar que representa você mesmo. Outra maneira de usar um Gravatar é permitir que as pessoas especifiquem seu endereço Gravatar ao se registrarem em seu site. (Você pode aprender como permitir que as pessoas se registrem na [adição de segurança e associação a um Site páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=202904).) Em seguida, sempre que exibir informações para esse usuário, você pode simplesmente adicionar o Gravatar ao local em que você exibe o nome do usuário.

1. Adicione a biblioteca de auxiliares Web do ASP.NET ao seu site, conforme descrito em [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não tiver feito isso.
2. Crie uma nova página da Web chamada *Gravatar. cshtml*.
3. Adicione a seguinte marcação ao arquivo: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    O método `Gravatar.GetHtml` exibe a imagem Gravatar na página. Para alterar o tamanho da imagem, você pode incluir um número como um segundo parâmetro. O tamanho padrão é 80. Números menores que 80 tornam a imagem menor. Números maiores que 80 tornam a imagem maior.
4. Nos métodos de `Gravatar.GetHtml`, substitua `<Your Gravatar account here>` pelo endereço de email que você usa para sua conta do Gravatar. (Se você não tiver uma conta do Gravatar, poderá usar o endereço de email de alguém que o faz.)
5. Execute a página em seu navegador. A página exibe duas imagens Gravatar para o endereço de email que você especificou. A segunda imagem é menor do que a primeira. 

    ![Imagem 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Exibindo um cartão de Game-Xbox

Quando as pessoas tocam os jogos do Microsoft Xbox online, cada usuário tem uma ID exclusiva. As estatísticas são mantidas para cada jogador na forma de um cartão de jogador, que mostra a reputação, a pontuação de jogador e jogos reproduzidos recentemente. Se você for um jogador do Xbox, poderá mostrar seu cartão de jogador em páginas no seu site usando o auxiliar de `GamerCard`.

1. Adicione a biblioteca de auxiliares Web do ASP.NET ao seu site, conforme descrito em [instalando auxiliares em um páginas da Web do ASP.net site](https://go.microsoft.com/fwlink/?LinkId=252372), se ainda não tiver feito isso.
2. Crie uma nova página chamada *XboxGamer. cshtml* e adicione a marcação a seguir.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Use a propriedade `GamerCard.GetHtml` para especificar o alias para o cartão de Gamer a ser exibido.
3. Execute a página em seu navegador. A página exibe o cartão de Game do Xbox que você especificou.

    ![Figura 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
