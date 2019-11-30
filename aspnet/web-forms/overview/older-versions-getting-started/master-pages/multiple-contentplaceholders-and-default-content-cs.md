---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
title: Vários ContentPlaceHolders e conteúdo padrão (C#) | Microsoft Docs
author: rick-anderson
description: Examina como adicionar vários contentores de conteúdo a uma página mestra, bem como especificar o conteúdo padrão nos contentores de conteúdo.
ms.author: riande
ms.date: 05/21/2008
ms.assetid: b9b9798b-027d-46cc-9636-473378e437ac
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-cs
msc.type: authoredcontent
ms.openlocfilehash: e902bcae05c0e7976a20293f2b01e5f2e2bee13a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74639413"
---
# <a name="multiple-contentplaceholders-and-default-content-c"></a>Vários ContentPlaceHolders e conteúdo padrão (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_CS.zip) ou [baixar PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_CS.pdf)

> Examina como adicionar vários contentores de conteúdo a uma página mestra, bem como especificar o conteúdo padrão nos contentores de conteúdo.

## <a name="introduction"></a>Introdução

No tutorial anterior, examinamos como as páginas mestras permitem que os desenvolvedores de ASP.NET criem um layout consistente em todo o site. As páginas mestras definem a marcação que é comum a todas as suas páginas de conteúdo e regiões que são personalizáveis de acordo com a página. No tutorial anterior, criamos uma página mestra simples (`Site.master`) e duas páginas de conteúdo (`Default.aspx` e `About.aspx`). Nossa página mestra consistiu em dois ContentPlaceHolders chamados `head` e `MainContent`, que estavam localizados no elemento `<head>` e no formulário da Web, respectivamente. Embora as páginas de conteúdo tenham dois controles de conteúdo, especificamos apenas a marcação para aquela correspondente a `MainContent`.

Conforme evidenciado pelos dois controles ContentPlaceHolder no `Site.master`, uma página mestra pode conter vários ContentPlaceHolders. E o que é mais, a página mestra pode especificar a marcação padrão para os controles ContentPlaceHolder. Uma página de conteúdo, em seguida, pode especificar opcionalmente sua própria marcação ou usar a marcação padrão. Neste tutorial, examinamos o uso de vários controles de conteúdo na página mestra e veremos como definir a marcação padrão nos controles ContentPlaceHolder.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Etapa 1: adicionar controles ContentPlaceHolder adicionais à página mestra

Muitos designs de site contêm várias áreas na tela que são personalizadas de acordo com a página. `Site.master`, a página mestra que criamos no tutorial anterior, contém um único ContentPlaceHolder dentro do formulário da Web chamado `MainContent`. Especificamente, esse ContentPlaceHolder está localizado dentro do elemento `mainContent` `<div>`.

A Figura 1 mostra `Default.aspx` quando visualizado por meio de um navegador. A região circulada em vermelho é a marcação específica da página correspondente ao `MainContent`.

[![região circulada mostra a área atualmente personalizável em uma base página por página](multiple-contentplaceholders-and-default-content-cs/_static/image2.png)](multiple-contentplaceholders-and-default-content-cs/_static/image1.png)

**Figura 01**: a região circulada mostra a área atualmente personalizável em uma base página a página ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-cs/_static/image3.png))

Imagine que, além da região mostrada na Figura 1, também precisamos adicionar itens específicos da página à coluna à esquerda sob as seções lições e notícias. Para fazer isso, adicionamos outro controle ContentPlaceHolder à página mestra. Para acompanhar, abra o `Site.master` página mestra no Visual Web Developer e arraste um controle ContentPlaceHolder da caixa de ferramentas para o designer após a seção notícias. Defina o `ID` do ContentPlaceHolder como `LeftColumnContent`.

[![adicionar um controle ContentPlaceHolder à coluna esquerda da página mestra](multiple-contentplaceholders-and-default-content-cs/_static/image5.png)](multiple-contentplaceholders-and-default-content-cs/_static/image4.png)

**Figura 02**: adicionar um controle ContentPlaceHolder à coluna esquerda da página mestra ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-cs/_static/image6.png))

Com a adição do `LeftColumnContent` ContentPlaceHolder à página mestra, podemos definir o conteúdo dessa região em uma base página por página, incluindo um controle de conteúdo na página cujo `ContentPlaceHolderID` está definido como `LeftColumnContent`. Examinamos esse processo na etapa 2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Etapa 2: definindo o conteúdo para o novo ContentPlaceHolder nas páginas de conteúdo

Ao adicionar uma nova página de conteúdo ao site, o Visual Web Developer cria automaticamente um controle de conteúdo na página para cada ContentPlaceHolder na página mestra selecionada. Depois de adicionar um `LeftColumnContent` ContentPlaceHolder à nossa página mestra na etapa 1, as novas páginas ASP.NET agora terão três controles de conteúdo.

Para ilustrar isso, adicione uma nova página de conteúdo ao diretório raiz chamado `MultipleContentPlaceHolders.aspx` que está associado à `Site.master` página mestra. O Visual Web Developer cria essa página com a seguinte marcação declarativa:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample1.aspx)]

Insira algum conteúdo no controle de conteúdo que faz referência a `MainContent` ContentPlaceHolders (`Content2`). Em seguida, adicione a seguinte marcação ao controle de conteúdo de `Content3` (que faz referência ao `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample2.html)]

Depois de adicionar essa marcação, visite a página por meio de um navegador. Como mostra a Figura 3, a marcação colocada no controle de conteúdo de `Content3` é exibida na coluna à esquerda abaixo da seção notícias (circulado em vermelho). A marcação colocada em `Content2` é exibida na parte direita da página (circulada em azul).

[![a coluna à esquerda agora inclui conteúdo específico da página abaixo da seção notícias](multiple-contentplaceholders-and-default-content-cs/_static/image8.png)](multiple-contentplaceholders-and-default-content-cs/_static/image7.png)

**Figura 03**: a coluna à esquerda agora inclui conteúdo específico da página abaixo da seção notícias ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-cs/_static/image9.png))

### <a name="defining-content-in-existing-content-pages"></a>Definindo o conteúdo em páginas de conteúdo existentes

A criação de uma nova página de conteúdo incorpora automaticamente o controle ContentPlaceHolder que adicionamos na etapa 1. Mas nossas duas páginas de conteúdo existentes – `About.aspx` e `Default.aspx`-não têm um controle de conteúdo para o `LeftColumnContent` ContentPlaceHolder. Para especificar o conteúdo para este ContentPlaceHolder nessas duas páginas existentes, precisamos adicionar um controle de conteúdo por si só.

Ao contrário da maioria dos controles da Web ASP.NET, a caixa de ferramentas do Visual Web Developer não inclui um item de controle de conteúdo. Podemos digitar manualmente a marcação declarativa do controle de conteúdo na exibição da fonte, mas uma abordagem mais fácil e rápida é usar o modo de exibição de Design. Abra a página `About.aspx` e alterne para o modo de exibição de Design. Como a Figura 4 ilustra, o `LeftColumnContent` ContentPlaceHolder aparece na modo de exibição de Design; Se você passar o mouse sobre ele, o título exibido lerá: "LeftColumnContent (Master)". A inclusão de "Master" no título indica que não há nenhum controle de conteúdo definido na página para este ContentPlaceHolder. Se existir um controle de conteúdo para o ContentPlaceHolder, como no caso de `MainContent`, o título lerá: "*ContentPlaceHolderID* (personalizado)".

Para adicionar um controle de conteúdo para o `LeftColumnContent` ContentPlaceHolder para `About.aspx`, expanda a marca inteligente do ContentPlaceHolder e clique no link criar conteúdo personalizado.

[![o modo de exibição de design para about. aspx mostra o LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image11.png)](multiple-contentplaceholders-and-default-content-cs/_static/image10.png)

**Figura 04**: o modo de exibição de Design para `About.aspx` mostra o `LeftColumnContent` ContentPlaceHolder ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-cs/_static/image12.png))

Clicar no link criar conteúdo personalizado gera o controle de conteúdo necessário na página e define sua propriedade `ContentPlaceHolderID` como o `ID`de ContentPlaceHolder. Por exemplo, clicar no link criar conteúdo personalizado para `LeftColumnContent` região no `About.aspx` adiciona a seguinte marcação declarativa à página:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>Omitindo controles de conteúdo

O ASP.NET não exige que todas as páginas de conteúdo incluam controles de conteúdo para cada um e todos os ContentPlaceHolder definidos na página mestra. Se um controle de conteúdo for omitido, o mecanismo ASP.NET usará a marcação definida no ContentPlaceHolder na página mestra. Essa marcação é conhecida como o *conteúdo padrão* do ContentPlaceHolder e é útil em cenários em que o conteúdo de alguma região é comum entre a maioria das páginas, mas precisa ser personalizado para um pequeno número de páginas. A etapa 3 explora a especificação do conteúdo padrão na página mestra.

Atualmente, `Default.aspx` contém dois controles de conteúdo para o `head` e `MainContent` ContentPlaceHolders; Ele não tem um controle de conteúdo para `LeftColumnContent`. Consequentemente, quando `Default.aspx` é renderizado, o conteúdo padrão do `LeftColumnContent` ContentPlaceHolder é usado. Como ainda definimos qualquer conteúdo padrão para esse ContentPlaceHolder, o efeito líquido é que nenhuma marcação é emitida para essa região. Para verificar esse comportamento, visite `Default.aspx` por meio de um navegador. Como mostra a Figura 5, nenhuma marcação é emitida na coluna esquerda abaixo da seção notícias.

[![nenhum conteúdo é renderizado para o LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image14.png)](multiple-contentplaceholders-and-default-content-cs/_static/image13.png)

**Figura 05**: nenhum conteúdo é renderizado para o `LeftColumnContent` ContentPlaceHolder ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-cs/_static/image15.png))

## <a name="step-3-specifying-default-content-in-the-master-page"></a>Etapa 3: especificando o conteúdo padrão na página mestra

Alguns designs de site incluem uma região cujo conteúdo é o mesmo para todas as páginas no site, exceto para uma ou duas exceções. Considere um site que dê suporte a contas de usuário. Esse site requer uma página de logon na qual os visitantes podem inserir suas credenciais para entrar no site. Para agilizar o processo de entrada, os designers do site podem incluir caixas de entrada de nome de usuário e senha no canto superior esquerdo de cada página para permitir que os usuários entrem sem precisar visitar explicitamente a página de logon. Embora essas caixas de entrada de nome de usuário e senha sejam úteis na maioria das páginas, elas são redundantes na página de logon, que já contém TextBoxes para as credenciais do usuário.

Para implementar esse design, você pode criar um controle ContentPlaceHolder no canto superior esquerdo da página mestra. Cada página que precisava exibir as caixas de entrada nome de usuário e senha no canto superior esquerdo criaria um controle de conteúdo para este ContentPlaceHolder e adicionaria a interface necessária. A página de logon, por outro lado, omitiria adicionar um controle de conteúdo para este ContentPlaceHolder ou criaria um controle de conteúdo sem marcação definida. A desvantagem dessa abordagem é que precisamos nos lembrar de adicionar as caixas de entrada nome de usuário e senha a cada página que adicionamos ao site (exceto para a página de logon). Isso está solicitando problemas. É provável que você se esqueça de adicionar essas caixas de texto a uma página ou a duas ou, pior, talvez não implemente a interface corretamente (talvez adicionando apenas uma caixa de texto em vez de duas).

Uma solução melhor é definir as caixas de entrada nome de usuário e senha como o conteúdo padrão do ContentPlaceHolder. Ao fazer isso, precisamos apenas substituir esse conteúdo padrão nessas poucas páginas que não exibem as caixas de caixa de nome de usuário e senha (a página de logon, por exemplo). Para ilustrar como especificar o conteúdo padrão para um controle ContentPlaceHolder, vamos implementar o cenário que acabou de ser discutido.

> [!NOTE]
> O restante deste tutorial atualiza nosso site para incluir uma interface de logon na coluna à esquerda de todas as páginas, mas a página de logon. No entanto, este tutorial não examina como configurar o site para dar suporte a contas de usuário. Para obter mais informações sobre este tópico, consulte meus tutoriais de [autenticação, autorização, contas de usuário e funções de formulários](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) .

### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Adicionando um ContentPlaceHolder e especificando seu conteúdo padrão

Abra a página `Site.master` mestra e adicione a seguinte marcação à coluna esquerda entre a seção `DateDisplay` rótulo e lições:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample4.aspx)]

Depois de adicionar essa marcação, a modo de exibição de Design da página mestra deve ser semelhante à figura 6.

[![a página mestra inclui um controle de logon](multiple-contentplaceholders-and-default-content-cs/_static/image17.png)](multiple-contentplaceholders-and-default-content-cs/_static/image16.png)

**Figura 06**: a página mestra inclui um controle de logon ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-cs/_static/image18.png))

Este ContentPlaceHolder, `QuickLoginUI`, tem um controle de logon na Web como seu conteúdo padrão. O controle de logon exibe uma interface do usuário que solicita ao usuário seu nome de usuários e senha junto com um botão de logon. Ao clicar no botão fazer logon, o controle de logon valida internamente as credenciais do usuário em relação à API de associação. Para usar esse controle de logon na prática, você precisa configurar seu site para usar a associação. Este tópico está além do escopo deste tutorial; consulte os tutoriais sobre [autenticação, autorização, contas de usuário e funções](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) de meus formulários para obter mais informações sobre como criar um aplicativo Web que dê suporte a contas de usuário.

Sinta-se à vontade para personalizar o comportamento ou a aparência do controle de logon. Defini duas de suas propriedades: `TitleText` e `FailureAction`. O valor da propriedade `TitleText`, que usa como padrão "logon", é exibido na parte superior da interface do usuário do controle. Defini essa propriedade para que ela exiba o texto "entrar" como um elemento `<h3>`. A propriedade `FailureAction` indica o que fazer se as credenciais do usuário forem inválidas. O padrão é um valor de `Refresh`, que deixa o usuário na mesma página e exibe uma mensagem de falha dentro do controle de logon. Alterei-o para `RedirectToLoginPage`, que envia o usuário para a página de logon no caso de credenciais inválidas. Prefiro enviar o usuário para a página de logon quando um usuário tenta fazer logon de alguma outra página, mas falha, porque a página de logon pode conter instruções e opções adicionais que não se ajustariam facilmente à coluna à esquerda. Por exemplo, a página de logon pode incluir opções para recuperar uma senha esquecida ou para criar uma nova conta.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Criando a página de logon e substituindo o conteúdo padrão

Com a página mestra concluída, nossa próxima etapa é criar a página de logon. Adicione uma página do ASP.NET ao diretório raiz do seu site chamado `Login.aspx`, associando-o à `Site.master` página mestra. Isso criará uma página com quatro controles de conteúdo, um para cada um dos ContentPlaceHolders definidos em `Site.master`.

Adicione um controle de logon ao controle de conteúdo `MainContent`. Da mesma forma, sinta-se à vontade para adicionar qualquer conteúdo à região de `LeftColumnContent`. No entanto, lembre-se de deixar o controle de conteúdo para o `QuickLoginUI` ContentPlaceHolder vazio. Isso garantirá que o controle de logon não apareça na coluna à esquerda da página de logon.

Depois de definir o conteúdo para as regiões de `MainContent` e `LeftColumnContent`, a marcação declarativa da sua página de logon deve ser semelhante ao seguinte:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-cs/samples/sample5.aspx)]

A Figura 7 mostra essa página quando exibida por meio de um navegador. Como essa página especifica um controle de conteúdo para o `QuickLoginUI` ContentPlaceHolder, ela substitui o conteúdo padrão especificado na página mestra. O efeito de rede é que o controle de logon exibido na modo de exibição de Design da página mestra (consulte a Figura 6) não é renderizado nesta página.

[![a página de logon repressiona o conteúdo padrão do QuickLoginUI ContentPlaceHolder](multiple-contentplaceholders-and-default-content-cs/_static/image20.png)](multiple-contentplaceholders-and-default-content-cs/_static/image19.png)

**Figura 07**: a página de logon repressiona o conteúdo padrão do `QuickLoginUI` ContentPlaceHolder ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-cs/_static/image21.png))

### <a name="using-the-default-content-in-new-pages"></a>Usando o conteúdo padrão em novas páginas

Queremos mostrar o controle de logon na coluna à esquerda de todas as páginas, exceto a página de logon. Para conseguir isso, todas as páginas de conteúdo, exceto a página de logon, devem omitir um controle de conteúdo para o `QuickLoginUI` ContentPlaceHolder. Ao omitir um controle de conteúdo, o conteúdo padrão do ContentPlaceHolder será usado em vez disso.

Nossas páginas de conteúdo existentes-`Default.aspx`, `About.aspx`e `MultipleContentPlaceHolders.aspx`-não incluem um controle de conteúdo para `QuickLoginUI` porque eles foram criados antes de adicionarmos esse controle ContentPlaceHolder à página mestra. Portanto, essas páginas existentes não precisam ser atualizadas. No entanto, as novas páginas adicionadas ao site incluem um controle de conteúdo para o `QuickLoginUI` ContentPlaceHolder, por padrão. Portanto, precisamos nos lembrar de remover esses controles de conteúdo sempre que adicionarmos uma nova página de conteúdo (a menos que queiramos substituir o conteúdo padrão do ContentPlaceHolder, como no caso da página de logon).

Para remover o controle de conteúdo, você pode excluir manualmente sua marcação declarativa da exibição de origem ou, na modo de exibição de Design, escolher o link de conteúdo padrão para o mestre em sua marca inteligente. Qualquer uma das abordagens remove o controle de conteúdo da página e produz o mesmo efeito líquido.

A Figura 8 mostra `Default.aspx` quando visualizado por meio de um navegador. Lembre-se de que `Default.aspx` tem apenas dois controles de conteúdo especificados em sua marcação declarativa-um para `head` e outro para `MainContent`. Como resultado, o conteúdo padrão para o `LeftColumnContent` e `QuickLoginUI` ContentPlaceHolders são exibidos.

[![o conteúdo padrão para os ContentPlaceHolders LeftColumnContent e QuickLoginUI são exibidos](multiple-contentplaceholders-and-default-content-cs/_static/image23.png)](multiple-contentplaceholders-and-default-content-cs/_static/image22.png)

**Figura 08**: o conteúdo padrão para o `LeftColumnContent` e `QuickLoginUI` ContentPlaceHolders são exibidos ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-cs/_static/image24.png))

## <a name="summary"></a>Resumo

O modelo de página mestra ASP.NET permite um número arbitrário de ContentPlaceHolders na página mestra. Além disso, os ContentPlaceHolders incluem conteúdo padrão, que é emitido no caso de não haver nenhum controle de conteúdo correspondente na página de conteúdo. Neste tutorial, vimos como incluir controles ContentPlaceHolder adicionais na página mestra e como definir controles de conteúdo para esses novos ContentPlaceHolders em páginas de ASP.NET novas e existentes. Também vimos a especificação do conteúdo padrão em um ContentPlaceHolder, que é útil em cenários em que apenas uma minoria de páginas precisa personalizar o conteúdo padronizado de outra forma em uma determinada região.

No próximo tutorial, examinaremos o `head` ContentPlaceHolder mais detalhadamente, vendo como definir de forma declarativa e programática o título, as marcas meta e outros cabeçalhos HTML em uma base página a página.

Boa programação!

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP. net e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 3,5 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott pode ser contatado em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Banerjee. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](creating-a-site-wide-layout-using-master-pages-cs.md)
> [Próximo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
