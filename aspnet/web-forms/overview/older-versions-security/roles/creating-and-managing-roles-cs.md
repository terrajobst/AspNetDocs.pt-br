---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: Criando e gerenciando funçõesC#() | Microsoft Docs
author: rick-anderson
description: Este tutorial examina as etapas necessárias para configurar a estrutura de funções. Depois disso, criaremos páginas da Web para criar e excluir funções.
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: a7883d0b05f2fa5a3fdac887f8c8b39d70418fb3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640241"
---
# <a name="creating-and-managing-roles-c"></a>Criar e gerenciar funções (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip) ou [baixar PDF](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> Este tutorial examina as etapas necessárias para configurar a estrutura de funções. Depois disso, criaremos páginas da Web para criar e excluir funções.

## <a name="introduction"></a>Introdução

No tutorial <a id="_msoanchor_1"> </a>de [*autorização baseada no usuário*](../membership/user-based-authorization-cs.md) , examinamos o uso da autorização de URL para restringir determinados usuários de um conjunto de páginas e exploramos técnicas declarativas e Programáticas para ajustar a funcionalidade de uma página ASP.NET com base no usuário visitante. No entanto, conceder permissão para acesso à página ou funcionalidade por usuário pode se tornar um pesadelo de manutenção em cenários em que há muitas contas de usuário ou quando os privilégios dos usuários são alterados com frequência. Sempre que um usuário Obtém ou perde autorização para executar uma tarefa específica, o administrador precisa atualizar as regras de autorização de URL apropriadas, a marcação declarativa e o código.

Geralmente, ele ajuda a classificar os usuários em grupos ou *funções* e, em seguida, aplicar permissões a uma base função por função. Por exemplo, a maioria dos aplicativos Web tem um determinado conjunto de páginas ou tarefas que são reservadas somente para usuários administrativos. Usando as técnicas aprendidas no tutorial de *autorização baseada no usuário* , adicionaremos as regras de autorização de URL apropriadas, a marcação declarativa e o código para permitir que as contas de usuário especificadas executem tarefas administrativas. Mas se um novo administrador tiver sido adicionado ou se um administrador existente precisar ter seus direitos de administração revogados, precisaremos retornar e atualizar os arquivos de configuração e as páginas da Web. Com as funções, no entanto, poderíamos criar uma função chamada Administrators e atribuir esses usuários confiáveis à função Administrators. Em seguida, adicionaremos as regras de autorização de URL apropriadas, marcação declarativa e código para permitir que a função Administradores execute as várias tarefas administrativas. Com essa infraestrutura em vigor, adicionar novos administradores ao site ou remover os existentes é tão simples quanto incluir ou remover o usuário da função Administradores. Nenhuma configuração, marcação declarativa ou alteração de código é necessária.

O ASP.NET oferece uma estrutura de funções para definir funções e associá-las a contas de usuário. Com a estrutura de funções, podemos criar e excluir funções, adicionar usuários ou remover usuários de uma função, determinar o conjunto de usuários que pertencem a uma função específica e informar se um usuário pertence a uma função específica. Depois que a estrutura de funções tiver sido configurada, podemos limitar o acesso a páginas por função por meio de regras de autorização de URL e mostrar ou ocultar informações ou funcionalidades adicionais em uma página com base nas funções do usuário conectado no momento.

Este tutorial examina as etapas necessárias para configurar a estrutura de funções. Depois disso, criaremos páginas da Web para criar e excluir funções. <a id="_msoanchor_2"> </a>No tutorial [*atribuindo funções a usuários*](assigning-roles-to-users-cs.md) , veremos como adicionar e remover usuários de funções. E, no <a id="_msoanchor_3"> </a>tutorial de [*autorização baseada em função*](role-based-authorization-cs.md) , veremos como limitar o acesso a páginas por função, juntamente com como ajustar a funcionalidade da página dependendo da função do usuário de visita. Vamos começar!

## <a name="step-1-adding-new-aspnet-pages"></a>Etapa 1: adicionando novas páginas do ASP.NET

Neste tutorial e nos próximos dois, examinaremos várias funções e recursos relacionados a funções. Precisaremos de uma série de páginas do ASP.NET para implementar os tópicos examinados em todos esses tutoriais. Vamos criar essas páginas e atualizar o mapa do site.

Comece criando uma nova pasta no projeto chamada `Roles`. Em seguida, adicione quatro novas páginas ASP.NET à pasta `Roles`, vinculando cada página com a `Site.master` página mestra. Nomeie as páginas:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

Neste ponto, o Gerenciador de Soluções do seu projeto deve ser semelhante à captura de tela mostrada na Figura 1.

[![quatro novas páginas foram adicionadas à pasta funções](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**Figura 1**: quatro novas páginas foram adicionadas à pasta `Roles` ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image3.png))

Cada página deve, neste ponto, ter os dois controles de conteúdo, um para cada um dos ContentPlaceHolders da página mestra: `MainContent` e `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

Lembre-se de que a marcação padrão do `LoginContent` ContentPlaceHolder exibe um link para fazer logon ou logoff do site, dependendo se o usuário está autenticado. No entanto, a presença do controle de conteúdo de `Content2` na página ASP.NET substitui a marcação padrão da página mestra. Como discutimos em <a id="_msoanchor_4"> </a> [*uma visão geral do tutorial de autenticação de formulários*](../introduction/an-overview-of-forms-authentication-cs.md) , a substituição da marcação padrão é útil em páginas nas quais não queremos exibir opções relacionadas ao logon na coluna à esquerda.

Para essas quatro páginas, no entanto, queremos mostrar a marcação padrão da página mestra para o `LoginContent` ContentPlaceHolder. Portanto, remova a marcação declarativa para o controle de conteúdo `Content2`. Depois de fazer isso, cada uma das quatro marcas da página deve conter apenas um controle de conteúdo.

Por fim, vamos atualizar o mapa do site (`Web.sitemap`) para incluir essas novas páginas da Web. Adicione o seguinte XML após o `<siteMapNode>` que adicionamos para os tutoriais de associação.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

Com o mapa do site atualizado, visite o site por meio de um navegador. Como mostra a Figura 2, a navegação à esquerda agora inclui itens para os tutoriais de funções.

[![quatro novas páginas foram adicionadas à pasta funções](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**Figura 2**: quatro novas páginas foram adicionadas à pasta `Roles` ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image6.png))

## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Etapa 2: especificando e Configurando o provedor de funções de estrutura

Como a estrutura de associação, a estrutura de funções é criada acima do modelo de provedor. Conforme discutido no tutorial <a id="_msoanchor_5">de </a> [*suporte básico de segurança e ASP.net*](../introduction/security-basics-and-asp-net-support-cs.md) , o .NET Framework é fornecido com três provedores de funções internas: [`AuthorizationStoreRoleProvider`](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx), [`WindowsTokenRoleProvider`](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx)e [`SqlRoleProvider`](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Esta série de tutoriais se concentra na `SqlRoleProvider`, que usa um banco de dados Microsoft SQL Server como o armazenamento de função.

Embaixo do, a estrutura de funções e `SqlRoleProvider` funcionam da mesma forma que a estrutura de associação e `SqlMembershipProvider`. O .NET Framework contém uma classe `Roles` que serve como a API para a estrutura de funções. A classe `Roles` tem métodos estáticos como `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`e assim por diante. Quando um desses métodos é invocado, a classe `Roles` delega a chamada para o provedor configurado. O `SqlRoleProvider` funciona com as tabelas específicas de função (`aspnet_Roles` e `aspnet_UsersInRoles`) em resposta.

Para usar o provedor de `SqlRoleProvider` em nosso aplicativo, precisamos especificar qual banco de dados usar como repositório. O `SqlRoleProvider` espera que o repositório de função especificado tenha determinadas tabelas de banco de dados, exibições e procedimentos armazenados. Esses objetos de banco de dados de requisito podem ser adicionados usando a [ferramenta de`aspnet_regsql.exe`](https://msdn.microsoft.com/library/ms229862.aspx). Neste ponto, já temos um banco de dados com o esquema necessário para o `SqlRoleProvider`. De volta ao <a id="_msoanchor_6"> </a>tutorial [*criando o esquema de associação no SQL Server*](../membership/creating-the-membership-schema-in-sql-server-cs.md) , criamos um banco de dados chamado `SecurityTutorials.mdf` e usamos `aspnet_regsql.exe` para adicionar os serviços de aplicativo, que incluíam os objetos de banco de dados exigidos pelo `SqlRoleProvider`. Portanto, precisamos apenas dizer à estrutura de funções para habilitar o suporte de função e usar o `SqlRoleProvider` com o banco de dados `SecurityTutorials.mdf` como o armazenamento de função.

A estrutura de funções é configurada por meio do elemento &lt;`roleManager`&gt; no arquivo de `Web.config` do aplicativo. Por padrão, o suporte à função está desabilitado. Para habilitá-lo, você deve definir a [&lt;`roleManager`](https://msdn.microsoft.com/library/ms164660.aspx) atributo de `enabled` do elemento &gt;para `true` da seguinte forma:

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

Por padrão, todos os aplicativos Web têm um provedor de funções chamado `AspNetSqlRoleProvider` do tipo `SqlRoleProvider`. Esse provedor padrão é registrado no `machine.config` (localizado em `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

O atributo `connectionStringName` do provedor especifica o repositório de função que é usado. O provedor de `AspNetSqlRoleProvider` define esse atributo como `LocalSqlServer`, que também é definido em `machine.config` e pontos, por padrão, para um banco de dados SQL Server 2005 Express Edition na pasta `App_Data` chamada `aspnet.mdf`.

Consequentemente, se simplesmente habilitarmos a estrutura de funções sem especificar nenhuma informação de provedor no arquivo de `Web.config` do nosso aplicativo, o aplicativo usará o provedor de funções registradas padrão, `AspNetSqlRoleProvider`. Se o banco de dados `~/App_Data/aspnet.mdf` não existir, o tempo de execução do ASP.NET o criará automaticamente e adicionará o esquema de serviços de aplicativo. No entanto, não queremos usar o banco de dados `aspnet.mdf`; em vez disso, queremos usar o banco de dados `SecurityTutorials.mdf` que já criamos e adicionamos o esquema de serviços de aplicativo ao. Essa modificação pode ser realizada de duas maneiras:

- <strong>Especifique um valor para o nome da</strong> cadeia de conexão<strong>`LocalSqlServer`</strong> <strong>em</strong> <strong>`Web.config`</strong> <strong>.</strong> Ao substituir o valor do nome da cadeia de conexão `LocalSqlServer` no `Web.config`, podemos usar o provedor de funções registradas padrão (`AspNetSqlRoleProvider`) e fazê-lo funcionar corretamente com o banco de dados `SecurityTutorials.mdf`. Para obter mais informações sobre essa técnica, consulte a postagem de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/), [configurando ASP.NET 2,0 Serviços de Aplicativos para usar SQL Server 2000 ou SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Adicione um novo provedor registrado do tipo</strong> <strong>`SqlRoleProvider`</strong> <strong>e defina sua</strong> configuração de<strong>`connectionStringName`</strong> <strong>para apontar para o banco de</strong> dados<strong>`SecurityTutorials.mdf`</strong> <strong>.</strong> Essa é a abordagem recomendada e usada no <a id="_msoanchor_7"> </a>tutorial [*criando o esquema de associação no SQL Server*](../membership/creating-the-membership-schema-in-sql-server-cs.md) , e é a abordagem que usarei neste tutorial também.

Adicione a seguinte marcação de configuração de funções ao arquivo de `Web.config`. Essa marcação registra um novo provedor chamado `SecurityTutorialsSqlRoleProvider`.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

A marcação acima define o `SecurityTutorialsSqlRoleProvider` como o provedor padrão (por meio do atributo `defaultProvider` no elemento `<roleManager>`). Ele também define a configuração de `applicationName` do `SecurityTutorialsSqlRoleProvider`como `SecurityTutorials`, que é a mesma `applicationName` configuração usada pelo provedor de associação (`SecurityTutorialsSqlMembershipProvider`). Embora não seja mostrado aqui, o [elemento`<add>`](https://msdn.microsoft.com/library/ms164662.aspx) para a `SqlRoleProvider` também pode conter um atributo `commandTimeout` para especificar a duração do tempo limite do banco de dados, em segundos. O valor padrão é 30.

Com essa marcação de configuração in-loco, estamos prontos para começar a usar a funcionalidade de função em nosso aplicativo.

> [!NOTE]
> A marcação de configuração acima ilustra o uso dos atributos `enabled` e `defaultProvider` do elemento &lt;`roleManager`&gt;. Há vários outros atributos que afetam o modo como a estrutura de funções associa as informações de função de cada usuário. Examinaremos essas configurações no <a id="_msoanchor_8"> </a>tutorial de [*autorização baseada em funções*](role-based-authorization-cs.md) .

## <a name="step-3-examining-the-roles-api"></a>Etapa 3: examinando a API de funções

A funcionalidade da estrutura de funções é exposta por meio da [classe`Roles`](https://msdn.microsoft.com/library/system.web.security.roles.aspx), que contém treze métodos estáticos para executar operações baseadas em funções. Quando examinamos a criação e a exclusão de funções nas etapas 4 e 6, usaremos os métodos [`CreateRole`](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) e [`DeleteRole`](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) , que adicionam ou removem uma função do sistema.

Para obter uma lista de todas as funções no sistema, use o [método`GetAllRoles`](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (consulte a etapa 5). O [método`RoleExists`](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) retorna um valor booliano que indica se existe uma função especificada.

No próximo tutorial, examinaremos como associar usuários a funções. Os métodos [`AddUserToRole`](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [`AddUserToRoles`](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [`AddUsersToRole`](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx)e [`AddUsersToRoles`](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) da classe `Roles` adicionam um ou mais usuários a uma ou mais funções. Para remover usuários de funções, use os métodos [`RemoveUserFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [`RemoveUserFromRoles`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [`RemoveUsersFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx)ou [`RemoveUsersFromRoles`](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) .

<a id="_msoanchor_9"> </a>No tutorial de [*autorização baseada em funções*](role-based-authorization-cs.md) , veremos maneiras de mostrar ou ocultar programaticamente a funcionalidade com base na função do usuário conectado no momento. Para fazer isso, podemos usar os métodos [`FindUsersInRole`](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [`GetRolesForUser`](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [`GetUsersInRole`](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx)ou [`IsUserInRole`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) da classe `Role`.

> [!NOTE]
> Tenha em mente que, sempre que um desses métodos for invocado, a classe `Roles` delegará a chamada para o provedor configurado. Em nosso caso, isso significa que a chamada está sendo enviada para o `SqlRoleProvider`. Em seguida, a `SqlRoleProvider` executa a operação de banco de dados apropriada com base no método invocado. Por exemplo, o código `Roles.CreateRole("Administrators")` resulta na `SqlRoleProvider` execução do procedimento armazenado `aspnet_Roles_CreateRole`, que insere um novo registro na tabela `aspnet_Roles` chamada Administrators.

O restante deste tutorial analisa o uso dos métodos `CreateRole`, `GetAllRoles`e `DeleteRole` da classe `Roles` para gerenciar as funções no sistema.

## <a name="step-4-creating-new-roles"></a>Etapa 4: criando novas funções

As funções oferecem uma maneira de agrupar arbitrariamente os usuários e, mais comumente, esse agrupamento é usado para uma maneira mais conveniente de aplicar regras de autorização. Mas, para usar funções como um mecanismo de autorização, primeiro precisamos definir quais funções existem no aplicativo. Infelizmente, o ASP.NET não inclui um controle CreateRoleWizard. Para adicionar novas funções, precisamos criar uma interface do usuário adequada e invocar a API de funções sozinhos. A boa notícia é que isso é muito fácil de realizar.

> [!NOTE]
> Embora não haja nenhum controle da Web CreateRoleWizard, há a [ferramenta de administração de site da ASP.net](https://msdn.microsoft.com/library/ms228053.aspx), que é um aplicativo ASP.net local projetado para ajudar a exibir e gerenciar a configuração de seu aplicativo Web. No entanto, não sou um grande fã da ferramenta de administração de site ASP.NET por dois motivos. Primeiro, trata-se de um pouco de bugs e a experiência do usuário deixa muito a desejar. Segundo, a ferramenta de administração de site ASP.NET foi projetada para funcionar localmente, o que significa que você terá que criar suas próprias páginas da Web de gerenciamento de funções se precisar gerenciar funções em um site ativo remotamente. Por esses dois motivos, este tutorial e o próximo se concentrarão na criação das ferramentas de gerenciamento de função necessárias em uma página da Web, em vez de depender da ferramenta de administração de site ASP.NET.

Abra a página `ManageRoles.aspx` na pasta `Roles` e adicione uma caixa de texto e um controle botão da Web para a página. Defina a propriedade `ID` do controle TextBox como `RoleName` e as propriedades `ID` e `Text` do botão como `CreateRoleButton` e crie a função, respectivamente. Neste ponto, a marcação declarativa de sua página deve ser semelhante ao seguinte:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

Em seguida, clique duas vezes no controle botão de `CreateRoleButton` no designer para criar um manipulador de eventos `Click` e, em seguida, adicione o seguinte código:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

O código acima começa atribuindo o nome da função cortada inserida na caixa de texto `RoleName` à variável `newRoleName`. Em seguida, o método `RoleExists` da classe `Roles` é chamado para determinar se a função `newRoleName` já existe no sistema. Se a função não existir, ela será criada por meio de uma chamada para o método `CreateRole`. Se o método de `CreateRole` for passado um nome de função que já existe no sistema, uma exceção de `ProviderException` será lançada. É por isso que o código verifica primeiro se a função ainda não existe no sistema antes de chamar `CreateRole`. O manipulador de eventos `Click` é concluído desmarcando a propriedade `Text` da `RoleName` caixa de texto.

> [!NOTE]
> Você deve estar se perguntando o que acontecerá se o usuário não inserir nenhum valor na caixa de texto `RoleName`. Se o valor passado para o método `CreateRole` for `null` ou uma cadeia de caracteres vazia, uma exceção será gerada. Da mesma forma, se o nome da função contiver uma vírgula, uma exceção será gerada. Consequentemente, a página deve conter controles de validação para garantir que o usuário insira uma função e que não contenha vírgulas. Eu deixe como um exercício para o leitor.

Vamos criar uma função chamada Administrators. Visite a página `ManageRoles.aspx` por meio de um navegador, digite administradores na caixa de texto (veja a Figura 3) e, em seguida, clique no botão criar função.

[![criar uma função de administradores](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**Figura 3**: criar uma função de administradores ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image9.png))

O que acontece? Ocorre um postback, mas não há nenhuma indicação visual de que a função foi realmente adicionada ao sistema. Atualizaremos esta página na etapa 5 para incluir comentários visuais. Por enquanto, no entanto, você pode verificar se a função foi criada acessando o banco de dados `SecurityTutorials.mdf` e exibindo-os na tabela `aspnet_Roles`. Como mostra a Figura 4, a tabela `aspnet_Roles` contém um registro para as funções de administradores recém-adicionadas.

[![a tabela aspnet_Roles tem uma linha para os administradores](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**Figura 4**: a tabela `aspnet_Roles` tem uma linha para os administradores ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image12.png))

## <a name="step-5-displaying-the-roles-in-the-system"></a>Etapa 5: exibindo as funções no sistema

Vamos aumentar a `ManageRoles.aspx` página para incluir uma lista das funções atuais no sistema. Para fazer isso, adicione um controle GridView à página e defina sua propriedade `ID` como `RoleList`. Em seguida, adicione um método à classe code-behind da página chamada `DisplayRolesInGrid` usando o seguinte código:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

O método `GetAllRoles` da classe `Roles` retorna todas as funções no sistema como uma matriz de cadeias de caracteres. Em seguida, essa matriz de cadeia de caracteres é associada ao GridView. Para associar a lista de funções ao GridView quando a página é carregada pela primeira vez, precisamos chamar o método `DisplayRolesInGrid` do manipulador de eventos `Page_Load` da página. O código a seguir chama esse método quando a página é visitada pela primeira vez, mas não em postbacks subsequentes.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

Com esse código em vigor, visite a página por meio de um navegador. Como mostra a Figura 5, você deve ver uma grade com uma única coluna rotulada item. A grade inclui uma linha para a função administradores que adicionamos na etapa 4.

[![o GridView exibe as funções em uma única coluna](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**Figura 5**: o GridView exibe as funções em uma única coluna ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image15.png))

O GridView exibe uma coluna solitário rotulada como item porque a propriedade `AutoGenerateColumns` do GridView está definida como true (o padrão), o que faz com que o GridView crie automaticamente uma coluna para cada propriedade em seu `DataSource`. Uma matriz tem uma única propriedade que representa os elementos na matriz, portanto, a única coluna no GridView.

Ao exibir dados com um GridView, prefiro definir explicitamente minhas colunas em vez de tê-las implicitamente geradas pelo GridView. Ao definir explicitamente as colunas, é muito mais fácil formatar os dados, reorganizá-las e executar outras tarefas comuns. Portanto, vamos atualizar a marcação declarativa do GridView para que suas colunas sejam definidas explicitamente.

Comece definindo a propriedade `AutoGenerateColumns` do GridView como false. Em seguida, adicione um TemplateField à grade, defina sua propriedade `HeaderText` como funções e configure seu `ItemTemplate` para que ele exiba o conteúdo da matriz. Para fazer isso, adicione um controle da Web de rótulo chamado `RoleNameLabel` ao `ItemTemplate` e associe sua propriedade `Text` a `Container.DataItem`.

Essas propriedades e o conteúdo do `ItemTemplate`podem ser definidos declarativamente ou por meio da caixa de diálogo campos do GridView e da interface de edição de modelos. Para acessar a caixa de diálogo campos, clique no link Editar colunas na marca inteligente do GridView. Em seguida, desmarque a caixa de seleção Gerar campos automaticamente para definir a propriedade `AutoGenerateColumns` como false e adicione um TemplateField ao GridView, definindo sua propriedade `HeaderText` como Role. Para definir o conteúdo do `ItemTemplate`, escolha a opção Editar modelos na marca inteligente do GridView. Arraste um controle de rótulo da Web para a `ItemTemplate`, defina sua propriedade `ID` como `RoleNameLabel`e defina suas configurações de associação de dados para que sua propriedade `Text` esteja associada a `Container.DataItem`.

Independentemente da abordagem que você usar, a marcação declarativa resultante do GridView deve ser semelhante ao seguinte quando você terminar.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> O conteúdo da matriz é exibido usando a sintaxe de vinculação de dados `<%# Container.DataItem %>`. Uma descrição completa do motivo pelo qual essa sintaxe é usada ao exibir o conteúdo de uma matriz associada ao GridView está além do escopo deste tutorial. Para obter mais informações sobre esse assunto, consulte [associando uma matriz escalar a um controle da Web de dados](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

Atualmente, o `RoleList` GridView só é associado à lista de funções quando a página é visitada pela primeira vez. Precisamos atualizar a grade sempre que uma nova função for adicionada. Para fazer isso, atualize o manipulador de eventos `Click` do botão `CreateRoleButton` para que ele chame o método `DisplayRolesInGrid` se uma nova função for criada.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

Agora, quando o usuário adiciona uma nova função, o `RoleList` GridView mostra a função recém-adicionada no postback, fornecendo comentários visuais de que a função foi criada com êxito. Para ilustrar isso, visite a página `ManageRoles.aspx` por meio de um navegador e adicione uma função denominada supervisores. Ao clicar no botão criar função, um postback será acontecerdo e a grade será atualizada para incluir administradores, bem como a nova função, supervisores.

[![a função de supervisores foi adicionada](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**Figura 6**: a função de supervisores foi adicionada ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image18.png))

## <a name="step-6-deleting-roles"></a>Etapa 6: excluindo funções

Neste ponto, um usuário pode criar uma nova função e exibir todas as funções existentes na página `ManageRoles.aspx`. Vamos permitir que os usuários também excluam funções. O método `Roles.DeleteRole` tem duas sobrecargas:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) -exclui a função *roleName*. Uma exceção será gerada se a função contiver um ou mais membros.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) -exclui a função *roleName*. Se *throwOnPopulateRole* for `true`, uma exceção será gerada se a função contiver um ou mais membros. Se *throwOnPopulateRole* for `false`, a função será excluída se ela contiver Membros ou não. Internamente, o método `DeleteRole(roleName)` chama `DeleteRole(roleName, true)`.

O método `DeleteRole` também gerará uma exceção se *roleName* for `null` ou uma cadeia de caracteres vazia ou se *roleName* contiver uma vírgula. Se *roleName* não existir no sistema, `DeleteRole` falha silenciosamente, sem gerar uma exceção.

Vamos aumentar o GridView em `ManageRoles.aspx` para incluir um botão de exclusão que, quando clicado, exclui a função selecionada. Comece adicionando um botão excluir ao GridView acessando a caixa de diálogo campos e adicionando um botão excluir, que está localizado na opção CommandField. Faça com que o botão de exclusão seja a coluna à esquerda e defina sua propriedade `DeleteText` para excluir a função.

[![adicionar um botão excluir à Rolelist GridView](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**Figura 7**: adicionar um botão excluir ao `RoleList` GridView ([clique para exibir a imagem em tamanho normal](creating-and-managing-roles-cs/_static/image21.png))

Depois de adicionar o botão excluir, a marcação declarativa de GridView deve ser semelhante ao seguinte:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

Em seguida, crie um manipulador de eventos para o evento `RowDeleting` do GridView. Esse é o evento que é gerado no postback quando o botão excluir função é clicado. Adicione o seguinte código ao manipulador de eventos.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

O código começa por meio de programação fazendo referência ao controle da Web `RoleNameLabel` na linha cujo botão excluir função foi clicado. O método `Roles.DeleteRole` é invocado, passando o `Text` do `RoleNameLabel` e `false`, excluindo, assim, a função, independentemente de haver algum usuário associado à função. Por fim, o `RoleList` GridView é atualizado para que a função recém-excluída não apareça mais na grade.

> [!NOTE]
> O botão excluir função não requer nenhum tipo de confirmação do usuário antes de excluir a função. Uma das maneiras mais fáceis de confirmar uma ação é por meio de uma caixa de diálogo de confirmação do lado do cliente. Para obter mais informações sobre essa técnica, consulte [adicionando confirmação do lado do cliente ao excluir](https://asp.net/learn/data-access/tutorial-42-cs.aspx).

## <a name="summary"></a>Resumo

Muitos aplicativos Web têm determinadas regras de autorização ou funcionalidade de nível de página que só está disponível para determinadas classes de usuários. Por exemplo, pode haver um conjunto de páginas da Web que somente administradores podem acessar. Em vez de definir essas regras de autorização de usuário por usuário, muitas vezes é mais útil definir as regras com base em uma função. Ou seja, em vez de permitir explicitamente que os usuários Scott e Jisun acessem as páginas da Web administrativas, uma abordagem mais passível de manutenção é permitir que os membros da função administradores acessem essas páginas e, em seguida, denotarmos Scott e Jisun como usuários pertencentes ao Função de administradores.

A estrutura de funções facilita a criação e o gerenciamento de funções. Neste tutorial, examinamos como configurar a estrutura de funções para usar o `SqlRoleProvider`, que usa um banco de dados Microsoft SQL Server como o armazenamento de função. Também criamos uma página da Web que lista as funções existentes no sistema e permite que novas funções sejam criadas e as existentes sejam excluídas. Nos tutoriais subsequentes, veremos como atribuir usuários a funções e como aplicar a autorização baseada em funções.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Examinando a associação, as funções e o perfil do ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Como: usar o Gerenciador de funções no ASP.NET 2,0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Provedores de função](https://msdn.microsoft.com/library/aa478950.aspx)
- [Distribuindo sua própria ferramenta de administração de sites](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Documentação técnica para o elemento `<roleManager>`](https://msdn.microsoft.com/library/ms164660.aspx)
- [Usando as APIs de associação e Gerenciador de funções](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP. NET e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a *[Sams ensina a ASP.NET 2,0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott pode ser contatado em [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores potenciais para este tutorial incluem Alicja Maziarz, como Banerjee e Teresa Murphy. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Próximo](assigning-roles-to-users-cs.md)
