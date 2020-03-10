---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: Visão geral de roteamento do ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther mostra como o ASP.NET MVC Framework mapeia solicitações de navegador para ações do controlador.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: ed043d76b89ce31945cf3423b0c5afca9383cc21
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78601531"
---
# <a name="aspnet-mvc-routing-overview-vb"></a>Visão geral sobre o roteamento do ASP.NET MVC (VB)

por [Stephen Walther](https://github.com/StephenWalther)

> Neste tutorial, Stephen Walther mostra como o ASP.NET MVC Framework mapeia solicitações de navegador para ações do controlador.

Neste tutorial, você será apresentado a um recurso importante de cada aplicativo MVC ASP.NET chamado *roteamento ASP.net*. O módulo de roteamento ASP.NET é responsável por mapear solicitações de navegador de entrada para ações específicas do controlador MVC. Ao final deste tutorial, você entenderá como a tabela de rotas padrão mapeia solicitações para ações do controlador.

## <a name="using-the-default-route-table"></a>Usando a tabela de rotas padrão

Quando você cria um novo aplicativo MVC do ASP.NET, o aplicativo já está configurado para usar o roteamento do ASP.NET. O roteamento do ASP.NET é configurado em dois locais.

Primeiro, o roteamento ASP.NET está habilitado no arquivo de configuração da Web do seu aplicativo (arquivo Web. config). Há quatro seções no arquivo de configuração que são relevantes para o roteamento: a seção System. Web. httpModules, a seção System. Web. httpHandlers, a seção System. WebServer. modules e a seção System. WebServer. Handlers. Tenha cuidado para não excluir essas seções porque, sem essas seções, o roteamento não funcionará mais.

Segundo, e o mais importante, uma tabela de rotas é criada no arquivo global. asax do aplicativo. O arquivo global. asax é um arquivo especial que contém manipuladores de eventos para eventos de ciclo de vida do aplicativo ASP.NET. A tabela de rotas é criada durante o evento de início do aplicativo.

O arquivo na Listagem 1 contém o arquivo global. asax padrão para um aplicativo MVC ASP.NET.

**Listagem 1-global. asax. vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

Quando um aplicativo MVC é iniciado pela primeira vez, o aplicativo\_método Start () é chamado. Esse método, por sua vez, chama o método RegisterRoutes (). O método RegisterRoutes () cria a tabela de rotas.

A tabela de rotas padrão contém uma única rota (denominada padrão). A rota padrão mapeia o primeiro segmento de uma URL para um nome de controlador, o segundo segmento de uma URL para uma ação de controlador e o terceiro segmento para um parâmetro chamado **ID**.

Imagine que você insira a seguinte URL na barra de endereços do navegador da Web:

/Home/Index/3

A rota padrão mapeia essa URL para os seguintes parâmetros:

- controlador = página inicial

- ação = índice

- id = 3

Quando você solicita a URL/Home/Index/3, o código a seguir é executado:

HomeController.Index(3)

A rota padrão inclui padrões para todos os três parâmetros. Se você não fornecer um controlador, o parâmetro do controlador padrão será o valor **página inicial**. Se você não fornecer uma ação, o parâmetro de ação assume como padrão o **índice**de valor. Por fim, se você não fornecer uma ID, o parâmetro de ID assume como padrão uma cadeia de caracteres vazia.

Vejamos alguns exemplos de como a rota padrão mapeia URLs para ações do controlador. Imagine que você insira a seguinte URL na barra de endereços do navegador:

/Home

Devido aos padrões de parâmetro de rota padrão, a inserção dessa URL fará com que o método index () da classe HomeController na Listagem 2 seja chamado.

**Listagem 2-HomeController. vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

Na Listagem 2, a classe HomeController inclui um método chamado index () que aceita um único parâmetro chamado ID. A URL/Home faz com que o método index () seja chamado com o valor Nothing como o valor do parâmetro ID.

Devido à forma como o MVC Framework invoca ações do controlador, a URL/Home também corresponde ao método index () da classe HomeController na Listagem 3.

**Listagem 3-HomeController. vb (ação de índice sem parâmetro)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

O método index () na Listagem 3 não aceita nenhum parâmetro. A URL/Home fará com que esse método de índice () seja chamado. A URL/Home/Index/3 também invoca esse método (a ID é ignorada).

A URL/Home também corresponde ao método index () da classe HomeController na Listagem 4.

**Listagem 4-HomeController. vb (ação de índice com parâmetro anulável)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

Na Listagem 4, o método index () tem um parâmetro Integer. Como o parâmetro é um parâmetro anulável (pode ter o valor Nothing), o índice () pode ser chamado sem gerar um erro.

Finalmente, invocar o método index () na listagem 5 com a URL/Home causa uma exceção, uma vez que o parâmetro ID *não é* um parâmetro anulável. Se você tentar invocar o método index (), você obterá o erro exibido na Figura 1.

**Listagem 5-HomeController. vb (ação de índice com parâmetro de ID)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]

[![invocando uma ação do controlador que espera um valor de parâmetro](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)

**Figura 01**: invocando uma ação do controlador que espera um valor de parâmetro ([clique para exibir a imagem em tamanho normal](asp-net-mvc-routing-overview-vb/_static/image2.png))

A URL/Home/Index/3, por outro lado, funciona bem com a ação de controlador de índice na listagem 5. A solicitação/Home/Index/3 faz com que o método index () seja chamado com um parâmetro de ID que tenha o valor 3.

## <a name="summary"></a>Resumo

O objetivo deste tutorial era fornecer uma breve introdução ao roteamento ASP.NET. Examinamos a tabela de rotas padrão que você obtém com um novo aplicativo MVC ASP.NET. Você aprendeu como a rota padrão mapeia URLs para ações do controlador.

> [!div class="step-by-step"]
> [Anterior](creating-an-action-cs.md)
> [Próximo](understanding-action-filters-vb.md)
