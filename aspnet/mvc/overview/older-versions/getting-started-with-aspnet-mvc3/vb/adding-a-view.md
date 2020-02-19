---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Adicionando uma exibição (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: fa200935d83bb26c07b302449a6eba6fd67b5322
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457285"
---
# <a name="adding-a-view-vb"></a>Adicionar uma exibição (VB)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no seguinte link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar os pré-requisitos individualmente usando os seguintes links:
> 
> - [Pré-requisitos do Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(suporte + ferramentas de tempo de execução)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no seguinte link: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com código-fonte VB.NET está disponível para acompanhar este tópico. [Baixe a versão do VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se preferir C#, alterne para a [ C# versão](../cs/adding-a-view.md) deste tutorial.

Nesta seção, vamos modificar a classe `HelloWorldController` para usar um arquivo de modelo de exibição para encapsular corretamente o processo de geração de respostas HTML para um cliente.

Vamos começar usando um modelo de exibição com o método `Index` na classe `HelloWorldController`. Atualmente, o método `Index` retorna uma cadeia de caracteres com uma mensagem que é embutida em código dentro da classe Controller. Altere o método `Index` para retornar um objeto `View`, conforme mostrado a seguir:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Agora, vamos adicionar um modelo de exibição ao nosso projeto que podemos invocar com o método `Index`. Para fazer isso, clique com o botão direito do mouse dentro do método `Index` e clique em **Adicionar exibição**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

A caixa de diálogo **Adicionar exibição** é exibida. Deixe as entradas padrão e clique no botão **Adicionar** .

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

A pasta *MvcMovie\Views\HelloWorld* e o arquivo *MvcMovie\Views\HelloWorld\Index.vbhtml* são criados. Você pode vê-los em **Gerenciador de soluções**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Adicione um HTML na marca de `<h2>`. O arquivo *MvcMovie\Views\HelloWorld\Index.vbhtml* modificado é mostrado abaixo.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Execute o aplicativo e navegue até o &quot;&quot; do controlador Hello World (`http://localhost:xxxx/HelloWorld`). O método `Index` em seu controlador não fazia muito trabalho; Ele simplesmente executou a instrução `return View()`, que indicava que queríamos usar um arquivo de modelo de exibição para renderizar uma resposta ao cliente. Como não especificamos explicitamente o nome do arquivo de modelo de exibição a ser usado, ASP.NET MVC assumei o uso do arquivo de exibição *index. vbhtml* dentro da pasta *\Views\HelloWorld* A imagem abaixo mostra a cadeia de caracteres embutida em código na exibição.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Parece muito bom. No entanto, observe que a barra de título do navegador diz &quot;índice&quot; e o título grande na página diz &quot;meu aplicativo MVC.&quot; vamos alterá-las.

## <a name="changing-views-and-layout-pages"></a>Alterando exibições e páginas de layout

Primeiro, vamos alterar o texto &quot;meu aplicativo MVC.&quot; esse texto é compartilhado e aparece em cada página. Na verdade, ele aparece em apenas um lugar em nosso projeto, mesmo que ele esteja em cada página em nosso aplicativo. Vá para a pasta */views/Shared* em **Gerenciador de soluções** e abra o arquivo *layout de\_. vbhtml* . Esse arquivo é chamado de página de layout e é o Shell de &quot;compartilhado&quot; que todas as outras páginas usam.

Observe a linha de `@RenderBody()` de código próxima à parte inferior do arquivo. `RenderBody` é um espaço reservado onde todas as páginas criadas são exibidas, &quot;encapsulado&quot; na página de layout. Altere o cabeçalho `<h1>` de **&quot;** meu aplicativo MVC&quot; para &quot;aplicativo de filme do MVC&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Execute o aplicativo e observe que agora ele indica &quot;aplicativo de filme do MVC&quot;. Clique no link **sobre** , e essa página mostra &quot;aplicativo de filme do MVC&quot;também.

O arquivo *. vbhtml de layout de\_* completo é mostrado abaixo:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Agora, vamos alterar o título da página de índice (exibição).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Abra *MvcMovie\Views\HelloWorld\Index.vbhtml*. Há dois locais para fazer uma alteração: primeiro, o texto que aparece no título do navegador e, em seguida, no cabeçalho secundário (o elemento `<h2>`). Vamos torná-los um pouco diferentes, para que você possa ver qual parte do código altera qual componente do aplicativo.

Execute o aplicativo e navegue até`http://localhost:xx/HelloWorld`. Observe que o título do navegador, o cabeçalho primário e os títulos secundários foram alterados. É fácil fazer grandes alterações em seu aplicativo com pequenas alterações em uma exibição. (Se as alterações não forem exibidas no navegador, talvez o conteúdo armazenado em cache esteja sendo exibido. Pressione Ctrl+F5 no navegador para forçar a resposta do servidor a ser carregada.)

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

Nosso pequeno &quot;de dados&quot; (nesse caso, a &quot;mensagem Olá, Mundo!&quot;) é embutida em código, no entanto. Nosso aplicativo MVC tem V (exibições) e temos C (controladores), mas nenhum M (modelo) ainda. Em breve, veremos como criar um banco de dados e como recuperar de forma a sua base.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passando dados do controlador para a exibição

Antes de passarmos para um banco de dados e falarmos sobre modelos, no entanto, vamos falar primeiro sobre como passar informações do controlador para uma exibição. Queremos passar o que um modelo de exibição requer para processar uma resposta HTML para um cliente. Normalmente, esses objetos são criados e passados por uma classe de controlador para um modelo de exibição, e eles devem conter apenas os dados que o modelo de exibição requer — e não mais.

Anteriormente, com a classe `HelloWorldController`, o método de ação `Welcome` pegava um `name` e um parâmetro `numTimes` e, em seguida, os valores de parâmetro são gerados para o navegador. Em vez de o controlador continuar a renderizar essa resposta diretamente, vamos colocar esses dados em uma bolsa para a exibição. Os controladores e as exibições podem usar um objeto `ViewBag` para manter esses dados. Que será passado para um modelo de exibição automaticamente e usado para renderizar a resposta HTML usando o conteúdo da bolsa como dados. Dessa forma, o controlador se preocupa com uma coisa e com o modelo de exibição com outro, permitindo manter a separação &quot;clara de preocupações&quot; dentro do aplicativo.

Como alternativa, poderíamos definir uma classe personalizada e, em seguida, criar uma instância desse objeto por conta própria, preenchê-la com os dados e passá-la para a exibição. Isso geralmente é chamado de ViewModel, porque é um modelo personalizado para a exibição. Para pequenas quantidades de dados, no entanto, ViewBag funciona muito bem.

Retornar ao arquivo *HelloWorldController. vb* altere o método `Welcome` dentro do controlador para colocar a mensagem e NumTimes em ViewBag. ViewBag é um objeto dinâmico. Isso significa que você pode colocar tudo o que desejar. ViewBag não tem propriedades definidas até que você coloque algo dentro dela.

A `HelloWorldController.vb` completa com a nova classe no mesmo arquivo.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Agora, nosso ViewBag contém dados que serão passados para a exibição automaticamente. Novamente, como alternativa, poderíamos ter passado em nosso próprio objeto, como se gostaríamos de:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Agora precisamos de um modelo de `WelcomeView`! Execute o aplicativo para que o novo código seja compilado. Feche o navegador, clique com o botão direito do mouse dentro do método `Welcome` e clique em **Adicionar exibição**.

Esta é a aparência da caixa de diálogo **Adicionar exibição** .

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Adicione o seguinte código sob o elemento `<h2>` na nova <em>tela de boas-vindas.</em> arquivo vbhtml. Vamos fazer um loop e dizer &quot;Olá&quot; quantas vezes o usuário disser que deveria!

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Execute o aplicativo e navegue até `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Agora, os dados são retirados da URL e passados para o controlador automaticamente. O controlador empacota os dados em um objeto `Model` e passa esse objeto para a exibição. A exibição do que exibe os dados como HTML para o usuário.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Bem, isso era um tipo de &quot;M&quot; para o modelo, mas não o tipo de banco de dados. Vamos ver o que aprendemos e criar um banco de dados de filmes.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Próximo](adding-a-model.md)
