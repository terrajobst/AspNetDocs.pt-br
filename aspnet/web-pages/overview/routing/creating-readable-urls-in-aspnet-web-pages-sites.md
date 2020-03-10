---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Criando URLs legíveis em sites Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este artigo descreve o roteamento em um site Páginas da Web do ASP.NET (Razor) e como isso permite que você use URLs que são mais legíveis e melhores para SEO. O que você vai...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 832db8e144cab730f16c78f67c12feb9b7c92c7c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628390"
---
# <a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Criando URLs legíveis em sites Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve o roteamento em um site Páginas da Web do ASP.NET (Razor) e como isso permite que você use URLs que são mais legíveis e melhores para SEO.
> 
> O que você aprenderá:
> 
> - Como o ASP.NET usa o roteamento para permitir que você use URLs mais legíveis e pesquisáveis.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com o Páginas da Web do ASP.NET 2.

## <a name="about-routing"></a>Sobre roteamento

As URLs para as páginas em seu site podem ter um impacto sobre o quão bem o site funciona. Uma URL &quot;&quot; amigável pode tornar mais fácil para as pessoas usarem o site. Ele também pode ajudar com a SEO (otimização do mecanismo de pesquisa) para o site. Os sites ASP.NET incluem a capacidade de usar URLs amigáveis automaticamente.

O ASP.NET permite que você crie URLs significativas que descrevem as ações do usuário em vez de apenas apontar para um arquivo no servidor. Considere estas URLs para um blog fictício:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Compare essas URLs com as seguintes:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

No primeiro par, um usuário precisa saber que o blog é exibido usando a página *blog. cshtml* e, em seguida, teria que construir uma cadeia de caracteres de consulta que obtenha a categoria ou o intervalo de datas corretos. O segundo conjunto de exemplos é muito mais fácil de compreender e criar.

As URLs para o primeiro exemplo também apontam diretamente para um arquivo específico (*blog. cshtml*). Se, por algum motivo, o blog fosse movido para outra pasta no servidor, ou se o blog tiver sido reescrito para usar uma página diferente, os links ficarão errados. O segundo conjunto de URLs não aponta para uma página específica, portanto, mesmo que a implementação do blog ou o local mudem, as URLs ainda seriam válidas.

No Páginas da Web do ASP.NET, você pode criar URLs mais amigáveis, como aquelas nos exemplos acima, porque ASP.NET usa o *Roteamento*. O roteamento cria um mapeamento lógico de uma URL para uma página (ou páginas) que pode atender à solicitação. Como o mapeamento é lógico (não físico, para um arquivo específico), o roteamento fornece grande flexibilidade para definir as URLs para seu site.

## <a name="how-routing-works"></a>Como funciona o roteamento

Quando o ASP.NET processa uma solicitação, ele lê a URL para determinar como roteá-la. O ASP.NET tenta corresponder segmentos individuais da URL para arquivos no disco, indo da esquerda para a direita. Se houver uma correspondência, qualquer coisa restante na URL será passada para a página como *informações de caminho*.

Imagine que alguém faça uma solicitação usando esta URL:

`http://www.contoso.com/a/b/c`

A pesquisa é assim:

1. Há um arquivo com o caminho e o nome de */a/b/c.cshtml*? Nesse caso, execute essa página e passe nenhuma informação para ela. Caso contrário...
2. Há um arquivo com o caminho e o nome de */a/b.cshtml*? Nesse caso, execute essa página e passe o valor `c` a ela. Caso contrário...
3. Há um arquivo com o caminho e o nome de */a.cshtml*? Nesse caso, execute essa página e passe o valor `b/c` a ela.

Se a pesquisa não encontrou correspondências exatas para arquivos *. cshtml* em suas pastas especificadas, o ASP.net continuará procurando esses arquivos por sua vez:

1. */a/b/c/default.cshtml* (nenhuma informação de caminho).
2. */a/b/c/index.cshtml* (nenhuma informação de caminho).

> [!NOTE]
> Para ser claro, as solicitações para páginas específicas (ou seja, solicitações que incluem a extensão de nome de arquivo *. cshtml* ) funcionam da mesma forma que esperado. Uma solicitação como `http://www.contoso.com/a/b.cshtml` executará a página *b. cshtml* , bem.

Dentro de uma página, você pode obter as informações de caminho por meio da propriedade `UrlData` da página, que é um dicionário. Imagine que você tenha um arquivo chamado *ViewCustomers. cshtml* e o site receba esta solicitação:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Conforme descrito nas regras acima, a solicitação vai para sua página. Dentro da página, você pode usar um código como o seguinte para obter e exibir as informações de caminho (nesse caso, o valor &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Como o roteamento não envolve nomes de arquivo completos, pode haver ambiguidade se você tiver páginas com o mesmo nome, mas extensões de nome de arquivo diferentes (por exemplo, *mypage. cshtml* e *mypage. html*). Para evitar problemas com o roteamento, é melhor certificar-se de que você não tem páginas em seu site cujos nomes diferem apenas em sua extensão.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

[WebMatrix-URLs, UrlData e roteamento para SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). Esta entrada de blog de Mike Brind fornece alguns detalhes adicionais sobre como funciona o roteamento no Páginas da Web do ASP.NET.
