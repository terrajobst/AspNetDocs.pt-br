---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Proteger uma API Web com contas individuais e logon local no ASP.NET Web API 2,2 | Microsoft Docs
author: MikeWasson
description: Este tópico mostra como proteger uma API Web usando o OAuth2 para se autenticar em um banco de dados de associação. Versões de software usadas no tutorial Visual Studio 201...
ms.author: riande
ms.date: 10/15/2014
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7492c4aa4c2a0a8aeed64c3462bda8fc51f35a6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555191"
---
# <a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Proteger uma API Web com contas individuais e logon local no ASP.NET Web API 2,2

por [Mike Wasson](https://github.com/MikeWasson)

[Baixar aplicativo de exemplo](https://github.com/MikeWasson/LocalAccountsApp)

> Este tópico mostra como proteger uma API Web usando o OAuth2 para se autenticar em um banco de dados de associação.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Visual Studio 2013 Atualização 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [API Web 2,2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2,1](../../../identity/index.md)

No Visual Studio 2013, o modelo de projeto de API Web oferece três opções de autenticação:

- **Contas individuais.** O aplicativo usa um banco de dados de associação.
- **Contas organizacionais.** Os usuários entram com suas credenciais Azure Active Directory, Office 365 ou local Active Directory.
- **Autenticação do Windows.** Essa opção destina-se a aplicativos de intranet e usa o módulo IIS de autenticação do Windows.

Para obter mais detalhes sobre essas opções, consulte [Creating ASP.NET Web Projects in Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

As contas individuais fornecem duas maneiras para um usuário fazer logon:

- **Logon local**. O usuário se registra no site, inserindo um nome de usuário e senha. O aplicativo armazena o hash de senha no banco de dados de associação. Quando o usuário faz logon, o sistema de ASP.NET Identity verifica a senha.
- **Logon social**. O usuário entra com um serviço externo, como Facebook, Microsoft ou Google. O aplicativo ainda cria uma entrada para o usuário no banco de dados de associação, mas não armazena nenhuma credencial. O usuário é autenticado entrando no serviço externo.

Este artigo analisa o cenário de logon local. Para logon local e social, a API da Web usa OAuth2 para autenticar solicitações. No entanto, os fluxos de credenciais são diferentes para logon local e social.

Neste artigo, demonstrarei um aplicativo simples que permite que o usuário faça logon e envie chamadas do AJAX autenticadas para uma API da Web. Você pode baixar o código de exemplo [aqui](https://github.com/MikeWasson/LocalAccountsApp). O Leiame descreve como criar o exemplo do zero no Visual Studio.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

O aplicativo de exemplo usa knockout. js para vinculação de dados e jQuery para enviar solicitações AJAX. Vou me concentrar nas chamadas AJAX, portanto, você não precisa conhecer knockout. js para este artigo.

Ao longo do caminho, descreverei:

- O que o aplicativo está fazendo no lado do cliente.
- O que está acontecendo no servidor.
- O tráfego HTTP no meio.

Primeiro, precisamos definir uma terminologia OAuth2.

- *Recurso*. Parte dos dados que podem ser protegidos.
- *Servidor de recursos*. O servidor que hospeda o recurso.
- *Proprietário do recurso*. A entidade que pode conceder permissão para acessar um recurso. (Normalmente, o usuário.)
- *Cliente*: o aplicativo que deseja acessar o recurso. Neste artigo, o cliente é um navegador da Web.
- *Token de acesso*. Um token que concede acesso a um recurso.
- *Token de portador*. Um tipo específico de token de acesso, com a propriedade que qualquer pessoa pode usar o token. Em outras palavras, um cliente não precisa de uma chave de criptografia ou outro segredo para usar um token de portador. Por esse motivo, os tokens de portador só devem ser usados em um HTTPS e devem ter tempos de expiração relativamente curtos.
- *Servidor de autorização*. Um servidor que fornece tokens de acesso.

Um aplicativo pode atuar como servidor de autorização e servidor de recursos. O modelo de projeto de API Web segue esse padrão.

## <a name="local-login-credential-flow"></a>Fluxo de credenciais de logon local

Para o logon local, a API da Web usa o [fluxo de senha do proprietário do recurso](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) definido em OAuth2.

1. O usuário insere um nome e uma senha no cliente.
2. O cliente envia essas credenciais ao servidor de autorização.
3. O servidor de autorização autentica as credenciais e retorna um token de acesso.
4. Para acessar um recurso protegido, o cliente inclui o token de acesso no cabeçalho de autorização da solicitação HTTP.

![](individual-accounts-in-web-api/_static/image3.png)

Quando você seleciona **contas individuais** no modelo de projeto de API Web, o projeto inclui um servidor de autorização que valida as credenciais do usuário e os tokens de problemas. O diagrama a seguir mostra o mesmo fluxo de credenciais em termos de componentes da API Web.

![](individual-accounts-in-web-api/_static/image4.png)

Nesse cenário, os controladores de API Web atuam como servidores de recursos. Um filtro de autenticação valida tokens de acesso e o atributo **[autorizar]** é usado para proteger um recurso. Quando um controlador ou uma ação tem o atributo **[autorizar]** , todas as solicitações para esse controlador ou ação devem ser autenticadas. Caso contrário, a autorização é negada e a API da Web retorna um erro 401 (não autorizado).

O servidor de autorização e o filtro de autenticação chamam um componente de [middleware OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) que manipula os detalhes de OAuth2. Descreverei o design em mais detalhes posteriormente neste tutorial.

## <a name="sending-an-unauthorized-request"></a>Enviando uma solicitação não autorizada

Para começar, execute o aplicativo e clique no botão **chamar API** . Quando a solicitação for concluída, você deverá ver uma mensagem de erro na caixa **resultado** . Isso ocorre porque a solicitação não contém um token de acesso, portanto, a solicitação não é autorizada.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

O botão **chamar API** envia uma solicitação Ajax para ~/API/Values, que invoca uma ação do controlador da API Web. Aqui está a seção do código JavaScript que envia a solicitação AJAX. No aplicativo de exemplo, todo o código do aplicativo JavaScript está localizado no arquivo Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Até que o usuário faça logon, não há nenhum token de portador e, portanto, nenhum cabeçalho de autorização na solicitação. Isso faz com que a solicitação retorne um erro 401.

Aqui está a solicitação HTTP. (Usei o [Fiddler](http://www.telerik.com/fiddler) para capturar o tráfego http.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

Resposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Observe que a resposta inclui um cabeçalho WWW-Authenticate com o desafio definido como portador. Isso indica que o servidor espera um token de portador.

## <a name="register-a-user"></a>Registrar um usuário

Na seção **registrar** do aplicativo, insira um email e uma senha e clique no botão **registrar** .

Você não precisa usar um endereço de email válido para este exemplo, mas um aplicativo real confirmaria o endereço. (Consulte [criar um aplicativo Web secure ASP.NET MVC 5 com logon, confirmação de email e redefinição de senha](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) Para a senha, use algo como "password1!", com letras maiúsculas, letras minúsculas, números e caracteres não alfanuméricos. Para manter o aplicativo simples, deixei a validação do lado do cliente, portanto, se houver um problema com o formato de senha, você receberá um erro de 400 (solicitação inadequada).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

O botão **registrar** envia uma solicitação post para ~/API/Account/Register/. O corpo da solicitação é um objeto JSON que contém o nome e a senha. Este é o código JavaScript que envia a solicitação:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

Solicitação HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

Resposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

Essa solicitação é tratada pela classe `AccountController`. Internamente, o `AccountController` usa ASP.NET Identity para gerenciar o banco de dados de associação.

Se você executar o aplicativo localmente do Visual Studio, as contas de usuário serão armazenadas no LocalDB, na tabela AspNetUsers. Para exibir as tabelas no Visual Studio, clique no menu **Exibir** , selecione **Gerenciador de servidores**e, em seguida, expanda **conexões de dados**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Obter um token de acesso

Até agora, não fizemos nenhum OAuth, mas agora veremos o servidor de autorização OAuth em ação, quando solicitamos um token de acesso. Na área de **logon** do aplicativo de exemplo, insira o email e a senha e clique em **fazer logon**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

O botão **fazer logon** envia uma solicitação para o ponto de extremidade do token. O corpo da solicitação contém os seguintes dados codificados de URL de formulário:

- tipo de\_de concessão: "senha"
- nome de usuário: &lt;&gt; de email do usuário
- senha: &lt;senha&gt;

Este é o código JavaScript que envia a solicitação AJAX:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Se a solicitação for realizada com sucesso, o servidor de autorização retornará um token de acesso no corpo da resposta. Observe que armazenamos o token no armazenamento de sessão, para uso posterior ao enviar solicitações para a API. Ao contrário de algumas formas de autenticação (como autenticação baseada em cookie), o navegador não incluirá automaticamente o token de acesso em solicitações subsequentes. O aplicativo deve fazer isso explicitamente. Isso é algo bom, pois limita as [vulnerabilidades de CSRF](preventing-cross-site-request-forgery-csrf-attacks.md).

Solicitação HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

Você pode ver que a solicitação contém as credenciais do usuário. Você *deve* usar HTTPS para fornecer segurança de camada de transporte.

Resposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Para facilitar a leitura, eu recuo o JSON e trunca o token de acesso, que é um pouco longo.

As propriedades `access_token`, `token_type`e `expires_in` são definidas pela especificação OAuth2. As outras propriedades (`userName`, `.issued`e `.expires`) são apenas para fins informativos. Você pode encontrar o código que adiciona essas propriedades adicionais no método `TokenEndpoint`, no arquivo/Providers/ApplicationOAuthProvider.cs.

## <a name="send-an-authenticated-request"></a>Enviar uma solicitação autenticada

Agora que temos um token de portador, podemos fazer uma solicitação autenticada para a API. Isso é feito definindo o cabeçalho de autorização na solicitação. Clique no botão **chamar API** novamente para ver isso.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

Solicitação HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

Resposta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Logoff

Como o navegador não armazena em cache as credenciais ou o token de acesso, o logout é simplesmente uma questão de "esquecer" o token, removendo-o do armazenamento de sessão:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Noções básicas sobre o modelo de projeto de contas individuais

Quando você seleciona **contas individuais** no modelo de projeto de aplicativo Web ASP.net, o projeto inclui:

- Um servidor de autorização OAuth2.
- Um ponto de extremidade da API Web para gerenciar contas de usuário
- Um modelo do EF para armazenar contas de usuário.

Aqui estão as principais classes de aplicativo que implementam esses recursos:

- `AccountController`. Fornece um ponto de extremidade de API Web para gerenciar contas de usuário. A ação `Register` é a única que usamos neste tutorial. Outros métodos na classe dão suporte à redefinição de senha, aos logons sociais e a outras funcionalidades.
- `ApplicationUser`, definido em/Models/IdentityModels.cs. Essa classe é o modelo de EF para contas de usuário no banco de dados de associação.
- `ApplicationUserManager`, definido em/app\_Start/IdentityConfig. cs, essa classe deriva de [usermanager](https://msdn.microsoft.com/library/dn613290.aspx) e executa operações em contas de usuário, como criar um novo usuário, verificar senhas e assim por diante, e persistir automaticamente as alterações no banco de dados.
- `ApplicationOAuthProvider`. Esse objeto é conectado ao middleware OWIN e processa eventos gerados pelo middleware. Ele deriva de [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Configurando o servidor de autorização

No StartupAuth.cs, o código a seguir configura o servidor de autorização OAuth2.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

A propriedade `TokenEndpointPath` é o caminho da URL para o ponto de extremidade do servidor de autorização. Essa é a URL que o aplicativo usa para obter os tokens de portador.

A propriedade `Provider` especifica um provedor que se conecta ao middleware OWIN e processa eventos gerados pelo middleware.

Este é o fluxo básico quando o aplicativo deseja obter um token:

1. Para obter um token de acesso, o aplicativo envia uma solicitação para ~/token.
2. O middleware OAuth chama `GrantResourceOwnerCredentials` no provedor.
3. O provedor chama o `ApplicationUserManager` para validar as credenciais e criar uma identidade de declarações.
4. Se isso for executado com sucesso, o provedor criará um tíquete de autenticação, que é usado para gerar o token.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

O middleware OAuth não sabe nada sobre as contas de usuário. O provedor se comunica entre o middleware e o ASP.NET Identity. Para obter mais informações sobre como implementar o servidor de autorização, consulte [servidor de autorização OWIN OAuth 2,0](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Configurando a API Web para usar tokens de portador

No método `WebApiConfig.Register`, o código a seguir configura a autenticação para o pipeline da API Web:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

A classe **HostAuthenticationFilter** habilita a autenticação usando tokens de portador.

O método **SuppressDefaultHostAuthentication** informa à API da Web para ignorar qualquer autenticação que ocorra antes que a solicitação alcance o pipeline da API Web, seja pelo IIS ou pelo middleware OWIN. Dessa forma, podemos restringir a API da Web para autenticar somente usando tokens de portador.

> [!NOTE]
> Em particular, a parte MVC do seu aplicativo pode usar a autenticação de formulários, que armazena as credenciais em um cookie. A autenticação baseada em cookie requer o uso de tokens antifalsificação, para evitar ataques de CSRF. Isso é um problema para APIs Web, porque não há uma maneira conveniente para a API da Web enviar o token antifalsificação ao cliente. (Para obter mais informações sobre esse problema, consulte [impedindo ataques CSRF na API da Web](preventing-cross-site-request-forgery-csrf-attacks.md).) Chamar **SuppressDefaultHostAuthentication** garante que a API da Web não seja vulnerável a ataques de CSRF de credenciais armazenadas em cookies.

Quando o cliente solicita um recurso protegido, aqui está o que acontece no pipeline da API Web:

1. O filtro **HostAuthentication** chama o middleware OAuth para validar o token.
2. O middleware converte o token em uma identidade de declarações.
3. Neste ponto, a solicitação é *autenticada* , mas não *autorizada*.
4. O filtro de autorização examina a identidade de declarações. Se as declarações autorizarem o usuário para esse recurso, a solicitação será autorizada. Por padrão, o atributo **[autorizar]** autorizará qualquer solicitação autenticada. No entanto, você pode autorizar por função ou por outras declarações. Para obter mais informações, consulte [autenticação e autorização na API da Web](authentication-and-authorization-in-aspnet-web-api.md).
5. Se as etapas anteriores forem bem-sucedidas, o controlador retornará o recurso protegido. Caso contrário, o cliente receberá um erro 401 (não autorizado).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Recursos adicionais

- [ASP.NET Identity](../../../identity/index.md)
- [Noções básicas sobre recursos de segurança no modelo Spa para VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). Postagem no blog do MSDN de Hongye Sun.
- Desapresentando [o modelo de contas individuais da API Web – parte 2: contas locais](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Postagem de blog de Dominick Baier.
- [Autenticação de host e API Web com OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Uma boa explicação de `SuppressDefaultHostAuthentication` e `HostAuthenticationFilter` pelo Brock Allen.
- [Personalizando informações de perfil em ASP.net Identity em modelos VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). Postagem no blog do MSDN de Pranav Rastogi.
- [Por solicitação de gerenciamento de tempo de vida para a classe usermanager no ASP.net Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). Postagem no blog do MSDN de Suhas Joshi, com uma boa explicação da classe `UserManager`.
