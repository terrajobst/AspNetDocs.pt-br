---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalando um auxiliar em um site Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este artigo descreve como instalar um auxiliar em um site Páginas da Web do ASP.NET (Razor). Um auxiliar é um componente reutilizável que inclui código e marcação para por...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 41e33c04a53a6ad257c3937cdadcec767e9217c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638582"
---
# <a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Instalando um auxiliar em um site Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como instalar um auxiliar em um site Páginas da Web do ASP.NET (Razor). Um *auxiliar* é um componente reutilizável que inclui código e marcação para executar uma tarefa que pode ser entediante ou complexa.
> 
> O que você aprenderá:
> 
> - Como instalar um auxiliar em um site criado usando o WebMatrix 3.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - WebMatrix 3

## <a name="overview-of-helpers"></a>Visão geral dos auxiliares

Algumas tarefas que as pessoas geralmente desejam fazer em páginas da Web exigem muito código ou exigem conhecimento extra. Os exemplos incluem a exibição de um gráfico para dados; colocando um botão "seguir" do Twitter em uma página; enviando emails do seu site; corte ou redimensionamento de imagens; usando o PayPal para seu site. Para facilitar esse tipo de coisas, Páginas da Web do ASP.NET permite que você use *auxiliares*. Os auxiliares são componentes que você instala para um site e que permitem executar tarefas típicas usando apenas uma linha ou duas de código do Razor.

Páginas da Web do ASP.NET tem alguns auxiliares internos. No entanto, muitos auxiliares estão disponíveis em pacotes (suplementos) que são fornecidos usando o Gerenciador de pacotes NuGet. O NuGet permite selecionar um pacote a ser instalado e cuida de todos os detalhes da instalação.

## <a name="installing-a-helper-in-webmatrix-3"></a>Instalando um auxiliar no WebMatrix 3

1. No WebMatrix 3, clique no botão **NuGet** .

    ![Caixa de diálogo Galeria do NuGet no WebMatrix](installing-helpers/_static/image1.png)
2. Isso inicia o Gerenciador de pacotes NuGet e exibe os pacotes disponíveis. Na caixa de pesquisa, insira uma palavra-chave para o auxiliar que você deseja instalar.

    ![Caixa de diálogo Galeria do NuGet no WebMatrix](installing-helpers/_static/image2.png)
3. Selecione o pacote e clique em **instalar**. Clique em **Sim** quando for perguntado se você deseja instalar o pacote e indicar que aceita os termos.

     Se esta for a primeira vez que você instalou um auxiliar, o NuGet criará pastas no seu site para o código que compõe o auxiliar.
4. Para desinstalar um auxiliar, clique no botão **Galeria** , clique na guia **instalado** e selecione o pacote que deseja desinstalar.

## <a name="installing-the-twitter-helper"></a>Instalando o auxiliar do Twitter

A versão mais recente da API do Twitter não é compatível com o auxiliar do Twitter que você instala por meio do NuGet. Em vez disso, consulte o tópico [auxiliar do Twitter com WebMatrix](twitter-helper.md) para obter informações sobre como configurar o auxiliar do Twitter em seu projeto.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

[Introdução ao Páginas da Web do ASP.NET 2-noções básicas de programação](../getting-started/introducing-razor-syntax-c.md)

[Auxiliar do Twitter com WebMatrix](twitter-helper.md)
