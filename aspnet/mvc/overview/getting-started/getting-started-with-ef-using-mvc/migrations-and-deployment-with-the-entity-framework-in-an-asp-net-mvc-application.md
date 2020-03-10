---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: usar migrações do EF em um aplicativo MVC ASP.NET e implantar no Azure'
author: tdykstra
description: Neste tutorial, você habilita Code First migrações e implanta o aplicativo na nuvem no Azure.
ms.author: riande
ms.date: 01/16/2019
ms.topic: tutorial
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 989dd0f0e18b338be057b9c5657586eff996d8ea
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616077"
---
# <a name="tutorial-use-ef-migrations-in-an-aspnet-mvc-app-and-deploy-to-azure"></a>Tutorial: usar migrações do EF em um aplicativo MVC ASP.NET e implantar no Azure

Até agora, o aplicativo Web de exemplo da Contoso University está em execução localmente em IIS Express no seu computador de desenvolvimento. Para tornar um aplicativo real disponível para outras pessoas usarem pela Internet, você precisa implantá-lo em um provedor de hospedagem na Web. Neste tutorial, você habilita Code First migrações e implanta o aplicativo na nuvem no Azure:

- Habilitar Migrações do Code First. O recurso de migrações permite que você altere o modelo de dados e implante suas alterações na produção atualizando o esquema de banco sem precisar remover e recriar o banco de dados.
- Implante no Azure. Esta etapa é opcional; Você pode continuar com os tutoriais restantes sem ter implantado o projeto.

Recomendamos que você use um processo de integração contínua com controle do código-fonte para implantação, mas este tutorial não aborda esses tópicos. Para obter mais informações, consulte o [controle do código-fonte](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control) e os capítulos de [integração contínua](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) da [criação de aplicativos de nuvem do mundo real com o Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction).

Neste tutorial, você:

> [!div class="checklist"]
> * Habilitar migrações de Code First
> * Implantar o aplicativo no Azure (opcional)

## <a name="prerequisites"></a>Prerequisites

- [Resiliência de conexão e interceptação de comando](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-code-first-migrations"></a>Habilitar migrações de Code First

Quando você desenvolve um novo aplicativo, o modelo de dados é alterado com frequência e, sempre que o modelo é alterado, ele fica fora de sincronia com o banco de dados. Você configurou o Entity Framework para descartar e recriar o banco de dados automaticamente sempre que alterar o modelo. Quando você adicionar, remover ou alterar as classes de entidade ou alterar sua classe de `DbContext`, na próxima vez que executar o aplicativo, ele excluirá automaticamente o banco de dados existente, criará um novo que corresponda ao modelo e propaga-o com os dado de teste.

Esse método de manter o banco de dados em sincronia com o modelo de dados funciona bem até que você implante o aplicativo em produção. Quando o aplicativo está em execução na produção, ele geralmente armazena os dados que você deseja manter e você não deseja perder tudo sempre que fizer uma alteração, como adicionar uma nova coluna. O recurso [migrações do Code First](https://msdn.microsoft.com/data/jj591621) resolve esse problema habilitando Code First para atualizar o esquema de banco de dados em vez de descartar e recriar o banco de dados. Neste tutorial, você implantará o aplicativo e para se preparar para que você habilite as migrações.

1. Desabilite o inicializador que você configurou anteriormente comentando ou excluindo o elemento `contexts` que você adicionou ao arquivo Web. config do aplicativo.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. Também no arquivo *Web. config* do aplicativo, altere o nome do banco de dados na cadeia de conexão para ContosoUniversity2.

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    Essa alteração configura o projeto para que a primeira migração crie um novo banco de dados. Isso não é necessário, mas você verá mais tarde por que é uma boa ideia.
3. No menu **Ferramentas** selecione **Gerenciador de Pacotes NuGet** > **Console do Gerenciador de Pacotes**.

1. No prompt de `PM>`, insira os seguintes comandos:

    ```text
    enable-migrations
    add-migration InitialCreate
    ```

    O comando `enable-migrations` cria uma pasta *migrações* no projeto ContosoUniversity e coloca essa pasta em um arquivo *Configuration.cs* que você pode editar para configurar as migrações.

    (Se você perdeu a etapa acima que o instrui a alterar o nome do banco de dados, as migrações encontrarão o banco de dados existente e executarão automaticamente o comando `add-migration`. Tudo bem, isso só significa que você não executará um teste do código de migrações antes de implantar o banco de dados. Posteriormente, quando você executar o comando `update-database`, nada acontecerá porque o banco de dados já existe.)

    Abra o arquivo *ContosoUniversity\Migrations\Configuration.cs* . Assim como a classe do inicializador que você viu anteriormente, a classe `Configuration` inclui um método `Seed`.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    A finalidade do método de [semente](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) é permitir que você insira ou atualize dados de teste depois que Code First criar ou atualizar o banco de dado. O método é chamado quando o banco de dados é criado e toda vez que o esquema de banco de dado é atualizado após uma alteração no modelo.

### <a name="set-up-the-seed-method"></a>Configurar o método semente

Quando você remove e recria o banco de dados para cada alteração de modelo de dado, usa o método de `Seed` da classe do inicializador para inserir dados de teste, pois depois que cada modelo é alterado, o banco de dados é Descartado e todos os dado de teste são perdidos. Com o Migrações do Code First, os dados de teste são retidos após as alterações do banco de dados, de modo que, inclusive, o método de [semente](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) não é necessário normalmente. Na verdade, você não quer que o método `Seed` insira dados de teste se você estiver usando migrações para implantar o Database para produção, pois o método `Seed` será executado na produção. Nesse caso, você deseja que o método de `Seed` para inserir no banco de dados somente os dados necessários na produção. Por exemplo, talvez você queira que o banco de dados inclua nomes de departamentos reais na tabela de `Department` quando o aplicativo se tornar disponível em produção.

Para este tutorial, você usará migrações para implantação, mas seu método de `Seed` inserirá dados de teste de qualquer forma para facilitar a visualização de como a funcionalidade do aplicativo funciona sem a necessidade de inserir manualmente muitos dados.

1. Substitua o conteúdo do arquivo *Configuration.cs* pelo código a seguir, que carrega os dados de teste para o novo banco de dado.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    O método [semente](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) usa o objeto de contexto de banco de dados como um parâmetro de entrada, e o código no método usa esse objeto para adicionar novas entidades ao banco de dados. Para cada tipo de entidade, o código cria uma coleção de novas entidades, adiciona-as à propriedade [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) apropriada e salva as alterações no banco de dados. Não é necessário chamar o método [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) depois de cada grupo de entidades, como é feito aqui, mas fazer isso ajuda a localizar a origem de um problema se ocorrer uma exceção enquanto o código estiver gravando no banco de dados.

    Algumas das instruções que inserem dados usam o método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) para executar uma operação "Upsert". Como o método `Seed` é executado sempre que você executa o comando `update-database`, normalmente após cada migração, não é possível apenas inserir dados, pois as linhas que você está tentando adicionar já estarão lá após a primeira migração que cria o banco de dado. A operação "Upsert" impede erros que ocorrerão se você tentar inserir uma linha que já existe, mas ela ***substitui*** quaisquer alterações nos dados que você tenha feito durante o teste do aplicativo. Com os dados de teste em algumas tabelas, talvez você não queira que isso aconteça: em alguns casos, ao alterar os dados durante o teste, você deseja que as alterações permaneçam após as atualizações do banco. Nesse caso, você deseja fazer uma operação de inserção condicional: Insira uma linha somente se ela ainda não existir. O método semente usa ambas as abordagens.

    O primeiro parâmetro passado para o método [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) especifica a propriedade a ser usada para verificar se já existe uma linha. Para os dados de aluno de teste que você está fornecendo, a propriedade `LastName` pode ser usada para essa finalidade, pois cada sobrenome na lista é exclusivo:

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Esse código pressupõe que os últimos nomes são exclusivos. Se você adicionar manualmente um aluno com um sobrenome duplicado, obterá a seguinte exceção na próxima vez que executar uma migração:

    **A sequência contém mais de um elemento**

    Para obter informações sobre como lidar com dados redundantes, como dois alunos nomeados como "Alexander Carson", consulte o de [FE (propagação e Entity Framework depuração) do EF](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) no blog de Rick Anderson. Para obter mais informações sobre o método `AddOrUpdate`, consulte [tomar cuidado com o método EF 4,3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) no blog de Julie Lerman.

    O código que cria `Enrollment` entidades pressupõe que você tenha o valor de `ID` nas entidades na coleção de `students`, embora você não tenha definido essa propriedade no código que cria a coleção.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    Você pode usar a propriedade `ID` aqui porque o valor de `ID` é definido quando você chama `SaveChanges` para a coleção de `students`. O EF obtém automaticamente o valor de chave primária quando insere uma entidade no banco de dados e atualiza a propriedade `ID` da entidade na memória.

    O código que adiciona cada entidade `Enrollment` ao conjunto de entidades `Enrollments` não usa o método `AddOrUpdate`. Ele verifica se uma entidade já existe e insere a entidade, caso ela não exista. Essa abordagem preservará as alterações feitas em uma classificação de registro usando a interface do usuário do aplicativo. O código percorre cada membro da [lista](https://msdn.microsoft.com/library/6sh2ey19.aspx) de `Enrollment`e, se o registro não for encontrado no banco de dados, ele adicionará o registro ao banco de dados. Na primeira vez que você atualizar o banco de dados, o banco de dados estará vazio, portanto, ele adicionará cada registro.

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

2. Compile o projeto.

### <a name="execute-the-first-migration"></a>Executar a primeira migração

Quando você executou o comando `add-migration`, as migrações geraram o código que criaria o banco de dados do zero. Esse código também está na pasta *migrações* , no arquivo chamado *&lt;timestamp&gt;\_InitialCreate.cs*. O método de `Up` da classe `InitialCreate` cria as tabelas de banco de dados que correspondem aos conjuntos de entidades de modelo, e o método `Down` os exclui.

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

As migrações chamam o método `Up` para implementar as alterações do modelo de dados para uma migração. Quando você insere um comando para reverter a atualização, as Migrações chamam o método `Down`.

Essa é a migração inicial que foi criada quando você inseriu o comando `add-migration InitialCreate`. O parâmetro (`InitialCreate` no exemplo) é usado para o nome do arquivo e pode ser o que você deseja; Normalmente, você escolhe uma palavra ou frase que resume o que está sendo feito na migração. Por exemplo, você pode nomear uma migração posterior &quot;&quot;de adddepartamentaltable.

Se você criou a migração inicial quando o banco de dados já existia, o código de criação de banco de dados é gerado, mas ele não precisa ser executado porque o banco de dados já corresponde ao modelo de dados. Quando você implantar o aplicativo em outro ambiente no qual o banco de dados ainda não existe, esse código será executado para criar o banco de dados; portanto, é uma boa ideia testá-lo primeiro. É por isso que você alterou o nome do banco de dados na cadeia de conexão anterior&mdash;para que as migrações possam criar uma nova a partir do zero.

1. Na janela **Console do Gerenciador de Pacotes** , digite o seguinte comando:

    `update-database`

    O comando `update-database` executa o método `Up` para criar o banco de dados e, em seguida, executa o método `Seed` para popular o banco de dados. O mesmo processo será executado automaticamente na produção após a implantação do aplicativo, como você verá na seção a seguir.
2. Use **Gerenciador de servidores** para inspecionar o banco de dados como você fez no primeiro tutorial e execute o aplicativo para verificar se tudo ainda funciona da mesma forma que antes.

## <a name="deploy-to-azure"></a>Implantar no Azure

Até agora, o aplicativo está em execução localmente em IIS Express no seu computador de desenvolvimento. Para torná-lo disponível para outras pessoas usarem pela Internet, você precisa implantá-lo em um provedor de hospedagem na Web. Nesta seção do tutorial, você o implantará no Azure. Esta seção é opcional; Você pode ignorar isso e continuar com o tutorial a seguir ou pode adaptar as instruções nesta seção para um provedor de hospedagem diferente de sua escolha.

### <a name="use-code-first-migrations-to-deploy-the-database"></a>Usar Code First migrações para implantar o banco de dados

Para implantar o banco de dados, você usará Migrações do Code First. Ao criar o perfil de publicação que você usa para definir as configurações de implantação do Visual Studio, você marcará uma caixa de seleção rotulada **Atualizar banco de dados**. Essa configuração faz com que o processo de implantação configure automaticamente o arquivo *Web. config* do aplicativo no servidor de destino para que Code First use a classe de inicializador `MigrateDatabaseToLatestVersion`.

O Visual Studio não faz nada com o banco de dados durante o processo de implantação enquanto copia seu projeto para o servidor de destino. Quando você executa o aplicativo implantado e acessa o banco de dados pela primeira vez após a implantação, o Code First verifica se o banco de dados corresponde ao modelo de dado. Se houver uma incompatibilidade, Code First criará automaticamente o banco de dados (se ele ainda não existir) ou atualizará o esquema de banco de dados para a versão mais recente (se existir um banco de dados, mas não corresponder ao modelo). Se o aplicativo implementa uma migração `Seed` método, o método é executado depois que o banco de dados é criado ou o esquema é atualizado.

As migrações `Seed` método insere dados de teste. Se você estivesse implantando em um ambiente de produção, precisaria alterar o método `Seed` para que ele insira apenas os dados que você deseja inserir em seu banco de dados de produção. Por exemplo, em seu modelo de dados atual, talvez você queira ter cursos reais, mas os alunos fictícios no banco de dado de desenvolvimento. Você pode escrever um método de `Seed` para carregar ambos no desenvolvimento e, em seguida, comentar os alunos fictícios antes de implantá-los na produção. Ou você pode escrever um método de `Seed` para carregar apenas cursos e inserir os alunos fictícios no banco de dados de teste manualmente usando a interface do usuário do aplicativo.

### <a name="get-an-azure-account"></a>Obter uma conta do Azure

Você precisará de uma conta do Azure. Se você ainda não tiver uma, mas tiver uma assinatura do Visual Studio, poderá [ativar os benefícios da sua assinatura](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/
). Caso contrário, você pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [Avaliação gratuita do Azure](https://azure.microsoft.com/free/).

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>Criar um site da Web e um banco de dados SQL no Azure

Seu aplicativo Web no Azure será executado em um ambiente de hospedagem compartilhado, o que significa que ele é executado em máquinas virtuais (VMs) que são compartilhadas com outros clientes do Azure. Um ambiente de hospedagem compartilhado é uma maneira de começar na nuvem a um baixo custo. Mais tarde, se seu tráfego da Web aumentar, o aplicativo poderá ser dimensionado para atender à necessidade executando em VMs dedicadas. Para saber mais sobre as opções de preço para Azure App serviço, leia [preços do serviço de aplicativo](https://azure.microsoft.com/pricing/details/app-service/).

Você implantará o banco de dados no banco de dados SQL do Azure. O banco de dados SQL é um serviço de banco de dados relacional baseado em nuvem criado em tecnologias de SQL Server. Ferramentas e aplicativos que funcionam com o SQL Server também funcionam com o banco de dados SQL.

1. Na [portal de gerenciamento do Azure](https://portal.azure.com), escolha **criar um recurso** na guia à esquerda e, em seguida, escolha **ver tudo** no **novo** painel (ou *folha*) para ver todos os recursos disponíveis. Escolha **aplicativo Web + SQL** na seção **da Web** da folha **tudo** . Por fim, escolha **criar**.

    ![Criar um recurso no Portal do Azure](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/create-azure-resource.png)

   O formulário para criar um novo **aplicativo Web + novo recurso SQL** é aberto.

2. Insira uma cadeia de caracteres na caixa **nome do aplicativo** para usar como a URL exclusiva para seu aplicativo. A URL completa consistirá no que você inserir aqui mais o domínio padrão dos serviços de Azure App (. azurewebsites.net). Se o **nome do aplicativo** já estiver em uso, o assistente o notificará com um vermelho *a mensagem o nome do aplicativo não está disponível* . Se o **nome do aplicativo** estiver disponível, você verá uma marca de seleção verde.

3. Na caixa **assinatura** , escolha a assinatura do Azure na qual você deseja que o **serviço de aplicativo** resida.

4. Na caixa de texto **grupo de recursos** , escolha um grupo de recursos ou crie um novo. Essa configuração especifica em qual data center seu site será executado. Para obter mais informações sobre grupos de recursos, consulte [grupos de recursos](/azure/azure-resource-manager/resource-group-overview#resource-groups).

5. Crie um novo **plano do serviço de aplicativo** clicando na *seção serviço de aplicativo*, **criar novo**e preencher o plano do serviço de **aplicativo** (pode ser o mesmo nome que o serviço de aplicativo), o **local**e o **tipo de preço** (há uma opção gratuita).

6. Clique em **banco de dados SQL**e escolha **criar um novo banco de dados** ou selecione um banco de dados existente.

7. Na caixa **nome** , insira um nome para o banco de dados.
8. Clique na caixa **servidor de destino** e selecione **criar um novo servidor**. Como alternativa, se você tiver criado anteriormente um servidor, poderá selecionar esse servidor na lista de servidores disponíveis.
9. Escolha a seção **tipo de preço** , escolha *gratuito*. Se forem necessários recursos adicionais, o banco de dados poderá ser escalado verticalmente a qualquer momento. Para saber mais sobre os preços do SQL Azure, confira [preços do banco de dados SQL do Azure](https://azure.microsoft.com/pricing/details/sql-database/managed/).
10. Modifique o [agrupamento](/sql/relational-databases/collations/collation-and-unicode-support) conforme necessário.
11. Insira um administrador **SQL admin nome de usuário** e uma **senha de administrador do SQL**.

    - Se você selecionou **novo servidor de banco de dados SQL**, defina um novo nome e senha que você usará mais tarde ao acessar o banco de dados.
    - Se você selecionou um servidor que você criou anteriormente, insira as credenciais para esse servidor.

12. A coleção de telemetria pode ser habilitada para o serviço de aplicativo usando Application Insights. Com pouca configuração, Application Insights coleta informações valiosas de evento, exceção, dependência, solicitação e rastreamento. Para saber mais sobre Application Insights, consulte [Azure monitor](https://azure.microsoft.com/services/monitor/).
13. Clique em **criar** na parte inferior para indicar que você terminou.

    O Portal de Gerenciamento retorna à página do painel e a área de **notificações** na parte superior da página mostra que o site está sendo criado. Após um tempo (normalmente menos de um minuto), há uma notificação de que a implantação foi bem-sucedida. Na barra de navegação à esquerda, o novo serviço de aplicativo é exibido na seção **serviços de aplicativos** e o novo banco de dados SQL é exibido na seção **SQL databases** .

### <a name="deploy-the-app-to-azure"></a>Implantar o aplicativo no Azure

1. No Visual Studio, clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e selecione **Publicar** no menu de contexto.

2. Na página **escolher um destino de publicação** , escolha **serviço de aplicativo** e, em seguida, **selecione existente**e, em seguida, escolha **publicar**.

    ![Escolha uma página de destino de publicação](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/publish-select-existing-azure-app-service.png)

3. Se você ainda não adicionou sua assinatura do Azure no Visual Studio, execute as etapas na tela. Essas etapas permitem que o Visual Studio se conecte à sua assinatura do Azure para que a lista de **serviços de aplicativos** inclua seu site.

4. Na página **serviço de aplicativo** , selecione a **assinatura** à qual você adicionou o serviço de aplicativo. Em **Exibir**, selecione **grupo de recursos**. Expanda o grupo de recursos ao qual você adicionou o serviço de aplicativo e, em seguida, selecione o serviço de aplicativo. Escolha **OK** para publicar o aplicativo.

5. A janela **Saída** mostra quais ações de implantação foram executadas e os relatórios da conclusão com êxito da implantação.

6. Após a implantação bem-sucedida, o navegador padrão será aberto automaticamente na URL do site implantado.

    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/cloud-app-browser.png)

    Seu aplicativo agora está em execução na nuvem.

Neste ponto, o banco de dados *SchoolContext* foi criado no banco de dados SQL do Azure porque você selecionou **executar migrações do Code First (é executado no início do aplicativo)** . O arquivo *Web. config* no site implantado foi alterado para que o inicializador [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) seja executado na primeira vez em que seu código lê ou grava dados no banco de dado (o que aconteceu quando você selecionou a guia **alunos** ):

![Trecho do arquivo Web. config](https://asp.net/media/4367421/mig.png)

O processo de implantação também criou uma nova cadeia de conexão *(SchoolContext\_DatabasePublish*) para migrações do Code First usar para atualizar o esquema de banco de dados e propagar o banco de dados.

![Cadeia de conexão no arquivo Web. config](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

Você pode encontrar a versão implantada do arquivo Web. config em seu próprio computador no *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Você pode acessar o próprio arquivo *Web. config* implantado usando FTP. Para obter instruções, consulte [implantação da Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update). Siga as instruções que começam com "para usar uma ferramenta de FTP, você precisa de três coisas: a URL de FTP, o nome de usuário e a senha".

> [!NOTE]
> O aplicativo Web não implementa a segurança, para que qualquer pessoa que encontre a URL possa alterar os dados. Para obter instruções sobre como proteger o site da Web, consulte [implantar um aplicativo MVC do secure ASP.NET com associação, OAuth e banco de dados SQL no Azure](/aspnet/core/security/authorization/secure-data). Você pode impedir que outras pessoas usem o site parando o serviço usando o Portal de Gerenciamento ou o **Gerenciador de servidores** do Azure no Visual Studio.

![Parar o item de menu do serviço de aplicativo](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/server-explorer-stop-app-service.png)

## <a name="advanced-migrations-scenarios"></a>Cenários de migrações avançadas

Se você implantar um banco de dados executando migrações automaticamente, conforme mostrado neste tutorial, e estiver implantando em um site que é executado em vários servidores, poderá obter vários servidores tentando executar migrações ao mesmo tempo. As migrações são atômicas, portanto, se dois servidores tentarem executar a mesma migração, um será bem sucedido e o outro falhará (supondo que as operações não possam ser feitas duas vezes). Nesse cenário, se você quiser evitar esses problemas, poderá chamar as migrações manualmente e configurar seu próprio código para que isso ocorra apenas uma vez. Para obter mais informações, consulte [executando e realizando migrações de scripts do código](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/) no blog de Rowan Miller e [Migrate. exe](/ef/ef6/modeling/code-first/migrations/migrate-exe) (para executar migrações na linha de comando).

Para obter informações sobre outros cenários de migração, consulte [Migrations screencast Series](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

## <a name="update-specific-migration"></a>Atualizar migração específica

`update-database -target MigrationName`

O comando `update-database -target MigrationName` executa a migração de destino.

## <a name="ignore-migration-changes-to-database"></a>Ignorar alterações de migração no banco de dados

`Add-migration MigrationName -ignoreChanges`

`ignoreChanges` cria uma migração vazia com o modelo atual como um instantâneo.

## <a name="code-first-initializers"></a>Inicializadores de Code First

Na seção de implantação, você viu o inicializador [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) que está sendo usado. Code First também fornece outros inicializadores, incluindo [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (o padrão), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) (que você usou anteriormente) e [DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). O inicializador de `DropCreateAlways` pode ser útil para configurar condições para testes de unidade. Você também pode escrever seus próprios inicializadores e pode chamar um inicializador explicitamente se não quiser esperar até que o aplicativo leia ou grave no banco de dados.

Para obter mais informações sobre inicializadores, consulte [noções básicas sobre inicializadores de banco de dados no Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm) e o capítulo 6 da programação de livros [Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) por Julie Lerman e Rowan Miller.

## <a name="get-the-code"></a>Obter o código

[Baixar o projeto concluído](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionais

Links para outros recursos de Entity Framework podem ser encontrados em [recursos de acesso a dados ASP.net-recomendado](xref:whitepapers/aspnet-data-access-content-map).

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Migrações Code First habilitadas
> * Implantou o aplicativo no Azure (opcional)

Avance para o próximo artigo para saber como criar um modelo de dados mais complexo para um aplicativo MVC ASP.NET.
> [!div class="nextstepaction"]
> [Criar um modelo de dados mais complexo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
