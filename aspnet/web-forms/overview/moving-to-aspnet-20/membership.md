---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Associação | Microsoft Docs
author: microsoft
description: A associação do ASP.NET baseia-se no sucesso do modelo de autenticação de formulários do ASP.NET 1. x. A autenticação de formulários do ASP.NET fornece uma maneira conveniente para a incorp...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: da6fc205bd852a818d65425586cec38fdb08d310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642152"
---
# <a name="membership"></a>Membership

pela [Microsoft](https://github.com/microsoft)

> A associação do ASP.NET baseia-se no sucesso do modelo de autenticação de formulários do ASP.NET 1. x. A autenticação de formulários do ASP.NET fornece uma maneira conveniente de incorporar um formulário de logon em seu aplicativo ASP.NET e validar os usuários em um banco de dados ou em outro armazenamento.

A associação do ASP.NET baseia-se no sucesso do modelo de autenticação de formulários do ASP.NET 1. x. A autenticação de formulários do ASP.NET fornece uma maneira conveniente de incorporar um formulário de logon em seu aplicativo ASP.NET e validar os usuários em um banco de dados ou em outro armazenamento. Os membros da classe FormsAuthentication possibilitam manipular cookies para autenticação, verificar se há um logon válido, fazer logoff de um usuário, etc. No entanto, implementar a autenticação de formulários em um aplicativo ASP.NET 1. x pode exigir uma quantidade razoável de código.

A associação no ASP.NET 2,0 é um grande avanço sobre o uso de autenticação de formulários sozinha. (A associação é mais robusta quando associada à autenticação de formulários, mas o uso da autenticação de formulários não é um requisito.) Como você verá em breve, você pode usar a associação do ASP.NET e os controles de logon no ASP.NET 2,0 para implementar um sistema de associação poderoso sem escrever muito código.

## <a name="implementing-membership-in-aspnet-20"></a>Implementando a associação no ASP.NET 2,0

A associação é implementada seguindo quatro etapas. Tenha em mente que há muitas subetapas envolvidas, bem como a configuração opcional que também pode ser implementada. Essas etapas destinam-se a ilustrar o panorama da configuração da associação.

1. Crie seu banco de dados de associação (se SQL Server for usado como o repositório de associação.)
2. Especifique as opções de associação nos arquivos de configuração de seus aplicativos. (A associação está habilitada por padrão.)
3. Determine o tipo de repositório de associação que você deseja usar. As opções são: 

    - Microsoft SQL Server (versão 7,0 ou posterior)
    - Active Directory Store
    - Provedor de associação personalizado
4. Configure o aplicativo para a autenticação de formulários do ASP.NET. Mais uma vez, a associação é projetada para tirar proveito da autenticação de formulários, mas usar a autenticação de formulários não é um requisito.
5. Defina contas de usuário para associação e configure funções, se desejado.

## <a name="creating-the-membership-database"></a>Criando o banco de dados de associação

Se você estiver usando o SQL Server 7,0 ou posterior como seu armazenamento de associação, poderá usar o utilitário ASPNET\_RegSql (disponível mais facilmente no prompt de comando do Visual Studio .NET 2005) para configurar seu banco de dados. O utilitário ASPNET\_RegSql pode ser usado como uma ferramenta de prompt de comando ou por meio de um assistente de GUI. O método do assistente é a maneira mais fácil de configurar seu banco de dados. Para acessar o assistente, basta executar o seguinte comando:

`aspnet_regsql W`

Depois de executar esse comando, você verá o assistente de instalação do ASP.NET SQL Server, como mostrado abaixo.

![](membership/_static/image1.jpg)

**Figura 1**

O assistente de instalação do ASP.NET SQL Server cria o site da Web na instância especificada no assistente. No entanto, o ASP.NET usará a cadeia de conexão no arquivo Machine. config para se conectar ao banco de dados. Por padrão, essa cadeia de conexão apontará para uma instância SQL Server 2005, portanto, se você estiver usando uma instância SQL Server 2000 ou SQL Server 7,0, será necessário modificar a cadeia de conexão no arquivo Machine. config. Essa cadeia de conexão pode estar localizada aqui:

[!code-xml[Main](membership/samples/sample1.xml)]

Infelizmente, se você não modificar a cadeia de conexão, o ASP.NET não apresentará um erro descritivo. Ele só continuará a reclamar dizendo que você não criou o banco de dados. No caso acima, modifiquei a cadeia de conexão para apontar para minha instância local do SQL Server 2000.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Especificando a configuração e adicionando usuários e funções

A próxima etapa na configuração da associação é adicionar as informações necessárias ao arquivo Web. config do aplicativo. No ASP.NET 1. x, às vezes, modificar o arquivo Web. config era difícil devido ao uso de lowerCamelCase e à falta do IntelliSense. O Visual Studio .NET 2005 torna a tarefa muito mais fácil com o IntelliSense para arquivos de configuração, mas ASP.NET 2,0 vai além, fornecendo uma interface da Web para editar arquivos de configuração.

Você pode iniciar a interface da Web clicando no botão configuração do ASP.NET na barra de ferramentas Gerenciador de Soluções, conforme mostrado abaixo. Você também pode iniciar a interface da Web por meio de pop-ups que são exibidos quando os controles de logon são inseridos.

![](membership/_static/image2.jpg)

**Figura 2**

Isso inicia a ferramenta de administração de site do ASP.NET mostrada abaixo. A administração de site da ASP.NET é uma interface de quatro guias que facilita o gerenciamento de configurações do aplicativo. As seguintes guias estão disponíveis:

- **Página Inicial**
- **Segurança** do Configure usuários, funções e acesso.
- Do **aplicativo** Definir configurações do aplicativo.
- **Provedor** de Configure e teste seu provedor de associação de aplicativos.

A ferramenta de administração de site permite que você crie novos usuários facilmente, crie novas funções e gerencie usuários e funções. Essa capacidade não está disponível na interface do Windows. A interface do Windows permite que você defina facilmente as configurações de autorização e adicione, exclua e gerencie provedores, recursos que não estão na ferramenta de administração de site.

Para iniciar a interface do Windows, abra o snap-in Serviços de Informações da Internet, clique com o botão direito do mouse no aplicativo e escolha Propriedades. Clique na guia ASP.NET e, em seguida, clique no botão Editar configuração. (O aplicativo deve estar em execução em ASP.NET 2,0 para que o botão Editar configuração seja habilitado. Você também pode configurar a versão ASP.NET na caixa de diálogo ASP.NET.) A caixa de diálogo Definições de configuração do ASP.NET é exibida, conforme mostrado abaixo.

![](membership/_static/image3.jpg)

**Figura 3**

Na guia geral, as cadeias de conexão e as configurações do aplicativo são listadas. As configurações em itálico são definidas em um arquivo de configuração pai (o Machine. config ou um Web. config em um nível superior) e as configurações que não estão em itálico são do arquivo de configuração de aplicativos. Se uma configuração for adicionada, removida ou editada no nível do aplicativo, o ASP.NET adicionará, removerá ou modificará a configuração nos níveis de aplicativo Web. config, em vez de remover a configuração do arquivo de configuração do qual ela é herdada.

A guia autenticação é mostrada abaixo. É aqui que você vai definir suas configurações de associação. As configurações de autenticação de formulários, provedores de associação e provedores de função podem ser configuradas aqui.

![](membership/_static/image4.jpg)

**Figura 4**

## <a name="implementing-membership-in-your-application"></a>Implementando a associação em seu aplicativo

A maneira mais fácil de implementar a associação do ASP.NET 2,0 em seu aplicativo é usar os controles de logon fornecidos. Esse método permite que você implemente as noções básicas da associação do ASP.NET 2,0 sem escrever nenhum código.

Os seguintes controles de logon estão disponíveis no ASP.NET 2,0:

## <a name="login-control"></a>Controle de logon

O controle de logon fornece uma interface para alguém fazer logon em seu sistema de associação. Ele fornece uma caixa de texto de nome de usuário e senha e um botão de logon. Muitos outros recursos comuns, como um link para se registrar para pessoas que ainda não fizeram isso, uma caixa de seleção que permite ao usuário fazer logon automaticamente em visitas subsequentes, um link para um lembrete de senha etc. Todos os recursos do controle de logon são personalizáveis por meio das propriedades do controle.

No ASP.NET 1. x, os desenvolvedores tinham que escrever uma quantidade razoável de código para fazer uma pesquisa ao usar a autenticação de formulários. Com a associação do ASP.NET 2,0, você pode validar os usuários sem escrever nenhum código. O ASP.NET fará automaticamente a pesquisa do usuário para você. (Se você estiver usando o controle de logon sem usar a associação ASP.NET, poderá usar o método **OnAuthenticate** para validar o usuário.)

## <a name="loginview-control"></a>Controle LoginView

O controle LoginView é um controle modelo que fornece dois modelos por padrão; o AnonymousTemplate e o LoggedInTemplate. O modelo que é exibido é determinado pelo fato de o usuário estar ou não conectado ao seu sistema de associação. Esse controle é normalmente usado para exibir um controle de logon quando um usuário ainda não fez logon e um controle LoginStatus e/ou outros controles de logon quando o usuário fez logon. Se você estiver usando o gerenciamento de função em seu aplicativo ASP.NET, o controle LoginView poderá exibir um modelo específico com base na função users. (Mais informações sobre o gerenciamento de função do ASP.NET serão abordadas posteriormente.)

## <a name="passwordrecovery-control"></a>Controle PasswordRecovery

O controle PasswordRecovery permite que os usuários recebam um email com sua senha atual ou redefinam sua senha. As senhas criptografadas e de texto sem criptografia podem ser recuperadas e enviadas por email aos usuários. Se a senha estiver em hash, ela não poderá ser recuperada. Em vez disso, o usuário será solicitado a executar uma redefinição de senha.

## <a name="loginstatus-control"></a>Controle LoginStatus

O controle LoginStatus é usado para exibir um indicador de logon para os usuários que não estão conectados e um indicador de logout para os usuários que estão conectados no momento. A propriedade Request. IsAuthenticated é usada para determinar qual indicador exibir. O indicador exibido pelo controle LoginStatus pode ser texto (implementado por meio das propriedades **LoginText** e **LogoutText** ) ou imagens (implementadas por meio das propriedades **LoginImageUrl** e **LogoutImageUrl** ).

Quando um usuário faz logoff por meio do controle LoginStatus, ele é redirecionado para a URL especificada pela propriedade **LogoutPageUrl** . Se essa propriedade não for definida, a página atual será atualizada. Como o site é provavelmente protegido por autenticação de formulários, a atualização da página atual redirecionará o usuário para a página de logon do site.

## <a name="loginname-control"></a>Controle LoginName

O controle LoginName exibe o nome do usuário que está atualmente conectado ao site.

## <a name="createuserwizard-control"></a>Controle CreateUserWizard

O controle CreateUserWizard fornece aos usuários uma maneira conveniente de se registrar em seu sistema de associação. Você pode adicionar etapas (implementadas como uma coleção de WizardSteps) por meio da interface mostrada abaixo.

![](membership/_static/image5.jpg)

**Figura 5**

O CreateUserWizard é um controle modelo que deriva da classe Wizard e fornece os seguintes modelos:

- **Cabeçalhotemplate** Este modelo controla a aparência do cabeçalho do assistente.
- **Barra lateraltemplate** Este modelo controla a aparência da barra lateral do assistente.
- **StartNavigationTemplate** Este modelo controla a aparência da navegação do assistente na etapa inicial.
- **StepNavigationTemplate** Este modelo controla a aparência da área de navegação quando não está na etapa de início ou término.
- **FinishNavigationTemplate** Este modelo controla a aparência da área de navegação quando na etapa concluir.

Além disso, para cada etapa que você adicionar ao assistente, o ASP.NET criará um modelo personalizado que contém um ContentTemplate e um CustomNavigationTemplate para essa etapa. Para obter detalhes completos sobre como personalizar o CreateUserWizard, consulte a documentação do VS.NET 2005:

## <a name="changepassword-control"></a>Controle ChangePassword

O controle ChangePassword permite que os usuários alterem sua senha. Se a propriedade DisplayUserName for true (é false por padrão), o usuário poderá alterar sua senha quando não estiver conectado. Se o usuário já *estiver* conectado e a propriedade DisplayUserName for true, o usuário poderá alterar a senha de outro usuário que não está conectado, fornecendo a ela o ID de usuário desse usuário.

Tenha em mente que, se você quiser que os usuários possam alterar as senhas sem precisar fazer logon, será necessário garantir que a página na qual o controle ChangePassword é exibido permita o acesso anônimo. Obviamente, os usuários precisarão fornecer sua senha antiga para alterar sua senha.

## <a name="role-management"></a>Gerenciamento de funções

O gerenciamento de função permite atribuir usuários a uma função específica e, em seguida, restringir o acesso a determinados arquivos ou pastas com base nessa função. O gerenciamento de função também fornece uma API para que você possa determinar programaticamente a função de alguém ou determinar todos os usuários em uma função específica e responder de forma adequada.

O gerenciamento de função não é um requisito na associação de ASP.NET, nem é membro de um requisito para usar o gerenciamento de função. No entanto, os dois complementam uns aos outros e é provável que os desenvolvedores os usarão em conjunto.

Para habilitar o gerenciamento de função em seu aplicativo, faça a seguinte alteração no arquivo Web. config:

[!code-xml[Main](membership/samples/sample2.xml)]

Quando o atributo **CacheRolesInCookie** é definido como true, o ASP.NET armazena em cache uma associação de função de usuários em um cookie no cliente. Isso permite que as pesquisas de função ocorram sem chamadas para o RoleProvider. Ao usar esse atributo, os desenvolvedores são incentivados a garantir que o atributo **CookieProtection** esteja definido como ALL. (Essa é a configuração padrão.) Isso garante que os dados do cookie sejam criptografados e ajuda a garantir que o conteúdo dos cookies não tenha sido alterado. As funções podem ser adicionadas usando a ferramenta de administração de site. Ele permite que você defina facilmente as funções, configure o acesso a partes do site com base nessas funções e atribua usuários a funções.

![](membership/_static/image6.jpg)

**Figura 6**

Como mostrado acima, novas funções podem ser adicionadas simplesmente digitando o nome da função e clicando em Adicionar função. As funções existentes podem ser gerenciadas ou excluídas clicando no link apropriado na lista de funções existentes.

Ao gerenciar uma função, você pode adicionar ou remover usuários, conforme mostrado abaixo.

![](membership/_static/image7.jpg)

**Figura 7**

Ao marcar a caixa de seleção o usuário está na função, você pode adicionar facilmente um usuário a uma função específica. O ASP.NET atualizará automaticamente seu banco de dados de associação com as entradas apropriadas. Você também vai querer configurar regras de acesso para seu aplicativo. Os desenvolvedores de ASP.NET 1. x estão familiarizados com isso por meio do elemento de&gt; de autorização &lt;no arquivo Web. config, e essa opção ainda está disponível no ASP.NET 2,0. No entanto, é mais fácil gerenciar as regras de acesso usando a ferramenta de administração de site, conforme mostrado abaixo.

![](membership/_static/image8.jpg)

**Figura 8**

Nesse caso, a pasta administração é realçada (sua dificuldade de ver, pois a ferramenta realça em cinza claro) e a função Administradores recebeu acesso. Todos os outros usuários são negados. Você pode clicar no ícone de cabeçalho para selecionar uma regra e, em seguida, usar os botões mover para cima e mover para baixo para organizar as regras. Assim como com o elemento de&gt; de autorização &lt;ASP.NET, as regras são processadas na ordem em que aparecem. Em outras palavras, se a ordem das regras na captura acima fosse revertida, ninguém teria acesso à pasta de administração porque a primeira regra que ASP.NET seria encontrada seria a regra que nega todos à pasta.

ASP.NET 2,0 adiciona um arquivo Web. config à pasta para a qual você está especificando uma regra de acesso. As regras de acesso podem ser editadas por meio do arquivo de configuração ou por meio da ferramenta de administração de site. Em outras palavras, a ferramenta de administração de site é simplesmente uma interface por meio da qual o arquivo de configuração pode ser editado em um ambiente amigável.

## <a name="using-roles-in-code"></a>Usando funções no código

A API para gerenciamento de função não foi alterada desde a versão 1. x. O método **IsInRole** é usado para determinar se um usuário está em uma função específica.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET também cria uma instância de RolePrincipal como membro do contexto atual. O objeto RolePrincipal pode ser usado para obter todas as funções às quais o usuário pertence da seguinte maneira:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Usando RoleGroups com o controle LoginView

Agora que você tem uma compreensão do gerenciamento de funções e da associação, vamos discutir brevemente como o controle LoginView aproveita esse recurso no ASP.NET 2,0. Conforme discutido anteriormente, o controle LoginView é um controle modelo que contém dois modelos por padrão; o AnonymousTemplate e o LoggedInTemplate. Dentro da caixa de diálogo tarefas do LoginView há um link (mostrado abaixo) que permite que você edite RoleGroups.

![](membership/_static/image9.jpg)

**Figura 9**

Cada objeto RoleGroup contém uma matriz de cadeias de caracteres que define a quais funções o RoleGroup se aplica. Para adicionar um novo RoleGroup ao controle LoginView, clique no link editar RoleGroups. Na imagem acima, você pode ver que adicionei um novo RoleGroup para administradores. Ao selecionar o RoleGroup (RoleGroup [0]) na lista suspensa exibições, posso configurar um modelo que só será exibido aos membros da função Administradores. Na imagem abaixo, adicionei um novo RoleGroup que se aplica aos membros da função Sales e à função Distribution. Isso adiciona um segundo RoleGroup à lista suspensa views na caixa de diálogo LoginView Tasks e qualquer coisa adicionada a esse modelo será visível por qualquer usuário na função Sales ou Distribution.

![](membership/_static/image10.jpg)

**Figura 10**

## <a name="overriding-the-existing-membership-provider"></a>Substituindo o provedor de associação existente

Há duas maneiras de estender a funcionalidade da associação ASP.NET. Em primeiro lugar, você pode, obviamente, alterar a funcionalidade existente da classe SqlMembershipProvider herdando dela e substituindo seus métodos. Por exemplo, se você deseja implementar sua própria funcionalidade quando os usuários são criados, você pode criar sua própria classe que herda de SqlMembershipProvider da seguinte maneira:

[!code-csharp[Main](membership/samples/sample5.cs)]

Se, por outro lado, você quiser criar seu próprio provedor (para armazenar suas informações de associação em um banco de dados do Access, por exemplo), você pode criar seu próprio provedor.

## <a name="creating-your-own-membership-provider"></a>Criando seu próprio provedor de associação

Para criar seu próprio provedor de associação, primeiro você precisará criar uma classe que herda da classe MembershipProvider. Se você estiver usando o VB.NET, o Visual Studio 2005 adicionará os stubs para todos os métodos que você precisa substituir. Se você estiver usando C#, você precisará adicionar os stubs.

Será necessário substituir o seguinte:

- Propriedade ApplicationName
- Função ChangePassword
- Função ChangePasswordQuestionAndAnswer
- Função CreateUser
- Função DeleteUser
- Propriedade EnablePasswordReset
- Propriedade EnablePasswordRetrieval
- Função FindUsersByEmail
- Função FindUsersByName
- Função GetAllUsers
- Função GetNumberOfUsersOnline
- Função GetPassword
- Função GetUser
- Função GetUserNameByEmail
- Propriedade MaxInvalidPasswordAttempts
- Propriedade MinRequiredNonAlphanumericCharacters
- Propriedade MinRequiredPasswordLength
- Propriedade PasswordAttemptWindow
- Propriedade PasswordFormat
- Propriedade PasswordStrengthRegularExpression
- Propriedade RequiresQuestionAndAnswer
- Propriedade RequiresUniqueEmail
- Função ResetPassword
- Desbloquear função de usuário
- Função UpdateUser
- Função ValidateUser

Essa é uma lista bastante implementada como C# desenvolvedor. Talvez você ache mais fácil criar a classe no VB.NET sem nenhuma implementação e, em seguida, usar o reflector do .NET ou uma ferramenta semelhante para converter C#o código para.

A cadeia de conexão e outras propriedades devem ser definidas para seus padrões no método Initialize. (O método Initialize é acionado quando o provedor é carregado em tempo de execução.) O segundo parâmetro para o método Initialize é do tipo System. Collections. especializada. NameValueCollection e é uma referência ao &lt;adicionar&gt; elemento que está associado ao seu provedor personalizado no arquivo Web. config. Essa entrada é semelhante ao seguinte:

[!code-xml[Main](membership/samples/sample6.xml)]

Aqui está um exemplo do método Initialize.

[!code-csharp[Main](membership/samples/sample7.cs)]

Para validar o usuário ao enviar seu formulário de logon, você precisará usar o método ValidateUser. Esse método é acionado quando o usuário clica no botão de logon no controle de logon. Você posicionará o código que faz a pesquisa do usuário nesse método.

Como você pode ver, escrever seu próprio provedor de associação não é difícil e permite que você estenda essa poderosa funcionalidade do ASP.NET 2,0.
