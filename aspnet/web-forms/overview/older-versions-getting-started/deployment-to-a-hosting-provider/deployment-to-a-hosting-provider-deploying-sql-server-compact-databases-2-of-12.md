---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 'Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio ou o Visual Web Developer: Implantando bancos de dados SQL Server Compact-2 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 56ceabc79947967846d342354fd033510be5f05a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74625518"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>Implantando um aplicativo Web ASP.NET com o SQL Server Compact usando o Visual Studio ou o Visual Web Developer: Implantando bancos de dados do SQL Server Compact-2 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Studio 2012 RC ou o Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial da série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar SQL Server edições diferentes de SQL Server Compact e mostra como implantar o Azure App aplicativos Web do serviço, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

Este tutorial mostra como configurar dois bancos de dados do SQL Server Compact e o mecanismo de banco de dados para implantação.

Para acesso ao banco de dados, o aplicativo da Contoso University requer o seguinte software que deve ser implantado com o aplicativo porque não está incluído no .NET Framework:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) (o mecanismo de banco de dados).
- [Provedores universais ASP.net](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (que permite que o sistema de associação do ASP.NET use SQL Server Compact)
- [Entity Framework 5,0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)(Code First com migrações).

A estrutura do banco de dados e alguns (nem todos) dos mesmos dos dois bancos de dado do aplicativo também devem ser implantados. Normalmente, à medida que você desenvolve um aplicativo, você insere dados de teste em um banco de dado que você não deseja implantar em um site ativo. No entanto, você também pode inserir alguns dados de produção que deseja implantar. Neste tutorial, você configurará o projeto Contoso University para que o software necessário e os dados corretos sejam incluídos quando você implantar o.

Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact versus SQL Server Express

O aplicativo de exemplo usa SQL Server Compact 4,0. Esse mecanismo de banco de dados é uma opção relativamente nova para sites; as versões anteriores do SQL Server Compact não funcionam em um ambiente de hospedagem na Web. O SQL Server Compact oferece alguns benefícios em comparação com o cenário mais comum de desenvolvimento com SQL Server Express e implantação em SQL Server completo. Dependendo do provedor de hospedagem que você escolher, SQL Server Compact pode ser mais barato implantar, pois alguns provedores cobram extra para dar suporte a um banco de dados completo SQL Server. Não há custo adicional para SQL Server Compact porque você pode implantar o próprio mecanismo de banco de dados como parte do seu aplicativo Web.

No entanto, você também deve estar ciente de suas limitações. SQL Server Compact não dá suporte a procedimentos armazenados, gatilhos, exibições ou replicação. (Para obter uma lista completa de recursos de SQL Server que não têm suporte do SQL Server Compact, consulte [diferenças entre SQL Server Compact e SQL Server](https://msdn.microsoft.com/library/bb896140.aspx).) Além disso, algumas das ferramentas que você pode usar para manipular esquemas e dados em SQL Server Express e SQL Server bancos de dado não funcionam com SQL Server Compact. Por exemplo, você não pode usar SQL Server Management Studio ou SQL Server Data Tools no Visual Studio com bancos de dados do SQL Server Compact. Você tem outras opções para trabalhar com bancos de dados do SQL Server Compact:

- Você pode usar Gerenciador de Servidores no Visual Studio, que oferece funcionalidade limitada de manipulação de banco de dados para SQL Server Compact.
- Você pode usar o recurso de manipulação de banco de dados do [WebMatrix](https://www.microsoft.com/web/webmatrix/), que tem mais recursos do que Gerenciador de servidores.
- Você pode usar ferramentas de terceiros ou software livre com recursos relativamente completos, como a caixa de [ferramentas SQL Server Compact](https://github.com/ErikEJ/SqlCeToolbox) e o [Utilitário de script do esquema e de dados do SQL Compact](https://github.com/ErikEJ/SqlCeToolbox).
- Você pode escrever e executar seus próprios scripts DDL (linguagem de definição de dados) para manipular o esquema de banco de dado.

Você pode começar com SQL Server Compact e depois atualizar mais tarde conforme suas necessidades evoluem. Os tutoriais mais recentes desta série mostram como migrar de SQL Server Compact para SQL Server Express e para SQL Server. No entanto, se você estiver criando um novo aplicativo e precisar de SQL Server em um futuro próximo, provavelmente será melhor começar com SQL Server ou SQL Server Express.

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>Configurando o Mecanismo de Banco de Dados de SQL Server Compact para implantação

O software necessário para acesso a dados no aplicativo da Contoso University foi adicionado por meio da instalação dos seguintes pacotes NuGet:

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System. Web. Providers](http://nuget.org/List/Packages/System.Web.Providers) (provedores universais ASP.net)
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework. SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

Os links apontam para as versões atuais desses pacotes, que podem ser mais recentes do que o que está instalado no projeto inicial que você baixou para este tutorial. Para implantação em um provedor de hospedagem, certifique-se de usar Entity Framework 5,0 ou posterior. As versões anteriores do Migrações do Code First exigem confiança total e em muitos provedores de hospedagem, seu aplicativo será executado em confiança média. Para obter mais informações sobre confiança média, consulte o tutorial [implantando o IIS como um ambiente de teste](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) .

A instalação do pacote NuGet geralmente cuida de tudo que você precisa para implantar esse software com o aplicativo. Em alguns casos, isso envolve tarefas como alterar o arquivo Web. config e adicionar scripts do PowerShell que são executados sempre que você cria a solução. **Se você quiser adicionar suporte a qualquer um desses recursos (como SQL Server Compact e Entity Framework) sem usar o NuGet, certifique-se de que você sabe o que a instalação do pacote NuGet faz para que você possa fazer o mesmo trabalho manualmente.**

Há uma exceção em que o NuGet não cuida de tudo que você precisa fazer para garantir uma implantação bem-sucedida. O pacote NuGet do SqlServerCompact adiciona um script de pós-compilação ao seu projeto que copia os assemblies nativos para as subpastas *x86* e *AMD64* na pasta *bin* do projeto, mas o script não inclui essas pastas no projeto. Como resultado, Implantação da Web não os copiará para o site de destino, a menos que você os inclua manualmente no projeto. (Esse comportamento resulta da configuração de implantação padrão; outra opção, que você não usará nesses tutoriais, é alterar a configuração que controla esse comportamento. A configuração que você pode alterar é **apenas os arquivos necessários para executar o aplicativo** em **itens a serem implantados** na guia **pacote/publicar Web** da janela **Propriedades do projeto** . A alteração dessa configuração geralmente não é recomendada porque pode resultar na implantação de muitos outros arquivos para o ambiente de produção do que o necessário. Para obter mais informações sobre as alternativas, consulte o tutorial [configurando as propriedades do projeto](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md) .)

Compile o projeto e, em **Gerenciador de soluções** clique em **Mostrar todos os arquivos** se ainda não tiver feito isso. Talvez você também precise clicar em **Atualizar**.

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

Expanda a pasta **bin** para ver as pastas **AMD64** e **x86** e, em seguida, selecione essas pastas, clique com o botão direito do mouse e selecione **incluir no projeto**.

![amd64_and_x86_in_Solution_Explorer. png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

Os ícones de pasta são alterados para mostrar que a pasta foi incluída no projeto.

![Solution_Explorer_amd64_included. png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>Configurando Migrações do Code First para implantação de banco de dados de aplicativo

Quando você implanta um banco de dados de aplicativo, normalmente você não simplesmente implanta seu banco de dados de desenvolvimento com todos eles na produção, porque grande parte dos dados nele é provavelmente apenas para fins de teste. Por exemplo, os nomes de aluno em um banco de dados de teste são fictícios. Por outro lado, geralmente não é possível implantar apenas a estrutura do banco de dados sem nenhum dado. Alguns dos dados no banco de dado de teste podem ser dados reais e devem estar lá quando os usuários começam a usar o aplicativo. Por exemplo, seu banco de dados pode ter uma tabela que contém valores de nível válido ou nomes de departamento reais.

Para simular esse cenário comum, você configurará um método de semente Migrações do Code First que é inserido no banco de dados somente para aqueles que você deseja que estejam na produção. Esse método de semente não inserirá dados de teste porque ele será executado na produção depois que Code First criar o banco de dado em produção.

Em versões anteriores do Code First antes da liberação de migrações, era comum que os métodos de semente inserissem dados de teste também, porque com cada alteração de modelo durante o desenvolvimento, era preciso excluir completamente e recriar a partir do zero. Com o Migrações do Code First, os dados de teste são retidos após as alterações do banco de dados, de modo que não é necessário, inclusive, o teste do método semente. O projeto que você baixou usa o método de pré-migração de inclusão de todos os dados no método semente de uma classe de inicializador. Neste tutorial, você desabilitará a classe do inicializador e habilitará as migrações. Em seguida, você atualizará o método semente na classe de configuração de migrações para que ele insira apenas os dados que você deseja inserir na produção.

O diagrama a seguir ilustra o esquema do banco de dados do aplicativo:

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

Para esses tutoriais, você presumirá que as tabelas `Student` e `Enrollment` devem estar vazias quando o site for implantado pela primeira vez. As outras tabelas contêm dados que têm que ser pré-carregados quando o aplicativo é colocado em tempo real.

Como você usará Migrações do Code First, você não precisa mais usar o inicializador de Code First **DropCreateDatabaseIfModelChanges** . O código para esse inicializador está no arquivo SchoolInitializer.cs no projeto ContosoUniversity. DAL. Uma configuração no elemento **appSettings** do arquivo Web. config faz com que esse inicializador seja executado sempre que o aplicativo tentar acessar o banco de dados pela primeira vez:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

Abra o arquivo Web. config do aplicativo e remova o elemento que especifica a classe do inicializador Code First do elemento appSettings. O elemento appSettings agora tem a seguinte aparência:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> Outra maneira de especificar uma classe de inicializador é fazer isso chamando `Database.SetInitializer` no método `Application_Start` no arquivo *global. asax* . Se você estiver habilitando migrações em um projeto que usa esse método para especificar o inicializador, remova essa linha de código.

Em seguida, habilite Migrações do Code First.

A primeira etapa é certificar-se de que o projeto ContosoUniversity seja definido como o projeto de inicialização. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto ContosoUniversity e selecione **definir como projeto de inicialização**. Migrações do Code First examinará o projeto de inicialização para localizar a cadeia de conexão do banco de dados.

No menu **ferramentas** , clique em **Gerenciador de pacotes NuGet** e em **console do Gerenciador de pacotes**.

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

Na parte superior da janela do **console do Gerenciador de pacotes** , selecione CONTOSOUNIVERSITY. Dal como o projeto padrão e, em seguida, no prompt de `PM>`, insira "Enable-Migrations".

![Habilitar-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

Este comando cria um arquivo *Configuration.cs* em uma nova pasta de *migrações* no projeto ContosoUniversity. Dal.

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

Você selecionou o projeto DAL porque o comando "Enable-Migrations" deve ser executado no projeto que contém a classe de contexto Code First. Quando essa classe está em um projeto de biblioteca de classes, Migrações do Code First procura a cadeia de conexão do banco de dados no projeto de inicialização da solução. Na solução ContosoUniversity, o projeto Web foi definido como o projeto de inicialização. (Se você não quiser designar o projeto que tem a cadeia de conexão como o projeto de inicialização no Visual Studio, poderá especificar o projeto de inicialização no comando do PowerShell. Para ver a sintaxe de comando para o comando Enable-Migrations, você pode inserir o comando "Get-Help Enable-Migrations".)

Abra o arquivo Configuration.cs e substitua os comentários no método `Seed` pelo seguinte código:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

As referências a `List` têm linhas onduladas vermelhas sob elas porque você ainda não tem uma instrução `using` para seu namespace. Clique com o botão direito do mouse em uma das instâncias de `List` e clique em **resolver**e, em seguida, clique em **usando System. Collections. Generic**.

![Resolver com a instrução using](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

Essa seleção de menu adiciona o código a seguir às instruções `using` próximas à parte superior do arquivo.

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> Adicionar código ao método de `Seed` é uma das várias maneiras de inserir dados fixos no banco de dado. Uma alternativa é adicionar código ao `Up` e `Down` métodos de cada classe de migração. Os métodos `Up` e `Down` contêm código que implementa alterações no banco de dados. Você verá exemplos deles no tutorial [implantando uma atualização de banco de dados](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) .
> 
> Você também pode escrever um código que execute instruções SQL usando o método `Sql`. Por exemplo, se você estivesse adicionando uma coluna de orçamento à tabela Department e quisesse inicializar todos os orçamentos de departamento como $1000 como parte de uma migração, você poderia adicionar a seguinte linha de código ao método `Up` para essa migração:
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> Este exemplo mostrado para este tutorial usa o método `AddOrUpdate` no método `Seed` da classe Migrações do Code First `Configuration`. Migrações do Code First chama o método `Seed` após cada migração, e esse método atualiza as linhas que já foram inseridas ou as insere se elas ainda não existirem. O método `AddOrUpdate` pode não ser a melhor opção para seu cenário. Para obter mais informações, consulte [tomar cuidado com o método EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) no blog de Julie Lerman.

Pressione CTRL-SHIFT-B para compilar o projeto.

A próxima etapa é criar uma classe de `DbMigration` para a migração inicial. Você deseja que essa migração crie um novo banco de dados, de modo que você precisa excluir o banco de dados que já existe. SQL Server Compact bancos de dados estão contidos em arquivos *. sdf* na pasta *\_data do aplicativo* . No **Gerenciador de soluções**, expanda *\_dados do aplicativo* no projeto ContosoUniversity para ver os dois bancos de dado SQL Server Compact, que são representados por arquivos *. sdf* .

Clique com o botão direito do mouse no arquivo *School. sdf* e clique em **excluir**.

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

Na janela do **console do Gerenciador de pacotes** , insira o comando "Add-Migration Initial" para criar a migração inicial e nomeie-a como "Initial".

![Adicionar migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Migrações do Code First cria outro arquivo de classe na pasta *migrações* , e essa classe contém um código que cria o esquema de banco de dados.

No **console do Gerenciador de pacotes**, digite o comando "Update-Database" para criar o banco de dados e executar o método **semente** .

![atualizar database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(Se você receber um erro que indica que uma tabela já existe e não pode ser criada, é provável que você tenha executado o aplicativo depois de ter excluído o banco de dados e antes de executar `update-database`. Nesse caso, exclua o arquivo *School. sdf* novamente e repita o comando `update-database`.)

Execute o aplicativo. Agora, a página estudantes está vazia, mas a página instrutores contém instrutores. Isso será o que você obterá em produção depois de implantar o aplicativo.

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

Agora, o projeto está pronto para implantar o banco de dados *escolar* .

## <a name="creating-a-membership-database-for-deployment"></a>Criando um banco de dados de associação para implantação

O aplicativo Contoso University usa o sistema de associação do ASP.NET e a autenticação de formulários para autenticar e autorizar usuários. Uma de suas páginas é acessível somente para administradores. Para ver essa página, execute o aplicativo e selecione **Atualizar créditos** no menu do submenu em **cursos**. O aplicativo exibe a página de **logon** , porque somente os administradores estão autorizados a usar a página de **créditos de atualização** .

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

Faça logon como "admin" usando a senha "Pas $ w0rd" (Observe o número zero no lugar da letra "o" em "w0rd"). Depois de fazer logon, a página **Atualizar créditos** é exibida.

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

Quando você implanta um site pela primeira vez, é comum excluir a maioria ou todas as contas de usuário que você cria para teste. Nesse caso, você implantará uma conta de administrador e não as contas de usuário. Em vez de excluir manualmente as contas de teste, você criará um novo banco de dados de associação que tem apenas a conta de usuário administrador necessária na produção.

> [!NOTE]
> O banco de dados de associação armazena um hash de senhas de conta. Para implantar contas de um computador para outro, você deve certificar-se de que as rotinas de hash não gerem hashes diferentes no servidor de destino do que no computador de origem. Eles gerarão os mesmos hashes quando você usar o Provedores Universais ASP.NET, desde que você não altere o algoritmo padrão. O algoritmo padrão é HMACSHA256 e é especificado no atributo **Validation** do elemento **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** no arquivo Web. config.

O banco de dados de associação não é mantido pelo Migrações do Code First, e não há nenhum inicializador automático que propaga o banco de dados com contas de teste (como há para o banco de dados escolar). Portanto, para manter os dados de teste disponíveis, você fará uma cópia do banco de dado de teste antes de criar um novo.

Em **Gerenciador de soluções**, renomeie o arquivo *ASPNET. sdf* na pasta de *dados\_do aplicativo* como *ASPNET-dev. sdf*. (Não faça uma cópia, basta renomeá-la — você criará um novo banco de dados daqui a pouco.)

Em **Gerenciador de soluções**, verifique se o projeto Web (ContosoUniversity, não CONTOSOUNIVERSITY. Dal) está selecionado. Em seguida, no menu **projeto** , selecione **configuração do ASP.net** para executar a ferramenta de administração de **site**(Wat).

Clique na guia **Segurança**.

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

Clique em **criar ou gerenciar funções** e adicione uma função de **administrador** .

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

Navegue de volta para a guia **segurança** , clique em **criar usuário**e adicione o usuário "admin" como um administrador. Antes de clicar no botão **criar usuário** na página **criar usuário** , certifique-se de marcar a caixa de seleção **administrador** . A senha usada neste tutorial é "Pas $ w0rd", e você pode inserir qualquer endereço de email.

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

Feche o navegador. Em **Gerenciador de soluções**, clique no botão atualizar para ver o novo arquivo *ASPNET. sdf* .

![New_aspnet. sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

Clique com o botão direito do mouse em **ASPNET. sdf** e selecione **incluir no projeto**.

## <a name="distinguishing-development-from-production-databases"></a>Diferenciando o desenvolvimento de bancos de dados de produção

Nesta seção, você renomeará os bancos de dados para que as versões de desenvolvimento sejam School-Dev. sdf e aspnet-Dev. sdf e as versões de produção sejam School-Prod. sdf e aspnet-Prod. sdf. Isso não é necessário, mas isso ajudará a evitar que você obtenha as versões de teste e produção dos bancos de dados confusos.

Em **Gerenciador de soluções**, clique em **Atualizar** e expanda a pasta\_dados do aplicativo para ver o banco de dado escolar que você criou anteriormente; Clique com o botão direito do mouse e selecione **incluir no projeto**.

![Including_School. sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

Renomeie *ASPNET. sdf* para *ASPNET-prod. sdf*.

Renomeie *School. sdf* para *School-dev. sdf*.

Quando você executa o aplicativo no Visual Studio, não quer usar as versões de *produção* dos arquivos de banco de dados, você deseja usar as versões do *-dev* . Portanto, você precisa alterar as cadeias de conexão no arquivo Web. config para que elas apontem para as versões do *-dev* dos bancos de dados. (Você não criou um arquivo School-Prod. sdf, mas isso está OK porque Code First criará esse banco de dados em produção na primeira vez em que você executar seu aplicativo.)

Abra o arquivo Web. config do aplicativo e localize as cadeias de conexão:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

Altere "ASPNET. sdf" para "aspnet-Dev. sdf" e altere "School. sdf" para "School-Dev. sdf":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

Agora, o mecanismo de banco de dados SQL Server Compact e ambos os bancos estão prontos para serem implantados. No tutorial a seguir, você configura as transformações de arquivo *Web. config* automáticas para as configurações que devem ser diferentes nos ambientes de desenvolvimento, teste e produção. (Entre as configurações que devem ser alteradas estão as cadeias de conexão, mas você irá configurar essas alterações posteriormente ao criar um perfil de publicação.)

## <a name="more-information"></a>Mais Informações

Para obter mais informações sobre o NuGet, consulte [gerenciar bibliotecas de projetos com a](https://msdn.microsoft.com/magazine/hh547106.aspx) documentação do NuGet e [NuGet](http://docs.nuget.org/docs/start-here/overview). Se você não quiser usar o NuGet, precisará aprender como analisar um pacote NuGet para determinar o que ele faz quando ele está instalado. (Por exemplo, ele pode configurar transformações de *Web. config* , configurar scripts do PowerShell para serem executados no momento da compilação, etc.) Para saber mais sobre como o NuGet funciona, consulte especialmente [criação e publicação de um pacote](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) e de [transformações de código-fonte](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
