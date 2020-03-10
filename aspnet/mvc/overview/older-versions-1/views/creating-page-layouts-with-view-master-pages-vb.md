---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
title: Criando layouts de página com exibir páginas mestras (VB) | Microsoft Docs
author: microsoft
description: Neste tutorial, você aprenderá a criar um layout de página comum para várias páginas em seu aplicativo aproveitando as páginas mestras de exibição. Você pode usar um...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: d34f90a1-6de3-482a-a326-f87fdcbaaaff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 97c0ecf1953cc54030656dd710a5150243877110
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541219"
---
# <a name="creating-page-layouts-with-view-master-pages-vb"></a>Criar layouts de página com Exibir páginas mestras (VB)

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_VB.pdf)

> Neste tutorial, você aprenderá a criar um layout de página comum para várias páginas em seu aplicativo aproveitando as páginas mestras de exibição. Você pode usar uma página mestra de exibição, por exemplo, para definir um layout de página de duas colunas e usar o layout de duas colunas para todas as páginas em seu aplicativo Web.

## <a name="creating-page-layouts-with-view-master-pages"></a>Criando layouts de página com exibir páginas mestras

Neste tutorial, você aprenderá a criar um layout de página comum para várias páginas em seu aplicativo aproveitando as páginas mestras de exibição. Você pode usar uma página mestra de exibição, por exemplo, para definir um layout de página de duas colunas e usar o layout de duas colunas para todas as páginas em seu aplicativo Web.

Você também pode tirar proveito de exibir páginas mestras para compartilhar conteúdo comum em várias páginas em seu aplicativo. Por exemplo, você pode posicionar o logotipo do site, os links de navegação e os anúncios em faixa em uma página mestra de exibição. Dessa forma, cada página em seu aplicativo exibirá esse conteúdo automaticamente.

Neste tutorial, você aprenderá a criar uma nova página mestra de exibição e a criar uma nova página de conteúdo de exibição com base na página mestra.

### <a name="creating-a-view-master-page"></a>Criando uma página mestra de exibição

Vamos começar criando uma página mestra de exibição que define um layout de duas colunas. Adicione uma nova página mestra de exibição a um projeto MVC clicando com o botão direito do mouse na pasta Views\Shared, selecionando a opção de menu **Adicionar, novo item**e selecionando o modelo de página mestra de exibição do MVC (veja a Figura 1).

[![adicionar uma página mestra de exibição](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)

**Figura 01**: adicionando uma página mestra de exibição ([clique para exibir a imagem em tamanho normal](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))

Você pode criar mais de uma exibição de página mestra em um aplicativo. Cada página mestra de exibição pode definir um layout de página diferente. Por exemplo, talvez você queira que determinadas páginas tenham um layout de duas colunas e outras páginas tenham um layout de três colunas.

Uma página mestra de exibição parece muito parecida com uma exibição padrão do ASP.NET MVC. No entanto, ao contrário de um modo de exibição normal, uma página mestra de exibição contém uma ou mais marcas de `<asp:ContentPlaceHolder>`. As marcas de `<contentplaceholder>` são usadas para marcar as áreas da página mestra que podem ser substituídas em uma página de conteúdo individual.

Por exemplo, a página de exibição mestra na Listagem 1 define um layout de duas colunas. Ele contém duas marcas de `<contentplaceholder>`. Uma `<ContentPlaceHolder>` para cada coluna.

**Listagem 1 – `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample1.aspx)]

O corpo da página mestra de exibição na Listagem 1 contém duas marcas de `<div>` que correspondem às duas colunas. A classe de coluna de folha de estilos em cascata é aplicada a ambas as marcas `<div>`. Essa classe é definida na folha de estilos declarada na parte superior da página mestra. Você pode visualizar como a página de exibição mestra será renderizada alternando para modo de exibição de Design. Clique na guia Design na parte inferior esquerda do editor de código-fonte (consulte a Figura 2).

[![Visualizar uma página mestra no designer](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)

**Figura 02**: Visualizar uma página mestra no designer ([clique para exibir a imagem em tamanho normal](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))

### <a name="creating-a-view-content-page"></a>Criando uma página exibir conteúdo

Depois de criar uma página mestra de exibição, você pode criar uma ou mais páginas de conteúdo de exibição com base na página de exibição mestra. Por exemplo, você pode criar uma página de conteúdo de exibição de índice para o controlador Home clicando com o botão direito do mouse na pasta Views\Home, selecionando **Adicionar, novo item**, selecionando o modelo de **página de conteúdo de exibição do MVC** , inserindo o nome index. aspx e clicando no botão Adicionar (consulte a Figura 3).

[![adicionar uma página exibir conteúdo](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)

**Figura 03**: adicionando uma página exibir conteúdo ([clique para exibir a imagem em tamanho normal](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))

Depois que você clicar no botão Adicionar, será exibida uma nova caixa de diálogo que permite selecionar uma página mestra de exibição a ser associada à página exibir conteúdo (consulte a Figura 4). Você pode navegar até a página mestra do site. modo de exibição mestre que criamos na seção anterior.

[![selecionando uma página mestra](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)

**Figura 04**: selecionando uma página mestra ([clique para exibir a imagem em tamanho normal](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))

Depois de criar uma nova página de conteúdo de exibição com base na página mestra site. Master, você obtém o arquivo na Listagem 2.

**Listagem 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample2.aspx)]

Observe que essa exibição contém uma marca `<asp:Content>` que corresponde a cada uma das marcas de `<asp:ContentPlaceHolder>` na página de exibição mestra. Cada marca de `<asp:Content>` inclui um atributo ContentPlaceHolderid que aponta para o `<asp:ContentPlaceHolder>` específico que ele substitui.

Observe, além disso, que a página de exibição de conteúdo na Listagem 2 não contém nenhuma das marcas HTML de abertura e fechamento normais. Por exemplo, ele não contém as marcas de abertura e fechamento `<html>` ou `<head>`. Todas as marcas normais de abertura e fechamento estão contidas na página de exibição mestra.

Todo o conteúdo que você deseja exibir em uma página exibir conteúdo deve ser colocado dentro de uma marca de `<asp:Content>`. Se você colocar qualquer HTML ou outro conteúdo fora dessas marcas, receberá um erro ao tentar exibir a página.

Você não precisa substituir todas as `<asp:ContentPlaceHolder>` marca de uma página mestra em uma página de exibição de conteúdo. Você só precisa substituir uma marca de `<asp:ContentPlaceHolder>` quando deseja substituir a marca por um conteúdo específico.

Por exemplo, a exibição de índice modificada na Listagem 3 contém apenas duas marcas de `<asp:Content>`. Cada uma das marcas de `<asp:Content>` inclui algum texto.

**Listagem 3 – `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample3.aspx)]

Quando a exibição na Listagem 3 é solicitada, ela renderiza a página na Figura 5. Observe que a exibição renderiza uma página com duas colunas. Observe, além disso, que o conteúdo da página exibir conteúdo é mesclado com o conteúdo da página de exibição mestra.

[![a página de conteúdo de exibição de índice](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)

**Figura 05**: a página de conteúdo de exibição de índice ([clique para exibir a imagem em tamanho normal](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))

### <a name="modifying-view-master-page-content"></a>Modificando o conteúdo da página mestra de exibição

Um problema que você encontra quase imediatamente ao trabalhar com o modo de exibição de páginas mestras é o problema de modificar o conteúdo da página mestra de exibição quando páginas de conteúdo de exibição diferentes são solicitadas. Por exemplo, você deseja que cada página em seu aplicativo Web tenha um título exclusivo. No entanto, o título é declarado na página de exibição mestra e não na página exibir conteúdo. Então, como você personaliza o título da página para cada página de conteúdo de exibição?

Há duas maneiras de modificar o título exibido por uma página exibir conteúdo. Primeiro, você pode atribuir um título de página ao atributo title da diretiva `<%@ page %>` declarada na parte superior de uma página exibir conteúdo. Por exemplo, se você quiser atribuir o título de página "super ótimo site" à exibição de índice, poderá incluir a seguinte diretiva na parte superior da exibição de índice:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample4.aspx)]

Quando a exibição do índice é renderizada para o navegador, o título desejado aparece na barra de título do navegador:

[barra de título do ![navegador](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)

Há um requisito importante que uma página de exibição mestra deve satisfazer para que o atributo title funcione. A página mestra de exibição deve conter uma marca de `<head runat="server">` em vez de uma marca de `<head>` normal para seu cabeçalho. Se a marca de `<head>` não incluir o atributo runat = "Server", o título não aparecerá. A página mestra de exibição padrão inclui a marca de `<head runat="server">` necessária.

Uma abordagem alternativa para modificar o conteúdo da página mestra de uma página de conteúdo de exibição individual é encapsular a região que você deseja modificar em uma marca de `<asp:ContentPlaceHolder>`. Por exemplo, imagine que você deseja alterar não apenas o título, mas também as marcas meta, renderizadas por uma página de exibição mestra. A página de exibição mestra na Listagem 4 contém uma marca de `<asp:ContentPlaceHolder>` dentro de sua marca de `<head>`.

**Listagem 4 – `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample5.aspx)]

Observe que a marca de `<asp:ContentPlaceHolder>` na Listagem 4 inclui conteúdo padrão: um título padrão e marcas meta padrão. Se você não substituir essa marca de `<asp:ContentPlaceHolder>` em uma página de conteúdo de exibição individual, o conteúdo padrão será exibido.

A página exibição de conteúdo na listagem 5 substitui a marca de `<asp:ContentPlaceHolder>` para exibir um título personalizado e marcas meta personalizadas.

**Listagem 5 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample6.aspx)]

### <a name="summary"></a>Resumo

Este tutorial forneceu uma introdução básica para exibir páginas mestras e exibir páginas de conteúdo. Você aprendeu a criar novas páginas mestras de exibição e a criar páginas de conteúdo de exibição com base nelas. Também examinamos como você pode modificar o conteúdo de uma página mestra de exibição de uma página de conteúdo de exibição específica.

> [!div class="step-by-step"]
> [Anterior](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
> [Próximo](passing-data-to-view-master-pages-vb.md)
