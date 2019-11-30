---
uid: mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
title: Criando testes de unidade para aplicativos ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: Saiba como criar testes de unidade para ações do controlador. Neste tutorial, Stephen Walther demonstra como testar se uma ação do controlador retorna uma partir de...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: eb35710d-1d99-44ac-b61f-e50af8cb328a
msc.legacyurl: /mvc/overview/older-versions-1/unit-testing/creating-unit-tests-for-asp-net-mvc-applications-vb
msc.type: authoredcontent
ms.openlocfilehash: 643faddaf6f9cd075131e8e5a9cccb303e355ceb
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594687"
---
# <a name="creating-unit-tests-for-aspnet-mvc-applications-vb"></a>Criação de testes de unidade para aplicativos do ASP.NET MVC (VB)

por [Stephen Walther](https://github.com/StephenWalther)

[Baixar PDF](https://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_07_VB.pdf)

> Saiba como criar testes de unidade para ações do controlador. Neste tutorial, Stephen Walther demonstra como testar se uma ação do controlador retorna uma determinada exibição, retorna um determinado conjunto de dados ou retorna um tipo diferente de resultado de ação.

O objetivo deste tutorial é demonstrar como você pode escrever testes de unidade para os controladores em seus aplicativos MVC do ASP.NET. Discutiremos como criar três tipos diferentes de testes de unidade. Você aprende a testar o modo de exibição retornado por uma ação do controlador, como testar os dados de exibição retornados por uma ação do controlador e como testar se uma ação do controlador redireciona ou não para uma segunda ação do controlador.

## <a name="creating-the-controller-under-test"></a>Criando o controlador em teste

Vamos começar criando o controlador que pretendemos testar. O controlador, chamado de `ProductController`, está contido na Listagem 1.

**Listagem 1 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample1.vb)]

O `ProductController` contém dois métodos de ação chamados `Index()` e `Details()`. Ambos os métodos de ação retornam uma exibição. Observe que a ação `Details()` aceita um parâmetro chamado ID.

## <a name="testing-the-view-returned-by-a-controller"></a>Testando a exibição retornada por um controlador

Imagine que desejamos testar se o `ProductController` retorna ou não o modo de exibição correto. Queremos garantir que quando a ação de `ProductController.Details()` for invocada, a exibição de detalhes será retornada. A classe de teste na Listagem 2 contém um teste de unidade para testar a exibição retornada pela ação de `ProductController.Details()`.

**Listagem 2 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample2.vb)]

A classe na Listagem 2 inclui um método de teste chamado `TestDetailsView()`. Esse método contém três linhas de código. A primeira linha de código cria uma nova instância da classe `ProductController`. A segunda linha de código invoca o método de ação de `Details()` do controlador. Por fim, a última linha de código verifica se a exibição retornada pela ação de `Details()` é a exibição de detalhes.

A propriedade `ViewResult.ViewName` representa o nome da exibição retornada por um controlador. Um grande aviso sobre como testar essa propriedade. Há duas maneiras de um controlador poder retornar uma exibição. Um controlador pode retornar explicitamente uma exibição como esta:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample3.vb)]

Como alternativa, o nome da exibição pode ser inferido a partir do nome da ação do controlador como esta:

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample4.vb)]

Essa ação do controlador também retorna uma exibição chamada `Details`. No entanto, o nome da exibição é inferido do nome da ação. Se desejar testar o nome da exibição, você deverá retornar explicitamente o nome da exibição da ação do controlador.

Você pode executar o teste de unidade na Listagem 2 inserindo a combinação de teclado **Ctrl-R, a** ou clicando no botão **executar todos os testes na solução** (veja a Figura 1). Se o teste passar, você verá a janela Resultados de Teste na Figura 2.

[![executar todos os testes na solução](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image2.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image1.png)

**Figura 01**: executar todos os testes na solução ([clique para exibir a imagem em tamanho normal](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image3.png))

[![êxito!](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image5.png)](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image4.png)

**Figura 02**: sucesso! ([Clique para exibir a imagem em tamanho normal](creating-unit-tests-for-asp-net-mvc-applications-vb/_static/image6.png))

## <a name="testing-the-view-data-returned-by-a-controller"></a>Testando os dados de exibição retornados por um controlador

Um controlador MVC passa dados para uma exibição usando algo chamado *`View Data`* . Por exemplo, imagine que você deseja exibir os detalhes de um produto específico ao invocar a ação de `ProductController Details()`. Nesse caso, você pode criar uma instância de uma classe de `Product` (definida em seu modelo) e passar a instância para a exibição de `Details` aproveitando o `View Data`.

A `ProductController` modificada na Listagem 3 inclui uma ação de `Details()` atualizada que retorna um produto.

**Listagem 3 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample5.vb)]

Primeiro, a ação `Details()` cria uma nova instância da classe `Product` que representa um computador laptop. Em seguida, a instância da classe `Product` é passada como o segundo parâmetro para o método `View()`.

Você pode escrever testes de unidade para testar se os dados esperados estão contidos na exibição de dados. O teste de unidade na Listagem 4 testa se um produto que representa um computador laptop é retornado quando você chama o método de ação `ProductController Details()`.

**Listagem 4 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample6.vb)]

Na Listagem 4, o método `TestDetailsView()` testa os dados de exibição retornados invocando o método `Details()`. O `ViewData` é exposto como uma propriedade no `ViewResult` retornado pela invocação do método `Details()`. A propriedade `ViewData.Model` contém o produto passado para a exibição. O teste simplesmente verifica se o produto contido na exibição de dados tem o nome laptop.

## <a name="testing-the-action-result-returned-by-a-controller"></a>Testando o resultado da ação retornado por um controlador

Uma ação de controlador mais complexa pode retornar tipos diferentes de resultados de ação, dependendo dos valores dos parâmetros passados para a ação do controlador. Uma ação do controlador pode retornar uma variedade de tipos de resultados de ação, incluindo um `ViewResult`, `RedirectToRouteResult`ou `JsonResult`.

Por exemplo, a ação `Details()` modificada na listagem 5 retorna a exibição `Details` quando você passa uma ID de produto válida para a ação. Se você passar uma ID de produto inválida – uma ID com um valor menor que 1--você será redirecionado para a ação de `Index()`.

**Listagem 5 – `ProductController.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample7.vb)]

Você pode testar o comportamento da ação de `Details()` com o teste de unidade na Listagem 6. O teste de unidade na Listagem 6 verifica se você é redirecionado para a exibição `Index` quando uma ID com o valor-1 é passada para o método `Details()`.

**Listagem 6 – `ProductControllerTest.vb`**

[!code-vb[Main](creating-unit-tests-for-asp-net-mvc-applications-vb/samples/sample8.vb)]

Quando você chama o método `RedirectToAction()` em uma ação do controlador, a ação do controlador retorna um `RedirectToRouteResult`. O teste verifica se o `RedirectToRouteResult` redirecionará o usuário para uma ação de controlador chamada `Index`.

## <a name="summary"></a>Resumo

Neste tutorial, você aprendeu a criar testes de unidade para ações do controlador MVC. Primeiro, você aprendeu como verificar se a exibição correta é retornada por uma ação do controlador. Você aprendeu a usar a propriedade `ViewResult.ViewName` para verificar o nome de uma exibição.

Em seguida, examinamos como você pode testar o conteúdo de `View Data`. Você aprendeu como verificar se o produto correto foi retornado em `View Data` depois de chamar uma ação do controlador.

Por fim, discutimos como você pode testar se diferentes tipos de resultados de ação são retornados de uma ação do controlador. Você aprendeu como testar se um controlador retorna um `ViewResult` ou um `RedirectToRouteResult`.

> [!div class="step-by-step"]
> [Anterior](creating-unit-tests-for-asp-net-mvc-applications-cs.md)
