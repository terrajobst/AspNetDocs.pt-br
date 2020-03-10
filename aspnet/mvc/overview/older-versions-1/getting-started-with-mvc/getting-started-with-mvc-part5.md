---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Acessando os dados do modelo de um controlador | Microsoft Docs
author: shanselman
description: Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Crie um aplicativo Web simples que lê e grava de um banco de dados.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 207ed880977d794d81efdc1ea458d17a68d501d8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543746"
---
# <a name="accessing-your-models-data-from-a-controller"></a>Acessar dados do modelo por meio de um controlador

por [Scott Hanselman](https://github.com/shanselman)

> Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Você criará um aplicativo Web simples que lê e grava de um banco de dados. Visite o [ASP.NET MVC Learning Center](../../../index.md) para encontrar outros tutoriais e exemplos do ASP.NET MVC.

Nesta seção, vamos criar uma nova classe MoviesController e escrever um código que recupere nossos dados de filme e o exiba de volta para o navegador usando um modelo de exibição.

Clique com o botão direito do mouse na pasta controladores e faça um novo MoviesController.

[![adicionar controlador](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Isso criará um novo arquivo "MoviesController.cs" sob nossa pasta \Controllers em nosso projeto. Vamos atualizar o MovieController para recuperar a lista de filmes do nosso banco de dados populado recentemente.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Estamos executando uma consulta LINQ para que recuperemos apenas filmes lançados após o verão de 1984. Precisaremos de um modelo de exibição para renderizar essa lista de filmes de volta, então clique com o botão direito do mouse no método e selecione Adicionar exibição para criá-lo.

Na caixa de diálogo Adicionar exibição, vamos indicar que estamos passando uma lista&lt;Movies. Models. Movie&gt; para nosso modelo de exibição. Ao contrário das horas anteriores, usamos a caixa de diálogo Adicionar exibição e optamos por criar um modelo "vazio", desta vez indicaremos que queremos que o Visual Studio "Scaffold" automaticamente um modelo de exibição para nós com algum conteúdo padrão. Faremos isso selecionando o item "listar" no menu suspenso "exibir conteúdo".

Lembre-se de que, quando você tiver criado uma nova classe, precisará compilar seu aplicativo para que ele apareça na caixa de diálogo Adicionar exibição.

![Adicionar modo de exibição](getting-started-with-mvc-part5/_static/image3.png)

Clique em Adicionar e o sistema gerará automaticamente o código para um modo de exibição para nós que exibe nossa lista de filmes. Esse é um bom momento para alterar o cabeçalho do &lt;H2&gt; para algo como "minha lista de filmes" como fizemos anteriormente com o Olá, Mundo exibição.

[Filmes de ![-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Execute o aplicativo e visite/Movies na barra de endereços. Agora, recuperamos os dados do Database usando uma consulta básica dentro do controlador e retornamos os dados a uma exibição que conhece os filmes. Essa exibição, em seguida, gira na lista de filmes e cria uma tabela de dados para nós.

[Lista de filmes ![-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Não implementaremos a funcionalidade de edição, detalhes e exclusão com este aplicativo. portanto, não precisamos dos links padrão que o modelo Scaffold criou para nós. Abra o arquivo/Movies/Index.aspx e remova-o.

Aqui está o código-fonte para o que nosso modelo de exibição atualizado deve ter uma aparência assim que fizermos essas alterações:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Ele está criando links que não precisaremos, portanto, vamos excluí-los para este exemplo. No entanto, vamos manter nosso novo link Create, como é o próximo! Veja a aparência de nosso aplicativo com essa coluna removida.

[Lista de filmes ![-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Agora temos uma lista simples de nossos dados de filme. No entanto, se clicarmos no link "criar novo", obteremos um erro, pois ele não é conectado! Vamos implementar um método Create Action e permitir que um usuário insira novos filmes em nosso banco de dados.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part4.md)
> [Próximo](getting-started-with-mvc-part6.md)
