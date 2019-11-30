---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 'Implantação da Web do ASP.NET usando o Visual Studio: preparando para implantação de banco de dados | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros, por usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: cdcb3578725c41e3c801afd54e6d34455bc4b281
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74618524"
---
# <a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>Implantação da Web do ASP.NET usando o Visual Studio: preparando para implantação de banco de dados

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros usando o Visual Studio 2012 ou o Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial da série](introduction.md).

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

Este tutorial mostra como preparar o projeto para implantação de banco de dados. A estrutura do banco de dados e alguns (nem todos) dos os dois bancos de dado do aplicativo devem ser implantados em ambientes de teste, de preparo e de produção.

Normalmente, à medida que você desenvolve um aplicativo, você insere dados de teste em um banco de dado que você não deseja implantar em um site ativo. No entanto, você também pode ter alguns dados de produção que deseja implantar. Neste tutorial, você configurará o projeto da Contoso University e preparará os scripts SQL para que os dados corretos sejam incluídos quando você implantar o.

Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](troubleshooting.md).

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

O aplicativo de exemplo usa SQL Server Express LocalDB. SQL Server Express é a edição gratuita do SQL Server. Normalmente, ele é usado durante o desenvolvimento porque ele se baseia no mesmo mecanismo de banco de dados que as versões completas do SQL Server. Você pode testar com SQL Server Express e ter certeza de que o aplicativo se comportará da mesma forma em produção, com algumas exceções para recursos que variam entre as edições SQL Server.

O LocalDB é um modo de execução especial de SQL Server Express que permite que você trabalhe com bancos de dados como arquivos *. MDF* . Normalmente, os arquivos de banco de dados LocalDB são mantidos na pasta *Data\_do aplicativo* de um projeto Web. O recurso de instância de usuário no SQL Server Express também permite que você trabalhe com arquivos *. MDF* , mas o recurso de instância de usuário foi preterido; Portanto, o LocalDB é recomendado para trabalhar com arquivos *. MDF* .

Normalmente SQL Server Express não é usado para aplicativos Web de produção. O LocalDB, em particular, não é recomendado para uso em produção com um aplicativo Web porque ele não foi projetado para funcionar com o IIS.

No Visual Studio 2012, o LocalDB é instalado por padrão com o Visual Studio. No Visual Studio 2010 e versões anteriores, SQL Server Express (sem o LocalDB) é instalado por padrão com o Visual Studio; é por isso que você o instalou como um dos pré-requisitos no [primeiro tutorial desta série](introduction.md).

Para obter mais informações sobre as edições do SQL Server, incluindo o LocalDB, consulte os recursos a seguir [trabalhando com bancos de dados do SQL Server](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver).

## <a name="entity-framework-and-universal-providers"></a>Entity Framework e Provedores Universais

Para acesso ao banco de dados, o aplicativo da Contoso University requer o seguinte software que deve ser implantado com o aplicativo porque não está incluído no .NET Framework:

- [Provedores universais ASP.net](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) (permite que o sistema de associação do ASP.net use o banco de dados SQL do Azure)
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

Como esse software está incluído nos pacotes NuGet, o projeto já está configurado para que os assemblies necessários sejam implantados com o projeto. (Os links apontam para as versões atuais desses pacotes, que podem ser mais recentes do que o que está instalado no projeto inicial que você baixou para este tutorial.)

Se você estiver implantando em um provedor de Hospedagem de terceiros em vez do Azure, certifique-se de usar o Entity Framework 5,0 ou posterior. As versões anteriores do Migrações do Code First exigem confiança total e a maioria dos provedores de hospedagem executará seu aplicativo em confiança média. Para obter mais informações sobre confiança média, consulte o tutorial [implantar no IIS como um ambiente de teste](deploying-to-iis.md) .

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>Configurar Migrações do Code First para implantação de banco de dados de aplicativo

O banco de dados do aplicativo da Contoso University é gerenciado pelo Code First e você o implantará usando Migrações do Code First. Para obter uma visão geral da implantação de banco de dados usando Migrações do Code First, consulte [o primeiro tutorial desta série](introduction.md).

Quando você implanta um banco de dados de aplicativo, normalmente você não simplesmente implanta seu banco de dados de desenvolvimento com todos eles na produção, porque grande parte dos dados nele é provavelmente apenas para fins de teste. Por exemplo, os nomes de aluno em um banco de dados de teste são fictícios. Por outro lado, geralmente não é possível implantar apenas a estrutura do banco de dados sem nenhum dado. Alguns dos dados no banco de dado de teste podem ser dados reais e devem estar lá quando os usuários começam a usar o aplicativo. Por exemplo, seu banco de dados pode ter uma tabela que contém valores de nível válido ou nomes de departamento reais.

Para simular esse cenário comum, você configurará um Migrações do Code First método `Seed` que é inserido no banco de dados apenas os que você deseja que estejam na produção. Este método de `Seed` não deve inserir dados de teste porque ele será executado em produção depois que Code First criar o banco de dado em produção.

Em versões anteriores do Code First antes da liberação de migrações, era comum `Seed` métodos para inserir dados de teste também, porque com cada alteração de modelo durante o desenvolvimento, o banco de dado tinha de ser completamente excluído e recriado do zero. Com o Migrações do Code First, os dados de teste são retidos após as alterações do banco de dados, de modo que não é necessário, inclusive, o teste do método `Seed`. O projeto que você baixou usa o método de incluir todos os dados no método `Seed` de uma classe de inicializador. Neste tutorial, você desabilitará essa classe de inicializador e habilitará as migrações. Em seguida, você atualizará o método `Seed` na classe de configuração migrações para que ele insira apenas os dados que você deseja inserir na produção.

O diagrama a seguir ilustra o esquema do banco de dados do aplicativo:

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

Para esses tutoriais, você presumirá que as tabelas `Student` e `Enrollment` devem estar vazias quando o site for implantado pela primeira vez. As outras tabelas contêm dados que têm que ser pré-carregados quando o aplicativo é colocado em tempo real.

### <a name="disable-the-initializer"></a>Desabilitar o inicializador

Como você usará Migrações do Code First, não é necessário usar o inicializador de Code First de `DropCreateDatabaseIfModelChanges`. O código para esse inicializador está no arquivo *SchoolInitializer.cs* no projeto CONTOSOUNIVERSITY. Dal. Uma configuração no elemento `appSettings` do arquivo *Web. config* faz com que esse inicializador seja executado sempre que o aplicativo tentar acessar o banco de dados pela primeira vez:

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

Abra o arquivo *Web. config* do aplicativo e remova ou comente o elemento `add` que especifica a classe do inicializador de Code First. O elemento `appSettings` agora tem esta aparência:

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> Outra maneira de especificar uma classe de inicializador é fazer isso chamando `Database.SetInitializer` no método `Application_Start` no arquivo *global. asax* . Se você estiver habilitando migrações em um projeto que usa esse método para especificar o inicializador, remova essa linha de código.

> [!NOTE]
> Se você estiver usando Visual Studio 2013, adicione as seguintes etapas entre as etapas 2 e 3: (a) em PMC digite "Update-Package EntityFramework-Version 6.1.1" para obter a versão atual do EF. Em seguida, (b) compile o projeto para obter uma lista de erros de compilação e corrija-os. Exclua instruções using para namespaces que não existem mais, clique com o botão direito do mouse e clique em resolver para adicionar instruções using em que são necessárias e altere as ocorrências de System. Data. EntityState para System. Data. Entity. EntityState.

### <a name="enable-code-first-migrations"></a>Habilitar Migrações do Code First

1. Verifique se o projeto ContosoUniversity (não ContosoUniversity. DAL) está definido como o projeto de inicialização. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto ContosoUniversity e selecione **definir como projeto de inicialização**. Migrações do Code First examinará o projeto de inicialização para localizar a cadeia de conexão do banco de dados.
2. No menu **ferramentas** , escolha **Gerenciador de pacotes NuGet** > **console do Gerenciador de pacotes**.

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. Na parte superior da janela do **console do Gerenciador de pacotes** , selecione CONTOSOUNIVERSITY. Dal como o projeto padrão e, em seguida, no prompt de `PM>`, insira "Enable-Migrations".

    ![comando Enable-Migrations](preparing-databases/_static/image4.png)

    (Se você receber um erro informando que o comando *Enable-Migrations* não é reconhecido, insira o comando *Update-Package EntityFramework-REINSTALL* e tente novamente.)

    Esse comando cria uma pasta *Migrations* no projeto CONTOSOUNIVERSITY. Dal e coloca na pasta dois arquivos: um arquivo *Configuration.cs* que você pode usar para configurar as migrações e um arquivo *InitialCreate.cs* para a primeira migração que cria o banco de dados.

    ![Pasta de migrações](preparing-databases/_static/image5.png)

    Você selecionou o projeto DAL na lista suspensa **projeto padrão** do **console do Gerenciador de pacotes** porque o comando `enable-migrations` deve ser executado no projeto que contém a classe de contexto Code First. Quando essa classe está em um projeto de biblioteca de classes, Migrações do Code First procura a cadeia de conexão do banco de dados no projeto de inicialização da solução. Na solução ContosoUniversity, o projeto Web foi definido como o projeto de inicialização. Se você não quiser designar o projeto que tem a cadeia de conexão como o projeto de inicialização no Visual Studio, poderá especificar o projeto de inicialização no comando do PowerShell. Para ver a sintaxe do comando, digite o comando `get-help enable-migrations`.

    O comando `enable-migrations` criou automaticamente a primeira migração porque o banco de dados já existe. Uma alternativa é fazer com que as migrações criem o banco de dados. Para fazer isso, use **Gerenciador de servidores** ou **pesquisador de objetos do SQL Server** para excluir o banco de dados ContosoUniversity antes de habilitar as migrações. Depois de habilitar as migrações, crie a primeira migração manualmente digitando o comando "Add-Migration InitialCreate". Em seguida, você pode criar o banco de dados digitando o comando "Update-Database".

### <a name="set-up-the-seed-method"></a>Configurar o método semente

Para este tutorial, você adicionará dados fixos adicionando código ao método `Seed` da classe Migrações do Code First `Configuration`. Migrações do Code First chama o método `Seed` após cada migração.

Como o método `Seed` é executado após cada migração, há dados já existentes nas tabelas após a primeira migração. Para lidar com essa situação, você usará o método `AddOrUpdate` para atualizar as linhas que já foram inseridas ou inseri-las se elas ainda não existirem. O método `AddOrUpdate` pode não ser a melhor opção para seu cenário. Para obter mais informações, consulte [tomar cuidado com o método EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) no blog de Julie Lerman.

1. Abra o arquivo *Configuration.cs* e substitua os comentários no método `Seed` pelo seguinte código:

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. As referências a `List` têm linhas onduladas vermelhas sob elas porque você ainda não tem uma instrução `using` para seu namespace. Clique com o botão direito do mouse em uma das instâncias de `List` e clique em **resolver**e, em seguida, clique em **usando System. Collections. Generic**.

    ![Resolver com a instrução using](preparing-databases/_static/image6.png)

    Essa seleção de menu adiciona o código a seguir às instruções `using` próximas à parte superior do arquivo.

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. Pressione CTRL-SHIFT-B para compilar o projeto.

Agora, o projeto está pronto para implantar o banco de dados *ContosoUniversity* . Depois de implantar o aplicativo, na primeira vez que você executá-lo e navegar até uma página que acessa o banco de dados, Code First criará o banco de dados e executará esse método de `Seed`.

> [!NOTE]
> Adicionar código ao método de `Seed` é uma das várias maneiras de inserir dados fixos no banco de dado. Uma alternativa é adicionar código ao `Up` e `Down` métodos de cada classe de migração. Os métodos `Up` e `Down` contêm código que implementa alterações no banco de dados. Você verá exemplos deles no tutorial [implantando uma atualização de banco de dados](deploying-a-database-update.md) .
> 
> Você também pode escrever um código que execute instruções SQL usando o método `Sql`. Por exemplo, se você estivesse adicionando uma coluna de orçamento à tabela Department e quisesse inicializar todos os orçamentos de departamento como $1000 como parte de uma migração, você poderia adicionar a seguinte linha de código ao método `Up` para essa migração:
> 
> `Sql("UPDATE Department SET Budget = 1000");`

## <a name="create-scripts-for-membership-database-deployment"></a>Criar scripts para a implantação de banco de dados de associação

O aplicativo Contoso University usa o sistema de associação do ASP.NET e a autenticação de formulários para autenticar e autorizar usuários. A página **créditos de atualização** só é acessível para usuários que estão na função Administrador.

Execute o aplicativo e clique em **cursos**e, em seguida, clique em **Atualizar créditos**.

![Clique em atualizar créditos](preparing-databases/_static/image7.png)

A página de **logon** é exibida porque a página de **créditos de atualização** requer privilégios administrativos.

Insira *administrador* como o nome de usuário e *devpwd* como a senha e clique em **fazer logon**.

![Página de logon](preparing-databases/_static/image8.png)

A página **créditos de atualização** é exibida.

![Página atualizar créditos](preparing-databases/_static/image9.png)

As informações de usuário e função estão no banco de dados *ASPNET-ContosoUniversity* especificado pela cadeia de conexão **DefaultConnection** no arquivo *Web. config* .

Esse banco de dados não é gerenciado pelo Entity Framework Code First, portanto, você não pode usar migrações para implantá-lo. Você usará o provedor dbDacFx para implantar o esquema de banco de dados e configurará o perfil de publicação para executar um script que irá inserir dados iniciais em tabelas de banco de dados.

> [!NOTE]
> Um novo sistema de associação ASP.NET (agora chamado ASP.NET Identity) foi introduzido com Visual Studio 2013. O novo sistema permite manter as tabelas de aplicativo e associação no mesmo banco de dados, e você pode usar Migrações do Code First para implantar ambos. O aplicativo de exemplo usa o sistema de associação ASP.NET anterior, que não pode ser implantado usando Migrações do Code First. Os procedimentos para implantar esse banco de dados de associação também se aplicam a qualquer outro cenário no qual seu aplicativo precise implantar um banco de dados SQL Server que não é criado pelo Entity Framework Code First.

Aqui também, você normalmente não quer os mesmos dados na produção que você tem em desenvolvimento. Quando você implanta um site pela primeira vez, é comum excluir a maioria ou todas as contas de usuário que você cria para teste. Portanto, o projeto baixado tem dois bancos de dados de associação: *ASPNET-ContosoUniversity. MDF* com usuários de desenvolvimento e *ASPNET-ContosoUniversity-prod. MDF* com usuários de produção. Para este tutorial, os nomes de usuário são os mesmos em ambos os bancos de dados: *admin* e não *admin*. Ambos os usuários têm a senha *devpwd* no banco de dados de desenvolvimento e *prodpwd* no banco de dados de produção.

Você implantará os usuários de desenvolvimento no ambiente de teste e os usuários de produção para preparo e produção. Para fazer isso, você criará dois scripts SQL neste tutorial, um para desenvolvimento e outro para produção e, em Tutoriais posteriores, configurará o processo de publicação para executá-los.

> [!NOTE]
> O banco de dados de associação armazena um hash de senhas de conta. Para implantar contas de um computador para outro, você deve certificar-se de que as rotinas de hash não gerem hashes diferentes no servidor de destino do que no computador de origem. Eles gerarão os mesmos hashes quando você usar o Provedores Universais ASP.NET, desde que você não altere o algoritmo padrão. O algoritmo padrão é HMACSHA256 e é especificado no atributo **Validation** do elemento **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** no arquivo Web. config.

Você pode criar scripts de implantação de dados manualmente, usando o SQL Server Management Studio (SSMS) ou usando uma ferramenta de terceiros. Este restante deste tutorial mostrará como fazer isso no SSMS, mas se você não quiser instalar e usar o SSMS, poderá obter os scripts da versão completa do projeto e pular para a seção onde armazená-los na pasta da solução.

Para instalar o SSMS, instale-o no [centro de download: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) clicando em [ENU\x64\SQLManagementStudio\_x64\_PTB. exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe) ou [ENU\x86\SQLManagementStudio\_x86\_PTB. exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe). Se você escolher um incorreto para o sistema, ele não será instalado e você poderá tentar o outro.

(Observe que esse é um download de 600 megabytes. Pode levar muito tempo para instalar e exigirá uma reinicialização do seu computador.)

Na primeira página da central de instalação do SQL Server, clique em **novo SQL Server instalação autônoma ou adicionar recursos a uma instalação existente**e siga as instruções, aceitando as opções padrão.

### <a name="create-the-development-database-script"></a>Criar o script de desenvolvimento de banco de dados

1. Execute o SSMS.
2. Na caixa de diálogo **conectar ao servidor** , digite *(LocalDB) \v11.0* como o **nome do servidor**, deixe a **autenticação** definida como **autenticação do Windows**e clique em **conectar**.

    ![Conectar o SSMS ao servidor](preparing-databases/_static/image10.png)
3. Na janela pesquisador de **objetos** , expanda **bancos de dados**, clique com o botão direito do mouse em **ASPNET-ContosoUniversity**, clique em **tarefas**e em **gerar scripts**.

    ![Gerar scripts do SSMS](preparing-databases/_static/image11.png)
4. Na caixa de diálogo **gerar e publicar scripts** , clique em **definir opções de script**.

    Você pode ignorar a etapa **escolher objetos** porque o padrão é o **banco de dados inteiro do script e todos os objetos de banco de dados** e isso é o que você deseja.
5. Clique em **Avançadas**.

    ![Opções de script do SSMS](preparing-databases/_static/image12.png)
6. Na caixa de diálogo **Opções de script avançadas** , role para baixo até **tipos de dados para script**e clique na opção **somente dados** na lista suspensa.
7. Alterar **script use o banco de dados** para **false**. As instruções USE não são válidas para o banco de dados SQL do Azure e não são necessárias para a implantação SQL Server Express no ambiente de teste.

    ![Somente dados de script do SSMS, nenhuma instrução USE](preparing-databases/_static/image13.png)
8. Clique em **OK**.
9. Na caixa de diálogo **gerar e publicar scripts** , a caixa **nome do arquivo** especifica onde o script será criado. Altere o caminho para a pasta da solução (a pasta que tem o arquivo ContosoUniversity. sln) e o nome do arquivo para *ASPNET-data-dev. SQL*.
10. Clique em **Avançar** para ir para a guia **Resumo** e, em seguida, clique em **Avançar** novamente para criar o script.

    ![Script do SSMS criado](preparing-databases/_static/image14.png)
11. Clique em **Finalizar**.

### <a name="create-the-production-database-script"></a>Criar o script do banco de dados de produção

Como você não executou o projeto com o banco de dados de produção, ele ainda não está anexado à instância de LocalDB. Portanto, você precisa primeiro anexar o banco de dados.

1. No Pesquisador de **objetos**do SSMS, clique com o botão direito do mouse em **bancos de dados** e clique em **anexar**.

    ![Anexo do SSMS](preparing-databases/_static/image15.png)
2. Na caixa de diálogo **anexar bancos de dados** , clique **em Adicionar** e, em seguida, navegue até o arquivo *ASPNET-ContosoUniversity-prod. MDF* na pasta de *dados do aplicativo\_* .

     ![Adicionar arquivo. MDF do SSMS para anexar](preparing-databases/_static/image16.png)
3. Clique em **OK**.
4. Siga o mesmo procedimento usado anteriormente para criar um script para o arquivo de produção. Nomeie o arquivo de script *ASPNET-data-prod. SQL*.

## <a name="summary"></a>Resumo

Os dois bancos de dados agora estão prontos para serem implantados e você tem dois scripts de implantação em sua pasta de solução.

![Scripts de implantação de dados](preparing-databases/_static/image17.png)

No tutorial a seguir, você define as configurações de projeto que afetam a implantação e configura as transformações de arquivo *Web. config* automáticas para as configurações que devem ser diferentes no aplicativo implantado.

## <a name="more-information"></a>Mais Informações

Para obter mais informações sobre o NuGet, consulte [gerenciar bibliotecas de projetos com a](https://msdn.microsoft.com/magazine/hh547106.aspx) documentação do NuGet e [NuGet](http://docs.nuget.org/docs/start-here/overview). Se você não quiser usar o NuGet, precisará aprender como analisar um pacote NuGet para determinar o que ele faz quando ele está instalado. (Por exemplo, ele pode configurar transformações de *Web. config* , configurar scripts do PowerShell para serem executados no momento da compilação, etc.) Para saber mais sobre como o NuGet funciona, consulte [criando e publicando um pacote](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package) e as [transformações de código-fonte e o arquivo de configuração](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations).

> [!div class="step-by-step"]
> [Anterior](introduction.md)
> [Próximo](web-config-transformations.md)
