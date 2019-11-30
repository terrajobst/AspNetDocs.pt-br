---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Implantação da Web do ASP.NET usando o Visual Studio: Implantando uma atualização de banco de dados | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros, por usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 805eb84c24764cf921291f89054435601dbac48e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74636827"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Implantação da Web do ASP.NET usando o Visual Studio: Implantando uma atualização de banco de dados

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros usando o Visual Studio 2012 ou o Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial da série](introduction.md).

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

Neste tutorial, você faz uma alteração de banco de dados e alterações de código relacionadas, testa as alterações no Visual Studio e, em seguida, implanta a atualização nos ambientes de teste, de preparo e de produção.

O tutorial mostra primeiro como atualizar um banco de dados gerenciado pelo Migrações do Code First e, depois, mostra como atualizar um banco de dados usando o provedor dbDacFx.

Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Implantar uma atualização de banco de dados usando Migrações do Code First

Nesta seção, você adiciona uma coluna data de nascimento à classe base `Person` para as entidades `Student` e `Instructor`. Em seguida, você atualiza a página que exibe dados do instrutor para que ele exiba a nova coluna. Por fim, você implanta as alterações para teste, preparo e produção.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Adicionar uma coluna a uma tabela no banco de dados do aplicativo

1. No projeto *ContosoUniversity. Dal* , abra *Person.cs* e adicione a seguinte propriedade no final da classe `Person` (deve haver duas chaves de fechamento após a ti):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Em seguida, atualize o método `Seed` para que ele forneça um valor para a nova coluna. Abra *Migrations\Configuration.cs* e substitua o bloco de código que começa `var instructors = new List<Instructor>` com o seguinte bloco de código que inclui informações de data de nascimento:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Compile a solução e, em seguida, abra a janela do **console do Gerenciador de pacotes** . Verifique se ContosoUniversity. DAL ainda está selecionado como o **projeto padrão**.
3. Na janela do **console do Gerenciador de pacotes** , selecione **ContosoUniversity. Dal** como o **projeto padrão**e, em seguida, digite o seguinte comando:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Quando esse comando for concluído, o Visual Studio abrirá o arquivo de classe que define a nova classe `DbMigration` e, no método `Up`, você poderá ver o código que cria a nova coluna. O método `Up` cria a coluna quando você está implementando a alteração, e o método `Down` exclui a coluna ao reverter a alteração.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Compile a solução e, em seguida, digite o seguinte comando na janela do **console do Gerenciador de pacotes** (verifique se o projeto CONTOSOUNIVERSITY. Dal ainda está selecionado):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    O Entity Framework executa o método `Up` e, em seguida, executa o método `Seed`.

### <a name="display-the-new-column-in-the-instructors-page"></a>Exibir a nova coluna na página de instrutores

1. No projeto ContosoUniversity, abra *instrutores. aspx* e adicione um novo campo de modelo para exibir a data de nascimento. Adicione-o entre os itens para a data de contratação e a atribuição do Office:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Se o recuo de código ficar fora de sincronia, você poderá pressionar CTRL-K e, em seguida, CTRL-D para reformatar o arquivo automaticamente.)
2. Execute o aplicativo e clique no link **instrutores** .

    Quando a página for carregada, você verá que ela tem o novo campo data de nascimento.

    ![Página de instrutores com DataDeNascimento](deploying-a-database-update/_static/image2.png)
3. Feche o navegador.

### <a name="deploy-the-database-update"></a>Implantar a atualização do banco de dados

1. Em **Gerenciador de soluções** selecione o projeto ContosoUniversity.
2. Na barra de ferramentas de **publicação do clique do Web One** , clique no perfil de publicação de **teste** e, em seguida, clique em **publicar Web**. (Se a barra de ferramentas estiver desabilitada, selecione o projeto ContosoUniversity em **Gerenciador de soluções**.)

    O Visual Studio implanta o aplicativo atualizado e o navegador é aberto na home page.
3. Execute a página **instrutores** para verificar se a atualização foi implantada com êxito.

    Quando o aplicativo tenta acessar o banco de dados para essa página, Code First atualiza o esquema de banco de dados e executa o método `Seed`. Quando a página for exibida, você verá a coluna **data de nascimento** esperada com datas.
4. Na barra de ferramentas de **publicação de um clique da Web** , clique no perfil de publicação de **preparo** e, em seguida, clique em **publicar Web**.
5. Execute a página **instrutores** em preparo para verificar se a atualização foi implantada com êxito.
6. Na barra de ferramentas de **publicação do clique do Web One** , clique no perfil de publicação de **produção** e clique em **publicar Web**.
7. Execute a página **instrutores** em produção para verificar se a atualização foi implantada com êxito.

    Para uma atualização de aplicativo de produção real que inclui uma alteração de banco de dados, normalmente, você também colocaria o aplicativo offline durante a implantação usando o *aplicativo\_offline. htm*, como vimos no tutorial anterior.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Implantar uma atualização de banco de dados usando o provedor dbDacFx

Nesta seção, você adiciona uma coluna de *comentários* à tabela de *usuário* no banco de dados de associação e cria uma página que permite exibir e editar comentários para cada usuário. Em seguida, você implanta as alterações para teste, preparo e produção.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Adicionar uma coluna a uma tabela no banco de dados de associação

1. No Visual Studio, abra **pesquisador de objetos do SQL Server**.
2. Expanda **(LocalDB) \v11.0**, expanda **bancos de dados**, expanda **ASPNET-ContosoUniversity** (não **ASPNET-ContosoUniversity-prod**) e expanda **tabelas**.

    Se você não vir **(LocalDB) \v11.0** no nó **SQL Server** , clique com o botão direito do mouse no nó **SQL Server** e clique em **Adicionar SQL Server**. Na caixa de diálogo **conectar ao servidor** , insira *(LocalDB) \v11.0* como o **nome do servidor**e, em seguida, clique em **conectar**.

    Se você não vir **ASPNET-ContosoUniversity**, execute o projeto e faça logon usando as credenciais de *administrador* (a senha é *devpwd*) e, em seguida, atualize a janela de **pesquisador de objetos do SQL Server** .
3. Clique com o botão direito do mouse na tabela **usuários** e clique em **Designer de exibição**.

    ![Designer de exibição do SSOX](deploying-a-database-update/_static/image3.png)
4. No designer, adicione uma coluna *comentários* e torne-a *nvarchar (128)* e Nullable e, em seguida, clique em **Atualizar**.

    ![Adicionando coluna de comentários](deploying-a-database-update/_static/image4.png)
5. Na caixa de **atualizações do banco de dados de visualização** , clique em **Atualizar banco de dados**.

    ![Visualizar atualizações de banco de dados](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Criar uma página para exibir e editar a nova coluna

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta **conta** no projeto ContosoUniversity, clique em **Adicionar**e em **novo item**.
2. Crie um novo **formulário da Web usando a página mestra** e nomeie-o como *UserInfo. aspx*. Aceite o arquivo *site. Master* padrão como a página mestra.
3. Copie a marcação a seguir no elemento `MainContent` `Content` (o último dos três elementos `Content`):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Clique com o botão direito do mouse na página *UserInfo. aspx* e clique em **Exibir no navegador**.
5. Faça logon com suas credenciais de usuário *administrador* (senha é *devpwd*) e adicione alguns comentários a um usuário para verificar se a página funciona corretamente.

    ![Página UserInfo](deploying-a-database-update/_static/image6.png)
6. Feche o navegador.

## <a name="deploy-the-database-update"></a>Implantar a atualização do banco de dados

Para implantar usando o provedor dbDacFx, basta selecionar a opção **Atualizar banco de dados** no perfil de publicação. No entanto, para a implantação inicial quando você usou essa opção, também configurou alguns scripts SQL adicionais a serem executados: eles ainda estão no perfil e você terá que impedi-los de executar novamente.

1. Abra o assistente **publicar na Web** clicando com o botão direito do mouse no projeto ContosoUniversity e clicando em **publicar**.
2. Selecione o perfil de **teste** .
3. Clique na guia **configurações** .
4. Em **DefaultConnection**, selecione **Atualizar banco de dados**.
5. Desabilite os scripts adicionais que você configurou para execução na implantação inicial:

    1. Clique em **Configurar atualizações de banco de dados**.
    2. Na caixa de diálogo **Configurar atualizações de banco de dados** , desmarque as caixas de seleção ao lado de *Grant. SQL* e *ASPNET-data-dev. SQL*.
    3. Clique em **Fechar**.
6. Clique na guia **Visualizar** .
7. Em **bancos** de dados e à direita de **DefaultConnection**, clique no link **Preview Database** .

    ![Visualização do banco de dados](deploying-a-database-update/_static/image7.png)

    A janela de visualização mostra o script que será executado no banco de dados de destino para fazer com que o esquema de banco de dados corresponda ao esquema do banco de dados de origem. O script inclui um comando ALTER TABLE que adiciona a nova coluna.
8. Feche a caixa de diálogo **visualização do banco de dados** e clique em **publicar**.

    O Visual Studio implanta o aplicativo atualizado e o navegador é aberto na home page.
9. Execute a página UserInfo (adicione *Account/UserInfo. aspx* à URL Home Page) para verificar se a atualização foi implantada com êxito. Você precisará fazer logon digitando *admin* e *devpwd*.

    Os dados nas tabelas não são implantados por padrão e você não configurou um script de implantação de dados para executar, portanto, não encontrará o comentário que você adicionou no desenvolvimento. Você pode adicionar um novo comentário agora em preparo para verificar se a alteração foi implantada no banco de dados e a página funciona corretamente.
10. Siga o mesmo procedimento para implantar o preparo e a produção.

    Não se esqueça de desabilitar os scripts extras. A única diferença em comparação com o perfil de teste é que você desabilitará apenas um script nos perfis de preparo e produção, pois eles foram configurados para executar somente *ASPNET-prod-Data. SQL*.

    As credenciais para preparo e produção são admin e prodpwd.

    Para uma atualização de aplicativo de produção real que inclui uma alteração de banco de dados, normalmente, você também colocaria o aplicativo offline durante a implantação carregando o *aplicativo\_offline. htm* antes de publicá-lo e excluí-lo posteriormente, como vimos no [tutorial anterior](deploying-a-code-update.md).

## <a name="summary"></a>Resumo

Agora você implantou uma atualização de aplicativo que incluiu uma alteração de banco de dados usando Migrações do Code First e o provedor dbDacFx.

![Página de instrutores com DataDeNascimento](deploying-a-database-update/_static/image8.png)

![Página UserInfo](deploying-a-database-update/_static/image9.png)

O próximo tutorial mostra como executar implantações usando a linha de comando.

> [!div class="step-by-step"]
> [Anterior](deploying-a-code-update.md)
> [Próximo](command-line-deployment.md)
