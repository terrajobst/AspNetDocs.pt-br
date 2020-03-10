---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
title: Páginas mestras aninhadas (VB) | Microsoft Docs
author: rick-anderson
description: Mostra como aninhar uma página mestra dentro de outra.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 14d9aa1b-4dca-43a0-aa9d-a6e891fee019
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 9bb39712855c37f5cbcbb447f7691e9451b8dc92
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78571844"
---
# <a name="nested-master-pages-vb"></a>Páginas mestras aninhadas (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.zip) ou [baixar PDF](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_VB.pdf)

> Mostra como aninhar uma página mestra dentro de outra.

## <a name="introduction"></a>Introdução

No decorrer dos nove tutoriais anteriores, vimos como implementar um layout de todo o site com páginas mestras. Em resumo, as páginas mestras nos permitem, o desenvolvedor da página, definir a marcação comum na página mestra junto com regiões específicas que podem ser personalizadas em uma página de conteúdo página por conteúdo. Os controles ContentPlaceHolder em uma página mestra indicam as regiões personalizáveis; a marcação personalizada para os controles ContentPlaceHolder é definida na página conteúdo por meio de controles de conteúdo.

As técnicas de página mestra que exploramos até agora são ótimas se você tiver um único layout usado em todo o site. No entanto, muitos sites grandes têm um layout de site que é personalizado em várias seções. Por exemplo, considere um aplicativo de assistência médica usado pela equipe do hospital para gerenciar informações de pacientes, atividades e cobrança. Pode haver três tipos de páginas da Web neste aplicativo:

- Páginas específicas de membros da equipe em que os membros da equipe podem atualizar a disponibilidade, exibir agendamentos ou solicitar tempo de férias.
- Páginas específicas do paciente em que os membros da equipe exibem ou editam informações de um paciente específico.
- Páginas específicas de cobrança em que os contadores examinam os status atuais da declaração e os relatórios financeiros.

Cada página pode compartilhar um layout comum, como um menu na parte superior e uma série de links usados com frequência ao longo da parte inferior. Mas as páginas específicas da equipe, do paciente e da cobrança podem precisar personalizar esse layout genérico. Por exemplo, talvez todas as páginas específicas da equipe incluam um calendário e uma lista de tarefas que mostrem a disponibilidade do usuário conectado no momento e a agenda diária. Talvez todas as páginas específicas do paciente precisem mostrar o nome, o endereço e as informações de seguro para o paciente cujas informações estão sendo editadas.

É possível criar esses layouts personalizados usando *páginas mestras aninhadas*. Para implementar o cenário acima, começaremos criando uma página mestra que definia o layout de todo o site, o menu e o conteúdo do rodapé, com ContentPlaceHolders definindo as regiões personalizáveis. Em seguida, criaremos três páginas mestras aninhadas, uma para cada tipo de página da Web. Cada página mestra aninhada definiria o conteúdo entre o tipo de páginas de conteúdo que usam a página mestra. Em outras palavras, a página mestra aninhada para páginas de conteúdo específicas do paciente inclui marcação e lógica programática para exibir informações sobre o paciente que está sendo editado. Ao criar uma nova página específica do paciente, ela será associada a essa página mestra aninhada.

Este tutorial começa destacando os benefícios de páginas mestras aninhadas. Em seguida, ele mostra como criar e usar páginas mestras aninhadas.

> [!NOTE]
> Páginas mestras aninhadas foram possíveis desde a versão 2,0 do .NET Framework. No entanto, o Visual Studio 2005 não inclui suporte ao tempo de design para páginas mestras aninhadas. A boa notícia é que o Visual Studio 2008 oferece uma experiência de tempo de design rica para páginas mestras aninhadas. Se você estiver interessado em usar páginas mestras aninhadas, mas ainda estiver usando o Visual Studio 2005, confira a entrada de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/), [dicas para páginas mestras aninhadas no tempo de Design de vs 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).

## <a name="the-benefits-of-nested-master-pages"></a>Os benefícios das páginas mestras aninhadas

Muitos sites têm um design de site abrangente, bem como designs mais personalizados específicos para determinados tipos de páginas. Por exemplo, em nosso aplicativo Web de demonstração, criamos uma seção de administração rudimentar (as páginas na pasta `~/Admin`). Atualmente, as páginas da Web na pasta `~/Admin` usam a mesma página mestra que as páginas que não estão na seção Administração (ou seja, `Site.master` ou `Alternate.master`, dependendo da seleção do usuário).

> [!NOTE]
> Por enquanto, finja que nosso site tem apenas uma página mestra, `Site.master`. Vamos abordar usando páginas mestras aninhadas com duas (ou mais) páginas mestras começando com "usando uma página mestra aninhada para a seção de administração" mais adiante neste tutorial.

Imagine que fomos solicitados a personalizar o layout das páginas de administração para incluir informações adicionais ou links que, de outra forma, não estaria presente em outras páginas do site. Há quatro técnicas para implementar esse requisito:

1. Adicione manualmente as informações específicas de administração e os links a cada página de conteúdo na pasta `~/Admin`.
2. Atualize a página mestra de `Site.master` para incluir informações e links específicos da seção de administração e, em seguida, adicione o código à página mestra para mostrar ou ocultar essas seções com base no fato de uma das páginas de administração estar sendo visitada.
3. Crie uma nova página mestra especificamente para a seção Administração, copie sobre a marcação de `Site.master`, adicione informações e links específicos da seção de administração e, em seguida, atualize as páginas de conteúdo na pasta `~/Admin` para usar essa nova página mestra.
4. Crie uma página mestra aninhada que se vincule a `Site.master` e faça com que as páginas de conteúdo na pasta `~/Admin` Use essa nova página mestra aninhada. Essa página mestra aninhada incluiria apenas as informações adicionais e os links específicos para as páginas de administração e não precisaria repetir a marcação já definida em `Site.master`.

A primeira opção é o menos agradável. O ponto completo de usar páginas mestras é deixar de copiar e colar manualmente a marcação comum em novas páginas do ASP.NET. A segunda opção é aceitável, mas torna o aplicativo menos passível de manutenção, pois ele faz o backup das páginas mestras com marcação que só é exibida ocasionalmente e exige que os desenvolvedores editem a página mestra para contornar essa marcação e ter que se lembrar quando, exatamente, determinada marcação é exibida em vez de ser ocultada. Essa abordagem seria menos tenable como personalizações de cada vez mais tipos de páginas da Web que precisavam ser acomodadas por essa única página mestra.

A terceira opção remove os problemas de aglomeração e complexidade da superfície com a segunda opção. No entanto, a principal desvantagem da opção 3 é que ela requer que possamos copiar e colar o layout comum de `Site.master` para a nova página mestra específica da seção de administração. Posteriormente, se decidirmos alterar o layout de todo o site, precisaremos se lembrar de alterá-lo em dois lugares.

A quarta opção, páginas mestras aninhadas, nos oferece o melhor da segunda e terceira opções. As informações de layout de todo o site são mantidas em um arquivo – a página mestra de nível superior-enquanto o conteúdo específico para regiões específicas é separado em arquivos diferentes.

Este tutorial começa com uma olhada na criação e no uso de uma página mestra aninhada simples. Criamos uma nova página mestra de nível superior, duas páginas mestras aninhadas e duas páginas de conteúdo. A partir da seção "usando uma página mestra aninhada para a administração", examinamos a atualização de nossa arquitetura de página mestra existente para incluir o uso de páginas mestras aninhadas. Especificamente, criamos uma página mestra aninhada e a usamos para incluir conteúdo personalizado adicional para as páginas de conteúdo na pasta `~/Admin`.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Etapa 1: criando uma página mestra de nível superior simples

Criar um mestre aninhado com base em uma das páginas mestras existentes e, em seguida, atualizar uma página de conteúdo existente para usar essa nova página mestra aninhada em vez da página mestra de nível superior envolve alguma complexidade porque as páginas de conteúdo existentes já esperam determinadas Controles ContentPlaceHolder definidos na página mestra de nível superior. Portanto, a página mestra aninhada também deve incluir os mesmos controles ContentPlaceHolder com os mesmos nomes. Além disso, nosso aplicativo de demonstração em particular tem duas páginas mestras (`Site.master` e `Alternate.master`) que são atribuídas dinamicamente a uma página de conteúdo com base nas preferências de um usuário, o que aumenta ainda mais essa complexidade. Veremos a atualização do aplicativo existente para usar páginas mestras aninhadas posteriormente neste tutorial, mas vamos nos concentrar primeiro em um exemplo simples de páginas mestras aninhadas.

Crie uma nova pasta chamada `NestedMasterPages` e, em seguida, adicione um novo arquivo de página mestra a essa pasta chamada `Simple.master`. (Consulte a Figura 1 para obter uma captura de tela do Gerenciador de Soluções depois que essa pasta e arquivo tiverem sido adicionados.) Arraste o `AlternateStyles.css` arquivo de folha de estilo do Gerenciador de Soluções para o designer. Isso adiciona um elemento `<link>` ao arquivo de folha de estilos no elemento `<head>`, após o qual a marcação do elemento de `<head>` da página mestra deve ser semelhante a:

[!code-aspx[Main](nested-master-pages-vb/samples/sample1.aspx)]

Em seguida, adicione a seguinte marcação no formulário da Web de `Simple.master`:

[!code-aspx[Main](nested-master-pages-vb/samples/sample2.aspx)]

Essa marcação exibe um link intitulado "páginas mestras aninhadas (simples)" na parte superior da página em uma fonte branca grande em um plano de fundo azul. Abaixo disso está o `MainContent` ContentPlaceHolder. A Figura 1 mostra a `Simple.master` página mestra quando carregada no Visual Studio Designer.

[![a página mestra aninhada define o conteúdo específico para as páginas na seção Administração](nested-master-pages-vb/_static/image2.png)](nested-master-pages-vb/_static/image1.png)

**Figura 01**: a página mestra aninhada define o conteúdo específico para as páginas na seção Administração ([clique para exibir a imagem em tamanho normal](nested-master-pages-vb/_static/image3.png))

## <a name="step-2-creating-a-simple-nested-master-page"></a>Etapa 2: criando uma página mestra aninhada simples

`Simple.master` contém dois controles ContentPlaceHolder: o `MainContent` ContentPlaceHolder que adicionamos dentro do formulário da Web, juntamente com o `head` ContentPlaceHolder no elemento `<head>`. Se tivéssemos de criar uma página de conteúdo e associá-la ao `Simple.master` a página de conteúdo teria dois controles de conteúdo referenciando os dois ContentPlaceHolders. Da mesma forma, se criarmos uma página mestra aninhada e associá-la a `Simple.master`, a página mestra aninhada terá dois controles de conteúdo.

Vamos adicionar uma nova página mestra aninhada à pasta `NestedMasterPages` chamada `SimpleNested.master`. Clique com o botão direito do mouse na pasta `NestedMasterPages` e escolha Adicionar novo item. Isso abre a caixa de diálogo Adicionar novo item mostrada na Figura 2. Selecione o tipo de modelo da página mestra e digite o nome da nova página mestra. Para indicar que a nova página mestra deve ser uma página mestra aninhada, marque a caixa de seleção "selecionar página mestra".

Em seguida, clique no botão Adicionar. Isso exibirá a caixa de diálogo Selecionar uma página mestra que você vê ao vincular uma página de conteúdo a uma página mestra (consulte a Figura 3). Escolha a página mestra de `Simple.master` na pasta `NestedMasterPages` e clique em OK.

> [!NOTE]
> Se você criou seu site do ASP.NET usando o modelo de projeto de aplicativo Web em vez do modelo de projeto de site, você não verá a caixa de seleção "selecionar página mestra" na caixa de diálogo Adicionar novo item mostrada na Figura 2. Para criar uma página mestra aninhada ao usar o modelo de projeto de aplicativo Web, você deve escolher o modelo de página mestra aninhada (em vez do modelo de página mestra). Depois de selecionar o modelo de página mestra aninhada e clicar em Adicionar, a mesma caixa de diálogo Selecionar uma página mestra mostrada na Figura 3 será exibida.

[![marque a caixa de seleção &quot;selecionar página mestra&quot; para adicionar uma página mestra aninhada](nested-master-pages-vb/_static/image5.png)](nested-master-pages-vb/_static/image4.png)

**Figura 02**: marque a caixa de seleção "selecionar página mestra" para adicionar uma página mestra aninhada ([clique para exibir a imagem em tamanho normal](nested-master-pages-vb/_static/image6.png))

[![associar a página mestra aninhada à página mestra simples. Master](nested-master-pages-vb/_static/image8.png)](nested-master-pages-vb/_static/image7.png)

**Figura 03**: associar a página mestra aninhada à página `Simple.master` mestra ([clique para exibir a imagem em tamanho normal](nested-master-pages-vb/_static/image9.png))

A marcação declarativa da página mestra aninhada, mostrada abaixo, contém dois controles de conteúdo que fazem referência aos dois controles ContentPlaceHolder da página mestra de nível superior.

[!code-aspx[Main](nested-master-pages-vb/samples/sample3.aspx)]

Exceto para a diretiva `<%@ Master %>`, a marcação declarativa inicial da página mestra aninhada é idêntica à marcação que é gerada inicialmente ao associar uma página de conteúdo à mesma página mestra de nível superior. Assim como a diretiva de `<%@ Page %>` de uma página de conteúdo, a diretiva de `<%@ Master %>` aqui inclui um atributo `MasterPageFile` que especifica a página mestra pai da página mestra aninhada. A principal diferença entre a página mestra aninhada e uma página de conteúdo associada à mesma página mestra de nível superior é que a página mestra aninhada pode incluir controles ContentPlaceHolder. Os controles ContentPlaceHolder da página mestra aninhada definem as regiões nas quais as páginas de conteúdo podem personalizar a marcação.

Atualize essa página mestra aninhada para que ela exiba o texto "Olá, de SimpleNested!" no controle de conteúdo que corresponde ao controle `MainContent` ContentPlaceHolder.

[!code-aspx[Main](nested-master-pages-vb/samples/sample4.aspx)]

Depois de fazer essa adição, salve a página mestra aninhada e, em seguida, adicione uma nova página de conteúdo à pasta `NestedMasterPages` chamada `Default.aspx`e associe-a à página mestra `SimpleNested.master`. Ao adicionar essa página, você pode ficar surpreso em ver que ela não contém nenhum controle de conteúdo (consulte a Figura 4)! Uma página de conteúdo só pode acessar seus ContentPlaceHolders da página mestra *pai* . `SimpleNested.master` não contém nenhum controle ContentPlaceHolder; Portanto, qualquer página de conteúdo associada a essa página mestra não pode conter nenhum controle de conteúdo.

[![a nova página de conteúdo não contém nenhum controle de conteúdo](nested-master-pages-vb/_static/image11.png)](nested-master-pages-vb/_static/image10.png)

**Figura 04**: a nova página de conteúdo não contém nenhum controle de conteúdo ([clique para exibir a imagem em tamanho normal](nested-master-pages-vb/_static/image12.png))

O que precisamos fazer é atualizar a página mestra aninhada (`SimpleNested.master`) para incluir controles ContentPlaceHolder. Normalmente, você desejará que suas páginas mestras aninhadas incluam um ContentPlaceHolder para cada ContentPlaceHolder definido por sua página mestra pai, permitindo que sua página mestra ou página de conteúdo filho funcione com qualquer um dos ContentPlaceHolders da página mestra de nível superior controles.

Atualize a página mestra de `SimpleNested.master` para incluir um ContentPlaceHolder em seus dois controles de conteúdo. Dê ao ContentPlaceHolder controles o mesmo nome que o controle ContentPlaceHolder ao qual seu controle de conteúdo se refere. Ou seja, adicione um controle ContentPlaceHolder chamado `MainContent` ao controle de conteúdo em `SimpleNested.master` que faz referência ao `MainContent` ContentPlaceHolder no `Simple.master`. Faça a mesma coisa no controle de conteúdo que faz referência ao `head` ContentPlaceHolder.

> [!NOTE]
> Embora eu recomende nomear os controles ContentPlaceHolder na página mestre aninhada da mesma forma que os ContentPlaceHolders na página mestra de nível superior, essa simetria de nomenclatura não é necessária. Você pode dar aos controles ContentPlaceHolder em sua página mestra aninhada qualquer nome que desejar. No entanto, acho mais fácil lembrar quais ContentPlaceHolders correspondem a quais regiões da página se minha página mestra de nível superior e páginas mestras aninhadas usam os mesmos nomes.

Depois de fazer essas adições, a marcação declarativa de `SimpleNested.master` página mestra deve ser semelhante ao seguinte:

[!code-aspx[Main](nested-master-pages-vb/samples/sample5.aspx)]

Exclua a página de conteúdo de `Default.aspx` que acabamos de criar e, em seguida, adicione-a novamente, associando-a à página mestra de `SimpleNested.master`. Desta vez, o Visual Studio adiciona dois controles de conteúdo ao `Default.aspx`, referenciando os ContentPlaceHolders agora definidos em `SimpleNested.master` (consulte a Figura 6). Adicione o texto "Olá, de default. aspx!" no controle de conteúdo que referenciou `MainContent`.

A Figura 5 mostra as três entidades envolvidas aqui-`Simple.master`, `SimpleNested.master`e `Default.aspx` e como elas se relacionam entre si. Como mostra o diagrama, a página mestra aninhada implementa controles de conteúdo para o ContentPlaceHolder de seu pai. Se essas regiões precisarem ser acessíveis para a página de conteúdo, a página mestra aninhada deverá adicionar seus próprios ContentPlaceHolders aos controles de conteúdo.

[![as páginas mestras de nível superior e aninhado ditam o layout da página de conteúdo](nested-master-pages-vb/_static/image14.png)](nested-master-pages-vb/_static/image13.png)

**Figura 05**: as páginas mestras de nível superior e aninhado ditam o layout da página de conteúdo ([clique para exibir a imagem em tamanho normal](nested-master-pages-vb/_static/image15.png))

Esse comportamento ilustra como uma página de conteúdo ou uma página mestra é apenas Cognizant de sua página mestra pai. Esse comportamento também é indicado pelo Visual Studio Designer. A Figura 6 mostra o designer para `Default.aspx`. Embora o designer mostre claramente quais regiões são editáveis na página conteúdo e quais partes não são, ela não faz a ambiguidade de quais regiões não editáveis são provenientes da página mestra aninhada e quais regiões são da página mestra de nível superior.

[![a página de conteúdo agora inclui controles de conteúdo para os ContentPlaceHolders da página mestra aninhada](nested-master-pages-vb/_static/image17.png)](nested-master-pages-vb/_static/image16.png)

**Figura 06**: a página de conteúdo agora inclui controles de conteúdo para os ContentPlaceHolders da página mestra aninhada ([clique para exibir a imagem em tamanho normal](nested-master-pages-vb/_static/image18.png))

## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Etapa 3: adicionando uma segunda página mestra aninhada simples

O benefício das páginas mestras aninhadas é mais evidente quando há várias páginas mestras aninhadas. Para ilustrar esse benefício, crie outra página mestra aninhada na pasta `NestedMasterPages`; Nomeie essa nova página mestra aninhada `SimpleNestedAlternate.master` e associe-a à `Simple.master` página mestra. Adicione controles ContentPlaceHolder nos dois controles de conteúdo da página mestra aninhada como fizemos na etapa 2. Adicione também o texto "Olá, de SimpleNestedAlternate!" no controle de conteúdo que corresponde ao `MainContent` ContentPlaceHolder da página mestra de nível superior. Depois de fazer essas alterações, a marcação declarativa de sua nova página mestra aninhada deve ser semelhante ao seguinte:

[!code-aspx[Main](nested-master-pages-vb/samples/sample6.aspx)]

Crie uma página de conteúdo chamada `Alternate.aspx` na pasta `NestedMasterPages` e associe-a à página mestra aninhada `SimpleNestedAlternate.master`. Adicione o texto "Olá, de alternativo!" no controle de conteúdo que corresponde a `MainContent`. A Figura 7 mostra `Alternate.aspx` quando exibido por meio do designer do Visual Studio.

[![o Alternate. aspx está associado à página mestra SimpleNestedAlternate. Master](nested-master-pages-vb/_static/image20.png)](nested-master-pages-vb/_static/image19.png)

**Figura 07**: `Alternate.aspx` está associado à `SimpleNestedAlternate.master` página mestra ([clique para exibir a imagem em tamanho normal](nested-master-pages-vb/_static/image21.png))

Compare o designer da Figura 7 com o designer na Figura 6. Ambas as páginas de conteúdo compartilham o mesmo layout definido na página mestra de nível superior (`Simple.master`), ou seja, o título "tutorial de páginas mestras aninhadas (simples)". Mas ambos têm conteúdo distinto definido em suas páginas mestras pai-o texto "Olá, de SimpleNested!" na Figura 6 e "Olá, de SimpleNestedAlternate!" na Figura 7. Tudo bem, essas diferenças são triviais, mas você pode estender esse exemplo para incluir diferenças mais significativas. Por exemplo, a página `SimpleNested.master` pode incluir um menu com opções específicas para suas páginas de conteúdo, enquanto `SimpleNestedAlternate.master` pode ter informações pertinentes às páginas de conteúdo que se associam a ela.

Agora, imagine que precisávamos fazer uma alteração no layout de site abrangente. Por exemplo, imagine que queríamos adicionar uma lista de links comuns a todas as páginas de conteúdo. Para fazer isso, atualizamos a página mestra de nível superior, `Simple.master`. Todas as alterações são refletidas imediatamente em suas páginas mestras aninhadas e, por extensão, suas páginas de conteúdo.

Para demonstrar a facilidade com a qual podemos alterar o layout de site abrangente, abra a página `Simple.master` mestra e adicione a seguinte marcação entre os elementos `topContent` e `mainContent` `<div>`:

[!code-aspx[Main](nested-master-pages-vb/samples/sample7.aspx)]

Isso adiciona dois links à parte superior de cada página vinculada a `Simple.master`, `SimpleNested.master`ou `SimpleNestedAlternate.master`; essas alterações se aplicam a todas as páginas mestras aninhadas e suas páginas de conteúdo imediatamente. A Figura 8 mostra `Alternate.aspx` quando visualizado por meio de um navegador. Observe a adição dos links na parte superior da página (em comparação com a Figura 7).

[![alterado para a página mestra de nível superior são refletidos imediatamente em suas páginas mestras aninhadas e suas páginas de conteúdo](nested-master-pages-vb/_static/image23.png)](nested-master-pages-vb/_static/image22.png)

**Figura 08**: alterado para a página mestra de nível superior é refletido imediatamente em suas páginas mestras aninhadas e suas páginas de conteúdo ([clique para exibir a imagem em tamanho normal](nested-master-pages-vb/_static/image24.png))

## <a name="using-a-nested-master-page-for-the-administration-section"></a>Usando uma página mestra aninhada para a seção de administração

Neste ponto, examinamos as vantagens das páginas mestras aninhadas e vimos como criá-las e usá-las em um aplicativo ASP.NET. No entanto, os exemplos nas etapas 1, 2 e 3 envolvem a criação de uma nova página mestra de nível superior, novas páginas mestras aninhadas e novas páginas de conteúdo. E quanto à adição de uma nova página mestra aninhada a um site com uma página mestra de nível superior e páginas de conteúdo existentes?

Integrar uma página mestra aninhada a um site existente e associá-la a páginas de conteúdo existentes requer um pouco mais de esforço do que a partir do zero. As etapas 4, 5, 6 e 7 exploram esses desafios à medida que aumentamos nosso aplicativo de demonstração para incluir uma nova página mestra aninhada chamada `AdminNested.master` que contém instruções para o administrador e é usada pelas páginas ASP.NET na pasta `~/Admin`.

A integração de uma página mestra aninhada ao nosso aplicativo de demonstração apresenta os seguintes obstáculos:

- As páginas de conteúdo existentes na pasta `~/Admin` têm determinadas expectativas de sua página mestra. Para os iniciantes, eles esperam que determinados controles ContentPlaceHolder estejam presentes. Além disso, as páginas `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` chamam o método de `RefreshRecentProductsGrid` público da página mestra, definimos sua propriedade `GridMessageText` ou um manipulador de eventos para seu evento `PricesDoubled`. Consequentemente, nossa página mestra aninhada deve fornecer os mesmos ContentPlaceHolders e membros públicos.
- No tutorial anterior, aprimoramos a classe `BasePage` para definir dinamicamente a propriedade `MasterPageFile` do objeto de `Page` com base em uma variável de sessão. Como damos suporte a páginas mestras dinâmicas ao usar páginas mestras aninhadas?

Esses dois desafios surgirão à medida que criarmos a página mestra aninhada e a usarem de nossas páginas de conteúdo existentes. Nós investigaremos e superaremos esses problemas à medida que surgirem.

## <a name="step-4-creating-the-nested-master-page"></a>Etapa 4: criando a página mestra aninhada

Nossa primeira tarefa é criar a página mestra aninhada a ser usada pelas páginas na seção Administração. Como vimos na etapa 2, ao adicionar uma nova página mestra aninhada, precisamos especificar a página mestra pai da página mestra aninhada. Mas temos duas páginas mestras de nível superior: `Site.master` e `Alternate.master`. Lembre-se de que criamos `Alternate.master` no tutorial anterior e escrevi o código na classe `BasePage` que define a propriedade `MasterPageFile` do objeto de `Page` em tempo de execução para `Site.master` ou `Alternate.master` dependendo do valor da variável de sessão de `MyMasterPage`.

Como podemos configurar nossa página mestra aninhada para que ela use a página mestra de nível superior apropriada? Nós temos duas opções:

- Crie duas páginas mestras aninhadas, `AdminNestedSite.master` e `AdminNestedAlternate.master`e associá-las às páginas mestras de nível superior `Site.master` e `Alternate.master`, respectivamente. Em `BasePage`, em seguida, definimos o `MasterPageFile` do objeto `Page` como a página mestra aninhada apropriada.
- Crie uma única página mestra aninhada e faça com que as páginas de conteúdo usem essa página mestra específica. Em seguida, em tempo de execução, seria necessário definir a propriedade `MasterPageFile` da página mestra aninhada como a página mestra de nível superior apropriada em tempo de execução. (Como você já deve ter descoberto por enquanto, as páginas mestras também têm uma propriedade `MasterPageFile`.)

Vamos usar a segunda opção. Crie um único arquivo de página mestra aninhada na pasta `~/Admin` chamada `AdminNested.master`. Como `Site.master` e `Alternate.master` têm o mesmo conjunto de controles ContentPlaceHolder, não importa a página mestra à qual você o associa, embora eu o incentive a associá-lo ao `Site.master` para fins de consistência.

[![adicionar uma página mestra aninhada à pasta ~/admin.](nested-master-pages-vb/_static/image26.png)](nested-master-pages-vb/_static/image25.png)

**Figura 09**: adicionar uma página mestra aninhada à pasta `~/Admin`. ([Clique para exibir a imagem em tamanho normal](nested-master-pages-vb/_static/image27.png))

Como a página mestra aninhada está associada a uma página mestra com quatro controles ContentPlaceHolder, o Visual Studio adiciona quatro controles de conteúdo à marcação inicial do novo arquivo de página mestra aninhada. Como fizemos nas etapas 2 e 3, adicione um controle ContentPlaceHolder em cada controle de conteúdo, dando a ele o mesmo nome que o controle ContentPlaceHolder da página mestra de nível superior. Além disso, adicione a seguinte marcação ao controle de conteúdo que corresponde ao `MainContent` ContentPlaceHolder:

[!code-html[Main](nested-master-pages-vb/samples/sample8.html)]

Em seguida, defina o `instructions` classe CSS nos arquivos `Styles.css` e `AlternateStyles.css` CSS. As regras CSS a seguir fazem com que os elementos HTML tenham como estilo a classe `instructions` a ser exibida com uma cor de fundo amarela clara e uma borda sólida preta:

[!code-css[Main](nested-master-pages-vb/samples/sample9.css)]

Como essa marcação foi adicionada à página mestra aninhada, ela só aparecerá nessas páginas que usam essa página mestra aninhada (ou seja, as páginas na seção Administração).

Depois de fazer essas adições à sua página mestra aninhada, sua marcação declarativa deve ser semelhante ao seguinte:

[!code-aspx[Main](nested-master-pages-vb/samples/sample10.aspx)]

Observe que cada controle de conteúdo tem um controle ContentPlaceHolder e que os controles ContentPlaceHolder ' `ID` Propriedades recebem os mesmos valores dos controles ContentPlaceHolder correspondentes na página mestra de nível superior. Além disso, a marcação específica da seção de administração aparece no `MainContent` ContentPlaceHolder.

A Figura 10 mostra a `AdminNested.master` página mestra aninhada quando exibida por meio do designer do Visual Studio. Você pode ver as instruções na caixa amarela na parte superior do controle de conteúdo de `MainContent`.

[![a página mestra aninhada estende a página mestra de nível superior para incluir instruções para o administrador.](nested-master-pages-vb/_static/image29.png)](nested-master-pages-vb/_static/image28.png)

**Figura 10**: a página mestra aninhada estende a página mestra de nível superior para incluir instruções para o administrador. ([Clique para exibir a imagem em tamanho normal](nested-master-pages-vb/_static/image30.png))

## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Etapa 5: atualizando as páginas de conteúdo existentes para usar a nova página mestra aninhada

Sempre que adicionarmos uma nova página de conteúdo à seção Administração, precisamos associá-la à página mestra de `AdminNested.master` que acabamos de criar. Mas e quanto às páginas de conteúdo existentes? Atualmente, todas as páginas de conteúdo no site derivam da classe `BasePage`, que define programaticamente a página mestra da página de conteúdo em tempo de execução. Esse não é o comportamento que queremos para as páginas de conteúdo na seção Administração. Em vez disso, queremos que essas páginas de conteúdo sempre usem a página `AdminNested.master`. Será responsabilidade da página mestra aninhada escolher a página de conteúdo de nível superior correta em tempo de execução.

Para obter a melhor maneira de atingir esse comportamento desejado é criar uma nova classe de página de base personalizada chamada `AdminBasePage` que estenda a classe `BasePage`. `AdminBasePage` pode substituir o `SetMasterPageFile` e definir a `MasterPageFile` do objeto `Page` como o valor embutido em código "~/Admin/AdminNested.master". Dessa forma, qualquer página derivada de `AdminBasePage` usará `AdminNested.master`, enquanto qualquer página derivada de `BasePage` terá sua propriedade `MasterPageFile` definida dinamicamente como "~/site.Master" ou "~/Alternate.Master" com base no valor da variável de sessão de `MyMasterPage`.

Comece adicionando um novo arquivo de classe à pasta `App_Code` chamada `AdminBasePage.vb`. `AdminBasePage` estender `BasePage` e, em seguida, substituir o método `SetMasterPageFile`. Nesse método, atribua o `MasterPageFile` valor "~/Admin/AdminNested.master". Depois de fazer essas alterações, seu arquivo de classe deve ser semelhante ao seguinte:

[!code-vb[Main](nested-master-pages-vb/samples/sample11.vb)]

Agora, precisamos que as páginas de conteúdo existentes na seção Administração derivem de `AdminBasePage` em vez de `BasePage`. Vá para o arquivo de classe code-behind para cada página de conteúdo na pasta `~/Admin` e faça essa alteração. Por exemplo, em `~/Admin/Default.aspx` você alteraria a declaração de classe code-behind de:

[!code-vb[Main](nested-master-pages-vb/samples/sample12.vb)]

Para:

[!code-vb[Main](nested-master-pages-vb/samples/sample13.vb)]

A Figura 11 descreve como a página mestra de nível superior (`Site.master` ou `Alternate.master`), a página mestra aninhada (`AdminNested.master`) e as páginas de conteúdo da seção de administração estão relacionadas entre si.

[![a página mestra aninhada define o conteúdo específico para as páginas na seção Administração](nested-master-pages-vb/_static/image32.png)](nested-master-pages-vb/_static/image31.png)

**Figura 11**: a página mestra aninhada define o conteúdo específico para as páginas na seção Administração ([clique para exibir a imagem em tamanho normal](nested-master-pages-vb/_static/image33.png))

## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Etapa 6: espelhamento dos métodos públicos e das propriedades da página mestra

Lembre-se de que as páginas `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` interagem programaticamente com a página mestra: `~/Admin/AddProduct.aspx` chama o método `RefreshRecentProductsGrid` público da página mestra e define sua propriedade `GridMessageText`; `~/Admin/Products.aspx` tem um manipulador de eventos para o evento `PricesDoubled`. No tutorial anterior, criamos uma classe `MustInherit` `BaseMasterPage` que definiu esses membros públicos.

As páginas `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` pressupõem que sua página mestra deriva da classe `BaseMasterPage`. A página de `AdminNested.master`, no entanto, atualmente estende a classe `System.Web.UI.MasterPage`. Como resultado, ao visitar `~/Admin/Products.aspx` um `InvalidCastException` é lançado com a mensagem: "não é possível converter o objeto do tipo ' ASP. admin\_adminnested\_Master ' para o tipo ' BaseMasterPage '."

Para corrigir isso, precisamos ter a classe code-behind `AdminNested.master` estender `BaseMasterPage`. Atualize a declaração de classe code-behind da página mestra aninhada de:

[!code-vb[Main](nested-master-pages-vb/samples/sample14.vb)]

Para:

[!code-vb[Main](nested-master-pages-vb/samples/sample15.vb)]

Ainda não estamos prontos. Precisamos substituir os membros marcados como `MustOverride`, ou seja, `RefreshRecentProductsGrid` e `GridMessageText`. Esses membros são usados pelas páginas mestras de nível superior para atualizar suas interfaces de usuário. (Na verdade, somente a página mestra de `Site.master` usa esses métodos, embora ambas as páginas mestras de nível superior implementem esses métodos, já que ambos estendem `BaseMasterPage`.)

Embora seja necessário implementar esses membros em `AdminNested.master`, todas essas implementações precisam simplesmente chamar o mesmo membro na página mestra de nível superior usada pela página mestra aninhada. Por exemplo, quando uma página de conteúdo na seção de administração chama o método `RefreshRecentProductsGrid` da página mestra aninhada, toda a página mestra aninhada precisa fazer é, por sua vez, chamar `Site.master` ou o método `RefreshRecentProductsGrid` do `Alternate.master`.

Para fazer isso, comece adicionando a seguinte diretiva de `@MasterType` à parte superior de `AdminNested.master`:

[!code-aspx[Main](nested-master-pages-vb/samples/sample16.aspx)]

Lembre-se de que a diretiva `@MasterType` adiciona uma propriedade fortemente tipada à classe code-behind chamada `Master`. Em seguida, substitua os membros `RefreshRecentProductsGrid` e `GridMessageText` e simplesmente delegue a chamada para o método correspondente do `Master`:

[!code-vb[Main](nested-master-pages-vb/samples/sample17.vb)]

Com esse código em vigor, você deve ser capaz de visitar e usar as páginas de conteúdo na seção Administração. A Figura 12 mostra a página de `~/Admin/Products.aspx` quando exibida por meio de um navegador. Como você pode ver, a página inclui a caixa de instruções de administração, que é definida na página mestre aninhada.

[![as páginas de conteúdo na seção Administração incluem instruções na parte superior de cada página](nested-master-pages-vb/_static/image35.png)](nested-master-pages-vb/_static/image34.png)

**Figura 12**: as páginas de conteúdo na seção Administração incluem instruções na parte superior de cada página ([clique para exibir a imagem em tamanho normal](nested-master-pages-vb/_static/image36.png))

## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Etapa 7: usando a página mestra de nível superior apropriada em tempo de execução

Embora todas as páginas de conteúdo na seção Administração sejam totalmente funcionais, todas usam a mesma página mestra de nível superior e ignoram a página mestra selecionada pelo usuário em `ChooseMasterPage.aspx`. Esse comportamento ocorre devido ao fato de que a página mestra aninhada tem sua propriedade `MasterPageFile` definida estaticamente como `Site.master` em sua diretiva de `<%@ Master %>`.

Para usar a página mestra de nível superior selecionada pelo usuário final, precisamos definir a propriedade `MasterPageFile` do `AdminNested.master`como o valor na variável de sessão de `MyMasterPage`. Como definimos as propriedades de `MasterPageFile` das páginas de conteúdo no `BasePage`, você pode imaginar que definiremos a propriedade `MasterPageFile` da página mestra aninhada em `BaseMasterPage` ou na classe code-behind do `AdminNested.master`. No entanto, isso não funcionará, pois precisamos definir a propriedade `MasterPageFile` pelo final do estágio PreInit. A primeira vez que podemos tocar programaticamente no ciclo de vida da página de uma página mestra é o estágio init (que ocorre após o estágio PreInit).

Portanto, precisamos definir a propriedade `MasterPageFile` da página mestra aninhada nas páginas de conteúdo. As únicas páginas de conteúdo que usam a página mestra de `AdminNested.master` derivam de `AdminBasePage`. Portanto, podemos colocar essa lógica lá. Na etapa 5, substituiu o método `SetMasterPageFile`, definindo a propriedade `MasterPageFile` do objeto Page como "~/Admin/AdminNested.master". Atualize `SetMasterPageFile` também defina a propriedade `MasterPageFile` da página mestra para o resultado armazenado na sessão:

[!code-vb[Main](nested-master-pages-vb/samples/sample18.vb)]

O método `GetMasterPageFileFromSession`, que adicionamos à classe `BasePage` no tutorial anterior, retorna o caminho do arquivo da página mestra apropriado com base no valor da variável de sessão.

Com essa alteração no local, a seleção da página mestra do usuário é transferida para a seção Administração. A Figura 13 mostra a mesma página da figura 12, mas depois que o usuário alterou a seleção de página mestra para `Alternate.master`.

[![a página de administração aninhada usar a página mestra de nível superior selecionada pelo usuário](nested-master-pages-vb/_static/image38.png)](nested-master-pages-vb/_static/image37.png)

**Figura 13**: a página de administração aninhada usa a página mestra de nível superior selecionada pelo usuário ([clique para exibir a imagem em tamanho normal](nested-master-pages-vb/_static/image39.png))

## <a name="summary"></a>Resumo

Assim como as páginas de conteúdo podem ser vinculadas a uma página mestra, é possível criar páginas mestras aninhadas fazendo com que uma página mestra filho seja associada a uma página mestra pai. A página mestra filha pode definir controles de conteúdo para cada um dos ContentPlaceHolders pai; em seguida, ele pode adicionar seus próprios controles ContentPlaceHolder (bem como outra marcação) a esses controles de conteúdo. As páginas mestras aninhadas são bastante úteis em grandes aplicativos Web, onde todas as páginas compartilham uma aparência abrangente, mas determinadas seções do site exigem personalizações exclusivas.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Páginas mestras ASP.NET aninhadas](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Dicas para páginas mestras aninhadas e tempo de design VS 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [Suporte a páginas mestras aninhadas do VS 2008](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP. net e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 3,5 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott pode ser contatado em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](specifying-the-master-page-programmatically-vb.md)
