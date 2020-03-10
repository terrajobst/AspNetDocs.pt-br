---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Implantando bancos de dados de associação em ambientes corporativos | Microsoft Docs
author: jrjlee
description: Este tópico explica as principais considerações e desafios que você precisará superar ao provisionar bancos de dados de serviços de aplicativos ASP.NET (mais comuns...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 50f49af502b75aa5ad52756a76a5e7340aca53f7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78526001"
---
# <a name="deploying-membership-databases-to-enterprise-environments"></a>Implantação do bancos de dados de associação em ambientes corporativos

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico explica as principais considerações e desafios que você precisará superar ao provisionar bancos de dados de serviços de aplicativos ASP.NET (mais comumente chamados de bancos de dados de associação) em ambientes de teste, de preparo ou de produção. Ele também descreve abordagens que você pode usar para atender a esses desafios.

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no coração desses tutoriais baseia-se na abordagem de arquivo de projeto dividido descrita em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), no qual o processo de compilação é&#x2014;controlado por dois arquivos de projeto, um contendo instruções de Build que se aplicam a todos os ambientes de destino e um contendo configurações específicas de ambiente e de implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto independente do ambiente para formar um conjunto completo de instruções de compilação.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Quais são os problemas quando você implanta um banco de dados de associação?

Na maioria dos casos, quando você planeja uma estratégia de implantação para um banco de dados, a primeira coisa que você precisa considerar é qual dado você deseja implantar. Em um ambiente de desenvolvimento ou teste, talvez você queira implantar dados de conta de usuário para facilitar testes rápidos e fáceis. Em um ambiente de preparo ou de produção, é muito improvável que você queira implantar dados de conta de usuário.

Infelizmente, os bancos de dados de associação do ASP.NET introduzem alguns desafios específicos que tornam essa decisão muito mais complexa:

- Uma implantação somente de esquema deixará o banco de dados de associação em um estado não operacional. Isso se deve ao fato de o banco de dados de associação incluir algum dado de configuração (na tabela **aspnet\_SchemaVersions** ) que o Database requer para funcionar. Dessa forma, se você executar uma implantação somente de esquema de seu banco de dados de associação para excluir a conta de usuário, precisará executar um script pós-implantação para adicionar os dados de configuração essenciais.
- Dependendo de como o banco de dados de associação é configurado, o provedor de associação pode usar a chave do computador para criptografar senhas e armazená-las no banco de dados. Nesse caso, qualquer dado de conta de usuário que você implantar com o banco de dados ficará inutilizável no servidor de destino. Por esse motivo, a implantação de dados de conta de usuário não é um cenário com suporte.

## <a name="choosing-a-membership-database-strategy"></a>Escolhendo uma estratégia de banco de dados de associação

Use estas diretrizes quando escolher como provisionar um banco de dados de associação em um ambiente de servidor corporativo:

- Sempre que possível, não implante bancos de dados de associação. Em vez disso, crie o banco de dados de associação manualmente no servidor de banco de dados de destino. Se você não tiver personalizado seu esquema de banco de dados de associação, poderá simplesmente criar um novo no in-situ no destino usando a [ferramenta de registro de SQL Server do ASP.net (aspnet\_RegSql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Se você não tiver nenhuma opção, mas implantar um banco&#x2014;de dados de associação, por exemplo, se tiver feito modificações extensivas no esquema&#x2014;de banco de dados, você deverá executar uma implantação somente de esquema do banco de dados de associação, excluir e executar um script de pós-implantação para adicionar quaisquer dados de configuração necessários. Você pode encontrar diretrizes amplas sobre essas abordagens em [como implantar o banco de dados de associação do ASP.NET sem incluir contas de usuário](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

É importante lembrar que *o esquema do seu banco de dados de associação provavelmente será razoavelmente estático*. Mesmo que você tenha personalizado o banco de dados de associação, é improvável que você precise atualizar o esquema regularmente,&#x2014;ele não será alterado com a mesma frequência que o código em um aplicativo Web ou um projeto de banco de dados. Dessa forma, você não precisará incluir o banco de dados de associação em nenhum processo de implantação automatizado ou de etapa única.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Usando VSDBCMD para atualizar um esquema de banco de dados de associação

Se você modificar a estrutura do banco de dados de associação após sua primeira implantação, talvez não queira usar a Implantação da Web ferramenta de implantação da Web do Serviços de Informações da Internet (IIS) para reimplantar o banco de dados. A funcionalidade de implantação de banco de dados no Implantação da Web não inclui a capacidade de fazer atualizações diferenciais em um banco de dados&#x2014;de destino, implantação da Web deve descartar e recriar o banco de dados. Isso significa que você perde qualquer dado de conta de usuário existente, que normalmente é indesejável em ambientes de preparo ou de produção.

A alternativa é usar o utilitário VSDBCMD para atualizar o esquema do banco de dados de destino. O VSDBCMD inclui dois recursos importantes. Primeiro, ele pode importar o esquema de um banco de dados existente para um arquivo. dbschema. Em segundo lugar, ele pode implantar um arquivo. dbschema em um banco de dados existente como uma atualização diferencial, o que significa que ele apenas faz as alterações necessárias para atualizar o banco de dados de destino e você não perde nenhum dado.

Você pode usar essas etapas de alto nível para atualizar um esquema de banco de dados de associação:

1. Use a ação de **importação** VSDBCMD para gerar um arquivo. dbschema para seu banco de dados de associação de origem. Esse procedimento é descrito em [como importar um esquema de um prompt de comando](https://msdn.microsoft.com/library/dd172135.aspx).
2. Use a ação de **implantação** VSDBCMD para implantar o arquivo. dbschema no banco de dados de associação de destino. Esse procedimento é descrito em [referência de linha de comando para VSDBCMD. EXE (implantação e importação de esquema)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Conclusão

Este tópico descreveu alguns dos desafios que você pode enfrentar quando precisa provisionar bancos de dados de associação do ASP.NET em vários ambientes de destino. Em particular, explicamos por que as implantações somente de esquema deixarão o banco de dados de associação em um estado não operacional e por que não há suporte para a implantação de bancos de dado de conta de usuário. O tópico também apresentava diretrizes sobre como provisionar, implantar e atualizar bancos de dados de associação em diferentes cenários.

## <a name="further-reading"></a>Leitura adicional

Para obter mais orientações e exemplos de como usar o VSDBCMD, consulte [referência de linha de comando para VSDBCMD. EXE (implantação e importação de esquema)](https://msdn.microsoft.com/library/dd193283.aspx) e [como importar um esquema de um prompt de comando](https://msdn.microsoft.com/library/dd172135.aspx). Para obter mais informações sobre como usar o ASPNET\_RegSql. exe para criar bancos de dados de associação, consulte [ferramenta de registro do ASP.NET SQL Server (aspnet\_RegSql. exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Para obter diretrizes mais gerais sobre a implantação de bancos de dados de associação, consulte [como implantar o banco de dados de associação do ASP.NET sem incluir contas de usuário](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Anterior](deploying-database-role-memberships-to-test-environments.md)
> [Próximo](excluding-files-and-folders-from-deployment.md)
