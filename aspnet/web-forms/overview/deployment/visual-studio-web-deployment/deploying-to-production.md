---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'Implantação da Web do ASP.NET usando o Visual Studio: implantando na produção | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros, por usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: ddc3d15f0436c4c3a24491cf0377111768da67df
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74617636"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Implantação da Web do ASP.NET usando o Visual Studio: implantando na produção

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros usando o Visual Studio 2012 ou o Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial da série](introduction.md).

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

Neste tutorial, você configura uma conta de Microsoft Azure, cria ambientes de preparo e produção e implanta seu aplicativo Web ASP.NET nos ambientes de preparo e produção usando o recurso de publicação com um clique do Visual Studio.

Se preferir, você pode implantar em um provedor de Hospedagem de terceiros. A maioria dos procedimentos descritos neste tutorial é a mesma para um provedor de hospedagem ou para o Azure, exceto que cada provedor tem sua própria interface de usuário para o gerenciamento de contas e sites. Você pode encontrar um provedor de hospedagem na [Galeria de provedores](https://www.microsoft.com/web/hosting) no site do Microsoft.com.

Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a página de solução de problemas nesta série de tutoriais.

## <a name="get-a-microsoft-azure-account"></a>Obter uma conta de Microsoft Azure

Se você ainda não tiver uma conta do Azure, poderá criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [avaliação gratuita do Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Criar um ambiente de preparo

> [!NOTE]
> Como este tutorial foi escrito, Azure App serviço adicionou um novo recurso para automatizar muitos dos processos de criação de ambientes de preparo e produção. Consulte [configurar ambientes de preparo para aplicativos Web no serviço Azure app](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).

Conforme explicado no [tutorial implantar no ambiente de teste](deploying-to-iis.md), o ambiente de teste mais confiável é um site no provedor de hospedagem que é exatamente como o site de produção. Em muitos provedores de hospedagem, você teria que avaliar os benefícios disso com relação ao custo adicional significativo, mas no Azure você pode criar um aplicativo Web gratuito adicional como seu aplicativo de preparo. Você também precisa de um banco de dados, e a despesa adicional para isso durante a despesa do seu banco de dados de produção será None ou Minimal. No Azure, você paga pela quantidade de armazenamento de banco de dados usada em vez de para cada banco de dados, e a quantidade de armazenamento adicional que você usará na preparação será mínima.

Conforme explicado no [tutorial implantar no ambiente de teste](deploying-to-iis.md), em preparo e produção, você implantará seus dois bancos de dados em um banco. Se você quisesse mantê-los separados, o processo seria o mesmo, exceto que você criaria um banco de dados adicional para cada ambiente e selecionaria a cadeia de caracteres de destino correta para cada banco de dados ao criar o perfil de publicação.

Nesta seção do tutorial, você criará um aplicativo Web e um banco de dados a serem usados para o ambiente de preparo, e você implantará em preparo e teste lá antes de criar e implantar no ambiente de produção.

> [!NOTE]
> As etapas a seguir mostram como criar um aplicativo Web no serviço Azure App usando o portal de gerenciamento do Azure. Na versão mais recente do SDK do Azure, você também pode fazer isso sem sair do Visual Studio, usando Gerenciador de Servidores. No Visual Studio 2013, você também pode criar um aplicativo Web diretamente da caixa de diálogo de publicação. Para obter mais informações, consulte [criar um aplicativo web ASP.net no serviço Azure app.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)

1. Na [portal de gerenciamento do Azure](https://manage.windowsazure.com/), clique em **sites**e em **novo**.
2. Clique em **site**e em **criação personalizada**.

    O assistente **novo site-criação personalizada** é aberto. O assistente de **criação personalizada** permite que você crie um site da Web e um banco de dados ao mesmo tempo.
3. Na etapa **criar site** do assistente, insira uma cadeia de caracteres na caixa **URL** para usar como a URL exclusiva para o ambiente de preparo do seu aplicativo. Por exemplo, digite ContosoUniversity-staging123 (incluindo números aleatórios no final para torná-lo exclusivo no caso de ContosoUniversity-preparo ser obtido).

    A URL completa consistirá no que você inserir aqui, além do sufixo que você vê ao lado da caixa de texto.
4. Na lista suspensa **região** , escolha a região mais próxima de você.

    Essa configuração especifica em qual data center seu aplicativo Web será executado.
5. Na lista suspensa **banco de dados** , escolha **criar um novo banco de dados SQL**.
6. Na caixa **nome da cadeia de conexão do BD** , deixe o valor padrão, *DefaultConnection*.
7. Clique na seta que aponta para a direita na parte inferior da caixa.

    A ilustração a seguir mostra a caixa de diálogo **criar site** com valores de exemplo. A URL e a região que você inseriu serão diferentes.

    ![Criar etapa do site](deploying-to-production/_static/image1.png)

    O assistente avança para a etapa **especificar configurações de banco de dados** .
8. Na caixa **nome** , digite *ContosoUniversity* mais um número aleatório para torná-lo exclusivo, por exemplo, *ContosoUniversity123*.
9. Na caixa **servidor** , selecione **novo servidor de banco de dados SQL**.
10. Insira um nome de administrador e uma senha.

    Você não está inserindo um nome e senha existentes aqui. Você está inserindo um novo nome e senha que você está definindo agora para usar posteriormente ao acessar o banco de dados.
11. Na caixa **região** , escolha a mesma região que você escolheu para o aplicativo Web.

    Manter o servidor Web e o servidor de banco de dados na mesma região oferece o melhor desempenho e minimiza as despesas.
12. Clique na marca de seleção na parte inferior da caixa para indicar que você terminou.

    A ilustração a seguir mostra a caixa de diálogo **especificar configurações de banco de dados** com valores de exemplo. Os valores que você inseriu podem ser diferentes.

    ![Configurações de banco de dados etapa do novo site-criar com o assistente de banco de dados](deploying-to-production/_static/image2.png)

    O Portal de Gerenciamento retorna à página de sites e a coluna **status** mostra que o aplicativo Web está sendo criado. Após um tempo (normalmente menos de um minuto), a coluna **status** mostra que o aplicativo Web foi criado com êxito. Na barra de navegação à esquerda, o número de aplicativos Web que você tem em sua conta aparece ao lado do ícone **sites** e o número de bancos de dados é exibido ao lado do ícone **bancos de dados SQL** .

    ![Página de sites da Portal de Gerenciamento, site da Web criado](deploying-to-production/_static/image3.png)

    O nome do aplicativo Web será diferente do aplicativo de exemplo na ilustração.

## <a name="deploy-the-application-to-staging"></a>Implantar o aplicativo para preparo

Agora que você criou um aplicativo Web e um banco de dados para o ambiente de preparo, você pode implantar o projeto nele.

> [!NOTE]
> Estas instruções mostram como criar um perfil de publicação baixando um arquivo *. publishsettings* , que funciona não apenas para o Azure, mas também para provedores de Hospedagem de terceiros. O SDK do Azure mais recente também permite que você se conecte diretamente ao Azure por meio do Visual Studio e escolha em uma lista de aplicativos Web que você tem em sua conta do Azure. No Visual Studio 2013, você pode entrar no Azure por meio da caixa de diálogo **publicação na Web** ou da janela **Gerenciador de servidores** . Para obter mais informações, consulte [criar um aplicativo web ASP.net no serviço Azure app](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

### <a name="download-the-publishsettings-file"></a>Baixar o arquivo. publishsettings

1. Clique no nome do aplicativo Web que você acabou de criar.

    ![Clique no site para ir para o painel](deploying-to-production/_static/image4.png)
2. Em **visão rápida** na guia **painel** , clique em **baixar perfil de publicação**.

    ![Baixar link do perfil de publicação](deploying-to-production/_static/image5.png)

    Esta etapa baixa um arquivo que contém todas as configurações necessárias para implantar um aplicativo em seu aplicativo Web. Você importará esse arquivo para o Visual Studio para que não precise inserir essas informações manualmente.
3. Salve o arquivo *. publishsettings* em uma pasta que você pode acessar por meio do Visual Studio.

    ![salvando o arquivo. publishsettings](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Segurança-o arquivo *. publishsettings* contém suas credenciais (sem codificação) que são usadas para administrar suas assinaturas e serviços do Azure. A prática recomendada de segurança para esse arquivo é armazená-lo temporariamente fora dos diretórios de origem (por exemplo, na pasta Libraries\Documents) e, em seguida, excluí-lo após a conclusão da importação. Um usuário mal-intencionado que obtém acesso ao arquivo *. publishsettings* pode editar, criar e excluir seus serviços do Azure.

### <a name="create-a-publish-profile"></a>Criar um perfil de publicação

1. No Visual Studio, clique com o botão direito do mouse no projeto ContosoUniversity em **Gerenciador de soluções** e selecione **publicar** no menu de contexto.

    O assistente **publicar Web** é aberto.
2. Clique na guia **perfil** .
3. Clique em **Importar**.
4. Navegue até o arquivo *. publishsettings* baixado anteriormente e clique em **abrir**.

    ![Caixa de diálogo Importar configurações de publicação](deploying-to-production/_static/image7.png)
5. Na guia **conexão** , clique em **validar conexão** para certificar-se de que as configurações estão corretas.

    Quando a conexão for validada, uma marca de seleção verde será mostrada ao lado do botão **validar conexão** .

    Para alguns provedores de hospedagem, ao clicar em **validar conexão**, você poderá ver uma caixa de diálogo de **erro de certificado** . Se você fizer isso, verifique se o nome do servidor é o esperado. Se o nome do servidor estiver correto, selecione **salvar este certificado para futuras sessões do Visual Studio** e clique em **aceitar**. (Esse erro significa que o provedor de hospedagem optou por evitar a despesa de comprar um certificado SSL para a URL na qual você está implantando. Se preferir estabelecer uma conexão segura usando um certificado válido, entre em contato com seu provedor de hospedagem.)
6. Clique em **Avançar**.

    ![ícone de conexão com êxito e botão Avançar na guia conexão](deploying-to-production/_static/image8.png)
7. Na guia **configurações** , expanda **Opções de publicação de arquivo**e, em seguida, selecione **Excluir arquivos da pasta de dados de\_de aplicativos**.

    Para obter informações sobre as outras opções em **Opções de publicação de arquivo**, consulte o tutorial [implantando no IIS](deploying-to-iis.md) . A captura de tela que mostra o resultado dessa etapa e as seguintes etapas de configuração do banco de dados estão no final das etapas de configuração do banco de dados.
8. Em **DefaultConnection** na seção **bancos** de dados, configure Database Deployment for the Membership Database.
9. 1. Selecione **Atualizar banco de dados**.

        A caixa **cadeia de conexão remota** diretamente abaixo de **DefaultConnection** é preenchida com a cadeia de conexão do arquivo. publishsettings. A cadeia de conexão inclui SQL Server credenciais, que são armazenadas em texto sem formatação no arquivo *. pubxml* . Se preferir não armazená-los permanentemente, você poderá removê-los do perfil de publicação depois que o banco de dados for implantado e armazená-los no Azure em vez disso. Para obter mais informações, consulte [como manter suas cadeias de conexão de banco de dados ASP.net seguras ao implantar no Azure da origem](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) no blog de Scott Hanselman.
      2. Clique em **Configurar atualizações de banco de dados**.
      3. Na caixa de diálogo **Configurar atualizações de banco de dados** , clique em **adicionar script SQL**.
      4. Na caixa **adicionar script SQL** , navegue até o script *ASPNET-data-prod. SQL* que você salvou anteriormente na pasta da solução e clique em **abrir**.
      5. Feche a caixa de diálogo **Configurar atualizações de banco de dados** .
10. Em **SchoolContext** na seção **bancos de dados** , selecione **executar migrações do Code First (é executado no início do aplicativo)** .

    O Visual Studio exibe **executar migrações do Code First** em vez de **atualizar o banco de dados** para `DbContext` classes. Se você quiser usar o provedor dbDacFx em vez de migrações para implantar um banco de dados que você acessa usando uma classe `DbContext`, consulte [como fazer implantar um banco de dados Code First sem migrações?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) nas perguntas frequentes sobre implantação na Web para Visual Studio e ASP.net no msdn.

    A guia **configurações** agora é semelhante ao exemplo a seguir:

    ![Guia Configurações para preparo](deploying-to-production/_static/image9.png)
11. Execute as seguintes etapas para salvar o perfil e renomeá-lo para *preparo*:

    1. Clique na guia **perfil** e, em seguida, clique em **gerenciar perfis**.
    2. A importação criou dois novos perfis, um para FTP e outro para Implantação da Web. Você configurou o perfil de Implantação da Web: renomeie este perfil para *preparo*.

        ![Renomear perfil para preparo](deploying-to-production/_static/image10.png)
    3. Feche a caixa de diálogo **Editar perfis de publicação na Web** .
    4. Feche o assistente **publicar Web** .

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Configurar uma transformação publicar perfil para o indicador do ambiente

> [!NOTE]
> Esta seção mostra como configurar uma transformação de Web. config para o indicador de ambiente. Como o indicador está no elemento `<appSettings>`, você tem outra alternativa para especificar a transformação quando estiver implantando no serviço Azure App. Para obter mais informações, consulte [especificando as configurações do Web. config no Azure](web-config-transformations.md#watransforms).

1. Em **Gerenciador de soluções**, expanda **Propriedades**e, em seguida, expanda **PublishProfiles**.
2. Clique com o botão direito do mouse em *preparo. pubxml*e clique em **Adicionar transformação de configuração**.

    ![Adicionar transformação de configuração para preparo](deploying-to-production/_static/image11.png)

    O Visual Studio cria o arquivo de transformação *Web. staging. config* e o abre.
3. No arquivo de transformação *Web. staging. config* , insira o código a seguir imediatamente após a marca de `configuration` de abertura.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Quando você usa o perfil de publicação de preparo, essa transformação define o indicador de ambiente como "Prod". No aplicativo Web implantado, você não verá nenhum sufixo como "(dev)" ou "(teste)" após o título H1 "Contoso University".
4. Clique com o botão direito do mouse no arquivo *Web. staging. config* e clique em **Visualizar transformação** para certificar-se de que a transformação codificada produz as alterações esperadas.

    A janela de **Visualização Web. config** mostra o resultado da aplicação das transformações *Web. Release. config* e das transformações *Web. preparo. config* .

### <a name="prevent-public-use-of-the-test-app"></a>Impedir o uso público do aplicativo de teste

Uma consideração importante para o aplicativo de preparo é que ele estará ativo na Internet, mas você não quer que o público o use. Para minimizar a probabilidade de as pessoas encontrá-la e usá-la, você pode usar um ou mais dos seguintes métodos:

- Defina as regras de firewall que permitem o acesso ao aplicativo de preparo somente de endereços IP que você usa para testar o preparo.
- Use uma URL ofuscada que seria impossível de adivinhar.
- Crie um arquivo *robots. txt* para garantir que os mecanismos de pesquisa não rastreiem o aplicativo de teste e os links de relatório para ele nos resultados da pesquisa.

O primeiro desses métodos é o mais eficiente, mas não é abordado neste tutorial porque ele exigiria que você implante em um serviço de nuvem do Azure em vez de Azure App serviço. Para obter mais informações sobre os serviços de nuvem e as restrições de IP no Azure, consulte [Opções de Hospedagem de computação fornecidas pelo Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) e [bloquear endereços IP específicos de acessar uma função Web](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Se você estiver implantando em um provedor de Hospedagem de terceiros, entre em contato com o provedor para descobrir como implementar restrições de IP.

Para este tutorial, você criará um arquivo *robots. txt* .

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto ContosoUniversity e clique em **Adicionar novo item**.
2. Crie um novo **arquivo de texto** chamado *robots. txt*e coloque o seguinte texto nele:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    A linha de `User-agent` informa aos mecanismos de pesquisa que as regras no arquivo se aplicam a todos os robôs (rastreadores da Web) do mecanismo de pesquisa e a linha de `Disallow` especifica que nenhuma página no site deve ser rastreada.

    Você deseja que os mecanismos de pesquisa catálogom seu aplicativo de produção, portanto, você precisa excluir esse arquivo da implantação de produção. Para fazer isso, você definirá uma configuração no perfil de publicação de produção ao criá-lo.

### <a name="deploy-to-staging"></a>Implantar para preparo

1. Abra o assistente **publicar na Web** clicando com o botão direito do mouse no projeto Contoso University e clicando em **publicar**.
2. Verifique se o perfil de **preparo** está selecionado.
3. Clique em **Publicar**.

    A janela **saída** mostra quais ações de implantação foram executadas e relata a conclusão bem-sucedida da implantação. O navegador padrão é aberto automaticamente para a URL do aplicativo Web implantado.

## <a name="test-in-the-staging-environment"></a>Testar no ambiente de preparo

Observe que o indicador de ambiente está ausente (não há "(teste)" ou "(dev)" após o rumo H1, que mostra que a transformação *Web. config* do indicador de ambiente foi bem-sucedida.

![Preparo da Home Page](deploying-to-production/_static/image12.png)

Execute a página **alunos** para verificar se o banco de dados implantado não tem nenhum aluno.

Execute a página de **instrutores** para verificar se Code First propagam o banco de dados com o instrutor:

Selecione **adicionar alunos** no menu **alunos** , adicione um aluno e, em seguida, exiba o novo aluno na página **alunos** para verificar se você pode gravar com êxito no banco de dados.

Na página **cursos** , clique em **Atualizar créditos**. A página **créditos de atualização** requer permissões de administrador, portanto, a página de **logon** é exibida. Insira as credenciais de conta de administrador que você criou anteriormente ("admin" e "prodpwd"). A página **créditos de atualização** é exibida, que verifica se a conta de administrador que você criou no tutorial anterior foi implantada corretamente no ambiente de teste.

Solicite uma URL inválida para causar um erro que o ELMAH controlará e, em seguida, solicite o relatório de erros do ELMAH. Se estiver implantando em um provedor de Hospedagem de terceiros, você provavelmente descobrirá que o relatório está vazio pelo mesmo motivo que ele estava vazio no tutorial anterior. Você precisará usar as ferramentas de gerenciamento de conta do provedor de hospedagem para configurar permissões de pasta para habilitar o ELMAH a gravar na pasta de log.

O aplicativo que você criou agora está em execução na nuvem em um aplicativo Web, assim como o que você usará para produção. Como tudo está funcionando corretamente, a próxima etapa é implantar na produção.

## <a name="deploy-to-production"></a>Implantar na produção

O processo de criação de um aplicativo Web de produção e implantação em produção é o mesmo para preparo, exceto pelo fato de que você precisa excluir o *robots. txt* da implantação. Para fazer isso, você editará o arquivo de perfil de publicação.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Criar o ambiente de produção e o perfil de publicação de produção

1. Crie o aplicativo Web de produção e o banco de dados no Azure, seguindo o mesmo procedimento que você usou para preparo.

    Ao criar o banco de dados, você pode optar por colocá-lo no mesmo servidor criado anteriormente ou criar um novo servidor.
2. Baixe o arquivo *. publishsettings* .
3. Crie o perfil de publicação importando o arquivo Production *. publishsettings* , seguindo o mesmo procedimento que você usou para preparo.

    Não se esqueça de configurar o script de implantação de dados em **DefaultConnection** na seção **databases** da guia **Settings** .
4. Renomeie o perfil de publicação para *produção*.
5. Configure uma transformação de perfil de publicação para o indicador de ambiente, seguindo o mesmo procedimento que você usou para preparo..

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Edite o arquivo. pubxml para excluir robots. txt

Os arquivos de perfil de publicação são nomeados &lt;ProfileName&gt; *. pubxml* e estão localizados na pasta *PublishProfiles* A *pasta PublishProfiles* está sob a pasta *Propriedades* em um C# projeto de aplicativo Web, na pasta *meu projeto* em um projeto de aplicativo web vb ou na pasta *dados* do aplicativo\_em um projeto de aplicativo Web. Cada arquivo *. pubxml* contém configurações que se aplicam a um perfil de publicação. Os valores inseridos no assistente publicar Web são armazenados nesses arquivos, e você pode editá-los para criar ou alterar as configurações que não estão disponíveis na interface do usuário do Visual Studio.

Por padrão, os arquivos *. pubxml* são incluídos no projeto quando você cria um perfil de publicação, mas você pode excluí-los do projeto e o Visual Studio ainda os usará. O Visual Studio procura na pasta *PublishProfiles* arquivos *. pubxml* , independentemente de eles estarem incluídos no projeto.

Para cada arquivo *. pubxml* , há um arquivo *. pubxml. User* . O arquivo *. pubxml. User* contém a senha criptografada se você selecionou a opção **salvar senha** e, por padrão, ela é excluída do projeto.

Um arquivo *. pubxml* contém as configurações que pertencem a um perfil de publicação específico. Se você quiser definir as configurações que se aplicam a todos os perfis, poderá criar um arquivo *. WPP. targets* . O processo de compilação importa esses arquivos para o arquivo de projeto *. csproj* ou *. vbproj* , de modo que a maioria das configurações que você pode configurar no arquivo de projeto pode ser configurada nesses arquivos. Para obter mais informações sobre arquivos *. pubxml* e *. WPP. targets* , consulte [como: Editar configurações de implantação em arquivos de perfil de publicação (. pubxml) e o arquivo. WPP. targets em projetos Web do Visual Studio](https://msdn.microsoft.com/library/ff398069.aspx).

1. Em **Gerenciador de soluções**, expanda **Propriedades** e expanda **PublishProfiles**.
2. Clique com o botão direito do mouse em *Production. pubxml* e clique em **abrir**.

    ![Abra o arquivo. pubxml](deploying-to-production/_static/image13.png)
3. Clique com o botão direito do mouse em *Production. pubxml* e clique em **abrir**.
4. Adicione as seguintes linhas imediatamente antes do elemento `PropertyGroup` de fechamento:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    O arquivo. pubxml agora é semelhante ao exemplo a seguir:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Para obter mais informações sobre como excluir arquivos e pastas, consulte posso [Excluir arquivos ou pastas específicas da implantação?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) nas **perguntas frequentes sobre implantação da Web para Visual Studio e ASP.net** no msdn.

### <a name="deploy-to-production"></a>Implantar na produção

1. Abra o assistente **publicar Web** Verifique se o perfil de publicação de **produção** está selecionado e, em seguida, clique em **Iniciar visualização** na guia **Visualizar** para verificar se o arquivo *robots. txt* não será copiado para o aplicativo de produção.

    ![Visualização de arquivos a serem publicados na produção](deploying-to-production/_static/image14.png)

    Examine a lista de arquivos que serão copiados. Você verá que todos os arquivos *. cs* , incluindo arquivos *. aspx.cs*, *. aspx.designer.cs*, *Master.cs*e *Master.designer.cs* são omitidos. Todo esse código foi compilado nos arquivos *ContosoUniversity. dll* e *ContosoUniversity. pdb* que você encontrará na pasta *bin* . Como apenas o *. dll* é necessário para executar o aplicativo e você especificou anteriormente que somente os arquivos necessários para executar o aplicativo devem ser implantados, nenhum arquivo *. cs* foi copiado para o ambiente de destino. A pasta *obj* e os arquivos *ContosoUniversity. csproj* e *. csproj. User* são omitidos pelo mesmo motivo.

    Clique em **publicar** para implantar no ambiente de produção.
2. Teste em produção, seguindo o mesmo procedimento usado para preparo.

    Tudo é idêntico ao preparo, exceto a URL e a ausência do arquivo *robots. txt* .

## <a name="summary"></a>Resumo

Agora você implantou e testou com êxito seu aplicativo Web e ele está disponível publicamente pela Internet.

![Produção da Home Page](deploying-to-production/_static/image15.png)

No próximo tutorial, você atualizará o código do aplicativo e implantará a alteração nos ambientes de teste, de preparo e de produção.

> [!NOTE]
> Enquanto seu aplicativo está em uso no ambiente de produção, você deve implementar um plano de recuperação. Ou seja, você deve fazer backup periodicamente de seus bancos de dados do aplicativo de produção para um local de armazenamento seguro, e deve manter várias gerações desses backups. Ao atualizar o banco de dados, você deve fazer uma cópia de backup imediatamente antes da alteração. Em seguida, se você cometer um erro e não o descobrir até depois de implantá-lo na produção, ainda poderá recuperar o banco de dados para o estado em que estava antes de ele ser corrompido. Para obter mais informações, consulte [backup e restauração do banco de dados SQL do Azure](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> Neste tutorial, a edição SQL Server na qual você está implantando é o banco de dados SQL do Azure. Embora o processo de implantação seja semelhante a outras edições do SQL Server, um aplicativo de produção real pode exigir um código especial para o banco de dados SQL do Azure em alguns cenários. Para obter mais informações, consulte [trabalhando com o banco de dados SQL do Azure](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) e [escolhendo entre SQL Server e o banco de dados SQL do Azure](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Anterior](setting-folder-permissions.md)
> [Próximo](deploying-a-code-update.md)
