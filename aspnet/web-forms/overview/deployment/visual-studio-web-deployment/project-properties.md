---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'Implantação da Web do ASP.NET usando o Visual Studio: propriedades do projeto | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros, por usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: b2811791a897c9166f6222c23dddc6921e5267ab
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74614948"
---
# <a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Implantação da Web do ASP.NET usando o Visual Studio: propriedades do projeto

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros usando o Visual Studio 2012 ou o Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial da série](introduction.md).

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

Algumas opções de implantação são configuradas nas propriedades do projeto que são armazenadas no arquivo de projeto (o arquivo *. csproj* ou *. vbproj* ). Na maioria dos casos, os valores padrão dessas configurações são o que você deseja, mas você pode usar a interface do usuário de **Propriedades do projeto** interna no Visual Studio para trabalhar com essas configurações se precisar alterá-las. Neste tutorial, você revisará as configurações de implantação em **Propriedades do projeto**. Você também cria um arquivo de espaço reservado que faz com que uma pasta vazia seja implantada.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Definir configurações de implantação na janela Propriedades do projeto

A maioria das configurações que afetam o que acontece durante a implantação são incluídas no perfil de publicação, como você verá nos tutoriais a seguir. Algumas configurações das quais você deve estar atento estão localizadas nas guias **empacotar/publicar** da janela **Propriedades do projeto** . Essas configurações são especificadas para cada configuração de compilação — ou seja, você pode ter configurações diferentes para uma compilação de versão do que você tem para uma compilação de depuração.

Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto **ContosoUniversity** , selecione **Propriedades**e, em seguida, selecione a guia **empacotar/publicar Web** .

![Guia empacotar/publicar Web](project-properties/_static/image1.png)

Quando a janela é exibida, o padrão é mostrar as configurações para qualquer configuração de compilação atualmente ativa para a solução. Se a caixa de **configuração** não indicar **ativo (versão)** , selecione **liberar** para exibir as configurações para a configuração de Build de versão. Você implantará compilações de versão em seus ambientes de teste e produção.

![Selecionando a configuração da compilação de versão](project-properties/_static/image2.png)

Com **Active (versão)** ou **versão** selecionada, você vê os valores que são efetivos ao implantar usando a configuração de Build de versão:

- Na caixa **itens a serem implantados** , **somente os arquivos necessários para executar o aplicativo** são selecionados. Outras opções são **todos os arquivos neste projeto** ou **todos os arquivos nesta pasta do projeto**. Deixando a seleção padrão inalterada, você evita a implantação de arquivos de código-fonte, por exemplo. Essa configuração é o motivo pelo qual as pastas que contêm os arquivos binários SQL Server Compact tinham que ser incluídas no projeto. Para obter mais informações sobre essa configuração, consulte **por que nem todos os arquivos na pasta do meu projeto são implantados?** em [perguntas frequentes sobre implantação do projeto de aplicativo Web ASP.net](https://msdn.microsoft.com/library/ee942158.aspx).
- **Excluir símbolos de depuração gerados** está selecionado. Você não estará Depurando quando usar essa configuração de compilação.
- **Incluir todos os bancos de dados configurados na guia pacote/publicar SQL** está selecionado. Especifica se o Visual Studio implantará bancos de dados, bem como arquivos. Embora o rótulo da caixa de seleção mencione apenas a guia **pacote/publicar SQL** , desmarcar essa caixa de seleção também desabilitará a implantação do banco de dados configurada no perfil de publicação. Você fará isso mais tarde, portanto, a caixa de seleção deve permanecer selecionada. A guia **pacote/publicar SQL** é usada para um método de publicação de banco de dados herdado que você não usará nesses tutoriais.
- A seção **configurações do pacote de implantação da Web** não se aplica porque você está usando a publicação com um clique nesses tutoriais.

Altere a caixa suspensa **configuração** para depurar a fim de ver as configurações padrão para compilações de depuração. Os valores são os mesmos, exceto que **excluir símbolos de depuração gerados** é limpo para que você possa depurar ao implantar uma compilação de depuração.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Certifique-se de que a pasta do ELMAH seja implantada

Como vimos no tutorial anterior, o [pacote NuGet do ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) fornece funcionalidade para registro em log de erros e relatórios. No aplicativo da Contoso University, o ELMAH foi configurado para armazenar detalhes do erro em uma pasta chamada *ELMAH*:

![Pasta do ELMAH](project-properties/_static/image3.png)

Excluir arquivos ou pastas específicas da implantação é um requisito comum; outro exemplo seria uma pasta na qual os usuários podem carregar arquivos. Você não quer arquivos de log ou arquivos carregados que foram criados em seu ambiente de desenvolvimento para serem implantados na produção. E se estiver implantando uma atualização para produção, você não deseja que o processo de implantação exclua arquivos que existem na produção. (Dependendo de como você define uma opção de implantação, se um arquivo existir no site de destino, mas não no site de origem quando você implantar o, Implantação da Web o excluirá do destino.)

Como vimos anteriormente neste tutorial, a opção **itens a serem implantados** na guia **empacotar/publicar Web** é definida **somente para os arquivos necessários para executar este aplicativo**. Como resultado, os arquivos de log criados pelo ELMAH no desenvolvimento não serão implantados, o que é o que você deseja que aconteça. (Para ser implantado, eles teriam que ser incluídos no projeto e sua propriedade de **ação de compilação** teria que ser definida como **Content**. Para obter mais informações, consulte **por que nem todos os arquivos na pasta do meu projeto são implantados?** em [perguntas frequentes sobre implantação do projeto de aplicativo Web ASP.net](https://msdn.microsoft.com/library/ee942158.aspx)). No entanto, Implantação da Web não criará uma pasta no site de destino, a menos que haja pelo menos um arquivo para copiar para ele. Portanto, você adicionará um arquivo *. txt* à pasta para atuar como um espaço reservado para que a pasta seja copiada.

Em **Gerenciador de soluções**, clique com o botão direito do mouse na pasta *ELMAH* , selecione **Adicionar novo item**e crie um arquivo de texto chamado *PlaceHolder. txt*. Coloque o seguinte texto: "Este é um arquivo de espaço reservado para garantir que a pasta seja implantada". e salve o arquivo. Isso é tudo o que você precisa fazer para ter certeza de que o Visual Studio implanta esse arquivo e a pasta em que ele está, porque a propriedade de **ação de compilação** de arquivos *. txt* está definida como **conteúdo** por padrão.

## <a name="summary"></a>Resumo

Agora você concluiu todas as tarefas de configuração de implantação. No próximo tutorial, você implantará o site da Contoso University no ambiente de teste e o testará.

> [!div class="step-by-step"]
> [Anterior](web-config-transformations.md)
> [Próximo](deploying-to-iis.md)
