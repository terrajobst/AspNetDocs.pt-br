---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: Personalizando implantações de banco de dados para vários ambientes | Microsoft Docs
author: jrjlee
description: 'Este tópico descreve como personalizar as propriedades de um banco de dados para ambientes de destino específicos como parte do processo de implantação. Observação: o tópico pressupõe th...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 8ae8cb1a322afb95c5d2e8d5e73c7825c7b2fe5a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78604023"
---
# <a name="customizing-database-deployments-for-multiple-environments"></a>Personalização das implantações de banco de dados para vários ambientes

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como personalizar as propriedades de um banco de dados para ambientes de destino específicos como parte do processo de implantação.
> 
> > [!NOTE]
> > O tópico pressupõe que você está implantando um projeto de banco de dados do Visual Studio 2010 usando MSBuild. exe e VSDBCMD. exe. Para obter mais informações sobre por que você pode escolher essa abordagem, consulte [implantação da Web na empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) e [Implantando projetos de banco de dados](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Ao implantar um projeto de banco de dados em vários destinos, muitas vezes você desejará personalizar as propriedades de implantação de banco de dados para cada ambiente de destino. Por exemplo, em ambientes de teste, você normalmente recriaria o banco de dados em todas as implantações, enquanto em ambientes de preparo ou de produção você seria muito mais provável de fazer atualizações incrementais para preservar seus dados.
> 
> Em um projeto de banco de dados do Visual Studio 2010, as configurações de implantação estão contidas em um arquivo de configuração de implantação (. sqldeployment). Este tópico mostrará como criar arquivos de configuração de implantação específicos do ambiente e especificar aquele que você deseja usar como um parâmetro VSDBCMD.

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no coração desses tutoriais baseia-se na abordagem de arquivo de projeto dividido descrita em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), no qual o processo de compilação é&#x2014;controlado por dois arquivos de projeto, um contendo instruções de Build que se aplicam a todos os ambientes de destino e um contendo configurações específicas de ambiente e de implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto independente do ambiente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Este tópico pressupõe que:

- Você usa a abordagem de arquivo de projeto dividido para implantação de solução, conforme descrito em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Você chama VSDBCMD do arquivo de projeto para implantar seu projeto de banco de dados, conforme descrito em [noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Para criar um sistema de implantação que dê suporte à variação das propriedades de implantação de banco de dados entre os ambientes de destino, você precisará:

- Crie um arquivo de configuração de implantação (. sqldeployment) para cada ambiente de destino.
- Crie um comando VSDBCMD que especifique o arquivo de configuração de implantação como uma opção de linha de comando.
- Parametrizar o comando VSDBCMD em um arquivo de projeto Microsoft Build Engine (MSBuild), para que as opções VSDBCMD sejam apropriadas para o ambiente de destino.

Este tópico mostrará como executar cada um desses procedimentos.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Criando arquivos de configuração de implantação específicos do ambiente

Por padrão, um projeto de banco de dados contém um único arquivo de configuração de implantação chamado *Database. sqldeployment*. Se você abrir esse arquivo no Visual Studio 2010, poderá ver as diferentes opções de implantação disponíveis para você:

- **Agrupamento de comparação de implantação**. Isso permite que você escolha se deseja usar o agrupamento de banco de dados do seu projeto (o agrupamento de *origem* ) ou o agrupamento de banco de dados do seu servidor de destino (o agrupamento de *destino* ). Na maioria dos casos, você desejará usar o agrupamento de origem ao implantar em um ambiente de desenvolvimento ou teste. Ao implantar em um ambiente de preparo ou de produção, normalmente você desejará deixar o agrupamento de destino inalterado para evitar problemas de interoperabilidade.
- **Implantar Propriedades de banco de dados**. Isso permite que você escolha se deseja aplicar as propriedades do banco de dados, conforme definido no arquivo *Database. sqlsettings* . Ao implantar um banco de dados pela primeira vez, você deve implantar as propriedades do banco de dados. Se você estiver atualizando um banco de dados existente, as propriedades já deverão estar em vigor e você não precisará implantá-los novamente.
- **Sempre recriar banco de dados**. Isso permite que você escolha se deseja recriar o banco de dados de destino toda vez que implantar ou fazer alterações incrementais para colocar o banco de dados de destino atualizado com o esquema. Se você recriar o banco de dados, perderá qualquer dado no banco de dados existente. Como tal, você geralmente deve definir isso como **false** para implantações em ambientes de preparo ou de produção.
- **Bloquear a implantação incremental se ocorrer perda de dados**. Isso permite que você escolha se a implantação deve ser interrompida se uma alteração no esquema de banco de dados causar a perda de dado. Normalmente, você define isso como **verdadeiro** para uma implantação em um ambiente de produção, para dar a oportunidade de intervir e proteger dados importantes. Se você tiver definido **sempre recriar banco de dados** como **false**, essa configuração não terá efeito.
- **Execute a implantação no modo de usuário único**. Isso geralmente não é um problema em ambientes de desenvolvimento ou teste. No entanto, você normalmente deve definir isso como **true** para implantações em ambientes de preparo ou produção. Isso impede que os usuários façam alterações no banco de dados enquanto a implantação está em andamento.
- **Faça backup do banco de dados antes da implantação**. Normalmente, você define isso como **verdadeiro** ao implantar em um ambiente de produção, como uma precaução contra perda de dados. Talvez você também queira defini-lo como **true** quando implantar em um ambiente de preparo, se o banco de dados de preparo contiver muitos Data.
- **Gerar instruções DROP para objetos que estão no banco de dados de destino, mas que não estão no projeto de banco de dados**. Na maioria dos casos, essa é uma parte integral e essencial da realização de alterações incrementais em um banco de dados. Se você tiver definido **sempre recriar banco de dados** como **false**, essa configuração não terá efeito.
- Não **use instruções ALTER assembly para atualizar tipos CLR**. Essa configuração determina como SQL Server deve atualizar os tipos de Common Language Runtime (CLR) para versões mais recentes do assembly. Isso deve ser definido como **false** na maioria dos cenários.

Esta tabela mostra configurações de implantação típicas para ambientes de destino diferentes. No entanto, suas configurações podem ser diferentes dependendo dos requisitos exatos.

|  | Desenvolvedor/teste | Preparo/integração | Produção |
| --- | --- | --- | --- |
| **Agrupamento de comparação de implantação** | Fonte | Destino | Destino |
| **Implantar Propriedades de banco de dados** | True | Somente a primeira vez | Somente a primeira vez |
| **Sempre recriar banco de dados** | True | Falso | Falso |
| **Bloquear a implantação incremental se puder ocorrer perda de dados** | Falso | Talvez | True |
| **Executar script de implantação no modo de usuário único** | Falso | True | True |
| **Fazer backup do banco de dados antes da implantação** | Falso | Talvez | True |
| **Gerar instruções DROP para objetos que estão no banco de dados de destino, mas que não estão no projeto de banco de dados** | Falso | True | True |
| **Não usar instruções ALTER ASSEMBLY para atualizar tipos CLR** | Falso | Falso | Falso |

> [!NOTE]
> Para obter mais informações sobre as propriedades de implantação de banco de dados e considerações de ambiente, consulte [uma visão geral das configurações de projeto de banco de dados](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), [como configurar propriedades de detalhes de implantação](https://msdn.microsoft.com/library/dd172125.aspx), [criar e implantar banco de dados em um ambiente de desenvolvimento isolado](https://msdn.microsoft.com/library/dd193409.aspx)e [criar e implantar bancos de dados em um ambiente de preparo ou de produção](https://msdn.microsoft.com/library/dd193413.aspx).

Para dar suporte à implantação de um projeto de banco de dados em vários destinos, você deve criar um arquivo de configuração de implantação para cada ambiente de destino.

**Para criar um arquivo de configuração específico do ambiente**

1. No Visual Studio 2010, na janela **Gerenciador de soluções** , clique com o botão direito do mouse no projeto de banco de dados e clique em **Propriedades**.
2. Na página Propriedades do projeto de banco de dados, na guia **implantar** , na linha **arquivo de configuração de implantação** , clique em **novo**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. Na caixa de diálogo **novo arquivo de configuração de implantação** , dê ao arquivo um nome significativo (por exemplo, **TestEnvironment. sqldeployment**) e, em seguida, clique em **salvar**.
4. Na página *[filename] * * *. sqldeployment**, defina as propriedades de implantação para corresponder aos requisitos do seu ambiente de destino e, em seguida, salve o arquivo.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Observe que o novo arquivo é adicionado à pasta propriedades em seu projeto de banco de dados.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Especificando o arquivo de configuração de implantação no VSDBCMD

Ao usar as configurações de solução (como Debug e Release) no Visual Studio 2010, você pode associar um arquivo de configuração de implantação a cada configuração. Quando você cria uma configuração específica, o processo de compilação gera um arquivo de manifesto de implantação específico da configuração que aponta para o arquivo de configuração de implantação específica da configuração. No entanto, um dos principais objetivos da abordagem da implantação descrita nesses tutoriais é dar às pessoas a capacidade de controlar o processo de implantação sem usar o Visual Studio 2010 e as configurações de solução. Nessa abordagem, a configuração da solução é a mesma, independentemente do ambiente de implantação de destino. Para personalizar sua implantação de banco de dados para um ambiente de destino específico, você pode usar as opções de linha de comando VSDBCMD para especificar o arquivo de configuração de implantação.

Para especificar um arquivo de configuração de implantação em seu VSDBCMD, use a opção **p:/DeploymentConfigurationFile** e forneça o caminho completo para o arquivo. Isso substituirá o arquivo de configuração de implantação que o manifesto de implantação identifica. Por exemplo, você pode usar esse comando VSDBCMD para implantar o banco de dados **ContactManager** em um ambiente de teste:

[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]

> [!NOTE]
> Observe que o processo de compilação pode renomear o arquivo de implantação. sqlquando ele copia o arquivo para o diretório de saída.

Se você usar variáveis de comando SQL em seus scripts SQL de pré-implantação ou pós-implantação, poderá usar uma abordagem semelhante para associar um arquivo. sqlcmdvars específico ao ambiente à sua implantação. Nesse caso, você usa a opção **p:/SqlCommandVariablesFile** para identificar o arquivo. sqlcmdvars.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Executando o comando VSDBCMD de um arquivo de projeto do MSBuild

Você pode invocar um comando VSDBCMD de um arquivo de projeto do MSBuild usando uma tarefa **exec** em um destino do MSBuild. Em sua forma mais simples, ele ficaria assim:

[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]

- Na prática, para tornar os arquivos de projeto fáceis de ler e reutilizar, você desejará criar propriedades para armazenar os vários parâmetros de linha de comando. Isso facilita que os usuários forneçam valores de propriedade em um arquivo de projeto específico do ambiente ou substituam os valores padrão da linha de comando do MSBuild. Se você usar a abordagem de arquivo de projeto dividido descrita em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), deverá dividir suas instruções e propriedades de compilação entre os dois arquivos de acordo:
- As configurações específicas do ambiente, como o nome de arquivo de configuração de implantação, a cadeia de conexão de banco de dados e o nome do banco de dados de destino, devem ir para o arquivo de projeto específico do ambiente.
- O destino do MSBuild que executa o comando VSDBCMD, junto com todas as propriedades universais, como o local do executável VSDBCMD, deve ir para o arquivo de projeto universal.

Você também deve certificar-se de criar o projeto de banco de dados antes de invocar VSDBCMD para que o arquivo. DeployManifest seja criado e pronto para uso. Você pode ver um exemplo completo dessa abordagem no tópico [noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md), que orienta você pelos arquivos de projeto na [solução de exemplo Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como você pode personalizar as propriedades do banco de dados para ambientes de destino diferentes ao implantar projetos de banco de dados usando o MSBuild e o VSDBCMD. Essa abordagem é útil quando você precisa implantar projetos de banco de dados como parte de soluções maiores e de escala empresarial. Essas soluções geralmente são implantadas em vários destinos, como ambientes de desenvolvimento ou teste em área restrita, plataformas de preparo ou de integração e ambientes de produção ou ao vivo. Cada um desses ambientes de destino geralmente requer um conjunto exclusivo de propriedades de implantação de banco de dados.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como implantar projetos de banco de dados usando o VSDBCMD. exe, consulte [Implantando projetos de banco de dados](../web-deployment-in-the-enterprise/deploying-database-projects.md). Para obter mais informações sobre como usar arquivos de projeto do MSBuild personalizados para controlar o processo de implantação, consulte [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Estes artigos sobre o MSDN fornecem diretrizes mais gerais sobre a implantação de banco de dados:

- [Uma visão geral das configurações de projeto de banco de dados](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Como configurar propriedades para detalhes de implantação](https://msdn.microsoft.com/library/dd172125.aspx)
- [Crie e implante bancos de dados em um ambiente de desenvolvimento isolado](https://msdn.microsoft.com/library/dd193409.aspx)
- [Criar e implantar bancos de dados em um ambiente de preparo ou de produção](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [Anterior](performing-a-what-if-deployment.md)
> [Próximo](deploying-database-role-memberships-to-test-environments.md)
