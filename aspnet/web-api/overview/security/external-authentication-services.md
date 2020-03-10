---
uid: web-api/overview/security/external-authentication-services
title: Serviços de autenticação externa com ASP.NET Web APIC#() | Microsoft Docs
author: rmcmurray
description: Descreve o uso de serviços de autenticação externa no ASP.NET Web API.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: b2571552a3f8040ff42bfa0a9fa48981f71a1e4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555471"
---
# <a name="external-authentication-services-with-aspnet-web-api-c"></a>Serviços de autenticação externa com ASP.NET Web APIC#()

O Visual Studio 2017 e o ASP.NET 4.7.2 expandem as opções de segurança para Spa ( [aplicativos de página única](../../../single-page-application/index.md) ) e serviços de [API da Web](../../index.md) para integrar com serviços de autenticação externa, que incluem vários serviços de autenticação OAuth/OpenID e de mídia social: contas da Microsoft, Twitter, Facebook e Google.  

### <a name="in-this-walkthrough"></a>Neste tutorial

- [Usando serviços de autenticação externa](#USING)
- [Criando o aplicativo Web de exemplo](#SAMPLE)
- [Habilitando a autenticação do Facebook](#FACEBOOK)
- [Habilitando a autenticação do Google](#GOOGLE)
- [Habilitando a autenticação da Microsoft](#MICROSOFT)
- [Habilitando a autenticação do Twitter](#TWITTER)
- [Informações adicionais](#MOREINFO)

    - [Combinando serviços de autenticação externa](#COMBINE)
    - [Configurando IIS Express para usar um nome de domínio totalmente qualificado](#FQDN)
    - [Como obter as configurações do aplicativo para autenticação da Microsoft](#OBTAIN)
    - [Opcional: desabilitar o registro local](#DISABLE)

### <a name="prerequisites"></a>Prerequisites

Para seguir os exemplos neste passo a passos, você precisa ter o seguinte:

- Visual Studio 2017
- Uma conta de desenvolvedor com o identificador de aplicativo e a chave secreta para um dos seguintes serviços de autenticação de mídia social:

  - Contas da Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
  - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
  - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))
  - Google ([https://developers.google.com/](https://developers.google.com))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>Usando serviços de autenticação externa

A abundância de serviços de autenticação externa que estão disponíveis atualmente para desenvolvedores da Web ajudam a reduzir o tempo de desenvolvimento ao criar novos aplicativos Web. Normalmente, os usuários da Web têm várias contas existentes para sites populares de serviços da Web e de mídia social. portanto, quando um aplicativo Web implementa os serviços de autenticação de um site de mídia social ou de um serviço Web externo, ele economiza o tempo de desenvolvimento que teria sido gasto criando uma implementação de autenticação. O uso de um serviço de autenticação externo evita que os usuários finais precisem criar outra conta para seu aplicativo Web e também não precisem se lembrar de outro nome de usuário e senha.

No passado, os desenvolvedores tinham duas opções: criar sua própria implementação de autenticação ou saber como integrar um serviço de autenticação externa em seus aplicativos. No nível mais básico, o diagrama a seguir ilustra um fluxo de solicitação simples para um agente do usuário (navegador da Web) que está solicitando informações de um aplicativo Web configurado para usar um serviço de autenticação externo:

[![](external-authentication-services/_static/image2.png "Click to Expand the Image")](external-authentication-services/_static/image1.png)

No diagrama anterior, o agente do usuário (ou navegador da Web neste exemplo) faz uma solicitação para um aplicativo Web, que redireciona o navegador da Web para um serviço de autenticação externa. O agente do usuário envia suas credenciais para o serviço de autenticação externa e, se o agente do usuário tiver sido autenticado com êxito, o serviço de autenticação externa redirecionará o agente do usuário para o aplicativo Web original com alguma forma de token que o o agente do usuário será enviado para o aplicativo Web. O aplicativo Web usará o token para verificar se o agente do usuário foi autenticado com êxito pelo serviço de autenticação externa e se o aplicativo Web pode usar o token para coletar mais informações sobre o agente do usuário. Quando o aplicativo terminar de processar as informações do agente do usuário, o aplicativo Web retornará a resposta apropriada ao agente do usuário com base em suas configurações de autorização.

Neste segundo exemplo, o agente do usuário negocia com o aplicativo Web e o servidor de autorização externo, e o aplicativo Web executa comunicação adicional com o servidor de autorização externo para recuperar informações adicionais sobre o usuário Agente

[![](external-authentication-services/_static/image4.png "Click to Expand the Image")](external-authentication-services/_static/image3.png)

O Visual Studio 2017 e o ASP.NET 4.7.2 tornam a integração com os serviços de autenticação externa mais fácil para os desenvolvedores, fornecendo integração interna para os seguintes serviços de autenticação:

- Facebook
- Google
- Contas da Microsoft (contas do Windows Live ID)
- Twitter

Os exemplos neste tutorial demonstrarão como configurar cada um dos serviços de autenticação externa com suporte usando o novo modelo de aplicativo Web ASP.NET que é fornecido com o Visual Studio 2017.

> [!NOTE]
> Se necessário, talvez seja necessário adicionar seu FQDN às configurações para o serviço de autenticação externa. Esse requisito é baseado em restrições de segurança para alguns serviços de autenticação externos que exigem o FQDN em suas configurações de aplicativo para corresponder ao FQDN usado por seus clientes. (As etapas para isso variam muito para cada serviço de autenticação externo; você precisará consultar a documentação de cada serviço de autenticação externo para ver se isso é necessário e como definir essas configurações.) Se você precisar configurar IIS Express para usar um FQDN para testar esse ambiente, consulte a seção [Configurando IIS Express para usar um nome de domínio totalmente qualificado](#FQDN) , mais adiante neste guia.

<a id="SAMPLE"></a>
## <a name="create-a-sample-web-application"></a>Criar um aplicativo Web de exemplo

As etapas a seguir irão orientá-lo na criação de um aplicativo de exemplo usando o modelo de aplicativo Web ASP.NET, e você usará esse aplicativo de exemplo para cada um dos serviços de autenticação externa posteriormente neste guia.

Inicie o Visual Studio 2017 e selecione **novo projeto** na página inicial. Ou, no menu **arquivo** , selecione **novo** e **projeto**.

<!-- [![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png) -->

Quando a caixa de diálogo **novo projeto** for exibida, selecione **instalado** e expanda **Visual C#** . Em **Visual C#** , selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web ASP.net (.NET Framework)** . Insira um nome para seu projeto e clique em **OK**.

[![](external-authentication-services/_static/image71.png "Click to Expand the Image")](external-authentication-services/_static/image71.png)

Quando o **novo projeto ASP.net** for exibido, selecione o modelo **aplicativo de página única** e clique em **criar projeto**.

[![](external-authentication-services/_static/image72.png "Click to Expand the Image")](external-authentication-services/_static/image72.png)

Aguarde, pois o Visual Studio 2017 cria seu projeto.

<!-- [![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png) -->

Quando o Visual Studio 2017 terminar de criar seu projeto, abra o arquivo *Startup.auth.cs* localizado na pasta **iniciar do aplicativo\_** .

Quando você cria o projeto pela primeira vez, nenhum dos serviços de autenticação externa é habilitado no arquivo *Startup.auth.cs* ; o seguinte ilustra o que seu código pode parecer, com as seções realçadas para onde você habilitaria um serviço de autenticação externa e quaisquer configurações relevantes para usar as contas da Microsoft, o Twitter, o Facebook ou a autenticação do Google com seu aplicativo ASP.NET:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

Quando você pressiona F5 para compilar e depurar seu aplicativo Web, ele exibirá uma tela de logon onde você verá que nenhum serviço de autenticação externa foi definido.

[![](external-authentication-services/_static/image73.png "Click to Expand the Image")](external-authentication-services/_static/image73.png)

Nas seções a seguir, você aprenderá a habilitar cada um dos serviços de autenticação externa fornecidos com o ASP.NET no Visual Studio 2017.

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Habilitando a autenticação do Facebook

Usar a autenticação do Facebook exige que você crie uma conta de desenvolvedor do Facebook e seu projeto exigirá uma ID do aplicativo e uma chave secreta do Facebook para funcionar. Para obter informações sobre como criar uma conta de desenvolvedor do Facebook e como obter a ID do aplicativo e a chave secreta, consulte [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).

Depois de obter a ID do aplicativo e a chave secreta, use as seguintes etapas para habilitar a autenticação do Facebook para seu aplicativo Web:

1. Quando o projeto estiver aberto no Visual Studio 2017, abra o arquivo *Startup.auth.cs* .

2. Localize a seção de autenticação do Facebook do código:

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. Remova o &quot;//&quot; caracteres para remover os comentários das linhas de código realçadas e, em seguida, adicione a ID do aplicativo e a chave secreta. Depois de adicionar esses parâmetros, você poderá Recompilar o projeto:

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Ao pressionar F5 para abrir seu aplicativo Web no navegador da Web, você verá que o Facebook foi definido como um serviço de autenticação externo:

    [![](external-authentication-services/_static/image74.png "Click to Expand the Image")](external-authentication-services/_static/image74.png)
5. Quando você clicar no botão do **Facebook** , seu navegador será redirecionado para a página de logon do Facebook:

    [![](external-authentication-services/_static/image22.png "Click to Expand the Image")](external-authentication-services/_static/image21.png)
6. Depois de inserir suas credenciais do Facebook e clicar em **fazer logon**, seu navegador da Web será Redirecionado de volta para seu aplicativo Web, o que lhe solicitará o **nome de usuário** que você deseja associar à sua conta do Facebook:

    [![](external-authentication-services/_static/image24.png "Click to Expand the Image")](external-authentication-services/_static/image23.png)
7. Depois de inserir seu nome de usuário e clicar no botão **inscrever-se** , seu aplicativo Web exibirá o **Home Page** padrão para sua conta do Facebook:

    [![](external-authentication-services/_static/image26.png "Click to Expand the Image")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Habilitando a autenticação do Google

Usar a autenticação do Google exige que você crie uma conta de desenvolvedor do Google e seu projeto exigirá uma ID do aplicativo e uma chave secreta do Google para funcionar. Para obter informações sobre como criar uma conta de desenvolvedor do Google e obter a ID do aplicativo e a chave secreta, consulte [https://developers.google.com](https://developers.google.com).

Para habilitar a autenticação do Google para seu aplicativo Web, use as seguintes etapas:

1. Quando o projeto estiver aberto no Visual Studio 2017, abra o arquivo *Startup.auth.cs* .

2. Localize a seção de autenticação do Google do código:

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. Remova o &quot;//&quot; caracteres para remover os comentários das linhas de código realçadas e, em seguida, adicione a ID do aplicativo e a chave secreta. Depois de adicionar esses parâmetros, você poderá Recompilar o projeto:

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Quando você pressionar F5 para abrir seu aplicativo Web em seu navegador da Web, verá que o Google foi definido como um serviço de autenticação externo:

    [![](external-authentication-services/_static/image75.png "Click to Expand the Image")](external-authentication-services/_static/image75.png)
5. Quando você clicar no botão **Google** , seu navegador será redirecionado para a página de logon do Google:

    [![](external-authentication-services/_static/image32.png "Click to Expand the Image")](external-authentication-services/_static/image31.png)
6. Depois de inserir suas credenciais do Google e clicar em **entrar**, o Google solicitará que você verifique se seu aplicativo Web tem permissões para acessar sua conta do Google:

    [![](external-authentication-services/_static/image34.png "Click to Expand the Image")](external-authentication-services/_static/image33.png)
7. Quando você clicar em **aceitar**, seu navegador da Web será Redirecionado de volta para seu aplicativo Web, que solicitará o **nome de usuário** que você deseja associar à sua conta do Google:

    [![](external-authentication-services/_static/image36.png "Click to Expand the Image")](external-authentication-services/_static/image35.png)
8. Depois de inserir seu nome de usuário e clicar no botão **inscrever-se** , seu aplicativo Web exibirá o **Home Page** padrão para sua conta do Google:

    [![](external-authentication-services/_static/image38.png "Click to Expand the Image")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Habilitando a autenticação da Microsoft

A autenticação da Microsoft exige que você crie uma conta de desenvolvedor e exija uma ID do cliente e um segredo do cliente para funcionar. Para obter informações sobre como criar uma conta de desenvolvedor da Microsoft e como obter a ID do cliente e o segredo do cliente, consulte [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).

Depois de obter a chave do consumidor e o segredo do consumidor, use as seguintes etapas para habilitar a autenticação da Microsoft para seu aplicativo Web:

1. Quando o projeto estiver aberto no Visual Studio 2017, abra o arquivo *Startup.auth.cs* .

2. Localize a seção Microsoft Authentication do código:

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. Remova o &quot;//&quot; caracteres para remover os comentários das linhas de código realçadas e, em seguida, adicione a ID do cliente e o segredo do cliente. Depois de adicionar esses parâmetros, você poderá Recompilar o projeto:

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Ao pressionar F5 para abrir seu aplicativo Web no navegador da Web, você verá que a Microsoft foi definida como um serviço de autenticação externo:

    [![](external-authentication-services/_static/image42.png "Click to Expand the Image")](external-authentication-services/_static/image41.png)
5. Quando você clicar no botão **Microsoft** , seu navegador será redirecionado para a página de logon da Microsoft:

    [![](external-authentication-services/_static/image44.png "Click to Expand the Image")](external-authentication-services/_static/image43.png)
6. Depois de inserir suas credenciais da Microsoft e clicar em **entrar**, você será solicitado a verificar se o seu aplicativo Web tem permissões para acessar seu conta Microsoft:

    [![](external-authentication-services/_static/image46.png "Click to Expand the Image")](external-authentication-services/_static/image45.png)
7. Quando você clicar em **Sim**, seu navegador da Web será Redirecionado de volta para seu aplicativo Web, o que solicitará o **nome de usuário** que você deseja associar ao seu conta Microsoft:

    [![](external-authentication-services/_static/image48.png "Click to Expand the Image")](external-authentication-services/_static/image47.png)
8. Depois de inserir seu nome de usuário e clicar no botão **inscrever-se** , seu aplicativo Web exibirá o **Home Page** padrão para seu conta Microsoft:

    [![](external-authentication-services/_static/image50.png "Click to Expand the Image")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Habilitando a autenticação do Twitter

A autenticação do Twitter exige que você crie uma conta de desenvolvedor e requer uma chave do consumidor e um segredo do consumidor para funcionar. Para obter informações sobre como criar uma conta de desenvolvedor do Twitter e como obter a chave do consumidor e o segredo do consumidor, consulte [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).

Depois de obter a chave do consumidor e o segredo do consumidor, use as seguintes etapas para habilitar a autenticação do Twitter para seu aplicativo Web:

1. Quando o projeto estiver aberto no Visual Studio 2017, abra o arquivo *Startup.auth.cs* .

2. Localize a seção de autenticação do Twitter do código:

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. Remova o &quot;//&quot; caracteres para remover os comentários das linhas de código realçadas e, em seguida, adicione a chave do consumidor e o segredo do consumidor. Depois de adicionar esses parâmetros, você poderá Recompilar o projeto:

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Ao pressionar F5 para abrir seu aplicativo Web no navegador da Web, você verá que o Twitter foi definido como um serviço de autenticação externo:

    [![](external-authentication-services/_static/image54.png "Click to Expand the Image")](external-authentication-services/_static/image53.png)
5. Quando você clicar no botão do **Twitter** , seu navegador será redirecionado para a página de logon do Twitter:

    [![](external-authentication-services/_static/image56.png "Click to Expand the Image")](external-authentication-services/_static/image55.png)
6. Depois de inserir suas credenciais do Twitter e clicar em **autorizar aplicativo**, seu navegador da Web será Redirecionado de volta para seu aplicativo Web, o que lhe solicitará o **nome de usuário** que você deseja associar à sua conta do Twitter:

    [![](external-authentication-services/_static/image58.png "Click to Expand the Image")](external-authentication-services/_static/image57.png)
7. Depois de inserir seu nome de usuário e clicar no botão **inscrever-se** , seu aplicativo Web exibirá o **Home Page** padrão para sua conta do Twitter:

    [![](external-authentication-services/_static/image60.png "Click to Expand the Image")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>Informações adicionais

Para obter informações adicionais sobre como criar aplicativos que usam OAuth e OpenID, consulte as seguintes URLs:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>Combinando serviços de autenticação externa

Para obter maior flexibilidade, você pode definir vários serviços de autenticação externa ao mesmo tempo – isso permite que os usuários do aplicativo Web usem uma conta de qualquer um dos serviços de autenticação externa habilitados:

[![](external-authentication-services/_static/image62.png "Click to Expand the Image")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configure-iis-express-to-use-a-fully-qualified-domain-name"></a>Configurar IIS Express para usar um nome de domínio totalmente qualificado

Alguns provedores de autenticação externa não dão suporte ao teste de seu aplicativo usando um endereço HTTP como `http://localhost:port/`. Para contornar esse problema, você pode adicionar um mapeamento de FQDN (nome de domínio totalmente qualificado) para o arquivo de HOSTs e configurar suas opções de projeto no Visual Studio 2017 para usar o FQDN para teste/depuração. Para fazer isso, execute as seguintes etapas:

- Adicione um FQDN estático mapeando seu arquivo HOSTs:

  1. Abra um prompt de comando com privilégios elevados no Windows.
  2. Digite o seguinte comando:

      <kbd>%WinDir%\system32\drivers\etc\hosts do bloco de notas</kbd>
  3. Adicione uma entrada como a seguinte ao arquivo de HOSTs:

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. Salve e feche o arquivo de HOSTs.

- Configure seu projeto do Visual Studio para usar o FQDN:

  1. Quando o projeto estiver aberto no Visual Studio 2017, clique no menu **projeto** e selecione as propriedades do projeto. Por exemplo, você pode selecionar **Propriedades WebApplication1**.
  2. Selecione a guia **Web** .
  3. Insira seu FQDN para a <strong>URL do projeto</strong>. Por exemplo, você deve inserir <kbd><http://www.wingtiptoys.com></kbd> se esse fosse o mapeamento de FQDN que você adicionou ao arquivo de hosts.

- Configure IIS Express para usar o FQDN para seu aplicativo:

    1. Abra um prompt de comando com privilégios elevados no Windows.
    2. Digite o seguinte comando para alterar para sua pasta IIS Express:

        <kbd>CD/d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. Digite o seguinte comando para adicionar o FQDN ao seu aplicativo:

        <kbd>Appcmd. exe set config-section: System. applicationHost/sites/+&quot;[name = ' WebApplication1 ']. Bindings. [Protocol = ' http ', bindingInformation = ' *: 80: www. wingtiptoys. com ']&quot;/commit: appHost</kbd>

  Em que **WebApplication1** é o nome do seu projeto e **bindingInformation** contém o número da porta e o FQDN que você deseja usar para o teste.

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Como obter as configurações do aplicativo para autenticação da Microsoft

Vincular um aplicativo ao Windows Live para autenticação da Microsoft é um processo simples. Se você ainda não tiver vinculado um aplicativo ao Windows Live, poderá usar as seguintes etapas:

1. Navegue até [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) e insira seu nome de conta Microsoft e senha quando solicitado e clique em **entrar**:

   <!--  [![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png) -->
2. Selecione **Adicionar um aplicativo** e insira o nome do seu aplicativo quando solicitado e, em seguida, clique em **criar**:

    [![](external-authentication-services/_static/image79.png "Click to Expand the Image")](external-authentication-services/_static/image79.png)
3. Selecione seu aplicativo em **nome** e sua página de propriedades do aplicativo será exibida.

4. Insira o domínio de redirecionamento para seu aplicativo. Copie a **ID do aplicativo** e, em **segredos do aplicativo**, selecione **gerar senha**. Copie a senha que aparece. A ID do aplicativo e a senha são a ID do cliente e o segredo do cliente. Selecione **OK** e, em seguida, **salvar**.

    [![](external-authentication-services/_static/image77.png "Click to Expand the Image")](external-authentication-services/_static/image77.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>Opcional: desabilitar o registro local

A funcionalidade atual de registro local ASP.NET não impede que os bots (programas automatizados) criem contas de membro; por exemplo, usando uma tecnologia de validação e prevenção de bot, como [captcha](../../../web-pages/overview/security/16-adding-security-and-membership.md). Por isso, você deve remover o formulário de logon local e o link de registro na página de logon. Para fazer isso, abra a página *\_login. cshtml* em seu projeto e comente as linhas para o painel de logon local e o link de registro. A página resultante deve ser semelhante ao seguinte exemplo de código:

[!code-html[Main](external-authentication-services/samples/sample10.html)]

Depois que o painel de logon local e o link de registro tiverem sido desabilitados, sua página de logon exibirá somente os provedores de autenticação externa que você tiver habilitado:

[![](external-authentication-services/_static/image70.png "Click to Expand the Image")](external-authentication-services/_static/image69.png)
