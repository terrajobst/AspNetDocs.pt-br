---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Parte 3: exibições e ViewModels | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 3 aborda exibições e ViewModels.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3fcfc816cde22c697a78bab2c9ea7ace1bf68501
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78559804"
---
# <a name="part-3-views-and-viewmodels"></a>Parte 3: exibições e ViewModels

por [Jon Galloway](https://github.com/jongalloway)

> A loja de música MVC é um aplicativo de tutorial que apresenta e explica passo a passo como usar o ASP.NET MVC e o Visual Studio para desenvolvimento para a Web.  
>   
> A loja de música MVC é uma implementação de armazenamento de exemplo leve que vende os álbuns de música online e implementa a administração básica de site, a entrada do usuário e a funcionalidade do carrinho de compras.  
>   
> Esta série de tutoriais detalha todas as etapas usadas para criar o aplicativo de exemplo de loja de música MVC do ASP.NET. A parte 3 aborda exibições e ViewModels.

Até agora, acabamos retornando cadeias de caracteres de ações do controlador. Essa é uma boa maneira de ter uma ideia de como os controladores funcionam, mas não é como você desejaria criar um aplicativo Web real. Vamos querer uma maneira melhor de gerar o HTML de volta para os navegadores visitando nosso site – um em que podemos usar arquivos de modelo para personalizar mais facilmente o envio de conteúdo HTML. Isso é exatamente o que as exibições fazem.

## <a name="adding-a-view-template"></a>Adicionando um modelo de exibição

Para usar um View-template, vamos alterar o método de índice HomeController para retornar um ActionResult e fazer com que ele retorne View (), como abaixo:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

A alteração acima indica que, em vez de retornar uma cadeia de caracteres, queremos usar uma "exibição" para gerar um resultado.

Agora vamos adicionar um modelo de exibição apropriado ao nosso projeto. Para fazer isso, vamos posicionar o cursor de texto dentro do método de ação de índice, clicar com o botão direito do mouse e selecionar "Add View". Isso abrirá a caixa de diálogo Adicionar exibição:

![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)

A caixa de diálogo "Adicionar exibição" nos permite gerar com rapidez e facilidade arquivos de modelo de exibição. Por padrão, a caixa de diálogo "Adicionar exibição" popula previamente o nome do modelo de exibição a ser criado para que ele corresponda ao método de ação que o usará. Como usamos o menu de contexto "Add View" dentro do método de ação index () de nosso HomeController, a caixa de diálogo "Add View" acima tem "index" como o nome de exibição preenchido previamente por padrão. Não precisamos alterar nenhuma das opções nessa caixa de diálogo, portanto, clique no botão Adicionar.

Quando clicamos no botão Adicionar, o Visual Web Developer criará um novo modelo de exibição index. cshtml para nós no diretório \Views\Home, criando a pasta se ela ainda não existir.

![](mvc-music-store-part-3/_static/image2.png)

O nome e o local da pasta do arquivo "index. cshtml" são importantes e seguem as convenções de nomenclatura ASP.NET MVC padrão. O nome do diretório, \Views\Home, corresponde ao controlador, que é chamado de HomeController. O nome do modelo de exibição, índice, corresponde ao método de ação do controlador que exibirá a exibição.

O ASP.NET MVC nos permite evitar ter que especificar explicitamente o nome ou o local de um modelo de exibição quando usamos essa Convenção de nomenclatura para retornar uma exibição. Por padrão, ele renderizará o modelo de exibição \Views\Home\Index.cshtml quando escrevermos código como abaixo em nosso HomeController:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

O Visual Web Developer criou e abriu o modelo de exibição "index. cshtml" depois que clicamos no botão "Adicionar" na caixa de diálogo "Adicionar exibição". O conteúdo de index. cshtml é mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Essa exibição está usando o sintaxe Razor, que é mais conciso do que o mecanismo de exibição Web Forms usado em ASP.NET Web Forms e versões anteriores do ASP.NET MVC. O mecanismo de exibição de Web Forms ainda está disponível no ASP.NET MVC 3, mas muitos desenvolvedores acham que o mecanismo de exibição do Razor se adapta muito bem ao desenvolvimento do MVC do ASP.NET.

As três primeiras linhas definem o título da página usando ViewBag. Title. Vamos examinar como isso funciona em mais detalhes em breve, mas primeiro vamos atualizar o texto do cabeçalho do texto e exibir a página. Atualize a marca &lt;H2&gt; para dizer "esta é a Home Page", conforme mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

A execução do aplicativo mostra que nosso novo texto está visível na home page.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Usando um layout para elementos de site comuns

A maioria dos sites tem conteúdo que é compartilhado entre muitas páginas: navegação, rodapés, imagens de logotipo, referências de folha de estilos, etc. O mecanismo de exibição do Razor facilita o gerenciamento usando uma página chamada \_layout. cshtml, que foi criada automaticamente para nós dentro da pasta/Views/Shared.

![](mvc-music-store-part-3/_static/image4.png)

Clique duas vezes nessa pasta para exibir o conteúdo, que é mostrado abaixo.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

O conteúdo de nossos modos de exibição individuais será exibido pelo comando @RenderBody() e qualquer conteúdo comum que desejarmos aparecer fora dele poderá ser adicionado à marcação \_layout. cshtml. Vamos querer que nossa loja de música MVC tenha um cabeçalho comum com links para nossa home page e área de armazenamento em todas as páginas do site, então vamos adicioná-la ao modelo diretamente acima dessa instrução @RenderBody().

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Atualizando a folha de estilos

O modelo de projeto vazio inclui um arquivo CSS muito simplificado que inclui apenas estilos usados para exibir mensagens de validação. Nosso designer forneceu algumas CSS e imagens adicionais para definir a aparência para nosso site, portanto, vamos adicioná-las agora.

O arquivo CSS atualizado e as imagens são incluídos no diretório de conteúdo do MvcMusicStore-Assets. zip que está disponível em [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store). Vamos selecionar ambos no Windows Explorer e soltá-los na pasta de conteúdo da nossa solução no Visual Web Developer, conforme mostrado abaixo:

![](mvc-music-store-part-3/_static/image5.png)

Você será solicitado a confirmar se deseja substituir o arquivo site. css existente. Clique em Sim.

![](mvc-music-store-part-3/_static/image6.png)

A pasta de conteúdo do seu aplicativo agora será exibida da seguinte maneira:

![](mvc-music-store-part-3/_static/image7.png)

Agora, vamos executar o aplicativo e ver como nossas alterações são examinadas na Home Page.

![](mvc-music-store-part-3/_static/image8.png)

- Vamos examinar o que mudou: o método de ação de índice do HomeController encontrou e exibiu o modelo \Views\Home\Index.cshtmlView, embora nosso código tenha chamado "Return View ()", porque nosso modelo de exibição seguiu a Convenção de nomenclatura padrão.
- A Home Page está exibindo uma mensagem de boas-vindas simples que é definida no modelo de exibição \Views\Home\Index.cshtml.
- A Home Page está usando nosso modelo \_layout. cshtml e, portanto, a mensagem de boas-vindas está contida no layout HTML do site padrão.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Usando um modelo para passar informações para nossa exibição

Um modelo de exibição que apenas exibe HTML embutido em código não fará um site muito interessante. Para criar um site dinâmico, vamos querer passar informações de nossas ações do controlador para nossos modelos de exibição.

No padrão Model-View-Controller, o termo Model refere-se a objetos que representam os dados no aplicativo. Geralmente, objetos de modelo correspondem a tabelas em seu banco de dados, mas não precisam.

Os métodos de ação do controlador que retornam um ActionResult podem passar um objeto de modelo para a exibição. Isso permite que um controlador empacote corretamente todas as informações necessárias para gerar uma resposta e, em seguida, passe essas informações para um modelo de exibição a ser usado para gerar a resposta HTML apropriada. Isso é mais fácil de entender ao vê-lo em ação, então vamos começar.

Primeiro, criaremos algumas classes de modelo para representar gêneros e álbuns em nossa loja. Vamos começar criando uma classe de gênero. Clique com o botão direito do mouse na pasta "modelos" em seu projeto, escolha a opção "Adicionar classe" e nomeie o arquivo como "Genre.cs".

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Em seguida, adicione uma propriedade de nome de cadeia de caracteres pública à classe que foi criada:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Observação: caso você esteja imaginando, a notação {Get; Set;} está usando o recurso C#de propriedades implementadas automaticamente. Isso nos dá os benefícios de uma propriedade sem exigir que declaremos um campo de apoio.*

Em seguida, siga as mesmas etapas para criar uma classe de álbum (chamada Album.cs) que tem um título e uma propriedade de Gênero:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Agora, podemos modificar o StoreController para usar modos de exibição que exibem informações dinâmicas de nosso modelo. Se-para fins de demonstração no momento, chamamos nossos álbuns com base na ID da solicitação, poderíamos exibir essas informações como na exibição abaixo.

![](mvc-music-store-part-3/_static/image10.png)

Vamos começar alterando a ação armazenar detalhes para que ela mostre as informações de um único álbum. Adicione uma instrução "using" à parte superior da classe **StoreControllers** para incluir o namespace MvcMusicStore. Models, portanto, não precisamos digitar MvcMusicStore. Models. Album toda vez que quisermos usar a classe Album. A seção "Usings" dessa classe agora deve aparecer como abaixo.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Em seguida, atualizaremos a ação do controlador de detalhes para que ela retorne um ActionResult em vez de uma cadeia de caracteres, como fizemos com o método de índice de HomeController.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Agora, podemos modificar a lógica para retornar um objeto de álbum para a exibição. Posteriormente neste tutorial, recuperaremos os dados de um banco de dado – mas, no momento, usaremos "dados fictícios" para começar.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Observação: se você não estiver familiarizado com C#o, poderá pressupor que o uso de var significa que nossa variável de álbum é de associação tardia. Isso não está correto – o C# compilador está usando a inferência de tipos com base no que estamos atribuindo à variável para determinar se o álbum é do tipo álbum e compilando a variável de álbum local como um tipo de álbum, portanto, obtemos a verificação de tempo de compilação e o suporte ao editor de código do Visual Studio.*

Agora, vamos criar um modelo de exibição que usa nosso álbum para gerar uma resposta HTML. Antes de fazermos isso, precisamos criar o projeto para que a caixa de diálogo Adicionar exibição saiba sobre nossa classe de álbum recém-criada. Você pode criar o projeto selecionando o item de menu Debug ⇨ Build MvcMusicStore (para crédito extra, você pode usar o atalho Ctrl-Shift-B para criar o projeto).

![](mvc-music-store-part-3/_static/image11.png)

Agora que configuramos nossas classes de suporte, estamos prontos para criar nosso modelo de exibição. Clique com o botão direito do mouse no método Details e selecione "Add View..." no menu de contexto.

![](mvc-music-store-part-3/_static/image12.png)

Vamos criar um novo modelo de exibição como fizemos antes com o HomeController. Como estamos criando a partir do StoreController, ele será gerado por padrão em um arquivo \Views\Store\Index.cshtml.

Ao contrário de antes, vamos marcar a caixa de seleção de exibição "criar um fortemente tipado". Em seguida, vamos selecionar nossa classe "Album" na lista suspensa "View Data-Class". Isso fará com que a caixa de diálogo "Adicionar exibição" Crie um modelo de exibição que espera que um objeto de álbum será passado para ele para uso.

![](mvc-music-store-part-3/_static/image13.png)

Quando clicamos no botão "Adicionar", nosso modelo de exibição \Views\Store\Details.cshtml será criado, contendo o código a seguir.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Observe a primeira linha, que indica que essa exibição é fortemente tipada para nossa classe de álbum. O mecanismo de exibição do Razor entende que passou um objeto de álbum, portanto, podemos acessar facilmente as propriedades do modelo e até mesmo ter o benefício do IntelliSense no editor do Visual Web Developer.

Atualize a marca &lt;H2&gt; para que ela exiba a propriedade Title do álbum, modificando essa linha para que ela apareça da seguinte maneira.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Observe que o IntelliSense é disparado quando você insere o período após a palavra-chave @Model, mostrando as propriedades e os métodos aos quais a classe de álbum dá suporte.

Agora, vamos executar novamente o nosso projeto e visitar a URL do/Store/Details/5. Veremos os detalhes de um álbum como abaixo.

![](mvc-music-store-part-3/_static/image14.png)

Agora, vamos fazer uma atualização semelhante para o método de ação da loja de procura. Atualize o método para que ele retorne um ActionResult e modifique a lógica do método para que ele crie um novo objeto de gênero e o retorne ao modo de exibição.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Clique com o botão direito do mouse no método procurar e selecione "Adicionar exibição..." no menu de contexto, adicione uma exibição fortemente tipada adicione um tipo fortemente tipado à classe gênero.

![](mvc-music-store-part-3/_static/image15.png)

Atualize o elemento &lt;H2&gt; no código de exibição (em/Views/Store/Browse.cshtml) para exibir as informações de gênero.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Agora, vamos executar novamente o nosso projeto e navegar até o/Store/Browse? Gênero = URL de disco. Veremos a página de navegação exibida como abaixo.

![](mvc-music-store-part-3/_static/image16.png)

Por fim, vamos fazer uma atualização um pouco mais complexa para o método de ação de **índice de loja** e exibir para exibir uma lista de todos os gêneros em nossa loja. Faremos isso usando uma lista de gêneros como nosso objeto de modelo, em vez de apenas um único gênero.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Clique com o botão direito do mouse no método de ação armazenar índice e selecione Adicionar exibição como antes, selecione gênero como a classe modelo e pressione o botão Adicionar.

![](mvc-music-store-part-3/_static/image17.png)

Primeiro, alteraremos a declaração de @model para indicar que a exibição estará esperando vários objetos gênero em vez de apenas um. Altere a primeira linha de/Store/Index.cshtml para ler da seguinte maneira:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Isso informa ao mecanismo de exibição do Razor que ele estará trabalhando com um objeto de modelo que pode conter vários objetos gênero. Estamos usando um gênero IEnumerable&lt;&gt; em vez de uma lista&lt;gênero&gt; porque é mais genérica, permitindo alterar nosso tipo de modelo posteriormente para qualquer tipo de objeto que dê suporte à interface IEnumerable.

Em seguida, vamos executar um loop nos objetos gênero no modelo, conforme mostrado no código de exibição concluído abaixo.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Observe que temos suporte total ao IntelliSense enquanto inserimos esse código, para que, quando digitarmos "@Model". vemos todos os métodos e propriedades com suporte de um IEnumerable do tipo gênero.

![](mvc-music-store-part-3/_static/image18.png)

Em nosso loop "foreach", o Visual Web Developer sabe que cada item é do tipo gênero, portanto, vemos o IntelliSense para cada tipo de gênero.

![](mvc-music-store-part-3/_static/image19.png)

Em seguida, o recurso scaffolding examinou o objeto gênero e determinou que cada um terá uma propriedade Name, portanto, ele faz o loop e os grava. Ele também gera links de edição, detalhes e exclusão para cada item individual. Aproveitaremos isso mais tarde em nosso Store Manager, mas, por ora, gostaríamos de ter uma lista simples em vez disso.

Quando executamos o aplicativo e navegamos até/Store, vemos que a contagem e a lista de gêneros são exibidas.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Adicionando links entre páginas

Nossa URL/Store que lista os gêneros atualmente lista os nomes de gênero simplesmente como texto sem formatação. Vamos alterar isso para que, em vez de texto sem formatação, tenhamos o link nomes de gênero para a URL/Store/Browse apropriada, de forma que clicar em um gênero musical como "disco" navegará para a URL/Store/Browse? gênero = disco. Poderíamos atualizar nosso modelo de exibição \Views\Store\Index.cshtml para gerar esses links usando um código como abaixo **(não digite isso em – vamos melhorar isso)** :

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Isso funciona, mas poderia causar problemas mais tarde, pois depende de uma cadeia de caracteres codificada. Por exemplo, se quiséssemos renomear o controlador, precisaremos Pesquisar nosso código procurando links que precisam ser atualizados.

Uma abordagem alternativa que podemos usar é aproveitar um método auxiliar HTML. O ASP.NET MVC inclui métodos auxiliares HTML que estão disponíveis em nosso código de modelo de exibição para executar uma variedade de tarefas comuns, assim como essa. O método auxiliar HTML. ActionLink () é especialmente útil e facilita a criação de HTML &lt;um&gt; links e cuida de detalhes incômodos, como garantir que os caminhos de URL sejam codificados corretamente em URL.

HTML. ActionLink () tem várias sobrecargas diferentes para permitir a especificação de quantas informações forem necessárias para seus links. No caso mais simples, você fornecerá apenas o texto do link e o método de ação para ir quando o hiperlink for clicado no cliente. Por exemplo, podemos vincular ao método "/Store/" index () na página de detalhes do repositório com o texto do link "go to the Store index" usando a seguinte chamada:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Observação: nesse caso, não precisamos especificar o nome do controlador porque estamos apenas vinculando a outra ação dentro do mesmo controlador que está renderizando a exibição atual.*

Os links para a página de navegação precisarão passar um parâmetro, mas, portanto, usaremos outra sobrecarga do método html. ActionLink que usa três parâmetros:

- 1. Texto do link, que exibirá o nome do gênero
- 2. Nome da ação do controlador (procurar)
- 3. Rotear valores de parâmetro, especificando o nome (gênero) e o valor (nome do gênero)

Colocando tudo isso, veja como escreveremos esses links para a exibição do índice de loja:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Agora, quando executamos nosso projeto novamente e acessamos a URL/Store/, veremos uma lista de gêneros. Cada gênero é um hiperlink – quando clicado, ele nos levará à nossa URL/Store/Browse? gênero = *[gênero]* .

![](mvc-music-store-part-3/_static/image3.jpg)

O HTML da lista de gêneros é semelhante a este:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-2.md)
> [Próximo](mvc-music-store-part-4.md)
