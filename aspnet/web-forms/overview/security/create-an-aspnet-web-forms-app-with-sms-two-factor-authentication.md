---
uid: web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
title: Criar um aplicativo ASP.NET Web Forms com a autenticação de dois fatores doC#SMS () | Microsoft Docs
author: Erikre
description: Este tutorial mostra como criar um aplicativo ASP.NET Web Forms com autenticação de dois fatores. Este tutorial foi projetado para complementar o tutorial intitulado CR...
ms.author: riande
ms.date: 10/09/2014
ms.assetid: 716264ae-ab72-45de-bfc5-53a6237089cf
msc.legacyurl: /web-forms/overview/security/create-an-aspnet-web-forms-app-with-sms-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: c9558aca8a655071c0c94ed66433cf721f26c011
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78565460"
---
# <a name="create-an-aspnet-web-forms-app-with-sms-two-factor-authentication-c"></a>Criar um aplicativo de Web Forms do ASP.NET com autenticação de dois fatores por SMS (C#)

por [Erik Reitan](https://github.com/Erikre)

[Baixar o aplicativo ASP.NET Web Forms com o email e a autenticação de dois fatores do SMS](https://code.msdn.microsoft.com/ASPNET-Web-Forms-App-with-5a0ff94e)

> Este tutorial mostra como criar um aplicativo ASP.NET Web Forms com autenticação de dois fatores. Este tutorial foi projetado para complementar o tutorial intitulado [criar um aplicativo secure ASP.NET Web Forms com registro de usuário, confirmação de email e redefinição de senha](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md). Além disso, este tutorial foi baseado no [tutorial do MVC](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)de Rick Anderson.

## <a name="introduction"></a>Introdução

Este tutorial orienta você pelas etapas necessárias para criar um aplicativo de Web Forms ASP.NET que dá suporte à autenticação de dois fatores usando o Visual Studio. A autenticação de dois fatores é uma etapa de autenticação de usuário extra. Essa etapa extra gera um PIN (número de identificação pessoal) exclusivo durante a entrada. O PIN é normalmente enviado ao usuário como uma mensagem de email ou SMS. O usuário do seu aplicativo, em seguida, insere o PIN como uma medida de autenticação extra ao entrar.

### <a name="tutorial-tasks-and-information"></a>Informações e tarefas do tutorial:

- [Criar um aplicativo de Web Forms ASP.NET](#createWebForms)
- [Configurar SMS e autenticação de dois fatores](#SMS)
- [Habilitar a autenticação de dois fatores para o usuário registrado](#use2FA)
- [Recursos adicionais](#addRes)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Criar um aplicativo de Web Forms ASP.NET

Comece Instalando e executando o [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instale [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior também. Além disso, será necessário criar uma conta do [twilio](https://www.twilio.com/try-twilio) , conforme explicado abaixo.

> [!NOTE]
> Importante: você deve instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior para concluir este tutorial.

1. Crie um novo projeto (**arquivo** -&gt; **novo projeto**) e selecione o modelo de **aplicativo Web ASP.net** junto com a .NET Framework versão 4.5.2 na caixa de diálogo **novo projeto** .
2. Na caixa de diálogo **novo projeto ASP.net** , selecione o modelo de **Web Forms** . Deixe a autenticação padrão como **contas de usuário individuais**. Em seguida, clique em **OK** para criar o novo projeto.  
    ![Caixa de diálogo Novo Projeto ASP .NET](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image1.png)
3. Habilite o protocolo SSL (SSL) para o projeto. Siga as etapas disponíveis na seção **habilitar SSL para o projeto** do [introdução com Web Forms série de tutoriais](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. No Visual Studio, abra o **console do Gerenciador de pacotes** (**ferramentas** -&gt; o gerenciador de **pacotes NuGet** -&gt; **console do Gerenciador de pacotes**) e digite o seguinte comando:  
    `Install-Package Twilio`

<a id="SMS"></a>
## <a name="setup-sms-and-two-factor-authentication"></a>Configurar SMS e autenticação de dois fatores

Este tutorial usa twilio, mas você pode usar qualquer provedor de SMS.

1. Crie uma conta do [twilio](https://www.twilio.com/try-twilio) .
2. Na guia **painel** da sua conta do twilio, copie o **SID da conta** e o **token de autenticação.** Você irá adicioná-los ao seu aplicativo posteriormente.
3. Na guia **números** , copie seu número de **telefone** twilio também.
4. Torne o **SID da conta**do twilio, o **token de autenticação** e o número de **telefone** disponíveis para o aplicativo. Para manter as coisas simples, você armazenará esses valores no arquivo *Web. config* . Ao implantar no Azure, você pode armazenar os valores com segurança na seção **appSettings** na guia configurar site. Além disso, ao adicionar o número de telefone, use apenas números.   
   Observe que você também pode adicionar credenciais SendGrid. SendGrid é um serviço de notificação por email. Para obter detalhes sobre como habilitar o SendGrid, consulte a seção "conectar SendGrid" do tutorial intitulado [criar um aplicativo ASP.net seguro Web Forms com registro de usuário, confirmação de email e redefinição de senha.](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md)

    [!code-xml[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample1.xml?highlight=2,6-10)]

    > [!WARNING]
    > Segurança-nunca armazene dados confidenciais em seu código-fonte. Neste exemplo, a conta e as credenciais são armazenadas na seção **appSettings** do arquivo *Web. config* . No Azure, você pode armazenar esses valores com segurança na guia **[Configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** na portal do Azure. Para obter informações relacionadas, consulte o tópico de Rick Anderson intitulado [práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.net e no Azure](/aspnet/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).
5. Configure a classe `SmsService` no arquivo de *\_Start\IdentityConfig.cs do aplicativo* fazendo as seguintes alterações realçadas em amarelo: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample2.cs?highlight=5-17)]
6. Adicione as seguintes instruções `using` ao início do arquivo *IdentityConfig.cs* : 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample3.cs?highlight=1-4)]
7. Atualize o arquivo de *conta/gerenciamento. aspx* removendo as linhas realçadas em amarelo:  

    [!code-aspx[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample4.aspx?highlight=38,53,57-60,63,66,70,73)]
8. No manipulador de `Page_Load` do code-behind *Manage.aspx.cs* , remova a marca de comentário da linha de código realçada em amarelo para que ela apareça da seguinte maneira: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample5.cs?highlight=8)]
9. No codebehind da *conta*/*TwoFactorAuthenticationSignIn.aspx.cs*, atualize o manipulador de `Page_Load` adicionando o seguinte código realçado em amarelo: 

    [!code-csharp[Main](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/samples/sample6.cs?highlight=3-4,13)]

   Ao fazer a alteração do código acima, a DropDownList de "provedores" que contém as opções de autenticação não será redefinida para o primeiro valor. Isso permitirá que o usuário selecione com êxito todas as opções a serem usadas durante a autenticação, não apenas a primeira.
10. Em **Gerenciador de soluções**, clique com o botão direito do mouse em *Default. aspx* e selecione **definir como página inicial**.
11. Testando seu aplicativo, primeiro compile o aplicativo (**Ctrl**+**Shift**+**B**) e, em seguida, execute o aplicativo (**F5**) e selecione **registrar** para criar uma nova conta de usuário ou selecione **fazer logon** se a conta de usuário já tiver sido registrada.
12. Depois que você (como o usuário) tiver feito logon, clique na ID de usuário (endereço de email) na barra de navegação para exibir a página **gerenciar conta** (Manage. aspx).  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image2.png)
13. Clique em **Adicionar** ao lado de **número de telefone** na página **gerenciar conta** .  
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image3.png)
14. Adicione o número de telefone onde você (como o usuário) gostaria de receber mensagens SMS (mensagens de texto) e clique no botão **Enviar** .   
    ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image4.png)  
    Neste ponto, o aplicativo usará as credenciais do *Web. config* para entrar em contato com o twilio. Uma mensagem SMS (mensagem de texto) será enviada ao telefone associado à conta de usuário. Você pode verificar se a mensagem twilio foi enviada exibindo o painel do twilio.
15. Em alguns segundos, o telefone associado à conta de usuário receberá uma mensagem de texto contendo o código de verificação. Insira o código de verificação e pressione **Enviar**.  
     ![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image5.png)

<a id="use2FA"></a>
## <a name="enable-two-factor-authentication-for-a-registered-user"></a>Habilitar a autenticação de dois fatores para um usuário registrado

Neste ponto, você habilitou a autenticação de dois fatores para seu aplicativo. Para que um usuário use a autenticação de dois fatores, ele pode simplesmente alterar suas configurações usando a interface do usuário. 

1. Como usuário de seu aplicativo, você pode habilitar a autenticação de dois fatores para sua conta específica clicando na ID de usuário (alias de email) na barra de navegação para exibir a página **gerenciar conta** . Em seguida, clique no link **habilitar** para habilitar a autenticação de dois fatores para a conta.![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image6.png)
2. Faça logoff e, em seguida, faça logon novamente. Se você tiver habilitado o email, poderá selecionar SMS ou email para autenticação de dois fatores. Se você não tiver habilitado o email, confira o tutorial [criar um aplicativo Secure ASP.NET Web Forms com registro de usuário, confirmação de email e redefinição de senha](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset.md).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image7.png)
3. A página de autenticação de dois fatores é exibida onde você pode inserir o código (de SMS ou email).![](create-an-aspnet-web-forms-app-with-sms-two-factor-authentication/_static/image8.png)  
 Clicar na caixa de seleção **lembrar este navegador** lhe isentará de precisar usar a autenticação de dois fatores para fazer logon ao usar o navegador e o dispositivo em que você marcou a caixa. Desde que os usuários mal-intencionados não possam obter acesso ao seu dispositivo, permitindo a autenticação de dois fatores e clicando no **Lembre-se de que este navegador** fornecerá um acesso de senha de uma etapa conveniente e, ao mesmo tempo, manterá uma forte proteção de autenticação de dois fatores para todo o acesso de dispositivos não confiáveis. Você pode fazer isso em qualquer dispositivo privado que você usa regularmente.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Autenticação de dois fatores usando SMS e email com a Identidade do ASP.NET](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md)
- [Links para ASP.NET Identity recursos recomendados](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Implantar um aplicativo ASP.NET Web Forms seguro com associação, OAuth e banco de dados SQL em um site do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [Série de tutoriais do ASP.NET Web Forms – adicionar um provedor OAuth 2,0](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Série de tutoriais do ASP.NET Web Forms – habilitar o SSL para o projeto](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
- [Confirmação de conta e recuperação de senha com ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Criando o aplicativo no Facebook e conectando o aplicativo ao projeto](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
