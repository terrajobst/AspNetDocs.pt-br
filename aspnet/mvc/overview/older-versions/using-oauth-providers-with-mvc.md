---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: Usando provedores OAuth com MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como criar um aplicativo Web ASP.NET MVC 4 que permite que os usuários façam logon com credenciais de um provedor externo, como Facebo...
ms.author: riande
ms.date: 06/19/2013
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5dfd1305376a62f4987caea242ca0f6aac1018e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539077"
---
# <a name="using-oauth-providers-with-mvc-4"></a>Uso de provedores OAuth com o MVC 4

por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como criar um aplicativo Web do ASP.NET MVC 4 que permite que os usuários façam logon com credenciais de um provedor externo, como Facebook, Twitter, Microsoft ou Google, e, em seguida, integre parte da funcionalidade desses provedores ao seu aplicativo Web. Para simplificar, este tutorial se concentra em trabalhar com credenciais do Facebook.
> 
> Para usar credenciais externas em um aplicativo Web ASP.NET MVC 5, confira [criar um aplicativo ASP.NET MVC 5 com o Facebook e o Google OAuth2 e o logon OpenID](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> A habilitação dessas credenciais em seus sites proporciona uma vantagem significativa porque milhões de usuários já têm contas com esses provedores externos. Esses usuários podem ser mais inclinados para se inscrever no seu site se não precisarem criar e lembrar um novo conjunto de credenciais. Além disso, depois que um usuário fizer logon por meio de um desses provedores, você poderá incorporar operações sociais do provedor.

## <a name="what-youll-build"></a>O que você criará

Há dois objetivos principais neste tutorial:

1. Permitir que um usuário faça logon com credenciais de um provedor OAuth.
2. Recupere as informações da conta do provedor e integre-as com o registro de conta do seu site.

Embora os exemplos neste tutorial se concentrem no uso do Facebook como o provedor de autenticação, você pode modificar o código para usar qualquer um dos provedores. As etapas para implementar qualquer provedor são muito semelhantes às etapas que você verá neste tutorial. Você só observará diferenças significativas ao fazer chamadas diretas para o conjunto de API do provedor.

## <a name="prerequisites"></a>Prerequisites

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) ou [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Ou

- Microsoft Visual Studio 2010 SP1 ou [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

Além disso, este tópico pressupõe que você tenha conhecimento básico sobre o ASP.NET MVC e o Visual Studio. Se você precisar de uma introdução ao ASP.NET MVC 4, consulte [introdução ao ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Criar o projeto

No Visual Studio, crie um novo aplicativo Web ASP.NET MVC 4 e nomeie-o &quot;OAuthMVC&quot;. Você pode ter como destino .NET Framework 4,5 ou 4.

![criar projeto](using-oauth-providers-with-mvc/_static/image1.png)

Na janela novo projeto do ASP.NET MVC 4, selecione **aplicativo de Internet** e deixe o **Razor** como o mecanismo de exibição.

![Selecionar aplicativo de Internet](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Habilitar um provedor

Quando você cria um aplicativo Web MVC 4 com o modelo de aplicativo de Internet, o projeto é criado com um arquivo chamado AuthConfig.cs na pasta iniciar do aplicativo\_.

![Arquivo AuthConfig](using-oauth-providers-with-mvc/_static/image3.png)

O arquivo AuthConfig contém o código para registrar clientes para provedores de autenticação externa. Por padrão, esse código é comentado, portanto nenhum dos provedores externos está habilitado.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

Você deve remover o comentário deste código para usar o cliente de autenticação externa. Você remove os comentários somente dos provedores que deseja incluir em seu site. Para este tutorial, você só habilitará as credenciais do Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Observe no exemplo acima que o método inclui cadeias de caracteres vazias para os parâmetros de registro. Se você tentar executar o aplicativo agora, o aplicativo gerará uma exceção de argumento porque cadeias de caracteres vazias não são permitidas para os parâmetros. Para fornecer valores válidos, você deve registrar seu site da Web com os provedores externos, conforme mostrado na próxima seção.

## <a name="registering-with-an-external-provider"></a>Registrando com um provedor externo

Para autenticar usuários com credenciais de um provedor externo, você deve registrar seu site com o provedor. Ao registrar seu site, você receberá os parâmetros (como chave ou ID e segredo) a serem incluídos ao registrar o cliente. Você deve ter uma conta com os provedores que deseja usar.

Este tutorial não mostra todas as etapas que você deve executar para se registrar com esses provedores. As etapas normalmente não são difíceis. Para registrar seu site com êxito, siga as instruções fornecidas nesses sites. Para começar a registrar seu site, consulte o site do desenvolvedor para:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Ao registrar seu site com o Facebook, você pode fornecer &quot;&quot; localhost para o domínio do site e `&quot; http://localhost/&quot;` para a URL, conforme mostrado na imagem abaixo. O uso do localhost funciona com a maioria dos provedores, mas atualmente não funciona com o provedor da Microsoft. Para o provedor Microsoft, você deve incluir uma URL de site válida.

![registrar site](using-oauth-providers-with-mvc/_static/image4.png)

Na imagem anterior, os valores para a ID do aplicativo, o segredo do aplicativo e o email de contato foram removidos. Quando você realmente registra seu site, esses valores estarão presentes. Você desejará observar os valores para ID do aplicativo e segredo do aplicativo, pois você os adicionará ao seu aplicativo.

## <a name="creating-test-users"></a>Criando usuários de teste

Se você não se importa com uma conta existente do Facebook para testar seu site, você pode ignorar esta seção.

Você pode criar facilmente usuários de teste para seu aplicativo na página de gerenciamento de aplicativo do Facebook. Você pode usar essas contas de teste para fazer logon no seu site. Crie usuários de teste clicando no link **funções** no painel de navegação à esquerda e clicando no link **criar** .

![criar usuários de teste](using-oauth-providers-with-mvc/_static/image5.png)

O site do Facebook cria automaticamente o número de contas de teste que você solicita.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Adicionando ID do aplicativo e segredo do provedor

Agora que você recebeu a ID e o segredo do Facebook, volte para o arquivo AuthConfig e adicione-os como os valores de parâmetro. Os valores mostrados abaixo não são valores reais.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Fazer logon com credenciais externas

Isso é tudo o que você precisa fazer para habilitar credenciais externas em seu site. Execute o aplicativo e clique no link logon no canto superior direito. O modelo reconhece automaticamente que você registrou o Facebook como um provedor e inclui um botão para o provedor. Se você registrar vários provedores, um botão para cada um será incluído automaticamente.

![logon externo](using-oauth-providers-with-mvc/_static/image6.png)

Este tutorial não aborda como personalizar os botões de logon para os provedores externos. Para obter essas informações, consulte [Personalizando a interface do usuário de logon ao usar OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Clique no botão do Facebook para fazer logon com as credenciais do Facebook. Quando você seleciona um dos provedores externos, você é redirecionado para esse site e solicitado por esse serviço para fazer logon.

A imagem a seguir mostra a tela de logon para o Facebook. Ele observa que você está usando sua conta do Facebook para fazer logon em um site chamado oauthmvcexample.

![autenticação do Facebook](using-oauth-providers-with-mvc/_static/image7.png)

Depois de fazer logon com as credenciais do Facebook, uma página informa ao usuário que o site terá acesso às informações básicas.

![solicitar permissão](using-oauth-providers-with-mvc/_static/image8.png)

Depois de selecionar **ir para o aplicativo**, o usuário deverá se registrar no site. A imagem a seguir mostra a página de registro depois que um usuário fez logon com as credenciais do Facebook. O nome de usuário normalmente é preenchido previamente com um nome do provedor.

![register](using-oauth-providers-with-mvc/_static/image9.png)

Clique em registrar para concluir **o** registro. Feche o navegador.

Você pode ver que a nova conta foi adicionada ao seu banco de dados. Em Gerenciador de Servidores, abra o banco de dados **DefaultConnection** e abra a pasta **tabelas** .

![tabelas de banco de dados](using-oauth-providers-with-mvc/_static/image10.png)

Clique com o botão direito do mouse na tabela **USERPROFILE** e selecione **Mostrar dados da tabela**.

![Mostrar dados](using-oauth-providers-with-mvc/_static/image11.png)

Você verá a nova conta que adicionou. Examine os dados na **página da web\_tabela OAuthMembership** . Você verá mais dados relacionados ao provedor externo para a conta que você acabou de adicionar.

Se você quiser apenas habilitar a autenticação externa, pronto. No entanto, você pode integrar ainda mais as informações do provedor ao novo processo de registro de usuário, conforme mostrado nas seções a seguir.

## <a name="create-models-for-additional-user-information"></a>Criar modelos para informações adicionais do usuário

Como você percebeu nas seções anteriores, não é necessário recuperar informações adicionais para que o registro de conta interno funcione. No entanto, a maioria dos provedores externos passa informações adicionais sobre o usuário. As seções a seguir mostram como reter essas informações e salvá-las em um banco de dados. Especificamente, você reterá valores para o nome completo do usuário, o URI da página da Web pessoal do usuário e um valor que indica se o Facebook verificou a conta.

Você usará [migrações do Code First](https://msdn.microsoft.com/data/jj591621) para adicionar uma tabela para armazenar informações adicionais do usuário. Você está adicionando a tabela a um banco de dados existente, portanto, primeiro será necessário criar um instantâneo do banco de dados atual. Ao criar um instantâneo do banco de dados atual, você pode criar posteriormente uma migração que contenha apenas a nova tabela. Para criar um instantâneo do banco de dados atual:

1. Abrir o **console do Gerenciador de pacotes**
2. Execute o comando **Enable-Migrations**
3. Execute o comando **Add-Migration Initial – IgnoreChanges**
4. Execute o comando **Update-Database**

Agora, você adicionará as novas propriedades. Na pasta modelos, abra o arquivo AccountModels.cs e localize a classe RegisterExternalLoginModel. A classe RegisterExternalLoginModel mantém os valores que retornam do provedor de autenticação. Adicione propriedades chamadas FullName e link, conforme destacado abaixo.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

Também em AccountModels.cs, adicione uma nova classe chamada ExtraUserInformation. Essa classe representa a nova tabela que será criada no banco de dados.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

Na classe UsersContext, adicione o código realçado abaixo para criar uma propriedade DbSet para a nova classe.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

Agora você está pronto para criar a nova tabela. Abra o console do Gerenciador de pacotes novamente e desta vez:

1. Execute o comando **Add-Migration AddExtraUserInformation**
2. Execute o comando **Update-Database**

A nova tabela agora existe no banco de dados.

## <a name="retrieve-the-additional-data"></a>Recuperar os dados adicionais

Há duas maneiras de recuperar dados de usuário adicionais. A primeira maneira é manter os dados do usuário que são passados de volta, por padrão, durante a solicitação de autenticação. A segunda maneira é chamar especificamente a API do provedor e solicitar mais informações. Os valores para FullName e link são passados automaticamente de volta pelo Facebook. Um valor que indica se o Facebook verificou se a conta foi recuperada por meio de uma chamada para a API do Facebook. Primeiro, você preencherá os valores para FullName e link e, posteriormente, obterá o valor verificado.

Para recuperar os dados de usuário adicionais, abra o arquivo **AccountController.cs** na pasta **controladores** .

Esse arquivo contém a lógica para registro em log, registro e gerenciamento de contas. Em particular, observe os métodos chamados **ExternalLoginCallback** e **ExternalLoginConfirmation**. Dentro desses métodos, você pode adicionar código para personalizar as operações de logon externo para seu aplicativo. A primeira linha do método **ExternalLoginCallback** contém:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Os dados de usuário adicionais são passados de volta na propriedade **ExtraData** do objeto **AuthenticationResult** que é retornado do método **VerifyAuthentication** . O cliente do Facebook contém os seguintes valores na propriedade **ExtraData** :

- id
- name
- link
- gender
- accesstoken

Outros provedores terão dados semelhantes, mas ligeiramente diferentes na propriedade ExtraData.

Se o usuário for novo no seu site, você recuperará alguns dados adicionais e passará esses dados para a exibição de confirmação. O último bloco de código no método será executado somente se o usuário for novo no seu site. Substitua a seguinte linha:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

Por esta linha:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Essa alteração inclui apenas valores para as propriedades FullName e link.

No método **ExternalLoginConfirmation** , modifique o código conforme realçado abaixo para salvar as informações adicionais do usuário.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Ajustando a exibição

Os dados de usuário adicionais que você recupera do provedor serão exibidos na página de registro.

Na pasta **views**/**Account** , abra **ExternalLoginConfirmation. cshtml**. Abaixo do campo existente para nome de usuário, adicione campos para FullName, link e PictureLink.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Agora você está quase pronto para executar o aplicativo e registrar um novo usuário com as informações adicionais salvas. Você deve ter uma conta que ainda não esteja registrada com o site. Você pode usar uma conta de teste diferente ou excluir as linhas no **USERPROFILE** e nas **páginas da Web\_tabelas OAuthMembership** para a conta que você deseja reutilizar. Ao excluir essas linhas, você garantirá que a conta seja registrada novamente.

Execute o aplicativo e registre o novo usuário. Observe que, desta vez, a página de confirmação contém mais valores.

![register](using-oauth-providers-with-mvc/_static/image12.png)

Depois de concluir o registro, feche o navegador. Examine o banco de dados para observar os novos valores na tabela **ExtraUserInformation** .

## <a name="install-nuget-package-for-facebook-api"></a>Instalar o pacote NuGet para a API do Facebook

O Facebook fornece uma [API](https://developers.facebook.com/docs/reference/apis/) que você pode chamar para executar operações. Você pode chamar a API do Facebook ao direcionar o envio de solicitações HTTP ou ao usar a instalação de um pacote NuGet que facilita o envio dessas solicitações. O uso de um pacote NuGet é mostrado neste tutorial, mas a instalação do pacote NuGet não é essencial. Este tutorial mostra como usar o pacote do C# SDK do Facebook. Há outros pacotes NuGet que auxiliam na chamada da API do Facebook.

Na janela **gerenciar pacotes NuGet** , selecione o pacote do C# SDK do Facebook.

![instalar pacote](using-oauth-providers-with-mvc/_static/image13.png)

Você usará o SDK C# do Facebook para chamar uma operação que requer o token de acesso para o usuário. A próxima seção mostra como obter o token de acesso.

## <a name="retrieve-access-token"></a>Recuperar token de acesso

A maioria dos provedores externos retorna um token de acesso depois que as credenciais do usuário são verificadas. Esse token de acesso é muito importante porque permite que você chame operações que só estão disponíveis para usuários autenticados. Portanto, a recuperação e o armazenamento do token de acesso é essencial quando você deseja fornecer mais funcionalidade.

Dependendo do provedor externo, o token de acesso pode ser válido por apenas um período limitado. Para garantir que você tenha um token de acesso válido, você o recuperará toda vez que o usuário fizer logon e o armazenará como um valor de sessão em vez de salvá-lo em um banco de dados.

No método **ExternalLoginCallback** , o token de acesso também é passado de volta na propriedade **ExtraData** do objeto **AuthenticationResult** . Adicione o código realçado a **ExternalLoginCallback** para salvar o token de acesso no objeto de **sessão** . Esse código é executado toda vez que o usuário faz logon com uma conta do Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Embora este exemplo recupere um token de acesso do Facebook, você pode recuperar o token de acesso de qualquer provedor externo por meio da mesma chave chamada &quot;accesstoken&quot;.

## <a name="logging-off"></a>Encerrando Sessão

O método de **logoff** padrão registra o usuário fora do seu aplicativo, mas não registra o usuário fora do provedor externo. Para fazer logoff do usuário do Facebook e impedir que o token persista após o logoff do usuário, você pode adicionar o seguinte código realçado ao método de **logoff** no AccountController.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

O valor que você fornece no parâmetro `next` é a URL a ser usada após o usuário fazer logoff do Facebook. Ao testar em seu computador local, você forneceria o número de porta correto para seu site local. Em produção, você forneceria uma página padrão, como contoso.com.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Recuperar informações do usuário que exigem o token de acesso

Agora que você armazenou o token de acesso e instalou o C# pacote do SDK do Facebook, você pode usá-los juntos para solicitar informações adicionais do usuário do Facebook. No método **ExternalLoginConfirmation** , crie uma instância da classe **FacebookClient** passando o valor do token de acesso. Solicite o valor da propriedade **verificada** para o usuário atual autenticado. A propriedade **verificada** indica se o Facebook validou a conta por outros meios, como enviar uma mensagem para um telefone celular. Salve esse valor no banco de dados.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Você precisará, novamente, excluir os registros no banco de dados para o usuário ou usar uma conta diferente do Facebook.

Execute o aplicativo e registre o novo usuário. Examine a tabela **ExtraUserInformation** para ver o valor da propriedade verificada.

## <a name="conclusion"></a>Conclusão

Neste tutorial, você criou um site que é integrado com o Facebook para dados de registro e autenticação de usuário. Você aprendeu sobre o comportamento padrão configurado para o aplicativo Web MVC 4 e como personalizar esse comportamento padrão.

## <a name="related-topics"></a>Tópicos relacionados

- [Criar um aplicativo ASP.NET MVC com autenticação e o banco de dados SQL e implantar no Serviço de Aplicativo do Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
