---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Criando e empacotando projetos de aplicativos Web | Microsoft Docs
author: jrjlee
description: Quando você deseja implantar um projeto de aplicativo Web em um ambiente de servidor remoto, sua primeira tarefa é compilar o projeto e gerar um pacote de implantação da Web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: 1d0ee0264ce6461d7b0159f1a44de4de31e2d079
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78573916"
---
# <a name="building-and-packaging-web-application-projects"></a>Criação e a empacotamento de projetos de aplicativos Web

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Quando você deseja implantar um projeto de aplicativo Web em um ambiente de servidor remoto, sua primeira tarefa é criar o projeto e gerar um pacote de implantação da Web. Este tópico descreve como o processo de compilação funciona para projetos de aplicativos Web. Em particular, ele explica:
> 
> - Como o WPP (pipeline de publicação na Web) estende o processo de compilação para incluir a funcionalidade de implantação.
> - Como a Implantação da Web (ferramenta de implantação da Web) do Serviços de Informações da Internet (IIS) transforma seu aplicativo Web em um pacote de implantação.
> - Como o processo de criação e empacotamento funciona e quais arquivos são criados.

No Visual Studio 2010, o WPP oferece suporte ao processo de compilação e implantação para projetos de aplicativos Web. O WPP fornece um conjunto de destinos de Microsoft Build Engine (MSBuild) que estendem a funcionalidade do MSBuild e permitem que ele se integre ao Implantação da Web. No Visual Studio, você pode ver essa funcionalidade estendida nas páginas de propriedades do seu projeto de aplicativo Web. A página **da Web empacotar/publicar** , junto com a página **pacote/publicar SQL** , permite que você configure como seu projeto de aplicativo Web é empacotado para implantação quando o processo de compilação é concluído.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>Como funciona o WPP?

Se você examinar o arquivo de projeto para um projeto de C#aplicativo Web baseado em um, poderá ver que ele importa dois arquivos. targets.

[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]

A primeira instrução de **importação** é comum a todos C# os projetos visuais. Esse arquivo, *Microsoft. CSharp. targets*, contém destinos e tarefas que são específicos do C#Visual. Por exemplo, a C# tarefa do compilador (**CSC**) é invocada aqui. O arquivo *Microsoft. CSharp. targets* , por sua vez, importa o arquivo *Microsoft. Common. targets* . Isso define os destinos comuns a todos os projetos, como **Compilar**, **Recompilar**, **executar**, **Compilar**e **limpar**. A segunda instrução de **importação** é específica para projetos de aplicativos Web. O arquivo *Microsoft. WebApplication. targets* , por sua vez, importa o arquivo *Microsoft. Web. Publishing. targets* . O arquivo *Microsoft. Web. Publishing. targets* essencialmente *é* o WPP. Ele define destinos, como **Package** e **MSDeployPublish**, que invocam implantação da Web para concluir várias tarefas de implantação.

Para entender como esses destinos adicionais são usados, na solução de exemplo do Contact Manager, abra o arquivo *Publish. proj* e dê uma olhada no destino **BuildProjects** .

[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]

Esse destino usa a tarefa **MSBuild** para criar vários projetos. Observe as propriedades **DeployOnBuild** e **DeployTarget** :

- A propriedade **DeployOnBuild = true** essencialmente significa "quero executar um destino adicional quando a compilação for concluída com êxito".
- A propriedade **DeployTarget** identifica o nome do destino que você deseja executar quando a propriedade **DeployOnBuild** é igual a **true**. Nesse caso, você está especificando que deseja que o MSBuild execute o destino do **pacote** depois de compilar o projeto.

O destino do **pacote** é definido no arquivo *Microsoft. Web. Publishing. targets* . Essencialmente, esse destino usa a saída da compilação do seu projeto de aplicativo Web e o transforma em um pacote de implantação da Web que pode ser publicado em um servidor Web IIS.

> [!NOTE]
> Para exibir um arquivo de projeto (por exemplo, <em>ContactManager. Mvc. csproj</em>) no Visual Studio 2010, primeiro você precisa descarregar o projeto de sua solução. Na janela <strong>Gerenciador de soluções</strong> , clique com o botão direito do mouse no nó do projeto e clique em <strong>descarregar projeto</strong>. Clique com o botão direito do mouse no nó do projeto novamente e clique em <strong>Editar</strong><em>[arquivo do projeto]</em>). O arquivo de projeto será aberto em seu formato XML bruto. Lembre-se de recarregar o projeto quando terminar.  
> Para obter mais informações sobre destinos, tarefas e instruções de <strong>importação</strong> do MSBuild, consulte [noções básicas sobre o arquivo de projeto](understanding-the-project-file.md). Para obter uma introdução mais detalhada aos arquivos de projeto e ao WPP, consulte [dentro do Microsoft Build Engine: usando o MSBuild e o Team Foundation Build](http://amzn.com/0735645248) de Sayed Ibrahim Hashimi e William BARTHOLOMEW, ISBN: 978-0-7356-4524-0.

## <a name="what-is-a-web-deployment-package"></a>O que é um pacote de implantação da Web?

Quando você cria e implanta um projeto de aplicativo Web usando o Visual Studio 2010 ou usando o MSBuild diretamente, o resultado final é normalmente um *pacote de implantação da Web*. O pacote de implantação da Web é um arquivo. zip. Ele contém tudo o que o IIS e o Implantação da Web precisam para recriar seu aplicativo Web, incluindo:

- A saída compilada de seu aplicativo Web, incluindo conteúdo, arquivos de recursos, arquivos de configuração, JavaScript e recursos CSS (folhas de estilo em cascata) e assim por diante.
- Assemblies para seu projeto de aplicativo Web e para quaisquer projetos referenciados dentro de sua solução.
- Scripts SQL para gerar qualquer banco de dados que você esteja implantando com seu aplicativo Web.

Depois que o pacote de implantação da Web for gerado, você poderá publicá-lo em um servidor Web do IIS de várias maneiras. Por exemplo, você pode implantá-lo remotamente direcionando o Implantação da Web serviço de agente remoto ou o manipulador de Implantação da Web no servidor Web de destino, ou pode usar o Gerenciador do IIS para importar manualmente o pacote no servidor Web de destino. Para obter mais informações sobre essas abordagens para a implantação, consulte [escolhendo a abordagem certa para a implantação da Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>Como funciona o processo de compilação?

Isso mostra o que acontece quando você cria e empacota um projeto de aplicativo Web:

![](building-and-packaging-web-application-projects/_static/image2.png)

Quando você cria um projeto de aplicativo Web, o processo de compilação gera um arquivo chamado *[nome do projeto]. SourceManifest. xml*. Junto com o arquivo de projeto e a saída da compilação, isso *. O arquivo SourceManifest. xml* informa implantação da Web o que precisa incluir no pacote de implantação da Web. Usando essas entradas, Implantação da Web gera um pacote de implantação da Web chamado *[nome do projeto]. zip*.

Junto com o pacote de implantação da Web, o processo de compilação gera dois arquivos que podem ajudá-lo a usar o pacote:

- O arquivo *. Deploy. cmd* inclui um conjunto de comandos implantação da Web parametrizados (MSDeploy. exe) que publicam seu pacote de implantação da Web em um servidor Web IIS remoto. A execução do arquivo *. Deploy. cmd* , com os parâmetros apropriados, normalmente fornece uma alternativa mais rápida e fácil para construir manualmente os comandos MSDeploy. exe.
- O arquivo *Parameters. xml* fornece um conjunto de valores de parâmetro para o comando MSDeploy. exe. Esses valores incluem propriedades como o nome do aplicativo Web do IIS para o qual você deseja implantar o pacote, os valores de quaisquer pontos de extremidade de serviço e cadeias de conexão definidos no arquivo *Web. config* e quaisquer valores de propriedade de implantação definidos nas páginas de propriedades do projeto.

O arquivo *Parameters. xml* é fundamental para gerenciar o processo de implantação. Esse arquivo é gerado dinamicamente de acordo com o conteúdo do seu projeto de aplicativo Web. Por exemplo, se você adicionar uma cadeia de conexão ao seu arquivo *Web. config* , o processo de compilação detectará automaticamente a cadeia de conexão, parametrizará a implantação de forma adequada e criará uma entrada no arquivo Parameters *. xml* para permitir que você modifique a cadeia de conexão como parte do processo de implantação. O próximo tópico, [configurando parâmetros para implantação de pacote da Web](configuring-parameters-for-web-package-deployment.md), explica a função desse arquivo em mais detalhes e descreve as diferentes maneiras em que você pode modificá-lo durante a compilação e implantação.

> [!NOTE]
> No Visual Studio 2010, o WPP não oferece suporte à pré-compilação de páginas em um aplicativo Web antes do empacotamento. A próxima versão do Visual Studio e do WPP incluirá a capacidade de pré-compilar um aplicativo Web como uma opção de empacotamento.

## <a name="conclusion"></a>Conclusão

Este tópico forneceu uma visão geral do processo de criação e empacotamento para projetos de aplicativos Web no Visual Studio 2010. Ele descreveu como o WPP permite invocar Implantação da Web comandos do MSBuild e explicou como funciona o processo de criação e empacotamento.

Depois de criar um pacote de implantação da Web, a próxima etapa é implantá-lo. Para obter mais informações sobre isso, consulte [configurando parâmetros para implantação de pacote da Web](configuring-parameters-for-web-package-deployment.md) e [implantando pacotes da Web](deploying-web-packages.md).

## <a name="further-reading"></a>Leitura adicional

Os tópicos a seguir neste tutorial, [configurando parâmetros para implantação de pacote da Web](configuring-parameters-for-web-package-deployment.md) e [implantando pacotes da Web](deploying-web-packages.md), fornecem orientação sobre como usar o pacote da Web que você criou. O tutorial final desta série, [Advanced Enterprise Web Deployment](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), fornece orientação sobre como personalizar e solucionar problemas do processo de empacotamento.

Para obter uma introdução mais detalhada aos arquivos de projeto e ao WPP, consulte [dentro do Microsoft Build Engine: usando o MSBuild e o Team Foundation Build](http://amzn.com/0735645248) de Sayed Ibrahim Hashimi e William BARTHOLOMEW, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Anterior](understanding-the-build-process.md)
> [Próximo](configuring-parameters-for-web-package-deployment.md)
