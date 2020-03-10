---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Páginas mestras | Microsoft Docs
author: microsoft
description: Um dos principais componentes para um site bem-sucedido é uma aparência consistente. No ASP.NET 1. x, os desenvolvedores usaram controles de usuário para replicar a página comum Elem...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: 36f2caf7c2c9bcafd22c8f6681c1d6b19fe5078a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78567329"
---
# <a name="master-pages"></a>Páginas mestras

pela [Microsoft](https://github.com/microsoft)

> Um dos principais componentes para um site bem-sucedido é uma aparência consistente. No ASP.NET 1. x, os desenvolvedores usaram controles de usuário para replicar elementos de página comuns em um aplicativo Web. Embora essa seja certamente uma solução viável, o uso de controles de usuário tem algumas desvantagens. Por exemplo, uma alteração na posição de um controle de usuário requer uma alteração em várias páginas em um site. Os controles de usuário também não são renderizados em modo de exibição de Design depois de serem inseridos em uma página.

Um dos principais componentes para um site bem-sucedido é uma aparência consistente. No ASP.NET 1. x, os desenvolvedores usaram controles de usuário para replicar elementos de página comuns em um aplicativo Web. Embora essa seja certamente uma solução viável, o uso de controles de usuário tem algumas desvantagens. Por exemplo, uma alteração na posição de um controle de usuário requer uma alteração em várias páginas em um site. Os controles de usuário também não são renderizados em modo de exibição de Design depois de serem inseridos em uma página.

O ASP.NET 2,0 apresenta páginas mestras como uma maneira de manter uma aparência consistente e, como você verá em breve, as páginas mestras representam uma melhoria significativa sobre o método de controle de usuário.

## <a name="why-master-pages"></a>Por que as páginas mestras?

Você deve estar se perguntando por que as páginas mestras foram necessárias no ASP.NET 2,0. Afinal, os desenvolvedores de sites já estão usando controles de usuário no ASP.NET 1. x para compartilhar áreas de conteúdo entre páginas. Na verdade, há várias razões pelas quais os controles de usuário são uma solução menos do que a ideal para criar um layout comum.

Na verdade, os controles de usuário não definem o layout da página. Em vez disso, eles definem o layout e a funcionalidade de uma parte de uma página. A distinção entre esses dois é importante porque torna a capacidade de gerenciamento de uma solução de controle de usuário muito mais difícil. Por exemplo, quando você deseja alterar a posição de um controle de usuário em sua página, você deve editar a página real na qual o controle de usuário aparece. Bem, se você tiver apenas algumas páginas, mas em sites grandes, ele se tornará rapidamente um pesadelo de gerenciamento de sites!

Outra desvantagem de usar controles de usuário para definir um layout comum é enraizada na arquitetura do próprio ASP.NET. Se qualquer membro público de um controle de usuário for alterado, ele exigirá que você recompile todas as páginas que usam o controle de usuário. Por sua vez, o ASP.NET, em seguida, fará o JIT novamente em suas páginas quando elas forem acessadas pela primeira vez. Isso, mais uma vez, produz uma arquitetura não escalonável e um problema de gerenciamento de site para sites maiores.

Esses dois problemas (e muitos outros) são bem abordados por páginas mestras no ASP.NET 2,0.

## <a name="how-master-pages-work"></a>Como as páginas mestras funcionam

Uma página mestra é análoga a um modelo para outras páginas. Os elementos da página que devem ser compartilhados entre outras páginas (ou seja, menus, bordas, etc.) são adicionados à página mestra. Quando novas páginas são adicionadas ao site, você pode associá-las a uma página mestra. Uma página associada a uma página mestra é chamada de página de **conteúdo**. Por padrão, uma página de conteúdo assume a aparência da página mestra. No entanto, ao criar uma página mestra, você pode definir partes da página que a página de conteúdo pode substituir com seu próprio conteúdo. Essas partes são definidas usando um novo controle introduzido no ASP.NET 2,0; o controle **ContentPlaceHolder** .

Uma página mestra pode conter qualquer número de controles ContentPlaceHolder (ou nenhum). Na página conteúdo, o conteúdo dos controles ContentPlaceHolder aparece dentro dos controles de **conteúdo** , outro controle novo no ASP.NET 2,0. Por padrão, os controles de conteúdo de páginas de conteúdo estão vazios para que você possa fornecer seu próprio conteúdo. Se você quiser usar o conteúdo da página mestra dentro dos controles de conteúdo, poderá fazê-lo como verá posteriormente neste módulo. O controle de conteúdo é mapeado para o controle ContentPlaceHolder por meio do atributo ContentPlaceHolderid do controle de conteúdo. O código a seguir mapeia um controle de conteúdo para um controle ContentPlaceHolder chamado mainBody em uma página mestra.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Muitas vezes, você ouvirá que as pessoas descrevem as páginas mestras como sendo uma classe base para outras páginas. Isso não é verdade. A relação entre páginas mestras e páginas de conteúdo não é uma de herança.

A **Figura 1** mostra uma página mestra e uma página de conteúdo associada conforme aparecem no Visual Studio 2005. Você pode ver o controle ContentPlaceHolder na página mestra e o controle de conteúdo correspondente na página de conteúdo. Observe que o conteúdo das páginas mestras que está fora do ContentPlaceHolder está visível, mas esmaecido na página de conteúdo. Somente o conteúdo dentro do ContentPlaceHolder pode ser suplantadodo pela página de conteúdo. Todo o outro conteúdo proveniente da página mestra é imutável.

![Uma página mestra e sua página de conteúdo associada](master-pages/_static/image1.jpg)

**Figura 1**: uma página mestra e sua página de conteúdo associada

## <a name="creating-a-master-page"></a>Criando uma página mestra

Para criar uma nova página mestra:

1. Abra o Visual Studio 2005 e crie um novo site da Web.
2. Clique em arquivo, novo, arquivo.
3. Escolha arquivo mestre na caixa de diálogo Adicionar novo item, conforme mostrado na **Figura 2**.
4. Clique em Adicionar.

![Criando uma nova página mestra](master-pages/_static/image2.jpg)

**Figura 2**: criando uma nova página mestra

Observe que a extensão de arquivo de uma página mestra é *. Master*. Essa é uma das maneiras como uma página mestra difere de uma página comum. A outra diferença principal é que, no lugar de uma diretiva @Page, a página mestra contém uma diretiva @Master. Alterne para o modo de exibição de origem da página mestra que você acabou de criar e examine o código.

Uma nova página mestra terá um controle ContentPlaceHolder por padrão. Na maioria dos casos, faz mais sentido criar primeiro os elementos de página comuns e, em seguida, inserir os controles ContentPlaceHolder em que o conteúdo personalizado é desejado. Nesses casos, os desenvolvedores desejarão excluir o controle padrão ContentPlaceHolder e inserir novos, à medida que a página for desenvolvida. Os controles ContentPlaceHolder não são redimensionáveis apesar do fato de que eles exibem alças de dimensionamento. Os tamanhos de controle ContentPlaceHolder se baseiam automaticamente no conteúdo que ele contém com uma exceção; Se você posicionar um controle ContentPlaceHolder dentro de um elemento de bloco, como uma célula de tabela, ele será dimensionado de acordo com o tamanho do elemento.

## <a name="lab-1-working-with-master-pages"></a>Laboratório 1 trabalhando com páginas mestras

Neste laboratório, você criará uma nova página mestra e definirá três controles ContentPlaceHolder. Em seguida, você criará uma nova página de conteúdo e substituirá o conteúdo de pelo menos um dos controles ContentPlaceHolder.

1. Crie uma página mestra e insira controles ContentPlaceHolder. 

    1. Crie uma nova página mestra, conforme descrito acima.
    2. Exclua o controle padrão ContentPlaceHolder.
    3. Selecione o controle ContentPlaceHolder clicando na borda superior sombreada do controle e, em seguida, exclua-o pressionando a tecla DEL no teclado.
    4. Insira uma nova tabela usando o *cabeçalho e* o modelo lateral, conforme mostrado na Figura 3. Altere a largura e a altura para 90% cada para que a tabela inteira esteja visível no designer.

![](master-pages/_static/image3.jpg)

**Figura 3**

1. Coloque o cursor em cada célula da tabela e defina a propriedade *VAlign* como *Top*.
2. Na caixa de ferramentas, insira um controle ContentPlaceHolder na célula superior da tabela (a célula de cabeçalho.)
3. Ao inserir esse controle ContentPlaceHolder, você notará que a altura da linha ocupará quase toda a página, como mostra a Figura 4. Não se preocupe com isso neste ponto.

![O espaço vazio está na mesma célula que o ContentPlaceHolder](master-pages/_static/image1.gif)

**Figura 4**: o espaço vazio está na mesma célula que o ContentPlaceHolder

1. Coloque um controle ContentPlaceHolder nas outras duas células. Depois que os outros controles ContentPlaceHolder tiverem sido inseridos, o tamanho das células da tabela deverá ser o esperado. A página agora deve ser parecida com a página mostrada na **Figura 5**.

![O mestre com todos os controles ContentPlaceHolder. Observe que a altura da célula da célula de cabeçalho agora é o que deveria ser](master-pages/_static/image2.gif)

**Figura 5**: o mestre com todos os controles ContentPlaceHolder. Observe que a altura da célula da célula de cabeçalho agora é o que deveria ser

1. Insira algum texto de sua escolha em cada um dos três controles ContentPlaceHolder.
2. Salve a página mestra como Exercise1. master.
3. Crie um novo formulário da Web e associe-o à página mestra Exercise1. master.
4. Selecione arquivo, novo, arquivo no Visual Studio 2005.
5. Selecione **formulário da Web** na caixa de diálogo Adicionar novo item.
6. Verifique se a caixa de seleção selecionar página mestra está marcada, conforme mostrado na Figura 6.

![Adicionando uma nova página de conteúdo](master-pages/_static/image3.gif)

**Figura 6**: adicionando uma nova página de conteúdo

1. Clique em Adicionar.
2. Selecione Exercise1. Master na caixa de diálogo Selecionar uma página mestra, conforme mostrado na Figura 7.
3. Clique em OK para adicionar a nova página de conteúdo.

A página novo conteúdo é exibida no Visual Studio com um controle de conteúdo para cada controle ContentPlaceHolder na página mestra. Por padrão, os controles de conteúdo estão vazios para que você possa adicionar seu próprio conteúdo. Se você quiser que eles usem o conteúdo do controle ContentPlaceHolder na página mestra, basta clicar no símbolo de marca inteligente (a pequena seta preta no canto superior direito do controle) e escolher *padrão para conteúdo mestre* na marca inteligente, como mostrado na **Figura 8**. Quando você fizer isso, o item de menu será alterado para *criar conteúdo personalizado*. Clicar nesse ponto remove o conteúdo da página mestra, permitindo que você defina o conteúdo personalizado para esse controle de conteúdo específico.

![Definindo um controle de conteúdo como padrão para o conteúdo de páginas mestras](master-pages/_static/image4.gif)

**Figura 7**: definindo um controle de conteúdo como padrão para o conteúdo de páginas mestras

## <a name="connecting-master-page-and-content-pages"></a>Conectando página mestra e páginas de conteúdo

A associação entre uma página mestra e uma página de conteúdo pode ser configurada de uma das quatro maneiras diferentes:

- O atributo <strong>MasterPageFile</strong> da diretiva @Page
- Definindo a propriedade **Page. MasterPageFile** no código.
- O **&lt;páginas&gt;** elemento no arquivo de configuração de aplicativos (Web. config na pasta raiz do aplicativo)
- O elemento de **&gt;de&lt;Pages** em um arquivo de configuração de subpastas (Web. config em uma sub-pasta)

## <a name="masterpagefile-attribute"></a>Atributo MasterPagefile

O atributo MasterPagefile facilita a aplicação de uma página mestra a uma página ASP.NET específica. Também é o método usado para aplicar a página mestra quando você marca a caixa de seleção **selecionar página mestra** como fez no exercício 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Configurando Page. MasterPagefile no código

Ao definir a propriedade MasterPagefile no código, você pode aplicar uma página mestra específica ao seu conteúdo em tempo de execução. Isso é útil em casos em que talvez seja necessário aplicar uma página mestra específica com base em uma função de usuários ou em algum outro critério. A propriedade MasterPagefile deve ser definida no método PreInit. Se ele for definido após o método PreInit, um InvalidOperationException será gerado. A página na qual essa propriedade está sendo definida também deve ter um controle de conteúdo como o controle de nível superior para a página. Caso contrário, uma HttpException será lançada quando a propriedade MasterPagefile for definida.

## <a name="using-the-ltpagesgt-element"></a>Usando o elemento de&gt; de páginas de &lt;

Você pode configurar uma página mestra para suas páginas definindo o atributo masterPagefile no elemento &lt;Pages&gt; do arquivo Web. config. Ao usar esse método, tenha em mente que os arquivos Web. config inferiores na estrutura do aplicativo podem substituir essa configuração. Qualquer atributo MasterPagefile definido em uma diretiva @Page também substituirá essa configuração. O uso do elemento &lt;Pages&gt; simplifica a criação *de uma página mestra mestra que* pode ser substituída, se necessário, em arquivos ou pastas particulares.

## <a name="properties-in-master-pages"></a>Propriedades em páginas mestras

Uma página mestra pode expor propriedades simplesmente tornando essas propriedades públicas dentro da página mestra. Por exemplo, o código a seguir define uma propriedade chamada SomeProperty:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Para acessar a propriedade de Algumaproperty na página conteúdo, você precisará usar a propriedade Master desta forma:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Aninhando páginas mestras

As páginas mestras são a solução perfeita para garantir uma aparência comum em um grande aplicativo Web. No entanto, não é incomum ter determinadas partes de um site grande compartilhar uma interface comum enquanto outras partes compartilham uma interface diferente. Para resolver essa necessidade, várias páginas mestras são a solução perfeita. No entanto, isso ainda não resolve o fato de que um aplicativo grande pode ter determinados componentes (como um menu, por exemplo) que são compartilhados entre todas as páginas e outros componentes que são compartilhados somente entre determinadas seções do site. Para esse tipo de situação, as páginas mestras aninhadas preenchem perfeitamente a necessidade. Como você viu, uma página mestra normal consiste em uma página mestra e uma página de conteúdo. Em uma situação de página mestra aninhada, há duas páginas mestras; um mestre pai e um mestre filho. A página mestra filha também é uma página de conteúdo e seu mestre é a página mestra pai.

Este é o código para uma página mestra típica:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

Em um cenário mestre aninhado, esse seria o mestre pai. Outra página mestra usará essa página como página mestra e esse código ficaria assim:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Observe que, nesse cenário, o mestre filho também é uma página de conteúdo para o mestre pai. Todo o conteúdo do mestre filho aparece dentro de um controle de conteúdo que obtém seu conteúdo do controle ContentPlaceHolder do pai.

> [!NOTE]
> O suporte ao designer não está disponível para páginas mestras aninhadas. Quando você estiver desenvolvendo usando mestres aninhados, será necessário usar o modo de exibição de origem.

Este vídeo mostra uma explicação do uso de páginas mestras aninhadas.

![](master-pages/_static/image1.png)

[Abrir vídeo de tela inteira](master-pages/_static/nested1.wmv)

![Selecionando uma página mestra](master-pages/_static/image4.jpg)

**Figura 8**: selecionando uma página mestra
