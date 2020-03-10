---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
title: Adicionando um método Create e Create View | Microsoft Docs
author: shanselman
description: Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Crie um aplicativo Web simples que lê e grava de um banco de dados.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: a3a90963-0286-4fa0-9b3d-c230cc18b0a3
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part6
msc.type: authoredcontent
ms.openlocfilehash: 05a281720f76b107fe8d902ef60d5d2e72af3ef5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543655"
---
# <a name="adding-a-create-method-and-create-view"></a>Adicionar um Criar Método e um Criar Exibição

por [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Você criará um aplicativo Web simples que lê e grava de um banco de dados. Visite o [ASP.NET MVC Learning Center](../../../index.md) para encontrar outros tutoriais e exemplos do ASP.NET MVC.

Nesta seção, vamos implementar o suporte necessário para permitir que os usuários criem novos filmes em nosso banco de dados. Faremos isso implementando a ação de URL/Movies/Create.

A implementação da URL/Movies/Create é um processo de duas etapas. Quando um usuário visita pela primeira vez a URL/Movies/Create, queremos mostrá-los em um formulário HTML que eles podem preencher para inserir um novo filme. Em seguida, quando o usuário envia o formulário e envia os dados de volta para o servidor, queremos recuperar o conteúdo postado e salvá-lo em nosso banco de dados.

Implementaremos essas duas etapas dentro de dois métodos Create () em nossa classe MoviesController. Um método mostrará o formulário de &lt;&gt; que o usuário deve preencher para criar um novo filme. O segundo método manipulará o processamento dos dados postados quando o usuário enviar o formulário de &lt;&gt; de volta ao servidor e salvar um novo filme dentro de nosso banco de dados.

Abaixo está o código que adicionaremos à nossa classe MoviesController para implementar isso:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample1.cs)]

O código acima contém todo o código que precisaremos dentro de nosso controlador.

Agora, vamos implementar o modelo Create View que usaremos para exibir um formulário para o usuário. Clicaremos com o botão direito do mouse no primeiro método Create e selecionaremos "Add View" para criar o modelo de exibição para o formulário de filme.

Vamos selecionar que vamos passar o modelo de exibição um "filme" como sua classe de dados de exibição e indicar que desejamos "Scaffold" um modelo "Create".

[![adicionar exibição](getting-started-with-mvc-part6/_static/image2.png)](getting-started-with-mvc-part6/_static/image1.png)

Depois de clicar no botão Adicionar, o modelo de exibição \Movies\Create.aspx será criado para você. Como selecionamos "criar" na lista suspensa "exibir conteúdo", a caixa de diálogo Adicionar exibição automaticamente "com Scaffold" parte do conteúdo padrão para nós. O scaffolding criou um formulário HTML &lt;&gt;, um lugar para as mensagens de erro de validação para ir e, como scaffolding sabe sobre filmes, ele criou rótulos e campos para cada propriedade da nossa classe.

[!code-aspx[Main](getting-started-with-mvc-part6/samples/sample2.aspx)]

Como nosso banco de dados fornece uma ID de filme automaticamente, vamos remover esses campos que fazem referência ao modelo. ID do nosso modo de exibição de criação. Remova as 7 linhas depois de &lt;legenda&gt;campos&lt;/Legend&gt; como mostram o campo de ID que não queremos.

Agora, vamos criar um novo filme e adicioná-lo ao banco de dados. Faremos isso executando o aplicativo novamente e visitaremos a URL "/Movies" e clicaremos no link "criar" para adicionar um novo filme.

[![criar-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image4.png)](getting-started-with-mvc-part6/_static/image3.png)

Quando clicamos no botão criar, lançaremos novamente (via HTTP POST) os dados neste formulário para o método/Movies/Create que acabamos de criar. Assim como quando o sistema pega automaticamente o parâmetro "numTimes" e "Name" da URL e os mapeou para os parâmetros em um método anterior, o sistema pegará automaticamente os campos de formulário de uma POSTAgem e os mapeará para um objeto. Nesse caso, os valores de campos em HTML, como "liberado" e "título", serão automaticamente colocados nas propriedades corretas de uma nova instância de um filme.

Vamos examinar o segundo método Create de nosso MoviesController novamente. Observe como ele usa um objeto "Movie" como um argumento:

[!code-csharp[Main](getting-started-with-mvc-part6/samples/sample3.cs)]

Esse objeto de filme foi passado para a versão [HttpPost] do nosso método Create Action e o salvamos no banco de dados e redirecionamos o usuário de volta para o método de ação index () que mostrará o resultado salvo na lista de filmes:

[Lista de filmes ![-Windows Internet Explorer](getting-started-with-mvc-part6/_static/image6.png)](getting-started-with-mvc-part6/_static/image5.png)

No entanto, não estamos verificando se nossos filmes estão corretos e o banco de dados não nos permitirá salvar um filme sem título. Seria legal se pudéssemos dizer ao usuário que antes do banco de dados emitisse um erro. Faremos isso em seguida adicionando o suporte de validação ao nosso aplicativo.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part5.md)
> [Próximo](getting-started-with-mvc-part7.md)
