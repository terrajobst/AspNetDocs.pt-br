---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
title: Criando uma interface para selecionar uma conta de usuário de muitos (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, criaremos uma interface do usuário com uma grade paginável e filtrada. Em particular, nossa interface do usuário consistirá em uma série de LinkButtons para...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: da53380c-a16b-41c7-a20d-24343c735c52
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-vb
msc.type: authoredcontent
ms.openlocfilehash: 6c711cdaab113d589d9c2535cb1b422de3f38103
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74614816"
---
# <a name="building-an-interface-to-select-one-user-account-from-many-vb"></a>Criação de uma interface para selecionar uma conta de usuário dentre muitas (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.12.zip) ou [baixar PDF](https://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_vb.pdf)

> Neste tutorial, criaremos uma interface do usuário com uma grade paginável e filtrada. Em particular, nossa interface do usuário consistirá em uma série de LinkButtons para filtrar os resultados com base na letra inicial do nome de usuário e um controle GridView para mostrar os usuários correspondentes. Vamos começar listando todas as contas de usuário em um GridView. Em seguida, na etapa 3, adicionaremos o filtro LinkButtons. A etapa 4 examina a paginação dos resultados filtrados. A interface construída nas etapas 2 a 4 será usada nos tutoriais subsequentes para executar tarefas administrativas para uma conta de usuário específica.

## <a name="introduction"></a>Introdução

<a id="_msoanchor_1"> </a>No tutorial [*atribuindo funções a usuários*](../roles/assigning-roles-to-users-vb.md) , criamos uma interface rudimentar para que um administrador selecione um usuário e gerencie suas funções. Especificamente, a interface apresentou ao administrador uma lista suspensa de todos os usuários. Essa interface é adequada quando há, mas uma dúzia de contas de usuário, mas é difícil para sites com centenas ou milhares de contas. Uma grade paginável e filtrável é uma interface de usuário mais adequada para sites com grandes bases de usuários.

Neste tutorial, criaremos uma interface de usuário desse tipo. Em particular, nossa interface do usuário consistirá em uma série de LinkButtons para filtrar os resultados com base na letra inicial do nome de usuário e um controle GridView para mostrar os usuários correspondentes. Vamos começar listando todas as contas de usuário em um GridView. Em seguida, na etapa 3, adicionaremos o filtro LinkButtons. A etapa 4 examina a paginação dos resultados filtrados. A interface construída nas etapas 2 a 4 será usada nos tutoriais subsequentes para executar tarefas administrativas para uma conta de usuário específica.

Vamos começar!

## <a name="step-1-adding-new-aspnet-pages"></a>Etapa 1: adicionando novas páginas do ASP.NET

Neste tutorial e nos próximos dois, examinaremos várias funções e recursos relacionados à administração. Precisaremos de uma série de páginas do ASP.NET para implementar os tópicos examinados em todos esses tutoriais. Vamos criar essas páginas e atualizar o mapa do site.

Comece criando uma nova pasta no projeto chamada `Administration`. Em seguida, adicione duas novas páginas ASP.NET à pasta, vinculando cada página com a página mestra `Site.master`. Nomeie as páginas:

- `ManageUsers.aspx`
- `UserInformation.aspx`

Além disso, adicione duas páginas ao diretório raiz do site: `ChangePassword.aspx` e `RecoverPassword.aspx`.

Essas quatro páginas devem, neste ponto, ter dois controles de conteúdo, um para cada um dos ContentPlaceHolders da página mestra: `MainContent` e `LoginContent`.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample1.aspx)]

Queremos mostrar a marcação padrão da página mestra para o `LoginContent` ContentPlaceHolder para essas páginas. Portanto, remova a marcação declarativa para o controle de conteúdo `Content2`. Depois de fazer isso, a marcação Pages deve conter apenas um controle de conteúdo.

As páginas ASP.NET na pasta `Administration` são destinadas somente a usuários administrativos. Adicionamos uma função de administradores ao sistema no <a id="_msoanchor_2"> </a>tutorial [*criando e gerenciando funções*](../roles/creating-and-managing-roles-vb.md) ; Restrinja o acesso a essas duas páginas a essa função. Para fazer isso, adicione um arquivo de `Web.config` à pasta `Administration` e configure seu elemento `<authorization>` para admitir usuários na função Administradores e para negar todos os outros.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample2.xml)]

Neste ponto, o Gerenciador de Soluções do seu projeto deve ser semelhante à captura de tela mostrada na Figura 1.

[![quatro novas páginas e um arquivo Web. config foram adicionados ao site](building-an-interface-to-select-one-user-account-from-many-vb/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image1.png)

**Figura 1**: quatro novas páginas e um arquivo `Web.config` foram adicionados ao site ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image3.png))

Por fim, atualize o mapa do site (`Web.sitemap`) para incluir uma entrada na página `ManageUsers.aspx`. Adicione o seguinte XML após o `<siteMapNode>` que adicionamos para os tutoriais de funções.

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample3.xml)]

Com o mapa do site atualizado, visite o site por meio de um navegador. Como mostra a Figura 2, a navegação à esquerda agora inclui itens para os tutoriais de administração.

[![o mapa do site inclui um nó chamado administração do usuário](building-an-interface-to-select-one-user-account-from-many-vb/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image4.png)

**Figura 2**: o mapa do site inclui um nó chamado administração do usuário ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image6.png))

## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>Etapa 2: listando todas as contas de usuário em um GridView

Nosso objetivo final deste tutorial é criar uma grade paginável e filtrável por meio da qual um administrador pode selecionar uma conta de usuário para gerenciar. Vamos começar listando *todos* os usuários em um GridView. Depois que isso for concluído, adicionaremos as interfaces e a funcionalidade de filtragem e de paginação.

Abra a página `ManageUsers.aspx` na pasta `Administration` e adicione um GridView, definindo seu `ID` como `UserAccounts` daqui a pouco, escreveremos o código para associar o conjunto de contas de usuário ao GridView usando o método `Membership` da classe `GetAllUsers`. Conforme discutido nos tutoriais anteriores, o método `GetAllUsers` retorna um objeto `MembershipUserCollection`, que é uma coleção de objetos `MembershipUser`. Cada `MembershipUser` na coleção inclui propriedades como `UserName`, `Email`, `IsApproved`e assim por diante.

Para exibir as informações de conta de usuário desejadas no GridView, defina a propriedade `AutoGenerateColumns` do GridView como false e adicione BoundFields para as propriedades `UserName`, `Email`e `Comment` e CheckBoxFields para as propriedades `IsApproved`, `IsLockedOut`e `IsOnline`. Essa configuração pode ser aplicada por meio da marcação declarativa do controle ou por meio da caixa de diálogo campos. A Figura 3 mostra uma captura de tela da caixa de diálogo campos após a seleção de gerar campos automaticamente ter sido desmarcada e os BoundFields e CheckBoxFields foram adicionados e configurados.

[![adicionar três BoundFields e três CheckBoxFields ao GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image7.png)

**Figura 3**: adicionar três boundfields e três checkboxfields ao GridView ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image9.png))

Depois de configurar seu GridView, verifique se sua marcação declarativa é semelhante ao seguinte:

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample4.aspx)]

Em seguida, precisamos escrever um código que associa as contas de usuário ao GridView. Crie um método chamado `BindUserAccounts` para executar essa tarefa e, em seguida, chame-a do manipulador de eventos `Page_Load` na primeira página.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample5.vb)]

Reserve um tempo para testar a página por meio de um navegador. Como mostra a Figura 4, o `UserAccounts` GridView lista o nome de usuário, o endereço de email e outras informações de conta pertinentes para todos os usuários no sistema.

[![as contas de usuário estão listadas no GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image10.png)

**Figura 4**: as contas de usuário são listadas em GridView ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image12.png))

## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>Etapa 3: filtrando os resultados pela primeira letra do nome de usuário

Atualmente, o `UserAccounts` GridView mostra *todas* as contas de usuário. Para sites com centenas ou milhares de contas de usuário, é imperativo que o usuário possa reduzir rapidamente as contas exibidas. Isso pode ser feito adicionando a filtragem de LinkButtons à página. Vamos adicionar 27 LinkButtons à página: um título tudo junto com um LinkButton para cada letra do alfabeto. Se um visitante clicar em todos os LinkButton, o GridView mostrará todos os usuários. Se eles clicarem em uma determinada letra, somente os usuários cujo nome de usuário começa com a letra selecionada serão exibidos.

Nossa primeira tarefa é adicionar os controles de 27 LinkButton. Uma opção seria criar os 27 LinkButtons declarativamente, um de cada vez. Uma abordagem mais flexível é usar um controle Repeater com um `ItemTemplate` que processa um LinkButton e, em seguida, associa as opções de filtragem ao repetidor como uma matriz de `String`.

Comece adicionando um controle Repeater à página acima do `UserAccounts` GridView. Defina a propriedade `ID` do repetidor como `FilteringUI` configurar os modelos do repetidor para que sua `ItemTemplate` processe um LinkButton cujas propriedades `Text` e `CommandName` estejam associadas ao elemento da matriz atual. Como vimos no <a id="_msoanchor_3"> </a>tutorial [*atribuindo funções a usuários*](../roles/assigning-roles-to-users-vb.md) , isso pode ser feito usando a sintaxe `Container.DataItem` DataBinding. Use o `SeparatorTemplate` do repetidor para exibir uma linha vertical entre cada link.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample6.aspx)]

Para preencher esse repetidor com as opções de filtragem desejadas, crie um método chamado `BindFilteringUI`. Certifique-se de chamar esse método a partir do manipulador de eventos `Page_Load` no primeiro carregamento de página.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample7.vb)]

Esse método especifica as opções de filtragem como elementos na matriz de `String` `filterOptions` para cada elemento na matriz, o repetidor renderizará um LinkButton com sua `Text` e `CommandName` propriedades atribuídas ao valor do elemento da matriz.

A Figura 5 mostra a página de `ManageUsers.aspx` quando exibida por meio de um navegador.

[![o repetidor lista 27 LinkButtons de filtragem](building-an-interface-to-select-one-user-account-from-many-vb/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image13.png)

**Figura 5**: o repetidor lista 27 os LinkButtons de filtragem ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image15.png))

> [!NOTE]
> Os nomes de acessadores podem começar com qualquer caractere, incluindo números e pontuação. Para exibir essas contas, o administrador precisará usar a opção todos os LinkButton. Como alternativa, você pode adicionar um LinkButton para retornar todas as contas de usuário que começam com um número. Eu deixe isso como um exercício para o leitor.

Clicar em qualquer um dos LinkButtons de filtragem causa um postback e gera o evento de `ItemCommand` do repetidor, mas não há nenhuma alteração na grade porque ainda não escrevemos nenhum código para filtrar os resultados. A classe `Membership` inclui um [método`FindUsersByName`](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) que retorna as contas de usuário cujo nome de usuário corresponde a um padrão de pesquisa especificado. Podemos usar esse método para recuperar apenas as contas de usuários cujos nomes de usuário começam com a letra especificada pelo `CommandName` do LinkButton filtrado que foi clicado.

Comece atualizando a classe code-behind da página `ManageUser.aspx` de forma que ela inclua uma propriedade chamada `UsernameToMatch` essa propriedade persiste a cadeia de caracteres de filtro de nome de usuário entre postbacks:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample8.vb)]

A propriedade `UsernameToMatch` armazena seu valor que é atribuído à coleção de `ViewState` usando a chave UsernameToMatch. Quando o valor dessa propriedade é lido, ele verifica se existe um valor na coleção de `ViewState`; caso contrário, ele retorna o valor padrão, uma cadeia de caracteres vazia. A propriedade `UsernameToMatch` exibe um padrão comum, o que mantém um valor para o estado de exibição, de modo que qualquer alteração na propriedade seja persistida entre postbacks. Para obter mais informações sobre esse padrão, leia [Understanding ASP.net view state](https://msdn.microsoftn-us/library/ms972976.aspx).

Em seguida, atualize o método `BindUserAccounts` de modo que, em vez de chamar `Membership.GetAllUsers`, ele chama `Membership.FindUsersByName`, passando o valor da propriedade `UsernameToMatch` acrescentada com o caractere curinga SQL,%.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample9.vb)]

Para exibir apenas os usuários cujo nome de usuário começa com a letra A, defina a propriedade `UsernameToMatch` como a e, em seguida, chame `BindUserAccounts` isso resultaria em uma chamada para `Membership.FindUsersByName("A%")`, que retornará todos os usuários cujo nome de usuário comece com um. da mesma forma, para retornar *todos* os usuários, atribua uma cadeia de caracteres vazia à propriedade `UsernameToMatch` para que o método `BindUserAccounts` invoque `Membership.FindUsersByName("%")`, retornando,

Crie um manipulador de eventos para o evento de `ItemCommand` do repetidor. Esse evento é gerado sempre que um dos seus LinkButton de filtro é clicado; é passado o valor de `CommandName` de LinkButton clicado por meio do objeto `RepeaterCommandEventArgs`. Precisamos atribuir o valor apropriado à propriedade `UsernameToMatch` e, em seguida, chamar o método `BindUserAccounts`. Se o `CommandName` for All, atribua uma cadeia de caracteres vazia a `UsernameToMatch` para que todas as contas de usuário sejam exibidas. Caso contrário, atribua o valor de `CommandName` a `UsernameToMatch`

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample10.vb)]

Com esse código em vigor, teste a funcionalidade de filtragem. Quando a página é visitada pela primeira vez, todas as contas de usuário são exibidas (consulte novamente a Figura 5). Clicar em um LinkButton causa um postback e filtra os resultados, exibindo apenas as contas de usuário que começam com um.

[![usar os LinkButtons de filtragem para exibir os usuários cujo nome de usuário começa com uma determinada letra](building-an-interface-to-select-one-user-account-from-many-vb/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image16.png)

**Figura 6**: usar os LinkButtons de filtragem para exibir os usuários cujo nome de usuário começa com uma determinada letra ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image18.png))

## <a name="step-4-updating-the-gridview-to-use-paging"></a>Etapa 4: atualizando o GridView para usar a paginação

O GridView mostrado nas figuras 5 e 6 lista todos os registros retornados do método `FindUsersByName`. Se houver centenas ou milhares de contas de usuário, isso poderá levar à sobrecarga de informações ao exibir todas as contas (como é o caso ao clicar em todos os LinkButton ou ao visitar inicialmente a página). Para ajudar a apresentar as contas de usuário em partes mais gerenciáveis, vamos configurar o GridView para exibir 10 contas de usuário por vez.

O controle GridView oferece dois tipos de paginação:

- **Paginação padrão** – fácil de implementar, mas ineficiente. Resumindo, com a paginação padrão, o GridView espera *todos* os registros de sua fonte de dados. Em seguida, ele exibe apenas a página apropriada de registros.
- **Paginação personalizada** – requer mais trabalho para implementar, mas é mais eficiente que a paginação padrão porque, com a paginação personalizada, a fonte de dados retorna apenas o conjunto preciso de registros a serem exibidos.

A diferença de desempenho entre a paginação padrão e personalizada pode ser bastante substancial ao paginar por meio de milhares de registros. Como estamos criando essa interface supondo que pode haver centenas ou milhares de contas de usuário, vamos usar a paginação personalizada.

> [!NOTE]
> Para obter uma discussão mais completa sobre as diferenças entre paginação padrão e personalizada, bem como os desafios envolvidos na implementação de paginação personalizada, consulte [paginar com eficiência por meio de grandes quantidades de dados](https://asp.net/learn/data-access/tutorial-25-vb.aspx). Para obter uma análise da diferença de desempenho entre paginação padrão e personalizada, consulte [paginação personalizada em ASP.NET com SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx).

Para implementar a paginação personalizada, precisamos primeiro de um mecanismo pelo qual recuperar o subconjunto preciso de registros exibidos pelo GridView. A boa notícia é que o método `FindUsersByName` da classe `Membership` tem uma sobrecarga que nos permite especificar o índice da página e o tamanho da página e retorna apenas as contas de usuário que estão dentro desse intervalo de registros.

Em particular, essa sobrecarga tem a seguinte assinatura: [`FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)`](https://msdn.microsoft.com/library/fa5st8b2.aspx).

O parâmetro *pageIndex* especifica a página de contas de usuário a serem retornadas; *PageSize* indica quantos registros Exibir por página. O parâmetro *totalRecords* é um parâmetro `ByRef` que retorna o número total de contas de usuário no repositório de usuários.

> [!NOTE]
> Os dados retornados pelo `FindUsersByName` são classificados por nome de usuário; os critérios de classificação não podem ser personalizados.

O GridView pode ser configurado para utilizar a paginação personalizada, mas somente quando associado a um controle ObjectDataSource. Para que o controle ObjectDataSource implemente a paginação personalizada, ele requer dois métodos: um que é passado como um índice de linha inicial e o número máximo de registros a serem exibidos e retorna o subconjunto preciso de registros que se enquadram nesse intervalo; e um método que retorna o número total de registros sendo paginados. A sobrecarga de `FindUsersByName` aceita um índice de página e o tamanho da página e retorna o número total de registros por meio de um parâmetro `ByRef`. Portanto, há uma incompatibilidade de interface aqui.

Uma opção seria criar uma classe proxy que expõe a interface esperada pelo ObjectDataSource e, em seguida, chama internamente o método `FindUsersByName`. Outra opção – e a que usaremos neste artigo é criar nossa própria interface de paginação e usá-la em vez da interface de paginação interna do GridView.

### <a name="creating-a-first-previous-next-last-paging-interface"></a>Criando uma primeira interface de paginação, anterior, próxima e última

Vamos criar uma interface de paginação com primeiro, anterior, próximo e último LinkButton. O primeiro LinkButton, quando clicado, levará o usuário para a primeira página de dados, enquanto anterior retornará-o para a página anterior. Da mesma forma, Next e Last passarão o usuário para a próxima e última página, respectivamente. Adicione os quatro controles LinkButton abaixo do `UserAccounts` GridView.

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample11.aspx)]

Em seguida, crie um manipulador de eventos para cada um dos eventos de `Click` de LinkButton.

A Figura 7 mostra os quatro LinkButtons quando exibidos por meio do modo de exibição de Design Visual Web Developer.

[![adicionar primeiro, anterior, próximo e último LinkButtons abaixo do GridView](building-an-interface-to-select-one-user-account-from-many-vb/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image19.png)

**Figura 7**: Adicionar primeiro, anterior, próximo e último LinkButtons abaixo do GridView ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image21.png))

### <a name="keeping-track-of-the-current-page-index"></a>Controlando o índice da página atual

Quando um usuário visita pela primeira vez a `ManageUsers.aspx` página ou clica em um dos botões de filtragem, queremos exibir a primeira página de dados no GridView. No entanto, quando o usuário clica em um dos LinkButton de navegação, precisamos atualizar o índice da página. Para manter o índice de página e o número de registros a serem exibidos por página, adicione as duas propriedades a seguir à classe code-behind da página:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample12.vb)]

Assim como a propriedade `UsernameToMatch`, a propriedade `PageIndex` persiste seu valor para o estado de exibição. A propriedade `PageSize` somente leitura retorna um valor embutido em código, 10. Convi o leitor interessado para atualizar essa propriedade para usar o mesmo padrão que `PageIndex`e, em seguida, aumentar a página `ManageUsers.aspx` de forma que a pessoa que visita a página possa especificar quantas contas de usuário exibir por página.

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>Recuperando apenas os registros da página atual, atualizando o índice da página e habilitando e desabilitando os LinkButton da interface de paginação

Com a interface de paginação em vigor e as propriedades `PageIndex` e `PageSize` adicionadas, estamos prontos para atualizar o método `BindUserAccounts` de forma que ele use a sobrecarga de `FindUsersByName` apropriada. Além disso, precisamos ter esse método para habilitar ou desabilitar a interface de paginação, dependendo de qual página está sendo exibida. Ao exibir a primeira página de dados, os links primeiros e anteriores devem ser desabilitados; Avançar e último devem ser desabilitados ao exibir a última página.

Atualize o método `BindUserAccounts` pelo seguinte código:

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample13.vb)]

Observe que o número total de registros sendo paginados por meio do é determinado pelo último parâmetro do método `FindUsersByName`. Depois que a página especificada de contas de usuário é retornada, os quatro LinkButtons são habilitados ou desabilitados, dependendo se a primeira ou última página de dados está sendo exibida.

A última etapa é escrever o código para os quatro manipuladores de eventos de `Click` LinkButtons. Esses manipuladores de eventos precisam atualizar a propriedade `PageIndex` e, em seguida, reassociar os dados ao GridView por meio de uma chamada para `BindUserAccounts` os manipuladores de eventos First, Previous e Next são muito simples. O manipulador de eventos `Click` para o último LinkButton, no entanto, é um pouco mais complexo, pois precisamos determinar quantos registros estão sendo exibidos para determinar o último índice de página.

[!code-vb[Main](building-an-interface-to-select-one-user-account-from-many-vb/samples/sample14.vb)]

As figuras 8 e 9 mostram a interface de paginação personalizada em ação. A Figura 8 mostra a página `ManageUsers.aspx` ao exibir a primeira página de dados para todas as contas de usuário. Observe que apenas as 10 das 13 contas são exibidas. Clicar no próximo ou no último link causa um postback, atualiza o `PageIndex` para 1 e associa a segunda página de contas de usuário à grade (consulte a Figura 9).

[![as primeiras 10 contas de usuário são exibidas](building-an-interface-to-select-one-user-account-from-many-vb/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image22.png)

**Figura 8**: as primeiras 10 contas de usuário são exibidas ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image24.png))

[![clicar no link seguinte exibe a segunda página de contas de usuário](building-an-interface-to-select-one-user-account-from-many-vb/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-vb/_static/image25.png)

**Figura 9**: clicar no link seguinte exibe a segunda página de contas de usuário ([clique para exibir a imagem em tamanho normal](building-an-interface-to-select-one-user-account-from-many-vb/_static/image27.png))

## <a name="summary"></a>Resumo

Os administradores geralmente precisam selecionar um usuário na lista de contas. Nos tutoriais anteriores, examinamos o uso de uma lista suspensa preenchida com os usuários, mas essa abordagem não é bem dimensionada. Neste tutorial, exploramos uma alternativa melhor: uma interface filtrável cujos resultados são exibidos em um GridView paginado. Com essa interface do usuário, os administradores podem localizar e selecionar com rapidez e eficiência uma conta de usuário entre milhares.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Paginação personalizada em ASP.NET com SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [Paginar com eficiência por meio de grandes quantidades de dados](https://asp.net/learn/data-access/tutorial-25-vb.aspx)
- [Distribuindo sua própria ferramenta de administração de sites](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP. NET e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a *[Sams ensina a ASP.NET 2,0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott pode ser contatado em [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Alicja Maziarz. Está interessado em revisar meus artigos futuros do MSDN? Nesse caso, me solte uma linha em

> [!div class="step-by-step"]
> [Anterior](unlocking-and-approving-user-accounts-cs.md)
> [Próximo](recovering-and-changing-passwords-vb.md)
