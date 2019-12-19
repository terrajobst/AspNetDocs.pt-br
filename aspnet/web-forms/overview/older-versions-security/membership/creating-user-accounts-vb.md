---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
title: Criando contas de usuário (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, exploraremos o uso da estrutura de associação (por meio do SqlMembershipProvider) para criar novas contas de usuário. Veremos como criar novos nós...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 9ef3e893-bebe-4b13-9fe5-8b71720dd85e
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: 01be198c329f372ddcd529ad8a369f2d3426a9fc
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74628082"
---
# <a name="creating-user-accounts-vb"></a>Criação de contas de usuário (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar código](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_VB.zip) ou [baixar PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_vb.pdf)

> Neste tutorial, exploraremos o uso da estrutura de associação (por meio do SqlMembershipProvider) para criar novas contas de usuário. Veremos como criar novos usuários programaticamente e por meio do ASP. Controle CreateUserWizard interno da rede.

## <a name="introduction"></a>Introdução

<a id="_msoanchor_1"> </a>No [tutorial anterior](creating-the-membership-schema-in-sql-server-vb.md) , instalamos o esquema de serviços de aplicativos em um banco de dados, que adicionou as tabelas, exibições e procedimentos armazenados necessários para o `SqlMembershipProvider` e `SqlRoleProvider`. Isso criou a infraestrutura que precisaremos para o restante dos tutoriais desta série. Neste tutorial, exploraremos o uso da estrutura de associação (por meio do `SqlMembershipProvider`) para criar novas contas de usuário. Veremos como criar novos usuários programaticamente e por meio do ASP. Controle CreateUserWizard interno da rede.

Além de aprender como criar novas contas de usuário, também precisaremos atualizar o site de demonstração que criamos primeiro na *<a id="_msoanchor_2"></a>[visão geral do tutorial de autenticação de formulários](../introduction/an-overview-of-forms-authentication-vb.md)* e, em seguida, aprimorado no tutorial *<a id="_msoanchor_3"></a>[configuração de autenticação de formulários e tópicos avançados](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* . Nosso aplicativo Web de demonstração tem uma página de logon que valida as credenciais dos usuários em relação a pares de nome de usuário/senha embutidos em código. Além disso, `Global.asax` inclui um código que cria `IPrincipal` personalizados e objetos de `IIdentity` para usuários autenticados. Atualizaremos a página de logon para validar as credenciais dos usuários em relação à estrutura de associação e remover a lógica de identidade e entidade personalizada.

Vamos começar!

## <a name="the-forms-authentication-and-membership-checklist"></a>A lista de verificação de autenticação e Associação de formulários

Antes de começarmos a trabalhar com a estrutura de associação, vamos examinar as etapas importantes que levamos para chegar a esse ponto. Ao usar a estrutura de associação com o `SqlMembershipProvider` em um cenário de autenticação baseada em formulários, as etapas a seguir precisam ser executadas antes de implementar a funcionalidade de associação em seu aplicativo Web:

1. **Habilite a autenticação baseada em formulários.** Como discutimos em *<a id="_msoanchor_4"></a>[uma visão geral da autenticação de formulários](../introduction/an-overview-of-forms-authentication-vb.md)* , a autenticação de formulários é habilitada editando `Web.config` e definindo o atributo `<authentication>` do elemento `mode` como `Forms`. Com a autenticação de formulários habilitada, cada solicitação de entrada é examinada para um *tíquete de autenticação de formulários*, que, se presente, identifica o solicitante.
2. **Adicione o esquema de serviços de aplicativos ao banco de dados apropriado.** Ao usar o `SqlMembershipProvider` precisamos instalar o esquema de serviços de aplicativo em um banco de dados. Normalmente, esse esquema é adicionado ao mesmo banco de dados que contém o modelo de dado do aplicativo. O *<a id="_msoanchor_5"></a>[tutorial Criando o esquema de associação no SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* examinou o uso da ferramenta `aspnet_regsql.exe` para fazer isso.
3. **Personalize as configurações do aplicativo Web para fazer referência ao banco de dados da etapa 2.** O tutorial *criando o esquema de associação no SQL Server* mostrou duas maneiras de configurar o aplicativo Web para que o `SqlMembershipProvider` use o banco de dados selecionado na etapa 2: modificando o nome da cadeia de conexão `LocalSqlServer`; ou adicionando um novo provedor registrado à lista de provedores de estrutura de associação e personalizando esse novo provedor para usar o banco de dados da etapa 2.

Ao criar um aplicativo Web que usa a `SqlMembershipProvider` e a autenticação baseada em formulários, você precisará executar essas três etapas antes de usar a classe `Membership` ou os controles da Web de logon ASP.NET. Como já realizamos essas etapas nos tutoriais anteriores, estamos prontos para começar a usar a estrutura de associação!

## <a name="step-1-adding-new-aspnet-pages"></a>Etapa 1: Adicionando novas páginas ASP.NET

Neste tutorial e nos próximos três, examinaremos várias funções e recursos relacionados à associação. Precisaremos de uma série de páginas do ASP.NET para implementar os tópicos examinados em todos esses tutoriais. Vamos criar essas páginas e, em seguida, um arquivo de mapa do site `(Web.sitemap)`.

Comece criando uma nova pasta no projeto chamada `Membership`. Em seguida, adicione cinco novas páginas ASP.NET à pasta `Membership`, vinculando cada página com a `Site.master` página mestra. Nomeie as páginas:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

Neste ponto, o Gerenciador de Soluções do seu projeto deve ser semelhante à captura de tela mostrada na Figura 1.

[![cinco novas páginas foram adicionadas à pasta de associação](creating-user-accounts-vb/_static/image2.png)](creating-user-accounts-vb/_static/image1.png)

**Figura 1**: Cinco novas páginas foram adicionadas à pasta `Membership` ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image3.png))

Cada página deve, neste ponto, ter os dois controles de conteúdo, um para cada um dos ContentPlaceHolders da página mestra: `MainContent` e `LoginContent`.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample1.aspx)]

Lembre-se de que a marcação padrão do `LoginContent` ContentPlaceHolder exibe um link para fazer logon ou logoff do site, dependendo se o usuário está autenticado. No entanto, a presença do controle de conteúdo de `Content2` substitui a marcação padrão da página mestra. Como discutimos em *<a id="_msoanchor_6"></a>[uma visão geral do tutorial de autenticação de formulários](../introduction/an-overview-of-forms-authentication-vb.md)* , isso é útil em páginas em que não queremos exibir opções relacionadas ao logon na coluna à esquerda.

Para essas cinco páginas, no entanto, queremos mostrar a marcação padrão da página mestra para o `LoginContent` ContentPlaceHolder. Portanto, remova a marcação declarativa para o controle de conteúdo `Content2`. Depois de fazer isso, cada uma das cinco marcas da página deve conter apenas um controle de conteúdo.

## <a name="step-2-creating-the-site-map"></a>Etapa 2: Criando o mapa do site

Todos, exceto os sites mais triviais, precisam implementar alguma forma de uma interface de usuário de navegação. A interface do usuário de navegação pode ser uma lista simples de links para as várias seções do site. Como alternativa, esses links podem ser organizados em menus ou exibições de árvore. Como os desenvolvedores de páginas, a criação da interface do usuário de navegação é apenas metade da história. Também precisamos de alguns meios para definir a estrutura lógica do site de maneira passível de manutenção e atualizável. À medida que novas páginas são adicionadas ou as páginas existentes são removidas, queremos poder atualizar uma única fonte – o mapa do site-e fazer com que essas modificações sejam refletidas na interface do usuário de navegação do site.

Essas duas tarefas – definir o mapa do site e implementar uma interface do usuário de navegação baseada no mapa do site – são fáceis de realizar graças à estrutura do mapa do site e aos controles da Web de navegação adicionados no ASP.NET versão 2,0. A estrutura do mapa do site permite que um desenvolvedor defina um mapa do site e, em seguida, acesse-o por meio de uma API programática (a [classe`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). Os controles da Web de navegação internos incluem um [controle de menu](https://msdn.microsoft.com/library/bz09dy46.aspx), o [controle TreeView](https://msdn.microsoft.com/library/3eafky27.aspx)e o [controle SiteMapPath](https://msdn.microsoft.com/library/3eafky27.aspx).

Assim como as estruturas de associação e de funções, a estrutura do mapa do site é criada sobre o [modelo do provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). O trabalho da classe do provedor do mapa do site é gerar a estrutura na memória usada pela classe `SiteMap` de um armazenamento de dados persistente, como um arquivo XML ou uma tabela de banco de dado. O .NET Framework é fornecido com um provedor de mapa do site padrão que lê os dados do mapa do site de um arquivo XML ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)), e esse é o provedor que usaremos neste tutorial. Para algumas implementações alternativas do provedor do mapa do site, consulte a seção leituras adicionais no final deste tutorial.

O provedor de mapa do site padrão espera um arquivo XML formatado corretamente chamado `Web.sitemap` para existir no diretório raiz. Como estamos usando esse provedor padrão, precisamos adicionar esse arquivo e definir a estrutura do mapa do site no formato XML apropriado. Para adicionar o arquivo, clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções e escolha Adicionar novo item. Na caixa de diálogo, opte por adicionar um arquivo do tipo site map chamado `Web.sitemap`.

[![adicionar um arquivo chamado Web. sitemap ao diretório raiz do projeto](creating-user-accounts-vb/_static/image5.png)](creating-user-accounts-vb/_static/image4.png)

**Figura 2**: Adicione um arquivo chamado `Web.sitemap` ao diretório raiz do projeto ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image6.png))

O arquivo do mapa do site XML define a estrutura do site como uma hierarquia. Essa relação hierárquica é modelada no arquivo XML por meio do ancestrais dos elementos de `<siteMapNode>`. O `Web.sitemap` deve começar com um nó pai de `<siteMap>` que tenha exatamente um `<siteMapNode>` filho. Esse elemento de `<siteMapNode>` de nível superior representa a raiz da hierarquia e pode ter um número arbitrário de nós descendentes. Cada elemento de `<siteMapNode>` deve incluir um atributo `title` e pode, opcionalmente, incluir `url` e `description` atributos, entre outros; cada atributo de `url` não vazio deve ser exclusivo.

Insira o seguinte XML no arquivo de `Web.sitemap`:

[!code-xml[Main](creating-user-accounts-vb/samples/sample2.xml)]

A marcação do mapa do site acima define a hierarquia mostrada na Figura 3.

[![o mapa do site representa uma estrutura de navegação hierárquica](creating-user-accounts-vb/_static/image8.png)](creating-user-accounts-vb/_static/image7.png)

**Figura 3**: O mapa do site representa uma estrutura de navegação hierárquica ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image9.png))

## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Etapa 3: Atualizando a página mestra para incluir uma interface do usuário de navegação

O ASP.NET inclui vários controles da Web relacionados à navegação para a criação de uma interface do usuário. Isso inclui os controles menu, TreeView e SiteMapPath. O menu e os controles TreeView renderizam a estrutura do mapa do site em um menu ou em uma árvore, respectivamente, enquanto o SiteMapPath exibe um breadcrumb que mostra o nó atual que está sendo visitado, bem como seus ancestrais. Os dados do mapa do site podem ser associados a outros controles da Web de dados usando SiteMapDataSource e podem ser acessados programaticamente por meio da classe `SiteMap`.

Como uma discussão completa sobre a estrutura do mapa do site e os controles de navegação está além do escopo desta série de tutoriais, em vez de gastar tempo criando nossa própria interface do usuário de navegação, vamos, em vez disso, usar um controle Repeater para exibir uma lista de links de navegação *[2,0 em](../../data-access/index.md)* duas profundidade, como mostra a Figura 4.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Adicionando uma lista de dois níveis de links na coluna à esquerda

Para criar essa interface, adicione a seguinte marcação declarativa à coluna esquerda da `Site.master` página mestra em que o texto TODO: O menu vai aqui... atualmente reside.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample3.aspx)]

A marcação acima associa um controle Repeater chamado `menu` a SiteMapDataSource, que retorna a hierarquia do mapa do site definida em `Web.sitemap`. Como a [propriedade`ShowStartingNode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) do controle SiteMapDataSource é definida como false, ela começa a retornar a hierarquia do mapa do site, começando com os descendentes do nó inicial. O Repeater exibe cada um desses nós (atualmente apenas associação) em um elemento `<li>`. Outro, o repetidor interno, em seguida, exibe os filhos do nó atual em uma lista não ordenada aninhada.

A Figura 4 mostra a saída renderizada da marcação acima com a estrutura do mapa do site que criamos na etapa 2. O Repeater renderiza marcação de lista não ordenada da baunilha; as regras de folha de estilos em cascata definidas na `Styles.css` são responsáveis pelo layout de estética. Para obter uma descrição mais detalhada de como a marcação acima funciona, consulte as [páginas mestras e](https://asp.net/learn/data-access/tutorial-03-vb.aspx) o tutorial de navegação do site.

[![a interface do usuário de navegação é renderizada usando listas não ordenadas aninhadas](creating-user-accounts-vb/_static/image11.png)](creating-user-accounts-vb/_static/image10.png)

**Figura 4**: A interface do usuário de navegação é renderizada usando listas não ordenadas aninhadas ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image12.png))

### <a name="adding-breadcrumb-navigation"></a>Adicionando navegação de trilha

Além da lista de links na coluna à esquerda, também vamos fazer com que cada página exiba um [breadcrumb](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Um breadcrumb é um elemento de interface do usuário de navegação que mostra rapidamente aos usuários sua posição atual na hierarquia do site. O controle SiteMapPath usa a estrutura do mapa do site para determinar o local da página atual no mapa do site e, em seguida, exibe uma trilha com base nessas informações.

Especificamente, adicione um elemento `<span>` ao elemento de cabeçalho `<div>` da página mestra e defina o atributo `class` do novo elemento `<span>` como breadcrumb. (A classe `Styles.css` contém uma regra para uma classe de navegação estrutural.) Em seguida, adicione um SiteMapPath a esse novo elemento `<span>`.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample4.aspx)]

A Figura 5 mostra a saída do SiteMapPath ao visitar `~/Membership/CreatingUserAccounts.aspx`.

[![o breadcrumb exibe a página atual e seus ancestrais no mapa do site](creating-user-accounts-vb/_static/image14.png)](creating-user-accounts-vb/_static/image13.png)

**Figura 5**: O breadcrumb exibe a página atual e seus ancestrais no mapa do site ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image15.png))

## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Etapa 4: Removendo a entidade personalizada e a lógica de identidade

No tutorial *<a id="_msoanchor_7"></a>[configuração de autenticação de formulários e tópicos avançados](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* , vimos como associar objetos de identidade e entidade personalizados ao usuário autenticado. Fizemos isso criando um manipulador de eventos em `Global.asax` para o evento `PostAuthenticateRequest` do aplicativo, que é acionado após a `FormsAuthenticationModule` ter autenticado o usuário. Nesse manipulador de eventos, substituímos os objetos `GenericPrincipal` e `FormsIdentity` adicionados pelo `FormsAuthenticationModule` com os objetos `CustomPrincipal` e `CustomIdentity` que criamos nesse tutorial.

Embora objetos de identidade e entidade personalizados sejam úteis em determinados cenários, na maioria dos casos, os objetos `GenericPrincipal` e `FormsIdentity` são suficientes. Consequentemente, acho que vale a pena retornar ao comportamento padrão. Faça essa alteração removendo ou comentando o manipulador de eventos `PostAuthenticateRequest` ou excluindo o arquivo de `Global.asax` totalmente.

> [!NOTE]
> Depois de ter comentado ou removido o código em `Global.asax`, você precisará comentar o código na classe code-behind `Default.aspx's` que converte a propriedade `User.Identity` em uma instância de `CustomIdentity`.

## <a name="step-5-programmatically-creating-a-new-user"></a>Etapa 5: Criando programaticamente um novo usuário

Para criar uma nova conta de usuário por meio da estrutura de associação, use o [método`CreateUser`](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx)da classe `Membership`. Esse método tem parâmetros de entrada para o nome de usuário, a senha e outros campos relacionados a usuários. Na invocação, ele delega a criação da nova conta de usuário para o provedor de associação configurado e, em seguida, retorna um [objeto de`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) que representa a conta de usuário recém criada.

O método `CreateUser` tem quatro sobrecargas, cada uma aceitando um número diferente de parâmetros de entrada:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Essas quatro sobrecargas diferem na quantidade de informações coletadas. A primeira sobrecarga, por exemplo, requer apenas o nome de usuário e a senha para a nova conta de usuários, enquanto a segunda também requer o endereço de email do usuário.

Essas sobrecargas existem porque as informações necessárias para criar uma nova conta de usuário dependem das definições de configuração do provedor de associação. No tutorial *<a id="_msoanchor_8"></a>[criando o esquema de associação no SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* examinamos a especificação das definições de configuração do provedor de associação no `Web.config`. A tabela 2 incluía uma lista completa dos parâmetros de configuração.

Uma dessas definições de configuração do provedor de associação que afeta o que `CreateUser` sobrecargas podem ser usadas é a configuração de `requiresQuestionAndAnswer`. Se `requiresQuestionAndAnswer` for definido como `true` (o padrão), ao criar uma nova conta de usuário, será necessário especificar uma pergunta de segurança e uma resposta. Essas informações serão usadas posteriormente se o usuário precisar redefinir ou alterar sua senha. Especificamente, nesse momento eles são mostrados a pergunta de segurança e devem inserir a resposta correta para redefinir ou alterar sua senha. Consequentemente, se a `requiresQuestionAndAnswer` for definida como `true` chamando uma das duas primeiras `CreateUser` sobrecargas resultará em uma exceção porque a pergunta e a resposta de segurança estão ausentes. Como nosso aplicativo está atualmente configurado para exigir uma pergunta e resposta de segurança, precisaremos usar uma das duas últimas sobrecargas ao criar o usuário de forma programática.

Para ilustrar o uso do método `CreateUser`, vamos criar uma interface do usuário onde solicitamos ao usuário seu nome, senha, email e uma resposta a uma pergunta de segurança predefinida. Abra a página `CreatingUserAccounts.aspx` na pasta `Membership` e adicione os seguintes controles da Web ao controle de conteúdo:

- Uma caixa de texto chamada `Username`
- Uma caixa de texto chamada `Password`, cuja propriedade `TextMode` está definida como `Password`
- Uma caixa de texto chamada `Email`
- Um rótulo chamado `SecurityQuestion` com sua propriedade `Text` desmarcada
- Uma caixa de texto chamada `SecurityAnswer`
- Um botão chamado `CreateAccountButton` cuja propriedade `Text` está definida para criar a conta de usuário
- Um controle de rótulo chamado `CreateAccountResults` com sua propriedade `Text` desmarcada

Neste ponto, sua tela deve ser semelhante à captura de tela mostrada na Figura 6.

[![adicionar vários controles da Web à página CreatingUserAccounts. aspx](creating-user-accounts-vb/_static/image17.png)](creating-user-accounts-vb/_static/image16.png)

**Figura 6**: Adicione os vários controles da Web ao `CreatingUserAccounts.aspx Page` ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image18.png))

O rótulo de `SecurityQuestion` e a caixa de texto `SecurityAnswer` devem exibir uma pergunta de segurança predefinida e coletar a resposta do usuário. Observe que tanto a pergunta de segurança quanto a resposta são armazenadas em uma base de usuário por usuário, portanto, é possível permitir que cada usuário defina sua própria pergunta de segurança. No entanto, para este exemplo, decidi usar uma pergunta de segurança universal, a saber: Qual é sua cor favorita?

Para implementar essa pergunta de segurança predefinida, adicione uma constante à classe code-behind da página chamada `passwordQuestion`, atribuindo a ela a pergunta de segurança. Em seguida, no manipulador de eventos `Page_Load`, atribua essa constante à propriedade `Text` do rótulo de `SecurityQuestion`:

[!code-vb[Main](creating-user-accounts-vb/samples/sample5.vb)]

Em seguida, crie um manipulador de eventos para o evento `CreateAccountButton'` s `Click` e adicione o seguinte código:

[!code-vb[Main](creating-user-accounts-vb/samples/sample6.vb)]

O manipulador de eventos `Click` começa definindo uma variável chamada `createStatus` do tipo [`MembershipCreateStatus`](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus` é uma enumeração que indica o status da operação de `CreateUser`. Por exemplo, se a conta de usuário for criada com êxito, a instância de `MembershipCreateStatus` resultante será definida com um valor de `Success;`, por outro lado, se a operação falhar porque já existe um usuário com o mesmo nome, ele será definido como um valor de `DuplicateUserName`. Na sobrecarga de `CreateUser` que usamos, precisamos passar uma instância de `MembershipCreateStatus` para o método. Esse parâmetro é definido como o valor apropriado dentro do método `CreateUser`, e podemos examinar seu valor após a chamada de método para determinar se a conta de usuário foi criada com êxito.

Depois de chamar `CreateUser`, passando `createStatus`, uma instrução `Select Case` é usada para gerar uma mensagem apropriada, dependendo do valor atribuído a `createStatus`. As figuras 7 mostram a saída quando um novo usuário foi criado com êxito. As figuras 8 e 9 mostram a saída quando a conta de usuário não é criada. Na Figura 8, o visitante inseriu uma senha de cinco letras, que não atende aos requisitos de força de senha descritos nas definições de configuração do provedor de associação. Na Figura 9, o visitante está tentando criar uma conta de usuário com um username existente (aquele criado na Figura 7).

[![uma nova conta de usuário foi criada com êxito](creating-user-accounts-vb/_static/image20.png)](creating-user-accounts-vb/_static/image19.png)

**Figura 7**: Uma nova conta de usuário foi criada com êxito ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image21.png))

[![a conta de usuário não for criada porque a senha fornecida é muito fraca](creating-user-accounts-vb/_static/image23.png)](creating-user-accounts-vb/_static/image22.png)

**Figura 8**: A conta de usuário não foi criada porque a senha fornecida é muito fraca ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image24.png))

[![a conta de usuário não for criada porque o nome de usuário já está em uso](creating-user-accounts-vb/_static/image26.png)](creating-user-accounts-vb/_static/image25.png)

**Figura 9**: A conta de usuário não foi criada porque o nome de usuários já está em uso ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image27.png))

> [!NOTE]
> Você pode estar se perguntando como determinar o êxito ou a falha ao usar um dos dois primeiros `CreateUser` sobrecargas de método, nenhum dos quais tem um parâmetro do tipo `MembershipCreateStatus`. Essas duas primeiras sobrecargas lançam uma [`MembershipCreateUserException` exceção](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) em face de uma falha, que inclui uma [Propriedade`StatusCode`](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) do tipo `MembershipCreateStatus`.

Depois de criar algumas contas de usuário, verifique se as contas foram criadas, listando o conteúdo das tabelas `aspnet_Users` e `aspnet_Membership` no banco de dados `SecurityTutorials.mdf`. Como mostra a Figura 10, adicionei dois usuários por meio da página `CreatingUserAccounts.aspx`: Tito e Bruce.

[![há dois usuários no repositório de usuários da associação: Tito e Bruce](creating-user-accounts-vb/_static/image29.png)](creating-user-accounts-vb/_static/image28.png)

**Figura 10**: Há dois usuários no repositório de usuários da associação: Tito e Bruce ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image30.png))

Embora o armazenamento do usuário da Associação agora inclua as informações da conta Bruce e Tito, ainda implementamos a funcionalidade que permite que Bruce ou Tito façam logon no site. Atualmente, `Login.aspx` valida as credenciais do usuário em relação a um conjunto embutido de pares de nome de usuário/senha-ele *não valida as* credenciais fornecidas em relação à estrutura de associação. Por ora, ver as novas contas de usuário nas tabelas `aspnet_Users` e `aspnet_Membership` precisará ser suficiente. No próximo tutorial, *<a id="_msoanchor_9"></a>[validando as credenciais do usuário em relação ao armazenamento do usuário da Associação](validating-user-credentials-against-the-membership-user-store-vb.md)* , atualizaremos a página de logon para validar em relação ao repositório de associação.

> [!NOTE]
> Se você não vir nenhum usuário em seu `SecurityTutorials.mdf` banco de dados, pode ser porque seu aplicativo Web está usando o provedor de associação padrão, `AspNetSqlMembershipProvider`, que usa o banco de dados `ASPNETDB.mdf` como seu armazenamento de usuário. Para determinar se esse é o problema, clique no botão atualizar na Gerenciador de Soluções. Se um banco de dados chamado `ASPNETDB.mdf` tiver sido adicionado à pasta `App_Data`, esse será o problema. Retorne à etapa 4 do tutorial *<a id="_msoanchor_10"></a>[criando o esquema de associação no SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* para obter instruções sobre como configurar corretamente o provedor de associação.

Na maioria dos cenários de criação de conta de usuário, o visitante recebe alguma interface para inserir seu nome de usuário, senha, email e outras informações essenciais. nesse ponto, uma nova conta é criada. Nesta etapa, examinamos a criação dessa interface manualmente e vimos como usar o método `Membership.CreateUser` para adicionar programaticamente a nova conta de usuário com base nas entradas do usuário. Nosso código, no entanto, acabou de criar a nova conta de usuário. Ele não executou nenhuma ação de acompanhamento, como fazer logon do usuário no site sob a conta de usuário recém-criada ou enviar um email de confirmação para o usuário. Essas etapas adicionais exigirão código adicional no manipulador de eventos de `Click` do botão.

O ASP.NET é fornecido com o controle CreateUserWizard, que é projetado para lidar com o processo de criação da conta do usuário, desde o processamento de uma interface do usuário para a criação de novas contas de usuário até a criação da conta na estrutura de associação e a execução de pós-instalação tarefas de criação, como enviar um email de confirmação e registrar em log o usuário recém-criado no site. O uso do controle CreateUserWizard é tão simples quanto arrastar o controle CreateUserWizard da caixa de ferramentas para uma página e, em seguida, definir algumas propriedades. Na maioria dos casos, você não precisará escrever uma única linha de código. Exploraremos esse controle inteligente em detalhes na etapa 6.

Se novas contas de usuário forem criadas apenas por meio de uma página da Web de criação de conta típica, é improvável que você precise escrever código que usa o método `CreateUser`, pois o controle CreateUserWizard provavelmente atenderá às suas necessidades. No entanto, o método `CreateUser` é útil em cenários em que você precisa de uma experiência de usuário de criação de conta altamente personalizada ou quando você precisa criar programaticamente novas contas de usuário por meio de uma interface alternativa. Por exemplo, você pode ter uma página que permite que um usuário carregue um arquivo XML que contém informações do usuário de algum outro aplicativo. A página pode analisar o conteúdo do arquivo XML carregado e criar uma nova conta para cada usuário representado no XML chamando o método `CreateUser`.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Etapa 6: Criando um novo usuário com o controle CreateUserWizard

O ASP.NET é fornecido com vários controles da Web de logon. Esses controles auxiliam em muitos cenários comuns relacionados à conta de usuário e ao logon. O [controle CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) é um desses controles que é projetado para apresentar uma interface do usuário para adicionar uma nova conta de usuário à estrutura de associação.

Como muitos dos outros controles da Web relacionados ao logon, o CreateUserWizard pode ser usado sem escrever uma única linha de código. Ele fornece intuitivamente uma interface do usuário com base nas definições de configuração do provedor de associação e chama internamente o método `CreateUser` da classe `Membership` depois que o usuário insere as informações necessárias e clica no botão criar usuário. O controle CreateUserWizard é extremamente personalizável. Há um host de eventos que são acionados durante vários estágios do processo de criação da conta. Podemos criar manipuladores de eventos, conforme necessário, para injetar lógica personalizada no fluxo de trabalho de criação de conta. Além disso, a aparência do CreateUserWizard é muito flexível. Há várias propriedades que definem a aparência da interface padrão; se necessário, o controle pode ser convertido em um modelo ou etapas de registro de usuário adicionais podem ser adicionadas.

Vamos começar com uma olhada no uso da interface e do comportamento padrão do controle CreateUserWizard. Em seguida, exploraremos como personalizar a aparência por meio de propriedades e eventos do controle.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Examinando a interface padrão e o comportamento do CreateUserWizard

Retorne à página `CreatingUserAccounts.aspx` na pasta `Membership`, alterne para o modo de design ou divisão e, em seguida, adicione um controle CreateUserWizard à parte superior da página. O controle CreateUserWizard é arquivado na seção controles de logon da caixa de ferramentas. Depois de adicionar o controle, defina sua propriedade `ID` como `RegisterUser`. Como mostra a captura de tela na Figura 11, o CreateUserWizard renderiza uma interface com caixas de correio para o nome de usuário, a senha, o endereço de email e a pergunta e a resposta de segurança.

[![o controle CreateUserWizard renderiza uma interface de usuário de criação genérica](creating-user-accounts-vb/_static/image32.png)](creating-user-accounts-vb/_static/image31.png)

**Figura 11**: O controle CreateUserWizard renderiza uma interface de usuário de criação genérica ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image33.png))

Vamos reservar um momento para comparar a interface do usuário padrão gerada pelo controle CreateUserWizard com a interface que criamos na etapa 5. Para iniciantes, o controle CreateUserWizard permite que o visitante especifique a pergunta e a resposta de segurança, enquanto nossa interface criada manualmente usou uma pergunta de segurança predefinida. A interface do controle CreateUserWizard também inclui controles de validação, enquanto ainda tínhamos de implementar a validação nos campos de formulário de nossa interface. E a interface de controle CreateUserWizard inclui uma caixa de texto de confirmação de senha (junto com um CompareValidator para garantir que o texto digitado a senha e as caixas de textos de comparação de senha sejam iguais).

O interessante é que o controle CreateUserWizard consulte as definições de configuração do provedor de associação ao renderizar sua interface do usuário. Por exemplo, as caixas de textpergunta de segurança e resposta só serão exibidas se `requiresQuestionAndAnswer` for definido como true. Da mesma forma, o CreateUserWizard adiciona automaticamente um controle RegularExpressionValidator para garantir que os requisitos de força de senha sejam atendidos e define suas propriedades `ErrorMessage` e `ValidationExpression` com base nas definições de configuração `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`e `passwordStrengthRegularExpression`.

O controle CreateUserWizard, como o nome indica, é derivado do [controle Wizard](https://msdn.microsoft.com/library/s2etd1ek.aspx). Os controles do assistente são projetados para fornecer uma interface para concluir tarefas de várias etapas. Um controle Wizard pode ter um número arbitrário de `WizardSteps`, cada um dos quais é um modelo que define o HTML e os controles da Web para essa etapa. O controle Wizard inicialmente exibe a primeira `WizardStep`, juntamente com os controles de navegação que permitem que o usuário prossiga de uma etapa para outra ou para retornar às etapas anteriores.

Como a marcação declarativa na Figura 11 mostra, a interface padrão do controle CreateUserWizard inclui duas `WizardStep` s:

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) ? Renderiza a interface para coletar informações para criar a nova conta de usuário. Esta é a etapa mostrada na Figura 11.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) ? renderiza uma mensagem indicando que a conta foi criada com êxito.

A aparência e o comportamento do CreateUserWizard podem ser modificados convertendo qualquer uma dessas etapas em modelos ou adicionando seus próprios `WizardStep` s. Veremos como adicionar um `WizardStep` à interface de registro no tutorial *armazenamento de informações adicionais do usuário* .

Vamos ver o controle CreateUserWizard em ação. Visite a página `CreatingUserAccounts.aspx` por meio de um navegador. Comece inserindo alguns valores inválidos na interface do CreateUserWizard. Tente inserir uma senha que não esteja de acordo com os requisitos de força de senha ou deixando a caixa de texto nome de usuário vazia. O CreateUserWizard exibirá uma mensagem de erro apropriada. A Figura 12 mostra a saída ao tentar criar um usuário com uma senha forte insuficiente.

[![o CreateUserWizard injeta automaticamente os controles de validação](creating-user-accounts-vb/_static/image35.png)](creating-user-accounts-vb/_static/image34.png)

**Figura 12**: O CreateUserWizard injeta automaticamente controles de validação ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image36.png))

Em seguida, insira os valores apropriados no CreateUserWizard e clique no botão criar usuário. Supondo que os campos obrigatórios tenham sido inseridos e a força da senha seja suficiente, o CreateUserWizard criará uma nova conta de usuário por meio da estrutura de associação e, em seguida, exibirá a interface do `CompleteWizardStep`(consulte a Figura 13). Nos bastidores, o CreateUserWizard chama o método `Membership.CreateUser`, exatamente como fizemos na etapa 5.

[![uma nova conta de usuário foi criada com êxito](creating-user-accounts-vb/_static/image38.png)](creating-user-accounts-vb/_static/image37.png)

**Figura 13**: Uma nova conta de usuário foi criada com êxito ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image39.png))

> [!NOTE]
> Como mostra a Figura 13, a interface do `CompleteWizardStep`inclui um botão continuar. No entanto, nesse ponto, clicar nele apenas executa um postback, deixando o visitante na mesma página. Na personalização da aparência e do comportamento do CreateUserWizard por meio de sua seção Propriedades, veremos como você pode fazer com que esse botão envie o visitante para `Default.aspx` (ou alguma outra página).

Depois de criar uma nova conta de usuário, retorne ao Visual Studio e examine as `aspnet_Users` e `aspnet_Membership` tabelas como fizemos na Figura 10 para verificar se a conta foi criada com êxito.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Personalizando o comportamento e a aparência do CreateUserWizard por meio de suas propriedades

O CreateUserWizard pode ser personalizado de várias maneiras, por meio de propriedades, `WizardStep` s e manipuladores de eventos. Nesta seção, veremos como personalizar a aparência do controle por meio de suas propriedades; a próxima seção analisa a extensão do comportamento do controle por meio de manipuladores de eventos.

Praticamente todo o texto exibido na interface do usuário padrão do controle CreateUserWizard pode ser personalizado por meio de sua infinidade de propriedades. Por exemplo, os rótulos nome de usuário, senha, confirmar senha, email, pergunta de segurança e resposta de segurança exibidos à esquerda das caixas de correio podem ser personalizados pelas propriedades [`UserNameLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [`PasswordLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [`ConfirmPasswordLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [`EmailLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [`QuestionLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx)e [`AnswerLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx) , respectivamente. Da mesma forma, há propriedades para especificar o texto para os botões criar usuário e continuar na `CreateUserWizardStep` e `CompleteWizardStep`, bem como se esses botões são renderizados como botões, LinkButtons ou ImageButtons.

As cores, bordas, fontes e outros elementos visuais são configuráveis por meio de um host de propriedades de estilo. O controle CreateUserWizard em si tem as propriedades comuns de estilo de controle da Web-`BackColor`, `BorderStyle`, `CssClass`, `Font`e assim por diante – e há várias propriedades de estilo para definir a aparência de seções específicas da interface do CreateUserWizard. A [propriedade`TextBoxStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), por exemplo, define os estilos para as caixas de textno `CreateUserWizardStep`, enquanto a [Propriedade`TitleTextStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) define o estilo do título (Inscreva-se para sua nova conta).

Além das propriedades relacionadas à aparência, há várias propriedades que afetam o comportamento do controle CreateUserWizard. A [propriedade`DisplayCancelButton`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), se definida como true, exibe um botão Cancelar ao lado do botão criar usuário (o valor padrão é false). Se você exibir o botão Cancelar, certifique-se também de definir a [propriedade`CancelDestinationPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), que especifica a página para a qual o usuário será enviado depois de clicar em cancelar. Conforme observado na seção anterior, o botão continuar na interface do `CompleteWizardStep`causa um postback, mas deixa o visitante na mesma página. Para enviar o visitante para alguma outra página depois de clicar no botão continuar, basta especificar a URL na [propriedade`ContinueDestinationPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Vamos atualizar o controle CreateUserWizard `RegisterUser` para mostrar um botão Cancelar e enviar o visitante para `Default.aspx` quando os botões cancelar ou continuar forem clicados. Para fazer isso, defina a propriedade `DisplayCancelButton` como true e as propriedades `CancelDestinationPageUrl` e `ContinueDestinationPageUrl` como ~/default.aspx. A Figura 14 mostra o CreateUserWizard atualizado quando exibido por meio de um navegador.

[![o CreateUserWizardStep inclui um botão Cancelar](creating-user-accounts-vb/_static/image41.png)](creating-user-accounts-vb/_static/image40.png)

**Figura 14**: O `CreateUserWizardStep` inclui um botão de cancelamento ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image42.png))

Quando um visitante insere um nome de usuário, uma senha, um endereço de email e uma pergunta e resposta de segurança e clica em Create User, uma nova conta de usuário é criada e o visitante é conectado como aquele usuário recém-criado. Supondo que a pessoa que visita a página está criando uma nova conta para si mesma, esse é provavelmente o comportamento desejado. No entanto, talvez você queira permitir que os administradores adicionem novas contas de usuário. Ao fazer isso, a conta de usuário seria criada, mas o administrador permaneceria conectado como administrador (e não como a conta recém-criada). Esse comportamento pode ser modificado por meio da [Propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx)booliana`LoginCreatedUser`.

As contas de usuário na estrutura de associação contêm um sinalizador aprovado; os usuários que não estão aprovados não podem fazer logon no site. Por padrão, uma conta recém-criada é marcada como aprovada, permitindo que o usuário faça logon no site imediatamente. No entanto, é possível ter novas contas de usuário marcadas como não aprovadas. Talvez você queira que um administrador aprove manualmente os novos usuários antes que eles possam fazer logon; ou talvez você queira verificar se o endereço de email inserido na inscrição é válido antes de permitir que um usuário faça logon. Seja qual for o caso, você pode ter a conta de usuário recém-criada marcada como não aprovada definindo a [propriedade de`DisableCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) do controle CreateUserWizard como true (o padrão é false).

Outras propriedades relacionadas ao comportamento de observação incluem `AutoGeneratePassword` e `MailDefinition`. Se a [propriedade`AutoGeneratePassword`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) for definida como true, a `CreateUserWizardStep` não exibirá as caixas de entrada senha e confirmar senha; em vez disso, a senha do usuário recém criada é gerada automaticamente usando o [método`GeneratePassword`](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)da classe `Membership`. O método `GeneratePassword` constrói uma senha de um comprimento especificado e com um número suficiente de caracteres não alfanuméricos para atender aos requisitos de força de senha configurados.

A [propriedade`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) será útil se você quiser enviar um email para o endereço de email especificado durante o processo de criação de conta. A propriedade `MailDefinition` inclui uma série de subpropriedades para definir informações sobre a mensagem de email construída. Essas subpropriedades incluem opções como `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`e `BodyFileName`. A [propriedade`BodyFileName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) aponta para um arquivo de texto ou HTML que contém o corpo da mensagem de email. O corpo dá suporte a dois espaços reservados predefinidos: `<%UserName%>` e `<%Password%>`. Esses espaços reservados, se presentes no arquivo de `BodyFileName`, serão substituídos pelo nome e senha do usuário recém-criado.

> [!NOTE]
> A propriedade `MailDefinition` do controle de `CreateUserWizard` especifica apenas detalhes sobre a mensagem de email que é enviada quando uma nova conta é criada. Ele não inclui detalhes sobre como a mensagem de email é realmente enviada (ou seja, se um servidor SMTP ou diretório de recebimento de email é usado, todas as informações de autenticação e assim por diante). Esses detalhes de baixo nível precisam ser definidos na seção `<system.net>` em `Web.config`. Para obter mais informações sobre essas definições de configuração e sobre como enviar email do ASP.NET 2,0 em geral, consulte as [perguntas frequentes em SystemNetMail.com](http://www.systemnetmail.com/) e meu artigo, [enviando email no ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).

### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Estendendo o comportamento do CreateUserWizard usando manipuladores de eventos

O controle CreateUserWizard gera vários eventos durante seu fluxo de trabalho. Por exemplo, depois que um visitante inserir seu nome de usuário, senha e outras informações pertinentes e clicar no botão criar usuário, o controle CreateUserWizard gerará seu [evento de`CreatingUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Se houver um problema durante o processo de criação, o [evento de`CreateUserError`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) será acionado; no entanto, se o usuário for criado com êxito, o [evento`CreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) será gerado. Há eventos de controle CreateUserWizard adicionais que são gerados, mas esses são os três mais alemão.

Em determinados cenários, talvez queiramos explorar o fluxo de trabalho CreateUserWizard, que podemos fazer criando um manipulador de eventos para o evento apropriado. Para ilustrar isso, vamos aprimorar o controle CreateUserWizard de `RegisterUser` para incluir alguma validação personalizada no nome de usuário e senha. Em particular, vamos aprimorar nosso CreateUserWizard para que os nomes de usuário não possam conter espaços à esquerda ou à direita, e o nome de acessador não pode aparecer em nenhum lugar da senha. Em suma, queremos impedir que alguém crie um nome de usuário como "Scott" ou tenha uma combinação de nome de usuário/senha como Scott e Scott. 1234.

Para fazer isso, criaremos um manipulador de eventos para o evento `CreatingUser` para executar nossas verificações de validação extras. Se os dados fornecidos não forem válidos, será necessário cancelar o processo de criação. Também precisamos adicionar um controle rótulo da Web à página para exibir uma mensagem explicando que o nome de usuário ou a senha são inválidos. Comece adicionando um controle rótulo abaixo do controle CreateUserWizard, definindo sua propriedade `ID` como `InvalidUserNameOrPasswordMessage` e sua propriedade `ForeColor` como `Red`. Desmarque sua propriedade `Text` e defina suas propriedades `EnableViewState` e `Visible` como false.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample7.aspx)]

Em seguida, crie um manipulador de eventos para o evento de `CreatingUser` do controle CreateUserWizard. Para criar um manipulador de eventos, selecione o controle no designer e, em seguida, vá para a janela Propriedades. A partir daí, clique no ícone de raio e clique duas vezes no evento apropriado para criar o manipulador de eventos.

Adicione o seguinte código ao manipulador de eventos do `CreatingUser`:

[!code-vb[Main](creating-user-accounts-vb/samples/sample8.vb)]

Observe que o nome de usuário e a senha inseridos no controle CreateUserWizard estão disponíveis por meio de suas propriedades [`UserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) e [`Password`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx), respectivamente. Usamos essas propriedades no manipulador de eventos acima para determinar se o nome de usuário fornecido contém espaços à esquerda ou à direita e se o nome de usuário é encontrado dentro da senha. Se uma dessas condições for atendida, uma mensagem de erro será exibida no rótulo `InvalidUserNameOrPasswordMessage` e a propriedade `e.Cancel` do manipulador de eventos será definida como `True`. Se `e.Cancel` for definido como `True`, o CreateUserWizard de curto-Circuit seu fluxo de trabalho, cancelando efetivamente o processo de criação da conta do usuário.

A Figura 15 mostra uma captura de tela de `CreatingUserAccounts.aspx` quando o usuário insere um nome de usuário com espaços à esquerda.

[Não são permitidos ![nomes de acessados com espaços à esquerda ou à direita](creating-user-accounts-vb/_static/image44.png)](creating-user-accounts-vb/_static/image43.png)

**Figura 15**: Não são permitidos nomes de acessados com espaços à esquerda ou à direita ([clique para exibir a imagem em tamanho normal](creating-user-accounts-vb/_static/image45.png))

> [!NOTE]
> Veremos um exemplo de como usar o evento de `CreatedUser` do controle CreateUserWizard no tutorial *<a id="_msoanchor_11"></a>[armazenamento de informações adicionais do usuário](storing-additional-user-information-vb.md)* .

## <a name="summary"></a>Resumo

O método `CreateUser` da classe `Membership` cria uma nova conta de usuário na estrutura de associação. Ele faz isso delegando a chamada para o provedor de associação configurado. No caso do `SqlMembershipProvider`, o método `CreateUser` adiciona um registro às tabelas `aspnet_Users` e `aspnet_Membership` banco de dados.

Embora novas contas de usuário possam ser criadas programaticamente (como vimos na etapa 5), uma abordagem mais rápida e fácil é usar o controle CreateUserWizard. Esse controle renderiza uma interface do usuário de várias etapas para coletar informações do usuário e criar um novo usuário na estrutura de associação. Nos bastidores, esse controle usa o mesmo método `Membership.CreateUser`, conforme examinado na etapa 5, mas o controle cria a interface do usuário, controles de validação e responde a erros de criação de conta de usuário sem precisar escrever uma lique de código.

Neste ponto, temos a funcionalidade em vigor para criar novas contas de usuário. No entanto, a página de logon ainda está validando as credenciais embutidas em código que especificamos de volta no segundo tutorial. <a id="_msoanchor_12"> </a>No [próximo tutorial](validating-user-credentials-against-the-membership-user-store-vb.md) , atualizaremos `Login.aspx` para validar as credenciais fornecidas pelo usuário em relação à estrutura de associação.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos discutidos neste tutorial, consulte os seguintes recursos:

- [Documentação técnica do `CreateUser`](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [Visão geral do controle CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Criando um provedor de mapa do site baseado no sistema de arquivos](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Criando uma interface do usuário passo a passo com o controle do assistente ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Examinando a navegação do site do ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Páginas mestras e navegação do site](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [O provedor de mapa do site do SQL que você está esperando](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP. NET e fundador da 4GuysFromRolla.com, tem trabalhado com tecnologias Web da Microsoft desde 1998. Scott trabalha como consultor, instrutor e escritor independentes. Seu livro mais recente é que a *[Sams ensina a ASP.NET 2,0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott pode ser contatado em [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. O revisor de Lead para este tutorial foi Teresa Murphy. Está interessado em revisar meus artigos futuros do MSDN? Em caso afirmativo, solte-me uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Anterior](creating-the-membership-schema-in-sql-server-vb.md)
> [Próximo](validating-user-credentials-against-the-membership-user-store-vb.md)
