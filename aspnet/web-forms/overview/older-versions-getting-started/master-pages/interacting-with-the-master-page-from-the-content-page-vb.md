---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
title: Interagindo com a página mestra da página de conteúdo (VB) | Microsoft Docs
author: rick-anderson
description: Examina como chamar métodos, definir propriedades, etc. da página mestra a partir do código na página conteúdo.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 081fe010-ba0f-4e7d-b4ba-774840b601c2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
msc.type: authoredcontent
ms.openlocfilehash: fe1f7b80b26ff25c1ce744e4f823e3fb35eea074
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74577214"
---
# <a name="interacting-with-the-master-page-from-the-content-page-vb"></a>Interagir com a página de conteúdo através da página mestra (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_VB.zip) ou [baixar PDF](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_VB.pdf)

> Examina como chamar métodos, definir propriedades, etc. da página mestra a partir do código na página conteúdo.

## <a name="introduction"></a>Introdução

No decorrer dos cinco últimos tutoriais, vimos como criar uma página mestra, definir regiões de conteúdo, associar páginas ASP.NET a uma página mestra e definir conteúdo específico de página. Quando um visitante solicita uma página de conteúdo específica, a marcação de conteúdo e páginas mestras é fundida em tempo de execução, resultando na renderização de uma hierarquia de controle unificada. Portanto, já vimos uma maneira na qual a página mestra e uma de suas páginas de conteúdo podem interagir: a página de conteúdo verifica a marcação para transfundir nos controles ContentPlaceHolder da página mestra.

O que ainda estamos examinando é como a página mestra e a página de conteúdo podem interagir programaticamente. Além de definir a marcação para os controles ContentPlaceHolder da página mestra, uma página de conteúdo também pode atribuir valores às propriedades públicas de sua página mestra e invocar seus métodos públicos. Da mesma forma, uma página mestra pode interagir com suas páginas de conteúdo. Embora a interação programática entre um mestre e uma página de conteúdo seja menos comum do que a interação entre suas marcações declarativas, há muitos cenários em que essa interação programática é necessária.

Neste tutorial, examinaremos como uma página de conteúdo pode interagir de forma programática com sua página mestra; no próximo tutorial, veremos como a página mestra pode interagir de forma semelhante com suas páginas de conteúdo.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Exemplos de interação programática entre uma página de conteúdo e sua página mestra

Quando uma determinada região de uma página precisa ser configurada em uma base página por página, usamos um controle ContentPlaceHolder. Mas e quanto às situações em que a maioria das páginas precisa emitir uma determinada saída, mas um pequeno número de páginas precisa personalizá-la para mostrar algo mais? Um exemplo, que examinamos no tutorial [*vários ContentPlaceHolders e conteúdo padrão*](multiple-contentplaceholders-and-default-content-vb.md) , envolve a exibição de uma interface de logon em cada página. Embora a maioria das páginas deva incluir uma interface de logon, ela deve ser suprimida por algumas páginas, como: a página de logon principal (`Login.aspx`); a página Criar conta; e outras páginas que só podem ser acessadas por usuários autenticados. O tutorial [*vários ContentPlaceHolders e conteúdo padrão*](multiple-contentplaceholders-and-default-content-vb.md) mostrou como definir o conteúdo padrão para um ContentPlaceHolder na página mestra e como substituí-lo nessas páginas em que o conteúdo padrão não era desejado.

Outra opção é criar uma propriedade pública ou um método dentro da página mestra que indica se a interface de logon deve ser mostrada ou ocultada. Por exemplo, a página mestra pode incluir uma propriedade pública chamada `ShowLoginUI` cujo valor foi usado para definir a propriedade `Visible` do controle de logon na página mestra. As páginas de conteúdo em que a interface do usuário de logon deve ser suprimida podem, então, definir a propriedade `ShowLoginUI` como `False`.

Talvez o exemplo mais comum de conteúdo e a interação da página mestra ocorra quando os dados exibidos na página mestra precisarem ser atualizados após a ocorrência de alguma ação na página conteúdo. Considere uma página mestra que inclui um GridView que exibe os cinco registros adicionados mais recentemente de uma tabela de banco de dados específica e que uma de suas páginas de conteúdo inclui uma interface para adicionar novos registros à mesma tabela.

Quando um usuário visita a página para adicionar um novo registro, ele vê os cinco registros adicionados mais recentemente exibidos na página mestra. Depois de preencher os valores das colunas do novo registro, ela envia o formulário. Supondo que o GridView na página mestra tenha sua propriedade `EnableViewState` definida como true (o padrão), seu conteúdo é recarregado a partir do estado de exibição e, consequentemente, os cinco mesmos registros são exibidos, embora um registro mais recente tenha acabado de ser adicionado ao banco de dados. Isso pode confundir o usuário.

> [!NOTE]
> Mesmo que você desabilite o estado de exibição do GridView para que ele se reassocie à sua fonte de dados subjacente em cada postback, ele ainda não mostrará o registro recém-adicionado porque os dados estão associados ao GridView anteriormente no ciclo de vida da página do que quando o novo registro for adicionado ao datab ASE.

Para corrigir isso, para que o registro recém-adicionado seja exibido no GridView da página mestra no postback, precisamos instruir o GridView a reassociar a sua fonte de dados *depois* que o novo registro tiver sido adicionado ao banco de dado. Isso requer interação entre o conteúdo e as páginas mestras porque a interface para adicionar o novo registro (e seus manipuladores de eventos) estão na página de conteúdo, mas o GridView que precisa ser atualizado está na página mestra.

Como a atualização da exibição da página mestra de um manipulador de eventos na página conteúdo é uma das necessidades mais comuns de interação entre conteúdo e página mestra, vamos explorar este tópico mais detalhadamente. O download para este tutorial inclui um banco de dados Microsoft SQL Server 2005 Express Edition chamado `NORTHWIND.MDF` na pasta `App_Data` do site. O banco de dados Northwind armazena informações sobre produtos, funcionários e vendas de uma empresa fictícia, Northwind Traders.

A etapa 1 percorre a exibição dos cinco produtos adicionados mais recentemente em um GridView na página mestra. A etapa 2 cria uma página de conteúdo para adicionar novos produtos. A etapa 3 analisa como criar propriedades públicas e métodos na página mestra, e a etapa 4 ilustra como interagir programaticamente com essas propriedades e métodos na página de conteúdo.

> [!NOTE]
> Este tutorial não se aprofunda nas especificidades de trabalhar com dados no ASP.NET. As etapas para configurar a página mestra para exibir dados e a página de conteúdo para inserção de dados estão concluídas, ainda Breezy. Para obter uma visão mais detalhada de como exibir e inserir dados e usar os controles SqlDataSource e GridView, consulte os recursos na seção leituras adicionais no final deste tutorial.

## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Etapa 1: exibindo os cinco produtos adicionados mais recentemente na página mestra

Abra a página mestra site. Master e adicione um rótulo e um controle GridView ao `<div>`de `leftContent`. Desmarque a propriedade `Text` do rótulo, defina sua propriedade `EnableViewState` como `False`e sua propriedade `ID` como `GridMessage`; Defina a propriedade `ID` do GridView como `RecentProducts`. Em seguida, no designer, expanda a marca inteligente do GridView e escolha associá-la a uma nova fonte de dados. Isso inicia o assistente de configuração de fonte de dados. Como o banco de dados Northwind na pasta `App_Data` é um banco de dados Microsoft SQL Server, escolha criar um SqlDataSource selecionando (veja a Figura 1); Nomeie o `RecentProductsDataSource`de SqlDataSource.

[![associar o GridView a um controle SqlDataSource chamado RecentProductsDataSource](interacting-with-the-master-page-from-the-content-page-vb/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image1.png)

**Figura 01**: associar o GridView a um controle SqlDataSource chamado `RecentProductsDataSource` ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image3.png))

A próxima etapa nos pede para especificar a qual banco de dados se conectar. Escolha o arquivo de banco de dados `NORTHWIND.MDF` na lista suspensa e clique em Avançar. Como esta é a primeira vez que usamos esse banco de dados, o assistente oferecerá para armazenar a cadeia de conexão em `Web.config`. Armazene a cadeia de conexão usando o nome `NorthwindConnectionString`.

[![conectar-se ao banco de dados Northwind](interacting-with-the-master-page-from-the-content-page-vb/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image4.png)

**Figura 02**: conectar-se ao banco de dados Northwind ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image6.png))

O assistente para configurar fonte de dados fornece dois meios pelos quais podemos especificar a consulta usada para recuperar dados:

- Especificando uma instrução SQL personalizada ou um procedimento armazenado, ou
- Escolhendo uma tabela ou exibição e, em seguida, especificando as colunas a serem retornadas

Como queremos retornar apenas os cinco produtos adicionados mais recentemente, precisamos especificar uma instrução SQL personalizada. Use a seguinte consulta de `SELECT`:

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample1.sql)]

A palavra-chave `TOP 5` retorna apenas os cinco primeiros registros da consulta. A chave primária da tabela `Products`, `ProductID`, é uma coluna `IDENTITY`, que garante que cada novo produto adicionado à tabela terá um valor maior do que a entrada anterior. Portanto, classificar os resultados por `ProductID` em ordem decrescente retorna os produtos que começam com os mais criados recentemente.

[![retornar os cinco produtos adicionados mais recentemente](interacting-with-the-master-page-from-the-content-page-vb/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image7.png)

**Figura 03**: retornar os cinco produtos adicionados mais recentemente ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image9.png))

Depois de concluir o assistente, o Visual Studio gera dois BoundFields para que o GridView exiba os campos `ProductName` e `UnitPrice` retornados do banco de dados. Neste ponto, a marcação declarativa da página mestra deve incluir marcação semelhante à seguinte:

[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample2.aspx)]

Como você pode ver, a marcação contém: o controle da Web do rótulo (`GridMessage`); o `RecentProducts`GridView, com dois BoundFields; e um controle SqlDataSource que retorna os cinco produtos adicionados mais recentemente.

Com esse GridView criado e seu controle SqlDataSource configurado, visite o site por meio de um navegador. Como mostra a Figura 4, você verá uma grade no canto inferior esquerdo que lista os cinco produtos adicionados mais recentemente.

[![o GridView exibe os cinco produtos adicionados mais recentemente](interacting-with-the-master-page-from-the-content-page-vb/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image10.png)

**Figura 04**: o GridView exibe os cinco produtos adicionados mais recentemente ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image12.png))

> [!NOTE]
> Sinta-se à vontade para limpar a aparência do GridView. Algumas sugestões incluem a formatação do valor de `UnitPrice` exibido como uma moeda e o uso de fontes e cores de plano de fundo para melhorar a aparência da grade.

## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Etapa 2: criando uma página de conteúdo para adicionar novos produtos

Nossa próxima tarefa é criar uma página de conteúdo a partir da qual um usuário pode adicionar um novo produto à tabela de `Products`. Adicione uma nova página de conteúdo à pasta `Admin` chamada `AddProduct.aspx`, conectando-a à página mestra `Site.master`. A Figura 5 mostra o Gerenciador de Soluções depois que essa página tiver sido adicionada ao site.

[![adicionar uma nova página ASP.NET à pasta admin](interacting-with-the-master-page-from-the-content-page-vb/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image13.png)

**Figura 05**: adicionar uma nova página do ASP.net à pasta `Admin` ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image15.png))

Lembre-se de que, no tutorial [*especificando o título, as marcas meta e outros cabeçalhos HTML na página mestra*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) , criamos uma classe de página de base personalizada chamada `BasePage` que gerou o título da página se ela não estava definida explicitamente. Vá para a classe code-behind da página `AddProduct.aspx` e faça com que ela derive de `BasePage` (em vez de `System.Web.UI.Page`).

Por fim, atualize o arquivo de `Web.sitemap` para incluir uma entrada para esta lição. Adicione a seguinte marcação abaixo do `<siteMapNode>` para a lição problemas de nomenclatura da ID de controle:

[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample3.xml)]

Como mostra a Figura 6, a adição desse elemento de `<siteMapNode>` é refletida na lista de lições.

Retornar para `AddProduct.aspx`. No controle de conteúdo para o `MainContent` ContentPlaceHolder, adicione um controle DetailsView e nomeie-o `NewProduct`. Associe o DetailsView a um novo controle SqlDataSource chamado `NewProductDataSource`. Assim como com o SqlDataSource na etapa 1, configure o assistente para que ele use o banco de dados Northwind e escolha especificar uma instrução SQL personalizada. Como o DetailsView será usado para adicionar itens ao banco de dados, precisamos especificar uma instrução `SELECT` e uma instrução `INSERT`. Use a seguinte consulta de `SELECT`:

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample4.sql)]

Em seguida, na guia Inserir, adicione a seguinte instrução de `INSERT`:

[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample5.sql)]

Depois de concluir o assistente, vá para a marca inteligente de DetailsView e marque a caixa de seleção "Habilitar inserção". Isso adiciona um CommandField ao DetailsView com sua propriedade `ShowInsertButton` definida como true. Como este DetailsView será usado unicamente para inserir dados, defina a propriedade `DefaultMode` do DetailsView como `Insert`.

E isso é tudo! Vamos testar esta página. Visite `AddProduct.aspx` por meio de um navegador, insira um nome e um preço (consulte a Figura 6).

[![adicionar um novo produto ao banco de dados](interacting-with-the-master-page-from-the-content-page-vb/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image16.png)

**Figura 06**: adicionar um novo produto ao banco de dados ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image18.png))

Depois de digitar o nome e o preço do novo produto, clique no botão Inserir. Isso faz com que o formulário seja renovado. No postback, a instrução de `INSERT` do controle SqlDataSource é executada; seus dois parâmetros são preenchidos com os valores inseridos pelo usuário nos dois controles TextBox de DetailsView. Infelizmente, não há nenhum comentário visual de que uma inserção ocorreu. Seria interessante ter uma mensagem exibida, confirmando que um novo registro foi adicionado. Eu deixe isso como um exercício para o leitor. Além disso, depois de adicionar um novo registro de DetailsView, o GridView na página mestra ainda mostrará os mesmos cinco registros de antes; Ele não inclui o registro recém-adicionado. Vamos examinar como corrigir isso nas próximas etapas.

> [!NOTE]
> Além de adicionar alguma forma de comentários visuais que a inserção teve êxito, recomendo que você também atualizasse a interface de inserção do DetailsView para incluir a validação. No momento, não há nenhuma validação. Se um usuário inserir um valor inválido para o campo `UnitPrice`, como "muito caro", uma exceção será lançada no postback quando o sistema tentar converter essa cadeia de caracteres em um decimal. Para obter mais informações sobre como personalizar a interface de inserção, consulte o [tutorial *Personalizando a interface de modificação de dados* ](https://asp.net/learn/data-access/tutorial-20-vb.aspx) em minha série de tutoriais de [trabalho com dados](../../data-access/index.md).

## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Etapa 3: criando propriedades públicas e métodos na página mestra

Na etapa 1, adicionamos um controle rótulo da Web chamado `GridMessage` acima do GridView na página mestra. Esse rótulo destina-se a exibir uma mensagem opcionalmente. Por exemplo, depois de adicionar um novo registro à tabela `Products`, talvez queiramos mostrar uma mensagem que diz: "*ProductName* foi adicionado ao banco de dados". Em vez de embutir em código o texto para esse rótulo na página mestra, podemos querer que a mensagem seja personalizável pela página de conteúdo.

Como o controle rótulo é implementado como uma variável de membro protegido dentro da página mestra, ele não pode ser acessado diretamente das páginas de conteúdo. Para trabalhar com o rótulo em uma página mestra da página de conteúdo (ou, para esse assunto, qualquer controle da Web na página mestra), precisamos criar uma propriedade pública na página mestra que expõe o controle da Web ou serve como um proxy pelo qual uma de suas propriedades pode ser  acessíveis. Adicione a seguinte sintaxe à classe code-behind da página mestra para expor a propriedade de `Text` do rótulo:

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample6.vb)]

Quando um novo registro é adicionado à tabela de `Products` de uma página de conteúdo, o `RecentProducts` GridView na página mestra precisa ser reassociado à sua fonte de dados subjacente. Para reassociar a chamada GridView, seu método `DataBind`. Como o GridView na página mestra não é acessível programaticamente para as páginas de conteúdo, precisamos criar um método público na página mestra que, quando chamado, reassocia os dados ao GridView. Adicione o seguinte método à classe code-behind da página mestra:

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample7.vb)]

Com a propriedade `GridMessageText` e o método `RefreshRecentProductsGrid` em vigor, qualquer página de conteúdo pode definir ou ler de forma programática o valor da propriedade `Text` do rótulo de `GridMessage` ou reassociar os dados ao `RecentProducts` GridView. A etapa 4 examina como acessar as propriedades públicas e os métodos da página mestra em uma página de conteúdo.

> [!NOTE]
> Não se esqueça de marcar as propriedades e os métodos da página mestra como `Public`. Se você não denotar explicitamente essas propriedades e métodos como `Public`, eles não estarão acessíveis na página de conteúdo.

## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Etapa 4: chamar os membros públicos da página mestra a partir de uma página de conteúdo

Agora que a página mestra tem as propriedades públicas e os métodos necessários, estamos prontos para invocar essas propriedades e métodos na página `AddProduct.aspx` conteúdo. Especificamente, precisamos definir a propriedade `GridMessageText` da página mestra e chamar seu método `RefreshRecentProductsGrid` depois que o novo produto tiver sido adicionado ao banco de dados. Todos os controles da Web de dados ASP.NET acionam eventos imediatamente antes e depois de concluir várias tarefas, o que facilita para os desenvolvedores de página executarem alguma ação programática antes ou depois da tarefa. Por exemplo, quando o usuário final clica no botão de inserção de DetailsView, no postback, o DetailsView gera seu evento de `ItemInserting` antes de iniciar o fluxo de trabalho de inserção. Em seguida, ele insere o registro no banco de dados. Depois disso, o DetailsView gera seu evento de `ItemInserted`. Portanto, para trabalhar com a página mestra após a adição do novo produto, crie um manipulador de eventos para o evento de `ItemInserted` do DetailsView.

Há duas maneiras pelas quais uma página de conteúdo pode interagir de forma programática com sua página mestra:

- Usando a propriedade `Page.Master`, que retorna uma referência de tipo inflexível à página mestra ou
- Especificar o tipo de página mestra da página ou o caminho do arquivo por meio de uma diretiva `@MasterType`; Isso adiciona automaticamente uma propriedade fortemente tipada à página chamada `Master`.

Vamos examinar ambas as abordagens.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Usando a propriedade`Page.Master`de tipo flexível

Todas as páginas da Web ASP.NET devem derivar da classe `Page`, localizada no namespace `System.Web.UI`. A classe `Page` inclui uma [propriedade`Master`](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) que retorna uma referência à página mestra da página. Se a página não tiver uma página mestra `Master` retornará `Nothing`.

A propriedade `Master` retorna um objeto do tipo [`MasterPage`](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (também localizado no namespace `System.Web.UI`), que é o tipo base do qual todas as páginas mestras derivam. Portanto, para usar propriedades públicas ou métodos definidos na página mestra de nosso site, devemos converter o `MasterPage` objeto retornado da propriedade `Master` para o tipo apropriado. Como nomeamos nosso arquivo de página mestra `Site.master`, a classe code-behind foi nomeada `Site`. Portanto, o código a seguir converte a propriedade `Page.Master` em uma instância da classe `Site`.

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample8.vb)]

Agora que convertidamos a propriedade de `Page.Master` com rigidez de tipos para o tipo de site, podemos fazer referência às propriedades e aos métodos específicos do site. Como mostra a Figura 7, a propriedade pública `GridMessageText` aparece na lista suspensa IntelliSense.

[![IntelliSense mostra as propriedades públicas e os métodos de nossa página mestra](interacting-with-the-master-page-from-the-content-page-vb/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image19.png)

**Figura 07**: o IntelliSense mostra as propriedades públicas e os métodos de nossa página mestra ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image21.png))

> [!NOTE]
> Se você tiver nomeado seu arquivo de página mestra `MasterPage.master` o nome da classe code-behind da página mestra será `MasterPage`. Isso pode levar a código ambíguo ao converter do tipo `System.Web.UI.MasterPage` para sua classe `MasterPage`. Em suma, você precisa qualificar totalmente o tipo para o qual está convertendo, o que pode ser um pouco complicado ao usar o modelo de projeto de site. Minha sugestão seria certificar-se de que, ao criar sua página mestra, você o nomeie como algo diferente `MasterPage.master` ou, ainda melhor, crie uma referência fortemente tipada para a página mestra.

### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Criando uma referência fortemente tipada com a diretiva`@MasterType`

Se você olhar de perto, pode ver que a classe code-behind de uma página ASP.NET é uma classe parcial (Observe a palavra-chave `Partial` na definição de classe). As classes parciais foram C# introduzidas em e Visual Basic estrutura with.NET 2,0 e, em poucas, permitem que os membros de uma classe sejam definidos em vários arquivos. A classe code-behind file-`AddProduct.aspx.vb`, por exemplo, contém o código que nós, o desenvolvedor da página, cria. Além do nosso código, o mecanismo ASP.NET cria automaticamente um arquivo de classe separado com propriedades e manipuladores de eventos no que convertem a marcação declarativa na hierarquia de classes da página.

A geração de código automática que ocorre sempre que uma página ASP.NET é visitada abre o caminho para algumas possibilidades muito interessantes e úteis. No caso de páginas mestras, se informar ao mecanismo ASP.NET qual página mestra está sendo usada pela página de conteúdo, ele gera uma propriedade de `Master` fortemente tipada para nós.

Use a [diretiva`@MasterType`](https://msdn.microsoft.com/library/ms228274.aspx) para informar o mecanismo ASP.net do tipo de página mestra da página de conteúdo. A diretiva `@MasterType` pode aceitar o nome do tipo da página mestra ou seu caminho de arquivo. Para especificar que a página de `AddProduct.aspx` usa `Site.master` como sua página mestra, adicione a seguinte diretiva à parte superior de `AddProduct.aspx`:

[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample9.aspx)]

Essa diretiva instrui o mecanismo ASP.NET a adicionar uma referência fortemente tipada à página mestra por meio de uma propriedade chamada `Master`. Com a diretiva `@MasterType` em vigor, podemos chamar os métodos e as propriedades públicas da página mestra `Site.master` diretamente por meio da propriedade `Master` sem nenhuma conversão.

> [!NOTE]
> Se você omitir a diretiva `@MasterType`, a sintaxe `Page.Master` e `Master` retornará a mesma coisa: um objeto com rigidez de tipos na página mestra da página. Se você incluir a diretiva `@MasterType`, `Master` retornará uma referência fortemente tipada para a página mestra especificada. `Page.Master`, no entanto, ainda retorna uma referência com rigidez de tipos. Para obter uma visão mais detalhada de por que esse é o caso e como a propriedade `Master` é construída quando a diretiva de `@MasterType` é incluída, consulte a entrada de blog de [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx) [`@MasterType` no ASP.NET 2,0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).

### <a name="updating-the-master-page-after-adding-a-new-product"></a>Atualizando a página mestra após a adição de um novo produto

Agora que sabemos como invocar as propriedades públicas e os métodos de uma página mestra em uma página de conteúdo, estamos prontos para atualizar a página de `AddProduct.aspx` para que a página mestra seja atualizada após a adição de um novo produto. No início da etapa 4, criamos um manipulador de eventos para o evento de `ItemInserting` do controle DetailsView, que é executado imediatamente após o novo produto ter sido adicionado ao banco de dados. Adicione o seguinte código ao manipulador de eventos:

[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample10.vb)]

O código acima usa a propriedade de `Page.Master` com rigidez de tipos e a propriedade `Master` fortemente tipada. Observe que a propriedade `GridMessageText` está definida como "*ProductName* adicionada à grade..." Os valores do produto recém-adicionado podem ser acessados por meio da coleção de `e.Values`; como você pode ver, o valor de `ProductName` recém-adicionado é acessado via `e.Values("ProductName")`.

A Figura 8 mostra a página `AddProduct.aspx` imediatamente após um novo produto – a soda de Scott-foi adicionada ao banco de dados. Observe que o nome do produto recém-adicionado é indicado no rótulo da página mestra e que o GridView foi atualizado para incluir o produto e seu preço.

[![o rótulo e o GridView da página mestra mostram o produto recém-adicionado](interacting-with-the-master-page-from-the-content-page-vb/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image22.png)

**Figura 08**: o rótulo e o GridView da página mestra mostram o produto recém-adicionado ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image24.png))

## <a name="summary"></a>Resumo

O ideal é que uma página mestra e suas páginas de conteúdo sejam completamente separadas umas das outras e não exijam nenhum nível de interação. Embora as páginas mestras e as páginas de conteúdo devam ser projetadas com essa meta em mente, há vários cenários comuns em que uma página de conteúdo deve ser interfaceda com sua página mestra. Um dos motivos mais comuns é a atualização de uma parte específica da página mestra exibida com base em alguma ação que ocorreu na página de conteúdo.

A boa notícia é que é relativamente simples ter uma página de conteúdo programaticamente interagir com sua página mestra. Comece criando propriedades públicas ou métodos na página mestra que encapsulam a funcionalidade que precisa ser invocada por uma página de conteúdo. Em seguida, na página conteúdo, acesse as propriedades e os métodos da página mestra por meio da propriedade `Page.Master` de tipo flexível ou use a diretiva `@MasterType` para criar uma referência fortemente tipada para a página mestra.

No próximo tutorial, examinaremos como fazer com que a página mestra interaja programaticamente com uma de suas páginas de conteúdo.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Acessando e atualizando dados no ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET páginas mestras: dicas, truques e armadilhas](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` no ASP.NET 2,0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Passando informações entre conteúdo e páginas mestras](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Trabalhando com dados em Tutoriais do ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP. net e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a [*Sams ensina a ASP.NET 3,5 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott pode ser contatado em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Zack Jones. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](control-id-naming-in-content-pages-vb.md)
> [Próximo](interacting-with-the-content-page-from-the-master-page-vb.md)
