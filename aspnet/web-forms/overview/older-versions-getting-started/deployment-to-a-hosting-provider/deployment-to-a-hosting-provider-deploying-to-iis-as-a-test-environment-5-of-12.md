---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio ou o Visual Web Developer: implantando no IIS como um ambiente de teste-5 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5d85232ff2cb229d771d517db7173721c9e277bf
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74633421"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Implantando um aplicativo Web ASP.NET com o SQL Server Compact usando o Visual Studio ou o Visual Web Developer: implantando no IIS como um ambiente de teste-5 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Studio 2012 RC ou o Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial da série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar SQL Server edições diferentes de SQL Server Compact e mostra como implantar o Azure App aplicativos Web do serviço, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

Este tutorial mostra como implantar um aplicativo Web ASP.NET no IIS no computador local.

Ao desenvolver um aplicativo, você geralmente testa executando-o no Visual Studio. Por padrão, isso significa que você está usando o Visual Studio Development Server (também conhecido como Cassini). O Visual Studio Development Server facilita o teste durante o desenvolvimento no Visual Studio, mas não funciona exatamente como o IIS. Como resultado, é possível que um aplicativo seja executado corretamente quando você testá-lo no Visual Studio, mas falhar quando for implantado no IIS em um ambiente de hospedagem.

Você pode testar seu aplicativo de forma mais confiável das seguintes maneiras:

1. Use IIS Express ou IIS completo em vez do Visual Studio Development Server quando você testar no Visual Studio durante o desenvolvimento. Esse método geralmente emula com mais precisão como seu site será executado no IIS. No entanto, esse método não testa seu processo de implantação ou valida que o resultado do processo de implantação será executado corretamente.
2. Implante o aplicativo no IIS em seu computador de desenvolvimento usando o mesmo processo que você usará posteriormente para implantá-lo em seu ambiente de produção. Esse método valida o processo de implantação, além de validar que seu aplicativo será executado corretamente no IIS.
3. Implante o aplicativo em um ambiente de teste que seja o mais próximo possível do seu ambiente de produção. Como o ambiente de produção para esses tutoriais é um provedor de Hospedagem de terceiros, o ambiente de teste ideal seria uma segunda conta com o provedor de hospedagem. Você usaria essa segunda conta apenas para teste, mas ela seria configurada da mesma forma que a conta de produção.

Este tutorial mostra as etapas para a opção 2. As diretrizes para a opção 3 são fornecidas no final do tutorial [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) e, no final deste tutorial, há links para recursos para a opção 1.

Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Configurando o aplicativo para ser executado em confiança média

Antes de instalar o IIS e implantá-lo, você alterará uma configuração de arquivo Web. config para fazer com que o site seja executado mais como em um ambiente de hospedagem compartilhado típico.

Provedores de hospedagem normalmente executam seu site em *confiança média*, o que significa que há algumas coisas que ele não tem permissão para fazer. Por exemplo, o código do aplicativo não pode acessar o registro do Windows e não pode ler ou gravar arquivos que estão fora da hierarquia de pastas do seu aplicativo. Por padrão, o aplicativo é executado em *confiança alta* no computador local, o que significa que o aplicativo pode ser capaz de fazer coisas que falhariam ao implantá-lo na produção. Portanto, para fazer com que o ambiente de teste reflita com mais precisão o ambiente de produção, você configurará o aplicativo para ser executado em confiança média.

No arquivo Web. config do aplicativo, adicione um elemento **Trust** no elemento **System. Web** , conforme mostrado neste exemplo.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

O aplicativo agora será executado em confiança média no IIS, mesmo no computador local. Essa configuração permite que você acompanhe o mais cedo possível qualquer tentativa pelo código do aplicativo de fazer algo que falharia na produção.

> [!NOTE]
> Se você estiver usando Migrações do Entity Framework Code First, verifique se você tem a versão 5,0 ou posterior instalada. No Entity Framework versão 4,3, as migrações exigem confiança total para atualizar o esquema de banco de dados.

## <a name="installing-iis-and-web-deploy"></a>Instalando o IIS e Implantação da Web

Para implantar no IIS em seu computador de desenvolvimento, você deve ter o IIS e o Implantação da Web instalados. Eles não são incluídos na configuração padrão do Windows 7. Se você já tiver instalado o IIS e o Implantação da Web, pule para a próxima seção.

Usar o [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) é a maneira preferida de instalar o IIS e o implantação da Web, porque o Web Platform Installer instala uma configuração recomendada para o IIS e instala automaticamente os pré-requisitos para o iis e implantação da Web, se necessário.

Para executar o Web Platform Installer para instalar o IIS e Implantação da Web, use o link a seguir. Se você já tiver instalado o IIS, Implantação da Web ou qualquer um de seus componentes necessários, o Web Platform Installer instalará apenas o que está faltando.

- [Instalar o IIS e Implantação da Web usando o WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Configurando o pool de aplicativos padrão para o .NET 4

Depois de instalar o IIS, execute o **Gerenciador do IIS** para certificar-se de que o .NET Framework versão 4 seja atribuído ao pool de aplicativos padrão.

No menu **Iniciar** do Windows, selecione **executar**, digite "inetmgr" e clique em **OK**. (Se o comando **executar** não estiver no menu **Iniciar** , você poderá pressionar a tecla Windows e R para abri-lo. Ou clique com o botão direito do mouse na barra de tarefas, clique em **Propriedades**, selecione a guia **menu iniciar** , clique em **Personalizar**e selecione **executar comando**.)

No painel **conexões** , expanda o nó do servidor e selecione **pools de aplicativos**. No painel **pools de aplicativos** , se **DefaultAppPool** for atribuído ao .NET Framework versão 4, como na ilustração a seguir, pule para a próxima seção.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Se você vir apenas dois pools de aplicativos e ambos estiverem definidos como o .NET Framework 2,0, você precisará instalar o ASP.NET 4 no IIS:

- Abra uma janela de prompt de comando clicando com o botão direito do mouse em **prompt de comando** no menu **Iniciar** do Windows e selecionando **Executar como administrador**. Em seguida, execute [aspnet\_regiis. exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) para instalar o ASP.NET 4 no IIS, usando os comandos a seguir. (Em sistemas de 64 bits, substitua "Framework" por "Framework64".)

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP. NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Este comando cria novos pools de aplicativos para o .NET Framework 4, mas o pool de aplicativos padrão ainda será definido como 2,0. Você implantará um aplicativo direcionado ao .NET 4 para esse pool de aplicativos, portanto, você precisa alterar o pool de aplicativos para o .NET 4.

Se você fechou o **Gerenciador do IIS**, execute-o novamente, expanda o nó do servidor e clique em **pools de aplicativos** para exibir o painel **pools de aplicativos** novamente.

No painel **pools de aplicativos** , clique em **DefaultAppPool**e, no painel **ações** , clique em **configurações básicas**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

Na caixa de diálogo **Editar pool de aplicativos** , **altere .NET Framework versão** para **.NET Framework v 4.0.30319** e clique em **OK**.

[![Selecting_. NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Agora você está pronto para publicar no IIS.

## <a name="publishing-to-iis"></a>Publicando no IIS

Há várias maneiras que você pode implantar usando o Visual Studio 2010 e Implantação da Web:

- Use a publicação de um clique do Visual Studio.
- Crie um *pacote de implantação* e instale-o usando a interface do usuário do Gerenciador do IIS. O pacote de implantação consiste em um arquivo *. zip* que contém todos os arquivos e metadados necessários para instalar um site no IIS.
- Crie um pacote de implantação e instale-o usando a linha de comando.

O processo que você passou nos tutoriais anteriores para configurar o Visual Studio para automatizar tarefas de implantação se aplica a todos esses três métodos. Nesses tutoriais, você usará o primeiro desses métodos. Para obter informações sobre como usar pacotes de implantação, consulte [mapa de conteúdo de implantação do ASP.net](https://msdn.microsoft.com/library/bb386521.aspx).

Antes de publicar, verifique se você está executando o Visual Studio no modo de administrador. (No menu **Iniciar** do Windows 7, clique com o botão direito do mouse no ícone da versão do Visual Studio que você está usando e selecione **Executar como administrador**.) O modo de administrador é necessário para publicar somente quando você está publicando no IIS no computador local.

Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto ContosoUniversity (não no projeto CONTOSOUNIVERSITY. Dal) e selecione **publicar**.

O assistente **publicar Web** é exibido.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

Na lista suspensa, selecione **&lt;novo...&gt;** .

Na caixa de diálogo **novo perfil** , digite "teste" e clique em **OK**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Esse nome é o mesmo que o nó do meio do arquivo de transformação Web. Test. config criado anteriormente. Essa correspondência é o que faz com que as transformações Web. Test. config sejam aplicadas quando você publica usando esse perfil.

O assistente avança automaticamente para a guia **conexão** .

Na caixa **URL do serviço** , digite *localhost*.

Na caixa **site/aplicativo** , digite *Default Web site/ContosoUniversity*.

Na caixa **URL de destino** , digite `http://localhost/ContosoUniversity`.

A configuração da **URL de destino** não é necessária. Quando o Visual Studio termina de implantar o aplicativo, ele abre automaticamente o navegador padrão para essa URL. Se você não quiser que o navegador abra automaticamente após a implantação, deixe essa caixa em branco.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Clique em **validar conexão** para verificar se as configurações estão corretas e se você pode se conectar ao IIS no computador local.

Uma marca de seleção verde verifica se a conexão foi bem-sucedida.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Clique em **Avançar** para ir para a guia **configurações** .

A caixa suspensa **configuração** especifica a configuração de compilação a ser implantada. O valor padrão é versão, que é o que você deseja.

Deixe a caixa de seleção **remover arquivos adicionais no destino** desmarcada. Como essa é sua primeira implantação, ainda não haverá nenhum arquivo na pasta de destino.

Na seção **bancos de dados** , insira o seguinte valor na caixa Cadeia de conexão para **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

O processo de implantação colocará essa cadeia de conexão no arquivo Web. config implantado **, porque Use esta cadeia de conexão em tempo de execução** está selecionado.

Também em **SchoolContext**, selecione **aplicar migrações do Code First**. Essa opção faz com que o processo de implantação configure o arquivo Web. config implantado para especificar o inicializador de `MigrateDatabaseToLatestVersion`. Esse inicializador atualiza automaticamente o banco de dados para a versão mais recente quando o aplicativo acessa o banco de dados pela primeira vez após a implantação.

Na caixa Cadeia de conexão para **DefaultConnection**, insira o seguinte valor:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Deixe o **banco de dados de atualização** limpo. O banco de dados de associação será implantado copiando o arquivo. sdf no aplicativo\_data, e você não quer que o processo de implantação faça mais nada com esse banco.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Clique em **Avançar** para ir para a guia **Visualizar** .

Na guia **Visualização** , clique em **Iniciar visualização** para ver uma lista dos arquivos que serão copiados.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Clique em **Publicar**.

Se o Visual Studio não estiver no modo de administrador, você poderá receber uma mensagem de erro que indica um erro de permissões. Nesse caso, feche o Visual Studio, abra-o no modo de administrador e tente publicar novamente.

Se o Visual Studio estiver no modo de administrador, a janela de **saída** relatará compilação e publicação com êxito.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

O navegador é aberto automaticamente na home page da Contoso University em execução no IIS no computador local.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Teste no ambiente de teste

Observe que o indicador de ambiente mostra "(teste)" em vez de "(dev)", que mostra que a transformação *Web. config* do indicador de ambiente foi bem-sucedida.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Execute a página **alunos** para verificar se o banco de dados implantado não tem nenhum aluno. Quando você seleciona essa página, pode levar alguns minutos para carregar porque Code First cria o banco de dados e, em seguida, executa o método `Seed`. (Ele não fazia isso quando você estava na home page porque o aplicativo não tentou acessar o banco de dados ainda.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Execute a página de **instrutores** para verificar se Code First propagam o banco de dados com o instrutor:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Selecione **adicionar alunos** no menu **alunos** , adicione um aluno e, em seguida, exiba o novo aluno na página **alunos** para verificar se você pode gravar com êxito no banco de dados:

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

No menu **cursos** , selecione **Atualizar créditos**. A página **créditos de atualização** requer permissões de administrador, portanto, a página de **logon** é exibida. Insira as credenciais de conta de administrador que você criou anteriormente ("admin" e "Pas $ w0rd"). A página **créditos de atualização** é exibida, que verifica se a conta de administrador que você criou no tutorial anterior foi implantada corretamente no ambiente de teste.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Verifique se existe uma pasta do *ELMAH* com apenas o arquivo de espaço reservado nela.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Revisando as alterações automáticas de Web. config para Migrações do Code First

Abra o arquivo *Web. config* no aplicativo implantado em *C:\inetpub\wwwroot\ContosoUniversity* e você pode ver onde o processo de implantação foi configurado migrações do Code First para atualizar automaticamente o banco de dados para a versão mais recente.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

O processo de implantação também criou uma nova cadeia de conexão para Migrações do Code First usar exclusivamente para atualizar o esquema de banco de dados:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Essa cadeia de conexão adicional permite que você especifique uma conta de usuário para atualizações de esquema de banco de dados e uma conta de usuário diferente para o acesso a aplicativos de aplicativo. Por exemplo, você pode atribuir o banco de\_função de proprietário para Migrações do Code First e DB\_as funções DataReader e DB\_DataWriter para o aplicativo. Esse é um padrão comum de defesa aprofundada que impede que códigos potencialmente mal-intencionados no aplicativo alterem o esquema de banco de dados. (Por exemplo, isso pode acontecer em um ataque de injeção de SQL bem-sucedido.) Esse padrão não é usado por esses tutoriais. Ele não se aplica a SQL Server Compact e não se aplica ao migrar para SQL Server em um tutorial posterior nesta série. O site do Cytanium oferece apenas uma conta de usuário para acessar o banco de dados SQL Server que você cria em Cytanium. Se você for capaz de implementar esse padrão em seu cenário, poderá fazer isso executando as seguintes etapas:

1. Na guia **configurações** do assistente **publicar Web** , insira a cadeia de conexão que especifica um usuário com permissões completas de atualização do esquema de banco de dados e desmarque a caixa de seleção **usar esta cadeia de conexão em tempo de execução** . No arquivo Web. config implantado, isso se torna a cadeia de conexão `DatabasePublish`.
2. Crie uma transformação de arquivo Web. config para a cadeia de conexão que você deseja que o aplicativo use em tempo de execução.

Agora você implantou seu aplicativo no IIS em seu computador de desenvolvimento e o testou lá. Isso verifica se o processo de implantação copiou o conteúdo do aplicativo para o local correto (excluindo os arquivos que você não deseja implantar) e também que Implantação da Web configurado corretamente o IIS durante a implantação. No próximo tutorial, você executará mais um teste que encontra uma tarefa de implantação que ainda não foi feita: definindo permissões de pasta na pasta *ELMAH* .

## <a name="more-information"></a>Mais Informações

Para obter informações sobre como executar o IIS ou IIS Express no Visual Studio, consulte os seguintes recursos:

- [IIS Express visão geral](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) no site IIS.net.
- [Introdução a IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) no blog de Scott Guthrie.
- [Como especificar o servidor Web para projetos Web no Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Principais diferenças entre o IIS e o ASP.NET Development Server](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) no site ASP.net.
- [Teste seu aplicativo ASP.NET MVC ou Web Forms no IIS 7 em 30 segundos](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) no blog de Rick Anderson. Essa entrada fornece exemplos de por que testar com o Visual Studio Development Server (Cassini) não é tão confiável quanto testar em IIS Express e por que testar em IIS Express não é tão confiável quanto testar no IIS.

Para obter informações sobre quais problemas podem surgir quando seu aplicativo é executado em confiança média, consulte [hospedando aplicativos ASP.net em confiança média](http://www.4guysfromrolla.com/articles/100307-1.aspx) no site 4 da equipe de distribuição.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
