---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: Introdução ao Páginas da Web do ASP.NET-Criando um layout consistente | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como usar layouts para criar uma aparência consistente para as páginas em um site que usa Páginas da Web do ASP.NET. Ele pressupõe que você concluiu o...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: 678eb7089e95e3d221d6b2d82034a62aefa75757
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526687"
---
# <a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a>Introdução ao Páginas da Web do ASP.NET-Criando um layout consistente

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como usar *layouts* para criar uma aparência consistente para as páginas em um site que usa páginas da Web do ASP.net. Ele pressupõe que você concluiu a série por meio da [exclusão de dados de banco de dado no páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=251584).
> 
> O que você aprenderá:
> 
> - O que é uma página de layout.
> - Como combinar páginas de layout com conteúdo dinâmico.
> - Como passar valores para uma página de layout.

## <a name="about-layouts"></a>Sobre layouts

As páginas que você criou até agora foram todas concluídas, páginas autônomas. Todos eles pertencem ao mesmo site, mas não têm nenhum elemento comum ou uma aparência padrão.

A maioria dos sites tem uma aparência e um layout consistentes. Por exemplo, se você for para o site do [Microsoft.com/Web](https://www.microsoft.com/web/) e observar, verá que as páginas são todas seguidas em um layout geral e em um tema Visual:

![Página do site do Microsoft.com/web mostrando o layout do cabeçalho, da área de navegação, da área de conteúdo e do rodapé](layouts/_static/image1.png)

Uma maneira *ineficiente* de criar esse layout seria definir um cabeçalho, uma barra de navegação e um rodapé separadamente em cada uma de suas páginas. Você estaria duplicando a mesma marcação a cada vez. Se você quisesse alterar algo (por exemplo, atualizar o rodapé), precisaria alterar cada página separadamente.

É aí que entram as *páginas de layout* . No Páginas da Web do ASP.NET, você pode definir uma página de layout que fornece um contêiner geral para páginas em seu site. Por exemplo, a página de layout pode conter o cabeçalho, a área de navegação e o rodapé. A página de layout inclui um espaço reservado onde o conteúdo principal vai.

Em seguida, você pode definir páginas de conteúdo individuais que contêm a marcação e o código somente para essa página. As páginas de conteúdo não precisam ser páginas HTML completas; Eles nem precisam ter um elemento `<body>`. Eles também têm uma linha de código que diz ao ASP.NET em qual página de layout você deseja exibir o conteúdo. Aqui está uma imagem que mostra aproximadamente como essa relação funciona:

![Diagrama conceitual que mostra duas páginas de conteúdo e uma página de layout na qual elas se ajustam](layouts/_static/image2.png)

Essa interação é fácil de entender quando você a vê em ação. Neste tutorial, você alterará suas páginas de filmes para usar um layout.

## <a name="adding-a-layout-page"></a>Adicionando uma página de layout

Você começará criando uma página de layout que define um layout de página típico com um cabeçalho, um rodapé e uma área para o conteúdo principal. No site do WebPagesMovies, adicione uma página CSHTML denominada *\_layout. cshtml*.

O caractere de sublinhado à esquerda (`_`) é significativo. Se o nome de uma página começar com um sublinhado, ASP.NET não enviará diretamente essa página para o navegador. Essa Convenção permite definir páginas que são necessárias para seu site, mas que os usuários não devem ser capazes de solicitar diretamente.

Substitua o conteúdo da página pelo seguinte:

[!code-html[Main](layouts/samples/sample1.html)]

Como você pode ver, essa marcação é apenas HTML que usa elementos `<div>` para definir três seções na página mais um elemento mais `<div>` para manter as três seções. O rodapé contém um pouco de código do Razor: `@DateTime.Now.Year`, que renderizará o ano atual nesse local na página.

Observe que há um link para uma folha de estilos chamada *Movies. css*. A folha de estilos é onde os detalhes do layout físico dos elementos serão definidos. Você criará isso daqui a pouco.

O único recurso incomum neste *\_página layout. cshtml* é a linha de `@Render.Body()`. Esse é o espaço reservado onde o conteúdo será usado quando esse layout for mesclado com outra página.

## <a name="adding-a-css-file"></a>Adicionando um arquivo. css

A maneira preferida de definir a organização real (ou seja, aparência) dos elementos na página é usar as regras de CSS (folha de estilos em cascata). Portanto, você criará um arquivo *. css* que tem as regras para o novo layout.

No WebMatrix, selecione a raiz do seu site. Na guia **arquivos** da faixa de faixas, clique na seta sob o botão **novo** e clique em **nova pasta**.

![A opção ' nova pasta ' em novo na faixa de opções.](layouts/_static/image3.png)

Nomeie os novos *estilos*de pasta.

![Nomeando a nova pasta ' Styles '](layouts/_static/image4.png)

Dentro da nova pasta *estilos* , crie um arquivo chamado *Movies. css*.

![Criando um novo arquivo Movies. css](layouts/_static/image5.png)

Substitua o conteúdo do novo arquivo *. css* pelo seguinte:

[!code-css[Main](layouts/samples/sample2.css)]

Não vamos dizer muito sobre essas regras de CSS, exceto para observar duas coisas. Uma delas é que, além de definir fontes e tamanhos, as regras usam o posicionamento absoluto para estabelecer o local do cabeçalho, do rodapé e da área de conteúdo principal. Se você for novo no posicionamento em CSS, poderá ler o tutorial de [posicionamento de CSS](http://www.w3schools.com/css/css_positioning.asp) no site do w3schools.

Outra coisa a ser observada é que, na parte inferior, copiamos as regras de estilo que foram originalmente definidas individualmente no arquivo *Movies. cshtml* . Essas regras foram usadas na [introdução à exibição de dados usando páginas da Web do ASP.net](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial para fazer com que o auxiliar de `WebGrid` processe a marcação que adicionou as faixas à tabela. (Se você for usar um arquivo *. css* para definições de estilo, você também poderá colocar as regras de estilo para todo o site nele.)

## <a name="updating-the-movies-file-to-use-the-layout"></a>Atualizando o arquivo de filmes para usar o layout

Agora você pode atualizar os arquivos existentes no seu site para usar o novo layout. Abra o arquivo *Movies. cshtml* . Na parte superior, como a primeira linha de código, adicione o seguinte:

[!code-csharp[Main](layouts/samples/sample3.cs)]

A página agora começa desta forma:

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

Essa linha de código informa ao ASP.NET que, quando a página de *filmes* é executada, ela deve ser mesclada com o arquivo *layout. cshtml de\_* .

Como o arquivo *Movies. cshtml* agora usa uma página de layout, você pode remover a marcação da página *Movies. cshtml* que é manipulada pelo arquivo *layout. cshtml de\_* . Retire as marcas `<!DOCTYPE>`, `<html>`e `<body>` de abertura e fechamento. Retire todo o elemento `<head>` e seu conteúdo, que inclui as regras de estilo para a grade, já que você tem essas regras em um arquivo *. css* . Enquanto estiver, altere o elemento `<h1>` existente para um elemento `<h2>`; Você já tem um elemento `<h1>` na página de layout. Altere o texto do `<h2>` para "listar filmes".

Normalmente, você não precisaria fazer esses tipos de alterações em uma página de conteúdo. Ao iniciar o site com uma página de layout, você cria páginas de conteúdo sem todos esses elementos para começar. Nesse caso, no entanto, você está convertendo uma página autônoma para uma que usa um layout, portanto, há um pouco de limpeza.

Quando tiver terminado, a página *Movies. cshtml* será parecida com a seguinte:

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a>Testando o layout

Agora você pode ver qual é a aparência do layout. No WebMatrix, clique com o botão direito do mouse na página *Movies. cshtml* e selecione **Iniciar no navegador**. Quando o navegador exibe a página, ele é semelhante a esta página:

![Página de filmes renderizada usando um layout](layouts/_static/image6.png)

ASP.NET mesclou o conteúdo da página Movies. cshtml na página *\_layout. cshtml* , onde o método `RenderBody` é. E, claro, a página *\_layout. cshtml* faz referência a um arquivo *. css* que define a aparência da página.

## <a name="updating-the-addmovie-page-to-use-the-layout"></a>Atualizando a página addmovie para usar o layout

O verdadeiro benefício dos layouts é que você pode usá-los para todas as páginas do seu site. Abra a página *addmovie. cshtml* .

Você pode se lembrar de que a página *addmovie. cshtml* tinha originalmente algumas regras de CSS nela para definir a aparência das mensagens de erro de validação. Como você tem um arquivo *. css* para seu site agora, você pode mover essas regras para o arquivo *. css* . Remova-os do arquivo *addmovie. cshtml* e adicione-os à parte inferior do arquivo *Movies. css* . Você está movendo as seguintes regras:

[!code-css[Main](layouts/samples/sample6.css)]

Agora faça os mesmos tipos de alterações em *addmovie. cshtml* que você fez para *Movies. cshtml* — adicione `Layout="~/_Layout.cshtml;` e remova a marcação HTML que agora é estranha. Altere o elemento `<h1>` para `<h2>`. Quando terminar, a página se parecerá com este exemplo:

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

Execute a página. Agora, ele é semelhante a esta ilustração:

![Página ' Adicionar filmes ' processada usando um layout](layouts/_static/image7.png)

Você deseja fazer alterações semelhantes às páginas no site — *EditMovie. cshtml* e *DeleteMovie. cshtml*. No entanto, antes de fazer isso, você pode fazer outra alteração no layout que o torna um pouco mais flexível.

## <a name="passing-title-information-to-the-layout-page"></a>Passando informações de título para a página de layout

A página *\_layout. cshtml* que você criou tem um elemento `<title>` definido como "meu site de filmes". A maioria dos navegadores exibe o conteúdo deste elemento como o texto em uma guia:

![O título da &lt;da página&gt; elemento exibido em uma guia do navegador](layouts/_static/image8.png)

Essa informação de título é genérica. Suponha que você deseja que o texto do título seja mais específico para a página atual. (O texto do título também é usado pelos mecanismos de pesquisa para determinar a sua página.) Você pode passar informações de uma página de conteúdo como *Movies. cshtml* ou *addmovie. cshtml* para a página de layout e, em seguida, usar essas informações para personalizar o que a página de layout renderiza.

Abra a página *Movies. cshtml* novamente. No código na parte superior, adicione a seguinte linha:

[!code-csharp[Main](layouts/samples/sample8.cs)]

O objeto `Page` está disponível em todas as páginas *. cshtml* e é para essa finalidade, ou seja, para compartilhar informações entre uma página e seu layout.

Abra a página *\_layout. cshtml* . Altere o elemento `<title>` de forma que ele se pareça com esta marcação:

[!code-html[Main](layouts/samples/sample9.html)]

Esse código renderiza o que estiver na propriedade `Page.Title` diretamente nesse local na página.

Execute a página *Movies. cshtml* . Desta vez, a guia Navegador mostra o que você passou como o valor de `Page.Title`:

![Uma guia do navegador mostrando o título criado dinamicamente](layouts/_static/image9.png)

Se desejar, exiba a origem da página no navegador. Você pode ver que o elemento `<title>` é renderizado como `<title>List Movies</title>`.

> [!TIP] 
> 
> **O objeto Page**
> 
> Um recurso útil do `Page` é que ele é um objeto dinâmico — a propriedade `Title` não é um nome fixo ou reservado. Você pode usar *qualquer* nome para um valor do objeto `Page`. Por exemplo, você poderia facilmente passar o título usando uma propriedade chamada `Page.CurrentName` ou `Page.MyPage`. A única restrição é que o nome precisa seguir as regras normais para quais propriedades podem ser nomeadas. (Por exemplo, o nome não pode conter um espaço).
> 
> Você pode passar qualquer número de valores usando o objeto `Page`. Se você quisesse passar informações de filme para a página de layout, poderá passar valores usando algo como `Page.MovieTitle` e `Page.Genre` e `Page.MovieYear`. (Ou quaisquer outros nomes que você inventou para armazenar as informações.) O único requisito, que provavelmente é óbvio, é que você precisa usar os mesmos nomes na página de conteúdo e na página de layout.
> 
> As informações que você passa usando o objeto `Page` não estão limitadas a apenas texto a ser exibido na página de layout. Você pode passar um valor para a página de layout e, em seguida, o código na página de layout pode usar o valor para decidir se deve exibir uma seção da página, o arquivo *. css* a ser usado e assim por diante. Os valores que você passa no objeto `Page` são como quaisquer outros valores que você usa no código. É apenas que os valores se originam na página de conteúdo e são passados para a página de layout.

Abra a página *addmovie. cshtml* e adicione uma linha à parte superior do código que fornece um título para a página *addmovie. cshtml* :

[!code-csharp[Main](layouts/samples/sample10.cs)]

Execute a página *addmovie. cshtml* . Você verá o novo título lá:

![Uma guia do navegador mostrando o título ' Adicionar filmes ' criado dinamicamente](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a>Atualizando as páginas restantes para usar o layout

Agora você pode concluir as páginas restantes no seu site para que elas usem o novo layout. Abra *EditMovie. cshtml* e *DeleteMovie. cshtml* por vez e faça as mesmas alterações em cada um.

Adicione a linha de código que vincula à página de layout:

[!code-csharp[Main](layouts/samples/sample11.cs)]

Adicione uma linha para definir o título da página:

[!code-csharp[Main](layouts/samples/sample12.cs)]

ou:

[!code-csharp[Main](layouts/samples/sample13.cs)]

Remover toda a marcação HTML incorreta — basicamente, deixe apenas os bits que estão dentro do elemento `<body>` (mais o bloco de código na parte superior).

Altere o elemento `<h1>` para que seja um elemento `<h2>`.

Quando você fez essas alterações, teste cada uma e verifique se ela está sendo exibida corretamente e se o título está correto.

## <a name="parting-thoughts-about-layout-pages"></a>Ideias de partes sobre páginas de layout

Neste tutorial, você criou uma página *\_layout. cshtml* e usou o método `RenderBody` para Mesclar conteúdo de outra página. Esse é o padrão básico para usar layouts em páginas da Web.

As páginas de layout têm recursos adicionais que não abordamos aqui. Por exemplo, você pode aninhar páginas de layout — uma página de layout pode, por sua vez, fazer referência a outra. Layouts aninhados podem ser úteis se você estiver trabalhando com subseções de um site que exigem layouts diferentes. Você também pode usar métodos adicionais (por exemplo, `RenderSection`) para configurar seções nomeadas na página layout.

A combinação de páginas de layout e arquivos *. css* é eficiente. Como você verá na próxima série de tutoriais, no WebMatrix, você pode criar um site com base em um *modelo*, que fornece um site com funcionalidade predefinida. Os modelos fazem uso bom das páginas de layout e do CSS para criar sites que têm uma aparência ótima e que têm recursos como menus. Aqui está uma captura de tela da home page de um site com base em um modelo, mostrando recursos que usam páginas de layout e CSS:

![Layout criado pelo modelo de site do WebMatrix mostrando o cabeçalho, área de navegação, área de conteúdo, seção opcional e links de logon](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a>Listagem completa da página de filme (atualizada para usar uma página de layout)

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a>Lista completa de páginas para adicionar filme (atualizado para layout)

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a>Concluir a listagem de página para excluir página de filme (atualizada para layout)

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a>Lista completa de páginas para editar filme (atualizado para layout)

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a>Chegando em seguida

No próximo tutorial, você aprenderá a publicar seu site na Internet para que todos possam vê-lo.

## <a name="additional-resources"></a>Recursos adicionais

- [Criando uma aparência consistente](https://go.microsoft.com/fwlink/?LinkID=202891) — um artigo que fornece mais detalhes sobre como trabalhar com layouts. Ele também descreve como passar um valor para uma página de layout que mostra ou oculta parte do conteúdo.
- [Páginas de layout aninhadas com Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind bloga um exemplo de como aninhar páginas de layout. (Inclui um download das páginas.)

> [!div class="step-by-step"]
> [Anterior](deleting-data.md)
> [Próximo](publishing.md)
