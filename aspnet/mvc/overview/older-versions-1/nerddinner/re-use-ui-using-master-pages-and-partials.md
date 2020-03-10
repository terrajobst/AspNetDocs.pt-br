---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: Usar novamente a interface do usuário usando páginas mestras e parciais | Microsoft Docs
author: microsoft
description: A etapa 7 analisa as maneiras pelas quais podemos aplicar o ' princípio seco ' em nossos modelos de exibição para eliminar a duplicação de código, usando modelos de exibição parcial e páginas mestras.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: 0b17cb6ac14b7f187bf1f175097a37907689d46e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78580328"
---
# <a name="re-use-ui-using-master-pages-and-partials"></a>Reutilizar a interface do usuário usando páginas mestras e parciais

pela [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Esta é a etapa 7 de um [tutorial de aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito que percorre como criar um aplicativo Web pequeno, mas completo usando o ASP.NET MVC 1.
> 
> A etapa 7 analisa as maneiras pelas quais podemos aplicar o "princípio seco" em nossos modelos de exibição para eliminar a duplicação de código, usando modelos de exibição parcial e páginas mestras.
> 
> Se você estiver usando o ASP.NET MVC 3, recomendamos seguir as [introdução com](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) os tutoriais da [loja de música](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou MVC.

## <a name="nerddinner-step-7-partials-and-master-pages"></a>Etapa 7 do NerdDinner: páginas parciais e mestras

Uma das filosofias de design que o MVC ASP.NET adota é o princípio "não se repetir" (normalmente chamado de "seca"). Um design seco ajuda a eliminar a duplicação do código e da lógica, o que, por fim, torna os aplicativos mais rápidos para serem criados e mais fáceis de manter.

Já vimos o princípio seco aplicado em vários dos nossos cenários de NerdDinner. Alguns exemplos: nossa lógica de validação é implementada em nossa camada de modelo, que permite que ela seja imposta nos cenários de edição e criação em nosso controlador; Estamos usando novamente o modelo de exibição "não encontrado" nos métodos de ação de edição, detalhes e exclusão; Estamos usando um padrão de nomenclatura de convenção com nossos modelos de exibição, o que elimina a necessidade de especificar explicitamente o nome quando chamamos o método auxiliar View (); e estamos reutilizando a classe DinnerFormViewModel para os cenários de ação editar e criar.

Agora, vamos examinar como podemos aplicar o "princípio seco" em nossos modelos de exibição para eliminar a duplicação de código também.

### <a name="re-visiting-our-edit-and-create-view-templates"></a>Visitando novamente nossos modelos de edição e criação de exibição

No momento, estamos usando dois modelos de exibição diferentes – "Edit. aspx" e "Create. aspx" – para exibir nossa interface do usuário do formulário de jantar. Uma rápida comparação visual deles realça como eles são semelhantes. Abaixo está a aparência do formulário de criação:

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

E aqui está a aparência de nosso formulário de "edição":

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

Não é muito uma diferença? Além do título e do texto do cabeçalho, o layout do formulário e os controles de entrada são idênticos.

Se abrirmos os modelos de exibição "Edit. aspx" e "Create. aspx", descobriremos que eles contêm o layout de formulário idêntico e o código de controle de entrada. Essa duplicação significa que acabamos tendo que fazer alterações duas vezes a qualquer momento que introduzirmos ou alterarmos uma nova propriedade de jantar, o que não é bom.

### <a name="using-partial-view-templates"></a>Usando modelos de exibição parcial

O ASP.NET MVC dá suporte à capacidade de definir modelos de "exibição parcial" que podem ser usados para encapsular a lógica de renderização de exibição para uma subparte de uma página. "Parciais" fornecem uma maneira útil de definir a lógica de renderização de exibição uma vez e, em seguida, reutilizá-la em vários lugares em um aplicativo.

Para ajudar a "secar" nossa duplicação de modelo de exibição Edit. aspx e Create. aspx, podemos criar um modelo de exibição parcial chamado "DinnerForm. ascx" que encapsula o layout do formulário e os elementos de entrada comuns a ambos. Faremos isso clicando com o botão direito do mouse em nosso diretório/Views/Dinners e escolhendo o comando de menu "Add-&gt;View":

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

Isso exibirá a caixa de diálogo "Adicionar exibição". Vamos nomear o novo modo de exibição que desejamos criar "DinnerForm", selecionar a caixa de seleção "criar uma exibição parcial" dentro da caixa de diálogo e indicar que passaremos a ela uma classe DinnerFormViewModel:

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

Quando clicamos no botão "Add" (Adicionar), o Visual Studio criará um novo modelo de exibição "DinnerForm. ascx" para nós no diretório "\Views\Dinners".

Em seguida, podemos copiar/colar o código de controle de entrada/layout de formulário duplicado de nossos modelos de exibição Edit. aspx/Create. aspx em nosso novo modelo de exibição parcial "DinnerForm. ascx":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

Em seguida, podemos atualizar nossos modelos Edit e Create View para chamar o modelo parcial DinnerForm e eliminar a duplicação do formulário. Podemos fazer isso chamando HTML. RenderPartial ("DinnerForm") em nossos modelos de exibição:

##### <a name="createaspx"></a>Create.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a>Edit.aspx

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

Você pode qualificar explicitamente o caminho do modelo parcial que você deseja ao chamar HTML. RenderPartial (por exemplo: ~ views/JANTARS/DinnerForm. ascx "). No entanto, em nosso código acima, estamos aproveitando o padrão de nomenclatura baseado em Convenção dentro do ASP.NET MVC e apenas especificando "DinnerForm" como o nome do parcial a ser renderizado. Quando fazemos isso, o ASP.NET MVC parecerá primeiro no diretório de exibições baseadas em Convenção (para DinnersController, isso seria/Views/Dinners). Se ele não encontrar o modelo parcial, ele irá procurar no diretório/Views/Shared.

Quando HTML. RenderPartial () é chamado apenas com o nome do modo de exibição parcial, o ASP.NET MVC passará para a exibição parcial dos mesmos objetos Model e de dicionário ViewData usados pelo modelo de exibição de chamada. Como alternativa, há versões sobrecarregadas de HTML. RenderPartial () que permitem que você passe um objeto de modelo alternativo e/ou um dicionário ViewData para a exibição parcial a ser usada. Isso é útil para cenários em que você deseja apenas passar um subconjunto do modelo/ViewModel completo.

| **Tópico do lado: por que &lt;%%&gt; em vez de &lt;% =%&gt;?** |
| --- |
| Uma das coisas sutis que você pode ter percebido com o código acima é que estamos usando um bloco &lt;%%&gt; em vez de um bloco &lt;% =%&gt; ao chamar HTML. RenderPartial (). &lt;% =%&gt; blocos em ASP.NET indicam que um desenvolvedor deseja renderizar um valor especificado (por exemplo: &lt;% = "Olá"%&gt; renderia "Olá"). &lt;%%&gt; blocos, em vez disso, indicam que o desenvolvedor deseja executar o código e que qualquer saída renderizada dentro deles deve ser feita explicitamente (por exemplo: &lt;% Response. Write ("Olá")%&gt;. O motivo pelo qual estamos usando um bloco &lt;%%&gt; com nosso código HTML. RenderPartial acima é porque o método html. RenderPartial () não retorna uma cadeia de caracteres e, em vez disso, gera o conteúdo diretamente para o fluxo de saída do modelo de exibição de chamada. Ele faz isso por motivos de eficiência de desempenho e, ao fazer isso, evita a necessidade de criar um objeto de cadeia de caracteres temporário (potencialmente muito grande). Isso reduz o uso de memória e melhora a taxa de transferência geral do aplicativo. Um erro comum ao usar HTML. RenderPartial () é esquecer de adicionar um ponto-e-vírgula no final da chamada quando estiver dentro de um bloco &lt;%%&gt;. Por exemplo, esse código causará um erro do compilador: &lt;% HTML. RenderPartial ("DinnerForm")%&gt; você precisa escrever: &lt;% HTML. RenderPartial ("DinnerForm"); %&gt; isso ocorre porque &lt;%%&gt; blocos são instruções de código independentes e, ao usar C# instruções de código, precisam ser terminadas com um ponto-e-vírgula. |

### <a name="using-partial-view-templates-to-clarify-code"></a>Usando modelos de exibição parcial para esclarecer o código

Criamos o modelo de exibição parcial "DinnerForm" para evitar duplicar a lógica de renderização de exibição em vários locais. Esse é o motivo mais comum para criar modelos de exibição parcial.

Às vezes, ainda faz sentido criar exibições parciais mesmo quando elas estão sendo chamadas apenas em um único lugar. Modelos de exibição muito complicados geralmente podem se tornar muito mais fáceis de ler quando a lógica de renderização de exibição é extraída e particionada em um ou mais modelos parciais bem nomeados.

Por exemplo, considere o trecho de código abaixo do arquivo site. Master em nosso projeto (que iremos examinar em breve). O código é relativamente direto para leitura – parcialmente, porque a lógica para exibir um link de logon/logoff na parte superior direita da tela é encapsulada dentro do "LogOnUserControl" parcial:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

Sempre que você ficar confuso tentando entender a marcação de HTML/código dentro de um modelo de exibição, considere se não seria mais claro se alguns deles foram extraídos e refatorado em exibições parciais bem nomeadas.

### <a name="master-pages"></a>Páginas mestras

Além de dar suporte a exibições parciais, o ASP.NET MVC também dá suporte à capacidade de criar modelos de "página mestra" que podem ser usados para definir o layout comum e o HTML de nível superior de um site. Os controles de espaço reservado de conteúdo podem ser adicionados à página mestra para identificar regiões substituíveis que podem ser substituídas ou "preenchidas" por exibições. Isso fornece uma maneira muito eficaz (e seca) de aplicar um layout comum em um aplicativo.

Por padrão, novos projetos do ASP.NET MVC têm um modelo de página mestra automaticamente adicionado a eles. Essa página mestra é denominada "site. Master" e reside na pasta \Views\Shared\:

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

O arquivo site. master padrão é semelhante a este. Ele define o HTML externo do site, juntamente com um menu para navegação na parte superior. Ele contém dois controles de espaço reservado para conteúdo substituíveis – um para o título e o outro para onde o conteúdo primário de uma página deve ser substituído:

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

Todos os modelos de exibição que criamos para nosso aplicativo NerdDinner ("lista", "detalhes", "Editar", "criar", "não encontrado", etc) foram baseados neste site. modelo mestre. Isso é indicado por meio do atributo "MasterPagefile" que foi adicionado por padrão à parte superior &lt;% @ Page%&gt; diretiva quando criamos nossas exibições usando a caixa de diálogo "Adicionar exibição":

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

Isso significa que podemos alterar o conteúdo do site. Master e fazer com que as alterações sejam aplicadas e usadas automaticamente quando renderizarmos qualquer um de nossos modelos de exibição.

Vamos atualizar nossa seção de cabeçalho do site. master para que o cabeçalho de nosso aplicativo seja "NerdDinner" em vez de "meu aplicativo MVC". Vamos também atualizar nosso menu de navegação para que a primeira guia seja "encontrar um jantar" (manipulado pelo método de ação index () do HomeController) e vamos adicionar uma nova guia chamada "host a jantar" (manipulada pelo método de ação Create () de DinnersController):

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

Quando salvamos o arquivo site. Master e atualizamos nosso navegador, veremos que nossas alterações de cabeçalho aparecem em todas as exibições em nosso aplicativo. Por exemplo:

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

E com a URL de */Dinners/Edit/[ID]* :

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a>Próxima etapa

As páginas parciais e mestras fornecem opções muito flexíveis que permitem que você organize corretamente as exibições. Você descobrirá que eles ajudam a evitar duplicar conteúdo/código de exibição e facilitar a leitura e a manutenção de seus modelos de exibição.

Agora, Vamos revisitar o cenário de listagem que criamos anteriormente e habilitar o suporte de paginação escalonável.

> [!div class="step-by-step"]
> [Anterior](use-viewdata-and-implement-viewmodel-classes.md)
> [Próximo](implement-efficient-data-paging.md)
