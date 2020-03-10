---
uid: mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
title: Noções básicas sobre filtrosC#de ação () | Microsoft Docs
author: microsoft
description: O objetivo deste tutorial é explicar os filtros de ação. Um filtro de ação é um atributo que você pode aplicar a uma ação do controlador, ou a um controlador inteiro...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: a94e4e81-40c1-47b7-8613-126a1a6cc93d
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/understanding-action-filters-cs
msc.type: authoredcontent
ms.openlocfilehash: d1c72c2355c6122f851351a8c1e8f04fa63ae04e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78581819"
---
# <a name="understanding-action-filters-c"></a>Noções básicas sobre filtros de ação (C#)

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_14_CS.pdf)

> O objetivo deste tutorial é explicar os filtros de ação. Um filtro de ação é um atributo que você pode aplicar a uma ação de controlador, ou a um controlador inteiro, que modifica a maneira como a ação é executada.

## <a name="understanding-action-filters"></a>Noções básicas sobre filtros de ação

O objetivo deste tutorial é explicar os filtros de ação. Um filtro de ação é um atributo que você pode aplicar a uma ação de controlador, ou a um controlador inteiro, que modifica a maneira como a ação é executada. A estrutura MVC do ASP.NET inclui vários filtros de ação:

- OutputCache – esse filtro de ação armazena em cache a saída de uma ação do controlador por um período de tempo especificado.
- HandleError – esse filtro de ação manipula erros gerados quando uma ação do controlador é executada.
- Autorizar – este filtro de ação permite restringir o acesso a um determinado usuário ou função.

Você também pode criar seus próprios filtros de ação personalizados. Por exemplo, talvez você queira criar um filtro de ação personalizado para implementar um sistema de autenticação personalizado. Ou talvez você queira criar um filtro de ação que modifique os dados de exibição retornados por uma ação do controlador.

Neste tutorial, você aprenderá a criar um filtro de ação desde o início. Criamos um filtro de ação de log que registra diferentes estágios do processamento de uma ação na janela de saída do Visual Studio.

### <a name="using-an-action-filter"></a>Usando um filtro de ação

Um filtro de ação é um atributo. Você pode aplicar a maioria dos filtros de ação a uma ação de controlador individual ou a um controlador inteiro.

Por exemplo, o controlador de dados na Listagem 1 expõe uma ação chamada `Index()` que retorna a hora atual. Essa ação é decorada com o filtro de ação `OutputCache`. Esse filtro faz com que o valor retornado pela ação seja armazenado em cache por 10 segundos.

**Listagem 1 – `Controllers\DataController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample1.cs)]

Se você invocar repetidamente a ação `Index()` inserindo a URL/Data/Index na barra de endereços do navegador e pressionando o botão atualizar várias vezes, você verá o mesmo tempo por 10 segundos. A saída da ação de `Index()` é armazenada em cache por 10 segundos (consulte a Figura 1).

[tempo em cache ![](understanding-action-filters-cs/_static/image2.png)](understanding-action-filters-cs/_static/image1.png)

**Figura 01**: tempo em cache ([clique para exibir a imagem em tamanho normal](understanding-action-filters-cs/_static/image3.png))

Na Listagem 1, um único filtro de ação – o `OutputCache` filtro de ação – é aplicado ao método `Index()`. Se precisar, você poderá aplicar vários filtros de ação à mesma ação. Por exemplo, talvez você queira aplicar os filtros de ação `OutputCache` e `HandleError` à mesma ação.

Na Listagem 1, o filtro de ação `OutputCache` é aplicado à ação `Index()`. Você também pode aplicar esse atributo à própria classe `DataController`. Nesse caso, o resultado retornado por qualquer ação exposta pelo controlador seria armazenado em cache por 10 segundos.

### <a name="the-different-types-of-filters"></a>Os diferentes tipos de filtros

O ASP.NET MVC Framework dá suporte a quatro tipos diferentes de filtros:

1. Filtros de autorização – implementa o atributo `IAuthorizationFilter`.
2. Filtros de ação – implementa o atributo `IActionFilter`.
3. Filtros de resultado – implementa o atributo `IResultFilter`.
4. Filtros de exceção – implementa o atributo `IExceptionFilter`.

Os filtros são executados na ordem listada acima. Por exemplo, os filtros de autorização são sempre executados antes que os filtros de ação e filtros de exceção sempre sejam executados depois de todos os outros tipos de filtro.

Os filtros de autorização são usados para implementar a autenticação e autorização para ações do controlador. Por exemplo, o filtro autorizar é um exemplo de um filtro de autorização.

Os filtros de ação contêm a lógica que é executada antes e depois que uma ação do controlador é executada. Você pode usar um filtro de ação, por exemplo, para modificar os dados de exibição que uma ação do controlador retorna.

Os filtros de resultado contêm a lógica que é executada antes e depois que um resultado de exibição é executado. Por exemplo, talvez você queira modificar um resultado de exibição logo antes que a exibição seja processada para o navegador.

Filtros de exceção são o último tipo de filtro a ser executado. Você pode usar um filtro de exceção para tratar erros gerados por ações do controlador ou pelos resultados da ação do controlador. Você também pode usar filtros de exceção para registrar erros.

Cada tipo diferente de filtro é executado em uma ordem específica. Se você quiser controlar a ordem na qual os filtros do mesmo tipo são executados, poderá definir a propriedade Order de um filtro.

A classe base para todos os filtros de ação é a classe `System.Web.Mvc.FilterAttribute`. Se você quiser implementar um tipo específico de filtro, precisará criar uma classe que herda da classe de filtro base e implementar uma ou mais das interfaces `IAuthorizationFilter`, `IActionFilter`, `IResultFilter`ou `IExceptionFilter`.

### <a name="the-base-actionfilterattribute-class"></a>A classe base ActionFilterAttribute

Para facilitar a implementação de um filtro de ação personalizado, a estrutura MVC do ASP.NET inclui uma classe base `ActionFilterAttribute`. Essa classe implementa as interfaces `IActionFilter` e `IResultFilter` e herda da classe `Filter`.

A terminologia aqui não é totalmente consistente. Tecnicamente, uma classe que herda da classe ActionFilterAttribute é um filtro de ação e um filtro de resultado. No entanto, no sentido flexível, o filtro de ação do Word é usado para fazer referência a qualquer tipo de filtro na estrutura MVC do ASP.NET.

A classe base `ActionFilterAttribute` tem os seguintes métodos que você pode substituir:

- OnActionExecuting – esse método é chamado antes da execução de uma ação do controlador.
- OnActionExecuted – esse método é chamado depois que uma ação do controlador é executada.
- OnResultExecuting – esse método é chamado antes da execução de um resultado de ação do controlador.
- OnResultExecuted – esse método é chamado depois que um resultado de ação do controlador é executado.

Na próxima seção, veremos como você pode implementar cada um desses métodos diferentes.

### <a name="creating-a-log-action-filter"></a>Criando um filtro de ação de log

Para ilustrar como você pode criar um filtro de ação personalizado, criaremos um filtro de ação personalizado que registra em log os estágios de processamento de uma ação do controlador para a janela de saída do Visual Studio. Nossa `LogActionFilter` está contida na Listagem 2.

**Listagem 2 – `ActionFilters\LogActionFilter.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample2.cs)]

Na Listagem 2, os métodos `OnActionExecuting()`, `OnActionExecuted()`, `OnResultExecuting()`e `OnResultExecuted()` chamam o método `Log()`. O nome do método e os dados de rota atuais são passados para o método de `Log()`. O método `Log()` grava uma mensagem na janela de saída do Visual Studio (consulte a Figura 2).

[![gravar na janela de saída do Visual Studio](understanding-action-filters-cs/_static/image5.png)](understanding-action-filters-cs/_static/image4.png)

**Figura 02**: gravando na janela de saída do Visual Studio ([clique para exibir a imagem em tamanho normal](understanding-action-filters-cs/_static/image6.png))

O controlador Home na Listagem 3 ilustra como você pode aplicar o filtro de ação de log a uma classe de controlador inteira. Sempre que qualquer uma das ações expostas pelo controlador Home é invocada – o método `Index()` ou o método `About()` – os estágios de processamento da ação são registrados na janela de saída do Visual Studio.

**Listagem 3 – `Controllers\HomeController.cs`**

[!code-csharp[Main](understanding-action-filters-cs/samples/sample3.cs)]

### <a name="summary"></a>Resumo

Neste tutorial, você foi apresentado aos filtros de ação do ASP.NET MVC. Você aprendeu sobre os quatro tipos diferentes de filtros: filtros de autorização, filtros de ação, filtros de resultado e filtros de exceção. Você também aprendeu sobre a classe de `ActionFilterAttribute` base.

Por fim, você aprendeu a implementar um filtro de ação simples. Criamos um filtro de ação de log que registra os estágios de processamento de uma ação de controlador na janela de saída do Visual Studio.

> [!div class="step-by-step"]
> [Anterior](asp-net-mvc-routing-overview-cs.md)
> [Próximo](improving-performance-with-output-caching-cs.md)
