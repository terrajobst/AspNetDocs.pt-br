---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Aplicativo ASP.NET MVC 5 com SMS e email autenticação de dois fatores | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como criar um aplicativo Web ASP.NET MVC 5 com autenticação de dois fatores. Você deve concluir a criação de um aplicativo Web ASP.NET MVC 5 com...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: c14149d802bfc0a227a839a2981dc3e8a3849c25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78538440"
---
# <a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a>Aplicativo do ASP.NET MVC 5 com autenticação de dois fatores por SMS e email

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial mostra como criar um aplicativo Web ASP.NET MVC 5 com autenticação de dois fatores. Você deve concluir [a criação de um aplicativo Web secure ASP.NET MVC 5 com logon, confirmação de email e redefinição de senha](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de continuar. Você pode baixar o aplicativo concluído [aqui](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952). O download contém auxiliares de depuração que permitem testar a confirmação de email e o SMS sem configurar um email ou provedor de SMS.
> 
> Este tutorial foi escrito por [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).

- [Criar um aplicativo MVC do ASP.NET](#createMvc)
- [Configurar o SMS para autenticação de dois fatores](#SMS)
- [Habilitar a autenticação de dois fatores](#enable2)
- [Recursos adicionais](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a>Criar um aplicativo MVC do ASP.NET

Comece Instalando e executando o [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instale [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior.

> [!NOTE]
> Aviso: você deve concluir [a criação de um aplicativo web ASP.NET MVC 5 com logon, confirmação de email e redefinição de senha](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) antes de continuar. Você deve instalar [Visual Studio 2013 atualização 3](https://go.microsoft.com/fwlink/?LinkId=390465) ou superior para concluir este tutorial.

1. Crie um novo projeto Web ASP.NET e selecione o modelo MVC. Web Forms também dá suporte a ASP.NET Identity, portanto, você pode seguir etapas semelhantes em um aplicativo Web Forms.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. Deixe a autenticação padrão como **contas de usuário individuais**. Se você quiser hospedar o aplicativo no Azure, deixe a caixa de seleção marcada. Posteriormente, no tutorial, iremos implantar no Azure. Você pode [abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
3. Defina o [projeto para usar SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a>Configurar o SMS para autenticação de dois fatores

Este tutorial fornece instruções para usar o twilio ou o ASPSMS, mas você pode usar qualquer outro provedor de SMS.

1. **Criando uma conta de usuário com um provedor de SMS**  
  
   Crie uma conta do [twilio](https://www.twilio.com/try-twilio) ou [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) .
2. **Instalando pacotes adicionais ou adicionando referências de serviço**  
  
   Twilio  
   No Console do Gerenciador de Pacotes, digite o seguinte comando:  
    `Install-Package Twilio`  
  
   ASPSMS:  
   A seguinte referência de serviço precisa ser adicionada:  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   Endereço:  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   Namespace:  
    `ASPSMSX2`
3. **Descobrindo credenciais de usuário do provedor de SMS**  
  
   Twilio  
   Na guia **painel** da sua conta do twilio, copie o **SID da conta** e o **token de autenticação**.  
  
   ASPSMS:  
   Em suas configurações de conta, navegue até **userKey** e copie-o junto com sua **senha**definida automaticamente.  
  
   Mais tarde, armazenaremos esses valores no arquivo *Web. config* dentro das chaves `"SMSAccountIdentification"` e `"SMSAccountPassword"`.
4. **Especificando SenderId/originador**  
  
   Twilio  
   Na guia **números** , copie o número de telefone twilio.  
  
   ASPSMS:  
   No menu **desbloquear originadores** , desbloqueie um ou mais originadores ou escolha um originador alfanumérico (sem suporte em todas as redes).  
  
   Mais tarde, armazenaremos esse valor no arquivo *Web. config* dentro da chave `"SMSAccountFrom"`.
5. **Transferindo credenciais do provedor de SMS para o aplicativo**  
  
   Torne as credenciais e o número de telefone do remetente disponíveis para o aplicativo. Para manter as coisas simples, armazenaremos esses valores no arquivo *Web. config* . Quando implantamos no Azure, podemos armazenar os valores com segurança na seção **configurações do aplicativo** na guia Configurar do site. 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > Segurança-nunca armazene dados confidenciais em seu código-fonte. A conta e as credenciais são adicionadas ao código acima para manter o exemplo simples. Consulte [as práticas recomendadas para implantar senhas e outros dados confidenciais no ASP.net e no Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
6. **Implementação de transferência de dados para o provedor de SMS**  
  
   Configure a classe `SmsService` no arquivo de *\_Start\IdentityConfig.cs do aplicativo* .  
  
   Dependendo do provedor de SMS usado, ative a seção **twilio** ou **ASPSMS** : 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. Atualize a exibição do Razor do *Views\Manage\Index.cshtml* : (Observação: não apenas remova os comentários no código de saída, use o código abaixo.)  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. Verifique se os métodos de ação `EnableTwoFactorAuthentication` e `DisableTwoFactorAuthentication` no `ManageController` têm o atributo[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) :  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. Execute o aplicativo e faça logon com a conta que você registrou anteriormente.
10. Clique em sua ID de usuário, que ativa o método de ação `Index` no controlador `Manage`.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. Clique em Adicionar.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. O método de ação `AddPhoneNumber` exibe uma caixa de diálogo para inserir um número de telefone que pode receber mensagens SMS.

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. Em alguns segundos, você receberá uma mensagem de texto com o código de verificação. Insira-o e pressione **Enviar**.  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. O modo de exibição gerenciar mostra seu número de telefone foi adicionado.

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a>Habilitar a autenticação de dois fatores

No aplicativo do modelo gerado, você precisa usar a interface do usuário para habilitar a autenticação de dois fatores (2FA). Para habilitar o 2FA, clique em sua ID de usuário (alias de email) na barra de navegação.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

Clique em Habilitar 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

Faça logoff e logon novamente. Se você habilitou o email (consulte meu [tutorial anterior](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), poderá selecionar o SMS ou email para 2FA.

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

A página verificar código é exibida onde você pode inserir o código (de SMS ou email).

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

Clicar na caixa de seleção **lembrar este navegador** lhe isentará de precisar usar o 2FA para fazer logon ao usar o navegador e o dispositivo em que você marcou a caixa. Desde que os usuários mal-intencionados não possam obter acesso ao seu dispositivo, habilitar o 2FA e clicar no **Lembre-se de que este navegador** fornecerá a você o acesso de senha de uma etapa conveniente e, ao mesmo tempo, manterá uma proteção de 2FA forte para todo o acesso de dispositivos não confiáveis. Você pode fazer isso em qualquer dispositivo privado que você usa regularmente.

Este tutorial fornece uma breve introdução à habilitação do 2FA em um novo aplicativo MVC ASP.NET. Meu tutorial de [autenticação de dois fatores usando SMS e email com ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) entra em detalhes sobre o código por trás do exemplo.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Autenticação de dois fatores usando SMS e email com ASP.net Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Entra em detalhes sobre a autenticação de dois fatores
- [Links para ASP.NET Identity recursos recomendados](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Confirmação de conta e recuperação de senha com ASP.net Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Apresenta mais detalhes sobre a recuperação de senha e a confirmação da conta.
- [Aplicativo MVC 5 com logon do Facebook, Twitter, LinkedIn e Google OAuth2](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Este tutorial mostra como escrever um aplicativo ASP.NET MVC 5 com a autorização do Facebook e do Google OAuth 2. Ele também mostra como adicionar dados adicionais ao banco de dado de identidade.
- [Implante um aplicativo MVC do ASP.net seguro com associação, OAuth e banco de dados SQL no Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Este tutorial adiciona a implantação do Azure, como proteger seu aplicativo com funções, como usar a API de associação para adicionar usuários e funções e recursos de segurança adicionais.
- [Criando um aplicativo do Google para OAuth 2 e conectando o aplicativo ao projeto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [Criando o aplicativo no Facebook e conectando o aplicativo ao projeto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [Configurando o SSL no projeto](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
- [Como configurar seu e o C# ambiente de desenvolvimento do ASP.NET MVC](https://www.twilio.com/docs/usage/tutorials/how-to-set-up-your-csharp-and-asp-net-mvc-development-environment)
