---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Autenticação de dois fatores usando SMS e email com ASP.NET Identity-ASP.NET 4. x
author: HaoK
description: Este tutorial mostrará como configurar a autenticação de dois fatores (2FA) usando o SMS e o email. Este artigo foi escrito por Rick Anderson (@RickAndMSFT), PR...
ms.author: riande
ms.date: 09/15/2015
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 5f5218ca6c65ed3a2cd39d4e100349efa35d14cd
ms.sourcegitcommit: 6f0e10e4ca61a1e5534b09c655fd35cdc6886c8a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2019
ms.locfileid: "74115096"
---
# <a name="two-factorauthentication-using-sms-and-email-with-aspnet-identity"></a>Autenticação de dois fatores usando SMS e email com ASP.NET Identity

por [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Suhas Joshi](https://github.com/suhasj)

> Este tutorial mostrará como configurar a autenticação de dois fatores (2FA) usando o SMS e o email.
> 
> Este artigo foi escrito por Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung e Suhas Joshi. O exemplo de NuGet foi escrito principalmente por Hao Kung.

Este tópico aborda o seguinte:

- [Criando o exemplo de identidade](#build)
- [Configurar o SMS para autenticação de dois fatores](#SMS)
- [Habilitar a autenticação de dois fatores](#enable2)
- [Como registrar um provedor de autenticação de dois fatores](#reg)
- [Combinar contas de logon sociais e locais](#combine)
- [Bloqueio de conta de ataques de força bruta](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a>Criando o exemplo de identidade

Nesta seção, você usará o NuGet para baixar um exemplo com o qual trabalharemos. Comece Instalando e executando o [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instale o Visual Studio [2013 atualização 2](https://go.microsoft.com/fwlink/?LinkId=390521) ou superior.

> [!NOTE]
> Aviso: você deve instalar o Visual Studio [2013 atualização 2](https://go.microsoft.com/fwlink/?LinkId=390521) para concluir este tutorial.

1. Crie um novo projeto Web ASP.NET ***vazio*** .
2. No console do Gerenciador de pacotes, insira os seguintes comandos:  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   Neste tutorial, usaremos [SendGrid](http://sendgrid.com/) para enviar email e [twilio](https://www.twilio.com/) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) para texto SMS. O pacote de `Identity.Samples` instala o código com o qual trabalharemos.
3. Defina o [projeto para usar SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
4. *Opcional*: siga as instruções em meu [tutorial de confirmação por email](account-confirmation-and-password-recovery-with-aspnet-identity.md) para conectar o SendGrid e executar o aplicativo e registrar uma conta de email.
5. *Opcional:* Remova o código de confirmação de link de email de demonstração do exemplo (o código de `ViewBag.Link` no controlador de conta. Consulte os métodos de ação `DisplayEmail` e `ForgotPasswordConfirmation` e exibições do Razor).
6. *Opcional:* Remova o código de `ViewBag.Status` dos controladores de gerenciamento e de conta e dos modos de exibição *Views\Account\VerifyCode.cshtml* e *Views\Manage\VerifyPhoneNumber.cshtml* Razor. Como alternativa, você pode manter o `ViewBag.Status` exibido para testar como esse aplicativo funciona localmente sem a necessidade de conectar e enviar mensagens de email e SMS.

> [!NOTE]
> Aviso: se você alterar qualquer uma das configurações de segurança neste exemplo, os aplicativos de produções precisarão passar por uma auditoria de segurança que chama explicitamente as alterações feitas.

<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a>Configurar o SMS para autenticação de dois fatores

Este tutorial fornece instruções para usar o twilio ou o ASPSMS, mas você pode usar qualquer outro provedor de SMS.

1. **Criando uma conta de usuário com um provedor de SMS**  
  
   Crie uma conta do [twilio](https://www.twilio.com/try-twilio) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) .
2. **Instalando pacotes adicionais ou adicionando referências de serviço**  
  
   Twilio  
   No console do Gerenciador de pacotes, digite o seguinte comando:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   A seguinte referência de serviço precisa ser adicionada:  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   Corrigir  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Namespace:  
    `ASPSMSX2`
3. **Descobrindo credenciais de usuário do provedor de SMS**  
  
   Twilio  
   Na guia **painel** da sua conta do twilio, copie o **SID da conta** e o **token de autenticação**.  
  
   ASPSMS:  
   Em suas configurações de conta, navegue até **userKey** e copie-o junto com sua **senha**definida automaticamente.  
  
   Mais tarde, armazenaremos esses valores nas variáveis `SMSAccountIdentification` e `SMSAccountPassword`.
4. **Especificando SenderId/originador**  
  
   Twilio  
   Na guia **números** , copie o número de telefone twilio.  
  
   ASPSMS:  
   No menu **desbloquear originadores** , desbloqueie um ou mais originadores ou escolha um originador alfanumérico (sem suporte em todas as redes).  
  
   Posteriormente, esse valor será armazenado na variável `SMSAccountFrom`.
5. **Transferindo credenciais do provedor de SMS para o aplicativo**  
  
   Torne as credenciais e o número de telefone do remetente disponíveis para o aplicativo:

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > Segurança-nunca armazene dados confidenciais em seu código-fonte. A conta e as credenciais são adicionadas ao código acima para manter o exemplo simples. Consulte o ASP.NET MVC de Jon Atten [: manter as configurações particulares do controle do código-fonte](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).
6. **Implementação de transferência de dados para o provedor de SMS**  
  
   Configure a classe `SmsService` no arquivo de *\_Start\IdentityConfig.cs do aplicativo* .  
  
   Dependendo do provedor de SMS usado, ative a seção **twilio** ou **ASPSMS** : 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. Execute o aplicativo e faça logon com a conta que você registrou anteriormente.
8. Clique em sua ID de usuário, que ativa o método de ação `Index` no controlador `Manage`.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. Clique em Adicionar.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. Em alguns segundos, você receberá uma mensagem de texto com o código de verificação. Insira-o e pressione **Enviar**.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. O modo de exibição gerenciar mostra seu número de telefone foi adicionado.  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a>Examinar o código

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

O método de ação `Index` no controlador `Manage` define a mensagem de status com base em sua ação anterior e fornece links para alterar sua senha local ou adicionar uma conta local. O método `Index` também exibe o estado ou seu número de telefone 2FA, logons externos, 2FA habilitado e lembra-se do método 2FA para este navegador (explicado posteriormente). Clicar em sua ID de usuário (email) na barra de título não passa por uma mensagem. Clicar no **número de telefone: remover** link passa `Message=RemovePhoneSuccess` como uma cadeia de caracteres de consulta.  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]

O método de ação `AddPhoneNumber` exibe uma caixa de diálogo para inserir um número de telefone que pode receber mensagens SMS.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

Clicar no botão **enviar código de verificação** posta o número de telefone para o método HTTP post `AddPhoneNumber` ação.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

O método `GenerateChangePhoneNumberTokenAsync` gera o token de segurança que será definido na mensagem SMS. Se o serviço SMS tiver sido configurado, o token será enviado como a cadeia de caracteres &quot;o seu código de segurança for &lt;token&gt;&quot;. O método `SmsService.SendAsync` para é chamado de forma assíncrona, então o aplicativo é redirecionado para o método de ação `VerifyPhoneNumber` (que exibe a caixa de diálogo a seguir), onde você pode inserir o código de verificação.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

Depois de inserir o código e clicar em enviar, o código será postado no método HTTP POST `VerifyPhoneNumber` ação.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

O método `ChangePhoneNumberAsync` verifica o código de segurança Postado. Se o código estiver correto, o número de telefone será adicionado ao campo `PhoneNumber` da tabela `AspNetUsers`. Se essa chamada for bem-sucedida, o método `SignInAsync` será chamado:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

O parâmetro `isPersistent` define se a sessão de autenticação é persistida em várias solicitações.

Quando você altera seu perfil de segurança, um novo carimbo de segurança é gerado e armazenado no campo `SecurityStamp` da tabela *AspNetUsers* . Observe que o campo `SecurityStamp` é diferente do cookie de segurança. O cookie de segurança não é armazenado na tabela de `AspNetUsers` (ou em qualquer outro lugar no BD de identidade). O token do cookie de segurança é autoassinado usando [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) e é criado com as informações de `UserId, SecurityStamp` e tempo de expiração.

O middleware do cookie verifica o cookie em cada solicitação. O método `SecurityStampValidator` na classe `Startup` atinge o DB e verifica o carimbo de segurança periodicamente, conforme especificado com o `validateInterval`. Isso ocorre apenas a cada 30 minutos (em nosso exemplo), a menos que você altere seu perfil de segurança. O intervalo de 30 minutos foi escolhido para minimizar as viagens no banco de dados.

O método `SignInAsync` precisa ser chamado quando qualquer alteração é feita no perfil de segurança. Quando o perfil de segurança é alterado, o banco de dados é atualizado o campo `SecurityStamp` e, sem chamar o método `SignInAsync`, você permaneceria conectado *somente* até a próxima vez que o pipeline OWIN atingir o banco de dados (o `validateInterval`). Você pode testar isso alterando o método `SignInAsync` para retornar imediatamente e definindo a propriedade cookie `validateInterval` de 30 minutos para 5 segundos:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

Com as alterações de código acima, você pode alterar seu perfil de segurança (por exemplo, alterando o estado de **dois fatores habilitados**) e você será desconectado em 5 segundos quando o método de `SecurityStampValidator.OnValidateIdentity` falhar. Remova a linha de retorno no método `SignInAsync`, faça outra alteração no perfil de segurança e você não será desconectado. O método `SignInAsync` gera um novo cookie de segurança.

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a>Habilitar a autenticação de dois fatores

No aplicativo de exemplo, você precisa usar a interface do usuário para habilitar a autenticação de dois fatores (2FA). Para habilitar o 2FA, clique em sua ID de usuário (alias de email) na barra de navegação.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)  
Clique em Habilitar 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png) Faça logoff e, em seguida, faça logon novamente. Se você habilitou o email (consulte meu [tutorial anterior](account-confirmation-and-password-recovery-with-aspnet-identity.md)), poderá selecionar o SMS ou email para 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png) A página verificar código é exibida onde você pode inserir o código (de SMS ou email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png) Clicar na caixa de seleção **lembrar este navegador** lhe isentará de precisar usar o 2FA para fazer logon com esse computador e navegador. Habilitar o 2FA e clicar no **Lembre-se de que este navegador** lhe dará proteção de 2FA forte contra usuários mal-intencionados que tentam acessar sua conta, desde que eles não tenham acesso ao seu computador. Você pode fazer isso em qualquer computador privado que usa regularmente. Ao configurar **lembrar este navegador**, você obtém a segurança adicional do 2FA de computadores que você não usa regularmente, e você tem a conveniência de não ter de passar pelo 2FA em seus próprios computadores. 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a>Como registrar um provedor de autenticação de dois fatores

Quando você cria um novo projeto MVC, o arquivo *IdentityConfig.cs* contém o seguinte código para registrar um provedor de autenticação de dois fatores:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a>Adicionar um número de telefone para 2FA

O método de ação `AddPhoneNumber` no controlador de `Manage` gera um token de segurança e o envia para o número de telefone que você forneceu.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

Depois de enviar o token, ele redireciona para o método de ação `VerifyPhoneNumber`, onde você pode inserir o código para registrar o SMS para 2FA. O 2FA de SMS não é usado até que você verifique o número de telefone.

## <a name="enabling-2fa"></a>Habilitando 2FA

O método de ação `EnableTFA` habilita o 2FA:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

Observe que o `SignInAsync` deve ser chamado porque Enable 2FA é uma alteração no perfil de segurança. Quando o 2FA estiver habilitado, o usuário precisará usar o 2FA para fazer logon, usando as abordagens de 2FA que eles registraram (SMS e email no exemplo).

Você pode adicionar mais provedores de 2FA, como geradores de código QR, ou você pode escrever seu próprio (consulte [usando o Google Authenticator com ASP.net Identity](https://www.jerriepelser.com//blog/using-google-authenticator-asp-net-identity/)).

> [!NOTE]
> Os códigos 2FA são gerados usando o algoritmo de senha de uso [único baseado em tempo](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) e os códigos são válidos por seis minutos. Se você levar mais de seis minutos para inserir o código, receberá uma mensagem de erro de código inválida.

<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a>Combinar contas de logon sociais e locais

Você pode combinar contas locais e sociais clicando no link de email. Na sequência a seguir &quot;RickAndMSFT@gmail.com&quot; é criado pela primeira vez como um logon local, mas você pode criar a conta como um log social primeiro e, em seguida, adicionar um logon local.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

Clique no link **gerenciar** . Observe o 0 externo (logons sociais) associado a essa conta.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

Clique no link para outro serviço de logon e aceite as solicitações do aplicativo. As duas contas foram combinadas, você poderá fazer logon com qualquer uma das contas. Talvez você queira que os usuários adicionem contas locais caso o serviço de autenticação de seu log social esteja inativo ou, provavelmente, tenha perdido o acesso à sua conta social.

Na imagem a seguir, José é um logon social (que pode ser visto nos **logons externos: 1** mostrado na página).

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

Clicar em **escolher uma senha** permite que você adicione um log local associado à mesma conta.

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a>Bloqueio de conta de ataques de força bruta

Você pode proteger as contas em seu aplicativo contra ataques de dicionário habilitando o bloqueio do usuário. O código a seguir no método `ApplicationUserManager Create` configura o bloqueio:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

O código acima habilita o bloqueio somente para autenticação de dois fatores. Embora você possa habilitar o bloqueio para logons alterando `shouldLockout` para true no método `Login` do controlador da conta, recomendamos que você não habilite o bloqueio para logons, pois ele torna a conta suscetível a ataques de logon do [dos](http://en.wikipedia.org/wiki/Denial-of-service_attack) . No código de exemplo, o bloqueio é desabilitado para a conta de administrador criada no método de `ApplicationDbInitializer Seed`:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a>Exigir que um usuário tenha uma conta de email validada

O código a seguir exige que um usuário tenha uma conta de email validada para que possa fazer logon:

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a>Como o SignInManager verifica o requisito de 2FA

O log local e o log social no verificam se o 2FA está habilitado. Se 2FA estiver habilitado, o método de logon `SignInManager` retornará `SignInStatus.RequiresVerification`, e o usuário será redirecionado para o método de ação `SendCode`, onde terá que inserir o código para concluir a sequência de logon. Se o usuário tiver RememberMe definido no cookie local dos usuários, o `SignInManager` retornará `SignInStatus.Success` e eles não precisarão passar pelo 2FA.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

O código a seguir mostra o `SendCode` método de ação. Um [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) é criado com todos os métodos 2FA habilitados para o usuário. O [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) é passado para o auxiliar do [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) , que permite ao usuário selecionar a abordagem 2FA (normalmente email e SMS).

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

Depois que o usuário envia a abordagem 2FA, o método de ação `HTTP POST SendCode` é chamado, o `SignInManager` envia o código 2FA e o usuário é redirecionado para o método de ação `VerifyCode`, onde pode inserir o código para concluir o logon.

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a>Bloqueio de 2FA

Embora você possa definir o bloqueio de conta em falhas de tentativa de senha de logon, essa abordagem torna seu logon suscetível a bloqueios de [dos](http://en.wikipedia.org/wiki/Denial-of-service_attack) . É recomendável usar o bloqueio de conta somente com 2FA. Quando o `ApplicationUserManager` é criado, o código de exemplo define o bloqueio 2FA e `MaxFailedAccessAttemptsBeforeLockout` como cinco. Quando um usuário faz logon (por meio de conta local ou conta social), cada tentativa com falha em 2FA é armazenada e, se o máximo de tentativas for atingido, o usuário será bloqueado por cinco minutos (você pode definir o tempo de bloqueio com `DefaultAccountLockoutTimeSpan`).

<a id="addRes"></a>

## <a name="additional-resources"></a>Recursos adicionais

- [ASP.net Identity recursos recomendados](../getting-started/aspnet-identity-recommended-resources.md) Lista completa de Blogs, vídeos, tutoriais e ótimos links de identidade.
- O [aplicativo MVC 5 com o logon do Facebook, Twitter, LinkedIn e Google OAuth2](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) também mostra como adicionar informações de perfil à tabela users.
- [ASP.NET MVC e identidade 2,0: Noções básicas de](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) John Atten.
- [Confirmação de conta e recuperação de senha com ASP.NET Identity](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [Introdução à Identidade do ASP.NET](../getting-started/introduction-to-aspnet-identity.md)
- [Anunciando a versão RTM do ASP.net Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.
- [ASP.NET Identity 2,0: Configurando a validação de conta e a autorização de dois fatores](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) por John Atten.
