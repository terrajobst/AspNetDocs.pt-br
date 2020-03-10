---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Usar ViewData e implementar classes ViewModel | Microsoft Docs
author: microsoft
description: A etapa 6 mostra como habilitar o suporte para cenários de edição de formulário mais avançados e também discute duas abordagens que podem ser usadas para passar dados de controladores para exibições:...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: ca9775417c2e25952511a73096fb76d5d4edaea2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541604"
---
# <a name="use-viewdata-and-implement-viewmodel-classes"></a>Usar ViewData e implementar classes ViewModel

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 6 de um [tutorial de aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito que percorre como criar um aplicativo Web pequeno, mas completo usando o ASP.NET MVC 1.
> 
> A etapa 6 mostra como habilitar o suporte para cenários de edição de formulário mais avançados e também discute duas abordagens que podem ser usadas para passar dados de controladores para modos de exibição: ViewData e ViewModel.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos seguir as [introdução com](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) os tutoriais da [loja de música](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou MVC.

## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>Etapa 6 do NerdDinner: ViewData e ViewModel

Abordamos vários cenários de postagem de formulário e discutimos como implementar o suporte CRUD (Create, Update e Delete). Agora, vamos pegar nossa implementação de DinnersController e habilitar o suporte para cenários de edição de formulário mais avançados. Ao fazer isso, discutiremos duas abordagens que podem ser usadas para passar dados de controladores para modos de exibição: ViewData e ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Passando dados de controladores para exibição-modelos

Uma das características de definição do padrão MVC é a "separação de preocupações" estrita que ajuda a impor entre os diferentes componentes de um aplicativo. Os modelos, os controladores e os modos de exibição têm funções e responsabilidades bem definidas e se comunicam entre si de maneiras bem definidas. Isso ajuda a promover a capacidade de teste e a reutilização de código.

Quando uma classe de controlador decide renderizar uma resposta HTML de volta para um cliente, ela é responsável por passar explicitamente para o modelo de exibição todos os dados necessários para renderizar a resposta. Os modelos de exibição nunca devem executar nenhuma recuperação de dados ou lógica de aplicativo – e, em vez disso, devem limitar-se a apenas um código de renderização que é acionado do modelo/dados passados para ele pelo controlador.

Agora, os dados do modelo que estão sendo passados por nossa classe DinnersController para nossos modelos de exibição são simples e diretos – uma lista de objetos de jantar no caso de index () e um único objeto de jantar no caso de Details (), Edit (), Create () e Delete (). À medida que adicionamos mais recursos de interface do usuário ao nosso aplicativo, frequentemente vamos precisar passar mais do que apenas esses dados para renderizar respostas HTML em nossos modelos de exibição. Por exemplo, talvez queiramos alterar o campo "Country" em nosso Edit e criar exibições de serem uma caixa de texto HTML para uma DropDownList. Em vez de embutir em código a lista suspensa de nomes de países no modelo de exibição, talvez queiramos gerá-lo em uma lista de países com suporte que populamos dinamicamente. Precisaremos de uma maneira de passar o objeto de jantar *e* a lista de países com suporte de nosso controlador para nossos modelos de exibição.

Vejamos duas maneiras de fazer isso.

### <a name="using-the-viewdata-dictionary"></a>Usando o dicionário ViewData

A classe base do controlador expõe uma propriedade de dicionário "ViewData" que pode ser usada para passar itens de dados adicionais de controladores para exibições.

Por exemplo, para dar suporte ao cenário em que queremos alterar a caixa de texto "Country" em nosso modo de exibição de edição de ser uma caixa de texto HTML para uma DropDownList, podemos atualizar nosso método de ação Edit () para passar (além de um objeto de jantar) um objeto SelectList que pode ser usado como o m IDI de uma DropDownList de países.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

O construtor da lista de seleção acima está aceitando uma listagem de conversões para preencher o menu suspenso com, bem como o valor selecionado no momento.

Em seguida, podemos atualizar nosso modelo de exibição Edit. aspx para usar o método auxiliar HTML. DropDownList () em vez do método auxiliar HTML. TextBox () que usamos anteriormente:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

O método auxiliar HTML. DropDownList () acima usa dois parâmetros. O primeiro é o nome do elemento de formulário HTML para saída. O segundo é o modelo "SelectList" que passamos por meio do dicionário ViewData. Estamos usando a C# palavra-chave "as" para converter o tipo dentro do dicionário como uma lista de seleção.

E agora, quando executamos nosso aplicativo e acessamos a URL */Dinners/Edit/1* em nosso navegador, veremos que nossa interface do usuário de edição foi atualizada para exibir uma DropDownList dos países, em vez de uma caixa de texto:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Como também renderizamos o modelo de exibição de edição do método HTTP-POST Edit (em cenários quando ocorrem erros), queremos garantir que também atualizaremos esse método para adicionar alist de seleção a ViewData quando o modelo de exibição for renderizado em cenários de erro:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

E agora nosso cenário de edição DinnersController dá suporte a DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Usando um padrão ViewModel

A abordagem do dicionário ViewData tem a vantagem de ser muito rápida e fácil de implementar. No entanto, alguns desenvolvedores não gostam de usar dicionários baseados em cadeia de caracteres, pois as digitações podem levar a erros que não serão detectados em tempo de compilação. O dicionário ViewData não tipado também requer o uso do operador "as" ou a conversão ao usar uma linguagem fortemente tipada, como C# em um modelo de exibição.

Uma abordagem alternativa que poderíamos usar é, com freqüência, conhecida como o padrão "ViewModel". Ao usar esse padrão, criamos classes fortemente tipadas que são otimizadas para nossos cenários de exibição específicos e que expõem propriedades para os valores dinâmicos/conteúdo exigidos por nossos modelos de exibição. Nossas classes de controlador podem então popular e passar essas classes otimizadas para exibição para nosso modelo de exibição a ser usado. Isso habilita a segurança de tipo, a verificação de tempo de compilação e o IntelliSense do editor em modelos de exibição.

Por exemplo, para habilitar cenários de edição de formulário de jantar, podemos criar uma classe "DinnerFormViewModel" como a mostrada abaixo, que expõe duas propriedades fortemente tipadas: um objeto de jantar e o modelo SelectList necessário para preencher os países DropDownList:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Em seguida, podemos atualizar nosso método de ação Edit () para criar o DinnerFormViewModel usando o objeto de jantar que recuperamos de nosso repositório e, em seguida, passá-lo para nosso modelo de exibição:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Em seguida, atualizaremos nosso modelo de exibição para que ele espere um "DinnerFormViewModel" em vez de um objeto "jantar", alterando o atributo "Inherits" na parte superior da página Edit. aspx da seguinte forma:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Depois de fazer isso, o IntelliSense da propriedade "Model" em nosso modelo de exibição será atualizado para refletir o modelo de objeto do tipo DinnerFormViewModel que está passando:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Em seguida, podemos atualizar nosso código de exibição para que ele funcione. Observe abaixo de como não estamos alterando os nomes dos elementos de entrada que estamos criando (os elementos de formulário ainda serão nomeados "title", "Country") – mas estamos atualizando os métodos auxiliares HTML para recuperar os valores usando a classe DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Também atualizaremos nosso método Edit post para usar a classe DinnerFormViewModel ao renderizar erros:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Também podemos atualizar nossos métodos de ação Create () para reutilizar exatamente a mesma classe *DinnerFormViewModel* para habilitar os países DropDownList dentro deles. Abaixo está a implementação de HTTP-GET:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Abaixo está a implementação do método HTTP-POST Create:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

E agora as duas telas editar e criar dão suporte a drop-downlists para escolher o país.

### <a name="custom-shaped-viewmodel-classes"></a>Classes ViewModel com formato personalizado

No cenário acima, nossa classe DinnerFormViewModel expõe diretamente o objeto de modelo de jantar como uma propriedade, juntamente com uma propriedade de modelo de seleção de suporte. Essa abordagem funciona bem para cenários em que a interface de usuário HTML que queremos criar em nosso modelo de exibição corresponde relativamente ao nosso objeto de modelo de domínio.

Para cenários em que esse não é o caso, uma opção que você pode usar é criar uma classe ViewModel de formato personalizado cujo modelo de objeto seja mais otimizado para consumo pela exibição – e que pode parecer completamente diferente do objeto de modelo de domínio subjacente. Por exemplo, ele poderia expor potencialmente diferentes nomes de propriedade e/ou propriedades de agregação coletadas de vários objetos de modelo.

Classes ViewModel com formato personalizado podem ser usadas para passar dados de controladores para exibições a serem renderizadas, bem como para ajudar a lidar com os dados de formulário postados para o método de ação de um controlador. Para esse cenário posterior, você pode fazer com que o método de ação atualize um objeto ViewModel com os dados postados pelo formulário e, em seguida, use a instância ViewModel para mapear ou recuperar um objeto de modelo de domínio real.

As classes ViewModel de forma personalizada podem fornecer uma grande flexibilidade, e são algo a investigar sempre que você encontrar o código de renderização em seus modelos de exibição ou o código de postagem de formulário dentro dos seus métodos de ação, começando a ficar muito complicado. Isso geralmente é um sinal de que os modelos de domínio não correspondem corretamente à interface do usuário que você está gerando e que uma classe ViewModel em formato personalizado intermediário pode ajudar.

### <a name="next-step"></a>Próxima etapa

Agora, vamos examinar como podemos usar parciais e páginas mestras para reutilizar e compartilhar a interface do usuário em nosso aplicativo.

> [!div class="step-by-step"]
> [Anterior](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [Próximo](re-use-ui-using-master-pages-and-partials.md)
