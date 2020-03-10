---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Desenvolvendo aplicativos ASP.NET com o Azure Active Directory-ASP.NET 4. x
author: Rick-Anderson
description: Microsoft ASP.NET Tools for Azure Active Directory simplifica a habilitação da autenticação para aplicativos Web hospedados no Azure. Você pode usar o Azure autenti...
ms.author: riande
ms.date: 08/14/2014
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 28425ea8d1312dfc6e14df9677396f2cbcf6f16d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583849"
---
# <a name="developing-aspnet-apps-with-azure-active-directory"></a>Desenvolver aplicativos do ASP.NET com o Azure Active Directory

por [Rick Anderson](https://twitter.com/RickAndMSFT)

As ferramentas de Microsoft ASP.NET para Azure Active Directory simplificam a habilitação da autenticação para aplicativos Web hospedados no [Azure](https://www.windowsazure.com/home/features/web-sites/). Você pode usar a autenticação do Azure para autenticar usuários do Office 365 de sua organização, contas corporativas sincronizadas do seu Active Directory local ou usuários criados em seu próprio domínio de Azure Active Directory personalizado. Habilitar a autenticação do Windows Azure configura seu aplicativo para autenticar usuários usando um único locatário [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) .

Este tutorial mostrará como criar um aplicativo ASP.NET configurado para logon com o [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (AD do Azure). Você também aprenderá a chamar o API do Graph para obter informações sobre o usuário conectado no momento e como implantar o aplicativo no Azure.

## <a name="prerequisites"></a>Prerequisites

1. [Visual Studio Express 2013 para Web](https://my.visualstudio.com/Downloads?q=visual%20studio%202013#d-2013-express) ou [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -a atualização 3 ou superior é necessária.
3. Uma conta do Azure. [Clique aqui](https://azure.microsoft.com/pricing/free-trial/) para obter uma avaliação gratuita se você ainda não tiver uma conta.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Adicionar um administrador global à sua Active Directory

1. Entre no [Portal de Gerenciamento do Azure](https://manage.windowsazure.com/).
2. Todas as contas do Azure contêm um **diretório padrão** --clique nele e, em seguida, clique na guia **usuários** na parte superior da página (consulte a imagem abaixo).
3. Clique em Adicionar usuário.
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Crie um novo usuário com a função de **administrador global** . Clique em **usuários** no menu superior e, em seguida, clique no botão **Adicionar usuário** na barra de comandos.
5. Na caixa de diálogo **Adicionar usuário** , insira um nome para o novo usuário e clique na seta para a direita.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Insira o nome de usuário e defina a função como **administrador global**. Os administradores globais exigem um endereço de email alternativo para fins de recuperação de senha. Depois de terminar, clique na seta para a direita.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. Na próxima página da caixa de diálogo, clique em **criar**. Uma senha temporária será criada para o novo usuário e exibida na caixa de diálogo.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)

   Salve a senha, será necessário alterar a senha após o primeiro logon. A imagem a seguir mostra a nova conta de administrador. Você deve usar o Azure Active Directory para fazer logon em seu aplicativo, não a conta Microsoft também mostrada nesta página.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Criar um aplicativo ASP.NET

As etapas a seguir usam [Visual Studio Express 2013 para Web](https://www.microsoft.com/download/details.aspx?id=40747)e requer [Visual Studio 2013 atualização 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. No Visual Studio, clique em **arquivo** e em **novo projeto**. Na caixa de diálogo **novo projeto** , selecione o C# projeto Web Visual no menu à esquerda e clique em **OK**. Talvez você também queira desmarcar a **Application insights adicionar ao projeto** se não quiser a funcionalidade do seu aplicativo.
2. Na caixa de diálogo **novo projeto ASP.net** , selecione **MVC**e clique em **alterar autenticação**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. Na caixa de diálogo **alterar autenticação** , selecione **contas organizacionais**. Essas opções podem ser usadas para registrar automaticamente seu aplicativo no Azure AD, bem como configurar automaticamente seu aplicativo para integração com o Azure AD. Você não precisa usar a caixa de diálogo **alterar autenticação** para registrar e configurar seu aplicativo, mas isso torna muito mais fácil. Se você estiver usando o Visual Studio 2012, por exemplo, você ainda poderá registrar manualmente o aplicativo no Portal de Gerenciamento do Azure e atualizar sua configuração para integração com o Azure AD.
   Nos menus suspensos, selecione **organização única de nuvem** e **logon único, ler dados do diretório**. Insira o domínio para seu diretório do Azure AD, por exemplo (nas imagens abaixo) *aricka0yahoo.onmicrosoft.com*e clique em **OK**. Você pode obter o nome de domínio na guia domínios para o diretório padrão no portal do Azure (consulte a próxima imagem para baixo).

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)

   A imagem a seguir mostra o nome de domínio do portal do Azure.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)

    > [!NOTE]
    > Opcionalmente, você pode configurar o URI de ID do aplicativo que será registrado no Azure AD clicando em **mais opções**. O URI da ID do aplicativo é o identificador exclusivo de um aplicativo, que é registrado no Azure AD e usado pelo aplicativo para se identificar ao se comunicar com o Azure AD. Para obter mais informações sobre o URI da ID do aplicativo e outras propriedades de aplicativos registrados, consulte [Este tópico](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Ao clicar na caixa de seleção abaixo do campo URI da ID do aplicativo, você também pode optar por substituir um registro existente no Azure AD que usa o mesmo URI da ID do aplicativo.
4. Depois de clicar em **OK**, uma caixa de diálogo de entrada será exibida e você precisará entrar usando uma conta de administrador global (não a conta Microsoft associada à sua assinatura). Se você criou uma nova conta de administrador anteriormente, será necessário alterar a senha e entrar novamente usando a nova senha.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Depois de autenticado com êxito, a caixa de diálogo **novo projeto ASP.net** mostrará sua opção de autenticação (**organizacional** ) e o diretório em que o novo aplicativo será registrado (*aricka0yahoo.onmicrosoft.com* na imagem abaixo). Abaixo dessas informações, marque a caixa de seleção chamado **host na nuvem**. Se essa caixa de seleção estiver selecionada, o projeto será provisionado como um aplicativo Web do Azure e será habilitado para publicação fácil posteriormente. Clique em **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. A caixa de diálogo **configurar site do Azure** será exibida, usando um nome de site e uma região gerados automaticamente. Observe também a conta à qual você está conectado no momento na caixa de diálogo. Você deseja certificar-se de que essa conta é aquela à qual sua assinatura do Azure está anexada, normalmente um conta Microsoft.

    > [!NOTE]
    > Este projeto requer um banco de dados. Você precisa selecionar um dos bancos de dados existentes ou criar um novo. Um banco de dados é necessário porque o projeto já usa um arquivo de banco de dado local para armazenar uma pequena quantidade de data de configuração de autenticação. Quando você implanta o aplicativo em um site do Azure, esse banco de dados não é empacotado com a implantação, portanto, você precisa escolher um que seja acessível na nuvem. Clique em **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. O projeto será criado e suas opções de autenticação e opções de aplicativo Web serão configuradas automaticamente com o projeto. Depois que esse processo for concluído, execute o projeto localmente pressionando **^ F5**. Você será solicitado a entrar usando sua conta institucional. Forneça o nome de usuário e a senha para a conta que você criou anteriormente e clique em **entrar**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Após a entrada bem-sucedida, o site ASP.NET mostrará que você autenticou exibindo o nome de usuário no canto superior direito da página.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)

   Se você receber o erro: o valor não pode ser nulo ou vazio. Nome do parâmetro: linkText ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)

   consulte a seção de [depuração](#dbg) no final do tutorial.

## <a name="basics-of-the-graph-api"></a>Noções básicas do API do Graph

[O API do Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx) é a interface programática usada para executar CRUD e outras operações em objetos em seu diretório do AD do Azure. Se você selecionar uma opção de conta organizacional para autenticação ao criar um novo projeto no Visual Studio 2013, seu aplicativo já estará configurado para chamar o API do Graph. Esta seção mostra brevemente como o API do Graph funciona.

1. Em seu aplicativo em execução, clique no nome do usuário conectado no canto superior direito da página. Isso o levará para a página de perfil do usuário, que é uma ação no controlador doméstico. Você observará que a tabela contém informações de usuário sobre a conta de administrador que você criou anteriormente. Essas informações são armazenadas em seu diretório, e o API do Graph é chamado para recuperar essas informações quando a página é carregada.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Volte para o Visual Studio e expanda a pasta **controladores** e, em seguida, abra o arquivo **HomeController.cs** . Você verá uma ação **USERPROFILE ()** que contém o código para recuperar um token e, em seguida, chamar o API do Graph. Este código está duplicado abaixo:

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Para chamar o API do Graph, primeiro você precisa recuperar um token. Quando o token é recuperado, seu valor de cadeia de caracteres deve ser anexado no cabeçalho de autorização para todas as solicitações subsequentes ao API do Graph. A maior parte do código acima trata dos detalhes da autenticação no Azure AD para obter um token, usando o token para fazer uma chamada para o API do Graph e, em seguida, transformar a resposta para que ela possa ser apresentada na exibição.

    A parte mais relevante para discussão é a seguinte linha realçada: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Essa linha representa o nome do usuário, que foi desserializado da resposta JSON e é apresentado na exibição.

    Você pode chamar o API do Graph usando HttpClient e manipular os dados brutos por conta própria, mas uma maneira mais fácil é usar a [biblioteca de cliente do Graph que está disponível via NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). A biblioteca de cliente manipula as solicitações HTTP brutas e a transformação dos dados retornados para você e torna muito mais fácil trabalhar com o API do Graph em um ambiente .NET. Consulte os exemplos de código de API do Graph relacionados no [github.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Implantar o Aplicativo no Azure

As etapas a seguir mostrarão como implantar o aplicativo no Azure. Nas etapas anteriores, você conectou seu novo projeto com um aplicativo Web no Azure, portanto, ele está pronto para ser publicado em apenas algumas etapas.

1. No Visual Studio, clique com o botão direito do mouse no projeto e selecione **publicar**. A caixa de diálogo **publicar Web** será exibida com cada configuração já configurada. Clique no botão **Avançar** para ir para a página **configurações** . Você pode ser solicitado a autenticar; Certifique-se de autenticar usando sua conta de assinatura do Azure (normalmente uma conta Microsoft) e não a conta institucional que você criou anteriormente.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Marque a opção **habilitar autenticação organizacional** . No campo **domínio** , insira o domínio para seu diretório. Na lista suspensa **nível de acesso** , selecione **logon único, ler dados do diretório**. Você observará que o banco de dados anterior que você usou já está populado **na seção banco de dados.** Clique em **Publicar**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. O Visual Studio começará a implantar seu site e uma nova janela do navegador será exibida. Você pode ser solicitado a autenticar em seu diretório novamente. Depois de autenticado, você será redirecionado para seu site publicado recentemente no Azure.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Depurando o aplicativo

Se você receber o seguinte erro: o valor não pode ser nulo ou vazio. Nome do parâmetro: linkText

![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)

Substitua o código no arquivo *Views\Shared\\_LoginPartial. cshtml* pelo seguinte:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Depois de executar o aplicativo, se o usuário conectado Mostrar "usuário nulo", saia e entre novamente com a conta de Active Directory que você criou anteriormente.

Um tutorial excelente a ser seguido é o aprofundamento de Rick Rainey [: sites do Azure e autenticação organizacional usando o Azure ad](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Mais informações

- [Análise aprofundada: sites do Azure e autenticação organizacional usando o Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Visão geral do Azure AD API do Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Cenários de autenticação no Azure AD](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Exemplos de código do Azure AD no GitHub](https://github.com/AzureADSamples)
