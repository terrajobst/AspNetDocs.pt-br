---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'Implantando um aplicativo Web ASP.NET com o SQL Server Compact usando o Visual Studio ou o Visual Web Developer: implantando no ambiente de produção-7 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: db838633accdedd7c0693b126a007e254ca681e4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78568015"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Implantando um aplicativo Web ASP.NET com o SQL Server Compact usando o Visual Studio ou o Visual Web Developer: implantando no ambiente de produção-7 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Studio 2012 RC ou o Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial da série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar SQL Server edições diferentes de SQL Server Compact e mostra como implantar o Azure App aplicativos Web do serviço, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Visão geral

Neste tutorial, você configura uma conta com um provedor de hospedagem e implanta seu aplicativo Web ASP.NET no ambiente de produção usando o recurso de publicação com um clique do Visual Studio.

Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Selecionando um provedor de hospedagem

Para o aplicativo da Contoso University e esta série de tutoriais, você precisa de um provedor que dê suporte a ASP.NET 4 e Implantação da Web. Uma empresa de hospedagem específica foi escolhida para que os tutoriais possam ilustrar a experiência completa de implantação em um site ativo. Cada empresa de hospedagem fornece recursos diferentes, e a experiência de implantação em seus servidores varia de certa forma. No entanto, o processo descrito neste tutorial é típico para o processo geral. O provedor de hospedagem usado para este tutorial, Cytanium.com, é um dos muitos que estão disponíveis e seu uso neste tutorial não constitui um endosso ou recomendação.

Quando estiver pronto para selecionar seu próprio provedor de hospedagem, você poderá comparar recursos e preços na [Galeria de provedores](https://www.microsoft.com/web/hosting) no site do Microsoft.com/Web.

## <a name="creating-an-account"></a>Criando uma conta

Crie uma conta no provedor selecionado. Se o suporte para um banco de dados SQL Server completo for um extra adicional, você não precisará selecioná-lo para este tutorial, mas você precisará dele para o tutorial [migrar para SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) mais adiante nesta série.

Para esses tutoriais, você não precisa registrar um novo nome de domínio. Você pode testar para verificar a implantação bem-sucedida usando a URL temporária atribuída ao site pelo provedor.

Depois que a conta tiver sido criada, você normalmente receberá um email de boas-vindas que contém todas as informações necessárias para implantar e gerenciar seu site. As informações que seu provedor de hospedagem envia serão semelhantes ao que é mostrado aqui. O email de boas-vindas do Cytanium enviado para novos proprietários da conta inclui as seguintes informações:

- A URL para o site do painel de controle do provedor, onde você pode gerenciar as configurações do seu site. A ID e a senha que você especificou estão incluídas nesta parte do email de boas-vindas para facilitar a referência. (Ambos foram alterados para um valor de demonstração para esta ilustração.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- A versão .NET Framework padrão e informações sobre como alterá-la. Muitos sites de hospedagem assumem como padrão o 2,0, que funciona com aplicativos ASP.NET destinados ao .NET Framework 2,0, 3,0 ou 3,5. No entanto, a Contoso University é um aplicativo .NET Framework 4, portanto, você precisa alterar essa configuração. (Para um aplicativo ASP.NET 4,5, você usaria a configuração do .NET 4,0.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- A URL temporária que você pode usar para acessar seu site da Web. Quando essa conta foi criada, "contosouniversity.com" foi inserido como o nome de domínio existente. Portanto, a URL temporária é `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Informações sobre como configurar bancos de dados e as cadeias de conexão de que você precisa para acessá-los:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Informações sobre as ferramentas e configurações para implantar seu site. (O email de Cytanium também menciona WebMatrix, que é omitido aqui.)

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Definindo a versão de .NET Framework

O email de boas-vindas do Cytanium inclui um link para obter instruções sobre como alterar a versão do .NET Framework. Estas instruções explicam que isso pode ser feito por meio do painel de controle do Cytanium. Outros provedores têm sites do painel de controle que parecem diferentes ou podem instruí-lo a fazer isso de maneira diferente.

Vá para a URL do painel de controle. Depois de fazer logon com seu nome de usuário e senha, você verá o painel de controle.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

Na caixa **espaços de hospedagem** , mantenha o ponteiro do mouse sobre o ícone da Web e selecione **sites** no menu.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

Na caixa **sites** , clique em **contosouniversity.com** (o nome do site que você usou quando criou a conta).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

Na caixa **Propriedades do site** , selecione a guia **extensões** .

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Altere ASP.NET do **pipeline integrado 2,0** para **4,0 (pipeline integrado)** e clique em **Atualizar**.

## <a name="publishing-to-the-hosting-provider"></a>Publicando no provedor de hospedagem

O email de boas-vindas do provedor de hospedagem inclui todas as configurações necessárias para publicar o projeto e você pode inserir essas informações manualmente em um perfil de publicação. Mas você usará um método mais fácil e menos propenso a erros para configurar a implantação no provedor: você baixará um arquivo *. publishsettings* e o importará para um perfil de publicação.

No navegador, vá para o painel de controle do Cytanium e selecione **Web** e, em seguida, selecione **sites.**

![Painel de controle selecionando sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Selecione o site da **contosouniversity.com** .

![Painel de controle selecionando contosouniversity.com](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Selecione a guia **publicação na Web** .

![Guia de publicação na Web do painel de controle](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Crie credenciais a serem usadas para publicação na Web inserindo um nome de usuário e senha. Você pode inserir as mesmas credenciais que usa para fazer logon no painel de controle. Em seguida, clique em **Habilitar**.

![Criar credenciais de publicação no painel de controle](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Clique em **baixar perfil de publicação para este site**.

![Baixar o perfil de publicação do painel de controle](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Quando for solicitado que você abra ou salve o arquivo, salve-o.

![Salvar arquivo de perfil de publicação](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

Em **Gerenciador de soluções** no Visual Studio, clique com o botão direito do mouse no projeto ContosoUniversity e selecione **publicar**. A caixa de diálogo **publicar Web** é aberta na guia **Visualizar** com o perfil de **teste** selecionado porque esse é o último perfil usado.

Selecione a guia **perfil** e clique em **importar**.

![Botão de importação do assistente publicar Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

Na caixa de diálogo **Importar configurações de publicação** , selecione o arquivo *. publishsettings* que você baixou e clique em **abrir**. O assistente avança para a guia conexão com todos os campos preenchidos.

![Guia de conexão do assistente publicar Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

O arquivo. publishsettings coloca a URL permanente planejada para o site na caixa URL de destino, mas se você ainda não tiver comprado esse domínio, substitua o valor pela URL temporária. Para este exemplo, a URL é  *[http://contosouniversity.com.vserver01.cytanium.com](http://contosouniversity.com.vserver01.cytanium.com).* A única finalidade desta caixa é especificar qual URL o navegador abrirá automaticamente após a implantação. Se você deixá-lo em branco, a única consequência é que o navegador não será iniciado automaticamente após a implantação.

Clique em **validar conexão** para verificar se as configurações estão corretas e se você pode se conectar ao servidor. Como vimos anteriormente, uma marca de seleção verde verifica se a conexão foi bem-sucedida.

Ao clicar em validar conexão, você poderá ver uma caixa de diálogo de **erro de certificado** . Se você fizer isso, verifique se o nome do servidor é o esperado. Se for, selecione **salvar este certificado para futuras sessões do Visual Studio** e clique em **aceitar**. (Esse erro significa que o provedor de hospedagem optou por evitar a despesa de comprar um certificado SSL para a URL na qual você está implantando. Se preferir estabelecer uma conexão segura usando um certificado válido, entre em contato com seu provedor de hospedagem.)

![Erro de certificado](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Clique em **Próximo**.

Na seção **bancos de dados** da guia **configurações** , insira os mesmos valores que você inseriu para o perfil de publicação de teste. Você encontrará as cadeias de conexão necessárias nas listas suspensas.

- Na caixa Cadeia de conexão para **SchoolContext,** selecione `Data Source=|DataDirectory|School-Prod.sdf`
- Em **SchoolContext**, selecione **aplicar migrações do Code First**.
- Na caixa Cadeia de conexão para **DefaultConnection**, selecione `Data Source=|DataDirectory|aspnet-Prod.sdf`
- Em **DefaultConnection**, deixe **Atualizar banco de dados** limpo.

![Guia de configurações do assistente publicar Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Clique em **Próximo**.

Na guia **Visualização** , clique em **Iniciar visualização** para ver uma lista dos arquivos que serão copiados. Você verá a mesma lista que viu anteriormente ao implantar no IIS no computador local.

Antes de publicar, altere o nome do perfil para que o arquivo de transformação Web. Production. config seja aplicado. Selecione a guia **perfil** e clique em **gerenciar perfis**.

![Assistente de publicação na Web gerenciar perfis](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

Na caixa de diálogo **Editar perfis de publicação na Web** , selecione o perfil de produção, clique em **renomear**e altere o nome do perfil para produção. Em seguida, clique em **Fechar**.

![Caixa de diálogo Editar perfis de publicação na Web](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Clique em **Publicar**.

O aplicativo é publicado no provedor de hospedagem. O resultado é mostrado na janela **saída** .

![Janela de saída após a implantação](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

O navegador é aberto automaticamente na caixa URL inserida na **URL de destino** na guia **conexão** do assistente **publicar Web** . Você vê o mesmo home page como quando executa o site no Visual Studio, exceto que agora não há nenhum indicador de ambiente "(teste)" ou "(dev)" na barra de título. Isso indica que a transformação *Web. config* do indicador de ambiente funcionou corretamente.

> [!NOTE]
> Se você ainda vir "(teste)" no cabeçalho, exclua a pasta *obj* do projeto ContosoUniversity e reimplante. Nas versões de pré-lançamento do software, o arquivo de transformação aplicado anteriormente (Web. Test. config) pode ser aplicado novamente, embora você esteja usando o perfil de produção.

[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Antes de executar uma página que cause acesso ao banco de dados, verifique se o ELMAH poderá registrar os erros que ocorrerem.

## <a name="setting-folder-permissions-for-elmah"></a>Definindo permissões de pasta para o ELMAH

Como você se lembra do tutorial anterior nesta série, você deve certificar-se de que o aplicativo tenha permissões de gravação para a pasta em seu aplicativo em que o ELMAH armazena arquivos de log de erros. Quando você implantou o no IIS localmente em seu computador, você define essas permissões manualmente. Nesta seção, você verá como definir permissões em Cytanium. (Alguns provedores de hospedagem podem não permitir que você faça isso; eles podem oferecer uma ou mais pastas predefinidas com permissões de gravação. Nesse caso, você precisaria modificar seu aplicativo para usar as pastas especificadas.)

Você pode definir permissões de pasta no painel de controle Cytanium. Vá para a URL do painel de controle e selecione **Gerenciador de arquivos**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

Na caixa **Gerenciador de arquivos** , selecione **contosouniversity.com** e **wwwroot** para ver a pasta raiz do aplicativo. Clique no ícone de cadeado ao lado de **ELMAH**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

Na janela permissões da **pasta**/de **arquivo** , marque as caixas de seleção **ler** e **gravar** para **contosouniversity.com** e clique em **definir permissões**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Verifique se o ELMAH tem acesso de gravação à pasta do *ELMAH* , causando um erro e exibindo o relatório de erros do ELMAH. Solicite uma URL inválida como *Studentsxxx. aspx*. Como antes, você vê a página *GenericErrorPage. aspx* . Clique no link **fazer logoff** e, em seguida, execute o *ELMAH. axd*. Primeiro, você obtém a página de **logon** , que valida que a transformação *Web. config* adicionou com êxito a autorização do ELMAH. Depois de fazer logon, você verá o relatório que mostra o erro que acabou de ser causado.

[![ELMAH. axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Teste no ambiente de produção

Execute a página **estudantes** . O aplicativo tenta acessar o banco de dados escolar pela primeira vez, o que dispara Migrações do Code First para criar o banco de dados. Quando a página é exibida após o atraso de um instante, ela mostra que não há alunos.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Execute a página **instrutores** para verificar se os dados de semente inseriram com êxito os dados do instrutor no banco de dado.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Como você fez no ambiente de teste, você deseja verificar se as atualizações do banco de dados funcionam no ambiente de produção, mas normalmente não deseja inserir dados de teste no banco de dado de produção. Para este tutorial, você usará o mesmo método que fez no teste. Mas, em um aplicativo real, você pode querer encontrar um método que valida que as atualizações de banco de dados são bem-sucedidas sem introduzir dados de teste no banco de dado de produção. Em alguns aplicativos, pode ser prático adicionar algo e, em seguida, excluí-lo.

Adicione um aluno e, em seguida, exiba os dados inseridos na página **alunos** para verificar se você pode atualizar os dados no banco de dado.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Valide se as regras de autorização estão funcionando corretamente selecionando **Atualizar créditos** no menu **cursos** . A página de **logon** é exibida. Insira suas credenciais de conta de administrador, clique em **fazer logon**e a página **Atualizar créditos** é exibida.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Se o logon for bem-sucedido, a página **Atualizar créditos** será exibida. Isso indica que o banco de dados de associação do ASP.NET (com a conta de administrador único) foi implantado com êxito.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Agora você implantou e testou com êxito seu site e está disponível publicamente pela Internet.

## <a name="creating-a-more-reliable-test-environment"></a>Criando um ambiente de teste mais confiável

Conforme explicado no tutorial [implantando no ambiente de teste](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) , o ambiente de teste mais confiável seria uma segunda conta no provedor de hospedagem, assim como a conta de produção. Isso seria mais caro do que usar o IIS local como seu ambiente de teste, pois você precisaria se inscrever para uma segunda conta de hospedagem. Mas se ele impedir erros ou interrupções no site de produção, você pode decidir que vale o custo.

A maior parte do processo de criação e implantação em uma conta de teste é semelhante ao que você já fez para implantar na produção:

- Crie um arquivo de transformação *Web. config* .
- Crie uma conta no provedor de hospedagem.
- Crie um novo perfil de publicação e implante-o na conta de teste.

### <a name="preventing-public-access-to-the-test-site"></a>Impedindo o acesso público ao site de teste

Uma consideração importante para a conta de teste é que ela estará ativa na Internet, mas você não quer que o público use-a. Para manter o site privado, você pode usar um ou mais dos seguintes métodos:

- Entre em contato com o provedor de hospedagem para definir as regras de firewall que permitem o acesso ao site de teste somente de endereços IP que você usa para teste.
- Disfarçar a URL para que ela não seja semelhante à URL do site público.
- Use um arquivo *robots. txt* para garantir que os mecanismos de pesquisa não rastreiem o site de teste e os links de relatório para ele nos resultados da pesquisa.

O primeiro desses métodos é, obviamente, o mais seguro, mas o procedimento para isso é específico para cada provedor de hospedagem e não será abordado neste tutorial. Se você fizer a organização com seu provedor de hospedagem para permitir que apenas seu endereço IP navegue até a URL da conta de teste, teoricamente, não precisará se preocupar com os mecanismos de pesquisa que o rastreiam. Mas mesmo nesse caso, a implantação de um arquivo *robots. txt* é uma boa ideia como um backup, caso a regra de firewall seja desativada acidentalmente.

O arquivo *robots. txt* é inserido na pasta do projeto e deve ter o seguinte texto:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

A linha de `User-agent` informa aos mecanismos de pesquisa que as regras no arquivo se aplicam a todos os robôs (rastreadores da Web) do mecanismo de pesquisa e a linha de `Disallow` especifica que nenhuma página no site deve ser rastreada.

Você provavelmente deseja que os mecanismos de pesquisa catalogassem seu site de produção, portanto, você precisa excluir esse arquivo da implantação de produção. Para fazer isso, consulte posso **Excluir arquivos ou pastas específicas da implantação?** em [ASP.net perguntas frequentes sobre implantação do projeto de aplicativo Web](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Certifique-se de especificar a exclusão somente para o perfil de publicação de produção.

A criação de uma segunda conta de hospedagem é uma abordagem para trabalhar com um ambiente de teste que não é necessário, mas pode valer a pena agregar a despesa. Nos tutoriais a seguir, você continuará a usar o IIS como seu ambiente de teste.

No próximo tutorial, você atualizará o código do aplicativo e implantará sua alteração nos ambientes de teste e produção.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
