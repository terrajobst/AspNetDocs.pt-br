---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Solucionando problemas do processo de empacotamento | Microsoft Docs
author: jrjlee
description: Este tópico descreve como você pode coletar informações detalhadas sobre o processo de empacotamento usando a propriedade EnablePackageProcessLoggingAndAssert no M...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 8ad649dfff085a8774cc13c11d8a3e3d48277d66
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628208"
---
# <a name="troubleshooting-the-packaging-process"></a>Solução de problemas do processo de empacotamento

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como você pode coletar informações detalhadas sobre o processo de empacotamento usando a propriedade **EnablePackageProcessLoggingAndAssert** no Microsoft Build Engine (MSBuild).
> 
> Quando você define a propriedade **EnablePackageProcessLoggingAndAssert** como **true**, o MSBuild irá:
> 
> - Adicione informações adicionais sobre o processo de empacotamento aos logs de compilação.
> - Registre erros em determinadas condições, por exemplo, se arquivos duplicados forem encontrados na lista de empacotamento.
> - Crie um diretório de log na pasta de pacote *ProjectName*\_e use-o para registrar informações sobre os arquivos que você está empacotando.
> 
> Se o processo de empacotamento estiver falhando ou se os seus pacotes de implantação da Web não contiverem os arquivos esperados, você poderá usar essas informações para solucionar o problema e identificar onde as coisas estão dando errado.
> 
> > [!NOTE]
> > A propriedade **EnablePackageProcessLoggingAndAssert** só funcionará se você compilar seu projeto usando a configuração de **depuração** . A propriedade é ignorada em outras configurações.

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no coração desses tutoriais baseia-se na abordagem de arquivo de projeto dividido descrita em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), no qual o processo de compilação é&#x2014;controlado por dois arquivos de projeto, um contendo instruções de Build que se aplicam a todos os ambientes de destino e um contendo configurações específicas de ambiente e de implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto independente do ambiente para formar um conjunto completo de instruções de compilação.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Compreendendo a propriedade EnablePackageProcessLoggingAndAssert

[Criar e empacotar projetos de aplicativos Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) descreveu como o WPP (Web Publishing Pipeline) fornece um conjunto de destinos do MSBuild que estendem a funcionalidade do MSBuild e permitem que ele se integre com a implantação da Web ferramenta de implantação da web do serviços de informações da Internet (IIS). Ao empacotar um projeto de aplicativo Web, você está invocando destinos de WPP.

Muitos desses destinos de WPP incluem lógica condicional que registra informações adicionais quando a propriedade **EnablePackageProcessLoggingAndAssert** é definida como **true**. Por exemplo, se você examinar o destino do **pacote** , poderá ver que ele cria um diretório de log adicional e gravará uma lista de arquivos em um arquivo de texto se **EnablePackageProcessLoggingAndAssert** for igual a **true**.

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]

> [!NOTE]
> Os destinos de WPP são definidos no arquivo *Microsoft. Web. Publishing. targets* na pasta% ProgramFiles (x86)% \ MSBuild\Microsoft\VisualStudio\v10.0\Web Você pode abrir esse arquivo e examinar os destinos no Visual Studio 2010 ou em qualquer editor de XML. Tome cuidado para não modificar o conteúdo do arquivo.

## <a name="enabling-the-additional-logging"></a>Habilitando o log adicional

Você pode fornecer um valor para a propriedade **EnablePackageProcessLoggingAndAssert** de várias maneiras, dependendo de como você cria seu projeto.

Se você criar seu projeto na linha de comando, poderá fornecer um valor para a propriedade **EnablePackageProcessLoggingAndAssert** como um argumento de linha de comando:

[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]

Se você estiver usando um arquivo de projeto personalizado para compilar seus projetos, poderá incluir o valor **EnablePackageProcessLoggingAndAssert** no atributo **Properties** da tarefa **MSBuild** :

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]

Se você estiver usando uma definição de compilação Team Foundation Server (TFS) para compilar seus projetos, você pode fornecer um valor para a propriedade **EnablePackageProcessLoggingAndAssert** na linha de **argumentos do MSBuild** :![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Para obter mais informações sobre como criar e configurar definições de compilação, consulte [criando uma definição de compilação que dá suporte à implantação](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

Como alternativa, se você quiser incluir o pacote em cada compilação, poderá modificar o arquivo de projeto para seu projeto de aplicativo Web para definir a propriedade **EnablePackageProcessLoggingAndAssert** como **true**. Você deve adicionar a propriedade ao primeiro elemento **PropertyGroup** dentro de seu arquivo. csproj ou. vbproj.

[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]

## <a name="reviewing-the-log-files"></a>Examinando os arquivos de log

Quando você cria e empacota um projeto de aplicativo Web com **EnablePackageProcessLoggingAndAssert** definido como **true**, o MSBuild cria uma pasta adicional chamada log na pasta de pacote *ProjectName*\_. A pasta de log contém vários arquivos:

![](troubleshooting-the-packaging-process/_static/image2.png)

A lista de arquivos que você vê variará de acordo com as coisas em seu projeto e seu processo de compilação. No entanto, esses arquivos normalmente são usados para registrar a lista de arquivos que o WPP está coletando para empacotamento, em vários estágios do processo:

- O arquivo *PreExcludePipelineCollectFilesPhaseFileList. txt* lista os arquivos que o MSBuild coleta para o empacotamento antes que todos os arquivos especificados para exclusão sejam removidos.
- O arquivo *AfterExcludeFilesFilesList. txt* contém a lista de arquivos modificados após a remoção de todos os arquivos especificados para exclusão.

    > [!NOTE]
    > Para obter mais informações sobre como excluir arquivos e pastas do processo de empacotamento, consulte [excluindo arquivos e pastas da implantação](excluding-files-and-folders-from-deployment.md).
- O arquivo *AfterTransformWebConfig. txt* lista os arquivos coletados para empacotamento após a execução de todas as transformações de *Web. config* . Nessa lista, todos os arquivos de transformação *Web. config específicos da* configuração, como *Web. Debug. config* e *Web. Release. config*, são excluídos da lista de arquivos para empacotamento. Um *Web. config* transformado único é incluído em seu lugar.
- O arquivo *PostAutoParameterizationWebConfigConnectionStrings. txt* contém a lista de arquivos depois que as cadeias de conexão no arquivo *Web. config* foram parametrizadas. Esse é o processo que permite que você substitua suas cadeias de conexão pelas configurações corretas para seu ambiente de destino ao implantar o pacote.
- O arquivo *prepackage. txt* contém a lista de pré-compilação finalizada de arquivos a serem incluídos no pacote.

> [!NOTE]
> Os nomes dos arquivos de log adicionais geralmente correspondem aos destinos do WPP. Você pode examinar esses destinos examinando o arquivo *Microsoft. Web. Publishing. targets* na pasta% ProgramFiles (x86)% \ MSBuild\Microsoft\VisualStudio\v10.0\Web

Se o conteúdo do seu pacote da Web não for o esperado, a revisão desses arquivos poderá ser uma maneira útil de identificar em que ponto as coisas do processo ficaram erradas.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como você pode usar a propriedade **EnablePackageProcessLoggingAndAssert** no MSBuild para solucionar problemas do processo de empacotamento. Ele explicou as diferentes maneiras pelas quais você pode fornecer o valor da propriedade ao processo de compilação e descreveu as informações adicionais que são registradas quando você define a propriedade como **true**.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como usar arquivos de projeto do MSBuild personalizados para controlar o processo de implantação, consulte [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Para obter mais informações sobre o WPP e como ele gerencia o processo de empacotamento, consulte [criando e empacotando projetos de aplicativos Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Para obter orientação sobre como excluir arquivos e pastas específicas de pacotes de implantação da Web, consulte [excluindo arquivos e pastas da implantação](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Anterior](running-windows-powershell-scripts-from-msbuild-project-files.md)
