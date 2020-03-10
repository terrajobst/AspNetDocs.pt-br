---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Adicionando ASP.NET Identity a um projeto Web Forms vazio ou existente-ASP.NET 4. x
author: raquelsa
description: Este tutorial mostra como adicionar ASP.NET Identity (o sistema de associação para ASP.NET) a um aplicativo ASP.NET. Quando você cria um novo Web Forms ou MVC...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 8e82951d57f0b8052ee3f6530a7470be7d030206
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78584122"
---
# <a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Adição de Identidade do ASP.NET a um projeto vazio ou existente do Web Forms

> Este tutorial mostra como adicionar [ASP.net Identity](introduction-to-aspnet-identity.md) (o novo sistema de associação para ASP.net) a um aplicativo ASP.net.
> 
> Quando você cria um novo projeto Web Forms ou MVC no Visual Studio 2017 RTM com contas individuais, o Visual Studio instalará todos os pacotes necessários e adicionará todas as classes necessárias para você. Este tutorial ilustra as etapas para adicionar ASP.NET Identity suporte ao seu projeto Web Forms existente ou a um novo projeto vazio. Vou descrever todos os pacotes NuGet que você precisa instalar e as classes que precisa adicionar. Abordarei Web Forms de exemplo para registrar novos usuários e fazer logon enquanto realçar todas as APIs de ponto de entrada principal para gerenciamento e autenticação de usuários. Este exemplo usará o ASP.NET Identity implementação padrão para o armazenamento de dados SQL, que é criado em Entity Framework. Este tutorial, usaremos o LocalDB para o banco de dados SQL.
> 

## <a name="get-started-with-aspnet-identity"></a>Introdução ao ASP.NET Identity

1. Comece Instalando e executando o [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
2. Selecione **novo projeto** na página inicial, ou você pode usar o menu e selecionar **arquivo**e, em seguida, **novo projeto**.
3. No painel esquerdo, expanda **Visual C#** , selecione **Web**e, em seguida, **aplicativo Web ASP.net (.NET Framework)** . Nomeie seu projeto como "WebFormsIdentity" e selecione **OK**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image17.png)
4. Na caixa de diálogo **novo projeto ASP.net** , selecione o modelo **vazio** .
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Observe que o botão **alterar autenticação** está desabilitado e nenhum suporte de autenticação é fornecido neste modelo. Os modelos Web Forms, MVC e API Web permitem que você selecione a abordagem de autenticação.

## <a name="add-identity-packages-to-your-app"></a>Adicionar pacotes de identidade ao seu aplicativo

Em Gerenciador de Soluções, clique com o botão direito do mouse em seu projeto e selecione **gerenciar pacotes NuGet**. Procure e instale o pacote **Microsoft. AspNet. Identity. EntityFramework** . 
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image15.png)
  
Observe que esse pacote instalará os pacotes de dependência: **EntityFramework** e **Microsoft ASP.net Identity Core**.

## <a name="add-a-web-form-to-register-users"></a>Adicionar um formulário da Web para registrar usuários

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar**e, em seguida, **formulário da Web**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. Na caixa de diálogo **especificar nome para o item** , nomeie o novo **registro**de formulário da Web e, em seguida, selecione **OK**
3. Substitua a marcação no arquivo *Register. aspx* gerado pelo código a seguir. As alterações de código são realçadas. 

    [!code-html[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Essa é apenas uma versão simplificada do arquivo *Register. aspx* que é criado quando você cria um novo projeto ASP.NET Web Forms. A marcação acima adiciona campos de formulário e um botão para registrar um novo usuário.
4. Abra o arquivo *Register.aspx.cs* e substitua o conteúdo do arquivo pelo código a seguir:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. O código acima é uma versão simplificada do arquivo *Register.aspx.cs* que é criada quando você cria um novo projeto de Web Forms do ASP.net.
    > 2. A classe *IdentityUser* é a implementação padrão do EntityFramework da interface *IUser* . A interface *IUser* é a interface mínima para um usuário no ASP.net Identity Core.
    > 3. A classe *USERSTORE* é a implementação padrão do EntityFramework de um armazenamento de usuário. Essa classe implementa as interfaces mínimas do ASP.NET Identity Core: *IUserStore*, *IUserLoginStore*, *IUserClaimStore* e *IUserRoleStore*.
    > 4. A classe *usermanager* expõe as APIs relacionadas ao usuário, que salvarão automaticamente as alterações no *USERSTORE*.
    > 5. A classe *IdentityResult* representa o resultado de uma operação de identidade.
5. Em **Gerenciador de soluções**, clique com o botão direito do mouse em seu projeto e selecione **Adicionar**, **Adicionar pasta ASP.net** e, em seguida, **aplicativos\_dados**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Abra o arquivo *Web. config* e adicione uma entrada de cadeia de conexão para o banco de dados que usaremos para armazenar informações do usuário. O banco de dados será criado em tempo de execução pelo EntityFramework para as entidades de identidade. A cadeia de conexão é semelhante a uma criada para você quando você cria um novo projeto de Web Forms. O código realçado mostra a marcação que você deve adicionar:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Para o Visual Studio 2015 ou superior, substitua `(localdb)\v11.0` por `(localdb)\MSSQLLocalDB` na cadeia de conexão.
    
7. Clique com o botão direito do mouse em File *Register. aspx* em seu projeto e selecione **definir como página inicial**. Pressione CTRL + F5 para compilar e executar o aplicativo Web. Insira um novo nome de usuário e senha e, em seguida, selecione **registrar**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET Identity tem suporte para validação e, neste exemplo, você pode verificar o comportamento padrão nos validadores de usuário e senha que vêm do pacote de identidade principal. O validador padrão para o usuário (`UserValidator`) tem uma propriedade `AllowOnlyAlphanumericUserNames` que tem o valor padrão definido como `true`. O validador padrão para a senha (`MinimumLengthValidator`) garante que a senha tenha pelo menos 6 caracteres. Esses validadores são propriedades em `UserManager` que podem ser substituídas se você quiser ter validação personalizada,

## <a name="verify-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Verificar o banco de dados de identidade LocalDb e as tabelas geradas por Entity Framework

1. No menu **Exibir** , selecione **Gerenciador de servidores**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Expanda **DefaultConnection (WebFormsIdentity)** , expanda **tabelas**, clique com o botão direito do mouse em **AspNetUsers** e selecione **Mostrar dados da tabela**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configure-the-application-for-owin-authentication"></a>Configurar o aplicativo para autenticação OWIN

Neste ponto, adicionamos apenas suporte para a criação de usuários. Agora, vamos demonstrar como podemos adicionar autenticação para fazer logon em um usuário. ASP.NET Identity usa o middleware de autenticação OWIN da Microsoft para autenticação de formulários. A autenticação de cookie OWIN é um mecanismo de autenticação baseado em declarações e um cookie que pode ser usado por qualquer estrutura hospedada no [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) ou no IIS. Com esse modelo, os mesmos pacotes de autenticação podem ser usados em várias estruturas, incluindo ASP.NET MVC e Web Forms. Para obter mais informações sobre o projeto Katana e como executar o middleware em um host independente, consulte [introdução com o projeto Katana](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="install-authentication-packages-to-your-application"></a>Instalar pacotes de autenticação em seu aplicativo

1. Em Gerenciador de Soluções, clique com o botão direito do mouse em seu projeto e selecione **gerenciar pacotes NuGet**. Procure e instale o pacote ***Microsoft. AspNet. Identity. Owin*** . 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image16.png)

2. Procure e instale o pacote ***Microsoft. Owin. host. SystemWeb*** .

    > [!NOTE]
    > O pacote **Microsoft. AspNet. Identity. Owin** contém um conjunto de classes de extensão Owin para gerenciar e configurar o middleware de autenticação Owin a ser consumido por pacotes de núcleo ASP.net Identity.
    > O pacote **Microsoft. Owin. host. SystemWeb** contém um servidor Owin que permite que aplicativos baseados em Owin sejam executados no IIS usando o pipeline de solicitação ASP.net. Para obter mais informações, consulte [middleware OWIN no pipeline integrado do IIS](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="add-owin-startup-and-authentication-configuration-classes"></a>Adicionar classes de configuração de inicialização e autenticação do OWIN

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse em seu projeto, selecione **Adicionar**e **Adicionar novo item**. No diálogo Pesquisar caixa de texto, digite "*Owin*". Nomeie a classe "*Startup*" e selecione **Adicionar**. 
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. No arquivo Startup.cs, adicione o código realçado mostrado abaixo para configurar a autenticação de cookie do OWIN.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Essa classe contém o atributo `OwinStartup` para especificar a classe de inicialização OWIN. Cada aplicativo OWIN tem uma classe de inicialização na qual você especifica componentes para o pipeline de aplicativo. Consulte [detecção de classe de inicialização OWIN](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) para obter mais informações sobre este modelo.

## <a name="add-web-forms-for-registering-and-signing-in-users"></a>Adicionar formulários da Web para registro e entrada de usuários

1. Abra o arquivo *Register.aspx.cs* e adicione o código a seguir que assina o usuário quando o registro é executado com sucesso.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Como a autenticação de cookie ASP.NET Identity e OWIN é um sistema baseado em declarações, a estrutura requer que o desenvolvedor do aplicativo gere um [ClaimsIdentity](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) para o usuário. ClaimsIdentity tem informações sobre todas as declarações para o usuário, como a quais funções o usuário pertence. Você também pode adicionar mais declarações para o usuário neste estágio.
    > - Você pode entrar no usuário usando o CustomTargetNameDictionary de OWIN e chamando `SignIn` e passando o ClaimsIdentity conforme mostrado acima. Esse código conectará o usuário e gerará um cookie também. Essa chamada é análoga a [FormAuthentication. SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) usado pelo módulo [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) .
2. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto, selecione **Adicionar**e, em seguida, **formulário da Web**. Nomeie o **logon**do Web Form.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Substitua o conteúdo do arquivo *login. aspx* pelo código a seguir:

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Substitua o conteúdo do arquivo *login.aspx.cs* pelo seguinte:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - O `Page_Load` agora verifica o status do usuário atual e executa uma ação com base em seu status de `Context.User.Identity.IsAuthenticated`.
    >   **Exibir nome de usuário conectado** : o Microsoft ASP.NET estrutura de identidade adicionou métodos de extensão em [System. Security. principal. IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) que permite que você obtenha o `UserName` e `UserId` para o usuário conectado. Esses métodos de extensão são definidos no assembly `Microsoft.AspNet.Identity.Core`. Esses métodos de extensão são a substituição para [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - Método de entrada: `This` método substitui o método `CreateUser_Click` anterior neste exemplo e agora entra no usuário depois de criar o usuário com êxito.   
    >   O Microsoft OWIN Framework adicionou métodos de extensão em `System.Web.HttpContext` que permite obter uma referência a um `IOwinContext`. Esses métodos de extensão são definidos no assembly `Microsoft.Owin.Host.SystemWeb`. A classe `OwinContext` expõe uma propriedade `IAuthenticationManager` que representa a funcionalidade de middleware de autenticação disponível na solicitação atual. Você pode entrar no usuário usando o `AuthenticationManager` de OWIN e chamando `SignIn` e passando o `ClaimsIdentity` conforme mostrado acima. Como a autenticação de cookie ASP.NET Identity e OWIN é um sistema baseado em declarações, a estrutura requer que o aplicativo gere um `ClaimsIdentity` para o usuário. O `ClaimsIdentity` tem informações sobre todas as declarações para o usuário, como as funções às quais o usuário pertence. Você também pode adicionar mais declarações para o usuário neste estágio. esse código entrará no usuário e gerará um cookie também. Essa chamada é análoga a [FormAuthentication. SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) usado pelo módulo [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) .
    > - método de `SignOut`: Obtém uma referência ao `AuthenticationManager` de OWIN e chama `SignOut`. Isso é análogo ao método [FormsAuthentication. SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) usado pelo módulo [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) .
5. Pressione **Ctrl + F5** para compilar e executar o aplicativo Web. Insira um novo nome de usuário e senha e, em seguida, selecione **registrar**.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Observação: neste ponto, o novo usuário é criado e conectado.
6. Selecione o botão **fazer logoff** . Você será redirecionado para o formulário de logon.
7. Insira um nome de usuário ou senha inválido e selecione o botão **fazer logon** . 
   O método `UserManager.Find` retornará NULL e a mensagem de erro: " *nome de usuário ou senha inválido* " será exibido.
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
