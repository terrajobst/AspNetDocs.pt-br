---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Criando um controlador (VB) | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther demonstra como você pode adicionar um controlador a um aplicativo MVC ASP.NET.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: 60636b79ab5fc06ca904dee90ce74f256e046d12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601454"
---
# <a name="creating-a-controller-vb"></a>Criar um controlador (VB)

por [Stephen Walther](https://github.com/StephenWalther)

> Neste tutorial, Stephen Walther demonstra como você pode adicionar um controlador a um aplicativo MVC ASP.NET.

O objetivo deste tutorial é explicar como você pode criar novos controladores MVC do ASP.NET. Você aprende a criar controladores usando a opção de menu Adicionar controlador do Visual Studio e criando um arquivo de classe manualmente.

### <a name="using-the-add-controller-menu-option"></a>Usando a opção de menu Adicionar controlador

A maneira mais fácil de criar um novo controlador é clicar com o botão direito do mouse na pasta controladores na janela de Gerenciador de Soluções do Visual Studio e selecionar a opção de menu **Adicionar, controlador** (consulte a Figura 1). A seleção dessa opção de menu abre a caixa de diálogo **Adicionar controlador** (consulte a Figura 2).

[![caixa de diálogo novo projeto](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)

**Figura 01**: adicionando um novo controlador ([clique para exibir a imagem em tamanho normal](creating-a-controller-vb/_static/image2.png))

[![caixa de diálogo novo projeto](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)

**Figura 02**: a caixa de diálogo Adicionar controlador ([clique para exibir a imagem em tamanho normal](creating-a-controller-vb/_static/image4.png))

Observe que a primeira parte do nome do controlador está realçada na caixa de diálogo **Adicionar controlador** . Cada nome de controlador deve terminar com o *controlador*de sufixo. Por exemplo, você pode criar um controlador chamado *ProductController* , mas não um controlador chamado *Product*.

Se você criar um controlador que não tem o sufixo do *controlador* , não será possível invocar o controlador. Não faça isso, desperdiçamos incontáveis horas da minha vida depois de fazer esse erro.

**Listagem 1-Controllers\ProductController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

Você sempre deve criar controladores na pasta controladores. Caso contrário, você violará as convenções do ASP.NET MVC e outros desenvolvedores terão um tempo mais difícil de entender seu aplicativo.

### <a name="scaffolding-action-methods"></a>Métodos de ação scaffolding

Ao criar um controlador, você tem a opção de gerar automaticamente os métodos de ação criar, atualizar e detalhes (veja a Figura 3). Se você selecionar essa opção, a classe de controlador na Listagem 2 será gerada.

[![criar métodos de ação automaticamente](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)

**Figura 03**: criando métodos de ação automaticamente ([clique para exibir a imagem em tamanho normal](creating-a-controller-vb/_static/image6.png))

**Listagem 2-Controllers\CustomerController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

Esses métodos gerados são métodos stub. Você deve adicionar a lógica real para criar, atualizar e mostrar os detalhes de um cliente por conta própria. Mas, os métodos de stub fornecem um bom ponto de partida.

### <a name="creating-a-controller-class"></a>Criando uma classe de controlador

O controlador MVC ASP.NET é apenas uma classe. Se preferir, você pode ignorar o scaffolding conveniente do Visual Studio Controller e criar uma classe de controlador manualmente. Siga estas etapas:

1. Clique com o botão direito do mouse na pasta controladores e selecione a opção de menu **Adicionar, novo item** e selecione o modelo de **classe** (consulte a Figura 4).
2. Nomeie a nova classe PersonController. vb e clique no botão **Adicionar** .
3. Modifique o arquivo de classe resultante para que a classe herde da classe base System. Web. Mvc. Controller (consulte a listagem 3).

[![criar uma nova classe](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)

**Figura 04**: criando uma nova classe ([clique para exibir a imagem em tamanho normal](creating-a-controller-vb/_static/image8.png))

**Listagem 3-Controllers\PersonController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

O controlador na Listagem 3 expõe uma ação chamada index () que retorna a cadeia de caracteres "Olá, Mundo!". Você pode invocar essa ação do controlador executando seu aplicativo e solicitando uma URL como a seguinte:

`http://localhost:40071/Person`

> [!NOTE]
> 
> O ASP.NET Development Server usa um número de porta aleatório (por exemplo, 40071). Ao inserir uma URL para invocar um controlador, você precisará fornecer o número de porta correto. Você pode determinar o número da porta passando o mouse sobre o ícone do ASP.NET Development Server na área de notificação do Windows (inferior direita da tela).
> 
> [!div class="step-by-step"]
> [Anterior](adding-dynamic-content-to-a-cached-page-vb.md)
> [Próximo](creating-an-action-vb.md)
