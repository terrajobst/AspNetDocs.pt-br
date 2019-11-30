---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Implantação da Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros, por usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 3881833bfe2a50a38a357614f92f434a04a8ab08
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74626774"
---
# <a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Implantação da Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros usando o Visual Studio 2012 ou o Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial da série](introduction.md).

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

Após a implantação inicial, seu trabalho de manutenção e desenvolvimento de seu site continua e, antes de longo, você desejará implantar uma atualização. Este tutorial orienta você pelo processo de implantação de uma atualização no código do aplicativo. A atualização implementada e implantada neste tutorial não envolve uma alteração no banco de dados; Você verá o que há de diferente na implantação de uma alteração de banco de dados no próximo tutorial.

Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](troubleshooting.md).

## <a name="make-a-code-change"></a>Fazer uma alteração de código

Como um exemplo simples de uma atualização para seu aplicativo, você adicionará à página **instrutores** uma lista de cursos ensinados pelo instrutor selecionado.

Se você executar a página **instrutores** , observará que há links **selecionados** na grade, mas eles não fazem nada além de deixar o plano de fundo da linha ficar cinza.

![Página de instrutores com seleção](deploying-a-code-update/_static/image1.png)

Agora você adicionará o código que é executado quando o link **selecionar** é clicado e exibe uma lista de cursos ensinados pelo instrutor selecionado.

1. Em *instrutores. aspx*, adicione a seguinte marcação imediatamente após o controle de `Label` **ErrorMessageLabel** :

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Execute a página e selecione um instrutor. Você verá uma lista de cursos ensinados por esse instrutor.

    ![Página de instrutores com cursos ensinados](deploying-a-code-update/_static/image2.png)
3. Feche o navegador.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Implantar a atualização de código no ambiente de teste

Antes de poder usar seus perfis de publicação para implantar para teste, preparo e produção, você precisa alterar as opções de publicação do banco de dados. Você não precisa mais executar os scripts de concessão e de implantação de dados para o banco de dado de associação.

1. Abra o assistente **publicar na Web** clicando com o botão direito do mouse no projeto ContosoUniversity e clicando em **publicar**.
2. Clique no perfil de **teste** na lista suspensa **perfil** .
3. Clique na guia **configurações** .
4. Em **DefaultConnection** na seção **databases** , desmarque a caixa de seleção **Atualizar banco de dados** .
5. Clique na guia **perfil** e, em seguida, clique no perfil de **preparo** na lista suspensa **perfil** .
6. Quando for perguntado se deseja salvar as alterações feitas no perfil de **teste** , clique em **Sim**.
7. Faça a mesma alteração no perfil de preparo.
8. Repita o processo para fazer a mesma alteração no perfil de produção.
9. Feche o assistente **publicar Web** .

A implantação no ambiente de teste agora é uma simples questão de executar a publicação com um clique novamente. Para tornar esse processo mais rápido, você pode usar a barra de ferramentas de **publicação de um clique da Web** .

1. No menu **Exibir** , escolha **barras de ferramentas** e, em seguida, selecione **Web um clique em publicar**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. Em **Gerenciador de soluções**, selecione o projeto ContosoUniversity.
3. a barra de ferramentas de **publicação de um clique da Web** , escolha o perfil de publicação de **teste** e clique em **publicar Web** (o ícone com setas apontando para a esquerda e para a direita).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. O Visual Studio implanta o aplicativo atualizado e o navegador é aberto automaticamente na home page.
5. Execute a página instrutores e selecione um instrutor para verificar se a atualização foi implantada com êxito.

Normalmente, você também faria testes de regressão (ou seja, teste o restante do site para garantir que a nova alteração não interrompa nenhuma funcionalidade existente). Mas, para este tutorial, você ignorará essa etapa e continuará a implantar a atualização para preparo e produção.

Quando você reimplanta, o Implantação da Web determina automaticamente quais arquivos foram alterados e copia apenas os arquivos alterados para o servidor. Por padrão, Implantação da Web usa as datas da última alteração nos arquivos para determinar quais foram alterados. Alguns sistemas de controle do código-fonte alteram datas do arquivo mesmo quando você não altera o conteúdo do arquivo. Nesse caso, talvez você queira configurar Implantação da Web para usar as somas de verificação de arquivo para determinar quais arquivos foram alterados. Para obter mais informações, consulte [por que todos os meus arquivos são reimplantados, embora eu não os tenha alterado?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) nas perguntas frequentes sobre a implantação do ASP.net.

## <a name="take-the-application-offline-during-deployment"></a>Colocar o aplicativo offline durante a implantação

A alteração que você está implantando agora é uma simples alteração em uma única página. Mas, às vezes, você implanta alterações maiores ou implanta alterações de código e de banco de dados, e o site poderá se comportar incorretamente se um usuário solicitar uma página antes da conclusão da implantação. Para impedir que os usuários acessem o site enquanto a implantação está em andamento, você pode usar um *aplicativo\_arquivo offline. htm* . Quando você coloca um arquivo chamado *app\_offline. htm* na pasta raiz do seu aplicativo, o IIS exibe automaticamente esse arquivo em vez de executar o aplicativo. Portanto, para evitar o acesso durante a implantação, coloque o *aplicativo\_offline. htm* na pasta raiz, execute o processo de implantação e remova o *aplicativo\_offline. htm* após a implantação bem-sucedida.

Você pode configurar Implantação da Web para colocar automaticamente um *aplicativo padrão\_arquivo offline. htm* no servidor quando ele iniciar a implantação e removê-lo quando for concluído. Para fazer isso, tudo o que você precisa fazer é adicionar o seguinte elemento XML ao arquivo de perfil de publicação (. pubxml):

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

Para este tutorial, você verá como criar e usar um aplicativo personalizado *\_arquivo offline. htm* .

O uso do *aplicativo\_offline. htm* no site de preparo não é necessário, pois você não tem usuários acessando o site de preparo. Mas é uma boa prática usar o preparo para testar tudo, como você planeja implantar na produção.

### <a name="create-app_offlinehtm"></a>Criar aplicativo\_offline. htm

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse na solução e clique em **Adicionar**e em **novo item**.
2. Crie uma **página HTML** denominada *app\_offline. htm* (exclua o "l" final na extensão *. html* que o Visual Studio cria por padrão).
3. Substitua a marcação do modelo pela seguinte marcação:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Salve e feche o arquivo.

### <a name="copy-app_offlinehtm-to-the-root-folder-of-the-web-site"></a>Copie o aplicativo\_offline. htm para a pasta raiz do site da Web

Você pode usar qualquer ferramenta de FTP para copiar arquivos para o site. O [FileZilla](http://filezilla-project.org/) é uma ferramenta de FTP popular e é mostrada nas capturas de tela.

Para usar uma ferramenta de FTP, você precisa de três coisas: a URL de FTP, o nome de usuário e a senha.

A URL é mostrada na página painel do site na Portal de Gerenciamento do Azure e o nome de usuário e a senha do FTP podem ser encontrados no arquivo *. publishsettings* que você baixou anteriormente. As etapas a seguir mostram como obter esses valores.

1. Na Portal de Gerenciamento do Azure, clique na guia **sites** e clique no site de preparo.
2. Na página **painel** , role para baixo até localizar o nome do host FTP na seção **visão rápida** .

    ![Nome do host do FTP](deploying-a-code-update/_static/image5.png)
3. Abra o arquivo *. publishsettings* de preparo no bloco de notas ou em outro editor de texto.
4. Localize o elemento `publishProfile` para o perfil de FTP.
5. Copie os valores de `userName` e `userPWD`.

    ![Credenciais de FTP](deploying-a-code-update/_static/image6.png)
6. Abra sua ferramenta FTP e faça logon na URL do FTP.
7. Copie o *aplicativo\_offline. htm* da pasta da solução para a pasta */site/wwwroot* no site de preparo.

    ![Copiar app_offline](deploying-a-code-update/_static/image7.png)
8. Navegue até a URL do site de preparo. Você verá que a página do *aplicativo\_offline. htm* agora é exibida em vez de seu Home Page.

    ![app_offline. htm na janela do navegador](deploying-a-code-update/_static/image8.png)

Agora você está pronto para implantar no preparo.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Implantar a atualização de código para preparo e produção

1. Na barra de ferramentas de **publicação de um clique da Web** , escolha o perfil de publicação de **preparo** e clique em **publicar Web**.

    O Visual Studio implanta o aplicativo atualizado e abre o navegador para o home page do site. O arquivo *offline. htm do aplicativo\_* é exibido. Antes de testar para verificar a implantação bem-sucedida, você deve remover o arquivo *\_offline. htm do aplicativo* .
2. Retorne à ferramenta de FTP e exclua o **aplicativo\_offline. htm** do site de preparo.
3. No navegador, abra a página instrutores no site de preparo e selecione um instrutor para verificar se a atualização foi implantada com êxito.
4. Siga o mesmo procedimento para produção, como fez para preparo.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Revisando alterações e implantando arquivos específicos

O Visual Studio 2012 também oferece a capacidade de implantar arquivos individuais. Para um arquivo selecionado, você pode exibir as diferenças entre a versão local e a versão implantada, implantar o arquivo no ambiente de destino ou copiar o arquivo do ambiente de destino para o projeto local. Nesta seção do tutorial, você verá como usar esses recursos.

### <a name="make-a-change-to-deploy"></a>Fazer uma alteração para implantar

1. Abra *Content/site. css*e localize o bloco para a marca de `body`.
2. Altere o valor de `background-color` de `#fff` para `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Exibir a alteração na janela de visualização de publicação

Ao usar o assistente **publicar Web** para publicar o projeto, você pode ver quais alterações serão publicadas clicando duas vezes no arquivo na janela de **Visualização** .

1. Clique com o botão direito do mouse no projeto ContosoUniversity e clique em **publicar**.
2. Na lista suspensa **perfil** , selecione o perfil de publicação de **teste** .
3. Clique em **Visualizar**e, em seguida, clique em **Iniciar visualização**.
4. No painel de **Visualização** , clique duas vezes em **site. css**.

    ![Clique duas vezes em site. css](deploying-a-code-update/_static/image9.png)

    A caixa de diálogo **Visualizar alterações** mostra uma visualização das alterações que serão implantadas.

    ![Visualizar alterações em site. css](deploying-a-code-update/_static/image10.png)

    Se você clicar duas vezes no arquivo *Web. config* , a caixa de diálogo **Visualizar alterações** mostrará o efeito de suas transformações de configuração de compilação e publicação das transformações de perfil. Neste ponto, você não fez nada que faria com que o arquivo *Web. config* no servidor fosse alterado, portanto, você espera não ver nenhuma alteração. No entanto, a janela **Visualizar alterações** incorretamente mostra duas alterações. Parece que dois elementos XML serão removidos. Esses elementos são adicionados pelo processo de publicação quando você seleciona **executar migrações do Code First no início do aplicativo** para uma classe de contexto de Code First. A comparação é feita antes que o processo de publicação adicione esses elementos, portanto, parece que eles estão sendo removidos, embora não sejam removidos. Esse erro será corrigido em uma versão futura.
5. Clique em **Fechar**.
6. Clique em **Publicar**.
7. Quando o navegador for aberto na home page do site de teste, pressione CTRL + F5 para fazer uma atualização impressa para ver o efeito da alteração do CSS.

    ![Efeito da alteração de CSS](deploying-a-code-update/_static/image11.png)
8. Feche o navegador.

### <a name="publish-specific-files-from-solution-explorer"></a>Publicar arquivos específicos de Gerenciador de Soluções

Suponha que você não goste do plano de fundo azul e queira reverter para a cor original. Nesta seção, você irá restaurar as configurações originais publicando um arquivo específico diretamente do **Gerenciador de soluções**.

1. Abra *Content/site. css* e restaure a configuração de `background-color` para `#fff`.
2. Em **Gerenciador de soluções**, clique com o botão direito do mouse no arquivo *Content/site. css* .

    O menu de contexto mostra três opções de publicação.

    ![Opções de publicação de Gerenciador de Soluções](deploying-a-code-update/_static/image12.png)
3. Clique em **Visualizar alterações em site. css**.

    Uma janela é aberta para mostrar as diferenças entre o arquivo local e a versão dele no ambiente de destino.

    ![Diff-Content/site. css](deploying-a-code-update/_static/image13.png)
4. Em **Gerenciador de soluções**, clique com o botão direito do mouse em **site. css** novamente e clique em **publicar site. css**.

    A janela **atividade de publicação na Web** mostra que o arquivo foi publicado.

    ![Janela de atividade de publicação na Web](deploying-a-code-update/_static/image14.png)
5. Abra um navegador para a URL de `http://localhost/contosouniversity` e pressione CTRL + F5 para fazer uma atualização rígida para ver o efeito da alteração de CSS.

    ![Home Page com CSS normal](deploying-a-code-update/_static/image15.png)
6. Feche o navegador.

## <a name="summary"></a>Resumo

Agora você viu várias maneiras de implantar uma atualização de aplicativo que não envolve uma alteração de banco de dados e viu como visualizar as alterações para verificar se o que será atualizado é o que você espera. A página de instrutores agora tem uma seção **cursos ensinados** .

![Página de instrutores com cursos ensinados](deploying-a-code-update/_static/image16.png)

O próximo tutorial mostra como implantar uma alteração no banco de dados: você adicionará um campo DataDeNascimento ao banco de dados e à página instrutores.

> [!div class="step-by-step"]
> [Anterior](deploying-to-production.md)
> [Próximo](deploying-a-database-update.md)
