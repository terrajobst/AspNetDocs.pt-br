---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio ou o Visual Web Developer: Implantando uma atualização somente de código-8 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: e4d094ef84a747c36ce05ddb0e3d1ce0391d5605
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564403"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio ou o Visual Web Developer: Implantando uma atualização somente de código-8 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Studio 2012 RC ou o Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial da série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar SQL Server edições diferentes de SQL Server Compact e mostra como implantar o Azure App aplicativos Web do serviço, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>Visão geral

Após a implantação inicial, seu trabalho de manutenção e desenvolvimento de seu site continua e, antes de longo, você desejará implantar uma atualização. Este tutorial orienta você pelo processo de implantação de uma atualização no código do aplicativo. Essa atualização não envolve uma alteração de banco de dados; Você verá o que há de diferente na implantação de uma alteração de banco de dados no próximo tutorial.

Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Alterando o código

Como um exemplo simples de uma atualização para seu aplicativo, você adicionará à página **instrutores** uma lista de cursos ensinados pelo instrutor selecionado.

Se você executar a página **instrutores** , observará que há links **selecionados** na grade, mas eles não fazem nada além de deixar o plano de fundo da linha ficar cinza.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Agora você adicionará o código que é executado quando o link **selecionar** é clicado e exibe uma lista de cursos ensinados pelo instrutor selecionado.

Em *instrutores. aspx*, adicione a seguinte marcação imediatamente após o controle de `Label` **ErrorMessageLabel** :

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Execute a página e selecione um instrutor. Você verá uma lista de cursos ensinados por esse instrutor.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Implantando a atualização de código no ambiente de teste

A implantação no ambiente de teste é uma simples questão de executar a publicação com um clique novamente. Para tornar esse processo mais rápido, você pode usar a barra de ferramentas de **publicação de um clique da Web** .

No menu **Exibir** , escolha **barras de ferramentas** e, em seguida, selecione **Web um clique em publicar**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

Em **Gerenciador de soluções**, selecione o projeto ContosoUniversity.

a barra de ferramentas de **publicação de um clique da Web** , escolha o perfil de publicação de **teste** e clique em **publicar Web** (o ícone com setas apontando para a esquerda e para a direita).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

O Visual Studio implanta o aplicativo atualizado e o navegador é aberto automaticamente na home page. Execute a página instrutores e selecione um instrutor para verificar se a atualização foi implantada com êxito.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Normalmente, você também faria testes de regressão (ou seja, teste o restante do site para garantir que a nova alteração não interrompa nenhuma funcionalidade existente). Mas, para este tutorial, você ignorará essa etapa e continuará a implantar a atualização para produção.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Impedindo a reimplantação do estado inicial do banco de dados para produção

Em um aplicativo real, os usuários interagem com seu site de produção após a implantação inicial e os bancos de dados são populados com Live Data. Portanto, você não deseja reimplantar o banco de dados de associação em seu estado inicial, o que apagaria todos os Live Data. Como os bancos de dados do SQL Server Compact são arquivos na pasta *\_data do aplicativo* , você precisa evitar isso alterando as configurações de implantação para que os arquivos na pasta *\_dados do aplicativo* não sejam implantados.

Abra a janela de **Propriedades do projeto** para o projeto ContosoUniversity e selecione a guia **pacote/publicar Web** . Verifique se a caixa suspensa **configuração** está **ativa (versão)** ou **versão** selecionada, selecione **Excluir arquivos da pasta de dados do\_de aplicativos**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

Caso você decida implantar uma compilação de depuração no futuro, é uma boa ideia fazer a mesma alteração para a configuração da compilação de depuração: alterar a **configuração** para **depurar** e, em seguida, selecionar **Excluir arquivos na pasta de dados do aplicativo\_** .

Salve e feche a guia **empacotar/publicar Web** .

> [!NOTE] 
> 
> [!IMPORTANT]
> Verifique se você não **removeu arquivos adicionais no destino** selecionado em seus perfis de publicação. Se você selecionar essa opção, o processo de implantação excluirá os bancos de dados que você tem no app\_data no site implantado e excluirá o aplicativo\_pasta de dados em si.

## <a name="preventing-user-access-to-the-production-site-during-update"></a>Impedindo o acesso do usuário ao site de produção durante a atualização

A alteração que você está implantando agora é uma simples alteração em uma única página. Mas, às vezes, você implanta alterações maiores e, nesse caso, o site pode se comportar de forma invariada se um usuário solicitar uma página antes da conclusão da implantação. Para evitar isso, você pode usar um *aplicativo\_arquivo. htm offline* . Quando você coloca um arquivo chamado *app\_offline. htm* na pasta raiz do seu aplicativo, o IIS exibe automaticamente esse arquivo em vez de executar o aplicativo. Portanto, para evitar o acesso durante a implantação, coloque o *aplicativo\_offline. htm* na pasta raiz, execute o processo de implantação e remova o *aplicativo\_offline. htm*.

Em **Gerenciador de soluções**, clique com o botão direito do mouse na solução (não um dos projetos) e selecione **nova pasta da solução**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Nomeie a pasta *SolutionFiles*.

Na nova pasta, crie uma página HTML chamada *app\_offline. htm*. Substitua o conteúdo existente pela seguinte marcação:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Você pode copiar o arquivo *. htm do aplicativo\_offline* para o site usando uma conexão FTP ou o utilitário **Gerenciador de arquivos** no painel de controle do provedor de hospedagem. Para este tutorial, você usará o **Gerenciador de arquivos**.

Abra o painel de controle e selecione **Gerenciador de arquivos** como fez no tutorial [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) . Selecione **contosouniversity.com** e **wwwroot** para obter a pasta raiz do aplicativo e clique em **carregar**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

Na caixa de diálogo **carregar arquivo** , selecione o arquivo *app\_offline. htm* e clique em **carregar**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Navegue até a URL do site. Você verá que a página do *aplicativo\_offline. htm* agora é exibida em vez de seu Home Page.

[![App_offline. htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Agora você está pronto para implantar na produção.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Implantando a atualização de código no ambiente de produção

Na barra de ferramentas de **publicação de um clique da Web** , escolha o perfil de publicação de **produção** e clique em **publicar Web**.

O Visual Studio implanta o aplicativo atualizado e abre o navegador para o home page do site. O arquivo *offline. htm do aplicativo\_* é exibido. Antes de testar para verificar a implantação bem-sucedida, você deve remover o arquivo *\_offline. htm do aplicativo* .

Retorne ao aplicativo **Gerenciador de arquivos** no painel de controle. Selecione **contosouniversity.com** e **wwwroot**, selecione **aplicativo\_offline. htm**e clique em **excluir**.

[![Deleting_app_offline. htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

No navegador, abra a página instrutores no site público e selecione um instrutor para verificar se a atualização foi implantada com êxito.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Agora você implantou uma atualização de aplicativo que não envolveu uma alteração no banco de dados. O próximo tutorial mostra como implantar uma alteração de banco de dados.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
