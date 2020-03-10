---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Introdução ao AJAX Control Toolkit (C#) | Microsoft Docs
author: microsoft
description: Saiba tudo o que você precisa saber para começar a usar o AJAX Control Toolkit.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 7478b090ec52778572d70065983de6be8bdb4e6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78621600"
---
# <a name="get-started-with-the-ajax-control-toolkit-c"></a>Introdução ao AJAX Control Toolkit (C#)

pela [Microsoft](https://github.com/microsoft)

> Saiba tudo o que você precisa saber para começar a usar o AJAX Control Toolkit.

O AJAX Control Toolkit contém mais de 30 controles gratuitos que você pode usar em seus aplicativos ASP.NET. Neste tutorial, você aprenderá a baixar o AJAX Control Toolkit e adicionar os controles do kit de ferramentas à sua caixa de ferramentas do Visual Studio/Visual Web Developer Express.

## <a name="downloading-the-ajax-control-toolkit"></a>Baixando o AJAX Control Toolkit

O [AJAX Control Toolkit](http://devexpress.com/act) é um projeto de software livre desenvolvido pelos membros da comunidade do ASP.net e da equipe do ASP.net. 

[![baixar o AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**Figura 01**: baixando o AJAX Control Toolkit ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))

Depois de baixar o arquivo, você precisará desbloquear o arquivo. Clique com o botão direito do mouse no arquivo, selecione Propriedades e clique no botão **desbloquear** (veja a Figura 2).

[![desbloquear o arquivo ZIP do AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**Figura 02**: desbloqueio do arquivo zip do AJAX Control Toolkit ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))

Depois de desbloquear o arquivo, você pode descompactar o arquivo: clique com o botão direito do mouse no arquivo e selecione a opção de menu **extrair tudo** . Agora, estamos prontos para adicionar o Toolkit à caixa de ferramentas do Visual Studio/Visual Web Developer.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Adicionando o AJAX Control Toolkit à caixa de ferramentas

A maneira mais fácil de usar o AJAX Control Toolkit é adicionar o Toolkit à sua caixa de ferramentas do Visual Studio/Visual Web Developer (consulte a Figura 3). Dessa forma, você pode simplesmente arrastar um controle do kit de ferramentas para uma página quando quiser usá-lo.

[![AJAX Control Toolkit aparece na caixa de ferramentas](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**Figura 03**: o AJAX Control Toolkit aparece na caixa de ferramentas ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))

Primeiro, você precisa adicionar uma guia do AJAX Control Toolkit à caixa de ferramentas. Siga estas etapas.

1. Crie um novo site do ASP.NET selecionando o arquivo de opção de menu, novo site. Clique duas vezes em Default. aspx na janela Gerenciador de Soluções para abrir o arquivo no editor.
2. Clique com o botão direito do mouse na caixa de ferramentas abaixo da guia geral e selecione a opção de menu **Adicionar guia** (consulte a Figura 4).
3. Insira uma nova guia chamada AJAX Control Toolkit.

[![adicionar uma nova guia](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**Figura 04**: adicionando uma nova guia ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))

Em seguida, você precisa adicionar os controles do AJAX Control Toolkit à nova guia. Siga estas etapas:

- Clique com o botão direito do mouse abaixo da guia AJAX Control Toolkit e selecione a opção de menu **escolher itens (veja a Figura 5)** .
- Navegue até o local onde você descompactou o AJAX Control Toolkit e selecione o assembly AjaxControlToolkit. dll.

[![escolher itens para adicionar à caixa de ferramentas](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**Figura 05**: escolher itens para adicionar à caixa de ferramentas ([clique para exibir a imagem em tamanho normal](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))

Depois de concluir essas etapas, todos os controles do kit de ferramentas serão exibidos na caixa de ferramentas.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Atualizando para uma nova versão do kit de ferramentas

Se você estava usando uma versão mais antiga do kit de ferramentas e agora precisa migrar para uma versão posterior aqui estão as etapas recomendadas:

- Binários – exclua a versão antiga do assembly AjaxControlToolkit. dll da pasta bin do seu site.
- Itens da caixa de ferramentas – exclua a guia AJAX Control Toolkit e siga as etapas acima para recriar a guia com a nova versão do assembly AjaxControlToolkit. dll.

> [!div class="step-by-step"]
> [Próximo](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
