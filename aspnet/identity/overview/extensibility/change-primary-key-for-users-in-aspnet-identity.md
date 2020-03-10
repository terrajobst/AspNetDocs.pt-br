---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Alterar a chave primária para usuários em ASP.NET Identity-ASP.NET 4. x
author: Rick-Anderson
description: No Visual Studio 2013, o aplicativo Web padrão usa um valor de cadeia de caracteres para a chave para contas de usuário. ASP.NET Identity permite que você altere o tipo de...
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0afea8eacfc646f1489b87629fdb2d437815d88c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584458"
---
# <a name="change-primary-key-for-users-in-aspnet-identity"></a>Alterar a chave primária de usuários na Identidade do ASP.NET

por [Tom FitzMacken](https://github.com/tfitzmac)

> No Visual Studio 2013, o aplicativo Web padrão usa um valor de cadeia de caracteres para a chave para contas de usuário. ASP.NET Identity permite que você altere o tipo da chave para atender aos seus requisitos de dados. Por exemplo, você pode alterar o tipo da chave de uma cadeia de caracteres para um número inteiro.
> 
> Este tópico mostra como começar com o aplicativo Web padrão e alterar a chave de conta de usuário para um número inteiro. Você pode usar as mesmas modificações para implementar qualquer tipo de chave em seu projeto. Ele mostra como fazer essas alterações no aplicativo Web padrão, mas você pode aplicar modificações semelhantes a um aplicativo personalizado. Ele mostra as alterações necessárias ao trabalhar com MVC ou Web Forms.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Visual Studio 2013 com a atualização 2 (ou posterior)
> - ASP.NET Identity 2,1 ou posterior

Para executar as etapas neste tutorial, você deve ter Visual Studio 2013 atualização 2 (ou posterior) e um aplicativo Web criado com base no modelo de aplicativo Web ASP.NET. O modelo foi alterado na atualização 3. Este tópico mostra como alterar o modelo na atualização 2 e na atualização 3.

Este tópico contém as seguintes seções:

- [Alterar o tipo da chave na classe de usuário de identidade](#userclass)
- [Adicionar classes de identidade personalizadas que usam o tipo de chave](#customclass)
- [Alterar a classe de contexto e o Gerenciador de usuários para usar o tipo de chave](#context)
- [Alterar a configuração de inicialização para usar o tipo de chave](#startup)
- [Para MVC com atualização 2, altere o AccountController para passar o tipo de chave](#mvcupdate2)
- [Para MVC com atualização 3, altere AccountController e ManageController para passar o tipo de chave](#mvcupdate3)
- [Para Web Forms com a atualização 2, altere as páginas da conta para passar o tipo de chave](#webformsupdate2)
- [Para Web Forms com a atualização 3, altere as páginas da conta para passar o tipo de chave](#webformsupdate3)
- [Executar aplicativo](#run)
- [Outros recursos](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Alterar o tipo da chave na classe de usuário de identidade

Em seu projeto criado no modelo de aplicativo Web ASP.NET, especifique que a classe ApplicationUser usa um inteiro para a chave para contas de usuário. Em IdentityModels.cs, altere a classe ApplicationUser para herdar de IdentityUser que tem um tipo de **int** para o parâmetro genérico TKey. Você também passa os nomes de três classes personalizadas que ainda não implementou.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Você alterou o tipo da chave, mas, por padrão, o restante do aplicativo ainda assume que a chave é uma cadeia de caracteres. Você deve indicar explicitamente o tipo da chave no código que assume uma cadeia de caracteres.

Na classe **ApplicationUser** , altere o método **GenerateUserIdentityAsync** para incluir int, conforme mostrado no código realçado abaixo. Essa alteração não é necessária para projetos Web Forms com o modelo atualização 3.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Adicionar classes de identidade personalizadas que usam o tipo de chave

As outras classes de identidade, como IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, USERSTORE, RoleStore, ainda estão configuradas para usar uma chave de cadeia de caracteres. Crie novas versões dessas classes que especificam um número inteiro para a chave. Você não precisa fornecer muito código de implementação nessas classes, você está apenas definindo int como a chave.

Adicione as seguintes classes ao arquivo IdentityModels.cs.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Alterar a classe de contexto e o Gerenciador de usuários para usar o tipo de chave

No IdentityModels.cs, altere a definição da classe **ApplicationDbContext** para usar suas novas classes personalizadas e um **int** para a chave, conforme mostrado no código realçado.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

O parâmetro ThrowIfV1Schema não é mais válido no construtor. Altere o construtor para que ele não passe um valor ThrowIfV1Schema.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

Abra IdentityConfig.cs e altere a classe **ApplicationUserManger** para usar sua nova classe de armazenamento de usuário para manter dados e um **int** para a chave.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

No modelo atualização 3, você deve alterar a classe ApplicationSignInManager.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Alterar a configuração de inicialização para usar o tipo de chave

Em Startup.Auth.cs, substitua o código OnValidateIdentity, conforme realçado abaixo. Observe que a definição de getUserIdCallback, analisa o valor da cadeia de caracteres em um número inteiro.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Se o seu projeto não reconhecer a implementação genérica do método **GetUserID** , talvez seja necessário atualizar o pacote ASP.net Identity NuGet para a versão 2,1

Você fez muitas alterações nas classes de infraestrutura usadas pelo ASP.NET Identity. Se você tentar compilar o projeto, observará muitos erros. Felizmente, os erros restantes são todos semelhantes. A classe Identity espera um inteiro para a chave, mas o controlador (ou formulário da Web) está passando um valor de cadeia de caracteres. Em cada caso, você precisa converter de uma cadeia de caracteres para e inteiro chamando **Getuserid&lt;int&gt;** . Você pode trabalhar na lista de erros da compilação ou seguir as alterações abaixo.

As alterações restantes dependem do tipo de projeto que você está criando e da atualização instalada no Visual Studio. Você pode ir diretamente para a seção relevante por meio dos links a seguir

- [Para MVC com atualização 2, altere o AccountController para passar o tipo de chave](#mvcupdate2)
- [Para MVC com atualização 3, altere AccountController e ManageController para passar o tipo de chave](#mvcupdate3)
- [Para Web Forms com a atualização 2, altere as páginas da conta para passar o tipo de chave](#webformsupdate2)
- [Para Web Forms com a atualização 3, altere as páginas da conta para passar o tipo de chave](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Para MVC com atualização 2, altere o AccountController para passar o tipo de chave

Abra o arquivo AccountController.cs. Você precisa alterar os métodos a seguir.

Método **ConfirmEmail**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**Desassociar** método

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

Método **Manage (ManageUserViewModel)**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

Método **LinkLoginCallback**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

Método **RemoveAccountList**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

Método **HasPassword**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

Agora você pode [executar o aplicativo](#run) e registrar um novo usuário.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Para MVC com atualização 3, altere AccountController e ManageController para passar o tipo de chave

Abra o arquivo AccountController.cs. Você precisa alterar o método a seguir.

Método **ConfirmEmail**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

Método **SendCode**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

Abra o arquivo ManageController.cs. Você precisa alterar os métodos a seguir.

Método de **índice**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

Métodos **RemoveLogin**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

Método **AddPhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

Método **EnableTwoFactorAuthentication**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

Método **DisableTwoFactorAuthentication**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

Métodos **VerifyPhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

Método **RemovePhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

Método **ChangePassword**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

Método **SetPassword**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

Método **ManageLogins**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

Método **LinkLoginCallback**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

Método **HasPassword**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

Método **HasPhoneNumber**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

Agora você pode [executar o aplicativo](#run) e registrar um novo usuário.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Para Web Forms com a atualização 2, altere as páginas da conta para passar o tipo de chave

Para Web Forms com a atualização 2, você precisa alterar as páginas a seguir.

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Agora você pode [executar o aplicativo](#run) e registrar um novo usuário.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Para Web Forms com a atualização 3, altere as páginas da conta para passar o tipo de chave

Para Web Forms com a atualização 3, você precisa alterar as páginas a seguir.

**Confirm.aspx.cx**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>Executar aplicativo

Você concluiu todas as alterações necessárias no modelo de aplicativo Web padrão. Execute o aplicativo e registre um novo usuário. Depois de registrar o usuário, você observará que a tabela AspNetUsers tem uma coluna de ID que é um número inteiro.

![nova chave primária](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Se você tiver criado anteriormente as tabelas de ASP.NET Identity com uma chave primária diferente, precisará fazer algumas alterações adicionais. Se possível, basta excluir o banco de dados existente. O banco de dados será recriado com o design correto quando você executar o aplicativo Web e adicionar um novo usuário. Se a exclusão não for possível, execute as migrações do Code First para alterar as tabelas. No entanto, a nova chave primária de inteiro não será configurada como uma propriedade de identidade do SQL no banco de dados. Você deve definir manualmente a coluna de ID como uma identidade.

<a id="other"></a>
## <a name="other-resources"></a>Outros recursos

- [Visão geral de provedores de armazenamento personalizados para a Identidade do ASP.NET](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Migração de um site existente da Associação do SQL para a Identidade do ASP.NET](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Migrando dados do provedor universal para associação e perfis de usuário para ASP.NET Identity](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Aplicativo de exemplo](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/ChangePK) com chave primária alterada
