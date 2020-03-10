---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
title: Executando a validação simples (VB) | Microsoft Docs
author: StephenWalther
description: Saiba como executar a validação em um aplicativo MVC ASP.NET. Neste tutorial, Stephen Walther apresenta o estado de modelo e o auxiliar HTML de validação...
ms.author: riande
ms.date: 03/02/2009
ms.assetid: df6cf4b7-0bb3-4c4e-b17a-bd78a759a6bc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-vb
msc.type: authoredcontent
ms.openlocfilehash: 46925f22b7dfc23f2bb89b8d2fff0cbd8ae49062
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78542927"
---
# <a name="performing-simple-validation-vb"></a>Realizar validação simples (VB)

por [Stephen Walther](https://github.com/StephenWalther)

> Saiba como executar a validação em um aplicativo MVC ASP.NET. Neste tutorial, Stephen Walther apresenta o estado de modelo e os auxiliares HTML de validação.

O objetivo deste tutorial é explicar como você pode executar a validação em um aplicativo MVC ASP.NET. Por exemplo, você aprende a impedir que alguém envie um formulário que não contém um valor para um campo obrigatório. Você aprende a usar o estado do modelo e os auxiliares HTML de validação.

## <a name="understanding-model-state"></a>Entendendo o estado do modelo

Você usa o estado do modelo-ou com mais precisão, o dicionário de estado do modelo – para representar erros de validação. Por exemplo, a ação criar () na Listagem 1 valida as propriedades de uma classe Product antes de adicionar a classe Product a um banco de dados.

Não estou recomendando que você adicione sua validação ou lógica de banco de dados a um controlador. Um controlador deve conter apenas lógica relacionada ao controle de fluxo do aplicativo. Estamos adotando um atalho para manter as coisas simples.

**Listagem 1-Controllers\ProductController.vb**

[!code-vb[Main](performing-simple-validation-vb/samples/sample1.vb)]

Na Listagem 1, as propriedades Name, Description e UnidadesEmEstoque da classe Product são validadas. Se qualquer uma dessas propriedades falhar em um teste de validação, um erro será adicionado ao dicionário de estado do modelo (representado pela Propriedade ModelState da classe Controller).

Se houver erros no estado do modelo, a Propriedade ModelState. IsValid retornará false. Nesse caso, o formulário HTML para a criação de um novo produto é exibido novamente. Caso contrário, se não houver nenhum erro de validação, o novo produto será adicionado ao banco de dados.

## <a name="using-the-validation-helpers"></a>Usando os auxiliares de validação

A estrutura MVC do ASP.NET inclui dois auxiliares de validação: o auxiliar HTML. ValidationMessage () e o auxiliar HTML. ValidationSummary (). Você usa esses dois auxiliares em uma exibição para exibir mensagens de erro de validação.

Os auxiliares HTML. ValidationMessage () e HTML. ValidationSummary () são usados nas exibições criar e editar que são geradas automaticamente pelo scaffolding MVC do ASP.NET. Siga estas etapas para gerar o modo de exibição de criação:

1. Clique com o botão direito do mouse na ação criar () no controlador do produto e selecione a opção de menu **Adicionar modo de exibição** (consulte a Figura 1).
2. No diálogo **Adicionar exibição** , marque a caixa de seleção **criar uma exibição fortemente tipada** (consulte a Figura 2).
3. Na lista suspensa da **classe exibir dados** , selecione a classe produto.
4. Na lista suspensa **Exibir conteúdo** , selecione criar.
5. Clique no botão **Adicionar** .

Certifique-se de compilar seu aplicativo antes de adicionar um modo de exibição. Caso contrário, a lista de classes não aparecerá na lista suspensa **Exibir classe de dados** .

[![caixa de diálogo novo projeto](performing-simple-validation-vb/_static/image1.jpg)](performing-simple-validation-vb/_static/image1.png)

**Figura 01**: adicionando uma exibição ([clique para exibir a imagem em tamanho normal](performing-simple-validation-vb/_static/image2.png))

[![caixa de diálogo novo projeto](performing-simple-validation-vb/_static/image2.jpg)](performing-simple-validation-vb/_static/image3.png)

**Figura 02**: criando uma exibição fortemente tipada ([clique para exibir a imagem em tamanho normal](performing-simple-validation-vb/_static/image4.png))

Depois de concluir essas etapas, você obterá a exibição criar na Listagem 2.

**Listagem 2-Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-vb/samples/sample2.aspx)]

Na Listagem 2, o auxiliar HTML. ValidationSummary () é chamado imediatamente acima do formulário HTML. Esse auxiliar é usado para exibir uma lista de mensagens de erro de validação. O auxiliar HTML. ValidationSummary () renderiza os erros em uma lista com marcadores.

O auxiliar HTML. ValidationMessage () é chamado ao lado de cada um dos campos de formulário HTML. Esse auxiliar é usado para exibir uma mensagem de erro ao lado de um campo de formulário. No caso da listagem 2, o auxiliar HTML. ValidationMessage () exibe um asterisco quando há um erro.

A página na Figura 3 ilustra as mensagens de erro renderizadas pelos auxiliares de validação quando o formulário é enviado com campos ausentes e valores inválidos.

[![caixa de diálogo novo projeto](performing-simple-validation-vb/_static/image3.jpg)](performing-simple-validation-vb/_static/image5.png)

**Figura 03**: a exibição criar foi enviada com problemas ([clique para exibir a imagem em tamanho normal](performing-simple-validation-vb/_static/image6.png))

Observe que a aparência dos campos de entrada HTML também é modificada quando há um erro de validação. O auxiliar HTML. TextBox () renderiza um atributo *Class = "entrada-validação-erro"* quando há um erro de validação associado à propriedade renderizada pelo auxiliar HTML. TextBox ().

Há três classes de folha de estilo em cascata usadas para controlar a aparência de erros de validação:

- entrada-validação-erro-aplicado à marca de&gt; de entrada de &lt;renderizada pelo auxiliar HTML. TextBox ().
- campo-validação-erro-aplicado ao &lt;span&gt; marcação renderizado pelo auxiliar HTML. ValidationMessage ().
- validação-resumo-erros-aplicados à &lt;UL&gt; marca renderizada pelo auxiliar HTML. ValidationSummary ().

Você pode modificar essas classes de folha de estilo em cascata e, portanto, modificar a aparência dos erros de validação, modificando o arquivo site. css localizado na pasta conteúdo.

> [!NOTE] 
> 
> A classe HtmlHelper inclui propriedades estáticas somente leitura para recuperar os nomes das classes CSS relacionadas à validação. Essas propriedades estáticas são nomeadas ValidationInputCssClassName, ValidationFieldCssClassName e ValidationSummaryCssClassName.

## <a name="prebinding-validation-and-postbinding-validation"></a>Validação de prebindion e Postbinding

Se você enviar o formulário HTML para criar um produto e inserir um valor inválido para o campo preço e nenhum valor para o campo UnidadesEmEstoque, você obterá as mensagens de validação exibidas na Figura 4. De onde vêm essas mensagens de erro de validação?

[![caixa de diálogo novo projeto](performing-simple-validation-vb/_static/image4.jpg)](performing-simple-validation-vb/_static/image7.png)

**Figura 04**: erros de validação de prebinding ([clique para exibir a imagem em tamanho normal](performing-simple-validation-vb/_static/image8.png))

Na verdade, há dois tipos de mensagens de erro de validação – aquelas geradas antes que os campos de formulário HTML sejam associados a uma classe e aqueles gerados depois que os campos de formulário são associados à classe. Em outras palavras, há erros de validação de prebind e erros de validação de postbinding.

A ação criar () exposta pelo controlador do produto na Listagem 1 aceita uma instância da classe Product. A assinatura do método Create é parecida com esta:

[!code-vb[Main](performing-simple-validation-vb/samples/sample3.vb)]

Os valores dos campos de formulário HTML do formulário criar são associados à classe productToCreate por algo chamado de associador de modelo. O associador de modelo padrão adiciona uma mensagem de erro ao estado do modelo automaticamente quando não é possível associar um campo de formulário a uma propriedade de formulário.

O associador de modelo padrão não pode associar a cadeia de caracteres "Apple" à propriedade Price da classe Product. Não é possível atribuir uma cadeia de caracteres a uma propriedade decimal. Portanto, o associador de modelo adiciona um erro ao estado do modelo.

O associador de modelo padrão também não pode atribuir o valor nada a uma propriedade que não aceite o valor Nothing. Em particular, o associador de modelo não pode atribuir o valor nada à propriedade UnidadesEmEstoque. Mais uma vez, o associador de modelo abre e adiciona uma mensagem de erro ao estado do modelo.

Se desejar personalizar a aparência dessas mensagens de erro de preligação, você precisará criar cadeias de caracteres de recurso para essas mensagens.

## <a name="summary"></a>Resumo

O objetivo deste tutorial era descrever a mecânica básica de validação no ASP.NET MVC Framework. Você aprendeu a usar o estado do modelo e os auxiliares HTML de validação. Também discutimos a distinção entre a validação de prebind e de postbinding. Em outros tutoriais, discutiremos várias estratégias para mover seu código de validação para fora de seus controladores e para suas classes de modelo.

> [!div class="step-by-step"]
> [Anterior](displaying-a-table-of-database-data-vb.md)
> [Próximo](validating-with-the-idataerrorinfo-interface-vb.md)
