---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Criando uma restrição de rotaC#() | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther demonstra como você pode controlar como o navegador solicita as rotas correspondentes criando restrições de rota com expressões regulares.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 51ef859287b3424faf85f4a3606a220ab48a9466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78582008"
---
# <a name="creating-a-route-constraint-c"></a>Criação de uma restrição de rota (C#)

por [Stephen Walther](https://github.com/StephenWalther)

> Neste tutorial, Stephen Walther demonstra como você pode controlar como o navegador solicita as rotas correspondentes criando restrições de rota com expressões regulares.

Você usa restrições de rota para restringir as solicitações do navegador que correspondem a uma rota específica. Você pode usar uma expressão regular para especificar uma restrição de rota.

Por exemplo, imagine que você definiu a rota na Listagem 1 em seu arquivo global. asax.

**Listagem 1-Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

A listagem 1 contém uma rota chamada Product. Você pode usar a rota do produto para mapear solicitações do navegador para o ProductController contido na Listagem 2.

**Listagem 2-Controllers\ProductController.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

Observe que a ação Details () exposta pelo controlador do produto aceita um único parâmetro chamado productId. Esse parâmetro é um parâmetro inteiro.

A rota definida na Listagem 1 corresponderá a qualquer uma das seguintes URLs:

- /Product/23
- /Product/7

Infelizmente, a rota também corresponderá às seguintes URLs:

- /Product/blah
- /Product/apple

Como a ação Details () espera um parâmetro inteiro, fazer uma solicitação que contém algo diferente de um valor inteiro causará um erro. Por exemplo, se você digitar a URL/Product/Apple em seu navegador, receberá a página de erro na Figura 1.

[![caixa de diálogo novo projeto](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)

**Figura 01**: ver um detalhamento de página ([clique para exibir a imagem em tamanho normal](creating-a-route-constraint-cs/_static/image2.png))

O que você realmente deseja fazer é corresponder apenas a URLs que contenham um número inteiro apropriado de productId. Você pode usar uma restrição ao definir uma rota para restringir as URLs que correspondem à rota. A rota de produto modificada na Listagem 3 contém uma restrição de expressão regular que só corresponde a inteiros.

**Listagem 3-Global.asax.cs**

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

A expressão regular \D + corresponde a um ou mais inteiros. Essa restrição faz com que a rota do produto corresponda às seguintes URLs:

- /Product/3
- /Product/8999

Mas não as seguintes URLs:

- /Product/apple
- /Product

- Essas solicitações de navegador serão tratadas por outra rota ou, se não houver rotas correspondentes, um erro *o recurso não pôde ser encontrado* será retornado.

> [!div class="step-by-step"]
> [Anterior](creating-custom-routes-cs.md)
> [Próximo](creating-a-custom-route-constraint-cs.md)
