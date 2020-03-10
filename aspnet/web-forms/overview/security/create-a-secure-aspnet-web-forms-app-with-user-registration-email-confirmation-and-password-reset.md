---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: Criar um aplicativo de Web Forms seguro do ASP.NET com registro de usuário, confirmação de emailC#e redefinição de senha () | Microsoft Docs
author: Erikre
description: Este tutorial mostra como criar um aplicativo ASP.NET Web Forms com registro de usuário, confirmação de email e redefinição de senha usando o membro ASP.NET Identity...
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: af3653bc164810126bc3bf8f1b1794d75642d807
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78625149"
---
# <a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Criar um aplicativo de Web Forms do ASP.NET seguro com registro de usuário, confirmação por email e redefinição de senha (C#)

por [Erik Reitan](https://github.com/Erikre)

> Este tutorial mostra como criar um aplicativo de Web Forms de ASP.NET com registro de usuário, confirmação de email e redefinição de senha usando o sistema de associação de ASP.NET Identity. Este tutorial foi baseado no [tutorial do MVC](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md)de Rick Anderson.

## <a name="introduction"></a>Introdução

Este tutorial orienta você pelas etapas necessárias para criar um aplicativo de Web Forms ASP.NET usando o Visual Studio e o ASP.NET 4,5 para criar um aplicativo de Web Forms seguro com registro de usuário, confirmação de email e redefinição de senha.

### <a name="tutorial-tasks-and-information"></a>Informações e tarefas do tutorial:

- [Criar um aplicativo de Web Forms ASP.NET](#createWebForms)
- [Conectar SendGrid](#SG)
- [Exigir confirmação de email antes de fazer logon](#require)
- [Recuperação e redefinição de senha](#reset)
- [Link reenviar confirmação de email](#rsend)
- [Solucionando problemas do aplicativo](#dbg)
- [Recursos adicionais](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Criar um aplicativo de Web Forms ASP.NET

Comece Instalando e executando o [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instale [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior também.

> [!NOTE]
> Aviso: você deve instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior para concluir este tutorial.

1. Crie um novo projeto (**arquivo** -&gt; **novo projeto**) e selecione o modelo de **aplicativo Web ASP.net** e a versão mais recente do .NET Framework na caixa de diálogo **novo projeto** .
2. Na caixa de diálogo **novo projeto ASP.net** , selecione o modelo de **Web Forms** . Deixe a autenticação padrão como **contas de usuário individuais**. Se você quiser hospedar o aplicativo no Azure, deixe a caixa de seleção **host na nuvem** marcada.   
 Em seguida, clique em **OK** para criar o novo projeto.  
    ![Caixa de diálogo Novo Projeto ASP .NET](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Habilite o protocolo SSL (SSL) para o projeto. Siga as etapas disponíveis na seção **habilitar SSL para o projeto** do [introdução com Web Forms série de tutoriais](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Execute o aplicativo, clique no link **registrar** e registre um novo usuário. Neste ponto, a única validação no email é baseada no atributo [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) para garantir que o endereço de email esteja bem formado. Você modificará o código para adicionar confirmação de email. Feche a janela do navegador.
5. Em **Gerenciador de servidores** do Visual Studio (**View** -&gt; **Gerenciador de servidores**), navegue até **Data Connections\DefaultConnection\Tables\AspNetUsers**, clique com o botão direito do mouse e selecione **Abrir definição de tabela**. 

    A imagem a seguir mostra o esquema de tabela `AspNetUsers`:

    ![Esquema de tabela AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. Em **Gerenciador de servidores**, clique com o botão direito do mouse na tabela **AspNetUsers** e selecione **Mostrar dados da tabela**.  
  
    ![Dados da tabela AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 Neste ponto, o email para o usuário registrado não foi confirmado.
7. Clique na linha e selecione Excluir para excluir o usuário. Você adicionará esse email novamente na próxima etapa e enviará uma mensagem de confirmação para o endereço de email.

## <a name="email-confirmation"></a>Confirmação de email

É uma prática recomendada confirmar o email durante o registro de um novo usuário para verificar se eles não estão representando outra pessoa (ou seja, se eles não foram registrados com o email de outra pessoa). Suponha que você tenha um fórum de discussão, você desejaria impedir que `"bob@cpandl.com"` se registrem como `"joe@contoso.com"`. Sem confirmação por email, `"joe@contoso.com"` pode obter email indesejado de seu aplicativo. Suponha que Bob seja acidentalmente registrado como `"bib@cpandl.com"` e não tenha notado, ele não seria capaz de usar a recuperação de senha porque o aplicativo não tem seu email correto. A confirmação por email fornece apenas proteção limitada de bots e não fornece proteção contra remetentes de spam determinados.

Em geral, você desejará impedir que novos usuários postem dados em seu site antes de serem confirmados por email, uma mensagem de texto SMS ou outro mecanismo. Nas seções a seguir, Habilitaremos a confirmação de email e modificaremos o código para impedir que usuários registrados recentemente façam logon até que seu email seja confirmado. Você usará o serviço de email SendGrid neste tutorial.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Conectar SendGrid

SendGrid alterou a API desde que este tutorial foi escrito. Para obter as instruções de SendGrid atuais, consulte [SendGrid](http://sendgrid.com/) ou [habilitar confirmação de conta e recuperação de senha](xref:security/authentication/accconfirm#enable-account-confirmation-and-password-recovery).

Embora este tutorial mostre apenas como adicionar notificação por email por meio do [SendGrid](http://sendgrid.com/), você pode enviar emails usando SMTP e outros mecanismos (consulte [recursos adicionais](#addRes)).

1. No Visual Studio, abra o **console do Gerenciador de pacotes** (**ferramentas** -&gt; o gerenciador de **pacotes NuGet** -&gt; **console do Gerenciador de pacotes**) e digite o seguinte comando:  
    `Install-Package SendGrid`
2. Vá para a [página de inscrição do SendGrid do Azure](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) e registre-se para obter uma conta gratuita do SendGrid. Você também pode se inscrever para uma conta gratuita do SendGrid diretamente no [site do SendGrid](http://www.sendgrid.com).
3. Em **Gerenciador de soluções** Abra o arquivo *IdentityConfig.cs* na pasta *\_iniciar do aplicativo* e adicione o seguinte código realçado em amarelo à classe `EmailService` para configurar o **SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Além disso, adicione as seguintes instruções `using` ao início do arquivo *IdentityConfig.cs* : 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Para manter esse exemplo simples, você armazenará os valores da conta de serviço de email na seção `appSettings` do arquivo *Web. config* . Adicione o seguinte XML realçado em amarelo ao arquivo *Web. config* na raiz do seu projeto:

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Segurança-nunca armazene dados confidenciais em seu código-fonte. Neste exemplo, a conta e as credenciais são armazenadas na seção **appSetting** do arquivo *Web. config* . No Azure, você pode armazenar esses valores com segurança na guia **[Configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** na portal do Azure. Para obter informações relacionadas, consulte o tópico de Rick Anderson intitulado [práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.net e no Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Adicione os valores do serviço de email para refletir seus valores de autenticação do SendGrid (nome de usuário e senha) para que você possa enviar emails com êxito do seu aplicativo. Certifique-se de usar seu nome de conta do SendGrid em vez do endereço de email que você forneceu SendGrid.

### <a name="enable-email-confirmation"></a>Habilitar confirmação de email

 Para habilitar a confirmação de email, você modificará o código de registro usando as etapas a seguir.  

1. Na pasta *conta* , abra o code-behind *Register.aspx.cs* e atualize o método `CreateUser_Click` para habilitar as seguintes alterações realçadas: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. Em **Gerenciador de soluções**, clique com o botão direito do mouse em *Default. aspx* e selecione **definir como página inicial**.
3. Execute o aplicativo pressionando **F5.** Depois que a página for exibida, clique no link **registrar** para exibir a página de registro.
4. Insira seu email e senha e, em seguida, clique no botão **registrar** para enviar uma mensagem de email por meio de SendGrid.  
   O estado atual do seu projeto e do código permitirá que o usuário faça logon depois de concluir o formulário de registro, mesmo que ele não tenha confirmado sua conta.
5. Verifique sua conta de email e clique no link para confirmar seu email.  
   Depois de enviar o formulário de registro, você será conectado.  
    ![Exemplo de logon no site](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Exigir confirmação de email antes de fazer logon

Embora você tenha confirmado a conta de email, neste ponto, não precisaria clicar no link contido no email de verificação para estar totalmente conectado. Na seção a seguir, você modificará o código que exige que novos usuários tenham um email confirmado antes de serem conectados (autenticado).

1. Em **Gerenciador de soluções** do Visual Studio, atualize o evento `CreateUser_Click` no code-behind *Register.aspx.cs* contido na pasta *contas* com as seguintes alterações realçadas: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Atualize o método `LogIn` no code-behind *login.aspx.cs* com as seguintes alterações realçadas: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Executar o aplicativo

 Agora que você implementou o código para verificar se o endereço de email de um usuário foi confirmado, você pode verificar a funcionalidade nas páginas de **registro** e de **logon** . 

1. Exclua todas as contas na tabela **AspNetUsers** que contêm o alias de email que você deseja testar.
2. Execute o aplicativo (**F5**) e verifique se você não pode se registrar como um usuário até confirmar seu endereço de email.
3. Antes de confirmar sua nova conta por email que acabou de ser enviado, tente fazer logon com a nova conta.  
 Você verá que não é possível fazer logon e que você deve ter uma conta de email confirmada.
4. Depois de confirmar seu endereço de email, faça logon no aplicativo.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Recuperação e redefinição de senha

1. No Visual Studio, remova os caracteres de comentário do método `Forgot` no code-behind *forgot.aspx.cs* contido na pasta *Account* , para que o método seja exibido da seguinte maneira: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Abra a página *login. aspx* . Substitua a marcação próximo ao final da seção **loginForm** , conforme destacado abaixo: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Abra o code-behind *login.aspx.cs* e remova a marca de comentário da linha de código a seguir realçada em amarelo no manipulador de eventos `Page_Load`: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Execute o aplicativo pressionando **F5.** Depois que a página for exibida, clique no link **fazer logon** .
5. Clique no link **esqueceu sua senha?** para exibir a página **esqueceu a senha** .
6. Insira seu endereço de email e clique no botão **Enviar** para enviar um email para seu endereço, o que permitirá que você Redefina sua senha.   
   Verifique sua conta de email e clique no link para exibir a página **Redefinir senha** .
7. Na página **Redefinir senha** , insira seu email, senha e senha confirmada. Em seguida, pressione o botão **Redefinir** .  
   Quando você redefinir sua senha com êxito, a página **senha alterada** será exibida. Agora você pode fazer logon com sua nova senha.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Link reenviar confirmação de email

Quando um usuário cria uma nova conta local, ele envia por email um link de confirmação que eles precisam usar antes que possam fazer logon. Se o usuário excluir acidentalmente o email de confirmação ou se o email nunca chegar, ele precisará do link de confirmação enviado novamente. As alterações de código a seguir mostram como habilitar isso.

1. No Visual Studio, abra o code-behind **login.aspx.cs** e adicione o seguinte manipulador de eventos após o manipulador de eventos `LogIn`:   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Modifique o manipulador de eventos `LogIn` no code-behind *login.aspx.cs* alterando o código realçado em amarelo da seguinte maneira: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Atualize a página *login. aspx* adicionando o código realçado em amarelo da seguinte maneira: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Exclua todas as contas na tabela **AspNetUsers** que contêm o alias de email que você deseja testar.
5. Execute o aplicativo (**F5**) e Registre seu endereço de email.
6. Antes de confirmar sua nova conta por email que acabou de ser enviado, tente fazer logon com a nova conta.  
   Você verá que não é possível fazer logon e que você deve ter uma conta de email confirmada. Além disso, agora você pode reenviar uma mensagem de confirmação para sua conta de email.
7. Insira seu endereço de email e senha e pressione o botão de **confirmação reenviar** .
8. Depois de confirmar seu endereço de email com base na mensagem de email enviada recentemente, faça logon no aplicativo.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Solucionando problemas do aplicativo

Se você não receber um email contendo o link para verificar suas credenciais:

- Verifique sua pasta de lixo eletrônico ou spam.
- Faça logon em sua conta do SendGrid e clique no [link atividade de email](https://sendgrid.com/logs/index).
- Certifique-se de que você usou o nome da conta de usuário do SendGrid como um valor *Web. config* em vez de seu endereço de email da conta do SendGrid.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Links para ASP.NET Identity recursos recomendados](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Confirmação de conta e recuperação de senha com ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Série de tutoriais do ASP.NET Web Forms – adicionar um provedor OAuth 2,0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Implantar um aplicativo ASP.NET Web Forms seguro com associação, OAuth e banco de dados SQL no serviço Azure App](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Série de tutoriais do ASP.NET Web Forms – habilitar o SSL para o projeto](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
