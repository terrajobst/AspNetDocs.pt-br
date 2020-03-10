---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Implantando associações de função de banco de dados a ambientes de teste | Microsoft Docs
author: jrjlee
description: Este tópico descreve como adicionar contas de usuário a funções de banco de dados como parte de uma implantação de solução em um ambiente de teste. Quando você implanta uma solução que contém...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: a15f5bf5f659d151e91ef9e53c5ad55bcd8e2b01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587587"
---
# <a name="deploying-database-role-memberships-to-test-environments"></a>Implantação das associações de função de banco de dados em ambientes de teste

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como adicionar contas de usuário a funções de banco de dados como parte de uma implantação de solução em um ambiente de teste.
> 
> Quando você implanta uma solução que contém um projeto de banco de dados em um ambiente de preparo ou de produção, normalmente não deseja que o desenvolvedor Automatize a adição de contas de usuário a funções de banco de dados. Na maioria dos casos, o desenvolvedor não saberá quais contas de usuário precisam ser adicionadas a quais funções de banco de dados, e esses requisitos podem mudar a qualquer momento. No entanto, quando você implanta uma solução que contém um projeto de banco de dados em um ambiente de desenvolvimento ou teste, a situação normalmente é diferente:
> 
> - Normalmente, o desenvolvedor implanta novamente a solução regularmente, muitas vezes por dia.
> - O banco de dados é normalmente recriado em cada implantação, o que significa que os usuários do banco de dados devem ser criados e adicionados às funções após cada implantação.
> - O desenvolvedor normalmente tem controle total sobre o ambiente de desenvolvimento ou teste de destino.
> 
> Nesse cenário, geralmente é benéfico criar automaticamente usuários de banco de dados e atribuir associações de função de banco de dados como parte do processo de implantação.
> 
> O fator chave é que essa operação precisa ser condicional com base no ambiente de destino. Se estiver implantando em um ambiente de preparo ou de produção, você desejará ignorar a operação. Se você estiver implantando em um ambiente de desenvolvimento ou de teste, você deseja implantar associações de função sem mais intervenção. Este tópico descreve uma abordagem que você pode usar para resolver esse desafio.

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no coração desses tutoriais baseia-se na abordagem de arquivo de projeto dividido descrita em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), no qual o processo de compilação é&#x2014;controlado por dois arquivos de projeto, um contendo instruções de Build que se aplicam a todos os ambientes de destino e um contendo configurações específicas de ambiente e de implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto independente do ambiente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Este tópico pressupõe que:

- Você usa a abordagem de arquivo de projeto dividido para implantação de solução, conforme descrito em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Você chama VSDBCMD do arquivo de projeto para implantar seu projeto de banco de dados, conforme descrito em [noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Para criar usuários de banco de dados e atribuir associações de função ao implantar um projeto de banco de dados em um ambiente de teste, você precisará:

- Crie um script de Transact linguagem SQL (Transact-SQL) que faz as alterações necessárias no banco de dados.
- Crie um destino de Microsoft Build Engine (MSBuild) que usa o utilitário sqlcmd. exe para executar o script SQL.
- Configure seus arquivos de projeto para invocar o destino quando você estiver implantando sua solução em um ambiente de teste.

Este tópico mostrará como executar cada um desses procedimentos.

## <a name="scripting-the-database-role-memberships"></a>Criando scripts para as associações da função de banco de dados

Você pode criar um script Transact-SQL de várias maneiras diferentes e em qualquer local escolhido. A abordagem mais fácil é criar o script dentro de sua solução no Visual Studio 2010.

**Para criar um script SQL**

1. Na janela **Gerenciador de soluções** , expanda o nó do projeto de banco de dados.
2. Clique com o botão direito do mouse na pasta **scripts** , aponte para **Adicionar**e clique em **nova pasta**.
3. Digite **teste** como o nome da pasta e pressione Enter.
4. Clique com o botão direito do mouse na pasta **Test** , aponte para **Adicionar**e clique em **script**.
5. Na caixa de diálogo **Adicionar novo item** , dê um nome significativo ao seu script (por exemplo, **AddRoleMemberships. SQL**) e clique em **Adicionar**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. No arquivo *AddRoleMemberships. SQL* , adicione instruções TRANSACT-SQL que:

    1. Crie um usuário de banco de dados para o logon SQL Server que acessará seu banco de dados.
    2. Adicione o usuário de banco de dados a qualquer função de banco de dados necessária.
7. O arquivo deve ser semelhante a este:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Salve o arquivo.

## <a name="executing-the-script-on-the-target-database"></a>Executando o script no banco de dados de destino

O ideal é que você execute quaisquer scripts Transact-SQL necessários como parte de um script pós-implantação ao implantar seu projeto de banco de dados. No entanto, os scripts pós-implantação não permitem que você execute a lógica condicionalmente com base nas configurações da solução ou nas propriedades de compilação. A alternativa é executar seus scripts SQL diretamente do arquivo de projeto do MSBuild, criando um elemento de **destino** que executa um comando sqlcmd. exe. Você pode usar este comando para executar o script no banco de dados de destino:

[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]

> [!NOTE]
> Para obter mais informações sobre as opções de linha de comando do sqlcmd, consulte [sqlcmd Utility](https://msdn.microsoft.com/library/ms162773.aspx).

Antes de inserir esse comando em um destino do MSBuild, você precisa considerar em quais condições você deseja que o script seja executado:

- O banco de dados de destino deve existir antes de você alterar suas associações de função. Assim, você precisa executar esse script *após* a implantação do banco de dados.
- Você precisa incluir uma condição para que o script seja executado somente para ambientes de teste.
- Se você estiver executando uma implantação&#x2014;"e se" em outras palavras, se você estiver gerando scripts de implantação, mas não&#x2014;executá-los de fato, não deverá executar o script SQL.

Se você estiver usando a abordagem de arquivo de projeto dividido descrita em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), conforme demonstrado pela solução de exemplo do Contact Manager, você poderá dividir as instruções de compilação para o script SQL como este:

- Qualquer propriedade específica do ambiente necessária, junto com a propriedade que determina se as permissões devem ser implantadas, deve estar no arquivo de projeto específico do ambiente (por exemplo, *env-dev. proj*).
- O próprio destino do MSBuild, junto com as propriedades que não serão alteradas entre os ambientes de destino, devem ir para o arquivo de projeto universal (por exemplo, *Publish. proj*).

No arquivo de projeto específico do ambiente, você precisa definir o nome do servidor de banco de dados, o nome do banco de dados de destino e uma propriedade booliana que permite ao usuário especificar se as associações de função devem ser implantadas.

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]

No arquivo de projeto universal, você precisa fornecer o local do executável SQLCMD e o local do script SQL que deseja executar. Essas propriedades permanecerão as mesmas, independentemente do ambiente de destino. Você também precisa criar um destino do MSBuild para executar o comando sqlcmd.

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]

Observe que você adiciona o local do executável sqlcmd como uma propriedade estática, pois isso pode ser útil para outros destinos. Por outro lado, você define o local do seu script SQL e a sintaxe do comando sqlcmd como propriedades dinâmicas dentro do destino, pois eles não serão necessários antes de o destino ser executado. Nesse caso, o destino **DeployTestDBPermissions** será executado somente se essas condições forem atendidas:

- A propriedade **DeployTestDBRoleMemberships** é definida como **true**.
- O usuário não especificou um sinalizador **WhatIf = true** .

Por fim, não se esqueça de invocar o destino. No arquivo *Publish. proj* , você pode fazer isso adicionando o destino à lista de dependências para o destino **FullPublish** padrão. Você precisa garantir que o destino **DeployTestDBPermissions** não seja executado até que o destino **PublishDbPackages** seja executado.

[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]

## <a name="conclusion"></a>Conclusão

Este tópico descreveu uma maneira na qual você pode adicionar usuários de banco de dados e associações de função como uma ação pós-implantação ao implantar um projeto de banco de dados. Isso normalmente é útil quando você recria regularmente um banco de dados em um ambiente de teste, mas ele normalmente deve ser evitado quando você implanta bancos de dados em ambientes de preparo ou produção. Dessa forma, você deve garantir que use a lógica condicional necessária para que os usuários do banco de dados e as associações de função sejam criados somente quando for apropriado.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como usar o VSDBCMD para implantar projetos de banco de dados, consulte [Implantando projetos de banco de dados](../web-deployment-in-the-enterprise/deploying-database-projects.md). Para obter orientação sobre como personalizar implantações de banco de dados para diferentes ambientes de destino, consulte [Personalizando implantações de banco de dados para vários ambientes](customizing-database-deployments-for-multiple-environments.md). Para obter mais informações sobre como usar arquivos de projeto do MSBuild personalizados para controlar o processo de implantação, consulte [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Para obter mais informações sobre as opções de linha de comando do sqlcmd, consulte [sqlcmd Utility](https://msdn.microsoft.com/library/ms162773.aspx).

> [!div class="step-by-step"]
> [Anterior](customizing-database-deployments-for-multiple-environments.md)
> [Próximo](deploying-membership-databases-to-enterprise-environments.md)
