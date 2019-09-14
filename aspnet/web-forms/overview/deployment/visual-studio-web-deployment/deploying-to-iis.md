---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Implantação da Web do ASP.NET usando o Visual Studio: Implantando no teste | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros, por usin...
ms.author: riande
ms.date: 01/16/2019
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: c45003325832258466a787bc589bf40e844248a2
ms.sourcegitcommit: 4b324a11131e38f920126066b94ff478aa9927f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70985850"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Implantação da Web do ASP.NET usando o Visual Studio: Implantação para teste

Por [Tom Dykstra](https://github.com/tdykstra)

Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros usando o Visual Studio 2017. Para obter informações sobre a série, consulte [o primeiro tutorial da série](introduction.md).

Para obter uma versão atual da implantação no Azure, consulte [criar um aplicativo web ASP.NET Core no Azure](/azure/app-service/app-service-web-get-started-dotnet).

## <a name="overview"></a>Visão geral

Neste tutorial, você implantará um aplicativo Web ASP.NET no IIS (servidor de informações da Internet) em seu computador local.

Geralmente, quando você desenvolve um aplicativo, você o executa e o testa no Visual Studio. Por padrão, os projetos de aplicativos Web no Visual Studio 2017 usam IIS Express como o servidor Web de desenvolvimento. IIS Express se comporta mais como o IIS completo do que o Visual Studio Development Server (também conhecido como Cassini), que o Visual Studio 2017 usa por padrão. Mas nenhum servidor Web de desenvolvimento funciona exatamente como o IIS. Consequentemente, um aplicativo poderia executar e testar corretamente no Visual Studio, mas falhar quando for implantado no IIS.

Você pode testar de forma confiável seu aplicativo de duas maneiras:

1. Implante seu aplicativo no IIS em seu computador de desenvolvimento usando o mesmo processo que você usará posteriormente para implantá-lo em seu ambiente de produção.

   Você pode configurar o Visual Studio para usar o IIS ao executar um projeto Web, mas isso não testaria o processo de implantação. Esse método valida o processo de implantação e se o aplicativo é executado corretamente no IIS.

2. Implante seu aplicativo em um ambiente de teste semelhante ao seu ambiente de produção. 
 
   O ambiente de produção para esses tutoriais são aplicativos Web no serviço Azure App. O ambiente de teste ideal é um aplicativo Web adicional criado no serviço do Azure. Embora seja configurado da mesma forma que um aplicativo Web de produção, você o usaria apenas para teste.

A opção 2 é a maneira mais confiável de testar. Se você usar a opção 2, não precisará necessariamente usar a opção 1. No entanto, se você estiver implantando em um provedor de Hospedagem de terceiros, a opção 2 pode não ser viável ou pode ser cara, portanto, essa série de tutoriais mostra os dois métodos. As diretrizes para a opção 2 são fornecidas no tutorial [implantando no ambiente de produção](deploying-to-production.md) .

Para obter mais informações sobre como usar servidores Web no Visual Studio, consulte [servidores Web no Visual Studio para projetos Web do ASP.net](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Funcionário Se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](troubleshooting.md).

## <a name="download-the-contoso-university-starter-project"></a>Baixe o projeto inicial da Contoso University

Baixe e instale a solução e o projeto de início do Contoso University Visual Studio. Esta solução contém o tutorial concluído. 

[Baixar o projeto inicial](http://go.microsoft.com/fwlink/p/?LinkId=282627)

## <a name="install-iis"></a>Instalar o IIS

Para implantar no IIS em seu computador de desenvolvimento, confirme se o IIS e o Implantação da Web estão instalados. Por padrão, o Visual Studio instala o Implantação da Web, mas o IIS não está incluído na configuração padrão do Windows 10, Windows 8 ou Windows 7. Se você já tiver instalado o IIS e o pool de aplicativos padrão já estiver definido como .NET 4, pule para [a próxima seção](#sqlexpress).

1. É recomendável usar o [Web Platform Installer (WPI)](https://www.microsoft.com/web/downloads/platform.aspx) para instalar o IIS e implantação da Web. WPI instala uma configuração do IIS recomendada que inclui o IIS e Implantação da Web pré-requisitos, se necessário.

   Se você já tiver instalado o IIS, Implantação da Web ou qualquer um de seus componentes necessários, o WPI instalará apenas o que está faltando.

   * Use o Web Platform Installer para instalar o IIS e o Implantação da Web:
    
     ![Instalar o IIS usando WPI](deploying-to-iis/_static/image30.png)

     ![Instalar Implantação da Web usando WPI](deploying-to-iis/_static/image31.png)

     Você verá mensagens indicando que o IIS 7 será instalado. O link funciona para o IIS 8 no Windows 8; Mas para o Windows 8 e posterior, siga as etapas a seguir para verificar se o ASP.NET 4,7 está instalado:

   * Abra **o painel** > de controle**programas** > programas**e recursos** > **Ativar ou desativar recursos do Windows**.

   * Expanda **serviços de informações da Internet**, **serviços de World Wide Web**e recursos de **desenvolvimento de aplicativos**.
   
   * Confirme se **ASP.NET 4,7** está selecionado.

     ![Selecione ASP.NET 4,7](deploying-to-iis/_static/image1a.png)

   * Confirme se a **World Wide Web serviços** e o **console de gerenciamento do IIS** está selecionado. Isso instala o IIS e o Gerenciador do IIS.
    
     ![Selecionar Serviços World Wide Webs](deploying-to-iis/_static/image24.png)    
  
   * Selecione **OK**. Mensagens da caixa de diálogo indicando que a instalação está ocorrendo são exibidas.

Depois de instalar o IIS, execute o **Gerenciador do IIS** para certificar-se de que o .NET Framework versão 4 seja atribuído ao pool de aplicativos padrão.

1. Pressione WINDOWS + R para abrir a caixa de diálogo **executar** .

   (No Windows 8 ou posterior, insira "executar" na página **inicial** . No Windows 7, selecione **executar** no menu **Iniciar** . Se **executar** não estiver no menu **Iniciar** , clique com o botão direito do mouse na barra de tarefas, selecione **Propriedades**, selecione a guia **menu iniciar** , selecione **Personalizar**e selecione **executar comando**.)

2. Digite "inetmgr" e selecione **OK**.

3. No painel **conexões** , expanda o nó do servidor e selecione **pools de aplicativos**. No painel **pools de aplicativos** , se **DefaultAppPool** for atribuído ao .NET Framework versão 4, como na ilustração a seguir, pule para a próxima seção.

   ![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image5a.png)

4. Se você vir apenas dois pools de aplicativos e ambos estiverem definidos como .NET Framework 2,0, instale o ASP.NET 4 no IIS.

   Para o Windows 8 ou posterior, consulte as instruções da seção anterior para verificar se o ASP.NET 4,7 está instalado ou confira [como instalar o ASP.NET 4,5 no Windows 8 e no Windows Server 2012](https://support.microsoft.com/kb/2736284). Para o Windows 7, abra uma janela de prompt de comando clicando com o botão direito do mouse em **prompt de comando** no menu **Iniciar** do Windows e selecionando **Executar como administrador**. Execute [ASPNET\_regiis. exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) para instalar o ASP.NET 4 no IIS usando os comandos a seguir. (Em sistemas de 32 bits, substitua "Framework64" por "Framework".)

   [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

   Este comando cria novos pools de aplicativos para o .NET Framework 4, mas o pool de aplicativos padrão permanecerá definido como 2,0. Você está implantando um aplicativo que tem como alvo o .NET 4 para esse pool de aplicativos, portanto, altere o pool de aplicativos para o .NET 4.

5. Se você fechou o **Gerenciador do IIS**, execute-o novamente, expanda o nó do servidor e selecione **pools de aplicativos**.

6. No painel **pools de aplicativos** , selecione **DefaultAppPool**. No painel **ações** , selecione **configurações básicas**.

   ![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image25.png)

7. Na caixa de diálogo **Editar pool de aplicativos** , altere a **versão .NET CLR** para **.NET CLR v 4.0.30319**. Selecione **OK**.

   ![Selecting_. NET _4_for_DefaultAppPool](deploying-to-iis/_static/image6a.png)

Agora você está pronto para publicar um aplicativo Web no IIS. No entanto, primeiro crie bancos de dados para teste.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Instalar SQL Server Express

O LocalDB não foi projetado para funcionar no IIS, portanto, o ambiente de teste precisa ter SQL Server Express instalado. Se você estiver usando o Visual Studio 2010 SQL Server Express, ele já estará instalado por padrão. Se você estiver usando o Visual Studio 2012 ou posterior, instale SQL Server Express.

Para instalar o SQL Server Express, baixe e instale- [o no centro de download: Microsoft SQL Server 2017 Express Edition](https://www.microsoft.com/sql-server/sql-server-editions-express). 

Na primeira página da central de instalação do SQL Server, selecione **novo SQL Server instalação autônoma ou adicionar recursos a uma instalação existente** e siga as instruções que aceitam as opções padrão. No assistente de instalação, aceite as configurações padrão. Para obter mais informações sobre as opções de instalação, consulte [instalar o SQL Server do assistente de instalação (instalação)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Criar bancos de dados SQL Server Express para o ambiente de teste

O aplicativo Contoso University tem dois bancos de dados: 

1. Banco de dados de associação 
2. Banco de dados do aplicativo 

Você pode implantar esses bancos de dados em dois bancos de dados separados ou em um único banco. A combinação delas torna as junções do banco de dados entre elas mais facilmente. 

Se você estiver implantando em um provedor de Hospedagem de terceiros, seu plano de hospedagem também poderá fornecer um motivo para combiná-los. Por exemplo, o provedor pode cobrar mais por vários bancos de dados ou pode nem mesmo permitir mais de um banco.

Neste tutorial, você implantará em dois bancos de dados no ambiente de teste e em um para um dos ambientes de preparo e produção.

No menu **Exibir** no Visual Studio, selecione **Gerenciador de servidores** (**Gerenciador de banco de dados** no Visual Web Developer). Clique com o botão direito do mouse em **conexões de dados** e selecione **criar novo banco de SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

Na caixa de diálogo **criar novo banco de dados SQL Server** , digite ".\sqlexpress" na caixa **nome do servidor** e "ASPNET-ContosoUniversity" na caixa **novo nome do banco de dados** . Selecione **OK**.

![Criar ASPNET-ContosoUniversity](deploying-to-iis/_static/image9.png)

Siga o mesmo procedimento para criar um novo banco de dados SQL Server Express `ContosoUniversity`School chamado.

**Gerenciador de servidores** mostra os dois novos bancos de dados.

![Novos bancos de dados no Gerenciador de Servidores](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Criar um script de concessão para os novos bancos de dados

Quando o aplicativo é executado no IIS em seu computador de desenvolvimento, o aplicativo usa as credenciais do pool de aplicativos padrão para acessar o banco de dados. No entanto, por padrão, o pool de aplicativos não tem permissão para abrir os bancos de dados. Isso significa que você precisa executar um script para conceder essa permissão. Nesta seção, você criará esse script e o executará mais tarde para garantir que o aplicativo possa abrir os bancos de dados quando ele for executado no IIS.

Em um editor de texto, copie os seguintes comandos SQL em um novo arquivo e salve-o como *Grant. SQL*. 

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

No Visual Studio, abra a solução Contoso University. Clique com o botão direito do mouse na solução (não um dos projetos) e selecione **Adicionar**. Selecione **Item existente**, navegue até *Grant. SQL*e abra-o.

> [!NOTE]
> Esse script foi projetado para trabalhar com o SQL Server Express 2012 ou posterior e com as configurações do IIS no Windows 10, Windows 8 ou Windows 7, já que elas são especificadas neste tutorial. Se você estiver usando uma versão diferente do SQL Server ou do Windows, ou se configurar o IIS em seu computador de forma diferente, as alterações nesse script poderão ser necessárias. Para obter mais informações sobre scripts de SQL Server, consulte [manuais online do SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).

> [!NOTE] 
> **Observação de segurança** Esse script concede `db_owner` permissões ao usuário que acessa o banco de dados em tempo de execução, que é o que você terá no ambiente de produção. Em alguns cenários, talvez você queira especificar um usuário que tenha permissões completas de atualização de esquema de banco de dados somente para implantação e especificar para tempo de execução um usuário diferente que tenha permissões somente para ler e gravar dados. Para obter mais informações, consulte [revisando as alterações automáticas de Web. config para migrações do Code First](#reviewingmigrations) mais adiante neste tutorial.

<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Executar o script de concessão no banco de dados do aplicativo

Você pode configurar o perfil de publicação para executar o script de concessão no banco de dados de associação durante a implantação, pois essa implantação de banco de dados usa o provedor [dbDacFx](https://docs.microsoft.com/iis/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing) . Você não pode executar scripts durante a implantação do Migrações do Code First, que é como você está implantando o banco de dados do aplicativo. Isso significa que você precisa executar o script manualmente antes da implantação no banco de dados do aplicativo.

1. No Visual Studio, abra o arquivo *Grant. SQL* que você criou anteriormente.

2. Selecione **conectar**. 

    ![Botão conectar](deploying-to-iis/_static/image11.png)

3. Na caixa de diálogo **conectar ao servidor** , digite *.\sqlexpress* como o **nome do servidor**. Selecione **conectar**.

4. Na lista suspensa banco de dados, selecione **ContosoUniversity**. Selecione **executar**. 

   ![](deploying-to-iis/_static/image12.png)

A identidade do pool de aplicativos padrão agora tem permissões suficientes no banco de dados de aplicativo para Migrações do Code First para criar as tabelas de banco de dados quando o aplicativo é executado.

## <a name="publish-to-iis"></a>Publicar no IIS

Há várias maneiras que você pode implantar no IIS usando o Visual Studio e Implantação da Web:

* Use a publicação de um clique do Visual Studio.
* Publicar a partir da linha de comando.
* Crie um pacote de implantação e instale-o usando o Gerenciador do IIS. O pacote tem um arquivo. zip com todos os arquivos e metadados necessários para instalar um site no IIS.
* Crie um pacote de implantação e instale-o usando a linha de comando.

O processo que você passou nos tutoriais anteriores para configurar o Visual Studio para automatizar tarefas de implantação se aplica a todos esses métodos. Nesses tutoriais, você usará os dois primeiros métodos. Para obter informações sobre como usar pacotes de implantação, consulte [implantando um aplicativo Web Criando e instalando um pacote de implantação da Web](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) no mapa de conteúdo da implantação da Web para Visual Studio e ASP.net.

Antes de publicar, verifique se você está executando o Visual Studio no modo de administrador. Se você não vir **(administrador)** na barra de título, feche o Visual Studio. Na página **inicial** do Windows 8 (ou posterior) ou no menu **Iniciar** do Windows 7, clique com o botão direito do mouse no ícone do Visual Studio e selecione **Executar como administrador**. O modo de administrador só é necessário para publicação quando você está publicando no IIS no computador local.

### <a name="create-the-publish-profile"></a>Criar o perfil de publicação

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **ContosoUniversity** (não no projeto **ContosoUniversity. Dal** ). Selecione **Publicar**. A página **publicar** é exibida.

2. Selecione **novo perfil**. A caixa de diálogo **selecionar um destino de publicação** é exibida.

3. Selecione **IIS, FTP, etc**. Selecione **Criar Perfil**. O assistente de **publicação** é exibido.

   ![Guia perfil do assistente publicar Web](deploying-to-iis/_static/image26.png)

4. No menu suspenso **método de publicação** , selecione **implantação da Web**.

5. Para **servidor**, digite *localhost*.

6. Para **nome do site**, digite *Default Web site/ContosoUniversity*.

7. Para **URL de destino**, *http://localhost/ContosoUniversity* insira.

   A configuração da **URL de destino** não é necessária. Quando o Visual Studio termina de implantar o aplicativo, ele abre automaticamente o navegador padrão para essa URL. Se você não quiser que o navegador abra automaticamente após a implantação, deixe essa caixa em branco.

8. Selecione **validar conexão** para verificar se as configurações estão corretas e se você pode se conectar ao IIS no computador local.

   Uma marca de seleção verde verifica se a conexão foi bem-sucedida.

   ![Guia de conexão do assistente publicar Web](deploying-to-iis/_static/image27.png)

9. Selecione **Avançar** para ir para a guia **configurações** .

10. A caixa suspensa **configuração** especifica a configuração de compilação a ser implantada. Deixe-o definido como o valor padrão de **versão**. Você não implantará compilações de depuração neste tutorial.

11. Expanda **Opções de publicação de arquivo**. Selecione **Excluir arquivos na pasta dados\_do aplicativo**.

    No ambiente de teste, o aplicativo acessa os bancos de dados que você criou na instância de SQL Server Express local, não os arquivos. MDF na pasta de *dados\_do aplicativo* .

12. Deixe as caixas de seleção **pré-compilar durante a publicação** e **remover arquivos adicionais no destino** desmarcadas.

    ![Opções de publicação de arquivo na guia Configurações](deploying-to-iis/_static/image15a.png)

    A pré-compilação é uma opção útil principalmente para sites grandes. Ele pode reduzir o tempo de inicialização na primeira vez que uma página for solicitada depois que o site for publicado.

    Você não precisa remover arquivos adicionais, pois essa é sua primeira implantação e não haverá arquivos na pasta de destino ainda.

    > [!NOTE] 
    > Se você selecionar **remover arquivos adicionais no destino** para uma implantação subsequente no mesmo site, certifique-se de usar o recurso de visualização para ver com antecedência quais arquivos serão excluídos antes da implantação. O comportamento esperado é que Implantação da Web excluirá os arquivos no servidor de destino que você excluiu em seu projeto. No entanto, toda a estrutura de pastas nas pastas de origem e destino é comparada; e, em alguns cenários, Implantação da Web pode excluir arquivos que você não deseja excluir.
    > 
    > Por exemplo, se você tiver um aplicativo Web em uma subpasta no servidor ao implantar um projeto na pasta raiz, a subpasta será excluída. Você pode ter um projeto para o site principal em contoso.com e outro projeto para um blog em contoso.com/blog. O aplicativo de blog está em uma subpasta. Se você selecionar **remover arquivos adicionais no destino** quando implantar o site principal, o aplicativo de blog será excluído.
    > 
    > Para outro exemplo, sua pasta\_de dados de aplicativo pode ser excluída inesperadamente. Certos bancos de dados, como SQL Server Compact armazenam arquivos de banco de\_dados na pasta Data app. Após a implantação inicial, você não deseja continuar copiando os arquivos de banco de dados em implantações subsequentes, portanto, selecione **excluir dados do aplicativo\_** na guia pacote/publicar Web. Depois de fazer isso, se você tiver **removido arquivos adicionais no destino** selecionado, os arquivos de banco de\_dados e a pasta Data de aplicativo serão excluídos da próxima vez que você publicar.

### <a name="configure-deployment-for-the-membership-database"></a>Configurar a implantação para o banco de dados de associação

As etapas a seguir se aplicam ao banco de dados **DefaultConnection** na seção **databases** da caixa de diálogo.

1. Na caixa **cadeia de conexão remota** , digite a seguinte cadeia de conexão que aponta para o novo banco de dados de associação SQL Server Express.

   [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

   O processo de implantação coloca essa cadeia de conexão no arquivo Web. config implantado porque **Use esta cadeia de conexão em tempo de execução** está selecionado.

    Você também pode obter a cadeia de conexão de **Gerenciador de servidores**. Em **Gerenciador de servidores**, expanda **Data Connections** e selecione o banco de  **&lt;&gt;dados MachineName \SQLEXPRESS.AspNet-ContosoUniversity** e, em seguida, na janela **Propriedades** , copie a **cadeia de conexão** valor. Essa cadeia de conexão terá uma configuração adicional que você pode excluir: `Pooling=False`.

2. Selecione **Atualizar banco de dados**.

   Isso faz com que o esquema de banco de dados seja criado no banco de dados de destino durante a implantação. Nas próximas etapas, você especificará os scripts adicionais que precisa executar: um para conceder acesso ao banco de dados para o pool de aplicativos padrão e um para implantá-los.

3. Selecione **Configurar atualizações de banco de dados**.
 
4. Na caixa de diálogo **Configurar atualizações de banco de dados** , selecione **adicionar script SQL**. Navegue até o script *Grant. SQL* que você salvou anteriormente na pasta da solução.

5. Repita o processo para adicionar o script *ASPNET-data-dev. SQL* .

   ![Configurar atualizações de banco de dados para banco de dados de associação](deploying-to-iis/_static/image16.png)

6. Selecione **Fechar**.

### <a name="configure-deployment-for-the-application-database"></a>Configurar a implantação para o banco de dados do aplicativo

Quando o Visual Studio detecta uma `DbContext` classe de Entity Framework, ele cria uma entrada na seção **bancos** de dados que tem uma caixa de seleção **executar migrações do Code First** em vez de uma caixa de seleção **Atualizar banco de dados** . Para este tutorial, você usará essa caixa de seleção para especificar Migrações do Code First implantação.

Em alguns cenários, você pode estar usando um `DbContext` banco de dados, mas deseja usar o provedor dbDacFx em vez de migrações para implantar o banco de dados. Nesse caso, consulte [como fazer implantar um banco de dados Code First sem migrações?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) nas perguntas frequentes sobre implantação da Web do ASP.net no msdn.

As etapas a seguir se aplicam ao banco de dados **SchoolContext** na seção **databases** da caixa de diálogo.

1. Na caixa **cadeia de conexão remota** , digite a seguinte cadeia de conexão que aponta para o novo SQL Server Express banco de dados de aplicativo.

   [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

   O processo de implantação coloca essa cadeia de conexão no arquivo Web. config implantado porque **Use esta cadeia de conexão em tempo de execução** está selecionado.

   Você também pode obter a cadeia de conexão do banco de dados do aplicativo de **Gerenciador de servidores** da mesma forma que obtém a cadeia de conexão do banco de dados de associação.

2. Selecione **executar migrações do Code First (é executado no início do aplicativo)** .

   Essa opção faz com que o processo de implantação configure o arquivo Web. config implantado para especificar o `MigrateDatabaseToLatestVersion` inicializador. Esse inicializador atualiza automaticamente o banco de dados para a versão mais recente quando o aplicativo acessa o banco de dados pela primeira vez após a implantação.

### <a name="configure-publish-profile-transforms"></a>Configurar transformações de perfil de publicação

1. Selecione **Fechar**. Selecione **Sim** quando for perguntado se deseja salvar as alterações.

2. Em **Gerenciador de soluções**, expanda **Propriedades**, expanda **PublishProfiles**.

3. Clique com o botão direito em *CustomProfile. pubxml* e renomeie-o como *Test. pubxml*.

4. Clique com o botão direito do mouse em *Test. pubxml*. Selecione **Adicionar configuração transformar**.

   ![Adicionar menu de transformação de configuração](deploying-to-iis/_static/image17.png)

   O Visual Studio cria o arquivo de transformação *Web. Test. config* e o abre.

5. No arquivo de transformação *Web. Test. config* , insira o código a seguir imediatamente após a marca de configuração de abertura.

   [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Quando você usa o perfil de publicação de teste, essa transformação define o indicador de ambiente como "teste". No site implantado, você verá "(teste)" após o título H1 "Contoso University".

6. Salve e feche o arquivo.

7. Clique com o botão direito do mouse no arquivo *Web. Test. config* e selecione **Visualizar transformação** para certificar-se de que a transformação codificada produz as alterações esperadas.

    A janela de **Visualização Web. config** mostra o resultado da aplicação das transformações *Web. Release. config* e das transformações *Web. Test. config* .

### <a name="preview-the-deployment-updates"></a>Visualizar as atualizações de implantação

1. Abra o assistente **publicar Web** novamente (clique com o botão direito do mouse no projeto ContosoUniversity, selecione **publicar**e, em seguida, **Visualizar**).

2. Na caixa de diálogo **Visualização** , selecione **Iniciar visualização** para ver uma lista dos arquivos que serão copiados. 

   ![Visualização da publicação](deploying-to-iis/_static/image29.png)

   Você também pode selecionar o link **Visualizar banco de dados** para ver os scripts que serão executados no banco de dados de associação. (Nenhum script é executado para Migrações do Code First implantação, portanto, não há nada a ser visualizado para o banco de dados do aplicativo.)

3. Selecione **Publicar**.

   Se o Visual Studio não estiver no modo de administrador, você poderá receber uma mensagem de erro de permissões. Nesse caso, feche o Visual Studio, abra-o no modo de administrador e tente publicar novamente.

   Se o Visual Studio estiver no modo de administrador, a janela de **saída** relatará compilação e publicação com êxito.

   ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

   Se você inseriu a URL na caixa **URL de destino** na guia publicar **conexão** de perfil, o navegador será aberto automaticamente na Home Page Contoso University em execução no IIS em seu computador.

## <a name="test-in-the-test-environment"></a>Testar no ambiente de teste

Observe que o indicador de ambiente mostra "(teste)" em vez de "(dev)", que mostra que a transformação *Web. config* do indicador de ambiente foi bem-sucedida.

Execute a página de **instrutores** para verificar se Code First propagam o banco de dados com os dados do instrutor. Quando você seleciona essa página, pode levar alguns minutos para carregar porque Code First cria o banco de dados e, em seguida `Seed` , executa o método. (Ele não fazia isso quando você estava na home page porque o aplicativo não tentou acessar o banco de dados ainda.)

Selecione a guia **alunos** para verificar se o banco de dados implantado não tem nenhum aluno.

Selecione **adicionar alunos** no menu **alunos** . Adicione um aluno e, em seguida, exiba o novo aluno na página **estudantes** . Isso verifica se você pode gravar com êxito no banco de dados.

No menu **cursos** , selecione **Atualizar créditos**. A página **créditos de atualização** requer permissões de administrador, portanto, a página de **logon** é exibida. Insira as credenciais de conta de administrador que você criou anteriormente ("admin" e "devpwd"). A página **créditos de atualização** é exibida. Isso verifica se a conta de administrador que você criou no tutorial anterior foi implantada corretamente no ambiente de teste.

Verifique se existe uma pasta do *ELMAH* na pasta *c:\inetpub\wwwroot\ContosoUniversity* somente com o arquivo de espaço reservado nela.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Examine as alterações automáticas de Web. config para Migrações do Code First

Abra o arquivo *Web. config* no aplicativo implantado em *C:\inetpub\wwwroot\ContosoUniversity* e você pode ver onde o processo de implantação foi configurado migrações do Code First para atualizar automaticamente o banco de dados para a versão mais recente.

![](deploying-to-iis/_static/image21.png)

O processo de implantação também criou uma nova cadeia de conexão para Migrações do Code First usar exclusivamente para atualizar o esquema de banco de dados:

![Cadeia de conexão Database_Publish](deploying-to-iis/_static/image22.png)

Essa cadeia de conexão adicional permite que você especifique uma conta de usuário para atualizações de esquema de banco de dados e uma conta de usuário diferente para acesso a dados de aplicativo. Por exemplo, você pode atribuir a função de **proprietário do banco de BD\_** para migrações do Code First e o DataReader do **BD\_** com as funções do **BD\_DataWriter** ao aplicativo. Esse é um padrão comum de defesa aprofundada que impede que códigos potencialmente mal-intencionados no aplicativo alterem o esquema de banco de dados. (Por exemplo, isso pode acontecer em um ataque de injeção de SQL bem-sucedido.) Esses tutoriais não usam esse padrão. Para implementar esse padrão em seu cenário, siga estas etapas:

1. No assistente **publicar Web** na guia **configurações** , insira a cadeia de conexão que especifica um usuário com permissões completas de atualização do esquema de banco de dados. Desmarque a caixa de seleção **usar esta cadeia de conexão em tempo de execução** . No arquivo Web. config implantado, isso se `DatabasePublish` torna a cadeia de conexão.

2. Crie uma transformação de arquivo Web. config para a cadeia de conexão que você deseja que o aplicativo use em tempo de execução.

## <a name="summary"></a>Resumo

Agora você implantou seu aplicativo no IIS em seu computador de desenvolvimento e o testou lá.

![Home Page no teste](deploying-to-iis/_static/image23.png)

Isso verifica se o processo de implantação copiou o conteúdo do aplicativo para o local correto (excluindo os arquivos que você não deseja implantar) e também que Implantação da Web configurado corretamente o IIS durante a implantação. No próximo tutorial, você executará mais um teste que encontra uma tarefa de implantação que ainda não foi feita: definindo permissões de pasta na pasta *Elm Ah* .

## <a name="more-information"></a>Mais informações

Para obter informações sobre como executar o IIS ou IIS Express no Visual Studio, consulte os seguintes recursos:

- [IIS Express visão geral](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) no site IIS.net.
- [Introdução a IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) no blog de Scott Guthrie.
- [Servidores Web no Visual Studio para projetos Web do ASP.net](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Principais diferenças entre o IIS e o ASP.NET Development Server](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) no site ASP.net.

Para obter informações sobre quais problemas podem surgir quando seu aplicativo é executado em confiança média, consulte [hospedando aplicativos ASP.net em confiança média](http://www.4guysfromrolla.com/articles/100307-1.aspx) nos quatro profissionais do site do Rollo.

> [!div class="step-by-step"]
> [Anterior](project-properties.md)
> [Próximo](setting-folder-permissions.md)
