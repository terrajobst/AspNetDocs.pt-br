---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
title: Como fazer usar o controle do editor de HTML? (C#) | Microsoft Docs
author: microsoft
description: HTMLEditor é um controle ASP.NET AJAX que permite criar e editar facilmente conteúdo HTML por meio de botões em uma barra de ferramentas.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: f47e6224-c2e5-4472-b069-b6c7b6115200
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-cs
msc.type: authoredcontent
ms.openlocfilehash: cb7d75b59b1361abeb6d3c38ad6e42e34d6e3f7b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78577857"
---
# <a name="how-do-i-use-the-html-editor-control-c"></a>Como fazer usar o controle do editor de HTML? (C#)

pela [Microsoft](https://github.com/microsoft)

> HTMLEditor é um controle ASP.NET AJAX que permite criar e editar facilmente conteúdo HTML por meio de botões em uma barra de ferramentas.

O objetivo deste tutorial é fornecer uma visão geral do controle do editor de HTML incluído no AJAX Control Toolkit. O editor de HTML inclui opções para alterar o tamanho da fonte, selecionar uma fonte, alterar a cor do plano de fundo, modificar a cor do primeiro plano, adicionar links, adicionar imagens, alterar o alinhamento do texto e executar operações de recortar, copiar e colar (consulte a Figura 1).

[![o editor de HTML](how-do-i-use-the-html-editor-control-cs/_static/image1.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image1.png)

**Figura 01**: o editor de HTML ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-html-editor-control-cs/_static/image2.png))

O editor de HTML permite inserir conteúdo usando um modo de design ou você pode inserir o HTML diretamente. Você também é fornecido com a opção de visualizar seu conteúdo HTML (consulte a Figura 2).

[![design, HTML e botões de visualização](how-do-i-use-the-html-editor-control-cs/_static/image2.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image3.png)

**Figura 02**: botões de design, HTML e visualização ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-html-editor-control-cs/_static/image4.png))

Neste tutorial, você aprende a exibir o editor de HTML, como personalizar os botões da barra de ferramentas que aparecem no editor de HTML e como evitar ataques de script entre sites.

## <a name="displaying-the-html-editor"></a>Exibindo o editor de HTML

Antes de usar o editor de HTML em uma página ASP.NET, você deve primeiro adicionar um controle ScriptManager à página. O controle ScriptManager está localizado abaixo da guia extensões AJAX na caixa de ferramentas do Visual Studio/Visual Web Developer Express.

Você deve posicionar o controle do ScriptManager na parte superior da página antes de qualquer outro controle na página. Por exemplo, você pode colocá-lo imediatamente abaixo do formulário de &lt;de abertura do lado do servidor&gt; marca.

O controle editor de HTML está localizado na caixa de ferramentas com o restante dos controles AJAX Control Toolkit. Ele é chamado de controle editor (consulte a Figura 3).

[![o controle do editor de HTML](how-do-i-use-the-html-editor-control-cs/_static/image3.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image5.png)

**Figura 03**: o controle do editor de HTML ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-html-editor-control-cs/_static/image6.png))

Depois de arrastar o editor de HTML para uma página, você pode definir suas propriedades na folha de propriedades. Por exemplo, normalmente você deseja definir as propriedades width e Height. A listagem 1 contém a origem de uma página ASP.NET que contém um editor de HTML.

**Listagem 1-SimpleEditor. aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample1.aspx)]

A página na Listagem 1 contém um controle de editor de HTML, um controle de botão e um controle literal. Quando você clica no botão, o conteúdo do editor de HTML aparece no controle literal (consulte a Figura 4).

[![enviar um formulário com um editor de HTML](how-do-i-use-the-html-editor-control-cs/_static/image4.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image7.png)

**Figura 04**: enviando um formulário com um editor de HTML ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-html-editor-control-cs/_static/image8.png))

A propriedade content do editor de HTML é usada para recuperar o conteúdo HTML inserido no editor de HTML. Lembre-se de que esse conteúdo HTML pode conter JavaScript. Na próxima seção, discutiremos como você pode impedir ataques de injeção de JavaScript.

## <a name="customizing-the-html-editor-toolbar"></a>Personalizando a barra de ferramentas do editor de HTML

Você pode personalizar exatamente quais botões aparecem no editor. Por exemplo, talvez você queira remover a guia HTML para impedir que os usuários alternem o editor de HTML para o modo HTML. Ou talvez você queira remover a lista suspensa tamanho da fonte para impedir que os usuários criem texto excessivamente grande em uma postagem de mensagem do fórum (consulte a Figura 5).

[![um editor de HTML personalizado](how-do-i-use-the-html-editor-control-cs/_static/image5.jpg)](how-do-i-use-the-html-editor-control-cs/_static/image9.png)

**Figura 05**: um editor de HTML personalizado ([clique para exibir a imagem em tamanho normal](how-do-i-use-the-html-editor-control-cs/_static/image10.png))

Você personaliza os botões da barra de ferramentas derivando um novo editor de HTML da classe do editor de base. Por exemplo, o editor personalizado na Listagem 2 contém apenas botões da barra de ferramentas para negrito e itálico. Todos os outros botões da barra de ferramentas foram removidos. Além disso, a guia HTML foi removida da parte inferior do editor (mas as guias design e visualização ainda estão lá).

**Listagem 2-aplicativo\_Code\CustomEditor.cs**

[!code-csharp[Main](how-do-i-use-the-html-editor-control-cs/samples/sample2.cs)]

Você deve adicionar a classe na Listagem 2 ao seu aplicativo\_pasta código para que a classe seja compilada automaticamente. Se a pasta de código do\_de aplicativos não existir em seu site, você poderá simplesmente adicionar a pasta.

Depois de criar um editor personalizado, você pode adicioná-lo a uma página do ASP.NET da mesma maneira que adiciona o editor de HTML normal (consulte a listagem 3).

**Listagem 3-ShowCustomEditor. aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-cs/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Evitando ataques XSS (script entre sites)

Sempre que você aceitar a entrada de um usuário e Reexibir essa entrada em seu site, você potencialmente abrirá seu site para ataques XSS (scripts entre sites). Teoricamente, um hacker mal-intencionado poderia enviar código JavaScript que é executado quando a entrada é reexibida. O JavaScript pode ser usado para roubar senhas de usuário ou outras informações confidenciais.

Normalmente, você pode anular ataques de XSS por codificação HTML, seja qual for a entrada que você recuperar de um usuário antes de exibi-lo em uma página da Web. No entanto, codificar HTML a saída do editor de HTML não apenas codificaria &lt;script&gt; marcas, ele também codificaria todas as marcas HTML. Em outras palavras, você perderia toda a formatação, como o tipo de fonte, o tamanho da fonte e a cor do plano de fundo.

Se você estiver coletando informações confidenciais de seus usuários, como senhas, números de cartão de crédito e números de seguro social, não deverá exibir o conteúdo não codificado que você recupera de um usuário com o editor de HTML. Você deve usar o editor de HTML somente em situações em que você não está Reexibindo o conteúdo HTML, ou o conteúdo HTML está sendo enviado ao seu site por uma parte confiável.

Imagine, por exemplo, que você está criando um aplicativo de blog. Nessa situação, faz sentido usar o editor de HTML ao compor postagens no blog. Você é o único que envia uma postagem de blog e, presumivelmente, você pode confiar para não enviar JavaScript mal-intencionado. No entanto, não faz sentido usar o editor de HTML ao permitir que usuários anônimos publiquem comentários. Você deve ser especialmente cuidadoso em situações em que os usuários enviam informações confidenciais, como senhas. Potencialmente, um usuário mal-intencionado poderia postar um comentário que contenha o JavaScript certo para roubar uma senha.

## <a name="summary"></a>Resumo

Neste tutorial, você foi fornecido com uma visão geral resumida do controle do editor de HTML incluído no AJAX Control Toolkit. Você aprendeu a usar o editor de HTML para aceitar conteúdo avançado de um usuário e enviar o conteúdo para o servidor. Também discutimos como você pode personalizar os botões da barra de ferramentas que são exibidos pelo editor de HTML. Por fim, você aprendeu como evitar ataques de script entre sites ao usar o editor de HTML para aceitar entradas potencialmente mal-intencionadas.

> [!div class="step-by-step"]
> [Próximo](how-do-i-use-the-html-editor-control-vb.md)
