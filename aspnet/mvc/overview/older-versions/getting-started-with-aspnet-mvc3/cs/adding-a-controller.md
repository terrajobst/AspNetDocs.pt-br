---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Adicionando um controlador (C#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que i...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 959116ff773f4ef466cda6b172e8321590b50e5b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540939"
---
# <a name="adding-a-controller-c"></a>Adicionar um controlador (C#)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstra mais recursos.
> 
> 
> Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no seguinte link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar os pré-requisitos individualmente usando os seguintes links:
> 
> - [Pré-requisitos do Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(suporte + ferramentas de tempo de execução)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no seguinte link: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com C# código-fonte está disponível para acompanhar este tópico. [Baixe a C# versão](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se preferir Visual Basic, alterne para a [versão Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.

O MVC significa *Model-View-Controller*. O MVC é um padrão para o desenvolvimento de aplicativos que são bem arquitetados e fáceis de manter. Os aplicativos baseados em MVC contêm:

- Controladores: classes que tratam solicitações de entrada para o aplicativo, recuperam dados de modelo e, em seguida, especificam modelos de exibição que retornam uma resposta ao cliente.
- Modelos: classes que representam os dados do aplicativo e que usam a lógica de validação para impor regras de negócio para esses dados.
- Exibições: Arquivos de modelo que seu aplicativo usa para gerar dinamicamente respostas HTML.

Abordaremos todos esses conceitos nesta série de tutoriais e mostraremos como usá-los para criar um aplicativo.

Vamos começar criando uma classe de controlador. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *controladores* e selecione **Adicionar controlador**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Nomeie o novo controlador "HelloWorldController". Deixe o modelo padrão como **controlador vazio** e clique em **Adicionar**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Observe no **Gerenciador de soluções** que um novo arquivo foi criado chamado *HelloWorldController.cs*. O arquivo está aberto no IDE.

![](adding-a-controller/_static/image5.png)

Dentro do bloco de `public class HelloWorldController`, crie dois métodos que se parecem com o código a seguir. O controlador retornará uma cadeia de caracteres de HTML como um exemplo.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Seu controlador é nomeado `HelloWorldController` e o primeiro método acima é nomeado `Index`. Vamos chamá-lo de um navegador. Execute o aplicativo (pressione F5 ou CTRL + F5). No navegador, acrescente "HelloWorld" ao caminho na barra de endereços. (Por exemplo, na ilustração abaixo, é `http://localhost:43246/HelloWorld.`) A página no navegador se parecerá com a captura de tela a seguir. No método acima, o código retornou uma cadeia de caracteres diretamente. Você disse ao sistema para retornar apenas um HTML e ele fazia!

![](adding-a-controller/_static/image6.png)

O ASP.NET MVC invoca classes de controlador diferentes (e métodos de ação diferentes dentro delas) dependendo da URL de entrada. A lógica de mapeamento padrão usada pelo ASP.NET MVC usa um formato como este para determinar qual código invocar:

`/[Controller]/[ActionName]/[Parameters]`

A primeira parte da URL determina a classe do controlador a ser executada. Portanto, */HelloWorld* é mapeado para a classe `HelloWorldController`. A segunda parte da URL determina o método de ação na classe a ser executada. Portanto, */HelloWorld/index* faria com que o método de `Index` da classe `HelloWorldController` fosse executado. Observe que precisamos apenas navegar até */HelloWorld* e o método `Index` foi usado por padrão. Isso ocorre porque um método chamado `Index` é o método padrão que será chamado em um controlador se um não for especificado explicitamente.

Navegue até `http://localhost:xxxx/HelloWorld/Welcome`. O método `Welcome` é executado e retorna a cadeia de caracteres “Este é o método de ação Boas-vindas...”. O mapeamento do MVC padrão é `/[Controller]/[ActionName]/[Parameters]`. Para essa URL, o controlador é `HelloWorld` e `Welcome` é o método de ação. Você ainda não usou a parte `[Parameters]` da URL.

![](adding-a-controller/_static/image7.png)

Vamos modificar o exemplo ligeiramente para que você possa passar algumas informações de parâmetro da URL para o controlador (por exemplo, */HelloWorld/Welcome? Name = Scott&amp;numtimes = 4*). Altere o método `Welcome` para incluir dois parâmetros, conforme mostrado abaixo. Observe que o código usa o C# recurso opcional-Parameter para indicar que o parâmetro `numTimes` deve usar como padrão 1 se nenhum valor for passado para esse parâmetro.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Execute o aplicativo e navegue até a URL de exemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Você pode tentar valores diferentes para `name` e `numtimes` na URL. O sistema mapeia automaticamente os parâmetros nomeados da cadeia de caracteres de consulta na barra de endereços para parâmetros em seu método.

![](adding-a-controller/_static/image8.png)

Em ambos os exemplos, o controlador está fazendo a parte "VC" do MVC, ou seja, o modo de exibição e o controlador funcionam. O controlador retorna o HTML diretamente. Normalmente, você não quer que os controladores retornem HTML diretamente, já que isso se torna muito complicado para o código. Em vez disso, normalmente usaremos um arquivo de modelo de exibição separado para ajudar a gerar a resposta HTML. Vejamos em seguida como podemos fazer isso.

> [!div class="step-by-step"]
> [Anterior](intro-to-aspnet-mvc-3.md)
> [Próximo](adding-a-view.md)
