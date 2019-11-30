---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
title: Atribuindo funções a usuários (C#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, criaremos duas páginas ASP.NET para ajudar no gerenciamento de quais usuários pertencem a quais funções. A primeira página incluirá recursos para ver o quê...
ms.author: riande
ms.date: 03/24/2008
ms.assetid: d522639a-5aca-421e-9a76-d73f95607f57
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
msc.type: authoredcontent
ms.openlocfilehash: 3346e47cf604ed1d4003ca83203116666e37cb1b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74634069"
---
# <a name="assigning-roles-to-users-c"></a>Atribuir funções aos usuários (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.10.zip) ou [baixar PDF](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_cs.pdf)

> Neste tutorial, criaremos duas páginas ASP.NET para ajudar no gerenciamento de quais usuários pertencem a quais funções. A primeira página incluirá recursos para ver quais usuários pertencem a uma determinada função, a quais funções um usuário específico pertence e a capacidade de atribuir ou remover um usuário específico de uma função específica. Na segunda página, aumentaremos o controle CreateUserWizard para que ele inclua uma etapa para especificar a quais funções o usuário recém-criado pertence. Isso é útil em cenários em que um administrador é capaz de criar novas contas de usuário.

## <a name="introduction"></a>Introdução

O <a id="_msoanchor_1"> </a> [tutorial anterior](creating-and-managing-roles-cs.md) examinou a estrutura de funções e a `SqlRoleProvider`; vimos como usar a classe `Roles` para criar, recuperar e excluir funções. Além de criar e excluir funções, é necessário poder atribuir ou remover usuários de uma função. Infelizmente, o ASP.NET não é fornecido com nenhum controle da Web para gerenciar o que os usuários pertencem a quais funções. Em vez disso, devemos criar nossas próprias páginas ASP.NET para gerenciar essas associações. A boa notícia é que a adição e a remoção de usuários para funções é muito fácil. A classe `Roles` contém vários métodos para adicionar um ou mais usuários a uma ou mais funções.

Neste tutorial, criaremos duas páginas ASP.NET para ajudar no gerenciamento de quais usuários pertencem a quais funções. A primeira página incluirá recursos para ver quais usuários pertencem a uma determinada função, a quais funções um usuário específico pertence e a capacidade de atribuir ou remover um usuário específico de uma função específica. Na segunda página, aumentaremos o controle CreateUserWizard para que ele inclua uma etapa para especificar a quais funções o usuário recém-criado pertence. Isso é útil em cenários em que um administrador é capaz de criar novas contas de usuário.

Vamos começar!

## <a name="listing-what-users-belong-to-what-roles"></a>Listando o que os usuários pertencem a quais funções

A primeira ordem de negócios para este tutorial é criar uma página da Web a partir da qual os usuários podem ser atribuídos a funções. Antes de nos preocuparmos com como atribuir usuários a funções, vamos nos concentrar primeiro em como determinar quais usuários pertencem a quais funções. Há duas maneiras de exibir essas informações: "por função" ou "por usuário". Poderíamos permitir que o visitante selecione uma função e, em seguida, mostrar todos os usuários que pertencem à função (a exibição "por função") ou pode solicitar que o visitante selecione um usuário e, em seguida, mostrar a eles as funções atribuídas a esse usuário (a exibição "por usuário").

A exibição "por função" é útil em circunstâncias em que o visitante deseja saber o conjunto de usuários que pertencem a uma função específica; a exibição "por usuário" é ideal quando o visitante precisa saber a (s) função (ões) de um usuário específico. Vamos fazer com que nossa página inclua as interfaces "por função" e "por usuário".

Começaremos com a criação da interface "por usuário". Essa interface consistirá em uma lista suspensa e uma lista de caixas de seleção. A lista suspensa será preenchida com o conjunto de usuários no sistema; as caixas de seleção enumerarão as funções. Selecionar um usuário na lista suspensa verificará as funções às quais o usuário pertence. A pessoa que visita a página pode marcar ou desmarcar as caixas de seleção para adicionar ou remover o usuário selecionado das funções correspondentes.

> [!NOTE]
> Usar uma lista suspensa para listar as contas de usuário não é uma opção ideal para sites em que pode haver centenas de contas de usuário. Uma lista suspensa é projetada para permitir que um usuário escolha um item de uma lista de opções relativamente curta. Rapidamente torna-se difícil o aumento do número de itens de lista. Se você estiver criando um site que terá um número potencialmente grande de contas de usuário, convém considerar o uso de uma interface de usuário alternativa, como um GridView pageable ou uma interface filtrável que lista os prompts para que o visitante escolha uma letra e, em seguida, apenas mostra os usuários cujo nome de usuário começa com a letra selecionada.

## <a name="step-1-building-the-by-user-user-interface"></a>Etapa 1: criando a interface do usuário "por usuário"

Abra a página `UsersAndRoles.aspx`. Na parte superior da página, adicione um controle da Web de rótulo chamado `ActionStatus` e desmarque sua propriedade `Text`. Usaremos esse rótulo para fornecer comentários sobre as ações executadas, exibindo mensagens como "o usuário Tito foi adicionado à função Administradores" ou "o usuário Jisun foi removido da função de supervisores". Para fazer essas mensagens se destacarem, defina a propriedade `CssClass` do rótulo como "importante".

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample1.aspx)]

Em seguida, adicione a seguinte definição de classe CSS à folha de estilos `Styles.css`:

[!code-css[Main](assigning-roles-to-users-cs/samples/sample2.css)]

Essa definição de CSS instrui o navegador a exibir o rótulo usando uma fonte grande e vermelha. A Figura 1 mostra esse efeito por meio do designer do Visual Studio.

[![a propriedade CssClass do rótulo resulta em uma fonte grande e vermelha](assigning-roles-to-users-cs/_static/image2.png)](assigning-roles-to-users-cs/_static/image1.png)

**Figura 1**: a propriedade `CssClass` do rótulo resulta em uma fonte grande e vermelha ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-cs/_static/image3.png))

Em seguida, adicione uma DropDownList à página, defina sua propriedade `ID` como `UserList`e defina sua propriedade `AutoPostBack` como true. Usaremos essa DropDownList para listar todos os usuários no sistema. Esta DropDownList será associada a uma coleção de objetos MembershipUser. Como queremos que a DropDownList exiba a propriedade UserName do objeto MembershipUser (e use-a como o valor dos itens de lista), defina a `DataTextField` da DropDownList e `DataValueField` propriedades como "UserName".

Sob a DropDownList, adicione um repetidor chamado `UsersRoleList`. Esse repetidor listará todas as funções no sistema como uma série de caixas de seleção. Defina o `ItemTemplate` do repetidor usando a seguinte marcação declarativa:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample3.aspx)]

A marcação de `ItemTemplate` inclui um único controle da Web de caixa de seleção chamado `RoleCheckBox`. A propriedade `AutoPostBack` da caixa de seleção está definida como true e a propriedade `Text` está associada a `Container.DataItem`. O motivo pelo qual a sintaxe de DataBinding é simplesmente `Container.DataItem` é porque a estrutura de funções retorna a lista de nomes de função como uma matriz de cadeia de caracteres, e é essa matriz de cadeia de caracteres que iremos associar ao repetidor. Uma descrição completa do motivo pelo qual essa sintaxe é usada para exibir o conteúdo de uma matriz associada a um controle da Web de dados está além do escopo deste tutorial. Para obter mais informações sobre esse assunto, consulte [associando uma matriz escalar a um controle da Web de dados](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

Neste ponto, a marcação declarativa da interface "por usuário" deve ser semelhante ao seguinte:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample4.aspx)]

Agora estamos prontos para escrever o código para associar o conjunto de contas de usuário à DropDownList e ao conjunto de funções para o repetidor. Na classe code-behind da página, adicione um método chamado `BindUsersToUserList` e outro `BindRolesList`nomeado, usando o seguinte código:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample5.cs)]

O método `BindUsersToUserList` recupera todas as contas de usuário no sistema por meio do [método`Membership.GetAllUsers`](https://msdn.microsoft.com/library/dy8swhya.aspx). Isso retorna um [objeto`MembershipUserCollection`](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), que é uma coleção de [instâncias de`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Em seguida, essa coleção é associada à `UserList` DropDownList. As instâncias de `MembershipUser` que subcomposiçãom a coleção contêm uma variedade de propriedades, como `UserName`, `Email`, `CreationDate`e `IsOnline`. Para instruir o DropDownList a exibir o valor da propriedade `UserName`, verifique se as propriedades `DataTextField` e `DataValueField` do `UserList` DropDownList foram definidas como "UserName".

> [!NOTE]
> O método `Membership.GetAllUsers` tem duas sobrecargas: uma que não aceita parâmetros de entrada e retorna todos os usuários e outra que usa valores inteiros para o índice de página e o tamanho da página e retorna apenas o subconjunto especificado dos usuários. Quando há grandes quantidades de contas de usuário sendo exibidas em um elemento de interface de usuário paginável, a segunda sobrecarga pode ser usada para percorrer com mais eficiência os usuários, uma vez que retorna apenas o subconjunto preciso de contas de usuário em vez de todos eles.

O método `BindRolesToList` começa chamando o [método`GetAllRoles`](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx)da classe `Roles`, que retorna uma matriz de cadeia de caracteres que contém as funções no sistema. Em seguida, essa matriz de cadeia de caracteres é associada ao repetidor.

Finalmente, precisamos chamar esses dois métodos quando a página é carregada pela primeira vez. Adicione o seguinte código ao manipulador de eventos do `Page_Load`:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample6.cs)]

Com esse código em vigor, Reserve um momento para visitar a página por meio de um navegador; sua tela deve ser semelhante à figura 2. Todas as contas de usuário são populadas na lista suspensa e, abaixo disso, cada função aparece como uma caixa de seleção. Como definimos as propriedades `AutoPostBack` de DropDownList e CheckBoxes como true, alterar o usuário selecionado ou marcar ou desmarcar uma função causa um postback. No entanto, nenhuma ação é executada porque ainda temos que escrever código para lidar com essas ações. Iremos lidar com essas tarefas nas próximas duas seções.

[![a página exibe os usuários e as funções](assigning-roles-to-users-cs/_static/image5.png)](assigning-roles-to-users-cs/_static/image4.png)

**Figura 2**: a página exibe os usuários e as funções ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-cs/_static/image6.png))

### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Verificando as funções às quais o usuário selecionado pertence

Quando a página é carregada pela primeira vez ou sempre que o visitante seleciona um novo usuário na lista suspensa, precisamos atualizar as caixas de seleção do `UsersRoleList`para que uma determinada função seja marcada somente se o usuário selecionado pertencer a essa função. Para fazer isso, crie um método chamado `CheckRolesForSelectedUser` com o seguinte código:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample7.cs)]

O código acima começa determinando quem é o usuário selecionado. Em seguida, ele usa o [método`GetRolesForUser(userName)`](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) da classe roles para retornar o conjunto de funções do usuário especificado como uma matriz de cadeia de caracteres. Em seguida, os itens do repetidor são enumerados e a caixa de seleção de `RoleCheckBox` de cada item é referenciada por programação. A caixa de seleção é verificada somente se a função à qual ela corresponde está contida na matriz de cadeia de caracteres `selectedUsersRoles`.

> [!NOTE]
> A sintaxe de `selectedUserRoles.Contains<string>(...)` não será compilada se você estiver usando o ASP.NET versão 2,0. O método `Contains<string>` faz parte da [biblioteca do LINQ](http://en.wikipedia.org/wiki/Language_Integrated_Query), que é nova no ASP.net 3,5. Se você ainda estiver usando o ASP.NET versão 2,0, use o [método`Array.IndexOf<string>`](https://msdn.microsoft.com/library/eha9t187.aspx) em vez disso.

O método `CheckRolesForSelectedUser` precisa ser chamado em dois casos: quando a página é carregada pela primeira vez e sempre que o índice selecionado da `UserList` DropDownList é alterado. Portanto, chame esse método do manipulador de eventos `Page_Load` (após as chamadas para `BindUsersToUserList` e `BindRolesToList`). Além disso, crie um manipulador de eventos para o evento de `SelectedIndexChanged` da DropDownList e chame esse método a partir daí.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample8.cs)]

Com esse código em vigor, você pode testar a página por meio do navegador. No entanto, como a página `UsersAndRoles.aspx` atualmente não tem a capacidade de atribuir usuários a funções, nenhum usuário tem funções. Criaremos a interface para atribuir usuários a funções em um momento, de modo que você pode pegar minha palavra que esse código funciona e verificar se ele faz isso mais tarde, ou você pode adicionar usuários manualmente a funções inserindo registros na tabela `aspnet_UsersInRoles` para testar essa funcionalidade agora.

### <a name="assigning-and-removing-users-from-roles"></a>Atribuindo e removendo usuários de funções

Quando o visitante marca ou desmarca uma caixa de seleção no `UsersRoleList` Repeater, precisamos adicionar ou remover o usuário selecionado da função correspondente. A propriedade `AutoPostBack` da caixa de seleção está atualmente definida como true, o que causa um postback sempre que uma caixa de seleção no repetidor é marcada ou desmarcada. Em suma, precisamos criar um manipulador de eventos para o evento de `CheckChanged` da caixa de seleção. Como a caixa de seleção está em um controle Repeater, precisamos adicionar manualmente o direcionamento do manipulador de eventos. Comece adicionando o manipulador de eventos à classe code-behind como um método `protected`, desta forma:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample9.cs)]

Retornaremos para escrever o código para esse manipulador de eventos em um momento. Mas primeiro vamos concluir o direcionamento de manipulação de eventos. Na caixa de seleção dentro do `ItemTemplate`do repetidor, adicione `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Essa sintaxe conecta o manipulador de eventos `RoleCheckBox_CheckChanged` ao evento de `CheckedChanged` do `RoleCheckBox`.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample10.aspx)]

Nossa tarefa final é concluir o manipulador de eventos `RoleCheckBox_CheckChanged`. Precisamos começar referenciando o controle CheckBox que disparou o evento porque essa instância de CheckBox nos informa qual função foi marcada ou desmarcada por meio de suas `Text` e `Checked` Propriedades. Usando essas informações junto com o nome do usuário selecionado, adicionamos ou removemos o usuário da função por meio do método [`AddUserToRole`](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) ou [`RemoveUserFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx)da classe de `Roles`.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample11.cs)]

O código acima começa pela referência programática à caixa de seleção que gerou o evento, que está disponível por meio do parâmetro de entrada `sender`. Se a caixa de seleção estiver marcada, o usuário selecionado será adicionado à função especificada, caso contrário, eles serão removidos da função. Em ambos os casos, o rótulo `ActionStatus` exibe uma mensagem Resumindo a ação que acabou de ser executada.

Reserve um tempo para testar esta página por meio de um navegador. Selecione usuário Tito e, em seguida, adicione Tito às funções Administradores e supervisores.

[![Tito foi adicionado às funções Administradores e supervisores](assigning-roles-to-users-cs/_static/image8.png)](assigning-roles-to-users-cs/_static/image7.png)

**Figura 3**: Tito foi adicionado às funções Administrators e supervisores ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-cs/_static/image9.png))

Em seguida, selecione usuário Bruce na lista suspensa. Há um postback e as caixas de seleção do repetidor são atualizadas por meio do `CheckRolesForSelectedUser`. Como o Bruce ainda não pertence a nenhuma função, as duas caixas de seleção são desmarcadas. Em seguida, adicione Bruce à função de supervisores.

[o ![Bruce foi adicionado à função de supervisores](assigning-roles-to-users-cs/_static/image11.png)](assigning-roles-to-users-cs/_static/image10.png)

**Figura 4**: Bruce foi adicionado à função de supervisores ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-cs/_static/image12.png))

Para verificar melhor a funcionalidade do método `CheckRolesForSelectedUser`, selecione um usuário diferente de Tito ou Bruce. Observe como as caixas de seleção são desmarcadas automaticamente, indicando que elas não pertencem a nenhuma função. Retorne para Tito. As caixas de seleção administradores e supervisores devem ser verificadas.

## <a name="step-2-building-the-by-roles-user-interface"></a>Etapa 2: criando a interface do usuário "por funções"

Neste ponto, concluímos a interface "por usuários" e estamos prontos para começar a lidar com a interface "por funções". A interface "por funções" solicita que o usuário selecione uma função em uma lista suspensa e, em seguida, exibe o conjunto de usuários que pertencem a essa função em um GridView.

Adicione outro controle DropDownList à página `UsersAndRoles.aspx`. Coloque esse um abaixo do controle Repeater, nomeie-o `RoleList`e defina sua propriedade `AutoPostBack` como true. Abaixo disso, adicione um GridView e nomeie-o `RolesUserList`. Este GridView listará os usuários que pertencem à função selecionada. Defina a propriedade `AutoGenerateColumns` do GridView como false, adicione um TemplateField à coleção de `Columns` da grade e defina sua propriedade `HeaderText` como "Users". Defina o `ItemTemplate` do TemplateField para que ele exiba o valor da expressão DataBinding `Container.DataItem` na propriedade `Text` de um rótulo chamado `UserNameLabel`.

Depois de adicionar e configurar o GridView, a marcação declarativa da interface "por função" deve ser semelhante ao seguinte:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample12.aspx)]

Precisamos preencher o `RoleList` DropDownList com o conjunto de funções no sistema. Para fazer isso, atualize o método `BindRolesToList` de modo que seja associado a matriz de cadeia de caracteres retornada pelo método `Roles.GetAllRoles` ao `RolesList` DropDownList (bem como o repetidor de `UsersRoleList`).

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample13.cs)]

As duas últimas linhas no método `BindRolesToList` foram adicionadas para associar o conjunto de funções ao controle `RoleList` DropDownList. A Figura 5 mostra o resultado final quando exibido por meio de um navegador – uma lista suspensa preenchida com as funções do sistema.

[![as funções são exibidas na Rolelist DropDownList](assigning-roles-to-users-cs/_static/image14.png)](assigning-roles-to-users-cs/_static/image13.png)

**Figura 5**: as funções são exibidas na `RoleList` DropDownList ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-cs/_static/image15.png))

### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Exibindo os usuários que pertencem à função selecionada

Quando a página é carregada pela primeira vez ou quando uma nova função é selecionada na `RoleList` DropDownList, precisamos exibir a lista de usuários que pertencem a essa função no GridView. Crie um método chamado `DisplayUsersBelongingToRole` usando o seguinte código:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample14.cs)]

Esse método começa obtendo a função selecionada do `RoleList` DropDownList. Em seguida, ele usa o [método`Roles.GetUsersInRole(roleName)`](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) para recuperar uma matriz de cadeia de caracteres dos nomes de usuário dos usuários que pertencem a essa função. Essa matriz é então associada à `RolesUserList` GridView.

Esse método precisa ser chamado em duas circunstâncias: quando a página é carregada inicialmente e quando a função selecionada no `RoleList` DropDownList é alterada. Portanto, atualize o manipulador de eventos `Page_Load` para que esse método seja invocado após a chamada para `CheckRolesForSelectedUser`. Em seguida, crie um manipulador de eventos para o evento de `SelectedIndexChanged` do `RoleList`e chame esse método de lá também.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample15.cs)]

Com esse código em vigor, o `RolesUserList` GridView deve exibir os usuários que pertencem à função selecionada. Como mostra a Figura 6, a função de supervisores consiste em dois membros: Bruce e Tito.

[![o GridView lista os usuários que pertencem à função selecionada](assigning-roles-to-users-cs/_static/image17.png)](assigning-roles-to-users-cs/_static/image16.png)

**Figura 6**: o GridView lista os usuários que pertencem à função selecionada ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-cs/_static/image18.png))

### <a name="removing-users-from-the-selected-role"></a>Removendo usuários da função selecionada

Vamos aumentar o `RolesUserList` GridView para que ele inclua uma coluna de botões "Remove". Clicar no botão "remover" de um usuário específico irá removê-los dessa função.

Comece adicionando um campo de botão de exclusão ao GridView. Faça esse campo aparecer como a mais à esquerda e altere sua propriedade `DeleteText` de "excluir" (o padrão) para "remover".

[![adicionar o](assigning-roles-to-users-cs/_static/image20.png)](assigning-roles-to-users-cs/_static/image19.png)

**Figura 7**: Adicionar o botão "remover" ao GridView ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-cs/_static/image21.png))

Quando o botão "remover" é clicado em um postback massacre e o evento de `RowDeleting` do GridView é gerado. Precisamos criar um manipulador de eventos para esse evento e escrever o código que remove o usuário da função selecionada. Crie o manipulador de eventos e, em seguida, adicione o seguinte código:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample16.cs)]

O código começa determinando o nome da função selecionada. Em seguida, ele faz referência programaticamente ao controle `UserNameLabel` da linha cujo botão "remover" foi clicado para determinar o nome de usuário a ser removido. O usuário é removido da função por meio de uma chamada para o método `Roles.RemoveUserFromRole`. Em seguida, o `RolesUserList` GridView é atualizado e uma mensagem é exibida por meio do controle rótulo de `ActionStatus`.

> [!NOTE]
> O botão "remover" não requer nenhum tipo de confirmação do usuário antes de remover o usuário da função. Convido você a adicionar algum nível de confirmação do usuário. Uma das maneiras mais fáceis de confirmar uma ação é por meio de uma caixa de diálogo de confirmação do lado do cliente. Para obter mais informações sobre essa técnica, consulte [adicionando confirmação do lado do cliente ao excluir](https://asp.net/learn/data-access/tutorial-42-cs.aspx).

A Figura 8 mostra a página após o usuário Tito ter sido removido do grupo de supervisores.

[![infelizmente, o Tito não é mais um supervisor](assigning-roles-to-users-cs/_static/image23.png)](assigning-roles-to-users-cs/_static/image22.png)

**Figura 8**: Infelizmente, o Tito não é mais um supervisor ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-cs/_static/image24.png))

### <a name="adding-new-users-to-the-selected-role"></a>Adicionando novos usuários à função selecionada

Junto com a remoção de usuários da função selecionada, o visitante dessa página também deve ser capaz de adicionar um usuário à função selecionada. A melhor interface para adicionar um usuário à função selecionada depende do número de contas de usuário que você espera ter. Se seu site alojar apenas algumas dúzias de contas de usuário ou menos, você pode usar uma DropDownList aqui. Se pode haver milhares de contas de usuário, você desejaria incluir uma interface do usuário que permita que o visitante percorra as contas, pesquise por uma conta específica ou filtre as contas de usuário de alguma outra maneira.

Para esta página, vamos usar uma interface muito simples que funciona independentemente do número de contas de usuário no sistema. Ou seja, usaremos uma caixa de texto, solicitando que o visitante digite o nome do usuário que deseja adicionar à função selecionada. Se não existir nenhum usuário com esse nome, ou se o usuário já for um membro da função, exibiremos uma mensagem no rótulo `ActionStatus`. Mas se o usuário existir e não for um membro da função, vamos adicioná-los à função e atualizar a grade.

Adicione uma caixa de texto e um botão abaixo do GridView. Defina a `ID` da caixa de texto como `UserNameToAddToRole` e defina as propriedades `ID` e `Text` do botão como `AddUserToRoleButton` e "Adicionar usuário à função", respectivamente.

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample17.aspx)]

Em seguida, crie um manipulador de eventos `Click` para o `AddUserToRoleButton` e adicione o seguinte código:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample18.cs)]

A maior parte do código no manipulador de eventos `Click` executa várias verificações de validação. Ele garante que o visitante forneceu um nome de usuário na caixa de texto `UserNameToAddToRole`, que o usuário existe no sistema e que ele ainda não pertence à função selecionada. Se qualquer uma dessas verificações falhar, uma mensagem apropriada será exibida no `ActionStatus` e o manipulador de eventos será encerrado. Se todas as verificações forem aprovadas, o usuário será adicionado à função por meio do método `Roles.AddUserToRole`. Depois disso, a propriedade de `Text` da caixa de texto é removida, o GridView é atualizado e o rótulo de `ActionStatus` exibe uma mensagem indicando que o usuário especificado foi adicionado com êxito à função selecionada.

> [!NOTE]
> Para garantir que o usuário especificado ainda não pertença à função selecionada, usamos o [método`Roles.IsUserInRole(userName, roleName)`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), que retorna um valor booliano indicando se *username* é membro de *roleName*. Usaremos esse método novamente no <a id="_msoanchor_2"> </a> [próximo tutorial](role-based-authorization-cs.md) quando examinarmos a autorização baseada em função.

Visite a página por meio de um navegador e selecione a função de supervisores na `RoleList` DropDownList. Tente inserir um nome de usuário inválido – você deverá ver uma mensagem explicando que o usuário não existe no sistema.

[![você não pode adicionar um usuário não existente a uma função](assigning-roles-to-users-cs/_static/image26.png)](assigning-roles-to-users-cs/_static/image25.png)

**Figura 9**: não é possível adicionar um usuário não existente a uma função ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-cs/_static/image27.png))

Agora, tente adicionar um usuário válido. Vá em frente e adicione novamente Tito à função de supervisores.

[![Tito é novamente um supervisor!](assigning-roles-to-users-cs/_static/image29.png)](assigning-roles-to-users-cs/_static/image28.png)

**Figura 10**: o Tito é novamente um supervisor!  ([Clique para exibir a imagem em tamanho normal](assigning-roles-to-users-cs/_static/image30.png))

## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Etapa 3: atualização cruzada das interfaces "por usuário" e "por função"

A página `UsersAndRoles.aspx` oferece duas interfaces distintas para gerenciar usuários e funções. Atualmente, essas duas interfaces atuam de forma independente uma da outra para que seja possível que uma alteração feita em uma interface não seja refletida imediatamente no outro. Por exemplo, imagine que o visitante da página selecione a função de supervisores da `RoleList` DropDownList, que lista Bruce e Tito como seus membros. Em seguida, o visitante seleciona Tito na `UserList` DropDownList, que verifica as caixas de seleção administradores e supervisores no repetidor de `UsersRoleList`. Se o visitante desmarcar a função de supervisor do repetidor, o Tito será removido da função de supervisores, mas essa modificação não será refletida na interface "por função". O GridView ainda mostrará Tito como membro da função de supervisores.

Para corrigir isso, precisamos atualizar o GridView sempre que uma função é marcada ou desmarcada no repetidor de `UsersRoleList`. Da mesma forma, precisamos atualizar o repetidor sempre que um usuário é removido ou adicionado a uma função da interface "por função".

O repetidor na interface "por usuário" é atualizado chamando o método `CheckRolesForSelectedUser`. A interface "por função" pode ser modificada no manipulador de eventos `RowDeleting` do `RolesUserList` GridView e no manipulador de eventos `Click` do botão `AddUserToRoleButton`. Portanto, precisamos chamar o método `CheckRolesForSelectedUser` de cada um desses métodos.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample19.cs)]

Da mesma forma, o GridView na interface "por função" é atualizado chamando o método `DisplayUsersBelongingToRole` e a interface "por usuário" é modificada por meio do manipulador de eventos `RoleCheckBox_CheckChanged`. Portanto, precisamos chamar o método `DisplayUsersBelongingToRole` a partir desse manipulador de eventos.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample20.cs)]

Com essas pequenas alterações de código, as interfaces "por usuário" e "por função" agora são atualizadas corretamente. Para verificar isso, visite a página por meio de um navegador e selecione Tito e supervisores na `UserList` e `RoleList` DropDownLists, respectivamente. Observe que, à medida que você desmarcar a função de supervisores para Tito do repetidor na interface "por usuário", Tito será automaticamente removido do GridView na interface "por função". Adicionar Tito de volta à função de supervisores da interface "por função" verifica automaticamente a caixa de seleção supervisores na interface "por usuário".

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Etapa 4: Personalizando o CreateUserWizard para incluir uma etapa "especificar funções"

<a id="_msoanchor_3"> </a>No tutorial [*criando contas de usuário*](../membership/creating-user-accounts-cs.md) , vimos como usar o controle da Web CreateUserWizard para fornecer uma interface para a criação de uma nova conta de usuário. O controle CreateUserWizard pode ser usado de uma das duas maneiras:

- Como um meio para os visitantes criarem sua própria conta de usuário no site e
- Como um meio para os administradores criarem novas contas

No primeiro caso de uso, um visitante chega ao site e preenche o CreateUserWizard, inserindo suas informações para se registrar no site. No segundo caso, um administrador cria uma nova conta para outra pessoa.

Quando uma conta está sendo criada por um administrador para alguma outra pessoa, pode ser útil permitir que o administrador especifique a quais funções a nova conta de usuário pertence. No tutorial [ *armazenamento* de *informações adicionais do usuário* ](../membership/storing-additional-user-information-cs.md) , vimos como personalizar o CreateUserWizard adicionando `WizardSteps`adicionais. <a id="_msoanchor_4"> </a> Vejamos como adicionar uma etapa adicional ao CreateUserWizard a fim de especificar as funções do novo usuário.

Abra a página `CreateUserWizardWithRoles.aspx` e adicione um controle CreateUserWizard chamado `RegisterUserWithRoles`. Defina a propriedade `ContinueDestinationPageUrl` do controle como "~/default.aspx". Como a ideia aqui é que um administrador usará esse controle CreateUserWizard para criar novas contas de usuário, defina a propriedade de `LoginCreatedUser` do controle como false. Essa propriedade `LoginCreatedUser` especifica se o visitante é automaticamente conectado como o usuário recém-criado e assume como padrão true. Definimos isso como false porque, quando um administrador cria uma nova conta, queremos mantê-lo conectado como próprio.

Em seguida, selecione "Adicionar/remover `WizardSteps`..." opção da marca inteligente do CreateUserWizard e adicione uma nova `WizardStep`, definindo sua `ID` como `SpecifyRolesStep`. Mova o `SpecifyRolesStep WizardStep` para que ele venha após a etapa "inscrever-se para sua nova conta", mas antes da etapa "Concluir". Defina a propriedade `Title` do `WizardStep`como "especificar funções", sua propriedade `StepType` como `Step`e sua propriedade `AllowReturn` como false.

[![adicionar o](assigning-roles-to-users-cs/_static/image32.png)](assigning-roles-to-users-cs/_static/image31.png)

**Figura 11**: adicionar a `WizardStep` "especificar funções" ao CreateUserWizard ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-cs/_static/image33.png))

Após essa alteração, a marcação declarativa de CreateUserWizard deve ser semelhante ao seguinte:

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample21.aspx)]

Na `WizardStep`"especificar funções", adicione um CheckBoxList chamado `RoleList`. Este CheckBoxList listará as funções disponíveis, habilitando a pessoa que visita a página para verificar a quais funções o usuário recém-criado pertence.

Deixamos de lado duas tarefas de codificação: primeiro, devemos preencher o `RoleList` CheckBoxList com as funções no sistema; em segundo lugar, precisamos adicionar o usuário criado às funções selecionadas quando o usuário se move da etapa "especificar funções" para a etapa "Concluir". Podemos realizar a primeira tarefa no manipulador de eventos `Page_Load`. O código a seguir faz referência programaticamente à caixa de seleção `RoleList` na primeira visita à página e associa as funções no sistema a ela.

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample22.cs)]

O código acima deve parecer familiar. No tutorial [ *armazenamento* de *informações adicionais do usuário* ](../membership/storing-additional-user-information-cs.md) , usamos duas instruções `FindControl` para fazer referência a um controle da Web de dentro de um `WizardStep`personalizado. <a id="_msoanchor_5"> </a> E o código que associa as funções à CheckBoxList foi extraído anteriormente neste tutorial.

A fim de executar a segunda tarefa de programação, precisamos saber quando a etapa "especificar funções" foi concluída. Lembre-se de que o CreateUserWizard tem um evento `ActiveStepChanged`, que é disparado cada vez que o visitante navega de uma etapa para outra. Aqui, podemos determinar se o usuário atingiu a etapa "completa"; Nesse caso, precisamos adicionar o usuário às funções selecionadas.

Crie um manipulador de eventos para o evento `ActiveStepChanged` e adicione o seguinte código:

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample23.cs)]

Se o usuário acabou de chegar à etapa "concluído", o manipulador de eventos enumera os itens do `RoleList` CheckBoxList e o usuário recém criado é atribuído às funções selecionadas.

Visite esta página por meio de um navegador. A primeira etapa no CreateUserWizard é a etapa padrão "inscrever-se para sua nova conta", que solicita o nome de usuário, a senha, o email e outras informações de chave do novo usuários. Insira as informações para criar um novo usuário chamado Wanda.

[![criar um novo usuário chamado Wanda](assigning-roles-to-users-cs/_static/image35.png)](assigning-roles-to-users-cs/_static/image34.png)

**Figura 12**: criar um novo usuário chamado Wanda ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-cs/_static/image36.png))

Clique no botão "criar usuário". O CreateUserWizard chama internamente o método `Membership.CreateUser`, criando a nova conta de usuário e, em seguida, avança para a próxima etapa, "especificar funções". Aqui, as funções do sistema são listadas. Marque a caixa de seleção supervisores e clique em Avançar.

[![tornar o Wanda um membro da função de supervisores](assigning-roles-to-users-cs/_static/image38.png)](assigning-roles-to-users-cs/_static/image37.png)

**Figura 13**: tornar o Wanda um membro da função de supervisores ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-cs/_static/image39.png))

Clicar em Avançar causa um postback e atualiza o `ActiveStep` para a etapa "Concluir". No manipulador de eventos `ActiveStepChanged`, a conta de usuário criada recentemente é atribuída à função de supervisores. Para verificar isso, retorne à página `UsersAndRoles.aspx` e selecione os supervisores na `RoleList` DropDownList. Como mostra a Figura 14, os supervisores agora são compostos por três usuários: Bruce, Tito e Wanda.

[![Bruce, Tito e Wanda são supervisores](assigning-roles-to-users-cs/_static/image41.png)](assigning-roles-to-users-cs/_static/image40.png)

**Figura 14**: Bruce, Tito e Wanda são supervisores ([clique para exibir a imagem em tamanho normal](assigning-roles-to-users-cs/_static/image42.png))

## <a name="summary"></a>Resumo

A estrutura de funções oferece métodos para recuperar informações sobre funções e métodos de um usuário específico para determinar quais usuários pertencem a uma função especificada. Além disso, há vários métodos para adicionar e remover um ou mais usuários de uma ou mais funções. Neste tutorial, nos concentramos em apenas dois desses métodos: `AddUserToRole` e `RemoveUserFromRole`. Há variantes adicionais projetadas para adicionar vários usuários a uma única função e para atribuir várias funções a um único usuário.

Este tutorial também incluiu uma visão de como estender o controle CreateUserWizard para incluir um `WizardStep` para especificar as funções de usuário recém-criadas. Essa etapa pode ajudar um administrador a simplificar o processo de criação de contas de usuário para novos usuários.

Neste ponto, vimos como criar e excluir funções e como adicionar e remover usuários de funções. Mas ainda temos que examinar a aplicação da autorização baseada em função. No <a id="_msoanchor_6"> </a> [tutorial a seguir](role-based-authorization-cs.md) , veremos como definir regras de autorização de URL por função, além de como limitar a funcionalidade em nível de página com base nas funções do usuário conectado no momento.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Visão geral da ferramenta de administração de site ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx)
- [Examinando ASP. Associação, funções e perfil da rede](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Distribuindo sua própria ferramenta de administração de sites](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP. NET e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a *[Sams ensina a ASP.NET 2,0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott pode ser contatado em [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a...

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Teresa Murphy. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-and-managing-roles-cs.md)
> [Próximo](role-based-authorization-cs.md)
