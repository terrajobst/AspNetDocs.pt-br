---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
title: URLs em páginas mestras (VB) | Microsoft Docs
author: rick-anderson
description: Aborda como as URLs na página mestra podem ser interrompidas devido ao arquivo de página mestra estar em um diretório relativo diferente da página de conteúdo. Examina a troca de base...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: 43d1e83c-0092-4dcf-977c-e709c4dce7c3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 01627988f68bb619969a5fe3cfaae68fe70b5d4f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548107"
---
# <a name="urls-in-master-pages-vb"></a>URLs em páginas mestras (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_VB.zip) ou [baixar PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_VB.pdf)

> Aborda como as URLs na página mestra podem ser interrompidas devido ao arquivo de página mestra estar em um diretório relativo diferente da página de conteúdo. Examina as URLs de troca de URL via ~ na sintaxe declarativa e usando ResolveUrl e ResolveClientUrl programaticamente. (Consulte também

## <a name="introduction"></a>Introdução

Em todos os exemplos que vimos até agora, as páginas mestras e de conteúdo estão localizadas na mesma pasta (a pasta raiz do site). Mas não há motivo para o mestre e as páginas de conteúdo precisarem estar na mesma pasta. Certamente, você pode criar páginas de conteúdo em subpastas. Da mesma forma, você pode criar uma pasta `~/MasterPages/` onde você coloca as páginas mestras do site.

Um possível problema com a colocação de páginas mestras e de conteúdo em pastas diferentes envolve URLs rompidas. Se a página mestra contiver URLs relativas em hiperlinks, imagens ou outros elementos, o link será inválido para páginas de conteúdo que residem em uma pasta diferente. Neste tutorial, examinaremos a origem desse problema, bem como soluções alternativas.

## <a name="the-problem-with-relative-urls"></a>O problema com URLs relativas

Uma URL em uma página da Web é considerada uma *URL relativa* se o local do recurso a que ele aponta for relativo ao local da página da Web na estrutura de pastas do site. Qualquer URL que não inicia com uma barra (`/`) ou um protocolo (como `http://`) à esquerda é relativa porque é resolvida pelo navegador com base no local da página da Web que contém a URL.

Por exemplo, nosso site tem uma pasta `~/Images/` com um único arquivo de imagem, `PoweredByASPNET.gif`. O arquivo da página mestra `Site.master` tem um elemento `<img>` na região `footerContent` com a seguinte marcação:

[!code-html[Main](urls-in-master-pages-vb/samples/sample1.html)]

O valor do atributo `src` no elemento `<img>` é uma URL relativa porque não começa com `/` ou `http://`. Em suma, o valor do atributo `src` informa ao navegador para procurar na subpasta `Images` por um arquivo chamado `PoweredByASPNET.gif`.

Ao visitar uma página de conteúdo, a marcação acima é enviada diretamente para o navegador. Reserve um tempo para visitar `About.aspx` e exibir a fonte HTML enviada ao navegador. Você verá que exatamente a mesma marcação na página mestra foi enviada para o navegador.

[!code-html[Main](urls-in-master-pages-vb/samples/sample2.html)]

Se a página de conteúdo estiver na pasta raiz (como `About.aspx`), tudo funcionará conforme o esperado porque há uma subpasta `Images` relativa à pasta raiz. No entanto, as coisas se desdividirão se a página de conteúdo estiver em uma pasta diferente da página mestra. Para ilustrar isso, crie uma subpasta chamada `Admin`. Em seguida, adicione uma página de conteúdo chamada `Default.aspx` à pasta `Admin`, vinculando a nova página à página mestra `Site.master`.

> [!NOTE]
> No tutorial [*especificando o título, as marcas meta e outros cabeçalhos HTML na página mestra*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) , criamos uma classe de página de base personalizada chamada `BasePage` que define automaticamente o título da página de conteúdo (se ela não tiver sido explicitamente atribuída). Não se esqueça de ter a classe code-behind da página recém-criada, derivada de `BasePage` para que possa utilizar essa funcionalidade.

Depois de criar esta página de conteúdo, seu Gerenciador de Soluções deve ser semelhante à figura 1.

![Uma nova pasta e a página ASP.NET foram adicionadas ao projeto](urls-in-master-pages-vb/_static/image1.png)

**Figura 01**: uma nova pasta e a página ASP.net foram adicionadas ao projeto

Em seguida, atualize o arquivo de `Web.sitemap` para incluir uma nova entrada de `<siteMapNode>` para esta lição. O XML a seguir mostra a marcação de `Web.sitemap` completa, que agora inclui a adição de um terceiro elemento `<siteMapNode>`.

[!code-xml[Main](urls-in-master-pages-vb/samples/sample3.xml)]

A página de `Default.aspx` recém-criada deve ter quatro controles de conteúdo correspondentes aos quatro ContentPlaceHolders em `Site.master`. Adicione texto ao controle de conteúdo referenciando o `MainContent` ContentPlaceHolder e visite a página por meio de um navegador. Como mostra a Figura 2, o navegador não consegue localizar o arquivo de imagem `PoweredByASPNET.gif`. O que está acontecendo?

A página de conteúdo `~/Admin/Default.aspx` é enviada ao mesmo HTML para a região de `footerContent`, como foi a página `About.aspx`:

[!code-html[Main](urls-in-master-pages-vb/samples/sample4.html)]

Como o atributo `src` do elemento `<img>` é uma URL relativa, o navegador tenta procurar uma pasta `Images` em relação ao local da pasta da página da Web. Em outras palavras, o navegador está procurando o arquivo de imagem `Admin/Images/PoweredByASPNET.gif`.

[![o arquivo de imagem PoweredByASPNET. gif não foi encontrado](urls-in-master-pages-vb/_static/image3.png)](urls-in-master-pages-vb/_static/image2.png)

**Figura 02**: o arquivo de imagem de `PoweredByASPNET.gif` não pode ser encontrado ([clique para exibir a imagem em tamanho normal](urls-in-master-pages-vb/_static/image4.png))

### <a name="replacing-relative-urls-with-absolute-urls"></a>Substituindo URLs relativas por URLs absolutas

O oposto de uma URL relativa é uma *URL absoluta*, que é uma que começa com uma barra "/" (`/`) ou um protocolo como `http://`. Como uma URL absoluta especifica o local de um recurso de um ponto fixo conhecido, a mesma URL absoluta é válida em qualquer página da Web, independentemente da localização da página da Web na estrutura de pastas do site.

Para corrigir a imagem quebrada mostrada na Figura 2, precisamos atualizar o atributo `src` do elemento de `<img>` para que ele use uma URL absoluta em vez de uma relativa. Para determinar a URL absoluta correta, visite uma das páginas da Web em seu site e examine a barra de endereços. Como mostra a barra de endereços na Figura 2, o caminho totalmente qualificado para o aplicativo Web é `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/`. Portanto, poderíamos atualizar o atributo `src` do elemento `<img>` para uma das duas URLs absolutas a seguir:

- `/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`

Reserve um tempo para atualizar o atributo `src` do elemento de `<img>` para uma URL absoluta usando um dos formulários mostrados acima e, em seguida, visite a página `~/Admin/Default.aspx` por meio de um navegador. Desta vez, o navegador localizará e exibirá corretamente o arquivo de imagem `PoweredByASPNET.gif` (consulte a Figura 3).

[![a imagem PoweredByASPNET. gif agora é exibida](urls-in-master-pages-vb/_static/image6.png)](urls-in-master-pages-vb/_static/image5.png)

**Figura 03**: a imagem de `PoweredByASPNET.gif` agora é exibida ([clique para exibir a imagem em tamanho normal](urls-in-master-pages-vb/_static/image7.png))

Enquanto a codificação rígida na URL absoluta funciona, ela acopla rigidamente o HTML ao local do servidor e da pasta do site, o que pode ser alterado. O uso de uma URL absoluta do formulário `http://localhost:3908/...` é frágil porque o número da porta anterior a localhost é selecionado automaticamente sempre que o servidor Web de desenvolvimento ASP.NET interno do Visual Studio é iniciado. Da mesma forma, a parte `http://localhost` só é válida durante o teste local. Depois que o código for implantado em um servidor de produção, a URL base será alterada para outra coisa, como `http://www.yourserver.com`. A URL absoluta na forma `/ASPNET_MasterPages_Tutorial_04_VB/...` também sofre da mesma fragilidade, pois muitas vezes esse caminho de aplicativo difere entre os servidores de desenvolvimento e de produção.

A boa notícia é que o ASP.NET oferece um método para gerar uma URL relativa válida em tempo de execução.

## <a name="usingandresolveclienturl"></a>Usando`~`e`ResolveClientUrl`

Em vez de embutir em código uma URL absoluta, o ASP.NET permite que os desenvolvedores de página usem o til (`~`) para indicar a raiz do aplicativo Web. Por exemplo, anteriormente neste tutorial, usei a notação `~/Admin/Default.aspx` no texto para fazer referência à página `Default.aspx` na pasta `Admin`. O `~` indica que a pasta `Admin` é uma subpasta da raiz do aplicativo Web.

O [método`ResolveClientUrl`](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) da classe `Control` usa uma URL e a modifica para uma URL relativa apropriada para a página da Web na qual o controle reside. Por exemplo, chamar `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` de `About.aspx` retorna `Images/PoweredByASPNET.gif`. No entanto, chamá-lo de `~/Admin/Default.aspx`retorna `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Como todos os controles de servidor ASP.NET derivam da classe `Control`, todos os controles de servidor têm acesso ao método `ResolveClientUrl`. Até mesmo a classe `Page` deriva da classe `Control`, o que significa que você pode usar esse método diretamente das classes code-behind de suas páginas ASP.NET.

### <a name="usingin-the-declarative-markup"></a>Usando`~`na marcação declarativa

Vários controles da Web ASP.NET incluem propriedades relacionadas à URL: o controle HyperLink tem uma propriedade `NavigateUrl`; o controle de imagem tem uma propriedade `ImageUrl`; e assim por diante. Quando renderizado, esses controles passam seus valores de propriedade relacionados à URL para `ResolveClientUrl`. Consequentemente, se essas propriedades contiverem um `~` para indicar a raiz do aplicativo Web, a URL será modificada para uma URL relativa válida.

Tenha em mente que somente os controles de servidor ASP.NET transformam a `~` em suas propriedades relacionadas à URL. Se um `~` aparecer na marcação HTML estática, como `<img src="~/Images/PoweredByASPNET.gif" />`, o mecanismo ASP.NET enviará o `~` ao navegador junto com o restante do conteúdo HTML. O navegador pressupõe que o `~` é parte da URL. Por exemplo, se o navegador receber a marcação `<img src="~/Images/PoweredByASPNET.gif" />` ele assumirá que há uma subpasta chamada `~` com uma subpasta `Images` que contém o arquivo de imagem `PoweredByASPNET.gif`.

Para corrigir a marcação da imagem em `Site.master`, substitua o elemento `<img>` existente por um controle da Web de imagem ASP.NET. Defina o `ID` do controle da Web de imagem como `PoweredByImage`, sua propriedade `ImageUrl` como `~/Images/PoweredByASPNET.gif`e sua propriedade `AlternateText` como "alimentado por ASP.NET!"

[!code-aspx[Main](urls-in-master-pages-vb/samples/sample5.aspx)]

Depois de fazer essa alteração na página mestra, visite novamente a página `~/Admin/Default.aspx`. Desta vez, o arquivo de imagem `PoweredByASPNET.gif` aparece na página (veja a Figura 3). Quando o controle da Web de imagem é renderizado, ele usa o método `ResolveClientUrl` para resolver seu valor de propriedade `ImageUrl`. Em `~/Admin/Default.aspx` a `ImageUrl` é convertida em uma URL relativa apropriada, como mostra o seguinte trecho de código-fonte HTML:

[!code-html[Main](urls-in-master-pages-vb/samples/sample6.html)]

> [!NOTE]
> Além de serem usados em Propriedades de controle da Web baseadas em URL, o `~` também pode ser usado ao chamar os métodos `Response.Redirect` e `Server.MapPath`, entre outros. Além disso, o método `ResolveClientUrl` pode ser invocado diretamente de uma marcação declarativa de ASP.NET ou página mestra, se necessário; consulte a entrada de blog de [Fritz cebola](https://www.pluralsight.com/blogs/fritz/) [usando `ResolveClientUrl` na marcação](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).

## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Corrigindo as URLs relativas restantes da página mestra

Além do elemento `<img>` no `footerContent` que acabamos de corrigir, a página mestra contém uma URL mais relativa que requer nossa atenção. A região de `topContent` inclui o link "tutoriais de páginas mestras", que aponta para `Default.aspx`.

[!code-html[Main](urls-in-master-pages-vb/samples/sample7.html)]

Como essa URL é relativa, ela enviará o usuário para a página de `Default.aspx` na pasta da página de conteúdo que está visitando. Para que esse link aponte sempre para `Default.aspx` na pasta raiz, precisamos substituir o elemento `<a>` por um controle Web de hiperlink para que possamos usar a notação de `~`.

Remova a marcação do elemento `<a>` e adicione um controle HyperLink em seu lugar. Defina o `ID` do hiperlink como `lnkHome`, sua propriedade `NavigateUrl` como `~/Default.aspx`e sua propriedade `Text` como "tutoriais de páginas mestras".

[!code-aspx[Main](urls-in-master-pages-vb/samples/sample8.aspx)]

É isso! Neste ponto, todas as URLs em nossa página mestra são corretamente baseadas quando renderizadas por uma página de conteúdo, independentemente de quais pastas a página mestra e a página de conteúdo estão localizadas.

### <a name="automatic-url-resolution-in-theheadsection"></a>Resolução automática de URL na seção`<head>`

No tutorial [*criando um layout de todo o site usando páginas mestras*](creating-a-site-wide-layout-using-master-pages-vb.md) , adicionamos um `<link>` ao arquivo `Styles.css` na região `<head>`:

[!code-aspx[Main](urls-in-master-pages-vb/samples/sample9.aspx)]

Embora o atributo `href` do elemento de `<link>` seja relativo, ele é automaticamente convertido em um caminho apropriado em tempo de execução. Como discutimos no tutorial [*especificando o título, as marcas meta e outros cabeçalhos HTML no tópico da página mestra*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) , a região de `<head>` é, na verdade, um controle do servidor, o que permite modificar o conteúdo de seus controles internos quando ele é renderizado.

Para verificar isso, visite novamente a página `~/Admin/Default.aspx` e exiba a fonte HTML enviada para o navegador. Como o trecho de código a seguir ilustra, o atributo `href` do elemento de `<link>` foi modificado automaticamente para uma URL relativa apropriada, `../Styles.css`.

[!code-html[Main](urls-in-master-pages-vb/samples/sample10.html)]

## <a name="summary"></a>Resumo

As páginas mestras muitas vezes incluem links, imagens e outros recursos externos que devem ser especificados por meio de uma URL. Como a página mestra e as páginas de conteúdo podem não existir na mesma pasta, é importante abstain o uso de URLs relativas. Embora seja possível usar URLs absolutas embutidas em código, fazer isso rigidamente a URL absoluta para o aplicativo Web. Se a URL absoluta for alterada, como geralmente faz ao mover ou implantar um aplicativo Web, você precisará se lembrar de voltar e atualizar as URLs absolutas.

A abordagem ideal é usar o til (`~`) para indicar a raiz do aplicativo. Os controles da Web ASP.NET que contêm propriedades relacionadas à URL mapeiam o `~` para a raiz do aplicativo em tempo de execução. Internamente, os controles da Web usam o método `ResolveClientUrl` da classe `Control` para gerar uma URL relativa válida. Esse método é público e está disponível em cada controle de servidor (incluindo a classe `Page`), para que você possa usá-lo programaticamente de suas classes code-behind, se necessário.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Páginas mestras em ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [Rebase de URL em uma página mestra](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Usando `ResolveClientUrl` na marcação](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP. net e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 3,5 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott pode ser contatado em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
> [Próximo](control-id-naming-in-content-pages-vb.md)
