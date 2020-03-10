---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Adicionando um controlador (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 2e77f62a9796211b0e59a99c71bc532659b7cb92
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540512"
---
# <a name="adding-a-controller-vb"></a>Adicionar um controlador (VB)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no seguinte link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar os pré-requisitos individualmente usando os seguintes links:
> 
> - [Pré-requisitos do Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(suporte + ferramentas de tempo de execução)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no seguinte link: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com código-fonte VB.NET está disponível para acompanhar este tópico. [Baixe a versão do VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se preferir C#, alterne para a [ C# versão](../cs/adding-a-controller.md) deste tutorial.

O MVC significa *Model-View-Controller*. O MVC é um padrão para o desenvolvimento de aplicativos de forma que cada parte tenha uma responsabilidade separada:

- Modelo: os dados para seu aplicativo.
- Exibições: os arquivos de modelo que seu aplicativo usará para gerar respostas HTML dinamicamente.
- Controladores: classes que manipulam solicitações de URL de entrada para o aplicativo, recuperam dados de modelo e, em seguida, especificam modelos de exibição que renderizam uma resposta ao cliente.

Abordaremos todos esses conceitos neste tutorial e mostraremos como usá-los para criar um aplicativo.

Crie um novo controlador clicando com o botão direito do mouse na pasta *controladores* em **Gerenciador de soluções** e, em seguida, selecionando **Adicionar controlador**.

[![Addcontroller](adding-a-controller/_static/image2.png "Addcontroller")](adding-a-controller/_static/image1.png)

Nomeie o novo controlador &quot;HelloWorldController&quot; e clique em **Adicionar**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Observe na **Gerenciador de soluções** à direita que um novo arquivo foi criado para você chamado *HelloWorldController.cs* e que o arquivo está aberto no IDE.

Dentro do novo bloco de `public class HelloWorldController`, crie dois novos métodos que se parecem com o código a seguir. Retornaremos uma cadeia de caracteres de HTML diretamente do controlador como um exemplo.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Seu controlador é nomeado `HelloWorldController` e o novo método é nomeado `Index`. Execute o aplicativo (pressione F5 ou CTRL + F5). Depois que o navegador for iniciado, acrescente &quot;HelloWorld&quot; ao caminho na barra de endereços. (No meu computador, é `http://localhost:43246/HelloWorld`) Seu navegador se parecerá com a captura de tela abaixo. No método acima, o código retornou uma cadeia de caracteres diretamente. Dissemos ao sistema para retornar apenas um HTML, e ele fazia!

![](adding-a-controller/_static/image5.png)

O ASP.NET MVC invoca classes de controlador diferentes (e métodos de ação diferentes dentro delas) dependendo da URL de entrada. A lógica de mapeamento padrão usada pelo ASP.NET MVC usa um formato como este para controlar qual código é invocado:

`/[Controller]/[ActionName]/[Parameters]`

A primeira parte da URL determina a classe do controlador a ser executada. Portanto, */HelloWorld* é mapeado para a classe `HelloWorldController`. A segunda parte da URL determina o método de ação na classe a ser executada. Portanto, */HelloWorld/index* faria com que o método de `Index` da classe `HelloWorldController` fosse executado. Observe que precisamos apenas visitar */HelloWorld* acima e o método `Index` foi usado por padrão. Isso ocorre porque um método chamado `Index` é o método padrão que será chamado em um controlador se um não for especificado explicitamente.

Navegue até `http://localhost:xxxx/HelloWorld/Welcome`. O método `Welcome` é executado e retorna a cadeia de caracteres &quot;este é o método de ação de boas-vindas...&quot;. O mapeamento do MVC padrão é `/[Controller]/[ActionName]/[Parameters]`. Para essa URL, o controlador é `HelloWorld` e `Welcome` é o método. Ainda não usamos a parte `[Parameters]` da URL.

![](adding-a-controller/_static/image6.png)

Vamos modificar o exemplo ligeiramente para que possamos passar algumas informações de parâmetro de uma URL para o controlador (por exemplo, */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Altere o método `Welcome` para incluir dois parâmetros, conforme mostrado abaixo. Observe que usamos o recurso de parâmetro opcional VB para indicar que o parâmetro `numTimes` deve usar como padrão 1 se nenhum valor for passado para esse parâmetro.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Execute o aplicativo e navegue até `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Você pode experimentar valores diferentes para `name` e `numtimes`. O sistema mapeia automaticamente os parâmetros nomeados da cadeia de caracteres de consulta na barra de endereços para parâmetros em seu método.

![](adding-a-controller/_static/image7.png)

Em ambos os exemplos, o controlador está fazendo a parte VC do MVC, que é a exibição e o trabalho do controlador. O controlador retorna o HTML diretamente. Normalmente, não queremos que os controladores retornem HTML diretamente, já que isso se torna muito complicado para o código. Em vez disso, normalmente usaremos um arquivo de modelo de exibição separado para ajudar a gerar a resposta HTML. Vejamos como podemos fazer isso.

> [!div class="step-by-step"]
> [Anterior](intro-to-aspnet-mvc-3.md)
> [Próximo](adding-a-view.md)
