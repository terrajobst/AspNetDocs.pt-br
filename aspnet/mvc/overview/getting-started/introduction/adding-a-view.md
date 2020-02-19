---
title: Adicionando uma exibição a um aplicativo MVC
author: Rick-Anderson
description: Adicionando uma exibição a um aplicativo MVC
ms.author: riande
ms.date: 01/23/2019
uid: mvc/overview/getting-started/introduction/adding-a-view
ms.openlocfilehash: 0bc6ac06d12aaee4b2a11c1bf246f9f20f0be017
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456641"
---
# <a name="adding-a-view"></a>Adicionar uma exibição

por [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [Tutorial Note](index.md)]

Nesta seção, você modificará a classe `HelloWorldController` para usar os arquivos de modelo de exibição para encapsular corretamente o processo de geração de respostas em HTML para um cliente. 

Você criará um arquivo de modelo de exibição usando o [mecanismo de exibição do Razor](../../../../web-pages/overview/getting-started/introducing-razor-syntax-c.md). Os modelos de exibição baseados em Razor têm uma extensão de arquivo *. cshtml* e fornecem uma maneira elegante de criar uma C#saída HTML usando o. O Razor minimiza o número de caracteres e pressionamentos de teclas necessários ao escrever um modelo de exibição e permite um fluxo de trabalho de codificação rápido e fluido.

Atualmente, o método `Index` retorna uma cadeia de caracteres com uma mensagem que é embutida em código na classe do controlador. Altere o método `Index` para chamar o método de [exibição](/dotnet/api/microsoft.aspnetcore.mvc.controller.view#Microsoft_AspNetCore_Mvc_Controller_View) de controladores, conforme mostrado no código a seguir:

[!code-csharp[Main](adding-a-view/samples/sample1.cs?highlight=1,3)]

O método `Index` acima usa um modelo de exibição para gerar uma resposta HTML para o navegador. Os métodos do controlador (também conhecidos como [métodos de ação](http://rachelappel.com/asp.net-mvc-actionresults-explained)), como o método `Index` acima, geralmente retornam um [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (ou uma classe derivada de [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), não tipos primitivos como cadeia de caracteres.

Clique com o botão direito do mouse na pasta *Views\HelloWorld* e clique em **Adicionar**e, em seguida, clique em **exibição MVC 5 página com layout (Razor)** .
  
![](adding-a-view/_static/image1.png)   
  
Na caixa de diálogo **especificar nome para o item** , insira *índice*e clique em **OK**.  
  
![](adding-a-view/_static/image2.png)  
  
Na caixa de diálogo **selecionar uma página de layout** , aceite o padrão **\_layout. cshtml** e clique em **OK**.  
  
![](adding-a-view/_static/image3.png)  
  
Na caixa de diálogo acima, a pasta *Views\Shared* está selecionada no painel esquerdo. Se você tiver um arquivo de layout personalizado em outra pasta, poderá selecioná-lo. Falaremos sobre o arquivo de layout mais adiante no tutorial

O arquivo *MvcMovie\Views\HelloWorld\Index.cshtml* é criado.

![](adding-a-view/_static/image4.png)

Adicione a seguinte marcação realçada.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml?highlight=4-11)]

Clique com o botão direito do mouse no arquivo *index. cshtml* e selecione **Exibir no navegador**.

![PI](adding-a-view/_static/image5.png)

Você também pode clicar com o botão direito do mouse no arquivo *index. cshtml* e selecionar **Exibir no Inspetor de página.** Consulte o [tutorial Inspetor de página](../../views/using-page-inspector-in-aspnet-mvc.md) para obter mais informações.

Como alternativa, execute o aplicativo e navegue até o controlador de `HelloWorld` (`http://localhost:xxxx/HelloWorld`). O método `Index` em seu controlador não fazia muito trabalho; Ele simplesmente executou a instrução `return View()`, que especificava que o método deve usar um arquivo de modelo de exibição para renderizar uma resposta para o navegador. Como você não especificou explicitamente o nome do arquivo de modelo de exibição a ser usado, o ASP.NET MVC assume o uso do arquivo de exibição *index. cshtml* na pasta *\Views\HelloWorld* A imagem abaixo mostra a cadeia de caracteres &quot;Olá de nosso modelo de exibição!&quot; embutido na exibição.

![](adding-a-view/_static/image6.png)

Parece muito bom. No entanto, observe que a barra de título do navegador mostra "índice-meu aplicativo ASP.NET" e o link grande na parte superior da página diz "nome do aplicativo". Dependendo do tamanho em que você fizer a janela do navegador, talvez seja necessário clicar nas três barras no canto superior direito para ver os links de **início**, **sobre**, **contato**, **registro** e **logon** .

## <a name="changing-views-and-layout-pages"></a>Alterando exibições e páginas de layout

Primeiro, você deseja alterar o nome do aplicativo &quot;&quot; link na parte superior da página. Esse texto é comum a cada página. Na verdade, ele é implementado em apenas um local no projeto, mesmo que ele apareça em cada página do aplicativo. Vá para a pasta */views/Shared* em **Gerenciador de soluções** e abra o arquivo *\_layout. cshtml* . Esse arquivo é chamado de *página de layout* e está na pasta compartilhada que todas as outras páginas usam.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Os modelos de layout permitem especificar o layout do contêiner HTML do seu site em um único local e, em seguida, aplicá-lo em várias páginas do seu site. Localize a linha `@RenderBody()`. `RenderBody` é um espaço reservado em que todas as páginas específicas à exibição criadas são mostradas, &quot;encapsuladas&quot; na página de layout. Por exemplo, se você selecionar o link **about** , a exibição *Views\Home\About.cshtml* será renderizada dentro do método `RenderBody`.

Altere o conteúdo do elemento de título. Altere o [ActionLink](https://msdn.microsoft.com/library/dd504972(v=vs.108).aspx) no modelo de layout de &quot;nome do aplicativo&quot; para &quot;&quot; de filmes do MVC e o controlador de `Home` para `Movies`. O arquivo de layout completo é mostrado abaixo:

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=6,20)]

Execute o aplicativo e observe que agora ele diz &quot;&quot;de filme do MVC. Clique no link **sobre** , e você verá como essa página mostra &quot;&quot;de filme do MVC também. Conseguimos fazer a alteração uma vez no modelo de layout e fazer com que todas as páginas no site reflitam o novo título.

![](adding-a-view/_static/image8.png)

Quando criamos o arquivo *Views\HelloWorld\Index.cshtml* pela primeira vez, ele continha o seguinte código:

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

O código do Razor acima está definindo explicitamente a página de layout. Examine as *exibições\\arquivo _ViewStart. cshtml* , que contém exatamente a mesma marcação Razor. As *[exibições\\arquivo _ViewStart. cshtml](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)* definem o layout comum que todas as exibições usarão, portanto, você pode comentar ou remover esse código do arquivo *Views\HelloWorld\Index.cshtml* .

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml?highlight=1-3)]

Use a propriedade `Layout` para definir outra exibição de layout ou defina-a como `null` para que nenhum arquivo de layout seja usado.

Agora, vamos alterar o título da exibição de índice.

Abra *MvcMovie\Views\HelloWorld\Index.cshtml*. Há dois locais para fazer uma alteração: primeiro, o texto que aparece no título do navegador e, em seguida, no cabeçalho secundário (o elemento `<h2>`). Você os tornará ligeiramente diferentes para que possa ver qual parte do código altera qual parte do aplicativo.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml?highlight=2,5)]

Para indicar o título HTML a ser exibido, o código acima define uma propriedade `Title` do objeto `ViewBag` (que está no modelo de exibição *index. cshtml* ). Observe que o modelo de layout ( *Views\Shared\\_Layout. cshtml* ) usa esse valor no elemento `<title>` como parte da seção `<head>` do HTML que modificamos anteriormente.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml?highlight=6)]

Usando essa abordagem de `ViewBag`, você pode facilmente passar outros parâmetros entre o modelo de exibição e o arquivo de layout.

Execute o aplicativo. Observe que o título do navegador, o cabeçalho primário e os títulos secundários foram alterados. (Se as alterações não forem exibidas no navegador, talvez o conteúdo armazenado em cache esteja sendo exibido. Pressione CTRL + F5 em seu navegador para forçar a resposta do servidor a ser carregado.) O título do navegador é criado com o `ViewBag.Title` que definimos no modelo de exibição *index. cshtml* e o aplicativo &quot;-Movie adicional&quot; adicionado no arquivo de layout.

Observe também como o conteúdo no modelo de exibição *index. cshtml* foi mesclado com o modelo de exibição *\_layout. cshtml* e uma única resposta HTML foi enviada ao navegador. Os modelos de layout facilitam realmente a realização de alterações que se aplicam a todas as páginas do aplicativo.

![](adding-a-view/_static/image9.png)

Nosso pequeno &quot;de dados&quot; (nesse caso, a &quot;Olá de nosso modelo de exibição!&quot; mensagem) é embutida em código. O aplicativo MVC tem uma&quot; de &quot;V (exibição) e você tem um &quot;C&quot; (controlador), mas não há &quot;M&quot; (modelo) ainda. Em breve, veremos como criar um banco de dados e recuperar os dados do modelo.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passando dados do controlador para a exibição

Antes de passarmos para um banco de dados e falarmos sobre modelos, no entanto, vamos falar primeiro sobre como passar informações do controlador para uma exibição. As classes de controlador são invocadas em resposta a uma solicitação de URL de entrada. Uma classe de controlador é onde você escreve o código que manipula as solicitações de entrada do navegador, recupera dados de um banco de dado e, por fim, decide que tipo de resposta enviar de volta ao navegador. Os modelos de exibição podem ser usados de um controlador para gerar e formatar uma resposta HTML para o navegador.

Os controladores são responsáveis por fornecer quaisquer dados ou objetos necessários para que um modelo de exibição processe uma resposta para o navegador. Uma prática recomendada: **um modelo de exibição nunca deve executar uma lógica de negócios ou interagir diretamente com um banco de dados**. Em vez disso, um modelo de exibição deve funcionar apenas com os dados fornecidos a ele pelo controlador. Manter essa &quot;separação de preocupações&quot; ajuda a manter a limpeza, o teste e a manutenção de seu código.

Atualmente, o método de ação `Welcome` na classe `HelloWorldController` usa um `name` e um parâmetro `numTimes` e, em seguida, gera os valores diretamente para o navegador. Em vez de fazer com que o controlador processe essa resposta como uma cadeia de caracteres, vamos alterar o controlador para usar um modelo de exibição. O modelo de exibição gerará uma resposta dinâmica, o que significa que você precisa passar bits de dados apropriados do controlador para a exibição para gerar a resposta. Você pode fazer isso fazendo com que o controlador Coloque os dados dinâmicos (parâmetros) que o modelo de exibição precisa em um `ViewBag` objeto que o modelo de exibição pode acessar.

Retorne ao arquivo *HelloWorldController.cs* e altere o método `Welcome` para adicionar um `Message` e `NumTimes` valor ao objeto `ViewBag`. `ViewBag` é um objeto dinâmico, o que significa que você pode colocar o que desejar nele; o objeto `ViewBag` não tem propriedades definidas até que você coloque algo dentro dele. O [sistema de associação de modelo MVC do ASP.net](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mapeia automaticamente os parâmetros nomeados (`name` e `numTimes`) da cadeia de caracteres de consulta na barra de endereços para parâmetros em seu método. O arquivo *HelloWorldController.cs* completo tem esta aparência:

[!code-csharp[Main](adding-a-view/samples/sample8.cs)]

Agora, o objeto `ViewBag` contém dados que serão passados para a exibição automaticamente. Em seguida, você precisa de um modelo de exibição de boas-vindas! No menu **Compilar** , selecione **Compilar solução** (ou Ctrl + Shift + B) para verificar se o projeto foi compilado. Clique com o botão direito do mouse na pasta *Views\HelloWorld* e clique em **Adicionar**e, em seguida, clique em **exibição MVC 5 página com layout (Razor)** .
  
![](adding-a-view/_static/image10.png)   
  
Na caixa de diálogo **especificar nome para o item** , digite *Bem-vindo*e clique em **OK**.   
  
Na caixa de diálogo **selecionar uma página de layout** , aceite o padrão **\_layout. cshtml** e clique em **OK**.  
  
![](adding-a-view/_static/image11.png)   

O arquivo *MvcMovie\Views\HelloWorld\Welcome.cshtml* é criado.

Substitua a marcação no arquivo *Welcome. cshtml* . Você criará um loop que diz &quot;Olá&quot; quantas vezes o usuário disser que deveria. O arquivo *Welcome. cshtml* completo é mostrado abaixo.

[!code-cshtml[Main](adding-a-view/samples/sample9.cshtml)]

Execute o aplicativo e navegue até a seguinte URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Agora, os dados são extraídos da URL e passados para o controlador usando o [associador de modelo](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). O controlador empacota os dados em um objeto `ViewBag` e passa esse objeto para a exibição. A exibição, em seguida, exibe os dados como HTML para o usuário.

![](adding-a-view/_static/image12.png)

No exemplo acima, usamos um objeto `ViewBag` para passar dados do controlador para uma exibição. Mais adiante no tutorial, usaremos um modelo de exibição para passar dados de um controlador para uma exibição. A abordagem do modelo de exibição para passar dados é geralmente muito preferida sobre a abordagem do recipiente de exibição. Confira a entrada de blog [Dynamic V](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) com rigidez de tipos para obter mais informações. 

Bem, isso era um tipo de &quot;M&quot; para o modelo, mas não o tipo de banco de dados. Vamos ver o que aprendemos e criar um banco de dados de filmes.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Próximo](adding-a-model.md)
