---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: Excluindo arquivos e pastas da implantação | Microsoft Docs
author: jrjlee
description: Este tópico descreve como você pode excluir arquivos e pastas de um pacote de implantação da Web ao compilar e empacotar um projeto de aplicativo Web.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: a262ce43d7199fb1015d54d0b7c213857c360946
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544971"
---
# <a name="excluding-files-and-folders-from-deployment"></a>Exclusão de arquivos e pastas da implantação

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como você pode excluir arquivos e pastas de um pacote de implantação da Web ao compilar e empacotar um projeto de aplicativo Web.

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no coração desses tutoriais baseia-se na abordagem de arquivo de projeto dividido descrita em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), no qual o processo de compilação é&#x2014;controlado por dois arquivos de projeto, um contendo instruções de Build que se aplicam a todos os ambientes de destino e um contendo configurações específicas de ambiente e de implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto independente do ambiente para formar um conjunto completo de instruções de compilação.

## <a name="overview"></a>Visão geral

Quando você cria um projeto de aplicativo Web no Visual Studio 2010, o WPP (Web Publishing Pipeline) permite que você estenda esse processo de compilação empacotando seu aplicativo Web compilado em um pacote da Web implantável. Você pode usar a Implantação da Web (ferramenta de implantação da Web) do Serviços de Informações da Internet (IIS) para implantar esse pacote da Web em um servidor Web IIS remoto ou importar o pacote da Web manualmente por meio do Gerenciador do IIS. Esse processo de empacotamento é explicado na [criação e no empacotamento de projetos de aplicativos Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

Então, como controlar o que é incluído em seu pacote da Web? As configurações do projeto no Visual Studio, por meio do arquivo de projeto subjacente, fornecem controle suficiente para muitos cenários. No entanto, em alguns casos, talvez você queira personalizar o conteúdo de seu pacote da Web para ambientes de destino específicos. Por exemplo, talvez você queira incluir uma pasta para arquivos de log ao implantar seu aplicativo em um ambiente de teste, mas excluir a pasta ao implantar o aplicativo em um ambiente de preparo ou de produção. Este tópico lhe mostrará como fazer isso.

## <a name="what-gets-included-by-default"></a>O que é incluído por padrão?

Quando você configura as propriedades do projeto de aplicativo Web no Visual Studio, a lista **itens a serem implantados** na página de **pacote/publicar Web** permite que você especifique o que deseja incluir em seu pacote de implantação da Web. Por padrão, isso é definido **somente para os arquivos necessários para executar este aplicativo**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Quando você escolhe **apenas os arquivos necessários para executar esse aplicativo**, o WPP tentará determinar quais arquivos devem ser adicionados ao pacote da Web. Isso inclui:

- Todas as saídas de compilação para o projeto.
- Todos os arquivos marcados com uma ação de compilação de **conteúdo**.

> [!NOTE]
> A lógica que determina quais arquivos incluir está contida neste arquivo:   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft. Web. Publishing. OnlyFilesToRunTheApp. targets*

## <a name="excluding-specific-files-and-folders"></a>Excluindo arquivos e pastas específicos

Em alguns casos, você desejará um controle mais refinado sobre quais arquivos e pastas são implantados. Se você souber quais arquivos deseja excluir antes do tempo e a exclusão se aplicar a todos os ambientes de destino, você poderá simplesmente definir a **ação de compilação** de cada arquivo como **nenhum**.

**Para excluir arquivos específicos da implantação**

1. Na janela **Gerenciador de soluções** , clique com o botão direito do mouse no arquivo e clique em **Propriedades**.
2. Na janela **Propriedades** , na linha **ação de compilação** , selecione **nenhum**.

No entanto, essa abordagem nem sempre é conveniente. Por exemplo, talvez você queira variar quais arquivos e pastas estão incluídos de acordo com seu ambiente de destino e de fora do Visual Studio. Por exemplo, na solução de exemplo do Contact Manager, dê uma olhada no conteúdo do projeto ContactManager. Mvc:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- A pasta interna contém alguns scripts SQL que o desenvolvedor usa para criar, remover e popular bancos de dados locais para fins de desenvolvimento. Nada nessa pasta deve ser implantado em um ambiente de preparo ou de produção.
- A pasta scripts contém vários arquivos JavaScript. Muitos desses arquivos são incluídos puramente para dar suporte à depuração ou fornecer IntelliSense no Visual Studio. Alguns desses arquivos não devem ser implantados em ambientes de preparo ou de produção. No entanto, talvez você queira implantá-los em um ambiente de teste do desenvolvedor para facilitar a solução de problemas.

Embora você possa manipular seus arquivos de projeto para excluir arquivos e pastas específicos, há uma maneira mais fácil. O WPP inclui um mecanismo para excluir arquivos e pastas criando listas de itens chamadas **ExcludeFromPackageFolders** e **ExcludeFromPackageFiles**. Você pode estender esse mecanismo adicionando seus próprios itens a essas listas. Para fazer isso, você precisa concluir estas etapas de alto nível:

1. Crie um arquivo de projeto personalizado chamado *[Project Name]. WPP. targets* na mesma pasta que o seu arquivo de projeto.

    > [!NOTE]
    > O arquivo *. WPP. targets* precisa ir para a mesma pasta do seu arquivo&#x2014;de projeto de aplicativo Web, por exemplo, *ContactManager. Mvc. csproj*&#x2014;, em vez de na mesma pasta que os arquivos de projeto personalizados que você usa para controlar o processo de compilação e implantação.
2. No arquivo *. WPP. targets* , adicione um elemento do **rowgroup** .
3. No elemento **rowgroup** , adicione itens **ExcludeFromPackageFolders** e **ExcludeFromPackageFiles** para excluir arquivos e pastas específicos, conforme necessário.

Esta é a estrutura básica deste arquivo *. WPP. targets* :

[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]

Observe que cada item inclui um elemento de metadados de item chamado **FromTarget**. Esse é um valor opcional que não afeta o processo de compilação; Ele simplesmente serve para indicar por que arquivos ou pastas particulares foram omitidos se alguém examinar os logs de compilação.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Excluindo arquivos e pastas de um pacote da Web

O procedimento a seguir mostra como adicionar um arquivo *. WPP. targets* a um projeto de aplicativo Web e como usar o arquivo para excluir arquivos e pastas específicas do pacote da Web ao compilar seu projeto.

**Para excluir arquivos e pastas de um pacote de implantação da Web**

1. Abra sua solução no Visual Studio 2010.
2. Na janela **Gerenciador de soluções** , clique com o botão direito do mouse no nó do projeto de aplicativo Web (por exemplo, **ContactManager. Mvc**), aponte para **Adicionar**e clique em **novo item**.
3. Na caixa de diálogo **Adicionar novo item** , selecione o modelo de **arquivo XML** .
4. Na caixa **nome** , digite *[nome do projeto] * * *. WPP. targets** (por exemplo, **ContactManager. Mvc. WPP. targets**) e clique em **Adicionar**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Se você adicionar um novo item ao nó raiz de um projeto, o arquivo será criado na mesma pasta que o arquivo de projeto. Você pode verificar isso abrindo a pasta no Windows Explorer.
5. No arquivo, adicione um elemento **Project** e um elemento **rowgroup** :

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Se você quiser excluir pastas do pacote da Web, adicione um elemento **ExcludeFromPackageFolders** ao elemento **rowgroup** :

   1. No atributo **incluir** , forneça uma lista separada por ponto-e-vírgula das pastas que você deseja excluir.
   2. No elemento de metadados **FromTarget** , forneça um valor significativo para indicar por que as pastas estão sendo excluídas, como o nome do arquivo *. WPP. targets* .

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Se você quiser excluir arquivos do pacote da Web, adicione um elemento **ExcludeFromPackageFiles** ao elemento **rowgroup** :

   1. No atributo **incluir** , forneça uma lista separada por ponto-e-vírgula dos arquivos que você deseja excluir.
   2. No elemento de metadados **FromTarget** , forneça um valor significativo para indicar por que os arquivos estão sendo excluídos, como o nome do arquivo *. WPP. targets* .

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. O arquivo *[Project Name]. WPP. targets* agora deve ser semelhante ao seguinte:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Salve e feche o arquivo *[nome do projeto]. WPP. targets* .

Na próxima vez que você criar e empacotar seu projeto de aplicativo Web, o WPP detectará automaticamente o arquivo *. WPP. targets* . Todos os arquivos e pastas especificados não serão incluídos no pacote da Web.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como excluir arquivos e pastas específicas quando você cria um pacote da Web, criando um arquivo *. WPP. targets* personalizado na mesma pasta que o arquivo de projeto do aplicativo Web.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como usar arquivos de projeto do Microsoft Build Engine (MSBuild) personalizados para controlar o processo de implantação, consulte [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Para obter mais informações sobre o processo de empacotamento e implantação, consulte [criando e empacotando projetos de aplicativos Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [configurando parâmetros para implantação de pacote da Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)e [implantando pacotes da Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

> [!div class="step-by-step"]
> [Anterior](deploying-membership-databases-to-enterprise-environments.md)
> [Próximo](taking-web-applications-offline-with-web-deploy.md)
