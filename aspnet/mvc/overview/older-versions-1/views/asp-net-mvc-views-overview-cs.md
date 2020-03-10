---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: Visão geral das exibiçõesC#do ASP.NET MVC () | Microsoft Docs
author: StephenWalther
description: O que é uma exibição do ASP.NET MVC e como ela difere de uma página HTML? Neste tutorial, Stephen Walther apresenta exibições e demonstra como você não pode...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: b3f44aa9654a2a718381eaf9c856ca3e15ed1e27
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600313"
---
# <a name="aspnet-mvc-views-overview-c"></a>Visão geral de exibições do ASP.NET MVC (C#)

por [Stephen Walther](https://github.com/StephenWalther)

> O que é uma exibição do ASP.NET MVC e como ela difere de uma página HTML? Neste tutorial, Stephen Walther apresenta exibições e demonstra como você pode tirar proveito da exibição de dados e auxiliares HTML em uma exibição.

A finalidade deste tutorial é fornecer uma breve introdução às exibições do ASP.NET MVC, exibir dados e auxiliares HTML. Ao final deste tutorial, você deve entender como criar novos modos de exibição, passar dados de um controlador para uma visualização e usar auxiliares HTML para gerar conteúdo em uma exibição.

## <a name="understanding-views"></a>Compreendendo as exibições

Para as páginas ASP.NET ou Active Server, o ASP.NET MVC não inclui nada que corresponda diretamente a uma página. Em um aplicativo MVC ASP.NET, não há uma página em disco que corresponda ao caminho na URL que você digitar na barra de endereços do seu navegador. A coisa mais próxima de uma página em um aplicativo MVC ASP.NET é algo chamado de *exibição*.

Em um aplicativo MVC do ASP.NET, as solicitações de entrada do navegador são mapeadas para ações do controlador. Uma ação do controlador pode retornar uma exibição. No entanto, uma ação do controlador pode executar algum outro tipo de ação, como redirecionar você para outra ação do controlador.

A listagem 1 contém um controlador simples chamado HomeController. O HomeController expõe duas ações de controlador denominadas index () e Details ().

**Listagem 1-HomeController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

Você pode invocar a primeira ação, a ação de índice (), digitando a seguinte URL na barra de endereços do navegador:

/Home/Index

Você pode invocar a segunda ação, a ação detalhes (), digitando esse endereço em seu navegador:

/Home/Details

A ação index () retorna uma exibição. A maioria das ações que você criar retornará exibições. No entanto, uma ação pode retornar outros tipos de resultados de ação. Por exemplo, a ação Details () retorna um RedirectToActionResult que redireciona a solicitação de entrada para a ação index ().

A ação index () contém a seguinte linha única de código:

Exibir ();

Essa linha de código retorna uma exibição que deve estar localizada no seguinte caminho no seu servidor Web:

\Views\Home\Index.aspx

O caminho para a exibição é inferido a partir do nome do controlador e do nome da ação do controlador.

Se preferir, você pode ser explícito sobre a exibição. A linha de código a seguir retorna uma exibição chamada Fred:

Exibir (Fred);

Quando essa linha de código é executada, uma exibição é retornada do seguinte caminho:

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> Se você planeja criar testes de unidade para seu aplicativo ASP.NET MVC, é uma boa ideia ser explícita sobre nomes de exibição. Dessa forma, você pode criar um teste de unidade para verificar se a exibição esperada foi retornada por uma ação do controlador.

## <a name="adding-content-to-a-view"></a>Adicionando conteúdo a uma exibição

Uma exibição é um documento HTML padrão (X) que pode conter scripts. Você usa scripts para adicionar conteúdo dinâmico a uma exibição.

Por exemplo, a exibição na Listagem 2 exibe a data e a hora atuais.

**Listagem 2-\Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

Observe que o corpo da página HTML na Listagem 2 contém o script a seguir:

&lt;% Response. Write (DateTime. Now);%&gt;

Você usa os delimitadores de script &lt;% e%&gt; para marcar o início e o fim de um script. Esse script é escrito em C#. Ele exibe a data e a hora atuais chamando o método Response. Write () para renderizar o conteúdo para o navegador. Os delimitadores de script &lt;% e%&gt; podem ser usados para executar uma ou mais instruções.

Como você chama Response. Write () com frequência, a Microsoft fornece um atalho para chamar o método Response. Write (). A exibição na Listagem 3 usa os delimitadores &lt;% = e%&gt; como um atalho para chamar Response. Write ().

**Listagem 3-Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

Você pode usar qualquer linguagem .NET para gerar conteúdo dinâmico em uma exibição. Normalmente, você usará o Visual Basic .NET ou C# escreverá seus controladores e exibições.

## <a name="using-html-helpers-to-generate-view-content"></a>Usando auxiliares HTML para gerar conteúdo de exibição

Para facilitar a adição de conteúdo a uma exibição, você pode aproveitar algo chamado de *auxiliar HTML*. Um auxiliar HTML, normalmente, é um método que gera uma cadeia de caracteres. Você pode usar os auxiliares HTML para gerar elementos HTML padrão, como caixas de Text, links, listas suspensas e caixas de listagem.

Por exemplo, a exibição na Listagem 4 aproveita os três auxiliares HTML--o BeginForm (), a caixa de texto () e a senha () auxiliares – para gerar um formulário de logon (consulte a Figura 1).

**Listagem 4--\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]

[![caixa de diálogo novo projeto](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)

**Figura 01**: um formulário de logon padrão ([clique para exibir a imagem em tamanho normal](asp-net-mvc-views-overview-cs/_static/image2.png))

Todos os métodos de auxiliares HTML são chamados na propriedade HTML da exibição. Por exemplo, você renderiza uma caixa de texto chamando o método html. TextBox ().

Observe que você usa os delimitadores de script &lt;% = e%&gt; ao chamar os auxiliares HTML. TextBox () e HTML. Password (). Esses auxiliares simplesmente retornam uma cadeia de caracteres. Você precisa chamar Response. Write () para renderizar a cadeia de caracteres para o navegador.

O uso de métodos auxiliares HTML é opcional. Eles facilitam sua vida reduzindo a quantidade de HTML e script que você precisa escrever. A exibição na listagem 5 renderiza exatamente a mesma forma que a exibição na Listagem 4 sem usar auxiliares HTML.

**Listagem 5--\Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

Você também tem a opção de criar seus próprios auxiliares HTML. Por exemplo, você pode criar um método auxiliar GridView () que exibe um conjunto de registros de banco de dados em uma tabela HTML automaticamente. Exploramos este tópico no tutorial **criando auxiliares HTML personalizados**.

## <a name="using-view-data-to-pass-data-to-a-view"></a>Usando dados de exibição para passar dados para uma exibição

Você usa exibir dados para passar dados de um controlador para um modo de exibição. Considere exibir dados como um pacote enviado por email. Todos os dados passados de um controlador para uma exibição devem ser enviados usando este pacote. Por exemplo, o controlador na Listagem 6 adiciona uma mensagem para exibir dados.

**Listagem 6-ProductController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

A propriedade ViewData do controlador representa uma coleção de pares de nome e valor. Na Listagem 6, o método index () adiciona um item à exibição da coleção de dados denominada Message com o valor Olá, Mundo!. Quando o modo de exibição é retornado pelo método index (), os dados de exibição são passados para a exibição automaticamente.

A exibição na Listagem 7 recupera a mensagem do modo de exibição dados e renderiza a mensagem para o navegador.

**Listagem 7--\Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

Observe que a exibição aproveita o método auxiliar HTML HTML. Encode () ao renderizar a mensagem. O auxiliar HTML HTML. Encode () codifica caracteres especiais, como &lt; e &gt; em caracteres que são seguros para serem exibidos em uma página da Web. Sempre que você renderiza o conteúdo que um usuário envia para um site, você deve codificar o conteúdo para evitar ataques de injeção de JavaScript.

(Como criamos a mensagem por si só no ProductController, não precisamos realmente codificar a mensagem. No entanto, é um bom hábito sempre chamar o método html. Encode () ao exibir o conteúdo recuperado dos dados de exibição em uma exibição.)

Na Listagem 7, tiramos proveito da exibição de dados para passar uma mensagem simples de cadeia de caracteres de um controlador para um modo de exibição. Você também pode usar o modo de exibição de dados para passar outros tipos de dados, como uma coleção de registros de banco de dado, de um controlador para um modo de exibição. Por exemplo, se você quiser exibir o conteúdo da tabela de banco de dados Products em uma exibição, você passaria a coleção de registros de banco de dados em View Data.

Você também tem a opção de passar dados de exibição com rigidez de tipos de um controlador para um modo de exibição. Exploraremos este tópico no tutorial **entendendo dados e exibições de exibição com rigidez de tipos**.

## <a name="summary"></a>Resumo

Este tutorial forneceu uma breve introdução às exibições do ASP.NET MVC, exibir dados e auxiliares HTML. Na primeira seção, você aprendeu a adicionar novas exibições ao seu projeto. Você aprendeu que deve adicionar uma exibição à pasta direita para chamá-la de um controlador específico. Em seguida, discutimos o tópico de auxiliares HTML. Você aprendeu como os auxiliares HTML permitem que você gere facilmente conteúdo HTML padrão. Por fim, você aprendeu a aproveitar os dados de exibição para passar dados de um controlador para um modo de exibição.

> [!div class="step-by-step"]
> [Próximo](creating-custom-html-helpers-cs.md)
