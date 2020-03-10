---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Implantação da Web do ASP.NET usando o Visual Studio: implantação de linha de comando | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros, por usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 13cfe4492398b59f2c80394689cc113ccb218c60
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78630917"
---
# <a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>Implantação da Web do ASP.NET usando o Visual Studio: implantação de linha de comando

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros usando o Visual Studio 2012 ou o Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial da série](introduction.md).

## <a name="overview"></a>Visão geral

Este tutorial mostra como invocar o pipeline de publicação da Web do Visual Studio a partir da linha de comando. Isso é útil para cenários em que você deseja [automatizar o processo de implantação](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) em vez de fazê-lo manualmente no Visual Studio, normalmente usando um [sistema de controle de versão de código-fonte](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Fazer uma alteração para implantar

Atualmente, a página sobre exibe o código do modelo.

![Página sobre com código de modelo](command-line-deployment/_static/image1.png)

Você o substituirá pelo código que exibe um resumo do registro do aluno.

Abra a página *about. aspx* , exclua toda a marcação dentro do elemento `MainContent` `Content` e insira a seguinte marcação em seu lugar:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Execute o projeto e selecione a página **sobre** .

![Página Sobre](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Implantar para teste usando a linha de comando

Você não implantará outra alteração no banco de dados, portanto, desabilite a implantação do banco de dados dbDacFx para o banco de dados ASPNET-ContosoUniversity. Abra o assistente **publicar Web** e, em cada um dos três perfis de publicação, desmarque a caixa de seleção **Atualizar banco de dados** na guia **configurações** .

Na página inicial do Windows 8, pesquise por **prompt de comando do desenvolvedor para VS2012**.

Clique com o botão direito do mouse no ícone para **prompt de comando do desenvolvedor para VS2012** e clique em **Executar como administrador**.

Digite o seguinte comando no prompt de comando, substituindo o caminho para o arquivo de solução pelo caminho para o arquivo de solução:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

O MSBuild compila a solução e a implanta no ambiente de teste.

![Saída da linha de comando](command-line-deployment/_static/image3.png)

Abra um navegador e vá para `http://localhost/ContosoUniversity`e, em seguida, clique na página **sobre** para verificar se a implantação foi bem-sucedida.

Se você não tiver criado nenhum aluno em teste, verá uma página vazia no título de **estatísticas do corpo do aluno** . Vá para a página **alunos** , clique em **Adicionar aluno**e adicione alguns alunos e, em seguida, retorne à página **sobre** para ver as estatísticas dos alunos.

![Página sobre no ambiente de teste](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Opções de linha de comando de chave

O comando que você inseriu passou o caminho do arquivo da solução e duas propriedades para o MSBuild:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Implantando a solução em vez de implantar projetos individuais

A especificação do arquivo de solução faz com que todos os projetos na solução sejam compilados. Se você tiver vários projetos Web na solução, o seguinte comportamento do MSBuild se aplicará:

- As propriedades que você especificar na linha de comando são passadas para cada projeto. Portanto, cada projeto Web deve ter um perfil de publicação com o nome que você especificar. Se você especificar `/p:PublishProfile=Test`, cada projeto da Web deverá ter um perfil de publicação chamado *teste*.
- Você pode publicar um projeto com êxito quando outro não mesmo é compilado. Para obter mais informações, consulte o StackOverflow thread [MSBuild falha com dois pacotes](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Se você especificar um projeto individual em vez de uma solução, precisará adicionar um parâmetro que especifica a versão do Visual Studio. Se você estiver usando o Visual Studio 2012, a linha de comando será semelhante ao exemplo a seguir:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

O número de versão do Visual Studio 2010 é 10,0. Para obter mais informações, consulte [compatibilidade de projetos do Visual Studio e VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) no blog do sayed Hashimi.

### <a name="specifying-the-publish-profile"></a>Especificando o perfil de publicação

Você pode especificar o perfil de publicação por nome ou pelo caminho completo para o arquivo *. pubxml* , conforme mostrado no exemplo a seguir:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Métodos de publicação na Web com suporte para publicação de linha de comando

Há suporte para três métodos de publicação para publicação de linha de comando:

- `MSDeploy`-publicar usando Implantação da Web.
- `Package`-publicar criando um pacote de Implantação da Web. Você precisa instalar o pacote separadamente do comando MSBuild que o cria.
- `FileSystem`-publicar copiando arquivos em uma pasta especificada.

### <a name="specifying-the-build-configuration-and-platform"></a>Especificando a plataforma e a configuração de compilação

A configuração e a plataforma de compilação devem ser definidas no Visual Studio ou na linha de comando. Os perfis de publicação incluem propriedades nomeadas `LastUsedBuildConfiguration` e `LastUsedPlatform`, mas você não pode definir essas propriedades para determinar como o projeto é compilado. Para obter mais informações, consulte [MSBuild: como definir a propriedade de configuração](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) no blog de sayed Hashimi.

## <a name="deploy-to-staging"></a>Implantar no preparo

Para implantar no Azure, você deve adicionar a senha à linha de comando. Se você salvou a senha no perfil de publicação no Visual Studio, ela foi armazenada em formato criptografado no arquivo *. pubxml. User* . Esse arquivo não é acessado pelo MSBuild quando você faz uma implantação de linha de comando, portanto, você precisa passar a senha em um parâmetro de linha de comando.

1. Copie a senha de que você precisa do arquivo *. publishsettings* que você baixou anteriormente para o site de preparo. A senha é o valor do atributo `userPWD` para o elemento Implantação da Web `publishProfile`.

    ![Implantação da Web senha](command-line-deployment/_static/image5.png)
2. Na página inicial do Windows 8, procure **prompt de comando do desenvolvedor para VS2012**e clique no ícone para abrir o prompt de comando. (Você não precisa abri-lo como administrador desta vez porque não está implantando no IIS no computador local.)
3. Digite o seguinte comando no prompt de comando, substituindo o caminho para o arquivo de solução pelo caminho para o arquivo de solução e a senha com sua senha:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Observe que essa linha de comando inclui um parâmetro extra: `/p:AllowUntrustedCertificate=true`. Como este tutorial está sendo gravado, a propriedade `AllowUntrustedCertificate` deve ser definida quando você publica no Azure por meio da linha de comando. Quando a correção para esse bug for liberada, você não precisará desse parâmetro.
4. Abra um navegador e acesse a URL do seu site de preparo e clique na página **sobre** para verificar se a implantação foi bem-sucedida.

    Como vimos anteriormente para o ambiente de teste, talvez seja necessário criar alguns alunos para ver as estatísticas na página **sobre** .

## <a name="deploy-to-production"></a>Implantar na produção

O processo de implantação em produção é semelhante ao processo de preparo.

1. Copie a senha de que você precisa do arquivo *. publishsettings* que você baixou anteriormente para o site de produção.
2. Abra **prompt de comando do desenvolvedor para VS2012**.
3. Digite o seguinte comando no prompt de comando, substituindo o caminho para o arquivo de solução pelo caminho para o arquivo de solução e a senha com sua senha:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Para um site de produção real, se também houvesse uma alteração no banco de dados, você normalmente copiaria o arquivo do *aplicativo\_offline. htm* para o site antes da implantação e excluí-lo após a implantação bem-sucedida.
4. Abra um navegador e acesse a URL do seu site de preparo e clique na página **sobre** para verificar se a implantação foi bem-sucedida.

## <a name="summary"></a>Resumo

Agora você implantou uma atualização de aplicativo usando a linha de comando.

![Página sobre no ambiente de teste](command-line-deployment/_static/image6.png)

No próximo tutorial, você verá um exemplo de como estender o pipeline de publicação na Web. O exemplo mostrará como implantar arquivos que não estão incluídos no projeto.

> [!div class="step-by-step"]
> [Anterior](deploying-a-database-update.md)
> [Próximo](deploying-extra-files.md)
