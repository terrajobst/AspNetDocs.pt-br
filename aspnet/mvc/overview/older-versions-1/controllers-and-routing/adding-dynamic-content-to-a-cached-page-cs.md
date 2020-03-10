---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Adicionando conteúdo dinâmico a uma página armazenada emC#cache () | Microsoft Docs
author: microsoft
description: Saiba como misturar conteúdo dinâmico e armazenado em cache na mesma página. A substituição após o cache permite que você exiba conteúdo dinâmico, como anúncios de faixa o...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: be43712d3dd5235117558e991d9dd71aa30ec470
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601580"
---
# <a name="adding-dynamic-content-to-a-cached-page-c"></a>Adicionar conteúdo dinâmico a uma página em cache (C#)

pela [Microsoft](https://github.com/microsoft)

> Saiba como misturar conteúdo dinâmico e armazenado em cache na mesma página. A substituição após o cache permite que você exiba conteúdo dinâmico, como anúncios de faixa ou itens de notícias, dentro de uma página que tenha sido de saída armazenada em cache.

Aproveitando o cache de saída, você pode melhorar drasticamente o desempenho de um aplicativo MVC ASP.NET. Em vez de regenerar uma página cada e sempre que a página é solicitada, a página pode ser gerada uma vez e armazenada em cache na memória para vários usuários.

Mas há um problema. E se você precisar exibir conteúdo dinâmico na página? Por exemplo, imagine que você deseja exibir um anúncio de faixa na página. Você não quer que o anúncio de faixa seja armazenado em cache para que todos os usuários vejam o mesmo anúncio. Você não ganharia nenhum dinheiro assim!

Felizmente, há uma solução fácil. Você pode aproveitar um recurso da estrutura ASP.NET chamada *substituição*pós-cache. A substituição após o cache permite que você substitua o conteúdo dinâmico em uma página que foi armazenada em cache na memória.

Normalmente, quando você faz o cache de saída de uma página usando o atributo [OutputCache], a página é armazenada em cache no servidor e no cliente (o navegador da Web). Quando você usa a substituição pós-cache, uma página é armazenada em cache somente no servidor.

#### <a name="using-post-cache-substitution"></a>Usando a substituição pós-cache

O uso da substituição após o cache requer duas etapas. Primeiro, você precisa definir um método que retorne uma cadeia de caracteres que represente o conteúdo dinâmico que você deseja exibir na página armazenada em cache. Em seguida, você chama o método HttpResponse. WriteSubstitution () para injetar o conteúdo dinâmico na página.

Imagine, por exemplo, que você deseja exibir aleatoriamente diferentes itens de notícias em uma página armazenada em cache. A classe na Listagem 1 expõe um único método, chamado RenderNews (), que retorna aleatoriamente um item de notícias de uma lista de três itens de notícias.

**Listagem 1 – Models\News.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

Para aproveitar a substituição após o cache, chame o método HttpResponse. WriteSubstitution (). O método WriteSubstitution () configura o código para substituir uma região da página armazenada em cache por conteúdo dinâmico. O método WriteSubstitution () é usado para exibir o item de notícias aleatório na exibição na Listagem 2.

**Listagem 2 – Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

O método RenderNews é passado para o método WriteSubstitution (). Observe que o método RenderNews não é chamado (não há parênteses). Em vez disso, uma referência ao método é passada para WriteSubstitution ().

A exibição do índice é armazenada em cache. A exibição é retornada pelo controlador na Listagem 3. Observe que a ação index () é decorada com um atributo [OutputCache] que faz com que a exibição de índice seja armazenada em cache por 60 segundos.

**Listagem 3 – Controllers\HomeController.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

Embora a exibição do índice seja armazenada em cache, diferentes itens de notícias aleatórios são exibidos quando você solicita a página de índice. Quando você solicita a página de índice, a hora exibida pela página não é alterada por 60 segundos (consulte a Figura 1). O fato de que a hora não muda, comprova que a página está armazenada em cache. No entanto, o conteúdo injetado pelo método WriteSubstitution () – o item de notícias aleatório – muda com cada solicitação.

**Figura 1 – injetando itens de notícias dinâmicos em uma página armazenada em cache**

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Usando a substituição pós-cache em métodos auxiliares

Uma maneira mais fácil de aproveitar a substituição após o cache é encapsular a chamada para o método WriteSubstitution () em um método auxiliar personalizado. Essa abordagem é ilustrada pelo método auxiliar na Listagem 4.

**Listagem 4 – AdHelper.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

A listagem 4 contém uma classe estática que expõe dois métodos: RenderBanner () e RenderBannerInternal (). O método RenderBanner () representa o método auxiliar real. Esse método estende a classe ASP.NET MVC HtmlHelper padrão para que você possa chamar HTML. RenderBanner () em uma exibição, assim como qualquer outro método auxiliar.

O método RenderBanner () chama o método HttpResponse. WriteSubstitution () passando o método RenderBannerInternal () para o método WriteSubstitution ().

O método RenderBannerInternal () é um método particular. Esse método não será exposto como um método auxiliar. O método RenderBannerInternal () retorna aleatoriamente uma imagem de anúncio de faixa de uma lista de três imagens de anúncio de faixa.

A exibição de índice modificada na listagem 5 ilustra como você pode usar o método auxiliar RenderBanner (). Observe que uma diretiva &lt;% @ Import%&gt; adicional está incluída na parte superior da exibição para importar o namespace MvcApplication1. Helpers. Se você não importar esse namespace, o método RenderBanner () não aparecerá como um método na propriedade HTML.

**Listagem 5 – Views\Home\Index.aspx (com o método RenderBanner ())**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

Quando você solicita a página renderizada pela exibição na listagem 5, um anúncio de faixa diferente é exibido com cada solicitação (consulte a Figura 2). A página é armazenada em cache, mas o anúncio de banner é injetado dinamicamente pelo método auxiliar RenderBanner ().

**Figura 2 – a exibição de índice exibindo um anúncio de banner aleatório**

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a>Resumo

Este tutorial explicou como você pode atualizar dinamicamente o conteúdo em uma página armazenada em cache. Você aprendeu a usar o método HttpResponse. WriteSubstitution () para permitir que o conteúdo dinâmico seja injetado em uma página armazenada em cache. Você também aprendeu como encapsular a chamada para o método WriteSubstitution () dentro de um método auxiliar HTML.

Aproveite o armazenamento em cache sempre que possível – ele pode ter um impacto considerável sobre o desempenho de seus aplicativos Web. Conforme explicado neste tutorial, você pode aproveitar o cache mesmo quando precisa exibir conteúdo dinâmico em suas páginas.

> [!div class="step-by-step"]
> [Anterior](improving-performance-with-output-caching-cs.md)
> [Próximo](creating-a-controller-cs.md)
