---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Compreendendo o arquivo de projeto | Microsoft Docs
author: jrjlee
description: Os arquivos de projeto do Microsoft Build Engine (MSBuild) se encontram no coração do processo de compilação e implantação. Este tópico começa com uma visão geral conceitual do MSBuild...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 419fe51aaf65bddcc2c50380f099f842a8d9439c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78626227"
---
# <a name="understanding-the-project-file"></a>Noções básicas sobre o arquivo de projeto

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Os arquivos de projeto do Microsoft Build Engine (MSBuild) se encontram no coração do processo de compilação e implantação. Este tópico começa com uma visão geral conceitual do MSBuild e do arquivo de projeto. Ele descreve os principais componentes que você vai chegar ao trabalhar com arquivos de projeto e funciona por meio de um exemplo de como você pode usar arquivos de projeto para implantar aplicativos do mundo real.
> 
> O que você aprenderá:
> 
> - Como o MSBuild usa arquivos de projeto do MSBuild para compilar projetos.
> - Como o MSBuild se integra a tecnologias de implantação, como a Implantação da Web (ferramenta de implantação da Web) do Serviços de Informações da Internet (IIS).
> - Como entender os principais componentes de um arquivo de projeto.
> - Como você pode usar arquivos de projeto para compilar e implantar aplicativos complexos.

## <a name="msbuild-and-the-project-file"></a>MSBuild e o arquivo de projeto

Quando você cria e cria soluções no Visual Studio, o Visual Studio usa o MSBuild para compilar cada projeto em sua solução. Cada projeto do Visual Studio inclui um arquivo de projeto do MSBuild, com uma extensão de arquivo que reflete&#x2014;o tipo de projeto C# , por exemplo, um projeto (. csproj), um projeto do Visual Basic.net (. vbproj) ou um projeto de banco de dados (. dbproj). Para criar um projeto, o MSBuild deve processar o arquivo de projeto associado ao projeto. O arquivo de projeto é um documento XML que contém todas as informações e instruções de que o MSBuild precisa para criar seu projeto, como o conteúdo a ser incluído, os requisitos de plataforma, informações de controle de versão, servidor Web ou configurações de servidor de banco de dados e o tarefas que devem ser executadas.

Os arquivos de projeto do MSBuild são baseados no [esquema XML do MSBuild](/visualstudio/msbuild/msbuild-project-file-schema-reference)e, como resultado, o processo de compilação é totalmente aberto e transparente. Além disso, você não precisa instalar o Visual Studio para usar o mecanismo&#x2014;do MSBuild. o executável do MSBuild. exe faz parte do .NET Framework e você pode executá-lo em um prompt de comando. Como desenvolvedor, você pode criar seus próprios arquivos de projeto do MSBuild, usando o esquema XML do MSBuild, para impor um controle sofisticado e refinado sobre como seus projetos são criados e implantados. Esses arquivos de projeto personalizados funcionam exatamente da mesma forma que os arquivos de projeto que o Visual Studio gera automaticamente.

> [!NOTE]
> Você também pode usar arquivos de projeto do MSBuild com o serviço Team Build no Team Foundation Server (TFS). Por exemplo, você pode usar arquivos de projeto em cenários de CI (integração contínua) para automatizar a implantação em um ambiente de teste quando for feito o check-in de um novo código. Para obter mais informações, consulte [Configurando Team Foundation Server para implantação automatizada da Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).

### <a name="project-file-naming-conventions"></a>Convenções de nomenclatura de arquivo de projeto

Ao criar seus próprios arquivos de projeto, você pode usar qualquer extensão de arquivo que desejar. No entanto, para facilitar a compreensão das suas soluções, você deve usar essas convenções comuns:

- Use a extensão. proj ao criar um arquivo de projeto que compila projetos.
- Use a extensão. targets ao criar um arquivo de projeto reutilizável para importar para outros arquivos de projeto. Arquivos com uma extensão. targets normalmente não criam nada em si, eles simplesmente contêm instruções que você pode importar para seus arquivos. proj.

### <a name="integration-with-deployment-technologies"></a>Integração com tecnologias de implantação

Se você trabalhou com projetos de aplicativos Web no Visual Studio 2010, como aplicativos Web ASP.NET e aplicativos Web ASP.NET MVC, saberá que esses projetos incluem suporte interno para empacotamento e implantação do aplicativo Web em um ambiente de destino. As páginas de **Propriedades** desses projetos incluem as guias **pacote/publicar Web** e **pacote/publicar SQL** que você pode usar para configurar como os componentes do aplicativo são empacotados e implantados. Isso mostra a guia **empacotar/publicar Web** :

![](understanding-the-project-file/_static/image1.png)

A tecnologia subjacente por trás desses recursos é conhecida como o WPP (pipeline de publicação na Web). Basicamente, o WPP traz o MSBuild e o [implantação da Web](https://go.microsoft.com/?linkid=9805122) juntos para fornecer um processo completo de compilação, pacote e implantação para seus aplicativos Web.

A boa notícia é que você pode aproveitar os pontos de integração que o WPP fornece ao criar arquivos de projeto personalizados para projetos da Web. Você pode incluir instruções de implantação no arquivo de projeto, o que permite que você crie seus projetos, crie pacotes de implantação da Web e instale esses pacotes em servidores remotos por meio de um único arquivo de projeto e uma única chamada para o MSBuild. Você também pode chamar qualquer outro executável como parte do processo de compilação. Por exemplo, você pode executar a ferramenta de linha de comando VSDBCMD. exe para implantar um banco de dados de um arquivo de esquema. No decorrer deste tópico, você verá como você pode aproveitar esses recursos para atender aos requisitos de seus cenários de implantação corporativa.

> [!NOTE]
> Para obter mais informações sobre como funciona o processo de implantação de aplicativo Web, consulte [visão geral da implantação do projeto de aplicativo web ASP.net](https://msdn.microsoft.com/library/dd394698.aspx).

## <a name="the-anatomy-of-a-project-file"></a>A anatomia de um arquivo de projeto

Antes de examinar o processo de compilação mais detalhadamente, vale a pena levar alguns instantes para se familiarizar com a estrutura básica de um arquivo de projeto do MSBuild. Esta seção fornece uma visão geral dos elementos mais comuns que você encontrará ao examinar, editar ou criar um arquivo de projeto. Em particular, você aprenderá:

- Como usar *Propriedades* para gerenciar variáveis para o processo de compilação.
- Como usar *itens* para identificar as entradas para o processo de compilação, como arquivos de código.
- Como usar *destinos* e *tarefas* para fornecer instruções de execução ao MSBuild, usando *Propriedades* e *itens* definidos em outro lugar no arquivo de projeto.

Isso mostra a relação entre os elementos principais em um arquivo de projeto do MSBuild:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>O elemento Project

O elemento [Project](https://msdn.microsoft.com/library/bcxfsh87.aspx) é o elemento raiz de cada arquivo de projeto. Além de identificar o esquema XML para o arquivo de projeto, o elemento **Project** pode incluir atributos para especificar os pontos de entrada para o processo de compilação. Por exemplo, na [solução de exemplo Contact Manager](the-contact-manager-solution.md), o arquivo *Publish. proj* especifica que a compilação deve começar chamando o destino chamado **FullPublish**.

[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]

### <a name="properties-and-conditions"></a>Propriedades e condições

Normalmente, um arquivo de projeto precisa fornecer muitas partes diferentes de informações para criar e implantar seus projetos com êxito. Essas informações podem incluir nomes de servidor, cadeias de conexão, credenciais, configurações de compilação, caminhos de arquivos de origem e de destino e qualquer outra informação que você queira incluir para dar suporte à personalização. Em um arquivo de projeto, as propriedades devem ser definidas dentro de um elemento [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) . As propriedades do MSBuild consistem em pares chave-valor. Dentro do elemento **PropertyGroup** , o nome do elemento define a chave de propriedade e o conteúdo do elemento define o valor da propriedade. Por exemplo, você pode definir propriedades chamadas **ServerName** e **ConnectionString** para armazenar um nome de servidor estático e uma cadeia de conexão.

[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]

Para recuperar um valor de propriedade, use o formato *$ (PropertyName)* . Por exemplo, para recuperar o valor da propriedade **ServerName** , você digitaria:

[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]

> [!NOTE]
> Você verá exemplos de como e quando usar valores de propriedade posteriormente neste tópico.

A incorporação de informações como propriedades estáticas em um arquivo de projeto nem sempre é a abordagem ideal para gerenciar o processo de compilação. Em muitos cenários, você desejará obter as informações de outras fontes ou capacitar o usuário a fornecer as informações do prompt de comando. O MSBuild permite que você especifique qualquer valor de propriedade como um parâmetro de linha de comando. Por exemplo, o usuário pode fornecer um valor para o **ServerName** quando ele executa o MSBuild. exe na linha de comando.

[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]

> [!NOTE]
> Para obter mais informações sobre os argumentos e as opções que você pode usar com o MSBuild. exe, consulte [referência de linha de comando do MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

Você pode usar a mesma sintaxe de propriedade para obter os valores de variáveis de ambiente e propriedades de projeto internas. Muitas propriedades usadas com frequência são definidas para você e você pode usá-las em seus arquivos de projeto, incluindo o nome de parâmetro relevante. Por exemplo, para recuperar a&#x2014;plataforma de projeto atual, por exemplo, **x86** ou **AnyCpu**&#x2014;, você pode incluir a referência de propriedade **$ (Platform)** em seu arquivo de projeto. Para obter mais informações, consulte [macros para comandos e propriedades de compilação](https://msdn.microsoft.com/library/c02as0cs.aspx), [Propriedades de projeto MSBuild comuns](https://msdn.microsoft.com/library/bb629394.aspx)e [Propriedades reservadas](https://msdn.microsoft.com/library/ms164309.aspx).

As propriedades geralmente são usadas em conjunto com *condições*. A maioria dos elementos do MSBuild oferece suporte ao atributo **Condition** , que permite especificar os critérios nos quais o MSBuild deve avaliar o elemento. Por exemplo, considere esta definição de propriedade:

[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]

Quando o MSBuild processa essa definição de propriedade, ele primeiro verifica se um valor de propriedade **$ (OutputRoot)** está disponível. Se o valor da propriedade estiver&#x2014;em branco em outras palavras, o usuário não forneceu um valor&#x2014;para essa propriedade. a condição é avaliada como **true** e o valor da propriedade é definido como **.. \Publish\Out**. Se o usuário forneceu um valor para essa propriedade, a condição é avaliada como **false** e o valor da propriedade estática não é usado.

Para obter mais informações sobre as diferentes maneiras em que você pode especificar condições, consulte [condições do MSBuild](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Itens e grupos de item

Uma das funções importantes do arquivo de projeto é definir as entradas para o processo de compilação. Normalmente, essas entradas são arquivos&#x2014;de código, arquivos de configuração, arquivos de comando e quaisquer outros arquivos que você precise processar ou copiar como parte do processo de compilação. No esquema de projeto do MSBuild, essas entradas são representadas por elementos de [Item](https://msdn.microsoft.com/library/ms164283.aspx) . Em um arquivo de projeto, os itens devem ser definidos em um elemento [rowgroup](https://msdn.microsoft.com/library/646dk05y.aspx) . Assim como os elementos de **Propriedade** , você pode nomear um elemento **Item** como desejar. No entanto, você deve especificar um atributo **include** para identificar o arquivo ou curinga que o item representa.

[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]

Ao especificar vários elementos de **Item** com o mesmo nome, você está criando efetivamente uma lista nomeada de recursos. Uma boa maneira de ver isso em ação é dar uma olhada dentro de um dos arquivos de projeto criados pelo Visual Studio. Por exemplo, o arquivo *ContactManager. Mvc. csproj* na solução de exemplo inclui muitos grupos de itens, cada um com vários elementos de **Item** nomeados de forma idêntica.

[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]

Dessa forma, o arquivo de projeto está instruindo o MSBuild a construir listas de arquivos que precisam ser processados da mesma maneira&#x2014;que a lista de **referência** inclui assemblies que devem estar em vigor para uma compilação bem-sucedida, a lista de **compilações** inclui arquivos de código que devem ser compilados e a lista de **conteúdo** inclui recursos que devem ser copiados inalterados. Veremos como o processo de compilação referencia e usa esses itens mais adiante neste tópico.

Elementos de item também podem incluir elementos filho [ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx) . Esses são pares chave-valor definidos pelo usuário e basicamente representam propriedades que são específicas para esse item. Por exemplo, muitos dos elementos de item de **compilação** no arquivo de projeto incluem elementos filho **DependentUpon** .

[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]

> [!NOTE]
> Além dos metadados de item criados pelo usuário, todos os itens são atribuídos a vários metadados comuns na criação. Para obter mais informações, consulte [Metadados de itens conhecidos](https://msdn.microsoft.com/library/ms164313.aspx).

Você pode criar elementos de **grupo** de projetos dentro do elemento de **projeto** de nível raiz ou dentro de elementos de **destino** específicos. Os elementos do **rowgroup** também oferecem suporte a atributos de **condição** , o que permite que você personalize as entradas para o processo de compilação de acordo com as condições, como a configuração do projeto ou a plataforma.

### <a name="targets-and-tasks"></a>Destinos e tarefas

No esquema do MSBuild, um elemento [Task](https://msdn.microsoft.com/library/77f2hx1s.aspx) representa uma instrução (ou tarefa) de compilação individual. O MSBuild inclui uma infinidade de tarefas predefinidas. Por exemplo:

- A tarefa de **cópia** copia os arquivos para um novo local.
- A tarefa **CSC** invoca o compilador Visual C# .
- A tarefa **Vbc** invoca o compilador Visual Basic.
- A tarefa **exec** executa um programa especificado.
- A tarefa **mensagem** grava uma mensagem em um agente de log.

> [!NOTE]
> Para obter detalhes completos das tarefas que estão disponíveis prontamente, consulte referência de [tarefa do MSBuild](https://msdn.microsoft.com/library/7z253716.aspx). Para obter mais informações sobre tarefas, incluindo como criar suas próprias tarefas personalizadas, consulte [tarefas do MSBuild](https://msdn.microsoft.com/library/ms171466.aspx).

As tarefas sempre devem estar contidas nos elementos de [destino](https://msdn.microsoft.com/library/t50z2hka.aspx) . Um elemento de **destino** é um conjunto de uma ou mais tarefas que são executadas sequencialmente, e um arquivo de projeto pode conter vários destinos. Quando você deseja executar uma tarefa ou um conjunto de tarefas, você invoca o destino que as contém. Por exemplo, suponha que você tenha um arquivo de projeto simples que registra uma mensagem.

[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]

Você pode invocar o destino na linha de comando, usando a opção **/t** para especificar o destino.

[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]

Como alternativa, você pode adicionar um atributo **DefaultTargets** ao elemento **Project** para especificar os destinos que deseja invocar.

[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]

Nesse caso, você não precisa especificar o destino na linha de comando. Você pode simplesmente especificar o arquivo de projeto e o MSBuild invocará o destino **FullPublish** para você.

[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]

Os destinos e as tarefas podem incluir atributos de **condição** . Dessa forma, você pode optar por omitir os destinos inteiros ou tarefas individuais se determinadas condições forem atendidas.

Em geral, ao criar tarefas e destinos úteis, você precisará consultar as propriedades e os itens que você definiu em outro lugar no arquivo de projeto:

- Para usar um valor de propriedade, digite **$ (***PropertyName***)** , em que *PropertyName* é o nome do elemento de **Propriedade** ou o nome do parâmetro.
- Para usar um item, digite **@ (***ItemName***)** , em que *ItemName* é o nome do elemento **Item** .

> [!NOTE]
> Lembre-se de que, se você criar vários itens com o mesmo nome, estará criando uma lista. Por outro lado, se você criar várias propriedades com o mesmo nome, o último valor de propriedade que você fornecer substituirá todas as propriedades anteriores&#x2014;com o mesmo nome, uma propriedade só poderá conter um único valor.

Por exemplo, no arquivo *Publish. proj* na solução de exemplo, dê uma olhada no destino **BuildProjects** .

[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]

Neste exemplo, você pode observar estes pontos-chave:

- Se o parâmetro **BuildingInTeamBuild** for especificado e tiver um valor de **true**, nenhuma das tarefas nesse destino será executada.
- O destino contém uma única instância da tarefa do [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) . Essa tarefa permite que você crie outros projetos do MSBuild.
- O item **ProjectsToBuild** é passado para a tarefa. Este item pode representar uma lista de arquivos de projeto ou de solução, todos definidos por elementos de item **ProjectsToBuild** dentro de um grupo de itens. Nesse caso, o item **ProjectsToBuild** se refere a um único arquivo de solução.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- Os valores de propriedade passados para a tarefa **MSBuild** incluem parâmetros chamados **OutputRoot** e **Configuration**. Eles serão definidos como valores de parâmetro se forem fornecidos ou valores de propriedade estática se não forem.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

Você também pode ver que a tarefa do **MSBuild** invoca um destino chamado **Build**. Esse é um dos vários destinos internos que são amplamente usados em arquivos de projeto do Visual Studio e estão disponíveis para você em seus arquivos de projeto personalizados, como **Compilar**, **limpar**, **Recompilar**e **publicar**. Você aprenderá mais sobre o uso de destinos e tarefas para controlar o processo de compilação e sobre a tarefa do **MSBuild** em particular, mais adiante neste tópico.

> [!NOTE]
> Para obter mais informações sobre destinos, consulte [destinos do MSBuild](https://msdn.microsoft.com/library/ms171462.aspx).

## <a name="splitting-project-files-to-support-multiple-environments"></a>Dividindo arquivos de projeto para dar suporte a vários ambientes

Suponha que você deseja ser capaz de implantar uma solução em vários ambientes, como servidores de teste, plataformas de preparo e ambientes de produção. A configuração pode variar substancialmente entre esses&#x2014;ambientes não apenas em termos de nomes de servidor, cadeias de conexão e assim por diante, mas também em termos de credenciais, configurações de segurança e muitos outros fatores. Se você precisar fazer isso regularmente, não é muito útil editar várias propriedades em seu arquivo de projeto sempre que você alternar o ambiente de destino. Nem é uma solução ideal para exigir uma lista infinita de valores de propriedade a serem fornecidos ao processo de compilação.

Felizmente, há uma alternativa. O MSBuild permite dividir a configuração de Build em vários arquivos de projeto. Para ver como isso funciona, na solução de exemplo, observe que há dois arquivos de projeto personalizados:

- *Publish. proj*, que contém propriedades, itens e destinos comuns a todos os ambientes.
- *Env-dev. proj*, que contém propriedades específicas para um ambiente de desenvolvedor.

Agora observe que o arquivo *Publish. proj* inclui um elemento [Import](https://msdn.microsoft.com/library/92x05xfs.aspx) , imediatamente abaixo da marca do **projeto** de abertura.

[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]

O elemento **Import** é usado para importar o conteúdo de outro arquivo de projeto do MSBuild para o arquivo de projeto do MSBuild atual. Nesse caso, o parâmetro **TargetEnvPropsFile** fornece o nome do arquivo do projeto que você deseja importar. Você pode fornecer um valor para esse parâmetro ao executar o MSBuild.

[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]

Isso mescla efetivamente o conteúdo dos dois arquivos em um único arquivo de projeto. Usando essa abordagem, você pode criar um arquivo de projeto que contém a configuração de compilação universal e vários arquivos de projeto suplementares contendo propriedades específicas do ambiente. Como resultado, simplesmente executar um comando com um valor de parâmetro diferente permite que você implante sua solução em um ambiente diferente.

![](understanding-the-project-file/_static/image3.png)

A divisão dos arquivos de projeto dessa maneira é uma boa prática a ser seguida. Ele permite que os desenvolvedores implantem em vários ambientes executando um único comando, ao mesmo tempo em que evitam a duplicação de propriedades de compilação universal em vários arquivos de projeto.

> [!NOTE]
> Para obter orientação sobre como personalizar os arquivos de projeto específicos do ambiente para seus próprios ambientes de servidor, consulte [Configurando propriedades de implantação para um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).

## <a name="conclusion"></a>Conclusão

Este tópico forneceu uma introdução geral aos arquivos de projeto do MSBuild e explicou como você pode criar seus próprios arquivos de projeto personalizados para controlar o processo de compilação. Ele também introduziu o conceito de divisão de arquivos de projeto em instruções de compilação universal e propriedades de compilação específicas do ambiente, para facilitar a criação e a implantação de projetos em vários destinos.

O próximo tópico, [noções básicas sobre o processo de compilação](understanding-the-build-process.md), fornece mais informações sobre como você pode usar arquivos de projeto para controlar a compilação e a implantação ao orientá-lo na implantação de uma solução com um nível realista de complexidade.

## <a name="further-reading"></a>Leitura adicional

Para obter uma introdução mais detalhada aos arquivos de projeto e ao WPP, consulte [dentro do Microsoft Build Engine: usando o MSBuild e o Team Foundation Build](http://amzn.com/0735645248) de Sayed Ibrahim Hashimi e William BARTHOLOMEW, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Anterior](setting-up-the-contact-manager-solution.md)
> [Próximo](understanding-the-build-process.md)
