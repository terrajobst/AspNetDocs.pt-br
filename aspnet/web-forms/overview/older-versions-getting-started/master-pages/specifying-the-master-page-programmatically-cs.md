---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
title: Especificando a página mestra programaticamente (C#) | Microsoft Docs
author: rick-anderson
description: Examina a configuração da página mestra da página de conteúdo programaticamente por meio do manipulador de eventos PreInit.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 7c4a3445-2440-4aee-b9fd-779c05e6abb2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
msc.type: authoredcontent
ms.openlocfilehash: 0db23ea05ba001c2bf9fc5330a60a767caa568a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78625485"
---
# <a name="specifying-the-master-page-programmatically-c"></a>Especificar a página mestra programaticamente (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.zip) ou [baixar PDF](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.pdf)

> Examina a configuração da página mestra da página de conteúdo programaticamente por meio do manipulador de eventos PreInit.

## <a name="introduction"></a>Introdução

Como o exemplo inaugural na [*criação de um layout de todo o site usando páginas mestras*](creating-a-site-wide-layout-using-master-pages-cs.md), todas as páginas de conteúdo referenciaram sua página mestra declarativamente por meio do atributo `MasterPageFile` na diretiva `@Page`. Por exemplo, a diretiva de `@Page` a seguir vincula a página de conteúdo à página mestra `Site.master`:

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample1.aspx)]

A [classe`Page`](https://msdn.microsoft.com/library/system.web.ui.page.aspx) no namespace `System.Web.UI` inclui uma [Propriedade`MasterPageFile`](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) que retorna o caminho para a página mestra da página de conteúdo; é essa propriedade definida pela diretiva `@Page`. Essa propriedade também pode ser usada para especificar programaticamente a página mestra da página de conteúdo. Essa abordagem será útil se você quiser atribuir dinamicamente a página mestra com base em fatores externos, como o usuário visitando a página.

Neste tutorial, adicionamos uma segunda página mestra ao nosso site e decidimos dinamicamente qual página mestra usar no tempo de execução.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Etapa 1: uma olhada no ciclo de vida da página

Sempre que uma solicitação chega ao servidor Web para uma página ASP.NET que é uma página de conteúdo, o mecanismo ASP.NET deve fundir os controles de conteúdo da página aos controles ContentPlaceHolder correspondentes da página mestra. Essa fusão cria uma única hierarquia de controle que pode prosseguir pelo ciclo de vida típico da página.

A Figura 1 ilustra essa fusão. A etapa 1 da Figura 1 mostra o conteúdo inicial e as hierarquias de controle da página mestra. Na extremidade final do estágio PreInit, os controles de conteúdo na página são adicionados aos ContentPlaceHolders correspondentes na página mestra (etapa 2). Após essa fusão, a página mestra serve como a raiz da hierarquia de controle com fusível. Essa hierarquia de controle com fusível é adicionada à página para produzir a hierarquia de controle finalizada (etapa 3). O resultado líquido é que a hierarquia de controle da página inclui a hierarquia de controle com fusível.

[![as hierarquias de controle da página mestra e da página de conteúdo são combinadas em conjunto durante o estágio PreInit](specifying-the-master-page-programmatically-cs/_static/image2.png)](specifying-the-master-page-programmatically-cs/_static/image1.png)

**Figura 01**: a página mestra e as hierarquias de controle da página de conteúdo são combinadas em conjunto durante o estágio PreInit ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-cs/_static/image3.png))

## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Etapa 2: definindo a propriedade`MasterPageFile`do código

O que a página mestra partakes nessa fusão depende do valor da propriedade `MasterPageFile` do objeto de `Page`. Definir o atributo `MasterPageFile` na diretiva `@Page` tem o efeito líquido de atribuir a propriedade `MasterPageFile` do `Page`durante o estágio de inicialização, que é o primeiro estágio do ciclo de vida da página. Como alternativa, podemos definir essa propriedade programaticamente. No entanto, é imperativo que essa propriedade seja definida antes que a fusão da Figura 1 ocorra.

No início do estágio PreInit, o objeto `Page` gera seu [evento`PreInit`](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) e chama seu [método`OnPreInit`](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Para definir a página mestra programaticamente, podemos criar um manipulador de eventos para o evento `PreInit` ou substituir o método `OnPreInit`. Vamos examinar ambas as abordagens.

Comece abrindo `Default.aspx.cs`, o arquivo de classe code-behind para a home page do nosso site. Adicione um manipulador de eventos para o evento de `PreInit` da página digitando o seguinte código:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample2.cs)]

Aqui, podemos definir a propriedade `MasterPageFile`. Atualize o código para que ele atribua o valor "~/site.Master" à propriedade `MasterPageFile`.

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample3.cs)]

Se você definir um ponto de interrupção e começar com a depuração, verá que sempre que a página de `Default.aspx` for visitada ou sempre que houver um postback para essa página, o manipulador de eventos de `Page_PreInit` será executado e a propriedade `MasterPageFile` será atribuída a "~/site.Master".

Como alternativa, você pode substituir o método `OnPreInit` da classe `Page` e definir a propriedade `MasterPageFile` lá. Para este exemplo, não vamos definir a página mestra em uma página específica, mas em vez de `BasePage`. Lembre-se de que criamos uma classe de página de base personalizada (`BasePage`) de volta nos tutoriais [*especificando o título, as marcas meta e outros cabeçalhos HTML no tutorial da página mestra*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) . Atualmente `BasePage` substitui o método `OnLoadComplete` da classe `Page`, em que ele define a propriedade `Title` da página com base nos dados do mapa do site. Vamos atualizar `BasePage` também substituir o método `OnPreInit` para especificar programaticamente a página mestra.

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample4.cs)]

Como todas as nossas páginas de conteúdo derivam de `BasePage`, todas elas agora têm sua página mestra programaticamente atribuída. Neste ponto, o manipulador de eventos `PreInit` no `Default.aspx.cs` é supérfluo; Fique à vontade para removê-lo.

### <a name="what-about-thepagedirective"></a>E quanto à diretiva de`@Page`?

O que pode ser um pouco confuso é que as propriedades de `MasterPageFile` das páginas de conteúdo agora estão sendo especificadas em dois locais: programaticamente no método `OnPreInit` da classe `BasePage`, bem como pelo atributo `MasterPageFile` na diretiva `@Page` de cada página de conteúdo.

O primeiro estágio no ciclo de vida da página é o estágio de inicialização. Durante esse estágio, a propriedade `MasterPageFile` do objeto de `Page` é atribuída ao valor do atributo `MasterPageFile` na diretiva `@Page` (se for fornecida). O estágio PreInit segue o estágio de inicialização e está aqui onde definimos de forma programática a propriedade `MasterPageFile` do objeto de `Page`, substituindo assim o valor atribuído da diretiva `@Page`. Como estamos definindo a propriedade de `MasterPageFile` do objeto de `Page` de forma programática, poderíamos remover o atributo `MasterPageFile` da diretiva `@Page` sem afetar a experiência do usuário final. Para convencê-lo, vá em frente e remova o atributo `MasterPageFile` da diretiva `@Page` no `Default.aspx` e visite a página por meio de um navegador. Como esperado, a saída é a mesma de antes de o atributo ser removido.

Se a propriedade de `MasterPageFile` é definida por meio da diretiva de `@Page` ou programaticamente, é irrelevante para a experiência do usuário final. No entanto, o atributo `MasterPageFile` na diretiva `@Page` é usado pelo Visual Studio durante o tempo de design para produzir a exibição WYSIWYG no designer. Se você retornar para `Default.aspx` no Visual Studio e navegar até o designer, verá a mensagem "erro de página mestra: a página tem controles que exigem uma referência de página mestra, mas nenhum é especificado" (consulte a Figura 2).

Em suma, você precisa deixar o atributo `MasterPageFile` na diretiva `@Page` para desfrutar de uma experiência de tempo de design rica no Visual Studio.

[![o Visual Studio usa o atributo MasterPagefile da diretiva de @Page para renderizar a exibição de design](specifying-the-master-page-programmatically-cs/_static/image5.png)](specifying-the-master-page-programmatically-cs/_static/image4.png)

**Figura 02**: o Visual Studio usa o atributo `MasterPageFile` da diretiva de `@Page` para renderizar o modo de exibição de design ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-cs/_static/image6.png))

## <a name="step-3-creating-an-alternative-master-page"></a>Etapa 3: criando uma página mestra alternativa

Como a página mestra de uma página de conteúdo pode ser definida programaticamente em tempo de execução, é possível carregar dinamicamente uma determinada página mestra com base em alguns critérios externos. Essa funcionalidade pode ser útil em situações em que o layout do site precisa variar com base no usuário. Por exemplo, um aplicativo Web do mecanismo de blog pode permitir que seus usuários escolham um layout para seu blog, onde cada layout é associado a uma página mestra diferente. Em tempo de execução, quando um visitante estiver visualizando o blog de um usuário, o aplicativo Web precisará determinar o layout do blog e associar dinamicamente a página mestra correspondente à página de conteúdo.

Vamos examinar como carregar dinamicamente uma página mestra no tempo de execução com base em alguns critérios externos. Nosso site contém atualmente apenas uma página mestra (`Site.master`). Precisamos de outra página mestra para ilustrar a escolha de uma página mestra em tempo de execução. Esta etapa se concentra na criação e configuração da nova página mestra. A etapa 4 examina a determinação da página mestra a ser usada no tempo de execução.

Crie uma nova página mestra na pasta raiz chamada `Alternate.master`. Além disso, adicione uma nova folha de estilo ao site denominada `AlternateStyles.css`.

[![adicionar outra página mestra e arquivo CSS ao site](specifying-the-master-page-programmatically-cs/_static/image8.png)](specifying-the-master-page-programmatically-cs/_static/image7.png)

**Figura 03**: adicionar outra página mestra e arquivo CSS ao site ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-cs/_static/image9.png))

Eu projetei a página mestra de `Alternate.master` para que o título seja exibido na parte superior da página, centralizada e em um plano de fundo azul. Eu despercebii da coluna esquerda e movi esse conteúdo sob o controle `MainContent` ContentPlaceHolder, que agora abrange toda a largura da página. Além disso, eu nixed a lista de lições não ordenadas e a substituai por uma lista horizontal acima `MainContent`. Também atualizei as fontes e cores usadas pela página mestra (e, por extensão, suas páginas de conteúdo). A Figura 4 mostra `Default.aspx` ao usar a página mestra de `Alternate.master`.

> [!NOTE]
> O ASP.NET inclui a capacidade de definir *temas*. Um tema é uma coleção de imagens, arquivos CSS e configurações de propriedade de controle da Web relacionadas a estilo que podem ser aplicadas a uma página em tempo de execução. Os temas são a melhor opção se os layouts do site diferirem apenas nas imagens exibidas e por suas regras de CSS. Se os layouts diferem mais substancialmente, como usar diferentes controles da Web ou ter um layout radicalmente diferente, você precisará usar páginas mestras separadas. Consulte a seção leitura adicional no final deste tutorial para obter mais informações sobre temas.

[![nossas páginas de conteúdo agora podem usar uma nova aparência](specifying-the-master-page-programmatically-cs/_static/image11.png)](specifying-the-master-page-programmatically-cs/_static/image10.png)

**Figura 04**: nossas páginas de conteúdo agora podem usar uma nova aparência ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-cs/_static/image12.png))

Quando a marcação mestre e as páginas de conteúdo são combinadas, a classe `MasterPage` verifica se cada controle de conteúdo na página de conteúdo faz referência a um ContentPlaceHolder na página mestra. Uma exceção será gerada se um controle de conteúdo que faz referência a um ContentPlaceHolder não existente for encontrado. Em outras palavras, é imperativo que a página mestra que está sendo atribuída à página de conteúdo tenha um ContentPlaceHolder para cada controle de conteúdo na página de conteúdo.

A página mestra de `Site.master` inclui quatro controles ContentPlaceHolder:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Algumas das páginas de conteúdo em nosso site incluem apenas um ou dois controles de conteúdo; Outras incluem um controle de conteúdo para cada um dos ContentPlaceHolders disponíveis. Se nossa nova página mestra (`Alternate.master`) pode ser atribuída a essas páginas de conteúdo que têm controles de conteúdo para todos os ContentPlaceHolders em `Site.master`, é essencial que `Alternate.master` também inclua os mesmos controles ContentPlaceHolder que `Site.master`.

Para que sua página mestra de `Alternate.master` seja semelhante a minha (consulte a Figura 4), Comece definindo os estilos da página mestra na folha estilo `AlternateStyles.css`. Adicione as seguintes regras ao `AlternateStyles.css`:

[!code-css[Main](specifying-the-master-page-programmatically-cs/samples/sample5.css)]

Em seguida, adicione a seguinte marcação declarativa a `Alternate.master`. Como você pode ver, `Alternate.master` contém quatro controles ContentPlaceHolder com os mesmos valores de `ID` que os controles ContentPlaceHolder em `Site.master`. Além disso, ele inclui um controle ScriptManager, que é necessário para essas páginas em nosso site que usam a estrutura AJAX do ASP.NET.

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Testando a nova página mestra

Para testar essa nova página mestra, atualize o método `OnPreInit` da classe `BasePage` para que a propriedade `MasterPageFile` receba o valor "~/Alternate.Master" e visite o site. Cada página deve funcionar sem erro, exceto duas: `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx`. Adicionar um produto ao DetailsView no `~/Admin/AddProduct.aspx` resulta em um `NullReferenceException` da linha de código que tenta definir a propriedade `GridMessageText` da página mestra. Ao visitar `~/Admin/Products.aspx` um `InvalidCastException` é gerado no carregamento da página com a mensagem: "não é possível converter o objeto do tipo ' ASP. Alternate\_mestre ' para o tipo ' ASP. site\_Master '."

Esses erros ocorrem porque a classe code-behind `Site.master` inclui eventos públicos, propriedades e métodos que não são definidos no `Alternate.master`. A parte de marcação dessas duas páginas tem uma diretiva `@MasterType` que faz referência à página mestra `Site.master`.

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample7.aspx)]

Além disso, o manipulador de eventos de `ItemInserted` do DetailsView em `~/Admin/AddProduct.aspx` inclui o código que converte a propriedade de `Page.Master` de tipo flexível em um objeto de tipos `Site`. A diretiva de `@MasterType` (usada dessa forma) e a conversão no manipulador de eventos `ItemInserted` acopla rigidamente as páginas `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` à página mestra de `Site.master`.

Para interromper esse acoplamento rígido, podemos ter `Site.master` e `Alternate.master` derivar de uma classe base comum que contém definições para os membros públicos. Depois disso, podemos atualizar a diretiva `@MasterType` para fazer referência a esse tipo base comum.

### <a name="creating-a-custom-base-master-page-class"></a>Criando uma classe de página mestra de base personalizada

Adicione um novo arquivo de classe à pasta `App_Code` chamada `BaseMasterPage.cs` e faça com que ela derive de `System.Web.UI.MasterPage`. Precisamos definir o método `RefreshRecentProductsGrid` e a propriedade `GridMessageText` em `BaseMasterPage`, mas não podemos simplesmente movê-los para o `Site.master` porque esses membros trabalham com controles da Web que são específicos para a `Site.master` página mestra (o `RecentProducts` GridView e `GridMessage` rótulo).

O que precisamos fazer é configurar `BaseMasterPage` de forma que esses membros sejam definidos lá, mas que são realmente implementados pelas classes derivadas de `BaseMasterPage`(`Site.master` e `Alternate.master`). Esse tipo de herança é possível marcando a classe e seus membros como `abstract`. Em suma, adicionar a palavra-chave `abstract` a esses dois membros anuncia que `BaseMasterPage` não implementou `RefreshRecentProductsGrid` e `GridMessageText`, mas que suas classes derivadas irão.

Também precisamos definir o evento `PricesDoubled` em `BaseMasterPage` e fornecer um meio pelas classes derivadas para gerar o evento. O padrão usado no .NET Framework para facilitar esse comportamento é criar um evento público na classe base e adicionar um método protegido `virtual` chamado `OnEventName`. Classes derivadas podem então chamar esse método para gerar o evento ou pode substituí-lo para executar o código imediatamente antes ou depois que o evento é gerado.

Atualize sua classe de `BaseMasterPage` para que ela contenha o seguinte código:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample8.cs)]

Em seguida, vá para a classe code-behind `Site.master` e faça com que ela derive de `BaseMasterPage`. Como `BaseMasterPage` é `abstract` precisamos substituir os membros `abstract` aqui no `Site.master`. Adicione a palavra-chave `override` às definições de método e propriedade. Além disso, atualize o código que gera o evento `PricesDoubled` no manipulador de eventos `Click` do botão `DoublePrice` com uma chamada para o método `OnPricesDoubled` da classe base.

Depois dessas modificações, a classe code-behind `Site.master` deve conter o seguinte código:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample9.cs)]

Também precisamos atualizar a classe code-behind de `Alternate.master`para derivar de `BaseMasterPage` e substituir os dois membros `abstract`. Mas como `Alternate.master` não contém um GridView que lista os produtos mais recentes nem um rótulo que exibe uma mensagem depois que um novo produto é adicionado ao banco de dados, esses métodos não precisam fazer nada.

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample10.cs)]

### <a name="referencing-the-base-master-page-class"></a>Referenciando a classe de página mestra de base

Agora que concluímos a classe `BaseMasterPage` e temos duas páginas mestras que a estendem, nossa etapa final é atualizar as páginas `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` para fazer referência a esse tipo comum. Comece alterando a diretiva `@MasterType` em ambas as páginas de:

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample11.aspx)]

Para:

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample12.aspx)]

Em vez de referenciar um caminho de arquivo, a propriedade `@MasterType` agora faz referência ao tipo base (`BaseMasterPage`). Consequentemente, a propriedade de `Master` fortemente tipada usada em classes code-behind de páginas agora é do tipo `BaseMasterPage` (em vez do tipo `Site`). Com essa alteração em vigor, reveja `~/Admin/Products.aspx`. Anteriormente, isso resultou em um erro de conversão porque a página está configurada para usar a página mestra de `Alternate.master`, mas a diretiva `@MasterType` referenciou o arquivo `Site.master`. Mas agora a página é renderizada sem erro. Isso ocorre porque a página mestra de `Alternate.master` pode ser convertida em um objeto do tipo `BaseMasterPage` (já que ela a estende).

Há uma pequena alteração que precisa ser feita em `~/Admin/AddProduct.aspx`. O manipulador de eventos de `ItemInserted` do controle DetailsView usa a propriedade `Master` fortemente tipada e a propriedade `Page.Master` com rigidez de tipos. Corrigimos a referência fortemente tipada quando atualizamos a diretiva `@MasterType`, mas ainda precisamos atualizar a referência com rigidez de tipos. Substitua a seguinte linha de código:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample13.cs)]

Com o seguinte, que converte `Page.Master` para o tipo base:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample14.cs)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Etapa 4: determinando qual página mestra associar às páginas de conteúdo

Nossa classe de `BasePage` atualmente define todas as propriedades de `MasterPageFile` de páginas de conteúdo para um valor embutido no estágio PreInit do ciclo de vida da página. Podemos atualizar esse código para basear a página mestra em algum fator externo. Talvez a página mestra a ser carregada dependa das preferências do usuário conectado no momento. Nesse caso, seria necessário escrever código no método `OnPreInit` no `BasePage` que pesquisa as preferências da página mestra do usuário que está visitando no momento.

Vamos criar uma página da Web que permita ao usuário escolher qual página mestra usar-`Site.master` ou `Alternate.master`-e salvar essa opção em uma variável de sessão. Comece criando uma nova página da Web no diretório raiz chamado `ChooseMasterPage.aspx`. Ao criar esta página (ou quaisquer outras páginas de conteúdo daqui em diante), você não precisa associá-la a uma página mestra porque a página mestra está definida programaticamente no `BasePage`. No entanto, se você não associar a nova página a uma página mestra, a marcação declarativa padrão da nova página conterá um formulário da Web e outro conteúdo fornecido pela página mestra. Você precisará substituir manualmente essa marcação pelos controles de conteúdo apropriados. Por esse motivo, acho mais fácil associar a nova página ASP.NET a uma página mestra.

> [!NOTE]
> Como `Site.master` e `Alternate.master` têm o mesmo conjunto de controles ContentPlaceHolder, não importa qual página mestra você escolhe ao criar a página novo conteúdo. Para fins de consistência, sugiro o uso de `Site.master`.

[![adicionar uma nova página de conteúdo ao site](specifying-the-master-page-programmatically-cs/_static/image14.png)](specifying-the-master-page-programmatically-cs/_static/image13.png)

**Figura 05**: adicionar uma nova página de conteúdo ao site ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-cs/_static/image15.png))

Atualize o arquivo de `Web.sitemap` para incluir uma entrada para esta lição. Adicione a seguinte marcação abaixo do `<siteMapNode>` para as páginas mestras e a lição do ASP.NET AJAX:

[!code-xml[Main](specifying-the-master-page-programmatically-cs/samples/sample15.xml)]

Antes de adicionar qualquer conteúdo à página de `ChooseMasterPage.aspx` Reserve um tempo para atualizar a classe code-behind da página para que ela seja derivada de `BasePage` (em vez de `System.Web.UI.Page`). Em seguida, adicione um controle DropDownList à página, defina sua propriedade `ID` como `MasterPageChoice`e adicione duas ListItems com os valores de `Text` de "~/site.Master" e "~/Alternate.Master".

Adicione um controle Web de botão à página e defina suas propriedades `ID` e `Text` como `SaveLayout` e "salvar layout Choice", respectivamente. Neste ponto, a marcação declarativa de sua página deve ser semelhante ao seguinte:

[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample16.aspx)]

Quando a página é visitada pela primeira vez, precisamos exibir a opção de página mestra selecionada no momento do usuário. Crie um manipulador de eventos `Page_Load` e adicione o seguinte código:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample17.cs)]

O código acima é executado somente na primeira página de visita (e não em postbacks subsequentes). Ele verifica primeiro se a variável de sessão `MyMasterPage` existe. Se tiver, ele tentará localizar o ListItem correspondente no `MasterPageChoice` DropDownList. Se um ListItem correspondente for encontrado, sua propriedade `Selected` será definida como `true`.

Também precisamos de um código que salve a opção do usuário na variável de sessão `MyMasterPage`. Crie um manipulador de eventos para o evento de `Click` do botão de `SaveLayout` e adicione o seguinte código:

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample18.cs)]

> [!NOTE]
> No momento em que o manipulador de eventos de `Click` é executado no postback, a página mestra já foi selecionada. Portanto, a seleção de lista suspensa do usuário não estará em vigor até que a próxima página seja visitada. O `Response.Redirect` força o navegador a solicitar novamente `ChooseMasterPage.aspx`.

Com a página de `ChooseMasterPage.aspx` concluída, nossa tarefa final é ter `BasePage` atribuir a propriedade `MasterPageFile` com base no valor da variável de sessão `MyMasterPage`. Se a variável de sessão não estiver definida, `BasePage` padrão será `Site.master`.

[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample19.cs)]

> [!NOTE]
> Movi o código que atribui a propriedade `MasterPageFile` do objeto de `Page` do manipulador de eventos `OnPreInit` e em dois métodos separados. Esse primeiro método, `SetMasterPageFile`, atribui a propriedade `MasterPageFile` ao valor retornado pelo segundo método, `GetMasterPageFileFromSession`. Fiz com que o método `SetMasterPageFile` `virtual` de forma que as classes futuras que estendem `BasePage` possam, opcionalmente, substituí-lo para implementar a lógica personalizada, se necessário. Veremos um exemplo de substituição da propriedade `SetMasterPageFile` do `BasePage`no próximo tutorial.

Com esse código em vigor, visite a página `ChooseMasterPage.aspx`. Inicialmente, a página mestra de `Site.master` é selecionada (veja a Figura 6), mas o usuário pode escolher uma página mestra diferente na lista suspensa.

[![páginas de conteúdo são exibidas usando a página mestra site. Master](specifying-the-master-page-programmatically-cs/_static/image17.png)](specifying-the-master-page-programmatically-cs/_static/image16.png)

**Figura 06**: as páginas de conteúdo são exibidas usando o `Site.master` página mestra ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-cs/_static/image18.png))

[![páginas de conteúdo são exibidas agora usando a página mestra. Master mestre](specifying-the-master-page-programmatically-cs/_static/image20.png)](specifying-the-master-page-programmatically-cs/_static/image19.png)

**Figura 07**: as páginas de conteúdo agora são exibidas usando a página mestra de `Alternate.master` ([clique para exibir a imagem em tamanho normal](specifying-the-master-page-programmatically-cs/_static/image21.png))

## <a name="summary"></a>Resumo

Quando uma página de conteúdo é visitada, seus controles de conteúdo são fundidos com os controles ContentPlaceHolder da página mestra. A página mestra da página de conteúdo é indicada pela propriedade `MasterPageFile` da classe `Page`, que é atribuída ao atributo `MasterPageFile` da diretiva de `@Page` durante o estágio de inicialização. Como mostra este tutorial, podemos atribuir um valor à propriedade `MasterPageFile`, desde que possamos fazer isso antes do final do estágio PreInit. Ser capaz de especificar programaticamente a página mestra abre a porta para cenários mais avançados, como associar dinamicamente uma página de conteúdo a uma página mestra com base em fatores externos.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Diagrama de ciclo de vida da página ASP.NET](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Visão geral do ciclo de vida da página ASP.NET](https://msdn.microsoft.com/library/ms178472.aspx)
- [Visão geral de temas e capas do ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Páginas mestras: dicas, truques e armadilhas](http://www.odetocode.com/articles/450.aspx)
- [Temas no ASP.NET](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP. net e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 3,5 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott pode ser contatado em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Banerjee. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-pages-and-asp-net-ajax-cs.md)
> [Próximo](nested-master-pages-cs.md)
