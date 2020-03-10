---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Configurando parâmetros para implantação de pacote da Web | Microsoft Docs
author: jrjlee
description: Este tópico descreve como definir valores de parâmetro, como nomes de aplicativos da Web do Serviços de Informações da Internet (IIS), cadeias de conexão e pontos de extremidade de serviço,...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: f04ace98d81a33053b10cab7e40dbd75a6c0992c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78544950"
---
# <a name="configuring-parameters-for-web-package-deployment"></a>Configuração de parâmetros para a implantação de pacote da Web

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como definir valores de parâmetro, como nomes de aplicativos da Web do Serviços de Informações da Internet (IIS), cadeias de conexão e pontos de extremidade de serviço, quando você implanta um pacote da Web em um servidor Web IIS remoto.

Quando você cria um projeto de aplicativo Web, o processo de criação e empacotamento gera três arquivos de chave:

- Um arquivo *. zip do [nome do projeto]* . Este é o pacote de implantação da Web para seu projeto de aplicativo Web. Este pacote contém todos os assemblies, arquivos, scripts de banco de dados e recursos necessários para recriar seu aplicativo Web em um servidor Web IIS remoto.
- Um arquivo *[Project Name]. Deploy. cmd* . Ele contém um conjunto de comandos Implantação da Web parametrizados (MSDeploy. exe) que publicam seu pacote de implantação da Web em um servidor Web IIS remoto.
- Um *[nome do projeto]. Arquivo Parameters. xml* . Isso fornece um conjunto de valores de parâmetro para o comando MSDeploy. exe. Você pode atualizar os valores nesse arquivo e passá-lo para Implantação da Web como um parâmetro de linha de comando ao implantar o pacote da Web.

> [!NOTE]
> Para obter mais informações sobre o processo de criação e empacotamento, consulte [criando e empacotando projetos de aplicativos Web](building-and-packaging-web-application-projects.md).

O arquivo *Parameters. xml* é gerado dinamicamente de seu arquivo de projeto de aplicativo Web e de quaisquer arquivos de configuração em seu projeto. Quando você compilar e empacotar seu projeto, o WPP (Web Publishing Pipeline) detectará automaticamente muitas das variáveis que provavelmente serão alteradas entre ambientes de implantação, como o aplicativo Web IIS de destino e quaisquer cadeias de conexão de banco de dados. Esses valores são parametrizados automaticamente no pacote de implantação da Web e adicionados ao arquivo *Parameters. xml* . Por exemplo, se você adicionar uma cadeia de conexão ao arquivo *Web. config* em seu projeto de aplicativo Web, o processo de compilação detectará essa alteração e adicionará uma entrada ao arquivo Parameters *. xml* de forma adequada.

Em muitos casos, essa parametrização automática será suficiente. No entanto, se os usuários precisarem variar outras configurações entre ambientes de implantação, como configurações de aplicativo ou URLs de ponto de extremidade de serviço, você precisará instruir o WPP a parametrizar esses valores no pacote de implantação e adicionar entradas correspondentes ao arquivo *Parameters. xml* . As seções a seguir explicam como fazer isso.

### <a name="automatic-parameterization"></a>Parametrização automática

Quando você compilar e empacotar um aplicativo Web, o WPP irá parametrizar automaticamente estas coisas:

- O caminho e o nome do aplicativo Web IIS de destino.
- Quaisquer cadeias de conexão em seu arquivo *Web. config* .
- Cadeias de conexão para quaisquer bancos de dados que você adicionar à guia **empacotar/publicar SQL** nas páginas de propriedades do projeto.

Por exemplo, se você fosse criar e empacotar a solução de exemplo do [Contact Manager](the-contact-manager-solution.md) sem tocar no processo de parametrização de alguma forma, o WPP geraria esse arquivo *ContactManager. Mvc. Parameters. xml* :

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]

Nesse caso:

- O parâmetro **nome do aplicativo Web do IIS** é o caminho do IIS no qual você deseja implantar o aplicativo Web. O valor padrão é obtido da página **da Web empacotar/publicar** nas páginas de propriedades do projeto.
- O parâmetro de **cadeia de conexão ApplicationServices-Web. config** foi gerado a partir de um elemento **connectionStrings/Add** no arquivo *Web. config* . Representa a cadeia de conexão que o aplicativo deve usar para contatar o banco de dados de associação. O valor que você fornecer aqui será substituído no arquivo *Web. config* implantado. O valor padrão é obtido do arquivo *Web. config* de pré-implantação.

O WPP também parametriza essas propriedades no pacote de implantação gerado por ele. Você pode fornecer valores para essas propriedades ao instalar o pacote de implantação. Se você instalar o pacote manualmente por meio do Gerenciador do IIS, conforme descrito em [instalando os pacotes da Web manualmente](manually-installing-web-packages.md), o assistente de instalação solicitará que você forneça valores para quaisquer parâmetros. Se você instalar o pacote remotamente usando o arquivo *. Deploy. cmd* , conforme descrito em [implantando pacotes da Web](deploying-web-packages.md), implantação da Web irá procurar esse arquivo Parameters *. xml* para fornecer os valores de parâmetro. Você pode editar os valores no arquivo *Parameters. xml* manualmente ou pode personalizar o arquivo como parte de um processo automatizado de compilação e implantação. Esse processo é descrito em mais detalhes posteriormente neste tópico.

### <a name="custom-parameterization"></a>Parametrização personalizada

Em cenários de implantação mais complexos, muitas vezes você desejará parametrizar propriedades adicionais antes de implantar seu projeto. Em termos gerais, você deve parametrizar todas as propriedades e configurações que irão variar entre os ambientes de destino. Eles podem incluir:

- Pontos de extremidade de serviço no arquivo *Web. config* .
- Configurações do aplicativo no arquivo *Web. config* .
- Quaisquer outras propriedades declarativas que você deseja solicitar que os usuários especifiquem.

A maneira mais fácil de parametrizar essas propriedades é adicionar um arquivo *Parameters. xml* à pasta raiz do seu projeto de aplicativo Web. Por exemplo, na solução Contact Manager, o projeto ContactManager. Mvc inclui um arquivo *Parameters. xml* na pasta raiz.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Se você abrir esse arquivo, verá que ele contém uma única entrada de **parâmetro** . A entrada usa uma consulta de linguagem de caminho XML (XPath) para localizar e parametrizar a URL do ponto de extremidade do serviço ContactService Windows Communication Foundation (WCF) no arquivo *Web. config* .

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]

Além de parametrizar a URL do ponto de extremidade no pacote de implantação, o WPP também adiciona uma entrada correspondente ao arquivo *Parameters. xml* que é gerado junto com o pacote de implantação.

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]

Se você instalar o pacote de implantação manualmente, o Gerenciador do IIS solicitará o endereço do ponto de extremidade de serviço ao lado das propriedades que foram parametrizadas automaticamente. Se você instalar o pacote de implantação executando o arquivo *. Deploy. cmd* , poderá editar o arquivo *Parameters. xml* para fornecer um valor para o endereço do ponto de extremidade de serviço junto com valores para as propriedades que foram parametrizadas automaticamente.

Para obter detalhes completos sobre como criar um arquivo *Parameters. xml* , consulte [como: usar parâmetros para definir as configurações de implantação quando um pacote é instalado](https://msdn.microsoft.com/library/ff398068.aspx). O procedimento nomeado **para usar parâmetros de implantação para configurações de arquivo Web. config** fornece instruções passo a passo.

## <a name="modifying-the-setparametersxml-file"></a>Modificando o arquivo Parameters. xml

Se você planeja implantar o pacote de aplicativo Web manualmente&#x2014;executando o arquivo *. Deploy. cmd* ou executando MSDeploy. exe na linha&#x2014;de comando, não há nada para impedir que você edite manualmente o arquivo Parameters *. xml* antes da implantação. No entanto, se você estiver trabalhando em uma solução de escala empresarial, talvez seja necessário implantar um pacote de aplicativo Web como parte de um processo de compilação e implantação maior e automatizado. Nesse cenário, você precisa do Microsoft Build Engine (MSBuild) para modificar o arquivo *Parameters. xml* para você. Você pode fazer isso usando a tarefa **xmlexaminar** do MSBuild.

A [solução de exemplo do Contact Manager](the-contact-manager-solution.md) ilustra esse processo. Os exemplos de código a seguir foram editados para mostrar apenas os detalhes que são relevantes para este exemplo.

> [!NOTE]
> Para obter uma visão geral mais ampla do modelo de arquivo do projeto na solução de exemplo e uma introdução aos arquivos de projeto personalizados em geral, consulte [noções básicas sobre o arquivo de projeto](understanding-the-project-file.md) e [noções básicas sobre o processo de compilação](understanding-the-build-process.md).

Primeiro, os valores de parâmetro de interesse são definidos como propriedades no arquivo de projeto específico do ambiente (por exemplo, *env-dev. proj*).

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]

> [!NOTE]
> Para obter orientação sobre como personalizar os arquivos de projeto específicos do ambiente para seus próprios ambientes de servidor, consulte [Configurar Propriedades de implantação para um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

Em seguida, o arquivo *Publish. proj* importa essas propriedades. Como cada arquivo *Parameters. xml* é associado a um arquivo *. Deploy. cmd* e, por fim, queremos que o arquivo de projeto invoque cada arquivo *. Deploy. cmd* , o arquivo de projeto cria um *Item* do MSBuild para cada arquivo *. Deploy. cmd* e define as propriedades de interesse como *metadados do item*.

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]

Nesse caso:

- O valor de metadados **ParametersXml** indica o local do arquivo *Parameters. xml* .
- O valor de **IisWebAppName** é o caminho do IIS no qual você deseja implantar o aplicativo Web.
- O valor de **MembershipDBConnectionString** é a cadeia de conexão para o banco de dados de associação e o valor de **MembershipDBConnectionName** é o atributo de **nome** do parâmetro correspondente no arquivo *Parameters. xml* .
- O **valor de** serviceendpointvalue é o endereço do ponto de extremidade para o serviço WCF no servidor de destino e o valor **ServiceEndpointParamName** é o atributo Name do parâmetro correspondente no arquivo *Parameters. xml* .

Por fim, no arquivo *Publish. proj* , o destino **PublishWebPackages** usa a tarefa **xmlexaminar** para modificar esses valores no arquivo Parameters *. xml* .

[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]

Você observará **que cada tarefa** xmlver especifica quatro valores de atributo:

- O atributo **XmlInputPath** informa à tarefa onde encontrar o arquivo que você deseja modificar.
- O atributo de **consulta** é uma consulta XPath que identifica o nó XML que você deseja alterar.
- O atributo **Value** é o novo valor que você deseja inserir no nó XML selecionado.
- O atributo **Condition** é o critério no qual a tarefa deve ser executada ou não executada. Nesses casos, a condição garante que você não tente inserir um valor nulo ou vazio no arquivo *Parameters. xml* .

## <a name="conclusion"></a>Conclusão

Este tópico descreveu a função do arquivo *Parameters. xml* e explicou como ele é gerado quando você cria um projeto de aplicativo Web. Ele explicou como você pode parametrizar configurações adicionais adicionando um arquivo *Parameters. xml* ao seu projeto. Ele também descreveu como você pode modificar o arquivo *Parameters. xml* como parte de um processo de compilação maior e automatizado, **usando a tarefa** xmlfazer em seus arquivos de projeto.

O próximo tópico, [implantando pacotes da Web](deploying-web-packages.md), descreve como você pode implantar um pacote da Web executando o arquivo *. Deploy. cmd* ou usando comandos MSDeploy. exe diretamente. Em ambos os casos, você pode especificar o arquivo *Parameters. xml* como um parâmetro de implantação.

## <a name="further-reading"></a>Leitura adicional

Para obter informações sobre como criar pacotes da Web, consulte [criando e empacotando projetos de aplicativos Web](building-and-packaging-web-application-projects.md). Para obter diretrizes sobre como realmente implantar um pacote da Web, consulte [implantando pacotes da Web](deploying-web-packages.md). Para obter instruções passo a passo sobre como criar um arquivo *Parameters. xml* , consulte [como: usar parâmetros para definir as configurações de implantação quando um pacote é instalado](https://msdn.microsoft.com/library/ff398068.aspx).

Para obter mais informações gerais sobre a parametrização no Implantação da Web, consulte [implantação da Web parametrização em ação](https://go.microsoft.com/?linkid=9805119) (postagem no blog).

> [!div class="step-by-step"]
> [Anterior](building-and-packaging-web-application-projects.md)
> [Próximo](deploying-web-packages.md)
