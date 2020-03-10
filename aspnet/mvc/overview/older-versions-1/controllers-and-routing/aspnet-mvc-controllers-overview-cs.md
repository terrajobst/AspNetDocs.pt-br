---
uid: mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
title: Visão geral do controlador MVCC#ASP.net () | Microsoft Docs
author: StephenWalther
description: Neste tutorial, Stephen Walther apresenta os controladores MVC ASP.NET. Você aprende como criar novos controladores e retornar tipos diferentes de ação res...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: b985c49a-3668-455c-a366-f85f6bc64b12
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/aspnet-mvc-controllers-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a287b37742400a17c2ed53cfd00bfb053b4f3d2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544110"
---
# <a name="aspnet-mvc-controller-overview-c"></a>Visão geral sobre o controlador do ASP.NET MVC (C#)

por [Stephen Walther](https://github.com/StephenWalther)

> Neste tutorial, Stephen Walther apresenta os controladores MVC ASP.NET. Você aprende como criar novos controladores e retornar tipos diferentes de resultados de ação.

Este tutorial explora o tópico de controladores MVC ASP.NET, ações do controlador e resultados da ação. Depois de concluir este tutorial, você entenderá como os controladores são usados para controlar a maneira como um visitante interage com um site do ASP.NET MVC.

## <a name="understanding-controllers"></a>Noções básicas sobre controladores

Os controladores MVC são responsáveis por responder às solicitações feitas em um site do ASP.NET MVC. Cada solicitação do navegador é mapeada para um controlador específico. Por exemplo, imagine que você insira a seguinte URL na barra de endereços do seu navegador:

`http://localhost/Product/Index/3`

Nesse caso, um controlador chamado ProductController é invocado. O ProductController é responsável por gerar a resposta para a solicitação do navegador. Por exemplo, o controlador pode retornar uma exibição específica de volta para o navegador ou o controlador pode redirecionar o usuário para outro controlador.

A listagem 1 contém um controlador simples chamado ProductController.

**Listing1 - Controllers\ProductController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample1.cs)]

Como você pode ver na Listagem 1, um controlador é apenas uma classe (um Visual Basic .NET ou C# classe). Um controlador é uma classe que deriva da classe base System. Web. Mvc. Controller. Como um controlador herda dessa classe base, um controlador herda vários métodos úteis gratuitamente (discutiremos esses métodos em breve).

## <a name="understanding-controller-actions"></a>Noções básicas sobre ações do controlador

Um controlador expõe ações do controlador. Uma ação é um método em um controlador que é chamado quando você insere uma URL específica na barra de endereços do navegador. Por exemplo, imagine que você faça uma solicitação para a seguinte URL:

`http://localhost/Product/Index/3`

Nesse caso, o método index () é chamado na classe ProductController. O método index () é um exemplo de uma ação do controlador.

Uma ação do controlador deve ser um método público de uma classe de controlador. C#os métodos, por padrão, são métodos privados. Observe que qualquer método público que você adiciona a uma classe de controlador é exposto como uma ação de controlador automaticamente (você deve ter cuidado com isso, já que uma ação do controlador pode ser invocada por qualquer pessoa no universo simplesmente digitando a URL correta em uma barra de endereços do navegador).

Há alguns requisitos adicionais que devem ser atendidos por uma ação do controlador. Um método usado como uma ação do controlador não pode ser sobrecarregado. Além disso, uma ação do controlador não pode ser um método estático. Além disso, você pode usar praticamente qualquer método como uma ação de controlador.

## <a name="understanding-action-results"></a>Compreendendo os resultados da ação

Uma ação do controlador retorna algo chamado de *resultado da ação*. Um resultado de ação é o que uma ação de controlador retorna em resposta a uma solicitação de navegador.

O ASP.NET MVC Framework dá suporte a vários tipos de resultados de ação, incluindo:

1. ViewResult-representa HTML e marcação.
2. EmptyResult-não representa nenhum resultado.
3. RedirectResult-representa um redirecionamento para uma nova URL.
4. JsonResult-representa um resultado JavaScript Object Notation que pode ser usado em um aplicativo AJAX.
5. JavaScriptResult-representa um script JavaScript.
6. ContentResult-representa um resultado de texto.
7. FileContentResult-representa um arquivo para download (com o conteúdo binário).
8. FilePathResult-representa um arquivo para download (com um caminho).
9. FileStreamResult-representa um arquivo para download (com um fluxo de arquivo).

Todos esses resultados de ação herdam da classe base ActionResult.

Na maioria dos casos, uma ação do controlador retorna um ViewResult. Por exemplo, a ação do controlador de índice na Listagem 2 retorna um ViewResult.

**Listagem 2-Controllers\BookController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample2.cs)]

Quando uma ação retorna um ViewResult, o HTML é retornado para o navegador. O método index () na Listagem 2 retorna uma exibição denominada index para o navegador.

Observe que a ação index () na Listagem 2 não retorna um ViewResult (). Em vez disso, o método View () da classe base Controller é chamado. Normalmente, você não retorna um resultado de ação diretamente. Em vez disso, você chama um dos seguintes métodos da classe base do controlador:

1. View – retorna um resultado de ação ViewResult.
2. Redirect-retorna um resultado de ação RedirectResult.
3. RedirectToAction-retorna um resultado de ação RedirectToRouteResult.
4. RedirectToRoute-retorna um resultado de ação RedirectToRouteResult.
5. JSON – retorna um resultado de ação JsonResult.
6. JavaScriptResult-retorna um JavaScriptResult.
7. Content-retorna um resultado de ação ContentResult.
8. Arquivo-retorna um FileContentResult, FilePathResult ou FileStreamResult dependendo dos parâmetros passados para o método.

Portanto, se você quiser retornar uma exibição para o navegador, chame o método View (). Se você quiser redirecionar o usuário de uma ação do controlador para outra, chame o método RedirectToAction (). Por exemplo, a ação Details () na Listagem 3 exibe uma exibição ou redireciona o usuário para a ação index () dependendo se o parâmetro ID tem um valor.

**Listagem 3-CustomerController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample3.cs)]

O resultado da ação ContentResult é especial. Você pode usar o resultado da ação ContentResult para retornar um resultado de ação como texto sem formatação. Por exemplo, o método index () na Listagem 4 retorna uma mensagem como texto sem formatação e não como HTML.

**Listagem 4-Controllers\StatusController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample4.cs)]

Quando a ação StatusController. Index () é invocada, uma exibição não é retornada. Em vez disso, o texto bruto "Olá, Mundo!" é retornado para o navegador.

Se uma ação do controlador retornar um resultado que não seja um resultado de ação, por exemplo, uma data ou um inteiro, o resultado será encapsulado automaticamente em um ContentResult. Por exemplo, quando a ação index () do WorkController na listagem 5 é invocada, a data é retornada como um ContentResult automaticamente.

**Listagem 5-WorkController.cs**

[!code-csharp[Main](aspnet-mvc-controllers-overview-cs/samples/sample5.cs)]

A ação index () na listagem 5 retorna um objeto DateTime. A estrutura MVC do ASP.NET converte o objeto DateTime em uma cadeia de caracteres e encapsula o valor DateTime em um ContentResult automaticamente. O navegador recebe a data e a hora como texto sem formatação.

## <a name="summary"></a>Resumo

A finalidade deste tutorial foi apresentar a você os conceitos de controladores MVC ASP.NET, ações do controlador e resultados da ação do controlador. Na primeira seção, você aprendeu a adicionar novos controladores a um projeto MVC do ASP.NET. Em seguida, você aprendeu como os métodos públicos de um controlador são expostos ao universo como ações do controlador. Por fim, discutimos os diferentes tipos de resultados de ação que podem ser retornados de uma ação de controlador. Em particular, discutimos como retornar um ViewResult, RedirectToActionResult e ContentResult de uma ação do controlador.

> [!div class="step-by-step"]
> [Anterior](creating-an-action-vb.md)
> [Próximo](creating-custom-routes-cs.md)
