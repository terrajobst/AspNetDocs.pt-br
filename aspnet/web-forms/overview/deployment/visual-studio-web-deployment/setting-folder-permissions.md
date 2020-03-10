---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Implantação da Web do ASP.NET usando o Visual Studio: definindo permissões de pasta | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros, por usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 410525bb2e3f6e5a0be6d7d6b33fb3a40509041a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78576058"
---
# <a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Implantação da Web do ASP.NET usando o Visual Studio: definindo permissões de pasta

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros usando o Visual Studio 2012 ou o Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial da série](introduction.md).

## <a name="overview"></a>Visão geral

Neste tutorial, você definirá permissões de pasta para a pasta *ELMAH* no site da Web implantado para que o aplicativo possa criar arquivos de log nessa pasta.

Quando você testa um aplicativo Web no Visual Studio usando o Visual Studio Development Server (Cassini) ou IIS Express, o aplicativo é executado sob sua identidade. Você provavelmente é um administrador em seu computador de desenvolvimento e tem autoridade total para fazer qualquer arquivo em qualquer pasta. Mas quando um aplicativo é executado no IIS, ele é executado sob a identidade definida para o pool de aplicativos ao qual o site é atribuído. Normalmente, essa é uma conta definida pelo sistema que tem permissões limitadas. Por padrão, ele tem permissões de leitura e execução nos arquivos e pastas de seu aplicativo Web, mas não tem acesso de gravação.

Isso se tornará um problema se o seu aplicativo criar ou atualizar arquivos, o que é uma necessidade comum em aplicativos Web. No aplicativo Contoso University, o ELMAH cria arquivos XML na pasta *ELMAH* para salvar detalhes sobre erros. Mesmo que você não use algo como o ELMAH, seu site pode permitir que os usuários carreguem arquivos ou executem outras tarefas que gravam dados em uma pasta em seu site.

Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Log e relatório de erros de teste

Para ver como o aplicativo não funciona corretamente no IIS (embora ele tenha feito isso quando você o testou no Visual Studio), você pode causar um erro que normalmente seria registrado pelo ELMAH e, em seguida, abrir o log de erros do ELMAH para ver os detalhes. Se o ELMAH não puder criar um arquivo XML e armazenar os detalhes do erro, você verá um relatório de erros vazio.

Abra um navegador e vá para `http://localhost/ContosoUniversity`e, em seguida, solicite uma URL inválida como *Studentsxxx. aspx*. Você verá uma página de erro gerada pelo sistema em vez da página *GenericErrorPage. aspx* porque a configuração de `customErrors` no arquivo Web. config é "RemoteOnly" e você está executando o IIS localmente:

![Página de erro HTTP 404](setting-folder-permissions/_static/image1.png)

Agora, execute o *ELMAH. axd* para ver o relatório de erros. Depois de fazer logon com as credenciais de conta de administrador (&quot;&quot; de administração e &quot;devpwd&quot;), você verá uma página de log de erros vazia porque o ELMAH não pôde criar um arquivo XML na pasta *ELMAH* :

![Log de erros vazio](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Definir permissão de gravação na pasta do ELMAH

Você pode definir as permissões de pasta manualmente ou pode torná-la uma parte automática do processo de implantação. Torná-lo automático requer código do MSBuild complexo e, como você só precisa fazer isso na primeira vez que implantar o, as etapas a seguir fazem como fazer isso manualmente. (Para obter informações sobre como fazer essa parte do processo de implantação, consulte [definindo permissões de pasta na publicação da Web](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) no blog do sayed Hashimi.)

1. No **Explorador de arquivos**, navegue até *C:\inetpub\wwwroot\ContosoUniversity*. Clique com o botão direito do mouse na pasta *ELMAH* , selecione **Propriedades**e, em seguida, selecione a guia **segurança** .
2. Clique em **Editar**.
3. Na caixa de diálogo **permissões para o ELMAH** , **selecione DefaultAppPool**e, em seguida, marque a caixa de seleção **gravar** na coluna **permitir** .

    ![Permissões para a pasta ELMAH](setting-folder-permissions/_static/image3.png)

    (Se você não vir **DefaultAppPool** na lista de **nomes de usuário ou grupo** , provavelmente usou algum outro método do que aquele especificado neste tutorial para configurar o IIS e o ASP.NET 4 no seu computador. Nesse caso, descubra qual identidade é usada pelo pool de aplicativos atribuído ao aplicativo da Contoso University e conceda permissão de gravação para essa identidade. Consulte os links sobre as identidades do pool de aplicativos no final deste tutorial. Clique em **OK** em ambas as caixas de diálogo.

## <a name="retest-error-logging-and-reporting"></a>Retestar log de erros e relatórios

Teste causando um erro novamente da mesma maneira (solicite uma URL inadequada) e execute a página **log de erros** . Desta vez, o erro aparecerá na página.

![Página de log de erros do ELMAH](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Resumo

Agora você concluiu todas as tarefas necessárias para obter a Contoso University funcionando corretamente no IIS no computador local. No próximo tutorial, você tornará o site publicamente disponível implantando-o no Azure.

## <a name="more-information"></a>Mais informações

Neste exemplo, o motivo pelo qual o ELMAH não conseguiu salvar arquivos de log era razoavelmente óbvio. Você pode usar o rastreamento do IIS nos casos em que a causa do problema não é tão óbvia; consulte [Solucionando problemas de solicitações com falha usando o rastreamento no IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) no site do IIS.net.

Para obter mais informações sobre como conceder permissões a identidades do pool de aplicativos, consulte [identidades do pool de aplicativos](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) e [conteúdo seguro no IIS por meio de ACLs do sistema de arquivos](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) no site do IIS.net.

> [!div class="step-by-step"]
> [Anterior](deploying-to-iis.md)
> [Próximo](deploying-to-production.md)
