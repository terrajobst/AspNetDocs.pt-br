---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Criando e usando um auxiliar em um site Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este artigo descreve como criar um auxiliar em um site Páginas da Web do ASP.NET (Razor). Um auxiliar é um componente reutilizável que inclui código e marcação para perf...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 380663951094c9fc7d5f0601e30995fa073a204b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563507"
---
# <a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Criando e usando um auxiliar em um site Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como criar um auxiliar em um site Páginas da Web do ASP.NET (Razor). Um *auxiliar* é um componente reutilizável que inclui código e marcação para executar uma tarefa que pode ser entediante ou complexa.
> 
> **O que você aprenderá:** 
> 
> - Como criar e usar um auxiliar simples.
> 
> Estes são os recursos do ASP.NET apresentados no artigo:
> 
> - A sintaxe de `@helper`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com o Páginas da Web do ASP.NET 2.

## <a name="overview-of-helpers"></a>Visão geral dos auxiliares

Se você precisar executar as mesmas tarefas em páginas diferentes em seu site, poderá usar um auxiliar. O Páginas da Web do ASP.NET inclui vários auxiliares, e há muito mais que você pode baixar e instalar. (Uma lista dos auxiliares internos no Páginas da Web do ASP.NET está listada na [referência rápida da API ASP.net](https://go.microsoft.com/fwlink/?LinkId=202907).) Se nenhum dos auxiliares existentes atender às suas necessidades, você poderá criar seu próprio auxiliar.

Um auxiliar permite que você use um bloco comum de código em várias páginas. Suponha que, em sua página, você geralmente queira criar um item de nota que seja separado de parágrafos normais. Talvez a observação seja criada como um elemento `<div>` com o estilo de uma caixa com uma borda. Em vez de adicionar essa mesma marcação a uma página toda vez que você quiser exibir uma observação, você pode empacotar a marcação como um auxiliar. Em seguida, você pode inserir a nota com uma única linha de código em qualquer lugar necessário.

Usar um auxiliar como esse torna o código em cada uma das páginas mais simples e fácil de ler. Ele também facilita a manutenção de seu site, porque se você precisar alterar a aparência das observações, poderá alterar a marcação em um único lugar.

## <a name="creating-a-helper"></a>Criando um auxiliar

Este procedimento mostra como criar o auxiliar que cria a observação, como acabei de descrever. Esse é um exemplo simples, mas o auxiliar personalizado pode incluir qualquer marcação e código ASP.NET necessário.

1. Na pasta raiz do site, crie uma pasta chamada *App\_código*. Esse é um nome de pasta reservado em ASP.NET, onde você pode colocar código para componentes como auxiliares.
2. Na pasta *código de\_de aplicativo* , crie um novo arquivo *. cshtml* e nomeie-o como *myhelprs. cshtml*.
3. Substitua o conteúdo existente pelo seguinte:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    O código usa a sintaxe `@helper` para declarar um novo auxiliar chamado `MakeNote`. Esse auxiliar específico permite passar um parâmetro chamado `content` que pode conter uma combinação de texto e marcação. O auxiliar insere a cadeia de caracteres no corpo da nota usando a variável `@content`.

    Observe que o arquivo é chamado de *Myhelper. cshtml*, mas o auxiliar é chamado de `MakeNote`. Você pode colocar vários auxiliares personalizados em um único arquivo.
4. Salve e feche o arquivo.

## <a name="using-the-helper-in-a-page"></a>Usando o auxiliar em uma página

1. Na pasta raiz, crie um novo arquivo em branco chamado *TestHelper. cshtml*.
2. Adicione o seguinte código ao arquivo:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Para chamar o auxiliar que você criou, use `@` seguido pelo nome do arquivo em que o auxiliar é, um ponto e o nome do auxiliar. (Se você tiver várias pastas na pasta de *código do\_de aplicativos* , poderá usar a sintaxe `@FolderName.FileName.HelperName` para chamar o auxiliar em qualquer nível de pasta aninhada). O texto que você adiciona entre aspas dentro dos parênteses é o texto que o auxiliar exibirá como parte da nota na página da Web.
3. Salve a página e execute-a em um navegador. O auxiliar gera o item de observação logo em que você chamou o auxiliar: entre os dois parágrafos.

    ![Captura de tela mostrando a página no navegador e como o auxiliar gerou a marcação que coloca uma caixa em volta do texto especificado.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.png)

## <a name="additional-resources"></a>Recursos adicionais

[Menu horizontal como um auxiliar do Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). Esta entrada de blog de Mike Papa mostra como criar um menu horizontal como um auxiliar usando marcação, CSS e código.

[Aproveitando o HTML5 nos auxiliares páginas da Web do ASP.net para WebMatrix e ASP.net MvC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Essa entrada de blog por Sam Abraham mostra um auxiliar que renderiza um elemento `Canvas` HTML5.

[A diferença entre @Helpers e @Functions no WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). Esta entrada de blog de Mike Brind descreve `@helper` sintaxe e sintaxe de `@function` e quando usar cada um.
