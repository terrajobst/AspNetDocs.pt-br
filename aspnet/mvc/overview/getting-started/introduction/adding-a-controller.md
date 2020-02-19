---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Adicionando um controlador | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 194a8a7398e163f0c37164a8724f98b16444984b
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457213"
---
# <a name="adding-a-controller"></a>Adicionando um controlador

por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

O MVC significa *Model-View-Controller*. O MVC é um padrão para o desenvolvimento de aplicativos que são bem arquitetados, testados e fáceis de manter. Os aplicativos baseados em MVC contêm:

- **M** Odels: classes que representam os dados do aplicativo e que usam a lógica de validação para impor regras de negócio para esses dados.
- **V** iews: Arquivos de modelo que seu aplicativo usa para gerar DINAMICAMENTE respostas HTML.
- **C** Ontrollers: classes que manipulam solicitações de navegador recebidas, recuperam dados de modelo e, em seguida, especificam modelos de exibição que retornam uma resposta ao navegador.

Abordaremos todos esses conceitos nesta série de tutoriais e mostraremos como usá-los para criar um aplicativo.

Vamos começar criando uma classe de controlador. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *controladores* e clique em **Adicionar**e em **controlador**.

![](adding-a-controller/_static/image1.png)

Na caixa de diálogo **Adicionar Scaffold** , clique em **controlador MVC 5 – vazio**e, em seguida, clique em **Adicionar**.

![](adding-a-controller/_static/image2.png)  

Nomeie o novo controlador "HelloWorldController" e clique em **Adicionar**.

![Adicionar controlador](adding-a-controller/_static/image3.png)

Observe no **Gerenciador de soluções** que um novo arquivo foi criado chamado *HelloWorldController.cs* e uma nova pasta *Views\HelloWorld*. O controlador está aberto no IDE.

![](adding-a-controller/_static/image4.png)

Substitua o conteúdo do arquivo pelo código a seguir.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Os métodos do controlador retornarão uma cadeia de caracteres de HTML como um exemplo. O controlador é nomeado `HelloWorldController` e o primeiro método é nomeado `Index`. Vamos chamá-lo de um navegador. Execute o aplicativo (pressione F5 ou CTRL + F5). No navegador, acrescente &quot;HelloWorld&quot; ao caminho na barra de endereços. (Por exemplo, na ilustração abaixo, é `http://localhost:1234/HelloWorld.`) A página no navegador se parecerá com a captura de tela a seguir. No método acima, o código retornou uma cadeia de caracteres diretamente. Você disse ao sistema para retornar apenas um HTML e ele fazia!

![](adding-a-controller/_static/image5.png)

O ASP.NET MVC invoca classes de controlador diferentes (e métodos de ação diferentes dentro delas) dependendo da URL de entrada. A lógica de roteamento de URL padrão usada pelo ASP.NET MVC usa um formato como este para determinar qual código invocar:

`/[Controller]/[ActionName]/[Parameters]`

Você define o formato para roteamento no *aplicativo\_arquivo start/RouteConfig. cs* .

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Quando você executa o aplicativo e não fornece nenhum segmento de URL, ele usa como padrão o controlador "Home" e o método de ação "index" especificado na seção padrões do código acima.

A primeira parte da URL determina a classe do controlador a ser executada. Portanto, */HelloWorld* é mapeado para a classe `HelloWorldController`. A segunda parte da URL determina o método de ação na classe a ser executada. Portanto, */HelloWorld/index* faria com que o método de `Index` da classe `HelloWorldController` fosse executado. Observe que precisamos apenas navegar até */HelloWorld* e o método `Index` foi usado por padrão. Isso ocorre porque um método chamado `Index` é o método padrão que será chamado em um controlador se um não for especificado explicitamente. A terceira parte do segmento de URL (`Parameters`) refere-se aos dados de rota. Veremos os dados de rota mais adiante neste tutorial.

Navegue até `http://localhost:xxxx/HelloWorld/Welcome`. O método `Welcome` é executado e retorna a cadeia de caracteres &quot;este é o método de ação de boas-vindas...&quot;. O mapeamento do MVC padrão é `/[Controller]/[ActionName]/[Parameters]`. Para essa URL, o controlador é `HelloWorld` e `Welcome` é o método de ação. Você ainda não usou a parte `[Parameters]` da URL.

![](adding-a-controller/_static/image6.png)

Vamos modificar o exemplo ligeiramente para que você possa passar algumas informações de parâmetro da URL para o controlador (por exemplo, */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Altere o método `Welcome` para incluir dois parâmetros, conforme mostrado abaixo. Observe que o código usa o C# recurso opcional-Parameter para indicar que o parâmetro `numTimes` deve usar como padrão 1 se nenhum valor for passado para esse parâmetro.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Observação de segurança: o código acima usa [HttpUtility. HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) para proteger o aplicativo contra entradas mal-intencionadas (ou seja, JavaScript). Para obter mais informações [, consulte Como: proteger contra explorações de script em um aplicativo Web aplicando codificação HTML a cadeias de caracteres](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).

 Execute o aplicativo e navegue até a URL de exemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). Você pode tentar valores diferentes para `name` e `numtimes` na URL. O [sistema de associação de modelo MVC do ASP.net](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mapeia automaticamente os parâmetros nomeados da cadeia de caracteres de consulta na barra de endereços para parâmetros em seu método.

![](adding-a-controller/_static/image7.png)

No exemplo acima, o segmento de URL (`Parameters`) não é usado, os parâmetros `name` e `numTimes` são passados como [cadeias de caracteres de consulta](http://en.wikipedia.org/wiki/Query_string). O ? (ponto de interrogação) na URL acima é um separador e as cadeias de consulta seguem. O caractere &amp; separa as cadeias de consulta.

Substitua o método Welcome pelo seguinte código:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Execute o aplicativo e insira a seguinte URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Desta vez, o terceiro segmento de URL correspondeu ao parâmetro de rota `ID.` o `Welcome` método de ação contém um parâmetro (`ID`) que correspondeu à especificação de URL no método `RegisterRoutes`.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

Em aplicativos MVC ASP.NET, é mais comum passar parâmetros como dados de rota (como fizemos com a ID acima) do que passá-los como cadeias de consulta. Você também pode adicionar uma rota para passar o `name` e `numtimes` em parâmetros como dados de rota na URL. No arquivo de *\_do aplicativo Start\RouteConfig.cs* , adicione a rota "Olá":

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Execute o aplicativo e navegue até `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

Para muitos aplicativos MVC, a rota padrão funciona bem. Você aprenderá mais adiante neste tutorial para passar dados usando o associador de modelo e não precisará modificar a rota padrão para isso.

Nestes exemplos, o controlador está fazendo a &quot;VC&quot; parte do MVC, ou seja, o modo de exibição e o controlador funcionam. O controlador retorna o HTML diretamente. Normalmente, você não quer que os controladores retornem HTML diretamente, já que isso se torna muito complicado para o código. Em vez disso, normalmente usaremos um arquivo de modelo de exibição separado para ajudar a gerar a resposta HTML. Vejamos em seguida como podemos fazer isso.

> [!div class="step-by-step"]
> [Anterior](getting-started.md)
> [Próximo](adding-a-view.md)
