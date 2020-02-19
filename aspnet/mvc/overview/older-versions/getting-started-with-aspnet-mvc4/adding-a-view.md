---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
title: Adicionando uma exibição | Microsoft Docs
author: Rick-Anderson
description: 'Observação: uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstrar...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: dde851d7-882e-4d99-9b96-cf96daed81cc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 81c2e1f46b08cbc9b5aa5d6c1b36d9d8dc2ba581
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457629"
---
# <a name="adding-a-view"></a>Adicionar uma exibição

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e o Visual Studio 2013. É mais seguro, muito mais simples de seguir e demonstra mais recursos.

Nesta seção, você modificará a classe `HelloWorldController` para usar os arquivos de modelo de exibição para encapsular corretamente o processo de geração de respostas em HTML para um cliente.

Você criará um arquivo de modelo de exibição usando o [mecanismo de exibição do Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introduzido com o ASP.NET MVC 3. Os modelos de exibição baseados em Razor têm uma extensão de arquivo *. cshtml* e fornecem uma maneira elegante de criar uma C#saída HTML usando o. O Razor minimiza o número de caracteres e pressionamentos de teclas necessários ao escrever um modelo de exibição e permite um fluxo de trabalho de codificação rápido e fluido.

Atualmente, o método `Index` retorna uma cadeia de caracteres com uma mensagem que é embutida em código na classe do controlador. Altere o método `Index` para retornar um objeto `View`, conforme mostrado no código a seguir:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

O método `Index` acima usa um modelo de exibição para gerar uma resposta HTML para o navegador. Os métodos do controlador (também conhecidos como [métodos de ação](http://rachelappel.com/asp.net-mvc-actionresults-explained)), como o método `Index` acima, geralmente retornam um [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx) (ou uma classe derivada de [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult.aspx)), não tipos primitivos como cadeia de caracteres.

No projeto, adicione um modelo de exibição que você pode usar com o método `Index`. Para fazer isso, clique com o botão direito do mouse dentro do método `Index` e clique em **Adicionar exibição**.

![](adding-a-view/_static/image1.png)

A caixa de diálogo **Adicionar exibição** é exibida. Deixe os padrões como estão e clique no botão **Adicionar** :

![](adding-a-view/_static/image2.png)

A pasta *MvcMovie\Views\HelloWorld* e o arquivo *MvcMovie\Views\HelloWorld\Index.cshtml* são criados. Você pode vê-los em **Gerenciador de soluções**:

![](adding-a-view/_static/image3.png)

O seguinte mostra o arquivo *index. cshtml* que foi criado:

![HelloWorldIndex](adding-a-view/_static/image4.png)

Adicione o HTML a seguir na marca de `<h2>`.

[!code-html[Main](adding-a-view/samples/sample2.html)]

O arquivo *MvcMovie\Views\HelloWorld\Index.cshtml* completo é mostrado abaixo.

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml?highlight=7-8)]

Se você estiver usando o Visual Studio 2012, no Gerenciador de soluções, clique com o botão direito do mouse no arquivo *index. cshtml* e selecione **Exibir em Inspetor de página**.

![PI](adding-a-view/_static/image5.png)

O [tutorial de Inspetor de página](../../views/using-page-inspector-in-aspnet-mvc.md) tem mais informações sobre essa nova ferramenta.

Como alternativa, execute o aplicativo e navegue até o controlador de `HelloWorld` (`http://localhost:xxxx/HelloWorld`). O método `Index` em seu controlador não fazia muito trabalho; Ele simplesmente executou a instrução `return View()`, que especificava que o método deve usar um arquivo de modelo de exibição para renderizar uma resposta para o navegador. Como você não especificou explicitamente o nome do arquivo de modelo de exibição a ser usado, o ASP.NET MVC assume o uso do arquivo de exibição *index. cshtml* na pasta *\Views\HelloWorld* A imagem abaixo mostra a cadeia de caracteres &quot;Olá de nosso modelo de exibição!&quot; embutido na exibição.

![](adding-a-view/_static/image6.png)

Parece muito bom. No entanto, observe que a barra de título do navegador mostra &quot;índice meu ASP.NET um&quot; e o link grande na parte superior da página diz &quot;seu logotipo aqui.&quot; abaixo do &quot;seu logotipo aqui.&quot; link são os links de registro e de logon, e abaixo deles links para página inicial, sobre e páginas de contato. Vamos alterar alguns deles.

## <a name="changing-views-and-layout-pages"></a>Alterando exibições e páginas de layout

Primeiro, você deseja alterar o &quot;seu logotipo aqui.&quot; título na parte superior da página. Esse texto é comum a cada página. Na verdade, ele é implementado em apenas um local no projeto, mesmo que ele apareça em cada página do aplicativo. Vá para a pasta */views/Shared* em **Gerenciador de soluções** e abra o arquivo *\_layout. cshtml* . Esse arquivo é chamado de *página de layout* e é o shell de &quot;compartilhado&quot; que todas as outras páginas usam.

![_LayoutCshtml](adding-a-view/_static/image7.png)

Os modelos de layout permitem especificar o layout do contêiner HTML do seu site em um único local e, em seguida, aplicá-lo em várias páginas do seu site. Localize a linha `@RenderBody()`. `RenderBody` é um espaço reservado em que todas as páginas específicas à exibição criadas são mostradas, &quot;encapsuladas&quot; na página de layout. Por exemplo, se você selecionar o link About, a exibição *Views\Home\About.cshtml* será renderizada dentro do método `RenderBody`.

Altere o título site-title no modelo de layout de &quot;seu logotipo aqui&quot; para &quot;&quot;de filmes do MVC.

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Substitua o conteúdo do elemento title pela seguinte marcação:

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Execute o aplicativo e observe que agora ele diz &quot;&quot;de filme do MVC. Clique no link **sobre** , e você verá como essa página mostra &quot;&quot;de filme do MVC também. Conseguimos fazer a alteração uma vez no modelo de layout e fazer com que todas as páginas no site reflitam o novo título.

![](adding-a-view/_static/image8.png)

Agora, vamos alterar o título da exibição de índice.

Abra *MvcMovie\Views\HelloWorld\Index.cshtml*. Há dois locais para fazer uma alteração: primeiro, o texto que aparece no título do navegador e, em seguida, no cabeçalho secundário (o elemento `<h2>`). Você os tornará ligeiramente diferentes para que possa ver qual parte do código altera qual parte do aplicativo.

[!code-cshtml[Main](adding-a-view/samples/sample6.cshtml)]

Para indicar o título HTML a ser exibido, o código acima define uma propriedade `Title` do objeto `ViewBag` (que está no modelo de exibição *index. cshtml* ). Se você olhar novamente o código-fonte do modelo de layout, observará que o modelo usa esse valor no elemento `<title>` como parte da seção `<head>` do HTML que modificamos anteriormente. Usando essa abordagem de `ViewBag`, você pode facilmente passar outros parâmetros entre o modelo de exibição e o arquivo de layout.

Execute o aplicativo e navegue até `http://localhost:xx/HelloWorld`. Observe que o título do navegador, o cabeçalho primário e os títulos secundários foram alterados. (Se as alterações não forem exibidas no navegador, talvez o conteúdo armazenado em cache esteja sendo exibido. Pressione CTRL + F5 em seu navegador para forçar a resposta do servidor a ser carregado.) O título do navegador é criado com o `ViewBag.Title` que definimos no modelo de exibição *index. cshtml* e o aplicativo &quot;-Movie adicional&quot; adicionado no arquivo de layout.

Observe também como o conteúdo no modelo de exibição *index. cshtml* foi mesclado com o modelo de exibição *\_layout. cshtml* e uma única resposta HTML foi enviada ao navegador. Os modelos de layout facilitam realmente a realização de alterações que se aplicam a todas as páginas do aplicativo.

![](adding-a-view/_static/image9.png)

Nosso pequeno &quot;de dados&quot; (nesse caso, a &quot;Olá de nosso modelo de exibição!&quot; mensagem) é embutida em código. O aplicativo MVC tem uma&quot; de &quot;V (exibição) e você tem um &quot;C&quot; (controlador), mas não há &quot;M&quot; (modelo) ainda. Em breve, veremos como criar um banco de dados e como recuperar de forma a sua base.

## <a name="passing-data-from-the-controller-to-the-view"></a>Passando dados do controlador para a exibição

Antes de passarmos para um banco de dados e falarmos sobre modelos, no entanto, vamos falar primeiro sobre como passar informações do controlador para uma exibição. As classes de controlador são invocadas em resposta a uma solicitação de URL de entrada. Uma classe de controlador é onde você escreve o código que manipula as solicitações de entrada do navegador, recupera dados de um banco de dado e, por fim, decide que tipo de resposta enviar de volta ao navegador. Os modelos de exibição podem ser usados de um controlador para gerar e formatar uma resposta HTML para o navegador.

Os controladores são responsáveis por fornecer quaisquer dados ou objetos necessários para que um modelo de exibição processe uma resposta para o navegador. Uma prática recomendada: **um modelo de exibição nunca deve executar uma lógica de negócios ou interagir diretamente com um banco de dados**. Em vez disso, um modelo de exibição deve funcionar apenas com os dados fornecidos a ele pelo controlador. Manter essa &quot;separação de preocupações&quot; ajuda a manter a limpeza, o teste e a manutenção de seu código.

Atualmente, o método de ação `Welcome` na classe `HelloWorldController` usa um `name` e um parâmetro `numTimes` e, em seguida, gera os valores diretamente para o navegador. Em vez de fazer com que o controlador processe essa resposta como uma cadeia de caracteres, vamos alterar o controlador para usar um modelo de exibição. O modelo de exibição gerará uma resposta dinâmica, o que significa que você precisa passar bits de dados apropriados do controlador para a exibição para gerar a resposta. Você pode fazer isso fazendo com que o controlador Coloque os dados dinâmicos (parâmetros) que o modelo de exibição precisa em um `ViewBag` objeto que o modelo de exibição pode acessar.

Retorne ao arquivo *HelloWorldController.cs* e altere o método `Welcome` para adicionar um `Message` e `NumTimes` valor ao objeto `ViewBag`. `ViewBag` é um objeto dinâmico, o que significa que você pode colocar o que desejar nele; o objeto `ViewBag` não tem propriedades definidas até que você coloque algo dentro dele. O [sistema de associação de modelo MVC do ASP.net](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) mapeia automaticamente os parâmetros nomeados (`name` e `numTimes`) da cadeia de caracteres de consulta na barra de endereços para parâmetros em seu método. O arquivo *HelloWorldController.cs* completo tem esta aparência:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Agora, o objeto `ViewBag` contém dados que serão passados para a exibição automaticamente.

Em seguida, você precisa de um modelo de exibição de boas-vindas! No menu **Compilar** , selecione **criar MvcMovie** para verificar se o projeto foi compilado.

Em seguida, clique com o botão direito do mouse dentro do método `Welcome` e clique em **Adicionar exibição**.

![](adding-a-view/_static/image10.png)

Esta é a aparência da caixa de diálogo **Adicionar exibição** :

![](adding-a-view/_static/image11.png)

Clique em **Adicionar**e, em seguida, adicione o código a seguir sob o elemento `<h2>` no novo arquivo *Welcome. cshtml* . Você criará um loop que diz &quot;Olá&quot; quantas vezes o usuário disser que deveria. O arquivo *Welcome. cshtml* completo é mostrado abaixo.

[!code-cshtml[Main](adding-a-view/samples/sample8.cshtml)]

Execute o aplicativo e navegue até a seguinte URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Agora, os dados são extraídos da URL e passados para o controlador usando o [associador de modelo](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx). O controlador empacota os dados em um objeto `ViewBag` e passa esse objeto para a exibição. A exibição, em seguida, exibe os dados como HTML para o usuário.

![](adding-a-view/_static/image12.png)

No exemplo acima, usamos um objeto `ViewBag` para passar dados do controlador para uma exibição. Em segundo, no tutorial, usaremos um modelo de exibição para passar dados de um controlador para um modo de exibição. A abordagem do modelo de exibição para passar dados é geralmente muito preferida sobre a abordagem do recipiente de exibição. Confira a entrada de blog [Dynamic V](https://blogs.msdn.com/b/rickandy/archive/2011/01/28/dynamic-v-strongly-typed-views.aspx) com rigidez de tipos para obter mais informações.

Bem, isso era um tipo de &quot;M&quot; para o modelo, mas não o tipo de banco de dados. Vamos ver o que aprendemos e criar um banco de dados de filmes.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Próximo](adding-a-model.md)
