---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Adicionando um controlador | Microsoft Docs
author: shanselman
description: Uma versão atualizada se este tutorial estiver disponível aqui usando Visual Studio 2013. O novo tutorial usa o ASP.NET MVC 5, que fornece muitas melhorias em relação a t...
ms.author: riande
ms.date: 08/14/2010
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: e2a298584473f57c2b14edf507f0f6886d906ea3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543977"
---
# <a name="adding-a-controller"></a>Adicionando um controlador

por [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Uma versão atualizada se este tutorial estiver disponível [aqui](../../getting-started/introduction/getting-started.md) usando [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). O novo tutorial usa o ASP.NET MVC 5, que fornece vários aprimoramentos sobre este tutorial.
>
>
> Este é um tutorial principiante que apresenta os fundamentos do ASP.NET MVC. Você criará um aplicativo Web simples que lê e grava de um banco de dados. Visite o [ASP.NET MVC Learning Center](../../../index.md) para encontrar outros tutoriais e exemplos do ASP.NET MVC.

O MVC significa modelo, exibição, controlador. O MVC é um padrão para o desenvolvimento de aplicativos de forma que cada parte tenha uma responsabilidade diferente de outra.

- Modelo: os dados do seu aplicativo
- Exibições: os arquivos de modelo que seu aplicativo usará para gerar respostas HTML dinamicamente.
- Controladores: classes que manipulam solicitações de URL de entrada para o aplicativo, recuperam dados de modelo e especificam modelos de exibição que processam uma resposta de volta ao cliente

Abordaremos todos esses conceitos neste tutorial e mostraremos como usá-los para criar um aplicativo.

Vamos criar um novo controlador clicando com o botão direito do mouse na pasta controladores no Gerenciador de soluções e selecionando Adicionar controlador.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Nomeie o novo controlador "HelloWorldController" e clique em Adicionar.

[Caixa de diálogo ![adicionar controlador](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Observe na Gerenciador de Soluções à direita que um novo arquivo foi criado para você chamado HelloWorldController.cs e que o arquivo agora está aberto no **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Crie dois novos métodos que se parecem com isso dentro de sua nova classe pública HelloWorldController. Retornaremos uma cadeia de caracteres de HTML diretamente do nosso controlador como um exemplo.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

Seu controlador é denominado HelloWorldController e o novo método é chamado de index. Execute o aplicativo novamente, assim como antes (clique no botão reproduzir ou pressione F5 para fazer isso). Depois que o navegador for iniciado, altere o caminho na barra de endereços para `http://localhost:xx/HelloWorld` em que XX é o número escolhido pelo computador. Agora seu navegador deve se parecer com a captura de tela abaixo. Em nosso método acima, retornamos uma cadeia de caracteres passada para um método chamado "content". Dissemos que o sistema simplesmente retorna algum HTML e foi!

O ASP.NET MVC invoca classes de controlador diferentes (e métodos de ação diferentes dentro delas) dependendo da URL de entrada. A lógica de mapeamento padrão usada pelo ASP.NET MVC usa um formato como este para controlar qual código é executado:

/[Controller]/[ActionName]/[Parameters]

A primeira parte da URL determina a classe do controlador a ser executada. Portanto,/HelloWorld é mapeado para a classe HelloWorldController. A segunda parte da URL determina o método de ação na classe a ser executada. Portanto,/HelloWorld/Index faria com que o método index () da classe HelloWorldController fosse executado. Observe que precisamos apenas visitar/HelloWorld acima e o índice do método estava implícito. Isso ocorre porque um método chamado "index" é o método padrão que será chamado em um controlador se um não for especificado explicitamente.

[![esta é minha ação padrão](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Agora, vamos visitar `http://localhost:xx/HelloWorld/Welcome.` agora nosso método de boas-vindas executou e retornou sua cadeia de caracteres HTML.

Novamente,/[Controller]/[ActionName]/[Parameters] para que o controlador seja HelloWorld e Welcome seja o método nesse caso. Ainda não fizemos parâmetros.

[![este é o método de ação de boas-vindas](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Vamos modificar nossa amostra ligeiramente para que possamos passar algumas informações da URL para o nosso controlador, por exemplo, como esta:/HelloWorld/Welcome? Name = Scott&amp;numtimes = 4. Altere seu método de boas-vindas para incluir dois parâmetros e atualizá-lo como abaixo. Observe que usamos o recurso de C# parâmetro opcional para indicar que o parâmetro numTimes deve usar como padrão 1 se não for passado.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Execute o aplicativo e visite `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` alterando o valor de Name e numtimes como desejar. O sistema mapeou automaticamente os parâmetros nomeados da cadeia de caracteres de consulta na barra de endereços para parâmetros em seu método.

Em ambos os exemplos, o controlador está fazendo todo o trabalho e está retornando o HTML diretamente. Normalmente, não queremos que nossos controladores retornem HTML diretamente, pois isso acaba sendo muito complicado para codificar. Em vez disso, normalmente usaremos um arquivo de modelo de exibição separado para ajudar a gerar a resposta HTML. Vejamos como podemos fazer isso. Feche o navegador e retorne ao IDE.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part1.md)
> [Próximo](getting-started-with-mvc-part3.md)
