---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Fazendo logon usando sites externos em um site Páginas da Web do ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Este artigo explica como fazer logon em seu site do Páginas da Web do ASP.NET (Razor) usando o Facebook, o Google, o Twitter, o Yahoo e outros sites, ou seja, como dar suporte a...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 860b75422c3df1d191ed861344963bfc19270e8f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638750"
---
# <a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Fazendo logon usando sites externos em um site Páginas da Web do ASP.NET (Razor)

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como fazer logon em seu site do Páginas da Web do ASP.NET (Razor) usando o Facebook, o Google, o Twitter, o Yahoo e outros sites, ou seja, como dar suporte ao OAuth e ao OpenID no seu site.
> 
> O que você aprenderá:
> 
> - Como habilitar o logon de outros sites ao usar o modelo de site inicial do WebMatrix.
> 
> Esse é o recurso ASP.NET introduzido no artigo:
> 
> - O auxiliar de `OAuthWebSecurity`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - WebMatrix 3

O Páginas da Web do ASP.NET inclui suporte para provedores [OAuth](http://oauth.net/) e [OpenID](http://openid.net/) . Usando esses provedores, você pode permitir que os usuários façam logon em seu site usando suas credenciais existentes do Facebook, Twitter, Microsoft e Google. Por exemplo, para fazer logon usando uma conta do Facebook, os usuários podem simplesmente escolher um ícone do Facebook, que os redireciona para a página de logon do Facebook onde eles inserem suas informações de usuário. Em seguida, eles podem associar o logon do Facebook à sua conta no seu site. Um aprimoramento relacionado aos recursos de associação de páginas da Web é que os usuários podem associar vários logons (incluindo logons de sites de rede social) a uma única conta no seu site.

Esta imagem mostra a página de logon do modelo de **site inicial** , em que um usuário pode escolher um ícone do Facebook, Twitter, Google ou Microsoft para habilitar o logon com uma conta externa:

![provedores externos](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Você pode habilitar a associação OAuth e OpenID removendo comentários de algumas linhas de código no modelo de **site inicial** . Os métodos e as propriedades que você usa para trabalhar com os provedores OAuth e OpenID estão na classe `WebMatrix.Security.OAuthWebSecurity`. O modelo de **site inicial** inclui uma infraestrutura de associação completa, completa com uma página de logon, um banco de dados de associação e todo o código necessário para permitir que os usuários façam logon em seu site usando as credenciais locais ou as de outro site.

Esta seção fornece um exemplo de como permitir que os usuários façam logon de sites externos em um site baseado no modelo de **site inicial** . Depois de criar um site inicial, você faz isso (detalhes a seguir):

- Para os sites que usam um provedor OAuth (Facebook, Twitter e Microsoft), você cria um aplicativo no site externo. Isso fornece as chaves de aplicativo que você precisará para invocar o recurso de logon para esses sites.
- Para sites que usam um provedor de OpenID (Google), você não precisa criar um aplicativo. Para todos esses sites, você deve ter uma conta para fazer logon e criar aplicativos de desenvolvedor.

    > [!NOTE]
    > Os aplicativos da Microsoft aceitam apenas uma URL ativa para um site de trabalho, portanto, você não pode usar uma URL do site local para testar logons.
- Edite alguns arquivos em seu site para especificar o provedor de autenticação apropriado e enviar um logon para o site que você deseja usar.

Este artigo fornece instruções separadas para as seguintes tarefas:

- [Habilitando logons do Google](#To_enable_Google_logins)
- [Habilitando logons do Facebook](#To_enable_Facebook_logins)
- [Habilitando logons do Twitter](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Habilitando logons do Google

1. Crie ou abra um site Páginas da Web do ASP.NET com base no modelo de site inicial do WebMatrix.
2. Abra a página *\_AppStart. cshtml* e remova a marca de comentário da linha de código a seguir. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Testando o logon do Google

1. Execute a página *Default. cshtml* do seu site e escolha o botão **fazer logon** .
2. Na página de *logon* , na seção **usar outro serviço para fazer logon** , escolha o botão enviar do **Google** ou **Yahoo** . Este exemplo usa o logon do Google. 

    A página da Web redireciona a solicitação para a página de logon do Google.

    ![Entrar no Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Insira as credenciais para uma conta existente do Google.
4. Se o Google perguntar se você deseja permitir que o *localhost* use as informações da conta, clique em **permitir**.

    O código usa o token do Google para autenticar o usuário e, em seguida, retorna a esta página no seu site. Esta página permite que os usuários associem seu logon do Google a uma conta existente no seu site ou possam registrar uma nova conta em seu site para associar o logon externo ao.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Escolha o botão **associar** . O navegador retorna para o home page do seu aplicativo.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Habilitando logons do Facebook

1. Vá para o [site de desenvolvedores do Facebook](https://developers.facebook.com/apps) (faça logon se você ainda não estiver conectado).
2. Escolha o botão **criar novo aplicativo** e siga os prompts para nomear e criar o novo aplicativo.
3. Na seção, **Selecione como seu aplicativo será integrado ao Facebook**, escolha a seção **site** .
4. Preencha o campo **URL do site** com a URL do seu site (por exemplo, `http://www.example.com`). O campo **domínio** é opcional; Você pode usar isso para fornecer autenticação para um domínio inteiro (como *example.com*). 

    > [!NOTE]
    > Se você estiver executando um site em seu computador local com uma URL como `http://localhost:12345` (em que o número é um número de porta local), poderá adicionar esse valor ao campo **URL do site** para testar seu site. No entanto, sempre que o número da porta do site local for alterado, você precisará atualizar o campo **URL do site** do seu aplicativo.
5. Escolha o botão **salvar alterações** .
6. Escolha a guia **aplicativos** novamente e, em seguida, exiba a página inicial do seu aplicativo.
7. Copie a **ID do aplicativo** e os valores de **segredo** do aplicativo para seu aplicativo e cole-os em um arquivo de texto temporário. Você passará esses valores para o provedor do Facebook no código do seu site.
8. Saia do site do desenvolvedor do Facebook.

Agora você faz alterações em duas páginas em seu site para que os usuários possam fazer logon no site usando suas contas do Facebook.

1. Crie ou abra um site Páginas da Web do ASP.NET com base no modelo de site inicial do WebMatrix.
2. Abra a página *\_AppStart. cshtml* e remova o comentário do código para o provedor OAuth do Facebook. O bloco de código não comentado é semelhante ao seguinte: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Copie o valor da **ID** do aplicativo do aplicativo do Facebook como o valor do parâmetro `appId` (dentro das aspas).
4. Copie o valor **secreto do aplicativo** do aplicativo Facebook como o valor do parâmetro `appSecret`.
5. Salve e feche o arquivo.

### <a name="testing-facebook-login"></a>Testando o logon do Facebook

1. Execute a página *Default. cshtml* do site e escolha o botão **logon** .
2. Na página de *logon* , na seção **usar outro serviço para fazer logon** , escolha o ícone do **Facebook** . 

    A página da Web redireciona a solicitação para a página de logon do Facebook.

    ![OAuth-2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Faça logon em uma conta do Facebook. 

    O código usa o token do Facebook para autenticá-lo e, em seguida, retorna a uma página na qual você pode associar o logon do Facebook ao logon do site. Seu nome de usuário ou endereço de email é preenchido no campo **email** do formulário.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Escolha o botão **associar** . 

    O navegador retorna ao home page e você está conectado.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Habilitando logons do Twitter

1. Navegue até o [site de desenvolvedores do Twitter](https://dev.twitter.com/).
2. Escolha o link **criar um aplicativo** e faça logon no site.
3. No formulário **criar um aplicativo** , preencha os campos **nome** e **Descrição** .
4. No campo **site** , insira a URL do seu site (por exemplo, `http://www.example.com`). 

    > [!NOTE]
    > Se você estiver testando seu site localmente (usando uma URL como `http://localhost:12345`), o Twitter poderá não aceitar a URL. No entanto, talvez você possa usar o endereço IP de loopback local (por exemplo `http://127.0.0.1:12345`). Isso simplifica o processo de teste do seu aplicativo localmente. No entanto, sempre que o número da porta do site local for alterado, você precisará atualizar o campo do **site** do seu aplicativo.
5. No campo **URL de retorno de chamada** , insira uma URL para a página no site que você deseja que os usuários retornem depois de fazer logon no Twitter. Por exemplo, para enviar usuários para o home page do site inicial (que reconhecerá seu status de logon), insira a mesma URL que você inseriu no campo **site** .
6. Aceite os termos e escolha o botão **criar seu aplicativo do Twitter** .
7. Na página inicial **meus aplicativos** , escolha o aplicativo que você criou.
8. Na guia **detalhes** , role até a parte inferior e escolha o botão **criar meu token de acesso** .
9. Na guia **detalhes** , copie os valores de **chave do consumidor** e segredo do **consumidor** para seu aplicativo e cole-os em um arquivo de texto temporário. Você passará esses valores para o provedor do Twitter no código do seu site.
10. Saia do site do Twitter.

Agora você faz alterações em duas páginas em seu site para que os usuários possam fazer logon no site usando suas contas do Twitter.

1. Crie ou abra um site Páginas da Web do ASP.NET com base no modelo de site inicial do WebMatrix.
2. Abra a página *\_AppStart. cshtml* e remova o comentário do código do provedor OAuth do Twitter. O bloco de código não comentado tem esta aparência: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Copie o valor de **chave do consumidor** do aplicativo do Twitter como o valor do parâmetro `consumerKey` (dentro das aspas).
4. Copie o valor do **segredo do consumidor** do aplicativo Twitter como o valor do parâmetro `consumerSecret`.
5. Salve e feche o arquivo.

### <a name="testing-twitter-login"></a>Testando o logon do Twitter

1. Execute a página *Default. cshtml* do seu site e escolha o botão **logon** .
2. Na página de *logon* , na seção **usar outro serviço para fazer logon** , escolha o ícone do **Twitter** . 

    A página da Web redireciona a solicitação para uma página de logon do Twitter para o aplicativo que você criou.

    ![OAuth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Faça logon em uma conta do Twitter.
4. O código usa o token do Twitter para autenticar o usuário e, em seguida, retorna a uma página onde você pode associar seu logon à sua conta de site. Seu nome ou endereço de email é preenchido no campo **email** do formulário.

    ![OAuth-5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Escolha o botão **associar** . 

    O navegador retorna ao home page e você está conectado.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [Personalizar o comportamento de todo o site](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Adicionando segurança e associação a um site Páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202904)
