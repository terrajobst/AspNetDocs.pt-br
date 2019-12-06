---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Criar um aplicativo Web Secure ASP.NET MVC 5 com logon, confirmação de email e redefinição deC#senha () | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como criar um aplicativo Web ASP.NET MVC 5 com confirmação de email e redefinição de senha usando o sistema de associação ASP.NET Identity. Sua autoridade de certificação...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 07f5b290b73f75000e6f29fe09e4dc25e144452f
ms.sourcegitcommit: 969e7db924ebad3cc0f0cb0d65d148e8b9221b9a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74899703"
---
# <a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a>Criar um aplicativo Web seguro do ASP.NET MVC 5 com logon, confirmação por email e redefinição de senha (C#)

por [Rick Anderson]((https://twitter.com/RickAndMSFT))

Este tutorial mostra como criar um aplicativo Web ASP.NET MVC 5 com confirmação de email e redefinição de senha usando o sistema de associação ASP.NET Identity.

Para obter uma versão atualizada deste tutorial que usa o .NET Core, consulte [confirmação da conta e recuperação de senha no ASP.NET Core [/ASPNET/Core/Security/Authentication/accconfirm).

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Criar um aplicativo MVC do ASP.NET

Comece Instalando e executando o [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instale [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior.

> [!NOTE]
> Aviso: você deve instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior para concluir este tutorial.

1. Crie um novo projeto Web ASP.NET e selecione o modelo MVC. Web Forms também dá suporte a ASP.NET Identity, portanto, você pode seguir etapas semelhantes em um aplicativo Web Forms.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. Deixe a autenticação padrão como **contas de usuário individuais**. Se você quiser hospedar o aplicativo no Azure, deixe a caixa de seleção marcada. Posteriormente, no tutorial, iremos implantar no Azure. Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Defina o [projeto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. Execute o aplicativo, clique no link **registrar** e registre um usuário. Neste ponto, a única validação no email é com o atributo [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) .
5. Em Gerenciador de Servidores, navegue até **Data Connections\DefaultConnection\Tables\AspNetUsers**, clique com o botão direito do mouse e selecione **Abrir definição de tabela**.

    A imagem a seguir mostra o esquema de `AspNetUsers`:

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. Clique com o botão direito do mouse na tabela **AspNetUsers** e selecione **Mostrar dados da tabela**.  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 Neste ponto, o email não foi confirmado.
7. Clique na linha e selecione Excluir. Você adicionará esse email novamente na próxima etapa e enviará um email de confirmação.

## <a name="email-confirmation"></a>Confirmação de email

É uma prática recomendada confirmar o email de um novo registro de usuário para verificar se eles não estão representando outra pessoa (ou seja, se eles não se registraram no email de outra pessoa). Suponha que você tenha um fórum de discussão, você desejaria impedir que `"bob@example.com"` se registrem como `"joe@contoso.com"`. Sem confirmação por email, `"joe@contoso.com"` pode obter email indesejado de seu aplicativo. Suponha que Bob seja acidentalmente registrado como `"bib@example.com"` e não tenha notado, ele não seria capaz de usar a recuperação de senha porque o aplicativo não tem seu email correto. A confirmação por email fornece apenas proteção limitada de bots e não fornece proteção contra remetentes de spam determinados, eles têm muitos aliases de email de trabalho que podem usar para se registrar.

Em geral, você deseja impedir que novos usuários enviem dados para seu site antes de terem sido confirmados por email, uma mensagem de texto SMS ou outro mecanismo. <a id="build"></a>Nas seções a seguir, Habilitaremos a confirmação de email e modificaremos o código para impedir que usuários registrados recentemente façam logon até que seu email seja confirmado.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Conectar SendGrid

As instruções nesta seção não são atuais. Consulte [Configurar provedor de email do SendGrid](/aspnet/core/security/authentication/accconfirm#configure-email-provider) para obter instruções atualizadas.

Embora este tutorial mostre apenas como adicionar notificação por email por meio do [SendGrid](http://sendgrid.com/), você pode enviar emails usando SMTP e outros mecanismos (consulte [recursos adicionais](#addRes)).

1. No Console do Gerenciador de Pacotes, digite o seguinte comando: 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. Vá para a [página de inscrição do SendGrid do Azure](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) e registre-se para uma conta gratuita do SendGrid. Configure o SendGrid adicionando um código semelhante ao seguinte no *App_Start/identityconfig.cs*:

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

Você precisará adicionar o seguinte:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

Para manter esse exemplo simples, armazenaremos as configurações do aplicativo no arquivo *Web. config* :

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> Segurança-nunca armazene dados confidenciais em seu código-fonte. A conta e as credenciais são armazenadas em appSetting. No Azure, você pode armazenar esses valores com segurança na guia **[Configurar](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** na portal do Azure. Consulte [as práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.net e no Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).

### <a name="enable-email-confirmation-in-the-account-controller"></a>Habilitar confirmação de email no controlador de conta

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

Verifique se o arquivo *Views\Account\ConfirmEmail.cshtml* tem a sintaxe correta do Razor. (O caractere @ na primeira linha pode estar ausente. )

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

Execute o aplicativo e clique no link registrar. Depois de enviar o formulário de registro, você está conectado.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

Verifique sua conta de email e clique no link para confirmar seu email.

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>Exigir confirmação de email antes de fazer logon

Atualmente, quando um usuário conclui o formulário de registro, ele está conectado. Em geral, você deseja confirmar seu email antes de fazer logon. Na seção abaixo, modificaremos o código para exigir que novos usuários tenham um email confirmado antes de serem conectados (autenticados). Atualize o método `HttpPost Register` com as seguintes alterações realçadas:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

Ao comentar o método `SignInAsync`, o usuário não será conectado pelo registro. A linha de `TempData["ViewBagLink"] = callbackUrl;` pode ser usada para [depurar o aplicativo e o registro de](#dbg) teste sem enviar email. `ViewBag.Message` é usado para exibir as instruções de confirmação. O [exemplo de download](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contém código para testar a confirmação de email sem configurar o email e também pode ser usado para depurar o aplicativo.

Crie um arquivo de `Views\Shared\Info.cshtml` e adicione a seguinte marcação Razor:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

Adicione o [atributo Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) ao método de ação `Contact` do controlador Home. Você pode clicar no link de **contato** para verificar se os usuários anônimos não têm acesso e os usuários autenticados têm o Access.

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

Você também deve atualizar o método de ação `HttpPost Login`:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

Atualize a exibição *Views\Shared\Error.cshtml* para exibir a mensagem de erro:

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

Exclua todas as contas na tabela **AspNetUsers** que contêm o alias de email com o qual você deseja testar. Execute o aplicativo e verifique se não é possível fazer logon até que você tenha confirmado seu endereço de email. Depois de confirmar seu endereço de email, clique no link de **contato** .

<a id="reset"></a>
## <a name="password-recoveryreset"></a>Recuperação/redefinição de senha

Remova os caracteres de comentário do método de ação `HttpPost ForgotPassword` no controlador de conta:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

Remova os caracteres de comentário do `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) no arquivo de exibição do Razor do *Views\Account\Login.cshtml* :

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

Agora, a página de logon mostrará um link para redefinir a senha.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Link reenviar confirmação de email

Quando um usuário cria uma nova conta local, ele envia por email um link de confirmação que eles precisam usar antes que possam fazer logon. Se o usuário excluir acidentalmente o email de confirmação ou se o email nunca chegar, ele precisará do link de confirmação enviado novamente. As alterações de código a seguir mostram como habilitar isso.

Adicione o seguinte método auxiliar à parte inferior do arquivo *Controllers\AccountController.cs* :

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

Atualize o método Register para usar o novo auxiliar:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

Atualize o método de logon para reenviar a senha se a conta de usuário não tiver sido confirmada:

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a>Combinar contas de logon sociais e locais

Você pode combinar contas locais e sociais clicando no link de email. Na sequência a seguir **RickAndMSFT@gmail.com** é criado pela primeira vez como um logon local, mas você pode criar a conta como um log social primeiro e, em seguida, adicionar um logon local.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

Clique no link **gerenciar** . Observe os **logons externos: 0** associados a esta conta.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

Clique no link para outro serviço de logon e aceite as solicitações do aplicativo. As duas contas foram combinadas, você poderá fazer logon com qualquer uma das contas. Talvez você queira que os usuários adicionem contas locais caso o serviço de autenticação de seu log social esteja inativo ou, provavelmente, tenha perdido o acesso à sua conta social.

Na imagem a seguir, José é um logon social (que pode ser visto nos **logons externos: 1** mostrado na página).

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

Clicar em **escolher uma senha** permite que você adicione um log local associado à mesma conta.

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a>Confirmação de email em mais profundidade

Minha [confirmação de conta do tutorial e recuperação de senha com ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) entrará neste tópico com mais detalhes.

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Depurando o aplicativo

Se você não receber um email contendo o link:

- Verifique sua pasta de lixo eletrônico ou spam.
- Faça logon em sua conta do SendGrid e clique no [link atividade de email](https://sendgrid.com/logs/index).

Para testar o link de verificação sem email, baixe o [exemplo concluído](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). O link de confirmação e os códigos de confirmação serão exibidos na página.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Links para ASP.NET Identity recursos recomendados](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Confirmação de conta e recuperação de senha com ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Apresenta mais detalhes sobre a recuperação de senha e a confirmação da conta.
- [Aplicativo MVC 5 com logon do Facebook, Twitter, LinkedIn e Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Este tutorial mostra como escrever um aplicativo ASP.NET MVC 5 com a autorização do Facebook e do Google OAuth 2. Ele também mostra como adicionar dados adicionais ao banco de dado de identidade.
- [Implante um aplicativo MVC do ASP.net seguro com associação, OAuth e banco de dados SQL no Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Este tutorial adiciona a implantação do Azure, como proteger seu aplicativo com funções, como usar a API de associação para adicionar usuários e funções e recursos de segurança adicionais.
- [Criando um aplicativo do Google para OAuth 2 e conectando o aplicativo ao projeto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Criando o aplicativo no Facebook e conectando o aplicativo ao projeto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configurando o SSL no projeto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
