---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Instalando pacotes da Web manualmente | Microsoft Docs
author: jrjlee
description: Este tópico descreve como importar manualmente um pacote de implantação da Web no Serviços de Informações da Internet (IIS). O tópico Criando e empacotando aplicativo da Web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: f778549d3e26989a2e71ef21171adec521842729
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634200"
---
# <a name="manually-installing-web-packages"></a>Instalação manual de pacotes da Web

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como importar manualmente um pacote de implantação da Web no Serviços de Informações da Internet (IIS).
> 
> O tópico [criando e empacotando projetos de aplicativos Web](building-and-packaging-web-application-projects.md) descreveu como a ferramenta de implantação da Web do IIS (implantação da Web), em conjunto com o Microsoft Build Engine (MSBuild) e o WPP (pipeline de publicação na Web), permite empacotar seus projetos de aplicativos Web em um único arquivo zip. Esse arquivo, normalmente conhecido como pacote de implantação da Web (ou simplesmente um pacote de implantação), contém todas as informações de conteúdo e configuração que o IIS precisa para recriar seu aplicativo Web em um servidor Web.
> 
> Depois de criar um pacote de implantação da Web, você pode publicá-lo em um servidor IIS de várias maneiras. Em muitos cenários, você desejará aproveitar os pontos de integração entre o MSBuild, o WPP e o Implantação da Web para criar e instalar pacotes da Web remotamente como parte de um processo de compilação e implantação automatizado ou de uma única etapa. Esse processo é descrito em [implantando pacotes da Web](deploying-web-packages.md). No entanto, isso nem sempre é possível. Suponha que você deseja implantar um aplicativo Web em um ambiente de produção voltado para a Internet. Por motivos de segurança, um ambiente de produção é, na pior das hipóteses, um firewall em uma sub-rede separada do servidor de compilação, em uma rede de perímetro (também conhecida como DMZ, zona desmilitarizada e sub-rede filtrada). Em muitos casos, o ambiente de produção estará em um domínio separado ou em uma rede isolada fisicamente.
> 
> Nesses cenários, sua única opção pode ser portar o pacote da Web para o servidor de destino e importá-lo manualmente para o IIS. Embora essa abordagem impedida a implantação automatizada, ela ainda é uma técnica altamente eficaz para publicar&#x2014;um aplicativo Web, basta copiar um único arquivo zip para o servidor Web e usar um assistente para orientá-lo no processo de importação.

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

## <a name="task-overview"></a>Visão geral da tarefa

Você precisará concluir estas tarefas de alto nível para importar um pacote de implantação da Web para o IIS:

- Crie um pacote de implantação da Web usando a linha de comando do MSBuild, o Team Build ou o Visual Studio 2010.
- Copie o pacote da Web para o servidor Web de destino.
- Use o assistente para importar pacote de aplicativos no Gerenciador do IIS para instalar o pacote da Web e fornecer valores para variáveis como cadeias de conexão e pontos de extremidade de serviço.

Este tópico mostrará como executar esses procedimentos. As tarefas e orientações neste tópico pressupõem que você já esteja familiarizado com os conceitos por trás de pacotes da Web, Implantação da Web e o WPP. Para obter mais informações, consulte [criando e empacotando projetos de aplicativos Web](building-and-packaging-web-application-projects.md).

> [!NOTE]
> Este tópico é melhor usado em conjunto com [a configuração de um servidor Web para publicação implantação da Web (implantação offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), que explica como instalar os componentes necessários e preparar um site do IIS para a importação de pacote.

## <a name="create-a-web-deployment-package"></a>Criar um pacote de implantação da Web

A primeira tarefa é criar um pacote de implantação da Web para o projeto de aplicativo Web que você deseja implantar. Você pode criar pacotes da Web de várias maneiras.

**Abordagem 1: criar um pacote como parte do processo de compilação com o Visual Studio**

Você pode configurar seu projeto de aplicativo Web para criar um pacote de implantação da Web após cada compilação por meio da guia **empacotar/publicar Web** nas páginas de propriedades do projeto. Esse processo é descrito na [criação e no empacotamento de projetos de aplicativos Web](building-and-packaging-web-application-projects.md).

**Abordagem 2: criar um pacote como parte do processo de compilação com o MSBuild**

Se você criar seu projeto de aplicativo Web usando o MSBuild diretamente, seja por meio de um arquivo de projeto do MSBuild personalizado ou da linha de comando, poderá criar um pacote de implantação da Web como parte do processo de compilação, incluindo as propriedades de pacote **DeployOnBuild = true** e **DeployTarget =** no comando. Esse processo é descrito em [noções básicas sobre o processo de compilação](understanding-the-build-process.md).

**Abordagem 3: criar um pacote sob demanda no Visual Studio**

Você pode criar um pacote de implantação da Web para um projeto de aplicativo Web a qualquer momento no Visual Studio 2010. Para fazer isso, na janela **Gerenciador de soluções** , clique com o botão direito do mouse no projeto de aplicativo Web e clique em **Compilar pacote de implantação**.

![](manually-installing-web-packages/_static/image1.png)

**Abordagem 4: criar um pacote sob demanda a partir da linha de comando**

Você pode criar um pacote de implantação da Web a partir da linha de comando invocando o destino do **pacote** em seu projeto de aplicativo Web usando o MSBuild. O comando deve ser semelhante a este:

[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]

Seja qual for a abordagem usada, o resultado final será o mesmo. O WPP cria um pacote de implantação da Web como um arquivo zip, junto com vários recursos de suporte, na pasta de saída para seu projeto de aplicativo Web.

![](manually-installing-web-packages/_static/image2.png)

Quando estiver planejando importar o pacote da Web manualmente, você só precisará do arquivo zip. Copie esse arquivo para o servidor Web de destino e você poderá iniciar o processo de importação.

## <a name="import-a-web-package-into-iis"></a>Importar um pacote da Web para o IIS

Você pode usar o procedimento a seguir para importar um pacote de implantação da Web do sistema de arquivos local para um site do IIS. Antes de executar este procedimento, verifique se você tem:

- O pacote de implantação da Web foi copiado para o servidor Web.
- Configurado um servidor Web do IIS para hospedar seu aplicativo.

Para obter mais informações sobre como configurar um servidor Web do IIS para oferecer suporte a pacotes de implantação da Web, consulte [configurar um servidor Web para publicação de implantação da Web (implantação offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**Para importar um pacote de implantação da Web usando o Gerenciador do IIS**

1. No Gerenciador do IIS, no painel **conexões** , clique com o botão direito do mouse no site do IIS, aponte para **implantar**e clique em **importar aplicativo**.

    ![](manually-installing-web-packages/_static/image3.png)
2. No Assistente para importar pacote de aplicativos, na página **selecionar o pacote** , navegue até o local do pacote de implantação da Web e clique em **Avançar**.
3. Na página **selecionar o conteúdo do pacote** , desmarque qualquer conteúdo que você não precisa e clique em **Avançar**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > Em muitos casos, talvez você não queira importar tudo o que vem com um pacote de implantação da Web. Por exemplo, talvez você não queira permitir que Implantação da Web substitua o banco de dados associado.  
    > As entradas **Grant Permissions** definem permissões no sistema de arquivos de destino para garantir que a identidade do pool de aplicativos possa acessar a pasta física que armazena o conteúdo do site. Além disso, o usuário de autenticação anônima recebe permissão de leitura para a pasta para permitir que o aplicativo sirva arquivos de tipo MIME (Multipurpose Internet Mail Extensions). Se preferir, você pode remover essas entradas e configurar as permissões manualmente.
4. Na página **inserir informações do pacote de aplicativo** , forneça as informações solicitadas.

    ![](manually-installing-web-packages/_static/image5.png)
5. Quando você cria um pacote da Web, o WPP analisa o arquivo de configuração para seu aplicativo e detecta quaisquer variáveis, como cadeias de conexão e pontos de extremidade de serviço. Nesse caso:

    1. **Caminho do aplicativo** é o caminho do IIS no qual você deseja instalar o aplicativo. Essa configuração é comum a todos os pacotes de implantação criados pelo WPP.
    2. O **endereço do ponto de extremidade do serviço ContactService** é o endereço que o aplicativo deve usar para se comunicar com o serviço WCF implantado. Essa configuração corresponde a uma entrada no arquivo *Web. config* .
    3. A primeira configuração de **cadeia de conexão** é a cadeia de conexão que implantação da Web deve usar para implantar o banco de dados associado ao aplicativo (neste caso, um banco de dados de associação do ASP.net). Essa configuração corresponde à configuração na guia **empacotar/publicar SQL** no Visual Studio.
    4. A segunda configuração de **cadeia de conexão** é a cadeia de conexão que seu aplicativo realmente usará para se comunicar com o banco de dados quando estiver em execução. Isso corresponde a uma entrada de cadeia de conexão no arquivo *Web. config* .

        > [!NOTE]
        > Para obter mais informações sobre a origem desses parâmetros, consulte [configurando parâmetros para implantação de pacote da Web](configuring-parameters-for-web-package-deployment.md).
6. Clique em **Próximo**.
7. Se essa não for a primeira vez que você implantou o aplicativo nesse site, será solicitado que você especifique se deseja excluir todo o conteúdo existente antes da instalação. Escolha a opção apropriada para seus requisitos e clique em **Avançar**.

    ![](manually-installing-web-packages/_static/image6.png)
8. Quando o IIS terminar de instalar o pacote, clique em **concluir**.

    ![](manually-installing-web-packages/_static/image7.png)

Neste ponto, você publicou com êxito seu aplicativo Web para o IIS.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como importar um pacote de implantação da Web para um site do IIS usando o Gerenciador do IIS. Essa abordagem à publicação de aplicativos Web é apropriada quando as restrições de segurança ou de infraestrutura tornam a implantação remota impossível ou indesejável.

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre como configurar um servidor Web do IIS para dar suporte à importação manual de um pacote da Web, consulte [configurar um servidor Web para publicação de implantação da Web (implantação offline)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Para obter uma orientação mais geral sobre a implantação de pacotes da Web, consulte [Walkthrough: Implantando um projeto de aplicativo Web usando um pacote de implantação da Web (parte 1 de 4)](https://msdn.microsoft.com/library/dd483479.aspx).

> [!div class="step-by-step"]
> [Anterior](creating-and-running-a-deployment-command-file.md)
