---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
title: Criando um layout de todo o site usando páginas mestras (C#) | Microsoft Docs
author: rick-anderson
description: Este tutorial mostrará noções básicas da página mestra. Ou seja, as páginas mestras, como cria uma página mestra, o que são os contentores de conteúdo, como faz uma CR...
ms.author: riande
ms.date: 05/21/2008
ms.assetid: 78f8d194-03b9-44a5-8255-90e7cd1c2ee1
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a5e85c443a2a3642ec185ab1897c43cdb2ab1f7
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74619338"
---
# <a name="creating-a-site-wide-layout-using-master-pages-c"></a>Criação de um layout de todo o site usando páginas mestras (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_CS.zip) ou [baixar PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_CS.pdf)

> Este tutorial mostrará noções básicas da página mestra. Ou seja, as páginas mestras, como cria uma página mestra, o que são os contentores de conteúdo, como cria uma página ASP.NET que usa uma página mestra, como a modificação da página mestra é refletida automaticamente em suas páginas de conteúdo associadas e assim por diante.

## <a name="introduction"></a>Introdução

Um atributo de um site bem projetado é um layout de página consistente em todo o site. Use o site www.asp.net, por exemplo. No momento da redação deste artigo, cada página tem o mesmo conteúdo na parte superior e inferior da página. Como mostra a Figura 1, a parte superior de cada página exibe uma barra cinza com uma lista de comunidades da Microsoft. Abaixo disso está o logotipo do site, a lista de idiomas nos quais o site foi traduzido e as seções principais: início, introdução, aprendizado, downloads e assim por diante. Da mesma forma, a parte inferior da página inclui informações sobre publicidade no www.asp.net, uma declaração de direitos autorais e um link para a política de privacidade.

[![o site www.asp.net emprega uma aparência consistente em todas as páginas](creating-a-site-wide-layout-using-master-pages-cs/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image1.png)

<strong>Figura 01</strong>: o site do www.ASP.net emprega uma aparência consistente em todas as páginas ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-cs/_static/image3.png))

Outro atributo de um site bem projetado é a facilidade com a qual a aparência do site pode ser alterada. A Figura 1 mostra a home page do www.asp.net a partir de março de 2008, mas entre agora e a publicação deste tutorial, a aparência pode ter sido alterada. Talvez os itens de menu na parte superior se expandam para incluir uma nova seção para a MVC Framework. Ou talvez um design radicalmente novo com cores, fontes e layout diferentes sejam invelados. A aplicação dessas alterações ao site inteiro deve ser um processo rápido e simples que não exige a modificação dos milhares de páginas da Web que compõem o site.

A criação de um modelo de página de todo o site no ASP.NET é possível por meio do uso de *páginas mestras*. Resumindo, uma página mestra é um tipo especial de página ASP.NET que define a marcação que é comum entre todas as *páginas de conteúdo* , bem como regiões que são personalizáveis em uma página de conteúdo página por conteúdo. (Uma página de conteúdo é uma página ASP.NET associada à página mestra.) Sempre que o layout ou a formatação de uma página mestra for alterado, todas as suas saídas de páginas de conteúdo serão atualizadas da mesma forma imediatamente, o que torna a aplicação de alterações de aparência em todo o site tão fácil quanto atualizar e implantar um único arquivo (ou seja, a página mestra).

Este é o primeiro tutorial de uma série de tutoriais que exploram o uso de páginas mestras. Ao longo desta série de tutoriais, nós:

- Examinar a criação de páginas mestras e suas páginas de conteúdo associadas,
- Discuta uma variedade de dicas, truques e armadilhas,
- Identificar armadilhas comuns da página mestra e explorar soluções alternativas,
- Veja como acessar a página mestra de uma página de conteúdo e vice-versa,
- Saiba como especificar a página mestra de uma página de conteúdo em tempo de execução e
- Outros tópicos avançados da página mestra.

Esses tutoriais são voltados para serem concisos e fornecem instruções passo a passo com muitas capturas de tela para orientá-lo no processo visualmente. Cada tutorial está disponível nas C# versões e Visual Basic e inclui um download do código completo usado.

Este tutorial de inaugural começa com uma visão básica das páginas mestras. Vamos discutir como as páginas mestras funcionam, examinar a criação de uma página mestra e as páginas de conteúdo associadas usando o Visual Web Developer e ver como as alterações em uma página mestra são refletidas imediatamente em suas páginas de conteúdo. Vamos começar!

## <a name="understanding-how-master-pages-work"></a>Compreendendo como as páginas mestras funcionam

A criação de um site com um layout de página em todo o site exige que cada página da Web emita marcação de formatação comum além de seu conteúdo personalizado. Por exemplo, embora cada postagem de tutorial ou de fórum em www.asp.net tenha seu próprio conteúdo exclusivo, cada uma dessas páginas também renderiza uma série de elementos de `<div>` comuns que exibem os links de seção de nível superior: início, introdução, aprendizado e assim por diante.

Há uma variedade de técnicas para a criação de páginas da Web com uma aparência consistente. Uma abordagem simples é simplesmente copiar e colar a marcação de layout comum em todas as páginas da Web, mas essa abordagem tem uma série de desvantagens. Para os iniciantes, sempre que uma nova página for criada, você deverá se lembrar de copiar e colar o conteúdo compartilhado na página. Essas operações de cópia e colagem são maficadas para erros, pois você pode copiar acidentalmente apenas um subconjunto da marcação compartilhada em uma nova página. E para melhorá-la, essa abordagem torna a substituição da aparência existente em todo o site por um novo problema real, pois cada única página do site deve ser editada para usar a nova aparência.

Antes da versão 2,0 do ASP.NET, os desenvolvedores de páginas geralmente fizeram a marcação comum nos [controles de usuário](https://msdn.microsoft.com/library/y6wb1a0e.aspx) e adicionamos esses controles de usuário a cada página. Essa abordagem exigia que o desenvolvedor da página Lembre-se de adicionar manualmente os controles de usuário a cada nova página, mas que permitia modificações mais fáceis em todo o site, porque ao atualizar a marcação comum somente os controles de usuário precisavam ser modificados. Infelizmente, o Visual Studio .NET 2002 e 2003-as versões do Visual Studio usadas para criar aplicativos ASP.NET 1. x – processaram controles de usuário no modo de exibição de Design como caixas cinzas. Consequentemente, os desenvolvedores de páginas que usam essa abordagem não gostavam de um ambiente de tempo de design WYSIWYG.

As deficiências de usar controles de usuário foram abordadas no ASP.NET versão 2,0 e no Visual Studio 2005 com a introdução de *páginas mestras*. Uma página mestra é um tipo especial de página ASP.NET que define a marcação de todo o site e as *regiões* nas quais as *páginas de conteúdo* associadas definem sua marcação personalizada. Como veremos na etapa 1, essas regiões são definidas por controles ContentPlaceHolder. O controle ContentPlaceHolder simplesmente denota uma posição na hierarquia de controle da página mestra, em que o conteúdo personalizado pode ser injetado por uma página de conteúdo.

> [!NOTE]
> Os principais conceitos e a funcionalidade de páginas mestras não foram alterados desde a versão ASP.NET 2,0. No entanto, o Visual Studio 2008 oferece suporte a tempo de design para páginas mestras aninhadas, um recurso que não era no Visual Studio 2005. Veremos o uso de páginas mestras aninhadas em um tutorial futuro.

A Figura 2 mostra a aparência da página mestra de www.asp.net. Observe que a página mestra define o layout comum em todo o site – a marcação na parte superior, inferior e direita de cada página, bem como um ContentPlaceHolder no meio da esquerda, onde o conteúdo exclusivo de cada página da Web individual está localizado.

![Uma página mestra define o layout de todo o site e as regiões editáveis em uma base de página de conteúdo por conteúdo](creating-a-site-wide-layout-using-master-pages-cs/_static/image4.png)

**Figura 02**: uma página mestra define o layout de todo o site e as regiões editáveis em uma base de página de conteúdo por conteúdo

Depois que uma página mestra foi definida, ela pode ser associada a novas páginas ASP.NET por meio do tique de uma caixa de seleção. Essas páginas ASP.NET-chamadas de páginas de conteúdo – incluem um controle de conteúdo para cada um dos controles ContentPlaceHolder da página mestra. Quando a página de conteúdo é visitada por meio de um navegador, o mecanismo de ASP.NET cria a hierarquia de controle da página mestra e injeta a hierarquia de controle da página de conteúdo nos locais apropriados. Essa hierarquia de controle combinada é renderizada e o HTML resultante é retornado ao navegador do usuário final. Consequentemente, a página de conteúdo emite a marcação comum definida em sua página mestra fora dos controles ContentPlaceHolder e a marcação específica de página definida em seus próprios controles de conteúdo. A Figura 3 ilustra esse conceito.

[![a marcação da página solicitada é fundida na página mestra](creating-a-site-wide-layout-using-master-pages-cs/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image5.png)

**Figura 03**: a marcação da página solicitada é fundida na página mestra ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-cs/_static/image7.png))

Agora que discutimos como as páginas mestras funcionam, vamos dar uma olhada na criação de uma página mestra e nas páginas de conteúdo associadas usando o Visual Web Developer.

> [!NOTE]
> Para alcançar o público mais amplo possível, o site do ASP.NET que criamos em toda esta série de tutoriais será criado usando o ASP.NET 3,5 com a versão gratuita da Microsoft do Visual Studio 2008, o [Visual Web developer 2008](https://www.microsoft.com/express/vwd/). Se você ainda não tiver atualizado para o ASP.NET 3,5, não se preocupe – os conceitos discutidos nesses tutoriais funcionam igualmente bem com o ASP.NET 2,0 e o Visual Studio 2005. No entanto, alguns aplicativos de demonstração podem usar os recursos novos para a versão .NET Framework 3,5; Quando recursos específicos do 3,5 são usados, eu inclui uma observação que discute como implementar funcionalidades semelhantes na versão 2,0. Tenha em mente que os aplicativos de demonstração disponíveis para download de cada tutorial visam a versão 3,5 do .NET Framework, o que resulta em um arquivo de `Web.config` que inclui elementos de configuração específicos de 3,5 e referências a namespaces específicos do 3,5 nas instruções `using` nas classes code-behind das páginas ASP.NET. Longa história, se você ainda tiver que instalar o .NET 3,5 em seu computador, o aplicativo Web para download não funcionará sem primeiro remover a marcação específica de 3,5 da `Web.config`. Para obter mais informações sobre este tópico, consulte como desaparecer o [arquivo de `Web.config` da versão 3.5 do ASP.net](http://www.4guysfromrolla.com/articles/121207-1.aspx) . Você também precisará remover as instruções de `using` que fazem referência a namespaces de 3,5 específicos.

## <a name="step-1-creating-a-master-page"></a>Etapa 1: criando uma página mestra

Antes que possamos explorar a criação e o uso de páginas mestras e de conteúdo, primeiro precisamos de um site ASP.NET. Comece criando um novo site do ASP.NET baseado em sistema de arquivos. Para fazer isso, inicie o Visual Web Developer e, em seguida, vá para o menu arquivo e escolha novo site, exibindo a caixa de diálogo novo site (veja a Figura 4). Escolha o modelo de site da Web ASP.NET, defina a lista suspensa local para sistema de arquivos, escolha uma pasta para colocar o site e defina o idioma como C#. Isso criará um novo site com uma página `Default.aspx` ASP.NET, uma pasta `App_Data` e um arquivo de `Web.config`.

> [!NOTE]
> O Visual Studio dá suporte a dois modos de gerenciamento de projeto: projetos de site e projetos de aplicativos Web. Projetos de site não têm um arquivo de projeto, enquanto projetos de aplicativos Web imitam a arquitetura do projeto no Visual Studio .NET 2002/2003-eles incluem um arquivo de projeto e compilam o código-fonte do projeto em um único assembly, que é colocado na pasta `/bin`. Inicialmente, o Visual Studio 2005 tem suporte apenas para projetos de site, embora o [modelo de projeto de aplicativo Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) tenha sido reintroduzido com o Service Pack 1; O Visual Studio 2008 oferece ambos os modelos de projeto. No entanto, as edições Visual Web Developer 2005 e 2008 oferecem suporte apenas a projetos de site. Eu uso o modelo de projeto de site para minhas demonstrações nesta série de tutoriais. Se você estiver usando uma edição não Express e quiser usar o modelo de projeto de aplicativo Web em vez disso, fique à vontade para fazer isso, mas lembre-se de que pode haver algumas discrepâncias entre o que você vê na tela e as etapas que devem ser executadas em comparação com as capturas de tela mostradas e Instructio NS fornecido nesses tutoriais.

[![criar um novo site baseado no sistema de arquivos](creating-a-site-wide-layout-using-master-pages-cs/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image8.png)

**Figura 04**: criar um novo site baseado no sistema de arquivos ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-cs/_static/image10.png))

Em seguida, adicione uma página mestra ao site no diretório raiz clicando com o botão direito do mouse no nome do projeto, escolhendo Adicionar novo item e selecionando o modelo de página mestra. Observe que as páginas mestras terminam com a extensão `.master`. Nomeie essa nova página mestra `Site.master` e clique em Adicionar.

[![adicionar uma página mestra denominada site. Master ao site](creating-a-site-wide-layout-using-master-pages-cs/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image11.png)

**Figura 05**: adicionar uma página mestra denominada `Site.master` ao site ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-cs/_static/image13.png))

Adicionar um novo arquivo de página mestra por meio do Visual Web Developer cria uma página mestra com a seguinte marcação declarativa:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample1.aspx)]

A primeira linha na marcação declarativa é a [diretiva`@Master`](https://msdn.microsoft.com/library/ms228176.aspx). A diretiva `@Master` é semelhante à [diretiva`@Page`](https://msdn.microsoft.com/library/ydy4x04a.aspx) que aparece em páginas ASP.net. Ele define a linguagem do lado do servidorC#() e as informações sobre o local e a herança da classe code-behind da página mestra.

A `DOCTYPE` e a marcação declarativa da página aparece abaixo da diretiva `@Master`. A página inclui HTML estático juntamente com quatro controles do lado do servidor:

- **Um formulário da Web (o `<form runat="server">`)** -como todas as páginas ASP.net normalmente têm um formulário da Web – e como a página mestra pode incluir controles da Web que devem aparecer dentro de um formulário da Web-certifique-se de adicionar o formulário da Web à sua página mestra (em vez de adicionar um formulário da Web a cada página de conteúdo).
- **Um controle ContentPlaceHolder chamado `ContentPlaceHolder1`** -este controle ContentPlaceHolder aparece dentro do formulário da Web e serve como a região da interface do usuário da página de conteúdo.
- **Um elemento de `<head>` do lado do servidor** – o elemento `<head>` tem o atributo `runat="server"`, tornando-o acessível por meio de código do lado do servidor. O elemento `<head>` é implementado dessa maneira para que o título da página e outras marcações relacionadas a `<head>`possam ser adicionados ou ajustados programaticamente. Por exemplo, definir a propriedade `Title` de uma página ASP.NET altera o elemento `<title>` processado pelo controle de servidor `<head>`.
- **Um controle ContentPlaceHolder chamado `head`** -este controle ContentPlaceHolder aparece dentro do controle `<head>` Server e pode ser usado para adicionar conteúdo declarativamente ao elemento `<head>`.

Essa marcação declarativa de página mestra padrão serve como um ponto de partida para criar suas próprias páginas mestras. Sinta-se à vontade para editar o HTML ou adicionar outros controles da Web ou ContentPlaceHolders à página mestra.

> [!NOTE]
> Ao criar uma página mestra, verifique se a página mestra contém um formulário da Web e se pelo menos um controle ContentPlaceHolder aparece nesse formulário da Web.

### <a name="creating-a-simple-site-layout"></a>Criando um layout de site simples

Vamos expandir `Site.master`marcação declarativa padrão para criar um layout de site onde todas as páginas compartilham: um cabeçalho comum; uma coluna esquerda com navegação, notícias e outros conteúdos em todo o site; e um rodapé que exibe o ícone "ligado por Microsoft ASP.NET". A Figura 6 mostra o resultado final da página mestra quando uma de suas páginas de conteúdo é exibida por meio de um navegador. A região do círculo vermelho na Figura 6 é específica para a página que está sendo visitada (`Default.aspx`); o outro conteúdo é definido na página mestra e, portanto, consistente em todas as páginas de conteúdo.

[![a página mestra define a marcação para as partes superior, esquerda e inferior](creating-a-site-wide-layout-using-master-pages-cs/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image14.png)

**Figura 06**: a página mestra define a marcação para as partes superior, esquerda e inferior ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-cs/_static/image16.png))

Para obter o layout do site mostrado na Figura 6, comece atualizando a página mestra de `Site.master` para que ela contenha a seguinte marcação declarativa:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample2.aspx)]

O layout da página mestra é definido usando uma série de `<div>` elementos HTML. A `topContent` `<div>` contém a marcação que aparece na parte superior de cada página, enquanto as `mainContent`, `leftContent`e `footerContent` `<div>` são usadas para exibir o conteúdo da página, a coluna esquerda e o ícone "ligado por Microsoft ASP.NET", respectivamente. Além de adicionar esses elementos de `<div>`, também renomeei a propriedade `ID` do controle principal ContentPlaceHolder de `ContentPlaceHolder1` para `MainContent`.

As regras de formatação e layout para esses elementos `<div>`s sortidos são escritas no arquivo [CSS (folha de estilos em cascata)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) `Styles.css`, que é especificado por meio de um link de &lt;&gt; elemento no elemento &lt;Head&gt; da página mestra. Essas várias regras definem a aparência de cada elemento `<div>` observado acima. Por exemplo, o elemento `topContent` `<div>`, que exibe o texto e o link "tutoriais de páginas mestras", tem suas regras de formatação especificadas em `Styles.css` da seguinte maneira:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample3.css)]

Se estiver acompanhando o seu computador, você precisará baixar o código de acompanhamento deste tutorial e adicionar o arquivo de `Styles.css` ao seu projeto. Da mesma forma, você também precisará criar uma pasta denominada imagens e copiar o ícone "ligado por Microsoft ASP.NET" do site de demonstração baixado para o seu projeto.

> [!NOTE]
> Uma discussão sobre CSS e formatação de página da Web está além do escopo deste artigo. Para saber mais sobre CSS, confira os [tutoriais de CSS](http://www.w3schools.com/css/default.asp) em [w3schools.com](http://www.w3schools.com/). Também recomendo que você baixe o código fornecido deste tutorial e jogue com as configurações de CSS no `Styles.css` para ver os efeitos de diferentes regras de formatação.

### <a name="creating-a-master-page-using-an-existing-design-template"></a>Criando uma página mestra usando um modelo de design existente

Ao longo dos anos, criei vários aplicativos Web ASP.NET para empresas de pequeno a médio porte. Alguns dos meus clientes tinham um layout de site existente que desejavam usar; outros contratam um designer de gráficos competente. Alguns confiam em criar o layout do site. Como você pode dizer pela figura 6, a tarefa de um programador de criar um layout de site geralmente é tão inteligente quanto fazer com que seu contador realize cirurgias de coração, enquanto seu médico faz seus impostos.

Felizmente, há muitos sites que oferecem modelos de design HTML gratuitos – o Google retornou mais de 6 milhões resultados para o termo de pesquisa "modelos de site gratuitos". Um dos meus favoritos é [OpenDesigns.org](http://opendesigns.org/). Depois de encontrar um modelo de site que você deseja, adicione os arquivos e as imagens do CSS ao seu projeto de site e integre o HTML do modelo à sua página mestra.

> [!NOTE]
> A Microsoft também oferece vários [modelos gratuitos do ASP.net design Start Kit](https://msdn.microsoft.com/asp.net/aa336613.aspx) que se integram à caixa de diálogo New Web site no Visual Studio.

## <a name="step-2-creating-associated-content-pages"></a>Etapa 2: Criando páginas de conteúdo associadas

Com a página mestra criada, estamos prontos para começar a criar páginas ASP.NET associadas à página mestra. Essas páginas são chamadas de *páginas de conteúdo*.

Vamos adicionar uma nova página ASP.NET ao projeto e associá-la à página mestra `Site.master`. Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções e escolha a opção Adicionar novo item. Selecione o modelo de formulário da Web, insira o nome `About.aspx`e marque a caixa de seleção "selecionar página mestra", conforme mostrado na Figura 7. Isso exibirá a caixa de diálogo Selecionar uma página mestra (consulte a Figura 8) de onde você pode escolher a página mestra a ser usada.

> [!NOTE]
> Se você criou seu site do ASP.NET usando o modelo de projeto de aplicativo Web em vez do modelo de projeto de site, você não verá a caixa de seleção "selecionar página mestra" na caixa de diálogo Adicionar novo item mostrada na Figura 7. Para criar uma página de conteúdo ao usar o modelo de projeto de aplicativo Web, você deve escolher o modelo de formulário de conteúdo da Web em vez do modelo de formulário da Web. Depois de selecionar o modelo de formulário de conteúdo da Web e clicar em Adicionar, a mesma caixa de diálogo Selecionar uma página mestra mostrada na Figura 8 será exibida.

[![adicionar uma nova página de conteúdo](creating-a-site-wide-layout-using-master-pages-cs/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image17.png)

**Figura 07**: adicionar uma nova página de conteúdo ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-cs/_static/image19.png))

[![selecionar a página mestra do site. Master](creating-a-site-wide-layout-using-master-pages-cs/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image20.png)

**Figura 08**: selecione a página mestra de `Site.master` ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-cs/_static/image22.png))

Como mostra a marcação declarativa a seguir, uma nova página de conteúdo contém uma diretiva `@Page` que aponta de volta para sua página mestra e um controle de conteúdo para cada um dos controles ContentPlaceHolder da página mestra.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample4.aspx)]

> [!NOTE]
> Na seção "criando um layout de site simples" na etapa 1, renomeei `ContentPlaceHolder1` como `MainContent`. Se você não renomeou o `ID` do controle ContentPlaceHolder da mesma maneira, a marcação declarativa da página de conteúdo será ligeiramente diferente da marcação mostrada acima. Ou seja, a segunda `ContentPlaceHolderID` do controle de conteúdo refletirá a `ID` do controle ContentPlaceHolder correspondente em sua página mestra.

Ao renderizar uma página de conteúdo, o mecanismo ASP.NET deve fundir os controles de conteúdo da página com os controles ContentPlaceHolder da página mestra. O mecanismo ASP.NET determina a página mestra da página de conteúdo do atributo `MasterPageFile` da diretiva de `@Page`. Como mostra a marcação acima, essa página de conteúdo é associada a `~/Site.master`.

Como a página mestra tem dois controles ContentPlaceHolder, `head` e `MainContent`-Visual Web Developer gerou dois controles de conteúdo. Cada controle de conteúdo faz referência a um ContentPlaceHolder específico por meio de sua propriedade `ContentPlaceHolderID`.

O local em que as páginas mestras se destacam sobre as técnicas de modelo em todo o site é com o suporte ao tempo de design. A Figura 9 mostra a página de conteúdo `About.aspx` quando exibida por meio do modo de exibição de Design do Visual Web Developer. Observe que, embora o conteúdo da página mestra esteja visível, ele fica esmaecido e não pode ser modificado. No entanto, os controles de conteúdo correspondentes aos ContentPlaceHolders da página mestra são editáveis. E, assim como ocorre com qualquer outra página do ASP.NET, você pode criar a interface da página de conteúdo adicionando controles da Web por meio das exibições de origem ou design.

[![a exibição de design da página de conteúdo exibe o conteúdo específico da página e da página mestra](creating-a-site-wide-layout-using-master-pages-cs/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image23.png)

**Figura 09**: a exibição de design da página de conteúdo exibe o conteúdo de página mestra e específica da página ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-cs/_static/image25.png))

### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Adicionando marcação e controles da Web à página de conteúdo

Reserve um momento para criar algum conteúdo para a página de `About.aspx`. Como você pode ver na Figura 10, digitei um título "sobre o autor" e alguns parágrafos de texto, mas fique à vontade para adicionar controles da Web também. Depois de criar essa interface, visite a página `About.aspx` por meio de um navegador.

[![visite a página About. aspx por meio de um navegador](creating-a-site-wide-layout-using-master-pages-cs/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image26.png)

**Figura 10**: visite a página `About.aspx` por meio de um navegador ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-cs/_static/image28.png))

É importante entender que a página de conteúdo solicitada e sua página mestra associada são combinadas e renderizadas como um todo totalmente no servidor Web. O navegador do usuário final, em seguida, envia o HTML resultante e com fusível. Para verificar isso, exiba o HTML recebido pelo navegador acessando o menu exibir e escolhendo origem. Observe que não há nenhum quadro ou outras técnicas especializadas para exibir duas páginas da Web diferentes em uma única janela.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Ligando uma página mestra a uma página ASP.NET existente

Como vimos nesta etapa, adicionar uma nova página de conteúdo a um aplicativo Web ASP.NET é tão fácil quanto marcar a caixa de seleção "selecionar página mestra" e escolher a página mestra. Infelizmente, a conversão de uma página ASP.NET existente em uma página mestra não é tão fácil.

Para associar uma página mestra a uma página ASP.NET existente, você precisa executar as seguintes etapas:

1. Adicione o atributo `MasterPageFile` à diretiva `@Page` da página ASP.NET, apontando-o para a página mestra apropriada.
2. Adicione controles de conteúdo para cada um dos ContentPlaceHolders na página mestra.
3. Recorte seletivamente e cole o conteúdo existente da página ASP.NET nos controles de conteúdo apropriados. Eu digo "seletivamente" aqui porque a página ASP.NET provavelmente contém marcação que já é expressa pela página mestra, como o `DOCTYPE`, o elemento `<html>` e o formulário da Web.

Para obter as instruções passo a passo sobre esse processo juntamente com capturas de tela, confira o tutorial do [Scott Guthrie](https://weblogs.asp.net/scottgu/) [usando páginas mestras e navegação do site](http://webproject.scottgu.com/CSharp/MasterPages/MasterPages.aspx) . A seção "atualizar `Default.aspx` e `DataSample.aspx` usar a página mestra" detalha essas etapas.

Como é muito mais fácil criar novas páginas de conteúdo do que converter páginas ASP.NET existentes em páginas de conteúdo, recomendo que sempre que você crie um novo site do ASP.NET, adicione uma página mestra ao site. Associar todas as novas páginas do ASP.NET a esta página mestra. Não se preocupe se a página mestra inicial for muito simples ou simples; Você pode atualizar a página mestra mais tarde.

> [!NOTE]
> Ao criar um novo aplicativo ASP.NET, o Visual Web Developer adiciona uma página `Default.aspx` que não está associada a uma página mestra. Se você quiser praticar a conversão de uma página ASP.NET existente em uma página de conteúdo, vá em frente e faça isso com `Default.aspx`. Como alternativa, você pode excluir `Default.aspx` e, em seguida, adicioná-lo novamente, mas desta vez verificando a caixa de seleção "selecionar página mestra".

## <a name="step-3-updating-the-master-pages-markup"></a>Etapa 3: atualizando a marcação da página mestra

Um dos principais benefícios das páginas mestras é que uma única página mestra pode ser usada para definir o layout geral de várias páginas no site. Portanto, atualizar a aparência do site requer a atualização de um único arquivo – a página mestra.

Para ilustrar esse comportamento, vamos atualizar nossa página mestra para incluir a data atual na parte superior da coluna esquerda. Adicione um rótulo chamado `DateDisplay` ao `<div>`de `leftContent`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample5.aspx)]

Em seguida, crie um manipulador de eventos `Page_Load` para a página mestra e adicione o seguinte código:

[!code-csharp[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample6.cs)]

O código acima define a propriedade `Text` do rótulo como a data e hora atuais formatados como o dia da semana, o nome do mês e o dia de dois dígitos (consulte a Figura 11). Com essa alteração, reveja uma das suas páginas de conteúdo. Como mostra a Figura 11, a marcação resultante é imediatamente atualizada para incluir a alteração na página mestra.

[![as alterações na página mestra são refletidas ao exibir a página de conteúdo](creating-a-site-wide-layout-using-master-pages-cs/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image29.png)

**Figura 11**: as alterações na página mestra são refletidas ao exibir a página de conteúdo ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-cs/_static/image31.png))

> [!NOTE]
> Como ilustra este exemplo, as páginas mestras podem conter controles da Web do servidor, código e manipuladores de eventos.

## <a name="summary"></a>Resumo

As páginas mestras permitem que os desenvolvedores de ASP.NET projetem um layout consistente em todo o site que é facilmente atualizável. A criação de páginas mestras e suas páginas de conteúdo associadas é tão simples quanto a criação de páginas ASP.NET padrão, pois o Visual Web Developer oferece suporte a tempo de design avançado.

O exemplo de página mestra que criamos neste tutorial tinha dois controles ContentPlaceHolder, `head` e `MainContent`. No entanto, especificamos apenas a marcação para o controle `MainContent` ContentPlaceHolder em nossa página de conteúdo. No próximo tutorial, examinaremos o uso de vários controles de conteúdo na página conteúdo. Também vemos como definir a marcação padrão para controles de conteúdo dentro da página mestra, bem como alternar entre usar a marcação padrão definida na página mestra e fornecer marcação personalizada na página de conteúdo.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [ASP.NET para designers: modelos de design gratuitos e orientações sobre a criação de sites ASP.NET usando padrões da Web](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [Visão geral das páginas mestras do ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Tutoriais de CSS (folhas de estilos em cascata)](http://www.w3schools.com/css/default.asp)
- [Definindo dinamicamente o título da página](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Páginas mestras em ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Tutoriais de início rápido das páginas mestras](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP. net e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 3,5 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott pode ser contatado em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Avançar](multiple-contentplaceholders-and-default-content-cs.md)
