---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: Visão geral dos provedores de armazenamento personalizados para ASP.NET Identity-ASP.NET 4. x
author: Rick-Anderson
description: ASP.NET Identity é um sistema extensível que permite que você crie seu próprio provedor de armazenamento e conecte-o ao seu aplicativo sem retrabalhar com o aplicado...
ms.author: riande
ms.date: 10/13/2014
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 21baedf6285b411f89627df9ca25d47a2a42e387
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584409"
---
# <a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Visão geral de provedores de armazenamento personalizados para a Identidade do ASP.NET

por [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity é um sistema extensível que permite criar seu próprio provedor de armazenamento e conectá-lo ao seu aplicativo sem retrabalhar com o aplicativo. Este tópico descreve como criar um provedor de armazenamento personalizado para ASP.NET Identity. Ele aborda os conceitos importantes para a criação de seu próprio provedor de armazenamento, mas não é uma explicação passo a passo da implementação de um provedor de armazenamento personalizado.
> 
> Para obter um exemplo de implementação de um provedor de armazenamento personalizado, consulte [implementando um provedor de armazenamento de ASP.net Identity do MySQL personalizado](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> Este tópico foi atualizado para o ASP.NET Identity 2,0.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Visual Studio 2013 com atualização 2
> - ASP.NET Identity 2

## <a name="introduction"></a>Introdução

Por padrão, o sistema de ASP.NET Identity armazena informações de usuário em um banco de dados SQL Server e usa Entity Framework Code First para criar o banco de dados. Para muitos aplicativos, essa abordagem funciona bem. No entanto, você pode preferir usar um tipo diferente de mecanismo de persistência, como o armazenamento de tabelas do Azure, ou talvez você já tenha tabelas de banco de dados com uma estrutura muito diferente da implementação padrão. Em ambos os casos, você pode escrever um provedor personalizado para seu mecanismo de armazenamento e conectar esse provedor ao seu aplicativo.

O ASP.NET Identity é incluído por padrão em muitos dos modelos de Visual Studio 2013. Você pode obter atualizações para ASP.NET Identity por meio [do pacote NuGet do Microsoft ASPNET Identity EntityFramework](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

Este tópico inclui as seções a seguir:

- [Entender a arquitetura](#architecture)
- [Entender os dados que são armazenados](#data)
- [Criar a camada de acesso a dados](#dal)
- [Personalizar a classe de usuário](#user)
- [Personalizar o armazenamento do usuário](#userstore)
- [Personalizar a classe de função](#role)
- [Personalizar o repositório de funções](#rolestore)
- [Reconfigurar o aplicativo para usar o novo provedor de armazenamento](#reconfigure)
- [Outras implementações de provedores de armazenamento personalizados](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Entender a arquitetura

ASP.NET Identity consiste em classes chamadas de gerentes e lojas. Os gerentes são classes de alto nível que o desenvolvedor de aplicativo usa para executar operações, como a criação de um usuário, no sistema ASP.NET Identity. As lojas são classes de nível inferior que especificam como as entidades, como usuários e funções, são mantidas. As lojas estão intimamente ligadas ao mecanismo de persistência, mas os gerentes são dissociados das lojas, o que significa que você pode substituir o mecanismo de persistência sem interromper todo o aplicativo.

O diagrama a seguir mostra como o aplicativo Web interage com os gerentes e armazena a interação com a camada de acesso a dados.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Para criar um provedor de armazenamento personalizado para ASP.NET Identity, você precisa criar a fonte de dados, a camada de acesso a dados e as classes de armazenamento que interagem com essa camada de acesso a dados. Você pode continuar usando as mesmas APIs de Gerenciador para executar operações de dados no usuário, mas agora esses dados são salvos em um sistema de armazenamento diferente.

Você não precisa personalizar as classes de Gerenciador porque ao criar uma nova instância de usermanager ou roleManager, você fornece o tipo da classe de usuário e passa uma instância da classe Store como um argumento. Essa abordagem permite que você conecte suas classes personalizadas à estrutura existente. Você verá como criar uma instância de usermanager e roleManager com suas classes de armazenamento personalizadas na seção [reconfigurar aplicativo para usar o novo provedor de armazenamento](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Entender os dados que são armazenados

Para implementar um provedor de armazenamento personalizado, você deve entender os tipos de dados usados com ASP.NET Identity e decidir quais recursos são relevantes para seu aplicativo.

| data | DESCRIÇÃO |
| --- | --- |
| Usuários | Usuários registrados do seu site da Web. Inclui a ID de usuário e o nome de usuário. Pode incluir uma senha com hash se os usuários fizerem logon com credenciais específicas para seu site (em vez de usar credenciais de um site externo, como o Facebook) e o carimbo de segurança para indicar se alguma coisa foi alterada nas credenciais do usuário. Também pode incluir endereço de email, número de telefone, se a autenticação de dois fatores está habilitada, o número atual de logons com falha e se uma conta foi bloqueada. |
| Declarações de usuário | Um conjunto de instruções (ou declarações) sobre o usuário que representa a identidade do usuário. Pode habilitar uma expressão maior da identidade do usuário do que pode ser obtido por meio de funções. |
| Logons de usuário | Informações sobre o provedor de autenticação externa (como o Facebook) para usar ao fazer logon em um usuário. |
| Funções | Grupos de autorização para seu site. Inclui a ID da função e o nome da função (como "admin" ou "Employee"). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Criar a camada de acesso a dados

Este tópico pressupõe que você esteja familiarizado com o mecanismo de persistência que você pretende usar e como criar entidades para esse mecanismo. Este tópico não fornece detalhes sobre como criar repositórios ou classes de acesso a dados; em vez disso, ele fornece algumas sugestões sobre as decisões de design que você precisa fazer ao trabalhar com ASP.NET Identity.

Você tem muita liberdade ao criar repositórios para um provedor de armazenamento personalizado. Você só precisa criar repositórios para os recursos que pretende usar em seu aplicativo. Por exemplo, se você não estiver usando funções em seu aplicativo, não precisará criar armazenamento para funções ou funções de usuário. Sua tecnologia e infraestrutura existente podem exigir uma estrutura muito diferente da implementação padrão do ASP.NET Identity. Na camada de acesso a dados, você fornece a lógica para trabalhar com a estrutura de seus repositórios.

Para obter uma implementação do MySQL de repositórios de dados para o ASP.NET Identity 2,0, consulte [MySQLIdentity. SQL](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

Na camada de acesso a dados, você fornece a lógica para salvar os dados de ASP.NET Identity em sua fonte de dados. A camada de acesso a dados para seu provedor de armazenamento personalizado pode incluir as seguintes classes para armazenar informações de usuário e função.

| Classe | DESCRIÇÃO | Exemplo |
| --- | --- | --- |
| Contexto | Encapsula as informações para se conectar ao mecanismo de persistência e executar consultas. Essa classe é fundamental para sua camada de acesso a dados. As outras classes de dados exigirão uma instância dessa classe para executar suas operações. Você também inicializará suas classes de armazenamento com uma instância dessa classe. | [MySQLDatabase](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Armazenamento do usuário | Armazena e recupera informações do usuário (como o nome de usuário e o hash de senha). | [Usertable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Armazenamento de função | Armazena e recupera informações de função (como o nome da função). | [Função (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Armazenamento de userreivindicações | Armazena e recupera informações de declaração do usuário (como o tipo e o valor da declaração). | [O MySQL (userclaimtable)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Armazenamento de userlogons | Armazena e recupera informações de logon de usuário (como um provedor de autenticação externa). | [UserLoginsTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| Armazenamento de UserRole | Armazena e recupera a quais funções um usuário é atribuído. | [Sqlroletable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

Novamente, você só precisa implementar as classes que pretende usar em seu aplicativo.

Nas classes de acesso a dados, você fornece código para executar operações de dados para seu mecanismo de persistência específico. Por exemplo, dentro da implementação do MySQL, a classe Usertable contém um método para inserir um novo registro na tabela de banco de dados users. A variável chamada `_database` é uma instância da classe MySQLDatabase.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Depois de criar suas classes de acesso a dados, você deve criar classes de armazenamento que chamam os métodos específicos na camada de acesso a dados.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Personalizar a classe de usuário

Ao implementar seu próprio provedor de armazenamento, você deve criar uma classe de usuário que seja equivalente à classe [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) no namespace [Microsoft. ASP. net. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) :

O diagrama a seguir mostra a classe IdentityUser que você deve criar e a interface a ser implementada nessa classe.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

A interface [IUser&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) define as propriedades que o usermanager tenta chamar ao executar as operações solicitadas. A interface contém duas propriedades-ID e nome de usuário. A interface [IUser&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) permite que você especifique o tipo da chave para o usuário por meio do parâmetro de **TKey** genérico. O tipo da propriedade de ID corresponde ao valor do parâmetro TKey.

A estrutura de identidade também fornece a interface [IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) (sem o parâmetro Generic) quando você deseja usar um valor de cadeia de caracteres para a chave.

A classe IdentityUser implementa IUser e contém quaisquer propriedades ou construtores adicionais para os usuários no seu site. O exemplo a seguir mostra uma classe IdentityUser que usa um inteiro para a chave. O campo ID é definido como **int** para corresponder ao valor do parâmetro Generic. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Para obter uma implementação completa, consulte [IdentityUser (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Personalizar o armazenamento do usuário

Você também cria uma classe USERSTORE que fornece os métodos para todas as operações de dados no usuário. Essa classe é equivalente à classe [USERSTORE&lt;TUser&gt;](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx) no namespace [Microsoft. ASP. net. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) . Em sua classe USERSTORE, você implementa o [IUserStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) e qualquer uma das interfaces opcionais. Você seleciona quais interfaces opcionais implementar com base na funcionalidade que deseja fornecer em seu aplicativo.

A imagem a seguir mostra a classe USERSTORE que você deve criar e as interfaces relevantes.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

O modelo de projeto padrão no Visual Studio contém código que assume que muitas das interfaces opcionais foram implementadas no armazenamento do usuário. Se você estiver usando o modelo padrão com um armazenamento de usuário personalizado, deverá implementar interfaces opcionais em seu repositório de usuários ou alterar o código do modelo para não chamar mais métodos nas interfaces que você não implementou.

 O exemplo a seguir mostra uma classe de armazenamento de usuário simples. O parâmetro genérico **TUser** usa o tipo de sua classe de usuário que geralmente é a classe IdentityUser que você definiu. O parâmetro genérico **TKey** usa o tipo de sua chave de usuário. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 Neste exemplo, o construtor que usa um parâmetro denominado *banco de dados* do tipo ExampleDatabase é apenas uma ilustração de como transmitir sua classe de acesso a dados. Por exemplo, na implementação do MySQL, esse construtor usa um parâmetro do tipo MySQLDatabase. 

Em sua classe USERSTORE, você usa as classes de acesso a dados que você criou para executar operações. Por exemplo, na implementação do MySQL, a classe USERSTORE tem o método createasync que usa uma instância de Usertable para inserir um novo registro. O método **Insert** no objeto **usertable** é o mesmo método mostrado na seção anterior. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfaces a serem implementadas ao personalizar o repositório de usuários

A imagem a seguir mostra mais detalhes sobre a funcionalidade definida em cada interface. Todas as interfaces opcionais herdam de IUserStore.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
  A interface [IUserStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) é a única interface que você deve implementar em seu repositório de usuários. Ele define métodos para criar, atualizar, excluir e recuperar usuários.
- **IUserClaimStore**  
  A interface [IUserClaimStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) define os métodos que você deve implementar em seu repositório de usuários para habilitar as declarações do usuário. Ele contém métodos ou adição, remoção e recuperação de declarações do usuário.
- **IUserLoginStore**  
  O [IUserLoginStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) define os métodos que você deve implementar em seu repositório de usuários para habilitar provedores de autenticação externa. Ele contém métodos para adicionar, remover e recuperar logons de usuário e um método para recuperar um usuário com base nas informações de logon.
- **IUserRoleStore**  
  A interface [IUserRoleStore&lt;TKey, TUser&gt;](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) define os métodos que você deve implementar em seu repositório de usuários para mapear um usuário para uma função. Ele contém métodos para adicionar, remover e recuperar funções de um usuário e um método para verificar se um usuário está atribuído a uma função.
- **IUserPasswordStore**  
  A interface [IUserPasswordStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) define os métodos que você deve implementar em seu repositório de usuários para manter as senhas com hash. Ele contém métodos para obter e definir a senha de hash e um método que indica se o usuário definiu uma senha.
- **IUserSecurityStampStore**  
  A interface [IUserSecurityStampStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) define os métodos que você deve implementar em seu repositório de usuários para usar um carimbo de segurança para indicar se as informações da conta do usuário foram alteradas. Esse carimbo é atualizado quando um usuário altera a senha ou adiciona ou remove logons. Ele contém métodos para obter e definir o carimbo de segurança.
- **IUserTwoFactorStore**  
  A interface [IUserTwoFactorStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) define os métodos que devem ser implementados para implementar a autenticação de dois fatores. Ele contém métodos para obter e definir se a autenticação de dois fatores está habilitada para um usuário.
- **IUserPhoneNumberStore**  
  A interface [IUserPhoneNumberStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) define os métodos que devem ser implementados para armazenar números de telefone do usuário. Ele contém métodos para obter e definir o número de telefone e se o número de telefone é confirmado.
- **IUserEmailStore**  
  A interface [IUserEmailStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) define os métodos que devem ser implementados para armazenar endereços de email do usuário. Ele contém métodos para obter e definir o endereço de email e se o email é confirmado.
- **IUserLockoutStore**  
  A interface [IUserLockoutStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) define os métodos que devem ser implementados para armazenar informações sobre o bloqueio de uma conta. Ele contém métodos para obter o número atual de tentativas de acesso com falha, obter e definir se a conta pode ser bloqueada, obter e definir a data de término do bloqueio, incrementar o número de tentativas com falha e redefinir o número de tentativas com falha.
- **IQueryableUserStore**  
  A interface [IQueryableUserStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) define os membros que você deve implementar para fornecer um repositório de usuários passível de consulta. Ele contém uma propriedade que contém os usuários consultáveis.

  Você implementa as interfaces que são necessárias em seu aplicativo; como, as interfaces IUserClaimStore, IUserLoginStore, IUserRoleStore, IUserPasswordStore e IUserSecurityStampStore, conforme mostrado abaixo. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Para obter uma implementação completa (incluindo todas as interfaces), consulte [USERSTORE (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin e IdentityUserRole

O namespace Microsoft. AspNet. Identity. EntityFramework contém implementações das classes [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx)e [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) . Se você estiver usando esses recursos, talvez queira criar suas próprias versões dessas classes e definir as propriedades do seu aplicativo. No entanto, às vezes é mais eficiente não carregar essas entidades na memória ao executar operações básicas (como adicionar ou remover uma declaração de usuário). Em vez disso, as classes de armazenamento de back-end podem executar essas operações diretamente na fonte de dados. Por exemplo, o método USERSTORE. GetClaimsAsync () pode chamar userclaimtable. FindByUserId (User. ID) para executar uma consulta na tabela diretamente e retornar uma lista de declarações.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Personalizar a classe de função

Ao implementar seu próprio provedor de armazenamento, você deve criar uma classe de função que seja equivalente à classe [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) no namespace [Microsoft. ASP. net. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) :

O diagrama a seguir mostra a classe IdentityRole que você deve criar e a interface a ser implementada nessa classe.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

A interface [IRole&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) define as propriedades que o roleManager tenta chamar ao executar as operações solicitadas. A interface contém duas propriedades-ID e nome. A interface [IRole&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) permite que você especifique o tipo da chave para a função por meio do parâmetro de **TKey** genérico. O tipo da propriedade de ID corresponde ao valor do parâmetro TKey.

A estrutura de identidade também fornece a interface [IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) (sem o parâmetro Generic) quando você deseja usar um valor de cadeia de caracteres para a chave.

O exemplo a seguir mostra uma classe IdentityRole que usa um inteiro para a chave. O campo ID é definido como int para corresponder ao valor do parâmetro Generic. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Para obter uma implementação completa, consulte [IdentityRole (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Personalizar o repositório de funções

Você também cria uma classe RoleStore que fornece os métodos para todas as operações de dados em funções. Essa classe é equivalente à classe [RoleStore&lt;TRole&gt;](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) no namespace Microsoft. ASP. net. Identity. EntityFramework. Na sua classe RoleStore, você implementa o [IRoleStore&lt;TRole, tkey&gt;](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx) e, opcionalmente [, a interface IQueryableRoleStore&lt;TRole, TKey&gt;](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx) .

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

O exemplo a seguir mostra uma classe de repositório de funções. O parâmetro genérico TRole usa o tipo de sua classe de função que geralmente é a classe IdentityRole que você definiu. O parâmetro genérico TKey usa o tipo de sua chave de função. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
  A interface [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx) define os métodos a serem implementados em sua classe de armazenamento de função. Ele contém métodos para criar, atualizar, excluir e recuperar funções.
- **RoleStore&lt;TRole&gt;**  
  Para personalizar o RoleStore, crie uma classe que implemente a interface IRoleStore. Você só precisa implementar essa classe se quiser usar funções em seu sistema. O construtor que usa um parâmetro denominado *banco* de dados do tipo ExampleDatabase é apenas uma ilustração de como transmitir sua classe de acesso a dados. Por exemplo, na implementação do MySQL, esse construtor usa um parâmetro do tipo MySQLDatabase.  
  
  Para obter uma implementação completa, consulte [RoleStore (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Reconfigurar o aplicativo para usar o novo provedor de armazenamento

Você implementou seu novo provedor de armazenamento. Agora, você deve configurar seu aplicativo para usar esse provedor de armazenamento. Se o provedor de armazenamento padrão foi incluído no seu projeto, você deve remover o provedor padrão e substituí-lo pelo seu provedor.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Substituir o provedor de armazenamento padrão no projeto MVC

1. Na janela **gerenciar pacotes NuGet** , desinstale o pacote do **Microsoft ASP.net Identity EntityFramework** . Você pode encontrar esse pacote pesquisando nos pacotes instalados para Identity. EntityFramework.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png) será perguntado se você também deseja desinstalar o Entity Framework. Se você não precisar dele em outras partes do seu aplicativo, poderá desinstalá-lo.
2. No arquivo IdentityModels.cs na pasta modelos, exclua ou comente as classes **ApplicationUser** e **ApplicationDbContext** . Em um aplicativo MVC, você pode excluir o arquivo IdentityModels.cs inteiro. Em um aplicativo Web Forms, exclua as duas classes, mas lembre-se de manter a classe auxiliar que também está localizada no arquivo IdentityModels.cs.
3. Se o provedor de armazenamento residir em um projeto separado, adicione uma referência a ele em seu aplicativo Web.
4. Substitua todas as referências a `using Microsoft.AspNet.Identity.EntityFramework;` com uma instrução using para o namespace do seu provedor de armazenamento.
5. Na classe **Startup.auth.cs** , altere o método **ConfigureAuth** para usar uma única instância do contexto apropriado. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. Na pasta iniciar do aplicativo\_, abra **IdentityConfig.cs**. Na classe ApplicationUserManager, altere o método **Create** para retornar um Gerenciador de usuários que usa seu armazenamento de usuário personalizado. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Substitua todas as referências a **ApplicationUser** por **IdentityUser**.
8. O projeto padrão inclui alguns membros na classe de usuário que não estão definidos na interface IUser; como email, PasswordHash e GenerateUserIdentityAsync. Se sua classe de usuário não tiver esses membros, você deverá implementá-los ou alterar o código que usa esses membros.
9. Se você tiver criado qualquer instância de roleManager, altere esse código para usar sua nova classe RoleStore.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. O projeto padrão é projetado para uma classe de usuário que tem um valor de cadeia de caracteres para a chave. Se sua classe de usuário tiver um tipo diferente para a chave (como um inteiro), você deverá alterar o projeto para trabalhar com seu tipo. Consulte [alterar chave primária para usuários em ASP.net Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. Se necessário, adicione a cadeia de conexão ao arquivo Web. config.

<a id="other"></a>
## <a name="other-resources"></a>Outros recursos

- Blog: [implementando ASP.net Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Tutorial e código GIT: [provedor de identidade Simple. Data ASP.net](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Tutorial:[configurando as contas de identidade básicas e apontando-as em um BD externo](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Por [@xivSolutions](https://twitter.com/xivSolutions).
- Tutorial[: Implementando um provedor de armazenamento de ASP.net Identity do MySQL personalizado](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [CodeFluent entidades](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) por [SoftFluent](http://www.softfluent.com/)
- [Armazenamento de tabelas do Azure](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) por James Randall.
- Armazenamento de tabelas do Azure: [ASPNET. Identity. TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) por [@stuartleeks](https://twitter.com/stuartleeks).
- [CouchDB/Cloudd por Daniel Wertheim.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Elástico pesqui[h: identidade elástica](https://github.com/bmbsqd/elastic-identity) por Bombsquad AB.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) por Jonathan Sheely Jonathan Sheely.
- [NHibernate. AspNet. Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) por Antônio milhasi Bastos.
- O [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) por [@tourismgeek](https://twitter.com/tourismgeek).
- [RavenDB. AspNet. Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) por [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis. AspNet. Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Modelos T4 para gerar código EF para um repositório de usuário "primeiro banco de dados": [ASPNET. Identity. EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
