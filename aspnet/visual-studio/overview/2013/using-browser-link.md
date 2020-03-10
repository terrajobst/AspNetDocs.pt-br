---
uid: visual-studio/overview/2013/using-browser-link
title: Usando o link do navegador no Visual Studio 2013 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 723a38de4569b0bb58817c70aabb84fef8e19591
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622902"
---
# <a name="using-browser-link-in-visual-studio-2013"></a>Usando o link do navegador no Visual Studio 2013

por [Mike Wasson](https://github.com/MikeWasson)

O link do navegador é um novo recurso do Visual Studio 2013 que cria um canal de comunicação entre o ambiente de desenvolvimento e um ou mais navegadores da Web. Você pode usar o link do navegador para atualizar seu aplicativo Web em vários navegadores de uma vez, o que é útil para testes entre navegadores.

- [Atualização do navegador](#browser-refresh)
- [Exibindo o painel do link do navegador](#dashboard)
- [Habilitando o link do navegador para arquivos HTML estáticos](#static-html)
- [Desativando o link do navegador](#disabling)
- [Como funciona?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Atualização do navegador

Com a atualização do navegador, você pode atualizar vários navegadores que estão conectados ao Visual Studio por meio do link do navegador.

Para usar a atualização do navegador, primeiro crie um aplicativo ASP.NET, usando qualquer um dos modelos de projeto. Depure o aplicativo pressionando F5 ou clicando no ícone de seta na barra de ferramentas:

![](using-browser-link/_static/image1.png)

Você também pode usar a lista suspensa para selecionar um navegador específico para depuração.

![](using-browser-link/_static/image2.png)

Para depurar com vários navegadores, selecione **procurar com**. Na caixa de diálogo **procurar com** , mantenha pressionada a tecla CTRL para selecionar mais de um navegador. Clique em **procurar** para depurar com os navegadores selecionados. O link do navegador também funcionará se você iniciar um navegador fora do Visual Studio e navegar até a URL do aplicativo.

![](using-browser-link/_static/image3.png)

Os controles de link do navegador estão localizados no menu suspenso com o ícone de seta circular. O ícone de seta é o botão **Atualizar** .

![](using-browser-link/_static/image4.png)

Para ver quais navegadores estão conectados, passe o mouse sobre o botão **Atualizar** durante a depuração. Os navegadores conectados são mostrados em uma janela de dica de ferramenta.

![](using-browser-link/_static/image5.png)

Para atualizar os navegadores conectados, clique no botão **Atualizar** ou pressione Ctrl + Alt + Enter. Por exemplo, a captura de tela a seguir mostra um projeto ASP.NET, que criei usando o modelo de projeto MVC 5. Você pode ver o aplicativo em execução em dois navegadores na parte superior. Na parte inferior, o projeto é aberto no Visual Studio.

![](using-browser-link/_static/image6.png)

No Visual Studio, alterei o rumo &lt;H1&gt; para o home page:

![](using-browser-link/_static/image7.png)

Quando eu cliquei no botão **Atualizar** , a alteração apareceu nas janelas do navegador:

![](using-browser-link/_static/image8.png)

**Observações**

- Para habilitar o link do navegador, defina `debug=true` no elemento [&lt;de compilação&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) no arquivo Web. config do projeto.
- O aplicativo deve estar em execução no localhost.
- O aplicativo deve ter como destino o .NET 4,0 ou posterior.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Exibindo o painel do link do navegador

O painel link do navegador mostra informações sobre as conexões de link do navegador. Para exibir o painel, selecione o menu suspenso link do navegador (a pequena seta ao lado do botão **Atualizar** ). Em seguida, clique em **painel de link do navegador**.

![](using-browser-link/_static/image9.png)

O painel lista os navegadores conectados e a URL para a qual cada navegador navegou.

![](using-browser-link/_static/image10.png)

A seção **pré-requisitos** mostra as etapas necessárias para habilitar o link do navegador para esse projeto. Por exemplo, a captura de tela a seguir mostra um projeto em que "debug" está definido como false no arquivo Web. config.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Habilitando o link do navegador para arquivos HTML estáticos

Para habilitar o link do navegador para arquivos HTML estáticos, adicione o seguinte ao seu arquivo Web. config.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Por motivos de desempenho, remova essa configuração ao publicar seu projeto.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Desativando o link do navegador

O link do navegador está habilitado por padrão. Há várias maneiras de desabilitá-lo:

- No menu suspenso link do navegador, desmarque **habilitar link do navegador**. 

    ![](using-browser-link/_static/image12.png)
- No arquivo Web. config, adicione uma chave chamada "vs: EnableBrowserLink" com o valor "false" na seção appSettings. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- No arquivo Web. config, defina depurar como false. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Como funciona?

O link do navegador usa o [signalr](../../../signalr/index.md) para criar um canal de comunicação entre o Visual Studio e o navegador. Quando o link do navegador está habilitado, o Visual Studio atua como um servidor de sinalização ao qual vários clientes (navegadores) podem se conectar. O link do navegador também registra um módulo HTTP com ASP.NET. Esse módulo injeta &lt;de script especiais&gt; referências a todas as solicitações de página do servidor. Você pode ver as referências de script selecionando "Exibir origem" no navegador.

![](using-browser-link/_static/image13.png)

Os arquivos de origem não são modificados. O módulo HTTP injeta as referências de script dinamicamente.

Como o código do lado do navegador é todo o JavaScript, ele funciona em todos os navegadores que o [signalr dá suporte](../../../signalr/overview/getting-started/supported-platforms.md), sem a necessidade de nenhum plug-in de navegador.
