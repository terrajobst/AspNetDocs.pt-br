---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Adicionando uma exibição (C#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará as noções básicas da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: b4a1316feb8d9b7f3ef5ca4755bf1cc5b23e6ef9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615468"
---
# <a name="adding-a-view-c"></a>Adicionar uma exibição (C#)

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

Nesta seção, você modificará a classe `HelloWorldController` para usar os arquivos de modelo de exibição para encapsular corretamente o processo de geração de respostas em HTML para um cliente.

Você criará um arquivo de modelo de exibição usando o novo [mecanismo de exibição do Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introduzido com o ASP.NET MVC 3. Os modelos de exibição baseados em Razor têm uma extensão de arquivo *. cshtml* e fornecem uma maneira elegante de criar uma C#saída HTML usando o. O Razor minimiza o número de caracteres e pressionamentos de teclas necessários ao escrever um modelo de exibição e permite um fluxo de trabalho de codificação rápido e fluido.

Comece usando um modelo de exibição com o método `Index` na classe `HelloWorldController`. Atualmente, o método `Index` retorna uma cadeia de caracteres com uma mensagem que é embutida em código na classe do controlador. Altere o método `Index` para retornar um objeto `View`, conforme mostrado a seguir:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Esse código usa um modelo de exibição para gerar uma resposta HTML para o navegador. No projeto, adicione um modelo de exibição que você pode usar com o método `Index`. Para fazer isso, clique com o botão direito do mouse dentro do método `Index` e clique em **Adicionar exibição**.

![](adding-a-view/_static/image1.png)

A caixa de diálogo **Adicionar exibição** é exibida. Deixe os padrões como estão e clique no botão **Adicionar** :

![](adding-a-view/_static/image2.png)

A pasta *MvcMovie\Views\HelloWorld* e o arquivo *MvcMovie\Views\HelloWorld\Index.cshtml* são criados. Você pode vê-los em **Gerenciador de soluções**:

![](adding-a-view/_static/image3.png)

O seguinte mostra o arquivo *index. cshtml* que foi criado:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Adicione um HTML na marca de `<h2>`. O arquivo *MvcMovie\Views\HelloWorld\Index.cshtml* modificado é mostrado abaixo.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Execute o aplicativo e navegue até o controlador de `HelloWorld` (`http://localhost:xxxx/HelloWorld`). O método `Index` em seu controlador não fazia muito trabalho; Ele simplesmente executou a instrução `return View()`, que especificava que o método deve usar um arquivo de modelo de exibição para renderizar uma resposta para o navegador. Como você não especificou explicitamente o nome do arquivo de modelo de exibição a ser usado, o ASP.NET MVC assume o uso do arquivo de exibição *index. cshtml* na pasta *\Views\HelloWorld* A imagem abaixo mostra a cadeia de caracteres embutida em código na exibição.

![](adding-a-view/_static/image6.png)

Parece muito bom. No entanto, observe que a barra de título do navegador diz "index" e o título grande na página diz "meu aplicativo MVC". Vamos alterá-las.

## <a name="changing-views-and-layout-pages"></a>Alterando exibições e páginas de layout

Primeiro, você deseja alterar o título "meu aplicativo MVC" na parte superior da página. Esse texto é comum a cada página. Na verdade, ele é implementado em apenas um local do projeto, embora ele apareça em cada página do aplicativo. Vá para a pasta */views/Shared* em **Gerenciador de soluções** e abra o arquivo *\_layout. cshtml* . Esse arquivo é chamado de *página de layout* e é o "Shell" compartilhado que todas as outras páginas usam.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Os modelos de layout permitem especificar o layout do contêiner HTML do seu site em um único local e, em seguida, aplicá-lo em várias páginas do seu site. Observe a linha de `@RenderBody()` próxima à parte inferior do arquivo. `RenderBody` é um espaço reservado onde todas as páginas específicas da exibição que você cria são mostradas, "encapsuladas" na página de layout. Altere o título título no modelo de layout de "meu aplicativo MVC" para "aplicativo de filme do MVC".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Execute o aplicativo e observe que agora ele diz "aplicativo de filme do MVC". Clique no link **sobre** , e você verá como essa página mostra "aplicativo de filme do MVC" também. Conseguimos fazer a alteração uma vez no modelo de layout e fazer com que todas as páginas no site reflitam o novo título.

![](adding-a-view/_static/image9.png)

O arquivo *layout. cshtml de\_* completo é mostrado abaixo:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Agora, vamos alterar o título da página de índice (exibição).

Abra *MvcMovie\Views\HelloWorld\Index.cshtml*. Há dois locais para fazer uma alteração: primeiro, o texto que aparece no título do navegador e, em seguida, no cabeçalho secundário (o elemento `<h2>`). Você os tornará ligeiramente diferentes para que possa ver qual parte do código altera qual parte do aplicativo.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Para indicar o título HTML a ser exibido, o código acima define uma propriedade `Title` do objeto `ViewBag` (que está no modelo de exibição *index. cshtml* ). Se você olhar novamente o código-fonte do modelo de layout, observará que o modelo usa esse valor no elemento `<title>` como parte da seção `<head>` do HTML. Usando essa abordagem, você pode facilmente passar outros parâmetros entre o modelo de exibição e o arquivo de layout.

Execute o aplicativo e navegue até `http://localhost:xx/HelloWorld`. Observe que o título do navegador, o cabeçalho primário e os títulos secundários foram alterados. (Se as alterações não forem exibidas no navegador, talvez o conteúdo armazenado em cache esteja sendo exibido. Pressione Ctrl+F5 no navegador para forçar a resposta do servidor a ser carregada.)

Observe também como o conteúdo no modelo de exibição *index. cshtml* foi mesclado com o modelo de exibição *\_layout. cshtml* e uma única resposta HTML foi enviada ao navegador. Os modelos de layout facilitam realmente a realização de alterações que se aplicam a todas as páginas do aplicativo.

![](adding-a-view/_static/image10.png)

Apesar disso, nossos poucos “dados” (nesse caso, a mensagem “Olá de nosso modelo de exibição!”) são embutidos em código. O aplicativo MVC tem um “V” (exibição) e você tem um “C” (controlador), mas ainda nenhum “M” (modelo). Em breve, veremos como criar um banco de dados e como recuperar de forma a sua base.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passando dados do controlador para a exibição

Antes de passarmos para um banco de dados e falarmos sobre modelos, no entanto, vamos falar primeiro sobre como passar informações do controlador para uma exibição. As classes de controlador são invocadas em resposta a uma solicitação de URL de entrada. Uma classe de controlador é onde você escreve o código que manipula os parâmetros de entrada, recupera dados de um banco de dado e, por fim, decide que tipo de resposta enviar de volta ao navegador. Os modelos de exibição podem ser usados de um controlador para gerar e formatar uma resposta HTML para o navegador.

Os controladores são responsáveis por fornecer quaisquer dados ou objetos necessários para que um modelo de exibição processe uma resposta para o navegador. Um modelo de exibição nunca deve executar uma lógica de negócios ou interagir diretamente com um banco de dados. Em vez disso, ele deve funcionar apenas com os dados fornecidos a ele pelo controlador. Manter essa "separação de preocupações" ajuda a manter seu código limpo e mais fácil de manter.

Atualmente, o método de ação `Welcome` na classe `HelloWorldController` usa um `name` e um parâmetro `numTimes` e, em seguida, gera os valores diretamente para o navegador. Em vez de fazer com que o controlador processe essa resposta como uma cadeia de caracteres, vamos alterar o controlador para usar um modelo de exibição. O modelo de exibição gerará uma resposta dinâmica, o que significa que você precisa passar bits de dados apropriados do controlador para a exibição para gerar a resposta. Você pode fazer isso fazendo com que o controlador Coloque os dados dinâmicos de que o modelo de exibição precisa em um `ViewBag` objeto que o modelo de exibição pode acessar.

Retorne ao arquivo *HelloWorldController.cs* e altere o método `Welcome` para adicionar um `Message` e `NumTimes` valor ao objeto `ViewBag`. `ViewBag` é um objeto dinâmico, o que significa que você pode colocar o que desejar nele; o objeto `ViewBag` não tem propriedades definidas até que você coloque algo dentro dele. O arquivo *HelloWorldController.cs* completo tem esta aparência:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Agora, o objeto `ViewBag` contém dados que serão passados para a exibição automaticamente.

Em seguida, você precisa de um modelo de exibição de boas-vindas! No menu **depurar** , selecione **criar MvcMovie** para garantir que o projeto seja compilado.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

Em seguida, clique com o botão direito do mouse dentro do método `Welcome` e clique em **Adicionar exibição**. Esta é a aparência da caixa de diálogo **Adicionar exibição** :

![](adding-a-view/_static/image13.png)

Clique em **Adicionar**e, em seguida, adicione o código a seguir sob o elemento `<h2>` no novo arquivo *Welcome. cshtml* . Você criará um loop que diz "Olá" Quantas vezes o usuário disser que deveria. O arquivo *Welcome. cshtml* completo é mostrado abaixo.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Execute o aplicativo e navegue até a seguinte URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Agora, os dados são retirados da URL e passados para o controlador automaticamente. O controlador empacota os dados em um objeto `ViewBag` e passa esse objeto para a exibição. A exibição, em seguida, exibe os dados como HTML para o usuário.

![](adding-a-view/_static/image14.png)

Bem, isso foi um tipo de “M” de modelo, mas não o tipo de banco de dados. Vamos ver o que aprendemos e criar um banco de dados de filmes.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Próximo](adding-a-model.md)
