---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Executando uma implantação de What If | Microsoft Docs
author: jrjlee
description: Este tópico descreve como executar implantações ' e se ' (ou simuladas) usando a ferramenta de implantação da Web do Serviços de Informações da Internet (IIS) (Implantação da Web) e o V...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: 73a0e038cc0d4ebae0ffc8ed3fd2de4c9dad673c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628887"
---
# <a name="performing-a-what-if-deployment"></a>Execução de uma implantação "What If"

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como executar implantações "e se" (ou simuladas) usando a Implantação da Web ferramenta de implantação da Web do Serviços de Informações da Internet (IIS) e o VSDBCMD. Isso permite que você determine os efeitos da lógica de implantação em um ambiente de destino específico antes de realmente implantar seu aplicativo.

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no coração desses tutoriais é baseado na abordagem do arquivo de projeto dividido descrito em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), no qual o processo de compilação e implantação é controlado&#x2014;por dois arquivos de projeto, um contendo instruções de Build que se aplicam a todos os ambientes de destino e um contendo configurações específicas de ambiente e de implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto independente do ambiente para formar um conjunto completo de instruções de compilação.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Executando uma implantação "What If" para pacotes da Web

O Implantação da Web inclui funcionalidade que permite executar implantações no modo "e se" (ou avaliação). Quando você implanta artefatos no modo "e se", o Implantação da Web gera um arquivo de log como se você tivesse realizado a implantação, mas não altera de fato nada no servidor de destino. A revisão do arquivo de log pode ajudá-lo a entender o impacto que sua implantação terá no servidor de destino, em particular:

- O que será adicionado.
- O que será atualizado.
- O que será excluído.

Como uma implantação "e se" não muda realmente nada no servidor de destino, o que nem sempre pode fazer é prever se uma implantação terá sucesso.

Conforme descrito em [implantando pacotes da Web](../web-deployment-in-the-enterprise/deploying-web-packages.md), você pode implantar pacotes da web usando implantação da Web&#x2014;de duas maneiras usando o utilitário de linha de comando MSDeploy. exe diretamente ou executando o arquivo *. Deploy. cmd* que o processo de compilação gera.

Se você estiver usando o MSDeploy. exe diretamente, poderá executar uma implantação "e se" adicionando o sinalizador **– WhatIf** ao comando. Por exemplo, para avaliar o que aconteceria se você implantou o pacote ContactManager. Mvc. zip em um ambiente de preparo, o comando MSDeploy deve ser semelhante ao seguinte:

[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]

Quando estiver satisfeito com os resultados da implantação "e se", você poderá remover o sinalizador **– WhatIf** para executar uma implantação dinâmica.

> [!NOTE]
> Para obter mais informações sobre as opções de linha de comando para o MSDeploy. exe, consulte [implantação da Web configurações de operação](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Se você estiver usando o arquivo *. Deploy. cmd* , poderá executar uma implantação "e se" incluindo o sinalizador **/t** Flag (modo de avaliação) em vez do sinalizador **/y** ("Sim" ou o modo de atualização) no comando. Por exemplo, para avaliar o que aconteceria se você implantou o pacote ContactManager. Mvc. zip executando o arquivo *. Deploy. cmd* , o comando deve ser semelhante ao seguinte:

[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]

Quando estiver satisfeito com os resultados de sua implantação de "modo de avaliação", você poderá substituir o sinalizador **/t** por um sinalizador **/y** para executar uma implantação dinâmica:

[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]

> [!NOTE]
> Para obter mais informações sobre as opções de linha de comando para arquivos *. Deploy. cmd* , consulte [como: instalar um pacote de implantação usando o arquivo Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx). Se você executar o arquivo *. Deploy. cmd* sem especificar nenhum sinalizador, o prompt de comando exibirá uma lista de sinalizadores disponíveis.

## <a name="performing-a-what-if-deployment-for-databases"></a>Executando uma implantação "What If" para bancos de dados

Esta seção pressupõe que você esteja usando o utilitário VSDBCMD para executar a implantação de banco de dados incremental baseada em esquema. Essa abordagem é descrita mais detalhadamente na [implantação de projetos de banco de dados](../web-deployment-in-the-enterprise/deploying-database-projects.md). Recomendamos que você se familiarize com este tópico antes de aplicar os conceitos descritos aqui.

Ao usar o VSDBCMD no modo de **implantação** , você pode usar o sinalizador **/DD** (ou **/DeployToDatabase**) para controlar se o VSDBCMD realmente implantará o banco de dados ou apenas gerará um script de implantação. Se você estiver implantando um arquivo. dbschema, esse será o comportamento:

- Se você especificar **/DD +** ou **/DD**, VSDBCMD irá gerar um script de implantação e implantar o banco de dados.
- Se você especificar **/DD-** ou omitir a opção, o VSDBCMD irá gerar apenas um script de implantação.

> [!NOTE]
> Se você estiver implantando um arquivo. DeployManifest em vez de um arquivo. dbschema, o comportamento da opção **/DD** será muito mais complicado. Essencialmente, VSDBCMD ignorará o valor da opção **/DD** se o arquivo. DeployManifest incluir um elemento **DeployToDatabase** com um valor de **true**. A [implantação de projetos de banco de dados](../web-deployment-in-the-enterprise/deploying-database-projects.md) descreve esse comportamento em completo.

Por exemplo, para gerar um script de implantação para o banco de dados **ContactManager** sem realmente implantar o banco de dados, o comando VSDBCMD deve ser semelhante ao seguinte:

[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]

VSDBCMD é uma ferramenta de implantação de banco de dados diferencial e, como tal, o script de implantação é gerado dinamicamente para conter todos os comandos SQL necessários para atualizar o banco de dados atual, se houver, para o esquema especificado. A revisão do script de implantação é uma maneira útil de determinar o impacto que a sua implantação terá no banco de dados atual e os que ele contém. Por exemplo, talvez você queira determinar:

- Se as tabelas existentes serão removidas e se isso resultará em perda de dados.
- Se a ordem das operações acarreta um risco de perda de dados, por exemplo, se você estiver dividindo ou mesclando tabelas.

Se estiver satisfeito com o script de implantação, você poderá repetir o VSDBCMD com um sinalizador **/DD +** para fazer as alterações. Como alternativa, você pode editar o script de implantação para atender às suas necessidades e, em seguida, executá-lo manualmente no servidor de banco de dados.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Integrando a funcionalidade "What If" em arquivos de projeto personalizados

Em cenários de implantação mais complexos, você desejará usar um arquivo de projeto de Microsoft Build Engine (MSBuild) personalizado para encapsular sua lógica de compilação e implantação, conforme descrito em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Por exemplo, na solução de exemplo [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , o arquivo *Publish. proj* :

- Compila a solução.
- Usa Implantação da Web para empacotar e implantar o aplicativo ContactManager. Mvc.
- Usa Implantação da Web para empacotar e implantar o aplicativo ContactManager. Service.
- Implanta o banco de dados **ContactManager** .

Ao integrar a implantação de vários pacotes da Web e/ou bancos de dados em um processo de etapa única dessa forma, você também pode querer a opção de executar toda a implantação em um modo "e se".

O arquivo *Publish. proj* demonstra como você pode fazer isso. Primeiro, você precisa criar uma propriedade para armazenar o valor "e se":

[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]

Nesse caso, você criou uma propriedade chamada **WhatIf** com um valor padrão de **false**. Os usuários podem substituir esse valor definindo a propriedade como **true** em um parâmetro de linha de comando, como você verá em breve.

O próximo estágio é parametrizar todos os comandos Implantação da Web e VSDBCMD para que os sinalizadores reflitam o valor da propriedade **WhatIf** . Por exemplo, o próximo destino (extraído do arquivo *Publish. proj* e simplificado) executa o arquivo *. Deploy. cmd* para implantar um pacote da Web. Por padrão, o comando inclui uma opção **/y** ("Sim" ou o modo de atualização). Se **WhatIf** for definido como **true**, isso será substituído por uma opção **/t** (versão de avaliação ou "e se").

[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]

Da mesma forma, o próximo destino usa o utilitário VSDBCMD para implantar um banco de dados. Por padrão, uma opção **/DD** não está incluída. Isso significa que o VSDBCMD irá gerar um script de implantação, mas não implantará o banco de dados&#x2014;em outras palavras, um cenário "e se". Se a propriedade **WhatIf** não estiver definida como **true**, uma opção **/DD** será adicionada e VSDBCMD implantará o banco de dados.

[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]

Você pode usar a mesma abordagem para parametrizar todos os comandos relevantes em seu arquivo de projeto. Quando desejar executar uma implantação "e se", você poderá simplesmente fornecer um valor da propriedade **WhatIf** da linha de comando:

[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]

Dessa forma, você pode executar uma implantação "e se" para todos os componentes do projeto em uma única etapa.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como executar implantações "e se" usando Implantação da Web, VSDBCMD e MSBuild. Uma implantação "e se" permite avaliar o impacto de uma implantação proposta antes de realmente fazer qualquer alteração no ambiente de destino.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre Implantação da Web sintaxe de linha de comando, consulte [implantação da Web configurações de operação](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Para obter orientação sobre opções de linha de comando ao usar o arquivo *. Deploy. cmd* , consulte [como: instalar um pacote de implantação usando o arquivo Deploy. cmd](https://msdn.microsoft.com/library/ff356104.aspx). Para obter orientação sobre a sintaxe de linha de comando VSDBCMD, consulte [referência de linha de comando para VSDBCMD. EXE (implantação e importação de esquema)](https://msdn.microsoft.com/library/dd193283.aspx).

> [!div class="step-by-step"]
> [Anterior](advanced-enterprise-web-deployment.md)
> [Próximo](customizing-database-deployments-for-multiple-environments.md)
