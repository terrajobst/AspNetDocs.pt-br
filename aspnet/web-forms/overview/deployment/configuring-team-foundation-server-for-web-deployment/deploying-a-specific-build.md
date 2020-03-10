---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Implantando uma compilação específica | Microsoft Docs
author: jrjlee
description: Este tópico descreve como implantar pacotes da Web e scripts de banco de dados de uma compilação anterior específica para um novo destino, como um Enviro de preparo ou de produção...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 6bede6b36c24ade928ab052e14daec1e017bd0b2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639667"
---
# <a name="deploying-a-specific-build"></a>Implantação de um build específico

por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como implantar pacotes da Web e scripts de banco de dados de uma compilação anterior específica para um novo destino, como um ambiente de preparo ou de produção.

Este tópico faz parte de uma série de tutoriais com base em relação aos requisitos de implantação empresarial de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução&#x2014;de exemplo da&#x2014; [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)para representar um aplicativo Web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, um serviço Windows Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no coração desses tutoriais é baseado na abordagem do arquivo de projeto dividido descrito em [noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), no qual o processo de compilação e implantação é controlado&#x2014;por dois arquivos de projeto, um contendo instruções de Build que se aplicam a todos os ambientes de destino e um contendo configurações específicas de ambiente e de implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto independente do ambiente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Até agora, os tópicos neste conjunto de tutoriais se concentraram em como criar, empacotar e implantar aplicativos Web e bancos de dados como parte de um processo automatizado ou de uma única etapa. No entanto, em alguns cenários comuns, você desejará selecionar os recursos implantados de uma lista de compilações em uma pasta de destino. Em outras palavras, a compilação mais recente pode não ser a compilação que você deseja implantar.

Considere o cenário de CI (integração contínua) descrito no tópico anterior, [criando uma definição de compilação que dá suporte à implantação](creating-a-build-definition-that-supports-deployment.md). Você criou uma definição de compilação no Team Foundation Server (TFS) 2010. Sempre que um desenvolvedor verifica o código no TFS, o Team Build criará seu código, criará pacotes da Web e scripts de banco de dados como parte do processo de compilação, executará qualquer teste de unidade e implantará seus recursos em um ambiente de teste. Dependendo da política de retenção configurada quando você criou a definição de compilação, o TFS manterá um determinado número de compilações anteriores.

![](deploying-a-specific-build/_static/image1.png)

Agora, suponha que você tenha realizado testes de verificação e validação em um desses Builds em seu ambiente de teste e esteja pronto para implantar seu aplicativo em um ambiente de preparo. Enquanto isso, os desenvolvedores podem ter feito o check-in de um novo código. Você não deseja recompilar a solução e implantá-la no ambiente de preparo e não deseja implantar a compilação mais recente no ambiente de preparo. Em vez disso, você deseja implantar a compilação específica que você verificou e validou nos servidores de teste.

Para fazer isso, você precisa informar ao Microsoft Build Engine (MSBuild) onde encontrar os pacotes da Web e os scripts de banco de dados que uma compilação específica gerou.

## <a name="overriding-the-outputroot-property"></a>Substituindo a propriedade OutputRoot

Na [solução de exemplo](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), o arquivo *Publish. proj* declara uma propriedade chamada **OutputRoot**. Como o nome sugere, essa é a pasta raiz que contém tudo o que o processo de compilação gera. No arquivo *Publish. proj* , você pode ver que a propriedade **OutputRoot** refere-se ao local raiz de todos os recursos de implantação.

> [!NOTE]
> **OutputRoot** é um nome de propriedade comumente usado. Arquivos C# de projeto Visual e Visual Basic também declaram essa propriedade para armazenar o local raiz de todas as saídas de compilação.

[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]

Se você quiser que o arquivo de projeto implante pacotes da Web e scripts de banco de&#x2014;dados de um local diferente, como as&#x2014;saídas de uma compilação anterior do TFS, basta substituir a propriedade **OutputRoot** . Você deve definir o valor da propriedade para a pasta de compilação relevante no servidor do Team Build. Se você estiver executando o MSBuild na linha de comando, poderá especificar um valor para **OutputRoot** como um argumento de linha de comando:

[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]

Na prática, no entanto, você também desejaria ignorar o&#x2014;destino da compilação não há nenhum ponto em criar sua solução se você não planeja usar as saídas da compilação. Você pode fazer isso especificando os destinos que deseja executar na linha de comando:

[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]

No entanto, na maioria dos casos, você desejará criar sua lógica de implantação em uma definição de compilação do TFS. Isso permite que os usuários com a **fila criem** permissão para disparar a implantação de qualquer instalação do Visual Studio com uma conexão com o servidor TFS.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Criando uma definição de compilação para implantar compilações específicas

O procedimento a seguir descreve como criar uma definição de compilação que permite aos usuários disparar implantações em um ambiente de preparo com um único comando.

Nesse caso, você não quer que a definição de compilação realmente crie nada&#x2014;que você queira que ele execute a lógica de implantação em seu arquivo de projeto personalizado. O arquivo *Publish. proj* inclui a lógica condicional que ignora o destino da **compilação** se o arquivo estiver em execução no Team Build. Ele faz isso avaliando a propriedade **BuildingInTeamBuild** interna, que será definida automaticamente como **true** se você executar o arquivo de projeto no Team Build. Como resultado, você pode ignorar o processo de compilação e simplesmente executar o arquivo de projeto para implantar uma compilação existente.

**Para criar uma definição de compilação para disparar a implantação manualmente**

1. No Visual Studio 2010, na janela **Team Explorer** , expanda o nó do projeto de equipe, clique com o botão direito do mouse em **Builds**e clique em **nova definição de compilação**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Na guia **geral** , dê um nome à definição de compilação (por exemplo, **DeployToStaging**) e uma descrição opcional.
3. Na guia **gatilho** , selecione **manual – os check-ins não disparam uma nova compilação**.
4. Na guia **padrões de compilação** , na caixa **copiar saída da compilação para a seguinte pasta de destino** , digite o caminho UNC (Convenção de nomenclatura universal) de sua pasta de destino (por exemplo, **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. Na guia **processo** , na lista suspensa de **arquivos do processo de compilação** , deixe **DefaultTemplate. XAML** selecionado. Este é um dos modelos de processo de compilação padrão que são adicionados a todos os novos projetos de equipe.
6. Na tabela **parâmetros do processo de compilação** , clique na linha **itens a serem criados** e, em seguida, clique no botão de **reticências** .

    ![](deploying-a-specific-build/_static/image4.png)
7. Na caixa de diálogo **itens a serem criados** , clique em **Adicionar**.
8. Na lista suspensa **itens do tipo** , selecione **arquivos de projeto do MSBuild**.
9. Navegue até o local do arquivo de projeto personalizado com o qual você controla o processo de implantação, selecione o arquivo e clique em **OK**.

    ![](deploying-a-specific-build/_static/image5.png)
10. Na caixa de diálogo **itens a serem criados** , clique em **OK**.
11. Na tabela **parâmetros do processo de compilação** , expanda a seção **avançado** .
12. Na linha de **argumentos do MSBuild** , especifique o local do arquivo de projeto específico do ambiente e adicione um espaço reservado para o local da sua pasta de compilação:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > Você precisará substituir o valor de **OutputRoot** toda vez que colocar uma compilação na fila. Isso é abordado no próximo procedimento.
13. Clique em **Save** (Salvar).

Ao disparar uma compilação, você precisa atualizar a propriedade **OutputRoot** para apontar para a compilação que deseja implantar.

**Para implantar uma compilação específica de uma definição de compilação**

1. Na janela **Team Explorer** , clique com o botão direito do mouse na definição da compilação e clique em **enfileirar nova compilação**.

    ![](deploying-a-specific-build/_static/image7.png)
2. Na caixa de diálogo **compilação da fila** , na guia **parâmetros** , expanda a seção **avançado** .
3. Na linha de **argumentos do MSBuild** , substitua o valor da propriedade **OutputRoot** pelo local da sua pasta de Build. Por exemplo:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Certifique-se de incluir uma barra à direita no final do caminho para a pasta de Build.
4. Clique em **fila**.

Ao colocar a compilação em fila, o arquivo de projeto implantará os scripts de banco de dados e os pacotes da Web da pasta de destino de compilação especificada na propriedade **OutputRoot** .

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como publicar recursos de implantação, como os pacotes da Web e scripts de banco de dados, de uma compilação anterior específica usando o modelo de implantação de arquivo de projeto dividido. Ele explicou como substituir a propriedade **OutputRoot** e como incorporar a lógica de implantação em uma definição de compilação do TFS.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como criar definições de compilação, consulte [criar uma definição de compilação básica](https://msdn.microsoft.com/library/ms181716.aspx) e [definir o processo de compilação](https://msdn.microsoft.com/library/ms181715.aspx). Para obter mais diretrizes sobre as compilações de enfileiramento, consulte enfileirar [uma compilação](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Anterior](creating-a-build-definition-that-supports-deployment.md)
> [Próximo](configuring-permissions-for-team-build-deployment.md)
