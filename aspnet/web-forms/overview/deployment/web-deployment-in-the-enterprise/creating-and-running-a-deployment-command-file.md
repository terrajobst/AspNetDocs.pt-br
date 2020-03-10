---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Criando e executando um arquivo de comando de implantação | Microsoft Docs
author: jrjlee
description: Este tópico descreve como criar um arquivo de comando que permitirá executar uma implantação usando arquivos de projeto Microsoft Build Engine (MSBuild) como uma etapa única, re...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: f1477ff423e4898385066a35b42503f3c70dcc68
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634305"
---
# <a name="creating-and-running-a-deployment-command-file"></a>Criação e execução de um arquivo de comando de implantação

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como criar um arquivo de comando que permitirá que você execute uma implantação usando arquivos de projeto Microsoft Build Engine (MSBuild) como um processo de etapa única e repetível.

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da solução&#x2014; [Contact Manager](the-contact-manager-solution.md) para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no coração desses tutoriais é baseado na abordagem de arquivo de projeto dividido descrita na [compreensão do processo de compilação](understanding-the-build-process.md), no qual o processo de compilação é controlado por dois&#x2014;arquivos de projeto, um contendo instruções de Build que se aplicam a todos os ambientes de destino e um contendo configurações específicas de ambiente e de implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto independente do ambiente para formar um conjunto completo de instruções de compilação.

## <a name="process-overview"></a>Visão geral do processo

Neste tópico, você aprenderá a criar e executar um arquivo de comando que usa esses arquivos de projeto para executar uma implantação repetível em seu ambiente de destino. Essencialmente, o arquivo de comando simplesmente precisa conter um comando do MSBuild que:

- Informa ao MSBuild para executar o arquivo *Publish. proj* independente do ambiente.
- Informa ao arquivo *Publish. proj* qual arquivo contém as configurações de projeto específicas do ambiente e onde encontrá-lo.

## <a name="create-an-msbuild-command"></a>Criar um comando do MSBuild

Conforme descrito em [noções básicas sobre o processo de compilação](understanding-the-build-process.md), o arquivo&#x2014;de projeto específico do ambiente, por exemplo, *env-dev. proj*&#x2014;, foi projetado para ser importado para o arquivo *Publish. proj* independente do ambiente no momento da compilação. Juntos, esses dois arquivos fornecem um conjunto completo de instruções que dizem ao MSBuild como criar e implantar sua solução.

O arquivo *Publish. proj* usa um elemento de **importação** para importar o arquivo de projeto específico do ambiente.

[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]

Dessa forma, ao usar o MSBuild. exe para compilar e implantar a solução Contact Manager, você precisa:

- Execute MSBuild. exe no arquivo *Publish. proj* .
- Especifique o local do arquivo de projeto específico do ambiente fornecendo um parâmetro de linha de comando chamado **TargetEnvPropsFile**.

Para fazer isso, o comando do MSBuild deve ser semelhante ao seguinte:

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]

A partir daqui, é uma etapa simples passar para uma implantação repetível e de uma única etapa. Tudo o que você precisa fazer é adicionar o comando do MSBuild a um arquivo. cmd. Na solução Contact Manager, a pasta Publish inclui um arquivo chamado *Publish-dev. cmd* que faz exatamente isso.

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]

> [!NOTE]
> A opção **/fl** instrui o MSBuild a criar um arquivo de log chamado *MSBuild. log* no diretório de trabalho no qual o MSBuild. exe foi invocado.

Para implantar ou reimplantar a solução de Gerenciador de contatos, tudo o que você precisa fazer é executar o arquivo *Publish-dev. cmd* . Quando você executar o arquivo, o MSBuild irá:

- Compilar todos os projetos na solução.
- Gere pacotes da Web implantáveis para os projetos de aplicativo Web.
- Gere arquivos. dbschema e. DeployManifest para os projetos de banco de dados.
- Implante os pacotes da Web no servidor Web.
- Implante o banco de dados no servidor de banco de dados.

## <a name="run-the-deployment"></a>Executar a implantação

Quando você criou um arquivo de comando para o seu ambiente de destino, você deve ser capaz de concluir toda a implantação simplesmente executando o arquivo.

**Para implantar a solução do Gerenciador de contatos em seu ambiente de teste**

1. Na estação de trabalho do desenvolvedor, abra o Windows Explorer e, em seguida, navegue até o local do arquivo *Publish-dev. cmd* .
2. Clique duas vezes no arquivo para executá-lo.
3. Se uma caixa de diálogo **arquivo aberto – aviso de segurança** for exibida, clique em **executar**.
4. Se as definições de configuração e os servidores de teste estiverem configurados corretamente, a janela de prompt de comando mostrará uma mensagem de **compilação bem-sucedida** quando o MSBuild tiver terminado de processar os arquivos de projeto.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Se esta for a primeira vez que você implantou a solução para esse ambiente, você precisará adicionar a conta de computador do servidor Web de teste ao banco de dados **\_DataWriter** e ao **BD\_** as funções do DataReader em **ContactManager** . Esse procedimento é descrito em [configurar um servidor de banco de dados para implantação da Web publicação](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Você só precisa atribuir essas permissões ao criar o banco de dados. Por padrão, o processo de compilação não recriará o banco de dados&#x2014;em cada implantação em vez disso, ele comparará o banco de dados existente com o esquema mais recente e fará apenas as alterações necessárias. Como resultado, você só precisará mapear essas funções de banco de dados na primeira vez que implantar a solução.
6. Abra o Internet Explorer e navegue até a URL do aplicativo Contact Manager (por exemplo, `http://testweb1:85/ContactManager/`).
7. Verifique se o aplicativo funciona conforme o esperado e se você pode adicionar contatos.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Conclusão

A criação de um arquivo de comando que contém as instruções do MSBuild fornece uma maneira rápida e fácil de criar e implantar uma solução de vários projetos em um ambiente de destino específico. Se você precisar implantar repetidamente sua solução em vários ambientes de destino, poderá criar vários arquivos de comando. Em cada arquivo de comando, o comando MSBuild criará o mesmo arquivo de projeto universal, mas ele especificará um arquivo de projeto específico do ambiente diferente. Por exemplo, um arquivo de comando para publicar em um ambiente de desenvolvedor ou de teste pode conter este comando do MSBuild:

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]

Um arquivo de comando para publicar em um ambiente de preparo pode conter este comando do MSBuild:

[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]

> [!NOTE]
> Para obter orientação sobre como personalizar os arquivos de projeto específicos do ambiente para seus próprios ambientes de servidor, consulte [Configurar Propriedades de implantação para um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Você também pode personalizar o processo de compilação para cada ambiente substituindo Propriedades ou configurando várias outras opções no comando do MSBuild. Para obter mais informações, consulte [referência de linha de comando do MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

> [!div class="step-by-step"]
> [Anterior](deploying-database-projects.md)
> [Próximo](manually-installing-web-packages.md)
