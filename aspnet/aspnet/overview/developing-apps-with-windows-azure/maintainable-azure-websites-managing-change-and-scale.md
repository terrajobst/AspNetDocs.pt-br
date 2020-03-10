---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Laboratório prático: sites do Azure que podem ser mantidos: Gerenciando alterações e escala | Microsoft Docs'
author: rick-anderson
description: Neste laboratório, saiba como Microsoft Azure facilita a criação e a implantação de sites para produção.
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: c88bae40a8aa092037c0b359ee391acaf161cf10
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624267"
---
# <a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Laboratório prático: sites do Azure que podem ser mantidos: Gerenciando alterações e escala

por [equipe de acampamentos da Web](https://twitter.com/webcamps)

[Baixe o kit de treinamento do Web acampamentos](https://aka.ms/webcamps-training-kit)

> Microsoft Azure facilita a criação e a implantação de sites para produção. Mas você não está pronto quando seu aplicativo está ativo, está apenas começando! Você precisará lidar com requisitos de alteração, atualizações de banco de dados, escala e muito mais. Felizmente, Azure App serviço lhe foi abordado, com muitos recursos para ajudá-lo a manter seus sites funcionando sem problemas.
>
> O Azure oferece opções seguras e flexíveis de desenvolvimento, implantação e dimensionamento para qualquer aplicativo Web de tamanho. Utilize suas ferramentas existentes para criar e implantar aplicativos sem a dificuldade de gerenciar a infraestrutura.
>
> Provisione um aplicativo Web de produção em minutos, implantando facilmente o conteúdo criado usando sua ferramenta de desenvolvimento favorita. Você pode implantar um site existente diretamente do controle do código-fonte com suporte para **git**, **GitHub**, **bitbucket**, **TFS**e até mesmo **Dropbox**. Implante diretamente do seu IDE favorito ou de scripts usando o **PowerShell** nas ferramentas do Windows ou da **CLI** em execução em qualquer sistema operacional. Depois de implantado, mantenha seus sites constantemente atualizados com suporte para implantação contínua.
>
> O Azure fornece soluções escalonáveis, de armazenamento durável na nuvem, backup e recuperação para quaisquer dados, grandes ou pequenos. Ao implantar aplicativos em um ambiente de produção, os serviços de armazenamento, como tabelas, BLOBs e bancos de dados SQL, ajudam a dimensionar seu aplicativo na nuvem.
>
> Com os bancos de dados SQL, é importante manter seu banco de dados produtivo atualizado ao implantar novas versões do seu aplicativo. Graças ao **migrações do Entity Framework Code First**, o desenvolvimento e a implantação do modelo de dados foram simplificados para atualizar seus ambientes em minutos. Este laboratório prático mostrará os diferentes tópicos que você pode encontrar ao implantar seu aplicativo Web em ambientes de produção no Microsoft Azure.
>
> Todos os códigos de exemplo e trechos de código estão incluídos no kit de treinamento do acampamentos da Web, disponível em [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).
>
> Para obter uma cobertura mais detalhada deste tópico, consulte o [criando aplicativos de nuvem do mundo real com o livro eletrônico do Azure](building-real-world-cloud-apps-with-windows-azure/introduction.md).

<a id="Overview"></a>
## <a name="overview"></a>Visão geral

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá a:

- Habilitar migrações de Entity Framework com um modelo existente
- Atualizar o modelo de objeto e o banco de dados de acordo usando migrações Entity Framework
- Implantar no serviço Azure App usando o Git
- Reverter para uma implantação anterior usando o portal de gerenciamento do Azure
- Usar o armazenamento do Azure para dimensionar um aplicativo Web
- Configurar o dimensionamento automático para um aplicativo Web usando o Portal de Gerenciamento do Azure
- Criar e configurar um projeto de teste de carga no Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Prerequisites

O seguinte é necessário para concluir este laboratório prático:

- [Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/) ou superior
- [SDK do Azure para .NET 2,2](https://www.microsoft.com/windowsazure/sdk/)
- [Sistema de controle de versão do GIT](http://git-scm.com/download)
- Uma assinatura Microsoft Azure

    - Inscreva-se para uma [avaliação gratuita](https://aka.ms/watk-freetrial)
    - Se você for um Visual Studio Professional, Test Professional, Premium ou Ultimate com o MSDN ou Plataformas MSDN assinante, ative seu [benefício do MSDN](https://aka.ms/watk-msdn) agora para começar a desenvolver e testar no Azure
    - Os membros do [BizSpark](https://aka.ms/watk-bizspark) recebem automaticamente o benefício do Azure por meio de suas assinaturas Visual Studio Ultimate com msdn
    - Os membros do programa [Microsoft Partner Network](https://aka.ms/watk-mpn) Cloud Essentials recebem créditos mensais do Azure sem encargos

<a id="Setup"></a>
### <a name="setup"></a>Instalação

Para executar os exercícios neste laboratório prático, você precisará configurar seu ambiente primeiro...

1. Abra o Windows Explorer e navegue até a pasta de **origem** do laboratório.
2. Clique com o botão direito do mouse em **Setup. cmd** e selecione **Executar como administrador** para iniciar o processo de instalação que irá configurar seu ambiente e instalar os trechos de código do Visual Studio para este laboratório.
3. Se a caixa de diálogo controle de conta de usuário for mostrada, confirme a ação para continuar.

> [!NOTE]
> Verifique se você verificou todas as dependências deste laboratório antes de executar a instalação.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Usando os trechos de código

Em todo o documento do laboratório, você será instruído a inserir blocos de código. Para sua conveniência, a maior parte desse código é fornecida como trechos de Visual Studio Code, que podem ser acessados em Visual Studio 2013 para evitar a necessidade de adicioná-lo manualmente.

> [!NOTE]
> Cada exercício é acompanhado por uma solução inicial localizada na pasta **begin** do exercício que permite que você siga cada exercício independentemente dos outros. Lembre-se de que os trechos de código que são adicionados durante um exercício estão ausentes dessas soluções iniciais e podem não funcionar até que você conclua o exercício. Dentro do código-fonte de um exercício, você também encontrará uma pasta **final** contendo uma solução do Visual Studio com o código que resulta da conclusão das etapas no exercício correspondente. Você pode usar essas soluções como diretrizes se precisar de ajuda adicional ao trabalhar com este laboratório prático.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Usando migrações de Entity Framework](#Exercise1)
2. [Implantando um aplicativo Web para preparo](#Exercise2)
3. [Executando a reversão de implantação na produção](#Exercise3)
4. [Dimensionamento usando o armazenamento do Azure](#Exercise4)
5. [Usando o dimensionamento automático para aplicativos Web](#Exercise5) (opcional para Visual Studio 2013 Ultimate Edition)

Tempo estimado para concluir este laboratório: **75 minutos**

> [!NOTE]
> Ao iniciar o Visual Studio pela primeira vez, você deve selecionar uma das coleções de configurações predefinidas. Cada coleção predefinida é projetada para corresponder a um estilo de desenvolvimento específico e determina layouts de janela, comportamento do editor, trechos de código IntelliSense e opções da caixa de diálogo. Os procedimentos neste laboratório descrevem as ações necessárias para realizar uma determinada tarefa no Visual Studio ao usar a coleção de **configurações de desenvolvimento geral** . Se você escolher uma coleção de configurações diferentes para seu ambiente de desenvolvimento, poderá haver diferenças nas etapas que você deve levar em conta.

<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Exercício 1: usando migrações de Entity Framework

Quando você estiver desenvolvendo um aplicativo, seu modelo de dados poderá mudar ao longo do tempo. Essas alterações podem afetar o modelo existente no banco de dados (se você estiver criando uma nova versão) e for importante manter seu banco de dados atualizado para evitar erros.

Para simplificar o acompanhamento dessas alterações em seu modelo, **migrações do Entity Framework Code First** detectar automaticamente as alterações que comparam seu modelo com o esquema de banco de dados e gera um código específico para atualizar seu banco de dados, criando novas *versões* do banco de dados.

Este exercício mostra como habilitar as **migrações** para seu aplicativo e como você pode facilmente detectar e gerar alterações para atualizar seus bancos de dados.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Tarefa 1 – habilitando as migrações

Nesta tarefa, você seguirá as etapas de habilitar **migrações do Entity Framework Code First** para o banco de dados de **teste especialista** , alterar o modelo e entender como essas alterações serão refletidas no banco de dados.

1. Abra o Visual Studio e abra o arquivo de solução **GeekQuiz. sln** de **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.
2. Compile a solução para baixar e instalar as dependências do pacote **NuGet** . Para fazer isso, clique com o botão direito do mouse na solução e clique em **Compilar solução** ou pressione **Ctrl + Shift + B**.
3. No menu **ferramentas** no Visual Studio, selecione **Gerenciador de pacotes NuGet**e clique em **console do Gerenciador de pacotes**.
4. No **console do Gerenciador de pacotes**, digite o comando a seguir e pressione **Enter**. Uma migração inicial com base no modelo existente será criada.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Habilitando migrações](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Habilitando as migrações")

    *Habilitando migrações*

    > [!NOTE]
    > Esse comando adiciona uma pasta **Migrations** ao projeto de teste de especialista que contém um arquivo chamado **Configuration.cs**. A classe de **configuração** permite que você configure como as migrações se comparam ao seu contexto.
5. Com as migrações habilitadas, você precisará atualizar a classe de **configuração** para popular o banco de dados com a data inicial exigida pelo **teste de especialista** . Em **migrações**, substitua o arquivo **Configuration.cs** com aquele localizado na pasta **Source\Assets** deste laboratório.

    > [!NOTE]
    > Como as **migrações** chamarão o método **semente** com cada atualização de banco de dados, você precisará ter certeza de que os registros não serão duplicados no banco de dados. O método **AddOrUpdate** ajudará a evitar dados duplicados.
6. Para adicionar uma migração inicial, insira o comando a seguir e pressione **Enter**.

    > [!NOTE]
    > Verifique se não há nenhum banco de dados chamado &quot;GeekQuizProd&quot; na instância de LocalDB.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Adicionando a migração de esquema base](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Adicionando a migração de esquema base")

    *Adicionando a migração de esquema base*

    > [!NOTE]
    > A **migração adicionará** Scaffold a próxima migração com base nas alterações feitas em seu modelo desde a criação da última migração. Nesse caso, como é a primeira migração do projeto, ele adicionará os scripts para criar todas as tabelas definidas na classe **TriviaContext** .
7. Execute a migração para atualizar o banco de dados executando o comando a seguir. Para este comando, você especificará o sinalizador **detalhado** para exibir as instruções SQL que estão sendo aplicadas ao banco de dados de destino.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Criando banco de dados inicial](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Criando banco de dados inicial")

    *Criando banco de dados inicial*

    > [!NOTE]
    > **Update-Database** aplicará todas as migrações pendentes ao banco de dados. Nesse caso, ele criará o banco de dados usando a cadeia de conexão definida em seu arquivo Web. config.
8. Vá para o menu **Exibir** e abra **pesquisador de objetos do SQL Server**.

    ![Abrir no Pesquisador de Objetos do SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Abrir no Pesquisador de Objetos do SQL Server")

    *Abrir no Pesquisador de Objetos do SQL Server*
9. Na janela **pesquisador de objetos do SQL Server** , conecte-se à instância do LocalDB clicando com o botão direito do mouse no nó **SQL Server** e selecionando a opção **Adicionar SQL Server...** .

    ![Adicionando uma instância de SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Adicionando uma instância de SQL Server")

    *Adicionando uma instância de SQL Server ao Pesquisador de Objetos do SQL Server*
10. Defina o **nome do servidor** como *(LocalDB) \v11.0* e deixe a **autenticação do Windows** como seu modo de autenticação. Clique em **Conectar** para continuar.

    ![Conectando ao LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Conectando ao LocalDB")

    *Conectando ao LocalDB*
11. Abra o banco de dados **GeekQuizProd** e expanda o nó **tabelas** . Como você pode ver, o comando **Update-Database** gerou todas as tabelas definidas na classe **TriviaContext** . Localize o **dbo. Tabela TriviaQuestions** e abra o nó colunas. Na próxima tarefa, você adicionará uma nova coluna a essa tabela e atualizará o banco de dados usando as **migrações**.

    ![Colunas de perguntas de Trívia](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Colunas de perguntas de Trívia")

    *Colunas de perguntas de Trívia*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Tarefa 2 – Atualizando o esquema de banco de dados usando migrações

Nesta tarefa, você usará **migrações do Entity Framework Code First** para detectar uma alteração em seu modelo e gerar o código necessário para atualizar o banco de dados. Você atualizará a entidade **TriviaQuestions** adicionando uma nova propriedade a ela. Em seguida, você executará comandos para criar uma nova migração para incluir a nova coluna na tabela.

1. Em **Gerenciador de soluções**, clique duas vezes no arquivo **TriviaQuestion.cs** localizado dentro da pasta **modelos** .
2. Adicione uma nova propriedade chamada **Hint**, conforme mostrado no trecho de código a seguir.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. No **console do Gerenciador de pacotes**, digite o comando a seguir e pressione **Enter**. Uma nova migração será criada refletindo a alteração em nosso modelo.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Adicionar e migrar](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Adicionar e migrar")

    *Adicionar e migrar*

    > [!NOTE]
    > Um arquivo de migração é composto por dois métodos, **para cima** e **para baixo**.
    >
    > - O método **up** será usado para especificar as alterações que a versão atual do aplicativo precisa aplicar ao banco de dados.
    > - O **inoperante** é usado para reverter as alterações que adicionamos ao método **up** .
    >
    > Quando a migração de banco de dados atualizar o banco de dados, ele executará todas as migrações na ordem de carimbo de data/hora e somente aquelas que não foram usadas desde a última atualização (a tabela \_MigrationHistory controla quais migrações foram aplicadas). O método **up** de todas as migrações será chamado e fará as alterações que especificamos para o banco de dados. Se decidirmos voltar a uma migração anterior, o método **inoperante** será chamado para refazer as alterações em uma ordem inversa.
4. No **console do Gerenciador de pacotes**, digite o comando a seguir e pressione **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. A saída do comando **Update-Database** gerou uma instrução **ALTER TABLE** SQL para adicionar uma nova coluna à tabela **TriviaQuestions** , conforme mostrado na imagem abaixo.

    ![Instrução SQL de adição de coluna gerada](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Instrução SQL de adição de coluna gerada")

    *Instrução SQL de adição de coluna gerada*
6. Em **pesquisador de objetos do SQL Server**, atualize o **dbo. TriviaQuestions** tabela e verifique se a nova coluna de **dica** é exibida.

    ![Mostrando a nova coluna de dica](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Mostrando a nova coluna de dica")

    *Mostrando a nova coluna de dica*
7. De volta ao editor de **TriviaQuestion.cs** , adicione uma restrição **StringLength** à propriedade *Hint* , conforme mostrado no trecho de código a seguir.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. No **console do Gerenciador de pacotes**, digite o comando a seguir e pressione **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. No **console do Gerenciador de pacotes**, digite o comando a seguir e pressione **Enter**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. A saída do comando **Update-Database** gerou uma instrução **ALTER TABLE** SQL para atualizar o tipo de coluna de *dica* da tabela **TriviaQuestions** , conforme mostrado na imagem abaixo.

    ![Instrução ALTER COLUMN SQL gerada](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Instrução ALTER COLUMN SQL gerada")

    *Instrução ALTER COLUMN SQL gerada*
11. Em **pesquisador de objetos do SQL Server**, atualize o **dbo. TriviaQuestions** tabela e verifique se o tipo de coluna de **dica** é **nvarchar (150)** .

    ![Mostrando a nova restrição](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Mostrando a nova restrição")

    *Mostrando a nova restrição*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Exercício 2: Implantando um aplicativo Web para preparo

Os **aplicativos Web no serviço de Azure app** permitem que você execute a publicação em etapas. A publicação em etapas cria um slot de site de preparo para cada local de produção padrão e permite que você troque esses slots sem tempo de inatividade. Isso é realmente útil para validar as alterações antes de liberá-las para o público, integrar incrementalmente o conteúdo do site e reverter se as alterações não estiverem funcionando conforme o esperado.

Neste exercício, você implantará o aplicativo de **teste de especialista** no ambiente de preparo do seu aplicativo Web usando o controle do código-fonte git. Para fazer isso, você criará o aplicativo Web e provisioná os componentes necessários no portal de gerenciamento, configurará um repositório **git** e enviará por push o código-fonte do aplicativo do computador local para o slot de preparo. Você também atualizará seu banco de dados de produção com o **migrações do Code First** criado no exercício anterior. Em seguida, você executará o aplicativo nesse ambiente de teste para verificar sua operação. Quando estiver satisfeito de estar trabalhando de acordo com suas expectativas, você promoverá o aplicativo para produção.

> [!NOTE]
> Para habilitar a publicação em etapas, o aplicativo Web deve estar no **modo padrão**. Observe que os encargos adicionais serão incorridos se você alterar seu aplicativo Web para o modo padrão. Para obter mais informações sobre preços, consulte [preços do serviço de aplicativo](https://azure.microsoft.com/pricing/details/app-service/).

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Tarefa 1 – Criando um aplicativo Web no serviço Azure App

Nesta tarefa, você criará um aplicativo Web no **serviço Azure app** no portal de gerenciamento. Você também configurará um banco de dados **SQL** para persistir o aplicativo e configurar um repositório git local para o controle do código-fonte.

1. Acesse o [portal de gerenciamento do Azure](https://manage.windowsazure.com) e entre usando o conta Microsoft associado à sua assinatura.

    ![Entre no portal de gerenciamento do Azure](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Entre no portal de gerenciamento do Azure*
2. Clique em **novo** na barra de comandos na parte inferior da página.

    ![Criando um novo aplicativo Web](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Criando um novo aplicativo Web")

    *Criando um novo aplicativo Web*
3. Clique em **computação**, **site** e **criação personalizada**.

    ![Criando um novo aplicativo Web usando a criação personalizada](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Criando um novo aplicativo Web usando a criação personalizada")

    *Criando um novo aplicativo Web usando a criação personalizada*
4. Na caixa de diálogo **novo site-criação personalizada** , forneça uma **URL** disponível (por exemplo *, especialista em teste*), selecione um local na lista suspensa **região** e selecione **criar um novo banco de dados SQL** na lista suspensa **banco de dados** . Por fim, marque a caixa **de seleção publicar do controle do código-fonte** e clique em **Avançar**.

    ![Personalizando o novo aplicativo Web](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Personalizando o novo aplicativo Web*
5. Especifique as seguintes informações para as configurações de banco de dados:

   - Na caixa de texto **nome** , insira um nome de banco de dados (por exemplo, *geekquiz\_DB*)
   - Na lista **suspensa** servidor, selecione **novo servidor de banco de dados SQL**. Como alternativa, você pode selecionar um servidor existente.
   - Nas caixas **nome de usuário** e **senha** do banco de dados, insira o nome de usuário e a senha do administrador para o servidor do banco de dados SQL. Se você selecionar um servidor que já criou, a senha será solicitada.

     ![Especificando as configurações do banco de dados](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     *Especificando as configurações do banco de dados*
6. Clique em **Avançar** para continuar.
7. Selecione **repositório git local** para o controle do código-fonte a ser usado e clique em **Avançar**.

    > [!NOTE]
    > Você pode ser solicitado a fornecer as credenciais de implantação (um nome de usuário e uma senha).

    ![Criando o repositório git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Criando o repositório git*
8. Aguarde até que o novo aplicativo Web seja criado.

    > [!NOTE]
    > Por padrão, o Azure fornece domínios em *azurewebsites.net* , mas também oferece a possibilidade de definir domínios personalizados usando o portal de gerenciamento do Azure. No entanto, você só poderá gerenciar domínios personalizados se estiver usando determinados modos de serviço de Azure App.
    >
    > O serviço de Azure App está disponível nas edições gratuita, compartilhada, básica, Standard e Premium. No modo gratuito e compartilhado, todos os aplicativos Web são executados em um ambiente multilocatário e têm cotas de uso de CPU, memória e rede. O número máximo de aplicativos gratuitos pode variar com seu plano. No modo padrão, você escolhe quais aplicativos são executados em máquinas virtuais dedicadas que correspondem aos recursos de computação padrão do Azure. Você pode encontrar a configuração do modo de aplicativo Web no menu **dimensionar** do seu aplicativo Web.
    >
    > ![Modos de serviço Azure App](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Modos de serviço Azure App")
    >
    > Se você estiver usando o modo **compartilhado** ou **padrão** , poderá gerenciar domínios personalizados para seu aplicativo Web acessando o menu **Configurar** do seu aplicativo e clicando em **gerenciar domínios** em *nomes de domínio*.
    >
    > ![Gerenciar domínios](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Gerenciar domínios")
    >
    > ![Gerenciar domínios personalizados](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Gerenciar domínios personalizados")
9. Depois que o aplicativo Web for criado, clique no link sob a coluna **URL** para verificar se o novo aplicativo Web está em execução.

    ![Navegando para o novo aplicativo Web](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Navegando para o novo aplicativo Web*

    ![aplicativo Web em execução](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *aplicativo Web em execução*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Tarefa 2 – Criando o banco de dados SQL de produção

Nesta tarefa, você usará o **migrações do Entity Framework Code First** para criar o banco de dados destinado à instância do **banco de dados SQL do Azure** criada na tarefa anterior.

1. No Portal de Gerenciamento, navegue até o aplicativo Web que você criou na tarefa anterior e vá para seu **painel**.
2. Na página **painel** , clique no link **exibir cadeias de conexão** na seção **visão rápida** .

    ![Exibir cadeias de conexão](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "Exibir cadeias de conexão")

    *Exibir cadeias de conexão*
3. Copie o valor da **cadeia de conexão** e feche a caixa de diálogo.

    ![Cadeia de conexão no Azure Portal de Gerenciamento](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Cadeia de conexão no Azure Portal de Gerenciamento")

    *Cadeia de conexão no Azure Portal de Gerenciamento*
4. Clique em **bancos de dados SQL** para ver a lista de bancos de dados SQL no Azure

    ![Menu do banco de dados SQL](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "Menu do banco de dados SQL")

    *Menu do banco de dados SQL*
5. Localize o banco de dados que você criou na tarefa anterior e clique no servidor.

    ![Servidor do banco de dados SQL](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "Servidor de banco de dados SQL")

    *Servidor do banco de dados SQL*
6. Na página **início rápido** do servidor, clique em **Configurar**.

    ![Configurar menu](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Configurar menu")

    *Configurar menu*
7. Na seção **endereços IP permitidos** , clique em **Adicionar ao link endereços IP permitidos** para permitir que seu IP se conecte ao servidor do banco de dados SQL.

    ![Endereços IP permitidos](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Endereços IP permitidos")

    *Endereços IP permitidos*
8. Clique em **Salvar** na parte inferior da página para concluir a etapa.
9. Volte para o Visual Studio.
10. No **console do Gerenciador de pacotes**, execute o seguinte comando substituindo o espaço reservado *[Your-Connection-String]* pela cadeia de conexão que você copiou do Azure

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Atualizar banco de dados destinado ao banco de dados SQL do Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Atualizar banco de dados destinado ao banco de dados SQL do Windows Azure")

    *Atualizar banco de dados de destino do banco de dados SQL*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Tarefa 3 – implantando o teste de especialista em preparo usando o Git

Nesta tarefa, você habilitará a publicação em etapas em seu aplicativo Web. Em seguida, você usará o Git para publicar o aplicativo Geek Quiz diretamente do seu computador local ao ambiente de preparo do aplicativo web.

1. Volte para o portal e clique no nome do aplicativo Web na coluna **nome** para exibir as páginas de gerenciamento.

    ![Abrindo as páginas de gerenciamento do aplicativo Web](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Abrindo as páginas de gerenciamento do aplicativo Web*
2. Navegue até a página **escala** . Na seção **geral** , selecione **padrão** para a configuração e clique em **salvar** na barra de comandos.

    > [!NOTE]
    > Para executar todos os aplicativos Web na região atual e na assinatura no modo **padrão** , deixe a caixa de seleção **selecionar tudo** marcada na configuração **escolher sites** . Caso contrário, desmarque a caixa de seleção **selecionar tudo** .

    ![Atualizando o aplicativo Web para o modo padrão](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Atualizando o aplicativo Web para o modo padrão")

    *Atualizando o aplicativo Web para o modo padrão*
3. Clique em **Sim** para confirmar as alterações.

    ![Confirmando a alteração no modo padrão](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Continuando com a alteração do modo de aplicativo Web")

    *Confirmando a alteração no modo padrão*
4. Vá para a página **painel** e clique em **habilitar publicação em etapas** na seção **visão rápida** .

    ![Habilitando a publicação em etapas](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Habilitando a publicação em etapas")

    *Habilitando a publicação em etapas*
5. Clique em **Sim** para habilitar a publicação em etapas.

    ![Confirmando a publicação em etapas](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Clicando em Sim para habilitar a publicação em etapas")

    *Confirmando a publicação em etapas*
6. Na lista de aplicativos Web, expanda a marca à esquerda do nome do aplicativo Web para exibir o slot do site de preparo. Ele tem o nome do seu aplicativo Web seguido por ***(preparo)***. Clique no site de preparo para ir para a página de gerenciamento.

    ![Navegando para o aplicativo Web de preparo](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navegando para o aplicativo Web de preparo")

    *Navegando para o aplicativo de preparo*
7. Observe que a página de gerenciamento do é semelhante a qualquer outra página de gerenciamento do aplicativo Web. Navegue até a página **implantações** e copie o valor da **URL do git** . Você o usará posteriormente neste exercício.

    ![Copiando o valor da URL do git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Copiando o valor da URL do git*
8. Abra um novo console do **git bash** e execute os comandos a seguir. Atualize o espaço reservado *[Your-Application-Path]* com o caminho para a solução **GeekQuiz** , localizada na pasta **Source\Ex1-DeployingWebSiteToStaging\Begin** deste laboratório.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Inicialização do git e primeira confirmação](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Inicialização do git e primeira confirmação*
9. Execute o comando a seguir para enviar por push seu aplicativo Web para o repositório **git** remoto. Substitua o espaço reservado pela URL obtida do portal de gerenciamento. Você será solicitado a fornecer sua senha de implantação.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Enviando por push para o Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Enviando por push para o Azure*

    > [!NOTE]
    > Ao implantar conteúdo no host de FTP ou repositório GIT de um aplicativo Web, você deve autenticar usando as **credenciais de implantação** que você criou nas páginas de gerenciamento de **início rápido** ou de **painel** do aplicativo Web. Se você não souber suas credenciais de implantação, poderá redefini-las facilmente usando o portal de gerenciamento. Abra a página **painel** do aplicativo Web e clique no link **redefinir suas credenciais de implantação** . Forneça uma nova senha e clique em **OK**. As credenciais de implantação são válidas para uso com todos os aplicativos Web associados à sua assinatura.
10. Para verificar se o aplicativo Web foi enviado por push com êxito para o Azure, volte para o portal de gerenciamento e clique em **sites**.
11. Localize seu aplicativo Web e expanda a entrada para exibir o slot do site de preparo. Clique em seu **nome** para ir para a página de gerenciamento.
12. Clique em **implantações** para ver o **histórico de implantação**. Verifique se há uma **implantação ativa** com sua *&quot;de confirmação inicial&quot;* .

    ![Implantação ativa](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Implantação ativa*
13. Por fim, clique em **procurar** na barra de comandos para ir para o aplicativo Web.

    ![Procurar aplicativo Web](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Procurar aplicativo Web*
14. Se o aplicativo for implantado com êxito, você verá a página de logon do teste de especialista.

    > [!NOTE]
    > A URL de endereço do aplicativo implantado contém o nome do seu aplicativo Web seguido por *-preparo*.

    ![Aplicativo em execução no ambiente de preparo](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Aplicativo em execução no ambiente de preparo*
15. Se você quiser explorar o aplicativo, clique em **registrar** para registrar um novo usuário. Preencha os detalhes da conta inserindo um nome de usuário, endereço de email e senha. Em seguida, o aplicativo mostra a primeira pergunta do teste. Responda a algumas perguntas para verificar se ele está funcionando conforme o esperado.

    ![Aplicativo pronto para ser usado](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Aplicativo pronto para ser usado*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Tarefa 4 – promovendo o aplicativo Web para produção

Agora que você verificou que o aplicativo Web está funcionando corretamente no ambiente de preparo, você está pronto para promovê-lo para produção. Nesta tarefa, você alternará o slot do site de preparo com o slot do local de produção.

1. Volte para o portal de gerenciamento e selecione o slot do site de preparo. Clique em **alternar** na barra de comandos.

    ![Alternar para produção](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Alternar para produção*
2. Clique em **Sim** na caixa de diálogo de confirmação para prosseguir com a operação de permuta. O Azure alternará imediatamente o conteúdo do site de produção com o conteúdo do site de preparo.

    > [!NOTE]
    > Algumas configurações da versão em etapas serão automaticamente copiadas para a versão de produção (por exemplo, substituições de cadeia de conexão, mapeamentos de manipulador, etc.), mas outras configurações não serão alteradas (por exemplo, pontos de extremidade DNS, associações SSL, etc.).

    ![Confirmando a operação de permuta](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Confirmando a operação de permuta*
3. Quando a permuta for concluída, selecione o slot de produção e clique em **procurar** na barra de comandos para abrir o site de produção. Observe a URL na barra de endereços.

    > [!NOTE]
    > Talvez seja necessário atualizar o navegador para limpar o cache. No Internet Explorer, você pode fazer isso pressionando **Ctrl + R**.

    ![Aplicativo Web em execução no ambiente de produção](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. No console do **GitBash** , atualize a URL remota para o repositório git local para o slot de produção de destino. Para fazer isso, execute o comando a seguir substituindo os espaços reservados pelo seu nome de usuário de implantação e o nome do seu aplicativo Web.

    > [!NOTE]
    > Nos exercícios a seguir, você enviará por push as alterações para o local de produção em vez de preparo apenas para a simplicidade do laboratório. Em um cenário do mundo real, é recomendável verificar as alterações no ambiente de preparo antes de promover para produção.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Exercício 3: executando a reversão de implantação na produção

Há cenários em que você não tem um slot de preparo para executar troca automática entre preparo e produção, por exemplo, se você estiver trabalhando com o modo **livre** ou **compartilhado** . Nesses cenários, você deve testar seu aplicativo em um ambiente de teste, seja localmente ou em um site remoto, antes de implantar na produção. No entanto, é possível que um problema não detectado durante a fase de teste possa surgir no local de produção. Nesse caso, é importante ter um mecanismo para mudar facilmente para uma versão anterior e mais estável do aplicativo o mais rápido possível.

No **serviço Azure app**, a implantação contínua do controle do código-fonte possibilita graças à ação de **reimplantação** disponível no portal de gerenciamento. O Azure controla as implantações associadas às confirmações enviadas por push ao repositório e fornece uma opção para reimplantar seu aplicativo usando qualquer uma das implantações anteriores, a qualquer momento.

Neste exercício, você executará uma alteração no código do aplicativo de **teste de especialista** que injeta intencionalmente um *bug*. Você implantará o aplicativo em produção para ver o erro e, em seguida, aproveitará o recurso reimplantar para voltar ao estado anterior.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Tarefa 1 – Atualizando o aplicativo de teste de especialista

Nesta tarefa, você recriará uma pequena parte do código da classe **TriviaController** para extrair parte da lógica que recupera a opção de teste selecionada do banco de dados para um novo método.

1. Alterne para a instância do Visual Studio com a solução **GeekQuiz** do exercício anterior.
2. No **Gerenciador de soluções**, abra o arquivo **TriviaController.cs** dentro da pasta **controladores** .
3. Localize o método **StoreAsync** e selecione o código realçado na figura a seguir.

    ![Selecionando o código](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Selecionando o código*
4. Clique com o botão direito do mouse no código selecionado, expanda o menu **Refactor** e selecione **extrair método...** .

    ![Extraindo o código como um novo método](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Selecionando método de extração*
5. Na caixa de diálogo **extrair método** , nomeie o novo método *MatchesOption* e clique em **OK**.

    ![Especificando o nome do método](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Especificando o nome para o método extraído*
6. O código selecionado é então extraído para o método **MatchesOption** . O código resultante é mostrado no trecho a seguir.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Pressione **Ctrl + S** para salvar as alterações.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Tarefa 2 – Reimplantando o aplicativo de teste de especialista

Agora você enviará por push as alterações feitas na tarefa anterior para o repositório, o que disparará uma nova implantação para o ambiente de produção. Em seguida, você solucionará um problema usando as **ferramentas de desenvolvimento F12** fornecidas pelo Internet Explorer e, em seguida, executará uma reversão para a implantação anterior no portal de gerenciamento do Azure.

1. Abra um novo console do **git bash** para implantar o aplicativo atualizado no serviço Azure app.
2. Execute os comandos a seguir para enviar as alterações por push para o Azure. Atualize o espaço reservado *[Your-Application-Path]* com o caminho para a solução **GeekQuiz** . Você será solicitado a fornecer sua senha de implantação.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Enviando por push o código refatorado para o Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Enviando por push o código refatorado para o Azure*
3. Abra o Internet Explorer e navegue até seu aplicativo Web (por exemplo, `http://<your-web-site>.azurewebsites.net`). Faça logon usando as credenciais criadas anteriormente.
4. Pressione **F12** para iniciar as ferramentas de desenvolvimento, selecione a guia **rede** e clique no botão **reproduzir** para iniciar a gravação.

    ![Iniciando a gravação da rede](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Iniciando a gravação da rede")

    *Iniciando a gravação da rede*
5. Selecione qualquer opção do teste. Você verá que nada acontece.
6. Na janela **F12** , a entrada correspondente à solicitação HTTP post mostra um resultado http **500** .

    ![Erro HTTP 500](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *Erro HTTP 500*
7. Selecione a guia **console** . Um erro é registrado com os detalhes da causa.

    ![Erro registrado](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Erro registrado*
8. Localize a parte detalhes do erro. Claramente, esse erro é causado pela refatoração do código que você confirmou nas etapas anteriores.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.
9. Não feche o navegador.
10. Em uma nova instância do navegador, navegue até o [portal de gerenciamento do Azure](https://manage.windowsazure.com) e entre usando o conta Microsoft associado à sua assinatura.
11. Selecione **sites** e clique no aplicativo Web criado no exercício 2.
12. Navegue até a página **implantações** . Observe que todas as confirmações executadas estão listadas no histórico de implantação.

    ![Lista de implantações existentes](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Lista de implantações existentes*
13. Selecione a confirmação anterior e clique em **reimplantar** na barra de comandos.

    ![Reimplantando a confirmação anterior](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Reimplantando a confirmação anterior*
14. Quando solicitado a confirmar, clique em **Sim**.

    ![Confirmando a reimplantação](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Quando a implantação for concluída, volte para a instância do navegador com seu aplicativo Web e pressione **Ctrl + F5**.
16. Clique em qualquer uma das opções. A animação invertida agora ocorrerá e o resultado (*correto/incorreto*) será exibido.
17. Adicional Alterne para o console do **git bash** e execute os comandos a seguir para reverter para a confirmação anterior.

    > [!NOTE]
    > Esses comandos criam uma nova confirmação que desfaz todas as alterações no repositório git que foram feitas na confirmação inadequada. O Azure reimplantará o aplicativo usando a nova confirmação.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Exercício 4: dimensionamento usando o armazenamento do Azure

Os **BLOBs** são a maneira mais simples de armazenar grandes quantidades de texto não estruturado ou dados binários, como vídeo, áudio e imagens. Mover o conteúdo estático do seu aplicativo para o armazenamento ajuda a dimensionar seu aplicativo ao servir imagens ou documentos diretamente para o navegador.

Neste exercício, você moverá o conteúdo estático do seu aplicativo para um contêiner de BLOB. Em seguida, você configurará seu aplicativo para adicionar uma **regra de reescrita de URL ASP.net** no **Web. config** para redirecionar o conteúdo para o contêiner de BLOB.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Tarefa 1 – criando uma conta de armazenamento do Azure

Nesta tarefa, você aprenderá a criar uma nova conta de armazenamento usando o portal de gerenciamento.

1. Navegue até o [portal de gerenciamento do Azure](https://manage.windowsazure.com) e entre usando o conta Microsoft associado à sua assinatura.
2. Selecione **novo | Serviços de dados | Armazenamento | Criação rápida** para começar a criar uma nova conta de armazenamento. Insira um nome exclusivo para a conta e selecione uma **região** na lista. Clique em **criar conta de armazenamento** para continuar.

    ![Criando uma nova conta de armazenamento](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Criando uma nova conta de armazenamento")

    *Criando uma nova conta de armazenamento*
3. Na seção **armazenamento** , aguarde até que o status da nova conta de armazenamento seja alterado para *online* a fim de continuar com a etapa a seguir.

    ![Conta de armazenamento criada](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Conta de armazenamento criada")

    *Conta de armazenamento criada*
4. Clique no nome da conta de armazenamento e, em seguida, clique no link **painel** na parte superior da página. A página **painel** fornece informações sobre o status da conta e os pontos de extremidade de serviço que podem ser usados em seus aplicativos.

    ![Exibindo o painel da conta de armazenamento](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Exibindo o painel da conta de armazenamento")

    *Exibindo o painel da conta de armazenamento*
5. Clique no botão **gerenciar chaves de acesso** na barra de navegação.

    ![Botão gerenciar chaves de acesso](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Botão gerenciar chaves de acesso")

    *Botão gerenciar chaves de acesso*
6. Na caixa de diálogo **gerenciar chaves de acesso** , copie o **nome da conta de armazenamento** e a chave de **acesso primária** , pois você precisará deles no exercício a seguir. Em seguida, feche a caixa de diálogo.

    ![Caixa de diálogo Gerenciar chave de acesso](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Caixa de diálogo Gerenciar chave de acesso")

    *Caixa de diálogo Gerenciar chave de acesso*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Tarefa 2 – carregando um ativo no armazenamento de BLOBs do Azure

Nesta tarefa, você usará a janela de Gerenciador de Servidores do Visual Studio para se conectar à sua conta de armazenamento. Você, em seguida, crie um contêiner de blob e carregar um arquivo com o logotipo do Geek Quiz para o contêiner.

1. Alterne para a instância do Visual Studio com a solução **GeekQuiz** do exercício anterior.
2. Na barra de menus, selecione **Exibir** e, em seguida, clique em **Gerenciador de servidores**.
3. Em **Gerenciador de servidores**, clique com o botão direito do mouse no nó **Azure** e selecione **conectar-se ao Azure...** . Entre usando o conta Microsoft associado à sua assinatura.

    ![Conectar ao Windows Azure...](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Conecte-se ao Azure*
4. Expanda o nó **Azure** , clique com o botão direito do mouse em **armazenamento** e selecione **anexar armazenamento externo...** .
5. Na caixa de diálogo **Adicionar nova conta de armazenamento** , insira **o nome da conta** e a chave da **conta** que você obteve na tarefa anterior e clique em **OK**.

    ![Caixa de diálogo Adicionar nova conta de armazenamento](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Caixa de diálogo Adicionar nova conta de armazenamento*
6. Sua conta de armazenamento deve aparecer sob o nó de **armazenamento** . Expanda sua conta de armazenamento, clique com o botão direito do mouse em **BLOBs** e selecione **criar contêiner de BLOB...** .

    ![Criar contêiner de BLOB](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Criar Contêiner de Blob")

    *Criar contêiner de BLOB*
7. Na caixa de diálogo **criar contêiner de blob** , insira um nome para o contêiner de BLOB e clique em **OK**.

    ![Caixa de diálogo Criar contêiner de BLOBs](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Caixa de diálogo Criar contêiner de BLOBs")

    *Caixa de diálogo Criar contêiner de BLOBs*
8. O novo contêiner de BLOB deve ser adicionado ao nó **BLOBs** . Altere as permissões de acesso no contêiner para tornar o contêiner público. Para fazer isso, clique com o botão direito do mouse no contêiner **imagens** e selecione **Propriedades**.

    ![Propriedades do contêiner de imagens](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "Propriedades do contêiner de imagens")

    *Propriedades do contêiner de imagens*
9. Na janela **Propriedades** , defina o **acesso de leitura público** para o **contêiner**.

    ![Alterando a propriedade de acesso de leitura público](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Alterando a propriedade de acesso de leitura público")

    *Alterando a propriedade de acesso de leitura público*
10. Quando for solicitado se você tiver certeza de que deseja alterar a propriedade de acesso público, clique em **Sim**.

    ![Aviso de Microsoft Visual Studio](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Aviso de Microsoft Visual Studio")

    *Aviso de Microsoft Visual Studio*
11. Em **Gerenciador de servidores**, clique com o botão direito do mouse no contêiner de blobs de **imagens** e selecione **Exibir contêiner de BLOBs**.

    ![Exibir contêiner de BLOB](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "Exibir contêiner de BLOB")

    *Exibir contêiner de BLOB*
12. O contêiner imagens deve ser aberto em uma nova janela e uma legenda sem entradas deve ser mostrada. Clique no ícone **carregar** para carregar um arquivo no contêiner de BLOB.

    ![Contêiner de imagens sem entradas](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Contêiner de imagens sem entradas")

    *Contêiner de imagens sem entradas*
13. Na caixa de diálogo **carregar blob** , navegue até a pasta **ativos** do laboratório. Selecione o arquivo **logo-Big. png** e clique em **abrir**.
14. Aguarde até que o arquivo seja carregado. Quando o carregamento for concluído, o arquivo deverá ser listado no contêiner imagens. Clique com o botão direito do mouse na entrada do arquivo e selecione **Copiar URL**.

    ![Copiar URL do blob](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Copiar URL do arquivo de BLOB")

    *Copiar URL do blob*
15. Abra o Internet Explorer e cole a URL. A imagem a seguir deve ser mostrada no navegador.

    ![imagem logo-Big. png do armazenamento de BLOBs do Windows](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "imagem logo-Big. png do armazenamento")

    *imagem logo-Big. png do armazenamento de BLOBs do Azure*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Tarefa 3 – atualizando a solução para consumir conteúdo estático do armazenamento de BLOBs do Azure

Nesta tarefa, você configurará a solução **GeekQuiz** para consumir a imagem carregada no armazenamento de BLOBs do Azure (em vez da imagem localizada no aplicativo Web) adicionando uma regra de reescrita de URL ASP.net no arquivo **Web. config** .

1. No Visual Studio, abra o arquivo **Web. config** dentro do projeto **GeekQuiz** e localize o elemento **&lt;System. WebServer&gt;** .
2. Adicione o código a seguir para adicionar uma regra de reescrita de URL, atualizando o espaço reservado com o nome da conta de armazenamento.

    (Trecho de código- *WebSitesInProduction-Ex4-UrlRewriteRule*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > A regravação de URL é o processo de interceptar uma solicitação da Web de entrada e de redirecionar a solicitação para um recurso diferente. As regras de regravação de URL dizem ao mecanismo de regravação quando uma solicitação precisa ser redirecionada e onde eles devem ser redirecionados. Uma regra de regravação é composta de duas cadeias de caracteres: o padrão a procurar na URL solicitada (geralmente, usando expressões regulares) e a cadeia de caracteres com a qual substituir o padrão, se encontrado. Para obter mais informações, consulte [regravação de URL em ASP.net](https://msdn.microsoft.com/library/ms972974.aspx).
3. Pressione **Ctrl + S** para salvar as alterações.
4. Abra um novo console do **git bash** para implantar o aplicativo atualizado no serviço Azure app.
5. Execute os comandos a seguir para enviar as alterações por push para o Azure. Atualize o espaço reservado *[Your-Application-Path]* com o caminho para a solução **GeekQuiz** . Você será solicitado a fornecer sua senha de implantação.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Implantando atualização no Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Implantando atualização no Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Tarefa 4 – verificação

Nesta tarefa, você usará o **Internet Explorer** para navegar pelo aplicativo de **teste de especialista** e verificar se a regra de reescrita de URL para imagens funciona e se você é redirecionado para a imagem hospedada no armazenamento de **BLOBs do Azure**.

1. Abra o Internet Explorer e navegue até seu aplicativo Web (por exemplo, `http://<your-web-site>.azurewebsites.net`). Faça logon usando as credenciais criadas anteriormente.

    ![Mostrando o aplicativo Web de teste de especialista com a imagem](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Mostrando o aplicativo Web de teste de especialista com a imagem")

    *Mostrando o aplicativo Web de teste de especialista com a imagem*
2. Pressione **F12** para iniciar as ferramentas de desenvolvimento, selecione a guia **rede** e inicie a gravação.

    ![Iniciando a gravação da rede](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Iniciando a gravação da rede")

    *Iniciando a gravação da rede*
3. Pressione **Ctrl + F5** para atualizar a página da Web.
4. Depois que a página terminar o carregamento, você deverá ver uma solicitação HTTP para a URL **/img/logo-Big.png** com um resultado http **301** (Redirect) e outra solicitação de `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL com um resultado http **200** .

    ![Verificando o redirecionamento de URL](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Mostrando o redirecionamento em ferramentas de desenvolvimento")

    *Verificando o redirecionamento de URL*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Exercício 5: usando o dimensionamento automático para aplicativos Web

> [!NOTE]
> Este exercício é opcional, pois requer suporte para o teste da Web &amp; testes de desempenho, que só está disponível para o **Visual Studio 2013 Ultimate Edition**. Para obter mais informações sobre recursos específicos do Visual Studio 2013, compare as versões [aqui](https://www.microsoft.com/visualstudio/eng/products/compare).

Os **aplicativos Web do serviço Azure app** fornecem o recurso de dimensionamento automático para aplicativos Web em execução no **modo padrão**. O dimensionamento automático permite que o Azure dimensione automaticamente a contagem de instâncias do seu aplicativo Web, dependendo da carga. Quando o dimensionamento automático é habilitado, o Azure verifica a CPU do seu aplicativo Web uma vez a cada cinco minutos e adiciona instâncias conforme necessário nesse ponto no tempo. Se o uso da CPU for baixo, o Azure removerá instâncias uma vez a cada duas horas para garantir que o desempenho do seu aplicativo Web não seja degradado.

Neste exercício, você seguirá as etapas necessárias para configurar o recurso de **dimensionamento automático** para o aplicativo Web de **teste de especialista** . Você verificará esse recurso executando um teste de carga do Visual Studio para gerar carga de CPU suficiente no aplicativo para disparar uma atualização de instância.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Tarefa 1 – Configurando o dimensionamento automático com base na métrica da CPU

Nesta tarefa, você usará o portal de gerenciamento do Azure para habilitar o recurso de dimensionamento automático para o aplicativo Web criado no exercício 2.

1. No [portal de gerenciamento do Azure](https://manage.windowsazure.com/), selecione **sites** e clique no aplicativo Web criado no exercício 2.
2. Navegue até a página **escala** . Na seção **capacidade** , selecione **CPU** para a configuração da **métrica de escala por** .

    > [!NOTE]
    > Ao Dimensionar por CPU, o Azure ajusta dinamicamente o número de instâncias que o aplicativo usa se o uso da CPU for alterado.

    ![Selecionando para dimensionar por CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Selecionando a métrica de CPU para dimensionamento automático")

    *Selecionando para dimensionar por CPU*
3. Altere a configuração de **CPU de destino** para **20**-**40** %.

    > [!NOTE]
    > Esse intervalo representa o uso médio de CPU para seu aplicativo Web. O Azure adicionará ou removerá instâncias para manter seu aplicativo Web nesse intervalo. O número mínimo e máximo de instâncias usadas para dimensionamento é especificado na configuração de **contagem de instâncias** . O Azure nunca vai além desse limite.
    >
    > Os valores de **CPU de destino** padrão são modificados apenas para os fins deste laboratório. Ao configurar o intervalo de CPU com valores pequenos, você está aumentando as chances de disparar o dimensionamento automático quando uma carga moderada é colocada no aplicativo.

    ![Alterando a CPU de destino para estar entre 20 e 40%](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Alterando a CPU de destino para estar entre 20 e 40%")

    *Alterando a CPU de destino para estar entre 20 e 40%*
4. Clique em **salvar** na barra de comandos para salvar as alterações.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Tarefa 2 – teste de carga com o Visual Studio

Agora que o **dimensionamento automático** foi configurado, você criará um **projeto de teste de carga e desempenho na Web** no Visual Studio para gerar alguma carga de CPU em seu aplicativo Web.

1. Abra **Visual Studio Ultimate 2013** e selecione **arquivo | Novo | Projeto...** para iniciar uma nova solução.

    ![Criando um novo projeto](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Criar um novo projeto")

    *Criando um novo projeto*
2. Na caixa de diálogo **novo projeto** , selecione **desempenho da Web e projeto de teste de carga** no **Visual C# | Guia teste** . Verifique se **.NET Framework 4,5** está selecionado, nomeie o projeto *WebAndLoadTestProject*, escolha um **local** e clique em **OK**.

    ![Criando um novo projeto de teste de carga e Web](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Criando um novo projeto de teste de carga e Web")

    *Criando um novo projeto de teste de carga e Web*
3. No **WebTest1. WebTest** , clique com o botão direito do mouse no nó **WebTest1** e clique em **Adicionar solicitação**.

    ![Adicionando uma solicitação ao WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Adicionando uma solicitação ao WebTest1")

    *Adicionando uma solicitação ao WebTest1*
4. Na janela **Propriedades** do novo nó solicitação, atualize a propriedade **URL** para apontar para a URL do seu aplicativo Web (por exemplo, *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)* ).

    ![Alterando a propriedade de URL](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Alterando a propriedade de URL")

    *Alterando a propriedade de URL*
5. Na janela **WebTest1. WebTest** , clique com o botão direito do mouse em **WebTest1** e clique em **Adicionar loop...** .

    ![Adicionando um loop a WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Adicionando um loop a WebTest1")

    *Adicionando um loop a WebTest1*
6. Na caixa de diálogo **Adicionar regra condicional e itens ao loop** , selecione a regra **de loop for** e modifique as propriedades a seguir.

   1. **Valor de encerramento:** 1000
   2. **Nome do parâmetro de contexto:** Repeti
   3. **Valor de incremento:** 1

      ![Selecionando a regra de loop for e atualizando as propriedades](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Selecionando a regra de loop for e atualizando as propriedades")

      *Selecionando a regra de loop for e atualizando as propriedades*
7. Na seção **itens em loop** , selecione a solicitação que você criou anteriormente para ser o primeiro e o último item para o loop. Clique em **OK** para continuar.

    ![Selecionando o primeiro e o último itens para o loop](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Selecionando o primeiro e o último itens para o loop")

    *Selecionando o primeiro e o último itens para o loop*
8. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **WebAndLoadTestProject** , expanda o menu **Adicionar** e selecione **carregar teste..** ..

    ![Adicionando um teste de carga ao projeto WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Adicionando um teste de carga ao projeto WebAndLoadTestProject")

    *Adicionando um teste de carga ao projeto WebAndLoadTestProject*
9. Na caixa de diálogo **novo assistente de teste de carga** , clique em **Avançar**.

    ![Novo Assistente de Teste de Carga](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Novo Assistente de Teste de Carga")

    *Novo Assistente de Teste de Carga*
10. Na página **cenário** , selecione **não usar tempos de refelxão** e clique em **Avançar**.

    ![Selecionando não usar tempos de refelxão](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Selecionando não usar tempos de refelxão")

    *Selecionando não usar tempos de refelxão*
11. Na página **padrão de carga** , verifique se a opção de **carregar constante** está selecionada. Altere a configuração de **contagem de usuários** para **250** usuários e clique em **Avançar**.

    ![Alterando a contagem de usuários para 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Alterando a contagem de usuários para 250")

    *Alterando a contagem de usuários para 250*
12. Na página **modelo de combinação de teste** , selecione **com base na ordem de teste sequencial** e clique em **Avançar**.

    ![Selecionando o modelo de combinação de teste](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Selecionando o modelo de combinação de teste")

    *Selecionando o modelo de combinação de teste*
13. Na página **modelo de combinação de teste** , clique em **Adicionar...** para adicionar um teste à combinação.

    ![Adicionando um teste à combinação de testes](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Adicionando um teste à combinação de testes")

    *Adicionando um teste à combinação de testes*
14. Na caixa de diálogo **Adicionar testes** , clique duas vezes em **WebTest1** para adicionar o teste à lista de **testes selecionados** . Clique em **OK** para continuar.

    ![Adicionando o teste WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Adicionando o teste WebTest1")

    *Adicionando o teste WebTest1*
15. De volta à página de **combinação de testes** , clique em **Avançar**.

    ![Concluindo a página de combinação de testes](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Concluindo a página de combinação de testes")

    *Concluindo a página de combinação de testes*
16. Na página **combinação de rede** , clique em **Avançar**.

    ![Clicando em avançar na página combinação de rede](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Clicando em avançar na página combinação de rede")

    *Clicando em avançar na página combinação de rede*
17. Na página de **combinação de navegador** , selecione **Internet Explorer 10,0** como o tipo de navegador e clique em **Avançar**.

    ![Selecionando o tipo de navegador](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Selecionando o tipo de navegador")

    *Selecionando o tipo de navegador*
18. Na página **conjuntos de contadores** , clique em **Avançar**.

    ![Clicando em avançar na página conjuntos de contadores](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Clicando em avançar na página conjuntos de contadores")

    *Clicando em avançar na página conjuntos de contadores*
19. Na página **configurações de execução** , defina a **duração do teste de carga** para **5 minutos** e clique em **concluir**.

    ![Definindo a duração do teste de carga para 5 minutos](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Definindo a duração do teste de carga para 5 minutos")

    *Definindo a duração do teste de carga para 5 minutos*
20. Em **Gerenciador de soluções**, clique duas vezes no arquivo **local. Settings** para explorar as configurações do teste. Por padrão, o Visual Studio usa o computador local para executar os testes.

    > [!NOTE]
    > Como alternativa, você pode configurar seu projeto de teste para executar os testes de carga na nuvem usando **Azure Test Plans**. O Azure Test Plans fornece um serviço de teste de carga baseado em nuvem que simula uma carga mais realista, evitando restrições de ambiente local, como capacidade de CPU, memória disponível e largura de banda de rede. Para obter mais informações sobre como usar Azure Test Plans para executar testes de carga, consulte [cenários de teste de carga](/azure/devops/test/load-test/overview?view=vsts).

    ![Configurações de teste](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Tarefa 3 – verificação de dimensionamento automático

Agora, você executará o teste de carga criado na tarefa anterior e verá como seu aplicativo Web se comporta sob carga.

1. Em **Gerenciador de soluções**, clique duas vezes em **loadtest1. LoadTest** para abrir o teste de carga.

    ![Abrindo LoadTest1. LoadTest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Abrindo LoadTest1. LoadTest")

    *Abrindo LoadTest1. LoadTest*
2. Na janela **loadtest1. LoadTest** , clique no primeiro botão na caixa de ferramentas para executar o teste de carga.

    ![Executando o teste de carga](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Executando o teste de carga")

    *Executando o teste de carga*
3. Aguarde até que o teste de carga seja concluído.

    > [!NOTE]
    > O teste de carga simula vários usuários que enviam solicitações ao aplicativo Web simultaneamente. Quando o teste estiver em execução, você poderá monitorar os contadores disponíveis para detectar erros, avisos ou outras informações relacionadas à execução do teste de carga.

    ![Execução de teste de carga](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "Aguardando até que o teste de carga seja concluído")

    *Execução de teste de carga*
4. Quando o teste for concluído, volte para o portal de gerenciamento e navegue até a página **escala** do seu aplicativo Web. Na seção **capacidade** , você deve ver no grafo que uma nova instância foi implantada automaticamente.

    ![Nova instância implantada automaticamente](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Nova instância implantada automaticamente*

    > [!NOTE]
    > Pode levar vários minutos para que as alterações apareçam no grafo (pressione **Ctrl + F5** periodicamente para atualizar a página). Se você não vir nenhuma alteração, poderá tentar o seguinte:
    >
    > - Aumentar a duração do teste de carga (por exemplo, para **10 minutos**)
    > - Reduzir os valores máximo e mínimo do intervalo de **CPU de destino** na configuração de dimensionamento automático do seu aplicativo Web
    > - Execute o teste de carga na nuvem com **Azure Test Plans**. Mais informações podem ser obtidas [aqui](/azure/devops/test/load-test/index?view=vsts)

---

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Neste laboratório prático, você aprendeu a configurar e implantar seu aplicativo em aplicativos Web de produção no Azure. Você começou detectando e atualizando seus bancos de dados usando o **migrações do Entity Framework Code First**e, em seguida, continuou implantando novas versões do seu site usando o **git** e executando reversões para a versão estável mais recente do seu site. Além disso, você aprendeu a dimensionar seu aplicativo usando o armazenamento para mover seu conteúdo estático para um contêiner de BLOBs.
