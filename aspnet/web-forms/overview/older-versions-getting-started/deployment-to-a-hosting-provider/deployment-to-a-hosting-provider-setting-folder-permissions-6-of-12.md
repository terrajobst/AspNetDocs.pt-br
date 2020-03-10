---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio ou o Visual Web Developer: definindo permissões de pasta-6 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 85a77a196cf3458bbb2e6308838a846936cd070b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630504"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio ou o Visual Web Developer: definindo permissões de pasta-6 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Studio 2012 RC ou o Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial da série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar SQL Server edições diferentes de SQL Server Compact e mostra como implantar o Azure App aplicativos Web do serviço, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Visão geral

Neste tutorial, você definirá permissões de pasta para a pasta *ELMAH* no site da Web implantado para que o aplicativo possa criar arquivos de log nessa pasta.

Quando você testa um aplicativo Web no Visual Studio usando o Visual Studio Development Server (Cassini), o aplicativo é executado sob sua identidade. Você provavelmente é um administrador em seu computador de desenvolvimento e tem autoridade total para fazer qualquer arquivo em qualquer pasta. Mas quando um aplicativo é executado no IIS, ele é executado sob a identidade definida para o pool de aplicativos ao qual o site é atribuído. Normalmente, essa é uma conta definida pelo sistema que tem permissões limitadas. Por padrão, ele tem permissões de leitura e execução nos arquivos e pastas de seu aplicativo Web, mas não tem acesso de gravação.

Isso se tornará um problema se o seu aplicativo criar ou atualizar arquivos, o que é uma necessidade comum em aplicativos Web. No aplicativo Contoso University, o ELMAH cria arquivos XML na pasta *ELMAH* para salvar detalhes sobre erros. Mesmo que você não use algo como o ELMAH, seu site pode permitir que os usuários carreguem arquivos ou executem outras tarefas que gravam dados em uma pasta em seu site.

Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Testando log de erros e relatórios

Para ver como o aplicativo não funciona corretamente no IIS (embora ele tenha feito isso quando você o testou no Visual Studio), você pode causar um erro que normalmente seria registrado pelo ELMAH e, em seguida, abrir o log de erros do ELMAH para ver os detalhes. Se o ELMAH não puder criar um arquivo XML e armazenar os detalhes do erro, você verá um relatório de erros vazio.

Abra um navegador e vá para `http://localhost/ContosoUniversity`e, em seguida, solicite uma URL inválida como *Studentsxxx. aspx*. Você verá uma página de erro gerada pelo sistema em vez da página *GenericErrorPage. aspx* porque a configuração de `customErrors` no arquivo Web. config é "RemoteOnly" e você está executando o IIS localmente:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Agora, execute o *ELMAH. axd* para ver o relatório de erros. Você verá uma página de log de erros vazia porque o ELMAH não pôde criar um arquivo XML na pasta *ELMAH* :

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Definindo a permissão de gravação na pasta do ELMAH

Você pode definir as permissões de pasta manualmente ou pode torná-la uma parte automática do processo de implantação. Torná-lo automático requer código do MSBuild complexo e, como você só precisa fazer isso na primeira vez que implantar, este tutorial mostra apenas como fazê-lo manualmente. (Para obter informações sobre como fazer essa parte do processo de implantação, consulte [definindo permissões de pasta na publicação da Web](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) no blog do sayed Hashimi.)

No **Windows Explorer**, navegue até *C:\inetpub\wwwroot\ContosoUniversity*. Clique com o botão direito do mouse na pasta *ELMAH* , selecione **Propriedades**e, em seguida, selecione a guia **segurança** .

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Se você não vir **DefaultAppPool** na lista de **nomes de usuário ou grupo** , provavelmente usou algum outro método do que aquele especificado neste tutorial para configurar o IIS e o ASP.NET 4 no seu computador. Nesse caso, descubra qual identidade é usada pelo pool de aplicativos atribuído ao aplicativo da Contoso University e conceda permissão de gravação para essa identidade. Consulte os links sobre as identidades do pool de aplicativos no final deste tutorial.

Clique em **Editar**. Na caixa de diálogo **permissões para o ELMAH** , **selecione DefaultAppPool**e, em seguida, marque a caixa de seleção **gravar** na coluna **permitir** .

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Clique em **OK** em ambas as caixas de diálogo.

## <a name="retesting-error-logging-and-reporting"></a>Retestando log de erros e relatórios

Teste causando um erro novamente da mesma maneira (solicite uma URL inadequada) e execute a página **log de erros** . Desta vez, o erro aparecerá na página.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

Você também precisa de permissão de gravação na pasta de *dados do\_de aplicativos* porque você tem SQL Server Compact arquivos de banco de dados nessa pasta e deseja ser capaz de atualizar os dados nesses bancos. Nesse caso, no entanto, você não precisa fazer nada extra porque o processo de implantação define automaticamente a permissão de gravação na pasta de *dados do aplicativo\_* .

Agora você concluiu todas as tarefas necessárias para obter a Contoso University funcionando corretamente no IIS no computador local. No próximo tutorial, você tornará o site publicamente disponível implantando-o em um provedor de hospedagem.

## <a name="more-information"></a>Mais informações

Neste exemplo, o motivo pelo qual o ELMAH não conseguiu salvar arquivos de log era razoavelmente óbvio. Você pode usar o rastreamento do IIS nos casos em que a causa do problema não é tão óbvia; consulte [Solucionando problemas de solicitações com falha usando o rastreamento no IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) no site do IIS.net.

Para obter mais informações sobre como conceder permissões a identidades do pool de aplicativos, consulte [identidades do pool de aplicativos](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) e [conteúdo seguro no IIS por meio de ACLs do sistema de arquivos](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) no site do IIS.net.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
