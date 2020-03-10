---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Adicionando um controlador | Microsoft Docs
author: Rick-Anderson
description: 'Observação: uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstrar...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: f528c56435976c7f31fce453c834ef9eaebe6244
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78599837"
---
# <a name="adding-a-controller"></a>Adicionando um controlador

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstra mais recursos.

O MVC significa *Model-View-Controller*. O MVC é um padrão para o desenvolvimento de aplicativos que são bem arquitetados, testados e fáceis de manter. Os aplicativos baseados em MVC contêm:

- **M** Odels: classes que representam os dados do aplicativo e que usam a lógica de validação para impor regras de negócio para esses dados.
- **V** iews: Arquivos de modelo que seu aplicativo usa para gerar DINAMICAMENTE respostas HTML.
- **C** Ontrollers: classes que manipulam solicitações de navegador recebidas, recuperam dados de modelo e, em seguida, especificam modelos de exibição que retornam uma resposta ao navegador.

Abordaremos todos esses conceitos nesta série de tutoriais e mostraremos como usá-los para criar um aplicativo.

Vamos começar criando uma classe de controlador. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *controladores* e selecione **Adicionar controlador**.

![](adding-a-controller/_static/image1.png)

Nomeie seu novo controlador &quot;HelloWorldController&quot;. Deixe o modelo padrão como **controlador MVC vazio** e clique em **Adicionar**.

![Adicionar controlador](adding-a-controller/_static/image2.png)

Observe no **Gerenciador de soluções** que um novo arquivo foi criado chamado *HelloWorldController.cs*. O arquivo está aberto no IDE.

![](adding-a-controller/_static/image3.png)

Substitua o conteúdo do arquivo pelo código a seguir.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Os métodos do controlador retornarão uma cadeia de caracteres de HTML como um exemplo. O controlador é nomeado `HelloWorldController` e o primeiro método acima é nomeado `Index`. Vamos chamá-lo de um navegador. Execute o aplicativo (pressione F5 ou CTRL + F5). No navegador, acrescente &quot;HelloWorld&quot; ao caminho na barra de endereços. (Por exemplo, na ilustração abaixo, é `http://localhost:1234/HelloWorld.`) A página no navegador se parecerá com a captura de tela a seguir. No método acima, o código retornou uma cadeia de caracteres diretamente. Você disse ao sistema para retornar apenas um HTML e ele fazia!

![](adding-a-controller/_static/image4.png)

O ASP.NET MVC invoca classes de controlador diferentes (e métodos de ação diferentes dentro delas) dependendo da URL de entrada. A lógica de roteamento de URL padrão usada pelo ASP.NET MVC usa um formato como este para determinar qual código invocar:

`/[Controller]/[ActionName]/[Parameters]`

A primeira parte da URL determina a classe do controlador a ser executada. Portanto, */HelloWorld* é mapeado para a classe `HelloWorldController`. A segunda parte da URL determina o método de ação na classe a ser executada. Portanto, */HelloWorld/index* faria com que o método de `Index` da classe `HelloWorldController` fosse executado. Observe que precisamos apenas navegar até */HelloWorld* e o método `Index` foi usado por padrão. Isso ocorre porque um método chamado `Index` é o método padrão que será chamado em um controlador se um não for especificado explicitamente.

Navegue até `http://localhost:xxxx/HelloWorld/Welcome`. O método `Welcome` é executado e retorna a cadeia de caracteres &quot;este é o método de ação de boas-vindas...&quot;. O mapeamento do MVC padrão é `/[Controller]/[ActionName]/[Parameters]`. Para essa URL, o controlador é `HelloWorld` e `Welcome` é o método de ação. Você ainda não usou a parte `[Parameters]` da URL.

![](adding-a-controller/_static/image5.png)

Vamos modificar o exemplo ligeiramente para que você possa passar algumas informações de parâmetro da URL para o controlador (por exemplo, */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Altere o método `Welcome` para incluir dois parâmetros, conforme mostrado abaixo. Observe que o código usa o C# recurso opcional-Parameter para indicar que o parâmetro `numTimes` deve usar como padrão 1 se nenhum valor for passado para esse parâmetro.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Execute o aplicativo e navegue até a URL de exemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Você pode tentar valores diferentes para `name` e `numtimes` na URL. O [sistema de associação de modelo MVC do ASP.net](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mapeia automaticamente os parâmetros nomeados da cadeia de caracteres de consulta na barra de endereços para parâmetros em seu método.

![](adding-a-controller/_static/image6.png)

Em ambos os exemplos, o controlador está fazendo o &quot;VC&quot; parte do MVC, ou seja, o modo de exibição e o controlador funcionam. O controlador retorna o HTML diretamente. Normalmente, você não quer que os controladores retornem HTML diretamente, já que isso se torna muito complicado para o código. Em vez disso, normalmente usaremos um arquivo de modelo de exibição separado para ajudar a gerar a resposta HTML. Vejamos em seguida como podemos fazer isso.

> [!div class="step-by-step"]
> [Anterior](intro-to-aspnet-mvc-4.md)
> [Próximo](adding-a-view.md)
