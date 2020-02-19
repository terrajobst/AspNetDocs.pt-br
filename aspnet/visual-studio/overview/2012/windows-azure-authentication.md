---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Autenticação do Windows Azure | Microsoft Docs
author: Rick-Anderson
description: As ferramentas de Microsoft ASP.NET para o Windows Azure Active Directory simplificam a habilitação da autenticação para aplicativos Web hospedados em sites do Windows Azure....
ms.author: riande
ms.date: 02/20/2013
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: ce98effe18dd739504fb0d5453bae8a46c3ba102
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457473"
---
# <a name="windows-azure-authentication"></a>Autenticação do Microsoft Azure

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> As ferramentas de Microsoft ASP.NET para o Windows Azure Active Directory simplificam a habilitação da autenticação para aplicativos Web hospedados em [sites do Windows Azure](https://www.windowsazure.com/home/features/web-sites/). Você pode usar a autenticação do Windows Azure para autenticar usuários do Office 365 de sua organização, contas corporativas sincronizadas do Active Directory local ou usuários criados em seu próprio domínio personalizado do Windows Azure Active Directory. Habilitar a autenticação do Windows Azure configura seu aplicativo para autenticar usuários usando um único locatário do [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) .
>
> A ferramenta de autenticação ASP.NET do Windows Azure não tem suporte para funções Web em um serviço de nuvem, mas planejamos fazer isso em uma versão futura. O [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) tem suporte em funções Web do Windows Azure.
>
> Para obter detalhes sobre como configurar a sincronização entre o Active Directory local e o locatário do Windows Azure Active Directory, consulte [usar AD FS 2,0 para implementar e gerenciar o logon único](https://technet.microsoft.com/library/jj205462.aspx).
>
> O Windows Azure Active Directory está disponível atualmente como um [serviço de visualização gratuito](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="requirements"></a>Requisitos:

- Visual Studio 2012 ou [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Extensões de ferramentas da Web para Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) ou [extensões de ferramentas da web para Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Ferramentas de Microsoft ASP.net para windows Azure Active Directory – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) ou [ferramentas de Microsoft ASP.NET para windows Azure Active Directory – Visual Studio Express 2012 para Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Criar um aplicativo Web ASP.NET com o Visual Studio 2012

Você pode criar qualquer aplicativo Web com o Visual Studio 2012, este tutorial usa o modelo de intranet ASP.NET MVC.

1. Crie um novo aplicativo de intranet do ASP.NET MVC 4 e aceite todos os padrões. (Deve ser um no **Traversal** net e não no projeto **de rede** de entrada).
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Habilitar a autenticação do Windows Azure (quando você for um administrador global da filosofia)

Se você não tiver um locatário do Windows Azure Active Directory existente (por exemplo, por meio de uma conta existente do Office 365), poderá criar um novo locatário inscrevendo-se em uma [nova conta do Windows Azure Active Directory](https://g.microsoftonline.com/0AX00en/5).

1. No menu do projeto, selecione **habilitar autenticação do Windows Azure**:

   ![](windows-azure-authentication/_static/image2.png)

2. Insira o domínio para seu locatário do Windows Azure Active Directory (por exemplo, contoso.onmicrosoft.com) e clique em **habilitar**:

![](windows-azure-authentication/_static/image3.png)

3. Na caixa de diálogo autenticação da Web, entre como um administrador para seu locatário do Windows Azure Active Directory:

   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Habilitar o Windows Azure por um não administrador da filosofia

Se você não tiver o privilégio de administrador global para seu locatário do Windows Azure Active Directory, você poderá desmarcar a caixa de seleção para provisionar o aplicativo.

![](windows-azure-authentication/_static/image6.png)

A caixa de diálogo exibirá o **domínio**, a **ID da entidade de segurança do aplicativo** e a URL de **resposta** , que são necessários para provisionar o aplicativo com um Azure Active Directory filosofia. Você precisa fornecer essas informações a alguém que tenha privilégios suficientes para provisionar o aplicativo. Consulte[como implementar o logon único com o Windows Azure Active Directory-aplicativo ASP.net](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) para obter detalhes sobre como usar o cmdlet para criar a entidade de serviço manualmente.
Depois que o aplicativo tiver sido provisionado com êxito, você poderá clicar em **continuar para atualizar o Web. config com as configurações selecionadas**. Se você quiser continuar desenvolvendo o aplicativo enquanto aguarda o provisionamento acontecer, clique em **fechar para lembrar as configurações no arquivo de projeto**. Na próxima vez que você invocar habilitar a autenticação do Windows Azure e desmarcar a caixa de seleção provisionamento, verá as mesmas configurações e poderá clicar em **continuar**e, em seguida, **aplicar essas configurações em Web. config**.

1. Aguarde enquanto o aplicativo é configurado para autenticação do Windows Azure e provisionado com o Windows Azure Active Directory.
2. Depois que a autenticação do Windows Azure tiver sido habilitada para seu aplicativo, clique em **fechar:**

    ![](windows-azure-authentication/_static/image7.png)
3. Pressione F5 para executar o aplicativo. Você deve ser automaticamente redirecionado para a página de logon. Use as credenciais de usuário da filosofia do diretório para fazer logon no aplicativo.

    ![](windows-azure-authentication/_static/image1.jpg)
4. Como seu aplicativo está usando um certificado de teste autoassinado, você receberá um aviso do navegador de que o certificado não foi emitido por uma autoridade de certificação confiável.

    Esse aviso pode ser ignorado com segurança durante o desenvolvimento local clicando em **continuar neste site:**

    ![](windows-azure-authentication/_static/image8.png)
5. Agora você fez logon com êxito no seu aplicativo usando a autenticação do Windows Azure!

    ![](windows-azure-authentication/_static/image2.jpg)

Habilitar a autenticação do Windows Azure faz as seguintes alterações em seu aplicativo:

- Uma classe[antiCSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))(solicitação de site forjada) ( *aplicativo\_Start\AntiXsrfConfig.cs* ) é adicionada ao seu projeto.
- Os pacotes NuGet `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` são adicionados ao seu projeto.
- As configurações do Windows Identity Foundation em seu aplicativo serão configuradas para aceitar tokens de segurança do seu locatário do Windows Azure Active Directory. Clique na imagem abaixo para ver uma exibição expandida das alterações feitas no arquivo *Web. config* .

     ![](windows-azure-authentication/_static/image9.png)
- Uma entidade de serviço para seu aplicativo no seu locatário do Windows Azure Active Directory será provisionada.
- O HTTPS está habilitado.

## <a name="deploy-the-application-to-windows-azure"></a>Implantar o aplicativo no Windows Azure

Para obter instruções completas, consulte [implantando um aplicativo Web ASP.net em um site do Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Para publicar um aplicativo usando a autenticação do Windows Azure em um site do Azure:

1. Clique com o botão direito do mouse em seu aplicativo e selecione **publicar:**

    ![](windows-azure-authentication/_static/image3.jpg)
2. Na caixa de diálogo Publicar na Web, baixe e importe um perfil de publicação para seu site do Azure.

    ![](windows-azure-authentication/_static/image4.jpg)
3. A guia **conexão** mostra a **URL de destino** (a URL voltada para o público para seu aplicativo). Clique em **validar conexão** para testar sua conexão:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Se você publicou neste site do Azure, considere marcar a configuração **remover arquivos adicionais no destino** para garantir que seu aplicativo seja publicado corretamente. Observe que a caixa de seleção **habilitar a autenticação do Windows Azure** está marcada.

    ![](windows-azure-authentication/_static/image10.png)
5. Opcional: na guia **Visualizar** , clique em **Iniciar visualização** para ver os arquivos implantados.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Clique em **publicar.**

    Você será solicitado a habilitar a autenticação do Windows Azure para o host de destino. Clique em **habilitar** para continuar:

    ![](windows-azure-authentication/_static/image11.png)
7. Insira suas credenciais de administrador para seu locatário do Windows Azure Active Directory:

    ![](windows-azure-authentication/_static/image7.jpg)
8. Depois que o aplicativo for publicado com êxito, um navegador será aberto no site da Web publicado.

    > [!NOTE]
    > Pode levar até cinco minutos (normalmente deve menos) para que seu aplicativo seja totalmente provisionado com o Windows Azure Active Directory depois de habilitar a autenticação do Windows Azure para o host de destino. Quando você executar o aplicativo pela primeira vez, se receber o erro ACS50001: a terceira parte confiável com o nome ' [Realm] ' não foi encontrada, aguarde alguns minutos e tente executar o aplicativo novamente.
9. Quando solicitado, faça logon como um usuário em seu diretório:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Agora você fez logon com êxito no seu aplicativo hospedado do Azure usando a autenticação do Windows Azure.

     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Problemas conhecidos

#### <a name="role-based-authorization-fails-when-using-windows-azure-authentication"></a>A autorização baseada em função falha ao usar a autenticação do Windows Azure

No momento, a autenticação do Windows Azure não fornece a declaração de função necessária para que a autorização baseada em função possa ser executada. A função do usuário autenticado deve ser recuperada manualmente do Windows Azure Active Directory.

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-sts"></a>Navegar até um aplicativo com a autenticação do Windows Azure resulta no erro "ACS20016 o domínio do usuário conectado (live.com) não corresponde a nenhum domínio permitido deste STS"

Se você já estiver conectado a uma conta da Microsoft (por exemplo, hotmail.com, live.com, outlook.com) e tentar acessar um aplicativo que habilitou a autenticação do Windows Azure, poderá receber uma resposta de erro 400, pois o domínio da sua conta da Microsoft Não é reconhecido pelo Windows Azure Active Directory. Para fazer logon no aplicativo, faça logoff da sua conta da Microsoft primeiro.

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificate"></a>Fazer logon em um aplicativo com a autenticação do Windows Azure habilitada e um X509CertificateValidationMode diferente de nenhum resulta em erros de validação de certificado para o certificado accounts.accesscontrol.windows.net

A validação de certificado não é necessária e deve ser deixada desabilitada. A impressão digital do certificado do emissor é validada pelo WSFederationAuthenticationModule.

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-sts"></a>Ao tentar habilitar a autenticação do Windows Azure, a caixa de diálogo de autenticação da Web mostra o erro "ACS20016: o domínio do usuário conectado (contoso.onmicrosoft.com) não corresponde a nenhum domínio permitido deste STS."

Você poderá ver esse erro quando tiver feito logon com êxito usando uma conta diferente do Windows Azure Active Directory de dentro do mesmo processo do Visual Studio. Faça logoff da conta especificada ou reinicie o Visual Studio. Se você fez logon anteriormente e selecionou a opção para "manter-me conectado", talvez seja necessário limpar os cookies do navegador.

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message"></a>ACS20012: a solicitação não é uma mensagem de protocolo WS-Federation válida

Isso pode acontecer se você já estiver conectado com alguma outra ID da Microsoft a um dos serviços do Azure. Use a janela particular do navegador, como InPrivate no IE ou Incognito no Chrome, ou limpe todos os cookies.

## <a name="additional-resources"></a>Recursos adicionais

- [Microsoft ASP.net Tools for Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci
- [Recursos do Windows Azure: identidade](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: desenvolver aplicativos para sua organização](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: desenvolver aplicativos para várias organizações](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Como implementar o logon único com o Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Logon único com o Windows Azure Active Directory: um](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) aprofundamento – Vittorio Bertocci
- [Use AD FS 2,0 para implementar e gerenciar o logon único](https://technet.microsoft.com/library/jj205462.aspx)
