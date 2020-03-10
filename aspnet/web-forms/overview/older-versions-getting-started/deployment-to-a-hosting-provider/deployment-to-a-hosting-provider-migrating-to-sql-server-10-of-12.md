---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 'Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio ou o Visual Web Developer: migrando para o SQL Server-10 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: c5281a42596d95e725b32e652c75785abe0fd64e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78573461"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio ou o Visual Web Developer: migrando para o SQL Server-10 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Studio 2012 RC ou o Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial da série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar SQL Server edições diferentes de SQL Server Compact e mostra como implantar o Azure App aplicativos Web do serviço, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Visão geral

Este tutorial mostra como migrar de SQL Server Compact para SQL Server. Um motivo para você fazer isso é aproveitar os recursos de SQL Server aos quais SQL Server Compact não oferece suporte, como procedimentos armazenados, gatilhos, exibições ou replicação. Para obter mais informações sobre as diferenças entre SQL Server Compact e SQL Server, consulte o tutorial [Implantando SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) .

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express versus SQL Server completo para desenvolvimento

Depois de decidir atualizar para SQL Server, talvez você queira usar SQL Server ou SQL Server Express em seus ambientes de desenvolvimento e teste. Além das diferenças no suporte a ferramentas e nos recursos do mecanismo de banco de dados, há diferenças nas implementações de provedor entre SQL Server Compact e outras versões do SQL Server. Essas diferenças podem fazer com que o mesmo código gere resultados diferentes. Portanto, se você decidir manter SQL Server Compact como seu banco de dados de desenvolvimento, deverá testar exaustivamente seu site em SQL Server ou SQL Server Express em um ambiente de teste antes de cada implantação na produção.

Ao contrário de SQL Server Compact, o SQL Server Express é essencialmente o mesmo mecanismo de banco de dados e usa o mesmo provedor .NET que o SQL Server completo. Ao testar com SQL Server Express, você pode ter certeza de obter os mesmos resultados que usará SQL Server. Você pode usar a maioria das mesmas ferramentas de banco de dados com SQL Server Express que você pode usar com SQL Server (uma exceção notável sendo [SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)) e oferece suporte a outros recursos de SQL Server como procedimentos armazenados, exibições, gatilhos e replicação. (No entanto, normalmente você precisa usar o SQL Server completo em um site de produção. SQL Server Express pode ser executado em um ambiente de hospedagem compartilhado, mas ele não foi projetado para isso, e muitos provedores de hospedagem não dão suporte a ele.)

Se você estiver usando o Visual Studio 2012, você normalmente escolhe SQL Server Express LocalDB para seu ambiente de desenvolvimento porque é o que é instalado por padrão com o Visual Studio. No entanto, o LocalDB não funciona no IIS, portanto, para seu ambiente de teste, você precisa usar SQL Server ou SQL Server Express.

### <a name="combining-databases-versus-keeping-them-separate"></a>Combinando bancos de dados em vez de mantê-los separados

O aplicativo da Contoso University tem dois bancos de dados de SQL Server Compact: o Membership Database (*ASPNET. sdf*) e o banco de dados do aplicativo (*School. sdf*). Quando você migra, é possível migrar esses bancos de dados para dois bancos de dados separados ou para um único. Talvez você queira combiná-los para facilitar as junções de banco de dados entre o banco de dados do aplicativo e o banco de dados de associação. Seu plano de hospedagem também pode fornecer um motivo para combiná-los. Por exemplo, o provedor de hospedagem pode cobrar mais por vários bancos de dados ou até mesmo permitir mais de um banco. Esse é o caso da conta de hospedagem do Cytanium Lite usada para este tutorial, que permite apenas um único banco de dados SQL Server.

Neste tutorial, você migrará seus dois bancos de dados desta forma:

- Migre para dois bancos de dados LocalDB no ambiente de desenvolvimento.
- Migre para dois bancos de dados SQL Server Express no ambiente de teste.
- Migre para um banco de dados de SQL Server completo combinado no ambiente de produção.

Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>Instalando o SQL Server Express

O SQL Server Express é instalado automaticamente por padrão com o Visual Studio 2010, mas, por padrão, ele não é instalado com o Visual Studio 2012. Para instalar o SQL Server 2012 Express, clique no link a seguir

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

Escolha *PTB/x64/SQLEXPR\_x64\_PTB. exe* ou *PTB/x86/SQLEXPR\_x86\_PTB. exe*e, no assistente de instalação, aceite as configurações padrão. Para obter mais informações sobre as opções de instalação, consulte [instalar SQL Server 2012 do assistente de instalação (instalação)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Criando bancos de dados SQL Server Express para o ambiente de teste

A próxima etapa é criar a associação ASP.NET e os bancos de dados escolares.

No menu **Exibir** , selecione **Gerenciador de servidores** (**Gerenciador de banco de dados** no Visual Web Developer) e, em seguida, clique com o botão direito do mouse em **conexões de dados** e selecione **criar novo banco de SQL Server**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

Na caixa de diálogo **criar novo banco de dados SQL Server** , digite ".\sqlexpress" na caixa **nome do servidor** e "ASPNET-Test" na caixa **novo nome do banco de dados** e clique em **OK**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Siga o mesmo procedimento para criar um novo banco de dados SQL Server Express School chamado "School-teste".

(Você está acrescentando "teste" a esses nomes de banco de dados porque, posteriormente, criará uma instância adicional de cada banco de dados para o ambiente de desenvolvimento, e você precisará ser capaz de diferenciar os dois conjuntos de dados.)

**Gerenciador de servidores** agora mostra os dois novos bancos de dados.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Criando um script de concessão para os novos bancos de dados

Quando o aplicativo é executado no IIS em seu computador de desenvolvimento, o aplicativo acessa o banco de dados usando as credenciais do pool de aplicativos padrão. No entanto, por padrão, a identidade do pool de aplicativos não tem permissão para abrir os bancos de dados. Portanto, você precisa executar um script para conceder essa permissão. Nesta seção, você cria o script que será executado posteriormente para garantir que o aplicativo possa abrir os bancos de dados quando ele for executado no IIS.

Na pasta *SolutionFiles* da solução que você criou no tutorial [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) , crie um novo arquivo SQL chamado *Grant. SQL*. Copie os seguintes comandos SQL para o arquivo e, em seguida, salve e feche o arquivo:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Esse script foi projetado para trabalhar com SQL Server 2008 e com as configurações do IIS no Windows 7, já que elas são especificadas neste tutorial. Se você estiver usando uma versão diferente do SQL Server ou do Windows, ou se configurar o IIS em seu computador de forma diferente, as alterações nesse script poderão ser necessárias. Para obter mais informações sobre scripts de SQL Server, consulte [manuais online do SQL Server](https://go.microsoft.com/fwlink/?LinkId=132511).

> [!NOTE] 
> 
> **Observação de segurança** Esse script fornece ao DB\_permissões de proprietário para o usuário que acessa o banco de dados em tempo de execução, que é o que você terá no ambiente de produção. Em alguns cenários, talvez você queira especificar um usuário que tenha permissões completas de atualização de esquema de banco de dados somente para implantação e especificar para tempo de execução um usuário diferente que tenha permissões somente para ler e gravar dados. Para obter mais informações, consulte **revisando as alterações automáticas de Web. config para migrações do Code First** em [implantando no IIS como um ambiente de teste](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).

## <a name="configuring-database-deployment-for-the-test-environment"></a>Configurando a implantação de banco de dados para o ambiente de teste

Em seguida, você configurará o Visual Studio para que ele execute as seguintes tarefas para cada banco de dados:

- Gerar um script SQL que cria a estrutura do banco de dados de origem (tabelas, colunas, restrições, etc.) no banco de dados de destino.
- Gere um script SQL que insira os dados do banco de dado de origem nas tabelas no banco de dados de destino.
- Execute os scripts gerados e o script de concessão que você criou, no banco de dados de destino.

Abra a janela **Propriedades do projeto** e selecione a guia **pacote/publicar SQL** .

Verifique se **ativo (versão)** ou **versão** está selecionado na lista suspensa **configuração** .

Clique em **habilitar esta página**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

A guia **pacote/publicar SQL** normalmente está desabilitada porque especifica um método de implantação herdado. Para a maioria dos cenários, você deve configurar a implantação de banco de dados no assistente de **publicação na Web** . Migrar de SQL Server Compact para SQL Server ou SQL Server Express é um caso especial para o qual esse método é uma boa opção.

Clique em **importar de Web. config**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

O Visual Studio procura cadeias de conexão no arquivo *Web. config* , localiza uma para o banco de dados de associação e outra para o banco de dados escolar e adiciona uma linha correspondente a cada cadeia de conexão na tabela **entradas de banco de dados** . As cadeias de conexão encontradas são para os bancos de dados do SQL Server Compact existentes e a próxima etapa será configurar como e onde implantar esses bancos de dados.

Você insere configurações de implantação de banco de dados na seção **detalhes de entrada de banco** de dados abaixo da tabela entradas de banco de **dados** . As configurações mostradas na seção **detalhes da entrada de banco de dados** pertencem a qualquer linha na tabela entradas de banco de **dados** selecionada, conforme mostrado na ilustração a seguir.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Definindo as configurações de implantação para o banco de dados de associação

Selecione a linha **DefaultConnection-Deployment** na tabela **entradas do banco de dados** para definir as configurações que se aplicam ao banco de dados de associação.

Em **cadeia de conexão para o banco de dados de destino**, insira uma cadeia de conexão que aponte para o novo banco de dados de associação SQL Server Express. Você pode obter a cadeia de conexão necessária em **Gerenciador de servidores**. Em **Gerenciador de servidores**, expanda **Data Connections** e selecione o banco de dados **aspnetTest** e, em seguida, na janela **Propriedades** , copie o valor da **cadeia de conexão** .

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

A mesma cadeia de conexão é reproduzida aqui:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Copie e cole essa cadeia de conexão na **cadeia de conexão para o banco de dados de destino** na guia **empacotar/publicar SQL** .

Certifique-se de que **os dados de pull e/ou o esquema de um banco de dado existente** esteja selecionado. Isso é o que faz com que os scripts SQL sejam gerados e executados automaticamente no banco de dados de destino.

A **cadeia de conexão para o** valor do banco de dados de origem é extraída do arquivo *Web. config* e aponta para o banco de dados de SQL Server Compact de desenvolvimento. Esse é o banco de dados de origem que será usado para gerar os scripts que serão executados posteriormente no banco de dados de destino. Como você deseja implantar a versão de produção do banco de dados, altere "aspnet-Dev. sdf" para "aspnet-Prod. sdf".

Altere **as opções de script de banco** de dados do **esquema somente** para o **esquema e o dado**, já que você deseja copiar seus dados (contas de usuário e funções), bem como a estrutura do banco do dados.

Para configurar a implantação para executar os scripts de concessão que você criou anteriormente, você precisa adicioná-los à seção **scripts de banco de dados** . Clique em **adicionar script**e, na caixa de diálogo **Adicionar scripts SQL** , navegue até a pasta em que você armazenou o script de concessão (essa é a pasta que contém o arquivo de solução). Selecione o arquivo chamado *Grant. SQL*e clique em **abrir**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

As configurações para a linha de **implantação DefaultConnection** nas **entradas do banco de dados** agora são parecidas com a ilustração a seguir:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Definindo as configurações de implantação para o banco de dados escolar

Em seguida, selecione a linha **SchoolContext-Deployment** na tabela **entradas de banco de dados** para definir as configurações de implantação para o banco de dados escolar.

Você pode usar o mesmo método usado anteriormente para obter a cadeia de conexão para o novo banco de dados SQL Server Express. Copie essa cadeia de conexão na **cadeia de conexão para o banco de dados de destino** na guia **empacotar/publicar SQL** .

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Certifique-se de que **os dados de pull e/ou o esquema de um banco de dado existente** esteja selecionado.

A **cadeia de conexão para o** valor do banco de dados de origem é extraída do arquivo *Web. config* e aponta para o banco de dados de SQL Server Compact de desenvolvimento. Altere "School-Dev. sdf" para "School-Prod. sdf" para implantar a versão de produção do banco de dados. (Você nunca criou um arquivo School-Prod. sdf na pasta de dados do\_de aplicativos, portanto, você copiará esse arquivo do ambiente de teste para a pasta de\_de dados do aplicativo na pasta do projeto ContosoUniversity mais tarde.)

Altere **as opções de script de banco** de **dados para esquema e data**.

Você também deseja executar o script para conceder permissão de leitura e gravação para esse banco de dados para a identidade do pool de aplicativos, portanto, adicione o arquivo de script *Grant. SQL* como você fez para o banco de dados de associação.

Quando terminar, as configurações da linha SchoolContext em **entradas do banco de dados** serão parecidas **com** a ilustração a seguir:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Salve as alterações na guia **pacote/publicar SQL** .

Copie o arquivo *School-prod. sdf* da pasta *c:\inetpub\wwwroot\ContosoUniversity\App\_dados* para a pasta *\_dados do aplicativo* no projeto ContosoUniversity.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Especificando o modo transacionado para o script de concessão

O processo de implantação gera scripts que implantam o esquema de banco de dados e os mesmos. Por padrão, esses scripts são executados em uma transação. No entanto, os scripts personalizados (como os scripts de concessão) por padrão não são executados em uma transação. Se o processo de implantação combina modos de transação, você pode obter um erro de tempo limite quando os scripts são executados durante a implantação. Nesta seção, você edita o arquivo de projeto para configurar os scripts personalizados a serem executados em uma transação.

Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **ContosoUniversity** e selecione **descarregar projeto**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

Em seguida, clique com o botão direito do mouse no projeto novamente e selecione **Editar ContosoUniversity. csproj**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

O editor do Visual Studio mostra o conteúdo XML do arquivo de projeto. Observe que há vários elementos `PropertyGroup`. (Na imagem, o conteúdo dos elementos de `PropertyGroup` foi omitido.)

![Janela Editor de arquivo do projeto](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

O primeiro, que não tem `Condition` atributo, é para configurações que se aplicam independentemente da configuração de compilação. Um elemento de `PropertyGroup` aplica-se somente à configuração de compilação de depuração (Observe o atributo `Condition`), que se aplica somente à configuração de Build de versão e um se aplica somente à configuração de compilação de teste. Dentro do elemento `PropertyGroup` para a configuração de Build de versão, você verá um elemento `PublishDatabaseSettings` que contém as configurações inseridas na guia **pacote/publicar SQL** . Há um elemento `Object` que corresponde a cada um dos scripts de concessão que você especificou (Observe as duas instâncias de "Grant. SQL"). Por padrão, o atributo `Transacted` do elemento `Source` para cada script de concessão é `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Altere o valor do atributo `Transacted` do elemento `Source` para `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Salve e feche o arquivo de projeto e clique com o botão direito do mouse no projeto em **Gerenciador de soluções** e selecione **recarregar projeto**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Configurando transformações de Web. config para as cadeias de conexão

As cadeias de conexão para os novos bancos de dados do SQL Express que você inseriu na guia **pacote/publicar SQL** são usadas pelo implantação da Web apenas para atualizar o banco de dados de destino durante a implantação. Você ainda precisa configurar as transformações de *Web. config* para que as cadeias de conexão no arquivo *Web. config* implantado apontem para os novos bancos de dados do SQL Server Express. (Quando você usa a guia **pacote/publicar SQL** , não é possível configurar cadeias de conexão no perfil de publicação.)

Abra *Web. Test. config* e substitua o elemento `connectionStrings` pelo elemento `connectionStrings` no exemplo a seguir. (Certifique-se de copiar apenas o elemento connectionStrings, não o código ao redor que é mostrado aqui para fornecer contexto.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Esse código faz com que os atributos `connectionString` e `providerName` de cada elemento `add` sejam substituídos no arquivo *Web. config* implantado. Essas cadeias de conexão não são idênticas àquelas que você inseriu na guia **pacote/publicar SQL** . A configuração "MultipleActiveResultSets = true" foi adicionada a eles porque é necessário para o Entity Framework e o Provedores Universais.

## <a name="installing-sql-server-compact"></a>Instalando o SQL Server Compact

O pacote NuGet do SqlServerCompact fornece os assemblies do mecanismo de banco de dados SQL Server Compact para o aplicativo da Contoso University. Mas agora ele não é o aplicativo, mas Implantação da Web que deve ser capaz de ler os bancos de dados do SQL Server Compact, a fim de criar scripts para serem executados nos bancos de dados do SQL Server. Para permitir que Implantação da Web Leia SQL Server Compact bancos de dados, instale SQL Server Compact no computador de desenvolvimento usando o seguinte link: [Microsoft SQL Server Compact 4,0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Implantando no ambiente de teste

Para publicar no ambiente de teste, você precisa criar um perfil de publicação configurado para usar a guia **pacote/publicar SQL** para publicação de banco de dados em vez das configurações de banco de dados de perfil de publicação.

Primeiro, exclua o perfil de teste existente.

Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto ContosoUniversity e clique em **publicar**.

Selecione a guia **perfil** .

Clique em **Gerenciar Perfis**.

Selecione **teste**, clique em **remover**e, em seguida, clique em **Fechar**.

Feche o assistente **publicar Web** para salvar essa alteração.

Em seguida, crie um novo perfil de teste e use-o para publicar o projeto.

Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto ContosoUniversity e clique em **publicar**.

Selecione a guia **perfil** .

Selecione **&lt;novo...&gt;** na lista suspensa e insira "teste" como o nome do perfil.

Na caixa **URL do serviço** , digite *localhost*.

Na caixa **site/aplicativo** , digite *Default Web site/ContosoUniversity*.

Na caixa **URL de destino** , digite `http://localhost/ContosoUniversity/`.

Clique em **Próximo**.

A guia **configurações** avisa que a guia **pacote/publicar SQL** foi configurada e oferece a oportunidade de substituí-las clicando em habilitar os novos aprimoramentos de publicação de banco de dados. Para essa implantação, você não deseja substituir as configurações da guia **pacote/publicar do SQL** , portanto, basta clicar em **Avançar**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Uma mensagem na guia **Visualização** indica que **nenhum banco de dados está selecionado para publicação**, mas isso só significa que a publicação do banco de dados não está configurada no perfil de publicação.

Clique em **Publicar**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

O Visual Studio implanta o aplicativo e abre o navegador para a home page do site no ambiente de teste. Execute a página instrutores para ver que ele exibe os mesmos dados que você viu anteriormente. Execute a página **adicionar alunos** , adicione um novo aluno e, em seguida, exiba o novo aluno na página **estudantes** . Isso verifica se você pode atualizar o banco de dados. Selecione a página **Atualizar créditos** (você precisará fazer logon) para verificar se o banco de dados de associação foi implantado e se você tem acesso a ele.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Criando um banco de dados SQL Server para o ambiente de produção

Agora que você implantou o no ambiente de teste, está pronto para configurar a implantação para produção. Você começa como fez no ambiente de teste, criando um banco de dados para o qual implantar. À medida que você se lembra da visão geral, o plano de hospedagem Cytanium Lite só permite um único banco de dados SQL Server, portanto, você configurará apenas um banco de dados, não dois. Todas as tabelas e dados da associação e da escola SQL Server Compact bancos de dados serão implantados em um banco de dados de SQL Server em produção.

Vá para o painel de controle do Cytanium em [http://panel.cytanium.com](http://panel.cytanium.com). Mantenha o mouse sobre **os bancos de dados** e, em seguida, clique em **SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

Na página **SQL Server 2008** , clique em **criar banco de dados**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Nomeie o banco de dados como "School" e clique em **salvar**. (A página adiciona automaticamente o prefixo "contosou", portanto, o nome efetivo será "contosouSchool".)

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

Na mesma página, clique em **criar usuário**. Nos servidores do Cytanium, em vez de usar a segurança integrada do Windows e permitir que a identidade do pool de aplicativos abra seu banco de dados, você criará um usuário com autoridade para abrir o banco de dados. Você adicionará as credenciais do usuário às cadeias de conexão que vão no arquivo *Web. config* de produção. Nesta etapa, você cria essas credenciais.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Preencha os campos obrigatórios na página de **Propriedades do usuário do SQL** :

- Insira "ContosoUniversityUser" como o nome.
- Digite uma senha.
- Selecione **contosouSchool** como o banco de dados padrão.
- Marque a caixa de seleção **contosouSchool** .

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Configurando a implantação de banco de dados para o ambiente de produção

Agora você está pronto para definir as configurações de implantação de banco de dados na guia **pacote/publicar SQL** , como fazia anteriormente para o ambiente de teste.

Abra a janela **Propriedades do projeto** , selecione a guia **pacote/publicar SQL** e verifique se a caixa **ativo (versão)** ou **versão** está selecionada na lista suspensa **configuração** .

Quando você define as configurações de implantação para cada banco de dados, a principal diferença entre o que você faz para ambientes de produção e de teste é a maneira como você configura as cadeias de conexão. Para o ambiente de teste, você inseriu cadeias de conexão de banco de dados de destino diferentes, mas para o ambiente de produção a cadeia de conexão de destino será a mesma para ambos os bancos de dados. Isso ocorre porque você está implantando ambos os bancos de dados em um banco em produção.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Definindo as configurações de implantação para o banco de dados de associação

Para definir as configurações que se aplicam ao banco de dados de associação, selecione a linha **DefaultConnection-Deployment** na tabela **entradas do banco de dados** .

Em **cadeia de conexão para o banco de dados de destino**, insira uma cadeia de conexão que aponte para o novo banco de dados de produção SQL Server que você acabou de criar. Você pode obter a cadeia de conexão do seu email de boas-vindas. A parte relevante do email contém a seguinte cadeia de conexão de exemplo:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Depois de substituir as três variáveis, a cadeia de conexão que você precisa será semelhante a este exemplo:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Copie e cole essa cadeia de conexão na **cadeia de conexão para o banco de dados de destino** na guia **empacotar/publicar SQL** .

Certifique-se de que **os dados de pull e/ou o esquema de um banco de dado existente** ainda estão selecionados e que **as opções de script de banco** de dados ainda são **esquema e data**.

Na caixa **scripts de banco de dados** , desmarque a caixa de seleção ao lado do script Grant. Sql.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Definindo as configurações de implantação para o banco de dados escolar

Em seguida, selecione a linha **SchoolContext-Deployment** na tabela **entradas do banco de dados** para definir as configurações do banco de dados School.

Copie a mesma cadeia de conexão na **cadeia de conexão do banco de dados de destino** que você copiou para esse campo para o banco de dados de associação.

Certifique-se de que **os dados de pull e/ou o esquema de um banco de dado existente** ainda estão selecionados e que **as opções de script de banco** de dados ainda são **esquema e data**.

Na caixa **scripts de banco de dados** , desmarque a caixa de seleção ao lado do script Grant. Sql.

Salve as alterações na guia **pacote/publicar SQL** .

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Configurando transformações de Web. config para as cadeias de conexão para bancos de dados de produção

Em seguida, você configurará as transformações de *Web. config* para que as cadeias de conexão no arquivo *Web. config* implantado apontem para o novo banco de dados de produção. A cadeia de conexão que você inseriu na guia **pacote/publicar SQL** para implantação da Web usar é a mesma que o aplicativo precisa usar, exceto para a adição da opção `MultipleResultSets`.

Abra *Web. Production. config* e substitua o elemento `connectionStrings` por um elemento `connectionStrings` parecido com o exemplo a seguir. (Só Copie o elemento `connectionStrings`, não as marcas ao redor que são fornecidas para mostrar o contexto.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

Às vezes, você vê conselhos que dizem para sempre criptografar cadeias de conexão no arquivo *Web. config* . Isso pode ser apropriado se você estivesse implantando em servidores na rede da sua própria empresa. No entanto, quando você está implantando em um ambiente de hospedagem compartilhado, está confiando nas práticas de segurança do provedor de hospedagem e não é necessário ou prático criptografar as cadeias de conexão.

## <a name="deploying-to-the-production-environment"></a>Implantando no ambiente de produção

Agora você está pronto para implantar na produção. Implantação da Web lerá os bancos de dados de SQL Server Compact no *aplicativo\_data* da pasta do seu projeto e recriará todas as tabelas e os dados do SQL Server de produção. Para publicar usando as configurações de guia **pacote/publicar Web** , você precisa criar um novo perfil de publicação para produção.

Primeiro, exclua o perfil de produção existente como você fez anteriormente o perfil de teste.

Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto ContosoUniversity e clique em **publicar**.

Selecione a guia **perfil** .

Clique em **Gerenciar Perfis**.

Selecione **produção**, clique em **remover**e, em seguida, clique em **Fechar**.

Feche o assistente **publicar Web** para salvar essa alteração.

Em seguida, crie um novo perfil de produção e use-o para publicar o projeto.

Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto ContosoUniversity e clique em **publicar**.

Selecione a guia **perfil** .

Clique em **importar**e selecione o arquivo. publishsettings que você baixou anteriormente.

Na guia **conexão** , altere a **URL de destino** para a URL temporária correta, que neste exemplo é http://contosouniversity.com.vserver01.cytanium.com.

Renomeie o perfil para produção. (Selecione a guia **perfil** e clique em **gerenciar perfis** para fazer isso).

Feche o assistente **publicar Web** para salvar suas alterações.

Em um aplicativo real no qual o banco de dados estava sendo atualizado na produção, você deve executar duas etapas adicionais agora antes de publicar:

1. Carregue o *aplicativo\_offline. htm*, conforme mostrado no tutorial [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) .
2. Use o **recurso Gerenciador de arquivos** do painel de controle Cytanium para copiar os arquivos *ASPNET-prod. sdf* e *School-prod. sdf* do site de produção para a pasta de dados do\_do *aplicativo* do projeto ContosoUniversity. Isso garante que os dados que você está implantando no novo banco de dado SQL Server incluem as atualizações mais recentes feitas pelo seu site de produção.

Na barra de ferramentas de **publicação de um clique da Web** , verifique se o perfil de **produção** está selecionado e clique em **publicar**.

Se você carregou o <em>aplicativo\_offline. htm</em> antes da publicação, será necessário usar o utilitário <strong>Gerenciador de arquivos</strong> no painel de controle Cytanium para excluir o <em>aplicativo\_offline.</em> htm antes de testar. Você também pode excluir os arquivos <em>. sdf</em> da pasta de <em>dados de\_do aplicativo</em> .

Agora você pode abrir um navegador e ir para a URL do seu site público para testar o aplicativo da mesma maneira que fez após a implantação no ambiente de teste.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Alternando para SQL Server Express LocalDB no desenvolvimento

Como foi explicado na visão geral, geralmente é melhor usar o mesmo mecanismo de banco de dados no desenvolvimento que você usa em teste e produção. (Lembre-se de que a vantagem de usar SQL Server Express em desenvolvimento é que o banco de dados funcionará da mesma forma em seus ambientes de desenvolvimento, teste e produção.) Nesta seção, você vai configurar o projeto ContosoUniversity para usar SQL Server Express LocalDB ao executar o aplicativo do Visual Studio.

A maneira mais simples de realizar essa migração é permitir que Code First e o sistema de associação criem novos bancos de dados de desenvolvimento para você. O uso deste método para migrar requer três etapas:

1. Altere as cadeias de conexão para especificar novos bancos de dados SQL Express LocalDB.
2. Execute a ferramenta de administração de site para criar um usuário administrador. Isso cria o banco de dados de associação.
3. Use o comando Migrações do Code First Update-Database para criar e propagar o banco de dados do aplicativo.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Atualizando cadeias de conexão no arquivo Web. config

Abra o arquivo *Web. config* e substitua o elemento `connectionStrings` pelo código a seguir:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Criando o banco de dados de associação

Em **Gerenciador de soluções**, selecione o projeto ContosoUniversity e, em seguida, clique em **configuração do ASP.net** no menu **projeto** .

Selecione a guia Segurança.

Clique em **criar ou gerenciar funções**e crie uma função de **administrador** .

Retorne à guia segurança.

Clique em **criar usuário**e marque a caixa de seleção **administrador** e crie um usuário chamado admin.

Feche a **ferramenta de administração de site**.

### <a name="creating-the-school-database"></a>Criando o banco de dados escolar

Abra a janela do console do Gerenciador de pacotes.

Na lista suspensa **projeto padrão** , selecione o projeto CONTOSOUNIVERSITY. Dal.

Insira o seguinte comando:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Migrações do Code First aplica a migração inicial que cria o banco de dados e, em seguida, aplica a migração adddatadenascimento, ele executa o método semente.

Execute o site pressionando Control-F5. Como você fez para os ambientes de teste e produção, execute a página **adicionar alunos** , adicione um novo aluno e, em seguida, exiba o novo aluno na página **estudantes** . Isso verifica se o banco de dados escolar foi criado e inicializado e se você tem acesso de leitura e gravação a ele.

Selecione a página **Atualizar créditos** e faça logon para verificar se o banco de dados de associação foi implantado e se você tem acesso a ele. Se você não migrou suas contas de usuário, crie uma conta de administrador e, em seguida, selecione a página **Atualizar créditos** para verificar se ela funciona.

## <a name="cleaning-up-sql-server-compact-files"></a>Limpando arquivos SQL Server Compact

Você não precisa mais de arquivos e pacotes NuGet que foram incluídos para dar suporte a SQL Server Compact. Se desejar (essa etapa não é necessária), você poderá limpar os arquivos e as referências desnecessários.

No **Gerenciador de soluções**, exclua os arquivos *. sdf* da pasta *\_dados do aplicativo* e as pastas *AMD64* e *x86* da pasta *bin* .

Em **Gerenciador de soluções**, clique com o botão direito do mouse na solução (não um dos projetos) e clique em **gerenciar pacotes NuGet para solução**.

No painel esquerdo da caixa de diálogo **gerenciar pacotes NuGet** , selecione **pacotes instalados**.

Selecione o pacote **EntityFramework. SqlServerCompact** e clique em **gerenciar**.

Na caixa de diálogo **selecionar projetos** , ambos os projetos são selecionados. Para desinstalar o pacote em ambos os projetos, desmarque ambas as caixas de seleção e clique em **OK**.

Na caixa de diálogo que pergunta se você deseja desinstalar os pacotes dependentes também, clique em não. Um deles é o pacote Entity Framework que você precisa manter.

Siga o mesmo procedimento para desinstalar o pacote **SqlServerCompact** . (Os pacotes devem ser desinstalados nesta ordem porque o pacote **EntityFramework. SqlServerCompact** depende do pacote **SqlServerCompact** .)

Agora você migrou com êxito para SQL Server Express e SQL Server completo. No próximo tutorial, você fará outra alteração no banco de dados e verá como implantar alterações no banco de dados quando seus bancos de dados de teste e produção usarem SQL Server Express e SQL Server completo.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
